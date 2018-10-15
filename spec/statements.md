# <a name="statements"></a><span data-ttu-id="ac6c8-101">Anweisungen</span><span class="sxs-lookup"><span data-stu-id="ac6c8-101">Statements</span></span>

<span data-ttu-id="ac6c8-102">Anweisungen darstellen ausführbaren Code.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-102">Statements represent executable code.</span></span>

```antlr
Statement
    : LabelDeclarationStatement
    | LocalDeclarationStatement
    | WithStatement
    | SyncLockStatement
    | EventStatement
    | AssignmentStatement
    | InvocationStatement
    | ConditionalStatement
    | LoopStatement
    | ErrorHandlingStatement
    | BranchStatement
    | ArrayHandlingStatement
    | UsingStatement
    | AwaitStatement
    | YieldStatement
    ;
```

<span data-ttu-id="ac6c8-103">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="ac6c8-103">__Note.__</span></span> <span data-ttu-id="ac6c8-104">Microsoft Visual Basic-Compiler lässt nur Anweisungen, die mit einem Schlüsselwort oder einen Bezeichner zu starten.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-104">The Microsoft Visual Basic Compiler only allows statements which start with a keyword or an identifier.</span></span> <span data-ttu-id="ac6c8-105">Also z. B. der aufrufanweisung "`Call (Console).WriteLine`"ist zulässig, aber der aufrufanweisung"`(Console).WriteLine`" nicht.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-105">Thus, for instance, the invocation statement "`Call (Console).WriteLine`" is allowed, but the invocation statement "`(Console).WriteLine`" is not.</span></span>

## <a name="control-flow"></a><span data-ttu-id="ac6c8-106">Ablaufsteuerung</span><span class="sxs-lookup"><span data-stu-id="ac6c8-106">Control Flow</span></span>

<span data-ttu-id="ac6c8-107">*Ablaufsteuerung* ist die Sequenz, die in der Anweisungen und Ausdrücke ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-107">*Control flow* is the sequence in which statements and expressions are executed.</span></span> <span data-ttu-id="ac6c8-108">Die Reihenfolge der Ausführung hängt davon ab, die bestimmte Anweisung oder den Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-108">The order of execution depends on the particular statement or expression.</span></span>

