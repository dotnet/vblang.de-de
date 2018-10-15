# <a name="types"></a>Typen

Die zwei grundlegenden Kategorien von Typen in Visual Basic sind *Werttypen* und *Verweistypen*. Primitive Typen (mit Ausnahme von Zeichenfolgen), Enumerationen und Strukturen sind Werttypen. Klassen, Zeichenfolgen, standard-Module, Schnittstellen, Arrays und Delegaten sind Verweistypen.

Jeder Typ verfügt über eine *Standardwert*, dies ist der Wert, der auf Variablen dieses Typs bei der Initialisierung zugewiesen ist.

```antlr
TypeName
    : ArrayTypeName
    | NonArrayTypeName
    ;

NonArrayTypeName
    : SimpleTypeName
    | NullableTypeName
    ;

SimpleTypeName
    : QualifiedTypeName
    | BuiltInTypeName
    ;

QualifiedTypeName
    : Identifier TypeArguments? (Period IdentifierOrKeyword TypeArguments?)*
    | 'Global' Period IdentifierOrKeyword TypeArguments?
      (Period IdentifierOrKeyword TypeArguments?)*
    ;

TypeArguments
    : OpenParenthesis 'Of' TypeArgumentList CloseParenthesis
    ;

TypeArgumentList
    : TypeName ( Comma TypeName )*
    ;

BuiltInTypeName
    : 'Object'
    | PrimitiveTypeName
    ;

TypeModifier
    : AccessModifier
    | 'Shadows'
    ;

IdentifierModifiers
    : NullableNameModifier? ArrayNameModifier?
    ;
```

## <a name="value-types-and-reference-types"></a>Wert- und Verweistypen

Obwohl Werttypen und Referenztypen ähnliche Deklaration Syntax- und Nutzungsinformationen sein können, unterscheiden sich die Semantik.

Verweistypen werden auf dem Heap der Laufzeit gespeichert. Sie können nur über einen Verweis auf den Speicher zugegriffen werden. Da Verweistypen immer über Verweise zugegriffen werden, wird ihre Lebensdauer von .NET Framework verwaltet. Ausstehenden Verweise auf eine bestimmte Instanz werden nachverfolgt, und die Instanz wird zerstört, nur, wenn keine weiteren Verweise bleiben. Eine Variable eines Referenztyps enthält einen Verweis auf einen Wert dieses Typs, den Wert eines stärker abgeleiteten Typs oder ein null-Wert. Ein *null-Wert* bezieht sich auf "nothing"; Es ist nicht möglich, die nichts mit einem null-Wert darf nur zugewiesen. Zuweisung zu einer Variable eines Referenztyps erstellt eine Kopie des Werts auf die verwiesen wird, anstatt eine Kopie des Verweises. Für eine Variable eines Referenztyps ist der Standardwert ein null-Wert.

Werttypen werden direkt auf dem Stapel, entweder in einem Array oder in einem anderen Typ gespeichert. der Speicher kann nur direkt zugegriffen werden. Da Werttypen, direkt in der Variablen gespeichert werden, wird deren Lebensdauer durch die Lebensdauer der Variablen bestimmt, die sie enthält. Wenn Sie der Speicherort mit einer Werttypinstanz zerstört wird, wird die Werttypinstanz auch zerstört. Werttypen sind immer direkt zugegriffen werden. Es ist nicht möglich, einen Verweis auf einen Werttyp zu erstellen. Einen solchen Verweis Verbot macht es unmöglich, die auf eine Klasseninstanz Wert verweisen, die zerstört wurde. Da Werttypen, immer sind `NotInheritable`, eine Variable eines Werttyps enthält immer den Wert dieses Typs. Aus diesem Grund der Wert eines Werttyps handelt es sich nicht um einen null-Wert, noch können sie ein Objekt eines stärker abgeleiteten Typs verweisen. Zuweisung zu einer Variable eines Werttyps erstellt eine Kopie der Wert zugewiesen wird. Für eine Variable eines Werttyps ist der Standardwert für das Ergebnis der einzelnen Variablen Member des Typs auf den Standardwert zu initialisieren.

Das folgende Beispiel zeigt den Unterschied zwischen Verweistypen und Werttypen:

```vb
Class Class1
    Public Value As Integer = 0
End Class

Module Test
    Sub Main()
        Dim val1 As Integer = 0
        Dim val2 As Integer = val1
        val2 = 123
        Dim ref1 As Class1 = New Class1()
        Dim ref2 As Class1 = ref1
        ref2.Value = 123
        Console.WriteLine("Values: " & val1 & ", " & val2)
        Console.WriteLine("Refs: " & ref1.Value & ", " & ref2.Value)
    End Sub
End Module
```

Die Ausgabe des Programms lautet:

```
Values: 0, 123
Refs: 123, 123
```

Die Zuweisung auf die lokale Variable `val2` wirkt sich nicht auf die lokale Variable `val1` da beide lokalen Variablen einen Werttyp sind (welche `Integer`) und jede lokale Variable eines Werttyps hat seinen eigenen Speicher. Im Gegensatz dazu sind die Zuweisung `ref2.Value = 123;` wirkt sich auf das Objekt, das sowohl `ref1` und `ref2` Verweis.

Über das .NET Framework-Typsystem zu beachten ist, die, obwohl Strukturen, Enumerationen und primitive Typen (mit Ausnahme von `String`) Werttypen sind, erben alle Verweistypen. Strukturen und den primitiven Typen erben von der Verweistyp `System.ValueType`, erbt von `Object`. Aufgelistete Typen erben von der Verweistyp `System.Enum`, erbt von `System.ValueType`.

### <a name="nullable-value-types"></a>Auf NULL festlegbare Werttypen

Für Werttypen ein `?` Modifizierer kann hinzugefügt werden, um einen Namen zur Darstellung der *auf NULL festlegbare* Version des betreffenden Typs.

```antlr
NullableTypeName
    : NonArrayTypeName '?'
    ;

NullableNameModifier
    : '?'
    ;
```

NULL-Werte zulassen, kann die gleichen Werte wie die nicht auf NULL festlegbare Version des Typs sowie den null-Wert enthalten. Daher ist es bei einem auf NULL festlegbaren Werttyp zuweisen von `Nothing` auf eine Variable des Typs legt den Wert der Variablen, die null-Wert, nicht den Wert des Werttyps. Zum Beispiel:

```vb
Dim x As Integer = Nothing
Dim y As Integer? = Nothing

' Prints zero
Console.WriteLine(x)
' Prints nothing (because the value of y is the null value)
Console.WriteLine(y)
```

Auch kann eine Variable deklariert werden, der ein Werttyp sein, einen nullable-Typ-Modifizierer in den Namen der Variablen platzieren. Aus Gründen der Übersichtlichkeit ist es nicht zulässig, einen nullable-Typ-Modifizierer auf einen Variablennamen und einen Typnamen in der gleichen Deklaration haben. Da auf NULL festlegbare Typen implementiert werden, mit dem Typ `System.Nullable(Of T)`, den Typ `T?` ist ein Synonym für den Typ `System.Nullable(Of T)`, und die beiden Namen austauschbar verwendet werden können. Die `?` Modifizierer kann nicht auf einen Typ, der bereits auf NULL festlegbar ist platziert werden; daher ist es nicht möglich, den Typ deklarieren, `Integer??` oder `System.Nullable(Of Integer)?`.

Ein Werttyp `T?` verfügt über die Member der `System.Nullable(Of T)` sowie Operatoren oder Konvertierungen *angehoben* aus dem zugrunde liegenden Typ `T` in den Typ `T?`. Heben die Kopien-Operatoren und Konvertierungen von den zugrunde liegenden Typ, in den meisten Fällen auf NULL festlegbare Werttypen für nicht auf NULL festlegbare Werttypen ersetzen. Auf diese Weise können viele der gleichen Konvertierungen und Vorgänge, die für gelten `T` zuweisen `T?` ebenfalls.


## <a name="interface-implementation"></a>Schnittstellenimplementierung

