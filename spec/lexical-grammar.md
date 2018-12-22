# <a name="lexical-grammar"></a><span data-ttu-id="abba6-101">Lexikalische Grammatik</span><span class="sxs-lookup"><span data-stu-id="abba6-101">Lexical Grammar</span></span>

<span data-ttu-id="abba6-102">Kompilierung von Visual Basic-Programms muss zunächst den unformatierten Stream von Unicode-Zeichen in einer geordneten Menge von Lexikalischer Token zu übersetzen.</span><span class="sxs-lookup"><span data-stu-id="abba6-102">Compilation of a Visual Basic program first involves translating the raw stream of Unicode characters into an ordered set of lexical tokens.</span></span> <span data-ttu-id="abba6-103">Da Visual Basic-Sprache kein bestimmtes Format ist, wird der Satz von Token dann weiter in eine Reihe von logische Zeilen dividiert.</span><span class="sxs-lookup"><span data-stu-id="abba6-103">Because the Visual Basic language is not free-format, the set of tokens is then further divided into a series of logical lines.</span></span> <span data-ttu-id="abba6-104">Ein *logischen Zeile* Spannen aus entweder den Anfang des Streams oder ein Zeilenabschlusszeichen bis zum nächsten Zeilenabschlusszeichen, die nicht bis zum Ende des Streams Zeilenfortsetzungszeichen oder über vorangestellt ist.</span><span class="sxs-lookup"><span data-stu-id="abba6-104">A *logical line* spans from either the start of the stream or a line terminator through to the next line terminator that is not preceded by a line continuation or through to the end of the stream.</span></span>

<span data-ttu-id="abba6-105">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="abba6-105">__Note.__</span></span> <span data-ttu-id="abba6-106">Mit der Einführung von XML-Literale Ausdrücke in der Version 9.0 der Sprache verfügt Visual Basic nicht mehr eine unterschiedliche lexikalische Grammatik in dem Sinne, dass Visual Basic-Code ohne Berücksichtigung des syntaktischen Kontexts mit einem Token versehen werden kann.</span><span class="sxs-lookup"><span data-stu-id="abba6-106">With the introduction of XML literal expressions in version 9.0 of the language, Visual Basic no longer has a distinct lexical grammar in the sense that Visual Basic code can be tokenized without regard to the syntactic context.</span></span> <span data-ttu-id="abba6-107">Dies ist darauf zurückzuführen, XML und Visual Basic verschiedene Regeln für die lexikalische besitzen und der Satz von lexikalischen Regeln zu einem bestimmten Zeitpunkt hängt von welche syntaktische Konstrukt zu diesem Zeitpunkt verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="abba6-107">This is due to the fact that XML and Visual Basic have different lexical rules and the set of lexical rules in use at any particular time depends on what syntactic construct is being processed at that moment.</span></span> <span data-ttu-id="abba6-108">In dieser Spezifikation werden in diesem Abschnitt lexikalische Grammatik als Leitfaden für den lexikalischen Regeln der regulären Visual Basic-Code beibehalten.</span><span class="sxs-lookup"><span data-stu-id="abba6-108">This specification retains this lexical grammar section as a guide to the lexical rules of regular Visual Basic code.</span></span>

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

## <a name="characters-and-lines"></a><span data-ttu-id="abba6-109">Zeichen und Zeilen</span><span class="sxs-lookup"><span data-stu-id="abba6-109">Characters and Lines</span></span>

<span data-ttu-id="abba6-110">Visual Basic-Programme bestehen aus Zeichen von Unicode-Zeichensatz.</span><span class="sxs-lookup"><span data-stu-id="abba6-110">Visual Basic programs are composed of characters from the Unicode character set.</span></span>

```antlr
Character:
    '<Any Unicode character except a LineTerminator>'
    ;
```

### <a name="line-terminators"></a><span data-ttu-id="abba6-111">Zeilenabschlusszeichen</span><span class="sxs-lookup"><span data-stu-id="abba6-111">Line Terminators</span></span>

<span data-ttu-id="abba6-112">Unicode-Zeilenumbruchzeichen trennen logische Zeilen.</span><span class="sxs-lookup"><span data-stu-id="abba6-112">Unicode line break characters separate logical lines.</span></span>

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

### <a name="line-continuation"></a><span data-ttu-id="abba6-113">Zeilenfortsetzung</span><span class="sxs-lookup"><span data-stu-id="abba6-113">Line Continuation</span></span>

<span data-ttu-id="abba6-114">Ein *Zeile Fortsetzung* besteht aus mindestens ein Leerzeichen, das sofort einen einzelner Unterstrich als letztes Zeichen in einer Textzeile (außer Leerzeichen) vorangestellt ist.</span><span class="sxs-lookup"><span data-stu-id="abba6-114">A *line continuation* consists of at least one white-space character that immediately precedes a single underscore character as the last character (other than white space) in a text line.</span></span> <span data-ttu-id="abba6-115">Eine zeilenfortsetzung ermöglicht es eine logische Zeile über mehrere physische Zeilen erstrecken.</span><span class="sxs-lookup"><span data-stu-id="abba6-115">A line continuation allows a logical line to span more than one physical line.</span></span> <span data-ttu-id="abba6-116">Zeilenfortsetzungen werden behandelt, als wären sie Leerzeichen, auch wenn dies nicht der Fall.</span><span class="sxs-lookup"><span data-stu-id="abba6-116">Line continuations are treated as if they were white space, even though they are not.</span></span>

