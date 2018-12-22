# <a name="lexical-grammar"></a>Lexikalische Grammatik

Kompilierung von Visual Basic-Programms muss zunächst den unformatierten Stream von Unicode-Zeichen in einer geordneten Menge von Lexikalischer Token zu übersetzen. Da Visual Basic-Sprache kein bestimmtes Format ist, wird der Satz von Token dann weiter in eine Reihe von logische Zeilen dividiert. Ein *logischen Zeile* Spannen aus entweder den Anfang des Streams oder ein Zeilenabschlusszeichen bis zum nächsten Zeilenabschlusszeichen, die nicht bis zum Ende des Streams Zeilenfortsetzungszeichen oder über vorangestellt ist.

__Beachten Sie.__ Mit der Einführung von XML-Literale Ausdrücke in der Version 9.0 der Sprache verfügt Visual Basic nicht mehr eine unterschiedliche lexikalische Grammatik in dem Sinne, dass Visual Basic-Code ohne Berücksichtigung des syntaktischen Kontexts mit einem Token versehen werden kann. Dies ist darauf zurückzuführen, XML und Visual Basic verschiedene Regeln für die lexikalische besitzen und der Satz von lexikalischen Regeln zu einem bestimmten Zeitpunkt hängt von welche syntaktische Konstrukt zu diesem Zeitpunkt verarbeitet wird. In dieser Spezifikation werden in diesem Abschnitt lexikalische Grammatik als Leitfaden für den lexikalischen Regeln der regulären Visual Basic-Code beibehalten.

```antlr
LogicalLineStart
    : LogicalLine*
    ;

LogicalLine
    : LogicalLineElement* Comment? LineTerminator
    ;

LogicalLineElement
    : WhiteSpace
    | LineContinuation
    | Token
    ;

Token
    : Identifier
    | Keyword
    | Literal
    | Separator
    | Operator
    ;
```

## <a name="characters-and-lines"></a>Zeichen und Zeilen

Visual Basic-Programme bestehen aus Zeichen von Unicode-Zeichensatz.

```antlr
Character:
    '<Any Unicode character except a LineTerminator>'
    ;
```

### <a name="line-terminators"></a>Zeilenabschlusszeichen

Unicode-Zeilenumbruchzeichen trennen logische Zeilen.

```antlr
LineTerminator
    : '<Unicode 0x00D>'
    | '<Unicode 0x00A>'
    | '<CR>'
    | '<LF>'
    | '<Unicode 0x2028>'
    | '<Unicode 0x2029>'
    ;
```

### <a name="line-continuation"></a>Zeilenfortsetzung

Ein *Zeile Fortsetzung* besteht aus mindestens ein Leerzeichen, das sofort einen einzelner Unterstrich als letztes Zeichen in einer Textzeile (außer Leerzeichen) vorangestellt ist. Eine zeilenfortsetzung ermöglicht es eine logische Zeile über mehrere physische Zeilen erstrecken. Zeilenfortsetzungen werden behandelt, als wären sie Leerzeichen, auch wenn dies nicht der Fall.

```antlr
LineContinuation
    : WhiteSpace '_' WhiteSpace* LineTerminator
    ;
```

Das folgende Programm zeigt einige zeilenfortsetzungen:

```vb
Module Test
    Sub Print( _
        Param1 As Integer, _
        Param2 As Integer )

        If (Param1 < Param2) Or _
            (Param1 > Param2) Then
            Console.WriteLine("Not equal")
        End If
    End Function
End Module
```

Einige stellen in der syntaktischen Grammatik ermöglichen *impliziten zeilenfortsetzungen*. Wenn ein Zeilenabschlusszeichen festgestellt wird

* nach einem Komma (`,`), offene Klammer (`(`), offene geschweifte Klammer (`{`), oder öffnen Sie die eingebetteten Ausdruck (`<%=`)

* nach der ein Memberqualifizierer (`.` oder `.@` oder `...`), vorausgesetzt, dass etwas qualifiziert wird, wird (d. h. nicht verwendet einen impliziten `With` Kontext)

* vor einer schließenden Klammer (`)`), schließen Sie die geschweifte Klammer (`}`), oder Schließen eines eingebetteten Ausdrucks (`%>`)

* nach einer kleiner-als (`<`) in einem Attributkontext

