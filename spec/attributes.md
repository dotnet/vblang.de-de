# <a name="attributes"></a><span data-ttu-id="7f588-101">Attribute</span><span class="sxs-lookup"><span data-stu-id="7f588-101">Attributes</span></span>

<span data-ttu-id="7f588-102">Visual Basic-Sprache ermöglicht es den Programmierer, Modifizierer auf Deklarationen, geben Sie die Informationen zu den deklarierten Entitäten darstellen.</span><span class="sxs-lookup"><span data-stu-id="7f588-102">The Visual Basic language enables the programmer to specify modifiers on declarations, which represent information about the entities being declared.</span></span> <span data-ttu-id="7f588-103">Anbringung z. B. die Methode einer Klasse mit den Modifizierern `Public`, `Protected`, `Friend`, `Protected Friend`, oder `Private` gibt an, den Zugriff.</span><span class="sxs-lookup"><span data-stu-id="7f588-103">For example, affixing a class method with the modifiers `Public`, `Protected`, `Friend`, `Protected Friend`, or `Private` specifies its accessibility.</span></span>

<span data-ttu-id="7f588-104">Zusätzlich zur die Modifizierer, die von der Programmiersprache definiert wird, ermöglicht Visual Basic auch Programmierer, die zum Erstellen von neuen Modifizierer, die Namen *Attribute*, und diese zu verwenden, wenn Sie neue Entitäten zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="7f588-104">In addition to the modifiers defined by the language, Visual Basic also enables programmers to create new modifiers, called *attributes*, and to use them when declaring new entities.</span></span> <span data-ttu-id="7f588-105">Diese neue Modifizierer, die durch die Deklaration von Attributklassen definiert sind, werden Entitäten über zugewiesen *Attributblöcke*.</span><span class="sxs-lookup"><span data-stu-id="7f588-105">These new modifiers, which are defined through the declaration of attribute classes, are then assigned to entities through *attribute blocks*.</span></span>

<span data-ttu-id="7f588-106">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="7f588-106">__Note.__</span></span> <span data-ttu-id="7f588-107">Attribute können zur Laufzeit durch die .NET Framework Reflektions-APIs abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="7f588-107">Attributes may be retrieved at run time through the .NET Framework's reflection APIs.</span></span> <span data-ttu-id="7f588-108">Diese APIs sind außerhalb des Bereichs dieser Spezifikation.</span><span class="sxs-lookup"><span data-stu-id="7f588-108">These APIs are outside the scope of this specification.</span></span>

<span data-ttu-id="7f588-109">Beispielsweise kann ein Framework definieren eine `Help` -Attribut, das auf Programmelemente wie Klassen und Methoden zur Bereitstellung einer Zuordnung von Programmelementen in der Dokumentation, platziert werden kann, wie das folgende Beispiel veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="7f588-109">For instance, a framework might define a `Help` attribute that can be placed on program elements such as classes and methods to provide a mapping from program elements to documentation, as the following example demonstrates:</span></span>

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

<span data-ttu-id="7f588-110">Das Beispiel definiert eine Attributklasse, die mit dem Namen `HelpAttribute`, oder `Help` kurz, hat, das ein Positionsparameter (`UrlValue`) und einen benannten Argument (`Topic`).</span><span class="sxs-lookup"><span data-stu-id="7f588-110">The example defines an attribute class named `HelpAttribute`, or `Help` for short, that has one positional parameter (`UrlValue`) and one named argument (`Topic`).</span></span>

<span data-ttu-id="7f588-111">Das nächste Beispiel zeigt das Attribut verwendet:</span><span class="sxs-lookup"><span data-stu-id="7f588-111">The next example shows several uses of the attribute:</span></span>

```vb
<Help("http://www.example.com/.../Class1.htm")> _
Public Class Class1
    <Help("http://www.example.com/.../Class1.htm", Topic:="F")> _
    Public Sub F()
    End Sub
End Class
```

