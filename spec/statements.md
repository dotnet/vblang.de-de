---
ms.openlocfilehash: e49e116e60e724bcd8f1148c8aad9d11dfc92e74
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306056"
---
# <a name="statements"></a><span data-ttu-id="8f89f-101">Anweisungen</span><span class="sxs-lookup"><span data-stu-id="8f89f-101">Statements</span></span>

<span data-ttu-id="8f89f-102">-Anweisungen stellen ausführbaren Code dar.</span><span class="sxs-lookup"><span data-stu-id="8f89f-102">Statements represent executable code.</span></span>

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

<span data-ttu-id="8f89f-103">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="8f89f-103">__Note.__</span></span> <span data-ttu-id="8f89f-104">Der Microsoft Visual Basic-Compiler lässt nur Anweisungen zu, die mit einem Schlüsselwort oder einem Bezeichner beginnen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-104">The Microsoft Visual Basic Compiler only allows statements which start with a keyword or an identifier.</span></span> <span data-ttu-id="8f89f-105">Folglich ist beispielsweise die Aufruf Anweisung "`Call (Console).WriteLine`" zulässig, aber die Aufruf Anweisung "`(Console).WriteLine`" ist nicht.</span><span class="sxs-lookup"><span data-stu-id="8f89f-105">Thus, for instance, the invocation statement "`Call (Console).WriteLine`" is allowed, but the invocation statement "`(Console).WriteLine`" is not.</span></span>

## <a name="control-flow"></a><span data-ttu-id="8f89f-106">Ablaufsteuerung</span><span class="sxs-lookup"><span data-stu-id="8f89f-106">Control Flow</span></span>

<span data-ttu-id="8f89f-107">Die Ablauf *Steuerung* ist die Sequenz, in der-Anweisungen und-Ausdrücke ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-107">*Control flow* is the sequence in which statements and expressions are executed.</span></span> <span data-ttu-id="8f89f-108">Die Ausführungsreihenfolge hängt von der jeweiligen Anweisung oder dem Ausdruck ab.</span><span class="sxs-lookup"><span data-stu-id="8f89f-108">The order of execution depends on the particular statement or expression.</span></span>