```antlr
LineContinuation
    : WhiteSpace '_' WhiteSpace* LineTerminator
    ;
```

<span data-ttu-id="abba6-117">Das folgende Programm zeigt einige zeilenfortsetzungen:</span><span class="sxs-lookup"><span data-stu-id="abba6-117">The following program shows some line continuations:</span></span>

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

<span data-ttu-id="abba6-118">Einige stellen in der syntaktischen Grammatik ermöglichen *impliziten zeilenfortsetzungen*.</span><span class="sxs-lookup"><span data-stu-id="abba6-118">Some places in the syntactic grammar allow for *implicit line continuations*.</span></span> <span data-ttu-id="abba6-119">Wenn ein Zeilenabschlusszeichen festgestellt wird</span><span class="sxs-lookup"><span data-stu-id="abba6-119">When a line terminator is encountered</span></span>

* <span data-ttu-id="abba6-120">nach einem Komma (`,`), offene Klammer (`(`), offene geschweifte Klammer (`{`), oder öffnen Sie die eingebetteten Ausdruck (`<%=`)</span><span class="sxs-lookup"><span data-stu-id="abba6-120">after a comma (`,`), open parenthesis (`(`), open curly brace (`{`), or open embedded expression (`<%=`)</span></span>

* <span data-ttu-id="abba6-121">nach der ein Memberqualifizierer (`.` oder `.@` oder `...`), vorausgesetzt, dass etwas qualifiziert wird, wird (d. h. nicht verwendet einen impliziten `With` Kontext)</span><span class="sxs-lookup"><span data-stu-id="abba6-121">after a member qualifier (`.` or `.@` or `...`), provided that something is being qualified (i.e. is not using an implicit `With` context)</span></span>

* <span data-ttu-id="abba6-122">vor einer schließenden Klammer (`)`), schließen Sie die geschweifte Klammer (`}`), oder Schließen eines eingebetteten Ausdrucks (`%>`)</span><span class="sxs-lookup"><span data-stu-id="abba6-122">before a close parenthesis (`)`), close curly brace (`}`), or close embedded expression (`%>`)</span></span>

* <span data-ttu-id="abba6-123">nach einer kleiner-als (`<`) in einem Attributkontext</span><span class="sxs-lookup"><span data-stu-id="abba6-123">after a less-than (`<`) in an attribute context</span></span>

* <span data-ttu-id="abba6-124">Bevor Sie ein größer-als (`>`) in einem Attributkontext</span><span class="sxs-lookup"><span data-stu-id="abba6-124">before a greater-than (`>`) in an attribute context</span></span>

* <span data-ttu-id="abba6-125">nach einem größer-als (`>`) in einem Attributkontext auf nicht-Datei-</span><span class="sxs-lookup"><span data-stu-id="abba6-125">after a greater-than (`>`) in a non-file-level attribute context</span></span>

* <span data-ttu-id="abba6-126">vor und nach Abfrageoperatoren (`Where`, `Order`, `Select`usw..)</span><span class="sxs-lookup"><span data-stu-id="abba6-126">before and after query operators (`Where`, `Order`, `Select`, etc.)</span></span>

* <span data-ttu-id="abba6-127">nach binären Operatoren (`+`, `-`, `/`, `*`usw.) in einem Ausdruckskontext</span><span class="sxs-lookup"><span data-stu-id="abba6-127">after binary operators (`+`, `-`, `/`, `*`, etc.) in an expression context</span></span>

* <span data-ttu-id="abba6-128">nach dem Zuweisungsoperatoren (`=`, `:=`, `+=`, `-=`usw.) in jedem Kontext.</span><span class="sxs-lookup"><span data-stu-id="abba6-128">after assignment operators (`=`, `:=`, `+=`, `-=`, etc.) in any context.</span></span>

<span data-ttu-id="abba6-129">die für den Zeilenabschluss wird behandelt, als ob es sich um eine zeilenfortsetzung war.</span><span class="sxs-lookup"><span data-stu-id="abba6-129">the line terminator is treated as if it was a line continuation.</span></span>

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

<span data-ttu-id="abba6-130">Im vorherige Beispiel könnte beispielsweise auch folgendermaßen geschrieben werden:</span><span class="sxs-lookup"><span data-stu-id="abba6-130">For example, the previous example could also be written as:</span></span>

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

<span data-ttu-id="abba6-131">Impliziten zeilenfortsetzungen werden immer nur direkt vor oder nach dem angegebenen Token abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="abba6-131">Implicit line continuations will only ever be inferred directly before or after the specified token.</span></span> <span data-ttu-id="abba6-132">Sie werden nicht vor oder nach einem zeilenfortsetzung abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="abba6-132">They will not be inferred before or after a line continuation.</span></span> <span data-ttu-id="abba6-133">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="abba6-133">For example:</span></span>

