# <a name="type-members"></a><span data-ttu-id="6e6ae-101">Typmember</span><span class="sxs-lookup"><span data-stu-id="6e6ae-101">Type Members</span></span>

<span data-ttu-id="6e6ae-102">Typmember Speicherorte und ausführbaren Code zu definieren.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-102">Type members define storage locations and executable code.</span></span> <span data-ttu-id="6e6ae-103">Sie können Methoden, Konstruktoren, Ereignisse, Konstanten, Variablen und Eigenschaften sein.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-103">They can be methods, constructors, events, constants, variables, and properties.</span></span>

## <a name="interface-method-implementation"></a><span data-ttu-id="6e6ae-104">Schnittstellenimplementierung-Methode</span><span class="sxs-lookup"><span data-stu-id="6e6ae-104">Interface Method Implementation</span></span>

<span data-ttu-id="6e6ae-105">Methoden, Ereignisse und Eigenschaften können Schnittstellenmember implementieren.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-105">Methods, events, and properties can implement interface members.</span></span> <span data-ttu-id="6e6ae-106">Um ein Schnittstellenmember zu implementieren, eine Memberdeklaration gibt an, die `Implements` Schlüsselwort und eine oder mehrere Schnittstellen-Member aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-106">To implement an interface member, a member declaration specifies the `Implements` keyword and lists one or more interface members.</span></span>

```antlr
ImplementsClause
    : ( 'Implements' ImplementsList )?
    ;

ImplementsList
    : InterfaceMemberSpecifier ( Comma InterfaceMemberSpecifier )*
    ;

InterfaceMemberSpecifier
    : NonArrayTypeName Period IdentifierOrKeyword
    ;
```

<span data-ttu-id="6e6ae-107">Methoden und Eigenschaften, die Schnittstellenmember zu implementieren sind implizit `NotOverridable` , sofern deklariert, um `MustOverride`, `Overridable`, oder einen anderen Member überschreiben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-107">Methods and properties that implement interface members are implicitly `NotOverridable` unless declared to be `MustOverride`, `Overridable`, or overriding another member.</span></span> <span data-ttu-id="6e6ae-108">Es ist ein Fehler für ein Element, das Implementieren eines Schnittstellenmembers werden `Shared`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-108">It is an error for a member implementing an interface member to be `Shared`.</span></span> <span data-ttu-id="6e6ae-109">Einen Member zugegriffen hat keine Auswirkungen auf ihre Möglichkeit, die Schnittstellenmember implementieren.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-109">A member's accessibility has no effect on its ability to implement interface members.</span></span>

<span data-ttu-id="6e6ae-110">Eine Implementierung der Schnittstelle gültig ist muss die Implementierungsliste den enthaltenden Typ eine Schnittstelle benennen, die einen kompatiblen Member enthält.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-110">For an interface implementation to be valid, the implements list of the containing type must name an interface that contains a compatible member.</span></span> <span data-ttu-id="6e6ae-111">Ein kompatibles Element ist eine, deren Signatur der Signatur des implementierenden Members entspricht.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-111">A compatible member is one whose signature matches the signature of the implementing member.</span></span> <span data-ttu-id="6e6ae-112">Wenn eine generische Schnittstelle implementiert wird, wird das Typargument in der Implements-Klausel beim Überprüfen der Hardwarekompatibilität in die Signatur ersetzt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-112">If a generic interface is being implemented, then the type argument supplied in the Implements clause is substituted into the signature when checking compatibility.</span></span> <span data-ttu-id="6e6ae-113">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-113">For example:</span></span>

```vb
Interface I1(Of T)
    Sub F(x As T)
End Interface

Class C1
    Implements I1(Of Integer)

    Sub F(x As Integer) Implements I1(Of Integer).F
    End Sub
End Class

Class C2(Of U)
    Implements I1(Of U)

    Sub F(x As U) Implements I1(Of U).F
    End Sub
End Class
```

<span data-ttu-id="6e6ae-114">Wenn ein Ereignis mit deklariert ein Delegattyp ein Schnittstellenereignisses implementiert wird, und dann eine kompatible-Ereignis ist eines, dessen zugrunde liegenden Delegattyp vom gleichen Typ ist.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-114">If an event declared using a delegate type is implementing an interface event, then a compatible event is one whose underlying delegate type is the same type.</span></span> <span data-ttu-id="6e6ae-115">Andernfalls verwendet das Ereignis den Typ des Delegaten aus der Schnittstellenereignis, das implementiert wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-115">Otherwise, the event uses the delegate type from the interface event it is implementing.</span></span> <span data-ttu-id="6e6ae-116">Wenn einem solchen mehrere Schnittstellenereignisse implementiert, müssen alle Schnittstellenereignisse den gleichen zugrunde liegenden Delegattyp.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-116">If such an event implements multiple interface events, all the interface events must have the same underlying delegate type.</span></span> <span data-ttu-id="6e6ae-117">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-117">For example:</span></span>

```vb
Interface ClickEvents
    Event LeftClick(x As Integer, y As Integer)
    Event RightClick(x As Integer, y As Integer)
End Interface

Class Button
    Implements ClickEvents

    ' OK. Signatures match, delegate type = ClickEvents.LeftClickHandler.
    Event LeftClick(x As Integer, y As Integer) _
        Implements ClickEvents.LeftClick

    ' OK. Signatures match, delegate type = ClickEvents.RightClickHandler.
    Event RightClick(x As Integer, y As Integer) _
        Implements ClickEvents.RightClick
End Class

Class Label
    Implements ClickEvents

    ' Error. Signatures match, but can't be both delegate types.
    Event Click(x As Integer, y As Integer) _
        Implements ClickEvents.LeftClick, ClickEvents.RightClick
End Class
```

<span data-ttu-id="6e6ae-118">Ein Schnittstellenmember in der Implementierungsliste wird, verwenden einen Namen, einem Punkt und ein Bezeichner angegeben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-118">An interface member in the implements list is specified using a type name, a period, and an identifier.</span></span> <span data-ttu-id="6e6ae-119">Der Typname muss eine Schnittstelle in der Implementierungsliste oder eine Basisschnittstelle einer Schnittstelle in der Implementierungsliste werden, und der Bezeichner muss ein Mitglied der angegebenen Schnittstelle sein.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-119">The type name must be an interface in the implements list or a base interface of an interface in the implements list, and the identifier must be a member of the specified interface.</span></span> <span data-ttu-id="6e6ae-120">Ein einzelnes Element kann mehr als eine übereinstimmende Schnittstellenmember zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-120">A single member can implement more than one matching interface member.</span></span>

```vb
Interface ILeft
    Sub F()
End Interface

Interface IRight
    Sub F()
End Interface

Class Test
    Implements ILeft, IRight

    Sub F() Implements ILeft.F, IRight.F
    End Sub
End Class
```

<span data-ttu-id="6e6ae-121">Wenn der Schnittstellenmember implementiert wird aufgrund nicht verfügbar in allen explizit implementierte Schnittstellen mehrfachvererbung-Schnittstelle ist, muss der implementierende Member explizit eine Basisschnittstelle verweisen, auf der das Element verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-121">If the interface member being implemented is unavailable in all explicitly implemented interfaces because of multiple interface inheritance, the implementing member must explicitly reference a base interface on which the member is available.</span></span> <span data-ttu-id="6e6ae-122">Z. B. wenn `I1` und `I2` -Member enthalten `M`, und `I3` erbt `I1` und `I2`, ein Typ implementieren `I3` implementieren `I1.M` und `I2.M`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-122">For example, if `I1` and `I2` contain a member `M`, and `I3` inherits from `I1` and `I2`, a type implementing `I3` will implement `I1.M` and `I2.M`.</span></span> <span data-ttu-id="6e6ae-123">Wenn eine Schnittstelle vererbte Member überschattet mehrfach, müssen einen Implementierungstyp die geerbten Member und die Mitglieder, die sie shadowing implementieren.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-123">If an interface shadows multiply inherited members, an implementing type will have to implement the inherited members and the member(s) shadowing them.</span></span>

```vb
Interface ILeft
    Sub F()
End Interface

Interface IRight
    Sub F()
End Interface

Interface ILeftRight
    Inherits ILeft, IRight

    Shadows Sub F()
End Interface

Class Test
    Implements ILeftRight

    Sub LeftF() Implements ILeft.F
    End Sub

    Sub RightF() Implements IRight.F
    End Sub

    Sub LeftRightF() Implements ILeftRight.F
    End Sub
End Class
```

<span data-ttu-id="6e6ae-124">Wenn die enthaltende Schnittstelle des Schnittstellenmembers implementiert werden generisch ist, wird vom selben Typ Argumente, wie die Schnittstelle implementiert wird, angegeben werden muss.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-124">If the containing interface of the interface member be implemented is generic, the same type arguments as the interface being implements must be supplied.</span></span> <span data-ttu-id="6e6ae-125">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-125">For example:</span></span>

```vb
Interface I1(Of T)
    Function F() As T
End Interface

Class C1
    Implements I1(Of Integer)
    Implements I1(Of Double)

    Function F1() As Integer Implements I1(Of Integer).F
    End Function

    Function F2() As Double Implements I1(Of Double).F
    End Function

    ' Error: I1(Of String) is not implemented by C1
    Function F3() As String Implements I1(Of String).F
    End Function
End Class

Class C2(Of U)
    Implements I1(Of U)

    Function F() As U Implements I1(Of U).F
    End Function
End Class
```


## <a name="methods"></a><span data-ttu-id="6e6ae-126">Methoden</span><span class="sxs-lookup"><span data-stu-id="6e6ae-126">Methods</span></span>

<span data-ttu-id="6e6ae-127">Methoden enthalten, die ausführbaren Anweisungen eines Programms.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-127">Methods contain the executable statements of a program.</span></span>

```antlr
MethodMemberDeclaration
    : MethodDeclaration
    | ExternalMethodDeclaration
    ;

InterfaceMethodMemberDeclaration
    : InterfaceMethodDeclaration
    ;

MethodDeclaration
    : SubDeclaration
    | MustOverrideSubDeclaration
    | FunctionDeclaration
    | MustOverrideFunctionDeclaration
    ;

InterfaceMethodDeclaration
    : InterfaceSubDeclaration
    | InterfaceFunctionDeclaration
    ;

SubSignature
    : 'Sub' Identifier TypeParameterList?
      ( OpenParenthesis ParameterList? CloseParenthesis )?
    ;

FunctionSignature
    : 'Function' Identifier TypeParameterList?
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )?
    ;

SubDeclaration
    : Attributes? ProcedureModifier* SubSignature
      HandlesOrImplements? LineTerminator
      Block
      'End' 'Sub' StatementTerminator
    ;

MustOverrideSubDeclaration
    : Attributes? MustOverrideProcedureModifier+ SubSignature
      HandlesOrImplements? StatementTerminator
    ;

InterfaceSubDeclaration
    : Attributes? InterfaceProcedureModifier* SubSignature StatementTerminator
    ;

FunctionDeclaration
    : Attributes? ProcedureModifier* FunctionSignature
      HandlesOrImplements? LineTerminator
      Block
      'End' 'Function' StatementTerminator
    ;

MustOverrideFunctionDeclaration
    : Attributes? MustOverrideProcedureModifier+ FunctionSignature
      HandlesOrImplements? StatementTerminator
    ;

InterfaceFunctionDeclaration
    : Attributes? InterfaceProcedureModifier* FunctionSignature StatementTerminator
    ;

ProcedureModifier
    : AccessModifier | 'Shadows' | 'Shared' | 'Overridable' | 'NotOverridable' | 'Overrides'
    | 'Overloads' | 'Partial' | 'Iterator' | 'Async'
    ;

MustOverrideProcedureModifier
    : ProcedureModifier
    | 'MustOverride'
    ;

InterfaceProcedureModifier
    : 'Shadows' | 'Overloads'
    ;

HandlesOrImplements
    : HandlesClause
    | ImplementsClause
    ;
```

<span data-ttu-id="6e6ae-128">Methoden, die eine optionale Liste von Parametern und einer optionalen Rückgabewert verfügen, freigegebene oder nicht freigegeben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-128">Methods, which have an optional list of parameters and an optional return value, are either shared or not shared.</span></span> <span data-ttu-id="6e6ae-129">Freigegebene Methoden über die Klasse oder Instanzen der Klasse zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-129">Shared methods are accessed through the class or instances of the class.</span></span> <span data-ttu-id="6e6ae-130">Nicht freigegebene Methoden Instanzenmethoden, so genannte durch Instanzen der Klasse zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-130">Non-shared methods, also called instance methods, are accessed through instances of the class.</span></span> <span data-ttu-id="6e6ae-131">Das folgende Beispiel zeigt eine Klasse `Stack` , die mehrere freigegebene Methoden aufweist (`Clone` und `Flip`), mehrere Methoden und Instanzenmethoden (`Push`, `Pop`, und `ToString`):</span><span class="sxs-lookup"><span data-stu-id="6e6ae-131">The following example shows a class `Stack` that has several shared methods (`Clone` and `Flip`), and several instance methods (`Push`, `Pop`, and `ToString`):</span></span>

```vb
Public Class Stack
    Public Shared Function Clone(s As Stack) As Stack
        ...
    End Function

    Public Shared Function Flip(s As Stack) As Stack
        ...
    End Function

    Public Function Pop() As Object
        ...
    End Function

    Public Sub Push(o As Object)
        ...
    End Sub 

    Public Overrides Function ToString() As String
        ...
    End Function 
End Class 

Module Test
    Sub Main()
        Dim s As Stack = New Stack()
        Dim i As Integer

        While i < 10
            s.Push(i)
        End While

        Dim flipped As Stack = Stack.Flip(s)
        Dim cloned As Stack = Stack.Clone(s)

        Console.WriteLine("Original stack: " & s.ToString())
        Console.WriteLine("Flipped stack: " & flipped.ToString())
        Console.WriteLine("Cloned stack: " & cloned.ToString())
    End Sub
End Module
```

<span data-ttu-id="6e6ae-132">Methoden können überladen werden, was bedeutet, dass mehrere Methoden den gleichen Namen haben können, solange sie eindeutige Signaturen aufweisen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-132">Methods can be overloaded, which means that multiple methods may have the same name so long as they have unique signatures.</span></span> <span data-ttu-id="6e6ae-133">Die Signatur einer Methode besteht aus der die Anzahl und Typen ihrer Parameter.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-133">The signature of a method consists of the number and types of its parameters.</span></span> <span data-ttu-id="6e6ae-134">Die Signatur einer Methode umfasst nicht ausdrücklich den Rückgabetyp oder Parametermodifizierern, z. B. Optional "," ByRef "oder" ParamArray.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-134">The signature of a method specifically does not include the return type or parameter modifiers such as Optional, ByRef or ParamArray.</span></span> <span data-ttu-id="6e6ae-135">Das folgende Beispiel zeigt eine Klasse mit einer Reihe von Überladungen:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-135">The following example shows a class with a number of overloads:</span></span>

```vb
Module Test
    Sub F()
        Console.WriteLine("F()")
    End Sub 

    Sub F(o As Object)
        Console.WriteLine("F(Object)")
    End Sub

    Sub F(value As Integer)
        Console.WriteLine("F(Integer)")
    End Sub 

    Sub F(a As Integer, b As Integer)
        Console.WriteLine("F(Integer, Integer)")
    End Sub 

    Sub F(values() As Integer)
        Console.WriteLine("F(Integer())")
    End Sub 

    Sub G(s As String, Optional s2 As String = 5)
        Console.WriteLine("G(String, Optional String")
    End Sub

    Sub G(s As String)
        Console.WriteLine("G(String)")
    End Sub


    Sub Main()
        F()
        F(1)
        F(CType(1, Object))
        F(1, 2)
        F(New Integer() { 1, 2, 3 })
        G("hello")
        G("hello", "world")
    End Sub
End Module
```

<span data-ttu-id="6e6ae-136">Die Ausgabe des Programms lautet:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-136">The output of the program is:</span></span>

```vb
F()
F(Integer)
F(Object)
F(Integer, Integer)
F(Integer())
G(String)
G(String, Optional String)
```

<span data-ttu-id="6e6ae-137">Überladungen, die nur durch optionale Parameter unterscheiden, können für die "versionsverwaltung" von Bibliotheken verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-137">Overloads that differ only in optional parameters can be used for "versioning" of libraries.</span></span> <span data-ttu-id="6e6ae-138">Version 1 einer Bibliothek könnte z. B. eine Funktion mit optionalen Parametern enthalten:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-138">For instance, v1 of a library might include a function with optional parameters:</span></span>

```vb
Sub fopen(fileName As String, Optional accessMode as Integer = 0)
```

<span data-ttu-id="6e6ae-139">Klicken Sie dann das Hinzufügen von weiteren optionalen Parameters "Password" v2 der Bibliothek möchte, und zu tun, ohne wichtige Source-Kompatibilität (also Anwendungen, die zum Auswählen des v1 verwendet neu kompiliert werden können) und ohne binäre Kompatibilität (also Anwendungen, die auf verwendet, möchte Verweis v1 können nun mit v2 ohne erneute Kompilierung zu verweisen).</span><span class="sxs-lookup"><span data-stu-id="6e6ae-139">Then v2 of the library wants to add another optional parameter "password", and it wants to do so without breaking source compatibility (so applications that used to target v1 can be recompiled), and without breaking binary compatibility (so applications that used to reference v1 can now reference v2 without recompilation).</span></span> <span data-ttu-id="6e6ae-140">Dies ist eine v2 wird:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-140">This is how v2 will look:</span></span>

```vb
Sub fopen(file As String, mode as Integer)
Sub fopen(file As String, Optional mode as Integer = 0, Optional pword As String = "")
```

<span data-ttu-id="6e6ae-141">Beachten Sie, dass optionale Parameter in eine öffentliche API nicht CLS-kompatibel sind.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-141">Note that optional parameters in a public API are not CLS-compliant.</span></span> <span data-ttu-id="6e6ae-142">Aber sie können von genutzt werden mindestens Visual Basic- und C# 4 und F#.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-142">However, they can be consumed at least by Visual Basic and C#4 and F#.</span></span>



### <a name="regular-async-and-iterator-method-declarations"></a><span data-ttu-id="6e6ae-143">Reguläre, Async- und Iterator-Methodendeklarationen</span><span class="sxs-lookup"><span data-stu-id="6e6ae-143">Regular, Async and Iterator Method Declarations</span></span>

<span data-ttu-id="6e6ae-144">Es gibt zwei Arten von Methoden: *Unterroutinen*, die keine Werte, zurückgeben und *Funktionen*, führen Sie die.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-144">There are two types of methods: *subroutines*, which do not return values, and *functions*, which do.</span></span> <span data-ttu-id="6e6ae-145">Der Text und `End` Konstrukt einer Methode kann nur ausgelassen werden, wenn die Methode in einer Schnittstelle definiert wird, oder über die `MustOverride` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-145">The body and `End` construct of a method may only be omitted if the method is defined in an interface or has the `MustOverride` modifier.</span></span> <span data-ttu-id="6e6ae-146">Wenn kein Rückgabetyp, auf eine Funktion angegeben wird und strenge Semantik verwendet wird, tritt ein Fehler während der Kompilierung; Andernfalls ist der Typ implizit `Object` oder den Typ der Methode Typzeichen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-146">If no return type is specified on a function and strict semantics are being used, a compile-time error occurs; otherwise the type is implicitly `Object` or the type of the method's type character.</span></span> <span data-ttu-id="6e6ae-147">Die Zugriffsdomäne von den Rückgabetyp und Parametertypen einer Methode muss es sich um die identisch oder eine Obermenge der Zugriffsdomäne von der Methode selbst sein.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-147">The accessibility domain of the return type and parameter types of a method must be the same as or a superset of the accessibility domain of the method itself.</span></span>

