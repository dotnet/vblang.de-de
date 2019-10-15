---
ms.openlocfilehash: 3d5c1e90283b6d6ec8cdeccd35e32c78f997cc27
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306036"
---
# <a name="type-members"></a><span data-ttu-id="b0f1b-101">Typmember</span><span class="sxs-lookup"><span data-stu-id="b0f1b-101">Type Members</span></span>

<span data-ttu-id="b0f1b-102">Typmember definieren Speicherorte und ausführbaren Code.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-102">Type members define storage locations and executable code.</span></span> <span data-ttu-id="b0f1b-103">Dabei kann es sich um Methoden, Konstruktoren, Ereignisse, Konstanten, Variablen und Eigenschaften handeln.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-103">They can be methods, constructors, events, constants, variables, and properties.</span></span>

## <a name="interface-method-implementation"></a><span data-ttu-id="b0f1b-104">Implementierung der Schnittstellen Methode</span><span class="sxs-lookup"><span data-stu-id="b0f1b-104">Interface Method Implementation</span></span>

<span data-ttu-id="b0f1b-105">Methoden, Ereignisse und Eigenschaften können Schnittstellenmember implementieren.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-105">Methods, events, and properties can implement interface members.</span></span> <span data-ttu-id="b0f1b-106">Um einen Schnittstellenmember zu implementieren, gibt eine Element Deklaration das `Implements`-Schlüsselwort an und listet mindestens ein Schnittstellenmember auf.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-106">To implement an interface member, a member declaration specifies the `Implements` keyword and lists one or more interface members.</span></span>

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

<span data-ttu-id="b0f1b-107">Methoden und Eigenschaften, die Schnittstellenmember implementieren, sind implizit `NotOverridable`, es sei denn, Sie deklarieren als `MustOverride`, `Overridable` oder überschreiben ein anderes Element.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-107">Methods and properties that implement interface members are implicitly `NotOverridable` unless declared to be `MustOverride`, `Overridable`, or overriding another member.</span></span> <span data-ttu-id="b0f1b-108">Es ist ein Fehler, wenn ein Member, der einen Schnittstellenmember implementiert, `Shared` ist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-108">It is an error for a member implementing an interface member to be `Shared`.</span></span> <span data-ttu-id="b0f1b-109">Die Barrierefreiheit eines Members hat keine Auswirkung auf die Möglichkeit, Schnittstellenmember zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-109">A member's accessibility has no effect on its ability to implement interface members.</span></span>

<span data-ttu-id="b0f1b-110">Damit eine Schnittstellen Implementierung gültig ist, muss die implementierende Liste des enthaltenden Typs eine Schnittstelle benennen, die einen kompatiblen Member enthält.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-110">For an interface implementation to be valid, the implements list of the containing type must name an interface that contains a compatible member.</span></span> <span data-ttu-id="b0f1b-111">Ein kompatibles Member ist ein Element, dessen Signatur mit der Signatur des implementierenden Members übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-111">A compatible member is one whose signature matches the signature of the implementing member.</span></span> <span data-ttu-id="b0f1b-112">Wenn eine generische Schnittstelle implementiert wird, wird das Typargument, das in der implementierten Klausel bereitgestellt wird, beim Überprüfen der Kompatibilität in die Signatur ersetzt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-112">If a generic interface is being implemented, then the type argument supplied in the Implements clause is substituted into the signature when checking compatibility.</span></span> <span data-ttu-id="b0f1b-113">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-113">For example:</span></span>

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

<span data-ttu-id="b0f1b-114">Wenn ein Ereignis, das mit einem Delegattyp deklariert wurde, ein Schnittstellen Ereignis implementiert, dann ist ein kompatibles Ereignis, dessen zugrunde liegender Delegattyp derselbe Typ ist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-114">If an event declared using a delegate type is implementing an interface event, then a compatible event is one whose underlying delegate type is the same type.</span></span> <span data-ttu-id="b0f1b-115">Andernfalls verwendet das Ereignis den Delegattyp aus dem Schnittstellen Ereignis, das er implementiert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-115">Otherwise, the event uses the delegate type from the interface event it is implementing.</span></span> <span data-ttu-id="b0f1b-116">Wenn ein derartiges Ereignis mehrere Schnittstellen Ereignisse implementiert, müssen alle Schnittstellen Ereignisse denselben zugrunde liegenden Delegattyp aufweisen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-116">If such an event implements multiple interface events, all the interface events must have the same underlying delegate type.</span></span> <span data-ttu-id="b0f1b-117">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-117">For example:</span></span>

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

<span data-ttu-id="b0f1b-118">Ein Schnittstellenmember in der implementierten Liste wird mithilfe eines Typnamens, eines Zeitraums und eines Bezeichners angegeben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-118">An interface member in the implements list is specified using a type name, a period, and an identifier.</span></span> <span data-ttu-id="b0f1b-119">Der Typname muss eine Schnittstelle in der implementierten Liste oder eine Basisschnittstelle einer Schnittstelle in der implementierten Liste sein, und der Bezeichner muss ein Member der angegebenen Schnittstelle sein.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-119">The type name must be an interface in the implements list or a base interface of an interface in the implements list, and the identifier must be a member of the specified interface.</span></span> <span data-ttu-id="b0f1b-120">Ein einzelner Member kann mehr als einen übereinstimmenden Schnittstellenmember implementieren.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-120">A single member can implement more than one matching interface member.</span></span>

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

<span data-ttu-id="b0f1b-121">Wenn der implementierte Schnittstellenmember in allen explizit implementierten Schnittstellen aufgrund mehrerer Schnittstellen Vererbung nicht verfügbar ist, muss der implementierende Member explizit auf eine Basisschnittstelle verweisen, auf der der Member verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-121">If the interface member being implemented is unavailable in all explicitly implemented interfaces because of multiple interface inheritance, the implementing member must explicitly reference a base interface on which the member is available.</span></span> <span data-ttu-id="b0f1b-122">Wenn z. b. `I1` und `I2` einen Member `M` enthalten und `I3` von `I1` und `I2` erbt, implementiert ein Typ, der `I3` implementiert, `I1.M` und `I2.M`.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-122">For example, if `I1` and `I2` contain a member `M`, and `I3` inherits from `I1` and `I2`, a type implementing `I3` will implement `I1.M` and `I2.M`.</span></span> <span data-ttu-id="b0f1b-123">Wenn eine Schnittstelle die Vererbung von geerbten Membern multipliziert, muss ein implementierender Typ die geerbten Member implementieren, und die Member (e) müssen Sie Shadowing durchführen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-123">If an interface shadows multiply inherited members, an implementing type will have to implement the inherited members and the member(s) shadowing them.</span></span>

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

<span data-ttu-id="b0f1b-124">Wenn die enthaltende Schnittstelle des Schnittstellenmembers generisch ist, müssen die gleichen Typargumente wie die implementierte Schnittstelle bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-124">If the containing interface of the interface member be implemented is generic, the same type arguments as the interface being implements must be supplied.</span></span> <span data-ttu-id="b0f1b-125">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-125">For example:</span></span>

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


## <a name="methods"></a><span data-ttu-id="b0f1b-126">Methoden</span><span class="sxs-lookup"><span data-stu-id="b0f1b-126">Methods</span></span>

<span data-ttu-id="b0f1b-127">Methoden enthalten die ausführbaren Anweisungen eines Programms.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-127">Methods contain the executable statements of a program.</span></span>

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

<span data-ttu-id="b0f1b-128">Methoden, die eine optionale Liste von Parametern und einen optionalen Rückgabewert aufweisen, sind entweder freigegeben oder nicht freigegeben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-128">Methods, which have an optional list of parameters and an optional return value, are either shared or not shared.</span></span> <span data-ttu-id="b0f1b-129">Auf freigegebene Methoden wird über die-Klasse oder Instanzen der-Klasse zugegriffen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-129">Shared methods are accessed through the class or instances of the class.</span></span> <span data-ttu-id="b0f1b-130">Auf nicht freigegebene Methoden, die auch als Instanzmethoden bezeichnet werden, erfolgt der Zugriff über Instanzen der-Klasse.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-130">Non-shared methods, also called instance methods, are accessed through instances of the class.</span></span> <span data-ttu-id="b0f1b-131">Das folgende Beispiel zeigt eine Klasse `Stack` mit mehreren freigegebenen Methoden (`Clone` und `Flip`) und mehrere Instanzmethoden (`Push`, `Pop` und `ToString`):</span><span class="sxs-lookup"><span data-stu-id="b0f1b-131">The following example shows a class `Stack` that has several shared methods (`Clone` and `Flip`), and several instance methods (`Push`, `Pop`, and `ToString`):</span></span>

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

<span data-ttu-id="b0f1b-132">Methoden können überladen werden, was bedeutet, dass mehrere Methoden denselben Namen haben können, solange Sie eindeutige Signaturen aufweisen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-132">Methods can be overloaded, which means that multiple methods may have the same name so long as they have unique signatures.</span></span> <span data-ttu-id="b0f1b-133">Die Signatur einer Methode besteht aus der Anzahl und den Typen ihrer Parameter.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-133">The signature of a method consists of the number and types of its parameters.</span></span> <span data-ttu-id="b0f1b-134">Die Signatur einer Methode schließt den Rückgabetyp oder die Parametermodifizierer wie "optional", "ByRef" oder "ParamArray" nicht ein.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-134">The signature of a method specifically does not include the return type or parameter modifiers such as Optional, ByRef or ParamArray.</span></span> <span data-ttu-id="b0f1b-135">Das folgende Beispiel zeigt eine-Klasse mit einer Reihe von über Ladungen:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-135">The following example shows a class with a number of overloads:</span></span>

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

<span data-ttu-id="b0f1b-136">Die Ausgabe des Programms lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-136">The output of the program is:</span></span>

```console
F()
F(Integer)
F(Object)
F(Integer, Integer)
F(Integer())
G(String)
G(String, Optional String)
```

<span data-ttu-id="b0f1b-137">Über Ladungen, die sich nur in optionalen Parametern unterscheiden, können für die Versionsverwaltung von Bibliotheken verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-137">Overloads that differ only in optional parameters can be used for "versioning" of libraries.</span></span> <span data-ttu-id="b0f1b-138">V1 einer Bibliothek kann beispielsweise eine Funktion mit optionalen Parametern enthalten:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-138">For instance, v1 of a library might include a function with optional parameters:</span></span>

```vb
Sub fopen(fileName As String, Optional accessMode as Integer = 0)
```

<span data-ttu-id="b0f1b-139">Anschließend möchte v2 der Bibliothek einen weiteren optionalen Parameter "Password" hinzufügen. Dies ist ohne Unterbrechung der Quell Kompatibilität zu tun (sodass Anwendungen, für die v1 verwendet wird, neu kompiliert werden können) und ohne Unterbrechung der Binärkompatibilität (also Anwendungen, die Verweis v1 kann jetzt ohne Neukompilierung auf v2 verweisen).</span><span class="sxs-lookup"><span data-stu-id="b0f1b-139">Then v2 of the library wants to add another optional parameter "password", and it wants to do so without breaking source compatibility (so applications that used to target v1 can be recompiled), and without breaking binary compatibility (so applications that used to reference v1 can now reference v2 without recompilation).</span></span> <span data-ttu-id="b0f1b-140">So sieht v2 aus:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-140">This is how v2 will look:</span></span>

```vb
Sub fopen(file As String, mode as Integer)
Sub fopen(file As String, Optional mode as Integer = 0, Optional pword As String = "")
```

<span data-ttu-id="b0f1b-141">Beachten Sie, dass optionale Parameter in einer öffentlichen API nicht CLS-kompatibel sind.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-141">Note that optional parameters in a public API are not CLS-compliant.</span></span> <span data-ttu-id="b0f1b-142">Sie können jedoch zumindest von Visual Basic und c# 4 und F#verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-142">However, they can be consumed at least by Visual Basic and C#4 and F#.</span></span>



### <a name="regular-async-and-iterator-method-declarations"></a><span data-ttu-id="b0f1b-143">Reguläre, Async-und Iterator-Methoden Deklarationen</span><span class="sxs-lookup"><span data-stu-id="b0f1b-143">Regular, Async and Iterator Method Declarations</span></span>

<span data-ttu-id="b0f1b-144">Es gibt zwei Arten von Methoden: *Unterroutinen*, die keine Werte zurückgeben, und *Funktionen*, die dies tun.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-144">There are two types of methods: *subroutines*, which do not return values, and *functions*, which do.</span></span> <span data-ttu-id="b0f1b-145">Der Body-und `End`-Konstruktor einer Methode darf nur ausgelassen werden, wenn die Methode in einer Schnittstelle definiert ist oder den `MustOverride`-Modifizierer aufweist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-145">The body and `End` construct of a method may only be omitted if the method is defined in an interface or has the `MustOverride` modifier.</span></span> <span data-ttu-id="b0f1b-146">Wenn für eine Funktion kein Rückgabetyp angegeben wird und eine strikte Semantik verwendet wird, tritt ein Kompilierzeitfehler auf. Andernfalls ist der Typ implizit `Object` oder der Typ des Typzeichens der Methode.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-146">If no return type is specified on a function and strict semantics are being used, a compile-time error occurs; otherwise the type is implicitly `Object` or the type of the method's type character.</span></span> <span data-ttu-id="b0f1b-147">Die Zugriffs Domäne des Rückgabe Typs und die Parametertypen einer Methode müssen mit oder einer übergeordneten Zugriffs Domäne der Methode selbst identisch sein.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-147">The accessibility domain of the return type and parameter types of a method must be the same as or a superset of the accessibility domain of the method itself.</span></span>

