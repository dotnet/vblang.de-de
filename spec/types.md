---
ms.openlocfilehash: 4560eac9f4ab52d07c77724aeca696d0195da91a
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306022"
---
# <a name="types"></a><span data-ttu-id="cb403-101">Typen</span><span class="sxs-lookup"><span data-stu-id="cb403-101">Types</span></span>

<span data-ttu-id="cb403-102">Die zwei grundlegenden Kategorien von Typen in Visual Basic sind *Werttypen* und *Verweis Typen*.</span><span class="sxs-lookup"><span data-stu-id="cb403-102">The two fundamental categories of types in Visual Basic are *value types* and *reference types*.</span></span> <span data-ttu-id="cb403-103">Primitive Typen (außer Zeichen folgen), Enumerationen und Strukturen sind Werttypen.</span><span class="sxs-lookup"><span data-stu-id="cb403-103">Primitive types (except strings), enumerations, and structures are value types.</span></span> <span data-ttu-id="cb403-104">Klassen, Zeichen folgen, Standardmodule, Schnittstellen, Arrays und Delegaten sind Verweis Typen.</span><span class="sxs-lookup"><span data-stu-id="cb403-104">Classes, strings, standard modules, interfaces, arrays, and delegates are reference types.</span></span>

<span data-ttu-id="cb403-105">Jeder Typ verfügt über einen *Standardwert*. Dies ist der Wert, der Variablen dieses Typs bei der Initialisierung zugewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="cb403-105">Every type has a *default value*, which is the value that is assigned to variables of that type upon initialization.</span></span>

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

## <a name="value-types-and-reference-types"></a><span data-ttu-id="cb403-106">Wert- und Verweistypen</span><span class="sxs-lookup"><span data-stu-id="cb403-106">Value Types and Reference Types</span></span>

<span data-ttu-id="cb403-107">Auch wenn Werttypen und Verweis Typen in Bezug auf Deklarations Syntax und-Verwendung ähnlich sein können, sind ihre Semantik unterschiedlich.</span><span class="sxs-lookup"><span data-stu-id="cb403-107">Although value types and reference types can be similar in terms of declaration syntax and usage, their semantics are distinct.</span></span>

<span data-ttu-id="cb403-108">Verweis Typen werden auf dem Lauf Zeit Heap gespeichert. auf Sie kann nur über einen Verweis auf diesen Speicher zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-108">Reference types are stored on the run-time heap; they may only be accessed through a reference to that storage.</span></span> <span data-ttu-id="cb403-109">Da auf Verweis Typen immer über Verweise zugegriffen wird, wird deren Lebensdauer vom .NET Framework verwaltet.</span><span class="sxs-lookup"><span data-stu-id="cb403-109">Because reference types are always accessed through references, their lifetime is managed by the .NET Framework.</span></span> <span data-ttu-id="cb403-110">Ausstehende Verweise auf eine bestimmte Instanz werden nachverfolgt, und die Instanz wird nur zerstört, wenn keine weiteren Verweise mehr vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="cb403-110">Outstanding references to a particular instance are tracked and the instance is destroyed only when no more references remain.</span></span> <span data-ttu-id="cb403-111">Eine Variable des Verweis Typs enthält einen Verweis auf einen Wert dieses Typs, einen Wert eines stärker abgeleiteten Typs oder einen NULL-Wert.</span><span class="sxs-lookup"><span data-stu-id="cb403-111">A variable of reference type contains a reference to a value of that type, a value of a more derived type, or a null value.</span></span> <span data-ttu-id="cb403-112">Ein *NULL-Wert* verweist auf "Nothing". Es ist nicht möglich, etwas mit einem NULL-Wert zu verwenden, mit Ausnahme der Zuweisung.</span><span class="sxs-lookup"><span data-stu-id="cb403-112">A *null value* refers to nothing; it is not possible to do anything with a null value except assign it.</span></span> <span data-ttu-id="cb403-113">Durch die Zuweisung zu einer Variablen eines Verweis Typs wird eine Kopie des Verweises anstelle einer Kopie des Werts erstellt, auf den verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="cb403-113">Assignment to a variable of a reference type creates a copy of the reference rather than a copy of the referenced value.</span></span> <span data-ttu-id="cb403-114">Bei einer Variablen eines Verweis Typs ist der Standardwert ein NULL-Wert.</span><span class="sxs-lookup"><span data-stu-id="cb403-114">For a variable of a reference type, the default value is a null value.</span></span>

<span data-ttu-id="cb403-115">Werttypen werden direkt auf dem Stapel gespeichert, entweder innerhalb eines Arrays oder innerhalb eines anderen Typs. auf Ihren Speicher kann nur direkt zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-115">Value types are stored directly on the stack, either within an array or within another type; their storage can only be accessed directly.</span></span> <span data-ttu-id="cb403-116">Da Werttypen direkt innerhalb von Variablen gespeichert werden, wird deren Lebensdauer von der Lebensdauer der Variablen bestimmt, in der Sie enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="cb403-116">Because value types are stored directly within variables, their lifetime is determined by the lifetime of the variable that contains them.</span></span> <span data-ttu-id="cb403-117">Wenn der Speicherort, der eine Werttyp Instanz enthält, zerstört wird, wird auch die Werttyp Instanz zerstört.</span><span class="sxs-lookup"><span data-stu-id="cb403-117">When the location containing a value type instance is destroyed, the value type instance is also destroyed.</span></span> <span data-ttu-id="cb403-118">Auf Werttypen wird immer direkt zugegriffen. Es ist nicht möglich, einen Verweis auf einen Werttyp zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="cb403-118">Value types are always accessed directly; it is not possible to create a reference to a value type.</span></span> <span data-ttu-id="cb403-119">Durch das verbieten eines solchen Verweises ist es unmöglich, auf eine Instanz der Wert Klasse zu verweisen, die zerstört wurde.</span><span class="sxs-lookup"><span data-stu-id="cb403-119">Prohibiting such a reference makes it impossible to refer to a value class instance that has been destroyed.</span></span> <span data-ttu-id="cb403-120">Da Werttypen immer `NotInheritable`sind, enthält eine Variable eines Werttyps immer einen Wert dieses Typs.</span><span class="sxs-lookup"><span data-stu-id="cb403-120">Because value types are always `NotInheritable`, a variable of a value type always contains a value of that type.</span></span> <span data-ttu-id="cb403-121">Aus diesem Grund kann der Wert eines Werttyps kein NULL-Wert sein, und er kann nicht auf ein Objekt eines stärker abgeleiteten Typs verweisen.</span><span class="sxs-lookup"><span data-stu-id="cb403-121">Because of this, the value of a value type cannot be a null value, nor can it reference an object of a more derived type.</span></span> <span data-ttu-id="cb403-122">Durch die Zuweisung zu einer Variablen eines Werttyps wird eine Kopie des zugewiesenen Werts erstellt.</span><span class="sxs-lookup"><span data-stu-id="cb403-122">Assignment to a variable of a value type creates a copy of the value being assigned.</span></span> <span data-ttu-id="cb403-123">Der Standardwert für eine Variable eines Werttyps ist das Ergebnis der Initialisierung der einzelnen Variablen Elemente des Typs mit dem Standardwert.</span><span class="sxs-lookup"><span data-stu-id="cb403-123">For a variable of a value type, the default value is the result of initializing each variable member of the type to its default value.</span></span>

<span data-ttu-id="cb403-124">Das folgende Beispiel zeigt den Unterschied zwischen Verweis Typen und Werttypen:</span><span class="sxs-lookup"><span data-stu-id="cb403-124">The following example shows the difference between reference types and value types:</span></span>

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

<span data-ttu-id="cb403-125">Die Ausgabe des Programms lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="cb403-125">The output of the program is:</span></span>

```console
Values: 0, 123
Refs: 123, 123
```

<span data-ttu-id="cb403-126">Die Zuweisung zur lokalen Variablen `val2` wirkt sich nicht auf die lokale Variable `val1` aus, da beide lokalen Variablen einen Werttyp (der Typ `Integer`) und jede lokale Variable eines Werttyps über einen eigenen Speicher verfügt.</span><span class="sxs-lookup"><span data-stu-id="cb403-126">The assignment to the local variable `val2` does not impact the local variable `val1` because both local variables are of a value type (the type `Integer`) and each local variable of a value type has its own storage.</span></span> <span data-ttu-id="cb403-127">Im Gegensatz dazu wirkt sich der Zuweisungs `ref2.Value = 123;` auf das Objekt aus, das sowohl `ref1` als auch `ref2` Verweis ist.</span><span class="sxs-lookup"><span data-stu-id="cb403-127">In contrast, the assignment `ref2.Value = 123;` affects the object that both `ref1` and `ref2` reference.</span></span>

<span data-ttu-id="cb403-128">Beachten Sie, dass die .NET Framework Typsystem auch dann, wenn Strukturen, Enumerationen und primitive Typen (mit Ausnahme von `String`) Werttypen sind, alle von Verweis Typen erben.</span><span class="sxs-lookup"><span data-stu-id="cb403-128">One thing to note about the .NET Framework type system is that even though structures, enumerations and primitive types (except for `String`) are value types, they all inherit from reference types.</span></span> <span data-ttu-id="cb403-129">Strukturen und primitive Typen erben vom Verweistyp `System.ValueType`, der von `Object`erbt.</span><span class="sxs-lookup"><span data-stu-id="cb403-129">Structures and the primitive types inherit from the reference type `System.ValueType`, which inherits from `Object`.</span></span> <span data-ttu-id="cb403-130">Enumerierte Typen erben vom Verweistyp `System.Enum`, der von `System.ValueType`erbt.</span><span class="sxs-lookup"><span data-stu-id="cb403-130">Enumerated types inherit from the reference type `System.Enum`, which inherits from `System.ValueType`.</span></span>

### <a name="nullable-value-types"></a><span data-ttu-id="cb403-131">Auf NULL festlegbare Werttypen</span><span class="sxs-lookup"><span data-stu-id="cb403-131">Nullable Value Types</span></span>

<span data-ttu-id="cb403-132">Für Werttypen kann einem Typnamen ein `?` Modifizierer hinzugefügt werden, der die auf NULL festleg *Bare* Version dieses Typs darstellt.</span><span class="sxs-lookup"><span data-stu-id="cb403-132">For value types, a `?` modifier can be added to a type name to represent the *nullable* version of that type.</span></span>

```antlr
NullableTypeName
    : NonArrayTypeName '?'
    ;

NullableNameModifier
    : '?'
    ;
```

<span data-ttu-id="cb403-133">Ein Werttyp, der NULL-Werte zulässt, kann die gleichen Werte wie die Version des Typs, die keine NULL-Werte zulässt, sowie den Nullwert enthalten.</span><span class="sxs-lookup"><span data-stu-id="cb403-133">A nullable value type can contain the same values as the non-nullable version of the type as well as the null value.</span></span> <span data-ttu-id="cb403-134">Daher wird für einen Werttyp, der NULL-Werte zulässt, das Zuweisen von `Nothing` zu einer Variablen vom Typ den Wert der Variablen auf den NULL-Wert und nicht auf den Wert 0 (null) des Werttyps festgelegt.</span><span class="sxs-lookup"><span data-stu-id="cb403-134">Thus, for a nullable value type, assigning `Nothing` to a variable of the type sets the value of the variable to the null value, not the zero value of the value type.</span></span> <span data-ttu-id="cb403-135">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cb403-135">For example:</span></span>

```vb
Dim x As Integer = Nothing
Dim y As Integer? = Nothing

' Prints zero
Console.WriteLine(x)
' Prints nothing (because the value of y is the null value)
Console.WriteLine(y)
```

<span data-ttu-id="cb403-136">Eine Variable kann auch als ein Werttyp, der NULL-Werte zulässt, deklariert werden, indem ein Typmodifizierer, der NULL-Werte zulässt, für den Variablennamen</span><span class="sxs-lookup"><span data-stu-id="cb403-136">A variable may also be declared to be of a nullable value type by putting a nullable type modifier on the variable name.</span></span> <span data-ttu-id="cb403-137">Aus Gründen der Übersichtlichkeit ist es nicht zulässig, einen Typmodifizierer, der NULL-Werte zulässt, für einen Variablennamen und einen Typnamen in derselben Deklaration zu haben.</span><span class="sxs-lookup"><span data-stu-id="cb403-137">For clarity, it is not valid to have a nullable type modifier on both a variable name and a type name in the same declaration.</span></span> <span data-ttu-id="cb403-138">Da Typen, die NULL-Werte zulassen, mithilfe des-Typs `System.Nullable(Of T)`implementiert werden, ist der Typ `T?` Synonym für den Typ `System.Nullable(Of T)`, und die beiden Namen können austauschbar verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-138">Since nullable types are implemented using the type `System.Nullable(Of T)`, the type `T?` is synonymous to the type `System.Nullable(Of T)`, and the two names can be used interchangeably.</span></span> <span data-ttu-id="cb403-139">Der `?` Modifizierer kann nicht für einen Typ platziert werden, der bereits NULL-Werte zulässt. Daher ist es nicht möglich, den Typ `Integer??` oder `System.Nullable(Of Integer)?`zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="cb403-139">The `?` modifier cannot be placed on a type that is already nullable; thus, it is not possible to declare the type `Integer??` or `System.Nullable(Of Integer)?`.</span></span>

<span data-ttu-id="cb403-140">Ein Werttyp, der auf NULL festgelegt werden kann `T?` die Member von `System.Nullable(Of T)` sowie alle Operatoren *oder Konvertierungen* , die vom zugrunde liegenden Typ entfernt wurden, in den Typ `T?``T`.</span><span class="sxs-lookup"><span data-stu-id="cb403-140">A nullable value type `T?` has the members of `System.Nullable(Of T)` as well as any operators or conversions *lifted* from the underlying type `T` into the type `T?`.</span></span> <span data-ttu-id="cb403-141">Das Lifting von kopiert Operatoren und Konvertierungen aus dem zugrunde liegenden Typ, in den meisten Fällen, die NULL-Werte zulassen, für Werttypen, die keine Nullwerte zulassen</span><span class="sxs-lookup"><span data-stu-id="cb403-141">Lifting copies operators and conversions from the underlying type, in most cases substituting nullable value types for non-nullable value types.</span></span> <span data-ttu-id="cb403-142">Dadurch können viele der gleichen Konvertierungen und Vorgänge, die für `T` gelten, auch auf `T?` angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-142">This allows many of the same conversions and operations that apply to `T` to apply to `T?` as well.</span></span>


