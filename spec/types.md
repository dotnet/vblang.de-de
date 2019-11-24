---
ms.openlocfilehash: 4560eac9f4ab52d07c77724aeca696d0195da91a
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306022"
---
# <a name="types"></a>Typen

Die zwei grundlegenden Kategorien von Typen in Visual Basic sind *Werttypen* und *Verweis Typen*. Primitive Typen (außer Zeichen folgen), Enumerationen und Strukturen sind Werttypen. Klassen, Zeichen folgen, Standardmodule, Schnittstellen, Arrays und Delegaten sind Verweis Typen.

Jeder Typ verfügt über einen *Standardwert*. Dies ist der Wert, der Variablen dieses Typs bei der Initialisierung zugewiesen wird.

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

Auch wenn Werttypen und Verweis Typen in Bezug auf Deklarations Syntax und-Verwendung ähnlich sein können, sind ihre Semantik unterschiedlich.

Verweis Typen werden auf dem Lauf Zeit Heap gespeichert. auf Sie kann nur über einen Verweis auf diesen Speicher zugegriffen werden. Da auf Verweis Typen immer über Verweise zugegriffen wird, wird deren Lebensdauer vom .NET Framework verwaltet. Ausstehende Verweise auf eine bestimmte Instanz werden nachverfolgt, und die Instanz wird nur zerstört, wenn keine weiteren Verweise mehr vorhanden sind. Eine Variable des Verweis Typs enthält einen Verweis auf einen Wert dieses Typs, einen Wert eines stärker abgeleiteten Typs oder einen NULL-Wert. Ein *NULL-Wert* verweist auf "Nothing". Es ist nicht möglich, etwas mit einem NULL-Wert zu verwenden, mit Ausnahme der Zuweisung. Durch die Zuweisung zu einer Variablen eines Verweis Typs wird eine Kopie des Verweises anstelle einer Kopie des Werts erstellt, auf den verwiesen wird. Bei einer Variablen eines Verweis Typs ist der Standardwert ein NULL-Wert.

Werttypen werden direkt auf dem Stapel gespeichert, entweder innerhalb eines Arrays oder innerhalb eines anderen Typs. auf Ihren Speicher kann nur direkt zugegriffen werden. Da Werttypen direkt innerhalb von Variablen gespeichert werden, wird deren Lebensdauer von der Lebensdauer der Variablen bestimmt, in der Sie enthalten sind. Wenn der Speicherort, der eine Werttyp Instanz enthält, zerstört wird, wird auch die Werttyp Instanz zerstört. Auf Werttypen wird immer direkt zugegriffen. Es ist nicht möglich, einen Verweis auf einen Werttyp zu erstellen. Durch das verbieten eines solchen Verweises ist es unmöglich, auf eine Instanz der Wert Klasse zu verweisen, die zerstört wurde. Da Werttypen immer `NotInheritable`sind, enthält eine Variable eines Werttyps immer einen Wert dieses Typs. Aus diesem Grund kann der Wert eines Werttyps kein NULL-Wert sein, und er kann nicht auf ein Objekt eines stärker abgeleiteten Typs verweisen. Durch die Zuweisung zu einer Variablen eines Werttyps wird eine Kopie des zugewiesenen Werts erstellt. Der Standardwert für eine Variable eines Werttyps ist das Ergebnis der Initialisierung der einzelnen Variablen Elemente des Typs mit dem Standardwert.

Das folgende Beispiel zeigt den Unterschied zwischen Verweis Typen und Werttypen:

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

Die Ausgabe des Programms lautet wie folgt:

```console
Values: 0, 123
Refs: 123, 123
```

Die Zuweisung zur lokalen Variablen `val2` wirkt sich nicht auf die lokale Variable `val1` aus, da beide lokalen Variablen einen Werttyp (der Typ `Integer`) und jede lokale Variable eines Werttyps über einen eigenen Speicher verfügt. Im Gegensatz dazu wirkt sich der Zuweisungs `ref2.Value = 123;` auf das Objekt aus, das sowohl `ref1` als auch `ref2` Verweis ist.

Beachten Sie, dass die .NET Framework Typsystem auch dann, wenn Strukturen, Enumerationen und primitive Typen (mit Ausnahme von `String`) Werttypen sind, alle von Verweis Typen erben. Strukturen und primitive Typen erben vom Verweistyp `System.ValueType`, der von `Object`erbt. Enumerierte Typen erben vom Verweistyp `System.Enum`, der von `System.ValueType`erbt.

### <a name="nullable-value-types"></a>Auf NULL festlegbare Werttypen

Für Werttypen kann einem Typnamen ein `?` Modifizierer hinzugefügt werden, der die auf NULL festleg *Bare* Version dieses Typs darstellt.

```antlr
NullableTypeName
    : NonArrayTypeName '?'
    ;

NullableNameModifier
    : '?'
    ;
```

Ein Werttyp, der NULL-Werte zulässt, kann die gleichen Werte wie die Version des Typs, die keine NULL-Werte zulässt, sowie den Nullwert enthalten. Daher wird für einen Werttyp, der NULL-Werte zulässt, das Zuweisen von `Nothing` zu einer Variablen vom Typ den Wert der Variablen auf den NULL-Wert und nicht auf den Wert 0 (null) des Werttyps festgelegt. Beispiel:

```vb
Dim x As Integer = Nothing
Dim y As Integer? = Nothing

' Prints zero
Console.WriteLine(x)
' Prints nothing (because the value of y is the null value)
Console.WriteLine(y)
```

Eine Variable kann auch als ein Werttyp, der NULL-Werte zulässt, deklariert werden, indem ein Typmodifizierer, der NULL-Werte zulässt, für den Variablennamen Aus Gründen der Übersichtlichkeit ist es nicht zulässig, einen Typmodifizierer, der NULL-Werte zulässt, für einen Variablennamen und einen Typnamen in derselben Deklaration zu haben. Da Typen, die NULL-Werte zulassen, mithilfe des-Typs `System.Nullable(Of T)`implementiert werden, ist der Typ `T?` Synonym für den Typ `System.Nullable(Of T)`, und die beiden Namen können austauschbar verwendet werden. Der `?` Modifizierer kann nicht für einen Typ platziert werden, der bereits NULL-Werte zulässt. Daher ist es nicht möglich, den Typ `Integer??` oder `System.Nullable(Of Integer)?`zu deklarieren.