<span data-ttu-id="b0f1b-148">Eine __reguläre Methode__ ist eine Methode, die weder `Async` noch `Iterator`-Modifizierer ist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-148">A __regular method__ is one with neither `Async` nor `Iterator` modifiers.</span></span> <span data-ttu-id="b0f1b-149">Dabei kann es sich um eine Unterroutine oder eine Funktion handeln.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-149">It may be a subroutine or a function.</span></span> <span data-ttu-id="b0f1b-150">Im Abschnitt [reguläre Methoden](statements.md#regular-methods) wird erläutert, was geschieht, wenn eine reguläre Methode aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-150">Section [Regular Methods](statements.md#regular-methods) details what happens when a regular method is invoked.</span></span>

<span data-ttu-id="b0f1b-151">Eine __Iteratormethode__ ist eine mit dem `Iterator`-Modifizierer und kein `Async`-Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-151">An __iterator method__ is one with the `Iterator` modifier and no `Async` modifier.</span></span> <span data-ttu-id="b0f1b-152">Er muss eine Funktion sein, und sein Rückgabetyp muss `IEnumerator`, `IEnumerable` oder `IEnumerator(Of T)` oder `IEnumerable(Of T)` für einige `T` sein, und er darf keine `ByRef`-Parameter aufweisen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-152">It must be a function, and its return type must be `IEnumerator`, `IEnumerable`, or `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`, and it must have no `ByRef` parameters.</span></span> <span data-ttu-id="b0f1b-153">Abschnitts [Iteratormethoden](statements.md#iterator-methods) gibt an, was geschieht, wenn eine Iteratormethode aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-153">Section [Iterator Methods](statements.md#iterator-methods) details what happens when an iterator method is invoked.</span></span>

<span data-ttu-id="b0f1b-154">Eine __Async-Methode__ ist eine Methode mit dem `Async`-Modifizierer und kein `Iterator`-Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-154">An __async method__ is one with the `Async` modifier and no `Iterator` modifier.</span></span> <span data-ttu-id="b0f1b-155">Er muss entweder eine Unterroutine oder eine Funktion mit dem Rückgabetyp `Task` oder `Task(Of T)` für einige `T` sein und darf keine `ByRef`-Parameter aufweisen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-155">It must be either a subroutine, or a function with return type `Task` or `Task(Of T)` for some `T`, and must have no `ByRef` parameters.</span></span> <span data-ttu-id="b0f1b-156">Im Abschnitt [Async-Methoden](statements.md#async-methods) wird erläutert, was geschieht, wenn eine Async-Methode aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-156">Section [Async Methods](statements.md#async-methods) details what happens when an async method is invoked.</span></span>

<span data-ttu-id="b0f1b-157">Es handelt sich um einen Kompilierzeitfehler, wenn eine Methode nicht eine dieser drei Arten von Methoden ist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-157">It is a compile-time error if a method is not one of these three kinds of method.</span></span>

<span data-ttu-id="b0f1b-158">Unterroutine-und Funktions Deklarationen sind ein besonderes Zeichen dafür, dass Ihre Start-und End-Anweisungen jeweils am Anfang einer logischen Zeile beginnen müssen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-158">Subroutine and function declarations are special in that their beginning and end statements must each start at the beginning of a logical line.</span></span> <span data-ttu-id="b0f1b-159">Außerdem muss der Text einer nicht-`MustOverride`-Unterroutine oder Funktionsdeklaration am Anfang einer logischen Zeile beginnen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-159">Additionally, the body of a non-`MustOverride` subroutine or function declaration must start at the beginning of a logical line.</span></span> <span data-ttu-id="b0f1b-160">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-160">For example:</span></span>

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


### <a name="external-method-declarations"></a><span data-ttu-id="b0f1b-161">Externe Methoden Deklarationen</span><span class="sxs-lookup"><span data-stu-id="b0f1b-161">External Method Declarations</span></span>

<span data-ttu-id="b0f1b-162">Eine externe Methoden Deklaration führt eine neue Methode ein, deren Implementierung außerhalb des Programms bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-162">An external method declaration introduces a new method whose implementation is provided external to the program.</span></span>

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

<span data-ttu-id="b0f1b-163">Da eine externe Methoden Deklaration keine tatsächliche Implementierung bereitstellt, verfügt sie über keinen Methoden Text oder `End`-Konstrukt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-163">Because an external method declaration provides no actual implementation, it has no method body or `End` construct.</span></span> <span data-ttu-id="b0f1b-164">Externe Methoden werden implizit freigegeben, haben möglicherweise keine Typparameter und behandeln keine Ereignisse oder implementieren Schnittstellenmember.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-164">External methods are implicitly shared, may not have type parameters, and may not handle events or implement interface members.</span></span> <span data-ttu-id="b0f1b-165">Wenn für eine Funktion kein Rückgabetyp angegeben wird und eine strikte Semantik verwendet wird, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-165">If no return type is specified on a function and strict semantics are being used, a compile-time error occurs.</span></span> <span data-ttu-id="b0f1b-166">Andernfalls ist der Typ implizit `Object` oder der Typ des Typzeichens der Methode.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-166">Otherwise the type is implicitly `Object` or the type of the method's type character.</span></span> <span data-ttu-id="b0f1b-167">Die Zugriffs Domäne der Rückgabetyp-und Parametertypen einer externen Methode muss mit oder einer übergeordneten Zugriffs Domäne der externen Methode selbst identisch sein.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-167">The accessibility domain of the return type and parameter types of an external method must be the same as or a superset of the accessibility domain of the external method itself.</span></span>

<span data-ttu-id="b0f1b-168">Die Library-Klausel einer externen Methoden Deklaration gibt den Namen der externen Datei an, die die-Methode implementiert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-168">The library clause of an external method declaration specifies the name of the external file that implements the method.</span></span> <span data-ttu-id="b0f1b-169">Die optionale Alias Klausel ist eine Zeichenfolge, die die numerische Ordinalzahl (mit dem Präfix "`#`") oder den Namen der Methode in der externen Datei angibt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-169">The optional alias clause is a string that specifies the numeric ordinal (prefixed by a `#` character) or name of the method in the external file.</span></span> <span data-ttu-id="b0f1b-170">Es kann auch ein Einzelzeichen-set-Modifizierer angegeben werden, der den Zeichensatz steuert, der beim Aufrufen der externen Methode zum Mars Hallen von Zeichen folgen verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-170">A single-character set modifier may also be specified, which governs the character set used to marshal strings during a call to the external method.</span></span> <span data-ttu-id="b0f1b-171">Der `Unicode`-Modifizierer Marshallen alle Zeichen folgen in Unicode-Werte, der `Ansi`-Modifizierer Marshallen alle Zeichen folgen in ANSI-Werte, und der `Auto`-Modifizierer Marshallen die Zeichen folgen gemäß .NET Framework Regeln basierend auf dem Namen der Methode oder dem Aliasnamen, falls angegeben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-171">The `Unicode` modifier marshals all strings to Unicode values, the `Ansi` modifier marshals all strings to ANSI values, and the `Auto` modifier marshals the strings according to .NET Framework rules based on the name of the method, or the alias name if specified.</span></span> <span data-ttu-id="b0f1b-172">Wenn kein Modifizierer angegeben wird, ist der Standardwert `Ansi`.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-172">If no modifier is specified, the default is `Ansi`.</span></span>

<span data-ttu-id="b0f1b-173">Wenn `Ansi` oder `Unicode` angegeben ist, wird der Methodenname ohne Änderung in der externen Datei gesucht.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-173">If `Ansi` or `Unicode` is specified, then the method name is looked up in the external file with no modification.</span></span> <span data-ttu-id="b0f1b-174">Wenn `Auto` angegeben ist, hängt die Methodennamen Suche von der Plattform ab.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-174">If `Auto` is specified, then method name lookup depends on the platform.</span></span> <span data-ttu-id="b0f1b-175">Wenn die Plattform als ANSI (z. b. Windows 95, Windows 98, Windows Me) eingestuft wird, wird der Methodenname ohne Änderung gesucht.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-175">If the platform is considered to be ANSI (for example, Windows 95, Windows 98, Windows ME), then the method name is looked up with no modification.</span></span> <span data-ttu-id="b0f1b-176">Wenn bei der Suche ein Fehler auftritt, wird eine `A` angefügt und die Suche erneut versucht.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-176">If the lookup fails, an `A` is appended and the lookup tried again.</span></span> <span data-ttu-id="b0f1b-177">Wenn die Plattform als Unicode angesehen wird (z. b. Windows NT, Windows 2000, Windows XP), wird eine `W` angehängt und der Name gesucht.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-177">If the platform is considered to be Unicode (for example, Windows NT, Windows 2000, Windows XP), then a `W` is appended and the name is looked up.</span></span> <span data-ttu-id="b0f1b-178">Wenn bei der Suche ein Fehler auftritt, wird die Suche ohne den `W` erneut versucht.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-178">If the lookup fails, the lookup is tried again without the `W`.</span></span> <span data-ttu-id="b0f1b-179">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-179">For example:</span></span>

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

<span data-ttu-id="b0f1b-180">Datentypen, die an externe Methoden übermittelt werden, werden gemäß den .NET Framework Datenmarshallingkonventionen mit einer Ausnahme gemarshallt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-180">Data types being passed to external methods are marshaled according to the .NET Framework data marshalling conventions with one exception.</span></span> <span data-ttu-id="b0f1b-181">Zeichen folgen Variablen, die als Wert (d. h. `ByVal x As String`) übergebenen werden, werden an den OLE-automatisierungbstr-Typ gemarshallt, und Änderungen, die an BSTR in der externen Methode vorgenommen werden, werden im Zeichen folgen Argument wiedergegeben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-181">String variables that are passed by value (that is, `ByVal x As String`) are marshaled to the OLE Automation BSTR type, and changes made to the BSTR in the external method are reflected back in the string argument.</span></span> <span data-ttu-id="b0f1b-182">Dies liegt daran, dass der Typ `String` in externen Methoden änderbar ist und diese besondere Marshalling dieses Verhalten imitiert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-182">This is because the type `String` in external methods is mutable, and this special marshalling mimics that behavior.</span></span> <span data-ttu-id="b0f1b-183">Zeichen folgen Parameter, die als Verweis (z. b. `ByRef x As String`) übergebenen werden, werden als Zeiger auf den Typ der OLE-Automatisierungs BSTR gemarshallt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-183">String parameters that are passed by reference (i.e. `ByRef x As String`) are marshaled as a pointer to the OLE Automation BSTR type.</span></span> <span data-ttu-id="b0f1b-184">Sie können diese speziellen Verhalten überschreiben, indem Sie das `System.Runtime.InteropServices.MarshalAsAttribute`-Attribut für den Parameter angeben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-184">It is possible to override these special behaviors by specifying the `System.Runtime.InteropServices.MarshalAsAttribute` attribute on the parameter.</span></span>

<span data-ttu-id="b0f1b-185">Das Beispiel veranschaulicht die Verwendung externer Methoden:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-185">The example demonstrates use of external methods:</span></span>

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


### <a name="overridable-methods"></a><span data-ttu-id="b0f1b-186">Über schreibbare Methoden</span><span class="sxs-lookup"><span data-stu-id="b0f1b-186">Overridable Methods</span></span>

<span data-ttu-id="b0f1b-187">Der `Overridable`-Modifizierer gibt an, dass eine Methode über schreibbar ist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-187">The `Overridable` modifier indicates that a method is overridable.</span></span> <span data-ttu-id="b0f1b-188">Der `Overrides`-Modifizierer gibt an, dass eine Methode eine über schreibbare Methode des Basistyps mit der gleichen Signatur überschreibt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-188">The `Overrides` modifier indicates that a method overrides a base-type overridable method that has the same signature.</span></span> <span data-ttu-id="b0f1b-189">Der `NotOverridable`-Modifizierer gibt an, dass eine über schreibbare Methode nicht weiter überschrieben werden kann.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-189">The `NotOverridable` modifier indicates that an overridable method cannot be further overridden.</span></span> <span data-ttu-id="b0f1b-190">Der `MustOverride`-Modifizierer gibt an, dass eine Methode in abgeleiteten Klassen überschrieben werden muss.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-190">The `MustOverride` modifier indicates that a method must be overridden in derived classes.</span></span>

<span data-ttu-id="b0f1b-191">Bestimmte Kombinationen dieser Modifizierer sind ungültig:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-191">Certain combinations of these modifiers are not valid:</span></span>

* <span data-ttu-id="b0f1b-192">`Overridable` und `NotOverridable` schließen sich gegenseitig aus und können nicht kombiniert werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-192">`Overridable` and `NotOverridable` are mutually exclusive and cannot be combined.</span></span>

* <span data-ttu-id="b0f1b-193">`MustOverride` impliziert `Overridable` (und kann daher nicht angegeben werden) und kann nicht mit `NotOverridable` kombiniert werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-193">`MustOverride` implies `Overridable` (and so cannot specify it) and cannot be combined with `NotOverridable`.</span></span>

* <span data-ttu-id="b0f1b-194">`NotOverridable` kann nicht mit `Overridable` oder `MustOverride` kombiniert werden und muss mit `Overrides` kombiniert werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-194">`NotOverridable` cannot be combined with `Overridable` or `MustOverride` and must be combined with `Overrides`.</span></span>

* <span data-ttu-id="b0f1b-195">`Overrides` impliziert `Overridable` (und kann daher nicht angegeben werden) und kann nicht mit `MustOverride` kombiniert werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-195">`Overrides` implies `Overridable` (and so cannot specify it) and cannot be combined with `MustOverride`.</span></span>

<span data-ttu-id="b0f1b-196">Außerdem gibt es zusätzliche Einschränkungen für über schreibbare Methoden:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-196">There are also additional restrictions on overridable methods:</span></span>

* <span data-ttu-id="b0f1b-197">Eine `MustOverride`-Methode darf keinen Methoden Text oder ein `End`-Konstrukt enthalten, kann keine andere Methode überschreiben und darf nur in `MustInherit`-Klassen vorkommen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-197">A `MustOverride` method may not include a method body or an `End` construct, may not override another method, and may only appear in `MustInherit` classes.</span></span>

* <span data-ttu-id="b0f1b-198">Wenn eine Methode `Overrides` angibt und keine übereinstimmende Basis Methode zum Überschreiben vorhanden ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-198">If a method specifies `Overrides` and there is no matching base method to override, a compile-time error occurs.</span></span> <span data-ttu-id="b0f1b-199">Eine über schreibende Methode darf nicht `Shadows` angeben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-199">An overriding method may not specify `Shadows`.</span></span>

* <span data-ttu-id="b0f1b-200">Eine Methode kann möglicherweise keine andere Methode überschreiben, wenn die Barrierefreiheits Domäne der über schreibenden Methode nicht gleich der Zugriffs Domäne der Methode ist, die überschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-200">A method may not override another method if the overriding method's accessibility domain is not equal to the accessibility domain of the method being overridden.</span></span> <span data-ttu-id="b0f1b-201">Die einzige Ausnahme ist, dass eine Methode, die eine `Protected Friend`-Methode in einer anderen Assembly überschreibt, die keinen `Friend`-Zugriff hat, `Protected` (nicht `Protected Friend`) angeben muss.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-201">The one exception is that a method overriding a `Protected Friend` method in another assembly that does not have `Friend` access must specify `Protected` (not `Protected Friend`).</span></span>

* <span data-ttu-id="b0f1b-202">`Private`-Methoden sind möglicherweise nicht `Overridable`, `NotOverridable` oder `MustOverride` und können auch andere Methoden überschreiben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-202">`Private` methods may not be `Overridable`, `NotOverridable`, or `MustOverride`, nor may they override other methods.</span></span>

* <span data-ttu-id="b0f1b-203">Methoden in `NotInheritable`-Klassen dürfen nicht als `Overridable` oder `MustOverride` deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-203">Methods in `NotInheritable` classes may not be declared `Overridable` or `MustOverride`.</span></span>

<span data-ttu-id="b0f1b-204">Das folgende Beispiel veranschaulicht die Unterschiede zwischen über schreibbaren und nicht über schreibbaren Methoden:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-204">The following example illustrates the differences between overridable and nonoverridable methods:</span></span>

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

<span data-ttu-id="b0f1b-205">Im Beispiel führt Class `Base` eine Methode ein `F` und eine `Overridable`-Methode `G`.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-205">In the example, class `Base` introduces a method `F` and an `Overridable` method `G`.</span></span> <span data-ttu-id="b0f1b-206">Mit der-Klasse `Derived` wird eine neue Methode `F` eingeführt, wodurch die geerbte `F` shadoassen und außerdem die geerbte Methode `G` überschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-206">The class `Derived` introduces a new method `F`, thus shadowing the inherited `F`, and also overrides the inherited method `G`.</span></span> <span data-ttu-id="b0f1b-207">Das Beispiel führt zur folgenden Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-207">The example produces the following output:</span></span>

```console
Base.F
Derived.F
Derived.G
Derived.G
```

<span data-ttu-id="b0f1b-208">Beachten Sie, dass die-Anweisung `b.G()` `Derived.G` aufruft, nicht `Base.G`.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-208">Notice that the statement `b.G()` invokes `Derived.G`, not `Base.G`.</span></span> <span data-ttu-id="b0f1b-209">Dies liegt daran, dass der Lauf Zeittyp der-Instanz (die `Derived` ist) und nicht der Kompilier Zeittyp der-Instanz (`Base`) die tatsächliche Methoden Implementierung bestimmt, die aufgerufen werden soll.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-209">This is because the run-time type of the instance (which is `Derived`) rather than the compile-time type of the instance (which is `Base`) determines the actual method implementation to invoke.</span></span>

### <a name="shared-methods"></a><span data-ttu-id="b0f1b-210">Freigegebene Methoden</span><span class="sxs-lookup"><span data-stu-id="b0f1b-210">Shared Methods</span></span>

<span data-ttu-id="b0f1b-211">Der `Shared`-Modifizierer gibt an, dass eine Methode eine frei *gegebene Methode*ist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-211">The `Shared` modifier indicates a method is a *shared method*.</span></span> <span data-ttu-id="b0f1b-212">Eine freigegebene Methode funktioniert nicht für eine bestimmte Instanz eines Typs und kann direkt von einem Typ und nicht über eine bestimmte Instanz eines Typs aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-212">A shared method does not operate on a specific instance of a type and may be invoked directly from a type rather than through a particular instance of a type.</span></span> <span data-ttu-id="b0f1b-213">Es ist jedoch gültig, eine gemeinsame Methode mit einer-Instanz zu qualifizieren.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-213">It is valid, however, to use an instance to qualify a shared method.</span></span> <span data-ttu-id="b0f1b-214">Es ist ungültig, in einer freigegebenen Methode auf `Me`, `MyClass` oder `MyBase` zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-214">It is invalid to refer to `Me`, `MyClass`, or `MyBase` in a shared method.</span></span> <span data-ttu-id="b0f1b-215">Freigegebene Methoden dürfen nicht `Overridable`, `NotOverridable` oder `MustOverride` sein, und Sie können keine Methoden überschreiben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-215">Shared methods may not be `Overridable`, `NotOverridable`, or `MustOverride`, and they may not override methods.</span></span> <span data-ttu-id="b0f1b-216">In Standardmodulen und Schnittstellen definierte Methoden dürfen nicht `Shared` angeben, da Sie bereits implizit `Shared` sind.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-216">Methods defined in standard modules and interfaces may not specify `Shared`, because they are implicitly `Shared` already.</span></span>

<span data-ttu-id="b0f1b-217">Eine Methode, die in einer Struktur oder Klasse ohne `Shared`-Modifizierer deklariert wird, ist eine *Instanzmethode*.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-217">A method declared in a structure or class without a `Shared` modifier is an *instance method*.</span></span> <span data-ttu-id="b0f1b-218">Eine Instanzmethode funktioniert für eine bestimmte Instanz eines Typs.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-218">An instance method operates on a given instance of a type.</span></span> <span data-ttu-id="b0f1b-219">Instanzmethoden können nur über eine Instanz eines Typs aufgerufen werden und können über den `Me`-Ausdruck auf die Instanz verweisen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-219">Instance methods can only be invoked through an instance of a type and may refer to the instance through the `Me` expression.</span></span>

<span data-ttu-id="b0f1b-220">Das folgende Beispiel veranschaulicht die Regeln für den Zugriff auf freigegebene Member und Instanzmember:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-220">The following example illustrates the rules for accessing shared and instance members:</span></span>

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

<span data-ttu-id="b0f1b-221">Die Methode `F` zeigt, dass ein Bezeichner in einem Instanzfunktionsmember für den Zugriff auf Instanzmember und freigegebene Member verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-221">Method `F` shows that in an instance function member, an identifier can be used to access both instance members and shared members.</span></span> <span data-ttu-id="b0f1b-222">Die Methode `G` zeigt, dass in einem freigegebenen Funktionsmember ein Fehler aufgetreten ist, um über einen Bezeichner auf einen Instanzmember zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-222">Method `G` shows that in a shared function member, it is an error to access an instance member through an identifier.</span></span> <span data-ttu-id="b0f1b-223">Die Methode `Main` zeigt, dass in einem Member-Zugriffs Ausdruck auf Instanzmember über-Instanzen zugegriffen werden muss, auf freigegebene Member kann jedoch über Typen oder Instanzen zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-223">Method `Main` shows that in a member access expression, instance members must be accessed through instances, but shared members can be accessed through types or instances.</span></span>

### <a name="method-parameters"></a><span data-ttu-id="b0f1b-224">Methodenparameter</span><span class="sxs-lookup"><span data-stu-id="b0f1b-224">Method Parameters</span></span>

<span data-ttu-id="b0f1b-225">Ein *Parameter* ist eine Variable, die verwendet werden kann, um Informationen an eine Methode zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-225">A *parameter* is a variable that can be used to pass information into and out of a method.</span></span> <span data-ttu-id="b0f1b-226">Parameter einer Methode werden durch die Parameterliste der Methode deklariert, die aus einem oder mehreren Parametern besteht, die durch Kommas getrennt sind.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-226">Parameters of a method are declared by the method's parameter list, which consists of one or more parameters separated by commas.</span></span>

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

<span data-ttu-id="b0f1b-227">Wenn für einen Parameter kein Typ angegeben ist und eine strikte Semantik verwendet wird, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-227">If no type is specified for a parameter and strict semantics are used, a compile-time error occurs.</span></span> <span data-ttu-id="b0f1b-228">Andernfalls ist der Standardtyp `Object` oder der Typ des Typzeichens des Parameters.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-228">Otherwise the default type is `Object` or the type of the parameter's type character.</span></span> <span data-ttu-id="b0f1b-229">Selbst bei einer einschränkend sein Semantik müssen alle Parametertypen angeben, wenn ein Parameter eine `As`-Klausel enthält.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-229">Even under permissive semantics, if one parameter includes an `As` clause, all parameters must specify types.</span></span>

<span data-ttu-id="b0f1b-230">Parameter werden durch die Modifizierer `ByVal`, `ByRef`, `Optional` und `ParamArray` als Wert, Verweis, optional oder ParamArray-Parameter angegeben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-230">Parameters are specified as value, reference, optional, or paramarray parameters by the modifiers `ByVal`, `ByRef`, `Optional`, and `ParamArray`, respectively.</span></span> <span data-ttu-id="b0f1b-231">Ein Parameter, der `ByRef` oder `ByVal` nicht angibt, ist standardmäßig `ByVal`.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-231">A parameter that does not specify `ByRef` or `ByVal` defaults to `ByVal`.</span></span>

<span data-ttu-id="b0f1b-232">Parameter Namen werden auf den gesamten Text der Methode festgelegt und sind immer öffentlich zugänglich.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-232">Parameter names are scoped to the entire body of the method and are always publicly accessible.</span></span> <span data-ttu-id="b0f1b-233">Ein Methodenaufruf erstellt eine Kopie, die für diesen Aufruf spezifisch ist, und die Argumentliste des aufzurufenden Parameters weist Werte oder Variablen Verweise den neu erstellten Parametern zu.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-233">A method invocation creates a copy, specific to that invocation, of the parameters, and the argument list of the invocation assigns values or variable references to the newly created parameters.</span></span> <span data-ttu-id="b0f1b-234">Da externe Methoden Deklarationen und Delegatdeklarationen keinen Text aufweisen, sind doppelte Parameternamen in Parameterlisten zulässig, jedoch nicht empfehlenswert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-234">Because external method declarations and delegate declarations have no body, duplicate parameter names are allowed in parameter lists, but discouraged.</span></span>

<span data-ttu-id="b0f1b-235">Auf den Bezeichner kann der namensmodifizierer, der NULL-Werte zulässt, `?`, um anzugeben, dass er NULL-Werte zulässt, und auch nach arraynamensmodifizierern, um anzugeben, dass es sich um ein Array</span><span class="sxs-lookup"><span data-stu-id="b0f1b-235">The identifier may be followed by the nullable name modifier `?` to indicate that it is nullable, and also by array name modifiers to indicate that it is an array.</span></span> <span data-ttu-id="b0f1b-236">Sie können kombiniert werden, z. b. "`ByVal x?() As Integer`".</span><span class="sxs-lookup"><span data-stu-id="b0f1b-236">They may be combined, e.g. "`ByVal x?() As Integer`".</span></span> <span data-ttu-id="b0f1b-237">Es ist nicht zulässig, explizite Array Begrenzungen zu verwenden. auch wenn der Modifizierer "Werte zulässt Name" vorhanden ist, muss eine `As`-Klausel vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-237">It is not allowed to use explicit array bounds; also, if the nullable name modifier is present then an `As` clause must be present.</span></span>


#### <a name="value-parameters"></a><span data-ttu-id="b0f1b-238">Wert Parameter</span><span class="sxs-lookup"><span data-stu-id="b0f1b-238">Value Parameters</span></span>

<span data-ttu-id="b0f1b-239">Ein *value-Parameter* wird mit einem expliziten `ByVal`-Modifizierer deklariert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-239">A *value parameter* is declared with an explicit `ByVal` modifier.</span></span> <span data-ttu-id="b0f1b-240">Wenn der `ByVal`-Modifizierer verwendet wird, kann der `ByRef`-Modifizierer nicht angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-240">If the `ByVal` modifier is used, the `ByRef` modifier may not be specified.</span></span> <span data-ttu-id="b0f1b-241">Ein value-Parameter wird mit dem Aufruf des Members, zu dem der Parameter gehört, vorhanden sein, und er wird mit dem Wert des im Aufruf angegebenen Arguments initialisiert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-241">A value parameter comes into existence with the invocation of the member the parameter belongs to, and is initialized with the value of the argument given in the invocation.</span></span> <span data-ttu-id="b0f1b-242">Ein value-Parameter ist bei der Rückgabe des Members nicht mehr vorhanden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-242">A value parameter ceases to exist upon return of the member.</span></span>

<span data-ttu-id="b0f1b-243">Eine Methode darf einem Wert Parameter neue Werte zuweisen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-243">A method is permitted to assign new values to a value parameter.</span></span> <span data-ttu-id="b0f1b-244">Solche Zuweisungen wirken sich nur auf den lokalen Speicherort aus, der durch den Wert Parameter dargestellt wird. Sie haben keine Auswirkung auf das tatsächliche Argument, das im Methodenaufruf angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-244">Such assignments only affect the local storage location represented by the value parameter; they have no effect on the actual argument given in the method invocation.</span></span>

<span data-ttu-id="b0f1b-245">Ein value-Parameter wird verwendet, wenn der Wert eines Arguments an eine Methode übergeben wird, und Änderungen des Parameters haben keine Auswirkung auf das ursprüngliche Argument.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-245">A value parameter is used when the value of an argument is passed into a method, and modifications of the parameter do not impact the original argument.</span></span> <span data-ttu-id="b0f1b-246">Ein value-Parameter verweist auf eine eigene Variable, die sich von der Variablen des entsprechenden Arguments unterscheidet.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-246">A value parameter refers to its own variable, one that is distinct from the variable of the corresponding argument.</span></span> <span data-ttu-id="b0f1b-247">Diese Variable wird initialisiert, indem der Wert des entsprechenden Arguments kopiert wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-247">This variable is initialized by copying the value of the corresponding argument.</span></span> <span data-ttu-id="b0f1b-248">Das folgende Beispiel zeigt eine Methode `F` mit einem value-Parameter mit dem Namen `p`:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-248">The following example shows a method `F` that has a value parameter named `p`:</span></span>

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

<span data-ttu-id="b0f1b-249">Im Beispiel wird die folgende Ausgabe erzeugt, auch wenn der value-Parameter `p` geändert wird:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-249">The example produces the following output, even though the value parameter `p` is modified:</span></span>

```console
pre: a = 1
p = 1
post: a = 1
```

#### <a name="reference-parameters"></a><span data-ttu-id="b0f1b-250">Verweis Parameter</span><span class="sxs-lookup"><span data-stu-id="b0f1b-250">Reference Parameters</span></span>

<span data-ttu-id="b0f1b-251">Ein Verweis Parameter ist ein Parameter, der mit einem `ByRef`-Modifizierer deklariert wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-251">A reference parameter is a parameter declared with a `ByRef` modifier.</span></span> <span data-ttu-id="b0f1b-252">Wenn der `ByRef`-Modifizierer angegeben ist, kann der `ByVal`-Modifizierer nicht verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-252">If the `ByRef` modifier is specified, the `ByVal` modifier may not be used.</span></span> <span data-ttu-id="b0f1b-253">Ein Verweis Parameter erstellt keinen neuen Speicherort.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-253">A reference parameter does not create a new storage location.</span></span> <span data-ttu-id="b0f1b-254">Stattdessen stellt ein Verweis Parameter die Variable dar, die als Argument in der Methode oder dem Konstruktoraufruf angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-254">Instead, a reference parameter represents the variable given as the argument in the method or constructor invocation.</span></span> <span data-ttu-id="b0f1b-255">Konzeptionell ist der Wert eines Verweis Parameters immer mit der zugrunde liegenden Variablen identisch.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-255">Conceptually, the value of a reference parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="b0f1b-256">Verweis Parameter agieren in zwei Modi, entweder als *Aliase* oder durch Kopieren von Kopier *Vorgängen.*</span><span class="sxs-lookup"><span data-stu-id="b0f1b-256">Reference parameters act in two modes, either as *aliases* or through *copy-in copy-back.*</span></span>

<span data-ttu-id="b0f1b-257">__Aliase.__</span><span class="sxs-lookup"><span data-stu-id="b0f1b-257">__Aliases.__</span></span> <span data-ttu-id="b0f1b-258">Ein Verweis Parameter wird verwendet, wenn der Parameter als Alias für ein vom Aufrufer bereitgestelltes Argument fungiert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-258">A reference parameter is used when the parameter acts as an alias for a caller-provided argument.</span></span> <span data-ttu-id="b0f1b-259">Ein Verweis Parameter definiert nicht selbst eine Variable, sondern verweist stattdessen auf die Variable des entsprechenden Arguments.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-259">A reference parameter does not itself define a variable, but rather refers to the variable of the corresponding argument.</span></span> <span data-ttu-id="b0f1b-260">Änderungen eines Verweis Parameters direkt und wirken sich unmittelbar auf das entsprechende Argument aus.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-260">Modifications of a reference parameter directly and immediately impact the corresponding argument.</span></span> <span data-ttu-id="b0f1b-261">Das folgende Beispiel zeigt eine Methode `Swap` mit zwei Verweis Parametern:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-261">The following example shows a method `Swap` that has two reference parameters:</span></span>

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

<span data-ttu-id="b0f1b-262">Die Ausgabe des Programms lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-262">The output of the program is:</span></span>

```console
pre: x = 1, y = 2
post: x = 2, y = 1
```

<span data-ttu-id="b0f1b-263">Für den Aufruf der-Methode `Swap` in der-Klasse `Main` stellt `a` `x,` und `b` `y` dar.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-263">For the invocation of method `Swap` in class `Main`, `a` represents `x,` and `b` represents `y`.</span></span> <span data-ttu-id="b0f1b-264">Folglich hat der Aufruf die Auswirkungen, die Werte von `x` und `y` zu tauschen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-264">Thus, the invocation has the effect of swapping the values of `x` and `y`.</span></span>

<span data-ttu-id="b0f1b-265">In einer Methode, die Verweis Parameter annimmt, ist es möglich, dass mehrere Namen denselben Speicherort darstellen:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-265">In a method that takes reference parameters, it is possible for multiple names to represent the same storage location:</span></span>

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

<span data-ttu-id="b0f1b-266">Im Beispiel übergibt der Aufruf der Methode `F` in `G` einen Verweis auf `s` für `a` und `b`.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-266">In the example the invocation of method `F` in `G` passes a reference to `s` for both `a` and `b`.</span></span> <span data-ttu-id="b0f1b-267">Daher verweisen die Namen `s`, `a` und `b` auf denselben Speicherort, und die drei Zuweisungen ändern alle die Instanzvariable `s`.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-267">Thus, for that invocation, the names `s`, `a`, and `b` all refer to the same storage location, and the three assignments all modify the instance variable `s`.</span></span>

<span data-ttu-id="b0f1b-268">__Kopiervorgang kopieren.__</span><span class="sxs-lookup"><span data-stu-id="b0f1b-268">__Copy-in copy-back.__</span></span> <span data-ttu-id="b0f1b-269">Wenn der Typ der Variablen, die an einen Verweis Parameter übergeben wird, nicht mit dem Typ des Verweis Parameters kompatibel ist oder wenn eine nicht-Variable (z. b. eine Eigenschaft) als Argument an einen Verweis Parameter übergeben wird, oder wenn der Aufruf spät gebunden ist, wird ein temporärer die Variable wird zugewiesen und an den Verweis Parameter übergeben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-269">If the type of the variable being passed to a reference parameter is not compatible with the reference parameter's type, or if a non-variable (e.g. a property) is passed as an argument to a reference parameter, or if the invocation is late-bound, then a temporary variable is allocated and passed to the reference parameter.</span></span> <span data-ttu-id="b0f1b-270">Der Wert, der in der Spalte verwendet wird, wird in diese temporäre Variable kopiert, bevor die-Methode aufgerufen wird. Sie wird zurück in die ursprüngliche Variable (sofern vorhanden, und wenn Sie beschreibbar ist) kopiert, wenn die-Methode zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-270">The value being passed in will be copied into this temporary variable before the method is invoked and will be copied back to the original variable (if there is one and if it's writable) when the method returns.</span></span> <span data-ttu-id="b0f1b-271">Folglich kann ein Verweis Parameter nicht notwendigerweise einen Verweis auf die genaue Speicherung der übergebenen Variable enthalten, und alle Änderungen am Verweis Parameter werden möglicherweise erst in der Variablen widergespiegelt, wenn die Methode beendet wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-271">Thus, a reference parameter may not necessarily contain a reference to the exact storage of the variable being passed in, and any changes to the reference parameter may not be reflected in the variable until the method exits.</span></span> <span data-ttu-id="b0f1b-272">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-272">For example:</span></span>

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

<span data-ttu-id="b0f1b-273">Beim ersten Aufruf von `F` wird eine temporäre Variable erstellt und der Wert der Eigenschaft `G` zugewiesen und an `F` geleitet.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-273">In the case of the first invocation of `F`, a temporary variable is created and the value of the property `G` is assigned to it and passed into `F`.</span></span> <span data-ttu-id="b0f1b-274">Bei der Rückgabe von `F` wird der Wert in der temporären Variablen der Eigenschaft `G` wieder zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-274">Upon return from `F`, the value in the temporary variable is assigned back to the property of `G`.</span></span> <span data-ttu-id="b0f1b-275">Im zweiten Fall wird eine weitere temporäre Variable erstellt und der Wert `d` zugewiesen und an `F` übermittelt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-275">In the second case, another temporary variable is created and the value of `d` is assigned to it and passed into `F`.</span></span> <span data-ttu-id="b0f1b-276">Bei der Rückgabe von `F` wird der Wert in der temporären Variablen zurück in den Typ der Variablen umgewandelt, `Derived` und `d` zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-276">When returning from `F`, the value in the temporary variable is cast back to the type of the variable, `Derived`, and assigned to `d`.</span></span> <span data-ttu-id="b0f1b-277">Da der zurückgegebene Wert nicht in `Derived` umgewandelt werden kann, wird zur Laufzeit eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-277">Since the value being passed back cannot be cast to `Derived`, an exception is thrown at run time.</span></span>

#### <a name="optional-parameters"></a><span data-ttu-id="b0f1b-278">Optionale Parameter</span><span class="sxs-lookup"><span data-stu-id="b0f1b-278">Optional Parameters</span></span>

<span data-ttu-id="b0f1b-279">Ein optionaler Parameter wird mit dem `Optional`-Modifizierer deklariert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-279">An optional parameter is declared with the `Optional` modifier.</span></span> <span data-ttu-id="b0f1b-280">Parameter, die auf einen optionalen Parameter in der Liste formaler Parameter folgen, müssen ebenfalls optional sein. Wenn Sie den `Optional`-Modifizierer für die folgenden Parameter nicht angeben, wird ein Kompilierzeitfehler ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-280">Parameters that follow an optional parameter in the formal parameter list must be optional as well; failure to specify the `Optional` modifier on the following parameters will trigger a compile-time error.</span></span> <span data-ttu-id="b0f1b-281">Ein optionaler Parameter eines Typs, der NULL-Werte zulässt, `T?` oder ein Typ, der keine NULL-Werte zulässt `T` muss einen konstanten Ausdruck `e` angeben, der als Standardwert verwendet werden soll, wenn kein Argument angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-281">An optional parameter of some type nullable type `T?` or non-nullable type `T` must specify a constant expression `e` to be used as a default value if no argument is specified.</span></span> <span data-ttu-id="b0f1b-282">Wenn `e` als `Nothing` vom Typ Object ausgewertet wird, wird der Standardwert des *Parameter Typs* als Standardwert für den Parameter verwendet.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-282">If `e` evaluates to `Nothing` of type Object, then the default value of the *parameter type* will be used as the default for the parameter.</span></span> <span data-ttu-id="b0f1b-283">Andernfalls muss `CType(e, T)` ein konstanter Ausdruck sein, der als Standardwert für den Parameter verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-283">Otherwise, `CType(e, T)` must be a constant expression and it is taken as the default for the parameter.</span></span>

<span data-ttu-id="b0f1b-284">Optionale Parameter sind die einzige Situation, in der ein Initialisierer für einen Parameter gültig ist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-284">Optional parameters are the only situation in which an initializer on a parameter is valid.</span></span> <span data-ttu-id="b0f1b-285">Die Initialisierung erfolgt immer als Teil des Aufruf Ausdrucks, nicht im Methoden Text selbst.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-285">The initialization is always done as a part of the invocation expression, not within the method body itself.</span></span>

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

<span data-ttu-id="b0f1b-286">Die Ausgabe des Programms lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-286">The output of the program is:</span></span>

```console
x = 10, y = 20
x = 30, y = 40
```

<span data-ttu-id="b0f1b-287">Optionale Parameter dürfen nicht in Delegaten oder Ereignis Deklarationen oder in Lambda Ausdrücken angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-287">Optional parameters may not be specified in delegate or event declarations, nor in lambda expressions.</span></span>

#### <a name="paramarray-parameters"></a><span data-ttu-id="b0f1b-288">ParamArray-Parameter</span><span class="sxs-lookup"><span data-stu-id="b0f1b-288">ParamArray Parameters</span></span>

<span data-ttu-id="b0f1b-289">`ParamArray`-Parameter werden mit dem `ParamArray`-Modifizierer deklariert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-289">`ParamArray` parameters are declared with the `ParamArray` modifier.</span></span> <span data-ttu-id="b0f1b-290">Wenn der `ParamArray`-Modifizierer vorhanden ist, muss der `ByVal`-Modifizierer angegeben werden, und kein anderer Parameter kann den `ParamArray`-Modifizierer verwenden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-290">If the `ParamArray` modifier is present, the `ByVal` modifier must be specified, and no other parameter may use the `ParamArray` modifier.</span></span> <span data-ttu-id="b0f1b-291">Der `ParamArray`-Parametertyp muss ein eindimensionales Array sein, und er muss der letzte Parameter in der Parameterliste sein.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-291">The `ParamArray` parameter's type must be a one-dimensional array, and it must be the last parameter in the parameter list.</span></span>

<span data-ttu-id="b0f1b-292">Ein `ParamArray`-Parameter stellt eine unbestimmte Anzahl von Parametern des Typs der `ParamArray` dar.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-292">A `ParamArray` parameter represents an indeterminate number of parameters of the type of the `ParamArray`.</span></span> <span data-ttu-id="b0f1b-293">Innerhalb der Methode selbst wird der Parameter "`ParamArray`" als sein deklarierter Typ behandelt und hat keine besondere Semantik.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-293">Within the method itself, a `ParamArray` parameter is treated as its declared type and has no special semantics.</span></span> <span data-ttu-id="b0f1b-294">Ein `ParamArray`-Parameter ist implizit optional, wobei der Standardwert ein leeres eindimensionales Array vom Typ des `ParamArray` ist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-294">A `ParamArray` parameter is implicitly optional, with a default value of an empty one-dimensional array of the type of the `ParamArray`.</span></span>

<span data-ttu-id="b0f1b-295">Ein `ParamArray` ermöglicht das Angeben von Argumenten in einem Methodenaufruf auf eine von zwei Arten:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-295">A `ParamArray` permits arguments to be specified in one of two ways in a method invocation:</span></span>

* <span data-ttu-id="b0f1b-296">Das für einen `ParamArray` angegebene Argument kann ein einzelner Ausdruck eines Typs sein, der zum `ParamArray`-Typ erweitert wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-296">The argument given for a `ParamArray` can be a single expression of a type that widens to the `ParamArray` type.</span></span> <span data-ttu-id="b0f1b-297">In diesem Fall verhält sich die `ParamArray` genau wie ein value-Parameter.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-297">In this case, the `ParamArray` acts precisely like a value parameter.</span></span>

* <span data-ttu-id="b0f1b-298">Alternativ kann der Aufruf NULL oder mehr Argumente für den `ParamArray` angeben, wobei jedes Argument ein Ausdruck eines Typs ist, der implizit in den Elementtyp von `ParamArray` konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-298">Alternatively, the invocation can specify zero or more arguments for the `ParamArray`, where each argument is an expression of a type that is implicitly convertible to the element type of the `ParamArray`.</span></span> <span data-ttu-id="b0f1b-299">In diesem Fall erstellt der Aufruf eine Instanz des `ParamArray`-Typs mit einer Länge, die der Anzahl der Argumente entspricht, initialisiert die Elemente der Array Instanz mit den angegebenen Argument Werten und verwendet die neu erstellte Array Instanz als die tatsächliche gestritten.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-299">In this case, the invocation creates an instance of the `ParamArray` type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="b0f1b-300">Mit der Ausnahme, dass eine Variable Anzahl von Argumenten in einem Aufruf zugelassen wird, entspricht ein `ParamArray` genau einem Wert Parameter desselben Typs, wie im folgenden Beispiel veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-300">Except for allowing a variable number of arguments in an invocation, a `ParamArray` is precisely equivalent to a value parameter of the same type, as the following example illustrates.</span></span>

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

<span data-ttu-id="b0f1b-301">Das Beispiel erzeugt die Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-301">The example produces the output</span></span>

```console
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

<span data-ttu-id="b0f1b-302">Beim ersten Aufruf von `F` wird das Array `a` einfach als Wert Parameter übergeben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-302">The first invocation of `F` simply passes the array `a` as a value parameter.</span></span> <span data-ttu-id="b0f1b-303">Der zweite Aufruf von `F` erstellt automatisch ein Array mit vier Elementen mit den angegebenen Element Werten und übergibt diese Array Instanz als Wert Parameter.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-303">The second invocation of `F` automatically creates a four-element array with the given element values and passes that array instance as a value parameter.</span></span> <span data-ttu-id="b0f1b-304">Ebenso erstellt der dritte Aufruf von `F` ein Array mit null Elementen und übergibt diese Instanz als Wert Parameter.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-304">Likewise, the third invocation of `F` creates a zero-element array and passes that instance as a value parameter.</span></span> <span data-ttu-id="b0f1b-305">Der zweite und der dritte Aufruf entsprechen genau dem Schreiben:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-305">The second and third invocations are precisely equivalent to writing:</span></span>

```vb
F(New Integer() {10, 20, 30, 40})
F(New Integer() {})
```

<span data-ttu-id="b0f1b-306">`ParamArray`-Parameter dürfen nicht in Delegaten-oder Ereignis Deklarationen angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-306">`ParamArray` parameters may not be specified in delegate or event declarations.</span></span>

### <a name="event-handling"></a><span data-ttu-id="b0f1b-307">Ereignisbehandlung</span><span class="sxs-lookup"><span data-stu-id="b0f1b-307">Event Handling</span></span>

<span data-ttu-id="b0f1b-308">Methoden können Ereignisse deklarativ verarbeiten, die von Objekten in einer Instanz oder freigegebenen Variablen ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-308">Methods can declaratively handle events raised by objects in instance or shared variables.</span></span> <span data-ttu-id="b0f1b-309">Zum Behandeln von Ereignissen gibt eine Methoden Deklaration das `Handles`-Schlüsselwort an und listet mindestens ein Ereignis auf.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-309">To handle events, a method declaration specifies the `Handles` keyword and lists one or more events.</span></span>

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

<span data-ttu-id="b0f1b-310">Ein Ereignis in der `Handles`-Liste wird durch zwei Bezeichner angegeben, die durch einen bestimmten Zeitraum voneinander getrennt sind:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-310">An event in the `Handles` list is specified by two identifiers separated by a period:</span></span>

* <span data-ttu-id="b0f1b-311">Der erste Bezeichner muss eine Instanz oder eine freigegebene Variable im enthaltenden Typ sein, der den `WithEvents`-Modifizierer oder das `MyBase`-oder `MyClass`-oder `Me`-Schlüsselwort angibt. Andernfalls tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-311">The first identifier must be an instance or shared variable in the containing type that specifies the `WithEvents` modifier or the `MyBase` or `MyClass` or `Me` keyword; otherwise, a compile-time error occurs.</span></span> <span data-ttu-id="b0f1b-312">Diese Variable enthält das-Objekt, das die von dieser Methode behandelten Ereignisse aufhebt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-312">This variable contains the object that will raise the events handled by this method.</span></span>

* <span data-ttu-id="b0f1b-313">Der zweite Bezeichner muss einen Member vom Typ des ersten Bezeichners angeben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-313">The second identifier must specify a member of the type of the first identifier.</span></span> <span data-ttu-id="b0f1b-314">Der Member muss ein Ereignis sein, und er kann freigegeben sein.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-314">The member must be an event, and may be shared.</span></span> <span data-ttu-id="b0f1b-315">Wenn für den ersten Bezeichner eine freigegebene Variable angegeben wird, muss das Ereignis freigegeben werden, oder es muss ein Fehler auftreten.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-315">If a shared variable is specified for the first identifier, then the event must be shared, or an error results.</span></span>

<span data-ttu-id="b0f1b-316">Eine Handlermethode `M` wird als gültiger Ereignishandler für ein Ereignis `E` betrachtet, wenn die Anweisung `AddHandler E, AddressOf M` ebenfalls gültig wäre.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-316">A handler method `M` is considered a valid event handler for an event `E` if the statement `AddHandler E, AddressOf M` would also be valid.</span></span> <span data-ttu-id="b0f1b-317">Anders als bei einer `AddHandler`-Anweisung ermöglichen explizite Ereignishandler jedoch die Behandlung eines Ereignisses mit einer Methode ohne Argumente, unabhängig davon, ob strikte Semantik verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-317">Unlike an `AddHandler` statement, however, explicit event handlers allow handling an event with a method with no arguments regardless of whether strict semantics are being used or not:</span></span>

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

<span data-ttu-id="b0f1b-318">Ein einzelnes Element kann mehrere übereinstimmende Ereignisse verarbeiten, und mehrere Methoden können ein einzelnes Ereignis verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-318">A single member can handle multiple matching events, and multiple methods may handle a single event.</span></span> <span data-ttu-id="b0f1b-319">Die Barrierefreiheit einer Methode hat keine Auswirkung auf die Möglichkeit, Ereignisse zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-319">A method's accessibility has no effect on its ability to handle events.</span></span> <span data-ttu-id="b0f1b-320">Das folgende Beispiel zeigt, wie eine Methode Ereignisse behandeln kann:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-320">The following example shows how a method can handle events:</span></span>

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

<span data-ttu-id="b0f1b-321">Dadurch wird Folgendes gedruckt:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-321">This will print out:</span></span>

```console
Raised
Raised
```

<span data-ttu-id="b0f1b-322">Ein Typ erbt alle Ereignishandler, die vom Basistyp bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-322">A type inherits all event handlers provided by its base type.</span></span> <span data-ttu-id="b0f1b-323">Ein abgeleiteter Typ kann in keiner Weise die Ereignis Zuordnungen ändern, die er von seinen Basis Typen erbt, kann jedoch dem Ereignis weitere Handler hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-323">A derived type cannot in any way alter the event mappings it inherits from its base types, but may add additional handlers to the event.</span></span>


### <a name="extension-methods"></a><span data-ttu-id="b0f1b-324">Erweiterungsmethoden</span><span class="sxs-lookup"><span data-stu-id="b0f1b-324">Extension Methods</span></span>

<span data-ttu-id="b0f1b-325">Methoden können Typen von außerhalb der Typdeklaration mithilfe von *Erweiterungs Methoden*hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-325">Methods can be added to types from outside of the type declaration using *extension methods*.</span></span> <span data-ttu-id="b0f1b-326">Erweiterungs Methoden sind Methoden, auf die das `System.Runtime.CompilerServices.ExtensionAttribute`-Attribut angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-326">Extension methods are methods with the `System.Runtime.CompilerServices.ExtensionAttribute` attribute applied to them.</span></span> <span data-ttu-id="b0f1b-327">Sie können nur in Standardmodulen deklariert werden und müssen über mindestens einen Parameter verfügen, der den Typ angibt, der von der Methode erweitert wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-327">They can only be declared in standard modules and must have at least one parameter, which specifies the type the method extends.</span></span> <span data-ttu-id="b0f1b-328">Mit der folgenden Erweiterungsmethode wird z. b. der-Typ `String` erweitert:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-328">For example, the following extension method extends the type `String`:</span></span>

```vb
Imports System.Runtime.CompilerServices

Module StringExtensions
    <Extension> _
    Sub Print(s As String)
        Console.WriteLine(s)
    End Sub
End Module
```

<span data-ttu-id="b0f1b-329">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="b0f1b-329">__Note.__</span></span> <span data-ttu-id="b0f1b-330">Obwohl Visual Basic Erweiterungs Methoden für die Deklaration in einem Standardmodul erfordert, C# können andere Sprachen, wie z. b., Sie in anderen Arten von Typen deklarieren.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-330">Although Visual Basic requires extension methods to be declared in a standard module, other languages such as C# may allow them to be declared in other kinds of types.</span></span> <span data-ttu-id="b0f1b-331">Solange die Methoden den hier beschriebenen anderen Konventionen folgen und der enthaltende Typ kein offener generischer Typ ist und nicht instanziiert werden kann, wird Visual Basic die Erweiterungs Methoden erkennen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-331">As long as the methods follow the other conventions outlined here and the containing type is not an open generic type and cannot be instantiated, Visual Basic will recognize the extension methods.</span></span>

<span data-ttu-id="b0f1b-332">Wenn eine Erweiterungsmethode aufgerufen wird, wird die Instanz, für die Sie aufgerufen wird, an den ersten Parameter übergeben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-332">When an extension method is invoked, the instance it is being invoked on is passed to the first parameter.</span></span> <span data-ttu-id="b0f1b-333">Der erste Parameter kann nicht als "`Optional`" oder "`ParamArray`" deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-333">The first parameter cannot be declared `Optional` or `ParamArray`.</span></span> <span data-ttu-id="b0f1b-334">Jeder Typ, einschließlich eines Typparameters, kann als erster Parameter einer Erweiterungsmethode angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-334">Any type, including a type parameter, can appear as the first parameter of an extension method.</span></span> <span data-ttu-id="b0f1b-335">Mit den folgenden Methoden werden z. b. die Typen `Integer()`, jeder Typ, der `System.Collections.Generic.IEnumerable(Of T)` implementiert, und ein beliebiger Typ erweitert:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-335">For example, the following methods extend the types `Integer()`, any type that implements `System.Collections.Generic.IEnumerable(Of T)`, and any type at all:</span></span>

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

<span data-ttu-id="b0f1b-336">Wie das vorherige Beispiel zeigt, können Schnittstellen erweitert werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-336">As the previous example shows, interfaces can be extended.</span></span> <span data-ttu-id="b0f1b-337">Schnittstellen Erweiterungs Methoden stellen die Implementierung der-Methode bereit, sodass Typen, die eine Schnittstelle implementieren, für die Erweiterungs Methoden definiert sind, immer noch die von der Schnittstelle deklarierten Member implementieren.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-337">Interface extension methods supply the implementation of the method, so types that implement an interface that has extension methods defined on it still only implement the members originally declared by the interface.</span></span> <span data-ttu-id="b0f1b-338">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-338">For example:</span></span>

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

<span data-ttu-id="b0f1b-339">Erweiterungs Methoden können auch Typeinschränkungen für ihre Typparameter aufweisen, und genau wie bei generischen Methoden ohne Erweiterung kann das Typargument abgeleitet werden:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-339">Extension methods can also have type constraints on their type parameters and, just as with non-extension generic methods, type argument can be inferred:</span></span>

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

<span data-ttu-id="b0f1b-340">Auf Erweiterungs Methoden kann auch über implizite Instanzausdrücke innerhalb des erweiterten Typs zugegriffen werden:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-340">Extension methods can also be accessed through implicit instance expressions within the type being extended:</span></span>

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

<span data-ttu-id="b0f1b-341">Für den Zugriff auf die Barrierefreiheit werden Erweiterungs Methoden auch als Member des Standardmoduls behandelt, in dem Sie deklariert werden. Sie haben keinen zusätzlichen Zugriff auf die Member des Typs, den Sie über den Zugriff haben, den Sie aufgrund ihres Deklarations Kontexts haben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-341">For the purposes of accessibility, extension methods are also treated as members of the standard module they are declared in -- they have no extra access to the members of the type they are extending beyond the access they have by virtue of their declaration context.</span></span>

<span data-ttu-id="b0f1b-342">Erweiterungs Methoden sind nur verfügbar, wenn sich die Standardmodul Methode im Gültigkeitsbereich befindet.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-342">Extensions methods are only available when the standard module method is in scope.</span></span> <span data-ttu-id="b0f1b-343">Andernfalls wird der ursprüngliche Typ anscheinend nicht erweitert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-343">Otherwise, the original type will not appear to have been extended.</span></span> <span data-ttu-id="b0f1b-344">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-344">For example:</span></span>

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

<span data-ttu-id="b0f1b-345">Ein Verweis auf einen Typ, wenn nur eine Erweiterungsmethode für den Typ verfügbar ist, erzeugt trotzdem einen Kompilierzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-345">Referring to a type when only an extension method on the type is available will still produce a compile-time error.</span></span>

<span data-ttu-id="b0f1b-346">Es ist wichtig zu beachten, dass Erweiterungs Methoden als Member des Typs in allen Kontexten angesehen werden, in denen Member gebunden werden, z. b. das stark typisierte `For Each`-Muster.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-346">It is important to note that extension methods are considered to be members of the type in all contexts where members are bound, such as the strongly-typed `For Each` pattern.</span></span> <span data-ttu-id="b0f1b-347">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-347">For example:</span></span>

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

<span data-ttu-id="b0f1b-348">Delegaten können auch erstellt werden, die auf Erweiterungs Methoden verweisen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-348">Delegates can also be created that refer to extension methods.</span></span> <span data-ttu-id="b0f1b-349">Daher ist der Code:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-349">Thus, the code:</span></span>

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

<span data-ttu-id="b0f1b-350">ist ungefähr Äquivalent zu:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-350">is roughly equivalent to:</span></span>

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

<span data-ttu-id="b0f1b-351">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="b0f1b-351">__Note.__</span></span> <span data-ttu-id="b0f1b-352">In Visual Basic wird normalerweise eine Prüfung auf einen instanzmethodenaufruf eingefügt, der bewirkt, dass ein `System.NullReferenceException` auftritt, wenn die Instanz, für die die Methode aufgerufen wird, `Nothing` ist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-352">Visual Basic normally inserts a check on an instance method call that causes a `System.NullReferenceException` to occur if the instance the method is being invoked on is `Nothing`.</span></span> <span data-ttu-id="b0f1b-353">Im Fall von Erweiterungs Methoden gibt es keine effiziente Möglichkeit, diese Überprüfung einzufügen, sodass Erweiterungs Methoden explizit nach `Nothing` suchen müssen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-353">In the case of extension methods, there is no efficient way to insert this check, so extension methods will need to explicitly check for `Nothing`.</span></span> 

<span data-ttu-id="b0f1b-354">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="b0f1b-354">__Note.__</span></span> <span data-ttu-id="b0f1b-355">Ein Werttyp wird beim übergeben als `ByVal`-Argument an einen Parameter übergeben, der als Schnittstelle typisiert ist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-355">A value type will be boxed when being passed as a `ByVal` argument to a parameter typed as an interface.</span></span>  <span data-ttu-id="b0f1b-356">Dies impliziert, dass Nebeneffekte der Erweiterungsmethode auf eine Kopie der Struktur anstatt auf den ursprünglichen angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-356">This implies that side effects of the extension method will operate on a copy of the structure instead of the original.</span></span> <span data-ttu-id="b0f1b-357">Obwohl die Sprache keine Einschränkungen für das erste Argument einer Erweiterungsmethode enthält, wird empfohlen, dass Erweiterungs Methoden nicht zum Erweitern von Werttypen verwendet werden, oder dass beim Erweitern von Werttypen der erste Parameter `ByRef` übergeben wird, um die Nebeneffekte zu gewährleisten. Arbeiten Sie mit dem ursprünglichen Wert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-357">While the language puts no restrictions on the first argument of an extension method, it is recommended that extension methods are not used to extend value types or that when extending value types, the first parameter is passed `ByRef` to ensure that side effects operate on the original value.</span></span>

### <a name="partial-methods"></a><span data-ttu-id="b0f1b-358">Partielle Methoden</span><span class="sxs-lookup"><span data-stu-id="b0f1b-358">Partial Methods</span></span>

<span data-ttu-id="b0f1b-359">Eine *partielle Methode* ist eine Methode, die eine Signatur, aber nicht den Text der Methode angibt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-359">A *partial method* is a method that specifies a signature but not the body of the method.</span></span> <span data-ttu-id="b0f1b-360">Der Text der Methode kann durch eine andere Methoden Deklaration mit demselben Namen und derselben Signatur angegeben werden, höchstwahrscheinlich in einer anderen partiellen Deklaration des Typs.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-360">The body of the method can be supplied by another method declaration with the same name and signature, most likely in another partial declaration of the type.</span></span> <span data-ttu-id="b0f1b-361">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-361">For example:</span></span>

<span data-ttu-id="b0f1b-362">a. vb:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-362">a.vb:</span></span>

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

<span data-ttu-id="b0f1b-363">b. vb:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-363">b.vb:</span></span>

```vb
Public Partial Class MyForm
    Public Sub ValidateControls()
        ' Validation logic goes here
        ...
    End Sub
End Class
```

<span data-ttu-id="b0f1b-364">In diesem Beispiel deklariert eine partielle Deklaration der-Klasse `MyForm` eine partielle Methode `ValidateControls` ohne Implementierung.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-364">In this example, a partial declaration of the class `MyForm` declares a partial method `ValidateControls` with no implementation.</span></span> <span data-ttu-id="b0f1b-365">Der Konstruktor in der partiellen Deklaration Ruft die partielle Methode auf, auch wenn kein Text in der Datei angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-365">The constructor in the partial declaration calls the partial method, even though there is no body supplied in the file.</span></span> <span data-ttu-id="b0f1b-366">Die andere partielle Deklaration von `MyForm` stellt dann die Implementierung der-Methode bereit.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-366">The other partial declaration of `MyForm` then supplies the implementation of the method.</span></span>

<span data-ttu-id="b0f1b-367">Partielle Methoden können aufgerufen werden, unabhängig davon, ob ein Text bereitgestellt wurde. Wenn kein Methoden Text angegeben wird, wird der-Rückruf ignoriert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-367">Partial methods can be called regardless of whether a body has been supplied; if no method body is supplied, the call is ignored.</span></span> <span data-ttu-id="b0f1b-368">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-368">For example:</span></span>

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

<span data-ttu-id="b0f1b-369">Alle Ausdrücke, die als Argumente an einen nicht ignorierten partiellen Methodenaufrufe übermittelt werden, werden ebenfalls ignoriert und nicht ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-369">Any expressions that are passed in as arguments to a partial method call that is ignored are ignored also and not evaluated.</span></span> <span data-ttu-id="b0f1b-370">(__Hinweis:__</span><span class="sxs-lookup"><span data-stu-id="b0f1b-370">(__Note.__</span></span> <span data-ttu-id="b0f1b-371">Dies bedeutet, dass partielle Methoden eine sehr effiziente Methode zur Bereitstellung von Verhalten sind, das über zwei partielle Typen definiert ist, da die partiellen Methoden keine Kosten haben, wenn Sie nicht verwendet werden.)</span><span class="sxs-lookup"><span data-stu-id="b0f1b-371">This means that partial methods are a very efficient way of providing behavior that is defined across two partial types, since the partial methods have no cost if they are not used.)</span></span>

<span data-ttu-id="b0f1b-372">Die Deklaration der partiellen Methode muss als `Private` deklariert werden und muss immer eine Unterroutine sein, ohne dass Anweisungen im Textkörper angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-372">The partial method declaration must be declared as `Private` and must always be a subroutine with no statements in its body.</span></span> <span data-ttu-id="b0f1b-373">Partielle Methoden können nicht selbst Schnittstellen Methoden implementieren, obwohl die Methode, die Ihren Text bereitstellt, möglich ist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-373">Partial methods cannot themselves implement interface methods, although the method that supplies their body can.</span></span>

<span data-ttu-id="b0f1b-374">Nur eine Methode kann einen Text für eine partielle Methode bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-374">Only one method can supply a body to a partial method.</span></span> <span data-ttu-id="b0f1b-375">Eine Methode, die einen Text für eine partielle Methode bereitstellt, muss die gleiche Signatur wie die partielle Methode, dieselben Einschränkungen für alle Typparameter, dieselben deklarationmodifizierer und die gleichen Parameter-und Typparameter Namen aufweisen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-375">A method supplying a body to a partial method must have the same signature as the partial method, the same constraints on any type parameters, the same declaration modifiers, and the same parameter and type parameter names.</span></span> <span data-ttu-id="b0f1b-376">Attribute der partiellen Methode und die Methode, die Ihren Text bereitstellt, werden zusammengeführt, ebenso wie alle Attribute der Methoden Parameter.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-376">Attributes on the partial method and the method that supplies its body are merged, as are any attributes on the methods' parameters.</span></span> <span data-ttu-id="b0f1b-377">Ebenso wird die Liste der Ereignisse, die von den Methoden behandelt werden, zusammengeführt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-377">Similarly, the list of events that the methods handle is merged.</span></span> <span data-ttu-id="b0f1b-378">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-378">For example:</span></span>

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

## <a name="constructors"></a><span data-ttu-id="b0f1b-379">Konstruktoren</span><span class="sxs-lookup"><span data-stu-id="b0f1b-379">Constructors</span></span>

<span data-ttu-id="b0f1b-380">*Konstruktoren* sind spezielle Methoden, die die Kontrolle über die Initialisierung ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-380">*Constructors* are special methods that allow control over initialization.</span></span> <span data-ttu-id="b0f1b-381">Sie werden ausgeführt, nachdem das Programm begonnen oder eine Instanz eines Typs erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-381">They are run after the program begins or when an instance of a type is created.</span></span> <span data-ttu-id="b0f1b-382">Im Gegensatz zu anderen Membern werden Konstruktoren nicht geerbt, und es wird kein Name in den Deklarations Bereich eines Typs eingeführt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-382">Unlike other members, constructors are not inherited and do not introduce a name into a type's declaration space.</span></span> <span data-ttu-id="b0f1b-383">Konstruktoren können nur von Ausdrücken zum Erstellen von Objekten oder vom .NET Framework aufgerufen werden. Sie werden möglicherweise nie direkt aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-383">Constructors may only be invoked by object-creation expressions or by the .NET Framework; they may never be directly invoked.</span></span>

<span data-ttu-id="b0f1b-384">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="b0f1b-384">__Note.__</span></span> <span data-ttu-id="b0f1b-385">Konstruktoren haben bei der Zeilen Platzierung die gleiche Einschränkung wie bei den Unterroutinen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-385">Constructors have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="b0f1b-386">Die beginnende Anweisung, End-Anweisung und Block müssen alle am Anfang einer logischen Zeile angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-386">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

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

### <a name="instance-constructors"></a><span data-ttu-id="b0f1b-387">Instanzkonstruktoren</span><span class="sxs-lookup"><span data-stu-id="b0f1b-387">Instance Constructors</span></span>

<span data-ttu-id="b0f1b-388">*Instanzkonstruktoren* initialisieren Instanzen eines Typs und werden von der .NET Framework ausgeführt, wenn eine Instanz erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-388">*Instance constructors* initialize instances of a type and are run by the .NET Framework when an instance is created.</span></span> <span data-ttu-id="b0f1b-389">Die Parameterliste eines Konstruktors unterliegt den gleichen Regeln wie die Parameterliste einer Methode.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-389">The parameter list of a constructor is subject to the same rules as the parameter list of a method.</span></span> <span data-ttu-id="b0f1b-390">Instanzkonstruktoren können überladen werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-390">Instance constructors may be overloaded.</span></span>

<span data-ttu-id="b0f1b-391">Alle Konstruktoren in Verweis Typen müssen einen anderen Konstruktor aufrufen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-391">All constructors in reference types must invoke another constructor.</span></span> <span data-ttu-id="b0f1b-392">Wenn der Aufruf explizit ist, muss es sich um die erste Anweisung im konstruktormethodentext handeln.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-392">If the invocation is explicit, it must be the first statement in the constructor method body.</span></span> <span data-ttu-id="b0f1b-393">Die Anweisung kann entweder einen anderen der Instanzkonstruktoren des Typs aufrufen (z. b. "`Me.New(...)`" oder "`MyClass.New(...)`"), oder wenn es sich nicht um eine Struktur handelt, kann er einen Instanzkonstruktor des Basistyps des Typs aufrufen (z. b. `MyBase.New(...)`).</span><span class="sxs-lookup"><span data-stu-id="b0f1b-393">The statement can either invoke another of the type's instance constructors -- for example, `Me.New(...)` or `MyClass.New(...)` -- or if it is not a structure it can invoke an instance constructor of the type's base type -- for example, `MyBase.New(...)`.</span></span> <span data-ttu-id="b0f1b-394">Ein Konstruktor kann sich nicht selbst aufrufen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-394">It is invalid for a constructor to invoke itself.</span></span> <span data-ttu-id="b0f1b-395">Wenn ein Konstruktor einen anderen Konstruktor aufruft, ist `MyBase.New()` implizit.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-395">If a constructor omits a call to another constructor, `MyBase.New()` is implicit.</span></span> <span data-ttu-id="b0f1b-396">Wenn kein Parameter loser Basistyp Konstruktor vorhanden ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-396">If there is no parameterless base type constructor, a compile-time error occurs.</span></span> <span data-ttu-id="b0f1b-397">Da `Me` erst nach dem Aufruf eines Basisklassenkonstruktors als konstruiert betrachtet wird, können die Parameter für eine Konstruktoraufruf-Anweisung weder implizit noch explizit auf `Me`, `MyClass` oder `MyBase` verweisen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-397">Because `Me` is not considered to be constructed until after the call to a base class constructor, the parameters to a constructor invocation statement cannot reference `Me`, `MyClass`, or `MyBase` implicitly or explicitly.</span></span>

<span data-ttu-id="b0f1b-398">Wenn die erste Anweisung eines Konstruktors die Form `MyBase.New(...)` hat, führt der Konstruktor implizit die Initialisierungen aus, die von den Variableninitialisierern der Instanzvariablen angegeben werden, die im-Typ deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-398">When a constructor's first statement is of the form `MyBase.New(...)`, the constructor implicitly performs the initializations specified by the variable initializers of the instance variables declared in the type.</span></span> <span data-ttu-id="b0f1b-399">Dies entspricht einer Sequenz von Zuweisungen, die unmittelbar nach dem Aufrufen des Konstruktors des direkten Basistyps ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-399">This corresponds to a sequence of assignments that are executed immediately after invoking the direct base type constructor.</span></span> <span data-ttu-id="b0f1b-400">Diese Reihenfolge stellt sicher, dass alle basisinstanzvariablen durch ihre Variableninitialisierer initialisiert werden, bevor Anweisungen ausgeführt werden, die auf die Instanz zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-400">Such ordering ensures that all base instance variables are initialized by their variable initializers before any statements that have access to the instance are executed.</span></span> <span data-ttu-id="b0f1b-401">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-401">For example:</span></span>

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

<span data-ttu-id="b0f1b-402">Wenn `New B()` zum Erstellen einer Instanz von `B` verwendet wird, wird die folgende Ausgabe erzeugt:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-402">When `New B()` is used to create an instance of `B`, the following output is produced:</span></span>

```console
x = 1, y = 1
```

<span data-ttu-id="b0f1b-403">Der Wert von `y` ist `1`, da der Variableninitialisierer ausgeführt wird, nachdem der Basisklassenkonstruktor aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-403">The value of `y` is `1` because the variable initializer is executed after the base class constructor is invoked.</span></span> <span data-ttu-id="b0f1b-404">Variableninitialisierer werden in der Text Reihenfolge ausgeführt, in der Sie in der Typdeklaration angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-404">Variable initializers are executed in the textual order they appear in the type declaration.</span></span>

<span data-ttu-id="b0f1b-405">Wenn ein Typ nur `Private`-Konstruktoren deklariert, ist es im Allgemeinen nicht möglich, dass andere Typen vom Typ abgeleitet werden, oder dass Instanzen des Typs erstellt werden. die einzige Ausnahme sind Typen, die innerhalb des Typs geschachtelt sind.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-405">When a type declares only `Private` constructors, it is not possible in general for other types to derive from the type or create instances of the type; the only exception is types nested within the type.</span></span> <span data-ttu-id="b0f1b-406">`Private`-Konstruktoren werden häufig in Typen verwendet, die nur `Shared`-Member enthalten.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-406">`Private` constructors are commonly used in types that contain only `Shared` members.</span></span>

<span data-ttu-id="b0f1b-407">Wenn ein Typ keine Instanzkonstruktordeklarationen enthält, wird automatisch ein Standardkonstruktor bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-407">If a type contains no instance constructor declarations, a default constructor is automatically provided.</span></span> <span data-ttu-id="b0f1b-408">Der Standardkonstruktor ruft einfach den Parameter losen Konstruktor des direkten Basistyps auf.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-408">The default constructor simply invokes the parameterless constructor of the direct base type.</span></span> <span data-ttu-id="b0f1b-409">Wenn der direkte Basistyp keinen zugänglichen Parameter losen Konstruktor hat, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-409">If the direct base type does not have an accessible parameterless constructor, a compile-time error occurs.</span></span> <span data-ttu-id="b0f1b-410">Der deklarierte Zugriffstyp für den Standardkonstruktor ist `Public`, es sei denn, der Typ ist `MustInherit`. in diesem Fall ist der Standardkonstruktor `Protected`.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-410">The declared access type for the default constructor is `Public` unless the type is `MustInherit`, in which case the default constructor is `Protected`.</span></span>

<span data-ttu-id="b0f1b-411">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="b0f1b-411">__Note.__</span></span> <span data-ttu-id="b0f1b-412">Der Standard Zugriff für den Standardkonstruktor eines `MustInherit`-Typs ist `Protected`, da `MustInherit`-Klassen nicht direkt erstellt werden können.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-412">The default access for a `MustInherit` type's default constructor is `Protected` because `MustInherit` classes cannot be created directly.</span></span> <span data-ttu-id="b0f1b-413">Es gibt also keinen Punkt, an dem der Standardkonstruktor `Public` ist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-413">So there is no point in making the default constructor `Public`.</span></span>

<span data-ttu-id="b0f1b-414">Im folgenden Beispiel wird ein Standardkonstruktor bereitgestellt, da die-Klasse keine Konstruktordeklarationen enthält:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-414">In the following example a default constructor is provided because the class contains no constructor declarations:</span></span>

```vb
Class Message
    Dim sender As Object
    Dim text As String
End Class
```

<span data-ttu-id="b0f1b-415">Daher entspricht das Beispiel genau folgendem:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-415">Thus, the example is precisely equivalent to the following:</span></span>

```vb
Class Message
    Dim sender As Object
    Dim text As String

    Sub New()
    End Sub
End Class
```

<span data-ttu-id="b0f1b-416">Standardkonstruktoren, die in eine vom Designer generierte Klasse ausgegeben werden, die mit dem-Attribut `Microsoft.VisualBasic.CompilerServices.DesignerGeneratedAttribute` gekennzeichnet ist, werden die-Methode `Sub InitializeComponent()`, sofern vorhanden, nach dem-Aufrufer des basiskonstruktors aufruft.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-416">Default constructors that are emitted into a designer generated class marked with the attribute `Microsoft.VisualBasic.CompilerServices.DesignerGeneratedAttribute` will call the method `Sub InitializeComponent()`, if it exists, after the call to the base constructor.</span></span> <span data-ttu-id="b0f1b-417">(__Hinweis:__</span><span class="sxs-lookup"><span data-stu-id="b0f1b-417">(__Note.__</span></span> <span data-ttu-id="b0f1b-418">Dadurch können vom Designer generierte Dateien, z. b. die vom WinForms-Designer erstellten Dateien, den Konstruktor in der Designer Datei weglassen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-418">This allows designer generated files, such as those created by the WinForms designer, to omit the constructor in the designer file.</span></span> <span data-ttu-id="b0f1b-419">Dies ermöglicht es dem Programmierer, ihn selbst anzugeben, wenn dies der Fall ist.)</span><span class="sxs-lookup"><span data-stu-id="b0f1b-419">This enables the programmer to specify it themselves, if they so choose.)</span></span>

### <a name="shared-constructors"></a><span data-ttu-id="b0f1b-420">Freigegebene Konstruktoren</span><span class="sxs-lookup"><span data-stu-id="b0f1b-420">Shared Constructors</span></span>

<span data-ttu-id="b0f1b-421">Frei *gegebene Konstruktoren* initialisieren die freigegebenen Variablen eines Typs. Sie werden ausgeführt, nachdem die Ausführung des Programms begonnen hat, jedoch vor allen Verweisen auf einen Member des Typs.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-421">*Shared constructors* initialize a type's shared variables; they are run after the program begins executing, but before any references to a member of the type.</span></span> <span data-ttu-id="b0f1b-422">Ein frei gegebener Konstruktor gibt den `Shared`-Modifizierer an, es sei denn, er befindet sich in einem Standardmodul. in diesem Fall wird der Modifizierer `Shared` impliziert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-422">A shared constructor specifies the `Shared` modifier, unless it is in a standard module in which case the `Shared` modifier is implied.</span></span>

<span data-ttu-id="b0f1b-423">Anders als Instanzkonstruktoren haben gemeinsam genutzte Konstruktoren impliziten öffentlichen Zugriff, haben keine Parameter und können keine anderen Konstruktoren aufrufen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-423">Unlike instance constructors, shared constructors have implicit public access, have no parameters, and may not call other constructors.</span></span> <span data-ttu-id="b0f1b-424">Vor der ersten Anweisung in einem freigegebenen Konstruktor führt der freigegebene Konstruktor implizit die Initialisierungen aus, die von den Variableninitialisierern der im-Typ deklarierten freigegebenen Variablen angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-424">Before the first statement in a shared constructor, the shared constructor implicitly performs the initializations specified by the variable initializers of the shared variables declared in the type.</span></span> <span data-ttu-id="b0f1b-425">Dies entspricht einer Sequenz von Zuweisungen, die unmittelbar nach dem Eintrag in den Konstruktor ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-425">This corresponds to a sequence of assignments that are executed immediately upon entry to the constructor.</span></span> <span data-ttu-id="b0f1b-426">Die Variableninitialisierer werden in der Text Reihenfolge ausgeführt, in der Sie in der Typdeklaration angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-426">The variable initializers are executed in the textual order they appear in the type declaration.</span></span>

<span data-ttu-id="b0f1b-427">Das folgende Beispiel zeigt eine `Employee`-Klasse mit einem freigegebenen Konstruktor, der eine freigegebene Variable initialisiert:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-427">The following example shows an `Employee` class with a shared constructor that initializes a shared variable:</span></span>

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

<span data-ttu-id="b0f1b-428">Ein separater frei gegebener Konstruktor ist für jeden geschlossenen generischen Typ vorhanden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-428">A separate shared constructor exists for each closed generic type.</span></span> <span data-ttu-id="b0f1b-429">Da der freigegebene Konstruktor für jeden geschlossenen Typ genau einmal ausgeführt wird, können Sie Laufzeitüberprüfungen für den Typparameter erzwingen, der zur Kompilierzeit nicht über Einschränkungen überprüft werden kann.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-429">Because the shared constructor is executed exactly once for each closed type, it is a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile-time via constraints.</span></span> <span data-ttu-id="b0f1b-430">Der folgende Typ verwendet beispielsweise einen freigegebenen Konstruktor, um zu erzwingen, dass der Typparameter `Integer` oder `Double` ist:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-430">For example, the following type uses a shared constructor to enforce that the type parameter is `Integer` or `Double`:</span></span>

```vb
Class EnumHolder(Of T)
    Shared Sub New() 
        If Not GetType(T).IsEnum() Then
            Throw New ArgumentException("T must be an enumerated type.")
        End If
    End Sub
End Class
```

<span data-ttu-id="b0f1b-431">Genau, wenn freigegebene Konstruktoren ausgeführt werden, ist dies größtenteils implementierungsabhängig, obwohl mehrere Garantien bereitgestellt werden, wenn ein gemeinsam genutzter Konstruktor explizit definiert ist:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-431">Exactly when shared constructors are run is mostly implementation dependent, though several guarantees are provided if a shared constructor is explicitly defined:</span></span>

* <span data-ttu-id="b0f1b-432">Freigegebene Konstruktoren werden vor dem ersten Zugriff auf ein statisches Feld des Typs ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-432">Shared constructors are run before the first access to any static field of the type.</span></span>

* <span data-ttu-id="b0f1b-433">Freigegebene Konstruktoren werden vor dem ersten Aufruf einer statischen Methode des Typs ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-433">Shared constructors are run before the first invocation of any static method of the type.</span></span>

* <span data-ttu-id="b0f1b-434">Freigegebene Konstruktoren werden vor dem ersten Aufruf eines Konstruktors für den Typ ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-434">Shared constructors are run before the first invocation of any constructor for the type.</span></span>

<span data-ttu-id="b0f1b-435">Die oben aufgeführten Garantien gelten nicht für die Situation, in der ein gemeinsam genutzter Konstruktor implizit für freigegebene Initialisierer erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-435">The above guarantees do not apply in the situation where a shared constructor is implicitly created for shared initializers.</span></span> <span data-ttu-id="b0f1b-436">Die Ausgabe des folgenden Beispiels ist unsicher, da die genaue Reihenfolge des Ladens und somit der freigegebenen Konstruktorausführung nicht definiert ist:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-436">The output from the following example is uncertain, because the exact ordering of loading and therefore of shared constructor execution is not defined:</span></span>

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

<span data-ttu-id="b0f1b-437">Die Ausgabe kann eine der folgenden sein:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-437">The output could be either of the following:</span></span>

```console
Init A
A.F
Init B
B.F
```

<span data-ttu-id="b0f1b-438">oder</span><span class="sxs-lookup"><span data-stu-id="b0f1b-438">or</span></span>

```console
Init B
Init A
A.F
B.F
```

<span data-ttu-id="b0f1b-439">Im Gegensatz dazu wird im folgenden Beispiel eine vorhersagbare Ausgabe erzeugt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-439">By contrast, the following example produces predictable output.</span></span> <span data-ttu-id="b0f1b-440">Beachten Sie, dass der `Shared`-Konstruktor für die Klasse `A` nie ausgeführt wird, auch wenn die Klasse `B` davon abgeleitet ist:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-440">Note that the `Shared` constructor for the class `A` never executes, even though class `B` derives from it:</span></span>

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

<span data-ttu-id="b0f1b-441">Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-441">The output is:</span></span>

```console
Init B
B.G
```

<span data-ttu-id="b0f1b-442">Es ist auch möglich, zirkuläre Abhängigkeiten zu erstellen, die es ermöglichen, dass `Shared`-Variablen mit Variableninitialisierern im Standardwert Zustand beobachtet werden, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-442">It is also possible to construct circular dependencies that allow `Shared` variables with variable initializers to be observed in their default value state, as in the following example:</span></span>

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

<span data-ttu-id="b0f1b-443">Dies erzeugt die Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-443">This produces the output:</span></span>

```console
X = 1, Y = 2
```

<span data-ttu-id="b0f1b-444">Um die `Main`-Methode auszuführen, lädt das System zuerst die Klasse `B`.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-444">To execute the `Main` method, the system first loads class `B`.</span></span> <span data-ttu-id="b0f1b-445">Der `Shared`-Konstruktor der Klasse `B` berechnet den Anfangswert von `Y`, der rekursiv bewirkt, dass die Klasse `A` geladen wird, da auf den Wert von `A.X` verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-445">The `Shared` constructor of class `B` proceeds to compute the initial value of `Y`, which recursively causes class `A` to be loaded because the value of `A.X` is referenced.</span></span> <span data-ttu-id="b0f1b-446">Der `Shared`-Konstruktor der Klasse `A` bewirkt, dass der Anfangswert von `X` berechnet wird. Dadurch wird der *Standard* Wert `Y` abgerufen, der 0 (null) ist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-446">The `Shared` constructor of class `A` in turn proceeds to compute the initial value of `X`, and in doing so fetches the *default* value of `Y`, which is zero.</span></span> <span data-ttu-id="b0f1b-447">`A.X` wird daher mit `1` initialisiert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-447">`A.X` is thus initialized to `1`.</span></span> <span data-ttu-id="b0f1b-448">Der Ladevorgang von `A` ist abgeschlossen, wobei die Berechnung des Anfangs Werts von `Y` zurückgegeben wird, dessen Ergebnis `2` ist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-448">The process of loading `A` then completes, returning to the calculation of the initial value of `Y`, the result of which becomes `2`.</span></span>

<span data-ttu-id="b0f1b-449">Hätte sich die `Main`-Methode stattdessen in der Klasse `A` befunden, hätte das Beispiel die folgende Ausgabe erzeugt:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-449">Had the `Main` method instead been located in class `A`, the example would have produced the following output:</span></span>

```console
X = 2, Y = 1
```

<span data-ttu-id="b0f1b-450">Vermeiden Sie zirkuläre Verweise in `Shared`-Variableninitialisierern, da es im Allgemeinen nicht möglich ist, die Reihenfolge zu bestimmen, in der Klassen mit solchen Verweisen geladen werden</span><span class="sxs-lookup"><span data-stu-id="b0f1b-450">Avoid circular references in `Shared` variable initializers since it is generally impossible to determine the order in which classes containing such references are loaded.</span></span>

## <a name="events"></a><span data-ttu-id="b0f1b-451">Ereignisse</span><span class="sxs-lookup"><span data-stu-id="b0f1b-451">Events</span></span>

<span data-ttu-id="b0f1b-452">Ereignisse werden verwendet, um Code über ein bestimmtes Vorkommen zu benachrichtigen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-452">Events are used to notify code of a particular occurrence.</span></span> <span data-ttu-id="b0f1b-453">Eine Ereignis Deklaration besteht aus einem Bezeichner, entweder einem Delegattyp oder einer Parameterliste, und einer optionalen `Implements`-Klausel.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-453">An event declaration consists of an identifier, either a delegate type or a parameter list, and an optional `Implements` clause.</span></span>

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

<span data-ttu-id="b0f1b-454">Wenn ein Delegattyp angegeben wird, weist der Delegattyp möglicherweise keinen Rückgabetyp auf.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-454">If a delegate type is specified, the delegate type may not have a return type.</span></span> <span data-ttu-id="b0f1b-455">Wenn eine Parameterliste angegeben wird, enthält Sie möglicherweise keine `Optional`-oder `ParamArray`-Parameter.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-455">If a parameter list is specified, it may not contain `Optional` or `ParamArray` parameters.</span></span> <span data-ttu-id="b0f1b-456">Die Zugriffs Domäne der Parametertypen und/oder des Delegattyps muss mit der Zugriffs Domäne des Ereignisses identisch sein oder eine übergeordnete Gruppe sein.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-456">The accessibility domain of the parameter types and/or delegate type must be the same as, or a superset of, the accessibility domain of the event itself.</span></span> <span data-ttu-id="b0f1b-457">Ereignisse können durch Angabe des `Shared`-Modifizierers freigegeben werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-457">Events may be shared by specifying the `Shared` modifier.</span></span>

<span data-ttu-id="b0f1b-458">Zusätzlich zu dem Elementnamen, der dem Deklarations Bereich des Typs hinzugefügt wird, werden von einer Ereignis Deklaration implizit mehrere andere Member deklariert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-458">In addition to the member name added to the type's declaration space, an event declaration implicitly declares several other members.</span></span> <span data-ttu-id="b0f1b-459">Wenn ein Ereignis mit dem Namen "`X`" angegeben wird, werden dem Deklarations Raum die folgenden Elemente hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-459">Given an event named `X`, the following members are added to the declaration space:</span></span>

* <span data-ttu-id="b0f1b-460">Wenn das Formular der Deklaration eine Methoden Deklaration ist, wird eine eingefügte Delegatklasse mit dem Namen "`XEventHandler`" eingeführt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-460">If the form of the declaration is a method declaration, a nested delegate class named `XEventHandler` is introduced.</span></span> <span data-ttu-id="b0f1b-461">Die Klassen für den eingefügten Delegaten stimmen mit der Methoden Deklaration überein und haben denselben Zugriff wie das Ereignis.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-461">The nested delegate class matches the method declaration and has the same accessibility as the event.</span></span> <span data-ttu-id="b0f1b-462">Die Attribute in der Parameterliste gelten für die Parameter der Delegatklasse.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-462">The attributes in the parameter list apply to the parameters of the delegate class.</span></span>

* <span data-ttu-id="b0f1b-463">Eine `Private`-Instanzvariable, die als Delegat mit dem Namen `XEvent` typisiert ist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-463">A `Private` instance variable typed as the delegate, named `XEvent`.</span></span>

* <span data-ttu-id="b0f1b-464">Zwei Methoden namens "`add_X`" und "`remove_X`", die nicht aufgerufen, überschrieben oder überladen werden können.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-464">Two methods named `add_X` and `remove_X` which cannot be invoked, overridden or overloaded.</span></span>

<span data-ttu-id="b0f1b-465">Wenn ein Typ versucht, einen Namen zu deklarieren, der mit einem der obigen Namen übereinstimmt, führt dies zu einem Kompilierzeitfehler, und die impliziten `add_X`-und `remove_X`-Deklarationen werden für den Zweck der namens Bindung ignoriert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-465">If a type attempts to declare a name that matches one of the above names, a compile-time error will result, and the implicit `add_X` and `remove_X` declarations are ignored for the purposes of name binding.</span></span> <span data-ttu-id="b0f1b-466">Es ist nicht möglich, die eingeführten Member außer Kraft zu setzen oder zu überladen, obwohl es möglich ist, Sie in abgeleiteten Typen zu schattieren.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-466">It is not possible to override or overload any of the introduced members, although it is possible to shadow them in derived types.</span></span> <span data-ttu-id="b0f1b-467">Beispielsweise die Klassen Deklaration</span><span class="sxs-lookup"><span data-stu-id="b0f1b-467">For example, the class declaration</span></span>

```vb
Class Raiser
    Public Event Constructed(i As Integer)
End Class
```

<span data-ttu-id="b0f1b-468">entspricht der folgenden Deklaration</span><span class="sxs-lookup"><span data-stu-id="b0f1b-468">is equivalent to the following declaration</span></span>

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

<span data-ttu-id="b0f1b-469">Das Deklarieren eines Ereignisses ohne Angabe eines Delegattyps ist die einfachste und kompakteste Syntax, hat aber den Nachteil, dass für jedes Ereignis ein neuer Delegattyp deklariert wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-469">Declaring an event without specifying a delegate type is the simplest and most compact syntax, but has the disadvantage of declaring a new delegate type for each event.</span></span> <span data-ttu-id="b0f1b-470">Im folgenden Beispiel werden z. b. drei verborgene Delegattypen erstellt, auch wenn alle drei Ereignisse dieselbe Parameterliste aufweisen:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-470">For example, in the following example, three hidden delegate types are created, even though all three events have the same parameter list:</span></span>

```vb
Public Class Button
    Public Event Click(sender As Object, e As EventArgs)
    Public Event DoubleClick(sender As Object, e As EventArgs)
    Public Event RightClick(sender As Object, e As EventArgs)
End Class
```

<span data-ttu-id="b0f1b-471">Im folgenden Beispiel verwenden die-Ereignisse einfach denselben Delegaten, `EventHandler`:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-471">In the following example, the events simply use the same delegate, `EventHandler`:</span></span>

```vb
Public Delegate Sub EventHandler(sender As Object, e As EventArgs)

Public Class Button
    Public Event Click As EventHandler
    Public Event DoubleClick As EventHandler
    Public Event RightClick As EventHandler
End Class
```

<span data-ttu-id="b0f1b-472">Ereignisse können auf eine von zwei Arten behandelt werden: statisch oder dynamisch.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-472">Events can be handled in one of two ways: statically or dynamically.</span></span> <span data-ttu-id="b0f1b-473">Statisch Behandlungsereignisse sind einfacher und benötigen nur eine `WithEvents`-Variable und eine `Handles`-Klausel.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-473">Statically handling events is simpler and only requires a `WithEvents` variable and a `Handles` clause.</span></span> <span data-ttu-id="b0f1b-474">Im folgenden Beispiel behandelt Class `Form1` das-Ereignis statisch `Click` des-Objekts `Button`:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-474">In the following example, class `Form1` statically handles the event `Click` of object `Button`:</span></span>

```vb
Public Class Form1
    Public WithEvents Button1 As New Button()

    Public Sub Button1_Click(sender As Object, e As EventArgs) _
           Handles Button1.Click
        Console.WriteLine("Button1 was clicked!")
    End Sub
End Class
```

<span data-ttu-id="b0f1b-475">Dynamisch Behandlungsereignisse sind komplexer, da das Ereignis explizit verbunden werden muss und in Code getrennt werden muss.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-475">Dynamically handling events is more complex because the event must be explicitly connected and disconnected to in code.</span></span> <span data-ttu-id="b0f1b-476">Die-Anweisung `AddHandler` fügt einen Handler für ein Ereignis hinzu, und die-Anweisung `RemoveHandler` entfernt einen Handler für ein Ereignis.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-476">The statement `AddHandler` adds a handler for an event, and the statement `RemoveHandler` removes a handler for an event.</span></span> <span data-ttu-id="b0f1b-477">Das nächste Beispiel zeigt eine Klasse `Form1`, die `Button1_Click` als Ereignishandler für das `Click`-Ereignis von `Button1` hinzufügt:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-477">The next example shows a class `Form1` that adds `Button1_Click` as an event handler for `Button1`'s `Click` event:</span></span>

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

<span data-ttu-id="b0f1b-478">In der-Methode `Disconnect` wird der Ereignishandler entfernt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-478">In method `Disconnect`, the event handler is removed.</span></span>


### <a name="custom-events"></a><span data-ttu-id="b0f1b-479">Benutzerdefinierte Ereignisse</span><span class="sxs-lookup"><span data-stu-id="b0f1b-479">Custom Events</span></span>

<span data-ttu-id="b0f1b-480">Wie bereits im vorherigen Abschnitt erläutert, definieren Ereignis Deklarationen implizit ein Feld, eine `add_`-Methode und eine `remove_`-Methode, die zum Nachverfolgen von Ereignis Handlern verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-480">As discussed in the previous section, event declarations implicitly define a field, an `add_` method, and a `remove_` method that are used to keep track of event handlers.</span></span> <span data-ttu-id="b0f1b-481">In einigen Situationen kann es jedoch wünschenswert sein, benutzerdefinierten Code zum Nachverfolgen von Ereignis Handlern bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-481">In some situations, however, it may be desirable to provide custom code for tracking event handlers.</span></span> <span data-ttu-id="b0f1b-482">Wenn eine Klasse z. b. 40-Ereignisse definiert, von denen nur wenige behandelt werden, kann die Verwendung einer Hash Tabelle anstelle von 40 Feldern, um die Handler für jedes Ereignis zu verfolgen, effizienter sein.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-482">For example, if a class defines forty events of which only a few will ever be handled, using a hash table instead of forty fields to track the handlers for each event may be more efficient.</span></span> <span data-ttu-id="b0f1b-483">*Benutzerdefinierte Ereignisse* ermöglichen das explizite Definieren der `add_X`-Methode und der `remove_X`-Methode, wodurch benutzerdefinierter Speicher für Ereignishandler aktiviert wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-483">*Custom events* allow the `add_X` and `remove_X` methods to be defined explicitly, which enables custom storage for event handlers.</span></span>

<span data-ttu-id="b0f1b-484">Benutzerdefinierte Ereignisse werden auf die gleiche Weise deklariert, wie Ereignisse, die einen Delegattyp angeben, deklariert werden, mit der Ausnahme, dass das Schlüsselwort `Custom` dem Schlüsselwort `Event` vorangestellt sein muss.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-484">Custom events are declared in the same way that events that specify a delegate type are declared, with the exception that the keyword `Custom` must precede the `Event` keyword.</span></span> <span data-ttu-id="b0f1b-485">Eine benutzerdefinierte Ereignis Deklaration enthält drei Deklarationen: eine `AddHandler`-Deklaration, eine `RemoveHandler`-Deklaration und eine `RaiseEvent`-Deklaration.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-485">A custom event declaration contains three declarations: an `AddHandler` declaration, a `RemoveHandler` declaration and a `RaiseEvent` declaration.</span></span> <span data-ttu-id="b0f1b-486">Keine der Deklarationen kann einen Modifizierer aufweisen, obwohl Sie über Attribute verfügen können.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-486">None of the declarations can have any modifiers, although they can have attributes.</span></span>

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

<span data-ttu-id="b0f1b-487">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-487">For example:</span></span>

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

<span data-ttu-id="b0f1b-488">Die `AddHandler`-und `RemoveHandler`-Deklaration nehmen einen `ByVal`-Parameter an, der den Delegattyp des Ereignisses aufweisen muss.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-488">The `AddHandler` and `RemoveHandler` declaration take one `ByVal` parameter, which must be of the delegate type of the event.</span></span> <span data-ttu-id="b0f1b-489">Wenn eine `AddHandler`-oder `RemoveHandler`-Anweisung ausgeführt wird (oder eine `Handles`-Klausel automatisch ein Ereignis behandelt), wird die entsprechende Deklaration aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-489">When an `AddHandler` or `RemoveHandler` statement is executed (or a `Handles` clause automatically handles an event), the corresponding declaration will be called.</span></span> <span data-ttu-id="b0f1b-490">Die `RaiseEvent`-Deklaration übernimmt dieselben Parameter wie der Ereignis Delegat und wird aufgerufen, wenn eine `RaiseEvent`-Anweisung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-490">The `RaiseEvent` declaration takes the same parameters as the event delegate and will be called when a `RaiseEvent` statement is executed.</span></span> <span data-ttu-id="b0f1b-491">Alle Deklarationen müssen bereitgestellt werden und als Unterroutinen angesehen werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-491">All of the declarations must be provided and are considered to be subroutines.</span></span>

<span data-ttu-id="b0f1b-492">Beachten Sie, dass die Deklarationen "`AddHandler`", "`RemoveHandler`" und "`RaiseEvent`" die gleiche Einschränkung in der Zeilen Platzierung aufweisen, die unter</span><span class="sxs-lookup"><span data-stu-id="b0f1b-492">Note that `AddHandler`, `RemoveHandler` and `RaiseEvent` declarations have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="b0f1b-493">Die beginnende Anweisung, End-Anweisung und Block müssen alle am Anfang einer logischen Zeile angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-493">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

<span data-ttu-id="b0f1b-494">Zusätzlich zu dem Elementnamen, der dem Deklarations Bereich des Typs hinzugefügt wird, deklariert eine benutzerdefinierte Ereignis Deklaration implizit mehrere andere Member.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-494">In addition to the member name added to the type's declaration space, a custom event declaration implicitly declares several other members.</span></span> <span data-ttu-id="b0f1b-495">Wenn ein Ereignis mit dem Namen "`X`" angegeben wird, werden dem Deklarations Raum die folgenden Elemente hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-495">Given an event named `X`, the following members are added to the declaration space:</span></span>

* <span data-ttu-id="b0f1b-496">Eine Methode mit dem Namen "`add_X`", die der `AddHandler`-Deklaration entspricht.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-496">A method named `add_X`, corresponding to the `AddHandler` declaration.</span></span>

* <span data-ttu-id="b0f1b-497">Eine Methode mit dem Namen "`remove_X`", die der `RemoveHandler`-Deklaration entspricht.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-497">A method named `remove_X`, corresponding to the `RemoveHandler` declaration.</span></span>

* <span data-ttu-id="b0f1b-498">Eine Methode mit dem Namen "`fire_X`", die der `RaiseEvent`-Deklaration entspricht.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-498">A method named `fire_X`, corresponding to the `RaiseEvent` declaration.</span></span>

<span data-ttu-id="b0f1b-499">Wenn ein Typ versucht, einen Namen zu deklarieren, der mit einem der obigen Namen übereinstimmt, führt dies zu einem Kompilierzeitfehler, und die impliziten Deklarationen werden für die namens Bindung ignoriert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-499">If a type attempts to declare a name that matches one of the above names, a compile-time error will result, and the implicit declarations are all ignored for the purposes of name binding.</span></span> <span data-ttu-id="b0f1b-500">Es ist nicht möglich, die eingeführten Member außer Kraft zu setzen oder zu überladen, obwohl es möglich ist, Sie in abgeleiteten Typen zu schattieren.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-500">It is not possible to override or overload any of the introduced members, although it is possible to shadow them in derived types.</span></span>

<span data-ttu-id="b0f1b-501">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="b0f1b-501">__Note.__</span></span> <span data-ttu-id="b0f1b-502">`Custom` ist kein reserviertes Wort.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-502">`Custom` is not a reserved word.</span></span>

#### <a name="custom-events-in-winrt-assemblies"></a><span data-ttu-id="b0f1b-503">Benutzerdefinierte Ereignisse in WinRT-Assemblys</span><span class="sxs-lookup"><span data-stu-id="b0f1b-503">Custom events in WinRT assemblies</span></span>

<span data-ttu-id="b0f1b-504">Ab Microsoft Visual Basic 11,0 werden Ereignisse, die in einer Datei deklariert sind, die mit `/target:winmdobj` kompiliert wurde, oder in einer Schnittstelle in einer solchen Datei deklariert und dann an anderer Stelle implementiert, etwas anders behandelt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-504">As of Microsoft Visual Basic 11.0, events declared in a file compiled with `/target:winmdobj`, or declared in an interface in such a file and then implemented elsewhere, are treated a little differently.</span></span>

* <span data-ttu-id="b0f1b-505">Externe Tools, die zum Erstellen von winmd verwendet werden, gestatten in der Regel nur bestimmte Delegattypen, z. b. `System.EventHandler(Of T)` oder `System.TypedEventHandle(Of T, U)`, und andere werden nicht zugelassen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-505">External tools used to build the winmd will typically allow only certain delegate types such as `System.EventHandler(Of T)` or `System.TypedEventHandle(Of T, U)`, and will disallow others.</span></span>

* <span data-ttu-id="b0f1b-506">Das Feld "`XEvent`" weist den Typ `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationTokenTable(Of T)` auf, wobei "`T`" der Delegattyp ist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-506">The `XEvent` field has type `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationTokenTable(Of T)` where `T` is the delegate type.</span></span>

* <span data-ttu-id="b0f1b-507">Der AddHandler-Accessor gibt einen `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationToken` zurück, und der RemoveHandler-Accessor nimmt einen einzelnen Parameter desselben Typs an.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-507">The AddHandler accessor returns a `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationToken`, and the RemoveHandler accessor takes a single parameter of the same type.</span></span>

<span data-ttu-id="b0f1b-508">Im folgenden finden Sie ein Beispiel für ein solches benutzerdefiniertes Ereignis.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-508">Here is an example of such a custom event.</span></span>

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


## <a name="constants"></a><span data-ttu-id="b0f1b-509">Konstanten</span><span class="sxs-lookup"><span data-stu-id="b0f1b-509">Constants</span></span>

<span data-ttu-id="b0f1b-510">Eine *Konstante* ist ein konstanter Wert, der ein Member eines Typs ist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-510">A *constant* is a constant value that is a member of a type.</span></span>

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

<span data-ttu-id="b0f1b-511">Konstanten werden implizit freigegeben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-511">Constants are implicitly shared.</span></span> <span data-ttu-id="b0f1b-512">Wenn die Deklaration eine `As`-Klausel enthält, gibt die-Klausel den Typ des Members an, der von der Deklaration eingeführt wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-512">If the declaration contains an `As` clause, the clause specifies the type of the member introduced by the declaration.</span></span> <span data-ttu-id="b0f1b-513">Wenn der Typ weggelassen wird, wird der Typ der Konstante abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-513">If the type is omitted then the type of the constant is inferred.</span></span> <span data-ttu-id="b0f1b-514">Der Typ einer Konstante darf nur ein primitiver Typ oder `Object` sein.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-514">The type of a constant may only be a primitive type or `Object`.</span></span> <span data-ttu-id="b0f1b-515">Wenn eine Konstante als `Object` eingegeben wird und kein Typzeichen vorhanden ist, ist der tatsächliche Typ der Konstante der Typ des konstanten Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-515">If a constant is typed as `Object` and there is no type character, the real type of the constant will be the type of the constant expression.</span></span> <span data-ttu-id="b0f1b-516">Andernfalls ist der Typ der Konstante der Typ des Typzeichens der Konstante.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-516">Otherwise, the type of the constant is the type of the constant's type character.</span></span>

<span data-ttu-id="b0f1b-517">Das folgende Beispiel zeigt eine Klasse mit dem Namen `Constants` mit zwei öffentlichen Konstanten:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-517">The following example shows a class named `Constants` that has two public constants:</span></span>

```vb
Class Constants
    Public Const A As Integer = 1
    Public Const B As Integer = A + 1
End Class
```

<span data-ttu-id="b0f1b-518">Auf Konstanten kann über die-Klasse zugegriffen werden, wie im folgenden Beispiel gezeigt, das die Werte `Constants.A` und `Constants.B` ausgibt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-518">Constants can be accessed through the class, as in the following example, which prints out the values of `Constants.A` and `Constants.B`.</span></span>

```vb
Module Test
    Sub Main()
        Console.WriteLine(Constants.A & ", " & Constants.B)
    End Sub 
End Module
```

<span data-ttu-id="b0f1b-519">Eine Konstante Deklaration, die mehrere Konstanten deklariert, entspricht mehreren Deklarationen von einzelnen Konstanten.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-519">A constant declaration that declares multiple constants is equivalent to multiple declarations of single constants.</span></span> <span data-ttu-id="b0f1b-520">Im folgenden Beispiel werden drei Konstanten in einer Deklarations Anweisung deklariert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-520">The following example declares three constants in one declaration statement.</span></span>

```vb
Class A
    Protected Const x As Integer = 1, y As Long = 2, z As Short = 3
End Class
```

<span data-ttu-id="b0f1b-521">Diese Deklaration entspricht Folgendem:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-521">This declaration is equivalent to the following:</span></span>

```vb
Class A
    Protected Const x As Integer = 1
    Protected Const y As Long = 2
    Protected Const z As Short = 3
End Class
```

<span data-ttu-id="b0f1b-522">Die Zugriffs Domäne des Typs der Konstante muss mit oder einer übergeordneten Zugriffs Domäne der Konstanten identisch sein.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-522">The accessibility domain of the type of the constant must be the same as or a superset of the accessibility domain of the constant itself.</span></span> <span data-ttu-id="b0f1b-523">Der Konstante Ausdruck muss einen Wert des Konstanten Typs oder eines Typs liefern, der implizit in den Typ der Konstante konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-523">The constant expression must yield a value of the constant's type or of a type that is implicitly convertible to the constant's type.</span></span> <span data-ttu-id="b0f1b-524">Der Konstante Ausdruck darf nicht zirkulär sein. Dies bedeutet, dass eine Konstante nicht in Bezug auf sich selbst definiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-524">The constant expression may not be circular; that is, a constant may not be defined in terms of itself.</span></span>

<span data-ttu-id="b0f1b-525">Der Compiler wertet die Konstanten Deklarationen automatisch in der entsprechenden Reihenfolge aus.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-525">The compiler automatically evaluates the constant declarations in the appropriate order.</span></span> <span data-ttu-id="b0f1b-526">Im folgenden Beispiel wertet der Compiler zuerst `Y` aus, dann `Z` und schließlich `X`, wobei die Werte 10, 11 bzw. 12 erzeugt werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-526">In the following example, the compiler first evaluates `Y`, then `Z`, and finally `X`, producing the values 10, 11, and 12, respectively.</span></span>

```vb
Class A
    Public Const X As Integer = B.Z + 1
    Public Const Y As Integer = 10
End Class

Class B
    Public Const Z As Integer = A.Y + 1
End Class
```

<span data-ttu-id="b0f1b-527">Wenn ein symbolischer Name für einen konstanten Wert gewünscht ist, aber der Typ des Werts in einer Konstanten Deklaration nicht zulässig ist oder wenn der Wert zur Kompilierzeit nicht durch einen konstanten Ausdruck berechnet werden kann, kann stattdessen eine schreibgeschützte Variable verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-527">When a symbolic name for a constant value is desired, but the type of the value is not permitted in a constant declaration or when the value cannot be computed at compile time by a constant expression, a read-only variable may be used instead.</span></span>


## <a name="instance-and-shared-variables"></a><span data-ttu-id="b0f1b-528">Instanz und freigegebene Variablen</span><span class="sxs-lookup"><span data-stu-id="b0f1b-528">Instance and Shared Variables</span></span>

<span data-ttu-id="b0f1b-529">Eine Instanz oder eine freigegebene Variable ist ein Member eines Typs, in dem Informationen gespeichert werden können.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-529">An instance or shared variable is a member of a type that can store information.</span></span>

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

<span data-ttu-id="b0f1b-530">Der `Dim`-Modifizierer muss angegeben werden, wenn keine Modifizierer angegeben werden, aber andernfalls weggelassen werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-530">The `Dim` modifier must be specified if no modifiers are specified, but may be omitted otherwise.</span></span> <span data-ttu-id="b0f1b-531">Eine einzelne Variablen Deklaration kann mehrere Variablen Deklaratoren enthalten. jeder Variablen Deklarator führt eine neue Instanz oder einen freigegebenen Member ein.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-531">A single variable declaration may include multiple variable declarators; each variable declarator introduces a new instance or shared member.</span></span>

<span data-ttu-id="b0f1b-532">Wenn ein Initialisierer angegeben ist, kann nur eine Instanz oder eine freigegebene Variable vom Variablen Deklarator deklariert werden:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-532">If an initializer is specified, only one instance or shared variable may be declared by the variable declarator:</span></span>

```vb
Class Test
    Dim a, b, c, d As Integer = 10  ' Invalid: multiple initialization
End Class
```

<span data-ttu-id="b0f1b-533">Diese Einschränkung gilt nicht für Objektinitialisierer:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-533">This restriction does not apply to object initializers:</span></span>

```vb
Class Test
    Dim a, b, c, d As New Collection() ' OK
End Class
```

<span data-ttu-id="b0f1b-534">Eine Variable, die mit dem `Shared`-Modifizierer deklariert wird, ist eine frei *gegebene Variable*.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-534">A variable declared with the `Shared` modifier is a *shared variable*.</span></span> <span data-ttu-id="b0f1b-535">Eine freigegebene Variable identifiziert genau einen Speicherort, unabhängig von der Anzahl der Instanzen des Typs, die erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-535">A shared variable identifies exactly one storage location regardless of the number of instances of the type that are created.</span></span> <span data-ttu-id="b0f1b-536">Eine freigegebene Variable ist vorhanden, wenn ein Programm mit der Ausführung beginnt, und ist nicht mehr vorhanden, wenn das Programm beendet wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-536">A shared variable comes into existence when a program begins executing, and ceases to exist when the program terminates.</span></span>

<span data-ttu-id="b0f1b-537">Eine freigegebene Variable wird nur für Instanzen eines bestimmten geschlossenen generischen Typs freigegeben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-537">A shared variable is shared only among instances of a particular closed generic type.</span></span> <span data-ttu-id="b0f1b-538">Das Programm lautet z. b.:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-538">For example, the program:</span></span>

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

<span data-ttu-id="b0f1b-539">Druckt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-539">Prints out:</span></span>

```console
1
1
2
```

<span data-ttu-id="b0f1b-540">Eine Variable, die ohne den `Shared`-Modifizierer deklariert wird, wird als *Instanzvariable*bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-540">A variable declared without the `Shared` modifier is called an *instance variable*.</span></span> <span data-ttu-id="b0f1b-541">Jede Instanz einer Klasse enthält eine separate Kopie aller Instanzvariablen der Klasse.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-541">Every instance of a class contains a separate copy of all instance variables of the class.</span></span> <span data-ttu-id="b0f1b-542">Eine Instanzvariable eines Verweis Typs kommt in Kraft, wenn eine neue Instanz dieses Typs erstellt wird, und ist nicht mehr vorhanden, wenn keine Verweise auf diese Instanz vorhanden sind und die `Finalize`-Methode ausgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-542">An instance variable of a reference type comes into existence when a new instance of that type is created, and ceases to exist when there are no references to that instance and the `Finalize` method has executed.</span></span> <span data-ttu-id="b0f1b-543">Eine Instanzvariable eines Werttyps hat genau die gleiche Lebensdauer wie die Variable, zu der Sie gehört.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-543">An instance variable of a value type has exactly the same lifetime as the variable to which it belongs.</span></span> <span data-ttu-id="b0f1b-544">Anders ausgedrückt: Wenn eine Variable eines Werttyps vorhanden ist oder nicht mehr vorhanden ist, wird die Instanzvariable des Werttyps verwendet.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-544">In other words, when a variable of a value type comes into existence or ceases to exist, so does the instance variable of the value type.</span></span>

<span data-ttu-id="b0f1b-545">Wenn der Deklarator eine `As`-Klausel enthält, gibt die-Klausel den Typ der Member an, die von der Deklaration eingeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-545">If the declarator contains an `As` clause, the clause specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="b0f1b-546">Wenn der Typ weggelassen wird und eine strikte Semantik verwendet wird, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-546">If the type is omitted and strict semantics are being used, a compile-time error occurs.</span></span> <span data-ttu-id="b0f1b-547">Andernfalls ist der Typ der Member implizit `Object` oder der Typ des Typzeichens des Members.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-547">Otherwise the type of the members is implicitly `Object` or the type of the members' type character.</span></span>

<span data-ttu-id="b0f1b-548">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="b0f1b-548">__Note.__</span></span> <span data-ttu-id="b0f1b-549">In der Syntax gibt es keine Mehrdeutigkeit: Wenn ein Deklarator einen Typ auslässt, wird immer der Typ eines folgenden Deklarators verwendet.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-549">There is no ambiguity in the syntax: if a declarator omits a type, it will always use the type of a following declarator.</span></span>

<span data-ttu-id="b0f1b-550">Die Zugriffs Domäne einer Instanz oder eines Typ-oder Array Elementtyps der freigegebenen Variablen muss mit oder einer übergeordneten Zugriffs Domäne der Instanz oder der freigegebenen Variablen selbst identisch sein.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-550">The accessibility domain of an instance or shared variable's type or array element type must be the same as or a superset of the accessibility domain of the instance or shared variable itself.</span></span>

<span data-ttu-id="b0f1b-551">Im folgenden Beispiel wird eine `Color`-Klasse gezeigt, die über interne Instanzvariablen mit dem Namen `redPart`, `greenPart` und `bluePart` verfügt:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-551">The following example shows a `Color` class that has internal instance variables named `redPart`, `greenPart`, and `bluePart`:</span></span>

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


### <a name="read-only-variables"></a><span data-ttu-id="b0f1b-552">Schreibgeschützte Variablen</span><span class="sxs-lookup"><span data-stu-id="b0f1b-552">Read-Only Variables</span></span>

<span data-ttu-id="b0f1b-553">Wenn eine Instanz oder eine freigegebene Variablen Deklaration einen `ReadOnly`-Modifizierer enthält, können Zuweisungen zu den Variablen, die von der Deklaration eingeführt wurden, nur als Teil der Deklaration oder in einem Konstruktor in derselben Klasse auftreten.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-553">When an instance or shared variable declaration includes a `ReadOnly` modifier, assignments to the variables introduced by the declaration may only occur as part of the declaration or in a constructor in the same class.</span></span> <span data-ttu-id="b0f1b-554">Insbesondere sind Zuweisungen zu einer schreibgeschützten Instanz oder einer freigegebenen Variablen nur in den folgenden Situationen zulässig:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-554">Specifically, assignments to a read-only instance or shared variable are permitted only in the following situations:</span></span>

* <span data-ttu-id="b0f1b-555">In der Variablen Deklaration, die die Instanz oder die freigegebene Variable (durch Einschließen eines variableninitialisierers in die Deklaration) einführt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-555">In the variable declaration that introduces the instance or shared variable (by including a variable initializer in the declaration).</span></span>

* <span data-ttu-id="b0f1b-556">Bei einer Instanzvariablen in den Instanzkonstruktoren der-Klasse, die die Variablen Deklaration enthält.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-556">For an instance variable, in the instance constructors of the class that contains the variable declaration.</span></span> <span data-ttu-id="b0f1b-557">Der Zugriff auf die Instanzvariable ist nur auf eine nicht qualifizierte Weise möglich oder über `Me` oder `MyClass`.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-557">The instance variable can only be accessed in an unqualified manner or through `Me` or `MyClass`.</span></span>

* <span data-ttu-id="b0f1b-558">Für eine freigegebene Variable im freigegebenen Konstruktor der-Klasse, die die Deklaration der freigegebenen Variablen enthält.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-558">For a shared variable, in the shared constructor of the class that contains the shared variable declaration.</span></span>

<span data-ttu-id="b0f1b-559">Eine freigegebene, schreibgeschützte Variable ist nützlich, wenn ein symbolischer Name für einen konstanten Wert erwünscht ist, aber wenn der Typ des Werts in einer Konstanten Deklaration nicht zulässig ist oder wenn der Wert nicht zur Kompilierzeit durch einen konstanten Ausdruck berechnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-559">A shared read-only variable is useful when a symbolic name for a constant value is desired, but when the type of the value is not permitted in a constant declaration, or when the value cannot be computed at compile time by a constant expression.</span></span>

<span data-ttu-id="b0f1b-560">Es folgt ein Beispiel für die erste Anwendung dieser Anwendung, bei der freigegebene Farben als `ReadOnly` deklariert werden, um zu verhindern, dass Sie von anderen Programmen geändert werden:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-560">An example of the first such application follows, in which color shared variables are declared `ReadOnly` to prevent them from being changed by other programs:</span></span>

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

<span data-ttu-id="b0f1b-561">Konstanten und schreibgeschützte freigegebene Variablen weisen eine unterschiedliche Semantik auf.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-561">Constants and read-only shared variables have different semantics.</span></span> <span data-ttu-id="b0f1b-562">Wenn ein Ausdruck auf eine Konstante verweist, wird der Wert der Konstante zur Kompilierzeit abgerufen. Wenn jedoch ein Ausdruck auf eine schreibgeschützte, freigegebene Variable verweist, wird der Wert der freigegebenen Variablen bis zur Laufzeit nicht abgerufen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-562">When an expression references a constant, the value of the constant is obtained at compile time, but when an expression references a read-only shared variable, the value of the shared variable is not obtained until run time.</span></span> <span data-ttu-id="b0f1b-563">Beachten Sie die folgende Anwendung, die aus zwei separaten Programmen besteht.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-563">Consider the following application, which consists of two separate programs.</span></span>

<span data-ttu-id="b0f1b-564">file1. vb:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-564">file1.vb:</span></span>

```vb
Namespace Program1
    Public Class Utils
        Public Shared ReadOnly X As Integer = 1
    End Class
End Namespace
```

<span data-ttu-id="b0f1b-565">file2. vb:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-565">file2.vb:</span></span>

```vb
Namespace Program2
    Module Test
        Sub Main()
            Console.WriteLine(Program1.Utils.X)
        End Sub
    End Module
End Namespace
```

<span data-ttu-id="b0f1b-566">Die Namespaces `Program1` und `Program2` bezeichnen zwei Programme, die separat kompiliert werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-566">The namespaces `Program1` and `Program2` denote two programs that are compiled separately.</span></span> <span data-ttu-id="b0f1b-567">Da Variable `Program1.Utils.X` als `Shared ReadOnly` deklariert ist, wird der Wert, der von der `Console.WriteLine`-Anweisung ausgegeben wird, zur Kompilierzeit nicht bekannt, sondern zur Laufzeit abgerufen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-567">Because variable `Program1.Utils.X` is declared as `Shared ReadOnly`, the value output by the `Console.WriteLine` statement is not known at compile time, but rather is obtained at run time.</span></span> <span data-ttu-id="b0f1b-568">Wenn der Wert `X` geändert und `Program1` erneut kompiliert wird, gibt die `Console.WriteLine`-Anweisung den neuen Wert auch dann aus, wenn `Program2` nicht erneut kompiliert wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-568">Thus, if the value of `X` is changed and `Program1` is recompiled, the `Console.WriteLine` statement will output the new value even if `Program2` is not recompiled.</span></span> <span data-ttu-id="b0f1b-569">Wenn `X` jedoch eine Konstante war, wurde der Wert von `X` zum Zeitpunkt der Kompilierung von `Program2` abgerufen, und hätte von Änderungen in `Program1` bis zum erneuten Kompilieren von `Program2` nicht beeinträchtigt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-569">However, if `X` had been a constant, the value of `X` would have been obtained at the time `Program2` was compiled, and would have remained unaffected by changes in `Program1` until `Program2` was recompiled.</span></span>

### <a name="withevents-variables"></a><span data-ttu-id="b0f1b-570">Widervents-Variablen</span><span class="sxs-lookup"><span data-stu-id="b0f1b-570">WithEvents Variables</span></span>

<span data-ttu-id="b0f1b-571">Ein Typ kann deklarieren, dass er eine Reihe von Ereignissen verarbeitet, die von einer seiner Instanzen oder freigegebenen Variablen ausgelöst werden, indem die Instanz oder die freigegebene Variable, die die Ereignisse auslöst, mit dem `WithEvents`-Modifizierer deklariert wird</span><span class="sxs-lookup"><span data-stu-id="b0f1b-571">A type can declare that it handles some set of events raised by one of its instance or shared variables by declaring the instance or shared variable that raises the events with the `WithEvents` modifier.</span></span> <span data-ttu-id="b0f1b-572">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-572">For example:</span></span>

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

<span data-ttu-id="b0f1b-573">In diesem Beispiel behandelt die-Methode `E1Handler` das Ereignis `E1`, das von der Instanz des Typs `Raiser` ausgelöst wird, der in der Instanzvariable `x` gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-573">In this example, the method `E1Handler` handles the event `E1` that is raised by the instance of the type `Raiser` stored in the instance variable `x`.</span></span>

<span data-ttu-id="b0f1b-574">Der `WithEvents`-Modifizierer bewirkt, dass die Variable mit einem führenden Unterstrich umbenannt und durch eine Eigenschaft mit dem gleichen Namen ersetzt wird, der das Ereignis einrichtet.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-574">The `WithEvents` modifier causes the variable to be renamed with a leading underscore and replaced with a property of the same name that does the event hookup.</span></span> <span data-ttu-id="b0f1b-575">Wenn der Name der Variablen beispielsweise `F` ist, wird Sie in `_F` umbenannt, und eine Eigenschaft `F` wird implizit deklariert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-575">For example, if the variable's name is `F`, it is renamed to `_F` and a property `F` is implicitly declared.</span></span> <span data-ttu-id="b0f1b-576">Wenn eine Kollision zwischen dem neuen Namen der Variablen und einer anderen Deklaration vorliegt, wird ein Kompilierzeitfehler gemeldet.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-576">If there is a collision between the variable's new name and another declaration, a compile-time error will be reported.</span></span> <span data-ttu-id="b0f1b-577">Alle Attribute, die auf die Variable angewendet werden, werden auf die umbenannte Variable übertragen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-577">Any attributes applied to the variable are carried over to the renamed variable.</span></span>

<span data-ttu-id="b0f1b-578">Die implizite Eigenschaft, die von einer `WithEvents`-Deklaration erstellt wurde, kümmert sich um das Verknüpfen und Aufheben der entsprechenden Ereignishandler.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-578">The implicit property created by a `WithEvents` declaration takes care of hooking and unhooking the relevant event handlers.</span></span> <span data-ttu-id="b0f1b-579">Wenn der Variablen ein Wert zugewiesen wird, ruft die-Eigenschaft zuerst die `remove`-Methode für das-Ereignis auf der-Instanz auf, die sich derzeit in der-Variable befindet (wobei der vorhandene Ereignishandler ggf. unverknüpft ist).</span><span class="sxs-lookup"><span data-stu-id="b0f1b-579">When a value is assigned to the variable, the property first calls the `remove` method for the event on the instance currently in the variable (unhooking the existing event handler, if any).</span></span> <span data-ttu-id="b0f1b-580">Als nächstes wird die Zuweisung vorgenommen, und die-Eigenschaft ruft die `add`-Methode für das-Ereignis auf der neuen Instanz in der-Variable auf (verknüpft den neuen Ereignishandler).</span><span class="sxs-lookup"><span data-stu-id="b0f1b-580">Next the assignment is made, and the property calls the `add` method for the event on the new instance in the variable (hooking up the new event handler).</span></span> <span data-ttu-id="b0f1b-581">Der folgende Code entspricht dem obigen Code für das Standardmodul `Test`:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-581">The following code is equivalent to the code above for the standard module `Test`:</span></span>

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

<span data-ttu-id="b0f1b-582">Es ist nicht zulässig, eine Instanz oder eine freigegebene Variable als `WithEvents` zu deklarieren, wenn die Variable als Struktur typisiert ist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-582">It is not valid to declare an instance or shared variable as `WithEvents` if the variable is typed as a structure.</span></span> <span data-ttu-id="b0f1b-583">Darüber hinaus darf `WithEvents` nicht in einer Struktur angegeben werden, und `WithEvents` und `ReadOnly` können nicht kombiniert werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-583">In addition, `WithEvents` may not be specified in a structure, and `WithEvents` and `ReadOnly` cannot be combined.</span></span>

### <a name="variable-initializers"></a><span data-ttu-id="b0f1b-584">Variableninitialisierer</span><span class="sxs-lookup"><span data-stu-id="b0f1b-584">Variable Initializers</span></span>

<span data-ttu-id="b0f1b-585">Deklarationen von Instanzen und freigegebenen Variablen in Klassen und Instanzen Variablen Deklarationen (aber nicht freigegebene Variablen Deklarationen) in Strukturen können Variableninitialisierer enthalten.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-585">Instance and shared variable declarations in classes and instance variable declarations (but not shared variable declarations) in structures may include variable initializers.</span></span> <span data-ttu-id="b0f1b-586">Für Variablen vom Typ "`Shared`" entsprechen Variableninitialisierern Zuweisungs Anweisungen, die nach Beginn des Programms ausgeführt werden, aber vor dem ersten Verweis auf die Variable "`Shared`".</span><span class="sxs-lookup"><span data-stu-id="b0f1b-586">For `Shared` variables, variable initializers correspond to assignment statements that are executed after the program begins, but before the `Shared` variable is first referenced.</span></span> <span data-ttu-id="b0f1b-587">Bei Instanzvariablen entsprechen Variableninitialisierern Zuweisungs Anweisungen, die ausgeführt werden, wenn eine Instanz der-Klasse erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-587">For instance variables, variable initializers correspond to assignment statements that are executed when an instance of the class is created.</span></span> <span data-ttu-id="b0f1b-588">Strukturen können keine instanzvariableninitialisierer aufweisen, da ihre Parameter losen Konstruktoren nicht geändert werden können.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-588">Structures cannot have instance variable initializers because their parameterless constructors cannot be modified.</span></span>

<span data-ttu-id="b0f1b-589">Betrachten Sie das folgende Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-589">Consider the following example:</span></span>

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

<span data-ttu-id="b0f1b-590">Das Beispiel führt zur folgenden Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-590">The example produces the following output:</span></span>

```console
x = 1.4142135623731, i = 100, s = Hello
```

<span data-ttu-id="b0f1b-591">Eine Zuweisung zu "`x`" tritt auf, wenn die Klasse geladen wird und Zuweisungen zu `i` und `s` auftreten, wenn eine neue Instanz der Klasse erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-591">An assignment to `x` occurs when the class is loaded, and assignments to `i` and `s` occur when a new instance of the class is created.</span></span>

<span data-ttu-id="b0f1b-592">Es ist hilfreich, Variablen Initialisierer als Zuweisungs Anweisungen zu betrachten, die automatisch in den-Block des Konstruktors des Typs eingefügt werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-592">It is useful to think of variable initializers as assignment statements that are automatically inserted in the block of the type's constructor.</span></span> <span data-ttu-id="b0f1b-593">Das folgende Beispiel enthält mehrere instanzvariableninitialisierer.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-593">The following example contains several instance variable initializers.</span></span>

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

<span data-ttu-id="b0f1b-594">Das Beispiel entspricht dem unten gezeigten Code, wobei jeder Kommentar eine automatisch eingefügte Anweisung angibt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-594">The example corresponds to the code shown below, where each comment indicates an automatically inserted statement.</span></span>

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

<span data-ttu-id="b0f1b-595">Alle Variablen werden mit dem Standardwert ihres Typs initialisiert, bevor Variablen Initialisierer ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-595">All variables are initialized to the default value of their type before any variable initializers are executed.</span></span> <span data-ttu-id="b0f1b-596">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-596">For example:</span></span>

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

<span data-ttu-id="b0f1b-597">Da `b` automatisch mit dem Standardwert initialisiert wird, wenn die-Klasse geladen wird und `i` automatisch mit dem Standardwert initialisiert wird, wenn eine Instanz der-Klasse erstellt wird, erzeugt der vorangehende Code die folgende Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-597">Because `b` is automatically initialized to its default value when the class is loaded and `i` is automatically initialized to its default value when an instance of the class is created, the preceding code produces the following output:</span></span>

```console
b = False, i = 0
```

<span data-ttu-id="b0f1b-598">Jeder Variableninitialisierer muss einen Wert des Variablen Typs oder eines Typs liefern, der implizit in den Typ der Variablen konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-598">Each variable initializer must yield a value of the variable's type or of a type that is implicitly convertible to the variable's type.</span></span> <span data-ttu-id="b0f1b-599">Ein Variableninitialisierer kann zirkulär sein oder auf eine Variable verweisen, die danach initialisiert wird. in diesem Fall ist der Wert der Variablen, auf die verwiesen wird, der Standardwert für den Initialisierer.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-599">A variable initializer may be circular or refer to a variable that will be initialized after it, in which case the value of the referenced variable is its default value for the purposes of the initializer.</span></span> <span data-ttu-id="b0f1b-600">Ein solcher Initialisierer weist einen zweifelhaften Wert auf.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-600">Such an initializer is of dubious value.</span></span>

<span data-ttu-id="b0f1b-601">Es gibt drei Formen von Variableninitialisierern: reguläre Initialisierer, arraygrößeninitialisierer und Objektinitialisierer.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-601">There are three forms of variable initializers: regular initializers, array-size initializers, and object initializers.</span></span> <span data-ttu-id="b0f1b-602">Die ersten beiden Formulare werden nach einem Gleichheitszeichen angezeigt, das auf den Typnamen folgt. die beiden beiden Formulare sind Teil der Deklaration selbst.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-602">The first two forms appear after an equal sign that follows the type name, the latter two are part of the declaration itself.</span></span> <span data-ttu-id="b0f1b-603">Für eine bestimmte Deklaration kann nur eine Form des Initialisierers verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-603">Only one form of initializer may be used on any particular declaration.</span></span>

#### <a name="regular-initializers"></a><span data-ttu-id="b0f1b-604">Reguläre Initialisierer</span><span class="sxs-lookup"><span data-stu-id="b0f1b-604">Regular Initializers</span></span>

<span data-ttu-id="b0f1b-605">Ein *regulärer Initialisierer* ist ein Ausdruck, der implizit in den Typ der Variablen konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-605">A *regular initializer* is an expression that is implicitly convertible to the type of the variable.</span></span> <span data-ttu-id="b0f1b-606">Sie wird nach einem Gleichheitszeichen angezeigt, das auf den Typnamen folgt und als Wert klassifiziert werden muss.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-606">It appears after an equal sign that follows the type name and must be classified as a value.</span></span> <span data-ttu-id="b0f1b-607">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-607">For example:</span></span>

```vb
Module Test
    Dim x As Integer = 10
    Dim y As Integer = 20

    Sub Main()
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub
End Module
```

<span data-ttu-id="b0f1b-608">Dieses Programm erzeugt die Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-608">This program produces the output:</span></span>

```console
x = 10, y = 20
```

<span data-ttu-id="b0f1b-609">Wenn eine Variablen Deklaration einen regulären Initialisierer aufweist, kann jeweils nur eine Variable deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-609">If a variable declaration has a regular initializer, then only a single variable can be declared at a time.</span></span> <span data-ttu-id="b0f1b-610">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-610">For example:</span></span>

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

#### <a name="object-initializers"></a><span data-ttu-id="b0f1b-611">Objektinitialisierer</span><span class="sxs-lookup"><span data-stu-id="b0f1b-611">Object Initializers</span></span>

<span data-ttu-id="b0f1b-612">Ein *Objektinitialisierer* wird mit einem Objekt Erstellungs Ausdruck an der Stelle des Typnamens angegeben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-612">An *object initializer* is specified using an object creation expression in the place of the type name.</span></span> <span data-ttu-id="b0f1b-613">Ein Objektinitialisierer entspricht einem regulären Initialisierer, der das Ergebnis des Ausdrucks zur Objekt Erstellung der Variablen zuweist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-613">An object initializer is equivalent to a regular initializer assigning the result of the object creation expression to the variable.</span></span> <span data-ttu-id="b0f1b-614">So</span><span class="sxs-lookup"><span data-stu-id="b0f1b-614">So</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As New Test(10)
    End Sub
End Module
```

<span data-ttu-id="b0f1b-615">für die folgende Syntax:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-615">is equivalent to</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test(10)
    End Sub
End Module
```

<span data-ttu-id="b0f1b-616">Die Klammer in einem Objektinitialisierer wird immer als Argumentliste für den Konstruktor und nie als Arraytypmodifizierer interpretiert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-616">The parenthesis in an object initializer is always interpreted as the argument list for the constructor and never as array type modifiers.</span></span> <span data-ttu-id="b0f1b-617">Ein Variablenname mit einem Objektinitialisierer kann keinen Arraytypmodifizierer oder einen Typmodifizierer, der NULL-Werte zulässt, aufweisen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-617">A variable name with an object initializer cannot have an array type modifier or a nullable type modifier.</span></span>

#### <a name="array-size-initializers"></a><span data-ttu-id="b0f1b-618">Initialisierer für Array Größe</span><span class="sxs-lookup"><span data-stu-id="b0f1b-618">Array-Size Initializers</span></span>

<span data-ttu-id="b0f1b-619">Ein *arraygrößeninitialisierer* ist ein Modifizierer für den Namen der Variablen, der einen Satz von Dimensions Obergrenzen bietet, der durch Ausdrücke gekennzeichnet ist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-619">An *array-size initializer* is a modifier on the name of the variable that gives a set of dimension upper bounds denoted by expressions.</span></span>

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

<span data-ttu-id="b0f1b-620">Die oberen gebundenen Ausdrücke müssen als Werte klassifiziert werden und müssen implizit in `Integer` konvertiert werden können.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-620">The upper bound expressions must be classified as values and must be implicitly convertible to `Integer`.</span></span> <span data-ttu-id="b0f1b-621">Der Satz von oberen Begrenzungen entspricht einem Variableninitialisierer eines Ausdrucks der Array Erstellung mit den angegebenen Obergrenzen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-621">The set of upper bounds is equivalent to a variable initializer of an array-creation expression with the given upper bounds.</span></span> <span data-ttu-id="b0f1b-622">Die Anzahl der Dimensionen des Arraytyps wird aus dem arraygrößeninitialisierer abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-622">The number of dimensions of the array type is inferred from the array size initializer.</span></span> <span data-ttu-id="b0f1b-623">So</span><span class="sxs-lookup"><span data-stu-id="b0f1b-623">So</span></span>

```vb
Module Test
    Sub Main()
        Dim x(5, 10) As Integer
    End Sub
End Module
```

<span data-ttu-id="b0f1b-624">für die folgende Syntax:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-624">is equivalent to</span></span>

```vb
Module Test
    Sub Main()
        Dim x As Integer(,) = New Integer(5, 10) {}
    End Sub
End Module
```

<span data-ttu-id="b0f1b-625">Alle oberen Begrenzungen müssen größer oder gleich-1 sein, und für alle Dimensionen muss eine obere Grenze angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-625">All upper bounds must be equal to or greater than -1, and all dimensions must have an upper bound specified.</span></span> <span data-ttu-id="b0f1b-626">Wenn der Elementtyp des zu initialisierenden Arrays selbst ein Arraytyp ist, werden die Arraytypmodifizierer auf der rechten Seite des Initialisierers der Array Größe angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-626">If the element type of the array being initialized is itself an array type, the array-type modifiers go to the right of the array-size initializer.</span></span> <span data-ttu-id="b0f1b-627">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-627">For example</span></span>

```vb
Module Test
    Sub Main()
        Dim x(5,10)(,,) As Integer
    End Sub
End Module
```

<span data-ttu-id="b0f1b-628">deklariert eine lokale Variable `x`, deren Typ ein zweidimensionales Array von dreidimensionalen Arrays von `Integer` ist, die auf ein Array mit den Begrenzungen `0..5` in der ersten Dimension und `0..10` in der zweiten Dimension initialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-628">declares a local variable `x` whose type is a two-dimensional array of three-dimensional arrays of `Integer`, initialized to an array with bounds of `0..5` in the first dimension and `0..10` in the second dimension.</span></span> <span data-ttu-id="b0f1b-629">Es ist nicht möglich, einen arraygrößeninitialisierer zu verwenden, um die Elemente einer Variablen zu initialisieren, deren Typ ein Array von Arrays ist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-629">It is not possible to use an array size initializer to initialize the elements of a variable whose type is an array of arrays.</span></span>

<span data-ttu-id="b0f1b-630">Eine Variablen Deklaration mit einem Initialisierer mit Array Größe kann keinen Arraytypmodifizierer für den Typ oder einen regulären Initialisierer aufweisen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-630">A variable declaration with an array-size initializer cannot have an array type modifier on its type or a regular initializer.</span></span>


### <a name="systemmarshalbyrefobject-classes"></a><span data-ttu-id="b0f1b-631">System. MarshalByRefObject-Klassen</span><span class="sxs-lookup"><span data-stu-id="b0f1b-631">System.MarshalByRefObject Classes</span></span>

<span data-ttu-id="b0f1b-632">Klassen, die von der-Klasse `System.MarshalByRefObject` abgeleitet werden, werden über Kontext Grenzen hinweg gemarshallt, und zwar mithilfe von Proxys (d. h. durch Verweis) anstatt durch Kopieren (d. h. durch Wert)</span><span class="sxs-lookup"><span data-stu-id="b0f1b-632">Classes that derive from the class `System.MarshalByRefObject` are marshaled across context boundaries using proxies (that is, by reference) rather than through copying (that is, by value).</span></span> <span data-ttu-id="b0f1b-633">Dies bedeutet, dass eine Instanz einer solchen Klasse möglicherweise keine echte Instanz ist, sondern stattdessen ein Stub sein kann, der Variablen Zugriffe und Methodenaufrufe über eine Kontext Grenze hinweg marshunrätet.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-633">This means that an instance of such a class may not be a true instance but instead may just be a stub that marshals variable accesses and method calls across a context boundary.</span></span>

<span data-ttu-id="b0f1b-634">Folglich ist es nicht möglich, einen Verweis auf den Speicherort von Variablen zu erstellen, die für solche Klassen definiert sind.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-634">As a result, it is not possible to create a reference to the storage location of variables defined on such classes.</span></span> <span data-ttu-id="b0f1b-635">Dies bedeutet, dass Variablen, die als von `System.MarshalByRefObject` abgeleitete Klassen eingegeben werden, nicht an Verweis Parameter und Methoden und Variablen von Variablen, die als Werttypen typisiert sind, möglicherweise nicht zugegriffen werden können.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-635">This means that variables typed as classes derived from `System.MarshalByRefObject` cannot be passed to reference parameters, and methods and variables of variables typed as value types may not be accessed.</span></span> <span data-ttu-id="b0f1b-636">Stattdessen Visual Basic die Variablen, die für solche Klassen definiert sind, so behandelt, als wären Sie Eigenschaften (da die Einschränkungen bei Eigenschaften identisch sind).</span><span class="sxs-lookup"><span data-stu-id="b0f1b-636">Instead, Visual Basic treats variables defined on such classes as if they were properties (since the restrictions are the same on properties).</span></span>

<span data-ttu-id="b0f1b-637">Es gibt eine Ausnahme von dieser Regel: ein Member, der implizit oder explizit mit `Me` qualifiziert ist, ist von den oben genannten Einschränkungen ausgenommen, da `Me` immer garantiert ein tatsächliches Objekt und kein Proxy ist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-637">There is one exception to this rule: a member implicitly or explicitly qualified with `Me` is exempt from the above restrictions, because `Me` is always guaranteed to be an actual object, not a proxy.</span></span>

## <a name="properties"></a><span data-ttu-id="b0f1b-638">Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="b0f1b-638">Properties</span></span>

<span data-ttu-id="b0f1b-639">*Eigenschaften* sind eine natürliche Erweiterung von Variablen. Beide sind benannte Member mit zugeordneten Typen, und die Syntax für den Zugriff auf Variablen und Eigenschaften ist identisch.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-639">*Properties* are a natural extension of variables; both are named members with associated types, and the syntax for accessing variables and properties is the same.</span></span> <span data-ttu-id="b0f1b-640">Im Gegensatz zu Variablen bezeichnen Eigenschaften jedoch keine Speicherorte.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-640">Unlike variables, however, properties do not denote storage locations.</span></span> <span data-ttu-id="b0f1b-641">Stattdessen verfügen Eigenschaften über *Accessoren*, die die auszuführenden Anweisungen angeben, um ihre Werte zu lesen oder zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-641">Instead, properties have *accessors*, which specify the statements to execute in order to read or write their values.</span></span>

<span data-ttu-id="b0f1b-642">Eigenschaften werden mit Eigenschafts Deklarationen definiert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-642">Properties are defined with property declarations.</span></span> <span data-ttu-id="b0f1b-643">Der erste Teil einer Eigenschafts Deklaration ähnelt einer Feld Deklaration.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-643">The first part of a property declaration resembles a field declaration.</span></span> <span data-ttu-id="b0f1b-644">Der zweite Teil enthält einen `Get`-Accessor und/oder einen `Set`-Accessor.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-644">The second part includes a `Get` accessor and/or a `Set` accessor.</span></span>

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

<span data-ttu-id="b0f1b-645">Im folgenden Beispiel definiert die Klasse `Button` eine Eigenschaft `Caption`.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-645">In the example below, the `Button` class defines a `Caption` property.</span></span>

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

<span data-ttu-id="b0f1b-646">Das folgende Beispiel zeigt die Verwendung der Eigenschaft "`Caption`" basierend auf der `Button`-Klasse:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-646">Based on the `Button` class above, the following is an example of use of the `Caption` property:</span></span>

```vb
Dim okButton As Button = New Button()

okButton.Caption = "OK" ' Invokes Set accessor.
Dim s As String = okButton.Caption ' Invokes Get accessor.
```

<span data-ttu-id="b0f1b-647">Hier wird der `Set`-Accessor aufgerufen, indem der-Eigenschaft ein Wert zugewiesen wird, und der `Get`-Accessor wird aufgerufen, indem auf die-Eigenschaft in einem Ausdruck verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-647">Here, the `Set` accessor is invoked by assigning a value to the property, and the `Get` accessor is invoked by referencing the property in an expression.</span></span>

<span data-ttu-id="b0f1b-648">Wenn für eine Eigenschaft kein Typ angegeben ist und eine strikte Semantik verwendet wird, tritt ein Kompilierzeitfehler auf. Andernfalls ist der Typ der Eigenschaft implizit `Object` oder der Typ des Typzeichens der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-648">If no type is specified for a property and strict semantics are being used, a compile-time error occurs; otherwise the type of the property is implicitly `Object` or the type of the property's type character.</span></span> <span data-ttu-id="b0f1b-649">Eine Eigenschafts Deklaration kann entweder einen `Get`-Accessor enthalten, der den Wert der-Eigenschaft abruft, einen `Set`-Accessor, der den Wert der-Eigenschaft speichert, oder beides.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-649">A property declaration may contain either a `Get` accessor, which retrieves the value of the property, a `Set` accessor, which stores the value of the property, or both.</span></span> <span data-ttu-id="b0f1b-650">Da eine Eigenschaft Methoden implizit deklariert, kann eine Eigenschaft mit denselben modifiziererobjekten wie eine Methode deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-650">Because a property implicitly declares methods, a property may be declared with the same modifiers as a method.</span></span> <span data-ttu-id="b0f1b-651">Wenn die Eigenschaft in einer Schnittstelle definiert oder mit dem `MustOverride`-Modifizierer definiert ist, müssen der Eigenschaften Text und das `End`-Konstrukt ausgelassen werden. Andernfalls tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-651">If the property is defined in an interface or defined with the `MustOverride` modifier, the property body and the `End` construct must be omitted; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="b0f1b-652">Die Index Parameterliste bildet die Signatur der-Eigenschaft, sodass Eigenschaften möglicherweise für Index Parameter überladen werden, jedoch nicht für den Typ der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-652">The index parameter list makes up the signature of the property, so properties may be overloaded on index parameters but not on the type of the property.</span></span> <span data-ttu-id="b0f1b-653">Die Index Parameterliste ist die gleiche wie bei einer regulären Methode.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-653">The index parameter list is the same as for a regular method.</span></span> <span data-ttu-id="b0f1b-654">Allerdings kann keiner der Parameter mit dem `ByRef`-Modifizierer geändert werden, und keiner der Parameter kann `Value` lauten (der für den impliziten value-Parameter im `Set`-Accessor reserviert ist).</span><span class="sxs-lookup"><span data-stu-id="b0f1b-654">However, none of the parameters may be modified with the `ByRef` modifier and none of them may be named `Value` (which is reserved for the implicit value parameter in the `Set` accessor).</span></span>

<span data-ttu-id="b0f1b-655">Eine Eigenschaft kann wie folgt deklariert werden:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-655">A property may be declared as follows:</span></span>

* <span data-ttu-id="b0f1b-656">Wenn die-Eigenschaft keinen Eigenschaftentyp-Modifizierer angibt, muss die-Eigenschaft sowohl einen `Get`-Accessor als auch einen `Set`-Accessor aufweisen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-656">If the property specifies no property type modifier, the property must have both a `Get` accessor and a `Set` accessor.</span></span> <span data-ttu-id="b0f1b-657">Die Eigenschaft wird als Lese-/Schreibeigenschaft bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-657">The property is said to be a read-write property.</span></span>

* <span data-ttu-id="b0f1b-658">Wenn die-Eigenschaft den `ReadOnly`-Modifizierer angibt, muss die-Eigenschaft über einen `Get`-Accessor verfügen, der möglicherweise nicht über einen `Set`-Accessor verfügt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-658">If the property specifies the `ReadOnly` modifier, the property must have a `Get` accessor and may not have a `Set` accessor.</span></span> <span data-ttu-id="b0f1b-659">Die Eigenschaft wird als schreibgeschützte Eigenschaft bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-659">The property is said to be read-only property.</span></span> <span data-ttu-id="b0f1b-660">Es handelt sich um einen Kompilierzeitfehler für eine schreibgeschützte Eigenschaft, die das Ziel einer Zuweisung ist.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-660">It is a compile-time error for a read-only property to be the target of an assignment.</span></span>

* <span data-ttu-id="b0f1b-661">Wenn die-Eigenschaft den `WriteOnly`-Modifizierer angibt, muss die-Eigenschaft über einen `Set`-Accessor verfügen, der möglicherweise nicht über einen `Get`-Accessor verfügt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-661">If the property specifies the `WriteOnly` modifier, the property must have a `Set` accessor and may not have a `Get` accessor.</span></span> <span data-ttu-id="b0f1b-662">Die Eigenschaft wird als schreibgeschützte Eigenschaft bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-662">The property is said to be write-only property.</span></span> <span data-ttu-id="b0f1b-663">Es ist ein Kompilierzeitfehler, der auf eine schreibgeschützte Eigenschaft in einem Ausdruck verweist, außer als Ziel einer Zuweisung oder als Argument für eine Methode.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-663">It is a compile-time error to reference a write-only property in an expression except as the target of an assignment or as an argument to a method.</span></span>

<span data-ttu-id="b0f1b-664">Die `Get`-und `Set`-Accessoren einer Eigenschaft sind keine unterschiedlichen Member, und es ist nicht möglich, die Accessoren einer Eigenschaft separat zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-664">The `Get` and `Set` accessors of a property are not distinct members, and it is not possible to declare the accessors of a property separately.</span></span> <span data-ttu-id="b0f1b-665">Im folgenden Beispiel wird keine einzige Lese-/Schreibeigenschaft deklariert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-665">The following example does not declare a single read-write property.</span></span> <span data-ttu-id="b0f1b-666">Stattdessen werden zwei Eigenschaften mit demselben Namen deklariert, ein Schreib geschützter und ein Schreib geschützter Wert:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-666">Rather, it declares two properties with the same name, one read-only and one write-only:</span></span>

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

<span data-ttu-id="b0f1b-667">Da zwei Member, die in derselben Klasse deklariert werden, nicht denselben Namen haben können, verursacht das Beispiel einen Kompilierzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-667">Since two members declared in the same class cannot have the same name, the example causes a compile-time error.</span></span>

<span data-ttu-id="b0f1b-668">Standardmäßig ist der Zugriff auf die `Get`-und `Set`-Accessoren der Eigenschaft identisch mit der Barrierefreiheit der Eigenschaft selbst.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-668">By default, the accessibility of a property's `Get` and `Set` accessors is the same as the accessibility of the property itself.</span></span> <span data-ttu-id="b0f1b-669">Allerdings können die Accessoren "`Get`" und "`Set`" auch den Zugriff separat von der Eigenschaft angeben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-669">However, the `Get` and `Set` accessors can also specify accessibility separately from the property.</span></span> <span data-ttu-id="b0f1b-670">In diesem Fall muss der Zugriff auf einen Accessor restriktiver sein als der Zugriff auf die-Eigenschaft, und nur ein Accessor kann eine andere Barrierefreiheits Stufe als die-Eigenschaft aufweisen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-670">In that case, the accessibility of an accessor must be more restrictive than the accessibility of the property and only one accessor can have a different accessibility level from the property.</span></span> <span data-ttu-id="b0f1b-671">Zugriffs Typen werden wie folgt als mehr oder weniger restriktiv angesehen:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-671">Access types are considered more or less restrictive as follows:</span></span>

* <span data-ttu-id="b0f1b-672">`Private` ist restriktiver als `Public`, `Protected Friend`, `Protected` oder `Friend`.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-672">`Private` is more restrictive than `Public`, `Protected Friend`, `Protected`, or `Friend`.</span></span>

* <span data-ttu-id="b0f1b-673">`Friend` ist restriktiver als `Protected Friend` oder `Public`.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-673">`Friend` is more restrictive than `Protected Friend` or `Public`.</span></span>

* <span data-ttu-id="b0f1b-674">`Protected` ist restriktiver als `Protected Friend` oder `Public`.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-674">`Protected` is more restrictive than `Protected Friend` or `Public`.</span></span>

* <span data-ttu-id="b0f1b-675">`Protected Friend` ist restriktiver als `Public`.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-675">`Protected Friend` is more restrictive than `Public`.</span></span>

<span data-ttu-id="b0f1b-676">Wenn auf einen der Accessoren einer Eigenschaft zugegriffen werden kann, der andere jedoch nicht, wird die Eigenschaft so behandelt, als ob Sie schreibgeschützt oder schreibgeschützt wäre.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-676">When one of a property's accessors is accessible but the other one is not, the property is treated as if it was read-only or write-only.</span></span> <span data-ttu-id="b0f1b-677">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-677">For example:</span></span>

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

<span data-ttu-id="b0f1b-678">Wenn ein abgeleiteter Typ einen Shadowing für eine Eigenschaft durchführt, verbirgt die abgeleitete Eigenschaft die Shadowing-Eigenschaft in Bezug auf das Lesen und schreiben</span><span class="sxs-lookup"><span data-stu-id="b0f1b-678">When a derived type shadows a property, the derived property hides the shadowed property with respect to both reading and writing.</span></span> <span data-ttu-id="b0f1b-679">Im folgenden Beispiel blendet die `P`-Eigenschaft in `B` die `P`-Eigenschaft in `A` in Bezug auf Lese-und Schreibvorgänge aus:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-679">In the following example, the `P` property in `B` hides the `P` property in `A` with respect to both reading and writing:</span></span>

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

<span data-ttu-id="b0f1b-680">Die Zugriffs Domäne des Rückgabe Typs oder der Parametertypen muss mit oder einer übergeordneten Zugriffs Domäne der Eigenschaft selbst identisch sein.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-680">The accessibility domain of the return type or parameter types must be the same as or a superset of the accessibility domain of the property itself.</span></span> <span data-ttu-id="b0f1b-681">Eine Eigenschaft darf nur über einen `Set`-Accessor und einen `Get`-Accessor verfügen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-681">A property may only have one `Set` accessor and one `Get` accessor.</span></span>

<span data-ttu-id="b0f1b-682">Mit Ausnahme der Unterschiede in der Deklaration und der Aufruf Syntax Verhalten sich `Overridable`-, `NotOverridable`-, `Overrides`-, `MustOverride`-und `MustInherit`-Eigenschaften genau wie `Overridable`-, `NotOverridable`-, `Overrides`-, `MustOverride`-und `MustInherit`-Methoden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-682">Except for differences in declaration and invocation syntax, `Overridable`, `NotOverridable`, `Overrides`, `MustOverride`, and `MustInherit` properties behave exactly like `Overridable`, `NotOverridable`, `Overrides`, `MustOverride`, and `MustInherit` methods.</span></span> <span data-ttu-id="b0f1b-683">Wenn eine Eigenschaft überschrieben wird, muss die über schreibende Eigenschaft denselben Typ aufweisen (Lese-/Schreibzugriff, schreibgeschützt, schreibgeschützt).</span><span class="sxs-lookup"><span data-stu-id="b0f1b-683">When a property is overridden, the overriding property must be of the same type (read-write, read-only, write-only).</span></span> <span data-ttu-id="b0f1b-684">Eine `Overridable`-Eigenschaft darf keinen `Private`-Accessor enthalten.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-684">An `Overridable` property cannot contain a `Private` accessor.</span></span>

<span data-ttu-id="b0f1b-685">Im folgenden Beispiel ist `X` eine schreibgeschützte Eigenschaft `Overridable`. `Y` ist eine Lese-/Schreibeigenschaft vom `Overridable`, und `Z` ist eine `MustOverride`-Eigenschaft mit Lese-/Schreibzugriff.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-685">In the following example `X` is an `Overridable` read-only property, `Y` is an `Overridable` read-write property, and `Z` is a `MustOverride` read-write property.</span></span>

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

<span data-ttu-id="b0f1b-686">Da `Z` `MustOverride` ist, muss die enthaltende Klasse `A` `MustInherit` deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-686">Because `Z` is `MustOverride`, the containing class `A` must be declared `MustInherit`.</span></span>

<span data-ttu-id="b0f1b-687">Im Gegensatz dazu wird eine Klasse, die von der Klasse `A` abgeleitet ist, im folgenden dargestellt:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-687">By contrast, a class that derives from class `A` is shown below:</span></span>

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

<span data-ttu-id="b0f1b-688">Hier überschreiben die Deklarationen von Eigenschaften `X`, `Y` und `Z` die Basis Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-688">Here, the declarations of properties `X`,`Y`, and `Z` override the base properties.</span></span> <span data-ttu-id="b0f1b-689">Jede Eigenschaften Deklaration stimmt genau mit den zugriffsmodifizierertypen und dem Namen der entsprechenden geerbten Eigenschaft überein.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-689">Each property declaration exactly matches the accessibility modifiers, type, and name of the corresponding inherited property.</span></span> <span data-ttu-id="b0f1b-690">Der `Get`-Accessor der Eigenschaft `X` und der `Set`-Accessor der Eigenschaft `Y` verwenden das `MyBase`-Schlüsselwort für den Zugriff auf die geerbten Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-690">The `Get` accessor of property `X` and the `Set` accessor of property `Y` use the `MyBase` keyword to access the inherited properties.</span></span> <span data-ttu-id="b0f1b-691">Die Deklaration der Eigenschaft "`Z`" überschreibt die Eigenschaft "`MustOverride`". Folglich gibt es keine ausstehenden `MustOverride`-Member in der Klasse "`B`", und `B` darf eine reguläre Klasse sein.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-691">The declaration of property `Z` overrides the `MustOverride` property -- thus, there are no outstanding `MustOverride` members in class `B`, and `B` is permitted to be a regular class.</span></span>

<span data-ttu-id="b0f1b-692">Eigenschaften können verwendet werden, um die Initialisierung einer Ressource zu verzögern, bis zu dem Zeitpunkt, zu dem Sie erstmals referenziert wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-692">Properties can be used to delay initialization of a resource until the moment it is first referenced.</span></span> <span data-ttu-id="b0f1b-693">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-693">For example:</span></span>

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

<span data-ttu-id="b0f1b-694">Die `ConsoleStreams`-Klasse enthält drei Eigenschaften, `In`, `Out` und `Error`, die jeweils die standardmäßigen Eingabe-, Ausgabe-und Fehler Geräte darstellen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-694">The `ConsoleStreams` class contains three properties, `In`, `Out`, and `Error`, that represent the standard input, output, and error devices, respectively.</span></span> <span data-ttu-id="b0f1b-695">Wenn diese Member als Eigenschaften verfügbar gemacht werden, kann die `ConsoleStreams`-Klasse Ihre Initialisierung verzögern, bis Sie tatsächlich verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-695">By exposing these members as properties, the `ConsoleStreams` class can delay their initialization until they are actually used.</span></span> <span data-ttu-id="b0f1b-696">Wenn Sie z. b. zuerst auf die `Out`-Eigenschaft verweisen, wie in `ConsoleStreams.Out.WriteLine("hello, world")`, wird die zugrunde liegende `TextWriter` für das Ausgabegerät initialisiert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-696">For example, upon first referencing the `Out` property, as in `ConsoleStreams.Out.WriteLine("hello, world")`, the underlying `TextWriter` for the output device is initialized.</span></span> <span data-ttu-id="b0f1b-697">Wenn die Anwendung jedoch keinen Verweis auf die Eigenschaften `In` und `Error` erstellt, werden für diese Geräte keine Objekte erstellt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-697">But if the application makes no reference to the `In` and `Error` properties, then no objects are created for those devices.</span></span>


### <a name="get-accessor-declarations"></a><span data-ttu-id="b0f1b-698">Get-Accessor-Deklarationen</span><span class="sxs-lookup"><span data-stu-id="b0f1b-698">Get Accessor Declarations</span></span>

<span data-ttu-id="b0f1b-699">Ein `Get`-Accessor (Getter) wird mithilfe einer Eigenschaft `Get`-Deklaration deklariert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-699">A `Get` accessor (getter) is declared by using a property `Get` declaration.</span></span> <span data-ttu-id="b0f1b-700">Eine Eigenschaft `Get`-Deklaration besteht aus dem Schlüsselwort `Get`, gefolgt von einem Anweisungsblock.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-700">A property `Get` declaration consists of the keyword `Get` followed by a statement block.</span></span> <span data-ttu-id="b0f1b-701">Wenn eine Eigenschaft mit dem Namen "`P`" angegeben ist, wird eine Methode mit dem Namen "`get_P`" von einer `Get`-Accessor-Deklaration implizit mit denselben modifizierertypen und Parameterlisten wie die-Eigenschaft deklariert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-701">Given a property named `P`, a `Get` accessor declaration implicitly declares a method with the name `get_P` with the same modifiers, type, and parameter list as the property.</span></span> <span data-ttu-id="b0f1b-702">Wenn der Typ eine Deklaration mit diesem Namen enthält, wird ein Fehler bei der Kompilierzeit ausgegeben, die implizite Deklaration wird jedoch zum Zweck der namens Bindung ignoriert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-702">If the type contains a declaration with that name, a compile-time error results, but the implicit declaration is ignored for the purposes of name binding.</span></span>

<span data-ttu-id="b0f1b-703">Eine spezielle lokale Variable, die implizit im Deklarations Bereich des `Get`-Accessor mit dem gleichen Namen wie die-Eigenschaft deklariert wird, stellt den Rückgabewert der-Eigenschaft dar.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-703">A special local variable, which is implicitly declared in the `Get` accessor body's declaration space with the same name as the property, represents the return value of the property.</span></span> <span data-ttu-id="b0f1b-704">Die lokale Variable hat eine spezielle namens Auflösungs Semantik, wenn Sie in Ausdrücken verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-704">The local variable has special name resolution semantics when used in expressions.</span></span> <span data-ttu-id="b0f1b-705">Wenn die lokale Variable in einem Kontext verwendet wird, der einen Ausdruck erwartet, der als Methoden Gruppe klassifiziert ist (z. b. ein Aufruf Ausdruck), wird der Name in die Funktion und nicht in die lokale Variable aufgelöst.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-705">If the local variable is used in a context that expects an expression that is classified as a method group, such as an invocation expression, then the name resolves to the function rather than to the local variable.</span></span> <span data-ttu-id="b0f1b-706">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-706">For example:</span></span>

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

<span data-ttu-id="b0f1b-707">Die Verwendung von Klammern kann mehrdeutige Situationen verursachen (z. b. `F(1)`, wenn `F` eine Eigenschaft ist, deren Typ ein eindimensionales Array ist).</span><span class="sxs-lookup"><span data-stu-id="b0f1b-707">The use of parentheses can cause ambiguous situations (such as `F(1)` where `F` is a property whose type is a one-dimensional array).</span></span> <span data-ttu-id="b0f1b-708">In allen mehrdeutigen Situationen wird der Name in die-Eigenschaft und nicht in die lokale Variable aufgelöst.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-708">In all ambiguous situations, the name resolves to the property rather than the local variable.</span></span> <span data-ttu-id="b0f1b-709">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-709">For example:</span></span>

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

<span data-ttu-id="b0f1b-710">Wenn die Ablauf Steuerung den `Get`-Accessor-Text verlässt, wird der Wert der lokalen Variablen an den Aufruf Ausdruck zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-710">When control flow leaves the `Get` accessor body, the value of the local variable is passed back to the invocation expression.</span></span> <span data-ttu-id="b0f1b-711">Da das Aufrufen eines `Get`-Accessoren konzeptionell Äquivalent zum Lesen des Werts einer Variablen ist, wird er als ungültiges Programmier Format angesehen, damit `Get`-Accessoren Observable-Nebeneffekte haben, wie im folgenden Beispiel veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-711">Because invoking a `Get` accessor is conceptually equivalent to reading the value of a variable, it is considered bad programming style for `Get` accessors to have observable side effects, as illustrated in the following example:</span></span>

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

<span data-ttu-id="b0f1b-712">Der Wert der `NextValue`-Eigenschaft hängt von der Häufigkeit ab, mit der zuvor auf die Eigenschaft zugegriffen wurde.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-712">The value of the `NextValue` property depends on the number of times the property has previously been accessed.</span></span> <span data-ttu-id="b0f1b-713">Folglich erzeugt der Zugriff auf die-Eigenschaft einen beobachtbaren Nebeneffekt, und die-Eigenschaft sollte stattdessen als-Methode implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-713">Thus, accessing the property produces an observable side effect, and the property should instead be implemented as a method.</span></span>

<span data-ttu-id="b0f1b-714">Die "No Side Effects"-Konvention für `Get`-Accessoren bedeutet nicht, dass `Get`-Accessoren immer geschrieben werden sollten, um nur in Variablen gespeicherte Werte zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-714">The "no side effects" convention for `Get` accessors does not mean that `Get` accessors should always be written to simply return values stored in variables.</span></span> <span data-ttu-id="b0f1b-715">In der Tat berechnen `Get`-Accessoren häufig den Wert einer Eigenschaft, indem Sie auf mehrere Variablen zugreifen oder Methoden aufrufen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-715">Indeed, `Get` accessors often compute the value of a property by accessing multiple variables or invoking methods.</span></span> <span data-ttu-id="b0f1b-716">Ein ordnungsgemäß entworfener `Get`-Accessor führt jedoch keine Aktionen aus, die Observable-Änderungen im Status des Objekts bewirken.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-716">However, a properly designed `Get` accessor performs no actions that cause observable changes in the state of the object.</span></span>

<span data-ttu-id="b0f1b-717">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="b0f1b-717">__Note.__</span></span> <span data-ttu-id="b0f1b-718">`Get`-Accessoren haben bei der Zeilen Platzierung die gleiche Einschränkung wie bei den Unterroutinen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-718">`Get` accessors have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="b0f1b-719">Die beginnende Anweisung, End-Anweisung und Block müssen alle am Anfang einer logischen Zeile angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-719">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

```antlr
PropertyGetDeclaration
    : Attributes? AccessModifier? 'Get' LineTerminator
      Block?
      'End' 'Get' StatementTerminator
    ;
```

### <a name="set-accessor-declarations"></a><span data-ttu-id="b0f1b-720">Set-Accessor-Deklarationen</span><span class="sxs-lookup"><span data-stu-id="b0f1b-720">Set Accessor Declarations</span></span>

<span data-ttu-id="b0f1b-721">Ein `Set`-Accessor (Setter) wird mithilfe einer Eigenschaften Satz Deklaration deklariert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-721">A `Set` accessor (setter) is declared by using a property set declaration.</span></span> <span data-ttu-id="b0f1b-722">Eine Eigenschafts Satz Deklaration besteht aus dem Schlüsselwort `Set`, einer optionalen Parameterliste und einem Anweisungsblock.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-722">A property set declaration consists of the keyword `Set`, an optional parameter list, and a statement block.</span></span> <span data-ttu-id="b0f1b-723">Wenn eine Eigenschaft mit dem Namen "`P`" angegeben ist, deklariert eine Setter-Deklaration implizit eine Methode mit dem Namen "`set_P`" mit denselben Modifizierers und Parameterlisten wie die-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-723">Given a property named `P`, a setter declaration implicitly declares a method with the name `set_P` with the same modifiers and parameter list as the property.</span></span> <span data-ttu-id="b0f1b-724">Wenn der Typ eine Deklaration mit diesem Namen enthält, wird ein Fehler bei der Kompilierzeit ausgegeben, die implizite Deklaration wird jedoch zum Zweck der namens Bindung ignoriert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-724">If the type contains a declaration with that name, a compile-time error results, but the implicit declaration is ignored for the purposes of name binding.</span></span>

<span data-ttu-id="b0f1b-725">Wenn eine Parameterliste angegeben ist, muss Sie über einen Member verfügen, dieser Member muss über keine Modifizierer verfügen, außer `ByVal`, und sein Typ muss mit dem Typ der Eigenschaft identisch sein.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-725">If a parameter list is specified, it must have one member, that member must have no modifiers except `ByVal`, and its type must be the same as the type of the property.</span></span> <span data-ttu-id="b0f1b-726">Der-Parameter stellt den festzulegenden Eigenschafts Wert dar.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-726">The parameter represents the property value being set.</span></span> <span data-ttu-id="b0f1b-727">Wenn der-Parameter ausgelassen wird, wird ein Parameter mit dem Namen "`Value`" implizit deklariert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-727">If the parameter is omitted, a parameter named `Value` is implicitly declared.</span></span>

<span data-ttu-id="b0f1b-728">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="b0f1b-728">__Note.__</span></span> <span data-ttu-id="b0f1b-729">`Set`-Accessoren haben bei der Zeilen Platzierung die gleiche Einschränkung wie bei den Unterroutinen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-729">`Set` accessors have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="b0f1b-730">Die beginnende Anweisung, End-Anweisung und Block müssen alle am Anfang einer logischen Zeile angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-730">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

```antlr
PropertySetDeclaration
    : Attributes? AccessModifier? 'Set'
      ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block?
      'End' 'Set' StatementTerminator
    ;
```

### <a name="default-properties"></a><span data-ttu-id="b0f1b-731">Standardeigenschaften</span><span class="sxs-lookup"><span data-stu-id="b0f1b-731">Default Properties</span></span>

<span data-ttu-id="b0f1b-732">Eine-Eigenschaft, die den-Modifizierer angibt, `Default` als *Standard Eigenschaft*bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-732">A property that specifies the modifier `Default` is called a *default property*.</span></span> <span data-ttu-id="b0f1b-733">Jeder Typ, der Eigenschaften zulässt, kann über eine Default-Eigenschaft verfügen, einschließlich Schnittstellen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-733">Any type that allows properties may have a default property, including interfaces.</span></span> <span data-ttu-id="b0f1b-734">Auf die Default-Eigenschaft kann verwiesen werden, ohne dass die Instanz mit dem Namen der Eigenschaft qualifiziert werden muss.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-734">The default property may be referenced without having to qualify the instance with the name of the property.</span></span> <span data-ttu-id="b0f1b-735">Folglich, wenn eine Klasse</span><span class="sxs-lookup"><span data-stu-id="b0f1b-735">Thus, given a class</span></span>

```vb
Class Test
    Public Default ReadOnly Property Item(i As Integer) As Integer
        Get
            Return i
        End Get
    End Property
End Class
```

<span data-ttu-id="b0f1b-736">Der Code</span><span class="sxs-lookup"><span data-stu-id="b0f1b-736">the code</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test()
        Dim y As Integer

        y = x(10)
    End Sub
End Module
```

<span data-ttu-id="b0f1b-737">für die folgende Syntax:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-737">is equivalent to</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test()
        Dim y As Integer

        y = x.Item(10)
    End Sub
End Module
```

<span data-ttu-id="b0f1b-738">Nachdem eine Eigenschaft `Default` deklariert wurde, werden alle Eigenschaften, die für diesen Namen in der Vererbungs Hierarchie überladen werden, als Standard Eigenschaft fest, unabhängig davon, ob Sie als `Default` deklariert wurden oder nicht.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-738">Once a property is declared `Default`, all of the properties overloaded on that name in the inheritance hierarchy become the default property, whether they have been declared `Default` or not.</span></span> <span data-ttu-id="b0f1b-739">Das Deklarieren einer Eigenschaft `Default` in einer abgeleiteten Klasse, wenn die Basisklasse, die eine Standard Eigenschaft mit einem anderen Namen deklariert hat, keine weiteren Modifizierer wie `Shadows` oder `Overrides` erfordert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-739">Declaring a property `Default` in a derived class when the base class declared a default property by another name does not require any other modifiers such as `Shadows` or `Overrides`.</span></span> <span data-ttu-id="b0f1b-740">Dies liegt daran, dass die Standard Eigenschaft keine Identität oder Signatur aufweist und daher nicht schattiert oder überladen werden kann.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-740">This is because the default property has no identity or signature and so cannot be shadowed or overloaded.</span></span> <span data-ttu-id="b0f1b-741">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-741">For example:</span></span>

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

<span data-ttu-id="b0f1b-742">Dieses Programm erzeugt die Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-742">This program will produce the output:</span></span>

```console
MoreDerived = 10
Derived = 10
Base = 10
```

<span data-ttu-id="b0f1b-743">Alle Standardeigenschaften, die innerhalb eines Typs deklariert werden, müssen denselben Namen aufweisen, und aus Gründen der Übersichtlichkeit muss den `Default`-Modifizierer angeben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-743">All default properties declared within a type must have the same name and, for clarity, must specify the `Default` modifier.</span></span> <span data-ttu-id="b0f1b-744">Da eine Standard Eigenschaft ohne Index Parameter eine mehrdeutige Situation verursachen würde, wenn Instanzen der enthaltenden Klasse zugewiesen werden, müssen die Standardeigenschaften über Index Parameter verfügen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-744">Because a default property with no index parameters would cause an ambiguous situation when assigning instances of the containing class, default properties must have index parameters.</span></span> <span data-ttu-id="b0f1b-745">Wenn eine Eigenschaft, die mit einem bestimmten Namen überladen wird, den `Default`-Modifizierer enthält, müssen alle Eigenschaften, die für diesen Namen überladen werden, Sie angeben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-745">Furthermore, if one property overloaded on a particular name includes the `Default` modifier, all properties overloaded on that name must specify it.</span></span> <span data-ttu-id="b0f1b-746">Standardeigenschaften dürfen nicht `Shared` sein, und mindestens ein Accessor der Eigenschaft darf nicht `Private` sein.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-746">Default properties may not be `Shared`, and at least one accessor of the property must not be `Private`.</span></span>

### <a name="automatically-implemented-properties"></a><span data-ttu-id="b0f1b-747">Automatisch implementierte Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="b0f1b-747">Automatically Implemented Properties</span></span>

<span data-ttu-id="b0f1b-748">Wenn eine Eigenschaft die Deklaration eines Accessors auslässt, wird automatisch eine Implementierung der-Eigenschaft bereitgestellt, es sei denn, die Eigenschaft wird in einer Schnittstelle deklariert oder als `MustOverride` deklariert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-748">If a property omits declaration of any accessors, an implementation of the property will be automatically supplied unless the property is declared in an interface or is declared `MustOverride`.</span></span> <span data-ttu-id="b0f1b-749">Nur Lese-/Schreibeigenschaften ohne Argumente können automatisch implementiert werden. Andernfalls tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-749">Only read/write properties with no arguments can be automatically implemented; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="b0f1b-750">Eine automatisch implementierte Eigenschaft `x`, auch wenn Sie eine andere Eigenschaft überschreibt, führt eine private lokale Variable ein `_x` mit dem gleichen Typ wie die Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-750">An automatically implemented property `x`, even one overriding another property, introduces a private local variable `_x` with the same type as the property.</span></span> <span data-ttu-id="b0f1b-751">Wenn eine Kollision zwischen dem Namen der lokalen Variablen und einer anderen Deklaration vorliegt, wird ein Kompilierzeitfehler gemeldet.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-751">If there is a collision between the local variable's name and another declaration, a compile-time error will be reported.</span></span> <span data-ttu-id="b0f1b-752">Der `Get`-Accessor der automatisch implementierten Eigenschaft gibt den Wert des lokalen und den `Set`-Accessor der Eigenschaft zurück, der den Wert des lokalen festlegt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-752">The automatically implemented property's `Get` accessor returns the value of the local and the property's `Set` accessor that sets the value of the local.</span></span> <span data-ttu-id="b0f1b-753">Beispielsweise ist die Deklaration:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-753">For example, the declaration:</span></span>

```vb
Public Property x() As Integer
```

<span data-ttu-id="b0f1b-754">ist ungefähr Äquivalent zu:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-754">is roughly equivalent to:</span></span>

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

<span data-ttu-id="b0f1b-755">Wie bei Variablen Deklarationen kann eine automatisch implementierte Eigenschaft einen Initialisierer enthalten.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-755">As with variable declarations, an automatically implemented property can include an initializer.</span></span> <span data-ttu-id="b0f1b-756">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-756">For example:</span></span>

```vb
Public Property x() As Integer = 10
Public Shared Property y() As New Customer() With { .Name = "Bob" }
```

<span data-ttu-id="b0f1b-757">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="b0f1b-757">__Note.__</span></span> <span data-ttu-id="b0f1b-758">Wenn eine automatisch implementierte Eigenschaft initialisiert wird, wird Sie über die-Eigenschaft und nicht über das zugrunde liegende Feld initialisiert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-758">When an automatically implemented property is initialized, it is initialized through the property, not the underlying field.</span></span> <span data-ttu-id="b0f1b-759">Das heißt, dass über schreibende Eigenschaften die Initialisierung bei Bedarf abfangen können.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-759">This is so overriding properties can intercept the initialization if they need to.</span></span>

<span data-ttu-id="b0f1b-760">Arrayinitialisierer sind für automatisch implementierte Eigenschaften zulässig, mit dem Unterschied, dass es keine Möglichkeit gibt, die Array Begrenzungen explizit anzugeben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-760">Array initializers are allowed on automatically implemented properties, except that there is no way to specify the array bounds explicitly.</span></span>  <span data-ttu-id="b0f1b-761">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-761">For example:</span></span>

```vb
' Valid
Property x As Integer() = {1, 2, 3}
Property y As Integer(,) = {{1, 2, 3}, {12, 13, 14}, {11, 10, 9}}

' Invalid
Property x4(5) As Short
```

### <a name="iterator-properties"></a><span data-ttu-id="b0f1b-762">Iteratoreigenschaften</span><span class="sxs-lookup"><span data-stu-id="b0f1b-762">Iterator Properties</span></span>

<span data-ttu-id="b0f1b-763">Eine *iteratoreigenschaft* ist eine Eigenschaft mit dem `Iterator`-Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-763">An *iterator property* is a property with the `Iterator` modifier.</span></span> <span data-ttu-id="b0f1b-764">Sie wird aus demselben Grund verwendet, aus dem eine Iteratormethode (Abschnitts [Iteratormethoden](statements.md#iterator-methods)) verwendet wird. Dies ist eine bequeme Methode zum Generieren einer Sequenz, die von der `For Each`-Anweisung verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-764">It is used for the same reason an iterator method (Section [Iterator Methods](statements.md#iterator-methods)) is used -- as a convenient way to generate a sequence, one which can be consumed by the `For Each` statement.</span></span> <span data-ttu-id="b0f1b-765">Der `Get`-Accessor einer iteratoreigenschaft wird auf die gleiche Weise interpretiert wie eine Iteratormethode.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-765">The `Get` accessor of an iterator property is interpreted in the same way as an iterator method.</span></span>

<span data-ttu-id="b0f1b-766">Eine iteratoreigenschaft muss über einen expliziten `Get`-Accessor verfügen, und ihr Typ muss `IEnumerator` oder `IEnumerable` oder `IEnumerator(Of T)` oder `IEnumerable(Of T)` für einige `T` sein.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-766">An iterator property must have an explicit `Get` accessor, and its type must be `IEnumerator`, or `IEnumerable`, or `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`.</span></span>

<span data-ttu-id="b0f1b-767">Im folgenden finden Sie ein Beispiel für eine iteratoreigenschaft:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-767">Here is an example of an iterator property:</span></span>

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

## <a name="operators"></a><span data-ttu-id="b0f1b-768">Operatoren</span><span class="sxs-lookup"><span data-stu-id="b0f1b-768">Operators</span></span>

<span data-ttu-id="b0f1b-769">*Operatoren* sind Methoden, die die Bedeutung eines vorhandenen Visual Basic Operators für die enthaltende Klasse definieren.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-769">*Operators* are methods that define the meaning of an existing Visual Basic operator for the containing class.</span></span> <span data-ttu-id="b0f1b-770">Wenn der-Operator in einem Ausdruck auf die-Klasse angewendet wird, wird der-Operator in einen aufzurufenden Operator in der-Klasse kompiliert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-770">When the operator is applied to the class in an expression, the operator is compiled into a call to the operator method defined in the class.</span></span> <span data-ttu-id="b0f1b-771">Die Definition eines Operators für eine Klasse wird auch als *überladen* des Operators bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-771">Defining an operator for a class is also known as *overloading* the operator.</span></span>

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

<span data-ttu-id="b0f1b-772">Es ist nicht möglich, einen Operator zu überladen, der bereits vorhanden ist. in der Praxis gilt dies hauptsächlich für Konvertierungs Operatoren.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-772">It is not possible to overload an operator that already exists; in practice, this primarily applies to conversion operators.</span></span> <span data-ttu-id="b0f1b-773">Beispielsweise ist es nicht möglich, die Konvertierung von einer abgeleiteten Klasse in eine Basisklasse zu überladen:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-773">For example, it is not possible to overload the conversion from a derived class to a base class:</span></span>

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

<span data-ttu-id="b0f1b-774">Operatoren können auch im allgemeinen Sinn des Worts überladen werden:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-774">Operators can also be overloaded in the common sense of the word:</span></span>

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

<span data-ttu-id="b0f1b-775">Operator Deklarationen fügen Namen nicht explizit dem Deklarations Bereich des enthaltenden Typs hinzu. Allerdings deklarieren Sie implizit eine entsprechende Methode, beginnend mit den Zeichen "op_".</span><span class="sxs-lookup"><span data-stu-id="b0f1b-775">Operator declarations do not explicitly add names to the containing type's declaration space; however they do implicitly declare a corresponding method starting with the characters "op_".</span></span> <span data-ttu-id="b0f1b-776">In den folgenden Abschnitten werden die entsprechenden Methodennamen mit den einzelnen Operatoren aufgelistet.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-776">The following sections list the corresponding method names with each operator.</span></span>

<span data-ttu-id="b0f1b-777">Es gibt drei Klassen von Operatoren, die definiert werden können: unäre Operatoren, binäre Operatoren und Konvertierungs Operatoren.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-777">There are three classes of operators that can be defined: unary operators, binary operators and conversion operators.</span></span> <span data-ttu-id="b0f1b-778">Alle Operator Deklarationen haben bestimmte Einschränkungen gemeinsam:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-778">All operator declarations share certain restrictions:</span></span>

* <span data-ttu-id="b0f1b-779">Operator Deklarationen müssen immer `Public` und `Shared` sein.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-779">Operator declarations must always be `Public` and `Shared`.</span></span> <span data-ttu-id="b0f1b-780">Der `Public`-Modifizierer kann in Kontexten ausgelassen werden, in denen der-Modifizierer angenommen wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-780">The `Public` modifier can be omitted in contexts where the modifier will be assumed.</span></span>

* <span data-ttu-id="b0f1b-781">Die Parameter eines Operators können nicht `ByRef`, `Optional` oder `ParamArray` deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-781">The parameters of an operator cannot be declared `ByRef`, `Optional` or `ParamArray`.</span></span>

* <span data-ttu-id="b0f1b-782">Der Typ mindestens eines der Operanden oder der Rückgabewert muss der Typ sein, der den Operator enthält.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-782">The type of at least one of the operands or the return value must be the type that contains the operator.</span></span>

* <span data-ttu-id="b0f1b-783">Für Operatoren ist keine Funktions Rückgabe Variable definiert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-783">There is no function return variable defined for operators.</span></span> <span data-ttu-id="b0f1b-784">Daher muss die `Return`-Anweisung verwendet werden, um Werte aus einem Operator Text zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-784">Therefore, the `Return` statement must be used to return values from an operator body.</span></span>

<span data-ttu-id="b0f1b-785">Die einzige Ausnahme dieser Einschränkungen gilt für Werttypen, die auf NULL festgelegt werden können.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-785">The only exception to these restrictions applies to nullable value types.</span></span> <span data-ttu-id="b0f1b-786">Da auf NULL festleg Bare Werttypen keine tatsächliche Typdefinition aufweisen, kann ein Werttyp benutzerdefinierte Operatoren für die Werte zulässt-Version des Typs deklarieren.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-786">Since nullable value types do not have an actual type definition, a value type can declare user-defined operators for the nullable version of the type.</span></span> <span data-ttu-id="b0f1b-787">Wenn Sie bestimmen, ob ein Typ einen bestimmten benutzerdefinierten Operator deklarieren kann, werden die `?`-Modifizierer zuerst aus allen Typen gelöscht, die an der Deklaration für die Gültigkeits Überprüfung beteiligt sind.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-787">When determining whether a type can declare a particular user-defined operator, the `?` modifiers are first dropped off of all of the types involved in the declaration for the purposes of validity checking.</span></span> <span data-ttu-id="b0f1b-788">Diese Lockerung gilt nicht für den Rückgabetyp der Operatoren "`IsTrue`" und "`IsFalse`". Sie müssen weiterhin `Boolean`, nicht jedoch `Boolean?` zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-788">This relaxation does not apply to the return type of the `IsTrue` and `IsFalse` operators; they must still return `Boolean`, not `Boolean?`.</span></span>

<span data-ttu-id="b0f1b-789">Die Rangfolge und Assoziativität eines Operators können nicht durch eine Operator Deklaration geändert werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-789">The precedence and associativity of an operator cannot be modified by an operator declaration.</span></span>

<span data-ttu-id="b0f1b-790">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="b0f1b-790">__Note.__</span></span> <span data-ttu-id="b0f1b-791">Operatoren haben bei der Zeilen Platzierung die gleiche Einschränkung wie bei den Unterroutinen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-791">Operators have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="b0f1b-792">Die beginnende Anweisung, End-Anweisung und Block müssen alle am Anfang einer logischen Zeile angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-792">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>


### <a name="unary-operators"></a><span data-ttu-id="b0f1b-793">Unäre Operatoren</span><span class="sxs-lookup"><span data-stu-id="b0f1b-793">Unary Operators</span></span>

<span data-ttu-id="b0f1b-794">Die folgenden unären Operatoren können überladen werden:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-794">The following unary operators can be overloaded:</span></span>

* <span data-ttu-id="b0f1b-795">Der unäre Plus-Operator `+` (entsprechende Methode: `op_UnaryPlus`)</span><span class="sxs-lookup"><span data-stu-id="b0f1b-795">The unary plus operator `+` (corresponding method: `op_UnaryPlus`)</span></span>

* <span data-ttu-id="b0f1b-796">Der unäre Minus Operator `-` (entsprechende Methode: `op_UnaryNegation`)</span><span class="sxs-lookup"><span data-stu-id="b0f1b-796">The unary minus operator `-` (corresponding method: `op_UnaryNegation`)</span></span>

* <span data-ttu-id="b0f1b-797">Der logische `Not`-Operator (entsprechende Methode: `op_OnesComplement`)</span><span class="sxs-lookup"><span data-stu-id="b0f1b-797">The logical `Not` operator (corresponding method: `op_OnesComplement`)</span></span>

* <span data-ttu-id="b0f1b-798">Die Operatoren "`IsTrue`" und "`IsFalse`" (entsprechende Methoden: `op_True`, `op_False`)</span><span class="sxs-lookup"><span data-stu-id="b0f1b-798">The `IsTrue` and `IsFalse` operators (corresponding methods: `op_True`, `op_False`)</span></span>

<span data-ttu-id="b0f1b-799">Alle überladenen unären Operatoren müssen einen einzelnen Parameter des enthaltenden Typs annehmen und können einen beliebigen Typ zurückgeben, mit Ausnahme von `IsTrue` und `IsFalse`, die `Boolean` zurückgeben müssen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-799">All overloaded unary operators must take a single parameter of the containing type and may return any type, except for `IsTrue` and `IsFalse`, which must return `Boolean`.</span></span> <span data-ttu-id="b0f1b-800">Wenn der enthaltende Typ ein generischer Typ ist, müssen die Typparameter den Typparametern des enthaltenden Typs entsprechen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-800">If the containing type is a generic type, the type parameters must match the containing type's type parameters.</span></span> <span data-ttu-id="b0f1b-801">Ein auf ein Objekt angewendeter</span><span class="sxs-lookup"><span data-stu-id="b0f1b-801">For example,</span></span>

```vb
Structure Complex
    ...

    Public Shared Operator +(v As Complex) As Complex
        Return v
    End Operator
End Structure
```

<span data-ttu-id="b0f1b-802">Wenn ein Typ einen der `IsTrue` oder `IsFalse` über lädt, muss er auch die andere überladen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-802">If a type overloads one of `IsTrue` or `IsFalse`, then it must overload the other as well.</span></span> <span data-ttu-id="b0f1b-803">Wenn nur eine überladen ist, ergibt sich ein Kompilierzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-803">If only one is overloaded, a compile-time error results.</span></span>

<span data-ttu-id="b0f1b-804">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="b0f1b-804">__Note.__</span></span> <span data-ttu-id="b0f1b-805">`IsTrue` und `IsFalse` sind keine reservierten Wörter.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-805">`IsTrue` and `IsFalse` are not reserved words.</span></span>

### <a name="binary-operators"></a><span data-ttu-id="b0f1b-806">Binäre Operatoren</span><span class="sxs-lookup"><span data-stu-id="b0f1b-806">Binary Operators</span></span>

<span data-ttu-id="b0f1b-807">Die folgenden binären Operatoren können überladen werden:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-807">The following binary operators can be overloaded:</span></span>

* <span data-ttu-id="b0f1b-808">Die Addition `+`, Subtraktion `-`, Multiplikation `*`, Division `/`, ganzzahlige Division `\`, Modulo `Mod`-und exponentiations-`^`-Operatoren (entsprechende Methode: `op_Addition`, `op_Subtraction`, `op_Multiply`, 0, 1, @no_ _T-12, 3)</span><span class="sxs-lookup"><span data-stu-id="b0f1b-808">The addition `+`, subtraction `-`, multiplication `*`, division `/`, integral division `\`, modulo `Mod` and exponentiation `^` operators (corresponding method: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_IntegerDivision`, `op_Modulus`, `op_Exponent`)</span></span>

* <span data-ttu-id="b0f1b-809">Die relationalen Operatoren `=`, `<>`, `<`, `>`, `<=`, `>=` (entsprechende Methoden: `op_Equality`, `op_Inequality`, `op_LessThan`, `op_GreaterThan`, 0, 1).</span><span class="sxs-lookup"><span data-stu-id="b0f1b-809">The relational operators `=`, `<>`, `<`, `>`, `<=`, `>=` (corresponding methods: `op_Equality`, `op_Inequality`, `op_LessThan`, `op_GreaterThan`, `op_LessThanOrEqual`, `op_GreaterThanOrEqual`).</span></span> <span data-ttu-id="b0f1b-810">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="b0f1b-810">__Note.__</span></span> <span data-ttu-id="b0f1b-811">Der Gleichheits Operator kann überladen werden, aber der Zuweisungs Operator (nur in Zuweisungs Anweisungen verwendet) kann nicht überladen werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-811">While the equality operator can be overloaded, the assignment operator (used only in assignment statements) cannot be overloaded.</span></span>

* <span data-ttu-id="b0f1b-812">Der `Like`-Operator (entsprechende Methode: `op_Like`)</span><span class="sxs-lookup"><span data-stu-id="b0f1b-812">The `Like` operator (corresponding method: `op_Like`)</span></span>

* <span data-ttu-id="b0f1b-813">Der Verkettungs Operator `&` (entsprechende Methode: `op_Concatenate`).</span><span class="sxs-lookup"><span data-stu-id="b0f1b-813">The concatenation operator `&` (corresponding method: `op_Concatenate`)</span></span>

* <span data-ttu-id="b0f1b-814">Die logischen Operatoren "`And`", "`Or`" und "`Xor`" (entsprechende Methoden: `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`)</span><span class="sxs-lookup"><span data-stu-id="b0f1b-814">The logical `And`, `Or` and `Xor` operators (corresponding methods: `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`)</span></span>

* <span data-ttu-id="b0f1b-815">Die Shift-Operatoren `<<` und `>>` (entsprechende Methoden: `op_LeftShift`, `op_RightShift`)</span><span class="sxs-lookup"><span data-stu-id="b0f1b-815">The shift operators `<<` and `>>` (corresponding methods: `op_LeftShift`, `op_RightShift`)</span></span>

<span data-ttu-id="b0f1b-816">Alle überladenen binären Operatoren müssen den enthaltenden Typ als einen der Parameter verwenden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-816">All overloaded binary operators must take the containing type as one of the parameters.</span></span> <span data-ttu-id="b0f1b-817">Wenn der enthaltende Typ ein generischer Typ ist, müssen die Typparameter den Typparametern des enthaltenden Typs entsprechen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-817">If the containing type is a generic type, the type parameters must match the containing type's type parameters.</span></span> <span data-ttu-id="b0f1b-818">Die Shift-Operatoren schränken diese Regel weiter ein, sodass der erste Parameter vom enthaltenden Typ sein muss. der zweite Parameter muss immer den Typ `Integer` aufweisen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-818">The shift operators further restrict this rule to require the first parameter to be of the containing type; the second parameter must always be of type `Integer`.</span></span>

<span data-ttu-id="b0f1b-819">Die folgenden binären Operatoren müssen paarweise deklariert werden:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-819">The following binary operators must be declared in pairs:</span></span>

* <span data-ttu-id="b0f1b-820">Operator `=` und Operator `<>`</span><span class="sxs-lookup"><span data-stu-id="b0f1b-820">Operator `=` and operator `<>`</span></span>

* <span data-ttu-id="b0f1b-821">Operator `>` und Operator `<`</span><span class="sxs-lookup"><span data-stu-id="b0f1b-821">Operator `>` and operator `<`</span></span>

* <span data-ttu-id="b0f1b-822">Operator `>=` und Operator `<=`</span><span class="sxs-lookup"><span data-stu-id="b0f1b-822">Operator `>=` and operator `<=`</span></span>

<span data-ttu-id="b0f1b-823">Wenn eines der Paare deklariert ist, muss das andere ebenfalls mit übereinstimmenden Parameter-und Rückgabe Typen deklariert werden, andernfalls tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-823">If one of the pair is declared, then the other must also be declared with matching parameter and return types, or a compile-time error will result.</span></span> <span data-ttu-id="b0f1b-824">(__Hinweis:__</span><span class="sxs-lookup"><span data-stu-id="b0f1b-824">(__Note.__</span></span> <span data-ttu-id="b0f1b-825">Der Zweck der Verwendung von paarweise zugeordneten Deklarationen relationaler Operatoren besteht darin, zumindest einen minimalen Grad an logischer Konsistenz in überladenen Operatoren sicherzustellen.)</span><span class="sxs-lookup"><span data-stu-id="b0f1b-825">The purpose of requiring paired declarations of relational operators is to try and ensure at least a minimum level of logical consistency in overloaded operators.)</span></span>

<span data-ttu-id="b0f1b-826">Im Gegensatz zu den relationalen Operatoren wird das Überladen von Operatoren der Division und der ganzzahligen Division dringend davon abgeraten, auch wenn kein Fehler vorliegt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-826">In contrast to the relational operators, overloading both the division and integral division operators is strongly discouraged, although not an error.</span></span> <span data-ttu-id="b0f1b-827">(__Hinweis:__</span><span class="sxs-lookup"><span data-stu-id="b0f1b-827">(__Note.__</span></span> <span data-ttu-id="b0f1b-828">Im Allgemeinen sollten die beiden Arten der Division ganz eindeutig sein: ein Typ, der die Division unterstützt, ist entweder eine Ganzzahl (in diesem Fall sollte `\`) oder nicht (in diesem Fall sollte `/` unterstützt werden).</span><span class="sxs-lookup"><span data-stu-id="b0f1b-828">In general, the two types of division should be entirely distinct: a type that supports division is either integral (in which case it should support `\`) or not (in which case it should support `/`).</span></span> <span data-ttu-id="b0f1b-829">Wir haben uns als Fehler beim Definieren beider Operatoren bewährt, aber da ihre Sprachen in der Regel nicht zwischen zwei Arten von Abteilungen unterscheiden, wie Visual Basic, haben wir gespürt, dass es am sichersten ist, die Übung zuzulassen, aber dringend abzuraten.)</span><span class="sxs-lookup"><span data-stu-id="b0f1b-829">We considered making it an error to define both operators, but because their languages do not generally distinguish between two types of division the way Visual Basic does, we felt it was safest to allow the practice but strongly discourage it.)</span></span>

<span data-ttu-id="b0f1b-830">Verbund Zuweisungs Operatoren können nicht direkt überladen werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-830">Compound assignment operators cannot be overloaded directly.</span></span> <span data-ttu-id="b0f1b-831">Stattdessen verwendet der Verbund Zuweisungs Operator den überladenen Operator, wenn der entsprechende binäre Operator überladen wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-831">Instead, when the corresponding binary operator is overloaded, the compound assignment operator will use the overloaded operator.</span></span> <span data-ttu-id="b0f1b-832">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-832">For example:</span></span>

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

### <a name="conversion-operators"></a><span data-ttu-id="b0f1b-833">Konvertierungsoperatoren</span><span class="sxs-lookup"><span data-stu-id="b0f1b-833">Conversion Operators</span></span>

<span data-ttu-id="b0f1b-834">Konvertierungs Operatoren definieren neue Konvertierungen zwischen Typen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-834">Conversion operators define new conversions between types.</span></span> <span data-ttu-id="b0f1b-835">Diese neuen Konvertierungen werden als *benutzerdefinierte Konvertierungen*bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-835">These new conversions are called *user-defined conversions*.</span></span> <span data-ttu-id="b0f1b-836">Ein Konvertierungs Operator konvertiert von einem Quelltyp, der durch den Parametertyp des Konvertierungs Operators angegeben ist, in einen Zieltyp, der durch den Rückgabetyp des Konvertierungs Operators angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-836">A conversion operator converts from a source type, indicated by the parameter type of the conversion operator, to a target type, indicated by the return type of the conversion operator.</span></span> <span data-ttu-id="b0f1b-837">Konvertierungen müssen entweder erweitert oder einschränkend klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-837">Conversions must be classified as either widening or narrowing.</span></span> <span data-ttu-id="b0f1b-838">Eine Konvertierungs Operator Deklaration, die das Schlüsselwort "`Widening`" enthält, führt eine benutzerdefinierte erweiternde Konvertierung ein (entsprechende Methode: `op_Implicit`).</span><span class="sxs-lookup"><span data-stu-id="b0f1b-838">A conversion operator declaration that includes the `Widening` keyword introduces a user-defined widening conversion (corresponding method: `op_Implicit`).</span></span> <span data-ttu-id="b0f1b-839">Eine Konvertierungs Operator Deklaration, die das Schlüsselwort "`Narrowing`" enthält, führt eine benutzerdefinierte einschränkende Konvertierung ein (entsprechende Methode: `op_Explicit`).</span><span class="sxs-lookup"><span data-stu-id="b0f1b-839">A conversion operator declaration that includes the `Narrowing` keyword introduces a user-defined narrowing conversion (corresponding method: `op_Explicit`).</span></span>

<span data-ttu-id="b0f1b-840">Im Allgemeinen sollten benutzerdefinierte erweiternde Konvertierungen so entworfen werden, dass nie Ausnahmen ausgelöst werden, und es werden niemals Informationen verloren.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-840">In general, user-defined widening conversions should be designed to never throw exceptions and never lose information.</span></span> <span data-ttu-id="b0f1b-841">Wenn eine benutzerdefinierte Konvertierung Ausnahmen verursachen kann (z. b. weil das Quell Argument außerhalb des gültigen Bereichs liegt) oder Informationen verloren geht (z. b. das Verwerfen von großen Bits), sollte diese Konvertierung als einschränkende Konvertierung definiert werden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-841">If a user-defined conversion can cause exceptions (for example, because the source argument is out of range) or loss of information (such as discarding high-order bits), then that conversion should be defined as a narrowing conversion.</span></span> <span data-ttu-id="b0f1b-842">Im folgenden Beispiel</span><span class="sxs-lookup"><span data-stu-id="b0f1b-842">In the example:</span></span>

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

<span data-ttu-id="b0f1b-843">die Konvertierung von `Digit` in `Byte` ist eine erweiternde Konvertierung, da Sie nie Ausnahmen auslöst oder Informationen verliert, aber die Konvertierung von `Byte` in `Digit` ist eine einschränkende Konvertierung, da `Digit` nur eine Teilmenge der möglichen Werte von darstellen kann. `Byte`.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-843">the conversion from `Digit` to `Byte` is a widening conversion because it never throws exceptions or loses information, but the conversion from `Byte` to `Digit` is a narrowing conversion since `Digit` can only represent a subset of the possible values of a `Byte`.</span></span>

<span data-ttu-id="b0f1b-844">Im Gegensatz zu allen anderen Typmembern, die überladen werden können, enthält die Signatur eines Konvertierungs Operators den Zieltyp der Konvertierung.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-844">Unlike all other type members that can be overloaded, the signature of a conversion operator includes the target type of the conversion.</span></span> <span data-ttu-id="b0f1b-845">Dies ist der einzige Typmember, für den der Rückgabetyp an der Signatur teilnimmt.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-845">This is the only type member for which the return type participates in the signature.</span></span> <span data-ttu-id="b0f1b-846">Die erweiternde oder einschränkende Klassifizierung eines Konvertierungs Operators ist jedoch nicht Teil der Signatur des Operators.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-846">The widening or narrowing classification of a conversion operator, however, is not part of the operator's signature.</span></span> <span data-ttu-id="b0f1b-847">Daher kann eine Klasse oder Struktur nicht sowohl einen erweiterungskonvertierungs Operator als auch einen einschränkenden Konvertierungs Operator mit denselben Quell-und Zieltyp deklarieren.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-847">Thus, a class or structure cannot declare both a widening conversion operator and a narrowing conversion operator with the same source and target types.</span></span>

<span data-ttu-id="b0f1b-848">Ein benutzerdefinierter Konvertierungs Operator muss entweder in oder aus dem enthaltenden Typ konvertieren. es ist z. b. möglich, dass eine Klasse `C` eine Konvertierung von `C` in `Integer` und von `Integer` in `C`, aber nicht von `Integer` in `Boolean` definiert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-848">A user-defined conversion operator must convert either to or from the containing type -- for example, it is possible for a class `C` to define a conversion from `C` to `Integer` and from `Integer` to `C`, but not from `Integer` to `Boolean`.</span></span> <span data-ttu-id="b0f1b-849">Wenn der enthaltende Typ ein generischer Typ ist, müssen die Typparameter den Typparametern des enthaltenden Typs entsprechen.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-849">If the containing type is a generic type, the type parameters must match the containing type's type parameters.</span></span> <span data-ttu-id="b0f1b-850">Außerdem ist es nicht möglich, eine intrinsische (d. h. Nichtbenutzer definierte) Konvertierung neu zu definieren.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-850">Also, it is not possible to redefine an intrinsic (i.e. non-user-defined) conversion.</span></span> <span data-ttu-id="b0f1b-851">Folglich kann ein Typ keine Konvertierung deklarieren, wobei Folgendes gilt:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-851">As a result, a type cannot declare a conversion where:</span></span>

* <span data-ttu-id="b0f1b-852">Der Quelltyp und der Zieltyp sind identisch.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-852">The source type and the destination type are the same.</span></span>

* <span data-ttu-id="b0f1b-853">Sowohl der Quelltyp als auch der Zieltyp sind nicht der Typ, der den Konvertierungs Operator definiert.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-853">Both the source type and the destination type are not the type that defines the conversion operator.</span></span>

* <span data-ttu-id="b0f1b-854">Der Quelltyp oder der Zieltyp ist ein Schnittstellentyp.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-854">The source type or the destination type is an interface type.</span></span>

* <span data-ttu-id="b0f1b-855">Der Quelltyp und die Zieltypen sind durch Vererbung verknüpft (einschließlich `Object`).</span><span class="sxs-lookup"><span data-stu-id="b0f1b-855">The source type and destination types are related by inheritance (including `Object`).</span></span>

<span data-ttu-id="b0f1b-856">Die einzige Ausnahme dieser Regeln gilt für Werttypen, die auf NULL festgelegt werden können.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-856">The only exception to these rules applies to nullable value types.</span></span> <span data-ttu-id="b0f1b-857">Da auf NULL festleg Bare Werttypen keine tatsächliche Typdefinition aufweisen, kann ein Werttyp benutzerdefinierte Konvertierungen für die Werte zulässt-Version des Typs deklarieren.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-857">Since nullable value types do not have an actual type definition, a value type can declare user-defined conversions for the nullable version of the type.</span></span> <span data-ttu-id="b0f1b-858">Wenn Sie bestimmen, ob ein Typ eine bestimmte benutzerdefinierte Konvertierung deklarieren kann, werden die `?`-Modifizierer zuerst aus allen Typen gelöscht, die an der Deklaration für die Gültigkeits Überprüfung beteiligt sind.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-858">When determining whether a type can declare a particular user-defined conversion, the `?` modifiers are first dropped off of all of the types involved in the declaration for the purposes of validity checking.</span></span> <span data-ttu-id="b0f1b-859">Daher ist die folgende Deklaration gültig, da `S` eine Konvertierung von `S` in `T` definieren kann:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-859">Thus, the following declaration is valid because `S` can define a conversion from `S` to `T`:</span></span>

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

<span data-ttu-id="b0f1b-860">Die folgende Deklaration ist jedoch nicht gültig, da die Struktur `S` keine Konvertierung von `S` in `S` definieren kann:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-860">The following declaration is not valid, however, because structure `S` cannot define a conversion from `S` to `S`:</span></span>

```vb
Structure S
    Public Shared Widening Operator CType(ByVal v As S) As S?
        ...
    End Operator
End Structure
```

### <a name="operator-mapping"></a><span data-ttu-id="b0f1b-861">Operator Zuordnung</span><span class="sxs-lookup"><span data-stu-id="b0f1b-861">Operator Mapping</span></span>

<span data-ttu-id="b0f1b-862">Da der Satz von Operatoren, der Visual Basic unterstützt, möglicherweise nicht genau mit dem Satz von Operatoren übereinstimmt, die andere Sprachen auf der .NET Framework, werden einige Operatoren bei der Definition oder Verwendung speziell anderen Operatoren zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-862">Because the set of operators that Visual Basic supports may not exactly match the set of operators that other languages on the .NET Framework, some operators are mapped specially onto other operators when being defined or used.</span></span> <span data-ttu-id="b0f1b-863">Dies gilt insbesondere in folgenden Fällen:</span><span class="sxs-lookup"><span data-stu-id="b0f1b-863">Specifically:</span></span>

* <span data-ttu-id="b0f1b-864">Wenn Sie einen integralen Divisions Operator definieren, wird automatisch ein regulärer Divisions Operator definiert (nur aus anderen Sprachen verwendbar), der den integralen Divisions Operator aufruft.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-864">Defining an integral division operator will automatically define a regular division operator (usable only from other languages) that will call the integral division operator.</span></span>

* <span data-ttu-id="b0f1b-865">Das Überladen der Operatoren "`Not`", "`And`" und "`Or`" überlastet nur den bitweisen Operator aus der Perspektive anderer Sprachen, die zwischen logischen und bitweisen Operatoren unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-865">Overloading the `Not`, `And`, and `Or` operators will overload only the bitwise operator from the perspective of other languages that distinguish between logical and bitwise operators.</span></span>

* <span data-ttu-id="b0f1b-866">Eine Klasse, die nur die logischen Operatoren in einer Sprache überlädt, die zwischen logischen und bitweisen Operatoren unterscheidet (d. h. eine Sprache, die `op_LogicalNot`, `op_LogicalAnd` und `op_LogicalOr` für `Not`, `And` bzw. `Or` verwendet), wird Ihr logisches Operatoren, die den Visual Basic logischen Operatoren zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-866">A class that overloads only the logical operators in a language that distinguishes between logical and bitwise operators (i.e. a languages that uses `op_LogicalNot`, `op_LogicalAnd`, and `op_LogicalOr` for `Not`, `And`, and `Or`, respectively) will have their logical operators mapped onto the Visual Basic logical operators.</span></span> <span data-ttu-id="b0f1b-867">Wenn sowohl die logischen als auch die bitweisen Operatoren überladen werden, werden nur die bitweisen Operatoren verwendet.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-867">If both the logical and bitwise operators are overloaded, only the bitwise operators will be used.</span></span>

* <span data-ttu-id="b0f1b-868">Durch Überladen der Operatoren `<<` und `>>` werden nur die signierten Operatoren aus der Perspektive anderer Sprachen überladen, die zwischen signierten und unsignierten Schiebe Operatoren unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-868">Overloading the `<<` and `>>` operators will overload only the signed operators from the perspective of other languages that distinguish between signed and unsigned shift operators.</span></span>

* <span data-ttu-id="b0f1b-869">Bei einer Klasse, die nur einen Ganzzahl ohne Vorzeichen Shift-Operator über lädt, wird der unsignierte Shift-Operator dem entsprechenden Visual Basic Shift-Operator zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-869">A class that overloads only an unsigned shift operator will have the unsigned shift operator mapped onto the corresponding Visual Basic shift operator.</span></span> <span data-ttu-id="b0f1b-870">Wenn sowohl ein Ganzzahl ohne Vorzeichen-als auch ein signed Shift-Operator überladen wird, wird nur der signierte Schiebe Operator verwendet.</span><span class="sxs-lookup"><span data-stu-id="b0f1b-870">If both an unsigned and signed shift operator is overloaded, only the signed shift operator will be used.</span></span>