## <a name="interface-implementation"></a><span data-ttu-id="cb403-143">Schnittstellen Implementierung</span><span class="sxs-lookup"><span data-stu-id="cb403-143">Interface Implementation</span></span>

<span data-ttu-id="cb403-144">Struktur-und Klassen Deklarationen können deklarieren, dass Sie einen Satz von Schnittstellentypen durch eine oder mehrere `Implements` Klauseln implementieren.</span><span class="sxs-lookup"><span data-stu-id="cb403-144">Structure and class declarations may declare that they implement a set of interface types through one or more `Implements` clauses.</span></span>

```antlr
TypeImplementsClause
    : 'Implements' TypeImplements StatementTerminator
    ;

TypeImplements
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

<span data-ttu-id="cb403-145">Alle in der `Implements`-Klausel angegebenen Typen müssen Schnittstellen sein, und der Typ muss alle Member der Schnittstellen implementieren.</span><span class="sxs-lookup"><span data-stu-id="cb403-145">All the types specified in the `Implements` clause must be interfaces, and the type must implement all members of the interfaces.</span></span> <span data-ttu-id="cb403-146">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cb403-146">For example:</span></span>

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

<span data-ttu-id="cb403-147">Ein Typ, der eine Schnittstelle implementiert, implementiert auch implizit alle Basis Schnittstellen der Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="cb403-147">A type that implements an interface also implicitly implements all of the interface's base interfaces.</span></span> <span data-ttu-id="cb403-148">Dies gilt auch, wenn der Typ nicht explizit alle Basis Schnittstellen in der `Implements`-Klausel aufführt.</span><span class="sxs-lookup"><span data-stu-id="cb403-148">This is true even if the type does not explicitly list all base interfaces in the `Implements` clause.</span></span> <span data-ttu-id="cb403-149">In diesem Beispiel implementiert die `TextBox` Struktur sowohl `IControl` als auch `ITextBox`.</span><span class="sxs-lookup"><span data-stu-id="cb403-149">In this example, the `TextBox` structure implements both `IControl` and `ITextBox`.</span></span>

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

<span data-ttu-id="cb403-150">Wenn Sie deklarieren, dass ein Typ eine Schnittstelle in und von sich selbst implementiert, wird im Deklarations Bereich des Typs nichts deklariert.</span><span class="sxs-lookup"><span data-stu-id="cb403-150">Declaring that a type implements an interface in and of itself does not declare anything in the declaration space of the type.</span></span> <span data-ttu-id="cb403-151">Daher ist es zulässig, zwei Schnittstellen zu implementieren, die eine Methode mit demselben Namen haben.</span><span class="sxs-lookup"><span data-stu-id="cb403-151">Thus, it is valid to implement two interfaces with a method by the same name.</span></span>

<span data-ttu-id="cb403-152">Typen können einen Typparameter nicht selbst implementieren, obwohl Sie möglicherweise die Typparameter enthalten, die sich im Gültigkeitsbereich befinden.</span><span class="sxs-lookup"><span data-stu-id="cb403-152">Types cannot implement a type parameter on its own, although it may involve the type parameters that are in scope.</span></span>

```vb
Class C1(Of V)
    Implements V  ' Error, can't implement type parameter directly
    Implements IEnumerable(Of V)  ' OK, not directly implementing

    ...
End Class
```

<span data-ttu-id="cb403-153">Generische Schnittstellen können mehrmals mit unterschiedlichen Typargumenten implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-153">Generic interfaces can be implemented multiple times using different type arguments.</span></span> <span data-ttu-id="cb403-154">Ein generischer Typ kann jedoch eine generische Schnittstelle nicht mithilfe eines Typparameters implementieren, wenn der angegebene Typparameter (ungeachtet der Typeinschränkungen) mit einer anderen Implementierung dieser Schnittstelle überlappt.</span><span class="sxs-lookup"><span data-stu-id="cb403-154">However, a generic type cannot implement a generic interface using a type parameter if the supplied type parameter (regardless of type constraints) could overlap with another implementation of that interface.</span></span> <span data-ttu-id="cb403-155">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cb403-155">For example:</span></span>

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


## <a name="primitive-types"></a><span data-ttu-id="cb403-156">Primitive Datentypen</span><span class="sxs-lookup"><span data-stu-id="cb403-156">Primitive Types</span></span>

<span data-ttu-id="cb403-157">Die *primitiven Typen* werden mithilfe von Schlüsselwörtern identifiziert, bei denen es sich um Aliase für vordefinierte Typen im `System`-Namespace handelt.</span><span class="sxs-lookup"><span data-stu-id="cb403-157">The *primitive types* are identified through keywords, which are aliases for predefined types in the `System` namespace.</span></span> <span data-ttu-id="cb403-158">Ein primitiver Typ ist von dem Typ, den er Aliase hat, vollständig nicht unterscheiden: das Schreiben des reservierten Worts `Byte` ist exakt identisch mit dem Schreiben von `System.Byte`.</span><span class="sxs-lookup"><span data-stu-id="cb403-158">A primitive type is completely indistinguishable from the type it aliases: writing the reserved word `Byte` is exactly the same as writing `System.Byte`.</span></span> <span data-ttu-id="cb403-159">Primitive Typen werden auch als systeminterne *Typen*bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="cb403-159">Primitive types are also known as *intrinsic types*.</span></span>

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


<span data-ttu-id="cb403-160">Da ein primitiver Typ einen regulären Typ Aliase, weist jeder Primitive Typ Member auf.</span><span class="sxs-lookup"><span data-stu-id="cb403-160">Because a primitive type aliases a regular type, every primitive type has members.</span></span> <span data-ttu-id="cb403-161">Beispielsweise `Integer` die Member in `System.Int32`deklariert.</span><span class="sxs-lookup"><span data-stu-id="cb403-161">For example, `Integer` has the members declared in `System.Int32`.</span></span> <span data-ttu-id="cb403-162">Literale können als Instanzen der entsprechenden Typen behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-162">Literals can be treated as instances of their corresponding types.</span></span>

* <span data-ttu-id="cb403-163">Die primitiven Typen unterscheiden sich von anderen Strukturtypen insofern, als Sie bestimmte zusätzliche Vorgänge zulassen:</span><span class="sxs-lookup"><span data-stu-id="cb403-163">The primitive types differ from other structure types in that they permit certain additional operations:</span></span>

* <span data-ttu-id="cb403-164">Primitive Typen ermöglichen das Erstellen von Werten, indem Literale geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-164">Primitive types permit values to be created by writing literals.</span></span> <span data-ttu-id="cb403-165">`123I` ist z. b. ein Literaltyp `Integer`.</span><span class="sxs-lookup"><span data-stu-id="cb403-165">For example, `123I` is a literal of type `Integer`.</span></span>

* <span data-ttu-id="cb403-166">Es ist möglich, Konstanten der primitiven Typen zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="cb403-166">It is possible to declare constants of the primitive types.</span></span>

* <span data-ttu-id="cb403-167">Wenn es sich bei den Operanden eines Ausdrucks um primitive Typkonstanten handelt, kann der Compiler den Ausdruck zum Zeitpunkt der Kompilierung auswerten.</span><span class="sxs-lookup"><span data-stu-id="cb403-167">When the operands of an expression are all primitive type constants, it is possible for the compiler to evaluate the expression at compile time.</span></span> <span data-ttu-id="cb403-168">Ein solcher Ausdruck wird als konstanter Ausdruck bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="cb403-168">Such an expression is known as a constant expression.</span></span>

<span data-ttu-id="cb403-169">Visual Basic definiert die folgenden primitiven Typen:</span><span class="sxs-lookup"><span data-stu-id="cb403-169">Visual Basic defines the following primitive types:</span></span>

* <span data-ttu-id="cb403-170">Die ganzzahligen Werttypen `Byte` (1-Byte-Ganzzahl ohne Vorzeichen), `SByte` (1-Byte-Ganzzahl mit Vorzeichen), `UShort` (2-Byte-Ganzzahl ohne Vorzeichen), `Short` (2-Byte-Ganzzahl mit Vorzeichen), `UInteger` (4-Byte-Ganzzahl ohne Vorzeichen), `Integer` (4-Byte-Ganzzahl mit Vorzeichen), `ULong` (8-Byte-Integer ohne Vorzeichen) und `Long` (8-Byte-Ganzzahl</span><span class="sxs-lookup"><span data-stu-id="cb403-170">The integral value types `Byte` (1-byte unsigned integer), `SByte` (1-byte signed integer), `UShort` (2-byte unsigned integer), `Short` (2-byte signed integer), `UInteger` (4-byte unsigned integer), `Integer` (4-byte signed integer), `ULong` (8-byte unsigned integer), and `Long` (8-byte signed integer).</span></span> <span data-ttu-id="cb403-171">Diese Typen werden `System.Byte`, `System.SByte`, `System.UInt16`, `System.Int16`, `System.UInt32`, `System.Int32`, `System.UInt64` bzw. `System.Int64`zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="cb403-171">These types map to `System.Byte`, `System.SByte`, `System.UInt16`, `System.Int16`, `System.UInt32`, `System.Int32`, `System.UInt64` and `System.Int64`, respectively.</span></span> <span data-ttu-id="cb403-172">Der Standardwert eines ganzzahligen Typs entspricht dem Literal`0`.</span><span class="sxs-lookup"><span data-stu-id="cb403-172">The default value of an integral type is equivalent to the literal `0`.</span></span>

* <span data-ttu-id="cb403-173">Die Gleit Komma Werttypen `Single` (4-Byte-Gleit Komma Zahl) und `Double` (8-Byte-Gleit Komma Zahl).</span><span class="sxs-lookup"><span data-stu-id="cb403-173">The floating-point value types `Single` (4-byte floating point) and `Double` (8-byte floating point).</span></span> <span data-ttu-id="cb403-174">Diese Typen werden `System.Single` bzw. `System.Double`zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="cb403-174">These types map to `System.Single` and `System.Double`, respectively.</span></span> <span data-ttu-id="cb403-175">Der Standardwert eines Gleit Komma Typs entspricht dem Literal`0`.</span><span class="sxs-lookup"><span data-stu-id="cb403-175">The default value of a floating-point type is equivalent to the literal `0`.</span></span>

* <span data-ttu-id="cb403-176">Der `Decimal` Typ (16-Byte-Dezimalwert), der `System.Decimal`zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="cb403-176">The `Decimal` type (16-byte decimal value), which maps to `System.Decimal`.</span></span> <span data-ttu-id="cb403-177">Der Standardwert von Decimal entspricht dem Literal`0D`.</span><span class="sxs-lookup"><span data-stu-id="cb403-177">The default value of decimal is equivalent to the literal `0D`.</span></span>

* <span data-ttu-id="cb403-178">Der `Boolean` Werttyp, der einen Wahrheitswert darstellt (in der Regel das Ergebnis einer relationalen oder logischen Operation).</span><span class="sxs-lookup"><span data-stu-id="cb403-178">The `Boolean` value type, which represents a truth value, typically the result of a relational or logical operation.</span></span> <span data-ttu-id="cb403-179">Der Literaltyp ist vom Typ `System.Boolean`.</span><span class="sxs-lookup"><span data-stu-id="cb403-179">The literal is of type `System.Boolean`.</span></span> <span data-ttu-id="cb403-180">Der Standardwert des `Boolean` Typs entspricht dem Literal`False`.</span><span class="sxs-lookup"><span data-stu-id="cb403-180">The default value of the `Boolean` type is equivalent to the literal `False`.</span></span>

* <span data-ttu-id="cb403-181">Der `Date` Werttyp, der ein Datum und/oder eine Uhrzeit darstellt und `System.DateTime`zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="cb403-181">The `Date` value type, which represents a date and/or a time and maps to `System.DateTime`.</span></span> <span data-ttu-id="cb403-182">Der Standardwert des `Date` Typs entspricht dem Literal`# 01/01/0001 12:00:00AM #`.</span><span class="sxs-lookup"><span data-stu-id="cb403-182">The default value of the `Date` type is equivalent to the literal `# 01/01/0001 12:00:00AM #`.</span></span>

* <span data-ttu-id="cb403-183">Der `Char` Werttyp, der ein einzelnes Unicode-Zeichen darstellt und `System.Char`zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="cb403-183">The `Char` value type, which represents a single Unicode character and maps to `System.Char`.</span></span> <span data-ttu-id="cb403-184">Der Standardwert des `Char` Typs entspricht dem konstanten Ausdruck `ChrW(0)`.</span><span class="sxs-lookup"><span data-stu-id="cb403-184">The default value of the `Char` type is equivalent to the constant expression `ChrW(0)`.</span></span>

* <span data-ttu-id="cb403-185">Der `String` Verweistyp, der eine Sequenz von Unicode-Zeichen darstellt und `System.String`zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="cb403-185">The `String` reference type, which represents a sequence of Unicode characters and maps to `System.String`.</span></span> <span data-ttu-id="cb403-186">Der Standardwert des `String` Typs ist ein NULL-Wert.</span><span class="sxs-lookup"><span data-stu-id="cb403-186">The default value of the `String` type is a null value.</span></span>



## <a name="enumerations"></a><span data-ttu-id="cb403-187">Enumerationen</span><span class="sxs-lookup"><span data-stu-id="cb403-187">Enumerations</span></span>

<span data-ttu-id="cb403-188">*Enumerationen* sind Werttypen, die von `System.Enum` erben und symbolisch eine Menge von Werten eines der primitiven ganzzahligen Typen darstellen.</span><span class="sxs-lookup"><span data-stu-id="cb403-188">*Enumerations* are value types that inherit from `System.Enum` and symbolically represent a set of values of one of the primitive integral types.</span></span>