Struktur und Klassendeklarationen können deklarieren, dass sie einen Satz von Schnittstellentypen über einen oder mehrere implementieren `Implements` Klauseln.

```antlr
TypeImplementsClause
    : 'Implements' TypeImplements StatementTerminator
    ;

TypeImplements
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

Alle Typen im angegebenen die `Implements` Klausel Schnittstellen sein, und der Typ muss alle Member der Schnittstellen implementieren. Zum Beispiel:

```vb
Interface ICloneable
    Function Clone() As Object
End Interface 

Interface IComparable
    Function CompareTo(other As Object) As Integer
End Interface 

Structure ListEntry
    Implements ICloneable, IComparable

    ...

    Public Function Clone() As Object Implements ICloneable.Clone
        ...
    End Function 

    Public Function CompareTo(other As Object) As Integer _
        Implements IComparable.CompareTo
        ...
    End Function 
End Structure
```

Ein Typ, der eine Schnittstelle, auch implizit implementiert implementiert alle Schnittstellen der Schnittstelle. Dies gilt auch, wenn alle Schnittstellen in der Typ nicht explizit aufgelistet werden die `Implements` Klausel. In diesem Beispiel die `TextBox` Struktur implementiert beide `IControl` und `ITextBox`.

```vb
Interface IControl
    Sub Paint()
End Interface 

Interface ITextBox
    Inherits IControl

    Sub SetText(text As String)
End Interface 

Structure TextBox
    Implements ITextBox

    ...

    Public Sub Paint() Implements ITextBox.Paint
        ...
    End Sub

    Public Sub SetText(text As String) Implements ITextBox.SetText
        ...
    End Sub
End Structure
```

Deklariert, dass ein Typ eine Schnittstelle, an und für sich implementiert ist nichts im Deklarationsbereich des Typs nicht deklarieren. Daher ist es zulässig, zwei Schnittstellen mit einer Methode mit dem gleichen Namen zu implementieren.

Typen können nicht allein einen Typparameter implementieren, obwohl er die Typparameter beinhalten kann, die im Gültigkeitsbereich befinden.

```vb
Class C1(Of V)
    Implements V  ' Error, can't implement type parameter directly
    Implements IEnumerable(Of V)  ' OK, not directly implementing

    ...
End Class
```

Generische Schnittstellen können mit verschiedenen Typargumenten implementiert mehrmals sein. Ein generischer Typ kann nicht jedoch eine generische Schnittstelle, die mithilfe eines Typparameters aus, wenn der übergebenen Typparameter (unabhängig von Einschränkungen) mit einer anderen Implementierung dieser Schnittstelle überschneidet implementieren. Zum Beispiel:

```vb
Interface I1(Of T)
End Interface

Class C1
    Implements I1(Of Integer)
    Implements I1(Of Double)    ' OK, no overlap
End Class

Class C2(Of T)
    Implements I1(Of Integer)
    Implements I1(Of T)         ' Error, T could be Integer
End Class
```


## <a name="primitive-types"></a>Primitive Typen

Die *primitive Typen* identifiziert werden, durch die Schlüsselwörter, die Aliase sind für die vordefinierte Typen in der `System` Namespace. Ein primitiver Typ ist nicht vom Typ vollständig zu unterscheiden sie Aliase: das reservierte Wort `Byte` ist der gleiche wie das Schreiben von `System.Byte`. Primitive Typen sind, auch bekannt als *systeminterne Typen*.

```antlr
PrimitiveTypeName
    : NumericTypeName
    | 'Boolean'
    | 'Date'
    | 'Char'
    | 'String'
    ;

NumericTypeName
    : IntegralTypeName
    | FloatingPointTypeName
    | 'Decimal'
    ;

IntegralTypeName
    : 'Byte' | 'SByte' | 'UShort' | 'Short' | 'UInteger'
    | 'Integer' | 'ULong' | 'Long'
    ;

FloatingPointTypeName
    : 'Single' | 'Double'
    ;
```


Da ein primitiver Typ Aliase regulären-Typ, verfügt über alle primitiver Typ Member aus. Z. B. `Integer` wurde im deklarierten Member `System.Int32`. Literale können als Instanzen von den entsprechenden Typen behandelt werden.

* Die primitiven Typen unterscheiden sich von anderen Strukturtypen, darin, dass sie bestimmte zusätzliche Vorgänge zulassen:

* Primitiven Typen können die Werte durch Schreiben von Literalen erstellt werden. Z. B. `123I` ist ein Literal vom Typ `Integer`.

* Es ist möglich, zum Deklarieren von Konstanten der primitiven Typen.

* Bei den Operanden eines Ausdrucks Konstanten für alle primitiven Typ handelt, ist es möglich, dass der Compiler den Ausdruck zur Kompilierzeit ausgewertet werden soll. Ein solcher Ausdruck wird als ein konstanter Ausdruck bezeichnet.

Visual Basic definiert die folgenden primitiven Typen:

* Die ganzzahligen Werttypen `Byte` (1-Byte-Ganzzahl ohne Vorzeichen), `SByte` (1-Byte-Ganzzahl mit Vorzeichen), `UShort` (2-Byte-Ganzzahl ohne Vorzeichen), `Short` (2-Byte-Ganzzahl mit Vorzeichen), `UInteger` (4-Byte-Ganzzahl ohne Vorzeichen), `Integer` () 4-Byte-Ganzzahl mit Vorzeichen), `ULong` (8-Byte-Ganzzahl ohne Vorzeichen), und `Long` (8-Byte-Ganzzahl mit Vorzeichen). Diese Typen zuordnen `System.Byte`, `System.SByte`, `System.UInt16`, `System.Int16`, `System.UInt32`, `System.Int32`, `System.UInt64` und `System.Int64`bzw. Der Standardwert, der ein ganzzahliger Typ entspricht dem Literal `0`.

* Die Gleitkommazahl Werttypen `Single` (4-Byte-Gleitkommazahl) und `Double` (8-Byte-Gleitkommazahl). Diese Typen zuordnen `System.Single` und `System.Double`bzw. Der Standardwert eines Gleitkommatyps ist gleichbedeutend mit dem Literal `0`.

* Die `Decimal` Typ (16-Byte-decimal-Wert), der zuordnet `System.Decimal`. Der Standardwert von Decimal entspricht dem Literal `0D`.

* Die `Boolean` Werttyp, der einen Wahrheitswert, der in der Regel das Ergebnis eines relationalen oder logischen Vorgangs darstellt. Das Literal weist den Typ `System.Boolean`. Der Standardwert der `Boolean` Typ entspricht dem Literal `False`.

* Die `Date` Werttyp dar, die darstellt, ein Datum und/oder eine Uhrzeit aus, und ordnet `System.DateTime`. Der Standardwert der `Date` Typ entspricht dem Literal `# 01/01/0001 12:00:00AM #`.

* Die `Char` Werttyp, der ein einzelnes Unicodezeichen darstellt, und ordnet `System.Char`. Der Standardwert der `Char` Typs entspricht der Konstante Ausdruck `ChrW(0)`.

* Die `String` verweisen auf Typ, der eine Sequenz von Unicode-Zeichen darstellt, und ordnet `System.String`. Der Standardwert der `String` Typ ist ein null-Wert.



## <a name="enumerations"></a>Enumerationen

*Enumerationen* sind Werttypen, die von erben `System.Enum` und stellen Sie eine Gruppe von Werten eines primitiven Ganzzahltypen symbolisch dar.

```antlr
EnumDeclaration
    : Attributes? TypeModifier* 'Enum' Identifier
      ( 'As' NonArrayTypeName )? StatementTerminator
      EnumMemberDeclaration+
      'End' 'Enum' StatementTerminator
    ;
```

Für einen Enumerationstyp `E`, der Standardwert ist der Wert, der durch den Ausdruck erzeugte `CType(0, E)`.