* Bevor Sie ein größer-als (`>`) in einem Attributkontext

* nach einem größer-als (`>`) in einem Attributkontext auf nicht-Datei-

* vor und nach Abfrageoperatoren (`Where`, `Order`, `Select`usw..)

* nach binären Operatoren (`+`, `-`, `/`, `*`usw.) in einem Ausdruckskontext

* nach dem Zuweisungsoperatoren (`=`, `:=`, `+=`, `-=`usw.) in jedem Kontext.

die für den Zeilenabschluss wird behandelt, als ob es sich um eine zeilenfortsetzung war.

```antlr
Comma
    : ',' LineTerminator?
    ;

Period
    : '.' LineTerminator?
    ;

OpenParenthesis
    : '(' LineTerminator?
    ;

CloseParenthesis
    : LineTerminator? ')'
    ;

OpenCurlyBrace
    : '{' LineTerminator?
    ;

CloseCurlyBrace
    : LineTerminator? '}'
    ;

Equals
    : '=' LineTerminator?
    ;

ColonEquals
    : ':' '=' LineTerminator?
    ;
```

Im vorherige Beispiel könnte beispielsweise auch folgendermaßen geschrieben werden:

```vb
Module Test
    Sub Print(
        Param1 As Integer,
        Param2 As Integer)

        If (Param1 < Param2) Or
            (Param1 > Param2) Then
            Console.WriteLine("Not equal")
        End If
    End Function
End Module
```

Impliziten zeilenfortsetzungen werden immer nur direkt vor oder nach dem angegebenen Token abgeleitet werden. Sie werden nicht vor oder nach einem zeilenfortsetzung abgeleitet werden. Zum Beispiel:

```vb
Dim y = 10
' Error: Expression expected for assignment to x
Dim x = _

y
```

Zeilenfortsetzungen werden in Kontexten für bedingte Kompilierung nicht abgeleitet werden. (__Beachten.__ Diese letzte Einschränkung ist erforderlich, da der Text in Blöcken für die bedingte Kompilierung, die nicht kompiliert werden keine syntaktisch gültig ist. Daher Text im Block möglicherweise versehentlich "durch die Anweisung für die bedingte Kompilierung übernommen abrufen" insbesondere dann, wenn die Sprache in der Zukunft erweitert ruft.)


### <a name="white-space"></a>Leerraum

*Leerraum* dient nur zum Trennen von Tokens und wird andernfalls ignoriert. Logische Zeilen, die nur Leerzeichen enthält, werden ignoriert. (__Beachten.__
Zeilenabschlusszeichen werden nicht mit Leerzeichen berücksichtigt.)

```antlr
WhiteSpace
    : '<Unicode class Zs>'
    | '<Unicode Tab 0x0009>'
    ;
```

### <a name="comments"></a>Kommentare

Ein *Kommentar* beginnt mit einem einzelnen Anführungszeichen oder das Schlüsselwort `REM`. Ein Apostroph-Zeichen wird entweder ein ASCII-Apostroph-Zeichen, eine Unicode-Zeichen nach links einfache Anführungszeichen oder ein Recht Unicode-Zeichen einfache Anführungszeichen. Kommentare können an einer beliebigen Stelle auf einer Quellzeile beginnen, und das Ende der physischen Zeile der Kommentar endet. Der Compiler ignoriert die Zeichen zwischen dem Anfang des Kommentars und das Zeilenabschlusszeichen an. Daher können keine Kommentare über mehrere Zeilen mithilfe von zeilenfortsetzungen erweitern.

```antlr
Comment
    : CommentMarker Character*
    ;

CommentMarker
    : SingleQuoteCharacter
    | 'REM'
    ;

SingleQuoteCharacter
    : '\''
    | '<Unicode 0x2018>'
    | '<Unicode 0x2019>'
    ;
```

## <a name="identifiers"></a>Bezeichner

Ein *Bezeichner* ist ein Name. Visual Basic-Bezeichner entsprechen, die Unicode-Standard-Anhang-15 mit einer Ausnahme: Bezeichner können mit einem Unterstrich (Connector) beginnen. Wenn ein Bezeichner mit einem Unterstrich beginnt, darf es mindestens ein gültiges Bezeichnerzeichen um sie von einer zeilenfortsetzung eindeutig zu machen.