Ein Werttyp, der auf NULL festgelegt werden kann `T?` die Member von `System.Nullable(Of T)` sowie alle Operatoren *oder Konvertierungen* , die vom zugrunde liegenden Typ entfernt wurden, in den Typ `T?``T`. Das Lifting von kopiert Operatoren und Konvertierungen aus dem zugrunde liegenden Typ, in den meisten Fällen, die NULL-Werte zulassen, für Werttypen, die keine Nullwerte zulassen Dadurch können viele der gleichen Konvertierungen und Vorgänge, die für `T` gelten, auch auf `T?` angewendet werden.


## <a name="interface-implementation"></a>Schnittstellen Implementierung

Struktur-und Klassen Deklarationen können deklarieren, dass Sie einen Satz von Schnittstellentypen durch eine oder mehrere `Implements` Klauseln implementieren.

```antlr
TypeImplementsClause
    : 'Implements' TypeImplements StatementTerminator
    ;

TypeImplements
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

Alle in der `Implements`-Klausel angegebenen Typen müssen Schnittstellen sein, und der Typ muss alle Member der Schnittstellen implementieren. Beispiel:

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

Ein Typ, der eine Schnittstelle implementiert, implementiert auch implizit alle Basis Schnittstellen der Schnittstelle. Dies gilt auch, wenn der Typ nicht explizit alle Basis Schnittstellen in der `Implements`-Klausel aufführt. In diesem Beispiel implementiert die `TextBox` Struktur sowohl `IControl` als auch `ITextBox`.

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

Wenn Sie deklarieren, dass ein Typ eine Schnittstelle in und von sich selbst implementiert, wird im Deklarations Bereich des Typs nichts deklariert. Daher ist es zulässig, zwei Schnittstellen zu implementieren, die eine Methode mit demselben Namen haben.

Typen können einen Typparameter nicht selbst implementieren, obwohl Sie möglicherweise die Typparameter enthalten, die sich im Gültigkeitsbereich befinden.

```vb
Class C1(Of V)
    Implements V  ' Error, can't implement type parameter directly
    Implements IEnumerable(Of V)  ' OK, not directly implementing

    ...
End Class
```

Generische Schnittstellen können mehrmals mit unterschiedlichen Typargumenten implementiert werden. Ein generischer Typ kann jedoch eine generische Schnittstelle nicht mithilfe eines Typparameters implementieren, wenn der angegebene Typparameter (ungeachtet der Typeinschränkungen) mit einer anderen Implementierung dieser Schnittstelle überlappt. Beispiel:

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


## <a name="primitive-types"></a>Primitive Datentypen

Die *primitiven Typen* werden mithilfe von Schlüsselwörtern identifiziert, bei denen es sich um Aliase für vordefinierte Typen im `System`-Namespace handelt. Ein primitiver Typ ist von dem Typ, den er Aliase hat, vollständig nicht unterscheiden: das Schreiben des reservierten Worts `Byte` ist exakt identisch mit dem Schreiben von `System.Byte`. Primitive Typen werden auch als systeminterne *Typen*bezeichnet.

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


Da ein primitiver Typ einen regulären Typ Aliase, weist jeder Primitive Typ Member auf. Beispielsweise `Integer` die Member in `System.Int32`deklariert. Literale können als Instanzen der entsprechenden Typen behandelt werden.

* Die primitiven Typen unterscheiden sich von anderen Strukturtypen insofern, als Sie bestimmte zusätzliche Vorgänge zulassen:

* Primitive Typen ermöglichen das Erstellen von Werten, indem Literale geschrieben werden. `123I` ist z. b. ein Literaltyp `Integer`.

* Es ist möglich, Konstanten der primitiven Typen zu deklarieren.

* Wenn es sich bei den Operanden eines Ausdrucks um primitive Typkonstanten handelt, kann der Compiler den Ausdruck zum Zeitpunkt der Kompilierung auswerten. Ein solcher Ausdruck wird als konstanter Ausdruck bezeichnet.

Visual Basic definiert die folgenden primitiven Typen:

* Die ganzzahligen Werttypen `Byte` (1-Byte-Ganzzahl ohne Vorzeichen), `SByte` (1-Byte-Ganzzahl mit Vorzeichen), `UShort` (2-Byte-Ganzzahl ohne Vorzeichen), `Short` (2-Byte-Ganzzahl mit Vorzeichen), `UInteger` (4-Byte-Ganzzahl ohne Vorzeichen), `Integer` (4-Byte-Ganzzahl mit Vorzeichen), `ULong` (8-Byte-Integer ohne Vorzeichen) und `Long` (8-Byte-Ganzzahl Diese Typen werden `System.Byte`, `System.SByte`, `System.UInt16`, `System.Int16`, `System.UInt32`, `System.Int32`, `System.UInt64` bzw. `System.Int64`zugeordnet. Der Standardwert eines ganzzahligen Typs entspricht dem Literal`0`.

* Die Gleit Komma Werttypen `Single` (4-Byte-Gleit Komma Zahl) und `Double` (8-Byte-Gleit Komma Zahl). Diese Typen werden `System.Single` bzw. `System.Double`zugeordnet. Der Standardwert eines Gleit Komma Typs entspricht dem Literal`0`.

* Der `Decimal` Typ (16-Byte-Dezimalwert), der `System.Decimal`zugeordnet ist. Der Standardwert von Decimal entspricht dem Literal`0D`.

* Der `Boolean` Werttyp, der einen Wahrheitswert darstellt (in der Regel das Ergebnis einer relationalen oder logischen Operation). Der Literaltyp ist vom Typ `System.Boolean`. Der Standardwert des `Boolean` Typs entspricht dem Literal`False`.

* Der `Date` Werttyp, der ein Datum und/oder eine Uhrzeit darstellt und `System.DateTime`zugeordnet ist. Der Standardwert des `Date` Typs entspricht dem Literal`# 01/01/0001 12:00:00AM #`.

* Der `Char` Werttyp, der ein einzelnes Unicode-Zeichen darstellt und `System.Char`zugeordnet ist. Der Standardwert des `Char` Typs entspricht dem konstanten Ausdruck `ChrW(0)`.

* Der `String` Verweistyp, der eine Sequenz von Unicode-Zeichen darstellt und `System.String`zugeordnet ist. Der Standardwert des `String` Typs ist ein NULL-Wert.



## <a name="enumerations"></a>Enumerationen

*Enumerationen* sind Werttypen, die von `System.Enum` erben und symbolisch eine Menge von Werten eines der primitiven ganzzahligen Typen darstellen.