```vb
Dim y = 10
' Error: Expression expected for assignment to x
Dim x = _

y
```

<span data-ttu-id="abba6-134">Zeilenfortsetzungen werden in Kontexten für bedingte Kompilierung nicht abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="abba6-134">Line continuations will not be inferred in conditional compilation contexts.</span></span> <span data-ttu-id="abba6-135">(__Beachten.__</span><span class="sxs-lookup"><span data-stu-id="abba6-135">(__Note.__</span></span> <span data-ttu-id="abba6-136">Diese letzte Einschränkung ist erforderlich, da der Text in Blöcken für die bedingte Kompilierung, die nicht kompiliert werden keine syntaktisch gültig ist.</span><span class="sxs-lookup"><span data-stu-id="abba6-136">This last restriction is required because text in conditional compilation blocks that are not compiled do not have to be syntactically valid.</span></span> <span data-ttu-id="abba6-137">Daher Text im Block möglicherweise versehentlich "durch die Anweisung für die bedingte Kompilierung übernommen abrufen" insbesondere dann, wenn die Sprache in der Zukunft erweitert ruft.)</span><span class="sxs-lookup"><span data-stu-id="abba6-137">Thus, text in the block might accidentally get "picked up" by the conditional compilation statement, especially as the language gets extended in the future.)</span></span>


### <a name="white-space"></a><span data-ttu-id="abba6-138">Leerraum</span><span class="sxs-lookup"><span data-stu-id="abba6-138">White Space</span></span>

<span data-ttu-id="abba6-139">*Leerraum* dient nur zum Trennen von Tokens und wird andernfalls ignoriert.</span><span class="sxs-lookup"><span data-stu-id="abba6-139">*White space* serves only to separate tokens and is otherwise ignored.</span></span> <span data-ttu-id="abba6-140">Logische Zeilen, die nur Leerzeichen enthält, werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="abba6-140">Logical lines containing only white space are ignored.</span></span> <span data-ttu-id="abba6-141">(__Beachten.__</span><span class="sxs-lookup"><span data-stu-id="abba6-141">(__Note.__</span></span>
<span data-ttu-id="abba6-142">Zeilenabschlusszeichen werden nicht mit Leerzeichen berücksichtigt.)</span><span class="sxs-lookup"><span data-stu-id="abba6-142">Line terminators are not considered white space.)</span></span>

```antlr
WhiteSpace
    : '<Unicode class Zs>'
    | '<Unicode Tab 0x0009>'
    ;
```

### <a name="comments"></a><span data-ttu-id="abba6-143">Kommentare</span><span class="sxs-lookup"><span data-stu-id="abba6-143">Comments</span></span>

<span data-ttu-id="abba6-144">Ein *Kommentar* beginnt mit einem einzelnen Anführungszeichen oder das Schlüsselwort `REM`.</span><span class="sxs-lookup"><span data-stu-id="abba6-144">A *comment* begins with a single-quote character or the keyword `REM`.</span></span> <span data-ttu-id="abba6-145">Ein Apostroph-Zeichen wird entweder ein ASCII-Apostroph-Zeichen, eine Unicode-Zeichen nach links einfache Anführungszeichen oder ein Recht Unicode-Zeichen einfache Anführungszeichen.</span><span class="sxs-lookup"><span data-stu-id="abba6-145">A single-quote character is either an ASCII single-quote character, a Unicode left single-quote character, or a Unicode right single-quote character.</span></span> <span data-ttu-id="abba6-146">Kommentare können an einer beliebigen Stelle auf einer Quellzeile beginnen, und das Ende der physischen Zeile der Kommentar endet.</span><span class="sxs-lookup"><span data-stu-id="abba6-146">Comments can begin anywhere on a source line, and the end of the physical line ends the comment.</span></span> <span data-ttu-id="abba6-147">Der Compiler ignoriert die Zeichen zwischen dem Anfang des Kommentars und das Zeilenabschlusszeichen an.</span><span class="sxs-lookup"><span data-stu-id="abba6-147">The compiler ignores the characters between the beginning of the comment and the line terminator.</span></span> <span data-ttu-id="abba6-148">Daher können keine Kommentare über mehrere Zeilen mithilfe von zeilenfortsetzungen erweitern.</span><span class="sxs-lookup"><span data-stu-id="abba6-148">Consequently, comments cannot extend across multiple lines by using line continuations.</span></span>

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

## <a name="identifiers"></a><span data-ttu-id="abba6-149">Bezeichner</span><span class="sxs-lookup"><span data-stu-id="abba6-149">Identifiers</span></span>

<span data-ttu-id="abba6-150">Ein *Bezeichner* ist ein Name.</span><span class="sxs-lookup"><span data-stu-id="abba6-150">An *identifier* is a name.</span></span> <span data-ttu-id="abba6-151">Visual Basic-Bezeichner entsprechen, die Unicode-Standard-Anhang-15 mit einer Ausnahme: Bezeichner können mit einem Unterstrich (Connector) beginnen.</span><span class="sxs-lookup"><span data-stu-id="abba6-151">Visual Basic identifiers conform to the Unicode Standard Annex 15 with one exception: identifiers may begin with an underscore (connector) character.</span></span> <span data-ttu-id="abba6-152">Wenn ein Bezeichner mit einem Unterstrich beginnt, darf es mindestens ein gültiges Bezeichnerzeichen um sie von einer zeilenfortsetzung eindeutig zu machen.</span><span class="sxs-lookup"><span data-stu-id="abba6-152">If an identifier begins with an underscore, it must contain at least one other valid identifier character to disambiguate it from a line continuation.</span></span>

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

