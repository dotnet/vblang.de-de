---
ms.openlocfilehash: 2cfc4380cd0a27f0aed9011406b2aa0ef4f7d23f
ms.sourcegitcommit: 6eca149bdc736113e0adb709212bd266c9503c33
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2019
ms.locfileid: "47426847"
---
# <a name="expressions"></a><span data-ttu-id="61fcb-101">Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-101">Expressions</span></span>

<span data-ttu-id="61fcb-102">Ein Ausdruck ist eine Sequenz von Operatoren und Operanden, der angibt, dass einer Berechnung eines Werts festgelegt oder eine Variable oder Konstante.</span><span class="sxs-lookup"><span data-stu-id="61fcb-102">An expression is a sequence of operators and operands that specifies a computation of a value, or that designates a variable or constant.</span></span> <span data-ttu-id="61fcb-103">In diesem Kapitel wird die Syntax, die Reihenfolge der Auswertung von Operanden und Operatoren und Bedeutung von Ausdrücken definiert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-103">This chapter defines the syntax, order of evaluation of operands and operators, and meaning of expressions.</span></span>

```antlr
Expression
    : SimpleExpression
    | TypeExpression
    | MemberAccessExpression
    | DictionaryAccessExpression
    | InvocationExpression
    | IndexExpression
    | NewExpression
    | CastExpression
    | OperatorExpression
    | ConditionalExpression
    | LambdaExpression
    | QueryExpression
    | XMLLiteralExpression
    | XMLMemberAccessExpression
    ;
```

## <a name="expression-classifications"></a><span data-ttu-id="61fcb-104">Ausdrucksklassifizierungen</span><span class="sxs-lookup"><span data-stu-id="61fcb-104">Expression Classifications</span></span>

<span data-ttu-id="61fcb-105">Jeder Ausdruck wird als eine der folgenden klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="61fcb-105">Every expression is classified as one of the following:</span></span>

* <span data-ttu-id="61fcb-106">*Ein-Wert.*</span><span class="sxs-lookup"><span data-stu-id="61fcb-106">*A value.*</span></span> <span data-ttu-id="61fcb-107">Jeder Wert verfügt über einen zugeordneten Typ.</span><span class="sxs-lookup"><span data-stu-id="61fcb-107">Every value has an associated type.</span></span>

* <span data-ttu-id="61fcb-108">*Eine Variable.*</span><span class="sxs-lookup"><span data-stu-id="61fcb-108">*A variable.*</span></span> <span data-ttu-id="61fcb-109">Jede Variable hat einen zugeordneten Typ, d. h. den deklarierten Typ der Variablen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-109">Every variable has an associated type, namely the declared type of the variable.</span></span>

* <span data-ttu-id="61fcb-110">*Ein Namespace.*</span><span class="sxs-lookup"><span data-stu-id="61fcb-110">*A namespace.*</span></span> <span data-ttu-id="61fcb-111">Ein Ausdruck mit dieser Klassifizierung kann nur als linke Seite der Memberzugriff stehen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-111">An expression with this classification can only appear as the left side of a member access.</span></span> <span data-ttu-id="61fcb-112">In einem anderen Kontext bewirkt, dass ein Ausdruck, der als Namespace klassifiziert einen Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-112">In any other context, an expression classified as a namespace causes a compile-time error.</span></span>

* <span data-ttu-id="61fcb-113">*Ein Typ.*</span><span class="sxs-lookup"><span data-stu-id="61fcb-113">*A type.*</span></span> <span data-ttu-id="61fcb-114">Ein Ausdruck mit dieser Klassifizierung kann nur als linke Seite der Memberzugriff stehen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-114">An expression with this classification can only appear as the left side of a member access.</span></span> <span data-ttu-id="61fcb-115">In einem anderen Kontext bewirkt, dass ein Ausdruck, der als Typ klassifiziert einen Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-115">In any other context, an expression classified as a type causes a compile-time error.</span></span>

* <span data-ttu-id="61fcb-116">*Eine Methodengruppe* ist eine Gruppe von Methoden, die auf dem gleichen Namen zu überladen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-116">*A method group,* which is a set of methods overloaded on the same name.</span></span> <span data-ttu-id="61fcb-117">Eine Methodengruppe kann es sich um einen Ausdruck für die zielbereitstellungsumgebung und einer Argumentliste für den zugeordneten Typ haben.</span><span class="sxs-lookup"><span data-stu-id="61fcb-117">A method group may have an associated target expression and an associated type argument list.</span></span>

* <span data-ttu-id="61fcb-118">*Ein Methodenzeiger* steht für den Speicherort einer Methode.</span><span class="sxs-lookup"><span data-stu-id="61fcb-118">*A method pointer,* which represents the location of a method.</span></span> <span data-ttu-id="61fcb-119">Ein Methodenzeiger kann es sich um einen Ausdruck für die zielbereitstellungsumgebung und einer Argumentliste für den zugeordneten Typ haben.</span><span class="sxs-lookup"><span data-stu-id="61fcb-119">A method pointer may have an associated target expression and an associated type argument list.</span></span>

* <span data-ttu-id="61fcb-120">*Ein Lambda-Methode,* einer anonymen Methode ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-120">*A lambda method,* which is an anonymous method.</span></span>

* <span data-ttu-id="61fcb-121">*Eine Eigenschaftengruppe* ist eine Gruppe von Eigenschaften, die auf dem gleichen Namen zu überladen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-121">*A property group,* which is a set of properties overloaded on the same name.</span></span> <span data-ttu-id="61fcb-122">Eine Eigenschaftengruppe möglicherweise einen zielbereitstellungsumgebung-Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="61fcb-122">A property group may have an associated target expression.</span></span>

* <span data-ttu-id="61fcb-123">*Ein Eigenschaftenzugriff.*</span><span class="sxs-lookup"><span data-stu-id="61fcb-123">*A property access.*</span></span> <span data-ttu-id="61fcb-124">Jedem Eigenschaftenzugriff ist einen zugeordneten Typ, d. h. den Typ der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="61fcb-124">Every property access has an associated type, namely the type of the property.</span></span> <span data-ttu-id="61fcb-125">Ein Eigenschaftenzugriff kann es sich um ein zielbereitstellungsumgebung Ausdruck enthalten.</span><span class="sxs-lookup"><span data-stu-id="61fcb-125">A property access may have an associated target expression.</span></span>

* <span data-ttu-id="61fcb-126">*Ein spät gebunden Zugriff,* steht für eine Methode oder Eigenschaft zugreifen, die verzögert, bis zur Laufzeit.</span><span class="sxs-lookup"><span data-stu-id="61fcb-126">*A late-bound access,* which represents a method or property access deferred until run-time.</span></span> <span data-ttu-id="61fcb-127">Ein spät gebundener Zugriff kann es sich um einen Ausdruck für die zielbereitstellungsumgebung und einer Argumentliste für den zugeordneten Typ haben.</span><span class="sxs-lookup"><span data-stu-id="61fcb-127">A late-bound access may have an associated target expression and an associated type argument list.</span></span> <span data-ttu-id="61fcb-128">Der Typ, der ein spät gebundener Zugriff ist immer `Object`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-128">The type of a late-bound access is always `Object`.</span></span>

* <span data-ttu-id="61fcb-129">*Ein Event-Zugriff.*</span><span class="sxs-lookup"><span data-stu-id="61fcb-129">*An event access.*</span></span> <span data-ttu-id="61fcb-130">Jedes Ereigniszugriff verfügt über einen zugeordneten Typ, d. h. den Typ des Ereignisses.</span><span class="sxs-lookup"><span data-stu-id="61fcb-130">Every event access has an associated type, namely the type of the event.</span></span> <span data-ttu-id="61fcb-131">Ein Ereignis kann einen Ausdruck für die zielbereitstellungsumgebung zugreifen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-131">An event access may have an associated target expression.</span></span> <span data-ttu-id="61fcb-132">Ein Ereigniszugriff möglicherweise angezeigt, als das erste Argument von der `RaiseEvent`, `AddHandler`, und `RemoveHandler` Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-132">An event access may appear as the first argument of the `RaiseEvent`, `AddHandler`, and `RemoveHandler` statements.</span></span> <span data-ttu-id="61fcb-133">In einem anderen Kontext bewirkt, dass ein Ausdruck, der klassifiziert als Zugriffs-Ereignis einen Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-133">In any other context, an expression classified as an event access causes a compile-time error.</span></span>

* <span data-ttu-id="61fcb-134">*Ein Arrayliteral* steht für die ursprünglichen Werte eines Arrays, dessen Typ noch nicht festgelegt wurden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-134">*An array literal,* which represents the initial values of an array whose type has not yet been determined.</span></span>

* <span data-ttu-id="61fcb-135">*Void.*</span><span class="sxs-lookup"><span data-stu-id="61fcb-135">*Void.*</span></span> <span data-ttu-id="61fcb-136">Dies tritt auf, wenn der Ausdruck einen Aufruf einer Unterroutine oder ein Await-Operator-Ausdruck kein Ergebnis zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-136">This occurs when the expression is an invocation of a subroutine, or an await operator expression with no result.</span></span> <span data-ttu-id="61fcb-137">Ein Ausdruck, der klassifiziert als "void" ist nur im Kontext einer aufrufanweisung oder eine Await-Anweisung gültig.</span><span class="sxs-lookup"><span data-stu-id="61fcb-137">An expression classified as void is only valid in the context of an invocation statement or an await statement.</span></span>

* <span data-ttu-id="61fcb-138">*Ein Standardwert.*</span><span class="sxs-lookup"><span data-stu-id="61fcb-138">*A default value.*</span></span> <span data-ttu-id="61fcb-139">Nur das Literal `Nothing` erzeugt diese Klassifizierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-139">Only the literal `Nothing` produces this classification.</span></span>

<span data-ttu-id="61fcb-140">Das Endergebnis eines Ausdrucks ist in der Regel einen Wert oder eine Variable, mit den anderen Kategorien von Ausdrücken, die wiederum als Zwischenwerte, die nur in bestimmten Kontexten zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="61fcb-140">The final result of an expression is usually a value or a variable, with the other categories of expressions functioning as intermediate values that are only permitted in certain contexts.</span></span>

<span data-ttu-id="61fcb-141">Beachten Sie, dass die Ausdrücke, dessen Typ um einen Typparameter, verwendet werden können, Anweisungen und Ausdrücke, die den Typ eines Ausdrucks mit bestimmten Eigenschaften (z. B. wird ein Verweistyp, Werttyp, der von einigen Typ usw.) erfordern, wenn die Einschränkungen auferlegt. erfüllen Sie die Eigenschaften für den Typparameter.</span><span class="sxs-lookup"><span data-stu-id="61fcb-141">Note that expressions whose type is a type parameter can be used in statements and expressions that require the type of an expression to have certain characteristics (such as being a reference type, value type, deriving from some type, etc.) if the constraints imposed on the type parameter satisfy those characteristics.</span></span>

### <a name="expression-reclassification"></a><span data-ttu-id="61fcb-142">Ausdruck Neuklassifizierung</span><span class="sxs-lookup"><span data-stu-id="61fcb-142">Expression Reclassification</span></span>

<span data-ttu-id="61fcb-143">In der Regel, wenn ein Ausdruck in einem Kontext, die sich von der der Ausdruck klassifiziert werden muss verwendet wird, tritt ein Fehler während der Kompilierung – beispielsweise ein Literal einen Wert zuweisen möchten.</span><span class="sxs-lookup"><span data-stu-id="61fcb-143">Normally, when an expression is used in a context that requires a classification different from that of the expression, a compile-time error occurs -- for example, attempting to assign a value to a literal.</span></span> <span data-ttu-id="61fcb-144">Allerdings in vielen Fällen kann ein Ausdruck, der Klassifizierung durch den Prozess der ändern *neuklassifizierung*.</span><span class="sxs-lookup"><span data-stu-id="61fcb-144">However, in many cases it is possible to change an expression's classification through the process of *reclassification*.</span></span>

<span data-ttu-id="61fcb-145">Wenn neuklassifizierung erfolgreich ist, klicken Sie dann die neuklassifizierung gilt als erweiternd oder einschränkend.</span><span class="sxs-lookup"><span data-stu-id="61fcb-145">If reclassification succeeds, then the reclassification is judged as widening or narrowing.</span></span> <span data-ttu-id="61fcb-146">Sofern nicht anders angegeben, werden alle Umbuchungshistorie in dieser Liste erweitern.</span><span class="sxs-lookup"><span data-stu-id="61fcb-146">Unless otherwise noted, all the reclassifications in this list are widening.</span></span>

<span data-ttu-id="61fcb-147">Die folgenden Typen von Ausdrücken können neu klassifiziert werden:</span><span class="sxs-lookup"><span data-stu-id="61fcb-147">The following types of expressions can be reclassified:</span></span>

* <span data-ttu-id="61fcb-148">Eine Variable kann als Wert neu klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-148">A variable can be reclassified as a value.</span></span> <span data-ttu-id="61fcb-149">Der in der Variable gespeicherte Wert wird abgerufen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-149">The value stored in the variable is fetched.</span></span>

* <span data-ttu-id="61fcb-150">Eine Methodengruppe kann als Wert neu klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-150">A method group can be reclassified as a value.</span></span> <span data-ttu-id="61fcb-151">Der Gruppierungsausdruck für die Methode als ein Aufrufausdruck mit den zugehörigen Zielausdruck Typparameterliste und leere Klammern interpretiert wird (d. h. `f` als interpretiert `f()` und `f(Of Integer)` als interpretiert`f(Of Integer)()`).</span><span class="sxs-lookup"><span data-stu-id="61fcb-151">The method group expression is interpreted as an invocation expression with the associated target expression and type parameter list, and empty parentheses (that is, `f` is interpreted as `f()` and `f(Of Integer)` is interpreted as `f(Of Integer)()`).</span></span> <span data-ttu-id="61fcb-152">Diese neuklassifizierung kann dazu führen, dass der Ausdruck, der weitere neu als ungültig eingestuft wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-152">This reclassification may result in the expression being further reclassified as void.</span></span>

* <span data-ttu-id="61fcb-153">Ein Methodenzeiger kann als Wert neu klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-153">A method pointer can be reclassified as a value.</span></span> <span data-ttu-id="61fcb-154">Diese neuklassifizierung kann nur im Kontext einer Konvertierung auftreten, wenn der Zieltyp bekannt ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-154">This reclassification can only occur in the context of a conversion where the target type is known.</span></span> <span data-ttu-id="61fcb-155">Die Methode Zeiger-Ausdruck wird als Argument ein Delegatausdruck für die Instanziierung des entsprechenden Typs mit der Argumentliste für den zugeordneten Typ interpretiert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-155">The method pointer expression is interpreted as the argument to a delegate instantiation expression of the appropriate type with the associated type argument list.</span></span> <span data-ttu-id="61fcb-156">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-156">For example:</span></span>
    
    ```vb
    Delegate Sub D(i As Integer)
    
    Module Test
        Sub F(i As Integer)
        End Sub
    
        Sub Main()
            Dim del As D
    
            ' The next two lines are equivalent.
            del = AddressOf F
            del = New D(AddressOf F)
        End Sub
    End Module
    ```

* <span data-ttu-id="61fcb-157">Eine Lambda-Methode kann als Wert neu klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-157">A lambda method can be reclassified as a value.</span></span> <span data-ttu-id="61fcb-158">Wenn die neuklassifizierung im Kontext einer Konvertierung auftritt, wenn der Zieltyp bekannt ist, kann eine der beiden Umbuchungshistorie auftreten:</span><span class="sxs-lookup"><span data-stu-id="61fcb-158">If the reclassification occurs in the context of a conversion where the target type is known, then one of two reclassifications can occur:</span></span>
    
  1. <span data-ttu-id="61fcb-159">Wenn der Zieltyp ein Delegattyp ist, wird der Lambda-Methode als Argument für einen Delegaten-konstruktionsausdruck des entsprechenden Typs interpretiert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-159">If the target type is a delegate type, the lambda method is interpreted as the argument to a delegate-construction expression of the appropriate type.</span></span>
    
  2. <span data-ttu-id="61fcb-160">Wenn der Zieltyp ist `System.Linq.Expressions.Expression(Of T)`, und `T` ein Delegattyp ist, und klicken Sie dann die Lambda-Methode interpretiert wird, als würde er in Delegatkonstruktion Ausdruck für verwendet werden `T` und anschließend in einen Ausdrucksbaum konvertiert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-160">If the target type is `System.Linq.Expressions.Expression(Of T)`, and `T` is a delegate type, then the lambda method is interpreted as if it was being used in delegate-construction expression for `T` and then converted to an expression tree.</span></span>
    
  <span data-ttu-id="61fcb-161">Eine Async- oder Iterator-Lambda-Methode kann nur als Argument für einen Delegatkonstruktion Ausdruck interpretiert werden, wenn der Delegat keine ByRef-Parameter.</span><span class="sxs-lookup"><span data-stu-id="61fcb-161">An async or iterator lambda method may only be interpreted as the argument to a delegate-construction expression, if the delegate has no ByRef parameters.</span></span>
    
  <span data-ttu-id="61fcb-162">Wenn die Konvertierung aus allen Parametertypen des Delegaten ab, die entsprechenden Typen des Lambda-Parameter eine einschränkende Konvertierung ist, klicken Sie dann die neuklassifizierung gilt als einschränkende; Andernfalls ist es erweitern.</span><span class="sxs-lookup"><span data-stu-id="61fcb-162">If conversion from any of the delegate's parameter types to the corresponding lambda parameter types is a narrowing conversion, then the reclassification is judged as narrowing; otherwise it is widening.</span></span>
    
  <span data-ttu-id="61fcb-163">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-163">__Note.__</span></span> <span data-ttu-id="61fcb-164">Die genaue Übersetzung zwischen Lambdamethoden und Ausdrucksbaumstrukturen kann nicht zwischen verschiedenen Versionen des Compilers behoben werden und ist nicht Gegenstand dieser Spezifikation.</span><span class="sxs-lookup"><span data-stu-id="61fcb-164">The exact translation between lambda methods and expression trees may not be fixed between versions of the compiler and is beyond the scope of this specification.</span></span> <span data-ttu-id="61fcb-165">Für Microsoft Visual Basic 11.0 möglicherweise alle Lambda-Ausdrücke in Ausdrucksbaumstrukturen jedoch mit folgenden Einschränkungen konvertiert werden: (1) 1.</span><span class="sxs-lookup"><span data-stu-id="61fcb-165">For Microsoft Visual Basic 11.0, all lambda expressions may be converted to expression trees subject to the following restrictions: (1) 1.</span></span>  <span data-ttu-id="61fcb-166">Nur einzeilige Lambda-Ausdrücke ohne ByRef-Parameter können in Ausdrucksbaumstrukturen konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-166">Only single-line lambda expressions without ByRef parameters may be converted to expression trees.</span></span> <span data-ttu-id="61fcb-167">Von der einzeiligen `Sub` Lambdas nur Aufruf-Anweisungen können in Ausdrucksbaumstrukturen konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-167">Of the single-line `Sub` lambdas, only invocation statements may be converted to expression trees.</span></span> <span data-ttu-id="61fcb-168">(2) anonymen Typs Ausdrücke können nicht in ausdrucksbäume konvertiert werden, wenn eine frühere Feldinitialisierer verwendet wird, um einen nachfolgenden Feldinitialisierer, z. B. zu initialisieren `New With {.a=1, .b=.a}`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-168">(2) Anonymous type expressions cannot be converted to expression trees if an earlier field initializer is used to initialize a subsequent field initializer, e.g. `New With {.a=1, .b=.a}`.</span></span> <span data-ttu-id="61fcb-169">(3) objektinitialisiererausdrücken können nicht konvertiert werden in Ausdrucksbaumstrukturen Wenn ein Element des aktuellen Objekts initialisiert wird eines der Feldinitialisierer verwendet wird z. B. `New C1 With {.a=1, .b=.Method1()}`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-169">(3) Object initializer expressions cannot be converted to expression trees if a member of the current object being initialized is used in one of the field initializers, e.g. `New C1 With {.a=1, .b=.Method1()}`.</span></span> <span data-ttu-id="61fcb-170">(4) ein mehrdimensionales Array erstellen Ausdrücke können nur in Ausdrucksbaumstrukturen konvertiert werden, wenn sie dem Elementtyp explizit deklarieren.</span><span class="sxs-lookup"><span data-stu-id="61fcb-170">(4) Multi-dimensional array creation expressions can only be converted to expression trees if they declare their element type explicitly.</span></span> <span data-ttu-id="61fcb-171">(5) Ausdrücken späte Bindung können nicht in ausdrucksbäume konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-171">(5) Late-binding expressions cannot be converted to expression trees.</span></span> <span data-ttu-id="61fcb-172">(6) Wenn ByRef an ein Aufrufausdruck, eine Variable oder ein Feld übergeben wird verfügt jedoch nicht genau den gleichen Typ wie der ByRef-Parameter, oder eine Eigenschaft mit ByRef übergeben wird, werden normal VB-Semantik, dass eine Kopie des Arguments ByRef übergeben wird und der endgültige Wert dann kopiert wird.  in der Variablen oder das Feld oder Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="61fcb-172">(6) When a variable or field is passed ByRef to an invocation expression but does not have exactly the same type as the ByRef parameter, or when a property is passed ByRef, normal VB semantics are that a copy of the argument is passed ByRef and its final value is then copied back into the variable or field or property.</span></span> <span data-ttu-id="61fcb-173">In Ausdrucksbaumstrukturen geschieht das Zurückkopieren nicht.</span><span class="sxs-lookup"><span data-stu-id="61fcb-173">In expression trees, the copy-back does not happen.</span></span> <span data-ttu-id="61fcb-174">(7) alle diese Einschränkungen gelten für geschachtelte Lambda-Ausdrücke ebenfalls.</span><span class="sxs-lookup"><span data-stu-id="61fcb-174">(7) All these restrictions apply to nested lambda expressions as well.</span></span>
    
  <span data-ttu-id="61fcb-175">Wenn der Zieltyp nicht bekannt ist, wird der Lambda-Methode als Argument für eine Instanziierung Delegatausdruck eines anonymen Delegaten-Typs mit der gleichen Signatur der Lambda-Methode interpretiert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-175">If the target type is not known, then the lambda method is interpreted as the argument to a delegate instantiation expression of an anonymous delegate type with the same signature of the lambda method.</span></span> <span data-ttu-id="61fcb-176">Wenn strenge Semantik verwendet wird, und die Art der Parameter ausgelassen werden, tritt ein Fehler während der Kompilierung; andernfalls `Object` wird für jeden fehlenden Parametertyp ersetzt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-176">If strict semantics are being used and the type of any of the parameters are omitted, a compile-time error occurs; otherwise, `Object` is substituted for any missing parameter type.</span></span> <span data-ttu-id="61fcb-177">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-177">For example:</span></span>
    
  ```vb
  Module Test
      Sub Main()
          ' Type of x will be equivalent to Func(Of Object, Object, Object)
          Dim x = Function(a, b) a + b
  
          ' Type of y will be equivalent to Action(Of Object, Object)
          Dim y = Sub(a, b) Console.WriteLine(a + b)
      End Sub
  End Module
  ```

* <span data-ttu-id="61fcb-178">Eine Eigenschaftengruppe kann als Eigenschaftszugriff neu klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-178">A property group can be reclassified as a property access.</span></span> <span data-ttu-id="61fcb-179">Der Eigenschaftsausdruck für die Gruppe wird als ein Indexausdruck mit leeren Klammern interpretiert (d. h. `f` als interpretiert `f()`).</span><span class="sxs-lookup"><span data-stu-id="61fcb-179">The property group expression is interpreted as an index expression with empty parentheses (that is, `f` is interpreted as `f()`).</span></span>

* <span data-ttu-id="61fcb-180">Ein Eigenschaftenzugriff kann als Wert neu klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-180">A property access can be reclassified as a value.</span></span> <span data-ttu-id="61fcb-181">Der Eigenschaftsausdruck der Zugriff wird als ein Aufrufausdruck von interpretiert die `Get` -Accessor der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="61fcb-181">The property access expression is interpreted as an invocation expression of the `Get` accessor of the property.</span></span> <span data-ttu-id="61fcb-182">Wenn die Eigenschaft keine Get-Methode aufweist, tritt auf, klicken Sie dann ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-182">If the property has no getter, then a compile-time error occurs.</span></span>

* <span data-ttu-id="61fcb-183">Ein spät gebundener Zugriff kann als spät gebundene Methode oder der Zugriff auf spät gebundene Eigenschaften neu klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-183">A late-bound access can be reclassified as a late-bound method or late-bound property access.</span></span> <span data-ttu-id="61fcb-184">In einer Situation, in denen für eine spät gebundener Zugriff ein sowohl als eine Methodenzugriff einen Eigenschaftenzugriff neu klassifiziert werden kann, wird die neuklassifizierung auf einen Eigenschaftenzugriff bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-184">In a situation where a late-bound access can be reclassified both as a method access and as a property access, reclassification to a property access is preferred.</span></span>

* <span data-ttu-id="61fcb-185">Ein spät gebundener Zugriff kann als Wert neu klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-185">A late-bound access can be reclassified as a value.</span></span>

* <span data-ttu-id="61fcb-186">Ein Array-literal kann als Wert neu klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-186">An array literal can be reclassified as a value.</span></span> <span data-ttu-id="61fcb-187">Der Typ des Werts wird wie folgt bestimmt:</span><span class="sxs-lookup"><span data-stu-id="61fcb-187">The type of the value is determined as follows:</span></span>

  1. <span data-ttu-id="61fcb-188">Bei der neuklassifizierung im Kontext einer Konvertierung wird, in denen der Zieltyp ist bekannt und der Zieltyp ist ein Arraytyp, ist das Array-literal als ein Wert vom Typ T() neu klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-188">If the reclassification occurs in the context of a conversion where the target type is known and the target type is an array type, then the array literal is reclassified as a value of type T().</span></span> <span data-ttu-id="61fcb-189">Wenn der Zieltyp ist `System.Collections.Generic.IList(Of T)`, `IReadOnlyList(Of T)`, `ICollection(Of T)`, `IReadOnlyCollection(Of T)`, oder `IEnumerable(Of T)`, das Array-literal ist eine Ebene der Schachtelung, und das Arrayliteral ist als ein Wert vom Typ neu klassifiziert `T()`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-189">If the target type is `System.Collections.Generic.IList(Of T)`, `IReadOnlyList(Of T)`, `ICollection(Of T)`, `IReadOnlyCollection(Of T)`, or `IEnumerable(Of T)`, and the array literal has one level of nesting, then the array literal is reclassified as a value of type `T()`.</span></span>

  2. <span data-ttu-id="61fcb-190">Das Array-literal ist, andernfalls auf einen Wert neu klassifiziert, deren Typ, dass ein Array von Rank gleich der Ebene der Schachtelung verwendet wird ist, mit dem Elementtyp bestimmt anhand des bestimmenden Typs der Elemente im Initialisierer; Wenn kein dominanter Typ bestimmt werden kann, `Object` verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-190">Otherwise, the array literal is reclassified to a value whose type is an array of rank equal to the level of nesting is used, with element type determined by the dominant type of the elements in the initializer; if no dominant type can be determined, `Object` is used.</span></span> <span data-ttu-id="61fcb-191">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-191">For example:</span></span>

     ```vb
     ' x Is GetType(Double(,,))
     Dim x = { { { 1, 2.0 }, { 3, 4 } }, { { 5, 6 }, { 7, 8 } } }.GetType()
        
     ' y Is GetType(Integer())
     Dim y = { 1, 2, 3 }.GetType()
        
     ' z Is GetType(Object())
     Dim z = { 1, "2" }.GetType()
        
     ' Error: Inconsistent nesting
     Dim a = { { 10 }, { 20, 30 } }.GetType()
     ```

  <span data-ttu-id="61fcb-192">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-192">__Note.__</span></span> <span data-ttu-id="61fcb-193">Es ist eine geringfügige Änderung im Verhalten zwischen der Version 9.0 und Version 10.0 der Sprache.</span><span class="sxs-lookup"><span data-stu-id="61fcb-193">There is a slight change in behavior between version 9.0 and version 10.0 of the language.</span></span> <span data-ttu-id="61fcb-194">Vor 10.0 Elementinitialisierer Array keine Auswirkungen auf die lokalen Variablen den Typrückschluss, und jetzt dies der Fall.</span><span class="sxs-lookup"><span data-stu-id="61fcb-194">Prior to 10.0, array element initializers did not affect local variable type inference and now they do.</span></span> <span data-ttu-id="61fcb-195">Also `Dim a() = { 1, 2, 3 }` abgeleitet haben würde `Object()` als Typ des `a` in Version 9.0 der Sprache und `Integer()` in Version 10.0.</span><span class="sxs-lookup"><span data-stu-id="61fcb-195">So `Dim a() = { 1, 2, 3 }` would have inferred `Object()` as the type of `a` in version 9.0 of the language and `Integer()` in version 10.0.</span></span>

  <span data-ttu-id="61fcb-196">Klicken Sie dann die neuklassifizierung interpretiert das Array-literal als Ausdruck für die Arrayerstellung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-196">The reclassification then reinterprets the array literal as an array-creation expression.</span></span> <span data-ttu-id="61fcb-197">Also in den Beispielen:</span><span class="sxs-lookup"><span data-stu-id="61fcb-197">So the examples:</span></span>

  ```vb
  Dim x As Double = { 1, 2, 3, 4 }
  Dim y = { "a", "b" }
  ```

  <span data-ttu-id="61fcb-198">entsprechen:</span><span class="sxs-lookup"><span data-stu-id="61fcb-198">are equivalent to:</span></span>

  ```vb
  Dim x As Double = New Double() { 1, 2, 3, 4 }
  Dim y = New String() { "a", "b" }
  ```

  <span data-ttu-id="61fcb-199">Die neuklassifizierung wird beurteilt, als Einschränkung betrachtet, wenn einschränkende Konvertierung von einem Elementausdruck, der den Elementtyp des Arrays ist; Andernfalls wird es als erweiternde beurteilt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-199">The reclassification is judged as narrowing if any conversion from an element expression to the array element type is narrowing; otherwise it is judged as widening.</span></span>

* <span data-ttu-id="61fcb-200">Der Standardwert `Nothing` können als Wert neu klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-200">The default value `Nothing` can be reclassified as a value.</span></span> <span data-ttu-id="61fcb-201">In einem Kontext, in dem der Zieltyp bekannt ist, ist das Ergebnis der Standardwert des Zieltyps.</span><span class="sxs-lookup"><span data-stu-id="61fcb-201">In a context where the target type is known, the result is the default value of the target type.</span></span> <span data-ttu-id="61fcb-202">In einem Kontext, in denen der Zieltyp ist nicht bekannt, ist, das Ergebnis ist ein null-Wert des Typs `Object`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-202">In a context where the target type is not known, the result is a null value of type `Object`.</span></span>

<span data-ttu-id="61fcb-203">Ein Namespace-Ausdruck, Ausdruck, Ereignisausdruck für den Zugriff oder "void" Ausdruck kann nicht neu klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-203">A namespace expression, type expression, event access expression, or void expression cannot be reclassified.</span></span> <span data-ttu-id="61fcb-204">Mehrere Umbuchungshistorie können gleichzeitig ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-204">Multiple reclassifications can be done at the same time.</span></span> <span data-ttu-id="61fcb-205">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-205">For example:</span></span>

```vb
Module Test
    Sub F(i As Integer)
    End Sub

    ReadOnly Property P() As Integer
        Get
        End Get
    End Sub

    Sub Main()
        F(P)
    End Property
End Module
```

<span data-ttu-id="61fcb-206">In diesem Fall die Eigenschaft Gruppierungsausdruck `P` zunächst aus einer Eigenschaftengruppe auf einen Eigenschaftenzugriff neu klassifiziert ist, und klicken Sie dann aus Zugriff auf eine Eigenschaft auf einen anderen Wert neu klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-206">In this case, the property group expression `P` is first reclassified from a property group to a property access and then reclassified from a property access to a value.</span></span> <span data-ttu-id="61fcb-207">Die geringste Anzahl von Umbuchungshistorie werden ausgeführt, um eine gültige Klassifizierung im Kontext zu erreichen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-207">The fewest number of reclassifications are performed to reach a valid classification in the context.</span></span>

## <a name="constant-expressions"></a><span data-ttu-id="61fcb-208">Konstante Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-208">Constant Expressions</span></span>

<span data-ttu-id="61fcb-209">Ein *Konstantenausdruck* ist ein Ausdruck, dessen Wert zum Zeitpunkt der Kompilierung vollständig ausgewertet werden kann.</span><span class="sxs-lookup"><span data-stu-id="61fcb-209">A *constant expression* is an expression whose value can be fully evaluated at compile time.</span></span>

```antlr
ConstantExpression
    : Expression
    ;
```

<span data-ttu-id="61fcb-210">Der Typ ein konstanter Ausdruck sein kann `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Char`, `Single`, `Double`, `Decimal`, `Date`, `Boolean`, `String`, `Object`, oder ein Enumerationstyp.</span><span class="sxs-lookup"><span data-stu-id="61fcb-210">The type of a constant expression can be `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Char`, `Single`, `Double`, `Decimal`, `Date`, `Boolean`, `String`, `Object`, or any enumeration type.</span></span> <span data-ttu-id="61fcb-211">Die folgenden Konstrukte sind in Konstanten Ausdrücken zulässig:</span><span class="sxs-lookup"><span data-stu-id="61fcb-211">The following constructs are permitted in constant expressions:</span></span>

* <span data-ttu-id="61fcb-212">Literale (einschließlich `Nothing`).</span><span class="sxs-lookup"><span data-stu-id="61fcb-212">Literals (including `Nothing`).</span></span>

* <span data-ttu-id="61fcb-213">Verweise auf Konstantentyp-Member "oder" constant "lokal".</span><span class="sxs-lookup"><span data-stu-id="61fcb-213">References to constant type members or constant locals.</span></span>

* <span data-ttu-id="61fcb-214">Verweise auf Member von Enumerationstypen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-214">References to members of enumeration types.</span></span>

* <span data-ttu-id="61fcb-215">Unterausdrücke in Klammern.</span><span class="sxs-lookup"><span data-stu-id="61fcb-215">Parenthesized subexpressions.</span></span>

* <span data-ttu-id="61fcb-216">Umwandlungsausdrücke, vorausgesetzt, dass der Zieltyp eine der oben aufgeführten Typen ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-216">Coercion expressions, provided the target type is one of the types listed above.</span></span> <span data-ttu-id="61fcb-217">Umwandlungen in und aus `String` stellen eine Ausnahme von dieser Regel und sind nur für null-Werte zulässig, da `String` Konvertierungen zur Laufzeit immer in der aktuellen Kultur der ausführungsumgebung ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-217">Coercions to and from `String` are an exception to this rule and are only allowed on null values because `String` conversions are always done in the current culture of the execution environment at run time.</span></span> <span data-ttu-id="61fcb-218">Beachten Sie, dass Konstanten Umwandlungsausdrücke systeminterne Konvertierungen nur verwenden können.</span><span class="sxs-lookup"><span data-stu-id="61fcb-218">Note that constant coercion expressions can only ever use intrinsic conversions.</span></span>

* <span data-ttu-id="61fcb-219">Die `+`, `-` und `Not` unäre Operatoren, bereitgestellt, die Operanden und Ergebnis ist ein Typ, der oben aufgeführten.</span><span class="sxs-lookup"><span data-stu-id="61fcb-219">The `+`, `-` and `Not` unary operators, provided the operand and result is of a type listed above.</span></span>

* <span data-ttu-id="61fcb-220">Die `+`, `-`, `*`, `^`, `Mod`, `/`, `\`, `<<`, `>>`, `&`, `And`, `Or`, `Xor`, `AndAlso`, `OrElse`, `=`, `<`, `>`, `<>`, `<=`, und `=>` binäre Operatoren angegeben von jeder Operand und Ergebnis ist ein Typ, der oben aufgeführten.</span><span class="sxs-lookup"><span data-stu-id="61fcb-220">The `+`, `-`, `*`, `^`, `Mod`, `/`, `\`, `<<`, `>>`, `&`, `And`, `Or`, `Xor`, `AndAlso`, `OrElse`, `=`, `<`, `>`, `<>`, `<=`, and `=>` binary operators, provided each operand and result is of a type listed above.</span></span>

* <span data-ttu-id="61fcb-221">Der bedingte Operator sofern, jeden Operanden und das Ergebnis ist ein Typ, der oben aufgeführten.</span><span class="sxs-lookup"><span data-stu-id="61fcb-221">The conditional operator If, provided each operand and result is of a type listed above.</span></span>

* <span data-ttu-id="61fcb-222">Die folgenden Funktionen der Laufzeit: `Microsoft.VisualBasic.Strings.ChrW`; `Microsoft.VisualBasic.Strings.Chr` ist von der Konstante Wert zwischen 0 und 128; `Microsoft.VisualBasic.Strings.AscW` ist die Konstante Zeichenfolge nicht leer ist. `Microsoft.VisualBasic.Strings.Asc` Wenn die Konstante Zeichenfolge nicht leer ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-222">The following run-time functions: `Microsoft.VisualBasic.Strings.ChrW`; `Microsoft.VisualBasic.Strings.Chr` if the constant value is between 0 and 128; `Microsoft.VisualBasic.Strings.AscW` if the constant string is not empty; `Microsoft.VisualBasic.Strings.Asc` if the constant string is not empty.</span></span>

<span data-ttu-id="61fcb-223">Die folgenden Konstrukte sind *nicht* in Konstanten Ausdrücken zulässig:</span><span class="sxs-lookup"><span data-stu-id="61fcb-223">The following constructs are *not* permitted in constant expressions:</span></span>

* <span data-ttu-id="61fcb-224">Implizite Bindung über einen `With` Kontext.</span><span class="sxs-lookup"><span data-stu-id="61fcb-224">Implicit binding through a `With` context.</span></span>

<span data-ttu-id="61fcb-225">Konstante Ausdrücke ein ganzzahliger Typ (`ULong`, `Long`, `UInteger`, `Integer`, `UShort`, `Short`, `SByte`, oder `Byte`) implizit in einen engeren ganzzahligen Typ konvertiert werden kann und Konstante Ausdrücke vom Typ `Double` kann implizit konvertiert werden, um `Single`, sofern der Wert des konstanten Ausdrucks innerhalb des Bereichs des Zieltyps liegt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-225">Constant expressions of an integral type (`ULong`, `Long`, `UInteger`, `Integer`, `UShort`, `Short`, `SByte`, or `Byte`) can be implicitly converted to a narrower integral type, and constant expressions of type `Double` can be implicitly converted to `Single`, provided the value of the constant expression is within the range of the destination type.</span></span> <span data-ttu-id="61fcb-226">Diese einschränkende Konvertierungen sind zulässig, unabhängig davon, ob flexible oder eine strikte Semantik verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-226">These narrowing conversions are allowed regardless of whether permissive or strict semantics are being used.</span></span>


## <a name="late-bound-expressions"></a><span data-ttu-id="61fcb-227">Spät gebundene Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-227">Late-Bound Expressions</span></span>

<span data-ttu-id="61fcb-228">Wenn das Ziel eines Memberzugriffsausdrucks oder Feldindex-Ausdrucks ist vom Typ `Object`, die Verarbeitung des Ausdrucks möglicherweise erst zur Laufzeit verzögert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-228">When the target of a member access expression or index expression is of type `Object`, the processing of the expression may be deferred until run time.</span></span> <span data-ttu-id="61fcb-229">Verschieben die Verarbeitung auf diese Weise wird aufgerufen, *späte Bindung*.</span><span class="sxs-lookup"><span data-stu-id="61fcb-229">Deferring processing this way is called *late binding*.</span></span> <span data-ttu-id="61fcb-230">Ermöglicht die späte Bindung `Object` Variablen für die in einem *typenlosen* Weise, in dem alle Auflösung von Elementen der eigentliche Laufzeittyp des Werts in der Variablen hängt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-230">Late binding allows `Object` variables to be used in a *typeless* way, where all resolution of members is based on the actual run-time type of the value in the variable.</span></span> <span data-ttu-id="61fcb-231">Wenn strikte Semantik, durch die kompilierungsumgebung oder durch angegeben werden `Option Strict`, späte Bindung führt dazu, dass einen Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-231">If strict semantics are specified by the compilation environment or by `Option Strict`, late binding causes a compile-time error.</span></span> <span data-ttu-id="61fcb-232">Nicht öffentliche Member werden ignoriert, wenn späte Bindung, einschließlich der im Rahmen der Auflösung von funktionsüberladungen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-232">Non-public members are ignored when doing late-binding, including for the purposes of overload resolution.</span></span> <span data-ttu-id="61fcb-233">Beachten Sie, dass ein Unterschied zu früh gebundene, Aufrufen von oder zugreifen auf eine `Shared` spät Datenmember gebundenen führt dazu, dass das Aufrufziel zur Laufzeit ausgewertet werden soll.</span><span class="sxs-lookup"><span data-stu-id="61fcb-233">Note that, unlike the early-bound case, invoking or accessing a `Shared` member late-bound will cause the invocation target to be evaluated at run time.</span></span><span data-ttu-id="61fcb-234"> Wenn der Ausdruck einen Aufruf für ein Element definiert ist `System.Object\`, späte Bindung nicht erfolgt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-234"> If the expression is an invocation expression for a member defined on `System.Object\`, late binding will not take place.</span></span>

<span data-ttu-id="61fcb-235">Im Allgemeinen werden spät gebundene Zugriffe aufgelöst zur Laufzeit durch Aufrufen der Bezeichner für die eigentliche Laufzeittyp des Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="61fcb-235">In general, late-bound accesses are resolved at run time by looking up the identifier on the actual run-time type of the expression.</span></span> <span data-ttu-id="61fcb-236">Wenn das spät gebundene Membersuche zur Laufzeit ein Fehler auftritt ein `System.MissingMemberException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="61fcb-236">If late-bound member lookup fails at run time, a `System.MissingMemberException` exception is thrown.</span></span> <span data-ttu-id="61fcb-237">Da die spät gebundene Memberlookup erfolgt ausschließlich aus der Laufzeittyp des zugeordneten Zielausdrucks-Laufzeittyp eines Objekts ist nie eine Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="61fcb-237">Because late-bound member lookup is done solely off the run-time type of the associated target expression, an object's run-time type is never an interface.</span></span> <span data-ttu-id="61fcb-238">Aus diesem Grund ist es unmöglich, die Schnittstellenmember in eine spät gebundene Memberzugriffsausdruck zugreifen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-238">Therefore, it is impossible to access interface members in a late-bound member access expression.</span></span>

<span data-ttu-id="61fcb-239">Die Argumente für eine spät gebundene Member zugreifen, werden in der Reihenfolge des Memberzugriffsausdrucks ausgewertet: nicht in der Reihenfolge in der Parameter im spät gebundene Member deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-239">The arguments to a late-bound member access are evaluated in the order they appear in the member access expression: not the order in which parameters are declared in the late-bound member.</span></span> <span data-ttu-id="61fcb-240">Das folgende Beispiel veranschaulicht diesen Unterschied:</span><span class="sxs-lookup"><span data-stu-id="61fcb-240">The following example illustrates this difference:</span></span>

```vb
Class C
    Public Sub f(ByVal x As Integer, ByVal y As Integer)
    End Sub
End Class

Module Module1
    Sub Main()
        Console.Write("Early-bound: ")
        Dim c As C = New C
        c.f(y:=t("y"), x:=t("x"))

        Console.Write(vbCrLf & "Late-bound: ")
        Dim o As Object = New C
        o.f(y:=t("y"), x:=t("x"))
    End Sub

    Function t(ByVal s As String) As Integer
        Console.Write(s)
        Return 0
    End Function
End Module
```

<span data-ttu-id="61fcb-241">Dieser Code zeigt:</span><span class="sxs-lookup"><span data-stu-id="61fcb-241">This code displays:</span></span>

```
Early-bound: xy
Late-bound: yx
```

<span data-ttu-id="61fcb-242">Da die spät gebundene überladungsauflösung für den Runtime-Typ der Argumente erfolgt, ist es möglich, dass ein Ausdruck möglicherweise zu unterschiedlichen Ergebnissen führen basierend auf der gibt an, ob sie zur Kompilierzeit oder zur Laufzeit ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-242">Because late-bound overload resolution is done on the run-time type of the arguments, it is possible that an expression might produce different results based on whether it is evaluated at compile time or run time.</span></span> <span data-ttu-id="61fcb-243">Das folgende Beispiel veranschaulicht diesen Unterschied:</span><span class="sxs-lookup"><span data-stu-id="61fcb-243">The following example illustrates this difference:</span></span>

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class

Module Test
    Sub F(b As Base)
        Console.WriteLine("F(Base)")
    End Sub

    Sub F(d As Derived)
        Console.WriteLine("F(Derived)")
    End Sub

    Sub Main()
        Dim b As Base = New Derived()
        Dim o As Object = b

        F(b)
        F(o)
    End Sub
End Module
```

<span data-ttu-id="61fcb-244">Dieser Code zeigt:</span><span class="sxs-lookup"><span data-stu-id="61fcb-244">This code displays:</span></span>

```
F(Base)
F(Derived)
```

## <a name="simple-expressions"></a><span data-ttu-id="61fcb-245">Einfache Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-245">Simple Expressions</span></span>

<span data-ttu-id="61fcb-246">Einfache Ausdrücke sind Literale, Ausdrücke in Klammern, Instanz Ausdrücke oder Ausdrücke mit einfachen Namen an.</span><span class="sxs-lookup"><span data-stu-id="61fcb-246">Simple expressions are literals, parenthesized expressions, instance expressions, or simple name expressions.</span></span>

```antlr
SimpleExpression
    : LiteralExpression
    | ParenthesizedExpression
    | InstanceExpression
    | SimpleNameExpression
    | AddressOfExpression
    ;
```

### <a name="literal-expressions"></a><span data-ttu-id="61fcb-247">Literale Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-247">Literal Expressions</span></span>

<span data-ttu-id="61fcb-248">Literale Ausdrücke ausgewertet werden auf den Wert, der durch das Literal dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-248">Literal expressions evaluate to the value represented by the literal.</span></span> <span data-ttu-id="61fcb-249">Ein literalen Ausdruck wird als ein Wert, mit Ausnahme des Literals klassifiziert `Nothing`, die als Standardwert klassifiziert wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-249">A literal expression is classified as a value, except for the literal `Nothing`, which is classified as a default value.</span></span>

```antlr
LiteralExpression
    : Literal
    ;
```

### <a name="parenthesized-expressions"></a><span data-ttu-id="61fcb-250">Ausdrücke in Klammern</span><span class="sxs-lookup"><span data-stu-id="61fcb-250">Parenthesized Expressions</span></span>

<span data-ttu-id="61fcb-251">Ein Ausdruck in Klammern besteht aus einem Ausdruck in Klammern eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-251">A parenthesized expression consists of an expression enclosed in parentheses.</span></span> <span data-ttu-id="61fcb-252">Ein Ausdruck in Klammern wird als Wert klassifiziert, und der eingeschlossene Ausdruck muss als Wert klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-252">A parenthesized expression is classified as a value, and the enclosed expression must be classified as a value.</span></span> <span data-ttu-id="61fcb-253">Ein Ausdruck in Klammern, die auf den Wert des Ausdrucks innerhalb der Klammern ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-253">A parenthesized expression evaluates to the value of the expression within the parentheses.</span></span>

```antlr
ParenthesizedExpression
    : OpenParenthesis Expression CloseParenthesis
    ;
```

### <a name="instance-expressions"></a><span data-ttu-id="61fcb-254">Instanz-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-254">Instance Expressions</span></span>

<span data-ttu-id="61fcb-255">Ein *Instanz Ausdruck* ist das Schlüsselwort `Me`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-255">An *instance expression* is the keyword `Me`.</span></span> <span data-ttu-id="61fcb-256">Es kann nur innerhalb des Texts für einen Accessor nicht freigegeben, Methode, Konstruktor oder einer Eigenschaft verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-256">It may only be used within the body of a non-shared method, constructor, or property accessor.</span></span> <span data-ttu-id="61fcb-257">Es wird als Wert klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-257">It is classified as a value.</span></span> <span data-ttu-id="61fcb-258">Das Schlüsselwort `Me` stellt die Instanz des Typs mit der Methode oder Eigenschaft Accessor, der ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-258">The keyword `Me` represents the instance of the type containing the method or property accessor being executed.</span></span> <span data-ttu-id="61fcb-259">Wenn ein Konstruktor explizit einen anderen Konstruktor aufruft (Abschnitt [Konstruktoren](type-members.md#constructors)), `Me` nicht erst nach dem den Konstruktoraufruf verwendet werden, da die Instanz noch nicht erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="61fcb-259">If a constructor explicitly invokes another constructor (Section [Constructors](type-members.md#constructors)), `Me` cannot be used until after that constructor call, because the instance has not yet been constructed.</span></span>

```antlr
InstanceExpression
    : 'Me'
    ;
```

### <a name="simple-name-expressions"></a><span data-ttu-id="61fcb-260">Ausdrücke für einfache Namen</span><span class="sxs-lookup"><span data-stu-id="61fcb-260">Simple Name Expressions</span></span>

<span data-ttu-id="61fcb-261">Ein *einfachen Namensausdruck* besteht aus einem einzelnen Bezeichner gefolgt von einer optionalen Liste der Typargumente.</span><span class="sxs-lookup"><span data-stu-id="61fcb-261">A *simple name expression* consists of a single identifier followed by an optional type argument list.</span></span>

```antlr
SimpleNameExpression
    : Identifier ( OpenParenthesis 'Of' TypeArgumentList CloseParenthesis )?
    ;
```

<span data-ttu-id="61fcb-262">Der Name aufgelöst und durch die folgenden "einfache Namen Auflösungsregeln" klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="61fcb-262">The name is resolved and classified by the following "simple name resolution rules":</span></span>

1.  <span data-ttu-id="61fcb-263">Beginnend mit dem unmittelbar einschließenden block und mit jeder Block mit einschließenden äußeren (sofern vorhanden), fortfahren, wenn der Bezeichner der Name eines lokalen Variablen, statischen Variablen, Konstanten lokalen Variable übereinstimmt, Methode geben Sie Parameter oder -Parameter, und klicken Sie dann auf der Bezeichner bezieht die entsprechende Entität.</span><span class="sxs-lookup"><span data-stu-id="61fcb-263">Starting with the immediately enclosing block and continuing with each enclosing outer block (if any), if the identifier matches the name of a local variable, static variable, constant local, method type parameter, or parameter, then the identifier refers to the matching entity.</span></span>

    <span data-ttu-id="61fcb-264">Wenn eine lokale Variable, die statische Variable oder Konstante lokale mit der ID übereinstimmt, und eine Liste der Typargumente bereitgestellt wurde, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-264">If the identifier matches a local variable, static variable, or constant local and a type argument list was provided, a compile-time error occurs.</span></span> <span data-ttu-id="61fcb-265">Wenn der Typparameter einer Methode mit der ID übereinstimmt, und eine Liste der Typargumente bereitgestellt wurde, wird keine Übereinstimmung gefunden, und Auflösung wird fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-265">If the identifier matches a method type parameter and a type argument list was provided, no match occurs and resolution continues.</span></span> <span data-ttu-id="61fcb-266">Wenn eine lokale Variable mit der ID übereinstimmt, wird der lokalen Variablen zugeordnet ist die implizite-Funktion oder `Get` Accessor zurückzugeben, lokale Variable, und der Ausdruck ist Teil eines Aufrufausdrucks, aufrufanweisung, oder ein `AddressOf` Ausdruck dann keine Übereinstimmung kommt und Auflösung wird fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-266">If the identifier matches a local variable, the local variable matched is the implicit function or `Get` accessor return local variable, and the expression is part of an invocation expression, invocation statement, or an `AddressOf` expression, then no match occurs and resolution continues.</span></span>

    <span data-ttu-id="61fcb-267">Der Ausdruck wird als Variable klassifiziert, wenn es sich um eine lokale Variable, die statische Variable oder Parameter handelt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-267">The expression is classified as a variable if it is a local variable, static variable, or parameter.</span></span> <span data-ttu-id="61fcb-268">Der Ausdruck wird als Typ klassifiziert, wenn es sich um einen Methodenparameter des Typs ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-268">The expression is classified as a type if it is a method type parameter.</span></span> <span data-ttu-id="61fcb-269">Der Ausdruck wird als Wert klassifiziert, wenn es sich um eine Konstante lokale handelt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-269">The expression is classified as a value if it is a constant local.</span></span>

2.  <span data-ttu-id="61fcb-270">Für jeden geschachtelten Typ mit dem Ausdruck wird beginnend mit der innersten und möchte die äußerste, wenn eine Suche des Bezeichners in den Typ eine Übereinstimmung mit einem Member zugegriffen werden kann:</span><span class="sxs-lookup"><span data-stu-id="61fcb-270">For each nested type containing the expression, starting from the innermost and going to the outermost, if a lookup of the identifier in the type produces a match with an accessible member:</span></span>

    21. <span data-ttu-id="61fcb-271">Wenn der entsprechende Typmember einen Typparameter ist, klicken Sie dann das Ergebnis wird als Typ klassifiziert und ist der entsprechende Typparameter.</span><span class="sxs-lookup"><span data-stu-id="61fcb-271">If the matching type member is a type parameter, then the result is classified as a type and is the matching type parameter.</span></span> <span data-ttu-id="61fcb-272">Wenn eine Liste der Typargumente angegeben wurde, wird keine Übereinstimmung gefunden, und Auflösung wird fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-272">If a type argument list was provided, no match occurs and resolution continues.</span></span>
    22. <span data-ttu-id="61fcb-273">Andernfalls, wenn der Typ der unmittelbar einschließenden Typ ist und die Suche einen nicht freigegebenen Typmember gibt, klicken Sie dann das Ergebnis ist identisch mit der ein Memberzugriff des Formulars `Me.E(Of A)`, wobei `E` ist der Bezeichner und `A` ist die Liste der Typargumente , falls vorhanden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-273">Otherwise, if the type is the immediately enclosing type and the lookup identifies a non-shared type member, then the result is the same as a member access of the form `Me.E(Of A)`, where `E` is the identifier and `A` is the type argument list, if any.</span></span>
    23. <span data-ttu-id="61fcb-274">Das Ergebnis ist, andernfalls genau identisch mit der ein Memberzugriff des Formulars `T.E(Of A)`, wobei `T` ist der Typ des entsprechenden Elements mit `E` ist der Bezeichner, und `A` ist die Liste der Typargumente, sofern vorhanden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-274">Otherwise, the result is exactly the same as a member access of the form `T.E(Of A)`, where `T` is the type containing the matching member, `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="61fcb-275">In diesem Fall ist es ein Fehler für den Bezeichner zum Verweisen auf einen nicht freigegebenen Member.</span><span class="sxs-lookup"><span data-stu-id="61fcb-275">In this case, it is an error for the identifier to refer to a non-shared member.</span></span>

3.  <span data-ttu-id="61fcb-276">Führen Sie für jeden geschachtelten Namespace, beginnend mit der innersten und möchte den äußersten-Namespace folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="61fcb-276">For each nested namespace, starting from the innermost and going to the outermost namespace, do the following:</span></span>

    31. <span data-ttu-id="61fcb-277">Wenn der Namespace einen zugreifbarer Typ mit dem angegebenen Namen enthält und die gleiche Anzahl von Typparametern aufweist, wie in der Liste der Typargumente, sofern vorhanden, angegeben wurde, und klicken Sie dann den Bezeichner bezieht sich auf diesen Typ und wird als Typ klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-277">If the namespace contains an accessible type with the given name and has the same number of type parameters as was supplied in the type argument list, if any, then the identifier refers to that type and is classified as a type.</span></span>
    32. <span data-ttu-id="61fcb-278">Andernfalls, wenn keine Liste der Typargumente angegeben wurde, und der Namespace eine Namespace-Members mit dem angegebenen Namen enthält, klicken Sie dann der Bezeichner bezieht sich auf diesen Namespace und wird als Namespace klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-278">Otherwise, if no type argument list was supplied and the namespace contains a namespace member with the given name, then the identifier refers to that namespace and is classified as a namespace.</span></span>
    33. <span data-ttu-id="61fcb-279">Andernfalls, wenn der Namespace eine oder mehrere zugänglich Standardmodulen enthält und eine Suche nach Membern-Name des Bezeichners genau einem Standardmodul eine Übereinstimmung zugegriffen werden kann erzeugt, klicken Sie dann das Ergebnis entspricht genau ein Memberzugriff des Formulars `M.E(Of A)`, in denen `M` ist das standard-Modul, enthält das entsprechende Element `E` ist der Bezeichner, und `A` ist die Liste der Typargumente, sofern vorhanden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-279">Otherwise, if the namespace contains one or more accessible standard modules, and a member name lookup of the identifier produces an accessible match in exactly one standard module, then the result is exactly the same as a member access of the form `M.E(Of A)`, where `M` is the standard module containing the matching member, `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="61fcb-280">Wenn zugänglich Typmember in mehr als ein Standardmodul mit der ID übereinstimmt, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-280">If the identifier matches accessible type members in more than one standard module, a compile-time error occurs.</span></span>

4.  <span data-ttu-id="61fcb-281">Wenn die Quelldatei eine oder mehrere Importaliase hat und der Bezeichner mit dem übereinstimmt, und klicken Sie dann der Bezeichner auf diesen Namespace oder Typ verweist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-281">If the source file has one or more import aliases, and the identifier matches the name of one of them, then the identifier refers to that namespace or type.</span></span> <span data-ttu-id="61fcb-282">Wenn eine Liste der Typargumente angegeben wird, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-282">If a type argument list is supplied, a compile-time error occurs.</span></span>

5. <span data-ttu-id="61fcb-283">Wenn die Quelldatei, die mit dem Namensverweis ein oder mehrere Importe aufweist:</span><span class="sxs-lookup"><span data-stu-id="61fcb-283">If the source file containing the name reference has one or more imports:</span></span>

    51. <span data-ttu-id="61fcb-284">Wenn der Bezeichner in genau einem entspricht importieren den Namen eines Typs zugegriffen werden kann, mit der gleichen Anzahl von Typparametern, wie in der Liste der Typargumente, sofern vorhanden, oder einen Typmember angegeben wurde, und klicken Sie dann der Bezeichner bezieht sich auf diesen Typ oder Typmember.</span><span class="sxs-lookup"><span data-stu-id="61fcb-284">If the identifier matches in exactly one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or a type member, then the identifier refers to that type or type member.</span></span> <span data-ttu-id="61fcb-285">Wenn der Bezeichner in mehr als ein Import mit dem Namen ein zugreifbarer Typ mit der gleichen Anzahl von Typparametern übereinstimmt, wie in der Liste der Typargumente, angegeben wurde, sofern vorhanden, oder ein Typmember zugegriffen werden kann, ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-285">If the identifier matches in more than one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or an accessible type member, a compile-time error occurs.</span></span>
    52. <span data-ttu-id="61fcb-286">Andernfalls verweist, wenn keine Liste der Typargumente angegeben wurde, und dem Bezeichner, der in genau einem Import den Namen eines Namespaces mit verfügbaren Typen übereinstimmt, klicken Sie dann der Bezeichner zu diesem Namespace.</span><span class="sxs-lookup"><span data-stu-id="61fcb-286">Otherwise, if no type argument list was supplied and the identifier matches in exactly one import the name of a namespace with accessible types, then the identifier refers to that namespace.</span></span> <span data-ttu-id="61fcb-287">Wenn keine Liste der Typargumente angegeben wurde, und dem Bezeichner, der in mehr als einem Import den Namen eines Namespaces mit verfügbaren Typen übereinstimmt, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-287">If no type argument list was supplied and the identifier matches in more than one import the name of a namespace with accessible types, a compile-time error occurs.</span></span>
    53. <span data-ttu-id="61fcb-288">Andernfalls, wenn der Import eine oder mehrere zugänglich Standardmodule enthält und eine Suche nach Membern-Name des Bezeichners genau einem Standardmodul eine Übereinstimmung zugegriffen werden kann erzeugt, klicken Sie dann das Ergebnis entspricht genau ein Memberzugriff des Formulars `M.E(Of A)`, wobei `M` ist das standard-Modul, enthält das entsprechende Element `E` ist der Bezeichner, und `A` ist die Liste der Typargumente, sofern vorhanden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-288">Otherwise, if the imports contain one or more accessible standard modules, and a member name lookup of the identifier produces an accessible match in exactly one standard module, then the result is exactly the same as a member access of the form `M.E(Of A)`, where `M` is the standard module containing the matching member, `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="61fcb-289">Wenn zugänglich Typmember in mehr als ein Standardmodul mit der ID übereinstimmt, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-289">If the identifier matches accessible type members in more than one standard module, a compile-time error occurs.</span></span>

6.  <span data-ttu-id="61fcb-290">Wenn der kompilierungsumgebung eine oder mehrere Importaliase definiert und der Bezeichner mit dem übereinstimmt, und klicken Sie dann der Bezeichner auf diesen Namespace oder Typ verweist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-290">If the compilation environment defines one or more import aliases, and the identifier matches the name of one of them, then the identifier refers to that namespace or type.</span></span> <span data-ttu-id="61fcb-291">Wenn eine Liste der Typargumente angegeben wird, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-291">If a type argument list is supplied, a compile-time error occurs.</span></span>

7. <span data-ttu-id="61fcb-292">Wenn der kompilierungsumgebung ein oder mehrere Importe definiert:</span><span class="sxs-lookup"><span data-stu-id="61fcb-292">If the compilation environment defines one or more imports:</span></span>

    71. <span data-ttu-id="61fcb-293">Wenn der Bezeichner in genau einem entspricht importieren den Namen eines Typs zugegriffen werden kann, mit der gleichen Anzahl von Typparametern, wie in der Liste der Typargumente, sofern vorhanden, oder einen Typmember angegeben wurde, und klicken Sie dann der Bezeichner bezieht sich auf diesen Typ oder Typmember.</span><span class="sxs-lookup"><span data-stu-id="61fcb-293">If the identifier matches in exactly one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or a type member, then the identifier refers to that type or type member.</span></span> <span data-ttu-id="61fcb-294">Wenn der Bezeichner in mehr als ein Import mit dem Namen übereinstimmt ein zugreifbarer Typ mit der gleichen Anzahl von Typparametern, wie in der Liste der Typargumente, die ggf. bereitgestellt wurde oder ein Typmember, ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-294">If the identifier matches in more than one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or a type member, a compile-time error occurs.</span></span>
    72. <span data-ttu-id="61fcb-295">Andernfalls verweist, wenn keine Liste der Typargumente angegeben wurde, und dem Bezeichner, der in genau einem Import den Namen eines Namespaces mit verfügbaren Typen übereinstimmt, klicken Sie dann der Bezeichner zu diesem Namespace.</span><span class="sxs-lookup"><span data-stu-id="61fcb-295">Otherwise, if no type argument list was supplied and the identifier matches in exactly one import the name of a namespace with accessible types, then the identifier refers to that namespace.</span></span> <span data-ttu-id="61fcb-296">Wenn keine Liste der Typargumente angegeben wurde, und dem Bezeichner, der in mehr als einem Import den Namen eines Namespaces mit verfügbaren Typen übereinstimmt, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-296">If no type argument list was supplied and the identifier matches in more than one import the name of a namespace with accessible types, a compile-time error occurs.</span></span>
    73. <span data-ttu-id="61fcb-297">Andernfalls, wenn der Import eine oder mehrere zugänglich Standardmodule enthält und eine Suche nach Membern-Name des Bezeichners genau einem Standardmodul eine Übereinstimmung zugegriffen werden kann erzeugt, klicken Sie dann das Ergebnis entspricht genau ein Memberzugriff des Formulars `M.E(Of A)`, wobei `M` ist das standard-Modul, enthält das entsprechende Element `E` ist der Bezeichner, und `A` ist die Liste der Typargumente, sofern vorhanden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-297">Otherwise, if the imports contain one or more accessible standard modules, and a member name lookup of the identifier produces an accessible match in exactly one standard module, then the result is exactly the same as a member access of the form `M.E(Of A)`, where `M` is the standard module containing the matching member,  `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="61fcb-298">Wenn zugänglich Typmember in mehr als ein Standardmodul mit der ID übereinstimmt, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-298">If the identifier matches accessible type members in more than one standard module, a compile-time error occurs.</span></span>

8. <span data-ttu-id="61fcb-299">Andernfalls ist die vom Bezeichner angegebene Name nicht definiert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-299">Otherwise, the name given by the identifier is undefined.</span></span>

<span data-ttu-id="61fcb-300">Ein einfacher Name-Ausdruck, der nicht definiert ist, ist ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-300">A simple name expression that is undefined is a compile-time error.</span></span>

<span data-ttu-id="61fcb-301">Normalerweise kann ein Namen in einem bestimmten Namespace nur einmal vorkommen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-301">Normally, a name can only occur once in a particular namespace.</span></span> <span data-ttu-id="61fcb-302">Da Namespaces mehreren .NET Assemblys deklariert werden können, ist es jedoch möglich, dass eine Situation, in denen zwei Assemblys für einen Typ mit den gleichen vollqualifizierten Namen definieren.</span><span class="sxs-lookup"><span data-stu-id="61fcb-302">However, because namespaces can be declared across multiple .NET assemblies, it is possible to have a situation where two assemblies define a type with the same fully qualified name.</span></span> <span data-ttu-id="61fcb-303">In diesem Fall wird ein Typ, der deklariert, die in den aktuellen Satz von Quelldateien über einem Typ deklariert, die in einer externen .NET-Assembly bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-303">In that case, a type declared in the current set of source files is preferred over a type declared in an external .NET assembly.</span></span> <span data-ttu-id="61fcb-304">Andernfalls der Name ist mehrdeutig, und es gibt keine Möglichkeit, um den Namen zu unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-304">Otherwise, the name is ambiguous and there is no way to disambiguate the name.</span></span>


### <a name="addressof-expressions"></a><span data-ttu-id="61fcb-305">AddressOf-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-305">AddressOf Expressions</span></span>

<span data-ttu-id="61fcb-306">Ein `AddressOf` Ausdruck wird verwendet, um einen Methodenzeiger zu erzeugen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-306">An `AddressOf` expression is used to produce a method pointer.</span></span> <span data-ttu-id="61fcb-307">Der Ausdruck besteht aus den `AddressOf` -Schlüsselwort und ein Ausdruck, der als einer Methodengruppe oder einer spät gebundener Zugriff klassifiziert werden muss.</span><span class="sxs-lookup"><span data-stu-id="61fcb-307">The expression consists of the `AddressOf` keyword and an expression that must be classified as a method group or a late-bound access.</span></span> <span data-ttu-id="61fcb-308">Die Methodengruppe kann nicht an die Konstruktoren verweisen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-308">The method group cannot refer to constructors.</span></span>

<span data-ttu-id="61fcb-309">Das Ergebnis wird als Methodenzeiger, mit dem gleichen zugeordneten Zielausdruck und die Liste der Typargumente (sofern vorhanden) als die Methodengruppe klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-309">The result is classified as a method pointer, with the same associated target expression and type argument list (if any) as the method group.</span></span>

```antlr
AddressOfExpression
    : 'AddressOf' Expression
    ;
```

## <a name="type-expressions"></a><span data-ttu-id="61fcb-310">Typenausdruck</span><span class="sxs-lookup"><span data-stu-id="61fcb-310">Type Expressions</span></span>

<span data-ttu-id="61fcb-311">Ein *geben Ausdruck* ist eine `GetType` Ausdruck eine `TypeOf...Is` Ausdruck eine `Is` Ausdruck oder ein `GetXmlNamespace` Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="61fcb-311">A *type expression* is a `GetType` expression, a `TypeOf...Is` expression, an `Is` expression, or a `GetXmlNamespace` expression.</span></span>

```antlr
TypeExpression
    : GetTypeExpression
    | TypeOfIsExpression
    | IsExpression
    | GetXmlNamespaceExpression
    ;
```

### <a name="gettype-expressions"></a><span data-ttu-id="61fcb-312">GetType-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-312">GetType Expressions</span></span>

<span data-ttu-id="61fcb-313">Ein `GetType` Ausdruck besteht aus dem Schlüsselwort `GetType` und den Namen eines Typs.</span><span class="sxs-lookup"><span data-stu-id="61fcb-313">A `GetType` expression consists of the keyword `GetType` and the name of a type.</span></span>

```antlr
GetTypeExpression
    : 'GetType' OpenParenthesis GetTypeTypeName CloseParenthesis
    ;

GetTypeTypeName
    : TypeName
    | QualifiedOpenTypeName
    ;

QualifiedOpenTypeName
    : Identifier TypeArityList? (Period IdentifierOrKeyword TypeArityList?)*
    | 'Global' Period IdentifierOrKeyword TypeArityList?
      (Period IdentifierOrKeyword TypeArityList?)*
    ;

TypeArityList
    : OpenParenthesis 'Of' CommaList? CloseParenthesis
    ;

CommaList
    : Comma Comma*
    ;
```

<span data-ttu-id="61fcb-314">Ein `GetType` Ausdruck wird als Wert klassifiziert, und sein Wert ist die Reflektion (`System.Type`)-Klasse, die stellt seine *GetTypeTypeName*.</span><span class="sxs-lookup"><span data-stu-id="61fcb-314">A `GetType` expression is classified as a value, and its value is the reflection (`System.Type`) class that represents its *GetTypeTypeName*.</span></span> <span data-ttu-id="61fcb-315">Wenn die *GetTypeTypeName* Typparameter ist ein, wird der Ausdruck zurückgeben, die `System.Type` Objekt, das das Typargument für den Parameter zur Laufzeit entspricht.</span><span class="sxs-lookup"><span data-stu-id="61fcb-315">If the *GetTypeTypeName* is a type parameter, the expression will return the `System.Type` object that corresponds to the type argument supplied for the type parameter at run-time.</span></span>

<span data-ttu-id="61fcb-316">Die *GetTypeTypeName* ist etwas Besonderes, gibt es zwei Möglichkeiten:</span><span class="sxs-lookup"><span data-stu-id="61fcb-316">The *GetTypeTypeName* is special in two ways:</span></span>

* <span data-ttu-id="61fcb-317">Es ist zulässig, werden `System.Void`, der einzige Ort in der Sprache, in dem dieser Typname verwiesen werden kann kann.</span><span class="sxs-lookup"><span data-stu-id="61fcb-317">It is allowed to be `System.Void`, the only place in the language where this type name may be referenced.</span></span>

* <span data-ttu-id="61fcb-318">Es kann es sich um einen konstruierten generischen Typ mit den Typargumenten ausgelassen sein.</span><span class="sxs-lookup"><span data-stu-id="61fcb-318">It may be a constructed generic type with the type arguments omitted.</span></span> <span data-ttu-id="61fcb-319">Dadurch wird die `GetType` zurückzugebende Ausdruck die `System.Type` Objekt, das den generischen Typ selbst entspricht.</span><span class="sxs-lookup"><span data-stu-id="61fcb-319">This allows the `GetType` expression to return the `System.Type` object that corresponds to the generic type itself.</span></span>

<span data-ttu-id="61fcb-320">Das folgende Beispiel zeigt die `GetType` Ausdruck:</span><span class="sxs-lookup"><span data-stu-id="61fcb-320">The following example demonstrates the `GetType` expression:</span></span>

```vb
Module Test
    Sub Main()
        Dim t As Type() = { GetType(Integer), GetType(System.Int32), _
            GetType(String), GetType(Double()) }
        Dim i As Integer

        For i = 0 To t.Length - 1
            Console.WriteLine(t(i).Name)
        Next i
    End Sub
End Module
```

<span data-ttu-id="61fcb-321">Die entstandene Ausgabe ist:</span><span class="sxs-lookup"><span data-stu-id="61fcb-321">The resulting output is:</span></span>

```
Int32
Int32
String
Double[]
```


### <a name="typeofis-expressions"></a><span data-ttu-id="61fcb-322">TypeOf... Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-322">TypeOf...Is Expressions</span></span>

<span data-ttu-id="61fcb-323">Ein `TypeOf...Is` Ausdruck wird verwendet, um zu überprüfen, ob der Laufzeittyp eines Werts mit einem angegebenen Typ kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-323">A `TypeOf...Is` expression is used to check whether the run-time type of a value is compatible with a given type.</span></span> <span data-ttu-id="61fcb-324">Der erste Operand muss als Wert klassifiziert werden, nicht möglich, eine klassifizierter Lambda-Methode und muss ein Verweistyp oder ein Parametertyp beschränkten Typ sein.</span><span class="sxs-lookup"><span data-stu-id="61fcb-324">The first operand must be classified as a value, cannot be a reclassified lambda method, and must be of a reference type or an unconstrained type parameter type.</span></span> <span data-ttu-id="61fcb-325">Der zweite Operand muss ein Typname sein.</span><span class="sxs-lookup"><span data-stu-id="61fcb-325">The second operand must be a type name.</span></span> <span data-ttu-id="61fcb-326">Das Ergebnis des Ausdrucks wird als Wert klassifiziert, und ist eine `Boolean` Wert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-326">The result of the expression is classified as a value and is a `Boolean` value.</span></span> <span data-ttu-id="61fcb-327">Der Ausdruck wird zu `True` verfügt der Laufzeittyp der Operanden eine Identität, Standard, Verweis, Array, Werttyp oder typparameterumwandlung in den Typ `False` andernfalls.</span><span class="sxs-lookup"><span data-stu-id="61fcb-327">The expression evaluates to `True` if the run-time type of the operand has an identity, default, reference, array, value type, or type parameter conversion to the type, `False` otherwise.</span></span> <span data-ttu-id="61fcb-328">Ein Fehler während der Kompilierung tritt auf, wenn keine Konvertierung zwischen den Typ des Ausdrucks und des angegebenen Typs vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-328">A compile-time error occurs if no conversion exists between the type of the expression and the specific type.</span></span>

```antlr
TypeOfIsExpression
    : 'TypeOf' Expression 'Is' LineTerminator? TypeName
    ;
```

### <a name="is-expressions"></a><span data-ttu-id="61fcb-329">Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-329">Is Expressions</span></span>

<span data-ttu-id="61fcb-330">Ein `Is` oder `IsNot` Ausdruck wird verwendet, um einen Verweisgleichheitsvergleich führen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-330">An `Is` or `IsNot` expression is used to do a reference equality comparison.</span></span>

```antlr
IsExpression
    : Expression 'Is' LineTerminator? Expression
    | Expression 'IsNot' LineTerminator? Expression
    ;
```

<span data-ttu-id="61fcb-331">Jeder Ausdruck muss als Wert klassifiziert werden, und der Typ jedes Ausdrucks muss ein Verweistyp, ein Parametertyp beschränkten Typ oder NULL-Werte zulassen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-331">Each expression must be classified as a value and the type of each expression must be a reference type, an unconstrained type parameter type, or a nullable value type.</span></span> <span data-ttu-id="61fcb-332">Der Typ eines Ausdrucks einer beschränkten Typ-Parametertyp oder der Werttyp ist, aber der andere Ausdruck muss, das Literal `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-332">If the type of one expression is an unconstrained type parameter type or nullable value type, however, the other expression must be the literal `Nothing`.</span></span>

<span data-ttu-id="61fcb-333">Das Ergebnis wird als Wert klassifiziert, und wird als eingegeben `Boolean`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-333">The result is classified as a value and is typed as `Boolean`.</span></span> <span data-ttu-id="61fcb-334">Ein `Is` Operation ergibt `True` Wenn beide Werte beziehen sich auf derselben Instanz oder beide Werte sind `Nothing`, oder `False` andernfalls.</span><span class="sxs-lookup"><span data-stu-id="61fcb-334">An `Is` operation evaluates to `True` if both values refer to the same instance or both values are `Nothing`, or `False` otherwise.</span></span> <span data-ttu-id="61fcb-335">Ein `IsNot` Operation ergibt `False` Wenn beide Werte beziehen sich auf derselben Instanz oder beide Werte sind `Nothing`, oder `True` andernfalls.</span><span class="sxs-lookup"><span data-stu-id="61fcb-335">An `IsNot` operation evaluates to `False` if both values refer to the same instance or both values are `Nothing`, or `True` otherwise.</span></span>


### <a name="getxmlnamespace-expressions"></a><span data-ttu-id="61fcb-336">GetXmlNamespace-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-336">GetXmlNamespace Expressions</span></span>

<span data-ttu-id="61fcb-337">Ein `GetXmlNamespace` Ausdruck besteht aus dem Schlüsselwort `GetXmlNamespace` und den Namen eines XML-Namespace deklariert, indem der quellumgebung für Datei oder die Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-337">A `GetXmlNamespace` expression consists of the keyword `GetXmlNamespace` and the name of an XML namespace declared by the source file or compilation environment.</span></span>

```antlr
GetXmlNamespaceExpression
    : 'GetXmlNamespace' OpenParenthesis XMLNamespaceName? CloseParenthesis
    ;
```

<span data-ttu-id="61fcb-338">Ein `GetXmlNamespace` Ausdruck wird als Wert klassifiziert, und sein Wert ist eine Instanz der `System.Xml.Linq.XNamespace` darstellt, die die *XMLNamespaceName*.</span><span class="sxs-lookup"><span data-stu-id="61fcb-338">An `GetXmlNamespace` expression is classified as a value, and its value is an instance of `System.Xml.Linq.XNamespace` that represents the *XMLNamespaceName*.</span></span> <span data-ttu-id="61fcb-339">Wenn Sie diesen Typ nicht verfügbar ist, und klicken Sie dann ein Fehler während der Kompilierung erfolgt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-339">If that type is not available, then a compile-time error will occur.</span></span>

<span data-ttu-id="61fcb-340">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-340">For example:</span></span>

```vb
Imports <xmlns:db="http://example.org/database">

Module Test
    Sub Main()
        Dim db = GetXmlNamespace(db)

        ' The following are equivalent
        Dim customer1 = _
            New System.Xml.Linq.XElement(db + "customer", "Bob")
        Dim customer2 = <db:customer>Bob</>
    End Sub
End Module
```

<span data-ttu-id="61fcb-341">Alles, was zwischen den Klammern wird als Teil der den Namespacenamen, betrachtet, also z. B. Leerzeichen XML-Regeln an.</span><span class="sxs-lookup"><span data-stu-id="61fcb-341">Everything between the parentheses is considered part of the namespace name, so XML rules around things such as whitespace apply.</span></span> <span data-ttu-id="61fcb-342">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-342">For example:</span></span>

```vb
Imports <xmlns:db-ns="http://example.org/database">

Module Test
    Sub Main()

        ' Error, XML name expected
        Dim db1 = GetXmlNamespace( db-ns )

        ' Error, ')' expected
        Dim db2 = GetXmlNamespace(db _
            )

        ' OK.
        Dim db3 = GetXmlNamespace(db-ns)
    End Sub
End Module
```

<span data-ttu-id="61fcb-343">Der XML-Namespace-Ausdruck kann in diesem Fall gibt der Ausdruck für das Objekt, das von der XML-Standardnamespace, auch weggelassen werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-343">The XML namespace expression can also be omitted, in which case the expression returns the object that represents the default XML namespace.</span></span>


## <a name="member-access-expressions"></a><span data-ttu-id="61fcb-344">Memberzugriffsausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-344">Member Access Expressions</span></span>

<span data-ttu-id="61fcb-345">Ein Memberzugriffsausdruck wird verwendet, Zugriff auf einen Member einer Entität.</span><span class="sxs-lookup"><span data-stu-id="61fcb-345">A member access expression is used to access a member of an entity.</span></span>

```antlr
MemberAccessExpression
    : MemberAccessBase? Period IdentifierOrKeyword
      ( OpenParenthesis 'Of' TypeArgumentList CloseParenthesis )?
    ;

MemberAccessBase
    : Expression
    | NonArrayTypeName
    | 'Global'
    | 'MyClass'
    | 'MyBase'
    ;
```

<span data-ttu-id="61fcb-346">Ein Memberzugriff des Formulars `E.I(Of A)`, wobei `E` ist ein Ausdruck, einen Typnamen mit nicht-Array, das Schlüsselwort `Global`, oder nicht angegeben und `I` ist ein Bezeichner mit einer optionalen Liste der Typargumente `A`, ausgewertet und klassifiziert wie folgt:</span><span class="sxs-lookup"><span data-stu-id="61fcb-346">A member access of the form `E.I(Of A)`, where `E` is an expression, a non-array type name, the keyword `Global`, or omitted and `I` is an identifier with an optional type argument list `A`, is evaluated and classified as follows:</span></span>

1. <span data-ttu-id="61fcb-347">Wenn `E` weggelassen wird, klicken Sie dann den Ausdruck aus, die direkt mit `With` Anweisung wird durch ersetzt `E` und der Memberzugriff ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-347">If `E` is omitted, then the expression from the immediately containing `With` statement is substituted for `E` and the member access is performed.</span></span> <span data-ttu-id="61fcb-348">Wenn es keine enthält ist `With` -Anweisung ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-348">If there is no containing `With` statement, a compile-time error occurs.</span></span>

2. <span data-ttu-id="61fcb-349">Wenn `E` wird als Namespace klassifiziert oder `E` ist das Schlüsselwort `Global`, und klicken Sie dann die Membersuche im Kontext des angegebenen Namespace ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-349">If `E` is classified as a namespace or `E` is the keyword `Global`, then the member lookup is done in the context of the specified namespace.</span></span> <span data-ttu-id="61fcb-350">Wenn `I` ist der Name des einen verfügbaren Member dieses Namespace mit der gleichen Anzahl von Typparametern, wie in der Liste der Typargumente, sofern vorhanden, angegeben wurde, lautet das Ergebnis dieses Members.</span><span class="sxs-lookup"><span data-stu-id="61fcb-350">If `I` is the name of an accessible member of that namespace with the same number of type parameters as was supplied in the type argument list, if any, then the result is that member.</span></span> <span data-ttu-id="61fcb-351">Das Ergebnis wird als ein Namespace oder Typ abhängig von der Members klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-351">The result is classified as a namespace or a type depending on the member.</span></span> <span data-ttu-id="61fcb-352">Andernfalls tritt ein Kompilierungsfehler auf.</span><span class="sxs-lookup"><span data-stu-id="61fcb-352">Otherwise, a compile-time error occurs.</span></span>

3. <span data-ttu-id="61fcb-353">Wenn `E` ist ein Typ oder ein Ausdruck, der als Typ klassifiziert, und klicken Sie dann die Membersuche im Kontext des angegebenen Typs ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-353">If `E` is a type or an expression classified as a type, then the member lookup is done in the context of the specified type.</span></span> <span data-ttu-id="61fcb-354">Wenn `I` ist der Name eines Mitglieds zugegriffen werden kann, der `E`, klicken Sie dann `E.I` ausgewertet und wie folgt klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="61fcb-354">If `I` is the name of an accessible member of `E`, then `E.I` is evaluated and classified as follows:</span></span>

    31. <span data-ttu-id="61fcb-355">Wenn `I` ist das Schlüsselwort `New` und `E` ist keine Enumeration dar, und klicken Sie dann ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-355">If `I` is the keyword `New` and `E` is not an enumeration then a compile-time error occurs.</span></span>
    32. <span data-ttu-id="61fcb-356">Wenn `I` identifiziert einen Typ mit der gleichen Anzahl von Typparametern, wie in der Liste der Typargumente, sofern vorhanden, angegeben wurde, lautet das Ergebnis dieses Typs.</span><span class="sxs-lookup"><span data-stu-id="61fcb-356">If `I` identifies a type with the same number of type parameters as was supplied in the type argument list, if any, then the result is that type.</span></span>
    33. <span data-ttu-id="61fcb-357">Wenn `I` eine oder mehrere Methoden, identifiziert, und klicken Sie dann das Ergebnis einer Methodengruppe mit der Argumentliste für den zugeordneten Typ und kein Ausdruck für die zielbereitstellungsumgebung ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-357">If `I` identifies one or more methods, then the result is a method group with the associated type argument list and no associated target expression.</span></span>
    34. <span data-ttu-id="61fcb-358">Wenn `I` identifiziert eine oder mehrere Eigenschaften und keine Argumentliste bereitgestellt wurde, und klicken Sie dann das Ergebnis ist eine Eigenschaftengruppe ohne zielbereitstellungsumgebung-Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="61fcb-358">If `I` identifies one or more properties and no type argument list was supplied, then the result is a property group with no associated target expression.</span></span>
    35. <span data-ttu-id="61fcb-359">Wenn `I` identifiziert eine freigegebene Variable und keine Argumentliste bereitgestellt wurde, und klicken Sie dann das Ergebnis ist eine Variable oder einen Wert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-359">If `I` identifies a shared variable and no type argument list was supplied, then the result is either a variable or a value.</span></span> <span data-ttu-id="61fcb-360">Wenn die Variable schreibgeschützt ist, ist der Verweis außerhalb des gemeinsam genutzte Konstruktors des Typs erfolgt, in dem die Variable wird deklariert, und das Ergebnis der Wert, der die freigegebene Variable ist `I` in `E`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-360">If the variable is read-only, and the reference occurs outside the shared constructor of the type in which the variable is declared, then the result is the value of the shared variable `I` in `E`.</span></span> <span data-ttu-id="61fcb-361">Das Ergebnis ist, andernfalls die freigegebene Variable `I` in `E`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-361">Otherwise, the result is the shared variable `I` in `E`.</span></span>
    36. <span data-ttu-id="61fcb-362">Wenn `I` identifiziert eines freigegebenen Ereignisses und keine Argumentliste bereitgestellt wurde, das Ergebnis ist ein Ereigniszugriff ohne zielbereitstellungsumgebung-Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="61fcb-362">If `I` identifies a shared event and no type argument list was supplied, the result is an event access with no associated target expression.</span></span>
    37. <span data-ttu-id="61fcb-363">Wenn `I` identifiziert eine Konstante und keine Argumentliste bereitgestellt wurde, und klicken Sie dann das Ergebnis ist der Wert dieser Konstanten.</span><span class="sxs-lookup"><span data-stu-id="61fcb-363">If `I` identifies a constant and no type argument list was supplied, then the result is the value of that constant.</span></span>
    38. <span data-ttu-id="61fcb-364">Wenn `I` identifiziert einen Enumerationsmember und keine Argumentliste bereitgestellt wurde, und klicken Sie dann das Ergebnis ist der Wert dieses Elements der Enumeration.</span><span class="sxs-lookup"><span data-stu-id="61fcb-364">If `I` identifies an enumeration member and no type argument list was supplied, then the result is the value of that enumeration member.</span></span>
    39. <span data-ttu-id="61fcb-365">Andernfalls `E.I` ist ein ungültiger Memberverweis, und ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-365">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>

4. <span data-ttu-id="61fcb-366">Wenn `E` wird als eine Variable oder ein-Wert, dessen Typ von klassifiziert `T`, und klicken Sie dann die Membersuche im Kontext des erfolgt `T`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-366">If `E` is classified as a variable or value, the type of which is `T`, then the member lookup is done in the context of `T`.</span></span> <span data-ttu-id="61fcb-367">Wenn `I` ist der Name eines Mitglieds zugegriffen werden kann, der `T`, klicken Sie dann `E.I` ausgewertet und wie folgt klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="61fcb-367">If `I` is the name of an accessible member of `T`, then `E.I` is evaluated and classified as follows:</span></span>

    41. <span data-ttu-id="61fcb-368">Wenn `I` ist das Schlüsselwort `New`, `E` ist `Me`, `MyBase`, oder `MyClass`, es wurden keine Typargumente bereitgestellt, und das Ergebnis ist eine Methodengruppe, die die Instanzkonstruktoren der den Typ des darstellt`E`mit einer zugeordneten Zielausdruck `E` und keine Liste der Typargumente.</span><span class="sxs-lookup"><span data-stu-id="61fcb-368">If `I` is the keyword `New`, `E` is  `Me`, `MyBase`, or `MyClass`, and no type arguments were supplied, then the result is a method group representing the instance constructors of the type of `E` with an associated target expression of `E` and no type argument list.</span></span> <span data-ttu-id="61fcb-369">Andernfalls tritt ein Kompilierungsfehler auf.</span><span class="sxs-lookup"><span data-stu-id="61fcb-369">Otherwise, a compile-time error occurs.</span></span>
    42. <span data-ttu-id="61fcb-370">Wenn `I` identifiziert eine oder mehrere Methoden, einschließlich Erweiterungsmethoden, wenn `T` nicht `Object`, lautet das Ergebnis einer Methodengruppe mit der Argumentliste für den zugeordneten Typ und eine zugeordnete Zielausdruck `E`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-370">If `I` identifies one or more methods, including extension methods if `T` is not `Object`, then the result is a method group with the associated type argument list and an associated target expression of `E`.</span></span>
    43. <span data-ttu-id="61fcb-371">Wenn `I` eine oder mehrere Eigenschaften und keine Argumente bereitgestellt wurden, Typ identifiziert werden, lautet das Ergebnis eine Eigenschaftengruppe mit einer zugeordneten Zielausdruck `E`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-371">If `I` identifies one or more properties and no type arguments were supplied, then the result is a property group with an associated target expression of `E`.</span></span>
    44. <span data-ttu-id="61fcb-372">Wenn `I` identifiziert, eine freigegebene Variable oder eine Instanzvariable und keinen Typ Argumente bereitgestellt wurden, und klicken Sie dann das Ergebnis entweder eine Variable oder ein Wert ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-372">If `I` identifies a shared variable or an instance variable and no type arguments were supplied, then the result is either a variable or a value.</span></span> <span data-ttu-id="61fcb-373">Wenn die Variable schreibgeschützt ist, ist der Verweis findet einen Konstruktor der Klasse in der Variablen für die Art der Variablen ("shared" oder "Instanz") deklariert wird, und das Ergebnis der Wert der Variablen ist `I` in das Objekt, das auf die verwiesen wird durch `E`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-373">If the variable is read-only, and the reference occurs outside a constructor of the class in which the variable is declared appropriate for the kind of variable (shared or instance), then the result is the value of the variable `I` in the object referenced by `E`.</span></span> <span data-ttu-id="61fcb-374">Wenn `T` ein Verweistyp ist, lautet das Ergebnis der Variablen `I` in das Objekt, das auf `E`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-374">If `T` is a reference type, then the result is the variable `I` in the object referenced by `E`.</span></span> <span data-ttu-id="61fcb-375">Andernfalls gilt: Wenn `T` ist ein Werttyp und der Ausdruck `E` als Variable klassifiziert, wird das Ergebnis einer Variablen; andernfalls ist das Ergebnis ein Wert ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-375">Otherwise, if `T` is a value type and the expression `E` is classified as a variable, the result is a variable; otherwise the result is a value.</span></span>
    45. <span data-ttu-id="61fcb-376">Wenn `I` bezeichnet ein Ereignis und keine Argumente wurden bereitgestellt, die das Ergebnis ist ein Ereigniszugriff mit einer zugeordneten Zielausdruck `E`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-376">If `I` identifies an event and no type arguments were supplied, the result is an event access with an associated target expression of `E`.</span></span>
    46. <span data-ttu-id="61fcb-377">Wenn `I` identifiziert eine Konstante und keine Argumente bereitgestellt wurden, Typ, lautet das Ergebnis den Wert dieser Konstanten.</span><span class="sxs-lookup"><span data-stu-id="61fcb-377">If `I` identifies a constant and no type arguments were supplied, then the result is the value of that constant.</span></span>
    47. <span data-ttu-id="61fcb-378">Wenn `I` identifiziert einen Enumerationsmember und keine Argumente bereitgestellt wurden, Typ, lautet das Ergebnis den Wert dieses Elements der Enumeration.</span><span class="sxs-lookup"><span data-stu-id="61fcb-378">If `I` identifies an enumeration member and no type arguments were supplied, then the result is the value of that enumeration member.</span></span>
    48. <span data-ttu-id="61fcb-379">Wenn `T` ist `Object`, lautet das Ergebnis einer spät gebundenen Elementsuche als ein spät gebundener Zugriff mit der Argumentliste für den zugeordneten Typ und eine zugeordnete Zielausdruck klassifiziert `E`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-379">If `T` is `Object`, then the result is a late-bound member lookup classified as a late-bound access with the associated type argument list and an associated target expression of `E`.</span></span>

5. <span data-ttu-id="61fcb-380">Andernfalls `E.I` ist ein ungültiger Memberverweis, und ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-380">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>

<span data-ttu-id="61fcb-381">Ein Memberzugriff des Formulars `MyClass.I(Of A)` entspricht `Me.I(Of A)`, aber alle Elemente, die darauf zugegriffen werden so behandelt, als ob die Elemente nicht mehr überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-381">A member access of the form `MyClass.I(Of A)` is equivalent to `Me.I(Of A)`, but all members accessed on it are treated as if the members are non-overridable.</span></span> <span data-ttu-id="61fcb-382">Das Element, das zugegriffen wird daher nicht von der Laufzeittyp des Werts beeinflusst werden auf dem das Element zugegriffen wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-382">Thus, the member accessed will not be affected by the run-time type of the value on which the member is being accessed.</span></span>

<span data-ttu-id="61fcb-383">Ein Memberzugriff des Formulars `MyBase.I(Of A)` entspricht `CType(Me, T).I(Of A)` , in denen `T` ist der direkte Basistyp des Typs mit dem Ausdruck für den Elementzugriff.</span><span class="sxs-lookup"><span data-stu-id="61fcb-383">A member access of the form `MyBase.I(Of A)` is equivalent to `CType(Me, T).I(Of A)` where `T` is the direct base type of the type containing the member access expression.</span></span> <span data-ttu-id="61fcb-384">Alle zugehörigen Methodenaufrufe werden behandelt, als ob die aufgerufene Methode nicht überschreibbaren ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-384">All method invocations on it are treated as if the method being invoked is non-overridable.</span></span> <span data-ttu-id="61fcb-385">Diese Form der Memberzugriff ist die Abkürzung eine *base Access*.</span><span class="sxs-lookup"><span data-stu-id="61fcb-385">This form of member access is also called a *base access*.</span></span>

<span data-ttu-id="61fcb-386">Im folgende Beispiel wird veranschaulicht, wie `Me`, `MyBase` und `MyClass` beziehen:</span><span class="sxs-lookup"><span data-stu-id="61fcb-386">The following example demonstrates how `Me`, `MyBase` and `MyClass` relate:</span></span>

```vb
Class Base
    Public Overridable Sub F()
        Console.WriteLine("Base.F")
    End Sub
End Class

Class Derived
    Inherits Base

    Public Overrides Sub F()
        Console.WriteLine("Derived.F")
    End Sub

    Public Sub G()
        MyClass.F()
    End Sub
End Class

Class MoreDerived
    Inherits Derived

    Public Overrides Sub F()
        Console.WriteLine("MoreDerived.F")
    End Sub

    Public Sub H()
        MyBase.F()
    End Sub
End Class

Module Test
    Sub Main()
        Dim x As MoreDerived = new MoreDerived()

        x.F()
        x.G()
        x.H()
    End Sub

End Module
```

<span data-ttu-id="61fcb-387">Dieser Code gibt:</span><span class="sxs-lookup"><span data-stu-id="61fcb-387">This code prints out:</span></span>

```
MoreDerived.F
Derived.F
Derived.F
```

<span data-ttu-id="61fcb-388">Wenn ein Memberzugriffsausdruck beginnt mit dem Schlüsselwort `Global`, das Schlüsselwort darstellt, der äußersten unbenannten Namespace, der die eignet sich für Situationen, in denen eine Deklaration führt Shadowing für eine einschließende Namespace.</span><span class="sxs-lookup"><span data-stu-id="61fcb-388">When a member access expression begins with the keyword `Global`, the keyword represents the outermost unnamed namespace, which is useful in situations where a declaration shadows an enclosing namespace.</span></span> <span data-ttu-id="61fcb-389">Die `Global` Schlüsselwort, "Schutz" auf den äußeren Namespace in dieser Situation kann.</span><span class="sxs-lookup"><span data-stu-id="61fcb-389">The `Global` keyword allows "escaping" out to the outermost namespace in that situation.</span></span> <span data-ttu-id="61fcb-390">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-390">For example:</span></span>

```vb
Class System
End Class

Module Test
    Sub Main()
        ' Error: Class System does not contain Console
        System.Console.WriteLine("Hello, world!") 


        ' Legal, binds to System in outermost namespace
        Global.System.Console.WriteLine("Hello, world!") 
    End Sub
End Module
```

<span data-ttu-id="61fcb-391">Im obigen Beispiel ist der erste Methodenaufruf ungültig da der Bezeichner `System` auf die Klasse bindet `System`, nicht den Namespace `System`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-391">In the above example, the first method call is invalid because the identifier `System` binds to the class `System`, not the namespace `System`.</span></span> <span data-ttu-id="61fcb-392">Die einzige Möglichkeit zum Zugriff auf die `System` Namespace ist die Verwendung `Global` , dem äußeren Namespace mit Escapezeichen versehen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-392">The only way to access the `System` namespace is to use `Global` to escape out to the outermost namespace.</span></span>

<span data-ttu-id="61fcb-393">Wenn das Element, auf die zugegriffen wird, freigegeben ist, wird jeder Ausdruck auf der linken Seite des Zeitraums ist überflüssig und wird nicht ausgewertet werden, es sei denn, der Mitglied Zugriff erfolgt spät gebunden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-393">If the member being accessed is shared, any expression on the left side of the period is superfluous and is not evaluated unless the member access is done late-bound.</span></span> <span data-ttu-id="61fcb-394">Beachten Sie z. B. folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="61fcb-394">For example, consider the following code:</span></span>

```vb
Class C
    Public Shared F As Integer = 10
End Class

Module Test
    Public Function ReturnC() As C
        Console.WriteLine("Returning a new instance of C.")
        Return New C()
    End Function

    Public Sub Main()
        Console.WriteLine("The value of F is: " & ReturnC().F)
    End Sub
End Module
```

<span data-ttu-id="61fcb-395">Gibt `The value of F is: 10` da die Funktion `ReturnC` muss nicht aufgerufen werden, um eine Instanz von bieten `C` Zugriff auf den freigegebenen Member `F`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-395">It prints `The value of F is: 10` because the function `ReturnC` does not need to be called to provide an instance of `C` to access the shared member `F`.</span></span>


### <a name="identical-type-and-member-names"></a><span data-ttu-id="61fcb-396">Gleichen Typs und Elementnamen</span><span class="sxs-lookup"><span data-stu-id="61fcb-396">Identical Type and Member Names</span></span>

<span data-ttu-id="61fcb-397">Es ist nicht ungewöhnlich, mit Name-Elemente, die mit dem gleichen Namen wie der Typ.</span><span class="sxs-lookup"><span data-stu-id="61fcb-397">It is not uncommon to name members using the same name as their type.</span></span> <span data-ttu-id="61fcb-398">In diesem Fall kann allerdings unpraktisch Namen auftreten:</span><span class="sxs-lookup"><span data-stu-id="61fcb-398">In that situation, however, inconvenient name hiding can occur:</span></span>

```vb
Enum Color
    Red
    Green
    Yellow
End Enum

Class Test
    ReadOnly Property Color() As Color
        Get
            Return Color.Red
        End Get
    End Property

    Shared Function DefaultColor() As Color
        Return Color.Green    ' Binds to the instance property!
    End Function
End Class
```

<span data-ttu-id="61fcb-399">Im vorherigen Beispiel ist der einfache Name `Color` in `DefaultColor` bindet an die Instanzeigenschaft den Typ.</span><span class="sxs-lookup"><span data-stu-id="61fcb-399">In the previous example, the simple name `Color` in `DefaultColor` binds to the instance property instead of the type.</span></span> <span data-ttu-id="61fcb-400">Da ein Instanzmember in einen freigegebenen Member verwiesen werden kann, würde dies normalerweise ein Fehler sein.</span><span class="sxs-lookup"><span data-stu-id="61fcb-400">Because an instance member cannot be referenced in a shared member, this would normally be an error.</span></span>

<span data-ttu-id="61fcb-401">Allerdings kann eine bestimmte Regel in diesem Fall den Zugriff auf den Typ aus.</span><span class="sxs-lookup"><span data-stu-id="61fcb-401">However, a special rule allows access to the type in this case.</span></span> <span data-ttu-id="61fcb-402">Wenn der Basis eines Memberzugriffsausdrucks Ausdruck ein einfacher Name ist und an eine Konstante, Feld, Eigenschaft, lokale Variable oder Parameter bindet, dessen Typ den gleichen Namen hat, kann die Basisausdruck entweder auf das Element oder den Typ verweisen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-402">If the base expression of a member access expression is a simple name and binds to a constant, field, property, local variable or parameter whose type has the same name, then the base expression can refer either to the member or the type.</span></span> <span data-ttu-id="61fcb-403">Dies kann sich nie in Mehrdeutigkeit führen, da die Elemente, die von einer zugegriffen werden können, die identisch sind.</span><span class="sxs-lookup"><span data-stu-id="61fcb-403">This can never result in ambiguity because the members that can be accessed off of either one are the same.</span></span>

### <a name="default-instances"></a><span data-ttu-id="61fcb-404">Standard-Instanzen</span><span class="sxs-lookup"><span data-stu-id="61fcb-404">Default Instances</span></span>

<span data-ttu-id="61fcb-405">In einigen Fällen Klassen, die von der eine allgemeine Basisklasse in der Regel oder immer nur eine einzige Instanz aufweisen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-405">In some situations, classes derived from a common base class usually or always have only a single instance.</span></span> <span data-ttu-id="61fcb-406">Beispielsweise verfügen die meisten Windows immer nur in einer Benutzeroberfläche gezeigt eine Instanz, die auf dem Bildschirm angezeigt wird, zu einem beliebigen Zeitpunkt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-406">For example, most windows shown in a user interface only ever have one instance showing on the screen at any time.</span></span> <span data-ttu-id="61fcb-407">Zur Vereinfachung der Arbeit mit diesen Typen von Klassen, Visual Basic können automatisch generieren *Standardinstanzen* der Klassen, die eine einzelne Instanz einfach auf die verwiesen wird für jede Klasse bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-407">To simplify working with these types of classes, Visual Basic can automatically generate *default instances* of the classes that provide a single, easily referenced instance for each class.</span></span>

<span data-ttu-id="61fcb-408">Standard-Instanzen werden immer für erstellt eine *Familie* von Typen und nicht für einen bestimmten Typ.</span><span class="sxs-lookup"><span data-stu-id="61fcb-408">Default instances are always created for a *family* of types rather than for one particular type.</span></span> <span data-ttu-id="61fcb-409">Statt eine Standardinstanz einer Klasse Form1, die von Form abgeleitet wird, werden also Standardinstanzen für alle Klassen, die von Form abgeleitet erstellt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-409">So instead of creating a default instance for a class Form1 that derives from Form, default instances are created for all classes derived from Form.</span></span> <span data-ttu-id="61fcb-410">Dies bedeutet, dass jeder einzelnen Klasse, die von der Basisklasse abgeleitet ist keine besonders gekennzeichnet werden, um eine Standardinstanz.</span><span class="sxs-lookup"><span data-stu-id="61fcb-410">This means that each individual class that derives from the base class does not have to be specially marked to have a default instance.</span></span>

<span data-ttu-id="61fcb-411">Durch eine vom Compiler generierte Eigenschaft, die die Standardinstanz dieser Klasse zurückgibt, wird die Standardinstanz einer Klasse dargestellt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-411">The default instance of a class is represented by a compiler-generated property that returns the default instance of that class.</span></span> <span data-ttu-id="61fcb-412">Wird aufgerufen, die Eigenschaft als Member einer Klasse generiert die *Klasse gruppieren* , verwaltet zuordnen und Zerstören von Standardinstanzen für alle Klassen aus der bestimmten Basisklasse abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-412">The property generated as a member of a class called the *group class* that manages allocating and destroying default instances for all classes derived from the particular base class.</span></span> <span data-ttu-id="61fcb-413">Beispielsweise alle Eigenschaften der Instanz der Klassen stammen aus `Form` möglicherweise in den erfassten der `MyForms` Klasse.</span><span class="sxs-lookup"><span data-stu-id="61fcb-413">For example, all of the default instance properties of classes derived from `Form` may be collected in the `MyForms` class.</span></span> <span data-ttu-id="61fcb-414">Wenn eine Instanz der Gruppenklasse, durch den Ausdruck zurückgegeben wird `My.Forms`, und klicken Sie dann der folgende Code die Standardinstanzen von abgeleiteten Klassen greift auf `Form1` und `Form2`:</span><span class="sxs-lookup"><span data-stu-id="61fcb-414">If an instance of the group class is returned by the expression `My.Forms`, then the following code accesses the default instances of derived classes `Form1` and `Form2`:</span></span>

```vb
Class Form1
    Inherits Form
    Public x As Integer
End Class

Class Form2
    Inherits Form
    Public y As Integer
End Class

Module Main
    Sub Main()
        My.Forms.Form1.x = 10
        Console.WriteLine(My.Forms.Form2.y)
    End Sub
End Module
```

<span data-ttu-id="61fcb-415">Standardinstanzen werden bis der erste Verweis auf diese nicht erstellt werden. Abrufen der Eigenschaft für die Standardinstanz bewirkt, dass die Standardinstanz erstellt werden, wenn er noch nicht erstellt wurde, oder auf festgelegt wurde `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-415">Default instances will not be created until the first reference to them; fetching the property representing the default instance causes the default instance to be created if it has not already been created or has been set to `Nothing`.</span></span> <span data-ttu-id="61fcb-416">Um zuzulassen, testen das Vorhandensein einer Standardinstanz, wenn eine Standardinstanz das Ziel ist eine `Is` oder `IsNot` Operator an mit, die Standardinstanz wird nicht erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-416">To allow testing for the existence of a default instance, when a default instance is the target of an `Is` or `IsNot` operator, the default instance will not be created.</span></span> <span data-ttu-id="61fcb-417">Daher ist es möglich, zu überprüfen, ob eine Standardinstanz ist `Nothing` oder einige andere Verweis, ohne dass die Standardinstanz erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-417">Thus, it is possible to test whether a default instance is `Nothing` or some other reference without causing the default instance to be created.</span></span>

<span data-ttu-id="61fcb-418">Standard-Instanzen zu vereinfachen, um mit der Standardinstanz von außerhalb der Klasse zu verweisen, die die Standardinstanz dienen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-418">Default instances are intended to make it easy to refer to the default instance from outside of the class that has the default instance.</span></span> <span data-ttu-id="61fcb-419">Mit einer Standardinstanz von innerhalb einer Klasse, die es definiert, kann für Verwirrung Sorgen darüber, welche Instanz, z. B. die Standardinstanz oder die aktuelle Instanz gemeint ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-419">Using a default instance from within a class that defines it might cause confusion as to which instance is being referred to, i.e. the default instance or the current instance.</span></span> <span data-ttu-id="61fcb-420">Der folgende Code ändert beispielsweise nur den Wert `x` in der Standardinstanz, auch wenn es aufgerufen wird von einer anderen Instanz.</span><span class="sxs-lookup"><span data-stu-id="61fcb-420">For example, the following code modifies only the value `x` in the default instance, even though it is being called from another instance.</span></span> <span data-ttu-id="61fcb-421">Daher würde der Code den Wert gedruckt `5` anstelle von `10`:</span><span class="sxs-lookup"><span data-stu-id="61fcb-421">Thus the code would print the value `5` instead of `10`:</span></span>

```vb
Class Form1
    Inherits Form

    Public x As Integer = 5

    Public Sub ChangeX()
        Form1.x = 10
    End Sub
End Class

Module Main
    Sub Main()
        Dim f As Form1 = New Form1()
        f.ChangeX()
        Console.WriteLine(f.x)
    End Sub
End Module
```

<span data-ttu-id="61fcb-422">Um diese Art von Verwirrung zu vermeiden, ist es nicht zum Verweisen auf die Standardinstanz des Typs eine Standardinstanz von innerhalb einer Instanzenmethode gültig.</span><span class="sxs-lookup"><span data-stu-id="61fcb-422">To prevent this kind of confusion, it is not valid to refer to a default instance from within an instance method of the default instance's type.</span></span>

#### <a name="default-instances-and-type-names"></a><span data-ttu-id="61fcb-423">Standard-Instanzen und Typnamen</span><span class="sxs-lookup"><span data-stu-id="61fcb-423">Default Instances and Type Names</span></span>

<span data-ttu-id="61fcb-424">Eine Standardinstanz kann auch direkt über den Namen des Typs zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-424">A default instance may also be accessible directly through its type's name.</span></span> <span data-ttu-id="61fcb-425">In diesem Fall in einem beliebigen Ausdruckskontext verwendet, in dem der Name ist den Ausdruck nicht zulässig `E`, wobei `E` steht für den vollqualifizierten Namen der Klasse mit einer Standardinstanz, die geändert wird, um `E'`, wobei `E'` darstellt Ein Ausdruck, der die Standardeigenschaft für die Instanz abruft.</span><span class="sxs-lookup"><span data-stu-id="61fcb-425">In this case, in any expression context where the type name is not allowed the expression `E`, where `E` represents the fully qualified name of the class with a default instance, is changed to `E'`, where `E'` represents an expression that fetches the default instance property.</span></span> <span data-ttu-id="61fcb-426">Z. B. wenn der Standardwert für die Instanzen von abgeleiteten Klassen `Form` können Sie die Standardinstanz über den Namen, und klicken Sie dann der folgende Code den Code im vorherigen Beispiel entspricht:</span><span class="sxs-lookup"><span data-stu-id="61fcb-426">For example, if default instances for classes derived from `Form` allow accessing the default instance through the type name, then the following code is equivalent to the code in the previous example:</span></span>

```vb
Module Main
    Sub Main()
        Form1.x = 10
        Console.WriteLine(Form2.y)
    End Sub
End Module
```

<span data-ttu-id="61fcb-427">Dies bedeutet auch, dass eine Standardinstanz, die über den Namen des Typs zugegriffen werden kann auch über den Namen des Typs zugewiesen werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-427">This also means that a default instance that is accessible through its type's name is also assignable through the type name.</span></span> <span data-ttu-id="61fcb-428">Im folgenden Code wird z. B. die Standardinstanz von `Form1` zu `Nothing`:</span><span class="sxs-lookup"><span data-stu-id="61fcb-428">For example, the following code sets the default instance of `Form1` to `Nothing`:</span></span>

```vb
Module Main
    Sub Main()
        Form1 = Nothing
    End Sub
End Module
```

<span data-ttu-id="61fcb-429">Beachten Sie, dass die Bedeutung der `E.I` wurden `E` stellt eine Klasse und `I` darstellt, die ein freigegebener Member nicht geändert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-429">Note that the meaning of `E.I` were `E` represents a class and `I` represents a shared member does not change.</span></span> <span data-ttu-id="61fcb-430">Ein solcher Ausdruck weiterhin greift auf den freigegebenen Member direkt aus die Instanz der Klasse und nicht auf die Standardinstanz verweist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-430">Such an expression still accesses the shared member directly off of the class instance and does not reference the default instance.</span></span>

#### <a name="group-classes"></a><span data-ttu-id="61fcb-431">Von Gruppenklassen</span><span class="sxs-lookup"><span data-stu-id="61fcb-431">Group Classes</span></span>

<span data-ttu-id="61fcb-432">Die `Microsoft.VisualBasic.MyGroupCollectionAttribute` Attribut gibt an, die Gruppe-Klasse für eine Standard-Instanzen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-432">The `Microsoft.VisualBasic.MyGroupCollectionAttribute` attribute indicates the group class for a family of default instances.</span></span> <span data-ttu-id="61fcb-433">Das Attribut verfügt über vier Parameter:</span><span class="sxs-lookup"><span data-stu-id="61fcb-433">The attribute has four parameters:</span></span>

* <span data-ttu-id="61fcb-434">Der Parameter `TypeToCollect` gibt die Basisklasse für die Gruppe an.</span><span class="sxs-lookup"><span data-stu-id="61fcb-434">The parameter `TypeToCollect` specifies the base class for the group.</span></span> <span data-ttu-id="61fcb-435">Alle instanziierbare Klassen ohne offene Typparameter, die von einem Typ mit diesem Namen (unabhängig vom Typparameter) abgeleitet werden, werden automatisch eine Standardinstanz zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-435">All instantiable classes without open type parameters that derive from a type with this name (regardless of type parameters) will automatically have a default instance.</span></span>

* <span data-ttu-id="61fcb-436">Der Parameter `CreateInstanceMethodName` gibt die Methode aufrufen, in der Gruppenklasse, um eine neue Instanz in eine Standardeigenschaft für die Instanz zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-436">The parameter `CreateInstanceMethodName` specifies the method to call in the group class to create a new instance in a default instance property.</span></span>

* <span data-ttu-id="61fcb-437">Der Parameter `DisposeInstanceMethodName` gibt die Methode aufrufen, in der Gruppenklasse in der eine Standardeigenschaft für die Instanz zu verwerfen, wenn die Standard-Instance-Eigenschaft der Wert zugewiesen wird `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-437">The parameter `DisposeInstanceMethodName` specifies the method to call in the group class to dispose of a default instance property if the default instance property is assigned the value `Nothing`.</span></span>

* <span data-ttu-id="61fcb-438">Der Parameter `DefaultInstanceAlias` Besonderheiten des Ausdrucks `E'` , für den Namen der Klasse zu ersetzen, wenn die Standardinstanzen direkt über ihren Typnamen zugänglich sind.</span><span class="sxs-lookup"><span data-stu-id="61fcb-438">The parameter `DefaultInstanceAlias` specifics the expression `E'` to substitute for the class name if the default instances are accessible directly through their type name.</span></span> <span data-ttu-id="61fcb-439">Wenn dieser Parameter ist `Nothing` oder eine leere Zeichenfolge, Standard-Instanzen auf diesem Gruppentyp nicht direkt über den Namen des Typs zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="61fcb-439">If this parameter is `Nothing` or an empty string, default instances on this group type are not accessible directly through their type's name.</span></span> <span data-ttu-id="61fcb-440">(__Beachten.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-440">(__Note.__</span></span> <span data-ttu-id="61fcb-441">Bei allen aktuellen Implementierungen der Visual Basic-Sprache die `DefaultInstanceAlias` Parameter wird ignoriert, außer im Compiler bereitgestellten Code.)</span><span class="sxs-lookup"><span data-stu-id="61fcb-441">In all current implementations of the Visual Basic language, the `DefaultInstanceAlias` parameter is ignored, except in compiler-provided code.)</span></span>

<span data-ttu-id="61fcb-442">Mehrere Typen können in der gleichen Gruppe erfasst werden, durch die Namen von Typen und Methoden in der ersten drei Parameter dabei mit Kommas trennen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-442">Multiple types can be collected into the same group by separating the names of the types and methods in the first three parameters using commas.</span></span> <span data-ttu-id="61fcb-443">Muss die gleiche Anzahl von Elementen in jeder Parameter vorhanden sein, und die Listenelemente der Reihe nach abgeglichen werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-443">There must be the same number of items in each parameter, and the list elements are matched in order.</span></span> <span data-ttu-id="61fcb-444">Die folgende Attributdeklaration erfasst beispielsweise Typen, die abgeleitet `C1`, `C2` oder `C3` in einer einzelnen Gruppe:</span><span class="sxs-lookup"><span data-stu-id="61fcb-444">For example, the following attribute declaration collects types that derive from `C1`, `C2` or `C3` into a single group:</span></span>

```vb
<Microsoft.VisualBasic.MyGroupCollection("C1, C2, C3", _
    "CreateC1, CreateC2, CreateC3", _
    "DisposeC1, DisposeC2, DisposeC3", "My.Cs")>
Public NotInheritable Class MyCs
    ...
End Class
```

<span data-ttu-id="61fcb-445">Die Signatur der Create-Methode muss im Format `Shared Function <Name>(Of T As {New, <Type>})(Instance Of T) As T`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-445">The signature of the create method must be of the form `Shared Function <Name>(Of T As {New, <Type>})(Instance Of T) As T`.</span></span> <span data-ttu-id="61fcb-446">Die Dispose-Methode muss im Format `Shared Sub <Name>(Of T As <Type>)(ByRef Instance Of T)`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-446">The dispose method must be of the form `Shared Sub <Name>(Of T As <Type>)(ByRef Instance Of T)`.</span></span> <span data-ttu-id="61fcb-447">Daher kann die Gruppe-Klasse für das Beispiel im vorherigen Abschnitt wie folgt deklariert werden:</span><span class="sxs-lookup"><span data-stu-id="61fcb-447">Thus, the group class for the example in the preceding section could be declared as follows:</span></span>

```vb
<Microsoft.VisualBasic.MyGroupCollection("Form", "Create", _
    "Dispose", "My.Forms")> _
Public NotInheritable Class MyForms
    Private Shared Function Create(Of T As {New, Form}) _
        (Instance As T) As T
        If Instance Is Nothing Then
            Return New T()
        Else
            Return Instance
        End If
    End Function

    Private Shared Sub Dispose(Of T As Form)(ByRef Instance As T)
        Instance.Close()
        Instance = Nothing
    End Sub
End Class
```

<span data-ttu-id="61fcb-448">Wenn eine Quelldatei mit eine abgeleitete Klasse deklariert `Form1`, wäre ist die Gruppe der generierten Klasse entspricht:</span><span class="sxs-lookup"><span data-stu-id="61fcb-448">If a source file declared a derived class `Form1`, the generated group class would be equivalent to:</span></span>

```vb
<Microsoft.VisualBasic.MyGroupCollection("Form", "Create", _
    "Dispose", "My.Forms")> _
Public NotInheritable Class MyForms
    Private Shared Function Create(Of T As {New, Form}) _
        (Instance As T) As T
        If Instance Is Nothing Then
            Return New T()
        Else
            Return Instance
        End If
    End Function

    Private Shared Sub Dispose(Of T As Form)(ByRef Instance As T)
        Instance.Close()
        Instance = Nothing
    End Sub

    Private m_Form1 As Form1

    Public Property Form1() As Form1
        Get
            Return Create(m_Form1)
        End Get
        Set (Value As Form1)
            If Value IsNot Nothing AndAlso Value IsNot m_Form1 Then
                Throw New ArgumentException( _
                    "Property can only be set to Nothing.")
            End If
            Dispose(m_Form1)
        End Set
    End Property
End Class
```

### <a name="extension-method-collection"></a><span data-ttu-id="61fcb-449">Erweiterungsauflistung-Methode</span><span class="sxs-lookup"><span data-stu-id="61fcb-449">Extension Method Collection</span></span>

<span data-ttu-id="61fcb-450">Erweiterungsmethoden für das Element zugreifen Ausdruck `E.I` werden gesammelt, indem Sie die Zusammenstellung der Erweiterungsmethoden mit dem Namen `I` , die im aktuellen Kontext verfügbar sind:</span><span class="sxs-lookup"><span data-stu-id="61fcb-450">Extension methods for the member access expression `E.I` are collected by gathering all of the extension methods with the name `I` that are available in the current context:</span></span>

1. <span data-ttu-id="61fcb-451">Zunächst wird jeden geschachtelter Typ mit dem Ausdruck überprüft, beginnend mit der innersten und zum äußersten.</span><span class="sxs-lookup"><span data-stu-id="61fcb-451">First, each nested type containing the expression is checked, starting from the innermost and going to the outermost.</span></span>
2. <span data-ttu-id="61fcb-452">Anschließend wird jeden geschachtelten Namespace überprüft, beginnend mit der innersten und dem äußeren Namespace navigieren.</span><span class="sxs-lookup"><span data-stu-id="61fcb-452">Then, each nested namespace is checked, starting from the innermost and going to the outermost namespace.</span></span>
3. <span data-ttu-id="61fcb-453">Anschließend werden die Importe in der Quelldatei überprüft.</span><span class="sxs-lookup"><span data-stu-id="61fcb-453">Then, the imports in the source file are checked.</span></span>
4. <span data-ttu-id="61fcb-454">Anschließend werden die Importe, die definiert, die von der kompilierungsumgebung überprüft.</span><span class="sxs-lookup"><span data-stu-id="61fcb-454">Then, the imports defined by the compilation environment are checked.</span></span>

<span data-ttu-id="61fcb-455">Nur dann, wenn eine erweiternde native Konvertierung vom Typ Ziel-Ausdrucks in den Typ des ersten Parameters der Erweiterungsmethode ist eine Erweiterungsmethode gesammelt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-455">An extension method is collected only if there is a widening native conversion from the target expression type to the type of the first parameter of the extension method.</span></span> <span data-ttu-id="61fcb-456">Und im Gegensatz zu regulären einfachen Namen ausdrucksbindung, die Suche erfasst *alle* Erweiterungsmethoden, die Auflistung wird nicht beendet, wenn eine Erweiterungsmethode gefunden wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-456">And unlike regular simple name expression binding, the search collects *all* extension methods; the collection does not stop when an extension method is found.</span></span> <span data-ttu-id="61fcb-457">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-457">For example:</span></span>

```vb
Imports System.Runtime.CompilerServices

Class C1
End Class


Namespace N1
    Module N1C1Extensions
        <Extension> _
        Sub M1(c As C1, x As Integer)
        End Sub
    End Module
End Namespace

Namespace N1.N2
    Module N2C1Extensions
        <Extension> _
        Sub M1(c As C1, y As Double)
        End Sub
    End Module
End Namespace

Namespace N1.N2.N3
    Module Test
        Sub Main()
            Dim x As New C1()

            ' Calls N1C1Extensions.M1
            x.M1(10)
        End Sub
    End Module
End Namespace
```

<span data-ttu-id="61fcb-458">In diesem Beispiel, obwohl `N2C1Extensions.M1` gefunden wird, bevor `N1C1Extensions.M1`, beide als Erweiterungsmethoden betrachtet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-458">In this example, even though `N2C1Extensions.M1` is found before `N1C1Extensions.M1`, they both are considered as extension methods.</span></span> <span data-ttu-id="61fcb-459">Sobald alle Erweiterungsmethoden erfasst wurden, sind sie *mit Currying*.</span><span class="sxs-lookup"><span data-stu-id="61fcb-459">Once all of the extension methods have been collected, they are then *curried*.</span></span> <span data-ttu-id="61fcb-460">Currying wird das Ziel der Aufruf der Erweiterungsmethode, und wendet sie auf der Aufruf der Erweiterungsmethode, wodurch die Signatur einer neuen Methode mit dem ersten Parameter entfernt (weil er angegeben wurde).</span><span class="sxs-lookup"><span data-stu-id="61fcb-460">Currying takes the target of the extension method call and applies it to the extension method call, resulting in a new method signature with the first parameter removed (because it has been specified).</span></span> <span data-ttu-id="61fcb-461">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-461">For example:</span></span>

```vb
Imports System.Runtime.CompilerServices

Module Ext1
    <Extension> _
    Sub M(x As Integer, y As Integer)
    End Sub
End Module

Module Ext2
    <Extension> _
    Sub M(x As Integer, y As Double)
    End Sub
End Module

Module Main
    Sub Test()
        Dim v As Integer = 10

        ' The curried method signatures considered are:
        '        Ext1.M(y As Integer)
        '        Ext2.M(y As Double)
        v.M(10)
    End Sub
End Module
```

<span data-ttu-id="61fcb-462">Im obigen Beispiel, das mit Currying Ergebnis des Anwendens `v` zu `Ext1.M` ist die Signatur der Methode `Sub M(y As Integer)`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-462">In the above example, the curried result of applying `v` to `Ext1.M` is the method signature `Sub M(y As Integer)`.</span></span>

<span data-ttu-id="61fcb-463">Zusätzlich zum Entfernen des ersten Parameters der Erweiterungsmethode, entfernt das currying auch Typparameter Methode, die Teil des Typs des ersten Parameters sind.</span><span class="sxs-lookup"><span data-stu-id="61fcb-463">In addition to removing the first parameter of the extension method, currying also removes any method type parameters that are a part of the type of the first parameter.</span></span> <span data-ttu-id="61fcb-464">Wenn eine Erweiterungsmethode mit Methodentypparameter currying, Typrückschluss auf den ersten Parameter angewendet wird, und das Ergebnis ist festgelegt, für alle Typparameter, die abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-464">When currying an extension method with method type parameter, type inference is applied to the first parameter and the result is fixed for any type parameters that are inferred.</span></span> <span data-ttu-id="61fcb-465">Wenn der Typrückschluss schlägt fehl, wird die Methode ignoriert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-465">If type inference fails, the method is ignored.</span></span> <span data-ttu-id="61fcb-466">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-466">For example:</span></span>

```vb
Imports System.Runtime.CompilerServices

Module Ext1
    <Extension> _
    Sub M(Of T, U)(x As T, y As U)
    End Sub
End Module

Module Ext2
    <Extension> _
    Sub M(Of T)(x As T, y As T)
    End Sub
End Module

Module Main
    Sub Test()
        Dim v As Integer = 10

        ' The curried method signatures considered are:
        '        Ext1.M(Of U)(y As U)
        '        Ext2.M(y As Integer)
        v.M(10)
    End Sub
End Module
```

<span data-ttu-id="61fcb-467">Im obigen Beispiel, das mit Currying Ergebnis des Anwendens `v` zu `Ext1.M` ist die Signatur der Methode `Sub M(Of U)(y As U)`, da der Typparameter `T` wird als Ergebnis der currying abgeleitet und ist nun behoben.</span><span class="sxs-lookup"><span data-stu-id="61fcb-467">In the above example, the curried result of applying `v` to `Ext1.M` is the method signature `Sub M(Of U)(y As U)`, because the type parameter `T` is inferred as a result of the currying and is now fixed.</span></span> <span data-ttu-id="61fcb-468">Da der Typparameter `U` wurde nicht per Rückschluss abgeleitet als Teil der currying, bleibt er ein open-Parameter.</span><span class="sxs-lookup"><span data-stu-id="61fcb-468">Because the type parameter `U` was not inferred as a part of the currying, it remains an open parameter.</span></span> <span data-ttu-id="61fcb-469">Auf ähnliche Weise, da der Typparameter `T` wird als Ergebnis der Anwendung abgeleitet `v` zu `Ext2.M`, der Typ des Parameters `y` wird als fester `Integer`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-469">Similarly, because the type parameter `T` is inferred as a result of applying `v` to `Ext2.M`, the type of parameter `y` becomes fixed as `Integer`.</span></span> <span data-ttu-id="61fcb-470">Es wird nicht abgeleitet werden, einen anderen Typ sein.</span><span class="sxs-lookup"><span data-stu-id="61fcb-470">It will not be inferred to be any other type.</span></span> <span data-ttu-id="61fcb-471">Wenn die Signatur, die alle Einschränkungen, mit Ausnahme von currying `New` Einschränkungen werden ebenfalls angewendet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-471">When currying the signature, all constraints except for `New` constraints are also applied.</span></span> <span data-ttu-id="61fcb-472">Wenn die Einschränkungen nicht erfüllt sind, oder Sie richten sich nach der ein Typ, der nicht als Teil der currying abgeleitet wurde, wird die Erweiterungsmethode ignoriert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-472">If the constraints are not satisfied, or depend on a type that was not inferred as a part of currying, the extension method is ignored.</span></span> <span data-ttu-id="61fcb-473">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-473">For example:</span></span>

```vb
Imports System.Runtime.CompilerServices

Module Ext1
    <Extension> _
    Sub M1(Of T As Structure)(x As T, y As Integer)
    End Sub

    <Extension> _
    Sub M2(Of T As U, U)(x As T, y As U)
    End Sub
End Module

Module Main
    Sub Test()
        Dim s As String = "abc"

        ' Error: String does not satisfy the Structure constraint
        s.M1(10)

        ' Error: T depends on U, which cannot be inferred
        s.M2(10)
    End Sub
End Module
```

<span data-ttu-id="61fcb-474">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-474">__Note.__</span></span> <span data-ttu-id="61fcb-475">Einer der Hauptgründe, warum dies von Erweiterungsmethoden currying ist, dass es sich um Abfrageausdrücke zum Ableiten des Typs der Iteration vor der Auswertung der Argumente für eine Abfragemethode-Muster ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="61fcb-475">One of the main reasons for doing currying of extension methods is that it allows query expressions to infer the type of the iteration before evaluating the arguments to a query pattern method.</span></span> <span data-ttu-id="61fcb-476">Da die meisten Abfragemethoden Muster Lambda-Ausdrücken in Anspruch die Typrückschluss selbst erforderlich sind nehmen, vereinfacht dies deutlich das Auswerten eines Abfrageausdrucks.</span><span class="sxs-lookup"><span data-stu-id="61fcb-476">Since most query pattern methods take lambda expressions, which require type inference themselves, this greatly simplifies the process of evaluating a query expression.</span></span>

<span data-ttu-id="61fcb-477">Im Gegensatz zu normalen schnittstellenvererbung sind Erweiterungsmethoden, die zwei Schnittstellen zu erweitern, die nicht miteinander beziehen verfügbar, solange sie nicht die gleiche Curry-Signatur verfügen:</span><span class="sxs-lookup"><span data-stu-id="61fcb-477">Unlike normal interface inheritance, extension methods that extend two interfaces that do not relate to one another are available, as long as they do not have the same curried signature:</span></span>

```vb
Imports System.Runtime.CompilerServices

Interface I1
End Interface

Interface I2
End Interface

Class C1
    Implements I1, I2
End Class

Module I1Ext
    <Extension> _
    Sub M1(i As I1, x As Integer)
    End Sub

    <Extension> _
    Sub M2(i As I1, x As Integer)
    End Sub
End Module

Module I2Ext
    <Extension> _
    Sub M1(i As I2, x As Integer)
    End Sub

    <Extension> _
    Sub M2(I As I2, x As Double)
    End Sub
End Module

Module Main
    Sub Test()
        Dim c As New C1()

        ' Error: M is ambiguous between I1Ext.M1 and I2Ext.M1.
        c.M1(10)

        ' Calls I1Ext.M2
        c.M2(10)
    End Sub
End Module
```

<span data-ttu-id="61fcb-478">Schließlich ist es wichtig, diese Erweiterung ist, denken Sie daran, die Methoden nicht berücksichtigt werden, bei der späten Bindung:</span><span class="sxs-lookup"><span data-stu-id="61fcb-478">Finally, it is important to remember that extension methods are not considered when doing late binding:</span></span>

```vb
Module Test
    Sub Main()
        Dim o As Object = ...

        ' Ignores extension methods
        o.M1()
    End Sub
End Module
```

## <a name="dictionary-member-access-expressions"></a><span data-ttu-id="61fcb-479">Memberzugriffsausdrücken</span><span class="sxs-lookup"><span data-stu-id="61fcb-479">Dictionary Member Access Expressions</span></span>

<span data-ttu-id="61fcb-480">Ein *Wörterbuch Memberzugriffsausdruck* wird verwendet, um ein Mitglied einer Sammlung zu suchen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-480">A *dictionary member access expression* is used to look up a member of a collection.</span></span> <span data-ttu-id="61fcb-481">Eine wörterbuchmemberzugriff nimmt die Form `E!I`, wobei `E` ist ein Ausdruck, der als Wert klassifiziert wird und `I` ist ein Bezeichner.</span><span class="sxs-lookup"><span data-stu-id="61fcb-481">A dictionary member access takes the form of `E!I`, where `E` is an expression that is classified as a value and `I` is an identifier.</span></span>

```antlr
DictionaryAccessExpression
    : Expression? '!' IdentifierOrKeyword
    ;
```

<span data-ttu-id="61fcb-482">Der Typ des Ausdrucks müssen durch eine einzelne indizierte Standardeigenschaft `String` Parameter.</span><span class="sxs-lookup"><span data-stu-id="61fcb-482">The type of the expression must have a default property indexed by a single `String` parameter.</span></span> <span data-ttu-id="61fcb-483">Das Wörterbuch-Memberzugriffsausdruck `E!I` transformiert wird, in dem Ausdruck `E.D("I")`, wobei `D` ist die Standardeigenschaft des `E`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-483">The dictionary member access expression `E!I` is transformed into the expression `E.D("I")`, where `D` is the default property of `E`.</span></span> <span data-ttu-id="61fcb-484">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-484">For example:</span></span>

```vb
Class Keys
    Public ReadOnly Default Property Item(s As String) As Integer
        Get
            Return 10
        End Get
    End Property 
End Class

Module Test
    Sub Main()
        Dim x As Keys = new Keys()
        Dim y As Integer
        ' The two statements are equivalent.
        y = x!abc
        y = x("abc")
    End Sub
End Module
```

<span data-ttu-id="61fcb-485">Wenn kein Ausdruck, der den Ausdruck aus, die direkt mit einem Ausrufezeichen angegeben wird `With` Anweisung wird davon ausgegangen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-485">If an exclamation point is specified with no expression, the expression from the immediately containing `With` statement is assumed.</span></span> <span data-ttu-id="61fcb-486">Wenn es keine enthält ist `With` -Anweisung ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-486">If there is no containing `With` statement, a compile-time error occurs.</span></span>


## <a name="invocation-expressions"></a><span data-ttu-id="61fcb-487">Aufrufausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-487">Invocation Expressions</span></span>

<span data-ttu-id="61fcb-488">Ein Aufrufausdruck besteht aus einem Aufrufziel und einer optionalen Argumentliste.</span><span class="sxs-lookup"><span data-stu-id="61fcb-488">An invocation expression consists of an invocation target and an optional argument list.</span></span>

```antlr
InvocationExpression
    : Expression ( OpenParenthesis ArgumentList? CloseParenthesis )?
    ;

ArgumentList
    : PositionalArgumentList
    | PositionalArgumentList Comma NamedArgumentList
    | NamedArgumentList
    ;

PositionalArgumentList
    : Expression? ( Comma Expression? )*
    ;

NamedArgumentList
    : IdentifierOrKeyword ColonEquals Expression
      ( Comma IdentifierOrKeyword ColonEquals Expression )*
    ;
```

<span data-ttu-id="61fcb-489">Der Zielausdruck muss klassifiziert werden, als einer Methodengruppe oder einen Wert, dessen Typ ein Delegattyp ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-489">The target expression must be classified as a method group or a value whose type is a delegate type.</span></span> <span data-ttu-id="61fcb-490">Wenn der Zielausdruck ist ein Wert, dessen Typ ein Delegattyp, und klicken Sie dann das Ziel der Aufrufausdruck die Methodengruppe für wird die `Invoke` Mitglied der Delegattyp und den Zielausdruck wird der zugeordnete Zielausdruck der-Methode Gruppe.</span><span class="sxs-lookup"><span data-stu-id="61fcb-490">If the target expression is a value whose type is a delegate type, then the target of the invocation expression becomes the method group for the `Invoke` member of the delegate type and the target expression becomes the associated target expression of the method group.</span></span>

<span data-ttu-id="61fcb-491">Eine Argumentliste besteht aus zwei Abschnitten: positionelle Argumente und benannte Argumente.</span><span class="sxs-lookup"><span data-stu-id="61fcb-491">An argument list has two sections: positional arguments and named arguments.</span></span> <span data-ttu-id="61fcb-492">*Positionelle Argumente* sind Ausdrücke und müssen vor benannten Argumenten vorangestellt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-492">*Positional arguments* are expressions and must precede any named arguments.</span></span> <span data-ttu-id="61fcb-493">*Benannte Argumente* starten mit einer ID, die Schlüsselwörter, gefolgt von abgleichen können `:=` und einen Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="61fcb-493">*Named arguments* start with an identifier that can match keywords, followed by `:=` and an expression.</span></span>

<span data-ttu-id="61fcb-494">Wenn die Methodengruppe nur über eine zugängliche Methode enthält, einschließlich der sowohl die Instanz als auch die Erweiterung Methoden und die Methode keine Argumente akzeptiert und ist eine Funktion, klicken Sie dann die Methodengruppe wird als ein Aufrufausdruck mit einer leeren Argumentliste interpretiert und das Ergebnis ist als Ziel eines Aufrufausdrucks mit der angegebenen Argumente verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-494">If the method group only contains one accessible method, including both instance and extension methods, and that method takes no arguments and is a function, then the method group is interpreted as an invocation expression with an empty argument list and the result is used as the target of an invocation expression with the provided argument list(s).</span></span> <span data-ttu-id="61fcb-495">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-495">For example:</span></span>

```vb
Class C1
    Function M1() As Integer()
        Return New Integer() { 1, 2, 3 }
    End Sub
End Class

Module Test
    Sub Main()
        Dim c As New C1()

        ' Prints 3
        Console.WriteLine(c.M1(2))
    End Sub
End Module
```

<span data-ttu-id="61fcb-496">Andernfalls wird die Auflösung von funktionsüberladungen an die Methoden, die am besten geeignete Methode für die angegebenen Argumente auszuwählen angewendet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-496">Otherwise, overload resolution is applied to the methods to pick the most applicable method for the given argument list(s).</span></span> <span data-ttu-id="61fcb-497">Ist die am besten geeignete Methode eine Funktion, wird das Ergebnis des Aufrufausdrucks als Wert als den Rückgabetyp der Funktion klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-497">If the most applicable method is a function, then the result of the invocation expression is classified as a value typed as the return type of the function.</span></span> <span data-ttu-id="61fcb-498">Wenn die am besten geeignete Methode eine Subroutine ist, wird das Ergebnis als "void" klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-498">If the most applicable method is a subroutine, then the result is classified as void.</span></span> <span data-ttu-id="61fcb-499">Ist die am besten geeignete Methode für eine partielle Methode, die ohne Nachrichtentext, der Aufrufausdruck wird ignoriert, und das Ergebnis wird als "void" klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-499">If the most applicable method is a partial method that has no body, then the invocation expression is ignored and the result is classified as void.</span></span>

<span data-ttu-id="61fcb-500">Für einen Aufrufausdruck früh gebundener sind die Argumente in der Reihenfolge ausgewertet, in denen die entsprechenden Parameter in der Zielmethode deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-500">For an early-bound invocation expression, the arguments are evaluated in the order in which the corresponding parameters are declared in the target method.</span></span> <span data-ttu-id="61fcb-501">Für spät gebundene Memberzugriffsausdruck, werden sie in der Reihenfolge in der der Ausdruck für den Elementzugriff ausgewertet: finden Sie im Abschnitt [Late-Bound Expressions](expressions.md#late-bound-expressions).</span><span class="sxs-lookup"><span data-stu-id="61fcb-501">For a late-bound member access expression, they are evaluated in the order in which they appear in the member access expression: see Section [Late-Bound Expressions](expressions.md#late-bound-expressions).</span></span>

## <a name="overloaded-method-resolution"></a><span data-ttu-id="61fcb-502">Lösung für die überladene Methode:</span><span class="sxs-lookup"><span data-stu-id="61fcb-502">Overloaded Method Resolution:</span></span>
<span data-ttu-id="61fcb-503">Overload Resolution, Detailgenauigkeit der Member/Typen, die ein Argument erhält aufzulisten, die Generizität, Anwendbarkeit Argumentliste, übergeben von Argumenten und Auswählen der Argumente für optionale Parameter, bedingte Methoden und Typrückschluss Argument: finden Sie im Abschnitt [ Auflösen der Überladung](overload-resolution.md).</span><span class="sxs-lookup"><span data-stu-id="61fcb-503">For Overload Resolution, Specificity of members/types given an argument list, Genericity, Applicability to Argument List, Passing Arguments, and Picking Arguments for Optional Parameters, Conditional Methods, and Type Argument Inference: see Section [Overload Resolution](overload-resolution.md).</span></span>

## <a name="index-expressions"></a><span data-ttu-id="61fcb-504">Indizieren von Ausdrücken</span><span class="sxs-lookup"><span data-stu-id="61fcb-504">Index Expressions</span></span>

<span data-ttu-id="61fcb-505">Ein *indizieren Ausdruck* führt zu einem Arrayelement oder Klassifizierung eine Eigenschaftengruppe in einen Eigenschaftenzugriff.</span><span class="sxs-lookup"><span data-stu-id="61fcb-505">An *index expression* results in an array element or reclassifies a property group into a property access.</span></span> <span data-ttu-id="61fcb-506">Ein Indexausdruck besteht aus, in der Reihenfolge, ein Ausdruck, eine öffnende Klammer ein, eine Argumentliste für den Index und eine schließende Klammer ein.</span><span class="sxs-lookup"><span data-stu-id="61fcb-506">An index expression consists of, in order, an expression, an opening parenthesis, an index argument list, and a closing parenthesis.</span></span>

```antlr
IndexExpression
    : Expression OpenParenthesis ArgumentList? CloseParenthesis
    ;
```

<span data-ttu-id="61fcb-507">Das Ziel der Indexausdruck muss als ein Wert oder eine Eigenschaftengruppe klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-507">The target of the index expression must be classified as either a property group or a value.</span></span> <span data-ttu-id="61fcb-508">Ein Indexausdruck wird wie folgt verarbeitet:</span><span class="sxs-lookup"><span data-stu-id="61fcb-508">An index expression is processed as follows:</span></span>

* <span data-ttu-id="61fcb-509">Wenn der Zielausdruck als Wert klassifiziert wird und dessen Typ kein Arraytyp ist `Object`, oder `System.Array`, der Typ muss eine Default-Eigenschaft aufweisen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-509">If the target expression is classified as a value and if its type is not an array type, `Object`, or `System.Array`, the type must have a default property.</span></span> <span data-ttu-id="61fcb-510">Der Index wird für eine Eigenschaftengruppe ausgeführt, die alle die Standardeigenschaften des Typs darstellt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-510">The index is performed on a property group that represents all of the default properties of the type.</span></span> <span data-ttu-id="61fcb-511">Obwohl es nicht zulässig, eine parameterlosen Standardeigenschaft in Visual Basic deklariert ist, können andere Sprachen, deklarieren eine solche Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="61fcb-511">Although it is not valid to declare a parameterless default property in Visual Basic, other languages may allow declaring such a property.</span></span> <span data-ttu-id="61fcb-512">Daher ist die Indizierung einer Eigenschaft ohne Argumente zulässig.</span><span class="sxs-lookup"><span data-stu-id="61fcb-512">Consequently, indexing a property with no arguments is allowed.</span></span>

* <span data-ttu-id="61fcb-513">Wenn der Ausdruck einen Wert eines Arraytyps ergibt, wird die Anzahl der Argumente in der Argumentliste muss identisch mit den Rang des Arraytyps und umfassen möglicherweise nicht die benannten Argumente.</span><span class="sxs-lookup"><span data-stu-id="61fcb-513">If the expression results in a value of an array type, the number of arguments in the argument list must be the same as the rank of the array type and may not include named arguments.</span></span> <span data-ttu-id="61fcb-514">Wenn einer der Indizes zur Laufzeit ungültig sind eine `System.IndexOutOfRangeException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="61fcb-514">If any of the indexes are invalid at run time, a `System.IndexOutOfRangeException` exception is thrown.</span></span> <span data-ttu-id="61fcb-515">Jeder Ausdruck muss implizit in den Typ `Integer`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-515">Each expression must be implicitly convertible to type `Integer`.</span></span> <span data-ttu-id="61fcb-516">Das Ergebnis des Indexausdrucks ist die Variable am angegebenen Index und wird als Variable klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-516">The result of the index expression is the variable at the specified index and is classified as a variable.</span></span>

* <span data-ttu-id="61fcb-517">Wenn der Ausdruck als eine Eigenschaftengruppe klassifiziert ist, wird Auflösung von funktionsüberladungen verwendet, um zu bestimmen, ob eine der Eigenschaften an die Argumentliste Index anwendbar ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-517">If the expression is classified as a property group, overload resolution is used to determine whether one of the properties is applicable to the index argument list.</span></span> <span data-ttu-id="61fcb-518">Wenn die Eigenschaftengruppe nur eine Eigenschaft enthält, besitzt eine `Get` -Accessor und wenn diese Zugriffsmethode akzeptiert keine Argumente, die Eigenschaftengruppe wird als ein Indexausdruck mit einer leeren Argumentliste interpretiert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-518">If the property group only contains one property that has a `Get` accessor and if that accessor takes no arguments, then the property group is interpreted as an index expression with an empty argument list.</span></span> <span data-ttu-id="61fcb-519">Das Ergebnis wird als Ziel für den aktuellen Indexausdruck verwendet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-519">The result is used as the target of the current index expression.</span></span> <span data-ttu-id="61fcb-520">Wenn keine Eigenschaften anwendbar sind, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-520">If no properties are applicable, then a compile-time error occurs.</span></span> <span data-ttu-id="61fcb-521">Andernfalls führt der Ausdruck ein Eigenschaftszugriff mit den zugehörigen Zielausdruck (sofern vorhanden) der Eigenschaftengruppe.</span><span class="sxs-lookup"><span data-stu-id="61fcb-521">Otherwise, the expression results in a property access with the associated target expression (if any) of the property group.</span></span>

* <span data-ttu-id="61fcb-522">Wenn der Ausdruck klassifiziert ist, als eine Gruppe von spät gebundenen Eigenschaft oder als ein Wert, dessen Typ `Object` oder `System.Array`, die Verarbeitung des Indexausdrucks wird verzögert, bis zur Laufzeit und die Indizierung wird spät gebunden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-522">If the expression is classified as a late-bound property group or as a value whose type is `Object` or `System.Array`, the processing of the index expression is deferred until run time and the indexing is late-bound.</span></span> <span data-ttu-id="61fcb-523">Die Ergebnisse des Ausdrucks in einer Eigenschaft mit später Bindung den Zugriff als typisierte `Object`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-523">The expression results in a late-bound property access typed as `Object`.</span></span> <span data-ttu-id="61fcb-524">Die zielbereitstellungsumgebung-Ausdruck ist der Zielausdruck, wenn er ein Wert ist, oder der zugehörigen Zielausdruck der Eigenschaftengruppe.</span><span class="sxs-lookup"><span data-stu-id="61fcb-524">The associated target expression is either the target expression, if it is a value, or the associated target expression of the property group.</span></span> <span data-ttu-id="61fcb-525">Zur Laufzeit wird der Ausdruck wie folgt verarbeitet:</span><span class="sxs-lookup"><span data-stu-id="61fcb-525">At run time the expression is processed as follows:</span></span>

* <span data-ttu-id="61fcb-526">Wenn der Ausdruck als spät gebundene Eigenschaftengruppe klassifiziert ist, möglicherweise der Ausdruck in einer Methodengruppe, eine Eigenschaftengruppe oder einen Wert (wenn das Element eine Instanz oder freigegebene Variable ist).</span><span class="sxs-lookup"><span data-stu-id="61fcb-526">If the expression is classified as a late-bound property group, the expression may result in a method group, a property group, or a value (if the member is an instance or shared variable).</span></span> <span data-ttu-id="61fcb-527">Ist das Ergebnis einer Methodengruppe oder einer Eigenschaftengruppe, wird die Auflösung von funktionsüberladungen auf die Gruppe aus, um zu bestimmen, die richtige Methode für die Argumentliste angewendet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-527">If the result is a method group or property group, overload resolution is applied to the group to determine the correct method for the argument list.</span></span> <span data-ttu-id="61fcb-528">Wenn die Auflösung von funktionsüberladungen ein Fehler auftritt, eine `System.Reflection.AmbiguousMatchException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="61fcb-528">If overload resolution fails, a `System.Reflection.AmbiguousMatchException` exception is thrown.</span></span> <span data-ttu-id="61fcb-529">Klicken Sie dann das Ergebnis als Zugriff auf eine Eigenschaft oder ein Aufruf verarbeitet wird, und das Ergebnis wird zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="61fcb-529">Then the result is processed either as a property access or as an invocation and the result is returned.</span></span> <span data-ttu-id="61fcb-530">Wenn der Aufruf eine Subroutine ist, wird das Ergebnis ist `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-530">If the invocation is of a subroutine, the result is `Nothing`.</span></span>

* <span data-ttu-id="61fcb-531">Die Run-Time-Typ, der den Zielausdruck ist ein Arraytyp oder `System.Array`, das Ergebnis des Indexausdrucks ist der Wert der Variablen am angegebenen Index.</span><span class="sxs-lookup"><span data-stu-id="61fcb-531">If the run-time type of the target expression is an array type or `System.Array`, the result of the index expression is the value of the variable at the specified index.</span></span>

* <span data-ttu-id="61fcb-532">Andernfalls die Run-Time-Typ des Ausdrucks muss eine Default-Eigenschaft haben, und der Index wird ausgeführt, auf die Eigenschaftengruppe, die alle die Standardeigenschaften für den Typ darstellt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-532">Otherwise, the run-time type of the expression must have a default property and the index is performed on the property group that represents all of the default properties on the type.</span></span> <span data-ttu-id="61fcb-533">Wenn der Typ keine Standardeigenschaft hat ein `System.MissingMemberException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="61fcb-533">If the type has no default property, then a `System.MissingMemberException` exception is thrown.</span></span>


## <a name="new-expressions"></a><span data-ttu-id="61fcb-534">New-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-534">New Expressions</span></span>

<span data-ttu-id="61fcb-535">Die `New` Operator wird verwendet, um neue Instanzen von Typen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-535">The `New` operator is used to create new instances of types.</span></span> <span data-ttu-id="61fcb-536">Es gibt vier Arten von `New` Ausdrücke:</span><span class="sxs-lookup"><span data-stu-id="61fcb-536">There are four forms of `New` expressions:</span></span>

* <span data-ttu-id="61fcb-537">Objekterstellung-Ausdrücke werden verwendet, um neue Instanzen der Klasse und Werttypen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-537">Object-creation expressions are used to create new instances of class types and value types.</span></span>

* <span data-ttu-id="61fcb-538">Für die Arrayerstellung-Ausdrücke werden verwendet, um neue Instanzen von Arraytypen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-538">Array-creation expressions are used to create new instances of array types.</span></span>

* <span data-ttu-id="61fcb-539">Delegaterstellung Ausdrücke (die eine unterschiedliche Syntax von Objekt-und Arrayerstellung Ausdrücken keine) werden verwendet, um neue Instanzen von Delegaten erstellt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-539">Delegate-creation expressions (which do not have a distinct syntax from object-creation expressions) are used to create new instances of delegate types.</span></span>

* <span data-ttu-id="61fcb-540">Anonyme objekterstellung-Ausdrücke werden verwendet, um neue Instanzen der anonyme Klassentypen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-540">Anonymous object-creation expressions are used to create new instances of anonymous class types.</span></span>

```antlr
NewExpression
    : ObjectCreationExpression
    | ArrayExpression
    | AnonymousObjectCreationExpression
    ;
```

<span data-ttu-id="61fcb-541">Ein `New` Ausdruck wird als Wert klassifiziert, und das Ergebnis ist die neue Instanz des Typs.</span><span class="sxs-lookup"><span data-stu-id="61fcb-541">A `New` expression is classified as a value and the result is the new instance of the type.</span></span>


### <a name="object-creation-expressions"></a><span data-ttu-id="61fcb-542">Objekterstellung Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-542">Object-Creation Expressions</span></span>

<span data-ttu-id="61fcb-543">Ein Ausdruck für die objekterstellung wird verwendet, um eine neue Instanz der ein Klassen- oder Strukturtyp zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-543">An object-creation expression is used to create a new instance of a class type or a structure type.</span></span>

```antlr
ObjectCreationExpression
    : 'New' NonArrayTypeName ( OpenParenthesis ArgumentList? CloseParenthesis )?
      ObjectCreationExpressionInitializer?
    ;

ObjectCreationExpressionInitializer
    : ObjectMemberInitializer
    | ObjectCollectionInitializer
    ;

ObjectMemberInitializer
    : 'With' OpenCurlyBrace FieldInitializerList CloseCurlyBrace
    ;

FieldInitializerList
    : FieldInitializer ( Comma FieldInitializer )*
    ;

FieldInitializer
    : 'Key'? ('.' IdentifierOrKeyword Equals )? Expression
    ;

ObjectCollectionInitializer
    : 'From' CollectionInitializer
    ;

CollectionInitializer
    : OpenCurlyBrace CollectionElementList? CloseCurlyBrace
    ;

CollectionElementList
    : CollectionElement ( Comma CollectionElement )*
    ;

CollectionElement
    : Expression
    | CollectionInitializer
    ;
```

<span data-ttu-id="61fcb-544">Der Typ, der eine Objekterstellungsausdruck muss ein Klassentyp, einen Strukturtyp oder ein Typparameter mit einer `New` Einschränkung und kann kein `MustInherit` Klasse.</span><span class="sxs-lookup"><span data-stu-id="61fcb-544">The type of an object creation expression must be a class type, a structure type, or a type parameter with a `New` constraint and cannot be a `MustInherit` class.</span></span> <span data-ttu-id="61fcb-545">Erhält ein Objekterstellungsausdruck des Formulars `New T(A)`, wobei `T` ist eine Klassen- oder Strukturtyp und `A` ist eine optionale Argumentliste Auflösung von funktionsüberladungen bestimmt den korrekten Konstruktor von `T` aufrufen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-545">Given an object creation expression of the form `New T(A)`, where `T` is a class type or structure type and `A` is an optional argument list, overload resolution determines the correct constructor of `T` to call.</span></span> <span data-ttu-id="61fcb-546">Ein Typparameter mit einer `New` Einschränkung gilt als einen einzelnen, parameterlosen Konstruktor haben.</span><span class="sxs-lookup"><span data-stu-id="61fcb-546">A type parameter with a `New` constraint is considered to have a single, parameterless constructor.</span></span> <span data-ttu-id="61fcb-547">Wenn kein Konstruktor aufgerufen werden kann, tritt ein Fehler während der Kompilierung; Andernfalls führt der Ausdruck zur Erstellung einer neuen Instanz von `T` mit dem ausgewählten Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="61fcb-547">If no constructor is callable, a compile-time error occurs; otherwise the expression results in the creation of a new instance of `T` using the chosen constructor.</span></span> <span data-ttu-id="61fcb-548">Wenn keine Argumente vorhanden sind, können die Klammern weggelassen werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-548">If there are no arguments, the parentheses may be omitted.</span></span>

<span data-ttu-id="61fcb-549">Der eine Instanz zugeordnet ist, hängt davon ab, ob die Instanz eines Klassentyps oder ein Werttyp ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-549">Where an instance is allocated depends on whether the instance is a class type or a value type.</span></span> <span data-ttu-id="61fcb-550">`New` Instanzen von Klassentypen werden auf den Systemheap, erstellt, während neue Instanzen von Werttypen, direkt auf dem Stapel erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-550">`New` instances of class types are created on the system heap, while new instances of value types are created directly on the stack.</span></span>

<span data-ttu-id="61fcb-551">Ein Ausdruck für die objekterstellung kann optional eine Liste der Standardmember-Initialisierer nach dem Konstruktorargument angeben.</span><span class="sxs-lookup"><span data-stu-id="61fcb-551">An object-creation expression can optionally specify a list of member initializers after the constructor arguments.</span></span> <span data-ttu-id="61fcb-552">Diese Standardmember-Initialisierer werden mit dem Schlüsselwort mit dem Präfix `With`, und die Initialisiererliste wird interpretiert, als wäre es im Rahmen einer `With` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-552">These member initializers are prefixed with the keyword `With`, and the initializer list is interpreted as if it was in the context of a `With` statement.</span></span> <span data-ttu-id="61fcb-553">Betrachten Sie z. B. die Klasse ein:</span><span class="sxs-lookup"><span data-stu-id="61fcb-553">For example, given the class:</span></span>

```vb
Class Customer
    Dim Name As String
    Dim Address As String
End Class
```

<span data-ttu-id="61fcb-554">Der Code:</span><span class="sxs-lookup"><span data-stu-id="61fcb-554">The code:</span></span>

```vb
Module Test
    Sub Main()
        Dim x As New Customer() With { .Name = "Bob Smith", _
            .Address = "123 Main St." }
    End Sub
End Module
```

<span data-ttu-id="61fcb-555">ist ungefähr gleich ist:</span><span class="sxs-lookup"><span data-stu-id="61fcb-555">Is roughly equivalent to:</span></span>

```vb
Module Test
    Sub Main()
        Dim x, _t1 As Customer

        _t1 = New Customer()
        With _t1
            .Name = "Bob Smith"
            .Address = "123 Main St."
        End With

        x = _t1
    End Sub
End Module
```

<span data-ttu-id="61fcb-556">Jeder Initialisierer muss angeben, dass ein Name zugewiesen, und es muss der Name einer nicht -`ReadOnly` Instanzvariable oder eine Eigenschaft des Typs erstellt wird; der Memberzugriff wird nicht spät gebunden werden, wenn der Typ der zu erstellenden `Object`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-556">Each initializer must specify a name to assign, and the name must be a non-`ReadOnly` instance variable or property of the type being constructed; the member access will not be late bound if the type being constructed is `Object`.</span></span> <span data-ttu-id="61fcb-557">Initialisierer können Sie nicht die `Key` Schlüsselwort.</span><span class="sxs-lookup"><span data-stu-id="61fcb-557">Initializers may not use the `Key` keyword.</span></span> <span data-ttu-id="61fcb-558">Jedes Element in einem Typ kann nur einmal initialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-558">Each member in a type can only be initialized once.</span></span> <span data-ttu-id="61fcb-559">Die Initialisierer-Ausdrücke können jedoch aufeinander verweisen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-559">The initializer expressions, however, may refer to each other.</span></span> <span data-ttu-id="61fcb-560">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-560">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim x As New Customer() With { .Name = "Bob Smith", _
            .Address = .Name & " St." }
    End Sub
End Module
```

<span data-ttu-id="61fcb-561">Die Initialisierer links nach rechts zugewiesen ist, also wenn Sie ein Initialisierer für einen Member bezieht, die noch nicht initialisiert wurde, anzeigen, wird Wert was auch immer die Instanzvariable, nachdem der Konstruktor ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="61fcb-561">The initializers are assigned left-to-right, so if an initializer refers to a member that has not been initialized yet, it will see whatever value the instance variable after the constructor ran:</span></span>

```vb
Module Test
    Sub Main()
        ' The value of Address will be " St." since Name has not been
        ' assigned yet.
        Dim x As New Customer() With { .Address = .Name & " St." }
    End Sub
End Module
```

<span data-ttu-id="61fcb-562">Initialisierer können geschachtelt werden:</span><span class="sxs-lookup"><span data-stu-id="61fcb-562">Initializers can be nested:</span></span>

```vb
Class Customer
    Dim Name As String
    Dim Address As Address
    Dim Age As Integer
End Class

Class Address
    Dim Street As String
    Dim City As String
    Dim State As String
    Dim ZIP As String
End Class

Module Test
    Sub Main()
        Dim c As New Customer() With { _
            .Name = "John Smith", _
            .Address = New Address() With { _
                .Street = "23 Main St.", _
                .City = "Peoria", _
                .State = "IL", _
                .ZIP = "13934" }, _
            .Age = 34 }
    End Sub
End Module
```

<span data-ttu-id="61fcb-563">Wenn der erstellte Typ wird vom Auflistungstyp und verfügt über eine Instanzmethode mit der Bezeichnung `Add` (einschließlich Erweiterungsmethoden und freigegebene Methoden), geben Sie dann der Ausdruck für die objekterstellung kann einen Auflistungsinitialisierer, mit dem Präfix werden mit dem Schlüsselwort `From`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-563">If the type being created is a collection type and has an instance method named `Add` (including extension methods and shared methods), then the object-creation expression can specify a collection initializer prefixed by the keyword `From`.</span></span> <span data-ttu-id="61fcb-564">Ein Ausdruck für die objekterstellung Memberinitialisierer und einem Auflistungsinitialisierer nicht angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-564">An object-creation expression cannot specify both a member initializer and a collection initializer.</span></span> <span data-ttu-id="61fcb-565">Jedes Element im Auflistungsinitialisierer als Argument übergeben wird, auf einen Aufruf der `Add` Funktion.</span><span class="sxs-lookup"><span data-stu-id="61fcb-565">Each element in the collection initializer is passed as an argument to an invocation of the `Add` function.</span></span> <span data-ttu-id="61fcb-566">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-566">For example:</span></span>

```vb
Dim list = New List(Of Integer)() From { 1, 2, 3, 4 }
```

<span data-ttu-id="61fcb-567">identisch mit folgendem Ausdruck:</span><span class="sxs-lookup"><span data-stu-id="61fcb-567">is equivalent to:</span></span>

```vb
Dim list = New List(Of Integer)()
list.Add(1)
list.Add(2)
list.Add(3)
```

<span data-ttu-id="61fcb-568">Wenn ein Element eines Auflistungsinitialisierers selbst ist, wird jedes Element des Initialisierers unterauflistung übergeben werden, als einzelne Argument an die `Add` Funktion.</span><span class="sxs-lookup"><span data-stu-id="61fcb-568">If an element is a collection initializer itself, each element of the sub-collection initializer will be passed as an individual argument to the `Add` function.</span></span> <span data-ttu-id="61fcb-569">Z. B. Folgendes:</span><span class="sxs-lookup"><span data-stu-id="61fcb-569">For example, the following:</span></span>

```vb
Dim dict = Dictionary(Of Integer, String) From { { 1, "One" },{ 2, "Two" } }
```

<span data-ttu-id="61fcb-570">identisch mit folgendem Ausdruck:</span><span class="sxs-lookup"><span data-stu-id="61fcb-570">is equivalent to:</span></span>

```vb
Dim dict = New Dictionary(Of Integer, String)
dict.Add(1, "One")
dict.Add(2, "Two")
```

<span data-ttu-id="61fcb-571">Diese Erweiterung ist erfolgt immer und immer nur eine Ebene tief; Danach gelten die untergeordnete Initialisierer Array-Literale.</span><span class="sxs-lookup"><span data-stu-id="61fcb-571">This expansion is always done and is only ever done one level deep; after that, sub-initializers are considered array literals.</span></span> <span data-ttu-id="61fcb-572">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-572">For example:</span></span>

```vb
' Error: List(Of T) does not have an Add method that takes two parameters.
Dim list = New List(Of Integer())() From { { 1, 2 }, { 3, 4 } }

' OK, this initializes the dictionary with (Integer, Integer()) pairs.
Dim dict = New Dictionary(Of Integer, Integer())() From _
        { {  1, { 2, 3 } }, { 3, { 4, 5 } } }
```


### <a name="array-expressions"></a><span data-ttu-id="61fcb-573">Array-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-573">Array Expressions</span></span>

<span data-ttu-id="61fcb-574">Ein Arrayausdruck wird verwendet, um eine neue Instanz der Array-Typ zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-574">An array expression is used to create a new instance of an array type.</span></span> <span data-ttu-id="61fcb-575">Es gibt zwei Arten von Array-Ausdrücke: Array erstellen Ausdrücke und Array-Literale.</span><span class="sxs-lookup"><span data-stu-id="61fcb-575">There are two types of array expressions: array creation expressions, and array literals.</span></span>

#### <a name="array-creation-expressions"></a><span data-ttu-id="61fcb-576">Arrayausdrücke-Erstellung</span><span class="sxs-lookup"><span data-stu-id="61fcb-576">Array creation expressions</span></span>

<span data-ttu-id="61fcb-577">Wenn ein Array-Größe Initialisierung-Modifizierer angegeben ist, wird der resultierende Arraytyp abgeleitet, durch das Löschen der einzelnen der einzelnen Argumente aus der Argumentliste von Array Größe Initialisierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-577">If an array size initialization modifier is supplied, the resulting array type is derived by deleting each of the individual arguments from the array size initialization argument list.</span></span> <span data-ttu-id="61fcb-578">Der Wert jedes Argument bestimmt die obere Grenze der entsprechenden Dimension in der neu zugewiesenen Arrayinstanz fest.</span><span class="sxs-lookup"><span data-stu-id="61fcb-578">The value of each argument determines the upper bound of the corresponding dimension in the newly allocated array instance.</span></span> <span data-ttu-id="61fcb-579">Wenn der Ausdruck einen nicht leeren Auflistungsinitialisierer, jedes Argument in der Argumentliste muss eine Konstante sein, und der Rang jede Dimension angegebenen Länge angegeben wird, indem Sie die Liste der Ausdrücke müssen mit dem der Auflistungsinitialisierer.</span><span class="sxs-lookup"><span data-stu-id="61fcb-579">If the expression has a non-empty collection initializer, each argument in the argument list must be a constant, and the rank and dimension lengths specified by the expression list must match those of the collection initializer.</span></span>

```vb
Dim a() As Integer = New Integer(2) {}
Dim b() As Integer = New Integer(2) { 1, 2, 3 }
Dim c(,) As Integer = New Integer(1, 2) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error, length/initializer mismatch.
Dim d() As Integer = New Integer(2) { 0, 1, 2, 3 }
```

<span data-ttu-id="61fcb-580">Wenn ein Array-Größe Initialisierung-Modifizierer nicht angegeben wird, klicken Sie dann der Typname muss ein Arraytyp sein und der Auflistungsinitialisierer muss leer sein oder die gleiche Anzahl von Schachtelungsebenen als der Rang des angegebenen Arraytyps.</span><span class="sxs-lookup"><span data-stu-id="61fcb-580">If an array size initialization modifier is not supplied, then the type name must be an array type and the collection initializer must be empty or have the same number of levels of nesting as the rank of the specified array type.</span></span> <span data-ttu-id="61fcb-581">Alle Elemente in der innersten Schachtelungsebene muss implizit in den Typ des Elements des Arrays sein und muss als Wert klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-581">All of the elements in the innermost nesting level must be implicitly convertible to the element type of the array and must be classified as a value.</span></span> <span data-ttu-id="61fcb-582">Die Anzahl der Elemente in jede geschachtelte Auflistungsinitialisierer muss immer konsistent mit der Größe der anderen Sammlungen auf der gleichen Ebene sein.</span><span class="sxs-lookup"><span data-stu-id="61fcb-582">The number of elements in each nested collection initializer must always be consistent with the size of the other collections at the same level.</span></span> <span data-ttu-id="61fcb-583">Die Längen der einzelnen Dimension werden von der Anzahl der Elemente in jedem von der entsprechenden Schachtelungsebenen des auflistungs-bzw. Arrayinitialisierers hergeleitet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-583">The individual dimension lengths are inferred from the number of elements in each of the corresponding nesting levels of the collection initializer.</span></span> <span data-ttu-id="61fcb-584">Wenn der Auflistungsinitialisierer leer ist, ist die Länge jeder Dimension 0 (null).</span><span class="sxs-lookup"><span data-stu-id="61fcb-584">If the collection initializer is empty, the length of each dimension is zero.</span></span>

```vb
Dim e() As Integer = New Integer() { 1, 2, 3 }
Dim f(,) As Integer = New Integer(,) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error: Inconsistent numbers of elements!
Dim g(,) As Integer = New Integer(,) { { 1, 2 }, { 4, 5, 6 } }

' Error: Inconsistent levels of nesting!
Dim h(,) As Integer = New Integer(,) { 1, 2, { 3, 4 } }
```

<span data-ttu-id="61fcb-585">Die äußerste Schachtelungsebene eines Auflistungsinitialisierers entsprechen der am weitesten links stehende Dimension eines Arrays, und die innerste Schachtelungsebene entspricht die Dimension ganz rechts.</span><span class="sxs-lookup"><span data-stu-id="61fcb-585">The outermost nesting level of a collection initializer corresponds to the leftmost dimension of an array, and the innermost nesting level corresponds to the rightmost dimension.</span></span> <span data-ttu-id="61fcb-586">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-586">The example:</span></span>

```vb
Dim array As Integer(,) = _
    { { 0, 1 }, { 2, 3 }, { 4, 5 }, { 6, 7 }, { 8, 9 } }
```

<span data-ttu-id="61fcb-587">Ist äquivalent zu folgendem:</span><span class="sxs-lookup"><span data-stu-id="61fcb-587">Is equivalent to the following:</span></span>

```vb
Dim array(4, 1) As Integer

array(0, 0) = 0: array(0, 1) = 1
array(1, 0) = 2: array(1, 1) = 3
array(2, 0) = 4: array(2, 1) = 5
array(3, 0) = 6: array(3, 1) = 7
array(4, 0) = 8: array(4, 1) = 9
```

<span data-ttu-id="61fcb-588">Wenn der Auflistungsinitialisierer ist leer (d. h. eine, die geschweifte Klammern enthält), aber keine Initialisierungsliste und die Grenzen des der Dimensionen des Arrays initialisiert wird, sind bekannt, der Initialisierer für die leere Auflistung stellt eine Arrayinstanz der angegebenen Größe in dem alle Elemente auf den Typ des Elements Standardwert initialisiert haben.</span><span class="sxs-lookup"><span data-stu-id="61fcb-588">If the collection initializer is empty (that is, one that contains curly braces but no initializer list) and the bounds of the dimensions of the array being initialized are known, the empty collection initializer represents an array instance of the specified size where all the elements have been initialized to the element type's default value.</span></span> <span data-ttu-id="61fcb-589">Wenn die Begrenzungen der Dimensionen des Arrays initialisiert wird, nicht bekannt sind, stellt die leere Auflistung-Initialisierer eine Array-Instanz, die in der alle Dimensionen Größe 0 (null) sind dar.</span><span class="sxs-lookup"><span data-stu-id="61fcb-589">If the bounds of the dimensions of the array being initialized are not known, the empty collection initializer represents an array instance in which all dimensions are size zero.</span></span>

<span data-ttu-id="61fcb-590">Rang und die Länge jeder Dimension einer Arrayinstanz bleiben während der gesamten Lebensdauer der Instanz zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-590">An array instance's rank and length of each dimension are constant for the entire lifetime of the instance.</span></span> <span data-ttu-id="61fcb-591">Das heißt, es ist nicht möglich, den Rang einer vorhandenen Instanz des Arrays zu ändern, noch ist es möglich, die Größe seiner Dimensionen ändern.</span><span class="sxs-lookup"><span data-stu-id="61fcb-591">In other words, it is not possible to change the rank of an existing array instance, nor is it possible to resize its dimensions.</span></span>

#### <a name="array-literals"></a><span data-ttu-id="61fcb-592">Arrayliterale</span><span class="sxs-lookup"><span data-stu-id="61fcb-592">Array Literals</span></span>

<span data-ttu-id="61fcb-593">Ein Arrayliteral gibt ein Array, dessen Elementtyp, Rang und Grenzen aus einer Kombination des Kontexts Ausdruck und einem Auflistungsinitialisierer abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-593">An array literal denotes an array whose element type, rank, and bounds are inferred from a combination of the expression context and a collection initializer.</span></span> <span data-ttu-id="61fcb-594">Dies wird im Abschnitt erläutert [Ausdruck Neuklassifizierung](expressions.md#expression-reclassification).</span><span class="sxs-lookup"><span data-stu-id="61fcb-594">This is explained in Section [Expression Reclassification](expressions.md#expression-reclassification).</span></span>

```antlr
ArrayExpression
    : ArrayCreationExpression
    | ArrayLiteralExpression
    ;

ArrayCreationExpression
    : 'New' NonArrayTypeName ArrayNameModifier CollectionInitializer
    ;

ArrayLiteralExpression
    : CollectionInitializer
    ;
```

<span data-ttu-id="61fcb-595">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-595">For example:</span></span>

```vb
' array of integers
Dim a = {1, 2, 3}

' array of shorts
Dim b = {1S, 2S, 3S}

' array of shorts whose type is taken from the context
Dim c As Short() = {1, 2, 3}

' array of type Integer(,)
Dim d = {{1, 0}, {0, 1}}

' jagged array of rank ()()
Dim e = {({1, 0}), ({0, 1})}

' error: inconsistent rank
Dim f = {{1}, {2, 3}}

' error: inconsistent rank
Dim g = {1, {2}}
```

<span data-ttu-id="61fcb-596">Das Format und die Anforderungen für den Auflistungsinitialisierer in einem Arrayliteral entspricht genau der für den Auflistungsinitialisierer in einem Arrayerstellungsausdruck.</span><span class="sxs-lookup"><span data-stu-id="61fcb-596">The format and requirements for the collection initializer in an array literal is exactly the same as that for the collection initializer in an array creation expression.</span></span>

<span data-ttu-id="61fcb-597">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-597">__Note.__</span></span> <span data-ttu-id="61fcb-598">Ein Array-literal erstellt das Array an und für sich keine, Stattdessen ist es die neuklassifizierung des Ausdrucks in einen Wert, der bewirkt, dass das Array, das erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-598">An array literal does not create the array in and of itself; instead, it is the reclassification of the expression into a value that causes the array to be created.</span></span> <span data-ttu-id="61fcb-599">Z. B. die Konvertierung `CType(new Integer() {1,2,3}, Short())` ist nicht möglich, da keine von Konvertierung `Integer()` zu `Short()`, aber der Ausdruck `CType({1,2,3},Short())` ist möglich, da sie zuerst das Array-literal in den Ausdruck zur Arrayerstellung Klassifizierung `New Short() {1,2,3}`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-599">For instance, the conversion `CType(new Integer() {1,2,3}, Short())` is not possible because there is no conversion from `Integer()` to `Short()`; but the expression `CType({1,2,3},Short())` is possible because it first reclassifies the array literal into the array creation expression `New Short() {1,2,3}`.</span></span>


### <a name="delegate-creation-expressions"></a><span data-ttu-id="61fcb-600">Delegaterstellung Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-600">Delegate-Creation Expressions</span></span>

<span data-ttu-id="61fcb-601">Ein Ausdruck eines wird verwendet, um eine neue Instanz eines Delegattyps zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-601">A delegate-creation expression is used to create a new instance of a delegate type.</span></span> <span data-ttu-id="61fcb-602">Das Argument eines eines Ausdrucks muss es sich um einen Ausdruck, der als ein Methodenzeiger oder einen Lambda-Methode klassifiziert sein.</span><span class="sxs-lookup"><span data-stu-id="61fcb-602">The argument of a delegate-creation expression must be an expression classified as a method pointer or a lambda method.</span></span>

<span data-ttu-id="61fcb-603">Wenn das Argument einer Methodenzeiger ist, muss gilt für die Signatur des Delegattyps eine der Methoden auf, die durch die Zeiger auf den verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-603">If the argument is a method pointer, one of the methods referenced by the method pointer must be applicable to the signature of the delegate type.</span></span> <span data-ttu-id="61fcb-604">Eine Methode `M` gilt für einen Delegattyp `D` wenn:</span><span class="sxs-lookup"><span data-stu-id="61fcb-604">A method `M` is applicable to a delegate type `D` if:</span></span>

* <span data-ttu-id="61fcb-605">`M` ist kein `Partial` oder Text.</span><span class="sxs-lookup"><span data-stu-id="61fcb-605">`M` is not `Partial` or has a body.</span></span>

* <span data-ttu-id="61fcb-606">Beide `M` und `D` sind Funktionen, oder `D` ist eine Unterroutine.</span><span class="sxs-lookup"><span data-stu-id="61fcb-606">Both `M` and `D` are functions, or `D` is a subroutine.</span></span>

* <span data-ttu-id="61fcb-607">`M` und `D` die gleiche Anzahl von Parametern aufweisen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-607">`M` and `D` have the same number of parameters.</span></span>

* <span data-ttu-id="61fcb-608">Die Parametertypen der `M` haben jeweils eine Konvertierung vom Typ des entsprechenden Parametertyps des `D`, und die Modifizierer (d. h. `ByRef`, `ByVal`) übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-608">The parameter types of `M` each have a conversion from the type of the corresponding parameter type of `D`, and their modifiers (i.e. `ByRef`, `ByVal`) match.</span></span>

* <span data-ttu-id="61fcb-609">Der Rückgabetyp der `M`, sofern vorhanden, hat es sich um eine Konvertierung in den Rückgabetyp der `D`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-609">The return type of `M`, if any, has a conversion to the return type of `D`.</span></span>

<span data-ttu-id="61fcb-610">Wenn die Zeiger auf ein spät gebundener Zugriff verweist, wird der spät gebundener Zugriff angenommen, dass auf eine Funktion sein, die die gleiche Anzahl von Parametern wie der Delegattyp.</span><span class="sxs-lookup"><span data-stu-id="61fcb-610">If the method pointer references a late-bound access, then the late-bound access is assumed to be to a function that has the same number of parameters as the delegate type.</span></span>

<span data-ttu-id="61fcb-611">Wenn die strenge Semantik nicht verwendet wird und es wird nur eine Methode, die durch die Zeiger auf den verwiesen wird, aber es ist nicht anwendbar aufgrund der Tatsache, dass es besitzt keine Parameter ist der Typ des Delegaten, und die Methode wird als anwendbar betrachtet und die Parameter oder Rückgabewerte einer RE ignoriert einfach.</span><span class="sxs-lookup"><span data-stu-id="61fcb-611">If strict semantics are not being used and there is only one method referenced by the method pointer, but it is not applicable due to the fact that it has no parameters and the delegate type does, then the method is considered applicable and the parameters or return value are simply ignored.</span></span> <span data-ttu-id="61fcb-612">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-612">For example:</span></span>

```vb
Delegate Sub F(x As Integer)

Module Test
    Sub M()
    End Sub

    Sub Main()
        ' Valid
        Dim x As F = AddressOf M
    End Sub
End Module
```

<span data-ttu-id="61fcb-613">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-613">__Note.__</span></span> <span data-ttu-id="61fcb-614">Die Lockerung ist nur zulässig, wenn aufgrund von Erweiterungsmethoden nicht strikte Semantik verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-614">This relaxation is only allowed when strict semantics are not being used because of extension methods.</span></span> <span data-ttu-id="61fcb-615">Da Erweiterungsmethoden nur betrachtet werden, wenn eine normale Methode ungültig war, ist es möglich, für eine Instanzmethode ohne Parameter aus, um eine Erweiterungsmethode mit Parametern für die Erstellung des Delegaten auszublenden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-615">Because extension methods are only considered if a regular method was not applicable, it is possible for an instance method with no parameters to hide an extension method with parameters for the purpose of delegate construction.</span></span>

<span data-ttu-id="61fcb-616">Wenn mehr als eine Methode, die durch die Zeiger auf den verwiesen wird in den Delegattyp anwendbar und anschließend überladungsauflösung wird verwendet, um zwischen den möglichen Methoden auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-616">If more than one method referenced by the method pointer is applicable to the delegate type, then overload resolution is used to pick between the candidate methods.</span></span> <span data-ttu-id="61fcb-617">Die Typen der Parameter für den Delegaten werden als die Typen der Argumente für die Zwecke der Auflösung von funktionsüberladungen verwendet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-617">The types of the parameters to the delegate are used as the types of the arguments for the purposes of overload resolution.</span></span> <span data-ttu-id="61fcb-618">Wenn kein Kandidat für eine Methode am besten geeignete ein Fehler während der Kompilierung auftritt ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-618">If no one method candidate is most applicable, a compile-time error occurs.</span></span> <span data-ttu-id="61fcb-619">Im folgenden Beispiel wird die lokale Variable mit einem Delegaten, der auf die zweite verweist initialisiert `Square` Methode, da diese Methode in der Methodensignatur und rückgabeanweisung Typ des ist `DoubleFunc`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-619">In the following example, the local variable is initialized with a delegate that refers to the second `Square` method because that method is more applicable to the signature and return type of `DoubleFunc`.</span></span>

```vb
Delegate Function DoubleFunc(x As Double) As Double

Module Test
    Function Square(x As Single) As Single
        Return x * x
    End Function 

    Function Square(x As Double) As Double
        Return x * x
    End Function

    Sub Main()
        Dim a As New DoubleFunc(AddressOf Square)
    End Sub
End Module
```

<span data-ttu-id="61fcb-620">Haben Sie die zweite `Square` -Methode nicht vorhanden war, werden die ersten `Square` Methode würde gewählt wurde.</span><span class="sxs-lookup"><span data-stu-id="61fcb-620">Had the second `Square` method not been present, the first `Square` method would have been chosen.</span></span> <span data-ttu-id="61fcb-621">Wenn strikte Semantik, durch die kompilierungsumgebung oder durch angegeben werden `Option Strict`, und klicken Sie dann ein Fehler während der Kompilierung tritt auf, wenn die spezifischste Methode, die auf die verwiesen wird durch die Zeiger auf den schmaler als die Signatur des Delegaten ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-621">If strict semantics are specified by the compilation environment or by `Option Strict`, then a compile-time error occurs if the most specific method referenced by the method pointer is narrower than the delegate signature.</span></span> <span data-ttu-id="61fcb-622">Eine Methode `M` gilt schmaler als einen Delegattyp `D` wenn:</span><span class="sxs-lookup"><span data-stu-id="61fcb-622">A method `M` is considered narrower than a delegate type `D` if:</span></span>

* <span data-ttu-id="61fcb-623">Der Parametertyp `M` verfügt über eine erweiternde Konvertierung in den entsprechenden Parametertyps des `D`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-623">A parameter type of `M` has a widening conversion to the corresponding parameter type of `D`.</span></span>

* <span data-ttu-id="61fcb-624">Oder der Rückgabetyp, falls vorhanden, der `M` verfügt über eine einschränkende Konvertierung in den Rückgabetyp der `D`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-624">Or, the return type, if any, of `M` has a narrowing conversion to the return type of `D`.</span></span>

<span data-ttu-id="61fcb-625">Wenn Argumente des Typs die Zeiger auf den zugeordnet sind, gelten nur für Methoden mit der gleichen Anzahl von Typargumenten.</span><span class="sxs-lookup"><span data-stu-id="61fcb-625">If type arguments are associated with the method pointer, only methods with the same number of type arguments are considered.</span></span> <span data-ttu-id="61fcb-626">Wenn keine Typargumente die Zeiger auf den zugeordnet sind, wird ein Typrückschluss verwendet, beim Abgleich von Signaturen für eine generische Methode.</span><span class="sxs-lookup"><span data-stu-id="61fcb-626">If no type arguments are associated with the method pointer, type inference is used when matching signatures against a generic method.</span></span> <span data-ttu-id="61fcb-627">Im Gegensatz zu anderen normalen Typrückschluss der Rückgabetyp des Delegaten wird verwendet, wenn die Typargumente ableiten, aber Rückgabetypen sind immer noch nicht berücksichtigt, wenn die am wenigsten generische Überladung zu bestimmen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-627">Unlike other normal type inference, the return type of the delegate is used when inferring type arguments, but return types are still not considered when determining the least generic overload.</span></span> <span data-ttu-id="61fcb-628">Das folgende Beispiel zeigt die beiden Möglichkeiten zum Bereitstellen von Typargument für einen Delegaterstellungsausdruck:</span><span class="sxs-lookup"><span data-stu-id="61fcb-628">The following example shows both ways of supplying a type argument to a delegate-creation expression:</span></span>

```vb
Delegate Function D(s As String, i As Integer) As Integer
Delegate Function E() As Integer

Module Test
    Public Function F(Of T)(s As String, t1 As T) As T
    End Function

    Public Function G(Of T)() As T
    End Function

    Sub Main()
        Dim d1 As D = AddressOf f(Of Integer)    ' OK, type arg explicit
        Dim d2 As D = AddressOf f                ' OK, type arg inferred

        Dim e1 As E = AddressOf g(Of Integer)    ' OK, type arg explicit
        Dim e2 As E = AddressOf g                ' OK, infer from return
  End Sub
End Module
```

<span data-ttu-id="61fcb-629">Im obigen Beispiel wurde ein nicht generischer Delegattyp mit einer generischen Methode instanziiert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-629">In the above example, a non-generic delegate type was instantiated using a generic method.</span></span> <span data-ttu-id="61fcb-630">Es ist auch möglich, zum Erstellen einer Instanz eines erstellten Delegattyps mit einer generischen Methode.</span><span class="sxs-lookup"><span data-stu-id="61fcb-630">It is also possible to create an instance of a constructed delegate type using a generic method.</span></span> <span data-ttu-id="61fcb-631">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-631">For example:</span></span>

```vb
Delegate Function Predicate(Of U)(u1 As U, u2 As U) As Boolean

Module Test
    Function Compare(Of T)(t1 As List(of T), t2 As List(of T)) As Boolean
        ...
    End Function

    Sub Main()
        Dim p As Predicate(Of List(Of Integer))
        p = AddressOf Compare(Of Integer)
    End Sub
End Module
```

<span data-ttu-id="61fcb-632">Wenn das Argument für die delegaterstellung Ausdruck einer Lambda-Methode ist, muss der Lambda-Methode für die Signatur des Delegattyps sein.</span><span class="sxs-lookup"><span data-stu-id="61fcb-632">If the argument to the delegate-creation expression is a lambda method, the lambda method must be applicable to the signature of the delegate type.</span></span> <span data-ttu-id="61fcb-633">Ein Lambda-Methode `L` gilt für einen Delegattyp `D` wenn:</span><span class="sxs-lookup"><span data-stu-id="61fcb-633">A lambda method `L` is applicable to a delegate type `D` if:</span></span>

* <span data-ttu-id="61fcb-634">Wenn `L` verfügt über Parameter, `D` hat die gleiche Anzahl von Parametern.</span><span class="sxs-lookup"><span data-stu-id="61fcb-634">If `L` has parameters, `D` has the same number of parameters.</span></span> <span data-ttu-id="61fcb-635">(Wenn `L` enthält keine Parameter, die Parameter der `D` werden ignoriert.)</span><span class="sxs-lookup"><span data-stu-id="61fcb-635">(If `L` has no parameters, the parameters of `D` are ignored.)</span></span>

* <span data-ttu-id="61fcb-636">Die Parametertypen der `L` haben jeweils eine Konvertierung in den Typ des entsprechenden Parametertyps des `D`, und die Modifizierer (d. h. `ByRef`, `ByVal`) übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-636">The parameter types of `L` each have a conversion to the type of the corresponding parameter type of `D`, and their modifiers (i.e. `ByRef`, `ByVal`) match.</span></span>

* <span data-ttu-id="61fcb-637">Wenn `D` ist eine Funktion, die den Rückgabetyp der `L` verfügt über eine Konvertierung in den Rückgabetyp der `D`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-637">If `D` is a function, the return type of `L` has a conversion to the return type of `D`.</span></span> <span data-ttu-id="61fcb-638">(Wenn `D` ist eine Unterroutine, die den Rückgabewert der `L` wird ignoriert.)</span><span class="sxs-lookup"><span data-stu-id="61fcb-638">(If `D` is a subroutine, the return value of `L` is ignored.)</span></span>

<span data-ttu-id="61fcb-639">Wenn der Parameter einen Parameter vom Typ `L` weggelassen wird, klicken Sie dann den Typ des entsprechenden Parameters in `D` abgeleitet wird, wenn der Parameter der `L` Array oder NULL-Werte zulässt, Name-Modifizierer, wird ein Fehler während der Kompilierung ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="61fcb-639">If the parameter type of a parameter of `L` is omitted, then the type of the corresponding parameter in `D` is inferred; if the parameter of `L` has array or nullable name modifiers, a compile-time error results.</span></span> <span data-ttu-id="61fcb-640">Einmal alle die Parametertypen der `L` sind verfügbar, und klicken Sie dann der Typ des Ausdrucks in der Lambda-Methode abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-640">Once all of the parameter types of `L` are available, then the type of the expression in the lambda method is inferred.</span></span> <span data-ttu-id="61fcb-641">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-641">For example:</span></span>

```vb
Delegate Function F(x As Integer, y As Long) As Long

Module Test
    Sub Main()
        ' b inferred to Integer, c and return type inferred to Long
        Dim a As F = Function(b, c) b + c

        ' e and return type inferred to Integer, f inferred to Long
        Dim d As F = Function(e, f) e + CInt(f)
    End Sub
End Module
```

<span data-ttu-id="61fcb-642">In einigen Situationen, in denen Signatur des Delegaten nicht genau der Lambda-Methode oder die Signatur der Methode übereinstimmt, kann .NET Framework die Delegateerstellung nicht nativ unterstützt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-642">In some situations where delegate signature does not exactly match the lambda method or method signature, the .NET Framework may not support the delegate creation natively.</span></span> <span data-ttu-id="61fcb-643">In diesem Fall wird ein Lambda-Ausdruck-Methode verwendet, mit die beiden Methoden übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-643">In that situation, a lambda method expression is used to match the two methods.</span></span> <span data-ttu-id="61fcb-644">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-644">For example:</span></span>

```vb
Delegate Function IntFunc(x As Integer) As Integer

Module Test
    Function SquareString(x As String) As String
        Return CInt(x) * CInt(x)
    End Function 

    Sub Main()
        ' The following two lines are equivalent
        Dim a As New IntFunc(AddressOf SquareString)
        Dim b As New IntFunc( _
            Function(x As Integer) CInt(SquareString(CStr(x))))
    End Sub
End Module
```

<span data-ttu-id="61fcb-645">Das Ergebnis eines Ausdrucks eines ist eine Delegatinstanz, die sich auf die entsprechende Methode mit den zugehörigen Zielausdruck (sofern vorhanden) aus der Methode Zeigerausdruck bezieht.</span><span class="sxs-lookup"><span data-stu-id="61fcb-645">The result of a delegate-creation expression is a delegate instance that refers to the matching method with the associated target expression (if any) from the method pointer expression.</span></span> <span data-ttu-id="61fcb-646">Wenn der Zielausdruck als Werttyp typisiert ist, wird der Werttyp auf dem Systemheap kopiert, daran, dass ein Delegat nur auf eine Methode eines Objekts auf dem Heap kann.</span><span class="sxs-lookup"><span data-stu-id="61fcb-646">If the target expression is typed as a value type, then the value type is copied onto the system heap because a delegate can only point to a method of an object on the heap.</span></span> <span data-ttu-id="61fcb-647">Der Methode und des Objekts, auf die ein Delegat verweist, bleiben konstant, während der gesamten Lebensdauer des Delegaten.</span><span class="sxs-lookup"><span data-stu-id="61fcb-647">The method and object to which a delegate refers remain constant for the entire lifetime of the delegate.</span></span> <span data-ttu-id="61fcb-648">Das heißt, ist es nicht möglich, das Ziel oder ein Objekt eines Delegaten zu ändern, nachdem es erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="61fcb-648">In other words, it is not possible to change the target or object of a delegate after it has been created.</span></span>

### <a name="anonymous-object-creation-expressions"></a><span data-ttu-id="61fcb-649">Anonyme Objekterstellung-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-649">Anonymous Object-Creation Expressions</span></span>

<span data-ttu-id="61fcb-650">Ein Ausdruck der objekterstellung mit Memberinitialisierer kann der Typname auch ganz weglassen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-650">An object-creation expression with member initializers can also omit the type name entirely.</span></span>

```antlr
AnonymousObjectCreationExpression
    : 'New' ObjectMemberInitializer
    ;
```

<span data-ttu-id="61fcb-651">In diesem Fall wird ein anonymer Typ erstellt, basierend auf den Typen und Namen der Member als Teil des Ausdrucks initialisiert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-651">In that case, an anonymous type is constructed based on the types and names of the members initialized as a part of the expression.</span></span> <span data-ttu-id="61fcb-652">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-652">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim Customer = New With { .Name = "John Smith", .Age = 34 }

        Console.WriteLine(Customer.Name)
    End Sub
End Module
```

<span data-ttu-id="61fcb-653">Der Typ, der durch einen Ausdruck für die anonyme objekterstellung erstellt ist, eine Klasse, die keinen Namen hat, erbt direkt von `Object`, und enthält eine Reihe von Eigenschaften mit dem gleichen Namen wie die Elemente in der Liste der Member-Initialisierer zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-653">The type created by an anonymous object-creation expression is a class that has no name, inherits directly from `Object`, and has a set of properties with the same name as the members assigned to in the member initializer list.</span></span> <span data-ttu-id="61fcb-654">Der Typ jeder Eigenschaft wird mit den gleichen Regeln als lokale Variable Typrückschluss abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-654">The type of each property is inferred using the same rules as local variable type inference.</span></span> <span data-ttu-id="61fcb-655">Generierte, anonyme Typen überschreiben auch `ToString`, eine Zeichenfolgendarstellung für alle Elemente und deren Werte zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="61fcb-655">Generated anonymous types also override `ToString`, returning a string representation of all members and their values.</span></span> <span data-ttu-id="61fcb-656">(Das genaue Format dieser Zeichenfolge ist Gegenstand dieser Spezifikation).</span><span class="sxs-lookup"><span data-stu-id="61fcb-656">(The exact format of this string is beyond the scope of this specification).</span></span>

<span data-ttu-id="61fcb-657">Standardmäßig sind die Eigenschaften des anonymen Typs vom Lese-/ Schreibzugriff.</span><span class="sxs-lookup"><span data-stu-id="61fcb-657">By default, the properties generated by the anonymous type are read-write.</span></span> <span data-ttu-id="61fcb-658">Es ist möglich, eine Eigenschaft eines anonymen Typs als schreibgeschützt zu markieren, mithilfe der `Key` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="61fcb-658">It is possible to mark an anonymous type property as read-only by using the `Key` modifier.</span></span> <span data-ttu-id="61fcb-659">Die `Key` %(Dateiname) gibt an, dass das Feld verwendet werden kann, zur eindeutigen Identifizierung den Wert des anonymen Typs darstellt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-659">The `Key` modifier specifies that the field can be used to uniquely identify the value the anonymous type represents.</span></span> <span data-ttu-id="61fcb-660">Nicht nur die Eigenschaft schreibgeschützt, bewirkt außerdem, dass den anonymen Typ überschreiben `Equals` und `GetHashCode` und implementieren die Schnittstelle `System.IEquatable(Of T)` (Ausfüllen des anonymen Typs für `T`).</span><span class="sxs-lookup"><span data-stu-id="61fcb-660">In addition to making the property read-only, it also causes the anonymous type to override `Equals` and  `GetHashCode` and to implement the interface `System.IEquatable(Of T)` (filling in the anonymous type for `T`).</span></span> <span data-ttu-id="61fcb-661">Die Elemente werden wie folgt definiert:</span><span class="sxs-lookup"><span data-stu-id="61fcb-661">The members are defined as follows:</span></span>

<span data-ttu-id="61fcb-662">`Function Equals(obj As Object) As Boolean` und `Function Equals(val As T) As Boolean` werden implementiert, überprüfen, dass die beiden Instanzen des gleichen Typs sind, und klicken Sie dann verglichen wird, jede `Key` Member mit `Object.Equals`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-662">`Function Equals(obj As Object) As Boolean` and `Function Equals(val As T) As Boolean` are implemented by validating that the two instances are of the same type and then comparing each `Key` member using `Object.Equals`.</span></span> <span data-ttu-id="61fcb-663">Wenn alle `Key` Elemente gleich sind, klicken Sie dann `Equals` gibt `True`, andernfalls `Equals` gibt `False`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-663">If all `Key` members are equal, then `Equals` returns `True`, otherwise `Equals` returns `False`.</span></span>

<span data-ttu-id="61fcb-664">`Function GetHashCode() As Integer` wird implementiert, dass, auch wenn `Equals` ist "true" für zwei Instanzen des anonymen Typs, `GetHashCode` wird derselbe Wert zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="61fcb-664">`Function GetHashCode() As Integer` is implemented such that that if `Equals` is true for two instances of the anonymous type, then `GetHashCode` will return the same value.</span></span> <span data-ttu-id="61fcb-665">Der Hash wird gestartet, mit einem Ausgangswert, und klicken Sie dann für jede `Key` Member, in der Reihenfolge den Hash durch 31 multipliziert und fügt die `Key` Hashwert des Members (gebotenen `GetHashCode`) ist das Element kein Verweistyp oder Werttyp mit dem Wert des `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-665">The hash starts with a seed value and then, for each `Key` member, in order multiplies the hash by 31 and adds the `Key` member's hash value (provided by `GetHashCode`) if the member is not a reference type or nullable value type with the value of `Nothing`.</span></span>

<span data-ttu-id="61fcb-666">Beispielsweise muss der Typ in der Anweisung erstellt:</span><span class="sxs-lookup"><span data-stu-id="61fcb-666">For example, the type created in the statement:</span></span>

```vb
Dim zipState = New With { Key .ZipCode = 98112, .State = "WA" }
```

<span data-ttu-id="61fcb-667">erstellt eine Klasse, die in etwa wie folgt aussieht (obwohl es sich um eine genaue Implementierung möglicherweise unterscheiden):</span><span class="sxs-lookup"><span data-stu-id="61fcb-667">creates a class that looks approximately like this (although exact implementation may vary):</span></span>

```vb
Friend NotInheritable Class $Anonymous1
    Implements IEquatable(Of $Anonymous1)

    Private ReadOnly _zipCode As Integer
    Private _state As String

    Public Sub New(zipCode As Integer, state As String)
        _zipCode = zipcode
        _state = state
    End Sub

    Public ReadOnly Property ZipCode As Integer
        Get
            Return _zipCode
        End Get
    End Property

    Public Property State As String
        Get
            Return _state
        End Get
        Set (value As Integer)
            _state = value
        End Set
    End Property

    Public Overrides Function Equals(obj As Object) As Boolean
        Dim val As $Anonymous1 = TryCast(obj, $Anonymous1)
        Return Equals(val)
    End Function

    Public Overloads Function Equals(val As $Anonymous1) As Boolean _
        Implements IEquatable(Of $Anonymous1).Equals

        If val Is Nothing Then 
            Return False
        End If

        If Not Object.Equals(_zipCode, val._zipCode) Then 
            Return False
        End If

        Return True
    End Function

    Public Overrides Function GetHashCode() As Integer
        Dim hash As Integer = 0

        hash = hash Xor _zipCode.GetHashCode()

        Return hash
    End Function

    Public Overrides Function ToString() As String
        Return "{ Key .ZipCode = " & _zipCode & ", .State = " & _state & " }"
    End Function
End Class
```

<span data-ttu-id="61fcb-668">Um die Situation zu vereinfachen, in ein anonymer Typ aus den Feldern eines anderen Typs erstellt wird, können die Feldnamen direkt in Ausdrücken in den folgenden Fällen abgeleitet werden:</span><span class="sxs-lookup"><span data-stu-id="61fcb-668">To simplify the situation where an anonymous type is created from the fields of another type, field names can be inferred directly from expressions in the following cases:</span></span>

* <span data-ttu-id="61fcb-669">Einem einfachen Namensausdruck `x` leitet den Namen `x`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-669">A simple name expression `x` infers the name `x`.</span></span>

* <span data-ttu-id="61fcb-670">Ein Memberzugriffsausdruck `x.y` leitet den Namen `y`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-670">A member access expression `x.y` infers the name `y`.</span></span>

* <span data-ttu-id="61fcb-671">Ein Wörterbuch Lookup-Ausdruck `x!y` leitet den Namen `y`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-671">A dictionary lookup expression `x!y` infers the name `y`.</span></span>

* <span data-ttu-id="61fcb-672">Ein Ausdruck aufrufen oder einen Index ohne Argumente `x()` leitet den Namen `x`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-672">An invocation or index expression with no arguments `x()` infers the name `x`.</span></span>

* <span data-ttu-id="61fcb-673">Ein XML-Memberzugriffsausdruck `x.<y>`, `x...<y>`, `x.@y` leitet den Namen `y`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-673">An XML member access expression `x.<y>`, `x...<y>`, `x.@y` infers the name `y`.</span></span>

* <span data-ttu-id="61fcb-674">Ein XML-Memberzugriffsausdruck, der das Ziel eines Memberzugriffsausdrucks ist `x.<y>.z` leitet den Namen `z`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-674">An XML member access expression that is the target of a member access expression `x.<y>.z` infers the name `z`.</span></span>

* <span data-ttu-id="61fcb-675">Ein XML-Memberzugriffsausdruck, der das Ziel eines Aufrufs oder Index Ausdrucks ohne Argumente ist `x.<y>.z()` leitet den Namen `z`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-675">An XML member access expression that is the target of an invocation or index expression with no arguments `x.<y>.z()` infers the name `z`.</span></span>

* <span data-ttu-id="61fcb-676">Ein XML-Memberzugriffsausdruck, der das Ziel eines Aufrufs oder Index-Ausdrucks ist `x.<y>(0)` leitet den Namen `y`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-676">An XML member access expression that is the target of an invocation or index expression `x.<y>(0)` infers the name `y`.</span></span>

<span data-ttu-id="61fcb-677">Die Initialisierung wird als eine Zuweisung des Ausdrucks, der dem abgeleiteten Namen interpretiert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-677">The initializer is interpreted as an assignment of the expression to the inferred name.</span></span> <span data-ttu-id="61fcb-678">Die folgenden Initialisierer sind beispielsweise äquivalent:</span><span class="sxs-lookup"><span data-stu-id="61fcb-678">For example, the following initializers are equivalent:</span></span>

```vb
Class Address
    Public Street As String
    Public City As String
    Public State As String
    Public ZIP As String
End Class

Class C1
    Sub Test(a As Address)
        Dim cityState1 = New With { .City = a.City, .State = a.State }
        Dim cityState2 = New With { a.City, a.State }
    End Sub
End Class
```

<span data-ttu-id="61fcb-679">Wenn Sie ein Elementnamen abgeleitet wird, die steht in Konflikt mit einem vorhandenen Member des Typs, z. B. `GetHashCode`, tritt ein, auf ein Fehler zur Kompilierzeit.</span><span class="sxs-lookup"><span data-stu-id="61fcb-679">If a member name is inferred that conflicts with an existing member of the type, such as `GetHashCode`, then a compile time error occurs.</span></span> <span data-ttu-id="61fcb-680">Im Gegensatz zu regulären Standardmember-Initialisierer zulassen anonyme objekterstellung Ausdrücke nicht Member Initialisierer keine Zirkelverweise verwendet oder auf einen Member verweisen, bevor es initialisiert wurde.</span><span class="sxs-lookup"><span data-stu-id="61fcb-680">Unlike regular member initializers, anonymous object-creation expressions do not allow member initializers to have circular references, or to refer to a member before it has been initialized.</span></span> <span data-ttu-id="61fcb-681">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-681">For example:</span></span>

```vb
Module Test
    Sub Main()
        ' Error: Circular references
        Dim x = New With { .a = .b, .b = .a }

        ' Error: Referring to .b before it has been assigned to
        Dim y = New With { .a = .b, .b = 10 }

        ' Error: Referring to .a before it has been assigned to
        Dim z = New With { .a = .a }
    End Sub
End Module
```

<span data-ttu-id="61fcb-682">Wenn zwei anonyme Klasse erstellen Ausdrücke innerhalb der gleichen Methode auftreten, und die gleiche resultierende Form – ergeben, wenn die Reihenfolge der Eigenschaft, die Eigenschaftennamen und die Eigenschaft Typen alle überein: werden sie beide auf dieselbe anonyme Klasse verweisen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-682">If two anonymous class creation expressions occur within the same method and yield the same resulting shape -- if the property order, property names, and property types all match -- they will both refer to the same anonymous class.</span></span> <span data-ttu-id="61fcb-683">Der Methodenbereich, der eine Instanz oder einen freigegebenen Member-Variable mit einem Initialisierer ist der Konstruktor, in dem die Variable initialisiert wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-683">The method scope of an instance or shared member variable with an initializer is the constructor in which the variable is initialized.</span></span>

<span data-ttu-id="61fcb-684">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-684">__Note.__</span></span> <span data-ttu-id="61fcb-685">Es ist möglich, dass ein Compiler anonyme vereinheitlichen auswählen kann Typen weiter, z. B. wie auf der Assemblyebene, aber dies kann nicht als zuverlässig betrachtet werden zu diesem Zeitpunkt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-685">It is possible that a compiler may choose to unify anonymous types further, such as at the assembly level, but this cannot be relied upon at this time.</span></span>


## <a name="cast-expressions"></a><span data-ttu-id="61fcb-686">CAST-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-686">Cast Expressions</span></span>

<span data-ttu-id="61fcb-687">Cast-Ausdruck Wandelt einen Ausdruck in einen angegebenen Typ.</span><span class="sxs-lookup"><span data-stu-id="61fcb-687">A cast expression coerces an expression to a given type.</span></span> <span data-ttu-id="61fcb-688">Bestimmte Umwandlungsschlüsselwörter coerce-Ausdrücke in den primitiven Typen gehört.</span><span class="sxs-lookup"><span data-stu-id="61fcb-688">Specific cast keywords coerce expressions into the primitive types.</span></span> <span data-ttu-id="61fcb-689">Drei allgemeine Umwandlungsschlüsselwörter `CType`, `TryCast` und `DirectCast`, einen Ausdruck in einen Typ umgewandelt werden soll.</span><span class="sxs-lookup"><span data-stu-id="61fcb-689">Three general cast keywords, `CType`, `TryCast` and `DirectCast`, coerce an expression into a type.</span></span>

```antlr
CastExpression
    : 'DirectCast' OpenParenthesis Expression Comma TypeName CloseParenthesis
    | 'TryCast' OpenParenthesis Expression Comma TypeName CloseParenthesis
    | 'CType' OpenParenthesis Expression Comma TypeName CloseParenthesis
    | CastTarget OpenParenthesis Expression CloseParenthesis
    ;

CastTarget
    : 'CBool' | 'CByte' | 'CChar'  | 'CDate'  | 'CDec' | 'CDbl' | 'CInt'
    | 'CLng'  | 'CObj'  | 'CSByte' | 'CShort' | 'CSng' | 'CStr' | 'CUInt'
    | 'CULng' | 'CUShort'
    ;
```

<span data-ttu-id="61fcb-690">`DirectCast` und `TryCast` spezielle Verhalten aufweisen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-690">`DirectCast` and `TryCast` have special behaviors.</span></span> <span data-ttu-id="61fcb-691">Aus diesem Grund unterstützen sie nur systemeigene Konvertierungen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-691">Because of this, they only support native conversions.</span></span> <span data-ttu-id="61fcb-692">Darüber hinaus den Zieltyp in einer `TryCast` Ausdruck handelt es sich nicht um einen Werttyp.</span><span class="sxs-lookup"><span data-stu-id="61fcb-692">Additionally, the target type in a `TryCast` expression cannot be a value type.</span></span> <span data-ttu-id="61fcb-693">Benutzerdefinierte Operatoren für die Konvertierung nicht beim berücksichtigt `DirectCast` oder `TryCast` verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-693">User-defined conversion operators are not considered when `DirectCast` or `TryCast` is used.</span></span> <span data-ttu-id="61fcb-694">(__Beachten.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-694">(__Note.__</span></span> <span data-ttu-id="61fcb-695">Die Konvertierung festgelegt, die `DirectCast` und `TryCast` Unterstützung sind eingeschränkt, da diese Konvertierungen von "systemeigener CLR" implementieren.</span><span class="sxs-lookup"><span data-stu-id="61fcb-695">The conversion set that `DirectCast` and `TryCast` support are restricted because they implement "native CLR" conversions.</span></span> <span data-ttu-id="61fcb-696">Der Zweck der `DirectCast` wird zum Bereitstellen der Funktionen der Anweisung "Unboxing", während der Zweck der `TryCast` besteht darin, die Funktionalität der Anweisung "Isinst" bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-696">The purpose of `DirectCast` is to provide the functionality of the "unbox" instruction, while the purpose of `TryCast` is to provide the functionality of the "isinst" instruction.</span></span> <span data-ttu-id="61fcb-697">Da sie auf der CLR-Anweisungen zuordnen, widerspricht Unterstützung von Konvertierungen, die nicht direkt von der CLR unterstützt den beabsichtigten Zweck.)</span><span class="sxs-lookup"><span data-stu-id="61fcb-697">Since they map onto CLR instructions, supporting conversions not directly supported by the CLR would defeat the intended purpose.)</span></span>

<span data-ttu-id="61fcb-698">`DirectCast` Konvertiert die Ausdrücke, die als typisiert sind `Object` anders als bei `CType`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-698">`DirectCast` converts expressions that are typed as `Object` differently than `CType`.</span></span> <span data-ttu-id="61fcb-699">Bei der Konvertierung eines Ausdrucks vom Typ `Object` , dessen Typ zur Laufzeit ist ein primitiver Wert `DirectCast` löst eine `System.InvalidCastException` -Ausnahme aus, wenn der angegebene Typ nicht identisch mit der Run-Time-Typ des Ausdrucks ist oder eine `System.NullReferenceException` Wenn der Ausdruck ergibt `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-699">When converting an expression of type `Object` whose run-time type is a primitive value type, `DirectCast` throws a `System.InvalidCastException` exception if the specified type is not the same as the run-time type of the expression or a `System.NullReferenceException` if the expression evaluates to `Nothing`.</span></span> <span data-ttu-id="61fcb-700">(__Beachten.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-700">(__Note.__</span></span> <span data-ttu-id="61fcb-701">Wie bereits erwähnt, `DirectCast` Maps direkt auf der CLR-Anweisung "Unboxing" Wenn der Typ des Ausdrucks ist `Object`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-701">As noted above, `DirectCast` maps directly onto the CLR instruction "unbox" when the type of the expression is `Object`.</span></span> <span data-ttu-id="61fcb-702">Im Gegensatz dazu `CType` wird in einen Aufruf an ein Laufzeit-Hilfsprogramm für die Konvertierung erforderlich sind, sodass Konvertierungen zwischen primitiven Typen unterstützt werden können.</span><span class="sxs-lookup"><span data-stu-id="61fcb-702">In contrast, `CType` turns into a call to a runtime helper to do the conversion so that conversions between primitive types can be supported.</span></span> <span data-ttu-id="61fcb-703">Im Fall bei einer `Object` Ausdruck konvertiert wird auf einen primitive-Wert-Typ und den Typ der tatsächlichen Instanz Übereinstimmung den Zieltyp `DirectCast` wird erheblich schneller sein als `CType`.)</span><span class="sxs-lookup"><span data-stu-id="61fcb-703">In the case when an `Object` expression is being converted to a primitive value type and the type of the actual instance match the target type, `DirectCast` will be significantly faster than `CType`.)</span></span>

<span data-ttu-id="61fcb-704">`TryCast` Konvertiert die Ausdrücke, aber löst keine Ausnahme aus, wenn der Ausdruck in den Zieltyp konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="61fcb-704">`TryCast` converts expressions but does not throw an exception if the expression cannot be converted to the target type.</span></span> <span data-ttu-id="61fcb-705">Stattdessen `TryCast` führt zu `Nothing` Wenn der Ausdruck zur Laufzeit konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="61fcb-705">Instead, `TryCast` will result in `Nothing` if the expression cannot be converted at runtime.</span></span> <span data-ttu-id="61fcb-706">(__Beachten.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-706">(__Note.__</span></span> <span data-ttu-id="61fcb-707">Wie bereits erwähnt, `TryCast` direkt auf der CLR-Anweisung "Isinst" zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-707">As noted above, `TryCast` maps directly onto the CLR instruction "isinst".</span></span> <span data-ttu-id="61fcb-708">Durch die Kombination der typüberprüfung und die Konvertierung in einen einzelnen Vorgang, `TryCast` möglich günstiger als auch eine `TypeOf ... Is` und dann eine `CType`.)</span><span class="sxs-lookup"><span data-stu-id="61fcb-708">By combining the type check and the conversion into a single operation, `TryCast` can be cheaper than doing a `TypeOf ... Is` and then a `CType`.)</span></span>

<span data-ttu-id="61fcb-709">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-709">For example:</span></span>

```vb
Interface ITest
    Sub Test()
End Interface

Module Test
    Sub Convert(o As Object)
        Dim i As ITest = TryCast(o, ITest)

        If i IsNot Nothing Then
            i.Test()
        End If
    End Sub
End Module
```

<span data-ttu-id="61fcb-710">Wenn keine Konvertierung vom Typ des Ausdrucks in den angegebenen Typ vorhanden ist, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-710">If no conversion exists from the type of the expression to the specified type, a compile-time error occurs.</span></span> <span data-ttu-id="61fcb-711">Andernfalls wird der Ausdruck wird als Wert klassifiziert, und das Ergebnis ist der Wert, der von der Konvertierung erstellt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-711">Otherwise, the expression is classified as a value and the result is the value produced by the conversion.</span></span>


## <a name="operator-expressions"></a><span data-ttu-id="61fcb-712">Operatorausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-712">Operator Expressions</span></span>

<span data-ttu-id="61fcb-713">Es gibt zwei Arten von Operatoren.</span><span class="sxs-lookup"><span data-stu-id="61fcb-713">There are two kinds of operators.</span></span> <span data-ttu-id="61fcb-714">*Unäre Operatoren* verwenden einen Operanden aus, und verwenden Sie die Präfixnotation (z. B. `-x`).</span><span class="sxs-lookup"><span data-stu-id="61fcb-714">*Unary operators* take one operand and use prefix notation (for example, `-x`).</span></span> <span data-ttu-id="61fcb-715">*Binäre Operatoren* umfassen zwei Operanden, und verwenden Sie die Infix-Notation (z. B. `x + y`).</span><span class="sxs-lookup"><span data-stu-id="61fcb-715">*Binary operators* take two operands and use infix notation (for example, `x + y`).</span></span> <span data-ttu-id="61fcb-716">Mit Ausnahme von relationalen Operatoren, die ergeben immer `Boolean`, einen Operator für ein bestimmter Typ dieses Typs führt definiert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-716">With the exception of the relational operators, which always result in `Boolean`, an operator defined for a particular type results in that type.</span></span> <span data-ttu-id="61fcb-717">Die Operanden zu einem Operator müssen immer als Wert klassifiziert werden; Das Ergebnis eines Ausdrucks Operator wird als Wert klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-717">The operands to an operator must always be classified as a value; the result of an operator expression is classified as a value.</span></span>

```antlr
OperatorExpression
    : ArithmeticOperatorExpression
    | RelationalOperatorExpression
    | LikeOperatorExpression
    | ConcatenationOperatorExpression
    | ShortCircuitLogicalOperatorExpression
    | LogicalOperatorExpression
    | ShiftOperatorExpression
    | AwaitOperatorExpression
    ;
```

### <a name="operator-precedence-and-associativity"></a><span data-ttu-id="61fcb-718">Operatorrangfolge und Assoziativität</span><span class="sxs-lookup"><span data-stu-id="61fcb-718">Operator Precedence and Associativity</span></span>

<span data-ttu-id="61fcb-719">Wenn ein Ausdruck mit mehreren binäre Operatoren, enthält die *Rangfolge* steuert, der Operatoren die Reihenfolge, in dem die einzelnen binären Operatoren ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-719">When an expression contains multiple binary operators, the *precedence* of the operators controls the order in which the individual binary operators are evaluated.</span></span> <span data-ttu-id="61fcb-720">Der Ausdruck `x + y * z` wird z.B. als `x + (y * z)` ausgewertet, da der `*`-Operator Vorrang vor dem `+`-Operator hat.</span><span class="sxs-lookup"><span data-stu-id="61fcb-720">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the `+` operator.</span></span> <span data-ttu-id="61fcb-721">In der folgende Tabelle werden die binären Operatoren in absteigender Rangfolge aufgelistet:</span><span class="sxs-lookup"><span data-stu-id="61fcb-721">The following table lists the binary operators in descending order of precedence:</span></span>


| <span data-ttu-id="61fcb-722">__Kategorie__</span><span class="sxs-lookup"><span data-stu-id="61fcb-722">__Category__</span></span>     | <span data-ttu-id="61fcb-723">__Operatoren__</span><span class="sxs-lookup"><span data-stu-id="61fcb-723">__Operators__</span></span>                                          | 
|------------------|--------------------------------------------------------|
| <span data-ttu-id="61fcb-724">Primär</span><span class="sxs-lookup"><span data-stu-id="61fcb-724">Primary</span></span>          | <span data-ttu-id="61fcb-725">Alle nicht-Operator-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-725">All non-operator expressions</span></span>                           |
| <span data-ttu-id="61fcb-726">Await-</span><span class="sxs-lookup"><span data-stu-id="61fcb-726">Await</span></span>            | `Await`                                                |
| <span data-ttu-id="61fcb-727">Exponentiation</span><span class="sxs-lookup"><span data-stu-id="61fcb-727">Exponentiation</span></span>   | `^`                                                    |
| <span data-ttu-id="61fcb-728">Unäre Negation</span><span class="sxs-lookup"><span data-stu-id="61fcb-728">Unary negation</span></span>   | <span data-ttu-id="61fcb-729">`+`, `-`</span><span class="sxs-lookup"><span data-stu-id="61fcb-729">`+`, `-`</span></span>                                               |
| <span data-ttu-id="61fcb-730">Multiplikativ</span><span class="sxs-lookup"><span data-stu-id="61fcb-730">Multiplicative</span></span>   | <span data-ttu-id="61fcb-731">`*`, `/`</span><span class="sxs-lookup"><span data-stu-id="61fcb-731">`*`, `/`</span></span>                                               |
| <span data-ttu-id="61fcb-732">Ganzzahldivision</span><span class="sxs-lookup"><span data-stu-id="61fcb-732">Integer division</span></span> | `\`                                                    |
| <span data-ttu-id="61fcb-733">Modulooperator</span><span class="sxs-lookup"><span data-stu-id="61fcb-733">Modulus</span></span>          | `Mod`                                                  |
| <span data-ttu-id="61fcb-734">Additiv</span><span class="sxs-lookup"><span data-stu-id="61fcb-734">Additive</span></span>         | <span data-ttu-id="61fcb-735">`+`, `-`</span><span class="sxs-lookup"><span data-stu-id="61fcb-735">`+`, `-`</span></span>                                               |
| <span data-ttu-id="61fcb-736">Verkettung</span><span class="sxs-lookup"><span data-stu-id="61fcb-736">Concatenation</span></span>    | `&`                                                    |
| <span data-ttu-id="61fcb-737">Shift</span><span class="sxs-lookup"><span data-stu-id="61fcb-737">Shift</span></span>            | <span data-ttu-id="61fcb-738">`<<`, `>>`</span><span class="sxs-lookup"><span data-stu-id="61fcb-738">`<<`, `>>`</span></span>                                             |
| <span data-ttu-id="61fcb-739">Relational</span><span class="sxs-lookup"><span data-stu-id="61fcb-739">Relational</span></span>       | <span data-ttu-id="61fcb-740">`=`, `<>`, `<`, `>`, `<=`, `>=`, `Like`, `Is`, `IsNot`</span><span class="sxs-lookup"><span data-stu-id="61fcb-740">`=`, `<>`, `<`, `>`, `<=`, `>=`, `Like`, `Is`, `IsNot`</span></span> |
| <span data-ttu-id="61fcb-741">Logisches NOT</span><span class="sxs-lookup"><span data-stu-id="61fcb-741">Logical NOT</span></span>      | `Not`                                                  |
| <span data-ttu-id="61fcb-742">Logisches AND</span><span class="sxs-lookup"><span data-stu-id="61fcb-742">Logical AND</span></span>      | <span data-ttu-id="61fcb-743">`And`, `AndAlso`</span><span class="sxs-lookup"><span data-stu-id="61fcb-743">`And`, `AndAlso`</span></span>                                       |
| <span data-ttu-id="61fcb-744">Logisches OR</span><span class="sxs-lookup"><span data-stu-id="61fcb-744">Logical OR</span></span>       | <span data-ttu-id="61fcb-745">`Or`, `OrElse`</span><span class="sxs-lookup"><span data-stu-id="61fcb-745">`Or`, `OrElse`</span></span>                                         |
| <span data-ttu-id="61fcb-746">Logisches XOR</span><span class="sxs-lookup"><span data-stu-id="61fcb-746">Logical XOR</span></span>      | `Xor`                                                  |

<span data-ttu-id="61fcb-747">Wenn ein Ausdruck zwei Operatoren mit gleicher Rangfolge, enthält die *Assoziativität* steuert, der Operatoren die Reihenfolge, in dem die Vorgänge ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-747">When an expression contains two operators with the same precedence, the *associativity* of the operators controls the order in which the operations are performed.</span></span> <span data-ttu-id="61fcb-748">Alle binäre Operatoren sind linksassoziativ, was bedeutet, dass Vorgänge von links nach rechts ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-748">All binary operators are left-associative, meaning that operations are performed from left to right.</span></span> <span data-ttu-id="61fcb-749">Rangfolge und Assoziativität können mit der Ausdrücke in Klammern gesteuert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-749">Precedence and associativity can be controlled using parenthetical expressions.</span></span>

### <a name="object-operands"></a><span data-ttu-id="61fcb-750">Objekt Operanden</span><span class="sxs-lookup"><span data-stu-id="61fcb-750">Object Operands</span></span>

<span data-ttu-id="61fcb-751">Zusätzlich zu den regulären Typen, die jeder Operator unterstützt, unterstützt alle Operatoren Operanden vom Typ `Object`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-751">In addition to the regular types supported by each operator, all operators support operands of type `Object`.</span></span> <span data-ttu-id="61fcb-752">Operatoren angewendet werden, um `Object` Operanden auf ähnliche Weise behandelt werden, um die Methodenaufrufe, die auf `Object` Werte: ein spät gebundenen Methodenaufruf kann ausgewählt werden, in diesem Fall die Gültigkeit der Kompilierzeittyp, anstatt den Laufzeittyp der Operanden bestimmt und der Typ des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="61fcb-752">Operators applied to `Object` operands are handled similarly to method calls made on `Object` values: a late-bound method call might be chosen, in which case the run-time type of the operands, rather than the compile-time type, determines the validity and type of the operation.</span></span> <span data-ttu-id="61fcb-753">Wenn strikte Semantik, durch die kompilierungsumgebung oder durch angegeben werden `Option Strict`, alle Operatoren mit Operanden vom Typ `Object` dazu führen, dass einen Fehler während der Kompilierung mit Ausnahme der `TypeOf...Is`, `Is` und `IsNot` Operatoren.</span><span class="sxs-lookup"><span data-stu-id="61fcb-753">If strict semantics are specified by the compilation environment or by `Option Strict`, any operators with operands of type `Object` cause a compile-time error, except for the `TypeOf...Is`, `Is` and `IsNot` operators.</span></span>

<span data-ttu-id="61fcb-754">Operator-Lösung fest, dass es sich bei ein Vorgang spät gebundene ausgeführt werden soll, ist das Ergebnis des Vorgangs das Ergebnis der Anwendung des Operators, der die Operandentypen, wenn die Runtime-Typen der Operanden Typen sind, die durch den Operator unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-754">When operator resolution determines that an operation should be performed late-bound, the outcome of the operation is the result of applying the operator to the operand types if the run-time types of the operands are types that are supported by the operator.</span></span> <span data-ttu-id="61fcb-755">Der Wert `Nothing` so behandelt, als der Standardwert des Typs von der andere Operand in einem Ausdruck für binäre Operatoren.</span><span class="sxs-lookup"><span data-stu-id="61fcb-755">The value `Nothing` is treated as the default value of the type of the other operand in a binary operator expression.</span></span> <span data-ttu-id="61fcb-756">In einem Ausdruck für unäre Operatoren oder wenn beide Operanden sind `Nothing` in einem binären Operator-Ausdruck, der Typ des Vorgangs ist `Integer` oder der nur Ergebnistyp, der den Operator an, wenn der Operator nicht führt `Integer`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-756">In a unary operator expression, or if both operands are `Nothing` in a binary operator expression, the type of the operation is `Integer` or the only result type of the operator, if the operator does not result in `Integer`.</span></span> <span data-ttu-id="61fcb-757">Das Ergebnis des Vorgangs wird immer dann an umgewandelt `Object`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-757">The result of the operation is always then cast back to `Object`.</span></span> <span data-ttu-id="61fcb-758">Wenn die Operandentypen kein gültigen Operator, haben eine `System.InvalidCastException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="61fcb-758">If the operand types have no valid operator, a `System.InvalidCastException` exception is thrown.</span></span> <span data-ttu-id="61fcb-759">Konvertierungen zur Laufzeit werden ausgeführt, unabhängig von, ob sie implizite oder explizite befinden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-759">Conversions at run time are done without regard to whether they are implicit or explicit.</span></span>

<span data-ttu-id="61fcb-760">Wenn das Ergebnis einer numerischen binären Operation eine Überlaufausnahme auslösen (unabhängig davon, ob Überprüfungen auf Ganzzahlüberlauf aktiviert oder deaktiviert ist) erzeugt wird, wird der Ergebnistyp nach Möglichkeit auf den nächsten größeren numerischen Typ heraufgestuft.</span><span class="sxs-lookup"><span data-stu-id="61fcb-760">If the result of a numeric binary operation would produce an overflow exception (regardless of whether integer overflow checking is on or off), then the result type is promoted to the next wider numeric type, if possible.</span></span> <span data-ttu-id="61fcb-761">Beachten Sie z. B. folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="61fcb-761">For example, consider the following code:</span></span>

```vb
Module Test
    Sub Main()
        Dim o As Object = CObj(CByte(2)) * CObj(CByte(255))

        Console.WriteLine(o.GetType().ToString() & " = " & o)
    End Sub
End Module
```

<span data-ttu-id="61fcb-762">Gibt das folgende Ergebnis:</span><span class="sxs-lookup"><span data-stu-id="61fcb-762">It prints the following result:</span></span>

```
System.Int16 = 512
```

<span data-ttu-id="61fcb-763">Wenn keine größeren numerischer Typ verfügbar ist, für die Anzahl, ist eine `System.OverflowException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="61fcb-763">If no wider numeric type is available to hold the number, a `System.OverflowException` exception is thrown.</span></span>

### <a name="operator-resolution"></a><span data-ttu-id="61fcb-764">Operator-Lösung</span><span class="sxs-lookup"><span data-stu-id="61fcb-764">Operator Resolution</span></span>

<span data-ttu-id="61fcb-765">Wenn ein Operatortyp und einen Satz von Operanden, bestimmt Operator Auflösung, welche Operators, der für den Operanden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-765">Given an operator type and a set of operands, operator resolution determines which operator to use for the operands.</span></span> <span data-ttu-id="61fcb-766">Beim Auflösen von Operatoren werden benutzerdefinierte Operatoren zuerst mit den folgenden Schritten berücksichtigt werden:</span><span class="sxs-lookup"><span data-stu-id="61fcb-766">When resolving operators, user-defined operators will be considered first, using the following steps:</span></span>

1. <span data-ttu-id="61fcb-767">Zunächst werden alle möglichen Operatoren gesammelt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-767">First, all of the candidate operators are collected.</span></span> <span data-ttu-id="61fcb-768">Die Candidate-Operatoren sind alle benutzerdefinierten Operatoren die bestimmten Operator-Typs in den Quelltyp und alle benutzerdefinierten Operatoren des bestimmten Typs in den Zieltyp.</span><span class="sxs-lookup"><span data-stu-id="61fcb-768">The candidate operators are all of the user-defined operators of the particular operator type in the source type and all of the user-defined operators of the particular type in the target type.</span></span> <span data-ttu-id="61fcb-769">Wenn die Quell- und Zieltyp verknüpft sind, werden allgemeine Operatoren nur einmal berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-769">If the source type and destination type are related, common operators are only considered once.</span></span>

2. <span data-ttu-id="61fcb-770">Anschließend wird die Auflösung von funktionsüberladungen, die Operatoren und Operanden, um das spezifischste Operator auszuwählen angewendet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-770">Then, overload resolution is applied to the operators and operands to select the most specific operator.</span></span> <span data-ttu-id="61fcb-771">Im Fall von binären Operatoren kann dies zu einer spät gebundenen Aufruf führen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-771">In the case of binary operators, this may result in a late-bound call.</span></span>

<span data-ttu-id="61fcb-772">Wenn die Candidate-Operatoren für einen Typ sammeln `T?`, die Operatoren des Typs `T` stattdessen verwendet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-772">When collecting the candidate operators for a type `T?`, the operators of type `T` are used instead.</span></span> <span data-ttu-id="61fcb-773">Einer der `T`des benutzerdefinierten Operatoren, bei denen nur die nicht auf NULL festlegbare Werttypen werden auch aufgehoben.</span><span class="sxs-lookup"><span data-stu-id="61fcb-773">Any of `T`'s user-defined operators that involve only non-nullable value types are also lifted.</span></span> <span data-ttu-id="61fcb-774">Ein Operator transformierten verwendet die NULL-Werte zulässt Version des Werttypen an, mit der Ausnahme die Rückgabetypen der `IsTrue` und `IsFalse` (muss `Boolean`).</span><span class="sxs-lookup"><span data-stu-id="61fcb-774">A lifted operator uses the nullable version of any value types, with the exception the return types of `IsTrue` and `IsFalse` (which must be `Boolean`).</span></span> <span data-ttu-id="61fcb-775">Transformierten Operatoren werden ausgewertet, durch die Konvertierung von Operanden in ihre Version ist keine NULL-Werte zulässt, evaluieren den benutzerdefinierten Operator, und klicken Sie dann das Ergebnis zu konvertieren und geben Sie dann auf die Version der NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-775">Lifted operators are evaluated by converting the operands to their non-nullable version, then evaluating the user-defined operator and then converting the result type to its nullable version.</span></span> <span data-ttu-id="61fcb-776">Wenn dieses Operand ist `Nothing`, das Ergebnis des Ausdrucks ist ein Wert von `Nothing` als der auf NULL festlegbare Version des Ergebnistyps typisiert ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-776">If ether operand is `Nothing`, the result of the expression is a value of `Nothing` typed as the nullable version of the result type.</span></span> <span data-ttu-id="61fcb-777">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-777">For example:</span></span>

```vb
Structure T
    ...
End Structure

Structure S
    Public Shared Operator +(ByVal op1 As S, ByVal op2 As T) As T
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim x As S?
        Dim y, z As T?

        ' Valid, as S + T = T is lifted to S? + T? = T?
        z = x + y 
    End Sub
End Module
```

<span data-ttu-id="61fcb-778">Wenn der Operator ein binärer Operator ist und einer der Operanden Verweistyp ist, auch der Operator angehoben wird, aber eine Bindung an den Operator erzeugt einen Fehler.</span><span class="sxs-lookup"><span data-stu-id="61fcb-778">If the operator is a binary operator and one of the operands is reference type, the operator is also lifted, but any binding to the operator produces an error.</span></span> <span data-ttu-id="61fcb-779">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-779">For example:</span></span>

```vb
Structure S1
    Public F1 As Integer

    Public Shared Operator +(left As S1, right As String) As S1
       ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim a? As S1
        Dim s As String
        
        ' Error: '+' is not defined for S1? and String
        a = a + s
    End Sub
End Module
```

<span data-ttu-id="61fcb-780">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-780">__Note.__</span></span> <span data-ttu-id="61fcb-781">Diese Regel vorhanden ist, da gab es berücksichtigt, ob wir möchten Verweistypen Null-Weitergabe in einer zukünftigen Version wird in der Groß-/Kleinschreibung des Verhaltens im Fall von binären Operatoren zwischen den beiden Typen ändern würde.</span><span class="sxs-lookup"><span data-stu-id="61fcb-781">This rule exists because there has been consideration whether we wish to add null-propagating reference types in a future version, in which case the behavior in the case of binary operators between the two types would change.</span></span>

<span data-ttu-id="61fcb-782">Wie bei Konvertierungen werden benutzerdefinierte Operatoren, immer über transformierten Operatoren bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-782">As with conversions, user-defined operators are always preferred over lifted operators.</span></span>

<span data-ttu-id="61fcb-783">Beim Auflösen von Operatoren überladen werden, möglicherweise gibt es Unterschiede zwischen Klassen, die in Visual Basic definiert und in anderen Sprachen definiert:</span><span class="sxs-lookup"><span data-stu-id="61fcb-783">When resolving overloaded operators, there may be differences between classes defined in Visual Basic and those defined in other languages:</span></span>

* <span data-ttu-id="61fcb-784">In anderen Sprachen `Not`, `And`, und `Or` kann sowohl als logische und bitweise Operatoren überladen werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-784">In other languages, `Not`, `And`, and `Or` may be overloaded both as logical operators and bitwise operators.</span></span> <span data-ttu-id="61fcb-785">Nach dem Importieren aus einer externen Assembly ist eine der Formen als gültige Überladung für diese Operatoren akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-785">Upon import from an external assembly, either form is accepted as a valid overload for these operators.</span></span> <span data-ttu-id="61fcb-786">Allerdings wird für einen Typ, der logische und bitweise Operatoren definiert, nur die bitweise Implementierung berücksichtigt werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-786">However, for a type which defines both logical and bitwise operators, only the bitwise implementation will be considered.</span></span>

* <span data-ttu-id="61fcb-787">In anderen Sprachen `>>` und `<<` kann sowohl als mit und ohne Vorzeichen Operatoren überladen werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-787">In other languages, `>>` and `<<` may be overloaded both as signed operators and unsigned operators.</span></span> <span data-ttu-id="61fcb-788">Nach dem Importieren aus einer externen Assembly wird entweder Formular als gültige Überladung akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-788">Upon import from an external assembly, either form is accepted as a valid overload.</span></span> <span data-ttu-id="61fcb-789">Für einen Typ, der mit und ohne Vorzeichen Operatoren definiert: wird jedoch nur die signierte Implementierung berücksichtigt werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-789">However, for a type which defines both signed and unsigned operators, only the signed implementation will be considered.</span></span>

* <span data-ttu-id="61fcb-790">Wenn keine benutzerdefinierten Operator auf die Operanden am spezifischsten ist, werden dann systeminterne Operatoren betrachtet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-790">If no user-defined operator is most specific to the operands, then intrinsic operators will be considered.</span></span> <span data-ttu-id="61fcb-791">Wenn keine systeminternen Operators aus der für den Operanden definiert ist, und entweder Operand vom Typ Objekt ist wird dann der Operator spät gebundene aufgelöst wird. Andernfalls führt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-791">If no intrinsic operator is defined for the operands and either operand has type Object then the operator will be resolved late-bound; otherwise,  a compile-time error results.</span></span>

<span data-ttu-id="61fcb-792">In früheren Versionen von Visual Basic Wenn es genau ein Operand vom Typ "Object" und keine anwendbaren benutzerdefinierte Operatoren und keine anwendbaren systeminternen Operatoren wurde, war es ein Fehler.</span><span class="sxs-lookup"><span data-stu-id="61fcb-792">In prior versions of Visual Basic, if there was exactly one operand of type Object, and no applicable user-defined operators, and no applicable intrinsic operators, then it was an error.</span></span> <span data-ttu-id="61fcb-793">Ab Visual Basic-11 ist es jetzt spät gebundene behoben.</span><span class="sxs-lookup"><span data-stu-id="61fcb-793">As of Visual Basic 11, it is now resolved late-bound.</span></span> <span data-ttu-id="61fcb-794">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-794">For example:</span></span>

```vb
Module Module1
  Sub Main()
      Dim p As Object = Nothing
      Dim U As New Uri("http://www.microsoft.com")
      Dim j = U * p  ' is now resolved late-bound
   End Sub
End Module
```

<span data-ttu-id="61fcb-795">Ein Typ `T` , bei dem eine systeminterne Funktion Operator definiert auch dieser Operator für `T?`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-795">A type `T` that has an intrinsic operator also defines that same operator for `T?`.</span></span> <span data-ttu-id="61fcb-796">Das Ergebnis des Operators für `T?` werden dieselbe wie für `T`, mit dem Unterschied, dass wenn ein Operand `Nothing`, das Ergebnis des Operators `Nothing` (d. h. der null-Wert wird weitergegeben).</span><span class="sxs-lookup"><span data-stu-id="61fcb-796">The result of the operator on `T?` will be the same as for `T`, except that if either operand is `Nothing`, the result of the operator will be `Nothing` (i.e. the null value is propagated).</span></span> <span data-ttu-id="61fcb-797">Im Rahmen der Auflösen des Typs eines Vorgangs der `?` wird entfernt, der Typ des Vorgangs über alle Operanden, die sie haben, wird bestimmt, und ein `?` wird in den Typ des Vorgangs hinzugefügt, wenn einer der Operanden auf NULL festlegbare Werttypen wurden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-797">For the purposes of resolving the type of an operation, the `?` is removed from any operands that have them, the type of the operation is determined, and a `?` is added to the type of the operation if any of the operands were nullable value types.</span></span> <span data-ttu-id="61fcb-798">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-798">For example:</span></span>

```vb
Dim v1? As Integer = 10
Dim v2 As Long = 20

' Type of operation will be Long?
Console.WriteLine(v1 + v2)
```

<span data-ttu-id="61fcb-799">Jeder Operator werden die systeminternen Typen, die, denen Sie für definiert ist, und den Typ des ausgeführten Vorgangs die Operandentypen aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-799">Each operator lists the intrinsic types it is defined for and the type of the operation performed given the operand types.</span></span> <span data-ttu-id="61fcb-800">Das Ergebnis des Typs eines systeminterne Vorgangs: folgende allgemeine Regeln</span><span class="sxs-lookup"><span data-stu-id="61fcb-800">The result of type of a intrinsic operation follows these general rules:</span></span>

* <span data-ttu-id="61fcb-801">Wenn alle Operanden vom selben Typ sind und der Operator für den Typ definiert ist, erfolgt keine Konvertierung, und der Operator für diesen Typ wird verwendet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-801">If all operands are of the same type, and the operator is defined for the type, then no conversion occurs and the operator for that type is used.</span></span>

* <span data-ttu-id="61fcb-802">Alle Operanden, dessen Typ nicht für den Operator definiert ist, wird konvertiert mithilfe der folgenden Schritte aus, und der Operator ist für die neuen Typen aufgelöst:</span><span class="sxs-lookup"><span data-stu-id="61fcb-802">Any operand whose type is not defined for the operator is converted using the following steps and the operator is resolved against the new types:</span></span>

  * <span data-ttu-id="61fcb-803">Der Operand wird konvertiert, zum nächsten allgemeinsten Typ, der für den Operanden und den Operator definiert ist und bei denen es implizit konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-803">The operand is converted to the next widest type that is defined for both the operator and the operand and to which it is implicitly convertible.</span></span>

  * <span data-ttu-id="61fcb-804">Wenn kein solcher Typ vorhanden ist, wird der Operand den Typ des nächsten engsten, der für den Operanden und den Operator definiert ist und bei denen es implizit konvertiert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-804">If there is no such type, then the operand is converted to the next narrowest type that is defined for both the operator and the operand and to which it is implicitly convertible.</span></span>

  * <span data-ttu-id="61fcb-805">Wenn kein solcher Typ vorhanden ist, oder die Konvertierung kann nicht durchgeführt, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-805">If there is no such type or the conversion cannot occur, a compile-time error occurs.</span></span>

* <span data-ttu-id="61fcb-806">Andernfalls werden die Operanden konvertiert, auf das breiter die Operandentypen und den Operator für diesen Typ wird verwendet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-806">Otherwise, the operands are converted to the wider of the operand types and the operator for that type is used.</span></span> <span data-ttu-id="61fcb-807">Wenn der engeren Operandentyp in größeren Operatortyp implizit konvertiert werden kann, tritt auf, ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-807">If the narrower operand type cannot be implicitly converted to the wider operator type, a compile-time error occurs.</span></span>

<span data-ttu-id="61fcb-808">Trotz dieser allgemeinen Regeln gibt es jedoch eine Anzahl von Sonderfälle in den Ergebnistabellen Operator aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-808">Despite these general rules, however, there are a number of special cases called out in the operator results tables.</span></span>

<span data-ttu-id="61fcb-809">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-809">__Note.__</span></span> <span data-ttu-id="61fcb-810">Formatierungsgründen, abkürzen der Operator den Tabellen des vordefinierten Namen, den ersten beiden Zeichen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-810">For formatting reasons, the operator type tables abbreviate the predefined names to their first two characters.</span></span> <span data-ttu-id="61fcb-811">Wird also "von" `Byte`, ist der "UI" `UInteger`, ist "St" `String`usw. "Err" bedeutet, die dass es keinen Vorgang für die angegebene Operandentypen definiert ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-811">So "By" is `Byte`, "UI" is `UInteger`, "St" is `String`, etc. "Err" means that there is no operation defined for the given operand types.</span></span>

## <a name="arithmetic-operators"></a><span data-ttu-id="61fcb-812">Arithmetische Operatoren</span><span class="sxs-lookup"><span data-stu-id="61fcb-812">Arithmetic Operators</span></span>

<span data-ttu-id="61fcb-813">Die `*`, `/`, `\`, `^`, `Mod`, `+`, und `-` Operatoren sind die *arithmetische Operatoren*.</span><span class="sxs-lookup"><span data-stu-id="61fcb-813">The `*`, `/`, `\`, `^`, `Mod`, `+`, and `-` operators are the *arithmetic operators*.</span></span>

```antlr
ArithmeticOperatorExpression
    : UnaryPlusExpression
    | UnaryMinusExpression
    | AdditionOperatorExpression
    | SubtractionOperatorExpression
    | MultiplicationOperatorExpression
    | DivisionOperatorExpression
    | ModuloOperatorExpression
    | ExponentOperatorExpression
    ;
```

<span data-ttu-id="61fcb-814">Arithmetische Operationen mit Gleitkommazahlen können mit einer höheren Genauigkeit als der Ergebnistyp des Vorgangs ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-814">Floating-point arithmetic operations may be performed with higher precision than the result type of the operation.</span></span> <span data-ttu-id="61fcb-815">Beispielsweise unterstützen einige Hardwarearchitekturen einen "erweiterten" oder "long double" vom Typ Gleitkommazahlen mit größeren Bereich und Genauigkeit als den `Double` geben, und führen Sie alle Operationen mit Gleitkommazahlen mit diesem Typ mit höherer Genauigkeit implizit.</span><span class="sxs-lookup"><span data-stu-id="61fcb-815">For example, some hardware architectures support an "extended" or "long double" floating-point type with greater range and precision than the `Double` type, and implicitly perform all floating-point operations using this higher-precision type.</span></span> <span data-ttu-id="61fcb-816">Hardware-Architekturen können zum Ausführen von Operations mit Gleitkommazahlen mit geringerer Genauigkeit nur eine übermäßige Kosten in Bezug auf Leistung vorgenommen werden; anstatt eine Implementierung, die in Anspruch genommenen Leistung und Genauigkeit zu benötigen, ermöglicht Visual Basic den Typ mit höherer Genauigkeit, die für alle Operationen mit Gleitkommazahlen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-816">Hardware architectures can be made to perform floating-point operations with less precision only at excessive cost in performance; rather than require an implementation to forfeit both performance and precision, Visual Basic allows the higher-precision type to be used for all floating-point operations.</span></span> <span data-ttu-id="61fcb-817">Als die Bereitstellung eine genauere Ergebnisse, hat dies nur selten keine messbaren Auswirkungen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-817">Other than delivering more precise results, this rarely has any measurable effects.</span></span> <span data-ttu-id="61fcb-818">In Ausdrücken des Formulars `x * y / z`, in denen die Multiplikation für ein Ergebnis erzeugt, die außerhalb der `Double` Bereich, aber die nachfolgende Division wird das temporäre Ergebnis wieder in die `Double` liegen, die Tatsache, dass der Ausdruck in einem höheren Bereich ausgewertet Format kann dazu führen, dass eine endliche Ergebnis unendlich erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-818">However, in expressions of the form `x * y / z`, where the multiplication produces a result that is outside the `Double` range, but the subsequent division brings the temporary result back into the `Double` range, the fact that the expression is evaluated in a higher-range format may cause a finite result to be produced instead of infinity.</span></span>


### <a name="unary-plus-operator"></a><span data-ttu-id="61fcb-819">Unärer Plus-Operator</span><span class="sxs-lookup"><span data-stu-id="61fcb-819">Unary Plus Operator</span></span>

```antlr
UnaryPlusExpression
    : '+' Expression
    ;
```

<span data-ttu-id="61fcb-820">Der unäre plus -Operator ist definiert, für die `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Single`, `Double`, und `Decimal` Typen .</span><span class="sxs-lookup"><span data-stu-id="61fcb-820">The unary plus operator is defined for the `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Single`, `Double`, and `Decimal` types.</span></span>

<span data-ttu-id="61fcb-821">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="61fcb-821">__Operation Type:__</span></span>


| <span data-ttu-id="61fcb-822">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-822">__Bo__</span></span> | <span data-ttu-id="61fcb-823">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-823">__SB__</span></span> | <span data-ttu-id="61fcb-824">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-824">__By__</span></span> | <span data-ttu-id="61fcb-825">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-825">__Sh__</span></span> | <span data-ttu-id="61fcb-826">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-826">__US__</span></span> | <span data-ttu-id="61fcb-827">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-827">__In__</span></span> | <span data-ttu-id="61fcb-828">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-828">__UI__</span></span> | <span data-ttu-id="61fcb-829">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-829">__Lo__</span></span> | <span data-ttu-id="61fcb-830">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-830">__UL__</span></span> | <span data-ttu-id="61fcb-831">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-831">__De__</span></span> | <span data-ttu-id="61fcb-832">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-832">__Si__</span></span> | <span data-ttu-id="61fcb-833">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-833">__Do__</span></span> | <span data-ttu-id="61fcb-834">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-834">__Da__</span></span>  | <span data-ttu-id="61fcb-835">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-835">__Ch__</span></span>  | <span data-ttu-id="61fcb-836">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-836">__St__</span></span> | <span data-ttu-id="61fcb-837">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-837">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="61fcb-838">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-838">Sh</span></span> | <span data-ttu-id="61fcb-839">SB</span><span class="sxs-lookup"><span data-stu-id="61fcb-839">SB</span></span> | <span data-ttu-id="61fcb-840">um</span><span class="sxs-lookup"><span data-stu-id="61fcb-840">By</span></span> | <span data-ttu-id="61fcb-841">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-841">Sh</span></span> | <span data-ttu-id="61fcb-842">US</span><span class="sxs-lookup"><span data-stu-id="61fcb-842">US</span></span> | <span data-ttu-id="61fcb-843">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-843">In</span></span> | <span data-ttu-id="61fcb-844">UI</span><span class="sxs-lookup"><span data-stu-id="61fcb-844">UI</span></span> | <span data-ttu-id="61fcb-845">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-845">Lo</span></span> | <span data-ttu-id="61fcb-846">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-846">UL</span></span> | <span data-ttu-id="61fcb-847">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-847">De</span></span> | <span data-ttu-id="61fcb-848">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-848">Si</span></span> | <span data-ttu-id="61fcb-849">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-849">Do</span></span> | <span data-ttu-id="61fcb-850">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-850">Err</span></span> | <span data-ttu-id="61fcb-851">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-851">Err</span></span> | <span data-ttu-id="61fcb-852">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-852">Do</span></span> | <span data-ttu-id="61fcb-853">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-853">Ob</span></span> | 


### <a name="unary-minus-operator"></a><span data-ttu-id="61fcb-854">Unäres Minus-Operator</span><span class="sxs-lookup"><span data-stu-id="61fcb-854">Unary Minus Operator</span></span>

```antlr
UnaryMinusExpression
    : '-' Expression
    ;
```

<span data-ttu-id="61fcb-855">Der unäre Minusoperator ist für die folgenden Typen definiert:</span><span class="sxs-lookup"><span data-stu-id="61fcb-855">The unary minus operator is defined for the following types:</span></span>

<span data-ttu-id="61fcb-856">`SByte`, `Short`, `Integer`und `Long`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-856">`SByte`, `Short`, `Integer`, and `Long`.</span></span> <span data-ttu-id="61fcb-857">Das Ergebnis wird durch Subtrahieren der Operand von 0 (null) berechnet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-857">The result is computed by subtracting the operand from zero.</span></span> <span data-ttu-id="61fcb-858">Wenn Überprüfungen auf Ganzzahlüberlauf eingeschaltet und der Wert des Operanden die maximale Negative ist `SByte`, `Short`, `Integer`, oder `Long`, `System.OverflowException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="61fcb-858">If integer overflow checking is on and the value of the operand is the maximum negative `SByte`, `Short`, `Integer`, or `Long`, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="61fcb-859">Wenn der Wert des Operanden um die maximale negativ ist, andernfalls `SByte`, `Short`, `Integer`, oder `Long`, das Ergebnis ist dieser Wert und der Überlauf wird nicht gemeldet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-859">Otherwise, if the value of the operand is the maximum negative `SByte`, `Short`, `Integer`, or `Long`, the result is that same value, and the overflow is not reported.</span></span>

<span data-ttu-id="61fcb-860">`Single` und `Double`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-860">`Single` and `Double`.</span></span> <span data-ttu-id="61fcb-861">Das Ergebnis ist der Wert des Operanden mit umgekehrtem Vorzeichen umkehren, einschließlich der Werte 0 und unendlich.</span><span class="sxs-lookup"><span data-stu-id="61fcb-861">The result is the value of the operand with its sign inverted, including the values 0 and Infinity.</span></span> <span data-ttu-id="61fcb-862">Wenn der Operand NaN ist, ist das Ergebnis auch NaN.</span><span class="sxs-lookup"><span data-stu-id="61fcb-862">If the operand is NaN, the result is also NaN.</span></span>

<span data-ttu-id="61fcb-863">`Decimal`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-863">`Decimal`.</span></span> <span data-ttu-id="61fcb-864">Das Ergebnis wird durch Subtrahieren der Operand von 0 (null) berechnet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-864">The result is computed by subtracting the operand from zero.</span></span>

<span data-ttu-id="61fcb-865">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="61fcb-865">__Operation Type:__</span></span>

| <span data-ttu-id="61fcb-866">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-866">__Bo__</span></span> | <span data-ttu-id="61fcb-867">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-867">__SB__</span></span> | <span data-ttu-id="61fcb-868">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-868">__By__</span></span> | <span data-ttu-id="61fcb-869">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-869">__Sh__</span></span> | <span data-ttu-id="61fcb-870">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-870">__US__</span></span> | <span data-ttu-id="61fcb-871">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-871">__In__</span></span> | <span data-ttu-id="61fcb-872">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-872">__UI__</span></span> | <span data-ttu-id="61fcb-873">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-873">__Lo__</span></span> | <span data-ttu-id="61fcb-874">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-874">__UL__</span></span> | <span data-ttu-id="61fcb-875">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-875">__De__</span></span> | <span data-ttu-id="61fcb-876">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-876">__Si__</span></span> | <span data-ttu-id="61fcb-877">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-877">__Do__</span></span> | <span data-ttu-id="61fcb-878">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-878">__Da__</span></span>  | <span data-ttu-id="61fcb-879">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-879">__Ch__</span></span>  | <span data-ttu-id="61fcb-880">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-880">__St__</span></span> | <span data-ttu-id="61fcb-881">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-881">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="61fcb-882">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-882">Sh</span></span> | <span data-ttu-id="61fcb-883">SB</span><span class="sxs-lookup"><span data-stu-id="61fcb-883">SB</span></span> | <span data-ttu-id="61fcb-884">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-884">Sh</span></span> | <span data-ttu-id="61fcb-885">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-885">Sh</span></span> | <span data-ttu-id="61fcb-886">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-886">In</span></span> | <span data-ttu-id="61fcb-887">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-887">In</span></span> | <span data-ttu-id="61fcb-888">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-888">Lo</span></span> | <span data-ttu-id="61fcb-889">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-889">Lo</span></span> | <span data-ttu-id="61fcb-890">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-890">De</span></span> | <span data-ttu-id="61fcb-891">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-891">De</span></span> | <span data-ttu-id="61fcb-892">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-892">Si</span></span> | <span data-ttu-id="61fcb-893">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-893">Do</span></span> | <span data-ttu-id="61fcb-894">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-894">Err</span></span> | <span data-ttu-id="61fcb-895">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-895">Err</span></span> | <span data-ttu-id="61fcb-896">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-896">Do</span></span> | <span data-ttu-id="61fcb-897">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-897">Ob</span></span> | 


### <a name="addition-operator"></a><span data-ttu-id="61fcb-898">Addition-Operator</span><span class="sxs-lookup"><span data-stu-id="61fcb-898">Addition Operator</span></span>

<span data-ttu-id="61fcb-899">Der Additionsoperator berechnet die Summe der beiden Operanden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-899">The addition operator computes the sum of the two operands.</span></span>

```antlr
AdditionOperatorExpression
    : Expression '+' LineTerminator? Expression
    ;
```

<span data-ttu-id="61fcb-900">Der Additionsoperator ist für die folgenden Typen definiert:</span><span class="sxs-lookup"><span data-stu-id="61fcb-900">The addition operator is defined for the following types:</span></span>

* <span data-ttu-id="61fcb-901">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` und `Long`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-901">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="61fcb-902">Wenn Überprüfungen auf Ganzzahlüberlauf auf und die Summe außerhalb des Bereichs von den Ergebnistyp ist einer `System.OverflowException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="61fcb-902">If integer overflow checking is on and the sum is outside the range of the result type, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="61fcb-903">Anderenfalls Überläufe werden nicht gemeldet, und alle signifikanten höherwertigen Bits des Ergebnisses werden verworfen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-903">Otherwise, overflows are not reported, and any significant high-order bits of the result are discarded.</span></span>

* <span data-ttu-id="61fcb-904">`Single` und `Double`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-904">`Single` and `Double`.</span></span> <span data-ttu-id="61fcb-905">Die Summe wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-905">The sum is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="61fcb-906">`Decimal`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-906">`Decimal`.</span></span> <span data-ttu-id="61fcb-907">Wenn der resultierende Wert für die Darstellung im decimal-Format zu groß ist eine `System.OverflowException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="61fcb-907">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="61fcb-908">Wenn der Ergebniswert zur Darstellung im Dezimalformat zu klein ist, ist das Ergebnis 0.</span><span class="sxs-lookup"><span data-stu-id="61fcb-908">If the result value is too small to represent in the decimal format, the result is 0.</span></span>

* <span data-ttu-id="61fcb-909">`String`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-909">`String`.</span></span> <span data-ttu-id="61fcb-910">Die beiden `String` Operanden miteinander verkettet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-910">The two `String` operands are concatenated together.</span></span>

* <span data-ttu-id="61fcb-911">`Date`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-911">`Date`.</span></span> <span data-ttu-id="61fcb-912">Die `System.DateTime` Typ definiert überladenen Addition-Operatoren.</span><span class="sxs-lookup"><span data-stu-id="61fcb-912">The `System.DateTime` type defines overloaded addition operators.</span></span> <span data-ttu-id="61fcb-913">Da `System.DateTime` entspricht, auf die systeminternen `Date` geben, diese Operatoren steht auch auf die `Date` Typ.</span><span class="sxs-lookup"><span data-stu-id="61fcb-913">Because `System.DateTime` is equivalent to the intrinsic `Date` type, these operators is also available on the `Date` type.</span></span>

<span data-ttu-id="61fcb-914">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="61fcb-914">__Operation Type:__</span></span>

|        | <span data-ttu-id="61fcb-915">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-915">__Bo__</span></span> | <span data-ttu-id="61fcb-916">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-916">__SB__</span></span> | <span data-ttu-id="61fcb-917">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-917">__By__</span></span> | <span data-ttu-id="61fcb-918">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-918">__Sh__</span></span> | <span data-ttu-id="61fcb-919">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-919">__US__</span></span> | <span data-ttu-id="61fcb-920">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-920">__In__</span></span> | <span data-ttu-id="61fcb-921">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-921">__UI__</span></span> | <span data-ttu-id="61fcb-922">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-922">__Lo__</span></span> | <span data-ttu-id="61fcb-923">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-923">__UL__</span></span> | <span data-ttu-id="61fcb-924">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-924">__De__</span></span> | <span data-ttu-id="61fcb-925">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-925">__Si__</span></span> | <span data-ttu-id="61fcb-926">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-926">__Do__</span></span> | <span data-ttu-id="61fcb-927">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-927">__Da__</span></span>  | <span data-ttu-id="61fcb-928">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-928">__Ch__</span></span>  | <span data-ttu-id="61fcb-929">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-929">__St__</span></span> | <span data-ttu-id="61fcb-930">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-930">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="61fcb-931">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-931">__Bo__</span></span> | <span data-ttu-id="61fcb-932">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-932">Sh</span></span> | <span data-ttu-id="61fcb-933">SB</span><span class="sxs-lookup"><span data-stu-id="61fcb-933">SB</span></span> | <span data-ttu-id="61fcb-934">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-934">Sh</span></span> | <span data-ttu-id="61fcb-935">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-935">Sh</span></span> | <span data-ttu-id="61fcb-936">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-936">In</span></span> | <span data-ttu-id="61fcb-937">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-937">In</span></span> | <span data-ttu-id="61fcb-938">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-938">Lo</span></span> | <span data-ttu-id="61fcb-939">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-939">Lo</span></span> | <span data-ttu-id="61fcb-940">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-940">De</span></span> | <span data-ttu-id="61fcb-941">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-941">De</span></span> | <span data-ttu-id="61fcb-942">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-942">Si</span></span> | <span data-ttu-id="61fcb-943">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-943">Do</span></span> | <span data-ttu-id="61fcb-944">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-944">Err</span></span> | <span data-ttu-id="61fcb-945">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-945">Err</span></span> | <span data-ttu-id="61fcb-946">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-946">Do</span></span> | <span data-ttu-id="61fcb-947">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-947">Ob</span></span> | 
| <span data-ttu-id="61fcb-948">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-948">__SB__</span></span> |    | <span data-ttu-id="61fcb-949">SB</span><span class="sxs-lookup"><span data-stu-id="61fcb-949">SB</span></span> | <span data-ttu-id="61fcb-950">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-950">Sh</span></span> | <span data-ttu-id="61fcb-951">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-951">Sh</span></span> | <span data-ttu-id="61fcb-952">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-952">In</span></span> | <span data-ttu-id="61fcb-953">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-953">In</span></span> | <span data-ttu-id="61fcb-954">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-954">Lo</span></span> | <span data-ttu-id="61fcb-955">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-955">Lo</span></span> | <span data-ttu-id="61fcb-956">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-956">De</span></span> | <span data-ttu-id="61fcb-957">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-957">De</span></span> | <span data-ttu-id="61fcb-958">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-958">Si</span></span> | <span data-ttu-id="61fcb-959">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-959">Do</span></span> | <span data-ttu-id="61fcb-960">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-960">Err</span></span> | <span data-ttu-id="61fcb-961">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-961">Err</span></span> | <span data-ttu-id="61fcb-962">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-962">Do</span></span> | <span data-ttu-id="61fcb-963">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-963">Ob</span></span> | 
| <span data-ttu-id="61fcb-964">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-964">__By__</span></span> |    |    | <span data-ttu-id="61fcb-965">um</span><span class="sxs-lookup"><span data-stu-id="61fcb-965">By</span></span> | <span data-ttu-id="61fcb-966">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-966">Sh</span></span> | <span data-ttu-id="61fcb-967">US</span><span class="sxs-lookup"><span data-stu-id="61fcb-967">US</span></span> | <span data-ttu-id="61fcb-968">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-968">In</span></span> | <span data-ttu-id="61fcb-969">UI</span><span class="sxs-lookup"><span data-stu-id="61fcb-969">UI</span></span> | <span data-ttu-id="61fcb-970">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-970">Lo</span></span> | <span data-ttu-id="61fcb-971">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-971">UL</span></span> | <span data-ttu-id="61fcb-972">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-972">De</span></span> | <span data-ttu-id="61fcb-973">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-973">Si</span></span> | <span data-ttu-id="61fcb-974">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-974">Do</span></span> | <span data-ttu-id="61fcb-975">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-975">Err</span></span> | <span data-ttu-id="61fcb-976">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-976">Err</span></span> | <span data-ttu-id="61fcb-977">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-977">Do</span></span> | <span data-ttu-id="61fcb-978">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-978">Ob</span></span> | 
| <span data-ttu-id="61fcb-979">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-979">__Sh__</span></span> |    |    |    | <span data-ttu-id="61fcb-980">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-980">Sh</span></span> | <span data-ttu-id="61fcb-981">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-981">In</span></span> | <span data-ttu-id="61fcb-982">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-982">In</span></span> | <span data-ttu-id="61fcb-983">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-983">Lo</span></span> | <span data-ttu-id="61fcb-984">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-984">Lo</span></span> | <span data-ttu-id="61fcb-985">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-985">De</span></span> | <span data-ttu-id="61fcb-986">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-986">De</span></span> | <span data-ttu-id="61fcb-987">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-987">Si</span></span> | <span data-ttu-id="61fcb-988">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-988">Do</span></span> | <span data-ttu-id="61fcb-989">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-989">Err</span></span> | <span data-ttu-id="61fcb-990">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-990">Err</span></span> | <span data-ttu-id="61fcb-991">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-991">Do</span></span> | <span data-ttu-id="61fcb-992">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-992">Ob</span></span> | 
| <span data-ttu-id="61fcb-993">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-993">__US__</span></span> |    |    |    |    | <span data-ttu-id="61fcb-994">US</span><span class="sxs-lookup"><span data-stu-id="61fcb-994">US</span></span> | <span data-ttu-id="61fcb-995">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-995">In</span></span> | <span data-ttu-id="61fcb-996">UI</span><span class="sxs-lookup"><span data-stu-id="61fcb-996">UI</span></span> | <span data-ttu-id="61fcb-997">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-997">Lo</span></span> | <span data-ttu-id="61fcb-998">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-998">UL</span></span> | <span data-ttu-id="61fcb-999">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-999">De</span></span> | <span data-ttu-id="61fcb-1000">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1000">Si</span></span> | <span data-ttu-id="61fcb-1001">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1001">Do</span></span> | <span data-ttu-id="61fcb-1002">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1002">Err</span></span> | <span data-ttu-id="61fcb-1003">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1003">Err</span></span> | <span data-ttu-id="61fcb-1004">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1004">Do</span></span> | <span data-ttu-id="61fcb-1005">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1005">Ob</span></span> | 
| <span data-ttu-id="61fcb-1006">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1006">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="61fcb-1007">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1007">In</span></span> | <span data-ttu-id="61fcb-1008">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1008">Lo</span></span> | <span data-ttu-id="61fcb-1009">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1009">Lo</span></span> | <span data-ttu-id="61fcb-1010">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1010">De</span></span> | <span data-ttu-id="61fcb-1011">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1011">De</span></span> | <span data-ttu-id="61fcb-1012">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1012">Si</span></span> | <span data-ttu-id="61fcb-1013">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1013">Do</span></span> | <span data-ttu-id="61fcb-1014">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1014">Err</span></span> | <span data-ttu-id="61fcb-1015">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1015">Err</span></span> | <span data-ttu-id="61fcb-1016">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1016">Do</span></span> | <span data-ttu-id="61fcb-1017">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1017">Ob</span></span> | 
| <span data-ttu-id="61fcb-1018">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1018">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="61fcb-1019">UI</span><span class="sxs-lookup"><span data-stu-id="61fcb-1019">UI</span></span> | <span data-ttu-id="61fcb-1020">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1020">Lo</span></span> | <span data-ttu-id="61fcb-1021">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-1021">UL</span></span> | <span data-ttu-id="61fcb-1022">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1022">De</span></span> | <span data-ttu-id="61fcb-1023">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1023">Si</span></span> | <span data-ttu-id="61fcb-1024">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1024">Do</span></span> | <span data-ttu-id="61fcb-1025">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1025">Err</span></span> | <span data-ttu-id="61fcb-1026">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1026">Err</span></span> | <span data-ttu-id="61fcb-1027">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1027">Do</span></span> | <span data-ttu-id="61fcb-1028">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1028">Ob</span></span> | 
| <span data-ttu-id="61fcb-1029">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1029">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1030">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1030">Lo</span></span> | <span data-ttu-id="61fcb-1031">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1031">De</span></span> | <span data-ttu-id="61fcb-1032">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1032">De</span></span> | <span data-ttu-id="61fcb-1033">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1033">Si</span></span> | <span data-ttu-id="61fcb-1034">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1034">Do</span></span> | <span data-ttu-id="61fcb-1035">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1035">Err</span></span> | <span data-ttu-id="61fcb-1036">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1036">Err</span></span> | <span data-ttu-id="61fcb-1037">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1037">Do</span></span> | <span data-ttu-id="61fcb-1038">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1038">Ob</span></span> | 
| <span data-ttu-id="61fcb-1039">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1039">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1040">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-1040">UL</span></span> | <span data-ttu-id="61fcb-1041">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1041">De</span></span> | <span data-ttu-id="61fcb-1042">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1042">Si</span></span> | <span data-ttu-id="61fcb-1043">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1043">Do</span></span> | <span data-ttu-id="61fcb-1044">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1044">Err</span></span> | <span data-ttu-id="61fcb-1045">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1045">Err</span></span> | <span data-ttu-id="61fcb-1046">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1046">Do</span></span> | <span data-ttu-id="61fcb-1047">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1047">Ob</span></span> | 
| <span data-ttu-id="61fcb-1048">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1048">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1049">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1049">De</span></span> | <span data-ttu-id="61fcb-1050">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1050">Si</span></span> | <span data-ttu-id="61fcb-1051">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1051">Do</span></span> | <span data-ttu-id="61fcb-1052">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1052">Err</span></span> | <span data-ttu-id="61fcb-1053">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1053">Err</span></span> | <span data-ttu-id="61fcb-1054">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1054">Do</span></span> | <span data-ttu-id="61fcb-1055">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1055">Ob</span></span> | 
| <span data-ttu-id="61fcb-1056">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1056">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1057">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1057">Si</span></span> | <span data-ttu-id="61fcb-1058">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1058">Do</span></span> | <span data-ttu-id="61fcb-1059">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1059">Err</span></span> | <span data-ttu-id="61fcb-1060">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1060">Err</span></span> | <span data-ttu-id="61fcb-1061">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1061">Do</span></span> | <span data-ttu-id="61fcb-1062">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1062">Ob</span></span> | 
| <span data-ttu-id="61fcb-1063">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1063">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1064">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1064">Do</span></span> | <span data-ttu-id="61fcb-1065">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1065">Err</span></span> | <span data-ttu-id="61fcb-1066">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1066">Err</span></span> | <span data-ttu-id="61fcb-1067">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1067">Do</span></span> | <span data-ttu-id="61fcb-1068">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1068">Ob</span></span> | 
| <span data-ttu-id="61fcb-1069">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1069">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1070">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-1070">St</span></span>  | <span data-ttu-id="61fcb-1071">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1071">Err</span></span> | <span data-ttu-id="61fcb-1072">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-1072">St</span></span> | <span data-ttu-id="61fcb-1073">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1073">Ob</span></span> | 
| <span data-ttu-id="61fcb-1074">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1074">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="61fcb-1075">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-1075">St</span></span>  | <span data-ttu-id="61fcb-1076">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-1076">St</span></span> | <span data-ttu-id="61fcb-1077">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1077">Ob</span></span> | 
| <span data-ttu-id="61fcb-1078">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1078">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="61fcb-1079">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-1079">St</span></span> | <span data-ttu-id="61fcb-1080">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1080">Ob</span></span> | 
| <span data-ttu-id="61fcb-1081">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1081">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | <span data-ttu-id="61fcb-1082">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1082">Ob</span></span> | 


### <a name="subtraction-operator"></a><span data-ttu-id="61fcb-1083">Subtraktionsoperator</span><span class="sxs-lookup"><span data-stu-id="61fcb-1083">Subtraction Operator</span></span>

<span data-ttu-id="61fcb-1084">Der Subtraktionsoperator subtrahiert den zweiten Operanden vom ersten Operanden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1084">The subtraction operator subtracts the second operand from the first operand.</span></span>

```antlr
SubtractionOperatorExpression
    : Expression '-' LineTerminator? Expression
    ;
```

<span data-ttu-id="61fcb-1085">Der Subtraktionsoperator ist für die folgenden Typen definiert:</span><span class="sxs-lookup"><span data-stu-id="61fcb-1085">The subtraction operator is defined for the following types:</span></span>

* <span data-ttu-id="61fcb-1086">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` und `Long`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1086">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="61fcb-1087">Wenn Überprüfungen auf Ganzzahlüberlauf eingeschaltet und der Unterschied, außerhalb des Bereichs von den Ergebnistyp ist einer `System.OverflowException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1087">If integer overflow checking is on and the difference is outside the range of the result type, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="61fcb-1088">Anderenfalls Überläufe werden nicht gemeldet, und alle signifikanten höherwertigen Bits des Ergebnisses werden verworfen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1088">Otherwise, overflows are not reported, and any significant high-order bits of the result are discarded.</span></span>

* <span data-ttu-id="61fcb-1089">`Single` und `Double`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1089">`Single` and `Double`.</span></span> <span data-ttu-id="61fcb-1090">Der Unterschied wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1090">The difference is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="61fcb-1091">`Decimal`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1091">`Decimal`.</span></span> <span data-ttu-id="61fcb-1092">Wenn der resultierende Wert für die Darstellung im decimal-Format zu groß ist eine `System.OverflowException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1092">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="61fcb-1093">Wenn der Ergebniswert zur Darstellung im Dezimalformat zu klein ist, ist das Ergebnis 0.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1093">If the result value is too small to represent in the decimal format, the result is 0.</span></span>

* <span data-ttu-id="61fcb-1094">`Date`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1094">`Date`.</span></span> <span data-ttu-id="61fcb-1095">Die `System.DateTime` Typ definiert überladene Subtraktionsoperatoren.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1095">The `System.DateTime` type defines overloaded subtraction operators.</span></span> <span data-ttu-id="61fcb-1096">Da `System.DateTime` entspricht, auf die systeminternen `Date` geben, diese Operatoren steht auch auf die `Date` Typ.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1096">Because `System.DateTime` is equivalent to the intrinsic `Date` type, these operators is also available on the `Date` type.</span></span>

<span data-ttu-id="61fcb-1097">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1097">__Operation Type:__</span></span>

|        | <span data-ttu-id="61fcb-1098">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1098">__Bo__</span></span> | <span data-ttu-id="61fcb-1099">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1099">__SB__</span></span> | <span data-ttu-id="61fcb-1100">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1100">__By__</span></span> | <span data-ttu-id="61fcb-1101">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1101">__Sh__</span></span> | <span data-ttu-id="61fcb-1102">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1102">__US__</span></span> | <span data-ttu-id="61fcb-1103">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1103">__In__</span></span> | <span data-ttu-id="61fcb-1104">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1104">__UI__</span></span> | <span data-ttu-id="61fcb-1105">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1105">__Lo__</span></span> | <span data-ttu-id="61fcb-1106">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1106">__UL__</span></span> | <span data-ttu-id="61fcb-1107">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1107">__De__</span></span> | <span data-ttu-id="61fcb-1108">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1108">__Si__</span></span> | <span data-ttu-id="61fcb-1109">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1109">__Do__</span></span> | <span data-ttu-id="61fcb-1110">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1110">__Da__</span></span>  | <span data-ttu-id="61fcb-1111">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1111">__Ch__</span></span>  | <span data-ttu-id="61fcb-1112">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1112">__St__</span></span> | <span data-ttu-id="61fcb-1113">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1113">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="61fcb-1114">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1114">__Bo__</span></span> | <span data-ttu-id="61fcb-1115">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1115">Sh</span></span> | <span data-ttu-id="61fcb-1116">SB</span><span class="sxs-lookup"><span data-stu-id="61fcb-1116">SB</span></span> | <span data-ttu-id="61fcb-1117">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1117">Sh</span></span> | <span data-ttu-id="61fcb-1118">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1118">Sh</span></span> | <span data-ttu-id="61fcb-1119">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1119">In</span></span> | <span data-ttu-id="61fcb-1120">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1120">In</span></span> | <span data-ttu-id="61fcb-1121">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1121">Lo</span></span> | <span data-ttu-id="61fcb-1122">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1122">Lo</span></span> | <span data-ttu-id="61fcb-1123">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1123">De</span></span> | <span data-ttu-id="61fcb-1124">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1124">De</span></span> | <span data-ttu-id="61fcb-1125">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1125">Si</span></span> | <span data-ttu-id="61fcb-1126">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1126">Do</span></span> | <span data-ttu-id="61fcb-1127">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1127">Err</span></span> | <span data-ttu-id="61fcb-1128">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1128">Err</span></span> | <span data-ttu-id="61fcb-1129">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1129">Do</span></span>  | <span data-ttu-id="61fcb-1130">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1130">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1131">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1131">__SB__</span></span> |    | <span data-ttu-id="61fcb-1132">SB</span><span class="sxs-lookup"><span data-stu-id="61fcb-1132">SB</span></span> | <span data-ttu-id="61fcb-1133">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1133">Sh</span></span> | <span data-ttu-id="61fcb-1134">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1134">Sh</span></span> | <span data-ttu-id="61fcb-1135">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1135">In</span></span> | <span data-ttu-id="61fcb-1136">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1136">In</span></span> | <span data-ttu-id="61fcb-1137">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1137">Lo</span></span> | <span data-ttu-id="61fcb-1138">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1138">Lo</span></span> | <span data-ttu-id="61fcb-1139">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1139">De</span></span> | <span data-ttu-id="61fcb-1140">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1140">De</span></span> | <span data-ttu-id="61fcb-1141">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1141">Si</span></span> | <span data-ttu-id="61fcb-1142">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1142">Do</span></span> | <span data-ttu-id="61fcb-1143">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1143">Err</span></span> | <span data-ttu-id="61fcb-1144">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1144">Err</span></span> | <span data-ttu-id="61fcb-1145">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1145">Do</span></span>  | <span data-ttu-id="61fcb-1146">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1146">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1147">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1147">__By__</span></span> |    |    | <span data-ttu-id="61fcb-1148">um</span><span class="sxs-lookup"><span data-stu-id="61fcb-1148">By</span></span> | <span data-ttu-id="61fcb-1149">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1149">Sh</span></span> | <span data-ttu-id="61fcb-1150">US</span><span class="sxs-lookup"><span data-stu-id="61fcb-1150">US</span></span> | <span data-ttu-id="61fcb-1151">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1151">In</span></span> | <span data-ttu-id="61fcb-1152">UI</span><span class="sxs-lookup"><span data-stu-id="61fcb-1152">UI</span></span> | <span data-ttu-id="61fcb-1153">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1153">Lo</span></span> | <span data-ttu-id="61fcb-1154">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-1154">UL</span></span> | <span data-ttu-id="61fcb-1155">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1155">De</span></span> | <span data-ttu-id="61fcb-1156">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1156">Si</span></span> | <span data-ttu-id="61fcb-1157">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1157">Do</span></span> | <span data-ttu-id="61fcb-1158">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1158">Err</span></span> | <span data-ttu-id="61fcb-1159">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1159">Err</span></span> | <span data-ttu-id="61fcb-1160">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1160">Do</span></span>  | <span data-ttu-id="61fcb-1161">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1161">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1162">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1162">__Sh__</span></span> |    |    |    | <span data-ttu-id="61fcb-1163">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1163">Sh</span></span> | <span data-ttu-id="61fcb-1164">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1164">In</span></span> | <span data-ttu-id="61fcb-1165">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1165">In</span></span> | <span data-ttu-id="61fcb-1166">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1166">Lo</span></span> | <span data-ttu-id="61fcb-1167">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1167">Lo</span></span> | <span data-ttu-id="61fcb-1168">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1168">De</span></span> | <span data-ttu-id="61fcb-1169">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1169">De</span></span> | <span data-ttu-id="61fcb-1170">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1170">Si</span></span> | <span data-ttu-id="61fcb-1171">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1171">Do</span></span> | <span data-ttu-id="61fcb-1172">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1172">Err</span></span> | <span data-ttu-id="61fcb-1173">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1173">Err</span></span> | <span data-ttu-id="61fcb-1174">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1174">Do</span></span>  | <span data-ttu-id="61fcb-1175">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1175">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1176">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1176">__US__</span></span> |    |    |    |    | <span data-ttu-id="61fcb-1177">US</span><span class="sxs-lookup"><span data-stu-id="61fcb-1177">US</span></span> | <span data-ttu-id="61fcb-1178">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1178">In</span></span> | <span data-ttu-id="61fcb-1179">UI</span><span class="sxs-lookup"><span data-stu-id="61fcb-1179">UI</span></span> | <span data-ttu-id="61fcb-1180">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1180">Lo</span></span> | <span data-ttu-id="61fcb-1181">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-1181">UL</span></span> | <span data-ttu-id="61fcb-1182">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1182">De</span></span> | <span data-ttu-id="61fcb-1183">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1183">Si</span></span> | <span data-ttu-id="61fcb-1184">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1184">Do</span></span> | <span data-ttu-id="61fcb-1185">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1185">Err</span></span> | <span data-ttu-id="61fcb-1186">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1186">Err</span></span> | <span data-ttu-id="61fcb-1187">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1187">Do</span></span>  | <span data-ttu-id="61fcb-1188">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1188">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1189">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1189">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="61fcb-1190">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1190">In</span></span> | <span data-ttu-id="61fcb-1191">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1191">Lo</span></span> | <span data-ttu-id="61fcb-1192">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1192">Lo</span></span> | <span data-ttu-id="61fcb-1193">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1193">De</span></span> | <span data-ttu-id="61fcb-1194">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1194">De</span></span> | <span data-ttu-id="61fcb-1195">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1195">Si</span></span> | <span data-ttu-id="61fcb-1196">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1196">Do</span></span> | <span data-ttu-id="61fcb-1197">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1197">Err</span></span> | <span data-ttu-id="61fcb-1198">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1198">Err</span></span> | <span data-ttu-id="61fcb-1199">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1199">Do</span></span>  | <span data-ttu-id="61fcb-1200">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1200">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1201">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1201">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="61fcb-1202">UI</span><span class="sxs-lookup"><span data-stu-id="61fcb-1202">UI</span></span> | <span data-ttu-id="61fcb-1203">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1203">Lo</span></span> | <span data-ttu-id="61fcb-1204">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-1204">UL</span></span> | <span data-ttu-id="61fcb-1205">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1205">De</span></span> | <span data-ttu-id="61fcb-1206">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1206">Si</span></span> | <span data-ttu-id="61fcb-1207">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1207">Do</span></span> | <span data-ttu-id="61fcb-1208">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1208">Err</span></span> | <span data-ttu-id="61fcb-1209">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1209">Err</span></span> | <span data-ttu-id="61fcb-1210">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1210">Do</span></span>  | <span data-ttu-id="61fcb-1211">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1211">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1212">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1212">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1213">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1213">Lo</span></span> | <span data-ttu-id="61fcb-1214">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1214">De</span></span> | <span data-ttu-id="61fcb-1215">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1215">De</span></span> | <span data-ttu-id="61fcb-1216">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1216">Si</span></span> | <span data-ttu-id="61fcb-1217">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1217">Do</span></span> | <span data-ttu-id="61fcb-1218">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1218">Err</span></span> | <span data-ttu-id="61fcb-1219">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1219">Err</span></span> | <span data-ttu-id="61fcb-1220">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1220">Do</span></span>  | <span data-ttu-id="61fcb-1221">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1221">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1222">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1222">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1223">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-1223">UL</span></span> | <span data-ttu-id="61fcb-1224">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1224">De</span></span> | <span data-ttu-id="61fcb-1225">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1225">Si</span></span> | <span data-ttu-id="61fcb-1226">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1226">Do</span></span> | <span data-ttu-id="61fcb-1227">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1227">Err</span></span> | <span data-ttu-id="61fcb-1228">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1228">Err</span></span> | <span data-ttu-id="61fcb-1229">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1229">Do</span></span>  | <span data-ttu-id="61fcb-1230">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1230">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1231">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1231">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1232">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1232">De</span></span> | <span data-ttu-id="61fcb-1233">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1233">Si</span></span> | <span data-ttu-id="61fcb-1234">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1234">Do</span></span> | <span data-ttu-id="61fcb-1235">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1235">Err</span></span> | <span data-ttu-id="61fcb-1236">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1236">Err</span></span> | <span data-ttu-id="61fcb-1237">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1237">Do</span></span>  | <span data-ttu-id="61fcb-1238">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1238">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1239">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1239">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1240">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1240">Si</span></span> | <span data-ttu-id="61fcb-1241">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1241">Do</span></span> | <span data-ttu-id="61fcb-1242">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1242">Err</span></span> | <span data-ttu-id="61fcb-1243">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1243">Err</span></span> | <span data-ttu-id="61fcb-1244">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1244">Do</span></span>  | <span data-ttu-id="61fcb-1245">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1245">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1246">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1246">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1247">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1247">Do</span></span> | <span data-ttu-id="61fcb-1248">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1248">Err</span></span> | <span data-ttu-id="61fcb-1249">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1249">Err</span></span> | <span data-ttu-id="61fcb-1250">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1250">Do</span></span>  | <span data-ttu-id="61fcb-1251">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1251">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1252">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1252">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1253">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1253">Err</span></span> | <span data-ttu-id="61fcb-1254">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1254">Err</span></span> | <span data-ttu-id="61fcb-1255">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1255">Err</span></span> | <span data-ttu-id="61fcb-1256">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1256">Err</span></span> | 
| <span data-ttu-id="61fcb-1257">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1257">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="61fcb-1258">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1258">Err</span></span> | <span data-ttu-id="61fcb-1259">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1259">Err</span></span> | <span data-ttu-id="61fcb-1260">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1260">Err</span></span> | 
| <span data-ttu-id="61fcb-1261">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1261">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="61fcb-1262">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1262">Do</span></span>  | <span data-ttu-id="61fcb-1263">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1263">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1264">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1264">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="61fcb-1265">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1265">Ob</span></span>  | 


### <a name="multiplication-operator"></a><span data-ttu-id="61fcb-1266">Multiplikationsoperator</span><span class="sxs-lookup"><span data-stu-id="61fcb-1266">Multiplication Operator</span></span>

<span data-ttu-id="61fcb-1267">Der Multiplikationsoperator berechnet das Produkt der beiden Operanden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1267">The multiplication operator computes the product of two operands.</span></span>

```antlr
MultiplicationOperatorExpression
    : Expression '*' LineTerminator? Expression
    ;
```

<span data-ttu-id="61fcb-1268">Der Multiplikationsoperator ist für die folgenden Typen definiert:</span><span class="sxs-lookup"><span data-stu-id="61fcb-1268">The multiplication operator is defined for the following types:</span></span>

* <span data-ttu-id="61fcb-1269">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` und `Long`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1269">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="61fcb-1270">Wenn die Überprüfungen auf Ganzzahlüberlauf ist, und das Produkt ist außerhalb des Bereichs des Ergebnistyps, eine `System.OverflowException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1270">If integer overflow checking is on and the product is outside the range of the result type, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="61fcb-1271">Anderenfalls Überläufe werden nicht gemeldet, und alle signifikanten höherwertigen Bits des Ergebnisses werden verworfen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1271">Otherwise, overflows are not reported, and any significant high-order bits of the result are discarded.</span></span>

* <span data-ttu-id="61fcb-1272">`Single` und `Double`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1272">`Single` and `Double`.</span></span> <span data-ttu-id="61fcb-1273">Das Produkt wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1273">The product is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="61fcb-1274">`Decimal`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1274">`Decimal`.</span></span> <span data-ttu-id="61fcb-1275">Wenn der resultierende Wert für die Darstellung im decimal-Format zu groß ist eine `System.OverflowException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1275">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="61fcb-1276">Wenn der Ergebniswert zur Darstellung im Dezimalformat zu klein ist, ist das Ergebnis 0.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1276">If the result value is too small to represent in the decimal format, the result is 0.</span></span>

<span data-ttu-id="61fcb-1277">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1277">__Operation Type:__</span></span>

|        | <span data-ttu-id="61fcb-1278">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1278">__Bo__</span></span> | <span data-ttu-id="61fcb-1279">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1279">__SB__</span></span> | <span data-ttu-id="61fcb-1280">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1280">__By__</span></span> | <span data-ttu-id="61fcb-1281">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1281">__Sh__</span></span> | <span data-ttu-id="61fcb-1282">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1282">__US__</span></span> | <span data-ttu-id="61fcb-1283">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1283">__In__</span></span> | <span data-ttu-id="61fcb-1284">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1284">__UI__</span></span> | <span data-ttu-id="61fcb-1285">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1285">__Lo__</span></span> | <span data-ttu-id="61fcb-1286">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1286">__UL__</span></span> | <span data-ttu-id="61fcb-1287">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1287">__De__</span></span> | <span data-ttu-id="61fcb-1288">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1288">__Si__</span></span> | <span data-ttu-id="61fcb-1289">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1289">__Do__</span></span> | <span data-ttu-id="61fcb-1290">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1290">__Da__</span></span>  | <span data-ttu-id="61fcb-1291">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1291">__Ch__</span></span>  | <span data-ttu-id="61fcb-1292">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1292">__St__</span></span> | <span data-ttu-id="61fcb-1293">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1293">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="61fcb-1294">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1294">__Bo__</span></span> | <span data-ttu-id="61fcb-1295">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1295">Sh</span></span> | <span data-ttu-id="61fcb-1296">SB</span><span class="sxs-lookup"><span data-stu-id="61fcb-1296">SB</span></span> | <span data-ttu-id="61fcb-1297">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1297">Sh</span></span> | <span data-ttu-id="61fcb-1298">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1298">Sh</span></span> | <span data-ttu-id="61fcb-1299">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1299">In</span></span> | <span data-ttu-id="61fcb-1300">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1300">In</span></span> | <span data-ttu-id="61fcb-1301">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1301">Lo</span></span> | <span data-ttu-id="61fcb-1302">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1302">Lo</span></span> | <span data-ttu-id="61fcb-1303">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1303">De</span></span> | <span data-ttu-id="61fcb-1304">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1304">De</span></span> | <span data-ttu-id="61fcb-1305">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1305">Si</span></span> | <span data-ttu-id="61fcb-1306">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1306">Do</span></span> | <span data-ttu-id="61fcb-1307">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1307">Err</span></span> | <span data-ttu-id="61fcb-1308">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1308">Err</span></span> | <span data-ttu-id="61fcb-1309">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1309">Do</span></span>  | <span data-ttu-id="61fcb-1310">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1310">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1311">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1311">__SB__</span></span> |    | <span data-ttu-id="61fcb-1312">SB</span><span class="sxs-lookup"><span data-stu-id="61fcb-1312">SB</span></span> | <span data-ttu-id="61fcb-1313">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1313">Sh</span></span> | <span data-ttu-id="61fcb-1314">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1314">Sh</span></span> | <span data-ttu-id="61fcb-1315">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1315">In</span></span> | <span data-ttu-id="61fcb-1316">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1316">In</span></span> | <span data-ttu-id="61fcb-1317">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1317">Lo</span></span> | <span data-ttu-id="61fcb-1318">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1318">Lo</span></span> | <span data-ttu-id="61fcb-1319">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1319">De</span></span> | <span data-ttu-id="61fcb-1320">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1320">De</span></span> | <span data-ttu-id="61fcb-1321">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1321">Si</span></span> | <span data-ttu-id="61fcb-1322">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1322">Do</span></span> | <span data-ttu-id="61fcb-1323">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1323">Err</span></span> | <span data-ttu-id="61fcb-1324">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1324">Err</span></span> | <span data-ttu-id="61fcb-1325">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1325">Do</span></span>  | <span data-ttu-id="61fcb-1326">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1326">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1327">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1327">__By__</span></span> |    |    | <span data-ttu-id="61fcb-1328">um</span><span class="sxs-lookup"><span data-stu-id="61fcb-1328">By</span></span> | <span data-ttu-id="61fcb-1329">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1329">Sh</span></span> | <span data-ttu-id="61fcb-1330">US</span><span class="sxs-lookup"><span data-stu-id="61fcb-1330">US</span></span> | <span data-ttu-id="61fcb-1331">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1331">In</span></span> | <span data-ttu-id="61fcb-1332">UI</span><span class="sxs-lookup"><span data-stu-id="61fcb-1332">UI</span></span> | <span data-ttu-id="61fcb-1333">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1333">Lo</span></span> | <span data-ttu-id="61fcb-1334">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-1334">UL</span></span> | <span data-ttu-id="61fcb-1335">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1335">De</span></span> | <span data-ttu-id="61fcb-1336">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1336">Si</span></span> | <span data-ttu-id="61fcb-1337">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1337">Do</span></span> | <span data-ttu-id="61fcb-1338">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1338">Err</span></span> | <span data-ttu-id="61fcb-1339">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1339">Err</span></span> | <span data-ttu-id="61fcb-1340">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1340">Do</span></span>  | <span data-ttu-id="61fcb-1341">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1341">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1342">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1342">__Sh__</span></span> |    |    |    | <span data-ttu-id="61fcb-1343">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1343">Sh</span></span> | <span data-ttu-id="61fcb-1344">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1344">In</span></span> | <span data-ttu-id="61fcb-1345">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1345">In</span></span> | <span data-ttu-id="61fcb-1346">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1346">Lo</span></span> | <span data-ttu-id="61fcb-1347">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1347">Lo</span></span> | <span data-ttu-id="61fcb-1348">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1348">De</span></span> | <span data-ttu-id="61fcb-1349">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1349">De</span></span> | <span data-ttu-id="61fcb-1350">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1350">Si</span></span> | <span data-ttu-id="61fcb-1351">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1351">Do</span></span> | <span data-ttu-id="61fcb-1352">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1352">Err</span></span> | <span data-ttu-id="61fcb-1353">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1353">Err</span></span> | <span data-ttu-id="61fcb-1354">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1354">Do</span></span>  | <span data-ttu-id="61fcb-1355">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1355">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1356">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1356">__US__</span></span> |    |    |    |    | <span data-ttu-id="61fcb-1357">US</span><span class="sxs-lookup"><span data-stu-id="61fcb-1357">US</span></span> | <span data-ttu-id="61fcb-1358">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1358">In</span></span> | <span data-ttu-id="61fcb-1359">UI</span><span class="sxs-lookup"><span data-stu-id="61fcb-1359">UI</span></span> | <span data-ttu-id="61fcb-1360">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1360">Lo</span></span> | <span data-ttu-id="61fcb-1361">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-1361">UL</span></span> | <span data-ttu-id="61fcb-1362">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1362">De</span></span> | <span data-ttu-id="61fcb-1363">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1363">Si</span></span> | <span data-ttu-id="61fcb-1364">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1364">Do</span></span> | <span data-ttu-id="61fcb-1365">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1365">Err</span></span> | <span data-ttu-id="61fcb-1366">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1366">Err</span></span> | <span data-ttu-id="61fcb-1367">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1367">Do</span></span>  | <span data-ttu-id="61fcb-1368">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1368">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1369">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1369">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="61fcb-1370">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1370">In</span></span> | <span data-ttu-id="61fcb-1371">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1371">Lo</span></span> | <span data-ttu-id="61fcb-1372">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1372">Lo</span></span> | <span data-ttu-id="61fcb-1373">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1373">De</span></span> | <span data-ttu-id="61fcb-1374">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1374">De</span></span> | <span data-ttu-id="61fcb-1375">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1375">Si</span></span> | <span data-ttu-id="61fcb-1376">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1376">Do</span></span> | <span data-ttu-id="61fcb-1377">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1377">Err</span></span> | <span data-ttu-id="61fcb-1378">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1378">Err</span></span> | <span data-ttu-id="61fcb-1379">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1379">Do</span></span>  | <span data-ttu-id="61fcb-1380">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1380">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1381">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1381">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="61fcb-1382">UI</span><span class="sxs-lookup"><span data-stu-id="61fcb-1382">UI</span></span> | <span data-ttu-id="61fcb-1383">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1383">Lo</span></span> | <span data-ttu-id="61fcb-1384">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-1384">UL</span></span> | <span data-ttu-id="61fcb-1385">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1385">De</span></span> | <span data-ttu-id="61fcb-1386">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1386">Si</span></span> | <span data-ttu-id="61fcb-1387">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1387">Do</span></span> | <span data-ttu-id="61fcb-1388">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1388">Err</span></span> | <span data-ttu-id="61fcb-1389">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1389">Err</span></span> | <span data-ttu-id="61fcb-1390">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1390">Do</span></span>  | <span data-ttu-id="61fcb-1391">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1391">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1392">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1392">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1393">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1393">Lo</span></span> | <span data-ttu-id="61fcb-1394">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1394">De</span></span> | <span data-ttu-id="61fcb-1395">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1395">De</span></span> | <span data-ttu-id="61fcb-1396">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1396">Si</span></span> | <span data-ttu-id="61fcb-1397">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1397">Do</span></span> | <span data-ttu-id="61fcb-1398">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1398">Err</span></span> | <span data-ttu-id="61fcb-1399">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1399">Err</span></span> | <span data-ttu-id="61fcb-1400">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1400">Do</span></span>  | <span data-ttu-id="61fcb-1401">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1401">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1402">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1402">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1403">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-1403">UL</span></span> | <span data-ttu-id="61fcb-1404">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1404">De</span></span> | <span data-ttu-id="61fcb-1405">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1405">Si</span></span> | <span data-ttu-id="61fcb-1406">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1406">Do</span></span> | <span data-ttu-id="61fcb-1407">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1407">Err</span></span> | <span data-ttu-id="61fcb-1408">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1408">Err</span></span> | <span data-ttu-id="61fcb-1409">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1409">Do</span></span>  | <span data-ttu-id="61fcb-1410">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1410">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1411">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1411">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1412">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1412">De</span></span> | <span data-ttu-id="61fcb-1413">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1413">Si</span></span> | <span data-ttu-id="61fcb-1414">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1414">Do</span></span> | <span data-ttu-id="61fcb-1415">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1415">Err</span></span> | <span data-ttu-id="61fcb-1416">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1416">Err</span></span> | <span data-ttu-id="61fcb-1417">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1417">Do</span></span>  | <span data-ttu-id="61fcb-1418">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1418">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1419">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1419">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1420">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1420">Si</span></span> | <span data-ttu-id="61fcb-1421">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1421">Do</span></span> | <span data-ttu-id="61fcb-1422">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1422">Err</span></span> | <span data-ttu-id="61fcb-1423">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1423">Err</span></span> | <span data-ttu-id="61fcb-1424">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1424">Do</span></span>  | <span data-ttu-id="61fcb-1425">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1425">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1426">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1426">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1427">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1427">Do</span></span> | <span data-ttu-id="61fcb-1428">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1428">Err</span></span> | <span data-ttu-id="61fcb-1429">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1429">Err</span></span> | <span data-ttu-id="61fcb-1430">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1430">Do</span></span>  | <span data-ttu-id="61fcb-1431">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1431">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1432">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1432">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1433">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1433">Err</span></span> | <span data-ttu-id="61fcb-1434">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1434">Err</span></span> | <span data-ttu-id="61fcb-1435">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1435">Err</span></span> | <span data-ttu-id="61fcb-1436">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1436">Err</span></span> | 
| <span data-ttu-id="61fcb-1437">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1437">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="61fcb-1438">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1438">Err</span></span> | <span data-ttu-id="61fcb-1439">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1439">Err</span></span> | <span data-ttu-id="61fcb-1440">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1440">Err</span></span> | 
| <span data-ttu-id="61fcb-1441">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1441">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="61fcb-1442">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1442">Do</span></span>  | <span data-ttu-id="61fcb-1443">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1443">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1444">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1444">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="61fcb-1445">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1445">Ob</span></span>  | 


### <a name="division-operators"></a><span data-ttu-id="61fcb-1446">Divisionsoperator</span><span class="sxs-lookup"><span data-stu-id="61fcb-1446">Division Operators</span></span>

<span data-ttu-id="61fcb-1447">Divisionsoperator berechnet den Quotienten aus zwei Operanden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1447">Division operators compute the quotient of two operands.</span></span> <span data-ttu-id="61fcb-1448">Es gibt zwei Divisionsoperatoren: der reguläre (Gleitkomma-) Divisionsoperator und der Ganzzahl-Divisionsoperator.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1448">There are two division operators: the regular (floating-point) division operator and the integer division operator.</span></span>

```antlr
DivisionOperatorExpression
    : FPDivisionOperatorExpression
    | IntegerDivisionOperatorExpression
    ;

FPDivisionOperatorExpression
    : Expression '/' LineTerminator? Expression
    ;

IntegerDivisionOperatorExpression
    : Expression '\\' LineTerminator? Expression
    ;
```

<span data-ttu-id="61fcb-1449">Der reguläre Divisionsoperator ist für die folgenden Typen definiert:</span><span class="sxs-lookup"><span data-stu-id="61fcb-1449">The regular division operator is defined for the following types:</span></span>

* <span data-ttu-id="61fcb-1450">`Single` und `Double`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1450">`Single` and `Double`.</span></span> <span data-ttu-id="61fcb-1451">Der Quotient ist gemäß den Regeln der IEEE 754-Arithmetik berechnet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1451">The quotient is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="61fcb-1452">`Decimal`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1452">`Decimal`.</span></span> <span data-ttu-id="61fcb-1453">Wenn der Wert des rechten Operanden NULL ist, ist eine `System.DivideByZeroException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1453">If the value of the right operand is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="61fcb-1454">Wenn der resultierende Wert für die Darstellung im decimal-Format zu groß ist eine `System.OverflowException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1454">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="61fcb-1455">Wenn der Ergebniswert zur Darstellung im Dezimalformat zu klein ist, ist das Ergebnis 0 (null).</span><span class="sxs-lookup"><span data-stu-id="61fcb-1455">If the result value is too small to represent in the decimal format, the result is zero.</span></span> <span data-ttu-id="61fcb-1456">Die Dezimalstellen des Ergebnisses wird vor der Rundung, sind keine Dezimalstellen am nächsten an die bevorzugten skalierungsgruppen die ein Ergebnis wird auf das genaue Ergebnis zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1456">The scale of the result, before any rounding, is the closest scale to the preferred scale which will preserve a result equal to the exact result.</span></span>  <span data-ttu-id="61fcb-1457">Die bevorzugte Skalierung sind keine Dezimalstellen von der erste Operand weniger die Skalierung des zweiten Operanden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1457">The preferred scale is the scale of the first operand less the scale of the second operand.</span></span>

<span data-ttu-id="61fcb-1458">Gemäß den Regeln für normale Operator Auflösung, reguläre Division ausschließlich zwischen den Operanden der Datentypen, z. B. `Byte`, `Short`, `Integer`, und `Long` würde dazu führen, dass beide Operanden in den Typ konvertiert werden `Decimal`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1458">According to normal operator resolution rules, regular division purely between operands of types such as `Byte`, `Short`, `Integer`, and `Long` would cause both operands to be converted to type `Decimal`.</span></span> <span data-ttu-id="61fcb-1459">Operator-namensauflösung in der Divisionsoperator jedoch bei Aktionen, wenn kein Typ ist `Decimal`, `Double` gilt schmaler als `Decimal`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1459">However, when doing operator resolution on the division operator when neither type is `Decimal`, `Double` is considered narrower than `Decimal`.</span></span> <span data-ttu-id="61fcb-1460">Diese Konvention eingehalten wird, da `Double` Division ist effizienter als `Decimal` Division.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1460">This convention is followed because `Double` division is more efficient than `Decimal` division.</span></span>

<span data-ttu-id="61fcb-1461">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1461">__Operation Type:__</span></span>

|        | <span data-ttu-id="61fcb-1462">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1462">__Bo__</span></span> | <span data-ttu-id="61fcb-1463">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1463">__SB__</span></span> | <span data-ttu-id="61fcb-1464">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1464">__By__</span></span> | <span data-ttu-id="61fcb-1465">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1465">__Sh__</span></span> | <span data-ttu-id="61fcb-1466">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1466">__US__</span></span> | <span data-ttu-id="61fcb-1467">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1467">__In__</span></span> | <span data-ttu-id="61fcb-1468">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1468">__UI__</span></span> | <span data-ttu-id="61fcb-1469">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1469">__Lo__</span></span> | <span data-ttu-id="61fcb-1470">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1470">__UL__</span></span> | <span data-ttu-id="61fcb-1471">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1471">__De__</span></span> | <span data-ttu-id="61fcb-1472">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1472">__Si__</span></span> | <span data-ttu-id="61fcb-1473">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1473">__Do__</span></span> | <span data-ttu-id="61fcb-1474">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1474">__Da__</span></span>  | <span data-ttu-id="61fcb-1475">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1475">__Ch__</span></span>  | <span data-ttu-id="61fcb-1476">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1476">__St__</span></span> | <span data-ttu-id="61fcb-1477">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1477">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="61fcb-1478">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1478">__Bo__</span></span> | <span data-ttu-id="61fcb-1479">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1479">Do</span></span> | <span data-ttu-id="61fcb-1480">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1480">Do</span></span> | <span data-ttu-id="61fcb-1481">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1481">Do</span></span> | <span data-ttu-id="61fcb-1482">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1482">Do</span></span> | <span data-ttu-id="61fcb-1483">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1483">Do</span></span> | <span data-ttu-id="61fcb-1484">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1484">Do</span></span> | <span data-ttu-id="61fcb-1485">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1485">Do</span></span> | <span data-ttu-id="61fcb-1486">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1486">Do</span></span> | <span data-ttu-id="61fcb-1487">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1487">Do</span></span> | <span data-ttu-id="61fcb-1488">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1488">De</span></span> | <span data-ttu-id="61fcb-1489">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1489">Si</span></span> | <span data-ttu-id="61fcb-1490">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1490">Do</span></span> | <span data-ttu-id="61fcb-1491">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1491">Err</span></span> | <span data-ttu-id="61fcb-1492">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1492">Err</span></span> | <span data-ttu-id="61fcb-1493">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1493">Do</span></span>  | <span data-ttu-id="61fcb-1494">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1494">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1495">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1495">__SB__</span></span> |    | <span data-ttu-id="61fcb-1496">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1496">Do</span></span> | <span data-ttu-id="61fcb-1497">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1497">Do</span></span> | <span data-ttu-id="61fcb-1498">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1498">Do</span></span> | <span data-ttu-id="61fcb-1499">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1499">Do</span></span> | <span data-ttu-id="61fcb-1500">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1500">Do</span></span> | <span data-ttu-id="61fcb-1501">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1501">Do</span></span> | <span data-ttu-id="61fcb-1502">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1502">Do</span></span> | <span data-ttu-id="61fcb-1503">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1503">Do</span></span> | <span data-ttu-id="61fcb-1504">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1504">De</span></span> | <span data-ttu-id="61fcb-1505">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1505">Si</span></span> | <span data-ttu-id="61fcb-1506">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1506">Do</span></span> | <span data-ttu-id="61fcb-1507">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1507">Err</span></span> | <span data-ttu-id="61fcb-1508">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1508">Err</span></span> | <span data-ttu-id="61fcb-1509">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1509">Do</span></span>  | <span data-ttu-id="61fcb-1510">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1510">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1511">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1511">__By__</span></span> |    |    | <span data-ttu-id="61fcb-1512">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1512">Do</span></span> | <span data-ttu-id="61fcb-1513">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1513">Do</span></span> | <span data-ttu-id="61fcb-1514">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1514">Do</span></span> | <span data-ttu-id="61fcb-1515">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1515">Do</span></span> | <span data-ttu-id="61fcb-1516">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1516">Do</span></span> | <span data-ttu-id="61fcb-1517">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1517">Do</span></span> | <span data-ttu-id="61fcb-1518">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1518">Do</span></span> | <span data-ttu-id="61fcb-1519">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1519">De</span></span> | <span data-ttu-id="61fcb-1520">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1520">Si</span></span> | <span data-ttu-id="61fcb-1521">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1521">Do</span></span> | <span data-ttu-id="61fcb-1522">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1522">Err</span></span> | <span data-ttu-id="61fcb-1523">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1523">Err</span></span> | <span data-ttu-id="61fcb-1524">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1524">Do</span></span>  | <span data-ttu-id="61fcb-1525">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1525">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1526">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1526">__Sh__</span></span> |    |    |    | <span data-ttu-id="61fcb-1527">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1527">Do</span></span> | <span data-ttu-id="61fcb-1528">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1528">Do</span></span> | <span data-ttu-id="61fcb-1529">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1529">Do</span></span> | <span data-ttu-id="61fcb-1530">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1530">Do</span></span> | <span data-ttu-id="61fcb-1531">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1531">Do</span></span> | <span data-ttu-id="61fcb-1532">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1532">Do</span></span> | <span data-ttu-id="61fcb-1533">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1533">De</span></span> | <span data-ttu-id="61fcb-1534">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1534">Si</span></span> | <span data-ttu-id="61fcb-1535">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1535">Do</span></span> | <span data-ttu-id="61fcb-1536">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1536">Err</span></span> | <span data-ttu-id="61fcb-1537">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1537">Err</span></span> | <span data-ttu-id="61fcb-1538">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1538">Do</span></span>  | <span data-ttu-id="61fcb-1539">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1539">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1540">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1540">__US__</span></span> |    |    |    |    | <span data-ttu-id="61fcb-1541">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1541">Do</span></span> | <span data-ttu-id="61fcb-1542">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1542">Do</span></span> | <span data-ttu-id="61fcb-1543">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1543">Do</span></span> | <span data-ttu-id="61fcb-1544">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1544">Do</span></span> | <span data-ttu-id="61fcb-1545">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1545">Do</span></span> | <span data-ttu-id="61fcb-1546">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1546">De</span></span> | <span data-ttu-id="61fcb-1547">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1547">Si</span></span> | <span data-ttu-id="61fcb-1548">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1548">Do</span></span> | <span data-ttu-id="61fcb-1549">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1549">Err</span></span> | <span data-ttu-id="61fcb-1550">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1550">Err</span></span> | <span data-ttu-id="61fcb-1551">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1551">Do</span></span>  | <span data-ttu-id="61fcb-1552">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1552">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1553">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1553">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="61fcb-1554">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1554">Do</span></span> | <span data-ttu-id="61fcb-1555">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1555">Do</span></span> | <span data-ttu-id="61fcb-1556">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1556">Do</span></span> | <span data-ttu-id="61fcb-1557">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1557">Do</span></span> | <span data-ttu-id="61fcb-1558">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1558">De</span></span> | <span data-ttu-id="61fcb-1559">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1559">Si</span></span> | <span data-ttu-id="61fcb-1560">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1560">Do</span></span> | <span data-ttu-id="61fcb-1561">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1561">Err</span></span> | <span data-ttu-id="61fcb-1562">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1562">Err</span></span> | <span data-ttu-id="61fcb-1563">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1563">Do</span></span>  | <span data-ttu-id="61fcb-1564">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1564">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1565">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1565">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="61fcb-1566">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1566">Do</span></span> | <span data-ttu-id="61fcb-1567">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1567">Do</span></span> | <span data-ttu-id="61fcb-1568">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1568">Do</span></span> | <span data-ttu-id="61fcb-1569">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1569">De</span></span> | <span data-ttu-id="61fcb-1570">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1570">Si</span></span> | <span data-ttu-id="61fcb-1571">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1571">Do</span></span> | <span data-ttu-id="61fcb-1572">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1572">Err</span></span> | <span data-ttu-id="61fcb-1573">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1573">Err</span></span> | <span data-ttu-id="61fcb-1574">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1574">Do</span></span>  | <span data-ttu-id="61fcb-1575">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1575">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1576">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1576">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1577">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1577">Do</span></span> | <span data-ttu-id="61fcb-1578">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1578">Do</span></span> | <span data-ttu-id="61fcb-1579">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1579">De</span></span> | <span data-ttu-id="61fcb-1580">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1580">Si</span></span> | <span data-ttu-id="61fcb-1581">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1581">Do</span></span> | <span data-ttu-id="61fcb-1582">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1582">Err</span></span> | <span data-ttu-id="61fcb-1583">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1583">Err</span></span> | <span data-ttu-id="61fcb-1584">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1584">Do</span></span>  | <span data-ttu-id="61fcb-1585">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1585">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1586">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1586">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1587">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1587">Do</span></span> | <span data-ttu-id="61fcb-1588">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1588">De</span></span> | <span data-ttu-id="61fcb-1589">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1589">Si</span></span> | <span data-ttu-id="61fcb-1590">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1590">Do</span></span> | <span data-ttu-id="61fcb-1591">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1591">Err</span></span> | <span data-ttu-id="61fcb-1592">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1592">Err</span></span> | <span data-ttu-id="61fcb-1593">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1593">Do</span></span>  | <span data-ttu-id="61fcb-1594">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1594">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1595">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1595">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1596">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1596">De</span></span> | <span data-ttu-id="61fcb-1597">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1597">Si</span></span> | <span data-ttu-id="61fcb-1598">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1598">Do</span></span> | <span data-ttu-id="61fcb-1599">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1599">Err</span></span> | <span data-ttu-id="61fcb-1600">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1600">Err</span></span> | <span data-ttu-id="61fcb-1601">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1601">Do</span></span>  | <span data-ttu-id="61fcb-1602">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1602">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1603">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1603">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1604">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1604">Si</span></span> | <span data-ttu-id="61fcb-1605">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1605">Do</span></span> | <span data-ttu-id="61fcb-1606">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1606">Err</span></span> | <span data-ttu-id="61fcb-1607">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1607">Err</span></span> | <span data-ttu-id="61fcb-1608">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1608">Do</span></span>  | <span data-ttu-id="61fcb-1609">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1609">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1610">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1610">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1611">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1611">Do</span></span> | <span data-ttu-id="61fcb-1612">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1612">Err</span></span> | <span data-ttu-id="61fcb-1613">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1613">Err</span></span> | <span data-ttu-id="61fcb-1614">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1614">Do</span></span>  | <span data-ttu-id="61fcb-1615">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1615">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1616">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1616">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1617">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1617">Err</span></span> | <span data-ttu-id="61fcb-1618">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1618">Err</span></span> | <span data-ttu-id="61fcb-1619">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1619">Err</span></span> | <span data-ttu-id="61fcb-1620">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1620">Err</span></span> | 
| <span data-ttu-id="61fcb-1621">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1621">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="61fcb-1622">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1622">Err</span></span> | <span data-ttu-id="61fcb-1623">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1623">Err</span></span> | <span data-ttu-id="61fcb-1624">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1624">Err</span></span> | 
| <span data-ttu-id="61fcb-1625">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1625">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="61fcb-1626">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1626">Do</span></span>  | <span data-ttu-id="61fcb-1627">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1627">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1628">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1628">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="61fcb-1629">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1629">Ob</span></span>  | 

<span data-ttu-id="61fcb-1630">Der Ganzzahl-Divisionsoperator ist für definiert `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, und `Long`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1630">The integer division operator is defined for `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="61fcb-1631">Wenn der Wert des rechten Operanden NULL ist, ist eine `System.DivideByZeroException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1631">If the value of the right operand is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="61fcb-1632">Die Division rundet das Ergebnis in Richtung 0 (null), und der Absolute Wert des Ergebnisses wird die größte ganze Zahl möglich, die kleiner als der Absolute Wert des Quotienten aus zwei Operanden ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1632">The division rounds the result towards zero, and the absolute value of the result is the largest possible integer that is less than the absolute value of the quotient of the two operands.</span></span> <span data-ttu-id="61fcb-1633">Das Ergebnis ist 0 (null) oder positiv, wenn die beiden Operanden dasselbe Zeichen, und 0 (null) oder negativ, wenn die beiden Operanden entgegengesetzten signiert haben.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1633">The result is zero or positive when the two operands have the same sign, and zero or negative when the two operands have opposite signs.</span></span> <span data-ttu-id="61fcb-1634">Der linke Operand ist der maximale Negative `SByte`, `Short`, `Integer`, oder `Long`, und der Rechte Operand ist `-1`, ein Überlauf auftritt, wenn Überprüfungen auf Ganzzahlüberlauf on ist, eine `System.OverflowException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1634">If the left operand is the maximum negative `SByte`, `Short`, `Integer`, or `Long`, and the right operand is `-1`, an overflow occurs; if integer overflow checking is on, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="61fcb-1635">Andernfalls der Überlauf wird nicht gemeldet, und das Ergebnis wird stattdessen der Wert des linken Operanden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1635">Otherwise, the overflow is not reported and the result is instead the value of the left operand.</span></span>

<span data-ttu-id="61fcb-1636">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1636">__Note.__</span></span> <span data-ttu-id="61fcb-1637">Wenn die beiden Operanden für die Typen ohne Vorzeichen ist immer 0 (null), oder positiv ist, das Ergebnis immer 0 (null) oder positiv ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1637">As the two operands for unsigned types will always be zero or positive, the result is always zero or positive.</span></span>  <span data-ttu-id="61fcb-1638">Da das Ergebnis des Ausdrucks immer kleiner als oder gleich der größten der beiden Operanden sind, ist es nicht möglich, ein Überlauf auftreten.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1638">As the result of the expression will always be less than or equal to the largest of the two operands, it is not possible for an overflow to occur.</span></span>  <span data-ttu-id="61fcb-1639">Überprüfungen auf Ganzzahlüberlauf wird daher für die Ganzzahldivision mit zwei Ganzzahlen ohne Vorzeichen nicht ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1639">As such integer overflow checking is not performed for integer divide with two unsigned integers.</span></span> <span data-ttu-id="61fcb-1640">Das Ergebnis ist der Typ des linken Operanden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1640">The result is the type as that of the left operand.</span></span>


<span data-ttu-id="61fcb-1641">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1641">__Operation Type:__</span></span>

|        | <span data-ttu-id="61fcb-1642">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1642">__Bo__</span></span> | <span data-ttu-id="61fcb-1643">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1643">__SB__</span></span> | <span data-ttu-id="61fcb-1644">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1644">__By__</span></span> | <span data-ttu-id="61fcb-1645">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1645">__Sh__</span></span> | <span data-ttu-id="61fcb-1646">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1646">__US__</span></span> | <span data-ttu-id="61fcb-1647">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1647">__In__</span></span> | <span data-ttu-id="61fcb-1648">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1648">__UI__</span></span> | <span data-ttu-id="61fcb-1649">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1649">__Lo__</span></span> | <span data-ttu-id="61fcb-1650">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1650">__UL__</span></span> | <span data-ttu-id="61fcb-1651">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1651">__De__</span></span> | <span data-ttu-id="61fcb-1652">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1652">__Si__</span></span> | <span data-ttu-id="61fcb-1653">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1653">__Do__</span></span> | <span data-ttu-id="61fcb-1654">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1654">__Da__</span></span>  | <span data-ttu-id="61fcb-1655">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1655">__Ch__</span></span>  | <span data-ttu-id="61fcb-1656">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1656">__St__</span></span> | <span data-ttu-id="61fcb-1657">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1657">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="61fcb-1658">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1658">__Bo__</span></span> | <span data-ttu-id="61fcb-1659">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1659">Sh</span></span> | <span data-ttu-id="61fcb-1660">SB</span><span class="sxs-lookup"><span data-stu-id="61fcb-1660">SB</span></span> | <span data-ttu-id="61fcb-1661">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1661">Sh</span></span> | <span data-ttu-id="61fcb-1662">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1662">Sh</span></span> | <span data-ttu-id="61fcb-1663">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1663">In</span></span> | <span data-ttu-id="61fcb-1664">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1664">In</span></span> | <span data-ttu-id="61fcb-1665">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1665">Lo</span></span> | <span data-ttu-id="61fcb-1666">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1666">Lo</span></span> | <span data-ttu-id="61fcb-1667">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1667">Lo</span></span> | <span data-ttu-id="61fcb-1668">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1668">Lo</span></span> | <span data-ttu-id="61fcb-1669">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1669">Lo</span></span> | <span data-ttu-id="61fcb-1670">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1670">Lo</span></span> | <span data-ttu-id="61fcb-1671">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1671">Err</span></span> | <span data-ttu-id="61fcb-1672">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1672">Err</span></span> | <span data-ttu-id="61fcb-1673">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1673">Lo</span></span>  | <span data-ttu-id="61fcb-1674">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1674">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1675">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1675">__SB__</span></span> |    | <span data-ttu-id="61fcb-1676">SB</span><span class="sxs-lookup"><span data-stu-id="61fcb-1676">SB</span></span> | <span data-ttu-id="61fcb-1677">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1677">Sh</span></span> | <span data-ttu-id="61fcb-1678">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1678">Sh</span></span> | <span data-ttu-id="61fcb-1679">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1679">In</span></span> | <span data-ttu-id="61fcb-1680">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1680">In</span></span> | <span data-ttu-id="61fcb-1681">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1681">Lo</span></span> | <span data-ttu-id="61fcb-1682">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1682">Lo</span></span> | <span data-ttu-id="61fcb-1683">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1683">Lo</span></span> | <span data-ttu-id="61fcb-1684">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1684">Lo</span></span> | <span data-ttu-id="61fcb-1685">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1685">Lo</span></span> | <span data-ttu-id="61fcb-1686">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1686">Lo</span></span> | <span data-ttu-id="61fcb-1687">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1687">Err</span></span> | <span data-ttu-id="61fcb-1688">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1688">Err</span></span> | <span data-ttu-id="61fcb-1689">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1689">Lo</span></span>  | <span data-ttu-id="61fcb-1690">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1690">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1691">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1691">__By__</span></span> |    |    | <span data-ttu-id="61fcb-1692">um</span><span class="sxs-lookup"><span data-stu-id="61fcb-1692">By</span></span> | <span data-ttu-id="61fcb-1693">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1693">Sh</span></span> | <span data-ttu-id="61fcb-1694">US</span><span class="sxs-lookup"><span data-stu-id="61fcb-1694">US</span></span> | <span data-ttu-id="61fcb-1695">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1695">In</span></span> | <span data-ttu-id="61fcb-1696">UI</span><span class="sxs-lookup"><span data-stu-id="61fcb-1696">UI</span></span> | <span data-ttu-id="61fcb-1697">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1697">Lo</span></span> | <span data-ttu-id="61fcb-1698">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-1698">UL</span></span> | <span data-ttu-id="61fcb-1699">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1699">Lo</span></span> | <span data-ttu-id="61fcb-1700">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1700">Lo</span></span> | <span data-ttu-id="61fcb-1701">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1701">Lo</span></span> | <span data-ttu-id="61fcb-1702">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1702">Err</span></span> | <span data-ttu-id="61fcb-1703">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1703">Err</span></span> | <span data-ttu-id="61fcb-1704">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1704">Lo</span></span>  | <span data-ttu-id="61fcb-1705">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1705">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1706">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1706">__Sh__</span></span> |    |    |    | <span data-ttu-id="61fcb-1707">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1707">Sh</span></span> | <span data-ttu-id="61fcb-1708">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1708">In</span></span> | <span data-ttu-id="61fcb-1709">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1709">In</span></span> | <span data-ttu-id="61fcb-1710">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1710">Lo</span></span> | <span data-ttu-id="61fcb-1711">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1711">Lo</span></span> | <span data-ttu-id="61fcb-1712">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1712">Lo</span></span> | <span data-ttu-id="61fcb-1713">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1713">Lo</span></span> | <span data-ttu-id="61fcb-1714">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1714">Lo</span></span> | <span data-ttu-id="61fcb-1715">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1715">Lo</span></span> | <span data-ttu-id="61fcb-1716">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1716">Err</span></span> | <span data-ttu-id="61fcb-1717">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1717">Err</span></span> | <span data-ttu-id="61fcb-1718">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1718">Lo</span></span>  | <span data-ttu-id="61fcb-1719">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1719">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1720">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1720">__US__</span></span> |    |    |    |    | <span data-ttu-id="61fcb-1721">US</span><span class="sxs-lookup"><span data-stu-id="61fcb-1721">US</span></span> | <span data-ttu-id="61fcb-1722">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1722">In</span></span> | <span data-ttu-id="61fcb-1723">UI</span><span class="sxs-lookup"><span data-stu-id="61fcb-1723">UI</span></span> | <span data-ttu-id="61fcb-1724">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1724">Lo</span></span> | <span data-ttu-id="61fcb-1725">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-1725">UL</span></span> | <span data-ttu-id="61fcb-1726">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1726">Lo</span></span> | <span data-ttu-id="61fcb-1727">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1727">Lo</span></span> | <span data-ttu-id="61fcb-1728">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1728">Lo</span></span> | <span data-ttu-id="61fcb-1729">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1729">Err</span></span> | <span data-ttu-id="61fcb-1730">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1730">Err</span></span> | <span data-ttu-id="61fcb-1731">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1731">Lo</span></span>  | <span data-ttu-id="61fcb-1732">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1732">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1733">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1733">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="61fcb-1734">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1734">In</span></span> | <span data-ttu-id="61fcb-1735">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1735">Lo</span></span> | <span data-ttu-id="61fcb-1736">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1736">Lo</span></span> | <span data-ttu-id="61fcb-1737">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1737">Lo</span></span> | <span data-ttu-id="61fcb-1738">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1738">Lo</span></span> | <span data-ttu-id="61fcb-1739">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1739">Lo</span></span> | <span data-ttu-id="61fcb-1740">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1740">Lo</span></span> | <span data-ttu-id="61fcb-1741">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1741">Err</span></span> | <span data-ttu-id="61fcb-1742">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1742">Err</span></span> | <span data-ttu-id="61fcb-1743">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1743">Lo</span></span>  | <span data-ttu-id="61fcb-1744">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1744">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1745">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1745">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="61fcb-1746">UI</span><span class="sxs-lookup"><span data-stu-id="61fcb-1746">UI</span></span> | <span data-ttu-id="61fcb-1747">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1747">Lo</span></span> | <span data-ttu-id="61fcb-1748">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-1748">UL</span></span> | <span data-ttu-id="61fcb-1749">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1749">Lo</span></span> | <span data-ttu-id="61fcb-1750">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1750">Lo</span></span> | <span data-ttu-id="61fcb-1751">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1751">Lo</span></span> | <span data-ttu-id="61fcb-1752">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1752">Err</span></span> | <span data-ttu-id="61fcb-1753">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1753">Err</span></span> | <span data-ttu-id="61fcb-1754">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1754">Lo</span></span>  | <span data-ttu-id="61fcb-1755">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1755">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1756">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1756">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1757">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1757">Lo</span></span> | <span data-ttu-id="61fcb-1758">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1758">Lo</span></span> | <span data-ttu-id="61fcb-1759">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1759">Lo</span></span> | <span data-ttu-id="61fcb-1760">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1760">Lo</span></span> | <span data-ttu-id="61fcb-1761">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1761">Lo</span></span> | <span data-ttu-id="61fcb-1762">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1762">Err</span></span> | <span data-ttu-id="61fcb-1763">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1763">Err</span></span> | <span data-ttu-id="61fcb-1764">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1764">Lo</span></span>  | <span data-ttu-id="61fcb-1765">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1765">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1766">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1766">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1767">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-1767">UL</span></span> | <span data-ttu-id="61fcb-1768">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1768">Lo</span></span> | <span data-ttu-id="61fcb-1769">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1769">Lo</span></span> | <span data-ttu-id="61fcb-1770">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1770">Lo</span></span> | <span data-ttu-id="61fcb-1771">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1771">Err</span></span> | <span data-ttu-id="61fcb-1772">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1772">Err</span></span> | <span data-ttu-id="61fcb-1773">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1773">Lo</span></span>  | <span data-ttu-id="61fcb-1774">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1774">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1775">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1775">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1776">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1776">Lo</span></span> | <span data-ttu-id="61fcb-1777">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1777">Lo</span></span> | <span data-ttu-id="61fcb-1778">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1778">Lo</span></span> | <span data-ttu-id="61fcb-1779">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1779">Err</span></span> | <span data-ttu-id="61fcb-1780">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1780">Err</span></span> | <span data-ttu-id="61fcb-1781">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1781">Lo</span></span>  | <span data-ttu-id="61fcb-1782">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1782">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1783">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1783">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1784">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1784">Lo</span></span> | <span data-ttu-id="61fcb-1785">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1785">Lo</span></span> | <span data-ttu-id="61fcb-1786">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1786">Err</span></span> | <span data-ttu-id="61fcb-1787">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1787">Err</span></span> | <span data-ttu-id="61fcb-1788">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1788">Lo</span></span>  | <span data-ttu-id="61fcb-1789">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1789">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1790">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1790">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1791">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1791">Lo</span></span> | <span data-ttu-id="61fcb-1792">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1792">Err</span></span> | <span data-ttu-id="61fcb-1793">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1793">Err</span></span> | <span data-ttu-id="61fcb-1794">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1794">Lo</span></span>  | <span data-ttu-id="61fcb-1795">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1795">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1796">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1796">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1797">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1797">Err</span></span> | <span data-ttu-id="61fcb-1798">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1798">Err</span></span> | <span data-ttu-id="61fcb-1799">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1799">Err</span></span> | <span data-ttu-id="61fcb-1800">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1800">Err</span></span> | 
| <span data-ttu-id="61fcb-1801">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1801">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="61fcb-1802">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1802">Err</span></span> | <span data-ttu-id="61fcb-1803">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1803">Err</span></span> | <span data-ttu-id="61fcb-1804">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1804">Err</span></span> | 
| <span data-ttu-id="61fcb-1805">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1805">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="61fcb-1806">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1806">Lo</span></span>  | <span data-ttu-id="61fcb-1807">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1807">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1808">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1808">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="61fcb-1809">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1809">Ob</span></span>  | 


### <a name="mod-operator"></a><span data-ttu-id="61fcb-1810">Operator Mod</span><span class="sxs-lookup"><span data-stu-id="61fcb-1810">Mod Operator</span></span>

<span data-ttu-id="61fcb-1811">Die `Mod` (modulo)-Operator berechnet den Rest der Division zwischen zwei Operanden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1811">The `Mod` (modulo) operator computes the remainder of the division between two operands.</span></span>

```antlr
ModuloOperatorExpression
    : Expression 'Mod' LineTerminator? Expression
    ;
```

<span data-ttu-id="61fcb-1812">Die `Mod` Operator ist für die folgenden Typen definiert:</span><span class="sxs-lookup"><span data-stu-id="61fcb-1812">The `Mod` operator is defined for the following types:</span></span>

* <span data-ttu-id="61fcb-1813">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` und `Long`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1813">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` and `Long`.</span></span> <span data-ttu-id="61fcb-1814">Das Ergebnis des `x Mod y` wird durch der Wert erzeugt `x - (x \ y) * y`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1814">The result of `x Mod y` is the value produced by `x - (x \ y) * y`.</span></span> <span data-ttu-id="61fcb-1815">Wenn `y` ist 0 (null), eine `System.DivideByZeroException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1815">If `y` is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="61fcb-1816">Der modulo-Operator, löst nie einen Überlauf.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1816">The modulo operator never causes an overflow.</span></span>

* <span data-ttu-id="61fcb-1817">`Single` und `Double`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1817">`Single` and `Double`.</span></span> <span data-ttu-id="61fcb-1818">Im weiteren Verlauf wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1818">The remainder is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="61fcb-1819">`Decimal`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1819">`Decimal`.</span></span> <span data-ttu-id="61fcb-1820">Wenn der Wert des rechten Operanden NULL ist, ist eine `System.DivideByZeroException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1820">If the value of the right operand is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="61fcb-1821">Wenn der resultierende Wert für die Darstellung im decimal-Format zu groß ist eine `System.OverflowException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1821">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="61fcb-1822">Wenn der Ergebniswert zur Darstellung im Dezimalformat zu klein ist, ist das Ergebnis 0 (null).</span><span class="sxs-lookup"><span data-stu-id="61fcb-1822">If the result value is too small to represent in the decimal format, the result is zero.</span></span>

<span data-ttu-id="61fcb-1823">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1823">__Operation Type:__</span></span>

|        | <span data-ttu-id="61fcb-1824">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1824">__Bo__</span></span> | <span data-ttu-id="61fcb-1825">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1825">__SB__</span></span> | <span data-ttu-id="61fcb-1826">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1826">__By__</span></span> | <span data-ttu-id="61fcb-1827">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1827">__Sh__</span></span> | <span data-ttu-id="61fcb-1828">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1828">__US__</span></span> | <span data-ttu-id="61fcb-1829">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1829">__In__</span></span> | <span data-ttu-id="61fcb-1830">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1830">__UI__</span></span> | <span data-ttu-id="61fcb-1831">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1831">__Lo__</span></span> | <span data-ttu-id="61fcb-1832">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1832">__UL__</span></span> | <span data-ttu-id="61fcb-1833">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1833">__De__</span></span> | <span data-ttu-id="61fcb-1834">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1834">__Si__</span></span> | <span data-ttu-id="61fcb-1835">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1835">__Do__</span></span> | <span data-ttu-id="61fcb-1836">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1836">__Da__</span></span>  | <span data-ttu-id="61fcb-1837">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1837">__Ch__</span></span>  | <span data-ttu-id="61fcb-1838">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1838">__St__</span></span> | <span data-ttu-id="61fcb-1839">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1839">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="61fcb-1840">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1840">__Bo__</span></span> | <span data-ttu-id="61fcb-1841">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1841">Sh</span></span> | <span data-ttu-id="61fcb-1842">SB</span><span class="sxs-lookup"><span data-stu-id="61fcb-1842">SB</span></span> | <span data-ttu-id="61fcb-1843">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1843">Sh</span></span> | <span data-ttu-id="61fcb-1844">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1844">Sh</span></span> | <span data-ttu-id="61fcb-1845">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1845">In</span></span> | <span data-ttu-id="61fcb-1846">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1846">In</span></span> | <span data-ttu-id="61fcb-1847">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1847">Lo</span></span> | <span data-ttu-id="61fcb-1848">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1848">Lo</span></span> | <span data-ttu-id="61fcb-1849">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1849">De</span></span> | <span data-ttu-id="61fcb-1850">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1850">De</span></span> | <span data-ttu-id="61fcb-1851">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1851">Si</span></span> | <span data-ttu-id="61fcb-1852">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1852">Do</span></span> | <span data-ttu-id="61fcb-1853">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1853">Err</span></span> | <span data-ttu-id="61fcb-1854">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1854">Err</span></span> | <span data-ttu-id="61fcb-1855">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1855">Do</span></span>  | <span data-ttu-id="61fcb-1856">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1856">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1857">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1857">__SB__</span></span> |    | <span data-ttu-id="61fcb-1858">SB</span><span class="sxs-lookup"><span data-stu-id="61fcb-1858">SB</span></span> | <span data-ttu-id="61fcb-1859">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1859">Sh</span></span> | <span data-ttu-id="61fcb-1860">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1860">Sh</span></span> | <span data-ttu-id="61fcb-1861">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1861">In</span></span> | <span data-ttu-id="61fcb-1862">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1862">In</span></span> | <span data-ttu-id="61fcb-1863">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1863">Lo</span></span> | <span data-ttu-id="61fcb-1864">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1864">Lo</span></span> | <span data-ttu-id="61fcb-1865">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1865">De</span></span> | <span data-ttu-id="61fcb-1866">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1866">De</span></span> | <span data-ttu-id="61fcb-1867">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1867">Si</span></span> | <span data-ttu-id="61fcb-1868">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1868">Do</span></span> | <span data-ttu-id="61fcb-1869">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1869">Err</span></span> | <span data-ttu-id="61fcb-1870">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1870">Err</span></span> | <span data-ttu-id="61fcb-1871">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1871">Do</span></span>  | <span data-ttu-id="61fcb-1872">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1872">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1873">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1873">__By__</span></span> |    |    | <span data-ttu-id="61fcb-1874">um</span><span class="sxs-lookup"><span data-stu-id="61fcb-1874">By</span></span> | <span data-ttu-id="61fcb-1875">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1875">Sh</span></span> | <span data-ttu-id="61fcb-1876">US</span><span class="sxs-lookup"><span data-stu-id="61fcb-1876">US</span></span> | <span data-ttu-id="61fcb-1877">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1877">In</span></span> | <span data-ttu-id="61fcb-1878">UI</span><span class="sxs-lookup"><span data-stu-id="61fcb-1878">UI</span></span> | <span data-ttu-id="61fcb-1879">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1879">Lo</span></span> | <span data-ttu-id="61fcb-1880">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-1880">UL</span></span> | <span data-ttu-id="61fcb-1881">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1881">De</span></span> | <span data-ttu-id="61fcb-1882">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1882">Si</span></span> | <span data-ttu-id="61fcb-1883">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1883">Do</span></span> | <span data-ttu-id="61fcb-1884">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1884">Err</span></span> | <span data-ttu-id="61fcb-1885">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1885">Err</span></span> | <span data-ttu-id="61fcb-1886">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1886">Do</span></span>  | <span data-ttu-id="61fcb-1887">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1887">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1888">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1888">__Sh__</span></span> |    |    |    | <span data-ttu-id="61fcb-1889">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-1889">Sh</span></span> | <span data-ttu-id="61fcb-1890">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1890">In</span></span> | <span data-ttu-id="61fcb-1891">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1891">In</span></span> | <span data-ttu-id="61fcb-1892">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1892">Lo</span></span> | <span data-ttu-id="61fcb-1893">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1893">Lo</span></span> | <span data-ttu-id="61fcb-1894">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1894">De</span></span> | <span data-ttu-id="61fcb-1895">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1895">De</span></span> | <span data-ttu-id="61fcb-1896">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1896">Si</span></span> | <span data-ttu-id="61fcb-1897">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1897">Do</span></span> | <span data-ttu-id="61fcb-1898">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1898">Err</span></span> | <span data-ttu-id="61fcb-1899">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1899">Err</span></span> | <span data-ttu-id="61fcb-1900">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1900">Do</span></span>  | <span data-ttu-id="61fcb-1901">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1901">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1902">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1902">__US__</span></span> |    |    |    |    | <span data-ttu-id="61fcb-1903">US</span><span class="sxs-lookup"><span data-stu-id="61fcb-1903">US</span></span> | <span data-ttu-id="61fcb-1904">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1904">In</span></span> | <span data-ttu-id="61fcb-1905">UI</span><span class="sxs-lookup"><span data-stu-id="61fcb-1905">UI</span></span> | <span data-ttu-id="61fcb-1906">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1906">Lo</span></span> | <span data-ttu-id="61fcb-1907">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-1907">UL</span></span> | <span data-ttu-id="61fcb-1908">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1908">De</span></span> | <span data-ttu-id="61fcb-1909">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1909">Si</span></span> | <span data-ttu-id="61fcb-1910">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1910">Do</span></span> | <span data-ttu-id="61fcb-1911">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1911">Err</span></span> | <span data-ttu-id="61fcb-1912">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1912">Err</span></span> | <span data-ttu-id="61fcb-1913">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1913">Do</span></span>  | <span data-ttu-id="61fcb-1914">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1914">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1915">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1915">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="61fcb-1916">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-1916">In</span></span> | <span data-ttu-id="61fcb-1917">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1917">Lo</span></span> | <span data-ttu-id="61fcb-1918">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1918">Lo</span></span> | <span data-ttu-id="61fcb-1919">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1919">De</span></span> | <span data-ttu-id="61fcb-1920">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1920">De</span></span> | <span data-ttu-id="61fcb-1921">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1921">Si</span></span> | <span data-ttu-id="61fcb-1922">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1922">Do</span></span> | <span data-ttu-id="61fcb-1923">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1923">Err</span></span> | <span data-ttu-id="61fcb-1924">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1924">Err</span></span> | <span data-ttu-id="61fcb-1925">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1925">Do</span></span>  | <span data-ttu-id="61fcb-1926">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1926">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1927">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1927">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="61fcb-1928">UI</span><span class="sxs-lookup"><span data-stu-id="61fcb-1928">UI</span></span> | <span data-ttu-id="61fcb-1929">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1929">Lo</span></span> | <span data-ttu-id="61fcb-1930">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-1930">UL</span></span> | <span data-ttu-id="61fcb-1931">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1931">De</span></span> | <span data-ttu-id="61fcb-1932">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1932">Si</span></span> | <span data-ttu-id="61fcb-1933">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1933">Do</span></span> | <span data-ttu-id="61fcb-1934">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1934">Err</span></span> | <span data-ttu-id="61fcb-1935">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1935">Err</span></span> | <span data-ttu-id="61fcb-1936">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1936">Do</span></span>  | <span data-ttu-id="61fcb-1937">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1937">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1938">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1938">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1939">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-1939">Lo</span></span> | <span data-ttu-id="61fcb-1940">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1940">De</span></span> | <span data-ttu-id="61fcb-1941">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1941">De</span></span> | <span data-ttu-id="61fcb-1942">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1942">Si</span></span> | <span data-ttu-id="61fcb-1943">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1943">Do</span></span> | <span data-ttu-id="61fcb-1944">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1944">Err</span></span> | <span data-ttu-id="61fcb-1945">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1945">Err</span></span> | <span data-ttu-id="61fcb-1946">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1946">Do</span></span>  | <span data-ttu-id="61fcb-1947">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1947">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1948">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1948">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1949">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-1949">UL</span></span> | <span data-ttu-id="61fcb-1950">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1950">De</span></span> | <span data-ttu-id="61fcb-1951">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1951">Si</span></span> | <span data-ttu-id="61fcb-1952">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1952">Do</span></span> | <span data-ttu-id="61fcb-1953">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1953">Err</span></span> | <span data-ttu-id="61fcb-1954">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1954">Err</span></span> | <span data-ttu-id="61fcb-1955">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1955">Do</span></span>  | <span data-ttu-id="61fcb-1956">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1956">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1957">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1957">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1958">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-1958">De</span></span> | <span data-ttu-id="61fcb-1959">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1959">Si</span></span> | <span data-ttu-id="61fcb-1960">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1960">Do</span></span> | <span data-ttu-id="61fcb-1961">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1961">Err</span></span> | <span data-ttu-id="61fcb-1962">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1962">Err</span></span> | <span data-ttu-id="61fcb-1963">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1963">Do</span></span>  | <span data-ttu-id="61fcb-1964">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1964">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1965">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1965">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1966">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-1966">Si</span></span> | <span data-ttu-id="61fcb-1967">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1967">Do</span></span> | <span data-ttu-id="61fcb-1968">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1968">Err</span></span> | <span data-ttu-id="61fcb-1969">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1969">Err</span></span> | <span data-ttu-id="61fcb-1970">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1970">Do</span></span>  | <span data-ttu-id="61fcb-1971">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1971">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1972">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1972">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1973">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1973">Do</span></span> | <span data-ttu-id="61fcb-1974">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1974">Err</span></span> | <span data-ttu-id="61fcb-1975">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1975">Err</span></span> | <span data-ttu-id="61fcb-1976">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1976">Do</span></span>  | <span data-ttu-id="61fcb-1977">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1977">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1978">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1978">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-1979">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1979">Err</span></span> | <span data-ttu-id="61fcb-1980">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1980">Err</span></span> | <span data-ttu-id="61fcb-1981">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1981">Err</span></span> | <span data-ttu-id="61fcb-1982">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1982">Err</span></span> | 
| <span data-ttu-id="61fcb-1983">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1983">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="61fcb-1984">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1984">Err</span></span> | <span data-ttu-id="61fcb-1985">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1985">Err</span></span> | <span data-ttu-id="61fcb-1986">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-1986">Err</span></span> | 
| <span data-ttu-id="61fcb-1987">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1987">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="61fcb-1988">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-1988">Do</span></span>  | <span data-ttu-id="61fcb-1989">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1989">Ob</span></span>  | 
| <span data-ttu-id="61fcb-1990">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1990">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="61fcb-1991">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-1991">Ob</span></span>  | 


### <a name="exponentiation-operator"></a><span data-ttu-id="61fcb-1992">Exponentialoperator</span><span class="sxs-lookup"><span data-stu-id="61fcb-1992">Exponentiation Operator</span></span>

<span data-ttu-id="61fcb-1993">Der Potenzierungsoperator berechnet der erste Operand, der die Potenz des zweiten Operanden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1993">The exponentiation operator computes the first operand raised to the power of the second operand.</span></span>

```antlr
ExponentOperatorExpression
    : Expression '^' LineTerminator? Expression
    ;
```

<span data-ttu-id="61fcb-1994">Der Potenzierungsoperator ist für den Typ definiert `Double`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1994">The exponentiation operator is defined for type `Double`.</span></span> <span data-ttu-id="61fcb-1995">Der Wert wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-1995">The value is computed according to the rules of IEEE 754 arithmetic.</span></span>

<span data-ttu-id="61fcb-1996">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1996">__Operation Type:__</span></span>

|        | <span data-ttu-id="61fcb-1997">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1997">__Bo__</span></span> | <span data-ttu-id="61fcb-1998">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1998">__SB__</span></span> | <span data-ttu-id="61fcb-1999">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-1999">__By__</span></span> | <span data-ttu-id="61fcb-2000">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2000">__Sh__</span></span> | <span data-ttu-id="61fcb-2001">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2001">__US__</span></span> | <span data-ttu-id="61fcb-2002">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2002">__In__</span></span> | <span data-ttu-id="61fcb-2003">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2003">__UI__</span></span> | <span data-ttu-id="61fcb-2004">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2004">__Lo__</span></span> | <span data-ttu-id="61fcb-2005">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2005">__UL__</span></span> | <span data-ttu-id="61fcb-2006">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2006">__De__</span></span> | <span data-ttu-id="61fcb-2007">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2007">__Si__</span></span> | <span data-ttu-id="61fcb-2008">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2008">__Do__</span></span> | <span data-ttu-id="61fcb-2009">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2009">__Da__</span></span>  | <span data-ttu-id="61fcb-2010">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2010">__Ch__</span></span>  | <span data-ttu-id="61fcb-2011">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2011">__St__</span></span> | <span data-ttu-id="61fcb-2012">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2012">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="61fcb-2013">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2013">__Bo__</span></span> | <span data-ttu-id="61fcb-2014">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2014">Do</span></span> | <span data-ttu-id="61fcb-2015">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2015">Do</span></span> | <span data-ttu-id="61fcb-2016">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2016">Do</span></span> | <span data-ttu-id="61fcb-2017">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2017">Do</span></span> | <span data-ttu-id="61fcb-2018">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2018">Do</span></span> | <span data-ttu-id="61fcb-2019">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2019">Do</span></span> | <span data-ttu-id="61fcb-2020">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2020">Do</span></span> | <span data-ttu-id="61fcb-2021">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2021">Do</span></span> | <span data-ttu-id="61fcb-2022">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2022">Do</span></span> | <span data-ttu-id="61fcb-2023">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2023">Do</span></span> | <span data-ttu-id="61fcb-2024">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2024">Do</span></span> | <span data-ttu-id="61fcb-2025">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2025">Do</span></span> | <span data-ttu-id="61fcb-2026">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2026">Err</span></span> | <span data-ttu-id="61fcb-2027">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2027">Err</span></span> | <span data-ttu-id="61fcb-2028">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2028">Do</span></span>  | <span data-ttu-id="61fcb-2029">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2029">Ob</span></span>  | 
| <span data-ttu-id="61fcb-2030">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2030">__SB__</span></span> |    | <span data-ttu-id="61fcb-2031">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2031">Do</span></span> | <span data-ttu-id="61fcb-2032">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2032">Do</span></span> | <span data-ttu-id="61fcb-2033">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2033">Do</span></span> | <span data-ttu-id="61fcb-2034">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2034">Do</span></span> | <span data-ttu-id="61fcb-2035">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2035">Do</span></span> | <span data-ttu-id="61fcb-2036">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2036">Do</span></span> | <span data-ttu-id="61fcb-2037">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2037">Do</span></span> | <span data-ttu-id="61fcb-2038">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2038">Do</span></span> | <span data-ttu-id="61fcb-2039">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2039">Do</span></span> | <span data-ttu-id="61fcb-2040">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2040">Do</span></span> | <span data-ttu-id="61fcb-2041">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2041">Do</span></span> | <span data-ttu-id="61fcb-2042">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2042">Err</span></span> | <span data-ttu-id="61fcb-2043">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2043">Err</span></span> | <span data-ttu-id="61fcb-2044">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2044">Do</span></span>  | <span data-ttu-id="61fcb-2045">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2045">Ob</span></span>  | 
| <span data-ttu-id="61fcb-2046">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2046">__By__</span></span> |    |    | <span data-ttu-id="61fcb-2047">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2047">Do</span></span> | <span data-ttu-id="61fcb-2048">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2048">Do</span></span> | <span data-ttu-id="61fcb-2049">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2049">Do</span></span> | <span data-ttu-id="61fcb-2050">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2050">Do</span></span> | <span data-ttu-id="61fcb-2051">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2051">Do</span></span> | <span data-ttu-id="61fcb-2052">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2052">Do</span></span> | <span data-ttu-id="61fcb-2053">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2053">Do</span></span> | <span data-ttu-id="61fcb-2054">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2054">Do</span></span> | <span data-ttu-id="61fcb-2055">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2055">Do</span></span> | <span data-ttu-id="61fcb-2056">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2056">Do</span></span> | <span data-ttu-id="61fcb-2057">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2057">Err</span></span> | <span data-ttu-id="61fcb-2058">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2058">Err</span></span> | <span data-ttu-id="61fcb-2059">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2059">Do</span></span>  | <span data-ttu-id="61fcb-2060">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2060">Ob</span></span>  | 
| <span data-ttu-id="61fcb-2061">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2061">__Sh__</span></span> |    |    |    | <span data-ttu-id="61fcb-2062">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2062">Do</span></span> | <span data-ttu-id="61fcb-2063">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2063">Do</span></span> | <span data-ttu-id="61fcb-2064">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2064">Do</span></span> | <span data-ttu-id="61fcb-2065">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2065">Do</span></span> | <span data-ttu-id="61fcb-2066">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2066">Do</span></span> | <span data-ttu-id="61fcb-2067">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2067">Do</span></span> | <span data-ttu-id="61fcb-2068">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2068">Do</span></span> | <span data-ttu-id="61fcb-2069">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2069">Do</span></span> | <span data-ttu-id="61fcb-2070">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2070">Do</span></span> | <span data-ttu-id="61fcb-2071">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2071">Err</span></span> | <span data-ttu-id="61fcb-2072">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2072">Err</span></span> | <span data-ttu-id="61fcb-2073">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2073">Do</span></span>  | <span data-ttu-id="61fcb-2074">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2074">Ob</span></span>  | 
| <span data-ttu-id="61fcb-2075">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2075">__US__</span></span> |    |    |    |    | <span data-ttu-id="61fcb-2076">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2076">Do</span></span> | <span data-ttu-id="61fcb-2077">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2077">Do</span></span> | <span data-ttu-id="61fcb-2078">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2078">Do</span></span> | <span data-ttu-id="61fcb-2079">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2079">Do</span></span> | <span data-ttu-id="61fcb-2080">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2080">Do</span></span> | <span data-ttu-id="61fcb-2081">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2081">Do</span></span> | <span data-ttu-id="61fcb-2082">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2082">Do</span></span> | <span data-ttu-id="61fcb-2083">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2083">Do</span></span> | <span data-ttu-id="61fcb-2084">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2084">Err</span></span> | <span data-ttu-id="61fcb-2085">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2085">Err</span></span> | <span data-ttu-id="61fcb-2086">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2086">Do</span></span>  | <span data-ttu-id="61fcb-2087">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2087">Ob</span></span>  | 
| <span data-ttu-id="61fcb-2088">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2088">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="61fcb-2089">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2089">Do</span></span> | <span data-ttu-id="61fcb-2090">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2090">Do</span></span> | <span data-ttu-id="61fcb-2091">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2091">Do</span></span> | <span data-ttu-id="61fcb-2092">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2092">Do</span></span> | <span data-ttu-id="61fcb-2093">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2093">Do</span></span> | <span data-ttu-id="61fcb-2094">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2094">Do</span></span> | <span data-ttu-id="61fcb-2095">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2095">Do</span></span> | <span data-ttu-id="61fcb-2096">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2096">Err</span></span> | <span data-ttu-id="61fcb-2097">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2097">Err</span></span> | <span data-ttu-id="61fcb-2098">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2098">Do</span></span>  | <span data-ttu-id="61fcb-2099">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2099">Ob</span></span>  | 
| <span data-ttu-id="61fcb-2100">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2100">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="61fcb-2101">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2101">Do</span></span> | <span data-ttu-id="61fcb-2102">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2102">Do</span></span> | <span data-ttu-id="61fcb-2103">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2103">Do</span></span> | <span data-ttu-id="61fcb-2104">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2104">Do</span></span> | <span data-ttu-id="61fcb-2105">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2105">Do</span></span> | <span data-ttu-id="61fcb-2106">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2106">Do</span></span> | <span data-ttu-id="61fcb-2107">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2107">Err</span></span> | <span data-ttu-id="61fcb-2108">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2108">Err</span></span> | <span data-ttu-id="61fcb-2109">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2109">Do</span></span>  | <span data-ttu-id="61fcb-2110">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2110">Ob</span></span>  | 
| <span data-ttu-id="61fcb-2111">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2111">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2112">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2112">Do</span></span> | <span data-ttu-id="61fcb-2113">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2113">Do</span></span> | <span data-ttu-id="61fcb-2114">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2114">Do</span></span> | <span data-ttu-id="61fcb-2115">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2115">Do</span></span> | <span data-ttu-id="61fcb-2116">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2116">Do</span></span> | <span data-ttu-id="61fcb-2117">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2117">Err</span></span> | <span data-ttu-id="61fcb-2118">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2118">Err</span></span> | <span data-ttu-id="61fcb-2119">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2119">Do</span></span>  | <span data-ttu-id="61fcb-2120">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2120">Ob</span></span>  | 
| <span data-ttu-id="61fcb-2121">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2121">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2122">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2122">Do</span></span> | <span data-ttu-id="61fcb-2123">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2123">Do</span></span> | <span data-ttu-id="61fcb-2124">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2124">Do</span></span> | <span data-ttu-id="61fcb-2125">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2125">Do</span></span> | <span data-ttu-id="61fcb-2126">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2126">Err</span></span> | <span data-ttu-id="61fcb-2127">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2127">Err</span></span> | <span data-ttu-id="61fcb-2128">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2128">Do</span></span>  | <span data-ttu-id="61fcb-2129">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2129">Ob</span></span>  | 
| <span data-ttu-id="61fcb-2130">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2130">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2131">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2131">Do</span></span> | <span data-ttu-id="61fcb-2132">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2132">Do</span></span> | <span data-ttu-id="61fcb-2133">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2133">Do</span></span> | <span data-ttu-id="61fcb-2134">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2134">Err</span></span> | <span data-ttu-id="61fcb-2135">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2135">Err</span></span> | <span data-ttu-id="61fcb-2136">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2136">Do</span></span>  | <span data-ttu-id="61fcb-2137">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2137">Ob</span></span>  | 
| <span data-ttu-id="61fcb-2138">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2138">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2139">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2139">Do</span></span> | <span data-ttu-id="61fcb-2140">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2140">Do</span></span> | <span data-ttu-id="61fcb-2141">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2141">Err</span></span> | <span data-ttu-id="61fcb-2142">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2142">Err</span></span> | <span data-ttu-id="61fcb-2143">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2143">Do</span></span>  | <span data-ttu-id="61fcb-2144">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2144">Ob</span></span>  | 
| <span data-ttu-id="61fcb-2145">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2145">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2146">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2146">Do</span></span> | <span data-ttu-id="61fcb-2147">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2147">Err</span></span> | <span data-ttu-id="61fcb-2148">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2148">Err</span></span> | <span data-ttu-id="61fcb-2149">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2149">Do</span></span>  | <span data-ttu-id="61fcb-2150">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2150">Ob</span></span>  | 
| <span data-ttu-id="61fcb-2151">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2151">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2152">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2152">Err</span></span> | <span data-ttu-id="61fcb-2153">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2153">Err</span></span> | <span data-ttu-id="61fcb-2154">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2154">Err</span></span> | <span data-ttu-id="61fcb-2155">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2155">Err</span></span> | 
| <span data-ttu-id="61fcb-2156">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2156">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="61fcb-2157">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2157">Err</span></span> | <span data-ttu-id="61fcb-2158">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2158">Err</span></span> | <span data-ttu-id="61fcb-2159">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2159">Err</span></span> | 
| <span data-ttu-id="61fcb-2160">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2160">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="61fcb-2161">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2161">Do</span></span>  | <span data-ttu-id="61fcb-2162">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2162">Ob</span></span>  | 
| <span data-ttu-id="61fcb-2163">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2163">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="61fcb-2164">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2164">Ob</span></span>  | 


## <a name="relational-operators"></a><span data-ttu-id="61fcb-2165">Operatoren (relational)</span><span class="sxs-lookup"><span data-stu-id="61fcb-2165">Relational Operators</span></span>

<span data-ttu-id="61fcb-2166">Die *relationalen Operatoren* Werte miteinander verglichen werden soll.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2166">The *relational operators* compare values to one other.</span></span> <span data-ttu-id="61fcb-2167">Die Vergleichsoperatoren `=`, `<>`, `<`, `>`, `<=`, und `>=`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2167">The comparison operators are `=`, `<>`, `<`, `>`, `<=`, and `>=`.</span></span>

```antlr
RelationalOperatorExpression
    : Expression '=' LineTerminator? Expression
    | Expression '<' '>' LineTerminator? Expression
    | Expression '<' LineTerminator? Expression
    | Expression '>' LineTerminator? Expression
    | Expression '<' '=' LineTerminator? Expression
    | Expression '>' '=' LineTerminator? Expression
    ;
```

<span data-ttu-id="61fcb-2168">Alle relationalen Operatoren führen eine `Boolean` Wert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2168">All of the relational operators result in a `Boolean` value.</span></span>

<span data-ttu-id="61fcb-2169">Die relationalen Operatoren haben folgende Bedeutung: Allgemeine:</span><span class="sxs-lookup"><span data-stu-id="61fcb-2169">The relational operators have the following general meaning:</span></span>

* <span data-ttu-id="61fcb-2170">Die `=` -Operator testet, ob, ob die beiden Operanden gleich sind.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2170">The `=` operator tests whether the two operands are equal.</span></span>

* <span data-ttu-id="61fcb-2171">Die `<>` -Operator testet, ob, ob die beiden Operanden nicht gleich sind.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2171">The `<>` operator tests whether the two operands are not equal.</span></span>

* <span data-ttu-id="61fcb-2172">Die `<` -Operator testet, ob der erste Operand kleiner als der zweite Operand.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2172">The `<` operator tests whether the first operand is less than the second operand.</span></span>

* <span data-ttu-id="61fcb-2173">Die `>` -Operator testet, ob, ob der erste Operand größer als der zweite Operand ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2173">The `>` operator tests whether the first operand is greater than the second operand.</span></span>

* <span data-ttu-id="61fcb-2174">Die `<=` -Operator testet, ob, ob der erste Operand kleiner als oder gleich der zweite Operand ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2174">The `<=` operator tests whether the first operand is less than or equal to the second operand.</span></span>

* <span data-ttu-id="61fcb-2175">Die `>=` -Operator testet, ob, ob der erste Operand größer als oder gleich dem zweiten Operand ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2175">The `>=` operator tests whether the first operand is greater than or equal to the second operand.</span></span>

<span data-ttu-id="61fcb-2176">Die relationalen Operatoren werden für die folgenden Typen definiert:</span><span class="sxs-lookup"><span data-stu-id="61fcb-2176">The relational operators are defined for the following types:</span></span>

* <span data-ttu-id="61fcb-2177">`Boolean`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2177">`Boolean`.</span></span> <span data-ttu-id="61fcb-2178">Die Operatoren vergleichen Sie die Wahrheit-Werte der beiden Operanden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2178">The operators compare the truth values of the two operands.</span></span> <span data-ttu-id="61fcb-2179">`True` gilt als kleiner als `False`, der mit den entsprechenden numerischen Werten entspricht.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2179">`True` is considered to be less than `False`, which matches with their numeric values.</span></span>

* <span data-ttu-id="61fcb-2180">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` und `Long`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2180">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="61fcb-2181">Die Operatoren vergleichen die numerischen Werte der beiden ganzzahligen Operanden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2181">The operators compare the numeric values of the two integral operands.</span></span>

* <span data-ttu-id="61fcb-2182">`Single` und `Double`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2182">`Single` and `Double`.</span></span> <span data-ttu-id="61fcb-2183">Die Operatoren vergleichen die Operanden gemäß den Regeln der IEEE 754-standard.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2183">The operators compare the operands according to the rules of the IEEE 754 standard.</span></span>

* <span data-ttu-id="61fcb-2184">`Decimal`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2184">`Decimal`.</span></span> <span data-ttu-id="61fcb-2185">Die Operatoren vergleichen die numerischen Werte der beiden Operanden decimal.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2185">The operators compare the numeric values of the two decimal operands.</span></span>

* <span data-ttu-id="61fcb-2186">`Date`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2186">`Date`.</span></span> <span data-ttu-id="61fcb-2187">Die Operatoren geben das Ergebnis des Vergleichs die zwei Datum/Uhrzeit-Werte zurück.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2187">The operators return the result of comparing the two date/time values.</span></span>

* <span data-ttu-id="61fcb-2188">`Char`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2188">`Char`.</span></span> <span data-ttu-id="61fcb-2189">Die Operatoren geben das Ergebnis des Vergleichs der zwei Unicode-Werte zurück.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2189">The operators return the result of comparing the two Unicode values.</span></span>

* <span data-ttu-id="61fcb-2190">`String`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2190">`String`.</span></span> <span data-ttu-id="61fcb-2191">Die Operatoren geben das Ergebnis des Vergleichs die beiden Werte, die über einen binären Vergleich oder ein Textvergleich zurück.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2191">The operators return the result of comparing the two values using either a binary comparison or a text comparison.</span></span> <span data-ttu-id="61fcb-2192">Der Vergleich verwendet, richtet sich nach der kompilierungsumgebung und die `Option Compare` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2192">The comparison used is determined by the compilation environment and the `Option Compare` statement.</span></span> <span data-ttu-id="61fcb-2193">Ein binärer Vergleich bestimmt, ob es sich bei der numerische Unicode-Wert jedes Zeichens in jeder Zeichenfolge übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2193">A binary comparison determines whether the numeric Unicode value of each character in each string is the same.</span></span> <span data-ttu-id="61fcb-2194">Ein Textvergleich ist einen Unicode-Vergleich, Text basierend auf der aktuellen Kultur verwendet, die auf .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2194">A text comparison does a Unicode text comparison based on the current culture in use on the .NET Framework.</span></span> <span data-ttu-id="61fcb-2195">Bei einem Zeichenfolgenvergleich, ein null-Wert entspricht der literalen Zeichenfolge `""`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2195">When doing a string comparison, a null value is equivalent to the string literal `""`.</span></span>

<span data-ttu-id="61fcb-2196">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2196">__Operation Type:__</span></span>
        
|        | <span data-ttu-id="61fcb-2197">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2197">__Bo__</span></span> | <span data-ttu-id="61fcb-2198">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2198">__SB__</span></span> | <span data-ttu-id="61fcb-2199">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2199">__By__</span></span> | <span data-ttu-id="61fcb-2200">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2200">__Sh__</span></span> | <span data-ttu-id="61fcb-2201">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2201">__US__</span></span> | <span data-ttu-id="61fcb-2202">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2202">__In__</span></span> | <span data-ttu-id="61fcb-2203">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2203">__UI__</span></span> | <span data-ttu-id="61fcb-2204">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2204">__Lo__</span></span> | <span data-ttu-id="61fcb-2205">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2205">__UL__</span></span> | <span data-ttu-id="61fcb-2206">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2206">__De__</span></span> | <span data-ttu-id="61fcb-2207">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2207">__Si__</span></span> | <span data-ttu-id="61fcb-2208">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2208">__Do__</span></span> | <span data-ttu-id="61fcb-2209">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2209">__Da__</span></span>  | <span data-ttu-id="61fcb-2210">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2210">__Ch__</span></span>  | <span data-ttu-id="61fcb-2211">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2211">__St__</span></span> | <span data-ttu-id="61fcb-2212">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2212">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="61fcb-2213">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2213">__Bo__</span></span> | <span data-ttu-id="61fcb-2214">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-2214">Bo</span></span> | <span data-ttu-id="61fcb-2215">SB</span><span class="sxs-lookup"><span data-stu-id="61fcb-2215">SB</span></span> | <span data-ttu-id="61fcb-2216">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-2216">Sh</span></span> | <span data-ttu-id="61fcb-2217">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-2217">Sh</span></span> | <span data-ttu-id="61fcb-2218">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-2218">In</span></span> | <span data-ttu-id="61fcb-2219">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-2219">In</span></span> | <span data-ttu-id="61fcb-2220">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2220">Lo</span></span> | <span data-ttu-id="61fcb-2221">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2221">Lo</span></span> | <span data-ttu-id="61fcb-2222">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-2222">De</span></span> | <span data-ttu-id="61fcb-2223">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-2223">De</span></span> | <span data-ttu-id="61fcb-2224">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-2224">Si</span></span> | <span data-ttu-id="61fcb-2225">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2225">Do</span></span> | <span data-ttu-id="61fcb-2226">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2226">Err</span></span> | <span data-ttu-id="61fcb-2227">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2227">Err</span></span> | <span data-ttu-id="61fcb-2228">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-2228">Bo</span></span> | <span data-ttu-id="61fcb-2229">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2229">Ob</span></span> | 
| <span data-ttu-id="61fcb-2230">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2230">__SB__</span></span> |    | <span data-ttu-id="61fcb-2231">SB</span><span class="sxs-lookup"><span data-stu-id="61fcb-2231">SB</span></span> | <span data-ttu-id="61fcb-2232">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-2232">Sh</span></span> | <span data-ttu-id="61fcb-2233">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-2233">Sh</span></span> | <span data-ttu-id="61fcb-2234">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-2234">In</span></span> | <span data-ttu-id="61fcb-2235">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-2235">In</span></span> | <span data-ttu-id="61fcb-2236">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2236">Lo</span></span> | <span data-ttu-id="61fcb-2237">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2237">Lo</span></span> | <span data-ttu-id="61fcb-2238">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-2238">De</span></span> | <span data-ttu-id="61fcb-2239">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-2239">De</span></span> | <span data-ttu-id="61fcb-2240">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-2240">Si</span></span> | <span data-ttu-id="61fcb-2241">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2241">Do</span></span> | <span data-ttu-id="61fcb-2242">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2242">Err</span></span> | <span data-ttu-id="61fcb-2243">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2243">Err</span></span> | <span data-ttu-id="61fcb-2244">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2244">Do</span></span> | <span data-ttu-id="61fcb-2245">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2245">Ob</span></span> | 
| <span data-ttu-id="61fcb-2246">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2246">__By__</span></span> |    |    | <span data-ttu-id="61fcb-2247">um</span><span class="sxs-lookup"><span data-stu-id="61fcb-2247">By</span></span> | <span data-ttu-id="61fcb-2248">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-2248">Sh</span></span> | <span data-ttu-id="61fcb-2249">US</span><span class="sxs-lookup"><span data-stu-id="61fcb-2249">US</span></span> | <span data-ttu-id="61fcb-2250">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-2250">In</span></span> | <span data-ttu-id="61fcb-2251">UI</span><span class="sxs-lookup"><span data-stu-id="61fcb-2251">UI</span></span> | <span data-ttu-id="61fcb-2252">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2252">Lo</span></span> | <span data-ttu-id="61fcb-2253">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-2253">UL</span></span> | <span data-ttu-id="61fcb-2254">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-2254">De</span></span> | <span data-ttu-id="61fcb-2255">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-2255">Si</span></span> | <span data-ttu-id="61fcb-2256">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2256">Do</span></span> | <span data-ttu-id="61fcb-2257">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2257">Err</span></span> | <span data-ttu-id="61fcb-2258">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2258">Err</span></span> | <span data-ttu-id="61fcb-2259">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2259">Do</span></span> | <span data-ttu-id="61fcb-2260">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2260">Ob</span></span> | 
| <span data-ttu-id="61fcb-2261">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2261">__Sh__</span></span> |    |    |    | <span data-ttu-id="61fcb-2262">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-2262">Sh</span></span> | <span data-ttu-id="61fcb-2263">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-2263">In</span></span> | <span data-ttu-id="61fcb-2264">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-2264">In</span></span> | <span data-ttu-id="61fcb-2265">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2265">Lo</span></span> | <span data-ttu-id="61fcb-2266">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2266">Lo</span></span> | <span data-ttu-id="61fcb-2267">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-2267">De</span></span> | <span data-ttu-id="61fcb-2268">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-2268">De</span></span> | <span data-ttu-id="61fcb-2269">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-2269">Si</span></span> | <span data-ttu-id="61fcb-2270">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2270">Do</span></span> | <span data-ttu-id="61fcb-2271">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2271">Err</span></span> | <span data-ttu-id="61fcb-2272">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2272">Err</span></span> | <span data-ttu-id="61fcb-2273">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2273">Do</span></span> | <span data-ttu-id="61fcb-2274">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2274">Ob</span></span> | 
| <span data-ttu-id="61fcb-2275">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2275">__US__</span></span> |    |    |    |    | <span data-ttu-id="61fcb-2276">US</span><span class="sxs-lookup"><span data-stu-id="61fcb-2276">US</span></span> | <span data-ttu-id="61fcb-2277">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-2277">In</span></span> | <span data-ttu-id="61fcb-2278">UI</span><span class="sxs-lookup"><span data-stu-id="61fcb-2278">UI</span></span> | <span data-ttu-id="61fcb-2279">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2279">Lo</span></span> | <span data-ttu-id="61fcb-2280">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-2280">UL</span></span> | <span data-ttu-id="61fcb-2281">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-2281">De</span></span> | <span data-ttu-id="61fcb-2282">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-2282">Si</span></span> | <span data-ttu-id="61fcb-2283">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2283">Do</span></span> | <span data-ttu-id="61fcb-2284">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2284">Err</span></span> | <span data-ttu-id="61fcb-2285">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2285">Err</span></span> | <span data-ttu-id="61fcb-2286">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2286">Do</span></span> | <span data-ttu-id="61fcb-2287">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2287">Ob</span></span> | 
| <span data-ttu-id="61fcb-2288">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2288">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="61fcb-2289">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-2289">In</span></span> | <span data-ttu-id="61fcb-2290">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2290">Lo</span></span> | <span data-ttu-id="61fcb-2291">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2291">Lo</span></span> | <span data-ttu-id="61fcb-2292">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-2292">De</span></span> | <span data-ttu-id="61fcb-2293">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-2293">De</span></span> | <span data-ttu-id="61fcb-2294">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-2294">Si</span></span> | <span data-ttu-id="61fcb-2295">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2295">Do</span></span> | <span data-ttu-id="61fcb-2296">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2296">Err</span></span> | <span data-ttu-id="61fcb-2297">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2297">Err</span></span> | <span data-ttu-id="61fcb-2298">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2298">Do</span></span> | <span data-ttu-id="61fcb-2299">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2299">Ob</span></span> | 
| <span data-ttu-id="61fcb-2300">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2300">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="61fcb-2301">UI</span><span class="sxs-lookup"><span data-stu-id="61fcb-2301">UI</span></span> | <span data-ttu-id="61fcb-2302">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2302">Lo</span></span> | <span data-ttu-id="61fcb-2303">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-2303">UL</span></span> | <span data-ttu-id="61fcb-2304">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-2304">De</span></span> | <span data-ttu-id="61fcb-2305">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-2305">Si</span></span> | <span data-ttu-id="61fcb-2306">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2306">Do</span></span> | <span data-ttu-id="61fcb-2307">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2307">Err</span></span> | <span data-ttu-id="61fcb-2308">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2308">Err</span></span> | <span data-ttu-id="61fcb-2309">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2309">Do</span></span> | <span data-ttu-id="61fcb-2310">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2310">Ob</span></span> | 
| <span data-ttu-id="61fcb-2311">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2311">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2312">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2312">Lo</span></span> | <span data-ttu-id="61fcb-2313">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-2313">De</span></span> | <span data-ttu-id="61fcb-2314">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-2314">De</span></span> | <span data-ttu-id="61fcb-2315">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-2315">Si</span></span> | <span data-ttu-id="61fcb-2316">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2316">Do</span></span> | <span data-ttu-id="61fcb-2317">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2317">Err</span></span> | <span data-ttu-id="61fcb-2318">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2318">Err</span></span> | <span data-ttu-id="61fcb-2319">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2319">Do</span></span> | <span data-ttu-id="61fcb-2320">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2320">Ob</span></span> | 
| <span data-ttu-id="61fcb-2321">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2321">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2322">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-2322">UL</span></span> | <span data-ttu-id="61fcb-2323">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-2323">De</span></span> | <span data-ttu-id="61fcb-2324">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-2324">Si</span></span> | <span data-ttu-id="61fcb-2325">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2325">Do</span></span> | <span data-ttu-id="61fcb-2326">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2326">Err</span></span> | <span data-ttu-id="61fcb-2327">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2327">Err</span></span> | <span data-ttu-id="61fcb-2328">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2328">Do</span></span> | <span data-ttu-id="61fcb-2329">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2329">Ob</span></span> | 
| <span data-ttu-id="61fcb-2330">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2330">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2331">De</span><span class="sxs-lookup"><span data-stu-id="61fcb-2331">De</span></span> | <span data-ttu-id="61fcb-2332">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-2332">Si</span></span> | <span data-ttu-id="61fcb-2333">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2333">Do</span></span> | <span data-ttu-id="61fcb-2334">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2334">Err</span></span> | <span data-ttu-id="61fcb-2335">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2335">Err</span></span> | <span data-ttu-id="61fcb-2336">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2336">Do</span></span> | <span data-ttu-id="61fcb-2337">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2337">Ob</span></span> | 
| <span data-ttu-id="61fcb-2338">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2338">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2339">Si</span><span class="sxs-lookup"><span data-stu-id="61fcb-2339">Si</span></span> | <span data-ttu-id="61fcb-2340">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2340">Do</span></span> | <span data-ttu-id="61fcb-2341">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2341">Err</span></span> | <span data-ttu-id="61fcb-2342">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2342">Err</span></span> | <span data-ttu-id="61fcb-2343">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2343">Do</span></span> | <span data-ttu-id="61fcb-2344">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2344">Ob</span></span> | 
| <span data-ttu-id="61fcb-2345">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2345">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2346">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2346">Do</span></span> | <span data-ttu-id="61fcb-2347">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2347">Err</span></span> | <span data-ttu-id="61fcb-2348">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2348">Err</span></span> | <span data-ttu-id="61fcb-2349">Do</span><span class="sxs-lookup"><span data-stu-id="61fcb-2349">Do</span></span> | <span data-ttu-id="61fcb-2350">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2350">Ob</span></span> | 
| <span data-ttu-id="61fcb-2351">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2351">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2352">Da</span><span class="sxs-lookup"><span data-stu-id="61fcb-2352">Da</span></span>  | <span data-ttu-id="61fcb-2353">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2353">Err</span></span> | <span data-ttu-id="61fcb-2354">Da</span><span class="sxs-lookup"><span data-stu-id="61fcb-2354">Da</span></span> | <span data-ttu-id="61fcb-2355">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2355">Ob</span></span> | 
| <span data-ttu-id="61fcb-2356">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2356">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="61fcb-2357">Ch</span><span class="sxs-lookup"><span data-stu-id="61fcb-2357">Ch</span></span>  | <span data-ttu-id="61fcb-2358">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2358">St</span></span> | <span data-ttu-id="61fcb-2359">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2359">Ob</span></span> | 
| <span data-ttu-id="61fcb-2360">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2360">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="61fcb-2361">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2361">St</span></span> | <span data-ttu-id="61fcb-2362">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2362">Ob</span></span> | 
| <span data-ttu-id="61fcb-2363">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2363">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | <span data-ttu-id="61fcb-2364">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2364">Ob</span></span> | 


## <a name="like-operator"></a><span data-ttu-id="61fcb-2365">Like-Operator</span><span class="sxs-lookup"><span data-stu-id="61fcb-2365">Like Operator</span></span>

<span data-ttu-id="61fcb-2366">Die `Like` Operator bestimmt, ob eine Zeichenfolge mit einem angegebenes Muster übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2366">The `Like` operator determines whether a string matches a given pattern.</span></span>

```antlr
LikeOperatorExpression
    : Expression 'Like' LineTerminator? Expression
    ;
```

<span data-ttu-id="61fcb-2367">Die `Like` Operator ist definiert, für die `String` Typ.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2367">The `Like` operator is defined for the `String` type.</span></span> <span data-ttu-id="61fcb-2368">Der erste Operand wird die Zeichenfolge abgeglichen wird, und der zweite Operand ist das Muster für den Abgleich.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2368">The first operand is the string being matched, and the second operand is the pattern to match against.</span></span> <span data-ttu-id="61fcb-2369">Das Muster besteht aus Unicode-Zeichen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2369">The pattern is made up of Unicode characters.</span></span> <span data-ttu-id="61fcb-2370">Die folgende Sequenz von Zeichen haben eine besondere Bedeutung:</span><span class="sxs-lookup"><span data-stu-id="61fcb-2370">The following character sequences have special meanings:</span></span>

* <span data-ttu-id="61fcb-2371">Das Zeichen `?` entspricht einem beliebigen einzelnes Zeichen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2371">The character `?` matches any single character.</span></span>

* <span data-ttu-id="61fcb-2372">Das Zeichen `*` entspricht null oder mehr Zeichen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2372">The character `*` matches zero or more characters.</span></span>

* <span data-ttu-id="61fcb-2373">Das Zeichen `#` entspricht eine beliebige einzelne Ziffer (0-9).</span><span class="sxs-lookup"><span data-stu-id="61fcb-2373">The character `#` matches any single digit (0-9).</span></span>

* <span data-ttu-id="61fcb-2374">Eine Liste von Zeichen, die in Klammern eingeschlossen (`[ab...]`) entspricht einem beliebigen einzelnes Zeichen in der Liste.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2374">A list of characters surrounded by brackets (`[ab...]`) matches any single character in the list.</span></span>

* <span data-ttu-id="61fcb-2375">Eine Liste von Zeichen von Klammern umgeben, und das Präfix durch ein Ausrufezeichen (`[!ab...]`) entspricht einem beliebigen einzelnes Zeichen nicht in der Zeichenliste.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2375">A list of characters surrounded by brackets and prefixed by an exclamation point (`[!ab...]`) matches any single character not in the character list.</span></span>

* <span data-ttu-id="61fcb-2376">Zwei Zeichen in einer Zeichenliste durch einen Bindestrich getrennt (`-`) Geben Sie einen Bereich von Unicode-Zeichen, mit dem ersten Zeichen beginnt und endet mit dem zweiten Zeichen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2376">Two characters in a character list separated by a hyphen (`-`) specify a range of Unicode characters starting with the first character and ending with the second character.</span></span> <span data-ttu-id="61fcb-2377">Wenn das zweite Zeichen nicht höher als das erste Zeichen in der Sortierreihenfolge ist, tritt eine Laufzeitausnahme.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2377">If the second character is not later in the sort order than the first character, a run-time exception occurs.</span></span> <span data-ttu-id="61fcb-2378">Ein Bindestrich, der am Anfang oder Ende einer Zeichenliste wird angezeigt, gibt selbst.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2378">A hyphen that appears at the beginning or end of a character list specifies itself.</span></span>

<span data-ttu-id="61fcb-2379">Entsprechend Sonderzeichen für die öffnende Klammer (`[`), Fragezeichen (`?`), Nummernzeichen (`#`), und das Sternchen (`*`), eckige Klammern eingeschlossen werden müssen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2379">To match the special characters left bracket (`[`), question mark (`?`), number sign (`#`), and asterisk (`*`), brackets must enclose them.</span></span> <span data-ttu-id="61fcb-2380">Die Rechte eckige Klammer (`]`) kann nicht innerhalb einer Gruppe verwendet werden, entsprechend selbst, aber es kann als ein einzelnes Zeichen außerhalb einer Gruppe verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2380">The right bracket (`]`) cannot be used within a group to match itself, but it can be used outside a group as an individual character.</span></span> <span data-ttu-id="61fcb-2381">Die Zeichensequenz `[]` gilt das Zeichenfolgenliteral `""`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2381">The character sequence `[]` is considered to be the string literal `""`.</span></span> 

<span data-ttu-id="61fcb-2382">Beachten Sie, dass Zeichenvergleiche und Sortierung für Zeichenlisten abhängig von dem Typ von Vergleichen verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2382">Note that character comparisons and ordering for character lists are dependent on the type of comparisons being used.</span></span> <span data-ttu-id="61fcb-2383">Wenn die binäre Vergleiche verwendet werden, basieren Zeichenvergleiche und Sortierung auf den numerischen Unicode-Werten.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2383">If binary comparisons are being used, character comparisons and ordering are based on the numeric Unicode values.</span></span> <span data-ttu-id="61fcb-2384">Wenn Textvergleiche verwendet werden, basieren Zeichenvergleiche und Sortierung auf dem aktuellen Gebietsschema, das auf .NET Framework verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2384">If text comparisons are being used, character comparisons and ordering are based on the current locale being used on the .NET Framework.</span></span>

<span data-ttu-id="61fcb-2385">In einigen Sprachen können Sonderzeichen in der Alphabet stellen zwei separate Zeichen und umgekehrt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2385">In some languages, special characters in the alphabet represent two separate characters and vice versa.</span></span> <span data-ttu-id="61fcb-2386">Mehrere Sprachen verwenden Sie z. B. das Zeichen `æ` zur Darstellung der Zeichen `a` und `e` Wenn sie angezeigt werden, während die Zeichen `^` und `O` können verwendet werden, um die Darstellung des Zeichens `Ô`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2386">For example, several languages use the character `æ` to represent the characters `a` and `e` when they appear together, while the characters `^` and `O` can be used to represent the character `Ô`.</span></span> <span data-ttu-id="61fcb-2387">Wenn Textvergleiche, die `Like` Operator erkennt diese kulturellen Äquivalenzen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2387">When using text comparisons, the `Like` operator recognizes such cultural equivalences.</span></span> <span data-ttu-id="61fcb-2388">In diesem Fall entspricht ein Vorkommen der einzelnen speziellen Zeichen in einem Muster oder Zeichenfolge das entsprechende zwei Zeichen bestehende Folge in die andere Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2388">In that case, an occurrence of the single special character in either pattern or string matches the equivalent two-character sequence in the other string.</span></span> <span data-ttu-id="61fcb-2389">Auf ähnliche Weise ein einzelnes spezielle Zeichen im Muster in Klammern eingeschlossen (allein in einer Liste oder in einem Bereich) entspricht der entspricht zwei Zeichen bestehende Folge in der Zeichenfolge und umgekehrt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2389">Similarly, a single special character in pattern enclosed in brackets (by itself, in a list, or in a range) matches the equivalent two-character sequence in the string and vice versa.</span></span>

<span data-ttu-id="61fcb-2390">In einem `Like` Ausdruck, in denen beide Operanden sind `Nothing` oder einer der Operanden eine Konvertierung zu `String` und der andere Operand `Nothing`, `Nothing` wird behandelt, als handele es sich um ein literal für die leere Zeichenfolge `""`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2390">In a `Like` expression where both operands are `Nothing` or one operand has an intrinsic conversion to `String` and the other operand is `Nothing`, `Nothing` is treated as if it were the empty string literal `""`.</span></span>

<span data-ttu-id="61fcb-2391">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2391">__Operation Type:__</span></span>

|        | <span data-ttu-id="61fcb-2392">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2392">__Bo__</span></span> | <span data-ttu-id="61fcb-2393">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2393">__SB__</span></span> | <span data-ttu-id="61fcb-2394">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2394">__By__</span></span> | <span data-ttu-id="61fcb-2395">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2395">__Sh__</span></span> | <span data-ttu-id="61fcb-2396">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2396">__US__</span></span> | <span data-ttu-id="61fcb-2397">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2397">__In__</span></span> | <span data-ttu-id="61fcb-2398">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2398">__UI__</span></span> | <span data-ttu-id="61fcb-2399">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2399">__Lo__</span></span> | <span data-ttu-id="61fcb-2400">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2400">__UL__</span></span> | <span data-ttu-id="61fcb-2401">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2401">__De__</span></span> | <span data-ttu-id="61fcb-2402">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2402">__Si__</span></span> | <span data-ttu-id="61fcb-2403">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2403">__Do__</span></span> | <span data-ttu-id="61fcb-2404">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2404">__Da__</span></span>  | <span data-ttu-id="61fcb-2405">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2405">__Ch__</span></span>  | <span data-ttu-id="61fcb-2406">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2406">__St__</span></span> | <span data-ttu-id="61fcb-2407">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2407">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| <span data-ttu-id="61fcb-2408">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2408">__Bo__</span></span> | <span data-ttu-id="61fcb-2409">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2409">St</span></span> | <span data-ttu-id="61fcb-2410">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2410">St</span></span> | <span data-ttu-id="61fcb-2411">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2411">St</span></span> | <span data-ttu-id="61fcb-2412">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2412">St</span></span> | <span data-ttu-id="61fcb-2413">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2413">St</span></span> | <span data-ttu-id="61fcb-2414">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2414">St</span></span> | <span data-ttu-id="61fcb-2415">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2415">St</span></span> | <span data-ttu-id="61fcb-2416">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2416">St</span></span> | <span data-ttu-id="61fcb-2417">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2417">St</span></span> | <span data-ttu-id="61fcb-2418">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2418">St</span></span> | <span data-ttu-id="61fcb-2419">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2419">St</span></span> | <span data-ttu-id="61fcb-2420">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2420">St</span></span> | <span data-ttu-id="61fcb-2421">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2421">St</span></span> | <span data-ttu-id="61fcb-2422">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2422">St</span></span> | <span data-ttu-id="61fcb-2423">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2423">St</span></span> | <span data-ttu-id="61fcb-2424">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2424">Ob</span></span> | 
| <span data-ttu-id="61fcb-2425">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2425">__SB__</span></span> |    | <span data-ttu-id="61fcb-2426">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2426">St</span></span> | <span data-ttu-id="61fcb-2427">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2427">St</span></span> | <span data-ttu-id="61fcb-2428">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2428">St</span></span> | <span data-ttu-id="61fcb-2429">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2429">St</span></span> | <span data-ttu-id="61fcb-2430">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2430">St</span></span> | <span data-ttu-id="61fcb-2431">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2431">St</span></span> | <span data-ttu-id="61fcb-2432">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2432">St</span></span> | <span data-ttu-id="61fcb-2433">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2433">St</span></span> | <span data-ttu-id="61fcb-2434">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2434">St</span></span> | <span data-ttu-id="61fcb-2435">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2435">St</span></span> | <span data-ttu-id="61fcb-2436">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2436">St</span></span> | <span data-ttu-id="61fcb-2437">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2437">St</span></span> | <span data-ttu-id="61fcb-2438">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2438">St</span></span> | <span data-ttu-id="61fcb-2439">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2439">St</span></span> | <span data-ttu-id="61fcb-2440">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2440">Ob</span></span> | 
| <span data-ttu-id="61fcb-2441">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2441">__By__</span></span> |    |    | <span data-ttu-id="61fcb-2442">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2442">St</span></span> | <span data-ttu-id="61fcb-2443">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2443">St</span></span> | <span data-ttu-id="61fcb-2444">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2444">St</span></span> | <span data-ttu-id="61fcb-2445">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2445">St</span></span> | <span data-ttu-id="61fcb-2446">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2446">St</span></span> | <span data-ttu-id="61fcb-2447">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2447">St</span></span> | <span data-ttu-id="61fcb-2448">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2448">St</span></span> | <span data-ttu-id="61fcb-2449">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2449">St</span></span> | <span data-ttu-id="61fcb-2450">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2450">St</span></span> | <span data-ttu-id="61fcb-2451">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2451">St</span></span> | <span data-ttu-id="61fcb-2452">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2452">St</span></span> | <span data-ttu-id="61fcb-2453">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2453">St</span></span> | <span data-ttu-id="61fcb-2454">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2454">St</span></span> | <span data-ttu-id="61fcb-2455">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2455">Ob</span></span> | 
| <span data-ttu-id="61fcb-2456">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2456">__Sh__</span></span> |    |    |    | <span data-ttu-id="61fcb-2457">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2457">St</span></span> | <span data-ttu-id="61fcb-2458">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2458">St</span></span> | <span data-ttu-id="61fcb-2459">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2459">St</span></span> | <span data-ttu-id="61fcb-2460">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2460">St</span></span> | <span data-ttu-id="61fcb-2461">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2461">St</span></span> | <span data-ttu-id="61fcb-2462">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2462">St</span></span> | <span data-ttu-id="61fcb-2463">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2463">St</span></span> | <span data-ttu-id="61fcb-2464">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2464">St</span></span> | <span data-ttu-id="61fcb-2465">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2465">St</span></span> | <span data-ttu-id="61fcb-2466">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2466">St</span></span> | <span data-ttu-id="61fcb-2467">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2467">St</span></span> | <span data-ttu-id="61fcb-2468">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2468">St</span></span> | <span data-ttu-id="61fcb-2469">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2469">Ob</span></span> | 
| <span data-ttu-id="61fcb-2470">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2470">__US__</span></span> |    |    |    |    | <span data-ttu-id="61fcb-2471">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2471">St</span></span> | <span data-ttu-id="61fcb-2472">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2472">St</span></span> | <span data-ttu-id="61fcb-2473">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2473">St</span></span> | <span data-ttu-id="61fcb-2474">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2474">St</span></span> | <span data-ttu-id="61fcb-2475">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2475">St</span></span> | <span data-ttu-id="61fcb-2476">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2476">St</span></span> | <span data-ttu-id="61fcb-2477">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2477">St</span></span> | <span data-ttu-id="61fcb-2478">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2478">St</span></span> | <span data-ttu-id="61fcb-2479">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2479">St</span></span> | <span data-ttu-id="61fcb-2480">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2480">St</span></span> | <span data-ttu-id="61fcb-2481">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2481">St</span></span> | <span data-ttu-id="61fcb-2482">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2482">Ob</span></span> | 
| <span data-ttu-id="61fcb-2483">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2483">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="61fcb-2484">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2484">St</span></span> | <span data-ttu-id="61fcb-2485">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2485">St</span></span> | <span data-ttu-id="61fcb-2486">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2486">St</span></span> | <span data-ttu-id="61fcb-2487">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2487">St</span></span> | <span data-ttu-id="61fcb-2488">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2488">St</span></span> | <span data-ttu-id="61fcb-2489">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2489">St</span></span> | <span data-ttu-id="61fcb-2490">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2490">St</span></span> | <span data-ttu-id="61fcb-2491">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2491">St</span></span> | <span data-ttu-id="61fcb-2492">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2492">St</span></span> | <span data-ttu-id="61fcb-2493">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2493">St</span></span> | <span data-ttu-id="61fcb-2494">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2494">Ob</span></span> | 
| <span data-ttu-id="61fcb-2495">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2495">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="61fcb-2496">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2496">St</span></span> | <span data-ttu-id="61fcb-2497">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2497">St</span></span> | <span data-ttu-id="61fcb-2498">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2498">St</span></span> | <span data-ttu-id="61fcb-2499">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2499">St</span></span> | <span data-ttu-id="61fcb-2500">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2500">St</span></span> | <span data-ttu-id="61fcb-2501">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2501">St</span></span> | <span data-ttu-id="61fcb-2502">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2502">St</span></span> | <span data-ttu-id="61fcb-2503">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2503">St</span></span> | <span data-ttu-id="61fcb-2504">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2504">St</span></span> | <span data-ttu-id="61fcb-2505">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2505">Ob</span></span> | 
| <span data-ttu-id="61fcb-2506">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2506">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2507">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2507">St</span></span> | <span data-ttu-id="61fcb-2508">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2508">St</span></span> | <span data-ttu-id="61fcb-2509">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2509">St</span></span> | <span data-ttu-id="61fcb-2510">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2510">St</span></span> | <span data-ttu-id="61fcb-2511">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2511">St</span></span> | <span data-ttu-id="61fcb-2512">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2512">St</span></span> | <span data-ttu-id="61fcb-2513">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2513">St</span></span> | <span data-ttu-id="61fcb-2514">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2514">St</span></span> | <span data-ttu-id="61fcb-2515">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2515">Ob</span></span> | 
| <span data-ttu-id="61fcb-2516">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2516">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2517">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2517">St</span></span> | <span data-ttu-id="61fcb-2518">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2518">St</span></span> | <span data-ttu-id="61fcb-2519">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2519">St</span></span> | <span data-ttu-id="61fcb-2520">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2520">St</span></span> | <span data-ttu-id="61fcb-2521">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2521">St</span></span> | <span data-ttu-id="61fcb-2522">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2522">St</span></span> | <span data-ttu-id="61fcb-2523">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2523">St</span></span> | <span data-ttu-id="61fcb-2524">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2524">Ob</span></span> | 
| <span data-ttu-id="61fcb-2525">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2525">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2526">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2526">St</span></span> | <span data-ttu-id="61fcb-2527">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2527">St</span></span> | <span data-ttu-id="61fcb-2528">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2528">St</span></span> | <span data-ttu-id="61fcb-2529">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2529">St</span></span> | <span data-ttu-id="61fcb-2530">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2530">St</span></span> | <span data-ttu-id="61fcb-2531">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2531">St</span></span> | <span data-ttu-id="61fcb-2532">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2532">Ob</span></span> | 
| <span data-ttu-id="61fcb-2533">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2533">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2534">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2534">St</span></span> | <span data-ttu-id="61fcb-2535">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2535">St</span></span> | <span data-ttu-id="61fcb-2536">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2536">St</span></span> | <span data-ttu-id="61fcb-2537">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2537">St</span></span> | <span data-ttu-id="61fcb-2538">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2538">St</span></span> | <span data-ttu-id="61fcb-2539">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2539">Ob</span></span> | 
| <span data-ttu-id="61fcb-2540">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2540">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2541">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2541">St</span></span> | <span data-ttu-id="61fcb-2542">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2542">St</span></span> | <span data-ttu-id="61fcb-2543">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2543">St</span></span> | <span data-ttu-id="61fcb-2544">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2544">St</span></span> | <span data-ttu-id="61fcb-2545">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2545">Ob</span></span> | 
| <span data-ttu-id="61fcb-2546">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2546">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2547">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2547">St</span></span> | <span data-ttu-id="61fcb-2548">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2548">St</span></span> | <span data-ttu-id="61fcb-2549">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2549">St</span></span> | <span data-ttu-id="61fcb-2550">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2550">Ob</span></span> | 
| <span data-ttu-id="61fcb-2551">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2551">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2552">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2552">St</span></span> | <span data-ttu-id="61fcb-2553">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2553">St</span></span> | <span data-ttu-id="61fcb-2554">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2554">Ob</span></span> | 
| <span data-ttu-id="61fcb-2555">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2555">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2556">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2556">St</span></span> | <span data-ttu-id="61fcb-2557">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2557">Ob</span></span> | 
| <span data-ttu-id="61fcb-2558">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2558">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2559">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2559">Ob</span></span> | 


## <a name="concatenation-operator"></a><span data-ttu-id="61fcb-2560">Operator für zeichenfolgenverkettung</span><span class="sxs-lookup"><span data-stu-id="61fcb-2560">Concatenation Operator</span></span>

```antlr
ConcatenationOperatorExpression
    : Expression '&' LineTerminator? Expression
    ;
```

<span data-ttu-id="61fcb-2561">Die *Verkettungsoperator* für alle systeminternen Typen, einschließlich der auf NULL festlegbare Versionen der systeminterne Werttypen definiert ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2561">The *concatenation operator* is defined for all of the intrinsic types, including the nullable versions of the intrinsic value types.</span></span> <span data-ttu-id="61fcb-2562">Es wird auch für die Verkettung zwischen den oben genannten Typen definiert und `System.DBNull`, die so behandelt, als eine `Nothing` Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2562">It is also defined for concatenation between the types mentioned above and `System.DBNull`, which is treated as a `Nothing` string.</span></span> <span data-ttu-id="61fcb-2563">Der Operator für Verkettungen konvertiert alle Operanden um `String`; im Ausdruck alle Konvertierungen in `String` erweiternde werden, unabhängig davon, ob die strikte Semantik verwendet werden, gelten.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2563">The concatenation operator converts all of its operands to `String`; in the expression, all conversions to `String` are considered to be widening, regardless of whether strict semantics are used.</span></span> <span data-ttu-id="61fcb-2564">Ein `System.DBNull` konvertierte Wert wird das Literal `Nothing` als `String`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2564">A `System.DBNull` value is converted to the literal `Nothing` typed as `String`.</span></span> <span data-ttu-id="61fcb-2565">Ein NULL-Wert, dessen Wert `Nothing` ist auch in das Literal konvertiert `Nothing` als `String`, anstatt einen Laufzeitfehler auszulösen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2565">A nullable value type whose value is `Nothing` is also converted to the literal `Nothing` typed as `String`, rather than throwing a run-time error.</span></span>

<span data-ttu-id="61fcb-2566">Eine Verkettungsoperation führt zu einer Zeichenfolge, die die Verkettung der beiden Operanden in der Reihenfolge von links nach rechts ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2566">A concatenation operation results in a string that is the concatenation of the two operands in order from left to right.</span></span> <span data-ttu-id="61fcb-2567">Der Wert `Nothing` wird behandelt, als handele es sich um ein literal für die leere Zeichenfolge `""`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2567">The value `Nothing` is treated as if it were the empty string literal `""`.</span></span>

<span data-ttu-id="61fcb-2568">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2568">__Operation Type:__</span></span>

|        | <span data-ttu-id="61fcb-2569">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2569">__Bo__</span></span> | <span data-ttu-id="61fcb-2570">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2570">__SB__</span></span> | <span data-ttu-id="61fcb-2571">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2571">__By__</span></span> | <span data-ttu-id="61fcb-2572">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2572">__Sh__</span></span> | <span data-ttu-id="61fcb-2573">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2573">__US__</span></span> | <span data-ttu-id="61fcb-2574">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2574">__In__</span></span> | <span data-ttu-id="61fcb-2575">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2575">__UI__</span></span> | <span data-ttu-id="61fcb-2576">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2576">__Lo__</span></span> | <span data-ttu-id="61fcb-2577">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2577">__UL__</span></span> | <span data-ttu-id="61fcb-2578">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2578">__De__</span></span> | <span data-ttu-id="61fcb-2579">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2579">__Si__</span></span> | <span data-ttu-id="61fcb-2580">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2580">__Do__</span></span> | <span data-ttu-id="61fcb-2581">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2581">__Da__</span></span>  | <span data-ttu-id="61fcb-2582">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2582">__Ch__</span></span>  | <span data-ttu-id="61fcb-2583">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2583">__St__</span></span> | <span data-ttu-id="61fcb-2584">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2584">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| <span data-ttu-id="61fcb-2585">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2585">__Bo__</span></span> | <span data-ttu-id="61fcb-2586">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2586">St</span></span> | <span data-ttu-id="61fcb-2587">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2587">St</span></span> | <span data-ttu-id="61fcb-2588">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2588">St</span></span> | <span data-ttu-id="61fcb-2589">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2589">St</span></span> | <span data-ttu-id="61fcb-2590">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2590">St</span></span> | <span data-ttu-id="61fcb-2591">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2591">St</span></span> | <span data-ttu-id="61fcb-2592">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2592">St</span></span> | <span data-ttu-id="61fcb-2593">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2593">St</span></span> | <span data-ttu-id="61fcb-2594">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2594">St</span></span> | <span data-ttu-id="61fcb-2595">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2595">St</span></span> | <span data-ttu-id="61fcb-2596">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2596">St</span></span> | <span data-ttu-id="61fcb-2597">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2597">St</span></span> | <span data-ttu-id="61fcb-2598">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2598">St</span></span> | <span data-ttu-id="61fcb-2599">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2599">St</span></span> | <span data-ttu-id="61fcb-2600">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2600">St</span></span> | <span data-ttu-id="61fcb-2601">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2601">Ob</span></span> | 
| <span data-ttu-id="61fcb-2602">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2602">__SB__</span></span> |    | <span data-ttu-id="61fcb-2603">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2603">St</span></span> | <span data-ttu-id="61fcb-2604">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2604">St</span></span> | <span data-ttu-id="61fcb-2605">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2605">St</span></span> | <span data-ttu-id="61fcb-2606">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2606">St</span></span> | <span data-ttu-id="61fcb-2607">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2607">St</span></span> | <span data-ttu-id="61fcb-2608">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2608">St</span></span> | <span data-ttu-id="61fcb-2609">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2609">St</span></span> | <span data-ttu-id="61fcb-2610">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2610">St</span></span> | <span data-ttu-id="61fcb-2611">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2611">St</span></span> | <span data-ttu-id="61fcb-2612">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2612">St</span></span> | <span data-ttu-id="61fcb-2613">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2613">St</span></span> | <span data-ttu-id="61fcb-2614">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2614">St</span></span> | <span data-ttu-id="61fcb-2615">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2615">St</span></span> | <span data-ttu-id="61fcb-2616">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2616">St</span></span> | <span data-ttu-id="61fcb-2617">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2617">Ob</span></span> | 
| <span data-ttu-id="61fcb-2618">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2618">__By__</span></span> |    |    | <span data-ttu-id="61fcb-2619">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2619">St</span></span> | <span data-ttu-id="61fcb-2620">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2620">St</span></span> | <span data-ttu-id="61fcb-2621">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2621">St</span></span> | <span data-ttu-id="61fcb-2622">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2622">St</span></span> | <span data-ttu-id="61fcb-2623">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2623">St</span></span> | <span data-ttu-id="61fcb-2624">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2624">St</span></span> | <span data-ttu-id="61fcb-2625">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2625">St</span></span> | <span data-ttu-id="61fcb-2626">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2626">St</span></span> | <span data-ttu-id="61fcb-2627">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2627">St</span></span> | <span data-ttu-id="61fcb-2628">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2628">St</span></span> | <span data-ttu-id="61fcb-2629">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2629">St</span></span> | <span data-ttu-id="61fcb-2630">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2630">St</span></span> | <span data-ttu-id="61fcb-2631">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2631">St</span></span> | <span data-ttu-id="61fcb-2632">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2632">Ob</span></span> | 
| <span data-ttu-id="61fcb-2633">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2633">__Sh__</span></span> |    |    |    | <span data-ttu-id="61fcb-2634">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2634">St</span></span> | <span data-ttu-id="61fcb-2635">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2635">St</span></span> | <span data-ttu-id="61fcb-2636">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2636">St</span></span> | <span data-ttu-id="61fcb-2637">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2637">St</span></span> | <span data-ttu-id="61fcb-2638">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2638">St</span></span> | <span data-ttu-id="61fcb-2639">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2639">St</span></span> | <span data-ttu-id="61fcb-2640">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2640">St</span></span> | <span data-ttu-id="61fcb-2641">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2641">St</span></span> | <span data-ttu-id="61fcb-2642">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2642">St</span></span> | <span data-ttu-id="61fcb-2643">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2643">St</span></span> | <span data-ttu-id="61fcb-2644">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2644">St</span></span> | <span data-ttu-id="61fcb-2645">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2645">St</span></span> | <span data-ttu-id="61fcb-2646">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2646">Ob</span></span> | 
| <span data-ttu-id="61fcb-2647">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2647">__US__</span></span> |    |    |    |    | <span data-ttu-id="61fcb-2648">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2648">St</span></span> | <span data-ttu-id="61fcb-2649">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2649">St</span></span> | <span data-ttu-id="61fcb-2650">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2650">St</span></span> | <span data-ttu-id="61fcb-2651">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2651">St</span></span> | <span data-ttu-id="61fcb-2652">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2652">St</span></span> | <span data-ttu-id="61fcb-2653">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2653">St</span></span> | <span data-ttu-id="61fcb-2654">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2654">St</span></span> | <span data-ttu-id="61fcb-2655">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2655">St</span></span> | <span data-ttu-id="61fcb-2656">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2656">St</span></span> | <span data-ttu-id="61fcb-2657">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2657">St</span></span> | <span data-ttu-id="61fcb-2658">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2658">St</span></span> | <span data-ttu-id="61fcb-2659">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2659">Ob</span></span> | 
| <span data-ttu-id="61fcb-2660">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2660">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="61fcb-2661">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2661">St</span></span> | <span data-ttu-id="61fcb-2662">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2662">St</span></span> | <span data-ttu-id="61fcb-2663">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2663">St</span></span> | <span data-ttu-id="61fcb-2664">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2664">St</span></span> | <span data-ttu-id="61fcb-2665">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2665">St</span></span> | <span data-ttu-id="61fcb-2666">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2666">St</span></span> | <span data-ttu-id="61fcb-2667">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2667">St</span></span> | <span data-ttu-id="61fcb-2668">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2668">St</span></span> | <span data-ttu-id="61fcb-2669">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2669">St</span></span> | <span data-ttu-id="61fcb-2670">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2670">St</span></span> | <span data-ttu-id="61fcb-2671">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2671">Ob</span></span> | 
| <span data-ttu-id="61fcb-2672">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2672">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="61fcb-2673">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2673">St</span></span> | <span data-ttu-id="61fcb-2674">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2674">St</span></span> | <span data-ttu-id="61fcb-2675">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2675">St</span></span> | <span data-ttu-id="61fcb-2676">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2676">St</span></span> | <span data-ttu-id="61fcb-2677">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2677">St</span></span> | <span data-ttu-id="61fcb-2678">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2678">St</span></span> | <span data-ttu-id="61fcb-2679">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2679">St</span></span> | <span data-ttu-id="61fcb-2680">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2680">St</span></span> | <span data-ttu-id="61fcb-2681">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2681">St</span></span> | <span data-ttu-id="61fcb-2682">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2682">Ob</span></span> | 
| <span data-ttu-id="61fcb-2683">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2683">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2684">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2684">St</span></span> | <span data-ttu-id="61fcb-2685">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2685">St</span></span> | <span data-ttu-id="61fcb-2686">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2686">St</span></span> | <span data-ttu-id="61fcb-2687">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2687">St</span></span> | <span data-ttu-id="61fcb-2688">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2688">St</span></span> | <span data-ttu-id="61fcb-2689">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2689">St</span></span> | <span data-ttu-id="61fcb-2690">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2690">St</span></span> | <span data-ttu-id="61fcb-2691">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2691">St</span></span> | <span data-ttu-id="61fcb-2692">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2692">Ob</span></span> | 
| <span data-ttu-id="61fcb-2693">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2693">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2694">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2694">St</span></span> | <span data-ttu-id="61fcb-2695">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2695">St</span></span> | <span data-ttu-id="61fcb-2696">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2696">St</span></span> | <span data-ttu-id="61fcb-2697">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2697">St</span></span> | <span data-ttu-id="61fcb-2698">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2698">St</span></span> | <span data-ttu-id="61fcb-2699">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2699">St</span></span> | <span data-ttu-id="61fcb-2700">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2700">St</span></span> | <span data-ttu-id="61fcb-2701">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2701">Ob</span></span> | 
| <span data-ttu-id="61fcb-2702">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2702">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2703">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2703">St</span></span> | <span data-ttu-id="61fcb-2704">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2704">St</span></span> | <span data-ttu-id="61fcb-2705">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2705">St</span></span> | <span data-ttu-id="61fcb-2706">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2706">St</span></span> | <span data-ttu-id="61fcb-2707">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2707">St</span></span> | <span data-ttu-id="61fcb-2708">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2708">St</span></span> | <span data-ttu-id="61fcb-2709">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2709">Ob</span></span> | 
| <span data-ttu-id="61fcb-2710">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2710">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2711">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2711">St</span></span> | <span data-ttu-id="61fcb-2712">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2712">St</span></span> | <span data-ttu-id="61fcb-2713">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2713">St</span></span> | <span data-ttu-id="61fcb-2714">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2714">St</span></span> | <span data-ttu-id="61fcb-2715">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2715">St</span></span> | <span data-ttu-id="61fcb-2716">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2716">Ob</span></span> | 
| <span data-ttu-id="61fcb-2717">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2717">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2718">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2718">St</span></span> | <span data-ttu-id="61fcb-2719">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2719">St</span></span> | <span data-ttu-id="61fcb-2720">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2720">St</span></span> | <span data-ttu-id="61fcb-2721">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2721">St</span></span> | <span data-ttu-id="61fcb-2722">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2722">Ob</span></span> | 
| <span data-ttu-id="61fcb-2723">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2723">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2724">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2724">St</span></span> | <span data-ttu-id="61fcb-2725">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2725">St</span></span> | <span data-ttu-id="61fcb-2726">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2726">St</span></span> | <span data-ttu-id="61fcb-2727">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2727">Ob</span></span> | 
| <span data-ttu-id="61fcb-2728">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2728">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2729">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2729">St</span></span> | <span data-ttu-id="61fcb-2730">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2730">St</span></span> | <span data-ttu-id="61fcb-2731">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2731">Ob</span></span> | 
| <span data-ttu-id="61fcb-2732">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2732">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2733">St</span><span class="sxs-lookup"><span data-stu-id="61fcb-2733">St</span></span> | <span data-ttu-id="61fcb-2734">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2734">Ob</span></span> | 
| <span data-ttu-id="61fcb-2735">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2735">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2736">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2736">Ob</span></span> | 


## <a name="logical-operators"></a><span data-ttu-id="61fcb-2737">Logische Operatoren</span><span class="sxs-lookup"><span data-stu-id="61fcb-2737">Logical Operators</span></span>

<span data-ttu-id="61fcb-2738">Die `And`, `Not`, `Or`, und `Xor` Operatoren werden als die logischen Operatoren bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2738">The `And`, `Not`, `Or`, and `Xor` operators are called the logical operators.</span></span>

```antlr
LogicalOperatorExpression
    : 'Not' Expression
    | Expression 'And' LineTerminator? Expression
    | Expression 'Or' LineTerminator? Expression
    | Expression 'Xor' LineTerminator? Expression
    ;
```

<span data-ttu-id="61fcb-2739">Die logischen Operatoren sind wie folgt ausgewertet:</span><span class="sxs-lookup"><span data-stu-id="61fcb-2739">The logical operators are evaluated as follows:</span></span>

* <span data-ttu-id="61fcb-2740">Für die `Boolean` Typ:</span><span class="sxs-lookup"><span data-stu-id="61fcb-2740">For the `Boolean` type:</span></span>

  * <span data-ttu-id="61fcb-2741">Eine logische `And` Vorgang wird für seine beiden Operanden ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2741">A logical `And` operation is performed on its two operands.</span></span>

  * <span data-ttu-id="61fcb-2742">Eine logische `Not` Vorgang wird für Operanden ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2742">A logical `Not` operation is performed on its operand.</span></span>

  * <span data-ttu-id="61fcb-2743">Eine logische `Or` Vorgang wird für seine beiden Operanden ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2743">A logical `Or` operation is performed on its two operands.</span></span>

  * <span data-ttu-id="61fcb-2744">Einen logischen exklusiv -`Or` Vorgang wird für seine beiden Operanden ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2744">A logical exclusive-`Or` operation is performed on its two operands.</span></span>

* <span data-ttu-id="61fcb-2745">Für `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, und alle Enumerationstypen, der angegebene Vorgang wird ausgeführt, für jedes Bit für die binäre Darstellung von der zwei Operand(s):</span><span class="sxs-lookup"><span data-stu-id="61fcb-2745">For `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, and all enumerated types, the specified operation is performed on each bit of the binary representation of the two operand(s):</span></span>

  * <span data-ttu-id="61fcb-2746">`And`: Das Ergebnisbit ist 1, wenn beide Bits 1 sind; Andernfalls ist das Ergebnisbit 0 auf.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2746">`And`: The result bit is 1 if both bits are 1; otherwise the result bit is 0.</span></span>

  * <span data-ttu-id="61fcb-2747">`Not`: Das Ergebnisbit ist 1, wenn das Bit 0 ist; Andernfalls ist das Ergebnis 1.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2747">`Not`: The result bit is 1 if the bit is 0; otherwise the result bit is 1.</span></span>

  * <span data-ttu-id="61fcb-2748">`Or`: Das Ergebnisbit ist 1, wenn jedes Bit 1 ist; Andernfalls ist das Ergebnisbit 0 auf.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2748">`Or`: The result bit is 1 if either bit is 1; otherwise the result bit is 0.</span></span>

  * <span data-ttu-id="61fcb-2749">`Xor`: Das Ergebnisbit ist 1, wenn jedes Bit 1, aber nicht beide Bits ist; Andernfalls ist das Ergebnisbit 0 (d. h. 1 `Xor` 0 = 1, 1 `Xor` 1 = 0).</span><span class="sxs-lookup"><span data-stu-id="61fcb-2749">`Xor`: The result bit is 1 if either bit is 1 but not both bits; otherwise the result bit is 0 (that is, 1 `Xor` 0 = 1, 1 `Xor` 1 = 0).</span></span>

* <span data-ttu-id="61fcb-2750">Wenn die logischen Operatoren `And` und `Or` aufgehoben werden, für den Typ `Boolean?`, sie wurden erweitert, um boolesche Logik mit drei Werten als solche umfassen:</span><span class="sxs-lookup"><span data-stu-id="61fcb-2750">When the logical operators `And` and `Or` are lifted for the type `Boolean?`, they are extended to encompass three-valued Boolean logic as such:</span></span>

  * <span data-ttu-id="61fcb-2751">`And` ergibt "true", wenn beide Operanden wahr sind; "false", wenn einer der Operanden falsch ist; `Nothing` andernfalls.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2751">`And` evaluates to true if both operands are true; false if one of the operands is false; `Nothing` otherwise.</span></span>

  * <span data-ttu-id="61fcb-2752">`Or` ergibt "true", wenn ein Operand true ist; "false" werden beide Operanden sind falsch; `Nothing` andernfalls.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2752">`Or` evaluates to true if either operand is true; false is both operands are false; `Nothing` otherwise.</span></span>

<span data-ttu-id="61fcb-2753">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-2753">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim x?, y? As Boolean

        x = Nothing
        y = True 

        If x Or y Then
            ' Will execute
        End If
    End Sub
End Module
```

<span data-ttu-id="61fcb-2754">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2754">__Note.__</span></span> <span data-ttu-id="61fcb-2755">Im Idealfall die logischen Operatoren `And` und `Or` würde aufgehoben werden, mithilfe von dreiwertige Logik für jeden Typ, der in einen booleschen Ausdruck verwendet werden kann (d. h. einen Typ, der implementiert `IsTrue` und `IsFalse`), in der gleichen Weise wie `AndAlso` und `OrElse` Kurzschluss für jeden Typ, der in einen booleschen Ausdruck verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2755">Ideally, the logical operators `And` and `Or` would be lifted using three-valued logic for any type that can be used in a Boolean expression (i.e. a type that implements `IsTrue` and `IsFalse`), in the same way that `AndAlso` and `OrElse` short circuit across any type that can be used in a Boolean expression.</span></span> <span data-ttu-id="61fcb-2756">Leider dreiwertige anheben wird nur angewendet, um `Boolean?`, sodass eine benutzerdefinierte Typen, die gewünschte dreiwertige Logik manuell, durch die Definition ausführen müssen `And` und `Or` Operatoren für ihre Version ist NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2756">Unfortunately, three-valued lifting is only applied to `Boolean?`, so user-defined types that desire three-valued logic must do so manually by defining `And` and `Or` operators for their nullable version.</span></span>

<span data-ttu-id="61fcb-2757">Keine Überläufe sind von diesen Vorgängen möglich.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2757">No overflows are possible from these operations.</span></span> <span data-ttu-id="61fcb-2758">Die enumerierten Typoperatoren führen bitweise Operation mit den zugrunde liegenden Typ des enumerierten Typs, aber der Rückgabewert ist die enumerierten Typ.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2758">The enumerated type operators do the bitwise operation on the underlying type of the enumerated type, but the return value is the enumerated type.</span></span>

<span data-ttu-id="61fcb-2759">__Nicht Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2759">__Not Operation Type:__</span></span>

| <span data-ttu-id="61fcb-2760">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2760">__Bo__</span></span> | <span data-ttu-id="61fcb-2761">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2761">__SB__</span></span> | <span data-ttu-id="61fcb-2762">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2762">__By__</span></span> | <span data-ttu-id="61fcb-2763">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2763">__Sh__</span></span> | <span data-ttu-id="61fcb-2764">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2764">__US__</span></span> | <span data-ttu-id="61fcb-2765">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2765">__In__</span></span> | <span data-ttu-id="61fcb-2766">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2766">__UI__</span></span> | <span data-ttu-id="61fcb-2767">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2767">__Lo__</span></span> | <span data-ttu-id="61fcb-2768">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2768">__UL__</span></span> | <span data-ttu-id="61fcb-2769">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2769">__De__</span></span> | <span data-ttu-id="61fcb-2770">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2770">__Si__</span></span> | <span data-ttu-id="61fcb-2771">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2771">__Do__</span></span> | <span data-ttu-id="61fcb-2772">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2772">__Da__</span></span>  | <span data-ttu-id="61fcb-2773">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2773">__Ch__</span></span>  | <span data-ttu-id="61fcb-2774">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2774">__St__</span></span> | <span data-ttu-id="61fcb-2775">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2775">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="61fcb-2776">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-2776">Bo</span></span> | <span data-ttu-id="61fcb-2777">SB</span><span class="sxs-lookup"><span data-stu-id="61fcb-2777">SB</span></span> | <span data-ttu-id="61fcb-2778">um</span><span class="sxs-lookup"><span data-stu-id="61fcb-2778">By</span></span> | <span data-ttu-id="61fcb-2779">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-2779">Sh</span></span> | <span data-ttu-id="61fcb-2780">US</span><span class="sxs-lookup"><span data-stu-id="61fcb-2780">US</span></span> | <span data-ttu-id="61fcb-2781">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-2781">In</span></span> | <span data-ttu-id="61fcb-2782">UI</span><span class="sxs-lookup"><span data-stu-id="61fcb-2782">UI</span></span> | <span data-ttu-id="61fcb-2783">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2783">Lo</span></span> | <span data-ttu-id="61fcb-2784">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-2784">UL</span></span> | <span data-ttu-id="61fcb-2785">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2785">Lo</span></span> | <span data-ttu-id="61fcb-2786">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2786">Lo</span></span> | <span data-ttu-id="61fcb-2787">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2787">Lo</span></span> | <span data-ttu-id="61fcb-2788">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2788">Err</span></span> | <span data-ttu-id="61fcb-2789">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2789">Err</span></span> | <span data-ttu-id="61fcb-2790">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2790">Lo</span></span> | <span data-ttu-id="61fcb-2791">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2791">Ob</span></span> | 

<span data-ttu-id="61fcb-2792">__Und, oder geben der Xor-Vorgang:__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2792">__And, Or, Xor Operation Type:__</span></span>

|        | <span data-ttu-id="61fcb-2793">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2793">__Bo__</span></span> | <span data-ttu-id="61fcb-2794">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2794">__SB__</span></span> | <span data-ttu-id="61fcb-2795">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2795">__By__</span></span> | <span data-ttu-id="61fcb-2796">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2796">__Sh__</span></span> | <span data-ttu-id="61fcb-2797">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2797">__US__</span></span> | <span data-ttu-id="61fcb-2798">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2798">__In__</span></span> | <span data-ttu-id="61fcb-2799">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2799">__UI__</span></span> | <span data-ttu-id="61fcb-2800">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2800">__Lo__</span></span> | <span data-ttu-id="61fcb-2801">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2801">__UL__</span></span> | <span data-ttu-id="61fcb-2802">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2802">__De__</span></span> | <span data-ttu-id="61fcb-2803">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2803">__Si__</span></span> | <span data-ttu-id="61fcb-2804">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2804">__Do__</span></span> | <span data-ttu-id="61fcb-2805">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2805">__Da__</span></span>  | <span data-ttu-id="61fcb-2806">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2806">__Ch__</span></span>  | <span data-ttu-id="61fcb-2807">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2807">__St__</span></span> | <span data-ttu-id="61fcb-2808">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2808">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="61fcb-2809">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2809">__Bo__</span></span> | <span data-ttu-id="61fcb-2810">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-2810">Bo</span></span> | <span data-ttu-id="61fcb-2811">SB</span><span class="sxs-lookup"><span data-stu-id="61fcb-2811">SB</span></span> | <span data-ttu-id="61fcb-2812">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-2812">Sh</span></span> | <span data-ttu-id="61fcb-2813">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-2813">Sh</span></span> | <span data-ttu-id="61fcb-2814">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-2814">In</span></span> | <span data-ttu-id="61fcb-2815">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-2815">In</span></span> | <span data-ttu-id="61fcb-2816">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2816">Lo</span></span> | <span data-ttu-id="61fcb-2817">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2817">Lo</span></span> | <span data-ttu-id="61fcb-2818">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2818">Lo</span></span> | <span data-ttu-id="61fcb-2819">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2819">Lo</span></span> | <span data-ttu-id="61fcb-2820">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2820">Lo</span></span> | <span data-ttu-id="61fcb-2821">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2821">Lo</span></span> | <span data-ttu-id="61fcb-2822">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2822">Err</span></span> | <span data-ttu-id="61fcb-2823">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2823">Err</span></span> | <span data-ttu-id="61fcb-2824">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-2824">Bo</span></span>  | <span data-ttu-id="61fcb-2825">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2825">Ob</span></span>  | 
| <span data-ttu-id="61fcb-2826">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2826">__SB__</span></span> |    | <span data-ttu-id="61fcb-2827">SB</span><span class="sxs-lookup"><span data-stu-id="61fcb-2827">SB</span></span> | <span data-ttu-id="61fcb-2828">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-2828">Sh</span></span> | <span data-ttu-id="61fcb-2829">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-2829">Sh</span></span> | <span data-ttu-id="61fcb-2830">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-2830">In</span></span> | <span data-ttu-id="61fcb-2831">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-2831">In</span></span> | <span data-ttu-id="61fcb-2832">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2832">Lo</span></span> | <span data-ttu-id="61fcb-2833">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2833">Lo</span></span> | <span data-ttu-id="61fcb-2834">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2834">Lo</span></span> | <span data-ttu-id="61fcb-2835">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2835">Lo</span></span> | <span data-ttu-id="61fcb-2836">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2836">Lo</span></span> | <span data-ttu-id="61fcb-2837">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2837">Lo</span></span> | <span data-ttu-id="61fcb-2838">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2838">Err</span></span> | <span data-ttu-id="61fcb-2839">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2839">Err</span></span> | <span data-ttu-id="61fcb-2840">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2840">Lo</span></span>  | <span data-ttu-id="61fcb-2841">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2841">Ob</span></span>  | 
| <span data-ttu-id="61fcb-2842">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2842">__By__</span></span> |    |    | <span data-ttu-id="61fcb-2843">um</span><span class="sxs-lookup"><span data-stu-id="61fcb-2843">By</span></span> | <span data-ttu-id="61fcb-2844">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-2844">Sh</span></span> | <span data-ttu-id="61fcb-2845">US</span><span class="sxs-lookup"><span data-stu-id="61fcb-2845">US</span></span> | <span data-ttu-id="61fcb-2846">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-2846">In</span></span> | <span data-ttu-id="61fcb-2847">UI</span><span class="sxs-lookup"><span data-stu-id="61fcb-2847">UI</span></span> | <span data-ttu-id="61fcb-2848">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2848">Lo</span></span> | <span data-ttu-id="61fcb-2849">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-2849">UL</span></span> | <span data-ttu-id="61fcb-2850">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2850">Lo</span></span> | <span data-ttu-id="61fcb-2851">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2851">Lo</span></span> | <span data-ttu-id="61fcb-2852">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2852">Lo</span></span> | <span data-ttu-id="61fcb-2853">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2853">Err</span></span> | <span data-ttu-id="61fcb-2854">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2854">Err</span></span> | <span data-ttu-id="61fcb-2855">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2855">Lo</span></span>  | <span data-ttu-id="61fcb-2856">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2856">Ob</span></span>  | 
| <span data-ttu-id="61fcb-2857">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2857">__Sh__</span></span> |    |    |    | <span data-ttu-id="61fcb-2858">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-2858">Sh</span></span> | <span data-ttu-id="61fcb-2859">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-2859">In</span></span> | <span data-ttu-id="61fcb-2860">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-2860">In</span></span> | <span data-ttu-id="61fcb-2861">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2861">Lo</span></span> | <span data-ttu-id="61fcb-2862">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2862">Lo</span></span> | <span data-ttu-id="61fcb-2863">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2863">Lo</span></span> | <span data-ttu-id="61fcb-2864">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2864">Lo</span></span> | <span data-ttu-id="61fcb-2865">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2865">Lo</span></span> | <span data-ttu-id="61fcb-2866">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2866">Lo</span></span> | <span data-ttu-id="61fcb-2867">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2867">Err</span></span> | <span data-ttu-id="61fcb-2868">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2868">Err</span></span> | <span data-ttu-id="61fcb-2869">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2869">Lo</span></span>  | <span data-ttu-id="61fcb-2870">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2870">Ob</span></span>  | 
| <span data-ttu-id="61fcb-2871">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2871">__US__</span></span> |    |    |    |    | <span data-ttu-id="61fcb-2872">US</span><span class="sxs-lookup"><span data-stu-id="61fcb-2872">US</span></span> | <span data-ttu-id="61fcb-2873">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-2873">In</span></span> | <span data-ttu-id="61fcb-2874">UI</span><span class="sxs-lookup"><span data-stu-id="61fcb-2874">UI</span></span> | <span data-ttu-id="61fcb-2875">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2875">Lo</span></span> | <span data-ttu-id="61fcb-2876">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-2876">UL</span></span> | <span data-ttu-id="61fcb-2877">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2877">Lo</span></span> | <span data-ttu-id="61fcb-2878">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2878">Lo</span></span> | <span data-ttu-id="61fcb-2879">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2879">Lo</span></span> | <span data-ttu-id="61fcb-2880">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2880">Err</span></span> | <span data-ttu-id="61fcb-2881">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2881">Err</span></span> | <span data-ttu-id="61fcb-2882">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2882">Lo</span></span>  | <span data-ttu-id="61fcb-2883">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2883">Ob</span></span>  | 
| <span data-ttu-id="61fcb-2884">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2884">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="61fcb-2885">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-2885">In</span></span> | <span data-ttu-id="61fcb-2886">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2886">Lo</span></span> | <span data-ttu-id="61fcb-2887">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2887">Lo</span></span> | <span data-ttu-id="61fcb-2888">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2888">Lo</span></span> | <span data-ttu-id="61fcb-2889">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2889">Lo</span></span> | <span data-ttu-id="61fcb-2890">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2890">Lo</span></span> | <span data-ttu-id="61fcb-2891">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2891">Lo</span></span> | <span data-ttu-id="61fcb-2892">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2892">Err</span></span> | <span data-ttu-id="61fcb-2893">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2893">Err</span></span> | <span data-ttu-id="61fcb-2894">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2894">Lo</span></span>  | <span data-ttu-id="61fcb-2895">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2895">Ob</span></span>  | 
| <span data-ttu-id="61fcb-2896">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2896">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="61fcb-2897">UI</span><span class="sxs-lookup"><span data-stu-id="61fcb-2897">UI</span></span> | <span data-ttu-id="61fcb-2898">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2898">Lo</span></span> | <span data-ttu-id="61fcb-2899">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-2899">UL</span></span> | <span data-ttu-id="61fcb-2900">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2900">Lo</span></span> | <span data-ttu-id="61fcb-2901">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2901">Lo</span></span> | <span data-ttu-id="61fcb-2902">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2902">Lo</span></span> | <span data-ttu-id="61fcb-2903">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2903">Err</span></span> | <span data-ttu-id="61fcb-2904">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2904">Err</span></span> | <span data-ttu-id="61fcb-2905">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2905">Lo</span></span>  | <span data-ttu-id="61fcb-2906">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2906">Ob</span></span>  | 
| <span data-ttu-id="61fcb-2907">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2907">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2908">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2908">Lo</span></span> | <span data-ttu-id="61fcb-2909">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2909">Lo</span></span> | <span data-ttu-id="61fcb-2910">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2910">Lo</span></span> | <span data-ttu-id="61fcb-2911">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2911">Lo</span></span> | <span data-ttu-id="61fcb-2912">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2912">Lo</span></span> | <span data-ttu-id="61fcb-2913">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2913">Err</span></span> | <span data-ttu-id="61fcb-2914">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2914">Err</span></span> | <span data-ttu-id="61fcb-2915">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2915">Lo</span></span>  | <span data-ttu-id="61fcb-2916">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2916">Ob</span></span>  | 
| <span data-ttu-id="61fcb-2917">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2917">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2918">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-2918">UL</span></span> | <span data-ttu-id="61fcb-2919">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2919">Lo</span></span> | <span data-ttu-id="61fcb-2920">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2920">Lo</span></span> | <span data-ttu-id="61fcb-2921">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2921">Lo</span></span> | <span data-ttu-id="61fcb-2922">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2922">Err</span></span> | <span data-ttu-id="61fcb-2923">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2923">Err</span></span> | <span data-ttu-id="61fcb-2924">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2924">Lo</span></span>  | <span data-ttu-id="61fcb-2925">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2925">Ob</span></span>  | 
| <span data-ttu-id="61fcb-2926">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2926">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2927">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2927">Lo</span></span> | <span data-ttu-id="61fcb-2928">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2928">Lo</span></span> | <span data-ttu-id="61fcb-2929">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2929">Lo</span></span> | <span data-ttu-id="61fcb-2930">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2930">Err</span></span> | <span data-ttu-id="61fcb-2931">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2931">Err</span></span> | <span data-ttu-id="61fcb-2932">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2932">Lo</span></span>  | <span data-ttu-id="61fcb-2933">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2933">Ob</span></span>  | 
| <span data-ttu-id="61fcb-2934">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2934">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2935">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2935">Lo</span></span> | <span data-ttu-id="61fcb-2936">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2936">Lo</span></span> | <span data-ttu-id="61fcb-2937">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2937">Err</span></span> | <span data-ttu-id="61fcb-2938">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2938">Err</span></span> | <span data-ttu-id="61fcb-2939">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2939">Lo</span></span>  | <span data-ttu-id="61fcb-2940">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2940">Ob</span></span>  | 
| <span data-ttu-id="61fcb-2941">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2941">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2942">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2942">Lo</span></span> | <span data-ttu-id="61fcb-2943">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2943">Err</span></span> | <span data-ttu-id="61fcb-2944">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2944">Err</span></span> | <span data-ttu-id="61fcb-2945">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2945">Lo</span></span>  | <span data-ttu-id="61fcb-2946">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2946">Ob</span></span>  | 
| <span data-ttu-id="61fcb-2947">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2947">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-2948">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2948">Err</span></span> | <span data-ttu-id="61fcb-2949">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2949">Err</span></span> | <span data-ttu-id="61fcb-2950">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2950">Err</span></span> | <span data-ttu-id="61fcb-2951">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2951">Err</span></span> | 
| <span data-ttu-id="61fcb-2952">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2952">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="61fcb-2953">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2953">Err</span></span> | <span data-ttu-id="61fcb-2954">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2954">Err</span></span> | <span data-ttu-id="61fcb-2955">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-2955">Err</span></span> | 
| <span data-ttu-id="61fcb-2956">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2956">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="61fcb-2957">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-2957">Lo</span></span>  | <span data-ttu-id="61fcb-2958">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2958">Ob</span></span>  | 
| <span data-ttu-id="61fcb-2959">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2959">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="61fcb-2960">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-2960">Ob</span></span>  | 


### <a name="short-circuiting-logical-operators"></a><span data-ttu-id="61fcb-2961">Kurzschließen von logischen Operatoren</span><span class="sxs-lookup"><span data-stu-id="61fcb-2961">Short-circuiting Logical Operators</span></span>

<span data-ttu-id="61fcb-2962">Die `AndAlso` und `OrElse` Operatoren sind die verkürzte Versionen der `And` und `Or` logische Operatoren.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2962">The `AndAlso` and `OrElse` operators are the short-circuiting versions of the `And` and `Or` logical operators.</span></span>

```antlr
ShortCircuitLogicalOperatorExpression
    : Expression 'AndAlso' LineTerminator? Expression
    | Expression 'OrElse' LineTerminator? Expression
    ;
```

<span data-ttu-id="61fcb-2963">Aufgrund des kurzen kurzschließende Verhaltens ist der zweite Operand nicht zur Laufzeit ausgewertet, wenn das Ergebnis der Operator bekannt ist, nach der Auswertung der erste Operand.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2963">Because of their short circuiting behavior, the second operand is not evaluated at run time if the operator result is known after evaluating the first operand.</span></span>

<span data-ttu-id="61fcb-2964">Die verkürzte logische Operatoren werden wie folgt ausgewertet:</span><span class="sxs-lookup"><span data-stu-id="61fcb-2964">The short-circuiting logical operators are evaluated as follows:</span></span>

* <span data-ttu-id="61fcb-2965">Wenn der erste Operand in eine `AndAlso` Operation ergibt `False` fest oder gibt "true", aus der `IsFalse` -Operator, der Ausdruck gibt den ersten Operanden zurück.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2965">If the first operand in an `AndAlso` operation evaluates to `False` or returns True from its `IsFalse` operator, the expression returns its first operand.</span></span> <span data-ttu-id="61fcb-2966">Andernfalls der zweite Operand ist, ausgewertet und eine logische `And` Vorgang für die beiden Ergebnisse ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2966">Otherwise, the second operand is evaluated and a logical `And` operation is performed on the two results.</span></span>

* <span data-ttu-id="61fcb-2967">Wenn der erste Operand in eine `OrElse` Operation ergibt `True` fest oder gibt "true", aus der `IsTrue` -Operator, der Ausdruck gibt den ersten Operanden zurück.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2967">If the first operand in an `OrElse` operation evaluates to `True` or returns True from its `IsTrue` operator, the expression returns its first operand.</span></span> <span data-ttu-id="61fcb-2968">Andernfalls der zweite Operand ist, ausgewertet und eine logische `Or` Vorgang für die beiden Ergebnisse ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2968">Otherwise, the second operand is evaluated and a logical `Or` operation is performed on its two results.</span></span>

<span data-ttu-id="61fcb-2969">Die `AndAlso` und `OrElse` Operatoren sind für den Typ definierte `Boolean`, oder für einen beliebigen Typ `T` methodenüberladungen, die folgenden Operatoren:</span><span class="sxs-lookup"><span data-stu-id="61fcb-2969">The `AndAlso` and `OrElse` operators are defined for the type `Boolean`, or for any type `T` that overloads the following operators:</span></span>

```vb
Public Shared Operator IsTrue(op As T) As Boolean
Public Shared Operator IsFalse(op As T) As Boolean
```

<span data-ttu-id="61fcb-2970">sowie das Überladen zulässt, die entsprechende `And` oder `Or` Operator:</span><span class="sxs-lookup"><span data-stu-id="61fcb-2970">as well as overloading the corresponding `And` or `Or` operator:</span></span>

```vb
Public Shared Operator And(op1 As T, op2 As T) As T
Public Shared Operator Or(op1 As T, op2 As T) As T
```

<span data-ttu-id="61fcb-2971">Beim Auswerten der `AndAlso` oder `OrElse` Operatoren, der erste Operand wird nur einmal ausgewertet und der zweite Operand ist entweder nicht ausgewertet oder genau einmal ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2971">When evaluating the `AndAlso` or `OrElse` operators, the first operand is evaluated only once, and the second operand is either not evaluated or evaluated exactly once.</span></span> <span data-ttu-id="61fcb-2972">Beachten Sie z. B. folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="61fcb-2972">For example, consider the following code:</span></span>

```vb
Module Test
    Function TrueValue() As Boolean
        Console.Write(" True")
        Return True
    End Function

    Function FalseValue() As Boolean
        Console.Write(" False")
        Return False
    End Function

    Sub Main()
        Console.Write("And:")
        If FalseValue() And TrueValue() Then
        End If
        Console.WriteLine()

        Console.Write("Or:")
        If TrueValue() Or FalseValue() Then
        End If
        Console.WriteLine()

        Console.Write("AndAlso:")
        If FalseValue() AndAlso TrueValue() Then
        End If
        Console.WriteLine()

        Console.Write("OrElse:")
        If TrueValue() OrElse FalseValue() Then
        End If
        Console.WriteLine()
    End Sub
End Module
```

<span data-ttu-id="61fcb-2973">Gibt das folgende Ergebnis:</span><span class="sxs-lookup"><span data-stu-id="61fcb-2973">It prints the following result:</span></span>

```
And: False True
Or: True False
AndAlso: False
OrElse: True
```

<span data-ttu-id="61fcb-2974">In der transformierten Form der `AndAlso` und `OrElse` Operatoren, wenn der erste Operand ein NULL-Wert wurde `Boolean?`, der zweite Operand ausgewertet werden, das Ergebnis ist immer ein NULL-Wert `Boolean?`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-2974">In the lifted form of the `AndAlso` and `OrElse` operators, if the first operand was a null `Boolean?`, then the second operand is evaluated but the result is always a null `Boolean?`.</span></span>

<span data-ttu-id="61fcb-2975">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2975">__Operation Type:__</span></span>

|        | <span data-ttu-id="61fcb-2976">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2976">__Bo__</span></span> | <span data-ttu-id="61fcb-2977">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2977">__SB__</span></span> | <span data-ttu-id="61fcb-2978">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2978">__By__</span></span> | <span data-ttu-id="61fcb-2979">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2979">__Sh__</span></span> | <span data-ttu-id="61fcb-2980">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2980">__US__</span></span> | <span data-ttu-id="61fcb-2981">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2981">__In__</span></span> | <span data-ttu-id="61fcb-2982">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2982">__UI__</span></span> | <span data-ttu-id="61fcb-2983">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2983">__Lo__</span></span> | <span data-ttu-id="61fcb-2984">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2984">__UL__</span></span> | <span data-ttu-id="61fcb-2985">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2985">__De__</span></span> | <span data-ttu-id="61fcb-2986">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2986">__Si__</span></span> | <span data-ttu-id="61fcb-2987">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2987">__Do__</span></span> | <span data-ttu-id="61fcb-2988">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2988">__Da__</span></span>  | <span data-ttu-id="61fcb-2989">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2989">__Ch__</span></span>  | <span data-ttu-id="61fcb-2990">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2990">__St__</span></span> | <span data-ttu-id="61fcb-2991">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2991">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="61fcb-2992">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-2992">__Bo__</span></span> | <span data-ttu-id="61fcb-2993">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-2993">Bo</span></span> | <span data-ttu-id="61fcb-2994">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-2994">Bo</span></span> | <span data-ttu-id="61fcb-2995">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-2995">Bo</span></span> | <span data-ttu-id="61fcb-2996">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-2996">Bo</span></span> | <span data-ttu-id="61fcb-2997">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-2997">Bo</span></span> | <span data-ttu-id="61fcb-2998">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-2998">Bo</span></span> | <span data-ttu-id="61fcb-2999">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-2999">Bo</span></span> | <span data-ttu-id="61fcb-3000">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3000">Bo</span></span> | <span data-ttu-id="61fcb-3001">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3001">Bo</span></span> | <span data-ttu-id="61fcb-3002">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3002">Bo</span></span> | <span data-ttu-id="61fcb-3003">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3003">Bo</span></span> | <span data-ttu-id="61fcb-3004">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3004">Bo</span></span> | <span data-ttu-id="61fcb-3005">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3005">Err</span></span> | <span data-ttu-id="61fcb-3006">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3006">Err</span></span> | <span data-ttu-id="61fcb-3007">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3007">Bo</span></span>  | <span data-ttu-id="61fcb-3008">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-3008">Ob</span></span>  | 
| <span data-ttu-id="61fcb-3009">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3009">__SB__</span></span> |    | <span data-ttu-id="61fcb-3010">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3010">Bo</span></span> | <span data-ttu-id="61fcb-3011">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3011">Bo</span></span> | <span data-ttu-id="61fcb-3012">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3012">Bo</span></span> | <span data-ttu-id="61fcb-3013">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3013">Bo</span></span> | <span data-ttu-id="61fcb-3014">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3014">Bo</span></span> | <span data-ttu-id="61fcb-3015">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3015">Bo</span></span> | <span data-ttu-id="61fcb-3016">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3016">Bo</span></span> | <span data-ttu-id="61fcb-3017">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3017">Bo</span></span> | <span data-ttu-id="61fcb-3018">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3018">Bo</span></span> | <span data-ttu-id="61fcb-3019">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3019">Bo</span></span> | <span data-ttu-id="61fcb-3020">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3020">Bo</span></span> | <span data-ttu-id="61fcb-3021">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3021">Err</span></span> | <span data-ttu-id="61fcb-3022">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3022">Err</span></span> | <span data-ttu-id="61fcb-3023">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3023">Bo</span></span>  | <span data-ttu-id="61fcb-3024">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-3024">Ob</span></span>  | 
| <span data-ttu-id="61fcb-3025">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3025">__By__</span></span> |    |    | <span data-ttu-id="61fcb-3026">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3026">Bo</span></span> | <span data-ttu-id="61fcb-3027">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3027">Bo</span></span> | <span data-ttu-id="61fcb-3028">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3028">Bo</span></span> | <span data-ttu-id="61fcb-3029">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3029">Bo</span></span> | <span data-ttu-id="61fcb-3030">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3030">Bo</span></span> | <span data-ttu-id="61fcb-3031">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3031">Bo</span></span> | <span data-ttu-id="61fcb-3032">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3032">Bo</span></span> | <span data-ttu-id="61fcb-3033">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3033">Bo</span></span> | <span data-ttu-id="61fcb-3034">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3034">Bo</span></span> | <span data-ttu-id="61fcb-3035">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3035">Bo</span></span> | <span data-ttu-id="61fcb-3036">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3036">Err</span></span> | <span data-ttu-id="61fcb-3037">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3037">Err</span></span> | <span data-ttu-id="61fcb-3038">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3038">Bo</span></span>  | <span data-ttu-id="61fcb-3039">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-3039">Ob</span></span>  | 
| <span data-ttu-id="61fcb-3040">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3040">__Sh__</span></span> |    |    |    | <span data-ttu-id="61fcb-3041">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3041">Bo</span></span> | <span data-ttu-id="61fcb-3042">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3042">Bo</span></span> | <span data-ttu-id="61fcb-3043">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3043">Bo</span></span> | <span data-ttu-id="61fcb-3044">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3044">Bo</span></span> | <span data-ttu-id="61fcb-3045">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3045">Bo</span></span> | <span data-ttu-id="61fcb-3046">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3046">Bo</span></span> | <span data-ttu-id="61fcb-3047">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3047">Bo</span></span> | <span data-ttu-id="61fcb-3048">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3048">Bo</span></span> | <span data-ttu-id="61fcb-3049">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3049">Bo</span></span> | <span data-ttu-id="61fcb-3050">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3050">Err</span></span> | <span data-ttu-id="61fcb-3051">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3051">Err</span></span> | <span data-ttu-id="61fcb-3052">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3052">Bo</span></span>  | <span data-ttu-id="61fcb-3053">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-3053">Ob</span></span>  | 
| <span data-ttu-id="61fcb-3054">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3054">__US__</span></span> |    |    |    |    | <span data-ttu-id="61fcb-3055">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3055">Bo</span></span> | <span data-ttu-id="61fcb-3056">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3056">Bo</span></span> | <span data-ttu-id="61fcb-3057">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3057">Bo</span></span> | <span data-ttu-id="61fcb-3058">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3058">Bo</span></span> | <span data-ttu-id="61fcb-3059">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3059">Bo</span></span> | <span data-ttu-id="61fcb-3060">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3060">Bo</span></span> | <span data-ttu-id="61fcb-3061">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3061">Bo</span></span> | <span data-ttu-id="61fcb-3062">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3062">Bo</span></span> | <span data-ttu-id="61fcb-3063">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3063">Err</span></span> | <span data-ttu-id="61fcb-3064">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3064">Err</span></span> | <span data-ttu-id="61fcb-3065">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3065">Bo</span></span>  | <span data-ttu-id="61fcb-3066">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-3066">Ob</span></span>  | 
| <span data-ttu-id="61fcb-3067">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3067">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="61fcb-3068">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3068">Bo</span></span> | <span data-ttu-id="61fcb-3069">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3069">Bo</span></span> | <span data-ttu-id="61fcb-3070">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3070">Bo</span></span> | <span data-ttu-id="61fcb-3071">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3071">Bo</span></span> | <span data-ttu-id="61fcb-3072">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3072">Bo</span></span> | <span data-ttu-id="61fcb-3073">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3073">Bo</span></span> | <span data-ttu-id="61fcb-3074">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3074">Bo</span></span> | <span data-ttu-id="61fcb-3075">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3075">Err</span></span> | <span data-ttu-id="61fcb-3076">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3076">Err</span></span> | <span data-ttu-id="61fcb-3077">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3077">Bo</span></span>  | <span data-ttu-id="61fcb-3078">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-3078">Ob</span></span>  | 
| <span data-ttu-id="61fcb-3079">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3079">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="61fcb-3080">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3080">Bo</span></span> | <span data-ttu-id="61fcb-3081">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3081">Bo</span></span> | <span data-ttu-id="61fcb-3082">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3082">Bo</span></span> | <span data-ttu-id="61fcb-3083">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3083">Bo</span></span> | <span data-ttu-id="61fcb-3084">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3084">Bo</span></span> | <span data-ttu-id="61fcb-3085">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3085">Bo</span></span> | <span data-ttu-id="61fcb-3086">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3086">Err</span></span> | <span data-ttu-id="61fcb-3087">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3087">Err</span></span> | <span data-ttu-id="61fcb-3088">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3088">Bo</span></span>  | <span data-ttu-id="61fcb-3089">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-3089">Ob</span></span>  | 
| <span data-ttu-id="61fcb-3090">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3090">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-3091">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3091">Bo</span></span> | <span data-ttu-id="61fcb-3092">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3092">Bo</span></span> | <span data-ttu-id="61fcb-3093">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3093">Bo</span></span> | <span data-ttu-id="61fcb-3094">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3094">Bo</span></span> | <span data-ttu-id="61fcb-3095">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3095">Bo</span></span> | <span data-ttu-id="61fcb-3096">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3096">Err</span></span> | <span data-ttu-id="61fcb-3097">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3097">Err</span></span> | <span data-ttu-id="61fcb-3098">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3098">Bo</span></span>  | <span data-ttu-id="61fcb-3099">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-3099">Ob</span></span>  | 
| <span data-ttu-id="61fcb-3100">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3100">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-3101">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3101">Bo</span></span> | <span data-ttu-id="61fcb-3102">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3102">Bo</span></span> | <span data-ttu-id="61fcb-3103">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3103">Bo</span></span> | <span data-ttu-id="61fcb-3104">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3104">Bo</span></span> | <span data-ttu-id="61fcb-3105">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3105">Err</span></span> | <span data-ttu-id="61fcb-3106">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3106">Err</span></span> | <span data-ttu-id="61fcb-3107">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3107">Bo</span></span>  | <span data-ttu-id="61fcb-3108">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-3108">Ob</span></span>  | 
| <span data-ttu-id="61fcb-3109">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3109">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-3110">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3110">Bo</span></span> | <span data-ttu-id="61fcb-3111">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3111">Bo</span></span> | <span data-ttu-id="61fcb-3112">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3112">Bo</span></span> | <span data-ttu-id="61fcb-3113">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3113">Err</span></span> | <span data-ttu-id="61fcb-3114">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3114">Err</span></span> | <span data-ttu-id="61fcb-3115">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3115">Bo</span></span>  | <span data-ttu-id="61fcb-3116">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-3116">Ob</span></span>  | 
| <span data-ttu-id="61fcb-3117">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3117">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-3118">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3118">Bo</span></span> | <span data-ttu-id="61fcb-3119">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3119">Bo</span></span> | <span data-ttu-id="61fcb-3120">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3120">Err</span></span> | <span data-ttu-id="61fcb-3121">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3121">Err</span></span> | <span data-ttu-id="61fcb-3122">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3122">Bo</span></span>  | <span data-ttu-id="61fcb-3123">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-3123">Ob</span></span>  | 
| <span data-ttu-id="61fcb-3124">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3124">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-3125">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3125">Bo</span></span> | <span data-ttu-id="61fcb-3126">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3126">Err</span></span> | <span data-ttu-id="61fcb-3127">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3127">Err</span></span> | <span data-ttu-id="61fcb-3128">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3128">Bo</span></span>  | <span data-ttu-id="61fcb-3129">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-3129">Ob</span></span>  | 
| <span data-ttu-id="61fcb-3130">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3130">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="61fcb-3131">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3131">Err</span></span> | <span data-ttu-id="61fcb-3132">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3132">Err</span></span> | <span data-ttu-id="61fcb-3133">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3133">Err</span></span> | <span data-ttu-id="61fcb-3134">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3134">Err</span></span> | 
| <span data-ttu-id="61fcb-3135">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3135">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="61fcb-3136">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3136">Err</span></span> | <span data-ttu-id="61fcb-3137">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3137">Err</span></span> | <span data-ttu-id="61fcb-3138">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3138">Err</span></span> | 
| <span data-ttu-id="61fcb-3139">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3139">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="61fcb-3140">BO</span><span class="sxs-lookup"><span data-stu-id="61fcb-3140">Bo</span></span>  | <span data-ttu-id="61fcb-3141">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-3141">Ob</span></span>  | 
| <span data-ttu-id="61fcb-3142">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3142">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="61fcb-3143">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-3143">Ob</span></span>  | 


## <a name="shift-operators"></a><span data-ttu-id="61fcb-3144">Schiebeoperatoren</span><span class="sxs-lookup"><span data-stu-id="61fcb-3144">Shift Operators</span></span>

<span data-ttu-id="61fcb-3145">Die binären Operatoren `<<` und `>>` Bit wechselnden Vorgänge ausführen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3145">The binary operators `<<` and `>>` perform bit shifting operations.</span></span>

```antlr
ShiftOperatorExpression
    : Expression '<' '<' LineTerminator? Expression
    | Expression '>' '>' LineTerminator? Expression
    ;
```

<span data-ttu-id="61fcb-3146">Die Operatoren definiert sind, für die `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` und `Long` Typen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3146">The operators are defined for the `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` and `Long` types.</span></span> <span data-ttu-id="61fcb-3147">Im Gegensatz zu anderen binären Operatoren bestimmt der Ergebnistyp eines schiebevorgangs, als ob der Operator ein unäroperator nur der linke Operand war.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3147">Unlike the other binary operators, the result type of a shift operation is determined as if the operator was a unary operator with just the left operand.</span></span> <span data-ttu-id="61fcb-3148">Der Typ des rechten Operanden muss implizit in `Integer` und wird bei der Bestimmung der Ergebnistyp des Vorgangs nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3148">The type of the right operand must be implicitly convertible to `Integer` and is not used in determining the result type of the operation.</span></span>

<span data-ttu-id="61fcb-3149">Die `<<` bewirkt, dass die Bits in den ersten Operanden verschoben werden sollen die Anzahl der Stellen, die gemäß den Betrag der Verschiebung nach links.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3149">The `<<` operator causes the bits in the first operand to be shifted left the number of places specified by the shift amount.</span></span> <span data-ttu-id="61fcb-3150">Die höherwertigen Bits außerhalb des Bereichs des Ergebnistyps werden verworfen, und die niederwertigen frei gewordenen Bitpositionen werden mit Nullen aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3150">The high-order bits outside the range of the result type are discarded and the low-order vacated bit positions are zero-filled.</span></span>

<span data-ttu-id="61fcb-3151">Die `>>` Operator bewirkt, dass die Bits in den ersten Operanden verschoben werden sollen, nach rechts die Anzahl der Stellen, die gemäß den Betrag der Verschiebung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3151">The `>>` operator causes the bits in the first operand to be shifted right the number of places specified by the shift amount.</span></span> <span data-ttu-id="61fcb-3152">Die niederwertigen Bits verworfen, und auf eine falls negativ oder 0 (null), wenn der linke Operand positiv ist, werden die frei gewordenen Bitpositionen mit höherwertigen festgelegt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3152">The low-order bits are discarded and the high-order vacated bit positions are set to zero if the left operand is positive or to one if negative.</span></span> <span data-ttu-id="61fcb-3153">Wenn der linke Operand vom Typ `Byte`, `UShort`, `UInteger`, oder `ULong` der freien höherwertigen Bits werden mit Nullen aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3153">If the left operand is of type `Byte`, `UShort`, `UInteger`, or `ULong` the vacant high-order bits are zero-filled.</span></span>

<span data-ttu-id="61fcb-3154">Die Schiebeoperatoren verschieben die Bits für die zugrunde liegende Darstellung des ersten Operanden durch die Menge des zweiten Operanden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3154">The shift operators shift the bits of the underlying representation of the first operand by the amount of the second operand.</span></span> <span data-ttu-id="61fcb-3155">Wenn der Wert des zweiten Operanden ist größer als die Anzahl der Bits in den ersten Operanden oder negativ ist, berechnet den Betrag der Verschiebung als `RightOperand And SizeMask` , in denen `SizeMask` ist:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3155">If the value of the second operand is greater than the number of bits in the first operand, or is negative, then the shift amount is computed as `RightOperand And SizeMask` where `SizeMask` is:</span></span>

| <span data-ttu-id="61fcb-3156">__LeftOperand-Typ__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3156">__LeftOperand Type__</span></span>  | <span data-ttu-id="61fcb-3157">__SizeMask__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3157">__SizeMask__</span></span> | 
|-----------------------|--------------|
| <span data-ttu-id="61fcb-3158">`Byte`, `SByte`</span><span class="sxs-lookup"><span data-stu-id="61fcb-3158">`Byte`, `SByte`</span></span>       | <span data-ttu-id="61fcb-3159">7 (`&H7`)</span><span class="sxs-lookup"><span data-stu-id="61fcb-3159">7 (`&H7`)</span></span>    | 
| <span data-ttu-id="61fcb-3160">`UShort`, `Short`</span><span class="sxs-lookup"><span data-stu-id="61fcb-3160">`UShort`, `Short`</span></span>     | <span data-ttu-id="61fcb-3161">15 (`&HF`)</span><span class="sxs-lookup"><span data-stu-id="61fcb-3161">15 (`&HF`)</span></span>   | 
| <span data-ttu-id="61fcb-3162">`UInteger`, `Integer`</span><span class="sxs-lookup"><span data-stu-id="61fcb-3162">`UInteger`, `Integer`</span></span> | <span data-ttu-id="61fcb-3163">31 (`&H1F`)</span><span class="sxs-lookup"><span data-stu-id="61fcb-3163">31 (`&H1F`)</span></span>  | 
| <span data-ttu-id="61fcb-3164">`ULong`, `Long`</span><span class="sxs-lookup"><span data-stu-id="61fcb-3164">`ULong`, `Long`</span></span>       | <span data-ttu-id="61fcb-3165">63 (`&H3F`)</span><span class="sxs-lookup"><span data-stu-id="61fcb-3165">63 (`&H3F`)</span></span>  | 

<span data-ttu-id="61fcb-3166">Wenn Sie den Betrag der Verschiebung auf 0 (null) ist, ist das Ergebnis des Vorgangs mit dem Wert des ersten Operanden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3166">If the shift amount is zero, the result of the operation is identical to the value of the first operand.</span></span> <span data-ttu-id="61fcb-3167">Keine Überläufe sind von diesen Vorgängen möglich.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3167">No overflows are possible from these operations.</span></span>

<span data-ttu-id="61fcb-3168">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3168">__Operation Type:__</span></span>


| <span data-ttu-id="61fcb-3169">__Bo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3169">__Bo__</span></span> | <span data-ttu-id="61fcb-3170">__SB__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3170">__SB__</span></span> | <span data-ttu-id="61fcb-3171">__By__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3171">__By__</span></span> | <span data-ttu-id="61fcb-3172">__Sh__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3172">__Sh__</span></span> | <span data-ttu-id="61fcb-3173">__US__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3173">__US__</span></span> | <span data-ttu-id="61fcb-3174">__In__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3174">__In__</span></span> | <span data-ttu-id="61fcb-3175">__UI__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3175">__UI__</span></span> | <span data-ttu-id="61fcb-3176">__Lo__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3176">__Lo__</span></span> | <span data-ttu-id="61fcb-3177">__UL__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3177">__UL__</span></span> | <span data-ttu-id="61fcb-3178">__De__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3178">__De__</span></span> | <span data-ttu-id="61fcb-3179">__Si__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3179">__Si__</span></span> | <span data-ttu-id="61fcb-3180">__Do__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3180">__Do__</span></span> | <span data-ttu-id="61fcb-3181">__Da__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3181">__Da__</span></span>  | <span data-ttu-id="61fcb-3182">__Ch__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3182">__Ch__</span></span>  | <span data-ttu-id="61fcb-3183">__St__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3183">__St__</span></span> | <span data-ttu-id="61fcb-3184">__Ob__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3184">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="61fcb-3185">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-3185">Sh</span></span> | <span data-ttu-id="61fcb-3186">SB</span><span class="sxs-lookup"><span data-stu-id="61fcb-3186">SB</span></span> | <span data-ttu-id="61fcb-3187">um</span><span class="sxs-lookup"><span data-stu-id="61fcb-3187">By</span></span> | <span data-ttu-id="61fcb-3188">Sh</span><span class="sxs-lookup"><span data-stu-id="61fcb-3188">Sh</span></span> | <span data-ttu-id="61fcb-3189">US</span><span class="sxs-lookup"><span data-stu-id="61fcb-3189">US</span></span> | <span data-ttu-id="61fcb-3190">In</span><span class="sxs-lookup"><span data-stu-id="61fcb-3190">In</span></span> | <span data-ttu-id="61fcb-3191">UI</span><span class="sxs-lookup"><span data-stu-id="61fcb-3191">UI</span></span> | <span data-ttu-id="61fcb-3192">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-3192">Lo</span></span> | <span data-ttu-id="61fcb-3193">UL</span><span class="sxs-lookup"><span data-stu-id="61fcb-3193">UL</span></span> | <span data-ttu-id="61fcb-3194">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-3194">Lo</span></span> | <span data-ttu-id="61fcb-3195">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-3195">Lo</span></span> | <span data-ttu-id="61fcb-3196">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-3196">Lo</span></span> | <span data-ttu-id="61fcb-3197">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3197">Err</span></span> | <span data-ttu-id="61fcb-3198">Err</span><span class="sxs-lookup"><span data-stu-id="61fcb-3198">Err</span></span> | <span data-ttu-id="61fcb-3199">Lo</span><span class="sxs-lookup"><span data-stu-id="61fcb-3199">Lo</span></span> | <span data-ttu-id="61fcb-3200">Ob</span><span class="sxs-lookup"><span data-stu-id="61fcb-3200">Ob</span></span> | 


## <a name="boolean-expressions"></a><span data-ttu-id="61fcb-3201">Boolesche Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-3201">Boolean Expressions</span></span>

<span data-ttu-id="61fcb-3202">Ein boolescher Ausdruck ist ein Ausdruck, der getestet werden kann, um festzustellen, ob es "true" ist, oder wenn sie falsch ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3202">A Boolean expression is an expression that can be tested to see if it is true or if it is false.</span></span>

```antlr
BooleanExpression
    : Expression
    ;
```

<span data-ttu-id="61fcb-3203">Ein Typ `T` kann in einen booleschen Ausdruck verwendet werden, sofern es sich um Reihenfolge ihrer Priorität:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3203">A type `T` can be used in a Boolean expression if, in order of preference:</span></span>

* <span data-ttu-id="61fcb-3204">`T` ist `Boolean` oder `Boolean?`</span><span class="sxs-lookup"><span data-stu-id="61fcb-3204">`T` is `Boolean` or `Boolean?`</span></span>

* <span data-ttu-id="61fcb-3205">`T` verfügt über eine erweiternde Konvertierung in `Boolean`</span><span class="sxs-lookup"><span data-stu-id="61fcb-3205">`T` has a widening conversion to `Boolean`</span></span>

* <span data-ttu-id="61fcb-3206">`T` verfügt über eine erweiternde Konvertierung in `Boolean?`</span><span class="sxs-lookup"><span data-stu-id="61fcb-3206">`T` has a widening conversion to `Boolean?`</span></span>

* <span data-ttu-id="61fcb-3207">`T` definiert zwei Pseudo-Operatoren, `IsTrue` und `IsFalse`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3207">`T` defines two pseudo operators, `IsTrue` and `IsFalse`.</span></span>

* <span data-ttu-id="61fcb-3208">`T` verfügt über eine einschränkende Konvertierung in `Boolean?` , ist eine Konvertierung von nicht beinhalten `Boolean` zu `Boolean?`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3208">`T` has a narrowing conversion to `Boolean?` that does not involve a conversion from `Boolean` to `Boolean?`.</span></span>

* <span data-ttu-id="61fcb-3209">`T` verfügt über eine einschränkende Konvertierung in `Boolean`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3209">`T` has a narrowing conversion to `Boolean`.</span></span>

<span data-ttu-id="61fcb-3210">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3210">__Note.__</span></span> <span data-ttu-id="61fcb-3211">Es ist interessant, beachten Sie, dass bei `Option Strict` deaktiviert ist, einen Ausdruck mit eine einschränkende Konvertierung in `Boolean` wird akzeptiert, ohne dass ein Fehler während der Kompilierung, aber der Sprache weiterhin bevorzugen eine `IsTrue` Operator, wenn es vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3211">It is interesting to note that if `Option Strict` is off, an expression that has a narrowing conversion to `Boolean` will be accepted without a compile-time error but the language will still prefer an `IsTrue` operator if it exists.</span></span> <span data-ttu-id="61fcb-3212">Grund hierfür ist, `Option Strict` nur geändert werden, was wird nicht von der Sprache akzeptiert und ist nie ändert die eigentliche Bedeutung eines Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3212">This is because `Option Strict` only changes what is and isn't accepted by the language, and never changes the actual meaning of an expression.</span></span> <span data-ttu-id="61fcb-3213">Daher `IsTrue` muss immer über eine einschränkende Konvertierung, bevorzugt werden, unabhängig von `Option Strict`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3213">Thus, `IsTrue` has to always be preferred over a narrowing conversion, regardless of `Option Strict`.</span></span>

<span data-ttu-id="61fcb-3214">Die folgende Klasse definiert z. B. keine erweiternde Konvertierung in `Boolean`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3214">For example, the following class does not define a widening conversion to `Boolean`.</span></span> <span data-ttu-id="61fcb-3215">Als Ergebnis der Verwendung in der die `If` Anweisung bewirkt, dass einen Aufruf der `IsTrue` Operator.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3215">As a result, its use in the `If` statement causes a call to the `IsTrue` operator.</span></span>

```vb
Class MyBool
    Public Shared Widening Operator CType(b As Boolean) As MyBool
        ...
    End Operator

    Public Shared Narrowing Operator CType(b As MyBool) As Boolean
        ...
    End Operator

    Public Shared Operator IsTrue(b As MyBool) As Boolean
        ...
    End Operator

    Public Shared Operator IsFalse(b As MyBool) As Boolean
        ...
    End Operator
End Class

Module Test
    Sub Main()
        Dim b As New MyBool

        If b Then Console.WriteLine("True")
    End Sub
End Module
```

<span data-ttu-id="61fcb-3216">Wenn als typisiert oder konvertiert ein boolescher Ausdruck `Boolean` oder `Boolean?`, wenn der Wert "true" ist `True` und "false" andernfalls.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3216">If a Boolean expression is typed as or converted to `Boolean` or `Boolean?`, then it is true if the value is `True` and false otherwise.</span></span>

<span data-ttu-id="61fcb-3217">Andernfalls ein boolescher Ausdruck ruft die `IsTrue` Operator und gibt `True` Operator zurückgegeben `True`; andernfalls "false" ist (jedoch nie aufruft, die `IsFalse` Operator).</span><span class="sxs-lookup"><span data-stu-id="61fcb-3217">Otherwise, a Boolean expression calls the `IsTrue` operator and returns `True` if the operator returned `True`; otherwise it is false (but never calls the `IsFalse` operator).</span></span>

<span data-ttu-id="61fcb-3218">Im folgenden Beispiel `Integer` verfügt über eine einschränkende Konvertierung in `Boolean`, sodass ein NULL-Wert `Integer?` verfügt über eine einschränkende Konvertierung in beide `Boolean?` (Rückgabe von Null `Boolean`) und `Boolean` (die löst eine Ausnahme aus).</span><span class="sxs-lookup"><span data-stu-id="61fcb-3218">In the following example, `Integer` has a narrowing conversion to `Boolean`, so a null `Integer?` has a narrowing conversion to both `Boolean?` (yielding a null `Boolean`) and to `Boolean` (which throws an exception).</span></span> <span data-ttu-id="61fcb-3219">Die einschränkende Konvertierung in `Boolean?` wird empfohlen, und daher den Wert des "`i`" als booleschen Ausdruck ist daher `False`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3219">The narrowing conversion to `Boolean?` is preferred, and so the value of "`i`" as a Boolean expression is therefore `False`.</span></span>

```vb
Dim i As Integer? = Nothing
If i Then Console.WriteLine()
```


## <a name="lambda-expressions"></a><span data-ttu-id="61fcb-3220">Lambda-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-3220">Lambda Expressions</span></span>

<span data-ttu-id="61fcb-3221">Ein *Lambda-Ausdruck* definiert eine anonyme Methode wird aufgerufen, eine *Lambda-Methode*.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3221">A *lambda expression* defines an anonymous method called a *lambda method*.</span></span> <span data-ttu-id="61fcb-3222">Lambda-Methoden erleichtern die "inline"-Methoden an andere Methoden übergeben, die Delegattypen zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3222">Lambda methods make it easy to pass "in-line" methods to other methods that take delegate types.</span></span>

```antlr
LambdaExpression
    : SingleLineLambda
    | MultiLineLambda
    ;

SingleLineLambda
    : LambdaModifier* 'Function' ( OpenParenthesis ParameterList? CloseParenthesis )? Expression
    | 'Sub' ( OpenParenthesis ParameterList? CloseParenthesis )? Statement
    ;

MultiLineLambda
    : MultiLineFunctionLambda
    | MultiLineSubLambda
    ;

MultiLineFunctionLambda
    : LambdaModifier? 'Function' ( OpenParenthesis ParameterList? CloseParenthesis )? ( 'As' TypeName )? LineTerminator
      Block
      'End' 'Function'
    ;

MultiLineSubLambda
    : LambdaModifier? 'Sub' ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block
      'End' 'Sub'
    ;

LambdaModifier
    : 'Async' | 'Iterator'
    ;
```

<span data-ttu-id="61fcb-3223">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3223">The example:</span></span>

```vb
Module Test
    Delegate Function IntFunc(x As Integer) As Integer

    Sub Apply(a() As Integer, func As IntFunc)
        For index As Integer = 0 To a.Length - 1
            a(index) = func(a(index))
        Next index
    End Sub

    Sub Main()
        Dim a() As Integer = { 1, 2, 3, 4 }

        Apply(a, Function(x As Integer) x * 2)

        For Each value In a
            Console.Write(value & " ")
        Next value
    End Sub
End Module
```

<span data-ttu-id="61fcb-3224">wird ausgegeben:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3224">will print out:</span></span>

```
2 4 6 8
```

<span data-ttu-id="61fcb-3225">Ein Lambda-Ausdruck beginnt mit der optionale Modifizierer `Async` oder `Iterator`, gefolgt vom Schlüsselwort `Function` oder `Sub` und einer Parameterliste.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3225">A lambda expression begins with the optional modifiers `Async` or `Iterator`, followed by the keyword `Function` or `Sub` and a parameter list.</span></span> <span data-ttu-id="61fcb-3226">Parameter in einem Lambdaausdruck können nicht deklariert werden `Optional` oder `ParamArray` und darf keine Attribute aufweisen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3226">Parameters in a lambda expression cannot be declared `Optional` or `ParamArray` and cannot have attributes.</span></span> <span data-ttu-id="61fcb-3227">Im Gegensatz zu regulären Methoden, das Auslassen der Parametertyp für eine Lambda-Methode automatisch leitet nicht `Object`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3227">Unlike regular methods, omitting a parameter type for a lambda method does not automatically infer `Object`.</span></span> <span data-ttu-id="61fcb-3228">Stattdessen bei eine Lambda-Methode ist neu klassifiziert, die ausgelassenen Parametertypen und `ByRef` Modifizierer aus dem Zieltyp abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3228">Instead, when a lambda method is reclassified, the omitted parameter types and `ByRef` modifiers are inferred from the target type.</span></span> <span data-ttu-id="61fcb-3229">Im vorherigen Beispiel der Lambda-Ausdruck ließe als `Function(x) x * 2`, und es würde den Typ des abgeleitet haben `x` sein `Integer` Wenn der Lambda-Methode wurde zum Erstellen einer Instanz von verwendet die `IntFunc` Delegattyp.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3229">In the previous example, the lambda expression could have been written as `Function(x) x * 2`, and it would have inferred the type of `x` to be `Integer` when the lambda method was used to create an instance of the `IntFunc` delegate type.</span></span> <span data-ttu-id="61fcb-3230">Im Gegensatz zu lokalen Variablen Typrückschluss Wenn Lambda-Parameter der Methode einen Typ lässt, aber verfügt über ein Array oder NULL-Werte zulassen Namen Modifizierer verwenden, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3230">Unlike local variable inference, if a lambda method parameter omits a type but has an array or nullable name modifier, a compile-time error occurs.</span></span>

<span data-ttu-id="61fcb-3231">Ein __regulären Lambda-Ausdruck__ ist ein URI mit keiner `Async` noch `Iterator` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3231">A __regular lambda expression__ is one with neither `Async` nor `Iterator` modifiers.</span></span>

<span data-ttu-id="61fcb-3232">Ein __Iterator-Lambdaausdruck__ ist ein URI mit der `Iterator` -Modifizierer und keine `Async` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3232">An __iterator lambda expression__ is one with the `Iterator` modifier and no `Async` modifier.</span></span> <span data-ttu-id="61fcb-3233">Es muss eine Funktion sein.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3233">It must be a function.</span></span> <span data-ttu-id="61fcb-3234">Wenn sie auf einen anderen Wert neu klassifiziert werden, es kann nur neu auf einen Wert der Delegattyp, dessen Rückgabetyp `IEnumerator`, oder `IEnumerable`, oder `IEnumerator(Of T)` oder `IEnumerable(Of T)` für einige `T`, verfügt über keine ByRef-Parameter.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3234">When it is reclassified to a value, it can only be reclassified to a value of delegate type whose return type is `IEnumerator`, or `IEnumerable`, or `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`, and which has no ByRef parameters.</span></span>

<span data-ttu-id="61fcb-3235">Ein __Async-Lambdaausdruck__ ist ein URI mit der `Async` -Modifizierer und keine `Iterator` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3235">An __async lambda expression__ is one with the `Async` modifier and no `Iterator` modifier.</span></span> <span data-ttu-id="61fcb-3236">Ein Async Sub-Lambda kann nur auf Sub-Delegattyp ohne ByRef-Parameter den Wert neu klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3236">An async sub lambda may only be reclassified to a value of sub delegate type with no ByRef parameters.</span></span> <span data-ttu-id="61fcb-3237">Das Lambda einer Async-Funktion kann nur auf einen Wert der Funktion Delegattyp, dessen Rückgabetyp, neu `Task` oder `Task(Of T)` für einige `T`, verfügt über keine ByRef-Parameter.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3237">An async function lambda may only be reclassified to a value of function delegate type whose return type is `Task` or `Task(Of T)` for some `T`, and which has no ByRef parameters.</span></span>

<span data-ttu-id="61fcb-3238">Lambda-Ausdrücke können entweder ein- oder mehrzeiligen sein.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3238">Lambda expressions can either be single-line or multi-line.</span></span> <span data-ttu-id="61fcb-3239">Einzeilige `Function` Lambda-Ausdrücke enthalten einen einzelnen Ausdruck, der den von der Lambda-Methode zurückgegebenen Wert darstellt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3239">Single-line `Function` lambda expressions contain a single expression that represents the value returned from the lambda method.</span></span> <span data-ttu-id="61fcb-3240">Einzeilige `Sub` Lambda-Ausdrücke enthalten eine einzelne Anweisung ohne dem abschließenden `StatementTerminator`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3240">Single-line `Sub` lambda expressions contain a single statement without its closing `StatementTerminator`.</span></span> <span data-ttu-id="61fcb-3241">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3241">For example:</span></span>

```vb
Module Test
    Sub Do(a() As Integer, action As Action(Of Integer))
        For index As Integer = 0 To a.Length - 1
            action(a(index))
        Next index
    End Sub

    Sub Main()
        Dim a() As Integer = { 1, 2, 3, 4 }

        Do(a, Sub(x As Integer) Console.WriteLine(x))
    End Sub
End Module
```

<span data-ttu-id="61fcb-3242">Einzeilige Lambda erstellt Bindung weniger streng als alle anderen Ausdrücke und Anweisungen an.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3242">Single-line lambda constructs bind less tightly than all other expressions and statements.</span></span> <span data-ttu-id="61fcb-3243">Also z. B. "`Function() x + 5`"ist gleich"`Function() (x+5)"` statt"`(Function() x) + 5`".</span><span class="sxs-lookup"><span data-stu-id="61fcb-3243">Thus, for example, "`Function() x + 5`" is equivalent to "`Function() (x+5)"` rather than "`(Function() x) + 5`".</span></span> <span data-ttu-id="61fcb-3244">Um Mehrdeutigkeit zu einem einzeiligen verhindern `Sub` Lambda-Ausdruck darf keine, eine Dim-Anweisung oder eine Bezeichnung-Declaration-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3244">To avoid ambiguity, a single-line `Sub` lambda expression may not contain a Dim statement or a label declaration statement.</span></span> <span data-ttu-id="61fcb-3245">Darüber hinaus, wenn es in einem einzeiligen Klammern eingeschlossen ist `Sub` Lambda-Ausdruck kann nicht unmittelbar folgen ein Doppelpunkt ":", ein Memberzugriffsoperator ".", ein Wörterbuch-Memberzugriffsoperator "!" oder eine öffnende Klammer "(".</span><span class="sxs-lookup"><span data-stu-id="61fcb-3245">Also, unless it is enclosed in parentheses, a single-line `Sub` lambda expression may not be immediately followed by a colon ":", a member access operator ".", a dictionary member access operator "!" or an open parenthesis "(".</span></span> <span data-ttu-id="61fcb-3246">Enthält keine Block-Anweisung (`With`, `SyncLock, If...EndIf`, `While`, `For`, `Do`, `Using`) noch `OnError` noch `Resume`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3246">It may not contain any block statement (`With`, `SyncLock, If...EndIf`, `While`, `For`, `Do`, `Using`) nor `OnError` nor `Resume`.</span></span>

<span data-ttu-id="61fcb-3247">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3247">__Note.__</span></span> <span data-ttu-id="61fcb-3248">Im Lambda-Ausdruck `Function(i) x=i`, als Text interpretiert eine *Ausdruck* (welche Tests, ob `x` und `i` gleich sind).</span><span class="sxs-lookup"><span data-stu-id="61fcb-3248">In the lambda expression `Function(i) x=i`, the body is interpreted as an *expression* (which tests whether `x` and `i` are equal).</span></span> <span data-ttu-id="61fcb-3249">Aber im Lambda-Ausdruck `Sub(i) x=i`, wird der Text als Anweisung interpretiert (die weist `i` zu `x`).</span><span class="sxs-lookup"><span data-stu-id="61fcb-3249">But in the lambda expression `Sub(i) x=i`, the body is interpreted as a statement (which assigns `i` to `x`).</span></span>

<span data-ttu-id="61fcb-3250">Ein mehrzeiligen Lambda-Ausdruck enthält einen Anweisungsblock und enden mit einem entsprechenden `End` Anweisung (d. h. `End Function` oder `End Sub`).</span><span class="sxs-lookup"><span data-stu-id="61fcb-3250">A multi-line lambda expression contains a statement block and must end with an appropriate `End` statement (i.e. `End Function` or `End Sub`).</span></span> <span data-ttu-id="61fcb-3251">Wie bei regulärer Methoden einer mehrzeiligen Lambda-Methode des `Function` oder `Sub` Anweisung und `End` -Anweisungen müssen in eigenen Zeilen sein.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3251">As with regular methods, a multi-line lambda method's `Function` or `Sub` statement and `End` statements must be on their own lines.</span></span> <span data-ttu-id="61fcb-3252">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3252">For example:</span></span>

```vb
' Error: Function statement must be on its own line!
Dim x = Sub(x As Integer) : Console.WriteLine(x) : End Sub

' OK
Dim y = Sub(x As Integer)
               Console.WriteLine(x)
          End Sub
```

<span data-ttu-id="61fcb-3253">Mehrzeilige `Function` Lambda-Ausdrücke können einen Rückgabetyp deklarieren, aber Attribute können nicht eingefügt werden, darauf.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3253">Multi-line `Function` lambda expressions can declare a return type but cannot put attributes on it.</span></span> <span data-ttu-id="61fcb-3254">Wenn ein mehrzeiliges `Function` Lambda-Ausdruck einen Rückgabetyp deklariert, aber der Rückgabetyp abgeleitet werden kann, aus dem Kontext, in dem der Lambda-Ausdruck wird verwendet, wird dieser Rückgabetyp verwendet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3254">If a multi-line `Function` lambda expression does not declare a return type but the return type can be inferred from the context in which the lambda expression is used , then that return type is used.</span></span> <span data-ttu-id="61fcb-3255">Andernfalls wird der Rückgabetyp der Funktion wie folgt berechnet:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3255">Otherwise the return type of the function is calculated as follows:</span></span>

* <span data-ttu-id="61fcb-3256">In einem regulären Lambda-Ausdruck, der Rückgabetyp ist der bestimmende Typ der Ausdrücke in alle der `Return` Anweisungen in der Anweisungsblock.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3256">In a regular lambda expression, the return type is the dominant type of the expressions in all the `Return` statements in the statement block.</span></span>

* <span data-ttu-id="61fcb-3257">In einer Async-Lambda-Ausdruck, der Rückgabetyp ist `Task(Of T)` , in denen `T` ist der bestimmende Typ der Ausdrücke in alle der `Return` Anweisungen in der Anweisungsblock.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3257">In an async lambda expression, the return type is `Task(Of T)` where `T` is the dominant type of the expressions in all the `Return` statements in the statement block.</span></span>

* <span data-ttu-id="61fcb-3258">In einem Iterator Lambda-Ausdruck, der Rückgabetyp ist `IEnumerable(Of T)` , in denen `T` ist der bestimmende Typ der Ausdrücke in alle der `Yield` Anweisungen in der Anweisungsblock.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3258">In an iterator lambda expression, the return type is `IEnumerable(Of T)` where `T` is the dominant type of the expressions in all the `Yield` statements in the statement block.</span></span>

<span data-ttu-id="61fcb-3259">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3259">For example:</span></span>

```vb
Function f(min As Integer, max As Integer) As IEnumerable(Of Integer)
    If min > max Then Throw New ArgumentException()
    Dim x = Iterator Function()
                  For i = min To max
                    Yield i
                Next
               End Function

    ' infers x to be a delegate with return type IEnumerable(Of Integer)
    Return x()
End Function
```

<span data-ttu-id="61fcb-3260">In allen Fällen gibt es keine `Return` (bzw. `Yield`)-Anweisungen, oder wenn kein dominanter Typ zwischen ihnen vorhanden ist und strenge Semantik verwendet wird, tritt ein Fehler während der Kompilierung ist; andernfalls ist des bestimmenden Typs implizit `Object`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3260">In all cases, if there are no `Return` (respectively `Yield`) statements, or if there is no dominant type among them, and strict semantics are being used, a compile-time error occurs; otherwise the dominant type is implicitly `Object`.</span></span>

<span data-ttu-id="61fcb-3261">Beachten Sie, dass der Rückgabetyp von allen berechnet wird `Return` -Anweisungen, selbst wenn sie nicht erreichbar sind.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3261">Note that the return type is calculated from all `Return` statements, even if they are not reachable.</span></span> <span data-ttu-id="61fcb-3262">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3262">For example:</span></span>

```vb
' Return type is Double
Dim x = Function()
              Return 10
               Return 10.50
          End Function
```

<span data-ttu-id="61fcb-3263">Es gibt keine implizite Rückgabevariablen auf, da kein Name für die Variable vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3263">There is no implicit return variable, as there is no name for the variable.</span></span>

<span data-ttu-id="61fcb-3264">Die Anweisungsblöcken in mehrzeiligen Lambda-Ausdrücke gelten die folgenden Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3264">The statement blocks inside multi-line lambda expressions have the following restrictions:</span></span>

* <span data-ttu-id="61fcb-3265">`On Error` und `Resume` Anweisungen sind nicht zulässig, obwohl `Try` -Anweisungen sind zulässig.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3265">`On Error` and `Resume` statements are not allowed, although `Try` statements are allowed.</span></span>

* <span data-ttu-id="61fcb-3266">Statische lokale Variablen können nicht in mehrzeiligen Lambda-Ausdrücken deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3266">Static locals cannot be declared in multi-line lambda expressions.</span></span>

* <span data-ttu-id="61fcb-3267">Kann nicht in oder aus der Anweisungsblock eines mehrzeiligen Lambda-Ausdrucks, verzweigt, obwohl die normalen Verzweigungen Regeln darin beziehen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3267">It is not possible to branch into or out of the statement block of a multi-line lambda expression, although the normal branching rules apply within it.</span></span> <span data-ttu-id="61fcb-3268">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3268">For example:</span></span>

  ```vb
  Label1:
  Dim x = Sub()
                 ' Error: Cannot branch out
                 GoTo Label1

                 ' OK: Wholly within the lamba.
                 GoTo Label2:
            Label2:
            End Sub

  ' Error: Cannot branch in
  GoTo Label2
  ```

<span data-ttu-id="61fcb-3269">Ein Lambda-Ausdruck entspricht etwa einer anonymen Methode, die für den enthaltenden Typ deklariert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3269">A lambda expression is roughly equivalent to an anonymous method declared on the containing type.</span></span> <span data-ttu-id="61fcb-3270">Im ersten Beispiel ist ungefähr gleich:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3270">The initial example is roughly equivalent to:</span></span>

```vb
Module Test
    Delegate Function IntFunc(x As Integer) As Integer

    Sub Apply(a() As Integer, func As IntFunc)
        For index As Integer = 0 To a.Length - 1
            a(index) = func(a(index))
        Next index
    End Sub

    Function $Lambda1(x As Integer) As Integer
        Return x * 2
    End Function

    Sub Main()
        Dim a() As Integer = { 1, 2, 3, 4 }

        Apply(a, AddressOf $Lambda1)

        For Each value In a
            Console.Write(value & " ")
        Next value
    End Sub
End Module
```


### <a name="closures"></a><span data-ttu-id="61fcb-3271">Closures</span><span class="sxs-lookup"><span data-stu-id="61fcb-3271">Closures</span></span>

<span data-ttu-id="61fcb-3272">Lambda-Ausdrücke haben Zugriff auf alle Variablen im Bereich, einschließlich lokale Variablen oder Parameter, die bei der enthaltenden Methode und Lambda-Ausdrücken definiert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3272">Lambda expressions have access to all of the variables in scope, including local variables or parameters defined in the containing method and lambda expressions.</span></span> <span data-ttu-id="61fcb-3273">Wenn ein Lambda-Ausdruck auf eine lokale Variable oder Parameter verwiesen wird, erfasst der Lambda-Ausdruck die Variable in einen Closure bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3273">When a lambda expression refers to a local variable or parameter, the lambda expression captures the variable being referred to into a closure.</span></span> <span data-ttu-id="61fcb-3274">Als Closure ist ein Objekt, das auf dem Heap anstelle von auf dem Stapel befindet, und wenn eine Variable erfasst sind, werden alle Verweise auf die Variable auf den Abschluss umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3274">A closure is an object that lives on the heap instead of on the stack, and when a variable is captured, all references to the variable are redirected to the closure.</span></span> <span data-ttu-id="61fcb-3275">Dadurch können Lambda-Ausdrücke, um den Vorgang fortzusetzen, um auf lokale Variablen und Parameter verweisen, auch nach die enthaltende Methode abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3275">This enables lambda expressions to continue to refer to local variables and parameters even after the containing method is complete.</span></span> <span data-ttu-id="61fcb-3276">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3276">For example:</span></span>

```vb
Module Test
    Delegate Function D() As Integer

    Function M() As D
        Dim x As Integer = 10
        Return Function() x
    End Function

    Sub Main()
        Dim y As D = M()

        ' Prints 10
        Console.WriteLine(y())
    End Sub
End Module
```

<span data-ttu-id="61fcb-3277">ist ungefähr gleich ist:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3277">is roughly equivalent to:</span></span>

```vb
Module Test
    Delegate Function D() As Integer

    Class $Closure1
        Public x As Integer

        Function $Lambda1() As Integer
            Return x
        End Function
    End Class

    Function M() As D
        Dim c As New $Closure1()
        c.x = 10
        Return AddressOf c.$Lambda1
    End Function

    Sub Main()
        Dim y As D = M()

        ' Prints 10
        Console.WriteLine(y())
    End Sub
End Module
```

<span data-ttu-id="61fcb-3278">Als Closure erfasst eine neue Kopie von einer lokalen Variable jedes Mal, es den Block in dem gibt, die lokale Variable wird deklariert, aber die neue Kopie wird mit dem Wert der vorherigen Kopie, initialisiert, sofern vorhanden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3278">A closure captures a new copy of a local variable each time it enters the block in which the local variable is declared, but the new copy is initialized with the value of the previous copy, if any.</span></span> <span data-ttu-id="61fcb-3279">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3279">For example:</span></span>

```vb
Module Test
    Delegate Function D() As Integer

    Function M() As D()
        Dim a(9) As D

        For i As Integer = 0 To 9
            Dim x
            a(i) = Function() x
            x += 1
        Next i

        Return a
    End Function

    Sub Main()
        Dim y() As D = M()

        For i As Integer = 0 To 9
            Console.Write(y(i)() & " ")
        Next i
    End Sub
End Module
```

<span data-ttu-id="61fcb-3280">Druckt</span><span class="sxs-lookup"><span data-stu-id="61fcb-3280">prints</span></span>

```
1 2 3 4 5 6 7 8 9 10
```

<span data-ttu-id="61fcb-3281">Statt</span><span class="sxs-lookup"><span data-stu-id="61fcb-3281">instead of</span></span>

```
9 9 9 9 9 9 9 9 9 9
```

<span data-ttu-id="61fcb-3282">Da Closures initialisiert werden, wenn Sie einen Block eingeben müssen, es ist nicht zulässig, `GoTo` in einen Block mit einer Schließung von außerhalb von diesem Block zwar erlaubt sind `Resume` in einen Block mit einer Closure.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3282">Because closures have to be initialized when entering a block, it is not allowed to `GoTo` into a block with a closure from outside of that block, although it is allowed to `Resume` into a block with a closure.</span></span> <span data-ttu-id="61fcb-3283">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3283">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim a = 10

        If a = 10 Then
L1:
            Dim x = Function() a

            ' Valid, source is within block
            GoTo L2
L2:
        End If

        ' ERROR: target is inside block with closure
        GoTo L1
    End Sub
End Module
```

<span data-ttu-id="61fcb-3284">Da sie nicht in einen Closure erfasst werden können, kann nicht innerhalb eines Lambda-Ausdrucks Folgendes angezeigt:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3284">Because they cannot be captured into a closure, the following cannot appear inside of a lambda expression:</span></span>

* <span data-ttu-id="61fcb-3285">Reference-Parameter.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3285">Reference parameters.</span></span>

* <span data-ttu-id="61fcb-3286">Instanz von Ausdrücken (`Me`, `MyClass`, `MyBase`), wenn der Typ des `Me` ist keine Klasse.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3286">Instance expressions (`Me`, `MyClass`, `MyBase`), if the type of `Me` is not a class.</span></span>

<span data-ttu-id="61fcb-3287">Die Member eines anonymen typerstellung-Ausdrucks, wenn der Lambda-Ausdruck Teil des Ausdrucks ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3287">The members of an anonymous type-creation expression, if the lambda expression is part of the expression.</span></span> <span data-ttu-id="61fcb-3288">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3288">For example:</span></span>

```vb
' Error: Lambda cannot refer to anonymous type field
Dim x = New With { .a = 12, .b = Function() .a }
```

<span data-ttu-id="61fcb-3289">`ReadOnly` Instanzvariablen in Instanzkonstruktoren oder `ReadOnly` freigegebene Variablen im shared-Konstruktoren, in denen die Variablen in einem Kontext ohne Wert verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3289">`ReadOnly` instance variables in instance constructors or `ReadOnly` shared variables in shared constructors where the variables are used in a non-value context.</span></span> <span data-ttu-id="61fcb-3290">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3290">For example:</span></span>

```vb
Class C1
    ReadOnly F1 As Integer

    Sub New()
        ' Valid, doesn't modify F1
        Dim x = Function() F1

        ' Error, tries to modify F1
        Dim f = Function() ModifyValue(F1)
    End Sub

    Sub ModifyValue(ByRef x As Integer)
    End Sub
End Class
```

## <a name="query-expressions"></a><span data-ttu-id="61fcb-3291">Abfrageausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-3291">Query Expressions</span></span>

<span data-ttu-id="61fcb-3292">Ein *Abfrageausdruck* ist ein Ausdruck, der eine Reihe von gilt *Abfrageoperatoren* auf die Elemente einer *abgefragt werden* Auflistung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3292">A *query expression* is an expression that applies a series of *query operators* to the elements of a *queryable* collection.</span></span> <span data-ttu-id="61fcb-3293">Der folgende Ausdruck wird beispielsweise eine Auflistung von `Customer` -Objekte und gibt die Namen aller Kunden in den Bundesstaat Washington zurück:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3293">For example, the following expression takes a collection of `Customer` objects and returns the names of all the customers in the state of Washington:</span></span>

```vb
Dim names = _
    From cust In Customers _
    Where cust.State = "WA" _
    Select cust.Name
```

<span data-ttu-id="61fcb-3294">Ein Abfrageausdruck muss mit beginnen eine `From` oder `Aggregate` Operator und kann mit jeder Abfrage-Operator enden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3294">A query expression must start with a `From` or an `Aggregate` operator and can end with any query operator.</span></span> <span data-ttu-id="61fcb-3295">Das Ergebnis eines Abfrageausdrucks wird als Wert klassifiziert. der Ergebnistyp des Ausdrucks hängt von der Ergebnistyp des letzten Abfrageoperator im Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3295">The result of a query expression is classified as a value; the result type of the expression depends on the result type of the last query operator in the expression.</span></span>

```antlr
QueryExpression
    : FromOrAggregateQueryOperator QueryOperator*
    ;

FromOrAggregateQueryOperator
    : FromQueryOperator
    | AggregateQueryOperator
    ;

QueryOperator
    : FromQueryOperator
    | AggregateQueryOperator
    | SelectQueryOperator
    | DistinctQueryOperator
    | WhereQueryOperator
    | OrderByQueryOperator
    | PartitionQueryOperator
    | LetQueryOperator
    | GroupByQueryOperator
    | JoinOrGroupJoinQueryOperator
    ;

JoinOrGroupJoinQueryOperator
    : JoinQueryOperator
    | GroupJoinQueryOperator
    ;
```

### <a name="range-variables"></a><span data-ttu-id="61fcb-3296">Bereichsvariablen</span><span class="sxs-lookup"><span data-stu-id="61fcb-3296">Range Variables</span></span>

<span data-ttu-id="61fcb-3297">Einige Abfrageoperatoren einer besonderen Art von Variable mit dem Namen einer *Bereichsvariable*.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3297">Some query operators introduce a special kind of variable called a *range variable*.</span></span> <span data-ttu-id="61fcb-3298">Bereichsvariablen sind nicht die echten Variablen. Stattdessen stellen sie die einzelnen Werte während der Auswertung der Abfrage über die eingabeauflistungen dar.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3298">Range variables are not real variables; instead, they represent the individual values during the evaluation of the query over the input collections.</span></span>

```antlr
CollectionRangeVariableDeclarationList
    : CollectionRangeVariableDeclaration ( Comma CollectionRangeVariableDeclaration )*
    ;

CollectionRangeVariableDeclaration
    : Identifier ( 'As' TypeName )? 'In' LineTerminator? Expression
    ;

ExpressionRangeVariableDeclarationList
    : ExpressionRangeVariableDeclaration ( Comma ExpressionRangeVariableDeclaration )*
    ;

ExpressionRangeVariableDeclaration
    : Identifier ( 'As' TypeName )? Equals Expression
    ;
```

<span data-ttu-id="61fcb-3299">Bereichsvariablen beziehen sich vom Einleitungsoperator Abfrage an das Ende eines Abfrageausdrucks oder auf einen Abfrageoperator wie z. B. `Select` , die blendet diese aus.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3299">Range variables are scoped from the introducing query operator to the end of a query expression, or to a query operator such as `Select` that hides them.</span></span> <span data-ttu-id="61fcb-3300">Z. B. in der folgenden Abfrage</span><span class="sxs-lookup"><span data-stu-id="61fcb-3300">For example, in the following query</span></span>

```vb
Dim waCusts = _
    From cust As Customer In Customers _
    Where cust.State = "WA"
```

<span data-ttu-id="61fcb-3301">die `From` Abfrage-Operator führt eine Bereichsvariable `cust` als `Customer` , darstellt, dass alle Kunden in der `Customers` Auflistung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3301">the `From` query operator introduces a range variable `cust` typed as `Customer` that represents each customer in the `Customers` collection.</span></span> <span data-ttu-id="61fcb-3302">Die folgenden `Where` -Abfrage-Operator bezieht sich dann auf die Bereichsvariable `cust` im Filterausdruck zu bestimmen, ob Sie einen einzelnen Kunden aus der resultierenden Auflistung zu filtern.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3302">The following `Where` query operator then refers to the range variable `cust` in the filter expression to determine whether to filter an individual customer out of the resulting collection.</span></span>

<span data-ttu-id="61fcb-3303">Es gibt zwei Arten der Bereichsvariablen: *Bereich sammlungsvariablen* und *Ausdruck Bereichsvariablen*.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3303">There are two types of range variables: *collection range variables* and *expression range variables*.</span></span> <span data-ttu-id="61fcb-3304">Sammlung von Bereichsvariablen dauern, deren Werte aus den Elementen der Sammlungen, die abgefragt wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3304">Collection range variables take their values from the elements of the collections being queried.</span></span> <span data-ttu-id="61fcb-3305">Des sammlungsausdrucks im Auflistung Deklaration einer Bereichsvariablen muss als Wert klassifiziert werden, dessen Typ abgefragt werden wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3305">The collection expression in a collection range variable declaration must be classified as a value whose type is queryable.</span></span> <span data-ttu-id="61fcb-3306">Wenn der Typ einer Bereichsvariablen für die Sammlung weggelassen wird, wird abgeleitet, die den Elementtyp des sammlungsausdrucks sein oder `Object` des sammlungsausdrucks keinen Elementtyp (definiert z. B. nur eine `Cast` Methode).</span><span class="sxs-lookup"><span data-stu-id="61fcb-3306">If the type of a collection range variable is omitted, it is inferred to be the element type of the collection expression, or `Object` if the collection expression does not have an element type (i.e. only defines a `Cast` method).</span></span> <span data-ttu-id="61fcb-3307">Wenn die Auflistungsausdruck nicht abgefragt werden (d. h. der Elementtyp der Sammlung kann nicht abgeleitet werden), einem Fehler während der Kompilierung führt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3307">If the collection expression is not queryable (i.e. the element type of the collection cannot be inferred), a compile-time error results.</span></span>

<span data-ttu-id="61fcb-3308">Eine Bereichsvariable Ausdruck ist eine Bereichsvariable, die von einer Sammlung, sondern ein Ausdruck, dessen Wert berechnet wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3308">An expression range variable is a range variable whose value is calculated by an expression rather than a collection.</span></span> <span data-ttu-id="61fcb-3309">Im folgenden Beispiel die `Select` Abfrage-Operator führt eine Ausdruck Range-Variable, die mit dem Namen `cityState` aus zwei Felder berechnet:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3309">In the following example, the `Select` query operator introduces an expression range variable named `cityState` calculated from two fields:</span></span>

```vb
Dim cityStates = _
    From cust As Customer In Customers _
    Select cityState = cust.City & "," & cust.State _
    Where cityState.Length() < 10
```

<span data-ttu-id="61fcb-3310">Eine Bereichsvariable Ausdruck ist nicht auf einem anderen Range-Variable, erforderlich, obwohl eine solche Variable fragwürdiger sein kann.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3310">An expression range variable is not required to reference another range variable, although such a variable may be of dubious value.</span></span> <span data-ttu-id="61fcb-3311">Der Ausdruck zugewiesen werden, auf die Bereichsvariable ein Ausdruck muss als Wert klassifiziert werden und muss implizit in den Typ der Bereichsvariablen, wenn angegeben.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3311">The expression assigned to an expression range variable must be classified as a value and must be implicitly convertible to the type of the range variable, if given.</span></span>

<span data-ttu-id="61fcb-3312">Nur in einer Let-Operator möglicherweise eine Bereichsvariable für den Ausdruck, den angegebenen Typ.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3312">Only in a Let operator may an expression range variable have its type specified.</span></span> <span data-ttu-id="61fcb-3313">In anderen Operatoren oder wenn der Typ ist nicht angegeben, lokale Variablen Typrückschluss wird verwendet, um den Typ der Bereichsvariablen zu bestimmen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3313">In other operators, or if its type is not specified, then local variable type inference is used to determine the type of the range variable.</span></span>

<span data-ttu-id="61fcb-3314">Eine Bereichsvariable muss den Regeln zum Deklarieren von lokaler Variablen in Bezug auf das shadowing folgen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3314">A range variable must follow the rules for declaring local variables in respect to shadowing.</span></span> <span data-ttu-id="61fcb-3315">Eine Bereichsvariable daher ausblenden nicht den Namen der lokalen Variable oder Parameter in die einschließende Methode oder einem anderen Bereichsvariable, (es sei denn, der Abfrage-Operator insbesondere alle aktuellen Bereichsvariablen im Bereich ausgeblendet).</span><span class="sxs-lookup"><span data-stu-id="61fcb-3315">Thus, a range variable cannot hide the name of a local variable or parameter in the enclosing method or another range variable (unless the query operator specifically hides all current range variables in scope).</span></span>


### <a name="queryable-types"></a><span data-ttu-id="61fcb-3316">Abfragbare Typen</span><span class="sxs-lookup"><span data-stu-id="61fcb-3316">Queryable Types</span></span>

<span data-ttu-id="61fcb-3317">Abfrageausdrücke werden implementiert, durch die Übersetzung des Ausdrucks in Aufrufe an bekannten auf einen Auflistungstyp.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3317">Query expressions are implemented by translating the expression into calls to well-known methods on a collection type.</span></span> <span data-ttu-id="61fcb-3318">Diese klar definierten Methoden definiert den Typ des Elements der Auflistung abgefragt werden, sowie die Ergebnistypen der Abfrageoperatoren, die in der Auflistung ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3318">These well-defined methods define the element type of the queryable collection as well as the result types of query operators executed on the collection.</span></span> <span data-ttu-id="61fcb-3319">Jede Abfrage-Operator gibt die Methode oder die Methoden, denen der Abfrage-Operator in der Regel übersetzt wird, auch wenn der bestimmte Übersetzung hängt von der Implementierung ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3319">Each query operator specifies the method or methods that the query operator is generally translated into, although the specific translation is implementation dependent.</span></span> <span data-ttu-id="61fcb-3320">Die Methoden werden in der Spezifikation, die mit einem allgemeinen Format, das aussieht wie angegeben:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3320">The methods are given in the specification using a general format that looks like:</span></span>

```vb
Function Select(selector As Func(Of T, R)) As CR
```

<span data-ttu-id="61fcb-3321">Die Methoden gilt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3321">The following applies to the methods:</span></span>

* <span data-ttu-id="61fcb-3322">Die Methode muss eine Instanz oder Erweiterungsmember des Auflistungstyps sein und muss zugänglich sein.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3322">The method must be an instance or extension member of the collection type and must be accessible.</span></span>

* <span data-ttu-id="61fcb-3323">Die Methode generisch ist, möglicherweise, bereitgestellt, die möglich, alle Typargumente abzuleiten.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3323">The method may be generic, provided that is possible to infer all type arguments.</span></span>

* <span data-ttu-id="61fcb-3324">Die Methode möglicherweise überladen werden, in dem Fall Auflösung von funktionsüberladungen verwendet wird, um zu bestimmen, die genau zu verwendende Methode.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3324">The method may be overloaded, in which case overload resolution is used to determine the exactly method to use.</span></span>

* <span data-ttu-id="61fcb-3325">Einen anderen Delegattyp kann verwendet werden, anstelle der Delegat `Func` eingeben, vorausgesetzt, dass die gleiche Signatur hat, einschließlich der Rückgabetyp, wie die entsprechenden `Func` Typ.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3325">Another delegate type may be used in place of the delegate `Func` type, provided that it has the same signature, including return type, as the matching `Func` type.</span></span>

* <span data-ttu-id="61fcb-3326">Der Typ `System.Linq.Expressions.Expression(Of D)` kann verwendet werden, anstelle der Delegat `Func` Typ bereitgestellt, die `D` ist ein Delegattyp, der die gleiche Signatur, einschließlich der Rückgabetyp, wie die entsprechenden hat `Func` Typ.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3326">The type `System.Linq.Expressions.Expression(Of D)` may be used in place of the delegate `Func` type, provided that `D` is a delegate type that has the same signature, including return type, as the matching `Func` type.</span></span>

* <span data-ttu-id="61fcb-3327">Der Typ `T` den Elementtyp der eingabeauflistung darstellt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3327">The type `T` represents the element type of the input collection.</span></span> <span data-ttu-id="61fcb-3328">Alle von einem Auflistungstyp definierten Methoden müssen den gleichen input-Element-Typ für den Auflistungstyp abgefragt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3328">All of the methods defined by a collection type must have the same input element type for the collection type to be queryable.</span></span>

* <span data-ttu-id="61fcb-3329">Der Typ `S` stellt den Elementtyp der zweiten Eingabe Auflistung im Fall von Abfrageoperatoren, die Joins auszuführen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3329">The type `S` represents the element type of the second input collection in the case of query operators that perform joins.</span></span>

* <span data-ttu-id="61fcb-3330">Der Typ `K` stellt einen Schlüsseltyp im Fall von Abfrageoperatoren, die einen Satz von Bereichsvariablen, die als Schlüssel verwendet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3330">The type `K` represents a key type in the case of query operators that have a set of range variables that act as keys.</span></span>

* <span data-ttu-id="61fcb-3331">Der Typ `N` stellt einen Typ, der einen numerischen Typ verwendet wird (obwohl sie weiterhin einen benutzerdefinierten Typ und nicht mit einem systeminternen numerischen Typ sein kann).</span><span class="sxs-lookup"><span data-stu-id="61fcb-3331">The type `N` represents a type that is used as a numeric type (although it could still be a user-defined type and not an intrinsic numeric type).</span></span>

* <span data-ttu-id="61fcb-3332">Der Typ `B` stellt einen Typ, der in einen booleschen Ausdruck verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3332">The type `B` represents a type that can be used in a Boolean expression.</span></span>

* <span data-ttu-id="61fcb-3333">Der Typ `R` den Elementtyp der Ergebnissammlung, darstellt, wenn die Abfrage-Operator eine ergebnisauflistung erzeugt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3333">The type `R` represents the element type of the result collection, if the query operator produces a result collection.</span></span> <span data-ttu-id="61fcb-3334">`R` hängt von der Anzahl der Bereichsvariablen im Bereich am Ende der Abfrage-Operator.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3334">`R` depends on the number of range variables in scope at the conclusion of the query operator.</span></span> <span data-ttu-id="61fcb-3335">Wenn eine Variable für die einzelnen Bereich im Bereich ist `R` ist der Typ von dieser Bereichsvariablen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3335">If a single range variable is in scope, then `R` is the type of that range variable.</span></span> <span data-ttu-id="61fcb-3336">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="61fcb-3336">In the example</span></span>

  ```vb
  Dim custNames = From c In Customers
                  Select c.Name
  ```

  <span data-ttu-id="61fcb-3337">Das Ergebnis der Abfrage werden mit einem Element vom Auflistungstyp `String`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3337">the result of the query will be a collection type with an element type of `String`.</span></span> <span data-ttu-id="61fcb-3338">Wenn mehrere Bereichsvariablen im Bereich, dann ist `R` ist ein anonymer Typ, der alle von der Bereichsvariablen im Bereich wie enthält `Key` Felder.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3338">If multiple range variables are in scope, then `R` is an anonymous type that contains all of the range variables in scope as `Key` fields.</span></span> <span data-ttu-id="61fcb-3339">Im folgenden Beispiel</span><span class="sxs-lookup"><span data-stu-id="61fcb-3339">In the example:</span></span>

  ```vb
  Dim custNames = From c In Customers, o In c.Orders 
                  Select Name = c.Name, ProductName = o.ProductName
  ```

  <span data-ttu-id="61fcb-3340">Das Ergebnis der Abfrage werden mit einem Elementtyp eines anonymen Typs mit einer nur-Lese Eigenschaft, die mit dem Namen vom Auflistungstyp `Name` des Typs `String` und eine schreibgeschützte Eigenschaft, die mit dem Namen `ProductName` des Typs `String`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3340">the result of the query will be a collection type with an element type of an anonymous type with a read-only property named `Name` of type `String` and a read-only property named `ProductName` of type `String`.</span></span>

  <span data-ttu-id="61fcb-3341">In einem Abfrageausdruck, anonyme Typen generiert, um die Bereichsvariablen enthalten sind *transparent*, d. h., die Variablen liegen stehen immer zur Verfügung, ohne Qualifizierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3341">Within a query expression, anonymous types generated to contain range variables are *transparent*, which means that range variables are always available without qualification.</span></span> <span data-ttu-id="61fcb-3342">Im vorherigen Beispiel z. B. die Bereichsvariablen `c` und `o` zugegriffen werden kann, ohne Qualifikation in die `Select` -Abfrage-Operator, obwohl der eingabeauflistung Elementtyp eines anonymen Typs wurde.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3342">For example, in the previous example the range variables `c` and `o` could be accessed without qualification in the `Select` query operator, even though the input collection's element type was an anonymous type.</span></span>

* <span data-ttu-id="61fcb-3343">Der Typ `CX` stellt einen Auflistungstyp, nicht unbedingt der Eingabesammlung, dessen Elementtyp eine Art ist `X`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3343">The type `CX` represents a collection type, not necessarily the input collection type, whose element type is some type `X`.</span></span>

<span data-ttu-id="61fcb-3344">Ein abfragbare Auflistung-Typ muss eine der folgenden Bedingungen, in der Reihenfolge ihrer Priorität erfüllen:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3344">A queryable collection type must satisfy one of the following conditions, in order of preference:</span></span>

* <span data-ttu-id="61fcb-3345">Müssen sie definieren, eine Übereinstimmung `Select` Methode.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3345">It must define a conforming `Select` method.</span></span>

* <span data-ttu-id="61fcb-3346">Sie haben benötigen einen der folgenden Methoden</span><span class="sxs-lookup"><span data-stu-id="61fcb-3346">It must have have one of the following methods</span></span>

  ```vb
  Function AsEnumerable() As CT
  Function AsQueryable() As CT
  ```

  <span data-ttu-id="61fcb-3347">Das kann zum Abrufen einer abfragbaren Auflistung aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3347">which can be called to obtain a queryable collection.</span></span> <span data-ttu-id="61fcb-3348">Wenn beide Methoden bereitgestellt werden, `AsQueryable` vorzuziehen ist `AsEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3348">If both methods are provided, `AsQueryable` is preferred over `AsEnumerable`.</span></span>

* <span data-ttu-id="61fcb-3349">Es muss eine Methode verfügen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3349">It must have a method</span></span>

  ```vb
  Function Cast(Of T)() As CT
  ```

  <span data-ttu-id="61fcb-3350">die mit dem Typ der Bereichsvariablen zum Erzeugen einer abfragbaren Auflistung aufgerufen werden kann.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3350">which can be called with the type of the range variable to produce a queryable collection.</span></span>

<span data-ttu-id="61fcb-3351">Da bestimmen den Typ des Elements einer Auflistung unabhängig von der eine tatsächliche Methodenaufruf auftritt, kann die Anwendbarkeit der spezifischen Methoden bestimmt werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3351">Because determining the element type of a collection occurs independently of an actual method invocation, the applicability of specific methods cannot be determined.</span></span> <span data-ttu-id="61fcb-3352">Daher werden beim Ermitteln den Typ des Elements einer Auflistung an, ob es sind Instanzmethoden, die bekannte Methoden entsprechen, Erweiterungsmethoden, die bekannte Methoden entsprechen dann ignoriert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3352">Thus, when determining the element type of a collection if there are instance methods that match well-known methods, then any extension methods that match well-known methods are ignored.</span></span>

<span data-ttu-id="61fcb-3353">Übersetzung von Standardabfrageoperatoren tritt auf, in der Reihenfolge, in der die Abfrageoperatoren im Ausdruck auftreten.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3353">Query operator translation occurs in the order in which the query operators occur in the expression.</span></span> <span data-ttu-id="61fcb-3354">Es ist nicht erforderlich für ein Auflistungsobjekt, alle Methoden, die von allen die Abfrageoperatoren, benötigt implementieren, obwohl jedes Objekt mindestens unterstützen muss die `Select` -Abfrage-Operator.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3354">It is not necessary for a collection object to implement all of the methods needed by all the query operators, although every collection object must at least support the `Select` query operator.</span></span> <span data-ttu-id="61fcb-3355">Wenn eine erforderliche Methode nicht vorhanden ist, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3355">If a needed method is not present, a compile-time error occurs.</span></span> <span data-ttu-id="61fcb-3356">Beim Binden von bekannten Namen werden nicht-Methoden für mehrfache Vererbung in Schnittstellen und Erweiterungsmethode, die Bindung ignoriert, obwohl shadowing Semantik weiterhin gelten.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3356">When binding well-known method names, non-methods are ignored for the purpose of multiple inheritance in interfaces and extension method binding, although shadowing semantics still apply.</span></span> <span data-ttu-id="61fcb-3357">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3357">For example:</span></span>

```vb
Class Q1
    Public Function [Select](selector As Func(Of Integer, Integer)) As Q1
    End Function
End Class

Class Q2
    Inherits Q1

    Public [Select] As Integer
End Class

Module Test
    Sub Main()
        Dim qs As New Q2()

        ' Error: Q2.Select still hides Q1.Select
        Dim zs = From q In qs Select q
    End Sub
End Module
```

### <a name="default-query-indexer"></a><span data-ttu-id="61fcb-3358">Standardindexer für die Abfrage</span><span class="sxs-lookup"><span data-stu-id="61fcb-3358">Default Query Indexer</span></span>

<span data-ttu-id="61fcb-3359">Jede abfragbare Auflistung-Typ, dessen Elementtyp `T` und noch keinen Standardwert Eigenschaft gilt eine Standardeigenschaft, die folgende allgemeine Form aufweisen:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3359">Every queryable collection type whose element type is `T` and does not already have a default property is considered to have a default property of the following general form:</span></span>

```vb
Public ReadOnly Default Property Item(index As Integer) As T
    Get
        Return Me.ElementAtOrDefault(index)
    End Get
End Property
```

<span data-ttu-id="61fcb-3360">Die Default-Eigenschaft kann nur mit der Syntax der Standardeigenschaft Zugriff verwiesen werden; Die Standardeigenschaft kann nicht namentlich verwiesen werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3360">The default property can only be referred to using the default property access syntax; the default property cannot be referred to by name.</span></span> <span data-ttu-id="61fcb-3361">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3361">For example:</span></span>

```vb
Dim customers As IEnumerable(Of Customer) = ...
Dim customerThree = customers(2)

' Error, no such property
Dim customerFour = customers.Item(4)
```

<span data-ttu-id="61fcb-3362">Wenn der Auflistungstyp kein `ElementAtOrDefault` Element ein Fehler während der Kompilierung erfolgt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3362">If the collection type does not have an `ElementAtOrDefault` member, a compile-time error will occur.</span></span>

### <a name="from-query-operator"></a><span data-ttu-id="61fcb-3363">Von Abfrage-Operator</span><span class="sxs-lookup"><span data-stu-id="61fcb-3363">From Query Operator</span></span>

<span data-ttu-id="61fcb-3364">Die `From` Abfrage-Operator stellt eine Auflistung Bereichsvariable, die die einzelnen Mitglieder einer Sammlung, die abgefragt werden darstellt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3364">The `From` query operator introduces a collection range variable that represents the individual members of a collection to be queried.</span></span>

```antlr
FromQueryOperator
    : LineTerminator? 'From' LineTerminator? CollectionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="61fcb-3365">Um beispielsweise den Abfrageausdruck:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3365">For example, the query expression:</span></span>

```vb
From c As Customer In Customers ...
```

<span data-ttu-id="61fcb-3366">kann als gleich betrachtet werden</span><span class="sxs-lookup"><span data-stu-id="61fcb-3366">can be thought of as equivalent to</span></span>

```vb
For Each c As Customer In Customers
        ...
Next c
```

<span data-ttu-id="61fcb-3367">Wenn eine `From` -Abfrage-Operator deklariert mehrere Auflistung Bereichsvariablen oder ist nicht der erste `From` im Abfrageausdruck-Abfrage-Operator, jedes neue Bereichsvariable für die Sammlung ist *Cross verknüpft* auf den vorhandenen Satz von Bereichsvariablen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3367">When a `From` query operator declares multiple collection range variables or is not the first `From` query operator in the query expression, each new collection range variable is *cross joined* to the existing set of range variables.</span></span> <span data-ttu-id="61fcb-3368">Das Ergebnis ist, dass die Abfrage über das Kreuzprodukt aller Elemente in den verknüpften Sammlungen ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3368">The result is that the query is evaluated over the cross-product of all the elements in the joined collections.</span></span> <span data-ttu-id="61fcb-3369">Beispielsweise der Ausdruck:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3369">For example, the expression:</span></span>

```vb
From c In Customers _
From e In Employees _
...
```

<span data-ttu-id="61fcb-3370">kann als gleich betrachtet werden:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3370">can be thought of as equivalent to:</span></span>

```vb
For Each c In Customers
    For Each e In Employees
            ...
    Next e
Next c
```

<span data-ttu-id="61fcb-3371">und genau gleich ist:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3371">and is exactly equivalent to:</span></span>

```vb
From c In Customers, e In Employees ...
```

<span data-ttu-id="61fcb-3372">Die Bereichsvariablen eingeführt, die in vorherigen Abfrageoperatoren können verwendet werden, in einem späteren `From` -Abfrage-Operator.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3372">The range variables introduced in previous query operators can be used in a later `From` query operator.</span></span> <span data-ttu-id="61fcb-3373">In der folgende Abfrageausdruck beispielsweise das zweite `From` -Abfrage-Operator bezieht sich auf den Wert der ersten Bereichsvariablen:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3373">For example, in the following query expression the second `From` query operator refers to the value of the first range variable:</span></span>

```vb
From c As Customer In Customers _
From o As Order In c.Orders _
Select c.Name, o
```

<span data-ttu-id="61fcb-3374">Mehrere Bereichsvariablen in einem `From` Operator oder mehrere Abfragen `From` Abfrageoperatoren werden nur unterstützt, wenn der Typ der Auflistung, eine oder beide der folgenden Methoden enthält:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3374">Multiple range variables in a `From` query operator or multiple `From` query operators are only supported if the collection type contains one or both of the following methods:</span></span>

```vb
Function SelectMany(selector As Func(Of T, CR)) As CR
Function SelectMany(selector As Func(Of T, CS), _
                          resultsSelector As Func(Of T, S, R)) As CR
```

<span data-ttu-id="61fcb-3375">Der code</span><span class="sxs-lookup"><span data-stu-id="61fcb-3375">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs, y In ys ...
```

<span data-ttu-id="61fcb-3376">wird in der Regel in übersetzt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3376">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.SelectMany( _
        Function(x As Integer) ys, _
        Function(x As Integer, y As Integer) New With {x, y})...
```

<span data-ttu-id="61fcb-3377">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3377">__Note.__</span></span> <span data-ttu-id="61fcb-3378">`From` ist ein reserviertes Wort.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3378">`From` is not a reserved word.</span></span>


### <a name="join-query-operator"></a><span data-ttu-id="61fcb-3379">JOIN-Abfrage-Operator</span><span class="sxs-lookup"><span data-stu-id="61fcb-3379">Join Query Operator</span></span>

<span data-ttu-id="61fcb-3380">Die `Join` Abfrage-Operator verknüpft vorhandenen Bereichsvariablen für eine neue Auflistung Bereichsvariable, erzeugt eine einzelne Sammlung, deren Elemente miteinander verknüpft wurde, basierend auf einem gleichheitsausdruck auf.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3380">The `Join` query operator joins existing range variables with a new collection range variable, producing a single collection whose elements have been joined together based on an equality expression.</span></span>

```antlr
JoinQueryOperator
    : LineTerminator? 'Join' LineTerminator? CollectionRangeVariableDeclaration
      JoinOrGroupJoinQueryOperator? LineTerminator? 'On' LineTerminator? JoinConditionList
    ;

JoinConditionList
    : JoinCondition ( 'And' LineTerminator? JoinCondition )*
    ;

JoinCondition
    : Expression 'Equals' LineTerminator? Expression
    ;
```

<span data-ttu-id="61fcb-3381">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3381">For example:</span></span>

```vb
Dim customersAndOrders = _
    From cust In Customers _
    Join ord In Orders On cust.ID Equals ord.CustomerID
```

<span data-ttu-id="61fcb-3382">Der gleichheitsausdruck auf ist stärker eingeschränkt als eine reguläre gleichheitsausdruck:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3382">The equality expression is more restricted than a regular equality expression:</span></span>

* <span data-ttu-id="61fcb-3383">Beide Ausdrücke müssen als Wert klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3383">Both expressions must be classified as a value.</span></span>

* <span data-ttu-id="61fcb-3384">Beide Ausdrücke müssen mindestens eine Bereichsvariable verweisen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3384">Both expressions must reference at least one range variable.</span></span>

* <span data-ttu-id="61fcb-3385">Die Bereichsvariable, deklariert im Join-Abfrage-Operator von einem der Ausdrücke verwiesen werden muss und dass der Ausdruck keine anderen Bereichsvariablen verweisen, muss.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3385">The range variable declared in the join query operator must be referenced by one of the expressions, and that expression must not reference any other range variables.</span></span>

<span data-ttu-id="61fcb-3386">Wenn die Typen der beiden Ausdrücke nicht exakt denselben Typ haben, klicken Sie dann</span><span class="sxs-lookup"><span data-stu-id="61fcb-3386">If the types of the two expressions are not the exact same type, then</span></span>

* <span data-ttu-id="61fcb-3387">Wenn der Gleichheitsoperator ist für die beiden Typen definiert, beide Ausdrücke implizit konvertierbar in es sind und es nicht ist `Object`, beide Ausdrücke dann in diesen Typ zu konvertieren.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3387">If the equality operator is defined for the two types, both expressions are implicitly convertible to it, and it is not `Object`, then convert both expressions to that type.</span></span>

* <span data-ttu-id="61fcb-3388">Andernfalls liegt ein bestimmende Typ, dem beide Ausdrücke in implizit konvertiert werden können, klicken Sie dann konvertieren Sie beide Ausdrücke auf diesen Typ.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3388">Otherwise, if there is a dominant type that both expressions can be implicitly converted to, then convert both expressions to that type.</span></span>

* <span data-ttu-id="61fcb-3389">Andernfalls tritt ein Kompilierungsfehler auf.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3389">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="61fcb-3390">Die Ausdrücke werden mithilfe von Hashwerten verglichen (z. B. durch Aufrufen von `GetHashCode()`), nicht für Effizienz mithilfe von Gleichheitsoperatoren.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3390">The expressions are compared using hash values (i.e. by calling `GetHashCode()`) rather than by using equality operators for efficiency.</span></span> <span data-ttu-id="61fcb-3391">Ein `Join` -Abfrage-Operator kann mehrere Joins oder Gleichheit-Bedingungen in denselben Operator ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3391">A `Join` query operator may do multiple joins or equality conditions in the same operator.</span></span> <span data-ttu-id="61fcb-3392">Ein `Join` -Abfrage-Operator wird nur unterstützt, wenn der Typ der Auflistung eine Methode enthält:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3392">A `Join` query operator is only supported if the collection type contains a method:</span></span>

```vb
Function Join(inner As CS, _
                  outerSelector As Func(Of T, K), _
                  innerSelector As Func(Of S, K), _
                  resultSelector As Func(Of T, S, R)) As CR
```

<span data-ttu-id="61fcb-3393">Der code</span><span class="sxs-lookup"><span data-stu-id="61fcb-3393">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs _
            Join y In ys On x Equals y _
            ...
```

<span data-ttu-id="61fcb-3394">wird in der Regel in übersetzt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3394">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.Join( _
        ys, _
        Function(x As Integer) x, _
        Function(y As Integer) y, _
        Function(x As Integer, y As Integer) New With {x, y})...
```

<span data-ttu-id="61fcb-3395">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3395">__Note.__</span></span> <span data-ttu-id="61fcb-3396">`Join`, `On` und `Equals` sind keine reservierten Wörter.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3396">`Join`, `On` and `Equals` are not reserved words.</span></span>


### <a name="let-query-operator"></a><span data-ttu-id="61fcb-3397">Let-Abfrage-Operator</span><span class="sxs-lookup"><span data-stu-id="61fcb-3397">Let Query Operator</span></span>

<span data-ttu-id="61fcb-3398">Die `Let` Abfrage-Operator führt eine Bereichsvariable für den Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3398">The `Let` query operator introduces an expression range variable.</span></span> <span data-ttu-id="61fcb-3399">Dadurch wird ein Zwischenwert berechnet, sobald, die mehrere Male in späteren Abfrageoperatoren verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3399">This allows calculating an intermediate value once that will be used multiple times in later query operators.</span></span>

```antlr
LetQueryOperator
    : LineTerminator? 'Let' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="61fcb-3400">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3400">For example:</span></span>

```vb
Dim taxedPrices = _
    From o In Orders _
    Let tax = o.Price * 0.088 _
    Where tax > 3.50 _
    Select o.Price, tax, total = o.Price + tax
```

<span data-ttu-id="61fcb-3401">kann als gleich betrachtet werden:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3401">can be thought of as equivalent to:</span></span>

```vb
For Each o In Orders
    Dim tax = o.Price * 0.088
    ...
Next o
```

<span data-ttu-id="61fcb-3402">Ein `Let` -Abfrage-Operator wird nur unterstützt, wenn der Typ der Auflistung eine Methode enthält:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3402">A `Let` query operator is only supported if the collection type contains a method:</span></span>

```vb
Function Select(selector As Func(Of T, R)) As CR
```

<span data-ttu-id="61fcb-3403">Der code</span><span class="sxs-lookup"><span data-stu-id="61fcb-3403">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Let y = x * 10 _
            ...
```

<span data-ttu-id="61fcb-3404">wird in der Regel in übersetzt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3404">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="select-query-operator"></a><span data-ttu-id="61fcb-3405">SELECT-Abfrage-Operator</span><span class="sxs-lookup"><span data-stu-id="61fcb-3405">Select Query Operator</span></span>

<span data-ttu-id="61fcb-3406">Die `Select` -Abfrage-Operator ist, wie die `Let` -Abfrage-Operator, da es Ausdruck Bereichsvariablen; führt jedoch eine `Select` Abfrage-Operator wird ausgeblendet, die derzeit verfügbaren Bereichsvariablen anstatt sie hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3406">The `Select` query operator is like the `Let` query operator in that it introduces expression range variables; however, a `Select` query operator hides the currently available range variables instead of adding to them.</span></span> <span data-ttu-id="61fcb-3407">Auch der Typ einer Ausdruck-Bereichsvariablen eingeführt, die von einem `Select` -Abfrage-Operator wird immer abgeleitet mithilfe der lokalen Variablen Typrückschlussregeln entspricht; ein expliziter Typ kann nicht angegeben werden, und wenn kein Typ abgeleitet werden kann, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3407">Also, the type of an expression range variable introduced by a `Select` query operator is always inferred using local variable type inference rules; an explicit type cannot be specified, and if no type can be inferred, a compile-time error occurs.</span></span>

```antlr
SelectQueryOperator
    : LineTerminator? 'Select' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="61fcb-3408">Beispielsweise ist in der Abfrage:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3408">For example, in the query:</span></span>

```vb
Dim smiths = _
    From cust In Customers _
    Select name = cust.name _
    Where name.EndsWith("Smith")
```

<span data-ttu-id="61fcb-3409">die `Where` -Abfrage-Operator hat nur Zugriff auf die `name` von eingeführten Bereichsvariable der `Select` Operator; Wenn die `Where` Operator würde, käme es auf `cust`, würde ein Kompilierung ein Fehler aufgetreten.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3409">the `Where` query operator only has access to the `name` range variable introduced by the `Select` operator; if the `Where` operator had tried to reference `cust`, a compile-time error would have occurred.</span></span>

<span data-ttu-id="61fcb-3410">Anstatt Sie explizit die Namen der Bereichsvariablen, eine `Select` Abfrage-Operator kann die Namen der Bereichsvariablen, ableiten mithilfe derselben Regeln als Ausdrücke in anonymen Typ-Objekt erstellen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3410">Instead of explicitly specifying the names of the range variables, a `Select` query operator can infer the names of the range variables, using the same rules as anonymous type object creation expressions.</span></span> <span data-ttu-id="61fcb-3411">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3411">For example:</span></span>

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.name, ord.ProductName _
        Where name.EndsWith("Smith")
```

<span data-ttu-id="61fcb-3412">Wenn der Name der Bereichsvariablen nicht angegeben wird, und ein Name kann nicht abgeleitet werden, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3412">If the name of the range variable is not supplied and a name cannot be inferred, a compile-time error occurs.</span></span> <span data-ttu-id="61fcb-3413">Wenn die `Select` -Abfrage-Operator enthält nur einen einzelnen Ausdruck, wird kein Fehler auftritt, wenn Sie ein Namen für die Range-Variable kann nicht abgeleitet werden, aber die Bereichsvariable ist namenlosen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3413">If the `Select` query operator contains only a single expression, no error occurs if a name for that range variable cannot be inferred but the range variable is nameless.</span></span>  <span data-ttu-id="61fcb-3414">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3414">For example:</span></span>

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.Name & " bought " & ord.ProductName _
        Take 10
```

<span data-ttu-id="61fcb-3415">Bei eine Mehrdeutigkeit in einem `Select` zwischen Zuweisen eines Namens zu einer Range-Variable und einem gleichheitsausdruck-Abfrage-Operator, der die namenszuweisung wird bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3415">If there is an ambiguity in a `Select` query operator between assigning a name to a range variable and an equality expression, the name assignment is preferred.</span></span> <span data-ttu-id="61fcb-3416">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3416">For example:</span></span>

```vb
Dim badCustNames = _
      From c In Customers _
        Let name = "John Smith" _
      Select name = c.Name ' Creates a range variable named "name"


Dim goodCustNames = _
      From c In Customers _
        Let name = "John Smith" _
      Select match = (name = c.Name)
```

<span data-ttu-id="61fcb-3417">Jeder Ausdruck in der `Select` -Abfrage-Operator muss als Wert klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3417">Each expression in the `Select` query operator must be classified as a value.</span></span> <span data-ttu-id="61fcb-3418">Ein `Select` -Abfrage-Operator wird nur unterstützt, wenn der Typ der Auflistung eine Methode enthält:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3418">A `Select` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Select(selector As Func(Of T, R)) As CR
```

<span data-ttu-id="61fcb-3419">Der code</span><span class="sxs-lookup"><span data-stu-id="61fcb-3419">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Select x, y = x * 10 _
            ...
```

<span data-ttu-id="61fcb-3420">wird in der Regel in übersetzt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3420">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="distinct-query-operator"></a><span data-ttu-id="61fcb-3421">DISTINCT-Abfrage-Operator</span><span class="sxs-lookup"><span data-stu-id="61fcb-3421">Distinct Query Operator</span></span>

<span data-ttu-id="61fcb-3422">Die `Distinct` Abfrage-Operator schränkt die Werte in einer Sammlung nur für Benutzer mit unterschiedlichen Werten vom Typ des Elements, auf Gleichheit verglichen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3422">The `Distinct` query operator restricts the values in a collection only to those with distinct values, as determined by comparing the element type for equality.</span></span>

```antlr
DistinctQueryOperator
    : LineTerminator? 'Distinct' LineTerminator?
    ;
```

<span data-ttu-id="61fcb-3423">Um beispielsweise die Abfrage:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3423">For example, the query:</span></span>

```vb
Dim distinctCustomerPrice = _
    From cust In Customers, ord In cust.Orders _
    Select cust.Name, ord.Price _
    Distinct
```

<span data-ttu-id="61fcb-3424">Gibt eine Zeile für jede unterschiedliche Paarung ergibt sich der Preis für Neukunden Name und die Reihenfolge, nur zurück, selbst wenn der Kunde mehrere Bestellungen mit den gleichen Preis hat.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3424">will only return one row for each distinct pairing of customer name and order price, even if the customer has multiple orders with the same price.</span></span> <span data-ttu-id="61fcb-3425">Ein `Distinct` -Abfrage-Operator wird nur unterstützt, wenn der Typ der Auflistung eine Methode enthält:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3425">A `Distinct` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Distinct() As CT
```

<span data-ttu-id="61fcb-3426">Der code</span><span class="sxs-lookup"><span data-stu-id="61fcb-3426">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Distinct _
            ...
```

<span data-ttu-id="61fcb-3427">wird in der Regel in übersetzt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3427">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = xs.Distinct()...
```

<span data-ttu-id="61fcb-3428">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3428">__Note.__</span></span> <span data-ttu-id="61fcb-3429">`Distinct` ist ein reserviertes Wort.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3429">`Distinct` is not a reserved word.</span></span>


### <a name="where-query-operator"></a><span data-ttu-id="61fcb-3430">In dem Abfrage-Operator</span><span class="sxs-lookup"><span data-stu-id="61fcb-3430">Where Query Operator</span></span>

<span data-ttu-id="61fcb-3431">Die `Where` Abfrage-Operator schränkt die Werte in einer Auflistung, die eine angegebene Bedingung erfüllen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3431">The `Where` query operator restricts the values in a collection to those that satisfy a given condition.</span></span>

```antlr
WhereQueryOperator
    : LineTerminator? 'Where' LineTerminator? BooleanExpression
    ;
```

<span data-ttu-id="61fcb-3432">Ein `Where` -Abfrage-Operator akzeptiert einen booleschen Ausdruck, der für jeden Satz von Variablenwerten Bereich ausgewertet wird, wenn der Wert des Ausdrucks "true", und klicken Sie dann die Werte in der Output-Auflistung angezeigt werden, andernfalls die Werte werden übersprungen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3432">A `Where` query operator takes a Boolean expression that is evaluated for each set of range variable values; if the value of the expression is true, then the values appear in the output collection, otherwise the values are skipped.</span></span> <span data-ttu-id="61fcb-3433">Um beispielsweise den Abfrageausdruck:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3433">For example, the query expression:</span></span>

```vb
From cust In Customers, ord In Orders _
Where cust.ID = ord.CustomerID _
...
```

<span data-ttu-id="61fcb-3434">kann auf die geschachtelte Schleife als gleichwertig betrachtet werden</span><span class="sxs-lookup"><span data-stu-id="61fcb-3434">can be thought of as equivalent to the nested loop</span></span>

```vb
For Each cust In Customers
    For Each ord In Orders
            If cust.ID = ord.CustomerID Then
                ...
            End If
    Next ord
Next cust
```

<span data-ttu-id="61fcb-3435">Ein `Where` -Abfrage-Operator wird nur unterstützt, wenn der Typ der Auflistung eine Methode enthält:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3435">A `Where` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Where(predicate As Func(Of T, B)) As CT
```

<span data-ttu-id="61fcb-3436">Der code</span><span class="sxs-lookup"><span data-stu-id="61fcb-3436">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Where x < 10 _
            ...
```

<span data-ttu-id="61fcb-3437">wird in der Regel in übersetzt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3437">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x As Integer) x < 10)...
```

<span data-ttu-id="61fcb-3438">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3438">__Note.__</span></span> <span data-ttu-id="61fcb-3439">`Where` ist ein reserviertes Wort.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3439">`Where` is not a reserved word.</span></span>


### <a name="partition-query-operators"></a><span data-ttu-id="61fcb-3440">Partition-Abfrageoperatoren</span><span class="sxs-lookup"><span data-stu-id="61fcb-3440">Partition Query Operators</span></span>

```antlr
PartitionQueryOperator
    : LineTerminator? 'Take' LineTerminator? Expression
    | LineTerminator? 'Take' 'While' LineTerminator? BooleanExpression
    | LineTerminator? 'Skip' LineTerminator? Expression
    | LineTerminator? 'Skip' 'While' LineTerminator? BooleanExpression
    ;
```

<span data-ttu-id="61fcb-3441">Die `Take` Abfrageergebnisse Operator in der ersten `n` Elemente einer Auflistung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3441">The `Take` query operator results in the first `n` elements of a collection.</span></span> <span data-ttu-id="61fcb-3442">Bei Verwendung mit der `While` Modifizierer, die `Take` Operatorergebnissen in der ersten `n` Elemente einer Auflistung, die einen booleschen Ausdruck erfüllen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3442">When used with the `While` modifier, the `Take` operator results in the first `n` elements of a collection that satisfy a Boolean expression.</span></span> <span data-ttu-id="61fcb-3443">Die `Skip` Operator überspringt die ersten `n` Elemente einer Auflistung und gibt dann den Rest der Auflistung zurück.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3443">The `Skip` operator skips the first `n` elements of a collection and then returns the remainder of the collection.</span></span>  <span data-ttu-id="61fcb-3444">Bei Verwendung in Verbindung mit der `While` Modifizierer, die `Skip` Operator überspringt die ersten `n` Elemente einer Auflistung, die einen booleschen Ausdruck erfüllen, und gibt dann den Rest der Auflistung zurück.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3444">When used in conjunction with the `While` modifier, the `Skip` operator skips the first `n` elements of a collection that satisfy a Boolean expression and then returns the rest of the collection.</span></span> <span data-ttu-id="61fcb-3445">Die Ausdrücke in einem `Take` oder `Skip` -Abfrage-Operator muss als Wert klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3445">The expressions in a `Take` or `Skip` query operator must be classified as a value.</span></span>

<span data-ttu-id="61fcb-3446">Ein `Take` -Abfrage-Operator wird nur unterstützt, wenn der Typ der Auflistung eine Methode enthält:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3446">A `Take` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Take(count As N) As CT
```

<span data-ttu-id="61fcb-3447">Ein `Skip` -Abfrage-Operator wird nur unterstützt, wenn der Typ der Auflistung eine Methode enthält:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3447">A `Skip` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Skip(count As N) As CT
```

<span data-ttu-id="61fcb-3448">Ein `Take While` -Abfrage-Operator wird nur unterstützt, wenn der Typ der Auflistung eine Methode enthält:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3448">A `Take While` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function TakeWhile(predicate As Func(Of T, B)) As CT
```

<span data-ttu-id="61fcb-3449">Ein `Skip While` -Abfrage-Operator wird nur unterstützt, wenn der Typ der Auflistung eine Methode enthält:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3449">A `Skip While` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function SkipWhile(predicate As Func(Of T, B)) As CT
```

<span data-ttu-id="61fcb-3450">Der code</span><span class="sxs-lookup"><span data-stu-id="61fcb-3450">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Skip 10 _
            Take 5 _
            Skip While x < 10 _
            Take While x > 5 _
            ...
```

<span data-ttu-id="61fcb-3451">wird in der Regel in übersetzt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3451">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Skip(10). _
        Take(5). _
        SkipWhile(Function(x) x < 10). _
        TakeWhile(Function(x) x > 5)...
```

<span data-ttu-id="61fcb-3452">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3452">__Note.__</span></span> <span data-ttu-id="61fcb-3453">`Take` und `Skip` sind keine reservierten Wörter.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3453">`Take` and `Skip` are not reserved words.</span></span>


### <a name="order-by-query-operator"></a><span data-ttu-id="61fcb-3454">Order By-Abfrage-Operator</span><span class="sxs-lookup"><span data-stu-id="61fcb-3454">Order By Query Operator</span></span>

<span data-ttu-id="61fcb-3455">Die `Order By` Abfrageoperator sortiert die Werte, die in den Bereichsvariablen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3455">The `Order By` query operator orders the values that appear in the range variables.</span></span> 

```antlr
OrderByQueryOperator
    : LineTerminator? 'Order' 'By' LineTerminator? OrderExpressionList
    ;

OrderExpressionList
    : OrderExpression ( Comma OrderExpression )*
    ;

OrderExpression
    : Expression Ordering?
    ;

Ordering
    : 'Ascending' | 'Descending'
    ;
```

<span data-ttu-id="61fcb-3456">Ein `Order By` -Abfrage-Operator verwendet, Ausdrücke, die die Schlüsselwerte angeben, die zum Sortieren der Iterationsvariablen verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3456">An `Order By` query operator takes expressions that specify the key values that should be used to order the iteration variables.</span></span> <span data-ttu-id="61fcb-3457">Die folgende Abfrage gibt z. B. Produkte nach Preis sortiert:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3457">For example, the following query returns products sorted by price:</span></span>

```vb
Dim productsByPrice = _
    From p In Products _
    Order By p.Price _
    Select p.Name
```

<span data-ttu-id="61fcb-3458">Eine Sortierung kann gekennzeichnet werden, als `Ascending`, kleinere Werte in diesem Fall vor dem größere Werte stammen oder `Descending`, größere Werte in diesem Fall vor kleineren Werten zu kommen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3458">An ordering can be marked as `Ascending`, in which case smaller values come before larger values, or `Descending`, in which case larger values come before smaller values.</span></span> <span data-ttu-id="61fcb-3459">Der Standardwert für eine Sortierung, wenn keine Angabe erfolgt ist `Ascending`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3459">The default for an ordering if none is specified is `Ascending`.</span></span> <span data-ttu-id="61fcb-3460">Die folgende Abfrage gibt z. B. Produkte, zuerst nach Preis, mit dem die teuersten Produkt sortiert:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3460">For example, the following query returns products sorted by price with the most expensive product first:</span></span>

```vb
Dim productsByPriceDesc = _
    From p In Products _
    Order By p.Price Descending _
    Select p.Name
```

<span data-ttu-id="61fcb-3461">Die `Order By` Abfrage-Operator kann angeben, mehrere Ausdrücke für die Reihenfolge, in diesem Fall wird die Auflistung in geschachtelte Weise sortiert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3461">The `Order By` query operator may specify multiple expressions for ordering, in which case the collection is ordered in a nested manner.</span></span> <span data-ttu-id="61fcb-3462">Die folgende Abfrage wird z. B. Kunden nach Bundesstaat, klicken Sie dann nach Stadt in jeden Status und dann nach Postleitzahl in jeder Stadt sortiert:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3462">For example, the following query orders customers by state, then by city within each state and then by ZIP code within each city:</span></span>

```vb
Dim customersByLocation = _
    From c In Customers _
    Order By c.State, c.City, c.ZIP _
    Select c.Name, c.State, c.City, c.ZIP
```

<span data-ttu-id="61fcb-3463">Die Ausdrücke in einer `Order By` -Abfrage-Operator muss als Wert klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3463">The expressions in an `Order By` query operator must be classified as a value.</span></span> <span data-ttu-id="61fcb-3464">Ein `Order By` -Abfrage-Operator wird nur unterstützt, wenn der Typ der Auflistung eine oder beide der folgenden Methoden enthält:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3464">An `Order By` query operator is supported only if the collection type contains one or both of the following methods:</span></span>

```vb
Function OrderBy(keySelector As Func(Of T, K)) As CT
Function OrderByDescending(keySelector As Func(Of T, K)) As CT
```

<span data-ttu-id="61fcb-3465">Der Rückgabetyp `CT` muss ein *geordnete Auflistung*.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3465">The return type `CT` must be an *ordered collection*.</span></span> <span data-ttu-id="61fcb-3466">Eine geordnete Auflistung ist eine Collection-Typ, der mindestens eine der beiden Methoden enthält:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3466">An ordered collection is a collection type that contains one or both of the methods:</span></span>

```vb
Function ThenBy(keySelector As Func(Of T, K)) As CT
Function ThenByDescending(keySelector As Func(Of T, K)) As CT
```

<span data-ttu-id="61fcb-3467">Der code</span><span class="sxs-lookup"><span data-stu-id="61fcb-3467">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Order By x Ascending, x Mod 2 Descending _
            ...
```

<span data-ttu-id="61fcb-3468">wird in der Regel in übersetzt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3468">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.OrderBy(Function(x) x).ThenByDescending(Function(x) x Mod 2)...
```

<span data-ttu-id="61fcb-3469">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3469">__Note.__</span></span> <span data-ttu-id="61fcb-3470">Da Abfrageoperatoren einfach Syntax Methoden, die einen bestimmten Abfragevorgang zu implementieren zuzuordnen, Beibehaltung der Reihenfolge wird nicht von der Sprache vorgegeben, und richtet sich nach der Implementierung der der Operator selbst.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3470">Because query operators simply map syntax to methods that implement a particular query operation, order preservation is not dictated by the language and is determined by the implementation of the operator itself.</span></span> <span data-ttu-id="61fcb-3471">Dies ist eine benutzerdefinierte Operatoren sehr ähnlich, dass die Implementierung, die für einen benutzerdefinierten numerischen Typ den Additionsoperator zu überladen, kann jede Aktion ähnlich wie eine Erweiterung nicht ausführen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3471">This is very similar to user-defined operators in that the implementation to overload the addition operator for a user-defined numeric type may not perform anything resembling an addition.</span></span> <span data-ttu-id="61fcb-3472">Natürlich wird zum Beibehalten der Vorhersagbarkeit implementieren, die nicht die Erwartungen der Benutzer entspricht nicht empfohlen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3472">Of course, to preserve predictability, implementing something that does not match user expectations is not recommended.</span></span>

<span data-ttu-id="61fcb-3473">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3473">__Note.__</span></span> <span data-ttu-id="61fcb-3474">`Order` und `By` sind keine reservierten Wörter.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3474">`Order` and `By` are not reserved words.</span></span>


### <a name="group-by-query-operator"></a><span data-ttu-id="61fcb-3475">Group By Abfrageoperator</span><span class="sxs-lookup"><span data-stu-id="61fcb-3475">Group By Query Operator</span></span>

<span data-ttu-id="61fcb-3476">Die `Group By` -Abfrage-Operator gruppiert der Bereichsvariablen im Bereich, die basierend auf einem oder mehreren Ausdrücken Bereich liegen, und klicken Sie dann erstellt neue Variablen basierend auf dieser Gruppierungen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3476">The `Group By` query operator groups the range variables in scope based on one or more expressions, and then produces new range variables based on those groupings.</span></span>

```antlr
GroupByQueryOperator
    : LineTerminator? 'Group' ( LineTerminator? ExpressionRangeVariableDeclarationList )?
      LineTerminator? 'By' LineTerminator? ExpressionRangeVariableDeclarationList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="61fcb-3477">Z. B. die folgende Abfrage gruppiert alle Kunden von `State`, und klicken Sie dann berechnet die Anzahl und durchschnittliche Age der einzelnen Gruppen:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3477">For example, the following query groups all customers by `State`, and then computes the count and average age of each group:</span></span>

```vb
Dim averageAges = _
    From cust In Customers _
    Group By cust.State _
    Into Count(), Average(cust.Age)
```

<span data-ttu-id="61fcb-3478">Die `Group By` Abfrage-Operator verfügt über drei Klauseln: der optionale `Group` -Klausel, die `By` -Klausel und die `Into` Klausel.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3478">The `Group By` query operator has three clauses: the optional `Group` clause, the `By` clause, and the `Into` clause.</span></span> <span data-ttu-id="61fcb-3479">Die `Group` -Klausel besitzt die gleiche Syntax und die gleiche Wirkung wie das eine `Select` -Abfrage-Operator, mit dem Unterschied, dass sie gilt nur für die Bereichsvariablen, die zur Verfügung, in der `Into` Klausel und nicht die `By` Klausel.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3479">The `Group` clause has the same syntax and effect as a `Select` query operator, except that it only affects the range variables available in the `Into` clause and not the `By` clause.</span></span> <span data-ttu-id="61fcb-3480">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3480">For example:</span></span>

```vb
Dim averageAges = _
    From cust In Customers _
    Group cust.Age By cust.State _
    Into Count(), Average(Age)
```

<span data-ttu-id="61fcb-3481">Die `By` -Klausel deklariert Ausdruck Bereichsvariablen, die als Schlüsselwerte in den Gruppierungsvorgang verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3481">The `By` clause declares expression range variables that are used as key values in the grouping operation.</span></span> <span data-ttu-id="61fcb-3482">Die `Into` -Klausel ermöglicht die Deklaration des Ausdrucks Bereichsvariablen, die über den kombinierten Gruppen Aggregationen Berechnen der `By` Klausel.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3482">The `Into` clause allows the declaration of expression range variables that calculate aggregations over each of the groups formed by the `By` clause.</span></span> <span data-ttu-id="61fcb-3483">In der `Into` -Klausel, die Bereichsvariable der Ausdruck kann nur zugewiesen werden einen Ausdruck, der Aufruf einer Methode wird von einer *Aggregatfunktion*.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3483">Within the `Into` clause, the expression range variable can only be assigned an expression which is a method invocation of an *aggregate function*.</span></span> <span data-ttu-id="61fcb-3484">Eine Aggregatfunktion handelt es sich um eine Funktion auf die Auflistung der Gruppe (der nicht unbedingt den gleichen Sammlungstyp der ursprünglichen Auflistung sein kann) und das aussieht wie eine der folgenden Methoden:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3484">An aggregate function is a function on the collection type of the group (which may not necessarily be the same collection type of the original collection) which looks like either of the following methods:</span></span>

```vb
Function _name_() As _type_
Function _name_(selector As Func(Of T, R)) As R
```

<span data-ttu-id="61fcb-3485">Wenn eine Aggregatfunktion ein Delegatargument akzeptiert, kann der Aufrufausdruck einen Argumentausdruck enthalten, der als Wert klassifiziert werden muss.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3485">If an aggregate function takes a delegate argument, then the invocation expression can have an argument expression that must be classified as a value.</span></span>  <span data-ttu-id="61fcb-3486">Der Argumentausdruck können die Bereichsvariablen, die im Bereich befinden; im Aufruf für eine Aggregatfunktion stellen diesen Bereichsvariablen die Werte in der Gruppe wird gebildet, nicht alle Werte in der Auflistung dar.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3486">The argument expression can use the range variables that are in scope; within the call to an aggregate function, those range variables represent the values in the group being formed, not all of the values in the collection.</span></span> <span data-ttu-id="61fcb-3487">Z. B. in das ursprüngliche Beispiel in diesem Abschnitt die `Average` -Funktion berechnet den Durchschnitt der Kunden Zugriffe pro Status und nicht für alle Kunden zusammen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3487">For example, in the original example in this section the `Average` function calculates the average of the customers' ages per state rather than for all of the customers together.</span></span>

<span data-ttu-id="61fcb-3488">Sämtliche Sammlungstypen gelten, haben die Aggregatfunktion `Group` definiert, die keine Parameter und gibt einfach die Gruppe zurück.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3488">All collection types are considered to have the aggregate function `Group` defined on it, which takes no parameters and simply returns the group.</span></span> <span data-ttu-id="61fcb-3489">Andere standard-Aggregatfunktionen, die ein Auflistungstyp liefern sind:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3489">Other standard aggregate functions that a collection type may provide are:</span></span>

<span data-ttu-id="61fcb-3490">`Count` und `LongCount`, womit die Anzahl der Elemente zurückgegeben, in der Gruppe oder die Anzahl der Elemente in der Gruppe, die einen booleschen Ausdruck erfüllen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3490">`Count` and `LongCount`, which return the count of the elements in the group or the count of the elements in the group that satisfy a Boolean expression.</span></span> <span data-ttu-id="61fcb-3491">`Count` und `LongCount` werden nur unterstützt, wenn der Typ der Auflistung eine der Methoden enthält:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3491">`Count` and `LongCount` are supported only if the collection type contains one of the methods:</span></span>

```vb
Function Count() As N
Function Count(selector As Func(Of T, B)) As N
Function LongCount() As N
Function LongCount(selector As Func(Of T, B)) As N
```

<span data-ttu-id="61fcb-3492">`Sum`, die die Summe eines Ausdrucks auf alle Elemente in der Gruppe zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3492">`Sum`, which returns the sum of an expression across all the elements in the group.</span></span> <span data-ttu-id="61fcb-3493">`Sum` wird nur unterstützt, wenn der Typ der Auflistung eine der Methoden enthält:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3493">`Sum` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Sum() As N
Function Sum(selector As Func(Of T, N)) As N
```

<span data-ttu-id="61fcb-3494">`Min` den minimale Wert eines Ausdrucks auf alle Elemente in der Gruppe zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3494">`Min` which returns the minimum value of an expression across all the elements in the group.</span></span> <span data-ttu-id="61fcb-3495">`Min` wird nur unterstützt, wenn der Typ der Auflistung eine der Methoden enthält:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3495">`Min` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Min() As N
Function Min(selector As Func(Of T, N)) As N
```

<span data-ttu-id="61fcb-3496">`Max`, womit den maximalen Wert eines Ausdrucks auf alle Elemente in der Gruppe.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3496">`Max`, which returns the maximum value of an expression across all the elements in the group.</span></span> <span data-ttu-id="61fcb-3497">`Max` wird nur unterstützt, wenn der Typ der Auflistung eine der Methoden enthält:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3497">`Max` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Max() As N
Function Max(selector As Func(Of T, N)) As N
```

<span data-ttu-id="61fcb-3498">`Average`, die gibt den Mittelwert eines Ausdrucks auf alle Elemente in der Gruppe zurück.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3498">`Average`, which returns the average of an expression across all the elements in the group.</span></span> <span data-ttu-id="61fcb-3499">`Average` wird nur unterstützt, wenn der Typ der Auflistung eine der Methoden enthält:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3499">`Average` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Average() As N
Function Average(selector As Func(Of T, N)) As N
```

<span data-ttu-id="61fcb-3500">`Any`, der bestimmt, ob eine Gruppe Elemente enthält oder ein boolescher Ausdruck für jedes Element in der Gruppe "true" ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3500">`Any`, which determines whether a group contains members or if a Boolean expression is true for any element in the group.</span></span> <span data-ttu-id="61fcb-3501">`Any` Gibt einen Wert, der in einem booleschen Ausdruck verwendet werden kann und wird nur unterstützt, wenn der Typ der Auflistung eine der Methoden enthält:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3501">`Any` returns a value that can be used in a Boolean expression and is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Any() As B
Function Any(predicate As Func(Of T, B)) As B
```

<span data-ttu-id="61fcb-3502">`All`, der bestimmt, ob ein boolescher Ausdruck für alle Elemente in der Gruppe "true" ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3502">`All`, which determines whether a Boolean expression is true for all elements in the group.</span></span> <span data-ttu-id="61fcb-3503">`All` Gibt einen Wert, der in einem booleschen Ausdruck verwendet werden kann und wird nur unterstützt, wenn der Typ der Auflistung eine Methode enthält:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3503">`All` returns a value that can be used in a Boolean expression and is supported only if the collection type contains a method:</span></span>

```vb
Function All(predicate As Func(Of T, B)) As B
```

<span data-ttu-id="61fcb-3504">Nach einem `Group By` -Abfrage-Operator, der Bereichsvariablen zuvor im Bereich ausgeblendet werden, und die Bereichsvariablen eingeführt, durch die `By` und `Into` Klauseln sind verfügbar.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3504">After a `Group By` query operator, the range variables previously in scope are hidden, and the range variables introduced by the `By` and `Into` clauses are available.</span></span> <span data-ttu-id="61fcb-3505">Ein `Group By` -Abfrage-Operator wird nur unterstützt, wenn der Typ der Auflistung die Methode enthält:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3505">A `Group By` query operator is supported only if the collection type contains the method:</span></span>

```vb
Function GroupBy(keySelector As Func(Of T, K), _
                      resultSelector As Func(Of K, CT, R)) As CR
```

<span data-ttu-id="61fcb-3506">Variablendeklarationen in den Bereich der `Group` -Klausel werden unterstützt, nur, wenn der Typ der Auflistung die Methode enthält:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3506">Range variable declarations in the `Group` clause are supported only if the collection type contains the method:</span></span>

```vb
Function GroupBy(keySelector As Func(Of T, K), _
                      elementSelector As Func(Of T, S), _
                      resultSelector As Func(Of K, CS, R)) As CR
```

<span data-ttu-id="61fcb-3507">Der code</span><span class="sxs-lookup"><span data-stu-id="61fcb-3507">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Group y = x * 10, z = x / 10 By evenOdd = x Mod 2 _
            Into Sum(y), Average(z) _
            ...
```

<span data-ttu-id="61fcb-3508">wird in der Regel in übersetzt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3508">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.GroupBy( _
        Function(x As Integer) x Mod 2, _
        Function(x As Integer) New With {.y = x * 10, .z = x / 10}, _
        Function(evenOdd, group) New With { _
            evenOdd, _
            .Sum = group.Sum(Function(e) e.y), _
            .Average = group.Average(Function(e) e.z)})...
```

<span data-ttu-id="61fcb-3509">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3509">__Note.__</span></span> <span data-ttu-id="61fcb-3510">`Group`, `By`, und `Into` sind keine reservierten Wörter.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3510">`Group`, `By`, and `Into` are not reserved words.</span></span>


### <a name="aggregate-query-operator"></a><span data-ttu-id="61fcb-3511">Aggregate-Abfrage-Operator</span><span class="sxs-lookup"><span data-stu-id="61fcb-3511">Aggregate Query Operator</span></span>

<span data-ttu-id="61fcb-3512">Die `Aggregate` -Abfrage-Operator hat eine ähnliche Funktion wie der `Group By` -Operator, mit der Ausnahme ermöglicht aggregieren über Gruppen, die bereits gebildet wurden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3512">The `Aggregate` query operator performs a similar function as the `Group By` operator, except it allows aggregating over groups that have already been formed.</span></span> <span data-ttu-id="61fcb-3513">Da die Gruppe bereits gebildet wurde, die `Into` -Klausel einer `Aggregate` Abfrage-Operator wird nicht der Bereichsvariablen im Bereich ausgeblendet (auf diese Weise `Aggregate` eher wie eine `Let`, und `Group By` eher wie eine `Select`).</span><span class="sxs-lookup"><span data-stu-id="61fcb-3513">Because the group has already been formed, the `Into` clause of an `Aggregate` query operator does not hide the range variables in scope (in this way, `Aggregate` is more like a `Let`, and `Group By` is more like a `Select`).</span></span>

```antlr
AggregateQueryOperator
    : LineTerminator? 'Aggregate' LineTerminator? CollectionRangeVariableDeclaration QueryOperator*
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="61fcb-3514">Die folgende Abfrage wird z. B. die Summe aller Bestellungen von Kunden in Washington, USA aggregiert:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3514">For example, the following query aggregates the total of all the orders placed by customers in Washington:</span></span>

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Into Sum(order.Total)
```

<span data-ttu-id="61fcb-3515">Das Ergebnis dieser Abfrage ist eine Auflistung, dessen Elementtyp, ist ein anonymer Typ mit einer Eigenschaft mit dem Namen `cust` als `Customer` und eine Eigenschaft mit dem Namen `Sum` als `Integer`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3515">The result of this query is a collection whose element type is an anonymous type with a property named `cust` typed as `Customer` and a property named `Sum` typed as `Integer`.</span></span>

<span data-ttu-id="61fcb-3516">Im Gegensatz zu `Group By`, zusätzlichen Abfrageoperatoren platziert werden können, zwischen den `Aggregate` und `Into` Klauseln.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3516">Unlike `Group By`, additional query operators can be placed between the `Aggregate` and `Into` clauses.</span></span> <span data-ttu-id="61fcb-3517">Zwischen einer `Aggregate` -Klausel und dem Ende der `Into` -Klausel, die alle Bereichsvariablen im Bereich, einschließlich der deklariert, indem die `Aggregate` -Klausel kann verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3517">Between an `Aggregate` clause and the end of the `Into` clause, all range variables in scope, including those declared by the `Aggregate` clause can be used.</span></span> <span data-ttu-id="61fcb-3518">Z. B. die folgende Abfrage Aggregate die Gesamtmenge aller Bestellungen platziert von Kunden in Washington, USA vor 2006:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3518">For example, the following query aggregates the sum total of all the orders placed by customers in Washington before 2006:</span></span>

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum = Sum(order.Total)
```

<span data-ttu-id="61fcb-3519">Die `Aggregate` Operator kann auch zu einem Abfrageausdruck verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3519">The `Aggregate` operator can also be used to start a query expression.</span></span> <span data-ttu-id="61fcb-3520">In diesem Fall wird das Ergebnis des Abfrageausdrucks werden den einzelnen Wert berechnet, indem die `Into` Klausel.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3520">In this case, the result of the query expression will be the single value computed by the `Into` clause.</span></span> <span data-ttu-id="61fcb-3521">Die folgende Abfrage wird z. B. die Summe der Auftragssummen vor dem 1. Januar 2006 berechnet:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3521">For example, the following query calculates the sum of all the order totals before January 1st, 2006:</span></span>

```vb
Dim ordersTotal = _
    Aggregate order In Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum(order.Total)
```

<span data-ttu-id="61fcb-3522">Das Ergebnis der Abfrage ist eine einzelne `Integer` Wert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3522">The result of the query is a single `Integer` value.</span></span> <span data-ttu-id="61fcb-3523">Ein `Aggregate` -Abfrage-Operator ist immer verfügbar (obwohl die aggregate-Funktion werden zudem muss für den Ausdruck gültig ist).</span><span class="sxs-lookup"><span data-stu-id="61fcb-3523">An `Aggregate` query operator is always available (although the aggregate function must be also be available for the expression to be valid).</span></span> <span data-ttu-id="61fcb-3524">Der code</span><span class="sxs-lookup"><span data-stu-id="61fcb-3524">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    Aggregate x In xs _
    Where x < 5 _
    Into Sum()
```

<span data-ttu-id="61fcb-3525">wird in der Regel in übersetzt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3525">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x) x < 5).Sum()
```

<span data-ttu-id="61fcb-3526">__Beachten Sie.__ `Aggregate`</span><span class="sxs-lookup"><span data-stu-id="61fcb-3526">__Note.__ `Aggregate`</span></span> <span data-ttu-id="61fcb-3527">und `Into` sind keine reservierten Wörter.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3527">and `Into` are not reserved words.</span></span>


### <a name="group-join-query-operator"></a><span data-ttu-id="61fcb-3528">Group Join-Abfrage-Operator</span><span class="sxs-lookup"><span data-stu-id="61fcb-3528">Group Join Query Operator</span></span>

<span data-ttu-id="61fcb-3529">Die `Group Join` -Abfrage-Operator kombiniert die Funktionen des die `Join` und `Group By` Abfrageoperatoren in einen einzelnen Operator.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3529">The `Group Join` query operator combines the functions of the `Join` and `Group By` query operators into a single operator.</span></span> <span data-ttu-id="61fcb-3530">`Group Join` verknüpft zwei Auflistungen, die auf Grundlage übereinstimmender Schlüssel extrahiert aus den Elementen, das Gruppieren aller Elemente auf der rechten Seite des Joins, die ein bestimmtes Element auf der linken Seite des Joins entsprechen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3530">`Group Join` joins two collections based on matching keys extracted from the elements, grouping together all of the elements on the right side of the join that match a particular element on the left side of the join.</span></span> <span data-ttu-id="61fcb-3531">Daher wird der Operator einen Satz von hierarchische Ergebnisse erzeugt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3531">Thus, the operator produces a set of hierarchical results.</span></span>

```antlr
GroupJoinQueryOperator
    : LineTerminator? 'Group' 'Join' LineTerminator? CollectionRangeVariableDeclaration
      JoinOrGroupJoinQueryOperator? LineTerminator? 'On' LineTerminator? JoinConditionList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="61fcb-3532">Die folgende Abfrage erzeugt z. B. Elemente, die einen einzelnen Kunden Namen, eine Gruppe von alle Bestellungen und die Gesamtmenge aller die Bestellungen enthalten:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3532">For example, the following query produces elements that contain a single customer's name, a group of all of their orders, and the total amount of all of those orders:</span></span>

```vb
Dim custsWithOrders = _
    From cust In Customers _
    Group Join order In Orders On cust.ID Equals order.CustomerID _ 
    Into Orders = Group, OrdersTotal = Sum(order.Total) _
    Select cust.Name, Orders, OrdersTotal
```

<span data-ttu-id="61fcb-3533">Das Ergebnis der Abfrage ist eine Auflistung, dessen Elementtyp ein anonymer Typ mit den drei Eigenschaften ist: `Name`, als typisierte `String`, `Orders` als eine Auflistung, dessen Elementtyp `Order`, und `OrdersTotal`, typisierte als `Integer`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3533">The result of the query is a collection whose element type is an anonymous type with three properties: `Name`, typed as `String`, `Orders` typed as a collection whose element type is `Order`, and `OrdersTotal`, typed as `Integer`.</span></span> <span data-ttu-id="61fcb-3534">Ein `Group Join` -Abfrage-Operator wird nur unterstützt, wenn der Typ der Auflistung die Methode enthält:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3534">A `Group Join` query operator is supported only if the collection type contains the method:</span></span>

```vb
Function GroupJoin(inner As CS, _
                         outerSelector As Func(Of T, K), _
                         innerSelector As Func(Of S, K), _
                         resultSelector As Func(Of T, CS, R)) As CR
```

<span data-ttu-id="61fcb-3535">Der code</span><span class="sxs-lookup"><span data-stu-id="61fcb-3535">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs _
            Group Join y in ys On x Equals y _
            Into g = Group _
            ...
```

<span data-ttu-id="61fcb-3536">wird in der Regel in übersetzt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3536">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.GroupJoin( _
        ys, _
        Function(x As Integer) x, _
        Function(y As Integer) y, _
        Function(x, group) New With {x, .g = group})...
```

<span data-ttu-id="61fcb-3537">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3537">__Note.__</span></span> <span data-ttu-id="61fcb-3538">`Group`, `Join`, und `Into` sind keine reservierten Wörter.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3538">`Group`, `Join`, and `Into` are not reserved words.</span></span>


## <a name="conditional-expressions"></a><span data-ttu-id="61fcb-3539">Bedingte Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-3539">Conditional Expressions</span></span>

<span data-ttu-id="61fcb-3540">Eine bedingte `If` Ausdruck einen Ausdruck und gibt einen Wert zurück.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3540">A conditional `If` expression tests an expression and returns a value.</span></span>

```antlr
ConditionalExpression
    : 'If' OpenParenthesis BooleanExpression Comma Expression Comma Expression CloseParenthesis
    | 'If' OpenParenthesis Expression Comma Expression CloseParenthesis
    ;
```

<span data-ttu-id="61fcb-3541">Im Gegensatz zu den `IIF` Laufzeitfunktion, jedoch ein bedingter Ausdruck nur wertet Operanden, die bei Bedarf.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3541">Unlike the `IIF` runtime function, however, a conditional expression only evaluates its operands if necessary.</span></span> <span data-ttu-id="61fcb-3542">Also z. B. der Ausdruck `If(c Is Nothing, c.Name, "Unknown")` wird keine Ausnahme ausgelöst, wenn der Wert des `c` ist `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3542">Thus, for example, the expression `If(c Is Nothing, c.Name, "Unknown")` will not throw an exception if the value of `c` is `Nothing`.</span></span> <span data-ttu-id="61fcb-3543">Der bedingte Ausdruck weist zwei Formen: eine, die zwei Operanden und eine, die akzeptiert verfügt über drei Operanden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3543">The conditional expression has two forms: one that takes two operands and one that takes three operands.</span></span>

<span data-ttu-id="61fcb-3544">Wenn drei Operanden bereitgestellt werden, müssen alle drei Ausdrücke als Werte klassifiziert, und der erste Operand muss ein boolescher Ausdruck sein.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3544">If three operands are provided, all three expressions must be classified as values, and the first operand must be a Boolean expression.</span></span> <span data-ttu-id="61fcb-3545">Wenn das Ergebnis des Ausdrucks ist "true", dann ist des zweiten Ausdrucks das Ergebnis des Operators, andernfalls der dritte Ausdruck werden das Ergebnis des Operators.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3545">If the result is of the expression is true, then the second expression will be the result of the operator, otherwise the third expression will be the result of the operator.</span></span> <span data-ttu-id="61fcb-3546">Der Ergebnistyp des Ausdrucks ist der bestimmende Typ zwischen den Typen des zweiten und dritten Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3546">The result type of the expression is the dominant type between the types of the second and third expression.</span></span> <span data-ttu-id="61fcb-3547">Ist kein dominanter Typ, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3547">If there is no dominant type, then a compile-time error occurs.</span></span>

<span data-ttu-id="61fcb-3548">Wenn zwei Operanden bereitgestellt werden, müssen beide Operanden als Werte klassifiziert, und der erste Operand muss ein Verweistyp oder ein Werttyp sein.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3548">If two operands are provided, both of the operands must be classified as values, and the first operand must be either a reference type or a nullable value type.</span></span> <span data-ttu-id="61fcb-3549">Der Ausdruck `If(x, y)` wird dann ausgewertet, als ob der Ausdruck wurde `If(x IsNot Nothing, x, y)`, mit zwei Ausnahmen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3549">The expression `If(x, y)` is then evaluated as if was the expression `If(x IsNot Nothing, x, y)`, with two exceptions.</span></span> <span data-ttu-id="61fcb-3550">Zuerst der erste Ausdruck wird immer nur ausgewertet, einmal und dann ein zweites, wenn der zweite Operand ein NULL-Werte ist und der erste Operand ist, die `?` wird aus dem Typ des ersten Operanden entfernt, wenn es sich bei den bestimmenden Typ für die Bestimmung der der Ergebnistyp des Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3550">First, the first expression is only ever evaluated once, and second, if the second operand's type is a non-nullable value type and the first operand's type is, the `?` is removed from the type of the first operand when determining the dominant type for the result type of the expression.</span></span> <span data-ttu-id="61fcb-3551">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3551">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim x?, y As Integer
        Dim a?, b As Long

        a = If(x, a)        ' Result type: Long?
        y = If(x, 0)        ' Result type: Integer
    End Sub
End Module
```

<span data-ttu-id="61fcb-3552">In beiden Formen des Ausdrucks, wenn ein Operand `Nothing`, dessen Typ wird nicht verwendet, um zu bestimmen, den bestimmenden Typ.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3552">In both forms of the expression, if an operand is `Nothing`, its type is not used to determine the dominant type.</span></span> <span data-ttu-id="61fcb-3553">Wenn der Ausdruck `If(<expression>, Nothing, Nothing)`, der bestimmende Typ gilt `Object`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3553">In the case of the expression `If(<expression>, Nothing, Nothing)`, the dominant type is considered to be `Object`.</span></span>


## <a name="xml-literal-expressions"></a><span data-ttu-id="61fcb-3554">XML-Literale Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-3554">XML Literal Expressions</span></span>

<span data-ttu-id="61fcb-3555">Ein XML-Literalen Ausdruck stellt eine XML (eXtensible Markup Language) 1.0-Wert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3555">An XML literal expression represents an XML (eXtensible Markup Language) 1.0 value.</span></span>

```antlr
XMLLiteralExpression
    : XMLDocument
    | XMLElement
    | XMLProcessingInstruction
    | XMLComment
    | XMLCDATASection
    ;
```

<span data-ttu-id="61fcb-3556">Das Ergebnis eines XML-Literalen Ausdrucks ist ein Wert, der als einen der Typen aus den `System.Xml.Linq` Namespace.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3556">The result of an XML literal expression is a value typed as one of the types from the `System.Xml.Linq` namespace.</span></span> <span data-ttu-id="61fcb-3557">Wenn die Typen in diesem Namespace nicht verfügbar sind, klicken Sie dann verursacht eine XML-Literalen Ausdrucks während der Kompilierung einen Fehler.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3557">If the types in that namespace are not available, then an XML literal expression will cause a compile-time error.</span></span> <span data-ttu-id="61fcb-3558">Die Werte werden durch Konstruktoraufrufe aus der XML-Literale Ausdruck übersetzt generiert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3558">The values are generated through constructor calls translated from the XML literal expression.</span></span> <span data-ttu-id="61fcb-3559">Beispielsweise kann der Code:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3559">For example, the code:</span></span>

```vb
Dim book As System.Xml.Linq.XElement = _
    <book title="My book"></book>
```

<span data-ttu-id="61fcb-3560">entspricht ungefähr der Code:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3560">is roughly equivalent to the code:</span></span>

```vb
Dim book As System.Xml.Linq.XElement = _
    New System.Xml.Linq.XElement( _
        "book", _
        New System.Xml.Linq.XAttribute("title", "My book"))
```

<span data-ttu-id="61fcb-3561">Ein XML-Literalen Ausdruck kann es sich um die Form des XML-Dokument, ein XML-Element, eine XML-verarbeitungsanweisung, ein XML-Kommentar oder einem CDATA-Abschnitt haben.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3561">An XML literal expression can take the form of an XML document, an XML element, an XML processing instruction, an XML comment, or a CDATA section.</span></span>

<span data-ttu-id="61fcb-3562">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3562">__Note.__</span></span> <span data-ttu-id="61fcb-3563">Diese Spezifikation enthält nur aus einer Beschreibung des XML-Codes, um das Verhalten von Visual Basic-Sprache zu beschreiben.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3563">This specification contains only enough of a description of XML to describe the behavior of the Visual Basic language.</span></span> <span data-ttu-id="61fcb-3564">Weitere Informationen zu XML finden Sie unter http://www.w3.org/TR/REC-xml/.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3564">More information on XML can be found at http://www.w3.org/TR/REC-xml/.</span></span>


### <a name="lexical-rules"></a><span data-ttu-id="61fcb-3565">Lexikalischen Regeln</span><span class="sxs-lookup"><span data-stu-id="61fcb-3565">Lexical rules</span></span>

```antlr
XMLCharacter
    : '<Unicode tab character (0x0009)>'
    | '<Unicode linefeed character (0x000A)>'
    | '<Unicode carriage return character (0x000D)>'
    | '<Unicode characters 0x0020 - 0xD7FF>'
    | '<Unicode characters 0xE000 - 0xFFFD>'
    | '<Unicode characters 0x10000 - 0x10FFFF>'
    ;

XMLString
    : XMLCharacter+
    ;

XMLWhitespace
    : XMLWhitespaceCharacter+
    ;

XMLWhitespaceCharacter
    : '<Unicode carriage return character (0x000D)>'
    | '<Unicode linefeed character (0x000A)>'
    | '<Unicode space character (0x0020)>'
    | '<Unicode tab character (0x0009)>'
    ;

XMLNameCharacter
    : XMLLetter
    | XMLDigit
    | '.'
    | '-'
    | '_'
    | ':'
    | XMLCombiningCharacter
    | XMLExtender
    ;

XMLNameStartCharacter
    : XMLLetter
    | '_'
    | ':'
    ;

XMLName
    : XMLNameStartCharacter XMLNameCharacter*
    ;

XMLLetter
    : '<Unicode character as defined in the Letter production of the XML 1.0 specification>'
    ;

XMLDigit
    : '<Unicode character as defined in the Digit production of the XML 1.0 specification>'
    ;

XMLCombiningCharacter
    : '<Unicode character as defined in the CombiningChar production of the XML 1.0 specification>'
    ;

XMLExtender
    : '<Unicode character as defined in the Extender production of the XML 1.0 specification>'
    ;
```

<span data-ttu-id="61fcb-3566">XML-Literale Ausdrücke werden mit den lexikalischen Regeln der XML anstelle von den lexikalischen Regeln der regulären Visual Basic-Code interpretiert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3566">XML literal expressions are interpreted using the lexical rules of XML instead of the lexical rules of regular Visual Basic code.</span></span> <span data-ttu-id="61fcb-3567">Die zwei Sätze von Regeln unterscheiden sich in der Regel auf folgende Weise:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3567">The two sets of rules generally differ in the following ways:</span></span>

* <span data-ttu-id="61fcb-3568">Leerraum ist wichtig, im XML-Format.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3568">White space is significant in XML.</span></span> <span data-ttu-id="61fcb-3569">Daher gibt die Grammatik für XML-Literale Ausdrücke explizit an, in denen Leerraum zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3569">As a result, the grammar for XML literal expressions explicitly states where white space is allowed.</span></span> <span data-ttu-id="61fcb-3570">Leerzeichen werden nicht beibehalten, außer wenn es im Kontext von Zeichendaten in einem Element auftritt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3570">Whitespace is not preserved, except when it occurs in the context of character data within an element.</span></span> <span data-ttu-id="61fcb-3571">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3571">For example:</span></span>

  ```vb
  ' The following element preserves no whitespace
  Dim e1 = _
      <customer>
          <name>Bob</>
      </>

  ' The following element preserves all of the whitespace
  Dim e2 = _
      <customer>
          Bob
      </>
  ```

* <span data-ttu-id="61fcb-3572">XML-End-of-Line-Leerraum wird anhand der XML-Spezifikation normalisiert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3572">XML end-of-line whitespace is normalized according to the XML specification.</span></span>

* <span data-ttu-id="61fcb-3573">Bei XML muss die Groß- und Kleinschreibung beachtet werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3573">XML is case-sensitive.</span></span> <span data-ttu-id="61fcb-3574">Schlüsselwörter müssen die Groß-/Kleinschreibung genau übereinstimmen. andernfalls tritt ein Fehler während der Kompilierung auf.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3574">Keywords must match casing exactly, or else a compile-time error will occur.</span></span>

* <span data-ttu-id="61fcb-3575">Zeilenabschlusszeichen werden Leerzeichen in XML-Datei berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3575">Line terminators are considered white space in XML.</span></span> <span data-ttu-id="61fcb-3576">Daher sind keine Zeilenfortsetzungszeichen in XML-Literale Ausdrücke erforderlich.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3576">As a result, no line-continuation characters are needed in XML literal expressions.</span></span>

* <span data-ttu-id="61fcb-3577">XML akzeptiert keine Zeichen voller Breite.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3577">XML does not accept full-width characters.</span></span> <span data-ttu-id="61fcb-3578">Wenn Zeichen voller Breite verwendet werden, wird ein Fehler während der Kompilierung auftreten.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3578">If full-width characters are used, a compile-time error will occur.</span></span>


### <a name="embedded-expressions"></a><span data-ttu-id="61fcb-3579">Eingebettete Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-3579">Embedded expressions</span></span>

<span data-ttu-id="61fcb-3580">XML-Literale Ausdrücke dürfen *eingebetteten Ausdrücken*.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3580">XML literal expressions can contain *embedded expressions*.</span></span> <span data-ttu-id="61fcb-3581">Ein eingebetteter Ausdruck ist eine Visual Basic-Ausdruck, der ausgewertet und eine oder mehrere Werte an der Position des eingebetteten Ausdrucks verwendet.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3581">An embedded expression is a Visual Basic expression that is evaluated and used to fill in one or more values at the location of embedded expression.</span></span>

```antlr
XMLEmbeddedExpression
    : '<' '%' '=' LineTerminator? Expression LineTerminator? '%' '>'
    ;
```

<span data-ttu-id="61fcb-3582">Der folgende Code wird z. B. die Zeichenfolge `John Smith` als Wert für das XML-Element:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3582">For example, the following code places the string `John Smith` as the value of the XML element:</span></span>

```vb
Dim name as String = "John Smith"
Dim element As System.Xml.Linq.XElement = <customer><%= name %></customer>
```

<span data-ttu-id="61fcb-3583">Ausdrücke können in verschiedenen Kontexten eingebettet werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3583">Expressions can be embedded in a number of contexts.</span></span> <span data-ttu-id="61fcb-3584">Der folgende Code erzeugt z. B. ein Element mit dem Namen `customer`:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3584">For example, the following code produces an element named `customer`:</span></span>

```vb
Dim name As String = "customer"
Dim element As System.Xml.Linq.XElement = <<%= name %>>John Smith</>
```

<span data-ttu-id="61fcb-3585">Jeder Kontext, in denen ein eingebetteter Ausdruck verwendet werden kann, gibt die Typen, die akzeptiert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3585">Each context where an embedded expression can be used specifies the types that will be accepted.</span></span> <span data-ttu-id="61fcb-3586">Wenn im Kontext der Ausdruck, der Teil eines eingebetteten Ausdrucks können müssen die normalen lexikalischen Regeln für Visual Basic-Code gelten also weiterhin, z. B. zeilenfortsetzungen verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3586">When within the context of the expression part of an embedded expression, the normal lexical rules for Visual Basic code still apply so, for example, line continuations must be used:</span></span>

```vb
' Visual Basic expression uses line continuation, XML does not
Dim element As System.Xml.Linq.XElement = _
    <<%= name & _
          name %>>John 
                     Smith</>
```


### <a name="xml-documents"></a><span data-ttu-id="61fcb-3587">XML-Dokumenten</span><span class="sxs-lookup"><span data-stu-id="61fcb-3587">XML Documents</span></span>

```antlr
XMLDocument
    : XMLDocumentPrologue XMLMisc* XMLDocumentBody XMLMisc*
    ;

XMLDocumentPrologue
    : '<' '?' 'xml' XMLVersion XMLEncoding? XMLStandalone? XMLWhitespace? '?' '>'
    ;

XMLVersion
    : XMLWhitespace 'version' XMLWhitespace? '=' XMLWhitespace? XMLVersionNumberValue
    ;

XMLVersionNumberValue
    : SingleQuoteCharacter '1' '.' '0' SingleQuoteCharacter
    | DoubleQuoteCharacter '1' '.' '0' DoubleQuoteCharacter
    ;

XMLEncoding
    : XMLWhitespace 'encoding' XMLWhitespace? '=' XMLWhitespace? XMLEncodingNameValue
    ;

XMLEncodingNameValue
    : SingleQuoteCharacter XMLEncodingName SingleQuoteCharacter
    | DoubleQuoteCharacter XMLEncodingName DoubleQuoteCharacter
    ;

XMLEncodingName
    : XMLLatinAlphaCharacter XMLEncodingNameCharacter*
    ;

XMLEncodingNameCharacter
    : XMLUnderscoreCharacter
    | XMLLatinAlphaCharacter
    | XMLNumericCharacter
    | XMLPeriodCharacter
    | XMLDashCharacter
    ;

XMLLatinAlphaCharacter
    : '<Unicode Latin alphabetic character (0x0041-0x005a, 0x0061-0x007a)>'
    ;

XMLNumericCharacter
    : '<Unicode digit character (0x0030-0x0039)>'
    ;

XMLHexNumericCharacter
    : XMLNumericCharacter
    | '<Unicode Latin hex alphabetic character (0x0041-0x0046, 0x0061-0x0066)>'
    ;

XMLPeriodCharacter
    : '<Unicode period character (0x002e)>'
    ;

XMLUnderscoreCharacter
    : '<Unicode underscore character (0x005f)>'
    ;

XMLDashCharacter
    : '<Unicode dash character (0x002d)>'
    ;

XMLStandalone
    : XMLWhitespace 'standalone' XMLWhitespace? '=' XMLWhitespace? XMLYesNoValue
    ;

XMLYesNoValue
    : SingleQuoteCharacter XMLYesNo SingleQuoteCharacter
    | DoubleQuoteCharacter XMLYesNo DoubleQuoteCharacter
    ;

XMLYesNo
    : 'yes'
    | 'no'
    ;

XMLMisc
    : XMLComment
    | XMLProcessingInstruction
    | XMLWhitespace
    ;

XMLDocumentBody
    : XMLElement
    | XMLEmbeddedExpression
    ;
```

<span data-ttu-id="61fcb-3588">Ein XML-Dokument einen Wert, der als ergibt `System.Xml.Linq.XDocument`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3588">An XML document results in a value typed as `System.Xml.Linq.XDocument`.</span></span> <span data-ttu-id="61fcb-3589">Im Gegensatz zu XML 1.0-Spezifikation werden XML-Dokumenten in XML-Literale Ausdrücke erforderlich, um den XML-Dokument Prolog angeben; XML-Literale Ausdrücke ohne den Prolog des XML-Dokument sind als die einzelne Entität interpretiert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3589">Unlike the XML 1.0 specification, XML documents in XML literal expressions are required to specify the XML document prologue; XML literal expressions without the XML document prologue are interpreted as their individual entity.</span></span> <span data-ttu-id="61fcb-3590">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3590">For example:</span></span>

```vb
Dim doc As System.Xml.Linq.XDocument = _
    <?xml version="1.0"?>
    <?instruction?>
    <customer>Bob</>

Dim pi As System.Xml.Linq.XProcessingInstruction = _
    <?instruction?>
```

<span data-ttu-id="61fcb-3591">Ein XML-Dokument kann einen eingebetteten Ausdruck enthalten, dessen Typ auf einen beliebigen Typ sein kann. zur Laufzeit jedoch das Objekt muss den Anforderungen entsprechen den `XDocument` Konstruktor oder einen Laufzeitfehler tritt auf.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3591">An XML document can contain an embedded expression whose type can be any type; at runtime, however, the object must satisfy the requirements of the `XDocument` constructor or a run-time error will occur.</span></span>

<span data-ttu-id="61fcb-3592">Im Gegensatz zu regulären XML unterstützen Ausdrücke für XML-Dokument keine DTDs (Document Type Deklarationen).</span><span class="sxs-lookup"><span data-stu-id="61fcb-3592">Unlike regular XML, XML document expressions do not support DTDs (Document Type Declarations).</span></span> <span data-ttu-id="61fcb-3593">Darüber hinaus wird das encoding-Attribut, wenn angegeben, ignoriert, da die Codierung des XML-Literalen Ausdrucks immer identisch mit der Codierung der Quelldatei selbst ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3593">Also, the encoding attribute, if supplied, will be ignored since the encoding of the Xml literal expression is always the same as the encoding of the source file itself.</span></span>

<span data-ttu-id="61fcb-3594">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3594">__Note.__</span></span> <span data-ttu-id="61fcb-3595">Obwohl das encoding-Attribut ignoriert wird, ist es noch gültig-Attribut, um die Möglichkeit, enthalten keine gültige Xml 1.0-Dokumente im Quellcode zu gewährleisten.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3595">Although the encoding attribute is ignored, it is still valid attribute in order to maintain the ability to include any valid Xml 1.0 documents in source code.</span></span>


### <a name="xml-elements"></a><span data-ttu-id="61fcb-3596">XML-Elemente</span><span class="sxs-lookup"><span data-stu-id="61fcb-3596">XML Elements</span></span>

```antlr
XMLElement
    : XMLEmptyElement
    | XMLElementStart XMLContent XMLElementEnd
    ;

XMLEmptyElement
    : '<' XMLQualifiedNameOrExpression XMLAttribute* XMLWhitespace? '/' '>'
    ;

XMLElementStart
    : '<' XMLQualifiedNameOrExpression XMLAttribute* XMLWhitespace? '>'
    ;

XMLElementEnd
    : '<' '/' '>'
    | '<' '/' XMLQualifiedName XMLWhitespace? '>'
    ;

XMLContent
    : XMLCharacterData? ( XMLNestedContent XMLCharacterData? )+
    ;

XMLCharacterData
    : '<Any XMLCharacterDataString that does not contain the string "]]>">'
    ;

XMLCharacterDataString
    : '<Any Unicode character except < or &>'+
    ;

XMLNestedContent
    : XMLElement
    | XMLReference
    | XMLCDATASection
    | XMLProcessingInstruction
    | XMLComment
    | XMLEmbeddedExpression
    ;

XMLAttribute
    : XMLWhitespace XMLAttributeName XMLWhitespace? '=' XMLWhitespace? XMLAttributeValue
    | XMLWhitespace XMLEmbeddedExpression
    ;

XMLAttributeName
    : XMLQualifiedNameOrExpression
    | XMLNamespaceAttributeName
    ;

XMLAttributeValue
    : DoubleQuoteCharacter XMLAttributeDoubleQuoteValueCharacter* DoubleQuoteCharacter
    | SingleQuoteCharacter XMLAttributeSingleQuoteValueCharacter* SingleQuoteCharacter
    | XMLEmbeddedExpression
    ;

XMLAttributeDoubleQuoteValueCharacter
    : '<Any XMLCharacter except <, &, or DoubleQuoteCharacter>'
    | XMLReference
    ;

XMLAttributeSingleQuoteValueCharacter
    : '<Any XMLCharacter except <, &, or SingleQuoteCharacter>'
    | XMLReference
    ;

XMLReference
    : XMLEntityReference
    | XMLCharacterReference
    ;

XMLEntityReference
    : '&' XMLEntityName ';'
    ;

XMLEntityName
    : 'lt' | 'gt' | 'amp' | 'apos' | 'quot'
    ;

XMLCharacterReference
    : '&' '#' XMLNumericCharacter+ ';'
    | '&' '#' 'x' XMLHexNumericCharacter+ ';'
    ;
```

<span data-ttu-id="61fcb-3597">Ein XML-Element einen Wert, der als ergibt `System.Xml.Linq.XElement`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3597">An XML element results in a value typed as `System.Xml.Linq.XElement`.</span></span> <span data-ttu-id="61fcb-3598">Im Gegensatz zu regulären XML XML-Elemente können die Namen in das schließende Tag weglassen, und das aktuelle Element für die meisten geschachtelten wird geschlossen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3598">Unlike regular XML, XML elements can omit the name in the closing tag and the current most-nested element will be closed.</span></span> <span data-ttu-id="61fcb-3599">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3599">For example:</span></span>

```vb
Dim name = <name>Bob</>
```

<span data-ttu-id="61fcb-3600">Deklarationen in einer XML-Element-Ergebnis in Werte vom Typ Attribut `System.Xml.Linq.XAttribute`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3600">Attribute declarations in an XML element result in values typed as `System.Xml.Linq.XAttribute`.</span></span> <span data-ttu-id="61fcb-3601">Attributwerte werden gemäß der XML-Spezifikation normalisiert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3601">Attribute values are normalized according to the XML specification.</span></span> <span data-ttu-id="61fcb-3602">Wenn der Wert eines Attributs ist `Nothing` das Attribut nicht erstellt werden, sodass der Attribut-Wert-Ausdruck nicht überprüft werden soll `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3602">When the value of an attribute is `Nothing` the attribute will not be created, so the attribute value expression will not have to be checked for `Nothing`.</span></span> <span data-ttu-id="61fcb-3603">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3603">For example:</span></span>

```vb
Dim expr = Nothing

' Throws null argument exception
Dim direct = New System.Xml.Linq.XElement( _
    "Name", _
    New System.Xml.Linq.XAttribute("Length", expr))

' Doesn't throw exception, the result is <Name/>
Dim literal = <Name Length=<%= expr %>/>
```

<span data-ttu-id="61fcb-3604">XML-Elemente und Attribute können geschachtelte Ausdrücke in den folgenden Orten enthalten:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3604">XML elements and attributes can contain nested expressions in the following places:</span></span>

<span data-ttu-id="61fcb-3605">Der Name des Elements in diesem Fall den eingebetteten Ausdruck muss einen Wert eines Typs, der implizit in sein `System.Xml.Linq.XName`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3605">The name of the element, in which case the embedded expression must be a value of a type implicitly convertible to `System.Xml.Linq.XName`.</span></span> <span data-ttu-id="61fcb-3606">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3606">For example:</span></span>

```vb
Dim name = <<%= "name" %>>Bob</>
```

<span data-ttu-id="61fcb-3607">Der Name eines Attributs des Elements in diesem Fall den eingebetteten Ausdruck muss einen Wert eines Typs, der implizit in sein `System.Xml.Linq.XName`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3607">The name of an attribute of the element, in which case the embedded expression must be a value of a type implicitly convertible to `System.Xml.Linq.XName`.</span></span> <span data-ttu-id="61fcb-3608">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3608">For example:</span></span>

```vb
Dim name = <name <%= "length" %>="3">Bob</>
```

<span data-ttu-id="61fcb-3609">Der Wert eines Attributs des Elements in diesem Fall den eingebetteten Ausdruck einen Wert eines beliebigen Typs sein kann.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3609">The value of an attribute of the element, in which case the embedded expression can be a value of any type.</span></span> <span data-ttu-id="61fcb-3610">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3610">For example:</span></span>

```vb
Dim name = <name length=<%= 3 %>>Bob</>
```

<span data-ttu-id="61fcb-3611">Ein Attribut des Elements in diesem Fall den eingebetteten Ausdruck einen Wert eines beliebigen Typs sein kann.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3611">An attribute of the element, in which case the embedded expression can be a value of any type.</span></span> <span data-ttu-id="61fcb-3612">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3612">For example:</span></span>

```vb
Dim name = <name <%= new XAttribute("length", 3) %>>Bob</>
```

<span data-ttu-id="61fcb-3613">Der Inhalt des Elements in diesem Fall den eingebetteten Ausdruck einen Wert eines beliebigen Typs sein kann.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3613">The content of the element, in which case the embedded expression can be a value of any type.</span></span> <span data-ttu-id="61fcb-3614">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3614">For example:</span></span>

```vb
Dim name = <name><%= "Bob" %></>
```

<span data-ttu-id="61fcb-3615">Wenn der Typ des eingebetteten Ausdrucks `Object()`, wird das Array als ein Paramarray zum Übergeben der `XElement` Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3615">If the type of the embedded expression is `Object()`, the array will be passed as a paramarray to the `XElement` constructor.</span></span>


### <a name="xml-namespaces"></a><span data-ttu-id="61fcb-3616">XML-Namespaces</span><span class="sxs-lookup"><span data-stu-id="61fcb-3616">XML Namespaces</span></span>

<span data-ttu-id="61fcb-3617">XML-Elemente können XML-Namespace-Deklarationen, enthalten, wie in der XML-Namespaces-1.0-Spezifikation definiert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3617">XML elements can contain XML namespace declarations, as defined by the XML namespaces 1.0 specification.</span></span>

```antlr
XMLNamespaceAttributeName
    : XMLPrefixedNamespaceAttributeName
    | XMLDefaultNamespaceAttributeName
    ;

XMLPrefixedNamespaceAttributeName
    : 'xmlns' ':' XMLNamespaceName
    ;

XMLDefaultNamespaceAttributeName
    : 'xmlns'
    ;

XMLNamespaceName
    : XMLNamespaceNameStartCharacter XMLNamespaceNameCharacter*
    ;

XMLNamespaceNameStartCharacter
    : '<Any XMLNameCharacter except :>'
    ;

XMLNamespaceNameCharacter
    : XMLLetter
    | '_'
    ;

XMLQualifiedNameOrExpression
    : XMLQualifiedName
    | XMLEmbeddedExpression
    ;

XMLQualifiedName
    : XMLPrefixedName
    | XMLUnprefixedName
    ;

XMLPrefixedName
    : XMLNamespaceName ':' XMLNamespaceName
    ;

XMLUnprefixedName
    : XMLNamespaceName
    ;
```

<span data-ttu-id="61fcb-3618">Die Einschränkungen für die Namespaces definieren `xml` und `xmlns` werden erzwungen, und Fehler während der Kompilierung erzeugt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3618">The restrictions on defining the namespaces `xml` and `xmlns` are enforced and will produce compile-time errors.</span></span> <span data-ttu-id="61fcb-3619">XML-Namespacedeklaration dürfen keinen eingebetteten Ausdruck für ihren Wert besitzen. Der angegebene Wert muss es sich um ein nicht leeres Zeichenfolgenliteral sein.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3619">XML namespace declarations cannot have an embedded expression for their value; the value supplied must be a non-empty string literal.</span></span> <span data-ttu-id="61fcb-3620">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3620">For example:</span></span>

```vb
' Declares a valid namespace
Dim customer = <db:customer xmlns:db="http://example.org/database">Bob</>

' Error: xmlns cannot be re-defined
Dim bad1 = <elem xmlns:xmlns="http://example.org/namespace"/>

' Error: cannot have an embedded expression
Dim bad2 = <elem xmlns:db=<%= "http://example.org/database" %>>Bob</>
```

<span data-ttu-id="61fcb-3621">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3621">__Note.__</span></span> <span data-ttu-id="61fcb-3622">Diese Spezifikation enthält nur genug eine Beschreibung der XML-Namespace, um das Verhalten von Visual Basic-Sprache zu beschreiben.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3622">This specification contains only enough of a description of XML namespace to describe the behavior of the Visual Basic language.</span></span> <span data-ttu-id="61fcb-3623">Weitere Informationen zu XML-Namespaces finden Sie unter http://www.w3.org/TR/REC-xml-names/.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3623">More information on XML namespaces can be found at http://www.w3.org/TR/REC-xml-names/.</span></span>

<span data-ttu-id="61fcb-3624">XML-Namen für Element- und Attributnamen können mithilfe von Namespacenamen qualifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3624">XML element and attribute names can be qualified using namespace names.</span></span> <span data-ttu-id="61fcb-3625">Namespaces wie reguläre XML-Code gebunden sind, werden mit der Ausnahme, die keinem Namespace importiert, die auf Dateiebene deklariert deklariert werden, in einem Kontext einschließen der Deklaration, die selbst von alle Namespace-Importe deklariert, indem die Kompilierung eingeschlossen wird berücksichtigt Umgebung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3625">Namespaces are bound as in regular XML, with the exception that any namespace imports declared at the file level are considered to be declared in a context enclosing the declaration, which is itself enclosed by any namespace imports declared by the compilation environment.</span></span> <span data-ttu-id="61fcb-3626">Wenn ein Namespacename kann nicht gefunden werden, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3626">If a namespace name cannot be found, a compile-time error occurs.</span></span> <span data-ttu-id="61fcb-3627">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3627">For example:</span></span>

```vb
Imports System.Xml.Linq
Imports <xmlns:db="http://example.org/database">

Module Test
    Sub Main()
        ' Binds to the imported namespace above.
        Dim c1 = <db:customer>Bob</>

        ' Binds to the namespace declaration in the element
        Dim c2 = _
            <db:customer xmlns:db="http://example.org/database-other">Mary</>

        ' Binds to the inner namespace declaration
        Dim c3 = _
            <database xmlns:db="http://example.org/database-one">
                <db:customer xmlns:db="http://example.org/database-two">Joe</>
            </>

        ' Error: namespace db2 cannot be found
        Dim c4 = _
            <db2:customer>Jim</>
    End Sub
End Module
```

<span data-ttu-id="61fcb-3628">XML-Namespaces in einem Element deklariert gelten nicht für XML-Literale in eingebettete Ausdrücke.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3628">XML namespaces declared in an element do not apply to XML literals inside embedded expressions.</span></span> <span data-ttu-id="61fcb-3629">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3629">For example:</span></span>

```vb
' Error: Namespace prefix 'db' is not declared
Dim customer = _
    <db:customer xmlns:db="http://example.org/database">
        <%= <db:customer>Bob</> %>
    </>
```

<span data-ttu-id="61fcb-3630">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3630">__Note.__</span></span> <span data-ttu-id="61fcb-3631">Das liegt der eingebettete Ausdruck einschließlich eines Funktionsaufrufs sein kann.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3631">This is because the embedded expression can be anything, including a function call.</span></span> <span data-ttu-id="61fcb-3632">Wenn der Funktionsaufruf einen XML-Literalen Ausdruck enthalten, ist es nicht klar, ob die Programmierer erwarten den XML-Namespace, angewendet oder ignoriert werden soll.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3632">If the function call contained an XML literal expression, it is not clear whether programmers would expect the XML namespace to be applied or ignored.</span></span>


### <a name="xml-processing-instructions"></a><span data-ttu-id="61fcb-3633">XML-Verarbeitungsanweisungen</span><span class="sxs-lookup"><span data-stu-id="61fcb-3633">XML Processing Instructions</span></span>

<span data-ttu-id="61fcb-3634">Eine XML-verarbeitungsanweisung ergibt einen Wert, der als `System.Xml.Linq.XProcessingInstruction`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3634">An XML processing instruction results in a value typed as `System.Xml.Linq.XProcessingInstruction`.</span></span> <span data-ttu-id="61fcb-3635">XML-verarbeitungsanweisungen können nicht eingebettete Ausdrücke enthalten, da sie gültige Syntax in die verarbeitungsanweisung sind.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3635">XML processing instructions cannot contain embedded expressions, as they are valid syntax within the processing instruction.</span></span>

```antlr
XMLProcessingInstruction
    : '<' '?' XMLProcessingTarget ( XMLWhitespace XMLProcessingValue? )? '?' '>'
    ;

XMLProcessingTarget
    : '<Any XMLName except a casing permutation of the string "xml">'
    ;

XMLProcessingValue
    : '<Any XMLString that does not contain a question-mark followed by ">">'
    ;
```

### <a name="xml-comments"></a><span data-ttu-id="61fcb-3636">XML-Kommentare</span><span class="sxs-lookup"><span data-stu-id="61fcb-3636">XML Comments</span></span>

<span data-ttu-id="61fcb-3637">Ein XML-Kommentar ergibt einen Wert, der als `System.Xml.Linq.XComment`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3637">An XML comment results in a value typed as `System.Xml.Linq.XComment`.</span></span> <span data-ttu-id="61fcb-3638">XML-Kommentare können nicht eingebettete Ausdrücke enthalten, wie gültige Syntax innerhalb des Kommentars.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3638">XML comments cannot contain embedded expressions, as they are valid syntax within the comment.</span></span>

```antlr
XMLComment
    : '<' '!' '-' '-' XMLCommentCharacter* '-' '-' '>'
    ;

XMLCommentCharacter
    : '<Any XMLCharacter except dash (0x002D)>'
    | '-' '<Any XMLCharacter except dash (0x002D)>'
    ;
```

### <a name="cdata-sections"></a><span data-ttu-id="61fcb-3639">CDATA-Abschnitte</span><span class="sxs-lookup"><span data-stu-id="61fcb-3639">CDATA sections</span></span>

<span data-ttu-id="61fcb-3640">Ein CDATA-Abschnitt ergibt einen Wert, der als `System.Xml.Linq.XCData`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3640">A CDATA section results in a value typed as `System.Xml.Linq.XCData`.</span></span> <span data-ttu-id="61fcb-3641">CDATA-Abschnitte können nicht eingebettete Ausdrücke enthalten, wie sie gültige Syntax innerhalb des CDATA-Abschnitts sind.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3641">CDATA sections cannot contain embedded expressions, as they are valid syntax within the CDATA section.</span></span>

```antlr
XMLCDATASection
    : '<' '!' ( 'CDATA' '[' XMLCDATASectionString? ']' )? '>'
    ;

XMLCDATASectionString
    : '<Any XMLString that does not contain the string "]]>">'
    ;
```

## <a name="xml-member-access-expressions"></a><span data-ttu-id="61fcb-3642">XML-Memberzugriffsausdrücke</span><span class="sxs-lookup"><span data-stu-id="61fcb-3642">XML Member Access Expressions</span></span>

<span data-ttu-id="61fcb-3643">Ein XML-Memberzugriffsausdruck greift auf die Elemente eines XML-Werts.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3643">An XML member access expression accesses the members of an XML value.</span></span>

```antlr
XMLMemberAccessExpression
    : Expression '.' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? IdentifierOrKeyword
    | Expression '.' '.' '.' LineTerminator? '<' XMLQualifiedName '>'
    ;
```

<span data-ttu-id="61fcb-3644">Es gibt drei Arten von XML-Memberzugriffsausdrücke:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3644">There are three types of XML member access expressions:</span></span>

* <span data-ttu-id="61fcb-3645">*Elementzugriff*, in dem ein XML-Name einen einzigen Punkt folgt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3645">*Element access*, in which an XML name follows a single dot.</span></span> <span data-ttu-id="61fcb-3646">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3646">For example:</span></span>

  ```vb
  Dim customer = _
      <customer>
          <name>Bob</>
      </>
  Dim customerName = customer.<name>.Value
  ```

  <span data-ttu-id="61fcb-3647">Elementzugriff wird an die Funktion:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3647">Element access maps to the function:</span></span>

  ```vb
  Function Elements(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XNode)
  ```

  <span data-ttu-id="61fcb-3648">Damit das obige Beispiel entspricht:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3648">So the above example is equivalent to:</span></span>

  ```vb
  Dim customerName = customer.Elements("name").Value
  ```

* <span data-ttu-id="61fcb-3649">*Attribut Zugriff*, in dem ein Visual Basic-Bezeichner einen Punkt folgt und eine Anmeldung oder eine XML-Name einen Punkt folgt und ein at-Zeichen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3649">*Attribute access*, in which a Visual Basic identifier follows a dot and an at sign, or an XML name follows a dot and an at sign.</span></span> <span data-ttu-id="61fcb-3650">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3650">For example:</span></span>

  ```vb
  Dim customer = <customer age="30"/>
  Dim customerAge = customer.@age
  ```

  <span data-ttu-id="61fcb-3651">Attribut-Access-Karten für die Funktion:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3651">Attribute access maps to the function:</span></span>

  ```vb
  Function AttributeValue(name As System.Xml.Linq.XName) as String
  ```

  <span data-ttu-id="61fcb-3652">Damit das obige Beispiel entspricht:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3652">So the above example is equivalent to:</span></span>

  ```vb
  Dim customerAge = customer.AttributeValue("age")
  ```

  <span data-ttu-id="61fcb-3653">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3653">__Note.__</span></span> <span data-ttu-id="61fcb-3654">Die `AttributeValue` Erweiterungsmethode (sowie die zugehörigen Erweiterungseigenschaft `Value`) derzeit nicht in einer beliebigen Assembly definiert ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3654">The `AttributeValue` extension method (as well as the related extension property `Value`) is not currently defined in any assembly.</span></span> <span data-ttu-id="61fcb-3655">Wenn die Erweiterung Elemente erforderlich sind, werden sie automatisch in der Assembly erstellt wird definiert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3655">If the extension members are needed, they are automatically defined in the assembly being produced.</span></span>

* <span data-ttu-id="61fcb-3656">*Zugriff für Nachfolger*, in dem ein XML-Namen die drei Punkte folgt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3656">*Descendents access*, in which an XML names follows three dots.</span></span> <span data-ttu-id="61fcb-3657">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3657">For example:</span></span>

  ```vb
  Dim company = _
      <company>
          <customers>
              <customer>Bob</>
              <customer>Mary</>
              <customer>Joe</>
          </>
      </>
  Dim customers = company...<customer>
  ```

  <span data-ttu-id="61fcb-3658">Zugriff für Nachfolger ordnet die Funktion:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3658">Descendents access maps to the function:</span></span>

  ```vb
  Function Descendents(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XElement)
  ```

  <span data-ttu-id="61fcb-3659">Damit das obige Beispiel entspricht:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3659">So the above example is equivalent to:</span></span>

  ```vb
  Dim customers = company.Descendants("customer")
  ```

<span data-ttu-id="61fcb-3660">Der Basis einer XML-Memberzugriffsausdruck Ausdruck muss ein Wert sein und muss vom Typ sein:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3660">The base expression of an XML member access expression must be a value and must be of the type:</span></span>

* <span data-ttu-id="61fcb-3661">Wenn ein Element oder ein Nachfolger zugreifen zu können, `System.Xml.Linq.XContainer` oder eines abgeleiteten Typs oder `System.Collections.Generic.IEnumerable(Of T)` oder eines abgeleiteten Typs, in denen `T` ist `System.Xml.Linq.XContainer` oder einen abgeleiteten Typ.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3661">If an element or descendents access,  `System.Xml.Linq.XContainer` or a derived type, or `System.Collections.Generic.IEnumerable(Of T)` or a derived type, where `T` is `System.Xml.Linq.XContainer` or a derived type.</span></span>

* <span data-ttu-id="61fcb-3662">Wenn einer Zugriff auf das Attribut `System.Xml.Linq.XElement` oder eines abgeleiteten Typs oder `System.Collections.Generic.IEnumerable(Of T)` oder eines abgeleiteten Typs, in denen `T` ist `System.Xml.Linq.XElement` oder einen abgeleiteten Typ.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3662">If an attribute access, `System.Xml.Linq.XElement` or a derived type, or `System.Collections.Generic.IEnumerable(Of T)` or a derived type, where `T` is `System.Xml.Linq.XElement` or a derived type.</span></span>

<span data-ttu-id="61fcb-3663">Namen in XML-Memberzugriffsausdrücke darf nicht leer sein.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3663">Names in XML member access expressions cannot be empty.</span></span> <span data-ttu-id="61fcb-3664">Sie können den Namespace qualifiziert, mit der alle Namespaces, die durch Importe definiert werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3664">They can be namespace qualified, using any namespaces defined by imports.</span></span> <span data-ttu-id="61fcb-3665">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3665">For example:</span></span>

```vb
Imports <xmlns:db="http://example.org/database">

Module Test
    Sub Main()
        Dim customer = _
            <db:customer>
                <db:name>Bob</>
            </>
        Dim name = customer.<db:name>
    End Sub
End Module
```

<span data-ttu-id="61fcb-3666">Nach der Dot(s) in eine XML-Memberzugriffsausdruck oder zwischen den spitzen Klammern und dem Namen sind keine Leerzeichen zugelassen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3666">Whitespace is not allowed after the dot(s) in an XML member access expression, or between the angle brackets and the name.</span></span> <span data-ttu-id="61fcb-3667">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3667">For example:</span></span>

```vb
Dim customer = _
    <customer age="30">
        <name>Bob</>
    </>
' All the following are error cases
Dim age = customer.@ age
Dim name = customer.< name >
Dim names = customer...< name >
```

<span data-ttu-id="61fcb-3668">Wenn die Typen in der `System.Xml.Linq` Namespace sind nicht verfügbar, und klicken Sie dann eine XML-Memberzugriffsausdruck führt dazu, dass einen Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3668">If the types in the `System.Xml.Linq` namespace are not available, then an XML member access expression will cause a compile-time error.</span></span>


## <a name="await-operator"></a><span data-ttu-id="61fcb-3669">Await-Operator</span><span class="sxs-lookup"><span data-stu-id="61fcb-3669">Await Operator</span></span>

<span data-ttu-id="61fcb-3670">Der Operator "await" bezieht sich auf asynchrone Methoden, die im Abschnitt beschriebenen [Async-Methoden](statements.md#async-methods).</span><span class="sxs-lookup"><span data-stu-id="61fcb-3670">The await operator is related to async methods, which are described in Section [Async Methods](statements.md#async-methods).</span></span>

```antlr
AwaitOperatorExpression
    : 'Await' Expression
    ;
```

<span data-ttu-id="61fcb-3671">`Await` ist ein reserviertes Wort, verfügt die unmittelbar einschließende Methode oder der Lambda-Ausdruck, in dem er angezeigt wird, ein `Async` Modifizierer, und wenn die `Await` angezeigt wird, `Async` Modifizierer; es an anderer Stelle nicht reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3671">`Await` is a reserved word if the immediately enclosing method or lambda expression in which it appears has an `Async` modifier, and if the `Await` appears after that `Async` modifier; it is unreserved elsewhere.</span></span> <span data-ttu-id="61fcb-3672">Es ist auch in präprozessoranweisungen nicht reserviert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3672">It is also unreserved in preprocessor directives.</span></span> <span data-ttu-id="61fcb-3673">Der Await-Operator darf nur im Text einer Methode oder einem Lambda-Ausdrücke, die, in denen es sich um ein reserviertes Wort ist.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3673">The await operator is only allowed in the body of a method or lambda expressions where it is a reserved word.</span></span> <span data-ttu-id="61fcb-3674">In der unmittelbar einschließenden Methode oder einem Lambdaausdruck, ein Await-Ausdruck kann nicht auftreten, innerhalb des Texts einer `Catch` oder `Finally` Block noder innerhalb der Text der ein `SyncLock` Anweisung noch innerhalb eines Abfrageausdrucks.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3674">Within the immediately enclosing method or lambda, an await expression may not occur inside the body of a `Catch` or `Finally` block, nor inside the body of a `SyncLock` statement, nor inside a query expression.</span></span>

<span data-ttu-id="61fcb-3675">Der Operator "await" nimmt einen einzelnen Ausdruck, der als Wert klassifiziert werden muss und, dessen Typ muss, eine *awaitable* Typ oder `Object`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3675">The await operator takes a single expression which must be classified as a value and whose type must be an *awaitable* type, or `Object`.</span></span> <span data-ttu-id="61fcb-3676">Wenn der Typ ist `Object` und klicken Sie dann die gesamte Verarbeitung bis zur Laufzeit verzögert wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3676">If its type is `Object` then all processing is deferred until run-time.</span></span> <span data-ttu-id="61fcb-3677">Ein Typ `C` gilt als "awaitable", wenn alle der folgenden Bedingungen erfüllt sind:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3677">A type `C` is said to be awaitable if all of the following are true:</span></span>

* <span data-ttu-id="61fcb-3678">`C` enthält eine zugängliche Instanz- oder Erweiterungsmethode-Methode, mit dem Namen `GetAwaiter` die verfügt über keine Argumente und womit eine Art `E`;</span><span class="sxs-lookup"><span data-stu-id="61fcb-3678">`C` contains an accessible instance or extension method named `GetAwaiter` which has no arguments and which returns some type `E`;</span></span>

* <span data-ttu-id="61fcb-3679">`E` enthält eine lesbare Instanz- oder Erweiterungsmethode-Eigenschaft, mit dem Namen `IsCompleted` der akzeptiert keine Argumente und weist den Typ Boolean;</span><span class="sxs-lookup"><span data-stu-id="61fcb-3679">`E` contains a readable instance or extension property named `IsCompleted` which takes no arguments and has type Boolean;</span></span>

* <span data-ttu-id="61fcb-3680">`E` enthält eine zugängliche Instanz- oder Erweiterungsmethode-Methode, mit dem Namen `GetResult` der akzeptiert keine Argumente.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3680">`E` contains an accessible instance or extension method named `GetResult` which takes no arguments;</span></span>

* <span data-ttu-id="61fcb-3681">`E` implementiert eine `System.Runtime.CompilerServices.INotifyCompletion` oder `ICriticalNotifyCompletion`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3681">`E` implements either `System.Runtime.CompilerServices.INotifyCompletion` or `ICriticalNotifyCompletion`.</span></span>

<span data-ttu-id="61fcb-3682">Wenn `GetResult` wurde eine `Sub`, und klicken Sie dann die "await"-Ausdruck als "void" eingestuft wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3682">If `GetResult` was a `Sub`, then the await expression is classified as void.</span></span> <span data-ttu-id="61fcb-3683">Andernfalls wird der Await-Ausdruck als Wert klassifiziert und sein Typ ist der Rückgabetyp der der `GetResult` Methode.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3683">Otherwise, the await expression is classified as a value and its type is the return type of the `GetResult` method.</span></span>

<span data-ttu-id="61fcb-3684">Hier ist ein Beispiel für eine Klasse, die erwartet werden kann:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3684">Here is an example of a class that can be awaited:</span></span>

```vb
Class MyTask(Of T)
    Function GetAwaiter() As MyTaskAwaiter(Of T)
        Return New MyTaskAwaiter With {.m_Task = Me}
    End Function

    ...
End Class

Structure MyTaskAwaiter(Of T)
    Implements INotifyCompletion

    Friend m_Task As MyTask(Of T)

    ReadOnly Property IsCompleted As Boolean
        Get
            Return m_Task.IsCompleted
        End Get
    End Property

    Sub OnCompleted(r As Action) Implements INotifyCompletion.OnCompleted
        ' r is the "resumptionDelegate"
        Dim sc = SynchronizationContext.Current
        If sc Is Nothing Then
            m_Task.ContinueWith(Sub() r())
        Else
            m_Task.ContinueWith(Sub() sc.Post(Sub() r(), Nothing))
        End If
    End Sub

    Function GetResult() As T
        If m_Task.IsCanceled Then Throw New TaskCanceledException(m_Task)
        If m_Task.IsFaulted Then Throw m_Task.Exception.InnerException
        Return m_Task.Result
    End Function
End Structure
```

<span data-ttu-id="61fcb-3685">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="61fcb-3685">__Note.__</span></span> <span data-ttu-id="61fcb-3686">Autoren von Klassenbibliotheken werden empfohlen, die dem Muster folgen, bereit, die sie auf dem gleichen Fortsetzungsdelegaten `SynchronizationContext` als ihre `OnCompleted` selbst auf aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3686">Library authors are recommended to follow the pattern that they invoke the continuation delegate on the same `SynchronizationContext` as their `OnCompleted` was itself invoked on.</span></span> <span data-ttu-id="61fcb-3687">Darüber hinaus der Wiederaufnahmedelegat sollte nicht ausgeführt werden synchron innerhalb der `OnCompleted` Methode, da, die auf einen Stapelüberlauf führen kann: stattdessen den Delegat sollte in der Warteschlange zur nachfolgenden Ausführung.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3687">Also, the resumption delegate should not be executed synchronously within the `OnCompleted` method since that can lead to stack overflow: instead, the delegate should be queued for subsequent execution.</span></span>

<span data-ttu-id="61fcb-3688">Wenn Steuerung Flow erreicht eine `Await` Operator Verhalten lautet wie folgt.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3688">When control flow reaches an `Await` operator, behavior is as follows.</span></span>

1.  <span data-ttu-id="61fcb-3689">Die `GetAwaiter` Methode des "await"-Operanden wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3689">The `GetAwaiter` method of the await operand is invoked.</span></span> <span data-ttu-id="61fcb-3690">Das Ergebnis dieser Aufruf wird als bezeichnet die *"awaiter"*.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3690">The result of this invocation is termed the *awaiter*.</span></span>

2.  <span data-ttu-id="61fcb-3691">Der "awaiter" `IsCompleted` Eigenschaft wird abgerufen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3691">The awaiter's `IsCompleted` property is retrieved.</span></span> <span data-ttu-id="61fcb-3692">Wenn das Ergebnis "true" ist dann:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3692">If the result is true then:</span></span>

    21. <span data-ttu-id="61fcb-3693">Die `GetResult` Methode von der "awaiter" wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3693">The `GetResult` method of the awaiter is invoked.</span></span> <span data-ttu-id="61fcb-3694">Wenn `GetResult` war dies eine Funktion, und klicken Sie dann der Wert des der Await-Ausdruck dieser Funktion zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3694">If `GetResult` was a function, then the value of the await expression is the return value of this function.</span></span>

3.  <span data-ttu-id="61fcb-3695">Wenn die IsCompleted-Eigenschaft nicht "true" ist dann:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3695">If the IsCompleted property isn't true then:</span></span>

    31. <span data-ttu-id="61fcb-3696">Entweder `ICriticalNotifyCompletion.UnsafeOnCompleted` im "awaiter" aufgerufen wird (wenn der "awaiter"-Typ `E` implementiert `ICriticalNotifyCompletion`) oder `INotifyCompletion.OnCompleted` (andernfalls).</span><span class="sxs-lookup"><span data-stu-id="61fcb-3696">Either `ICriticalNotifyCompletion.UnsafeOnCompleted` is invoked on the awaiter (if the awaiter's type `E` implements `ICriticalNotifyCompletion`) or `INotifyCompletion.OnCompleted` (otherwise).</span></span> <span data-ttu-id="61fcb-3697">In beiden Fällen wird es übergibt eine *Wiederaufnahmedelegat* verknüpft ist, mit der aktuellen Instanz der Async-Methode.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3697">In both cases it passes a *resumption delegate* associated with the current instance of the async method.</span></span>

    32. <span data-ttu-id="61fcb-3698">Der Kontrollpunkt der aktuellen Instanz der Async-Methode wird angehalten, und Steuern der Flow wird fortgesetzt, in der *aktuellen Aufrufer* (im Abschnitt definiert [Async-Methoden](statements.md#async-methods)).</span><span class="sxs-lookup"><span data-stu-id="61fcb-3698">The control point of the current async method instance is suspended, and control flow resumes in the *current caller* (defined in Section [Async Methods](statements.md#async-methods)).</span></span>

    33. <span data-ttu-id="61fcb-3699">Wenn später der Wiederaufnahmedelegat aufgerufen wird,</span><span class="sxs-lookup"><span data-stu-id="61fcb-3699">If later the resumption delegate is invoked,</span></span>

        331. <span data-ttu-id="61fcb-3700">zuerst stellt der Wiederaufnahmedelegat wieder her `System.Threading.Thread.CurrentThread.ExecutionContext` war zum Zeitpunkt `OnCompleted` aufgerufen wurde,</span><span class="sxs-lookup"><span data-stu-id="61fcb-3700">the resumption delegate first restores `System.Threading.Thread.CurrentThread.ExecutionContext` to what it was at the time `OnCompleted` was called,</span></span>
        332. <span data-ttu-id="61fcb-3701">und dann die ablaufsteuerung an den Kontrollpunkt der asynchronen Methodeninstanz fortgesetzt (siehe Abschnitt [Async-Methoden](statements.md#async-methods)),</span><span class="sxs-lookup"><span data-stu-id="61fcb-3701">then it resumes control flow at the control point of the async method instance (see Section [Async Methods](statements.md#async-methods)),</span></span>
        333. <span data-ttu-id="61fcb-3702">Ruft, in dem die `GetResult` Methode der "awaiter", wie in der oben genannten 2.1.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3702">where it calls the `GetResult` method of the awaiter, as in 2.1 above.</span></span>

<span data-ttu-id="61fcb-3703">Wenn der Operand des "await" Typs "Object" aufweist, wird dieses Verhalten bis zur Laufzeit verzögert:</span><span class="sxs-lookup"><span data-stu-id="61fcb-3703">If the await operand has type Object, then this behavior is deferred until runtime:</span></span>

- <span data-ttu-id="61fcb-3704">Schritt 1 wird durch die aufrufende GetAwaiter() ohne Argumente erreicht; kann daher zur Laufzeit, um Instanzmethoden bindet, bindet es die optionalen Parameter zu akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3704">Step 1 is accomplished by calling GetAwaiter() with no arguments; it may therefore bind at runtime to instance methods which take optional parameters.</span></span>
- <span data-ttu-id="61fcb-3705">Schritt 2 erfolgt durch Abrufen der Eigenschaft IsCompleted() ohne Argumente und es wird versucht, eine Konvertierung in einen booleschen Wert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3705">Step 2 is accomplished by retrieving the IsCompleted() property with no arguments, and by attempting an intrinsic conversion to Boolean.</span></span>
- <span data-ttu-id="61fcb-3706">Schritt 3.a wird erreicht, indem Sie versuchen, `TryCast(awaiter, ICriticalNotifyCompletion)`, wenn auch dieser dann `DirectCast(awaiter, INotifyCompletion)`.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3706">Step 3.a is accomplished by attempting `TryCast(awaiter, ICriticalNotifyCompletion)`, and if this fails then `DirectCast(awaiter, INotifyCompletion)`.</span></span>

<span data-ttu-id="61fcb-3707">Der Wiederaufnahmedelegat 3.a übergeben kann nur einmal aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3707">The resumption delegate passed in 3.a may only be invoked once.</span></span> <span data-ttu-id="61fcb-3708">Wenn sie mehr als einmal aufgerufen wird, ist das Verhalten nicht definiert.</span><span class="sxs-lookup"><span data-stu-id="61fcb-3708">If it is invoked more than once, the behavior is undefined.</span></span>