Der zugrunde liegende Typ einer Enumeration muss ein ganzzahliger Typ sein, der alle in der Enumeration definierten Enumeratorwerte darstellen können. Wenn ein zugrunde liegender Typ angegeben ist, muss er `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, mindestens eine ihrer entsprechenden Typen in der `System` Namespace. Wenn kein zugrunde liegender Typ explizit angegeben wird, wird der Standardwert ist `Integer`.

Das folgende Beispiel deklariert eine Enumeration mit zugrunde liegender Typ `Long`:

```vb
Enum Color As Long
    Red
    Green
    Blue
End Enum
```

Ein Entwickler kann auswählen, einen zugrunde liegenden Typ verwenden `Long`, wie im Beispiel verwenden, um die Verwendung von Werten zu ermöglichen, die im Bereich von `Long`, aber nicht in den Bereich der `Integer`, oder um diese Option für die Zukunft zu erhalten.


### <a name="enumeration-members"></a>Enumerationsmember

Die Member einer Enumeration sind die Enumerationswerte, die in der Enumeration deklariert und die von der-Klasse geerbten Member `System.Enum`.

Der Bereich eines Enumerationsmembers ist der Enumeration-Deklaration-Text. Dies bedeutet, dass außerhalb der Enumerationsdeklaration einer ein Enumerationsmember immer qualifiziert werden muss (es sei denn, die der Typ explizit in einen Namespace ein Namespace importieren importiert wird).

Reihenfolge der Deklaration für die Enumerationsmemberdeklarationen spielt, wenn die Werte der Konstanten Ausdruck ausgelassen werden. Enumerationsmember verfügen implizit über `Public` nur zugreifen, die keine Zugriffsmodifizierer für Member Enumerationsdeklarationen zulässig sind.

```antlr
EnumMemberDeclaration
    : Attributes? Identifier ( Equals ConstantExpression )? StatementTerminator
    ;
```

### <a name="enumeration-values"></a>Enumerationswerte

Konstanten, die als den zugrunde liegenden Enumerationstyp typisiert, und sie können angezeigt werden, wo die Konstanten erforderlich sind, werden die aufgelisteten Werte in einer Memberliste der Enumeration deklariert. Die Definition von Enumerationsmembern mit `=` gibt dem zugeordnete Element, das den Wert, der durch den Konstantenausdruck angegeben. Der Konstante Ausdruck muss zu einem ganzzahligen Typ, der implizit in den zugrunde liegenden Typ ausgewertet werden und muss innerhalb des Bereichs von Werten, die durch den zugrunde liegenden Typ dargestellt werden kann. Im folgende Beispiel wird Fehler, da die Konstanten Werte `1.5`, `2.3`, und `3.3` sind nicht implizit in den zugrunde liegenden ganzzahligen Typ `Long` mit strict-Semantik.

```vb
Option Strict On

Enum Color As Long
    Red = 1.5
    Green = 2.3
    Blue = 3.3
End Enum
```

Mehrere Enumerationsmember möglicherweise derselben Wert zugeordneten, freigeben, wie unten dargestellt:

```vb
Enum Color
    Red
    Green
    Blue
    Max = Blue
End Enum
```

Das Beispiel zeigt eine Enumeration, die zwei--Enumerationsmembern `Blue` und `Max` –, dass derselbe Wert zugewiesen haben.

Wenn die erste Enumerator-Wert-Definition in der Enumeration keinen Initialisierer aufweist, ist der Wert der entsprechenden Konstanten `0`. Eine aufzählungsdefinition-Wert ohne einen Initialisierer gibt dem Enumerator den Wert abgerufen, indem Sie den Wert von den vorherigen Enumerationswert von `1`. Dieser höhere Wert muss innerhalb des Bereichs der Werte sein, die von den zugrunde liegenden Typ dargestellt werden können.

```vb
Enum Color
    Red
    Green = 10
    Blue
End Enum 

Module Test
    Sub Main()
        Console.WriteLine(StringFromColor(Color.Red))
        Console.WriteLine(StringFromColor(Color.Green))
        Console.WriteLine(StringFromColor(Color.Blue))
    End Sub

    Function StringFromColor(c As Color) As String
        Select Case c
            Case Color.Red
                Return String.Format("Red = " & CInt(c))

            Case Color.Green
                Return String.Format("Green = " & CInt(c))

            Case Color.Blue
                Return String.Format("Blue = " & CInt(c))

            Case Else
                Return "Invalid color"
        End Select
    End Function
End Module
```

Das obige Beispiel gibt die Enumerationswerte und die zugehörigen Werte. Ausgabe:

```
Red = 0
Green = 10
Blue = 11
```

Die Gründe für die Werte lauten wie folgt aus:

* Der Enumerationswert `Red` wird automatisch der Wert zugewiesen `0` (da es keinen Initialisierer aufweist und das erste Element der Enumeration-Wert ist).

* Der Enumerationswert `Green` erhält den Wert explizit `10`.

* Der Enumerationswert `Blue` wird automatisch der Wert eins größer ist als der Enumerationswert, der textlich vorausgehenden zugewiesen.

Der Konstante Ausdruck kann nicht direkt oder indirekt verwenden Sie den Wert von ihrem eigenen Wert zugeordnete Enumeration (d. h. eine Zirkularität, in der Konstante Ausdruck ist nicht zulässig). Im folgende Beispiel ist ungültig. da die Deklarationen der `A` und `B` sind zirkulär.

```vb
Enum Circular
    A = B
    B
End Enum
```

`A` hängt von `B` explizit und `B` hängt `A` implizit.

## <a name="classes"></a>Klassen

Ein *Klasse* ist eine Datenstruktur, die Datenmember (Konstanten, Variablen und Ereignisse), Funktionsmember (Methoden, Eigenschaften, Indexer, Operatoren und Konstruktoren) und geschachtelte Typen enthalten können. Klassen sind Verweistypen.

```antlr
ClassDeclaration
    : Attributes? ClassModifier* 'Class' Identifier TypeParameterList? StatementTerminator
      ClassBase?
      TypeImplementsClause*
      ClassMemberDeclaration*
      'End' 'Class' StatementTerminator
    ;

ClassModifier
    : TypeModifier
    | 'MustInherit'
    | 'NotInheritable'
    | 'Partial'
    ;
```

Das folgende Beispiel zeigt eine Klasse, die jede Art von Member enthält:

```vb
Class AClass
    Public Sub New()
        Console.WriteLine("Constructor")
    End Sub

    Public Sub New(value As Integer)
        MyVariable = value
        Console.WriteLine("Constructor")
    End Sub

    Public Const MyConst As Integer = 12
    Public MyVariable As Integer = 34

    Public Sub MyMethod()
        Console.WriteLine("MyClass.MyMethod")
    End Sub

    Public Property MyProperty() As Integer
        Get
            Return MyVariable
        End Get

        Set (value As Integer)
            MyVariable = value
        End Set
    End Property

    Default Public Property Item(index As Integer) As Integer
        Get
            Return 0
        End Get

        Set (value As Integer)
            Console.WriteLine("Item(" & index & ") = " & value)
        End Set
    End Property

    Public Event MyEvent()

    Friend Class MyNestedClass
    End Class 
End Class
```

Das folgende Beispiel zeigt die Verwendung dieser Member:

```vb
Module Test

    ' Event usage.
    Dim WithEvents aInstance As AClass

    Sub Main()
        ' Constructor usage.
        Dim a As AClass = New AClass()
        Dim b As AClass = New AClass(123)

        ' Constant usage.
        Console.WriteLine("MyConst = " & AClass.MyConst)

        ' Variable usage.
        a.MyVariable += 1
        Console.WriteLine("a.MyVariable = " & a.MyVariable)

        ' Method usage.
        a.MyMethod()

        ' Property usage.
        a.MyProperty += 1
        Console.WriteLine("a.MyProperty = " & a.MyProperty)
        a(1) = 1

        ' Event usage.
        aInstance = a
    End Sub 

    Sub MyHandler() Handles aInstance.MyEvent
        Console.WriteLine("Test.MyHandler")
    End Sub 