<span data-ttu-id="8f89f-109">Wenn Sie z. b. einen Additions Operator (Section Additions [Operator](expressions.md#addition-operator)) auswerten, wird zuerst der linke Operand ausgewertet, dann der rechte Operand und dann der Operator selbst.</span><span class="sxs-lookup"><span data-stu-id="8f89f-109">For example, when evaluating an addition operator (Section [Addition Operator](expressions.md#addition-operator)), first the left operand is evaluated, then the right operand, and then the operator itself.</span></span> <span data-ttu-id="8f89f-110">-Blöcke werden ausgeführt (Abschnitts [Blöcke und Bezeichnungen](statements.md#blocks-and-labels)), indem Sie zuerst Ihre erste unter Anweisung ausführen und dann nacheinander durch die-Anweisungen des-Blocks fortfahren.</span><span class="sxs-lookup"><span data-stu-id="8f89f-110">Blocks are executed (Section [Blocks and Labels](statements.md#blocks-and-labels)) by first executing their first substatement, and then proceeding one by one through the statements of the block.</span></span>

<span data-ttu-id="8f89f-111">Implizit in dieser Reihenfolge ist das Konzept eines *Steuerungs Punkts*, bei dem es sich um den nächsten auszuführenden Vorgang handelt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-111">Implicit in this ordering is the concept of a *control point*, which is the next operation to be executed.</span></span> <span data-ttu-id="8f89f-112">Wenn eine Methode aufgerufen wird (oder "aufgerufen"), wird eine *Instanz* der Methode erstellt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-112">When a method is invoked (or "called"), we say it creates an *instance* of the method.</span></span> <span data-ttu-id="8f89f-113">Eine Methoden Instanz besteht aus einer eigenen Kopie der Methoden Parameter und lokalen Variablen sowie einem eigenen Kontrollpunkt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-113">A method instance consists of its own copy of the method's parameters and local variables, and its own control point.</span></span>

### <a name="regular-methods"></a><span data-ttu-id="8f89f-114">Reguläre Methoden</span><span class="sxs-lookup"><span data-stu-id="8f89f-114">Regular Methods</span></span>

<span data-ttu-id="8f89f-115">Im folgenden finden Sie ein Beispiel für eine reguläre Methode.</span><span class="sxs-lookup"><span data-stu-id="8f89f-115">Here is an example of a regular method</span></span>

```vb
Function Test() As Integer
    Console.WriteLine("hello")
    Return 1
End Sub

Dim x = Test()    ' invokes the function, prints "hello", assigns 1 to x
```

<span data-ttu-id="8f89f-116">Wenn eine reguläre Methode aufgerufen wird,</span><span class="sxs-lookup"><span data-stu-id="8f89f-116">When a regular method is invoked,</span></span>

1. <span data-ttu-id="8f89f-117">Zuerst wird eine Instanz der Methode erstellt, die für diesen Aufruf spezifisch ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-117">First an instance of the method is created specific to that invocation.</span></span> <span data-ttu-id="8f89f-118">Diese Instanz enthält eine Kopie aller Parameter und lokalen Variablen der Methode.</span><span class="sxs-lookup"><span data-stu-id="8f89f-118">This instance includes a copy of all parameters and local variables of the method.</span></span>
2. <span data-ttu-id="8f89f-119">Anschließend werden alle zugehörigen Parameter mit den angegebenen Werten und allen lokalen Variablen für die Standardwerte ihrer Typen initialisiert.</span><span class="sxs-lookup"><span data-stu-id="8f89f-119">Then all of its parameters are initialized to the supplied values, and all of its local variables to the default values of their types.</span></span>
3. <span data-ttu-id="8f89f-120">Im Fall eines `Function` wird auch eine implizite lokale Variable initialisiert, die als *Funktions Rückgabe Variable* bezeichnet wird, deren Name der Name der Funktion ist, deren Typ der Rückgabetyp der Funktion und dessen ursprünglicher Wert der Standardwert des Typs ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-120">In the case of a `Function`, an implicit local variable is also initialized called the *function return variable* whose name is the function's name, whose type is the return type of the function and whose initial value is the default of its type.</span></span>
4. <span data-ttu-id="8f89f-121">Der Kontrollpunkt der Methoden Instanz wird dann bei der ersten Anweisung des Methoden Texts festgelegt, und der Methoden Text beginnt sofort mit der Ausführung (Abschnitts [Blöcke und Bezeichnungen](statements.md#blocks-and-labels)).</span><span class="sxs-lookup"><span data-stu-id="8f89f-121">The method instance's control point is then set at the first statement of the method body, and the method body immediately starts to execute from there (Section [Blocks and Labels](statements.md#blocks-and-labels)).</span></span>

<span data-ttu-id="8f89f-122">Wenn die Ablauf Steuerung den Methoden Text normal beendet, indem Sie den `End Function` oder `End Sub` erreicht, der das Ende markiert, oder durch eine explizite `Return`-oder `Exit`-Anweisung, wird die Ablauf Steuerung an den Aufrufer der Methoden Instanz zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="8f89f-122">When control flow exits the method body normally - through reaching the `End Function` or `End Sub` that mark its end, or through an explicit `Return` or `Exit` statement - control flow returns to the caller of the method instance.</span></span> <span data-ttu-id="8f89f-123">Wenn eine Funktions Rückgabe Variable vorhanden ist, ist das Ergebnis des auffalls der endgültige Wert dieser Variablen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-123">If there is a function return variable, the result of the invocation is the final value of this variable.</span></span>

<span data-ttu-id="8f89f-124">Wenn die Ablauf Steuerung den Methoden Text durch eine nicht behandelte Ausnahme beendet, wird diese Ausnahme an den Aufrufer weitergegeben.</span><span class="sxs-lookup"><span data-stu-id="8f89f-124">When control flow exits the method body through an unhandled exception, that exception is propagated to the caller.</span></span>

<span data-ttu-id="8f89f-125">Nachdem die Ablauf Steuerung beendet wurde, gibt es keine Live Verweise mehr auf die Methoden Instanz.</span><span class="sxs-lookup"><span data-stu-id="8f89f-125">After control flow has exited, there are no longer any live references to the method instance.</span></span> <span data-ttu-id="8f89f-126">Wenn die Methoden Instanz nur Verweise auf Ihre Kopie von lokalen Variablen oder Parametern enthielt, werden Sie möglicherweise in die Garbage Collection aufgenommen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-126">If the method instance held the only references to its copy of local variables or parameters, then they may be garbage collected.</span></span>

### <a name="iterator-methods"></a><span data-ttu-id="8f89f-127">Iteratormethoden</span><span class="sxs-lookup"><span data-stu-id="8f89f-127">Iterator Methods</span></span>

<span data-ttu-id="8f89f-128">Iteratormethoden werden als bequeme Methode zum Generieren einer Sequenz verwendet, die von der `For Each`-Anweisung verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="8f89f-128">Iterator methods are used as a convenient way to generate a sequence, one which can be consumed by the `For Each` statement.</span></span> <span data-ttu-id="8f89f-129">Iteratormethoden verwenden die `Yield`-Anweisung (Section [yield-Anweisung](statements.md#yield-statement)), um Elemente der Sequenz bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-129">Iterator methods use the `Yield` statement (Section [Yield Statement](statements.md#yield-statement)) to provide elements of the sequence.</span></span> <span data-ttu-id="8f89f-130">(Eine Iteratormethode ohne `Yield`-Anweisungen erzeugt eine leere Sequenz).</span><span class="sxs-lookup"><span data-stu-id="8f89f-130">(An iterator method with no `Yield` statements will produce an empty sequence).</span></span> <span data-ttu-id="8f89f-131">Im folgenden finden Sie ein Beispiel für eine Iteratormethode:</span><span class="sxs-lookup"><span data-stu-id="8f89f-131">Here is an example of an iterator method:</span></span>

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

<span data-ttu-id="8f89f-132">Wenn eine Iteratormethode aufgerufen wird, deren Rückgabetyp `IEnumerator(Of T)` ist,</span><span class="sxs-lookup"><span data-stu-id="8f89f-132">When an iterator method is invoked whose return type is `IEnumerator(Of T)`,</span></span>

1. <span data-ttu-id="8f89f-133">Zuerst wird eine Instanz der Iteratormethode erstellt, die für diesen Aufruf spezifisch ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-133">First an instance of the iterator method is created specific to that invocation.</span></span> <span data-ttu-id="8f89f-134">Diese Instanz enthält eine Kopie aller Parameter und lokalen Variablen der Methode.</span><span class="sxs-lookup"><span data-stu-id="8f89f-134">This instance includes a copy of all parameters and local variables of the method.</span></span>
2. <span data-ttu-id="8f89f-135">Anschließend werden alle zugehörigen Parameter mit den angegebenen Werten und allen lokalen Variablen für die Standardwerte ihrer Typen initialisiert.</span><span class="sxs-lookup"><span data-stu-id="8f89f-135">Then all of its parameters are initialized to the supplied values, and all of its local variables to the default values of their types.</span></span>
3. <span data-ttu-id="8f89f-136">Eine implizite lokale Variable wird auch als *iteratoraktuelle Variable*initialisiert, deren Typ `T` und dessen ursprünglicher Wert der Standardwert des Typs ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-136">An implicit local variable is also initialized called the *iterator current variable*, whose type is `T` and whose initial value is the default of its type.</span></span>
4. <span data-ttu-id="8f89f-137">Der Kontrollpunkt der Methoden Instanz wird dann bei der ersten Anweisung des Methoden Texts festgelegt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-137">The method instance's control point is then set at the first statement of the method body.</span></span>
5. <span data-ttu-id="8f89f-138">Anschließend wird ein *Iteratorobjekt* erstellt, das dieser Methoden Instanz zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-138">An *iterator object* is then created, associated with this method instance.</span></span> <span data-ttu-id="8f89f-139">Das Iteratorobjekt implementiert den deklarierten Rückgabetyp und weist das Verhalten auf, wie unten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="8f89f-139">The iterator object implements the declared return type and has behavior as described below.</span></span>
6. <span data-ttu-id="8f89f-140">Die Ablauf Steuerung wird dann *sofort* im Aufrufer fortgesetzt, und das Ergebnis des Aufrufers ist das Iteratorobjekt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-140">Control flow is then resumed *immediately* in the caller, and the result of the invocation is the iterator object.</span></span> <span data-ttu-id="8f89f-141">Beachten Sie, dass diese Übertragung erfolgt, ohne die iteratormethodeninstanz zu beenden, und führt nicht dazu, dass die Handler schließlich ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-141">Note that this transfer is done without exiting the iterator method instance, and does not cause finally handlers to execute.</span></span> <span data-ttu-id="8f89f-142">Auf die Methoden Instanz wird weiterhin durch das Iteratorobjekt verwiesen, und es wird keine Garbage Collection durchgeführt, solange ein Live Verweis auf das Iteratorobjekt vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-142">The method instance is still referenced by the iterator object, and will not be garbage collected so long as there exists a live reference to the iterator object.</span></span>

<span data-ttu-id="8f89f-143">Beim Zugriff auf die `Current`-Eigenschaft des iteratorobjekts wird die *aktuelle Variable* des aufrubers zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="8f89f-143">When the iterator object's `Current` property is accessed, the *current variable* of the invocation is returned.</span></span>

<span data-ttu-id="8f89f-144">Wenn die `MoveNext`-Methode des iteratorobjekts aufgerufen wird, erstellt der Aufruf keine neue Methoden Instanz.</span><span class="sxs-lookup"><span data-stu-id="8f89f-144">When the iterator object's `MoveNext` method is invoked, the invocation does not create a new method instance.</span></span> <span data-ttu-id="8f89f-145">Stattdessen wird die vorhandene Methoden Instanz (und deren Steuerungspunkt und lokale Variablen und Parameter)-die Instanz verwendet, die beim ersten Aufrufen der Iteratormethode erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="8f89f-145">Instead the existing method instance is used (and its control point and local variables and parameters) - the instance that was created when the iterator method was first invoked.</span></span> <span data-ttu-id="8f89f-146">Die Ablauf Steuerung nimmt die Ausführung am Steuerungspunkt dieser Methoden Instanz wieder auf und verläuft wie gewohnt durch den Text der Iteratormethode.</span><span class="sxs-lookup"><span data-stu-id="8f89f-146">Control flow resumes execution at the control point of that method instance, and proceeds through the body of the iterator method as normal.</span></span>

<span data-ttu-id="8f89f-147">Wenn die `Dispose`-Methode des iteratorobjekts aufgerufen wird, wird wieder die vorhandene Methoden Instanz verwendet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-147">When the iterator object's `Dispose` method is invoked, again the existing method instance is used.</span></span> <span data-ttu-id="8f89f-148">Die Ablauf Steuerung wird am Steuerungspunkt dieser Methoden Instanz fortgesetzt, verhält sich jedoch sofort so, als ob eine `Exit Function`-Anweisung der nächste Vorgang wäre.</span><span class="sxs-lookup"><span data-stu-id="8f89f-148">Control flow resumes at the control point of that method instance, but then immediately behaves as if an `Exit Function` statement were the next operation.</span></span>

<span data-ttu-id="8f89f-149">Die obigen Beschreibungen des Verhaltens des Aufrufs von `MoveNext` oder `Dispose` für ein Iteratorobjekt gelten nur, wenn alle vorherigen Aufrufe von `MoveNext` oder `Dispose` für dieses Iteratorobjekt bereits an ihre Aufrufer zurückgegeben wurden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-149">The above descriptions of behavior for invocation of `MoveNext` or `Dispose` on an iterator object only apply if all previous invocations of `MoveNext` or `Dispose` on that iterator object have already returned to their callers.</span></span> <span data-ttu-id="8f89f-150">Wenn dies nicht der Fall ist, ist das Verhalten nicht definiert.</span><span class="sxs-lookup"><span data-stu-id="8f89f-150">If they haven't, then the behavior is undefined.</span></span>

<span data-ttu-id="8f89f-151">Wenn die Ablauf Steuerung den iteratormethodentext normal beendet, indem die `End Function` erreicht wird, die das Ende oder eine explizite `Return`-oder `Exit`-Anweisung erreicht haben, muss Sie dies im Kontext eines aufhebers der `MoveNext`-oder `Dispose`-Funktion für einen Iterator durchgeführt haben. , um die iteratormethodeninstanz fortzusetzen, und Sie verwendet die-Methoden Instanz, die beim ersten Aufruf der Iteratormethode erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="8f89f-151">When control flow exits the iterator method body normally -- through reaching the `End Function` that mark its end, or through an explicit `Return` or `Exit` statement -- it must have done so in the context of an invocation of `MoveNext` or `Dispose` function on an iterator object to resume the iterator method instance, and it will have been using the method instance that was created when the iterator method was first invoked.</span></span> <span data-ttu-id="8f89f-152">Der Kontrollpunkt dieser Instanz bleibt bei der `End Function`-Anweisung, und die Ablauf Steuerung wird im Aufrufer fortgesetzt. Wenn die Wiederaufnahme durch einen Aufruf von `MoveNext` erfolgt ist, wird der Wert `False` an den Aufrufer zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="8f89f-152">The control point of that instance is left at the `End Function` statement, and control flow resumes in the caller; and if it had been resumed by a call to `MoveNext` then the value `False` is returned to the caller.</span></span>

<span data-ttu-id="8f89f-153">Wenn die Ablauf Steuerung den iteratormethodentext über eine nicht behandelte Ausnahme beendet, wird die Ausnahme an den Aufrufer weitergegeben, der wiederum entweder ein Aufruf von `MoveNext` oder `Dispose` ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-153">When control flow exits the iterator method body through an unhandled exception, then the exception is propagated to the caller, which again will be either an invocation of `MoveNext` or of `Dispose`.</span></span>

<span data-ttu-id="8f89f-154">Wie bei den anderen möglichen Rückgabe Typen einer Iteratorfunktion</span><span class="sxs-lookup"><span data-stu-id="8f89f-154">As for the other possible return types of an iterator function,</span></span>

* <span data-ttu-id="8f89f-155">Wenn eine Iteratormethode aufgerufen wird, deren Rückgabetyp für einige `T` `IEnumerable(Of T)` ist, wird zuerst eine-Instanz erstellt, die für diesen Aufruf der Iteratormethode gilt, und Sie wird mit den angegebenen Werten initialisiert.</span><span class="sxs-lookup"><span data-stu-id="8f89f-155">When an iterator method is invoked whose return type is `IEnumerable(Of T)` for some `T`, an instance is first created -- specific to that invocation of the iterator method -- of all parameters in the method, and they are initialized with the supplied values.</span></span> <span data-ttu-id="8f89f-156">Das Ergebnis des aufbilden ist ein Objekt, das den Rückgabetyp implementiert.</span><span class="sxs-lookup"><span data-stu-id="8f89f-156">The result of the invocation is an an object which implements the return type.</span></span> <span data-ttu-id="8f89f-157">Wenn die `GetEnumerator`-Methode dieses Objekts aufgerufen wird, erstellt Sie eine-Instanz, die für diesen Aufruf von `GetEnumerator`-mit allen Parametern und lokalen Variablen in der Methode spezifisch ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-157">Should this object's `GetEnumerator` method be called, it creates an instance -- specific to that invocation of `GetEnumerator` -- of all parameters and local variables in the method.</span></span> <span data-ttu-id="8f89f-158">Die Parameter werden für die bereits gespeicherten Werte initialisiert, und Sie werden wie für die oben genannte Iteratormethode fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-158">It initializes the parameters to the values already saved, and proceeds as for the iterator method above.</span></span>
* <span data-ttu-id="8f89f-159">Wenn eine Iteratormethode aufgerufen wird, deren Rückgabetyp die nicht generische Schnittstelle `IEnumerator` ist, ist das Verhalten genau so wie bei `IEnumerator(Of Object)`.</span><span class="sxs-lookup"><span data-stu-id="8f89f-159">When an iterator method is invoked whose return type is the non-generic interface `IEnumerator`, the behavior is exactly as for `IEnumerator(Of Object)`.</span></span>
* <span data-ttu-id="8f89f-160">Wenn eine Iteratormethode aufgerufen wird, deren Rückgabetyp die nicht generische Schnittstelle `IEnumerable` ist, ist das Verhalten genau so wie bei `IEnumerable(Of Object)`.</span><span class="sxs-lookup"><span data-stu-id="8f89f-160">When an iterator method is invoked whose return type is the non-generic interface `IEnumerable`, the behavior is exactly as for `IEnumerable(Of Object)`.</span></span>

### <a name="async-methods"></a><span data-ttu-id="8f89f-161">Asynchrone Methoden</span><span class="sxs-lookup"><span data-stu-id="8f89f-161">Async Methods</span></span>

<span data-ttu-id="8f89f-162">Async-Methoden sind eine bequeme Methode zum Ausführen von Aufgaben mit langer Ausführungszeit, ohne beispielsweise die Benutzeroberfläche einer Anwendung zu blockieren.</span><span class="sxs-lookup"><span data-stu-id="8f89f-162">Async methods are a convenient way to do long-running work without for example blocking the UI of an application.</span></span> <span data-ttu-id="8f89f-163">Async ist für *Asynchronous* kurz. Dies bedeutet, dass der Aufrufer der Async-Methode die Ausführung umgehend fort setzt. der endgültige Abschluss der-Instanz der Async-Methode kann jedoch zu einem späteren Zeitpunkt in der Zukunft auftreten.</span><span class="sxs-lookup"><span data-stu-id="8f89f-163">Async is short for *Asynchronous* - it means that the caller of the async method will resume its execution promptly, but the eventual completion of the async method's instance may happen at some later time in the future.</span></span> <span data-ttu-id="8f89f-164">Gemäß Konvention werden Async-Methoden mit dem Suffix "Async" benannt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-164">By convention async methods are named with the suffix "Async".</span></span>

```vb
Async Function TestAsync() As Task(Of String)
    Console.WriteLine("hello")
    Await Task.Delay(100)
    Return "world"
End Function

Dim t = TestAsync()         ' prints "hello"
Console.WriteLine(Await t)  ' prints "world"
```

<span data-ttu-id="8f89f-165">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="8f89f-165">__Note.__</span></span> <span data-ttu-id="8f89f-166">Async-Methoden werden *nicht* in einem Hintergrund Thread ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-166">Async methods are *not* run on a background thread.</span></span> <span data-ttu-id="8f89f-167">Stattdessen ermöglichen Sie es einer Methode, sich selbst durch den `Await`-Operator anzuhalten und zu planen, als Reaktion auf ein Ereignis fortgesetzt zu werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-167">Instead they allow a method to suspend itself through the `Await` operator, and schedule itself to be resumed in response to some event.</span></span>

<span data-ttu-id="8f89f-168">Wenn eine Async-Methode aufgerufen wird</span><span class="sxs-lookup"><span data-stu-id="8f89f-168">When an async method is invoked</span></span>

1. <span data-ttu-id="8f89f-169">Zuerst wird eine Instanz der Async-Methode erstellt, die für diesen Aufruf spezifisch ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-169">First an instance of the async method is created specific to that invocation.</span></span> <span data-ttu-id="8f89f-170">Diese Instanz enthält eine Kopie aller Parameter und lokalen Variablen der Methode.</span><span class="sxs-lookup"><span data-stu-id="8f89f-170">This instance includes a copy of all parameters and local variables of the method.</span></span>
2. <span data-ttu-id="8f89f-171">Anschließend werden alle zugehörigen Parameter mit den angegebenen Werten und allen lokalen Variablen für die Standardwerte ihrer Typen initialisiert.</span><span class="sxs-lookup"><span data-stu-id="8f89f-171">Then all of its parameters are initialized to the supplied values, and all of its local variables to the default values of their types.</span></span>
3. <span data-ttu-id="8f89f-172">Bei einer Async-Methode mit dem Rückgabetyp `Task(Of T)` für einige `T` wird auch eine implizite lokale Variable initialisiert, die als *Task Rückgabe Variable*bezeichnet wird, deren Typ `T` und deren ursprünglicher Wert der Standardwert `T` ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-172">In the case of an async method with return type `Task(Of T)` for some `T`, an implicit local variable is also initialized called the *task return variable*, whose type is `T` and whose initial value is the default of `T`.</span></span>
4. <span data-ttu-id="8f89f-173">Wenn die Async-Methode eine `Function` mit dem Rückgabetyp `Task` oder `Task(Of T)` für einige `T` ist, wird ein Objekt dieses Typs implizit erstellt, das dem aktuellen Aufruf zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-173">If the async method is a `Function` with return type `Task` or `Task(Of T)` for some `T`, then an object of that type implicitly created, associated with the current invocation.</span></span> <span data-ttu-id="8f89f-174">Dies wird als *Async-Objekt* bezeichnet und stellt die zukünftige Arbeit dar, die ausgeführt wird, indem die Instanz der Async-Methode ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-174">This is called an *async object* and represents the future work that will be done by executing the instance of the async method.</span></span> <span data-ttu-id="8f89f-175">Wenn das Steuerelement im Aufrufer dieser Async-Methoden Instanz fortgesetzt wird, empfängt der Aufrufer dieses Async-Objekt als Ergebnis des Aufrufens.</span><span class="sxs-lookup"><span data-stu-id="8f89f-175">When control resumes in the caller of this async method instance, the caller will receive this async object as the result of its invocation.</span></span>
5. <span data-ttu-id="8f89f-176">Der Kontrollpunkt der-Instanz wird dann bei der ersten Anweisung des asynchronen Methoden Texts festgelegt und beginnt sofort mit der Ausführung des Methoden Texts von dort (Abschnitts [Blöcke und Bezeichnungen](statements.md#blocks-and-labels)).</span><span class="sxs-lookup"><span data-stu-id="8f89f-176">The instance's control point is then set at the first statement of the async method body, and immediately starts to execute the method body from there (Section [Blocks and Labels](statements.md#blocks-and-labels)).</span></span>

<span data-ttu-id="8f89f-177">__Fort-und aktueller Aufrufer__</span><span class="sxs-lookup"><span data-stu-id="8f89f-177">__Resumption Delegate and Current Caller__</span></span>

<span data-ttu-id="8f89f-178">Wie im Abschnitt "section Erwartungs [Operator](expressions.md#await-operator)" ausführlich beschrieben, kann die Ausführung eines `Await`-Ausdrucks den Steuerungspunkt der Methoden Instanz aussetzen, sodass die Ablauf Steuerung an anderer Stelle weitergeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-178">As detailed in Section [Await Operator](expressions.md#await-operator), execution of an `Await` expression has the ability to suspend the method instance's control point leaving control flow to go elsewhere.</span></span> <span data-ttu-id="8f89f-179">Die Ablauf Steuerung kann später auf dem Steuerungspunkt derselben Instanz durch Aufrufen eines- *Wiederaufnahme*Delegaten fortgesetzt werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-179">Control flow can later resume at the same instance's control point through invocation of a *resumption delegate*.</span></span> <span data-ttu-id="8f89f-180">Beachten Sie, dass diese Unterbrechung durchgeführt wird, ohne dass die Async-Methode beendet wird, und führt nicht dazu, dass die Handler ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-180">Note that this suspension is done without exiting the async method, and does not cause finally handlers to execute.</span></span> <span data-ttu-id="8f89f-181">Auf die Methoden Instanz wird immer noch durch den Wiederaufnahme Delegaten und das Ergebnis `Task` oder `Task(Of T)` verwiesen (sofern vorhanden), und es wird keine Garbage Collection durchgeführt, solange ein Live Verweis auf den Delegaten oder das Ergebnis vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-181">The method instance is still referenced by both the resumption delegate and the `Task` or `Task(Of T)` result (if any), and will not be garbage collected so long as there exists a live reference to either delegate or result.</span></span>

<span data-ttu-id="8f89f-182">Es ist hilfreich, sich die-Anweisung `Dim x = Await WorkAsync()` ungefähr als syntaktische Kurzform für Folgendes vorzustellen:</span><span class="sxs-lookup"><span data-stu-id="8f89f-182">It is helpful to imagine the statement `Dim x = Await WorkAsync()` approximately as syntactic shorthand for the following:</span></span>

```vb
Dim temp = WorkAsync().GetAwaiter()
If Not temp.IsCompleted Then
       temp.OnCompleted(resumptionDelegate)
       Return [task]
       CONT:   ' invocation of 'resumptionDelegate' will resume here
End If
Dim x = temp.GetResult()
```

<span data-ttu-id="8f89f-183">Im folgenden Beispiel wird der *aktuelle* Aufrufer der-Methoden Instanz entweder als ursprünglicher Aufrufer oder als Aufrufer des Wiederaufnahme Delegaten definiert, je nachdem, welcher Wert aktueller ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-183">In the following, the *current caller* of the method instance is defined as either the original caller, or the caller of the resumption delegate, whichever is more recent.</span></span>

<span data-ttu-id="8f89f-184">Wenn die Ablauf Steuerung den asynchronen Methoden Text beendet, indem die `End Sub` oder `End Function` erreicht wird, die das Ende oder eine explizite `Return`-oder `Exit`-Anweisung oder eine nicht behandelte Ausnahme erreichen, wird der Kontrollpunkt der Instanz auf das Ende der Methode festgelegt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-184">When control flow exits the async method body -- through reaching the `End Sub` or `End Function` that mark its end, or through an explicit `Return` or `Exit` statement, or through an unhandled exception -- the instance's control point is set to the end of the method.</span></span> <span data-ttu-id="8f89f-185">Das Verhalten hängt dann vom Rückgabetyp der Async-Methode ab.</span><span class="sxs-lookup"><span data-stu-id="8f89f-185">Behavior then depends on the return type of the async method.</span></span>

* <span data-ttu-id="8f89f-186">Im Fall eines `Async Function` mit dem Rückgabetyp `Task`:</span><span class="sxs-lookup"><span data-stu-id="8f89f-186">In the case of an `Async Function` with return type `Task`:</span></span>
  1. <span data-ttu-id="8f89f-187">Wenn die Ablauf Steuerung durch eine nicht behandelte Ausnahme beendet wird, wird der Status des asynchronen Objekts auf `TaskStatus.Faulted` festgelegt, und die Eigenschaft `Exception.InnerException` wird auf die Ausnahme festgelegt (außer: bestimmte von der Implementierung definierte Ausnahmen wie `OperationCanceledException` ändern Sie in `TaskStatus.Canceled`).</span><span class="sxs-lookup"><span data-stu-id="8f89f-187">If control flow exits through an unhandled exception, then the async object's status is set to `TaskStatus.Faulted` and its `Exception.InnerException` property is set to the exception (except: certain implementation-defined exceptions such as `OperationCanceledException` change it to `TaskStatus.Canceled`).</span></span> <span data-ttu-id="8f89f-188">Die Ablauf Steuerung wird im aktuellen Aufrufer fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-188">Control flow resumes in the current caller.</span></span>
  2. <span data-ttu-id="8f89f-189">Andernfalls wird der Status des Async-Objekts auf `TaskStatus.Completed` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-189">Otherwise, the async object's status is set to `TaskStatus.Completed`.</span></span> <span data-ttu-id="8f89f-190">Die Ablauf Steuerung wird im aktuellen Aufrufer fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-190">Control flow resumes in the current caller.</span></span>

     <span data-ttu-id="8f89f-191">(__Hinweis:__</span><span class="sxs-lookup"><span data-stu-id="8f89f-191">(__Note.__</span></span> <span data-ttu-id="8f89f-192">Der ganze Punkt der Aufgabe und die Art und Weise, wie asynchrone Methoden interessant werden, besteht darin, dass bei der Fertigstellung einer Aufgabe für alle Methoden, die darauf warten, ihre Wiederverwendungs Delegaten ausgeführt werden, d. h., Sie werden die Blockierung aufgehoben.</span><span class="sxs-lookup"><span data-stu-id="8f89f-192">The whole point of Task, and what makes async methods interesting, is that when a task becomes Completed then any methods that were awaiting it will presently have their resumption delegates executed, i.e. they will become unblocked.)</span></span>

* <span data-ttu-id="8f89f-193">Im Fall eines `Async Function` mit dem Rückgabetyp `Task(Of T)` für einige `T`: das Verhalten ist wie oben angegeben, mit der Ausnahme, dass in nicht Ausnahmefällen die `Result`-Eigenschaft des Async-Objekts auch auf den endgültigen Wert der Task Rückgabe Variablen festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-193">In the case of an `Async Function` with return type `Task(Of T)` for some `T`: the behavior is as above, except that in non-exception cases the async object's `Result` property is also set to the final value of the task return variable.</span></span>

* <span data-ttu-id="8f89f-194">Im Fall eines `Async Sub`:</span><span class="sxs-lookup"><span data-stu-id="8f89f-194">In the case of an `Async Sub`:</span></span>
  1. <span data-ttu-id="8f89f-195">Wenn die Ablauf Steuerung durch eine nicht behandelte Ausnahme beendet wird, wird diese Ausnahme in Implementierungs spezifischer Weise an die Umgebung weitergegeben.</span><span class="sxs-lookup"><span data-stu-id="8f89f-195">If control flow exits through an unhandled exception, then that exception is propagated to the environment in some implementation-specific manner.</span></span> <span data-ttu-id="8f89f-196">Die Ablauf Steuerung wird im aktuellen Aufrufer fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-196">Control flow resumes in the current caller.</span></span>
  2. <span data-ttu-id="8f89f-197">Andernfalls wird die Ablauf Steuerung einfach im aktuellen Aufrufer fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-197">Otherwise, control flow simply resumes in the current caller.</span></span>

#### <a name="async-sub"></a><span data-ttu-id="8f89f-198">Async Sub</span><span class="sxs-lookup"><span data-stu-id="8f89f-198">Async Sub</span></span>

<span data-ttu-id="8f89f-199">Es gibt ein Microsoft-spezifisches Verhalten einer `Async Sub`.</span><span class="sxs-lookup"><span data-stu-id="8f89f-199">There is some Microsoft-specific behavior of an `Async Sub`.</span></span>

<span data-ttu-id="8f89f-200">Wenn `SynchronizationContext.Current` zu Beginn des aufzurufenden `Nothing` ist, werden alle nicht behandelten Ausnahmen von einem asynchronen Sub an den Thread Pool gesendet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-200">If `SynchronizationContext.Current` is `Nothing` at the start of the invocation, then any unhandled exceptions from an Async Sub will be posted to the Threadpool.</span></span>

<span data-ttu-id="8f89f-201">Wenn `SynchronizationContext.Current` zu Beginn des aufgerufenen nicht `Nothing` ist, wird `OperationStarted()` in diesem Kontext vor dem Anfang der Methode und `OperationCompleted()` nach dem Ende aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-201">If `SynchronizationContext.Current` is not `Nothing` at the start of the invocation, then `OperationStarted()` is invoked on that context before the start of the method and `OperationCompleted()` after the end.</span></span> <span data-ttu-id="8f89f-202">Außerdem werden alle nicht behandelten Ausnahmen bereitgestellt, damit Sie für den Synchronisierungs Kontext erneut ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-202">Additionally, any unhandled exceptions will be posted to be rethrown on the synchronization context.</span></span>

<span data-ttu-id="8f89f-203">Dies bedeutet, dass bei UI-Anwendungen für eine `Async Sub`, die im UI-Thread aufgerufen wird, alle Ausnahmen, die nicht behandelt werden können, den UI-Thread erneut bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-203">This means that in UI applications, for an `Async Sub` that is invoked on the UI thread, any exceptions it fails to handle will be reposted the UI thread.</span></span>

#### <a name="mutable-structures-in-async-and-iterator-methods"></a><span data-ttu-id="8f89f-204">Änderbare Strukturen in Async-und Iterator-Methoden</span><span class="sxs-lookup"><span data-stu-id="8f89f-204">Mutable structures in async and iterator methods</span></span>

<span data-ttu-id="8f89f-205">Änderbare Strukturen im Allgemeinen gelten als ungültige Praktiken und werden von Async-oder Iteratormethoden nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-205">Mutable structures in general are considered bad practice, and they are not supported by async or iterator methods.</span></span> <span data-ttu-id="8f89f-206">Insbesondere wird jeder Aufruf einer Async-oder Iterator-Methode in einer-Struktur implizit auf eine *Kopie* dieser Struktur angewendet, die zum Zeitpunkt des aufhebers kopiert wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-206">In particular, each invocation of an async or iterator method in a structure will implicitly operate on a *copy* of that structure that is copied at its moment of invocation.</span></span> <span data-ttu-id="8f89f-207">Wenn Sie also beispielsweise</span><span class="sxs-lookup"><span data-stu-id="8f89f-207">Thus, for example,</span></span>

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

### <a name="blocks-and-labels"></a><span data-ttu-id="8f89f-208">Blöcke und Bezeichnungen</span><span class="sxs-lookup"><span data-stu-id="8f89f-208">Blocks and Labels</span></span>

<span data-ttu-id="8f89f-209">Eine Gruppe ausführbarer Anweisungen wird als Anweisungsblock bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-209">A group of executable statements is called a statement block.</span></span> <span data-ttu-id="8f89f-210">Die Ausführung eines Anweisungsblocks beginnt mit der ersten Anweisung im-Block.</span><span class="sxs-lookup"><span data-stu-id="8f89f-210">Execution of a statement block begins with the first statement in the block.</span></span> <span data-ttu-id="8f89f-211">Nachdem eine Anweisung ausgeführt wurde, wird die nächste Anweisung in lexikalischer Reihenfolge ausgeführt, es sei denn, eine Anweisung überträgt an anderer Stelle die Ausführung, oder es tritt eine Ausnahme auf.</span><span class="sxs-lookup"><span data-stu-id="8f89f-211">Once a statement has been executed, the next statement in lexical order is executed, unless a statement transfers execution elsewhere or an exception occurs.</span></span>

<span data-ttu-id="8f89f-212">Innerhalb eines Anweisungsblocks ist die Division von Anweisungen in logischen Zeilen mit Ausnahme von Bezeichnungs Deklarations Anweisungen nicht signifikant.</span><span class="sxs-lookup"><span data-stu-id="8f89f-212">Within a statement block, the division of statements on logical lines is not significant with the exception of label declaration statements.</span></span> <span data-ttu-id="8f89f-213">Eine Bezeichnung ist ein Bezeichner, der eine bestimmte Position innerhalb des Anweisungsblocks identifiziert, die als Ziel einer Verzweigungs Anweisung (z. b. `GoTo`) verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="8f89f-213">A label is an identifier that identifies a particular position within the statement block that can be used as the target of a branch statement such as `GoTo`.</span></span>

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


<span data-ttu-id="8f89f-214">Bezeichnungs Deklarations Anweisungen müssen am Anfang einer logischen Zeile und Bezeichnungen entweder ein Bezeichner oder ein ganzzahliges Literalzeichen sein.</span><span class="sxs-lookup"><span data-stu-id="8f89f-214">Label declaration statements must appear at the beginning of a logical line and labels may be either an identifier or an integer literal.</span></span> <span data-ttu-id="8f89f-215">Da sowohl Bezeichnungs Deklarations Anweisungen als auch Aufruf Anweisungen aus einem einzelnen Bezeichner bestehen können, wird ein einzelner Bezeichner am Anfang einer lokalen Zeile immer als Bezeichnungs Deklarations Anweisung angesehen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-215">Because both label declaration statements and invocation statements can consist of a single identifier, a single identifier at the beginning of a local line is always considered a label declaration statement.</span></span> <span data-ttu-id="8f89f-216">Auf Bezeichnungs Deklarations Anweisungen muss immer ein Doppelpunkt folgen, auch wenn keine-Anweisungen in derselben logischen Zeile folgen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-216">Label declaration statements must always be followed by a colon, even if no statements follow on the same logical line.</span></span>

<span data-ttu-id="8f89f-217">Bezeichnungen verfügen über einen eigenen Deklarations Bereich und stören andere Bezeichner nicht.</span><span class="sxs-lookup"><span data-stu-id="8f89f-217">Labels have their own declaration space and do not interfere with other identifiers.</span></span> <span data-ttu-id="8f89f-218">Das folgende Beispiel ist gültig und verwendet die Name-Variable `x` sowohl als Parameter als auch als Bezeichnung.</span><span class="sxs-lookup"><span data-stu-id="8f89f-218">The following example is valid and uses the name variable `x` both as a parameter and as a label.</span></span>

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

<span data-ttu-id="8f89f-219">Der Gültigkeitsbereich einer Bezeichnung ist der Text der Methode, in der Sie enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-219">The scope of a label is the body of the method containing it.</span></span>

<span data-ttu-id="8f89f-220">Zur besseren Lesbarkeit werden Anweisungs Produktionen, die mehrere unter Anweisungen einbeziehen, als eine einzelne Produktion in dieser Spezifikation behandelt, auch wenn sich die untergeordneten Anweisungen jeweils in einer gekennzeichneten Zeile befinden können.</span><span class="sxs-lookup"><span data-stu-id="8f89f-220">For the sake of readability, statement productions that involve multiple substatements are treated as a single production in this specification, even though the substatements may each be by themselves on a labeled line.</span></span>


### <a name="local-variables-and-parameters"></a><span data-ttu-id="8f89f-221">Lokale Variablen und Parameter</span><span class="sxs-lookup"><span data-stu-id="8f89f-221">Local Variables and Parameters</span></span>

<span data-ttu-id="8f89f-222">In den vorstehenden Abschnitten wird erläutert, wie und wann Methoden Instanzen erstellt werden und wie Sie mit den Kopien der lokalen Variablen und Parameter einer Methode erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-222">The preceding sections detail how and when method instances are created, and with them the copies of a method's local variables and parameters.</span></span> <span data-ttu-id="8f89f-223">Außerdem wird jedes Mal, wenn der Text einer Schleife eingegeben wird, eine neue Kopie aus jeder in dieser Schleife deklarierten lokalen Variable erstellt, wie in Abschnitts [Schleifen Anweisungen](statements.md#loop-statements)beschrieben, und die-Methoden Instanz enthält jetzt diese Kopie der lokalen Variablen anstelle der vorherigen Kopie.</span><span class="sxs-lookup"><span data-stu-id="8f89f-223">In addition, each time the body of a loop is entered, a new copy is made of each local variable declared inside that loop as described in Section [Loop Statements](statements.md#loop-statements), and the method instance now contains this copy of its local variable rather than the previous copy.</span></span>

<span data-ttu-id="8f89f-224">Alle lokalen Variablen werden mit dem Standardwert ihres Typs initialisiert.</span><span class="sxs-lookup"><span data-stu-id="8f89f-224">All locals are initialized to their type's default value.</span></span> <span data-ttu-id="8f89f-225">Lokale Variablen und Parameter sind immer öffentlich zugänglich.</span><span class="sxs-lookup"><span data-stu-id="8f89f-225">Local variables and parameters are always publicly accessible.</span></span> <span data-ttu-id="8f89f-226">Es ist ein Fehler, in einer Textposition, die der Deklaration vorangestellt ist, auf eine lokale Variable zu verweisen, wie im folgenden Beispiel veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="8f89f-226">It is an error to refer to a local variable in a textual position that precedes its declaration, as the following example illustrates:</span></span>

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

<span data-ttu-id="8f89f-227">In der obigen Methode `F` verweist die erste Zuweisung an `i` nicht auf das im äußeren Gültigkeitsbereich deklarierte Feld.</span><span class="sxs-lookup"><span data-stu-id="8f89f-227">In the `F` method above, the first assignment to `i` specifically does not refer to the field declared in the outer scope.</span></span> <span data-ttu-id="8f89f-228">Stattdessen verweist Sie auf die lokale Variable und ist fehlerhaft, da Sie sich im textext vor der Deklaration der Variablen befindet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-228">Rather, it refers to the local variable, and it is in error because it textually precedes the declaration of the variable.</span></span> <span data-ttu-id="8f89f-229">In der `G`-Methode verweist eine nachfolgende Variablen Deklaration auf eine lokale Variable, die in einer früheren Variablen Deklaration innerhalb derselben lokalen Variablen Deklaration deklariert ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-229">In the `G` method, a subsequent variable declaration refers to a local variable declared in an earlier variable declaration within the same local variable declaration.</span></span>

<span data-ttu-id="8f89f-230">Jeder Block in einer Methode erstellt einen Deklarations Raum für lokale Variablen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-230">Each block in a method creates a declaration space for local variables.</span></span> <span data-ttu-id="8f89f-231">Namen werden in diesem Deklarations Raum durch lokale Variablen Deklarationen im Methoden Text und durch die Parameterliste der Methode eingeführt, die Namen in den Deklarations Bereich des äußersten Blocks einführt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-231">Names are introduced into this declaration space through local variable declarations in the method body and through the parameter list of the method, which introduces names into the outermost block's declaration space.</span></span> <span data-ttu-id="8f89f-232">-Blöcke lassen das shadodown von Namen durch Schachtelung nicht zu: Nachdem ein Name in einem-Block deklariert wurde, kann der Name nicht mehr in geschachtelten-Blöcken deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-232">Blocks do not allow shadowing of names through nesting: once a name has been declared in a block, the name may not be redeclared in any nested blocks.</span></span>

<span data-ttu-id="8f89f-233">Im folgenden Beispiel sind die Methoden `F` und `G` fehlerhaft, da der Name `i` im äußeren Block deklariert ist und nicht im Inneren Block erneut deklariert werden kann.</span><span class="sxs-lookup"><span data-stu-id="8f89f-233">Thus, in the following example, the `F` and `G` methods are in error because the name `i` is declared in the outer block and cannot be redeclared in the inner block.</span></span> <span data-ttu-id="8f89f-234">Allerdings sind die Methoden `H` und `I` gültig, da die beiden `i` in separaten nicht in-Blöcken deklarierten Blöcken deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-234">However, the `H` and `I` methods are valid because the two `i`'s are declared in separate non-nested blocks.</span></span>

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

<span data-ttu-id="8f89f-235">Wenn die Methode eine Funktion ist, wird eine spezielle lokale Variable implizit im Deklarations Bereich des Methoden Texts mit dem gleichen Namen deklariert wie die Methode, die den Rückgabewert der Funktion darstellt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-235">When the method is a function, a special local variable is implicitly declared in the method body's declaration space with the same name as the method representing the return value of the function.</span></span> <span data-ttu-id="8f89f-236">Die lokale Variable hat eine spezielle namens Auflösungs Semantik, wenn Sie in Ausdrücken verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-236">The local variable has special name resolution semantics when used in expressions.</span></span> <span data-ttu-id="8f89f-237">Wenn die lokale Variable in einem Kontext verwendet wird, der einen als Methoden Gruppe klassifizierten Ausdruck erwartet (z. b. ein Aufruf Ausdruck), wird der Name in die Funktion und nicht in die lokale Variable aufgelöst.</span><span class="sxs-lookup"><span data-stu-id="8f89f-237">If the local variable is used in a context that expects an expression classified as a method group, such as an invocation expression, then the name resolves to the function rather than to the local variable.</span></span> <span data-ttu-id="8f89f-238">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8f89f-238">For example:</span></span>

```vb
Function F(i As Integer) As Integer
    If i = 0 Then
        F = 1        ' Sets the return value.
    Else
        F = F(i - 1) ' Recursive call.
    End If
End Function
```

<span data-ttu-id="8f89f-239">Die Verwendung von Klammern kann mehrdeutige Situationen verursachen (z. b. "`F(1)`", wobei "`F`" eine Funktion ist, deren Rückgabetyp ein eindimensionales Array ist). in allen mehrdeutigen Situationen wird der Name in die Funktion und nicht in die lokale Variable aufgelöst.</span><span class="sxs-lookup"><span data-stu-id="8f89f-239">The use of parentheses can cause ambiguous situations (such as `F(1)`, where `F` is a function whose return type is a one-dimensional array); in all ambiguous situations, the name resolves to the function rather than the local variable.</span></span> <span data-ttu-id="8f89f-240">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8f89f-240">For example:</span></span>

```vb
Function F(i As Integer) As Integer()
    If i = 0 Then
        F = new Integer(2) { 1, 2, 3 }
    Else
        F = F(i - 1) ' Recursive call, not an index.
    End If
End Function
```

<span data-ttu-id="8f89f-241">Wenn die Ablauf Steuerung den Methoden Text verlässt, wird der Wert der lokalen Variablen an den Aufruf Ausdruck zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="8f89f-241">When control flow leaves the method body, the value of the local variable is passed back to the invocation expression.</span></span> <span data-ttu-id="8f89f-242">Wenn es sich bei der Methode um eine Unterroutine handelt, gibt es keine implizite lokale Variable, und die Steuerung kehrt einfach zum Aufruf Ausdruck zurück.</span><span class="sxs-lookup"><span data-stu-id="8f89f-242">If the method is a subroutine, there is no such implicit local variable, and control simply returns to the invocation expression.</span></span>

## <a name="local-declaration-statements"></a><span data-ttu-id="8f89f-243">Lokale Deklarations Anweisungen</span><span class="sxs-lookup"><span data-stu-id="8f89f-243">Local Declaration Statements</span></span>

<span data-ttu-id="8f89f-244">Eine lokale Deklarations Anweisung deklariert eine neue lokale Variable, eine lokale Konstante oder eine statische Variable.</span><span class="sxs-lookup"><span data-stu-id="8f89f-244">A local declaration statement declares a new local variable, local constant, or static variable.</span></span> <span data-ttu-id="8f89f-245">*Lokale Variablen* und *lokale Konstanten* entsprechen Instanzvariablen und Konstanten, die auf die-Methode bezogen sind und auf die gleiche Weise deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-245">*Local variables* and *local constants* are equivalent to instance variables and constants scoped to the method and are declared in the same way.</span></span> <span data-ttu-id="8f89f-246">*Statische Variablen* ähneln `Shared`-Variablen und werden mithilfe des `Static`-Modifizierers deklariert.</span><span class="sxs-lookup"><span data-stu-id="8f89f-246">*Static variables* are similar to `Shared` variables and are declared using the `Static` modifier.</span></span>

```antlr
LocalDeclarationStatement
    : LocalModifier VariableDeclarators StatementTerminator
    ;

LocalModifier
    : 'Static' | 'Dim' | 'Const'
    ;
```

<span data-ttu-id="8f89f-247">Statische Variablen sind lokale Variablen, deren Wert über Aufrufe der Methode hinweg beibehalten wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-247">Static variables are locals that retain their value across invocations of the method.</span></span> <span data-ttu-id="8f89f-248">Statische Variablen, die in nicht freigegebenen Methoden deklariert werden, sind pro Instanz: jede Instanz des Typs, der die Methode enthält, verfügt über eine eigene Kopie der statischen Variablen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-248">Static variables declared within non-shared methods are per instance: each instance of the type that contains the method has its own copy of the static variable.</span></span> <span data-ttu-id="8f89f-249">Statische Variablen, die in `Shared`-Methoden deklariert werden, sind pro Typ. Es ist nur eine Kopie der statischen Variablen für alle Instanzen vorhanden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-249">Static variables declared within `Shared` methods are per type; there is only one copy of the static variable for all instances.</span></span> <span data-ttu-id="8f89f-250">Während lokale Variablen bei jedem Eintrag in die-Methode auf den Standardwert ihres Typs initialisiert werden, werden statische Variablen nur mit dem Standardwert ihres Typs initialisiert, wenn der Typ oder die Typinstanz initialisiert wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-250">While local variables are initialized to their type's default value upon each entry into the method, static variables are only initialized to their type's default value when the type or type instance is initialized.</span></span> <span data-ttu-id="8f89f-251">Statische Variablen dürfen nicht in Strukturen oder generischen Methoden deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-251">Static variables may not be declared in structures or generic methods.</span></span>

<span data-ttu-id="8f89f-252">Lokale Variablen, lokale Konstanten und statische Variablen haben immer öffentliche Barrierefreiheit und können keine Zugriffsmodifizierer angeben.</span><span class="sxs-lookup"><span data-stu-id="8f89f-252">Local variables, local constants, and static variables always have public accessibility and may not specify accessibility modifiers.</span></span> <span data-ttu-id="8f89f-253">Wenn für eine lokale Deklarations Anweisung kein Typ angegeben ist, bestimmen die folgenden Schritte den Typ der lokalen Deklaration:</span><span class="sxs-lookup"><span data-stu-id="8f89f-253">If no type is specified on a local declaration statement, then the following steps determine the type of the local declaration:</span></span>

1. <span data-ttu-id="8f89f-254">Wenn die Deklaration ein Typzeichen aufweist, ist der Typ des Typzeichens der Typ der lokalen Deklaration.</span><span class="sxs-lookup"><span data-stu-id="8f89f-254">If the declaration has a type character, the type of the type character is the type of the local declaration.</span></span>

2. <span data-ttu-id="8f89f-255">Wenn die lokale Deklaration eine lokale Konstante ist, oder wenn die lokale Deklaration eine lokale Variable mit einem Initialisierer ist, und ein lokaler Variablen-Typrückschluss verwendet wird, wird der Typ der lokalen Deklaration vom Typ des Initialisierers abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-255">If the local declaration is a local constant, or if the local declaration is a local variable with an initializer and local variable type inference is being used, the type of the local declaration is inferred from the type of the initializer.</span></span> <span data-ttu-id="8f89f-256">Wenn sich der Initialisierer auf die lokale Deklaration bezieht, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="8f89f-256">If the initializer refers to the local declaration, a compile-time error occurs.</span></span> <span data-ttu-id="8f89f-257">(Für lokale Konstanten sind Initialisierer erforderlich.)</span><span class="sxs-lookup"><span data-stu-id="8f89f-257">(Local constants are required to have initializers.)</span></span>

3. <span data-ttu-id="8f89f-258">Wenn eine strikte Semantik nicht verwendet wird, ist der Typ der lokalen Deklarations Anweisung implizit `Object`.</span><span class="sxs-lookup"><span data-stu-id="8f89f-258">If strict semantics are not being used, the type of the local declaration statement is implicitly `Object`.</span></span>

4. <span data-ttu-id="8f89f-259">Andernfalls tritt ein Kompilierungsfehler auf.</span><span class="sxs-lookup"><span data-stu-id="8f89f-259">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="8f89f-260">Wenn für eine lokale Deklarations Anweisung, die eine Array Größe oder einen Arraytypmodifizierer aufweist, kein Typ angegeben ist, ist der Typ der lokalen Deklaration ein Array mit dem angegebenen Rang, und die vorherigen Schritte werden verwendet, um den Elementtyp des Arrays zu bestimmen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-260">If no type is specified on a local declaration statement that has an array size or array type modifier, then the type of the local declaration is an array with the specified rank and the previous steps are used to determine the element type of the array.</span></span> <span data-ttu-id="8f89f-261">Wenn der Typrückschluss für lokale Variablen verwendet wird, muss der Typ des Initialisierers ein Arraytyp mit derselben Array Form (d.h. Arraytypmodifizierer) als lokale Deklarations Anweisung sein.</span><span class="sxs-lookup"><span data-stu-id="8f89f-261">If local variable type inference is used, the type of the initializer must be an array type with the same array shape (i.e. array type modifiers) as the local declaration statement.</span></span> <span data-ttu-id="8f89f-262">Beachten Sie, dass es möglicherweise immer noch ein Arraytyp ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-262">Note that it is possible that the inferred element type may still be an array type.</span></span> <span data-ttu-id="8f89f-263">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8f89f-263">For example:</span></span>

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

<span data-ttu-id="8f89f-264">Wenn für eine lokale Deklarations Anweisung mit einem Typmodifizierer, der NULL-Werte zulässt, kein Typ angegeben ist, ist der Typ der lokalen Deklaration die auf NULL festleg Bare Version des abhergenten Typs oder der abhergelegte Typ selbst, wenn er bereits einen Werttyp aufweist, der NULL</span><span class="sxs-lookup"><span data-stu-id="8f89f-264">If no type is specified on a local declaration statement that has a nullable type modifier, then the type of the local declaration is the nullable version of the inferred type or the inferred type itself if it a nullable value type already.</span></span>  <span data-ttu-id="8f89f-265">Wenn es sich bei dem abhergestellten Typ nicht um einen Werttyp handelt, der NULL-Werte zulässt, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="8f89f-265">If the inferred type is not a value type that can be made nullable, a compile-time error occurs.</span></span> <span data-ttu-id="8f89f-266">Wenn sowohl ein Typmodifizierer, der NULL-Werte zulässt, als auch eine Array Größe oder ein Arraytypmodifizierer in einer lokalen Deklarations Anweisung ohne Typ platziert werden, wird der Typmodifizierer, der NULL-Werte zulässt, auf den Elementtyp des Arrays angewendet, und die vorherigen Schritte werden verwendet, um die ELEME zu bestimmen NT-Typ.</span><span class="sxs-lookup"><span data-stu-id="8f89f-266">If both a nullable type modifier and an array size or array type modifier are placed on a local declaration statement with no type, then the nullable type modifier is considered to apply to the element type of the array and the previous steps are used to determine the element type.</span></span>

<span data-ttu-id="8f89f-267">Variableninitialisierer für lokale Deklarations Anweisungen entsprechen Zuweisungs Anweisungen, die an der Textposition der Deklaration platziert werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-267">Variable initializers on local declaration statements are equivalent to assignment statements placed at the textual location of the declaration.</span></span> <span data-ttu-id="8f89f-268">Wenn die Ausführung über die lokale Deklarations Anweisung verzweigt, wird der Variableninitialisierer daher nicht ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-268">Thus, if execution branches over the local declaration statement, the variable initializer is not executed.</span></span> <span data-ttu-id="8f89f-269">Wenn die lokale Deklarations Anweisung mehrmals ausgeführt wird, wird der Variableninitialisierer gleich oft ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-269">If the local declaration statement is executed more than once, the variable initializer is executed an equal number of times.</span></span> <span data-ttu-id="8f89f-270">Statische Variablen führen den Initialisierer nur beim ersten Mal aus.</span><span class="sxs-lookup"><span data-stu-id="8f89f-270">Static variables only execute their initializer the first time.</span></span> <span data-ttu-id="8f89f-271">Wenn beim Initialisieren einer statischen Variablen eine Ausnahme auftritt, wird die statische Variable als initialisiert mit dem Standardwert des Typs der statischen Variablen betrachtet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-271">If an exception occurs while initializing a static variable, the static variable is considered initialized with the default value of the static variable's type.</span></span>

<span data-ttu-id="8f89f-272">Das folgende Beispiel zeigt die Verwendung von Initialisierern:</span><span class="sxs-lookup"><span data-stu-id="8f89f-272">The following example shows the use of initializers:</span></span>

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

<span data-ttu-id="8f89f-273">Dieses Programm druckt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="8f89f-273">This program prints:</span></span>

```console
Static variable x = 5
Static variable x = 6
Static variable x = 7
Local variable y = 8
Local variable y = 8
Local variable y = 8
```

<span data-ttu-id="8f89f-274">Initialisierer für statische lokale Variablen sind Thread sicher und vor Ausnahmen während der Initialisierung geschützt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-274">Initializers on static locals are thread-safe and protected against exceptions during initialization.</span></span> <span data-ttu-id="8f89f-275">Wenn bei einem statischen lokalen Initialisierer eine Ausnahme auftritt, weist das statische lokale den Standardwert auf und wird nicht initialisiert.</span><span class="sxs-lookup"><span data-stu-id="8f89f-275">If an exception occurs during a static local initializer, the static local will have its default value and not be initialized.</span></span> <span data-ttu-id="8f89f-276">Ein statischer lokaler Initialisierer</span><span class="sxs-lookup"><span data-stu-id="8f89f-276">A static local initializer</span></span>

```vb
Module Test
    Sub F()
        Static x As Integer = 5
    End Sub
End Module
```

<span data-ttu-id="8f89f-277">für die folgende Syntax:</span><span class="sxs-lookup"><span data-stu-id="8f89f-277">is equivalent to</span></span>

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

<span data-ttu-id="8f89f-278">Lokale Variablen, lokale Konstanten und statische Variablen werden auf den Anweisungsblock festgelegt, in dem Sie deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-278">Local variables, local constants, and static variables are scoped to the statement block in which they are declared.</span></span> <span data-ttu-id="8f89f-279">Statische Variablen sind ein besonderes Zeichen darin, dass ihre Namen nur einmal in der gesamten Methode verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="8f89f-279">Static variables are special in that their names may only be used once throughout the entire method.</span></span> <span data-ttu-id="8f89f-280">Beispielsweise ist es nicht zulässig, zwei statische Variablen Deklarationen mit demselben Namen anzugeben, auch wenn Sie sich in unterschiedlichen Blöcken befinden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-280">For example, it is not valid to specify two static variable declarations with the same name even if they are in different blocks.</span></span>


### <a name="implicit-local-declarations"></a><span data-ttu-id="8f89f-281">Implizite lokale Deklarationen</span><span class="sxs-lookup"><span data-stu-id="8f89f-281">Implicit Local Declarations</span></span>

<span data-ttu-id="8f89f-282">Zusätzlich zu den lokalen Deklarations Anweisungen können auch lokale Variablen implizit durch Verwendung von deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-282">In addition to local declaration statements, local variables can also be declared implicitly through use.</span></span> <span data-ttu-id="8f89f-283">Ein einfacher namens Ausdruck, der einen Namen verwendet, der nicht in etwas anderes aufgelöst wird, deklariert eine lokale Variable mit diesem Namen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-283">A simple name expression that uses a name that does not resolve to something else declares a local variable by that name.</span></span> <span data-ttu-id="8f89f-284">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8f89f-284">For example:</span></span>

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

<span data-ttu-id="8f89f-285">Die implizite lokale Deklaration findet nur in Ausdrucks Kontexten statt, die einen als Variable klassifizierten Ausdruck akzeptieren können.</span><span class="sxs-lookup"><span data-stu-id="8f89f-285">Implicit local declaration only occurs in expression contexts that can accept an expression classified as a variable.</span></span> <span data-ttu-id="8f89f-286">Eine Ausnahme von dieser Regel ist, dass eine lokale Variable möglicherweise nicht implizit deklariert wird, wenn Sie das Ziel eines Funktionsaufruf Ausdrucks, eines Indizierungs Ausdrucks oder eines Element Zugriffs Ausdrucks ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-286">The exception to this rule is that a local variable may not be implicitly declared when it is the target of a function invocation expression, indexing expression, or a member access expression.</span></span>

<span data-ttu-id="8f89f-287">Implizite lokale Variablen werden so behandelt, als wären Sie am Anfang der enthaltenden Methode deklariert.</span><span class="sxs-lookup"><span data-stu-id="8f89f-287">Implicit locals are treated as if they are declared at the beginning of the containing method.</span></span> <span data-ttu-id="8f89f-288">Folglich werden Sie immer auf den gesamten Methoden Text beschränkt, auch wenn Sie in einem Lambda-Ausdruck deklariert sind.</span><span class="sxs-lookup"><span data-stu-id="8f89f-288">Thus, they are always scoped to the entire method body, even if declared inside of a lambda expression.</span></span> <span data-ttu-id="8f89f-289">Beispielsweise folgender Code:</span><span class="sxs-lookup"><span data-stu-id="8f89f-289">For example, the following code:</span></span>

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

<span data-ttu-id="8f89f-290">druckt den Wert `10`.</span><span class="sxs-lookup"><span data-stu-id="8f89f-290">will print the value `10`.</span></span> <span data-ttu-id="8f89f-291">Implizite lokale Variablen werden als `Object` typisiert, wenn kein Typzeichen an den Variablennamen angefügt wurde. Andernfalls ist der Typ der Variablen der Typ des Typzeichens.</span><span class="sxs-lookup"><span data-stu-id="8f89f-291">Implicit locals are typed as `Object` if no type character was attached to the variable name; otherwise the type of the variable is the type of the type character.</span></span> <span data-ttu-id="8f89f-292">Der Typrückschluss für lokale Variablen wird nicht für implizite lokale Variablen verwendet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-292">Local variable type inference is not used for implicit locals.</span></span>

<span data-ttu-id="8f89f-293">Wenn eine explizite lokale Deklaration durch die Kompilierungs Umgebung oder `Option Explicit` angegeben wird, müssen alle lokalen Variablen explizit deklariert werden, und die implizite Variablen Deklaration ist nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="8f89f-293">If explicit local declaration is specified by the compilation environment or by `Option Explicit`, all local variables must be explicitly declared and implicit variable declaration is disallowed.</span></span>

## <a name="with-statement"></a><span data-ttu-id="8f89f-294">With-Anweisung</span><span class="sxs-lookup"><span data-stu-id="8f89f-294">With Statement</span></span>

<span data-ttu-id="8f89f-295">Eine `With`-Anweisung lässt mehrere Verweise auf die Member eines Ausdrucks zu, ohne den Ausdruck mehrmals anzugeben.</span><span class="sxs-lookup"><span data-stu-id="8f89f-295">A `With` statement allows multiple references to an expression's members without specifying the expression multiple times.</span></span>

```antlr
WithStatement
    : 'With' Expression StatementTerminator
      Block?
      'End' 'With' StatementTerminator
    ;
```

<span data-ttu-id="8f89f-296">Der Ausdruck muss als Wert klassifiziert werden und wird nach dem Einzug in den-Block einmal ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-296">The expression must be classified as a value and is evaluated once, upon entry into the block.</span></span> <span data-ttu-id="8f89f-297">Innerhalb des `With`-Anweisungsblocks wird ein Member-Zugriffs Ausdruck oder ein Wörterbuch Zugriffs Ausdruck, der mit einem Punkt oder einem Ausrufezeichen beginnt, ausgewertet, als wäre der `With`-Ausdruck vorangestellt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-297">Within the `With` statement block, a member access expression or dictionary access expression starting with a period or an exclamation point is evaluated as if the `With` expression preceded it.</span></span> <span data-ttu-id="8f89f-298">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8f89f-298">For example:</span></span>

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

<span data-ttu-id="8f89f-299">Die Verzweigung in einen `With`-Anweisungsblock von außerhalb des Blocks ist ungültig.</span><span class="sxs-lookup"><span data-stu-id="8f89f-299">It is invalid to branch into a `With` statement block from outside of the block.</span></span>


## <a name="synclock-statement"></a><span data-ttu-id="8f89f-300">SyncLock-Anweisung</span><span class="sxs-lookup"><span data-stu-id="8f89f-300">SyncLock Statement</span></span>

<span data-ttu-id="8f89f-301">Mit einer `SyncLock`-Anweisung können Anweisungen für einen Ausdruck synchronisiert werden, wodurch sichergestellt wird, dass die gleichen Anweisungen nicht gleichzeitig von mehreren Ausführungs Ausführungsthreads ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-301">A `SyncLock` statement allows statements to be synchronized on an expression, which ensures that multiple threads of execution do not execute the same statements at the same time.</span></span>

```antlr
SyncLockStatement
    : 'SyncLock' Expression StatementTerminator
      Block?
      'End' 'SyncLock' StatementTerminator
    ;
```

<span data-ttu-id="8f89f-302">Der Ausdruck muss als Wert klassifiziert werden und wird nach dem Einzug in den Block einmal ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-302">The expression must be classified as a value and is evaluated once, upon entry to the block.</span></span> <span data-ttu-id="8f89f-303">Wenn Sie den `SyncLock`-Block eingeben, wird die `Shared`-Methode `System.Threading.Monitor.Enter` für den angegebenen Ausdruck aufgerufen, der blockiert, bis der Ausführungs Thread eine exklusive Sperre für das vom Ausdruck zurückgegebene Objekt aufweist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-303">When entering the `SyncLock` block, the `Shared` method `System.Threading.Monitor.Enter` is called on the specified expression, which blocks until the thread of execution has an exclusive lock on the object returned by the expression.</span></span> <span data-ttu-id="8f89f-304">Der Typ des Ausdrucks in einer `SyncLock`-Anweisung muss ein Verweistyp sein.</span><span class="sxs-lookup"><span data-stu-id="8f89f-304">The type of the expression in a `SyncLock` statement must be a reference type.</span></span> <span data-ttu-id="8f89f-305">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8f89f-305">For example:</span></span>

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

<span data-ttu-id="8f89f-306">Im obigen Beispiel wird auf der spezifischen Instanz der-Klasse `Test` synchronisiert, um sicherzustellen, dass nicht mehr als ein Ausführungs Thread für eine bestimmte Instanz von der count-Variablen zu einem Zeitpunkt hinzugefügt oder von diesem subtrahiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="8f89f-306">The example above synchronizes on the specific instance of the class `Test` to ensure that no more than one thread of execution can add or subtract from the count variable at a time for a particular instance.</span></span>

<span data-ttu-id="8f89f-307">Der `SyncLock`-Block ist implizit in einer `Try`-Anweisung enthalten, deren `Finally`-Block die `Shared`-Methode `System.Threading.Monitor.Exit` für den Ausdruck aufruft.</span><span class="sxs-lookup"><span data-stu-id="8f89f-307">The `SyncLock` block is implicitly contained by a `Try` statement whose `Finally` block calls the `Shared` method `System.Threading.Monitor.Exit` on the expression.</span></span> <span data-ttu-id="8f89f-308">Dadurch wird sichergestellt, dass die Sperre freigegeben wird, auch wenn eine Ausnahme ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-308">This ensures the lock is freed even when an exception is thrown.</span></span> <span data-ttu-id="8f89f-309">Daher ist es ungültig, von außerhalb des Blocks in einen `SyncLock`-Block zu verzweigen, und ein `SyncLock`-Block wird als einzelne Anweisung für `Resume` und `Resume Next` behandelt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-309">As a result, it is invalid to branch into a `SyncLock` block from outside of the block, and a `SyncLock` block is treated as a single statement for the purposes of `Resume` and `Resume Next`.</span></span> <span data-ttu-id="8f89f-310">Das obige Beispiel entspricht dem folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="8f89f-310">The above example is equivalent to the following code:</span></span>

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


## <a name="event-statements"></a><span data-ttu-id="8f89f-311">Ereignis Anweisungen</span><span class="sxs-lookup"><span data-stu-id="8f89f-311">Event Statements</span></span>

<span data-ttu-id="8f89f-312">Die Anweisungen `RaiseEvent`, `AddHandler` und `RemoveHandler` lösen Ereignisse aus und behandeln Ereignisse dynamisch.</span><span class="sxs-lookup"><span data-stu-id="8f89f-312">The `RaiseEvent`, `AddHandler`, and `RemoveHandler` statements raise events and handle events dynamically.</span></span>

```antlr
EventStatement
    : RaiseEventStatement
    | AddHandlerStatement
    | RemoveHandlerStatement
    ;
```

### <a name="raiseevent-statement"></a><span data-ttu-id="8f89f-313">RaiseEvent-Anweisung</span><span class="sxs-lookup"><span data-stu-id="8f89f-313">RaiseEvent Statement</span></span>

<span data-ttu-id="8f89f-314">Eine `RaiseEvent`-Anweisung benachrichtigt Ereignishandler, dass ein bestimmtes Ereignis aufgetreten ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-314">A `RaiseEvent` statement notifies event handlers that a particular event has occurred.</span></span>

```antlr
RaiseEventStatement
    : 'RaiseEvent' IdentifierOrKeyword
      ( OpenParenthesis ArgumentList? CloseParenthesis )? StatementTerminator
    ;
```

<span data-ttu-id="8f89f-315">Der einfache namens Ausdruck in einer `RaiseEvent`-Anweisung wird als Element Suche auf `Me` interpretiert.</span><span class="sxs-lookup"><span data-stu-id="8f89f-315">The simple name expression in a `RaiseEvent` statement is interpreted as a member lookup on `Me`.</span></span> <span data-ttu-id="8f89f-316">Folglich wird `RaiseEvent x` so interpretiert, als ob es `RaiseEvent Me.x` wäre.</span><span class="sxs-lookup"><span data-stu-id="8f89f-316">Thus, `RaiseEvent x` is interpreted as if it were `RaiseEvent Me.x`.</span></span> <span data-ttu-id="8f89f-317">Das Ergebnis des Ausdrucks muss als Ereignis Zugriff für ein Ereignis klassifiziert werden, das in der Klasse selbst definiert ist. für Basis Typen definierte Ereignisse können nicht in einer `RaiseEvent`-Anweisung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-317">The result of the expression must be classified as an event access for an event defined in the class itself; events defined on base types cannot be used in a `RaiseEvent` statement.</span></span>

<span data-ttu-id="8f89f-318">Die `RaiseEvent`-Anweisung wird als Aufruf der `Invoke`-Methode des Delegaten des Ereignisses verarbeitet, wobei die angegebenen Parameter verwendet werden (sofern vorhanden).</span><span class="sxs-lookup"><span data-stu-id="8f89f-318">The `RaiseEvent` statement is processed as a call to the `Invoke` method of the event's delegate, using the supplied parameters, if any.</span></span> <span data-ttu-id="8f89f-319">Wenn der Wert des Delegaten `Nothing` ist, wird keine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="8f89f-319">If the delegate's value is `Nothing`, no exception is thrown.</span></span> <span data-ttu-id="8f89f-320">Wenn keine Argumente vorhanden sind, können die Klammern ausgelassen werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-320">If there are no arguments, the parentheses may be omitted.</span></span> <span data-ttu-id="8f89f-321">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8f89f-321">For example:</span></span>

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

<span data-ttu-id="8f89f-322">Die oben genannte Klasse `Raiser` ist äquivalent zu:</span><span class="sxs-lookup"><span data-stu-id="8f89f-322">The class `Raiser` above is equivalent to:</span></span>

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


### <a name="addhandler-and-removehandler-statements"></a><span data-ttu-id="8f89f-323">AddHandler-und RemoveHandler-Anweisungen</span><span class="sxs-lookup"><span data-stu-id="8f89f-323">AddHandler and RemoveHandler Statements</span></span>

<span data-ttu-id="8f89f-324">Obwohl die meisten Ereignishandler automatisch über `WithEvents`-Variablen verknüpft werden, ist es möglicherweise notwendig, Ereignishandler zur Laufzeit dynamisch hinzuzufügen und zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-324">Although most event handlers are automatically hooked up through `WithEvents` variables, it may be necessary to dynamically add and remove event handlers at run time.</span></span> <span data-ttu-id="8f89f-325">Dies wird von `AddHandler`-und `RemoveHandler`-Anweisung durchführen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-325">`AddHandler` and `RemoveHandler` statements do this.</span></span>

```antlr
AddHandlerStatement
    : 'AddHandler' Expression Comma Expression StatementTerminator
    ;

RemoveHandlerStatement
    : 'RemoveHandler' Expression Comma Expression StatementTerminator
    ;
```

<span data-ttu-id="8f89f-326">Jede Anweisung erfordert zwei Argumente: das erste Argument muss ein Ausdruck sein, der als Ereignis Zugriff klassifiziert ist, und das zweite Argument muss ein Ausdruck sein, der als Wert klassifiziert ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-326">Each statement takes two arguments: the first argument must be an expression that is classified as an event access and the second argument must be an expression that is classified as a value.</span></span> <span data-ttu-id="8f89f-327">Der Typ des zweiten Arguments muss der Delegattyp sein, der mit dem Ereignis Zugriff verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-327">The second argument's type must be the delegate type associated with the event access.</span></span> <span data-ttu-id="8f89f-328">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8f89f-328">For example:</span></span>

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

<span data-ttu-id="8f89f-329">Bei einem Ereignis `E,` ruft die-Anweisung die relevante `add_E`-oder `remove_E`-Methode für die-Instanz auf, um den Delegaten als Handler für das-Ereignis hinzuzufügen oder zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-329">Given an event `E,` the statement calls the relevant `add_E` or `remove_E` method on the instance to add or remove the delegate as a handler for the event.</span></span> <span data-ttu-id="8f89f-330">Folglich ist der obige Code äquivalent zu:</span><span class="sxs-lookup"><span data-stu-id="8f89f-330">Thus, the above code is equivalent to:</span></span>

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


## <a name="assignment-statements"></a><span data-ttu-id="8f89f-331">Zuweisungs Anweisungen</span><span class="sxs-lookup"><span data-stu-id="8f89f-331">Assignment Statements</span></span>

<span data-ttu-id="8f89f-332">Eine Zuweisungsanweisung weist den Wert eines Ausdrucks einer Variablen zu.</span><span class="sxs-lookup"><span data-stu-id="8f89f-332">An assignment statement assigns the value of an expression to a variable.</span></span> <span data-ttu-id="8f89f-333">Es gibt mehrere Arten der Zuweisung.</span><span class="sxs-lookup"><span data-stu-id="8f89f-333">There are several types of assignment.</span></span>

```antlr
AssignmentStatement
    : RegularAssignmentStatement
    | CompoundAssignmentStatement
    | MidAssignmentStatement
    ;
```

### <a name="regular-assignment-statements"></a><span data-ttu-id="8f89f-334">Reguläre Zuweisungs Anweisungen</span><span class="sxs-lookup"><span data-stu-id="8f89f-334">Regular Assignment Statements</span></span>

<span data-ttu-id="8f89f-335">Eine einfache Zuweisungsanweisung speichert das Ergebnis eines Ausdrucks in einer Variablen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-335">A simple assignment statement stores the result of an expression in a variable.</span></span>

```antlr
RegularAssignmentStatement
    : Expression Equals Expression StatementTerminator
    ;
```

<span data-ttu-id="8f89f-336">Der Ausdruck auf der linken Seite des Zuweisungs Operators muss als Variable oder Eigenschafts Zugriff klassifiziert werden, während der Ausdruck auf der rechten Seite des Zuweisungs Operators als Wert klassifiziert werden muss.</span><span class="sxs-lookup"><span data-stu-id="8f89f-336">The expression on the left side of the assignment operator must be classified as a variable or a property access, while the expression on the right side of the assignment operator must be classified as a value.</span></span> <span data-ttu-id="8f89f-337">Der Typ des Ausdrucks muss implizit in den Typ der Variablen oder des Eigenschafts Zugriffs konvertiert werden können.</span><span class="sxs-lookup"><span data-stu-id="8f89f-337">The type of the expression must be implicitly convertible to the type of the variable or property access.</span></span>

<span data-ttu-id="8f89f-338">Wenn die Variable, die in zugewiesen wird, ein Array Element eines Verweis Typs ist, wird eine Lauf Zeit Überprüfung durchgeführt, um sicherzustellen, dass der Ausdruck mit dem Array Elementtyp kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-338">If the variable being assigned into is an array element of a reference type, a run-time check will be performed to ensure that the expression is compatible with the array-element type.</span></span> <span data-ttu-id="8f89f-339">Im folgenden Beispiel bewirkt die letzte Zuweisung, dass eine `System.ArrayTypeMismatchException` ausgelöst wird, da eine Instanz von `ArrayList` nicht in einem Element eines `String`-Arrays gespeichert werden kann.</span><span class="sxs-lookup"><span data-stu-id="8f89f-339">In the following example, the last assignment causes a `System.ArrayTypeMismatchException` to be thrown, because an instance of `ArrayList` cannot be stored in an element of a `String` array.</span></span>

```vb
Dim sa(10) As String
Dim oa As Object() = sa
oa(0) = Nothing         ' This is allowed.
oa(1) = "Hello"         ' This is allowed.
oa(2) = New ArrayList() ' System.ArrayTypeMismatchException is thrown.
```

<span data-ttu-id="8f89f-340">Wenn der Ausdruck auf der linken Seite des Zuweisungs Operators als Variable klassifiziert wird, speichert die Zuweisungsanweisung den Wert in der Variablen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-340">If the expression on the left side of the assignment operator is classified as a variable, then the assignment statement stores the value in the variable.</span></span> <span data-ttu-id="8f89f-341">Wenn der Ausdruck als Eigenschaften Zugriff klassifiziert ist, wird der Eigenschafts Zugriff durch die Zuweisungsanweisung in einen Aufruf des `Set`-Accessors der Eigenschaft umgewandelt, wobei der Wert durch den value-Parameter ersetzt wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-341">If the expression is classified as a property access, then the assignment statement turns the property access into an invocation of the `Set` accessor of the property with the value substituted for the value parameter.</span></span> <span data-ttu-id="8f89f-342">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8f89f-342">For example:</span></span>

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

<span data-ttu-id="8f89f-343">Wenn das Ziel der Variablen oder des Eigenschaften Zugriffs als Werttyp typisiert ist, aber nicht als Variable klassifiziert ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="8f89f-343">If the target of the variable or property access is typed as a value type but not classified as a variable, a compile-time error occurs.</span></span> <span data-ttu-id="8f89f-344">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8f89f-344">For example:</span></span>

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

<span data-ttu-id="8f89f-345">Beachten Sie, dass die Semantik der Zuweisung vom Typ der Variablen oder Eigenschaft abhängt, der Sie zugewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-345">Note that the semantics of the assignment depend on the type of the variable or property to which it is being assigned.</span></span> <span data-ttu-id="8f89f-346">Wenn die Variable, der Sie zugewiesen wird, ein Werttyp ist, kopiert die Zuweisung den Wert des Ausdrucks in die Variable.</span><span class="sxs-lookup"><span data-stu-id="8f89f-346">If the variable to which it is being assigned is a value type, the assignment copies the value of the expression into the variable.</span></span> <span data-ttu-id="8f89f-347">Wenn die Variable, der Sie zugewiesen wird, ein Verweistyp ist, kopiert die Zuweisung den Verweis und nicht den Wert selbst in die Variable.</span><span class="sxs-lookup"><span data-stu-id="8f89f-347">If the variable to which it is being assigned is a reference type, the assignment copies the reference, not the value itself, into the variable.</span></span> <span data-ttu-id="8f89f-348">Wenn der Typ der Variablen `Object` ist, wird die Zuweisungs Semantik bestimmt, ob der Werttyp zur Laufzeit ein Werttyp oder ein Verweistyp ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-348">If the type of the variable is `Object`, the assignment semantics are determined by whether the value's type is a value type or a reference type at run time.</span></span>


<span data-ttu-id="8f89f-349">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="8f89f-349">__Note.__</span></span> <span data-ttu-id="8f89f-350">Bei systeminternen Typen wie `Integer` und `Date` sind Verweis-und Wert Zuweisungs Semantik identisch, da die Typen unveränderlich sind.</span><span class="sxs-lookup"><span data-stu-id="8f89f-350">For intrinsic types such as `Integer` and `Date`, reference and value assignment semantics are the same because the types are immutable.</span></span> <span data-ttu-id="8f89f-351">Folglich ist es für die Sprache frei, die Verweis Zuweisung für systeminterne schachtelte Typen als Optimierung zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-351">As a result, the language is free to use reference assignment on boxed intrinsic types as an optimization.</span></span> <span data-ttu-id="8f89f-352">Aus Sicht des Werts ist das Ergebnis identisch.</span><span class="sxs-lookup"><span data-stu-id="8f89f-352">From a value perspective, the result is the same.</span></span>

<span data-ttu-id="8f89f-353">Da das Gleichheitszeichen (`=`) sowohl für die Zuweisung als auch für Gleichheit verwendet wird, besteht eine Mehrdeutigkeit zwischen einer einfachen Zuweisung und einer Aufruf Anweisung in Situationen wie `x = y.ToString()`.</span><span class="sxs-lookup"><span data-stu-id="8f89f-353">Because the equals character (`=`) is used both for assignment and for equality, there is an ambiguity between a simple assignment and an invocation statement in situations such as `x = y.ToString()`.</span></span> <span data-ttu-id="8f89f-354">In all diesen Fällen hat die Zuweisungsanweisung Vorrang vor dem Gleichheits Operator.</span><span class="sxs-lookup"><span data-stu-id="8f89f-354">In all such cases, the assignment statement takes precedence over the equality operator.</span></span> <span data-ttu-id="8f89f-355">Dies bedeutet, dass der Beispiel Ausdruck als `x = (y.ToString())` und nicht als `(x = y).ToString()` interpretiert wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-355">This means that the example expression is interpreted as `x = (y.ToString())` rather than `(x = y).ToString()`.</span></span>


### <a name="compound-assignment-statements"></a><span data-ttu-id="8f89f-356">Verbund Zuweisungs Anweisungen</span><span class="sxs-lookup"><span data-stu-id="8f89f-356">Compound Assignment Statements</span></span>

<span data-ttu-id="8f89f-357">Eine *Verbund Zuweisungsanweisung* hat die Form `V op= E` (wobei `op` ein gültiger binärer Operator ist).</span><span class="sxs-lookup"><span data-stu-id="8f89f-357">A *compound assignment statement* takes the form `V op= E` (where `op` is a valid binary operator).</span></span>

```antlr
CompoundAssignmentStatement
    : Expression CompoundBinaryOperator LineTerminator? Expression StatementTerminator
    ;

CompoundBinaryOperator
    : '^' '=' | '*' '=' | '/' '=' | '\\' '=' | '+' '=' | '-' '='
    | '&' '=' | '<' '<' '=' | '>' '>' '='
    ;
```

<span data-ttu-id="8f89f-358">Der Ausdruck auf der linken Seite des Zuweisungs Operators muss als Variablen-oder Eigenschafts Zugriff klassifiziert werden, während der Ausdruck auf der rechten Seite des Zuweisungs Operators als Wert klassifiziert werden muss.</span><span class="sxs-lookup"><span data-stu-id="8f89f-358">The expression on the left side of the assignment operator must be classified as a variable or property access, while the expression on the right side of the assignment operator must be classified as a value.</span></span> <span data-ttu-id="8f89f-359">Die Verbund Zuweisungsanweisung entspricht der-Anweisung `V = V op E` mit dem Unterschied, dass die Variable auf der linken Seite des Verbund Zuweisungs Operators nur einmal ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-359">The compound assignment statement is equivalent to the statement `V = V op E` with the difference that the variable on the left side of the compound assignment operator is only evaluated once.</span></span> <span data-ttu-id="8f89f-360">Dieses Unterschied wird im folgenden Beispiel veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="8f89f-360">The following example demonstrates this difference:</span></span>

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

<span data-ttu-id="8f89f-361">Der Ausdruck "`a(GetIndex())`" wird für die einfache Zuweisung zweimal ausgewertet, aber nur einmal für die Verbund Zuweisung, daher druckt der Code Folgendes:</span><span class="sxs-lookup"><span data-stu-id="8f89f-361">The expression `a(GetIndex())` is evaluated twice for simple assignment but only once for compound assignment, so the code prints:</span></span>

```console
Simple assignment
Getting index
Getting index
Compound assignment
Getting index
```


### <a name="mid-assignment-statement"></a><span data-ttu-id="8f89f-362">Mid-Zuweisungsanweisung</span><span class="sxs-lookup"><span data-stu-id="8f89f-362">Mid Assignment Statement</span></span>

<span data-ttu-id="8f89f-363">Eine `Mid`-Zuweisungsanweisung weist eine Zeichenfolge einer anderen Zeichenfolge zu.</span><span class="sxs-lookup"><span data-stu-id="8f89f-363">A `Mid` assignment statement assigns a string into another string.</span></span> <span data-ttu-id="8f89f-364">Die linke Seite der Zuweisung hat dieselbe Syntax wie ein Aufrufs`Microsoft.VisualBasic.Strings.Mid`.</span><span class="sxs-lookup"><span data-stu-id="8f89f-364">The left side of the assignment has the same syntax as a call to the function `Microsoft.VisualBasic.Strings.Mid`.</span></span>

```antlr
MidAssignmentStatement
    : 'Mid' '$'? OpenParenthesis Expression Comma Expression
      ( Comma Expression )? CloseParenthesis Equals Expression StatementTerminator
    ;
```

<span data-ttu-id="8f89f-365">Das erste Argument ist das Ziel der Zuweisung und muss als Variable oder Eigenschafts Zugriff klassifiziert werden, dessen Typ implizit in und aus `String` konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="8f89f-365">The first argument is the target of the assignment and must be classified as a variable or a property access whose type is implicitly convertible to and from `String`.</span></span> <span data-ttu-id="8f89f-366">Der zweite Parameter ist die 1-basierte Startposition, die entspricht, wo die Zuweisung in der Ziel Zeichenfolge beginnen soll und als Wert klassifiziert werden muss, dessen Typ implizit in `Integer` konvertiert werden muss.</span><span class="sxs-lookup"><span data-stu-id="8f89f-366">The second parameter is the 1-based start position that corresponds to where the assignment should begin in the target string and must be classified as a value whose type must be implicitly convertible to `Integer`.</span></span> <span data-ttu-id="8f89f-367">Der optionale dritte Parameter ist die Anzahl der Zeichen aus dem rechtsseitigen Wert, der der Ziel Zeichenfolge zugewiesen werden soll. er muss als Wert klassifiziert werden, dessen Typ implizit in `Integer` konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="8f89f-367">The optional third parameter is the number of characters from the right-side value to assign into the target string and must be classified as a value whose type is implicitly convertible to `Integer`.</span></span> <span data-ttu-id="8f89f-368">Die Rechte Seite ist die Quell Zeichenfolge und muss als Wert klassifiziert werden, dessen Typ implizit in `String` konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="8f89f-368">The right side is the source string and must be classified as a value whose type is implicitly convertible to `String`.</span></span> <span data-ttu-id="8f89f-369">Die Rechte Seite wird ggf. auf den length-Parameter gekürzt und ersetzt die Zeichen in der Zeichenfolge auf der linken Seite, beginnend bei der Startposition.</span><span class="sxs-lookup"><span data-stu-id="8f89f-369">The right side is truncated to the length parameter, if specified, and replaces the characters in the left-side string, starting at the start position.</span></span> <span data-ttu-id="8f89f-370">Wenn die Zeichenfolge der rechten Seite weniger Zeichen als der dritte Parameter enthielt, werden nur die Zeichen aus der rechten Zeichenfolge kopiert.</span><span class="sxs-lookup"><span data-stu-id="8f89f-370">If the right side string contained fewer characters than the third parameter, only the characters from the right side string will be copied.</span></span>

<span data-ttu-id="8f89f-371">Im folgenden Beispiel wird `ab123fg` angezeigt:</span><span class="sxs-lookup"><span data-stu-id="8f89f-371">The following example displays `ab123fg`:</span></span>

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

<span data-ttu-id="8f89f-372">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="8f89f-372">__Note.__</span></span> <span data-ttu-id="8f89f-373">`Mid` ist kein reserviertes Wort.</span><span class="sxs-lookup"><span data-stu-id="8f89f-373">`Mid` is not a reserved word.</span></span>


## <a name="invocation-statements"></a><span data-ttu-id="8f89f-374">Aufruf Anweisungen</span><span class="sxs-lookup"><span data-stu-id="8f89f-374">Invocation Statements</span></span>

<span data-ttu-id="8f89f-375">Eine Aufruf Anweisung ruft eine Methode auf, der das optionale Schlüsselwort `Call` vorangestellt ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-375">An invocation statement invokes a method preceded by the optional keyword `Call`.</span></span> <span data-ttu-id="8f89f-376">Die Aufruf Anweisung wird auf die gleiche Weise wie der Funktionsaufruf Ausdruck verarbeitet, mit einigen Unterschieden, die unten angegeben sind.</span><span class="sxs-lookup"><span data-stu-id="8f89f-376">The invocation statement is processed in the same way as the function invocation expression, with some differences noted below.</span></span> <span data-ttu-id="8f89f-377">Der Aufruf Ausdruck muss als Wert oder als void klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-377">The invocation expression must be classified as a value or void.</span></span> <span data-ttu-id="8f89f-378">Alle Werte, die sich aus der Auswertung des Aufruf Ausdrucks ergeben, werden verworfen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-378">Any value resulting from the evaluation of the invocation expression is discarded.</span></span>

<span data-ttu-id="8f89f-379">Wenn das Schlüsselwort `Call` weggelassen wird, muss der Aufruf Ausdruck mit einem Bezeichner oder Schlüsselwort oder mit `.` innerhalb eines `With`-Blocks beginnen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-379">If the `Call` keyword is omitted, then the invocation expression must start with an identifier or keyword, or with `.` inside a `With` block.</span></span> <span data-ttu-id="8f89f-380">So ist beispielsweise "`Call 1.ToString()`" eine gültige Anweisung, "`1.ToString()`" jedoch nicht.</span><span class="sxs-lookup"><span data-stu-id="8f89f-380">Thus, for instance, "`Call 1.ToString()`" is a valid statement but "`1.ToString()`" is not.</span></span> <span data-ttu-id="8f89f-381">(Beachten Sie, dass Aufruf Ausdrücke in einem Ausdrucks Kontext auch nicht mit einem Bezeichner beginnen müssen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-381">(Note that in an expression context, invocation expressions also need not start with an identifier.</span></span> <span data-ttu-id="8f89f-382">Beispielsweise ist "`Dim x = 1.ToString()`" eine gültige-Anweisung).</span><span class="sxs-lookup"><span data-stu-id="8f89f-382">For example, "`Dim x = 1.ToString()`" is a valid statement).</span></span>

<span data-ttu-id="8f89f-383">Es gibt einen weiteren Unterschied zwischen den Aufruf Anweisungen und Aufruf Ausdrücken: Wenn eine Aufruf Anweisung eine Argumentliste enthält, wird dies immer als Argumentliste des Auflistungs Ausdrucks verwendet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-383">There is another difference between the invocation statements and invocation expressions: if an invocation statement includes an argument list, then this is always taken as the argument list of the invocation.</span></span> <span data-ttu-id="8f89f-384">Das folgende Beispiel veranschaulicht den Unterschied:</span><span class="sxs-lookup"><span data-stu-id="8f89f-384">The following example illustrates the difference:</span></span>

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

## <a name="conditional-statements"></a><span data-ttu-id="8f89f-385">Bedingte Anweisungen</span><span class="sxs-lookup"><span data-stu-id="8f89f-385">Conditional Statements</span></span>

<span data-ttu-id="8f89f-386">Bedingte Anweisungen ermöglichen die bedingte Ausführung von Anweisungen auf der Grundlage von Ausdrücken, die zur Laufzeit ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-386">Conditional statements allow conditional execution of statements based on expressions evaluated at run time.</span></span>

```antlr
ConditionalStatement
    : IfStatement
    | SelectStatement
    ;
```

### <a name="ifthenelse-statements"></a><span data-ttu-id="8f89f-387">Wenn... Dann... Else-Anweisungen</span><span class="sxs-lookup"><span data-stu-id="8f89f-387">If...Then...Else Statements</span></span>

<span data-ttu-id="8f89f-388">Eine `If...Then...Else`-Anweisung ist die grundlegende bedingte Anweisung.</span><span class="sxs-lookup"><span data-stu-id="8f89f-388">An `If...Then...Else` statement is the basic conditional statement.</span></span>

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

<span data-ttu-id="8f89f-389">Jeder Ausdruck in einer `If...Then...Else`-Anweisung muss ein boolescher Ausdruck sein, und zwar gemäß [booleschen Abschnitts Ausdrücken](expressions.md#boolean-expressions).</span><span class="sxs-lookup"><span data-stu-id="8f89f-389">Each expression in an `If...Then...Else` statement must be a Boolean expression, as per Section [Boolean Expressions](expressions.md#boolean-expressions).</span></span> <span data-ttu-id="8f89f-390">(Hinweis: Dies erfordert nicht, dass der Ausdruck einen booleschen Typ hat).</span><span class="sxs-lookup"><span data-stu-id="8f89f-390">(Note: this does not require the expression to have Boolean type).</span></span> <span data-ttu-id="8f89f-391">Wenn der Ausdruck in der `If`-Anweisung True ist, werden die Anweisungen, die vom `If`-Block eingeschlossen werden, ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-391">If the expression in the `If` statement is true, the statements enclosed by the `If` block are executed.</span></span> <span data-ttu-id="8f89f-392">Wenn der Ausdruck false ist, wird jeder der `ElseIf`-Ausdrücke ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-392">If the expression is false, each of the `ElseIf` expressions is evaluated.</span></span> <span data-ttu-id="8f89f-393">Wenn einer der `ElseIf`-Ausdrücke als true ausgewertet wird, wird der entsprechende-Block ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-393">If one of the `ElseIf` expressions evaluates to true, the corresponding block is executed.</span></span> <span data-ttu-id="8f89f-394">Wenn kein Ausdruck zu true ausgewertet wird und ein `Else`-Block vorhanden ist, wird der `Else`-Block ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-394">If no expression evaluates to true and there is an `Else` block, the `Else` block is executed.</span></span> <span data-ttu-id="8f89f-395">Nachdem die Ausführung eines-Blocks abgeschlossen ist, wird die Ausführung an das Ende der `If...Then...Else`-Anweisung weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-395">Once a block finishes executing, execution passes to the end of the `If...Then...Else` statement.</span></span>

<span data-ttu-id="8f89f-396">Die Zeilen Version der `If`-Anweisung verfügt über einen einzelnen Satz von Anweisungen, die ausgeführt werden sollen, wenn der `If`-Ausdruck `True` ist, und ein optionaler Satz von Anweisungen, die ausgeführt werden sollen, wenn der Ausdruck `False` ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-396">The line version of the `If` statement has a single set of statements to be executed if the `If` expression is `True` and an optional set of statements to be executed if the expression is `False`.</span></span> <span data-ttu-id="8f89f-397">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8f89f-397">For example:</span></span>

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

<span data-ttu-id="8f89f-398">Die Zeilen Version der if-Anweisung bindet weniger eng als ":", und der `Else` bindet an die lexikalisch nächstliegende `If`, die von der Syntax zugelassen wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-398">The line version of the If statement binds less tightly than ":", and its `Else` binds to the lexically nearest preceding `If` that is allowed by the syntax.</span></span> <span data-ttu-id="8f89f-399">Die folgenden zwei Versionen sind z. b. äquivalent:</span><span class="sxs-lookup"><span data-stu-id="8f89f-399">For example, the following two versions are equivalent:</span></span>

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

<span data-ttu-id="8f89f-400">Alle Anweisungen außer Bezeichnungs Deklarations Anweisungen sind innerhalb einer Line `If`-Anweisung zulässig, einschließlich Block Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-400">All statements other than label declaration statements are allowed inside a line `If` statement, including block statements.</span></span> <span data-ttu-id="8f89f-401">Allerdings dürfen Sie lineterminators nicht als "Status-Terminatoren" verwenden, außer in mehrzeiligen Lambda Ausdrücken.</span><span class="sxs-lookup"><span data-stu-id="8f89f-401">However, they may not use LineTerminators as StatementTerminators except inside multi-line lambda expressions.</span></span> <span data-ttu-id="8f89f-402">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8f89f-402">For example:</span></span>

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

### <a name="select-case-statements"></a><span data-ttu-id="8f89f-403">Select Case-Anweisungen</span><span class="sxs-lookup"><span data-stu-id="8f89f-403">Select Case Statements</span></span>

<span data-ttu-id="8f89f-404">Eine `Select Case`-Anweisung führt Anweisungen auf der Grundlage des Werts eines Ausdrucks aus.</span><span class="sxs-lookup"><span data-stu-id="8f89f-404">A `Select Case` statement executes statements based on the value of an expression.</span></span>

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

<span data-ttu-id="8f89f-405">Der Ausdruck muss als Wert klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-405">The expression must be classified as a value.</span></span> <span data-ttu-id="8f89f-406">Wenn eine `Select Case`-Anweisung ausgeführt wird, wird der `Select`-Ausdruck zuerst ausgewertet, und die `Case`-Anweisungen werden dann in der Reihenfolge der Text Deklaration ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-406">When a `Select Case` statement is executed, the `Select` expression is evaluated first, and the `Case` statements are then evaluated in order of textual declaration.</span></span> <span data-ttu-id="8f89f-407">Die erste `Case`-Anweisung, die als `True` ausgewertet wird, wird der-Block ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-407">The first `Case` statement that evaluates to `True` has its block executed.</span></span> <span data-ttu-id="8f89f-408">Wenn keine `Case`-Anweisung als `True` ausgewertet wird und eine `Case Else`-Anweisung vorhanden ist, wird dieser Block ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-408">If no `Case` statement evaluates to `True` and there is a `Case Else` statement, that block is executed.</span></span> <span data-ttu-id="8f89f-409">Nachdem die Ausführung eines Blocks abgeschlossen ist, wird die Ausführung an das Ende der `Select`-Anweisung weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-409">Once a block has finished executing, execution passes to the end of the `Select` statement.</span></span>

<span data-ttu-id="8f89f-410">Das Ausführen eines `Case`-Blocks ist nicht zulässig, um zum nächsten switchabschnitt zu wechseln.</span><span class="sxs-lookup"><span data-stu-id="8f89f-410">Execution of a `Case` block is not permitted to "fall through" to the next switch section.</span></span> <span data-ttu-id="8f89f-411">Dadurch wird eine häufige Klasse von Fehlern verhindert, die in anderen Sprachen auftreten, wenn eine `Case`-abschließende Anweisung versehentlich ausgelassen wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-411">This prevents a common class of bugs that occur in other languages when a `Case` terminating statement is accidentally omitted.</span></span> <span data-ttu-id="8f89f-412">Dieses Verhalten wird im folgenden Beispiel veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="8f89f-412">The following example illustrates this behavior:</span></span>

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

<span data-ttu-id="8f89f-413">Der Code druckt:</span><span class="sxs-lookup"><span data-stu-id="8f89f-413">The code prints:</span></span>

```console
x = 10
```

<span data-ttu-id="8f89f-414">Obwohl `Case 10` und `Case 20 - 10` für denselben Wert auswählen, wird `Case 10` ausgeführt, da es sich vor `Case 20 - 10` textueller handelt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-414">Although `Case 10` and `Case 20 - 10` select for the same value, `Case 10` is executed because it precedes `Case 20 - 10` textually.</span></span> <span data-ttu-id="8f89f-415">Wenn die nächste `Case` erreicht wird, wird die Ausführung nach der `Select`-Anweisung fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-415">When the next `Case` is reached, execution continues after the `Select` statement.</span></span>

<span data-ttu-id="8f89f-416">Eine `Case`-Klausel kann zwei Formen annehmen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-416">A `Case` clause may take two forms.</span></span> <span data-ttu-id="8f89f-417">Ein Formular ist ein optionales `Is`-Schlüsselwort, ein Vergleichs Operator und ein Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="8f89f-417">One form is an optional `Is` keyword, a comparison operator, and an expression.</span></span> <span data-ttu-id="8f89f-418">Der Ausdruck wird in den Typ des `Select`-Ausdrucks konvertiert. Wenn der Ausdruck nicht implizit in den Typ des `Select`-Ausdrucks konvertiert werden kann, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="8f89f-418">The expression is converted to the type of the `Select` expression; if the expression is not implicitly convertible to the type of the `Select` expression, a compile-time error occurs.</span></span> <span data-ttu-id="8f89f-419">Wenn der `Select`-Ausdruck *E*ist, der Vergleichs Operator *op*ist und der `Case`-Ausdruck *E1*ist, wird der Fall als *E op E1*ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-419">If the `Select` expression is *E*, the comparison operator is *Op*, and the `Case` expression is *E1*, the case is evaluated as *E OP E1*.</span></span> <span data-ttu-id="8f89f-420">Der Operator muss für die Typen der beiden Ausdrücke gültig sein. Andernfalls tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="8f89f-420">The operator must be valid for the types of the two expressions; otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="8f89f-421">Das andere Formular ist ein Ausdruck, auf den optional das Schlüsselwort `To` und ein zweiter Ausdruck folgt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-421">The other form is an expression optionally followed by the keyword `To` and a second expression.</span></span> <span data-ttu-id="8f89f-422">Beide Ausdrücke werden in den Typ des `Select`-Ausdrucks konvertiert. Wenn einer der Ausdrücke nicht implizit in den Typ des `Select`-Ausdrucks konvertiert werden kann, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="8f89f-422">Both expressions are converted to the type of the `Select` expression; if either expression is not implicitly convertible to the type of the `Select` expression, a compile-time error occurs.</span></span> <span data-ttu-id="8f89f-423">Wenn der `Select`-Ausdruck `E` ist, der erste `Case`-Ausdruck `E1` und der zweite `Case`-Ausdruck `E2` ist, wird der `Case` entweder als `E = E1` ausgewertet (wenn kein `E2` angegeben ist) oder `(E >= E1) And (E <= E2)`.</span><span class="sxs-lookup"><span data-stu-id="8f89f-423">If the `Select` expression is `E`, the first `Case` expression is `E1`, and the second `Case` expression is `E2`, the `Case` is evaluated either as `E = E1` (if no `E2` is specified) or `(E >= E1) And (E <= E2)`.</span></span> <span data-ttu-id="8f89f-424">Die Operatoren müssen für die Typen der beiden Ausdrücke gültig sein. Andernfalls tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="8f89f-424">The operators must be valid for the types of the two expressions; otherwise a compile-time error occurs.</span></span>


## <a name="loop-statements"></a><span data-ttu-id="8f89f-425">Schleifen Anweisungen</span><span class="sxs-lookup"><span data-stu-id="8f89f-425">Loop Statements</span></span>

<span data-ttu-id="8f89f-426">Schleifen Anweisungen ermöglichen die wiederholte Ausführung der-Anweisungen im Textkörper.</span><span class="sxs-lookup"><span data-stu-id="8f89f-426">Loop statements allow repeated execution of the statements in their body.</span></span>

```antlr
LoopStatement
    : WhileStatement
    | DoLoopStatement
    | ForStatement
    | ForEachStatement
    ;
```

<span data-ttu-id="8f89f-427">Jedes Mal, wenn ein Schleifen Text eingegeben wird, wird eine neue Kopie von allen lokalen Variablen, die in diesem Text deklariert sind, erstellt, die mit den vorherigen Werten der Variablen initialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-427">Each time a loop body is entered, a fresh copy is made of all local variables declared in that body, initialized to the previous values of the variables.</span></span> <span data-ttu-id="8f89f-428">Jeder Verweis auf eine Variable im Schleifen Text verwendet die zuletzt hergestellte Kopie.</span><span class="sxs-lookup"><span data-stu-id="8f89f-428">Any reference to a variable within the loop body will use the most recently made copy.</span></span> <span data-ttu-id="8f89f-429">Der folgende Code zeigt ein Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8f89f-429">This code shows an example:</span></span>

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

<span data-ttu-id="8f89f-430">Der Code erzeugt die Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="8f89f-430">The code produces the output:</span></span>

```console
31    32    33
```

<span data-ttu-id="8f89f-431">Wenn der Schleifen Text ausgeführt wird, wird die aktuelle Kopie der Variablen verwendet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-431">When the loop body is executed, it uses whichever copy of the variable is current.</span></span> <span data-ttu-id="8f89f-432">Beispielsweise verweist die-Anweisung `Dim y = x` auf die neueste Kopie von `y` und die ursprüngliche Kopie von `x`.</span><span class="sxs-lookup"><span data-stu-id="8f89f-432">For example, the statement  `Dim y = x` refers to the latest copy of `y` and the original copy of `x`.</span></span> <span data-ttu-id="8f89f-433">Wenn ein Lambda-Ausdruck erstellt wird, wird er unabhängig davon, welche Kopie einer Variablen zum Zeitpunkt der Erstellung aktuell war, gespeichert.</span><span class="sxs-lookup"><span data-stu-id="8f89f-433">And when a lambda is created, it remembers whichever copy of a variable was current at the time it was created.</span></span> <span data-ttu-id="8f89f-434">Daher verwendet jedes Lambda die gleiche freigegebene Kopie von `x`, aber eine andere Kopie von `y`.</span><span class="sxs-lookup"><span data-stu-id="8f89f-434">Therefore, each lambda uses the same shared copy of `x`, but a different copy of `y`.</span></span> <span data-ttu-id="8f89f-435">Am Ende des Programms wird beim Ausführen der Lambdas eine freigegebene Kopie von `x`, auf die Sie alle verweisen, jetzt am endgültigen Wert 3 angezeigt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-435">At the end of the program, when it executes the lambdas, that shared copy of `x` that they all refer to is now at its final value 3.</span></span>

<span data-ttu-id="8f89f-436">Beachten Sie Folgendes: Wenn keine Lambdas-oder LINQ-Ausdrücke vorhanden sind, ist es unmöglich zu wissen, dass eine neue Kopie für den Schleifen Eintrag erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-436">Note that if there are no lambdas or LINQ expressions, then it's impossible to know that a fresh copy is made on loop entry.</span></span> <span data-ttu-id="8f89f-437">In diesem Fall vermeiden Compileroptimierungen das Erstellen von Kopien.</span><span class="sxs-lookup"><span data-stu-id="8f89f-437">Indeed, compiler optimizations will avoid making copies in this case.</span></span> <span data-ttu-id="8f89f-438">Beachten Sie auch, dass es nicht zulässig ist,-0 in eine Schleife zu @no__t, die Lambdas-oder LINQ-Ausdrücke enthält.</span><span class="sxs-lookup"><span data-stu-id="8f89f-438">Note too that it's illegal to `GoTo` into a loop that contains lambdas or LINQ expressions.</span></span>


### <a name="whileend-while-and-doloop-statements"></a><span data-ttu-id="8f89f-439">While... Ende und Do... Schleifen Anweisungen</span><span class="sxs-lookup"><span data-stu-id="8f89f-439">While...End While and Do...Loop Statements</span></span>

<span data-ttu-id="8f89f-440">Eine `While`-oder `Do`-Schleifen Anweisung, die auf einem booleschen Ausdruck basiert.</span><span class="sxs-lookup"><span data-stu-id="8f89f-440">A `While` or `Do` loop statement loops based on a Boolean expression.</span></span>

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

<span data-ttu-id="8f89f-441">Eine `While`-Schleifen Anweisung wird so lange Schleifen Schleifen, wie der boolesche Ausdruck zu true ausgewertet wird. eine `Do`-Schleifen Anweisung kann eine komplexere Bedingung enthalten.</span><span class="sxs-lookup"><span data-stu-id="8f89f-441">A `While` loop statement loops as long as the Boolean expression evaluates to true; a `Do` loop statement may contain a more complex condition.</span></span> <span data-ttu-id="8f89f-442">Ein Ausdruck kann nach dem `Do`-Schlüsselwort oder nach dem Schlüsselwort `Loop` platziert werden, aber nicht nach beiden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-442">An expression may be placed after the `Do` keyword or after the `Loop` keyword, but not after both.</span></span> <span data-ttu-id="8f89f-443">Der boolesche Ausdruck wird als [boolesche Ausdrücke](expressions.md#boolean-expressions)pro Abschnitt ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-443">The Boolean expression is evaluated as per Section [Boolean Expressions](expressions.md#boolean-expressions).</span></span> <span data-ttu-id="8f89f-444">(Hinweis: Dies erfordert nicht, dass der Ausdruck einen booleschen Typ hat).</span><span class="sxs-lookup"><span data-stu-id="8f89f-444">(Note: this does not require the expression to have Boolean type).</span></span> <span data-ttu-id="8f89f-445">Es ist auch zulässig, überhaupt keinen Ausdruck anzugeben; in diesem Fall wird die Schleife nie beendet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-445">It is also valid to specify no expression at all; in that case, the loop will never exit.</span></span> <span data-ttu-id="8f89f-446">Wenn der Ausdruck nach `Do` platziert wird, wird er ausgewertet, bevor der Schleifen Block für jede Iterationen ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-446">If the expression is placed after `Do`, it will be evaluated before the loop block is executed on each iteration.</span></span> <span data-ttu-id="8f89f-447">Wenn der Ausdruck nach `Loop` platziert wird, wird er nach der Ausführung des Schleifen Blocks bei jeder Iterationen ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-447">If the expression is placed after `Loop`, it will be evaluated after the loop block has executed on each iteration.</span></span> <span data-ttu-id="8f89f-448">Wenn Sie den Ausdruck nach `Loop` platzieren, wird daher eine weitere Schleife generiert, als die Platzierung nach `Do`.</span><span class="sxs-lookup"><span data-stu-id="8f89f-448">Placing the expression after `Loop` will therefore generate one more loop than placement after `Do`.</span></span> <span data-ttu-id="8f89f-449">Das folgende Beispiel veranschaulicht dieses Verhalten:</span><span class="sxs-lookup"><span data-stu-id="8f89f-449">The following example demonstrates this behavior:</span></span>

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

<span data-ttu-id="8f89f-450">Der Code erzeugt die Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="8f89f-450">The code produces the output:</span></span>

```console
Second Loop
```

<span data-ttu-id="8f89f-451">Im Fall der ersten Schleife wird die Bedingung ausgewertet, bevor die Schleife ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-451">In the case of the first loop, the condition is evaluated before the loop executes.</span></span> <span data-ttu-id="8f89f-452">Im Fall der zweiten Schleife wird die Bedingung nach Ausführung der Schleife ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-452">In the case of the second loop, the condition is executed after the loop executes.</span></span> <span data-ttu-id="8f89f-453">Der bedingte Ausdruck muss entweder ein `While`-Schlüsselwort oder ein `Until`-Schlüsselwort als Präfix aufweisen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-453">The conditional expression must be prefixed with either a `While` keyword or an `Until` keyword.</span></span> <span data-ttu-id="8f89f-454">Der erste unterbricht die Schleife, wenn die Bedingung als false ausgewertet wird, letzteres, wenn die Bedingung als true ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-454">The former breaks the loop if the condition evaluates to false, the latter when the condition evaluates to true.</span></span>

<span data-ttu-id="8f89f-455">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="8f89f-455">__Note.__</span></span> <span data-ttu-id="8f89f-456">`Until` ist kein reserviertes Wort.</span><span class="sxs-lookup"><span data-stu-id="8f89f-456">`Until` is not a reserved word.</span></span>


### <a name="fornext-statements"></a><span data-ttu-id="8f89f-457">Für... Nächste Anweisungen</span><span class="sxs-lookup"><span data-stu-id="8f89f-457">For...Next Statements</span></span>

<span data-ttu-id="8f89f-458">Eine `For...Next`-Anweisung, die auf einem Satz von Begrenzungen basiert.</span><span class="sxs-lookup"><span data-stu-id="8f89f-458">A `For...Next` statement loops based on a set of bounds.</span></span> <span data-ttu-id="8f89f-459">Eine `For`-Anweisung gibt eine Schleifen Steuerungs Variable, einen unteren gebundenen Ausdruck, einen oberen gebundenen Ausdruck und einen optionalen Schrittwert Ausdruck an.</span><span class="sxs-lookup"><span data-stu-id="8f89f-459">A `For` statement specifies a loop control variable, a lower bound expression, an upper bound expression, and an optional step value expression.</span></span> <span data-ttu-id="8f89f-460">Die Schleifen Steuerungs Variable wird entweder durch einen Bezeichner angegeben, gefolgt von einer optionalen `As`-Klausel oder einem Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="8f89f-460">The loop control variable is specified either through an identifier followed by an optional `As` clause or an expression.</span></span>

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

<span data-ttu-id="8f89f-461">Gemäß den folgenden Regeln verweist die Schleifen Steuerungs Variable entweder auf eine neue lokale Variable, die für diese `For...Next`-Anweisung spezifisch ist, oder auf eine bereits vorhandene Variable oder auf einen Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="8f89f-461">As per the following rules, the loop control variable refers either to a new local variable specific to this `For...Next` statement, or to a pre-existing variable, or to an expression.</span></span>

* <span data-ttu-id="8f89f-462">Wenn die Schleifen Steuerungs Variable ein Bezeichner mit einer `As`-Klausel ist, definiert der Bezeichner eine neue lokale Variable des in der `As`-Klausel angegebenen Typs, die auf die gesamte `For`-Schleife beschränkt ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-462">If the loop control variable is an identifier with an `As` clause, the identifier defines a new local variable of the type specified in the `As` clause, scoped to the entire `For` loop.</span></span>

* <span data-ttu-id="8f89f-463">Wenn die Schleifen Steuerungs Variable ein Bezeichner ohne eine `As`-Klausel ist, wird der Bezeichner zuerst mithilfe der Regeln für einfache Namensauflösung aufgelöst (siehe Abschnitt [einfache namens Ausdrücke](expressions.md#simple-name-expressions)), ausgenommen, dass dieses Vorkommen des Bezeichners nicht in und von selbst bewirkt, dass eine implizite lokale Variable erstellt wird (Abschnitt [implizite lokale Deklarationen](statements.md#implicit-local-declarations)).</span><span class="sxs-lookup"><span data-stu-id="8f89f-463">If the loop control variable is an identifier without an `As` clause, then the identifier is first resolved using the simple name resolution rules (see Section [Simple Name Expressions](expressions.md#simple-name-expressions)), excepting that this occurrence of the identifier would not in and of itself cause an implicit local variable to be created (Section [Implicit Local Declarations](statements.md#implicit-local-declarations)).</span></span>

  * <span data-ttu-id="8f89f-464">Wenn diese Auflösung erfolgreich ist und das Ergebnis als Variable klassifiziert wird, ist die Schleifen Steuerungs Variable diese bereits vorhandene Variable.</span><span class="sxs-lookup"><span data-stu-id="8f89f-464">If this resolution succeeds and the result is classified as a variable, then the loop control variable is that pre-existing variable.</span></span>

  * <span data-ttu-id="8f89f-465">Wenn die Auflösung fehlschlägt oder die Auflösung erfolgreich ist und das Ergebnis als Typ klassifiziert ist, dann gilt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="8f89f-465">If resolution fails, or if resolution succeeds and the result is classified as a type, then:</span></span>
    * <span data-ttu-id="8f89f-466">Wenn der Typrückschluss für lokale Variablen verwendet wird, definiert der Bezeichner eine neue lokale Variable, deren Typ aus den gebundenen und Schritt Ausdrücken abgeleitet wird, die auf die gesamte `For`-Schleife beschränkt sind.</span><span class="sxs-lookup"><span data-stu-id="8f89f-466">if local variable type inference is being used, the identifier defines a new local variable whose type is inferred from the bound and step expressions, scoped to the entire `For` loop;</span></span>
    * <span data-ttu-id="8f89f-467">Wenn der Typrückschluss für lokale Variablen nicht verwendet wird, aber die implizite lokale Deklaration ist, wird eine implizite lokale Variable erstellt, deren Gültigkeitsbereich die gesamte Methode (Abschnitt [implizite lokale Deklarationen](statements.md#implicit-local-declarations)) ist, und die Schleifen Steuerungs Variable verweist auf dieses bereits vorhandene Variable;</span><span class="sxs-lookup"><span data-stu-id="8f89f-467">if local variable type inference is not being used but implicit local declaration is, then an implicit local variable is created whose scope is the entire method (Section [Implicit Local Declarations](statements.md#implicit-local-declarations)), and the loop control variable refers to this pre-existing variable;</span></span>
    * <span data-ttu-id="8f89f-468">Wenn weder eine lokale Variable-Typrückschluss noch implizite lokale Deklarationen verwendet werden, ist dies ein Fehler.</span><span class="sxs-lookup"><span data-stu-id="8f89f-468">if neither local variable type inference nor implicit local declarations are used, it is an error.</span></span>

  * <span data-ttu-id="8f89f-469">Wenn die Auflösung mit etwas, das entweder als Typ oder als Variable klassifiziert ist, erfolgreich ist, handelt es sich um einen Fehler.</span><span class="sxs-lookup"><span data-stu-id="8f89f-469">If resolution succeeds with something classified as neither a type nor a variable, it is an error.</span></span>

* <span data-ttu-id="8f89f-470">Wenn die Schleifen Steuerungs Variable ein Ausdruck ist, muss der Ausdruck als Variable klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-470">If the loop control variable is an expression, the expression must be classified as a variable.</span></span>

<span data-ttu-id="8f89f-471">Eine Schleifen Steuerungs Variable kann nicht von einer anderen einschließenden `For...Next`-Anweisung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-471">A loop control variable cannot be used by another enclosing `For...Next` statement.</span></span> <span data-ttu-id="8f89f-472">Der Typ der Schleifen Steuerungsvariablen einer `For`-Anweisung bestimmt den Typ der Iterationen und muss einer der folgenden sein:</span><span class="sxs-lookup"><span data-stu-id="8f89f-472">The type of the loop control variable of a `For` statement determines the type of the iteration and must be one of:</span></span>

* <span data-ttu-id="8f89f-473">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`</span><span class="sxs-lookup"><span data-stu-id="8f89f-473">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`</span></span>
* <span data-ttu-id="8f89f-474">Ein enumerierter Typ</span><span class="sxs-lookup"><span data-stu-id="8f89f-474">An enumerated type</span></span>
* `Object`
* <span data-ttu-id="8f89f-475">Ein Typ `T` mit den folgenden Operatoren, wobei `B` ein Typ ist, der in einem booleschen Ausdruck verwendet werden kann:</span><span class="sxs-lookup"><span data-stu-id="8f89f-475">A type `T` that has the following operators, where `B` is a type that can be used in a Boolean expression:</span></span>

```vb
Public Shared Operator >= (op1 As T, op2 As T) As B
Public Shared Operator <= (op1 As T, op2 As T) As B
Public Shared Operator - (op1 As T, op2 As T) As T
Public Shared Operator + (op1 As T, op2 As T) As T
```

<span data-ttu-id="8f89f-476">Die gebundenen und Schritt Ausdrücke müssen implizit in den Typ der Schleifen Steuerungsvariablen konvertiert werden und müssen als Werte klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-476">The bound and step expressions must be implicitly convertible to the type of the loop control variable and must be classified as values.</span></span> <span data-ttu-id="8f89f-477">Zum Zeitpunkt der Kompilierung wird der Typ der Schleifen Steuerungsvariablen abgeleitet, indem der breiteste Typ zwischen den Ausdrucks Typen für untere Grenze, obere Grenze und Schritt ausgewählt wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-477">At compile time, the type of the loop control variable is inferred by choosing the widest type among the lower bound, upper bound, and step expression types.</span></span> <span data-ttu-id="8f89f-478">Wenn es keine erweiternde Konvertierung zwischen zwei Typen gibt, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="8f89f-478">If there is no widening conversion between two of the types, then a compile-time error occurs.</span></span>

<span data-ttu-id="8f89f-479">Wenn der Typ der Schleifen Steuerungsvariablen zur Laufzeit `Object` ist, wird der iterationstyp zum Zeitpunkt der Kompilierung mit zwei Ausnahmen abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-479">At run time, if the type of the loop control variable is `Object`, then the type of the iteration is inferred the same as at compile time, with two exceptions.</span></span> <span data-ttu-id="8f89f-480">Erstens: Wenn die gebundenen und Schritt Ausdrücke alle ganzzahligen Typen sind, aber keinen größten Typ aufweisen, wird der breiteste Typ abgeleitet, der alle drei Typen umfasst.</span><span class="sxs-lookup"><span data-stu-id="8f89f-480">First, if the bound and step expressions are all of integral types but have no widest type, then the widest type that encompasses all three types will be inferred.</span></span> <span data-ttu-id="8f89f-481">Wenn der Typ der Schleifen Steuerungsvariablen als `String` abgeleitet wird, wird stattdessen `Double` abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-481">And second, if the type of the loop control variable is inferred to be `String`, `Double` will be inferred instead.</span></span> <span data-ttu-id="8f89f-482">Wenn zur Laufzeit kein Schleifen Steuerungstyp bestimmt werden kann oder wenn einer der Ausdrücke nicht in den Schleifen Steuerungstyp konvertiert werden kann, tritt ein `System.InvalidCastException` auf.</span><span class="sxs-lookup"><span data-stu-id="8f89f-482">If, at run time, no loop control type can be determined or if any of the expressions cannot be converted to the loop control type, a `System.InvalidCastException` will occur.</span></span> <span data-ttu-id="8f89f-483">Sobald ein Schleifen Steuerungstyp am Anfang der Schleife ausgewählt wurde, wird während der gesamten Iterationen derselbe Typ verwendet, unabhängig von Änderungen, die an dem Wert in der Schleifen Steuerungsvariablen vorgenommen werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-483">Once a loop control type has been chosen at the beginning of the loop, the same type will be used throughout the iteration, regardless of changes made to the value in the loop control variable.</span></span>

<span data-ttu-id="8f89f-484">Eine `For`-Anweisung muss durch eine entsprechende `Next`-Anweisung geschlossen werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-484">A `For` statement must be closed by a matching `Next` statement.</span></span> <span data-ttu-id="8f89f-485">Eine `Next`-Anweisung ohne Variable entspricht der innersten geöffneten `For`-Anweisung, während eine `Next`-Anweisung mit einer oder mehreren Schleifen Steuerungsvariablen von links nach rechts mit den `For`-Schleifen übereinstimmt, die mit den einzelnen Variablen übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-485">A `Next` statement without a variable matches the innermost open `For` statement, while a `Next` statement with one or more loop control variables will, from left to right, match the `For` loops that match each variable.</span></span> <span data-ttu-id="8f89f-486">Wenn eine Variable mit einer `For`-Schleife übereinstimmt, die zu diesem Zeitpunkt nicht die am meisten gebräuchliche Schleife ist, ergibt sich ein Kompilierzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="8f89f-486">If a variable matches a `For` loop that is not the most nested loop at that point, a compile-time error results.</span></span>

<span data-ttu-id="8f89f-487">Am Anfang der Schleife werden die drei Ausdrücke in der Text Reihenfolge ausgewertet, und der untere gebundene Ausdruck wird der Schleifen Steuerungsvariablen zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-487">At the beginning of the loop, the three expressions are evaluated in textual order and the lower bound expression is assigned to the loop control variable.</span></span> <span data-ttu-id="8f89f-488">Wenn der Schrittwert weggelassen wird, ist er implizit das Literale `1`, das in den Typ der Schleifen Steuerungsvariablen konvertiert wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-488">If the step value is omitted, it is implicitly the literal `1`, converted to the type of the loop control variable.</span></span> <span data-ttu-id="8f89f-489">Die drei Ausdrücke werden nur am Anfang der Schleife ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-489">The three expressions are only ever evaluated at the beginning of the loop.</span></span>

<span data-ttu-id="8f89f-490">Am Anfang jeder Schleife wird die Steuerelement Variable verglichen, um festzustellen, ob Sie größer als der Endpunkt ist, wenn der Schritt Ausdruck positiv ist, oder kleiner als der Endpunkt, wenn der Schritt Ausdruck negativ ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-490">At the beginning of each loop, the control variable is compared to see if it is greater than the end point if the step expression is positive, or less than the end point if the step expression is negative.</span></span> <span data-ttu-id="8f89f-491">Wenn dies der Fall ist, wird die `For`-Schleife beendet. Andernfalls wird der Schleifen Block ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-491">If it is, the `For` loop terminates; otherwise the loop block executes.</span></span> <span data-ttu-id="8f89f-492">Wenn die Schleifen Steuerungs Variable kein primitiver Typ ist, wird der Vergleichs Operator bestimmt, ob der Ausdruck `step >= step - step` true oder false ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-492">If the loop control variable is not a primitive type, the comparison operator is determined by whether the expression `step >= step - step` is true or false.</span></span> <span data-ttu-id="8f89f-493">Bei der `Next`-Anweisung wird der Schrittwert der Steuerelement Variablen hinzugefügt, und die Ausführung wird an den Anfang der Schleife zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="8f89f-493">At the `Next` statement, the step value is added to the control variable and execution returns to the top of the loop.</span></span>

<span data-ttu-id="8f89f-494">Beachten Sie, dass bei jeder Iterations Schleife des Schleifen Blocks *keine* neue Kopie der Schleifen Steuerungs Variable erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-494">Note that a new copy of the loop control variable is *not* created on each iteration of the loop block.</span></span> <span data-ttu-id="8f89f-495">In dieser Hinsicht unterscheidet sich die `For`-Anweisung von `For Each` (Abschnitt [für jeden... Next-Anweisungen](statements.md#for-eachnext-statements)).</span><span class="sxs-lookup"><span data-stu-id="8f89f-495">In this respect, the `For` statement differs from `For Each` (Section [For Each...Next Statements](statements.md#for-eachnext-statements)).</span></span>

<span data-ttu-id="8f89f-496">Es ist nicht zulässig, von außerhalb der Schleife in eine `For`-Schleife zu verzweigen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-496">It is not valid to branch into a `For` loop from outside the loop.</span></span>


### <a name="for-eachnext-statements"></a><span data-ttu-id="8f89f-497">For each... Nächste Anweisungen</span><span class="sxs-lookup"><span data-stu-id="8f89f-497">For Each...Next Statements</span></span>

<span data-ttu-id="8f89f-498">Eine `For Each...Next`-Anweisung, die auf den Elementen in einem Ausdruck basiert.</span><span class="sxs-lookup"><span data-stu-id="8f89f-498">A `For Each...Next` statement loops based on the elements in an expression.</span></span> <span data-ttu-id="8f89f-499">Eine `For Each`-Anweisung gibt eine Schleifen Steuerungs Variable und einen Enumeratorausdruck an.</span><span class="sxs-lookup"><span data-stu-id="8f89f-499">A `For Each` statement specifies a loop control variable and an enumerator expression.</span></span> <span data-ttu-id="8f89f-500">Die Schleifen Steuerungs Variable wird entweder durch einen Bezeichner angegeben, gefolgt von einer optionalen `As`-Klausel oder einem Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="8f89f-500">The loop control variable is specified either through an identifier followed by an optional `As` clause or an expression.</span></span>

```antlr
ForEachStatement
    : 'For' 'Each' LoopControlVariable 'In' LineTerminator? Expression StatementTerminator
      Block?
      ( 'Next' NextExpressionList? StatementTerminator )?
    ;
```

<span data-ttu-id="8f89f-501">Befolgen Sie dieselben Regeln wie `For...Next`-Anweisungen (Abschnitt [für... Next-Anweisungen](statements.md#fornext-statements)), bezieht sich die Schleifen Steuerungs Variable auf eine neue lokale Variable, die für diese spezifisch ist... Next-Anweisung, oder zu einer bereits vorhandenen Variablen oder zu einem Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="8f89f-501">Following the same rules as `For...Next` statements (Section [For...Next Statements](statements.md#fornext-statements)), the loop control variable refers either to a new local variable specific to this For Each...Next statement, or to a pre-existing variable, or to an expression.</span></span>

<span data-ttu-id="8f89f-502">Der Enumeratorausdruck muss als Wert klassifiziert werden, und sein Typ muss ein Sammlungstyp oder `Object` sein.</span><span class="sxs-lookup"><span data-stu-id="8f89f-502">The enumerator expression must be classified as a value and its type must be a collection type or `Object`.</span></span> <span data-ttu-id="8f89f-503">Wenn der Typ des Enumeratorausdrucks `Object` ist, wird die gesamte Verarbeitung bis zur Laufzeit verzögert.</span><span class="sxs-lookup"><span data-stu-id="8f89f-503">If the type of the enumerator expression is `Object`, then all processing is deferred until run-time.</span></span> <span data-ttu-id="8f89f-504">Andernfalls muss eine Konvertierung vom Elementtyp der Auflistung bis zum Typ der Schleifen Steuerungsvariablen vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="8f89f-504">Otherwise, a conversion must exist from the element type of the collection to the type of the loop control variable.</span></span>

<span data-ttu-id="8f89f-505">Die Schleifen Steuerungs Variable kann nicht von einer anderen einschließenden `For Each`-Anweisung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-505">The loop control variable cannot be used by another enclosing `For Each` statement.</span></span> <span data-ttu-id="8f89f-506">Eine `For Each`-Anweisung muss durch eine entsprechende `Next`-Anweisung geschlossen werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-506">A `For Each` statement must be closed by a matching `Next` statement.</span></span> <span data-ttu-id="8f89f-507">Eine `Next`-Anweisung ohne eine Schleifen Steuerungs Variable entspricht der innersten geöffneten `For Each`.</span><span class="sxs-lookup"><span data-stu-id="8f89f-507">A `Next` statement without a loop control variable matches the innermost open `For Each`.</span></span> <span data-ttu-id="8f89f-508">Eine `Next`-Anweisung mit einer oder mehreren Schleifen Steuerungsvariablen findet von links nach rechts mit den `For Each`-Schleifen überein, die dieselbe Schleifen Steuerungs Variable aufweisen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-508">A `Next` statement with one or more loop control variables will, from left to right, match the `For Each` loops that have the same loop control variable.</span></span> <span data-ttu-id="8f89f-509">Wenn eine Variable mit einer `For Each`-Schleife übereinstimmt, die zu diesem Zeitpunkt nicht die am meisten gebräuchliche Schleife ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="8f89f-509">If a variable matches a `For Each` loop that is not the most nested loop at that point, a compile-time error occurs.</span></span>

<span data-ttu-id="8f89f-510">Ein Typ `C` als *Auflistungstyp* bezeichnet wird, wenn einer der folgenden Typen ist:</span><span class="sxs-lookup"><span data-stu-id="8f89f-510">A type `C` is said to be a *collection type* if one of:</span></span>

* <span data-ttu-id="8f89f-511">Folgendes gilt für Folgendes:</span><span class="sxs-lookup"><span data-stu-id="8f89f-511">All of the following are true:</span></span>
  * <span data-ttu-id="8f89f-512">`C` enthält eine barrierefreie Instanz, eine freigegebene Methode oder eine Erweiterungsmethode mit der Signatur `GetEnumerator()`, die einen Typ `E` zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-512">`C` contains an accessible instance, shared or extension method with the signature `GetEnumerator()` that returns a type `E`.</span></span>
  * <span data-ttu-id="8f89f-513">`E` enthält eine barrierefreie Instanz, eine freigegebene Methode oder eine Erweiterungsmethode mit der Signatur `MoveNext()` und dem Rückgabetyp `Boolean`.</span><span class="sxs-lookup"><span data-stu-id="8f89f-513">`E` contains an accessible instance, shared or extension method with the signature `MoveNext()` and the return type `Boolean`.</span></span>
  * <span data-ttu-id="8f89f-514">`E` enthält eine barrierefreie Instanz oder eine freigegebene Eigenschaft mit dem Namen `Current`, die über einen Getter verfügt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-514">`E` contains an accessible instance or shared property named `Current` that has a getter.</span></span> <span data-ttu-id="8f89f-515">Der Typ dieser Eigenschaft ist der Elementtyp des Auflistungs Typs.</span><span class="sxs-lookup"><span data-stu-id="8f89f-515">The type of this property is the element type of the collection type.</span></span>

* <span data-ttu-id="8f89f-516">Es implementiert die-Schnittstelle `System.Collections.Generic.IEnumerable(Of T)`. in diesem Fall wird der Elementtyp der Auflistung als `T` betrachtet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-516">It implements the interface `System.Collections.Generic.IEnumerable(Of T)`, in which case the element type of the collection is considered to be `T`.</span></span>

* <span data-ttu-id="8f89f-517">Es implementiert die-Schnittstelle `System.Collections.IEnumerable`. in diesem Fall wird der Elementtyp der Auflistung als `Object` betrachtet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-517">It implements the interface `System.Collections.IEnumerable`, in which case the element type of the collection is considered to be `Object`.</span></span>

<span data-ttu-id="8f89f-518">Im folgenden finden Sie ein Beispiel für eine Klasse, die aufgelistet werden kann:</span><span class="sxs-lookup"><span data-stu-id="8f89f-518">Following is an example of a class that can be enumerated:</span></span>

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

<span data-ttu-id="8f89f-519">Bevor die Schleife beginnt, wird der Enumeratorausdruck ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-519">Before the loop begins, the enumerator expression is evaluated.</span></span> <span data-ttu-id="8f89f-520">Wenn der Typ des Ausdrucks das Entwurfsmuster nicht erfüllt, wird der Ausdruck in `System.Collections.IEnumerable` oder `System.Collections.Generic.IEnumerable(Of T)` umgewandelt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-520">If the type of the expression does not satisfy the design pattern, then the expression is cast to `System.Collections.IEnumerable` or `System.Collections.Generic.IEnumerable(Of T)`.</span></span> <span data-ttu-id="8f89f-521">Wenn der Ausdruckstyp die generische Schnittstelle implementiert, wird die generische Schnittstelle zur Kompilierzeit bevorzugt, aber die nicht generische Schnittstelle wird zur Laufzeit bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-521">If the expression type implements the generic interface, the generic interface is preferred at compile-time but the non-generic interface is preferred at run-time.</span></span> <span data-ttu-id="8f89f-522">Wenn der Ausdruckstyp die generische Schnittstelle mehrmals implementiert, wird die Anweisung als mehrdeutig angesehen und ein Kompilierzeitfehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-522">If the expression type implements the generic interface multiple times, the statement is considered ambiguous and a compile-time error occurs.</span></span>

<span data-ttu-id="8f89f-523">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="8f89f-523">__Note.__</span></span> <span data-ttu-id="8f89f-524">Die nicht generische Schnittstelle wird im spät gebundenen Fall bevorzugt, da das Auswählen der generischen Schnittstelle bedeuten würde, dass alle Aufrufe der Schnittstellen Methoden Typparameter einschließen würden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-524">The non-generic interface is preferred in the late bound case, because picking the generic interface would mean that all the calls to the interface methods would involve type parameters.</span></span> <span data-ttu-id="8f89f-525">Da es nicht möglich ist, die übereinstimmenden Typargumente zur Laufzeit zu erkennen, müssten alle diese Aufrufe mithilfe von spät gebundenen aufrufen erfolgen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-525">Since it is not possible to know the matching type arguments at run-time, all such calls would have to be made using late-bound calls.</span></span> <span data-ttu-id="8f89f-526">Dies ist langsamer als das Aufrufen der nicht generischen-Schnittstelle, da die nicht generische-Schnittstelle mithilfe von Kompilierzeit aufrufen aufgerufen werden kann.</span><span class="sxs-lookup"><span data-stu-id="8f89f-526">This would be slower than calling the non-generic interface because the non-generic interface could be called using compile-time calls.</span></span>

<span data-ttu-id="8f89f-527">`GetEnumerator` wird für den resultierenden Wert aufgerufen, und der Rückgabewert der Funktion wird in einem temporären gespeichert.</span><span class="sxs-lookup"><span data-stu-id="8f89f-527">`GetEnumerator` is called on the resulting value and the return value of the function is stored in a temporary.</span></span> <span data-ttu-id="8f89f-528">Am Anfang jeder Iterationen wird `MoveNext` für den temporären aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-528">Then at the beginning of each iteration, `MoveNext` is called on the temporary.</span></span> <span data-ttu-id="8f89f-529">Wenn `False` zurückgegeben wird, wird die Schleife beendet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-529">If it returns `False`, the loop terminates.</span></span> <span data-ttu-id="8f89f-530">Andernfalls wird jede Iterations Schleife wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="8f89f-530">Otherwise, each iteration of the loop is executed as follows:</span></span>

1. <span data-ttu-id="8f89f-531">Wenn die Schleifen Steuerungs Variable eine neue lokale Variable (anstatt eine bereits vorhandene) identifiziert hat, wird eine neue Kopie dieser lokalen Variablen erstellt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-531">If the loop control variable identified a new local variable (rather than a pre-existing one), then a fresh copy of this local variable is created.</span></span> <span data-ttu-id="8f89f-532">Bei der aktuellen Iterationen verweisen alle Verweise innerhalb des Schleifen Blocks auf diese Kopie.</span><span class="sxs-lookup"><span data-stu-id="8f89f-532">For the current iteration, all references within the loop block will refer to this copy.</span></span>
2. <span data-ttu-id="8f89f-533">Die `Current`-Eigenschaft wird abgerufen und in den Typ der Schleifen Steuerungsvariablen umgewandelt (unabhängig davon, ob die Konvertierung implizit oder explizit erfolgt) und der Schleifen Steuerungsvariablen zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-533">The `Current` property is retrieved, coerced to the type of the loop control variable (regardless of whether the conversion is implicit or explicit), and assigned to the loop control variable.</span></span>
3. <span data-ttu-id="8f89f-534">Der Schleifen Block wird ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-534">The loop block executes.</span></span>

<span data-ttu-id="8f89f-535">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="8f89f-535">__Note.__</span></span> <span data-ttu-id="8f89f-536">Es gibt eine geringfügige Änderung des Verhaltens zwischen Version 10,0 und 11,0 der Sprache.</span><span class="sxs-lookup"><span data-stu-id="8f89f-536">There is a slight change in behavior between version 10.0 and 11.0 of the language.</span></span> <span data-ttu-id="8f89f-537">Vor 11,0 wurde für jede Iterations Schleife *keine* neue Iterations Variable erstellt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-537">Prior to 11.0, a fresh iteration variable was *not* created for each iteration of the loop.</span></span> <span data-ttu-id="8f89f-538">Dieser Unterschied ist nur sichtbar, wenn die Iterations Variable von einem Lambda-oder LINQ-Ausdruck aufgezeichnet wird, der dann nach der-Schleife aufgerufen wird:</span><span class="sxs-lookup"><span data-stu-id="8f89f-538">This difference is observable only if the iteration variable is captured by a lambda or a LINQ expression which is then invoked after the loop:</span></span>

```vb
Dim lambdas As New List(Of Action)
For Each x In {1,2,3}
   lambdas.Add(Sub() Console.WriteLine(x)
Next
lambdas(0).Invoke()
lambdas(1).Invoke()
lambdas(2).Invoke()
```

<span data-ttu-id="8f89f-539">Bis zu Visual Basic 10,0 wurde eine Warnung zur Kompilierzeit erzeugt, und "3" wurde dreimal gedruckt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-539">Up to Visual Basic 10.0, this produced a warning at compile-time and printed "3" three times.</span></span> <span data-ttu-id="8f89f-540">Der Grund war, dass nur eine einzelne "x"-Variable von allen Iterationen der Schleife gemeinsam verwendet wurde, und alle drei Lambdas haben dasselbe "x" erfasst, und zum Zeitpunkt der Ausführung der Lambda-Ausdrücke enthielt Sie die Zahl 3.</span><span class="sxs-lookup"><span data-stu-id="8f89f-540">That was because there was only a single variable "x" shared by all iterations of the loop, and all three lambdas captured the same "x", and by the time the lambdas were executed it then held the number 3.</span></span> <span data-ttu-id="8f89f-541">Ab Visual Basic 11,0 wird "1, 2, 3" ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="8f89f-541">As of Visual Basic 11.0, it prints "1, 2, 3".</span></span> <span data-ttu-id="8f89f-542">Dies liegt daran, dass jeder Lambda-Ausdruck eine andere Variable "x" erfasst.</span><span class="sxs-lookup"><span data-stu-id="8f89f-542">That is because each lambda captures a different variable "x".</span></span>


<span data-ttu-id="8f89f-543">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="8f89f-543">__Note.__</span></span> <span data-ttu-id="8f89f-544">Das aktuelle Element der Iterationen wird in den Typ der Schleifen Steuerungsvariablen konvertiert, auch wenn die Konvertierung explizit ist, da es keine bequeme Stelle gibt, um einen Konvertierungs Operator in der Anweisung einzuführen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-544">The current element of the iteration is converted to the type of the loop control variable even if the conversion is explicit because there is no convenient place to introduce a conversion operator in the statement.</span></span> <span data-ttu-id="8f89f-545">Dies war besonders problematisch beim Arbeiten mit dem mittlerweile veralteten Typ `System.Collections.ArrayList`, da der Elementtyp `Object` ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-545">This became particularly troublesome when working with the now-obsolete type `System.Collections.ArrayList`, because its element type is `Object`.</span></span> <span data-ttu-id="8f89f-546">Dies hätte die erforderlichen Umwandlungen in einer großen Zahl von Schleifen enthalten, was uns als ideal empfunden hat.</span><span class="sxs-lookup"><span data-stu-id="8f89f-546">This would have required casts in a great many loops, something we felt was not ideal.</span></span> <span data-ttu-id="8f89f-547">Ironischerweise ermöglichte Generika das Erstellen einer stark typisierten Auflistung, `System.Collections.Generic.List(Of T)`, was uns möglicherweise zum überdenken dieses Entwurfs Punkts geführt hat, aber aus Kompatibilitätsgründen kann dies jetzt nicht geändert werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-547">Ironically, generics enabled the creation of a strongly-typed collection, `System.Collections.Generic.List(Of T)`, which might have made us rethink this design point, but for compatibility's sake, this cannot be changed now.</span></span>


<span data-ttu-id="8f89f-548">Wenn die `Next`-Anweisung erreicht wird, wird die Ausführung an den Anfang der Schleife zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="8f89f-548">When the `Next` statement is reached, execution returns to the top of the loop.</span></span> <span data-ttu-id="8f89f-549">Wenn eine Variable nach dem `Next`-Schlüsselwort angegeben wird, muss Sie mit der ersten Variablen nach `For Each` identisch sein.</span><span class="sxs-lookup"><span data-stu-id="8f89f-549">If a variable is specified after the `Next` keyword, it must be the same as the first variable after the `For Each`.</span></span> <span data-ttu-id="8f89f-550">Beachten Sie z. B. folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="8f89f-550">For example, consider the following code:</span></span>

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

<span data-ttu-id="8f89f-551">Dies entspricht dem folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="8f89f-551">It is equivalent to the following code:</span></span>

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

<span data-ttu-id="8f89f-552">Wenn der Typ `E` des Enumerators `System.IDisposable` implementiert, wird der Enumerator beim Beenden der Schleife gelöscht, indem die `Dispose`-Methode aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-552">If the type `E` of the enumerator implements `System.IDisposable`, then the enumerator is disposed upon exiting the loop by calling the `Dispose` method.</span></span> <span data-ttu-id="8f89f-553">Dadurch wird sichergestellt, dass die vom Enumerator gehaltenen Ressourcen freigegeben werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-553">This ensures that resources held by the enumerator are released.</span></span> <span data-ttu-id="8f89f-554">Wenn die Methode, die die `For Each`-Anweisung enthält, keine unstrukturierte Fehlerbehandlung verwendet, wird die `For Each`-Anweisung in eine `Try`-Anweisung umschließt, wobei die `Dispose`-Methode in den `Finally` aufgerufen wird, um die Bereinigung sicherzustellen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-554">If the method containing the `For Each` statement does not use unstructured error handling, then the `For Each` statement is wrapped in a `Try` statement with the `Dispose` method called in the `Finally` to ensure cleanup.</span></span>

<span data-ttu-id="8f89f-555">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="8f89f-555">__Note.__</span></span> <span data-ttu-id="8f89f-556">Der `System.Array`-Typ ist ein Sammlungstyp, und da alle Array Typen von `System.Array` abgeleitet sind, ist jeder arraytypausdruck in einer `For Each`-Anweisung zulässig.</span><span class="sxs-lookup"><span data-stu-id="8f89f-556">The `System.Array` type is a collection type, and since all array types derive from `System.Array`, any array type expression is permitted in a `For Each` statement.</span></span> <span data-ttu-id="8f89f-557">Bei eindimensionalen Arrays listet die `For Each`-Anweisung die Array Elemente in zunehmender Index Reihenfolge auf, beginnend mit Index 0 und endet mit Indexlänge-1.</span><span class="sxs-lookup"><span data-stu-id="8f89f-557">For single-dimensional arrays, the `For Each` statement enumerates the array elements in increasing index order, starting with index 0 and ending with index Length - 1.</span></span> <span data-ttu-id="8f89f-558">Bei mehrdimensionalen Arrays werden die Indizes der äußersten rechten Dimension zuerst angehoben.</span><span class="sxs-lookup"><span data-stu-id="8f89f-558">For multidimensional arrays, the indices of the rightmost dimension are increased first.</span></span>

<span data-ttu-id="8f89f-559">Beispielsweise druckt der folgende Code `1 2 3 4`:</span><span class="sxs-lookup"><span data-stu-id="8f89f-559">For example, the following code prints `1 2 3 4`:</span></span>

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

<span data-ttu-id="8f89f-560">Es ist nicht zulässig, von außerhalb des Blocks in einen `For Each`-Anweisungsblock zu verzweigen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-560">It is not valid to branch into a `For Each` statement block from outside the block.</span></span>


## <a name="exception-handling-statements"></a><span data-ttu-id="8f89f-561">Ausnahme behandlungsanweisungen</span><span class="sxs-lookup"><span data-stu-id="8f89f-561">Exception-Handling Statements</span></span>

<span data-ttu-id="8f89f-562">Visual Basic unterstützt die strukturierte Ausnahmebehandlung und die unstrukturierte Ausnahmebehandlung.</span><span class="sxs-lookup"><span data-stu-id="8f89f-562">Visual Basic supports structured exception handling and unstructured exception handling.</span></span> <span data-ttu-id="8f89f-563">In einer Methode kann nur ein Stil der Ausnahmebehandlung verwendet werden, aber die `Error`-Anweisung kann bei der strukturierten Ausnahmebehandlung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-563">Only one style of exception handling may be used in a method, but the `Error` statement may be used in structured exception handling.</span></span> <span data-ttu-id="8f89f-564">Wenn eine Methode beide Arten der Ausnahmebehandlung verwendet, wird ein Fehler bei der Kompilierzeit ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="8f89f-564">If a method uses both styles of exception handling, a compile-time error results.</span></span>

```antlr
ErrorHandlingStatement
    : StructuredErrorStatement
    | UnstructuredErrorStatement
    ;
```

### <a name="structured-exception-handling-statements"></a><span data-ttu-id="8f89f-565">Strukturierte Ausnahme behandlungsanweisungen</span><span class="sxs-lookup"><span data-stu-id="8f89f-565">Structured Exception-Handling Statements</span></span>

<span data-ttu-id="8f89f-566">Die strukturierte Ausnahmebehandlung ist eine Methode zur Behandlung von Fehlern durch Deklarieren expliziter Blöcke, in denen bestimmte Ausnahmen behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-566">Structured exception handling is a method of handling errors by declaring explicit blocks within which certain exceptions will be handled.</span></span> <span data-ttu-id="8f89f-567">Die strukturierte Ausnahmebehandlung erfolgt über eine `Try`-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="8f89f-567">Structured exception handling is done through a `Try` statement.</span></span>

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


<span data-ttu-id="8f89f-568">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8f89f-568">For example:</span></span>

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

<span data-ttu-id="8f89f-569">Eine `Try`-Anweisung besteht aus drei Arten von Blöcken: Try-Blöcke, catch-Blöcke und schließlich-Blöcke.</span><span class="sxs-lookup"><span data-stu-id="8f89f-569">A `Try` statement is made up of three kinds of blocks: try blocks, catch blocks, and finally blocks.</span></span> <span data-ttu-id="8f89f-570">Ein *try-Block* ist ein Anweisungsblock, der die auszuführenden-Anweisungen enthält.</span><span class="sxs-lookup"><span data-stu-id="8f89f-570">A *try block* is a statement block that contains the statements to be executed.</span></span> <span data-ttu-id="8f89f-571">Ein *catch-Block* ist ein Anweisungsblock, der eine Ausnahme behandelt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-571">A *catch block* is a statement block that handles an exception.</span></span> <span data-ttu-id="8f89f-572">Ein letztes *Block* ist ein Anweisungsblock, der-Anweisungen enthält, die ausgeführt werden, wenn die `Try`-Anweisung beendet wird, unabhängig davon, ob eine Ausnahme aufgetreten ist und behandelt wurde.</span><span class="sxs-lookup"><span data-stu-id="8f89f-572">A *finally block* is a statement block that contains statements to be run when the `Try` statement is exited, regardless of whether an exception has occurred and been handled.</span></span> <span data-ttu-id="8f89f-573">Eine `Try`-Anweisung, die nur einen try-Block und einen schließlich-Block enthalten kann, muss mindestens einen catch-Block oder schließlich einen Block enthalten.</span><span class="sxs-lookup"><span data-stu-id="8f89f-573">A `Try` statement, which can only contain one try block and one finally block, must contain at least one catch block or finally block.</span></span> <span data-ttu-id="8f89f-574">Es ist ungültig, die Ausführung explizit in einen try-Block zu übertragen, mit Ausnahme von innerhalb eines catch-Blocks in derselben Anweisung.</span><span class="sxs-lookup"><span data-stu-id="8f89f-574">It is invalid to explicitly transfer execution into a try block except from within a catch block in the same statement.</span></span>


#### <a name="finally-blocks"></a><span data-ttu-id="8f89f-575">Finally-Blöcke</span><span class="sxs-lookup"><span data-stu-id="8f89f-575">Finally Blocks</span></span>

<span data-ttu-id="8f89f-576">Ein `Finally`-Block wird immer ausgeführt, wenn die Ausführung einen beliebigen Teil der `Try`-Anweisung verlässt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-576">A `Finally` block is always executed when execution leaves any part of the `Try` statement.</span></span> <span data-ttu-id="8f89f-577">Es ist keine explizite Aktion erforderlich, um den `Finally`-Block auszuführen. Wenn die Ausführung die `Try`-Anweisung verlässt, führt das System automatisch den Block "`Finally`" aus und überträgt anschließend die Ausführung an das gewünschte Ziel.</span><span class="sxs-lookup"><span data-stu-id="8f89f-577">No explicit action is required to execute the `Finally` block; when execution leaves the `Try` statement, the system will automatically execute the `Finally` block and then transfer execution to its intended destination.</span></span> <span data-ttu-id="8f89f-578">Der `Finally`-Block wird ausgeführt, unabhängig davon, wie die Ausführung die `Try`-Anweisung verlässt: über das Ende des `Try`-Blocks bis zum Ende eines `Catch`-Blocks, über eine `Exit Try`-Anweisung, über eine `GoTo`-Anweisung oder durch die Behandlung einer ausgelösten Ausnahme.</span><span class="sxs-lookup"><span data-stu-id="8f89f-578">The `Finally` block is executed regardless of how execution leaves the `Try` statement: through the end of the `Try` block, through the end of a `Catch` block, through an `Exit Try` statement, through a `GoTo` statement, or by not handling a thrown exception.</span></span>

<span data-ttu-id="8f89f-579">Beachten Sie, dass der `Await`-Ausdruck in einer Async-Methode und die `Yield`-Anweisung in einer Iteratormethode dazu führen können, dass die Ablauf Steuerung in der Async-oder Iterator-Methoden Instanz angehalten und in einer anderen Methoden Instanz fortgesetzt werden kann.</span><span class="sxs-lookup"><span data-stu-id="8f89f-579">Note that the `Await` expression in an async method, and the `Yield` statement in an iterator method, can cause flow of control to suspend in the async or iterator method instance and resume in some other method instance.</span></span> <span data-ttu-id="8f89f-580">Dies ist jedoch nur eine Unterbrechung der Ausführung und umfasst nicht das Beenden der entsprechenden asynchronen Methode oder iteratormethodeninstanz, weshalb keine `Finally`-Blöcke ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-580">However, this is merely a suspension of execution and does not involve exiting the respective async method or iterator method instance, and so does not cause `Finally` blocks to be executed.</span></span>

<span data-ttu-id="8f89f-581">Die explizite Übertragung der Ausführung in einen `Finally`-Block ist ungültig. Es ist auch ungültig, die Ausführung aus einem `Finally`-Block außer über eine Ausnahme zu übertragen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-581">It is invalid to explicitly transfer execution into a `Finally` block; it is also invalid to transfer execution out of a `Finally` block except through an exception.</span></span>

```antlr
FinallyStatement
    : 'Finally' StatementTerminator
      Block?
    ;
```

#### <a name="catch-blocks"></a><span data-ttu-id="8f89f-582">catch-Blöcke</span><span class="sxs-lookup"><span data-stu-id="8f89f-582">Catch Blocks</span></span>

<span data-ttu-id="8f89f-583">Wenn bei der Verarbeitung des `Try`-Blocks eine Ausnahme auftritt, wird jede `Catch`-Anweisung in der Text Reihenfolge untersucht, um zu bestimmen, ob die Ausnahme behandelt wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-583">If an exception occurs while processing the `Try` block, each `Catch` statement is examined in textual order to determine if it handles the exception.</span></span>

```antlr
CatchStatement
    : 'Catch' ( Identifier ( 'As' NonArrayTypeName )? )?
      ( 'When' BooleanExpression )? StatementTerminator
      Block?
    ;
```

<span data-ttu-id="8f89f-584">Der in einer `Catch`-Klausel angegebene Bezeichner stellt die ausgelöste Ausnahme dar.</span><span class="sxs-lookup"><span data-stu-id="8f89f-584">The identifier specified in a `Catch` clause represents the exception that has been thrown.</span></span> <span data-ttu-id="8f89f-585">Wenn der Bezeichner eine `As`-Klausel enthält, wird der Bezeichner als innerhalb des lokalen Deklarations Raums des `Catch`-Blocks deklariert.</span><span class="sxs-lookup"><span data-stu-id="8f89f-585">If the identifier contains an `As` clause, then the identifier is considered to be declared within the `Catch` block's local declaration space.</span></span> <span data-ttu-id="8f89f-586">Andernfalls muss der Bezeichner eine lokale Variable (keine statische Variable) sein, die in einem enthaltenden Block definiert wurde.</span><span class="sxs-lookup"><span data-stu-id="8f89f-586">Otherwise, the identifier must be a local variable (not a static variable) that was defined in a containing block.</span></span>

<span data-ttu-id="8f89f-587">Eine `Catch`-Klausel ohne Bezeichner fängt alle Ausnahmen ab, die von `System.Exception` abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-587">A `Catch` clause with no identifier will catch all exceptions derived from `System.Exception`.</span></span> <span data-ttu-id="8f89f-588">Eine `Catch`-Klausel mit einem Bezeichner fängt nur Ausnahmen ab, deren Typen mit dem Bezeichnertyp übereinstimmen oder davon abgeleitet sind.</span><span class="sxs-lookup"><span data-stu-id="8f89f-588">A `Catch` clause with an identifier will only catch exceptions whose types are the same as or derived from the type of the identifier.</span></span> <span data-ttu-id="8f89f-589">Der Typ muss `System.Exception` oder ein von `System.Exception` abgeleiteter Typ sein.</span><span class="sxs-lookup"><span data-stu-id="8f89f-589">The type must be `System.Exception`, or a type derived from `System.Exception`.</span></span> <span data-ttu-id="8f89f-590">Wenn eine Ausnahme abgefangen wird, die von `System.Exception` abgeleitet ist, wird ein Verweis auf das Ausnahme Objekt in dem Objekt gespeichert, das von der Funktion `Microsoft.VisualBasic.Information.Err` zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-590">When an exception is caught that derives from `System.Exception`, a reference to the exception object is stored in the object returned by the function `Microsoft.VisualBasic.Information.Err`.</span></span>

<span data-ttu-id="8f89f-591">Eine `Catch`-Klausel mit einer `When`-Klausel fängt nur Ausnahmen ab, wenn der Ausdruck zu `True`; ausgewertet wird. der Typ des Ausdrucks muss ein boolescher Ausdruck sein, wie [boolesche Ausdrücke](expressions.md#boolean-expressions)pro Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-591">A `Catch` clause with a `When` clause will only catch exceptions when the expression evaluates to `True`; the type of the expression must be a Boolean expression as per Section [Boolean Expressions](expressions.md#boolean-expressions).</span></span> <span data-ttu-id="8f89f-592">Eine `When`-Klausel wird nur angewendet, nachdem der Typ der Ausnahme überprüft wurde, und der Ausdruck verweist möglicherweise auf den Bezeichner, der die Ausnahme darstellt, wie in diesem Beispiel veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="8f89f-592">A `When` clause is only applied after checking the type of the exception, and the expression may refer to the identifier representing the exception, as this example demonstrates:</span></span>

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

<span data-ttu-id="8f89f-593">In diesem Beispiel wird Folgendes gedruckt:</span><span class="sxs-lookup"><span data-stu-id="8f89f-593">This example prints:</span></span>

```console
Third handler
```

<span data-ttu-id="8f89f-594">Wenn eine `Catch`-Klausel die Ausnahme behandelt, wird die Ausführung an den `Catch`-Block übertragen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-594">If a `Catch` clause handles the exception, execution transfers to the `Catch` block.</span></span> <span data-ttu-id="8f89f-595">Am Ende des `Catch`-Blocks wird die Ausführung an die erste Anweisung nach der `Try`-Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-595">At the end of the `Catch` block, execution transfers to the first statement following the `Try` statement.</span></span> <span data-ttu-id="8f89f-596">Die `Try`-Anweisung behandelt keine Ausnahmen, die in einem `Catch`-Block ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-596">The `Try` statement will not handle any exceptions thrown in a `Catch` block.</span></span> <span data-ttu-id="8f89f-597">Wenn die Ausnahme von keiner `Catch`-Klausel behandelt wird, wird die Ausführung an einen vom System festgelegten Speicherort übertragen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-597">If no `Catch` clause handles the exception, execution transfers to a location determined by the system.</span></span>

<span data-ttu-id="8f89f-598">Die explizite Übertragung der Ausführung in einen `Catch`-Block ist ungültig.</span><span class="sxs-lookup"><span data-stu-id="8f89f-598">It is invalid to explicitly transfer execution into a `Catch` block.</span></span>

<span data-ttu-id="8f89f-599">Die Filter in WHEN-Klauseln werden normalerweise ausgewertet, bevor die Ausnahme ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-599">The filters in When clauses are normally evaluated prior to the exception being thrown.</span></span> <span data-ttu-id="8f89f-600">Beispielsweise druckt der folgende Code "Filter, abschließend, catch".</span><span class="sxs-lookup"><span data-stu-id="8f89f-600">For instance, the following code will print "Filter, Finally, Catch".</span></span>

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

 <span data-ttu-id="8f89f-601">Asynchrone und Iteratormethoden bewirken jedoch, dass alle schließlich darin enthaltenen Blöcke vor allen Filtern außerhalb von ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-601">However, Async and Iterator methods cause all finally blocks inside them to be executed prior to any filters outside.</span></span> <span data-ttu-id="8f89f-602">Wenn der obige Code beispielsweise `Async Sub Foo()` hätte, wäre die Ausgabe "endlich, Filter, catch".</span><span class="sxs-lookup"><span data-stu-id="8f89f-602">For instance, if the above code had `Async Sub Foo()`, then the output would be "Finally, Filter, Catch".</span></span>


#### <a name="throw-statement"></a><span data-ttu-id="8f89f-603">Throw-Anweisung</span><span class="sxs-lookup"><span data-stu-id="8f89f-603">Throw Statement</span></span>

<span data-ttu-id="8f89f-604">Die `Throw`-Anweisung löst eine Ausnahme aus, die durch eine Instanz eines Typs dargestellt wird, der von `System.Exception` abgeleitet ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-604">The `Throw` statement raises an exception, which is represented by an instance of a type derived from `System.Exception`.</span></span>

```antlr
ThrowStatement
    : 'Throw' Expression? StatementTerminator
    ;
```

<span data-ttu-id="8f89f-605">Wenn der Ausdruck nicht als Wert klassifiziert ist oder kein von `System.Exception` abgeleiteter Typ ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="8f89f-605">If the expression is not classified as a value or is not a type derived from `System.Exception`, then a compile-time error occurs.</span></span> <span data-ttu-id="8f89f-606">Wenn der Ausdruck zur Laufzeit zu einem NULL-Wert ausgewertet wird, wird stattdessen eine Ausnahme vom Typ "`System.NullReferenceException`" ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="8f89f-606">If the expression evaluates to a null value at run time, then a `System.NullReferenceException` exception is raised instead.</span></span>

<span data-ttu-id="8f89f-607">Eine `Throw`-Anweisung kann den Ausdruck innerhalb eines catch-Blocks einer `Try`-Anweisung weglassen, solange kein dazwischenliegende Block vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-607">A `Throw` statement may omit the expression within a catch block of a `Try` statement, as long as there is no intervening finally block.</span></span> <span data-ttu-id="8f89f-608">In diesem Fall löst die-Anweisung die Ausnahme erneut aus, die gerade im catch-Block behandelt wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-608">In that case, the statement rethrows the exception currently being handled within the catch block.</span></span> <span data-ttu-id="8f89f-609">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8f89f-609">For example:</span></span>

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


### <a name="unstructured-exception-handling-statements"></a><span data-ttu-id="8f89f-610">Unstrukturierte Ausnahme behandlungsanweisungen</span><span class="sxs-lookup"><span data-stu-id="8f89f-610">Unstructured Exception-Handling Statements</span></span>

<span data-ttu-id="8f89f-611">Bei der unstrukturierten Ausnahmebehandlung handelt es sich um eine Methode zur Behandlung von Fehlern, die beim Auftreten einer Ausnahme beim Auftreten von Anweisungen zum Verzweigen</span><span class="sxs-lookup"><span data-stu-id="8f89f-611">Unstructured exception handling is a method of handling errors by indicating statements to branch to when an exception occurs.</span></span> <span data-ttu-id="8f89f-612">Die unstrukturierte Ausnahmebehandlung wird mithilfe von drei Anweisungen implementiert: der `Error`-Anweisung, der `On Error`-Anweisung und der `Resume`-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="8f89f-612">Unstructured exception handling is implemented using three statements: the `Error` statement, the `On Error` statement, and the `Resume` statement.</span></span>

```antlr
UnstructuredErrorStatement
    : ErrorStatement
    | OnErrorStatement
    | ResumeStatement
    ;
```

<span data-ttu-id="8f89f-613">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8f89f-613">For example:</span></span>

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

<span data-ttu-id="8f89f-614">Wenn eine Methode eine unstrukturierte Ausnahmebehandlung verwendet, wird ein einzelner strukturierter Ausnahmehandler für die gesamte Methode eingerichtet, die alle Ausnahmen abfängt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-614">When a method uses unstructured exception handling, a single structured exception handler is established for the entire method that catches all exceptions.</span></span> <span data-ttu-id="8f89f-615">(Beachten Sie, dass sich dieser Handler in Konstruktoren nicht über den aufzurufenden Aufrufe von `New` am Anfang des Konstruktors erstreckt.) Die-Methode verfolgt dann den letzten Speicherort des Ausnahme Handlers und die letzte Ausnahme, die ausgelöst wurde.</span><span class="sxs-lookup"><span data-stu-id="8f89f-615">(Note that in constructors this handler does not extend over the call to the call to `New` at the beginning of the constructor.) The method then keeps track of the most recent exception-handler location and the most recent exception that has been thrown.</span></span> <span data-ttu-id="8f89f-616">Beim Einstieg in die-Methode werden der Speicherort des Ausnahme Handlers und die Ausnahme auf `Nothing` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-616">At entry to the method, the exception-handler location and the exception are both set to `Nothing`.</span></span> <span data-ttu-id="8f89f-617">Wenn eine Ausnahme in einer Methode ausgelöst wird, die eine unstrukturierte Ausnahmebehandlung verwendet, wird ein Verweis auf das Ausnahme Objekt in dem Objekt gespeichert, das von der Funktion zurückgegeben wird `Microsoft.VisualBasic.Information.Err`.</span><span class="sxs-lookup"><span data-stu-id="8f89f-617">When an exception is thrown in a method that uses unstructured exception handling, a reference to the exception object is stored in the object returned by the function `Microsoft.VisualBasic.Information.Err`.</span></span>

<span data-ttu-id="8f89f-618">Unstrukturierte Fehler behandlungsanweisungen sind in Iterator-oder Async-Methoden nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="8f89f-618">Unstructured error handling statements are not allowed in iterator or async methods.</span></span>


#### <a name="error-statement"></a><span data-ttu-id="8f89f-619">Error-Anweisung</span><span class="sxs-lookup"><span data-stu-id="8f89f-619">Error Statement</span></span>

<span data-ttu-id="8f89f-620">Eine `Error`-Anweisung löst eine `System.Exception`-Ausnahme aus, die eine Visual Basic 6-Ausnahme Nummer enthält.</span><span class="sxs-lookup"><span data-stu-id="8f89f-620">An `Error` statement throws a `System.Exception` exception containing a Visual Basic 6 exception number.</span></span> <span data-ttu-id="8f89f-621">Der Ausdruck muss als Wert klassifiziert werden, und sein Typ muss implizit in `Integer` konvertiert werden können.</span><span class="sxs-lookup"><span data-stu-id="8f89f-621">The expression must be classified as a value and its type must be implicitly convertible to `Integer`.</span></span>

```antlr
ErrorStatement
    : 'Error' Expression StatementTerminator
    ;
```

#### <a name="on-error-statement"></a><span data-ttu-id="8f89f-622">On Error-Anweisung</span><span class="sxs-lookup"><span data-stu-id="8f89f-622">On Error Statement</span></span>

<span data-ttu-id="8f89f-623">Eine `On Error`-Anweisung ändert den letzten Ausnahme Behandlungs Zustand.</span><span class="sxs-lookup"><span data-stu-id="8f89f-623">An `On Error` statement modifies the most recent exception-handling state.</span></span>

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

<span data-ttu-id="8f89f-624">Sie kann auf eine von vier Arten verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="8f89f-624">It may be used in one of four ways:</span></span>

* <span data-ttu-id="8f89f-625">`On Error GoTo -1` setzt die letzte Ausnahme auf `Nothing` zurück.</span><span class="sxs-lookup"><span data-stu-id="8f89f-625">`On Error GoTo -1` resets the most recent exception to `Nothing`.</span></span>

* <span data-ttu-id="8f89f-626">`On Error GoTo 0` setzt den letzten Speicherort des Ausnahme Handlers auf `Nothing` zurück.</span><span class="sxs-lookup"><span data-stu-id="8f89f-626">`On Error GoTo 0` resets the most recent exception-handler location to `Nothing`.</span></span>

* <span data-ttu-id="8f89f-627">`On Error GoTo LabelName` legt die Bezeichnung als den letzten Speicherort des Ausnahme Handlers fest.</span><span class="sxs-lookup"><span data-stu-id="8f89f-627">`On Error GoTo LabelName` establishes the label as the most recent exception-handler location.</span></span> <span data-ttu-id="8f89f-628">Diese Anweisung kann nicht in einer Methode verwendet werden, die einen Lambda-oder Abfrage Ausdruck enthält.</span><span class="sxs-lookup"><span data-stu-id="8f89f-628">This statement cannot be used in a method that contains a lambda or query expression.</span></span>

* <span data-ttu-id="8f89f-629">`On Error Resume Next` erstellt das `Resume Next`-Verhalten als den letzten Speicherort des Ausnahme Handlers.</span><span class="sxs-lookup"><span data-stu-id="8f89f-629">`On Error Resume Next` establishes the `Resume Next` behavior as the most recent exception-handler location.</span></span>


#### <a name="resume-statement"></a><span data-ttu-id="8f89f-630">Resume-Anweisung</span><span class="sxs-lookup"><span data-stu-id="8f89f-630">Resume Statement</span></span>

<span data-ttu-id="8f89f-631">Eine `Resume`-Anweisung gibt die Ausführung an die-Anweisung zurück, die die letzte Ausnahme ausgelöst hat.</span><span class="sxs-lookup"><span data-stu-id="8f89f-631">A `Resume` statement returns execution to the statement that caused the most recent exception.</span></span>

```antlr
ResumeStatement
    : 'Resume' ResumeClause? StatementTerminator
    ;

ResumeClause
    : 'Next'
    | LabelName
    ;
```

<span data-ttu-id="8f89f-632">Wenn der `Next`-Modifizierer angegeben ist, wird die Ausführung an die Anweisung zurückgegeben, die nach der Anweisung ausgeführt worden wäre, die die letzte Ausnahme verursacht hat.</span><span class="sxs-lookup"><span data-stu-id="8f89f-632">If the `Next` modifier is specified, execution returns to the statement that would have been executed after the statement that caused the most recent exception.</span></span> <span data-ttu-id="8f89f-633">Wenn ein Bezeichnungs Name angegeben ist, wird die Ausführung an die Bezeichnung zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="8f89f-633">If a label name is specified, execution returns to the label.</span></span>

<span data-ttu-id="8f89f-634">Da die `SyncLock`-Anweisung einen impliziten strukturierten Fehler Behandlungs Block enthält, haben `Resume` und `Resume Next` besondere Verhaltensweisen für Ausnahmen, die in `SyncLock`-Anweisungen auftreten.</span><span class="sxs-lookup"><span data-stu-id="8f89f-634">Because the `SyncLock` statement contains an implicit structured error-handling block, `Resume` and `Resume Next` have special behaviors for exceptions that occur in `SyncLock` statements.</span></span> <span data-ttu-id="8f89f-635">`Resume` gibt die Ausführung bis zum Anfang der `SyncLock`-Anweisung zurück, während `Resume Next` die Ausführung an die nächste Anweisung nach der `SyncLock`-Anweisung zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-635">`Resume` returns execution to the beginning of the `SyncLock` statement, while `Resume Next` returns execution to the next statement following the `SyncLock` statement.</span></span> <span data-ttu-id="8f89f-636">Beachten Sie z. B. folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="8f89f-636">For example, consider the following code:</span></span>

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

<span data-ttu-id="8f89f-637">Das folgende Ergebnis wird ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="8f89f-637">It prints the following result.</span></span>

```console
Before exception
Before exception
After SyncLock
```

<span data-ttu-id="8f89f-638">Beim ersten Mal durch die `SyncLock`-Anweisung gibt `Resume` die Ausführung bis zum Anfang der `SyncLock`-Anweisung zurück.</span><span class="sxs-lookup"><span data-stu-id="8f89f-638">The first time through the `SyncLock` statement, `Resume` returns execution to the beginning of the `SyncLock` statement.</span></span> <span data-ttu-id="8f89f-639">Beim zweiten Mal durch die `SyncLock`-Anweisung gibt `Resume Next` die Ausführung bis zum Ende der `SyncLock`-Anweisung zurück.</span><span class="sxs-lookup"><span data-stu-id="8f89f-639">The second time through the `SyncLock` statement, `Resume Next` returns execution to the end of the `SyncLock` statement.</span></span> <span data-ttu-id="8f89f-640">`Resume` und `Resume Next` sind innerhalb einer `SyncLock`-Anweisung nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="8f89f-640">`Resume` and `Resume Next` are not allowed within a `SyncLock` statement.</span></span>

<span data-ttu-id="8f89f-641">Wenn eine `Resume`-Anweisung ausgeführt wird, wird in allen Fällen die letzte Ausnahme auf `Nothing` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-641">In all cases, when a `Resume` statement is executed, the most recent exception is set to `Nothing`.</span></span> <span data-ttu-id="8f89f-642">Wenn eine `Resume`-Anweisung ohne letzte Ausnahme ausgeführt wird, löst die Anweisung eine `System.Exception`-Ausnahme aus, die die Visual Basic Fehlernummer `20` (ohne Fehler fortsetzen) enthält.</span><span class="sxs-lookup"><span data-stu-id="8f89f-642">If a `Resume` statement is executed with no most recent exception, the statement raises a `System.Exception` exception containing the Visual Basic error number `20` (Resume without error).</span></span>


## <a name="branch-statements"></a><span data-ttu-id="8f89f-643">Verzweigungs Anweisungen</span><span class="sxs-lookup"><span data-stu-id="8f89f-643">Branch Statements</span></span>

<span data-ttu-id="8f89f-644">Branchanweisungen ändern den Ausführungs Ablauf in einer Methode.</span><span class="sxs-lookup"><span data-stu-id="8f89f-644">Branch statements modify the flow of execution in a method.</span></span> <span data-ttu-id="8f89f-645">Es gibt sechs Verzweigungs Anweisungen:</span><span class="sxs-lookup"><span data-stu-id="8f89f-645">There are six branch statements:</span></span>

1. <span data-ttu-id="8f89f-646">Eine `GoTo`-Anweisung bewirkt, dass die Ausführung in der-Methode auf die angegebene Bezeichnung übertragen wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-646">A `GoTo` statement causes execution to transfer to the specified label in the method.</span></span> <span data-ttu-id="8f89f-647">Es ist nicht zulässig,-0 in einen `Try`-, `Using`-, `SyncLock`-, `With`-, `For`-oder `For Each`-Block zu @no__t, und auch nicht in einen Schleifen Block, wenn eine lokale Variable dieses Blocks in einem Lambda-oder LINQ-Ausdruck aufgezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-647">It is not allowed to `GoTo` into a `Try`, `Using`, `SyncLock`, `With`, `For` or `For Each` block, nor into any loop block if a local variable of that block is captured in a lambda or LINQ expression.</span></span>
2. <span data-ttu-id="8f89f-648">Eine `Exit`-Anweisung überträgt die Ausführung an die nächste Anweisung nach dem Ende der direkt enthaltenden Block Anweisung der angegebenen Art.</span><span class="sxs-lookup"><span data-stu-id="8f89f-648">An `Exit` statement transfers execution to the next statement after the end of the immediately containing block statement of the specified kind.</span></span> <span data-ttu-id="8f89f-649">Wenn der Block der Methoden Block ist, beendet die Ablauf Steuerung die Methode, wie im Abschnitt Ablauf [Steuerung](statements.md#control-flow)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="8f89f-649">If the block is the method block, then control flow exits the method as described in Section [Control Flow](statements.md#control-flow).</span></span> <span data-ttu-id="8f89f-650">Wenn die `Exit`-Anweisung nicht in der Art von Block enthalten ist, der in der-Anweisung angegeben ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="8f89f-650">If the `Exit` statement is not contained within the kind of block specified in the statement, a compile-time error occurs.</span></span>
3. <span data-ttu-id="8f89f-651">Eine `Continue`-Anweisung überträgt die Ausführung an das Ende der unmittelbar enthaltenden Block Schleifen Anweisung der angegebenen Art.</span><span class="sxs-lookup"><span data-stu-id="8f89f-651">A `Continue` statement transfers execution to the end of the immediately containing block loop statement of the specified kind.</span></span> <span data-ttu-id="8f89f-652">Wenn die `Continue`-Anweisung nicht in der Art von Block enthalten ist, der in der-Anweisung angegeben ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="8f89f-652">If the `Continue` statement is not contained within the kind of block specified in the statement, a compile-time error occurs.</span></span>
4. <span data-ttu-id="8f89f-653">Eine `Stop`-Anweisung bewirkt, dass eine Debugger-Ausnahme auftritt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-653">A `Stop` statement causes a debugger exception to occur.</span></span>
5. <span data-ttu-id="8f89f-654">Mit einer `End`-Anweisung wird das Programm beendet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-654">An `End` statement terminates the program.</span></span> <span data-ttu-id="8f89f-655">Finalizer werden vor dem Herunterfahren ausgeführt, aber die letzten Blöcke aller aktuell ausgeführten `Try`-Anweisungen werden nicht ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-655">Finalizers are run before shutdown, but the finally blocks of any currently executing `Try` statements are not executed.</span></span> <span data-ttu-id="8f89f-656">Diese Anweisung kann nicht in Programmen verwendet werden, die keine ausführbare Datei sind (z. b. DLLs).</span><span class="sxs-lookup"><span data-stu-id="8f89f-656">This statement may not be used in programs that are not executable (for example, DLLs).</span></span>
6. <span data-ttu-id="8f89f-657">Eine `Return`-Anweisung ohne Ausdruck entspricht einer `Exit Sub`-oder `Exit Function`-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="8f89f-657">A `Return` statement with no expression is equivalent to an `Exit Sub` or `Exit Function` statement.</span></span> <span data-ttu-id="8f89f-658">Eine `Return`-Anweisung mit einem Ausdruck ist nur in einer regulären Methode zulässig, die eine Funktion ist, oder in einer asynchronen Methode, bei der es sich um eine Funktion mit dem Rückgabetyp `Task(Of T)` für einige `T` handelt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-658">A `Return` statement with an expression is only allowed in a regular method that is a function, or in an async method that is a function with return type `Task(Of T)` for some `T`.</span></span> <span data-ttu-id="8f89f-659">Der Ausdruck muss als Wert klassifiziert werden, der implizit in die *Funktions Rückgabe Variable* (im Fall regulärer Methoden) oder in die *Aufgaben Rückgabe Variable* (im Fall von Async-Methoden) konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="8f89f-659">The expression must be classified as a value which is implicitly convertible to the *function return variable* (in the case of regular methods) or to the *task return variable* (in the case of async methods).</span></span> <span data-ttu-id="8f89f-660">Das Verhalten besteht darin, den Ausdruck auszuwerten, ihn in der Rückgabe Variablen zu speichern und dann eine implizite `Exit Function`-Anweisung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-660">Its behavior is to evaluate its expression, then store it in the return variable, then execute an implicit `Exit Function` statement.</span></span>

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

## <a name="array-handling-statements"></a><span data-ttu-id="8f89f-661">Array behandlungsanweisungen</span><span class="sxs-lookup"><span data-stu-id="8f89f-661">Array-Handling Statements</span></span>

<span data-ttu-id="8f89f-662">Zwei Anweisungen vereinfachen das Arbeiten mit Arrays: `ReDim`-Anweisungen und `Erase`-Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-662">Two statements simplify working with arrays: `ReDim` statements and `Erase` statements.</span></span>

```antlr
ArrayHandlingStatement
    : RedimStatement
    | EraseStatement
    ;
```

### <a name="redim-statement"></a><span data-ttu-id="8f89f-663">ReDim-Anweisung</span><span class="sxs-lookup"><span data-stu-id="8f89f-663">ReDim Statement</span></span>

<span data-ttu-id="8f89f-664">Eine `ReDim`-Anweisung instanziiert neue Arrays.</span><span class="sxs-lookup"><span data-stu-id="8f89f-664">A `ReDim` statement instantiates new arrays.</span></span>

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

<span data-ttu-id="8f89f-665">Jede Klausel in der Anweisung muss als Variable oder Eigenschafts Zugriff klassifiziert werden, deren Typ ein Arraytyp oder `Object` ist, und eine Liste von Array Begrenzungen folgt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-665">Each clause in the statement must be classified as a variable or a property access whose type is an array type or `Object`, and be followed by a list of array bounds.</span></span> <span data-ttu-id="8f89f-666">Die Anzahl der Begrenzungen muss mit dem Typ der Variablen übereinstimmen. eine beliebige Anzahl von Begrenzungen ist für `Object` zulässig.</span><span class="sxs-lookup"><span data-stu-id="8f89f-666">The number of the bounds must be consistent with the type of the variable; any number of bounds is allowed for `Object`.</span></span> <span data-ttu-id="8f89f-667">Zur Laufzeit wird ein Array für jeden Ausdruck von links nach rechts mit den angegebenen Begrenzungen instanziiert und dann der Variablen oder Eigenschaft zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-667">At run time, an array is instantiated for each expression from left to right with the specified bounds and then assigned to the variable or property.</span></span> <span data-ttu-id="8f89f-668">Wenn der Variablentyp `Object` ist, entspricht die Anzahl der Dimensionen der angegebenen Anzahl von Dimensionen, und der Array Elementtyp ist `Object`.</span><span class="sxs-lookup"><span data-stu-id="8f89f-668">If the variable type is `Object`, the number of dimensions is the number of dimensions specified, and the array element type is `Object`.</span></span> <span data-ttu-id="8f89f-669">Wenn die angegebene Anzahl von Dimensionen zur Laufzeit nicht mit der Variablen oder Eigenschaft kompatibel ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="8f89f-669">If the given number of dimensions is incompatible with the variable or property at run time a compile-time error occurs.</span></span> <span data-ttu-id="8f89f-670">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8f89f-670">For example:</span></span>

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

<span data-ttu-id="8f89f-671">Wenn das `Preserve`-Schlüsselwort angegeben ist, müssen die Ausdrücke auch als-Wert klassifiziert werden, und die neue Größe für jede Dimension, mit Ausnahme von ganz rechts, muss mit der Größe des vorhandenen Arrays identisch sein.</span><span class="sxs-lookup"><span data-stu-id="8f89f-671">If the `Preserve` keyword is specified, then the expressions must also be classifiable as a value, and the new size for each dimension except for the rightmost one must be the same as the size of the existing array.</span></span> <span data-ttu-id="8f89f-672">Die Werte im vorhandenen Array werden in das neue Array kopiert: Wenn das neue Array kleiner ist, werden die vorhandenen Werte verworfen. Wenn das neue Array größer ist, werden die zusätzlichen Elemente mit dem Standardwert des Elementtyps des Arrays initialisiert.</span><span class="sxs-lookup"><span data-stu-id="8f89f-672">The values in the existing array are copied into the new array: if the new array is smaller, the existing values are discarded; if the new array is bigger, the extra elements will be initialized to the default value of the element type of the array.</span></span> <span data-ttu-id="8f89f-673">Beachten Sie z. B. folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="8f89f-673">For example, consider the following code:</span></span>

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

<span data-ttu-id="8f89f-674">Es gibt das folgende Ergebnis aus:</span><span class="sxs-lookup"><span data-stu-id="8f89f-674">It prints the following result:</span></span>

```console
3, 0
```

<span data-ttu-id="8f89f-675">Wenn der vorhandene Array Verweis zur Laufzeit ein NULL-Wert ist, wird kein Fehler angegeben.</span><span class="sxs-lookup"><span data-stu-id="8f89f-675">If the existing array reference is a null value at run time, no error is given.</span></span> <span data-ttu-id="8f89f-676">Wenn sich die Größe einer Dimension ändert, wird eine `System.ArrayTypeMismatchException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="8f89f-676">Other than the rightmost dimension, if the size of a dimension changes, a `System.ArrayTypeMismatchException` will be thrown.</span></span>

<span data-ttu-id="8f89f-677">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="8f89f-677">__Note.__</span></span> <span data-ttu-id="8f89f-678">`Preserve` ist kein reserviertes Wort.</span><span class="sxs-lookup"><span data-stu-id="8f89f-678">`Preserve` is not a reserved word.</span></span>


### <a name="erase-statement"></a><span data-ttu-id="8f89f-679">Erase-Anweisung</span><span class="sxs-lookup"><span data-stu-id="8f89f-679">Erase Statement</span></span>

<span data-ttu-id="8f89f-680">Eine `Erase`-Anweisung legt jede der in der-Anweisung angegebenen Array Variablen oder Eigenschaften auf `Nothing` fest.</span><span class="sxs-lookup"><span data-stu-id="8f89f-680">An `Erase` statement sets each of the array variables or properties specified in the statement to `Nothing`.</span></span> <span data-ttu-id="8f89f-681">Jeder Ausdruck in der Anweisung muss als Variablen oder Eigenschafts Zugriff klassifiziert werden, deren Typ ein Arraytyp oder `Object` ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-681">Each expression in the statement must be classified as a variable or property access whose type is an array type or `Object`.</span></span> <span data-ttu-id="8f89f-682">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8f89f-682">For example:</span></span>

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

## <a name="using-statement"></a><span data-ttu-id="8f89f-683">Using-Anweisung</span><span class="sxs-lookup"><span data-stu-id="8f89f-683">Using statement</span></span>

<span data-ttu-id="8f89f-684">Instanzen von Typen werden automatisch vom Garbage Collector freigegeben, wenn eine Auflistung ausgeführt wird und keine Live Verweise auf die Instanz gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-684">Instances of types are automatically released by the garbage collector when a collection is run and no live references to the instance are found.</span></span> <span data-ttu-id="8f89f-685">Wenn ein Typ in einer besonders wertvollen und knappen Ressource (z. b. Datenbankverbindungen oder Datei Handles) enthalten ist, ist es möglicherweise nicht wünschenswert, bis zum nächsten Garbage Collection eine bestimmte Instanz des Typs zu bereinigen, der nicht mehr verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-685">If a type holds on a particularly valuable and scarce resource (such as database connections or file handles), it may not be desirable to wait until the next garbage collection to clean up a particular instance of the type that is no longer in use.</span></span> <span data-ttu-id="8f89f-686">Um eine einfachere Methode zum Freigeben von Ressourcen vor einer Auflistung bereitzustellen, kann ein Typ die `System.IDisposable`-Schnittstelle implementieren.</span><span class="sxs-lookup"><span data-stu-id="8f89f-686">To provide a lightweight way of releasing resources before a collection, a type may implement the `System.IDisposable` interface.</span></span> <span data-ttu-id="8f89f-687">Ein Typ, der eine `Dispose`-Methode verfügbar macht, die aufgerufen werden kann, um zu erzwingen, dass wertvolle Ressourcen sofort freigegeben werden, wie z. b.:</span><span class="sxs-lookup"><span data-stu-id="8f89f-687">A type that does so exposes a `Dispose` method that can be called to force valuable resources to be released immediately, as such:</span></span>

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

<span data-ttu-id="8f89f-688">Die `Using`-Anweisung automatisiert das Abrufen einer Ressource, das Ausführen eines Satzes von Anweisungen und das anschließende verwerfen der Ressource.</span><span class="sxs-lookup"><span data-stu-id="8f89f-688">The `Using` statement automates the process of acquiring a resource, executing a set of statements, and then disposing of the resource.</span></span> <span data-ttu-id="8f89f-689">Die-Anweisung kann zwei Formen annehmen: in einer ist die Ressource eine lokale Variable, die als Teil der-Anweisung deklariert und als reguläre Anweisung der lokalen Variablen Deklaration behandelt wird. in der anderen ist die Ressource das Ergebnis eines Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="8f89f-689">The statement can take two forms: in one, the resource is a local variable declared as a part of the statement and treated as a regular local variable declaration statement; in the other, the resource is the result of an expression.</span></span>

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

<span data-ttu-id="8f89f-690">Wenn die Ressource eine Deklaration der lokalen Variablen Deklaration ist, muss der Typ der lokalen Variablen Deklaration ein Typ sein, der implizit in `System.IDisposable` konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="8f89f-690">If the resource is a local variable declaration statement then the type of the local variable declaration must be a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="8f89f-691">Die deklarierten lokalen Variablen sind schreibgeschützt, sind auf den `Using`-Anweisungsblock festgelegt und müssen einen Initialisierer enthalten.</span><span class="sxs-lookup"><span data-stu-id="8f89f-691">The declared local variables are read-only, scoped to the `Using` statement block and must include an initializer.</span></span> <span data-ttu-id="8f89f-692">Wenn die Ressource das Ergebnis eines Ausdrucks ist, muss der Ausdruck als Wert klassifiziert werden und muss einen Typ aufweisen, der implizit in `System.IDisposable` konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="8f89f-692">If the resource is the result of an expression then the expression must be classified as a value and must be of a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="8f89f-693">Der Ausdruck wird nur einmal am Anfang der Anweisung ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="8f89f-693">The expression is only evaluated once, at the beginning of the statement.</span></span>

<span data-ttu-id="8f89f-694">Der `Using`-Block ist implizit in einer `Try`-Anweisung enthalten, deren schließlich-Block die-Methode `IDisposable.Dispose` für die Ressource aufruft.</span><span class="sxs-lookup"><span data-stu-id="8f89f-694">The `Using` block is implicitly contained by a `Try` statement whose finally block calls the method `IDisposable.Dispose` on the resource.</span></span> <span data-ttu-id="8f89f-695">Dadurch wird sichergestellt, dass die Ressource auch dann verworfen wird, wenn eine Ausnahme ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-695">This ensures the resource is disposed even when an exception is thrown.</span></span> <span data-ttu-id="8f89f-696">Daher ist es ungültig, von außerhalb des Blocks in einen `Using`-Block zu verzweigen, und ein `Using`-Block wird als einzelne Anweisung für `Resume` und `Resume Next` behandelt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-696">As a result, it is invalid to branch into a `Using` block from outside of the block, and a `Using` block is treated as a single statement for the purposes of `Resume` and `Resume Next`.</span></span> <span data-ttu-id="8f89f-697">Wenn die Ressource `Nothing` ist, wird kein "`Dispose`" aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-697">If the resource is `Nothing`, then no call to `Dispose` is made.</span></span> <span data-ttu-id="8f89f-698">Das Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8f89f-698">Thus, the example:</span></span>

```vb
Using f As C = New C()
    ...
End Using
```

<span data-ttu-id="8f89f-699">identisch mit folgendem Ausdruck:</span><span class="sxs-lookup"><span data-stu-id="8f89f-699">is equivalent to:</span></span>

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

<span data-ttu-id="8f89f-700">Eine `Using`-Anweisung, die eine Deklaration der lokalen Variablen Deklaration aufweist, kann mehrere Ressourcen gleichzeitig abrufen. Dies entspricht der Anweisung von `Using`-Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-700">A `Using` statement that has a local variable declaration statement may acquire multiple resources at a time, which is equivalent to nested `Using` statements.</span></span>  <span data-ttu-id="8f89f-701">Beispielsweise eine `Using`-Anweisung in der Form:</span><span class="sxs-lookup"><span data-stu-id="8f89f-701">For example, a `Using` statement of the form:</span></span>

```vb
Using r1 As R = New R(), r2 As R = New R()
    r1.F()
    r2.F()
End Using
```

<span data-ttu-id="8f89f-702">identisch mit folgendem Ausdruck:</span><span class="sxs-lookup"><span data-stu-id="8f89f-702">is equivalent to:</span></span>

```vb
Using r1 As R = New R()
    Using r2 As R = New R()
        r1.F()
        r2.F()
    End Using
End Using
```


## <a name="await-statement"></a><span data-ttu-id="8f89f-703">Erwartungs Anweisung</span><span class="sxs-lookup"><span data-stu-id="8f89f-703">Await Statement</span></span>

<span data-ttu-id="8f89f-704">Eine Erwartungs Anweisung hat dieselbe Syntax wie der Ausdruck eines Erwartungs Operators (Section-Erwartungs [Operator](expressions.md#await-operator)), ist nur in Methoden zulässig, die ebenfalls Erwartungs Ausdrücke zulassen, und weist das gleiche Verhalten wie ein Erwartungs Operator Ausdruck auf.</span><span class="sxs-lookup"><span data-stu-id="8f89f-704">An await statement has the same syntax as an await operator expression (Section [Await Operator](expressions.md#await-operator)), is allowed only in methods that also allow await expressions, and has the same behavior as an await operator expression.</span></span>

<span data-ttu-id="8f89f-705">Sie kann jedoch entweder als Wert oder als void klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-705">However, it may be classified as either a value or void.</span></span> <span data-ttu-id="8f89f-706">Alle Werte, die sich aus der Auswertung des Ausdrucks des Erwartungs Operators ergeben, werden verworfen.</span><span class="sxs-lookup"><span data-stu-id="8f89f-706">Any value resulting from evaluation of the await operator expression is discarded.</span></span>

```antlr
AwaitStatement
    : AwaitOperatorExpression StatementTerminator
    ;
```

## <a name="yield-statement"></a><span data-ttu-id="8f89f-707">Yield-Anweisung</span><span class="sxs-lookup"><span data-stu-id="8f89f-707">Yield Statement</span></span>

<span data-ttu-id="8f89f-708">Yield-Anweisungen beziehen sich auf Iteratormethoden, die im Abschnitt [Iteratormethoden](statements.md#iterator-methods)beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="8f89f-708">Yield statements are related to iterator methods, which are described in Section [Iterator Methods](statements.md#iterator-methods).</span></span>

```antlr
YieldStatement
    : 'Yield' Expression StatementTerminator
    ;
```

<span data-ttu-id="8f89f-709">`Yield` ist ein reserviertes Wort, wenn die unmittelbar einschließende Methode oder der Lambda-Ausdruck, in dem Sie angezeigt wird, einen `Iterator`-Modifizierer aufweist und der `Yield` nach diesem `Iterator`-Modifizierer erscheint. Es ist nicht an anderer Stelle reserviert.</span><span class="sxs-lookup"><span data-stu-id="8f89f-709">`Yield` is a reserved word if the immediately enclosing method or lambda expression in which it appears has an `Iterator` modifier, and if the `Yield` appears after that `Iterator` modifier; it is unreserved elsewhere.</span></span> <span data-ttu-id="8f89f-710">Es ist auch nicht in Präprozessordirektiven reserviert.</span><span class="sxs-lookup"><span data-stu-id="8f89f-710">It is also unreserved in preprocessor directives.</span></span> <span data-ttu-id="8f89f-711">Die yield-Anweisung ist nur im Text einer Methode oder eines Lambda-Ausdrucks zulässig, bei der es sich um ein reserviertes Wort handelt.</span><span class="sxs-lookup"><span data-stu-id="8f89f-711">The yield statement is only allowed in the body of a method or lambda expression where it is a reserved word.</span></span> <span data-ttu-id="8f89f-712">Innerhalb der unmittelbar einschließenden Methode oder des Lambda-Ausdrucks tritt die yield-Anweisung möglicherweise nicht innerhalb des Texts eines `Catch`-oder `Finally`-Blocks oder im Text einer `SyncLock`-Anweisung auf.</span><span class="sxs-lookup"><span data-stu-id="8f89f-712">Within the immediately enclosing method or lambda, the yield statement may not occur inside the body of a `Catch` or `Finally` block, nor inside the body of a `SyncLock` statement.</span></span>

<span data-ttu-id="8f89f-713">Die yield-Anweisung nimmt einen einzelnen Ausdruck an, der als Wert klassifiziert werden muss und dessen Typ implizit in den Typ der *aktuellen Variable des Iterators* (Abschnitts [Iteratormethoden](statements.md#iterator-methods)) der umschließenden Iteratormethode konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="8f89f-713">The yield statement takes a single expression which must be classified as a value and whose type is implicitly convertible to the type of the *iterator current variable* (Section [Iterator Methods](statements.md#iterator-methods)) of its enclosing iterator method.</span></span>

<span data-ttu-id="8f89f-714">Die Ablauf Steuerung erreicht immer nur eine `Yield`-Anweisung, wenn die `MoveNext`-Methode für ein Iteratorobjekt aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="8f89f-714">Control flow only ever reaches a `Yield` statement when the `MoveNext` method is invoked on an iterator object.</span></span> <span data-ttu-id="8f89f-715">(Dies ist darauf zurückzuführen, dass eine iteratormethodeninstanz nur die Anweisungen ausführt, weil die `MoveNext`-oder `Dispose`-Methoden für ein Iteratorobjekt aufgerufen werden. die `Dispose`-Methode führt nur Code in `Finally`-Blöcken aus, wobei `Yield` nicht zulässig ist).</span><span class="sxs-lookup"><span data-stu-id="8f89f-715">(This is because an iterator method instance only ever executes its statements due to the `MoveNext` or `Dispose` methods being called on an iterator object; and the `Dispose` method will only ever execute code in `Finally` blocks, where `Yield` is not allowed).</span></span>

<span data-ttu-id="8f89f-716">Wenn eine `Yield`-Anweisung ausgeführt wird, wird der Ausdruck ausgewertet und in der *aktuellen Iterator-Variablen* der iteratormethodeninstanz gespeichert, die diesem Iteratorobjekt zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="8f89f-716">When a `Yield` statement is executed, its expression is evaluated and stored in the *iterator current variable* of the iterator method instance associated with that iterator object.</span></span> <span data-ttu-id="8f89f-717">Der Wert `True` wird an den aufrufende Instanz von `MoveNext` zurückgegeben, und der Kontrollpunkt dieser Instanz hält den Fortschritt bis zum nächsten Aufruf von `MoveNext` für das Iteratorobjekt an.</span><span class="sxs-lookup"><span data-stu-id="8f89f-717">The value `True` is returned to the invoker of `MoveNext`, and the control point of this instance stops advancing until the next invocation of `MoveNext` on the iterator object.</span></span>