```antlr
EnumDeclaration
    : Attributes? TypeModifier* 'Enum' Identifier
      ( 'As' NonArrayTypeName )? StatementTerminator
      EnumMemberDeclaration+
      'End' 'Enum' StatementTerminator
    ;
```

<span data-ttu-id="cb403-189">Bei einem Enumerationstyp `E`ist der Standardwert der Wert, der vom Ausdruck `CType(0, E)`erzeugt wird.</span><span class="sxs-lookup"><span data-stu-id="cb403-189">For an enumeration type `E`, the default value is the value produced by the expression `CType(0, E)`.</span></span>

<span data-ttu-id="cb403-190">Der zugrunde liegende Typ einer Enumeration muss ein ganzzahliger Typ sein, der alle in der-Enumeration definierten Enumeratorwerte darstellen kann.</span><span class="sxs-lookup"><span data-stu-id="cb403-190">The underlying type of an enumeration must be an integral type that can represent all the enumerator values defined in the enumeration.</span></span> <span data-ttu-id="cb403-191">Wenn ein zugrunde liegender Typ angegeben wird, muss es sich um `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`oder einen der entsprechenden Typen im `System`-Namespace handeln.</span><span class="sxs-lookup"><span data-stu-id="cb403-191">If an underlying type is specified, it must be `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, or one of their corresponding types in the `System` namespace.</span></span> <span data-ttu-id="cb403-192">Wenn kein zugrunde liegender Typ explizit angegeben wird, wird der Standardwert `Integer`.</span><span class="sxs-lookup"><span data-stu-id="cb403-192">If no underlying type is explicitly specified, the default is `Integer`.</span></span>

<span data-ttu-id="cb403-193">Im folgenden Beispiel wird eine Enumeration mit einem zugrunde liegenden Typ von `Long`deklariert:</span><span class="sxs-lookup"><span data-stu-id="cb403-193">The following example declares an enumeration with an underlying type of `Long`:</span></span>

```vb
Enum Color As Long
    Red
    Green
    Blue
End Enum
```

<span data-ttu-id="cb403-194">Ein Entwickler kann eine zugrunde liegende Art von `Long`verwenden, wie z. b., um die Verwendung von Werten zu ermöglichen, die sich im Bereich von `Long`, aber nicht im Bereich von `Integer`befinden, oder um diese Option für die Zukunft beizubehalten.</span><span class="sxs-lookup"><span data-stu-id="cb403-194">A developer might choose to use an underlying type of `Long`, as in the example, to enable the use of values that are in the range of `Long`, but not in the range of `Integer`, or to preserve this option for the future.</span></span>


### <a name="enumeration-members"></a><span data-ttu-id="cb403-195">Enumerationsmember</span><span class="sxs-lookup"><span data-stu-id="cb403-195">Enumeration Members</span></span>

<span data-ttu-id="cb403-196">Die Member einer Enumeration sind die in der-Enumeration deklarierten Enumerationswerte und die von der-Klasse geerbten Member `System.Enum`.</span><span class="sxs-lookup"><span data-stu-id="cb403-196">The members of an enumeration are the enumerated values declared in the enumeration and the members inherited from class `System.Enum`.</span></span>

<span data-ttu-id="cb403-197">Der Bereich eines Enumerationsmembers ist der enumerationsdeklarations-Text.</span><span class="sxs-lookup"><span data-stu-id="cb403-197">The scope of an enumeration member is the enumeration declaration body.</span></span> <span data-ttu-id="cb403-198">Dies bedeutet, dass ein Enumerationsmember außerhalb einer Enumerationsdeklaration immer qualifiziert werden muss (es sei denn, der Typ wird durch einen Namespace Import ausdrücklich in einen Namespace importiert).</span><span class="sxs-lookup"><span data-stu-id="cb403-198">This means that outside of an enumeration declaration, an enumeration member must always be qualified (unless the type is specifically imported into a namespace through a namespace import).</span></span>

<span data-ttu-id="cb403-199">Die Deklarations Reihenfolge für enumerationselementdeklarationen ist signifikant, wenn Konstante Ausdrucks Werte ausgelassen werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-199">Declaration order for enumeration member declarations is significant when constant expression values are omitted.</span></span> <span data-ttu-id="cb403-200">Enumerationsmember haben implizit nur `Public` Zugriff; Es sind keine Zugriffsmodifizierer für Enumerationsmember zulässig.</span><span class="sxs-lookup"><span data-stu-id="cb403-200">Enumeration members implicitly have `Public` access only; no access modifiers are allowed on enumeration member declarations.</span></span>

```antlr
EnumMemberDeclaration
    : Attributes? Identifier ( Equals ConstantExpression )? StatementTerminator
    ;
```

### <a name="enumeration-values"></a><span data-ttu-id="cb403-201">Enumerationswerte</span><span class="sxs-lookup"><span data-stu-id="cb403-201">Enumeration Values</span></span>

<span data-ttu-id="cb403-202">Die Enumerationswerte in einer Enumerationsmember-Liste werden als Konstanten deklariert, die als zugrunde liegender Enumerationstyp typisiert sind. Sie können dort vorkommen, wo Konstanten erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="cb403-202">The enumerated values in an enumeration member list are declared as constants typed as the underlying enumeration type, and they can appear wherever constants are required.</span></span> <span data-ttu-id="cb403-203">Eine Enumerationsmember-Definition mit `=` gibt dem zugeordneten Member den Wert, der durch den konstanten Ausdruck angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="cb403-203">An enumeration member definition with `=` gives the associated member the value indicated by the constant expression.</span></span> <span data-ttu-id="cb403-204">Der Konstante Ausdruck muss zu einem ganzzahligen Typ ausgewertet werden, der implizit in den zugrunde liegenden Typ konvertierbar ist und innerhalb des Bereichs von Werten liegen muss, der durch den zugrunde liegenden Typ dargestellt werden kann.</span><span class="sxs-lookup"><span data-stu-id="cb403-204">The constant expression must evaluate to an integral type that is implicitly convertible to the underlying type and must be within the range of values that can be represented by the underlying type.</span></span> <span data-ttu-id="cb403-205">Das folgende Beispiel ist fehlerhaft, da die Konstanten Werte `1.5`, `2.3`und `3.3` nicht implizit in den zugrunde liegenden ganzzahligen Typ `Long` mit strenger Semantik konvertiert werden können.</span><span class="sxs-lookup"><span data-stu-id="cb403-205">The following example is in error because the constant values `1.5`, `2.3`, and `3.3` are not implicitly convertible to the underlying integral type `Long` with strict semantics.</span></span>

```vb
Option Strict On

Enum Color As Long
    Red = 1.5
    Green = 2.3
    Blue = 3.3
End Enum
```

<span data-ttu-id="cb403-206">Mehrere Enumerationsmember haben möglicherweise denselben zugeordneten Wert, wie unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="cb403-206">Multiple enumeration members may share the same associated value, as shown below:</span></span>

```vb
Enum Color
    Red
    Green
    Blue
    Max = Blue
End Enum
```

<span data-ttu-id="cb403-207">Das Beispiel zeigt eine Enumeration, die über zwei Enumerationsmember verfügt: `Blue` und `Max`, die denselben zugeordneten Wert aufweisen.</span><span class="sxs-lookup"><span data-stu-id="cb403-207">The example shows an enumeration that has two enumeration members -- `Blue` and `Max` -- that have the same associated value.</span></span>

<span data-ttu-id="cb403-208">Wenn die erste Enumeratorwertdefinition in der-Enumeration keinen Initialisierer aufweist, wird der Wert der entsprechenden Konstante `0`.</span><span class="sxs-lookup"><span data-stu-id="cb403-208">If the first enumerator value definition in the enumeration has no initializer, the value of the corresponding constant is `0`.</span></span> <span data-ttu-id="cb403-209">Eine Enumerationswertdefinition ohne Initialisierer übergibt dem Enumerator den Wert, der durch die Erhöhung des Werts des vorherigen Enumerationswerts durch `1`abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="cb403-209">An enumeration value definition without an initializer gives the enumerator the value obtained by increasing the value of the previous enumeration value by `1`.</span></span> <span data-ttu-id="cb403-210">Dieser erweiterte Wert muss innerhalb des Wertebereichs liegen, der durch den zugrunde liegenden Typ dargestellt werden kann.</span><span class="sxs-lookup"><span data-stu-id="cb403-210">This increased value must be within the range of values that can be represented by the underlying type.</span></span>

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

<span data-ttu-id="cb403-211">Im obigen Beispiel werden die Enumerationswerte und ihre zugeordneten Werte ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="cb403-211">The example above prints the enumeration values and their associated values.</span></span> <span data-ttu-id="cb403-212">Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="cb403-212">The output is:</span></span>

```console
Red = 0
Green = 10
Blue = 11
```

<span data-ttu-id="cb403-213">Die Gründe für die Werte lauten wie folgt:</span><span class="sxs-lookup"><span data-stu-id="cb403-213">The reasons for the values are as follows:</span></span>

* <span data-ttu-id="cb403-214">Dem Enumerationswert `Red` wird automatisch der Wert `0` zugewiesen (da er über keinen Initialisierer verfügt und der erste enumerationswertmember ist).</span><span class="sxs-lookup"><span data-stu-id="cb403-214">The enumeration value `Red` is automatically assigned the value `0` (since it has no initializer and is the first enumeration value member).</span></span>

* <span data-ttu-id="cb403-215">Der-Enumerationswert `Green` der explizit den Wert `10`erhält.</span><span class="sxs-lookup"><span data-stu-id="cb403-215">The enumeration value `Green` is explicitly given the value `10`.</span></span>

* <span data-ttu-id="cb403-216">Dem Enumerationswert `Blue` wird automatisch der Wert zugewiesen, der größer ist als der Enumerationswert, der ihm textlich vorangestellt ist.</span><span class="sxs-lookup"><span data-stu-id="cb403-216">The enumeration value `Blue` is automatically assigned the value one greater than the enumeration value that textually precedes it.</span></span>

<span data-ttu-id="cb403-217">Der Konstante Ausdruck darf nicht direkt oder indirekt den Wert seines eigenen zugeordneten Enumerationswerts verwenden (d. h., Zirkularität im Konstantenausdruck ist nicht zulässig).</span><span class="sxs-lookup"><span data-stu-id="cb403-217">The constant expression may not directly or indirectly use the value of its own associated enumeration value (that is, circularity in the constant expression is not allowed).</span></span> <span data-ttu-id="cb403-218">Das folgende Beispiel ist ungültig, da die Deklarationen von `A` und `B` zirkulär sind.</span><span class="sxs-lookup"><span data-stu-id="cb403-218">The following example is invalid because the declarations of `A` and `B` are circular.</span></span>

```vb
Enum Circular
    A = B
    B
End Enum
```

<span data-ttu-id="cb403-219">`A` ist explizit von `B` abhängig, und `B` von `A` implizit abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="cb403-219">`A` depends on `B` explicitly, and `B` depends on `A` implicitly.</span></span>

## <a name="classes"></a><span data-ttu-id="cb403-220">Klassen</span><span class="sxs-lookup"><span data-stu-id="cb403-220">Classes</span></span>

<span data-ttu-id="cb403-221">Eine- *Klasse* ist eine Datenstruktur, die Datenmember (Konstanten, Variablen und Ereignisse), Funktionsmember (Methoden, Eigenschaften, Indexer, Operatoren und Konstruktoren) und die in der Struktur enthaltenen Typen enthalten kann.</span><span class="sxs-lookup"><span data-stu-id="cb403-221">A *class* is a data structure that may contain data members (constants, variables, and events), function members (methods, properties, indexers, operators, and constructors), and nested types.</span></span> <span data-ttu-id="cb403-222">Klassen sind Verweis Typen.</span><span class="sxs-lookup"><span data-stu-id="cb403-222">Classes are reference types.</span></span>

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

<span data-ttu-id="cb403-223">Das folgende Beispiel zeigt eine Klasse, die jede Art von Member enthält:</span><span class="sxs-lookup"><span data-stu-id="cb403-223">The following example shows a class that contains each kind of member:</span></span>

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

<span data-ttu-id="cb403-224">Das folgende Beispiel zeigt die Verwendung dieser Member:</span><span class="sxs-lookup"><span data-stu-id="cb403-224">The following example shows uses of these members:</span></span>

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

<span data-ttu-id="cb403-225">Es gibt zwei klassenspezifische Modifizierer, `MustInherit` und `NotInheritable`.</span><span class="sxs-lookup"><span data-stu-id="cb403-225">There are two class-specific modifiers, `MustInherit` and `NotInheritable`.</span></span> <span data-ttu-id="cb403-226">Beide können nicht gleichzeitig angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-226">It is invalid to specify them both.</span></span>


### <a name="class-base-specification"></a><span data-ttu-id="cb403-227">Klassenbasis Spezifikation</span><span class="sxs-lookup"><span data-stu-id="cb403-227">Class Base Specification</span></span>

<span data-ttu-id="cb403-228">Eine Klassen Deklaration kann eine Basistyp Spezifikation enthalten, die den direkten Basistyp der Klasse definiert.</span><span class="sxs-lookup"><span data-stu-id="cb403-228">A class declaration may include a base type specification that defines the direct base type of the class.</span></span>

```antlr
ClassBase
    : 'Inherits' NonArrayTypeName StatementTerminator
    ;
```

<span data-ttu-id="cb403-229">Wenn eine Klassen Deklaration keinen expliziten Basistyp aufweist, wird der direkte Basistyp implizit `Object`.</span><span class="sxs-lookup"><span data-stu-id="cb403-229">If a class declaration has no explicit base type, the direct base type is implicitly `Object`.</span></span> <span data-ttu-id="cb403-230">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cb403-230">For example:</span></span>

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class
```

<span data-ttu-id="cb403-231">Basis Typen können nicht eigenständig ein Typparameter sein, obwohl Sie möglicherweise die Typparameter enthalten, die sich im Gültigkeitsbereich befinden.</span><span class="sxs-lookup"><span data-stu-id="cb403-231">Base types cannot be a type parameter on its own, although it may involve the type parameters that are in scope.</span></span>

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