```antlr
EnumDeclaration
    : Attributes? TypeModifier* 'Enum' Identifier
      ( 'As' NonArrayTypeName )? StatementTerminator
      EnumMemberDeclaration+
      'End' 'Enum' StatementTerminator
    ;
```

Bei einem Enumerationstyp `E`ist der Standardwert der Wert, der vom Ausdruck `CType(0, E)`erzeugt wird.

Der zugrunde liegende Typ einer Enumeration muss ein ganzzahliger Typ sein, der alle in der-Enumeration definierten Enumeratorwerte darstellen kann. Wenn ein zugrunde liegender Typ angegeben wird, muss es sich um `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`oder einen der entsprechenden Typen im `System`-Namespace handeln. Wenn kein zugrunde liegender Typ explizit angegeben wird, wird der Standardwert `Integer`.

Im folgenden Beispiel wird eine Enumeration mit einem zugrunde liegenden Typ von `Long`deklariert:

```vb
Enum Color As Long
    Red
    Green
    Blue
End Enum
```

Ein Entwickler kann eine zugrunde liegende Art von `Long`verwenden, wie z. b., um die Verwendung von Werten zu ermöglichen, die sich im Bereich von `Long`, aber nicht im Bereich von `Integer`befinden, oder um diese Option für die Zukunft beizubehalten.


### <a name="enumeration-members"></a>Enumerationsmember

Die Member einer Enumeration sind die in der-Enumeration deklarierten Enumerationswerte und die von der-Klasse geerbten Member `System.Enum`.

Der Bereich eines Enumerationsmembers ist der enumerationsdeklarations-Text. Dies bedeutet, dass ein Enumerationsmember außerhalb einer Enumerationsdeklaration immer qualifiziert werden muss (es sei denn, der Typ wird durch einen Namespace Import ausdrücklich in einen Namespace importiert).

Die Deklarations Reihenfolge für enumerationselementdeklarationen ist signifikant, wenn Konstante Ausdrucks Werte ausgelassen werden. Enumerationsmember haben implizit nur `Public` Zugriff; Es sind keine Zugriffsmodifizierer für Enumerationsmember zulässig.

```antlr
EnumMemberDeclaration
    : Attributes? Identifier ( Equals ConstantExpression )? StatementTerminator
    ;
```

### <a name="enumeration-values"></a>Enumerationswerte

Die Enumerationswerte in einer Enumerationsmember-Liste werden als Konstanten deklariert, die als zugrunde liegender Enumerationstyp typisiert sind. Sie können dort vorkommen, wo Konstanten erforderlich sind. Eine Enumerationsmember-Definition mit `=` gibt dem zugeordneten Member den Wert, der durch den konstanten Ausdruck angegeben wird. Der Konstante Ausdruck muss zu einem ganzzahligen Typ ausgewertet werden, der implizit in den zugrunde liegenden Typ konvertierbar ist und innerhalb des Bereichs von Werten liegen muss, der durch den zugrunde liegenden Typ dargestellt werden kann. Das folgende Beispiel ist fehlerhaft, da die Konstanten Werte `1.5`, `2.3`und `3.3` nicht implizit in den zugrunde liegenden ganzzahligen Typ `Long` mit strenger Semantik konvertiert werden können.

```vb
Option Strict On

Enum Color As Long
    Red = 1.5
    Green = 2.3
    Blue = 3.3
End Enum
```

Mehrere Enumerationsmember haben möglicherweise denselben zugeordneten Wert, wie unten dargestellt:

```vb
Enum Color
    Red
    Green
    Blue
    Max = Blue
End Enum
```

Das Beispiel zeigt eine Enumeration, die über zwei Enumerationsmember verfügt: `Blue` und `Max`, die denselben zugeordneten Wert aufweisen.

Wenn die erste Enumeratorwertdefinition in der-Enumeration keinen Initialisierer aufweist, wird der Wert der entsprechenden Konstante `0`. Eine Enumerationswertdefinition ohne Initialisierer übergibt dem Enumerator den Wert, der durch die Erhöhung des Werts des vorherigen Enumerationswerts durch `1`abgerufen wird. Dieser erweiterte Wert muss innerhalb des Wertebereichs liegen, der durch den zugrunde liegenden Typ dargestellt werden kann.

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

Im obigen Beispiel werden die Enumerationswerte und ihre zugeordneten Werte ausgegeben. Ausgabe:

```console
Red = 0
Green = 10
Blue = 11
```

Die Gründe für die Werte lauten wie folgt:

* Dem Enumerationswert `Red` wird automatisch der Wert `0` zugewiesen (da er über keinen Initialisierer verfügt und der erste enumerationswertmember ist).

* Der-Enumerationswert `Green` der explizit den Wert `10`erhält.

* Dem Enumerationswert `Blue` wird automatisch der Wert zugewiesen, der größer ist als der Enumerationswert, der ihm textlich vorangestellt ist.

Der Konstante Ausdruck darf nicht direkt oder indirekt den Wert seines eigenen zugeordneten Enumerationswerts verwenden (d. h., Zirkularität im Konstantenausdruck ist nicht zulässig). Das folgende Beispiel ist ungültig, da die Deklarationen von `A` und `B` zirkulär sind.

```vb
Enum Circular
    A = B
    B
End Enum
```

`A` ist explizit von `B` abhängig, und `B` von `A` implizit abhängig ist.

## <a name="classes"></a>Klassen

Eine- *Klasse* ist eine Datenstruktur, die Datenmember (Konstanten, Variablen und Ereignisse), Funktionsmember (Methoden, Eigenschaften, Indexer, Operatoren und Konstruktoren) und die in der Struktur enthaltenen Typen enthalten kann. Klassen sind Verweis Typen.

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

Es gibt zwei klassenspezifische Modifizierer, `MustInherit` und `NotInheritable`. Beide können nicht gleichzeitig angegeben werden.


### <a name="class-base-specification"></a>Klassenbasis Spezifikation

Eine Klassen Deklaration kann eine Basistyp Spezifikation enthalten, die den direkten Basistyp der Klasse definiert.

```antlr
ClassBase
    : 'Inherits' NonArrayTypeName StatementTerminator
    ;
```

