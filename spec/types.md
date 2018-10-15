# <a name="types"></a><span data-ttu-id="f92f4-101">Typen</span><span class="sxs-lookup"><span data-stu-id="f92f4-101">Types</span></span>

<span data-ttu-id="f92f4-102">Die zwei grundlegenden Kategorien von Typen in Visual Basic sind *Werttypen* und *Verweistypen*.</span><span class="sxs-lookup"><span data-stu-id="f92f4-102">The two fundamental categories of types in Visual Basic are *value types* and *reference types*.</span></span> <span data-ttu-id="f92f4-103">Primitive Typen (mit Ausnahme von Zeichenfolgen), Enumerationen und Strukturen sind Werttypen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-103">Primitive types (except strings), enumerations, and structures are value types.</span></span> <span data-ttu-id="f92f4-104">Klassen, Zeichenfolgen, standard-Module, Schnittstellen, Arrays und Delegaten sind Verweistypen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-104">Classes, strings, standard modules, interfaces, arrays, and delegates are reference types.</span></span>

<span data-ttu-id="f92f4-105">Jeder Typ verfügt über eine *Standardwert*, dies ist der Wert, der auf Variablen dieses Typs bei der Initialisierung zugewiesen ist.</span><span class="sxs-lookup"><span data-stu-id="f92f4-105">Every type has a *default value*, which is the value that is assigned to variables of that type upon initialization.</span></span>

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

## <a name="value-types-and-reference-types"></a><span data-ttu-id="f92f4-106">Wert- und Verweistypen</span><span class="sxs-lookup"><span data-stu-id="f92f4-106">Value Types and Reference Types</span></span>

<span data-ttu-id="f92f4-107">Obwohl Werttypen und Referenztypen ähnliche Deklaration Syntax- und Nutzungsinformationen sein können, unterscheiden sich die Semantik.</span><span class="sxs-lookup"><span data-stu-id="f92f4-107">Although value types and reference types can be similar in terms of declaration syntax and usage, their semantics are distinct.</span></span>

<span data-ttu-id="f92f4-108">Verweistypen werden auf dem Heap der Laufzeit gespeichert. Sie können nur über einen Verweis auf den Speicher zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="f92f4-108">Reference types are stored on the run-time heap; they may only be accessed through a reference to that storage.</span></span> <span data-ttu-id="f92f4-109">Da Verweistypen immer über Verweise zugegriffen werden, wird ihre Lebensdauer von .NET Framework verwaltet.</span><span class="sxs-lookup"><span data-stu-id="f92f4-109">Because reference types are always accessed through references, their lifetime is managed by the .NET Framework.</span></span> <span data-ttu-id="f92f4-110">Ausstehenden Verweise auf eine bestimmte Instanz werden nachverfolgt, und die Instanz wird zerstört, nur, wenn keine weiteren Verweise bleiben.</span><span class="sxs-lookup"><span data-stu-id="f92f4-110">Outstanding references to a particular instance are tracked and the instance is destroyed only when no more references remain.</span></span> <span data-ttu-id="f92f4-111">Eine Variable eines Referenztyps enthält einen Verweis auf einen Wert dieses Typs, den Wert eines stärker abgeleiteten Typs oder ein null-Wert.</span><span class="sxs-lookup"><span data-stu-id="f92f4-111">A variable of reference type contains a reference to a value of that type, a value of a more derived type, or a null value.</span></span> <span data-ttu-id="f92f4-112">Ein *null-Wert* bezieht sich auf "nothing"; Es ist nicht möglich, die nichts mit einem null-Wert darf nur zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-112">A *null value* refers to nothing; it is not possible to do anything with a null value except assign it.</span></span> <span data-ttu-id="f92f4-113">Zuweisung zu einer Variable eines Referenztyps erstellt eine Kopie des Werts auf die verwiesen wird, anstatt eine Kopie des Verweises.</span><span class="sxs-lookup"><span data-stu-id="f92f4-113">Assignment to a variable of a reference type creates a copy of the reference rather than a copy of the referenced value.</span></span> <span data-ttu-id="f92f4-114">Für eine Variable eines Referenztyps ist der Standardwert ein null-Wert.</span><span class="sxs-lookup"><span data-stu-id="f92f4-114">For a variable of a reference type, the default value is a null value.</span></span>

<span data-ttu-id="f92f4-115">Werttypen werden direkt auf dem Stapel, entweder in einem Array oder in einem anderen Typ gespeichert. der Speicher kann nur direkt zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="f92f4-115">Value types are stored directly on the stack, either within an array or within another type; their storage can only be accessed directly.</span></span> <span data-ttu-id="f92f4-116">Da Werttypen, direkt in der Variablen gespeichert werden, wird deren Lebensdauer durch die Lebensdauer der Variablen bestimmt, die sie enthält.</span><span class="sxs-lookup"><span data-stu-id="f92f4-116">Because value types are stored directly within variables, their lifetime is determined by the lifetime of the variable that contains them.</span></span> <span data-ttu-id="f92f4-117">Wenn Sie der Speicherort mit einer Werttypinstanz zerstört wird, wird die Werttypinstanz auch zerstört.</span><span class="sxs-lookup"><span data-stu-id="f92f4-117">When the location containing a value type instance is destroyed, the value type instance is also destroyed.</span></span> <span data-ttu-id="f92f4-118">Werttypen sind immer direkt zugegriffen werden. Es ist nicht möglich, einen Verweis auf einen Werttyp zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-118">Value types are always accessed directly; it is not possible to create a reference to a value type.</span></span> <span data-ttu-id="f92f4-119">Einen solchen Verweis Verbot macht es unmöglich, die auf eine Klasseninstanz Wert verweisen, die zerstört wurde.</span><span class="sxs-lookup"><span data-stu-id="f92f4-119">Prohibiting such a reference makes it impossible to refer to a value class instance that has been destroyed.</span></span> <span data-ttu-id="f92f4-120">Da Werttypen, immer sind `NotInheritable`, eine Variable eines Werttyps enthält immer den Wert dieses Typs.</span><span class="sxs-lookup"><span data-stu-id="f92f4-120">Because value types are always `NotInheritable`, a variable of a value type always contains a value of that type.</span></span> <span data-ttu-id="f92f4-121">Aus diesem Grund der Wert eines Werttyps handelt es sich nicht um einen null-Wert, noch können sie ein Objekt eines stärker abgeleiteten Typs verweisen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-121">Because of this, the value of a value type cannot be a null value, nor can it reference an object of a more derived type.</span></span> <span data-ttu-id="f92f4-122">Zuweisung zu einer Variable eines Werttyps erstellt eine Kopie der Wert zugewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="f92f4-122">Assignment to a variable of a value type creates a copy of the value being assigned.</span></span> <span data-ttu-id="f92f4-123">Für eine Variable eines Werttyps ist der Standardwert für das Ergebnis der einzelnen Variablen Member des Typs auf den Standardwert zu initialisieren.</span><span class="sxs-lookup"><span data-stu-id="f92f4-123">For a variable of a value type, the default value is the result of initializing each variable member of the type to its default value.</span></span>

<span data-ttu-id="f92f4-124">Das folgende Beispiel zeigt den Unterschied zwischen Verweistypen und Werttypen:</span><span class="sxs-lookup"><span data-stu-id="f92f4-124">The following example shows the difference between reference types and value types:</span></span>

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

<span data-ttu-id="f92f4-125">Die Ausgabe des Programms lautet:</span><span class="sxs-lookup"><span data-stu-id="f92f4-125">The output of the program is:</span></span>

```
Values: 0, 123
Refs: 123, 123
```

<span data-ttu-id="f92f4-126">Die Zuweisung auf die lokale Variable `val2` wirkt sich nicht auf die lokale Variable `val1` da beide lokalen Variablen einen Werttyp sind (welche `Integer`) und jede lokale Variable eines Werttyps hat seinen eigenen Speicher.</span><span class="sxs-lookup"><span data-stu-id="f92f4-126">The assignment to the local variable `val2` does not impact the local variable `val1` because both local variables are of a value type (the type `Integer`) and each local variable of a value type has its own storage.</span></span> <span data-ttu-id="f92f4-127">Im Gegensatz dazu sind die Zuweisung `ref2.Value = 123;` wirkt sich auf das Objekt, das sowohl `ref1` und `ref2` Verweis.</span><span class="sxs-lookup"><span data-stu-id="f92f4-127">In contrast, the assignment `ref2.Value = 123;` affects the object that both `ref1` and `ref2` reference.</span></span>

<span data-ttu-id="f92f4-128">Über das .NET Framework-Typsystem zu beachten ist, die, obwohl Strukturen, Enumerationen und primitive Typen (mit Ausnahme von `String`) Werttypen sind, erben alle Verweistypen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-128">One thing to note about the .NET Framework type system is that even though structures, enumerations and primitive types (except for `String`) are value types, they all inherit from reference types.</span></span> <span data-ttu-id="f92f4-129">Strukturen und den primitiven Typen erben von der Verweistyp `System.ValueType`, erbt von `Object`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-129">Structures and the primitive types inherit from the reference type `System.ValueType`, which inherits from `Object`.</span></span> <span data-ttu-id="f92f4-130">Aufgelistete Typen erben von der Verweistyp `System.Enum`, erbt von `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-130">Enumerated types inherit from the reference type `System.Enum`, which inherits from `System.ValueType`.</span></span>

### <a name="nullable-value-types"></a><span data-ttu-id="f92f4-131">Auf NULL festlegbare Werttypen</span><span class="sxs-lookup"><span data-stu-id="f92f4-131">Nullable Value Types</span></span>

<span data-ttu-id="f92f4-132">Für Werttypen ein `?` Modifizierer kann hinzugefügt werden, um einen Namen zur Darstellung der *auf NULL festlegbare* Version des betreffenden Typs.</span><span class="sxs-lookup"><span data-stu-id="f92f4-132">For value types, a `?` modifier can be added to a type name to represent the *nullable* version of that type.</span></span>

```antlr
NullableTypeName
    : NonArrayTypeName '?'
    ;

NullableNameModifier
    : '?'
    ;
```

<span data-ttu-id="f92f4-133">NULL-Werte zulassen, kann die gleichen Werte wie die nicht auf NULL festlegbare Version des Typs sowie den null-Wert enthalten.</span><span class="sxs-lookup"><span data-stu-id="f92f4-133">A nullable value type can contain the same values as the non-nullable version of the type as well as the null value.</span></span> <span data-ttu-id="f92f4-134">Daher ist es bei einem auf NULL festlegbaren Werttyp zuweisen von `Nothing` auf eine Variable des Typs legt den Wert der Variablen, die null-Wert, nicht den Wert des Werttyps.</span><span class="sxs-lookup"><span data-stu-id="f92f4-134">Thus, for a nullable value type, assigning `Nothing` to a variable of the type sets the value of the variable to the null value, not the zero value of the value type.</span></span> <span data-ttu-id="f92f4-135">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f92f4-135">For example:</span></span>

```vb
Dim x As Integer = Nothing
Dim y As Integer? = Nothing

' Prints zero
Console.WriteLine(x)
' Prints nothing (because the value of y is the null value)
Console.WriteLine(y)
```

<span data-ttu-id="f92f4-136">Auch kann eine Variable deklariert werden, der ein Werttyp sein, einen nullable-Typ-Modifizierer in den Namen der Variablen platzieren.</span><span class="sxs-lookup"><span data-stu-id="f92f4-136">A variable may also be declared to be of a nullable value type by putting a nullable type modifier on the variable name.</span></span> <span data-ttu-id="f92f4-137">Aus Gründen der Übersichtlichkeit ist es nicht zulässig, einen nullable-Typ-Modifizierer auf einen Variablennamen und einen Typnamen in der gleichen Deklaration haben.</span><span class="sxs-lookup"><span data-stu-id="f92f4-137">For clarity, it is not valid to have a nullable type modifier on both a variable name and a type name in the same declaration.</span></span> <span data-ttu-id="f92f4-138">Da auf NULL festlegbare Typen implementiert werden, mit dem Typ `System.Nullable(Of T)`, den Typ `T?` ist ein Synonym für den Typ `System.Nullable(Of T)`, und die beiden Namen austauschbar verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="f92f4-138">Since nullable types are implemented using the type `System.Nullable(Of T)`, the type `T?` is synonymous to the type `System.Nullable(Of T)`, and the two names can be used interchangeably.</span></span> <span data-ttu-id="f92f4-139">Die `?` Modifizierer kann nicht auf einen Typ, der bereits auf NULL festlegbar ist platziert werden; daher ist es nicht möglich, den Typ deklarieren, `Integer??` oder `System.Nullable(Of Integer)?`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-139">The `?` modifier cannot be placed on a type that is already nullable; thus, it is not possible to declare the type `Integer??` or `System.Nullable(Of Integer)?`.</span></span>

<span data-ttu-id="f92f4-140">Ein Werttyp `T?` verfügt über die Member der `System.Nullable(Of T)` sowie Operatoren oder Konvertierungen *angehoben* aus dem zugrunde liegenden Typ `T` in den Typ `T?`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-140">A nullable value type `T?` has the members of `System.Nullable(Of T)` as well as any operators or conversions *lifted* from the underlying type `T` into the type `T?`.</span></span> <span data-ttu-id="f92f4-141">Heben die Kopien-Operatoren und Konvertierungen von den zugrunde liegenden Typ, in den meisten Fällen auf NULL festlegbare Werttypen für nicht auf NULL festlegbare Werttypen ersetzen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-141">Lifting copies operators and conversions from the underlying type, in most cases substituting nullable value types for non-nullable value types.</span></span> <span data-ttu-id="f92f4-142">Auf diese Weise können viele der gleichen Konvertierungen und Vorgänge, die für gelten `T` zuweisen `T?` ebenfalls.</span><span class="sxs-lookup"><span data-stu-id="f92f4-142">This allows many of the same conversions and operations that apply to `T` to apply to `T?` as well.</span></span>


## <a name="interface-implementation"></a><span data-ttu-id="f92f4-143">Schnittstellenimplementierung</span><span class="sxs-lookup"><span data-stu-id="f92f4-143">Interface Implementation</span></span>

<span data-ttu-id="f92f4-144">Struktur und Klassendeklarationen können deklarieren, dass sie einen Satz von Schnittstellentypen über einen oder mehrere implementieren `Implements` Klauseln.</span><span class="sxs-lookup"><span data-stu-id="f92f4-144">Structure and class declarations may declare that they implement a set of interface types through one or more `Implements` clauses.</span></span>

```antlr
TypeImplementsClause
    : 'Implements' TypeImplements StatementTerminator
    ;

