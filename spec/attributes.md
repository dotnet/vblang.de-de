---
ms.openlocfilehash: 7286ee117cae82b03ebd92130383bd6051431100
ms.sourcegitcommit: 6eca149bdc736113e0adb709212bd266c9503c33
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2019
ms.locfileid: "47426676"
---
# <a name="attributes"></a>Attribute

Visual Basic-Sprache ermöglicht es den Programmierer, Modifizierer auf Deklarationen, geben Sie die Informationen zu den deklarierten Entitäten darstellen. Anbringung z. B. die Methode einer Klasse mit den Modifizierern `Public`, `Protected`, `Friend`, `Protected Friend`, oder `Private` gibt an, den Zugriff.

Zusätzlich zur die Modifizierer, die von der Programmiersprache definiert wird, ermöglicht Visual Basic auch Programmierer, die zum Erstellen von neuen Modifizierer, die Namen *Attribute*, und diese zu verwenden, wenn Sie neue Entitäten zu deklarieren. Diese neue Modifizierer, die durch die Deklaration von Attributklassen definiert sind, werden Entitäten über zugewiesen *Attributblöcke*.

__Beachten Sie.__ Attribute können zur Laufzeit durch die .NET Framework Reflektions-APIs abgerufen werden. Diese APIs sind außerhalb des Bereichs dieser Spezifikation.

Beispielsweise kann ein Framework definieren eine `Help` -Attribut, das auf Programmelemente wie Klassen und Methoden zur Bereitstellung einer Zuordnung von Programmelementen in der Dokumentation, platziert werden kann, wie das folgende Beispiel veranschaulicht:

```vb
<AttributeUsage(AttributeTargets.All)> _
Public Class HelpAttribute
    Inherits Attribute

    Public Sub New(urlValue As String)
        Me.UrlValue = urlValue
    End Sub

    Public Topic As String
    Private UrlValue As String

    Public ReadOnly Property Url() As String
        Get
            Return UrlValue
        End Get
    End Property
End Class
```

Das Beispiel definiert eine Attributklasse, die mit dem Namen `HelpAttribute`, oder `Help` kurz, hat, das ein Positionsparameter (`UrlValue`) und einen benannten Argument (`Topic`).

Das nächste Beispiel zeigt das Attribut verwendet:

```vb
<Help("http://www.example.com/.../Class1.htm")> _
Public Class Class1
    <Help("http://www.example.com/.../Class1.htm", Topic:="F")> _
    Public Sub F()
    End Sub
End Class
```

Im nächsten Beispiel wird überprüft, ob `Class1` verfügt über eine `Help` -Attribut, und schreibt das zugeordnete `Topic` und `Url` von Werten für das Attribut vorhanden ist.

```vb
Module Test
    Sub Main()
        Dim type As Type = GetType(Class1)
        Dim arr() As Object = _
            type.GetCustomAttributes(GetType(HelpAttribute), True)

        If arr.Length = 0 Then
            Console.WriteLine("Class1 has no Help attribute.")
        Else
            Dim ha As HelpAttribute = CType(arr(0), HelpAttribute)
            Console.WriteLine("Url = " & ha.Url & ", Topic = " & ha.Topic)
        End If
    End Sub
End Module
```

## <a name="attribute-classes"></a>Attributklassen

Ein *Attributklasse* ist eine nicht generische Klasse, die von abgeleitet `System.Attribute` und nicht `MustInherit`. Die Attributklasse verfügt möglicherweise über eine `System.AttributeUsage` Attribut, deklariert werden, was das Attribut auf gültig ist und gibt an, ob er mehrere Male in einer Deklaration verwendet werden kann, ob es geerbt wird. Das folgende Beispiel definiert eine Attributklasse, die mit dem Namen `SimpleAttribute` dar, die in den Klassendeklarationen und Schnittstellendeklarationen platziert werden kann:

```vb
<AttributeUsage(AttributeTargets.Class Or AttributeTargets.Interface)> _
Public Class SimpleAttribute
    Inherits System.Attribute
End Class
```

Das nächste Beispiel zeigt einige Anwendungsbereiche der der `Simple` Attribut. Obwohl die Attributklasse namens ist `SimpleAttribute`, verwendet dieses Attributs können weglassen, die `Attribute` Suffix, verkürzen Sie daher der Name, der `Simple`:

```vb
<Simple> Class Class1
End Class

<Simple> Interface Interface1
End Interface
```