Wenn eine Klassen Deklaration keinen expliziten Basistyp aufweist, wird der direkte Basistyp implizit `Object`. Beispiel:

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class
```

Basis Typen können nicht eigenständig ein Typparameter sein, obwohl Sie möglicherweise die Typparameter enthalten, die sich im Gültigkeitsbereich befinden.

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

Klassen können nur von `Object`-und-Klassen abgeleitet werden. Es ist ungültig, dass eine Klasse von `System.ValueType`, `System.Enum`, `System.Array`, `System.MulticastDelegate` oder `System.Delegate`abgeleitet wird. Eine generische Klasse kann nicht von `System.Attribute` oder von einer Klasse abgeleitet werden, die davon abgeleitet ist.

Jede Klasse verfügt über genau eine direkte Basisklasse, und Zirkularität bei der Ableitung ist nicht zulässig. Es ist nicht möglich, von einer `NotInheritable` Klasse abzuleiten, und die Zugriffs Domäne der Basisklasse muss mit der Zugriffs Domäne der Klasse selbst identisch sein.


### <a name="class-members"></a>Klassenmember

Die Member einer Klasse bestehen aus den Membern, die von ihren Klassenmember Deklarationen und den Membern, die von der direkten Basisklasse geerbt wurden,

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

Eine Klassenmember-Deklaration kann über `Public`, `Protected`, `Friend`, `Protected Friend`oder `Private` Zugriff verfügen. Wenn eine Klassenmember-Deklaration keinen Zugriffsmodifizierer enthält, wird standardmäßig `Public` Access verwendet, sofern es sich nicht um eine Variablen Deklaration handelt. in diesem Fall wird standardmäßig `Private` Access verwendet.

Der Gültigkeitsbereich eines Klassenmembers ist der Klassen Text, in dem die Element Deklaration auftritt, sowie die Einschränkungs Liste dieser Klasse (wenn Sie generisch und Einschränkungen aufweist). Wenn das Element `Friend` Access hat, erstreckt sich sein Bereich auf den Klassen Text einer beliebigen abgeleiteten Klasse im gleichen Programm oder auf eine beliebige Assembly, die `Friend` Zugriff erhalten hat, und wenn der Member `Public`, `Protected`oder `Protected Friend` Zugriff hat, erstreckt sich der Gültigkeitsbereich auf den Klassen Text einer beliebigen abgeleiteten Klasse in jedem Programm.


## <a name="structures"></a>Strukturen

*Strukturen* sind Werttypen, die von `System.ValueType`erben. Strukturen ähneln Klassen darin, dass Sie Datenstrukturen darstellen, die Datenmember und Funktionsmember enthalten können. Im Gegensatz zu-Klassen erfordern Strukturen jedoch keine Heap Zuordnung.

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

Im Fall von Klassen können zwei Variablen auf das gleiche Objekt verweisen, und so können Vorgänge in einer Variablen das Objekt beeinflussen, auf das von der anderen Variablen verwiesen wird. Bei Strukturen verfügen die Variablen jeweils über eine eigene Kopie der Daten, die nicht`Shared` sind. Daher ist es nicht möglich, dass sich Vorgänge auf dem anderen befinden, wie im folgenden Beispiel veranschaulicht:

```vb
Structure Point
    Public x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

In der obigen Deklaration gibt der folgende Code den Wert `10`aus:

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

Durch die Zuweisung von `a` `b` wird eine Kopie des Werts erstellt, und `b` ist von der Zuweisung zum `a.x`nicht betroffen. Wenn `Point` stattdessen als Klasse deklariert wurde, wird die Ausgabe `100`, da `a` und `b` auf das gleiche Objekt verweisen würden.


### <a name="structure-members"></a>Strukturmember

Die Member einer Struktur sind die Member, die von den zugehörigen Strukturmember-Deklarationen und den von `System.ValueType`geerbten Membern eingeführt werden.

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

Jede Struktur verfügt implizit über einen `Public` Parameter losen Instanzenkonstruktor, der den Standardwert der-Struktur erzeugt. Daher ist es für eine Strukturtyp Deklaration nicht möglich, einen Parameter losen Instanzenkonstruktor zu deklarieren. Ein Strukturtyp ist jedoch zulässig, um *parametrisierte* Instanzkonstruktoren zu deklarieren, wie im folgenden Beispiel gezeigt:

```vb
Structure Point
    Private x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

Bei der obigen Deklaration erstellen die folgenden Anweisungen eine `Point` mit `x` und `y` auf NULL initialisiert.

```vb
Dim p1 As Point = New Point()
Dim p2 As Point = New Point(0, 0)
```

Da Strukturen ihre Feldwerte direkt (anstatt Verweise auf diese Werte) enthalten, können Strukturen keine Felder enthalten, die sich direkt oder indirekt auf sich selbst beziehen. Der folgende Code ist beispielsweise ungültig:

```vb
Structure S1
    Dim f1 As S2
End Structure

Structure S2
    ' This would require S1 to contain itself.
    Dim f1 As S1
End Structure
```

Normalerweise kann eine Strukturmember-Deklaration nur über `Public`, `Friend`oder `Private` Access verfügen, aber wenn Sie von `Object`geerbte Member überschreiben, können auch `Protected` und `Protected Friend` Zugriff verwendet werden. Wenn eine Strukturmember-Deklaration keinen Zugriffsmodifizierer enthält, wird standardmäßig `Public` Access in der Deklaration verwendet. Der Gültigkeitsbereich eines Members, der von einer-Struktur deklariert wird, ist der Struktur Text, in dem die Deklaration auftritt, sowie die Einschränkungen dieser Struktur (wenn Sie generisch und Einschränkungen enthielt).


## <a name="standard-modules"></a>Standard Module

Ein *Standardmodul* ist ein Typ, dessen Member implizit `Shared` werden und auf den Deklarations Bereich des enthaltenden Namespace des Standardmoduls beschränkt sind, und nicht nur auf die Standardmodul Deklaration selbst. Standard Module werden möglicherweise nie instanziiert. Es ist ein Fehler, eine Variable eines Standard Modultyps zu deklarieren.

```antlr
ModuleDeclaration
    : Attributes? TypeModifier* 'Module' Identifier StatementTerminator
      ModuleMemberDeclaration*
      'End' 'Module' StatementTerminator
    ;