TypeImplements
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

<span data-ttu-id="f92f4-145">Alle Typen im angegebenen die `Implements` Klausel Schnittstellen sein, und der Typ muss alle Member der Schnittstellen implementieren.</span><span class="sxs-lookup"><span data-stu-id="f92f4-145">All the types specified in the `Implements` clause must be interfaces, and the type must implement all members of the interfaces.</span></span> <span data-ttu-id="f92f4-146">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f92f4-146">For example:</span></span>

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

<span data-ttu-id="f92f4-147">Ein Typ, der eine Schnittstelle, auch implizit implementiert implementiert alle Schnittstellen der Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="f92f4-147">A type that implements an interface also implicitly implements all of the interface's base interfaces.</span></span> <span data-ttu-id="f92f4-148">Dies gilt auch, wenn alle Schnittstellen in der Typ nicht explizit aufgelistet werden die `Implements` Klausel.</span><span class="sxs-lookup"><span data-stu-id="f92f4-148">This is true even if the type does not explicitly list all base interfaces in the `Implements` clause.</span></span> <span data-ttu-id="f92f4-149">In diesem Beispiel die `TextBox` Struktur implementiert beide `IControl` und `ITextBox`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-149">In this example, the `TextBox` structure implements both `IControl` and `ITextBox`.</span></span>

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

<span data-ttu-id="f92f4-150">Deklariert, dass ein Typ eine Schnittstelle, an und für sich implementiert ist nichts im Deklarationsbereich des Typs nicht deklarieren.</span><span class="sxs-lookup"><span data-stu-id="f92f4-150">Declaring that a type implements an interface in and of itself does not declare anything in the declaration space of the type.</span></span> <span data-ttu-id="f92f4-151">Daher ist es zulässig, zwei Schnittstellen mit einer Methode mit dem gleichen Namen zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="f92f4-151">Thus, it is valid to implement two interfaces with a method by the same name.</span></span>

<span data-ttu-id="f92f4-152">Typen können nicht allein einen Typparameter implementieren, obwohl er die Typparameter beinhalten kann, die im Gültigkeitsbereich befinden.</span><span class="sxs-lookup"><span data-stu-id="f92f4-152">Types cannot implement a type parameter on its own, although it may involve the type parameters that are in scope.</span></span>

```vb
Class C1(Of V)
    Implements V  ' Error, can't implement type parameter directly
    Implements IEnumerable(Of V)  ' OK, not directly implementing

    ...
End Class
```

<span data-ttu-id="f92f4-153">Generische Schnittstellen können mit verschiedenen Typargumenten implementiert mehrmals sein.</span><span class="sxs-lookup"><span data-stu-id="f92f4-153">Generic interfaces can be implemented multiple times using different type arguments.</span></span> <span data-ttu-id="f92f4-154">Ein generischer Typ kann nicht jedoch eine generische Schnittstelle, die mithilfe eines Typparameters aus, wenn der übergebenen Typparameter (unabhängig von Einschränkungen) mit einer anderen Implementierung dieser Schnittstelle überschneidet implementieren.</span><span class="sxs-lookup"><span data-stu-id="f92f4-154">However, a generic type cannot implement a generic interface using a type parameter if the supplied type parameter (regardless of type constraints) could overlap with another implementation of that interface.</span></span> <span data-ttu-id="f92f4-155">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f92f4-155">For example:</span></span>

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


## <a name="primitive-types"></a><span data-ttu-id="f92f4-156">Primitive Typen</span><span class="sxs-lookup"><span data-stu-id="f92f4-156">Primitive Types</span></span>

<span data-ttu-id="f92f4-157">Die *primitive Typen* identifiziert werden, durch die Schlüsselwörter, die Aliase sind für die vordefinierte Typen in der `System` Namespace.</span><span class="sxs-lookup"><span data-stu-id="f92f4-157">The *primitive types* are identified through keywords, which are aliases for predefined types in the `System` namespace.</span></span> <span data-ttu-id="f92f4-158">Ein primitiver Typ ist nicht vom Typ vollständig zu unterscheiden sie Aliase: das reservierte Wort `Byte` ist der gleiche wie das Schreiben von `System.Byte`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-158">A primitive type is completely indistinguishable from the type it aliases: writing the reserved word `Byte` is exactly the same as writing `System.Byte`.</span></span> <span data-ttu-id="f92f4-159">Primitive Typen sind, auch bekannt als *systeminterne Typen*.</span><span class="sxs-lookup"><span data-stu-id="f92f4-159">Primitive types are also known as *intrinsic types*.</span></span>

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


<span data-ttu-id="f92f4-160">Da ein primitiver Typ Aliase regulären-Typ, verfügt über alle primitiver Typ Member aus.</span><span class="sxs-lookup"><span data-stu-id="f92f4-160">Because a primitive type aliases a regular type, every primitive type has members.</span></span> <span data-ttu-id="f92f4-161">Z. B. `Integer` wurde im deklarierten Member `System.Int32`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-161">For example, `Integer` has the members declared in `System.Int32`.</span></span> <span data-ttu-id="f92f4-162">Literale können als Instanzen von den entsprechenden Typen behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="f92f4-162">Literals can be treated as instances of their corresponding types.</span></span>

* <span data-ttu-id="f92f4-163">Die primitiven Typen unterscheiden sich von anderen Strukturtypen, darin, dass sie bestimmte zusätzliche Vorgänge zulassen:</span><span class="sxs-lookup"><span data-stu-id="f92f4-163">The primitive types differ from other structure types in that they permit certain additional operations:</span></span>

* <span data-ttu-id="f92f4-164">Primitiven Typen können die Werte durch Schreiben von Literalen erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="f92f4-164">Primitive types permit values to be created by writing literals.</span></span> <span data-ttu-id="f92f4-165">Z. B. `123I` ist ein Literal vom Typ `Integer`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-165">For example, `123I` is a literal of type `Integer`.</span></span>

* <span data-ttu-id="f92f4-166">Es ist möglich, zum Deklarieren von Konstanten der primitiven Typen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-166">It is possible to declare constants of the primitive types.</span></span>

* <span data-ttu-id="f92f4-167">Bei den Operanden eines Ausdrucks Konstanten für alle primitiven Typ handelt, ist es möglich, dass der Compiler den Ausdruck zur Kompilierzeit ausgewertet werden soll.</span><span class="sxs-lookup"><span data-stu-id="f92f4-167">When the operands of an expression are all primitive type constants, it is possible for the compiler to evaluate the expression at compile time.</span></span> <span data-ttu-id="f92f4-168">Ein solcher Ausdruck wird als ein konstanter Ausdruck bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="f92f4-168">Such an expression is known as a constant expression.</span></span>

<span data-ttu-id="f92f4-169">Visual Basic definiert die folgenden primitiven Typen:</span><span class="sxs-lookup"><span data-stu-id="f92f4-169">Visual Basic defines the following primitive types:</span></span>

* <span data-ttu-id="f92f4-170">Die ganzzahligen Werttypen `Byte` (1-Byte-Ganzzahl ohne Vorzeichen), `SByte` (1-Byte-Ganzzahl mit Vorzeichen), `UShort` (2-Byte-Ganzzahl ohne Vorzeichen), `Short` (2-Byte-Ganzzahl mit Vorzeichen), `UInteger` (4-Byte-Ganzzahl ohne Vorzeichen), `Integer` () 4-Byte-Ganzzahl mit Vorzeichen), `ULong` (8-Byte-Ganzzahl ohne Vorzeichen), und `Long` (8-Byte-Ganzzahl mit Vorzeichen).</span><span class="sxs-lookup"><span data-stu-id="f92f4-170">The integral value types `Byte` (1-byte unsigned integer), `SByte` (1-byte signed integer), `UShort` (2-byte unsigned integer), `Short` (2-byte signed integer), `UInteger` (4-byte unsigned integer), `Integer` (4-byte signed integer), `ULong` (8-byte unsigned integer), and `Long` (8-byte signed integer).</span></span> <span data-ttu-id="f92f4-171">Diese Typen zuordnen `System.Byte`, `System.SByte`, `System.UInt16`, `System.Int16`, `System.UInt32`, `System.Int32`, `System.UInt64` und `System.Int64`bzw.</span><span class="sxs-lookup"><span data-stu-id="f92f4-171">These types map to `System.Byte`, `System.SByte`, `System.UInt16`, `System.Int16`, `System.UInt32`, `System.Int32`, `System.UInt64` and `System.Int64`, respectively.</span></span> <span data-ttu-id="f92f4-172">Der Standardwert, der ein ganzzahliger Typ entspricht dem Literal `0`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-172">The default value of an integral type is equivalent to the literal `0`.</span></span>

* <span data-ttu-id="f92f4-173">Die Gleitkommazahl Werttypen `Single` (4-Byte-Gleitkommazahl) und `Double` (8-Byte-Gleitkommazahl).</span><span class="sxs-lookup"><span data-stu-id="f92f4-173">The floating-point value types `Single` (4-byte floating point) and `Double` (8-byte floating point).</span></span> <span data-ttu-id="f92f4-174">Diese Typen zuordnen `System.Single` und `System.Double`bzw.</span><span class="sxs-lookup"><span data-stu-id="f92f4-174">These types map to `System.Single` and `System.Double`, respectively.</span></span> <span data-ttu-id="f92f4-175">Der Standardwert eines Gleitkommatyps ist gleichbedeutend mit dem Literal `0`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-175">The default value of a floating-point type is equivalent to the literal `0`.</span></span>

* <span data-ttu-id="f92f4-176">Die `Decimal` Typ (16-Byte-decimal-Wert), der zuordnet `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-176">The `Decimal` type (16-byte decimal value), which maps to `System.Decimal`.</span></span> <span data-ttu-id="f92f4-177">Der Standardwert von Decimal entspricht dem Literal `0D`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-177">The default value of decimal is equivalent to the literal `0D`.</span></span>

* <span data-ttu-id="f92f4-178">Die `Boolean` Werttyp, der einen Wahrheitswert, der in der Regel das Ergebnis eines relationalen oder logischen Vorgangs darstellt.</span><span class="sxs-lookup"><span data-stu-id="f92f4-178">The `Boolean` value type, which represents a truth value, typically the result of a relational or logical operation.</span></span> <span data-ttu-id="f92f4-179">Das Literal weist den Typ `System.Boolean`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-179">The literal is of type `System.Boolean`.</span></span> <span data-ttu-id="f92f4-180">Der Standardwert der `Boolean` Typ entspricht dem Literal `False`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-180">The default value of the `Boolean` type is equivalent to the literal `False`.</span></span>

* <span data-ttu-id="f92f4-181">Die `Date` Werttyp dar, die darstellt, ein Datum und/oder eine Uhrzeit aus, und ordnet `System.DateTime`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-181">The `Date` value type, which represents a date and/or a time and maps to `System.DateTime`.</span></span> <span data-ttu-id="f92f4-182">Der Standardwert der `Date` Typ entspricht dem Literal `# 01/01/0001 12:00:00AM #`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-182">The default value of the `Date` type is equivalent to the literal `# 01/01/0001 12:00:00AM #`.</span></span>

* <span data-ttu-id="f92f4-183">Die `Char` Werttyp, der ein einzelnes Unicodezeichen darstellt, und ordnet `System.Char`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-183">The `Char` value type, which represents a single Unicode character and maps to `System.Char`.</span></span> <span data-ttu-id="f92f4-184">Der Standardwert der `Char` Typs entspricht der Konstante Ausdruck `ChrW(0)`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-184">The default value of the `Char` type is equivalent to the constant expression `ChrW(0)`.</span></span>

* <span data-ttu-id="f92f4-185">Die `String` verweisen auf Typ, der eine Sequenz von Unicode-Zeichen darstellt, und ordnet `System.String`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-185">The `String` reference type, which represents a sequence of Unicode characters and maps to `System.String`.</span></span> <span data-ttu-id="f92f4-186">Der Standardwert der `String` Typ ist ein null-Wert.</span><span class="sxs-lookup"><span data-stu-id="f92f4-186">The default value of the `String` type is a null value.</span></span>



## <a name="enumerations"></a><span data-ttu-id="f92f4-187">Enumerationen</span><span class="sxs-lookup"><span data-stu-id="f92f4-187">Enumerations</span></span>

<span data-ttu-id="f92f4-188">*Enumerationen* sind Werttypen, die von erben `System.Enum` und stellen Sie eine Gruppe von Werten eines primitiven Ganzzahltypen symbolisch dar.</span><span class="sxs-lookup"><span data-stu-id="f92f4-188">*Enumerations* are value types that inherit from `System.Enum` and symbolically represent a set of values of one of the primitive integral types.</span></span>

```antlr
EnumDeclaration
    : Attributes? TypeModifier* 'Enum' Identifier
      ( 'As' NonArrayTypeName )? StatementTerminator
      EnumMemberDeclaration+
      'End' 'Enum' StatementTerminator
    ;
```

<span data-ttu-id="f92f4-189">Für einen Enumerationstyp `E`, der Standardwert ist der Wert, der durch den Ausdruck erzeugte `CType(0, E)`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-189">For an enumeration type `E`, the default value is the value produced by the expression `CType(0, E)`.</span></span>