<span data-ttu-id="cb403-232">Klassen können nur von `Object`-und-Klassen abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-232">Classes may only derive from `Object` and classes.</span></span> <span data-ttu-id="cb403-233">Es ist ungültig, dass eine Klasse von `System.ValueType`, `System.Enum`, `System.Array`, `System.MulticastDelegate` oder `System.Delegate`abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="cb403-233">It is invalid for a class to derive from `System.ValueType`, `System.Enum`, `System.Array`, `System.MulticastDelegate` or `System.Delegate`.</span></span> <span data-ttu-id="cb403-234">Eine generische Klasse kann nicht von `System.Attribute` oder von einer Klasse abgeleitet werden, die davon abgeleitet ist.</span><span class="sxs-lookup"><span data-stu-id="cb403-234">A generic class cannot derive from `System.Attribute` or from a class that derives from it.</span></span>

<span data-ttu-id="cb403-235">Jede Klasse verfügt über genau eine direkte Basisklasse, und Zirkularität bei der Ableitung ist nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="cb403-235">Every class has exactly one direct base class, and circularity in derivation is prohibited.</span></span> <span data-ttu-id="cb403-236">Es ist nicht möglich, von einer `NotInheritable` Klasse abzuleiten, und die Zugriffs Domäne der Basisklasse muss mit der Zugriffs Domäne der Klasse selbst identisch sein.</span><span class="sxs-lookup"><span data-stu-id="cb403-236">It is not possible to derive from a `NotInheritable` class, and the accessibility domain of the base class must be the same as or a superset of the accessibility domain of the class itself.</span></span>


### <a name="class-members"></a><span data-ttu-id="cb403-237">Klassenmember</span><span class="sxs-lookup"><span data-stu-id="cb403-237">Class Members</span></span>

<span data-ttu-id="cb403-238">Die Member einer Klasse bestehen aus den Membern, die von ihren Klassenmember Deklarationen und den Membern, die von der direkten Basisklasse geerbt wurden,</span><span class="sxs-lookup"><span data-stu-id="cb403-238">The members of a class consist of the members introduced by its class member declarations and the members inherited from its direct base class.</span></span>

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

<span data-ttu-id="cb403-239">Eine Klassenmember-Deklaration kann über `Public`, `Protected`, `Friend`, `Protected Friend`oder `Private` Zugriff verfügen.</span><span class="sxs-lookup"><span data-stu-id="cb403-239">A class member declaration may have `Public`, `Protected`, `Friend`, `Protected Friend`, or `Private` access.</span></span> <span data-ttu-id="cb403-240">Wenn eine Klassenmember-Deklaration keinen Zugriffsmodifizierer enthält, wird standardmäßig `Public` Access verwendet, sofern es sich nicht um eine Variablen Deklaration handelt. in diesem Fall wird standardmäßig `Private` Access verwendet.</span><span class="sxs-lookup"><span data-stu-id="cb403-240">When a class member declaration does not include an access modifier, the declaration defaults to `Public` access, unless it is a variable declaration; in that case it defaults to `Private` access.</span></span>

<span data-ttu-id="cb403-241">Der Gültigkeitsbereich eines Klassenmembers ist der Klassen Text, in dem die Element Deklaration auftritt, sowie die Einschränkungs Liste dieser Klasse (wenn Sie generisch und Einschränkungen aufweist).</span><span class="sxs-lookup"><span data-stu-id="cb403-241">The scope of a class member is the class body in which the member declaration occurs, plus the constraint list of that class (if it is generic and has constraints).</span></span> <span data-ttu-id="cb403-242">Wenn das Element `Friend` Access hat, erstreckt sich sein Bereich auf den Klassen Text einer beliebigen abgeleiteten Klasse im gleichen Programm oder auf eine beliebige Assembly, die `Friend` Zugriff erhalten hat, und wenn der Member `Public`, `Protected`oder `Protected Friend` Zugriff hat, erstreckt sich der Gültigkeitsbereich auf den Klassen Text einer beliebigen abgeleiteten Klasse in jedem Programm.</span><span class="sxs-lookup"><span data-stu-id="cb403-242">If the member has `Friend` access, its scope extends to the class body of any derived class in the same program or any assembly that has been given `Friend` access, and if the member has `Public`, `Protected`, or `Protected Friend` access, its scope extends to the class body of any derived class in any program.</span></span>


## <a name="structures"></a><span data-ttu-id="cb403-243">Strukturen</span><span class="sxs-lookup"><span data-stu-id="cb403-243">Structures</span></span>

<span data-ttu-id="cb403-244">*Strukturen* sind Werttypen, die von `System.ValueType`erben.</span><span class="sxs-lookup"><span data-stu-id="cb403-244">*Structures* are value types that inherit from `System.ValueType`.</span></span> <span data-ttu-id="cb403-245">Strukturen ähneln Klassen darin, dass Sie Datenstrukturen darstellen, die Datenmember und Funktionsmember enthalten können.</span><span class="sxs-lookup"><span data-stu-id="cb403-245">Structures are similar to classes in that they represent data structures that can contain data members and function members.</span></span> <span data-ttu-id="cb403-246">Im Gegensatz zu-Klassen erfordern Strukturen jedoch keine Heap Zuordnung.</span><span class="sxs-lookup"><span data-stu-id="cb403-246">Unlike classes, however, structures do not require heap allocation.</span></span>

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

<span data-ttu-id="cb403-247">Im Fall von Klassen können zwei Variablen auf das gleiche Objekt verweisen, und so können Vorgänge in einer Variablen das Objekt beeinflussen, auf das von der anderen Variablen verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="cb403-247">In the case of classes, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="cb403-248">Bei Strukturen verfügen die Variablen jeweils über eine eigene Kopie der Daten, die nicht`Shared` sind. Daher ist es nicht möglich, dass sich Vorgänge auf dem anderen befinden, wie im folgenden Beispiel veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="cb403-248">With structures, the variables each have their own copy of the non-`Shared` data, so it is not possible for operations on one to affect the other, as the following example illustrates:</span></span>

```vb
Structure Point
    Public x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

<span data-ttu-id="cb403-249">In der obigen Deklaration gibt der folgende Code den Wert `10`aus:</span><span class="sxs-lookup"><span data-stu-id="cb403-249">Given the above declaration the following code outputs the value `10`:</span></span>

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

<span data-ttu-id="cb403-250">Durch die Zuweisung von `a` `b` wird eine Kopie des Werts erstellt, und `b` ist von der Zuweisung zum `a.x`nicht betroffen.</span><span class="sxs-lookup"><span data-stu-id="cb403-250">The assignment of `a` to `b` creates a copy of the value, and `b` is thus unaffected by the assignment to `a.x`.</span></span> <span data-ttu-id="cb403-251">Wenn `Point` stattdessen als Klasse deklariert wurde, wird die Ausgabe `100`, da `a` und `b` auf das gleiche Objekt verweisen würden.</span><span class="sxs-lookup"><span data-stu-id="cb403-251">Had `Point` instead been declared as a class, the output would be `100` because `a` and `b` would reference the same object.</span></span>


### <a name="structure-members"></a><span data-ttu-id="cb403-252">Strukturmember</span><span class="sxs-lookup"><span data-stu-id="cb403-252">Structure Members</span></span>

<span data-ttu-id="cb403-253">Die Member einer Struktur sind die Member, die von den zugehörigen Strukturmember-Deklarationen und den von `System.ValueType`geerbten Membern eingeführt werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-253">The members of a structure are the members introduced by its structure member declarations and the members inherited from `System.ValueType`.</span></span>

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

<span data-ttu-id="cb403-254">Jede Struktur verfügt implizit über einen `Public` Parameter losen Instanzenkonstruktor, der den Standardwert der-Struktur erzeugt.</span><span class="sxs-lookup"><span data-stu-id="cb403-254">Every structure implicitly has a `Public` parameterless instance constructor that produces the default value of the structure.</span></span> <span data-ttu-id="cb403-255">Daher ist es für eine Strukturtyp Deklaration nicht möglich, einen Parameter losen Instanzenkonstruktor zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="cb403-255">As a result, it is not possible for a structure type declaration to declare a parameterless instance constructor.</span></span> <span data-ttu-id="cb403-256">Ein Strukturtyp ist jedoch zulässig, um *parametrisierte* Instanzkonstruktoren zu deklarieren, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="cb403-256">A structure type is, however, permitted to declare *parameterized* instance constructors, as in the following example:</span></span>

```vb
Structure Point
    Private x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

<span data-ttu-id="cb403-257">Bei der obigen Deklaration erstellen die folgenden Anweisungen eine `Point` mit `x` und `y` auf NULL initialisiert.</span><span class="sxs-lookup"><span data-stu-id="cb403-257">Given the above declaration, the following statements both create a `Point` with `x` and `y` initialized to zero.</span></span>

```vb
Dim p1 As Point = New Point()
Dim p2 As Point = New Point(0, 0)
```

<span data-ttu-id="cb403-258">Da Strukturen ihre Feldwerte direkt (anstatt Verweise auf diese Werte) enthalten, können Strukturen keine Felder enthalten, die sich direkt oder indirekt auf sich selbst beziehen.</span><span class="sxs-lookup"><span data-stu-id="cb403-258">Because structures directly contain their field values (rather than references to those values), structures cannot contain fields that directly or indirectly reference themselves.</span></span> <span data-ttu-id="cb403-259">Der folgende Code ist beispielsweise ungültig:</span><span class="sxs-lookup"><span data-stu-id="cb403-259">For example, the following code is not valid:</span></span>

```vb
Structure S1
    Dim f1 As S2
End Structure

Structure S2
    ' This would require S1 to contain itself.
    Dim f1 As S1
End Structure
```

<span data-ttu-id="cb403-260">Normalerweise kann eine Strukturmember-Deklaration nur über `Public`, `Friend`oder `Private` Access verfügen, aber wenn Sie von `Object`geerbte Member überschreiben, können auch `Protected` und `Protected Friend` Zugriff verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-260">Normally, a structure member declaration may only have `Public`, `Friend`, or `Private` access, but when overriding members inherited from `Object`, `Protected` and `Protected Friend` access may also be used.</span></span> <span data-ttu-id="cb403-261">Wenn eine Strukturmember-Deklaration keinen Zugriffsmodifizierer enthält, wird standardmäßig `Public` Access in der Deklaration verwendet.</span><span class="sxs-lookup"><span data-stu-id="cb403-261">When a structure member declaration does not include an access modifier, the declaration defaults to `Public` access.</span></span> <span data-ttu-id="cb403-262">Der Gültigkeitsbereich eines Members, der von einer-Struktur deklariert wird, ist der Struktur Text, in dem die Deklaration auftritt, sowie die Einschränkungen dieser Struktur (wenn Sie generisch und Einschränkungen enthielt).</span><span class="sxs-lookup"><span data-stu-id="cb403-262">The scope of a member declared by a structure is the structure body in which the declaration occurs, plus the constraints of that structure (if it was generic and had constraints).</span></span>


## <a name="standard-modules"></a><span data-ttu-id="cb403-263">Standard Module</span><span class="sxs-lookup"><span data-stu-id="cb403-263">Standard Modules</span></span>

<span data-ttu-id="cb403-264">Ein *Standardmodul* ist ein Typ, dessen Member implizit `Shared` werden und auf den Deklarations Bereich des enthaltenden Namespace des Standardmoduls beschränkt sind, und nicht nur auf die Standardmodul Deklaration selbst.</span><span class="sxs-lookup"><span data-stu-id="cb403-264">A *standard module* is a type whose members are implicitly `Shared` and scoped to the declaration space of the standard module's containing namespace, rather than just to the standard module declaration itself.</span></span> <span data-ttu-id="cb403-265">Standard Module werden möglicherweise nie instanziiert.</span><span class="sxs-lookup"><span data-stu-id="cb403-265">Standard modules may never be instantiated.</span></span> <span data-ttu-id="cb403-266">Es ist ein Fehler, eine Variable eines Standard Modultyps zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="cb403-266">It is an error to declare a variable of a standard module type.</span></span>

```antlr
ModuleDeclaration
    : Attributes? TypeModifier* 'Module' Identifier StatementTerminator
      ModuleMemberDeclaration*
      'End' 'Module' StatementTerminator
    ;
```

<span data-ttu-id="cb403-267">Ein Member eines Standardmoduls verfügt über zwei voll qualifizierte Namen, eine ohne den standardmodulnamen und eine mit dem standardmodulnamen.</span><span class="sxs-lookup"><span data-stu-id="cb403-267">A member of a standard module has two fully qualified names, one without the standard module name and one with the standard module name.</span></span> <span data-ttu-id="cb403-268">Mehrere Standardmodule in einem Namespace können einen Member mit einem bestimmten Namen definieren. nicht qualifizierte Verweise auf den Namen außerhalb eines der beiden Module sind mehrdeutig.</span><span class="sxs-lookup"><span data-stu-id="cb403-268">More than one standard module in a namespace may define a member with a particular name; unqualified references to the name outside of either module are ambiguous.</span></span> <span data-ttu-id="cb403-269">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cb403-269">For example:</span></span>

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

<span data-ttu-id="cb403-270">Ein Modul kann nur in einem Namespace deklariert werden und darf nicht in einem anderen Typ eingefügt werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-270">A module may only be declared in a namespace and may not be nested in another type.</span></span> <span data-ttu-id="cb403-271">Standard Module können keine Schnittstellen implementieren, Sie werden implizit von `Object`abgeleitet und verfügen nur über `Shared` Konstruktoren.</span><span class="sxs-lookup"><span data-stu-id="cb403-271">Standard modules may not implement interfaces, they implicitly derive from `Object`, and they have only `Shared` constructors.</span></span>


### <a name="standard-module-members"></a><span data-ttu-id="cb403-272">Standardmodulmember</span><span class="sxs-lookup"><span data-stu-id="cb403-272">Standard Module Members</span></span>