End Module
```

Es gibt zwei mandantenklassen geltenden schemaanpassungen Modifizierern `MustInherit` und `NotInheritable`. Es ist ungültig. Geben Sie beide.


### <a name="class-base-specification"></a>Die Basisspezifikationen Klasse

Eine Klassendeklaration kann es sich um eine Spezifikation Basistyp enthalten, die die direkte Basistyp der Klasse definiert.

```antlr
ClassBase
    : 'Inherits' NonArrayTypeName StatementTerminator
    ;
```

Wenn eine Deklaration der Klasse keinen expliziten Basistyp hat, wird der direkte Basistyp implizit ist `Object`. Zum Beispiel:

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class
```

Basistypen darf nicht Typparameter in ihren eigenen sein, obwohl er die Typparameter beinhalten kann, die im Gültigkeitsbereich befinden.

```vb
Class C1(Of V) 
End Class

Class C2(Of V)
    Inherits V    ' Error, type parameter used as base class 
End Class

Class C3(Of V)
    Inherits C1(Of V)    ' OK: not directly inheriting from V.
End Class
```

Klassen können nur leiten sich von `Object` und Klassen. Ist ungültig für eine Klasse für die Ableitung `System.ValueType`, `System.Enum`, `System.Array`, `System.MulticastDelegate` oder `System.Delegate`. Eine generische Klasse kann nicht abgeleitet werden, von `System.Attribute` oder aus einer Klasse, die von ihr abgeleitet wird.

Jede Klasse verfügt über genau eine direkte Basisklasse und Zirkularität in Ableitung ist nicht zulässig. Es ist nicht möglich, für die Ableitung einer `NotInheritable` -Klasse und die Zugriffsdomäne von der Basisklasse müssen identisch oder eine Obermenge der Zugriffsdomäne von der Klasse selbst sein.


### <a name="class-members"></a>Klassenmember

Die Member einer Klasse bestehen aus der Elemente, die durch die Klassenmember-Deklarationen eingeführt und die von der direkten Basisklasse geerbten Member.

```antlr
ClassMemberDeclaration
    : NonModuleDeclaration
    | EventMemberDeclaration
    | VariableMemberDeclaration
    | ConstantMemberDeclaration
    | MethodMemberDeclaration
    | PropertyMemberDeclaration
    | ConstructorMemberDeclaration
    | OperatorDeclaration
    ;
```

Eine Klassendeklaration für das Element möglicherweise `Public`, `Protected`, `Friend`, `Protected Friend`, oder `Private` Zugriff. Wenn Sie eine Klassendeklaration für das Element keine Zugriffsmodifizierer enthalten, die Deklaration standardmäßig `Public` Zugriff, sofern dies nicht die Deklaration einer Variablen; in diesem Fall wird standardmäßig `Private` Zugriff.

Der Bereich eines Klassenmembers ist der Text einer Klasse in dem die Memberdeklaration erfolgt, sowie der Einschränkungsliste dieser Klasse, (wenn er generisch und verfügt über Einschränkungen). Wenn der Member verfügt `Friend` Zugriff, der Bereich erweitert, in den Klassentext von jeder abgeleiteten Klasse in der gleichen Anwendung oder einer beliebigen Assembly, die erteilt wurden `Friend` Zugriff, und wenn der Member verfügt `Public`, `Protected`, oder `Protected Friend` den Zugriff auf ihren Bereich in den Klassentext einer beliebigen abgeleiteten Klasse in jedem Programm erweitert.


## <a name="structures"></a>Strukturen

*Strukturen* sind Werttypen, die von erben `System.ValueType`. Strukturen sind ähnlich wie Klassen, Datenstrukturen dar, die Datenmember und Funktionsmember enthalten können. Im Gegensatz zu Klassen erfordern Strukturen jedoch keine Heapzuordnung.

```antlr
StructureDeclaration
    : Attributes? StructureModifier* 'Structure' Identifier
      TypeParameterList? StatementTerminator
      TypeImplementsClause*
      StructMemberDeclaration*
      'End' 'Structure' StatementTerminator
    ;

StructureModifier
    : TypeModifier
    | 'Partial'
    ;
```

Bei Klassen ist es möglich, dass zwei Variablen auf dasselbe Objekt verweisen, und so können Vorgänge auf eine Variable auf das Objekt, das die andere Variable verweist auswirken. Mit Strukturen besitzt jede Variable eine eigene Kopie einer anderen`Shared` Daten, daher es nicht möglich, dass Vorgänge für mindestens eine der anderen zu beeinflussen ist, wie das folgende Beispiel veranschaulicht:

```vb
Structure Point
    Public x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

Wenn die obige Deklaration der folgende Code gibt den Wert `10`:

```vb
Module Test
    Sub Main()
        Dim a As New Point(10, 10)
        Dim b As Point = a

        a.x = 100
        Console.WriteLine(b.x)
    End Sub
End Module
```

Die Zuweisung von `a` zu `b` erstellt eine Kopie des Werts und `b` ist daher nicht betroffen, durch die Zuweisung zu `a.x`. Hatte `Point` wurde stattdessen als eine Klasse deklariert, ist die Ausgabe wäre `100` da `a` und `b` würde das gleiche Objekt verweisen.


### <a name="structure-members"></a>Strukturmember

Die Member einer Struktur sind die Elemente, die von den Deklarationen der Strukturmember eingeführt und die Mitglieder von geerbt `System.ValueType`.

```antlr
StructMemberDeclaration
    : NonModuleDeclaration
    | VariableMemberDeclaration
    | ConstantMemberDeclaration
    | EventMemberDeclaration
    | MethodMemberDeclaration
    | PropertyMemberDeclaration
    | ConstructorMemberDeclaration
    | OperatorDeclaration
    ;
```

Jede Struktur weist implizit einen `Public` parameterlosen Instanzenkonstruktor, die den Standardwert der Struktur erzeugt. Daher ist es nicht möglich, für eine Typdeklaration Struktur, um einen parameterlosen Instanzenkonstruktor zu deklarieren. Ein Strukturtyp ist, darf jedoch deklarieren *parametrisierte* Instanzkonstruktoren, wie im folgenden Beispiel gezeigt:

```vb
Structure Point
    Private x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

Wenn die obige Deklaration, die folgenden Anweisungen erstellen eine `Point` mit `x` und `y` auf 0 (null) initialisiert.

```vb
Dim p1 As Point = New Point()
Dim p2 As Point = New Point(0, 0)
```

Da Strukturen direkt ihre Feld Werte (statt Verweise auf diese Werte) enthalten, können nicht in Strukturen Felder enthalten, die direkt oder indirekt auf sich selbst verweisen. Der folgende Code ist beispielsweise ungültig:

```vb
Structure S1
    Dim f1 As S2
End Structure

Structure S2
    ' This would require S1 to contain itself.
    Dim f1 As S1
End Structure
```

Deklaration von Strukturmembern möglicherweise in der Regel nur `Public`, `Friend`, oder `Private` Zugriff, aber beim Überschreiben von geerbten Member `Object`, `Protected` und `Protected Friend` Zugriff kann auch verwendet werden. Wenn Sie eine Deklaration von Strukturmembern keine Zugriffsmodifizierer enthalten, die Deklaration standardmäßig `Public` Zugriff. Der Bereich eines Elements, das von einer Struktur deklariert ist Rumpf der Struktur, in dem sich die Deklaration erfolgt, sowie die Einschränkungen dieser Struktur, (Wenn sie generische wurde und Einschränkungen haben).


## <a name="standard-modules"></a>Standardmodule

Ein *Standardmodul* ist ein Typ, dessen Member handelt es sich implizit `Shared` Gültigkeitsbereich zu Deklarationsabschnitt des enthaltenden Namespace des standard-Moduls, statt die standardmäßige Moduldeklaration selbst. Standard-Module können nie instanziiert werden. Es ist ein Fehler auf eine Variable eines Typs für die standard-Modul zu deklarieren.

```antlr
ModuleDeclaration
    : Attributes? TypeModifier* 'Module' Identifier StatementTerminator
      ModuleMemberDeclaration*
      'End' 'Module' StatementTerminator
    ;
```