<span data-ttu-id="f92f4-190">Der zugrunde liegende Typ einer Enumeration muss ein ganzzahliger Typ sein, der alle in der Enumeration definierten Enumeratorwerte darstellen können.</span><span class="sxs-lookup"><span data-stu-id="f92f4-190">The underlying type of an enumeration must be an integral type that can represent all the enumerator values defined in the enumeration.</span></span> <span data-ttu-id="f92f4-191">Wenn ein zugrunde liegender Typ angegeben ist, muss er `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, mindestens eine ihrer entsprechenden Typen in der `System` Namespace.</span><span class="sxs-lookup"><span data-stu-id="f92f4-191">If an underlying type is specified, it must be `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, or one of their corresponding types in the `System` namespace.</span></span> <span data-ttu-id="f92f4-192">Wenn kein zugrunde liegender Typ explizit angegeben wird, wird der Standardwert ist `Integer`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-192">If no underlying type is explicitly specified, the default is `Integer`.</span></span>

<span data-ttu-id="f92f4-193">Das folgende Beispiel deklariert eine Enumeration mit zugrunde liegender Typ `Long`:</span><span class="sxs-lookup"><span data-stu-id="f92f4-193">The following example declares an enumeration with an underlying type of `Long`:</span></span>

```vb
Enum Color As Long
    Red
    Green
    Blue
End Enum
```

<span data-ttu-id="f92f4-194">Ein Entwickler kann auswählen, einen zugrunde liegenden Typ verwenden `Long`, wie im Beispiel verwenden, um die Verwendung von Werten zu ermöglichen, die im Bereich von `Long`, aber nicht in den Bereich der `Integer`, oder um diese Option für die Zukunft zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="f92f4-194">A developer might choose to use an underlying type of `Long`, as in the example, to enable the use of values that are in the range of `Long`, but not in the range of `Integer`, or to preserve this option for the future.</span></span>


### <a name="enumeration-members"></a><span data-ttu-id="f92f4-195">Enumerationsmember</span><span class="sxs-lookup"><span data-stu-id="f92f4-195">Enumeration Members</span></span>

<span data-ttu-id="f92f4-196">Die Member einer Enumeration sind die Enumerationswerte, die in der Enumeration deklariert und die von der-Klasse geerbten Member `System.Enum`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-196">The members of an enumeration are the enumerated values declared in the enumeration and the members inherited from class `System.Enum`.</span></span>

<span data-ttu-id="f92f4-197">Der Bereich eines Enumerationsmembers ist der Enumeration-Deklaration-Text.</span><span class="sxs-lookup"><span data-stu-id="f92f4-197">The scope of an enumeration member is the enumeration declaration body.</span></span> <span data-ttu-id="f92f4-198">Dies bedeutet, dass außerhalb der Enumerationsdeklaration einer ein Enumerationsmember immer qualifiziert werden muss (es sei denn, die der Typ explizit in einen Namespace ein Namespace importieren importiert wird).</span><span class="sxs-lookup"><span data-stu-id="f92f4-198">This means that outside of an enumeration declaration, an enumeration member must always be qualified (unless the type is specifically imported into a namespace through a namespace import).</span></span>

<span data-ttu-id="f92f4-199">Reihenfolge der Deklaration für die Enumerationsmemberdeklarationen spielt, wenn die Werte der Konstanten Ausdruck ausgelassen werden.</span><span class="sxs-lookup"><span data-stu-id="f92f4-199">Declaration order for enumeration member declarations is significant when constant expression values are omitted.</span></span> <span data-ttu-id="f92f4-200">Enumerationsmember verfügen implizit über `Public` nur zugreifen, die keine Zugriffsmodifizierer für Member Enumerationsdeklarationen zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="f92f4-200">Enumeration members implicitly have `Public` access only; no access modifiers are allowed on enumeration member declarations.</span></span>

```antlr
EnumMemberDeclaration
    : Attributes? Identifier ( Equals ConstantExpression )? StatementTerminator
    ;
```

### <a name="enumeration-values"></a><span data-ttu-id="f92f4-201">Enumerationswerte</span><span class="sxs-lookup"><span data-stu-id="f92f4-201">Enumeration Values</span></span>

<span data-ttu-id="f92f4-202">Konstanten, die als den zugrunde liegenden Enumerationstyp typisiert, und sie können angezeigt werden, wo die Konstanten erforderlich sind, werden die aufgelisteten Werte in einer Memberliste der Enumeration deklariert.</span><span class="sxs-lookup"><span data-stu-id="f92f4-202">The enumerated values in an enumeration member list are declared as constants typed as the underlying enumeration type, and they can appear wherever constants are required.</span></span> <span data-ttu-id="f92f4-203">Die Definition von Enumerationsmembern mit `=` gibt dem zugeordnete Element, das den Wert, der durch den Konstantenausdruck angegeben.</span><span class="sxs-lookup"><span data-stu-id="f92f4-203">An enumeration member definition with `=` gives the associated member the value indicated by the constant expression.</span></span> <span data-ttu-id="f92f4-204">Der Konstante Ausdruck muss zu einem ganzzahligen Typ, der implizit in den zugrunde liegenden Typ ausgewertet werden und muss innerhalb des Bereichs von Werten, die durch den zugrunde liegenden Typ dargestellt werden kann.</span><span class="sxs-lookup"><span data-stu-id="f92f4-204">The constant expression must evaluate to an integral type that is implicitly convertible to the underlying type and must be within the range of values that can be represented by the underlying type.</span></span> <span data-ttu-id="f92f4-205">Im folgende Beispiel wird Fehler, da die Konstanten Werte `1.5`, `2.3`, und `3.3` sind nicht implizit in den zugrunde liegenden ganzzahligen Typ `Long` mit strict-Semantik.</span><span class="sxs-lookup"><span data-stu-id="f92f4-205">The following example is in error because the constant values `1.5`, `2.3`, and `3.3` are not implicitly convertible to the underlying integral type `Long` with strict semantics.</span></span>

```vb
Option Strict On

Enum Color As Long
    Red = 1.5
    Green = 2.3
    Blue = 3.3
End Enum
```

<span data-ttu-id="f92f4-206">Mehrere Enumerationsmember möglicherweise derselben Wert zugeordneten, freigeben, wie unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="f92f4-206">Multiple enumeration members may share the same associated value, as shown below:</span></span>

```vb
Enum Color
    Red
    Green
    Blue
    Max = Blue
End Enum
```

<span data-ttu-id="f92f4-207">Das Beispiel zeigt eine Enumeration, die zwei--Enumerationsmembern `Blue` und `Max` –, dass derselbe Wert zugewiesen haben.</span><span class="sxs-lookup"><span data-stu-id="f92f4-207">The example shows an enumeration that has two enumeration members -- `Blue` and `Max` -- that have the same associated value.</span></span>

<span data-ttu-id="f92f4-208">Wenn die erste Enumerator-Wert-Definition in der Enumeration keinen Initialisierer aufweist, ist der Wert der entsprechenden Konstanten `0`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-208">If the first enumerator value definition in the enumeration has no initializer, the value of the corresponding constant is `0`.</span></span> <span data-ttu-id="f92f4-209">Eine aufzählungsdefinition-Wert ohne einen Initialisierer gibt dem Enumerator den Wert abgerufen, indem Sie den Wert von den vorherigen Enumerationswert von `1`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-209">An enumeration value definition without an initializer gives the enumerator the value obtained by increasing the value of the previous enumeration value by `1`.</span></span> <span data-ttu-id="f92f4-210">Dieser höhere Wert muss innerhalb des Bereichs der Werte sein, die von den zugrunde liegenden Typ dargestellt werden können.</span><span class="sxs-lookup"><span data-stu-id="f92f4-210">This increased value must be within the range of values that can be represented by the underlying type.</span></span>

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

<span data-ttu-id="f92f4-211">Das obige Beispiel gibt die Enumerationswerte und die zugehörigen Werte.</span><span class="sxs-lookup"><span data-stu-id="f92f4-211">The example above prints the enumeration values and their associated values.</span></span> <span data-ttu-id="f92f4-212">Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="f92f4-212">The output is:</span></span>

```
Red = 0
Green = 10
Blue = 11
```

<span data-ttu-id="f92f4-213">Die Gründe für die Werte lauten wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="f92f4-213">The reasons for the values are as follows:</span></span>

* <span data-ttu-id="f92f4-214">Der Enumerationswert `Red` wird automatisch der Wert zugewiesen `0` (da es keinen Initialisierer aufweist und das erste Element der Enumeration-Wert ist).</span><span class="sxs-lookup"><span data-stu-id="f92f4-214">The enumeration value `Red` is automatically assigned the value `0` (since it has no initializer and is the first enumeration value member).</span></span>

* <span data-ttu-id="f92f4-215">Der Enumerationswert `Green` erhält den Wert explizit `10`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-215">The enumeration value `Green` is explicitly given the value `10`.</span></span>

* <span data-ttu-id="f92f4-216">Der Enumerationswert `Blue` wird automatisch der Wert eins größer ist als der Enumerationswert, der textlich vorausgehenden zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-216">The enumeration value `Blue` is automatically assigned the value one greater than the enumeration value that textually precedes it.</span></span>

<span data-ttu-id="f92f4-217">Der Konstante Ausdruck kann nicht direkt oder indirekt verwenden Sie den Wert von ihrem eigenen Wert zugeordnete Enumeration (d. h. eine Zirkularität, in der Konstante Ausdruck ist nicht zulässig).</span><span class="sxs-lookup"><span data-stu-id="f92f4-217">The constant expression may not directly or indirectly use the value of its own associated enumeration value (that is, circularity in the constant expression is not allowed).</span></span> <span data-ttu-id="f92f4-218">Im folgende Beispiel ist ungültig. da die Deklarationen der `A` und `B` sind zirkulär.</span><span class="sxs-lookup"><span data-stu-id="f92f4-218">The following example is invalid because the declarations of `A` and `B` are circular.</span></span>

```vb
Enum Circular
    A = B
    B
End Enum
```

<span data-ttu-id="f92f4-219">`A` hängt von `B` explizit und `B` hängt `A` implizit.</span><span class="sxs-lookup"><span data-stu-id="f92f4-219">`A` depends on `B` explicitly, and `B` depends on `A` implicitly.</span></span>

## <a name="classes"></a><span data-ttu-id="f92f4-220">Klassen</span><span class="sxs-lookup"><span data-stu-id="f92f4-220">Classes</span></span>

<span data-ttu-id="f92f4-221">Ein *Klasse* ist eine Datenstruktur, die Datenmember (Konstanten, Variablen und Ereignisse), Funktionsmember (Methoden, Eigenschaften, Indexer, Operatoren und Konstruktoren) und geschachtelte Typen enthalten können.</span><span class="sxs-lookup"><span data-stu-id="f92f4-221">A *class* is a data structure that may contain data members (constants, variables, and events), function members (methods, properties, indexers, operators, and constructors), and nested types.</span></span> <span data-ttu-id="f92f4-222">Klassen sind Verweistypen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-222">Classes are reference types.</span></span>

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

<span data-ttu-id="f92f4-223">Das folgende Beispiel zeigt eine Klasse, die jede Art von Member enthält:</span><span class="sxs-lookup"><span data-stu-id="f92f4-223">The following example shows a class that contains each kind of member:</span></span>

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

<span data-ttu-id="f92f4-224">Das folgende Beispiel zeigt die Verwendung dieser Member:</span><span class="sxs-lookup"><span data-stu-id="f92f4-224">The following example shows uses of these members:</span></span>

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

<span data-ttu-id="f92f4-225">Es gibt zwei mandantenklassen geltenden schemaanpassungen Modifizierern `MustInherit` und `NotInheritable`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-225">There are two class-specific modifiers, `MustInherit` and `NotInheritable`.</span></span> <span data-ttu-id="f92f4-226">Es ist ungültig. Geben Sie beide.</span><span class="sxs-lookup"><span data-stu-id="f92f4-226">It is invalid to specify them both.</span></span>


### <a name="class-base-specification"></a><span data-ttu-id="f92f4-227">Die Basisspezifikationen Klasse</span><span class="sxs-lookup"><span data-stu-id="f92f4-227">Class Base Specification</span></span>

<span data-ttu-id="f92f4-228">Eine Klassendeklaration kann es sich um eine Spezifikation Basistyp enthalten, die die direkte Basistyp der Klasse definiert.</span><span class="sxs-lookup"><span data-stu-id="f92f4-228">A class declaration may include a base type specification that defines the direct base type of the class.</span></span>

```antlr
ClassBase
    : 'Inherits' NonArrayTypeName StatementTerminator
    ;
```

<span data-ttu-id="f92f4-229">Wenn eine Deklaration der Klasse keinen expliziten Basistyp hat, wird der direkte Basistyp implizit ist `Object`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-229">If a class declaration has no explicit base type, the direct base type is implicitly `Object`.</span></span> <span data-ttu-id="f92f4-230">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f92f4-230">For example:</span></span>

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class
```

<span data-ttu-id="f92f4-231">Basistypen darf nicht Typparameter in ihren eigenen sein, obwohl er die Typparameter beinhalten kann, die im Gültigkeitsbereich befinden.</span><span class="sxs-lookup"><span data-stu-id="f92f4-231">Base types cannot be a type parameter on its own, although it may involve the type parameters that are in scope.</span></span>

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

<span data-ttu-id="f92f4-232">Klassen können nur leiten sich von `Object` und Klassen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-232">Classes may only derive from `Object` and classes.</span></span> <span data-ttu-id="f92f4-233">Ist ungültig für eine Klasse für die Ableitung `System.ValueType`, `System.Enum`, `System.Array`, `System.MulticastDelegate` oder `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-233">It is invalid for a class to derive from `System.ValueType`, `System.Enum`, `System.Array`, `System.MulticastDelegate` or `System.Delegate`.</span></span> <span data-ttu-id="f92f4-234">Eine generische Klasse kann nicht abgeleitet werden, von `System.Attribute` oder aus einer Klasse, die von ihr abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="f92f4-234">A generic class cannot derive from `System.Attribute` or from a class that derives from it.</span></span>