<span data-ttu-id="7f588-112">Im nächsten Beispiel wird überprüft, ob `Class1` verfügt über eine `Help` -Attribut, und schreibt das zugeordnete `Topic` und `Url` von Werten für das Attribut vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="7f588-112">The next example checks to see if `Class1` has a `Help` attribute, and writes out the associated `Topic` and `Url` values if the attribute is present.</span></span>

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

## <a name="attribute-classes"></a><span data-ttu-id="7f588-113">Attributklassen</span><span class="sxs-lookup"><span data-stu-id="7f588-113">Attribute Classes</span></span>

<span data-ttu-id="7f588-114">Ein *Attributklasse* ist eine nicht generische Klasse, die von abgeleitet `System.Attribute` und nicht `MustInherit`.</span><span class="sxs-lookup"><span data-stu-id="7f588-114">An *attribute class* is a non-generic class that derives from `System.Attribute` and is not `MustInherit`.</span></span> <span data-ttu-id="7f588-115">Die Attributklasse verfügt möglicherweise über eine `System.AttributeUsage` Attribut, deklariert werden, was das Attribut auf gültig ist und gibt an, ob er mehrere Male in einer Deklaration verwendet werden kann, ob es geerbt wird.</span><span class="sxs-lookup"><span data-stu-id="7f588-115">The attribute class may have a `System.AttributeUsage` attribute that declares what the attribute is valid on, whether it may be used multiple times in a declaration, and whether it is inherited.</span></span> <span data-ttu-id="7f588-116">Das folgende Beispiel definiert eine Attributklasse, die mit dem Namen `SimpleAttribute` dar, die in den Klassendeklarationen und Schnittstellendeklarationen platziert werden kann:</span><span class="sxs-lookup"><span data-stu-id="7f588-116">The following example defines an attribute class named `SimpleAttribute` that can be placed on class declarations and interface declarations:</span></span>

```vb
<AttributeUsage(AttributeTargets.Class Or AttributeTargets.Interface)> _
Public Class SimpleAttribute
    Inherits System.Attribute
End Class
```

<span data-ttu-id="7f588-117">Das nächste Beispiel zeigt einige Anwendungsbereiche der der `Simple` Attribut.</span><span class="sxs-lookup"><span data-stu-id="7f588-117">The next example shows a few uses of the `Simple` attribute.</span></span> <span data-ttu-id="7f588-118">Obwohl die Attributklasse namens ist `SimpleAttribute`, verwendet dieses Attributs können weglassen, die `Attribute` Suffix, verkürzen Sie daher der Name, der `Simple`:</span><span class="sxs-lookup"><span data-stu-id="7f588-118">Although the attribute class is named `SimpleAttribute`, uses of this attribute may omit the `Attribute` suffix, thus shortening the name to `Simple`:</span></span>

```vb
<Simple> Class Class1
End Class

<Simple> Interface Interface1
End Interface
```

<span data-ttu-id="7f588-119">Wenn das Attribut fehlt eine `System.AttributeUsage`, und klicken Sie dann das Attribut für jedes beliebige Ziel platziert werden kann (entspricht `AttributeTargets.All`).</span><span class="sxs-lookup"><span data-stu-id="7f588-119">If the attribute lacks a `System.AttributeUsage`, then the attribute can be placed on any target (equivalent to `AttributeTargets.All`).</span></span> <span data-ttu-id="7f588-120">Die `System.AttributeUsage` Attribut wurde einem Variableninitialisierer, `AllowMultiple`, der angibt, ob das angegebene Attribut mehr als einmal für eine bestimmte Deklaration angegeben werden kann.</span><span class="sxs-lookup"><span data-stu-id="7f588-120">The `System.AttributeUsage` attribute has a variable initializer, `AllowMultiple`, which specifies whether the indicated attribute can be specified more than once for a given declaration.</span></span> <span data-ttu-id="7f588-121">Wenn `AllowMultiple` für ein Attribut `True`, es ist ein *mehrfach verwendbare Attributklasse*, und kann mehr als einmal in einer Deklaration angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="7f588-121">If `AllowMultiple` for an attribute is `True`, it is a *multiple-use attribute class*, and can be specified more than once on a declaration.</span></span> <span data-ttu-id="7f588-122">Wenn `AllowMultiple` für ein Attribut `False` oder ohne Angabe eines Attributs ist eine *einmaligen Verwendung Attributklasse*, und kann höchstens einmal in einer Deklaration angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="7f588-122">If `AllowMultiple` for an attribute is `False` or unspecified for an attribute, it is a *single-use attribute class*, and can be specified at most once on a declaration.</span></span>