```

Ein Member eines Standardmoduls verfügt über zwei voll qualifizierte Namen, eine ohne den standardmodulnamen und eine mit dem standardmodulnamen. Mehrere Standardmodule in einem Namespace können einen Member mit einem bestimmten Namen definieren. nicht qualifizierte Verweise auf den Namen außerhalb eines der beiden Module sind mehrdeutig. Beispiel:

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

Ein Modul kann nur in einem Namespace deklariert werden und darf nicht in einem anderen Typ eingefügt werden. Standard Module können keine Schnittstellen implementieren, Sie werden implizit von `Object`abgeleitet und verfügen nur über `Shared` Konstruktoren.


### <a name="standard-module-members"></a>Standardmodulmember

Die Member eines Standardmoduls sind die Member, die von den Element Deklarationen und den von `Object`geerbten Membern eingeführt werden. Standard Module können einen beliebigen Typ von Membern aufweisen, außer Instanzkonstruktoren. Alle standardmodultyp Elemente werden implizit `Shared`.

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

Normalerweise kann eine standardmäßige Modulmember-Deklaration nur über `Public`, `Friend`oder `Private` Zugriff verfügen, aber wenn Sie von `Object`geerbte Member überschreiben, können die `Protected`-und `Protected Friend` Zugriffsmodifizierer angegeben werden. Wenn die Deklaration eines Standardmodulmembers keinen Zugriffsmodifizierer enthält, wird standardmäßig `Public` Access verwendet, es sei denn, es handelt sich um eine Variable, deren standardmäßig `Private` Access verwendet wird

Wie bereits erwähnt, ist der Gültigkeitsbereich eines Standardmodulmembers die Deklaration, die die Standardmodul Deklaration enthält. Member, die von `Object` geerbt werden, sind in dieser speziellen Bereichs Definition nicht enthalten. Diese Member haben keinen Gültigkeitsbereich und müssen immer mit dem Namen des Moduls qualifiziert werden. Wenn das Element `Friend` Access hat, erstreckt sich sein Bereich nur auf Namespace Elemente, die im selben Programm oder in Assemblys deklariert sind, denen `Friend` Zugriff erteilt wurde.


## <a name="interfaces"></a>Schnittstellen

*Schnittstellen* sind Verweis Typen, die andere Typen implementieren, um sicherzustellen, dass Sie bestimmte Methoden unterstützen. Eine Schnittstelle wird nie direkt erstellt und hat keine tatsächliche Darstellung. andere Typen müssen in einen Schnittstellentyp konvertiert werden. Eine Schnittstelle definiert einen Vertrag. Eine Klasse oder Struktur, die eine Schnittstelle implementiert, muss ihren Vertrag einhalten.

```antlr
InterfaceDeclaration
    : Attributes? TypeModifier* 'Interface' Identifier
      TypeParameterList? StatementTerminator
      InterfaceBase*
      InterfaceMemberDeclaration*
      'End' 'Interface' StatementTerminator
    ;
```


Das folgende Beispiel zeigt eine Schnittstelle, die eine Standard Eigenschaft `Item`, einen Ereignis `E`, eine Methode `F`und eine Eigenschaften `P`enthält:

```vb
Interface IExample
    Default Property Item(index As Integer) As String

    Event E()

    Sub F(value As Integer)

    Property P() As String
End Interface
```

Schnittstellen können mehrere Vererbung verwenden. Im folgenden Beispiel erbt die-Schnittstelle `IComboBox` von `ITextBox` und `IListBox`:

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

Klassen und Strukturen können mehrere Schnittstellen implementieren. Im folgenden Beispiel wird die-Klasse `EditBox` von der-Klasse abgeleitet `Control` und sowohl `IControl` als auch `IDataBound`implementiert:

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

Die Basis Schnittstellen einer Schnittstelle sind die expliziten Basis Schnittstellen und deren Basis Schnittstellen. Mit anderen Worten: der Satz von Basis Schnittstellen ist die komplette transitiv Schließung der expliziten Basis Schnittstellen, ihrer expliziten Basis Schnittstellen usw. Wenn eine Schnittstellen Deklaration keine explizite Schnittstellen Basis hat, gibt es keine Basisschnittstelle für den Typ---Schnittstellen erben nicht von `Object` (obwohl Sie über eine erweiternde Konvertierung in `Object`verfügen).

```antlr
InterfaceBase
    : 'Inherits' InterfaceBases StatementTerminator
    ;

InterfaceBases
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

Im folgenden Beispiel sind die Basis Schnittstellen von `IComboBox` `IControl`, `ITextBox`und `IListBox`.

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

Eine Schnittstelle erbt alle Member ihrer Basis Schnittstellen. Mit anderen Worten, die obige `IComboBox`-Schnittstelle erbt Member `SetText` und `SetItems` sowie `Paint`.

Eine Klasse oder Struktur, die eine Schnittstelle implementiert, implementiert auch implizit alle Basis Schnittstellen der Schnittstelle.

Wenn eine Schnittstelle bei der transitiven Schließung der Basis Schnittstellen mehrmals angezeigt wird, werden die Member nur einmal zur abgeleiteten Schnittstelle hinzugezogen. Ein Typ, der die abgeleitete Schnittstelle implementiert, muss nur einmal die Methoden der mehrfach definierten Basisschnittstelle implementieren. Im folgenden Beispiel muss `Paint` nur einmal implementiert werden, auch wenn die-Klasse `IComboBox` und `IControl`implementiert.

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

Eine `Inherits`-Klausel hat keine Auswirkung auf andere `Inherits` Klauseln. Im folgenden Beispiel müssen `IDerived` den Namen `INested` mit `IBase`qualifizieren.

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

Die Zugriffs Domäne einer Basisschnittstelle muss mit oder einer übergeordneten Zugriffs Domäne der Schnittstelle selbst identisch sein.


### <a name="interface-members"></a>Schnittstellenmember

Die Member einer Schnittstelle bestehen aus den Membern, die von ihren Member-Deklarationen und den von ihren Basis Schnittstellen geerbten Membern eingeführt werden.

```antlr
InterfaceMemberDeclaration
    : NonModuleDeclaration
    | InterfaceEventMemberDeclaration
    | InterfaceMethodMemberDeclaration
    | InterfacePropertyMemberDeclaration
    ;
```

Obwohl Schnittstellen keine Member von `Object`erben, da jede Klasse oder Struktur, die eine Schnittstelle implementiert, von `Object`erbt, werden die Member von `Object`, einschließlich der Erweiterungs Methoden, als Member einer Schnittstelle angesehen und können direkt für eine Schnittstelle aufgerufen werden, ohne dass eine Umwandlung in `Object`erforderlich ist. Beispiel:

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

Member einer Schnittstelle mit dem gleichen Namen wie Member von `Object` implizit Schatten `Object` Membern. Nur geschachtelte Typen, Methoden, Eigenschaften und Ereignisse können Member einer Schnittstelle sein. Methoden und Eigenschaften dürfen keinen Text enthalten. Schnittstellenmember sind implizit `Public` und können keinen Zugriffsmodifizierer angeben. Der Gültigkeitsbereich eines in einer Schnittstelle deklarierten Members ist der Schnittstellen Text, in dem die Deklaration auftritt, sowie die Einschränkungs Liste dieser Schnittstelle (wenn Sie generisch und Einschränkungen aufweist).