<span data-ttu-id="abba6-153">Reguläre Bezeichner können nicht mit Schlüsselwörtern übereinstimmen, jedoch können mit Escapezeichen versehene Bezeichner oder Bezeichner mit einem Typzeichen.</span><span class="sxs-lookup"><span data-stu-id="abba6-153">Regular identifiers may not match keywords, but escaped identifiers or identifiers with a type character can.</span></span> <span data-ttu-id="abba6-154">Ein *mit Escapezeichen versehen Bezeichner* ist ein Bezeichner durch eckige Klammern getrennt.</span><span class="sxs-lookup"><span data-stu-id="abba6-154">An *escaped identifier* is an identifier delimited by square brackets.</span></span> <span data-ttu-id="abba6-155">Mit Escapezeichen versehenen Bezeichner führen Sie dieselben Regeln wie für reguläre Bezeichner mit dem Unterschied, dass sie möglicherweise mit Schlüsselwörtern übereinstimmen, und dürfen keine Typzeichen.</span><span class="sxs-lookup"><span data-stu-id="abba6-155">Escaped identifiers follow the same rules as regular identifiers except that they may match keywords and may not have type characters.</span></span>

<span data-ttu-id="abba6-156">In diesem Beispiel definiert eine Klasse namens `class` mit einer freigegebenen Methode namens `shared` , akzeptiert einen Parameter namens `boolean` und ruft dann die Methode.</span><span class="sxs-lookup"><span data-stu-id="abba6-156">This example defines a class named `class` with a shared method named `shared` that takes a parameter named `boolean` and then calls the method.</span></span>

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

<span data-ttu-id="abba6-157">Bezeichner sind Groß-/Kleinschreibung, also den gleichen Bezeichner sein, wenn sie nur im Fall unterscheiden sich zwei Bezeichner gelten.</span><span class="sxs-lookup"><span data-stu-id="abba6-157">Identifiers are case insensitive, so two identifiers are considered to be the same identifier if they differ only in case.</span></span> <span data-ttu-id="abba6-158">(__Beachten.__</span><span class="sxs-lookup"><span data-stu-id="abba6-158">(__Note.__</span></span> <span data-ttu-id="abba6-159">Die Unicode-Standard 1: 1-schreibungszuordnungen werden verwendet, wenn es sich bei Vergleichen von Bezeichnern und Groß-/Kleinschreibung Gebietsschema-spezifische Zuordnungen werden ignoriert.)</span><span class="sxs-lookup"><span data-stu-id="abba6-159">The Unicode Standard one-to-one case mappings are used when comparing identifiers and any locale-specific case mappings are ignored.)</span></span>


### <a name="type-characters"></a><span data-ttu-id="abba6-160">Typzeichen</span><span class="sxs-lookup"><span data-stu-id="abba6-160">Type Characters</span></span>

<span data-ttu-id="abba6-161">Ein *Typzeichen* bezeichnet den Typ des vorherigen Bezeichners.</span><span class="sxs-lookup"><span data-stu-id="abba6-161">A *type character* denotes the type of the preceding identifier.</span></span> <span data-ttu-id="abba6-162">Das Typzeichen gilt nicht als Teil des Bezeichners.</span><span class="sxs-lookup"><span data-stu-id="abba6-162">The type character is not considered part of the identifier.</span></span>

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

<span data-ttu-id="abba6-163">Wenn eine Deklaration ein Typzeichen enthält, muss das Typzeichen mit den in die eigentliche Deklaration angegebenen Typ übereinstimmen; andernfalls tritt ein Kompilierungsfehler auf.</span><span class="sxs-lookup"><span data-stu-id="abba6-163">If a declaration includes a type character, the type character must agree with the type specified in the declaration itself; otherwise, a compile-time error occurs.</span></span> <span data-ttu-id="abba6-164">Wenn die Deklaration den Typ lässt (z. B., wenn er nicht angegeben ist ein `As` Klausel), das Typzeichen ist implizit als den Typ der Deklaration ersetzt.</span><span class="sxs-lookup"><span data-stu-id="abba6-164">If the declaration omits the type (for example, if it does not specify an `As` clause), the type character is implicitly substituted as the type of the declaration.</span></span>

<span data-ttu-id="abba6-165">Keine Leerzeichen können zwischen einem Bezeichner und die Typzeichen stammen.</span><span class="sxs-lookup"><span data-stu-id="abba6-165">No white space may come between an identifier and its type character.</span></span> <span data-ttu-id="abba6-166">Es gibt keine Typzeichen für `Byte`, `SByte`, `UShort`, `Short`, `UInteger` oder `ULong`, da ein Mangel an geeigneten Zeichen.</span><span class="sxs-lookup"><span data-stu-id="abba6-166">There are no type characters for `Byte`, `SByte`, `UShort`, `Short`, `UInteger` or `ULong`, due to a lack of suitable characters.</span></span>