Ein Mitglied in einem Standardmodul verfügt über zwei vollqualifizierte Namen, eine ohne den Modulnamen des standard-und eine mit den Namen des standard-Moduls. Mehr als eine standard-Modul in einem Namespace kann es sich um ein Element mit einem bestimmten Namen definieren; nicht gekennzeichnete Verweise auf den Namen außerhalb eines der Module sind mehrdeutig. Zum Beispiel:

```vb
Namespace N1
    Module M1
        Sub S1()
        End Sub

        Sub S2()
        End Sub
    End Module

    Module M2
        Sub S2()
        End Sub
    End Module

    Module M3
        Sub Main()
            S1()       ' Valid: Calls N1.M1.S1.
            N1.S1()    ' Valid: Calls N1.M1.S1.
            S2()       ' Not valid: ambiguous.
            N1.S2()    ' Not valid: ambiguous.
            N1.M2.S2() ' Valid: Calls N1.M2.S2.
        End Sub
    End Module
End Namespace
```

Ein Modul kann nur in einem Namespace deklariert werden und darf nicht in einen anderen Typ geschachtelt sein. Standardmodule dürfen keine Schnittstellen implementieren, die sie implizit abgeleitet `Object`, und sie müssen nur `Shared` Konstruktoren.


### <a name="standard-module-members"></a>Standard Modulelemente

Die Member eines standard-Moduls sind die Elemente, die durch die Memberdeklarationen eingeführt und die Mitglieder von geerbt `Object`. Standardmodule möglicherweise jede Art von Member, mit Ausnahme der Instanzkonstruktoren. Alle Standardmodul Typmember handelt es sich implizit `Shared`.

```antlr
ModuleMemberDeclaration
    : NonModuleDeclaration
    | VariableMemberDeclaration
    | ConstantMemberDeclaration
    | EventMemberDeclaration
    | MethodMemberDeclaration
    | PropertyMemberDeclaration
    | ConstructorMemberDeclaration
    ;
```

Eine Deklaration möglicherweise in der Regel nur `Public`, `Friend`, oder `Private` Zugriff, aber beim Überschreiben von geerbten Member `Object`, `Protected` und `Protected Friend` Zugriffsmodifizierer können angegeben werden. Wenn Sie eine Deklaration keine Zugriffsmodifizierer enthalten, die Deklaration standardmäßig `Public` zugreifen, es sei denn, es sich um eine Variable ist standardmäßig `Private` Zugriff.

Wie bereits erwähnt ist der Gültigkeitsbereich eines standard-Modul die Deklaration mit der standard Moduldeklaration. Mitglieder von geerbt `Object` sind nicht in dieser speziellen Bereichsdefinition; enthalten diese Elemente kein Bereich haben und muss immer mit dem Namen des Moduls qualifiziert werden. Wenn der Member verfügt `Friend` Zugriff, der Bereich erstreckt sich ausschließlich auf Namespacemember im selben Programm oder in Assemblys, die erteilt wurden deklariert `Friend` Zugriff.


## <a name="interfaces"></a>Schnittstellen

*Schnittstellen* sind Referenztypen, die von anderen Typen implementieren, um sicherzustellen, dass sie bestimmte Methoden unterstützen. Eine Schnittstelle wird nie direkt erstellt und verfügt über keine tatsächliche Darstellung – andere Datentypen müssen in einen Schnittstellentyp konvertiert werden. Eine Schnittstelle definiert einen Vertrag. Eine Klasse oder Struktur, die eine Schnittstelle implementiert, muss ihren Vertrag einhalten.

```antlr
InterfaceDeclaration
    : Attributes? TypeModifier* 'Interface' Identifier
      TypeParameterList? StatementTerminator
      InterfaceBase*
      InterfaceMemberDeclaration*
      'End' 'Interface' StatementTerminator
    ;
```


Das folgende Beispiel zeigt eine Schnittstelle, eine Standardeigenschaft enthält `Item`, ein Ereignis `E`, eine Methode `F`, und eine Eigenschaft `P`:

```vb
Interface IExample
    Default Property Item(index As Integer) As String

    Event E()

    Sub F(value As Integer)

    Property P() As String
End Interface
```

Schnittstellen können mehrfache Vererbung nutzen. Im folgenden Beispiel ist die Schnittstelle `IComboBox` erbt von `ITextBox` und `IListBox`:

```vb
Interface IControl
    Sub Paint()
End Interface 

Interface ITextBox
    Inherits IControl

    Sub SetText(text As String)
End Interface 

Interface IListBox
    Inherits IControl

    Sub SetItems(items() As String)
End Interface 

Interface IComboBox
    Inherits ITextBox, IListBox 
End Interface
```

Klassen und Strukturen können mehrere Schnittstellen implementieren. Im folgenden Beispiel die Klasse `EditBox` leitet sich von der Klasse `Control` und implementiert beide `IControl` und `IDataBound`:

```vb
Interface IDataBound
    Sub Bind(b As Binder)
End Interface 

Public Class EditBox
    Inherits Control
    Implements IControl, IDataBound

    Public Sub Paint() Implements IControl.Paint
        ...
    End Sub

    Public Sub Bind(b As Binder) Implements IDataBound.Bind
        ...
    End Sub
End Class
```


### <a name="interface-inheritance"></a>Schnittstellenvererbung

Die Basisschnittstellen einer Schnittstelle sind die explizite Basisschnittstelle und deren Basisschnittstellen. Der Satz von Schnittstellen ist also die vollständige transitiven Abschluss von die explizite Basisschnittstelle, die explizite Basisschnittstelle und So weiter. Wenn eine Schnittstellendeklaration ist keine explizite Schnittstellenmember-Basis, und es ist keine Basisschnittstelle für den Typ Schnittstellen erben nicht von `Object` (obwohl sie über eine erweiterungskonvertierung verfügen `Object`).

```antlr
InterfaceBase
    : 'Inherits' InterfaceBases StatementTerminator
    ;

InterfaceBases
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

Im folgenden Beispiel ist die Basisschnittstelle `IComboBox` sind `IControl`, `ITextBox`, und `IListBox`.

```vb
Interface IControl
    Sub Paint()
End Interface 

Interface ITextBox
    Inherits IControl

    Sub SetText(text As String)
End Interface 

Interface IListBox
    Inherits IControl

    Sub SetItems(items() As String)
End Interface 

Interface IComboBox
    Inherits ITextBox, IListBox 
End Interface
```

Eine Schnittstelle erbt alle Member der Basisschnittstellen. Das heißt, die `IComboBox` obige Schnittstelle erbt Member `SetText` und `SetItems` sowie `Paint`.

Eine Klasse oder Struktur, die eine Schnittstelle, auch implizit implementiert implementiert alle die Basisschnittstellen.

Wenn eine Schnittstelle mehr als einmal in den transitiven Abschluss der Basisschnittstellen angezeigt wird, trägt es nur einmal Member der abgeleiteten Schnittstelle. Ein Typ implementieren, die nur die abgeleitete Schnittstelle implementieren, die Methoden der muss definiert Basisschnittstelle einmal. Im folgenden Beispiel `Paint` nur einmal implementiert werden muss, obwohl die Klasse implementiert `IComboBox` und `IControl`.

```vb
Class ComboBox
    Implements IControl, IComboBox

    Sub SetText(text As String) Implements IComboBox.SetText
    End Sub

    Sub SetItems(items() As String) Implements IComboBox.SetItems
    End Sub

    Sub Print() Implements IComboBox.Paint
    End Sub
End Class
```

Ein `Inherits` Klausel hat keine Auswirkungen auf andere `Inherits` Klauseln. Im folgenden Beispiel `IDerived` muss es sich um den Namen des qualifizieren `INested` mit `IBase`.

```vb
Interface IBase
    Interface INested
        Sub Nested()
    End Interface

    Sub Base()
End Interface

Interface IDerived
    Inherits IBase, INested   ' Error: Must specify IBase.INested.