```antlr
Identifier
    : NonEscapedIdentifier TypeCharacter?
    | Keyword TypeCharacter
    | EscapedIdentifier
    ;

NonEscapedIdentifier
    : '<Any IdentifierName but not Keyword>'
    ;

EscapedIdentifier
    : '[' IdentifierName ']'
    ;

IdentifierName
    : IdentifierStart IdentifierCharacter*
    ;

IdentifierStart
    : AlphaCharacter
    | UnderscoreCharacter IdentifierCharacter
    ;

IdentifierCharacter
    : UnderscoreCharacter
    | AlphaCharacter
    | NumericCharacter
    | CombiningCharacter
    | FormattingCharacter
    ;

AlphaCharacter
    : '<Unicode classes Lu,Ll,Lt,Lm,Lo,Nl>'
    ;

NumericCharacter
    : '<Unicode decimal digit class Nd>'
    ;

CombiningCharacter
    : '<Unicode combining character classes Mn, Mc>'
    ;

FormattingCharacter
    : '<Unicode formatting character class Cf>'
    ;

UnderscoreCharacter
    : '<Unicode connection character class Pc>'
    ;

IdentifierOrKeyword
    : Identifier
    | Keyword
    ;
```

Reguläre Bezeichner können nicht mit Schlüsselwörtern übereinstimmen, jedoch können mit Escapezeichen versehene Bezeichner oder Bezeichner mit einem Typzeichen. Ein *mit Escapezeichen versehen Bezeichner* ist ein Bezeichner durch eckige Klammern getrennt. Mit Escapezeichen versehenen Bezeichner führen Sie dieselben Regeln wie für reguläre Bezeichner mit dem Unterschied, dass sie möglicherweise mit Schlüsselwörtern übereinstimmen, und dürfen keine Typzeichen.

In diesem Beispiel definiert eine Klasse namens `class` mit einer freigegebenen Methode namens `shared` , akzeptiert einen Parameter namens `boolean` und ruft dann die Methode.

```vb
Class [class]
    Shared Sub [shared]([boolean] As Boolean)
        If [boolean] Then
            Console.WriteLine("true")
        Else
            Console.WriteLine("false")
        End If
    End Sub
End Class

Module [module]
    Sub Main()
        [class].[shared](True)
    End Sub
End Module
```

Bezeichner sind Groß-/Kleinschreibung, also den gleichen Bezeichner sein, wenn sie nur im Fall unterscheiden sich zwei Bezeichner gelten. (__Beachten.__ Die Unicode-Standard 1: 1-schreibungszuordnungen werden verwendet, wenn es sich bei Vergleichen von Bezeichnern und Groß-/Kleinschreibung Gebietsschema-spezifische Zuordnungen werden ignoriert.)


### <a name="type-characters"></a>Typzeichen

Ein *Typzeichen* bezeichnet den Typ des vorherigen Bezeichners. Das Typzeichen gilt nicht als Teil des Bezeichners.

```antlr
TypeCharacter
    : IntegerTypeCharacter
    | LongTypeCharacter
    | DecimalTypeCharacter
    | SingleTypeCharacter
    | DoubleTypeCharacter
    | StringTypeCharacter
    ;

IntegerTypeCharacter
    : '%'
    ;

LongTypeCharacter
    : '&'
    ;

DecimalTypeCharacter
    : '@'
    ;

SingleTypeCharacter
    : '!'
    ;

DoubleTypeCharacter
    : '#'
    ;

StringTypeCharacter
    : '$'
    ;
```

Wenn eine Deklaration ein Typzeichen enthält, muss das Typzeichen mit den in die eigentliche Deklaration angegebenen Typ übereinstimmen; andernfalls tritt ein Kompilierungsfehler auf. Wenn die Deklaration den Typ lässt (z. B., wenn er nicht angegeben ist ein `As` Klausel), das Typzeichen ist implizit als den Typ der Deklaration ersetzt.

Keine Leerzeichen können zwischen einem Bezeichner und die Typzeichen stammen. Es gibt keine Typzeichen für `Byte`, `SByte`, `UShort`, `Short`, `UInteger` oder `ULong`, da ein Mangel an geeigneten Zeichen.