<span data-ttu-id="abba6-167">Ein Typzeichen anfügen, ein Bezeichner, der vom Konzept her nicht über einen Typ (z. B. ein Namespacename) verfügt oder ein Bezeichner, dessen Typ stimmt nicht mit dem Typ des Typzeichen, bewirkt, dass einen Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="abba6-167">Appending a type character to an identifier that conceptually does not have a type (for example, a namespace name) or to an identifier whose type disagrees with the type of the type character causes a compile-time error.</span></span>

<span data-ttu-id="abba6-168">Das folgende Beispiel zeigt die Verwendung von Typzeichen:</span><span class="sxs-lookup"><span data-stu-id="abba6-168">The following example shows the use of type characters:</span></span>

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

<span data-ttu-id="abba6-169">Das Typzeichen `!` als spezielle problematisch, da es sowohl als ein Typzeichen als ein Trennzeichen in der Sprache verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="abba6-169">The type character `!` presents a special problem in that it can be used both as a type character and as a separator in the language.</span></span> <span data-ttu-id="abba6-170">Um Mehrdeutigkeit, eine `!` Zeichen ist ein Typzeichen, solange die darauf folgende Zeichen ein Bezeichners nicht gestartet werden kann.</span><span class="sxs-lookup"><span data-stu-id="abba6-170">To remove ambiguity, a `!` character is a type character as long as the character that follows it cannot start an identifier.</span></span> <span data-ttu-id="abba6-171">Wenn dies möglich ist, und klicken Sie dann die `!` Zeichen ein Trennzeichen ist, nicht über ein Typzeichen.</span><span class="sxs-lookup"><span data-stu-id="abba6-171">If it can, then the `!` character is a separator, not a type character.</span></span>


## <a name="keywords"></a><span data-ttu-id="abba6-172">Schlüsselwörter</span><span class="sxs-lookup"><span data-stu-id="abba6-172">Keywords</span></span>

<span data-ttu-id="abba6-173">Ein *Schlüsselwort* ist ein Wort, das in einem Sprachkonstrukt besondere Bedeutung hat.</span><span class="sxs-lookup"><span data-stu-id="abba6-173">A *keyword* is a word that has special meaning in a language construct.</span></span> <span data-ttu-id="abba6-174">Alle Schlüsselwörter sind von der Sprache reserviert und können nicht als Bezeichner verwendet werden, es sei denn, die Bezeichner mit Escapezeichen versehen werden.</span><span class="sxs-lookup"><span data-stu-id="abba6-174">All keywords are reserved by the language and may not be used as identifiers unless the identifiers are escaped.</span></span> <span data-ttu-id="abba6-175">(__Beachten.__</span><span class="sxs-lookup"><span data-stu-id="abba6-175">(__Note.__</span></span> <span data-ttu-id="abba6-176">`EndIf``GoSub`, `Let`, `Variant`, und `Wend` werden als Schlüsselwörter, beibehalten, obwohl sie nicht mehr in Visual Basic verwendet werden.)</span><span class="sxs-lookup"><span data-stu-id="abba6-176">`EndIf`, `GoSub`, `Let`, `Variant`, and `Wend` are retained as keywords, although they are no longer used in Visual Basic.)</span></span>

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

## <a name="literals"></a><span data-ttu-id="abba6-177">Literale</span><span class="sxs-lookup"><span data-stu-id="abba6-177">Literals</span></span>

<span data-ttu-id="abba6-178">Ein *literal* ist eine Textdarstellung eines bestimmten Werts eines Typs.</span><span class="sxs-lookup"><span data-stu-id="abba6-178">A *literal* is a textual representation of a particular value of a type.</span></span> <span data-ttu-id="abba6-179">Zeichenfolgenliterale-Typen enthalten, boolescher Wert, ganze Zahl, Gleitkomma, Zeichenfolge, Zeichen und Datum.</span><span class="sxs-lookup"><span data-stu-id="abba6-179">Literal types include Boolean, integer, floating point, string, character, and date.</span></span>

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

### <a name="boolean-literals"></a><span data-ttu-id="abba6-180">Boolesche Literale</span><span class="sxs-lookup"><span data-stu-id="abba6-180">Boolean Literals</span></span>

<span data-ttu-id="abba6-181">`True` und `False` sind Literale, die von der `Boolean` Typ, der bzw. in den Zustand "true" und "false", zuordnen.</span><span class="sxs-lookup"><span data-stu-id="abba6-181">`True` and `False` are literals of the `Boolean` type that map to the true and false state, respectively.</span></span>

```antlr
BooleanLiteral
    : 'True' | 'False'
    ;
```

### <a name="integer-literals"></a><span data-ttu-id="abba6-182">Integer-Literale</span><span class="sxs-lookup"><span data-stu-id="abba6-182">Integer Literals</span></span>