<span data-ttu-id="7f588-123">Das folgende Beispiel definiert eine mehrfach verwendbare Attributklasse namens `AuthorAttribute`:</span><span class="sxs-lookup"><span data-stu-id="7f588-123">The following example defines a multiple-use attribute class named `AuthorAttribute`:</span></span>

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

<span data-ttu-id="7f588-124">Das Beispiel zeigt eine Klassendeklaration mit zwei Verwendungen von der `Author` Attribut:</span><span class="sxs-lookup"><span data-stu-id="7f588-124">The example shows a class declaration with two uses of the `Author` attribute:</span></span>

```vb
<Author("Maria Hammond"), Author("Ramesh Meyyappan")> _
Class Class1
End Class
```

<span data-ttu-id="7f588-125">Die `System.AttributeUsage` Attribut verfügt über eine öffentliche Instanz-Variable, `Inherited`, das angibt, ob das Attribut, wenn Sie in ein Basistyp angegeben von Typen, die von diesem Basistyp abgeleitet werden, auch geerbt wird.</span><span class="sxs-lookup"><span data-stu-id="7f588-125">The `System.AttributeUsage` attribute has a public instance variable, `Inherited`, that specifies whether the attribute, when specified on a base type, is also inherited by types that derive from this base type.</span></span> <span data-ttu-id="7f588-126">Wenn die `Inherited` öffentliche Variable wurde nicht initialisiert werden, den Standardwert `False` verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="7f588-126">If the `Inherited` public instance variable is not initialized, a default value of `False` is used.</span></span> <span data-ttu-id="7f588-127">Eigenschaften und Ereignisse erben Attribute, nicht, obwohl die Methoden, die durch Eigenschaften und Ereignisse definiert werden.</span><span class="sxs-lookup"><span data-stu-id="7f588-127">Properties and events do not inherit attributes, although the methods defined by properties and events do.</span></span> <span data-ttu-id="7f588-128">Schnittstellen erben keine Attribute.</span><span class="sxs-lookup"><span data-stu-id="7f588-128">Interfaces do not inherit attributes.</span></span>

<span data-ttu-id="7f588-129">Wenn ein Attribut zur einmaligen Nutzung sowohl geerbt und für einen abgeleiteten Typ angegeben, überschreibt das Dateiattribut für den abgeleiteten Typ das geerbte Attribut an.</span><span class="sxs-lookup"><span data-stu-id="7f588-129">If a single-use attribute is both inherited and specified on a derived type, the attribute specified on the derived type overrides the inherited attribute.</span></span> <span data-ttu-id="7f588-130">Wenn ein Attribut mehrfach verwendbare sowohl geerbt und für einen abgeleiteten Typ angegeben, werden beide Attribute in den abgeleiteten Typ angegeben.</span><span class="sxs-lookup"><span data-stu-id="7f588-130">If a multiple-use attribute is both inherited and specified on a derived type, both attributes are specified on the derived type.</span></span> <span data-ttu-id="7f588-131">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="7f588-131">For example:</span></span>

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