Wenn das Attribut fehlt eine `System.AttributeUsage`, und klicken Sie dann das Attribut für jedes beliebige Ziel platziert werden kann (entspricht `AttributeTargets.All`). Die `System.AttributeUsage` Attribut wurde einem Variableninitialisierer, `AllowMultiple`, der angibt, ob das angegebene Attribut mehr als einmal für eine bestimmte Deklaration angegeben werden kann. Wenn `AllowMultiple` für ein Attribut `True`, es ist ein *mehrfach verwendbare Attributklasse*, und kann mehr als einmal in einer Deklaration angegeben werden. Wenn `AllowMultiple` für ein Attribut `False` oder ohne Angabe eines Attributs ist eine *einmaligen Verwendung Attributklasse*, und kann höchstens einmal in einer Deklaration angegeben werden.

Das folgende Beispiel definiert eine mehrfach verwendbare Attributklasse namens `AuthorAttribute`:

```vb
<AttributeUsage(AttributeTargets.Class, AllowMultiple:=True)> _
Public Class AuthorAttribute
    Inherits System.Attribute

    Private _Value As String

    Public Sub New(value As String)
        Me._Value = value
    End Sub

    Public ReadOnly Property Value() As String
        Get
            Return _Value
        End Get
    End Property
End Class
```

Das Beispiel zeigt eine Klassendeklaration mit zwei Verwendungen von der `Author` Attribut:

```vb
<Author("Maria Hammond"), Author("Ramesh Meyyappan")> _
Class Class1
End Class
```

Die `System.AttributeUsage` Attribut verfügt über eine öffentliche Instanz-Variable, `Inherited`, das angibt, ob das Attribut, wenn Sie in ein Basistyp angegeben von Typen, die von diesem Basistyp abgeleitet werden, auch geerbt wird. Wenn die `Inherited` öffentliche Variable wurde nicht initialisiert werden, den Standardwert `False` verwendet wird. Eigenschaften und Ereignisse erben Attribute, nicht, obwohl die Methoden, die durch Eigenschaften und Ereignisse definiert werden. Schnittstellen erben keine Attribute.

Wenn ein Attribut zur einmaligen Nutzung sowohl geerbt und für einen abgeleiteten Typ angegeben, überschreibt das Dateiattribut für den abgeleiteten Typ das geerbte Attribut an. Wenn ein Attribut mehrfach verwendbare sowohl geerbt und für einen abgeleiteten Typ angegeben, werden beide Attribute in den abgeleiteten Typ angegeben. Zum Beispiel:

```vb
<AttributeUsage(AttributeTargets.Class, AllowMultiple:=True, _
                Inherited:=True) > _
Class MultiUseAttribute
    Inherits System.Attribute

    Public Sub New(value As Boolean)
    End Sub
End Class

<AttributeUsage(AttributeTargets.Class, Inherited:=True)> _
Class SingleUseAttribute
    Inherits Attribute

    Public Sub New(value As Boolean)
    End Sub
End Class

<SingleUse(True), MultiUse(True)> Class Base
End Class

' Derived has three attributes defined on it: SingleUse(False),
' MultiUse(True) and MultiUse(False)
<SingleUse(False), MultiUse(False)> _
Class Derived
    Inherits Base
End Class
```

Die Positionsparameter des Attributs werden durch die Parameter des öffentlichen Konstruktoren einer Attributklasse definiert. Positionsparameter muss `ByVal` und möglicherweise keinen `ByRef`. Öffentliche Variablen und Eigenschaften werden vom öffentlichen Lese-/ Schreibeigenschaften oder Instanzvariablen der Attributklasse definiert. Die Typen, die in der Positionsparameter und öffentliche Variablen und Eigenschaften verwendet werden können, sind auf Attributtypen beschränkt. Ein Typ ist ein Typ des Attributs ist dies eine der folgenden:

* Alle primitiven Typen mit Ausnahme von `Date` und `Decimal`.

* Typ `Object`.

* Typ `System.Type`.

* Ein enumerierter Typ, vorausgesetzt, dass sie und die Typen, die in der geschachtelt ist (sofern vorhanden) verfügen über `Public` Barrierefreiheit.

* Ein eindimensionales Array von einem der vorherigen Typen in dieser Liste.

## <a name="attribute-blocks"></a>Attributblöcke