<span data-ttu-id="abba6-183">Ganzzahlliterale können dezimale (Basis 10), oktale (Basis 8) oder Hexadezimalformat (Basis 16) sein.</span><span class="sxs-lookup"><span data-stu-id="abba6-183">Integer literals can be decimal (base 10), hexadecimal (base 16), or octal (base 8).</span></span> <span data-ttu-id="abba6-184">Ein Ganzzahlliteral im Dezimalformat ist eine Zeichenfolge aus Dezimalzahlen (0-9).</span><span class="sxs-lookup"><span data-stu-id="abba6-184">A decimal integer literal is a string of decimal digits (0-9).</span></span> <span data-ttu-id="abba6-185">Ist ein hexadezimales Literal `&H` gefolgt von einer Zeichenfolge von hexadezimalen Ziffern (0-9, A-F).</span><span class="sxs-lookup"><span data-stu-id="abba6-185">A hexadecimal literal is `&H` followed by a string of hexadecimal digits (0-9, A-F).</span></span> <span data-ttu-id="abba6-186">Ist ein oktales Literal `&O` gefolgt von einer Zeichenfolge von oktalen Ziffern (0-7).</span><span class="sxs-lookup"><span data-stu-id="abba6-186">An octal literal is `&O` followed by a string of octal digits (0-7).</span></span> <span data-ttu-id="abba6-187">Dezimale Literale direkt den Dezimalwert der ganzzahliges Literal darstellen, während oktale und hexadezimale Literale den Binärwert des Integer-literal darstellen (also `&H8000S` -32768, kein Überlauffehler).</span><span class="sxs-lookup"><span data-stu-id="abba6-187">Decimal literals directly represent the decimal value of the integral literal, whereas octal and hexadecimal literals represent the binary value of the integer literal (thus, `&H8000S` is -32768, not an overflow error).</span></span>

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

<span data-ttu-id="abba6-188">Der Typ eines Literals wird durch seinen Wert oder durch das folgende Typzeichen bestimmt.</span><span class="sxs-lookup"><span data-stu-id="abba6-188">The type of a literal is determined by its value or by the following type character.</span></span> <span data-ttu-id="abba6-189">Wenn kein Typzeichen angegeben ist, Werte, die im Bereich der `Integer` Typ typisiert sind, als `Integer`; Werte außerhalb des Bereichs für `Integer` typisiert sind, als `Long`.</span><span class="sxs-lookup"><span data-stu-id="abba6-189">If no type character is specified, values in the range of the `Integer` type are typed as `Integer`; values outside the range for `Integer` are typed as `Long`.</span></span> <span data-ttu-id="abba6-190">Wenn ein Integer-Literal-Typ, der das Ganzzahlliteral nicht groß genug ist, führt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="abba6-190">If an integer literal's type is of insufficient size to hold the integer literal, a compile-time error results.</span></span> <span data-ttu-id="abba6-191">(__Beachten.__</span><span class="sxs-lookup"><span data-stu-id="abba6-191">(__Note.__</span></span> <span data-ttu-id="abba6-192">Gibt es kein Typzeichen für `Byte` da wäre das naheliegendste Zeichen `B`, d.h. ein zulässigen Zeichen in ein hexadezimales Literal.)</span><span class="sxs-lookup"><span data-stu-id="abba6-192">There isn't a type character for `Byte` because the most natural character would be `B`, which is a legal character in a hexadecimal literal.)</span></span>


### <a name="floating-point-literals"></a><span data-ttu-id="abba6-193">Gleitkommaliterale</span><span class="sxs-lookup"><span data-stu-id="abba6-193">Floating-Point Literals</span></span>

<span data-ttu-id="abba6-194">Einem Gleitkommaliteral ist ein Integer-literal gefolgt von einem optionalen Dezimaltrennzeichen (der Zeitraum ASCII-Zeichen) und Mantisse und eine optionale Basis 10 Exponent.</span><span class="sxs-lookup"><span data-stu-id="abba6-194">A floating-point literal is an integer literal followed by an optional decimal point (the ASCII period character) and mantissa, and an optional base 10 exponent.</span></span> <span data-ttu-id="abba6-195">Standardmäßig ist ein Gleitkomma-Literal vom Typ `Double`.</span><span class="sxs-lookup"><span data-stu-id="abba6-195">By default, a floating-point literal is of type `Double`.</span></span> <span data-ttu-id="abba6-196">Wenn die `Single`, `Double`, oder `Decimal` Typzeichen angegeben wird, ist Sie das Literal von diesem Typ.</span><span class="sxs-lookup"><span data-stu-id="abba6-196">If the `Single`, `Double`, or `Decimal` type character is specified, the literal is of that type.</span></span> <span data-ttu-id="abba6-197">Wenn ein Typ des Gleitkommaliterals, des Gleitkommaliterals nicht groß genug ist, führt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="abba6-197">If a floating-point literal's type is of insufficient size to hold the floating-point literal, a compile-time error results.</span></span>