<span data-ttu-id="f92f4-235">Jede Klasse verfügt über genau eine direkte Basisklasse und Zirkularität in Ableitung ist nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="f92f4-235">Every class has exactly one direct base class, and circularity in derivation is prohibited.</span></span> <span data-ttu-id="f92f4-236">Es ist nicht möglich, für die Ableitung einer `NotInheritable` -Klasse und die Zugriffsdomäne von der Basisklasse müssen identisch oder eine Obermenge der Zugriffsdomäne von der Klasse selbst sein.</span><span class="sxs-lookup"><span data-stu-id="f92f4-236">It is not possible to derive from a `NotInheritable` class, and the accessibility domain of the base class must be the same as or a superset of the accessibility domain of the class itself.</span></span>


### <a name="class-members"></a><span data-ttu-id="f92f4-237">Klassenmember</span><span class="sxs-lookup"><span data-stu-id="f92f4-237">Class Members</span></span>

<span data-ttu-id="f92f4-238">Die Member einer Klasse bestehen aus der Elemente, die durch die Klassenmember-Deklarationen eingeführt und die von der direkten Basisklasse geerbten Member.</span><span class="sxs-lookup"><span data-stu-id="f92f4-238">The members of a class consist of the members introduced by its class member declarations and the members inherited from its direct base class.</span></span>

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

<span data-ttu-id="f92f4-239">Eine Klassendeklaration für das Element möglicherweise `Public`, `Protected`, `Friend`, `Protected Friend`, oder `Private` Zugriff.</span><span class="sxs-lookup"><span data-stu-id="f92f4-239">A class member declaration may have `Public`, `Protected`, `Friend`, `Protected Friend`, or `Private` access.</span></span> <span data-ttu-id="f92f4-240">Wenn Sie eine Klassendeklaration für das Element keine Zugriffsmodifizierer enthalten, die Deklaration standardmäßig `Public` Zugriff, sofern dies nicht die Deklaration einer Variablen; in diesem Fall wird standardmäßig `Private` Zugriff.</span><span class="sxs-lookup"><span data-stu-id="f92f4-240">When a class member declaration does not include an access modifier, the declaration defaults to `Public` access, unless it is a variable declaration; in that case it defaults to `Private` access.</span></span>

<span data-ttu-id="f92f4-241">Der Bereich eines Klassenmembers ist der Text einer Klasse in dem die Memberdeklaration erfolgt, sowie der Einschränkungsliste dieser Klasse, (wenn er generisch und verfügt über Einschränkungen).</span><span class="sxs-lookup"><span data-stu-id="f92f4-241">The scope of a class member is the class body in which the member declaration occurs, plus the constraint list of that class (if it is generic and has constraints).</span></span> <span data-ttu-id="f92f4-242">Wenn der Member verfügt `Friend` Zugriff, der Bereich erweitert, in den Klassentext von jeder abgeleiteten Klasse in der gleichen Anwendung oder einer beliebigen Assembly, die erteilt wurden `Friend` Zugriff, und wenn der Member verfügt `Public`, `Protected`, oder `Protected Friend` den Zugriff auf ihren Bereich in den Klassentext einer beliebigen abgeleiteten Klasse in jedem Programm erweitert.</span><span class="sxs-lookup"><span data-stu-id="f92f4-242">If the member has `Friend` access, its scope extends to the class body of any derived class in the same program or any assembly that has been given `Friend` access, and if the member has `Public`, `Protected`, or `Protected Friend` access, its scope extends to the class body of any derived class in any program.</span></span>


## <a name="structures"></a><span data-ttu-id="f92f4-243">Strukturen</span><span class="sxs-lookup"><span data-stu-id="f92f4-243">Structures</span></span>

<span data-ttu-id="f92f4-244">*Strukturen* sind Werttypen, die von erben `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-244">*Structures* are value types that inherit from `System.ValueType`.</span></span> <span data-ttu-id="f92f4-245">Strukturen sind ähnlich wie Klassen, Datenstrukturen dar, die Datenmember und Funktionsmember enthalten können.</span><span class="sxs-lookup"><span data-stu-id="f92f4-245">Structures are similar to classes in that they represent data structures that can contain data members and function members.</span></span> <span data-ttu-id="f92f4-246">Im Gegensatz zu Klassen erfordern Strukturen jedoch keine Heapzuordnung.</span><span class="sxs-lookup"><span data-stu-id="f92f4-246">Unlike classes, however, structures do not require heap allocation.</span></span>

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

<span data-ttu-id="f92f4-247">Bei Klassen ist es möglich, dass zwei Variablen auf dasselbe Objekt verweisen, und so können Vorgänge auf eine Variable auf das Objekt, das die andere Variable verweist auswirken.</span><span class="sxs-lookup"><span data-stu-id="f92f4-247">In the case of classes, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="f92f4-248">Mit Strukturen besitzt jede Variable eine eigene Kopie einer anderen`Shared` Daten, daher es nicht möglich, dass Vorgänge für mindestens eine der anderen zu beeinflussen ist, wie das folgende Beispiel veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="f92f4-248">With structures, the variables each have their own copy of the non-`Shared` data, so it is not possible for operations on one to affect the other, as the following example illustrates:</span></span>

```vb
Structure Point
    Public x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

<span data-ttu-id="f92f4-249">Wenn die obige Deklaration der folgende Code gibt den Wert `10`:</span><span class="sxs-lookup"><span data-stu-id="f92f4-249">Given the above declaration the following code outputs the value `10`:</span></span>

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

<span data-ttu-id="f92f4-250">Die Zuweisung von `a` zu `b` erstellt eine Kopie des Werts und `b` ist daher nicht betroffen, durch die Zuweisung zu `a.x`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-250">The assignment of `a` to `b` creates a copy of the value, and `b` is thus unaffected by the assignment to `a.x`.</span></span> <span data-ttu-id="f92f4-251">Hatte `Point` wurde stattdessen als eine Klasse deklariert, ist die Ausgabe wäre `100` da `a` und `b` würde das gleiche Objekt verweisen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-251">Had `Point` instead been declared as a class, the output would be `100` because `a` and `b` would reference the same object.</span></span>


### <a name="structure-members"></a><span data-ttu-id="f92f4-252">Strukturmember</span><span class="sxs-lookup"><span data-stu-id="f92f4-252">Structure Members</span></span>

<span data-ttu-id="f92f4-253">Die Member einer Struktur sind die Elemente, die von den Deklarationen der Strukturmember eingeführt und die Mitglieder von geerbt `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-253">The members of a structure are the members introduced by its structure member declarations and the members inherited from `System.ValueType`.</span></span>

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

<span data-ttu-id="f92f4-254">Jede Struktur weist implizit einen `Public` parameterlosen Instanzenkonstruktor, die den Standardwert der Struktur erzeugt.</span><span class="sxs-lookup"><span data-stu-id="f92f4-254">Every structure implicitly has a `Public` parameterless instance constructor that produces the default value of the structure.</span></span> <span data-ttu-id="f92f4-255">Daher ist es nicht möglich, für eine Typdeklaration Struktur, um einen parameterlosen Instanzenkonstruktor zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="f92f4-255">As a result, it is not possible for a structure type declaration to declare a parameterless instance constructor.</span></span> <span data-ttu-id="f92f4-256">Ein Strukturtyp ist, darf jedoch deklarieren *parametrisierte* Instanzkonstruktoren, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="f92f4-256">A structure type is, however, permitted to declare *parameterized* instance constructors, as in the following example:</span></span>

```vb
Structure Point
    Private x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

<span data-ttu-id="f92f4-257">Wenn die obige Deklaration, die folgenden Anweisungen erstellen eine `Point` mit `x` und `y` auf 0 (null) initialisiert.</span><span class="sxs-lookup"><span data-stu-id="f92f4-257">Given the above declaration, the following statements both create a `Point` with `x` and `y` initialized to zero.</span></span>

```vb
Dim p1 As Point = New Point()
Dim p2 As Point = New Point(0, 0)
```

<span data-ttu-id="f92f4-258">Da Strukturen direkt ihre Feld Werte (statt Verweise auf diese Werte) enthalten, können nicht in Strukturen Felder enthalten, die direkt oder indirekt auf sich selbst verweisen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-258">Because structures directly contain their field values (rather than references to those values), structures cannot contain fields that directly or indirectly reference themselves.</span></span> <span data-ttu-id="f92f4-259">Der folgende Code ist beispielsweise ungültig:</span><span class="sxs-lookup"><span data-stu-id="f92f4-259">For example, the following code is not valid:</span></span>

```vb
Structure S1
    Dim f1 As S2
End Structure

Structure S2
    ' This would require S1 to contain itself.
    Dim f1 As S1
End Structure
```

<span data-ttu-id="f92f4-260">Deklaration von Strukturmembern möglicherweise in der Regel nur `Public`, `Friend`, oder `Private` Zugriff, aber beim Überschreiben von geerbten Member `Object`, `Protected` und `Protected Friend` Zugriff kann auch verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="f92f4-260">Normally, a structure member declaration may only have `Public`, `Friend`, or `Private` access, but when overriding members inherited from `Object`, `Protected` and `Protected Friend` access may also be used.</span></span> <span data-ttu-id="f92f4-261">Wenn Sie eine Deklaration von Strukturmembern keine Zugriffsmodifizierer enthalten, die Deklaration standardmäßig `Public` Zugriff.</span><span class="sxs-lookup"><span data-stu-id="f92f4-261">When a structure member declaration does not include an access modifier, the declaration defaults to `Public` access.</span></span> <span data-ttu-id="f92f4-262">Der Bereich eines Elements, das von einer Struktur deklariert ist Rumpf der Struktur, in dem sich die Deklaration erfolgt, sowie die Einschränkungen dieser Struktur, (Wenn sie generische wurde und Einschränkungen haben).</span><span class="sxs-lookup"><span data-stu-id="f92f4-262">The scope of a member declared by a structure is the structure body in which the declaration occurs, plus the constraints of that structure (if it was generic and had constraints).</span></span>


## <a name="standard-modules"></a><span data-ttu-id="f92f4-263">Standardmodule</span><span class="sxs-lookup"><span data-stu-id="f92f4-263">Standard Modules</span></span>

<span data-ttu-id="f92f4-264">Ein *Standardmodul* ist ein Typ, dessen Member handelt es sich implizit `Shared` Gültigkeitsbereich zu Deklarationsabschnitt des enthaltenden Namespace des standard-Moduls, statt die standardmäßige Moduldeklaration selbst.</span><span class="sxs-lookup"><span data-stu-id="f92f4-264">A *standard module* is a type whose members are implicitly `Shared` and scoped to the declaration space of the standard module's containing namespace, rather than just to the standard module declaration itself.</span></span> <span data-ttu-id="f92f4-265">Standard-Module können nie instanziiert werden.</span><span class="sxs-lookup"><span data-stu-id="f92f4-265">Standard modules may never be instantiated.</span></span> <span data-ttu-id="f92f4-266">Es ist ein Fehler auf eine Variable eines Typs für die standard-Modul zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="f92f4-266">It is an error to declare a variable of a standard module type.</span></span>

```antlr
ModuleDeclaration
    : Attributes? TypeModifier* 'Module' Identifier StatementTerminator
      ModuleMemberDeclaration*
      'End' 'Module' StatementTerminator
    ;
```

<span data-ttu-id="f92f4-267">Ein Mitglied in einem Standardmodul verfügt über zwei vollqualifizierte Namen, eine ohne den Modulnamen des standard-und eine mit den Namen des standard-Moduls.</span><span class="sxs-lookup"><span data-stu-id="f92f4-267">A member of a standard module has two fully qualified names, one without the standard module name and one with the standard module name.</span></span> <span data-ttu-id="f92f4-268">Mehr als eine standard-Modul in einem Namespace kann es sich um ein Element mit einem bestimmten Namen definieren; nicht gekennzeichnete Verweise auf den Namen außerhalb eines der Module sind mehrdeutig.</span><span class="sxs-lookup"><span data-stu-id="f92f4-268">More than one standard module in a namespace may define a member with a particular name; unqualified references to the name outside of either module are ambiguous.</span></span> <span data-ttu-id="f92f4-269">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f92f4-269">For example:</span></span>

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

<span data-ttu-id="f92f4-270">Ein Modul kann nur in einem Namespace deklariert werden und darf nicht in einen anderen Typ geschachtelt sein.</span><span class="sxs-lookup"><span data-stu-id="f92f4-270">A module may only be declared in a namespace and may not be nested in another type.</span></span> <span data-ttu-id="f92f4-271">Standardmodule dürfen keine Schnittstellen implementieren, die sie implizit abgeleitet `Object`, und sie müssen nur `Shared` Konstruktoren.</span><span class="sxs-lookup"><span data-stu-id="f92f4-271">Standard modules may not implement interfaces, they implicitly derive from `Object`, and they have only `Shared` constructors.</span></span>


### <a name="standard-module-members"></a><span data-ttu-id="f92f4-272">Standard Modulelemente</span><span class="sxs-lookup"><span data-stu-id="f92f4-272">Standard Module Members</span></span>

<span data-ttu-id="f92f4-273">Die Member eines standard-Moduls sind die Elemente, die durch die Memberdeklarationen eingeführt und die Mitglieder von geerbt `Object`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-273">The members of a standard module are the members introduced by its member declarations and the members inherited from `Object`.</span></span> <span data-ttu-id="f92f4-274">Standardmodule möglicherweise jede Art von Member, mit Ausnahme der Instanzkonstruktoren.</span><span class="sxs-lookup"><span data-stu-id="f92f4-274">Standard modules may have any type of member except instance constructors.</span></span> <span data-ttu-id="f92f4-275">Alle Standardmodul Typmember handelt es sich implizit `Shared`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-275">All standard module type members are implicitly `Shared`.</span></span>

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