## <a name="arrays"></a>Arrays

Ein *Array* ist ein Verweistyp, der Variablen enthält, auf die über *Indizes* zugegriffen wird, die einzeln mit der Reihenfolge der Variablen im Array übereinstimmt. Die Variablen, die in einem Array enthalten sind, das auch als *Elemente* des Arrays bezeichnet wird, müssen alle denselben Typ aufweisen, und dieser Typ wird als *Elementtyp* des Arrays bezeichnet.

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

Die Elemente eines Arrays entstehen, wenn eine Array Instanz erstellt wird, und sind nicht mehr vorhanden, wenn die Array Instanz zerstört wird. Jedes Element eines Arrays wird mit dem Standardwert seines Typs initialisiert. Der Typ `System.Array` ist der Basistyp aller Array Typen und kann nicht instanziiert werden. Jeder Arraytyp erbt die vom `System.Array` Typ deklarierten Member und kann in ihn konvertiert werden (und `Object`). Ein eindimensionaler Arraytyp mit Element `T` implementiert auch die Schnittstellen `System.Collections.Generic.IList(Of T)` und `IReadOnlyList(Of T)`; Wenn `T` ein Referenztyp ist, implementiert der Arraytyp auch `IList(Of U)` und `IReadOnlyList(Of U)` für jede `U`, die über eine erweiternde Verweis Konvertierung aus `T`verfügt.

Ein Array verfügt über einen *Rang* , der die Anzahl der Indizes bestimmt, die mit jedem Array Element verknüpft sind. Der Rang eines Arrays bestimmt die Anzahl der *Dimensionen* des Arrays. Beispielsweise wird ein Array mit dem Rang eins als eindimensionales Array bezeichnet, und ein Array mit einem Rang größer als eins wird als mehrdimensionales Array bezeichnet.

Im folgenden Beispiel wird ein eindimensionales Array von ganzzahligen Werten erstellt, die Array Elemente initialisiert und dann jede von Ihnen ausgegeben:

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

```console
arr(0) = 0
arr(1) = 1
arr(2) = 4
arr(3) = 9
arr(4) = 16
arr(5) = 25
```

Jede Dimension eines Arrays hat eine zugeordnete Länge. Dimensions Längen sind nicht Teil des Array Typs, sondern werden stattdessen festgelegt, wenn eine Instanz des Arraytyps zur Laufzeit erstellt wird. Die Länge einer Dimension bestimmt den gültigen Bereich von Indizes für diese Dimension: für eine Dimension der Länge `N`können Indizes zwischen null und `N-1`reichen. Wenn eine Dimension eine Länge von 0 (null) aufweist, sind für diese Dimension keine gültigen Indizes vorhanden. Die Gesamtanzahl der Elemente in einem Array ist das Produkt der Längen der einzelnen Dimensionen im Array. Wenn eine der Dimensionen eines Arrays eine Länge von 0 (null) aufweist, wird das Array als leer bezeichnet. Der Elementtyp eines Arrays kann ein beliebiger Typ sein.

Array Typen werden angegeben, indem einem vorhandenen Typnamen ein-Modifizierer hinzugefügt wird. Der-Modifizierer besteht aus einer linken Klammer, einem Satz von NULL oder mehr Kommas und einer schließenden Klammer. Der Typ, der geändert wird, ist der Elementtyp des Arrays, und die Anzahl der Dimensionen ist die Anzahl von Kommas plus eins. Wenn mehr als ein Modifizierer angegeben wird, ist der Elementtyp des Arrays ein Array. Die Modifizierer werden von links nach rechts gelesen, wobei der äußerste-Modifizierer das äußerste Array ist. Im Beispiel

```vb
Module Test
    Dim arr As Integer(,)(,,)()
End Module
```

der Elementtyp von `arr` ist ein zweidimensionales Array von dreidimensionalen Arrays von `Integer`.

Eine Variable kann auch als Arraytyp deklariert werden, indem ein Arraytypmodifizierer oder ein Initialisierungs Modifizierer für die Array Größe für den Variablennamen festgelegt wird. In diesem Fall ist der Array Elementtyp der Typ, der in der Deklaration angegeben ist, und die Array Dimensionen werden durch den Variablen namensmodifizierer bestimmt. Aus Gründen der Übersichtlichkeit ist es nicht zulässig, einen Arraytypmodifizierer für einen Variablennamen und einen Typnamen in derselben Deklaration zu haben.

Das folgende Beispiel zeigt eine Vielzahl von lokalen Variablen Deklarationen, die Array Typen mit `Integer` als Elementtyp verwenden:

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

Ein arraytypnamensmodifizierer erstreckt sich auf alle darauf folgenden Sätze von Klammern. Dies bedeutet, dass in Situationen, in denen eine Reihe von Argumenten, die in Klammern eingeschlossen sind, nach einem Typnamen zulässig ist, dass es nicht möglich ist, die Argumente für einen arraytypnamen anzugeben. Beispiel:

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

Im letzten Fall wird `(3)` als Teil des Typnamens und nicht als Satz von Konstruktorargumenten interpretiert.


## <a name="delegates"></a>Delegaten

Ein *Delegat ist ein* Verweistyp, der auf eine `Shared` Methode eines Typs oder auf eine Instanzmethode eines Objekts verweist.

```antlr
DelegateDeclaration
    : Attributes? TypeModifier* 'Delegate' MethodSignature StatementTerminator
    ;

MethodSignature
    : SubSignature
    | FunctionSignature
    ;
```

 Die nächstgelegene Entsprechung eines Delegaten in anderen Sprachen ist ein Funktionszeiger, aber ein Funktionszeiger kann nur auf `Shared` Funktionen verweisen, ein Delegat kann jedoch sowohl auf `Shared`-als auch auf Instanzmethoden verweisen. Im letzteren Fall speichert der Delegat nicht nur einen Verweis auf den Einstiegspunkt der Methode, sondern auch einen Verweis auf die Objektinstanz, mit der die Methode aufgerufen werden soll.

Die Delegatdeklaration darf nicht über eine `Handles`-Klausel, eine `Implements`-Klausel, einen Methoden Text oder ein `End` Konstrukt verfügen. Die Parameterliste der Delegatdeklaration darf keine `Optional` oder `ParamArray` Parameter enthalten. Die Zugriffs Domäne des Rückgabe Typs und der Parametertypen muss mit oder einer übergeordneten Zugriffs Domäne des Delegaten selbst identisch sein.