<span data-ttu-id="abba6-198">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="abba6-198">__Note.__</span></span> <span data-ttu-id="abba6-199">Es ist erwähnenswert, die die `Decimal` -Datentyp kann nachfolgende Nullen in einen Wert zu codieren.</span><span class="sxs-lookup"><span data-stu-id="abba6-199">It is worth noting that the `Decimal` data type can encode trailing zeros in a value.</span></span> <span data-ttu-id="abba6-200">Die Spezifikation ist derzeit kein Kommentar zu gibt an, ob nachfolgende in Nullen einer `Decimal` Literal vom Compiler berücksichtigt werden.</span><span class="sxs-lookup"><span data-stu-id="abba6-200">The specification currently makes no comment about whether trailing zeros in a `Decimal` literal should be honored by a compiler.</span></span>

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

### <a name="string-literals"></a><span data-ttu-id="abba6-201">Zeichenfolgenliterale</span><span class="sxs-lookup"><span data-stu-id="abba6-201">String Literals</span></span>

<span data-ttu-id="abba6-202">Ein Zeichenfolgenliteral ist eine Sequenz von NULL oder mehr Unicode-Zeichen beginnt und endet mit einem ASCII-Anführungszeichen, eine Unicode-Zeichen nach links doppelte Anführungszeichen oder einem Unicode-Zeichen nach rechts doppelte Anführungszeichen.</span><span class="sxs-lookup"><span data-stu-id="abba6-202">A string literal is a sequence of zero or more Unicode characters beginning and ending with an ASCII double-quote character, a Unicode left double-quote character, or a Unicode right double-quote character.</span></span> <span data-ttu-id="abba6-203">Innerhalb einer Zeichenfolge ist eine Sequenz von zwei doppelte Anführungszeichen eine Escapesequenz, die ein doppeltes Anführungszeichen in der Zeichenfolge darstellt.</span><span class="sxs-lookup"><span data-stu-id="abba6-203">Within a string, a sequence of two double-quote characters is an escape sequence representing a double quote in the string.</span></span>

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

<span data-ttu-id="abba6-204">Eine Zeichenfolgenkonstante ist von der `String` Typ.</span><span class="sxs-lookup"><span data-stu-id="abba6-204">A string constant is of the `String` type.</span></span>

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

<span data-ttu-id="abba6-205">Der Compiler kann einen Konstante Zeichenfolge-Ausdruck mit einem Zeichenfolgenliteral zu ersetzen.</span><span class="sxs-lookup"><span data-stu-id="abba6-205">The compiler is allowed to replace a constant string expression with a string literal.</span></span> <span data-ttu-id="abba6-206">Jeder String-literal führt nicht unbedingt in eine neue Zeichenfolgeninstanz.</span><span class="sxs-lookup"><span data-stu-id="abba6-206">Each string literal does not necessarily result in a new string instance.</span></span> <span data-ttu-id="abba6-207">Wenn zwei oder mehr Zeichenfolgenliterale, die gemäß den Zeichenfolge-Equality-Operator mit binärer Vergleichssemantik äquivalent sind, im selben Programm angezeigt werden, können diese Zeichenfolgenliterale, auf die gleiche Zeichenfolgeninstanz verweisen.</span><span class="sxs-lookup"><span data-stu-id="abba6-207">When two or more string literals that are equivalent according to the string equality operator using binary comparison semantics appear in the same program, these string literals may refer to the same string instance.</span></span> <span data-ttu-id="abba6-208">Beispielsweise kann die Ausgabe des folgenden Programms zurückgeben `True` da die beiden Literale auf die gleiche Zeichenfolgeninstanz verweisen können.</span><span class="sxs-lookup"><span data-stu-id="abba6-208">For instance, the output of the following program may return `True` because the two literals may refer to the same string instance.</span></span>

```vb
Module Test
    Sub Main()
        Dim a As Object = "he" & "llo"
        Dim b As Object = "hello"
        Console.WriteLine(a Is b)
    End Sub
End Module
```


### <a name="character-literals"></a><span data-ttu-id="abba6-209">Zeichenliterale</span><span class="sxs-lookup"><span data-stu-id="abba6-209">Character Literals</span></span>

<span data-ttu-id="abba6-210">Ein Zeichenfolgenliteral stellt ein einzelnes Unicodezeichen von der `Char` Typ.</span><span class="sxs-lookup"><span data-stu-id="abba6-210">A character literal represents a single Unicode character of the `Char` type.</span></span> <span data-ttu-id="abba6-211">Zwei doppelte Anführungszeichen ist eine Escapesequenz, die die doppelten Anführungszeichen darstellt.</span><span class="sxs-lookup"><span data-stu-id="abba6-211">Two double-quote characters is an escape sequence representing the double-quote character.</span></span>

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


### <a name="date-literals"></a><span data-ttu-id="abba6-212">Datumsliterale</span><span class="sxs-lookup"><span data-stu-id="abba6-212">Date Literals</span></span>

<span data-ttu-id="abba6-213">Ein Datumsliteral darstellt, einen bestimmten Zeitpunkt als Wert des der `Date` Typ.</span><span class="sxs-lookup"><span data-stu-id="abba6-213">A date literal represents a particular moment in time expressed as a value of the `Date` type.</span></span>

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