Attribute werden in angegeben *Attributblöcke*. Jedes Attributblock wird getrennt durch die spitzen Klammern ("<>"), und mehrere Attribute können in einer durch Trennzeichen getrennte Liste in einem Attributblock oder in mehreren Attributblöcke angegeben werden. Die Reihenfolge, in der Attribute angegeben werden, ist nicht erheblich. Z. B. die Attributblöcke `<A, B>`, `<B, A>`, `<A> <B>` und `<B> <A>` alle gleich sind.

```antlr
Attributes
    : AttributeBlock+
    ;

AttributeBlock
    : LineTerminator? '<' AttributeList LineTerminator? '>' LineTerminator?
    ;

AttributeList
    : Attribute ( Comma Attribute )*
    ;

Attribute
    : ( AttributeModifier ':' )? SimpleTypeName
    ( OpenParenthesis AttributeArguments? CloseParenthesis )?
    ;

AttributeModifier
    : 'Assembly' | 'Module'
    ;
```

Ein Attribut kann nicht angegeben werden, auf eine Art der Deklaration, die dies nicht der Fall, Support und einmaligen Verwendung Attribute nicht mehr als einmal in einem Attributblock möglicherweise angegeben werden. Im folgenden Beispiel bewirkt, dass sowohl Fehler, da versucht wird, verwenden Sie `HelpString` für die Schnittstelle `Interface1` und mehr als einmal in der Deklaration der `Class1`.

```vb
<AttributeUsage(AttributeTargets.Class)> _
Public Class HelpStringAttribute
    Inherits System.Attribute

    Private InternalValue As String

    Public Sub New(value As String)
        Me.InternalValue = value
    End Sub

    Public ReadOnly Property Value() As String
        Get
            Return InternalValue
        End Get
    End Property
End Class

' Error: HelpString only applies to classes.
<HelpString("Description of Interface1")> _
Interface Interface1
    Sub Sub1()
End Interface

' Error: HelpString is single-use.
<HelpString("Description of Class1"), _
    HelpString("Another description of Class1")> _
Public Class Class1
End Class
```

Ein optionales Attribut-Modifizierer, die einen Attributnamen an, die eine optionale Liste von positionellen Argumenten und Variablen/Eigenschaft-Initialisierer besteht ein Attribut aus. Wenn keine Parameter oder einen Initialisierer vorhanden sind, können die Klammern weggelassen werden. Wenn ein Attribut einen Modifizierer verfügt, muss sie in einem Attributblock am Anfang einer Quelldatei.

Wenn eine Quelldatei einen Attributblock am Anfang der Datei, der angibt, die Attribute für die Assembly oder das Modul, das die Quelldatei enthält enthält, alle Attribute in dem Attributblock muss das Präfix sowohl die `Assembly` oder `Module` Modifizierer und ein Doppelpunkt.


### <a name="attribute-names"></a>Die Namen der Attribute

Der Name eines Attributs gibt eine Attributklasse an. Attributklassen heißen gemäß der Konvention, mit dem Suffix `Attribute`. Ein Attribut können entweder ein- oder lassen Sie das Suffix. Daher ist der Name der Attributklasse, die einen Attributbezeichner entspricht, entweder den Bezeichner oder die Verkettung von qualifizierten Bezeichner und `Attribute`. Wenn der Compiler einen Attributnamen aufgelöst wird, fügt es `Attribute` auf den Namen und die Suche nach versucht. Wenn es sich bei dieser Suche ein Fehler auftritt, versucht der Compiler die Suche ohne das Suffix. Verwendet z. B. einer Attributklasse `SimpleAttribute` können weglassen der `Attribute` Suffix, verkürzen Sie daher der Name, der `Simple`:

```vb
<Simple> Class Class1
End Class

<Simple> Interface Interface1
End Interface
```

Im obigen Beispiel ist semantisch äquivalent zu folgendem:

```vb
<SimpleAttribute> Class Class1
End Class

<SimpleAttribute> Interface Interface1
End Interface
```

Im allgemeinen Attribute mit dem Namen mit dem Suffix `Attribute` , als bevorzugt eingestuft. Das folgende Beispiel zeigt zwei Attributklassen, die mit dem Namen `T` und `T``Attribute`.

```vb
<AttributeUsage(AttributeTargets.All)> _
Public Class T
    Inherits System.Attribute
End Class

<AttributeUsage(AttributeTargets.All)> _
Public Class TAttribute
    Inherits System.Attribute
End Class

' Refers to TAttribute.
<T> Class Class1 
End Class

' Refers to TAttribute.
<TAttribute> Class Class2 
End Class
```