Ein Typzeichen anfügen, ein Bezeichner, der vom Konzept her nicht über einen Typ (z. B. ein Namespacename) verfügt oder ein Bezeichner, dessen Typ stimmt nicht mit dem Typ des Typzeichen, bewirkt, dass einen Fehler während der Kompilierung.

Das folgende Beispiel zeigt die Verwendung von Typzeichen:

```vb
' The follow line will cause an error: standard modules have no type.
Module Test1#
End Module

Module Test2

    ' This function takes a Long parameter and returns a String.
    Function Func$(Param&)

        ' The following line causes an error because the type character
        ' conflicts with the declared type of Func and Param.
        Func# = CStr(Param@)

        ' The following line is valid.
        Func$ = CStr(Param&)
    End Function
End Module
```

Das Typzeichen `!` als spezielle problematisch, da es sowohl als ein Typzeichen als ein Trennzeichen in der Sprache verwendet werden kann. Um Mehrdeutigkeit, eine `!` Zeichen ist ein Typzeichen, solange die darauf folgende Zeichen ein Bezeichners nicht gestartet werden kann. Wenn dies möglich ist, und klicken Sie dann die `!` Zeichen ein Trennzeichen ist, nicht über ein Typzeichen.


## <a name="keywords"></a>Schlüsselwörter

Ein *Schlüsselwort* ist ein Wort, das in einem Sprachkonstrukt besondere Bedeutung hat. Alle Schlüsselwörter sind von der Sprache reserviert und können nicht als Bezeichner verwendet werden, es sei denn, die Bezeichner mit Escapezeichen versehen werden. (__Beachten.__ `EndIf``GoSub`, `Let`, `Variant`, und `Wend` werden als Schlüsselwörter, beibehalten, obwohl sie nicht mehr in Visual Basic verwendet werden.)

```antlr
Keyword
    : 'AddHandler'      | 'AddressOf'      | 'Alias'       | 'And'
    | 'AndAlso'         | 'As'             | 'Boolean'     | 'ByRef'
    | 'Byte'            | 'ByVal'          | 'Call'        | 'Case'        
    | 'Catch'           | 'CBool'          | 'CByte'       | 'CChar'       
    | 'CDate'           | 'CDbl'           | 'CDec'        | 'Char'        
    | 'CInt'            | 'Class'          | 'CLng'        | 'CObj'        
    | 'Const'           | 'Continue'       | 'CSByte'      | 'CShort'      
    | 'CSng'            | 'CStr'           | 'CType'       | 'CUInt'       
    | 'CULng'           | 'CUShort'        | 'Date'        | 'Decimal'     
    | 'Declare'         | 'Default'        | 'Delegate'    | 'Dim'         
    | 'DirectCast'      | 'Do'             | 'Double'      | 'Each'        
    | 'Else'            | 'ElseIf'         | 'End'         | 'EndIf'       
    | 'Enum'            | 'Erase'          | 'Error'       | 'Event'       
    | 'Exit'            | 'False'          | 'Finally'     | 'For'         
    | 'Friend'          | 'Function'       | 'Get'         | 'GetType'     
    | 'GetXmlNamespace' | 'Global'         | 'GoSub'       | 'GoTo'        
    | 'Handles'         | 'If'             | 'Implements'  | 'Imports'     
    | 'In'              | 'Inherits'       | 'Integer'     | 'Interface'   
    | 'Is'              | 'IsNot'          | 'Let'         | 'Lib'         
    | 'Like'            | 'Long'           | 'Loop'        | 'Me'          
    | 'Mod'             | 'Module'         | 'MustInherit' | 'MustOverride'
    | 'MyBase'          | 'MyClass'        | 'Namespace'   | 'Narrowing'   
    | 'New'             | 'Next'           | 'Not'         | 'Nothing'     
    | 'NotInheritable'  | 'NotOverridable' | 'Object'      | 'Of'          
    | 'On'              | 'Operator'       | 'Option'      | 'Optional'    
    | 'Or'              | 'OrElse'         | 'Overloads'   | 'Overridable' 
    | 'Overrides'       | 'ParamArray'     | 'Partial'     | 'Private'     
    | 'Property'        | 'Protected'      | 'Public'      | 'RaiseEvent'  
    | 'ReadOnly'        | 'ReDim'          | 'REM'         | 'RemoveHandler'
    | 'Resume'          | 'Return'         | 'SByte'       | 'Select'      
    | 'Set'             | 'Shadows'        | 'Shared'      | 'Short'       
    | 'Single'          | 'Static'         | 'Step'        | 'Stop'        
    | 'String'          | 'Structure'      | 'Sub'         | 'SyncLock'    
    | 'Then'            | 'Throw'          | 'To'          | 'True'        
    | 'Try'             | 'TryCast'        | 'TypeOf'      | 'UInteger'    
    | 'ULong'           | 'UShort'         | 'Using'       | 'Variant'     
    | 'Wend'            | 'When'           | 'While'       | 'Widening'    
    | 'With'            | 'WithEvents'     | 'WriteOnly'   | 'Xor'         
    ;
```