End Interface
```

Die Zugriffsdomäne von einer Basisschnittstelle muss identisch oder eine Obermenge der Zugriffsdomäne von der Schnittstelle selbst sein.


### <a name="interface-members"></a>Schnittstellenmember

Die Member einer Schnittstelle bestehen die Elemente, die durch die Memberdeklarationen eingeführt und die von der Basisschnittstellen geerbten Member aus.

```antlr
InterfaceMemberDeclaration
    : NonModuleDeclaration
    | InterfaceEventMemberDeclaration
    | InterfaceMethodMemberDeclaration
    | InterfacePropertyMemberDeclaration
    ;
```

Obwohl Schnittstellen, Elemente aus nicht erben `Object`, da jede Klasse oder Struktur, die eine Schnittstelle implementiert von erbt `Object`, die Mitglieder der `Object`, einschließlich Erweiterungsmethoden, gelten als Member einer Schnittstelle und auf einer Schnittstelle aufgerufen werden kann, um direkt ohne eine Umwandlung in `Object`. Zum Beispiel:

```vb
Interface I1
End Interface

Class C1
    Implements I1
End Class

Module Test
    Sub Main()
        Dim i As I1 = New C1()
        Dim h As Integer = i.GetHashCode()
    End Sub
End Module
```

Member einer Schnittstelle mit dem gleichen Namen als Mitglieder der `Object` implizit Schattenkopie `Object` Member. Nur geschachtelte Typen, Methoden, Eigenschaften und Ereignisse können Member einer Schnittstelle sein. Methoden und Eigenschaften können keinen Text enthalten. Schnittstellenmember sind implizit `Public` und können keinen Zugriffsmodifizierer angeben. Der Bereich eines Elements in einer Schnittstelle deklariert ist der Schnittstelle-Text, in dem sich die Deklaration erfolgt, sowie der Einschränkungsliste dieser Schnittstelle, (wenn er generisch und verfügt über Einschränkungen).


## <a name="arrays"></a>Arrays

Ein *Array* ist ein Verweistyp, der Zugriff erfolgt über eine Variable enthält *Indizes* eins mit der Reihenfolge der Variablen im Array entspricht. Wird aufgerufen, die in einem Array enthaltenen Variablen auch der *Elemente* des Arrays muss alle vom selben Typ, und dieser Typ wird aufgerufen, die *Elementtyp* des Arrays.

```antlr
ArrayTypeName
    : NonArrayTypeName ArrayTypeModifiers
    ;

ArrayTypeModifiers
    : ArrayTypeModifier+
    ;

ArrayTypeModifier
    : OpenParenthesis RankList? CloseParenthesis
    ;

RankList
    : Comma*
    ;

ArrayNameModifier
    : ArrayTypeModifiers
    | ArraySizeInitializationModifier
    ;
```

Die Elemente eines Arrays sind vorhanden, wenn eine Arrayinstanz erstellt wird, und Sie sind nicht mehr vorhanden, wenn die Arrayinstanz zerstört wird. Jedes Element eines Arrays ist auf den Standardwert dieses Typs initialisiert. Der Typ `System.Array` ist der Basistyp aller Typen von Arrays und kann nicht instanziiert werden. Jedes Array-Typ erbt die Member deklariert, indem die `System.Array` geben, und es konvertierbar ist (und `Object`). Ein eindimensionales Array mit Elementen `T` außerdem implementiert die Schnittstellen `System.Collections.Generic.IList(Of T)` und `IReadOnlyList(Of T)`; Wenn `T` ist ein Verweistyp, und klicken Sie dann auf das Array vom Typ auch implementiert `IList(Of U)` und `IReadOnlyList(Of U)` für alle `U`, bei dem eine erweiternde Konvertierung von Verweisen auf `T`.

Ein Array hat eine *Rang* , bestimmt die Anzahl der Indizes, die jedes Arrayelement zugeordnet. Der Rang eines Arrays bestimmt die Anzahl der *Dimensionen* des Arrays. Z. B. ein Array mit dem Rang eins ist ein eindimensionales Array bezeichnet, und ein Array mit einem Rang größer als 1 wird ein mehrdimensionales Array bezeichnet.

Das folgende Beispiel erstellt ein eindimensionales Array von ganzzahligen Werten, die Elemente des Arrays initialisiert und gibt dann die einzelnen Elemente:

```vb
Module Test
    Sub Main()
        Dim arr(5) As Integer
        Dim i As Integer

        For i = 0 To arr.Length - 1
            arr(i) = i * i
        Next i

        For i = 0 To arr.Length - 1
            Console.WriteLine("arr(" & i & ") = " & arr(i))
        Next i
    End Sub
End Module
```

Das Programm gibt Folgendes aus:

```
arr(0) = 0
arr(1) = 1
arr(2) = 4
arr(3) = 9
arr(4) = 16
arr(5) = 25
```

Jeder Dimension eines Arrays verfügt über eine zugeordnete Länge. Längen der Dimension sind nicht Teil des Typs des Arrays, aber eingerichtet, wenn eine Instanz des Arraytyps zur Laufzeit erstellt wird. Die Länge einer Dimension bestimmt des gültigen Bereichs von Indizes für diese Dimension: für eine Dimension der Länge `N`, Indizes reichen von 0 bis `N-1`. Wenn eine Dimension der Länge 0 (null) ist, sind keine gültigen Indizes für diese Dimension. Die Gesamtanzahl der Elemente in einem Array ist das Produkt der Längen der in jeder Dimension im Array. Wenn eine der Dimensionen eines Arrays eine Länge von 0 (null), hört das Array leer sein. Der Elementtyp eines Arrays kann einen beliebigen Typ sein.

Arraytypen werden durch Hinzufügen eines vorhandenen Typnamens Modifizierer angegeben. Der Modifizierer besteht aus eine linke Klammer, die einen Satz von 0 (null) oder mehrere Kommas und eine schließende Klammer. Der geänderte Typ ist der Elementtyp des Arrays, und die Anzahl der Dimensionen ist die Anzahl von Kommas plus eins. Wenn mehr als ein Modifizierer angegeben ist, ist der Elementtyp des Arrays ein Array. Die Modifizierer werden mit dem am weitesten links stehende Modifizierer, der das äußerste Array von links nach rechts gelesen. Im Beispiel

```vb
Module Test
    Dim arr As Integer(,)(,,)()
End Module
```

der Elementtyp der `arr` wird ein zweidimensionales Array von dreidimensionalen Arrays eines eindimensionalen Arrays von `Integer`.

Eine Variable kann auch ein Arraytyp sein, einen Arraytypmodifizierer oder ein Array-Größe Initialisierung-Modifizierer in den Namen der Variablen platzieren deklariert werden. In diesem Fall den Elementtyp des Arrays ist der Typ, der in der Deklaration angegeben, und die Dimensionen des Arrays werden durch den Variablennamenmodifizierer bestimmt. Aus Gründen der Übersichtlichkeit ist es nicht zulässig, einen Array der Modifizierer auf einen Variablennamen und einen Typnamen in der gleichen Deklaration haben.

Das folgende Beispiel zeigt eine Vielzahl von Deklarationen von lokalen Variablen, mit denen Arraytypen mit `Integer` als Typ des Elements:

```vb
Module Test
    Sub Main()
        Dim a1() As Integer    ' Declares 1-dimensional array of integers.
        Dim a2(,) As Integer   ' Declares 2-dimensional array of integers.
        Dim a3(,,) As Integer  ' Declares 3-dimensional array of integers.

        Dim a4 As Integer()    ' Declares 1-dimensional array of integers.
        Dim a5 As Integer(,)   ' Declares 2-dimensional array of integers.
        Dim a6 As Integer(,,)  ' Declares 3-dimensional array of integers.

        ' Declare 1-dimensional array of 2-dimensional arrays of integers 
        Dim a7()(,) As Integer
        ' Declare 2-dimensional array of 1-dimensional arrays of integers.
        Dim a8(,)() As Integer

        Dim a9() As Integer() ' Not allowed.
    End Sub