<span data-ttu-id="f92f4-276">Eine Deklaration möglicherweise in der Regel nur `Public`, `Friend`, oder `Private` Zugriff, aber beim Überschreiben von geerbten Member `Object`, `Protected` und `Protected Friend` Zugriffsmodifizierer können angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="f92f4-276">Normally, a standard module member declaration may only have `Public`, `Friend`, or `Private` access, but when overriding members inherited from `Object`, the `Protected` and `Protected Friend` access modifiers may be specified.</span></span> <span data-ttu-id="f92f4-277">Wenn Sie eine Deklaration keine Zugriffsmodifizierer enthalten, die Deklaration standardmäßig `Public` zugreifen, es sei denn, es sich um eine Variable ist standardmäßig `Private` Zugriff.</span><span class="sxs-lookup"><span data-stu-id="f92f4-277">When a standard module member declaration does not include an access modifier, the declaration defaults to `Public` access, unless it is a variable, which defaults to `Private` access.</span></span>

<span data-ttu-id="f92f4-278">Wie bereits erwähnt ist der Gültigkeitsbereich eines standard-Modul die Deklaration mit der standard Moduldeklaration.</span><span class="sxs-lookup"><span data-stu-id="f92f4-278">As previously noted, the scope of a standard module member is the declaration containing the standard module declaration.</span></span> <span data-ttu-id="f92f4-279">Mitglieder von geerbt `Object` sind nicht in dieser speziellen Bereichsdefinition; enthalten diese Elemente kein Bereich haben und muss immer mit dem Namen des Moduls qualifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="f92f4-279">Members inherited from `Object` are not included in this special scoping; those members have no scope and must always be qualified with the name of the module.</span></span> <span data-ttu-id="f92f4-280">Wenn der Member verfügt `Friend` Zugriff, der Bereich erstreckt sich ausschließlich auf Namespacemember im selben Programm oder in Assemblys, die erteilt wurden deklariert `Friend` Zugriff.</span><span class="sxs-lookup"><span data-stu-id="f92f4-280">If the member has `Friend` access, its scope extends only to namespace members declared in the same program or assemblies that have been given `Friend` access.</span></span>


## <a name="interfaces"></a><span data-ttu-id="f92f4-281">Schnittstellen</span><span class="sxs-lookup"><span data-stu-id="f92f4-281">Interfaces</span></span>

<span data-ttu-id="f92f4-282">*Schnittstellen* sind Referenztypen, die von anderen Typen implementieren, um sicherzustellen, dass sie bestimmte Methoden unterstützen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-282">*Interfaces* are reference types that other types implement to guarantee that they support certain methods.</span></span> <span data-ttu-id="f92f4-283">Eine Schnittstelle wird nie direkt erstellt und verfügt über keine tatsächliche Darstellung – andere Datentypen müssen in einen Schnittstellentyp konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="f92f4-283">An interface is never directly created and has no actual representation -- other types must be converted to an interface type.</span></span> <span data-ttu-id="f92f4-284">Eine Schnittstelle definiert einen Vertrag.</span><span class="sxs-lookup"><span data-stu-id="f92f4-284">An interface defines a contract.</span></span> <span data-ttu-id="f92f4-285">Eine Klasse oder Struktur, die eine Schnittstelle implementiert, muss ihren Vertrag einhalten.</span><span class="sxs-lookup"><span data-stu-id="f92f4-285">A class or structure that implements an interface must adhere to its contract.</span></span>

```antlr
InterfaceDeclaration
    : Attributes? TypeModifier* 'Interface' Identifier
      TypeParameterList? StatementTerminator
      InterfaceBase*
      InterfaceMemberDeclaration*
      'End' 'Interface' StatementTerminator
    ;
```


<span data-ttu-id="f92f4-286">Das folgende Beispiel zeigt eine Schnittstelle, eine Standardeigenschaft enthält `Item`, ein Ereignis `E`, eine Methode `F`, und eine Eigenschaft `P`:</span><span class="sxs-lookup"><span data-stu-id="f92f4-286">The following example shows an interface that contains a default property `Item`, an event `E`, a method `F`, and a property `P`:</span></span>

```vb
Interface IExample
    Default Property Item(index As Integer) As String

    Event E()

    Sub F(value As Integer)

    Property P() As String
End Interface
```

<span data-ttu-id="f92f4-287">Schnittstellen können mehrfache Vererbung nutzen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-287">Interfaces may employ multiple inheritance.</span></span> <span data-ttu-id="f92f4-288">Im folgenden Beispiel ist die Schnittstelle `IComboBox` erbt von `ITextBox` und `IListBox`:</span><span class="sxs-lookup"><span data-stu-id="f92f4-288">In the following example, the interface `IComboBox` inherits from both `ITextBox` and `IListBox`:</span></span>

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

<span data-ttu-id="f92f4-289">Klassen und Strukturen können mehrere Schnittstellen implementieren.</span><span class="sxs-lookup"><span data-stu-id="f92f4-289">Classes and structures can implement multiple interfaces.</span></span> <span data-ttu-id="f92f4-290">Im folgenden Beispiel die Klasse `EditBox` leitet sich von der Klasse `Control` und implementiert beide `IControl` und `IDataBound`:</span><span class="sxs-lookup"><span data-stu-id="f92f4-290">In the following example, the class `EditBox` derives from the class `Control` and implements both `IControl` and `IDataBound`:</span></span>

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


### <a name="interface-inheritance"></a><span data-ttu-id="f92f4-291">Schnittstellenvererbung</span><span class="sxs-lookup"><span data-stu-id="f92f4-291">Interface Inheritance</span></span>

<span data-ttu-id="f92f4-292">Die Basisschnittstellen einer Schnittstelle sind die explizite Basisschnittstelle und deren Basisschnittstellen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-292">The base interfaces of an interface are the explicit base interfaces and their base interfaces.</span></span> <span data-ttu-id="f92f4-293">Der Satz von Schnittstellen ist also die vollständige transitiven Abschluss von die explizite Basisschnittstelle, die explizite Basisschnittstelle und So weiter.</span><span class="sxs-lookup"><span data-stu-id="f92f4-293">In other words, the set of base interfaces is the complete transitive closure of the explicit base interfaces, their explicit base interfaces, and so on.</span></span> <span data-ttu-id="f92f4-294">Wenn eine Schnittstellendeklaration ist keine explizite Schnittstellenmember-Basis, und es ist keine Basisschnittstelle für den Typ Schnittstellen erben nicht von `Object` (obwohl sie über eine erweiterungskonvertierung verfügen `Object`).</span><span class="sxs-lookup"><span data-stu-id="f92f4-294">If an interface declaration has no explicit interface base, then there is no base interface for the type -- interfaces do not inherit from `Object` (although they do have a widening conversion to `Object`).</span></span>

```antlr
InterfaceBase
    : 'Inherits' InterfaceBases StatementTerminator
    ;

InterfaceBases
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

<span data-ttu-id="f92f4-295">Im folgenden Beispiel ist die Basisschnittstelle `IComboBox` sind `IControl`, `ITextBox`, und `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-295">In the following example, the base interfaces of `IComboBox` are `IControl`, `ITextBox`, and `IListBox`.</span></span>

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

<span data-ttu-id="f92f4-296">Eine Schnittstelle erbt alle Member der Basisschnittstellen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-296">An interface inherits all members of its base interfaces.</span></span> <span data-ttu-id="f92f4-297">Das heißt, die `IComboBox` obige Schnittstelle erbt Member `SetText` und `SetItems` sowie `Paint`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-297">In other words, the `IComboBox` interface above inherits members `SetText` and `SetItems` as well as `Paint`.</span></span>

<span data-ttu-id="f92f4-298">Eine Klasse oder Struktur, die eine Schnittstelle, auch implizit implementiert implementiert alle die Basisschnittstellen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-298">A class or structure that implements an interface also implicitly implements all of the interface's base interfaces.</span></span>

<span data-ttu-id="f92f4-299">Wenn eine Schnittstelle mehr als einmal in den transitiven Abschluss der Basisschnittstellen angezeigt wird, trägt es nur einmal Member der abgeleiteten Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="f92f4-299">If an interface appears more than once in the transitive closure of the base interfaces, it only contributes its members to the derived interface once.</span></span> <span data-ttu-id="f92f4-300">Ein Typ implementieren, die nur die abgeleitete Schnittstelle implementieren, die Methoden der muss definiert Basisschnittstelle einmal.</span><span class="sxs-lookup"><span data-stu-id="f92f4-300">A type implementing the derived interface only has to implement the methods of the multiply defined base interface once.</span></span> <span data-ttu-id="f92f4-301">Im folgenden Beispiel `Paint` nur einmal implementiert werden muss, obwohl die Klasse implementiert `IComboBox` und `IControl`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-301">In the following example, `Paint` only needs to be implemented once, even though the class implements `IComboBox` and `IControl`.</span></span>

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

<span data-ttu-id="f92f4-302">Ein `Inherits` Klausel hat keine Auswirkungen auf andere `Inherits` Klauseln.</span><span class="sxs-lookup"><span data-stu-id="f92f4-302">An `Inherits` clause has no effect on other `Inherits` clauses.</span></span> <span data-ttu-id="f92f4-303">Im folgenden Beispiel `IDerived` muss es sich um den Namen des qualifizieren `INested` mit `IBase`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-303">In the following example, `IDerived` must qualify the name of `INested` with `IBase`.</span></span>

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

<span data-ttu-id="f92f4-304">Die Zugriffsdomäne von einer Basisschnittstelle muss identisch oder eine Obermenge der Zugriffsdomäne von der Schnittstelle selbst sein.</span><span class="sxs-lookup"><span data-stu-id="f92f4-304">The accessibility domain of a base interface must be the same as or a superset of the accessibility domain of the interface itself.</span></span>


### <a name="interface-members"></a><span data-ttu-id="f92f4-305">Schnittstellenmember</span><span class="sxs-lookup"><span data-stu-id="f92f4-305">Interface Members</span></span>

<span data-ttu-id="f92f4-306">Die Member einer Schnittstelle bestehen die Elemente, die durch die Memberdeklarationen eingeführt und die von der Basisschnittstellen geerbten Member aus.</span><span class="sxs-lookup"><span data-stu-id="f92f4-306">The members of an interface consist of the members introduced by its member declarations and the members inherited from its base interfaces.</span></span>

```antlr
InterfaceMemberDeclaration
    : NonModuleDeclaration
    | InterfaceEventMemberDeclaration
    | InterfaceMethodMemberDeclaration
    | InterfacePropertyMemberDeclaration
    ;
```

<span data-ttu-id="f92f4-307">Obwohl Schnittstellen, Elemente aus nicht erben `Object`, da jede Klasse oder Struktur, die eine Schnittstelle implementiert von erbt `Object`, die Mitglieder der `Object`, einschließlich Erweiterungsmethoden, gelten als Member einer Schnittstelle und auf einer Schnittstelle aufgerufen werden kann, um direkt ohne eine Umwandlung in `Object`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-307">Although interfaces do not inherit members from `Object`, because every class or structure that implements an interface does inherit from `Object`, the members of `Object`, including extension methods, are considered members of an interface and can be called on an interface directly without requiring a cast to `Object`.</span></span> <span data-ttu-id="f92f4-308">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f92f4-308">For example:</span></span>

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

<span data-ttu-id="f92f4-309">Member einer Schnittstelle mit dem gleichen Namen als Mitglieder der `Object` implizit Schattenkopie `Object` Member.</span><span class="sxs-lookup"><span data-stu-id="f92f4-309">Members of an interface with the same name as members of `Object` implicitly shadow `Object` members.</span></span> <span data-ttu-id="f92f4-310">Nur geschachtelte Typen, Methoden, Eigenschaften und Ereignisse können Member einer Schnittstelle sein.</span><span class="sxs-lookup"><span data-stu-id="f92f4-310">Only nested types, methods, properties, and events may be members of an interface.</span></span> <span data-ttu-id="f92f4-311">Methoden und Eigenschaften können keinen Text enthalten.</span><span class="sxs-lookup"><span data-stu-id="f92f4-311">Methods and properties may not have a body.</span></span> <span data-ttu-id="f92f4-312">Schnittstellenmember sind implizit `Public` und können keinen Zugriffsmodifizierer angeben.</span><span class="sxs-lookup"><span data-stu-id="f92f4-312">Interface members are implicitly `Public` and may not specify an access modifier.</span></span> <span data-ttu-id="f92f4-313">Der Bereich eines Elements in einer Schnittstelle deklariert ist der Schnittstelle-Text, in dem sich die Deklaration erfolgt, sowie der Einschränkungsliste dieser Schnittstelle, (wenn er generisch und verfügt über Einschränkungen).</span><span class="sxs-lookup"><span data-stu-id="f92f4-313">The scope of a member declared in an interface is the interface body in which the declaration occurs, plus the constraint list of that interface (if it is generic and has constraints).</span></span>


## <a name="arrays"></a><span data-ttu-id="f92f4-314">Arrays</span><span class="sxs-lookup"><span data-stu-id="f92f4-314">Arrays</span></span>

<span data-ttu-id="f92f4-315">Ein *Array* ist ein Verweistyp, der Zugriff erfolgt über eine Variable enthält *Indizes* eins mit der Reihenfolge der Variablen im Array entspricht.</span><span class="sxs-lookup"><span data-stu-id="f92f4-315">An *array* is a reference type that contains variables accessed through *indices* corresponding in a one-to-one fashion with the order of the variables in the array.</span></span> <span data-ttu-id="f92f4-316">Wird aufgerufen, die in einem Array enthaltenen Variablen auch der *Elemente* des Arrays muss alle vom selben Typ, und dieser Typ wird aufgerufen, die *Elementtyp* des Arrays.</span><span class="sxs-lookup"><span data-stu-id="f92f4-316">The variables contained in an array, also called the *elements* of the array, must all be of the same type, and this type is called the *element type* of the array.</span></span>

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