## <a name="literals"></a>Literale

Ein *literal* ist eine Textdarstellung eines bestimmten Werts eines Typs. Zeichenfolgenliterale-Typen enthalten, boolescher Wert, ganze Zahl, Gleitkomma, Zeichenfolge, Zeichen und Datum.

```antlr
Literal
    : BooleanLiteral
    | IntegerLiteral
    | FloatingPointLiteral
    | StringLiteral
    | CharacterLiteral
    | DateLiteral
    | Nothing
    ;
```

### <a name="boolean-literals"></a>Boolesche Literale

`True` und `False` sind Literale, die von der `Boolean` Typ, der bzw. in den Zustand "true" und "false", zuordnen.

```antlr
BooleanLiteral
    : 'True' | 'False'
    ;
```

### <a name="integer-literals"></a>Integer-Literale

Ganzzahlliterale können dezimale (Basis 10), oktale (Basis 8) oder Hexadezimalformat (Basis 16) sein. Ein Ganzzahlliteral im Dezimalformat ist eine Zeichenfolge aus Dezimalzahlen (0-9). Ist ein hexadezimales Literal `&H` gefolgt von einer Zeichenfolge von hexadezimalen Ziffern (0-9, A-F). Ist ein oktales Literal `&O` gefolgt von einer Zeichenfolge von oktalen Ziffern (0-7). Dezimale Literale direkt den Dezimalwert der ganzzahliges Literal darstellen, während oktale und hexadezimale Literale den Binärwert des Integer-literal darstellen (also `&H8000S` -32768, kein Überlauffehler).

```antlr
IntegerLiteral
    : IntegralLiteralValue IntegralTypeCharacter?
    ;

IntegralLiteralValue
    : IntLiteral
    | HexLiteral
    | OctalLiteral
    ;

IntegralTypeCharacter
    : ShortCharacter
    | UnsignedShortCharacter
    | IntegerCharacter
    | UnsignedIntegerCharacter
    | LongCharacter
    | UnsignedLongCharacter
    | IntegerTypeCharacter
    | LongTypeCharacter
    ;

ShortCharacter
    : 'S'
    ;

UnsignedShortCharacter
    : 'US'
    ;

IntegerCharacter
    : 'I'
    ;

UnsignedIntegerCharacter
    : 'UI'
    ;

LongCharacter
    : 'L'
    ;

UnsignedLongCharacter
    : 'UL'
    ;

IntLiteral
    : Digit+
    ;

HexLiteral
    : '&' 'H' HexDigit+
    ;

OctalLiteral
    : '&' 'O' OctalDigit+
    ;

Digit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    ;

HexDigit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    | 'A' | 'B' | 'C' | 'D' | 'E' | 'F'
    ;

OctalDigit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7'
    ;
```

Der Typ eines Literals wird durch seinen Wert oder durch das folgende Typzeichen bestimmt. Wenn kein Typzeichen angegeben ist, Werte, die im Bereich der `Integer` Typ typisiert sind, als `Integer`; Werte außerhalb des Bereichs für `Integer` typisiert sind, als `Long`. Wenn ein Integer-Literal-Typ, der das Ganzzahlliteral nicht groß genug ist, führt ein Fehler während der Kompilierung. (__Beachten.__ Gibt es kein Typzeichen für `Byte` da wäre das naheliegendste Zeichen `B`, d.h. ein zulässigen Zeichen in ein hexadezimales Literal.)


### <a name="floating-point-literals"></a>Gleitkommaliterale