<span data-ttu-id="cb403-273">Die Member eines Standardmoduls sind die Member, die von den Element Deklarationen und den von `Object`geerbten Membern eingeführt werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-273">The members of a standard module are the members introduced by its member declarations and the members inherited from `Object`.</span></span> <span data-ttu-id="cb403-274">Standard Module können einen beliebigen Typ von Membern aufweisen, außer Instanzkonstruktoren.</span><span class="sxs-lookup"><span data-stu-id="cb403-274">Standard modules may have any type of member except instance constructors.</span></span> <span data-ttu-id="cb403-275">Alle standardmodultyp Elemente werden implizit `Shared`.</span><span class="sxs-lookup"><span data-stu-id="cb403-275">All standard module type members are implicitly `Shared`.</span></span>

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

<span data-ttu-id="cb403-276">Normalerweise kann eine standardmäßige Modulmember-Deklaration nur über `Public`, `Friend`oder `Private` Zugriff verfügen, aber wenn Sie von `Object`geerbte Member überschreiben, können die `Protected`-und `Protected Friend` Zugriffsmodifizierer angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-276">Normally, a standard module member declaration may only have `Public`, `Friend`, or `Private` access, but when overriding members inherited from `Object`, the `Protected` and `Protected Friend` access modifiers may be specified.</span></span> <span data-ttu-id="cb403-277">Wenn die Deklaration eines Standardmodulmembers keinen Zugriffsmodifizierer enthält, wird standardmäßig `Public` Access verwendet, es sei denn, es handelt sich um eine Variable, deren standardmäßig `Private` Access verwendet wird</span><span class="sxs-lookup"><span data-stu-id="cb403-277">When a standard module member declaration does not include an access modifier, the declaration defaults to `Public` access, unless it is a variable, which defaults to `Private` access.</span></span>

<span data-ttu-id="cb403-278">Wie bereits erwähnt, ist der Gültigkeitsbereich eines Standardmodulmembers die Deklaration, die die Standardmodul Deklaration enthält.</span><span class="sxs-lookup"><span data-stu-id="cb403-278">As previously noted, the scope of a standard module member is the declaration containing the standard module declaration.</span></span> <span data-ttu-id="cb403-279">Member, die von `Object` geerbt werden, sind in dieser speziellen Bereichs Definition nicht enthalten. Diese Member haben keinen Gültigkeitsbereich und müssen immer mit dem Namen des Moduls qualifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-279">Members inherited from `Object` are not included in this special scoping; those members have no scope and must always be qualified with the name of the module.</span></span> <span data-ttu-id="cb403-280">Wenn das Element `Friend` Access hat, erstreckt sich sein Bereich nur auf Namespace Elemente, die im selben Programm oder in Assemblys deklariert sind, denen `Friend` Zugriff erteilt wurde.</span><span class="sxs-lookup"><span data-stu-id="cb403-280">If the member has `Friend` access, its scope extends only to namespace members declared in the same program or assemblies that have been given `Friend` access.</span></span>


## <a name="interfaces"></a><span data-ttu-id="cb403-281">Schnittstellen</span><span class="sxs-lookup"><span data-stu-id="cb403-281">Interfaces</span></span>

<span data-ttu-id="cb403-282">*Schnittstellen* sind Verweis Typen, die andere Typen implementieren, um sicherzustellen, dass Sie bestimmte Methoden unterstützen.</span><span class="sxs-lookup"><span data-stu-id="cb403-282">*Interfaces* are reference types that other types implement to guarantee that they support certain methods.</span></span> <span data-ttu-id="cb403-283">Eine Schnittstelle wird nie direkt erstellt und hat keine tatsächliche Darstellung. andere Typen müssen in einen Schnittstellentyp konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-283">An interface is never directly created and has no actual representation -- other types must be converted to an interface type.</span></span> <span data-ttu-id="cb403-284">Eine Schnittstelle definiert einen Vertrag.</span><span class="sxs-lookup"><span data-stu-id="cb403-284">An interface defines a contract.</span></span> <span data-ttu-id="cb403-285">Eine Klasse oder Struktur, die eine Schnittstelle implementiert, muss ihren Vertrag einhalten.</span><span class="sxs-lookup"><span data-stu-id="cb403-285">A class or structure that implements an interface must adhere to its contract.</span></span>

```antlr
InterfaceDeclaration
    : Attributes? TypeModifier* 'Interface' Identifier
      TypeParameterList? StatementTerminator
      InterfaceBase*
      InterfaceMemberDeclaration*
      'End' 'Interface' StatementTerminator
    ;
```


<span data-ttu-id="cb403-286">Das folgende Beispiel zeigt eine Schnittstelle, die eine Standard Eigenschaft `Item`, einen Ereignis `E`, eine Methode `F`und eine Eigenschaften `P`enthält:</span><span class="sxs-lookup"><span data-stu-id="cb403-286">The following example shows an interface that contains a default property `Item`, an event `E`, a method `F`, and a property `P`:</span></span>

```vb
Interface IExample
    Default Property Item(index As Integer) As String

    Event E()

    Sub F(value As Integer)

    Property P() As String
End Interface
```

<span data-ttu-id="cb403-287">Schnittstellen können mehrere Vererbung verwenden.</span><span class="sxs-lookup"><span data-stu-id="cb403-287">Interfaces may employ multiple inheritance.</span></span> <span data-ttu-id="cb403-288">Im folgenden Beispiel erbt die-Schnittstelle `IComboBox` von `ITextBox` und `IListBox`:</span><span class="sxs-lookup"><span data-stu-id="cb403-288">In the following example, the interface `IComboBox` inherits from both `ITextBox` and `IListBox`:</span></span>

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

<span data-ttu-id="cb403-289">Klassen und Strukturen können mehrere Schnittstellen implementieren.</span><span class="sxs-lookup"><span data-stu-id="cb403-289">Classes and structures can implement multiple interfaces.</span></span> <span data-ttu-id="cb403-290">Im folgenden Beispiel wird die-Klasse `EditBox` von der-Klasse abgeleitet `Control` und sowohl `IControl` als auch `IDataBound`implementiert:</span><span class="sxs-lookup"><span data-stu-id="cb403-290">In the following example, the class `EditBox` derives from the class `Control` and implements both `IControl` and `IDataBound`:</span></span>

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


### <a name="interface-inheritance"></a><span data-ttu-id="cb403-291">Schnittstellenvererbung</span><span class="sxs-lookup"><span data-stu-id="cb403-291">Interface Inheritance</span></span>

<span data-ttu-id="cb403-292">Die Basis Schnittstellen einer Schnittstelle sind die expliziten Basis Schnittstellen und deren Basis Schnittstellen.</span><span class="sxs-lookup"><span data-stu-id="cb403-292">The base interfaces of an interface are the explicit base interfaces and their base interfaces.</span></span> <span data-ttu-id="cb403-293">Mit anderen Worten: der Satz von Basis Schnittstellen ist die komplette transitiv Schließung der expliziten Basis Schnittstellen, ihrer expliziten Basis Schnittstellen usw.</span><span class="sxs-lookup"><span data-stu-id="cb403-293">In other words, the set of base interfaces is the complete transitive closure of the explicit base interfaces, their explicit base interfaces, and so on.</span></span> <span data-ttu-id="cb403-294">Wenn eine Schnittstellen Deklaration keine explizite Schnittstellen Basis hat, gibt es keine Basisschnittstelle für den Typ---Schnittstellen erben nicht von `Object` (obwohl Sie über eine erweiternde Konvertierung in `Object`verfügen).</span><span class="sxs-lookup"><span data-stu-id="cb403-294">If an interface declaration has no explicit interface base, then there is no base interface for the type -- interfaces do not inherit from `Object` (although they do have a widening conversion to `Object`).</span></span>

```antlr
InterfaceBase
    : 'Inherits' InterfaceBases StatementTerminator
    ;

InterfaceBases
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

<span data-ttu-id="cb403-295">Im folgenden Beispiel sind die Basis Schnittstellen von `IComboBox` `IControl`, `ITextBox`und `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="cb403-295">In the following example, the base interfaces of `IComboBox` are `IControl`, `ITextBox`, and `IListBox`.</span></span>

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

<span data-ttu-id="cb403-296">Eine Schnittstelle erbt alle Member ihrer Basis Schnittstellen.</span><span class="sxs-lookup"><span data-stu-id="cb403-296">An interface inherits all members of its base interfaces.</span></span> <span data-ttu-id="cb403-297">Mit anderen Worten, die obige `IComboBox`-Schnittstelle erbt Member `SetText` und `SetItems` sowie `Paint`.</span><span class="sxs-lookup"><span data-stu-id="cb403-297">In other words, the `IComboBox` interface above inherits members `SetText` and `SetItems` as well as `Paint`.</span></span>

<span data-ttu-id="cb403-298">Eine Klasse oder Struktur, die eine Schnittstelle implementiert, implementiert auch implizit alle Basis Schnittstellen der Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="cb403-298">A class or structure that implements an interface also implicitly implements all of the interface's base interfaces.</span></span>

<span data-ttu-id="cb403-299">Wenn eine Schnittstelle bei der transitiven Schließung der Basis Schnittstellen mehrmals angezeigt wird, werden die Member nur einmal zur abgeleiteten Schnittstelle hinzugezogen.</span><span class="sxs-lookup"><span data-stu-id="cb403-299">If an interface appears more than once in the transitive closure of the base interfaces, it only contributes its members to the derived interface once.</span></span> <span data-ttu-id="cb403-300">Ein Typ, der die abgeleitete Schnittstelle implementiert, muss nur einmal die Methoden der mehrfach definierten Basisschnittstelle implementieren.</span><span class="sxs-lookup"><span data-stu-id="cb403-300">A type implementing the derived interface only has to implement the methods of the multiply defined base interface once.</span></span> <span data-ttu-id="cb403-301">Im folgenden Beispiel muss `Paint` nur einmal implementiert werden, auch wenn die-Klasse `IComboBox` und `IControl`implementiert.</span><span class="sxs-lookup"><span data-stu-id="cb403-301">In the following example, `Paint` only needs to be implemented once, even though the class implements `IComboBox` and `IControl`.</span></span>

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

<span data-ttu-id="cb403-302">Eine `Inherits`-Klausel hat keine Auswirkung auf andere `Inherits` Klauseln.</span><span class="sxs-lookup"><span data-stu-id="cb403-302">An `Inherits` clause has no effect on other `Inherits` clauses.</span></span> <span data-ttu-id="cb403-303">Im folgenden Beispiel müssen `IDerived` den Namen `INested` mit `IBase`qualifizieren.</span><span class="sxs-lookup"><span data-stu-id="cb403-303">In the following example, `IDerived` must qualify the name of `INested` with `IBase`.</span></span>

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

<span data-ttu-id="cb403-304">Die Zugriffs Domäne einer Basisschnittstelle muss mit oder einer übergeordneten Zugriffs Domäne der Schnittstelle selbst identisch sein.</span><span class="sxs-lookup"><span data-stu-id="cb403-304">The accessibility domain of a base interface must be the same as or a superset of the accessibility domain of the interface itself.</span></span>


### <a name="interface-members"></a><span data-ttu-id="cb403-305">Schnittstellenmember</span><span class="sxs-lookup"><span data-stu-id="cb403-305">Interface Members</span></span>

<span data-ttu-id="cb403-306">Die Member einer Schnittstelle bestehen aus den Membern, die von ihren Member-Deklarationen und den von ihren Basis Schnittstellen geerbten Membern eingeführt werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-306">The members of an interface consist of the members introduced by its member declarations and the members inherited from its base interfaces.</span></span>

```antlr
InterfaceMemberDeclaration
    : NonModuleDeclaration
    | InterfaceEventMemberDeclaration
    | InterfaceMethodMemberDeclaration
    | InterfacePropertyMemberDeclaration
    ;
```

<span data-ttu-id="cb403-307">Obwohl Schnittstellen keine Member von `Object`erben, da jede Klasse oder Struktur, die eine Schnittstelle implementiert, von `Object`erbt, werden die Member von `Object`, einschließlich der Erweiterungs Methoden, als Member einer Schnittstelle angesehen und können direkt für eine Schnittstelle aufgerufen werden, ohne dass eine Umwandlung in `Object`erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="cb403-307">Although interfaces do not inherit members from `Object`, because every class or structure that implements an interface does inherit from `Object`, the members of `Object`, including extension methods, are considered members of an interface and can be called on an interface directly without requiring a cast to `Object`.</span></span> <span data-ttu-id="cb403-308">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cb403-308">For example:</span></span>

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

<span data-ttu-id="cb403-309">Member einer Schnittstelle mit dem gleichen Namen wie Member von `Object` implizit Schatten `Object` Membern.</span><span class="sxs-lookup"><span data-stu-id="cb403-309">Members of an interface with the same name as members of `Object` implicitly shadow `Object` members.</span></span> <span data-ttu-id="cb403-310">Nur geschachtelte Typen, Methoden, Eigenschaften und Ereignisse können Member einer Schnittstelle sein.</span><span class="sxs-lookup"><span data-stu-id="cb403-310">Only nested types, methods, properties, and events may be members of an interface.</span></span> <span data-ttu-id="cb403-311">Methoden und Eigenschaften dürfen keinen Text enthalten.</span><span class="sxs-lookup"><span data-stu-id="cb403-311">Methods and properties may not have a body.</span></span> <span data-ttu-id="cb403-312">Schnittstellenmember sind implizit `Public` und können keinen Zugriffsmodifizierer angeben.</span><span class="sxs-lookup"><span data-stu-id="cb403-312">Interface members are implicitly `Public` and may not specify an access modifier.</span></span> <span data-ttu-id="cb403-313">Der Gültigkeitsbereich eines in einer Schnittstelle deklarierten Members ist der Schnittstellen Text, in dem die Deklaration auftritt, sowie die Einschränkungs Liste dieser Schnittstelle (wenn Sie generisch und Einschränkungen aufweist).</span><span class="sxs-lookup"><span data-stu-id="cb403-313">The scope of a member declared in an interface is the interface body in which the declaration occurs, plus the constraint list of that interface (if it is generic and has constraints).</span></span>