Die Member eines Delegaten sind die Member, die von der Klasse `System.Delegate`geerbt werden. Ein Delegat definiert auch die folgenden Methoden:

* Ein Konstruktor, der zwei Parameter annimmt, eine vom Typ `Object` und eine vom Typ `System.IntPtr`.

* Eine `Invoke` Methode, die über dieselbe Signatur wie der Delegat verfügt.

* Eine `BeginInvoke` Methode, deren Signatur die Delegatsignatur ist, mit drei unterschieden. Zuerst wird der Rückgabetyp in "`System.IAsyncResult`" geändert. Zweitens werden zwei zusätzliche Parameter am Ende der Parameterliste hinzugefügt: der erste vom Typ `System.AsyncCallback` und der zweite Typ `Object`. Und schließlich werden alle `ByRef` Parameter so geändert, dass Sie `ByVal`werden.

* Eine `EndInvoke` Methode, deren Rückgabetyp mit dem Delegaten übereinstimmt. Bei den Parametern der-Methode handelt es sich nur um die Delegatparameter, die in derselben Reihenfolge wie in der Delegatsignatur vorhanden sind `ByRef` Parameter.  Zusätzlich zu diesen Parametern gibt es einen zusätzlichen Parameter vom Typ `System.IAsyncResult` am Ende der Parameterliste.

Zum Definieren und Verwenden von Delegaten sind drei Schritte erforderlich: Deklaration, Instanziierung und Aufruf.

Delegaten werden mithilfe der Delegatdeklarationssyntax deklariert. Im folgenden Beispiel wird ein Delegat mit dem Namen `SimpleDelegate` deklariert, der keine Argumente annimmt:

```vb
Delegate Sub SimpleDelegate()
```

Im nächsten Beispiel wird eine `SimpleDelegate` Instanz erstellt und dann sofort aufgerufen:

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

Es gibt keinen großen Punkt beim Instanziieren eines Delegaten für eine Methode und dann sofort über den Delegaten aufzurufen, da es einfacher wäre, die Methode direkt aufzurufen. Delegaten zeigen ihre Nützlichkeit, wenn Ihre Anonymität verwendet wird. Das nächste Beispiel zeigt eine `MultiCall` Methode, die wiederholt eine `SimpleDelegate` Instanz aufruft:

```vb
Sub MultiCall(d As SimpleDelegate, count As Integer)
    Dim i As Integer

    For i = 0 To count - 1
        d()
    Next i
End Sub
```

Es ist unwichtig für die `MultiCall` Methode, wie die Ziel Methode für die `SimpleDelegate` ist, welche Barrierefreiheit diese Methode hat oder ob die Methode `Shared` ist oder nicht. Alles, was wichtig ist, ist, dass die Signatur der Ziel Methode mit `SimpleDelegate`kompatibel ist.


## <a name="partial-types"></a>Partial types (Partielle Typen)

Klassen-und Struktur Deklarationen können *partielle* Deklarationen sein. In einer partiellen Deklaration kann der deklarierte Typ innerhalb der Deklaration vollständig beschrieben werden. Stattdessen kann die Deklaration des Typs auf mehrere partielle Deklarationen innerhalb des Programms verteilt werden. Partielle Typen können nicht über Programm Grenzen hinweg deklariert werden. Eine partielle Typdeklaration gibt den `Partial` Modifizierer für die Deklaration an. Anschließend werden alle anderen Deklarationen im Programm für einen Typ mit demselben voll qualifizierten Namen zusammen mit der partiellen Deklaration zur Kompilierzeit zusammengeführt, um eine einzelne Typdeklaration zu bilden. Der folgende Code deklariert z. b. eine einzelne Klasse `Test` mit Membern `Test.C1` und `Test.C2`.

a. vb:

```vb
Public Partial Class Test
    Public Sub S1()
    End Sub
End Class
```

b. vb:

```vb
Public Class Test
    Public Sub S2()
    End Sub
End Class
```

Beim Kombinieren von partiellen Typdeklarationen muss mindestens eine der Deklarationen einen `Partial` Modifizierer aufweisen, andernfalls wird ein Kompilierzeitfehler ausgegeben.

__Nebenbei.__ Obwohl es möglich ist, `Partial` nur für eine Deklaration zwischen vielen partiellen Deklarationen anzugeben, ist es besser, es für alle partiellen Deklarationen anzugeben. In der Situation, in der eine partielle Deklaration sichtbar ist, aber eine oder mehrere partielle Deklarationen ausgeblendet sind (z. b. bei der Erweiterung des Tool generierten Codes), ist es akzeptabel, den `Partial` Modifizierer der sichtbaren Deklaration zu lassen, ihn aber in den verborgenen Deklarationen anzugeben.

Nur Klassen und Strukturen können mit partiellen Deklarationen deklariert werden. Die Stelligkeit eines Typs wird berücksichtigt, wenn partielle Deklarationen miteinander abgeglichen werden: zwei Klassen mit dem gleichen Namen, aber unterschiedlicher Anzahl von Typparametern werden nicht als partielle Deklarationen gleichzeitig betrachtet. Partielle Deklarationen können Attribute, Klassenmodifizierer, `Inherits` Anweisung oder `Implements` Anweisung angeben. Zum Zeitpunkt der Kompilierung werden alle Teile der partiellen Deklarationen zusammen kombiniert und als Teil der Typdeklaration verwendet. Wenn Konflikte zwischen Attributen, modifiziererelementen, Basen, Schnittstellen oder Typmembern auftreten, wird ein Fehler bei der Kompilierzeit ausgegeben. Beispiel:

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

Im vorherigen Beispiel wird ein Typ deklariert, `Test1` `Public`ist, von `Object` erbt und `System.IDisposable` und `System.IComparable`implementiert. Die partiellen Deklarationen von `Test2` verursachen einen Kompilierzeitfehler, da eine der Deklarationen besagt, dass `Test2` `Public` ist und ein anderer besagt, dass `Test2` `Private`ist.

Partielle Typen mit Typparametern können Einschränkungen und Varianz für die Typparameter deklarieren, aber die Einschränkungen und Varianz der einzelnen partiellen Deklarationen müssen stimmen. Daher sind Einschränkungen und Varianz speziell darin, dass Sie nicht automatisch wie andere Modifizierer kombiniert werden:

```vb
Partial Public Class List(Of T As IEnumerable)
End Class

' Error: Constraints on T don't match
Class List(Of T As IComparable)
End Class
```