Einem Gleitkommaliteral ist ein Integer-literal gefolgt von einem optionalen Dezimaltrennzeichen (der Zeitraum ASCII-Zeichen) und Mantisse und eine optionale Basis 10 Exponent. Standardmäßig ist ein Gleitkomma-Literal vom Typ `Double`. Wenn die `Single`, `Double`, oder `Decimal` Typzeichen angegeben wird, ist Sie das Literal von diesem Typ. Wenn ein Typ des Gleitkommaliterals, des Gleitkommaliterals nicht groß genug ist, führt ein Fehler während der Kompilierung.

__Beachten Sie.__ Es ist erwähnenswert, die die `Decimal` -Datentyp kann nachfolgende Nullen in einen Wert zu codieren. Die Spezifikation ist derzeit kein Kommentar zu gibt an, ob nachfolgende in Nullen einer `Decimal` Literal vom Compiler berücksichtigt werden.

```antlr
FloatingPointLiteral
    : FloatingPointLiteralValue FloatingPointTypeCharacter?
    | IntLiteral FloatingPointTypeCharacter
    ;

FloatingPointTypeCharacter
    : SingleCharacter
    | DoubleCharacter
    | DecimalCharacter
    | SingleTypeCharacter
    | DoubleTypeCharacter
    | DecimalTypeCharacter
    ;

SingleCharacter
    : 'F'
    ;

DoubleCharacter
    : 'R'
    ;

DecimalCharacter
    : 'D'
    ;

FloatingPointLiteralValue
    : IntLiteral '.' IntLiteral Exponent?
    | '.' IntLiteral Exponent?
    | IntLiteral Exponent
    ;

Exponent
    : 'E' Sign? IntLiteral
    ;

Sign
    : '+'
    | '-'
    ;
```

### <a name="string-literals"></a>Zeichenfolgenliterale

Ein Zeichenfolgenliteral ist eine Sequenz von NULL oder mehr Unicode-Zeichen beginnt und endet mit einem ASCII-Anführungszeichen, eine Unicode-Zeichen nach links doppelte Anführungszeichen oder einem Unicode-Zeichen nach rechts doppelte Anführungszeichen. Innerhalb einer Zeichenfolge ist eine Sequenz von zwei doppelte Anführungszeichen eine Escapesequenz, die ein doppeltes Anführungszeichen in der Zeichenfolge darstellt.

```antlr
StringLiteral
    : DoubleQuoteCharacter StringCharacter* DoubleQuoteCharacter
    ;

DoubleQuoteCharacter
    : '"'
    | '<unicode left double-quote 0x201c>'
    | '<unicode right double-quote 0x201D>'
    ;

StringCharacter
    : '<Any character except DoubleQuoteCharacter>'
    | DoubleQuoteCharacter DoubleQuoteCharacter
    ;
```

Eine Zeichenfolgenkonstante ist von der `String` Typ.

```vb
Module Test
    Sub Main()

        ' This prints out: ".
        Console.WriteLine("""")

        ' This prints out: a"b.
        Console.WriteLine("a""b")

        ' This causes a compile error due to mismatched double-quotes.
        Console.WriteLine("a"b")
    End Sub
End Module
```

Der Compiler kann einen Konstante Zeichenfolge-Ausdruck mit einem Zeichenfolgenliteral zu ersetzen. Jeder String-literal führt nicht unbedingt in eine neue Zeichenfolgeninstanz. Wenn zwei oder mehr Zeichenfolgenliterale, die gemäß den Zeichenfolge-Equality-Operator mit binärer Vergleichssemantik äquivalent sind, im selben Programm angezeigt werden, können diese Zeichenfolgenliterale, auf die gleiche Zeichenfolgeninstanz verweisen. Beispielsweise kann die Ausgabe des folgenden Programms zurückgeben `True` da die beiden Literale auf die gleiche Zeichenfolgeninstanz verweisen können.

```vb
Module Test
    Sub Main()
        Dim a As Object = "he" & "llo"
        Dim b As Object = "hello"
        Console.WriteLine(a Is b)
    End Sub
End Module
```


### <a name="character-literals"></a>Zeichenliterale

Ein Zeichenfolgenliteral stellt ein einzelnes Unicodezeichen von der `Char` Typ. Zwei doppelte Anführungszeichen ist eine Escapesequenz, die die doppelten Anführungszeichen darstellt.