Beide dem Attributblock `<T>` und dem Attributblock `<TAttribute>` finden Sie in der Attributklasse, die mit dem Namen `TAttribute`. Es ist nicht möglich, verwenden Sie `T` als ein Attribut, bis Sie die Deklaration für Klasse entfernen `TAttribute`.

### <a name="attribute-arguments"></a>Aufruferinfoattribut-Argumente

Argumente für ein Attribut können zwei Formen annehmen: *positionelle Argumente* und *Instanz Variable bzw. eine Eigenschaft-Initialisierer*. Keine positionellen Argumente für das Attribut müssen die Instanz-Variable bzw. eine Eigenschaft-Initialisierer vorangestellt sein. Ein Positionsargument besteht aus einem konstanten Ausdruck, ein eindimensionales Array-Creation-Ausdruck oder ein `GetType` Ausdruck. Ein Variable bzw. eine Eigenschaft-Initialisierer Instanz besteht aus einem Bezeichner, der Schlüsselwörter, gefolgt von einem Doppelpunkt und dem Gleichheitszeichen und beendet, die durch einen konstanten Ausdruck verglichen werden kann oder ein `GetType` Ausdruck.

Erhalten ein Attribut mit der Attributklasse `T`, mit Feldern fester Breite Argumentliste `P`, und die Instanzliste Variable bzw. eine Eigenschaft-Initialisierer `N`, diese Schritte zu bestimmen, ob die Argumente gültig sind:

1. Führen Sie die während der Kompilierung Verarbeitungsschritte für die Kompilierung von eines Ausdrucks der Form `New T(P)`. Dies führt zu einem Fehler während der Kompilierung oder einen Konstruktor wird bestimmt, auf `T` , die an die Argumentliste anwendbar ist.

2. Wenn der Konstruktor, der bestimmt, die in Schritt 1 verfügt über Parameter, die nicht Attributtypen sind oder ist nicht zugegriffen werden kann, auf der Website für die Deklaration, tritt ein Fehler während der Kompilierung.

3. Für jede Instanz Variable bzw. eine Eigenschaft-Initialisierer `Arg` in `N`ermöglichen `Name` der Bezeichner von der Instanz-Variable bzw. eine Eigenschaft-Initialisierer `Arg`. `Name` identifizieren müssen einer nicht -`Shared`, beschreibbar `Public` Instanz-Variable oder parameterlosen-Eigenschaft in `T` , dessen Typ ist ein Typ des Attributs. Wenn `T` hat keine solche Instanzvariable oder die Eigenschaft ein Fehler während der Kompilierung auftritt.

Zum Beispiel:

```vb
<AttributeUsage(AttributeTargets.All)> _
Public Class GeneralAttribute
    Inherits Attribute

    Public Sub New(x As Integer)
    End Sub

    Public Sub New(x As Double)
    End Sub

    Public y As Type

    Public Property z As Integer
        Get
        End Get

        Set
        End Set
    End Property
End Class

' Calls the first constructor.
<General(10, z:=30, y:=GetType(Integer))> _
Class C1
End Class

' Calls the second constructor.
<General(10.5, z:=10)> _
Class C2
End Class
```

Typparameter können nicht an einer beliebigen Stelle in Attributargumenten verwendet werden. Allerdings können konstruierte Typen verwendet werden:

```vb
<AttributeUsage(AttributeTargets.All)> _
Class A 
   Inherits System.Attribute 

   Public Sub New(t As Type)
   End Sub 
End Class

Class List(Of T) 
    ' Error: attribute argument cannot use type parameter
    <A(GetType(T))> Dim t1 As T 

    ' OK: closed type
    <A(GetType(List(Of Integer)))> Dim y As Integer
End Class
```


```antlr
AttributeArguments
    : AttributePositionalArgumentList
    | AttributePositionalArgumentList Comma VariablePropertyInitializerList
    | VariablePropertyInitializerList
    ;

AttributePositionalArgumentList
    : AttributeArgumentExpression? ( Comma AttributeArgumentExpression? )*
    ;

VariablePropertyInitializerList
    : VariablePropertyInitializer ( Comma VariablePropertyInitializer )*
    ;

VariablePropertyInitializer
    : IdentifierOrKeyword ColonEquals AttributeArgumentExpression
    ;

AttributeArgumentExpression
    : ConstantExpression
    | GetTypeExpression
    | ArrayExpression
    ;
```