<span data-ttu-id="7f588-132">Die Positionsparameter des Attributs werden durch die Parameter des öffentlichen Konstruktoren einer Attributklasse definiert.</span><span class="sxs-lookup"><span data-stu-id="7f588-132">The positional parameters of the attribute are defined by the parameters of the public constructors of the attribute class.</span></span> <span data-ttu-id="7f588-133">Positionsparameter muss `ByVal` und möglicherweise keinen `ByRef`.</span><span class="sxs-lookup"><span data-stu-id="7f588-133">Positional parameters must be `ByVal` and may not specify `ByRef`.</span></span> <span data-ttu-id="7f588-134">Öffentliche Variablen und Eigenschaften werden vom öffentlichen Lese-/ Schreibeigenschaften oder Instanzvariablen der Attributklasse definiert.</span><span class="sxs-lookup"><span data-stu-id="7f588-134">Public instance variables and properties are defined by public read-write properties or instance variables of the attribute class.</span></span> <span data-ttu-id="7f588-135">Die Typen, die in der Positionsparameter und öffentliche Variablen und Eigenschaften verwendet werden können, sind auf Attributtypen beschränkt.</span><span class="sxs-lookup"><span data-stu-id="7f588-135">The types that can be used in positional parameters and public instance variables and properties are restricted to attribute types.</span></span> <span data-ttu-id="7f588-136">Ein Typ ist ein Typ des Attributs ist dies eine der folgenden:</span><span class="sxs-lookup"><span data-stu-id="7f588-136">A type is an attribute type if it is one of the following:</span></span>

* <span data-ttu-id="7f588-137">Alle primitiven Typen mit Ausnahme von `Date` und `Decimal`.</span><span class="sxs-lookup"><span data-stu-id="7f588-137">Any primitive type except for `Date` and `Decimal`.</span></span>

* <span data-ttu-id="7f588-138">Typ `Object`.</span><span class="sxs-lookup"><span data-stu-id="7f588-138">The type `Object`.</span></span>

* <span data-ttu-id="7f588-139">Typ `System.Type`.</span><span class="sxs-lookup"><span data-stu-id="7f588-139">The type `System.Type`.</span></span>

* <span data-ttu-id="7f588-140">Ein enumerierter Typ, vorausgesetzt, dass sie und die Typen, die in der geschachtelt ist (sofern vorhanden) verfügen über `Public` Barrierefreiheit.</span><span class="sxs-lookup"><span data-stu-id="7f588-140">An enumerated type, provided that it and the types in which it is nested (if any) have `Public` accessibility.</span></span>

* <span data-ttu-id="7f588-141">Ein eindimensionales Array von einem der vorherigen Typen in dieser Liste.</span><span class="sxs-lookup"><span data-stu-id="7f588-141">A one-dimensional array of one of the previous types in this list.</span></span>

## <a name="attribute-blocks"></a><span data-ttu-id="7f588-142">Attributblöcke</span><span class="sxs-lookup"><span data-stu-id="7f588-142">Attribute Blocks</span></span>

<span data-ttu-id="7f588-143">Attribute werden in angegeben *Attributblöcke*.</span><span class="sxs-lookup"><span data-stu-id="7f588-143">Attributes are specified in *attribute blocks*.</span></span> <span data-ttu-id="7f588-144">Jedes Attributblock wird getrennt durch die spitzen Klammern ("<>"), und mehrere Attribute können in einer durch Trennzeichen getrennte Liste in einem Attributblock oder in mehreren Attributblöcke angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="7f588-144">Each attribute block is delimited by angle brackets ("<>"), and multiple attributes can be specified in a comma-separated list within an attribute block or in multiple attribute blocks.</span></span> <span data-ttu-id="7f588-145">Die Reihenfolge, in der Attribute angegeben werden, ist nicht erheblich.</span><span class="sxs-lookup"><span data-stu-id="7f588-145">The order in which attributes are specified is not significant.</span></span> <span data-ttu-id="7f588-146">Z. B. die Attributblöcke `<A, B>`, `<B, A>`, `<A> <B>` und `<B> <A>` alle gleich sind.</span><span class="sxs-lookup"><span data-stu-id="7f588-146">For example, the attribute blocks `<A, B>`, `<B, A>`, `<A> <B>` and `<B> <A>` are all equivalent.</span></span>

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