Die Tatsache, dass ein Typ mit mehreren partiellen Deklarationen deklariert wird, hat keine Auswirkung auf die Regeln für die Namenssuche innerhalb des Typs. Demzufolge kann eine partielle Typdeklaration Member verwenden, die in anderen partiellen Typdeklarationen deklariert sind, oder Methoden für Schnittstellen implementieren, die in anderen partiellen Typdeklarationen deklariert werden. Beispiel:

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

In Form von Typen können auch partielle Deklarationen vorhanden sein. Beispiel:

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

Initialisierer in einer partiellen Deklaration werden weiterhin in der Deklarations Reihenfolge ausgeführt. Es gibt jedoch keine garantierte Ausführungsreihenfolge für Initialisierer, die in separaten partiellen Deklarationen auftreten.

## <a name="constructed-types"></a>Konstruierte Typen

Eine generische Typdeklaration bezeichnet allein keinen Typ. Stattdessen kann eine generische Typdeklaration als "Blueprint" verwendet werden, um viele verschiedene Typen durch Anwenden von Typargumenten zu bilden. Ein generischer Typ, auf den die Typargumente angewendet werden, wird als *konstruierter Typ*bezeichnet. Die Typargumente in einem konstruierten Typ müssen immer die Einschränkungen erfüllen, die auf die Typparameter angewendet werden, denen Sie entsprechen.

Ein Typname kann einen konstruierten Typ identifizieren, auch wenn er keine Typparameter direkt angibt. Dies kann vorkommen, wenn ein Typ in einer generischen Klassen Deklaration geschachtelt ist und der Instanztyp der enthaltenden Deklaration implizit für die Namenssuche verwendet wird:

```vb
Class Outer(Of T) 
    Public Class Inner 
    End Class

    ' Type of i is the constructed type Outer(Of T).Inner
    Public i As Inner 
End Class
```

Auf einen konstruierten Typ `C(Of T1,...,Tn)` auf den zugegriffen werden kann, wenn der generische Typ und alle Typargumente zugänglich sind. Wenn beispielsweise der generische Typ `C` `Public` ist und alle Typargumente `T1,...,Tn` `Public`sind, ist der konstruierte Typ `Public`. Wenn entweder der Typname oder eines der Typargumente `Private`ist, dann ist der Zugriff auf den konstruierten Typ `Private`. Wenn ein Typargument des konstruierten Typs `Protected` und ein anderes Typargument `Friend`ist, kann auf den konstruierten Typ nur in der-Klasse und den zugehörigen Unterklassen in dieser Assembly oder auf einer beliebigen Assembly zugegriffen werden, der `Friend` Zugriff erteilt wurde. Das heißt, die Zugriffs Domäne für einen konstruierten Typ ist die Schnittmenge der Barrierefreiheits Domänen der Bestandteile.

__Nebenbei.__ Die Tatsache, dass es sich bei der Zugriffsdomäne des konstruierten Typs um die Schnittmenge der gebildeten Teile handelt, hat den interessanten Nebeneffekt, dass eine neue Barrierefreiheits Stufe definiert Ein konstruierter Typ, der ein Element enthält, das `Protected` ist, und auf das ein Element `Friend` kann nur in Kontexten zugegriffen werden, die *sowohl* auf `Friend` *als auch* auf `Protected` Member zugreifen können. Allerdings gibt es keine Möglichkeit, diese Barrierefreiheits Stufe in der Sprache auszudrücken, da die Barrierefreiheits `Protected Friend` bedeutet, dass auf eine Entität in einem Kontext zugegriffen werden kann, der *auf `Friend`* *oder* `Protected` Member zugreifen kann.

Die Basis, die implementierten Schnittstellen und Member konstruierter Typen werden bestimmt, indem die angegebenen Typargumente für jedes Vorkommen des Typparameters im generischen Typ ersetzt werden.

### <a name="open-types-and-closed-types"></a>Öffnen von Typen und geschlossenen Typen

Ein konstruierter Typ, für den ein oder mehrere Typargumente Typparameter eines enthaltenden Typs oder einer Methode sind, wird als *offener Typ*bezeichnet. Dies liegt daran, dass einige Typparameter des Typs immer noch nicht bekannt sind, sodass die tatsächliche Form des Typs noch nicht vollständig bekannt ist. Im Gegensatz dazu wird ein generischer Typ, dessen Typargumente alle nicht-Typparameter sind, als *geschlossener Typ*bezeichnet. Die Form eines geschlossenen Typs ist immer vollständig bekannt. Beispiel:

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

Der erstellte Typ `Base(Of Integer, V)` ist ein offener Typ, da der Typparameter, `U` der `T` bereitgestellt wurde, ein weiterer Typparameter bereitgestellt wurde. Daher ist die vollständige Form des Typs noch nicht bekannt. Der konstruierte Typ `Derived(Of Double)`jedoch ein geschlossener Typ, da alle Typparameter in der Vererbungs Hierarchie bereitgestellt wurden.

Geöffnete Typen werden wie folgt definiert:

* Ein Typparameter ist ein offener Typ.

* Ein Arraytyp ist ein offener Typ, wenn sein Elementtyp ein offener Typ ist.

* Ein konstruierter Typ ist ein offener Typ, wenn mindestens eines der Typargumente ein offener Typ ist.

* Ein geschlossener Typ ist ein Typ, bei dem es sich nicht um einen geöffneten Typ handelt.

Da der Programm Einstiegspunkt nicht in einem generischen Typ vorliegen kann, sind alle zur Laufzeit verwendeten Typen geschlossene Typen.

## <a name="special-types"></a>Besondere Typen

Die .NET Framework enthält eine Reihe von Klassen, die speziell vom .NET Framework und der Visual Basic Sprache behandelt werden:

Der Typ `System.Void`, der einen void-Typ in der .NET Framework darstellt, kann nur in `GetType` Ausdrücken direkt referenziert werden.

Die Typen `System.RuntimeArgumentHandle`, `System.ArgIterator` und `System.TypedReference` alle können Zeiger auf den Stapel enthalten und können daher nicht auf dem .NET Framework Heap angezeigt werden. Daher können Sie nicht als Array Elementtypen, Rückgabe Typen, Feldtypen, generische Typargumente, Typen, die NULL-Werte zulassen, `ByRef` Parametertypen, den Typ eines Werts, der in `Object` oder `System.ValueType`konvertiert wird, das Ziel eines aufzurufenden Instanzmembers von `Object` oder `System.ValueType`oder eine Sperre in eine Sperre verwendet werden.