## <a name="arrays"></a><span data-ttu-id="cb403-314">Arrays</span><span class="sxs-lookup"><span data-stu-id="cb403-314">Arrays</span></span>

<span data-ttu-id="cb403-315">Ein *Array* ist ein Verweistyp, der Variablen enthält, auf die über *Indizes* zugegriffen wird, die einzeln mit der Reihenfolge der Variablen im Array übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="cb403-315">An *array* is a reference type that contains variables accessed through *indices* corresponding in a one-to-one fashion with the order of the variables in the array.</span></span> <span data-ttu-id="cb403-316">Die Variablen, die in einem Array enthalten sind, das auch als *Elemente* des Arrays bezeichnet wird, müssen alle denselben Typ aufweisen, und dieser Typ wird als *Elementtyp* des Arrays bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="cb403-316">The variables contained in an array, also called the *elements* of the array, must all be of the same type, and this type is called the *element type* of the array.</span></span>

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

<span data-ttu-id="cb403-317">Die Elemente eines Arrays entstehen, wenn eine Array Instanz erstellt wird, und sind nicht mehr vorhanden, wenn die Array Instanz zerstört wird.</span><span class="sxs-lookup"><span data-stu-id="cb403-317">The elements of an array come into existence when an array instance is created, and cease to exist when the array instance is destroyed.</span></span> <span data-ttu-id="cb403-318">Jedes Element eines Arrays wird mit dem Standardwert seines Typs initialisiert.</span><span class="sxs-lookup"><span data-stu-id="cb403-318">Each element of an array is initialized to the default value of its type.</span></span> <span data-ttu-id="cb403-319">Der Typ `System.Array` ist der Basistyp aller Array Typen und kann nicht instanziiert werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-319">The type `System.Array` is the base type of all array types and may not be instantiated.</span></span> <span data-ttu-id="cb403-320">Jeder Arraytyp erbt die vom `System.Array` Typ deklarierten Member und kann in ihn konvertiert werden (und `Object`).</span><span class="sxs-lookup"><span data-stu-id="cb403-320">Every array type inherits the members declared by the `System.Array` type and is convertible to it (and `Object`).</span></span> <span data-ttu-id="cb403-321">Ein eindimensionaler Arraytyp mit Element `T` implementiert auch die Schnittstellen `System.Collections.Generic.IList(Of T)` und `IReadOnlyList(Of T)`; Wenn `T` ein Referenztyp ist, implementiert der Arraytyp auch `IList(Of U)` und `IReadOnlyList(Of U)` für jede `U`, die über eine erweiternde Verweis Konvertierung aus `T`verfügt.</span><span class="sxs-lookup"><span data-stu-id="cb403-321">A one-dimensional array type with element `T` also implements the interfaces `System.Collections.Generic.IList(Of T)` and `IReadOnlyList(Of T)`; if `T` is a reference type, then the array type also implements `IList(Of U)` and `IReadOnlyList(Of U)` for any `U` that has a widening  reference conversion from `T`.</span></span>

<span data-ttu-id="cb403-322">Ein Array verfügt über einen *Rang* , der die Anzahl der Indizes bestimmt, die mit jedem Array Element verknüpft sind.</span><span class="sxs-lookup"><span data-stu-id="cb403-322">An array has a *rank* that determines the number of indices associated with each array element.</span></span> <span data-ttu-id="cb403-323">Der Rang eines Arrays bestimmt die Anzahl der *Dimensionen* des Arrays.</span><span class="sxs-lookup"><span data-stu-id="cb403-323">The rank of an array determines the number of *dimensions* of the array.</span></span> <span data-ttu-id="cb403-324">Beispielsweise wird ein Array mit dem Rang eins als eindimensionales Array bezeichnet, und ein Array mit einem Rang größer als eins wird als mehrdimensionales Array bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="cb403-324">For example, an array with a rank of one is called a single-dimensional array, and an array with a rank greater than one is called a multidimensional array.</span></span>

<span data-ttu-id="cb403-325">Im folgenden Beispiel wird ein eindimensionales Array von ganzzahligen Werten erstellt, die Array Elemente initialisiert und dann jede von Ihnen ausgegeben:</span><span class="sxs-lookup"><span data-stu-id="cb403-325">The following example creates a single-dimensional array of integer values, initializes the array elements, and then prints each of them out:</span></span>

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

<span data-ttu-id="cb403-326">Das Programm gibt Folgendes aus:</span><span class="sxs-lookup"><span data-stu-id="cb403-326">The program outputs the following:</span></span>

```console
arr(0) = 0
arr(1) = 1
arr(2) = 4
arr(3) = 9
arr(4) = 16
arr(5) = 25
```

<span data-ttu-id="cb403-327">Jede Dimension eines Arrays hat eine zugeordnete Länge.</span><span class="sxs-lookup"><span data-stu-id="cb403-327">Each dimension of an array has an associated length.</span></span> <span data-ttu-id="cb403-328">Dimensions Längen sind nicht Teil des Array Typs, sondern werden stattdessen festgelegt, wenn eine Instanz des Arraytyps zur Laufzeit erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="cb403-328">Dimension lengths are not part of the type of the array, but rather are established when an instance of the array type is created at run time.</span></span> <span data-ttu-id="cb403-329">Die Länge einer Dimension bestimmt den gültigen Bereich von Indizes für diese Dimension: für eine Dimension der Länge `N`können Indizes zwischen null und `N-1`reichen.</span><span class="sxs-lookup"><span data-stu-id="cb403-329">The length of a dimension determines the valid range of indices for that dimension: for a dimension of length `N`, indices can range from zero to `N-1`.</span></span> <span data-ttu-id="cb403-330">Wenn eine Dimension eine Länge von 0 (null) aufweist, sind für diese Dimension keine gültigen Indizes vorhanden.</span><span class="sxs-lookup"><span data-stu-id="cb403-330">If a dimension is of length zero, there are no valid indices for that dimension.</span></span> <span data-ttu-id="cb403-331">Die Gesamtanzahl der Elemente in einem Array ist das Produkt der Längen der einzelnen Dimensionen im Array.</span><span class="sxs-lookup"><span data-stu-id="cb403-331">The total number of elements in an array is the product of the lengths of each dimension in the array.</span></span> <span data-ttu-id="cb403-332">Wenn eine der Dimensionen eines Arrays eine Länge von 0 (null) aufweist, wird das Array als leer bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="cb403-332">If any of the dimensions of an array has a length of zero, the array is said to be empty.</span></span> <span data-ttu-id="cb403-333">Der Elementtyp eines Arrays kann ein beliebiger Typ sein.</span><span class="sxs-lookup"><span data-stu-id="cb403-333">The element type of an array can be any type.</span></span>

<span data-ttu-id="cb403-334">Array Typen werden angegeben, indem einem vorhandenen Typnamen ein-Modifizierer hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="cb403-334">Array types are specified by adding a modifier to an existing type name.</span></span> <span data-ttu-id="cb403-335">Der-Modifizierer besteht aus einer linken Klammer, einem Satz von NULL oder mehr Kommas und einer schließenden Klammer.</span><span class="sxs-lookup"><span data-stu-id="cb403-335">The modifier consists of a left parenthesis, a set of zero or more commas, and a right parenthesis.</span></span> <span data-ttu-id="cb403-336">Der Typ, der geändert wird, ist der Elementtyp des Arrays, und die Anzahl der Dimensionen ist die Anzahl von Kommas plus eins.</span><span class="sxs-lookup"><span data-stu-id="cb403-336">The type modified is the element type of the array, and the number of dimensions is the number of commas plus one.</span></span> <span data-ttu-id="cb403-337">Wenn mehr als ein Modifizierer angegeben wird, ist der Elementtyp des Arrays ein Array.</span><span class="sxs-lookup"><span data-stu-id="cb403-337">If more than one modifier is specified, then the element type of the array is an array.</span></span> <span data-ttu-id="cb403-338">Die Modifizierer werden von links nach rechts gelesen, wobei der äußerste-Modifizierer das äußerste Array ist.</span><span class="sxs-lookup"><span data-stu-id="cb403-338">The modifiers are read left to right, with the leftmost modifier being the outermost array.</span></span> <span data-ttu-id="cb403-339">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cb403-339">In the example</span></span>

```vb
Module Test
    Dim arr As Integer(,)(,,)()
End Module
```

<span data-ttu-id="cb403-340">der Elementtyp von `arr` ist ein zweidimensionales Array von dreidimensionalen Arrays von `Integer`.</span><span class="sxs-lookup"><span data-stu-id="cb403-340">the element type of `arr` is a two-dimensional array of three-dimensional arrays of one-dimensional arrays of `Integer`.</span></span>

<span data-ttu-id="cb403-341">Eine Variable kann auch als Arraytyp deklariert werden, indem ein Arraytypmodifizierer oder ein Initialisierungs Modifizierer für die Array Größe für den Variablennamen festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="cb403-341">A variable may also be declared to be of an array type by putting an array type modifier or an array-size initialization modifier on the variable name.</span></span> <span data-ttu-id="cb403-342">In diesem Fall ist der Array Elementtyp der Typ, der in der Deklaration angegeben ist, und die Array Dimensionen werden durch den Variablen namensmodifizierer bestimmt.</span><span class="sxs-lookup"><span data-stu-id="cb403-342">In that case, the array element type is the type given in the declaration, and the array dimensions are determined by the variable name modifier.</span></span> <span data-ttu-id="cb403-343">Aus Gründen der Übersichtlichkeit ist es nicht zulässig, einen Arraytypmodifizierer für einen Variablennamen und einen Typnamen in derselben Deklaration zu haben.</span><span class="sxs-lookup"><span data-stu-id="cb403-343">For clarity, it is not valid to have an array type modifier on both a variable name and a type name in the same declaration.</span></span>

<span data-ttu-id="cb403-344">Das folgende Beispiel zeigt eine Vielzahl von lokalen Variablen Deklarationen, die Array Typen mit `Integer` als Elementtyp verwenden:</span><span class="sxs-lookup"><span data-stu-id="cb403-344">The following example shows a variety of local variable declarations that use array types with `Integer` as the element type:</span></span>

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

<span data-ttu-id="cb403-345">Ein arraytypnamensmodifizierer erstreckt sich auf alle darauf folgenden Sätze von Klammern.</span><span class="sxs-lookup"><span data-stu-id="cb403-345">An array type name modifier extends to all sets of parentheses that follow it.</span></span> <span data-ttu-id="cb403-346">Dies bedeutet, dass in Situationen, in denen eine Reihe von Argumenten, die in Klammern eingeschlossen sind, nach einem Typnamen zulässig ist, dass es nicht möglich ist, die Argumente für einen arraytypnamen anzugeben.</span><span class="sxs-lookup"><span data-stu-id="cb403-346">This means that in the situations where a set of arguments enclosed in parenthesis is allowed after a type name, it is not possible to specify the arguments for an array type name.</span></span> <span data-ttu-id="cb403-347">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cb403-347">For example:</span></span>

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

<span data-ttu-id="cb403-348">Im letzten Fall wird `(3)` als Teil des Typnamens und nicht als Satz von Konstruktorargumenten interpretiert.</span><span class="sxs-lookup"><span data-stu-id="cb403-348">In the last case, `(3)` is interpreted as part of the type name rather than as a set of constructor arguments.</span></span>


## <a name="delegates"></a><span data-ttu-id="cb403-349">Delegaten</span><span class="sxs-lookup"><span data-stu-id="cb403-349">Delegates</span></span>

<span data-ttu-id="cb403-350">Ein *Delegat ist ein* Verweistyp, der auf eine `Shared` Methode eines Typs oder auf eine Instanzmethode eines Objekts verweist.</span><span class="sxs-lookup"><span data-stu-id="cb403-350">A *delegate* is a reference type that refers to a `Shared` method of a type or to an instance method of an object.</span></span>

```antlr
DelegateDeclaration
    : Attributes? TypeModifier* 'Delegate' MethodSignature StatementTerminator
    ;

MethodSignature
    : SubSignature
    | FunctionSignature
    ;
```

 <span data-ttu-id="cb403-351">Die nächstgelegene Entsprechung eines Delegaten in anderen Sprachen ist ein Funktionszeiger, aber ein Funktionszeiger kann nur auf `Shared` Funktionen verweisen, ein Delegat kann jedoch sowohl auf `Shared`-als auch auf Instanzmethoden verweisen.</span><span class="sxs-lookup"><span data-stu-id="cb403-351">The closest equivalent of a delegate in other languages is a function pointer, but whereas a function pointer can only reference `Shared` functions, a delegate can reference both `Shared` and instance methods.</span></span> <span data-ttu-id="cb403-352">Im letzteren Fall speichert der Delegat nicht nur einen Verweis auf den Einstiegspunkt der Methode, sondern auch einen Verweis auf die Objektinstanz, mit der die Methode aufgerufen werden soll.</span><span class="sxs-lookup"><span data-stu-id="cb403-352">In the latter case, the delegate stores not only a reference to the method's entry point, but also a reference to the object instance with which to invoke the method.</span></span>

<span data-ttu-id="cb403-353">Die Delegatdeklaration darf nicht über eine `Handles`-Klausel, eine `Implements`-Klausel, einen Methoden Text oder ein `End` Konstrukt verfügen.</span><span class="sxs-lookup"><span data-stu-id="cb403-353">The delegate declaration may not have  a `Handles` clause, an `Implements` clause, a method body, or an `End` construct.</span></span> <span data-ttu-id="cb403-354">Die Parameterliste der Delegatdeklaration darf keine `Optional` oder `ParamArray` Parameter enthalten.</span><span class="sxs-lookup"><span data-stu-id="cb403-354">The parameter list of the delegate declaration may not contain `Optional` or `ParamArray` parameters.</span></span> <span data-ttu-id="cb403-355">Die Zugriffs Domäne des Rückgabe Typs und der Parametertypen muss mit oder einer übergeordneten Zugriffs Domäne des Delegaten selbst identisch sein.</span><span class="sxs-lookup"><span data-stu-id="cb403-355">The accessibility domain of the return type and parameter types must be the same as or a superset of the accessibility domain of the delegate itself.</span></span>