End Module
```

Ein Array der Namen Modifizierer erstreckt sich auf alle Sätze mit Klammern, die folgen. Dies bedeutet, dass in den Situationen, in dem ein Satz von Argumenten in Klammern eingeschlossenes hinter dem Namen des Typs zulässig ist, es nicht möglich, die Argumente für einen Typnamen des Arrays anzugeben. Zum Beispiel:

```vb
Module Test
    Sub Main()
        ' This calls the Integer constructor.
        Dim x As New Integer(3)

        ' This declares a variable of Integer().
        Dim y As Integer()

        ' This gives an error.
        ' Array sizes can not be specified in a type name.
        Dim z As Integer()(3)
    End Sub
End Module
```

Im letzten Fall `(3)` als Teil des Typnamens und einen Satz von Konstruktorargumenten interpretiert wird.


## <a name="delegates"></a>Delegaten

Ein *Delegieren* ist ein Verweistyp, der auf verweist eine `Shared` -Methode eines Typs oder um eine Instanzmethode eines Objekts.

```antlr
DelegateDeclaration
    : Attributes? TypeModifier* 'Delegate' MethodSignature StatementTerminator
    ;

MethodSignature
    : SubSignature
    | FunctionSignature
    ;
```

 Am nächsten eines Delegaten in anderen Sprachen entspricht ein Funktionszeiger, jedoch nur auf ein Funktionszeiger verweisen kann `Shared` -Funktionen Delegaten können sowohl verweisen `Shared` und Instanzmethoden. Im letzteren Fall speichert der Delegaten nicht nur einen Verweis auf den Einstiegspunkt der Methode, sondern auch einen Verweis auf die Objektinstanz mit dem zum Aufrufen der Methode.

Die Delegatdeklaration möglicherweise kein `Handles` -Klausel einer `Implements` -Klausel, einer Methode verwendet, oder ein `End` zu erstellen. Die Liste der Parameter in der Delegatdeklaration überein darf keine `Optional` oder `ParamArray` Parameter. Die Zugriffsdomäne von den Rückgabetyp und Parametertypen muss identisch oder eine Obermenge der Zugriffsdomäne des Delegaten selbst sein.

Die Member eines Delegaten sind die Elemente, die von der Klasse geerbt `System.Delegate`. Ein Delegat definiert auch die folgenden Methoden:

* Ein Konstruktor, zwei Parameter: eine vom Typ akzeptiert `Object` und eine vom Typ `System.IntPtr`.

* Ein `Invoke` -Methode, die die gleiche Signatur wie der Delegat hat.

* Ein `BeginInvoke` -Methode, deren Signatur die Signatur des Delegaten mit drei Unterschiede ist. Zunächst wird der Rückgabetyp geändert, um `System.IAsyncResult`. Zweitens: zwei zusätzliche Parameter an das Ende der Parameterliste hinzugefügt werden: der erste Typ `System.AsyncCallback` und der zweite Typ `Object`. Und schließlich alle `ByRef` Parameter geändert werden, um `ByVal`.

* Ein `EndInvoke` -Methode, deren Rückgabetyp identisch mit den Delegaten ist. Die Parameter der Methode sind nur die Delegatparameter genau, `ByRef` -Parameter, in der gleichen Reihenfolge, die sie in der Signatur des Delegaten auftreten.  Zusätzlich zu diesen Parametern, besteht ein weiterer Parameter vom Typ `System.IAsyncResult` am Ende der Parameterliste.

Es gibt drei Schritte im definieren und Verwenden von Delegaten: Deklaration, Instanziierung und das aufrufen.

Delegaten werden mithilfe der Syntax zum Deklarieren von Delegaten deklariert. Das folgende Beispiel deklariert einen Delegaten, mit dem Namen `SimpleDelegate` , die keine Argumente akzeptiert:

```vb
Delegate Sub SimpleDelegate()
```

Im nächste Beispiel erstellt eine `SimpleDelegate` Instanz, und unmittelbar danach aufgerufen wird:

```vb
Module Test
    Sub F()
        System.Console.WriteLine("Test.F")
    End Sub 

    Sub Main()
        Dim d As SimpleDelegate = AddressOf F
        d()
    End Sub 
End Module
```

Es ist nicht viel Sinn Instanziieren von Delegaten für eine Methode, und klicken Sie dann sofort über den Delegaten aufrufen, da es einfacher wäre, die Methode direkt aufzurufen. Delegaten wird deutlich, wenn ihre Anonymität verwendet wird. Das folgende Beispiel zeigt eine `MultiCall` -Methode, die ruft wiederholt eine `SimpleDelegate` Instanz:

```vb
Sub MultiCall(d As SimpleDelegate, count As Integer)
    Dim i As Integer

    For i = 0 To count - 1
        d()
    Next i
End Sub
```

Es ist unwichtig, die `MultiCall` Methode, welche die Zielmethode für die `SimpleDelegate` ist, welche Zugriff auf diese Methode verfügbar ist und angibt, ob die Methode ist `Shared` oder nicht. Wichtig ist, dass die Signatur der Zielmethode kompatibel mit `SimpleDelegate`.


## <a name="partial-types"></a>Partielle Typen

Klasse und Struktur Deklarationen möglich *teilweise* Deklarationen. Eine partielle Deklaration kann oder den deklarierten Typ innerhalb der Deklaration möglicherweise nicht vollständig beschrieben. Stattdessen kann die Deklaration des Typs auf mehreren partiellen Deklarationen innerhalb des Programms verteilt werden; Partielle Typen können nicht über Programms hinweg deklariert werden. Eine partielle Deklaration gibt an, die `Partial` Modifizierer in der Deklaration. Klicken Sie dann werden allen anderen Deklarationen in der Anwendung für einen Typ mit dem gleichen vollqualifizierten Namen zusammen mit der partiellen Deklaration zur Kompilierzeit auf eine einzelne Typdeklaration bilden zusammengeführt. Der folgende Code deklariert beispielsweise eine einzelne Klasse `Test` mit Mitgliedern `Test.C1` und `Test.C2`.

a.vb:

```vb
Public Partial Class Test
    Public Sub S1()
    End Sub
End Class
```

b.vb:

```vb
Public Class Test
    Public Sub S2()
    End Sub
End Class
```

Wenn partielle Typdeklarationen kombinieren möchten, müssen mindestens eine der Deklarationen einer `Partial` Modifizierer verwenden, andernfalls ein Fehler während der Kompilierung führt.

__Beachten Sie.__ Obwohl es möglich ist, geben Sie `Partial` auf nur eine Deklaration auf viele partielle Deklarationen, es ist besser, ihn auf allen partiellen Deklarationen angeben. In der Situation, in dem eine partielle Deklaration ist sichtbar, aber ein oder mehr partielle Deklarationen sind (z. B. im Fall von Tool generierten Code erweitern) ausgeblendet, werden, es ist akzeptabel, lassen die `Partial` Modifizierer aus der sichtbare Deklaration Geben sie jedoch auf die Ausgeblendete Deklarationen.

Nur Klassen und Strukturen können mithilfe von partiellen Deklarationen deklariert werden. Die arität eines Typs gilt beim Zuordnen von partiellen Deklarationen zusammen: zwei Klassen mit demselben Namen, aber unterschiedliche Anzahl von Typparametern werden nicht als partiellen Deklarationen desselben Typs. Partielle Deklarationen können Attribute angeben, Modifizierer, Klasse `Inherits` Anweisung oder `Implements` Anweisung. Zum Zeitpunkt der Kompilierung werden alle Teile der partiellen Deklarationen kombiniert und als Teil der Typdeklaration verwendet. Treten Konflikte zwischen den Attributen, Modifizierer, Basisklassen, Schnittstellen oder Typmember, einem Fehler während der Kompilierung führt. Zum Beispiel:

```vb
Public Partial Class Test1
    Implements IDisposable
End Class

Class Test1
    Inherits Object
    Implements IComparable
End Class

Public Partial Class Test2
End Class