<span data-ttu-id="f92f4-317">Die Elemente eines Arrays sind vorhanden, wenn eine Arrayinstanz erstellt wird, und Sie sind nicht mehr vorhanden, wenn die Arrayinstanz zerstört wird.</span><span class="sxs-lookup"><span data-stu-id="f92f4-317">The elements of an array come into existence when an array instance is created, and cease to exist when the array instance is destroyed.</span></span> <span data-ttu-id="f92f4-318">Jedes Element eines Arrays ist auf den Standardwert dieses Typs initialisiert.</span><span class="sxs-lookup"><span data-stu-id="f92f4-318">Each element of an array is initialized to the default value of its type.</span></span> <span data-ttu-id="f92f4-319">Der Typ `System.Array` ist der Basistyp aller Typen von Arrays und kann nicht instanziiert werden.</span><span class="sxs-lookup"><span data-stu-id="f92f4-319">The type `System.Array` is the base type of all array types and may not be instantiated.</span></span> <span data-ttu-id="f92f4-320">Jedes Array-Typ erbt die Member deklariert, indem die `System.Array` geben, und es konvertierbar ist (und `Object`).</span><span class="sxs-lookup"><span data-stu-id="f92f4-320">Every array type inherits the members declared by the `System.Array` type and is convertible to it (and `Object`).</span></span> <span data-ttu-id="f92f4-321">Ein eindimensionales Array mit Elementen `T` außerdem implementiert die Schnittstellen `System.Collections.Generic.IList(Of T)` und `IReadOnlyList(Of T)`; Wenn `T` ist ein Verweistyp, und klicken Sie dann auf das Array vom Typ auch implementiert `IList(Of U)` und `IReadOnlyList(Of U)` für alle `U`, bei dem eine erweiternde Konvertierung von Verweisen auf `T`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-321">A one-dimensional array type with element `T` also implements the interfaces `System.Collections.Generic.IList(Of T)` and `IReadOnlyList(Of T)`; if `T` is a reference type, then the array type also implements `IList(Of U)` and `IReadOnlyList(Of U)` for any `U` that has a widening  reference conversion from `T`.</span></span>

<span data-ttu-id="f92f4-322">Ein Array hat eine *Rang* , bestimmt die Anzahl der Indizes, die jedes Arrayelement zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="f92f4-322">An array has a *rank* that determines the number of indices associated with each array element.</span></span> <span data-ttu-id="f92f4-323">Der Rang eines Arrays bestimmt die Anzahl der *Dimensionen* des Arrays.</span><span class="sxs-lookup"><span data-stu-id="f92f4-323">The rank of an array determines the number of *dimensions* of the array.</span></span> <span data-ttu-id="f92f4-324">Z. B. ein Array mit dem Rang eins ist ein eindimensionales Array bezeichnet, und ein Array mit einem Rang größer als 1 wird ein mehrdimensionales Array bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="f92f4-324">For example, an array with a rank of one is called a single-dimensional array, and an array with a rank greater than one is called a multidimensional array.</span></span>

<span data-ttu-id="f92f4-325">Das folgende Beispiel erstellt ein eindimensionales Array von ganzzahligen Werten, die Elemente des Arrays initialisiert und gibt dann die einzelnen Elemente:</span><span class="sxs-lookup"><span data-stu-id="f92f4-325">The following example creates a single-dimensional array of integer values, initializes the array elements, and then prints each of them out:</span></span>

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

<span data-ttu-id="f92f4-326">Das Programm gibt Folgendes aus:</span><span class="sxs-lookup"><span data-stu-id="f92f4-326">The program outputs the following:</span></span>

```
arr(0) = 0
arr(1) = 1
arr(2) = 4
arr(3) = 9
arr(4) = 16
arr(5) = 25
```

<span data-ttu-id="f92f4-327">Jeder Dimension eines Arrays verfügt über eine zugeordnete Länge.</span><span class="sxs-lookup"><span data-stu-id="f92f4-327">Each dimension of an array has an associated length.</span></span> <span data-ttu-id="f92f4-328">Längen der Dimension sind nicht Teil des Typs des Arrays, aber eingerichtet, wenn eine Instanz des Arraytyps zur Laufzeit erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="f92f4-328">Dimension lengths are not part of the type of the array, but rather are established when an instance of the array type is created at run time.</span></span> <span data-ttu-id="f92f4-329">Die Länge einer Dimension bestimmt des gültigen Bereichs von Indizes für diese Dimension: für eine Dimension der Länge `N`, Indizes reichen von 0 bis `N-1`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-329">The length of a dimension determines the valid range of indices for that dimension: for a dimension of length `N`, indices can range from zero to `N-1`.</span></span> <span data-ttu-id="f92f4-330">Wenn eine Dimension der Länge 0 (null) ist, sind keine gültigen Indizes für diese Dimension.</span><span class="sxs-lookup"><span data-stu-id="f92f4-330">If a dimension is of length zero, there are no valid indices for that dimension.</span></span> <span data-ttu-id="f92f4-331">Die Gesamtanzahl der Elemente in einem Array ist das Produkt der Längen der in jeder Dimension im Array.</span><span class="sxs-lookup"><span data-stu-id="f92f4-331">The total number of elements in an array is the product of the lengths of each dimension in the array.</span></span> <span data-ttu-id="f92f4-332">Wenn eine der Dimensionen eines Arrays eine Länge von 0 (null), hört das Array leer sein.</span><span class="sxs-lookup"><span data-stu-id="f92f4-332">If any of the dimensions of an array has a length of zero, the array is said to be empty.</span></span> <span data-ttu-id="f92f4-333">Der Elementtyp eines Arrays kann einen beliebigen Typ sein.</span><span class="sxs-lookup"><span data-stu-id="f92f4-333">The element type of an array can be any type.</span></span>

<span data-ttu-id="f92f4-334">Arraytypen werden durch Hinzufügen eines vorhandenen Typnamens Modifizierer angegeben.</span><span class="sxs-lookup"><span data-stu-id="f92f4-334">Array types are specified by adding a modifier to an existing type name.</span></span> <span data-ttu-id="f92f4-335">Der Modifizierer besteht aus eine linke Klammer, die einen Satz von 0 (null) oder mehrere Kommas und eine schließende Klammer.</span><span class="sxs-lookup"><span data-stu-id="f92f4-335">The modifier consists of a left parenthesis, a set of zero or more commas, and a right parenthesis.</span></span> <span data-ttu-id="f92f4-336">Der geänderte Typ ist der Elementtyp des Arrays, und die Anzahl der Dimensionen ist die Anzahl von Kommas plus eins.</span><span class="sxs-lookup"><span data-stu-id="f92f4-336">The type modified is the element type of the array, and the number of dimensions is the number of commas plus one.</span></span> <span data-ttu-id="f92f4-337">Wenn mehr als ein Modifizierer angegeben ist, ist der Elementtyp des Arrays ein Array.</span><span class="sxs-lookup"><span data-stu-id="f92f4-337">If more than one modifier is specified, then the element type of the array is an array.</span></span> <span data-ttu-id="f92f4-338">Die Modifizierer werden mit dem am weitesten links stehende Modifizierer, der das äußerste Array von links nach rechts gelesen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-338">The modifiers are read left to right, with the leftmost modifier being the outermost array.</span></span> <span data-ttu-id="f92f4-339">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="f92f4-339">In the example</span></span>

```vb
Module Test
    Dim arr As Integer(,)(,,)()
End Module
```

<span data-ttu-id="f92f4-340">der Elementtyp der `arr` wird ein zweidimensionales Array von dreidimensionalen Arrays eines eindimensionalen Arrays von `Integer`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-340">the element type of `arr` is a two-dimensional array of three-dimensional arrays of one-dimensional arrays of `Integer`.</span></span>

<span data-ttu-id="f92f4-341">Eine Variable kann auch ein Arraytyp sein, einen Arraytypmodifizierer oder ein Array-Größe Initialisierung-Modifizierer in den Namen der Variablen platzieren deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="f92f4-341">A variable may also be declared to be of an array type by putting an array type modifier or an array-size initialization modifier on the variable name.</span></span> <span data-ttu-id="f92f4-342">In diesem Fall den Elementtyp des Arrays ist der Typ, der in der Deklaration angegeben, und die Dimensionen des Arrays werden durch den Variablennamenmodifizierer bestimmt.</span><span class="sxs-lookup"><span data-stu-id="f92f4-342">In that case, the array element type is the type given in the declaration, and the array dimensions are determined by the variable name modifier.</span></span> <span data-ttu-id="f92f4-343">Aus Gründen der Übersichtlichkeit ist es nicht zulässig, einen Array der Modifizierer auf einen Variablennamen und einen Typnamen in der gleichen Deklaration haben.</span><span class="sxs-lookup"><span data-stu-id="f92f4-343">For clarity, it is not valid to have an array type modifier on both a variable name and a type name in the same declaration.</span></span>

<span data-ttu-id="f92f4-344">Das folgende Beispiel zeigt eine Vielzahl von Deklarationen von lokalen Variablen, mit denen Arraytypen mit `Integer` als Typ des Elements:</span><span class="sxs-lookup"><span data-stu-id="f92f4-344">The following example shows a variety of local variable declarations that use array types with `Integer` as the element type:</span></span>

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

<span data-ttu-id="f92f4-345">Ein Array der Namen Modifizierer erstreckt sich auf alle Sätze mit Klammern, die folgen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-345">An array type name modifier extends to all sets of parentheses that follow it.</span></span> <span data-ttu-id="f92f4-346">Dies bedeutet, dass in den Situationen, in dem ein Satz von Argumenten in Klammern eingeschlossenes hinter dem Namen des Typs zulässig ist, es nicht möglich, die Argumente für einen Typnamen des Arrays anzugeben.</span><span class="sxs-lookup"><span data-stu-id="f92f4-346">This means that in the situations where a set of arguments enclosed in parenthesis is allowed after a type name, it is not possible to specify the arguments for an array type name.</span></span> <span data-ttu-id="f92f4-347">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f92f4-347">For example:</span></span>

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

<span data-ttu-id="f92f4-348">Im letzten Fall `(3)` als Teil des Typnamens und einen Satz von Konstruktorargumenten interpretiert wird.</span><span class="sxs-lookup"><span data-stu-id="f92f4-348">In the last case, `(3)` is interpreted as part of the type name rather than as a set of constructor arguments.</span></span>


## <a name="delegates"></a><span data-ttu-id="f92f4-349">Delegaten</span><span class="sxs-lookup"><span data-stu-id="f92f4-349">Delegates</span></span>

<span data-ttu-id="f92f4-350">Ein *Delegieren* ist ein Verweistyp, der auf verweist eine `Shared` -Methode eines Typs oder um eine Instanzmethode eines Objekts.</span><span class="sxs-lookup"><span data-stu-id="f92f4-350">A *delegate* is a reference type that refers to a `Shared` method of a type or to an instance method of an object.</span></span>

```antlr
DelegateDeclaration
    : Attributes? TypeModifier* 'Delegate' MethodSignature StatementTerminator
    ;

MethodSignature
    : SubSignature
    | FunctionSignature
    ;
```

 <span data-ttu-id="f92f4-351">Am nächsten eines Delegaten in anderen Sprachen entspricht ein Funktionszeiger, jedoch nur auf ein Funktionszeiger verweisen kann `Shared` -Funktionen Delegaten können sowohl verweisen `Shared` und Instanzmethoden.</span><span class="sxs-lookup"><span data-stu-id="f92f4-351">The closest equivalent of a delegate in other languages is a function pointer, but whereas a function pointer can only reference `Shared` functions, a delegate can reference both `Shared` and instance methods.</span></span> <span data-ttu-id="f92f4-352">Im letzteren Fall speichert der Delegaten nicht nur einen Verweis auf den Einstiegspunkt der Methode, sondern auch einen Verweis auf die Objektinstanz mit dem zum Aufrufen der Methode.</span><span class="sxs-lookup"><span data-stu-id="f92f4-352">In the latter case, the delegate stores not only a reference to the method's entry point, but also a reference to the object instance with which to invoke the method.</span></span>

<span data-ttu-id="f92f4-353">Die Delegatdeklaration möglicherweise kein `Handles` -Klausel einer `Implements` -Klausel, einer Methode verwendet, oder ein `End` zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-353">The delegate declaration may not have  a `Handles` clause, an `Implements` clause, a method body, or an `End` construct.</span></span> <span data-ttu-id="f92f4-354">Die Liste der Parameter in der Delegatdeklaration überein darf keine `Optional` oder `ParamArray` Parameter.</span><span class="sxs-lookup"><span data-stu-id="f92f4-354">The parameter list of the delegate declaration may not contain `Optional` or `ParamArray` parameters.</span></span> <span data-ttu-id="f92f4-355">Die Zugriffsdomäne von den Rückgabetyp und Parametertypen muss identisch oder eine Obermenge der Zugriffsdomäne des Delegaten selbst sein.</span><span class="sxs-lookup"><span data-stu-id="f92f4-355">The accessibility domain of the return type and parameter types must be the same as or a superset of the accessibility domain of the delegate itself.</span></span>