<span data-ttu-id="7f588-147">Ein Attribut kann nicht angegeben werden, auf eine Art der Deklaration, die dies nicht der Fall, Support und einmaligen Verwendung Attribute nicht mehr als einmal in einem Attributblock möglicherweise angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="7f588-147">An attribute may not be specified on a kind of declaration it does not support, and single-use attributes may not be specified more than once in an attribute block.</span></span> <span data-ttu-id="7f588-148">Im folgenden Beispiel bewirkt, dass sowohl Fehler, da versucht wird, verwenden Sie `HelpString` für die Schnittstelle `Interface1` und mehr als einmal in der Deklaration der `Class1`.</span><span class="sxs-lookup"><span data-stu-id="7f588-148">The example below causes errors both because it attempts to use `HelpString` on the interface `Interface1` and more than once on the declaration of `Class1`.</span></span>

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

<span data-ttu-id="7f588-149">Ein optionales Attribut-Modifizierer, die einen Attributnamen an, die eine optionale Liste von positionellen Argumenten und Variablen/Eigenschaft-Initialisierer besteht ein Attribut aus.</span><span class="sxs-lookup"><span data-stu-id="7f588-149">An attribute consists of an optional attribute modifier, an attribute name, an optional list of positional arguments, and variable/property initializers.</span></span> <span data-ttu-id="7f588-150">Wenn keine Parameter oder einen Initialisierer vorhanden sind, können die Klammern weggelassen werden.</span><span class="sxs-lookup"><span data-stu-id="7f588-150">If there are no parameters or initializers, the parentheses may be omitted.</span></span> <span data-ttu-id="7f588-151">Wenn ein Attribut einen Modifizierer verfügt, muss sie in einem Attributblock am Anfang einer Quelldatei.</span><span class="sxs-lookup"><span data-stu-id="7f588-151">If an attribute has a modifier, it must be in an attribute block at the top of a source file.</span></span>

<span data-ttu-id="7f588-152">Wenn eine Quelldatei einen Attributblock am Anfang der Datei, der angibt, die Attribute für die Assembly oder das Modul, das die Quelldatei enthält enthält, alle Attribute in dem Attributblock muss das Präfix sowohl die `Assembly` oder `Module` Modifizierer und ein Doppelpunkt.</span><span class="sxs-lookup"><span data-stu-id="7f588-152">If a source file contains an attribute block at the top of the file that specifies attributes for the assembly or module that will contain the source file, each attribute in the attribute block must be prefixed by both the `Assembly` or `Module` modifier and a colon.</span></span>


### <a name="attribute-names"></a><span data-ttu-id="7f588-153">Die Namen der Attribute</span><span class="sxs-lookup"><span data-stu-id="7f588-153">Attribute Names</span></span>

<span data-ttu-id="7f588-154">Der Name eines Attributs gibt eine Attributklasse an.</span><span class="sxs-lookup"><span data-stu-id="7f588-154">The name of an attribute specifies an attribute class.</span></span> <span data-ttu-id="7f588-155">Attributklassen heißen gemäß der Konvention, mit dem Suffix `Attribute`.</span><span class="sxs-lookup"><span data-stu-id="7f588-155">By convention, attribute classes are named with the suffix `Attribute`.</span></span> <span data-ttu-id="7f588-156">Ein Attribut können entweder ein- oder lassen Sie das Suffix.</span><span class="sxs-lookup"><span data-stu-id="7f588-156">Uses of an attribute may either include or omit this suffix.</span></span> <span data-ttu-id="7f588-157">Daher ist der Name der Attributklasse, die einen Attributbezeichner entspricht, entweder den Bezeichner oder die Verkettung von qualifizierten Bezeichner und `Attribute`.</span><span class="sxs-lookup"><span data-stu-id="7f588-157">Consequently the name of an attribute class that corresponds to an attribute identifier is either the identifier itself or the concatenation of the qualified identifier and `Attribute`.</span></span> <span data-ttu-id="7f588-158">Wenn der Compiler einen Attributnamen aufgelöst wird, fügt es `Attribute` auf den Namen und die Suche nach versucht.</span><span class="sxs-lookup"><span data-stu-id="7f588-158">When the compiler resolves an attribute name, it appends `Attribute` to the name and tries the lookup.</span></span> <span data-ttu-id="7f588-159">Wenn es sich bei dieser Suche ein Fehler auftritt, versucht der Compiler die Suche ohne das Suffix.</span><span class="sxs-lookup"><span data-stu-id="7f588-159">If that lookup fails, the compiler tries the lookup without the suffix.</span></span> <span data-ttu-id="7f588-160">Verwendet z. B. einer Attributklasse `SimpleAttribute` können weglassen der `Attribute` Suffix, verkürzen Sie daher der Name, der `Simple`:</span><span class="sxs-lookup"><span data-stu-id="7f588-160">For example, uses of an attribute class `SimpleAttribute` may omit the `Attribute` suffix, thus shortening the name to `Simple`:</span></span>