<span data-ttu-id="cb403-356">Die Member eines Delegaten sind die Member, die von der Klasse `System.Delegate`geerbt werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-356">The members of a delegate are the members inherited from class `System.Delegate`.</span></span> <span data-ttu-id="cb403-357">Ein Delegat definiert auch die folgenden Methoden:</span><span class="sxs-lookup"><span data-stu-id="cb403-357">A delegate also defines the following methods:</span></span>

* <span data-ttu-id="cb403-358">Ein Konstruktor, der zwei Parameter annimmt, eine vom Typ `Object` und eine vom Typ `System.IntPtr`.</span><span class="sxs-lookup"><span data-stu-id="cb403-358">A constructor that takes two parameters, one of type `Object` and one of type `System.IntPtr`.</span></span>

* <span data-ttu-id="cb403-359">Eine `Invoke` Methode, die über dieselbe Signatur wie der Delegat verfügt.</span><span class="sxs-lookup"><span data-stu-id="cb403-359">An `Invoke` method that has the same signature as the delegate.</span></span>

* <span data-ttu-id="cb403-360">Eine `BeginInvoke` Methode, deren Signatur die Delegatsignatur ist, mit drei unterschieden.</span><span class="sxs-lookup"><span data-stu-id="cb403-360">A `BeginInvoke` method whose signature is the delegate signature, with three differences.</span></span> <span data-ttu-id="cb403-361">Zuerst wird der Rückgabetyp in "`System.IAsyncResult`" geändert.</span><span class="sxs-lookup"><span data-stu-id="cb403-361">First, the return type is changed to `System.IAsyncResult`.</span></span> <span data-ttu-id="cb403-362">Zweitens werden zwei zusätzliche Parameter am Ende der Parameterliste hinzugefügt: der erste vom Typ `System.AsyncCallback` und der zweite Typ `Object`.</span><span class="sxs-lookup"><span data-stu-id="cb403-362">Second, two additional parameters are added to the end of the parameter list: the first of type `System.AsyncCallback` and the second of type `Object`.</span></span> <span data-ttu-id="cb403-363">Und schließlich werden alle `ByRef` Parameter so geändert, dass Sie `ByVal`werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-363">And finally, all `ByRef` parameters are changed to be `ByVal`.</span></span>

* <span data-ttu-id="cb403-364">Eine `EndInvoke` Methode, deren Rückgabetyp mit dem Delegaten übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="cb403-364">An `EndInvoke` method whose return type is the same as the delegate.</span></span> <span data-ttu-id="cb403-365">Bei den Parametern der-Methode handelt es sich nur um die Delegatparameter, die in derselben Reihenfolge wie in der Delegatsignatur vorhanden sind `ByRef` Parameter.</span><span class="sxs-lookup"><span data-stu-id="cb403-365">The parameters of the method are only the delegate parameters exactly that are `ByRef` parameters, in the same order they occur in the delegate signature.</span></span>  <span data-ttu-id="cb403-366">Zusätzlich zu diesen Parametern gibt es einen zusätzlichen Parameter vom Typ `System.IAsyncResult` am Ende der Parameterliste.</span><span class="sxs-lookup"><span data-stu-id="cb403-366">In addition to those parameters, there is an additional parameter of type `System.IAsyncResult` at the end of the parameter list.</span></span>

<span data-ttu-id="cb403-367">Zum Definieren und Verwenden von Delegaten sind drei Schritte erforderlich: Deklaration, Instanziierung und Aufruf.</span><span class="sxs-lookup"><span data-stu-id="cb403-367">There are three steps in defining and using delegates: declaration, instantiation, and invocation.</span></span>

<span data-ttu-id="cb403-368">Delegaten werden mithilfe der Delegatdeklarationssyntax deklariert.</span><span class="sxs-lookup"><span data-stu-id="cb403-368">Delegates are declared using delegate declaration syntax.</span></span> <span data-ttu-id="cb403-369">Im folgenden Beispiel wird ein Delegat mit dem Namen `SimpleDelegate` deklariert, der keine Argumente annimmt:</span><span class="sxs-lookup"><span data-stu-id="cb403-369">The following example declares a delegate named `SimpleDelegate` that takes no arguments:</span></span>

```vb
Delegate Sub SimpleDelegate()
```

<span data-ttu-id="cb403-370">Im nächsten Beispiel wird eine `SimpleDelegate` Instanz erstellt und dann sofort aufgerufen:</span><span class="sxs-lookup"><span data-stu-id="cb403-370">The next example creates a `SimpleDelegate` instance and then immediately calls it:</span></span>

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

<span data-ttu-id="cb403-371">Es gibt keinen großen Punkt beim Instanziieren eines Delegaten für eine Methode und dann sofort über den Delegaten aufzurufen, da es einfacher wäre, die Methode direkt aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="cb403-371">There is not much point in instantiating a delegate for a method and then immediately calling via the delegate, as it would be simpler to call the method directly.</span></span> <span data-ttu-id="cb403-372">Delegaten zeigen ihre Nützlichkeit, wenn Ihre Anonymität verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="cb403-372">Delegates show their usefulness when their anonymity is used.</span></span> <span data-ttu-id="cb403-373">Das nächste Beispiel zeigt eine `MultiCall` Methode, die wiederholt eine `SimpleDelegate` Instanz aufruft:</span><span class="sxs-lookup"><span data-stu-id="cb403-373">The next example shows a `MultiCall` method that repeatedly calls a `SimpleDelegate` instance:</span></span>

```vb
Sub MultiCall(d As SimpleDelegate, count As Integer)
    Dim i As Integer

    For i = 0 To count - 1
        d()
    Next i
End Sub
```

<span data-ttu-id="cb403-374">Es ist unwichtig für die `MultiCall` Methode, wie die Ziel Methode für die `SimpleDelegate` ist, welche Barrierefreiheit diese Methode hat oder ob die Methode `Shared` ist oder nicht.</span><span class="sxs-lookup"><span data-stu-id="cb403-374">It is unimportant to the `MultiCall` method what the target method for the `SimpleDelegate` is, what accessibility this method has, or whether the method is `Shared` or not.</span></span> <span data-ttu-id="cb403-375">Alles, was wichtig ist, ist, dass die Signatur der Ziel Methode mit `SimpleDelegate`kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="cb403-375">All that matters is that the signature of the target method is compatible with `SimpleDelegate`.</span></span>


## <a name="partial-types"></a><span data-ttu-id="cb403-376">Partial types (Partielle Typen)</span><span class="sxs-lookup"><span data-stu-id="cb403-376">Partial types</span></span>

<span data-ttu-id="cb403-377">Klassen-und Struktur Deklarationen können *partielle* Deklarationen sein.</span><span class="sxs-lookup"><span data-stu-id="cb403-377">Class and structure declarations can be *partial* declarations.</span></span> <span data-ttu-id="cb403-378">In einer partiellen Deklaration kann der deklarierte Typ innerhalb der Deklaration vollständig beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-378">A partial declaration may or may not fully describe the declared type within the declaration.</span></span> <span data-ttu-id="cb403-379">Stattdessen kann die Deklaration des Typs auf mehrere partielle Deklarationen innerhalb des Programms verteilt werden. Partielle Typen können nicht über Programm Grenzen hinweg deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-379">Instead, the declaration of the type may be spread across multiple partial declarations within the program; partial types cannot be declared across program boundaries.</span></span> <span data-ttu-id="cb403-380">Eine partielle Typdeklaration gibt den `Partial` Modifizierer für die Deklaration an.</span><span class="sxs-lookup"><span data-stu-id="cb403-380">A partial type declaration specifies the `Partial` modifier on the declaration.</span></span> <span data-ttu-id="cb403-381">Anschließend werden alle anderen Deklarationen im Programm für einen Typ mit demselben voll qualifizierten Namen zusammen mit der partiellen Deklaration zur Kompilierzeit zusammengeführt, um eine einzelne Typdeklaration zu bilden.</span><span class="sxs-lookup"><span data-stu-id="cb403-381">Then, any other declarations in the program for a type with the same fully-qualified name will be merged together with the partial declaration at compile-time to form a single type declaration.</span></span> <span data-ttu-id="cb403-382">Der folgende Code deklariert z. b. eine einzelne Klasse `Test` mit Membern `Test.C1` und `Test.C2`.</span><span class="sxs-lookup"><span data-stu-id="cb403-382">For example, the following code declares a single class `Test` with members `Test.C1` and `Test.C2`.</span></span>

<span data-ttu-id="cb403-383">a. vb:</span><span class="sxs-lookup"><span data-stu-id="cb403-383">a.vb:</span></span>

```vb
Public Partial Class Test
    Public Sub S1()
    End Sub
End Class
```

<span data-ttu-id="cb403-384">b. vb:</span><span class="sxs-lookup"><span data-stu-id="cb403-384">b.vb:</span></span>

```vb
Public Class Test
    Public Sub S2()
    End Sub
End Class
```

<span data-ttu-id="cb403-385">Beim Kombinieren von partiellen Typdeklarationen muss mindestens eine der Deklarationen einen `Partial` Modifizierer aufweisen, andernfalls wird ein Kompilierzeitfehler ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="cb403-385">When combining partial type declarations, at least one of the declarations must have a `Partial` modifier, otherwise a compile-time error results.</span></span>

<span data-ttu-id="cb403-386">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="cb403-386">__Note.__</span></span> <span data-ttu-id="cb403-387">Obwohl es möglich ist, `Partial` nur für eine Deklaration zwischen vielen partiellen Deklarationen anzugeben, ist es besser, es für alle partiellen Deklarationen anzugeben.</span><span class="sxs-lookup"><span data-stu-id="cb403-387">Although it is possible to specify `Partial` on only one declaration among many partial declarations, it is better form to specify it on all partial declarations.</span></span> <span data-ttu-id="cb403-388">In der Situation, in der eine partielle Deklaration sichtbar ist, aber eine oder mehrere partielle Deklarationen ausgeblendet sind (z. b. bei der Erweiterung des Tool generierten Codes), ist es akzeptabel, den `Partial` Modifizierer der sichtbaren Deklaration zu lassen, ihn aber in den verborgenen Deklarationen anzugeben.</span><span class="sxs-lookup"><span data-stu-id="cb403-388">In the situation where one partial declaration is visible but one or more partial declarations are hidden (such as the case of extending tool-generated code), it is acceptable to leave the `Partial` modifier off of the visible declaration but specify it on the hidden declarations.</span></span>

<span data-ttu-id="cb403-389">Nur Klassen und Strukturen können mit partiellen Deklarationen deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-389">Only classes and structures can be declared using partial declarations.</span></span> <span data-ttu-id="cb403-390">Die Stelligkeit eines Typs wird berücksichtigt, wenn partielle Deklarationen miteinander abgeglichen werden: zwei Klassen mit dem gleichen Namen, aber unterschiedlicher Anzahl von Typparametern werden nicht als partielle Deklarationen gleichzeitig betrachtet.</span><span class="sxs-lookup"><span data-stu-id="cb403-390">The arity of a type is considered when matching partial declarations together: two classes with the same name but different numbers of type parameters are not considered to be partial declarations of the same time.</span></span> <span data-ttu-id="cb403-391">Partielle Deklarationen können Attribute, Klassenmodifizierer, `Inherits` Anweisung oder `Implements` Anweisung angeben.</span><span class="sxs-lookup"><span data-stu-id="cb403-391">Partial declarations can specify attributes, class modifiers, `Inherits` statement or `Implements` statement.</span></span> <span data-ttu-id="cb403-392">Zum Zeitpunkt der Kompilierung werden alle Teile der partiellen Deklarationen zusammen kombiniert und als Teil der Typdeklaration verwendet.</span><span class="sxs-lookup"><span data-stu-id="cb403-392">At compile time, all of the pieces of the partial declarations are combined together and used as a part of the type declaration.</span></span> <span data-ttu-id="cb403-393">Wenn Konflikte zwischen Attributen, modifiziererelementen, Basen, Schnittstellen oder Typmembern auftreten, wird ein Fehler bei der Kompilierzeit ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="cb403-393">If there are any conflicts between attributes, modifiers, bases, interfaces, or type members, a compile-time error results.</span></span> <span data-ttu-id="cb403-394">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cb403-394">For example:</span></span>

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

<span data-ttu-id="cb403-395">Im vorherigen Beispiel wird ein Typ deklariert, `Test1` `Public`ist, von `Object` erbt und `System.IDisposable` und `System.IComparable`implementiert.</span><span class="sxs-lookup"><span data-stu-id="cb403-395">The previous example declares a type `Test1` that is `Public`, inherits from `Object` and implements `System.IDisposable` and `System.IComparable`.</span></span> <span data-ttu-id="cb403-396">Die partiellen Deklarationen von `Test2` verursachen einen Kompilierzeitfehler, da eine der Deklarationen besagt, dass `Test2` `Public` ist und ein anderer besagt, dass `Test2` `Private`ist.</span><span class="sxs-lookup"><span data-stu-id="cb403-396">The partial declarations of `Test2` will cause a compile-time error because one of the declarations says that `Test2` is `Public` and another says that `Test2` is `Private`.</span></span>

<span data-ttu-id="cb403-397">Partielle Typen mit Typparametern können Einschränkungen und Varianz für die Typparameter deklarieren, aber die Einschränkungen und Varianz der einzelnen partiellen Deklarationen müssen stimmen.</span><span class="sxs-lookup"><span data-stu-id="cb403-397">Partial types with type parameters can declare constraints and variance for the type parameters, but the constraints and variance from each partial declaration must match.</span></span> <span data-ttu-id="cb403-398">Daher sind Einschränkungen und Varianz speziell darin, dass Sie nicht automatisch wie andere Modifizierer kombiniert werden:</span><span class="sxs-lookup"><span data-stu-id="cb403-398">Thus, constraints and variance are special in that they are not automatically combined like other modifiers:</span></span>

```vb
Partial Public Class List(Of T As IEnumerable)
End Class

' Error: Constraints on T don't match
Class List(Of T As IComparable)
End Class
```