```antlr
CharacterLiteral
    : DoubleQuoteCharacter StringCharacter DoubleQuoteCharacter 'C'
    ;
```


```vb
Module Test
    Sub Main()

        ' This prints out: a.
        Console.WriteLine("a"c)

        ' This prints out: ".
        Console.WriteLine(""""c)
    End Sub
End Module
```


### <a name="date-literals"></a>Datumsliterale

Ein Datumsliteral darstellt, einen bestimmten Zeitpunkt als Wert des der `Date` Typ.

```antlr
DateLiteral
    : '#' WhiteSpace* DateOrTime WhiteSpace* '#'
    ;

DateOrTime
    : DateValue WhiteSpace+ TimeValue
    | DateValue
    | TimeValue
    ;

DateValue
    : MonthValue '/' DayValue '/' YearValue
    | MonthValue '-' DayValue '-' YearValue
    ;

TimeValue
    : HourValue ':' MinuteValue ( ':' SecondValue )? WhiteSpace* AMPM?
    | HourValue WhiteSpace* AMPM
    ;

MonthValue
    : IntLiteral
    ;

DayValue
    : IntLiteral
    ;

YearValue
    : IntLiteral
    ;

HourValue
    : IntLiteral
    ;

MinuteValue
    : IntLiteral
    ;

SecondValue
    : IntLiteral
    ;

AMPM
    : 'AM' | 'PM'
    ;    
```

Das Literal kann ein Datum und einem Zeitpunkt nur ein Datum oder nur eine Uhrzeit angeben. Wenn der Date-Wert weggelassen wird, wird am 1. Januar des Jahres 1 im gregorianischen Kalender ausgegangen. Wenn der Zeitwert weggelassen wird, wird 12:00:00 Uhr angenommen.

Zur Vermeidung von Problemen bei der Interpretation der Wert des Jahres in einen Date-Wert darf nicht den Wert des Jahres zweistellig sein. Wenn Sie ein Datum in der ersten Jahrhundert AD/CE auszudrücken, muss führenden Nullen angegeben werden.

Ein Zeitwert kann entweder über einen 24-Stunden-Wert oder einen 12-Stunden-Wert angegeben werden; Zeitwerte, die auslassen einer `AM` oder `PM` wird angenommen, dass 24-Stunden-Werte. Wenn Sie ein Time-Wert, die Minuten, das Literal lässt `0` wird standardmäßig verwendet. Wenn Sie ein Time-Wert, die Sekunden, das Literal lässt `0` wird standardmäßig verwendet. Wenn weder Minuten noch Sekunden, dann weggelassen werden `AM` oder `PM` muss angegeben werden. Ist der angegebene Datumswert außerhalb des Bereichs von der `Date` eingeben, ein Fehler während der Kompilierung auftritt.

Das folgende Beispiel enthält mehrere Datumsliterale.

```vb
Dim d As Date

d = # 8/23/1970 3:45:39AM #
d = # 8/23/1970 #              ' Date value: 8/23/1970 12:00:00AM.
d = # 3:45:39AM #              ' Date value: 1/1/1 3:45:39AM.
d = # 3:45:39 #                ' Date value: 1/1/1 3:45:39AM.
d = # 13:45:39 #               ' Date value: 1/1/1 1:45:39PM.
d = # 1AM #                    ' Date value: 1/1/1 1:00:00AM.
d = # 13:45:39PM #             ' This date value is not valid.
```


### <a name="nothing"></a>Nothing

`Nothing` ist ein spezielles Literal. Er verfügt nicht über einen Typ und wird im Typsystem, einschließlich der Typparameter für alle Typen konvertiert werden kann. Wenn auf einen bestimmten Typ konvertiert wird, entspricht es der Standardwert dieses Typs.

```antlr
Nothing
    : 'Nothing'
    ;
```

## <a name="separators"></a>Trennzeichen

Die folgenden ASCII-Zeichen sind, Trennzeichen:

```antlr
Separator
    : '(' | ')' | '{' | '}' | '!' | '#' | ',' | '.' | ':' | '?'
    ;
```

## <a name="operator-characters"></a>Operatorzeichen

Die folgenden ASCII-Zeichen oder Zeichenfolgen sind Operatoren:

```antlr
Operator
    : '&' | '*' | '+' | '-' | '/' | '\\' | '^' | '<' | '=' | '>'
    ;
```