```vb
<Simple> Class Class1
End Class

<Simple> Interface Interface1
End Interface
```

<span data-ttu-id="7f588-161">Im obigen Beispiel ist semantisch äquivalent zu folgendem:</span><span class="sxs-lookup"><span data-stu-id="7f588-161">The example above is semantically equivalent to the following:</span></span>

```vb
<SimpleAttribute> Class Class1
End Class

<SimpleAttribute> Interface Interface1
End Interface
```

<span data-ttu-id="7f588-162">Im allgemeinen Attribute mit dem Namen mit dem Suffix `Attribute` , als bevorzugt eingestuft.</span><span class="sxs-lookup"><span data-stu-id="7f588-162">In general, attributes named with the suffix `Attribute` are preferred.</span></span> <span data-ttu-id="7f588-163">Das folgende Beispiel zeigt zwei Attributklassen, die mit dem Namen `T` und `T``Attribute`.</span><span class="sxs-lookup"><span data-stu-id="7f588-163">The following example shows two attribute classes named `T` and `T``Attribute`.</span></span>

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

<span data-ttu-id="7f588-164">Beide dem Attributblock `<T>` und dem Attributblock `<TAttribute>` finden Sie in der Attributklasse, die mit dem Namen `TAttribute`.</span><span class="sxs-lookup"><span data-stu-id="7f588-164">Both the attribute block `<T>` and the attribute block `<TAttribute>` refer to the attribute class named `TAttribute`.</span></span> <span data-ttu-id="7f588-165">Es ist nicht möglich, verwenden Sie `T` als ein Attribut, bis Sie die Deklaration für Klasse entfernen `TAttribute`.</span><span class="sxs-lookup"><span data-stu-id="7f588-165">It is not possible to use `T` as an attribute until you remove the declaration for class `TAttribute`.</span></span>

### <a name="attribute-arguments"></a><span data-ttu-id="7f588-166">Aufruferinfoattribut-Argumente</span><span class="sxs-lookup"><span data-stu-id="7f588-166">Attribute Arguments</span></span>

<span data-ttu-id="7f588-167">Argumente für ein Attribut können zwei Formen annehmen: *positionelle Argumente* und *Instanz Variable bzw. eine Eigenschaft-Initialisierer*.</span><span class="sxs-lookup"><span data-stu-id="7f588-167">Arguments to an attribute may take two forms: *positional arguments* and *instance variable/property initializers*.</span></span> <span data-ttu-id="7f588-168">Keine positionellen Argumente für das Attribut müssen die Instanz-Variable bzw. eine Eigenschaft-Initialisierer vorangestellt sein.</span><span class="sxs-lookup"><span data-stu-id="7f588-168">Any positional arguments to the attribute must precede the instance variable/property initializers.</span></span> <span data-ttu-id="7f588-169">Ein Positionsargument besteht aus einem konstanten Ausdruck, ein eindimensionales Array-Creation-Ausdruck oder ein `GetType` Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="7f588-169">A positional argument consists of a constant expression, a one-dimensional array-creation expression or a `GetType` expression.</span></span> <span data-ttu-id="7f588-170">Ein Variable bzw. eine Eigenschaft-Initialisierer Instanz besteht aus einem Bezeichner, der Schlüsselwörter, gefolgt von einem Doppelpunkt und dem Gleichheitszeichen und beendet, die durch einen konstanten Ausdruck verglichen werden kann oder ein `GetType` Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="7f588-170">An instance variable/property initializer consists of an identifier, which can match keywords, followed by a colon and equal sign, and terminated by a constant expression or a `GetType` expression.</span></span>