<span data-ttu-id="cb403-399">Die Tatsache, dass ein Typ mit mehreren partiellen Deklarationen deklariert wird, hat keine Auswirkung auf die Regeln für die Namenssuche innerhalb des Typs.</span><span class="sxs-lookup"><span data-stu-id="cb403-399">The fact that a type is declared using multiple partial declarations does not affect the name lookup rules within the type.</span></span> <span data-ttu-id="cb403-400">Demzufolge kann eine partielle Typdeklaration Member verwenden, die in anderen partiellen Typdeklarationen deklariert sind, oder Methoden für Schnittstellen implementieren, die in anderen partiellen Typdeklarationen deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-400">As a result, a partial type declaration can use members declared in other partial type declarations, or may implement methods on interfaces declared in other partial type declarations.</span></span> <span data-ttu-id="cb403-401">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cb403-401">For example:</span></span>

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

<span data-ttu-id="cb403-402">In Form von Typen können auch partielle Deklarationen vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="cb403-402">Nested types can have partial declarations as well.</span></span> <span data-ttu-id="cb403-403">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cb403-403">For example:</span></span>

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

<span data-ttu-id="cb403-404">Initialisierer in einer partiellen Deklaration werden weiterhin in der Deklarations Reihenfolge ausgeführt. Es gibt jedoch keine garantierte Ausführungsreihenfolge für Initialisierer, die in separaten partiellen Deklarationen auftreten.</span><span class="sxs-lookup"><span data-stu-id="cb403-404">Initializers within a partial declaration will still be executed in declaration order; however, there is no guaranteed order of execution for initializers that occur in separate partial declarations.</span></span>

## <a name="constructed-types"></a><span data-ttu-id="cb403-405">Konstruierte Typen</span><span class="sxs-lookup"><span data-stu-id="cb403-405">Constructed Types</span></span>

<span data-ttu-id="cb403-406">Eine generische Typdeklaration bezeichnet allein keinen Typ.</span><span class="sxs-lookup"><span data-stu-id="cb403-406">A generic type declaration, by itself, does not denote a type.</span></span> <span data-ttu-id="cb403-407">Stattdessen kann eine generische Typdeklaration als "Blueprint" verwendet werden, um viele verschiedene Typen durch Anwenden von Typargumenten zu bilden.</span><span class="sxs-lookup"><span data-stu-id="cb403-407">Instead, a generic type declaration can be used as a "blueprint" to form many different types by applying type arguments.</span></span> <span data-ttu-id="cb403-408">Ein generischer Typ, auf den die Typargumente angewendet werden, wird als *konstruierter Typ*bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="cb403-408">A generic type that has type arguments applied to it is called a *constructed type*.</span></span> <span data-ttu-id="cb403-409">Die Typargumente in einem konstruierten Typ müssen immer die Einschränkungen erfüllen, die auf die Typparameter angewendet werden, denen Sie entsprechen.</span><span class="sxs-lookup"><span data-stu-id="cb403-409">The type arguments in a constructed type must always satisfy the constraints placed on the type parameters they match to.</span></span>

<span data-ttu-id="cb403-410">Ein Typname kann einen konstruierten Typ identifizieren, auch wenn er keine Typparameter direkt angibt.</span><span class="sxs-lookup"><span data-stu-id="cb403-410">A type name might identify a constructed type even though it doesn't specify type parameters directly.</span></span> <span data-ttu-id="cb403-411">Dies kann vorkommen, wenn ein Typ in einer generischen Klassen Deklaration geschachtelt ist und der Instanztyp der enthaltenden Deklaration implizit für die Namenssuche verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="cb403-411">This can occur where a type is nested within a generic class declaration, and the instance type of the containing declaration is implicitly used for name lookup:</span></span>

```vb
Class Outer(Of T) 
    Public Class Inner 
    End Class

    ' Type of i is the constructed type Outer(Of T).Inner
    Public i As Inner 
End Class
```

<span data-ttu-id="cb403-412">Auf einen konstruierten Typ `C(Of T1,...,Tn)` auf den zugegriffen werden kann, wenn der generische Typ und alle Typargumente zugänglich sind.</span><span class="sxs-lookup"><span data-stu-id="cb403-412">A constructed type `C(Of T1,...,Tn)` is accessible when the generic type and all the type arguments are accessible.</span></span> <span data-ttu-id="cb403-413">Wenn beispielsweise der generische Typ `C` `Public` ist und alle Typargumente `T1,...,Tn` `Public`sind, ist der konstruierte Typ `Public`.</span><span class="sxs-lookup"><span data-stu-id="cb403-413">For instance, if the generic type `C` is `Public` and all of the type arguments `T1,...,Tn` are `Public`, then the constructed type is `Public`.</span></span> <span data-ttu-id="cb403-414">Wenn entweder der Typname oder eines der Typargumente `Private`ist, dann ist der Zugriff auf den konstruierten Typ `Private`.</span><span class="sxs-lookup"><span data-stu-id="cb403-414">If either the type name or one of the type arguments is `Private`, however, then the accessibility of the constructed type is `Private`.</span></span> <span data-ttu-id="cb403-415">Wenn ein Typargument des konstruierten Typs `Protected` und ein anderes Typargument `Friend`ist, kann auf den konstruierten Typ nur in der-Klasse und den zugehörigen Unterklassen in dieser Assembly oder auf einer beliebigen Assembly zugegriffen werden, der `Friend` Zugriff erteilt wurde.</span><span class="sxs-lookup"><span data-stu-id="cb403-415">If one type argument of the constructed type is `Protected` and another type argument is `Friend`, then the constructed type is accessible only in the class and its subclasses in this assembly or any assembly that has been given `Friend` access.</span></span> <span data-ttu-id="cb403-416">Das heißt, die Zugriffs Domäne für einen konstruierten Typ ist die Schnittmenge der Barrierefreiheits Domänen der Bestandteile.</span><span class="sxs-lookup"><span data-stu-id="cb403-416">In other words, the accessibility domain for a constructed type is the intersection of the accessibility domains of its constituent parts.</span></span>

<span data-ttu-id="cb403-417">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="cb403-417">__Note.__</span></span> <span data-ttu-id="cb403-418">Die Tatsache, dass es sich bei der Zugriffsdomäne des konstruierten Typs um die Schnittmenge der gebildeten Teile handelt, hat den interessanten Nebeneffekt, dass eine neue Barrierefreiheits Stufe definiert</span><span class="sxs-lookup"><span data-stu-id="cb403-418">The fact that the accessibility domain of constructed type is the intersection of its constituted parts has the interesting side effect of defining a new accessibility level.</span></span> <span data-ttu-id="cb403-419">Ein konstruierter Typ, der ein Element enthält, das `Protected` ist, und auf das ein Element `Friend` kann nur in Kontexten zugegriffen werden, die *sowohl* auf `Friend` *als auch* auf `Protected` Member zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="cb403-419">A constructed type that contains an element that is `Protected` and an element that is `Friend` can only be accessed in contexts that can access *both* `Friend` *and* `Protected` members.</span></span> <span data-ttu-id="cb403-420">Allerdings gibt es keine Möglichkeit, diese Barrierefreiheits Stufe in der Sprache auszudrücken, da die Barrierefreiheits `Protected Friend` bedeutet, dass auf eine Entität in einem Kontext zugegriffen werden kann, der *auf `Friend`* *oder* `Protected` Member zugreifen kann.</span><span class="sxs-lookup"><span data-stu-id="cb403-420">However, there is no way to express this accessibility level in the language, as the accessibility `Protected Friend` means that an entity can be accessed in a context that can access *either* `Friend` *or* `Protected` members.</span></span>

<span data-ttu-id="cb403-421">Die Basis, die implementierten Schnittstellen und Member konstruierter Typen werden bestimmt, indem die angegebenen Typargumente für jedes Vorkommen des Typparameters im generischen Typ ersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-421">The base, implemented interfaces and members of constructed types are determined by substituting the supplied type arguments for each occurrence of the type parameter in the generic type.</span></span>

### <a name="open-types-and-closed-types"></a><span data-ttu-id="cb403-422">Öffnen von Typen und geschlossenen Typen</span><span class="sxs-lookup"><span data-stu-id="cb403-422">Open Types and Closed Types</span></span>

<span data-ttu-id="cb403-423">Ein konstruierter Typ, für den ein oder mehrere Typargumente Typparameter eines enthaltenden Typs oder einer Methode sind, wird als *offener Typ*bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="cb403-423">A constructed type for who one or more type arguments are type parameters of a containing type or method is called an *open type*.</span></span> <span data-ttu-id="cb403-424">Dies liegt daran, dass einige Typparameter des Typs immer noch nicht bekannt sind, sodass die tatsächliche Form des Typs noch nicht vollständig bekannt ist.</span><span class="sxs-lookup"><span data-stu-id="cb403-424">This is because some of the type parameters of the type are still not known, so the actual shape of the type is not yet fully known.</span></span> <span data-ttu-id="cb403-425">Im Gegensatz dazu wird ein generischer Typ, dessen Typargumente alle nicht-Typparameter sind, als *geschlossener Typ*bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="cb403-425">In contrast, a generic type whose type arguments are all non-type parameters is called a *closed type*.</span></span> <span data-ttu-id="cb403-426">Die Form eines geschlossenen Typs ist immer vollständig bekannt.</span><span class="sxs-lookup"><span data-stu-id="cb403-426">The shape of a closed type is always fully known.</span></span> <span data-ttu-id="cb403-427">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cb403-427">For example:</span></span>

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

<span data-ttu-id="cb403-428">Der erstellte Typ `Base(Of Integer, V)` ist ein offener Typ, da der Typparameter, `U` der `T` bereitgestellt wurde, ein weiterer Typparameter bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="cb403-428">The constructed type `Base(Of Integer, V)` is an open type because although the type parameter `T` has been supplied, the type parameter `U` has been supplied another type parameter.</span></span> <span data-ttu-id="cb403-429">Daher ist die vollständige Form des Typs noch nicht bekannt.</span><span class="sxs-lookup"><span data-stu-id="cb403-429">Thus, the full shape of the type is not yet known.</span></span> <span data-ttu-id="cb403-430">Der konstruierte Typ `Derived(Of Double)`jedoch ein geschlossener Typ, da alle Typparameter in der Vererbungs Hierarchie bereitgestellt wurden.</span><span class="sxs-lookup"><span data-stu-id="cb403-430">The constructed type `Derived(Of Double)`, however, is a closed type because all type parameters in the inheritance hierarchy have been supplied.</span></span>

<span data-ttu-id="cb403-431">Geöffnete Typen werden wie folgt definiert:</span><span class="sxs-lookup"><span data-stu-id="cb403-431">Open types are defined as follows:</span></span>

* <span data-ttu-id="cb403-432">Ein Typparameter ist ein offener Typ.</span><span class="sxs-lookup"><span data-stu-id="cb403-432">A type parameter is an open type.</span></span>

* <span data-ttu-id="cb403-433">Ein Arraytyp ist ein offener Typ, wenn sein Elementtyp ein offener Typ ist.</span><span class="sxs-lookup"><span data-stu-id="cb403-433">An array type is an open type if its element type is an open type.</span></span>

* <span data-ttu-id="cb403-434">Ein konstruierter Typ ist ein offener Typ, wenn mindestens eines der Typargumente ein offener Typ ist.</span><span class="sxs-lookup"><span data-stu-id="cb403-434">A constructed type is an open type if one or more of its type arguments are an open type.</span></span>

* <span data-ttu-id="cb403-435">Ein geschlossener Typ ist ein Typ, bei dem es sich nicht um einen geöffneten Typ handelt.</span><span class="sxs-lookup"><span data-stu-id="cb403-435">A closed type is a type that is not an open type.</span></span>

<span data-ttu-id="cb403-436">Da der Programm Einstiegspunkt nicht in einem generischen Typ vorliegen kann, sind alle zur Laufzeit verwendeten Typen geschlossene Typen.</span><span class="sxs-lookup"><span data-stu-id="cb403-436">Because the program entry point cannot be in a generic type, all types used at run-time will be closed types.</span></span>

## <a name="special-types"></a><span data-ttu-id="cb403-437">Besondere Typen</span><span class="sxs-lookup"><span data-stu-id="cb403-437">Special Types</span></span>

<span data-ttu-id="cb403-438">Die .NET Framework enthält eine Reihe von Klassen, die speziell vom .NET Framework und der Visual Basic Sprache behandelt werden:</span><span class="sxs-lookup"><span data-stu-id="cb403-438">The .NET Framework contains a number of classes that are treated specially by the .NET Framework and by the Visual Basic language:</span></span>

<span data-ttu-id="cb403-439">Der Typ `System.Void`, der einen void-Typ in der .NET Framework darstellt, kann nur in `GetType` Ausdrücken direkt referenziert werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-439">The type `System.Void`, which represents a void type in the .NET Framework, can be directly referenced only in `GetType` expressions.</span></span>

<span data-ttu-id="cb403-440">Die Typen `System.RuntimeArgumentHandle`, `System.ArgIterator` und `System.TypedReference` alle können Zeiger auf den Stapel enthalten und können daher nicht auf dem .NET Framework Heap angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-440">The types `System.RuntimeArgumentHandle`, `System.ArgIterator` and `System.TypedReference` all can contain pointers into the stack and so cannot appear on the .NET Framework heap.</span></span> <span data-ttu-id="cb403-441">Daher können Sie nicht als Array Elementtypen, Rückgabe Typen, Feldtypen, generische Typargumente, Typen, die NULL-Werte zulassen, `ByRef` Parametertypen, den Typ eines Werts, der in `Object` oder `System.ValueType`konvertiert wird, das Ziel eines aufzurufenden Instanzmembers von `Object` oder `System.ValueType`oder eine Sperre in eine Sperre verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cb403-441">Therefore, they cannot be used as array element types, return types, field types, generic type arguments, nullable types, `ByRef` parameter types, the type of a value being converted to `Object` or `System.ValueType`, the target of a call to instance members of `Object` or `System.ValueType`, or lifted into a closure.</span></span>