Private Partial Class Test2
End Class
```

Im vorherige Beispiel deklariert einen Typ `Test1` , `Public`, erbt `Object` und implementiert `System.IDisposable` und `System.IComparable`. Die partiellen Deklarationen von `Test2` verursacht einen Fehler während der Kompilierung aus, da einer der Deklarationen, auf welchem `Test2` ist `Public` und einen anderen wird angegeben, dass `Test2` ist `Private`.

Partielle Typen mit den beiden Typparametern können deklarieren, Einschränkungen und die Varianz für die Typparameter an, aber die Einschränkungen und der Varianz aus jeder partiellen Deklaration müssen übereinstimmen. Somit sind Einschränkungen und Varianz spezielle darin, dass sie nicht automatisch wie andere Modifizierer kombiniert werden:

```vb
Partial Public Class List(Of T As IEnumerable)
End Class

' Error: Constraints on T don't match
Class List(Of T As IComparable)
End Class
```

Die Tatsache, dass ein Typ deklariert wird mit mehreren partiellen Deklarationen wirkt sich nicht auf die Name-Suchregeln innerhalb des Typs aus. Daher eine Deklaration der partiellen Typs kann in andere partielle Typdeklarationen deklarierten Member oder kann Methoden auf, die in andere partielle Typdeklarationen deklariert Schnittstellen implementieren. Zum Beispiel:

```vb
Public Partial Class Test1
    Implements IDisposable

    Private IsDisposed As Boolean = False
End Class

Class Test1
    Private Sub Dispose() Implements IDisposable.Dispose
        If Not IsDisposed Then
            ...
        End If
    End Sub
End Class
```

Geschachtelte Typen haben auch die partielle Deklarationen. Zum Beispiel:

```vb
Public Partial Class Test
    Public Partial Class NestedTest
        Public Sub S1()
        End Sub
    End Class
End Class

Public Partial Class Test
    Public Partial Class NestedTest
        Public Sub S2()
        End Sub
    End Class
End Class
```

Initialisierer in eine partielle Deklaration werden immer noch in der Reihenfolge der Deklaration ausgeführt werden. Es ist jedoch keine festgelegte Reihenfolge der Ausführung von Initialisierern, die in separaten partiellen Deklarationen auftreten.

## <a name="constructed-types"></a>Konstruierte Typen

Die Deklaration eines generischen Typs bezeichnet selbst, einen Typ keinen. Deklaration eines generischen Typs kann stattdessen als "Blaupause" verwendet werden, um viele verschiedene Arten zu erstellen, durch Anwenden von Typargumenten. Ein generischer Typ, die Typargumente, die darauf angewendet wird aufgerufen, eine *konstruierter Typ*. Die Typargumente in einem konstruierten Typ müssen immer erfüllen, die Einschränkungen platziert, die von den Typparametern, die, denen Sie zu entsprechen.

Ein Typname möglicherweise ein konstruierten Typs identifizieren, obwohl sie direkt Typparameter angeben nicht. Dies kann auftreten, in denen ein Typ in der Deklaration einer generischen Klasse geschachtelt ist und der Instanztyp der enthaltenden Deklaration wird implizit für die Namenssuche verwendet:

```vb
Class Outer(Of T) 
    Public Class Inner 
    End Class

    ' Type of i is the constructed type Outer(Of T).Inner
    Public i As Inner 
End Class
```

Ein konstruierter Typ `C(Of T1,...,Tn)` kann zugegriffen werden, wenn der generische Typ und alle Typargumente zugegriffen werden kann. Z. B. wenn der generische Typ `C` ist `Public` und alle Typargumente `T1,...,Tn` sind `Public`, und klicken Sie dann der konstruierte Typ ist `Public`. Wenn der Typname oder eines der Argumente des Typs ist `Private`, allerdings wird der Zugriff des konstruierten Typs `Private`. Wenn ein Argument des konstruierten Typs ist `Protected` und ein anderes Typargument `Friend`, und klicken Sie dann der konstruierte Typ ist nur in der Klasse und deren Unterklassen in dieser Assembly oder einer beliebigen Assembly, die erteilt wurden `Friend` Zugriff. Das heißt, ist die Zugriffsdomäne eines konstruierten Typs die Schnittmenge der Barrierefreiheit Domänen seiner Bestandteile.

__Beachten Sie.__ Die Tatsache, dass die Zugriffsdomäne des konstruierten Typs die Schnittmenge mit den zugehörigen ausgebildetes Teilen ist hat es sich um den interessanten Nebeneffekt definieren Sie eine neue Ebene für die Barrierefreiheit. Ein konstruierter Typ, der ein Element enthält, die `Protected` und ein Element, das `Friend` kann nur in Kontexten, die auf Sie zugreifen, können zugegriffen werden *sowohl* `Friend` *und* `Protected`Member. Allerdings besteht keine Möglichkeit, diese Zugriffsebene in der Sprache, wie der Zugriff auf express `Protected Friend` bedeutet, dass eine Entität in einem Kontext zugegriffen werden kann, die zugreifen können *entweder* `Friend` *oder* `Protected` Member.

Die Basis, implementierten Schnittstellen und die Member der konstruierten Typen werden durch, und Ersetzen Sie dabei die angegebenen Typargumente für jedes Vorkommen des Typparameters in der generische Typ bestimmt.

### <a name="open-types-and-closed-types"></a>Offene und geschlossene Typen

Ein konstruierter Typ, für, die eine oder mehrere Typargumente Typparameter eines enthaltenden Typs oder-Methode wird aufgerufen, eine *offener Typ*. Grund hierfür ist Teil der Typparameter des Typs immer noch nicht bekannt sind, damit die tatsächliche Form vom Typ noch nicht vollständig bekannt ist. Ein generischer Typ, deren Typargumente alle Nichttyp-Parameter werden, wird aufgerufen, im Gegensatz dazu ein *geschlossener Typ*. Die Form eines geschlossenen Typs wird immer vollständig bezeichnet. Zum Beispiel:

```vb
Class Base(Of T, V)
End Class

Class Derived(Of V)
    Inherits Base(Of Integer, V)
End Class

Class MoreDerived
    Inherits Derived(Of Double)
End Class
```

Der konstruierte Typ `Base(Of Integer, V)` ist ein offener Typ, da jedoch der Typparameter `T` wurde bereitgestellt, der Typparameter `U` wurde einen anderen Typparameter angegeben. Daher ist die vollständige Form vom Typ noch nicht bekannt. Der konstruierte Typ `Derived(Of Double)`, ist jedoch ein geschlossener Typ aus, da alle Typparameter in der Vererbungshierarchie angegeben wurden.

Offene Typen werden wie folgt definiert:

* Ein Typparameter ist ein offener Typ.

* Array-Typ ist ein offener Typ, wenn der Elementtyp ein offener Typ ist.

* Ein konstruierter Typ ist ein offener Typ, wenn eine oder mehrere seiner Typargumente sind ein offener Typ.

* Ein geschlossener Typ ist ein Typ, der nicht auf einen offenen Typ handelt.

Da der Einstiegspunkt des Programms in einem generischen Typ sein kann, werden alle Typen, die zur Laufzeit verwendet die geschlossene Typen.

## <a name="special-types"></a>Spezielle Typen

Das .NET Framework enthält eine Reihe von Klassen, die speziell .NET Framework und Visual Basic-Sprache behandelt werden:

Der Typ `System.Void`, steht für einen void-Typ in .NET Framework kann direkt verwiesen werden nur in `GetType` Ausdrücke.

Die Typen `System.RuntimeArgumentHandle`, `System.ArgIterator` und `System.TypedReference` alle Zeiger in den Stapel enthalten kann und daher darf nicht auf dem .NET Framework-Heap. Sie können nicht aus diesem Grund verwendet werden, als Arrayelementtypen, Rückgabetypen, Feldtypen, generische Typargumente, nullable-Typen `ByRef` Parametertypen und den Typ des zu konvertierenden Wert `Object` oder `System.ValueType`, das Ziel eines Aufrufs an die Instanz Mitglieder der `Object` oder `System.ValueType`, oder in einen Closure aufgehoben.