<span data-ttu-id="f92f4-356">Die Member eines Delegaten sind die Elemente, die von der Klasse geerbt `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-356">The members of a delegate are the members inherited from class `System.Delegate`.</span></span> <span data-ttu-id="f92f4-357">Ein Delegat definiert auch die folgenden Methoden:</span><span class="sxs-lookup"><span data-stu-id="f92f4-357">A delegate also defines the following methods:</span></span>

* <span data-ttu-id="f92f4-358">Ein Konstruktor, zwei Parameter: eine vom Typ akzeptiert `Object` und eine vom Typ `System.IntPtr`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-358">A constructor that takes two parameters, one of type `Object` and one of type `System.IntPtr`.</span></span>

* <span data-ttu-id="f92f4-359">Ein `Invoke` -Methode, die die gleiche Signatur wie der Delegat hat.</span><span class="sxs-lookup"><span data-stu-id="f92f4-359">An `Invoke` method that has the same signature as the delegate.</span></span>

* <span data-ttu-id="f92f4-360">Ein `BeginInvoke` -Methode, deren Signatur die Signatur des Delegaten mit drei Unterschiede ist.</span><span class="sxs-lookup"><span data-stu-id="f92f4-360">A `BeginInvoke` method whose signature is the delegate signature, with three differences.</span></span> <span data-ttu-id="f92f4-361">Zunächst wird der Rückgabetyp geändert, um `System.IAsyncResult`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-361">First, the return type is changed to `System.IAsyncResult`.</span></span> <span data-ttu-id="f92f4-362">Zweitens: zwei zusätzliche Parameter an das Ende der Parameterliste hinzugefügt werden: der erste Typ `System.AsyncCallback` und der zweite Typ `Object`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-362">Second, two additional parameters are added to the end of the parameter list: the first of type `System.AsyncCallback` and the second of type `Object`.</span></span> <span data-ttu-id="f92f4-363">Und schließlich alle `ByRef` Parameter geändert werden, um `ByVal`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-363">And finally, all `ByRef` parameters are changed to be `ByVal`.</span></span>

* <span data-ttu-id="f92f4-364">Ein `EndInvoke` -Methode, deren Rückgabetyp identisch mit den Delegaten ist.</span><span class="sxs-lookup"><span data-stu-id="f92f4-364">An `EndInvoke` method whose return type is the same as the delegate.</span></span> <span data-ttu-id="f92f4-365">Die Parameter der Methode sind nur die Delegatparameter genau, `ByRef` -Parameter, in der gleichen Reihenfolge, die sie in der Signatur des Delegaten auftreten.</span><span class="sxs-lookup"><span data-stu-id="f92f4-365">The parameters of the method are only the delegate parameters exactly that are `ByRef` parameters, in the same order they occur in the delegate signature.</span></span>  <span data-ttu-id="f92f4-366">Zusätzlich zu diesen Parametern, besteht ein weiterer Parameter vom Typ `System.IAsyncResult` am Ende der Parameterliste.</span><span class="sxs-lookup"><span data-stu-id="f92f4-366">In addition to those parameters, there is an additional parameter of type `System.IAsyncResult` at the end of the parameter list.</span></span>

<span data-ttu-id="f92f4-367">Es gibt drei Schritte im definieren und Verwenden von Delegaten: Deklaration, Instanziierung und das aufrufen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-367">There are three steps in defining and using delegates: declaration, instantiation, and invocation.</span></span>

<span data-ttu-id="f92f4-368">Delegaten werden mithilfe der Syntax zum Deklarieren von Delegaten deklariert.</span><span class="sxs-lookup"><span data-stu-id="f92f4-368">Delegates are declared using delegate declaration syntax.</span></span> <span data-ttu-id="f92f4-369">Das folgende Beispiel deklariert einen Delegaten, mit dem Namen `SimpleDelegate` , die keine Argumente akzeptiert:</span><span class="sxs-lookup"><span data-stu-id="f92f4-369">The following example declares a delegate named `SimpleDelegate` that takes no arguments:</span></span>

```vb
Delegate Sub SimpleDelegate()
```

<span data-ttu-id="f92f4-370">Im nächste Beispiel erstellt eine `SimpleDelegate` Instanz, und unmittelbar danach aufgerufen wird:</span><span class="sxs-lookup"><span data-stu-id="f92f4-370">The next example creates a `SimpleDelegate` instance and then immediately calls it:</span></span>

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

<span data-ttu-id="f92f4-371">Es ist nicht viel Sinn Instanziieren von Delegaten für eine Methode, und klicken Sie dann sofort über den Delegaten aufrufen, da es einfacher wäre, die Methode direkt aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-371">There is not much point in instantiating a delegate for a method and then immediately calling via the delegate, as it would be simpler to call the method directly.</span></span> <span data-ttu-id="f92f4-372">Delegaten wird deutlich, wenn ihre Anonymität verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="f92f4-372">Delegates show their usefulness when their anonymity is used.</span></span> <span data-ttu-id="f92f4-373">Das folgende Beispiel zeigt eine `MultiCall` -Methode, die ruft wiederholt eine `SimpleDelegate` Instanz:</span><span class="sxs-lookup"><span data-stu-id="f92f4-373">The next example shows a `MultiCall` method that repeatedly calls a `SimpleDelegate` instance:</span></span>

```vb
Sub MultiCall(d As SimpleDelegate, count As Integer)
    Dim i As Integer

    For i = 0 To count - 1
        d()
    Next i
End Sub
```

<span data-ttu-id="f92f4-374">Es ist unwichtig, die `MultiCall` Methode, welche die Zielmethode für die `SimpleDelegate` ist, welche Zugriff auf diese Methode verfügbar ist und angibt, ob die Methode ist `Shared` oder nicht.</span><span class="sxs-lookup"><span data-stu-id="f92f4-374">It is unimportant to the `MultiCall` method what the target method for the `SimpleDelegate` is, what accessibility this method has, or whether the method is `Shared` or not.</span></span> <span data-ttu-id="f92f4-375">Wichtig ist, dass die Signatur der Zielmethode kompatibel mit `SimpleDelegate`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-375">All that matters is that the signature of the target method is compatible with `SimpleDelegate`.</span></span>


## <a name="partial-types"></a><span data-ttu-id="f92f4-376">Partielle Typen</span><span class="sxs-lookup"><span data-stu-id="f92f4-376">Partial types</span></span>

<span data-ttu-id="f92f4-377">Klasse und Struktur Deklarationen möglich *teilweise* Deklarationen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-377">Class and structure declarations can be *partial* declarations.</span></span> <span data-ttu-id="f92f4-378">Eine partielle Deklaration kann oder den deklarierten Typ innerhalb der Deklaration möglicherweise nicht vollständig beschrieben.</span><span class="sxs-lookup"><span data-stu-id="f92f4-378">A partial declaration may or may not fully describe the declared type within the declaration.</span></span> <span data-ttu-id="f92f4-379">Stattdessen kann die Deklaration des Typs auf mehreren partiellen Deklarationen innerhalb des Programms verteilt werden; Partielle Typen können nicht über Programms hinweg deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="f92f4-379">Instead, the declaration of the type may be spread across multiple partial declarations within the program; partial types cannot be declared across program boundaries.</span></span> <span data-ttu-id="f92f4-380">Eine partielle Deklaration gibt an, die `Partial` Modifizierer in der Deklaration.</span><span class="sxs-lookup"><span data-stu-id="f92f4-380">A partial type declaration specifies the `Partial` modifier on the declaration.</span></span> <span data-ttu-id="f92f4-381">Klicken Sie dann werden allen anderen Deklarationen in der Anwendung für einen Typ mit dem gleichen vollqualifizierten Namen zusammen mit der partiellen Deklaration zur Kompilierzeit auf eine einzelne Typdeklaration bilden zusammengeführt.</span><span class="sxs-lookup"><span data-stu-id="f92f4-381">Then, any other declarations in the program for a type with the same fully-qualified name will be merged together with the partial declaration at compile-time to form a single type declaration.</span></span> <span data-ttu-id="f92f4-382">Der folgende Code deklariert beispielsweise eine einzelne Klasse `Test` mit Mitgliedern `Test.C1` und `Test.C2`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-382">For example, the following code declares a single class `Test` with members `Test.C1` and `Test.C2`.</span></span>

<span data-ttu-id="f92f4-383">a.vb:</span><span class="sxs-lookup"><span data-stu-id="f92f4-383">a.vb:</span></span>

```vb
Public Partial Class Test
    Public Sub S1()
    End Sub
End Class
```

<span data-ttu-id="f92f4-384">b.vb:</span><span class="sxs-lookup"><span data-stu-id="f92f4-384">b.vb:</span></span>

```vb
Public Class Test
    Public Sub S2()
    End Sub
End Class
```

<span data-ttu-id="f92f4-385">Wenn partielle Typdeklarationen kombinieren möchten, müssen mindestens eine der Deklarationen einer `Partial` Modifizierer verwenden, andernfalls ein Fehler während der Kompilierung führt.</span><span class="sxs-lookup"><span data-stu-id="f92f4-385">When combining partial type declarations, at least one of the declarations must have a `Partial` modifier, otherwise a compile-time error results.</span></span>

<span data-ttu-id="f92f4-386">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="f92f4-386">__Note.__</span></span> <span data-ttu-id="f92f4-387">Obwohl es möglich ist, geben Sie `Partial` auf nur eine Deklaration auf viele partielle Deklarationen, es ist besser, ihn auf allen partiellen Deklarationen angeben.</span><span class="sxs-lookup"><span data-stu-id="f92f4-387">Although it is possible to specify `Partial` on only one declaration among many partial declarations, it is better form to specify it on all partial declarations.</span></span> <span data-ttu-id="f92f4-388">In der Situation, in dem eine partielle Deklaration ist sichtbar, aber ein oder mehr partielle Deklarationen sind (z. B. im Fall von Tool generierten Code erweitern) ausgeblendet, werden, es ist akzeptabel, lassen die `Partial` Modifizierer aus der sichtbare Deklaration Geben sie jedoch auf die Ausgeblendete Deklarationen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-388">In the situation where one partial declaration is visible but one or more partial declarations are hidden (such as the case of extending tool-generated code), it is acceptable to leave the `Partial` modifier off of the visible declaration but specify it on the hidden declarations.</span></span>

<span data-ttu-id="f92f4-389">Nur Klassen und Strukturen können mithilfe von partiellen Deklarationen deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="f92f4-389">Only classes and structures can be declared using partial declarations.</span></span> <span data-ttu-id="f92f4-390">Die arität eines Typs gilt beim Zuordnen von partiellen Deklarationen zusammen: zwei Klassen mit demselben Namen, aber unterschiedliche Anzahl von Typparametern werden nicht als partiellen Deklarationen desselben Typs.</span><span class="sxs-lookup"><span data-stu-id="f92f4-390">The arity of a type is considered when matching partial declarations together: two classes with the same name but different numbers of type parameters are not considered to be partial declarations of the same time.</span></span> <span data-ttu-id="f92f4-391">Partielle Deklarationen können Attribute angeben, Modifizierer, Klasse `Inherits` Anweisung oder `Implements` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="f92f4-391">Partial declarations can specify attributes, class modifiers, `Inherits` statement or `Implements` statement.</span></span> <span data-ttu-id="f92f4-392">Zum Zeitpunkt der Kompilierung werden alle Teile der partiellen Deklarationen kombiniert und als Teil der Typdeklaration verwendet.</span><span class="sxs-lookup"><span data-stu-id="f92f4-392">At compile time, all of the pieces of the partial declarations are combined together and used as a part of the type declaration.</span></span> <span data-ttu-id="f92f4-393">Treten Konflikte zwischen den Attributen, Modifizierer, Basisklassen, Schnittstellen oder Typmember, einem Fehler während der Kompilierung führt.</span><span class="sxs-lookup"><span data-stu-id="f92f4-393">If there are any conflicts between attributes, modifiers, bases, interfaces, or type members, a compile-time error results.</span></span> <span data-ttu-id="f92f4-394">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f92f4-394">For example:</span></span>

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

<span data-ttu-id="f92f4-395">Im vorherige Beispiel deklariert einen Typ `Test1` , `Public`, erbt `Object` und implementiert `System.IDisposable` und `System.IComparable`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-395">The previous example declares a type `Test1` that is `Public`, inherits from `Object` and implements `System.IDisposable` and `System.IComparable`.</span></span> <span data-ttu-id="f92f4-396">Die partiellen Deklarationen von `Test2` verursacht einen Fehler während der Kompilierung aus, da einer der Deklarationen, auf welchem `Test2` ist `Public` und einen anderen wird angegeben, dass `Test2` ist `Private`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-396">The partial declarations of `Test2` will cause a compile-time error because one of the declarations says that `Test2` is `Public` and another says that `Test2` is `Private`.</span></span>

<span data-ttu-id="f92f4-397">Partielle Typen mit den beiden Typparametern können deklarieren, Einschränkungen und die Varianz für die Typparameter an, aber die Einschränkungen und der Varianz aus jeder partiellen Deklaration müssen übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-397">Partial types with type parameters can declare constraints and variance for the type parameters, but the constraints and variance from each partial declaration must match.</span></span> <span data-ttu-id="f92f4-398">Somit sind Einschränkungen und Varianz spezielle darin, dass sie nicht automatisch wie andere Modifizierer kombiniert werden:</span><span class="sxs-lookup"><span data-stu-id="f92f4-398">Thus, constraints and variance are special in that they are not automatically combined like other modifiers:</span></span>

```vb
Partial Public Class List(Of T As IEnumerable)
End Class