<span data-ttu-id="abba6-214">Das Literal kann ein Datum und einem Zeitpunkt nur ein Datum oder nur eine Uhrzeit angeben.</span><span class="sxs-lookup"><span data-stu-id="abba6-214">The literal may specify both a date and a time, just a date, or just a time.</span></span> <span data-ttu-id="abba6-215">Wenn der Date-Wert weggelassen wird, wird am 1. Januar des Jahres 1 im gregorianischen Kalender ausgegangen.</span><span class="sxs-lookup"><span data-stu-id="abba6-215">If the date value is omitted, then January 1 of the year 1 in the Gregorian calendar is assumed.</span></span> <span data-ttu-id="abba6-216">Wenn der Zeitwert weggelassen wird, wird 12:00:00 Uhr angenommen.</span><span class="sxs-lookup"><span data-stu-id="abba6-216">If the time value is omitted, then 12:00:00 AM is assumed.</span></span>

<span data-ttu-id="abba6-217">Zur Vermeidung von Problemen bei der Interpretation der Wert des Jahres in einen Date-Wert darf nicht den Wert des Jahres zweistellig sein.</span><span class="sxs-lookup"><span data-stu-id="abba6-217">To avoid problems with interpreting the year value in a date value, the year value cannot be two digits.</span></span> <span data-ttu-id="abba6-218">Wenn Sie ein Datum in der ersten Jahrhundert AD/CE auszudrücken, muss führenden Nullen angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="abba6-218">When expressing a date in the first century AD/CE, leading zeros must be specified.</span></span>

<span data-ttu-id="abba6-219">Ein Zeitwert kann entweder über einen 24-Stunden-Wert oder einen 12-Stunden-Wert angegeben werden; Zeitwerte, die auslassen einer `AM` oder `PM` wird angenommen, dass 24-Stunden-Werte.</span><span class="sxs-lookup"><span data-stu-id="abba6-219">A time value may be specified either using a 24-hour value or a 12-hour value; time values that omit an `AM` or `PM` are assumed to be 24-hour values.</span></span> <span data-ttu-id="abba6-220">Wenn Sie ein Time-Wert, die Minuten, das Literal lässt `0` wird standardmäßig verwendet.</span><span class="sxs-lookup"><span data-stu-id="abba6-220">If a time value omits the minutes, the literal `0` is used by default.</span></span> <span data-ttu-id="abba6-221">Wenn Sie ein Time-Wert, die Sekunden, das Literal lässt `0` wird standardmäßig verwendet.</span><span class="sxs-lookup"><span data-stu-id="abba6-221">If a time value omits the seconds, the literal `0` is used by default.</span></span> <span data-ttu-id="abba6-222">Wenn weder Minuten noch Sekunden, dann weggelassen werden `AM` oder `PM` muss angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="abba6-222">If both minutes and second are omitted, then `AM` or `PM` must be specified.</span></span> <span data-ttu-id="abba6-223">Ist der angegebene Datumswert außerhalb des Bereichs von der `Date` eingeben, ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="abba6-223">If the date value specified is outside the range of the `Date` type, a compile-time error occurs.</span></span>

<span data-ttu-id="abba6-224">Das folgende Beispiel enthält mehrere Datumsliterale.</span><span class="sxs-lookup"><span data-stu-id="abba6-224">The following example contains several date literals.</span></span>

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


### <a name="nothing"></a><span data-ttu-id="abba6-225">Nothing</span><span class="sxs-lookup"><span data-stu-id="abba6-225">Nothing</span></span>

<span data-ttu-id="abba6-226">`Nothing` ist ein spezielles Literal. Er verfügt nicht über einen Typ und wird im Typsystem, einschließlich der Typparameter für alle Typen konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="abba6-226">`Nothing` is a special literal; it does not have a type and is convertible to all types in the type system, including type parameters.</span></span> <span data-ttu-id="abba6-227">Wenn auf einen bestimmten Typ konvertiert wird, entspricht es der Standardwert dieses Typs.</span><span class="sxs-lookup"><span data-stu-id="abba6-227">When converted to a particular type, it is the equivalent of the default value of that type.</span></span>

```antlr
Nothing
    : 'Nothing'
    ;
```

## <a name="separators"></a><span data-ttu-id="abba6-228">Trennzeichen</span><span class="sxs-lookup"><span data-stu-id="abba6-228">Separators</span></span>

<span data-ttu-id="abba6-229">Die folgenden ASCII-Zeichen sind, Trennzeichen:</span><span class="sxs-lookup"><span data-stu-id="abba6-229">The following ASCII characters are separators:</span></span>

```antlr
Separator
    : '(' | ')' | '{' | '}' | '!' | '#' | ',' | '.' | ':' | '?'
    ;
```

## <a name="operator-characters"></a><span data-ttu-id="abba6-230">Operatorzeichen</span><span class="sxs-lookup"><span data-stu-id="abba6-230">Operator Characters</span></span>

<span data-ttu-id="abba6-231">Die folgenden ASCII-Zeichen oder Zeichenfolgen sind Operatoren:</span><span class="sxs-lookup"><span data-stu-id="abba6-231">The following ASCII characters or character sequences denote operators:</span></span>

```antlr
Operator
    : '&' | '*' | '+' | '-' | '/' | '\\' | '^' | '<' | '=' | '>'
    ;
```