<span data-ttu-id="7f588-171">Erhalten ein Attribut mit der Attributklasse `T`, mit Feldern fester Breite Argumentliste `P`, und die Instanzliste Variable bzw. eine Eigenschaft-Initialisierer `N`, diese Schritte zu bestimmen, ob die Argumente gültig sind:</span><span class="sxs-lookup"><span data-stu-id="7f588-171">Given an attribute with attribute class `T`, positional argument list `P`, and instance variable/property initializer list `N`, these steps determine whether the arguments are valid:</span></span>

1. <span data-ttu-id="7f588-172">Führen Sie die während der Kompilierung Verarbeitungsschritte für die Kompilierung von eines Ausdrucks der Form `New T(P)`.</span><span class="sxs-lookup"><span data-stu-id="7f588-172">Follow the compile-time processing steps for compiling an expression of the form `New T(P)`.</span></span> <span data-ttu-id="7f588-173">Dies führt zu einem Fehler während der Kompilierung oder einen Konstruktor wird bestimmt, auf `T` , die an die Argumentliste anwendbar ist.</span><span class="sxs-lookup"><span data-stu-id="7f588-173">This either results in a compile-time error or determines a constructor on `T` that is most applicable to the argument list.</span></span>

2. <span data-ttu-id="7f588-174">Wenn der Konstruktor, der bestimmt, die in Schritt 1 verfügt über Parameter, die nicht Attributtypen sind oder ist nicht zugegriffen werden kann, auf der Website für die Deklaration, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="7f588-174">If the constructor determined in step 1 has parameters that are not attribute types or is inaccessible at the declaration site, a compile-time error occurs.</span></span>

3. <span data-ttu-id="7f588-175">Für jede Instanz Variable bzw. eine Eigenschaft-Initialisierer `Arg` in `N`ermöglichen `Name` der Bezeichner von der Instanz-Variable bzw. eine Eigenschaft-Initialisierer `Arg`.</span><span class="sxs-lookup"><span data-stu-id="7f588-175">For each instance variable/property initializer `Arg` in `N`, let `Name` be the identifier of the instance variable/property initializer `Arg`.</span></span> <span data-ttu-id="7f588-176">`Name` identifizieren müssen einer nicht -`Shared`, beschreibbar `Public` Instanz-Variable oder parameterlosen-Eigenschaft in `T` , dessen Typ ist ein Typ des Attributs.</span><span class="sxs-lookup"><span data-stu-id="7f588-176">`Name` must identify a non-`Shared`, writeable, `Public` instance variable or parameterless property on `T` whose type is an attribute type.</span></span> <span data-ttu-id="7f588-177">Wenn `T` hat keine solche Instanzvariable oder die Eigenschaft ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="7f588-177">If `T` has no such instance variable or property, a compile-time error occurs.</span></span>

<span data-ttu-id="7f588-178">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="7f588-178">For example:</span></span>

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

<span data-ttu-id="7f588-179">Typparameter können nicht an einer beliebigen Stelle in Attributargumenten verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="7f588-179">Type parameters cannot be used anywhere in attribute arguments.</span></span> <span data-ttu-id="7f588-180">Allerdings können konstruierte Typen verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="7f588-180">However, constructed types may be used:</span></span>

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