' Error: Constraints on T don't match
Class List(Of T As IComparable)
End Class
```

<span data-ttu-id="f92f4-399">Die Tatsache, dass ein Typ deklariert wird mit mehreren partiellen Deklarationen wirkt sich nicht auf die Name-Suchregeln innerhalb des Typs aus.</span><span class="sxs-lookup"><span data-stu-id="f92f4-399">The fact that a type is declared using multiple partial declarations does not affect the name lookup rules within the type.</span></span> <span data-ttu-id="f92f4-400">Daher eine Deklaration der partiellen Typs kann in andere partielle Typdeklarationen deklarierten Member oder kann Methoden auf, die in andere partielle Typdeklarationen deklariert Schnittstellen implementieren.</span><span class="sxs-lookup"><span data-stu-id="f92f4-400">As a result, a partial type declaration can use members declared in other partial type declarations, or may implement methods on interfaces declared in other partial type declarations.</span></span> <span data-ttu-id="f92f4-401">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f92f4-401">For example:</span></span>

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

<span data-ttu-id="f92f4-402">Geschachtelte Typen haben auch die partielle Deklarationen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-402">Nested types can have partial declarations as well.</span></span> <span data-ttu-id="f92f4-403">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f92f4-403">For example:</span></span>

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

<span data-ttu-id="f92f4-404">Initialisierer in eine partielle Deklaration werden immer noch in der Reihenfolge der Deklaration ausgeführt werden. Es ist jedoch keine festgelegte Reihenfolge der Ausführung von Initialisierern, die in separaten partiellen Deklarationen auftreten.</span><span class="sxs-lookup"><span data-stu-id="f92f4-404">Initializers within a partial declaration will still be executed in declaration order; however, there is no guaranteed order of execution for initializers that occur in separate partial declarations.</span></span>

## <a name="constructed-types"></a><span data-ttu-id="f92f4-405">Konstruierte Typen</span><span class="sxs-lookup"><span data-stu-id="f92f4-405">Constructed Types</span></span>

<span data-ttu-id="f92f4-406">Die Deklaration eines generischen Typs bezeichnet selbst, einen Typ keinen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-406">A generic type declaration, by itself, does not denote a type.</span></span> <span data-ttu-id="f92f4-407">Deklaration eines generischen Typs kann stattdessen als "Blaupause" verwendet werden, um viele verschiedene Arten zu erstellen, durch Anwenden von Typargumenten.</span><span class="sxs-lookup"><span data-stu-id="f92f4-407">Instead, a generic type declaration can be used as a "blueprint" to form many different types by applying type arguments.</span></span> <span data-ttu-id="f92f4-408">Ein generischer Typ, die Typargumente, die darauf angewendet wird aufgerufen, eine *konstruierter Typ*.</span><span class="sxs-lookup"><span data-stu-id="f92f4-408">A generic type that has type arguments applied to it is called a *constructed type*.</span></span> <span data-ttu-id="f92f4-409">Die Typargumente in einem konstruierten Typ müssen immer erfüllen, die Einschränkungen platziert, die von den Typparametern, die, denen Sie zu entsprechen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-409">The type arguments in a constructed type must always satisfy the constraints placed on the type parameters they match to.</span></span>

<span data-ttu-id="f92f4-410">Ein Typname möglicherweise ein konstruierten Typs identifizieren, obwohl sie direkt Typparameter angeben nicht.</span><span class="sxs-lookup"><span data-stu-id="f92f4-410">A type name might identify a constructed type even though it doesn't specify type parameters directly.</span></span> <span data-ttu-id="f92f4-411">Dies kann auftreten, in denen ein Typ in der Deklaration einer generischen Klasse geschachtelt ist und der Instanztyp der enthaltenden Deklaration wird implizit für die Namenssuche verwendet:</span><span class="sxs-lookup"><span data-stu-id="f92f4-411">This can occur where a type is nested within a generic class declaration, and the instance type of the containing declaration is implicitly used for name lookup:</span></span>

```vb
Class Outer(Of T) 
    Public Class Inner 
    End Class

    ' Type of i is the constructed type Outer(Of T).Inner
    Public i As Inner 
End Class
```

<span data-ttu-id="f92f4-412">Ein konstruierter Typ `C(Of T1,...,Tn)` kann zugegriffen werden, wenn der generische Typ und alle Typargumente zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="f92f4-412">A constructed type `C(Of T1,...,Tn)` is accessible when the generic type and all the type arguments are accessible.</span></span> <span data-ttu-id="f92f4-413">Z. B. wenn der generische Typ `C` ist `Public` und alle Typargumente `T1,...,Tn` sind `Public`, und klicken Sie dann der konstruierte Typ ist `Public`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-413">For instance, if the generic type `C` is `Public` and all of the type arguments `T1,...,Tn` are `Public`, then the constructed type is `Public`.</span></span> <span data-ttu-id="f92f4-414">Wenn der Typname oder eines der Argumente des Typs ist `Private`, allerdings wird der Zugriff des konstruierten Typs `Private`.</span><span class="sxs-lookup"><span data-stu-id="f92f4-414">If either the type name or one of the type arguments is `Private`, however, then the accessibility of the constructed type is `Private`.</span></span> <span data-ttu-id="f92f4-415">Wenn ein Argument des konstruierten Typs ist `Protected` und ein anderes Typargument `Friend`, und klicken Sie dann der konstruierte Typ ist nur in der Klasse und deren Unterklassen in dieser Assembly oder einer beliebigen Assembly, die erteilt wurden `Friend` Zugriff.</span><span class="sxs-lookup"><span data-stu-id="f92f4-415">If one type argument of the constructed type is `Protected` and another type argument is `Friend`, then the constructed type is accessible only in the class and its subclasses in this assembly or any assembly that has been given `Friend` access.</span></span> <span data-ttu-id="f92f4-416">Das heißt, ist die Zugriffsdomäne eines konstruierten Typs die Schnittmenge der Barrierefreiheit Domänen seiner Bestandteile.</span><span class="sxs-lookup"><span data-stu-id="f92f4-416">In other words, the accessibility domain for a constructed type is the intersection of the accessibility domains of its constituent parts.</span></span>

<span data-ttu-id="f92f4-417">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="f92f4-417">__Note.__</span></span> <span data-ttu-id="f92f4-418">Die Tatsache, dass die Zugriffsdomäne des konstruierten Typs die Schnittmenge mit den zugehörigen ausgebildetes Teilen ist hat es sich um den interessanten Nebeneffekt definieren Sie eine neue Ebene für die Barrierefreiheit.</span><span class="sxs-lookup"><span data-stu-id="f92f4-418">The fact that the accessibility domain of constructed type is the intersection of its constituted parts has the interesting side effect of defining a new accessibility level.</span></span> <span data-ttu-id="f92f4-419">Ein konstruierter Typ, der ein Element enthält, die `Protected` und ein Element, das `Friend` kann nur in Kontexten, die auf Sie zugreifen, können zugegriffen werden *sowohl* `Friend` *und* `Protected`Member.</span><span class="sxs-lookup"><span data-stu-id="f92f4-419">A constructed type that contains an element that is `Protected` and an element that is `Friend` can only be accessed in contexts that can access *both* `Friend` *and* `Protected` members.</span></span> <span data-ttu-id="f92f4-420">Allerdings besteht keine Möglichkeit, diese Zugriffsebene in der Sprache, wie der Zugriff auf express `Protected Friend` bedeutet, dass eine Entität in einem Kontext zugegriffen werden kann, die zugreifen können *entweder* `Friend` *oder* `Protected` Member.</span><span class="sxs-lookup"><span data-stu-id="f92f4-420">However, there is no way to express this accessibility level in the language, as the accessibility `Protected Friend` means that an entity can be accessed in a context that can access *either* `Friend` *or* `Protected` members.</span></span>

<span data-ttu-id="f92f4-421">Die Basis, implementierten Schnittstellen und die Member der konstruierten Typen werden durch, und Ersetzen Sie dabei die angegebenen Typargumente für jedes Vorkommen des Typparameters in der generische Typ bestimmt.</span><span class="sxs-lookup"><span data-stu-id="f92f4-421">The base, implemented interfaces and members of constructed types are determined by substituting the supplied type arguments for each occurrence of the type parameter in the generic type.</span></span>

### <a name="open-types-and-closed-types"></a><span data-ttu-id="f92f4-422">Offene und geschlossene Typen</span><span class="sxs-lookup"><span data-stu-id="f92f4-422">Open Types and Closed Types</span></span>

<span data-ttu-id="f92f4-423">Ein konstruierter Typ, für, die eine oder mehrere Typargumente Typparameter eines enthaltenden Typs oder-Methode wird aufgerufen, eine *offener Typ*.</span><span class="sxs-lookup"><span data-stu-id="f92f4-423">A constructed type for who one or more type arguments are type parameters of a containing type or method is called an *open type*.</span></span> <span data-ttu-id="f92f4-424">Grund hierfür ist Teil der Typparameter des Typs immer noch nicht bekannt sind, damit die tatsächliche Form vom Typ noch nicht vollständig bekannt ist.</span><span class="sxs-lookup"><span data-stu-id="f92f4-424">This is because some of the type parameters of the type are still not known, so the actual shape of the type is not yet fully known.</span></span> <span data-ttu-id="f92f4-425">Ein generischer Typ, deren Typargumente alle Nichttyp-Parameter werden, wird aufgerufen, im Gegensatz dazu ein *geschlossener Typ*.</span><span class="sxs-lookup"><span data-stu-id="f92f4-425">In contrast, a generic type whose type arguments are all non-type parameters is called a *closed type*.</span></span> <span data-ttu-id="f92f4-426">Die Form eines geschlossenen Typs wird immer vollständig bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="f92f4-426">The shape of a closed type is always fully known.</span></span> <span data-ttu-id="f92f4-427">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f92f4-427">For example:</span></span>

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

<span data-ttu-id="f92f4-428">Der konstruierte Typ `Base(Of Integer, V)` ist ein offener Typ, da jedoch der Typparameter `T` wurde bereitgestellt, der Typparameter `U` wurde einen anderen Typparameter angegeben.</span><span class="sxs-lookup"><span data-stu-id="f92f4-428">The constructed type `Base(Of Integer, V)` is an open type because although the type parameter `T` has been supplied, the type parameter `U` has been supplied another type parameter.</span></span> <span data-ttu-id="f92f4-429">Daher ist die vollständige Form vom Typ noch nicht bekannt.</span><span class="sxs-lookup"><span data-stu-id="f92f4-429">Thus, the full shape of the type is not yet known.</span></span> <span data-ttu-id="f92f4-430">Der konstruierte Typ `Derived(Of Double)`, ist jedoch ein geschlossener Typ aus, da alle Typparameter in der Vererbungshierarchie angegeben wurden.</span><span class="sxs-lookup"><span data-stu-id="f92f4-430">The constructed type `Derived(Of Double)`, however, is a closed type because all type parameters in the inheritance hierarchy have been supplied.</span></span>

<span data-ttu-id="f92f4-431">Offene Typen werden wie folgt definiert:</span><span class="sxs-lookup"><span data-stu-id="f92f4-431">Open types are defined as follows:</span></span>

* <span data-ttu-id="f92f4-432">Ein Typparameter ist ein offener Typ.</span><span class="sxs-lookup"><span data-stu-id="f92f4-432">A type parameter is an open type.</span></span>

* <span data-ttu-id="f92f4-433">Array-Typ ist ein offener Typ, wenn der Elementtyp ein offener Typ ist.</span><span class="sxs-lookup"><span data-stu-id="f92f4-433">An array type is an open type if its element type is an open type.</span></span>

* <span data-ttu-id="f92f4-434">Ein konstruierter Typ ist ein offener Typ, wenn eine oder mehrere seiner Typargumente sind ein offener Typ.</span><span class="sxs-lookup"><span data-stu-id="f92f4-434">A constructed type is an open type if one or more of its type arguments are an open type.</span></span>

* <span data-ttu-id="f92f4-435">Ein geschlossener Typ ist ein Typ, der nicht auf einen offenen Typ handelt.</span><span class="sxs-lookup"><span data-stu-id="f92f4-435">A closed type is a type that is not an open type.</span></span>

<span data-ttu-id="f92f4-436">Da der Einstiegspunkt des Programms in einem generischen Typ sein kann, werden alle Typen, die zur Laufzeit verwendet die geschlossene Typen.</span><span class="sxs-lookup"><span data-stu-id="f92f4-436">Because the program entry point cannot be in a generic type, all types used at run-time will be closed types.</span></span>

## <a name="special-types"></a><span data-ttu-id="f92f4-437">Spezielle Typen</span><span class="sxs-lookup"><span data-stu-id="f92f4-437">Special Types</span></span>

<span data-ttu-id="f92f4-438">Das .NET Framework enthält eine Reihe von Klassen, die speziell .NET Framework und Visual Basic-Sprache behandelt werden:</span><span class="sxs-lookup"><span data-stu-id="f92f4-438">The .NET Framework contains a number of classes that are treated specially by the .NET Framework and by the Visual Basic language:</span></span>

<span data-ttu-id="f92f4-439">Der Typ `System.Void`, steht für einen void-Typ in .NET Framework kann direkt verwiesen werden nur in `GetType` Ausdrücke.</span><span class="sxs-lookup"><span data-stu-id="f92f4-439">The type `System.Void`, which represents a void type in the .NET Framework, can be directly referenced only in `GetType` expressions.</span></span>

<span data-ttu-id="f92f4-440">Die Typen `System.RuntimeArgumentHandle`, `System.ArgIterator` und `System.TypedReference` alle Zeiger in den Stapel enthalten kann und daher darf nicht auf dem .NET Framework-Heap.</span><span class="sxs-lookup"><span data-stu-id="f92f4-440">The types `System.RuntimeArgumentHandle`, `System.ArgIterator` and `System.TypedReference` all can contain pointers into the stack and so cannot appear on the .NET Framework heap.</span></span> <span data-ttu-id="f92f4-441">Sie können nicht aus diesem Grund verwendet werden, als Arrayelementtypen, Rückgabetypen, Feldtypen, generische Typargumente, nullable-Typen `ByRef` Parametertypen und den Typ des zu konvertierenden Wert `Object` oder `System.ValueType`, das Ziel eines Aufrufs an die Instanz Mitglieder der `Object` oder `System.ValueType`, oder in einen Closure aufgehoben.</span><span class="sxs-lookup"><span data-stu-id="f92f4-441">Therefore, they cannot be used as array element types, return types, field types, generic type arguments, nullable types, `ByRef` parameter types, the type of a value being converted to `Object` or `System.ValueType`, the target of a call to instance members of `Object` or `System.ValueType`, or lifted into a closure.</span></span>