<span data-ttu-id="ac6c8-109">Beispielsweise wird beim Auswerten der Addition-Operator (Abschnitt [Additionsoperator](expressions.md#addition-operator)), zuerst der linke Operand ausgewertet wird, klicken Sie dann der Rechte Operand, und klicken Sie dann der Operator selbst.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-109">For example, when evaluating an addition operator (Section [Addition Operator](expressions.md#addition-operator)), first the left operand is evaluated, then the right operand, and then the operator itself.</span></span> <span data-ttu-id="ac6c8-110">-Blocke ausgeführt werden (Abschnitt [Blöcke und Bezeichnungen](statements.md#blocks-and-labels)) zuerst Ausführen ihrer ersten-unteranweisung ist nicht möglich, und dann fortfahren einzeln durch die Anweisungen des Blocks.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-110">Blocks are executed (Section [Blocks and Labels](statements.md#blocks-and-labels)) by first executing their first substatement, and then proceeding one by one through the statements of the block.</span></span>

<span data-ttu-id="ac6c8-111">In diesem implizite Sortierung ist das Konzept der ein *, Utility control Point*, dies ist der nächste Vorgang ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-111">Implicit in this ordering is the concept of a *control point*, which is the next operation to be executed.</span></span> <span data-ttu-id="ac6c8-112">Wenn eine Methode aufgerufen (oder "namens" ist), wir sagen erstellt eine *Instanz* der Methode.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-112">When a method is invoked (or "called"), we say it creates an *instance* of the method.</span></span> <span data-ttu-id="ac6c8-113">Methodeninstanz besteht eine eigene Kopie der Parameter der Methode und lokale Variablen und eine eigene Kontrollpunkt aus.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-113">A method instance consists of its own copy of the method's parameters and local variables, and its own control point.</span></span>

### <a name="regular-methods"></a><span data-ttu-id="ac6c8-114">Reguläre Methoden</span><span class="sxs-lookup"><span data-stu-id="ac6c8-114">Regular Methods</span></span>

<span data-ttu-id="ac6c8-115">Hier ist ein Beispiel für eine normale Methode</span><span class="sxs-lookup"><span data-stu-id="ac6c8-115">Here is an example of a regular method</span></span>

```vb
Function Test() As Integer
    Console.WriteLine("hello")
    Return 1
End Sub

Dim x = Test()    ' invokes the function, prints "hello", assigns 1 to x
```

<span data-ttu-id="ac6c8-116">Wenn eine normale Methode aufgerufen wird,</span><span class="sxs-lookup"><span data-stu-id="ac6c8-116">When a regular method is invoked,</span></span>

1. <span data-ttu-id="ac6c8-117">Zuerst wird eine Instanz der Methode für diesen Aufruf spezifischen erstellt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-117">First an instance of the method is created specific to that invocation.</span></span> <span data-ttu-id="ac6c8-118">Diese Instanz enthält eine Kopie aller Parameter und lokalen Variablen der Methode.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-118">This instance includes a copy of all parameters and local variables of the method.</span></span>
2. <span data-ttu-id="ac6c8-119">Klicken Sie dann werden der Parameter für die angegebenen Werte und alle zugehörigen lokalen Variablen auf die Standardwerte der Typen initialisiert.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-119">Then all of its parameters are initialized to the supplied values, and all of its local variables to the default values of their types.</span></span>
3. <span data-ttu-id="ac6c8-120">Im Fall von einer `Function`, eine implizite lokale Variable ist initialisiert wird aufgerufen, die *Funktion Rückgabevariablen* , deren Name den Namen der Funktion ist, dessen Typ ist der Rückgabetyp der Funktion und, dessen erste ist der Standardwert dieses Typs.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-120">In the case of a `Function`, an implicit local variable is also initialized called the *function return variable* whose name is the function's name, whose type is the return type of the function and whose initial value is the default of its type.</span></span>
4. <span data-ttu-id="ac6c8-121">Kontrollpunkt für die Methodeninstanz wird bei der ersten Anweisung des Methodentexts festgelegt, und der Methodentext beginnt sofort von dort aus ausgeführt (Abschnitt [Blöcke und Bezeichnungen](statements.md#blocks-and-labels)).</span><span class="sxs-lookup"><span data-stu-id="ac6c8-121">The method instance's control point is then set at the first statement of the method body, and the method body immediately starts to execute from there (Section [Blocks and Labels](statements.md#blocks-and-labels)).</span></span>

<span data-ttu-id="ac6c8-122">Wenn ablaufsteuerung beendet wird den Methodentext normalerweise - durch erreichen die `End Function` oder `End Sub` , die Ende, markiert oder durch eine explizite `Return` oder `Exit` Statement - ablaufsteuerung zurückgibt, an den Aufrufer der Methodeninstanz.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-122">When control flow exits the method body normally - through reaching the `End Function` or `End Sub` that mark its end, or through an explicit `Return` or `Exit` statement - control flow returns to the caller of the method instance.</span></span> <span data-ttu-id="ac6c8-123">Bei eine Funktion Rückgabevariablen wird ist das Ergebnis des Aufrufs der endgültige Wert dieser Variablen an.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-123">If there is a function return variable, the result of the invocation is the final value of this variable.</span></span>

<span data-ttu-id="ac6c8-124">Wenn die ablaufsteuerung den Methodentext mithilfe einer nicht behandelten Ausnahme beendet wird, wird diese Ausnahme an den Aufrufer weitergegeben.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-124">When control flow exits the method body through an unhandled exception, that exception is propagated to the caller.</span></span>

<span data-ttu-id="ac6c8-125">Nachdem der ablaufsteuerung beendet wurde, sind nicht mehr alle aktiven Verweise auf die Instanz.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-125">After control flow has exited, there are no longer any live references to the method instance.</span></span> <span data-ttu-id="ac6c8-126">Wenn die Instanz nur Verweise auf eine Kopie der lokalen Variablen oder Parameter gehalten wird, können sie Garbage Collection bereinigt werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-126">If the method instance held the only references to its copy of local variables or parameters, then they may be garbage collected.</span></span>

### <a name="iterator-methods"></a><span data-ttu-id="ac6c8-127">Iterator-Methoden</span><span class="sxs-lookup"><span data-stu-id="ac6c8-127">Iterator Methods</span></span>

<span data-ttu-id="ac6c8-128">Iterator-Methoden werden verwendet, um eine Sequenz, einem generieren, die von genutzt werden können auf einfache Weise die `For Each` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-128">Iterator methods are used as a convenient way to generate a sequence, one which can be consumed by the `For Each` statement.</span></span> <span data-ttu-id="ac6c8-129">Iterator-Methoden verwenden die `Yield` Anweisung (Abschnitt [Yield-Anweisung](statements.md#yield-statement)), geben Sie die Elemente der Sequenz.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-129">Iterator methods use the `Yield` statement (Section [Yield Statement](statements.md#yield-statement)) to provide elements of the sequence.</span></span> <span data-ttu-id="ac6c8-130">(Eine Iteratormethode ohne `Yield` Anweisungen erzeugt eine leere Sequenz).</span><span class="sxs-lookup"><span data-stu-id="ac6c8-130">(An iterator method with no `Yield` statements will produce an empty sequence).</span></span> <span data-ttu-id="ac6c8-131">Hier ist ein Beispiel einer Iteratormethode:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-131">Here is an example of an iterator method:</span></span>

```vb
Iterator Function Test() As IEnumerable(Of Integer)
    Console.WriteLine("hello")
    Yield 1
    Yield 2
End Function

Dim en = Test()
For Each x In en          ' prints "hello" before the first x
    Console.WriteLine(x)  ' prints "1" and then "2"
Next
```

<span data-ttu-id="ac6c8-132">Wenn eine Iteratormethode aufgerufen wird, deren Rückgabetyp `IEnumerator(Of T)`,</span><span class="sxs-lookup"><span data-stu-id="ac6c8-132">When an iterator method is invoked whose return type is `IEnumerator(Of T)`,</span></span>

1. <span data-ttu-id="ac6c8-133">Zuerst wird eine Instanz der Iteratormethode für diesen Aufruf spezifischen erstellt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-133">First an instance of the iterator method is created specific to that invocation.</span></span> <span data-ttu-id="ac6c8-134">Diese Instanz enthält eine Kopie aller Parameter und lokalen Variablen der Methode.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-134">This instance includes a copy of all parameters and local variables of the method.</span></span>
2. <span data-ttu-id="ac6c8-135">Klicken Sie dann werden der Parameter für die angegebenen Werte und alle zugehörigen lokalen Variablen auf die Standardwerte der Typen initialisiert.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-135">Then all of its parameters are initialized to the supplied values, and all of its local variables to the default values of their types.</span></span>
3. <span data-ttu-id="ac6c8-136">Eine implizite lokale Variable ist initialisiert wird aufgerufen, die *aktuelle Iteratorvariable*, dessen Typ `T` und, dessen erste ist der Standardwert dieses Typs.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-136">An implicit local variable is also initialized called the *iterator current variable*, whose type is `T` and whose initial value is the default of its type.</span></span>
4. <span data-ttu-id="ac6c8-137">Kontrollpunkt für die Methodeninstanz wird bei der ersten Anweisung des Methodentexts festgelegt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-137">The method instance's control point is then set at the first statement of the method body.</span></span>
5. <span data-ttu-id="ac6c8-138">Ein *Iterator-Objekt* wird dann erstellt, mit dieser Instanz verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-138">An *iterator object* is then created, associated with this method instance.</span></span> <span data-ttu-id="ac6c8-139">Das Iterator-Objekt implementiert den deklarierte Rückgabetyp und verhält sich wie unten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-139">The iterator object implements the declared return type and has behavior as described below.</span></span>
6. <span data-ttu-id="ac6c8-140">Ablaufsteuerung wird dann fortgesetzt *sofort* im Aufrufer und das Ergebnis des Aufrufs ist das Iterator-Objekt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-140">Control flow is then resumed *immediately* in the caller, and the result of the invocation is the iterator object.</span></span> <span data-ttu-id="ac6c8-141">Beachten Sie, dass diese Übertragung realisiert wird, ohne eine Instanz der Iterator beendet, und führt nicht dazu, dass finally-Handler ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-141">Note that this transfer is done without exiting the iterator method instance, and does not cause finally handlers to execute.</span></span> <span data-ttu-id="ac6c8-142">Die Methodeninstanz wird weiterhin von dem Iterator-Objekt verwiesen, und wird keine Garbage Collection verschoben werden, sofern es vorhanden einen aktiven Verweis auf das Iterator-Objekt ist.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-142">The method instance is still referenced by the iterator object, and will not be garbage collected so long as there exists a live reference to the iterator object.</span></span>

<span data-ttu-id="ac6c8-143">Wenn des Iterator-Objekts `Current` -Eigenschaft zugegriffen wird, die *aktuelle Variable* des Aufrufs wird zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-143">When the iterator object's `Current` property is accessed, the *current variable* of the invocation is returned.</span></span>

<span data-ttu-id="ac6c8-144">Wenn die Iterator-Objekt `MoveNext` -Methode wird aufgerufen, die der Aufruf erstellt eine neue Methodeninstanz nicht.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-144">When the iterator object's `MoveNext` method is invoked, the invocation does not create a new method instance.</span></span> <span data-ttu-id="ac6c8-145">Stattdessen wird die vorhandene Methodeninstanz verwendet (und der Kontrollpunkt und lokale Variablen und Parameter) – die Instanz, die erstellt wurde, wenn die Iteratormethode zuerst aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-145">Instead the existing method instance is used (and its control point and local variables and parameters) - the instance that was created when the iterator method was first invoked.</span></span> <span data-ttu-id="ac6c8-146">Ablaufsteuerung, wird die Ausführung an den Kontrollpunkt der dieser Instanz fortgesetzt, und in den Text der Iteratormethode wie gewohnt fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-146">Control flow resumes execution at the control point of that method instance, and proceeds through the body of the iterator method as normal.</span></span>

<span data-ttu-id="ac6c8-147">Wenn die Iterator-Objekt `Dispose` -Methode wird aufgerufen, wird erneut die vorhandene Methodeninstanz verwendet.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-147">When the iterator object's `Dispose` method is invoked, again the existing method instance is used.</span></span> <span data-ttu-id="ac6c8-148">Steuern Sie Flow wird fortgesetzt, an der Kontrollpunkt dieses Methodeninstanz, aber dann sofort verhält sich wie ein `Exit Function` Anweisung wurden den nächsten Vorgang.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-148">Control flow resumes at the control point of that method instance, but then immediately behaves as if an `Exit Function` statement were the next operation.</span></span>

<span data-ttu-id="ac6c8-149">Der obigen Beschreibung des Verhaltens für das Aufrufen von `MoveNext` oder `Dispose` auf nur ein Iterator-Objekt angewendet werden soll, wenn alle vorherigen Aufrufe von `MoveNext` oder `Dispose` für das iteratorobjekt haben bereits an den Aufrufer zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-149">The above descriptions of behavior for invocation of `MoveNext` or `Dispose` on an iterator object only apply if all previous invocations of `MoveNext` or `Dispose` on that iterator object have already returned to their callers.</span></span> <span data-ttu-id="ac6c8-150">Wenn sie dies nicht getan haben, ist das Verhalten nicht definiert.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-150">If they haven't, then the behavior is undefined.</span></span>

<span data-ttu-id="ac6c8-151">Wenn ablaufsteuerung beendet wird den Text der Iterator-Methode normal über erreichen die `End Function` , die Ende, markiert oder durch eine explizite `Return` oder `Exit` Anweisung: sie müssen diesen Schritt ausgeführt haben im Kontext von einem Aufruf der `MoveNext` oder `Dispose` Funktion von einem iteratorobjekt zum Fortsetzen der Iteratorinstanz-Methode, und es wird bereits verwenden die Methodeninstanz, die erstellt wurde, wenn die Iteratormethode zuerst aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-151">When control flow exits the iterator method body normally -- through reaching the `End Function` that mark its end, or through an explicit `Return` or `Exit` statement -- it must have done so in the context of an invocation of `MoveNext` or `Dispose` function on an iterator object to resume the iterator method instance, and it will have been using the method instance that was created when the iterator method was first invoked.</span></span> <span data-ttu-id="ac6c8-152">Der Kontrollpunkt der Instanz beibehalten. bei der `End Function` -Anweisung und der Flow wieder im Aufrufer; und wenn er durch einen Aufruf von fortgesetzt wurde, hatte `MoveNext` klicken Sie dann den Wert `False` an den Aufrufer zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-152">The control point of that instance is left at the `End Function` statement, and control flow resumes in the caller; and if it had been resumed by a call to `MoveNext` then the value `False` is returned to the caller.</span></span>

<span data-ttu-id="ac6c8-153">Wenn die ablaufsteuerung beendet der Iterator Methodentext über eine nicht behandelte Ausnahme ist, wird die Ausnahme weitergegeben wird, an den Aufrufer, die es entweder eines Aufrufs der `MoveNext` oder `Dispose`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-153">When control flow exits the iterator method body through an unhandled exception, then the exception is propagated to the caller, which again will be either an invocation of `MoveNext` or of `Dispose`.</span></span>

<span data-ttu-id="ac6c8-154">Wie für die anderen möglichen Rückgabetypen einer Iterator-Funktion,</span><span class="sxs-lookup"><span data-stu-id="ac6c8-154">As for the other possible return types of an iterator function,</span></span>

* <span data-ttu-id="ac6c8-155">Wenn eine Iteratormethode aufgerufen wird, deren Rückgabetyp `IEnumerable(Of T)` für einige `T`, eine Instanz zuerst erstellt wird – speziell für diesen Aufruf der Iteratormethode--aller Parameter in der Methode, und sie werden mit den angegebenen Werten initialisiert.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-155">When an iterator method is invoked whose return type is `IEnumerable(Of T)` for some `T`, an instance is first created -- specific to that invocation of the iterator method -- of all parameters in the method, and they are initialized with the supplied values.</span></span> <span data-ttu-id="ac6c8-156">Das Ergebnis des Aufrufs ist ein ein Objekt, das den Rückgabetyp implementiert.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-156">The result of the invocation is an an object which implements the return type.</span></span> <span data-ttu-id="ac6c8-157">Sollte dieses Objekts `GetEnumerator` Methode aufgerufen werden, erstellt er eine Instanz – für diesen Aufruf von spezifischen `GetEnumerator` --aller Parameter und lokalen Variablen in der Methode.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-157">Should this object's `GetEnumerator` method be called, it creates an instance -- specific to that invocation of `GetEnumerator` -- of all parameters and local variables in the method.</span></span> <span data-ttu-id="ac6c8-158">Die Parameter für Werte, die bereits initialisiert, und fährt fort, wie der oben angegebenen Iteratormethode.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-158">It initializes the parameters to the values already saved, and proceeds as for the iterator method above.</span></span>
* <span data-ttu-id="ac6c8-159">Wenn eine Iteratormethode aufgerufen wird, deren Rückgabetyp, die nicht generische Schnittstelle ist `IEnumerator`, das Verhalten ist, genau wie bei `IEnumerator(Of Object)`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-159">When an iterator method is invoked whose return type is the non-generic interface `IEnumerator`, the behavior is exactly as for `IEnumerator(Of Object)`.</span></span>
* <span data-ttu-id="ac6c8-160">Wenn eine Iteratormethode aufgerufen wird, deren Rückgabetyp, die nicht generische Schnittstelle ist `IEnumerable`, das Verhalten ist, genau wie bei `IEnumerable(Of Object)`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-160">When an iterator method is invoked whose return type is the non-generic interface `IEnumerable`, the behavior is exactly as for `IEnumerable(Of Object)`.</span></span>

### <a name="async-methods"></a><span data-ttu-id="ac6c8-161">Asynchrone Methoden</span><span class="sxs-lookup"><span data-stu-id="ac6c8-161">Async Methods</span></span>

<span data-ttu-id="ac6c8-162">Async-Methoden sind eine einfache Möglichkeit, eine langfristig ausgeführte Arbeit zu tun, ohne zu blockieren, z. B. die Benutzeroberfläche einer Anwendung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-162">Async methods are a convenient way to do long-running work without for example blocking the UI of an application.</span></span> <span data-ttu-id="ac6c8-163">Async ist die Kurzform für *asynchrone* – Dies bedeutet, dass der Aufrufer der Async-Methode wird sofort die Ausführung fortgesetzt, aber die letztendliche Fertigstellung der Async-Methode-Instanz möglicherweise zu einem späteren Zeitpunkt in der Zukunft.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-163">Async is short for *Asynchronous* - it means that the caller of the async method will resume its execution promptly, but the eventual completion of the async method's instance may happen at some later time in the future.</span></span> <span data-ttu-id="ac6c8-164">Gemäß der Konvention werden asynchrone Methoden mit dem Suffix "Async" benannt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-164">By convention async methods are named with the suffix "Async".</span></span>

```vb
Async Function TestAsync() As Task(Of String)
    Console.WriteLine("hello")
    Await Task.Delay(100)
    Return "world"
End Function

Dim t = TestAsync()         ' prints "hello"
Console.WriteLine(Await t)  ' prints "world"
```

<span data-ttu-id="ac6c8-165">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="ac6c8-165">__Note.__</span></span> <span data-ttu-id="ac6c8-166">Async-Methoden sind *nicht* in einem Hintergrundthread ausführen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-166">Async methods are *not* run on a background thread.</span></span> <span data-ttu-id="ac6c8-167">Stattdessen können sie eine Methode, um durch Anhalten der `Await` Operator und Zeitplan selbst als Reaktion auf ein Ereignis fortgesetzt werden soll.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-167">Instead they allow a method to suspend itself through the `Await` operator, and schedule itself to be resumed in response to some event.</span></span>

<span data-ttu-id="ac6c8-168">Wenn eine asynchrone Methode aufgerufen wird</span><span class="sxs-lookup"><span data-stu-id="ac6c8-168">When an async method is invoked</span></span>

1. <span data-ttu-id="ac6c8-169">Zuerst wird eine Instanz der Async-Methode für diesen Aufruf spezifischen erstellt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-169">First an instance of the async method is created specific to that invocation.</span></span> <span data-ttu-id="ac6c8-170">Diese Instanz enthält eine Kopie aller Parameter und lokalen Variablen der Methode.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-170">This instance includes a copy of all parameters and local variables of the method.</span></span>
2. <span data-ttu-id="ac6c8-171">Klicken Sie dann werden der Parameter für die angegebenen Werte und alle zugehörigen lokalen Variablen auf die Standardwerte der Typen initialisiert.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-171">Then all of its parameters are initialized to the supplied values, and all of its local variables to the default values of their types.</span></span>
3. <span data-ttu-id="ac6c8-172">Im Fall von einer Async-Methode, mit dem Rückgabetyp `Task(Of T)` für einige `T`, eine implizite lokale Variable ist initialisiert wird aufgerufen, die *aufgabenrückgabevariable*, dessen Typ `T` und dessen erste Wert ist der Standardwert `T`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-172">In the case of an async method with return type `Task(Of T)` for some `T`, an implicit local variable is also initialized called the *task return variable*, whose type is `T` and whose initial value is the default of `T`.</span></span>
4. <span data-ttu-id="ac6c8-173">Wenn die Async-Methode ist eine `Function` mit Rückgabetyp `Task` oder `Task(Of T)` für einige `T`, klicken Sie dann ein Objekt dieses Typs implizit erstellt, den aktuellen Aufruf zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-173">If the async method is a `Function` with return type `Task` or `Task(Of T)` for some `T`, then an object of that type implicitly created, associated with the current invocation.</span></span> <span data-ttu-id="ac6c8-174">Dies wird als bezeichnet ein *Async-Objekt* und stellt dar, die zukünftige Arbeit, die durch Ausführen der Instanz der Async-Methode durchgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-174">This is called an *async object* and represents the future work that will be done by executing the instance of the async method.</span></span> <span data-ttu-id="ac6c8-175">Wenn Steuerelement im Aufrufer dieser Instanz der Async-Methode fortgesetzt wird, erhalten der Aufrufer dieser Async-Objekt als Ergebnis aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-175">When control resumes in the caller of this async method instance, the caller will receive this async object as the result of its invocation.</span></span>
5. <span data-ttu-id="ac6c8-176">Steuerungspunkts für das der Instanz wird bei der ersten Anweisung des Hauptteils des Async-Methode festgelegt, und startet umgehend den Methodentext dort ausgeführt (Abschnitt [Blöcke und Bezeichnungen](statements.md#blocks-and-labels)).</span><span class="sxs-lookup"><span data-stu-id="ac6c8-176">The instance's control point is then set at the first statement of the async method body, and immediately starts to execute the method body from there (Section [Blocks and Labels](statements.md#blocks-and-labels)).</span></span>

<span data-ttu-id="ac6c8-177">__Wiederaufnahmedelegat und aktuellen Aufrufer__</span><span class="sxs-lookup"><span data-stu-id="ac6c8-177">__Resumption Delegate and Current Caller__</span></span>

<span data-ttu-id="ac6c8-178">Finden Sie im Abschnitt [Await-Operator](expressions.md#await-operator), Ausführung einer `Await` Ausdruck hat die Möglichkeit, unterbrechen die Methodeninstanz Steuerungspunkts für das Verlassen der ablaufsteuerung, um an anderer Stelle zu wechseln.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-178">As detailed in Section [Await Operator](expressions.md#await-operator), execution of an `Await` expression has the ability to suspend the method instance's control point leaving control flow to go elsewhere.</span></span> <span data-ttu-id="ac6c8-179">Ablaufsteuerung kann später fortsetzen, an der gleichen Instanz des Steuerungspunkts für das durch Aufruf einer *Wiederaufnahmedelegat*.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-179">Control flow can later resume at the same instance's control point through invocation of a *resumption delegate*.</span></span> <span data-ttu-id="ac6c8-180">Beachten Sie, dass dieses Aussetzens durchgeführt werden, ohne die Async-Methode wird beendet, und führt nicht dazu, dass finally-Handler ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-180">Note that this suspension is done without exiting the async method, and does not cause finally handlers to execute.</span></span> <span data-ttu-id="ac6c8-181">Beide Wiederaufnahmedelegat wird noch die Methodeninstanz verweisen und die `Task` oder `Task(Of T)` führen (sofern vorhanden), und nicht Garbage gesammelt, sofern live Referenz vorhanden zu delegieren, oder führen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-181">The method instance is still referenced by both the resumption delegate and the `Task` or `Task(Of T)` result (if any), and will not be garbage collected so long as there exists a live reference to either delegate or result.</span></span>

<span data-ttu-id="ac6c8-182">Es ist hilfreich, stellen Sie sich vor der Anweisung `Dim x = Await WorkAsync()` ungefähr als syntaktische Kurzform für die folgenden:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-182">It is helpful to imagine the statement `Dim x = Await WorkAsync()` approximately as syntactic shorthand for the following:</span></span>

```vb
Dim temp = WorkAsync().GetAwaiter()
If Not temp.IsCompleted Then
       temp.OnCompleted(resumptionDelegate)
       Return [task]
       CONT:   ' invocation of 'resumptionDelegate' will resume here
End If
Dim x = temp.GetResult()
```

<span data-ttu-id="ac6c8-183">Im folgenden wird die *aktuellen Aufrufer* der Methode Instanz definiert ist, als der ursprüngliche Aufrufer oder der Aufrufer den Wiederaufnahmedelegat je aktueller ist.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-183">In the following, the *current caller* of the method instance is defined as either the original caller, or the caller of the resumption delegate, whichever is more recent.</span></span>

<span data-ttu-id="ac6c8-184">Wenn ablaufsteuerung beendet den Text für das Async-Methode – über erreicht wird die `End Sub` oder `End Function` , die Ende, markiert oder durch eine explizite `Return` oder `Exit` -Anweisung oder über eine nicht behandelte Ausnahme--Steuerungspunkts für das der Instanz auf festgelegt ist das Ende der Methode.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-184">When control flow exits the async method body -- through reaching the `End Sub` or `End Function` that mark its end, or through an explicit `Return` or `Exit` statement, or through an unhandled exception -- the instance's control point is set to the end of the method.</span></span> <span data-ttu-id="ac6c8-185">Verhalten, hängt vom Rückgabetyp der asynchronen Methode.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-185">Behavior then depends on the return type of the async method.</span></span>

* <span data-ttu-id="ac6c8-186">Im Fall von einem `Async Function` mit Rückgabetyp `Task`:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-186">In the case of an `Async Function` with return type `Task`:</span></span>
  1. <span data-ttu-id="ac6c8-187">Wenn beendet wird, über eine nicht behandelte Ausnahme, klicken Sie dann die asynchronen den Status des Objekts ablaufsteuerung nastaven NA hodnotu `TaskStatus.Faulted` und die zugehörige `Exception.InnerException` -Eigenschaftensatz auf die Ausnahme (mit Ausnahme von: bestimmte Ausnahmen Implementierung definiert, wie z. B. `OperationCanceledException` ändern Sie ihn in `TaskStatus.Canceled`).</span><span class="sxs-lookup"><span data-stu-id="ac6c8-187">If control flow exits through an unhandled exception, then the async object's status is set to `TaskStatus.Faulted` and its `Exception.InnerException` property is set to the exception (except: certain implementation-defined exceptions such as `OperationCanceledException` change it to `TaskStatus.Canceled`).</span></span> <span data-ttu-id="ac6c8-188">Flow wieder in den aktuellen Aufrufer.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-188">Control flow resumes in the current caller.</span></span>
  2. <span data-ttu-id="ac6c8-189">Andernfalls wird der asynchronen den Status des Objekts auf festgelegt `TaskStatus.Completed`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-189">Otherwise, the async object's status is set to `TaskStatus.Completed`.</span></span> <span data-ttu-id="ac6c8-190">Flow wieder in den aktuellen Aufrufer.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-190">Control flow resumes in the current caller.</span></span>

     <span data-ttu-id="ac6c8-191">(__Beachten.__</span><span class="sxs-lookup"><span data-stu-id="ac6c8-191">(__Note.__</span></span> <span data-ttu-id="ac6c8-192">Die gesamte Punkt der Aufgabe und Async-Methoden interessant, woraus ist, wird eine Aufgabe abgeschlossen und alle Methoden, die auf ihn warten wurden haben, werden derzeit Wiederaufnahme Delegate ausgeführt, d. h. werden sie entsperrt werden.)</span><span class="sxs-lookup"><span data-stu-id="ac6c8-192">The whole point of Task, and what makes async methods interesting, is that when a task becomes Completed then any methods that were awaiting it will presently have their resumption delegates executed, i.e. they will become unblocked.)</span></span>

* <span data-ttu-id="ac6c8-193">Im Fall von einer `Async Function` mit Rückgabetyp `Task(Of T)` für einige `T`: das Verhalten ist als oben, außer, dass in nicht-Ausnahme des asynchronen Objekts Fällen `Result` -Eigenschaft auch auf den endgültigen Wert, der die aufgabenrückgabevariable festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-193">In the case of an `Async Function` with return type `Task(Of T)` for some `T`: the behavior is as above, except that in non-exception cases the async object's `Result` property is also set to the final value of the task return variable.</span></span>

* <span data-ttu-id="ac6c8-194">Im Fall von einem `Async Sub`:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-194">In the case of an `Async Sub`:</span></span>
  1. <span data-ttu-id="ac6c8-195">Wenn beendet wird, über eine nicht behandelte Ausnahme, klicken Sie dann die Ausnahme ablaufsteuerung werden in der Umgebung auf implementierungsspezifische weitergegeben.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-195">If control flow exits through an unhandled exception, then that exception is propagated to the environment in some implementation-specific manner.</span></span> <span data-ttu-id="ac6c8-196">Flow wieder in den aktuellen Aufrufer.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-196">Control flow resumes in the current caller.</span></span>
  2. <span data-ttu-id="ac6c8-197">Andernfalls wird ablaufsteuerung einfach in den aktuellen Aufrufer fortgesetzt werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-197">Otherwise, control flow simply resumes in the current caller.</span></span>

#### <a name="async-sub"></a><span data-ttu-id="ac6c8-198">Async-Unterfunktion.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-198">Async Sub</span></span>

<span data-ttu-id="ac6c8-199">Es gibt einige Microsoft-spezifische Verhalten einer `Async Sub`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-199">There is some Microsoft-specific behavior of an `Async Sub`.</span></span>

<span data-ttu-id="ac6c8-200">Wenn `SynchronizationContext.Current` ist `Nothing` zu Beginn des Aufrufs, klicken Sie dann jede nicht behandelten Ausnahmen einer Async-Unterfunktion werden im Verteilungsverlauf Threadpool der Warteschleife hinzu.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-200">If `SynchronizationContext.Current` is `Nothing` at the start of the invocation, then any unhandled exceptions from an Async Sub will be posted to the Threadpool.</span></span>

<span data-ttu-id="ac6c8-201">Wenn `SynchronizationContext.Current` nicht `Nothing` am Anfang des Aufrufs, klicken Sie dann `OperationStarted()` für diesen Kontext vor dem Start der Methode aufgerufen wird und `OperationCompleted()` nach dem Ende.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-201">If `SynchronizationContext.Current` is not `Nothing` at the start of the invocation, then `OperationStarted()` is invoked on that context before the start of the method and `OperationCompleted()` after the end.</span></span> <span data-ttu-id="ac6c8-202">Darüber hinaus werden alle Ausnahmefehler bereitgestellt werden, auf den Synchronisierungskontext erneut ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-202">Additionally, any unhandled exceptions will be posted to be rethrown on the synchronization context.</span></span>

<span data-ttu-id="ac6c8-203">Dies bedeutet, dass in UI-Anwendungen, für eine `Async Sub` , die im UI-Thread aufgerufen wird, ein Fehler auftritt, behandeln, Ausnahmen werden erneut den UI-Thread abgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-203">This means that in UI applications, for an `Async Sub` that is invoked on the UI thread, any exceptions it fails to handle will be reposted the UI thread.</span></span>

#### <a name="mutable-structures-in-async-and-iterator-methods"></a><span data-ttu-id="ac6c8-204">Änderbare Strukturen in Async-und iteratormethoden</span><span class="sxs-lookup"><span data-stu-id="ac6c8-204">Mutable structures in async and iterator methods</span></span>

<span data-ttu-id="ac6c8-205">Änderbare Strukturen im Allgemeinen werden als ungültiges Verfahren angesehen, und sie werden von Async- oder Iterator-Methoden nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-205">Mutable structures in general are considered bad practice, and they are not supported by async or iterator methods.</span></span> <span data-ttu-id="ac6c8-206">Jeder Aufruf einer Async- oder Iterator-Methode in einer Struktur werden insbesondere implizit ausgeführt, auf eine *Kopie* dieser Struktur, die zu der Zeitpunkt des Aufrufs kopiert werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-206">In particular, each invocation of an async or iterator method in a structure will implicitly operate on a *copy* of that structure that is copied at its moment of invocation.</span></span> <span data-ttu-id="ac6c8-207">Daher ist es beispielsweise bei</span><span class="sxs-lookup"><span data-stu-id="ac6c8-207">Thus, for example,</span></span>

```vb
Structure S
       Dim x As Integer
       Async Sub Mutate()
           x = 2
       End Sub
End Structure

Dim s As New S With {.x = 1}
s.Mutate()
Console.WriteLine(s.x)   ' prints "1"
```

### <a name="blocks-and-labels"></a><span data-ttu-id="ac6c8-208">Blöcke und Bezeichnungen</span><span class="sxs-lookup"><span data-stu-id="ac6c8-208">Blocks and Labels</span></span>

<span data-ttu-id="ac6c8-209">Eine Gruppe von ausführbaren Anweisungen wird einen Anweisungsblock aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-209">A group of executable statements is called a statement block.</span></span> <span data-ttu-id="ac6c8-210">Die Ausführung der Anweisungsblock beginnt mit der ersten Anweisung im-Block.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-210">Execution of a statement block begins with the first statement in the block.</span></span> <span data-ttu-id="ac6c8-211">Nachdem eine Anweisung ausgeführt wurde, wird die nächste Anweisung im lexikalischen Reihenfolge ausgeführt, es sei denn, eine Anweisung überträgt die Ausführung an einem anderen Ort oder eine Ausnahme auftritt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-211">Once a statement has been executed, the next statement in lexical order is executed, unless a statement transfers execution elsewhere or an exception occurs.</span></span>

<span data-ttu-id="ac6c8-212">Innerhalb eines Blocks Anweisung ist nicht die Division von Anweisungen für logische Zeilen mit Ausnahme von Bezeichnung deklarationsanweisungen erhebliche.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-212">Within a statement block, the division of statements on logical lines is not significant with the exception of label declaration statements.</span></span> <span data-ttu-id="ac6c8-213">Eine Bezeichnung ist ein Bezeichner, der eine bestimmte Position innerhalb der Anweisungsblock identifiziert, die als Ziel einer Verzweigung-Anweisung wie z. B. verwendet werden können `GoTo`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-213">A label is an identifier that identifies a particular position within the statement block that can be used as the target of a branch statement such as `GoTo`.</span></span>

```antlr
Block
    : Statements*
    ;

LabelDeclarationStatement
    : LabelName ':'
    ;

LabelName
    : Identifier
    | IntLiteral
    ;

Statements
    : Statement? ( ':' Statement? )*
    ;
```


<span data-ttu-id="ac6c8-214">Deklarationsanweisungen Bezeichnung müssen am Anfang einer logischen Zeile angezeigt werden und Bezeichnungen können entweder ein Bezeichner oder ein Integer-literal.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-214">Label declaration statements must appear at the beginning of a logical line and labels may be either an identifier or an integer literal.</span></span> <span data-ttu-id="ac6c8-215">Da sowohl die Bezeichnung deklarationsanweisungen als auch die Aufruf-Anweisungen einen einzelnen Bezeichner enthalten können, wird am Anfang eine lokale Zeile ein einzelnen Bezeichner immer eine deklarationsanweisung Bezeichnung angesehen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-215">Because both label declaration statements and invocation statements can consist of a single identifier, a single identifier at the beginning of a local line is always considered a label declaration statement.</span></span> <span data-ttu-id="ac6c8-216">Deklarationsanweisungen Bezeichnung müssen immer von einem Doppelpunkt folgen, selbst wenn keine Anweisungen in derselben logischen Zeile folgen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-216">Label declaration statements must always be followed by a colon, even if no statements follow on the same logical line.</span></span>

<span data-ttu-id="ac6c8-217">Bezeichnungen müssen ihre eigenen Deklarationsabschnitt und verursachen keine Konflikte mit anderen Bezeichnern.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-217">Labels have their own declaration space and do not interfere with other identifiers.</span></span> <span data-ttu-id="ac6c8-218">Im folgenden Beispiel ist gültig und wird verwendet, die Name-Variable `x` als Parameter sowohl als Bezeichnung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-218">The following example is valid and uses the name variable `x` both as a parameter and as a label.</span></span>

```vb
Function F(x As Integer) As Integer
    If x >= 0 Then
        GoTo x
    End If
    x = -x
x: 
    Return x
End Function
```

<span data-ttu-id="ac6c8-219">Der Umfang einer Bezeichnung wird der Text der Methode, die sie enthält.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-219">The scope of a label is the body of the method containing it.</span></span>

<span data-ttu-id="ac6c8-220">Aus Gründen der besseren Lesbarkeit werden Anweisung Produktionen, bei denen mehrere Substatements als einer einzigen Produktion in dieser Spezifikation behandelt, obwohl die Substatements jedes selbst in einer Zeile mit Bezeichnung sein können.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-220">For the sake of readability, statement productions that involve multiple substatements are treated as a single production in this specification, even though the substatements may each be by themselves on a labeled line.</span></span>


### <a name="local-variables-and-parameters"></a><span data-ttu-id="ac6c8-221">Lokale Variablen und Parameter</span><span class="sxs-lookup"><span data-stu-id="ac6c8-221">Local Variables and Parameters</span></span>

<span data-ttu-id="ac6c8-222">Details wie der vorherigen Abschnitte. und wenn Methodeninstanzen erstellt werden und mit ihnen die Kopien der lokalen Variablen und Parameter einer Methode.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-222">The preceding sections detail how and when method instances are created, and with them the copies of a method's local variables and parameters.</span></span> <span data-ttu-id="ac6c8-223">Darüber hinaus jedes Mal, die der Text einer Schleife erreicht wird, eine neue Kopie wird erstellt für jede lokale Variable, die innerhalb dieser Schleife deklariert werden, wie im Abschnitt beschrieben [Schleifenanweisungen](statements.md#loop-statements), und die Methodeninstanz enthält nun diese Kopie der lokalen Variablen anstatt als die vorherigen Kopie.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-223">In addition, each time the body of a loop is entered, a new copy is made of each local variable declared inside that loop as described in Section [Loop Statements](statements.md#loop-statements), and the method instance now contains this copy of its local variable rather than the previous copy.</span></span>

<span data-ttu-id="ac6c8-224">Alle lokalen Variablen werden mit Standardwert des Typs initialisiert.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-224">All locals are initialized to their type's default value.</span></span> <span data-ttu-id="ac6c8-225">Lokale Variablen und Parameter sind immer öffentlich zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-225">Local variables and parameters are always publicly accessible.</span></span> <span data-ttu-id="ac6c8-226">Es ist ein Fehler in der Lage, Text, der vor der Deklaration einer lokalen Variablen verweisen, wie im folgende Beispiel veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-226">It is an error to refer to a local variable in a textual position that precedes its declaration, as the following example illustrates:</span></span>

```vb
Class A
    Private i As Integer = 0

    Sub F()
        i = 1
        Dim i As Integer       ' Error, use precedes declaration.
        i = 2
    End Sub

    Sub G()
        Dim a As Integer = 1
        Dim b As Integer = a   ' This is valid.
    End Sub
End Class
```

<span data-ttu-id="ac6c8-227">In der `F` oben genannte Methode, die erste Zuweisung zu `i` insbesondere verweist nicht auf das Feld im äußeren Bereich deklariert.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-227">In the `F` method above, the first assignment to `i` specifically does not refer to the field declared in the outer scope.</span></span> <span data-ttu-id="ac6c8-228">Stattdessen verweist er auf die lokale Variable, und Fehler ist, da die Deklaration der Variablen textlich vorausgehenden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-228">Rather, it refers to the local variable, and it is in error because it textually precedes the declaration of the variable.</span></span> <span data-ttu-id="ac6c8-229">In der `G` -Methode, eine nachfolgenden Variablendeklaration bezieht sich auf eine lokale Variable, die in der eine früheren Deklaration in der gleichen Deklaration lokaler Variablen deklariert.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-229">In the `G` method, a subsequent variable declaration refers to a local variable declared in an earlier variable declaration within the same local variable declaration.</span></span>

<span data-ttu-id="ac6c8-230">Jeder Block in einer Methode erstellt eine Deklaration Speicherplatz für lokale Variablen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-230">Each block in a method creates a declaration space for local variables.</span></span> <span data-ttu-id="ac6c8-231">Namen werden in diesen Deklarationsabschnitt über Deklarationen von lokalen Variablen im Methodentext und der Parameterliste der Methode, die Namen in der äußersten Blocks Deklarationsabschnitt führt eingeführt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-231">Names are introduced into this declaration space through local variable declarations in the method body and through the parameter list of the method, which introduces names into the outermost block's declaration space.</span></span> <span data-ttu-id="ac6c8-232">Blöcke nicht shadowing von Namen durch die Schachtelung zulassen: Nachdem Sie ein Namen in einem Block deklariert wurde, der Name kann nicht erneut deklariert werden in einem geschachtelten Block.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-232">Blocks do not allow shadowing of names through nesting: once a name has been declared in a block, the name may not be redeclared in any nested blocks.</span></span>

<span data-ttu-id="ac6c8-233">Im folgenden Beispiel die `F` und `G` Methoden sind Fehler, da der Name `i` im äußeren-Block deklariert wird und nicht im inneren Block erneut deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-233">Thus, in the following example, the `F` and `G` methods are in error because the name `i` is declared in the outer block and cannot be redeclared in the inner block.</span></span> <span data-ttu-id="ac6c8-234">Allerdings die `H` und `I` Methoden sind gültig. da die beiden `i`des werden in separaten nicht geschachtelten Blöcken deklariert.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-234">However, the `H` and `I` methods are valid because the two `i`'s are declared in separate non-nested blocks.</span></span>

```vb
Class A
    Sub F()
        Dim i As Integer = 0
        If True Then
               Dim i As Integer = 1
        End If
    End Sub

    Sub G()
        If True Then
            Dim i As Integer = 0
        End If
        Dim i As Integer = 1
    End Sub 

    Sub H()
        If True Then
            Dim i As Integer = 0
        End If
        If True Then
            Dim i As Integer = 1
        End If
    End Sub

    Sub I() 
        For i As Integer = 0 To 9
            H()
        Next i

        For i As Integer = 0 To 9
            H()
        Next i
    End Sub 
End Class
```

<span data-ttu-id="ac6c8-235">Wenn die Methode eine Funktion ist, wird eine spezielle lokale Variable implizit in den Methodentext Deklarationsabschnitt mit dem gleichen Namen wie die Methode, die den Rückgabewert der Funktion darstellt deklariert.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-235">When the method is a function, a special local variable is implicitly declared in the method body's declaration space with the same name as the method representing the return value of the function.</span></span> <span data-ttu-id="ac6c8-236">Die lokale Variable ist sondername Auflösung Semantik, wenn in Ausdrücken verwendet.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-236">The local variable has special name resolution semantics when used in expressions.</span></span> <span data-ttu-id="ac6c8-237">Wenn die lokale Variable in einem Kontext verwendet wird, die einen Ausdruck, der als eine Methodengruppe, z. B. ein Aufrufausdruck klassifiziert erwartet löst den Namen der Funktion und nicht auf die lokale Variable.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-237">If the local variable is used in a context that expects an expression classified as a method group, such as an invocation expression, then the name resolves to the function rather than to the local variable.</span></span> <span data-ttu-id="ac6c8-238">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-238">For example:</span></span>

```vb
Function F(i As Integer) As Integer
    If i = 0 Then
        F = 1        ' Sets the return value.
    Else
        F = F(i - 1) ' Recursive call.
    End If
End Function
```

<span data-ttu-id="ac6c8-239">Die Verwendung von Klammern kann dazu führen, dass mehrdeutige Situationen (z. B. `F(1)`, wobei `F` ist eine Funktion, deren Rückgabetyp ein eindimensionales Array ist); in allen Situationen nicht eindeutig, der Name aufgelöst wird, um die Funktion anstatt der lokalen Variablen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-239">The use of parentheses can cause ambiguous situations (such as `F(1)`, where `F` is a function whose return type is a one-dimensional array); in all ambiguous situations, the name resolves to the function rather than the local variable.</span></span> <span data-ttu-id="ac6c8-240">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-240">For example:</span></span>

```vb
Function F(i As Integer) As Integer()
    If i = 0 Then
        F = new Integer(2) { 1, 2, 3 }
    Else
        F = F(i - 1) ' Recursive call, not an index.
    End If
End Function
```

<span data-ttu-id="ac6c8-241">Bei der ablaufsteuerung des Methodentexts verlässt, wird der Wert der lokalen Variablen zurück, der Aufrufausdruck übergeben.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-241">When control flow leaves the method body, the value of the local variable is passed back to the invocation expression.</span></span> <span data-ttu-id="ac6c8-242">Wenn die Methode eine Subroutine ist, gibt es keine solche implizite lokale Variable ist und Steuerelement auf der Aufrufausdruck gibt einfach auftragsantwortnachrichten zurück.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-242">If the method is a subroutine, there is no such implicit local variable, and control simply returns to the invocation expression.</span></span>

## <a name="local-declaration-statements"></a><span data-ttu-id="ac6c8-243">Lokale Deklarationsanweisungen</span><span class="sxs-lookup"><span data-stu-id="ac6c8-243">Local Declaration Statements</span></span>

<span data-ttu-id="ac6c8-244">Eine lokalen deklarationsanweisung deklariert eine neue lokale Variable, eine lokale Konstante oder eine statische Variable.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-244">A local declaration statement declares a new local variable, local constant, or static variable.</span></span> <span data-ttu-id="ac6c8-245">*Lokale Variablen* und *lokale Konstanten* sind gleichwertig mit der von Instanzvariablen und Konstanten, die an die Methode beschränkt, und auf die gleiche Weise deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-245">*Local variables* and *local constants* are equivalent to instance variables and constants scoped to the method and are declared in the same way.</span></span> <span data-ttu-id="ac6c8-246">*Statische Variablen* ähneln `Shared` mithilfe von Variablen und sind deklariert die `Static` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-246">*Static variables* are similar to `Shared` variables and are declared using the `Static` modifier.</span></span>

```antlr
LocalDeclarationStatement
    : LocalModifier VariableDeclarators StatementTerminator
    ;

LocalModifier
    : 'Static' | 'Dim' | 'Const'
    ;
```

<span data-ttu-id="ac6c8-247">Statische Variablen handelt es sich um lokale Variablen, die über Aufrufe der Methode ihren Wert beibehalten.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-247">Static variables are locals that retain their value across invocations of the method.</span></span> <span data-ttu-id="ac6c8-248">Statische Variablen, die innerhalb von nicht freigegebenen Methoden deklariert werden, pro Instanz: jede Instanz des Typs, der die Methode enthält, hat eine eigene Kopie der statischen Variable.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-248">Static variables declared within non-shared methods are per instance: each instance of the type that contains the method has its own copy of the static variable.</span></span> <span data-ttu-id="ac6c8-249">Statische Variablen deklariert in `Shared` Methoden sind pro Typ; es ist nur eine Kopie der statischen Variablen für alle Instanzen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-249">Static variables declared within `Shared` methods are per type; there is only one copy of the static variable for all instances.</span></span> <span data-ttu-id="ac6c8-250">Während der lokale Variablen des Typs der Standardwert bei jeder Eintrag in die Methode initialisiert werden, sind statische Variablen auf den Standardwert des Typs nur initialisiert, wenn der Typ oder die Instanz initialisiert wird.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-250">While local variables are initialized to their type's default value upon each entry into the method, static variables are only initialized to their type's default value when the type or type instance is initialized.</span></span> <span data-ttu-id="ac6c8-251">Statische Variablen können nicht in Strukturen oder generischen Methoden deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-251">Static variables may not be declared in structures or generic methods.</span></span>

<span data-ttu-id="ac6c8-252">Lokale Variablen, lokale Konstanten und statische Variablen immer öffentliche zugreifbarkeit besitzen, und können keinen Zugriffsmodifizierer angeben.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-252">Local variables, local constants, and static variables always have public accessibility and may not specify accessibility modifiers.</span></span> <span data-ttu-id="ac6c8-253">Wenn kein Typ in einer lokalen deklarationsanweisung angegeben wird, bestimmen die folgenden Schritte aus den Typ der lokalen Deklaration:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-253">If no type is specified on a local declaration statement, then the following steps determine the type of the local declaration:</span></span>

1. <span data-ttu-id="ac6c8-254">Wenn die Deklaration ein Typzeichen, ist der Typ des Zeichens Typ den Typ der lokalen Deklaration.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-254">If the declaration has a type character, the type of the type character is the type of the local declaration.</span></span>

2. <span data-ttu-id="ac6c8-255">Wenn die lokale Deklaration eine lokale Konstante ist, oder wenn die lokale Deklaration ist eine lokale Variable mit einem Initialisierer und lokaler Variablen Typrückschluss verwendet wird, wird der Typ der lokalen Deklaration aus dem Typ des Initialisierers abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-255">If the local declaration is a local constant, or if the local declaration is a local variable with an initializer and local variable type inference is being used, the type of the local declaration is inferred from the type of the initializer.</span></span> <span data-ttu-id="ac6c8-256">Wenn der Initialisierer der lokalen Deklaration verweist, tritt auf, ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-256">If the initializer refers to the local declaration, a compile-time error occurs.</span></span> <span data-ttu-id="ac6c8-257">(Lokale Konstanten müssen initialisiert werden.)</span><span class="sxs-lookup"><span data-stu-id="ac6c8-257">(Local constants are required to have initializers.)</span></span>

3. <span data-ttu-id="ac6c8-258">Wenn strikte Semantik nicht verwendet werden, ist implizit der Typ des von der lokalen deklarationsanweisung `Object`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-258">If strict semantics are not being used, the type of the local declaration statement is implicitly `Object`.</span></span>

4. <span data-ttu-id="ac6c8-259">Andernfalls tritt ein Kompilierungsfehler auf.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-259">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="ac6c8-260">Wenn kein Typ in einer lokalen deklarationsanweisung, die eine Arraygröße oder ein Array der Modifizierer verfügt angegeben ist, klicken Sie dann der Typ der lokalen Deklaration ist ein Array mit dem angegebenen Rang und die vorherigen Schritte werden verwendet, um den Elementtyp des Arrays zu bestimmen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-260">If no type is specified on a local declaration statement that has an array size or array type modifier, then the type of the local declaration is an array with the specified rank and the previous steps are used to determine the element type of the array.</span></span> <span data-ttu-id="ac6c8-261">Wenn lokale Variable Typrückschluss verwendet wird, muss der Typ des Initialisierers einen Arraytyp mit der gleichen Array-Form (d. h. Arraymodifizierer Typ) wie die Deklaration von lokalen-Anweisung sein.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-261">If local variable type inference is used, the type of the initializer must be an array type with the same array shape (i.e. array type modifiers) as the local declaration statement.</span></span> <span data-ttu-id="ac6c8-262">Beachten Sie, dass es möglich, dass der Typ des abgeleiteten Elements noch ein Arraytyp sein kann.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-262">Note that it is possible that the inferred element type may still be an array type.</span></span> <span data-ttu-id="ac6c8-263">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-263">For example:</span></span>

```vb
Option Infer On

Module Test
    Sub Main()
        ' Error: initializer is not an array type
        Dim x() = 1

        ' Type is Integer()
        Dim y() = New Integer() {}

        ' Type is Integer()()
        Dim z() = New Integer()() {}

        ' Type is Integer()()()

        Dim a()() = New Integer()()() {}

        ' Error: initializer does not have same array shape
        Dim b()() = New Integer(,)() {}
    End Sub
End Module
```

<span data-ttu-id="ac6c8-264">Wenn kein Typ in einer lokalen deklarationsanweisung angegeben wird, besitzt den Typmodifizierer für einen NULL-Werte zulässt, und klicken Sie dann der Typ der lokalen Deklaration ist der auf NULL festlegbare Version des abgeleiteten Typs oder der abgeleitete Typ selbst aus, wenn sie bereits ein NULL-Wert eingeben.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-264">If no type is specified on a local declaration statement that has a nullable type modifier, then the type of the local declaration is the nullable version of the inferred type or the inferred type itself if it a nullable value type already.</span></span>  <span data-ttu-id="ac6c8-265">Wenn der abgeleitete Typ kein Werttyp, der auf NULL festgelegt werden kann ist, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-265">If the inferred type is not a value type that can be made nullable, a compile-time error occurs.</span></span> <span data-ttu-id="ac6c8-266">Wenn sowohl den Typmodifizierer für einen NULL-Werte zulässt und ein Array-Größe oder Array Typmodifizierer auf einer lokalen deklarationsanweisung ohne Typ gespeichert werden, klicken Sie dann der Modifizierer nullable-Typ gilt, die den Elementtyp des Arrays angewendet, und die vorherigen Schritte werden verwendet, um zu bestimmen, die eleme NT-Typ.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-266">If both a nullable type modifier and an array size or array type modifier are placed on a local declaration statement with no type, then the nullable type modifier is considered to apply to the element type of the array and the previous steps are used to determine the element type.</span></span>

<span data-ttu-id="ac6c8-267">Variableninitialisierern auf lokale deklarationsanweisungen sind gleichwertig mit zuweisungsanweisungen, die am Text Speicherort der Deklaration platziert werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-267">Variable initializers on local declaration statements are equivalent to assignment statements placed at the textual location of the declaration.</span></span> <span data-ttu-id="ac6c8-268">Wenn die Ausführung über die lokale deklarationsanweisung verzweigt werden soll, wird die Variableninitialisierer daher nicht ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-268">Thus, if execution branches over the local declaration statement, the variable initializer is not executed.</span></span> <span data-ttu-id="ac6c8-269">Die Variableninitialisierer wird ausgeführt, wenn die Deklaration von lokalen-Anweisung mehr als einmal ausgeführt wird, eine gleiche Anzahl an, wie oft.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-269">If the local declaration statement is executed more than once, the variable initializer is executed an equal number of times.</span></span> <span data-ttu-id="ac6c8-270">Statische Variablen führen ihre Initialisierer nur beim ersten.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-270">Static variables only execute their initializer the first time.</span></span> <span data-ttu-id="ac6c8-271">Wenn eine Ausnahme tritt auf, bei der Initialisierung einer statischen Variablen, gilt die statische Variable mit dem Standardwert des Typs für die statische Variable initialisiert.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-271">If an exception occurs while initializing a static variable, the static variable is considered initialized with the default value of the static variable's type.</span></span>

<span data-ttu-id="ac6c8-272">Das folgende Beispiel zeigt die Verwendung der Initialisierer:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-272">The following example shows the use of initializers:</span></span>

```vb
Module Test
    Sub F()
        Static x As Integer = 5

        Console.WriteLine("Static variable x = " & x)
        x += 1
    End Sub

    Sub Main()
        Dim i As Integer

        For i = 1 to 3
            F()
        Next i

        i = 3
label:
        Dim y As Integer = 8

        If i > 0 Then
            Console.WriteLine("Local variable y = " & y)
            y -= 1
            i -= 1
            GoTo label
        End If
    End Sub
End Module
```

<span data-ttu-id="ac6c8-273">Dieses Programm druckt:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-273">This program prints:</span></span>

```
Static variable x = 5
Static variable x = 6
Static variable x = 7
Local variable y = 8
Local variable y = 8
Local variable y = 8
```

<span data-ttu-id="ac6c8-274">Initialisierer für statische lokale Variablen sind threadsicher und vor Ausnahmen geschützt, während der Initialisierung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-274">Initializers on static locals are thread-safe and protected against exceptions during initialization.</span></span> <span data-ttu-id="ac6c8-275">Wenn während einer statischen lokalen Initialisierung eine Ausnahme auftritt, wird der statische lokalen hat den Standardwert zurück, und nicht initialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-275">If an exception occurs during a static local initializer, the static local will have its default value and not be initialized.</span></span> <span data-ttu-id="ac6c8-276">Eine lokale statische Initialisierung</span><span class="sxs-lookup"><span data-stu-id="ac6c8-276">A static local initializer</span></span>

```vb
Module Test
    Sub F()
        Static x As Integer = 5
    End Sub
End Module
```

<span data-ttu-id="ac6c8-277">für die folgende Syntax:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-277">is equivalent to</span></span>

```vb
Imports System.Threading
Imports Microsoft.VisualBasic.CompilerServices

Module Test
    Class InitFlag
        Public State As Short
    End Class

    Private xInitFlag As InitFlag = New InitFlag()

    Sub F()
        Dim x As Integer

        If xInitFlag.State <> 1 Then
            Monitor.Enter(xInitFlag)
            Try
                If xInitFlag.State = 0 Then
                    xInitFlag.State = 2
                    x = 5
                Else If xInitFlag.State = 2 Then
                    Throw New IncompleteInitialization()
                End If
            Finally
                xInitFlag.State = 1
                Monitor.Exit(xInitFlag)
            End Try
        End If
    End Sub
End Module
```

<span data-ttu-id="ac6c8-278">Lokale Variablen, lokale Konstanten und statische Variablen beziehen sich auf die Anweisung blockiert wird, in dem sie deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-278">Local variables, local constants, and static variables are scoped to the statement block in which they are declared.</span></span> <span data-ttu-id="ac6c8-279">Statische Variablen sind spezielle, da ihre Namen nur einmal in die gesamte Methode verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-279">Static variables are special in that their names may only be used once throughout the entire method.</span></span> <span data-ttu-id="ac6c8-280">Beispielsweise ist es auf zwei Deklarationen mit statische Variable mit dem gleichen Namen zu geben, auch wenn sie in verschiedene Blöcke sind ungültig.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-280">For example, it is not valid to specify two static variable declarations with the same name even if they are in different blocks.</span></span>


### <a name="implicit-local-declarations"></a><span data-ttu-id="ac6c8-281">Implizite lokale Deklarationen</span><span class="sxs-lookup"><span data-stu-id="ac6c8-281">Implicit Local Declarations</span></span>

<span data-ttu-id="ac6c8-282">Zusätzlich zu Anweisungen, lokale Deklaration können lokale Variablen auch implizit durch die Verwendung deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-282">In addition to local declaration statements, local variables can also be declared implicitly through use.</span></span> <span data-ttu-id="ac6c8-283">Ein einfacher Name-Ausdruck, der einen Namen verwendet, der nicht auf etwas anderes behebt deklariert eine lokale Variable mit diesem Namen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-283">A simple name expression that uses a name that does not resolve to something else declares a local variable by that name.</span></span> <span data-ttu-id="ac6c8-284">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-284">For example:</span></span>

```vb
Option Explicit Off

Module Test
    Sub Main()
        x = 10
        y = 20
        Console.WriteLine(x + y)
    End Sub
End Module
```

<span data-ttu-id="ac6c8-285">Implizite Deklaration von lokalen tritt nur in ausdruckskontexten, die einen Ausdruck, der als Variable klassifiziert akzeptieren kann.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-285">Implicit local declaration only occurs in expression contexts that can accept an expression classified as a variable.</span></span> <span data-ttu-id="ac6c8-286">Die Ausnahme von dieser Regel ist, dass eine lokale Variable nicht implizit deklariert werden kann, wenn es das Ziel des Aufrufs Funktionsausdruck, Indizierung Ausdruck oder eines Memberzugriffsausdrucks ist.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-286">The exception to this rule is that a local variable may not be implicitly declared when it is the target of a function invocation expression, indexing expression, or a member access expression.</span></span>

<span data-ttu-id="ac6c8-287">Implizite "lokal" werden behandelt, als ob sie am Anfang der enthaltenden Methode deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-287">Implicit locals are treated as if they are declared at the beginning of the containing method.</span></span> <span data-ttu-id="ac6c8-288">Daher sind sie immer auf den gesamten Methodentext, beschränkt, auch wenn innerhalb eines Lambdaausdrucks deklariert.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-288">Thus, they are always scoped to the entire method body, even if declared inside of a lambda expression.</span></span> <span data-ttu-id="ac6c8-289">Beispielsweise folgender Code:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-289">For example, the following code:</span></span>

```vb
Option Explicit Off 

Module Test
    Sub Main()
        Dim x = Sub()
                    a = 10
                End Sub
        Dim y = Sub()
                    Console.WriteLine(a)
                End Sub

        x()
        y()
    End Sub
End Module
```

<span data-ttu-id="ac6c8-290">Gibt den Wert `10`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-290">will print the value `10`.</span></span> <span data-ttu-id="ac6c8-291">Implizite "lokal" werden als typisierte `Object` Wenn keine Typzeichen wurde angefügt, um den Namen der Variablen ist; andernfalls ist des Typs der Variablen den Typ des Typzeichen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-291">Implicit locals are typed as `Object` if no type character was attached to the variable name; otherwise the type of the variable is the type of the type character.</span></span> <span data-ttu-id="ac6c8-292">Lokale Variablen Typrückschluss wird nicht für implizite "lokal" verwendet.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-292">Local variable type inference is not used for implicit locals.</span></span>

<span data-ttu-id="ac6c8-293">Wenn explizite Deklaration von lokalen, durch die kompilierungsumgebung oder durch angegeben wird `Option Explicit`, alle lokale Variablen explizit deklariert werden müssen, und implizite Deklaration von Variablen ist nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-293">If explicit local declaration is specified by the compilation environment or by `Option Explicit`, all local variables must be explicitly declared and implicit variable declaration is disallowed.</span></span>

## <a name="with-statement"></a><span data-ttu-id="ac6c8-294">Mit-Anweisung</span><span class="sxs-lookup"><span data-stu-id="ac6c8-294">With Statement</span></span>

<span data-ttu-id="ac6c8-295">Ein `With` -Anweisung können mehrere Verweise auf die Member eines Ausdrucks ohne Angabe des Ausdrucks mehrmals.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-295">A `With` statement allows multiple references to an expression's members without specifying the expression multiple times.</span></span>

```antlr
WithStatement
    : 'With' Expression StatementTerminator
      Block?
      'End' 'With' StatementTerminator
    ;
```

<span data-ttu-id="ac6c8-296">Der Ausdruck als Wert klassifiziert werden muss und beim Eintritt in den Block einmal ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-296">The expression must be classified as a value and is evaluated once, upon entry into the block.</span></span> <span data-ttu-id="ac6c8-297">Innerhalb der `With` Anweisungsblock, ein Memberzugriffsausdruck oder den Ausdruck für den Wörterbuch ab, die mit einem Punkt oder einem Ausrufezeichen wird ausgewertet, als ob die `With` Ausdruck vorangestellt es.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-297">Within the `With` statement block, a member access expression or dictionary access expression starting with a period or an exclamation point is evaluated as if the `With` expression preceded it.</span></span> <span data-ttu-id="ac6c8-298">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-298">For example:</span></span>

```vb
Structure Test
    Public x As Integer

    Function F() As Integer
        Return 10
    End Sub
End Structure

Module TestModule
    Sub Main()
        Dim y As Test

        With y
            .x = 10
            Console.WriteLine(.x)
            .x = .F()
        End With
    End Sub
End Module
```

<span data-ttu-id="ac6c8-299">Es ist unzulässig, eine Verzweigung in einem `With` Anweisungsblock von außerhalb des Blocks.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-299">It is invalid to branch into a `With` statement block from outside of the block.</span></span>


## <a name="synclock-statement"></a><span data-ttu-id="ac6c8-300">SyncLock-Anweisung</span><span class="sxs-lookup"><span data-stu-id="ac6c8-300">SyncLock Statement</span></span>

<span data-ttu-id="ac6c8-301">Ein `SyncLock` -Anweisung können die Anweisungen auf einem Ausdruck, synchronisiert werden, der sicherstellt, dass mehrere Ausführungsthreads gleichzeitig nicht die gleichen Anweisungen ausführen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-301">A `SyncLock` statement allows statements to be synchronized on an expression, which ensures that multiple threads of execution do not execute the same statements at the same time.</span></span>

```antlr
SyncLockStatement
    : 'SyncLock' Expression StatementTerminator
      Block?
      'End' 'SyncLock' StatementTerminator
    ;
```

<span data-ttu-id="ac6c8-302">Der Ausdruck als Wert klassifiziert werden muss und beim Einstieg in den Block einmal ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-302">The expression must be classified as a value and is evaluated once, upon entry to the block.</span></span> <span data-ttu-id="ac6c8-303">Bei der Eingabe der `SyncLock` Block, der `Shared` Methode `System.Threading.Monitor.Enter` wird aufgerufen, in dem angegebenen Ausdruck, die blockiert, bis der Thread der Ausführung eine exklusive Sperre für das Objekt, das vom Ausdruck zurückgegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-303">When entering the `SyncLock` block, the `Shared` method `System.Threading.Monitor.Enter` is called on the specified expression, which blocks until the thread of execution has an exclusive lock on the object returned by the expression.</span></span> <span data-ttu-id="ac6c8-304">Der Typ des Ausdrucks in einer `SyncLock` -Anweisung muss ein Verweistyp sein.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-304">The type of the expression in a `SyncLock` statement must be a reference type.</span></span> <span data-ttu-id="ac6c8-305">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-305">For example:</span></span>

```vb
Class Test
    Private count As Integer = 0

    Public Function Add() As Integer
        SyncLock Me
            count += 1
            Add = count
        End SyncLock
    End Function

    Public Function Subtract() As Integer
        SyncLock Me
            count -= 1
            Subtract = count
        End SyncLock
    End Function
End Class
```

<span data-ttu-id="ac6c8-306">Das obige Beispiel synchronisiert wird, auf die jeweilige Instanz der Klasse `Test` um sicherzustellen, dass nicht mehr als einen Thread der Ausführung kann hinzufügen oder davon subtrahiert der Variablen "Count" zu einem Zeitpunkt für eine bestimmte Instanz.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-306">The example above synchronizes on the specific instance of the class `Test` to ensure that no more than one thread of execution can add or subtract from the count variable at a time for a particular instance.</span></span>

<span data-ttu-id="ac6c8-307">Die `SyncLock` Block implizit enthalten ist ein `Try` Anweisung, deren `Finally` Aufrufe der `Shared` Methode `System.Threading.Monitor.Exit` für den Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-307">The `SyncLock` block is implicitly contained by a `Try` statement whose `Finally` block calls the `Shared` method `System.Threading.Monitor.Exit` on the expression.</span></span> <span data-ttu-id="ac6c8-308">Dadurch wird sichergestellt, dass die Sperre aufgehoben wird, selbst wenn eine Ausnahme ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-308">This ensures the lock is freed even when an exception is thrown.</span></span> <span data-ttu-id="ac6c8-309">Daher ist ungültig in branch ein `SyncLock` Blockieren von außerhalb des Blocks auf, und ein `SyncLock` Block wird als einzelne Anweisung behandelt, im Rahmen `Resume` und `Resume Next`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-309">As a result, it is invalid to branch into a `SyncLock` block from outside of the block, and a `SyncLock` block is treated as a single statement for the purposes of `Resume` and `Resume Next`.</span></span> <span data-ttu-id="ac6c8-310">Im obige Beispiel entspricht dem folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-310">The above example is equivalent to the following code:</span></span>

```vb
Class Test
    Private count As Integer = 0

    Public Function Add() As Integer
        Try
            System.Threading.Monitor.Enter(Me)

            count += 1
            Add = count
        Finally
            System.Threading.Monitor.Exit(Me)
        End Try
    End Function

    Public Function Subtract() As Integer
        Try
            System.Threading.Monitor.Enter(Me)

            count -= 1
            Subtract = count
        Finally
            System.Threading.Monitor.Exit(Me)
        End Try
    End Function
End Class
```


## <a name="event-statements"></a><span data-ttu-id="ac6c8-311">Event-Anweisungen</span><span class="sxs-lookup"><span data-stu-id="ac6c8-311">Event Statements</span></span>

<span data-ttu-id="ac6c8-312">Die `RaiseEvent`, `AddHandler`, und `RemoveHandler` Anweisungen Ereignisse auslösen und Behandeln von Ereignissen dynamisch.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-312">The `RaiseEvent`, `AddHandler`, and `RemoveHandler` statements raise events and handle events dynamically.</span></span>

```antlr
EventStatement
    : RaiseEventStatement
    | AddHandlerStatement
    | RemoveHandlerStatement
    ;
```

### <a name="raiseevent-statement"></a><span data-ttu-id="ac6c8-313">RaiseEvent-Anweisung</span><span class="sxs-lookup"><span data-stu-id="ac6c8-313">RaiseEvent Statement</span></span>

<span data-ttu-id="ac6c8-314">Ein `RaiseEvent` Anweisung benachrichtigt, Ereignishandler, die ein bestimmtes Ereignis aufgetreten ist.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-314">A `RaiseEvent` statement notifies event handlers that a particular event has occurred.</span></span>

```antlr
RaiseEventStatement
    : 'RaiseEvent' IdentifierOrKeyword
      ( OpenParenthesis ArgumentList? CloseParenthesis )? StatementTerminator
    ;
```

<span data-ttu-id="ac6c8-315">Der einfache Namensausdruck in einer `RaiseEvent` Anweisung wird als eine Suche nach Membern auf interpretiert `Me`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-315">The simple name expression in a `RaiseEvent` statement is interpreted as a member lookup on `Me`.</span></span> <span data-ttu-id="ac6c8-316">Daher `RaiseEvent x` wird interpretiert, als wäre er `RaiseEvent Me.x`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-316">Thus, `RaiseEvent x` is interpreted as if it were `RaiseEvent Me.x`.</span></span> <span data-ttu-id="ac6c8-317">Das Ergebnis des Ausdrucks muss als Zugriffs-Ereignis für ein Ereignis in der Klasse selbst definierten klassifiziert werden; Ereignisse für Basistypen definierten können nicht verwendet werden, einem `RaiseEvent` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-317">The result of the expression must be classified as an event access for an event defined in the class itself; events defined on base types cannot be used in a `RaiseEvent` statement.</span></span>

<span data-ttu-id="ac6c8-318">Die `RaiseEvent` -Anweisung verarbeitet wird, als Aufruf an die `Invoke` Methode des Ereignisses-Delegaten, mit der bereitgestellten Parameter, sofern vorhanden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-318">The `RaiseEvent` statement is processed as a call to the `Invoke` method of the event's delegate, using the supplied parameters, if any.</span></span> <span data-ttu-id="ac6c8-319">Falls der Wert des Delegaten `Nothing`, wird keine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-319">If the delegate's value is `Nothing`, no exception is thrown.</span></span> <span data-ttu-id="ac6c8-320">Wenn keine Argumente vorhanden sind, können die Klammern weggelassen werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-320">If there are no arguments, the parentheses may be omitted.</span></span> <span data-ttu-id="ac6c8-321">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-321">For example:</span></span>

```vb
Class Raiser
    Public Event E1(Count As Integer)

    Public Sub Raise()
        Static RaiseCount As Integer = 0

        RaiseCount += 1
        RaiseEvent E1(RaiseCount)
    End Sub
End Class

Module Test
    Private WithEvents x As Raiser

    Private Sub E1Handler(Count As Integer) Handles x.E1
        Console.WriteLine("Raise #" & Count)
    End Sub

    Public Sub Main()
        x = New Raiser
        x.Raise()        ' Prints "Raise #1".
        x.Raise()        ' Prints "Raise #2".
        x.Raise()        ' Prints "Raise #3".
    End Sub
End Module
```

<span data-ttu-id="ac6c8-322">Die Klasse `Raiser` höher ist äquivalent zu:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-322">The class `Raiser` above is equivalent to:</span></span>

```vb
Class Raiser
    Public Event E1(Count As Integer)

    Public Sub Raise()
        Static RaiseCount As Integer = 0
        Dim TemporaryDelegate As E1EventHandler

        RaiseCount += 1

        ' Use a temporary to avoid a race condition.
        TemporaryDelegate = E1Event
        If Not TemporaryDelegate Is Nothing Then
            TemporaryDelegate.Invoke(RaiseCount)
        End If
    End Sub
End Class
```


### <a name="addhandler-and-removehandler-statements"></a><span data-ttu-id="ac6c8-323">AddHandler und RemoveHandler-Anweisungen</span><span class="sxs-lookup"><span data-stu-id="ac6c8-323">AddHandler and RemoveHandler Statements</span></span>

<span data-ttu-id="ac6c8-324">Obwohl die meisten Ereignishandler über automatisch eingebunden werden `WithEvents` Variablen, es kann erforderlich sein, dynamisch hinzufügen und Entfernen von Ereignishandlern zur Laufzeit.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-324">Although most event handlers are automatically hooked up through `WithEvents` variables, it may be necessary to dynamically add and remove event handlers at run time.</span></span> <span data-ttu-id="ac6c8-325">`AddHandler` und `RemoveHandler` Anweisungen dazu.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-325">`AddHandler` and `RemoveHandler` statements do this.</span></span>

```antlr
AddHandlerStatement
    : 'AddHandler' Expression Comma Expression StatementTerminator
    ;

RemoveHandlerStatement
    : 'RemoveHandler' Expression Comma Expression StatementTerminator
    ;
```

<span data-ttu-id="ac6c8-326">Jede Anweisung akzeptiert zwei Argumente: das erste Argument muss ein Ausdruck, der als ein Ereigniszugriff eingestuft wird und das zweite Argument muss ein Ausdruck, der als Wert klassifiziert wird.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-326">Each statement takes two arguments: the first argument must be an expression that is classified as an event access and the second argument must be an expression that is classified as a value.</span></span> <span data-ttu-id="ac6c8-327">Das zweite Argument Typ muss es sich um den Typ des Delegaten, mit der Ereignis verknüpft sein.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-327">The second argument's type must be the delegate type associated with the event access.</span></span> <span data-ttu-id="ac6c8-328">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-328">For example:</span></span>

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

<span data-ttu-id="ac6c8-329">Ein Ereignis `E,` die Anweisung ruft die relevante `add_E` oder `remove_E` Methode für die Instanz zum Hinzufügen oder entfernen den Delegaten als Handler für das Ereignis.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-329">Given an event `E,` the statement calls the relevant `add_E` or `remove_E` method on the instance to add or remove the delegate as a handler for the event.</span></span> <span data-ttu-id="ac6c8-330">Daher entspricht der obige Code:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-330">Thus, the above code is equivalent to:</span></span>

```vb
Public Class Form1
    Public Sub New()
        Button1.add_Click(AddressOf Button1_Click)
    End Sub 

    Private Button1 As Button = New Button()

    Sub Button1_Click(sender As Object, e As EventArgs)
        Console.WriteLine("Button1 was clicked!")
    End Sub

    Public Sub Disconnect()
        Button1.remove_Click(AddressOf Button1_Click)
    End Sub
End Class
```


## <a name="assignment-statements"></a><span data-ttu-id="ac6c8-331">Zuweisungsanweisungen</span><span class="sxs-lookup"><span data-stu-id="ac6c8-331">Assignment Statements</span></span>

<span data-ttu-id="ac6c8-332">Eine zuweisungsanweisung weist den Wert eines Ausdrucks zu einer Variablen an.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-332">An assignment statement assigns the value of an expression to a variable.</span></span> <span data-ttu-id="ac6c8-333">Es gibt mehrere Typen von Zuweisung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-333">There are several types of assignment.</span></span>

```antlr
AssignmentStatement
    : RegularAssignmentStatement
    | CompoundAssignmentStatement
    | MidAssignmentStatement
    ;
```

### <a name="regular-assignment-statements"></a><span data-ttu-id="ac6c8-334">Reguläre Zuweisungsanweisungen</span><span class="sxs-lookup"><span data-stu-id="ac6c8-334">Regular Assignment Statements</span></span>

<span data-ttu-id="ac6c8-335">Eine einfache zuweisungsanweisung speichert das Ergebnis eines Ausdrucks in einer Variablen an.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-335">A simple assignment statement stores the result of an expression in a variable.</span></span>

```antlr
RegularAssignmentStatement
    : Expression Equals Expression StatementTerminator
    ;
```

<span data-ttu-id="ac6c8-336">Während der Ausdruck auf der rechten Seite des Zuweisungsoperators als Wert klassifiziert werden muss, muss der Ausdruck auf der linken Seite des Zuweisungsoperators als Variable oder ein Eigenschaftenzugriff klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-336">The expression on the left side of the assignment operator must be classified as a variable or a property access, while the expression on the right side of the assignment operator must be classified as a value.</span></span> <span data-ttu-id="ac6c8-337">Der Typ des Ausdrucks muss implizit in den Typ des Zugriffs Variable oder eine Eigenschaft sein.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-337">The type of the expression must be implicitly convertible to the type of the variable or property access.</span></span>

<span data-ttu-id="ac6c8-338">Wenn die Variable zugewiesen wird, in ein Arrayelement einen Verweistyp handelt, wird eine laufzeitüberprüfung ausgeführt werden, um sicherzustellen, dass der Ausdruck mit dem Arrayelement Typ kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-338">If the variable being assigned into is an array element of a reference type, a run-time check will be performed to ensure that the expression is compatible with the array-element type.</span></span> <span data-ttu-id="ac6c8-339">Im folgenden Beispiel wird die letzte Zuweisung einer `System.ArrayTypeMismatchException` da ausgelöst wird eine Instanz von `ArrayList` kann nicht gespeichert werden, in einem Element des eine `String` Array.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-339">In the following example, the last assignment causes a `System.ArrayTypeMismatchException` to be thrown, because an instance of `ArrayList` cannot be stored in an element of a `String` array.</span></span>

```vb
Dim sa(10) As String
Dim oa As Object() = sa
oa(0) = Nothing         ' This is allowed.
oa(1) = "Hello"         ' This is allowed.
oa(2) = New ArrayList() ' System.ArrayTypeMismatchException is thrown.
```

<span data-ttu-id="ac6c8-340">Wenn der Ausdruck auf der linken Seite des Zuweisungsoperators als Variable klassifiziert ist, klicken Sie dann die zuweisungsanweisung den Wert in der Variablen gespeichert.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-340">If the expression on the left side of the assignment operator is classified as a variable, then the assignment statement stores the value in the variable.</span></span> <span data-ttu-id="ac6c8-341">Wenn der Ausdruck wird als Eigenschaftszugriff klassifiziert, und klicken Sie dann die zuweisungsanweisung der Eigenschaftenzugriff in einen Aufruf wandelt der `Set` -Accessor der Eigenschaft mit dem Wert für den Value-Parameter ersetzt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-341">If the expression is classified as a property access, then the assignment statement turns the property access into an invocation of the `Set` accessor of the property with the value substituted for the value parameter.</span></span> <span data-ttu-id="ac6c8-342">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-342">For example:</span></span>

```vb
Module Test
    Private PValue As Integer

    Public Property P As Integer
        Get
            Return PValue
        End Get

        Set (Value As Integer)
            PValue = Value
        End Set
    End Property

    Sub Main()
        ' The following two lines are equivalent.
        P = 10
        set_P(10)
    End Sub
End Module
```

<span data-ttu-id="ac6c8-343">Wenn das Ziel des Zugriffs Variable oder eine Eigenschaft ist als ein Wert eingegeben, aber nicht als Variable klassifiziert, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-343">If the target of the variable or property access is typed as a value type but not classified as a variable, a compile-time error occurs.</span></span> <span data-ttu-id="ac6c8-344">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-344">For example:</span></span>

```vb
Structure S
    Public F As Integer
End Structure

Class C
    Private PValue As S

    Public Property P As S
        Get
            Return PValue
        End Get

        Set (Value As S)
            PValue = Value
        End Set
    End Property
End Class

Module Test
    Sub Main()
        Dim ct As C = New C()
        Dim rt As Object = new C()

        ' Compile-time error: ct.P not classified as variable.
        ct.P.F = 10

        ' Run-time exception.
        rt.P.F = 10
    End Sub
End Module
```

<span data-ttu-id="ac6c8-345">Beachten Sie, dass die Semantik der Zuweisung, die für den Typ der Variablen oder Eigenschaft, die sie zugewiesen wird, ist, abhängig sind.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-345">Note that the semantics of the assignment depend on the type of the variable or property to which it is being assigned.</span></span> <span data-ttu-id="ac6c8-346">Wenn die Variable, die sie zugewiesen wird, ist, ein Werttyp ist, kopiert die Zuweisung den Wert des Ausdrucks in die Variable an.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-346">If the variable to which it is being assigned is a value type, the assignment copies the value of the expression into the variable.</span></span> <span data-ttu-id="ac6c8-347">Wenn die Variable, die sie zugewiesen wird, ist, ein Verweistyp ist, kopiert die Zuweisung den Verweis, nicht den Wert selbst, in die Variable an.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-347">If the variable to which it is being assigned is a reference type, the assignment copies the reference, not the value itself, into the variable.</span></span> <span data-ttu-id="ac6c8-348">Wenn der Typ der Variablen ist `Object`, die Zuweisungssemantik werden bestimmt, indem Sie, ob der Typ des Werts zur Laufzeit ein Werttyp oder ein Verweistyp ist.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-348">If the type of the variable is `Object`, the assignment semantics are determined by whether the value's type is a value type or a reference type at run time.</span></span>


<span data-ttu-id="ac6c8-349">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="ac6c8-349">__Note.__</span></span> <span data-ttu-id="ac6c8-350">Für systeminterne wie Typen `Integer` und `Date`, Verweis- und Zuweisungssemantik sind identisch, da die Typen unveränderlich sind.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-350">For intrinsic types such as `Integer` and `Date`, reference and value assignment semantics are the same because the types are immutable.</span></span> <span data-ttu-id="ac6c8-351">Daher ist die Sprache Zuweisung eines Verweises auf die systeminternen Typen als Optimierung verwenden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-351">As a result, the language is free to use reference assignment on boxed intrinsic types as an optimization.</span></span> <span data-ttu-id="ac6c8-352">Hinsichtlich der Wert ist das Ergebnis identisch.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-352">From a value perspective, the result is the same.</span></span>

<span data-ttu-id="ac6c8-353">Da das Gleichheitszeichen (`=`) wird verwendet, sowohl für Zuweisungen und Gleichheit, besteht eine Mehrdeutigkeit zwischen einer einfachen Zuweisung und einer aufrufanweisung in Situationen wie z. B. `x = y.ToString()`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-353">Because the equals character (`=`) is used both for assignment and for equality, there is an ambiguity between a simple assignment and an invocation statement in situations such as `x = y.ToString()`.</span></span> <span data-ttu-id="ac6c8-354">In allen diesen Fällen hat die zuweisungsanweisung Vorrang vor den Equality-Operator.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-354">In all such cases, the assignment statement takes precedence over the equality operator.</span></span> <span data-ttu-id="ac6c8-355">Dies bedeutet, dass der Beispielausdruck, als interpretiert wird `x = (y.ToString())` statt `(x = y).ToString()`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-355">This means that the example expression is interpreted as `x = (y.ToString())` rather than `(x = y).ToString()`.</span></span>


### <a name="compound-assignment-statements"></a><span data-ttu-id="ac6c8-356">Verbundanweisungen für Zuordnung</span><span class="sxs-lookup"><span data-stu-id="ac6c8-356">Compound Assignment Statements</span></span>

<span data-ttu-id="ac6c8-357">Ein *zusammengesetzte zuweisungsanweisung* hat das Format `V op= E` (, in denen `op` ist ein gültiger binärer Operator).</span><span class="sxs-lookup"><span data-stu-id="ac6c8-357">A *compound assignment statement* takes the form `V op= E` (where `op` is a valid binary operator).</span></span>

```antlr
CompoundAssignmentStatement
    : Expression CompoundBinaryOperator LineTerminator? Expression StatementTerminator
    ;

CompoundBinaryOperator
    : '^' '=' | '*' '=' | '/' '=' | '\\' '=' | '+' '=' | '-' '='
    | '&' '=' | '<' '<' '=' | '>' '>' '='
    ;
```

<span data-ttu-id="ac6c8-358">Der Ausdruck auf der linken Seite des Zuweisungsoperators muss als eine Variable oder Eigenschaftszugriff, klassifiziert werden, während der Ausdruck auf der rechten Seite des Zuweisungsoperators als Wert klassifiziert werden muss.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-358">The expression on the left side of the assignment operator must be classified as a variable or property access, while the expression on the right side of the assignment operator must be classified as a value.</span></span> <span data-ttu-id="ac6c8-359">Die verbundzuweisung-Anweisung entspricht der Anweisung `V = V op E` mit dem Unterschied, dass die Variable auf der linken Seite des Operators verbundzuweisung nur einmal ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-359">The compound assignment statement is equivalent to the statement `V = V op E` with the difference that the variable on the left side of the compound assignment operator is only evaluated once.</span></span> <span data-ttu-id="ac6c8-360">Das folgende Beispiel veranschaulicht diesen Unterschied:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-360">The following example demonstrates this difference:</span></span>

```vb
Module Test
    Function GetIndex() As Integer
        Console.WriteLine("Getting index")
        Return 1
    End Function

    Sub Main()
        Dim a(2) As Integer

        Console.WriteLine("Simple assignment")
        a(GetIndex()) = a(GetIndex()) + 1

        Console.WriteLine("Compound assignment")
        a(GetIndex()) += 1
    End Sub
End Module
```

<span data-ttu-id="ac6c8-361">Der Ausdruck `a(GetIndex())` wird zweimal für einfache Zuweisung jedoch nur ausgewertet, nachdem für die verbundzuweisung, also der Code ausgibt:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-361">The expression `a(GetIndex())` is evaluated twice for simple assignment but only once for compound assignment, so the code prints:</span></span>

```
Simple assignment
Getting index
Getting index
Compound assignment
Getting index
```


### <a name="mid-assignment-statement"></a><span data-ttu-id="ac6c8-362">Mid Zuweisungsanweisung</span><span class="sxs-lookup"><span data-stu-id="ac6c8-362">Mid Assignment Statement</span></span>

<span data-ttu-id="ac6c8-363">Ein `Mid` zuweisungsanweisung weist eine Zeichenfolge in eine andere Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-363">A `Mid` assignment statement assigns a string into another string.</span></span> <span data-ttu-id="ac6c8-364">Die linke Seite der Zuweisung enthält die gleiche Syntax wie ein Aufruf an die Funktion `Microsoft.VisualBasic.Strings.Mid`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-364">The left side of the assignment has the same syntax as a call to the function `Microsoft.VisualBasic.Strings.Mid`.</span></span>

```antlr
MidAssignmentStatement
    : 'Mid' '$'? OpenParenthesis Expression Comma Expression
      ( Comma Expression )? CloseParenthesis Equals Expression StatementTerminator
    ;
```

<span data-ttu-id="ac6c8-365">Das erste Argument ist das Ziel der Zuweisung und muss als eine Variable oder ein Eigenschaftenzugriff, dessen Typ ist implizit konvertierbar in und aus, klassifiziert `String`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-365">The first argument is the target of the assignment and must be classified as a variable or a property access whose type is implicitly convertible to and from `String`.</span></span> <span data-ttu-id="ac6c8-366">Der zweite Parameter ist die 1-basierte Anfangsposition an, in dem die Zuweisung in der Zielzeichenfolge beginnen soll, und muss als Wert, dessen Typ implizit in muss, klassifiziert werden, das entspricht `Integer`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-366">The second parameter is the 1-based start position that corresponds to where the assignment should begin in the target string and must be classified as a value whose type must be implicitly convertible to `Integer`.</span></span> <span data-ttu-id="ac6c8-367">Dritte optionale Parameter ist die Anzahl von Zeichen aus der rechten Seite Wert, der in der Zielzeichenfolge zugewiesen und muss als ein Wert, dessen Typ implizit in klassifiziert `Integer`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-367">The optional third parameter is the number of characters from the right-side value to assign into the target string and must be classified as a value whose type is implicitly convertible to `Integer`.</span></span> <span data-ttu-id="ac6c8-368">Die rechte Seite ist die Quellzeichenfolge und muss als ein Wert, dessen Typ implizit in klassifiziert `String`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-368">The right side is the source string and must be classified as a value whose type is implicitly convertible to `String`.</span></span> <span data-ttu-id="ac6c8-369">Rechts auf der Length-Parameter, abgeschnitten wird, wenn angegeben, und ersetzt die Zeichen in der linken Seite-Zeichenfolge, beginnend ab der Startposition.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-369">The right side is truncated to the length parameter, if specified, and replaces the characters in the left-side string, starting at the start position.</span></span> <span data-ttu-id="ac6c8-370">Wenn die rechte Seite Zeichenfolge weniger Zeichen als dritten Parameter enthalten, werden nur die Zeichen aus der Zeichenfolge rechts kopiert.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-370">If the right side string contained fewer characters than the third parameter, only the characters from the right side string will be copied.</span></span>

<span data-ttu-id="ac6c8-371">Das folgende Beispiel zeigt `ab123fg`:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-371">The following example displays `ab123fg`:</span></span>

```vb
Module Test
    Sub Main()
        Dim s1 As String = "abcdefg"
        Dim s2 As String = "1234567"

        Mid$(s1, 3, 3) = s2
        Console.WriteLine(s1)
    End Sub
End Module
```

<span data-ttu-id="ac6c8-372">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="ac6c8-372">__Note.__</span></span> <span data-ttu-id="ac6c8-373">`Mid` ist ein reserviertes Wort.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-373">`Mid` is not a reserved word.</span></span>


## <a name="invocation-statements"></a><span data-ttu-id="ac6c8-374">Aufruf-Anweisungen</span><span class="sxs-lookup"><span data-stu-id="ac6c8-374">Invocation Statements</span></span>

<span data-ttu-id="ac6c8-375">Eine aufrufanweisung Ruft eine Methode, die das optionale Schlüsselwort vorangestellt `Call`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-375">An invocation statement invokes a method preceded by the optional keyword `Call`.</span></span> <span data-ttu-id="ac6c8-376">Der aufrufanweisung wird in die gleiche Weise wie der Aufrufausdruck Funktion, mit einigen Unterschieden, die unten aufgeführten verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-376">The invocation statement is processed in the same way as the function invocation expression, with some differences noted below.</span></span> <span data-ttu-id="ac6c8-377">Der Aufrufausdruck muss als Wert oder "void" klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-377">The invocation expression must be classified as a value or void.</span></span> <span data-ttu-id="ac6c8-378">Jeder Wert aus der Auswertung der Aufrufausdruck wird verworfen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-378">Any value resulting from the evaluation of the invocation expression is discarded.</span></span>

<span data-ttu-id="ac6c8-379">Wenn die `Call` -Schlüsselwort ausgelassen wird, und klicken Sie dann der Aufrufausdruck muss ein Bezeichner oder ein Schlüsselwort oder mit beginnen `.` innerhalb einer `With` Block.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-379">If the `Call` keyword is omitted, then the invocation expression must start with an identifier or keyword, or with `.` inside a `With` block.</span></span> <span data-ttu-id="ac6c8-380">Daher, z. B. "`Call 1.ToString()`" eine gültige Anweisung jedoch "`1.ToString()`" nicht.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-380">Thus, for instance, "`Call 1.ToString()`" is a valid statement but "`1.ToString()`" is not.</span></span> <span data-ttu-id="ac6c8-381">(Beachten Sie, dass in einem Ausdruckskontext Aufrufausdrücke auch nicht mit einem Bezeichner beginnen müssen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-381">(Note that in an expression context, invocation expressions also need not start with an identifier.</span></span> <span data-ttu-id="ac6c8-382">Z. B. "`Dim x = 1.ToString()`" eine gültige Anweisung).</span><span class="sxs-lookup"><span data-stu-id="ac6c8-382">For example, "`Dim x = 1.ToString()`" is a valid statement).</span></span>

<span data-ttu-id="ac6c8-383">Es ist ein weiterer Unterschied zwischen dem Aufruf Anweisungen und das Aufrufen von Ausdrücken: Falls eine aufrufanweisung enthält eine Argumentliste, dies immer als der Argumentliste des Aufrufs verwendet.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-383">There is another difference between the invocation statements and invocation expressions: if an invocation statement includes an argument list, then this is always taken as the argument list of the invocation.</span></span> <span data-ttu-id="ac6c8-384">Das folgende Beispiel veranschaulicht den Unterschied:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-384">The following example illustrates the difference:</span></span>

```vb
Module Test
    Sub Main()
        Call {Function() 15}(0)
        ' error: (0) is taken as argument list, but array is not invokable

        Call ({Function() 15}(0))
        ' valid, since the invocation statement has no argument list

        Dim x = {Function() 15}(0)
        ' valid as an expression, since (0) is taken as an array-indexing

        Call f("a")
        ' error: ("a") is taken as argument list to the invocation of f

        Call f()("a")
        ' valid, since () is the argument list for the invocation of f

        Dim y = f("a")
        ' valid as an expression, since f("a") is interpreted as f()("a")
    End Sub

    Sub f() As Func(Of String,String)
        Return Function(x) x
    End Sub
End Module
```

```antlr
InvocationStatement
    : 'Call'? InvocationExpression StatementTerminator
    ;
```

## <a name="conditional-statements"></a><span data-ttu-id="ac6c8-385">Bedingungsanweisungen</span><span class="sxs-lookup"><span data-stu-id="ac6c8-385">Conditional Statements</span></span>

<span data-ttu-id="ac6c8-386">Bedingte Anweisungen ermöglichen das bedingte Ausführung von Anweisungen, die basierend auf Ausdrücken, die zur Laufzeit ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-386">Conditional statements allow conditional execution of statements based on expressions evaluated at run time.</span></span>

```antlr
ConditionalStatement
    : IfStatement
    | SelectStatement
    ;
```

### <a name="ifthenelse-statements"></a><span data-ttu-id="ac6c8-387">If... Klicken Sie dann... Else-Anweisungen</span><span class="sxs-lookup"><span data-stu-id="ac6c8-387">If...Then...Else Statements</span></span>

<span data-ttu-id="ac6c8-388">Ein `If...Then...Else` ist die grundlegende bedingten Anweisung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-388">An `If...Then...Else` statement is the basic conditional statement.</span></span>

```antlr
IfStatement
    : BlockIfStatement
    | LineIfThenStatement
    ;

BlockIfStatement
    : 'If' BooleanExpression 'Then'? StatementTerminator
      Block?
      ElseIfStatement*
      ElseStatement?
      'End' 'If' StatementTerminator
    ;

ElseIfStatement
    : ElseIf BooleanExpression 'Then'? StatementTerminator
      Block?
    ;

ElseStatement
    : 'Else' StatementTerminator
      Block?
    ;

LineIfThenStatement
    : 'If' BooleanExpression 'Then' Statements ( 'Else' Statements )? StatementTerminator
    ;
    
ElseIf      
    : 'ElseIf'      
    | 'Else' 'If'   
   ;
```

<span data-ttu-id="ac6c8-389">Jeder Ausdruck in einem `If...Then...Else` Anweisung muss einem booleschen Ausdruck gemäß Abschnitt [boolesche Ausdrücke](expressions.md#boolean-expressions).</span><span class="sxs-lookup"><span data-stu-id="ac6c8-389">Each expression in an `If...Then...Else` statement must be a Boolean expression, as per Section [Boolean Expressions](expressions.md#boolean-expressions).</span></span> <span data-ttu-id="ac6c8-390">(Hinweis: Dies erfordert keine den Ausdruck, der Boolesch sein).</span><span class="sxs-lookup"><span data-stu-id="ac6c8-390">(Note: this does not require the expression to have Boolean type).</span></span> <span data-ttu-id="ac6c8-391">Wenn der Ausdruck in der `If` -Anweisung ist "true", die Anweisungen, die von der `If` Block ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-391">If the expression in the `If` statement is true, the statements enclosed by the `If` block are executed.</span></span> <span data-ttu-id="ac6c8-392">Wenn der Ausdruck "false" ist jede der `ElseIf` Ausdrücke ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-392">If the expression is false, each of the `ElseIf` expressions is evaluated.</span></span> <span data-ttu-id="ac6c8-393">Wenn eine von der `ElseIf` Ausdrücke, die auf "true" ausgewertet wird, der entsprechenden Block ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-393">If one of the `ElseIf` expressions evaluates to true, the corresponding block is executed.</span></span> <span data-ttu-id="ac6c8-394">Wenn kein Ausdruck wird zu True ausgewertet, und es gibt eine `Else` Block, der `Else` Block wird ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-394">If no expression evaluates to true and there is an `Else` block, the `Else` block is executed.</span></span> <span data-ttu-id="ac6c8-395">Nach ein-Block die Ausführung abgeschlossen ist, übergibt die Ausführung bis zum Ende der `If...Then...Else` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-395">Once a block finishes executing, execution passes to the end of the `If...Then...Else` statement.</span></span>

<span data-ttu-id="ac6c8-396">Die Zeilenversion des der `If` Anweisung verfügt über einen Satz von Anweisungen ausgeführt werden, wenn die `If` Ausdruck ist `True` und einer optionalen Gruppe von Anweisungen, die ausgeführt werden, wenn der Ausdruck ist `False`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-396">The line version of the `If` statement has a single set of statements to be executed if the `If` expression is `True` and an optional set of statements to be executed if the expression is `False`.</span></span> <span data-ttu-id="ac6c8-397">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-397">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim a As Integer = 10
        Dim b As Integer = 20

        ' Block If statement.
        If a < b Then
            a = b
        Else
            b = a
        End If

        ' Line If statement
        If a < b Then a = b Else b = a
    End Sub
End Module
```

<span data-ttu-id="ac6c8-398">Die Zeilenversion zurück, wenn die Anweisung bindet weniger streng als ":", und die zugehörige `Else` bindet an die lexikalisch am nächsten vorangehenden `If` , durch die Syntax zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-398">The line version of the If statement binds less tightly than ":", and its `Else` binds to the lexically nearest preceding `If` that is allowed by the syntax.</span></span> <span data-ttu-id="ac6c8-399">Die folgenden beiden Versionen sind beispielsweise äquivalent:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-399">For example, the following two versions are equivalent:</span></span>

```vb
If True Then _
If True Then Console.WriteLine("a") Else Console.WriteLine("b") _
Else Console.WriteLine("c") : Console.WriteLine("d")

If True Then
    If True Then
        Console.WriteLine("a")
    Else
        Console.WriteLine("b")
    End If
    Console.WriteLine("c") : Console.WriteLine("d")
End If
```

<span data-ttu-id="ac6c8-400">Alle Anweisungen als Bezeichnung deklarationsanweisungen sind innerhalb einer Zeile zulässig `If` -Anweisung, einschließlich der Block von Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-400">All statements other than label declaration statements are allowed inside a line `If` statement, including block statements.</span></span> <span data-ttu-id="ac6c8-401">Sie können jedoch nicht verwenden LineTerminators als StatementTerminators außer in mehrzeiligen Lambda-Ausdrücke.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-401">However, they may not use LineTerminators as StatementTerminators except inside multi-line lambda expressions.</span></span> <span data-ttu-id="ac6c8-402">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-402">For example:</span></span>

```vb
' Allowed, since it uses : instead of LineTerminator to separate statements
If b Then With New String("a"(0),5) : Console.WriteLine(.Length) : End With

' Disallowed, since it uses a LineTerminator
If b then With New String("a"(0), 5)
              Console.WriteLine(.Length)
          End With

' Allowed, since it only uses LineTerminator inside a multi-line lambda
If b Then Call Sub()
                   Console.WriteLine("a")
               End Sub.Invoke()
```

### <a name="select-case-statements"></a><span data-ttu-id="ac6c8-403">Select Case-Anweisungen</span><span class="sxs-lookup"><span data-stu-id="ac6c8-403">Select Case Statements</span></span>

<span data-ttu-id="ac6c8-404">Ein `Select Case` -Anweisung führt Anweisungen basierend auf dem Wert eines Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-404">A `Select Case` statement executes statements based on the value of an expression.</span></span>

```antlr
SelectStatement
    : 'Select' 'Case'? Expression StatementTerminator
      CaseStatement*
      CaseElseStatement?
      'End' 'Select' StatementTerminator
    ;

CaseStatement
    : 'Case' CaseClauses StatementTerminator
      Block?
    ;

CaseClauses
    : CaseClause ( Comma CaseClause )*
    ;

CaseClause
    : ( 'Is' LineTerminator? )? ComparisonOperator LineTerminator? Expression
    | Expression ( 'To' Expression )?
    ;

ComparisonOperator
    : '=' | '<' '>' | '<' | '>' | '>' '=' | '<' '='
    ;

CaseElseStatement
    : 'Case' 'Else' StatementTerminator
      Block?
    ;
```

<span data-ttu-id="ac6c8-405">Der Ausdruck muss als Wert klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-405">The expression must be classified as a value.</span></span> <span data-ttu-id="ac6c8-406">Wenn eine `Select Case` -Anweisung wird ausgeführt, die `Select` Ausdrucks zuerst ausgewertet, und die `Case` Anweisungen werden in der Reihenfolge der Text der Deklaration ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-406">When a `Select Case` statement is executed, the `Select` expression is evaluated first, and the `Case` statements are then evaluated in order of textual declaration.</span></span> <span data-ttu-id="ac6c8-407">Die erste `Case` Anweisung, die überprüft `True` verfügt über einen Block ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-407">The first `Case` statement that evaluates to `True` has its block executed.</span></span> <span data-ttu-id="ac6c8-408">Wenn kein `Case` Anweisung ergibt `True` und es gibt eine `Case Else` -Anweisung, der Block ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-408">If no `Case` statement evaluates to `True` and there is a `Case Else` statement, that block is executed.</span></span> <span data-ttu-id="ac6c8-409">Nach der Ausführung ein Blocks beendet hat, übergibt die Ausführung bis zum Ende der `Select` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-409">Once a block has finished executing, execution passes to the end of the `Select` statement.</span></span>

<span data-ttu-id="ac6c8-410">Ausführung einer `Case` Block ist auf "mit dem nächsten Switch-Abschnitt fortfahren" nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-410">Execution of a `Case` block is not permitted to "fall through" to the next switch section.</span></span> <span data-ttu-id="ac6c8-411">Dies verhindert, dass eine allgemeine Klasse von Fehlern, die auftreten, in anderen Sprachen bei der ein `Case` Anweisung beenden versehentlich ausgelassen wird.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-411">This prevents a common class of bugs that occur in other languages when a `Case` terminating statement is accidentally omitted.</span></span> <span data-ttu-id="ac6c8-412">Dieses Verhalten wird im folgenden Beispiel veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-412">The following example illustrates this behavior:</span></span>

```vb
Module Test
    Sub Main()
        Dim x As Integer = 10

        Select Case x
            Case 5
                Console.WriteLine("x = 5")
            Case 10
                Console.WriteLine("x = 10")
            Case 20 - 10
                Console.WriteLine("x = 20 - 10")
            Case 30
                Console.WriteLine("x = 30")
        End Select
    End Sub
End Module
```

<span data-ttu-id="ac6c8-413">Vom Code ausgegeben:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-413">The code prints:</span></span>

```
x = 10
```

<span data-ttu-id="ac6c8-414">Obwohl `Case 10` und `Case 20 - 10` Option für den gleichen Wert `Case 10` wird ausgeführt, da ihm vorausgeht `Case 20 - 10` textlich.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-414">Although `Case 10` and `Case 20 - 10` select for the same value, `Case 10` is executed because it precedes `Case 20 - 10` textually.</span></span> <span data-ttu-id="ac6c8-415">Wenn die nächste `Case` wird erreicht, die Ausführung wird fortgesetzt, nachdem die `Select` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-415">When the next `Case` is reached, execution continues after the `Select` statement.</span></span>

<span data-ttu-id="ac6c8-416">Ein `Case` -Klausel zwei Formen annehmen kann.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-416">A `Case` clause may take two forms.</span></span> <span data-ttu-id="ac6c8-417">Ein Formular ist eine optionale `Is` Schlüsselwort, ein Vergleichsoperator und einen Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-417">One form is an optional `Is` keyword, a comparison operator, and an expression.</span></span> <span data-ttu-id="ac6c8-418">Der Ausdruck wird in den Typ des konvertiert die `Select` Ausdruck; Wenn der Ausdruck nicht implizit in den Typ des der `Select` Ausdruck ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-418">The expression is converted to the type of the `Select` expression; if the expression is not implicitly convertible to the type of the `Select` expression, a compile-time error occurs.</span></span> <span data-ttu-id="ac6c8-419">Wenn die `Select` Ausdruck ist *E*, der Vergleichsoperator ist *Op*, und die `Case` Ausdruck *E1*, wird die Groß-/Kleinschreibung als bewertet *E OP E1*.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-419">If the `Select` expression is *E*, the comparison operator is *Op*, and the `Case` expression is *E1*, the case is evaluated as *E OP E1*.</span></span> <span data-ttu-id="ac6c8-420">Der Operator muss für die Datentypen der beiden Ausdrücke gültig sein; andernfalls tritt ein Kompilierungsfehler auf.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-420">The operator must be valid for the types of the two expressions; otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="ac6c8-421">Die andere Form ist ein Ausdruck, optional gefolgt von dem Schlüsselwort `To` und einen zweiten Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-421">The other form is an expression optionally followed by the keyword `To` and a second expression.</span></span> <span data-ttu-id="ac6c8-422">Beide Ausdrücke werden in den Typ des konvertiert die `Select` Ausdruck; ist einer der Ausdrücke nicht implizit in den Typ des der `Select` Ausdruck ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-422">Both expressions are converted to the type of the `Select` expression; if either expression is not implicitly convertible to the type of the `Select` expression, a compile-time error occurs.</span></span> <span data-ttu-id="ac6c8-423">Wenn die `Select` Ausdruck ist `E`, die erste `Case` Ausdruck ist `E1`, und das zweite `Case` Ausdruck ist `E2`, `Case` wird ausgewertet, entweder als `E = E1` (wenn keine `E2`angegeben ist) oder `(E >= E1) And (E <= E2)`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-423">If the `Select` expression is `E`, the first `Case` expression is `E1`, and the second `Case` expression is `E2`, the `Case` is evaluated either as `E = E1` (if no `E2` is specified) or `(E >= E1) And (E <= E2)`.</span></span> <span data-ttu-id="ac6c8-424">Die Operatoren müssen für die Datentypen der beiden Ausdrücke gültig sein. andernfalls tritt ein Kompilierungsfehler auf.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-424">The operators must be valid for the types of the two expressions; otherwise a compile-time error occurs.</span></span>


## <a name="loop-statements"></a><span data-ttu-id="ac6c8-425">Schleifenanweisungen</span><span class="sxs-lookup"><span data-stu-id="ac6c8-425">Loop Statements</span></span>

<span data-ttu-id="ac6c8-426">Schleifenanweisungen ermöglichen die wiederholte Ausführung der Anweisungen in ihrem Textkörper.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-426">Loop statements allow repeated execution of the statements in their body.</span></span>

```antlr
LoopStatement
    : WhileStatement
    | DoLoopStatement
    | ForStatement
    | ForEachStatement
    ;
```

<span data-ttu-id="ac6c8-427">Jedes Mal, wenn ein Schleifenkörper eingegeben wird, erfolgt eine neue Kopie aller lokalen Variablen, die in dieses Texts, auf die vorherigen Werte der Variablen initialisiert deklariert.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-427">Each time a loop body is entered, a fresh copy is made of all local variables declared in that body, initialized to the previous values of the variables.</span></span> <span data-ttu-id="ac6c8-428">Jeder Verweis auf eine Variable innerhalb der Schleife wird die am häufigsten vor kurzem wurden Kopie verwendet.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-428">Any reference to a variable within the loop body will use the most recently made copy.</span></span> <span data-ttu-id="ac6c8-429">Dieser Code zeigt ein Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-429">This code shows an example:</span></span>

```vb
Module Test
    Sub Main()
        Dim lambdas As New List(Of Action)
        Dim x = 1

        For i = 1 To 3
            x = i
            Dim y = x
            lambdas.Add(Sub() Console.WriteLine(x & y))
        Next

        For Each lambda In lambdas
            lambda()
        Next
    End Sub
End Module
```

<span data-ttu-id="ac6c8-430">Der Code erzeugt die Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-430">The code produces the output:</span></span>

```
31    32    33
```

<span data-ttu-id="ac6c8-431">Wenn der Inhalt der Schleife ausgeführt wird, wird welcher Kopie der Variable aktuell ist.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-431">When the loop body is executed, it uses whichever copy of the variable is current.</span></span> <span data-ttu-id="ac6c8-432">Z. B. die Anweisung `Dim y = x` bezieht sich auf die letzte Kopie `y` und die ursprüngliche Version der `x`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-432">For example, the statement  `Dim y = x` refers to the latest copy of `y` and the original copy of `x`.</span></span> <span data-ttu-id="ac6c8-433">Gespeichert werden und wenn ein Lambda-Ausdruck erstellt wird, welche Kopie einer Variablen zum Zeitpunkt wurde, die sie erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-433">And when a lambda is created, it remembers whichever copy of a variable was current at the time it was created.</span></span> <span data-ttu-id="ac6c8-434">Aus diesem Grund jede Lambda verwendet die gleiche freigegebene Kopie `x`, aber eine andere Kopie der `y`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-434">Therefore, each lambda uses the same shared copy of `x`, but a different copy of `y`.</span></span> <span data-ttu-id="ac6c8-435">Am Ende des Programms, wenn sie den Lambda-Ausdrücke ausgeführt wird, gemeinsam genutzt Kopie `x` , dass sie alle auf verweisen befindet sich nun bei den endgültigen Wert 3.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-435">At the end of the program, when it executes the lambdas, that shared copy of `x` that they all refer to is now at its final value 3.</span></span>

<span data-ttu-id="ac6c8-436">Beachten Sie, wenn kein Lambda-Ausdrücke oder eine LINQ-Ausdrücke vorhanden sind, dann ist es unmöglich zu wissen, dass eine neue Kopie auf Schleife Eintrag vorgenommen wird.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-436">Note that if there are no lambdas or LINQ expressions, then it's impossible to know that a fresh copy is made on loop entry.</span></span> <span data-ttu-id="ac6c8-437">In der Tat compileroptimierungen vermieden werden Kopien in diesem Fall.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-437">Indeed, compiler optimizations will avoid making copies in this case.</span></span> <span data-ttu-id="ac6c8-438">Beachten Sie auch, dass es nicht zulässig, `GoTo` in einer Schleife, die Lambda-Ausdrücken oder LINQ-Ausdrücke enthält.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-438">Note too that it's illegal to `GoTo` into a loop that contains lambdas or LINQ expressions.</span></span>


### <a name="whileend-while-and-doloop-statements"></a><span data-ttu-id="ac6c8-439">Während... End beim und möchten... Schleifenanweisungen</span><span class="sxs-lookup"><span data-stu-id="ac6c8-439">While...End While and Do...Loop Statements</span></span>

<span data-ttu-id="ac6c8-440">Ein `While` oder `Do` Schleife von anweisungsschleifen basierend auf einem booleschen Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-440">A `While` or `Do` loop statement loops based on a Boolean expression.</span></span>

```antlr
WhileStatement
    : 'While' BooleanExpression StatementTerminator
      Block?
      'End' 'While' StatementTerminator
    ;

DoLoopStatement
    : DoTopLoopStatement
    | DoBottomLoopStatement
    ;

DoTopLoopStatement
    : 'Do' ( WhileOrUntil BooleanExpression )? StatementTerminator
      Block?
      'Loop' StatementTerminator
    ;

DoBottomLoopStatement
    : 'Do' StatementTerminator
      Block?
      'Loop' WhileOrUntil BooleanExpression StatementTerminator
    ;

WhileOrUntil
    : 'While' | 'Until'
    ;
```

<span data-ttu-id="ac6c8-441">Ein `While` Schleifenanweisung Schleifen, solange der boolesche Ausdruck True ergibt; eine `Do` Loop-Anweisung kann eine komplexere Bedingung enthalten.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-441">A `While` loop statement loops as long as the Boolean expression evaluates to true; a `Do` loop statement may contain a more complex condition.</span></span> <span data-ttu-id="ac6c8-442">Nach ein Ausdruck platziert werden die `Do` Schlüsselwort oder nach der `Loop` -Schlüsselwort, aber nicht beide nach.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-442">An expression may be placed after the `Do` keyword or after the `Loop` keyword, but not after both.</span></span> <span data-ttu-id="ac6c8-443">Der boolesche Ausdruck wird ausgewertet, gemäß Abschnitt [boolesche Ausdrücke](expressions.md#boolean-expressions).</span><span class="sxs-lookup"><span data-stu-id="ac6c8-443">The Boolean expression is evaluated as per Section [Boolean Expressions](expressions.md#boolean-expressions).</span></span> <span data-ttu-id="ac6c8-444">(Hinweis: Dies erfordert keine den Ausdruck, der Boolesch sein).</span><span class="sxs-lookup"><span data-stu-id="ac6c8-444">(Note: this does not require the expression to have Boolean type).</span></span> <span data-ttu-id="ac6c8-445">Es ist auch zulässig, die kein Ausdruck angeben. In diesem Fall wird die Schleife nie beendet werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-445">It is also valid to specify no expression at all; in that case, the loop will never exit.</span></span> <span data-ttu-id="ac6c8-446">Wenn der Ausdruck, nach dem platziert wird `Do`, es vor der Ausführung des Schleifenblocks bei jeder Iteration ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-446">If the expression is placed after `Do`, it will be evaluated before the loop block is executed on each iteration.</span></span> <span data-ttu-id="ac6c8-447">Wenn der Ausdruck, nach dem platziert wird `Loop`, es nach dem Ausführen des Schleifenblocks bei jeder Iteration ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-447">If the expression is placed after `Loop`, it will be evaluated after the loop block has executed on each iteration.</span></span> <span data-ttu-id="ac6c8-448">Platzieren den Ausdruck nach `Loop` generiert daher eine weitere Schleife als Platzierung nach dem `Do`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-448">Placing the expression after `Loop` will therefore generate one more loop than placement after `Do`.</span></span> <span data-ttu-id="ac6c8-449">Das folgende Beispiel veranschaulicht dieses Verhalten:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-449">The following example demonstrates this behavior:</span></span>

```vb
Module Test
    Sub Main()
        Dim x As Integer

        x = 3
        Do While x = 1
            Console.WriteLine("First loop")
        Loop

        Do
            Console.WriteLine("Second loop")
        Loop While x = 1
    End Sub
End Module
```

<span data-ttu-id="ac6c8-450">Der Code erzeugt die Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-450">The code produces the output:</span></span>

```
Second Loop
```

<span data-ttu-id="ac6c8-451">Bei der ersten Schleife wird die Bedingung ausgewertet, bevor die Schleife ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-451">In the case of the first loop, the condition is evaluated before the loop executes.</span></span> <span data-ttu-id="ac6c8-452">Bei der zweiten Schleife wird die Bedingung ausgeführt, nachdem die Schleife wird ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-452">In the case of the second loop, the condition is executed after the loop executes.</span></span> <span data-ttu-id="ac6c8-453">Der bedingte Ausdruck muss mit einem Präfix einen `While` Schlüsselwort oder einem `Until` Schlüsselwort.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-453">The conditional expression must be prefixed with either a `While` keyword or an `Until` keyword.</span></span> <span data-ttu-id="ac6c8-454">Die erste unterbricht die Schleife, ergibt die Bedingung auf "false", der zweite Wert wird bei der Bedingung "true" ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-454">The former breaks the loop if the condition evaluates to false, the latter when the condition evaluates to true.</span></span>

<span data-ttu-id="ac6c8-455">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="ac6c8-455">__Note.__</span></span> <span data-ttu-id="ac6c8-456">`Until` ist ein reserviertes Wort.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-456">`Until` is not a reserved word.</span></span>


### <a name="fornext-statements"></a><span data-ttu-id="ac6c8-457">Für... Nächste Anweisungen</span><span class="sxs-lookup"><span data-stu-id="ac6c8-457">For...Next Statements</span></span>

<span data-ttu-id="ac6c8-458">Ein `For...Next` anweisungsschleifen basierend auf einem Satz von Grenzen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-458">A `For...Next` statement loops based on a set of bounds.</span></span> <span data-ttu-id="ac6c8-459">Ein `For` -Anweisung gibt, eine Schleifensteuerungsvariable, einen Ausdruck für die untere Grenze, einen Ausdruck für die obere Grenze und ein optionaler Schritt-Wert-Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-459">A `For` statement specifies a loop control variable, a lower bound expression, an upper bound expression, and an optional step value expression.</span></span> <span data-ttu-id="ac6c8-460">Die Loop-Steuerelementvariable angegeben ist, entweder über einen Bezeichner gefolgt von einem optionalen `As` -Klausel oder einen Ausdruck ein.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-460">The loop control variable is specified either through an identifier followed by an optional `As` clause or an expression.</span></span>

```antlr
ForStatement
    : 'For' LoopControlVariable Equals Expression 'To' Expression
      ( 'Step' Expression )? StatementTerminator
      Block?
      ( 'Next' NextExpressionList? StatementTerminator )?
    ;

LoopControlVariable
    : Identifier ( IdentifierModifiers 'As' TypeName )?
    | Expression
    ;

NextExpressionList
    : Expression ( Comma Expression )*
    ;
```

<span data-ttu-id="ac6c8-461">Gemäß den folgenden Regeln, die Schleifensteuerungsvariable bezieht sich entweder um eine neue lokale Variable, die bestimmten dieser `For...Next` -Anweisung oder eine vorhandene Variable oder einen Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-461">As per the following rules, the loop control variable refers either to a new local variable specific to this `For...Next` statement, or to a pre-existing variable, or to an expression.</span></span>

* <span data-ttu-id="ac6c8-462">Die Loop-Steuerelementvariable ist ein Bezeichner mit einer `As` -Klausel der Bezeichner definiert eine neue lokale Variable des Typs im angegebenen die `As` -Klausel, beschränkt auf die gesamte `For` Schleife.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-462">If the loop control variable is an identifier with an `As` clause, the identifier defines a new local variable of the type specified in the `As` clause, scoped to the entire `For` loop.</span></span>

* <span data-ttu-id="ac6c8-463">Die Loop-Steuerelementvariable ist eine ID ohne eine `As` -Klausel, und klicken Sie dann auf den Bezeichner wird zuerst aufgelöst, mit der einfachen Regeln zur namensauflösung (finden Sie im Abschnitt [einfache Namen Ausdrücke](expressions.md#simple-name-expressions)), ausgenommen, die dieses Auftretens von Der Bezeichner nicht in und von sich selbst würde eine implizite lokale Variable erstellt werden soll (Abschnitt [implizite lokale Deklarationen](statements.md#implicit-local-declarations)).</span><span class="sxs-lookup"><span data-stu-id="ac6c8-463">If the loop control variable is an identifier without an `As` clause, then the identifier is first resolved using the simple name resolution rules (see Section [Simple Name Expressions](expressions.md#simple-name-expressions)), excepting that this occurrence of the identifier would not in and of itself cause an implicit local variable to be created (Section [Implicit Local Declarations](statements.md#implicit-local-declarations)).</span></span>

  * <span data-ttu-id="ac6c8-464">Wenn diese Auflösung erfolgreich ist, und das Ergebnis wird als Variable klassifiziert, erfolgt die Loop-Steuerelementvariable, bereits vorhandene Variablen ab.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-464">If this resolution succeeds and the result is classified as a variable, then the loop control variable is that pre-existing variable.</span></span>

  * <span data-ttu-id="ac6c8-465">Wenn die Auflösung fehlschlägt oder wenn die Auflösung erfolgreich ist, und klicken Sie dann das Ergebnis wird als Typ klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-465">If resolution fails, or if resolution succeeds and the result is classified as a type, then:</span></span>
    * <span data-ttu-id="ac6c8-466">Wenn lokale Variable Typrückschluss verwendet wird, definiert der Bezeichner eine neue lokale Variable, deren Typ aus der Grenze abgeleitet ist, und der step-Ausdrücke, die den gesamten Bereich `For` -Schleife</span><span class="sxs-lookup"><span data-stu-id="ac6c8-466">if local variable type inference is being used, the identifier defines a new local variable whose type is inferred from the bound and step expressions, scoped to the entire `For` loop;</span></span>
    * <span data-ttu-id="ac6c8-467">Wenn nicht lokale Variablen Typrückschluss verwendet wird, aber implizite Deklaration von lokalen, wird eine implizite lokale Variable erstellt wird, dessen Bereich die gesamte Methode ist (Abschnitt [implizite lokale Deklarationen](statements.md#implicit-local-declarations)), und die Schleifensteuerungsvariable bezieht sich auf diesen bereits vorhandenen Variablen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-467">if local variable type inference is not being used but implicit local declaration is, then an implicit local variable is created whose scope is the entire method (Section [Implicit Local Declarations](statements.md#implicit-local-declarations)), and the loop control variable refers to this pre-existing variable;</span></span>
    * <span data-ttu-id="ac6c8-468">Wenn weder die lokale Variable Typrückschluss als auch die implizite lokale Deklarationen verwendet werden, ist es ein Fehler auf.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-468">if neither local variable type inference nor implicit local declarations are used, it is an error.</span></span>

  * <span data-ttu-id="ac6c8-469">Wenn Auflösung mit einem als weder auf einen Typ als auch auf eine Variable klassifiziert erfolgreich ist, ist es ein Fehler auf.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-469">If resolution succeeds with something classified as neither a type nor a variable, it is an error.</span></span>

* <span data-ttu-id="ac6c8-470">Wenn die Schleifensteuerungsvariable ein Ausdruck ist, muss der Ausdruck als Variable klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-470">If the loop control variable is an expression, the expression must be classified as a variable.</span></span>

<span data-ttu-id="ac6c8-471">Eine Schleifensteuerungsvariable kann nicht verwendet werden, indem Sie einen anderen einschließen `For...Next` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-471">A loop control variable cannot be used by another enclosing `For...Next` statement.</span></span> <span data-ttu-id="ac6c8-472">Der Typ, der die Loop-Steuerelementvariable, der eine `For` -Anweisung bestimmt den Typ der Iteration und muss:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-472">The type of the loop control variable of a `For` statement determines the type of the iteration and must be one of:</span></span>

* <span data-ttu-id="ac6c8-473">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`</span><span class="sxs-lookup"><span data-stu-id="ac6c8-473">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`</span></span>
* <span data-ttu-id="ac6c8-474">Ein enumerierter Typ</span><span class="sxs-lookup"><span data-stu-id="ac6c8-474">An enumerated type</span></span>
* `Object`
* <span data-ttu-id="ac6c8-475">Ein `T` , besitzt die folgenden Operatoren, wobei `B` ist ein Typ, der in einen booleschen Ausdruck verwendet werden kann:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-475">A type `T` that has the following operators, where `B` is a type that can be used in a Boolean expression:</span></span>

```vb
Public Shared Operator >= (op1 As T, op2 As T) As B
Public Shared Operator <= (op1 As T, op2 As T) As B
Public Shared Operator - (op1 As T, op2 As T) As T
Public Shared Operator + (op1 As T, op2 As T) As T
```

<span data-ttu-id="ac6c8-476">Die Grenze und die Step-Ausdrücke müssen implizit in den Typ der Schleifensteuerungsvariablen konvertiert werden, und müssen klassifiziert werden, als Werte.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-476">The bound and step expressions must be implicitly convertible to the type of the loop control variable and must be classified as values.</span></span> <span data-ttu-id="ac6c8-477">Zum Zeitpunkt der Kompilierung wird der Typ der Schleifensteuerungsvariablen abgeleitet, durch den umfangreichsten Typ die Untergrenze, Obergrenze und Ausdruckstypen Schritt auswählen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-477">At compile time, the type of the loop control variable is inferred by choosing the widest type among the lower bound, upper bound, and step expression types.</span></span> <span data-ttu-id="ac6c8-478">Wenn keine widening-Konvertierung zwischen zwei Typen vorhanden ist, und klicken Sie dann ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-478">If there is no widening conversion between two of the types, then a compile-time error occurs.</span></span>

<span data-ttu-id="ac6c8-479">Laufzeit, wenn der Typ der Schleifensteuerungsvariablen `Object`, und klicken Sie dann der Typ der Iteration abgeleitet wird der gleiche wie zum Zeitpunkt der Kompilierung mit zwei Ausnahmen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-479">At run time, if the type of the loop control variable is `Object`, then the type of the iteration is inferred the same as at compile time, with two exceptions.</span></span> <span data-ttu-id="ac6c8-480">Wenn zunächst die Grenze auf Step-Ausdrücke sind "all" von ganzzahligen Typen jedoch keine allgemeinsten Typ haben, und allgemeinsten Typ, der umfasst alle drei Typen werden abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-480">First, if the bound and step expressions are all of integral types but have no widest type, then the widest type that encompasses all three types will be inferred.</span></span> <span data-ttu-id="ac6c8-481">Und dann der Typ der Schleifensteuerungsvariablen abgeleitet `String`, `Double` wird stattdessen abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-481">And second, if the type of the loop control variable is inferred to be `String`, `Double` will be inferred instead.</span></span> <span data-ttu-id="ac6c8-482">Wenn Sie zur Laufzeit keine Schleife Steuerelementtyp bestimmt werden kann, oder wenn einer der Ausdrücke für den Steuerelementtyp "Schleife" konvertiert werden kann eine `System.InvalidCastException` erfolgt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-482">If, at run time, no loop control type can be determined or if any of the expressions cannot be converted to the loop control type, a `System.InvalidCastException` will occur.</span></span> <span data-ttu-id="ac6c8-483">Sobald ein Schleifentyp des Steuerelements am Anfang der Schleife ausgewählt wurde, wird während der Iteration, unabhängig von Änderungen an den Wert in der Loop-Steuerelementvariable desselben Typs verwendet.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-483">Once a loop control type has been chosen at the beginning of the loop, the same type will be used throughout the iteration, regardless of changes made to the value in the loop control variable.</span></span>

<span data-ttu-id="ac6c8-484">Ein `For` Anweisung muss geschlossen werden, durch eine entsprechende `Next` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-484">A `For` statement must be closed by a matching `Next` statement.</span></span> <span data-ttu-id="ac6c8-485">Ein `Next` -Anweisung ohne eine Variable entspricht die innerste Open `For` -Anweisung, während eine `Next` -Anweisung mit der eine oder mehrere Variablen der Schleife-Steuerelement, von links nach rechts, entspricht die `For` Schleifen, die jede Variable entsprechen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-485">A `Next` statement without a variable matches the innermost open `For` statement, while a `Next` statement with one or more loop control variables will, from left to right, match the `For` loops that match each variable.</span></span> <span data-ttu-id="ac6c8-486">Wenn eine Variable entspricht einem `For` Schleife, die nicht die am häufigsten geschachtelte Schleife zu diesem Zeitpunkt ist ein Fehler während der Kompilierung führt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-486">If a variable matches a `For` loop that is not the most nested loop at that point, a compile-time error results.</span></span>

<span data-ttu-id="ac6c8-487">Am Anfang der Schleife, die drei Ausdrücke werden ausgewertet, in der Reihenfolge im Text, und der Ausdruck für die untere Grenze der Loop-Steuerelementvariable zugewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-487">At the beginning of the loop, the three expressions are evaluated in textual order and the lower bound expression is assigned to the loop control variable.</span></span> <span data-ttu-id="ac6c8-488">Wenn der Step-Wert weggelassen wird, ist implizit das Literal `1`, in den Typ der Schleifensteuerungsvariablen konvertiert.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-488">If the step value is omitted, it is implicitly the literal `1`, converted to the type of the loop control variable.</span></span> <span data-ttu-id="ac6c8-489">Die drei Ausdrücke werden nur am Anfang der Schleife ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-489">The three expressions are only ever evaluated at the beginning of the loop.</span></span>

<span data-ttu-id="ac6c8-490">Am Anfang des foreach-Schleife, ist die Steuerelementvariable verglichen, um festzustellen, ob es größer als der Endpunkt ist die Schrittausdruck positiv oder kleiner als der Endpunkt, wenn der Schrittausdruck negativ ist.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-490">At the beginning of each loop, the control variable is compared to see if it is greater than the end point if the step expression is positive, or less than the end point if the step expression is negative.</span></span> <span data-ttu-id="ac6c8-491">Wenn es sich handelt, die `For` Schleife beendet wird; andernfalls der Schleifenblock wird ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-491">If it is, the `For` loop terminates; otherwise the loop block executes.</span></span> <span data-ttu-id="ac6c8-492">Wenn die Schleifensteuerungsvariable kein primitiver Typ ist, richtet sich der Vergleichsoperator nach, ob der Ausdruck `step >= step - step` ist "true" oder "false".</span><span class="sxs-lookup"><span data-stu-id="ac6c8-492">If the loop control variable is not a primitive type, the comparison operator is determined by whether the expression `step >= step - step` is true or false.</span></span> <span data-ttu-id="ac6c8-493">Auf der `Next` -Anweisung Step-Wert ist die Steuerelementvariable hinzugefügt und an den Anfang der Schleife zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-493">At the `Next` statement, the step value is added to the control variable and execution returns to the top of the loop.</span></span>

<span data-ttu-id="ac6c8-494">Beachten Sie, dass eine neue Kopie der Loop-Steuerelementvariable *nicht* bei jeder Iteration der Schleifenblock erstellt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-494">Note that a new copy of the loop control variable is *not* created on each iteration of the loop block.</span></span> <span data-ttu-id="ac6c8-495">In dieser Hinsicht die `For` Anweisung unterscheidet sich von `For Each` (Abschnitt [For Each…Next Nächste Anweisungen](statements.md#for-eachnext-statements)).</span><span class="sxs-lookup"><span data-stu-id="ac6c8-495">In this respect, the `For` statement differs from `For Each` (Section [For Each...Next Statements](statements.md#for-eachnext-statements)).</span></span>

<span data-ttu-id="ac6c8-496">Es ist nicht zulässig, die eine Verzweigung in einem `For` eine Schleife von außerhalb der Schleife.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-496">It is not valid to branch into a `For` loop from outside the loop.</span></span>


### <a name="for-eachnext-statements"></a><span data-ttu-id="ac6c8-497">Für jede... Nächste Anweisungen</span><span class="sxs-lookup"><span data-stu-id="ac6c8-497">For Each...Next Statements</span></span>

<span data-ttu-id="ac6c8-498">Ein `For Each...Next` anweisungsschleifen auf der Grundlage der Elemente in einem Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-498">A `For Each...Next` statement loops based on the elements in an expression.</span></span> <span data-ttu-id="ac6c8-499">Ein `For Each` -Anweisung gibt, eine Schleifensteuerungsvariable und ein Enumeratorausdruck.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-499">A `For Each` statement specifies a loop control variable and an enumerator expression.</span></span> <span data-ttu-id="ac6c8-500">Die Loop-Steuerelementvariable angegeben ist, entweder über einen Bezeichner gefolgt von einem optionalen `As` -Klausel oder einen Ausdruck ein.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-500">The loop control variable is specified either through an identifier followed by an optional `As` clause or an expression.</span></span>

```antlr
ForEachStatement
    : 'For' 'Each' LoopControlVariable 'In' LineTerminator? Expression StatementTerminator
      Block?
      ( 'Next' NextExpressionList? StatementTerminator )?
    ;
```

<span data-ttu-id="ac6c8-501">Befolgen die gleichen Regeln wie `For...Next` Anweisungen (Abschnitt [für... Nächste Anweisungen](statements.md#fornext-statements)), die Schleifensteuerungsvariable verweist entweder auf eine neue lokale Variable, die bestimmten dieser für jede... Next-Anweisung, oder eine vorhandene Variable oder einen Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-501">Following the same rules as `For...Next` statements (Section [For...Next Statements](statements.md#fornext-statements)), the loop control variable refers either to a new local variable specific to this For Each...Next statement, or to a pre-existing variable, or to an expression.</span></span>

<span data-ttu-id="ac6c8-502">Enumeratorausdruck muss als Wert klassifiziert werden, und der Typ muss vom Auflistungstyp oder `Object`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-502">The enumerator expression must be classified as a value and its type must be a collection type or `Object`.</span></span> <span data-ttu-id="ac6c8-503">Wenn der Typ des Enumeratorausdrucks `Object`, und klicken Sie dann die gesamte Verarbeitung bis zur Laufzeit verzögert wird.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-503">If the type of the enumerator expression is `Object`, then all processing is deferred until run-time.</span></span> <span data-ttu-id="ac6c8-504">Andernfalls muss eine Konvertierung vom Elementtyp der Auflistung in den Typ der Schleifensteuerungsvariablen vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-504">Otherwise, a conversion must exist from the element type of the collection to the type of the loop control variable.</span></span>

<span data-ttu-id="ac6c8-505">Die Schleifensteuerungsvariable kann nicht verwendet werden, indem Sie einen anderen einschließen `For Each` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-505">The loop control variable cannot be used by another enclosing `For Each` statement.</span></span> <span data-ttu-id="ac6c8-506">Ein `For Each` Anweisung muss geschlossen werden, durch eine entsprechende `Next` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-506">A `For Each` statement must be closed by a matching `Next` statement.</span></span> <span data-ttu-id="ac6c8-507">Ein `Next` -Anweisung ohne eine Schleifensteuerungsvariable entspricht der innersten, offenen `For Each`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-507">A `Next` statement without a loop control variable matches the innermost open `For Each`.</span></span> <span data-ttu-id="ac6c8-508">Ein `Next` -Anweisung mit der eine oder mehrere Variablen der Schleife-Steuerelement, von links nach rechts, entspricht die `For Each` Schleifen, die die gleichen Loop-Steuerelementvariable haben.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-508">A `Next` statement with one or more loop control variables will, from left to right, match the `For Each` loops that have the same loop control variable.</span></span> <span data-ttu-id="ac6c8-509">Wenn eine Variable entspricht einer `For Each` Schleife, die nicht die am häufigsten geschachtelte Schleife zu diesem Zeitpunkt ist ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-509">If a variable matches a `For Each` loop that is not the most nested loop at that point, a compile-time error occurs.</span></span>

<span data-ttu-id="ac6c8-510">Ein Typ `C` gilt eine *Auflistungstyp* eine von:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-510">A type `C` is said to be a *collection type* if one of:</span></span>

* <span data-ttu-id="ac6c8-511">Die Folgendes zutrifft:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-511">All of the following are true:</span></span>
  * <span data-ttu-id="ac6c8-512">`C` enthält eine zugängliche Instanzmethode, freigegeben oder eine Erweiterungsmethode mit der Signatur `GetEnumerator()` , die einen Typ zurückgibt `E`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-512">`C` contains an accessible instance, shared or extension method with the signature `GetEnumerator()` that returns a type `E`.</span></span>
  * <span data-ttu-id="ac6c8-513">`E` enthält eine zugängliche Instanzmethode, freigegeben oder eine Erweiterungsmethode mit der Signatur `MoveNext()` sowie des Rückgabetyps `Boolean`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-513">`E` contains an accessible instance, shared or extension method with the signature `MoveNext()` and the return type `Boolean`.</span></span>
  * <span data-ttu-id="ac6c8-514">`E` enthält eine zugängliche Instanzmethode oder eine freigegebene Eigenschaft, die mit dem Namen `Current` , die einen Getter hat.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-514">`E` contains an accessible instance or shared property named `Current` that has a getter.</span></span> <span data-ttu-id="ac6c8-515">Der Typ dieser Eigenschaft ist der Elementtyp des Auflistungstyps.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-515">The type of this property is the element type of the collection type.</span></span>

* <span data-ttu-id="ac6c8-516">Sie implementiert die Schnittstelle `System.Collections.Generic.IEnumerable(Of T)`, wobei der Elementtyp der Sammlung betrachtet wird `T`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-516">It implements the interface `System.Collections.Generic.IEnumerable(Of T)`, in which case the element type of the collection is considered to be `T`.</span></span>

* <span data-ttu-id="ac6c8-517">Sie implementiert die Schnittstelle `System.Collections.IEnumerable`, wobei der Elementtyp der Sammlung betrachtet wird `Object`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-517">It implements the interface `System.Collections.IEnumerable`, in which case the element type of the collection is considered to be `Object`.</span></span>

<span data-ttu-id="ac6c8-518">Es folgt ein Beispiel für eine Klasse, die aufgelistet werden kann:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-518">Following is an example of a class that can be enumerated:</span></span>

```vb
Public Class IntegerCollection
    Private integers(10) As Integer

    Public Class IntegerCollectionEnumerator
        Private collection As IntegerCollection
        Private index As Integer = -1

        Friend Sub New(c As IntegerCollection)
            collection = c
        End Sub

        Public Function MoveNext() As Boolean
            index += 1

            Return index <= 10
        End Function

        Public ReadOnly Property Current As Integer
            Get
                If index < 0 OrElse index > 10 Then
                    Throw New System.InvalidOperationException()
                End If

                Return collection.integers(index)
            End Get
        End Property
    End Class

    Public Sub New()
        Dim i As Integer

        For i = 0 To 10
            integers(i) = I
        Next i
    End Sub

    Public Function GetEnumerator() As IntegerCollectionEnumerator
        Return New IntegerCollectionEnumerator(Me)
    End Function
End Class
```

<span data-ttu-id="ac6c8-519">Vor dem Beginn die Schleife wird der Enumeratorausdruck ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-519">Before the loop begins, the enumerator expression is evaluated.</span></span> <span data-ttu-id="ac6c8-520">Wenn der Typ des Ausdrucks erfüllt nicht die Entwurfsmuster, wird der Ausdruck umgewandelt wird `System.Collections.IEnumerable` oder `System.Collections.Generic.IEnumerable(Of T)`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-520">If the type of the expression does not satisfy the design pattern, then the expression is cast to `System.Collections.IEnumerable` or `System.Collections.Generic.IEnumerable(Of T)`.</span></span> <span data-ttu-id="ac6c8-521">Wenn der Ausdruckstyp die generische Schnittstelle implementiert, die generische Schnittstelle wird zum Zeitpunkt der Kompilierung bevorzugt, aber die nicht generische Schnittstelle wird zur Laufzeit bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-521">If the expression type implements the generic interface, the generic interface is preferred at compile-time but the non-generic interface is preferred at run-time.</span></span> <span data-ttu-id="ac6c8-522">Wenn der Ausdruckstyp mehrere Male die generische Schnittstelle implementiert, die Anweisung gilt nicht eindeutig, und ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-522">If the expression type implements the generic interface multiple times, the statement is considered ambiguous and a compile-time error occurs.</span></span>

<span data-ttu-id="ac6c8-523">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="ac6c8-523">__Note.__</span></span> <span data-ttu-id="ac6c8-524">Die nicht generische Schnittstelle wird im spät gebundenen Fall bevorzugt, da die generische Schnittstelle auswählen bedeutet, dass alle Aufrufe der Schnittstellenmethoden Typparameter erforderlich machen würde.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-524">The non-generic interface is preferred in the late bound case, because picking the generic interface would mean that all the calls to the interface methods would involve type parameters.</span></span> <span data-ttu-id="ac6c8-525">Da es nicht wissen, den entsprechenden Argumenten zur Laufzeit zu geben, müssten alle Aufrufe an. verwenden die spät gebundene Aufrufe.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-525">Since it is not possible to know the matching type arguments at run-time, all such calls would have to be made using late-bound calls.</span></span> <span data-ttu-id="ac6c8-526">Dies wäre langsamer als die nicht generische Schnittstelle aufrufen, da die nicht generische Schnittstelle mithilfe von Aufrufen der Kompilierung aufgerufen werden kann.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-526">This would be slower than calling the non-generic interface because the non-generic interface could be called using compile-time calls.</span></span>

<span data-ttu-id="ac6c8-527">`GetEnumerator` wird aufgerufen, auf den sich ergebenden Wert und der Rückgabetyp Wert der Funktion wird in ein temporäres gespeichert.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-527">`GetEnumerator` is called on the resulting value and the return value of the function is stored in a temporary.</span></span> <span data-ttu-id="ac6c8-528">Und dann am Anfang jeder Iteration `MoveNext` für die temporäre aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-528">Then at the beginning of each iteration, `MoveNext` is called on the temporary.</span></span> <span data-ttu-id="ac6c8-529">Wenn sie zurückgibt `False`, die Schleife wird beendet.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-529">If it returns `False`, the loop terminates.</span></span> <span data-ttu-id="ac6c8-530">Andernfalls wird jede Iteration der Schleife wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-530">Otherwise, each iteration of the loop is executed as follows:</span></span>

1. <span data-ttu-id="ac6c8-531">Wenn die Schleifensteuerungsvariable eine neue lokale Variable (anstelle einer bereits vorhandenen) identifiziert, wird dann eine neue Kopie dieser lokalen Variablen erstellt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-531">If the loop control variable identified a new local variable (rather than a pre-existing one), then a fresh copy of this local variable is created.</span></span> <span data-ttu-id="ac6c8-532">Für die aktuelle Iteration werden alle Verweise innerhalb des Schleifenblocks auf diese Kopie verweisen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-532">For the current iteration, all references within the loop block will refer to this copy.</span></span>
2. <span data-ttu-id="ac6c8-533">Die `Current` Eigenschaft wird abgerufen, umgewandelt in den Typ der Schleifensteuerungsvariablen (unabhängig davon, ob die Konvertierung implizit oder explizit), und die Schleifensteuerungsvariable zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-533">The `Current` property is retrieved, coerced to the type of the loop control variable (regardless of whether the conversion is implicit or explicit), and assigned to the loop control variable.</span></span>
3. <span data-ttu-id="ac6c8-534">Die Schleifenblock wird ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-534">The loop block executes.</span></span>

<span data-ttu-id="ac6c8-535">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="ac6c8-535">__Note.__</span></span> <span data-ttu-id="ac6c8-536">Es ist eine geringfügige Änderung im Verhalten zwischen der Version 10.0 und 11.0 der Sprache.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-536">There is a slight change in behavior between version 10.0 and 11.0 of the language.</span></span> <span data-ttu-id="ac6c8-537">Vor dem 11.0, wurde eine neue Iterationsvariable *nicht* erstellt für jede Iteration der Schleife.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-537">Prior to 11.0, a fresh iteration variable was *not* created for each iteration of the loop.</span></span> <span data-ttu-id="ac6c8-538">Dieser Unterschied ist die Observable-Objekt nur dann, wenn die Iterationsvariable erfasst wird, durch einen Lambda-Ausdruck oder eine LINQ-Ausdruck, der dann nach der Schleife aufgerufen:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-538">This difference is observable only if the iteration variable is captured by a lambda or a LINQ expression which is then invoked after the loop:</span></span>

```vb
Dim lambdas As New List(Of Action)
For Each x In {1,2,3}
   lambdas.Add(Sub() Console.WriteLine(x)
Next
lambdas(0).Invoke()
lambdas(1).Invoke()
lambdas(2).Invoke()
```

<span data-ttu-id="ac6c8-539">Bis zu Visual Basic 10.0, dies wurde eine Warnung ausgegeben, zum Zeitpunkt der Kompilierung und gedruckt "3" drei Mal.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-539">Up to Visual Basic 10.0, this produced a warning at compile-time and printed "3" three times.</span></span> <span data-ttu-id="ac6c8-540">Das war, da gab es nur eine einzelne Variable "X" freigegeben, die für alle Iterationen der Schleife, und alle drei Lambdas erfasst das gleiche "X" mit der Zeit, die den Lambda-Ausdrücke ausgeführt wurden sie dann die Zahl 3 gespeichert.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-540">That was because there was only a single variable "x" shared by all iterations of the loop, and all three lambdas captured the same "x", and by the time the lambdas were executed it then held the number 3.</span></span> <span data-ttu-id="ac6c8-541">Ab Visual Basic-11.0 gibt sie "1, 2, 3".</span><span class="sxs-lookup"><span data-stu-id="ac6c8-541">As of Visual Basic 11.0, it prints "1, 2, 3".</span></span> <span data-ttu-id="ac6c8-542">Das ist da jeder Lambda "X" über eine andere Variable erfasst.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-542">That is because each lambda captures a different variable "x".</span></span>


<span data-ttu-id="ac6c8-543">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="ac6c8-543">__Note.__</span></span> <span data-ttu-id="ac6c8-544">Das aktuelle Element der Iteration wird in den Typ der Schleifensteuerungsvariablen konvertiert, auch wenn die Konvertierung explizit ist, da keine praktische Möglichkeit besteht, einen Konvertierungsoperator in der Anweisung einzuführen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-544">The current element of the iteration is converted to the type of the loop control variable even if the conversion is explicit because there is no convenient place to introduce a conversion operator in the statement.</span></span> <span data-ttu-id="ac6c8-545">Dies war besonders problematischen, bei der Arbeit mit der jetzt veralteten Typ `System.Collections.ArrayList`, da der Elementtyp ist der `Object`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-545">This became particularly troublesome when working with the now-obsolete type `System.Collections.ArrayList`, because its element type is `Object`.</span></span> <span data-ttu-id="ac6c8-546">Dies müsste erforderlichen Umwandlungen in sehr viele Schleifen, etwa die unserer Meinung nach war nicht ideal.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-546">This would have required casts in a great many loops, something we felt was not ideal.</span></span> <span data-ttu-id="ac6c8-547">Ironischerweise Generika aktiviert die Erstellung einer stark typisierten Auflistung und `System.Collections.Generic.List(Of T)`, die möglicherweise haben wir denken Sie an diesem Punkt Design, aber aus Gründen der Kompatibilität kann nicht dadurch jetzt geändert werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-547">Ironically, generics enabled the creation of a strongly-typed collection, `System.Collections.Generic.List(Of T)`, which might have made us rethink this design point, but for compatibility's sake, this cannot be changed now.</span></span>


<span data-ttu-id="ac6c8-548">Wenn die `Next` Ausführung-Anweisung erreicht wird, zurückgegeben werden, am Anfang der Schleife.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-548">When the `Next` statement is reached, execution returns to the top of the loop.</span></span> <span data-ttu-id="ac6c8-549">Wenn eine Variable angegeben wird, nach der `Next` -Schlüsselwort, es muss identisch sein als die erste Variable nach der `For Each`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-549">If a variable is specified after the `Next` keyword, it must be the same as the first variable after the `For Each`.</span></span> <span data-ttu-id="ac6c8-550">Beachten Sie z. B. folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-550">For example, consider the following code:</span></span>

```vb
Module Test
    Sub Main()
        Dim i As Integer
        Dim c As IntegerCollection = New IntegerCollection()

        For Each i In c
            Console.WriteLine(i)
        Next i
    End Sub
End Module
```

<span data-ttu-id="ac6c8-551">Dies entspricht dem folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-551">It is equivalent to the following code:</span></span>

```vb
Module Test
    Sub Main()
        Dim i As Integer
        Dim c As IntegerCollection = New IntegerCollection()

        Dim e As IntegerCollection.IntegerCollectionEnumerator

        e = c.GetEnumerator()
        While e.MoveNext()
            i = e.Current

            Console.WriteLine(i)
        End While
    End Sub
End Module
```

<span data-ttu-id="ac6c8-552">Wenn der Typ `E` des Enumerators implementiert `System.IDisposable`, und klicken Sie dann der Enumerator, beim Beenden der schleifenstatus verworfen wird durch Aufrufen der `Dispose` Methode.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-552">If the type `E` of the enumerator implements `System.IDisposable`, then the enumerator is disposed upon exiting the loop by calling the `Dispose` method.</span></span> <span data-ttu-id="ac6c8-553">Dadurch wird sichergestellt, dass der Enumerator reservierten Ressourcen freigegeben werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-553">This ensures that resources held by the enumerator are released.</span></span> <span data-ttu-id="ac6c8-554">Wenn die Methode mit der `For Each` Anweisung nicht strukturierte Fehlerbehandlung, nicht verwendet und dann die `For Each` Anweisung umgeben eine `Try` -Anweisung mit der `Dispose` Methode mit dem Namen der `Finally` auf Bereinigung zu gewährleisten.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-554">If the method containing the `For Each` statement does not use unstructured error handling, then the `For Each` statement is wrapped in a `Try` statement with the `Dispose` method called in the `Finally` to ensure cleanup.</span></span>

<span data-ttu-id="ac6c8-555">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="ac6c8-555">__Note.__</span></span> <span data-ttu-id="ac6c8-556">Die `System.Array` Typ ist ein Sammlungstyp und da Arrays alle Typen abgeleitet `System.Array`, jeder Ausdruck der Array-Typ ist zulässig, einem `For Each` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-556">The `System.Array` type is a collection type, and since all array types derive from `System.Array`, any array type expression is permitted in a `For Each` statement.</span></span> <span data-ttu-id="ac6c8-557">Für eindimensionale Arrays die `For Each` Anweisung listet die Elemente in aufsteigender Indexreihenfolge, mit dem Index 0 beginnt und endet mit Index-Länge - 1.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-557">For single-dimensional arrays, the `For Each` statement enumerates the array elements in increasing index order, starting with index 0 and ending with index Length - 1.</span></span> <span data-ttu-id="ac6c8-558">Für mehrdimensionale Arrays werden die Indizes von die Dimension ganz rechts zuerst erhöht.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-558">For multidimensional arrays, the indices of the rightmost dimension are increased first.</span></span>

<span data-ttu-id="ac6c8-559">Der folgende code beispielsweise druckt `1 2 3 4`:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-559">For example, the following code prints `1 2 3 4`:</span></span>

```vb
Module Test
    Sub Main()
        Dim x(,) As Integer = { { 1, 2 }, { 3, 4 } }
        Dim i As Integer

        For Each i In x
            Console.Write(i & " ")
        Next i
    End Sub
End Module
```

<span data-ttu-id="ac6c8-560">Es ist nicht zulässig, die eine Verzweigung in einem `For Each` Anweisungsblock von außerhalb des Blocks.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-560">It is not valid to branch into a `For Each` statement block from outside the block.</span></span>


## <a name="exception-handling-statements"></a><span data-ttu-id="ac6c8-561">Behandlung von Ausnahmen-Anweisungen</span><span class="sxs-lookup"><span data-stu-id="ac6c8-561">Exception-Handling Statements</span></span>

<span data-ttu-id="ac6c8-562">Visual Basic unterstützt strukturierte und unstrukturierte Ausnahmebehandlung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-562">Visual Basic supports structured exception handling and unstructured exception handling.</span></span> <span data-ttu-id="ac6c8-563">Nur eine Art der Ausnahmebehandlung kann verwendet werden, in einer Methode, aber die `Error` Anweisung kann in die strukturierte Ausnahmebehandlung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-563">Only one style of exception handling may be used in a method, but the `Error` statement may be used in structured exception handling.</span></span> <span data-ttu-id="ac6c8-564">Wenn eine Methode für beide Arten der Ausnahmebehandlung verwendet, führt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-564">If a method uses both styles of exception handling, a compile-time error results.</span></span>

```antlr
ErrorHandlingStatement
    : StructuredErrorStatement
    | UnstructuredErrorStatement
    ;
```

### <a name="structured-exception-handling-statements"></a><span data-ttu-id="ac6c8-565">Strukturierte Ausnahmebehandlung-Anweisungen</span><span class="sxs-lookup"><span data-stu-id="ac6c8-565">Structured Exception-Handling Statements</span></span>

<span data-ttu-id="ac6c8-566">Strukturierte Ausnahmebehandlung ist eine Methode zur Handhabung von Fehlern durch das Deklarieren von expliziten Blöcke, die in denen bestimmte Ausnahmen behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-566">Structured exception handling is a method of handling errors by declaring explicit blocks within which certain exceptions will be handled.</span></span> <span data-ttu-id="ac6c8-567">Strukturierte Ausnahmebehandlung erfolgt über eine `Try` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-567">Structured exception handling is done through a `Try` statement.</span></span>

```antlr
StructuredErrorStatement
    : ThrowStatement
    | TryStatement
    ;

TryStatement
    : 'Try' StatementTerminator
      Block?
      CatchStatement*
      FinallyStatement?
      'End' 'Try' StatementTerminator
    ;
```


<span data-ttu-id="ac6c8-568">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-568">For example:</span></span>

```vb
Module Test
    Sub ThrowException()
        Throw New Exception()
    End Sub

    Sub Main()
        Try
            ThrowException()
        Catch e As Exception
            Console.WriteLine("Caught exception!")
        Finally
            Console.WriteLine("Exiting try.")
        End Try
    End Sub
End Module
```

<span data-ttu-id="ac6c8-569">Ein `Try` Anweisung besteht aus drei verschiedenen Arten von Blöcken: try-Blöcke, catch-Blöcken, und finally-Blöcken.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-569">A `Try` statement is made up of three kinds of blocks: try blocks, catch blocks, and finally blocks.</span></span> <span data-ttu-id="ac6c8-570">Ein *try-Block* ist ein Anweisungsblock ein, die die auszuführenden Anweisungen enthält.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-570">A *try block* is a statement block that contains the statements to be executed.</span></span> <span data-ttu-id="ac6c8-571">Ein *catch-Block* ein Anweisungsblock aus, die die Ausnahme behandelt wird.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-571">A *catch block* is a statement block that handles an exception.</span></span> <span data-ttu-id="ac6c8-572">Ein *finally-block* ist ein Anweisungsblock mit Anweisungen aus, um die Ausführung werden die `Try` Anweisung beendet wird, unabhängig davon, ob eine Ausnahme aufgetreten ist, und behandelt wurde.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-572">A *finally block* is a statement block that contains statements to be run when the `Try` statement is exited, regardless of whether an exception has occurred and been handled.</span></span> <span data-ttu-id="ac6c8-573">Ein `Try` -Anweisung, die nur einen Try-Block und einem finally enthalten kann blockieren, müssen mindestens einen Catch-Block enthalten oder finally-block.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-573">A `Try` statement, which can only contain one try block and one finally block, must contain at least one catch block or finally block.</span></span> <span data-ttu-id="ac6c8-574">Es ist unzulässig, die Ausführung explizit in einen Try-Block mit Ausnahme von in einem Catch-Block in der gleichen Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-574">It is invalid to explicitly transfer execution into a try block except from within a catch block in the same statement.</span></span>


#### <a name="finally-blocks"></a><span data-ttu-id="ac6c8-575">Finally-Blöcke</span><span class="sxs-lookup"><span data-stu-id="ac6c8-575">Finally Blocks</span></span>

<span data-ttu-id="ac6c8-576">Ein `Finally` Block wird immer ausgeführt, wenn die Ausführung eines beliebigen Teils der `Try` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-576">A `Finally` block is always executed when execution leaves any part of the `Try` statement.</span></span> <span data-ttu-id="ac6c8-577">Zur Ausführung ist keine bestimmte Handlung erforderlich. die `Finally` des Blocks aufheben; Wenn die Ausführung verlässt die `Try` -Anweisung, die das System wird automatisch ausgeführt. die `Finally` blockieren, und klicken Sie dann die Ausführung an das vorgesehene Ziel übertragen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-577">No explicit action is required to execute the `Finally` block; when execution leaves the `Try` statement, the system will automatically execute the `Finally` block and then transfer execution to its intended destination.</span></span> <span data-ttu-id="ac6c8-578">Die `Finally` Block wird ausgeführt, unabhängig davon, wie die Ausführung erfolgt die `Try` Anweisung: bis zum Ende der `Try` blockieren, bis zum Ende einer `Catch` blockieren, über eine `Exit Try` Anweisung, über eine `GoTo` -Anweisung oder durch eine ausgelöste Ausnahme nicht behandelt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-578">The `Finally` block is executed regardless of how execution leaves the `Try` statement: through the end of the `Try` block, through the end of a `Catch` block, through an `Exit Try` statement, through a `GoTo` statement, or by not handling a thrown exception.</span></span>

<span data-ttu-id="ac6c8-579">Beachten Sie, dass die `Await` Ausdruck in einer Async-Methode und die `Yield` -Anweisung in eine Iteratormethode, kann dazu führen, dass ablaufsteuerung in die Async- oder Iterator-Methodeninstanz anhalten und fortsetzen in einer anderen Methodeninstanz.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-579">Note that the `Await` expression in an async method, and the `Yield` statement in an iterator method, can cause flow of control to suspend in the async or iterator method instance and resume in some other method instance.</span></span> <span data-ttu-id="ac6c8-580">Dies ist jedoch lediglich führt zu einer Unterbrechung der Ausführung und umfasst jedoch nicht das Beenden der Instanz entsprechende Async-Methode oder ein Iterator-Methode, und verursacht also keine `Finally` Blöcke ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-580">However, this is merely a suspension of execution and does not involve exiting the respective async method or iterator method instance, and so does not cause `Finally` blocks to be executed.</span></span>

<span data-ttu-id="ac6c8-581">Ist ungültig, die explizit die Ausführung in übertragen einer `Finally` blockieren; es ist ebenfalls ungültig. um die Ausführung von übertragen ein `Finally` block, außer durch eine Ausnahme.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-581">It is invalid to explicitly transfer execution into a `Finally` block; it is also invalid to transfer execution out of a `Finally` block except through an exception.</span></span>

```antlr
FinallyStatement
    : 'Finally' StatementTerminator
      Block?
    ;
```

#### <a name="catch-blocks"></a><span data-ttu-id="ac6c8-582">catch-Blöcke</span><span class="sxs-lookup"><span data-stu-id="ac6c8-582">Catch Blocks</span></span>

<span data-ttu-id="ac6c8-583">Wenn eine Ausnahme, während der Verarbeitung auftritt der `Try` blockieren, von denen jede `Catch` Anweisung wird untersucht, in der Reihenfolge im Text zu bestimmen, ob sie die Ausnahme behandelt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-583">If an exception occurs while processing the `Try` block, each `Catch` statement is examined in textual order to determine if it handles the exception.</span></span>

```antlr
CatchStatement
    : 'Catch' ( Identifier ( 'As' NonArrayTypeName )? )?
      ( 'When' BooleanExpression )? StatementTerminator
      Block?
    ;
```

<span data-ttu-id="ac6c8-584">Der Bezeichner angegeben wird, einem `Catch` Klausel stellt die Ausnahme dar, die ausgelöst wurde.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-584">The identifier specified in a `Catch` clause represents the exception that has been thrown.</span></span> <span data-ttu-id="ac6c8-585">Wenn der Bezeichner enthält ein `As` gilt als-Klausel, und klicken Sie dann auf den Bezeichner deklariert werden, in der `Catch` lokalen Deklarationsabschnitt des Blocks.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-585">If the identifier contains an `As` clause, then the identifier is considered to be declared within the `Catch` block's local declaration space.</span></span> <span data-ttu-id="ac6c8-586">Andernfalls muss der Bezeichner eine lokale Variable (nicht statische Variable), die in einem enthaltenden Block definiert wurde.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-586">Otherwise, the identifier must be a local variable (not a static variable) that was defined in a containing block.</span></span>

<span data-ttu-id="ac6c8-587">Ein `Catch` -Klausel ohne Bezeichner fängt alle Ausnahmen, die von abgeleiteten `System.Exception`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-587">A `Catch` clause with no identifier will catch all exceptions derived from `System.Exception`.</span></span> <span data-ttu-id="ac6c8-588">Ein `Catch` Klausel mit einer ID, fängt nur Ausnahmen, deren Typen entsprechen den oder vom Typ des Bezeichners abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-588">A `Catch` clause with an identifier will only catch exceptions whose types are the same as or derived from the type of the identifier.</span></span> <span data-ttu-id="ac6c8-589">Der Typ muss `System.Exception`, oder ein abgeleiteter Typ von `System.Exception`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-589">The type must be `System.Exception`, or a type derived from `System.Exception`.</span></span> <span data-ttu-id="ac6c8-590">Wann wird eine Ausnahme, die von abgeleitet `System.Exception`, ein Verweis auf das Ausnahmeobjekt wird gespeichert, in dem Objekt, das von der Funktion zurückgegebene `Microsoft.VisualBasic.Information.Err`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-590">When an exception is caught that derives from `System.Exception`, a reference to the exception object is stored in the object returned by the function `Microsoft.VisualBasic.Information.Err`.</span></span>

<span data-ttu-id="ac6c8-591">Ein `Catch` -Klausel mit einer `When` -Klausel nur Ausnahmen abgefangen, wenn der Ausdruck ergibt `True`; der Typ des Ausdrucks muss einen booleschen Ausdruck gemäß Abschnitt [boolesche Ausdrücke](expressions.md#boolean-expressions).</span><span class="sxs-lookup"><span data-stu-id="ac6c8-591">A `Catch` clause with a `When` clause will only catch exceptions when the expression evaluates to `True`; the type of the expression must be a Boolean expression as per Section [Boolean Expressions](expressions.md#boolean-expressions).</span></span> <span data-ttu-id="ac6c8-592">Ein `When` -Klausel wird nur angewendet, nachdem der Typ der Ausnahme überprüft, und der Ausdruck bezieht sich möglicherweise auf den Bezeichner, der die Ausnahme darstellt wie dieses Beispiel veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-592">A `When` clause is only applied after checking the type of the exception, and the expression may refer to the identifier representing the exception, as this example demonstrates:</span></span>

```vb
Module Test
    Sub Main()
        Dim i As Integer = 5

        Try
            Throw New ArgumentException()
        Catch e As OverflowException When i = 5
            Console.WriteLine("First handler")
        Catch e As ArgumentException When i = 4
            Console.WriteLine("Second handler")
        Catch When i = 5
            Console.WriteLine("Third handler")
        End Try

    End Sub
End Module
```

<span data-ttu-id="ac6c8-593">In diesem Beispiel gibt:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-593">This example prints:</span></span>

```
Third handler
```

<span data-ttu-id="ac6c8-594">Wenn eine `Catch` Klausel wird die Ausnahme behandelt, überträgt die Ausführung an die `Catch` Block.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-594">If a `Catch` clause handles the exception, execution transfers to the `Catch` block.</span></span> <span data-ttu-id="ac6c8-595">Am Ende der `Catch` blockieren, Ausführung Datenübertragungen an den der ersten Anweisung nach der `Try` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-595">At the end of the `Catch` block, execution transfers to the first statement following the `Try` statement.</span></span> <span data-ttu-id="ac6c8-596">Die `Try` Anweisung werden Ausnahmen nicht behandeln eine `Catch` Block.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-596">The `Try` statement will not handle any exceptions thrown in a `Catch` block.</span></span> <span data-ttu-id="ac6c8-597">Wenn kein `Catch` -Klausel die Ausnahme behandelt, überträgt der Ausführung an einem Speicherort, der durch das System bestimmt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-597">If no `Catch` clause handles the exception, execution transfers to a location determined by the system.</span></span>

<span data-ttu-id="ac6c8-598">Es ist nicht zulässig, die explizit die Ausführung in übertragen einer `Catch` Block.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-598">It is invalid to explicitly transfer execution into a `Catch` block.</span></span>

<span data-ttu-id="ac6c8-599">Die Filter in Klauseln wenn normalerweise vor der Ausnahme, die ausgelöst wird, ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-599">The filters in When clauses are normally evaluated prior to the exception being thrown.</span></span> <span data-ttu-id="ac6c8-600">Der folgende Code gibt beispielsweise "Filter, schließlich Catch".</span><span class="sxs-lookup"><span data-stu-id="ac6c8-600">For instance, the following code will print "Filter, Finally, Catch".</span></span>

```vb
Sub Main()
   Try
       Foo()
   Catch ex As Exception When F()
       Console.WriteLine("Catch")
   End Try
End Sub

Sub Foo()
    Try
        Throw New Exception
    Finally
        Console.WriteLine("Finally")
    End Try
End Sub

Function F() As Boolean
    Console.WriteLine("Filter")
    Return True
End Function
```

 <span data-ttu-id="ac6c8-601">Async-und Iteratormethoden führt allerdings alle finally-Blöcke in der sie vor dem Filter außerhalb ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-601">However, Async and Iterator methods cause all finally blocks inside them to be executed prior to any filters outside.</span></span> <span data-ttu-id="ac6c8-602">Beispielsweise hätte der obige Code `Async Sub Foo()`, lautet die Ausgabe "Schließlich zu filtern, Catch".</span><span class="sxs-lookup"><span data-stu-id="ac6c8-602">For instance, if the above code had `Async Sub Foo()`, then the output would be "Finally, Filter, Catch".</span></span>


#### <a name="throw-statement"></a><span data-ttu-id="ac6c8-603">Throw-Anweisung</span><span class="sxs-lookup"><span data-stu-id="ac6c8-603">Throw Statement</span></span>

<span data-ttu-id="ac6c8-604">Die `Throw` -Anweisung löst eine Ausnahme, die von einer Instanz von abgeleiteten Typs dargestellt wird `System.Exception`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-604">The `Throw` statement raises an exception, which is represented by an instance of a type derived from `System.Exception`.</span></span>

```antlr
ThrowStatement
    : 'Throw' Expression? StatementTerminator
    ;
```

<span data-ttu-id="ac6c8-605">Wenn der Ausdruck nicht als Wert klassifiziert wird oder kein Typ abgeleitet sind ist `System.Exception`, und klicken Sie dann ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-605">If the expression is not classified as a value or is not a type derived from `System.Exception`, then a compile-time error occurs.</span></span> <span data-ttu-id="ac6c8-606">Wenn der Ausdruck mit einem null-Wert zur Laufzeit ausgewertet und dann eine `System.NullReferenceException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-606">If the expression evaluates to a null value at run time, then a `System.NullReferenceException` exception is raised instead.</span></span>

<span data-ttu-id="ac6c8-607">Ein `Throw` Anweisung kann weglassen, den Ausdruck in einem Catch-Block, der eine `Try` finally-Anweisung, solange es ist keine dazwischen liegenden block.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-607">A `Throw` statement may omit the expression within a catch block of a `Try` statement, as long as there is no intervening finally block.</span></span> <span data-ttu-id="ac6c8-608">In diesem Fall löst die Ausnahme, die gerade verarbeitet wird, im Catch-Block von die Anweisung erneut aus.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-608">In that case, the statement rethrows the exception currently being handled within the catch block.</span></span> <span data-ttu-id="ac6c8-609">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-609">For example:</span></span>

```vb
Sub Test(x As Integer)
    Try
        Throw New Exception()
    Catch
        If x = 0 Then
            Throw    ' OK, rethrows exception from above.
        Else
            Try
                If x = 1 Then
                    Throw   ' OK, rethrows exception from above.
                End If
            Finally
                Throw    ' Invalid, inside of a Finally.
            End Try
        End If
    End Try
End Sub
```


### <a name="unstructured-exception-handling-statements"></a><span data-ttu-id="ac6c8-610">Unstrukturierte Ausnahmebehandlung-Anweisungen</span><span class="sxs-lookup"><span data-stu-id="ac6c8-610">Unstructured Exception-Handling Statements</span></span>

<span data-ttu-id="ac6c8-611">Unstrukturierte Ausnahmebehandlung ist eine Methode zur Handhabung von Fehlern durch, der angibt, Anweisungen, um zum Verzweigen, wenn eine Ausnahme auftritt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-611">Unstructured exception handling is a method of handling errors by indicating statements to branch to when an exception occurs.</span></span> <span data-ttu-id="ac6c8-612">Unstrukturierte Ausnahmebehandlung wird mithilfe von drei Anweisungen implementiert: die `Error` -Anweisung, die `On Error` -Anweisung, und die `Resume` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-612">Unstructured exception handling is implemented using three statements: the `Error` statement, the `On Error` statement, and the `Resume` statement.</span></span>

```antlr
UnstructuredErrorStatement
    : ErrorStatement
    | OnErrorStatement
    | ResumeStatement
    ;
```

<span data-ttu-id="ac6c8-613">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-613">For example:</span></span>

```vb
Module Test
    Sub ThrowException()
        Error 5
    End Sub

    Sub Main()
        On Error GoTo GotException

        ThrowException()
        Exit Sub

GotException:
        Console.WriteLine("Caught exception!")
        Resume Next
    End Sub
End Module
```

<span data-ttu-id="ac6c8-614">Wenn eine Methode nicht strukturierte Ausnahmebehandlung verwendet wird, wird ein einzelnes strukturierter Ausnahmehandler für die gesamte Methode eingerichtet, die alle Ausnahmen abfängt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-614">When a method uses unstructured exception handling, a single structured exception handler is established for the entire method that catches all exceptions.</span></span> <span data-ttu-id="ac6c8-615">(Beachten Sie, dass in Konstruktoren, die dieser Handler nicht über den Aufruf an den Aufruf hinausreicht `New` am Anfang des Konstruktors.) Die Methode nachverfolgung von klicken Sie dann den Speicherort für die letzte Ausnahme-Handler und die letzte Ausnahme, die ausgelöst wurde.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-615">(Note that in constructors this handler does not extend over the call to the call to `New` at the beginning of the constructor.) The method then keeps track of the most recent exception-handler location and the most recent exception that has been thrown.</span></span> <span data-ttu-id="ac6c8-616">Am Anfang der Methode, den Speicherort der Ausnahme-Handler und die Ausnahme sind festgelegt auf `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-616">At entry to the method, the exception-handler location and the exception are both set to `Nothing`.</span></span> <span data-ttu-id="ac6c8-617">Wenn eine Ausnahme in einer Methode ausgelöst wird, die unstrukturierte Ausnahmebehandlung verwendet wird, befindet sich ein Verweis auf das Ausnahmeobjekt in das Objekt, das von der Funktion zurückgegebene `Microsoft.VisualBasic.Information.Err`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-617">When an exception is thrown in a method that uses unstructured exception handling, a reference to the exception object is stored in the object returned by the function `Microsoft.VisualBasic.Information.Err`.</span></span>

<span data-ttu-id="ac6c8-618">Unstrukturierte Fehlerbehandlung-Anweisungen sind in Iterator- oder Async-Methoden nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-618">Unstructured error handling statements are not allowed in iterator or async methods.</span></span>


#### <a name="error-statement"></a><span data-ttu-id="ac6c8-619">Error-Anweisung</span><span class="sxs-lookup"><span data-stu-id="ac6c8-619">Error Statement</span></span>

<span data-ttu-id="ac6c8-620">Ein `Error` Anweisung löst einen `System.Exception` Ausnahme, die mit einer Anzahl der Visual Basic 6-Ausnahme.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-620">An `Error` statement throws a `System.Exception` exception containing a Visual Basic 6 exception number.</span></span> <span data-ttu-id="ac6c8-621">Der Ausdruck muss als Wert klassifiziert werden, und der Typ muss implizit in `Integer`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-621">The expression must be classified as a value and its type must be implicitly convertible to `Integer`.</span></span>

```antlr
ErrorStatement
    : 'Error' Expression StatementTerminator
    ;
```

#### <a name="on-error-statement"></a><span data-ttu-id="ac6c8-622">On Error-Anweisung</span><span class="sxs-lookup"><span data-stu-id="ac6c8-622">On Error Statement</span></span>

<span data-ttu-id="ac6c8-623">Ein `On Error` Anweisung ändert die its most recent State für die Behandlung von Ausnahmen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-623">An `On Error` statement modifies the most recent exception-handling state.</span></span>

```antlr
OnErrorStatement
    : 'On' 'Error' ErrorClause StatementTerminator
    ;

ErrorClause
    : 'GoTo' '-' '1'
    | 'GoTo' '0'
    | GoToStatement
    | 'Resume' 'Next'
    ;
```

<span data-ttu-id="ac6c8-624">Sie können auf vier Arten verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-624">It may be used in one of four ways:</span></span>

* <span data-ttu-id="ac6c8-625">`On Error GoTo -1` setzt die letzte Ausnahme auf `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-625">`On Error GoTo -1` resets the most recent exception to `Nothing`.</span></span>

* <span data-ttu-id="ac6c8-626">`On Error GoTo 0` Speicherort für die letzte Ausnahme-Handler setzt `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-626">`On Error GoTo 0` resets the most recent exception-handler location to `Nothing`.</span></span>

* <span data-ttu-id="ac6c8-627">`On Error GoTo LabelName` Legt die Bezeichnung als Speicherort für die letzte Ausnahme-Handler.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-627">`On Error GoTo LabelName` establishes the label as the most recent exception-handler location.</span></span> <span data-ttu-id="ac6c8-628">Diese Anweisung kann nicht in einer Methode verwendet werden, die einen Lambda- oder Abfrageausdruck enthält.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-628">This statement cannot be used in a method that contains a lambda or query expression.</span></span>

* <span data-ttu-id="ac6c8-629">`On Error Resume Next` Richtet die `Resume Next` Verhalten als Speicherort für die letzte Ausnahme-Handler.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-629">`On Error Resume Next` establishes the `Resume Next` behavior as the most recent exception-handler location.</span></span>


#### <a name="resume-statement"></a><span data-ttu-id="ac6c8-630">Resume-Anweisung</span><span class="sxs-lookup"><span data-stu-id="ac6c8-630">Resume Statement</span></span>

<span data-ttu-id="ac6c8-631">Ein `Resume` Anweisung setzt die Ausführung der Anweisung, die die letzte Ausnahme verursacht hat.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-631">A `Resume` statement returns execution to the statement that caused the most recent exception.</span></span>

```antlr
ResumeStatement
    : 'Resume' ResumeClause? StatementTerminator
    ;

ResumeClause
    : 'Next'
    | LabelName
    ;
```

<span data-ttu-id="ac6c8-632">Wenn die `Next` Ausführung Modifizierer angegeben ist, zurückgegeben werden, um die Anweisung, die nach der Anweisung ausgeführt worden wären, die die letzte Ausnahme verursacht hat.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-632">If the `Next` modifier is specified, execution returns to the statement that would have been executed after the statement that caused the most recent exception.</span></span> <span data-ttu-id="ac6c8-633">Wenn Sie ein Bezeichnungsnamen angegeben ist, gibt die Ausführung an die Bezeichnung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-633">If a label name is specified, execution returns to the label.</span></span>

<span data-ttu-id="ac6c8-634">Da die `SyncLock` -Anweisung enthält einen implizite strukturierten Fehlerbehandlung Block `Resume` und `Resume Next` haben spezielle Verhaltensweisen für Ausnahmen, die in auftreten `SyncLock` Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-634">Because the `SyncLock` statement contains an implicit structured error-handling block, `Resume` and `Resume Next` have special behaviors for exceptions that occur in `SyncLock` statements.</span></span> <span data-ttu-id="ac6c8-635">`Resume` Gibt die Ausführung an den Anfang der `SyncLock` -Anweisung während `Resume Next` gibt die Ausführung an die nächste Anweisung nach der `SyncLock` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-635">`Resume` returns execution to the beginning of the `SyncLock` statement, while `Resume Next` returns execution to the next statement following the `SyncLock` statement.</span></span> <span data-ttu-id="ac6c8-636">Beachten Sie z. B. folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-636">For example, consider the following code:</span></span>

```vb
Class LockClass
End Class

Module Test
    Sub Main()
        Dim FirstTime As Boolean = True
        Dim Lock As LockClass = New LockClass()

        On Error GoTo Handler

        SyncLock Lock
            Console.WriteLine("Before exception")
            Throw New Exception()
            Console.WriteLine("After exception")
        End SyncLock

        Console.WriteLine("After SyncLock")
        Exit Sub

Handler:
        If FirstTime Then
            FirstTime = False
            Resume
        Else
            Resume Next
        End If
    End Sub
End Module
```

<span data-ttu-id="ac6c8-637">Das folgende Ergebnis ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-637">It prints the following result.</span></span>

```
Before exception
Before exception
After SyncLock
```

<span data-ttu-id="ac6c8-638">Beim ersten Durchlaufen der `SyncLock` Anweisung `Resume` gibt die Ausführung an den Anfang der `SyncLock` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-638">The first time through the `SyncLock` statement, `Resume` returns execution to the beginning of the `SyncLock` statement.</span></span> <span data-ttu-id="ac6c8-639">Beim zweiten Durchlaufen der `SyncLock` Anweisung `Resume Next` gibt die Ausführung bis zum Ende der `SyncLock` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-639">The second time through the `SyncLock` statement, `Resume Next` returns execution to the end of the `SyncLock` statement.</span></span> <span data-ttu-id="ac6c8-640">`Resume` und `Resume Next` dürfen nicht innerhalb einer `SyncLock` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-640">`Resume` and `Resume Next` are not allowed within a `SyncLock` statement.</span></span>

<span data-ttu-id="ac6c8-641">In allen Fällen bei einem `Resume` -Anweisung ausgeführt wird, ist die letzte Ausnahme festgelegt `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-641">In all cases, when a `Resume` statement is executed, the most recent exception is set to `Nothing`.</span></span> <span data-ttu-id="ac6c8-642">Wenn eine `Resume` -Anweisung wird ausgeführt, ohne die letzte eine Ausnahme ist, löst die Anweisung eine `System.Exception` Ausnahme, die mit der Anzahl der Visual Basic-Fehler `20` (ohne Fehler fortsetzen).</span><span class="sxs-lookup"><span data-stu-id="ac6c8-642">If a `Resume` statement is executed with no most recent exception, the statement raises a `System.Exception` exception containing the Visual Basic error number `20` (Resume without error).</span></span>


## <a name="branch-statements"></a><span data-ttu-id="ac6c8-643">Branchanweisungen</span><span class="sxs-lookup"><span data-stu-id="ac6c8-643">Branch Statements</span></span>

<span data-ttu-id="ac6c8-644">Branchanweisungen ändern Sie den Ablauf der Ausführung in einer Methode.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-644">Branch statements modify the flow of execution in a method.</span></span> <span data-ttu-id="ac6c8-645">Es gibt sechs branchanweisungen ein:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-645">There are six branch statements:</span></span>

1. <span data-ttu-id="ac6c8-646">Ein `GoTo` Anweisung wird die Ausführung an der angegebenen Bezeichnung in der Methode zu übertragen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-646">A `GoTo` statement causes execution to transfer to the specified label in the method.</span></span> <span data-ttu-id="ac6c8-647">Es ist nicht zulässig, `GoTo` in einem `Try`, `Using`, `SyncLock`, `With`, `For` oder `For Each` Block noch in einer Schleife der Blöcke, wenn eine lokale Variable des Blocks in einem Lambda-Ausdruck oder eine LINQ-Ausdruck erfasst wird.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-647">It is not allowed to `GoTo` into a `Try`, `Using`, `SyncLock`, `With`, `For` or `For Each` block, nor into any loop block if a local variable of that block is captured in a lambda or LINQ expression.</span></span>
2. <span data-ttu-id="ac6c8-648">Ein `Exit` -Anweisung überträgt die Ausführung an die nächste Anweisung nach dem Ende des direkt enthaltenden Block-Anweisung der angegebenen Art.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-648">An `Exit` statement transfers execution to the next statement after the end of the immediately containing block statement of the specified kind.</span></span> <span data-ttu-id="ac6c8-649">Wenn der Block ist die Methode, ablaufsteuerung beendet die Methode an, wie im Abschnitt beschrieben [Ablaufsteuerung](statements.md#control-flow).</span><span class="sxs-lookup"><span data-stu-id="ac6c8-649">If the block is the method block, then control flow exits the method as described in Section [Control Flow](statements.md#control-flow).</span></span> <span data-ttu-id="ac6c8-650">Wenn die `Exit` Anweisung befindet sich nicht in der Art von Block angegeben in der Anweisung ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-650">If the `Exit` statement is not contained within the kind of block specified in the statement, a compile-time error occurs.</span></span>
3. <span data-ttu-id="ac6c8-651">Ein `Continue` -Anweisung überträgt die Ausführung bis zum Ende des direkt enthaltenden Block Loop-Anweisung der angegebenen Art.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-651">A `Continue` statement transfers execution to the end of the immediately containing block loop statement of the specified kind.</span></span> <span data-ttu-id="ac6c8-652">Wenn die `Continue` Anweisung befindet sich nicht in der Art von Block angegeben in der Anweisung ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-652">If the `Continue` statement is not contained within the kind of block specified in the statement, a compile-time error occurs.</span></span>
4. <span data-ttu-id="ac6c8-653">Ein `Stop` Anweisung bewirkt, dass eine Debuggerausnahme auftritt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-653">A `Stop` statement causes a debugger exception to occur.</span></span>
5. <span data-ttu-id="ac6c8-654">Ein `End` -Anweisung beendet die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-654">An `End` statement terminates the program.</span></span> <span data-ttu-id="ac6c8-655">Finalizer werden vor dem Herunterfahren ausgeführt, aber die finally-Blöcken der derzeit ausgeführten `Try` Anweisungen werden nicht ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-655">Finalizers are run before shutdown, but the finally blocks of any currently executing `Try` statements are not executed.</span></span> <span data-ttu-id="ac6c8-656">Diese Anweisung kann nicht in Programmen verwendet werden, die keine ausführbare Datei (z. B. DLLs) sind.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-656">This statement may not be used in programs that are not executable (for example, DLLs).</span></span>
6. <span data-ttu-id="ac6c8-657">Ein `Return` Anweisung ohne Ausdruck entspricht einer `Exit Sub` oder `Exit Function` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-657">A `Return` statement with no expression is equivalent to an `Exit Sub` or `Exit Function` statement.</span></span> <span data-ttu-id="ac6c8-658">Ein `Return` -Anweisung mit einem Ausdruck ist nur zulässig, in eine normale Methode, die eine Funktion ist oder in einer asynchronen Methode, die eine Funktion mit Rückgabetyp `Task(Of T)` für einige `T`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-658">A `Return` statement with an expression is only allowed in a regular method that is a function, or in an async method that is a function with return type `Task(Of T)` for some `T`.</span></span> <span data-ttu-id="ac6c8-659">Der Ausdruck muss klassifiziert werden, als ein Wert, der implizit in den *Funktion Rückgabevariablen* (im Fall von regulären Methoden) oder die *aufgabenrückgabevariable* (im Fall von Async-Methoden) .</span><span class="sxs-lookup"><span data-stu-id="ac6c8-659">The expression must be classified as a value which is implicitly convertible to the *function return variable* (in the case of regular methods) or to the *task return variable* (in the case of async methods).</span></span> <span data-ttu-id="ac6c8-660">Das Verhalten ist der Ausdruck, ausgewertet werden soll Sie in der Rückgabevariablen zu speichern, anschließend führen Sie ein implizites `Exit Function` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-660">Its behavior is to evaluate its expression, then store it in the return variable, then execute an implicit `Exit Function` statement.</span></span>

```antlr
BranchStatement
    : GoToStatement
    | ExitStatement
    | ContinueStatement
    | StopStatement
    | EndStatement
    | ReturnStatement
    ;

GoToStatement
    : 'GoTo' LabelName StatementTerminator
    ;

ExitStatement
    : 'Exit' ExitKind StatementTerminator
    ;

ExitKind
    : 'Do' | 'For' | 'While' | 'Select' | 'Sub' | 'Function' | 'Property' | 'Try'
    ;

ContinueStatement
    : 'Continue' ContinueKind StatementTerminator
    ;

ContinueKind
    : 'Do' | 'For' | 'While'
    ;

StopStatement
    : 'Stop' StatementTerminator
    ;

EndStatement
    : 'End' StatementTerminator
    ;

ReturnStatement
    : 'Return' Expression? StatementTerminator
    ;
```

## <a name="array-handling-statements"></a><span data-ttu-id="ac6c8-661">Array-Fehlerbehandlungsanweisungen</span><span class="sxs-lookup"><span data-stu-id="ac6c8-661">Array-Handling Statements</span></span>

<span data-ttu-id="ac6c8-662">Zwei Anweisungen vereinfachen die Arbeit mit Arrays: `ReDim` Anweisungen und `Erase` Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-662">Two statements simplify working with arrays: `ReDim` statements and `Erase` statements.</span></span>

```antlr
ArrayHandlingStatement
    : RedimStatement
    | EraseStatement
    ;
```

### <a name="redim-statement"></a><span data-ttu-id="ac6c8-663">ReDim-Anweisung</span><span class="sxs-lookup"><span data-stu-id="ac6c8-663">ReDim Statement</span></span>

<span data-ttu-id="ac6c8-664">Ein `ReDim` Anweisung instanziiert die neuen Arrays.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-664">A `ReDim` statement instantiates new arrays.</span></span>

```antlr
RedimStatement
    : 'ReDim' 'Preserve'? RedimClauses StatementTerminator
    ;

RedimClauses
    : RedimClause ( Comma RedimClause )*
    ;

RedimClause
    : Expression ArraySizeInitializationModifier
    ;
```

<span data-ttu-id="ac6c8-665">Jede Klausel in der Anweisung muss als eine Variable oder ein Eigenschaftenzugriff, dessen Typ ein Arraytyp ist, klassifiziert oder `Object`, und eine Liste der Arraygrenzen gefolgt werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-665">Each clause in the statement must be classified as a variable or a property access whose type is an array type or `Object`, and be followed by a list of array bounds.</span></span> <span data-ttu-id="ac6c8-666">Die Anzahl der Grenzen muss mit dem Datentyp der Variablen konsistent sein; ist eine beliebige Anzahl von Grenzen für zulässig `Object`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-666">The number of the bounds must be consistent with the type of the variable; any number of bounds is allowed for `Object`.</span></span> <span data-ttu-id="ac6c8-667">Ein Array ist zur Laufzeit instanziiert, die für die einzelnen Ausdrücke von links nach rechts mit der angegebenen Begrenzungen und klicken Sie dann die Variable oder Eigenschaft zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-667">At run time, an array is instantiated for each expression from left to right with the specified bounds and then assigned to the variable or property.</span></span> <span data-ttu-id="ac6c8-668">Wenn der Variablentyp ist `Object`, die Anzahl der Dimensionen ist die Anzahl der angegebenen Dimensionen und den Elementtyp des Arrays ist `Object`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-668">If the variable type is `Object`, the number of dimensions is the number of dimensions specified, and the array element type is `Object`.</span></span> <span data-ttu-id="ac6c8-669">Tritt auf, ein Fehler während der Kompilierung, wenn die angegebene Anzahl von Dimensionen, die zur Laufzeit mit der Variablen oder Eigenschaft nicht kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-669">If the given number of dimensions is incompatible with the variable or property at run time a compile-time error occurs.</span></span> <span data-ttu-id="ac6c8-670">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-670">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim o As Object
        Dim b() As Byte
        Dim i(,) As Integer

        ' The next two statements are equivalent.
        ReDim o(10,30)
        o = New Object(10, 30) {}

        ' The next two statements are equivalent.
        ReDim b(10)
        b = New Byte(10) {}

        ' Error: Incorrect number of dimensions.
        ReDim i(10, 30, 40)
    End Sub
End Module
```

<span data-ttu-id="ac6c8-671">Wenn die `Preserve` Schlüsselwort angegeben ist, und klicken Sie dann die Ausdrücke zudem klassifizierbare als Wert muss, und die neue Größe für jede Dimension, mit Ausnahme des letzten muss die Größe des vorhandenen Arrays identisch sein.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-671">If the `Preserve` keyword is specified, then the expressions must also be classifiable as a value, and the new size for each dimension except for the rightmost one must be the same as the size of the existing array.</span></span> <span data-ttu-id="ac6c8-672">Die Werte im vorhandenen Array in das neue Array kopiert werden: Wenn das neue Array kleiner ist, werden die vorhandenen Werte verworfen. Wenn das neue Array größer ist, werden zusätzliche Elemente auf den Standardwert, der den Elementtyp des Arrays initialisiert.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-672">The values in the existing array are copied into the new array: if the new array is smaller, the existing values are discarded; if the new array is bigger, the extra elements will be initialized to the default value of the element type of the array.</span></span> <span data-ttu-id="ac6c8-673">Beachten Sie z. B. folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-673">For example, consider the following code:</span></span>

```vb
Module Test
    Sub Main()
        Dim x(5, 5) As Integer

        x(3, 3) = 3

        ReDim Preserve x(5, 6)
        Console.WriteLine(x(3, 3) & ", " & x(3, 6))
    End Sub
End Module
```

<span data-ttu-id="ac6c8-674">Gibt das folgende Ergebnis:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-674">It prints the following result:</span></span>

```
3, 0
```

<span data-ttu-id="ac6c8-675">Die vorhandenen arrayverweises ein null-Wert zur Laufzeit ist, dass kein Fehler erhält.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-675">If the existing array reference is a null value at run time, no error is given.</span></span> <span data-ttu-id="ac6c8-676">Als die Dimension, auf die ganz rechts, wenn die Größe einer Dimension geändert wird eine `System.ArrayTypeMismatchException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-676">Other than the rightmost dimension, if the size of a dimension changes, a `System.ArrayTypeMismatchException` will be thrown.</span></span>

<span data-ttu-id="ac6c8-677">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="ac6c8-677">__Note.__</span></span> <span data-ttu-id="ac6c8-678">`Preserve` ist ein reserviertes Wort.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-678">`Preserve` is not a reserved word.</span></span>


### <a name="erase-statement"></a><span data-ttu-id="ac6c8-679">Erase-Anweisung</span><span class="sxs-lookup"><span data-stu-id="ac6c8-679">Erase Statement</span></span>

<span data-ttu-id="ac6c8-680">Ein `Erase` Anweisung legt alle Arrayvariablen oder Eigenschaften, die in der Anweisung angegebene `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-680">An `Erase` statement sets each of the array variables or properties specified in the statement to `Nothing`.</span></span> <span data-ttu-id="ac6c8-681">Jeder Ausdruck in der Anweisung muss klassifiziert werden, wie eine Variable oder eine Eigenschaft, deren Typ ein Arraytyp ist, oder `Object`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-681">Each expression in the statement must be classified as a variable or property access whose type is an array type or `Object`.</span></span> <span data-ttu-id="ac6c8-682">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-682">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim x() As Integer = New Integer(5) {}

        ' The following two statements are equivalent.
        Erase x
        x = Nothing
    End Sub
End Module
```

```antlr
EraseStatement
    : 'Erase' EraseExpressions StatementTerminator
    ;

EraseExpressions
    : Expression ( Comma Expression )*
    ;
```

## <a name="using-statement"></a><span data-ttu-id="ac6c8-683">Using-Anweisung</span><span class="sxs-lookup"><span data-stu-id="ac6c8-683">Using statement</span></span>

<span data-ttu-id="ac6c8-684">Instanzen von Typen werden automatisch vom Garbage Collector freigegeben, wenn eine Sammlung ausgeführt wird, und keine aktiven Verweise auf die Instanz gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-684">Instances of types are automatically released by the garbage collector when a collection is run and no live references to the instance are found.</span></span> <span data-ttu-id="ac6c8-685">Wenn ein Typ für eine besonders nützliche und knappe Ressource (z. B. Datenbankverbindungen oder Dateihandles) enthält, kann es nicht wünschenswert sein, warten Sie, bis die nächste Garbagecollection einer bestimmten Instanz des Typs bereinigen, die nicht mehr verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-685">If a type holds on a particularly valuable and scarce resource (such as database connections or file handles), it may not be desirable to wait until the next garbage collection to clean up a particular instance of the type that is no longer in use.</span></span> <span data-ttu-id="ac6c8-686">Um eine einfache Möglichkeit des Freigebens von Ressourcen, bevor Sie eine Sammlung bereitstellen, kann ein Typ implementieren die `System.IDisposable` Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-686">To provide a lightweight way of releasing resources before a collection, a type may implement the `System.IDisposable` interface.</span></span> <span data-ttu-id="ac6c8-687">Ein Typ, der ist dies der Fall ist, stellt eine `Dispose` -Methode, die aufgerufen werden kann, um wertvolle Ressourcen freigegeben werden, sofort, die als solche zu erzwingen:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-687">A type that does so exposes a `Dispose` method that can be called to force valuable resources to be released immediately, as such:</span></span>

```vb
Module Test
    Sub Main()
        Dim x As DBConnection = New DBConnection("...")

        ' Do some work
        ...

        x.Dispose()        ' Free the connection
    End Sub
End Module
```

<span data-ttu-id="ac6c8-688">Die `Using` Anweisung automatisiert den Prozess der Erwerb einer Ressourcensatzes, eine Reihe von Anweisungen ausgeführt und dann die Ressource freigegeben.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-688">The `Using` statement automates the process of acquiring a resource, executing a set of statements, and then disposing of the resource.</span></span> <span data-ttu-id="ac6c8-689">Die Anweisung kann zwei Formen annehmen: ein Element die Ressource ist eine lokale Variable, die als Teil der Anweisung deklariert und als einer regulären lokalen Variablendeklaration-Anweisung behandelt Bei der anderen ist die Ressource für das Ergebnis eines Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-689">The statement can take two forms: in one, the resource is a local variable declared as a part of the statement and treated as a regular local variable declaration statement; in the other, the resource is the result of an expression.</span></span>

```antlr
UsingStatement
    : 'Using' UsingResources StatementTerminator
      Block?
      'End' 'Using' StatementTerminator
    ;

UsingResources
    : VariableDeclarators
    | Expression
    ;
```

<span data-ttu-id="ac6c8-690">Wenn die Ressource wird von einer lokalen Variablendeklaration-Anweisung, und klicken Sie dann der Typ des der Deklaration lokaler Variablen muss ein Typ, der implizit in konvertiert werden kann `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-690">If the resource is a local variable declaration statement then the type of the local variable declaration must be a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="ac6c8-691">Die deklarierten lokalen Variablen sind schreibgeschützt, beschränkt auf die `Using` Anweisung blockieren und muss einen Initialisierer enthalten.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-691">The declared local variables are read-only, scoped to the `Using` statement block and must include an initializer.</span></span> <span data-ttu-id="ac6c8-692">Wenn die Ressource das Ergebnis eines Ausdrucks ist wird der Ausdruck muss als Wert klassifiziert werden und muss von einem Typ sein, der implizit konvertiert werden kann `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-692">If the resource is the result of an expression then the expression must be classified as a value and must be of a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="ac6c8-693">Der Ausdruck wird zu Beginn der Anweisung nur einmal ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-693">The expression is only evaluated once, at the beginning of the statement.</span></span>

<span data-ttu-id="ac6c8-694">Die `Using` Block befindet sich implizit durch eine `Try` dessen finally-block Anweisung ruft die Methode `IDisposable.Dispose` für die Ressource.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-694">The `Using` block is implicitly contained by a `Try` statement whose finally block calls the method `IDisposable.Dispose` on the resource.</span></span> <span data-ttu-id="ac6c8-695">Dadurch wird sichergestellt, dass die Ressource freigegeben wird, selbst wenn eine Ausnahme ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-695">This ensures the resource is disposed even when an exception is thrown.</span></span> <span data-ttu-id="ac6c8-696">Daher ist ungültig in branch ein `Using` Blockieren von außerhalb des Blocks auf, und ein `Using` Block wird als einzelne Anweisung behandelt, im Rahmen `Resume` und `Resume Next`.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-696">As a result, it is invalid to branch into a `Using` block from outside of the block, and a `Using` block is treated as a single statement for the purposes of `Resume` and `Resume Next`.</span></span> <span data-ttu-id="ac6c8-697">Wenn die Ressource ist `Nothing`, klicken Sie dann ohne Aufruf `Dispose` erfolgt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-697">If the resource is `Nothing`, then no call to `Dispose` is made.</span></span> <span data-ttu-id="ac6c8-698">Das Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-698">Thus, the example:</span></span>

```vb
Using f As C = New C()
    ...
End Using
```

<span data-ttu-id="ac6c8-699">identisch mit folgendem Ausdruck:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-699">is equivalent to:</span></span>

```vb
Dim f As C = New C()
Try
    ...
Finally
    If f IsNot Nothing Then
        f.Dispose()
    End If
End Try
```

<span data-ttu-id="ac6c8-700">Ein `Using` -Anweisung mit einer lokalen Variablendeklaration-Anweisung kann mehrere Ressourcen abrufen, zu einem Zeitpunkt, d.h. entspricht dies geschachtelten `Using` Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-700">A `Using` statement that has a local variable declaration statement may acquire multiple resources at a time, which is equivalent to nested `Using` statements.</span></span>  <span data-ttu-id="ac6c8-701">Z. B. eine `Using` -Anweisung der Form:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-701">For example, a `Using` statement of the form:</span></span>

```vb
Using r1 As R = New R(), r2 As R = New R()
    r1.F()
    r2.F()
End Using
```

<span data-ttu-id="ac6c8-702">identisch mit folgendem Ausdruck:</span><span class="sxs-lookup"><span data-stu-id="ac6c8-702">is equivalent to:</span></span>

```vb
Using r1 As R = New R()
    Using r2 As R = New R()
        r1.F()
        r2.F()
    End Using
End Using
```


## <a name="await-statement"></a><span data-ttu-id="ac6c8-703">Await-Anweisung</span><span class="sxs-lookup"><span data-stu-id="ac6c8-703">Await Statement</span></span>

<span data-ttu-id="ac6c8-704">Eine Await-Anweisung weist die gleiche Syntax als Ausdruck "await"-Operator (Abschnitt [Await-Operator](expressions.md#await-operator)), ist nur in Methoden, die auch, Await-Ausdrücken ermöglichen zulässig und hat das gleiche Verhalten wie ein Await-Operator-Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-704">An await statement has the same syntax as an await operator expression (Section [Await Operator](expressions.md#await-operator)), is allowed only in methods that also allow await expressions, and has the same behavior as an await operator expression.</span></span>

<span data-ttu-id="ac6c8-705">Allerdings können sie als Wert oder "void" klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-705">However, it may be classified as either a value or void.</span></span> <span data-ttu-id="ac6c8-706">Jeder Wert mit dem Ergebnis der Auswertung des Ausdrucks "await"-Operator wird verworfen.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-706">Any value resulting from evaluation of the await operator expression is discarded.</span></span>

```antlr
AwaitStatement
    : AwaitOperatorExpression StatementTerminator
    ;
```

## <a name="yield-statement"></a><span data-ttu-id="ac6c8-707">Yield-Anweisung</span><span class="sxs-lookup"><span data-stu-id="ac6c8-707">Yield Statement</span></span>

<span data-ttu-id="ac6c8-708">Yield-Anweisungen beziehen sich auf die Iterator-Methoden, die in Abschnitt beschrieben werden [Iteratormethoden](statements.md#iterator-methods).</span><span class="sxs-lookup"><span data-stu-id="ac6c8-708">Yield statements are related to iterator methods, which are described in Section [Iterator Methods](statements.md#iterator-methods).</span></span>

```antlr
YieldStatement
    : 'Yield' Expression StatementTerminator
    ;
```

<span data-ttu-id="ac6c8-709">`Yield` ist ein reserviertes Wort, verfügt die unmittelbar einschließende Methode oder der Lambda-Ausdruck, in dem er angezeigt wird, ein `Iterator` Modifizierer, und wenn die `Yield` angezeigt wird, `Iterator` Modifizierer; es an anderer Stelle nicht reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-709">`Yield` is a reserved word if the immediately enclosing method or lambda expression in which it appears has an `Iterator` modifier, and if the `Yield` appears after that `Iterator` modifier; it is unreserved elsewhere.</span></span> <span data-ttu-id="ac6c8-710">Es ist auch in präprozessoranweisungen nicht reserviert.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-710">It is also unreserved in preprocessor directives.</span></span> <span data-ttu-id="ac6c8-711">Die Yield-Anweisung ist nur im Text einer Methode oder einem Lambda-Ausdruck zulässig, in denen es sich um ein reserviertes Wort ist.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-711">The yield statement is only allowed in the body of a method or lambda expression where it is a reserved word.</span></span> <span data-ttu-id="ac6c8-712">In der unmittelbar einschließenden Methode oder einem Lambdaausdruck, die Yield-Anweisung kann nicht auftreten, innerhalb des Texts einer `Catch` oder `Finally` Block noder innerhalb der Text der ein `SyncLock` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-712">Within the immediately enclosing method or lambda, the yield statement may not occur inside the body of a `Catch` or `Finally` block, nor inside the body of a `SyncLock` statement.</span></span>

<span data-ttu-id="ac6c8-713">Die Yield-Anweisung erhält einen einzelnen Ausdruck, der als Wert klassifiziert werden muss und dessen Typ implizit in den Typ des, der *aktuelle Iteratorvariable* (Abschnitt [Iteratormethoden](statements.md#iterator-methods)) von der Einschließen von Iterator-Methode.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-713">The yield statement takes a single expression which must be classified as a value and whose type is implicitly convertible to the type of the *iterator current variable* (Section [Iterator Methods](statements.md#iterator-methods)) of its enclosing iterator method.</span></span>

<span data-ttu-id="ac6c8-714">Ablaufsteuerung nur erreicht eine `Yield` Anweisung bei der `MoveNext` Methode für ein Iterator-Objekt aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-714">Control flow only ever reaches a `Yield` statement when the `MoveNext` method is invoked on an iterator object.</span></span> <span data-ttu-id="ac6c8-715">(Dies ist, da eine Instanz der Iterator-Methode immer nur die Anweisungen aufgrund von ausgeführt wird die `MoveNext` oder `Dispose` Methoden, die von einem iteratorobjekt aufgerufen wird und die `Dispose` Methode wird nur einmal ausführen von Code in `Finally` -Blöcken, in dem `Yield` ist nicht zulässig).</span><span class="sxs-lookup"><span data-stu-id="ac6c8-715">(This is because an iterator method instance only ever executes its statements due to the `MoveNext` or `Dispose` methods being called on an iterator object; and the `Dispose` method will only ever execute code in `Finally` blocks, where `Yield` is not allowed).</span></span>

<span data-ttu-id="ac6c8-716">Wenn eine `Yield` Anweisung ausgeführt wird, wird der Ausdruck ausgewertet und in gespeicherten der *aktuelle Iteratorvariable* von der Methode, mit dem iteratorobjekt verknüpfte Iteratorinstanz.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-716">When a `Yield` statement is executed, its expression is evaluated and stored in the *iterator current variable* of the iterator method instance associated with that iterator object.</span></span> <span data-ttu-id="ac6c8-717">Der Wert `True` wird zurückgegeben, um die aufrufende Instanz für `MoveNext`, und der Kontrollpunkt der dieser Instanz beendet wird, gelangt sind, bis des nächsten Aufrufs von `MoveNext` auf dem Iterator-Objekt.</span><span class="sxs-lookup"><span data-stu-id="ac6c8-717">The value `True` is returned to the invoker of `MoveNext`, and the control point of this instance stops advancing until the next invocation of `MoveNext` on the iterator object.</span></span>