<span data-ttu-id="6e6ae-148">Ein __normale Methode__ ist ein URI mit keiner `Async` noch `Iterator` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-148">A __regular method__ is one with neither `Async` nor `Iterator` modifiers.</span></span> <span data-ttu-id="6e6ae-149">Es kann eine Unterroutine oder Funktion sein.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-149">It may be a subroutine or a function.</span></span> <span data-ttu-id="6e6ae-150">Abschnitt [reguläre Methoden](statements.md#regular-methods) erläutert, was geschieht, wenn eine normale Methode aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-150">Section [Regular Methods](statements.md#regular-methods) details what happens when a regular method is invoked.</span></span>

<span data-ttu-id="6e6ae-151">Ein __Iteratormethode__ ist ein URI mit der `Iterator` -Modifizierer und keine `Async` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-151">An __iterator method__ is one with the `Iterator` modifier and no `Async` modifier.</span></span> <span data-ttu-id="6e6ae-152">Es muss eine Funktion, und der Rückgabetyp muss `IEnumerator`, `IEnumerable`, oder `IEnumerator(Of T)` oder `IEnumerable(Of T)` für einige `T`, und es muss keine `ByRef` Parameter.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-152">It must be a function, and its return type must be `IEnumerator`, `IEnumerable`, or `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`, and it must have no `ByRef` parameters.</span></span> <span data-ttu-id="6e6ae-153">Abschnitt [Iteratormethoden](statements.md#iterator-methods) erläutert, was geschieht, wenn eine Iteratormethode aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-153">Section [Iterator Methods](statements.md#iterator-methods) details what happens when an iterator method is invoked.</span></span>

<span data-ttu-id="6e6ae-154">Ein __Async-Methode__ ist ein URI mit der `Async` -Modifizierer und keine `Iterator` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-154">An __async method__ is one with the `Async` modifier and no `Iterator` modifier.</span></span> <span data-ttu-id="6e6ae-155">Es muss entweder eine Unterroutine oder eine Funktion mit Rückgabetyp `Task` oder `Task(Of T)` für einige `T`, und Sie müssen keine `ByRef` Parameter.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-155">It must be either a subroutine, or a function with return type `Task` or `Task(Of T)` for some `T`, and must have no `ByRef` parameters.</span></span> <span data-ttu-id="6e6ae-156">Abschnitt [Async-Methoden](statements.md#async-methods) erläutert, was geschieht, wenn eine asynchrone Methode aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-156">Section [Async Methods](statements.md#async-methods) details what happens when an async method is invoked.</span></span>

<span data-ttu-id="6e6ae-157">Es ist ein Fehler während der Kompilierung, wenn eine Methode nicht mit einer der folgenden drei Arten von Methode ist.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-157">It is a compile-time error if a method is not one of these three kinds of method.</span></span>

<span data-ttu-id="6e6ae-158">Unterroutine und Funktionsdeklarationen sind spezielle, da jede ihren Anfang und Ende-Anweisungen am Anfang einer logischen Zeile beginnen müssen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-158">Subroutine and function declarations are special in that their beginning and end statements must each start at the beginning of a logical line.</span></span> <span data-ttu-id="6e6ae-159">Darüber hinaus wird der Text von einer nicht -`MustOverride` Unterroutine oder Funktion-Deklaration muss am Anfang einer logischen Zeile beginnen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-159">Additionally, the body of a non-`MustOverride` subroutine or function declaration must start at the beginning of a logical line.</span></span> <span data-ttu-id="6e6ae-160">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-160">For example:</span></span>

```vb
Module Test
    ' Illegal: Subroutine doesn't start the line
    Public x As Integer : Sub F() : End Sub

    ' Illegal: First statement doesn't start the line
    Sub G() : Console.WriteLine("G")
    End Sub

    ' Illegal: End Sub doesn't start the line
    Sub H() : End Sub
End Module
```


### <a name="external-method-declarations"></a><span data-ttu-id="6e6ae-161">Externe Methodendeklarationen</span><span class="sxs-lookup"><span data-stu-id="6e6ae-161">External Method Declarations</span></span>

<span data-ttu-id="6e6ae-162">Eine Deklaration für die externe Methode führt eine neue Methode, deren Implementierung für die Anwendung extern bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-162">An external method declaration introduces a new method whose implementation is provided external to the program.</span></span>

```antlr
ExternalMethodDeclaration
    : ExternalSubDeclaration
    | ExternalFunctionDeclaration
    ;

ExternalSubDeclaration
    : Attributes? ExternalMethodModifier* 'Declare' CharsetModifier? 'Sub'
      Identifier LibraryClause AliasClause?
      ( OpenParenthesis ParameterList? CloseParenthesis )? StatementTerminator
    ;

ExternalFunctionDeclaration
    : Attributes? ExternalMethodModifier* 'Declare' CharsetModifier? 'Function'
      Identifier LibraryClause AliasClause?
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )?
      StatementTerminator
    ;

ExternalMethodModifier
    : AccessModifier
    | 'Shadows'
    | 'Overloads'
    ;

CharsetModifier
    : 'Ansi' | 'Unicode' | 'Auto'
    ;

LibraryClause
    : 'Lib' StringLiteral
    ;

AliasClause
    : 'Alias' StringLiteral
    ;
```

<span data-ttu-id="6e6ae-163">Da eine externe Methodendeklaration keine Implementierungen bietet, verfügt er über keinen Methodentext oder `End` zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-163">Because an external method declaration provides no actual implementation, it has no method body or `End` construct.</span></span> <span data-ttu-id="6e6ae-164">Externe Methoden sind implizit freigegeben, möglicherweise nicht verfügen über Typparameter, und möglicherweise nicht verarbeiten von Ereignissen oder Implementieren von Schnittstellenmembern.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-164">External methods are implicitly shared, may not have type parameters, and may not handle events or implement interface members.</span></span> <span data-ttu-id="6e6ae-165">Wenn kein Rückgabetyp, auf eine Funktion angegeben wird und strenge Semantik verwendet wird, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-165">If no return type is specified on a function and strict semantics are being used, a compile-time error occurs.</span></span> <span data-ttu-id="6e6ae-166">Andernfalls ist der Typ implizit `Object` oder den Typ der Methode Typzeichen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-166">Otherwise the type is implicitly `Object` or the type of the method's type character.</span></span> <span data-ttu-id="6e6ae-167">Die Zugriffsdomäne von den Rückgabetyp und Parametertypen von einer externen Methode muss es sich um die identisch oder eine Obermenge der Zugriffsdomäne von der externen Methode selbst sein.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-167">The accessibility domain of the return type and parameter types of an external method must be the same as or a superset of the accessibility domain of the external method itself.</span></span>

<span data-ttu-id="6e6ae-168">Die Bibliothek-Klausel einer Deklaration für eine externe Methode gibt den Namen der externen Datei, die die Methode implementiert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-168">The library clause of an external method declaration specifies the name of the external file that implements the method.</span></span> <span data-ttu-id="6e6ae-169">Die optionale Alias-Klausel ist eine Zeichenfolge, der angibt, die numerische Ordinalzahl (das Präfix einer `#` Zeichen) oder den Namen der Methode in der externen Datei.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-169">The optional alias clause is a string that specifies the numeric ordinal (prefixed by a `#` character) or name of the method in the external file.</span></span> <span data-ttu-id="6e6ae-170">Ein einzelnen Zeichen Set-Modifizierer kann auch angegeben werden, der bestimmt, dass des Zeichensatzes, das zum Marshallen von Zeichenfolgen während eines Aufrufs an die externe Methode verwendet.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-170">A single-character set modifier may also be specified, which governs the character set used to marshal strings during a call to the external method.</span></span> <span data-ttu-id="6e6ae-171">Die `Unicode` Modifizierer marshallt alle Zeichenfolgen in Unicode-Werten, die `Ansi` Modifizierer marshallt alle Zeichenfolgen in ANSI-Werte, und die `Auto` Modifizierer marshallt Zeichenfolgen gemäß der .NET Framework-Regeln, die basierend auf den Namen der Methode oder der Aliasname angegeben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-171">The `Unicode` modifier marshals all strings to Unicode values, the `Ansi` modifier marshals all strings to ANSI values, and the `Auto` modifier marshals the strings according to .NET Framework rules based on the name of the method, or the alias name if specified.</span></span> <span data-ttu-id="6e6ae-172">Wenn kein Modifizierer angegeben ist, wird standardmäßig `Ansi`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-172">If no modifier is specified, the default is `Ansi`.</span></span>

<span data-ttu-id="6e6ae-173">Wenn `Ansi` oder `Unicode` angegeben ist, wird der Name der Methode in der externen Datei ohne weitere Änderungen gesucht wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-173">If `Ansi` or `Unicode` is specified, then the method name is looked up in the external file with no modification.</span></span> <span data-ttu-id="6e6ae-174">Wenn `Auto` angegeben ist, wird die Methode Namenssuche, die von der Plattform abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-174">If `Auto` is specified, then method name lookup depends on the platform.</span></span> <span data-ttu-id="6e6ae-175">Wenn die Plattform werden ANSI (z. B. Windows 95, Windows 98, Windows ME) betrachtet wird, wird der Name der Methode ohne weitere Änderungen nachgeschlagen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-175">If the platform is considered to be ANSI (for example, Windows 95, Windows 98, Windows ME), then the method name is looked up with no modification.</span></span> <span data-ttu-id="6e6ae-176">Wenn die Suche fehlschlägt, eine `A` angefügt wird und die Suche nach erneuter Versuch gestartet.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-176">If the lookup fails, an `A` is appended and the lookup tried again.</span></span> <span data-ttu-id="6e6ae-177">Wenn die Plattform als Unicode (z. B. Windows NT, Windows 2000, Windows XP), gilt ein `W` angefügt wird und der Name gesucht wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-177">If the platform is considered to be Unicode (for example, Windows NT, Windows 2000, Windows XP), then a `W` is appended and the name is looked up.</span></span> <span data-ttu-id="6e6ae-178">Wenn die Suche ein Fehler auftritt, wird die Suche erneut ohne versucht die `W`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-178">If the lookup fails, the lookup is tried again without the `W`.</span></span> <span data-ttu-id="6e6ae-179">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-179">For example:</span></span>

```vb
Module Test
    ' All platforms bind to "ExternSub".
    Declare Ansi Sub ExternSub Lib "ExternDLL" ()

    ' All platforms bind to "ExternSub".
    Declare Unicode Sub ExternSub Lib "ExternDLL" ()

    ' ANSI platforms: bind to "ExternSub" then "ExternSubA".
    ' Unicode platforms: bind to "ExternSubW" then "ExternSub".
    Declare Auto Sub ExternSub Lib "ExternDLL" ()
End Module
```

<span data-ttu-id="6e6ae-180">Datentypen, die an externe Methoden übergeben werden, werden entsprechend den .NET Framework Data marshalling Konventionen mit einer Ausnahme gemarshallt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-180">Data types being passed to external methods are marshaled according to the .NET Framework data marshalling conventions with one exception.</span></span> <span data-ttu-id="6e6ae-181">String-Variablen, die als Wert übergeben werden (d. h. `ByVal x As String`) gemarshallt werden der OLE-Automatisierung BSTR-Typ, und Änderungen an der BSTR in der externen Methode wieder im Zeichenfolgenargument widergespiegelt werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-181">String variables that are passed by value (that is, `ByVal x As String`) are marshaled to the OLE Automation BSTR type, and changes made to the BSTR in the external method are reflected back in the string argument.</span></span> <span data-ttu-id="6e6ae-182">Grund hierfür ist der Typ `String` in externen Methoden ist änderbar und diese spezielle marshalling imitiert dieses Verhalten.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-182">This is because the type `String` in external methods is mutable, and this special marshalling mimics that behavior.</span></span> <span data-ttu-id="6e6ae-183">String-Parameter, die als Verweis übergeben werden (d. h. `ByRef x As String`) werden als Zeiger auf den OLE-Automatisierung BSTR-Typ gemarshallt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-183">String parameters that are passed by reference (i.e. `ByRef x As String`) are marshaled as a pointer to the OLE Automation BSTR type.</span></span> <span data-ttu-id="6e6ae-184">Es ist möglich, überschreiben Sie diese speziellen Verhaltensweisen durch Angabe der `System.Runtime.InteropServices.MarshalAsAttribute` Attribut für den Parameter.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-184">It is possible to override these special behaviors by specifying the `System.Runtime.InteropServices.MarshalAsAttribute` attribute on the parameter.</span></span>

<span data-ttu-id="6e6ae-185">Das Beispiel veranschaulicht die Verwendung externer Methoden:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-185">The example demonstrates use of external methods:</span></span>

```vb
Class Path
    Declare Function CreateDirectory Lib "kernel32" ( _
        Name As String, sa As SecurityAttributes) As Boolean
    Declare Function RemoveDirectory Lib "kernel32" ( _
        Name As String) As Boolean
    Declare Function GetCurrentDirectory Lib "kernel32" ( _
        BufSize As Integer, Buf As String) As Integer
    Declare Function SetCurrentDirectory Lib "kernel32" ( _
        Name As String) As Boolean
End Class
```


### <a name="overridable-methods"></a><span data-ttu-id="6e6ae-186">Überschreibbare Methoden</span><span class="sxs-lookup"><span data-stu-id="6e6ae-186">Overridable Methods</span></span>

<span data-ttu-id="6e6ae-187">Die `Overridable` Modifizierer gibt an, dass eine Methode überschrieben werden kann.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-187">The `Overridable` modifier indicates that a method is overridable.</span></span> <span data-ttu-id="6e6ae-188">Die `Overrides` Modifizierer gibt an, dass eine Methode eine Basistyp überschreibbare Methode überschreibt, die die gleiche Signatur aufweist.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-188">The `Overrides` modifier indicates that a method overrides a base-type overridable method that has the same signature.</span></span> <span data-ttu-id="6e6ae-189">Die `NotOverridable` Modifizierer gibt an, dass eine überschreibbare Methode nicht weiter überschrieben werden kann.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-189">The `NotOverridable` modifier indicates that an overridable method cannot be further overridden.</span></span> <span data-ttu-id="6e6ae-190">Die `MustOverride` Modifizierer gibt an, dass eine Methode in abgeleiteten Klassen überschrieben werden muss.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-190">The `MustOverride` modifier indicates that a method must be overridden in derived classes.</span></span>

<span data-ttu-id="6e6ae-191">Bestimmte Kombinationen dieser Modifizierer sind nicht gültig:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-191">Certain combinations of these modifiers are not valid:</span></span>

* <span data-ttu-id="6e6ae-192">`Overridable` und `NotOverridable` gegenseitig und können nicht kombiniert werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-192">`Overridable` and `NotOverridable` are mutually exclusive and cannot be combined.</span></span>

* <span data-ttu-id="6e6ae-193">`MustOverride` impliziert `Overridable` (und damit sie nicht angeben) und können nicht kombiniert werden, mit `NotOverridable`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-193">`MustOverride` implies `Overridable` (and so cannot specify it) and cannot be combined with `NotOverridable`.</span></span>

* <span data-ttu-id="6e6ae-194">`NotOverridable` können nicht kombiniert werden, mit `Overridable` oder `MustOverride` und muss zusammen mit `Overrides`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-194">`NotOverridable` cannot be combined with `Overridable` or `MustOverride` and must be combined with `Overrides`.</span></span>

* <span data-ttu-id="6e6ae-195">`Overrides` impliziert `Overridable` (und damit sie nicht angeben) und können nicht kombiniert werden, mit `MustOverride`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-195">`Overrides` implies `Overridable` (and so cannot specify it) and cannot be combined with `MustOverride`.</span></span>

<span data-ttu-id="6e6ae-196">Es gibt auch zusätzliche Beschränkungen auf überschreibbaren Methoden:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-196">There are also additional restrictions on overridable methods:</span></span>

* <span data-ttu-id="6e6ae-197">Ein `MustOverride` Methode darf keinen Methodentext oder `End` erstellen, können keine andere Methode überschreiben und darf nur in `MustInherit` Klassen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-197">A `MustOverride` method may not include a method body or an `End` construct, may not override another method, and may only appear in `MustInherit` classes.</span></span>

* <span data-ttu-id="6e6ae-198">Wenn eine Methode angibt `Overrides` und es gibt keine übereinstimmende Basismethode außer Kraft zu setzenden ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-198">If a method specifies `Overrides` and there is no matching base method to override, a compile-time error occurs.</span></span> <span data-ttu-id="6e6ae-199">Eine überschreibende Methode kann keine angeben `Shadows`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-199">An overriding method may not specify `Shadows`.</span></span>

* <span data-ttu-id="6e6ae-200">Eine Methode kann keine andere Methode überschreiben, ist die überschreibende Methode Zugriffsdomäne nicht entspricht die Zugriffsdomäne von der Methode überschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-200">A method may not override another method if the overriding method's accessibility domain is not equal to the accessibility domain of the method being overridden.</span></span> <span data-ttu-id="6e6ae-201">Die einzige Ausnahme ist, die eine Methode zum Überschreiben einer `Protected Friend` -Methode in einer anderen Assembly, die nicht `Friend` Zugriff angeben muss `Protected` (nicht `Protected Friend`).</span><span class="sxs-lookup"><span data-stu-id="6e6ae-201">The one exception is that a method overriding a `Protected Friend` method in another assembly that does not have `Friend` access must specify `Protected` (not `Protected Friend`).</span></span>

* <span data-ttu-id="6e6ae-202">`Private` Methoden möglicherweise nicht `Overridable`, `NotOverridable`, oder `MustOverride`, noch können sie andere Methoden überschreiben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-202">`Private` methods may not be `Overridable`, `NotOverridable`, or `MustOverride`, nor may they override other methods.</span></span>

* <span data-ttu-id="6e6ae-203">Methoden in `NotInheritable` Klassen können nicht deklariert werden `Overridable` oder `MustOverride`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-203">Methods in `NotInheritable` classes may not be declared `Overridable` or `MustOverride`.</span></span>

<span data-ttu-id="6e6ae-204">Im folgende Beispiel werden die Unterschiede zwischen überschreibbare und nicht überschreibbaren Methoden veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-204">The following example illustrates the differences between overridable and nonoverridable methods:</span></span>

```vb
Class Base
    Public Sub F()
        Console.WriteLine("Base.F")
    End Sub

    Public Overridable Sub G()
        Console.WriteLine("Base.G")
    End Sub
End Class

Class Derived
    Inherits Base

    Public Shadows Sub F()
        Console.WriteLine("Derived.F")
    End Sub

    Public Overrides Sub G()
        Console.WriteLine("Derived.G")
    End Sub
End Class

Module Test
    Sub Main()
        Dim d As Derived = New Derived()
        Dim b As Base = d

        b.F()
        d.F()
        b.G()
        d.G()
    End Sub
End Module
```

<span data-ttu-id="6e6ae-205">In diesem Beispiel-Klasse `Base` führt eine Methode `F` und `Overridable` Methode `G`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-205">In the example, class `Base` introduces a method `F` and an `Overridable` method `G`.</span></span> <span data-ttu-id="6e6ae-206">Die Klasse `Derived` führt eine neue Methode `F`, daher shadowing der geerbten `F`, und außerdem überschreibt die geerbte Methode `G`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-206">The class `Derived` introduces a new method `F`, thus shadowing the inherited `F`, and also overrides the inherited method `G`.</span></span> <span data-ttu-id="6e6ae-207">Das Beispiel führt zur folgenden Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-207">The example produces the following output:</span></span>

```
Base.F
Derived.F
Derived.G
Derived.G
```

<span data-ttu-id="6e6ae-208">Beachten Sie, dass die Anweisung `b.G()` ruft `Derived.G`, nicht `Base.G`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-208">Notice that the statement `b.G()` invokes `Derived.G`, not `Base.G`.</span></span> <span data-ttu-id="6e6ae-209">Dies ist, da der Laufzeittyp der Instanz (die `Derived`) anstelle der Kompilierzeit-Typ der Instanz (d.h. `Base`) bestimmt die Implementierung der tatsächlichen Methode aufrufen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-209">This is because the run-time type of the instance (which is `Derived`) rather than the compile-time type of the instance (which is `Base`) determines the actual method implementation to invoke.</span></span>

### <a name="shared-methods"></a><span data-ttu-id="6e6ae-210">Freigegebene Methoden</span><span class="sxs-lookup"><span data-stu-id="6e6ae-210">Shared Methods</span></span>

<span data-ttu-id="6e6ae-211">Die `Shared` Modifizierer gibt an, dass eine Methode eine *freigegebene Methode*.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-211">The `Shared` modifier indicates a method is a *shared method*.</span></span> <span data-ttu-id="6e6ae-212">Eine freigegebene Methode führt keine Vorgänge für eine bestimmte Instanz eines Typs und kann direkt von einem Typ und nicht über eine bestimmte Instanz eines Typs aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-212">A shared method does not operate on a specific instance of a type and may be invoked directly from a type rather than through a particular instance of a type.</span></span> <span data-ttu-id="6e6ae-213">Es ist jedoch gültig ist, eine Instanz zu verwenden, um eine freigegebene Methode zu qualifizieren.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-213">It is valid, however, to use an instance to qualify a shared method.</span></span> <span data-ttu-id="6e6ae-214">Es ist unzulässig, verweisen auf `Me`, `MyClass`, oder `MyBase` in einer freigegebenen Methode.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-214">It is invalid to refer to `Me`, `MyClass`, or `MyBase` in a shared method.</span></span> <span data-ttu-id="6e6ae-215">Freigegebene Methoden möglicherweise nicht `Overridable`, `NotOverridable`, oder `MustOverride`, und sie dürfen die Methoden nicht überschreiben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-215">Shared methods may not be `Overridable`, `NotOverridable`, or `MustOverride`, and they may not override methods.</span></span> <span data-ttu-id="6e6ae-216">Methoden, die definiert, die in standard-Module und Schnittstellen können keine angeben `Shared`, da sie implizit sind `Shared` bereits.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-216">Methods defined in standard modules and interfaces may not specify `Shared`, because they are implicitly `Shared` already.</span></span>

<span data-ttu-id="6e6ae-217">Eine Methode deklariert werden, in einer Struktur oder Klasse, ohne dass eine `Shared` Modifizierer ist ein *Instanzmethode*.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-217">A method declared in a structure or class without a `Shared` modifier is an *instance method*.</span></span> <span data-ttu-id="6e6ae-218">Eine Instanzmethode funktioniert für eine bestimmte Instanz eines Typs.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-218">An instance method operates on a given instance of a type.</span></span> <span data-ttu-id="6e6ae-219">Instanzmethoden kann nur über eine Instanz eines Typs aufgerufen werden und bezieht sich möglicherweise auf die Instanz mithilfe der `Me` Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-219">Instance methods can only be invoked through an instance of a type and may refer to the instance through the `Me` expression.</span></span>

<span data-ttu-id="6e6ae-220">Das folgende Beispiel veranschaulicht die Regeln für den Zugriff auf freigegebene als auch Instanzmember:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-220">The following example illustrates the rules for accessing shared and instance members:</span></span>

```vb
Class Test
    Private x As Integer
    Private Shared y As Integer

    Sub F()
        x = 1 ' Ok, same as Me.x = 1.
        y = 1 ' Ok, same as Test.y = 1.
    End Sub

    Shared Sub G()
        x = 1 ' Error, cannot access Me.x.
        y = 1 ' Ok, same as Test.y = 1.
    End Sub

    Shared Sub Main()
        Dim t As Test = New Test()

        t.x = 1 ' Ok.
        t.y = 1 ' Ok.
        Test.x = 1 ' Error, cannot access instance member through type.
        Test.y = 1 ' Ok.
    End Sub
End Class
```

<span data-ttu-id="6e6ae-221">Methode `F` zeigt an, dass in ein Instanzmember-Funktion, ein Bezeichner für den Zugriff auf Instanzmember und freigegebene Member verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-221">Method `F` shows that in an instance function member, an identifier can be used to access both instance members and shared members.</span></span> <span data-ttu-id="6e6ae-222">Methode `G` zeigt an, dass in einem freigegebenen Funktionsmember, ein Fehler auf einen Instanzmember zuzugreifen, über einen Bezeichner kann.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-222">Method `G` shows that in a shared function member, it is an error to access an instance member through an identifier.</span></span> <span data-ttu-id="6e6ae-223">Methode `Main` zeigt, dass in einem Memberzugriffsausdruck Instanzmember über Instanzen zugegriffen werden müssen, aber gemeinsam genutzten Member über Typen oder Instanzen zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-223">Method `Main` shows that in a member access expression, instance members must be accessed through instances, but shared members can be accessed through types or instances.</span></span>

### <a name="method-parameters"></a><span data-ttu-id="6e6ae-224">Methodenparameter</span><span class="sxs-lookup"><span data-stu-id="6e6ae-224">Method Parameters</span></span>

<span data-ttu-id="6e6ae-225">Ein *Parameter* ist eine Variable, die verwendet werden kann, um Informationen in und aus einer Methode zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-225">A *parameter* is a variable that can be used to pass information into and out of a method.</span></span> <span data-ttu-id="6e6ae-226">Parameter einer Methode werden von der Parameterliste der Methode deklariert, besteht aus einem oder mehreren Parametern, die durch Kommas getrennt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-226">Parameters of a method are declared by the method's parameter list, which consists of one or more parameters separated by commas.</span></span>

```antlr
ParameterList
    : Parameter ( Comma Parameter )*
    ;

Parameter
    : Attributes? ParameterModifier* ParameterIdentifier ( 'As' TypeName )?
      ( Equals ConstantExpression )?
    ;

ParameterModifier
    : 'ByVal' | 'ByRef' | 'Optional' | 'ParamArray'
    ;

ParameterIdentifier
    : Identifier IdentifierModifiers
    ;
```

<span data-ttu-id="6e6ae-227">Wenn kein Typ für einen Parameter angegeben ist, und strikte Semantik verwendet werden, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-227">If no type is specified for a parameter and strict semantics are used, a compile-time error occurs.</span></span> <span data-ttu-id="6e6ae-228">Andernfalls ist der Standardtyp `Object` oder den Typ des Parameters-Typzeichen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-228">Otherwise the default type is `Object` or the type of the parameter's type character.</span></span> <span data-ttu-id="6e6ae-229">Sogar unter Semantik, wenn ein Parameter enthält einen `As` -Klausel, alle Parameter müssen Typen angeben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-229">Even under permissive semantics, if one parameter includes an `As` clause, all parameters must specify types.</span></span>

<span data-ttu-id="6e6ae-230">Parameter als Wert, optional, Verweis oder Paramarray-Parameter angegeben werden, indem Sie die Modifizierer `ByVal`, `ByRef`, `Optional`, und `ParamArray`bzw.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-230">Parameters are specified as value, reference, optional, or paramarray parameters by the modifiers `ByVal`, `ByRef`, `Optional`, and `ParamArray`, respectively.</span></span> <span data-ttu-id="6e6ae-231">Ein Parameter, der nicht `ByRef` oder `ByVal` standardmäßig `ByVal`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-231">A parameter that does not specify `ByRef` or `ByVal` defaults to `ByVal`.</span></span>

<span data-ttu-id="6e6ae-232">Parameternamen definiert und gelten für den gesamten Text der Methode und sind immer öffentlich zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-232">Parameter names are scoped to the entire body of the method and are always publicly accessible.</span></span> <span data-ttu-id="6e6ae-233">Aufruf einer Methode erstellt eine Kopie, die speziell für diesen Aufruf, der Parameter, und die Argumentliste des Aufrufs weist Werte oder Variablenverweise an die neu erstellte Parameter.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-233">A method invocation creates a copy, specific to that invocation, of the parameters, and the argument list of the invocation assigns values or variable references to the newly created parameters.</span></span> <span data-ttu-id="6e6ae-234">Da externe Methodendeklarationen und Delegatdeklarationen ohne Text aufweisen, sind doppelte Parameternamen in Parameterlisten zulässig, aber nicht empfohlen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-234">Because external method declarations and delegate declarations have no body, duplicate parameter names are allowed in parameter lists, but discouraged.</span></span>

<span data-ttu-id="6e6ae-235">Der Bezeichner der Modifizierer auf NULL festlegbare Name folgt möglicherweise `?` , um anzugeben, dass es NULL-Werte zulässt, und auch Namen Arraymodifizierer, um anzugeben, dass die It ein Array ist.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-235">The identifier may be followed by the nullable name modifier `?` to indicate that it is nullable, and also by array name modifiers to indicate that it is an array.</span></span> <span data-ttu-id="6e6ae-236">Sie können kombiniert werden, z. B. "`ByVal x?() As Integer`".</span><span class="sxs-lookup"><span data-stu-id="6e6ae-236">They may be combined, e.g. "`ByVal x?() As Integer`".</span></span> <span data-ttu-id="6e6ae-237">Es ist nicht mit expliziten Arraygrenzen zulässig. auch wenn der Modifizierer auf NULL festlegbare Namen vorhanden ist wird eine `As` Klausel muss vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-237">It is not allowed to use explicit array bounds; also, if the nullable name modifier is present then an `As` clause must be present.</span></span>


#### <a name="value-parameters"></a><span data-ttu-id="6e6ae-238">Wert-Parametern</span><span class="sxs-lookup"><span data-stu-id="6e6ae-238">Value Parameters</span></span>

<span data-ttu-id="6e6ae-239">Ein *"Value"-Parameter* wird deklariert, mit einem expliziten `ByVal` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-239">A *value parameter* is declared with an explicit `ByVal` modifier.</span></span> <span data-ttu-id="6e6ae-240">Wenn die `ByVal` -Modifizierer wird verwendet, die `ByRef` Modifizierer dürfen nicht angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-240">If the `ByVal` modifier is used, the `ByRef` modifier may not be specified.</span></span> <span data-ttu-id="6e6ae-241">Ein Werteparameter wird mit dem Aufruf des Members der Parameter gehört zu und initialisiert wird, mit dem Wert des Arguments im Aufruf angegeben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-241">A value parameter comes into existence with the invocation of the member the parameter belongs to, and is initialized with the value of the argument given in the invocation.</span></span> <span data-ttu-id="6e6ae-242">Ein Value-Parameter wird bei der Rückgabe des Elements gelöscht.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-242">A value parameter ceases to exist upon return of the member.</span></span>

<span data-ttu-id="6e6ae-243">Eine Methode ist zulässig, einen Wertparameter neue Werte zuweisen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-243">A method is permitted to assign new values to a value parameter.</span></span> <span data-ttu-id="6e6ae-244">Diese Zuweisungen wirken sich nur auf den Speicherort der lokalen Speicherressource, dargestellt durch den Wertparameter; Sie haben keine Auswirkungen auf das tatsächliche Argument im Methodenaufruf angegeben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-244">Such assignments only affect the local storage location represented by the value parameter; they have no effect on the actual argument given in the method invocation.</span></span>

<span data-ttu-id="6e6ae-245">Ein Value-Parameter wird verwendet, wenn der Wert eines Arguments in eine Methode übergeben, und Änderungen an den Parameter sich nicht auf den ursprünglichen Arguments wirken.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-245">A value parameter is used when the value of an argument is passed into a method, and modifications of the parameter do not impact the original argument.</span></span> <span data-ttu-id="6e6ae-246">Ein Value-Parameter verweist auf eine eigene Variable, eine Variable, die sich aus der Variablen des entsprechenden Arguments unterscheidet.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-246">A value parameter refers to its own variable, one that is distinct from the variable of the corresponding argument.</span></span> <span data-ttu-id="6e6ae-247">Diese Variable wird initialisiert, indem der Wert des entsprechenden Arguments kopiert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-247">This variable is initialized by copying the value of the corresponding argument.</span></span> <span data-ttu-id="6e6ae-248">Das folgende Beispiel zeigt eine Methode `F` , die über einen Werteparameter, die mit dem Namen verfügt `p`:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-248">The following example shows a method `F` that has a value parameter named `p`:</span></span>

```vb
Module Test
    Sub F(p As Integer)
        Console.WriteLine("p = " & p)
        p += 1
    End Sub 

    Sub Main()
        Dim a As Integer = 1

        Console.WriteLine("pre: a = " & a)
        F(a)
        Console.WriteLine("post: a = " & a)
    End Sub
End Module
```

<span data-ttu-id="6e6ae-249">Das Beispiel erzeugt der folgenden Ausgabe, auch wenn den Value-Parameter `p` geändert wird:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-249">The example produces the following output, even though the value parameter `p` is modified:</span></span>

```
pre: a = 1
p = 1
post: a = 1
```

#### <a name="reference-parameters"></a><span data-ttu-id="6e6ae-250">Verweisparameter</span><span class="sxs-lookup"><span data-stu-id="6e6ae-250">Reference Parameters</span></span>

<span data-ttu-id="6e6ae-251">Ein Verweisparameter ist ein Parameter, die mit dem deklariert eine `ByRef` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-251">A reference parameter is a parameter declared with a `ByRef` modifier.</span></span> <span data-ttu-id="6e6ae-252">Wenn die `ByRef` -Modifizierer wird angegeben, die `ByVal` Modifizierer kann nicht verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-252">If the `ByRef` modifier is specified, the `ByVal` modifier may not be used.</span></span> <span data-ttu-id="6e6ae-253">Ein Verweisparameter wird nicht mit einen neuen Speicherort erstellt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-253">A reference parameter does not create a new storage location.</span></span> <span data-ttu-id="6e6ae-254">Stattdessen stellt ein Verweisparameter die Variable als Argument im Aufruf Methode oder des Konstruktors angegeben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-254">Instead, a reference parameter represents the variable given as the argument in the method or constructor invocation.</span></span> <span data-ttu-id="6e6ae-255">Vom Konzept her ist der Wert eines Verweisparameters immer identisch mit der zugrunde liegenden Variablen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-255">Conceptually, the value of a reference parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="6e6ae-256">Verweisparameter agieren in zwei Modi, entweder als *Aliase* oder über *Kopie in Kopie zurück.*</span><span class="sxs-lookup"><span data-stu-id="6e6ae-256">Reference parameters act in two modes, either as *aliases* or through *copy-in copy-back.*</span></span>

<span data-ttu-id="6e6ae-257">__Aliase.__</span><span class="sxs-lookup"><span data-stu-id="6e6ae-257">__Aliases.__</span></span> <span data-ttu-id="6e6ae-258">Ein Verweisparameter wird verwendet, wenn der Parameter als Alias für ein vom Aufrufer bereitgestellten Argument fungiert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-258">A reference parameter is used when the parameter acts as an alias for a caller-provided argument.</span></span> <span data-ttu-id="6e6ae-259">Ein Verweisparameter definiert nicht selbst eine Variable, sondern verweist auf die Variable des entsprechenden Arguments.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-259">A reference parameter does not itself define a variable, but rather refers to the variable of the corresponding argument.</span></span> <span data-ttu-id="6e6ae-260">Änderungen an Verweisparameter Auswirkungen direkt und unmittelbar das entsprechende Argument auf.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-260">Modifications of a reference parameter directly and immediately impact the corresponding argument.</span></span> <span data-ttu-id="6e6ae-261">Das folgende Beispiel zeigt eine Methode `Swap` über zwei Verweisparameter verfügt:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-261">The following example shows a method `Swap` that has two reference parameters:</span></span>

```vb
Module Test
    Sub Swap(ByRef a As Integer, ByRef b As Integer)
        Dim t As Integer = a
        a = b
        b = t
    End Sub 

    Sub Main()
        Dim x As Integer = 1
        Dim y As Integer = 2

        Console.WriteLine("pre: x = " & x & ", y = " & y)
        Swap(x, y)
        Console.WriteLine("post: x = " & x & ", y = " & y)
    End Sub 
End Module
```

<span data-ttu-id="6e6ae-262">Die Ausgabe des Programms lautet:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-262">The output of the program is:</span></span>

```
pre: x = 1, y = 2
post: x = 2, y = 1
```

<span data-ttu-id="6e6ae-263">Für den Aufruf der Methode `Swap` in Klasse `Main`, `a` stellt `x,` und `b` stellt `y`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-263">For the invocation of method `Swap` in class `Main`, `a` represents `x,` and `b` represents `y`.</span></span> <span data-ttu-id="6e6ae-264">Der Aufruf ist daher die Auswirkungen der Tauschen der Werte der `x` und `y`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-264">Thus, the invocation has the effect of swapping the values of `x` and `y`.</span></span>

<span data-ttu-id="6e6ae-265">In eine Methode, nimmt die Parameter verweisen ist es möglich, dass mehrere Namen am gleichen Speicherort dargestellt:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-265">In a method that takes reference parameters, it is possible for multiple names to represent the same storage location:</span></span>

```vb
Module Test
    Private s As String

    Sub F(ByRef a As String, ByRef b As String)
        s = "One"
        a = "Two"
        b = "Three"
    End Sub

    Sub G()
        F(s, s)
    End Sub
End Module
```

<span data-ttu-id="6e6ae-266">Im Beispiel der Aufruf der Methode `F` in `G` übergibt einen Verweis auf `s` für beide `a` und `b`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-266">In the example the invocation of method `F` in `G` passes a reference to `s` for both `a` and `b`.</span></span> <span data-ttu-id="6e6ae-267">Daher ist es bei diesen Aufruf, der die Namen `s`, `a`, und `b` verweisen alle auf den gleichen Speicherort aus, und die drei alle Zuweisungen ändern Sie die Instanzvariable `s`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-267">Thus, for that invocation, the names `s`, `a`, and `b` all refer to the same storage location, and the three assignments all modify the instance variable `s`.</span></span>

<span data-ttu-id="6e6ae-268">__Kopieren Sie sich an Kopie zurück.__</span><span class="sxs-lookup"><span data-stu-id="6e6ae-268">__Copy-in copy-back.__</span></span> <span data-ttu-id="6e6ae-269">Wenn der Typ der Variablen an Verweisparameter übergeben werden nicht mit dem Typ der Verweisparameter, kompatibel ist oder wenn eine nicht-Variable (z. B. eine Eigenschaft) als Argument an einen Verweisparameter übergeben wird oder ist der Aufruf spät gebunden, und klicken Sie dann auf eine temporäre Variable zugeordnet und der Verweisparameter übergeben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-269">If the type of the variable being passed to a reference parameter is not compatible with the reference parameter's type, or if a non-variable (e.g. a property) is passed as an argument to a reference parameter, or if the invocation is late-bound, then a temporary variable is allocated and passed to the reference parameter.</span></span> <span data-ttu-id="6e6ae-270">Der Wert, der übergeben wird, bevor die Methode aufgerufen wird, und wieder die ursprüngliche Variable kopiert werden (sofern vorhanden und nicht schreibgeschützt ist) beim Beenden der Methode in diese temporäre Variable kopiert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-270">The value being passed in will be copied into this temporary variable before the method is invoked and will be copied back to the original variable (if there is one and if it's writable) when the method returns.</span></span> <span data-ttu-id="6e6ae-271">Daher ein Verweisparameter darf keine unbedingt einen Verweis auf den genauen Speicher, der die Variable übergeben werden, und alle Änderungen an der Verweisparameter werden in der Variablen nicht wirksam, bis die Methode beendet wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-271">Thus, a reference parameter may not necessarily contain a reference to the exact storage of the variable being passed in, and any changes to the reference parameter may not be reflected in the variable until the method exits.</span></span> <span data-ttu-id="6e6ae-272">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-272">For example:</span></span>

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class

Module Test
    Sub F(ByRef b As Base)
        b = New Base()
    End Sub

    Property G() As Base
        Get
        End Get
        Set
        End Set
    End Property

    Sub Main()
        Dim d As Derived

        F(G)   ' OK.
        F(d)   ' Throws System.InvalidCastException after F returns.
    End Sub
End Module
```

<span data-ttu-id="6e6ae-273">Bei den ersten Aufruf des `F`, eine temporäre Variable erstellt wird und der Wert der Eigenschaft `G` zugewiesen ist, und übergeben Sie in `F`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-273">In the case of the first invocation of `F`, a temporary variable is created and the value of the property `G` is assigned to it and passed into `F`.</span></span> <span data-ttu-id="6e6ae-274">Bei der Rückgabe von `F`, der Wert in der temporären Variablen wird zugewiesen, an die Eigenschaft des `G`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-274">Upon return from `F`, the value in the temporary variable is assigned back to the property of `G`.</span></span> <span data-ttu-id="6e6ae-275">Im zweiten Fall wird eine andere temporäre Variable erstellt und der Wert der `d` zugewiesen ist, und übergeben Sie in `F`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-275">In the second case, another temporary variable is created and the value of `d` is assigned to it and passed into `F`.</span></span> <span data-ttu-id="6e6ae-276">Bei der Rückgabe von `F`, der Wert in der temporären Variablen wieder in den Typ der Variablen umgewandelt wird `Derived`, zugewiesen `d`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-276">When returning from `F`, the value in the temporary variable is cast back to the type of the variable, `Derived`, and assigned to `d`.</span></span> <span data-ttu-id="6e6ae-277">Da der Wert zurück übergeben werden nicht konvertiert werden kann `Derived`, zur Laufzeit eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-277">Since the value being passed back cannot be cast to `Derived`, an exception is thrown at run time.</span></span>

#### <a name="optional-parameters"></a><span data-ttu-id="6e6ae-278">Optionale Parameter</span><span class="sxs-lookup"><span data-stu-id="6e6ae-278">Optional Parameters</span></span>

<span data-ttu-id="6e6ae-279">Ein optionaler Parameter ist deklariert, mit der `Optional` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-279">An optional parameter is declared with the `Optional` modifier.</span></span> <span data-ttu-id="6e6ae-280">Parameter, die einen optionalen Parameter in der Liste der formalen Parameter folgen müssen ebenfalls optional sein; Fehler beim Angeben der `Optional` Modifizierer in den folgenden Parametern löst einen Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-280">Parameters that follow an optional parameter in the formal parameter list must be optional as well; failure to specify the `Optional` modifier on the following parameters will trigger a compile-time error.</span></span> <span data-ttu-id="6e6ae-281">Ein optionaler Parameter für einige geben nullable-Typ `T?` oder NULL-Typ `T` muss einen konstanten Ausdruck angeben `e` als Standardwert verwendet werden soll, kein Argument angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-281">An optional parameter of some type nullable type `T?` or non-nullable type `T` must specify a constant expression `e` to be used as a default value if no argument is specified.</span></span> <span data-ttu-id="6e6ae-282">Wenn `e` ergibt `Nothing` von Typ "Object" wird der Standardwert von der *Parametertyp* wird als Standardwert für den Parameter verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-282">If `e` evaluates to `Nothing` of type Object, then the default value of the *parameter type* will be used as the default for the parameter.</span></span> <span data-ttu-id="6e6ae-283">Andernfalls `CType(e, T)` muss ein konstanter Ausdruck sein, und es ist als der Standardwert für den Parameter.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-283">Otherwise, `CType(e, T)` must be a constant expression and it is taken as the default for the parameter.</span></span>

<span data-ttu-id="6e6ae-284">Optionale Parameter sind die einzige Situation, in der ein Initialisierer für einen Parameter gültig ist.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-284">Optional parameters are the only situation in which an initializer on a parameter is valid.</span></span> <span data-ttu-id="6e6ae-285">Die Initialisierung erfolgt immer als Teil der Aufrufausdruck, nicht innerhalb des Methodentexts selbst.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-285">The initialization is always done as a part of the invocation expression, not within the method body itself.</span></span>

```vb
Module Test
    Sub F(x As Integer, Optional y As Integer = 20)
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub

    Sub Main()
        F(10)
        F(30,40)
    End Sub
End Module
```

<span data-ttu-id="6e6ae-286">Die Ausgabe des Programms lautet:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-286">The output of the program is:</span></span>

```
x = 10, y = 20
x = 30, y = 40
```

<span data-ttu-id="6e6ae-287">Optionale Parameter können nicht in Deklarationen für Delegat- oder Ereignisparameter noch in Lambda-Ausdrücke angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-287">Optional parameters may not be specified in delegate or event declarations, nor in lambda expressions.</span></span>

#### <a name="paramarray-parameters"></a><span data-ttu-id="6e6ae-288">ParamArray-Parameter</span><span class="sxs-lookup"><span data-stu-id="6e6ae-288">ParamArray Parameters</span></span>

<span data-ttu-id="6e6ae-289">`ParamArray` Parameter werden deklariert, mit der `ParamArray` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-289">`ParamArray` parameters are declared with the `ParamArray` modifier.</span></span> <span data-ttu-id="6e6ae-290">Wenn die `ParamArray` Modifizierer vorhanden ist, die `ByVal` Modifizierer muss angegeben werden, und keine anderen Parameter können Sie die `ParamArray` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-290">If the `ParamArray` modifier is present, the `ByVal` modifier must be specified, and no other parameter may use the `ParamArray` modifier.</span></span> <span data-ttu-id="6e6ae-291">Die `ParamArray` Typ des Parameters muss ein eindimensionales Array sein, und es muss der letzte Parameter in der Parameterliste sein.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-291">The `ParamArray` parameter's type must be a one-dimensional array, and it must be the last parameter in the parameter list.</span></span>

<span data-ttu-id="6e6ae-292">Ein `ParamArray` Parameter darstellt, eine unbestimmte Anzahl von Parametern des Typs von der `ParamArray`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-292">A `ParamArray` parameter represents an indeterminate number of parameters of the type of the `ParamArray`.</span></span> <span data-ttu-id="6e6ae-293">In der Methode selbst eine `ParamArray` Parameter als seinem deklarierten Typ behandelt und besitzt keine spezielle Semantik.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-293">Within the method itself, a `ParamArray` parameter is treated as its declared type and has no special semantics.</span></span> <span data-ttu-id="6e6ae-294">Ein `ParamArray` -Parameter ist implizit optional, Standardwert eine leere eindimensionalen Arrays des Typs von der `ParamArray`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-294">A `ParamArray` parameter is implicitly optional, with a default value of an empty one-dimensional array of the type of the `ParamArray`.</span></span>

<span data-ttu-id="6e6ae-295">Ein `ParamArray` können die Argumente in eine von zwei Arten in einem Methodenaufruf angegeben werden:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-295">A `ParamArray` permits arguments to be specified in one of two ways in a method invocation:</span></span>

* <span data-ttu-id="6e6ae-296">Das Argument für die ein `ParamArray` kann ein einzelner Ausdruck eines Typs, die erweitert werden kann, werden die `ParamArray` Typ.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-296">The argument given for a `ParamArray` can be a single expression of a type that widens to the `ParamArray` type.</span></span> <span data-ttu-id="6e6ae-297">In diesem Fall die `ParamArray` verhält sich genau wie ein Value-Parameter.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-297">In this case, the `ParamArray` acts precisely like a value parameter.</span></span>

* <span data-ttu-id="6e6ae-298">Der Aufruf kann auch angeben, NULL oder mehr Argumente für die `ParamArray`, wobei jedes Argument einen Ausdruck eines Typs, der implizit in den Typ des Elements, der die `ParamArray`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-298">Alternatively, the invocation can specify zero or more arguments for the `ParamArray`, where each argument is an expression of a type that is implicitly convertible to the element type of the `ParamArray`.</span></span> <span data-ttu-id="6e6ae-299">In diesem Fall der Aufruf erstellt eine Instanz der `ParamArray` Typ mit einer Länge, die Anzahl der Argumente, die Elemente des Arrays mit den Werten des angegebenen Arguments Instanz, initialisiert und verwendet das neu erstellte Array-Instanz als die tatsächliche entsprechen Argument.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-299">In this case, the invocation creates an instance of the `ParamArray` type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="6e6ae-300">Mit Ausnahme von ermöglichen, dass eine Variable Anzahl von Argumenten in einem Aufruf einer `ParamArray` entspricht genau eine Value-Parameter des gleichen Typs wie das folgende Beispiel veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-300">Except for allowing a variable number of arguments in an invocation, a `ParamArray` is precisely equivalent to a value parameter of the same type, as the following example illustrates.</span></span>

```vb
Module Test
    Sub F(ParamArray args() As Integer)
        Dim i As Integer

        Console.Write("Array contains " & args.Length & " elements:")
        For Each i In args
            Console.Write(" " & i)
        Next i
        Console.WriteLine()
    End Sub

    Sub Main()
        Dim a As Integer() = { 1, 2, 3 }

        F(a)
        F(10, 20, 30, 40)
        F()
    End Sub
End Module
```

<span data-ttu-id="6e6ae-301">Das Beispiel erzeugt die Ausgabe</span><span class="sxs-lookup"><span data-stu-id="6e6ae-301">The example produces the output</span></span>

```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

<span data-ttu-id="6e6ae-302">Der erste Aufruf der `F` einfach das Array übergibt `a` als ein Value-Parameter.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-302">The first invocation of `F` simply passes the array `a` as a value parameter.</span></span> <span data-ttu-id="6e6ae-303">Der zweite Aufruf von `F` automatisch erstellt ein Array mit vier Elementen mit den Werten des angegebenen Elements und übergibt diese Arrayinstanz als ein Value-Parameter.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-303">The second invocation of `F` automatically creates a four-element array with the given element values and passes that array instance as a value parameter.</span></span> <span data-ttu-id="6e6ae-304">Entsprechend der dritte Aufruf von `F` erstellt ein Array mit 0 (null) Elementen, und übergibt diese Instanz als ein Value-Parameter.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-304">Likewise, the third invocation of `F` creates a zero-element array and passes that instance as a value parameter.</span></span> <span data-ttu-id="6e6ae-305">Die zweite und dritte Aufrufe sind wie folgt:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-305">The second and third invocations are precisely equivalent to writing:</span></span>

```vb
F(New Integer() {10, 20, 30, 40})
F(New Integer() {})
```

<span data-ttu-id="6e6ae-306">`ParamArray` in Deklarationen von Delegat- oder Ereignisparameter können keine Parameter angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-306">`ParamArray` parameters may not be specified in delegate or event declarations.</span></span>

### <a name="event-handling"></a><span data-ttu-id="6e6ae-307">Ereignisbehandlung</span><span class="sxs-lookup"><span data-stu-id="6e6ae-307">Event Handling</span></span>

<span data-ttu-id="6e6ae-308">Methoden können deklarativ von Objekten in der Instanz oder freigegebene Variablen ausgelösten Ereignisse behandeln.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-308">Methods can declaratively handle events raised by objects in instance or shared variables.</span></span> <span data-ttu-id="6e6ae-309">Zum Verarbeiten von Ereignissen, die Deklaration einer Methode gibt die `Handles` Schlüsselwort und führt eine oder mehrere Ereignisse.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-309">To handle events, a method declaration specifies the `Handles` keyword and lists one or more events.</span></span>

```antlr
HandlesClause
    : ( 'Handles' EventHandlesList )?
    ;

EventHandlesList
    : EventMemberSpecifier ( Comma EventMemberSpecifier )*
    ;

EventMemberSpecifier
    : Identifier Period IdentifierOrKeyword
    | 'MyBase' Period IdentifierOrKeyword
    | 'MyClass' Period IdentifierOrKeyword
    | 'Me' Period IdentifierOrKeyword
    ;
```

<span data-ttu-id="6e6ae-310">Ein Ereignis in der `Handles` mithilfe von zwei Bezeichnern, die durch einen Punkt getrennten Liste angegeben:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-310">An event in the `Handles` list is specified by two identifiers separated by a period:</span></span>

* <span data-ttu-id="6e6ae-311">Der erste Bezeichner muss eine Instanz oder freigegebene Variable in der enthaltende Typ, der angibt, die `WithEvents` Modifizierer oder `MyBase` oder `MyClass` oder `Me` Schlüsselwort; andernfalls ein Kompilierungsfehler tritt auf.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-311">The first identifier must be an instance or shared variable in the containing type that specifies the `WithEvents` modifier or the `MyBase` or `MyClass` or `Me` keyword; otherwise, a compile-time error occurs.</span></span> <span data-ttu-id="6e6ae-312">Diese Variable enthält das Objekt, das die Ereignisse behandelt, die von dieser Methode ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-312">This variable contains the object that will raise the events handled by this method.</span></span>

* <span data-ttu-id="6e6ae-313">Der zweite Bezeichner muss ein Mitglied der Typ des der erste Bezeichner angeben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-313">The second identifier must specify a member of the type of the first identifier.</span></span> <span data-ttu-id="6e6ae-314">Das Element muss ein Ereignis, und freigegeben werden kann.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-314">The member must be an event, and may be shared.</span></span> <span data-ttu-id="6e6ae-315">Wenn eine freigegebene Variable für den ersten Bezeichner angegeben wird, klicken Sie dann das Ereignis freigegeben werden muss, oder ein Fehler ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-315">If a shared variable is specified for the first identifier, then the event must be shared, or an error results.</span></span>

<span data-ttu-id="6e6ae-316">Eine Handlermethode `M` gilt als einen gültigen-Ereignishandler für ein Ereignis `E` Wenn die Anweisung `AddHandler E, AddressOf M` wäre auch gültig.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-316">A handler method `M` is considered a valid event handler for an event `E` if the statement `AddHandler E, AddressOf M` would also be valid.</span></span> <span data-ttu-id="6e6ae-317">Im Gegensatz zu einer `AddHandler` -Anweisung, ermöglichen jedoch explizite Ereignishandler behandeln eines Ereignisses mit einer Methode ohne Argumente, unabhängig davon, ob strikte Semantik oder nicht verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-317">Unlike an `AddHandler` statement, however, explicit event handlers allow handling an event with a method with no arguments regardless of whether strict semantics are being used or not:</span></span>

```vb
Option Strict On

Class C1
    Event E(x As Integer)
End Class

Class C2
    withEvents C1 As New C1()

    ' Valid
    Sub M1() Handles C1.E
    End Sub

    Sub M2()
        ' Invalid
        AddHandler C1.E, AddressOf M1
    End Sub
End Class
```

<span data-ttu-id="6e6ae-318">Ein einzelnes Element mehrere übereinstimmende Ereignisse behandeln, und mehrere Methoden können ein einzelnes Ereignis zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-318">A single member can handle multiple matching events, and multiple methods may handle a single event.</span></span> <span data-ttu-id="6e6ae-319">Zugriff auf eine Methode hat keine Auswirkungen auf die Möglichkeit, Ereignisse zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-319">A method's accessibility has no effect on its ability to handle events.</span></span> <span data-ttu-id="6e6ae-320">Das folgende Beispiel zeigt, wie eine Methode Ereignisse behandeln kann:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-320">The following example shows how a method can handle events:</span></span>

```vb
Class Raiser
    Event E1()

    Sub Raise()
        RaiseEvent E1
    End Sub
End Class

Module Test
    WithEvents x As Raiser

    Sub E1Handler() Handles x.E1
        Console.WriteLine("Raised")
    End Sub

    Sub Main()
        x = New Raiser()
        x.Raise()
        x.Raise()
    End Sub
End Module
```

<span data-ttu-id="6e6ae-321">Dies wird ausgegeben:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-321">This will print out:</span></span>

```
Raised
Raised
```

<span data-ttu-id="6e6ae-322">Ein Typ erbt alle Ereignishandler, die von seinem Basistyp bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-322">A type inherits all event handlers provided by its base type.</span></span> <span data-ttu-id="6e6ae-323">Ein abgeleiteter Typ kann nicht in keiner Weise die Ereignis-Zuordnungen geändert, erbt von Basistypen, sondern fügen Sie möglicherweise zusätzliche Handler zum Ereignis.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-323">A derived type cannot in any way alter the event mappings it inherits from its base types, but may add additional handlers to the event.</span></span>


### <a name="extension-methods"></a><span data-ttu-id="6e6ae-324">Erweiterungsmethoden</span><span class="sxs-lookup"><span data-stu-id="6e6ae-324">Extension Methods</span></span>

<span data-ttu-id="6e6ae-325">Methoden können hinzugefügt werden, auf die Typen von außerhalb des Deklaration mit *Erweiterungsmethoden*.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-325">Methods can be added to types from outside of the type declaration using *extension methods*.</span></span> <span data-ttu-id="6e6ae-326">Erweiterungsmethoden sind Methoden, mit der `System.Runtime.CompilerServices.ExtensionAttribute` Attribut angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-326">Extension methods are methods with the `System.Runtime.CompilerServices.ExtensionAttribute` attribute applied to them.</span></span> <span data-ttu-id="6e6ae-327">Sie können nur im standard-Modulen deklariert werden und müssen mindestens ein Parameter gibt an, dass der Typ der Methode erweitert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-327">They can only be declared in standard modules and must have at least one parameter, which specifies the type the method extends.</span></span> <span data-ttu-id="6e6ae-328">Die folgende Erweiterungsmethode erweitert beispielsweise den Typ `String`:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-328">For example, the following extension method extends the type `String`:</span></span>

```vb
Imports System.Runtime.CompilerServices

Module StringExtensions
    <Extension> _
    Sub Print(s As String)
        Console.WriteLine(s)
    End Sub
End Module
```

<span data-ttu-id="6e6ae-329">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="6e6ae-329">__Note.__</span></span> <span data-ttu-id="6e6ae-330">Obwohl Visual Basic Erweiterungsmethoden deklariert werden, in einem Standardmodul erforderlich sind, können andere Sprachen wie c# werden in anderen Arten von Typen deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-330">Although Visual Basic requires extension methods to be declared in a standard module, other languages such as C# may allow them to be declared in other kinds of types.</span></span> <span data-ttu-id="6e6ae-331">Solange die Methoden folgen, die anderen hier beschriebenen Konventionen und die mit Typ ein offener generischer Typ ist, und kann nicht instanziiert werden, Visual Basic erkennt die Erweiterungsmethoden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-331">As long as the methods follow the other conventions outlined here and the containing type is not an open generic type and cannot be instantiated, Visual Basic will recognize the extension methods.</span></span>

<span data-ttu-id="6e6ae-332">Wenn eine Erweiterungsmethode aufgerufen wird, wird die Instanz, die, der Sie auf aufgerufen wird, wird, auf den ersten Parameter übergeben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-332">When an extension method is invoked, the instance it is being invoked on is passed to the first parameter.</span></span> <span data-ttu-id="6e6ae-333">Der erste Parameter kann nicht deklariert werden `Optional` oder `ParamArray`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-333">The first parameter cannot be declared `Optional` or `ParamArray`.</span></span> <span data-ttu-id="6e6ae-334">Der erste Parameter einer Erweiterungsmethode kann beliebigen Typs, einschließlich der einen Parameter vom Typ sein.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-334">Any type, including a type parameter, can appear as the first parameter of an extension method.</span></span> <span data-ttu-id="6e6ae-335">Die folgenden Methoden erweitern, z. B. die Typen `Integer()`, jeder Typ, der implementiert `System.Collections.Generic.IEnumerable(Of T)`, und alle Typen überhaupt:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-335">For example, the following methods extend the types `Integer()`, any type that implements `System.Collections.Generic.IEnumerable(Of T)`, and any type at all:</span></span>

```vb
Imports System.Runtime.CompilerServices

Module Extensions
    <Extension> _
    Sub PrintArray(a() As Integer)
        ...
    End Sub

    <Extension> _
    Sub PrintList(Of T)(a As IEnumerable(Of T))
        ...
    End Sub

    <Extension> _
    Sub Print(Of T)(a As T)
        ...
    End Sub
End Module
```

<span data-ttu-id="6e6ae-336">Wie im vorherige Beispiel wird gezeigt, können die Schnittstellen erweitert werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-336">As the previous example shows, interfaces can be extended.</span></span> <span data-ttu-id="6e6ae-337">Schnittstelle Erweiterungsmethoden Geben Sie die Implementierung der Methode, damit Typen, die eine Schnittstelle zu implementieren, die Erweiterungsmethoden, die nach wie vor nur definiert wurde ursprünglich von der Schnittstelle deklarierten Member implementieren.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-337">Interface extension methods supply the implementation of the method, so types that implement an interface that has extension methods defined on it still only implement the members originally declared by the interface.</span></span> <span data-ttu-id="6e6ae-338">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-338">For example:</span></span>

```vb
Imports System.Runtime.CompilerServices

Interface IAction
  Sub DoAction()
End Interface

Module IActionExtensions 
    <Extension> _
    Public Sub DoAnotherAction(i As IAction) 
        i.DoAction()
    End Sub
End Module

Class C
  Implements IAction

  Sub DoAction() Implements IAction.DoAction
    ...
  End Sub

  ' ERROR: Cannot implement extension method IAction.DoAnotherAction
  Sub DoAnotherAction() Implements IAction.DoAnotherAction
    ...
  End Sub
End Class
```

<span data-ttu-id="6e6ae-339">Erweiterungsmethoden können auch Einschränkungen für ihre Typparameter und, ebenso wie mit nicht-generische Erweiterungsmethoden Typargument abgeleitet werden kann:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-339">Extension methods can also have type constraints on their type parameters and, just as with non-extension generic methods, type argument can be inferred:</span></span>

```vb
Imports System.Runtime.CompilerServices

Module IEnumerableComparableExtensions
    <Extension> _
    Public Function Sort(Of T As IComparable(Of T))(i As IEnumerable(Of T)) _
        As IEnumerable(Of T)
        ...
    End Function
End Module
```

<span data-ttu-id="6e6ae-340">Erweiterungsmethoden können auch über implizite ausdrucksinstanzen innerhalb des Typs, der erweitert wird zugegriffen werden:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-340">Extension methods can also be accessed through implicit instance expressions within the type being extended:</span></span>

```vb
Imports System.Runtime.CompilerServices

Class C1
    Sub M1()
        Me.M2()
        M2()
    End Sub
End Class

Module C1Extensions
    <Extension>
    Sub M2(c As C1)
        ...
    End Sub
End Module
```

<span data-ttu-id="6e6ae-341">Für die Zwecke der Barrierefreiheit werden die Erweiterungsmethoden bereit, die als Mitglieder des standard-Moduls sie deklariert werden in – er besitzt keinen zusätzlichen Zugriff auf die Member des Typs, den sie über den Zugriff, die sie aufgrund ihrer Deklarationskontext haben erweitern werden auch behandelt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-341">For the purposes of accessibility, extension methods are also treated as members of the standard module they are declared in -- they have no extra access to the members of the type they are extending beyond the access they have by virtue of their declaration context.</span></span>

<span data-ttu-id="6e6ae-342">Erweiterungsmethoden sind nur verfügbar, wenn die standard modulmethode im Gültigkeitsbereich befindet.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-342">Extensions methods are only available when the standard module method is in scope.</span></span> <span data-ttu-id="6e6ae-343">Andernfalls wird der ursprüngliche Typ nicht angezeigt, die erweitert wurden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-343">Otherwise, the original type will not appear to have been extended.</span></span> <span data-ttu-id="6e6ae-344">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-344">For example:</span></span>

```vb
Imports System.Runtime.CompilerServices

Class C1
End Class

Namespace N1
    Module C1Extensions
        <Extension> _
        Sub M1(c As C1)
            ...
        End Sub
    End Module
End Namespace

Module Test
    Sub Main()
        Dim c As New C1()

        ' Error: c has no member named "M1"
        c.M1()
    End Sub
End Module
```

<span data-ttu-id="6e6ae-345">Auf einen Typ verweist, wenn nur eine Erweiterungsmethode für den Typ verfügbar ist, wird weiterhin einen Fehler während der Kompilierung erstellt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-345">Referring to a type when only an extension method on the type is available will still produce a compile-time error.</span></span>

<span data-ttu-id="6e6ae-346">Es ist wichtig zu beachten, dass Erweiterungsmethoden betrachtet werden Member des Typs in allen Kontexten, in denen Elemente, z. B. die stark typisierte gebunden sind `For Each` Muster.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-346">It is important to note that extension methods are considered to be members of the type in all contexts where members are bound, such as the strongly-typed `For Each` pattern.</span></span> <span data-ttu-id="6e6ae-347">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-347">For example:</span></span>

```vb
Imports System.Runtime.CompilerServices

Class C1
End Class

Class C1Enumerator
    ReadOnly Property Current() As C1
        Get
            ...
        End Get
    End Property

    Function MoveNext() As Boolean
        ...
    End Function
End Class

Module C1Extensions
    <Extension> _
    Function GetEnumerator(c As C1) As C1Enumerator
        ...
    End Function
End Module

Module Test
    Sub Main()
        Dim c As New C1()

        ' Valid
        For Each o As Object In c
            ...
        Next o
    End Sub
End Module
```

<span data-ttu-id="6e6ae-348">Auch können Delegaten erstellt werden, die Erweiterungsmethoden verweisen, werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-348">Delegates can also be created that refer to extension methods.</span></span> <span data-ttu-id="6e6ae-349">Daher ist der Code:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-349">Thus, the code:</span></span>

```vb
Delegate Sub D1()

Module Test
    Sub Main()
        Dim s As String = "Hello, World!"
        Dim d As D1

        d = AddressOf s.Print
        d()
    End Sub
End Module
```

<span data-ttu-id="6e6ae-350">ist ungefähr gleich ist:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-350">is roughly equivalent to:</span></span>

```vb
Delegate Sub D1()

Module Test
    Sub Main()
      Dim s As String = "Hello, World!"
      Dim d As D1

      d = CType([Delegate].CreateDelegate(GetType(D1), s, _
                GetType(StringExtensions).GetMethod("Print")), D1)
      d()
    End Sub
End Module
```

<span data-ttu-id="6e6ae-351">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="6e6ae-351">__Note.__</span></span> <span data-ttu-id="6e6ae-352">Visual Basic fügt normalerweise eine Überprüfung auf eine Instanz-Methodenaufruf, der bewirkt, dass eine `System.NullReferenceException` auftreten, wenn die Instanz die Methode aufgerufen wird `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-352">Visual Basic normally inserts a check on an instance method call that causes a `System.NullReferenceException` to occur if the instance the method is being invoked on is `Nothing`.</span></span> <span data-ttu-id="6e6ae-353">Im Fall von Erweiterungsmethoden, es gibt keine effiziente Methode, diese Überprüfung eingefügt, sodass Erweiterungsmethoden explizite Prüfung müssen `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-353">In the case of extension methods, there is no efficient way to insert this check, so extension methods will need to explicitly check for `Nothing`.</span></span> 

<span data-ttu-id="6e6ae-354">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="6e6ae-354">__Note.__</span></span> <span data-ttu-id="6e6ae-355">Wird ein Werttyp geschachtelt werden, beim Übergeben als wird eine `ByVal` Argument an einen Parameter typisiert als eine Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-355">A value type will be boxed when being passed as a `ByVal` argument to a parameter typed as an interface.</span></span>  <span data-ttu-id="6e6ae-356">Dies bedeutet, dass die Nebenwirkungen der Erweiterungsmethode eine Kopie der Struktur anstelle der ursprünglichen ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-356">This implies that side effects of the extension method will operate on a copy of the structure instead of the original.</span></span> <span data-ttu-id="6e6ae-357">Während die Sprache keine Einschränkungen für das erste Argument einer Erweiterungsmethode platziert, es empfiehlt sich, dass Erweiterungsmethoden bereit, die nicht zum Erweitern von Werttypen verwendet werden oder wenn Sie Werttypen zu erweitern, der erste Parameter übergeben wird `ByRef` um sicherzustellen, dass diese Seite Effekte werden auf den ursprünglichen Wert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-357">While the language puts no restrictions on the first argument of an extension method, it is recommended that extension methods are not used to extend value types or that when extending value types, the first parameter is passed `ByRef` to ensure that side effects operate on the original value.</span></span>

### <a name="partial-methods"></a><span data-ttu-id="6e6ae-358">Partielle Methoden</span><span class="sxs-lookup"><span data-stu-id="6e6ae-358">Partial Methods</span></span>

<span data-ttu-id="6e6ae-359">Ein *partielle Methode* ist eine Methode, die eine Signatur jedoch nicht den Text der Methode angibt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-359">A *partial method* is a method that specifies a signature but not the body of the method.</span></span> <span data-ttu-id="6e6ae-360">Der Text der Methode kann von einer anderen Deklaration der Methode mit dem gleichen Namen und eine Signatur, die sehr wahrscheinlich bereits in eine andere partielle Deklaration des Typs bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-360">The body of the method can be supplied by another method declaration with the same name and signature, most likely in another partial declaration of the type.</span></span> <span data-ttu-id="6e6ae-361">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-361">For example:</span></span>

<span data-ttu-id="6e6ae-362">a.vb:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-362">a.vb:</span></span>

```vb
' Designer generated code
Public Partial Class MyForm
    Private Partial Sub ValidateControls()
    End Sub

    Public Sub New()
        ' Initialize controls
        ...

        ValidateControls()
    End Sub    
End Class
```

<span data-ttu-id="6e6ae-363">b.vb:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-363">b.vb:</span></span>

```vb
Public Partial Class MyForm
    Public Sub ValidateControls()
        ' Validation logic goes here
        ...
    End Sub
End Class
```

<span data-ttu-id="6e6ae-364">In diesem Beispiel ist eine partielle Deklaration der Klasse `MyForm` deklariert eine partielle Methode `ValidateControls` ohne Implementierung.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-364">In this example, a partial declaration of the class `MyForm` declares a partial method `ValidateControls` with no implementation.</span></span> <span data-ttu-id="6e6ae-365">Obwohl es kein Nachrichtentext, der in der Datei angegeben ist, ruft der Konstruktor in der partiellen Deklaration der partielle Methode, an.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-365">The constructor in the partial declaration calls the partial method, even though there is no body supplied in the file.</span></span> <span data-ttu-id="6e6ae-366">Die andere partielle Deklaration `MyForm` dann stellt die Implementierung der Methode.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-366">The other partial declaration of `MyForm` then supplies the implementation of the method.</span></span>

<span data-ttu-id="6e6ae-367">Partielle Methoden können aufgerufen werden, unabhängig davon, ob ein Textkörper angegeben wurde. Wenn kein Methodentext angegeben wird, wird der Aufruf ignoriert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-367">Partial methods can be called regardless of whether a body has been supplied; if no method body is supplied, the call is ignored.</span></span> <span data-ttu-id="6e6ae-368">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-368">For example:</span></span>

```vb
Public Class C1
    Private Partial Sub M1()
    End Sub

    Public Sub New()
        ' Since no implementation is supplied, this call will not be made.
        M1()
    End Sub
End Class
```

<span data-ttu-id="6e6ae-369">Any-Ausdrücke, die als Argumente für einen partiellen Methodenaufruf übergeben werden, die ignoriert wird, sind auch ignoriert und nicht ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-369">Any expressions that are passed in as arguments to a partial method call that is ignored are ignored also and not evaluated.</span></span> <span data-ttu-id="6e6ae-370">(__Beachten.__</span><span class="sxs-lookup"><span data-stu-id="6e6ae-370">(__Note.__</span></span> <span data-ttu-id="6e6ae-371">Dies bedeutet, dass partielle Methoden sind eine sehr effiziente Möglichkeit für die Bereitstellung von Verhalten, das über zwei partielle Typen definiert ist, da die partiellen Methoden ohne Kosten haben, wenn sie nicht verwendet werden.)</span><span class="sxs-lookup"><span data-stu-id="6e6ae-371">This means that partial methods are a very efficient way of providing behavior that is defined across two partial types, since the partial methods have no cost if they are not used.)</span></span>

<span data-ttu-id="6e6ae-372">Deklaration der partiellen Methode muss deklariert werden, als `Private` und muss immer eine Unterroutine mit keine Anweisungen im Text.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-372">The partial method declaration must be declared as `Private` and must always be a subroutine with no statements in its body.</span></span> <span data-ttu-id="6e6ae-373">Partielle Methoden können nicht selbst Schnittstellenmethoden, implementieren, jedoch können die Methode, die ihren Text bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-373">Partial methods cannot themselves implement interface methods, although the method that supplies their body can.</span></span>

<span data-ttu-id="6e6ae-374">Nur eine Methode kann es sich um Text zu einer partiellen Methode bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-374">Only one method can supply a body to a partial method.</span></span> <span data-ttu-id="6e6ae-375">Eine Methode aus, geben Sie einen Text für eine partielle Methode müssen die gleiche Signatur wie die partielle Methode, die dieselben Einschränkungen für Typparameter, die gleichen deklarationsmodifizierer, und die gleichen Parameter und Typparameternamen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-375">A method supplying a body to a partial method must have the same signature as the partial method, the same constraints on any type parameters, the same declaration modifiers, and the same parameter and type parameter names.</span></span> <span data-ttu-id="6e6ae-376">Attribute für die partielle Methode und die Methode, die Text bereitstellt werden zusammengeführt, wie Attribute auf die Methoden-Parameter.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-376">Attributes on the partial method and the method that supplies its body are merged, as are any attributes on the methods' parameters.</span></span> <span data-ttu-id="6e6ae-377">Auf ähnliche Weise wird die Liste der Ereignisse, die die Methoden handhaben zusammengeführt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-377">Similarly, the list of events that the methods handle is merged.</span></span> <span data-ttu-id="6e6ae-378">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-378">For example:</span></span>

```vb
Class C1
    Event E1()
    Event E2()

    Private Partial Sub S() Handles Me.E1
    End Sub

    ' Handles both E1 and E2
    Private Sub S() Handles Me.E2
        ...
    End Sub
End Class
```

## <a name="constructors"></a><span data-ttu-id="6e6ae-379">Konstruktoren</span><span class="sxs-lookup"><span data-stu-id="6e6ae-379">Constructors</span></span>

<span data-ttu-id="6e6ae-380">*Konstruktoren* sind spezielle Methoden, die Kontrolle über die Initialisierung zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-380">*Constructors* are special methods that allow control over initialization.</span></span> <span data-ttu-id="6e6ae-381">Sie werden ausgeführt, nachdem das Programm beginnt, oder wenn eine Instanz eines Typs erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-381">They are run after the program begins or when an instance of a type is created.</span></span> <span data-ttu-id="6e6ae-382">Im Gegensatz zu anderen Membern Konstruktoren nicht geerbt werden, und führen einen Namen nicht in Deklarationsabschnitt des Typs.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-382">Unlike other members, constructors are not inherited and do not introduce a name into a type's declaration space.</span></span> <span data-ttu-id="6e6ae-383">Konstruktoren können nur von Objekt-und Arrayerstellung Ausdrücken oder von .NET Framework aufgerufen werden; Sie können nicht direkt aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-383">Constructors may only be invoked by object-creation expressions or by the .NET Framework; they may never be directly invoked.</span></span>

<span data-ttu-id="6e6ae-384">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="6e6ae-384">__Note.__</span></span> <span data-ttu-id="6e6ae-385">Konstruktoren haben die gleiche Einschränkung auf Zeile Platzierung, die Unterroutinen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-385">Constructors have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="6e6ae-386">Der Anfang-Anweisung, die End-Anweisung und die Block müssen alle am Anfang einer logischen Zeile angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-386">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

```antlr
ConstructorMemberDeclaration
    : Attributes? ConstructorModifier* 'Sub' 'New'
      ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block?
      'End' 'Sub' StatementTerminator
    ;

ConstructorModifier
    : AccessModifier
    | 'Shared'
    ;
```

### <a name="instance-constructors"></a><span data-ttu-id="6e6ae-387">Instanzkonstruktoren</span><span class="sxs-lookup"><span data-stu-id="6e6ae-387">Instance Constructors</span></span>

<span data-ttu-id="6e6ae-388">*Instanzkonstruktoren* Instanzen eines Typs initialisieren und von .NET Framework ausgeführt werden, wenn eine Instanz erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-388">*Instance constructors* initialize instances of a type and are run by the .NET Framework when an instance is created.</span></span> <span data-ttu-id="6e6ae-389">Die Parameterliste eines Konstruktors unterliegt den gleichen Regeln wie die Parameterliste einer Methode ab.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-389">The parameter list of a constructor is subject to the same rules as the parameter list of a method.</span></span> <span data-ttu-id="6e6ae-390">Instanzkonstruktoren können überladen werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-390">Instance constructors may be overloaded.</span></span>

<span data-ttu-id="6e6ae-391">Alle Konstruktoren in Verweistypen müssen es sich um einen anderen Konstruktor aufrufen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-391">All constructors in reference types must invoke another constructor.</span></span> <span data-ttu-id="6e6ae-392">Wenn der Aufruf explizit ist, muss er die erste Anweisung in den Text des Konstruktors-Methode.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-392">If the invocation is explicit, it must be the first statement in the constructor method body.</span></span> <span data-ttu-id="6e6ae-393">Die Anweisung kann entweder Aufrufen einer anderen Instanzkonstruktoren "des Typs" – z. B. `Me.New(...)` oder `MyClass.New(...)` – oder wenn es sich nicht um eine Struktur ist können sie einem Instanzenkonstruktor der Basistyp des Typs aufrufen – z. B. `MyBase.New(...)`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-393">The statement can either invoke another of the type's instance constructors -- for example, `Me.New(...)` or `MyClass.New(...)` -- or if it is not a structure it can invoke an instance constructor of the type's base type -- for example, `MyBase.New(...)`.</span></span> <span data-ttu-id="6e6ae-394">Es ist nicht zulässig für einen Konstruktor selbst aufrufen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-394">It is invalid for a constructor to invoke itself.</span></span> <span data-ttu-id="6e6ae-395">Wenn ein Konstruktor einen Aufruf an einen anderen Konstruktor lässt `MyBase.New()` ist implizit.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-395">If a constructor omits a call to another constructor, `MyBase.New()` is implicit.</span></span> <span data-ttu-id="6e6ae-396">Wenn Sie keinen parameterlosen Konstruktor vorhanden ist, tritt auf, ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-396">If there is no parameterless base type constructor, a compile-time error occurs.</span></span> <span data-ttu-id="6e6ae-397">Da `Me` gilt als nicht erstellt werden soll, bis nach dem Aufruf eines Basisklassenkonstruktors, nicht die Parameter für eine aufrufanweisung Konstruktor verweisen können `Me`, `MyClass`, oder `MyBase` implizit oder explizit.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-397">Because `Me` is not considered to be constructed until after the call to a base class constructor, the parameters to a constructor invocation statement cannot reference `Me`, `MyClass`, or `MyBase` implicitly or explicitly.</span></span>

<span data-ttu-id="6e6ae-398">Wenn ein Konstruktor für die erste Anweisung besitzt das Format `MyBase.New(...)`, der Konstruktor führt implizit die Initialisierungen, die von der Instanzvariablen, die in den Typ deklariert die Variable Initialisierer angegeben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-398">When a constructor's first statement is of the form `MyBase.New(...)`, the constructor implicitly performs the initializations specified by the variable initializers of the instance variables declared in the type.</span></span> <span data-ttu-id="6e6ae-399">Dies entspricht einer Sequenz von Zuweisungen, die ausgeführt werden, sofort nach dem Aufrufen des Konstruktors direkten Basistyp.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-399">This corresponds to a sequence of assignments that are executed immediately after invoking the direct base type constructor.</span></span> <span data-ttu-id="6e6ae-400">Diese Reihenfolge wird sichergestellt, dass alle Basisinstanz Variablen durch ihre Variableninitialisierern initialisiert werden, bevor alle Anweisungen, die Zugriff auf die Instanz ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-400">Such ordering ensures that all base instance variables are initialized by their variable initializers before any statements that have access to the instance are executed.</span></span> <span data-ttu-id="6e6ae-401">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-401">For example:</span></span>

```vb
Class A
    Protected x As Integer = 1
End Class

Class B
    Inherits A

    Private y As Integer = x

    Public Sub New()
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub
End Class
```

<span data-ttu-id="6e6ae-402">Wenn `New B()` dient zum Erstellen einer Instanz von `B`, wird die folgende Ausgabe generiert:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-402">When `New B()` is used to create an instance of `B`, the following output is produced:</span></span>

```
x = 1, y = 1
```

<span data-ttu-id="6e6ae-403">Der Wert des `y` ist `1` da die Variableninitialisierer ausgeführt wird, nach dem Aufruf des Basisklassenkonstruktors.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-403">The value of `y` is `1` because the variable initializer is executed after the base class constructor is invoked.</span></span> <span data-ttu-id="6e6ae-404">Variableninitialisierern werden in der Reihenfolge im Text ausgeführt, die sie in der Typdeklaration angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-404">Variable initializers are executed in the textual order they appear in the type declaration.</span></span>

<span data-ttu-id="6e6ae-405">Wenn ein Typ deklariert nur `Private` Konstruktoren, es ist nicht möglich im Allgemeinen für andere Typen von dem Typ abgeleitet sind, oder erstellen Instanzen des Typs; die einzige Ausnahme ist der Typ geschachtelten Typen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-405">When a type declares only `Private` constructors, it is not possible in general for other types to derive from the type or create instances of the type; the only exception is types nested within the type.</span></span> <span data-ttu-id="6e6ae-406">`Private` Konstruktoren werden am häufigsten in Typen, die nur `Shared` Member.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-406">`Private` constructors are commonly used in types that contain only `Shared` members.</span></span>

<span data-ttu-id="6e6ae-407">Wenn ein Typ keine Instanz-Deklarationen enthält, wird automatisch ein Standardkonstruktor bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-407">If a type contains no instance constructor declarations, a default constructor is automatically provided.</span></span> <span data-ttu-id="6e6ae-408">Die Standard-Konstruktor ruft einfach den parameterlosen Konstruktor der direkten Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-408">The default constructor simply invokes the parameterless constructor of the direct base type.</span></span> <span data-ttu-id="6e6ae-409">Wenn direkte Basistyp nicht über einen zugänglichen parameterlosen Konstruktor verfügt, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-409">If the direct base type does not have an accessible parameterless constructor, a compile-time error occurs.</span></span> <span data-ttu-id="6e6ae-410">Wird von der deklarierten Zugriffstyp für den Standardkonstruktor `Public` , wenn der Typ ist `MustInherit`, in diesem Fall der Standardkonstruktor wird `Protected`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-410">The declared access type for the default constructor is `Public` unless the type is `MustInherit`, in which case the default constructor is `Protected`.</span></span>

<span data-ttu-id="6e6ae-411">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="6e6ae-411">__Note.__</span></span> <span data-ttu-id="6e6ae-412">Der Standardzugriff für eine `MustInherit` Standardkonstruktor des Typs ist `Protected` da `MustInherit` Klassen können nicht direkt erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-412">The default access for a `MustInherit` type's default constructor is `Protected` because `MustInherit` classes cannot be created directly.</span></span> <span data-ttu-id="6e6ae-413">Es gibt also keinen Sinn, sodass den Standardkonstruktor `Public`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-413">So there is no point in making the default constructor `Public`.</span></span>

<span data-ttu-id="6e6ae-414">Im folgenden Beispiel wird ein Standardkonstruktor bereitgestellt, da die Klasse keine Deklarationen enthält:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-414">In the following example a default constructor is provided because the class contains no constructor declarations:</span></span>

```vb
Class Message
    Dim sender As Object
    Dim text As String
End Class
```

<span data-ttu-id="6e6ae-415">Daher ist das Beispiel genau äquivalent zu folgendem:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-415">Thus, the example is precisely equivalent to the following:</span></span>

```vb
Class Message
    Dim sender As Object
    Dim text As String

    Sub New()
    End Sub
End Class
```

<span data-ttu-id="6e6ae-416">Standardkonstruktoren, die in einem Designer ausgegeben werden generiert, mit dem Attribut markierten Klasse `Microsoft.VisualBasic.CompilerServices.DesignerGeneratedAttribute` rufen Sie die Methode `Sub InitializeComponent()`, sofern es vorhanden, nach dem Aufruf des Basiskonstruktors ist.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-416">Default constructors that are emitted into a designer generated class marked with the attribute `Microsoft.VisualBasic.CompilerServices.DesignerGeneratedAttribute` will call the method `Sub InitializeComponent()`, if it exists, after the call to the base constructor.</span></span> <span data-ttu-id="6e6ae-417">(__Beachten.__</span><span class="sxs-lookup"><span data-stu-id="6e6ae-417">(__Note.__</span></span> <span data-ttu-id="6e6ae-418">Dadurch können Designer generierte Dateien, z. B. den im Windows Forms-Designer, um den Konstruktor in die Designer-Datei zu unterdrücken.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-418">This allows designer generated files, such as those created by the WinForms designer, to omit the constructor in the designer file.</span></span> <span data-ttu-id="6e6ae-419">Dies ermöglicht den Programmierer, die sie selbst, geben Sie bei Bedarf wechselseitig.)</span><span class="sxs-lookup"><span data-stu-id="6e6ae-419">This enables the programmer to specify it themselves, if they so choose.)</span></span>

### <a name="shared-constructors"></a><span data-ttu-id="6e6ae-420">Shared-Konstruktoren</span><span class="sxs-lookup"><span data-stu-id="6e6ae-420">Shared Constructors</span></span>

<span data-ttu-id="6e6ae-421">*Shared-Konstruktoren* Initialisieren eines Typs gemeinsam genutzt, Variablen, nachdem das Programm wird ausgeführt, aber vor verweisen auf ein Member des Typs ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-421">*Shared constructors* initialize a type's shared variables; they are run after the program begins executing, but before any references to a member of the type.</span></span> <span data-ttu-id="6e6ae-422">Gibt an, ein shared-Konstruktor die `Shared` Modifizierer, sofern dies nicht in einem Standardmodul in diesem Fall die `Shared` Modifizierer wird impliziert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-422">A shared constructor specifies the `Shared` modifier, unless it is in a standard module in which case the `Shared` modifier is implied.</span></span>

<span data-ttu-id="6e6ae-423">Anders als Instanzkonstruktoren shared-Konstruktoren verfügen über implizite öffentlichen Zugriff, darf keine Parameter, und keine anderen Konstruktoren aufgerufen werden können.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-423">Unlike instance constructors, shared constructors have implicit public access, have no parameters, and may not call other constructors.</span></span> <span data-ttu-id="6e6ae-424">Vor der ersten Anweisung in einem freigegebenen Konstruktor führt der gemeinsam genutzte Konstruktor implizit die Initialisierungen, die gemäß der Variableninitialisierern einzelne shared-Variable, die in den Typ deklariert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-424">Before the first statement in a shared constructor, the shared constructor implicitly performs the initializations specified by the variable initializers of the shared variables declared in the type.</span></span> <span data-ttu-id="6e6ae-425">Dies entspricht einer Sequenz von Zuweisungen, die bei der Eingabe an den Konstruktor sofort ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-425">This corresponds to a sequence of assignments that are executed immediately upon entry to the constructor.</span></span> <span data-ttu-id="6e6ae-426">Die Variable Initialisierer werden in der Reihenfolge im Text ausgeführt, die sie in der Typdeklaration angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-426">The variable initializers are executed in the textual order they appear in the type declaration.</span></span>

<span data-ttu-id="6e6ae-427">Das folgende Beispiel zeigt eine `Employee` Klasse mit einem shared-Konstruktor, der eine freigegebene Variable initialisiert:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-427">The following example shows an `Employee` class with a shared constructor that initializes a shared variable:</span></span>

```vb
Imports System.Data

Class Employee
    Private Shared ds As DataSet

    Shared Sub New()
        ds = New DataSet()
    End Sub

    Public Name As String
    Public Salary As Decimal
End Class
```

<span data-ttu-id="6e6ae-428">Für jede geschlossener generischer Typ ist ein separater freigegebener Konstruktor vorhanden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-428">A separate shared constructor exists for each closed generic type.</span></span> <span data-ttu-id="6e6ae-429">Da der gemeinsam genutzte Konstruktor ausgeführt wird, genau einmal für jeden Typ geschlossen, die es ist eine bequeme Möglichkeit, Überprüfungen zur Laufzeit für den Typparameter zu erzwingen, die zum Zeitpunkt der Kompilierung über die Einschränkungen nicht überprüft werden kann.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-429">Because the shared constructor is executed exactly once for each closed type, it is a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile-time via constraints.</span></span> <span data-ttu-id="6e6ae-430">Beispielsweise verwendet der folgende Typ einen shared-Konstruktor erzwingen, dass der Typparameter `Integer` oder `Double`:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-430">For example, the following type uses a shared constructor to enforce that the type parameter is `Integer` or `Double`:</span></span>

```vb
Class EnumHolder(Of T)
    Shared Sub New() 
        If Not GetType(T).IsEnum() Then
            Throw New ArgumentException("T must be an enumerated type.")
        End If
    End Sub
End Class
```

<span data-ttu-id="6e6ae-431">Genau bei shared-Konstruktoren ausgeführt werden ist größtenteils hängt von der Implementierung, obwohl mehrere Garantien bereitgestellt werden, wenn ein shared-Konstruktor explizit definiert ist:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-431">Exactly when shared constructors are run is mostly implementation dependent, though several guarantees are provided if a shared constructor is explicitly defined:</span></span>

* <span data-ttu-id="6e6ae-432">Shared-Konstruktoren werden vor den ersten Zugriff auf ein statisches Feld des Typs ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-432">Shared constructors are run before the first access to any static field of the type.</span></span>

* <span data-ttu-id="6e6ae-433">Shared-Konstruktoren werden vor den ersten Aufruf einer statischen Methode des Typs ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-433">Shared constructors are run before the first invocation of any static method of the type.</span></span>

* <span data-ttu-id="6e6ae-434">Shared-Konstruktoren werden vor den ersten Aufruf des Konstruktors für den Typ ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-434">Shared constructors are run before the first invocation of any constructor for the type.</span></span>

<span data-ttu-id="6e6ae-435">Die oben genannten Garantien gelten nicht in der Situation, in denen ein shared-Konstruktor implizit für freigegebene Initialisierer erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-435">The above guarantees do not apply in the situation where a shared constructor is implicitly created for shared initializers.</span></span> <span data-ttu-id="6e6ae-436">Die Ausgabe aus dem folgenden Beispiel ist unsicher, da die genaue Reihenfolge der laden und aus diesem Grund der gemeinsam genutzte Konstruktor Ausführung ist nicht definiert:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-436">The output from the following example is uncertain, because the exact ordering of loading and therefore of shared constructor execution is not defined:</span></span>

```vb
Module Test
    Sub Main()
        A.F()
        B.F()
    End Sub
End Module

Class A
    Shared Sub New()
        Console.WriteLine("Init A")
    End Sub

    Public Shared Sub F()
        Console.WriteLine("A.F")
    End Sub
End Class

Class B
    Shared Sub New()
        Console.WriteLine("Init B")
    End Sub

    Public Shared Sub F()
        Console.WriteLine("B.F")
    End Sub
End Class
```

<span data-ttu-id="6e6ae-437">Die Ausgabe kann eine der folgenden sein:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-437">The output could be either of the following:</span></span>

```
Init A
A.F
Init B
B.F
```

<span data-ttu-id="6e6ae-438">oder</span><span class="sxs-lookup"><span data-stu-id="6e6ae-438">or</span></span>

```
Init B
Init A
A.F
B.F
```

<span data-ttu-id="6e6ae-439">Im Gegensatz dazu wird im folgende Beispiel vorhersagbare Ausgabe erzeugt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-439">By contrast, the following example produces predictable output.</span></span> <span data-ttu-id="6e6ae-440">Beachten Sie, dass die `Shared` Konstruktor für die Klasse `A` nie ausgeführt wird, obwohl Klasse `B` werden von dieser abgeleitet:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-440">Note that the `Shared` constructor for the class `A` never executes, even though class `B` derives from it:</span></span>

```vb
Module Test
    Sub Main()
        B.G()
    End Sub
End Module

Class A
    Shared Sub New()
        Console.WriteLine("Init A")
    End Sub
End Class

Class B
    Inherits A

    Shared Sub New()
        Console.WriteLine("Init B")
    End Sub

    Public Shared Sub G()
        Console.WriteLine("B.G")
    End Sub
End Class
```

<span data-ttu-id="6e6ae-441">Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-441">The output is:</span></span>

```
Init B
B.G
```

<span data-ttu-id="6e6ae-442">Es ist auch möglich, um zirkuläre Abhängigkeiten zu erstellen, mit denen `Shared` Variablen mit Variableninitialisierern an, die in ihrer standardmäßigen beachtet werden Wert, Status, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-442">It is also possible to construct circular dependencies that allow `Shared` variables with variable initializers to be observed in their default value state, as in the following example:</span></span>

```vb
Class A
    Public Shared X As Integer = B.Y + 1
End Class

Class B
    Public Shared Y As Integer = A.X + 1

    Shared Sub Main()
        Console.WriteLine("X = " & A.X & ", Y = " & B.Y)
    End Sub
End Class
```

<span data-ttu-id="6e6ae-443">Dies erzeugt die Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-443">This produces the output:</span></span>

```
X = 1, Y = 2
```

<span data-ttu-id="6e6ae-444">Zum Ausführen der `Main` -Methode, das System zuerst lädt Klasse `B`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-444">To execute the `Main` method, the system first loads class `B`.</span></span> <span data-ttu-id="6e6ae-445">Die `Shared` Konstruktor der Klasse `B` wird zum Berechnen des Anfangswert des `Y`, bewirkt, dass der rekursiv Klasse `A` geladen werden, da der Wert des `A.X` verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-445">The `Shared` constructor of class `B` proceeds to compute the initial value of `Y`, which recursively causes class `A` to be loaded because the value of `A.X` is referenced.</span></span> <span data-ttu-id="6e6ae-446">Die `Shared` Konstruktor der Klasse `A` wiederum wird zum Berechnen des Anfangswert des `X`, und dies der Fall ist, ruft der *Standard* Wert `Y`, d.h. 0 (null).</span><span class="sxs-lookup"><span data-stu-id="6e6ae-446">The `Shared` constructor of class `A` in turn proceeds to compute the initial value of `X`, and in doing so fetches the *default* value of `Y`, which is zero.</span></span> <span data-ttu-id="6e6ae-447">`A.X` wird so initialisiert `1`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-447">`A.X` is thus initialized to `1`.</span></span> <span data-ttu-id="6e6ae-448">Die während des Ladevorgangs `A` dann abgeschlossen, und die Berechnung des Anfangswerts der `Y`, wird das Ergebnis der `2`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-448">The process of loading `A` then completes, returning to the calculation of the initial value of `Y`, the result of which becomes `2`.</span></span>

<span data-ttu-id="6e6ae-449">Mussten die `Main` Methode stattdessen wurde gefunden in Klasse `A`, das Beispiel würde die folgende Ausgabe erzeugt haben:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-449">Had the `Main` method instead been located in class `A`, the example would have produced the following output:</span></span>

```
X = 2, Y = 1
```

<span data-ttu-id="6e6ae-450">Vermeiden von Zirkelbezügen in `Shared` Variableninitialisierern, da sie sich im Allgemeinen unmöglich, um zu bestimmen, die Reihenfolge, in die Klassen, die solche Verweise werden geladen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-450">Avoid circular references in `Shared` variable initializers since it is generally impossible to determine the order in which classes containing such references are loaded.</span></span>

## <a name="events"></a><span data-ttu-id="6e6ae-451">Ereignisse</span><span class="sxs-lookup"><span data-stu-id="6e6ae-451">Events</span></span>

<span data-ttu-id="6e6ae-452">Ereignisse werden verwendet, um Code eines bestimmten Auftretens zu benachrichtigen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-452">Events are used to notify code of a particular occurrence.</span></span> <span data-ttu-id="6e6ae-453">Eine Ereignisdeklaration besteht aus einem Bezeichner, der entweder einen Delegattyp aufweisen oder einer Parameterliste, und eine optionale `Implements` Klausel.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-453">An event declaration consists of an identifier, either a delegate type or a parameter list, and an optional `Implements` clause.</span></span>

```antlr
EventMemberDeclaration
    : RegularEventMemberDeclaration
    | CustomEventMemberDeclaration
    ;

RegularEventMemberDeclaration
    : Attributes? EventModifiers* 'Event'
      Identifier ParametersOrType ImplementsClause? StatementTerminator
    ;

InterfaceEventMemberDeclaration
    : Attributes? InterfaceEventModifiers* 'Event'
      Identifier ParametersOrType StatementTerminator
    ;

ParametersOrType
    : ( OpenParenthesis ParameterList? CloseParenthesis )?
    | 'As' NonArrayTypeName
    ;

EventModifiers
    : AccessModifier
    | 'Shadows'
    | 'Shared'
    ;

InterfaceEventModifiers
    : 'Shadows'
    ;
```

<span data-ttu-id="6e6ae-454">Wenn ein Delegattyp angegeben wird, kann der Delegattyp einen Rückgabetyp keine.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-454">If a delegate type is specified, the delegate type may not have a return type.</span></span> <span data-ttu-id="6e6ae-455">Wenn eine Liste von Parametern angegeben wird, es darf keine `Optional` oder `ParamArray` Parameter.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-455">If a parameter list is specified, it may not contain `Optional` or `ParamArray` parameters.</span></span> <span data-ttu-id="6e6ae-456">Die Zugriffsdomäne des Parametertypen und/oder der Delegattyp muss identisch oder eine Obermenge der, die Zugriffsdomäne des Ereignisses.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-456">The accessibility domain of the parameter types and/or delegate type must be the same as, or a superset of, the accessibility domain of the event itself.</span></span> <span data-ttu-id="6e6ae-457">Ereignisse können gemeinsam genutzt werden, durch Angabe der `Shared` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-457">Events may be shared by specifying the `Shared` modifier.</span></span>

<span data-ttu-id="6e6ae-458">Zusätzlich zu den Namen des Members des Typs Deklarationsabschnitt hinzugefügt wird eine Ereignisdeklaration implizit mehrere andere Member deklariert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-458">In addition to the member name added to the type's declaration space, an event declaration implicitly declares several other members.</span></span> <span data-ttu-id="6e6ae-459">Ein Ereignis mit dem Namen `X`, werden die folgenden Elemente zum Deklarationsabschnitt hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-459">Given an event named `X`, the following members are added to the declaration space:</span></span>

* <span data-ttu-id="6e6ae-460">Ist die Form der Deklaration einer Methodendeklaration, eine geschachtelte Delegatklasse mit dem Namen `XEventHandler` eingeführt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-460">If the form of the declaration is a method declaration, a nested delegate class named `XEventHandler` is introduced.</span></span> <span data-ttu-id="6e6ae-461">Die geschachtelte Delegate-Klasse die Deklaration der Methode entspricht, und hat den gleichen Zugriff wie das Ereignis.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-461">The nested delegate class matches the method declaration and has the same accessibility as the event.</span></span> <span data-ttu-id="6e6ae-462">Die Attribute in der Parameterliste gelten für die Parameter, der die Delegate-Klasse.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-462">The attributes in the parameter list apply to the parameters of the delegate class.</span></span>

* <span data-ttu-id="6e6ae-463">Ein `Private` Instanzvariable eingegeben wird, wie der Delegat, mit der Bezeichnung `XEvent`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-463">A `Private` instance variable typed as the delegate, named `XEvent`.</span></span>

* <span data-ttu-id="6e6ae-464">Zwei Methoden namens `add_X` und `remove_X` die kann nicht aufgerufen, überschrieben oder überladen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-464">Two methods named `add_X` and `remove_X` which cannot be invoked, overridden or overloaded.</span></span>

<span data-ttu-id="6e6ae-465">Wenn ein Typ versucht, einen Namen zu deklarieren, die einen der oben genannten Namen entspricht, ein Fehler während der Kompilierung ausgegeben, und die implizite `add_X` und `remove_X` Deklarationen werden zum Zweck der Bindungen ignoriert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-465">If a type attempts to declare a name that matches one of the above names, a compile-time error will result, and the implicit `add_X` and `remove_X` declarations are ignored for the purposes of name binding.</span></span> <span data-ttu-id="6e6ae-466">Es ist nicht überschrieben oder überladen keines der Elemente eingeführt, obwohl es möglich, diese in abgeleiteten Typen zu überschatten.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-466">It is not possible to override or overload any of the introduced members, although it is possible to shadow them in derived types.</span></span> <span data-ttu-id="6e6ae-467">Beispielsweise werden in der Klassendeklaration</span><span class="sxs-lookup"><span data-stu-id="6e6ae-467">For example, the class declaration</span></span>

```vb
Class Raiser
    Public Event Constructed(i As Integer)
End Class
```

<span data-ttu-id="6e6ae-468">Die folgende Deklaration entspricht</span><span class="sxs-lookup"><span data-stu-id="6e6ae-468">is equivalent to the following declaration</span></span>

```vb
Class Raiser
    Public Delegate Sub ConstructedEventHandler(i As Integer)

    Protected ConstructedEvent As ConstructedEventHandler

    Public Sub add_Constructed(d As ConstructedEventHandler)
        ConstructedEvent = _
            CType( _
                [Delegate].Combine(ConstructedEvent, d), _
                    Raiser.ConstructedEventHandler)
    End Sub

    Public Sub remove_Constructed(d As ConstructedEventHandler)
        ConstructedEvent = _
            CType( _
                [Delegate].Remove(ConstructedEvent, d), _
                    Raiser.ConstructedEventHandler)
    End Sub
End Class
```

<span data-ttu-id="6e6ae-469">Deklarieren eines Ereignisses ohne einen Delegattyp ist die einfachste und möglichst kompakte Syntax, jedoch hat den Nachteil, dass einen neuer Delegattyp für jedes Ereignis zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-469">Declaring an event without specifying a delegate type is the simplest and most compact syntax, but has the disadvantage of declaring a new delegate type for each event.</span></span> <span data-ttu-id="6e6ae-470">Beispielsweise werden im folgenden Beispiel drei verborgene Delegattypen erstellt, obwohl alle drei Ereignisse mit derselben Parameterliste haben:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-470">For example, in the following example, three hidden delegate types are created, even though all three events have the same parameter list:</span></span>

```vb
Public Class Button
    Public Event Click(sender As Object, e As EventArgs)
    Public Event DoubleClick(sender As Object, e As EventArgs)
    Public Event RightClick(sender As Object, e As EventArgs)
End Class
```

<span data-ttu-id="6e6ae-471">Im folgenden Beispiel verwenden Sie die Ereignisse einfach denselben Delegaten `EventHandler`:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-471">In the following example, the events simply use the same delegate, `EventHandler`:</span></span>

```vb
Public Delegate Sub EventHandler(sender As Object, e As EventArgs)

Public Class Button
    Public Event Click As EventHandler
    Public Event DoubleClick As EventHandler
    Public Event RightClick As EventHandler
End Class
```

<span data-ttu-id="6e6ae-472">Ereignisse können auf zwei Arten bearbeitet werden: statisch oder dynamisch.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-472">Events can be handled in one of two ways: statically or dynamically.</span></span> <span data-ttu-id="6e6ae-473">Statische Behandlung von Ereignissen ist einfacher und erfordert lediglich eine `WithEvents` Variable und ein `Handles` Klausel.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-473">Statically handling events is simpler and only requires a `WithEvents` variable and a `Handles` clause.</span></span> <span data-ttu-id="6e6ae-474">Im folgenden Beispiel Klasse `Form1` statisch behandelt das Ereignis `Click` Objekts `Button`:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-474">In the following example, class `Form1` statically handles the event `Click` of object `Button`:</span></span>

```vb
Public Class Form1
    Public WithEvents Button1 As New Button()

    Public Sub Button1_Click(sender As Object, e As EventArgs) _
           Handles Button1.Click
        Console.WriteLine("Button1 was clicked!")
    End Sub
End Class
```

<span data-ttu-id="6e6ae-475">Behandeln von Ereignissen ist komplexer, da das Ereignis muss explizit verbunden und die Verbindung im Code getrennt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-475">Dynamically handling events is more complex because the event must be explicitly connected and disconnected to in code.</span></span> <span data-ttu-id="6e6ae-476">Die Anweisung `AddHandler` Fügt einen Handler für ein Ereignis, und die Anweisung `RemoveHandler` entfernt einen Handler für ein Ereignis.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-476">The statement `AddHandler` adds a handler for an event, and the statement `RemoveHandler` removes a handler for an event.</span></span> <span data-ttu-id="6e6ae-477">Das nächste Beispiel zeigt eine Klasse `Form1` , addiert `Button1_Click` als Ereignishandler für `Button1`des `Click` Ereignis:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-477">The next example shows a class `Form1` that adds `Button1_Click` as an event handler for `Button1`'s `Click` event:</span></span>

```vb
Public Class Form1
    Public Sub New()
        ' Add Button1_Click as an event handler for Button1's Click event.
        AddHandler Button1.Click, AddressOf Button1_Click
    End Sub 

    Private Button1 As Button = New Button()

    Sub Button1_Click(sender As Object, e As EventArgs)
        Console.WriteLine("Button1 was clicked!")
    End Sub

    Public Sub Disconnect()
        RemoveHandler Button1.Click, AddressOf Button1_Click
    End Sub 
End Class
```

<span data-ttu-id="6e6ae-478">In der Methode `Disconnect`, wird der Ereignishandler entfernt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-478">In method `Disconnect`, the event handler is removed.</span></span>


### <a name="custom-events"></a><span data-ttu-id="6e6ae-479">Benutzerdefinierte Ereignisse</span><span class="sxs-lookup"><span data-stu-id="6e6ae-479">Custom Events</span></span>

<span data-ttu-id="6e6ae-480">Wie im vorherigen Abschnitt erläutert wird, definieren Sie Ereignisdeklarationen implizit ein Feld, eine `add_` -Methode, und ein `remove_` Methode, die verwendet werden, um zu verfolgen-Ereignishandler.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-480">As discussed in the previous section, event declarations implicitly define a field, an `add_` method, and a `remove_` method that are used to keep track of event handlers.</span></span> <span data-ttu-id="6e6ae-481">In einigen Fällen kann jedoch sie benutzerdefinierten Code bereitstellen, für die nachverfolgung von Ereignishandlern wünschenswert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-481">In some situations, however, it may be desirable to provide custom code for tracking event handlers.</span></span> <span data-ttu-id="6e6ae-482">Wenn eine Klasse vierzig Ereignisse definiert, von denen nur wenige je anstelle von 40 Felder zum Nachverfolgen einer Hashtabelle verarbeitet werden, können der Handler für jedes Ereignis z. B. effizienter sein.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-482">For example, if a class defines forty events of which only a few will ever be handled, using a hash table instead of forty fields to track the handlers for each event may be more efficient.</span></span> <span data-ttu-id="6e6ae-483">*Benutzerdefinierte Ereignisse* ermöglichen die `add_X` und `remove_X` Methoden, um explizit definiert werden, für die benutzerdefinierten Speicher für Ereignishandler ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-483">*Custom events* allow the `add_X` and `remove_X` methods to be defined explicitly, which enables custom storage for event handlers.</span></span>

<span data-ttu-id="6e6ae-484">Benutzerdefinierte Ereignisse deklariert werden, auf die gleiche Weise, dass Ereignisse, die angeben, einen Delegattyp, mit der Ausnahme deklariert werden, die das Schlüsselwort `Custom` muss vor stehen die `Event` Schlüsselwort.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-484">Custom events are declared in the same way that events that specify a delegate type are declared, with the exception that the keyword `Custom` must precede the `Event` keyword.</span></span> <span data-ttu-id="6e6ae-485">Eine benutzerdefinierte Ereignisdeklaration enthält drei Deklarationen: ein `AddHandler` Deklaration einer `RemoveHandler` Deklaration und ein `RaiseEvent` Deklaration.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-485">A custom event declaration contains three declarations: an `AddHandler` declaration, a `RemoveHandler` declaration and a `RaiseEvent` declaration.</span></span> <span data-ttu-id="6e6ae-486">Keiner der Deklarationen können Modifizierer, verfügt zwar über Attribute verfügen können.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-486">None of the declarations can have any modifiers, although they can have attributes.</span></span>

```antlr
CustomEventMemberDeclaration
    : Attributes? EventModifiers* 'Custom' 'Event'
      Identifier 'As' TypeName ImplementsClause? StatementTerminator
      EventAccessorDeclaration+
      'End' 'Event' StatementTerminator
    ;

EventAccessorDeclaration
    : AddHandlerDeclaration
    | RemoveHandlerDeclaration
    | RaiseEventDeclaration
    ;

AddHandlerDeclaration
    : Attributes? 'AddHandler'
      OpenParenthesis ParameterList CloseParenthesis LineTerminator
      Block?
      'End' 'AddHandler' StatementTerminator
    ;

RemoveHandlerDeclaration
    : Attributes? 'RemoveHandler'
      OpenParenthesis ParameterList CloseParenthesis LineTerminator
      Block?
      'End' 'RemoveHandler' StatementTerminator
    ;

RaiseEventDeclaration
    : Attributes? 'RaiseEvent'
      OpenParenthesis ParameterList CloseParenthesis LineTerminator
      Block?
      'End' 'RaiseEvent' StatementTerminator
    ;
```

<span data-ttu-id="6e6ae-487">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-487">For example:</span></span>

```vb
Class Test
    Private Handlers As EventHandler

    Public Custom Event TestEvent As EventHandler
        AddHandler(value As EventHandler)
            Handlers = CType([Delegate].Combine(Handlers, value), _
                EventHandler)
        End AddHandler

        RemoveHandler(value as EventHandler)
            Handlers = CType([Delegate].Remove(Handlers, value), _
                EventHandler)
        End RemoveHandler

        RaiseEvent(sender As Object, e As EventArgs)
            Dim TempHandlers As EventHandler = Handlers

            If TempHandlers IsNot Nothing Then
                TempHandlers(sender, e)
            End If
        End RaiseEvent
    End Event
End Class
```

<span data-ttu-id="6e6ae-488">Die `AddHandler` und `RemoveHandler` Deklaration nehmen Sie an einer `ByVal` -Parameter, der von den Delegattyp des Ereignisses sein muss.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-488">The `AddHandler` and `RemoveHandler` declaration take one `ByVal` parameter, which must be of the delegate type of the event.</span></span> <span data-ttu-id="6e6ae-489">Wenn ein `AddHandler` oder `RemoveHandler` -Anweisung ausgeführt wird (oder ein `Handles` -Klausel automatisch behandelt ein Ereignis), wird die entsprechende Deklaration aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-489">When an `AddHandler` or `RemoveHandler` statement is executed (or a `Handles` clause automatically handles an event), the corresponding declaration will be called.</span></span> <span data-ttu-id="6e6ae-490">Die `RaiseEvent` Deklaration weist die gleichen Parameter wie der Ereignisdelegat und wird aufgerufen, wenn eine `RaiseEvent` -Anweisung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-490">The `RaiseEvent` declaration takes the same parameters as the event delegate and will be called when a `RaiseEvent` statement is executed.</span></span> <span data-ttu-id="6e6ae-491">Alle Deklarationen müssen bereitgestellt werden und gelten als Unterroutinen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-491">All of the declarations must be provided and are considered to be subroutines.</span></span>

<span data-ttu-id="6e6ae-492">Beachten Sie, dass `AddHandler`, `RemoveHandler` und `RaiseEvent` Deklarationen haben die gleiche Einschränkung Zeile Platzierung, die Unterroutinen verfügen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-492">Note that `AddHandler`, `RemoveHandler` and `RaiseEvent` declarations have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="6e6ae-493">Der Anfang-Anweisung, die End-Anweisung und die Block müssen alle am Anfang einer logischen Zeile angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-493">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

<span data-ttu-id="6e6ae-494">Zusätzlich zu den Namen des Members des Typs Deklarationsabschnitt hinzugefügt wird eine benutzerdefinierte Ereignisdeklaration implizit mehrere andere Member deklariert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-494">In addition to the member name added to the type's declaration space, a custom event declaration implicitly declares several other members.</span></span> <span data-ttu-id="6e6ae-495">Ein Ereignis mit dem Namen `X`, werden die folgenden Elemente zum Deklarationsabschnitt hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-495">Given an event named `X`, the following members are added to the declaration space:</span></span>

* <span data-ttu-id="6e6ae-496">Eine Methode namens `add_X`, entspricht die `AddHandler` Deklaration.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-496">A method named `add_X`, corresponding to the `AddHandler` declaration.</span></span>

* <span data-ttu-id="6e6ae-497">Eine Methode namens `remove_X`, entspricht die `RemoveHandler` Deklaration.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-497">A method named `remove_X`, corresponding to the `RemoveHandler` declaration.</span></span>

* <span data-ttu-id="6e6ae-498">Eine Methode namens `fire_X`, entspricht die `RaiseEvent` Deklaration.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-498">A method named `fire_X`, corresponding to the `RaiseEvent` declaration.</span></span>

<span data-ttu-id="6e6ae-499">Wenn ein Typ versucht, einen Namen zu deklarieren, der einen der oben genannten Namen entspricht, ein Fehler während der Kompilierung führt und die implizite Deklarationen werden zum Zweck der Bindungen ignoriert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-499">If a type attempts to declare a name that matches one of the above names, a compile-time error will result, and the implicit declarations are all ignored for the purposes of name binding.</span></span> <span data-ttu-id="6e6ae-500">Es ist nicht überschrieben oder überladen keines der Elemente eingeführt, obwohl es möglich, diese in abgeleiteten Typen zu überschatten.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-500">It is not possible to override or overload any of the introduced members, although it is possible to shadow them in derived types.</span></span>

<span data-ttu-id="6e6ae-501">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="6e6ae-501">__Note.__</span></span> <span data-ttu-id="6e6ae-502">`Custom` ist ein reserviertes Wort.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-502">`Custom` is not a reserved word.</span></span>

#### <a name="custom-events-in-winrt-assemblies"></a><span data-ttu-id="6e6ae-503">Benutzerdefinierte Ereignisse in WinRT-Assemblys</span><span class="sxs-lookup"><span data-stu-id="6e6ae-503">Custom events in WinRT assemblies</span></span>

<span data-ttu-id="6e6ae-504">Ab Microsoft Visual Basic 11.0, Ereignisse, die in einer Datei deklariert, die mit kompiliert `/target:winmdobj`, oder in einer Schnittstelle in eine solche Datei deklariert und implementiert dann an einem anderen Ort, ein wenig anders behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-504">As of Microsoft Visual Basic 11.0, events declared in a file compiled with `/target:winmdobj`, or declared in an interface in such a file and then implemented elsewhere, are treated a little differently.</span></span>

* <span data-ttu-id="6e6ae-505">Externe Tools verwendet, um die Winmd erstellen lässt in der Regel nur für bestimmte Delegattypen wie z. B. `System.EventHandler(Of T)` oder `System.TypedEventHandle(Of T, U)`, und nicht zu, wenn andere Benutzer.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-505">External tools used to build the winmd will typically allow only certain delegate types such as `System.EventHandler(Of T)` or `System.TypedEventHandle(Of T, U)`, and will disallow others.</span></span>

* <span data-ttu-id="6e6ae-506">Die `XEvent` Feld weist den Typ `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationTokenTable(Of T)` , in denen `T` ist der Delegattyp.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-506">The `XEvent` field has type `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationTokenTable(Of T)` where `T` is the delegate type.</span></span>

* <span data-ttu-id="6e6ae-507">Der AddHandler-Accessor gibt einen `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationToken`, und die RemoveHandler-Accessor nimmt einen einzelnen Parameter desselben Typs.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-507">The AddHandler accessor returns a `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationToken`, and the RemoveHandler accessor takes a single parameter of the same type.</span></span>

<span data-ttu-id="6e6ae-508">Hier ist ein Beispiel für solche ein benutzerdefiniertes Ereignis aus.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-508">Here is an example of such a custom event.</span></span>

```vb
Imports System.Runtime.InteropServices.WindowsRuntime

Public NotInheritable Class ClassInWinMD
    Private XEvent As EventRegistrationTokenTable(Of EventHandler(Of Integer))

    Public Custom Event X As EventHandler(Of Integer)
        AddHandler(handler As EventHandler(Of Integer))
            Return EventRegistrationTokenTable(Of EventHandler(Of Integer)).
                   GetOrCreateEventRegistrationTokenTable(XEvent).
                   AddEventHandler(handler)
        End AddHandler

        RemoveHandler(token As EventRegistrationToken)
            EventRegistrationTokenTable(Of EventHandler(Of Integer)).
                GetOrCreateEventRegistrationTokenTable(XEvent).
                RemoveEventHandler(token)
        End RemoveHandler

        RaiseEvent(sender As Object, i As Integer)
            Dim table = EventRegistrationTokenTable(Of EventHandler(Of Integer)).
                GetOrCreateEventRegistrationTokenTable(XEvent).
                InvocationList
            If table IsNot Nothing Then table(sender, i)
        End RaiseEvent
    End Event
End Class
```


## <a name="constants"></a><span data-ttu-id="6e6ae-509">Konstanten</span><span class="sxs-lookup"><span data-stu-id="6e6ae-509">Constants</span></span>

<span data-ttu-id="6e6ae-510">Ein *Konstanten* ist ein konstanter Wert, der ein Member eines Typs ist.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-510">A *constant* is a constant value that is a member of a type.</span></span>

```antlr
ConstantMemberDeclaration
    : Attributes? ConstantModifier* 'Const' ConstantDeclarators StatementTerminator
    ;

ConstantModifier
    : AccessModifier
    | 'Shadows'
    ;

ConstantDeclarators
    : ConstantDeclarator ( Comma ConstantDeclarator )*
    ;

ConstantDeclarator
    : Identifier ( 'As' TypeName )? Equals ConstantExpression StatementTerminator
    ;
```

<span data-ttu-id="6e6ae-511">Konstanten werden implizit gemeinsam genutzt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-511">Constants are implicitly shared.</span></span> <span data-ttu-id="6e6ae-512">Wenn die Deklaration enthält eine `As` -Klausel, die Klausel gibt den Typ des Elements durch die Deklaration eingeführt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-512">If the declaration contains an `As` clause, the clause specifies the type of the member introduced by the declaration.</span></span> <span data-ttu-id="6e6ae-513">Wenn der Typ ausgelassen wird, und klicken Sie dann der Typ der Konstante abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-513">If the type is omitted then the type of the constant is inferred.</span></span> <span data-ttu-id="6e6ae-514">Der Typ einer Konstante kann nur ein primitiver Typ sein oder `Object`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-514">The type of a constant may only be a primitive type or `Object`.</span></span> <span data-ttu-id="6e6ae-515">Wenn eine Konstante als typisiert ist `Object` und kein Typzeichen vorhanden ist, der tatsächliche Typ der Konstanten wird der Typ des konstanten Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-515">If a constant is typed as `Object` and there is no type character, the real type of the constant will be the type of the constant expression.</span></span> <span data-ttu-id="6e6ae-516">Andernfalls ist der Typ der Konstante der Typ der Konstante des Typs Zeichens.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-516">Otherwise, the type of the constant is the type of the constant's type character.</span></span>

<span data-ttu-id="6e6ae-517">Das folgende Beispiel zeigt eine Klasse namens `Constants` , bei dem zwei öffentliche Konstanten:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-517">The following example shows a class named `Constants` that has two public constants:</span></span>

```vb
Class Constants
    Public Const A As Integer = 1
    Public Const B As Integer = A + 1
End Class
```

<span data-ttu-id="6e6ae-518">Konstanten können Sie über die Klasse, wie im folgenden Beispiel an, die die Werte der druckt zugreifen `Constants.A` und `Constants.B`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-518">Constants can be accessed through the class, as in the following example, which prints out the values of `Constants.A` and `Constants.B`.</span></span>

```vb
Module Test
    Sub Main()
        Console.WriteLine(Constants.A & ", " & Constants.B)
    End Sub 
End Module
```

<span data-ttu-id="6e6ae-519">Deklaration eine Konstante, die mehrere Konstanten deklariert entspricht mehreren Deklarationen der einzelnen Konstanten.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-519">A constant declaration that declares multiple constants is equivalent to multiple declarations of single constants.</span></span> <span data-ttu-id="6e6ae-520">Im folgende Beispiel werden drei Konstanten in einer deklarationsanweisung deklariert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-520">The following example declares three constants in one declaration statement.</span></span>

```vb
Class A
    Protected Const x As Integer = 1, y As Long = 2, z As Short = 3
End Class
```

<span data-ttu-id="6e6ae-521">Diese Deklaration ist äquivalent zu folgendem:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-521">This declaration is equivalent to the following:</span></span>

```vb
Class A
    Protected Const x As Integer = 1
    Protected Const y As Long = 2
    Protected Const z As Short = 3
End Class
```

<span data-ttu-id="6e6ae-522">Die Zugriffsdomäne des Typs der Konstanten muss identisch oder eine Obermenge der Zugriffsdomäne von die Konstante selbst sein.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-522">The accessibility domain of the type of the constant must be the same as or a superset of the accessibility domain of the constant itself.</span></span> <span data-ttu-id="6e6ae-523">Der Konstante Ausdruck muss es sich um einen Wert, der den Typ der Konstante oder einen Typ, der implizit in den Typ der Konstante liefern.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-523">The constant expression must yield a value of the constant's type or of a type that is implicitly convertible to the constant's type.</span></span> <span data-ttu-id="6e6ae-524">Der Konstante Ausdruck darf nicht zirkulär sein; eine Konstante kann, also nicht hinsichtlich sich selbst definiert werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-524">The constant expression may not be circular; that is, a constant may not be defined in terms of itself.</span></span>

<span data-ttu-id="6e6ae-525">Der Compiler wertet automatisch die Konstanten Deklarationen in der richtigen Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-525">The compiler automatically evaluates the constant declarations in the appropriate order.</span></span> <span data-ttu-id="6e6ae-526">Im folgenden Beispiel wertet der Compiler zunächst `Y`, klicken Sie dann `Z`, und schließlich `X`, die Werten 10, 11 und 12, bzw. zu erzeugen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-526">In the following example, the compiler first evaluates `Y`, then `Z`, and finally `X`, producing the values 10, 11, and 12, respectively.</span></span>

```vb
Class A
    Public Const X As Integer = B.Z + 1
    Public Const Y As Integer = 10
End Class

Class B
    Public Const Z As Integer = A.Y + 1
End Class
```

<span data-ttu-id="6e6ae-527">Wenn ein symbolische Namen für einen konstanten Wert erwünscht ist, aber der Typ des Werts ist nicht zulässig in einer Konstantendeklaration oder wenn der Wert zum Zeitpunkt der Kompilierung durch einen konstanten Ausdruck berechnet werden kann, kann stattdessen eine schreibgeschützte Variable verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-527">When a symbolic name for a constant value is desired, but the type of the value is not permitted in a constant declaration or when the value cannot be computed at compile time by a constant expression, a read-only variable may be used instead.</span></span>


## <a name="instance-and-shared-variables"></a><span data-ttu-id="6e6ae-528">-Instanz und freigegebene Variablen</span><span class="sxs-lookup"><span data-stu-id="6e6ae-528">Instance and Shared Variables</span></span>

<span data-ttu-id="6e6ae-529">Eine Instanz oder freigegebene Variable ist ein Member eines Typs, das Informationen speichern kann.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-529">An instance or shared variable is a member of a type that can store information.</span></span>

```antlr
VariableMemberDeclaration
    : Attributes? VariableModifier+ VariableDeclarators StatementTerminator
    ;

VariableModifier
    : AccessModifier
    | 'Shadows'
    | 'Shared'
    | 'ReadOnly'
    | 'WithEvents'
    | 'Dim'
    ;

VariableDeclarators
    : VariableDeclarator ( Comma VariableDeclarator )*
    ;

VariableDeclarator
    : VariableIdentifiers 'As' ObjectCreationExpression
    | VariableIdentifiers ( 'As' TypeName )? ( Equals Expression )?
    ;

VariableIdentifiers
    : VariableIdentifier ( Comma VariableIdentifier )*
    ;

VariableIdentifier
    : Identifier IdentifierModifiers
    ;
```

<span data-ttu-id="6e6ae-530">Die `Dim` Modifizierer muss angegeben werden, wenn kein Modifizierer angegeben sind, jedoch werden, andernfalls ausgelassen können.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-530">The `Dim` modifier must be specified if no modifiers are specified, but may be omitted otherwise.</span></span> <span data-ttu-id="6e6ae-531">Eine einzige Variablendeklaration möglicherweise mehrere Variablendeklaratoren enthalten; Jeder Variablen Deklarator führt eine neue Instanz oder einen freigegebenen Member.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-531">A single variable declaration may include multiple variable declarators; each variable declarator introduces a new instance or shared member.</span></span>

<span data-ttu-id="6e6ae-532">Wenn ein Initialisierer angegeben ist, kann nur eine Instanz oder freigegebene Variable von der Variable Deklarator deklariert werden:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-532">If an initializer is specified, only one instance or shared variable may be declared by the variable declarator:</span></span>

```vb
Class Test
    Dim a, b, c, d As Integer = 10  ' Invalid: multiple initialization
End Class
```

<span data-ttu-id="6e6ae-533">Diese Einschränkung gilt nicht für Objektinitialisierer:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-533">This restriction does not apply to object initializers:</span></span>

```vb
Class Test
    Dim a, b, c, d As New Collection() ' OK
End Class
```

<span data-ttu-id="6e6ae-534">Eine Variable mit dem `Shared` Modifizierer ist ein *freigegebene Variable*.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-534">A variable declared with the `Shared` modifier is a *shared variable*.</span></span> <span data-ttu-id="6e6ae-535">Eine freigegebene Variable gibt genau einen Speicherort unabhängig von der Anzahl der Instanzen des Typs, die erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-535">A shared variable identifies exactly one storage location regardless of the number of instances of the type that are created.</span></span> <span data-ttu-id="6e6ae-536">Eine freigegebene Variable wird erstellt, wenn die Ausführung ein Programms beginnt, und es ist nicht mehr vorhanden sein, wenn das Programm beendet wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-536">A shared variable comes into existence when a program begins executing, and ceases to exist when the program terminates.</span></span>

<span data-ttu-id="6e6ae-537">Eine freigegebene Variable wird nur von Instanzen eines bestimmten geschlossenen generischen Typs gemeinsam genutzt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-537">A shared variable is shared only among instances of a particular closed generic type.</span></span> <span data-ttu-id="6e6ae-538">Um beispielsweise das Programm:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-538">For example, the program:</span></span>

```vb
Class C(Of V) 
    Shared InstanceCount As Integer = 0

    Public Sub New()  
        InstanceCount += 1 
    End Sub

    Public Shared ReadOnly Property Count() As Integer 
        Get
            Return InstanceCount
        End Get
    End Property
End Class

Class Application 
    Shared Sub Main() 
        Dim x1 As New C(Of Integer)()
        Console.WriteLine(C(Of Integer).Count)

        Dim x2 As New C(Of Double)() 
        Console.WriteLine(C(Of Integer).Count)

        Dim x3 As New C(Of Integer)() 
        Console.WriteLine(C(Of Integer).Count)
    End Sub
End Class
```

<span data-ttu-id="6e6ae-539">Druckt:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-539">Prints out:</span></span>

```
1
1
2
```

<span data-ttu-id="6e6ae-540">Eine Variable deklariert, ohne die `Shared` Modifizierer wird aufgerufen, eine *Instanzvariable*.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-540">A variable declared without the `Shared` modifier is called an *instance variable*.</span></span> <span data-ttu-id="6e6ae-541">Jede Instanz einer Klasse enthält eine separate Kopie aller Instanz Variablen der Klasse.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-541">Every instance of a class contains a separate copy of all instance variables of the class.</span></span> <span data-ttu-id="6e6ae-542">Eine Instanzvariable für ein Verweistyp wird erstellt, wenn eine neue Instanz der, Typ erstellt wurde, und ist nicht mehr vorhanden sein, wenn keine Verweise auf diese Instanz vorhanden sind und die `Finalize` Methode ausgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-542">An instance variable of a reference type comes into existence when a new instance of that type is created, and ceases to exist when there are no references to that instance and the `Finalize` method has executed.</span></span> <span data-ttu-id="6e6ae-543">Eine Instanzvariable für ein Werttyp ist genau die gleiche Lebensdauer wie die Variable an, zu der er gehört.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-543">An instance variable of a value type has exactly the same lifetime as the variable to which it belongs.</span></span> <span data-ttu-id="6e6ae-544">Das heißt, wenn eine Variable eines Werttyps erstellt wird, oder ist nicht mehr vorhanden ist, steigt auch die Instanzenvariable des Werttyps.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-544">In other words, when a variable of a value type comes into existence or ceases to exist, so does the instance variable of the value type.</span></span>

<span data-ttu-id="6e6ae-545">Wenn der Deklarator enthält ein `As` -Klausel, die Klausel gibt den Typ der Member durch die Deklaration eingeführt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-545">If the declarator contains an `As` clause, the clause specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="6e6ae-546">Wenn der Typ ausgelassen wird und strenge Semantik verwendet wird, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-546">If the type is omitted and strict semantics are being used, a compile-time error occurs.</span></span> <span data-ttu-id="6e6ae-547">Andernfalls ist der Typ der Elemente implizit `Object` oder den Typ der Member-Typzeichen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-547">Otherwise the type of the members is implicitly `Object` or the type of the members' type character.</span></span>

<span data-ttu-id="6e6ae-548">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="6e6ae-548">__Note.__</span></span> <span data-ttu-id="6e6ae-549">Liegt keine Mehrdeutigkeit in der Syntax: Wenn ein Deklarator ein weggelassen wird, wird immer verwendet, den Typ der folgenden Deklaration.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-549">There is no ambiguity in the syntax: if a declarator omits a type, it will always use the type of a following declarator.</span></span>

<span data-ttu-id="6e6ae-550">Die Zugriffsdomäne von einer Instanz oder freigegebene Variable Typ oder Elementtyp des Arrays muss identisch oder eine Obermenge der Zugriffsdomäne von der Instanz oder freigegebene Variable sich selbst sein.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-550">The accessibility domain of an instance or shared variable's type or array element type must be the same as or a superset of the accessibility domain of the instance or shared variable itself.</span></span>

<span data-ttu-id="6e6ae-551">Das folgende Beispiel zeigt eine `Color` -Klasse, die interne Instanz mit dem Namen Variablen `redPart`, `greenPart`, und `bluePart`:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-551">The following example shows a `Color` class that has internal instance variables named `redPart`, `greenPart`, and `bluePart`:</span></span>

```vb
Class Color
    Friend redPart As Short
    Friend bluePart As Short
    Friend greenPart As Short

    Public Sub New(red As Short, blue As Short, green As Short)
        redPart = red
        bluePart = blue
        greenPart = green
    End Sub
End Class
```


### <a name="read-only-variables"></a><span data-ttu-id="6e6ae-552">Schreibgeschützte Variablen</span><span class="sxs-lookup"><span data-stu-id="6e6ae-552">Read-Only Variables</span></span>

<span data-ttu-id="6e6ae-553">Wenn eine Instanz oder freigegebene-Variablendeklaration enthält eine `ReadOnly` Modifizierer-Zuweisungen für die Variablen, die durch die Deklaration eingeführt können nur auftreten, als Teil der Deklaration oder in einem Konstruktor derselben Klasse.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-553">When an instance or shared variable declaration includes a `ReadOnly` modifier, assignments to the variables introduced by the declaration may only occur as part of the declaration or in a constructor in the same class.</span></span> <span data-ttu-id="6e6ae-554">Insbesondere dürfen Zuweisungen zu einer schreibgeschützten Instanz oder freigegebene Variable nur in den folgenden Situationen:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-554">Specifically, assignments to a read-only instance or shared variable are permitted only in the following situations:</span></span>

* <span data-ttu-id="6e6ae-555">In der Variablendeklaration, die die Instanz oder freigegebene Variable eingeführt werden (durch Einbeziehen von einem Variableninitialisierer in der Deklaration).</span><span class="sxs-lookup"><span data-stu-id="6e6ae-555">In the variable declaration that introduces the instance or shared variable (by including a variable initializer in the declaration).</span></span>

* <span data-ttu-id="6e6ae-556">Für eine Instanzenvariable in den Instanzkonstruktoren der Klasse, die die Variablendeklaration enthält.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-556">For an instance variable, in the instance constructors of the class that contains the variable declaration.</span></span> <span data-ttu-id="6e6ae-557">Die Instanzvariable kann nur zugegriffen werden, auf einen nicht qualifizierten Weise oder über `Me` oder `MyClass`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-557">The instance variable can only be accessed in an unqualified manner or through `Me` or `MyClass`.</span></span>

* <span data-ttu-id="6e6ae-558">Für eine freigegebene Variable in der gemeinsam genutzte Konstruktor der Klasse, die die freigegebene Variablendeklaration enthält.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-558">For a shared variable, in the shared constructor of the class that contains the shared variable declaration.</span></span>

<span data-ttu-id="6e6ae-559">Eine freigegebene schreibgeschützte-Variable ist hilfreich, wenn ein symbolische Namen für einen konstanten Wert gewünscht ist, aber wenn der Typ des Werts in einer Konstantendeklaration nicht zulässig ist, oder wenn der Wert durch einen konstanten Ausdruck nicht zur Kompilierzeit berechnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-559">A shared read-only variable is useful when a symbolic name for a constant value is desired, but when the type of the value is not permitted in a constant declaration, or when the value cannot be computed at compile time by a constant expression.</span></span>

<span data-ttu-id="6e6ae-560">Ein Beispiel für die erste eine solche Anwendung folgt, in welche Farbe gemeinsam verwendeter Variablen deklariert werden `ReadOnly` zu verhindern, dass sie von anderen Programmen geändert wird:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-560">An example of the first such application follows, in which color shared variables are declared `ReadOnly` to prevent them from being changed by other programs:</span></span>

```vb
Class Color
    Friend redPart As Short
    Friend bluePart As Short
    Friend greenPart As Short

    Public Sub New(red As Short, blue As Short, green As Short)
        redPart = red
        bluePart = blue
        greenPart = green
    End Sub 

    Public Shared ReadOnly Red As Color = New Color(&HFF, 0, 0)
    Public Shared ReadOnly Blue As Color = New Color(0, &HFF, 0)
    Public Shared ReadOnly Green As Color = New Color(0, 0, &HFF)
    Public Shared ReadOnly White As Color = New Color(&HFF, &HFF, &HFF)
End Class
```

<span data-ttu-id="6e6ae-561">Konstanten und schreibgeschützte freigegebene Variablen haben unterschiedliche Semantiken auf.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-561">Constants and read-only shared variables have different semantics.</span></span> <span data-ttu-id="6e6ae-562">Wenn eine Konstante in ein Ausdruck verwiesen wird, wird der Wert der Konstanten zur Kompilierzeit abgerufen, aber wenn ein Ausdruck eine schreibgeschützte freigegebene Variable verweist, ist der Wert der freigegebene Variablen nicht bis zur Laufzeit abgerufen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-562">When an expression references a constant, the value of the constant is obtained at compile time, but when an expression references a read-only shared variable, the value of the shared variable is not obtained until run time.</span></span> <span data-ttu-id="6e6ae-563">Erwägen Sie die folgende Anwendung, die aus zwei verschiedenen Programmen besteht.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-563">Consider the following application, which consists of two separate programs.</span></span>

<span data-ttu-id="6e6ae-564">file1.vb:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-564">file1.vb:</span></span>

```vb
Namespace Program1
    Public Class Utils
        Public Shared ReadOnly X As Integer = 1
    End Class
End Namespace
```

<span data-ttu-id="6e6ae-565">file2.vb:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-565">file2.vb:</span></span>

```vb
Namespace Program2
    Module Test
        Sub Main()
            Console.WriteLine(Program1.Utils.X)
        End Sub
    End Module
End Namespace
```

<span data-ttu-id="6e6ae-566">Die Namespaces `Program1` und `Program2` kennzeichnen die beiden Programme, die separat kompiliert werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-566">The namespaces `Program1` and `Program2` denote two programs that are compiled separately.</span></span> <span data-ttu-id="6e6ae-567">Da Variablen `Program1.Utils.X` ist als deklariert `Shared ReadOnly`, Ausgabe des Werts durch die `Console.WriteLine` Anweisung zum Zeitpunkt der Kompilierung nicht bekannt ist, aber stattdessen zur Laufzeit abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-567">Because variable `Program1.Utils.X` is declared as `Shared ReadOnly`, the value output by the `Console.WriteLine` statement is not known at compile time, but rather is obtained at run time.</span></span> <span data-ttu-id="6e6ae-568">Also wenn der Wert des `X` geändert wird und `Program1` erneut kompiliert wird, die `Console.WriteLine` Anweisung gibt der neue Wert, selbst wenn `Program2` nicht neu kompiliert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-568">Thus, if the value of `X` is changed and `Program1` is recompiled, the `Console.WriteLine` statement will output the new value even if `Program2` is not recompiled.</span></span> <span data-ttu-id="6e6ae-569">Aber wenn `X` war eine Konstante, die den Wert der `X` würde abgerufen haben, wurden zum Zeitpunkt `Program2` kompiliert wurde, und würden geblieben sind nicht betroffen von Änderungen in `Program1` bis `Program2` neu kompiliert wurde.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-569">However, if `X` had been a constant, the value of `X` would have been obtained at the time `Program2` was compiled, and would have remained unaffected by changes in `Program1` until `Program2` was recompiled.</span></span>

### <a name="withevents-variables"></a><span data-ttu-id="6e6ae-570">WithEvents-Variablen</span><span class="sxs-lookup"><span data-stu-id="6e6ae-570">WithEvents Variables</span></span>

<span data-ttu-id="6e6ae-571">Einen Typ zu deklarieren kann, dass sie eine Gruppe von Ereignissen, die durch eine der zugehörigen Instanz oder freigegebene Variablen ausgelöst wird, durch die Deklaration der Instanz oder freigegebene Variable behandelt, die die Ereignisse mit löst die `WithEvents` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-571">A type can declare that it handles some set of events raised by one of its instance or shared variables by declaring the instance or shared variable that raises the events with the `WithEvents` modifier.</span></span> <span data-ttu-id="6e6ae-572">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-572">For example:</span></span>

```vb
Class Raiser
    Public Event E1()

    Public Sub Raise()
        RaiseEvent E1
    End Sub
End Class

Module Test
    Private WithEvents x As Raiser

    Private Sub E1Handler() Handles x.E1
        Console.WriteLine("Raised")
    End Sub

    Public Sub Main()
        x = New Raiser()
    End Sub
End Module
```

<span data-ttu-id="6e6ae-573">In diesem Beispiel ist die Methode `E1Handler` behandelt das Ereignis `E1` , der von der Instanz des Typs ausgelöst wird `Raiser` in der Instanzvariable gespeicherte `x`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-573">In this example, the method `E1Handler` handles the event `E1` that is raised by the instance of the type `Raiser` stored in the instance variable `x`.</span></span>

<span data-ttu-id="6e6ae-574">Die `WithEvents` Modifizierer bewirkt, dass die Variable mit einem führenden Unterstrich umbenannt und durch eine Eigenschaft mit dem gleichen Namen, die die ereigniseinbindung ersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-574">The `WithEvents` modifier causes the variable to be renamed with a leading underscore and replaced with a property of the same name that does the event hookup.</span></span> <span data-ttu-id="6e6ae-575">Wenn Sie den Namen der Variablen wird z. B. `F`, es wird umbenannt in `_F` und eine Eigenschaft `F` wird implizit deklariert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-575">For example, if the variable's name is `F`, it is renamed to `_F` and a property `F` is implicitly declared.</span></span> <span data-ttu-id="6e6ae-576">Liegt ein Konflikt zwischen dem neuen Variablennamen und eine andere Deklaration vor, wird ein Kompilierzeitfehler gemeldet.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-576">If there is a collision between the variable's new name and another declaration, a compile-time error will be reported.</span></span> <span data-ttu-id="6e6ae-577">Alle Attribute der Variablen angewendet werden auf die umbenannte Variable übernommen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-577">Any attributes applied to the variable are carried over to the renamed variable.</span></span>

<span data-ttu-id="6e6ae-578">Die implizite von erstellte Eigenschaft eine `WithEvents` Deklaration verbinden und lösen die entsprechenden Ereignishandler kümmert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-578">The implicit property created by a `WithEvents` declaration takes care of hooking and unhooking the relevant event handlers.</span></span> <span data-ttu-id="6e6ae-579">Wenn ein Wert der Variablen zugewiesen ist, ruft die Eigenschaft zunächst die `remove` Methode für das Ereignis für die Instanz derzeit in der Variablen (Trennen des den vorhandenen Ereignishandler, sofern vorhanden).</span><span class="sxs-lookup"><span data-stu-id="6e6ae-579">When a value is assigned to the variable, the property first calls the `remove` method for the event on the instance currently in the variable (unhooking the existing event handler, if any).</span></span> <span data-ttu-id="6e6ae-580">Als Nächstes die Zuweisung erfolgt und die Eigenschaft ruft die `add` Methode für das Ereignis auf der neuen Instanz in der Variablen (Einbinden von den neuen Ereignishandler).</span><span class="sxs-lookup"><span data-stu-id="6e6ae-580">Next the assignment is made, and the property calls the `add` method for the event on the new instance in the variable (hooking up the new event handler).</span></span> <span data-ttu-id="6e6ae-581">Der folgende Code entspricht der Code oben für das standard-Modul `Test`:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-581">The following code is equivalent to the code above for the standard module `Test`:</span></span>

```vb
Module Test
    Private _x As Raiser

    Public Property x() As Raiser
        Get
            Return _x
        End Get

        Set (Value As Raiser)
            ' Unhook any existing handlers.
            If _x IsNot Nothing Then
                RemoveHandler _x.E1, AddressOf E1Handler
            End If

            ' Change value.
            _x = Value

            ' Hook-up new handlers.
            If _x IsNot Nothing Then
                AddHandler _x.E1, AddressOf E1Handler
            End If
        End Set
    End Property

    Sub E1Handler()
        Console.WriteLine("Raised")
    End Sub

    Sub Main()
        x = New Raiser()
    End Sub
End Module
```

<span data-ttu-id="6e6ae-582">Es ist nicht zulässig, deklarieren eine Instanz oder freigegebene Variable als `WithEvents` Wenn die Variable als Struktur typisiert ist.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-582">It is not valid to declare an instance or shared variable as `WithEvents` if the variable is typed as a structure.</span></span> <span data-ttu-id="6e6ae-583">Darüber hinaus `WithEvents` dürfen nicht in einer Struktur angegeben werden und `WithEvents` und `ReadOnly` können nicht kombiniert werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-583">In addition, `WithEvents` may not be specified in a structure, and `WithEvents` and `ReadOnly` cannot be combined.</span></span>

### <a name="variable-initializers"></a><span data-ttu-id="6e6ae-584">Variableninitialisierern</span><span class="sxs-lookup"><span data-stu-id="6e6ae-584">Variable Initializers</span></span>

<span data-ttu-id="6e6ae-585">-Instanz und freigegebene Variablendeklarationen in Klassen und Instanz Variablendeklarationen (aber nicht freigegeben Variablendeklarationen) in Strukturen können Variableninitialisierern enthalten.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-585">Instance and shared variable declarations in classes and instance variable declarations (but not shared variable declarations) in structures may include variable initializers.</span></span> <span data-ttu-id="6e6ae-586">Für `Shared` Variablen Variableninitialisierern entsprechen zuweisungsanweisungen, die ausgeführt werden, nachdem das Programm, jedoch bevor beginnt die `Shared` Variable ist zunächst auf die verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-586">For `Shared` variables, variable initializers correspond to assignment statements that are executed after the program begins, but before the `Shared` variable is first referenced.</span></span> <span data-ttu-id="6e6ae-587">Beispielsweise entsprechen Variablen Variableninitialisierern zuweisungsanweisungen, die ausgeführt werden, wenn eine Instanz der Klasse erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-587">For instance variables, variable initializers correspond to assignment statements that are executed when an instance of the class is created.</span></span> <span data-ttu-id="6e6ae-588">Strukturen können keine Instanz Variableninitialisierern verwenden, da die parameterlosen Konstruktoren nicht geändert werden können.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-588">Structures cannot have instance variable initializers because their parameterless constructors cannot be modified.</span></span>

<span data-ttu-id="6e6ae-589">Betrachten Sie das folgende Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-589">Consider the following example:</span></span>

```vb
Class Test
    Public Shared x As Double = Math.Sqrt(2.0)
    Public i As Integer = 100
    Public s As String = "Hello"
End Class

Module TestModule
    Sub Main()
        Dim a As New Test()

        Console.WriteLine("x = " & Test.x & ", i = " & a.i & ", s = " & a.s)
    End Sub
End Module
```

<span data-ttu-id="6e6ae-590">Das Beispiel führt zur folgenden Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-590">The example produces the following output:</span></span>

```
x = 1.4142135623731, i = 100, s = Hello
```

<span data-ttu-id="6e6ae-591">Eine Zuweisung zu `x` tritt auf, wenn die Klasse geladen wird, und Zuweisungen `i` und `s` auftreten, wenn eine neue Instanz der Klasse erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-591">An assignment to `x` occurs when the class is loaded, and assignments to `i` and `s` occur when a new instance of the class is created.</span></span>

<span data-ttu-id="6e6ae-592">Es ist sinnvoll, Variableninitialisierern als zuweisungsanweisungen vorstellen, die im Konstruktor des Typs-Block eingefügt werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-592">It is useful to think of variable initializers as assignment statements that are automatically inserted in the block of the type's constructor.</span></span> <span data-ttu-id="6e6ae-593">Das folgende Beispiel enthält mehrere Instanz Variableninitialisierern.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-593">The following example contains several instance variable initializers.</span></span>

```vb
Class A
    Private x As Integer = 1
    Private y As Integer = -1
    Private count As Integer

    Public Sub New()
        count = 0
    End Sub

    Public Sub New(n As Integer)
        count = n
    End Sub
End Class

Class B
    Inherits A

    Private sqrt2 As Double = Math.Sqrt(2.0)
    Private items As ArrayList = New ArrayList(100)
    Private max As Integer

    Public Sub New()
        Me.New(100)
        items.Add("default")
    End Sub

    Public Sub New(n As Integer)
        MyBase.New(n - 1)
        max = n
    End Sub
End Class
```

<span data-ttu-id="6e6ae-594">Das Beispiel entspricht dem folgenden Code, wobei jeder Kommentar eine automatisch eingefügte Anweisung angibt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-594">The example corresponds to the code shown below, where each comment indicates an automatically inserted statement.</span></span>

```vb
Class A
    Private x, y, count As Integer

    Public Sub New()
        MyBase.New ' Invoke object() constructor.
        x = 1 ' This is a variable initializer.
        y = -1 ' This is a variable initializer.
        count = 0
    End Sub

    Public Sub New(n As Integer)
        MyBase.New ' Invoke object() constructor. 
        x = 1 ' This is a variable initializer.
        y = - 1 ' This is a variable initializer.
        count = n
    End Sub
End Class

Class B
    Inherits A

    Private sqrt2 As Double
    Private items As ArrayList
    Private max As Integer

    Public Sub New()
        Me.New(100) 
        items.Add("default")
    End Sub

    Public Sub New(n As Integer)
        MyBase.New(n - 1) 
        sqrt2 = Math.Sqrt(2.0) ' This is a variable initializer.
        items = New ArrayList(100) ' This is a variable initializer.
        max = n
    End Sub
End Class
```

<span data-ttu-id="6e6ae-595">Alle Variablen werden auf den Standardwert von ihrem Typ initialisiert, bevor alle Variableninitialisierern ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-595">All variables are initialized to the default value of their type before any variable initializers are executed.</span></span> <span data-ttu-id="6e6ae-596">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-596">For example:</span></span>

```vb
Class Test
    Public Shared b As Boolean
    Public i As Integer
End Class

Module TestModule
    Sub Main()
        Dim t As New Test()
        Console.WriteLine("b = " & Test.b & ", i = " & t.i)
    End Sub
End Module
```

<span data-ttu-id="6e6ae-597">Da `b` wird automatisch auf den Standardwert initialisiert, wenn die Klasse geladen wird und `i` wird automatisch auf den Standardwert initialisiert, wenn eine Instanz der Klasse erstellt wird, erzeugt der vorhergehende Code die folgende Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-597">Because `b` is automatically initialized to its default value when the class is loaded and `i` is automatically initialized to its default value when an instance of the class is created, the preceding code produces the following output:</span></span>

```
b = False, i = 0
```

<span data-ttu-id="6e6ae-598">Jede Variableninitialisierer muss einen Wert, der den Typ der Variablen oder eines Typs, der implizit in den Typ der Variable ist liefern.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-598">Each variable initializer must yield a value of the variable's type or of a type that is implicitly convertible to the variable's type.</span></span> <span data-ttu-id="6e6ae-599">Variableninitialisierer zirkulär sein oder finden Sie in einer Variablen, die hinter ihm, in dem Fall wird der Wert der Variablen, auf die verwiesen wird der Standardwert für die Zwecke des Initialisierers ist initialisiert wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-599">A variable initializer may be circular or refer to a variable that will be initialized after it, in which case the value of the referenced variable is its default value for the purposes of the initializer.</span></span> <span data-ttu-id="6e6ae-600">Solche ein Initialisierer ist fragwürdiger.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-600">Such an initializer is of dubious value.</span></span>

<span data-ttu-id="6e6ae-601">Es gibt drei Arten von Variableninitialisierern: reguläre Initialisierer Arraygröße Initialisierer und Objektinitialisierer.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-601">There are three forms of variable initializers: regular initializers, array-size initializers, and object initializers.</span></span> <span data-ttu-id="6e6ae-602">Die ersten beiden Formen angezeigt werden, nach einem Gleichheitszeichen, die den Typnamen folgt, die letzten beiden sind Teil der Deklaration selbst.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-602">The first two forms appear after an equal sign that follows the type name, the latter two are part of the declaration itself.</span></span> <span data-ttu-id="6e6ae-603">Nur eine Art der Initialisierer kann in einer Deklaration verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-603">Only one form of initializer may be used on any particular declaration.</span></span>

#### <a name="regular-initializers"></a><span data-ttu-id="6e6ae-604">Reguläre Initialisierer</span><span class="sxs-lookup"><span data-stu-id="6e6ae-604">Regular Initializers</span></span>

<span data-ttu-id="6e6ae-605">Ein *regulären Initialisierer* ist ein Ausdruck, der implizit in den Typ der Variablen konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-605">A *regular initializer* is an expression that is implicitly convertible to the type of the variable.</span></span> <span data-ttu-id="6e6ae-606">Er wird nach einem Gleichheitszeichen, das folgt des Typnamens und muss als Wert klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-606">It appears after an equal sign that follows the type name and must be classified as a value.</span></span> <span data-ttu-id="6e6ae-607">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-607">For example:</span></span>

```vb
Module Test
    Dim x As Integer = 10
    Dim y As Integer = 20

    Sub Main()
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub
End Module
```

<span data-ttu-id="6e6ae-608">Dieses Programm erzeugt die Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-608">This program produces the output:</span></span>

```vb
x = 10, y = 20
```

<span data-ttu-id="6e6ae-609">Wenn eine Variablendeklaration einen regulären Initialisierer hat, kann jeweils nur eine einzelne Variable deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-609">If a variable declaration has a regular initializer, then only a single variable can be declared at a time.</span></span> <span data-ttu-id="6e6ae-610">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-610">For example:</span></span>

```vb
Module Test
    Sub Main()
        ' OK, only one variable declared at a time.
        Dim x As Integer = 10, y As Integer = 20

        ' Error: Can't initialize multiple variables at once.
        Dim a, b As Integer = 10
    End Sub
End Module
```

#### <a name="object-initializers"></a><span data-ttu-id="6e6ae-611">Objektinitialisierer</span><span class="sxs-lookup"><span data-stu-id="6e6ae-611">Object Initializers</span></span>

<span data-ttu-id="6e6ae-612">Ein *Objektinitialisierer* wird mithilfe einer Objekterstellungsausdruck anstelle der Typname angegeben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-612">An *object initializer* is specified using an object creation expression in the place of the type name.</span></span> <span data-ttu-id="6e6ae-613">Ein Objektinitialisierer ist gleichbedeutend mit der eine reguläre Initialisierung der Variablen das Ergebnis der Objekterstellungsausdruck zuweisen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-613">An object initializer is equivalent to a regular initializer assigning the result of the object creation expression to the variable.</span></span> <span data-ttu-id="6e6ae-614">So</span><span class="sxs-lookup"><span data-stu-id="6e6ae-614">So</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As New Test(10)
    End Sub
End Module
```

<span data-ttu-id="6e6ae-615">für die folgende Syntax:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-615">is equivalent to</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test(10)
    End Sub
End Module
```

<span data-ttu-id="6e6ae-616">Die Klammer in einem Objektinitialisierer wird immer als Argumentliste für den Konstruktor und niemals als Arraymodifizierer-Typ interpretiert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-616">The parenthesis in an object initializer is always interpreted as the argument list for the constructor and never as array type modifiers.</span></span> <span data-ttu-id="6e6ae-617">Ein Variablenname mit einem Objektinitialisierer kein Array der Modifizierer oder den Typmodifizierer für einen NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-617">A variable name with an object initializer cannot have an array type modifier or a nullable type modifier.</span></span>

#### <a name="array-size-initializers"></a><span data-ttu-id="6e6ae-618">Größe des Arrays Initialisierer</span><span class="sxs-lookup"><span data-stu-id="6e6ae-618">Array-Size Initializers</span></span>

<span data-ttu-id="6e6ae-619">Ein *Arraygröße Initialisierer* ist ein Modifizierer für den Namen der Variablen, die einen Satz von Dimension Obergrenzen, gekennzeichnet durch Ausdrücke bietet.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-619">An *array-size initializer* is a modifier on the name of the variable that gives a set of dimension upper bounds denoted by expressions.</span></span>

```antlr
ArraySizeInitializationModifier
    : OpenParenthesis BoundList CloseParenthesis ArrayTypeModifiers?
    ;

BoundList
    : Bound ( Comma Bound )*
    ;

Bound
    : Expression
    | '0' 'To' Expression
    ;
```

<span data-ttu-id="6e6ae-620">Die obere Grenze-Ausdrücke müssen klassifiziert werden, als Werte und muss implizit in `Integer`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-620">The upper bound expressions must be classified as values and must be implicitly convertible to `Integer`.</span></span> <span data-ttu-id="6e6ae-621">Der Satz von Obergrenzen ist gleichbedeutend mit einem Variableninitialisierer eines Ausdrucks für die Arrayerstellung mit der angegebenen oberen Grenzen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-621">The set of upper bounds is equivalent to a variable initializer of an array-creation expression with the given upper bounds.</span></span> <span data-ttu-id="6e6ae-622">Die Anzahl der Dimensionen des Arraytyps wird von der Größe Arrayinitialisierer abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-622">The number of dimensions of the array type is inferred from the array size initializer.</span></span> <span data-ttu-id="6e6ae-623">So</span><span class="sxs-lookup"><span data-stu-id="6e6ae-623">So</span></span>

```vb
Module Test
    Sub Main()
        Dim x(5, 10) As Integer
    End Sub
End Module
```

<span data-ttu-id="6e6ae-624">für die folgende Syntax:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-624">is equivalent to</span></span>

```vb
Module Test
    Sub Main()
        Dim x As Integer(,) = New Integer(5, 10) {}
    End Sub
End Module
```

<span data-ttu-id="6e6ae-625">Alle oberen Grenzen muss gleich oder größer als-1 sein, und alle Dimensionen müssen eine Obergrenze angegeben haben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-625">All upper bounds must be equal to or greater than -1, and all dimensions must have an upper bound specified.</span></span> <span data-ttu-id="6e6ae-626">Wenn der Elementtyp des Arrays initialisiert wird selbst einen Arraytyp ist, wechseln Sie die Array-Typ-Modifizierer, rechts von der Größe des Arrays Initialisierer.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-626">If the element type of the array being initialized is itself an array type, the array-type modifiers go to the right of the array-size initializer.</span></span> <span data-ttu-id="6e6ae-627">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-627">For example</span></span>

```vb
Module Test
    Sub Main()
        Dim x(5,10)(,,) As Integer
    End Sub
End Module
```

<span data-ttu-id="6e6ae-628">deklariert eine lokale Variable `x` , dessen Typ ist ein zweidimensionales Array von dreidimensionalen Arrays `Integer`, auf ein Array mit den Grenzen des initialisierten `0..5` in der ersten Dimension und `0..10` in der zweiten Dimension.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-628">declares a local variable `x` whose type is a two-dimensional array of three-dimensional arrays of `Integer`, initialized to an array with bounds of `0..5` in the first dimension and `0..10` in the second dimension.</span></span> <span data-ttu-id="6e6ae-629">Es ist nicht möglich, einen Arrayinitialisierer der Größe zu verwenden, um die Elemente einer Variablen zu initialisieren, dessen Typ ein Array aus Arrays ist.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-629">It is not possible to use an array size initializer to initialize the elements of a variable whose type is an array of arrays.</span></span>

<span data-ttu-id="6e6ae-630">Die Deklaration eine Variable mit einem Initialisierer für die Größe des Arrays kann keinen Array der Modifizierer für den Typ oder einen regulären Initialisierer haben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-630">A variable declaration with an array-size initializer cannot have an array type modifier on its type or a regular initializer.</span></span>


### <a name="systemmarshalbyrefobject-classes"></a><span data-ttu-id="6e6ae-631">System.MarshalByRefObject-Klassen</span><span class="sxs-lookup"><span data-stu-id="6e6ae-631">System.MarshalByRefObject Classes</span></span>

<span data-ttu-id="6e6ae-632">Klassen, die von der Klasse abgeleitet sind `System.MarshalByRefObject` Kontextgrenzen hinweg proxybasierte gemarshallt werden (d. h. als Verweis) statt durch Kopieren (, also nach Wert).</span><span class="sxs-lookup"><span data-stu-id="6e6ae-632">Classes that derive from the class `System.MarshalByRefObject` are marshaled across context boundaries using proxies (that is, by reference) rather than through copying (that is, by value).</span></span> <span data-ttu-id="6e6ae-633">Dies bedeutet, dass eine Instanz einer solchen Klasse möglicherweise nicht in eine Instanz der "true", aber stattdessen nur ggf. ein Stub, der Variablen marshallt greift auf Methodenaufrufe, die über eine Kontextgrenze hinweg.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-633">This means that an instance of such a class may not be a true instance but instead may just be a stub that marshals variable accesses and method calls across a context boundary.</span></span>

<span data-ttu-id="6e6ae-634">Daher ist es nicht möglich, einen Verweis auf den Speicherort der Variablen für diese Klassen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-634">As a result, it is not possible to create a reference to the storage location of variables defined on such classes.</span></span> <span data-ttu-id="6e6ae-635">Dies bedeutet, dass Variablen typisiert, abgeleitete Klassen `System.MarshalByRefObject` kann nicht zum Verweisen auf Parameter, und die Methoden und Variablen von Variablen wie Werttypen nicht zugegriffen werden können, übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-635">This means that variables typed as classes derived from `System.MarshalByRefObject` cannot be passed to reference parameters, and methods and variables of variables typed as value types may not be accessed.</span></span> <span data-ttu-id="6e6ae-636">Visual Basic behandelt stattdessen Variablen, die für solche Klassen definiert werden, als wären sie Eigenschaften (da die Einschränkungen in den Eigenschaften identisch sind).</span><span class="sxs-lookup"><span data-stu-id="6e6ae-636">Instead, Visual Basic treats variables defined on such classes as if they were properties (since the restrictions are the same on properties).</span></span>

<span data-ttu-id="6e6ae-637">Es gibt eine Ausnahme von dieser Regel: ein Element mit implizit oder explizit qualifiziert `Me` unterliegt der oben beschriebenen Einschränkungen, da `Me` ist immer garantiert ein tatsächliches Objekt, nicht über einen Proxy.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-637">There is one exception to this rule: a member implicitly or explicitly qualified with `Me` is exempt from the above restrictions, because `Me` is always guaranteed to be an actual object, not a proxy.</span></span>

## <a name="properties"></a><span data-ttu-id="6e6ae-638">Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="6e6ae-638">Properties</span></span>

<span data-ttu-id="6e6ae-639">*Eigenschaften* sind eine natürliche Erweiterung von Variablen; beide sind benannte Member mit zugeordneten Typen und die Syntax für den Zugriff auf Variablen und Eigenschaften entspricht.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-639">*Properties* are a natural extension of variables; both are named members with associated types, and the syntax for accessing variables and properties is the same.</span></span> <span data-ttu-id="6e6ae-640">Im Gegensatz zu den Variablen keinen jedoch Eigenschaften Speicherorte an.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-640">Unlike variables, however, properties do not denote storage locations.</span></span> <span data-ttu-id="6e6ae-641">Müssen Sie Eigenschaften stattdessen *Accessoren*, die die Anweisungen ausführen, um das Lesen oder Schreiben deren Werte angeben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-641">Instead, properties have *accessors*, which specify the statements to execute in order to read or write their values.</span></span>

<span data-ttu-id="6e6ae-642">Eigenschaften werden mit Eigenschaftendeklarationen definiert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-642">Properties are defined with property declarations.</span></span> <span data-ttu-id="6e6ae-643">Der erste Teil einer Eigenschaftendeklaration ähnelt einer Felddeklaration.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-643">The first part of a property declaration resembles a field declaration.</span></span> <span data-ttu-id="6e6ae-644">Der zweite Teil enthält eine `Get` Accessor und/oder einen `Set` Accessor.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-644">The second part includes a `Get` accessor and/or a `Set` accessor.</span></span>

```antlr
PropertyMemberDeclaration
    : RegularPropertyMemberDeclaration
    | MustOverridePropertyMemberDeclaration
    | AutoPropertyMemberDeclaration
    ;

PropertySignature
    : 'Property'
      Identifier ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )?
    ;

RegularPropertyMemberDeclaration
    : Attributes? PropertyModifier* PropertySignature
      ImplementsClause? LineTerminator
      PropertyAccessorDeclaration+
      'End' 'Property' StatementTerminator
    ;

MustOverridePropertyMemberDeclaration
    : Attributes? MustOverridePropertyModifier+ PropertySignature
      ImplementsClause? StatementTerminator
    ;

AutoPropertyMemberDeclaration
    : Attributes? AutoPropertyModifier* 'Property' Identifier
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )? ( Equals Expression )?
      ImplementsClause? LineTerminator
    | Attributes? AutoPropertyModifier* 'Property' Identifier
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      'As' Attributes? 'New'
      ( NonArrayTypeName ( OpenParenthesis ArgumentList? CloseParenthesis )? )?
      ObjectCreationExpressionInitializer?
      ImplementsClause? LineTerminator
    ;

InterfacePropertyMemberDeclaration
    : Attributes? InterfacePropertyModifier* PropertySignature StatementTerminator
    ;

AutoPropertyModifier
    : AccessModifier
    | 'Shadows'
    | 'Shared'
    | 'Overridable'
    | 'NotOverridable'
    | 'Overrides'
    | 'Overloads'
    ;

PropertyModifier
    : AutoPropertyModifier
    | 'Default'
    | 'ReadOnly'
    | 'WriteOnly'
    | 'Iterator'
    ;

MustOverridePropertyModifier
    : PropertyModifier
    | 'MustOverride'
    ;

InterfacePropertyModifier
    : 'Shadows'
    | 'Overloads'
    | 'Default'
    | 'ReadOnly'
    | 'WriteOnly'
    ;

PropertyAccessorDeclaration
    : PropertyGetDeclaration
    | PropertySetDeclaration
    ;
```

<span data-ttu-id="6e6ae-645">Im folgenden Beispiel wird die `Button` -Klasse definiert eine `Caption` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-645">In the example below, the `Button` class defines a `Caption` property.</span></span>

```vb
Public Class Button
    Private captionValue As String

    Public Property Caption() As String
        Get
            Return captionValue
        End Get

        Set (Value As String)
            captionValue = value
            Repaint()
        End Set
    End Property

    ...
End Class
```

<span data-ttu-id="6e6ae-646">Auf der Grundlage der `Button` oben gezeigte Klasse, die folgenden ist ein Beispiel für die Verwendung von der `Caption` Eigenschaft:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-646">Based on the `Button` class above, the following is an example of use of the `Caption` property:</span></span>

```vb
Dim okButton As Button = New Button()

okButton.Caption = "OK" ' Invokes Set accessor.
Dim s As String = okButton.Caption ' Invokes Get accessor.
```

<span data-ttu-id="6e6ae-647">Hier ist die `Set` -Accessor wird aufgerufen, durch das Zuweisen eines Werts der Eigenschaft und die `Get` Accessor wird aufgerufen, indem Sie auf die Eigenschaft in einem Ausdruck verweisen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-647">Here, the `Set` accessor is invoked by assigning a value to the property, and the `Get` accessor is invoked by referencing the property in an expression.</span></span>

<span data-ttu-id="6e6ae-648">Wenn kein Typ für eine Eigenschaft angegeben wird und strenge Semantik verwendet wird, tritt ein Fehler während der Kompilierung; Andernfalls ist der Typ der Eigenschaft implizit `Object` oder den Typ der Eigenschaft Typzeichen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-648">If no type is specified for a property and strict semantics are being used, a compile-time error occurs; otherwise the type of the property is implicitly `Object` or the type of the property's type character.</span></span> <span data-ttu-id="6e6ae-649">Eine Eigenschaftendeklaration darf entweder eine `Get` Accessor, der den Wert der Eigenschaft abgerufen wird, eine `Set` -Accessor, der den Wert der Eigenschaft oder sowohl speichert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-649">A property declaration may contain either a `Get` accessor, which retrieves the value of the property, a `Set` accessor, which stores the value of the property, or both.</span></span> <span data-ttu-id="6e6ae-650">Da eine Eigenschaft Methoden implizit deklariert wird, kann eine Eigenschaft mit den gleichen Modifizierern als eine Methode deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-650">Because a property implicitly declares methods, a property may be declared with the same modifiers as a method.</span></span> <span data-ttu-id="6e6ae-651">Wenn die Eigenschaft in einer Schnittstelle definiert wird, oder mit definiert die `MustOverride` Modifizierer, der den Eigenschaftentext und die `End` Konstrukt muss ausgelassen werden kann; andernfalls ein Kompilierungsfehler tritt auf.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-651">If the property is defined in an interface or defined with the `MustOverride` modifier, the property body and the `End` construct must be omitted; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="6e6ae-652">Die Parameterliste für den Index verwendet wird, bildet die Signatur der Eigenschaft, damit die Eigenschaften für Indexparameter jedoch nicht für den Typ der Eigenschaft überladen werden können.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-652">The index parameter list makes up the signature of the property, so properties may be overloaded on index parameters but not on the type of the property.</span></span> <span data-ttu-id="6e6ae-653">Die Liste der Indexparameter ist identisch mit dem eine normale Methode.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-653">The index parameter list is the same as for a regular method.</span></span> <span data-ttu-id="6e6ae-654">Keiner der Parameter kann jedoch geändert werden, mit der `ByRef` -Modifizierer und keine von ihnen unter Umständen werden mit dem Namen `Value` (vorbehalten für den impliziten Wertparameter in der `Set` Accessor).</span><span class="sxs-lookup"><span data-stu-id="6e6ae-654">However, none of the parameters may be modified with the `ByRef` modifier and none of them may be named `Value` (which is reserved for the implicit value parameter in the `Set` accessor).</span></span>

<span data-ttu-id="6e6ae-655">Eine Eigenschaft kann wie folgt deklariert werden:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-655">A property may be declared as follows:</span></span>

* <span data-ttu-id="6e6ae-656">Wenn die Eigenschaft keine Eigenschaft der Modifizierer gibt an, die Eigenschaft benötigen Sie Folgendes ein `Get` Accessor und einen `Set` Accessor.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-656">If the property specifies no property type modifier, the property must have both a `Get` accessor and a `Set` accessor.</span></span> <span data-ttu-id="6e6ae-657">Die Eigenschaft gilt eine Lese-/ Schreibzugriff-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-657">The property is said to be a read-write property.</span></span>

* <span data-ttu-id="6e6ae-658">Wenn die Eigenschaft gibt an, die `ReadOnly` Modifizierer die Eigenschaft müssen eine `Get` Accessor und dürfen keine `Set` Accessor.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-658">If the property specifies the `ReadOnly` modifier, the property must have a `Get` accessor and may not have a `Set` accessor.</span></span> <span data-ttu-id="6e6ae-659">Die Eigenschaft gilt nur-Lese Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-659">The property is said to be read-only property.</span></span> <span data-ttu-id="6e6ae-660">Es ist ein Fehler während der Kompilierung für eine schreibgeschützte Eigenschaft, die das Ziel einer Zuweisung sein.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-660">It is a compile-time error for a read-only property to be the target of an assignment.</span></span>

* <span data-ttu-id="6e6ae-661">Wenn die Eigenschaft gibt an, die `WriteOnly` Modifizierer die Eigenschaft müssen eine `Set` Accessor und dürfen keine `Get` Accessor.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-661">If the property specifies the `WriteOnly` modifier, the property must have a `Set` accessor and may not have a `Get` accessor.</span></span> <span data-ttu-id="6e6ae-662">Die Eigenschaft gilt nur-Schreiben-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-662">The property is said to be write-only property.</span></span> <span data-ttu-id="6e6ae-663">Es ist ein Fehler während der Kompilierung, um eine Nur-Schreiben-Eigenschaft in einem Ausdruck außer als Ziel einer Zuweisung oder als Argument an eine Methode zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-663">It is a compile-time error to reference a write-only property in an expression except as the target of an assignment or as an argument to a method.</span></span>

<span data-ttu-id="6e6ae-664">Die `Get` und `Set` Accessor einer Eigenschaft sind nicht unterschiedliche Elemente aus, und es ist nicht möglich, um die Accessoren der Eigenschaft separat zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-664">The `Get` and `Set` accessors of a property are not distinct members, and it is not possible to declare the accessors of a property separately.</span></span> <span data-ttu-id="6e6ae-665">Im folgende Beispiel wird eine einzelne Lese-/ Schreibzugriff-Eigenschaft nicht deklarieren.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-665">The following example does not declare a single read-write property.</span></span> <span data-ttu-id="6e6ae-666">Stattdessen deklariert zwei Eigenschaften mit dem gleichen Namen, eine schreibgeschützte und lesegeschützte:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-666">Rather, it declares two properties with the same name, one read-only and one write-only:</span></span>

```vb
Class A
    Private nameValue As String

    ' Error, contains a duplicate member name.
    Public ReadOnly Property Name() As String 
        Get
            Return nameValue
        End Get
    End Property

    ' Error, contains a duplicate member name.
    Public WriteOnly Property Name() As String 
        Set (Value As String)
            nameValue = value
        End Set
    End Property
End Class
```

<span data-ttu-id="6e6ae-667">Da zwei Elemente, die in der gleichen Klasse deklariert den gleichen Namen haben können, wird im Beispiel wird einen Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-667">Since two members declared in the same class cannot have the same name, the example causes a compile-time error.</span></span>

<span data-ttu-id="6e6ae-668">Standardmäßig wird der Zugriff auf einer Eigenschaft des `Get` und `Set` Accessoren entspricht dem der Zugriff auf die Eigenschaft selbst.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-668">By default, the accessibility of a property's `Get` and `Set` accessors is the same as the accessibility of the property itself.</span></span> <span data-ttu-id="6e6ae-669">Allerdings die `Get` und `Set` Accessoren können auch angeben, Barrierefreiheit, getrennt von der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-669">However, the `Get` and `Set` accessors can also specify accessibility separately from the property.</span></span> <span data-ttu-id="6e6ae-670">In diesem Fall wird der Zugriff auf einen Accessor muss restriktiver sein als die Zugriffsebene der Eigenschaft, und nur einen Accessor haben eine andere Zugriffsebene aus der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-670">In that case, the accessibility of an accessor must be more restrictive than the accessibility of the property and only one accessor can have a different accessibility level from the property.</span></span> <span data-ttu-id="6e6ae-671">Zugriffstypen werden mehr oder weniger restriktive wie folgt berücksichtigt:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-671">Access types are considered more or less restrictive as follows:</span></span>

* <span data-ttu-id="6e6ae-672">`Private` ist stärker eingeschränkt als `Public`, `Protected Friend`, `Protected`, oder `Friend`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-672">`Private` is more restrictive than `Public`, `Protected Friend`, `Protected`, or `Friend`.</span></span>

* <span data-ttu-id="6e6ae-673">`Friend` ist stärker eingeschränkt als `Protected Friend` oder `Public`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-673">`Friend` is more restrictive than `Protected Friend` or `Public`.</span></span>

* <span data-ttu-id="6e6ae-674">`Protected` ist stärker eingeschränkt als `Protected Friend` oder `Public`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-674">`Protected` is more restrictive than `Protected Friend` or `Public`.</span></span>

* <span data-ttu-id="6e6ae-675">`Protected Friend` ist stärker eingeschränkt als `Public`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-675">`Protected Friend` is more restrictive than `Public`.</span></span>

<span data-ttu-id="6e6ae-676">Wenn eine der die Zugriffsmethoden einer Eigenschaft zugegriffen werden kann, aber der andere Controller nicht ist, wird die Eigenschaft behandelt, als wäre es schreibgeschützt oder lesegeschützt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-676">When one of a property's accessors is accessible but the other one is not, the property is treated as if it was read-only or write-only.</span></span> <span data-ttu-id="6e6ae-677">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-677">For example:</span></span>

```vb
Class A
    Public Property P() As Integer
        Get
            ...
        End Get

        Private Set (Value As Integer)
            ...
        End Set
    End Property
End Class

Module Test
    Sub Main()
        Dim a As A = New A()

        ' Error: A.P is read-only in this context.
        a.P = 10
    End Sub
End Module
```

<span data-ttu-id="6e6ae-678">Wenn ein abgeleiteter Typ führt Shadowing für eine Eigenschaft, blendet die abgeleitete Eigenschaft die Shadowing-Eigenschaft in Bezug auf das Lesen und schreiben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-678">When a derived type shadows a property, the derived property hides the shadowed property with respect to both reading and writing.</span></span> <span data-ttu-id="6e6ae-679">Im folgenden Beispiel die `P` -Eigenschaft in `B` Blendet die `P` -Eigenschaft in `A` in Bezug auf das Lesen und schreiben:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-679">In the following example, the `P` property in `B` hides the `P` property in `A` with respect to both reading and writing:</span></span>

```vb
Class A
    Public WriteOnly Property P() As Integer
        Set (Value As Integer)
        End Set
    End Property
End Class

Class B
    Inherits A

    Public Shadows ReadOnly Property P() As Integer
       Get
       End Get
    End Property
End Class

Module Test
    Sub Main()
        Dim x As B = New B

        B.P = 10     ' Error, B.P is read-only.
    End Sub
End Module
```

<span data-ttu-id="6e6ae-680">Die Zugriffsdomäne des Rückgabetyp oder Parametertypen muss identisch oder eine Obermenge der Zugriffsdomäne von der Eigenschaft selbst sein.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-680">The accessibility domain of the return type or parameter types must be the same as or a superset of the accessibility domain of the property itself.</span></span> <span data-ttu-id="6e6ae-681">Eine Eigenschaft kann nur eine haben `Set` -Accessor und einen `Get` Accessor.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-681">A property may only have one `Set` accessor and one `Get` accessor.</span></span>

<span data-ttu-id="6e6ae-682">Mit Ausnahme der Unterschiede in der Deklarations- und Aufrufsyntax Syntax `Overridable`, `NotOverridable`, `Overrides`, `MustOverride`, und `MustInherit` Eigenschaften verhalten sich genau wie `Overridable`, `NotOverridable`, `Overrides`, `MustOverride`, und `MustInherit` Methoden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-682">Except for differences in declaration and invocation syntax, `Overridable`, `NotOverridable`, `Overrides`, `MustOverride`, and `MustInherit` properties behave exactly like `Overridable`, `NotOverridable`, `Overrides`, `MustOverride`, and `MustInherit` methods.</span></span> <span data-ttu-id="6e6ae-683">Wenn eine Eigenschaft überschrieben wird, muss die überschreibende Eigenschaft desselben Typs (Lese-/ Schreibzugriff, schreibgeschützten, lesegeschützten) sein.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-683">When a property is overridden, the overriding property must be of the same type (read-write, read-only, write-only).</span></span> <span data-ttu-id="6e6ae-684">Ein `Overridable` Eigenschaft darf nicht enthalten eine `Private` Accessor.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-684">An `Overridable` property cannot contain a `Private` accessor.</span></span>

<span data-ttu-id="6e6ae-685">Im folgenden Beispiel `X` ist ein `Overridable` schreibgeschützte Eigenschaft, `Y` ist ein `Overridable` Lese-/ Schreibzugriff-Eigenschaft und `Z` ist eine `MustOverride` Lese-/ Schreibzugriff-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-685">In the following example `X` is an `Overridable` read-only property, `Y` is an `Overridable` read-write property, and `Z` is a `MustOverride` read-write property.</span></span>

```vb
MustInherit Class A
    Private _y As Integer

    Public Overridable ReadOnly Property X() As Integer
        Get
            Return 0
        End Get
    End Property

    Public Overridable Property Y() As Integer
        Get
            Return _y
         End Get
        Set (Value As Integer)
            _y = value
        End Set
    End Property

    Public MustOverride Property Z() As Integer
End Class
```

<span data-ttu-id="6e6ae-686">Da `Z` ist `MustOverride`, wird die enthaltende Klasse `A` muss deklariert werden `MustInherit`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-686">Because `Z` is `MustOverride`, the containing class `A` must be declared `MustInherit`.</span></span>

<span data-ttu-id="6e6ae-687">Im Gegensatz dazu eine Klasse, die von Klasse abgeleitet ist `A` ist unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-687">By contrast, a class that derives from class `A` is shown below:</span></span>

```vb
Class B
    Inherits A

    Private _z As Integer

    Public Overrides ReadOnly Property X() As Integer
        Get
            Return MyBase.X + 1
        End Get
    End Property

    Public Overrides Property Y() As Integer
        Get
            Return MyBase.Y
        End Get
        Set (Value As Integer)
            If value < 0 Then
                MyBase.Y = 0
            Else
                MyBase.Y = Value
            End If
        End Set
    End Property

    Public Overrides Property Z() As Integer
        Get
            Return _z
        End Get
        Set (Value As Integer)
            _z = Value
        End Set
    End Property
End Class
```

<span data-ttu-id="6e6ae-688">Hier wird die Deklarationen der Eigenschaften `X`,`Y`, und `Z` überschreiben Sie die grundlegenden Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-688">Here, the declarations of properties `X`,`Y`, and `Z` override the base properties.</span></span> <span data-ttu-id="6e6ae-689">Jede Eigenschaftendeklaration entspricht genau der Zugriffsmodifizierer, Typ und Namen der entsprechenden geerbte Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-689">Each property declaration exactly matches the accessibility modifiers, type, and name of the corresponding inherited property.</span></span> <span data-ttu-id="6e6ae-690">Die `Get` -Accessor der Eigenschaft `X` und `Set` -Accessor der Eigenschaft `Y` verwenden die `MyBase` Schlüsselwort, um die geerbten Eigenschaften zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-690">The `Get` accessor of property `X` and the `Set` accessor of property `Y` use the `MyBase` keyword to access the inherited properties.</span></span> <span data-ttu-id="6e6ae-691">Die Deklaration der Eigenschaft `Z` überschreibt die `MustOverride` Eigenschaft – es gibt also keine ausstehenden `MustOverride` Member in Klasse `B`, und `B` ist zulässig, werden von einer normalen Klasse.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-691">The declaration of property `Z` overrides the `MustOverride` property -- thus, there are no outstanding `MustOverride` members in class `B`, and `B` is permitted to be a regular class.</span></span>

<span data-ttu-id="6e6ae-692">Eigenschaften können verwendet werden, um die Initialisierung einer Ressource bis zum Moment zu verzögern, die es erstmalig verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-692">Properties can be used to delay initialization of a resource until the moment it is first referenced.</span></span> <span data-ttu-id="6e6ae-693">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-693">For example:</span></span>

```vb
Imports System.IO

Public Class ConsoleStreams
    Private Shared reader As TextReader
    Private Shared writer As TextWriter
    Private Shared errors As TextWriter

    Public Shared ReadOnly Property [In]() As TextReader
        Get
            If reader Is Nothing Then
                reader = Console.In
            End If
            Return reader
        End Get
    End Property

    Public Shared ReadOnly Property Out() As TextWriter
        Get
            If writer Is Nothing Then
                writer = Console.Out
            End If
            Return writer
        End Get
    End Property

    Public Shared ReadOnly Property [Error]() As TextWriter
        Get
            If errors Is Nothing Then
                errors = Console.Error
            End If
            Return errors
        End Get
    End Property
End Class
```

<span data-ttu-id="6e6ae-694">Die `ConsoleStreams` -Klasse enthält drei Eigenschaften `In`, `Out`, und `Error`, die Eingabe, Ausgabe und Fehler Geräten bzw. darstellen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-694">The `ConsoleStreams` class contains three properties, `In`, `Out`, and `Error`, that represent the standard input, output, and error devices, respectively.</span></span> <span data-ttu-id="6e6ae-695">Durch diese Member als Eigenschaften verfügbar macht, die `ConsoleStreams` Klasse kann die Initialisierung verzögert, bis sie tatsächlich verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-695">By exposing these members as properties, the `ConsoleStreams` class can delay their initialization until they are actually used.</span></span> <span data-ttu-id="6e6ae-696">Z. B. bei der ersten Verweis auf die `Out` Eigenschaft, wie im `ConsoleStreams.Out.WriteLine("hello, world")`, den zugrunde liegenden `TextWriter` für das Ausgabegerät initialisiert wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-696">For example, upon first referencing the `Out` property, as in `ConsoleStreams.Out.WriteLine("hello, world")`, the underlying `TextWriter` for the output device is initialized.</span></span> <span data-ttu-id="6e6ae-697">Aber wenn die Anwendung keinen Verweis auf die `In` und `Error` Eigenschaften, keine Objekte für diese Geräte erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-697">But if the application makes no reference to the `In` and `Error` properties, then no objects are created for those devices.</span></span>


### <a name="get-accessor-declarations"></a><span data-ttu-id="6e6ae-698">Get Accessordeklarationen</span><span class="sxs-lookup"><span data-stu-id="6e6ae-698">Get Accessor Declarations</span></span>

<span data-ttu-id="6e6ae-699">Ein `Get` Accessors (Getters) deklariert wird, indem Sie eine Eigenschaft `Get` Deklaration.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-699">A `Get` accessor (getter) is declared by using a property `Get` declaration.</span></span> <span data-ttu-id="6e6ae-700">Eine Eigenschaft `Get` besteht aus der Deklaration des Schlüsselworts `Get` gefolgt von einem Anweisungsblock.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-700">A property `Get` declaration consists of the keyword `Get` followed by a statement block.</span></span> <span data-ttu-id="6e6ae-701">Erhalten eine Eigenschaft namens `P`, `Get` Zugriffsmethoden-Deklaration deklariert implizit eine Methode mit dem Namen `get_P` mit dem gleichen Modifizierer, den Typ und die Parameterliste als Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-701">Given a property named `P`, a `Get` accessor declaration implicitly declares a method with the name `get_P` with the same modifiers, type, and parameter list as the property.</span></span> <span data-ttu-id="6e6ae-702">Wenn der Typ eine Deklaration mit diesem Namen enthält, die implizite Deklaration wird ignoriert, für die Zwecke namensbindung ergibt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-702">If the type contains a declaration with that name, a compile-time error results, but the implicit declaration is ignored for the purposes of name binding.</span></span>

<span data-ttu-id="6e6ae-703">Eine spezielle lokale Variable, die implizit in deklariert ist die `Get` Deklarationsabschnitt der Accessor-Body, mit dem gleichen Namen wie die Eigenschaft stellt den Rückgabewert der Eigenschaft dar.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-703">A special local variable, which is implicitly declared in the `Get` accessor body's declaration space with the same name as the property, represents the return value of the property.</span></span> <span data-ttu-id="6e6ae-704">Die lokale Variable ist sondername Auflösung Semantik, wenn in Ausdrücken verwendet.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-704">The local variable has special name resolution semantics when used in expressions.</span></span> <span data-ttu-id="6e6ae-705">Wenn die lokale Variable in einem Kontext verwendet wird, die einen Ausdruck erwartet, der als eine Methodengruppe, z. B. ein Aufrufausdruck klassifiziert ist, löst den Namen an die Funktion und nicht an die lokale Variable.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-705">If the local variable is used in a context that expects an expression that is classified as a method group, such as an invocation expression, then the name resolves to the function rather than to the local variable.</span></span> <span data-ttu-id="6e6ae-706">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-706">For example:</span></span>

```vb
ReadOnly Property F(i As Integer) As Integer
    Get
        If i = 0 Then
            F = 1    ' Sets the return value.
        Else
            F = F(i - 1) ' Recursive call.
        End If
    End Get
End Property
```

<span data-ttu-id="6e6ae-707">Die Verwendung von Klammern kann dazu führen, dass mehrdeutige Situationen (z. B. `F(1)` , in denen `F` ist eine Eigenschaft, deren Typ ein eindimensionales Array ist).</span><span class="sxs-lookup"><span data-stu-id="6e6ae-707">The use of parentheses can cause ambiguous situations (such as `F(1)` where `F` is a property whose type is a one-dimensional array).</span></span> <span data-ttu-id="6e6ae-708">In allen Situationen nicht eindeutig löst den Namen der Eigenschaft anstelle der lokalen Variablen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-708">In all ambiguous situations, the name resolves to the property rather than the local variable.</span></span> <span data-ttu-id="6e6ae-709">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-709">For example:</span></span>

```vb
ReadOnly Property F(i As Integer) As Integer()
    Get
        If i = 0 Then
            F = new Integer(2) { 1, 2, 3 }
        Else
            F = F(i - 1) ' Recursive call, not index.
        End If
    End Get
End Property
```

<span data-ttu-id="6e6ae-710">Wenn Steuerelement Flow / / Blätter der `Get` Accessortext hinausgehen, der Wert der lokalen Variablen wird an der Aufrufausdruck übergeben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-710">When control flow leaves the `Get` accessor body, the value of the local variable is passed back to the invocation expression.</span></span> <span data-ttu-id="6e6ae-711">Da Aufrufen einer `Get` Accessor Konzept entspricht dem Lesen des Werts einer Variablen, gilt dies Unzulässiger Programmierstil für `Get` Accessoren Observable Nebeneffekte haben, wie im folgenden Beispiel dargestellt:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-711">Because invoking a `Get` accessor is conceptually equivalent to reading the value of a variable, it is considered bad programming style for `Get` accessors to have observable side effects, as illustrated in the following example:</span></span>

```vb
Class Counter
    Private Value As Integer

    Public ReadOnly Property NextValue() As Integer
        Get
            Value += 1
            Return Value
        End Get
    End Property
End Class
```

<span data-ttu-id="6e6ae-712">Der Wert des der `NextValue` Eigenschaft hängt die Anzahl der Male, die die Eigenschaft zuvor zugegriffen wurde.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-712">The value of the `NextValue` property depends on the number of times the property has previously been accessed.</span></span> <span data-ttu-id="6e6ae-713">Klicken Sie daher Zugriff auf die Eigenschaft einen Observable Nebeneffekt erzeugt, und die Eigenschaft sollte stattdessen als eine Methode implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-713">Thus, accessing the property produces an observable side effect, and the property should instead be implemented as a method.</span></span>

<span data-ttu-id="6e6ae-714">Die "keine Nebeneffekte" Konvention für `Get` Accessoren bedeutet nicht, dass `Get` Accessoren sollte stets so verfasst werden einfach in Variablen gespeicherten Werte zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-714">The "no side effects" convention for `Get` accessors does not mean that `Get` accessors should always be written to simply return values stored in variables.</span></span> <span data-ttu-id="6e6ae-715">In der Tat `Get` Accessoren häufig berechnen Sie den Wert einer Eigenschaft, indem Sie Zugriff auf mehrere Variablen oder Methoden aufrufen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-715">Indeed, `Get` accessors often compute the value of a property by accessing multiple variables or invoking methods.</span></span> <span data-ttu-id="6e6ae-716">Allerdings ein ordnungsgemäß entwickeltes `Get` Accessor führt keine Aktionen, die dazu führen, wahrnehmbare Änderungen in den Zustand des Objekts dass.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-716">However, a properly designed `Get` accessor performs no actions that cause observable changes in the state of the object.</span></span>

<span data-ttu-id="6e6ae-717">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="6e6ae-717">__Note.__</span></span> <span data-ttu-id="6e6ae-718">`Get` Accessoren haben die gleiche Einschränkung Zeile Platzierung, die Unterroutinen aufweisen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-718">`Get` accessors have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="6e6ae-719">Der Anfang-Anweisung, die End-Anweisung und die Block müssen alle am Anfang einer logischen Zeile angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-719">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

```antlr
PropertyGetDeclaration
    : Attributes? AccessModifier? 'Get' LineTerminator
      Block?
      'End' 'Get' StatementTerminator
    ;
```

### <a name="set-accessor-declarations"></a><span data-ttu-id="6e6ae-720">Set-Accessor-Deklarationen</span><span class="sxs-lookup"><span data-stu-id="6e6ae-720">Set Accessor Declarations</span></span>

<span data-ttu-id="6e6ae-721">Ein `Set` Accessor (Setter) deklariert wird, indem Sie eine Eigenschaftsdeklaration für die Gruppe.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-721">A `Set` accessor (setter) is declared by using a property set declaration.</span></span> <span data-ttu-id="6e6ae-722">Eine Eigenschaftsdeklaration für die Gruppe besteht aus dem Schlüsselwort `Set`, eine Liste optionaler Parameter, und einen Anweisungsblock.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-722">A property set declaration consists of the keyword `Set`, an optional parameter list, and a statement block.</span></span> <span data-ttu-id="6e6ae-723">Erhalten eine Eigenschaft namens `P`, eine Setter-Deklaration deklariert implizit eine Methode mit dem Namen `set_P` mit dem gleichen Modifizierer und der Parameterliste als Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-723">Given a property named `P`, a setter declaration implicitly declares a method with the name `set_P` with the same modifiers and parameter list as the property.</span></span> <span data-ttu-id="6e6ae-724">Wenn der Typ eine Deklaration mit diesem Namen enthält, die implizite Deklaration wird ignoriert, für die Zwecke namensbindung ergibt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-724">If the type contains a declaration with that name, a compile-time error results, but the implicit declaration is ignored for the purposes of name binding.</span></span>

<span data-ttu-id="6e6ae-725">Bei Verwendung eine Parameterliste angegeben wird, muss einen Member, in diesen Member müssen keine Modifizierer mit Ausnahme von `ByVal`, und der Typ muss den Typ der Eigenschaft identisch sein.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-725">If a parameter list is specified, it must have one member, that member must have no modifiers except `ByVal`, and its type must be the same as the type of the property.</span></span> <span data-ttu-id="6e6ae-726">Der Parameter repräsentiert den Wert der Eigenschaft festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-726">The parameter represents the property value being set.</span></span> <span data-ttu-id="6e6ae-727">Wenn der Parameter ausgelassen wird, ein Parameter mit dem Namen `Value` wird implizit deklariert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-727">If the parameter is omitted, a parameter named `Value` is implicitly declared.</span></span>

<span data-ttu-id="6e6ae-728">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="6e6ae-728">__Note.__</span></span> <span data-ttu-id="6e6ae-729">`Set` Accessoren haben die gleiche Einschränkung Zeile Platzierung, die Unterroutinen aufweisen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-729">`Set` accessors have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="6e6ae-730">Der Anfang-Anweisung, die End-Anweisung und die Block müssen alle am Anfang einer logischen Zeile angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-730">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

```antlr
PropertySetDeclaration
    : Attributes? AccessModifier? 'Set'
      ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block?
      'End' 'Set' StatementTerminator
    ;
```

### <a name="default-properties"></a><span data-ttu-id="6e6ae-731">Standardeigenschaften</span><span class="sxs-lookup"><span data-stu-id="6e6ae-731">Default Properties</span></span>

<span data-ttu-id="6e6ae-732">Eine Eigenschaft, die den Modifizierer gibt an, `Default` heißt eine *Standardeigenschaft*.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-732">A property that specifies the modifier `Default` is called a *default property*.</span></span> <span data-ttu-id="6e6ae-733">Jeder Typ, der Eigenschaften kann möglicherweise eine Standardeigenschaft, einschließlich der Schnittstellen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-733">Any type that allows properties may have a default property, including interfaces.</span></span> <span data-ttu-id="6e6ae-734">Die Standardeigenschaft kann verwiesen werden, ohne die Instanz mit dem Namen der Eigenschaft zu qualifizieren.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-734">The default property may be referenced without having to qualify the instance with the name of the property.</span></span> <span data-ttu-id="6e6ae-735">Daher erhalten eine Klasse</span><span class="sxs-lookup"><span data-stu-id="6e6ae-735">Thus, given a class</span></span>

```vb
Class Test
    Public Default ReadOnly Property Item(i As Integer) As Integer
        Get
            Return i
        End Get
    End Property
End Class
```

<span data-ttu-id="6e6ae-736">Der code</span><span class="sxs-lookup"><span data-stu-id="6e6ae-736">the code</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test()
        Dim y As Integer

        y = x(10)
    End Sub
End Module
```

<span data-ttu-id="6e6ae-737">für die folgende Syntax:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-737">is equivalent to</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test()
        Dim y As Integer

        y = x.Item(10)
    End Sub
End Module
```

<span data-ttu-id="6e6ae-738">Nachdem eine Eigenschaft deklariert ist `Default`, werden alle auf diesen Namen in der Vererbungshierarchie überladenen Eigenschaften der Standardeigenschaft, ob sie deklariert wurden `Default` oder nicht.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-738">Once a property is declared `Default`, all of the properties overloaded on that name in the inheritance hierarchy become the default property, whether they have been declared `Default` or not.</span></span> <span data-ttu-id="6e6ae-739">Deklarieren einer Eigenschaft `Default` in einer abgeleiteten Klasse, wenn die Basisklasse der Klasse deklariert eine Standardeigenschaft unter einem anderen Namen erfordert keine andere Modifizierer wie z. B. `Shadows` oder `Overrides`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-739">Declaring a property `Default` in a derived class when the base class declared a default property by another name does not require any other modifiers such as `Shadows` or `Overrides`.</span></span> <span data-ttu-id="6e6ae-740">Dies ist da die Standardeigenschaft keine Identitäten und keine Signatur vorhanden ist und daher schattiert oder überladen werden kann.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-740">This is because the default property has no identity or signature and so cannot be shadowed or overloaded.</span></span> <span data-ttu-id="6e6ae-741">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-741">For example:</span></span>

```vb
Class Base
    Public ReadOnly Default Property Item(i As Integer) As Integer
        Get
            Console.WriteLine("Base = " & i)
        End Get
    End Property
End Class

Class Derived
    Inherits Base

    ' This hides Item, but does not change the default property.
    Public Shadows ReadOnly Property Item(i As Integer) As Integer
        Get
            Console.WriteLine("Derived = " & i)
        End Get
    End Property
End Class

Class MoreDerived
    Inherits Derived

    ' This declares a new default property, but not Item.
    ' This does not need to be declared Shadows
    Public ReadOnly Default Property Value(i As Integer) As Integer
        Get
            Console.WriteLine("MoreDerived = " & i)
        End Get
    End Property
End Class

Module Test
    Sub Main()
        Dim x As MoreDerived = New MoreDerived()
        Dim y As Integer
        Dim z As Derived = x

        y = x(10)        ' Calls MoreDerived.Value.
        y = x.Item(10)   ' Calls Derived.Item
        y = z(10)        ' Calls Base.Item
    End Sub
End Module
```

<span data-ttu-id="6e6ae-742">Dieses Programm wird die Ausgabe erzeugen:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-742">This program will produce the output:</span></span>

```
MoreDerived = 10
Derived = 10
Base = 10
```

<span data-ttu-id="6e6ae-743">Alle Standard-Eigenschaften, die innerhalb eines Typs deklariert muss den gleichen Namen aufweisen und müssen aus Gründen der Übersichtlichkeit angeben der `Default` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-743">All default properties declared within a type must have the same name and, for clarity, must specify the `Default` modifier.</span></span> <span data-ttu-id="6e6ae-744">Da eine Standardeigenschaft ohne Index-Parameter eine mehrdeutige Situation führen würde, wenn Instanzen von der enthaltenden Klasse zuweisen, müssen die Standardeigenschaften Indexparameter aufweisen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-744">Because a default property with no index parameters would cause an ambiguous situation when assigning instances of the containing class, default properties must have index parameters.</span></span> <span data-ttu-id="6e6ae-745">Darüber hinaus, wenn Überladen einer Eigenschaft auf ein bestimmter Namen umfasst die `Default` Modifizierer verwenden, alle auf diesen Namen überladene Eigenschaften angegeben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-745">Furthermore, if one property overloaded on a particular name includes the `Default` modifier, all properties overloaded on that name must specify it.</span></span> <span data-ttu-id="6e6ae-746">Standardeigenschaften möglicherweise nicht `Shared`, und mindestens einen Accessor der Eigenschaft darf nicht sein `Private`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-746">Default properties may not be `Shared`, and at least one accessor of the property must not be `Private`.</span></span>

### <a name="automatically-implemented-properties"></a><span data-ttu-id="6e6ae-747">Automatisch implementierte Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="6e6ae-747">Automatically Implemented Properties</span></span>

<span data-ttu-id="6e6ae-748">Falls eine Eigenschaft alle Accessoren Deklaration weggelassen wird, eine Implementierung der Eigenschaft wird automatisch bereitgestellt werden, wenn die Eigenschaft in einer Schnittstelle deklariert wird oder deklariert `MustOverride`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-748">If a property omits declaration of any accessors, an implementation of the property will be automatically supplied unless the property is declared in an interface or is declared `MustOverride`.</span></span> <span data-ttu-id="6e6ae-749">Nur Lese-/Schreibeigenschaften ohne Argumente können automatisch implementiert werden; andernfalls tritt ein Kompilierungsfehler auf.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-749">Only read/write properties with no arguments can be automatically implemented; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="6e6ae-750">Eine automatisch implementierte Eigenschaft `x`, sogar einer anderen Eigenschaft zu überschreiben, führt eine private lokale Variable `_x` mit dem gleichen Typ wie die Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-750">An automatically implemented property `x`, even one overriding another property, introduces a private local variable `_x` with the same type as the property.</span></span> <span data-ttu-id="6e6ae-751">Liegt ein Konflikt zwischen der lokalen Variablen-Namen und eine andere Deklaration vor, wird ein Kompilierzeitfehler gemeldet.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-751">If there is a collision between the local variable's name and another declaration, a compile-time error will be reported.</span></span> <span data-ttu-id="6e6ae-752">Der automatisch implementierten Eigenschaft `Get` Accessor gibt den Wert der lokalen und der Eigenschaft `Set` Accessor, der den Wert der lokalen festlegt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-752">The automatically implemented property's `Get` accessor returns the value of the local and the property's `Set` accessor that sets the value of the local.</span></span> <span data-ttu-id="6e6ae-753">Beispielsweise ist die Deklaration:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-753">For example, the declaration:</span></span>

```vb
Public Property x() As Integer
```

<span data-ttu-id="6e6ae-754">ist ungefähr gleich ist:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-754">is roughly equivalent to:</span></span>

```vb
Private _x As Integer
Public Property x() As Integer
    Get
        Return _x
    End Get
    Set (value As Integer)
        _x = value
    End Set
End Property
```

<span data-ttu-id="6e6ae-755">Wie bei Variablendeklarationen, kann eine automatisch implementierte Eigenschaft einen Initialisierer enthalten.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-755">As with variable declarations, an automatically implemented property can include an initializer.</span></span> <span data-ttu-id="6e6ae-756">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-756">For example:</span></span>

```vb
Public Property x() As Integer = 10
Public Shared Property y() As New Customer() With { .Name = "Bob" }
```

<span data-ttu-id="6e6ae-757">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="6e6ae-757">__Note.__</span></span> <span data-ttu-id="6e6ae-758">Wenn eine automatisch implementierte Eigenschaft initialisiert wird, wird er durch die Eigenschaft, nicht das zugrunde liegende Feld initialisiert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-758">When an automatically implemented property is initialized, it is initialized through the property, not the underlying field.</span></span> <span data-ttu-id="6e6ae-759">Dies ist daher Überschreiben von Eigenschaften die Initialisierung abfangen kann, wenn dies erforderlich.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-759">This is so overriding properties can intercept the initialization if they need to.</span></span>

<span data-ttu-id="6e6ae-760">Arrayinitialisierer sind für automatisch implementierte Eigenschaften zulässig, außer dass es keine Möglichkeit gibt, die Arraygrenzen explizit angeben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-760">Array initializers are allowed on automatically implemented properties, except that there is no way to specify the array bounds explicitly.</span></span>  <span data-ttu-id="6e6ae-761">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-761">For example:</span></span>

```vb
' Valid
Property x As Integer() = {1, 2, 3}
Property y As Integer(,) = {{1, 2, 3}, {12, 13, 14}, {11, 10, 9}}

' Invalid
Property x4(5) As Short
```

### <a name="iterator-properties"></a><span data-ttu-id="6e6ae-762">Iterator-Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="6e6ae-762">Iterator Properties</span></span>

<span data-ttu-id="6e6ae-763">Ein *Iterator Eigenschaft* ist eine Eigenschaft mit dem `Iterator` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-763">An *iterator property* is a property with the `Iterator` modifier.</span></span> <span data-ttu-id="6e6ae-764">Dient aus demselben Grund eine Iteratormethode (Abschnitt [Iteratormethoden](statements.md#iterator-methods)) verwendet wird – als einfache Möglichkeit zum Generieren einer Sequenz, die von genutzt werden kann die `For Each` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-764">It is used for the same reason an iterator method (Section [Iterator Methods](statements.md#iterator-methods)) is used -- as a convenient way to generate a sequence, one which can be consumed by the `For Each` statement.</span></span> <span data-ttu-id="6e6ae-765">Die `Get` Accessor ein Iterator-Eigenschaft wird auf die gleiche Weise wie eine Iteratormethode interpretiert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-765">The `Get` accessor of an iterator property is interpreted in the same way as an iterator method.</span></span>

<span data-ttu-id="6e6ae-766">Eine Iterator-Eigenschaft muss einen expliziten haben `Get` -Accessor, und der Typ muss `IEnumerator`, oder `IEnumerable`, oder `IEnumerator(Of T)` oder `IEnumerable(Of T)` für einige `T`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-766">An iterator property must have an explicit `Get` accessor, and its type must be `IEnumerator`, or `IEnumerable`, or `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`.</span></span>

<span data-ttu-id="6e6ae-767">Hier ist ein Beispiel für einen Iterator-Eigenschaft:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-767">Here is an example of an iterator property:</span></span>

```vb
Class Family
    Property Daughters As New List(Of String) From {"Beth", "Diane"}
    Property Sons As New List(Of String) From {"Abe", "Carl"}

    ReadOnly Iterator Property Children As IEnumerable(Of String)
        Get
            For Each name In Daughters : Yield name : Next
            For Each name In Sons : Yield name : Next
        End Get
    End Property
End Class

Module Module1
    Sub Main()
        Dim x As New Family
        For Each c In x.Children
            Console.WriteLine(c) ' prints Beth, Diane, Abe, Carl
        Next
    End Sub
End Module
```

## <a name="operators"></a><span data-ttu-id="6e6ae-768">Operatoren</span><span class="sxs-lookup"><span data-stu-id="6e6ae-768">Operators</span></span>

<span data-ttu-id="6e6ae-769">*Operatoren* sind Methoden, die die Bedeutung eines vorhandenen Visual Basic-Operators für die enthaltende Klasse zu definieren.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-769">*Operators* are methods that define the meaning of an existing Visual Basic operator for the containing class.</span></span> <span data-ttu-id="6e6ae-770">Wenn der Operator auf die Klasse in einem Ausdruck angewendet wird, wird der Operator in einen Aufruf der Standardabfrageoperator-Methode, die in der Klasse definiert kompiliert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-770">When the operator is applied to the class in an expression, the operator is compiled into a call to the operator method defined in the class.</span></span> <span data-ttu-id="6e6ae-771">Definieren einen Operator aus, für eine Klasse, auch bekannt als ist *überladen* den Operator.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-771">Defining an operator for a class is also known as *overloading* the operator.</span></span>

```antlr
OperatorDeclaration
    : Attributes? OperatorModifier* 'Operator' OverloadableOperator
      OpenParenthesis ParameterList CloseParenthesis
      ( 'As' Attributes? TypeName )? LineTerminator
      Block?
      'End' 'Operator' StatementTerminator
    ;

OperatorModifier
    : 'Public' | 'Shared' | 'Overloads' | 'Shadows' | 'Widening' | 'Narrowing'
    ;

OverloadableOperator
    : '+' | '-' | '*' | '/' | '\\' | '&' | 'Like' | 'Mod' | 'And' | 'Or' | 'Xor'
    | '^' | '<' '<' | '>' '>' | '=' | '<' '>' | '>' | '<' | '>' '=' | '<' '='
    | 'Not' | 'IsTrue' | 'IsFalse' | 'CType'
    ;
```

<span data-ttu-id="6e6ae-772">Es ist nicht möglich, einen Operator überladen, der bereits vorhanden ist; in der Praxis ist gilt dies vor allem für Konvertierungsoperatoren.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-772">It is not possible to overload an operator that already exists; in practice, this primarily applies to conversion operators.</span></span> <span data-ttu-id="6e6ae-773">Beispielsweise ist es nicht möglich, die die Konvertierung von einer abgeleiteten Klasse in einer Basisklasse überladen:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-773">For example, it is not possible to overload the conversion from a derived class to a base class:</span></span>

```vb
Class Base
End Class

Class Derived
    ' Cannot redefine conversion from Derived to Base,
    ' conversion will be ignored.
    Public Shared Widening Operator CType(s As Derived) As Base
        ...
    End Operator
End Class
```

<span data-ttu-id="6e6ae-774">Operatoren können auch im allgemeinen Sinn des Wortes überladen werden:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-774">Operators can also be overloaded in the common sense of the word:</span></span>

```vb
Class Base
    Public Shared Widening Operator CType(b As Base) As Integer
        ...
    End Operator

    Public Shared Narrowing Operator CType(i As Integer) As Base
        ...
    End Operator
End Class
```

<span data-ttu-id="6e6ae-775">Operatordeklarationen sind Namen nicht explizit Deklarationsabschnitt des enthaltenden Typs hinzufügen. jedoch sollten sie eine entsprechende Methode, das die Zeichen "Op_" implizit deklarieren.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-775">Operator declarations do not explicitly add names to the containing type's declaration space; however they do implicitly declare a corresponding method starting with the characters "op_".</span></span> <span data-ttu-id="6e6ae-776">Den folgenden Abschnitten werden der entsprechenden Methodennamen mit jeder Operator.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-776">The following sections list the corresponding method names with each operator.</span></span>

<span data-ttu-id="6e6ae-777">Es gibt drei Klassen von Operatoren, die definiert werden können: unäre Operatoren, binäre Operatoren und Konvertierungsoperatoren.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-777">There are three classes of operators that can be defined: unary operators, binary operators and conversion operators.</span></span> <span data-ttu-id="6e6ae-778">Alle Operatordeklarationen teilen sich bestimmte Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-778">All operator declarations share certain restrictions:</span></span>

* <span data-ttu-id="6e6ae-779">Operatordeklarationen sein `Public` und `Shared`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-779">Operator declarations must always be `Public` and `Shared`.</span></span> <span data-ttu-id="6e6ae-780">Die `Public` Modifizierer kann in Kontexten, in dem der Modifizierer wird davon ausgegangen, dass, weggelassen werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-780">The `Public` modifier can be omitted in contexts where the modifier will be assumed.</span></span>

* <span data-ttu-id="6e6ae-781">Die Parameter der Bediener können nicht deklariert werden `ByRef`, `Optional` oder `ParamArray`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-781">The parameters of an operator cannot be declared `ByRef`, `Optional` or `ParamArray`.</span></span>

* <span data-ttu-id="6e6ae-782">Der Typ von mindestens einer der Operanden oder den Rückgabewert muss den Typ, der den Operator enthält.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-782">The type of at least one of the operands or the return value must be the type that contains the operator.</span></span>

* <span data-ttu-id="6e6ae-783">Es ist keine Funktion Rückgabevariablen, die für die Operatoren definiert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-783">There is no function return variable defined for operators.</span></span> <span data-ttu-id="6e6ae-784">Aus diesem Grund die `Return` -Anweisung zum Zurückgeben von Werten aus einem Operator Text verwendet werden muss.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-784">Therefore, the `Return` statement must be used to return values from an operator body.</span></span>

<span data-ttu-id="6e6ae-785">Die einzige Ausnahme für diese Einschränkungen gilt für auf NULL festlegbare Werttypen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-785">The only exception to these restrictions applies to nullable value types.</span></span> <span data-ttu-id="6e6ae-786">Da auf NULL festlegbare Werttypen keine Definition der tatsächliche Typ verfügen, kann ein Werttyp benutzerdefinierten Operatoren, die auf NULL festlegbare Version des Typs deklarieren.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-786">Since nullable value types do not have an actual type definition, a value type can declare user-defined operators for the nullable version of the type.</span></span> <span data-ttu-id="6e6ae-787">Beim bestimmen, ob es sich bei einen bestimmten benutzerdefinierten Operator, einen Typ deklarieren, kann die `?` Modifizierer sind im Rahmen der Prüfung der Gültigkeit auf alle Typen in der Deklaration beteiligten zuerst gelöscht.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-787">When determining whether a type can declare a particular user-defined operator, the `?` modifiers are first dropped off of all of the types involved in the declaration for the purposes of validity checking.</span></span> <span data-ttu-id="6e6ae-788">Die Lockerung gelten nicht für den Rückgabetyp der der `IsTrue` und `IsFalse` Operatoren; sie müssen weiterhin zurückgeben `Boolean`, nicht `Boolean?`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-788">This relaxation does not apply to the return type of the `IsTrue` and `IsFalse` operators; they must still return `Boolean`, not `Boolean?`.</span></span>

<span data-ttu-id="6e6ae-789">Die Rangfolge und Assoziativität von einem Operator können nicht von einem Operatordeklaration geändert werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-789">The precedence and associativity of an operator cannot be modified by an operator declaration.</span></span>

<span data-ttu-id="6e6ae-790">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="6e6ae-790">__Note.__</span></span> <span data-ttu-id="6e6ae-791">Operatoren weisen die gleiche Einschränkung Zeile Platzierung, die Unterroutinen verfügen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-791">Operators have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="6e6ae-792">Der Anfang-Anweisung, die End-Anweisung und die Block müssen alle am Anfang einer logischen Zeile angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-792">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>


### <a name="unary-operators"></a><span data-ttu-id="6e6ae-793">Unäre Operatoren</span><span class="sxs-lookup"><span data-stu-id="6e6ae-793">Unary Operators</span></span>

<span data-ttu-id="6e6ae-794">Die folgenden Unäroperatoren können überladen werden:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-794">The following unary operators can be overloaded:</span></span>

* <span data-ttu-id="6e6ae-795">Der unäre plus -Operator `+` (entsprechende-Methode: `op_UnaryPlus`)</span><span class="sxs-lookup"><span data-stu-id="6e6ae-795">The unary plus operator `+` (corresponding method: `op_UnaryPlus`)</span></span>

* <span data-ttu-id="6e6ae-796">Der unäre Operator minus `-` (entsprechende-Methode: `op_UnaryNegation`)</span><span class="sxs-lookup"><span data-stu-id="6e6ae-796">The unary minus operator `-` (corresponding method: `op_UnaryNegation`)</span></span>

* <span data-ttu-id="6e6ae-797">Die logische `Not` Operator (entsprechende-Methode: `op_OnesComplement`)</span><span class="sxs-lookup"><span data-stu-id="6e6ae-797">The logical `Not` operator (corresponding method: `op_OnesComplement`)</span></span>

* <span data-ttu-id="6e6ae-798">Die `IsTrue` und `IsFalse` Operatoren (entsprechenden Methoden: `op_True`, `op_False`)</span><span class="sxs-lookup"><span data-stu-id="6e6ae-798">The `IsTrue` and `IsFalse` operators (corresponding methods: `op_True`, `op_False`)</span></span>

<span data-ttu-id="6e6ae-799">Alle der überladene unäre Operatoren muss einen einzelnen Parameter des enthaltenden Typs und kann einen beliebigen Typ zurückgeben, mit Ausnahme von `IsTrue` und `IsFalse`, muss die zurückgeben, `Boolean`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-799">All overloaded unary operators must take a single parameter of the containing type and may return any type, except for `IsTrue` and `IsFalse`, which must return `Boolean`.</span></span> <span data-ttu-id="6e6ae-800">Wenn der enthaltende Typ ein generischer Typ ist, müssen die Typparameter des enthaltenden Typs-Typ-Parametern übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-800">If the containing type is a generic type, the type parameters must match the containing type's type parameters.</span></span> <span data-ttu-id="6e6ae-801">Ein auf ein Objekt angewendeter</span><span class="sxs-lookup"><span data-stu-id="6e6ae-801">For example,</span></span>

```vb
Structure Complex
    ...

    Public Shared Operator +(v As Complex) As Complex
        Return v
    End Operator
End Structure
```

<span data-ttu-id="6e6ae-802">Wenn ein Typ eines `IsTrue` oder `IsFalse`, und klicken Sie dann sie auf der anderen überladen muss.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-802">If a type overloads one of `IsTrue` or `IsFalse`, then it must overload the other as well.</span></span> <span data-ttu-id="6e6ae-803">Wenn nur eine überladen ist, führt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-803">If only one is overloaded, a compile-time error results.</span></span>

<span data-ttu-id="6e6ae-804">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="6e6ae-804">__Note.__</span></span> <span data-ttu-id="6e6ae-805">`IsTrue` und `IsFalse` sind keine reservierten Wörter.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-805">`IsTrue` and `IsFalse` are not reserved words.</span></span>

### <a name="binary-operators"></a><span data-ttu-id="6e6ae-806">Binäre Operatoren</span><span class="sxs-lookup"><span data-stu-id="6e6ae-806">Binary Operators</span></span>

<span data-ttu-id="6e6ae-807">Die folgenden binären Operatoren können überladen werden:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-807">The following binary operators can be overloaded:</span></span>

* <span data-ttu-id="6e6ae-808">Die Hinzufügung `+`, Subtraktion `-`, Multiplikation `*`, Division `/`, ganzzahligen Division `\`, modulo `Mod` als auch die Potenzierung `^` Operatoren (entsprechende-Methode: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_IntegerDivision`, `op_Modulus`, `op_Exponent`)</span><span class="sxs-lookup"><span data-stu-id="6e6ae-808">The addition `+`, subtraction `-`, multiplication `*`, division `/`, integral division `\`, modulo `Mod` and exponentiation `^` operators (corresponding method: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_IntegerDivision`, `op_Modulus`, `op_Exponent`)</span></span>

* <span data-ttu-id="6e6ae-809">Die relationalen Operatoren `=`, `<>`, `<`, `>`, `<=`, `>=` (entsprechenden Methoden: `op_Equality`, `op_Inequality`, `op_LessThan`, `op_GreaterThan`, `op_LessThanOrEqual` , `op_GreaterThanOrEqual`).</span><span class="sxs-lookup"><span data-stu-id="6e6ae-809">The relational operators `=`, `<>`, `<`, `>`, `<=`, `>=` (corresponding methods: `op_Equality`, `op_Inequality`, `op_LessThan`, `op_GreaterThan`, `op_LessThanOrEqual`, `op_GreaterThanOrEqual`).</span></span> <span data-ttu-id="6e6ae-810">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="6e6ae-810">__Note.__</span></span> <span data-ttu-id="6e6ae-811">Während der Equality-Operator überladen werden kann, nicht der Zuweisungsoperator (nur in zuweisungsanweisungen verwendet) nicht überladen werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-811">While the equality operator can be overloaded, the assignment operator (used only in assignment statements) cannot be overloaded.</span></span>

* <span data-ttu-id="6e6ae-812">Die `Like` Operator (entsprechende-Methode: `op_Like`)</span><span class="sxs-lookup"><span data-stu-id="6e6ae-812">The `Like` operator (corresponding method: `op_Like`)</span></span>

* <span data-ttu-id="6e6ae-813">Der Operator für Verkettungen `&` (entsprechende-Methode: `op_Concatenate`)</span><span class="sxs-lookup"><span data-stu-id="6e6ae-813">The concatenation operator `&` (corresponding method: `op_Concatenate`)</span></span>

* <span data-ttu-id="6e6ae-814">Die logische `And`, `Or` und `Xor` Operatoren (entsprechenden Methoden: `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`)</span><span class="sxs-lookup"><span data-stu-id="6e6ae-814">The logical `And`, `Or` and `Xor` operators (corresponding methods: `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`)</span></span>

* <span data-ttu-id="6e6ae-815">Die Schiebeoperatoren `<<` und `>>` (entsprechenden Methoden: `op_LeftShift`, `op_RightShift`)</span><span class="sxs-lookup"><span data-stu-id="6e6ae-815">The shift operators `<<` and `>>` (corresponding methods: `op_LeftShift`, `op_RightShift`)</span></span>

<span data-ttu-id="6e6ae-816">Alle überladene binäre Operatoren müssen den enthaltenden Typ als einen der Parameter verwenden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-816">All overloaded binary operators must take the containing type as one of the parameters.</span></span> <span data-ttu-id="6e6ae-817">Wenn der enthaltende Typ ein generischer Typ ist, müssen die Typparameter des enthaltenden Typs-Typ-Parametern übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-817">If the containing type is a generic type, the type parameters must match the containing type's type parameters.</span></span> <span data-ttu-id="6e6ae-818">Die Schiebeoperatoren weiter einschränken, diese Regel, dass den ersten Parameter für den enthaltenden Typ aufweisen müssen; der zweite Parameter muss stets vom Typ `Integer`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-818">The shift operators further restrict this rule to require the first parameter to be of the containing type; the second parameter must always be of type `Integer`.</span></span>

<span data-ttu-id="6e6ae-819">Die folgenden binären Operatoren müssen als Paar deklariert werden:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-819">The following binary operators must be declared in pairs:</span></span>

* <span data-ttu-id="6e6ae-820">Operator `=` and -Operator `<>`</span><span class="sxs-lookup"><span data-stu-id="6e6ae-820">Operator `=` and operator `<>`</span></span>

* <span data-ttu-id="6e6ae-821">Operator `>` and -Operator `<`</span><span class="sxs-lookup"><span data-stu-id="6e6ae-821">Operator `>` and operator `<`</span></span>

* <span data-ttu-id="6e6ae-822">Operator `>=` and -Operator `<=`</span><span class="sxs-lookup"><span data-stu-id="6e6ae-822">Operator `>=` and operator `<=`</span></span>

<span data-ttu-id="6e6ae-823">Wenn der beiden deklariert wird, klicken Sie dann die andere muss ebenfalls deklariert werden mit übereinstimmenden Parameter- und Rückgabetypen oder ein Fehler während der Kompilierung führt.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-823">If one of the pair is declared, then the other must also be declared with matching parameter and return types, or a compile-time error will result.</span></span> <span data-ttu-id="6e6ae-824">(__Beachten.__</span><span class="sxs-lookup"><span data-stu-id="6e6ae-824">(__Note.__</span></span> <span data-ttu-id="6e6ae-825">Der Zweck der gekoppelte Deklarationen von relationalen Operatoren erfordern ist testen und stellen Sie mindestens ein Mindestmaß an überladenen Operatoren logische Konsistenz sicher.)</span><span class="sxs-lookup"><span data-stu-id="6e6ae-825">The purpose of requiring paired declarations of relational operators is to try and ensure at least a minimum level of logical consistency in overloaded operators.)</span></span>

<span data-ttu-id="6e6ae-826">Im Gegensatz zu relationalen Operatoren wird überladen der Division und die Divisionsoperatoren der ganzzahligen dringend abgeraten, jedoch nicht um einen Fehler.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-826">In contrast to the relational operators, overloading both the division and integral division operators is strongly discouraged, although not an error.</span></span> <span data-ttu-id="6e6ae-827">(__Beachten.__</span><span class="sxs-lookup"><span data-stu-id="6e6ae-827">(__Note.__</span></span> <span data-ttu-id="6e6ae-828">Im Allgemeinen muss die beiden Arten von Abteilung völlig unterschiedliche: ein Typ, unterstützt der Division, ist entweder ganzzahlige (in diesem Fall es unterstützen soll `\`) oder nicht (in diesem Fall es unterstützen soll `/`).</span><span class="sxs-lookup"><span data-stu-id="6e6ae-828">In general, the two types of division should be entirely distinct: a type that supports division is either integral (in which case it should support `\`) or not (in which case it should support `/`).</span></span> <span data-ttu-id="6e6ae-829">Wir als somit einen Fehler, wenn beide Operatoren definieren, aber da die Sprache nicht in der Regel zwischen zwei Typen der Abteilung wie wie Visual Basic unterschieden werden, wir sind der Ansicht, dass es am sichersten, können die Methode jedoch dringend davon abgeraten, es war.)</span><span class="sxs-lookup"><span data-stu-id="6e6ae-829">We considered making it an error to define both operators, but because their languages do not generally distinguish between two types of division the way Visual Basic does, we felt it was safest to allow the practice but strongly discourage it.)</span></span>

<span data-ttu-id="6e6ae-830">Verbundzuweisung, die Operatoren können nicht direkt überladen werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-830">Compound assignment operators cannot be overloaded directly.</span></span> <span data-ttu-id="6e6ae-831">Wenn der entsprechende binäre Operator überladen ist, wird der Verbundzuweisungsoperator den überladenen Operator verwenden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-831">Instead, when the corresponding binary operator is overloaded, the compound assignment operator will use the overloaded operator.</span></span> <span data-ttu-id="6e6ae-832">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-832">For example:</span></span>

```vb
Structure Complex
    ...

    Public Shared Operator +(x As Complex, y As Complex) _
        As Complex
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim c1, c2 As Complex
        ' Calls the overloaded + operator
        c1 += c2
    End Sub
End Module
```

### <a name="conversion-operators"></a><span data-ttu-id="6e6ae-833">Konvertierungsoperatoren</span><span class="sxs-lookup"><span data-stu-id="6e6ae-833">Conversion Operators</span></span>

<span data-ttu-id="6e6ae-834">Konvertierungsoperatoren definieren neue Konvertierungen zwischen Typen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-834">Conversion operators define new conversions between types.</span></span> <span data-ttu-id="6e6ae-835">Diese neuen Konvertierungen heißen *benutzerdefinierte Konvertierungen*.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-835">These new conversions are called *user-defined conversions*.</span></span> <span data-ttu-id="6e6ae-836">Ein Konvertierungsoperator konvertiert aus einer Datenquelle, die von der Parametertyp des Konvertierungsoperators in einen Zieltyp, angegeben durch den Rückgabetyp der Operator für die Konvertierung angegeben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-836">A conversion operator converts from a source type, indicated by the parameter type of the conversion operator, to a target type, indicated by the return type of the conversion operator.</span></span> <span data-ttu-id="6e6ae-837">Konvertierungen müssen als erweiternd oder einschränkend klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-837">Conversions must be classified as either widening or narrowing.</span></span> <span data-ttu-id="6e6ae-838">Eine Konvertierung Operator-Deklaration, enthält die `Widening` -Schlüsselwort Führt eine benutzerdefinierte widening-Konvertierung (entsprechende-Methode: `op_Implicit`).</span><span class="sxs-lookup"><span data-stu-id="6e6ae-838">A conversion operator declaration that includes the `Widening` keyword introduces a user-defined widening conversion (corresponding method: `op_Implicit`).</span></span> <span data-ttu-id="6e6ae-839">Eine Konvertierung Operator-Deklaration, enthält die `Narrowing` -Schlüsselwort Führt eine benutzerdefinierte einschränkende Konvertierung (entsprechende-Methode: `op_Explicit`).</span><span class="sxs-lookup"><span data-stu-id="6e6ae-839">A conversion operator declaration that includes the `Narrowing` keyword introduces a user-defined narrowing conversion (corresponding method: `op_Explicit`).</span></span>

<span data-ttu-id="6e6ae-840">Im Allgemeinen sollten benutzerdefinierte erweiternde Konvertierungen entworfen werden, um keine Ausnahmen auslösen und keine Informationen verlieren.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-840">In general, user-defined widening conversions should be designed to never throw exceptions and never lose information.</span></span> <span data-ttu-id="6e6ae-841">Wenn eine benutzerdefinierte Konvertierung dazu führen, Ausnahmen dass kann (z. B. weil das Quellargument außerhalb des gültigen Bereichs ist) oder Verlust von Informationen (z. B. das höherwertige Bits werden verworfen), und klicken Sie dann auf diese Konvertierung als eine einschränkende Konvertierung definiert werden sollte.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-841">If a user-defined conversion can cause exceptions (for example, because the source argument is out of range) or loss of information (such as discarding high-order bits), then that conversion should be defined as a narrowing conversion.</span></span> <span data-ttu-id="6e6ae-842">Im folgenden Beispiel</span><span class="sxs-lookup"><span data-stu-id="6e6ae-842">In the example:</span></span>

```vb
Structure Digit
    Dim value As Byte

    Public Sub New(value As Byte)
        if value < 0 OrElse value > 9 Then Throw New ArgumentException()
        Me.value = value
    End Sub

    Public Shared Widening Operator CType(d As Digit) As Byte
        Return d.value
    End Operator

    Public Shared Narrowing Operator CType(b As Byte) As Digit
        Return New Digit(b)
    End Operator
End Structure
```

<span data-ttu-id="6e6ae-843">die Konvertierung von `Digit` zu `Byte` ist eine erweiternde Konvertierung, da sie nie Ausnahmen auslöst oder Informationen, aber die Konvertierung von verliert `Byte` zu `Digit` ist eine einschränkende Konvertierung seit `Digit` können nur darstellen einer die Teilmenge der möglichen Werte einer `Byte`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-843">the conversion from `Digit` to `Byte` is a widening conversion because it never throws exceptions or loses information, but the conversion from `Byte` to `Digit` is a narrowing conversion since `Digit` can only represent a subset of the possible values of a `Byte`.</span></span>

<span data-ttu-id="6e6ae-844">Im Gegensatz zu allen anderen Typmember, die überladen werden können, enthält die Signatur eines Konvertierungsoperators den Zieltyp der Konvertierung.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-844">Unlike all other type members that can be overloaded, the signature of a conversion operator includes the target type of the conversion.</span></span> <span data-ttu-id="6e6ae-845">Dies ist der einzige Typmember, die für den Rückgabetyp in der Signatur beteiligt ist.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-845">This is the only type member for which the return type participates in the signature.</span></span> <span data-ttu-id="6e6ae-846">Die erweiternde oder einschränkende Klassifizierung eines Konvertierungsoperators ist jedoch nicht Teil der Signatur des Operators.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-846">The widening or narrowing classification of a conversion operator, however, is not part of the operator's signature.</span></span> <span data-ttu-id="6e6ae-847">Eine Klasse oder Struktur kann daher nicht sowohl eine erweiternde Konvertierungsoperator als auch eine einschränkende Konvertierungsoperator mit derselben Quelle und Zieltypen deklarieren.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-847">Thus, a class or structure cannot declare both a widening conversion operator and a narrowing conversion operator with the same source and target types.</span></span>

<span data-ttu-id="6e6ae-848">Operator für die benutzerdefinierte Konvertierung muss zum oder vom enthaltenden Typ konvertieren: Es ist beispielsweise möglich, dass eine Klasse `C` definieren Sie eine Konvertierung von `C` zu `Integer` und `Integer` zu `C`, jedoch nicht aus `Integer` zu `Boolean`.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-848">A user-defined conversion operator must convert either to or from the containing type -- for example, it is possible for a class `C` to define a conversion from `C` to `Integer` and from `Integer` to `C`, but not from `Integer` to `Boolean`.</span></span> <span data-ttu-id="6e6ae-849">Wenn der enthaltende Typ ein generischer Typ ist, müssen die Typparameter des enthaltenden Typs-Typ-Parametern übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-849">If the containing type is a generic type, the type parameters must match the containing type's type parameters.</span></span> <span data-ttu-id="6e6ae-850">Darüber hinaus ist es nicht möglich, um eine Konvertierung (d. h. nicht-benutzerdefiniert) neu zu definieren.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-850">Also, it is not possible to redefine an intrinsic (i.e. non-user-defined) conversion.</span></span> <span data-ttu-id="6e6ae-851">Daher Deklarieren eines Typs kann keine Konvertierung, in denen:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-851">As a result, a type cannot declare a conversion where:</span></span>

* <span data-ttu-id="6e6ae-852">Quell- und Zieltyp sind identisch.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-852">The source type and the destination type are the same.</span></span>

* <span data-ttu-id="6e6ae-853">Sowohl die Quell- und Zieltyp sind nicht der Typ, der den Konvertierungsoperator definiert.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-853">Both the source type and the destination type are not the type that defines the conversion operator.</span></span>

* <span data-ttu-id="6e6ae-854">Der Quelltyp oder Zieltyp ist ein Schnittstellentyp.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-854">The source type or the destination type is an interface type.</span></span>

* <span data-ttu-id="6e6ae-855">Die Quell- und Zieltypen sind durch Vererbung verwandt sind (einschließlich `Object`).</span><span class="sxs-lookup"><span data-stu-id="6e6ae-855">The source type and destination types are related by inheritance (including `Object`).</span></span>

<span data-ttu-id="6e6ae-856">Diese Regeln die einzige Ausnahme gilt für auf NULL festlegbare Werttypen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-856">The only exception to these rules applies to nullable value types.</span></span> <span data-ttu-id="6e6ae-857">Da auf NULL festlegbare Werttypen keine Definition der tatsächliche Typ verfügen, kann ein Werttyp benutzerdefinierte Konvertierungen, die auf NULL festlegbare Version des Typs deklarieren.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-857">Since nullable value types do not have an actual type definition, a value type can declare user-defined conversions for the nullable version of the type.</span></span> <span data-ttu-id="6e6ae-858">Beim bestimmen, ob eine bestimmte benutzerdefinierte Konvertierung, einen Typ deklarieren, kann die `?` Modifizierer sind im Rahmen der Prüfung der Gültigkeit auf alle Typen in der Deklaration beteiligten zuerst gelöscht.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-858">When determining whether a type can declare a particular user-defined conversion, the `?` modifiers are first dropped off of all of the types involved in the declaration for the purposes of validity checking.</span></span> <span data-ttu-id="6e6ae-859">Daher ist die folgende Deklaration gültig da `S` können definieren, eine Konvertierung von `S` zu `T`:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-859">Thus, the following declaration is valid because `S` can define a conversion from `S` to `T`:</span></span>

```vb
Structure T
    ...
End Structure

Structure S
    Public Shared Widening Operator CType(ByVal v As S?) As T
    ...
    End Operator
End Structure
```

<span data-ttu-id="6e6ae-860">Die folgende Deklaration ist gültig, jedoch nicht, da Struktur `S` kann nicht definiert eine Konvertierung von `S` zu `S`:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-860">The following declaration is not valid, however, because structure `S` cannot define a conversion from `S` to `S`:</span></span>

```vb
Structure S
    Public Shared Widening Operator CType(ByVal v As S) As S?
        ...
    End Operator
End Structure
```

### <a name="operator-mapping"></a><span data-ttu-id="6e6ae-861">Operator-Zuordnung</span><span class="sxs-lookup"><span data-stu-id="6e6ae-861">Operator Mapping</span></span>

<span data-ttu-id="6e6ae-862">Da es sich bei der Satz von Standardabfrageoperatoren, die Visual Basic unterstützt nicht genau dem Satz von Standardabfrageoperatoren, andere Sprachen in .NET Framework möglicherweise übereinstimmen, werden einige Operatoren zugeordnet, speziell auf andere Operatoren definiert oder verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-862">Because the set of operators that Visual Basic supports may not exactly match the set of operators that other languages on the .NET Framework, some operators are mapped specially onto other operators when being defined or used.</span></span> <span data-ttu-id="6e6ae-863">Dies gilt insbesondere in folgenden Fällen:</span><span class="sxs-lookup"><span data-stu-id="6e6ae-863">Specifically:</span></span>

* <span data-ttu-id="6e6ae-864">Definieren eines Operators ganzzahligen Division wird automatisch eine reguläre Divisionsoperator (nur in anderen Sprachen verwendet werden) definiert, die den ganzzahligen Divisionsoperator aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-864">Defining an integral division operator will automatically define a regular division operator (usable only from other languages) that will call the integral division operator.</span></span>

* <span data-ttu-id="6e6ae-865">Das Überladen der `Not`, `And`, und `Or` Operatoren werden nur den bitweisen Operator aus der Perspektive der anderen Sprachen, die Unterscheidung zwischen logische und bitweise Operatoren überladen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-865">Overloading the `Not`, `And`, and `Or` operators will overload only the bitwise operator from the perspective of other languages that distinguish between logical and bitwise operators.</span></span>

* <span data-ttu-id="6e6ae-866">Eine Klasse, die nur die logischen Operatoren in einer anderen Sprache Überladungen, die zwischen logische und bitweise Operatoren unterscheidet (z. B. ein Sprachen, die verwendet `op_LogicalNot`, `op_LogicalAnd`, und `op_LogicalOr` für `Not`, `And`, und `Or`bzw.) müssen ihre logischen Operatoren, die auf die logischen Operatoren mit Visual Basic zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-866">A class that overloads only the logical operators in a language that distinguishes between logical and bitwise operators (i.e. a languages that uses `op_LogicalNot`, `op_LogicalAnd`, and `op_LogicalOr` for `Not`, `And`, and `Or`, respectively) will have their logical operators mapped onto the Visual Basic logical operators.</span></span> <span data-ttu-id="6e6ae-867">Wenn die logischen und bitweisen Operatoren überladen werden, werden nur die bitweisen Operatoren verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-867">If both the logical and bitwise operators are overloaded, only the bitwise operators will be used.</span></span>

* <span data-ttu-id="6e6ae-868">Das Überladen der `<<` und `>>` Operatoren werden nur die mit Operatoren aus der Perspektive der anderen Sprachen, die Unterscheidung zwischen mit und ohne Vorzeichen Shift-Operatoren überladen.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-868">Overloading the `<<` and `>>` operators will overload only the signed operators from the perspective of other languages that distinguish between signed and unsigned shift operators.</span></span>

* <span data-ttu-id="6e6ae-869">Eine Klasse, die nur einen nicht signierte Shift-Operator überlädt, wird den nicht signierte Shift-Operator, der auf der entsprechenden Visual Basic-Shift-Operator zugeordnet haben.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-869">A class that overloads only an unsigned shift operator will have the unsigned shift operator mapped onto the corresponding Visual Basic shift operator.</span></span> <span data-ttu-id="6e6ae-870">Wenn sowohl ein nicht signierter und signierte Shift-Operator überladen ist, wird nur der signierten Shift-Operator verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6e6ae-870">If both an unsigned and signed shift operator is overloaded, only the signed shift operator will be used.</span></span>
