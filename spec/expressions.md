---
ms.openlocfilehash: 8f250f8cee957fa60e7970ace250e8d7ce145fd7
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306170"
---
# <a name="expressions"></a><span data-ttu-id="87dba-101">Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-101">Expressions</span></span>

<span data-ttu-id="87dba-102">Ein Ausdruck ist eine Sequenz von Operatoren und Operanden, die eine Berechnung eines Werts angibt oder eine Variable oder Konstante bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="87dba-102">An expression is a sequence of operators and operands that specifies a computation of a value, or that designates a variable or constant.</span></span> <span data-ttu-id="87dba-103">In diesem Kapitel werden die Syntax, die Reihenfolge der Auswertung von Operanden und Operatoren sowie die Bedeutung von Ausdrücken definiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-103">This chapter defines the syntax, order of evaluation of operands and operators, and meaning of expressions.</span></span>

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

## <a name="expression-classifications"></a><span data-ttu-id="87dba-104">Ausdrucksklassifizierungen</span><span class="sxs-lookup"><span data-stu-id="87dba-104">Expression Classifications</span></span>

<span data-ttu-id="87dba-105">Jeder Ausdruck wird als einer der folgenden klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="87dba-105">Every expression is classified as one of the following:</span></span>

* <span data-ttu-id="87dba-106">*Ein-Wert.*</span><span class="sxs-lookup"><span data-stu-id="87dba-106">*A value.*</span></span> <span data-ttu-id="87dba-107">Jeder Wert verfügt über einen zugeordneten Typ.</span><span class="sxs-lookup"><span data-stu-id="87dba-107">Every value has an associated type.</span></span>

* <span data-ttu-id="87dba-108">*Eine Variable.*</span><span class="sxs-lookup"><span data-stu-id="87dba-108">*A variable.*</span></span> <span data-ttu-id="87dba-109">Jede Variable verfügt über einen zugeordneten Typ, nämlich den deklarierten Typ der Variablen.</span><span class="sxs-lookup"><span data-stu-id="87dba-109">Every variable has an associated type, namely the declared type of the variable.</span></span>

* <span data-ttu-id="87dba-110">*Ein Namespace.*</span><span class="sxs-lookup"><span data-stu-id="87dba-110">*A namespace.*</span></span> <span data-ttu-id="87dba-111">Ein Ausdruck mit dieser Klassifizierung kann nur als linke Seite eines Element Zugriffs angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-111">An expression with this classification can only appear as the left side of a member access.</span></span> <span data-ttu-id="87dba-112">In jedem anderen Kontext verursacht ein Ausdruck, der als Namespace klassifiziert ist, einen Kompilierzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="87dba-112">In any other context, an expression classified as a namespace causes a compile-time error.</span></span>

* <span data-ttu-id="87dba-113">*Ein-Typ.*</span><span class="sxs-lookup"><span data-stu-id="87dba-113">*A type.*</span></span> <span data-ttu-id="87dba-114">Ein Ausdruck mit dieser Klassifizierung kann nur als linke Seite eines Element Zugriffs angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-114">An expression with this classification can only appear as the left side of a member access.</span></span> <span data-ttu-id="87dba-115">In jedem anderen Kontext verursacht ein Ausdruck, der als Typ klassifiziert ist, einen Kompilierzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="87dba-115">In any other context, an expression classified as a type causes a compile-time error.</span></span>

* <span data-ttu-id="87dba-116">*Eine Methoden Gruppe,* bei der es sich um einen Satz von Methoden handelt, die mit demselben Namen überladen werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-116">*A method group,* which is a set of methods overloaded on the same name.</span></span> <span data-ttu-id="87dba-117">Eine Methoden Gruppe kann über einen zugeordneten Ziel Ausdruck und eine zugeordnete Typargument Liste verfügen.</span><span class="sxs-lookup"><span data-stu-id="87dba-117">A method group may have an associated target expression and an associated type argument list.</span></span>

* <span data-ttu-id="87dba-118">*Ein Methoden Zeiger,* der den Speicherort einer Methode darstellt.</span><span class="sxs-lookup"><span data-stu-id="87dba-118">*A method pointer,* which represents the location of a method.</span></span> <span data-ttu-id="87dba-119">Einem Methoden Zeiger können ein zugeordneter Ziel Ausdruck und eine zugeordnete Typargument Liste zugeordnet sein.</span><span class="sxs-lookup"><span data-stu-id="87dba-119">A method pointer may have an associated target expression and an associated type argument list.</span></span>

* <span data-ttu-id="87dba-120">*Eine Lambda-Methode,* bei der es sich um eine anonyme Methode handelt.</span><span class="sxs-lookup"><span data-stu-id="87dba-120">*A lambda method,* which is an anonymous method.</span></span>

* <span data-ttu-id="87dba-121">*Eine Eigenschaften Gruppe,* bei der es sich um einen Satz von Eigenschaften handelt, die mit demselben Namen überladen werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-121">*A property group,* which is a set of properties overloaded on the same name.</span></span> <span data-ttu-id="87dba-122">Einer Eigenschaften Gruppe kann ein zugeordneter Ziel Ausdruck zugeordnet sein.</span><span class="sxs-lookup"><span data-stu-id="87dba-122">A property group may have an associated target expression.</span></span>

* <span data-ttu-id="87dba-123">*Ein Eigenschaften Zugriff.*</span><span class="sxs-lookup"><span data-stu-id="87dba-123">*A property access.*</span></span> <span data-ttu-id="87dba-124">Jeder Eigenschaften Zugriff verfügt über einen zugeordneten Typ, nämlich den Typ der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="87dba-124">Every property access has an associated type, namely the type of the property.</span></span> <span data-ttu-id="87dba-125">Ein Eigenschaften Zugriff kann über einen zugeordneten Ziel Ausdruck verfügen.</span><span class="sxs-lookup"><span data-stu-id="87dba-125">A property access may have an associated target expression.</span></span>

* <span data-ttu-id="87dba-126">*Ein spät gebundener Zugriff,* der den Zugriff auf eine Methode oder Eigenschaft darstellt, der bis zur Laufzeit verzögert wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-126">*A late-bound access,* which represents a method or property access deferred until run-time.</span></span> <span data-ttu-id="87dba-127">Einem spät gebundenen Zugriff können ein zugeordneter Ziel Ausdruck und eine zugeordnete Typargument Liste zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-127">A late-bound access may have an associated target expression and an associated type argument list.</span></span> <span data-ttu-id="87dba-128">Der Typ eines spät gebundenen Zugriffs ist immer `Object`.</span><span class="sxs-lookup"><span data-stu-id="87dba-128">The type of a late-bound access is always `Object`.</span></span>

* <span data-ttu-id="87dba-129">*Ein Ereignis Zugriff.*</span><span class="sxs-lookup"><span data-stu-id="87dba-129">*An event access.*</span></span> <span data-ttu-id="87dba-130">Jedem Ereignis Zugriff ist ein Typ zugeordnet, nämlich der Typ des Ereignisses.</span><span class="sxs-lookup"><span data-stu-id="87dba-130">Every event access has an associated type, namely the type of the event.</span></span> <span data-ttu-id="87dba-131">Ein Ereignis Zugriff kann über einen zugeordneten Ziel Ausdruck verfügen.</span><span class="sxs-lookup"><span data-stu-id="87dba-131">An event access may have an associated target expression.</span></span> <span data-ttu-id="87dba-132">Ein Ereignis Zugriff wird möglicherweise als erstes Argument der Anweisungen `RaiseEvent`, `AddHandler` und `RemoveHandler` angezeigt.</span><span class="sxs-lookup"><span data-stu-id="87dba-132">An event access may appear as the first argument of the `RaiseEvent`, `AddHandler`, and `RemoveHandler` statements.</span></span> <span data-ttu-id="87dba-133">In jedem anderen Kontext verursacht ein Ausdruck, der als Ereignis Zugriff klassifiziert ist, einen Kompilierzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="87dba-133">In any other context, an expression classified as an event access causes a compile-time error.</span></span>

* <span data-ttu-id="87dba-134">*Ein arrayliteralwert,* der die Anfangswerte eines Arrays darstellt, dessen Typ noch nicht bestimmt wurde.</span><span class="sxs-lookup"><span data-stu-id="87dba-134">*An array literal,* which represents the initial values of an array whose type has not yet been determined.</span></span>

* <span data-ttu-id="87dba-135">*Blutung.*</span><span class="sxs-lookup"><span data-stu-id="87dba-135">*Void.*</span></span> <span data-ttu-id="87dba-136">Dies tritt auf, wenn der Ausdruck ein Aufruf einer Unterroutine oder ein Erwartungs Operator Ausdruck ohne Ergebnis ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-136">This occurs when the expression is an invocation of a subroutine, or an await operator expression with no result.</span></span> <span data-ttu-id="87dba-137">Ein als void klassifiziert Ausdruck ist nur im Kontext einer Aufruf Anweisung oder einer Erwartungs Anweisung gültig.</span><span class="sxs-lookup"><span data-stu-id="87dba-137">An expression classified as void is only valid in the context of an invocation statement or an await statement.</span></span>

* <span data-ttu-id="87dba-138">*Ein Standardwert.*</span><span class="sxs-lookup"><span data-stu-id="87dba-138">*A default value.*</span></span> <span data-ttu-id="87dba-139">Nur die Literale `Nothing` erzeugt diese Klassifizierung.</span><span class="sxs-lookup"><span data-stu-id="87dba-139">Only the literal `Nothing` produces this classification.</span></span>

<span data-ttu-id="87dba-140">Das Endergebnis eines Ausdrucks ist in der Regel ein Wert oder eine Variable, wobei die anderen Kategorien von Ausdrücken als Zwischenwerte fungieren, die nur in bestimmten Kontexten zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="87dba-140">The final result of an expression is usually a value or a variable, with the other categories of expressions functioning as intermediate values that are only permitted in certain contexts.</span></span>

<span data-ttu-id="87dba-141">Beachten Sie, dass Ausdrücke, deren Typ ein Typparameter ist, in Anweisungen und Ausdrücken verwendet werden können, die erfordern, dass der Typ eines Ausdrucks bestimmte Merkmale aufweist (z. b. als Verweistyp, Werttyp, ableiten von einem Typ usw.), wenn die Einschränkungen auferlegt werden. Diese Eigenschaften werden vom Typparameter erfüllt.</span><span class="sxs-lookup"><span data-stu-id="87dba-141">Note that expressions whose type is a type parameter can be used in statements and expressions that require the type of an expression to have certain characteristics (such as being a reference type, value type, deriving from some type, etc.) if the constraints imposed on the type parameter satisfy those characteristics.</span></span>

### <a name="expression-reclassification"></a><span data-ttu-id="87dba-142">Neuklassifizierung von Ausdrücken</span><span class="sxs-lookup"><span data-stu-id="87dba-142">Expression Reclassification</span></span>

<span data-ttu-id="87dba-143">Normalerweise tritt ein Kompilierzeitfehler auf, wenn ein Ausdruck in einem Kontext verwendet wird, der eine Klassifizierung erfordert, die sich von der des Ausdrucks unterscheidet, z. b. wenn versucht wird, einen Wert einem Literalwert zuzuweisen.</span><span class="sxs-lookup"><span data-stu-id="87dba-143">Normally, when an expression is used in a context that requires a classification different from that of the expression, a compile-time error occurs -- for example, attempting to assign a value to a literal.</span></span> <span data-ttu-id="87dba-144">In vielen Fällen ist es jedoch möglich, die Klassifizierung eines Ausdrucks durch den Prozess der *Neuklassifizierung*zu ändern.</span><span class="sxs-lookup"><span data-stu-id="87dba-144">However, in many cases it is possible to change an expression's classification through the process of *reclassification*.</span></span>

<span data-ttu-id="87dba-145">Wenn die Neuklassifizierung erfolgreich ist, wird die Neuklassifizierung als Erweiterung oder Einschränkung bewertet.</span><span class="sxs-lookup"><span data-stu-id="87dba-145">If reclassification succeeds, then the reclassification is judged as widening or narrowing.</span></span> <span data-ttu-id="87dba-146">Sofern nicht anders angegeben, werden alle Neuklassifizierungen in dieser Liste erweitert.</span><span class="sxs-lookup"><span data-stu-id="87dba-146">Unless otherwise noted, all the reclassifications in this list are widening.</span></span>

<span data-ttu-id="87dba-147">Die folgenden Typen von Ausdrücken können neu klassifiziert werden:</span><span class="sxs-lookup"><span data-stu-id="87dba-147">The following types of expressions can be reclassified:</span></span>

* <span data-ttu-id="87dba-148">Eine Variable kann als Wert neu klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-148">A variable can be reclassified as a value.</span></span> <span data-ttu-id="87dba-149">Der in der Variable gespeicherte Wert wird abgerufen.</span><span class="sxs-lookup"><span data-stu-id="87dba-149">The value stored in the variable is fetched.</span></span>

* <span data-ttu-id="87dba-150">Eine Methoden Gruppe kann als Wert neu klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-150">A method group can be reclassified as a value.</span></span> <span data-ttu-id="87dba-151">Der Methoden Gruppen Ausdruck wird als Aufruf Ausdruck mit dem zugeordneten Ziel Ausdruck und der Typparameter Liste und leeren Klammern interpretiert (d. h. `f` wird als `f()` interpretiert, und `f(Of Integer)` wird als `f(Of Integer)()` interpretiert).</span><span class="sxs-lookup"><span data-stu-id="87dba-151">The method group expression is interpreted as an invocation expression with the associated target expression and type parameter list, and empty parentheses (that is, `f` is interpreted as `f()` and `f(Of Integer)` is interpreted as `f(Of Integer)()`).</span></span> <span data-ttu-id="87dba-152">Diese Neuklassifizierung kann dazu führen, dass der Ausdruck weiter als void neu klassifiziert wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-152">This reclassification may result in the expression being further reclassified as void.</span></span>

* <span data-ttu-id="87dba-153">Ein Methoden Zeiger kann als Wert neu klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-153">A method pointer can be reclassified as a value.</span></span> <span data-ttu-id="87dba-154">Diese Neuklassifizierung kann nur im Kontext einer Konvertierung auftreten, bei der der Zieltyp bekannt ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-154">This reclassification can only occur in the context of a conversion where the target type is known.</span></span> <span data-ttu-id="87dba-155">Der Methoden Zeiger Ausdruck wird als Argument für einen delegatinstanzizierungsausdruck des entsprechenden Typs mit der zugeordneten Typargument Liste interpretiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-155">The method pointer expression is interpreted as the argument to a delegate instantiation expression of the appropriate type with the associated type argument list.</span></span> <span data-ttu-id="87dba-156">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-156">For example:</span></span>
    
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

* <span data-ttu-id="87dba-157">Eine Lambda-Methode kann als Wert neu klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-157">A lambda method can be reclassified as a value.</span></span> <span data-ttu-id="87dba-158">Wenn die Neuklassifizierung im Kontext einer Konvertierung erfolgt, bei der der Zieltyp bekannt ist, kann eine von zwei Neuklassifizierungen auftreten:</span><span class="sxs-lookup"><span data-stu-id="87dba-158">If the reclassification occurs in the context of a conversion where the target type is known, then one of two reclassifications can occur:</span></span>
    
  1. <span data-ttu-id="87dba-159">Wenn der Zieltyp ein Delegattyp ist, wird die Lambda-Methode als Argument für einen delegatkonstruktions-Ausdruck des entsprechenden Typs interpretiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-159">If the target type is a delegate type, the lambda method is interpreted as the argument to a delegate-construction expression of the appropriate type.</span></span>
    
  2. <span data-ttu-id="87dba-160">Wenn der Zieltyp `System.Linq.Expressions.Expression(Of T)` und `T` ein Delegattyp ist, wird die Lambda-Methode so interpretiert, als ob Sie im delegaterstellungs-Ausdruck für `T` verwendet und anschließend in eine Ausdrucks Baumstruktur konvertiert wurde.</span><span class="sxs-lookup"><span data-stu-id="87dba-160">If the target type is `System.Linq.Expressions.Expression(Of T)`, and `T` is a delegate type, then the lambda method is interpreted as if it was being used in delegate-construction expression for `T` and then converted to an expression tree.</span></span>
    
  <span data-ttu-id="87dba-161">Eine Async-oder Iterator-Lambda-Methode kann nur als Argument für einen delegatkonstruktions-Ausdruck interpretiert werden, wenn der Delegat keine ByRef-Parameter aufweist.</span><span class="sxs-lookup"><span data-stu-id="87dba-161">An async or iterator lambda method may only be interpreted as the argument to a delegate-construction expression, if the delegate has no ByRef parameters.</span></span>
    
  <span data-ttu-id="87dba-162">Wenn die Konvertierung von einem der Parametertypen eines Delegaten in die entsprechenden Lambda-Parametertypen eine einschränkende Konvertierung ist, wird die Neuklassifizierung als Einschränkungs Methode bewertet. Andernfalls wird Sie erweitert.</span><span class="sxs-lookup"><span data-stu-id="87dba-162">If conversion from any of the delegate's parameter types to the corresponding lambda parameter types is a narrowing conversion, then the reclassification is judged as narrowing; otherwise it is widening.</span></span>
    
  <span data-ttu-id="87dba-163">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-163">__Note.__</span></span> <span data-ttu-id="87dba-164">Die genaue Übersetzung zwischen Lambda-Methoden und Ausdrucks Baumstrukturen wird möglicherweise nicht Zwischenversionen des Compilers korrigiert und geht über den Rahmen dieser Spezifikation hinaus.</span><span class="sxs-lookup"><span data-stu-id="87dba-164">The exact translation between lambda methods and expression trees may not be fixed between versions of the compiler and is beyond the scope of this specification.</span></span> <span data-ttu-id="87dba-165">Für Microsoft Visual Basic 11,0 können alle Lambda-Ausdrücke in Ausdrucks Baumstrukturen konvertiert werden, die den folgenden Einschränkungen unterliegen: (1) 1.</span><span class="sxs-lookup"><span data-stu-id="87dba-165">For Microsoft Visual Basic 11.0, all lambda expressions may be converted to expression trees subject to the following restrictions: (1) 1.</span></span>  <span data-ttu-id="87dba-166">Nur einzeilige Lambda-Ausdrücke ohne ByRef-Parameter können in Ausdrucks Baumstrukturen konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-166">Only single-line lambda expressions without ByRef parameters may be converted to expression trees.</span></span> <span data-ttu-id="87dba-167">Der einzeiligen `Sub`-Lambdas können nur Aufruf Anweisungen in Ausdrucks Baumstrukturen konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-167">Of the single-line `Sub` lambdas, only invocation statements may be converted to expression trees.</span></span> <span data-ttu-id="87dba-168">(2) anonyme typausdrücke können nicht in Ausdrucks Baumstrukturen konvertiert werden, wenn ein früherer Feldinitialisierer verwendet wird, um einen nachfolgenden Feldinitialisierer zu initialisieren, z. b. `New With {.a=1, .b=.a}`.</span><span class="sxs-lookup"><span data-stu-id="87dba-168">(2) Anonymous type expressions cannot be converted to expression trees if an earlier field initializer is used to initialize a subsequent field initializer, e.g. `New With {.a=1, .b=.a}`.</span></span> <span data-ttu-id="87dba-169">(3) objektinitialisiererausdrücke können nicht in Ausdrucks Baumstrukturen konvertiert werden, wenn ein Member des aktuellen Objekts, das initialisiert wird, in einem der Feldinitialisierer verwendet wird, z. b. `New C1 With {.a=1, .b=.Method1()}`.</span><span class="sxs-lookup"><span data-stu-id="87dba-169">(3) Object initializer expressions cannot be converted to expression trees if a member of the current object being initialized is used in one of the field initializers, e.g. `New C1 With {.a=1, .b=.Method1()}`.</span></span> <span data-ttu-id="87dba-170">(4) mehrdimensionale Array Erstellungs Ausdrücke können nur in Ausdrucks Baumstrukturen konvertiert werden, wenn Sie Ihren Elementtyp explizit deklarieren.</span><span class="sxs-lookup"><span data-stu-id="87dba-170">(4) Multi-dimensional array creation expressions can only be converted to expression trees if they declare their element type explicitly.</span></span> <span data-ttu-id="87dba-171">(5) spät Bindungs Ausdrücke können nicht in Ausdrucks Baumstrukturen konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-171">(5) Late-binding expressions cannot be converted to expression trees.</span></span> <span data-ttu-id="87dba-172">(6) Wenn eine Variable oder ein Feld ByRef an einen Aufruf Ausdruck übergeben wird, aber nicht denselben Typ wie der ByRef-Parameter hat, oder wenn eine Eigenschaft ByRef übergeben wird, ist die normale VB-Semantik, dass eine Kopie des Arguments ByRef übergeben und der endgültige Wert dann kopiert wird.  zurück in die Variable oder das Feld oder die Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="87dba-172">(6) When a variable or field is passed ByRef to an invocation expression but does not have exactly the same type as the ByRef parameter, or when a property is passed ByRef, normal VB semantics are that a copy of the argument is passed ByRef and its final value is then copied back into the variable or field or property.</span></span> <span data-ttu-id="87dba-173">In Ausdrucks Baumstrukturen findet das Zurückkopieren nicht statt.</span><span class="sxs-lookup"><span data-stu-id="87dba-173">In expression trees, the copy-back does not happen.</span></span> <span data-ttu-id="87dba-174">(7) alle diese Einschränkungen gelten auch für die in der Tabelle genannten Lambda-Ausdrücke.</span><span class="sxs-lookup"><span data-stu-id="87dba-174">(7) All these restrictions apply to nested lambda expressions as well.</span></span>
    
  <span data-ttu-id="87dba-175">Wenn der Zieltyp nicht bekannt ist, wird die Lambda-Methode als Argument für einen delegatinstanzizierungsausdruck eines anonymen Delegattyps mit derselben Signatur der Lambda-Methode interpretiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-175">If the target type is not known, then the lambda method is interpreted as the argument to a delegate instantiation expression of an anonymous delegate type with the same signature of the lambda method.</span></span> <span data-ttu-id="87dba-176">Wenn eine strikte Semantik verwendet wird und der Typ eines beliebigen Parameters weggelassen wird, tritt ein Kompilierzeitfehler auf. Andernfalls wird von `Object` ein fehlender Parametertyp ersetzt.</span><span class="sxs-lookup"><span data-stu-id="87dba-176">If strict semantics are being used and the type of any of the parameters are omitted, a compile-time error occurs; otherwise, `Object` is substituted for any missing parameter type.</span></span> <span data-ttu-id="87dba-177">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-177">For example:</span></span>
    
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

* <span data-ttu-id="87dba-178">Eine Eigenschaften Gruppe kann als Eigenschaften Zugriff neu klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-178">A property group can be reclassified as a property access.</span></span> <span data-ttu-id="87dba-179">Der Eigenschafts Gruppen Ausdruck wird als Index Ausdruck mit leeren Klammern interpretiert (d. h. `f` wird als `f()` interpretiert).</span><span class="sxs-lookup"><span data-stu-id="87dba-179">The property group expression is interpreted as an index expression with empty parentheses (that is, `f` is interpreted as `f()`).</span></span>

* <span data-ttu-id="87dba-180">Ein Eigenschaftenzugriff kann als Wert neu klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-180">A property access can be reclassified as a value.</span></span> <span data-ttu-id="87dba-181">Der Eigenschafts Zugriffs Ausdruck wird als Aufruf Ausdruck der `Get`-Zugriffsmethode der-Eigenschaft interpretiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-181">The property access expression is interpreted as an invocation expression of the `Get` accessor of the property.</span></span> <span data-ttu-id="87dba-182">Wenn die Eigenschaft über keinen Getter verfügt, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-182">If the property has no getter, then a compile-time error occurs.</span></span>

* <span data-ttu-id="87dba-183">Ein spät gebundener Zugriff kann als spät gebundene Methode oder spät gebundener Eigenschaften Zugriff neu klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-183">A late-bound access can be reclassified as a late-bound method or late-bound property access.</span></span> <span data-ttu-id="87dba-184">In einer Situation, in der ein spät gebundener Zugriff sowohl als Methoden Zugriff als auch als Eigenschaften Zugriff neu klassifiziert werden kann, wird die Neuklassifizierung an einen Eigenschaften Zugriff bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="87dba-184">In a situation where a late-bound access can be reclassified both as a method access and as a property access, reclassification to a property access is preferred.</span></span>

* <span data-ttu-id="87dba-185">Ein spät gebundener Zugriff kann als Wert neu klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-185">A late-bound access can be reclassified as a value.</span></span>

* <span data-ttu-id="87dba-186">Ein Arrayliterale kann als Wert neu klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-186">An array literal can be reclassified as a value.</span></span> <span data-ttu-id="87dba-187">Der Typ des Werts wird wie folgt bestimmt:</span><span class="sxs-lookup"><span data-stu-id="87dba-187">The type of the value is determined as follows:</span></span>

  1. <span data-ttu-id="87dba-188">Wenn die Neuklassifizierung im Kontext einer Konvertierung erfolgt, bei der der Zieltyp bekannt ist und der Zieltyp ein Arraytyp ist, wird das arrayliteralformat als ein Wert vom Typ T () neu klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="87dba-188">If the reclassification occurs in the context of a conversion where the target type is known and the target type is an array type, then the array literal is reclassified as a value of type T().</span></span> <span data-ttu-id="87dba-189">Wenn der Zieltyp "`System.Collections.Generic.IList(Of T)`", "`IReadOnlyList(Of T)`", "`ICollection(Of T)`", "`IReadOnlyCollection(Of T)`" oder "`IEnumerable(Of T)`" ist und das Arrayliterale eine Schachtelungs Ebene aufweist, wird das Array-Literalformat als Wert des Typs `T()` neu klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="87dba-189">If the target type is `System.Collections.Generic.IList(Of T)`, `IReadOnlyList(Of T)`, `ICollection(Of T)`, `IReadOnlyCollection(Of T)`, or `IEnumerable(Of T)`, and the array literal has one level of nesting, then the array literal is reclassified as a value of type `T()`.</span></span>

  2. <span data-ttu-id="87dba-190">Andernfalls wird das Arrayliteral in einen Wert umklassifiziert, dessen Typ ein Array von Rang ist, das gleich der Schachtelungs Schachtelung ist, wobei der Elementtyp durch den vorherrschenden Typ der Elemente im Initialisierer bestimmt wird. Wenn kein dominanter Typ bestimmt werden kann, wird `Object` verwendet.</span><span class="sxs-lookup"><span data-stu-id="87dba-190">Otherwise, the array literal is reclassified to a value whose type is an array of rank equal to the level of nesting is used, with element type determined by the dominant type of the elements in the initializer; if no dominant type can be determined, `Object` is used.</span></span> <span data-ttu-id="87dba-191">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-191">For example:</span></span>

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

  <span data-ttu-id="87dba-192">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-192">__Note.__</span></span> <span data-ttu-id="87dba-193">Es gibt eine geringfügige Änderung des Verhaltens zwischen Version 9,0 und Version 10,0 der Sprache.</span><span class="sxs-lookup"><span data-stu-id="87dba-193">There is a slight change in behavior between version 9.0 and version 10.0 of the language.</span></span> <span data-ttu-id="87dba-194">Vor 10,0 hat sich die Initialisierer von Array Elementen nicht auf den Typrückschluss für lokale Variablen ausgewirkt, und dies geschieht nun.</span><span class="sxs-lookup"><span data-stu-id="87dba-194">Prior to 10.0, array element initializers did not affect local variable type inference and now they do.</span></span> <span data-ttu-id="87dba-195">@No__t-0 hätte daher `Object()` als Typ von `a` in Version 9,0 der Sprache und `Integer()` in Version 10,0 abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="87dba-195">So `Dim a() = { 1, 2, 3 }` would have inferred `Object()` as the type of `a` in version 9.0 of the language and `Integer()` in version 10.0.</span></span>

  <span data-ttu-id="87dba-196">Die Neuklassifizierung interpretiert dann das Arrayliterale als Ausdruck für die Array Erstellung neu.</span><span class="sxs-lookup"><span data-stu-id="87dba-196">The reclassification then reinterprets the array literal as an array-creation expression.</span></span> <span data-ttu-id="87dba-197">Die Beispiele:</span><span class="sxs-lookup"><span data-stu-id="87dba-197">So the examples:</span></span>

  ```vb
  Dim x As Double = { 1, 2, 3, 4 }
  Dim y = { "a", "b" }
  ```

  <span data-ttu-id="87dba-198">sind äquivalent zu:</span><span class="sxs-lookup"><span data-stu-id="87dba-198">are equivalent to:</span></span>

  ```vb
  Dim x As Double = New Double() { 1, 2, 3, 4 }
  Dim y = New String() { "a", "b" }
  ```

  <span data-ttu-id="87dba-199">Die Neuklassifizierung wird als Einschränkungs Einschränkung eingestuft, wenn eine Konvertierung eines Element Ausdrucks in den Array Elementtyp einschränkend ist. Andernfalls wird es als Erweiterung bewertet.</span><span class="sxs-lookup"><span data-stu-id="87dba-199">The reclassification is judged as narrowing if any conversion from an element expression to the array element type is narrowing; otherwise it is judged as widening.</span></span>

* <span data-ttu-id="87dba-200">Der Standardwert `Nothing` kann als Wert neu klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-200">The default value `Nothing` can be reclassified as a value.</span></span> <span data-ttu-id="87dba-201">In einem Kontext, in dem der Zieltyp bekannt ist, ist das Ergebnis der Standardwert des Zieltyps.</span><span class="sxs-lookup"><span data-stu-id="87dba-201">In a context where the target type is known, the result is the default value of the target type.</span></span> <span data-ttu-id="87dba-202">In einem Kontext, in dem der Zieltyp nicht bekannt ist, ist das Ergebnis ein NULL-Wert vom Typ `Object`.</span><span class="sxs-lookup"><span data-stu-id="87dba-202">In a context where the target type is not known, the result is a null value of type `Object`.</span></span>

<span data-ttu-id="87dba-203">Ein Namespace Ausdruck, ein Typausdruck, ein Ereignis Zugriffs Ausdruck oder ein void-Ausdruck können nicht neu klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-203">A namespace expression, type expression, event access expression, or void expression cannot be reclassified.</span></span> <span data-ttu-id="87dba-204">Mehrere Neuklassifizierungen können gleichzeitig durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-204">Multiple reclassifications can be done at the same time.</span></span> <span data-ttu-id="87dba-205">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-205">For example:</span></span>

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

<span data-ttu-id="87dba-206">In diesem Fall wird der Eigenschafts Gruppen Ausdruck `P` zuerst von einer Eigenschaften Gruppe zu einem Eigenschaften Zugriff neu klassifiziert und dann von einem Eigenschaften Zugriff auf einen Wert neu klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="87dba-206">In this case, the property group expression `P` is first reclassified from a property group to a property access and then reclassified from a property access to a value.</span></span> <span data-ttu-id="87dba-207">Die geringste Anzahl von Neuklassifizierungen wird ausgeführt, um eine gültige Klassifizierung im Kontext zu erreichen.</span><span class="sxs-lookup"><span data-stu-id="87dba-207">The fewest number of reclassifications are performed to reach a valid classification in the context.</span></span>

## <a name="constant-expressions"></a><span data-ttu-id="87dba-208">Konstante Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-208">Constant Expressions</span></span>

<span data-ttu-id="87dba-209">Ein *konstanter Ausdruck* ist ein Ausdruck, dessen Wert zur Kompilierzeit vollständig ausgewertet werden kann.</span><span class="sxs-lookup"><span data-stu-id="87dba-209">A *constant expression* is an expression whose value can be fully evaluated at compile time.</span></span>

```antlr
ConstantExpression
    : Expression
    ;
```

<span data-ttu-id="87dba-210">Der Typ eines konstanten Ausdrucks kann "`Byte`", "`SByte`", "`UShort`", "`Short`", "`UInteger`", "`Integer`", "`ULong`", "`Long`", "`Char`", "`Single`", "@no__t-@no__t", "3", "@no__t</span><span class="sxs-lookup"><span data-stu-id="87dba-210">The type of a constant expression can be `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Char`, `Single`, `Double`, `Decimal`, `Date`, `Boolean`, `String`, `Object`, or any enumeration type.</span></span> <span data-ttu-id="87dba-211">Die folgenden Konstrukte sind in konstanten Ausdrücken zulässig:</span><span class="sxs-lookup"><span data-stu-id="87dba-211">The following constructs are permitted in constant expressions:</span></span>

* <span data-ttu-id="87dba-212">Literale (einschließlich `Nothing`).</span><span class="sxs-lookup"><span data-stu-id="87dba-212">Literals (including `Nothing`).</span></span>

* <span data-ttu-id="87dba-213">Verweise auf Konstante Typmember oder Konstante lokale Elemente.</span><span class="sxs-lookup"><span data-stu-id="87dba-213">References to constant type members or constant locals.</span></span>

* <span data-ttu-id="87dba-214">Verweise auf Member von Enumerationstypen.</span><span class="sxs-lookup"><span data-stu-id="87dba-214">References to members of enumeration types.</span></span>

* <span data-ttu-id="87dba-215">Teil Ausdrücke in Klammern.</span><span class="sxs-lookup"><span data-stu-id="87dba-215">Parenthesized subexpressions.</span></span>

* <span data-ttu-id="87dba-216">Umwandlungs Ausdrücke, wenn der Zieltyp einem der oben aufgeführten Typen entspricht.</span><span class="sxs-lookup"><span data-stu-id="87dba-216">Coercion expressions, provided the target type is one of the types listed above.</span></span> <span data-ttu-id="87dba-217">Umwandlungen von und aus `String` sind eine Ausnahme von dieser Regel und sind nur für NULL-Werte zulässig, da `String`-Konvertierungen immer in der aktuellen Kultur der Ausführungsumgebung zur Laufzeit durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-217">Coercions to and from `String` are an exception to this rule and are only allowed on null values because `String` conversions are always done in the current culture of the execution environment at run time.</span></span> <span data-ttu-id="87dba-218">Beachten Sie, dass Konstante Umwandlungs Ausdrücke nur systeminterne Konvertierungen verwenden können.</span><span class="sxs-lookup"><span data-stu-id="87dba-218">Note that constant coercion expressions can only ever use intrinsic conversions.</span></span>

* <span data-ttu-id="87dba-219">Die unären Operatoren "`+`", "`-`" und "`Not`", sofern der Operand und das Ergebnis einen oben aufgeführten Typ enthalten.</span><span class="sxs-lookup"><span data-stu-id="87dba-219">The `+`, `-` and `Not` unary operators, provided the operand and result is of a type listed above.</span></span>

* <span data-ttu-id="87dba-220">Der `+`, `-`, `*` `^`, `Mod`, `/`, `\`, `<<`, `>>`, `&`, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 und 0 binäre Operatoren Gibt an, dass alle Operanden und Ergebnisse einen oben aufgeführten Typ haben.</span><span class="sxs-lookup"><span data-stu-id="87dba-220">The `+`, `-`, `*`, `^`, `Mod`, `/`, `\`, `<<`, `>>`, `&`, `And`, `Or`, `Xor`, `AndAlso`, `OrElse`, `=`, `<`, `>`, `<>`, `<=`, and `=>` binary operators, provided each operand and result is of a type listed above.</span></span>

* <span data-ttu-id="87dba-221">Der bedingte Operator if gibt an, dass jeder Operand und jedes Ergebnis einen oben aufgelisteten Typ hat.</span><span class="sxs-lookup"><span data-stu-id="87dba-221">The conditional operator If, provided each operand and result is of a type listed above.</span></span>

* <span data-ttu-id="87dba-222">Die folgenden Lauf Zeitfunktionen: `Microsoft.VisualBasic.Strings.ChrW`; `Microsoft.VisualBasic.Strings.Chr`, wenn der Konstante Wert zwischen 0 und 128 liegt. `Microsoft.VisualBasic.Strings.AscW`, wenn die Konstante Zeichenfolge nicht leer ist. `Microsoft.VisualBasic.Strings.Asc`, wenn die Konstante Zeichenfolge nicht leer ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-222">The following run-time functions: `Microsoft.VisualBasic.Strings.ChrW`; `Microsoft.VisualBasic.Strings.Chr` if the constant value is between 0 and 128; `Microsoft.VisualBasic.Strings.AscW` if the constant string is not empty; `Microsoft.VisualBasic.Strings.Asc` if the constant string is not empty.</span></span>

<span data-ttu-id="87dba-223">Die folgenden Konstrukte sind in konstanten Ausdrücken *nicht* zulässig:</span><span class="sxs-lookup"><span data-stu-id="87dba-223">The following constructs are *not* permitted in constant expressions:</span></span>

* <span data-ttu-id="87dba-224">Implizite Bindung durch einen `With`-Kontext.</span><span class="sxs-lookup"><span data-stu-id="87dba-224">Implicit binding through a `With` context.</span></span>

<span data-ttu-id="87dba-225">Konstante Ausdrücke eines ganzzahligen Typs ("`ULong`", "`Long`", "`UInteger`", "`Integer`", "`UShort`", "`Short`", "`SByte`" oder "`Byte`") können implizit in einen engeren ganzzahligen Typ konvertiert werden. Konstante Ausdrücke vom Typ `Double` können implizit in t-9, wenn der Wert des konstanten Ausdrucks innerhalb des Bereichs des Zieltyps liegt.</span><span class="sxs-lookup"><span data-stu-id="87dba-225">Constant expressions of an integral type (`ULong`, `Long`, `UInteger`, `Integer`, `UShort`, `Short`, `SByte`, or `Byte`) can be implicitly converted to a narrower integral type, and constant expressions of type `Double` can be implicitly converted to `Single`, provided the value of the constant expression is within the range of the destination type.</span></span> <span data-ttu-id="87dba-226">Diese einschränkenden Konvertierungen sind unabhängig davon zulässig, ob eine einschränkend sein oder strikte Semantik verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-226">These narrowing conversions are allowed regardless of whether permissive or strict semantics are being used.</span></span>


## <a name="late-bound-expressions"></a><span data-ttu-id="87dba-227">Spät gebundene Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-227">Late-Bound Expressions</span></span>

<span data-ttu-id="87dba-228">Wenn das Ziel eines Element Zugriffs Ausdrucks oder Index Ausdrucks vom Typ `Object` ist, wird die Verarbeitung des Ausdrucks möglicherweise bis zur Laufzeit verzögert.</span><span class="sxs-lookup"><span data-stu-id="87dba-228">When the target of a member access expression or index expression is of type `Object`, the processing of the expression may be deferred until run time.</span></span> <span data-ttu-id="87dba-229">Das Verzögern der Verarbeitung auf diese Weise wird als *späte Bindung*bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="87dba-229">Deferring processing this way is called *late binding*.</span></span> <span data-ttu-id="87dba-230">Bei der späten Bindung können `Object`-Variablen auf *typlose* Weise verwendet werden, wobei die gesamte Auflösung von Membern auf dem tatsächlichen Lauf Zeittyp des Werts in der Variablen basiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-230">Late binding allows `Object` variables to be used in a *typeless* way, where all resolution of members is based on the actual run-time type of the value in the variable.</span></span> <span data-ttu-id="87dba-231">Wenn eine strikte Semantik von der Kompilierungs Umgebung oder `Option Strict` angegeben wird, verursacht die späte Bindung einen Kompilierzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="87dba-231">If strict semantics are specified by the compilation environment or by `Option Strict`, late binding causes a compile-time error.</span></span> <span data-ttu-id="87dba-232">Nicht öffentliche Member werden bei einer späten Bindung ignoriert, einschließlich für den Zweck der Überladungs Auflösung.</span><span class="sxs-lookup"><span data-stu-id="87dba-232">Non-public members are ignored when doing late-binding, including for the purposes of overload resolution.</span></span> <span data-ttu-id="87dba-233">Beachten Sie, dass im Gegensatz zum früh gebundenen Fall das Aufrufen von oder das Zugreifen auf ein `Shared`-Element spät gebunden bewirkt, dass das Aufruf Ziel zur Laufzeit ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-233">Note that, unlike the early-bound case, invoking or accessing a `Shared` member late-bound will cause the invocation target to be evaluated at run time.</span></span><span data-ttu-id="87dba-234"> Wenn der Ausdruck ein Aufruf Ausdruck für ein Element ist, das auf `System.Object` definiert ist, wird die späte Bindung nicht durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="87dba-234"> If the expression is an invocation expression for a member defined on `System.Object`, late binding will not take place.</span></span>

<span data-ttu-id="87dba-235">Im Allgemeinen werden spät gebundene Zugriffe zur Laufzeit aufgelöst, indem der Bezeichner für den tatsächlichen Lauf Zeittyp des Ausdrucks gesucht wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-235">In general, late-bound accesses are resolved at run time by looking up the identifier on the actual run-time type of the expression.</span></span> <span data-ttu-id="87dba-236">Wenn die Suche nach einem spät gebundenen Member zur Laufzeit fehlschlägt, wird eine Ausnahme vom Typ "`System.MissingMemberException`" ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="87dba-236">If late-bound member lookup fails at run time, a `System.MissingMemberException` exception is thrown.</span></span> <span data-ttu-id="87dba-237">Da die Nachrichten für spät gebundene Elemente ausschließlich vom Lauf Zeittyp des zugeordneten Ziel Ausdrucks ausgeführt werden, ist der Lauf Zeittyp eines Objekts nie eine Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="87dba-237">Because late-bound member lookup is done solely off the run-time type of the associated target expression, an object's run-time type is never an interface.</span></span> <span data-ttu-id="87dba-238">Daher ist es unmöglich, auf Schnittstellenmember in einem spät gebundenen Member-Zugriffs Ausdruck zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="87dba-238">Therefore, it is impossible to access interface members in a late-bound member access expression.</span></span>

<span data-ttu-id="87dba-239">Die Argumente für einen Zugriff auf spät gebundene Elemente werden in der Reihenfolge ausgewertet, in der Sie im Member-Zugriffs Ausdruck angezeigt werden: nicht in der Reihenfolge, in der Parameter im spät gebundenen Member deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-239">The arguments to a late-bound member access are evaluated in the order they appear in the member access expression: not the order in which parameters are declared in the late-bound member.</span></span> <span data-ttu-id="87dba-240">Dieses Unterschied wird im folgenden Beispiel veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="87dba-240">The following example illustrates this difference:</span></span>

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

<span data-ttu-id="87dba-241">Dieser Code zeigt Folgendes an:</span><span class="sxs-lookup"><span data-stu-id="87dba-241">This code displays:</span></span>

```console
Early-bound: xy
Late-bound: yx
```

<span data-ttu-id="87dba-242">Da die Auflösung spät gebundener Überladungen für den Lauf Zeittyp der Argumente erfolgt, kann es vorkommen, dass ein Ausdruck unterschiedliche Ergebnisse erzeugt, je nachdem, ob er zur Kompilierzeit oder zur Laufzeit ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-242">Because late-bound overload resolution is done on the run-time type of the arguments, it is possible that an expression might produce different results based on whether it is evaluated at compile time or run time.</span></span> <span data-ttu-id="87dba-243">Dieses Unterschied wird im folgenden Beispiel veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="87dba-243">The following example illustrates this difference:</span></span>

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

<span data-ttu-id="87dba-244">Dieser Code zeigt Folgendes an:</span><span class="sxs-lookup"><span data-stu-id="87dba-244">This code displays:</span></span>

```console
F(Base)
F(Derived)
```

## <a name="simple-expressions"></a><span data-ttu-id="87dba-245">Einfache Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-245">Simple Expressions</span></span>

<span data-ttu-id="87dba-246">Einfache Ausdrücke sind Literale, Ausdrücke in Klammern, Instanzausdrücke oder einfache namens Ausdrücke.</span><span class="sxs-lookup"><span data-stu-id="87dba-246">Simple expressions are literals, parenthesized expressions, instance expressions, or simple name expressions.</span></span>

```antlr
SimpleExpression
    : LiteralExpression
    | ParenthesizedExpression
    | InstanceExpression
    | SimpleNameExpression
    | AddressOfExpression
    ;
```

### <a name="literal-expressions"></a><span data-ttu-id="87dba-247">Literale Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-247">Literal Expressions</span></span>

<span data-ttu-id="87dba-248">Literale Ausdrücke Werten den Wert aus, der vom Literalwert dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-248">Literal expressions evaluate to the value represented by the literal.</span></span> <span data-ttu-id="87dba-249">Ein Literalausdruck wird als Wert klassifiziert, ausgenommen der Literalwert `Nothing`, der als Standardwert klassifiziert wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-249">A literal expression is classified as a value, except for the literal `Nothing`, which is classified as a default value.</span></span>

```antlr
LiteralExpression
    : Literal
    ;
```

### <a name="parenthesized-expressions"></a><span data-ttu-id="87dba-250">Ausdrücke in Klammern</span><span class="sxs-lookup"><span data-stu-id="87dba-250">Parenthesized Expressions</span></span>

<span data-ttu-id="87dba-251">Ein Ausdruck in Klammern besteht aus einem Ausdruck, der in Klammern eingeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-251">A parenthesized expression consists of an expression enclosed in parentheses.</span></span> <span data-ttu-id="87dba-252">Ein Ausdruck in Klammern wird als Wert klassifiziert, und der eingeschlossene Ausdruck muss als Wert klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-252">A parenthesized expression is classified as a value, and the enclosed expression must be classified as a value.</span></span> <span data-ttu-id="87dba-253">Ein Ausdruck in Klammern ergibt den Wert des Ausdrucks innerhalb der Klammern.</span><span class="sxs-lookup"><span data-stu-id="87dba-253">A parenthesized expression evaluates to the value of the expression within the parentheses.</span></span>

```antlr
ParenthesizedExpression
    : OpenParenthesis Expression CloseParenthesis
    ;
```

### <a name="instance-expressions"></a><span data-ttu-id="87dba-254">Instanzausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-254">Instance Expressions</span></span>

<span data-ttu-id="87dba-255">Ein *Instanzausdruck* ist das Schlüsselwort `Me`.</span><span class="sxs-lookup"><span data-stu-id="87dba-255">An *instance expression* is the keyword `Me`.</span></span> <span data-ttu-id="87dba-256">Sie kann nur innerhalb des Texts einer nicht freigegebenen Methode, eines Konstruktors oder einer Eigenschafts Zugriffsmethode verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-256">It may only be used within the body of a non-shared method, constructor, or property accessor.</span></span> <span data-ttu-id="87dba-257">Sie wird als Wert klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="87dba-257">It is classified as a value.</span></span> <span data-ttu-id="87dba-258">Das Schlüsselwort `Me` stellt die Instanz des Typs dar, der die ausgeführte Methode oder den Eigenschaften Accessor enthält.</span><span class="sxs-lookup"><span data-stu-id="87dba-258">The keyword `Me` represents the instance of the type containing the method or property accessor being executed.</span></span> <span data-ttu-id="87dba-259">Wenn ein Konstruktor einen anderen Konstruktor (Abschnitts [Konstruktoren](type-members.md#constructors)) explizit aufruft, kann `Me` erst nach diesem Konstruktoraufruf verwendet werden, da die Instanz noch nicht erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="87dba-259">If a constructor explicitly invokes another constructor (Section [Constructors](type-members.md#constructors)), `Me` cannot be used until after that constructor call, because the instance has not yet been constructed.</span></span>

```antlr
InstanceExpression
    : 'Me'
    ;
```

### <a name="simple-name-expressions"></a><span data-ttu-id="87dba-260">Einfache namens Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-260">Simple Name Expressions</span></span>

<span data-ttu-id="87dba-261">Ein *einfacher namens Ausdruck* besteht aus einem einzelnen Bezeichner, gefolgt von einer optionalen Typargument Liste.</span><span class="sxs-lookup"><span data-stu-id="87dba-261">A *simple name expression* consists of a single identifier followed by an optional type argument list.</span></span>

```antlr
SimpleNameExpression
    : Identifier ( OpenParenthesis 'Of' TypeArgumentList CloseParenthesis )?
    ;
```

<span data-ttu-id="87dba-262">Der Name wird durch die folgenden "Regeln für einfache Namensauflösung" aufgelöst und klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="87dba-262">The name is resolved and classified by the following "simple name resolution rules":</span></span>

1.  <span data-ttu-id="87dba-263">Beginnend mit dem unmittelbar einschließenden Block und fortsetzen mit jedem einschließenden äußeren Block (sofern vorhanden), bezieht sich der Bezeichner auf den Wert, wenn der Bezeichner mit dem Namen einer lokalen Variablen, einer statischen Variable, einer Konstanten lokalen Methode, eines methodentypparameters oder eines Parameters übereinstimmt. entsprechende Entität.</span><span class="sxs-lookup"><span data-stu-id="87dba-263">Starting with the immediately enclosing block and continuing with each enclosing outer block (if any), if the identifier matches the name of a local variable, static variable, constant local, method type parameter, or parameter, then the identifier refers to the matching entity.</span></span>

    <span data-ttu-id="87dba-264">Wenn der Bezeichner mit einer lokalen Variablen, einer statischen Variablen oder einer Konstanten lokalen Konstante übereinstimmt und eine Typargument Liste angegeben wurde, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-264">If the identifier matches a local variable, static variable, or constant local and a type argument list was provided, a compile-time error occurs.</span></span> <span data-ttu-id="87dba-265">Wenn der Bezeichner mit einem Methodentypparameter übereinstimmt und eine Typargument Liste bereitgestellt wurde, erfolgt keine Übereinstimmung, und die Auflösung wird</span><span class="sxs-lookup"><span data-stu-id="87dba-265">If the identifier matches a method type parameter and a type argument list was provided, no match occurs and resolution continues.</span></span> <span data-ttu-id="87dba-266">Wenn der Bezeichner mit einer lokalen Variablen übereinstimmt, ist die lokale Variable, die übereinstimmt, die implizite Funktion, oder `Get`-Accessor gibt eine lokale Variable zurück, und der Ausdruck ist Teil eines Aufruf Ausdrucks, einer Aufruf Anweisung oder eines `AddressOf`-Ausdrucks, dann keine Übereinstimmung. Tritt auf und die Auflösung wird fortgesetzt</span><span class="sxs-lookup"><span data-stu-id="87dba-266">If the identifier matches a local variable, the local variable matched is the implicit function or `Get` accessor return local variable, and the expression is part of an invocation expression, invocation statement, or an `AddressOf` expression, then no match occurs and resolution continues.</span></span>

    <span data-ttu-id="87dba-267">Der Ausdruck wird als Variable klassifiziert, wenn es sich um eine lokale Variable, eine statische Variable oder einen Parameter handelt.</span><span class="sxs-lookup"><span data-stu-id="87dba-267">The expression is classified as a variable if it is a local variable, static variable, or parameter.</span></span> <span data-ttu-id="87dba-268">Der Ausdruck wird als Typ klassifiziert, wenn es sich um einen Methodentypparameter handelt.</span><span class="sxs-lookup"><span data-stu-id="87dba-268">The expression is classified as a type if it is a method type parameter.</span></span> <span data-ttu-id="87dba-269">Der Ausdruck wird als Wert klassifiziert, wenn es sich um eine Konstante local handelt.</span><span class="sxs-lookup"><span data-stu-id="87dba-269">The expression is classified as a value if it is a constant local.</span></span>

2.  <span data-ttu-id="87dba-270">Wenn eine Suche des Bezeichners im Typ eine Entsprechung mit einem zugänglichen Member erzeugt, werden für jeden in der-Spalte enthaltenden Typ, der den Ausdruck enthält, beginnend mit dem innersten und zum äußersten Wert zurückgegeben:</span><span class="sxs-lookup"><span data-stu-id="87dba-270">For each nested type containing the expression, starting from the innermost and going to the outermost, if a lookup of the identifier in the type produces a match with an accessible member:</span></span>

    21. <span data-ttu-id="87dba-271">Wenn der übereinstimmende Typmember ein Typparameter ist, wird das Ergebnis als-Typ klassifiziert und ist der passende Typparameter.</span><span class="sxs-lookup"><span data-stu-id="87dba-271">If the matching type member is a type parameter, then the result is classified as a type and is the matching type parameter.</span></span> <span data-ttu-id="87dba-272">Wenn eine Typargument Liste angegeben wurde, erfolgt keine Entsprechung, und die Auflösung wird fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="87dba-272">If a type argument list was provided, no match occurs and resolution continues.</span></span>
    22. <span data-ttu-id="87dba-273">Andernfalls ist das Ergebnis, wenn der Typ der unmittelbar einschließende Typ und die Suche einen nicht freigegebenen Typmember identifiziert, identisch mit dem Element Zugriff auf das Formular `Me.E(Of A)`, wobei `E` der Bezeichner und `A` die Typargument Liste ist. , falls vorhanden.</span><span class="sxs-lookup"><span data-stu-id="87dba-273">Otherwise, if the type is the immediately enclosing type and the lookup identifies a non-shared type member, then the result is the same as a member access of the form `Me.E(Of A)`, where `E` is the identifier and `A` is the type argument list, if any.</span></span>
    23. <span data-ttu-id="87dba-274">Andernfalls ist das Ergebnis genau das gleiche wie ein Element Zugriff auf das Formular `T.E(Of A)`, wobei `T` der Typ ist, der das übereinstimmende Element enthält, `E` der Bezeichner ist, und `A` ist die Typargument Liste (falls vorhanden).</span><span class="sxs-lookup"><span data-stu-id="87dba-274">Otherwise, the result is exactly the same as a member access of the form `T.E(Of A)`, where `T` is the type containing the matching member, `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="87dba-275">In diesem Fall ist es ein Fehler für den Bezeichner, auf einen nicht freigegebenen Member zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="87dba-275">In this case, it is an error for the identifier to refer to a non-shared member.</span></span>

3.  <span data-ttu-id="87dba-276">Führen Sie für jeden schsted Namespace, beginnend am innersten und zum äußersten Namespace, die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="87dba-276">For each nested namespace, starting from the innermost and going to the outermost namespace, do the following:</span></span>

    31. <span data-ttu-id="87dba-277">Wenn der Namespace einen zugreif baren Typ mit dem angegebenen Namen enthält und die gleiche Anzahl von Typparametern aufweist, die in der Typargument Liste angegeben sind (sofern vorhanden), verweist der Bezeichner auf diesen Typ und wird als Typ klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="87dba-277">If the namespace contains an accessible type with the given name and has the same number of type parameters as was supplied in the type argument list, if any, then the identifier refers to that type and is classified as a type.</span></span>
    32. <span data-ttu-id="87dba-278">Andernfalls bezieht sich der Bezeichner auf diesen Namespace und wird als Namespace klassifiziert, wenn keine Typargument Liste angegeben wurde und der Namespace einen Namespace-Member mit dem angegebenen Namen enthält.</span><span class="sxs-lookup"><span data-stu-id="87dba-278">Otherwise, if no type argument list was supplied and the namespace contains a namespace member with the given name, then the identifier refers to that namespace and is classified as a namespace.</span></span>
    33. <span data-ttu-id="87dba-279">Wenn der Namespace andernfalls mindestens ein zugreif bares Standardmodul enthält und eine Elementnamen Suche des Bezeichners eine barrierefreie Übereinstimmung in genau einem Standardmodul erzeugt, ist das Ergebnis genau das gleiche wie ein Member-Zugriff im Formular `M.E(Of A)`, wobei @no_ _T-1 ist das Standardmodul, das den passenden Member enthält, `E` der Bezeichner ist, und `A` ist die Typargument Liste, falls vorhanden.</span><span class="sxs-lookup"><span data-stu-id="87dba-279">Otherwise, if the namespace contains one or more accessible standard modules, and a member name lookup of the identifier produces an accessible match in exactly one standard module, then the result is exactly the same as a member access of the form `M.E(Of A)`, where `M` is the standard module containing the matching member, `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="87dba-280">Wenn der Bezeichner mit zugänglichen Typmembern in mehr als einem Standardmodul übereinstimmt, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-280">If the identifier matches accessible type members in more than one standard module, a compile-time error occurs.</span></span>

4.  <span data-ttu-id="87dba-281">Wenn die Quelldatei mindestens eine Import-Aliase aufweist und der Bezeichner mit dem Namen einer dieser Dateien übereinstimmt, verweist der Bezeichner auf diesen Namespace oder Typ.</span><span class="sxs-lookup"><span data-stu-id="87dba-281">If the source file has one or more import aliases, and the identifier matches the name of one of them, then the identifier refers to that namespace or type.</span></span> <span data-ttu-id="87dba-282">Wenn eine Typargument Liste angegeben wird, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-282">If a type argument list is supplied, a compile-time error occurs.</span></span>

5. <span data-ttu-id="87dba-283">Wenn die Quelldatei, die den Namen Verweis enthält, mindestens einen Import hat:</span><span class="sxs-lookup"><span data-stu-id="87dba-283">If the source file containing the name reference has one or more imports:</span></span>

    51. <span data-ttu-id="87dba-284">Wenn der Bezeichner in genau einem Import den Namen eines barrierefreien Typs mit der gleichen Anzahl von Typparametern, die in der Typargument Liste angegeben sind (sofern vorhanden), oder einem Typmember übereinstimmt, verweist der Bezeichner auf diesen Typ oder Typmember.</span><span class="sxs-lookup"><span data-stu-id="87dba-284">If the identifier matches in exactly one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or a type member, then the identifier refers to that type or type member.</span></span> <span data-ttu-id="87dba-285">Wenn der Bezeichner in mehr als einem Import mit der gleichen Anzahl von Typparametern übereinstimmt, die in der Typargument Liste, sofern vorhanden, oder einem zugänglichen Typmember angegeben wurde, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-285">If the identifier matches in more than one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or an accessible type member, a compile-time error occurs.</span></span>
    52. <span data-ttu-id="87dba-286">Andernfalls bezieht sich der Bezeichner auf diesen Namespace, wenn keine Typargument Liste angegeben wurde und der Bezeichner genau einem Import den Namen eines Namespaces mit barrierefreien Typen entspricht.</span><span class="sxs-lookup"><span data-stu-id="87dba-286">Otherwise, if no type argument list was supplied and the identifier matches in exactly one import the name of a namespace with accessible types, then the identifier refers to that namespace.</span></span> <span data-ttu-id="87dba-287">Wenn keine Typargument Liste angegeben wurde und der Bezeichner in mehr als einem Import den Namen eines Namespace mit zugänglichen Typen findet, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-287">If no type argument list was supplied and the identifier matches in more than one import the name of a namespace with accessible types, a compile-time error occurs.</span></span>
    53. <span data-ttu-id="87dba-288">Wenn die Importe ein oder mehrere barrierefreie Standardmodule enthalten und eine Element Namenssuche des Bezeichners eine barrierefreie Übereinstimmung in genau einem Standardmodul erzeugt, ist das Ergebnis genau das gleiche wie ein Member-Zugriff im Formular `M.E(Of A)`, wobei @no__ t-1 ist das Standardmodul, das den passenden Member enthält, `E` der Bezeichner ist, und `A` ist die Typargument Liste, falls vorhanden.</span><span class="sxs-lookup"><span data-stu-id="87dba-288">Otherwise, if the imports contain one or more accessible standard modules, and a member name lookup of the identifier produces an accessible match in exactly one standard module, then the result is exactly the same as a member access of the form `M.E(Of A)`, where `M` is the standard module containing the matching member, `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="87dba-289">Wenn der Bezeichner mit zugänglichen Typmembern in mehr als einem Standardmodul übereinstimmt, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-289">If the identifier matches accessible type members in more than one standard module, a compile-time error occurs.</span></span>

6.  <span data-ttu-id="87dba-290">Wenn in der Kompilierungs Umgebung mindestens eine Import-Aliase definiert ist und der Bezeichner mit dem Namen einer dieser Elemente übereinstimmt, verweist der Bezeichner auf diesen Namespace oder Typ.</span><span class="sxs-lookup"><span data-stu-id="87dba-290">If the compilation environment defines one or more import aliases, and the identifier matches the name of one of them, then the identifier refers to that namespace or type.</span></span> <span data-ttu-id="87dba-291">Wenn eine Typargument Liste angegeben wird, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-291">If a type argument list is supplied, a compile-time error occurs.</span></span>

7. <span data-ttu-id="87dba-292">Wenn in der Kompilierungs Umgebung mindestens ein Import definiert ist:</span><span class="sxs-lookup"><span data-stu-id="87dba-292">If the compilation environment defines one or more imports:</span></span>

    71. <span data-ttu-id="87dba-293">Wenn der Bezeichner in genau einem Import den Namen eines barrierefreien Typs mit der gleichen Anzahl von Typparametern, die in der Typargument Liste angegeben sind (sofern vorhanden), oder einem Typmember übereinstimmt, verweist der Bezeichner auf diesen Typ oder Typmember.</span><span class="sxs-lookup"><span data-stu-id="87dba-293">If the identifier matches in exactly one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or a type member, then the identifier refers to that type or type member.</span></span> <span data-ttu-id="87dba-294">Wenn der Bezeichner in mehr als einem Import mit der gleichen Anzahl von Typparametern übereinstimmt, die in der Typargument Liste, sofern vorhanden, oder einem Typmember angegeben wurde, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-294">If the identifier matches in more than one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or a type member, a compile-time error occurs.</span></span>
    72. <span data-ttu-id="87dba-295">Andernfalls bezieht sich der Bezeichner auf diesen Namespace, wenn keine Typargument Liste angegeben wurde und der Bezeichner genau einem Import den Namen eines Namespaces mit barrierefreien Typen entspricht.</span><span class="sxs-lookup"><span data-stu-id="87dba-295">Otherwise, if no type argument list was supplied and the identifier matches in exactly one import the name of a namespace with accessible types, then the identifier refers to that namespace.</span></span> <span data-ttu-id="87dba-296">Wenn keine Typargument Liste angegeben wurde und der Bezeichner in mehr als einem Import den Namen eines Namespace mit zugänglichen Typen findet, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-296">If no type argument list was supplied and the identifier matches in more than one import the name of a namespace with accessible types, a compile-time error occurs.</span></span>
    73. <span data-ttu-id="87dba-297">Wenn die Importe ein oder mehrere barrierefreie Standardmodule enthalten und eine Element Namenssuche des Bezeichners eine barrierefreie Übereinstimmung in genau einem Standardmodul erzeugt, ist das Ergebnis genau das gleiche wie ein Member-Zugriff im Formular `M.E(Of A)`, wobei @no__ t-1 ist das Standardmodul, das den passenden Member enthält, `E` der Bezeichner ist, und `A` ist die Typargument Liste, falls vorhanden.</span><span class="sxs-lookup"><span data-stu-id="87dba-297">Otherwise, if the imports contain one or more accessible standard modules, and a member name lookup of the identifier produces an accessible match in exactly one standard module, then the result is exactly the same as a member access of the form `M.E(Of A)`, where `M` is the standard module containing the matching member,  `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="87dba-298">Wenn der Bezeichner mit zugänglichen Typmembern in mehr als einem Standardmodul übereinstimmt, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-298">If the identifier matches accessible type members in more than one standard module, a compile-time error occurs.</span></span>

8. <span data-ttu-id="87dba-299">Andernfalls ist der vom Bezeichner angegebene Name nicht definiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-299">Otherwise, the name given by the identifier is undefined.</span></span>

<span data-ttu-id="87dba-300">Ein einfacher namens Ausdruck, der nicht definiert ist, ist ein Kompilierzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="87dba-300">A simple name expression that is undefined is a compile-time error.</span></span>

<span data-ttu-id="87dba-301">Normalerweise kann ein Name nur einmal in einem bestimmten Namespace vorkommen.</span><span class="sxs-lookup"><span data-stu-id="87dba-301">Normally, a name can only occur once in a particular namespace.</span></span> <span data-ttu-id="87dba-302">Da Namespaces jedoch über mehrere .NET-Assemblys hinweg deklariert werden können, kann es vorkommen, dass zwei Assemblys einen Typ mit demselben voll qualifizierten Namen definieren.</span><span class="sxs-lookup"><span data-stu-id="87dba-302">However, because namespaces can be declared across multiple .NET assemblies, it is possible to have a situation where two assemblies define a type with the same fully qualified name.</span></span> <span data-ttu-id="87dba-303">In diesem Fall wird ein Typ, der im aktuellen Satz von Quelldateien deklariert ist, von einem in einer externen .NET-Assembly deklarierten Typ bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="87dba-303">In that case, a type declared in the current set of source files is preferred over a type declared in an external .NET assembly.</span></span> <span data-ttu-id="87dba-304">Andernfalls ist der Name mehrdeutig, und es gibt keine Möglichkeit, den Namen zu unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="87dba-304">Otherwise, the name is ambiguous and there is no way to disambiguate the name.</span></span>


### <a name="addressof-expressions"></a><span data-ttu-id="87dba-305">AddressOf-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-305">AddressOf Expressions</span></span>

<span data-ttu-id="87dba-306">Ein Ausdruck vom Typ "`AddressOf`" wird verwendet, um einen Methoden Zeiger zu entwickeln.</span><span class="sxs-lookup"><span data-stu-id="87dba-306">An `AddressOf` expression is used to produce a method pointer.</span></span> <span data-ttu-id="87dba-307">Der Ausdruck besteht aus dem `AddressOf`-Schlüsselwort und einem Ausdruck, der als Methoden Gruppe oder spät gebundener Zugriff klassifiziert werden muss.</span><span class="sxs-lookup"><span data-stu-id="87dba-307">The expression consists of the `AddressOf` keyword and an expression that must be classified as a method group or a late-bound access.</span></span> <span data-ttu-id="87dba-308">Die Methoden Gruppe kann nicht auf Konstruktoren verweisen.</span><span class="sxs-lookup"><span data-stu-id="87dba-308">The method group cannot refer to constructors.</span></span>

<span data-ttu-id="87dba-309">Das Ergebnis wird als Methoden Zeiger klassifiziert, mit dem gleichen zugeordneten Ziel Ausdruck und der gleichen Typargument Liste (sofern vorhanden) als Methoden Gruppe.</span><span class="sxs-lookup"><span data-stu-id="87dba-309">The result is classified as a method pointer, with the same associated target expression and type argument list (if any) as the method group.</span></span>

```antlr
AddressOfExpression
    : 'AddressOf' Expression
    ;
```

## <a name="type-expressions"></a><span data-ttu-id="87dba-310">Typausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-310">Type Expressions</span></span>

<span data-ttu-id="87dba-311">Ein *Typausdruck* ist ein `GetType`-Ausdruck, ein `TypeOf...Is`-Ausdruck, ein `Is`-Ausdruck oder ein `GetXmlNamespace`-Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="87dba-311">A *type expression* is a `GetType` expression, a `TypeOf...Is` expression, an `Is` expression, or a `GetXmlNamespace` expression.</span></span>

```antlr
TypeExpression
    : GetTypeExpression
    | TypeOfIsExpression
    | IsExpression
    | GetXmlNamespaceExpression
    ;
```

### <a name="gettype-expressions"></a><span data-ttu-id="87dba-312">GetType-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-312">GetType Expressions</span></span>

<span data-ttu-id="87dba-313">Ein `GetType`-Ausdruck besteht aus dem Schlüsselwort `GetType` und dem Namen eines Typs.</span><span class="sxs-lookup"><span data-stu-id="87dba-313">A `GetType` expression consists of the keyword `GetType` and the name of a type.</span></span>

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

<span data-ttu-id="87dba-314">Ein `GetType`-Ausdruck wird als Wert klassifiziert, und sein Wert ist die Reflektionsklasse (`System.Type`), die den *gettypeer-Typnamen*darstellt.</span><span class="sxs-lookup"><span data-stu-id="87dba-314">A `GetType` expression is classified as a value, and its value is the reflection (`System.Type`) class that represents its *GetTypeTypeName*.</span></span> <span data-ttu-id="87dba-315">Wenn " *gettypeer* Type" ein Typparameter ist, gibt der Ausdruck das `System.Type`-Objekt zurück, das dem Typargument entspricht, das für den Typparameter zur Laufzeit angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-315">If the *GetTypeTypeName* is a type parameter, the expression will return the `System.Type` object that corresponds to the type argument supplied for the type parameter at run-time.</span></span>

<span data-ttu-id="87dba-316">Der *gettypeer-Name* hat zwei Möglichkeiten:</span><span class="sxs-lookup"><span data-stu-id="87dba-316">The *GetTypeTypeName* is special in two ways:</span></span>

* <span data-ttu-id="87dba-317">Es darf `System.Void` sein, die einzige Stelle in der Sprache, auf die auf diesen Typnamen verwiesen werden kann.</span><span class="sxs-lookup"><span data-stu-id="87dba-317">It is allowed to be `System.Void`, the only place in the language where this type name may be referenced.</span></span>

* <span data-ttu-id="87dba-318">Möglicherweise handelt es sich um einen konstruierten generischen Typ mit den Typargumenten.</span><span class="sxs-lookup"><span data-stu-id="87dba-318">It may be a constructed generic type with the type arguments omitted.</span></span> <span data-ttu-id="87dba-319">Dadurch kann der `GetType`-Ausdruck das `System.Type`-Objekt zurückgeben, das dem generischen Typ selbst entspricht.</span><span class="sxs-lookup"><span data-stu-id="87dba-319">This allows the `GetType` expression to return the `System.Type` object that corresponds to the generic type itself.</span></span>

<span data-ttu-id="87dba-320">Das folgende Beispiel veranschaulicht den `GetType`-Ausdruck:</span><span class="sxs-lookup"><span data-stu-id="87dba-320">The following example demonstrates the `GetType` expression:</span></span>

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

<span data-ttu-id="87dba-321">Die resultierende Ausgabe lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="87dba-321">The resulting output is:</span></span>

```console
Int32
Int32
String
Double[]
```


### <a name="typeofis-expressions"></a><span data-ttu-id="87dba-322">Typeof... Is-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-322">TypeOf...Is Expressions</span></span>

<span data-ttu-id="87dba-323">Ein `TypeOf...Is`-Ausdruck wird verwendet, um zu überprüfen, ob der Lauf Zeittyp eines Werts mit einem bestimmten Typ kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-323">A `TypeOf...Is` expression is used to check whether the run-time type of a value is compatible with a given type.</span></span> <span data-ttu-id="87dba-324">Der erste Operand muss als Wert klassifiziert werden, darf keine neu klassifizierte Lambda-Methode sein und muss einen Verweistyp oder einen nicht eingeschränkten typparametertyp aufweisen.</span><span class="sxs-lookup"><span data-stu-id="87dba-324">The first operand must be classified as a value, cannot be a reclassified lambda method, and must be of a reference type or an unconstrained type parameter type.</span></span> <span data-ttu-id="87dba-325">Der zweite Operand muss ein Typname sein.</span><span class="sxs-lookup"><span data-stu-id="87dba-325">The second operand must be a type name.</span></span> <span data-ttu-id="87dba-326">Das Ergebnis des Ausdrucks wird als Wert klassifiziert und ist ein `Boolean`-Wert.</span><span class="sxs-lookup"><span data-stu-id="87dba-326">The result of the expression is classified as a value and is a `Boolean` value.</span></span> <span data-ttu-id="87dba-327">Der Ausdruck wird zu `True` ausgewertet, wenn der Lauf Zeittyp des Operanden Identitäts-, Standard-, Verweis-, Array-, Werttyp-oder Typparameter Konvertierung in den Typ aufweist, andernfalls `False`.</span><span class="sxs-lookup"><span data-stu-id="87dba-327">The expression evaluates to `True` if the run-time type of the operand has an identity, default, reference, array, value type, or type parameter conversion to the type, `False` otherwise.</span></span> <span data-ttu-id="87dba-328">Ein Kompilierzeitfehler tritt auf, wenn keine Konvertierung zwischen dem Typ des Ausdrucks und dem spezifischen Typ vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-328">A compile-time error occurs if no conversion exists between the type of the expression and the specific type.</span></span>

```antlr
TypeOfIsExpression
    : 'TypeOf' Expression 'Is' LineTerminator? TypeName
    ;
```

### <a name="is-expressions"></a><span data-ttu-id="87dba-329">Is-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-329">Is Expressions</span></span>

<span data-ttu-id="87dba-330">Ein Ausdruck vom Typ "`Is`" oder "`IsNot`" wird für einen Verweis Gleichheits Vergleich verwendet.</span><span class="sxs-lookup"><span data-stu-id="87dba-330">An `Is` or `IsNot` expression is used to do a reference equality comparison.</span></span>

```antlr
IsExpression
    : Expression 'Is' LineTerminator? Expression
    | Expression 'IsNot' LineTerminator? Expression
    ;
```

<span data-ttu-id="87dba-331">Jeder Ausdruck muss als Wert klassifiziert werden, und der Typ jedes Ausdrucks muss ein Verweistyp, ein uneingeschränkter typparametertyp oder ein Werte zulässt-Werttyp sein.</span><span class="sxs-lookup"><span data-stu-id="87dba-331">Each expression must be classified as a value and the type of each expression must be a reference type, an unconstrained type parameter type, or a nullable value type.</span></span> <span data-ttu-id="87dba-332">Wenn der Typ eines Ausdrucks ein uneingeschränkter typparametertyp oder ein Werte zulässt-Werttyp ist, muss der andere Ausdruck der Literalwert `Nothing` sein.</span><span class="sxs-lookup"><span data-stu-id="87dba-332">If the type of one expression is an unconstrained type parameter type or nullable value type, however, the other expression must be the literal `Nothing`.</span></span>

<span data-ttu-id="87dba-333">Das Ergebnis wird als Wert klassifiziert und als `Boolean` typisiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-333">The result is classified as a value and is typed as `Boolean`.</span></span> <span data-ttu-id="87dba-334">Ein `Is`-Vorgang wird als `True` ausgewertet, wenn beide Werte auf dieselbe Instanz verweisen oder beide Werte `Nothing` oder `False` sind.</span><span class="sxs-lookup"><span data-stu-id="87dba-334">An `Is` operation evaluates to `True` if both values refer to the same instance or both values are `Nothing`, or `False` otherwise.</span></span> <span data-ttu-id="87dba-335">Ein `IsNot`-Vorgang wird als `False` ausgewertet, wenn beide Werte auf dieselbe Instanz verweisen oder beide Werte `Nothing` oder `True` sind.</span><span class="sxs-lookup"><span data-stu-id="87dba-335">An `IsNot` operation evaluates to `False` if both values refer to the same instance or both values are `Nothing`, or `True` otherwise.</span></span>


### <a name="getxmlnamespace-expressions"></a><span data-ttu-id="87dba-336">GetXmlNamespace-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-336">GetXmlNamespace Expressions</span></span>

<span data-ttu-id="87dba-337">Ein `GetXmlNamespace`-Ausdruck besteht aus dem Schlüsselwort `GetXmlNamespace` und dem Namen eines XML-Namespace, der von der Quelldatei oder der Kompilierungs Umgebung deklariert wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-337">A `GetXmlNamespace` expression consists of the keyword `GetXmlNamespace` and the name of an XML namespace declared by the source file or compilation environment.</span></span>

```antlr
GetXmlNamespaceExpression
    : 'GetXmlNamespace' OpenParenthesis XMLNamespaceName? CloseParenthesis
    ;
```

<span data-ttu-id="87dba-338">Ein `GetXmlNamespace`-Ausdruck wird als Wert klassifiziert, und sein Wert ist eine Instanz von `System.Xml.Linq.XNamespace`, die den *xmlNamespaceName*darstellt.</span><span class="sxs-lookup"><span data-stu-id="87dba-338">An `GetXmlNamespace` expression is classified as a value, and its value is an instance of `System.Xml.Linq.XNamespace` that represents the *XMLNamespaceName*.</span></span> <span data-ttu-id="87dba-339">Wenn dieser Typ nicht verfügbar ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-339">If that type is not available, then a compile-time error will occur.</span></span>

<span data-ttu-id="87dba-340">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-340">For example:</span></span>

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

<span data-ttu-id="87dba-341">Alles zwischen den Klammern wird als Teil des Namespace namens betrachtet, sodass XML-Regeln, wie z. b. Leerzeichen, angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-341">Everything between the parentheses is considered part of the namespace name, so XML rules around things such as whitespace apply.</span></span> <span data-ttu-id="87dba-342">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-342">For example:</span></span>

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

<span data-ttu-id="87dba-343">Der XML-Namespace Ausdruck kann auch weggelassen werden. in diesem Fall gibt der Ausdruck das Objekt zurück, das den XML-Standard Namespace darstellt.</span><span class="sxs-lookup"><span data-stu-id="87dba-343">The XML namespace expression can also be omitted, in which case the expression returns the object that represents the default XML namespace.</span></span>


## <a name="member-access-expressions"></a><span data-ttu-id="87dba-344">Memberzugriffsausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-344">Member Access Expressions</span></span>

<span data-ttu-id="87dba-345">Ein Member-Zugriffs Ausdruck wird für den Zugriff auf einen Member einer Entität verwendet.</span><span class="sxs-lookup"><span data-stu-id="87dba-345">A member access expression is used to access a member of an entity.</span></span>

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

<span data-ttu-id="87dba-346">Ein Element Zugriff auf das Formular `E.I(Of A)`, wobei `E` ein Ausdruck, ein nicht Array-Typname, das Schlüsselwort `Global` oder ausgelassen und `I` ein Bezeichner mit einer optionalen Typargument Liste `A` ist, wird wie folgt ausgewertet und klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="87dba-346">A member access of the form `E.I(Of A)`, where `E` is an expression, a non-array type name, the keyword `Global`, or omitted and `I` is an identifier with an optional type argument list `A`, is evaluated and classified as follows:</span></span>

1. <span data-ttu-id="87dba-347">Wenn `E` weggelassen wird, wird der Ausdruck aus der direkt enthaltenden `With`-Anweisung `E` ersetzt, und der Element Zugriff wird durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="87dba-347">If `E` is omitted, then the expression from the immediately containing `With` statement is substituted for `E` and the member access is performed.</span></span> <span data-ttu-id="87dba-348">Wenn keine `With`-Anweisung enthalten ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-348">If there is no containing `With` statement, a compile-time error occurs.</span></span>

2. <span data-ttu-id="87dba-349">Wenn `E` als Namespace klassifiziert ist oder `E` das Schlüsselwort `Global` ist, erfolgt die Element Suche im Kontext des angegebenen Namespace.</span><span class="sxs-lookup"><span data-stu-id="87dba-349">If `E` is classified as a namespace or `E` is the keyword `Global`, then the member lookup is done in the context of the specified namespace.</span></span> <span data-ttu-id="87dba-350">Wenn `I` der Name eines zugreif baren Members dieses Namespace mit derselben Anzahl von Typparametern ist, die in der Typargument Liste angegeben wurden (sofern vorhanden), ist das Ergebnis dieser Member.</span><span class="sxs-lookup"><span data-stu-id="87dba-350">If `I` is the name of an accessible member of that namespace with the same number of type parameters as was supplied in the type argument list, if any, then the result is that member.</span></span> <span data-ttu-id="87dba-351">Das Ergebnis wird abhängig vom Member als Namespace oder Typ klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="87dba-351">The result is classified as a namespace or a type depending on the member.</span></span> <span data-ttu-id="87dba-352">Andernfalls tritt ein Kompilierungsfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-352">Otherwise, a compile-time error occurs.</span></span>

3. <span data-ttu-id="87dba-353">Wenn `E` ein Typ oder ein Ausdruck ist, der als-Typ klassifiziert ist, erfolgt die Element Suche im Kontext des angegebenen Typs.</span><span class="sxs-lookup"><span data-stu-id="87dba-353">If `E` is a type or an expression classified as a type, then the member lookup is done in the context of the specified type.</span></span> <span data-ttu-id="87dba-354">Wenn `I` der Name eines barrierefreien Members von `E` ist, wird `E.I` wie folgt ausgewertet und klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="87dba-354">If `I` is the name of an accessible member of `E`, then `E.I` is evaluated and classified as follows:</span></span>

    31. <span data-ttu-id="87dba-355">Wenn `I` das Schlüsselwort `New` und `E` keine Enumeration ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-355">If `I` is the keyword `New` and `E` is not an enumeration then a compile-time error occurs.</span></span>
    32. <span data-ttu-id="87dba-356">Wenn `I` einen Typ mit der gleichen Anzahl von Typparametern identifiziert, der in der Typargument Liste angegeben wurde (sofern vorhanden), ist das Ergebnis dieser Typ.</span><span class="sxs-lookup"><span data-stu-id="87dba-356">If `I` identifies a type with the same number of type parameters as was supplied in the type argument list, if any, then the result is that type.</span></span>
    33. <span data-ttu-id="87dba-357">Wenn `I` eine oder mehrere Methoden identifiziert, ist das Ergebnis eine Methoden Gruppe mit der zugeordneten Typargument Liste und keinem zugeordneten Ziel Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="87dba-357">If `I` identifies one or more methods, then the result is a method group with the associated type argument list and no associated target expression.</span></span>
    34. <span data-ttu-id="87dba-358">Wenn `I` eine oder mehrere Eigenschaften identifiziert und keine Typargument Liste angegeben wurde, ist das Ergebnis eine Eigenschaften Gruppe ohne zugeordneten Ziel Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="87dba-358">If `I` identifies one or more properties and no type argument list was supplied, then the result is a property group with no associated target expression.</span></span>
    35. <span data-ttu-id="87dba-359">Wenn `I` eine freigegebene Variable identifiziert und keine Typargument Liste angegeben wurde, ist das Ergebnis entweder eine Variable oder ein Wert.</span><span class="sxs-lookup"><span data-stu-id="87dba-359">If `I` identifies a shared variable and no type argument list was supplied, then the result is either a variable or a value.</span></span> <span data-ttu-id="87dba-360">Wenn die Variable schreibgeschützt ist und der Verweis außerhalb des freigegebenen Konstruktors des Typs auftritt, in dem die Variable deklariert ist, ist das Ergebnis der Wert der freigegebenen Variablen `I` in `E`.</span><span class="sxs-lookup"><span data-stu-id="87dba-360">If the variable is read-only, and the reference occurs outside the shared constructor of the type in which the variable is declared, then the result is the value of the shared variable `I` in `E`.</span></span> <span data-ttu-id="87dba-361">Andernfalls ist das Ergebnis die freigegebene Variable `I` in `E`.</span><span class="sxs-lookup"><span data-stu-id="87dba-361">Otherwise, the result is the shared variable `I` in `E`.</span></span>
    36. <span data-ttu-id="87dba-362">Wenn `I` ein frei gegebenes Ereignis identifiziert und keine Typargument Liste angegeben wurde, ist das Ergebnis ein Ereignis Zugriff ohne zugeordneten Ziel Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="87dba-362">If `I` identifies a shared event and no type argument list was supplied, the result is an event access with no associated target expression.</span></span>
    37. <span data-ttu-id="87dba-363">Wenn `I` eine Konstante identifiziert und keine Typargument Liste angegeben wurde, ist das Ergebnis der Wert dieser Konstante.</span><span class="sxs-lookup"><span data-stu-id="87dba-363">If `I` identifies a constant and no type argument list was supplied, then the result is the value of that constant.</span></span>
    38. <span data-ttu-id="87dba-364">Wenn `I` einen Enumerationsmember identifiziert und keine Typargument Liste angegeben wurde, ist das Ergebnis der Wert dieses Enumerationsmembers.</span><span class="sxs-lookup"><span data-stu-id="87dba-364">If `I` identifies an enumeration member and no type argument list was supplied, then the result is the value of that enumeration member.</span></span>
    39. <span data-ttu-id="87dba-365">Andernfalls ist `E.I` ein ungültiger Element Verweis, und es tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-365">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>

4. <span data-ttu-id="87dba-366">Wenn `E` als Variable oder Wert klassifiziert ist, der Typ, der `T` ist, wird die Element Suche im Kontext von `T` ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="87dba-366">If `E` is classified as a variable or value, the type of which is `T`, then the member lookup is done in the context of `T`.</span></span> <span data-ttu-id="87dba-367">Wenn `I` der Name eines barrierefreien Members von `T` ist, wird `E.I` wie folgt ausgewertet und klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="87dba-367">If `I` is the name of an accessible member of `T`, then `E.I` is evaluated and classified as follows:</span></span>

    41. <span data-ttu-id="87dba-368">Wenn `I` das Schlüsselwort `New`, `E` `Me`, `MyBase` oder `MyClass` ist und keine Typargumente angegeben wurden, ist das Ergebnis eine Methoden Gruppe, die die Instanzkonstruktoren des Typs `E` mit dem zugeordneten Ziel Ausdruck `E` darstellt. und keine Typargument Liste.</span><span class="sxs-lookup"><span data-stu-id="87dba-368">If `I` is the keyword `New`, `E` is  `Me`, `MyBase`, or `MyClass`, and no type arguments were supplied, then the result is a method group representing the instance constructors of the type of `E` with an associated target expression of `E` and no type argument list.</span></span> <span data-ttu-id="87dba-369">Andernfalls tritt ein Kompilierungsfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-369">Otherwise, a compile-time error occurs.</span></span>
    42. <span data-ttu-id="87dba-370">Wenn `I` eine oder mehrere Methoden identifiziert, einschließlich der Erweiterungs Methoden, wenn `T` nicht `Object` ist, ist das Ergebnis eine Methoden Gruppe mit der zugeordneten Typargument Liste und einem zugeordneten Ziel Ausdruck `E`.</span><span class="sxs-lookup"><span data-stu-id="87dba-370">If `I` identifies one or more methods, including extension methods if `T` is not `Object`, then the result is a method group with the associated type argument list and an associated target expression of `E`.</span></span>
    43. <span data-ttu-id="87dba-371">Wenn `I` eine oder mehrere Eigenschaften identifiziert und keine Typargumente angegeben wurden, ist das Ergebnis eine Eigenschaften Gruppe mit einem zugeordneten Ziel Ausdruck `E`.</span><span class="sxs-lookup"><span data-stu-id="87dba-371">If `I` identifies one or more properties and no type arguments were supplied, then the result is a property group with an associated target expression of `E`.</span></span>
    44. <span data-ttu-id="87dba-372">Wenn `I` eine freigegebene Variable oder eine Instanzvariable identifiziert und keine Typargumente angegeben wurden, ist das Ergebnis entweder eine Variable oder ein Wert.</span><span class="sxs-lookup"><span data-stu-id="87dba-372">If `I` identifies a shared variable or an instance variable and no type arguments were supplied, then the result is either a variable or a value.</span></span> <span data-ttu-id="87dba-373">Wenn die Variable schreibgeschützt ist und der Verweis außerhalb eines Konstruktors der Klasse erfolgt, in der die Variable für die Art der Variablen (freigegeben oder Instanz) als geeignet deklariert ist, ist das Ergebnis der Wert der Variablen `I` in dem Objekt, auf das von verwiesen wird @no__ t-1.</span><span class="sxs-lookup"><span data-stu-id="87dba-373">If the variable is read-only, and the reference occurs outside a constructor of the class in which the variable is declared appropriate for the kind of variable (shared or instance), then the result is the value of the variable `I` in the object referenced by `E`.</span></span> <span data-ttu-id="87dba-374">Wenn `T` ein Referenztyp ist, ist das Ergebnis die Variable `I` in dem Objekt, auf das von `E` verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-374">If `T` is a reference type, then the result is the variable `I` in the object referenced by `E`.</span></span> <span data-ttu-id="87dba-375">Andernfalls ist das Ergebnis eine Variable, wenn `T` ein Werttyp ist und der Ausdruck `E` als Variable klassifiziert ist. Andernfalls ist das Ergebnis ein-Wert.</span><span class="sxs-lookup"><span data-stu-id="87dba-375">Otherwise, if `T` is a value type and the expression `E` is classified as a variable, the result is a variable; otherwise the result is a value.</span></span>
    45. <span data-ttu-id="87dba-376">Wenn `I` ein Ereignis identifiziert und keine Typargumente angegeben wurden, ist das Ergebnis ein Ereignis Zugriff mit einem zugeordneten Ziel Ausdruck `E`.</span><span class="sxs-lookup"><span data-stu-id="87dba-376">If `I` identifies an event and no type arguments were supplied, the result is an event access with an associated target expression of `E`.</span></span>
    46. <span data-ttu-id="87dba-377">Wenn `I` eine Konstante identifiziert und keine Typargumente angegeben wurden, ist das Ergebnis der Wert dieser Konstante.</span><span class="sxs-lookup"><span data-stu-id="87dba-377">If `I` identifies a constant and no type arguments were supplied, then the result is the value of that constant.</span></span>
    47. <span data-ttu-id="87dba-378">Wenn `I` einen Enumerationsmember identifiziert und keine Typargumente angegeben wurden, ist das Ergebnis der Wert dieses Enumerationsmembers.</span><span class="sxs-lookup"><span data-stu-id="87dba-378">If `I` identifies an enumeration member and no type arguments were supplied, then the result is the value of that enumeration member.</span></span>
    48. <span data-ttu-id="87dba-379">Wenn `T` `Object` ist, ist das Ergebnis eine spät gebundene Member-Suche, die als spät gebundener Zugriff mit der zugeordneten Typargument Liste und einem zugeordneten Ziel Ausdruck `E` klassifiziert ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-379">If `T` is `Object`, then the result is a late-bound member lookup classified as a late-bound access with the associated type argument list and an associated target expression of `E`.</span></span>

5. <span data-ttu-id="87dba-380">Andernfalls ist `E.I` ein ungültiger Element Verweis, und es tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-380">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>

<span data-ttu-id="87dba-381">Ein Element Zugriff auf das Formular `MyClass.I(Of A)` entspricht `Me.I(Of A)`, aber alle Elemente, auf die zugegriffen wird, werden so behandelt, als wären die Member nicht über schreibbar.</span><span class="sxs-lookup"><span data-stu-id="87dba-381">A member access of the form `MyClass.I(Of A)` is equivalent to `Me.I(Of A)`, but all members accessed on it are treated as if the members are non-overridable.</span></span> <span data-ttu-id="87dba-382">Folglich wird das Element, auf das zugegriffen wird, nicht durch den Lauf Zeittyp des Werts beeinflusst, auf den der Member zugreift.</span><span class="sxs-lookup"><span data-stu-id="87dba-382">Thus, the member accessed will not be affected by the run-time type of the value on which the member is being accessed.</span></span>

<span data-ttu-id="87dba-383">Ein Element Zugriff auf das Formular `MyBase.I(Of A)` entspricht `CType(Me, T).I(Of A)`, wobei `T` der direkte Basistyp des Typs ist, der den Member Access-Ausdruck enthält.</span><span class="sxs-lookup"><span data-stu-id="87dba-383">A member access of the form `MyBase.I(Of A)` is equivalent to `CType(Me, T).I(Of A)` where `T` is the direct base type of the type containing the member access expression.</span></span> <span data-ttu-id="87dba-384">Alle Methodenaufrufe werden so behandelt, als ob die aufgerufene Methode nicht über schreibbar ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-384">All method invocations on it are treated as if the method being invoked is non-overridable.</span></span> <span data-ttu-id="87dba-385">Diese Form des Member-Zugriffs wird auch als *Basis Zugriff*bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="87dba-385">This form of member access is also called a *base access*.</span></span>

<span data-ttu-id="87dba-386">Im folgenden Beispiel wird veranschaulicht, wie sich `Me`, `MyBase` und `MyClass` in Beziehung setzen:</span><span class="sxs-lookup"><span data-stu-id="87dba-386">The following example demonstrates how `Me`, `MyBase` and `MyClass` relate:</span></span>

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

<span data-ttu-id="87dba-387">Dieser Code gibt Folgendes aus:</span><span class="sxs-lookup"><span data-stu-id="87dba-387">This code prints out:</span></span>

```console
MoreDerived.F
Derived.F
Derived.F
```

<span data-ttu-id="87dba-388">Wenn ein Member-Zugriffs Ausdruck mit dem-Schlüsselwort `Global` beginnt, stellt das-Schlüsselwort den äußersten unbenannten Namespace dar. Dies ist in Situationen nützlich, in denen eine Deklaration einen einschließenden Namespace überschattet.</span><span class="sxs-lookup"><span data-stu-id="87dba-388">When a member access expression begins with the keyword `Global`, the keyword represents the outermost unnamed namespace, which is useful in situations where a declaration shadows an enclosing namespace.</span></span> <span data-ttu-id="87dba-389">Das Schlüsselwort "`Global`" ermöglicht das "Escapezeichen" im äußersten Namespace in dieser Situation.</span><span class="sxs-lookup"><span data-stu-id="87dba-389">The `Global` keyword allows "escaping" out to the outermost namespace in that situation.</span></span> <span data-ttu-id="87dba-390">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-390">For example:</span></span>

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

<span data-ttu-id="87dba-391">Im obigen Beispiel ist der erste Methoden Aufrufwert ungültig, da der Bezeichner `System` an die-Klasse `System` und nicht an den-Namespace `System` gebunden ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-391">In the above example, the first method call is invalid because the identifier `System` binds to the class `System`, not the namespace `System`.</span></span> <span data-ttu-id="87dba-392">Die einzige Möglichkeit, auf den Namespace "`System`" zuzugreifen, ist die Verwendung von `Global`, um den äußersten Namespace zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="87dba-392">The only way to access the `System` namespace is to use `Global` to escape out to the outermost namespace.</span></span>

<span data-ttu-id="87dba-393">Wenn das Element, auf das zugegriffen wird, freigegeben wird, ist jeder Ausdruck auf der linken Seite des Zeitraums überflüssig und wird nicht ausgewertet, es sei denn, der Element Zugriff wurde spät gebunden.</span><span class="sxs-lookup"><span data-stu-id="87dba-393">If the member being accessed is shared, any expression on the left side of the period is superfluous and is not evaluated unless the member access is done late-bound.</span></span> <span data-ttu-id="87dba-394">Beachten Sie z. B. folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="87dba-394">For example, consider the following code:</span></span>

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

<span data-ttu-id="87dba-395">Er gibt `The value of F is: 10` aus, da die Funktion `ReturnC` nicht aufgerufen werden muss, um eine Instanz von `C` für den Zugriff auf den freigegebenen Member `F` bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="87dba-395">It prints `The value of F is: 10` because the function `ReturnC` does not need to be called to provide an instance of `C` to access the shared member `F`.</span></span>


### <a name="identical-type-and-member-names"></a><span data-ttu-id="87dba-396">Identische Typen-und Elementnamen</span><span class="sxs-lookup"><span data-stu-id="87dba-396">Identical Type and Member Names</span></span>

<span data-ttu-id="87dba-397">Es ist nicht ungewöhnlich, dass Sie Member mit dem gleichen Namen wie deren Typ benennen.</span><span class="sxs-lookup"><span data-stu-id="87dba-397">It is not uncommon to name members using the same name as their type.</span></span> <span data-ttu-id="87dba-398">In dieser Situation kann jedoch ein unbequyes namens ausblenden auftreten:</span><span class="sxs-lookup"><span data-stu-id="87dba-398">In that situation, however, inconvenient name hiding can occur:</span></span>

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

<span data-ttu-id="87dba-399">Im vorherigen Beispiel wird der einfache Name `Color` in `DefaultColor` an die Instanzeigenschaft anstatt an den-Typ gebunden.</span><span class="sxs-lookup"><span data-stu-id="87dba-399">In the previous example, the simple name `Color` in `DefaultColor` binds to the instance property instead of the type.</span></span> <span data-ttu-id="87dba-400">Da in einem freigegebenen Member nicht auf einen Instanzmember verwiesen werden kann, ist dies normalerweise ein Fehler.</span><span class="sxs-lookup"><span data-stu-id="87dba-400">Because an instance member cannot be referenced in a shared member, this would normally be an error.</span></span>

<span data-ttu-id="87dba-401">In diesem Fall kann jedoch eine spezielle Regel auf den-Typ zugreifen.</span><span class="sxs-lookup"><span data-stu-id="87dba-401">However, a special rule allows access to the type in this case.</span></span> <span data-ttu-id="87dba-402">Wenn der Basis Ausdruck eines Element Zugriffs Ausdrucks ein einfacher Name ist und an eine Konstante, ein Feld, eine Eigenschaft, eine lokale Variable oder einen Parameter gebunden ist, deren Typ denselben Namen hat, kann der Basis Ausdruck entweder auf den Member oder den Typ verweisen.</span><span class="sxs-lookup"><span data-stu-id="87dba-402">If the base expression of a member access expression is a simple name and binds to a constant, field, property, local variable or parameter whose type has the same name, then the base expression can refer either to the member or the type.</span></span> <span data-ttu-id="87dba-403">Dies kann niemals zu Mehrdeutigkeit führen, da die Member, auf die von beiden eines zugegriffen werden kann, identisch sind.</span><span class="sxs-lookup"><span data-stu-id="87dba-403">This can never result in ambiguity because the members that can be accessed off of either one are the same.</span></span>

### <a name="default-instances"></a><span data-ttu-id="87dba-404">Standard Instanzen</span><span class="sxs-lookup"><span data-stu-id="87dba-404">Default Instances</span></span>

<span data-ttu-id="87dba-405">In einigen Fällen haben Klassen, die von einer gemeinsamen Basisklasse abgeleitet sind, in der Regel nur eine einzige Instanz.</span><span class="sxs-lookup"><span data-stu-id="87dba-405">In some situations, classes derived from a common base class usually or always have only a single instance.</span></span> <span data-ttu-id="87dba-406">Beispielsweise verfügen die meisten Fenster, die auf einer Benutzeroberfläche angezeigt werden, immer nur über eine Instanz, die auf dem Bildschirm angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-406">For example, most windows shown in a user interface only ever have one instance showing on the screen at any time.</span></span> <span data-ttu-id="87dba-407">Um die Arbeit mit diesen Klassentypen zu vereinfachen, können Visual Basic automatisch *Standard Instanzen* der Klassen generieren, die eine einzelne, leicht referenzierte Instanz für jede Klasse bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="87dba-407">To simplify working with these types of classes, Visual Basic can automatically generate *default instances* of the classes that provide a single, easily referenced instance for each class.</span></span>

<span data-ttu-id="87dba-408">Standard Instanzen werden immer für eine Typen *Familie* erstellt, nicht für einen bestimmten Typ.</span><span class="sxs-lookup"><span data-stu-id="87dba-408">Default instances are always created for a *family* of types rather than for one particular type.</span></span> <span data-ttu-id="87dba-409">Anstatt eine Standard Instanz für eine Klasse Form1 zu erstellen, die von Form abgeleitet wird, werden Standard Instanzen für alle Klassen erstellt, die vom Formular abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-409">So instead of creating a default instance for a class Form1 that derives from Form, default instances are created for all classes derived from Form.</span></span> <span data-ttu-id="87dba-410">Dies bedeutet, dass jede einzelne Klasse, die von der Basisklasse abgeleitet wird, nicht speziell für eine Standard Instanz gekennzeichnet werden muss.</span><span class="sxs-lookup"><span data-stu-id="87dba-410">This means that each individual class that derives from the base class does not have to be specially marked to have a default instance.</span></span>

<span data-ttu-id="87dba-411">Die Standard Instanz einer Klasse wird durch eine vom Compiler generierte Eigenschaft dargestellt, die die Standard Instanz dieser Klasse zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="87dba-411">The default instance of a class is represented by a compiler-generated property that returns the default instance of that class.</span></span> <span data-ttu-id="87dba-412">Die Eigenschaft, die als Member einer Klasse mit dem Namen " *Group class* " generiert wurde, die das zuordnen und zerstören von Standard Instanzen für alle Klassen verwaltet, die von der jeweiligen Basisklasse abgeleitet sind.</span><span class="sxs-lookup"><span data-stu-id="87dba-412">The property generated as a member of a class called the *group class* that manages allocating and destroying default instances for all classes derived from the particular base class.</span></span> <span data-ttu-id="87dba-413">Beispielsweise können alle standardinstanzeigenschaften von Klassen, die von `Form` abgeleitet sind, in der `MyForms`-Klasse gesammelt werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-413">For example, all of the default instance properties of classes derived from `Form` may be collected in the `MyForms` class.</span></span> <span data-ttu-id="87dba-414">Wenn eine Instanz der Group-Klasse vom Ausdruck `My.Forms` zurückgegeben wird, greift der folgende Code auf die Standard Instanzen abgeleiteter Klassen zu `Form1` und `Form2`:</span><span class="sxs-lookup"><span data-stu-id="87dba-414">If an instance of the group class is returned by the expression `My.Forms`, then the following code accesses the default instances of derived classes `Form1` and `Form2`:</span></span>

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

<span data-ttu-id="87dba-415">Standard Instanzen werden erst erstellt, wenn der erste Verweis darauf besteht. das Abrufen der Eigenschaft, die die Standard Instanz darstellt, bewirkt, dass die Standard Instanz erstellt wird, wenn Sie nicht bereits erstellt wurde oder `Nothing` festgelegt wurde.</span><span class="sxs-lookup"><span data-stu-id="87dba-415">Default instances will not be created until the first reference to them; fetching the property representing the default instance causes the default instance to be created if it has not already been created or has been set to `Nothing`.</span></span> <span data-ttu-id="87dba-416">Um zu testen, ob eine Standard Instanz vorhanden ist, wird die Standard Instanz nicht erstellt, wenn eine Standard Instanz das Ziel eines `Is`-oder `IsNot`-Operators ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-416">To allow testing for the existence of a default instance, when a default instance is the target of an `Is` or `IsNot` operator, the default instance will not be created.</span></span> <span data-ttu-id="87dba-417">Daher ist es möglich, zu testen, ob eine Standard Instanz `Nothing` oder ein anderer Verweis ist, ohne dass die Standard Instanz erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-417">Thus, it is possible to test whether a default instance is `Nothing` or some other reference without causing the default instance to be created.</span></span>

<span data-ttu-id="87dba-418">Standard Instanzen sollen auf einfache Weise von außerhalb der Klasse mit der Standard Instanz auf die Standard Instanz verweisen.</span><span class="sxs-lookup"><span data-stu-id="87dba-418">Default instances are intended to make it easy to refer to the default instance from outside of the class that has the default instance.</span></span> <span data-ttu-id="87dba-419">Die Verwendung einer Standard Instanz aus einer Klasse, die diese definiert, kann Verwirrung verursachen, wenn auf die Instanz verwiesen wird, d. h. die Standard Instanz oder die aktuelle Instanz.</span><span class="sxs-lookup"><span data-stu-id="87dba-419">Using a default instance from within a class that defines it might cause confusion as to which instance is being referred to, i.e. the default instance or the current instance.</span></span> <span data-ttu-id="87dba-420">Der folgende Code ändert z. b. nur den Wert `x` in der Standard Instanz, auch wenn er von einer anderen Instanz aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-420">For example, the following code modifies only the value `x` in the default instance, even though it is being called from another instance.</span></span> <span data-ttu-id="87dba-421">Daher würde der Code den Wert `5` anstelle von `10` ausgeben:</span><span class="sxs-lookup"><span data-stu-id="87dba-421">Thus the code would print the value `5` instead of `10`:</span></span>

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

<span data-ttu-id="87dba-422">Um diese Art von Verwirrung zu vermeiden, ist es nicht zulässig, in einer Instanzmethode des Typs der Standard Instanz auf eine Standard Instanz zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="87dba-422">To prevent this kind of confusion, it is not valid to refer to a default instance from within an instance method of the default instance's type.</span></span>

#### <a name="default-instances-and-type-names"></a><span data-ttu-id="87dba-423">Standard Instanzen und Typnamen</span><span class="sxs-lookup"><span data-stu-id="87dba-423">Default Instances and Type Names</span></span>

<span data-ttu-id="87dba-424">Auf eine Standard Instanz kann auch direkt über den Namen des Typs zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-424">A default instance may also be accessible directly through its type's name.</span></span> <span data-ttu-id="87dba-425">In diesem Fall wird in jedem Ausdrucks Kontext, in dem der Typname nicht zulässig ist, der Ausdruck `E`, wobei `E` den voll qualifizierten Namen der Klasse mit einer Standard Instanz darstellt, in `E'` geändert, wobei `E'` einen Ausdruck darstellt, der abruft. die standardinstanzeigenschaft.</span><span class="sxs-lookup"><span data-stu-id="87dba-425">In this case, in any expression context where the type name is not allowed the expression `E`, where `E` represents the fully qualified name of the class with a default instance, is changed to `E'`, where `E'` represents an expression that fetches the default instance property.</span></span> <span data-ttu-id="87dba-426">Wenn beispielsweise Standard Instanzen für Klassen, die von `Form` abgeleitet sind, den Zugriff auf die Standard Instanz über den Typnamen zulassen, entspricht der folgende Code dem Code im vorherigen Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-426">For example, if default instances for classes derived from `Form` allow accessing the default instance through the type name, then the following code is equivalent to the code in the previous example:</span></span>

```vb
Module Main
    Sub Main()
        Form1.x = 10
        Console.WriteLine(Form2.y)
    End Sub
End Module
```

<span data-ttu-id="87dba-427">Dies bedeutet auch, dass eine Standard Instanz, die über den Namen des Typs zugänglich ist, auch über den Typnamen zugewiesen werden kann.</span><span class="sxs-lookup"><span data-stu-id="87dba-427">This also means that a default instance that is accessible through its type's name is also assignable through the type name.</span></span> <span data-ttu-id="87dba-428">Der folgende Code legt z. b. die Standard Instanz von `Form1` auf `Nothing` fest:</span><span class="sxs-lookup"><span data-stu-id="87dba-428">For example, the following code sets the default instance of `Form1` to `Nothing`:</span></span>

```vb
Module Main
    Sub Main()
        Form1 = Nothing
    End Sub
End Module
```

<span data-ttu-id="87dba-429">Beachten Sie, dass die Bedeutung von "`E.I`" `E` eine Klasse darstellt und `I` einen freigegebenen Member darstellt, der sich nicht ändert.</span><span class="sxs-lookup"><span data-stu-id="87dba-429">Note that the meaning of `E.I` were `E` represents a class and `I` represents a shared member does not change.</span></span> <span data-ttu-id="87dba-430">Ein solcher Ausdruck greift weiterhin direkt aus der Klasseninstanz auf den freigegebenen Member zu und verweist nicht auf die Standard Instanz.</span><span class="sxs-lookup"><span data-stu-id="87dba-430">Such an expression still accesses the shared member directly off of the class instance and does not reference the default instance.</span></span>

#### <a name="group-classes"></a><span data-ttu-id="87dba-431">Gruppen Klassen</span><span class="sxs-lookup"><span data-stu-id="87dba-431">Group Classes</span></span>

<span data-ttu-id="87dba-432">Das Attribut "`Microsoft.VisualBasic.MyGroupCollectionAttribute`" gibt die Gruppenklasse für eine Familie von Standard Instanzen an.</span><span class="sxs-lookup"><span data-stu-id="87dba-432">The `Microsoft.VisualBasic.MyGroupCollectionAttribute` attribute indicates the group class for a family of default instances.</span></span> <span data-ttu-id="87dba-433">Das-Attribut verfügt über vier Parameter:</span><span class="sxs-lookup"><span data-stu-id="87dba-433">The attribute has four parameters:</span></span>

* <span data-ttu-id="87dba-434">Der Parameter "`TypeToCollect`" gibt die Basisklasse für die Gruppe an.</span><span class="sxs-lookup"><span data-stu-id="87dba-434">The parameter `TypeToCollect` specifies the base class for the group.</span></span> <span data-ttu-id="87dba-435">Alle instanziier baren Klassen ohne geöffnete Typparameter, die von einem Typ mit diesem Namen abgeleitet werden (unabhängig von Typparametern), verfügen automatisch über eine Standard Instanz.</span><span class="sxs-lookup"><span data-stu-id="87dba-435">All instantiable classes without open type parameters that derive from a type with this name (regardless of type parameters) will automatically have a default instance.</span></span>

* <span data-ttu-id="87dba-436">Der-Parameter `CreateInstanceMethodName` gibt die Methode an, die in der Group-Klasse aufgerufen wird, um eine neue-Instanz in einer standardinstanzeigenschaft zu erstellen</span><span class="sxs-lookup"><span data-stu-id="87dba-436">The parameter `CreateInstanceMethodName` specifies the method to call in the group class to create a new instance in a default instance property.</span></span>

* <span data-ttu-id="87dba-437">Der-Parameter `DisposeInstanceMethodName` gibt die Methode an, die in der Group-Klasse aufgerufen wird, um eine standardinstanzeigenschaft zu verwerfen, wenn der Wert `Nothing` der standardinstanzeigenschaft zugewiesen wird</span><span class="sxs-lookup"><span data-stu-id="87dba-437">The parameter `DisposeInstanceMethodName` specifies the method to call in the group class to dispose of a default instance property if the default instance property is assigned the value `Nothing`.</span></span>

* <span data-ttu-id="87dba-438">Der-Parameter `DefaultInstanceAlias` ist der Ausdruck `E'`, um den Klassennamen zu ersetzen, wenn der Zugriff auf die Standard Instanzen direkt über den Typnamen möglich ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-438">The parameter `DefaultInstanceAlias` specifics the expression `E'` to substitute for the class name if the default instances are accessible directly through their type name.</span></span> <span data-ttu-id="87dba-439">Wenn dieser Parameter `Nothing` oder eine leere Zeichenfolge ist, kann auf Standard Instanzen für diesen Gruppentyp nicht direkt über den Namen des Typs zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-439">If this parameter is `Nothing` or an empty string, default instances on this group type are not accessible directly through their type's name.</span></span> <span data-ttu-id="87dba-440">(__Hinweis:__</span><span class="sxs-lookup"><span data-stu-id="87dba-440">(__Note.__</span></span> <span data-ttu-id="87dba-441">In allen aktuellen Implementierungen der Visual Basic Sprache wird der Parameter "`DefaultInstanceAlias`" ignoriert, außer im vom Compiler bereitgestellten Code.)</span><span class="sxs-lookup"><span data-stu-id="87dba-441">In all current implementations of the Visual Basic language, the `DefaultInstanceAlias` parameter is ignored, except in compiler-provided code.)</span></span>

<span data-ttu-id="87dba-442">Mehrere Typen können in derselben Gruppe gesammelt werden, indem die Namen der Typen und Methoden in den ersten drei Parametern mithilfe von Kommas getrennt werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-442">Multiple types can be collected into the same group by separating the names of the types and methods in the first three parameters using commas.</span></span> <span data-ttu-id="87dba-443">In jedem Parameter muss die gleiche Anzahl von Elementen vorhanden sein, und die Listenelemente werden in der Reihenfolge abgeglichen.</span><span class="sxs-lookup"><span data-stu-id="87dba-443">There must be the same number of items in each parameter, and the list elements are matched in order.</span></span> <span data-ttu-id="87dba-444">Die folgende Attribut Deklaration sammelt beispielsweise Typen, die von `C1`, `C2` oder `C3` abgeleitet sind, in einer einzelnen Gruppe:</span><span class="sxs-lookup"><span data-stu-id="87dba-444">For example, the following attribute declaration collects types that derive from `C1`, `C2` or `C3` into a single group:</span></span>

```vb
<Microsoft.VisualBasic.MyGroupCollection("C1, C2, C3", _
    "CreateC1, CreateC2, CreateC3", _
    "DisposeC1, DisposeC2, DisposeC3", "My.Cs")>
Public NotInheritable Class MyCs
    ...
End Class
```

<span data-ttu-id="87dba-445">Die Signatur der Create-Methode muss die Form `Shared Function <Name>(Of T As {New, <Type>})(Instance Of T) As T` aufweisen.</span><span class="sxs-lookup"><span data-stu-id="87dba-445">The signature of the create method must be of the form `Shared Function <Name>(Of T As {New, <Type>})(Instance Of T) As T`.</span></span> <span data-ttu-id="87dba-446">Die verwerfen-Methode muss die Form `Shared Sub <Name>(Of T As <Type>)(ByRef Instance Of T)` aufweisen.</span><span class="sxs-lookup"><span data-stu-id="87dba-446">The dispose method must be of the form `Shared Sub <Name>(Of T As <Type>)(ByRef Instance Of T)`.</span></span> <span data-ttu-id="87dba-447">Die Group-Klasse für das Beispiel im vorherigen Abschnitt könnte daher wie folgt deklariert werden:</span><span class="sxs-lookup"><span data-stu-id="87dba-447">Thus, the group class for the example in the preceding section could be declared as follows:</span></span>

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

<span data-ttu-id="87dba-448">Wenn eine Quelldatei eine abgeleitete Klasse `Form1` deklariert hat, wäre die generierte Gruppenklasse Äquivalent zu:</span><span class="sxs-lookup"><span data-stu-id="87dba-448">If a source file declared a derived class `Form1`, the generated group class would be equivalent to:</span></span>

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

### <a name="extension-method-collection"></a><span data-ttu-id="87dba-449">Erweiterungs Methoden Auflistung</span><span class="sxs-lookup"><span data-stu-id="87dba-449">Extension Method Collection</span></span>

<span data-ttu-id="87dba-450">Erweiterungs Methoden für den Member-Zugriffs Ausdruck `E.I` werden gesammelt, indem alle Erweiterungs Methoden mit dem Namen `I`, die im aktuellen Kontext verfügbar sind, erfasst werden:</span><span class="sxs-lookup"><span data-stu-id="87dba-450">Extension methods for the member access expression `E.I` are collected by gathering all of the extension methods with the name `I` that are available in the current context:</span></span>

1. <span data-ttu-id="87dba-451">Zuerst wird jeder eingegebene Typ, der den Ausdruck enthält, geprüft, beginnend mit dem innersten und dem äußersten.</span><span class="sxs-lookup"><span data-stu-id="87dba-451">First, each nested type containing the expression is checked, starting from the innermost and going to the outermost.</span></span>
2. <span data-ttu-id="87dba-452">Anschließend wird jeder eingegebene Namespace geprüft, beginnend am innersten und zum äußersten Namespace.</span><span class="sxs-lookup"><span data-stu-id="87dba-452">Then, each nested namespace is checked, starting from the innermost and going to the outermost namespace.</span></span>
3. <span data-ttu-id="87dba-453">Anschließend werden die Importe in der Quelldatei geprüft.</span><span class="sxs-lookup"><span data-stu-id="87dba-453">Then, the imports in the source file are checked.</span></span>
4. <span data-ttu-id="87dba-454">Anschließend werden die von der Kompilierungs Umgebung definierten Importe geprüft.</span><span class="sxs-lookup"><span data-stu-id="87dba-454">Then, the imports defined by the compilation environment are checked.</span></span>

<span data-ttu-id="87dba-455">Eine Erweiterungsmethode wird nur gesammelt, wenn eine erweiternde Native Konvertierung vom Ziel Ausdruckstyp in den Typ des ersten Parameters der Erweiterungsmethode vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-455">An extension method is collected only if there is a widening native conversion from the target expression type to the type of the first parameter of the extension method.</span></span> <span data-ttu-id="87dba-456">Und im Gegensatz zu regulären Simple Name Expression-Bindungen sammelt die Suche *alle* Erweiterungs Methoden. die Auflistung wird nicht angehalten, wenn eine Erweiterungsmethode gefunden wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-456">And unlike regular simple name expression binding, the search collects *all* extension methods; the collection does not stop when an extension method is found.</span></span> <span data-ttu-id="87dba-457">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-457">For example:</span></span>

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

<span data-ttu-id="87dba-458">In diesem Beispiel werden beide als Erweiterungs Methoden betrachtet, obwohl `N2C1Extensions.M1` vor `N1C1Extensions.M1` gefunden wurde.</span><span class="sxs-lookup"><span data-stu-id="87dba-458">In this example, even though `N2C1Extensions.M1` is found before `N1C1Extensions.M1`, they both are considered as extension methods.</span></span> <span data-ttu-id="87dba-459">Nachdem alle Erweiterungs Methoden erfasst wurden, werden Sie dann in den *Cursor*aufgenommen.</span><span class="sxs-lookup"><span data-stu-id="87dba-459">Once all of the extension methods have been collected, they are then *curried*.</span></span> <span data-ttu-id="87dba-460">Currying nimmt das Ziel des Erweiterungs Methoden Aufrufes an und wendet es auf den Aufrufen der Erweiterungsmethode an, was zu einer neuen Methoden Signatur führt, bei der der erste Parameter entfernt wurde (da er angegeben wurde).</span><span class="sxs-lookup"><span data-stu-id="87dba-460">Currying takes the target of the extension method call and applies it to the extension method call, resulting in a new method signature with the first parameter removed (because it has been specified).</span></span> <span data-ttu-id="87dba-461">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-461">For example:</span></span>

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

<span data-ttu-id="87dba-462">Im obigen Beispiel ist das Curry-Ergebnis der Anwendung von `v` auf `Ext1.M` die Methoden Signatur `Sub M(y As Integer)`.</span><span class="sxs-lookup"><span data-stu-id="87dba-462">In the above example, the curried result of applying `v` to `Ext1.M` is the method signature `Sub M(y As Integer)`.</span></span>

<span data-ttu-id="87dba-463">Zusätzlich zum Entfernen des ersten Parameters der Erweiterungsmethode entfernt Currying auch alle Methodentypparameter, die Teil des Typs des ersten Parameters sind.</span><span class="sxs-lookup"><span data-stu-id="87dba-463">In addition to removing the first parameter of the extension method, currying also removes any method type parameters that are a part of the type of the first parameter.</span></span> <span data-ttu-id="87dba-464">Wenn eine Erweiterungsmethode mit dem Methodentypparameter verwendet wird, wird der Typrückschluss auf den ersten Parameter angewendet, und das Ergebnis wird für alle Typparameter korrigiert, die abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-464">When currying an extension method with method type parameter, type inference is applied to the first parameter and the result is fixed for any type parameters that are inferred.</span></span> <span data-ttu-id="87dba-465">Wenn der Typrückschluss fehlschlägt, wird die Methode ignoriert.</span><span class="sxs-lookup"><span data-stu-id="87dba-465">If type inference fails, the method is ignored.</span></span> <span data-ttu-id="87dba-466">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-466">For example:</span></span>

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

<span data-ttu-id="87dba-467">Im obigen Beispiel ist das Curry-Ergebnis der Anwendung von `v` auf `Ext1.M` die Methoden Signatur `Sub M(Of U)(y As U)`, da der Typparameter `T` als Ergebnis der Currying abgeleitet und nun korrigiert wurde.</span><span class="sxs-lookup"><span data-stu-id="87dba-467">In the above example, the curried result of applying `v` to `Ext1.M` is the method signature `Sub M(Of U)(y As U)`, because the type parameter `T` is inferred as a result of the currying and is now fixed.</span></span> <span data-ttu-id="87dba-468">Da der Typparameter `U` nicht als Teil der Currying abgeleitet wurde, bleibt er ein offener Parameter.</span><span class="sxs-lookup"><span data-stu-id="87dba-468">Because the type parameter `U` was not inferred as a part of the currying, it remains an open parameter.</span></span> <span data-ttu-id="87dba-469">Ebenso, weil der Typparameter `T` als Ergebnis der Anwendung von `v` auf `Ext2.M` abgeleitet wird, wird der Parametertyp `y` als `Integer` korrigiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-469">Similarly, because the type parameter `T` is inferred as a result of applying `v` to `Ext2.M`, the type of parameter `y` becomes fixed as `Integer`.</span></span> <span data-ttu-id="87dba-470">Er wird nicht als beliebiger anderer Typ abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="87dba-470">It will not be inferred to be any other type.</span></span> <span data-ttu-id="87dba-471">Bei der Erstellung der Signatur werden alle Einschränkungen außer `New`-Einschränkungen ebenfalls angewendet.</span><span class="sxs-lookup"><span data-stu-id="87dba-471">When currying the signature, all constraints except for `New` constraints are also applied.</span></span> <span data-ttu-id="87dba-472">Wenn die Einschränkungen nicht erfüllt werden oder von einem Typ abhängen, der nicht als Teil von Currying abgeleitet wurde, wird die Erweiterungsmethode ignoriert.</span><span class="sxs-lookup"><span data-stu-id="87dba-472">If the constraints are not satisfied, or depend on a type that was not inferred as a part of currying, the extension method is ignored.</span></span> <span data-ttu-id="87dba-473">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-473">For example:</span></span>

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

<span data-ttu-id="87dba-474">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-474">__Note.__</span></span> <span data-ttu-id="87dba-475">Einer der Hauptgründe für das Ausführen von Erweiterungs Methoden besteht darin, dass Abfrage Ausdrücke den Typ der Iterationen ableiten können, bevor die Argumente für eine Abfrage Muster Methode ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-475">One of the main reasons for doing currying of extension methods is that it allows query expressions to infer the type of the iteration before evaluating the arguments to a query pattern method.</span></span> <span data-ttu-id="87dba-476">Da die meisten Abfrage Muster Methoden Lambda-Ausdrücke akzeptieren, die den Typrückschluss selbst erfordern, vereinfacht dies den Prozess der Auswertung eines Abfrage Ausdrucks erheblich.</span><span class="sxs-lookup"><span data-stu-id="87dba-476">Since most query pattern methods take lambda expressions, which require type inference themselves, this greatly simplifies the process of evaluating a query expression.</span></span>

<span data-ttu-id="87dba-477">Anders als bei der normalen Schnittstellen Vererbung stehen Erweiterungs Methoden zur Verfügung, die zwei Schnittstellen erweitern, die nicht miteinander in Beziehung stehen, sofern Sie nicht die gleiche geschweifende Signatur aufweisen:</span><span class="sxs-lookup"><span data-stu-id="87dba-477">Unlike normal interface inheritance, extension methods that extend two interfaces that do not relate to one another are available, as long as they do not have the same curried signature:</span></span>

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

<span data-ttu-id="87dba-478">Schließlich ist es wichtig zu beachten, dass Erweiterungs Methoden beim Ausführen späterer Bindungen nicht berücksichtigt werden:</span><span class="sxs-lookup"><span data-stu-id="87dba-478">Finally, it is important to remember that extension methods are not considered when doing late binding:</span></span>

```vb
Module Test
    Sub Main()
        Dim o As Object = ...

        ' Ignores extension methods
        o.M1()
    End Sub
End Module
```

## <a name="dictionary-member-access-expressions"></a><span data-ttu-id="87dba-479">Zugriffs Ausdrücke für Wörterbuch Elemente</span><span class="sxs-lookup"><span data-stu-id="87dba-479">Dictionary Member Access Expressions</span></span>

<span data-ttu-id="87dba-480">Ein *Wörterbuch-Member-Zugriffs Ausdruck* wird verwendet, um einen Member einer Auflistung zu suchen.</span><span class="sxs-lookup"><span data-stu-id="87dba-480">A *dictionary member access expression* is used to look up a member of a collection.</span></span> <span data-ttu-id="87dba-481">Ein Zugriff auf Wörterbuch Elemente hat die Form `E!I`, wobei `E` ein Ausdruck ist, der als Wert klassifiziert wird, und `I` ein Bezeichner ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-481">A dictionary member access takes the form of `E!I`, where `E` is an expression that is classified as a value and `I` is an identifier.</span></span>

```antlr
DictionaryAccessExpression
    : Expression? '!' IdentifierOrKeyword
    ;
```

<span data-ttu-id="87dba-482">Der Typ des Ausdrucks muss über eine Standard Eigenschaft verfügen, die durch einen einzelnen `String`-Parameter indiziert wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-482">The type of the expression must have a default property indexed by a single `String` parameter.</span></span> <span data-ttu-id="87dba-483">Der "Dictionary Member Access"-Ausdruck "`E!I`" wird in den Ausdruck `E.D("I")` transformiert, wobei "`D`" die Standard Eigenschaft von "`E`" ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-483">The dictionary member access expression `E!I` is transformed into the expression `E.D("I")`, where `D` is the default property of `E`.</span></span> <span data-ttu-id="87dba-484">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-484">For example:</span></span>

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

<span data-ttu-id="87dba-485">Wenn ein Ausrufezeichen ohne Ausdruck angegeben wird, wird der Ausdruck aus der direkt enthaltenden `With`-Anweisung angenommen.</span><span class="sxs-lookup"><span data-stu-id="87dba-485">If an exclamation point is specified with no expression, the expression from the immediately containing `With` statement is assumed.</span></span> <span data-ttu-id="87dba-486">Wenn keine `With`-Anweisung enthalten ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-486">If there is no containing `With` statement, a compile-time error occurs.</span></span>


## <a name="invocation-expressions"></a><span data-ttu-id="87dba-487">Aufrufausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-487">Invocation Expressions</span></span>

<span data-ttu-id="87dba-488">Ein Aufruf Ausdruck besteht aus einem Aufruf Ziel und einer optionalen Argumentliste.</span><span class="sxs-lookup"><span data-stu-id="87dba-488">An invocation expression consists of an invocation target and an optional argument list.</span></span>

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

<span data-ttu-id="87dba-489">Der Ziel Ausdruck muss als Methoden Gruppe oder als Wert mit einem Delegattyp klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-489">The target expression must be classified as a method group or a value whose type is a delegate type.</span></span> <span data-ttu-id="87dba-490">Wenn der Ziel Ausdruck ein Wert ist, dessen Typ ein Delegattyp ist, wird das Ziel des Aufruf Ausdrucks zur Methoden Gruppe für das `Invoke`-Member des Delegattyps, und der Ziel Ausdruck wird zum zugeordneten Ziel Ausdruck der Methoden Gruppe.</span><span class="sxs-lookup"><span data-stu-id="87dba-490">If the target expression is a value whose type is a delegate type, then the target of the invocation expression becomes the method group for the `Invoke` member of the delegate type and the target expression becomes the associated target expression of the method group.</span></span>

<span data-ttu-id="87dba-491">Eine Argumentliste besteht aus zwei Abschnitten: Positions Argumente und benannte Argumente.</span><span class="sxs-lookup"><span data-stu-id="87dba-491">An argument list has two sections: positional arguments and named arguments.</span></span> <span data-ttu-id="87dba-492">*Positions Argumente* sind Ausdrücke und müssen allen benannten Argumenten vorangestellt sein.</span><span class="sxs-lookup"><span data-stu-id="87dba-492">*Positional arguments* are expressions and must precede any named arguments.</span></span> <span data-ttu-id="87dba-493">*Benannte Argumente* beginnen mit einem Bezeichner, der Schlüsselwörter entsprechen kann, gefolgt von `:=` und einem Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="87dba-493">*Named arguments* start with an identifier that can match keywords, followed by `:=` and an expression.</span></span>

<span data-ttu-id="87dba-494">Wenn die Methoden Gruppe nur eine barrierefreie Methode enthält, einschließlich der Instanz-und Erweiterungs Methoden, und diese Methode keine Argumente annimmt und eine Funktion ist, wird die Methoden Gruppe als Aufruf Ausdruck mit einer leeren Argumentliste interpretiert, und das Ergebnis ist wird als Ziel eines Aufruf Ausdrucks mit der angegebenen Argumentliste (n) verwendet.</span><span class="sxs-lookup"><span data-stu-id="87dba-494">If the method group only contains one accessible method, including both instance and extension methods, and that method takes no arguments and is a function, then the method group is interpreted as an invocation expression with an empty argument list and the result is used as the target of an invocation expression with the provided argument list(s).</span></span> <span data-ttu-id="87dba-495">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-495">For example:</span></span>

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

<span data-ttu-id="87dba-496">Andernfalls wird die Überladungs Auflösung auf die Methoden angewendet, um die am häufigsten anwendbare Methode für die angegebene Argumentliste (n) auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="87dba-496">Otherwise, overload resolution is applied to the methods to pick the most applicable method for the given argument list(s).</span></span> <span data-ttu-id="87dba-497">Wenn die am meisten anwendbare Methode eine Funktion ist, wird das Ergebnis des Aufruf Ausdrucks als Wert klassifiziert, der als Rückgabetyp der Funktion typisiert ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-497">If the most applicable method is a function, then the result of the invocation expression is classified as a value typed as the return type of the function.</span></span> <span data-ttu-id="87dba-498">Wenn die am meisten anwendbare Methode eine Unterroutine ist, wird das Ergebnis als void klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="87dba-498">If the most applicable method is a subroutine, then the result is classified as void.</span></span> <span data-ttu-id="87dba-499">Wenn es sich bei der am meisten anwendbaren Methode um eine partielle Methode handelt, die keinen Text enthält, wird der Aufruf Ausdruck ignoriert, und das Ergebnis wird als void klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="87dba-499">If the most applicable method is a partial method that has no body, then the invocation expression is ignored and the result is classified as void.</span></span>

<span data-ttu-id="87dba-500">Für einen früh gebundenen Aufruf Ausdruck werden die Argumente in der Reihenfolge ausgewertet, in der die entsprechenden Parameter in der Ziel Methode deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-500">For an early-bound invocation expression, the arguments are evaluated in the order in which the corresponding parameters are declared in the target method.</span></span> <span data-ttu-id="87dba-501">Für einen Ausdruck mit einem spät gebundenen Member werden Sie in der Reihenfolge ausgewertet, in der Sie im Member-Zugriffs Ausdruck angezeigt werden: siehe Abschnitt [spät gebundene Ausdrücke](expressions.md#late-bound-expressions).</span><span class="sxs-lookup"><span data-stu-id="87dba-501">For a late-bound member access expression, they are evaluated in the order in which they appear in the member access expression: see Section [Late-Bound Expressions](expressions.md#late-bound-expressions).</span></span>

## <a name="overloaded-method-resolution"></a><span data-ttu-id="87dba-502">Auflösung der überladenen Methode:</span><span class="sxs-lookup"><span data-stu-id="87dba-502">Overloaded Method Resolution:</span></span>
<span data-ttu-id="87dba-503">Für die Überladungs Auflösung, eine Spezifizität von Membern/Typen mit einer Argumentliste, Generizität, Anwendbarkeit in Argumentliste, Übergabe von Argumenten und Auswahl von Argumenten für optionale Parameter, bedingte Methoden und das Ableiten von Typargumenten: siehe Abschnitt [ Überladungs Auflösung](overload-resolution.md).</span><span class="sxs-lookup"><span data-stu-id="87dba-503">For Overload Resolution, Specificity of members/types given an argument list, Genericity, Applicability to Argument List, Passing Arguments, and Picking Arguments for Optional Parameters, Conditional Methods, and Type Argument Inference: see Section [Overload Resolution](overload-resolution.md).</span></span>

## <a name="index-expressions"></a><span data-ttu-id="87dba-504">Index Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-504">Index Expressions</span></span>

<span data-ttu-id="87dba-505">Ein *Index Ausdruck* führt zu einem Array Element oder klassifiziert eine Eigenschaften Gruppe in einen Eigenschaften Zugriff neu.</span><span class="sxs-lookup"><span data-stu-id="87dba-505">An *index expression* results in an array element or reclassifies a property group into a property access.</span></span> <span data-ttu-id="87dba-506">Ein Index Ausdruck besteht aus einem Ausdruck, einer öffnenden Klammer, einer Index Argumentliste und einer schließenden Klammer.</span><span class="sxs-lookup"><span data-stu-id="87dba-506">An index expression consists of, in order, an expression, an opening parenthesis, an index argument list, and a closing parenthesis.</span></span>

```antlr
IndexExpression
    : Expression OpenParenthesis ArgumentList? CloseParenthesis
    ;
```

<span data-ttu-id="87dba-507">Das Ziel des Index Ausdrucks muss entweder als Eigenschaften Gruppe oder als Wert klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-507">The target of the index expression must be classified as either a property group or a value.</span></span> <span data-ttu-id="87dba-508">Ein Index Ausdruck wird wie folgt verarbeitet:</span><span class="sxs-lookup"><span data-stu-id="87dba-508">An index expression is processed as follows:</span></span>

* <span data-ttu-id="87dba-509">Wenn der Ziel Ausdruck als Wert klassifiziert wird und der Typ kein Arraytyp ist, `Object` oder `System.Array`, muss der Typ über eine Default-Eigenschaft verfügen.</span><span class="sxs-lookup"><span data-stu-id="87dba-509">If the target expression is classified as a value and if its type is not an array type, `Object`, or `System.Array`, the type must have a default property.</span></span> <span data-ttu-id="87dba-510">Der Index wird für eine Eigenschaften Gruppe ausgeführt, die alle Standardeigenschaften des Typs darstellt.</span><span class="sxs-lookup"><span data-stu-id="87dba-510">The index is performed on a property group that represents all of the default properties of the type.</span></span> <span data-ttu-id="87dba-511">Obwohl es nicht zulässig ist, eine Parameter lose Default-Eigenschaft in Visual Basic zu deklarieren, können andere Sprachen eine solche Eigenschaft deklarieren.</span><span class="sxs-lookup"><span data-stu-id="87dba-511">Although it is not valid to declare a parameterless default property in Visual Basic, other languages may allow declaring such a property.</span></span> <span data-ttu-id="87dba-512">Folglich ist das Indizieren einer Eigenschaft ohne Argumente zulässig.</span><span class="sxs-lookup"><span data-stu-id="87dba-512">Consequently, indexing a property with no arguments is allowed.</span></span>

* <span data-ttu-id="87dba-513">Wenn der Ausdruck einen Wert eines Array Typs ergibt, muss die Anzahl der Argumente in der Argumentliste mit dem Rang des Arraytyps identisch sein und darf keine benannten Argumente enthalten.</span><span class="sxs-lookup"><span data-stu-id="87dba-513">If the expression results in a value of an array type, the number of arguments in the argument list must be the same as the rank of the array type and may not include named arguments.</span></span> <span data-ttu-id="87dba-514">Wenn einer der Indizes zur Laufzeit ungültig ist, wird eine Ausnahme vom Typ "`System.IndexOutOfRangeException`" ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="87dba-514">If any of the indexes are invalid at run time, a `System.IndexOutOfRangeException` exception is thrown.</span></span> <span data-ttu-id="87dba-515">Jeder Ausdruck muss implizit in den Typ "`Integer`" konvertiert werden können.</span><span class="sxs-lookup"><span data-stu-id="87dba-515">Each expression must be implicitly convertible to type `Integer`.</span></span> <span data-ttu-id="87dba-516">Das Ergebnis des Index Ausdrucks ist die Variable am angegebenen Index und wird als Variable klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="87dba-516">The result of the index expression is the variable at the specified index and is classified as a variable.</span></span>

* <span data-ttu-id="87dba-517">Wenn der Ausdruck als Eigenschaften Gruppe klassifiziert wird, wird die Überladungs Auflösung verwendet, um zu bestimmen, ob eine der Eigenschaften auf die Index Argumentliste anwendbar ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-517">If the expression is classified as a property group, overload resolution is used to determine whether one of the properties is applicable to the index argument list.</span></span> <span data-ttu-id="87dba-518">Wenn die Eigenschaften Gruppe nur eine Eigenschaft enthält, die über einen `Get`-Accessor verfügt und dieser Accessor keine Argumente annimmt, wird die Eigenschaften Gruppe als Index Ausdruck mit einer leeren Argumentliste interpretiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-518">If the property group only contains one property that has a `Get` accessor and if that accessor takes no arguments, then the property group is interpreted as an index expression with an empty argument list.</span></span> <span data-ttu-id="87dba-519">Das Ergebnis wird als Ziel des aktuellen Index Ausdrucks verwendet.</span><span class="sxs-lookup"><span data-stu-id="87dba-519">The result is used as the target of the current index expression.</span></span> <span data-ttu-id="87dba-520">Wenn keine Eigenschaften anwendbar sind, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-520">If no properties are applicable, then a compile-time error occurs.</span></span> <span data-ttu-id="87dba-521">Andernfalls führt der Ausdruck zu einem Eigenschaften Zugriff mit dem zugeordneten Ziel Ausdruck (sofern vorhanden) der Eigenschaften Gruppe.</span><span class="sxs-lookup"><span data-stu-id="87dba-521">Otherwise, the expression results in a property access with the associated target expression (if any) of the property group.</span></span>

* <span data-ttu-id="87dba-522">Wenn der Ausdruck als spät gebundene Eigenschaften Gruppe oder als Wert klassifiziert wird, dessen Typ `Object` oder `System.Array` ist, wird die Verarbeitung des Index Ausdrucks bis zur Laufzeit verzögert, und die Indizierung ist spät gebunden.</span><span class="sxs-lookup"><span data-stu-id="87dba-522">If the expression is classified as a late-bound property group or as a value whose type is `Object` or `System.Array`, the processing of the index expression is deferred until run time and the indexing is late-bound.</span></span> <span data-ttu-id="87dba-523">Der Ausdruck führt dazu, dass ein Eigenschaften Zugriff mit später Bindung als `Object` typisiert ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-523">The expression results in a late-bound property access typed as `Object`.</span></span> <span data-ttu-id="87dba-524">Der zugeordnete Ziel Ausdruck ist entweder der Ziel Ausdruck, wenn es sich um einen Wert handelt, oder der zugehörige Ziel Ausdruck der Eigenschaften Gruppe.</span><span class="sxs-lookup"><span data-stu-id="87dba-524">The associated target expression is either the target expression, if it is a value, or the associated target expression of the property group.</span></span> <span data-ttu-id="87dba-525">Zur Laufzeit wird der Ausdruck wie folgt verarbeitet:</span><span class="sxs-lookup"><span data-stu-id="87dba-525">At run time the expression is processed as follows:</span></span>

* <span data-ttu-id="87dba-526">Wenn der Ausdruck als spät gebundene Eigenschaften Gruppe klassifiziert ist, kann der Ausdruck zu einer Methoden Gruppe, einer Eigenschaften Gruppe oder einem Wert führen (wenn der Member eine Instanz oder eine freigegebene Variable ist).</span><span class="sxs-lookup"><span data-stu-id="87dba-526">If the expression is classified as a late-bound property group, the expression may result in a method group, a property group, or a value (if the member is an instance or shared variable).</span></span> <span data-ttu-id="87dba-527">Wenn das Ergebnis eine Methoden Gruppe oder eine Eigenschaften Gruppe ist, wird die Überladungs Auflösung auf die Gruppe angewendet, um die richtige Methode für die Argumentliste zu ermitteln.</span><span class="sxs-lookup"><span data-stu-id="87dba-527">If the result is a method group or property group, overload resolution is applied to the group to determine the correct method for the argument list.</span></span> <span data-ttu-id="87dba-528">Wenn die Überladungs Auflösung fehlschlägt, wird eine Ausnahme vom Typ `System.Reflection.AmbiguousMatchException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="87dba-528">If overload resolution fails, a `System.Reflection.AmbiguousMatchException` exception is thrown.</span></span> <span data-ttu-id="87dba-529">Anschließend wird das Ergebnis entweder als Eigenschaften Zugriff oder als Aufruf verarbeitet, und das Ergebnis wird zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="87dba-529">Then the result is processed either as a property access or as an invocation and the result is returned.</span></span> <span data-ttu-id="87dba-530">Wenn der Aufruf einer Unterroutine ist, ist das Ergebnis `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="87dba-530">If the invocation is of a subroutine, the result is `Nothing`.</span></span>

* <span data-ttu-id="87dba-531">Wenn der Lauf Zeittyp des Ziel Ausdrucks ein Arraytyp oder `System.Array` ist, ist das Ergebnis des Index Ausdrucks der Wert der Variablen am angegebenen Index.</span><span class="sxs-lookup"><span data-stu-id="87dba-531">If the run-time type of the target expression is an array type or `System.Array`, the result of the index expression is the value of the variable at the specified index.</span></span>

* <span data-ttu-id="87dba-532">Andernfalls muss der Lauf Zeittyp des Ausdrucks über eine Default-Eigenschaft verfügen, und der Index wird für die Eigenschaften Gruppe ausgeführt, die alle Standardeigenschaften für den Typ darstellt.</span><span class="sxs-lookup"><span data-stu-id="87dba-532">Otherwise, the run-time type of the expression must have a default property and the index is performed on the property group that represents all of the default properties on the type.</span></span> <span data-ttu-id="87dba-533">Wenn der Typ keine Standard Eigenschaft aufweist, wird eine Ausnahme vom Typ "`System.MissingMemberException`" ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="87dba-533">If the type has no default property, then a `System.MissingMemberException` exception is thrown.</span></span>


## <a name="new-expressions"></a><span data-ttu-id="87dba-534">Neue Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-534">New Expressions</span></span>

<span data-ttu-id="87dba-535">Der `New`-Operator wird verwendet, um neue Instanzen von Typen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="87dba-535">The `New` operator is used to create new instances of types.</span></span> <span data-ttu-id="87dba-536">Es gibt vier Formen von `New`-ausdrücken:</span><span class="sxs-lookup"><span data-stu-id="87dba-536">There are four forms of `New` expressions:</span></span>

* <span data-ttu-id="87dba-537">Objekterstellungs-Ausdrücke werden verwendet, um neue Instanzen von Klassentypen und Werttypen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="87dba-537">Object-creation expressions are used to create new instances of class types and value types.</span></span>

* <span data-ttu-id="87dba-538">Array Erstellungs Ausdrücke werden verwendet, um neue Instanzen von Array Typen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="87dba-538">Array-creation expressions are used to create new instances of array types.</span></span>

* <span data-ttu-id="87dba-539">Delegaterstellungs-Ausdrücke (die keine unterschiedliche Syntax aus Objekt Erstellungs Ausdrücken aufweisen) werden verwendet, um neue Instanzen von Delegattypen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="87dba-539">Delegate-creation expressions (which do not have a distinct syntax from object-creation expressions) are used to create new instances of delegate types.</span></span>

* <span data-ttu-id="87dba-540">Anonyme Objekterstellungs Ausdrücke werden verwendet, um neue Instanzen anonymer Klassentypen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="87dba-540">Anonymous object-creation expressions are used to create new instances of anonymous class types.</span></span>

```antlr
NewExpression
    : ObjectCreationExpression
    | ArrayExpression
    | AnonymousObjectCreationExpression
    ;
```

<span data-ttu-id="87dba-541">Ein `New`-Ausdruck wird als Wert klassifiziert, und das Ergebnis ist die neue Instanz des-Typs.</span><span class="sxs-lookup"><span data-stu-id="87dba-541">A `New` expression is classified as a value and the result is the new instance of the type.</span></span>


### <a name="object-creation-expressions"></a><span data-ttu-id="87dba-542">Objekterstellungs-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-542">Object-Creation Expressions</span></span>

<span data-ttu-id="87dba-543">Ein Objekt Erstellungs Ausdruck wird verwendet, um eine neue Instanz eines Klassen Typs oder eines Struktur Typs zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="87dba-543">An object-creation expression is used to create a new instance of a class type or a structure type.</span></span>

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

<span data-ttu-id="87dba-544">Der Typ eines Objekterstellungs-Ausdrucks muss ein Klassentyp, ein Strukturtyp oder ein Typparameter mit einer `New`-Einschränkung sein und darf keine `MustInherit`-Klasse sein.</span><span class="sxs-lookup"><span data-stu-id="87dba-544">The type of an object creation expression must be a class type, a structure type, or a type parameter with a `New` constraint and cannot be a `MustInherit` class.</span></span> <span data-ttu-id="87dba-545">Wenn ein Objekt Erstellungs Ausdruck in der Form `New T(A)` ist, wobei `T` ein Klassentyp oder Strukturtyp ist und `A` eine optionale Argumentliste ist, bestimmt die Überladungs Auflösung den korrekten Konstruktor von `T` zum Aufrufen von.</span><span class="sxs-lookup"><span data-stu-id="87dba-545">Given an object creation expression of the form `New T(A)`, where `T` is a class type or structure type and `A` is an optional argument list, overload resolution determines the correct constructor of `T` to call.</span></span> <span data-ttu-id="87dba-546">Ein Typparameter mit einer `New`-Einschränkung wird als einzelner Parameter loser Konstruktor betrachtet.</span><span class="sxs-lookup"><span data-stu-id="87dba-546">A type parameter with a `New` constraint is considered to have a single, parameterless constructor.</span></span> <span data-ttu-id="87dba-547">Wenn kein Konstruktor aufgerufen werden kann, tritt ein Kompilierzeitfehler auf. Andernfalls führt der Ausdruck zur Erstellung einer neuen Instanz von `T` mithilfe des ausgewählten Konstruktors.</span><span class="sxs-lookup"><span data-stu-id="87dba-547">If no constructor is callable, a compile-time error occurs; otherwise the expression results in the creation of a new instance of `T` using the chosen constructor.</span></span> <span data-ttu-id="87dba-548">Wenn keine Argumente vorhanden sind, können die Klammern ausgelassen werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-548">If there are no arguments, the parentheses may be omitted.</span></span>

<span data-ttu-id="87dba-549">Wo eine Instanz zugewiesen wird, hängt davon ab, ob die Instanz ein Klassentyp oder ein Werttyp ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-549">Where an instance is allocated depends on whether the instance is a class type or a value type.</span></span> <span data-ttu-id="87dba-550">`New` Instanzen von Klassentypen werden auf dem System Heap erstellt, während neue Instanzen von Werttypen direkt auf dem Stapel erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-550">`New` instances of class types are created on the system heap, while new instances of value types are created directly on the stack.</span></span>

<span data-ttu-id="87dba-551">Ein Objekt Erstellungs Ausdruck kann optional eine Liste von Elementinitialisierern nach den Konstruktorargumenten angeben.</span><span class="sxs-lookup"><span data-stu-id="87dba-551">An object-creation expression can optionally specify a list of member initializers after the constructor arguments.</span></span> <span data-ttu-id="87dba-552">Diesen Member-Initialisierern wird das Schlüsselwort `With` vorangestellt, und die Initialisiererliste wird so interpretiert, als ob Sie im Kontext einer `With`-Anweisung wäre.</span><span class="sxs-lookup"><span data-stu-id="87dba-552">These member initializers are prefixed with the keyword `With`, and the initializer list is interpreted as if it was in the context of a `With` statement.</span></span> <span data-ttu-id="87dba-553">Beispielsweise mit der-Klasse:</span><span class="sxs-lookup"><span data-stu-id="87dba-553">For example, given the class:</span></span>

```vb
Class Customer
    Dim Name As String
    Dim Address As String
End Class
```

<span data-ttu-id="87dba-554">Der Code:</span><span class="sxs-lookup"><span data-stu-id="87dba-554">The code:</span></span>

```vb
Module Test
    Sub Main()
        Dim x As New Customer() With { .Name = "Bob Smith", _
            .Address = "123 Main St." }
    End Sub
End Module
```

<span data-ttu-id="87dba-555">ist ungefähr Äquivalent zu:</span><span class="sxs-lookup"><span data-stu-id="87dba-555">Is roughly equivalent to:</span></span>

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

<span data-ttu-id="87dba-556">Jeder Initialisierer muss einen Namen angeben, der zugewiesen werden soll, und der Name muss eine nicht-`ReadOnly`-Instanzvariable oder-Eigenschaft des erstellten Typs sein. der Member-Zugriff wird nicht spät gebunden, wenn der erstellte Typ `Object` ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-556">Each initializer must specify a name to assign, and the name must be a non-`ReadOnly` instance variable or property of the type being constructed; the member access will not be late bound if the type being constructed is `Object`.</span></span> <span data-ttu-id="87dba-557">Initialisierer dürfen das `Key`-Schlüsselwort nicht verwenden.</span><span class="sxs-lookup"><span data-stu-id="87dba-557">Initializers may not use the `Key` keyword.</span></span> <span data-ttu-id="87dba-558">Jedes Element in einem Typ kann nur einmal initialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-558">Each member in a type can only be initialized once.</span></span> <span data-ttu-id="87dba-559">Die initialisiererausdrücke können jedoch aufeinander verweisen.</span><span class="sxs-lookup"><span data-stu-id="87dba-559">The initializer expressions, however, may refer to each other.</span></span> <span data-ttu-id="87dba-560">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-560">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim x As New Customer() With { .Name = "Bob Smith", _
            .Address = .Name & " St." }
    End Sub
End Module
```

<span data-ttu-id="87dba-561">Die Initialisierer werden von links nach rechts zugewiesen. Wenn also ein Initialisierer auf einen Member verweist, der noch nicht initialisiert wurde, wird der Wert der Instanzvariablen nach dem Ausführen des Konstruktors angezeigt:</span><span class="sxs-lookup"><span data-stu-id="87dba-561">The initializers are assigned left-to-right, so if an initializer refers to a member that has not been initialized yet, it will see whatever value the instance variable after the constructor ran:</span></span>

```vb
Module Test
    Sub Main()
        ' The value of Address will be " St." since Name has not been
        ' assigned yet.
        Dim x As New Customer() With { .Address = .Name & " St." }
    End Sub
End Module
```

<span data-ttu-id="87dba-562">Initialisierer können eingebettet werden:</span><span class="sxs-lookup"><span data-stu-id="87dba-562">Initializers can be nested:</span></span>

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

<span data-ttu-id="87dba-563">Wenn der erstellte Typ ein Sammlungstyp ist und über eine Instanzmethode mit dem Namen `Add` (einschließlich Erweiterungs Methoden und freigegebenen Methoden) verfügt, kann der Objekt Erstellungs Ausdruck einen sammlungsinitialisierer angeben, dem das Schlüsselwort `From` vorangestellt ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-563">If the type being created is a collection type and has an instance method named `Add` (including extension methods and shared methods), then the object-creation expression can specify a collection initializer prefixed by the keyword `From`.</span></span> <span data-ttu-id="87dba-564">Ein Objekt Erstellungs Ausdruck kann nicht gleichzeitig einen Elementinitialisierer und einen Auflistungsinitialisierer angeben.</span><span class="sxs-lookup"><span data-stu-id="87dba-564">An object-creation expression cannot specify both a member initializer and a collection initializer.</span></span> <span data-ttu-id="87dba-565">Jedes Element im Auflistungsinitialisierer wird als Argument an einen Aufruf der `Add`-Funktion übermittelt.</span><span class="sxs-lookup"><span data-stu-id="87dba-565">Each element in the collection initializer is passed as an argument to an invocation of the `Add` function.</span></span> <span data-ttu-id="87dba-566">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-566">For example:</span></span>

```vb
Dim list = New List(Of Integer)() From { 1, 2, 3, 4 }
```

<span data-ttu-id="87dba-567">identisch mit folgendem Ausdruck:</span><span class="sxs-lookup"><span data-stu-id="87dba-567">is equivalent to:</span></span>

```vb
Dim list = New List(Of Integer)()
list.Add(1)
list.Add(2)
list.Add(3)
```

<span data-ttu-id="87dba-568">Wenn ein Element ein Auflistungsinitialisierer selbst ist, wird jedes Element des untergeordneten Auflistungsinitialisierers als einzelnes Argument an die Funktion "`Add`" übermittelt.</span><span class="sxs-lookup"><span data-stu-id="87dba-568">If an element is a collection initializer itself, each element of the sub-collection initializer will be passed as an individual argument to the `Add` function.</span></span> <span data-ttu-id="87dba-569">Beispielsweise Folgendes:</span><span class="sxs-lookup"><span data-stu-id="87dba-569">For example, the following:</span></span>

```vb
Dim dict = Dictionary(Of Integer, String) From { { 1, "One" },{ 2, "Two" } }
```

<span data-ttu-id="87dba-570">identisch mit folgendem Ausdruck:</span><span class="sxs-lookup"><span data-stu-id="87dba-570">is equivalent to:</span></span>

```vb
Dim dict = New Dictionary(Of Integer, String)
dict.Add(1, "One")
dict.Add(2, "Two")
```

<span data-ttu-id="87dba-571">Diese Erweiterung wird immer durchgeführt und wird nur einmal auf eine Ebene tief geführt; Danach gelten subinitialisierer als Array Literale.</span><span class="sxs-lookup"><span data-stu-id="87dba-571">This expansion is always done and is only ever done one level deep; after that, sub-initializers are considered array literals.</span></span> <span data-ttu-id="87dba-572">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-572">For example:</span></span>

```vb
' Error: List(Of T) does not have an Add method that takes two parameters.
Dim list = New List(Of Integer())() From { { 1, 2 }, { 3, 4 } }

' OK, this initializes the dictionary with (Integer, Integer()) pairs.
Dim dict = New Dictionary(Of Integer, Integer())() From _
        { {  1, { 2, 3 } }, { 3, { 4, 5 } } }
```


### <a name="array-expressions"></a><span data-ttu-id="87dba-573">Array Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-573">Array Expressions</span></span>

<span data-ttu-id="87dba-574">Ein Array Ausdruck wird verwendet, um eine neue Instanz eines Arraytyps zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="87dba-574">An array expression is used to create a new instance of an array type.</span></span> <span data-ttu-id="87dba-575">Es gibt zwei Typen von Array Ausdrücken: Array Erstellungs Ausdrücke und Array Literale.</span><span class="sxs-lookup"><span data-stu-id="87dba-575">There are two types of array expressions: array creation expressions, and array literals.</span></span>

#### <a name="array-creation-expressions"></a><span data-ttu-id="87dba-576">Ausdrücke zum Erstellen von Arrays</span><span class="sxs-lookup"><span data-stu-id="87dba-576">Array creation expressions</span></span>

<span data-ttu-id="87dba-577">Wenn ein Initialisierungs Modifizierer für die Array Größe angegeben wird, wird der resultierende Arraytyp abgeleitet, indem jedes einzelne Argument aus der Argumentliste der Array Größen Initialisierung gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-577">If an array size initialization modifier is supplied, the resulting array type is derived by deleting each of the individual arguments from the array size initialization argument list.</span></span> <span data-ttu-id="87dba-578">Der Wert jedes Arguments bestimmt die obere Grenze der entsprechenden Dimension in der neu zugeordneten Array Instanz.</span><span class="sxs-lookup"><span data-stu-id="87dba-578">The value of each argument determines the upper bound of the corresponding dimension in the newly allocated array instance.</span></span> <span data-ttu-id="87dba-579">Wenn der Ausdruck über einen nicht leeren Auflistungsinitialisierer verfügt, muss jedes Argument in der Argumentliste eine Konstante sein, und die von der Ausdrucks Liste angegebenen Rang-und Dimensions Längen müssen mit denen des Auflistungsinitialisierers identisch sein.</span><span class="sxs-lookup"><span data-stu-id="87dba-579">If the expression has a non-empty collection initializer, each argument in the argument list must be a constant, and the rank and dimension lengths specified by the expression list must match those of the collection initializer.</span></span>

```vb
Dim a() As Integer = New Integer(2) {}
Dim b() As Integer = New Integer(2) { 1, 2, 3 }
Dim c(,) As Integer = New Integer(1, 2) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error, length/initializer mismatch.
Dim d() As Integer = New Integer(2) { 0, 1, 2, 3 }
```

<span data-ttu-id="87dba-580">Wenn ein Initialisierungs Modifizierer für die Array Größe nicht angegeben wird, muss der Typname ein Arraytyp sein, und der Auflistungsinitialisierer muss leer sein oder über die gleiche Anzahl von Schachtelungs Ebenen verfügen wie der Rang des angegebenen Array Typs.</span><span class="sxs-lookup"><span data-stu-id="87dba-580">If an array size initialization modifier is not supplied, then the type name must be an array type and the collection initializer must be empty or have the same number of levels of nesting as the rank of the specified array type.</span></span> <span data-ttu-id="87dba-581">Alle Elemente in der innersten Schachtelungs Ebene müssen implizit in den Elementtyp des Arrays konvertiert werden und müssen als Wert klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-581">All of the elements in the innermost nesting level must be implicitly convertible to the element type of the array and must be classified as a value.</span></span> <span data-ttu-id="87dba-582">Die Anzahl der Elemente in jedem Initialisierer für die Initialisierung einer Initialisierung muss immer mit der Größe der anderen Auflistungen auf derselben Ebene konsistent sein.</span><span class="sxs-lookup"><span data-stu-id="87dba-582">The number of elements in each nested collection initializer must always be consistent with the size of the other collections at the same level.</span></span> <span data-ttu-id="87dba-583">Die einzelnen Dimensions Längen werden von der Anzahl von Elementen in jeder der entsprechenden Schachtelungs Ebenen des Auflistungsinitialisierers abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="87dba-583">The individual dimension lengths are inferred from the number of elements in each of the corresponding nesting levels of the collection initializer.</span></span> <span data-ttu-id="87dba-584">Wenn der Auflistungsinitialisierer leer ist, ist die Länge der einzelnen Dimensionen gleich 0 (null).</span><span class="sxs-lookup"><span data-stu-id="87dba-584">If the collection initializer is empty, the length of each dimension is zero.</span></span>

```vb
Dim e() As Integer = New Integer() { 1, 2, 3 }
Dim f(,) As Integer = New Integer(,) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error: Inconsistent numbers of elements!
Dim g(,) As Integer = New Integer(,) { { 1, 2 }, { 4, 5, 6 } }

' Error: Inconsistent levels of nesting!
Dim h(,) As Integer = New Integer(,) { 1, 2, { 3, 4 } }
```

<span data-ttu-id="87dba-585">Die äußerste Schachtelungs Ebene eines Auflistungsinitialisierers entspricht der äußersten linken Dimension eines Arrays, und die innerste Schachtelungs Ebene entspricht der äußersten rechten Dimension.</span><span class="sxs-lookup"><span data-stu-id="87dba-585">The outermost nesting level of a collection initializer corresponds to the leftmost dimension of an array, and the innermost nesting level corresponds to the rightmost dimension.</span></span> <span data-ttu-id="87dba-586">Das Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-586">The example:</span></span>

```vb
Dim array As Integer(,) = _
    { { 0, 1 }, { 2, 3 }, { 4, 5 }, { 6, 7 }, { 8, 9 } }
```

<span data-ttu-id="87dba-587">Entspricht Folgendem:</span><span class="sxs-lookup"><span data-stu-id="87dba-587">Is equivalent to the following:</span></span>

```vb
Dim array(4, 1) As Integer

array(0, 0) = 0: array(0, 1) = 1
array(1, 0) = 2: array(1, 1) = 3
array(2, 0) = 4: array(2, 1) = 5
array(3, 0) = 6: array(3, 1) = 7
array(4, 0) = 8: array(4, 1) = 9
```

<span data-ttu-id="87dba-588">Wenn der Auflistungsinitialisierer leer ist (d. h. einen, der geschweifte Klammern, aber keine Initialisiererliste enthält) und die Begrenzungen der Dimensionen des zu initialisierenden Arrays bekannt sind, stellt der leere sammlungsinitialisierer eine Array Instanz der angegebenen Größe dar. , wo alle Elemente mit dem Standardwert des Elementtyps initialisiert wurden.</span><span class="sxs-lookup"><span data-stu-id="87dba-588">If the collection initializer is empty (that is, one that contains curly braces but no initializer list) and the bounds of the dimensions of the array being initialized are known, the empty collection initializer represents an array instance of the specified size where all the elements have been initialized to the element type's default value.</span></span> <span data-ttu-id="87dba-589">Wenn die Begrenzungen der Dimensionen des Arrays, das initialisiert wird, nicht bekannt sind, stellt der leere Auflistungsinitialisierer eine Array Instanz dar, in der alle Dimensionen die Größe 0 (null) aufweisen.</span><span class="sxs-lookup"><span data-stu-id="87dba-589">If the bounds of the dimensions of the array being initialized are not known, the empty collection initializer represents an array instance in which all dimensions are size zero.</span></span>

<span data-ttu-id="87dba-590">Der Rang und die Länge einer Array Instanz der einzelnen Dimensionen sind für die gesamte Lebensdauer der Instanz konstant.</span><span class="sxs-lookup"><span data-stu-id="87dba-590">An array instance's rank and length of each dimension are constant for the entire lifetime of the instance.</span></span> <span data-ttu-id="87dba-591">Anders ausgedrückt: Es ist nicht möglich, den Rang einer vorhandenen Array Instanz zu ändern, und es ist nicht möglich, die Größe der Dimensionen zu ändern.</span><span class="sxs-lookup"><span data-stu-id="87dba-591">In other words, it is not possible to change the rank of an existing array instance, nor is it possible to resize its dimensions.</span></span>

#### <a name="array-literals"></a><span data-ttu-id="87dba-592">Arrayliterale</span><span class="sxs-lookup"><span data-stu-id="87dba-592">Array Literals</span></span>

<span data-ttu-id="87dba-593">Ein Arrayliterale bezeichnet ein Array, dessen Elementtyp, Rang und Begrenzungen von einer Kombination aus dem Ausdrucks Kontext und einem Auflistungsinitialisierer abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-593">An array literal denotes an array whose element type, rank, and bounds are inferred from a combination of the expression context and a collection initializer.</span></span> <span data-ttu-id="87dba-594">Dies wird im Abschnitt [Ausdrucks Neuklassifizierung](expressions.md#expression-reclassification)erläutert.</span><span class="sxs-lookup"><span data-stu-id="87dba-594">This is explained in Section [Expression Reclassification](expressions.md#expression-reclassification).</span></span>

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

<span data-ttu-id="87dba-595">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-595">For example:</span></span>

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

<span data-ttu-id="87dba-596">Das Format und die Anforderungen für den Auflistungsinitialisierer in einem arrayliteralformat sind identisch mit denen für den Auflistungsinitialisierer in einem Array Erstellungs Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="87dba-596">The format and requirements for the collection initializer in an array literal is exactly the same as that for the collection initializer in an array creation expression.</span></span>

<span data-ttu-id="87dba-597">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-597">__Note.__</span></span> <span data-ttu-id="87dba-598">Ein arrayliteralarray erstellt das Array nicht in und von sich selbst. Stattdessen ist es die Neuklassifizierung des Ausdrucks in einen Wert, der bewirkt, dass das Array erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-598">An array literal does not create the array in and of itself; instead, it is the reclassification of the expression into a value that causes the array to be created.</span></span> <span data-ttu-id="87dba-599">Beispielsweise ist die Konvertierung `CType(new Integer() {1,2,3}, Short())` nicht möglich, da keine Konvertierung von `Integer()` in `Short()`; der Ausdruck `CType({1,2,3},Short())` ist jedoch möglich, da er zuerst das arrayliteralzeichen in den Array Erstellungs Ausdruck `New Short() {1,2,3}` umklassifiziert.</span><span class="sxs-lookup"><span data-stu-id="87dba-599">For instance, the conversion `CType(new Integer() {1,2,3}, Short())` is not possible because there is no conversion from `Integer()` to `Short()`; but the expression `CType({1,2,3},Short())` is possible because it first reclassifies the array literal into the array creation expression `New Short() {1,2,3}`.</span></span>


### <a name="delegate-creation-expressions"></a><span data-ttu-id="87dba-600">Delegat-Erstellungs Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-600">Delegate-Creation Expressions</span></span>

<span data-ttu-id="87dba-601">Ein delegaterstellungs-Ausdruck wird verwendet, um eine neue Instanz eines Delegattyps zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="87dba-601">A delegate-creation expression is used to create a new instance of a delegate type.</span></span> <span data-ttu-id="87dba-602">Das Argument eines Delegaten zum Erstellen von Delegaten muss ein Ausdruck sein, der als Methoden Zeiger oder als Lambda-Methode klassifiziert ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-602">The argument of a delegate-creation expression must be an expression classified as a method pointer or a lambda method.</span></span>

<span data-ttu-id="87dba-603">Wenn das Argument ein Methoden Zeiger ist, muss eine der Methoden, auf die vom Methoden Zeiger verwiesen wird, auf die Signatur des Delegattyps anwendbar sein.</span><span class="sxs-lookup"><span data-stu-id="87dba-603">If the argument is a method pointer, one of the methods referenced by the method pointer must be applicable to the signature of the delegate type.</span></span> <span data-ttu-id="87dba-604">Eine Methode `M` gilt für einen Delegattyp `D`, wenn Folgendes zutrifft:</span><span class="sxs-lookup"><span data-stu-id="87dba-604">A method `M` is applicable to a delegate type `D` if:</span></span>

* <span data-ttu-id="87dba-605">`M` ist nicht `Partial` oder hat einen Text.</span><span class="sxs-lookup"><span data-stu-id="87dba-605">`M` is not `Partial` or has a body.</span></span>

* <span data-ttu-id="87dba-606">Sowohl `M` als auch `D` sind Funktionen, oder `D` ist eine Unterroutine.</span><span class="sxs-lookup"><span data-stu-id="87dba-606">Both `M` and `D` are functions, or `D` is a subroutine.</span></span>

* <span data-ttu-id="87dba-607">`M` und `D` verfügen über die gleiche Anzahl von Parametern.</span><span class="sxs-lookup"><span data-stu-id="87dba-607">`M` and `D` have the same number of parameters.</span></span>

* <span data-ttu-id="87dba-608">Die Parametertypen von `M` verfügen jeweils über eine Konvertierung vom Typ des entsprechenden Parameter Typs von `D`, und ihre Modifizierer (d. h. `ByRef`, `ByVal`) stimmen überein.</span><span class="sxs-lookup"><span data-stu-id="87dba-608">The parameter types of `M` each have a conversion from the type of the corresponding parameter type of `D`, and their modifiers (i.e. `ByRef`, `ByVal`) match.</span></span>

* <span data-ttu-id="87dba-609">Der Rückgabetyp von `M` ist, falls vorhanden, eine Konvertierung in den Rückgabetyp von `D`.</span><span class="sxs-lookup"><span data-stu-id="87dba-609">The return type of `M`, if any, has a conversion to the return type of `D`.</span></span>

<span data-ttu-id="87dba-610">Wenn der Methoden Zeiger auf einen spät gebundenen Zugriff verweist, wird davon ausgegangen, dass der spät gebundene Zugriff an eine Funktion erfolgt, die über die gleiche Anzahl von Parametern wie der Delegattyp verfügt.</span><span class="sxs-lookup"><span data-stu-id="87dba-610">If the method pointer references a late-bound access, then the late-bound access is assumed to be to a function that has the same number of parameters as the delegate type.</span></span>

<span data-ttu-id="87dba-611">Wenn eine strikte Semantik nicht verwendet wird und nur eine Methode durch den Methoden Zeiger referenziert wird, aber aufgrund der Tatsache, dass Sie über keine Parameter verfügt und der Delegattyp dies tut, gilt die Methode als anwendbar und die Parameter oder der Rückgabewert a wird erneut ignoriert.</span><span class="sxs-lookup"><span data-stu-id="87dba-611">If strict semantics are not being used and there is only one method referenced by the method pointer, but it is not applicable due to the fact that it has no parameters and the delegate type does, then the method is considered applicable and the parameters or return value are simply ignored.</span></span> <span data-ttu-id="87dba-612">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-612">For example:</span></span>

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

<span data-ttu-id="87dba-613">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-613">__Note.__</span></span> <span data-ttu-id="87dba-614">Diese Lockerung ist nur zulässig, wenn eine strikte Semantik aufgrund von Erweiterungs Methoden nicht verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-614">This relaxation is only allowed when strict semantics are not being used because of extension methods.</span></span> <span data-ttu-id="87dba-615">Da Erweiterungs Methoden nur berücksichtigt werden, wenn eine reguläre Methode nicht anwendbar ist, kann eine Instanzmethode ohne Parameter eine Erweiterungsmethode mit Parametern zum Zweck der Delegaterstellung ausblenden.</span><span class="sxs-lookup"><span data-stu-id="87dba-615">Because extension methods are only considered if a regular method was not applicable, it is possible for an instance method with no parameters to hide an extension method with parameters for the purpose of delegate construction.</span></span>

<span data-ttu-id="87dba-616">Wenn mehr als eine Methode, auf die der Methoden Zeiger verweist, auf den Delegattyp anwendbar ist, wird die Überladungs Auflösung verwendet, um zwischen den Kandidaten Methoden zu wählen.</span><span class="sxs-lookup"><span data-stu-id="87dba-616">If more than one method referenced by the method pointer is applicable to the delegate type, then overload resolution is used to pick between the candidate methods.</span></span> <span data-ttu-id="87dba-617">Die Typen der Parameter für den Delegaten werden als Typargumente für den Zweck der Überladungs Auflösung verwendet.</span><span class="sxs-lookup"><span data-stu-id="87dba-617">The types of the parameters to the delegate are used as the types of the arguments for the purposes of overload resolution.</span></span> <span data-ttu-id="87dba-618">Wenn kein Methoden Kandidat am meisten anwendbar ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-618">If no one method candidate is most applicable, a compile-time error occurs.</span></span> <span data-ttu-id="87dba-619">Im folgenden Beispiel wird die lokale Variable mit einem Delegaten initialisiert, der auf die zweite `Square`-Methode verweist, da diese Methode besser auf die Signatur und den Rückgabetyp `DoubleFunc` anwendbar ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-619">In the following example, the local variable is initialized with a delegate that refers to the second `Square` method because that method is more applicable to the signature and return type of `DoubleFunc`.</span></span>

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

<span data-ttu-id="87dba-620">Wenn die zweite `Square`-Methode nicht vorhanden war, wurde die erste `Square`-Methode ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="87dba-620">Had the second `Square` method not been present, the first `Square` method would have been chosen.</span></span> <span data-ttu-id="87dba-621">Wenn eine strikte Semantik von der Kompilierungs Umgebung oder `Option Strict` angegeben wird, tritt ein Kompilierzeitfehler auf, wenn die spezifischere Methode, auf die der Methoden Zeiger verweist, schmaler ist als die Signatur des Delegaten.</span><span class="sxs-lookup"><span data-stu-id="87dba-621">If strict semantics are specified by the compilation environment or by `Option Strict`, then a compile-time error occurs if the most specific method referenced by the method pointer is narrower than the delegate signature.</span></span> <span data-ttu-id="87dba-622">Eine Methode `M` gilt als schmaler als ein Delegattyp `D`, wenn Folgendes gilt:</span><span class="sxs-lookup"><span data-stu-id="87dba-622">A method `M` is considered narrower than a delegate type `D` if:</span></span>

* <span data-ttu-id="87dba-623">Der Parametertyp `M` hat eine erweiternde Konvertierung in den entsprechenden Parametertyp `D`.</span><span class="sxs-lookup"><span data-stu-id="87dba-623">A parameter type of `M` has a widening conversion to the corresponding parameter type of `D`.</span></span>

* <span data-ttu-id="87dba-624">Der Rückgabetyp (sofern vorhanden) von `M` hat eine einschränkende Konvertierung in den Rückgabetyp `D`.</span><span class="sxs-lookup"><span data-stu-id="87dba-624">Or, the return type, if any, of `M` has a narrowing conversion to the return type of `D`.</span></span>

<span data-ttu-id="87dba-625">Wenn dem Methoden Zeiger Typargumente zugeordnet sind, werden nur Methoden mit der gleichen Anzahl von Typargumenten berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="87dba-625">If type arguments are associated with the method pointer, only methods with the same number of type arguments are considered.</span></span> <span data-ttu-id="87dba-626">Wenn dem Methoden Zeiger keine Typargumente zugeordnet sind, wird der Typrückschluss verwendet, wenn Signaturen mit einer generischen Methode abgeglichen werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-626">If no type arguments are associated with the method pointer, type inference is used when matching signatures against a generic method.</span></span> <span data-ttu-id="87dba-627">Anders als bei einem anderen normalen Typrückschluss wird der Rückgabetyp des Delegaten beim Ableiten von Typargumenten verwendet, aber Rückgabe Typen werden immer noch nicht berücksichtigt, wenn die geringste generische Überladung bestimmt wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-627">Unlike other normal type inference, the return type of the delegate is used when inferring type arguments, but return types are still not considered when determining the least generic overload.</span></span> <span data-ttu-id="87dba-628">Im folgenden Beispiel werden beide Methoden zum Bereitstellen eines Typarguments für einen delegaterstellungs-Ausdruck gezeigt:</span><span class="sxs-lookup"><span data-stu-id="87dba-628">The following example shows both ways of supplying a type argument to a delegate-creation expression:</span></span>

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

<span data-ttu-id="87dba-629">Im obigen Beispiel wurde ein nicht generischer Delegattyp mithilfe einer generischen Methode instanziiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-629">In the above example, a non-generic delegate type was instantiated using a generic method.</span></span> <span data-ttu-id="87dba-630">Es ist auch möglich, mithilfe einer generischen Methode eine Instanz eines konstruierten Delegattyps zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="87dba-630">It is also possible to create an instance of a constructed delegate type using a generic method.</span></span> <span data-ttu-id="87dba-631">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-631">For example:</span></span>

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

<span data-ttu-id="87dba-632">Wenn das Argument für den Ausdruck zur Delegaterstellung eine Lambda-Methode ist, muss die Lambda-Methode auf die Signatur des Delegattyps anwendbar sein.</span><span class="sxs-lookup"><span data-stu-id="87dba-632">If the argument to the delegate-creation expression is a lambda method, the lambda method must be applicable to the signature of the delegate type.</span></span> <span data-ttu-id="87dba-633">Eine Lambda-Methode `L` ist auf einen Delegattyp anwendbar `D`, wenn Folgendes zutrifft:</span><span class="sxs-lookup"><span data-stu-id="87dba-633">A lambda method `L` is applicable to a delegate type `D` if:</span></span>

* <span data-ttu-id="87dba-634">Wenn `L` über Parameter verfügt, verfügt `D` über die gleiche Anzahl von Parametern.</span><span class="sxs-lookup"><span data-stu-id="87dba-634">If `L` has parameters, `D` has the same number of parameters.</span></span> <span data-ttu-id="87dba-635">(Wenn `L` keine Parameter aufweist, werden die Parameter von `D` ignoriert.)</span><span class="sxs-lookup"><span data-stu-id="87dba-635">(If `L` has no parameters, the parameters of `D` are ignored.)</span></span>

* <span data-ttu-id="87dba-636">Die Parametertypen von `L` verfügen jeweils über eine Konvertierung in den Typ des entsprechenden Parameter Typs von `D`, und ihre Modifizierer (d. h. `ByRef`, `ByVal`) stimmen überein.</span><span class="sxs-lookup"><span data-stu-id="87dba-636">The parameter types of `L` each have a conversion to the type of the corresponding parameter type of `D`, and their modifiers (i.e. `ByRef`, `ByVal`) match.</span></span>

* <span data-ttu-id="87dba-637">Wenn `D` eine Funktion ist, wird der Rückgabetyp `L` in den Rückgabetyp `D` konvertiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-637">If `D` is a function, the return type of `L` has a conversion to the return type of `D`.</span></span> <span data-ttu-id="87dba-638">(Wenn `D` eine Unterroutine ist, wird der Rückgabewert von `L` ignoriert.)</span><span class="sxs-lookup"><span data-stu-id="87dba-638">(If `D` is a subroutine, the return value of `L` is ignored.)</span></span>

<span data-ttu-id="87dba-639">Wenn der Parametertyp eines Parameters von `L` weggelassen wird, wird der Typ des entsprechenden Parameters in `D` abgeleitet. Wenn der-Parameter von `L` Array-oder Werte zulässt-namensmodifizierer aufweist, wird ein Kompilierzeitfehler ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="87dba-639">If the parameter type of a parameter of `L` is omitted, then the type of the corresponding parameter in `D` is inferred; if the parameter of `L` has array or nullable name modifiers, a compile-time error results.</span></span> <span data-ttu-id="87dba-640">Wenn alle Parametertypen von `L` verfügbar sind, wird der Typ des Ausdrucks in der Lambda-Methode abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="87dba-640">Once all of the parameter types of `L` are available, then the type of the expression in the lambda method is inferred.</span></span> <span data-ttu-id="87dba-641">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-641">For example:</span></span>

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

<span data-ttu-id="87dba-642">In einigen Situationen, in denen die Delegatsignatur nicht genau mit der Lambda-Methode oder der Methoden Signatur übereinstimmt, unterstützt die .NET Framework die Delegaterstellung möglicherweise nicht System intern.</span><span class="sxs-lookup"><span data-stu-id="87dba-642">In some situations where delegate signature does not exactly match the lambda method or method signature, the .NET Framework may not support the delegate creation natively.</span></span> <span data-ttu-id="87dba-643">In dieser Situation wird ein Lambda-Methoden Ausdruck verwendet, um die beiden Methoden abzugleichen.</span><span class="sxs-lookup"><span data-stu-id="87dba-643">In that situation, a lambda method expression is used to match the two methods.</span></span> <span data-ttu-id="87dba-644">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-644">For example:</span></span>

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

<span data-ttu-id="87dba-645">Das Ergebnis eines Delegaten zum Erstellen von Delegaten ist eine Delegatinstanz, die auf die passende Methode mit dem zugeordneten Ziel Ausdruck (sofern vorhanden) aus dem Methoden Zeiger Ausdruck verweist.</span><span class="sxs-lookup"><span data-stu-id="87dba-645">The result of a delegate-creation expression is a delegate instance that refers to the matching method with the associated target expression (if any) from the method pointer expression.</span></span> <span data-ttu-id="87dba-646">Wenn der Ziel Ausdruck als Werttyp typisiert ist, wird der Werttyp auf den System Heap kopiert, da ein Delegat nur auf eine Methode eines Objekts auf dem Heap verweisen kann.</span><span class="sxs-lookup"><span data-stu-id="87dba-646">If the target expression is typed as a value type, then the value type is copied onto the system heap because a delegate can only point to a method of an object on the heap.</span></span> <span data-ttu-id="87dba-647">Die Methode und das Objekt, auf die ein Delegat verweist, bleiben für die gesamte Lebensdauer des Delegaten konstant.</span><span class="sxs-lookup"><span data-stu-id="87dba-647">The method and object to which a delegate refers remain constant for the entire lifetime of the delegate.</span></span> <span data-ttu-id="87dba-648">Anders ausgedrückt: Es ist nicht möglich, das Ziel oder das Objekt eines Delegaten zu ändern, nachdem es erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="87dba-648">In other words, it is not possible to change the target or object of a delegate after it has been created.</span></span>

### <a name="anonymous-object-creation-expressions"></a><span data-ttu-id="87dba-649">Anonyme Objekt Erstellungs Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-649">Anonymous Object-Creation Expressions</span></span>

<span data-ttu-id="87dba-650">Ein Objekt Erstellungs Ausdruck mit Member-Initialisierern kann den Typnamen auch vollständig weglassen.</span><span class="sxs-lookup"><span data-stu-id="87dba-650">An object-creation expression with member initializers can also omit the type name entirely.</span></span>

```antlr
AnonymousObjectCreationExpression
    : 'New' ObjectMemberInitializer
    ;
```

<span data-ttu-id="87dba-651">In diesem Fall wird ein anonymer Typ basierend auf den Typen und Namen der Member erstellt, die als Teil des Ausdrucks initialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-651">In that case, an anonymous type is constructed based on the types and names of the members initialized as a part of the expression.</span></span> <span data-ttu-id="87dba-652">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-652">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim Customer = New With { .Name = "John Smith", .Age = 34 }

        Console.WriteLine(Customer.Name)
    End Sub
End Module
```

<span data-ttu-id="87dba-653">Der von einem anonymen Objekt Erstellungs Ausdruck erstellte Typ ist eine Klasse, die über keinen Namen verfügt, direkt von `Object` erbt und über eine Reihe von Eigenschaften mit demselben Namen wie die in der Liste der Element Initialisierer zugewiesenen Member verfügt.</span><span class="sxs-lookup"><span data-stu-id="87dba-653">The type created by an anonymous object-creation expression is a class that has no name, inherits directly from `Object`, and has a set of properties with the same name as the members assigned to in the member initializer list.</span></span> <span data-ttu-id="87dba-654">Der Typ der einzelnen Eigenschaften wird mithilfe der gleichen Regeln wie der lokale variablentyprückschluss abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="87dba-654">The type of each property is inferred using the same rules as local variable type inference.</span></span> <span data-ttu-id="87dba-655">Generierte anonyme Typen überschreiben auch `ToString` und geben eine Zeichen folgen Darstellung aller Member und ihrer Werte zurück.</span><span class="sxs-lookup"><span data-stu-id="87dba-655">Generated anonymous types also override `ToString`, returning a string representation of all members and their values.</span></span> <span data-ttu-id="87dba-656">(Das genaue Format dieser Zeichenfolge überschreitet den Gültigkeitsbereich dieser Spezifikation).</span><span class="sxs-lookup"><span data-stu-id="87dba-656">(The exact format of this string is beyond the scope of this specification).</span></span>

<span data-ttu-id="87dba-657">Standardmäßig sind die vom anonymen Typ generierten Eigenschaften mit Lese-/Schreibzugriff.</span><span class="sxs-lookup"><span data-stu-id="87dba-657">By default, the properties generated by the anonymous type are read-write.</span></span> <span data-ttu-id="87dba-658">Es ist möglich, eine Eigenschaft für anonyme Typen mit dem `Key`-Modifizierer als schreibgeschützt zu markieren.</span><span class="sxs-lookup"><span data-stu-id="87dba-658">It is possible to mark an anonymous type property as read-only by using the `Key` modifier.</span></span> <span data-ttu-id="87dba-659">Der `Key`-Modifizierer gibt an, dass das Feld verwendet werden kann, um den Wert, den der anonyme Typ darstellt, eindeutig zu identifizieren.</span><span class="sxs-lookup"><span data-stu-id="87dba-659">The `Key` modifier specifies that the field can be used to uniquely identify the value the anonymous type represents.</span></span> <span data-ttu-id="87dba-660">Zusätzlich dazu, dass die Eigenschaft schreibgeschützt ist, bewirkt dies auch, dass der anonyme Typ `Equals` und `GetHashCode` überschreibt und die-Schnittstelle `System.IEquatable(Of T)` (die den anonymen Typ für `T` füllt) implementiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-660">In addition to making the property read-only, it also causes the anonymous type to override `Equals` and  `GetHashCode` and to implement the interface `System.IEquatable(Of T)` (filling in the anonymous type for `T`).</span></span> <span data-ttu-id="87dba-661">Die Member werden wie folgt definiert:</span><span class="sxs-lookup"><span data-stu-id="87dba-661">The members are defined as follows:</span></span>

<span data-ttu-id="87dba-662">`Function Equals(obj As Object) As Boolean` und `Function Equals(val As T) As Boolean` werden implementiert, indem überprüft wird, ob die beiden Instanzen denselben Typ haben und dann jedes `Key`-Element mit `Object.Equals` vergleicht.</span><span class="sxs-lookup"><span data-stu-id="87dba-662">`Function Equals(obj As Object) As Boolean` and `Function Equals(val As T) As Boolean` are implemented by validating that the two instances are of the same type and then comparing each `Key` member using `Object.Equals`.</span></span> <span data-ttu-id="87dba-663">Wenn alle `Key`-Elemente gleich sind, gibt `Equals` `True` zurück, andernfalls gibt `Equals` `False` zurück.</span><span class="sxs-lookup"><span data-stu-id="87dba-663">If all `Key` members are equal, then `Equals` returns `True`, otherwise `Equals` returns `False`.</span></span>

<span data-ttu-id="87dba-664">`Function GetHashCode() As Integer` ist so implementiert, dass, wenn `Equals` für zwei Instanzen des anonymen Typs true ist, `GetHashCode` denselben Wert zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="87dba-664">`Function GetHashCode() As Integer` is implemented such that that if `Equals` is true for two instances of the anonymous type, then `GetHashCode` will return the same value.</span></span> <span data-ttu-id="87dba-665">Der Hashwert beginnt mit einem Ausgangswert und dann für jedes `Key`-Element, in der Reihenfolge multipliziert den Hash mit 31 und fügt den Hashwert des `Key`-Members (bereitgestellt von `GetHashCode`) hinzu, wenn der Member kein Verweistyp oder ein Werte zulässt-Werttyp mit dem Wert `Nothing` ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-665">The hash starts with a seed value and then, for each `Key` member, in order multiplies the hash by 31 and adds the `Key` member's hash value (provided by `GetHashCode`) if the member is not a reference type or nullable value type with the value of `Nothing`.</span></span>

<span data-ttu-id="87dba-666">Beispielsweise der in der-Anweisung erstellte-Typ:</span><span class="sxs-lookup"><span data-stu-id="87dba-666">For example, the type created in the statement:</span></span>

```vb
Dim zipState = New With { Key .ZipCode = 98112, .State = "WA" }
```

<span data-ttu-id="87dba-667">erstellt eine Klasse, die ungefähr so aussieht (obwohl die genaue Implementierung variieren kann):</span><span class="sxs-lookup"><span data-stu-id="87dba-667">creates a class that looks approximately like this (although exact implementation may vary):</span></span>

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

<span data-ttu-id="87dba-668">Um die Situation zu vereinfachen, in der ein anonymer Typ aus den Feldern eines anderen Typs erstellt wird, können Feldnamen in den folgenden Fällen direkt von Ausdrücken abgeleitet werden:</span><span class="sxs-lookup"><span data-stu-id="87dba-668">To simplify the situation where an anonymous type is created from the fields of another type, field names can be inferred directly from expressions in the following cases:</span></span>

* <span data-ttu-id="87dba-669">Ein einfacher namens Ausdruck `x` leitet den Namen `x` ab.</span><span class="sxs-lookup"><span data-stu-id="87dba-669">A simple name expression `x` infers the name `x`.</span></span>

* <span data-ttu-id="87dba-670">Ein Member-Zugriffs Ausdruck `x.y` leitet den Namen `y` ab.</span><span class="sxs-lookup"><span data-stu-id="87dba-670">A member access expression `x.y` infers the name `y`.</span></span>

* <span data-ttu-id="87dba-671">Ein Wörterbuch-Suche-Ausdruck `x!y` leitet den Namen `y` ab.</span><span class="sxs-lookup"><span data-stu-id="87dba-671">A dictionary lookup expression `x!y` infers the name `y`.</span></span>

* <span data-ttu-id="87dba-672">Ein Aufruf-oder Index Ausdruck ohne Argumente `x()` leitet den Namen `x` ab.</span><span class="sxs-lookup"><span data-stu-id="87dba-672">An invocation or index expression with no arguments `x()` infers the name `x`.</span></span>

* <span data-ttu-id="87dba-673">Ein XML-Member-Zugriffs Ausdruck `x.<y>`, `x...<y>` `x.@y` leitet den Namen `y` ab.</span><span class="sxs-lookup"><span data-stu-id="87dba-673">An XML member access expression `x.<y>`, `x...<y>`, `x.@y` infers the name `y`.</span></span>

* <span data-ttu-id="87dba-674">Ein XML-Member-Zugriffs Ausdruck, der das Ziel eines Element Zugriffs Ausdrucks ist `x.<y>.z` den Namen `z`.</span><span class="sxs-lookup"><span data-stu-id="87dba-674">An XML member access expression that is the target of a member access expression `x.<y>.z` infers the name `z`.</span></span>

* <span data-ttu-id="87dba-675">Ein XML-Member-Zugriffs Ausdruck, bei dem es sich um das Ziel eines aufzurufenden oder Index Ausdrucks ohne Argumente handelt `x.<y>.z()` den Namen `z`.</span><span class="sxs-lookup"><span data-stu-id="87dba-675">An XML member access expression that is the target of an invocation or index expression with no arguments `x.<y>.z()` infers the name `z`.</span></span>

* <span data-ttu-id="87dba-676">Ein XML-Member-Zugriffs Ausdruck, bei dem es sich um das Ziel eines Aufruf-oder Index Ausdrucks handelt, `x.<y>(0)` den Namen `y` leitet.</span><span class="sxs-lookup"><span data-stu-id="87dba-676">An XML member access expression that is the target of an invocation or index expression `x.<y>(0)` infers the name `y`.</span></span>

<span data-ttu-id="87dba-677">Der Initialisierer wird als Zuweisung des Ausdrucks zum abzurufenden Namen interpretiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-677">The initializer is interpreted as an assignment of the expression to the inferred name.</span></span> <span data-ttu-id="87dba-678">Die folgenden Initialisierer sind z. b. äquivalent:</span><span class="sxs-lookup"><span data-stu-id="87dba-678">For example, the following initializers are equivalent:</span></span>

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

<span data-ttu-id="87dba-679">Wenn ein Elementname abgeleitet wird, der zu einem Konflikt mit einem vorhandenen Member des Typs führt (z. b. `GetHashCode`), tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-679">If a member name is inferred that conflicts with an existing member of the type, such as `GetHashCode`, then a compile time error occurs.</span></span> <span data-ttu-id="87dba-680">Im Gegensatz zu regulären Member-Initialisierern können Member-Initialisierer von anonymen Objekten keine Zirkel Verweise aufweisen oder auf einen Member verweisen, bevor dieser initialisiert wurde.</span><span class="sxs-lookup"><span data-stu-id="87dba-680">Unlike regular member initializers, anonymous object-creation expressions do not allow member initializers to have circular references, or to refer to a member before it has been initialized.</span></span> <span data-ttu-id="87dba-681">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-681">For example:</span></span>

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

<span data-ttu-id="87dba-682">Wenn zwei Ausdrücke der anonymen Klassen Erstellung innerhalb derselben Methode auftreten und die gleiche resultierende Form ergeben, wenn die Eigenschaften Reihenfolge, die Eigenschaften Namen und die Eigenschafts Typen alle übereinstimmen, verweisen beide auf dieselbe anonyme Klasse.</span><span class="sxs-lookup"><span data-stu-id="87dba-682">If two anonymous class creation expressions occur within the same method and yield the same resulting shape -- if the property order, property names, and property types all match -- they will both refer to the same anonymous class.</span></span> <span data-ttu-id="87dba-683">Der Methoden Bereich einer Instanz oder einer freigegebenen Element Variablen mit einem Initialisierer ist der Konstruktor, in dem die Variable initialisiert wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-683">The method scope of an instance or shared member variable with an initializer is the constructor in which the variable is initialized.</span></span>

<span data-ttu-id="87dba-684">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-684">__Note.__</span></span> <span data-ttu-id="87dba-685">Es ist möglich, dass ein Compiler anonyme Typen weiter vereinheitlichen kann, z. b. auf Assemblyebene, aber dies kann zu diesem Zeitpunkt nicht darauf beruhen.</span><span class="sxs-lookup"><span data-stu-id="87dba-685">It is possible that a compiler may choose to unify anonymous types further, such as at the assembly level, but this cannot be relied upon at this time.</span></span>


## <a name="cast-expressions"></a><span data-ttu-id="87dba-686">Umwandlungs Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-686">Cast Expressions</span></span>

<span data-ttu-id="87dba-687">Ein Cast Ausdruck wandelt einen Ausdruck in einen angegebenen Typ um.</span><span class="sxs-lookup"><span data-stu-id="87dba-687">A cast expression coerces an expression to a given type.</span></span> <span data-ttu-id="87dba-688">Bestimmte Umwandlungs Schlüsselwörter leiten Ausdrücke in die primitiven Typen um.</span><span class="sxs-lookup"><span data-stu-id="87dba-688">Specific cast keywords coerce expressions into the primitive types.</span></span> <span data-ttu-id="87dba-689">Drei allgemeine Cast Schlüsselwörter ("`CType`", "`TryCast`" und "`DirectCast`" wandeln einen Ausdruck in einen Typ um.</span><span class="sxs-lookup"><span data-stu-id="87dba-689">Three general cast keywords, `CType`, `TryCast` and `DirectCast`, coerce an expression into a type.</span></span>

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

<span data-ttu-id="87dba-690">`DirectCast` und `TryCast` weisen ein spezielles Verhalten auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-690">`DirectCast` and `TryCast` have special behaviors.</span></span> <span data-ttu-id="87dba-691">Aus diesem Grund unterstützen Sie nur native Konvertierungen.</span><span class="sxs-lookup"><span data-stu-id="87dba-691">Because of this, they only support native conversions.</span></span> <span data-ttu-id="87dba-692">Außerdem kann der Zieltyp in einem `TryCast`-Ausdruck kein Werttyp sein.</span><span class="sxs-lookup"><span data-stu-id="87dba-692">Additionally, the target type in a `TryCast` expression cannot be a value type.</span></span> <span data-ttu-id="87dba-693">Benutzerdefinierte Konvertierungs Operatoren werden nicht berücksichtigt, wenn `DirectCast` oder `TryCast` verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-693">User-defined conversion operators are not considered when `DirectCast` or `TryCast` is used.</span></span> <span data-ttu-id="87dba-694">(__Hinweis:__</span><span class="sxs-lookup"><span data-stu-id="87dba-694">(__Note.__</span></span> <span data-ttu-id="87dba-695">Der Konvertierungs Satz, der die Unterstützung von "`DirectCast`" und "`TryCast`" hat, ist eingeschränkt, da Sie "Native CLR"</span><span class="sxs-lookup"><span data-stu-id="87dba-695">The conversion set that `DirectCast` and `TryCast` support are restricted because they implement "native CLR" conversions.</span></span> <span data-ttu-id="87dba-696">Der Zweck von `DirectCast` besteht darin, die Funktionalität der "Unbox"-Anweisung bereitzustellen, während `TryCast` die Funktionalität der "Isinst"-Anweisung bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="87dba-696">The purpose of `DirectCast` is to provide the functionality of the "unbox" instruction, while the purpose of `TryCast` is to provide the functionality of the "isinst" instruction.</span></span> <span data-ttu-id="87dba-697">Da Sie CLR-Anweisungen zugeordnet sind, würden die von der CLR nicht direkt unterstützten Konvertierungen den beabsichtigten Zweck zunichte machen.</span><span class="sxs-lookup"><span data-stu-id="87dba-697">Since they map onto CLR instructions, supporting conversions not directly supported by the CLR would defeat the intended purpose.)</span></span>

<span data-ttu-id="87dba-698">`DirectCast` konvertiert Ausdrücke, die als `Object` typisiert sind, anders als `CType`.</span><span class="sxs-lookup"><span data-stu-id="87dba-698">`DirectCast` converts expressions that are typed as `Object` differently than `CType`.</span></span> <span data-ttu-id="87dba-699">Beim Umrechnen eines Ausdrucks vom Typ "`Object`", dessen Lauf Zeittyp ein primitiver Werttyp ist, löst "`DirectCast`" eine `System.InvalidCastException`-Ausnahme aus, wenn der angegebene Typ nicht mit dem Lauf Zeittyp des Ausdrucks identisch ist, oder ein `System.NullReferenceException`, wenn der Ausdruck als `Nothing` ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-699">When converting an expression of type `Object` whose run-time type is a primitive value type, `DirectCast` throws a `System.InvalidCastException` exception if the specified type is not the same as the run-time type of the expression or a `System.NullReferenceException` if the expression evaluates to `Nothing`.</span></span> <span data-ttu-id="87dba-700">(__Hinweis:__</span><span class="sxs-lookup"><span data-stu-id="87dba-700">(__Note.__</span></span> <span data-ttu-id="87dba-701">Wie bereits erwähnt, wird "`DirectCast`" direkt auf der CLR-Anweisung "Unbox" zugeordnet, wenn der Typ des Ausdrucks `Object` ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-701">As noted above, `DirectCast` maps directly onto the CLR instruction "unbox" when the type of the expression is `Object`.</span></span> <span data-ttu-id="87dba-702">Im Gegensatz dazu wandelt sich `CType` in einen Aufrufer zur Laufzeit um, um die Konvertierung durchzuführen, sodass Konvertierungen zwischen primitiven Typen unterstützt werden können.</span><span class="sxs-lookup"><span data-stu-id="87dba-702">In contrast, `CType` turns into a call to a runtime helper to do the conversion so that conversions between primitive types can be supported.</span></span> <span data-ttu-id="87dba-703">Wenn ein `Object`-Ausdruck in einen primitiven Werttyp konvertiert wird und der Typ der tatsächlichen Instanz dem Zieltyp entspricht, ist `DirectCast` deutlich schneller als `CType`.)</span><span class="sxs-lookup"><span data-stu-id="87dba-703">In the case when an `Object` expression is being converted to a primitive value type and the type of the actual instance match the target type, `DirectCast` will be significantly faster than `CType`.)</span></span>

<span data-ttu-id="87dba-704">`TryCast` konvertiert Ausdrücke, löst jedoch keine Ausnahme aus, wenn der Ausdruck nicht in den Zieltyp konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="87dba-704">`TryCast` converts expressions but does not throw an exception if the expression cannot be converted to the target type.</span></span> <span data-ttu-id="87dba-705">Stattdessen ergibt `TryCast` den `Nothing`, wenn der Ausdruck nicht zur Laufzeit konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="87dba-705">Instead, `TryCast` will result in `Nothing` if the expression cannot be converted at runtime.</span></span> <span data-ttu-id="87dba-706">(__Hinweis:__</span><span class="sxs-lookup"><span data-stu-id="87dba-706">(__Note.__</span></span> <span data-ttu-id="87dba-707">Wie bereits erwähnt, wird `TryCast` direkt auf die CLR-Anweisung "Isinst" zuordnet.</span><span class="sxs-lookup"><span data-stu-id="87dba-707">As noted above, `TryCast` maps directly onto the CLR instruction "isinst".</span></span> <span data-ttu-id="87dba-708">Durch die Kombination der Typüberprüfung und der Konvertierung in einen einzelnen Vorgang kann `TryCast` günstiger sein als eine `TypeOf ... Is` und dann eine `CType`.)</span><span class="sxs-lookup"><span data-stu-id="87dba-708">By combining the type check and the conversion into a single operation, `TryCast` can be cheaper than doing a `TypeOf ... Is` and then a `CType`.)</span></span>

<span data-ttu-id="87dba-709">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-709">For example:</span></span>

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

<span data-ttu-id="87dba-710">Wenn keine Konvertierung vom Typ des Ausdrucks in den angegebenen Typ vorhanden ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-710">If no conversion exists from the type of the expression to the specified type, a compile-time error occurs.</span></span> <span data-ttu-id="87dba-711">Andernfalls wird der Ausdruck als Wert klassifiziert, und das Ergebnis ist der Wert, der von der Konvertierung erzeugt wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-711">Otherwise, the expression is classified as a value and the result is the value produced by the conversion.</span></span>


## <a name="operator-expressions"></a><span data-ttu-id="87dba-712">Operator Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-712">Operator Expressions</span></span>

<span data-ttu-id="87dba-713">Es gibt zwei Arten von Operatoren.</span><span class="sxs-lookup"><span data-stu-id="87dba-713">There are two kinds of operators.</span></span> <span data-ttu-id="87dba-714">*Unäre Operatoren* nehmen einen Operanden an und verwenden eine Präfix Notation (z. b. `-x`).</span><span class="sxs-lookup"><span data-stu-id="87dba-714">*Unary operators* take one operand and use prefix notation (for example, `-x`).</span></span> <span data-ttu-id="87dba-715">*Binäre Operatoren* nehmen zwei Operanden an und verwenden die Infix-Notation (z. b. `x + y`).</span><span class="sxs-lookup"><span data-stu-id="87dba-715">*Binary operators* take two operands and use infix notation (for example, `x + y`).</span></span> <span data-ttu-id="87dba-716">Mit Ausnahme der relationalen Operatoren, die immer `Boolean` ergeben, führt ein Operator, der für einen bestimmten Typ definiert ist, zu diesem Typ.</span><span class="sxs-lookup"><span data-stu-id="87dba-716">With the exception of the relational operators, which always result in `Boolean`, an operator defined for a particular type results in that type.</span></span> <span data-ttu-id="87dba-717">Die Operanden für einen Operator müssen immer als Wert klassifiziert werden. Das Ergebnis eines Operator Ausdrucks wird als Wert klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="87dba-717">The operands to an operator must always be classified as a value; the result of an operator expression is classified as a value.</span></span>

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

### <a name="operator-precedence-and-associativity"></a><span data-ttu-id="87dba-718">Operatorrangfolge und Assoziativität</span><span class="sxs-lookup"><span data-stu-id="87dba-718">Operator Precedence and Associativity</span></span>

<span data-ttu-id="87dba-719">Wenn ein Ausdruck mehrere binäre Operatoren enthält, steuert die *Rangfolge* der Operatoren die Reihenfolge, in der die einzelnen binären Operatoren ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-719">When an expression contains multiple binary operators, the *precedence* of the operators controls the order in which the individual binary operators are evaluated.</span></span> <span data-ttu-id="87dba-720">Der Ausdruck `x + y * z` wird z.B. als `x + (y * z)` ausgewertet, da der `*`-Operator Vorrang vor dem `+`-Operator hat.</span><span class="sxs-lookup"><span data-stu-id="87dba-720">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the `+` operator.</span></span> <span data-ttu-id="87dba-721">In der folgenden Tabelle sind die binären Operatoren in absteigender Rangfolge aufgeführt:</span><span class="sxs-lookup"><span data-stu-id="87dba-721">The following table lists the binary operators in descending order of precedence:</span></span>


| <span data-ttu-id="87dba-722">__Kategorie__</span><span class="sxs-lookup"><span data-stu-id="87dba-722">__Category__</span></span>     | <span data-ttu-id="87dba-723">__Operatoren__</span><span class="sxs-lookup"><span data-stu-id="87dba-723">__Operators__</span></span>                                          | 
|------------------|--------------------------------------------------------|
| <span data-ttu-id="87dba-724">Primär</span><span class="sxs-lookup"><span data-stu-id="87dba-724">Primary</span></span>          | <span data-ttu-id="87dba-725">Alle nicht-Operator Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-725">All non-operator expressions</span></span>                           |
| <span data-ttu-id="87dba-726">Await-</span><span class="sxs-lookup"><span data-stu-id="87dba-726">Await</span></span>            | `Await`                                                |
| <span data-ttu-id="87dba-727">Potenzierung</span><span class="sxs-lookup"><span data-stu-id="87dba-727">Exponentiation</span></span>   | `^`                                                    |
| <span data-ttu-id="87dba-728">Unäre Negation</span><span class="sxs-lookup"><span data-stu-id="87dba-728">Unary negation</span></span>   | <span data-ttu-id="87dba-729">`+`, `-`</span><span class="sxs-lookup"><span data-stu-id="87dba-729">`+`, `-`</span></span>                                               |
| <span data-ttu-id="87dba-730">Multiplikativ</span><span class="sxs-lookup"><span data-stu-id="87dba-730">Multiplicative</span></span>   | <span data-ttu-id="87dba-731">`*`, `/`</span><span class="sxs-lookup"><span data-stu-id="87dba-731">`*`, `/`</span></span>                                               |
| <span data-ttu-id="87dba-732">Ganzzahldivision</span><span class="sxs-lookup"><span data-stu-id="87dba-732">Integer division</span></span> | `\`                                                    |
| <span data-ttu-id="87dba-733">Modulooperator</span><span class="sxs-lookup"><span data-stu-id="87dba-733">Modulus</span></span>          | `Mod`                                                  |
| <span data-ttu-id="87dba-734">Additiv</span><span class="sxs-lookup"><span data-stu-id="87dba-734">Additive</span></span>         | <span data-ttu-id="87dba-735">`+`, `-`</span><span class="sxs-lookup"><span data-stu-id="87dba-735">`+`, `-`</span></span>                                               |
| <span data-ttu-id="87dba-736">Verkettung</span><span class="sxs-lookup"><span data-stu-id="87dba-736">Concatenation</span></span>    | `&`                                                    |
| <span data-ttu-id="87dba-737">Shift</span><span class="sxs-lookup"><span data-stu-id="87dba-737">Shift</span></span>            | <span data-ttu-id="87dba-738">`<<`, `>>`</span><span class="sxs-lookup"><span data-stu-id="87dba-738">`<<`, `>>`</span></span>                                             |
| <span data-ttu-id="87dba-739">Relational</span><span class="sxs-lookup"><span data-stu-id="87dba-739">Relational</span></span>       | <span data-ttu-id="87dba-740">`=`, `<>`, `<`, `>`, `<=`, `>=`, `Like`, `Is`, `IsNot`</span><span class="sxs-lookup"><span data-stu-id="87dba-740">`=`, `<>`, `<`, `>`, `<=`, `>=`, `Like`, `Is`, `IsNot`</span></span> |
| <span data-ttu-id="87dba-741">Logisches NOT</span><span class="sxs-lookup"><span data-stu-id="87dba-741">Logical NOT</span></span>      | `Not`                                                  |
| <span data-ttu-id="87dba-742">Logisches AND</span><span class="sxs-lookup"><span data-stu-id="87dba-742">Logical AND</span></span>      | <span data-ttu-id="87dba-743">`And`, `AndAlso`</span><span class="sxs-lookup"><span data-stu-id="87dba-743">`And`, `AndAlso`</span></span>                                       |
| <span data-ttu-id="87dba-744">Logisches OR</span><span class="sxs-lookup"><span data-stu-id="87dba-744">Logical OR</span></span>       | <span data-ttu-id="87dba-745">`Or`, `OrElse`</span><span class="sxs-lookup"><span data-stu-id="87dba-745">`Or`, `OrElse`</span></span>                                         |
| <span data-ttu-id="87dba-746">Logisches XOR</span><span class="sxs-lookup"><span data-stu-id="87dba-746">Logical XOR</span></span>      | `Xor`                                                  |

<span data-ttu-id="87dba-747">Wenn ein Ausdruck zwei Operatoren mit der gleichen Rangfolge enthält, steuert die *Assoziativität* der Operatoren die Reihenfolge, in der die Vorgänge ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-747">When an expression contains two operators with the same precedence, the *associativity* of the operators controls the order in which the operations are performed.</span></span> <span data-ttu-id="87dba-748">Alle binären Operatoren sind links assoziativ, was bedeutet, dass Vorgänge von links nach rechts ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-748">All binary operators are left-associative, meaning that operations are performed from left to right.</span></span> <span data-ttu-id="87dba-749">Rangfolge und Assoziativität können mithilfe von Klammern gesteuert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-749">Precedence and associativity can be controlled using parenthetical expressions.</span></span>

### <a name="object-operands"></a><span data-ttu-id="87dba-750">Objekt Operanden</span><span class="sxs-lookup"><span data-stu-id="87dba-750">Object Operands</span></span>

<span data-ttu-id="87dba-751">Zusätzlich zu den regulären Typen, die von den einzelnen Operatoren unterstützt werden, unterstützen alle Operatoren Operanden vom Typ `Object`.</span><span class="sxs-lookup"><span data-stu-id="87dba-751">In addition to the regular types supported by each operator, all operators support operands of type `Object`.</span></span> <span data-ttu-id="87dba-752">Operanden, die auf `Object`-Operanden angewendet werden, werden ähnlich wie Methodenaufrufe für `Object`-Werte behandelt: ein spät gebundener Methodenaufruf kann ausgewählt werden. in diesem Fall bestimmt der Lauf Zeittyp der Operanden und nicht der Kompilier Zeittyp die Gültigkeit und den Typ des Betriebs.</span><span class="sxs-lookup"><span data-stu-id="87dba-752">Operators applied to `Object` operands are handled similarly to method calls made on `Object` values: a late-bound method call might be chosen, in which case the run-time type of the operands, rather than the compile-time type, determines the validity and type of the operation.</span></span> <span data-ttu-id="87dba-753">Wenn eine strikte Semantik von der Kompilierungs Umgebung oder `Option Strict` angegeben wird, verursachen Operatoren mit Operanden vom Typ "`Object`" einen Kompilierzeitfehler, mit Ausnahme der Operatoren "`TypeOf...Is`", "`Is`" und "`IsNot`".</span><span class="sxs-lookup"><span data-stu-id="87dba-753">If strict semantics are specified by the compilation environment or by `Option Strict`, any operators with operands of type `Object` cause a compile-time error, except for the `TypeOf...Is`, `Is` and `IsNot` operators.</span></span>

<span data-ttu-id="87dba-754">Wenn die Operator Auflösung festlegt, dass ein Vorgang spät gebunden werden soll, ist das Ergebnis des Vorgangs das Ergebnis der Anwendung des Operators auf die Operandentypen, wenn die Lauf Zeit Typen der Operanden vom Operator unterstützte Typen sind.</span><span class="sxs-lookup"><span data-stu-id="87dba-754">When operator resolution determines that an operation should be performed late-bound, the outcome of the operation is the result of applying the operator to the operand types if the run-time types of the operands are types that are supported by the operator.</span></span> <span data-ttu-id="87dba-755">Der Wert `Nothing` wird als Standardwert des Typs des anderen Operanden in einem binären Operator Ausdruck behandelt.</span><span class="sxs-lookup"><span data-stu-id="87dba-755">The value `Nothing` is treated as the default value of the type of the other operand in a binary operator expression.</span></span> <span data-ttu-id="87dba-756">In einem unären Operator Ausdruck oder wenn beide Operanden in einem binären Operator Ausdruck `Nothing` sind, ist der Typ des Vorgangs `Integer` oder der einzige Ergebnistyp des Operators, wenn der Operator nicht `Integer` ergibt.</span><span class="sxs-lookup"><span data-stu-id="87dba-756">In a unary operator expression, or if both operands are `Nothing` in a binary operator expression, the type of the operation is `Integer` or the only result type of the operator, if the operator does not result in `Integer`.</span></span> <span data-ttu-id="87dba-757">Das Ergebnis des Vorgangs wird immer wieder in `Object` umgewandelt.</span><span class="sxs-lookup"><span data-stu-id="87dba-757">The result of the operation is always then cast back to `Object`.</span></span> <span data-ttu-id="87dba-758">Wenn die Operanden Typen keinen gültigen Operator aufweisen, wird eine Ausnahme vom Typ "`System.InvalidCastException`" ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="87dba-758">If the operand types have no valid operator, a `System.InvalidCastException` exception is thrown.</span></span> <span data-ttu-id="87dba-759">Konvertierungen werden zur Laufzeit ohne Berücksichtigung der impliziten oder expliziten Konvertierung durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="87dba-759">Conversions at run time are done without regard to whether they are implicit or explicit.</span></span>

<span data-ttu-id="87dba-760">Wenn das Ergebnis einer numerischen binären Operation eine Überlauf Ausnahme erzeugt (unabhängig davon, ob die ganzzahlige Überlauf Überprüfung ein-oder ausgeschaltet ist), wird der Ergebnistyp nach Möglichkeit auf den nächsten umfassenderen numerischen Typ herauf gestuft.</span><span class="sxs-lookup"><span data-stu-id="87dba-760">If the result of a numeric binary operation would produce an overflow exception (regardless of whether integer overflow checking is on or off), then the result type is promoted to the next wider numeric type, if possible.</span></span> <span data-ttu-id="87dba-761">Beachten Sie z. B. folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="87dba-761">For example, consider the following code:</span></span>

```vb
Module Test
    Sub Main()
        Dim o As Object = CObj(CByte(2)) * CObj(CByte(255))

        Console.WriteLine(o.GetType().ToString() & " = " & o)
    End Sub
End Module
```

<span data-ttu-id="87dba-762">Es gibt das folgende Ergebnis aus:</span><span class="sxs-lookup"><span data-stu-id="87dba-762">It prints the following result:</span></span>

```console
System.Int16 = 512
```

<span data-ttu-id="87dba-763">Wenn kein größerer numerischer Typ zum Speichern der Zahl verfügbar ist, wird eine Ausnahme vom Typ "`System.OverflowException`" ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="87dba-763">If no wider numeric type is available to hold the number, a `System.OverflowException` exception is thrown.</span></span>

### <a name="operator-resolution"></a><span data-ttu-id="87dba-764">Operator Auflösung</span><span class="sxs-lookup"><span data-stu-id="87dba-764">Operator Resolution</span></span>

<span data-ttu-id="87dba-765">Bei einem Operatortyp und einem Satz von Operanden bestimmt die Operator Auflösung, welcher Operator für die Operanden verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="87dba-765">Given an operator type and a set of operands, operator resolution determines which operator to use for the operands.</span></span> <span data-ttu-id="87dba-766">Beim Auflösen von Operatoren werden benutzerdefinierte Operatoren zunächst mit den folgenden Schritten in Erwägung gezogen:</span><span class="sxs-lookup"><span data-stu-id="87dba-766">When resolving operators, user-defined operators will be considered first, using the following steps:</span></span>

1. <span data-ttu-id="87dba-767">Zunächst werden alle Kandidaten Operatoren gesammelt.</span><span class="sxs-lookup"><span data-stu-id="87dba-767">First, all of the candidate operators are collected.</span></span> <span data-ttu-id="87dba-768">Die Kandidaten Operatoren sind alle benutzerdefinierten Operatoren des jeweiligen Operator Typs im Quelltyp und alle benutzerdefinierten Operatoren des jeweiligen Typs im Zieltyp.</span><span class="sxs-lookup"><span data-stu-id="87dba-768">The candidate operators are all of the user-defined operators of the particular operator type in the source type and all of the user-defined operators of the particular type in the target type.</span></span> <span data-ttu-id="87dba-769">Wenn der Quelltyp und der Zieltyp verknüpft sind, werden allgemeine Operatoren nur einmal berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="87dba-769">If the source type and destination type are related, common operators are only considered once.</span></span>

2. <span data-ttu-id="87dba-770">Anschließend wird die Überladungs Auflösung auf die Operatoren und Operanden angewendet, um den spezifischsten Operator auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="87dba-770">Then, overload resolution is applied to the operators and operands to select the most specific operator.</span></span> <span data-ttu-id="87dba-771">Im Fall von binären Operatoren kann dies zu einem spät gebundenen Aufrufer führen.</span><span class="sxs-lookup"><span data-stu-id="87dba-771">In the case of binary operators, this may result in a late-bound call.</span></span>

<span data-ttu-id="87dba-772">Beim Erfassen der Kandidaten Operatoren für einen Typ `T?` werden stattdessen die Operatoren vom Typ `T` verwendet.</span><span class="sxs-lookup"><span data-stu-id="87dba-772">When collecting the candidate operators for a type `T?`, the operators of type `T` are used instead.</span></span> <span data-ttu-id="87dba-773">Alle benutzerdefinierten Operatoren von `T`, die nur nicht auf NULL festleg Bare Werttypen einschließen, werden ebenfalls angehoben.</span><span class="sxs-lookup"><span data-stu-id="87dba-773">Any of `T`'s user-defined operators that involve only non-nullable value types are also lifted.</span></span> <span data-ttu-id="87dba-774">Ein angehobene Operator verwendet die NULL-Werte, die auf NULL festgelegt werden können, mit Ausnahme der Rückgabe Typen von `IsTrue` und `IsFalse` (die `Boolean` sein müssen).</span><span class="sxs-lookup"><span data-stu-id="87dba-774">A lifted operator uses the nullable version of any value types, with the exception the return types of `IsTrue` and `IsFalse` (which must be `Boolean`).</span></span> <span data-ttu-id="87dba-775">Aufgenommene Operatoren werden ausgewertet, indem die Operanden in Ihre nicht auf NULL festleg Bare Version umgerechnet werden. Anschließend wird der benutzerdefinierte Operator ausgewertet und der Ergebnistyp in seine Version, die NULL-Werte zulässt, transformiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-775">Lifted operators are evaluated by converting the operands to their non-nullable version, then evaluating the user-defined operator and then converting the result type to its nullable version.</span></span> <span data-ttu-id="87dba-776">Wenn der Äther Operand `Nothing` ist, ist das Ergebnis des Ausdrucks ein Wert von `Nothing`, der als auf NULL festleg Bare Version des Ergebnis Typs typisiert ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-776">If ether operand is `Nothing`, the result of the expression is a value of `Nothing` typed as the nullable version of the result type.</span></span> <span data-ttu-id="87dba-777">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-777">For example:</span></span>

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

<span data-ttu-id="87dba-778">Wenn der Operator ein binärer Operator ist und einer der Operanden ein Referenztyp ist, wird der Operator ebenfalls angehoben, aber jede Bindung an den Operator erzeugt einen Fehler.</span><span class="sxs-lookup"><span data-stu-id="87dba-778">If the operator is a binary operator and one of the operands is reference type, the operator is also lifted, but any binding to the operator produces an error.</span></span> <span data-ttu-id="87dba-779">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-779">For example:</span></span>

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

<span data-ttu-id="87dba-780">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-780">__Note.__</span></span> <span data-ttu-id="87dba-781">Diese Regel ist vorhanden, da berücksichtigt wird, ob in einer zukünftigen Version NULL-propagierende Verweis Typen hinzugefügt werden sollen. in diesem Fall würde sich das Verhalten bei binären Operatoren zwischen den beiden Typen ändern.</span><span class="sxs-lookup"><span data-stu-id="87dba-781">This rule exists because there has been consideration whether we wish to add null-propagating reference types in a future version, in which case the behavior in the case of binary operators between the two types would change.</span></span>

<span data-ttu-id="87dba-782">Wie bei Konvertierungen werden benutzerdefinierte Operatoren immer als bevorzugte Operatoren bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="87dba-782">As with conversions, user-defined operators are always preferred over lifted operators.</span></span>

<span data-ttu-id="87dba-783">Beim Auflösen überladener Operatoren kann es Unterschiede zwischen Klassen geben, die in Visual Basic definiert sind, und in anderen Sprachen definierte Klassen:</span><span class="sxs-lookup"><span data-stu-id="87dba-783">When resolving overloaded operators, there may be differences between classes defined in Visual Basic and those defined in other languages:</span></span>

* <span data-ttu-id="87dba-784">In anderen Sprachen können `Not`, `And` und `Or` sowohl als logische Operatoren als auch als bitweise Operatoren überladen werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-784">In other languages, `Not`, `And`, and `Or` may be overloaded both as logical operators and bitwise operators.</span></span> <span data-ttu-id="87dba-785">Beim Importieren aus einer externen Assembly wird jedes Formular als gültige Überladung für diese Operatoren akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-785">Upon import from an external assembly, either form is accepted as a valid overload for these operators.</span></span> <span data-ttu-id="87dba-786">Für einen Typ, der sowohl logische als auch bitweise Operatoren definiert, wird jedoch nur die bitweise Implementierung berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="87dba-786">However, for a type which defines both logical and bitwise operators, only the bitwise implementation will be considered.</span></span>

* <span data-ttu-id="87dba-787">In anderen Sprachen können `>>` und `<<` sowohl als signierte Operatoren als auch als nicht signierte Operatoren überladen werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-787">In other languages, `>>` and `<<` may be overloaded both as signed operators and unsigned operators.</span></span> <span data-ttu-id="87dba-788">Beim Importieren aus einer externen Assembly wird jedes Formular als gültige Überladung akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-788">Upon import from an external assembly, either form is accepted as a valid overload.</span></span> <span data-ttu-id="87dba-789">Für einen Typ, der sowohl signierte als auch nicht signierte Operatoren definiert, wird jedoch nur die signierte Implementierung berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="87dba-789">However, for a type which defines both signed and unsigned operators, only the signed implementation will be considered.</span></span>

* <span data-ttu-id="87dba-790">Wenn kein benutzerdefinierter Operator für die Operanden am spezifischsten ist, werden intrinsische Operatoren berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="87dba-790">If no user-defined operator is most specific to the operands, then intrinsic operators will be considered.</span></span> <span data-ttu-id="87dba-791">Wenn kein System interner Operator für die Operanden definiert ist und der Operand einen typobjekttyp aufweist, wird der Operator spät gebunden aufgelöst. Andernfalls wird ein Fehler bei der Kompilierzeit ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="87dba-791">If no intrinsic operator is defined for the operands and either operand has type Object then the operator will be resolved late-bound; otherwise,  a compile-time error results.</span></span>

<span data-ttu-id="87dba-792">In früheren Versionen von Visual Basic war es ein Fehler, wenn genau ein Operand vom Typ "Object" und keine anwendbaren benutzerdefinierten Operatoren und keine anwendbaren intrinsischen Operatoren vorhanden waren.</span><span class="sxs-lookup"><span data-stu-id="87dba-792">In prior versions of Visual Basic, if there was exactly one operand of type Object, and no applicable user-defined operators, and no applicable intrinsic operators, then it was an error.</span></span> <span data-ttu-id="87dba-793">Ab Visual Basic 11 ist die Lösung spät gebunden.</span><span class="sxs-lookup"><span data-stu-id="87dba-793">As of Visual Basic 11, it is now resolved late-bound.</span></span> <span data-ttu-id="87dba-794">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-794">For example:</span></span>

```vb
Module Module1
  Sub Main()
      Dim p As Object = Nothing
      Dim U As New Uri("http://www.microsoft.com")
      Dim j = U * p  ' is now resolved late-bound
   End Sub
End Module
```

<span data-ttu-id="87dba-795">Ein Typ `T` mit einem intrinsischen Operator definiert auch denselben Operator für `T?`.</span><span class="sxs-lookup"><span data-stu-id="87dba-795">A type `T` that has an intrinsic operator also defines that same operator for `T?`.</span></span> <span data-ttu-id="87dba-796">Das Ergebnis des-Operators auf `T?` ist identisch mit dem-Wert für `T`, mit dem Unterschied, dass, wenn einer der beiden Operanden `Nothing` ist, das Ergebnis des Operators `Nothing` ist (d. h., der NULL-Wert wird weitergegeben).</span><span class="sxs-lookup"><span data-stu-id="87dba-796">The result of the operator on `T?` will be the same as for `T`, except that if either operand is `Nothing`, the result of the operator will be `Nothing` (i.e. the null value is propagated).</span></span> <span data-ttu-id="87dba-797">Um den Typ eines Vorgangs aufzulösen, wird der `?` aus allen darin befindlichen Operanden entfernt, der Typ des Vorgangs wird bestimmt, und dem Typ des Vorgangs wird ein `?` hinzugefügt, wenn einer der Operanden auf NULL festleg Bare Werttypen wäre.</span><span class="sxs-lookup"><span data-stu-id="87dba-797">For the purposes of resolving the type of an operation, the `?` is removed from any operands that have them, the type of the operation is determined, and a `?` is added to the type of the operation if any of the operands were nullable value types.</span></span> <span data-ttu-id="87dba-798">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-798">For example:</span></span>

```vb
Dim v1? As Integer = 10
Dim v2 As Long = 20

' Type of operation will be Long?
Console.WriteLine(v1 + v2)
```

<span data-ttu-id="87dba-799">Jeder Operator listet die systeminternen Typen, für die er definiert ist, und den Typ des Vorgangs auf, der mit den Operanden Typen ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-799">Each operator lists the intrinsic types it is defined for and the type of the operation performed given the operand types.</span></span> <span data-ttu-id="87dba-800">Das Ergebnis des Typs eines intrinsischen Vorgangs folgt den folgenden allgemeinen Regeln:</span><span class="sxs-lookup"><span data-stu-id="87dba-800">The result of type of a intrinsic operation follows these general rules:</span></span>

* <span data-ttu-id="87dba-801">Wenn alle Operanden denselben Typ haben und der Operator für den Typ definiert ist, erfolgt keine Konvertierung, und der Operator für diesen Typ wird verwendet.</span><span class="sxs-lookup"><span data-stu-id="87dba-801">If all operands are of the same type, and the operator is defined for the type, then no conversion occurs and the operator for that type is used.</span></span>

* <span data-ttu-id="87dba-802">Alle Operanden, deren Typ nicht für den-Operator definiert ist, werden mithilfe der folgenden Schritte konvertiert, und der-Operator wird für die neuen Typen aufgelöst:</span><span class="sxs-lookup"><span data-stu-id="87dba-802">Any operand whose type is not defined for the operator is converted using the following steps and the operator is resolved against the new types:</span></span>

  * <span data-ttu-id="87dba-803">Der Operand wird in den nächstgrößten Typ konvertiert, der sowohl für den Operator als auch den Operanden definiert ist und in den er implizit konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="87dba-803">The operand is converted to the next widest type that is defined for both the operator and the operand and to which it is implicitly convertible.</span></span>

  * <span data-ttu-id="87dba-804">Wenn kein solcher Typ vorhanden ist, wird der Operand in den nächst engsten Typ konvertiert, der sowohl für den Operator als auch den Operanden und für den Operanden definiert ist, und an den er implizit konvertierbar ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-804">If there is no such type, then the operand is converted to the next narrowest type that is defined for both the operator and the operand and to which it is implicitly convertible.</span></span>

  * <span data-ttu-id="87dba-805">Wenn kein solcher Typ vorhanden ist oder die Konvertierung nicht durchgeführt werden kann, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-805">If there is no such type or the conversion cannot occur, a compile-time error occurs.</span></span>

* <span data-ttu-id="87dba-806">Andernfalls werden die Operanden in die breitere der Operandentypen konvertiert, und der Operator für diesen Typ wird verwendet.</span><span class="sxs-lookup"><span data-stu-id="87dba-806">Otherwise, the operands are converted to the wider of the operand types and the operator for that type is used.</span></span> <span data-ttu-id="87dba-807">Wenn der Typ des engeren Operanden nicht implizit in den Typ "breiter Operator" konvertiert werden kann, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-807">If the narrower operand type cannot be implicitly converted to the wider operator type, a compile-time error occurs.</span></span>

<span data-ttu-id="87dba-808">Trotz dieser allgemeinen Regeln gibt es jedoch eine Reihe von besonderen Fällen, die in den operatorergebnistabellen genannt werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-808">Despite these general rules, however, there are a number of special cases called out in the operator results tables.</span></span>

<span data-ttu-id="87dba-809">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-809">__Note.__</span></span> <span data-ttu-id="87dba-810">Aus Formatierungs Gründen kürzen die Operatortyp Tabellen die vordefinierten Namen auf die ersten beiden Zeichen.</span><span class="sxs-lookup"><span data-stu-id="87dba-810">For formatting reasons, the operator type tables abbreviate the predefined names to their first two characters.</span></span> <span data-ttu-id="87dba-811">"By" ist also `Byte`, "UI" ist `UInteger`, "St" `String` usw. "Err" bedeutet, dass für die angegebenen Operanden Typen kein Vorgang definiert ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-811">So "By" is `Byte`, "UI" is `UInteger`, "St" is `String`, etc. "Err" means that there is no operation defined for the given operand types.</span></span>

## <a name="arithmetic-operators"></a><span data-ttu-id="87dba-812">Arithmetische Operatoren</span><span class="sxs-lookup"><span data-stu-id="87dba-812">Arithmetic Operators</span></span>

<span data-ttu-id="87dba-813">Die Operatoren `*`, `/`, `\`, `^`, `Mod`, `+` und `-` sind die *arithmetischen Operatoren*.</span><span class="sxs-lookup"><span data-stu-id="87dba-813">The `*`, `/`, `\`, `^`, `Mod`, `+`, and `-` operators are the *arithmetic operators*.</span></span>

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

<span data-ttu-id="87dba-814">Arithmetische Operationen für Gleit Komma Zahlen können mit höherer Genauigkeit als der Ergebnistyp des Vorgangs ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-814">Floating-point arithmetic operations may be performed with higher precision than the result type of the operation.</span></span> <span data-ttu-id="87dba-815">Beispielsweise unterstützen einige Hardwarearchitekturen einen "Extended"-oder "long Double"-Gleit kommatyp mit größerem Bereich und präziser als der `Double`-Typ und führen implizit alle Gleit Komma Vorgänge mit diesem Typ mit höherer Genauigkeit aus.</span><span class="sxs-lookup"><span data-stu-id="87dba-815">For example, some hardware architectures support an "extended" or "long double" floating-point type with greater range and precision than the `Double` type, and implicitly perform all floating-point operations using this higher-precision type.</span></span> <span data-ttu-id="87dba-816">Hardware Architekturen können zur Durchführung von Gleit Komma Vorgängen mit geringerer Genauigkeit nur zu hohen Leistungseinbußen erstellt werden. anstatt eine Implementierung zum Verlust von Leistung und Genauigkeit zu benötigen, Visual Basic ermöglicht, dass der Typ mit höherer Genauigkeit für alle Gleit Komma Vorgänge verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-816">Hardware architectures can be made to perform floating-point operations with less precision only at excessive cost in performance; rather than require an implementation to forfeit both performance and precision, Visual Basic allows the higher-precision type to be used for all floating-point operations.</span></span> <span data-ttu-id="87dba-817">Abgesehen von der Bereitstellung präziseren Ergebnisse hat dies nur selten messbare Auswirkungen.</span><span class="sxs-lookup"><span data-stu-id="87dba-817">Other than delivering more precise results, this rarely has any measurable effects.</span></span> <span data-ttu-id="87dba-818">In Ausdrücken der Form `x * y / z`, wobei die Multiplikation ein Ergebnis erzeugt, das außerhalb des `Double`-Bereichs liegt, aber die nachfolgende Division das temporäre Ergebnis wieder in den `Double`-Bereich bringt, die Tatsache, dass der Ausdruck in einem das Format eines höheren Bereichs kann dazu führen, dass anstelle von unendlich ein endliches Ergebnis erzeugt wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-818">However, in expressions of the form `x * y / z`, where the multiplication produces a result that is outside the `Double` range, but the subsequent division brings the temporary result back into the `Double` range, the fact that the expression is evaluated in a higher-range format may cause a finite result to be produced instead of infinity.</span></span>


### <a name="unary-plus-operator"></a><span data-ttu-id="87dba-819">Unärer Plus-Operator</span><span class="sxs-lookup"><span data-stu-id="87dba-819">Unary Plus Operator</span></span>

```antlr
UnaryPlusExpression
    : '+' Expression
    ;
```

<span data-ttu-id="87dba-820">Der unäre Plus-Operator ist für die Typen "`Byte`", "`SByte`", "`UShort`", "`Short`", "`UInteger`", "`Integer`", "`ULong`", "`Long`" und "0" definiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-820">The unary plus operator is defined for the `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Single`, `Double`, and `Decimal` types.</span></span>

<span data-ttu-id="87dba-821">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="87dba-821">__Operation Type:__</span></span>


| <span data-ttu-id="87dba-822">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-822">__Bo__</span></span> | <span data-ttu-id="87dba-823">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-823">__SB__</span></span> | <span data-ttu-id="87dba-824">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-824">__By__</span></span> | <span data-ttu-id="87dba-825">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-825">__Sh__</span></span> | <span data-ttu-id="87dba-826">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-826">__US__</span></span> | <span data-ttu-id="87dba-827">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-827">__In__</span></span> | <span data-ttu-id="87dba-828">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-828">__UI__</span></span> | <span data-ttu-id="87dba-829">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-829">__Lo__</span></span> | <span data-ttu-id="87dba-830">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-830">__UL__</span></span> | <span data-ttu-id="87dba-831">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-831">__De__</span></span> | <span data-ttu-id="87dba-832">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-832">__Si__</span></span> | <span data-ttu-id="87dba-833">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-833">__Do__</span></span> | <span data-ttu-id="87dba-834">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-834">__Da__</span></span>  | <span data-ttu-id="87dba-835">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-835">__Ch__</span></span>  | <span data-ttu-id="87dba-836">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-836">__St__</span></span> | <span data-ttu-id="87dba-837">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-837">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="87dba-838">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-838">Sh</span></span> | <span data-ttu-id="87dba-839">SB</span><span class="sxs-lookup"><span data-stu-id="87dba-839">SB</span></span> | <span data-ttu-id="87dba-840">um</span><span class="sxs-lookup"><span data-stu-id="87dba-840">By</span></span> | <span data-ttu-id="87dba-841">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-841">Sh</span></span> | <span data-ttu-id="87dba-842">US</span><span class="sxs-lookup"><span data-stu-id="87dba-842">US</span></span> | <span data-ttu-id="87dba-843">In</span><span class="sxs-lookup"><span data-stu-id="87dba-843">In</span></span> | <span data-ttu-id="87dba-844">UI</span><span class="sxs-lookup"><span data-stu-id="87dba-844">UI</span></span> | <span data-ttu-id="87dba-845">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-845">Lo</span></span> | <span data-ttu-id="87dba-846">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-846">UL</span></span> | <span data-ttu-id="87dba-847">De</span><span class="sxs-lookup"><span data-stu-id="87dba-847">De</span></span> | <span data-ttu-id="87dba-848">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-848">Si</span></span> | <span data-ttu-id="87dba-849">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-849">Do</span></span> | <span data-ttu-id="87dba-850">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-850">Err</span></span> | <span data-ttu-id="87dba-851">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-851">Err</span></span> | <span data-ttu-id="87dba-852">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-852">Do</span></span> | <span data-ttu-id="87dba-853">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-853">Ob</span></span> | 


### <a name="unary-minus-operator"></a><span data-ttu-id="87dba-854">Unärer Minus-Operator</span><span class="sxs-lookup"><span data-stu-id="87dba-854">Unary Minus Operator</span></span>

```antlr
UnaryMinusExpression
    : '-' Expression
    ;
```

<span data-ttu-id="87dba-855">Der unäre Minus-Operator ist für die folgenden Typen definiert:</span><span class="sxs-lookup"><span data-stu-id="87dba-855">The unary minus operator is defined for the following types:</span></span>

<span data-ttu-id="87dba-856">`SByte`, `Short`, `Integer`und `Long`.</span><span class="sxs-lookup"><span data-stu-id="87dba-856">`SByte`, `Short`, `Integer`, and `Long`.</span></span> <span data-ttu-id="87dba-857">Das Ergebnis wird durch Subtrahieren des Operanden von 0 (null) berechnet.</span><span class="sxs-lookup"><span data-stu-id="87dba-857">The result is computed by subtracting the operand from zero.</span></span> <span data-ttu-id="87dba-858">Wenn die ganzzahlige Überlauf Überprüfung auf on und der Wert des Operanden das Maximum negative `SByte`, `Short`, `Integer` oder `Long` ist, wird eine `System.OverflowException`-Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="87dba-858">If integer overflow checking is on and the value of the operand is the maximum negative `SByte`, `Short`, `Integer`, or `Long`, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="87dba-859">Andernfalls ist der Wert, wenn der Wert des Operanden der maximale negative `SByte`, `Short`, `Integer` oder `Long` ist, derselbe Wert, und der Überlauf wird nicht gemeldet.</span><span class="sxs-lookup"><span data-stu-id="87dba-859">Otherwise, if the value of the operand is the maximum negative `SByte`, `Short`, `Integer`, or `Long`, the result is that same value, and the overflow is not reported.</span></span>

<span data-ttu-id="87dba-860">`Single` und `Double`.</span><span class="sxs-lookup"><span data-stu-id="87dba-860">`Single` and `Double`.</span></span> <span data-ttu-id="87dba-861">Das Ergebnis ist der Wert des Operanden mit dem zugehörigen Vorzeichen, einschließlich der Werte 0 und unendlich.</span><span class="sxs-lookup"><span data-stu-id="87dba-861">The result is the value of the operand with its sign inverted, including the values 0 and Infinity.</span></span> <span data-ttu-id="87dba-862">Wenn der Operand "NaN" ist, ist das Ergebnis ebenfalls "NaN".</span><span class="sxs-lookup"><span data-stu-id="87dba-862">If the operand is NaN, the result is also NaN.</span></span>

<span data-ttu-id="87dba-863">`Decimal`. installiert haben.</span><span class="sxs-lookup"><span data-stu-id="87dba-863">`Decimal`.</span></span> <span data-ttu-id="87dba-864">Das Ergebnis wird durch Subtrahieren des Operanden von 0 (null) berechnet.</span><span class="sxs-lookup"><span data-stu-id="87dba-864">The result is computed by subtracting the operand from zero.</span></span>

<span data-ttu-id="87dba-865">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="87dba-865">__Operation Type:__</span></span>

| <span data-ttu-id="87dba-866">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-866">__Bo__</span></span> | <span data-ttu-id="87dba-867">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-867">__SB__</span></span> | <span data-ttu-id="87dba-868">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-868">__By__</span></span> | <span data-ttu-id="87dba-869">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-869">__Sh__</span></span> | <span data-ttu-id="87dba-870">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-870">__US__</span></span> | <span data-ttu-id="87dba-871">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-871">__In__</span></span> | <span data-ttu-id="87dba-872">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-872">__UI__</span></span> | <span data-ttu-id="87dba-873">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-873">__Lo__</span></span> | <span data-ttu-id="87dba-874">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-874">__UL__</span></span> | <span data-ttu-id="87dba-875">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-875">__De__</span></span> | <span data-ttu-id="87dba-876">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-876">__Si__</span></span> | <span data-ttu-id="87dba-877">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-877">__Do__</span></span> | <span data-ttu-id="87dba-878">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-878">__Da__</span></span>  | <span data-ttu-id="87dba-879">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-879">__Ch__</span></span>  | <span data-ttu-id="87dba-880">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-880">__St__</span></span> | <span data-ttu-id="87dba-881">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-881">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="87dba-882">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-882">Sh</span></span> | <span data-ttu-id="87dba-883">SB</span><span class="sxs-lookup"><span data-stu-id="87dba-883">SB</span></span> | <span data-ttu-id="87dba-884">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-884">Sh</span></span> | <span data-ttu-id="87dba-885">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-885">Sh</span></span> | <span data-ttu-id="87dba-886">In</span><span class="sxs-lookup"><span data-stu-id="87dba-886">In</span></span> | <span data-ttu-id="87dba-887">In</span><span class="sxs-lookup"><span data-stu-id="87dba-887">In</span></span> | <span data-ttu-id="87dba-888">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-888">Lo</span></span> | <span data-ttu-id="87dba-889">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-889">Lo</span></span> | <span data-ttu-id="87dba-890">De</span><span class="sxs-lookup"><span data-stu-id="87dba-890">De</span></span> | <span data-ttu-id="87dba-891">De</span><span class="sxs-lookup"><span data-stu-id="87dba-891">De</span></span> | <span data-ttu-id="87dba-892">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-892">Si</span></span> | <span data-ttu-id="87dba-893">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-893">Do</span></span> | <span data-ttu-id="87dba-894">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-894">Err</span></span> | <span data-ttu-id="87dba-895">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-895">Err</span></span> | <span data-ttu-id="87dba-896">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-896">Do</span></span> | <span data-ttu-id="87dba-897">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-897">Ob</span></span> | 


### <a name="addition-operator"></a><span data-ttu-id="87dba-898">Additions Operator</span><span class="sxs-lookup"><span data-stu-id="87dba-898">Addition Operator</span></span>

<span data-ttu-id="87dba-899">Der Additions Operator berechnet die Summe der beiden Operanden.</span><span class="sxs-lookup"><span data-stu-id="87dba-899">The addition operator computes the sum of the two operands.</span></span>

```antlr
AdditionOperatorExpression
    : Expression '+' LineTerminator? Expression
    ;
```

<span data-ttu-id="87dba-900">Der Additions Operator ist für die folgenden Typen definiert:</span><span class="sxs-lookup"><span data-stu-id="87dba-900">The addition operator is defined for the following types:</span></span>

* <span data-ttu-id="87dba-901">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` und `Long`.</span><span class="sxs-lookup"><span data-stu-id="87dba-901">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="87dba-902">Wenn die ganzzahlige Überlauf Überprüfung auf on und die Summe außerhalb des Bereichs des Ergebnis Typs liegt, wird eine Ausnahme vom Typ "`System.OverflowException`" ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="87dba-902">If integer overflow checking is on and the sum is outside the range of the result type, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="87dba-903">Andernfalls werden Überläufe nicht gemeldet, und alle signifikanten höherwertigen Bits des Ergebnisses werden verworfen.</span><span class="sxs-lookup"><span data-stu-id="87dba-903">Otherwise, overflows are not reported, and any significant high-order bits of the result are discarded.</span></span>

* <span data-ttu-id="87dba-904">`Single` und `Double`.</span><span class="sxs-lookup"><span data-stu-id="87dba-904">`Single` and `Double`.</span></span> <span data-ttu-id="87dba-905">Die Summe wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.</span><span class="sxs-lookup"><span data-stu-id="87dba-905">The sum is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="87dba-906">`Decimal`. installiert haben.</span><span class="sxs-lookup"><span data-stu-id="87dba-906">`Decimal`.</span></span> <span data-ttu-id="87dba-907">Wenn der resultierende Wert zu groß ist, um im Dezimal Format dargestellt zu werden, wird eine Ausnahme vom Typ "`System.OverflowException`" ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="87dba-907">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="87dba-908">Wenn der Ergebniswert zu klein ist, um im Dezimal Format darzustellen, ist das Ergebnis 0 (null).</span><span class="sxs-lookup"><span data-stu-id="87dba-908">If the result value is too small to represent in the decimal format, the result is 0.</span></span>

* <span data-ttu-id="87dba-909">`String`. installiert haben.</span><span class="sxs-lookup"><span data-stu-id="87dba-909">`String`.</span></span> <span data-ttu-id="87dba-910">Die beiden Operanden `String` werden zusammen verkettet.</span><span class="sxs-lookup"><span data-stu-id="87dba-910">The two `String` operands are concatenated together.</span></span>

* <span data-ttu-id="87dba-911">`Date`. installiert haben.</span><span class="sxs-lookup"><span data-stu-id="87dba-911">`Date`.</span></span> <span data-ttu-id="87dba-912">Der `System.DateTime`-Typ definiert überladene Additions Operatoren.</span><span class="sxs-lookup"><span data-stu-id="87dba-912">The `System.DateTime` type defines overloaded addition operators.</span></span> <span data-ttu-id="87dba-913">Da `System.DateTime` dem intrinsischen `Date`-Typ entspricht, sind diese Operatoren auch für den `Date`-Typ verfügbar.</span><span class="sxs-lookup"><span data-stu-id="87dba-913">Because `System.DateTime` is equivalent to the intrinsic `Date` type, these operators is also available on the `Date` type.</span></span>

<span data-ttu-id="87dba-914">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="87dba-914">__Operation Type:__</span></span>

|        | <span data-ttu-id="87dba-915">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-915">__Bo__</span></span> | <span data-ttu-id="87dba-916">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-916">__SB__</span></span> | <span data-ttu-id="87dba-917">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-917">__By__</span></span> | <span data-ttu-id="87dba-918">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-918">__Sh__</span></span> | <span data-ttu-id="87dba-919">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-919">__US__</span></span> | <span data-ttu-id="87dba-920">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-920">__In__</span></span> | <span data-ttu-id="87dba-921">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-921">__UI__</span></span> | <span data-ttu-id="87dba-922">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-922">__Lo__</span></span> | <span data-ttu-id="87dba-923">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-923">__UL__</span></span> | <span data-ttu-id="87dba-924">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-924">__De__</span></span> | <span data-ttu-id="87dba-925">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-925">__Si__</span></span> | <span data-ttu-id="87dba-926">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-926">__Do__</span></span> | <span data-ttu-id="87dba-927">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-927">__Da__</span></span>  | <span data-ttu-id="87dba-928">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-928">__Ch__</span></span>  | <span data-ttu-id="87dba-929">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-929">__St__</span></span> | <span data-ttu-id="87dba-930">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-930">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="87dba-931">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-931">__Bo__</span></span> | <span data-ttu-id="87dba-932">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-932">Sh</span></span> | <span data-ttu-id="87dba-933">SB</span><span class="sxs-lookup"><span data-stu-id="87dba-933">SB</span></span> | <span data-ttu-id="87dba-934">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-934">Sh</span></span> | <span data-ttu-id="87dba-935">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-935">Sh</span></span> | <span data-ttu-id="87dba-936">In</span><span class="sxs-lookup"><span data-stu-id="87dba-936">In</span></span> | <span data-ttu-id="87dba-937">In</span><span class="sxs-lookup"><span data-stu-id="87dba-937">In</span></span> | <span data-ttu-id="87dba-938">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-938">Lo</span></span> | <span data-ttu-id="87dba-939">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-939">Lo</span></span> | <span data-ttu-id="87dba-940">De</span><span class="sxs-lookup"><span data-stu-id="87dba-940">De</span></span> | <span data-ttu-id="87dba-941">De</span><span class="sxs-lookup"><span data-stu-id="87dba-941">De</span></span> | <span data-ttu-id="87dba-942">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-942">Si</span></span> | <span data-ttu-id="87dba-943">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-943">Do</span></span> | <span data-ttu-id="87dba-944">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-944">Err</span></span> | <span data-ttu-id="87dba-945">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-945">Err</span></span> | <span data-ttu-id="87dba-946">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-946">Do</span></span> | <span data-ttu-id="87dba-947">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-947">Ob</span></span> | 
| <span data-ttu-id="87dba-948">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-948">__SB__</span></span> |    | <span data-ttu-id="87dba-949">SB</span><span class="sxs-lookup"><span data-stu-id="87dba-949">SB</span></span> | <span data-ttu-id="87dba-950">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-950">Sh</span></span> | <span data-ttu-id="87dba-951">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-951">Sh</span></span> | <span data-ttu-id="87dba-952">In</span><span class="sxs-lookup"><span data-stu-id="87dba-952">In</span></span> | <span data-ttu-id="87dba-953">In</span><span class="sxs-lookup"><span data-stu-id="87dba-953">In</span></span> | <span data-ttu-id="87dba-954">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-954">Lo</span></span> | <span data-ttu-id="87dba-955">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-955">Lo</span></span> | <span data-ttu-id="87dba-956">De</span><span class="sxs-lookup"><span data-stu-id="87dba-956">De</span></span> | <span data-ttu-id="87dba-957">De</span><span class="sxs-lookup"><span data-stu-id="87dba-957">De</span></span> | <span data-ttu-id="87dba-958">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-958">Si</span></span> | <span data-ttu-id="87dba-959">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-959">Do</span></span> | <span data-ttu-id="87dba-960">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-960">Err</span></span> | <span data-ttu-id="87dba-961">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-961">Err</span></span> | <span data-ttu-id="87dba-962">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-962">Do</span></span> | <span data-ttu-id="87dba-963">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-963">Ob</span></span> | 
| <span data-ttu-id="87dba-964">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-964">__By__</span></span> |    |    | <span data-ttu-id="87dba-965">um</span><span class="sxs-lookup"><span data-stu-id="87dba-965">By</span></span> | <span data-ttu-id="87dba-966">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-966">Sh</span></span> | <span data-ttu-id="87dba-967">US</span><span class="sxs-lookup"><span data-stu-id="87dba-967">US</span></span> | <span data-ttu-id="87dba-968">In</span><span class="sxs-lookup"><span data-stu-id="87dba-968">In</span></span> | <span data-ttu-id="87dba-969">UI</span><span class="sxs-lookup"><span data-stu-id="87dba-969">UI</span></span> | <span data-ttu-id="87dba-970">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-970">Lo</span></span> | <span data-ttu-id="87dba-971">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-971">UL</span></span> | <span data-ttu-id="87dba-972">De</span><span class="sxs-lookup"><span data-stu-id="87dba-972">De</span></span> | <span data-ttu-id="87dba-973">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-973">Si</span></span> | <span data-ttu-id="87dba-974">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-974">Do</span></span> | <span data-ttu-id="87dba-975">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-975">Err</span></span> | <span data-ttu-id="87dba-976">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-976">Err</span></span> | <span data-ttu-id="87dba-977">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-977">Do</span></span> | <span data-ttu-id="87dba-978">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-978">Ob</span></span> | 
| <span data-ttu-id="87dba-979">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-979">__Sh__</span></span> |    |    |    | <span data-ttu-id="87dba-980">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-980">Sh</span></span> | <span data-ttu-id="87dba-981">In</span><span class="sxs-lookup"><span data-stu-id="87dba-981">In</span></span> | <span data-ttu-id="87dba-982">In</span><span class="sxs-lookup"><span data-stu-id="87dba-982">In</span></span> | <span data-ttu-id="87dba-983">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-983">Lo</span></span> | <span data-ttu-id="87dba-984">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-984">Lo</span></span> | <span data-ttu-id="87dba-985">De</span><span class="sxs-lookup"><span data-stu-id="87dba-985">De</span></span> | <span data-ttu-id="87dba-986">De</span><span class="sxs-lookup"><span data-stu-id="87dba-986">De</span></span> | <span data-ttu-id="87dba-987">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-987">Si</span></span> | <span data-ttu-id="87dba-988">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-988">Do</span></span> | <span data-ttu-id="87dba-989">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-989">Err</span></span> | <span data-ttu-id="87dba-990">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-990">Err</span></span> | <span data-ttu-id="87dba-991">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-991">Do</span></span> | <span data-ttu-id="87dba-992">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-992">Ob</span></span> | 
| <span data-ttu-id="87dba-993">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-993">__US__</span></span> |    |    |    |    | <span data-ttu-id="87dba-994">US</span><span class="sxs-lookup"><span data-stu-id="87dba-994">US</span></span> | <span data-ttu-id="87dba-995">In</span><span class="sxs-lookup"><span data-stu-id="87dba-995">In</span></span> | <span data-ttu-id="87dba-996">UI</span><span class="sxs-lookup"><span data-stu-id="87dba-996">UI</span></span> | <span data-ttu-id="87dba-997">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-997">Lo</span></span> | <span data-ttu-id="87dba-998">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-998">UL</span></span> | <span data-ttu-id="87dba-999">De</span><span class="sxs-lookup"><span data-stu-id="87dba-999">De</span></span> | <span data-ttu-id="87dba-1000">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1000">Si</span></span> | <span data-ttu-id="87dba-1001">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1001">Do</span></span> | <span data-ttu-id="87dba-1002">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1002">Err</span></span> | <span data-ttu-id="87dba-1003">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1003">Err</span></span> | <span data-ttu-id="87dba-1004">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1004">Do</span></span> | <span data-ttu-id="87dba-1005">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1005">Ob</span></span> | 
| <span data-ttu-id="87dba-1006">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-1006">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="87dba-1007">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1007">In</span></span> | <span data-ttu-id="87dba-1008">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1008">Lo</span></span> | <span data-ttu-id="87dba-1009">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1009">Lo</span></span> | <span data-ttu-id="87dba-1010">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1010">De</span></span> | <span data-ttu-id="87dba-1011">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1011">De</span></span> | <span data-ttu-id="87dba-1012">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1012">Si</span></span> | <span data-ttu-id="87dba-1013">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1013">Do</span></span> | <span data-ttu-id="87dba-1014">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1014">Err</span></span> | <span data-ttu-id="87dba-1015">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1015">Err</span></span> | <span data-ttu-id="87dba-1016">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1016">Do</span></span> | <span data-ttu-id="87dba-1017">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1017">Ob</span></span> | 
| <span data-ttu-id="87dba-1018">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1018">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="87dba-1019">UI</span><span class="sxs-lookup"><span data-stu-id="87dba-1019">UI</span></span> | <span data-ttu-id="87dba-1020">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1020">Lo</span></span> | <span data-ttu-id="87dba-1021">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-1021">UL</span></span> | <span data-ttu-id="87dba-1022">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1022">De</span></span> | <span data-ttu-id="87dba-1023">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1023">Si</span></span> | <span data-ttu-id="87dba-1024">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1024">Do</span></span> | <span data-ttu-id="87dba-1025">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1025">Err</span></span> | <span data-ttu-id="87dba-1026">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1026">Err</span></span> | <span data-ttu-id="87dba-1027">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1027">Do</span></span> | <span data-ttu-id="87dba-1028">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1028">Ob</span></span> | 
| <span data-ttu-id="87dba-1029">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-1029">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1030">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1030">Lo</span></span> | <span data-ttu-id="87dba-1031">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1031">De</span></span> | <span data-ttu-id="87dba-1032">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1032">De</span></span> | <span data-ttu-id="87dba-1033">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1033">Si</span></span> | <span data-ttu-id="87dba-1034">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1034">Do</span></span> | <span data-ttu-id="87dba-1035">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1035">Err</span></span> | <span data-ttu-id="87dba-1036">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1036">Err</span></span> | <span data-ttu-id="87dba-1037">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1037">Do</span></span> | <span data-ttu-id="87dba-1038">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1038">Ob</span></span> | 
| <span data-ttu-id="87dba-1039">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1039">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1040">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-1040">UL</span></span> | <span data-ttu-id="87dba-1041">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1041">De</span></span> | <span data-ttu-id="87dba-1042">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1042">Si</span></span> | <span data-ttu-id="87dba-1043">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1043">Do</span></span> | <span data-ttu-id="87dba-1044">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1044">Err</span></span> | <span data-ttu-id="87dba-1045">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1045">Err</span></span> | <span data-ttu-id="87dba-1046">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1046">Do</span></span> | <span data-ttu-id="87dba-1047">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1047">Ob</span></span> | 
| <span data-ttu-id="87dba-1048">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-1048">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1049">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1049">De</span></span> | <span data-ttu-id="87dba-1050">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1050">Si</span></span> | <span data-ttu-id="87dba-1051">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1051">Do</span></span> | <span data-ttu-id="87dba-1052">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1052">Err</span></span> | <span data-ttu-id="87dba-1053">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1053">Err</span></span> | <span data-ttu-id="87dba-1054">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1054">Do</span></span> | <span data-ttu-id="87dba-1055">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1055">Ob</span></span> | 
| <span data-ttu-id="87dba-1056">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-1056">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1057">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1057">Si</span></span> | <span data-ttu-id="87dba-1058">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1058">Do</span></span> | <span data-ttu-id="87dba-1059">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1059">Err</span></span> | <span data-ttu-id="87dba-1060">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1060">Err</span></span> | <span data-ttu-id="87dba-1061">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1061">Do</span></span> | <span data-ttu-id="87dba-1062">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1062">Ob</span></span> | 
| <span data-ttu-id="87dba-1063">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-1063">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1064">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1064">Do</span></span> | <span data-ttu-id="87dba-1065">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1065">Err</span></span> | <span data-ttu-id="87dba-1066">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1066">Err</span></span> | <span data-ttu-id="87dba-1067">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1067">Do</span></span> | <span data-ttu-id="87dba-1068">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1068">Ob</span></span> | 
| <span data-ttu-id="87dba-1069">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-1069">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1070">St</span><span class="sxs-lookup"><span data-stu-id="87dba-1070">St</span></span>  | <span data-ttu-id="87dba-1071">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1071">Err</span></span> | <span data-ttu-id="87dba-1072">St</span><span class="sxs-lookup"><span data-stu-id="87dba-1072">St</span></span> | <span data-ttu-id="87dba-1073">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1073">Ob</span></span> | 
| <span data-ttu-id="87dba-1074">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-1074">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="87dba-1075">St</span><span class="sxs-lookup"><span data-stu-id="87dba-1075">St</span></span>  | <span data-ttu-id="87dba-1076">St</span><span class="sxs-lookup"><span data-stu-id="87dba-1076">St</span></span> | <span data-ttu-id="87dba-1077">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1077">Ob</span></span> | 
| <span data-ttu-id="87dba-1078">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-1078">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="87dba-1079">St</span><span class="sxs-lookup"><span data-stu-id="87dba-1079">St</span></span> | <span data-ttu-id="87dba-1080">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1080">Ob</span></span> | 
| <span data-ttu-id="87dba-1081">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-1081">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | <span data-ttu-id="87dba-1082">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1082">Ob</span></span> | 


### <a name="subtraction-operator"></a><span data-ttu-id="87dba-1083">Subtraktions Operator</span><span class="sxs-lookup"><span data-stu-id="87dba-1083">Subtraction Operator</span></span>

<span data-ttu-id="87dba-1084">Der Subtraktions Operator subtrahiert den zweiten Operanden vom ersten Operanden.</span><span class="sxs-lookup"><span data-stu-id="87dba-1084">The subtraction operator subtracts the second operand from the first operand.</span></span>

```antlr
SubtractionOperatorExpression
    : Expression '-' LineTerminator? Expression
    ;
```

<span data-ttu-id="87dba-1085">Der Subtraktions Operator ist für die folgenden Typen definiert:</span><span class="sxs-lookup"><span data-stu-id="87dba-1085">The subtraction operator is defined for the following types:</span></span>

* <span data-ttu-id="87dba-1086">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` und `Long`.</span><span class="sxs-lookup"><span data-stu-id="87dba-1086">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="87dba-1087">Wenn die ganzzahlige Überlauf Überprüfung auf on und der Unterschied außerhalb des Bereichs des Ergebnis Typs liegt, wird eine Ausnahme vom Typ "`System.OverflowException`" ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="87dba-1087">If integer overflow checking is on and the difference is outside the range of the result type, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="87dba-1088">Andernfalls werden Überläufe nicht gemeldet, und alle signifikanten höherwertigen Bits des Ergebnisses werden verworfen.</span><span class="sxs-lookup"><span data-stu-id="87dba-1088">Otherwise, overflows are not reported, and any significant high-order bits of the result are discarded.</span></span>

* <span data-ttu-id="87dba-1089">`Single` und `Double`.</span><span class="sxs-lookup"><span data-stu-id="87dba-1089">`Single` and `Double`.</span></span> <span data-ttu-id="87dba-1090">Der Unterschied wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.</span><span class="sxs-lookup"><span data-stu-id="87dba-1090">The difference is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="87dba-1091">`Decimal`. installiert haben.</span><span class="sxs-lookup"><span data-stu-id="87dba-1091">`Decimal`.</span></span> <span data-ttu-id="87dba-1092">Wenn der resultierende Wert zu groß ist, um im Dezimal Format dargestellt zu werden, wird eine Ausnahme vom Typ "`System.OverflowException`" ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="87dba-1092">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="87dba-1093">Wenn der Ergebniswert zu klein ist, um im Dezimal Format darzustellen, ist das Ergebnis 0 (null).</span><span class="sxs-lookup"><span data-stu-id="87dba-1093">If the result value is too small to represent in the decimal format, the result is 0.</span></span>

* <span data-ttu-id="87dba-1094">`Date`. installiert haben.</span><span class="sxs-lookup"><span data-stu-id="87dba-1094">`Date`.</span></span> <span data-ttu-id="87dba-1095">Der `System.DateTime`-Typ definiert überladene Subtraktions Operatoren.</span><span class="sxs-lookup"><span data-stu-id="87dba-1095">The `System.DateTime` type defines overloaded subtraction operators.</span></span> <span data-ttu-id="87dba-1096">Da `System.DateTime` dem intrinsischen `Date`-Typ entspricht, sind diese Operatoren auch für den `Date`-Typ verfügbar.</span><span class="sxs-lookup"><span data-stu-id="87dba-1096">Because `System.DateTime` is equivalent to the intrinsic `Date` type, these operators is also available on the `Date` type.</span></span>

<span data-ttu-id="87dba-1097">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="87dba-1097">__Operation Type:__</span></span>

|        | <span data-ttu-id="87dba-1098">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-1098">__Bo__</span></span> | <span data-ttu-id="87dba-1099">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-1099">__SB__</span></span> | <span data-ttu-id="87dba-1100">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-1100">__By__</span></span> | <span data-ttu-id="87dba-1101">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-1101">__Sh__</span></span> | <span data-ttu-id="87dba-1102">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1102">__US__</span></span> | <span data-ttu-id="87dba-1103">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-1103">__In__</span></span> | <span data-ttu-id="87dba-1104">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1104">__UI__</span></span> | <span data-ttu-id="87dba-1105">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-1105">__Lo__</span></span> | <span data-ttu-id="87dba-1106">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1106">__UL__</span></span> | <span data-ttu-id="87dba-1107">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-1107">__De__</span></span> | <span data-ttu-id="87dba-1108">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-1108">__Si__</span></span> | <span data-ttu-id="87dba-1109">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-1109">__Do__</span></span> | <span data-ttu-id="87dba-1110">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-1110">__Da__</span></span>  | <span data-ttu-id="87dba-1111">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-1111">__Ch__</span></span>  | <span data-ttu-id="87dba-1112">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-1112">__St__</span></span> | <span data-ttu-id="87dba-1113">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-1113">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="87dba-1114">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-1114">__Bo__</span></span> | <span data-ttu-id="87dba-1115">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1115">Sh</span></span> | <span data-ttu-id="87dba-1116">SB</span><span class="sxs-lookup"><span data-stu-id="87dba-1116">SB</span></span> | <span data-ttu-id="87dba-1117">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1117">Sh</span></span> | <span data-ttu-id="87dba-1118">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1118">Sh</span></span> | <span data-ttu-id="87dba-1119">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1119">In</span></span> | <span data-ttu-id="87dba-1120">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1120">In</span></span> | <span data-ttu-id="87dba-1121">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1121">Lo</span></span> | <span data-ttu-id="87dba-1122">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1122">Lo</span></span> | <span data-ttu-id="87dba-1123">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1123">De</span></span> | <span data-ttu-id="87dba-1124">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1124">De</span></span> | <span data-ttu-id="87dba-1125">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1125">Si</span></span> | <span data-ttu-id="87dba-1126">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1126">Do</span></span> | <span data-ttu-id="87dba-1127">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1127">Err</span></span> | <span data-ttu-id="87dba-1128">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1128">Err</span></span> | <span data-ttu-id="87dba-1129">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1129">Do</span></span>  | <span data-ttu-id="87dba-1130">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1130">Ob</span></span>  | 
| <span data-ttu-id="87dba-1131">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-1131">__SB__</span></span> |    | <span data-ttu-id="87dba-1132">SB</span><span class="sxs-lookup"><span data-stu-id="87dba-1132">SB</span></span> | <span data-ttu-id="87dba-1133">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1133">Sh</span></span> | <span data-ttu-id="87dba-1134">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1134">Sh</span></span> | <span data-ttu-id="87dba-1135">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1135">In</span></span> | <span data-ttu-id="87dba-1136">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1136">In</span></span> | <span data-ttu-id="87dba-1137">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1137">Lo</span></span> | <span data-ttu-id="87dba-1138">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1138">Lo</span></span> | <span data-ttu-id="87dba-1139">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1139">De</span></span> | <span data-ttu-id="87dba-1140">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1140">De</span></span> | <span data-ttu-id="87dba-1141">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1141">Si</span></span> | <span data-ttu-id="87dba-1142">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1142">Do</span></span> | <span data-ttu-id="87dba-1143">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1143">Err</span></span> | <span data-ttu-id="87dba-1144">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1144">Err</span></span> | <span data-ttu-id="87dba-1145">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1145">Do</span></span>  | <span data-ttu-id="87dba-1146">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1146">Ob</span></span>  | 
| <span data-ttu-id="87dba-1147">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-1147">__By__</span></span> |    |    | <span data-ttu-id="87dba-1148">um</span><span class="sxs-lookup"><span data-stu-id="87dba-1148">By</span></span> | <span data-ttu-id="87dba-1149">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1149">Sh</span></span> | <span data-ttu-id="87dba-1150">US</span><span class="sxs-lookup"><span data-stu-id="87dba-1150">US</span></span> | <span data-ttu-id="87dba-1151">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1151">In</span></span> | <span data-ttu-id="87dba-1152">UI</span><span class="sxs-lookup"><span data-stu-id="87dba-1152">UI</span></span> | <span data-ttu-id="87dba-1153">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1153">Lo</span></span> | <span data-ttu-id="87dba-1154">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-1154">UL</span></span> | <span data-ttu-id="87dba-1155">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1155">De</span></span> | <span data-ttu-id="87dba-1156">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1156">Si</span></span> | <span data-ttu-id="87dba-1157">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1157">Do</span></span> | <span data-ttu-id="87dba-1158">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1158">Err</span></span> | <span data-ttu-id="87dba-1159">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1159">Err</span></span> | <span data-ttu-id="87dba-1160">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1160">Do</span></span>  | <span data-ttu-id="87dba-1161">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1161">Ob</span></span>  | 
| <span data-ttu-id="87dba-1162">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-1162">__Sh__</span></span> |    |    |    | <span data-ttu-id="87dba-1163">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1163">Sh</span></span> | <span data-ttu-id="87dba-1164">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1164">In</span></span> | <span data-ttu-id="87dba-1165">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1165">In</span></span> | <span data-ttu-id="87dba-1166">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1166">Lo</span></span> | <span data-ttu-id="87dba-1167">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1167">Lo</span></span> | <span data-ttu-id="87dba-1168">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1168">De</span></span> | <span data-ttu-id="87dba-1169">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1169">De</span></span> | <span data-ttu-id="87dba-1170">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1170">Si</span></span> | <span data-ttu-id="87dba-1171">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1171">Do</span></span> | <span data-ttu-id="87dba-1172">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1172">Err</span></span> | <span data-ttu-id="87dba-1173">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1173">Err</span></span> | <span data-ttu-id="87dba-1174">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1174">Do</span></span>  | <span data-ttu-id="87dba-1175">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1175">Ob</span></span>  | 
| <span data-ttu-id="87dba-1176">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1176">__US__</span></span> |    |    |    |    | <span data-ttu-id="87dba-1177">US</span><span class="sxs-lookup"><span data-stu-id="87dba-1177">US</span></span> | <span data-ttu-id="87dba-1178">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1178">In</span></span> | <span data-ttu-id="87dba-1179">UI</span><span class="sxs-lookup"><span data-stu-id="87dba-1179">UI</span></span> | <span data-ttu-id="87dba-1180">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1180">Lo</span></span> | <span data-ttu-id="87dba-1181">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-1181">UL</span></span> | <span data-ttu-id="87dba-1182">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1182">De</span></span> | <span data-ttu-id="87dba-1183">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1183">Si</span></span> | <span data-ttu-id="87dba-1184">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1184">Do</span></span> | <span data-ttu-id="87dba-1185">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1185">Err</span></span> | <span data-ttu-id="87dba-1186">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1186">Err</span></span> | <span data-ttu-id="87dba-1187">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1187">Do</span></span>  | <span data-ttu-id="87dba-1188">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1188">Ob</span></span>  | 
| <span data-ttu-id="87dba-1189">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-1189">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="87dba-1190">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1190">In</span></span> | <span data-ttu-id="87dba-1191">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1191">Lo</span></span> | <span data-ttu-id="87dba-1192">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1192">Lo</span></span> | <span data-ttu-id="87dba-1193">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1193">De</span></span> | <span data-ttu-id="87dba-1194">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1194">De</span></span> | <span data-ttu-id="87dba-1195">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1195">Si</span></span> | <span data-ttu-id="87dba-1196">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1196">Do</span></span> | <span data-ttu-id="87dba-1197">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1197">Err</span></span> | <span data-ttu-id="87dba-1198">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1198">Err</span></span> | <span data-ttu-id="87dba-1199">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1199">Do</span></span>  | <span data-ttu-id="87dba-1200">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1200">Ob</span></span>  | 
| <span data-ttu-id="87dba-1201">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1201">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="87dba-1202">UI</span><span class="sxs-lookup"><span data-stu-id="87dba-1202">UI</span></span> | <span data-ttu-id="87dba-1203">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1203">Lo</span></span> | <span data-ttu-id="87dba-1204">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-1204">UL</span></span> | <span data-ttu-id="87dba-1205">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1205">De</span></span> | <span data-ttu-id="87dba-1206">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1206">Si</span></span> | <span data-ttu-id="87dba-1207">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1207">Do</span></span> | <span data-ttu-id="87dba-1208">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1208">Err</span></span> | <span data-ttu-id="87dba-1209">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1209">Err</span></span> | <span data-ttu-id="87dba-1210">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1210">Do</span></span>  | <span data-ttu-id="87dba-1211">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1211">Ob</span></span>  | 
| <span data-ttu-id="87dba-1212">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-1212">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1213">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1213">Lo</span></span> | <span data-ttu-id="87dba-1214">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1214">De</span></span> | <span data-ttu-id="87dba-1215">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1215">De</span></span> | <span data-ttu-id="87dba-1216">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1216">Si</span></span> | <span data-ttu-id="87dba-1217">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1217">Do</span></span> | <span data-ttu-id="87dba-1218">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1218">Err</span></span> | <span data-ttu-id="87dba-1219">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1219">Err</span></span> | <span data-ttu-id="87dba-1220">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1220">Do</span></span>  | <span data-ttu-id="87dba-1221">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1221">Ob</span></span>  | 
| <span data-ttu-id="87dba-1222">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1222">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1223">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-1223">UL</span></span> | <span data-ttu-id="87dba-1224">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1224">De</span></span> | <span data-ttu-id="87dba-1225">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1225">Si</span></span> | <span data-ttu-id="87dba-1226">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1226">Do</span></span> | <span data-ttu-id="87dba-1227">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1227">Err</span></span> | <span data-ttu-id="87dba-1228">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1228">Err</span></span> | <span data-ttu-id="87dba-1229">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1229">Do</span></span>  | <span data-ttu-id="87dba-1230">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1230">Ob</span></span>  | 
| <span data-ttu-id="87dba-1231">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-1231">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1232">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1232">De</span></span> | <span data-ttu-id="87dba-1233">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1233">Si</span></span> | <span data-ttu-id="87dba-1234">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1234">Do</span></span> | <span data-ttu-id="87dba-1235">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1235">Err</span></span> | <span data-ttu-id="87dba-1236">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1236">Err</span></span> | <span data-ttu-id="87dba-1237">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1237">Do</span></span>  | <span data-ttu-id="87dba-1238">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1238">Ob</span></span>  | 
| <span data-ttu-id="87dba-1239">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-1239">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1240">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1240">Si</span></span> | <span data-ttu-id="87dba-1241">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1241">Do</span></span> | <span data-ttu-id="87dba-1242">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1242">Err</span></span> | <span data-ttu-id="87dba-1243">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1243">Err</span></span> | <span data-ttu-id="87dba-1244">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1244">Do</span></span>  | <span data-ttu-id="87dba-1245">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1245">Ob</span></span>  | 
| <span data-ttu-id="87dba-1246">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-1246">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1247">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1247">Do</span></span> | <span data-ttu-id="87dba-1248">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1248">Err</span></span> | <span data-ttu-id="87dba-1249">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1249">Err</span></span> | <span data-ttu-id="87dba-1250">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1250">Do</span></span>  | <span data-ttu-id="87dba-1251">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1251">Ob</span></span>  | 
| <span data-ttu-id="87dba-1252">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-1252">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1253">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1253">Err</span></span> | <span data-ttu-id="87dba-1254">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1254">Err</span></span> | <span data-ttu-id="87dba-1255">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1255">Err</span></span> | <span data-ttu-id="87dba-1256">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1256">Err</span></span> | 
| <span data-ttu-id="87dba-1257">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-1257">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="87dba-1258">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1258">Err</span></span> | <span data-ttu-id="87dba-1259">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1259">Err</span></span> | <span data-ttu-id="87dba-1260">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1260">Err</span></span> | 
| <span data-ttu-id="87dba-1261">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-1261">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="87dba-1262">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1262">Do</span></span>  | <span data-ttu-id="87dba-1263">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1263">Ob</span></span>  | 
| <span data-ttu-id="87dba-1264">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-1264">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="87dba-1265">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1265">Ob</span></span>  | 


### <a name="multiplication-operator"></a><span data-ttu-id="87dba-1266">Multiplikations Operator</span><span class="sxs-lookup"><span data-stu-id="87dba-1266">Multiplication Operator</span></span>

<span data-ttu-id="87dba-1267">Der Multiplikations Operator berechnet das Produkt von zwei-Operanden.</span><span class="sxs-lookup"><span data-stu-id="87dba-1267">The multiplication operator computes the product of two operands.</span></span>

```antlr
MultiplicationOperatorExpression
    : Expression '*' LineTerminator? Expression
    ;
```

<span data-ttu-id="87dba-1268">Der Multiplikations Operator ist für die folgenden Typen definiert:</span><span class="sxs-lookup"><span data-stu-id="87dba-1268">The multiplication operator is defined for the following types:</span></span>

* <span data-ttu-id="87dba-1269">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` und `Long`.</span><span class="sxs-lookup"><span data-stu-id="87dba-1269">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="87dba-1270">Wenn die ganzzahlige Überlauf Überprüfung auf on und das Produkt außerhalb des Bereichs des Ergebnis Typs liegt, wird eine Ausnahme vom Typ "`System.OverflowException`" ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="87dba-1270">If integer overflow checking is on and the product is outside the range of the result type, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="87dba-1271">Andernfalls werden Überläufe nicht gemeldet, und alle signifikanten höherwertigen Bits des Ergebnisses werden verworfen.</span><span class="sxs-lookup"><span data-stu-id="87dba-1271">Otherwise, overflows are not reported, and any significant high-order bits of the result are discarded.</span></span>

* <span data-ttu-id="87dba-1272">`Single` und `Double`.</span><span class="sxs-lookup"><span data-stu-id="87dba-1272">`Single` and `Double`.</span></span> <span data-ttu-id="87dba-1273">Das Produkt wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.</span><span class="sxs-lookup"><span data-stu-id="87dba-1273">The product is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="87dba-1274">`Decimal`. installiert haben.</span><span class="sxs-lookup"><span data-stu-id="87dba-1274">`Decimal`.</span></span> <span data-ttu-id="87dba-1275">Wenn der resultierende Wert zu groß ist, um im Dezimal Format dargestellt zu werden, wird eine Ausnahme vom Typ "`System.OverflowException`" ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="87dba-1275">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="87dba-1276">Wenn der Ergebniswert zu klein ist, um im Dezimal Format darzustellen, ist das Ergebnis 0 (null).</span><span class="sxs-lookup"><span data-stu-id="87dba-1276">If the result value is too small to represent in the decimal format, the result is 0.</span></span>

<span data-ttu-id="87dba-1277">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="87dba-1277">__Operation Type:__</span></span>

|        | <span data-ttu-id="87dba-1278">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-1278">__Bo__</span></span> | <span data-ttu-id="87dba-1279">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-1279">__SB__</span></span> | <span data-ttu-id="87dba-1280">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-1280">__By__</span></span> | <span data-ttu-id="87dba-1281">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-1281">__Sh__</span></span> | <span data-ttu-id="87dba-1282">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1282">__US__</span></span> | <span data-ttu-id="87dba-1283">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-1283">__In__</span></span> | <span data-ttu-id="87dba-1284">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1284">__UI__</span></span> | <span data-ttu-id="87dba-1285">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-1285">__Lo__</span></span> | <span data-ttu-id="87dba-1286">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1286">__UL__</span></span> | <span data-ttu-id="87dba-1287">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-1287">__De__</span></span> | <span data-ttu-id="87dba-1288">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-1288">__Si__</span></span> | <span data-ttu-id="87dba-1289">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-1289">__Do__</span></span> | <span data-ttu-id="87dba-1290">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-1290">__Da__</span></span>  | <span data-ttu-id="87dba-1291">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-1291">__Ch__</span></span>  | <span data-ttu-id="87dba-1292">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-1292">__St__</span></span> | <span data-ttu-id="87dba-1293">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-1293">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="87dba-1294">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-1294">__Bo__</span></span> | <span data-ttu-id="87dba-1295">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1295">Sh</span></span> | <span data-ttu-id="87dba-1296">SB</span><span class="sxs-lookup"><span data-stu-id="87dba-1296">SB</span></span> | <span data-ttu-id="87dba-1297">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1297">Sh</span></span> | <span data-ttu-id="87dba-1298">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1298">Sh</span></span> | <span data-ttu-id="87dba-1299">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1299">In</span></span> | <span data-ttu-id="87dba-1300">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1300">In</span></span> | <span data-ttu-id="87dba-1301">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1301">Lo</span></span> | <span data-ttu-id="87dba-1302">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1302">Lo</span></span> | <span data-ttu-id="87dba-1303">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1303">De</span></span> | <span data-ttu-id="87dba-1304">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1304">De</span></span> | <span data-ttu-id="87dba-1305">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1305">Si</span></span> | <span data-ttu-id="87dba-1306">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1306">Do</span></span> | <span data-ttu-id="87dba-1307">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1307">Err</span></span> | <span data-ttu-id="87dba-1308">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1308">Err</span></span> | <span data-ttu-id="87dba-1309">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1309">Do</span></span>  | <span data-ttu-id="87dba-1310">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1310">Ob</span></span>  | 
| <span data-ttu-id="87dba-1311">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-1311">__SB__</span></span> |    | <span data-ttu-id="87dba-1312">SB</span><span class="sxs-lookup"><span data-stu-id="87dba-1312">SB</span></span> | <span data-ttu-id="87dba-1313">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1313">Sh</span></span> | <span data-ttu-id="87dba-1314">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1314">Sh</span></span> | <span data-ttu-id="87dba-1315">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1315">In</span></span> | <span data-ttu-id="87dba-1316">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1316">In</span></span> | <span data-ttu-id="87dba-1317">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1317">Lo</span></span> | <span data-ttu-id="87dba-1318">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1318">Lo</span></span> | <span data-ttu-id="87dba-1319">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1319">De</span></span> | <span data-ttu-id="87dba-1320">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1320">De</span></span> | <span data-ttu-id="87dba-1321">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1321">Si</span></span> | <span data-ttu-id="87dba-1322">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1322">Do</span></span> | <span data-ttu-id="87dba-1323">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1323">Err</span></span> | <span data-ttu-id="87dba-1324">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1324">Err</span></span> | <span data-ttu-id="87dba-1325">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1325">Do</span></span>  | <span data-ttu-id="87dba-1326">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1326">Ob</span></span>  | 
| <span data-ttu-id="87dba-1327">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-1327">__By__</span></span> |    |    | <span data-ttu-id="87dba-1328">um</span><span class="sxs-lookup"><span data-stu-id="87dba-1328">By</span></span> | <span data-ttu-id="87dba-1329">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1329">Sh</span></span> | <span data-ttu-id="87dba-1330">US</span><span class="sxs-lookup"><span data-stu-id="87dba-1330">US</span></span> | <span data-ttu-id="87dba-1331">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1331">In</span></span> | <span data-ttu-id="87dba-1332">UI</span><span class="sxs-lookup"><span data-stu-id="87dba-1332">UI</span></span> | <span data-ttu-id="87dba-1333">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1333">Lo</span></span> | <span data-ttu-id="87dba-1334">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-1334">UL</span></span> | <span data-ttu-id="87dba-1335">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1335">De</span></span> | <span data-ttu-id="87dba-1336">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1336">Si</span></span> | <span data-ttu-id="87dba-1337">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1337">Do</span></span> | <span data-ttu-id="87dba-1338">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1338">Err</span></span> | <span data-ttu-id="87dba-1339">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1339">Err</span></span> | <span data-ttu-id="87dba-1340">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1340">Do</span></span>  | <span data-ttu-id="87dba-1341">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1341">Ob</span></span>  | 
| <span data-ttu-id="87dba-1342">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-1342">__Sh__</span></span> |    |    |    | <span data-ttu-id="87dba-1343">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1343">Sh</span></span> | <span data-ttu-id="87dba-1344">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1344">In</span></span> | <span data-ttu-id="87dba-1345">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1345">In</span></span> | <span data-ttu-id="87dba-1346">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1346">Lo</span></span> | <span data-ttu-id="87dba-1347">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1347">Lo</span></span> | <span data-ttu-id="87dba-1348">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1348">De</span></span> | <span data-ttu-id="87dba-1349">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1349">De</span></span> | <span data-ttu-id="87dba-1350">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1350">Si</span></span> | <span data-ttu-id="87dba-1351">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1351">Do</span></span> | <span data-ttu-id="87dba-1352">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1352">Err</span></span> | <span data-ttu-id="87dba-1353">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1353">Err</span></span> | <span data-ttu-id="87dba-1354">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1354">Do</span></span>  | <span data-ttu-id="87dba-1355">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1355">Ob</span></span>  | 
| <span data-ttu-id="87dba-1356">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1356">__US__</span></span> |    |    |    |    | <span data-ttu-id="87dba-1357">US</span><span class="sxs-lookup"><span data-stu-id="87dba-1357">US</span></span> | <span data-ttu-id="87dba-1358">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1358">In</span></span> | <span data-ttu-id="87dba-1359">UI</span><span class="sxs-lookup"><span data-stu-id="87dba-1359">UI</span></span> | <span data-ttu-id="87dba-1360">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1360">Lo</span></span> | <span data-ttu-id="87dba-1361">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-1361">UL</span></span> | <span data-ttu-id="87dba-1362">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1362">De</span></span> | <span data-ttu-id="87dba-1363">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1363">Si</span></span> | <span data-ttu-id="87dba-1364">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1364">Do</span></span> | <span data-ttu-id="87dba-1365">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1365">Err</span></span> | <span data-ttu-id="87dba-1366">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1366">Err</span></span> | <span data-ttu-id="87dba-1367">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1367">Do</span></span>  | <span data-ttu-id="87dba-1368">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1368">Ob</span></span>  | 
| <span data-ttu-id="87dba-1369">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-1369">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="87dba-1370">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1370">In</span></span> | <span data-ttu-id="87dba-1371">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1371">Lo</span></span> | <span data-ttu-id="87dba-1372">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1372">Lo</span></span> | <span data-ttu-id="87dba-1373">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1373">De</span></span> | <span data-ttu-id="87dba-1374">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1374">De</span></span> | <span data-ttu-id="87dba-1375">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1375">Si</span></span> | <span data-ttu-id="87dba-1376">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1376">Do</span></span> | <span data-ttu-id="87dba-1377">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1377">Err</span></span> | <span data-ttu-id="87dba-1378">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1378">Err</span></span> | <span data-ttu-id="87dba-1379">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1379">Do</span></span>  | <span data-ttu-id="87dba-1380">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1380">Ob</span></span>  | 
| <span data-ttu-id="87dba-1381">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1381">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="87dba-1382">UI</span><span class="sxs-lookup"><span data-stu-id="87dba-1382">UI</span></span> | <span data-ttu-id="87dba-1383">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1383">Lo</span></span> | <span data-ttu-id="87dba-1384">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-1384">UL</span></span> | <span data-ttu-id="87dba-1385">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1385">De</span></span> | <span data-ttu-id="87dba-1386">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1386">Si</span></span> | <span data-ttu-id="87dba-1387">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1387">Do</span></span> | <span data-ttu-id="87dba-1388">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1388">Err</span></span> | <span data-ttu-id="87dba-1389">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1389">Err</span></span> | <span data-ttu-id="87dba-1390">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1390">Do</span></span>  | <span data-ttu-id="87dba-1391">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1391">Ob</span></span>  | 
| <span data-ttu-id="87dba-1392">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-1392">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1393">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1393">Lo</span></span> | <span data-ttu-id="87dba-1394">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1394">De</span></span> | <span data-ttu-id="87dba-1395">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1395">De</span></span> | <span data-ttu-id="87dba-1396">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1396">Si</span></span> | <span data-ttu-id="87dba-1397">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1397">Do</span></span> | <span data-ttu-id="87dba-1398">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1398">Err</span></span> | <span data-ttu-id="87dba-1399">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1399">Err</span></span> | <span data-ttu-id="87dba-1400">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1400">Do</span></span>  | <span data-ttu-id="87dba-1401">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1401">Ob</span></span>  | 
| <span data-ttu-id="87dba-1402">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1402">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1403">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-1403">UL</span></span> | <span data-ttu-id="87dba-1404">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1404">De</span></span> | <span data-ttu-id="87dba-1405">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1405">Si</span></span> | <span data-ttu-id="87dba-1406">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1406">Do</span></span> | <span data-ttu-id="87dba-1407">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1407">Err</span></span> | <span data-ttu-id="87dba-1408">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1408">Err</span></span> | <span data-ttu-id="87dba-1409">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1409">Do</span></span>  | <span data-ttu-id="87dba-1410">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1410">Ob</span></span>  | 
| <span data-ttu-id="87dba-1411">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-1411">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1412">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1412">De</span></span> | <span data-ttu-id="87dba-1413">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1413">Si</span></span> | <span data-ttu-id="87dba-1414">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1414">Do</span></span> | <span data-ttu-id="87dba-1415">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1415">Err</span></span> | <span data-ttu-id="87dba-1416">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1416">Err</span></span> | <span data-ttu-id="87dba-1417">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1417">Do</span></span>  | <span data-ttu-id="87dba-1418">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1418">Ob</span></span>  | 
| <span data-ttu-id="87dba-1419">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-1419">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1420">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1420">Si</span></span> | <span data-ttu-id="87dba-1421">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1421">Do</span></span> | <span data-ttu-id="87dba-1422">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1422">Err</span></span> | <span data-ttu-id="87dba-1423">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1423">Err</span></span> | <span data-ttu-id="87dba-1424">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1424">Do</span></span>  | <span data-ttu-id="87dba-1425">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1425">Ob</span></span>  | 
| <span data-ttu-id="87dba-1426">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-1426">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1427">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1427">Do</span></span> | <span data-ttu-id="87dba-1428">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1428">Err</span></span> | <span data-ttu-id="87dba-1429">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1429">Err</span></span> | <span data-ttu-id="87dba-1430">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1430">Do</span></span>  | <span data-ttu-id="87dba-1431">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1431">Ob</span></span>  | 
| <span data-ttu-id="87dba-1432">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-1432">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1433">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1433">Err</span></span> | <span data-ttu-id="87dba-1434">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1434">Err</span></span> | <span data-ttu-id="87dba-1435">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1435">Err</span></span> | <span data-ttu-id="87dba-1436">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1436">Err</span></span> | 
| <span data-ttu-id="87dba-1437">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-1437">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="87dba-1438">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1438">Err</span></span> | <span data-ttu-id="87dba-1439">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1439">Err</span></span> | <span data-ttu-id="87dba-1440">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1440">Err</span></span> | 
| <span data-ttu-id="87dba-1441">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-1441">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="87dba-1442">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1442">Do</span></span>  | <span data-ttu-id="87dba-1443">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1443">Ob</span></span>  | 
| <span data-ttu-id="87dba-1444">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-1444">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="87dba-1445">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1445">Ob</span></span>  | 


### <a name="division-operators"></a><span data-ttu-id="87dba-1446">Divisions Operatoren</span><span class="sxs-lookup"><span data-stu-id="87dba-1446">Division Operators</span></span>

<span data-ttu-id="87dba-1447">Divisions Operatoren berechnen den Quotienten von zwei Operanden.</span><span class="sxs-lookup"><span data-stu-id="87dba-1447">Division operators compute the quotient of two operands.</span></span> <span data-ttu-id="87dba-1448">Es gibt zwei Divisions Operatoren: den regulären Operator (Gleit Komma) und den Operator für die ganzzahlige Division.</span><span class="sxs-lookup"><span data-stu-id="87dba-1448">There are two division operators: the regular (floating-point) division operator and the integer division operator.</span></span>

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

<span data-ttu-id="87dba-1449">Der Operator für reguläre Division ist für die folgenden Typen definiert:</span><span class="sxs-lookup"><span data-stu-id="87dba-1449">The regular division operator is defined for the following types:</span></span>

* <span data-ttu-id="87dba-1450">`Single` und `Double`.</span><span class="sxs-lookup"><span data-stu-id="87dba-1450">`Single` and `Double`.</span></span> <span data-ttu-id="87dba-1451">Der Quotienten wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.</span><span class="sxs-lookup"><span data-stu-id="87dba-1451">The quotient is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="87dba-1452">`Decimal`. installiert haben.</span><span class="sxs-lookup"><span data-stu-id="87dba-1452">`Decimal`.</span></span> <span data-ttu-id="87dba-1453">Wenn der Wert des rechten Operanden 0 (null) ist, wird eine Ausnahme vom Typ "`System.DivideByZeroException`" ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="87dba-1453">If the value of the right operand is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="87dba-1454">Wenn der resultierende Wert zu groß ist, um im Dezimal Format dargestellt zu werden, wird eine Ausnahme vom Typ "`System.OverflowException`" ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="87dba-1454">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="87dba-1455">Wenn der Ergebniswert zu klein ist, um im Dezimal Format darzustellen, ist das Ergebnis 0 (null).</span><span class="sxs-lookup"><span data-stu-id="87dba-1455">If the result value is too small to represent in the decimal format, the result is zero.</span></span> <span data-ttu-id="87dba-1456">Die Dezimalstellen des Ergebnisses, vor der Rundung, sind die nächstgelegene Skala für die bevorzugte Skala, die ein Ergebnis gleich dem exakten Ergebnis behält.</span><span class="sxs-lookup"><span data-stu-id="87dba-1456">The scale of the result, before any rounding, is the closest scale to the preferred scale which will preserve a result equal to the exact result.</span></span>  <span data-ttu-id="87dba-1457">Die bevorzugte Skala ist die Skala des ersten Operanden abzüglich der Skala des zweiten Operanden.</span><span class="sxs-lookup"><span data-stu-id="87dba-1457">The preferred scale is the scale of the first operand less the scale of the second operand.</span></span>

<span data-ttu-id="87dba-1458">Gemäß den normalen Operator Auflösungs Regeln würden reguläre Teilungen, die ausschließlich zwischen Operanden von Typen wie `Byte`, `Short`, `Integer` und `Long` stehen, dazu führen, dass beide Operanden in den Typ `Decimal` konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-1458">According to normal operator resolution rules, regular division purely between operands of types such as `Byte`, `Short`, `Integer`, and `Long` would cause both operands to be converted to type `Decimal`.</span></span> <span data-ttu-id="87dba-1459">Wenn jedoch die Operator Auflösung für den Divisions Operator ausgeführt wird, wenn keiner der Typen `Decimal` ist, gilt `Double` als schmaler als `Decimal`.</span><span class="sxs-lookup"><span data-stu-id="87dba-1459">However, when doing operator resolution on the division operator when neither type is `Decimal`, `Double` is considered narrower than `Decimal`.</span></span> <span data-ttu-id="87dba-1460">Diese Konvention wird befolgt, weil die Division von `Double` effizienter ist als `Decimal`-Division.</span><span class="sxs-lookup"><span data-stu-id="87dba-1460">This convention is followed because `Double` division is more efficient than `Decimal` division.</span></span>

<span data-ttu-id="87dba-1461">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="87dba-1461">__Operation Type:__</span></span>

|        | <span data-ttu-id="87dba-1462">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-1462">__Bo__</span></span> | <span data-ttu-id="87dba-1463">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-1463">__SB__</span></span> | <span data-ttu-id="87dba-1464">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-1464">__By__</span></span> | <span data-ttu-id="87dba-1465">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-1465">__Sh__</span></span> | <span data-ttu-id="87dba-1466">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1466">__US__</span></span> | <span data-ttu-id="87dba-1467">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-1467">__In__</span></span> | <span data-ttu-id="87dba-1468">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1468">__UI__</span></span> | <span data-ttu-id="87dba-1469">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-1469">__Lo__</span></span> | <span data-ttu-id="87dba-1470">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1470">__UL__</span></span> | <span data-ttu-id="87dba-1471">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-1471">__De__</span></span> | <span data-ttu-id="87dba-1472">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-1472">__Si__</span></span> | <span data-ttu-id="87dba-1473">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-1473">__Do__</span></span> | <span data-ttu-id="87dba-1474">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-1474">__Da__</span></span>  | <span data-ttu-id="87dba-1475">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-1475">__Ch__</span></span>  | <span data-ttu-id="87dba-1476">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-1476">__St__</span></span> | <span data-ttu-id="87dba-1477">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-1477">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="87dba-1478">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-1478">__Bo__</span></span> | <span data-ttu-id="87dba-1479">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1479">Do</span></span> | <span data-ttu-id="87dba-1480">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1480">Do</span></span> | <span data-ttu-id="87dba-1481">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1481">Do</span></span> | <span data-ttu-id="87dba-1482">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1482">Do</span></span> | <span data-ttu-id="87dba-1483">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1483">Do</span></span> | <span data-ttu-id="87dba-1484">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1484">Do</span></span> | <span data-ttu-id="87dba-1485">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1485">Do</span></span> | <span data-ttu-id="87dba-1486">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1486">Do</span></span> | <span data-ttu-id="87dba-1487">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1487">Do</span></span> | <span data-ttu-id="87dba-1488">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1488">De</span></span> | <span data-ttu-id="87dba-1489">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1489">Si</span></span> | <span data-ttu-id="87dba-1490">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1490">Do</span></span> | <span data-ttu-id="87dba-1491">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1491">Err</span></span> | <span data-ttu-id="87dba-1492">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1492">Err</span></span> | <span data-ttu-id="87dba-1493">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1493">Do</span></span>  | <span data-ttu-id="87dba-1494">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1494">Ob</span></span>  | 
| <span data-ttu-id="87dba-1495">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-1495">__SB__</span></span> |    | <span data-ttu-id="87dba-1496">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1496">Do</span></span> | <span data-ttu-id="87dba-1497">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1497">Do</span></span> | <span data-ttu-id="87dba-1498">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1498">Do</span></span> | <span data-ttu-id="87dba-1499">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1499">Do</span></span> | <span data-ttu-id="87dba-1500">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1500">Do</span></span> | <span data-ttu-id="87dba-1501">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1501">Do</span></span> | <span data-ttu-id="87dba-1502">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1502">Do</span></span> | <span data-ttu-id="87dba-1503">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1503">Do</span></span> | <span data-ttu-id="87dba-1504">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1504">De</span></span> | <span data-ttu-id="87dba-1505">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1505">Si</span></span> | <span data-ttu-id="87dba-1506">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1506">Do</span></span> | <span data-ttu-id="87dba-1507">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1507">Err</span></span> | <span data-ttu-id="87dba-1508">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1508">Err</span></span> | <span data-ttu-id="87dba-1509">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1509">Do</span></span>  | <span data-ttu-id="87dba-1510">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1510">Ob</span></span>  | 
| <span data-ttu-id="87dba-1511">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-1511">__By__</span></span> |    |    | <span data-ttu-id="87dba-1512">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1512">Do</span></span> | <span data-ttu-id="87dba-1513">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1513">Do</span></span> | <span data-ttu-id="87dba-1514">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1514">Do</span></span> | <span data-ttu-id="87dba-1515">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1515">Do</span></span> | <span data-ttu-id="87dba-1516">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1516">Do</span></span> | <span data-ttu-id="87dba-1517">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1517">Do</span></span> | <span data-ttu-id="87dba-1518">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1518">Do</span></span> | <span data-ttu-id="87dba-1519">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1519">De</span></span> | <span data-ttu-id="87dba-1520">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1520">Si</span></span> | <span data-ttu-id="87dba-1521">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1521">Do</span></span> | <span data-ttu-id="87dba-1522">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1522">Err</span></span> | <span data-ttu-id="87dba-1523">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1523">Err</span></span> | <span data-ttu-id="87dba-1524">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1524">Do</span></span>  | <span data-ttu-id="87dba-1525">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1525">Ob</span></span>  | 
| <span data-ttu-id="87dba-1526">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-1526">__Sh__</span></span> |    |    |    | <span data-ttu-id="87dba-1527">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1527">Do</span></span> | <span data-ttu-id="87dba-1528">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1528">Do</span></span> | <span data-ttu-id="87dba-1529">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1529">Do</span></span> | <span data-ttu-id="87dba-1530">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1530">Do</span></span> | <span data-ttu-id="87dba-1531">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1531">Do</span></span> | <span data-ttu-id="87dba-1532">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1532">Do</span></span> | <span data-ttu-id="87dba-1533">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1533">De</span></span> | <span data-ttu-id="87dba-1534">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1534">Si</span></span> | <span data-ttu-id="87dba-1535">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1535">Do</span></span> | <span data-ttu-id="87dba-1536">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1536">Err</span></span> | <span data-ttu-id="87dba-1537">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1537">Err</span></span> | <span data-ttu-id="87dba-1538">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1538">Do</span></span>  | <span data-ttu-id="87dba-1539">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1539">Ob</span></span>  | 
| <span data-ttu-id="87dba-1540">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1540">__US__</span></span> |    |    |    |    | <span data-ttu-id="87dba-1541">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1541">Do</span></span> | <span data-ttu-id="87dba-1542">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1542">Do</span></span> | <span data-ttu-id="87dba-1543">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1543">Do</span></span> | <span data-ttu-id="87dba-1544">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1544">Do</span></span> | <span data-ttu-id="87dba-1545">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1545">Do</span></span> | <span data-ttu-id="87dba-1546">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1546">De</span></span> | <span data-ttu-id="87dba-1547">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1547">Si</span></span> | <span data-ttu-id="87dba-1548">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1548">Do</span></span> | <span data-ttu-id="87dba-1549">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1549">Err</span></span> | <span data-ttu-id="87dba-1550">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1550">Err</span></span> | <span data-ttu-id="87dba-1551">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1551">Do</span></span>  | <span data-ttu-id="87dba-1552">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1552">Ob</span></span>  | 
| <span data-ttu-id="87dba-1553">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-1553">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="87dba-1554">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1554">Do</span></span> | <span data-ttu-id="87dba-1555">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1555">Do</span></span> | <span data-ttu-id="87dba-1556">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1556">Do</span></span> | <span data-ttu-id="87dba-1557">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1557">Do</span></span> | <span data-ttu-id="87dba-1558">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1558">De</span></span> | <span data-ttu-id="87dba-1559">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1559">Si</span></span> | <span data-ttu-id="87dba-1560">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1560">Do</span></span> | <span data-ttu-id="87dba-1561">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1561">Err</span></span> | <span data-ttu-id="87dba-1562">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1562">Err</span></span> | <span data-ttu-id="87dba-1563">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1563">Do</span></span>  | <span data-ttu-id="87dba-1564">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1564">Ob</span></span>  | 
| <span data-ttu-id="87dba-1565">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1565">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="87dba-1566">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1566">Do</span></span> | <span data-ttu-id="87dba-1567">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1567">Do</span></span> | <span data-ttu-id="87dba-1568">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1568">Do</span></span> | <span data-ttu-id="87dba-1569">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1569">De</span></span> | <span data-ttu-id="87dba-1570">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1570">Si</span></span> | <span data-ttu-id="87dba-1571">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1571">Do</span></span> | <span data-ttu-id="87dba-1572">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1572">Err</span></span> | <span data-ttu-id="87dba-1573">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1573">Err</span></span> | <span data-ttu-id="87dba-1574">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1574">Do</span></span>  | <span data-ttu-id="87dba-1575">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1575">Ob</span></span>  | 
| <span data-ttu-id="87dba-1576">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-1576">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1577">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1577">Do</span></span> | <span data-ttu-id="87dba-1578">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1578">Do</span></span> | <span data-ttu-id="87dba-1579">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1579">De</span></span> | <span data-ttu-id="87dba-1580">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1580">Si</span></span> | <span data-ttu-id="87dba-1581">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1581">Do</span></span> | <span data-ttu-id="87dba-1582">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1582">Err</span></span> | <span data-ttu-id="87dba-1583">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1583">Err</span></span> | <span data-ttu-id="87dba-1584">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1584">Do</span></span>  | <span data-ttu-id="87dba-1585">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1585">Ob</span></span>  | 
| <span data-ttu-id="87dba-1586">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1586">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1587">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1587">Do</span></span> | <span data-ttu-id="87dba-1588">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1588">De</span></span> | <span data-ttu-id="87dba-1589">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1589">Si</span></span> | <span data-ttu-id="87dba-1590">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1590">Do</span></span> | <span data-ttu-id="87dba-1591">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1591">Err</span></span> | <span data-ttu-id="87dba-1592">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1592">Err</span></span> | <span data-ttu-id="87dba-1593">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1593">Do</span></span>  | <span data-ttu-id="87dba-1594">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1594">Ob</span></span>  | 
| <span data-ttu-id="87dba-1595">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-1595">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1596">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1596">De</span></span> | <span data-ttu-id="87dba-1597">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1597">Si</span></span> | <span data-ttu-id="87dba-1598">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1598">Do</span></span> | <span data-ttu-id="87dba-1599">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1599">Err</span></span> | <span data-ttu-id="87dba-1600">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1600">Err</span></span> | <span data-ttu-id="87dba-1601">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1601">Do</span></span>  | <span data-ttu-id="87dba-1602">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1602">Ob</span></span>  | 
| <span data-ttu-id="87dba-1603">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-1603">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1604">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1604">Si</span></span> | <span data-ttu-id="87dba-1605">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1605">Do</span></span> | <span data-ttu-id="87dba-1606">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1606">Err</span></span> | <span data-ttu-id="87dba-1607">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1607">Err</span></span> | <span data-ttu-id="87dba-1608">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1608">Do</span></span>  | <span data-ttu-id="87dba-1609">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1609">Ob</span></span>  | 
| <span data-ttu-id="87dba-1610">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-1610">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1611">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1611">Do</span></span> | <span data-ttu-id="87dba-1612">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1612">Err</span></span> | <span data-ttu-id="87dba-1613">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1613">Err</span></span> | <span data-ttu-id="87dba-1614">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1614">Do</span></span>  | <span data-ttu-id="87dba-1615">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1615">Ob</span></span>  | 
| <span data-ttu-id="87dba-1616">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-1616">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1617">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1617">Err</span></span> | <span data-ttu-id="87dba-1618">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1618">Err</span></span> | <span data-ttu-id="87dba-1619">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1619">Err</span></span> | <span data-ttu-id="87dba-1620">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1620">Err</span></span> | 
| <span data-ttu-id="87dba-1621">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-1621">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="87dba-1622">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1622">Err</span></span> | <span data-ttu-id="87dba-1623">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1623">Err</span></span> | <span data-ttu-id="87dba-1624">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1624">Err</span></span> | 
| <span data-ttu-id="87dba-1625">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-1625">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="87dba-1626">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1626">Do</span></span>  | <span data-ttu-id="87dba-1627">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1627">Ob</span></span>  | 
| <span data-ttu-id="87dba-1628">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-1628">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="87dba-1629">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1629">Ob</span></span>  | 

<span data-ttu-id="87dba-1630">Der Operator für die ganzzahlige Division ist für `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` und `Long` definiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-1630">The integer division operator is defined for `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="87dba-1631">Wenn der Wert des rechten Operanden 0 (null) ist, wird eine Ausnahme vom Typ "`System.DivideByZeroException`" ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="87dba-1631">If the value of the right operand is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="87dba-1632">Die Division rundet das Ergebnis auf NULL, und der absolute Wert des Ergebnisses ist die größtmögliche Ganzzahl, die kleiner ist als der absolute Wert des Quotienten der beiden Operanden.</span><span class="sxs-lookup"><span data-stu-id="87dba-1632">The division rounds the result towards zero, and the absolute value of the result is the largest possible integer that is less than the absolute value of the quotient of the two operands.</span></span> <span data-ttu-id="87dba-1633">Das Ergebnis ist 0 (null) oder positiv, wenn die beiden Operanden dasselbe Vorzeichen aufweisen, und 0 (null) oder negativ, wenn die beiden Operanden gegenüberliegende Vorzeichen verfügen.</span><span class="sxs-lookup"><span data-stu-id="87dba-1633">The result is zero or positive when the two operands have the same sign, and zero or negative when the two operands have opposite signs.</span></span> <span data-ttu-id="87dba-1634">Wenn der linke Operand der maximale negative `SByte`, `Short`, `Integer` oder `Long` und der rechte Operand `-1` ist, tritt ein Überlauf auf. Wenn die ganzzahlige Überlauf Überprüfung on ist, wird eine `System.OverflowException`-Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="87dba-1634">If the left operand is the maximum negative `SByte`, `Short`, `Integer`, or `Long`, and the right operand is `-1`, an overflow occurs; if integer overflow checking is on, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="87dba-1635">Andernfalls wird der Überlauf nicht gemeldet, und das Ergebnis ist stattdessen der Wert des linken Operanden.</span><span class="sxs-lookup"><span data-stu-id="87dba-1635">Otherwise, the overflow is not reported and the result is instead the value of the left operand.</span></span>

<span data-ttu-id="87dba-1636">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-1636">__Note.__</span></span> <span data-ttu-id="87dba-1637">Da die beiden Operanden für nicht signierte Typen immer 0 (null) oder positiv sind, ist das Ergebnis immer 0 (null) oder positiv.</span><span class="sxs-lookup"><span data-stu-id="87dba-1637">As the two operands for unsigned types will always be zero or positive, the result is always zero or positive.</span></span>  <span data-ttu-id="87dba-1638">Da das Ergebnis des Ausdrucks immer kleiner oder gleich dem größten der beiden Operanden ist, ist es nicht möglich, dass ein Überlauf auftritt.</span><span class="sxs-lookup"><span data-stu-id="87dba-1638">As the result of the expression will always be less than or equal to the largest of the two operands, it is not possible for an overflow to occur.</span></span>  <span data-ttu-id="87dba-1639">Da eine solche ganzzahlige Überlauf Überprüfung nicht für eine ganzzahlige Teilung mit zwei Ganzzahlen ohne Vorzeichen ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-1639">As such integer overflow checking is not performed for integer divide with two unsigned integers.</span></span> <span data-ttu-id="87dba-1640">Das Ergebnis ist der-Typ des linken Operanden.</span><span class="sxs-lookup"><span data-stu-id="87dba-1640">The result is the type as that of the left operand.</span></span>


<span data-ttu-id="87dba-1641">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="87dba-1641">__Operation Type:__</span></span>

|        | <span data-ttu-id="87dba-1642">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-1642">__Bo__</span></span> | <span data-ttu-id="87dba-1643">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-1643">__SB__</span></span> | <span data-ttu-id="87dba-1644">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-1644">__By__</span></span> | <span data-ttu-id="87dba-1645">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-1645">__Sh__</span></span> | <span data-ttu-id="87dba-1646">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1646">__US__</span></span> | <span data-ttu-id="87dba-1647">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-1647">__In__</span></span> | <span data-ttu-id="87dba-1648">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1648">__UI__</span></span> | <span data-ttu-id="87dba-1649">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-1649">__Lo__</span></span> | <span data-ttu-id="87dba-1650">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1650">__UL__</span></span> | <span data-ttu-id="87dba-1651">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-1651">__De__</span></span> | <span data-ttu-id="87dba-1652">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-1652">__Si__</span></span> | <span data-ttu-id="87dba-1653">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-1653">__Do__</span></span> | <span data-ttu-id="87dba-1654">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-1654">__Da__</span></span>  | <span data-ttu-id="87dba-1655">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-1655">__Ch__</span></span>  | <span data-ttu-id="87dba-1656">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-1656">__St__</span></span> | <span data-ttu-id="87dba-1657">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-1657">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="87dba-1658">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-1658">__Bo__</span></span> | <span data-ttu-id="87dba-1659">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1659">Sh</span></span> | <span data-ttu-id="87dba-1660">SB</span><span class="sxs-lookup"><span data-stu-id="87dba-1660">SB</span></span> | <span data-ttu-id="87dba-1661">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1661">Sh</span></span> | <span data-ttu-id="87dba-1662">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1662">Sh</span></span> | <span data-ttu-id="87dba-1663">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1663">In</span></span> | <span data-ttu-id="87dba-1664">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1664">In</span></span> | <span data-ttu-id="87dba-1665">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1665">Lo</span></span> | <span data-ttu-id="87dba-1666">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1666">Lo</span></span> | <span data-ttu-id="87dba-1667">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1667">Lo</span></span> | <span data-ttu-id="87dba-1668">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1668">Lo</span></span> | <span data-ttu-id="87dba-1669">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1669">Lo</span></span> | <span data-ttu-id="87dba-1670">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1670">Lo</span></span> | <span data-ttu-id="87dba-1671">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1671">Err</span></span> | <span data-ttu-id="87dba-1672">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1672">Err</span></span> | <span data-ttu-id="87dba-1673">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1673">Lo</span></span>  | <span data-ttu-id="87dba-1674">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1674">Ob</span></span>  | 
| <span data-ttu-id="87dba-1675">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-1675">__SB__</span></span> |    | <span data-ttu-id="87dba-1676">SB</span><span class="sxs-lookup"><span data-stu-id="87dba-1676">SB</span></span> | <span data-ttu-id="87dba-1677">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1677">Sh</span></span> | <span data-ttu-id="87dba-1678">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1678">Sh</span></span> | <span data-ttu-id="87dba-1679">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1679">In</span></span> | <span data-ttu-id="87dba-1680">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1680">In</span></span> | <span data-ttu-id="87dba-1681">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1681">Lo</span></span> | <span data-ttu-id="87dba-1682">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1682">Lo</span></span> | <span data-ttu-id="87dba-1683">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1683">Lo</span></span> | <span data-ttu-id="87dba-1684">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1684">Lo</span></span> | <span data-ttu-id="87dba-1685">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1685">Lo</span></span> | <span data-ttu-id="87dba-1686">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1686">Lo</span></span> | <span data-ttu-id="87dba-1687">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1687">Err</span></span> | <span data-ttu-id="87dba-1688">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1688">Err</span></span> | <span data-ttu-id="87dba-1689">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1689">Lo</span></span>  | <span data-ttu-id="87dba-1690">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1690">Ob</span></span>  | 
| <span data-ttu-id="87dba-1691">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-1691">__By__</span></span> |    |    | <span data-ttu-id="87dba-1692">um</span><span class="sxs-lookup"><span data-stu-id="87dba-1692">By</span></span> | <span data-ttu-id="87dba-1693">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1693">Sh</span></span> | <span data-ttu-id="87dba-1694">US</span><span class="sxs-lookup"><span data-stu-id="87dba-1694">US</span></span> | <span data-ttu-id="87dba-1695">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1695">In</span></span> | <span data-ttu-id="87dba-1696">UI</span><span class="sxs-lookup"><span data-stu-id="87dba-1696">UI</span></span> | <span data-ttu-id="87dba-1697">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1697">Lo</span></span> | <span data-ttu-id="87dba-1698">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-1698">UL</span></span> | <span data-ttu-id="87dba-1699">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1699">Lo</span></span> | <span data-ttu-id="87dba-1700">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1700">Lo</span></span> | <span data-ttu-id="87dba-1701">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1701">Lo</span></span> | <span data-ttu-id="87dba-1702">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1702">Err</span></span> | <span data-ttu-id="87dba-1703">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1703">Err</span></span> | <span data-ttu-id="87dba-1704">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1704">Lo</span></span>  | <span data-ttu-id="87dba-1705">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1705">Ob</span></span>  | 
| <span data-ttu-id="87dba-1706">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-1706">__Sh__</span></span> |    |    |    | <span data-ttu-id="87dba-1707">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1707">Sh</span></span> | <span data-ttu-id="87dba-1708">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1708">In</span></span> | <span data-ttu-id="87dba-1709">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1709">In</span></span> | <span data-ttu-id="87dba-1710">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1710">Lo</span></span> | <span data-ttu-id="87dba-1711">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1711">Lo</span></span> | <span data-ttu-id="87dba-1712">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1712">Lo</span></span> | <span data-ttu-id="87dba-1713">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1713">Lo</span></span> | <span data-ttu-id="87dba-1714">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1714">Lo</span></span> | <span data-ttu-id="87dba-1715">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1715">Lo</span></span> | <span data-ttu-id="87dba-1716">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1716">Err</span></span> | <span data-ttu-id="87dba-1717">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1717">Err</span></span> | <span data-ttu-id="87dba-1718">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1718">Lo</span></span>  | <span data-ttu-id="87dba-1719">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1719">Ob</span></span>  | 
| <span data-ttu-id="87dba-1720">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1720">__US__</span></span> |    |    |    |    | <span data-ttu-id="87dba-1721">US</span><span class="sxs-lookup"><span data-stu-id="87dba-1721">US</span></span> | <span data-ttu-id="87dba-1722">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1722">In</span></span> | <span data-ttu-id="87dba-1723">UI</span><span class="sxs-lookup"><span data-stu-id="87dba-1723">UI</span></span> | <span data-ttu-id="87dba-1724">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1724">Lo</span></span> | <span data-ttu-id="87dba-1725">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-1725">UL</span></span> | <span data-ttu-id="87dba-1726">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1726">Lo</span></span> | <span data-ttu-id="87dba-1727">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1727">Lo</span></span> | <span data-ttu-id="87dba-1728">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1728">Lo</span></span> | <span data-ttu-id="87dba-1729">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1729">Err</span></span> | <span data-ttu-id="87dba-1730">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1730">Err</span></span> | <span data-ttu-id="87dba-1731">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1731">Lo</span></span>  | <span data-ttu-id="87dba-1732">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1732">Ob</span></span>  | 
| <span data-ttu-id="87dba-1733">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-1733">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="87dba-1734">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1734">In</span></span> | <span data-ttu-id="87dba-1735">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1735">Lo</span></span> | <span data-ttu-id="87dba-1736">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1736">Lo</span></span> | <span data-ttu-id="87dba-1737">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1737">Lo</span></span> | <span data-ttu-id="87dba-1738">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1738">Lo</span></span> | <span data-ttu-id="87dba-1739">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1739">Lo</span></span> | <span data-ttu-id="87dba-1740">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1740">Lo</span></span> | <span data-ttu-id="87dba-1741">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1741">Err</span></span> | <span data-ttu-id="87dba-1742">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1742">Err</span></span> | <span data-ttu-id="87dba-1743">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1743">Lo</span></span>  | <span data-ttu-id="87dba-1744">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1744">Ob</span></span>  | 
| <span data-ttu-id="87dba-1745">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1745">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="87dba-1746">UI</span><span class="sxs-lookup"><span data-stu-id="87dba-1746">UI</span></span> | <span data-ttu-id="87dba-1747">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1747">Lo</span></span> | <span data-ttu-id="87dba-1748">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-1748">UL</span></span> | <span data-ttu-id="87dba-1749">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1749">Lo</span></span> | <span data-ttu-id="87dba-1750">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1750">Lo</span></span> | <span data-ttu-id="87dba-1751">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1751">Lo</span></span> | <span data-ttu-id="87dba-1752">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1752">Err</span></span> | <span data-ttu-id="87dba-1753">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1753">Err</span></span> | <span data-ttu-id="87dba-1754">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1754">Lo</span></span>  | <span data-ttu-id="87dba-1755">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1755">Ob</span></span>  | 
| <span data-ttu-id="87dba-1756">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-1756">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1757">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1757">Lo</span></span> | <span data-ttu-id="87dba-1758">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1758">Lo</span></span> | <span data-ttu-id="87dba-1759">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1759">Lo</span></span> | <span data-ttu-id="87dba-1760">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1760">Lo</span></span> | <span data-ttu-id="87dba-1761">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1761">Lo</span></span> | <span data-ttu-id="87dba-1762">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1762">Err</span></span> | <span data-ttu-id="87dba-1763">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1763">Err</span></span> | <span data-ttu-id="87dba-1764">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1764">Lo</span></span>  | <span data-ttu-id="87dba-1765">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1765">Ob</span></span>  | 
| <span data-ttu-id="87dba-1766">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1766">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1767">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-1767">UL</span></span> | <span data-ttu-id="87dba-1768">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1768">Lo</span></span> | <span data-ttu-id="87dba-1769">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1769">Lo</span></span> | <span data-ttu-id="87dba-1770">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1770">Lo</span></span> | <span data-ttu-id="87dba-1771">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1771">Err</span></span> | <span data-ttu-id="87dba-1772">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1772">Err</span></span> | <span data-ttu-id="87dba-1773">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1773">Lo</span></span>  | <span data-ttu-id="87dba-1774">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1774">Ob</span></span>  | 
| <span data-ttu-id="87dba-1775">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-1775">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1776">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1776">Lo</span></span> | <span data-ttu-id="87dba-1777">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1777">Lo</span></span> | <span data-ttu-id="87dba-1778">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1778">Lo</span></span> | <span data-ttu-id="87dba-1779">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1779">Err</span></span> | <span data-ttu-id="87dba-1780">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1780">Err</span></span> | <span data-ttu-id="87dba-1781">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1781">Lo</span></span>  | <span data-ttu-id="87dba-1782">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1782">Ob</span></span>  | 
| <span data-ttu-id="87dba-1783">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-1783">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1784">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1784">Lo</span></span> | <span data-ttu-id="87dba-1785">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1785">Lo</span></span> | <span data-ttu-id="87dba-1786">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1786">Err</span></span> | <span data-ttu-id="87dba-1787">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1787">Err</span></span> | <span data-ttu-id="87dba-1788">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1788">Lo</span></span>  | <span data-ttu-id="87dba-1789">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1789">Ob</span></span>  | 
| <span data-ttu-id="87dba-1790">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-1790">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1791">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1791">Lo</span></span> | <span data-ttu-id="87dba-1792">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1792">Err</span></span> | <span data-ttu-id="87dba-1793">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1793">Err</span></span> | <span data-ttu-id="87dba-1794">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1794">Lo</span></span>  | <span data-ttu-id="87dba-1795">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1795">Ob</span></span>  | 
| <span data-ttu-id="87dba-1796">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-1796">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1797">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1797">Err</span></span> | <span data-ttu-id="87dba-1798">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1798">Err</span></span> | <span data-ttu-id="87dba-1799">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1799">Err</span></span> | <span data-ttu-id="87dba-1800">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1800">Err</span></span> | 
| <span data-ttu-id="87dba-1801">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-1801">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="87dba-1802">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1802">Err</span></span> | <span data-ttu-id="87dba-1803">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1803">Err</span></span> | <span data-ttu-id="87dba-1804">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1804">Err</span></span> | 
| <span data-ttu-id="87dba-1805">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-1805">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="87dba-1806">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1806">Lo</span></span>  | <span data-ttu-id="87dba-1807">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1807">Ob</span></span>  | 
| <span data-ttu-id="87dba-1808">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-1808">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="87dba-1809">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1809">Ob</span></span>  | 


### <a name="mod-operator"></a><span data-ttu-id="87dba-1810">Operator Mod</span><span class="sxs-lookup"><span data-stu-id="87dba-1810">Mod Operator</span></span>

<span data-ttu-id="87dba-1811">Der `Mod` (Modulo)-Operator berechnet den Rest der Division zwischen zwei Operanden.</span><span class="sxs-lookup"><span data-stu-id="87dba-1811">The `Mod` (modulo) operator computes the remainder of the division between two operands.</span></span>

```antlr
ModuloOperatorExpression
    : Expression 'Mod' LineTerminator? Expression
    ;
```

<span data-ttu-id="87dba-1812">Der `Mod`-Operator ist für die folgenden Typen definiert:</span><span class="sxs-lookup"><span data-stu-id="87dba-1812">The `Mod` operator is defined for the following types:</span></span>

* <span data-ttu-id="87dba-1813">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` und `Long`.</span><span class="sxs-lookup"><span data-stu-id="87dba-1813">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` and `Long`.</span></span> <span data-ttu-id="87dba-1814">Das Ergebnis von `x Mod y` ist der Wert, der von `x - (x \ y) * y` erzeugt wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-1814">The result of `x Mod y` is the value produced by `x - (x \ y) * y`.</span></span> <span data-ttu-id="87dba-1815">Wenn `y` 0 (null) ist, wird eine Ausnahme vom Typ "`System.DivideByZeroException`" ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="87dba-1815">If `y` is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="87dba-1816">Der Modulo-Operator verursacht nie einen Überlauf.</span><span class="sxs-lookup"><span data-stu-id="87dba-1816">The modulo operator never causes an overflow.</span></span>

* <span data-ttu-id="87dba-1817">`Single` und `Double`.</span><span class="sxs-lookup"><span data-stu-id="87dba-1817">`Single` and `Double`.</span></span> <span data-ttu-id="87dba-1818">Der Rest wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.</span><span class="sxs-lookup"><span data-stu-id="87dba-1818">The remainder is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="87dba-1819">`Decimal`. installiert haben.</span><span class="sxs-lookup"><span data-stu-id="87dba-1819">`Decimal`.</span></span> <span data-ttu-id="87dba-1820">Wenn der Wert des rechten Operanden 0 (null) ist, wird eine Ausnahme vom Typ "`System.DivideByZeroException`" ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="87dba-1820">If the value of the right operand is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="87dba-1821">Wenn der resultierende Wert zu groß ist, um im Dezimal Format dargestellt zu werden, wird eine Ausnahme vom Typ "`System.OverflowException`" ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="87dba-1821">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="87dba-1822">Wenn der Ergebniswert zu klein ist, um im Dezimal Format darzustellen, ist das Ergebnis 0 (null).</span><span class="sxs-lookup"><span data-stu-id="87dba-1822">If the result value is too small to represent in the decimal format, the result is zero.</span></span>

<span data-ttu-id="87dba-1823">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="87dba-1823">__Operation Type:__</span></span>

|        | <span data-ttu-id="87dba-1824">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-1824">__Bo__</span></span> | <span data-ttu-id="87dba-1825">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-1825">__SB__</span></span> | <span data-ttu-id="87dba-1826">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-1826">__By__</span></span> | <span data-ttu-id="87dba-1827">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-1827">__Sh__</span></span> | <span data-ttu-id="87dba-1828">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1828">__US__</span></span> | <span data-ttu-id="87dba-1829">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-1829">__In__</span></span> | <span data-ttu-id="87dba-1830">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1830">__UI__</span></span> | <span data-ttu-id="87dba-1831">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-1831">__Lo__</span></span> | <span data-ttu-id="87dba-1832">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1832">__UL__</span></span> | <span data-ttu-id="87dba-1833">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-1833">__De__</span></span> | <span data-ttu-id="87dba-1834">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-1834">__Si__</span></span> | <span data-ttu-id="87dba-1835">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-1835">__Do__</span></span> | <span data-ttu-id="87dba-1836">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-1836">__Da__</span></span>  | <span data-ttu-id="87dba-1837">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-1837">__Ch__</span></span>  | <span data-ttu-id="87dba-1838">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-1838">__St__</span></span> | <span data-ttu-id="87dba-1839">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-1839">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="87dba-1840">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-1840">__Bo__</span></span> | <span data-ttu-id="87dba-1841">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1841">Sh</span></span> | <span data-ttu-id="87dba-1842">SB</span><span class="sxs-lookup"><span data-stu-id="87dba-1842">SB</span></span> | <span data-ttu-id="87dba-1843">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1843">Sh</span></span> | <span data-ttu-id="87dba-1844">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1844">Sh</span></span> | <span data-ttu-id="87dba-1845">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1845">In</span></span> | <span data-ttu-id="87dba-1846">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1846">In</span></span> | <span data-ttu-id="87dba-1847">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1847">Lo</span></span> | <span data-ttu-id="87dba-1848">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1848">Lo</span></span> | <span data-ttu-id="87dba-1849">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1849">De</span></span> | <span data-ttu-id="87dba-1850">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1850">De</span></span> | <span data-ttu-id="87dba-1851">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1851">Si</span></span> | <span data-ttu-id="87dba-1852">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1852">Do</span></span> | <span data-ttu-id="87dba-1853">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1853">Err</span></span> | <span data-ttu-id="87dba-1854">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1854">Err</span></span> | <span data-ttu-id="87dba-1855">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1855">Do</span></span>  | <span data-ttu-id="87dba-1856">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1856">Ob</span></span>  | 
| <span data-ttu-id="87dba-1857">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-1857">__SB__</span></span> |    | <span data-ttu-id="87dba-1858">SB</span><span class="sxs-lookup"><span data-stu-id="87dba-1858">SB</span></span> | <span data-ttu-id="87dba-1859">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1859">Sh</span></span> | <span data-ttu-id="87dba-1860">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1860">Sh</span></span> | <span data-ttu-id="87dba-1861">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1861">In</span></span> | <span data-ttu-id="87dba-1862">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1862">In</span></span> | <span data-ttu-id="87dba-1863">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1863">Lo</span></span> | <span data-ttu-id="87dba-1864">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1864">Lo</span></span> | <span data-ttu-id="87dba-1865">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1865">De</span></span> | <span data-ttu-id="87dba-1866">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1866">De</span></span> | <span data-ttu-id="87dba-1867">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1867">Si</span></span> | <span data-ttu-id="87dba-1868">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1868">Do</span></span> | <span data-ttu-id="87dba-1869">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1869">Err</span></span> | <span data-ttu-id="87dba-1870">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1870">Err</span></span> | <span data-ttu-id="87dba-1871">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1871">Do</span></span>  | <span data-ttu-id="87dba-1872">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1872">Ob</span></span>  | 
| <span data-ttu-id="87dba-1873">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-1873">__By__</span></span> |    |    | <span data-ttu-id="87dba-1874">um</span><span class="sxs-lookup"><span data-stu-id="87dba-1874">By</span></span> | <span data-ttu-id="87dba-1875">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1875">Sh</span></span> | <span data-ttu-id="87dba-1876">US</span><span class="sxs-lookup"><span data-stu-id="87dba-1876">US</span></span> | <span data-ttu-id="87dba-1877">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1877">In</span></span> | <span data-ttu-id="87dba-1878">UI</span><span class="sxs-lookup"><span data-stu-id="87dba-1878">UI</span></span> | <span data-ttu-id="87dba-1879">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1879">Lo</span></span> | <span data-ttu-id="87dba-1880">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-1880">UL</span></span> | <span data-ttu-id="87dba-1881">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1881">De</span></span> | <span data-ttu-id="87dba-1882">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1882">Si</span></span> | <span data-ttu-id="87dba-1883">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1883">Do</span></span> | <span data-ttu-id="87dba-1884">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1884">Err</span></span> | <span data-ttu-id="87dba-1885">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1885">Err</span></span> | <span data-ttu-id="87dba-1886">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1886">Do</span></span>  | <span data-ttu-id="87dba-1887">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1887">Ob</span></span>  | 
| <span data-ttu-id="87dba-1888">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-1888">__Sh__</span></span> |    |    |    | <span data-ttu-id="87dba-1889">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-1889">Sh</span></span> | <span data-ttu-id="87dba-1890">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1890">In</span></span> | <span data-ttu-id="87dba-1891">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1891">In</span></span> | <span data-ttu-id="87dba-1892">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1892">Lo</span></span> | <span data-ttu-id="87dba-1893">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1893">Lo</span></span> | <span data-ttu-id="87dba-1894">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1894">De</span></span> | <span data-ttu-id="87dba-1895">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1895">De</span></span> | <span data-ttu-id="87dba-1896">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1896">Si</span></span> | <span data-ttu-id="87dba-1897">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1897">Do</span></span> | <span data-ttu-id="87dba-1898">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1898">Err</span></span> | <span data-ttu-id="87dba-1899">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1899">Err</span></span> | <span data-ttu-id="87dba-1900">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1900">Do</span></span>  | <span data-ttu-id="87dba-1901">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1901">Ob</span></span>  | 
| <span data-ttu-id="87dba-1902">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1902">__US__</span></span> |    |    |    |    | <span data-ttu-id="87dba-1903">US</span><span class="sxs-lookup"><span data-stu-id="87dba-1903">US</span></span> | <span data-ttu-id="87dba-1904">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1904">In</span></span> | <span data-ttu-id="87dba-1905">UI</span><span class="sxs-lookup"><span data-stu-id="87dba-1905">UI</span></span> | <span data-ttu-id="87dba-1906">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1906">Lo</span></span> | <span data-ttu-id="87dba-1907">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-1907">UL</span></span> | <span data-ttu-id="87dba-1908">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1908">De</span></span> | <span data-ttu-id="87dba-1909">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1909">Si</span></span> | <span data-ttu-id="87dba-1910">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1910">Do</span></span> | <span data-ttu-id="87dba-1911">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1911">Err</span></span> | <span data-ttu-id="87dba-1912">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1912">Err</span></span> | <span data-ttu-id="87dba-1913">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1913">Do</span></span>  | <span data-ttu-id="87dba-1914">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1914">Ob</span></span>  | 
| <span data-ttu-id="87dba-1915">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-1915">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="87dba-1916">In</span><span class="sxs-lookup"><span data-stu-id="87dba-1916">In</span></span> | <span data-ttu-id="87dba-1917">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1917">Lo</span></span> | <span data-ttu-id="87dba-1918">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1918">Lo</span></span> | <span data-ttu-id="87dba-1919">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1919">De</span></span> | <span data-ttu-id="87dba-1920">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1920">De</span></span> | <span data-ttu-id="87dba-1921">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1921">Si</span></span> | <span data-ttu-id="87dba-1922">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1922">Do</span></span> | <span data-ttu-id="87dba-1923">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1923">Err</span></span> | <span data-ttu-id="87dba-1924">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1924">Err</span></span> | <span data-ttu-id="87dba-1925">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1925">Do</span></span>  | <span data-ttu-id="87dba-1926">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1926">Ob</span></span>  | 
| <span data-ttu-id="87dba-1927">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1927">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="87dba-1928">UI</span><span class="sxs-lookup"><span data-stu-id="87dba-1928">UI</span></span> | <span data-ttu-id="87dba-1929">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1929">Lo</span></span> | <span data-ttu-id="87dba-1930">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-1930">UL</span></span> | <span data-ttu-id="87dba-1931">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1931">De</span></span> | <span data-ttu-id="87dba-1932">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1932">Si</span></span> | <span data-ttu-id="87dba-1933">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1933">Do</span></span> | <span data-ttu-id="87dba-1934">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1934">Err</span></span> | <span data-ttu-id="87dba-1935">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1935">Err</span></span> | <span data-ttu-id="87dba-1936">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1936">Do</span></span>  | <span data-ttu-id="87dba-1937">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1937">Ob</span></span>  | 
| <span data-ttu-id="87dba-1938">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-1938">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1939">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-1939">Lo</span></span> | <span data-ttu-id="87dba-1940">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1940">De</span></span> | <span data-ttu-id="87dba-1941">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1941">De</span></span> | <span data-ttu-id="87dba-1942">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1942">Si</span></span> | <span data-ttu-id="87dba-1943">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1943">Do</span></span> | <span data-ttu-id="87dba-1944">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1944">Err</span></span> | <span data-ttu-id="87dba-1945">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1945">Err</span></span> | <span data-ttu-id="87dba-1946">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1946">Do</span></span>  | <span data-ttu-id="87dba-1947">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1947">Ob</span></span>  | 
| <span data-ttu-id="87dba-1948">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-1948">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1949">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-1949">UL</span></span> | <span data-ttu-id="87dba-1950">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1950">De</span></span> | <span data-ttu-id="87dba-1951">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1951">Si</span></span> | <span data-ttu-id="87dba-1952">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1952">Do</span></span> | <span data-ttu-id="87dba-1953">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1953">Err</span></span> | <span data-ttu-id="87dba-1954">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1954">Err</span></span> | <span data-ttu-id="87dba-1955">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1955">Do</span></span>  | <span data-ttu-id="87dba-1956">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1956">Ob</span></span>  | 
| <span data-ttu-id="87dba-1957">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-1957">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1958">De</span><span class="sxs-lookup"><span data-stu-id="87dba-1958">De</span></span> | <span data-ttu-id="87dba-1959">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1959">Si</span></span> | <span data-ttu-id="87dba-1960">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1960">Do</span></span> | <span data-ttu-id="87dba-1961">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1961">Err</span></span> | <span data-ttu-id="87dba-1962">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1962">Err</span></span> | <span data-ttu-id="87dba-1963">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1963">Do</span></span>  | <span data-ttu-id="87dba-1964">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1964">Ob</span></span>  | 
| <span data-ttu-id="87dba-1965">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-1965">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1966">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-1966">Si</span></span> | <span data-ttu-id="87dba-1967">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1967">Do</span></span> | <span data-ttu-id="87dba-1968">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1968">Err</span></span> | <span data-ttu-id="87dba-1969">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1969">Err</span></span> | <span data-ttu-id="87dba-1970">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1970">Do</span></span>  | <span data-ttu-id="87dba-1971">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1971">Ob</span></span>  | 
| <span data-ttu-id="87dba-1972">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-1972">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1973">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1973">Do</span></span> | <span data-ttu-id="87dba-1974">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1974">Err</span></span> | <span data-ttu-id="87dba-1975">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1975">Err</span></span> | <span data-ttu-id="87dba-1976">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1976">Do</span></span>  | <span data-ttu-id="87dba-1977">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1977">Ob</span></span>  | 
| <span data-ttu-id="87dba-1978">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-1978">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-1979">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1979">Err</span></span> | <span data-ttu-id="87dba-1980">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1980">Err</span></span> | <span data-ttu-id="87dba-1981">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1981">Err</span></span> | <span data-ttu-id="87dba-1982">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1982">Err</span></span> | 
| <span data-ttu-id="87dba-1983">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-1983">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="87dba-1984">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1984">Err</span></span> | <span data-ttu-id="87dba-1985">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1985">Err</span></span> | <span data-ttu-id="87dba-1986">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-1986">Err</span></span> | 
| <span data-ttu-id="87dba-1987">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-1987">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="87dba-1988">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-1988">Do</span></span>  | <span data-ttu-id="87dba-1989">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1989">Ob</span></span>  | 
| <span data-ttu-id="87dba-1990">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-1990">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="87dba-1991">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-1991">Ob</span></span>  | 


### <a name="exponentiation-operator"></a><span data-ttu-id="87dba-1992">Exponentialoperator</span><span class="sxs-lookup"><span data-stu-id="87dba-1992">Exponentiation Operator</span></span>

<span data-ttu-id="87dba-1993">Der Exponentialoperator berechnet den ersten Operanden, der in die Potenz des zweiten Operanden ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-1993">The exponentiation operator computes the first operand raised to the power of the second operand.</span></span>

```antlr
ExponentOperatorExpression
    : Expression '^' LineTerminator? Expression
    ;
```

<span data-ttu-id="87dba-1994">Der exponentiations Operator ist für den Typ `Double` definiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-1994">The exponentiation operator is defined for type `Double`.</span></span> <span data-ttu-id="87dba-1995">Der Wert wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.</span><span class="sxs-lookup"><span data-stu-id="87dba-1995">The value is computed according to the rules of IEEE 754 arithmetic.</span></span>

<span data-ttu-id="87dba-1996">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="87dba-1996">__Operation Type:__</span></span>

|        | <span data-ttu-id="87dba-1997">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-1997">__Bo__</span></span> | <span data-ttu-id="87dba-1998">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-1998">__SB__</span></span> | <span data-ttu-id="87dba-1999">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-1999">__By__</span></span> | <span data-ttu-id="87dba-2000">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-2000">__Sh__</span></span> | <span data-ttu-id="87dba-2001">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2001">__US__</span></span> | <span data-ttu-id="87dba-2002">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-2002">__In__</span></span> | <span data-ttu-id="87dba-2003">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2003">__UI__</span></span> | <span data-ttu-id="87dba-2004">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-2004">__Lo__</span></span> | <span data-ttu-id="87dba-2005">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2005">__UL__</span></span> | <span data-ttu-id="87dba-2006">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-2006">__De__</span></span> | <span data-ttu-id="87dba-2007">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-2007">__Si__</span></span> | <span data-ttu-id="87dba-2008">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-2008">__Do__</span></span> | <span data-ttu-id="87dba-2009">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-2009">__Da__</span></span>  | <span data-ttu-id="87dba-2010">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-2010">__Ch__</span></span>  | <span data-ttu-id="87dba-2011">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-2011">__St__</span></span> | <span data-ttu-id="87dba-2012">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-2012">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="87dba-2013">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-2013">__Bo__</span></span> | <span data-ttu-id="87dba-2014">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2014">Do</span></span> | <span data-ttu-id="87dba-2015">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2015">Do</span></span> | <span data-ttu-id="87dba-2016">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2016">Do</span></span> | <span data-ttu-id="87dba-2017">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2017">Do</span></span> | <span data-ttu-id="87dba-2018">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2018">Do</span></span> | <span data-ttu-id="87dba-2019">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2019">Do</span></span> | <span data-ttu-id="87dba-2020">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2020">Do</span></span> | <span data-ttu-id="87dba-2021">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2021">Do</span></span> | <span data-ttu-id="87dba-2022">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2022">Do</span></span> | <span data-ttu-id="87dba-2023">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2023">Do</span></span> | <span data-ttu-id="87dba-2024">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2024">Do</span></span> | <span data-ttu-id="87dba-2025">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2025">Do</span></span> | <span data-ttu-id="87dba-2026">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2026">Err</span></span> | <span data-ttu-id="87dba-2027">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2027">Err</span></span> | <span data-ttu-id="87dba-2028">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2028">Do</span></span>  | <span data-ttu-id="87dba-2029">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2029">Ob</span></span>  | 
| <span data-ttu-id="87dba-2030">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-2030">__SB__</span></span> |    | <span data-ttu-id="87dba-2031">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2031">Do</span></span> | <span data-ttu-id="87dba-2032">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2032">Do</span></span> | <span data-ttu-id="87dba-2033">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2033">Do</span></span> | <span data-ttu-id="87dba-2034">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2034">Do</span></span> | <span data-ttu-id="87dba-2035">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2035">Do</span></span> | <span data-ttu-id="87dba-2036">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2036">Do</span></span> | <span data-ttu-id="87dba-2037">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2037">Do</span></span> | <span data-ttu-id="87dba-2038">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2038">Do</span></span> | <span data-ttu-id="87dba-2039">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2039">Do</span></span> | <span data-ttu-id="87dba-2040">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2040">Do</span></span> | <span data-ttu-id="87dba-2041">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2041">Do</span></span> | <span data-ttu-id="87dba-2042">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2042">Err</span></span> | <span data-ttu-id="87dba-2043">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2043">Err</span></span> | <span data-ttu-id="87dba-2044">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2044">Do</span></span>  | <span data-ttu-id="87dba-2045">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2045">Ob</span></span>  | 
| <span data-ttu-id="87dba-2046">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-2046">__By__</span></span> |    |    | <span data-ttu-id="87dba-2047">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2047">Do</span></span> | <span data-ttu-id="87dba-2048">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2048">Do</span></span> | <span data-ttu-id="87dba-2049">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2049">Do</span></span> | <span data-ttu-id="87dba-2050">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2050">Do</span></span> | <span data-ttu-id="87dba-2051">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2051">Do</span></span> | <span data-ttu-id="87dba-2052">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2052">Do</span></span> | <span data-ttu-id="87dba-2053">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2053">Do</span></span> | <span data-ttu-id="87dba-2054">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2054">Do</span></span> | <span data-ttu-id="87dba-2055">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2055">Do</span></span> | <span data-ttu-id="87dba-2056">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2056">Do</span></span> | <span data-ttu-id="87dba-2057">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2057">Err</span></span> | <span data-ttu-id="87dba-2058">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2058">Err</span></span> | <span data-ttu-id="87dba-2059">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2059">Do</span></span>  | <span data-ttu-id="87dba-2060">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2060">Ob</span></span>  | 
| <span data-ttu-id="87dba-2061">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-2061">__Sh__</span></span> |    |    |    | <span data-ttu-id="87dba-2062">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2062">Do</span></span> | <span data-ttu-id="87dba-2063">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2063">Do</span></span> | <span data-ttu-id="87dba-2064">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2064">Do</span></span> | <span data-ttu-id="87dba-2065">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2065">Do</span></span> | <span data-ttu-id="87dba-2066">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2066">Do</span></span> | <span data-ttu-id="87dba-2067">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2067">Do</span></span> | <span data-ttu-id="87dba-2068">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2068">Do</span></span> | <span data-ttu-id="87dba-2069">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2069">Do</span></span> | <span data-ttu-id="87dba-2070">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2070">Do</span></span> | <span data-ttu-id="87dba-2071">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2071">Err</span></span> | <span data-ttu-id="87dba-2072">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2072">Err</span></span> | <span data-ttu-id="87dba-2073">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2073">Do</span></span>  | <span data-ttu-id="87dba-2074">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2074">Ob</span></span>  | 
| <span data-ttu-id="87dba-2075">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2075">__US__</span></span> |    |    |    |    | <span data-ttu-id="87dba-2076">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2076">Do</span></span> | <span data-ttu-id="87dba-2077">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2077">Do</span></span> | <span data-ttu-id="87dba-2078">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2078">Do</span></span> | <span data-ttu-id="87dba-2079">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2079">Do</span></span> | <span data-ttu-id="87dba-2080">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2080">Do</span></span> | <span data-ttu-id="87dba-2081">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2081">Do</span></span> | <span data-ttu-id="87dba-2082">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2082">Do</span></span> | <span data-ttu-id="87dba-2083">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2083">Do</span></span> | <span data-ttu-id="87dba-2084">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2084">Err</span></span> | <span data-ttu-id="87dba-2085">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2085">Err</span></span> | <span data-ttu-id="87dba-2086">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2086">Do</span></span>  | <span data-ttu-id="87dba-2087">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2087">Ob</span></span>  | 
| <span data-ttu-id="87dba-2088">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-2088">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="87dba-2089">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2089">Do</span></span> | <span data-ttu-id="87dba-2090">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2090">Do</span></span> | <span data-ttu-id="87dba-2091">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2091">Do</span></span> | <span data-ttu-id="87dba-2092">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2092">Do</span></span> | <span data-ttu-id="87dba-2093">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2093">Do</span></span> | <span data-ttu-id="87dba-2094">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2094">Do</span></span> | <span data-ttu-id="87dba-2095">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2095">Do</span></span> | <span data-ttu-id="87dba-2096">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2096">Err</span></span> | <span data-ttu-id="87dba-2097">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2097">Err</span></span> | <span data-ttu-id="87dba-2098">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2098">Do</span></span>  | <span data-ttu-id="87dba-2099">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2099">Ob</span></span>  | 
| <span data-ttu-id="87dba-2100">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2100">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="87dba-2101">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2101">Do</span></span> | <span data-ttu-id="87dba-2102">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2102">Do</span></span> | <span data-ttu-id="87dba-2103">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2103">Do</span></span> | <span data-ttu-id="87dba-2104">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2104">Do</span></span> | <span data-ttu-id="87dba-2105">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2105">Do</span></span> | <span data-ttu-id="87dba-2106">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2106">Do</span></span> | <span data-ttu-id="87dba-2107">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2107">Err</span></span> | <span data-ttu-id="87dba-2108">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2108">Err</span></span> | <span data-ttu-id="87dba-2109">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2109">Do</span></span>  | <span data-ttu-id="87dba-2110">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2110">Ob</span></span>  | 
| <span data-ttu-id="87dba-2111">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-2111">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2112">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2112">Do</span></span> | <span data-ttu-id="87dba-2113">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2113">Do</span></span> | <span data-ttu-id="87dba-2114">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2114">Do</span></span> | <span data-ttu-id="87dba-2115">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2115">Do</span></span> | <span data-ttu-id="87dba-2116">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2116">Do</span></span> | <span data-ttu-id="87dba-2117">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2117">Err</span></span> | <span data-ttu-id="87dba-2118">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2118">Err</span></span> | <span data-ttu-id="87dba-2119">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2119">Do</span></span>  | <span data-ttu-id="87dba-2120">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2120">Ob</span></span>  | 
| <span data-ttu-id="87dba-2121">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2121">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2122">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2122">Do</span></span> | <span data-ttu-id="87dba-2123">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2123">Do</span></span> | <span data-ttu-id="87dba-2124">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2124">Do</span></span> | <span data-ttu-id="87dba-2125">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2125">Do</span></span> | <span data-ttu-id="87dba-2126">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2126">Err</span></span> | <span data-ttu-id="87dba-2127">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2127">Err</span></span> | <span data-ttu-id="87dba-2128">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2128">Do</span></span>  | <span data-ttu-id="87dba-2129">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2129">Ob</span></span>  | 
| <span data-ttu-id="87dba-2130">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-2130">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2131">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2131">Do</span></span> | <span data-ttu-id="87dba-2132">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2132">Do</span></span> | <span data-ttu-id="87dba-2133">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2133">Do</span></span> | <span data-ttu-id="87dba-2134">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2134">Err</span></span> | <span data-ttu-id="87dba-2135">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2135">Err</span></span> | <span data-ttu-id="87dba-2136">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2136">Do</span></span>  | <span data-ttu-id="87dba-2137">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2137">Ob</span></span>  | 
| <span data-ttu-id="87dba-2138">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-2138">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2139">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2139">Do</span></span> | <span data-ttu-id="87dba-2140">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2140">Do</span></span> | <span data-ttu-id="87dba-2141">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2141">Err</span></span> | <span data-ttu-id="87dba-2142">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2142">Err</span></span> | <span data-ttu-id="87dba-2143">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2143">Do</span></span>  | <span data-ttu-id="87dba-2144">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2144">Ob</span></span>  | 
| <span data-ttu-id="87dba-2145">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-2145">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2146">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2146">Do</span></span> | <span data-ttu-id="87dba-2147">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2147">Err</span></span> | <span data-ttu-id="87dba-2148">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2148">Err</span></span> | <span data-ttu-id="87dba-2149">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2149">Do</span></span>  | <span data-ttu-id="87dba-2150">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2150">Ob</span></span>  | 
| <span data-ttu-id="87dba-2151">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-2151">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2152">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2152">Err</span></span> | <span data-ttu-id="87dba-2153">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2153">Err</span></span> | <span data-ttu-id="87dba-2154">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2154">Err</span></span> | <span data-ttu-id="87dba-2155">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2155">Err</span></span> | 
| <span data-ttu-id="87dba-2156">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-2156">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="87dba-2157">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2157">Err</span></span> | <span data-ttu-id="87dba-2158">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2158">Err</span></span> | <span data-ttu-id="87dba-2159">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2159">Err</span></span> | 
| <span data-ttu-id="87dba-2160">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-2160">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="87dba-2161">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2161">Do</span></span>  | <span data-ttu-id="87dba-2162">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2162">Ob</span></span>  | 
| <span data-ttu-id="87dba-2163">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-2163">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="87dba-2164">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2164">Ob</span></span>  | 


## <a name="relational-operators"></a><span data-ttu-id="87dba-2165">Relationale Operatoren</span><span class="sxs-lookup"><span data-stu-id="87dba-2165">Relational Operators</span></span>

<span data-ttu-id="87dba-2166">Die *relationalen Operatoren* vergleichen Werte miteinander.</span><span class="sxs-lookup"><span data-stu-id="87dba-2166">The *relational operators* compare values to one other.</span></span> <span data-ttu-id="87dba-2167">Die Vergleichs Operatoren sind `=`, `<>`, `<`, `>`, `<=` und `>=`.</span><span class="sxs-lookup"><span data-stu-id="87dba-2167">The comparison operators are `=`, `<>`, `<`, `>`, `<=`, and `>=`.</span></span>

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

<span data-ttu-id="87dba-2168">Alle relationalen Operatoren führen zu einem `Boolean`-Wert.</span><span class="sxs-lookup"><span data-stu-id="87dba-2168">All of the relational operators result in a `Boolean` value.</span></span>

<span data-ttu-id="87dba-2169">Die relationalen Operatoren haben die folgende allgemeine Bedeutung:</span><span class="sxs-lookup"><span data-stu-id="87dba-2169">The relational operators have the following general meaning:</span></span>

* <span data-ttu-id="87dba-2170">Der `=`-Operator testet, ob die beiden Operanden gleich sind.</span><span class="sxs-lookup"><span data-stu-id="87dba-2170">The `=` operator tests whether the two operands are equal.</span></span>

* <span data-ttu-id="87dba-2171">Der `<>`-Operator testet, ob die beiden Operanden nicht gleich sind.</span><span class="sxs-lookup"><span data-stu-id="87dba-2171">The `<>` operator tests whether the two operands are not equal.</span></span>

* <span data-ttu-id="87dba-2172">Der `<`-Operator testet, ob der erste Operand kleiner als der zweite Operand ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-2172">The `<` operator tests whether the first operand is less than the second operand.</span></span>

* <span data-ttu-id="87dba-2173">Der `>`-Operator testet, ob der erste Operand größer als der zweite Operand ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-2173">The `>` operator tests whether the first operand is greater than the second operand.</span></span>

* <span data-ttu-id="87dba-2174">Der `<=`-Operator testet, ob der erste Operand kleiner als oder gleich dem zweiten Operand ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-2174">The `<=` operator tests whether the first operand is less than or equal to the second operand.</span></span>

* <span data-ttu-id="87dba-2175">Der `>=`-Operator testet, ob der erste Operand größer oder gleich dem zweiten Operand ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-2175">The `>=` operator tests whether the first operand is greater than or equal to the second operand.</span></span>

<span data-ttu-id="87dba-2176">Die relationalen Operatoren sind für die folgenden Typen definiert:</span><span class="sxs-lookup"><span data-stu-id="87dba-2176">The relational operators are defined for the following types:</span></span>

* <span data-ttu-id="87dba-2177">`Boolean`. installiert haben.</span><span class="sxs-lookup"><span data-stu-id="87dba-2177">`Boolean`.</span></span> <span data-ttu-id="87dba-2178">Die Operatoren vergleichen die Wahrheits Werte der beiden Operanden.</span><span class="sxs-lookup"><span data-stu-id="87dba-2178">The operators compare the truth values of the two operands.</span></span> <span data-ttu-id="87dba-2179">`True` gilt als kleiner als `False`, was mit den numerischen Werten übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="87dba-2179">`True` is considered to be less than `False`, which matches with their numeric values.</span></span>

* <span data-ttu-id="87dba-2180">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` und `Long`.</span><span class="sxs-lookup"><span data-stu-id="87dba-2180">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="87dba-2181">Die Operatoren vergleichen die numerischen Werte der beiden ganzzahligen Operanden.</span><span class="sxs-lookup"><span data-stu-id="87dba-2181">The operators compare the numeric values of the two integral operands.</span></span>

* <span data-ttu-id="87dba-2182">`Single` und `Double`.</span><span class="sxs-lookup"><span data-stu-id="87dba-2182">`Single` and `Double`.</span></span> <span data-ttu-id="87dba-2183">Die Operanden vergleichen die Operanden gemäß den Regeln des IEEE 754-Standards.</span><span class="sxs-lookup"><span data-stu-id="87dba-2183">The operators compare the operands according to the rules of the IEEE 754 standard.</span></span>

* <span data-ttu-id="87dba-2184">`Decimal`. installiert haben.</span><span class="sxs-lookup"><span data-stu-id="87dba-2184">`Decimal`.</span></span> <span data-ttu-id="87dba-2185">Die Operatoren vergleichen die numerischen Werte der zwei Dezimal Operanden.</span><span class="sxs-lookup"><span data-stu-id="87dba-2185">The operators compare the numeric values of the two decimal operands.</span></span>

* <span data-ttu-id="87dba-2186">`Date`. installiert haben.</span><span class="sxs-lookup"><span data-stu-id="87dba-2186">`Date`.</span></span> <span data-ttu-id="87dba-2187">Die Operatoren geben das Ergebnis des Vergleichs der beiden Datums-/Uhrzeitwerte zurück.</span><span class="sxs-lookup"><span data-stu-id="87dba-2187">The operators return the result of comparing the two date/time values.</span></span>

* <span data-ttu-id="87dba-2188">`Char`. installiert haben.</span><span class="sxs-lookup"><span data-stu-id="87dba-2188">`Char`.</span></span> <span data-ttu-id="87dba-2189">Die Operatoren geben das Ergebnis des Vergleichs der beiden Unicode-Werte zurück.</span><span class="sxs-lookup"><span data-stu-id="87dba-2189">The operators return the result of comparing the two Unicode values.</span></span>

* <span data-ttu-id="87dba-2190">`String`. installiert haben.</span><span class="sxs-lookup"><span data-stu-id="87dba-2190">`String`.</span></span> <span data-ttu-id="87dba-2191">Die Operatoren geben das Ergebnis des Vergleichs der beiden Werte entweder mithilfe eines binären Vergleichs oder eines Text Vergleichs zurück.</span><span class="sxs-lookup"><span data-stu-id="87dba-2191">The operators return the result of comparing the two values using either a binary comparison or a text comparison.</span></span> <span data-ttu-id="87dba-2192">Der verwendete Vergleich wird von der Kompilierungs Umgebung und der `Option Compare`-Anweisung bestimmt.</span><span class="sxs-lookup"><span data-stu-id="87dba-2192">The comparison used is determined by the compilation environment and the `Option Compare` statement.</span></span> <span data-ttu-id="87dba-2193">Ein binärer Vergleich bestimmt, ob der numerische Unicode-Wert jedes Zeichens in jeder Zeichenfolge identisch ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-2193">A binary comparison determines whether the numeric Unicode value of each character in each string is the same.</span></span> <span data-ttu-id="87dba-2194">Ein Textvergleich führt einen Unicode-Textvergleich basierend auf der aktuellen Kultur aus, die für die .NET Framework verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-2194">A text comparison does a Unicode text comparison based on the current culture in use on the .NET Framework.</span></span> <span data-ttu-id="87dba-2195">Wenn Sie einen Zeichen folgen Vergleich durchgeführt haben, entspricht ein NULL-Wert dem Zeichenfolgenliteralwert `""`.</span><span class="sxs-lookup"><span data-stu-id="87dba-2195">When doing a string comparison, a null value is equivalent to the string literal `""`.</span></span>

<span data-ttu-id="87dba-2196">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="87dba-2196">__Operation Type:__</span></span>
        
|        | <span data-ttu-id="87dba-2197">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-2197">__Bo__</span></span> | <span data-ttu-id="87dba-2198">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-2198">__SB__</span></span> | <span data-ttu-id="87dba-2199">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-2199">__By__</span></span> | <span data-ttu-id="87dba-2200">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-2200">__Sh__</span></span> | <span data-ttu-id="87dba-2201">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2201">__US__</span></span> | <span data-ttu-id="87dba-2202">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-2202">__In__</span></span> | <span data-ttu-id="87dba-2203">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2203">__UI__</span></span> | <span data-ttu-id="87dba-2204">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-2204">__Lo__</span></span> | <span data-ttu-id="87dba-2205">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2205">__UL__</span></span> | <span data-ttu-id="87dba-2206">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-2206">__De__</span></span> | <span data-ttu-id="87dba-2207">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-2207">__Si__</span></span> | <span data-ttu-id="87dba-2208">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-2208">__Do__</span></span> | <span data-ttu-id="87dba-2209">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-2209">__Da__</span></span>  | <span data-ttu-id="87dba-2210">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-2210">__Ch__</span></span>  | <span data-ttu-id="87dba-2211">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-2211">__St__</span></span> | <span data-ttu-id="87dba-2212">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-2212">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="87dba-2213">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-2213">__Bo__</span></span> | <span data-ttu-id="87dba-2214">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-2214">Bo</span></span> | <span data-ttu-id="87dba-2215">SB</span><span class="sxs-lookup"><span data-stu-id="87dba-2215">SB</span></span> | <span data-ttu-id="87dba-2216">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-2216">Sh</span></span> | <span data-ttu-id="87dba-2217">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-2217">Sh</span></span> | <span data-ttu-id="87dba-2218">In</span><span class="sxs-lookup"><span data-stu-id="87dba-2218">In</span></span> | <span data-ttu-id="87dba-2219">In</span><span class="sxs-lookup"><span data-stu-id="87dba-2219">In</span></span> | <span data-ttu-id="87dba-2220">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2220">Lo</span></span> | <span data-ttu-id="87dba-2221">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2221">Lo</span></span> | <span data-ttu-id="87dba-2222">De</span><span class="sxs-lookup"><span data-stu-id="87dba-2222">De</span></span> | <span data-ttu-id="87dba-2223">De</span><span class="sxs-lookup"><span data-stu-id="87dba-2223">De</span></span> | <span data-ttu-id="87dba-2224">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-2224">Si</span></span> | <span data-ttu-id="87dba-2225">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2225">Do</span></span> | <span data-ttu-id="87dba-2226">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2226">Err</span></span> | <span data-ttu-id="87dba-2227">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2227">Err</span></span> | <span data-ttu-id="87dba-2228">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-2228">Bo</span></span> | <span data-ttu-id="87dba-2229">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2229">Ob</span></span> | 
| <span data-ttu-id="87dba-2230">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-2230">__SB__</span></span> |    | <span data-ttu-id="87dba-2231">SB</span><span class="sxs-lookup"><span data-stu-id="87dba-2231">SB</span></span> | <span data-ttu-id="87dba-2232">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-2232">Sh</span></span> | <span data-ttu-id="87dba-2233">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-2233">Sh</span></span> | <span data-ttu-id="87dba-2234">In</span><span class="sxs-lookup"><span data-stu-id="87dba-2234">In</span></span> | <span data-ttu-id="87dba-2235">In</span><span class="sxs-lookup"><span data-stu-id="87dba-2235">In</span></span> | <span data-ttu-id="87dba-2236">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2236">Lo</span></span> | <span data-ttu-id="87dba-2237">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2237">Lo</span></span> | <span data-ttu-id="87dba-2238">De</span><span class="sxs-lookup"><span data-stu-id="87dba-2238">De</span></span> | <span data-ttu-id="87dba-2239">De</span><span class="sxs-lookup"><span data-stu-id="87dba-2239">De</span></span> | <span data-ttu-id="87dba-2240">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-2240">Si</span></span> | <span data-ttu-id="87dba-2241">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2241">Do</span></span> | <span data-ttu-id="87dba-2242">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2242">Err</span></span> | <span data-ttu-id="87dba-2243">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2243">Err</span></span> | <span data-ttu-id="87dba-2244">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2244">Do</span></span> | <span data-ttu-id="87dba-2245">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2245">Ob</span></span> | 
| <span data-ttu-id="87dba-2246">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-2246">__By__</span></span> |    |    | <span data-ttu-id="87dba-2247">um</span><span class="sxs-lookup"><span data-stu-id="87dba-2247">By</span></span> | <span data-ttu-id="87dba-2248">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-2248">Sh</span></span> | <span data-ttu-id="87dba-2249">US</span><span class="sxs-lookup"><span data-stu-id="87dba-2249">US</span></span> | <span data-ttu-id="87dba-2250">In</span><span class="sxs-lookup"><span data-stu-id="87dba-2250">In</span></span> | <span data-ttu-id="87dba-2251">UI</span><span class="sxs-lookup"><span data-stu-id="87dba-2251">UI</span></span> | <span data-ttu-id="87dba-2252">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2252">Lo</span></span> | <span data-ttu-id="87dba-2253">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-2253">UL</span></span> | <span data-ttu-id="87dba-2254">De</span><span class="sxs-lookup"><span data-stu-id="87dba-2254">De</span></span> | <span data-ttu-id="87dba-2255">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-2255">Si</span></span> | <span data-ttu-id="87dba-2256">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2256">Do</span></span> | <span data-ttu-id="87dba-2257">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2257">Err</span></span> | <span data-ttu-id="87dba-2258">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2258">Err</span></span> | <span data-ttu-id="87dba-2259">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2259">Do</span></span> | <span data-ttu-id="87dba-2260">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2260">Ob</span></span> | 
| <span data-ttu-id="87dba-2261">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-2261">__Sh__</span></span> |    |    |    | <span data-ttu-id="87dba-2262">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-2262">Sh</span></span> | <span data-ttu-id="87dba-2263">In</span><span class="sxs-lookup"><span data-stu-id="87dba-2263">In</span></span> | <span data-ttu-id="87dba-2264">In</span><span class="sxs-lookup"><span data-stu-id="87dba-2264">In</span></span> | <span data-ttu-id="87dba-2265">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2265">Lo</span></span> | <span data-ttu-id="87dba-2266">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2266">Lo</span></span> | <span data-ttu-id="87dba-2267">De</span><span class="sxs-lookup"><span data-stu-id="87dba-2267">De</span></span> | <span data-ttu-id="87dba-2268">De</span><span class="sxs-lookup"><span data-stu-id="87dba-2268">De</span></span> | <span data-ttu-id="87dba-2269">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-2269">Si</span></span> | <span data-ttu-id="87dba-2270">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2270">Do</span></span> | <span data-ttu-id="87dba-2271">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2271">Err</span></span> | <span data-ttu-id="87dba-2272">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2272">Err</span></span> | <span data-ttu-id="87dba-2273">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2273">Do</span></span> | <span data-ttu-id="87dba-2274">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2274">Ob</span></span> | 
| <span data-ttu-id="87dba-2275">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2275">__US__</span></span> |    |    |    |    | <span data-ttu-id="87dba-2276">US</span><span class="sxs-lookup"><span data-stu-id="87dba-2276">US</span></span> | <span data-ttu-id="87dba-2277">In</span><span class="sxs-lookup"><span data-stu-id="87dba-2277">In</span></span> | <span data-ttu-id="87dba-2278">UI</span><span class="sxs-lookup"><span data-stu-id="87dba-2278">UI</span></span> | <span data-ttu-id="87dba-2279">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2279">Lo</span></span> | <span data-ttu-id="87dba-2280">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-2280">UL</span></span> | <span data-ttu-id="87dba-2281">De</span><span class="sxs-lookup"><span data-stu-id="87dba-2281">De</span></span> | <span data-ttu-id="87dba-2282">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-2282">Si</span></span> | <span data-ttu-id="87dba-2283">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2283">Do</span></span> | <span data-ttu-id="87dba-2284">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2284">Err</span></span> | <span data-ttu-id="87dba-2285">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2285">Err</span></span> | <span data-ttu-id="87dba-2286">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2286">Do</span></span> | <span data-ttu-id="87dba-2287">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2287">Ob</span></span> | 
| <span data-ttu-id="87dba-2288">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-2288">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="87dba-2289">In</span><span class="sxs-lookup"><span data-stu-id="87dba-2289">In</span></span> | <span data-ttu-id="87dba-2290">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2290">Lo</span></span> | <span data-ttu-id="87dba-2291">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2291">Lo</span></span> | <span data-ttu-id="87dba-2292">De</span><span class="sxs-lookup"><span data-stu-id="87dba-2292">De</span></span> | <span data-ttu-id="87dba-2293">De</span><span class="sxs-lookup"><span data-stu-id="87dba-2293">De</span></span> | <span data-ttu-id="87dba-2294">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-2294">Si</span></span> | <span data-ttu-id="87dba-2295">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2295">Do</span></span> | <span data-ttu-id="87dba-2296">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2296">Err</span></span> | <span data-ttu-id="87dba-2297">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2297">Err</span></span> | <span data-ttu-id="87dba-2298">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2298">Do</span></span> | <span data-ttu-id="87dba-2299">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2299">Ob</span></span> | 
| <span data-ttu-id="87dba-2300">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2300">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="87dba-2301">UI</span><span class="sxs-lookup"><span data-stu-id="87dba-2301">UI</span></span> | <span data-ttu-id="87dba-2302">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2302">Lo</span></span> | <span data-ttu-id="87dba-2303">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-2303">UL</span></span> | <span data-ttu-id="87dba-2304">De</span><span class="sxs-lookup"><span data-stu-id="87dba-2304">De</span></span> | <span data-ttu-id="87dba-2305">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-2305">Si</span></span> | <span data-ttu-id="87dba-2306">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2306">Do</span></span> | <span data-ttu-id="87dba-2307">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2307">Err</span></span> | <span data-ttu-id="87dba-2308">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2308">Err</span></span> | <span data-ttu-id="87dba-2309">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2309">Do</span></span> | <span data-ttu-id="87dba-2310">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2310">Ob</span></span> | 
| <span data-ttu-id="87dba-2311">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-2311">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2312">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2312">Lo</span></span> | <span data-ttu-id="87dba-2313">De</span><span class="sxs-lookup"><span data-stu-id="87dba-2313">De</span></span> | <span data-ttu-id="87dba-2314">De</span><span class="sxs-lookup"><span data-stu-id="87dba-2314">De</span></span> | <span data-ttu-id="87dba-2315">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-2315">Si</span></span> | <span data-ttu-id="87dba-2316">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2316">Do</span></span> | <span data-ttu-id="87dba-2317">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2317">Err</span></span> | <span data-ttu-id="87dba-2318">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2318">Err</span></span> | <span data-ttu-id="87dba-2319">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2319">Do</span></span> | <span data-ttu-id="87dba-2320">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2320">Ob</span></span> | 
| <span data-ttu-id="87dba-2321">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2321">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2322">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-2322">UL</span></span> | <span data-ttu-id="87dba-2323">De</span><span class="sxs-lookup"><span data-stu-id="87dba-2323">De</span></span> | <span data-ttu-id="87dba-2324">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-2324">Si</span></span> | <span data-ttu-id="87dba-2325">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2325">Do</span></span> | <span data-ttu-id="87dba-2326">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2326">Err</span></span> | <span data-ttu-id="87dba-2327">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2327">Err</span></span> | <span data-ttu-id="87dba-2328">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2328">Do</span></span> | <span data-ttu-id="87dba-2329">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2329">Ob</span></span> | 
| <span data-ttu-id="87dba-2330">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-2330">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2331">De</span><span class="sxs-lookup"><span data-stu-id="87dba-2331">De</span></span> | <span data-ttu-id="87dba-2332">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-2332">Si</span></span> | <span data-ttu-id="87dba-2333">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2333">Do</span></span> | <span data-ttu-id="87dba-2334">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2334">Err</span></span> | <span data-ttu-id="87dba-2335">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2335">Err</span></span> | <span data-ttu-id="87dba-2336">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2336">Do</span></span> | <span data-ttu-id="87dba-2337">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2337">Ob</span></span> | 
| <span data-ttu-id="87dba-2338">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-2338">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2339">Si</span><span class="sxs-lookup"><span data-stu-id="87dba-2339">Si</span></span> | <span data-ttu-id="87dba-2340">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2340">Do</span></span> | <span data-ttu-id="87dba-2341">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2341">Err</span></span> | <span data-ttu-id="87dba-2342">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2342">Err</span></span> | <span data-ttu-id="87dba-2343">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2343">Do</span></span> | <span data-ttu-id="87dba-2344">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2344">Ob</span></span> | 
| <span data-ttu-id="87dba-2345">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-2345">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2346">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2346">Do</span></span> | <span data-ttu-id="87dba-2347">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2347">Err</span></span> | <span data-ttu-id="87dba-2348">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2348">Err</span></span> | <span data-ttu-id="87dba-2349">Do</span><span class="sxs-lookup"><span data-stu-id="87dba-2349">Do</span></span> | <span data-ttu-id="87dba-2350">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2350">Ob</span></span> | 
| <span data-ttu-id="87dba-2351">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-2351">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2352">De</span><span class="sxs-lookup"><span data-stu-id="87dba-2352">Da</span></span>  | <span data-ttu-id="87dba-2353">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2353">Err</span></span> | <span data-ttu-id="87dba-2354">De</span><span class="sxs-lookup"><span data-stu-id="87dba-2354">Da</span></span> | <span data-ttu-id="87dba-2355">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2355">Ob</span></span> | 
| <span data-ttu-id="87dba-2356">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-2356">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="87dba-2357">Ch</span><span class="sxs-lookup"><span data-stu-id="87dba-2357">Ch</span></span>  | <span data-ttu-id="87dba-2358">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2358">St</span></span> | <span data-ttu-id="87dba-2359">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2359">Ob</span></span> | 
| <span data-ttu-id="87dba-2360">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-2360">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="87dba-2361">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2361">St</span></span> | <span data-ttu-id="87dba-2362">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2362">Ob</span></span> | 
| <span data-ttu-id="87dba-2363">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-2363">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | <span data-ttu-id="87dba-2364">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2364">Ob</span></span> | 


## <a name="like-operator"></a><span data-ttu-id="87dba-2365">Like-Operator</span><span class="sxs-lookup"><span data-stu-id="87dba-2365">Like Operator</span></span>

<span data-ttu-id="87dba-2366">Der `Like`-Operator bestimmt, ob eine Zeichenfolge mit einem angegebenen Muster übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="87dba-2366">The `Like` operator determines whether a string matches a given pattern.</span></span>

```antlr
LikeOperatorExpression
    : Expression 'Like' LineTerminator? Expression
    ;
```

<span data-ttu-id="87dba-2367">Der `Like`-Operator ist für den `String`-Typ definiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-2367">The `Like` operator is defined for the `String` type.</span></span> <span data-ttu-id="87dba-2368">Der erste Operand ist die übereinstimmende Zeichenfolge, und der zweite Operand ist das Muster für die Übereinstimmung.</span><span class="sxs-lookup"><span data-stu-id="87dba-2368">The first operand is the string being matched, and the second operand is the pattern to match against.</span></span> <span data-ttu-id="87dba-2369">Das Muster besteht aus Unicode-Zeichen.</span><span class="sxs-lookup"><span data-stu-id="87dba-2369">The pattern is made up of Unicode characters.</span></span> <span data-ttu-id="87dba-2370">Die folgenden Zeichen folgen haben eine besondere Bedeutung:</span><span class="sxs-lookup"><span data-stu-id="87dba-2370">The following character sequences have special meanings:</span></span>

* <span data-ttu-id="87dba-2371">Das Zeichen `?` entspricht einem beliebigen einzelnen Zeichen.</span><span class="sxs-lookup"><span data-stu-id="87dba-2371">The character `?` matches any single character.</span></span>

* <span data-ttu-id="87dba-2372">Das Zeichen `*` entspricht 0 (null) oder mehr Zeichen.</span><span class="sxs-lookup"><span data-stu-id="87dba-2372">The character `*` matches zero or more characters.</span></span>

* <span data-ttu-id="87dba-2373">Das Zeichen `#` entspricht einer beliebigen einzelnen Ziffer (0-9).</span><span class="sxs-lookup"><span data-stu-id="87dba-2373">The character `#` matches any single digit (0-9).</span></span>

* <span data-ttu-id="87dba-2374">Eine Liste von Zeichen, die von eckigen Klammern (`[ab...]`) umgeben sind, entspricht einem beliebigen einzelnen Zeichen in der Liste.</span><span class="sxs-lookup"><span data-stu-id="87dba-2374">A list of characters surrounded by brackets (`[ab...]`) matches any single character in the list.</span></span>

* <span data-ttu-id="87dba-2375">Eine Liste von Zeichen, die von eckigen Klammern eingeschlossen und mit vorangestelltem Ausrufezeichen (`[!ab...]`) vorangestellt werden, stimmt mit jedem beliebigen Zeichen in der Zeichen Liste überein.</span><span class="sxs-lookup"><span data-stu-id="87dba-2375">A list of characters surrounded by brackets and prefixed by an exclamation point (`[!ab...]`) matches any single character not in the character list.</span></span>

* <span data-ttu-id="87dba-2376">Zwei Zeichen in einer Zeichen Liste, getrennt durch einen Bindestrich (`-`), geben einen Bereich von Unicode-Zeichen an, beginnend mit dem ersten Zeichen und endende mit dem zweiten Zeichen.</span><span class="sxs-lookup"><span data-stu-id="87dba-2376">Two characters in a character list separated by a hyphen (`-`) specify a range of Unicode characters starting with the first character and ending with the second character.</span></span> <span data-ttu-id="87dba-2377">Wenn das zweite Zeichen nicht später in der Sortierreihenfolge als das erste Zeichen ist, tritt eine Lauf Zeit Ausnahme auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-2377">If the second character is not later in the sort order than the first character, a run-time exception occurs.</span></span> <span data-ttu-id="87dba-2378">Ein Bindestrich, der am Anfang oder Ende einer Zeichen Liste angezeigt wird, gibt sich selbst an.</span><span class="sxs-lookup"><span data-stu-id="87dba-2378">A hyphen that appears at the beginning or end of a character list specifies itself.</span></span>

<span data-ttu-id="87dba-2379">Zum Abgleichen der Sonderzeichen Left eckige Klammer (`[`), Fragezeichen (`?`), Nummern Zeichen (`#`) und Sternchen (`*`) müssen eckige Klammern diese einschließen.</span><span class="sxs-lookup"><span data-stu-id="87dba-2379">To match the special characters left bracket (`[`), question mark (`?`), number sign (`#`), and asterisk (`*`), brackets must enclose them.</span></span> <span data-ttu-id="87dba-2380">Die rechte eckige Klammer (`]`) kann nicht innerhalb einer Gruppe verwendet werden, um sich selbst abzugleichen, Sie kann jedoch außerhalb einer Gruppe als einzelnes Zeichen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-2380">The right bracket (`]`) cannot be used within a group to match itself, but it can be used outside a group as an individual character.</span></span> <span data-ttu-id="87dba-2381">Die Zeichenfolge `[]` wird als Zeichenfolgenliteralzeichen `""` betrachtet.</span><span class="sxs-lookup"><span data-stu-id="87dba-2381">The character sequence `[]` is considered to be the string literal `""`.</span></span> 

<span data-ttu-id="87dba-2382">Beachten Sie, dass die Zeichen Vergleiche und die Reihenfolge von Zeichen Listen vom Typ der verwendeten Vergleiche abhängig sind.</span><span class="sxs-lookup"><span data-stu-id="87dba-2382">Note that character comparisons and ordering for character lists are dependent on the type of comparisons being used.</span></span> <span data-ttu-id="87dba-2383">Wenn binäre Vergleiche verwendet werden, basieren die Zeichen Vergleiche und die Reihenfolge auf den numerischen Unicode-Werten.</span><span class="sxs-lookup"><span data-stu-id="87dba-2383">If binary comparisons are being used, character comparisons and ordering are based on the numeric Unicode values.</span></span> <span data-ttu-id="87dba-2384">Bei Verwendung von Text vergleichen basieren die Zeichen Vergleiche und die Reihenfolge auf dem aktuellen Gebiets Schema, das auf dem .NET Framework verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-2384">If text comparisons are being used, character comparisons and ordering are based on the current locale being used on the .NET Framework.</span></span>

<span data-ttu-id="87dba-2385">In einigen Sprachen stellen Sonderzeichen im Alphabet zwei separate Zeichen und umgekehrt dar.</span><span class="sxs-lookup"><span data-stu-id="87dba-2385">In some languages, special characters in the alphabet represent two separate characters and vice versa.</span></span> <span data-ttu-id="87dba-2386">Beispielsweise verwenden mehrere Sprachen das Zeichen `æ`, um die Zeichen `a` und `e` darzustellen, wenn Sie einander vorkommen, während die Zeichen `^` und `O` zur Darstellung des Zeichens `Ô` verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="87dba-2386">For example, several languages use the character `æ` to represent the characters `a` and `e` when they appear together, while the characters `^` and `O` can be used to represent the character `Ô`.</span></span> <span data-ttu-id="87dba-2387">Bei der Verwendung von Text vergleichen erkennt der `Like`-Operator solche kulturellen äquivalente.</span><span class="sxs-lookup"><span data-stu-id="87dba-2387">When using text comparisons, the `Like` operator recognizes such cultural equivalences.</span></span> <span data-ttu-id="87dba-2388">In diesem Fall entspricht das Vorkommen des einzelnen Sonderzeichens in pattern oder String der entsprechenden zweistelligen Sequenz in der anderen Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="87dba-2388">In that case, an occurrence of the single special character in either pattern or string matches the equivalent two-character sequence in the other string.</span></span> <span data-ttu-id="87dba-2389">Ebenso entspricht ein einzelnes Sonderzeichen in einem Muster, das in eckige Klammern eingeschlossen ist (selbst, in einer Liste oder in einem Bereich), mit der entsprechenden zwei Zeichenfolge in der Zeichenfolge und umgekehrt.</span><span class="sxs-lookup"><span data-stu-id="87dba-2389">Similarly, a single special character in pattern enclosed in brackets (by itself, in a list, or in a range) matches the equivalent two-character sequence in the string and vice versa.</span></span>

<span data-ttu-id="87dba-2390">In einem `Like`-Ausdruck, bei dem beide Operanden `Nothing` oder ein Operand eine intrinsische Konvertierung in `String` und der andere Operand `Nothing` sind, wird `Nothing` so behandelt, als ob es sich um das leere Zeichenfolgenliteral`""` handelt.</span><span class="sxs-lookup"><span data-stu-id="87dba-2390">In a `Like` expression where both operands are `Nothing` or one operand has an intrinsic conversion to `String` and the other operand is `Nothing`, `Nothing` is treated as if it were the empty string literal `""`.</span></span>

<span data-ttu-id="87dba-2391">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="87dba-2391">__Operation Type:__</span></span>

|        | <span data-ttu-id="87dba-2392">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-2392">__Bo__</span></span> | <span data-ttu-id="87dba-2393">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-2393">__SB__</span></span> | <span data-ttu-id="87dba-2394">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-2394">__By__</span></span> | <span data-ttu-id="87dba-2395">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-2395">__Sh__</span></span> | <span data-ttu-id="87dba-2396">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2396">__US__</span></span> | <span data-ttu-id="87dba-2397">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-2397">__In__</span></span> | <span data-ttu-id="87dba-2398">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2398">__UI__</span></span> | <span data-ttu-id="87dba-2399">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-2399">__Lo__</span></span> | <span data-ttu-id="87dba-2400">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2400">__UL__</span></span> | <span data-ttu-id="87dba-2401">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-2401">__De__</span></span> | <span data-ttu-id="87dba-2402">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-2402">__Si__</span></span> | <span data-ttu-id="87dba-2403">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-2403">__Do__</span></span> | <span data-ttu-id="87dba-2404">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-2404">__Da__</span></span>  | <span data-ttu-id="87dba-2405">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-2405">__Ch__</span></span>  | <span data-ttu-id="87dba-2406">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-2406">__St__</span></span> | <span data-ttu-id="87dba-2407">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-2407">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| <span data-ttu-id="87dba-2408">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-2408">__Bo__</span></span> | <span data-ttu-id="87dba-2409">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2409">St</span></span> | <span data-ttu-id="87dba-2410">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2410">St</span></span> | <span data-ttu-id="87dba-2411">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2411">St</span></span> | <span data-ttu-id="87dba-2412">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2412">St</span></span> | <span data-ttu-id="87dba-2413">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2413">St</span></span> | <span data-ttu-id="87dba-2414">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2414">St</span></span> | <span data-ttu-id="87dba-2415">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2415">St</span></span> | <span data-ttu-id="87dba-2416">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2416">St</span></span> | <span data-ttu-id="87dba-2417">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2417">St</span></span> | <span data-ttu-id="87dba-2418">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2418">St</span></span> | <span data-ttu-id="87dba-2419">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2419">St</span></span> | <span data-ttu-id="87dba-2420">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2420">St</span></span> | <span data-ttu-id="87dba-2421">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2421">St</span></span> | <span data-ttu-id="87dba-2422">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2422">St</span></span> | <span data-ttu-id="87dba-2423">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2423">St</span></span> | <span data-ttu-id="87dba-2424">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2424">Ob</span></span> | 
| <span data-ttu-id="87dba-2425">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-2425">__SB__</span></span> |    | <span data-ttu-id="87dba-2426">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2426">St</span></span> | <span data-ttu-id="87dba-2427">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2427">St</span></span> | <span data-ttu-id="87dba-2428">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2428">St</span></span> | <span data-ttu-id="87dba-2429">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2429">St</span></span> | <span data-ttu-id="87dba-2430">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2430">St</span></span> | <span data-ttu-id="87dba-2431">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2431">St</span></span> | <span data-ttu-id="87dba-2432">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2432">St</span></span> | <span data-ttu-id="87dba-2433">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2433">St</span></span> | <span data-ttu-id="87dba-2434">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2434">St</span></span> | <span data-ttu-id="87dba-2435">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2435">St</span></span> | <span data-ttu-id="87dba-2436">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2436">St</span></span> | <span data-ttu-id="87dba-2437">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2437">St</span></span> | <span data-ttu-id="87dba-2438">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2438">St</span></span> | <span data-ttu-id="87dba-2439">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2439">St</span></span> | <span data-ttu-id="87dba-2440">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2440">Ob</span></span> | 
| <span data-ttu-id="87dba-2441">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-2441">__By__</span></span> |    |    | <span data-ttu-id="87dba-2442">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2442">St</span></span> | <span data-ttu-id="87dba-2443">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2443">St</span></span> | <span data-ttu-id="87dba-2444">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2444">St</span></span> | <span data-ttu-id="87dba-2445">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2445">St</span></span> | <span data-ttu-id="87dba-2446">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2446">St</span></span> | <span data-ttu-id="87dba-2447">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2447">St</span></span> | <span data-ttu-id="87dba-2448">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2448">St</span></span> | <span data-ttu-id="87dba-2449">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2449">St</span></span> | <span data-ttu-id="87dba-2450">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2450">St</span></span> | <span data-ttu-id="87dba-2451">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2451">St</span></span> | <span data-ttu-id="87dba-2452">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2452">St</span></span> | <span data-ttu-id="87dba-2453">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2453">St</span></span> | <span data-ttu-id="87dba-2454">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2454">St</span></span> | <span data-ttu-id="87dba-2455">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2455">Ob</span></span> | 
| <span data-ttu-id="87dba-2456">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-2456">__Sh__</span></span> |    |    |    | <span data-ttu-id="87dba-2457">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2457">St</span></span> | <span data-ttu-id="87dba-2458">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2458">St</span></span> | <span data-ttu-id="87dba-2459">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2459">St</span></span> | <span data-ttu-id="87dba-2460">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2460">St</span></span> | <span data-ttu-id="87dba-2461">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2461">St</span></span> | <span data-ttu-id="87dba-2462">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2462">St</span></span> | <span data-ttu-id="87dba-2463">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2463">St</span></span> | <span data-ttu-id="87dba-2464">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2464">St</span></span> | <span data-ttu-id="87dba-2465">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2465">St</span></span> | <span data-ttu-id="87dba-2466">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2466">St</span></span> | <span data-ttu-id="87dba-2467">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2467">St</span></span> | <span data-ttu-id="87dba-2468">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2468">St</span></span> | <span data-ttu-id="87dba-2469">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2469">Ob</span></span> | 
| <span data-ttu-id="87dba-2470">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2470">__US__</span></span> |    |    |    |    | <span data-ttu-id="87dba-2471">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2471">St</span></span> | <span data-ttu-id="87dba-2472">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2472">St</span></span> | <span data-ttu-id="87dba-2473">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2473">St</span></span> | <span data-ttu-id="87dba-2474">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2474">St</span></span> | <span data-ttu-id="87dba-2475">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2475">St</span></span> | <span data-ttu-id="87dba-2476">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2476">St</span></span> | <span data-ttu-id="87dba-2477">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2477">St</span></span> | <span data-ttu-id="87dba-2478">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2478">St</span></span> | <span data-ttu-id="87dba-2479">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2479">St</span></span> | <span data-ttu-id="87dba-2480">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2480">St</span></span> | <span data-ttu-id="87dba-2481">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2481">St</span></span> | <span data-ttu-id="87dba-2482">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2482">Ob</span></span> | 
| <span data-ttu-id="87dba-2483">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-2483">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="87dba-2484">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2484">St</span></span> | <span data-ttu-id="87dba-2485">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2485">St</span></span> | <span data-ttu-id="87dba-2486">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2486">St</span></span> | <span data-ttu-id="87dba-2487">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2487">St</span></span> | <span data-ttu-id="87dba-2488">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2488">St</span></span> | <span data-ttu-id="87dba-2489">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2489">St</span></span> | <span data-ttu-id="87dba-2490">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2490">St</span></span> | <span data-ttu-id="87dba-2491">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2491">St</span></span> | <span data-ttu-id="87dba-2492">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2492">St</span></span> | <span data-ttu-id="87dba-2493">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2493">St</span></span> | <span data-ttu-id="87dba-2494">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2494">Ob</span></span> | 
| <span data-ttu-id="87dba-2495">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2495">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="87dba-2496">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2496">St</span></span> | <span data-ttu-id="87dba-2497">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2497">St</span></span> | <span data-ttu-id="87dba-2498">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2498">St</span></span> | <span data-ttu-id="87dba-2499">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2499">St</span></span> | <span data-ttu-id="87dba-2500">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2500">St</span></span> | <span data-ttu-id="87dba-2501">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2501">St</span></span> | <span data-ttu-id="87dba-2502">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2502">St</span></span> | <span data-ttu-id="87dba-2503">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2503">St</span></span> | <span data-ttu-id="87dba-2504">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2504">St</span></span> | <span data-ttu-id="87dba-2505">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2505">Ob</span></span> | 
| <span data-ttu-id="87dba-2506">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-2506">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2507">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2507">St</span></span> | <span data-ttu-id="87dba-2508">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2508">St</span></span> | <span data-ttu-id="87dba-2509">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2509">St</span></span> | <span data-ttu-id="87dba-2510">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2510">St</span></span> | <span data-ttu-id="87dba-2511">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2511">St</span></span> | <span data-ttu-id="87dba-2512">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2512">St</span></span> | <span data-ttu-id="87dba-2513">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2513">St</span></span> | <span data-ttu-id="87dba-2514">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2514">St</span></span> | <span data-ttu-id="87dba-2515">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2515">Ob</span></span> | 
| <span data-ttu-id="87dba-2516">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2516">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2517">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2517">St</span></span> | <span data-ttu-id="87dba-2518">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2518">St</span></span> | <span data-ttu-id="87dba-2519">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2519">St</span></span> | <span data-ttu-id="87dba-2520">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2520">St</span></span> | <span data-ttu-id="87dba-2521">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2521">St</span></span> | <span data-ttu-id="87dba-2522">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2522">St</span></span> | <span data-ttu-id="87dba-2523">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2523">St</span></span> | <span data-ttu-id="87dba-2524">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2524">Ob</span></span> | 
| <span data-ttu-id="87dba-2525">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-2525">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2526">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2526">St</span></span> | <span data-ttu-id="87dba-2527">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2527">St</span></span> | <span data-ttu-id="87dba-2528">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2528">St</span></span> | <span data-ttu-id="87dba-2529">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2529">St</span></span> | <span data-ttu-id="87dba-2530">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2530">St</span></span> | <span data-ttu-id="87dba-2531">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2531">St</span></span> | <span data-ttu-id="87dba-2532">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2532">Ob</span></span> | 
| <span data-ttu-id="87dba-2533">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-2533">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2534">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2534">St</span></span> | <span data-ttu-id="87dba-2535">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2535">St</span></span> | <span data-ttu-id="87dba-2536">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2536">St</span></span> | <span data-ttu-id="87dba-2537">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2537">St</span></span> | <span data-ttu-id="87dba-2538">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2538">St</span></span> | <span data-ttu-id="87dba-2539">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2539">Ob</span></span> | 
| <span data-ttu-id="87dba-2540">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-2540">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2541">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2541">St</span></span> | <span data-ttu-id="87dba-2542">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2542">St</span></span> | <span data-ttu-id="87dba-2543">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2543">St</span></span> | <span data-ttu-id="87dba-2544">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2544">St</span></span> | <span data-ttu-id="87dba-2545">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2545">Ob</span></span> | 
| <span data-ttu-id="87dba-2546">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-2546">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2547">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2547">St</span></span> | <span data-ttu-id="87dba-2548">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2548">St</span></span> | <span data-ttu-id="87dba-2549">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2549">St</span></span> | <span data-ttu-id="87dba-2550">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2550">Ob</span></span> | 
| <span data-ttu-id="87dba-2551">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-2551">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2552">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2552">St</span></span> | <span data-ttu-id="87dba-2553">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2553">St</span></span> | <span data-ttu-id="87dba-2554">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2554">Ob</span></span> | 
| <span data-ttu-id="87dba-2555">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-2555">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2556">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2556">St</span></span> | <span data-ttu-id="87dba-2557">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2557">Ob</span></span> | 
| <span data-ttu-id="87dba-2558">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-2558">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2559">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2559">Ob</span></span> | 


## <a name="concatenation-operator"></a><span data-ttu-id="87dba-2560">Verkettungs Operator</span><span class="sxs-lookup"><span data-stu-id="87dba-2560">Concatenation Operator</span></span>

```antlr
ConcatenationOperatorExpression
    : Expression '&' LineTerminator? Expression
    ;
```

<span data-ttu-id="87dba-2561">Der *Verkettungs Operator* ist für alle systeminternen Typen definiert, einschließlich der auf NULL festleg baren Versionen der systeminternen Werttypen.</span><span class="sxs-lookup"><span data-stu-id="87dba-2561">The *concatenation operator* is defined for all of the intrinsic types, including the nullable versions of the intrinsic value types.</span></span> <span data-ttu-id="87dba-2562">Sie wird auch für die Verkettung zwischen den oben erwähnten Typen und `System.DBNull` definiert, die als `Nothing`-Zeichenfolge behandelt wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-2562">It is also defined for concatenation between the types mentioned above and `System.DBNull`, which is treated as a `Nothing` string.</span></span> <span data-ttu-id="87dba-2563">Der Verkettungs Operator konvertiert alle seine Operanden in `String`; im Ausdruck werden alle Konvertierungen in `String` als erweiternd betrachtet, unabhängig davon, ob strikte Semantik verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-2563">The concatenation operator converts all of its operands to `String`; in the expression, all conversions to `String` are considered to be widening, regardless of whether strict semantics are used.</span></span> <span data-ttu-id="87dba-2564">Ein `System.DBNull`-Wert wird in das Literal`Nothing` konvertiert, das als `String` typisiert ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-2564">A `System.DBNull` value is converted to the literal `Nothing` typed as `String`.</span></span> <span data-ttu-id="87dba-2565">Ein Werttyp, der auf NULL festgelegt werden kann, dessen Wert `Nothing` ist, wird ebenfalls in das Literale `Nothing` als `String` eingegeben, anstatt einen Laufzeitfehler auszulösen.</span><span class="sxs-lookup"><span data-stu-id="87dba-2565">A nullable value type whose value is `Nothing` is also converted to the literal `Nothing` typed as `String`, rather than throwing a run-time error.</span></span>

<span data-ttu-id="87dba-2566">Ein Verkettungs Vorgang führt zu einer Zeichenfolge, bei der es sich um die Verkettung der beiden Operanden von links nach rechts handelt.</span><span class="sxs-lookup"><span data-stu-id="87dba-2566">A concatenation operation results in a string that is the concatenation of the two operands in order from left to right.</span></span> <span data-ttu-id="87dba-2567">Der Wert `Nothing` wird so behandelt, als ob es sich um das leere Zeichenfolgenliteral`""` handelt.</span><span class="sxs-lookup"><span data-stu-id="87dba-2567">The value `Nothing` is treated as if it were the empty string literal `""`.</span></span>

<span data-ttu-id="87dba-2568">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="87dba-2568">__Operation Type:__</span></span>

|        | <span data-ttu-id="87dba-2569">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-2569">__Bo__</span></span> | <span data-ttu-id="87dba-2570">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-2570">__SB__</span></span> | <span data-ttu-id="87dba-2571">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-2571">__By__</span></span> | <span data-ttu-id="87dba-2572">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-2572">__Sh__</span></span> | <span data-ttu-id="87dba-2573">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2573">__US__</span></span> | <span data-ttu-id="87dba-2574">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-2574">__In__</span></span> | <span data-ttu-id="87dba-2575">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2575">__UI__</span></span> | <span data-ttu-id="87dba-2576">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-2576">__Lo__</span></span> | <span data-ttu-id="87dba-2577">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2577">__UL__</span></span> | <span data-ttu-id="87dba-2578">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-2578">__De__</span></span> | <span data-ttu-id="87dba-2579">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-2579">__Si__</span></span> | <span data-ttu-id="87dba-2580">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-2580">__Do__</span></span> | <span data-ttu-id="87dba-2581">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-2581">__Da__</span></span>  | <span data-ttu-id="87dba-2582">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-2582">__Ch__</span></span>  | <span data-ttu-id="87dba-2583">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-2583">__St__</span></span> | <span data-ttu-id="87dba-2584">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-2584">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| <span data-ttu-id="87dba-2585">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-2585">__Bo__</span></span> | <span data-ttu-id="87dba-2586">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2586">St</span></span> | <span data-ttu-id="87dba-2587">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2587">St</span></span> | <span data-ttu-id="87dba-2588">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2588">St</span></span> | <span data-ttu-id="87dba-2589">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2589">St</span></span> | <span data-ttu-id="87dba-2590">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2590">St</span></span> | <span data-ttu-id="87dba-2591">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2591">St</span></span> | <span data-ttu-id="87dba-2592">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2592">St</span></span> | <span data-ttu-id="87dba-2593">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2593">St</span></span> | <span data-ttu-id="87dba-2594">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2594">St</span></span> | <span data-ttu-id="87dba-2595">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2595">St</span></span> | <span data-ttu-id="87dba-2596">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2596">St</span></span> | <span data-ttu-id="87dba-2597">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2597">St</span></span> | <span data-ttu-id="87dba-2598">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2598">St</span></span> | <span data-ttu-id="87dba-2599">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2599">St</span></span> | <span data-ttu-id="87dba-2600">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2600">St</span></span> | <span data-ttu-id="87dba-2601">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2601">Ob</span></span> | 
| <span data-ttu-id="87dba-2602">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-2602">__SB__</span></span> |    | <span data-ttu-id="87dba-2603">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2603">St</span></span> | <span data-ttu-id="87dba-2604">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2604">St</span></span> | <span data-ttu-id="87dba-2605">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2605">St</span></span> | <span data-ttu-id="87dba-2606">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2606">St</span></span> | <span data-ttu-id="87dba-2607">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2607">St</span></span> | <span data-ttu-id="87dba-2608">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2608">St</span></span> | <span data-ttu-id="87dba-2609">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2609">St</span></span> | <span data-ttu-id="87dba-2610">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2610">St</span></span> | <span data-ttu-id="87dba-2611">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2611">St</span></span> | <span data-ttu-id="87dba-2612">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2612">St</span></span> | <span data-ttu-id="87dba-2613">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2613">St</span></span> | <span data-ttu-id="87dba-2614">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2614">St</span></span> | <span data-ttu-id="87dba-2615">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2615">St</span></span> | <span data-ttu-id="87dba-2616">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2616">St</span></span> | <span data-ttu-id="87dba-2617">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2617">Ob</span></span> | 
| <span data-ttu-id="87dba-2618">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-2618">__By__</span></span> |    |    | <span data-ttu-id="87dba-2619">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2619">St</span></span> | <span data-ttu-id="87dba-2620">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2620">St</span></span> | <span data-ttu-id="87dba-2621">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2621">St</span></span> | <span data-ttu-id="87dba-2622">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2622">St</span></span> | <span data-ttu-id="87dba-2623">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2623">St</span></span> | <span data-ttu-id="87dba-2624">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2624">St</span></span> | <span data-ttu-id="87dba-2625">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2625">St</span></span> | <span data-ttu-id="87dba-2626">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2626">St</span></span> | <span data-ttu-id="87dba-2627">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2627">St</span></span> | <span data-ttu-id="87dba-2628">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2628">St</span></span> | <span data-ttu-id="87dba-2629">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2629">St</span></span> | <span data-ttu-id="87dba-2630">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2630">St</span></span> | <span data-ttu-id="87dba-2631">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2631">St</span></span> | <span data-ttu-id="87dba-2632">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2632">Ob</span></span> | 
| <span data-ttu-id="87dba-2633">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-2633">__Sh__</span></span> |    |    |    | <span data-ttu-id="87dba-2634">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2634">St</span></span> | <span data-ttu-id="87dba-2635">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2635">St</span></span> | <span data-ttu-id="87dba-2636">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2636">St</span></span> | <span data-ttu-id="87dba-2637">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2637">St</span></span> | <span data-ttu-id="87dba-2638">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2638">St</span></span> | <span data-ttu-id="87dba-2639">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2639">St</span></span> | <span data-ttu-id="87dba-2640">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2640">St</span></span> | <span data-ttu-id="87dba-2641">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2641">St</span></span> | <span data-ttu-id="87dba-2642">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2642">St</span></span> | <span data-ttu-id="87dba-2643">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2643">St</span></span> | <span data-ttu-id="87dba-2644">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2644">St</span></span> | <span data-ttu-id="87dba-2645">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2645">St</span></span> | <span data-ttu-id="87dba-2646">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2646">Ob</span></span> | 
| <span data-ttu-id="87dba-2647">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2647">__US__</span></span> |    |    |    |    | <span data-ttu-id="87dba-2648">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2648">St</span></span> | <span data-ttu-id="87dba-2649">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2649">St</span></span> | <span data-ttu-id="87dba-2650">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2650">St</span></span> | <span data-ttu-id="87dba-2651">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2651">St</span></span> | <span data-ttu-id="87dba-2652">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2652">St</span></span> | <span data-ttu-id="87dba-2653">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2653">St</span></span> | <span data-ttu-id="87dba-2654">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2654">St</span></span> | <span data-ttu-id="87dba-2655">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2655">St</span></span> | <span data-ttu-id="87dba-2656">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2656">St</span></span> | <span data-ttu-id="87dba-2657">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2657">St</span></span> | <span data-ttu-id="87dba-2658">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2658">St</span></span> | <span data-ttu-id="87dba-2659">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2659">Ob</span></span> | 
| <span data-ttu-id="87dba-2660">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-2660">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="87dba-2661">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2661">St</span></span> | <span data-ttu-id="87dba-2662">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2662">St</span></span> | <span data-ttu-id="87dba-2663">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2663">St</span></span> | <span data-ttu-id="87dba-2664">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2664">St</span></span> | <span data-ttu-id="87dba-2665">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2665">St</span></span> | <span data-ttu-id="87dba-2666">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2666">St</span></span> | <span data-ttu-id="87dba-2667">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2667">St</span></span> | <span data-ttu-id="87dba-2668">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2668">St</span></span> | <span data-ttu-id="87dba-2669">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2669">St</span></span> | <span data-ttu-id="87dba-2670">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2670">St</span></span> | <span data-ttu-id="87dba-2671">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2671">Ob</span></span> | 
| <span data-ttu-id="87dba-2672">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2672">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="87dba-2673">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2673">St</span></span> | <span data-ttu-id="87dba-2674">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2674">St</span></span> | <span data-ttu-id="87dba-2675">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2675">St</span></span> | <span data-ttu-id="87dba-2676">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2676">St</span></span> | <span data-ttu-id="87dba-2677">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2677">St</span></span> | <span data-ttu-id="87dba-2678">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2678">St</span></span> | <span data-ttu-id="87dba-2679">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2679">St</span></span> | <span data-ttu-id="87dba-2680">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2680">St</span></span> | <span data-ttu-id="87dba-2681">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2681">St</span></span> | <span data-ttu-id="87dba-2682">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2682">Ob</span></span> | 
| <span data-ttu-id="87dba-2683">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-2683">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2684">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2684">St</span></span> | <span data-ttu-id="87dba-2685">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2685">St</span></span> | <span data-ttu-id="87dba-2686">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2686">St</span></span> | <span data-ttu-id="87dba-2687">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2687">St</span></span> | <span data-ttu-id="87dba-2688">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2688">St</span></span> | <span data-ttu-id="87dba-2689">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2689">St</span></span> | <span data-ttu-id="87dba-2690">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2690">St</span></span> | <span data-ttu-id="87dba-2691">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2691">St</span></span> | <span data-ttu-id="87dba-2692">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2692">Ob</span></span> | 
| <span data-ttu-id="87dba-2693">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2693">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2694">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2694">St</span></span> | <span data-ttu-id="87dba-2695">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2695">St</span></span> | <span data-ttu-id="87dba-2696">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2696">St</span></span> | <span data-ttu-id="87dba-2697">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2697">St</span></span> | <span data-ttu-id="87dba-2698">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2698">St</span></span> | <span data-ttu-id="87dba-2699">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2699">St</span></span> | <span data-ttu-id="87dba-2700">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2700">St</span></span> | <span data-ttu-id="87dba-2701">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2701">Ob</span></span> | 
| <span data-ttu-id="87dba-2702">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-2702">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2703">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2703">St</span></span> | <span data-ttu-id="87dba-2704">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2704">St</span></span> | <span data-ttu-id="87dba-2705">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2705">St</span></span> | <span data-ttu-id="87dba-2706">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2706">St</span></span> | <span data-ttu-id="87dba-2707">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2707">St</span></span> | <span data-ttu-id="87dba-2708">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2708">St</span></span> | <span data-ttu-id="87dba-2709">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2709">Ob</span></span> | 
| <span data-ttu-id="87dba-2710">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-2710">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2711">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2711">St</span></span> | <span data-ttu-id="87dba-2712">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2712">St</span></span> | <span data-ttu-id="87dba-2713">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2713">St</span></span> | <span data-ttu-id="87dba-2714">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2714">St</span></span> | <span data-ttu-id="87dba-2715">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2715">St</span></span> | <span data-ttu-id="87dba-2716">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2716">Ob</span></span> | 
| <span data-ttu-id="87dba-2717">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-2717">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2718">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2718">St</span></span> | <span data-ttu-id="87dba-2719">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2719">St</span></span> | <span data-ttu-id="87dba-2720">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2720">St</span></span> | <span data-ttu-id="87dba-2721">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2721">St</span></span> | <span data-ttu-id="87dba-2722">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2722">Ob</span></span> | 
| <span data-ttu-id="87dba-2723">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-2723">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2724">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2724">St</span></span> | <span data-ttu-id="87dba-2725">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2725">St</span></span> | <span data-ttu-id="87dba-2726">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2726">St</span></span> | <span data-ttu-id="87dba-2727">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2727">Ob</span></span> | 
| <span data-ttu-id="87dba-2728">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-2728">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2729">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2729">St</span></span> | <span data-ttu-id="87dba-2730">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2730">St</span></span> | <span data-ttu-id="87dba-2731">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2731">Ob</span></span> | 
| <span data-ttu-id="87dba-2732">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-2732">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2733">St</span><span class="sxs-lookup"><span data-stu-id="87dba-2733">St</span></span> | <span data-ttu-id="87dba-2734">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2734">Ob</span></span> | 
| <span data-ttu-id="87dba-2735">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-2735">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2736">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2736">Ob</span></span> | 


## <a name="logical-operators"></a><span data-ttu-id="87dba-2737">Logische Operatoren</span><span class="sxs-lookup"><span data-stu-id="87dba-2737">Logical Operators</span></span>

<span data-ttu-id="87dba-2738">Die Operatoren "`And`", "`Not`", "`Or`" und "`Xor`" werden als logische Operatoren bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="87dba-2738">The `And`, `Not`, `Or`, and `Xor` operators are called the logical operators.</span></span>

```antlr
LogicalOperatorExpression
    : 'Not' Expression
    | Expression 'And' LineTerminator? Expression
    | Expression 'Or' LineTerminator? Expression
    | Expression 'Xor' LineTerminator? Expression
    ;
```

<span data-ttu-id="87dba-2739">Die logischen Operatoren werden wie folgt ausgewertet:</span><span class="sxs-lookup"><span data-stu-id="87dba-2739">The logical operators are evaluated as follows:</span></span>

* <span data-ttu-id="87dba-2740">Für den `Boolean`-Typ:</span><span class="sxs-lookup"><span data-stu-id="87dba-2740">For the `Boolean` type:</span></span>

  * <span data-ttu-id="87dba-2741">Ein logischer `And`-Vorgang wird für die beiden Operanden ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="87dba-2741">A logical `And` operation is performed on its two operands.</span></span>

  * <span data-ttu-id="87dba-2742">Ein logischer `Not`-Vorgang wird für den Operanden ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="87dba-2742">A logical `Not` operation is performed on its operand.</span></span>

  * <span data-ttu-id="87dba-2743">Ein logischer `Or`-Vorgang wird für die beiden Operanden ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="87dba-2743">A logical `Or` operation is performed on its two operands.</span></span>

  * <span data-ttu-id="87dba-2744">Ein logischer exklusiv-`Or`-Vorgang wird für die beiden Operanden ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="87dba-2744">A logical exclusive-`Or` operation is performed on its two operands.</span></span>

* <span data-ttu-id="87dba-2745">Bei `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long` und allen enumerierten Typen wird der angegebene Vorgang für jedes Bit der binären Darstellung der beiden Operanden ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="87dba-2745">For `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, and all enumerated types, the specified operation is performed on each bit of the binary representation of the two operand(s):</span></span>

  * <span data-ttu-id="87dba-2746">`And`: Das Ergebnisbit ist 1, wenn beide Bits 1 sind. Andernfalls ist das Ergebnisbit 0 (null).</span><span class="sxs-lookup"><span data-stu-id="87dba-2746">`And`: The result bit is 1 if both bits are 1; otherwise the result bit is 0.</span></span>

  * <span data-ttu-id="87dba-2747">`Not`: Das Ergebnisbit ist 1, wenn das Bit 0 (null) ist. Andernfalls ist das Ergebnisbit 1.</span><span class="sxs-lookup"><span data-stu-id="87dba-2747">`Not`: The result bit is 1 if the bit is 0; otherwise the result bit is 1.</span></span>

  * <span data-ttu-id="87dba-2748">`Or`: Das Ergebnisbit ist 1, wenn eines der Bits 1 ist. Andernfalls ist das Ergebnisbit 0 (null).</span><span class="sxs-lookup"><span data-stu-id="87dba-2748">`Or`: The result bit is 1 if either bit is 1; otherwise the result bit is 0.</span></span>

  * <span data-ttu-id="87dba-2749">`Xor`: Das Ergebnisbit ist 1, wenn eines der beiden Bits gleich 1 ist. Andernfalls ist das Ergebnisbit 0 (d. h. 1 `Xor` 0 = 1, 1 `Xor` 1 = 0).</span><span class="sxs-lookup"><span data-stu-id="87dba-2749">`Xor`: The result bit is 1 if either bit is 1 but not both bits; otherwise the result bit is 0 (that is, 1 `Xor` 0 = 1, 1 `Xor` 1 = 0).</span></span>

* <span data-ttu-id="87dba-2750">Wenn die logischen Operatoren `And` und `Or` für den Typ `Boolean?` angehoben werden, werden Sie auf eine dreiwertige boolesche Logik wie folgt erweitert:</span><span class="sxs-lookup"><span data-stu-id="87dba-2750">When the logical operators `And` and `Or` are lifted for the type `Boolean?`, they are extended to encompass three-valued Boolean logic as such:</span></span>

  * <span data-ttu-id="87dba-2751">`And` ergibt true, wenn beide Operanden true sind. false, wenn einer der Operanden false ist. `Nothing` andernfalls.</span><span class="sxs-lookup"><span data-stu-id="87dba-2751">`And` evaluates to true if both operands are true; false if one of the operands is false; `Nothing` otherwise.</span></span>

  * <span data-ttu-id="87dba-2752">`Or` wird zu true ausgewertet, wenn einer der beiden Operanden true ist. false gibt an, dass beide Operanden false sind. `Nothing` andernfalls.</span><span class="sxs-lookup"><span data-stu-id="87dba-2752">`Or` evaluates to true if either operand is true; false is both operands are false; `Nothing` otherwise.</span></span>

<span data-ttu-id="87dba-2753">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-2753">For example:</span></span>

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

<span data-ttu-id="87dba-2754">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-2754">__Note.__</span></span> <span data-ttu-id="87dba-2755">Im Idealfall werden die logischen Operatoren `And` und `Or` mithilfe von dreiwertigen Logik für jeden Typ, der in einem booleschen Ausdruck verwendet werden kann (d. h. einen Typ, der `IsTrue` und `IsFalse` implementiert), auf die gleiche Weise wie `AndAlso` und `OrElse` Kurzschluss über alle der Typ, der in einem booleschen Ausdruck verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="87dba-2755">Ideally, the logical operators `And` and `Or` would be lifted using three-valued logic for any type that can be used in a Boolean expression (i.e. a type that implements `IsTrue` and `IsFalse`), in the same way that `AndAlso` and `OrElse` short circuit across any type that can be used in a Boolean expression.</span></span> <span data-ttu-id="87dba-2756">Leider wird die dreiwertige Erhöhung nur auf `Boolean?` angewendet, sodass benutzerdefinierte Typen, die dreiwertige Logik benötigen, dies manuell tun müssen, indem Sie `And`-und `Or`-Operatoren für Ihre Version definieren, die NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="87dba-2756">Unfortunately, three-valued lifting is only applied to `Boolean?`, so user-defined types that desire three-valued logic must do so manually by defining `And` and `Or` operators for their nullable version.</span></span>

<span data-ttu-id="87dba-2757">Von diesen Vorgängen können keine über Flüsse durchlaufen werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-2757">No overflows are possible from these operations.</span></span> <span data-ttu-id="87dba-2758">Die enumerierten Typoperatoren führen die bitweise Operation für den zugrunde liegenden Typ des Enumerationstyps aus, aber der Rückgabewert ist der enumerierte Typ.</span><span class="sxs-lookup"><span data-stu-id="87dba-2758">The enumerated type operators do the bitwise operation on the underlying type of the enumerated type, but the return value is the enumerated type.</span></span>

<span data-ttu-id="87dba-2759">__Nicht Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="87dba-2759">__Not Operation Type:__</span></span>

| <span data-ttu-id="87dba-2760">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-2760">__Bo__</span></span> | <span data-ttu-id="87dba-2761">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-2761">__SB__</span></span> | <span data-ttu-id="87dba-2762">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-2762">__By__</span></span> | <span data-ttu-id="87dba-2763">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-2763">__Sh__</span></span> | <span data-ttu-id="87dba-2764">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2764">__US__</span></span> | <span data-ttu-id="87dba-2765">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-2765">__In__</span></span> | <span data-ttu-id="87dba-2766">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2766">__UI__</span></span> | <span data-ttu-id="87dba-2767">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-2767">__Lo__</span></span> | <span data-ttu-id="87dba-2768">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2768">__UL__</span></span> | <span data-ttu-id="87dba-2769">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-2769">__De__</span></span> | <span data-ttu-id="87dba-2770">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-2770">__Si__</span></span> | <span data-ttu-id="87dba-2771">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-2771">__Do__</span></span> | <span data-ttu-id="87dba-2772">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-2772">__Da__</span></span>  | <span data-ttu-id="87dba-2773">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-2773">__Ch__</span></span>  | <span data-ttu-id="87dba-2774">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-2774">__St__</span></span> | <span data-ttu-id="87dba-2775">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-2775">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="87dba-2776">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-2776">Bo</span></span> | <span data-ttu-id="87dba-2777">SB</span><span class="sxs-lookup"><span data-stu-id="87dba-2777">SB</span></span> | <span data-ttu-id="87dba-2778">um</span><span class="sxs-lookup"><span data-stu-id="87dba-2778">By</span></span> | <span data-ttu-id="87dba-2779">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-2779">Sh</span></span> | <span data-ttu-id="87dba-2780">US</span><span class="sxs-lookup"><span data-stu-id="87dba-2780">US</span></span> | <span data-ttu-id="87dba-2781">In</span><span class="sxs-lookup"><span data-stu-id="87dba-2781">In</span></span> | <span data-ttu-id="87dba-2782">UI</span><span class="sxs-lookup"><span data-stu-id="87dba-2782">UI</span></span> | <span data-ttu-id="87dba-2783">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2783">Lo</span></span> | <span data-ttu-id="87dba-2784">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-2784">UL</span></span> | <span data-ttu-id="87dba-2785">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2785">Lo</span></span> | <span data-ttu-id="87dba-2786">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2786">Lo</span></span> | <span data-ttu-id="87dba-2787">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2787">Lo</span></span> | <span data-ttu-id="87dba-2788">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2788">Err</span></span> | <span data-ttu-id="87dba-2789">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2789">Err</span></span> | <span data-ttu-id="87dba-2790">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2790">Lo</span></span> | <span data-ttu-id="87dba-2791">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2791">Ob</span></span> | 

<span data-ttu-id="87dba-2792">__And-, or-, Xor-Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="87dba-2792">__And, Or, Xor Operation Type:__</span></span>

|        | <span data-ttu-id="87dba-2793">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-2793">__Bo__</span></span> | <span data-ttu-id="87dba-2794">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-2794">__SB__</span></span> | <span data-ttu-id="87dba-2795">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-2795">__By__</span></span> | <span data-ttu-id="87dba-2796">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-2796">__Sh__</span></span> | <span data-ttu-id="87dba-2797">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2797">__US__</span></span> | <span data-ttu-id="87dba-2798">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-2798">__In__</span></span> | <span data-ttu-id="87dba-2799">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2799">__UI__</span></span> | <span data-ttu-id="87dba-2800">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-2800">__Lo__</span></span> | <span data-ttu-id="87dba-2801">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2801">__UL__</span></span> | <span data-ttu-id="87dba-2802">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-2802">__De__</span></span> | <span data-ttu-id="87dba-2803">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-2803">__Si__</span></span> | <span data-ttu-id="87dba-2804">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-2804">__Do__</span></span> | <span data-ttu-id="87dba-2805">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-2805">__Da__</span></span>  | <span data-ttu-id="87dba-2806">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-2806">__Ch__</span></span>  | <span data-ttu-id="87dba-2807">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-2807">__St__</span></span> | <span data-ttu-id="87dba-2808">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-2808">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="87dba-2809">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-2809">__Bo__</span></span> | <span data-ttu-id="87dba-2810">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-2810">Bo</span></span> | <span data-ttu-id="87dba-2811">SB</span><span class="sxs-lookup"><span data-stu-id="87dba-2811">SB</span></span> | <span data-ttu-id="87dba-2812">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-2812">Sh</span></span> | <span data-ttu-id="87dba-2813">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-2813">Sh</span></span> | <span data-ttu-id="87dba-2814">In</span><span class="sxs-lookup"><span data-stu-id="87dba-2814">In</span></span> | <span data-ttu-id="87dba-2815">In</span><span class="sxs-lookup"><span data-stu-id="87dba-2815">In</span></span> | <span data-ttu-id="87dba-2816">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2816">Lo</span></span> | <span data-ttu-id="87dba-2817">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2817">Lo</span></span> | <span data-ttu-id="87dba-2818">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2818">Lo</span></span> | <span data-ttu-id="87dba-2819">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2819">Lo</span></span> | <span data-ttu-id="87dba-2820">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2820">Lo</span></span> | <span data-ttu-id="87dba-2821">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2821">Lo</span></span> | <span data-ttu-id="87dba-2822">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2822">Err</span></span> | <span data-ttu-id="87dba-2823">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2823">Err</span></span> | <span data-ttu-id="87dba-2824">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-2824">Bo</span></span>  | <span data-ttu-id="87dba-2825">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2825">Ob</span></span>  | 
| <span data-ttu-id="87dba-2826">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-2826">__SB__</span></span> |    | <span data-ttu-id="87dba-2827">SB</span><span class="sxs-lookup"><span data-stu-id="87dba-2827">SB</span></span> | <span data-ttu-id="87dba-2828">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-2828">Sh</span></span> | <span data-ttu-id="87dba-2829">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-2829">Sh</span></span> | <span data-ttu-id="87dba-2830">In</span><span class="sxs-lookup"><span data-stu-id="87dba-2830">In</span></span> | <span data-ttu-id="87dba-2831">In</span><span class="sxs-lookup"><span data-stu-id="87dba-2831">In</span></span> | <span data-ttu-id="87dba-2832">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2832">Lo</span></span> | <span data-ttu-id="87dba-2833">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2833">Lo</span></span> | <span data-ttu-id="87dba-2834">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2834">Lo</span></span> | <span data-ttu-id="87dba-2835">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2835">Lo</span></span> | <span data-ttu-id="87dba-2836">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2836">Lo</span></span> | <span data-ttu-id="87dba-2837">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2837">Lo</span></span> | <span data-ttu-id="87dba-2838">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2838">Err</span></span> | <span data-ttu-id="87dba-2839">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2839">Err</span></span> | <span data-ttu-id="87dba-2840">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2840">Lo</span></span>  | <span data-ttu-id="87dba-2841">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2841">Ob</span></span>  | 
| <span data-ttu-id="87dba-2842">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-2842">__By__</span></span> |    |    | <span data-ttu-id="87dba-2843">um</span><span class="sxs-lookup"><span data-stu-id="87dba-2843">By</span></span> | <span data-ttu-id="87dba-2844">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-2844">Sh</span></span> | <span data-ttu-id="87dba-2845">US</span><span class="sxs-lookup"><span data-stu-id="87dba-2845">US</span></span> | <span data-ttu-id="87dba-2846">In</span><span class="sxs-lookup"><span data-stu-id="87dba-2846">In</span></span> | <span data-ttu-id="87dba-2847">UI</span><span class="sxs-lookup"><span data-stu-id="87dba-2847">UI</span></span> | <span data-ttu-id="87dba-2848">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2848">Lo</span></span> | <span data-ttu-id="87dba-2849">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-2849">UL</span></span> | <span data-ttu-id="87dba-2850">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2850">Lo</span></span> | <span data-ttu-id="87dba-2851">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2851">Lo</span></span> | <span data-ttu-id="87dba-2852">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2852">Lo</span></span> | <span data-ttu-id="87dba-2853">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2853">Err</span></span> | <span data-ttu-id="87dba-2854">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2854">Err</span></span> | <span data-ttu-id="87dba-2855">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2855">Lo</span></span>  | <span data-ttu-id="87dba-2856">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2856">Ob</span></span>  | 
| <span data-ttu-id="87dba-2857">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-2857">__Sh__</span></span> |    |    |    | <span data-ttu-id="87dba-2858">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-2858">Sh</span></span> | <span data-ttu-id="87dba-2859">In</span><span class="sxs-lookup"><span data-stu-id="87dba-2859">In</span></span> | <span data-ttu-id="87dba-2860">In</span><span class="sxs-lookup"><span data-stu-id="87dba-2860">In</span></span> | <span data-ttu-id="87dba-2861">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2861">Lo</span></span> | <span data-ttu-id="87dba-2862">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2862">Lo</span></span> | <span data-ttu-id="87dba-2863">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2863">Lo</span></span> | <span data-ttu-id="87dba-2864">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2864">Lo</span></span> | <span data-ttu-id="87dba-2865">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2865">Lo</span></span> | <span data-ttu-id="87dba-2866">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2866">Lo</span></span> | <span data-ttu-id="87dba-2867">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2867">Err</span></span> | <span data-ttu-id="87dba-2868">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2868">Err</span></span> | <span data-ttu-id="87dba-2869">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2869">Lo</span></span>  | <span data-ttu-id="87dba-2870">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2870">Ob</span></span>  | 
| <span data-ttu-id="87dba-2871">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2871">__US__</span></span> |    |    |    |    | <span data-ttu-id="87dba-2872">US</span><span class="sxs-lookup"><span data-stu-id="87dba-2872">US</span></span> | <span data-ttu-id="87dba-2873">In</span><span class="sxs-lookup"><span data-stu-id="87dba-2873">In</span></span> | <span data-ttu-id="87dba-2874">UI</span><span class="sxs-lookup"><span data-stu-id="87dba-2874">UI</span></span> | <span data-ttu-id="87dba-2875">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2875">Lo</span></span> | <span data-ttu-id="87dba-2876">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-2876">UL</span></span> | <span data-ttu-id="87dba-2877">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2877">Lo</span></span> | <span data-ttu-id="87dba-2878">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2878">Lo</span></span> | <span data-ttu-id="87dba-2879">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2879">Lo</span></span> | <span data-ttu-id="87dba-2880">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2880">Err</span></span> | <span data-ttu-id="87dba-2881">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2881">Err</span></span> | <span data-ttu-id="87dba-2882">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2882">Lo</span></span>  | <span data-ttu-id="87dba-2883">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2883">Ob</span></span>  | 
| <span data-ttu-id="87dba-2884">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-2884">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="87dba-2885">In</span><span class="sxs-lookup"><span data-stu-id="87dba-2885">In</span></span> | <span data-ttu-id="87dba-2886">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2886">Lo</span></span> | <span data-ttu-id="87dba-2887">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2887">Lo</span></span> | <span data-ttu-id="87dba-2888">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2888">Lo</span></span> | <span data-ttu-id="87dba-2889">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2889">Lo</span></span> | <span data-ttu-id="87dba-2890">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2890">Lo</span></span> | <span data-ttu-id="87dba-2891">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2891">Lo</span></span> | <span data-ttu-id="87dba-2892">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2892">Err</span></span> | <span data-ttu-id="87dba-2893">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2893">Err</span></span> | <span data-ttu-id="87dba-2894">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2894">Lo</span></span>  | <span data-ttu-id="87dba-2895">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2895">Ob</span></span>  | 
| <span data-ttu-id="87dba-2896">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2896">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="87dba-2897">UI</span><span class="sxs-lookup"><span data-stu-id="87dba-2897">UI</span></span> | <span data-ttu-id="87dba-2898">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2898">Lo</span></span> | <span data-ttu-id="87dba-2899">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-2899">UL</span></span> | <span data-ttu-id="87dba-2900">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2900">Lo</span></span> | <span data-ttu-id="87dba-2901">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2901">Lo</span></span> | <span data-ttu-id="87dba-2902">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2902">Lo</span></span> | <span data-ttu-id="87dba-2903">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2903">Err</span></span> | <span data-ttu-id="87dba-2904">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2904">Err</span></span> | <span data-ttu-id="87dba-2905">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2905">Lo</span></span>  | <span data-ttu-id="87dba-2906">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2906">Ob</span></span>  | 
| <span data-ttu-id="87dba-2907">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-2907">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2908">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2908">Lo</span></span> | <span data-ttu-id="87dba-2909">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2909">Lo</span></span> | <span data-ttu-id="87dba-2910">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2910">Lo</span></span> | <span data-ttu-id="87dba-2911">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2911">Lo</span></span> | <span data-ttu-id="87dba-2912">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2912">Lo</span></span> | <span data-ttu-id="87dba-2913">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2913">Err</span></span> | <span data-ttu-id="87dba-2914">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2914">Err</span></span> | <span data-ttu-id="87dba-2915">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2915">Lo</span></span>  | <span data-ttu-id="87dba-2916">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2916">Ob</span></span>  | 
| <span data-ttu-id="87dba-2917">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2917">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2918">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-2918">UL</span></span> | <span data-ttu-id="87dba-2919">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2919">Lo</span></span> | <span data-ttu-id="87dba-2920">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2920">Lo</span></span> | <span data-ttu-id="87dba-2921">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2921">Lo</span></span> | <span data-ttu-id="87dba-2922">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2922">Err</span></span> | <span data-ttu-id="87dba-2923">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2923">Err</span></span> | <span data-ttu-id="87dba-2924">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2924">Lo</span></span>  | <span data-ttu-id="87dba-2925">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2925">Ob</span></span>  | 
| <span data-ttu-id="87dba-2926">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-2926">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2927">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2927">Lo</span></span> | <span data-ttu-id="87dba-2928">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2928">Lo</span></span> | <span data-ttu-id="87dba-2929">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2929">Lo</span></span> | <span data-ttu-id="87dba-2930">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2930">Err</span></span> | <span data-ttu-id="87dba-2931">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2931">Err</span></span> | <span data-ttu-id="87dba-2932">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2932">Lo</span></span>  | <span data-ttu-id="87dba-2933">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2933">Ob</span></span>  | 
| <span data-ttu-id="87dba-2934">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-2934">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2935">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2935">Lo</span></span> | <span data-ttu-id="87dba-2936">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2936">Lo</span></span> | <span data-ttu-id="87dba-2937">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2937">Err</span></span> | <span data-ttu-id="87dba-2938">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2938">Err</span></span> | <span data-ttu-id="87dba-2939">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2939">Lo</span></span>  | <span data-ttu-id="87dba-2940">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2940">Ob</span></span>  | 
| <span data-ttu-id="87dba-2941">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-2941">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2942">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2942">Lo</span></span> | <span data-ttu-id="87dba-2943">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2943">Err</span></span> | <span data-ttu-id="87dba-2944">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2944">Err</span></span> | <span data-ttu-id="87dba-2945">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2945">Lo</span></span>  | <span data-ttu-id="87dba-2946">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2946">Ob</span></span>  | 
| <span data-ttu-id="87dba-2947">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-2947">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-2948">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2948">Err</span></span> | <span data-ttu-id="87dba-2949">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2949">Err</span></span> | <span data-ttu-id="87dba-2950">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2950">Err</span></span> | <span data-ttu-id="87dba-2951">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2951">Err</span></span> | 
| <span data-ttu-id="87dba-2952">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-2952">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="87dba-2953">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2953">Err</span></span> | <span data-ttu-id="87dba-2954">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2954">Err</span></span> | <span data-ttu-id="87dba-2955">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-2955">Err</span></span> | 
| <span data-ttu-id="87dba-2956">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-2956">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="87dba-2957">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-2957">Lo</span></span>  | <span data-ttu-id="87dba-2958">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2958">Ob</span></span>  | 
| <span data-ttu-id="87dba-2959">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-2959">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="87dba-2960">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-2960">Ob</span></span>  | 


### <a name="short-circuiting-logical-operators"></a><span data-ttu-id="87dba-2961">Kurzschluss logische Operatoren</span><span class="sxs-lookup"><span data-stu-id="87dba-2961">Short-circuiting Logical Operators</span></span>

<span data-ttu-id="87dba-2962">Die Operatoren "`AndAlso`" und "`OrElse`" sind die Kurzschluss Versionen der logischen Operatoren "`And`" und "`Or`".</span><span class="sxs-lookup"><span data-stu-id="87dba-2962">The `AndAlso` and `OrElse` operators are the short-circuiting versions of the `And` and `Or` logical operators.</span></span>

```antlr
ShortCircuitLogicalOperatorExpression
    : Expression 'AndAlso' LineTerminator? Expression
    | Expression 'OrElse' LineTerminator? Expression
    ;
```

<span data-ttu-id="87dba-2963">Aufgrund des kurzen Schluss Verhaltens wird der zweite Operand nicht zur Laufzeit ausgewertet, wenn das Operator Ergebnis nach der Auswertung des ersten Operanden bekannt ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-2963">Because of their short circuiting behavior, the second operand is not evaluated at run time if the operator result is known after evaluating the first operand.</span></span>

<span data-ttu-id="87dba-2964">Die Kurzschluss logischen Operatoren werden wie folgt ausgewertet:</span><span class="sxs-lookup"><span data-stu-id="87dba-2964">The short-circuiting logical operators are evaluated as follows:</span></span>

* <span data-ttu-id="87dba-2965">Wenn der erste Operand in einem `AndAlso`-Vorgang zu `False` ausgewertet wird oder true von seinem `IsFalse`-Operator zurückgibt, gibt der Ausdruck seinen ersten Operanden zurück.</span><span class="sxs-lookup"><span data-stu-id="87dba-2965">If the first operand in an `AndAlso` operation evaluates to `False` or returns True from its `IsFalse` operator, the expression returns its first operand.</span></span> <span data-ttu-id="87dba-2966">Andernfalls wird der zweite Operand ausgewertet, und ein logischer `And`-Vorgang wird für die beiden Ergebnisse ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="87dba-2966">Otherwise, the second operand is evaluated and a logical `And` operation is performed on the two results.</span></span>

* <span data-ttu-id="87dba-2967">Wenn der erste Operand in einem `OrElse`-Vorgang zu `True` ausgewertet wird oder true von seinem `IsTrue`-Operator zurückgibt, gibt der Ausdruck seinen ersten Operanden zurück.</span><span class="sxs-lookup"><span data-stu-id="87dba-2967">If the first operand in an `OrElse` operation evaluates to `True` or returns True from its `IsTrue` operator, the expression returns its first operand.</span></span> <span data-ttu-id="87dba-2968">Andernfalls wird der zweite Operand ausgewertet, und ein logischer `Or`-Vorgang wird für die beiden Ergebnisse ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="87dba-2968">Otherwise, the second operand is evaluated and a logical `Or` operation is performed on its two results.</span></span>

<span data-ttu-id="87dba-2969">Die Operatoren "`AndAlso`" und "`OrElse`" sind für den Typ "`Boolean`" oder für jeden Typ "`T`" definiert, der die folgenden Operatoren über lädt:</span><span class="sxs-lookup"><span data-stu-id="87dba-2969">The `AndAlso` and `OrElse` operators are defined for the type `Boolean`, or for any type `T` that overloads the following operators:</span></span>

```vb
Public Shared Operator IsTrue(op As T) As Boolean
Public Shared Operator IsFalse(op As T) As Boolean
```

<span data-ttu-id="87dba-2970">Außerdem wird der entsprechende `And`-oder `Or`-Operator überladen:</span><span class="sxs-lookup"><span data-stu-id="87dba-2970">as well as overloading the corresponding `And` or `Or` operator:</span></span>

```vb
Public Shared Operator And(op1 As T, op2 As T) As T
Public Shared Operator Or(op1 As T, op2 As T) As T
```

<span data-ttu-id="87dba-2971">Beim Auswerten der Operatoren "`AndAlso`" oder "`OrElse`" wird der erste Operand nur einmal ausgewertet, und der zweite Operand wird entweder nicht genau einmal ausgewertet oder ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="87dba-2971">When evaluating the `AndAlso` or `OrElse` operators, the first operand is evaluated only once, and the second operand is either not evaluated or evaluated exactly once.</span></span> <span data-ttu-id="87dba-2972">Beachten Sie z. B. folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="87dba-2972">For example, consider the following code:</span></span>

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

<span data-ttu-id="87dba-2973">Es gibt das folgende Ergebnis aus:</span><span class="sxs-lookup"><span data-stu-id="87dba-2973">It prints the following result:</span></span>

```console
And: False True
Or: True False
AndAlso: False
OrElse: True
```

<span data-ttu-id="87dba-2974">Wenn der erste Operand ein NULL-Wert `Boolean?` ist, wird der zweite Operand ausgewertet, wobei das Ergebnis immer ein NULL-`Boolean?` ist, wenn der erste Operand ein NULL--2-Operator ist. @no__t @no__t</span><span class="sxs-lookup"><span data-stu-id="87dba-2974">In the lifted form of the `AndAlso` and `OrElse` operators, if the first operand was a null `Boolean?`, then the second operand is evaluated but the result is always a null `Boolean?`.</span></span>

<span data-ttu-id="87dba-2975">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="87dba-2975">__Operation Type:__</span></span>

|        | <span data-ttu-id="87dba-2976">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-2976">__Bo__</span></span> | <span data-ttu-id="87dba-2977">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-2977">__SB__</span></span> | <span data-ttu-id="87dba-2978">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-2978">__By__</span></span> | <span data-ttu-id="87dba-2979">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-2979">__Sh__</span></span> | <span data-ttu-id="87dba-2980">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2980">__US__</span></span> | <span data-ttu-id="87dba-2981">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-2981">__In__</span></span> | <span data-ttu-id="87dba-2982">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2982">__UI__</span></span> | <span data-ttu-id="87dba-2983">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-2983">__Lo__</span></span> | <span data-ttu-id="87dba-2984">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-2984">__UL__</span></span> | <span data-ttu-id="87dba-2985">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-2985">__De__</span></span> | <span data-ttu-id="87dba-2986">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-2986">__Si__</span></span> | <span data-ttu-id="87dba-2987">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-2987">__Do__</span></span> | <span data-ttu-id="87dba-2988">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-2988">__Da__</span></span>  | <span data-ttu-id="87dba-2989">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-2989">__Ch__</span></span>  | <span data-ttu-id="87dba-2990">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-2990">__St__</span></span> | <span data-ttu-id="87dba-2991">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-2991">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="87dba-2992">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-2992">__Bo__</span></span> | <span data-ttu-id="87dba-2993">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-2993">Bo</span></span> | <span data-ttu-id="87dba-2994">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-2994">Bo</span></span> | <span data-ttu-id="87dba-2995">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-2995">Bo</span></span> | <span data-ttu-id="87dba-2996">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-2996">Bo</span></span> | <span data-ttu-id="87dba-2997">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-2997">Bo</span></span> | <span data-ttu-id="87dba-2998">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-2998">Bo</span></span> | <span data-ttu-id="87dba-2999">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-2999">Bo</span></span> | <span data-ttu-id="87dba-3000">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3000">Bo</span></span> | <span data-ttu-id="87dba-3001">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3001">Bo</span></span> | <span data-ttu-id="87dba-3002">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3002">Bo</span></span> | <span data-ttu-id="87dba-3003">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3003">Bo</span></span> | <span data-ttu-id="87dba-3004">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3004">Bo</span></span> | <span data-ttu-id="87dba-3005">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3005">Err</span></span> | <span data-ttu-id="87dba-3006">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3006">Err</span></span> | <span data-ttu-id="87dba-3007">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3007">Bo</span></span>  | <span data-ttu-id="87dba-3008">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-3008">Ob</span></span>  | 
| <span data-ttu-id="87dba-3009">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-3009">__SB__</span></span> |    | <span data-ttu-id="87dba-3010">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3010">Bo</span></span> | <span data-ttu-id="87dba-3011">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3011">Bo</span></span> | <span data-ttu-id="87dba-3012">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3012">Bo</span></span> | <span data-ttu-id="87dba-3013">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3013">Bo</span></span> | <span data-ttu-id="87dba-3014">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3014">Bo</span></span> | <span data-ttu-id="87dba-3015">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3015">Bo</span></span> | <span data-ttu-id="87dba-3016">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3016">Bo</span></span> | <span data-ttu-id="87dba-3017">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3017">Bo</span></span> | <span data-ttu-id="87dba-3018">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3018">Bo</span></span> | <span data-ttu-id="87dba-3019">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3019">Bo</span></span> | <span data-ttu-id="87dba-3020">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3020">Bo</span></span> | <span data-ttu-id="87dba-3021">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3021">Err</span></span> | <span data-ttu-id="87dba-3022">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3022">Err</span></span> | <span data-ttu-id="87dba-3023">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3023">Bo</span></span>  | <span data-ttu-id="87dba-3024">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-3024">Ob</span></span>  | 
| <span data-ttu-id="87dba-3025">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-3025">__By__</span></span> |    |    | <span data-ttu-id="87dba-3026">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3026">Bo</span></span> | <span data-ttu-id="87dba-3027">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3027">Bo</span></span> | <span data-ttu-id="87dba-3028">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3028">Bo</span></span> | <span data-ttu-id="87dba-3029">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3029">Bo</span></span> | <span data-ttu-id="87dba-3030">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3030">Bo</span></span> | <span data-ttu-id="87dba-3031">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3031">Bo</span></span> | <span data-ttu-id="87dba-3032">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3032">Bo</span></span> | <span data-ttu-id="87dba-3033">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3033">Bo</span></span> | <span data-ttu-id="87dba-3034">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3034">Bo</span></span> | <span data-ttu-id="87dba-3035">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3035">Bo</span></span> | <span data-ttu-id="87dba-3036">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3036">Err</span></span> | <span data-ttu-id="87dba-3037">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3037">Err</span></span> | <span data-ttu-id="87dba-3038">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3038">Bo</span></span>  | <span data-ttu-id="87dba-3039">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-3039">Ob</span></span>  | 
| <span data-ttu-id="87dba-3040">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-3040">__Sh__</span></span> |    |    |    | <span data-ttu-id="87dba-3041">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3041">Bo</span></span> | <span data-ttu-id="87dba-3042">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3042">Bo</span></span> | <span data-ttu-id="87dba-3043">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3043">Bo</span></span> | <span data-ttu-id="87dba-3044">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3044">Bo</span></span> | <span data-ttu-id="87dba-3045">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3045">Bo</span></span> | <span data-ttu-id="87dba-3046">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3046">Bo</span></span> | <span data-ttu-id="87dba-3047">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3047">Bo</span></span> | <span data-ttu-id="87dba-3048">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3048">Bo</span></span> | <span data-ttu-id="87dba-3049">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3049">Bo</span></span> | <span data-ttu-id="87dba-3050">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3050">Err</span></span> | <span data-ttu-id="87dba-3051">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3051">Err</span></span> | <span data-ttu-id="87dba-3052">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3052">Bo</span></span>  | <span data-ttu-id="87dba-3053">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-3053">Ob</span></span>  | 
| <span data-ttu-id="87dba-3054">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-3054">__US__</span></span> |    |    |    |    | <span data-ttu-id="87dba-3055">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3055">Bo</span></span> | <span data-ttu-id="87dba-3056">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3056">Bo</span></span> | <span data-ttu-id="87dba-3057">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3057">Bo</span></span> | <span data-ttu-id="87dba-3058">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3058">Bo</span></span> | <span data-ttu-id="87dba-3059">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3059">Bo</span></span> | <span data-ttu-id="87dba-3060">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3060">Bo</span></span> | <span data-ttu-id="87dba-3061">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3061">Bo</span></span> | <span data-ttu-id="87dba-3062">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3062">Bo</span></span> | <span data-ttu-id="87dba-3063">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3063">Err</span></span> | <span data-ttu-id="87dba-3064">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3064">Err</span></span> | <span data-ttu-id="87dba-3065">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3065">Bo</span></span>  | <span data-ttu-id="87dba-3066">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-3066">Ob</span></span>  | 
| <span data-ttu-id="87dba-3067">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-3067">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="87dba-3068">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3068">Bo</span></span> | <span data-ttu-id="87dba-3069">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3069">Bo</span></span> | <span data-ttu-id="87dba-3070">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3070">Bo</span></span> | <span data-ttu-id="87dba-3071">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3071">Bo</span></span> | <span data-ttu-id="87dba-3072">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3072">Bo</span></span> | <span data-ttu-id="87dba-3073">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3073">Bo</span></span> | <span data-ttu-id="87dba-3074">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3074">Bo</span></span> | <span data-ttu-id="87dba-3075">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3075">Err</span></span> | <span data-ttu-id="87dba-3076">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3076">Err</span></span> | <span data-ttu-id="87dba-3077">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3077">Bo</span></span>  | <span data-ttu-id="87dba-3078">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-3078">Ob</span></span>  | 
| <span data-ttu-id="87dba-3079">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-3079">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="87dba-3080">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3080">Bo</span></span> | <span data-ttu-id="87dba-3081">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3081">Bo</span></span> | <span data-ttu-id="87dba-3082">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3082">Bo</span></span> | <span data-ttu-id="87dba-3083">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3083">Bo</span></span> | <span data-ttu-id="87dba-3084">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3084">Bo</span></span> | <span data-ttu-id="87dba-3085">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3085">Bo</span></span> | <span data-ttu-id="87dba-3086">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3086">Err</span></span> | <span data-ttu-id="87dba-3087">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3087">Err</span></span> | <span data-ttu-id="87dba-3088">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3088">Bo</span></span>  | <span data-ttu-id="87dba-3089">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-3089">Ob</span></span>  | 
| <span data-ttu-id="87dba-3090">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-3090">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="87dba-3091">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3091">Bo</span></span> | <span data-ttu-id="87dba-3092">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3092">Bo</span></span> | <span data-ttu-id="87dba-3093">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3093">Bo</span></span> | <span data-ttu-id="87dba-3094">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3094">Bo</span></span> | <span data-ttu-id="87dba-3095">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3095">Bo</span></span> | <span data-ttu-id="87dba-3096">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3096">Err</span></span> | <span data-ttu-id="87dba-3097">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3097">Err</span></span> | <span data-ttu-id="87dba-3098">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3098">Bo</span></span>  | <span data-ttu-id="87dba-3099">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-3099">Ob</span></span>  | 
| <span data-ttu-id="87dba-3100">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-3100">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-3101">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3101">Bo</span></span> | <span data-ttu-id="87dba-3102">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3102">Bo</span></span> | <span data-ttu-id="87dba-3103">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3103">Bo</span></span> | <span data-ttu-id="87dba-3104">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3104">Bo</span></span> | <span data-ttu-id="87dba-3105">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3105">Err</span></span> | <span data-ttu-id="87dba-3106">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3106">Err</span></span> | <span data-ttu-id="87dba-3107">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3107">Bo</span></span>  | <span data-ttu-id="87dba-3108">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-3108">Ob</span></span>  | 
| <span data-ttu-id="87dba-3109">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-3109">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-3110">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3110">Bo</span></span> | <span data-ttu-id="87dba-3111">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3111">Bo</span></span> | <span data-ttu-id="87dba-3112">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3112">Bo</span></span> | <span data-ttu-id="87dba-3113">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3113">Err</span></span> | <span data-ttu-id="87dba-3114">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3114">Err</span></span> | <span data-ttu-id="87dba-3115">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3115">Bo</span></span>  | <span data-ttu-id="87dba-3116">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-3116">Ob</span></span>  | 
| <span data-ttu-id="87dba-3117">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-3117">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-3118">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3118">Bo</span></span> | <span data-ttu-id="87dba-3119">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3119">Bo</span></span> | <span data-ttu-id="87dba-3120">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3120">Err</span></span> | <span data-ttu-id="87dba-3121">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3121">Err</span></span> | <span data-ttu-id="87dba-3122">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3122">Bo</span></span>  | <span data-ttu-id="87dba-3123">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-3123">Ob</span></span>  | 
| <span data-ttu-id="87dba-3124">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-3124">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-3125">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3125">Bo</span></span> | <span data-ttu-id="87dba-3126">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3126">Err</span></span> | <span data-ttu-id="87dba-3127">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3127">Err</span></span> | <span data-ttu-id="87dba-3128">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3128">Bo</span></span>  | <span data-ttu-id="87dba-3129">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-3129">Ob</span></span>  | 
| <span data-ttu-id="87dba-3130">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-3130">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="87dba-3131">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3131">Err</span></span> | <span data-ttu-id="87dba-3132">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3132">Err</span></span> | <span data-ttu-id="87dba-3133">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3133">Err</span></span> | <span data-ttu-id="87dba-3134">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3134">Err</span></span> | 
| <span data-ttu-id="87dba-3135">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-3135">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="87dba-3136">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3136">Err</span></span> | <span data-ttu-id="87dba-3137">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3137">Err</span></span> | <span data-ttu-id="87dba-3138">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3138">Err</span></span> | 
| <span data-ttu-id="87dba-3139">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-3139">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="87dba-3140">Ur</span><span class="sxs-lookup"><span data-stu-id="87dba-3140">Bo</span></span>  | <span data-ttu-id="87dba-3141">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-3141">Ob</span></span>  | 
| <span data-ttu-id="87dba-3142">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-3142">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="87dba-3143">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-3143">Ob</span></span>  | 


## <a name="shift-operators"></a><span data-ttu-id="87dba-3144">Schiebeoperatoren</span><span class="sxs-lookup"><span data-stu-id="87dba-3144">Shift Operators</span></span>

<span data-ttu-id="87dba-3145">Die binären Operatoren `<<` und `>>` führen Bitverschiebungs Vorgänge aus.</span><span class="sxs-lookup"><span data-stu-id="87dba-3145">The binary operators `<<` and `>>` perform bit shifting operations.</span></span>

```antlr
ShiftOperatorExpression
    : Expression '<' '<' LineTerminator? Expression
    | Expression '>' '>' LineTerminator? Expression
    ;
```

<span data-ttu-id="87dba-3146">Die Operatoren sind für die Typen `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` und `Long` definiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-3146">The operators are defined for the `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` and `Long` types.</span></span> <span data-ttu-id="87dba-3147">Im Gegensatz zu den anderen binären Operatoren wird der Ergebnistyp einer Verschiebungs Operation so bestimmt, als wäre der Operator ein unärer Operator mit nur dem linken Operanden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3147">Unlike the other binary operators, the result type of a shift operation is determined as if the operator was a unary operator with just the left operand.</span></span> <span data-ttu-id="87dba-3148">Der Typ des rechten Operanden muss implizit in `Integer` konvertiert werden und wird nicht verwendet, um den Ergebnistyp des Vorgangs zu bestimmen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3148">The type of the right operand must be implicitly convertible to `Integer` and is not used in determining the result type of the operation.</span></span>

<span data-ttu-id="87dba-3149">Der `<<`-Operator bewirkt, dass die Bits im ersten Operanden um die Anzahl der durch die UMSCHALT Menge angegebenen Stellen nach links verschoben werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3149">The `<<` operator causes the bits in the first operand to be shifted left the number of places specified by the shift amount.</span></span> <span data-ttu-id="87dba-3150">Die höherwertigen Bits außerhalb des Bereichs des Ergebnis Typs werden verworfen, und die in niedriger Reihenfolge frei gewordenen Bitpositionen werden mit Nullen aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3150">The high-order bits outside the range of the result type are discarded and the low-order vacated bit positions are zero-filled.</span></span>

<span data-ttu-id="87dba-3151">Der `>>`-Operator bewirkt, dass die Bits im ersten Operanden nach rechts um die durch die UMSCHALT Menge angegebene Anzahl von Stellen verschoben werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3151">The `>>` operator causes the bits in the first operand to be shifted right the number of places specified by the shift amount.</span></span> <span data-ttu-id="87dba-3152">Die Bits mit niedriger Ordnung werden verworfen, und die in der Reihenfolge frei gewordenen Bitpositionen werden auf 0 (null) festgelegt, wenn der linke Operand positiv ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3152">The low-order bits are discarded and the high-order vacated bit positions are set to zero if the left operand is positive or to one if negative.</span></span> <span data-ttu-id="87dba-3153">Wenn der linke Operand vom Typ "`Byte`", "`UShort`", "`UInteger`" oder "`ULong`" ist, werden die leeren höherwertigen Bits mit Nullen gefüllt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3153">If the left operand is of type `Byte`, `UShort`, `UInteger`, or `ULong` the vacant high-order bits are zero-filled.</span></span>

<span data-ttu-id="87dba-3154">Die Shift-Operatoren verschieben die Bits der zugrunde liegenden Darstellung des ersten Operanden um die Menge des zweiten Operanden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3154">The shift operators shift the bits of the underlying representation of the first operand by the amount of the second operand.</span></span> <span data-ttu-id="87dba-3155">Wenn der Wert des zweiten Operanden größer als die Anzahl der Bits im ersten Operanden ist oder negativ ist, wird der UMSCHALT Betrag als `RightOperand And SizeMask` berechnet, wobei `SizeMask` ist:</span><span class="sxs-lookup"><span data-stu-id="87dba-3155">If the value of the second operand is greater than the number of bits in the first operand, or is negative, then the shift amount is computed as `RightOperand And SizeMask` where `SizeMask` is:</span></span>

| <span data-ttu-id="87dba-3156">__LeftOperand-Typ__</span><span class="sxs-lookup"><span data-stu-id="87dba-3156">__LeftOperand Type__</span></span>  | <span data-ttu-id="87dba-3157">__SizeMask__</span><span class="sxs-lookup"><span data-stu-id="87dba-3157">__SizeMask__</span></span> | 
|-----------------------|--------------|
| <span data-ttu-id="87dba-3158">`Byte`, `SByte`</span><span class="sxs-lookup"><span data-stu-id="87dba-3158">`Byte`, `SByte`</span></span>       | <span data-ttu-id="87dba-3159">7 (`&H7`)</span><span class="sxs-lookup"><span data-stu-id="87dba-3159">7 (`&H7`)</span></span>    | 
| <span data-ttu-id="87dba-3160">`UShort`, `Short`</span><span class="sxs-lookup"><span data-stu-id="87dba-3160">`UShort`, `Short`</span></span>     | <span data-ttu-id="87dba-3161">15 (`&HF`)</span><span class="sxs-lookup"><span data-stu-id="87dba-3161">15 (`&HF`)</span></span>   | 
| <span data-ttu-id="87dba-3162">`UInteger`, `Integer`</span><span class="sxs-lookup"><span data-stu-id="87dba-3162">`UInteger`, `Integer`</span></span> | <span data-ttu-id="87dba-3163">31 (`&H1F`)</span><span class="sxs-lookup"><span data-stu-id="87dba-3163">31 (`&H1F`)</span></span>  | 
| <span data-ttu-id="87dba-3164">`ULong`, `Long`</span><span class="sxs-lookup"><span data-stu-id="87dba-3164">`ULong`, `Long`</span></span>       | <span data-ttu-id="87dba-3165">63 (`&H3F`)</span><span class="sxs-lookup"><span data-stu-id="87dba-3165">63 (`&H3F`)</span></span>  | 

<span data-ttu-id="87dba-3166">Wenn die UMSCHALT Menge 0 (null) ist, ist das Ergebnis des Vorgangs mit dem Wert des ersten Operanden identisch.</span><span class="sxs-lookup"><span data-stu-id="87dba-3166">If the shift amount is zero, the result of the operation is identical to the value of the first operand.</span></span> <span data-ttu-id="87dba-3167">Von diesen Vorgängen können keine über Flüsse durchlaufen werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3167">No overflows are possible from these operations.</span></span>

<span data-ttu-id="87dba-3168">__Vorgangstyp:__</span><span class="sxs-lookup"><span data-stu-id="87dba-3168">__Operation Type:__</span></span>


| <span data-ttu-id="87dba-3169">__Ur__</span><span class="sxs-lookup"><span data-stu-id="87dba-3169">__Bo__</span></span> | <span data-ttu-id="87dba-3170">__SB__</span><span class="sxs-lookup"><span data-stu-id="87dba-3170">__SB__</span></span> | <span data-ttu-id="87dba-3171">__Am__</span><span class="sxs-lookup"><span data-stu-id="87dba-3171">__By__</span></span> | <span data-ttu-id="87dba-3172">__Sh__</span><span class="sxs-lookup"><span data-stu-id="87dba-3172">__Sh__</span></span> | <span data-ttu-id="87dba-3173">__VORLIEGENDEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-3173">__US__</span></span> | <span data-ttu-id="87dba-3174">__In__</span><span class="sxs-lookup"><span data-stu-id="87dba-3174">__In__</span></span> | <span data-ttu-id="87dba-3175">__ANGETAN__</span><span class="sxs-lookup"><span data-stu-id="87dba-3175">__UI__</span></span> | <span data-ttu-id="87dba-3176">__Lo__</span><span class="sxs-lookup"><span data-stu-id="87dba-3176">__Lo__</span></span> | <span data-ttu-id="87dba-3177">__NÜTZLICHEN__</span><span class="sxs-lookup"><span data-stu-id="87dba-3177">__UL__</span></span> | <span data-ttu-id="87dba-3178">__Liga__</span><span class="sxs-lookup"><span data-stu-id="87dba-3178">__De__</span></span> | <span data-ttu-id="87dba-3179">__Si__</span><span class="sxs-lookup"><span data-stu-id="87dba-3179">__Si__</span></span> | <span data-ttu-id="87dba-3180">__Do__</span><span class="sxs-lookup"><span data-stu-id="87dba-3180">__Do__</span></span> | <span data-ttu-id="87dba-3181">__De__</span><span class="sxs-lookup"><span data-stu-id="87dba-3181">__Da__</span></span>  | <span data-ttu-id="87dba-3182">__Ch__</span><span class="sxs-lookup"><span data-stu-id="87dba-3182">__Ch__</span></span>  | <span data-ttu-id="87dba-3183">__St__</span><span class="sxs-lookup"><span data-stu-id="87dba-3183">__St__</span></span> | <span data-ttu-id="87dba-3184">__Ob__</span><span class="sxs-lookup"><span data-stu-id="87dba-3184">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="87dba-3185">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-3185">Sh</span></span> | <span data-ttu-id="87dba-3186">SB</span><span class="sxs-lookup"><span data-stu-id="87dba-3186">SB</span></span> | <span data-ttu-id="87dba-3187">um</span><span class="sxs-lookup"><span data-stu-id="87dba-3187">By</span></span> | <span data-ttu-id="87dba-3188">Sh</span><span class="sxs-lookup"><span data-stu-id="87dba-3188">Sh</span></span> | <span data-ttu-id="87dba-3189">US</span><span class="sxs-lookup"><span data-stu-id="87dba-3189">US</span></span> | <span data-ttu-id="87dba-3190">In</span><span class="sxs-lookup"><span data-stu-id="87dba-3190">In</span></span> | <span data-ttu-id="87dba-3191">UI</span><span class="sxs-lookup"><span data-stu-id="87dba-3191">UI</span></span> | <span data-ttu-id="87dba-3192">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-3192">Lo</span></span> | <span data-ttu-id="87dba-3193">UL</span><span class="sxs-lookup"><span data-stu-id="87dba-3193">UL</span></span> | <span data-ttu-id="87dba-3194">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-3194">Lo</span></span> | <span data-ttu-id="87dba-3195">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-3195">Lo</span></span> | <span data-ttu-id="87dba-3196">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-3196">Lo</span></span> | <span data-ttu-id="87dba-3197">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3197">Err</span></span> | <span data-ttu-id="87dba-3198">Err</span><span class="sxs-lookup"><span data-stu-id="87dba-3198">Err</span></span> | <span data-ttu-id="87dba-3199">Lo</span><span class="sxs-lookup"><span data-stu-id="87dba-3199">Lo</span></span> | <span data-ttu-id="87dba-3200">Johannis</span><span class="sxs-lookup"><span data-stu-id="87dba-3200">Ob</span></span> | 


## <a name="boolean-expressions"></a><span data-ttu-id="87dba-3201">Boolesche Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-3201">Boolean Expressions</span></span>

<span data-ttu-id="87dba-3202">Ein boolescher Ausdruck ist ein Ausdruck, der getestet werden kann, um festzustellen, ob er true ist, oder ob er false ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3202">A Boolean expression is an expression that can be tested to see if it is true or if it is false.</span></span>

```antlr
BooleanExpression
    : Expression
    ;
```

<span data-ttu-id="87dba-3203">Ein Typ `T` kann in einem booleschen Ausdruck verwendet werden, wenn in der Reihenfolge der bevorzugte Werte:</span><span class="sxs-lookup"><span data-stu-id="87dba-3203">A type `T` can be used in a Boolean expression if, in order of preference:</span></span>

* <span data-ttu-id="87dba-3204">`T` ist `Boolean` oder `Boolean?`</span><span class="sxs-lookup"><span data-stu-id="87dba-3204">`T` is `Boolean` or `Boolean?`</span></span>

* <span data-ttu-id="87dba-3205">`T` hat eine erweiternde Konvertierung in `Boolean`</span><span class="sxs-lookup"><span data-stu-id="87dba-3205">`T` has a widening conversion to `Boolean`</span></span>

* <span data-ttu-id="87dba-3206">`T` hat eine erweiternde Konvertierung in `Boolean?`</span><span class="sxs-lookup"><span data-stu-id="87dba-3206">`T` has a widening conversion to `Boolean?`</span></span>

* <span data-ttu-id="87dba-3207">`T` definiert zwei Pseudo Operatoren, `IsTrue` und `IsFalse`.</span><span class="sxs-lookup"><span data-stu-id="87dba-3207">`T` defines two pseudo operators, `IsTrue` and `IsFalse`.</span></span>

* <span data-ttu-id="87dba-3208">`T` hat eine einschränkende Konvertierung in `Boolean?`, bei der keine Konvertierung von `Boolean` in `Boolean?` beteiligt ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3208">`T` has a narrowing conversion to `Boolean?` that does not involve a conversion from `Boolean` to `Boolean?`.</span></span>

* <span data-ttu-id="87dba-3209">`T` hat eine einschränkende Konvertierung in `Boolean`.</span><span class="sxs-lookup"><span data-stu-id="87dba-3209">`T` has a narrowing conversion to `Boolean`.</span></span>

<span data-ttu-id="87dba-3210">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-3210">__Note.__</span></span> <span data-ttu-id="87dba-3211">Beachten Sie, dass wenn `Option Strict` deaktiviert ist, ein Ausdruck, der eine einschränkende Konvertierung in `Boolean` aufweist, ohne Kompilierzeitfehler akzeptiert wird, die Sprache jedoch immer noch einen `IsTrue`-Operator bevorzugt, wenn dieser vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3211">It is interesting to note that if `Option Strict` is off, an expression that has a narrowing conversion to `Boolean` will be accepted without a compile-time error but the language will still prefer an `IsTrue` operator if it exists.</span></span> <span data-ttu-id="87dba-3212">Der Grund hierfür ist, dass `Option Strict` nur ändert, was von der Sprache nicht akzeptiert wird, und die tatsächliche Bedeutung eines Ausdrucks nie ändert.</span><span class="sxs-lookup"><span data-stu-id="87dba-3212">This is because `Option Strict` only changes what is and isn't accepted by the language, and never changes the actual meaning of an expression.</span></span> <span data-ttu-id="87dba-3213">Daher muss `IsTrue` unabhängig von `Option Strict` immer über eine einschränkende Konvertierung bevorzugt werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3213">Thus, `IsTrue` has to always be preferred over a narrowing conversion, regardless of `Option Strict`.</span></span>

<span data-ttu-id="87dba-3214">Die folgende Klasse definiert z. b. keine erweiternde Konvertierung in `Boolean`.</span><span class="sxs-lookup"><span data-stu-id="87dba-3214">For example, the following class does not define a widening conversion to `Boolean`.</span></span> <span data-ttu-id="87dba-3215">Folglich verursacht die Verwendung in der `If`-Anweisung einen aufzurufenden Operator `IsTrue`.</span><span class="sxs-lookup"><span data-stu-id="87dba-3215">As a result, its use in the `If` statement causes a call to the `IsTrue` operator.</span></span>

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

<span data-ttu-id="87dba-3216">Wenn ein boolescher Ausdruck in `Boolean` oder `Boolean?` eingegeben oder konvertiert wird, ist es true, wenn der Wert `True` ist, und andernfalls false.</span><span class="sxs-lookup"><span data-stu-id="87dba-3216">If a Boolean expression is typed as or converted to `Boolean` or `Boolean?`, then it is true if the value is `True` and false otherwise.</span></span>

<span data-ttu-id="87dba-3217">Andernfalls ruft ein boolescher Ausdruck den `IsTrue`-Operator auf und gibt `True` zurück, wenn der Operator `True`; zurückgegeben hat. Andernfalls ist Sie false (der `IsFalse`-Operator wird jedoch nie aufgerufen).</span><span class="sxs-lookup"><span data-stu-id="87dba-3217">Otherwise, a Boolean expression calls the `IsTrue` operator and returns `True` if the operator returned `True`; otherwise it is false (but never calls the `IsFalse` operator).</span></span>

<span data-ttu-id="87dba-3218">Im folgenden Beispiel verfügt `Integer` über eine einschränkende Konvertierung in `Boolean`, sodass eine NULL-`Integer?` eine einschränkende Konvertierung in `Boolean?` (ergibt einen NULL-Wert `Boolean`) und `Boolean` (wodurch eine Ausnahme ausgelöst wird) aufweist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3218">In the following example, `Integer` has a narrowing conversion to `Boolean`, so a null `Integer?` has a narrowing conversion to both `Boolean?` (yielding a null `Boolean`) and to `Boolean` (which throws an exception).</span></span> <span data-ttu-id="87dba-3219">Die einschränkende Konvertierung in `Boolean?` wird bevorzugt. Daher ist der Wert von "`i`" als boolescher Ausdruck `False`.</span><span class="sxs-lookup"><span data-stu-id="87dba-3219">The narrowing conversion to `Boolean?` is preferred, and so the value of "`i`" as a Boolean expression is therefore `False`.</span></span>

```vb
Dim i As Integer? = Nothing
If i Then Console.WriteLine()
```


## <a name="lambda-expressions"></a><span data-ttu-id="87dba-3220">Lambda-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-3220">Lambda Expressions</span></span>

<span data-ttu-id="87dba-3221">Ein *Lambda-Ausdruck* definiert eine anonyme Methode, die als *Lambda-Methode*bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-3221">A *lambda expression* defines an anonymous method called a *lambda method*.</span></span> <span data-ttu-id="87dba-3222">Mit Lambda-Methoden können "Inline"-Methoden einfach an andere Methoden übergeben werden, die Delegattypen akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="87dba-3222">Lambda methods make it easy to pass "in-line" methods to other methods that take delegate types.</span></span>

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

<span data-ttu-id="87dba-3223">Das Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3223">The example:</span></span>

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

<span data-ttu-id="87dba-3224">druckt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="87dba-3224">will print out:</span></span>

```console
2 4 6 8
```

<span data-ttu-id="87dba-3225">Ein Lambda-Ausdruck beginnt mit den optionalen modifiziermetern `Async` oder `Iterator`, gefolgt vom-Schlüsselwort `Function` oder `Sub` und einer Parameterliste.</span><span class="sxs-lookup"><span data-stu-id="87dba-3225">A lambda expression begins with the optional modifiers `Async` or `Iterator`, followed by the keyword `Function` or `Sub` and a parameter list.</span></span> <span data-ttu-id="87dba-3226">Parameter in einem Lambda-Ausdruck können nicht als `Optional` oder `ParamArray` deklariert werden und können keine Attribute aufweisen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3226">Parameters in a lambda expression cannot be declared `Optional` or `ParamArray` and cannot have attributes.</span></span> <span data-ttu-id="87dba-3227">Anders als bei regulären Methoden wird durch das Weglassen eines Parameter Typs für eine Lambda-Methode nicht automatisch `Object` abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="87dba-3227">Unlike regular methods, omitting a parameter type for a lambda method does not automatically infer `Object`.</span></span> <span data-ttu-id="87dba-3228">Stattdessen werden bei der Neuklassifizierung einer Lambda-Methode die ausgelassenen Parametertypen und `ByRef`-Modifizierer aus dem Zieltyp abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="87dba-3228">Instead, when a lambda method is reclassified, the omitted parameter types and `ByRef` modifiers are inferred from the target type.</span></span> <span data-ttu-id="87dba-3229">Im vorherigen Beispiel hätte der Lambda-Ausdruck möglicherweise als `Function(x) x * 2` geschrieben worden, und der Typ von `x` muss `Integer` abgeleitet werden, wenn die Lambda-Methode verwendet wurde, um eine Instanz des `IntFunc`-Delegattyps zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3229">In the previous example, the lambda expression could have been written as `Function(x) x * 2`, and it would have inferred the type of `x` to be `Integer` when the lambda method was used to create an instance of the `IntFunc` delegate type.</span></span> <span data-ttu-id="87dba-3230">Anders als bei der lokalen Variablen Ableitung tritt ein Kompilierzeitfehler auf, wenn ein Lambda-Methoden Parameter einen Typ auslässt, aber über ein Array oder einen änderungsmodifizierer mit Nullwert verfügt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3230">Unlike local variable inference, if a lambda method parameter omits a type but has an array or nullable name modifier, a compile-time error occurs.</span></span>

<span data-ttu-id="87dba-3231">Ein __regulärer Lambda-Ausdruck__ ist ein Ausdruck, der weder `Async` noch `Iterator`-Modifizierer ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3231">A __regular lambda expression__ is one with neither `Async` nor `Iterator` modifiers.</span></span>

<span data-ttu-id="87dba-3232">Ein __iteratorlambda-Ausdruck__ ist ein Ausdruck mit dem `Iterator`-Modifizierer und kein `Async`-Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="87dba-3232">An __iterator lambda expression__ is one with the `Iterator` modifier and no `Async` modifier.</span></span> <span data-ttu-id="87dba-3233">Es muss eine Funktion sein.</span><span class="sxs-lookup"><span data-stu-id="87dba-3233">It must be a function.</span></span> <span data-ttu-id="87dba-3234">Wenn ein Wert neu klassifiziert wird, kann er nur zu einem Wert des Delegattyps neu klassifiziert werden, dessen Rückgabetyp `IEnumerator` oder `IEnumerable` oder `IEnumerator(Of T)` oder `IEnumerable(Of T)` für einige `T` ist und der keine ByRef-Parameter aufweist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3234">When it is reclassified to a value, it can only be reclassified to a value of delegate type whose return type is `IEnumerator`, or `IEnumerable`, or `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`, and which has no ByRef parameters.</span></span>

<span data-ttu-id="87dba-3235">Ein asynchroner __Lambda-Ausdruck__ ist ein Ausdruck mit dem `Async`-Modifizierer und kein `Iterator`-Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="87dba-3235">An __async lambda expression__ is one with the `Async` modifier and no `Iterator` modifier.</span></span> <span data-ttu-id="87dba-3236">Ein Async-Sub-Lambda kann nur in einen Wert des subdelegattyps ohne ByRef-Parameter neu klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3236">An async sub lambda may only be reclassified to a value of sub delegate type with no ByRef parameters.</span></span> <span data-ttu-id="87dba-3237">Ein asynchroner funktionslambda kann nur in einen Wert des funktionsdelegattyps neu klassifiziert werden, dessen Rückgabetyp `Task` oder `Task(Of T)` für einige `T` ist und der keine ByRef-Parameter aufweist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3237">An async function lambda may only be reclassified to a value of function delegate type whose return type is `Task` or `Task(Of T)` for some `T`, and which has no ByRef parameters.</span></span>

<span data-ttu-id="87dba-3238">Lambda Ausdrücke können entweder einzeilige oder mehrzeilige Zeilen sein.</span><span class="sxs-lookup"><span data-stu-id="87dba-3238">Lambda expressions can either be single-line or multi-line.</span></span> <span data-ttu-id="87dba-3239">Einzeilige `Function`-Lambda Ausdrücke enthalten einen einzelnen Ausdruck, der den von der Lambda-Methode zurückgegebenen Wert darstellt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3239">Single-line `Function` lambda expressions contain a single expression that represents the value returned from the lambda method.</span></span> <span data-ttu-id="87dba-3240">Einzeilige `Sub`-Lambda Ausdrücke enthalten eine einzelne Anweisung ohne die schließende `StatementTerminator`.</span><span class="sxs-lookup"><span data-stu-id="87dba-3240">Single-line `Sub` lambda expressions contain a single statement without its closing `StatementTerminator`.</span></span> <span data-ttu-id="87dba-3241">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3241">For example:</span></span>

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

<span data-ttu-id="87dba-3242">Einzeilige Lambda-Konstrukte binden weniger eng als alle anderen Ausdrücke und Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3242">Single-line lambda constructs bind less tightly than all other expressions and statements.</span></span> <span data-ttu-id="87dba-3243">So entspricht beispielsweise "`Function() x + 5`" "`Function() (x+5)"` anstelle von" `(Function() x) + 5` ".</span><span class="sxs-lookup"><span data-stu-id="87dba-3243">Thus, for example, "`Function() x + 5`" is equivalent to "`Function() (x+5)"` rather than "`(Function() x) + 5`".</span></span> <span data-ttu-id="87dba-3244">Um Mehrdeutigkeit zu vermeiden, darf ein einzeilige `Sub`-Lambda-Ausdruck keine Dim-Anweisung oder eine Bezeichnungs Deklarations Anweisung enthalten.</span><span class="sxs-lookup"><span data-stu-id="87dba-3244">To avoid ambiguity, a single-line `Sub` lambda expression may not contain a Dim statement or a label declaration statement.</span></span> <span data-ttu-id="87dba-3245">Auch wenn es nicht in Klammern eingeschlossen ist, darf ein Einzeiger `Sub`-Lambda-Ausdruck nicht unmittelbar auf einen Doppelpunkt (":"), einen Member Access-Operator ".", einen Wörterbuch-Member-Zugriffs Operator "!" oder eine öffnende Klammer "(" folgen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3245">Also, unless it is enclosed in parentheses, a single-line `Sub` lambda expression may not be immediately followed by a colon ":", a member access operator ".", a dictionary member access operator "!" or an open parenthesis "(".</span></span> <span data-ttu-id="87dba-3246">Sie darf keine Block-Anweisung (`With`, `SyncLock, If...EndIf`, `While`, `For`, `Do`, `Using`) und `OnError` oder `Resume` enthalten.</span><span class="sxs-lookup"><span data-stu-id="87dba-3246">It may not contain any block statement (`With`, `SyncLock, If...EndIf`, `While`, `For`, `Do`, `Using`) nor `OnError` nor `Resume`.</span></span>

<span data-ttu-id="87dba-3247">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-3247">__Note.__</span></span> <span data-ttu-id="87dba-3248">Im Lambda-Ausdruck `Function(i) x=i` wird der Text als *Ausdruck* interpretiert (der testet, ob `x` und `i` gleich sind).</span><span class="sxs-lookup"><span data-stu-id="87dba-3248">In the lambda expression `Function(i) x=i`, the body is interpreted as an *expression* (which tests whether `x` and `i` are equal).</span></span> <span data-ttu-id="87dba-3249">Im Lambda-Ausdruck `Sub(i) x=i` wird der Text jedoch als-Anweisung interpretiert (die `i` zu `x` zuweist).</span><span class="sxs-lookup"><span data-stu-id="87dba-3249">But in the lambda expression `Sub(i) x=i`, the body is interpreted as a statement (which assigns `i` to `x`).</span></span>

<span data-ttu-id="87dba-3250">Ein mehrzeilige Lambda-Ausdruck enthält einen Anweisungsblock und muss mit einer entsprechenden `End`-Anweisung (d. h. `End Function` oder `End Sub`) enden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3250">A multi-line lambda expression contains a statement block and must end with an appropriate `End` statement (i.e. `End Function` or `End Sub`).</span></span> <span data-ttu-id="87dba-3251">Wie bei regulären Methoden müssen sich die `Function`-oder `Sub`-Anweisung einer mehrzeiligen Lambda-Methode und die `End`-Anweisungen in ihren eigenen Zeilen befinden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3251">As with regular methods, a multi-line lambda method's `Function` or `Sub` statement and `End` statements must be on their own lines.</span></span> <span data-ttu-id="87dba-3252">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3252">For example:</span></span>

```vb
' Error: Function statement must be on its own line!
Dim x = Sub(x As Integer) : Console.WriteLine(x) : End Sub

' OK
Dim y = Sub(x As Integer)
               Console.WriteLine(x)
          End Sub
```

<span data-ttu-id="87dba-3253">Mehrzeilige `Function`-Lambda Ausdrücke können einen Rückgabetyp deklarieren, aber keine Attribute darauf ablegen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3253">Multi-line `Function` lambda expressions can declare a return type but cannot put attributes on it.</span></span> <span data-ttu-id="87dba-3254">Wenn ein mehrzeilige `Function`-Lambda Ausdruck keinen Rückgabetyp deklariert, aber der Rückgabetyp aus dem Kontext abgeleitet werden kann, in dem der Lambda-Ausdruck verwendet wird, wird dieser Rückgabetyp verwendet.</span><span class="sxs-lookup"><span data-stu-id="87dba-3254">If a multi-line `Function` lambda expression does not declare a return type but the return type can be inferred from the context in which the lambda expression is used , then that return type is used.</span></span> <span data-ttu-id="87dba-3255">Andernfalls wird der Rückgabetyp der Funktion wie folgt berechnet:</span><span class="sxs-lookup"><span data-stu-id="87dba-3255">Otherwise the return type of the function is calculated as follows:</span></span>

* <span data-ttu-id="87dba-3256">In einem regulären Lambda-Ausdruck ist der Rückgabetyp der vorherrschende Typ der Ausdrücke in allen `Return`-Anweisungen im Anweisungsblock.</span><span class="sxs-lookup"><span data-stu-id="87dba-3256">In a regular lambda expression, the return type is the dominant type of the expressions in all the `Return` statements in the statement block.</span></span>

* <span data-ttu-id="87dba-3257">In einem asynchronen Lambda-Ausdruck ist der Rückgabetyp `Task(Of T)`, wobei `T` der vorherrschende Typ der Ausdrücke in allen `Return`-Anweisungen im Anweisungsblock ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3257">In an async lambda expression, the return type is `Task(Of T)` where `T` is the dominant type of the expressions in all the `Return` statements in the statement block.</span></span>

* <span data-ttu-id="87dba-3258">In einem iteratorlambda-Ausdruck ist der Rückgabetyp `IEnumerable(Of T)`, wobei `T` der vorherrschende Typ der Ausdrücke in allen `Yield`-Anweisungen im Anweisungsblock ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3258">In an iterator lambda expression, the return type is `IEnumerable(Of T)` where `T` is the dominant type of the expressions in all the `Yield` statements in the statement block.</span></span>

<span data-ttu-id="87dba-3259">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3259">For example:</span></span>

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

<span data-ttu-id="87dba-3260">In allen Fällen, wenn keine `Return`-Anweisungen (bzw. `Yield`) vorhanden sind, oder wenn es keinen vorherrschenden Typ gibt und eine strikte Semantik verwendet wird, tritt ein Kompilierzeitfehler auf. Andernfalls ist der vorherrschende Typ implizit `Object`.</span><span class="sxs-lookup"><span data-stu-id="87dba-3260">In all cases, if there are no `Return` (respectively `Yield`) statements, or if there is no dominant type among them, and strict semantics are being used, a compile-time error occurs; otherwise the dominant type is implicitly `Object`.</span></span>

<span data-ttu-id="87dba-3261">Beachten Sie, dass der Rückgabetyp von allen `Return`-Anweisungen berechnet wird, auch wenn Sie nicht erreichbar sind.</span><span class="sxs-lookup"><span data-stu-id="87dba-3261">Note that the return type is calculated from all `Return` statements, even if they are not reachable.</span></span> <span data-ttu-id="87dba-3262">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3262">For example:</span></span>

```vb
' Return type is Double
Dim x = Function()
              Return 10
               Return 10.50
          End Function
```

<span data-ttu-id="87dba-3263">Es gibt keine implizite Rückgabe Variable, da kein Name für die Variable vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3263">There is no implicit return variable, as there is no name for the variable.</span></span>

<span data-ttu-id="87dba-3264">Die-Anweisungsblöcke in mehrzeiligen Lambda-Ausdrücken weisen die folgenden Einschränkungen auf:</span><span class="sxs-lookup"><span data-stu-id="87dba-3264">The statement blocks inside multi-line lambda expressions have the following restrictions:</span></span>

* <span data-ttu-id="87dba-3265">die Anweisungen `On Error` und `Resume` sind nicht zulässig, obwohl `Try`-Anweisungen zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="87dba-3265">`On Error` and `Resume` statements are not allowed, although `Try` statements are allowed.</span></span>

* <span data-ttu-id="87dba-3266">Statische lokale Variablen können nicht in mehrzeiligen Lambda Ausdrücken deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3266">Static locals cannot be declared in multi-line lambda expressions.</span></span>

* <span data-ttu-id="87dba-3267">Es ist nicht möglich, in einen oder aus dem Anweisungsblock eines mehrzeiligen Lambda-Ausdrucks zu verzweigen, obwohl die normalen Verzweigungs Regeln darin zutreffen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3267">It is not possible to branch into or out of the statement block of a multi-line lambda expression, although the normal branching rules apply within it.</span></span> <span data-ttu-id="87dba-3268">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3268">For example:</span></span>

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

<span data-ttu-id="87dba-3269">Ein Lambda-Ausdruck entspricht ungefähr einer anonymen Methode, die für den enthaltenden Typ deklariert wurde.</span><span class="sxs-lookup"><span data-stu-id="87dba-3269">A lambda expression is roughly equivalent to an anonymous method declared on the containing type.</span></span> <span data-ttu-id="87dba-3270">Das ursprüngliche Beispiel entspricht ungefähr dem folgenden:</span><span class="sxs-lookup"><span data-stu-id="87dba-3270">The initial example is roughly equivalent to:</span></span>

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


### <a name="closures"></a><span data-ttu-id="87dba-3271">Accoun</span><span class="sxs-lookup"><span data-stu-id="87dba-3271">Closures</span></span>

<span data-ttu-id="87dba-3272">Lambda-Ausdrücke haben Zugriff auf alle Variablen im Gültigkeitsbereich, einschließlich lokaler Variablen oder Parameter, die in der enthaltenden Methode und in Lambda Ausdrücken definiert sind.</span><span class="sxs-lookup"><span data-stu-id="87dba-3272">Lambda expressions have access to all of the variables in scope, including local variables or parameters defined in the containing method and lambda expressions.</span></span> <span data-ttu-id="87dba-3273">Wenn ein Lambda-Ausdruck auf eine lokale Variable oder einen lokalen Parameter verweist, erfasst der Lambda-Ausdruck die Variable, auf die in einen Abschluss verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-3273">When a lambda expression refers to a local variable or parameter, the lambda expression captures the variable being referred to into a closure.</span></span> <span data-ttu-id="87dba-3274">Ein Closure ist ein Objekt, das sich nicht auf dem Stapel, sondern auf dem Heap befindet. Wenn eine Variable aufgezeichnet wird, werden alle Verweise auf die Variable an den Abschluss umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="87dba-3274">A closure is an object that lives on the heap instead of on the stack, and when a variable is captured, all references to the variable are redirected to the closure.</span></span> <span data-ttu-id="87dba-3275">Dies ermöglicht es Lambda-Ausdrücken, weiterhin auf lokale Variablen und Parameter zu verweisen, auch nachdem die enthaltende Methode fertig ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3275">This enables lambda expressions to continue to refer to local variables and parameters even after the containing method is complete.</span></span> <span data-ttu-id="87dba-3276">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3276">For example:</span></span>

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

<span data-ttu-id="87dba-3277">ist ungefähr Äquivalent zu:</span><span class="sxs-lookup"><span data-stu-id="87dba-3277">is roughly equivalent to:</span></span>

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

<span data-ttu-id="87dba-3278">Bei einem Abschluss wird eine neue Kopie einer lokalen Variablen aufgezeichnet, wenn Sie in den Block eintritt, in dem die lokale Variable deklariert ist, aber die neue Kopie wird mit dem Wert der vorherigen Kopie initialisiert, sofern vorhanden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3278">A closure captures a new copy of a local variable each time it enters the block in which the local variable is declared, but the new copy is initialized with the value of the previous copy, if any.</span></span> <span data-ttu-id="87dba-3279">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3279">For example:</span></span>

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

<span data-ttu-id="87dba-3280">druckt</span><span class="sxs-lookup"><span data-stu-id="87dba-3280">prints</span></span>

```console
1 2 3 4 5 6 7 8 9 10
```

<span data-ttu-id="87dba-3281">Statt</span><span class="sxs-lookup"><span data-stu-id="87dba-3281">instead of</span></span>

```console
9 9 9 9 9 9 9 9 9 9
```

<span data-ttu-id="87dba-3282">Da bei der Eingabe eines-Blocks die-Abschlüsse initialisiert werden müssen, ist es nicht zulässig, 0 (null) in einen-Block mit einem Closure von außerhalb dieses Blocks zu @no__t, obwohl es zulässig ist,-1 in einen Block mit einer Schließung zu @no__t.</span><span class="sxs-lookup"><span data-stu-id="87dba-3282">Because closures have to be initialized when entering a block, it is not allowed to `GoTo` into a block with a closure from outside of that block, although it is allowed to `Resume` into a block with a closure.</span></span> <span data-ttu-id="87dba-3283">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3283">For example:</span></span>

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

<span data-ttu-id="87dba-3284">Da Sie nicht in einem Closure aufgezeichnet werden können, kann Folgendes nicht in einem Lambda-Ausdruck vorkommen:</span><span class="sxs-lookup"><span data-stu-id="87dba-3284">Because they cannot be captured into a closure, the following cannot appear inside of a lambda expression:</span></span>

* <span data-ttu-id="87dba-3285">Verweis Parameter.</span><span class="sxs-lookup"><span data-stu-id="87dba-3285">Reference parameters.</span></span>

* <span data-ttu-id="87dba-3286">Instanzausdrücke (`Me`, `MyClass`, `MyBase`), wenn der Typ von `Me` keine Klasse ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3286">Instance expressions (`Me`, `MyClass`, `MyBase`), if the type of `Me` is not a class.</span></span>

<span data-ttu-id="87dba-3287">Die Member eines anonymen typerstellungs Ausdrucks, wenn der Lambda-Ausdruck Teil des Ausdrucks ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3287">The members of an anonymous type-creation expression, if the lambda expression is part of the expression.</span></span> <span data-ttu-id="87dba-3288">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3288">For example:</span></span>

```vb
' Error: Lambda cannot refer to anonymous type field
Dim x = New With { .a = 12, .b = Function() .a }
```

<span data-ttu-id="87dba-3289">`ReadOnly`-Instanzvariablen in Instanzkonstruktoren oder `ReadOnly` freigegebene Variablen in freigegebenen Konstruktoren, bei denen die Variablen in einem nicht-Wert-Kontext verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3289">`ReadOnly` instance variables in instance constructors or `ReadOnly` shared variables in shared constructors where the variables are used in a non-value context.</span></span> <span data-ttu-id="87dba-3290">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3290">For example:</span></span>

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

## <a name="query-expressions"></a><span data-ttu-id="87dba-3291">Abfrageausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-3291">Query Expressions</span></span>

<span data-ttu-id="87dba-3292">Ein *Abfrage Ausdruck* ist ein Ausdruck, *der eine Reihe* von *Abfrage Operatoren* auf die Elemente einer abfragbaren Auflistung anwendet.</span><span class="sxs-lookup"><span data-stu-id="87dba-3292">A *query expression* is an expression that applies a series of *query operators* to the elements of a *queryable* collection.</span></span> <span data-ttu-id="87dba-3293">Der folgende Ausdruck nimmt z. b. eine Auflistung von `Customer`-Objekten an und gibt die Namen aller Kunden im Bundesstaat Washington zurück:</span><span class="sxs-lookup"><span data-stu-id="87dba-3293">For example, the following expression takes a collection of `Customer` objects and returns the names of all the customers in the state of Washington:</span></span>

```vb
Dim names = _
    From cust In Customers _
    Where cust.State = "WA" _
    Select cust.Name
```

<span data-ttu-id="87dba-3294">Ein Abfrage Ausdruck muss mit einem `From`-oder einem `Aggregate`-Operator beginnen und mit einem beliebigen Abfrage Operator enden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3294">A query expression must start with a `From` or an `Aggregate` operator and can end with any query operator.</span></span> <span data-ttu-id="87dba-3295">Das Ergebnis eines Abfrage Ausdrucks wird als Wert klassifiziert. der Ergebnistyp des Ausdrucks hängt vom Ergebnistyp des letzten Abfrage Operators im Ausdruck ab.</span><span class="sxs-lookup"><span data-stu-id="87dba-3295">The result of a query expression is classified as a value; the result type of the expression depends on the result type of the last query operator in the expression.</span></span>

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

### <a name="range-variables"></a><span data-ttu-id="87dba-3296">Bereichs Variablen</span><span class="sxs-lookup"><span data-stu-id="87dba-3296">Range Variables</span></span>

<span data-ttu-id="87dba-3297">Einige Abfrage Operatoren führen eine besondere Art von Variablen ein, die als *Bereichs Variable*bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-3297">Some query operators introduce a special kind of variable called a *range variable*.</span></span> <span data-ttu-id="87dba-3298">Bereichs Variablen sind keine echten Variablen. Stattdessen stellen Sie die einzelnen Werte während der Auswertung der Abfrage für die Eingabe Auflistungen dar.</span><span class="sxs-lookup"><span data-stu-id="87dba-3298">Range variables are not real variables; instead, they represent the individual values during the evaluation of the query over the input collections.</span></span>

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

<span data-ttu-id="87dba-3299">Bereichs Variablen werden vom Introducing Query-Operator bis zum Ende eines Abfrage Ausdrucks oder bis zu einem Abfrage Operator, z. b. `Select`, der Sie verbirgt, festgelegt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3299">Range variables are scoped from the introducing query operator to the end of a query expression, or to a query operator such as `Select` that hides them.</span></span> <span data-ttu-id="87dba-3300">Beispielsweise in der folgenden Abfrage:</span><span class="sxs-lookup"><span data-stu-id="87dba-3300">For example, in the following query</span></span>

```vb
Dim waCusts = _
    From cust As Customer In Customers _
    Where cust.State = "WA"
```

<span data-ttu-id="87dba-3301">der `From`-Abfrage Operator führt eine Bereichs Variable `cust` ein, die als `Customer` typisiert ist, die jeden Kunden in der `Customers`-Auflistung darstellt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3301">the `From` query operator introduces a range variable `cust` typed as `Customer` that represents each customer in the `Customers` collection.</span></span> <span data-ttu-id="87dba-3302">Der folgende `Where`-Abfrage Operator verweist dann auf die Bereichs Variable `cust` im Filter Ausdruck, um zu bestimmen, ob ein einzelner Kunde aus der resultierenden Auflistung gefiltert werden soll.</span><span class="sxs-lookup"><span data-stu-id="87dba-3302">The following `Where` query operator then refers to the range variable `cust` in the filter expression to determine whether to filter an individual customer out of the resulting collection.</span></span>

<span data-ttu-id="87dba-3303">Es gibt zwei Typen von Bereichs Variablen: Auflistungs *Bereichs Variablen* und *Ausdrucks Bereichs Variablen*.</span><span class="sxs-lookup"><span data-stu-id="87dba-3303">There are two types of range variables: *collection range variables* and *expression range variables*.</span></span> <span data-ttu-id="87dba-3304">Sammlungs Bereichs Variablen übernehmen ihre Werte aus den Elementen der Auflistungen, die abgefragt werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3304">Collection range variables take their values from the elements of the collections being queried.</span></span> <span data-ttu-id="87dba-3305">Der Auflistungs Ausdruck in einer Variablen Deklaration für den Sammlungs Bereich muss als Wert klassifiziert werden, dessen Typ abgefragt werden kann.</span><span class="sxs-lookup"><span data-stu-id="87dba-3305">The collection expression in a collection range variable declaration must be classified as a value whose type is queryable.</span></span> <span data-ttu-id="87dba-3306">Wenn der Typ einer Variablen für den Sammlungs Bereich weggelassen wird, wird er als Elementtyp des Auflistungs Ausdrucks abgeleitet, oder `Object`, wenn der Auflistungs Ausdruck keinen Elementtyp hat (d. h. nur eine `Cast`-Methode definiert).</span><span class="sxs-lookup"><span data-stu-id="87dba-3306">If the type of a collection range variable is omitted, it is inferred to be the element type of the collection expression, or `Object` if the collection expression does not have an element type (i.e. only defines a `Cast` method).</span></span> <span data-ttu-id="87dba-3307">Wenn der Auflistungs Ausdruck nicht abgefragt werden kann (d. h. der Elementtyp der Auflistung kann nicht abgeleitet werden), wird ein Kompilierzeitfehler ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="87dba-3307">If the collection expression is not queryable (i.e. the element type of the collection cannot be inferred), a compile-time error results.</span></span>

<span data-ttu-id="87dba-3308">Eine Ausdrucks Bereichs Variable ist eine Bereichs Variable, deren Wert durch einen Ausdruck und nicht durch eine Auflistung berechnet wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-3308">An expression range variable is a range variable whose value is calculated by an expression rather than a collection.</span></span> <span data-ttu-id="87dba-3309">Im folgenden Beispiel führt der `Select`-Abfrage Operator eine Ausdrucks Bereichs Variable mit dem Namen `cityState` ein, die aus zwei Feldern berechnet wurde:</span><span class="sxs-lookup"><span data-stu-id="87dba-3309">In the following example, the `Select` query operator introduces an expression range variable named `cityState` calculated from two fields:</span></span>

```vb
Dim cityStates = _
    From cust As Customer In Customers _
    Select cityState = cust.City & "," & cust.State _
    Where cityState.Length() < 10
```

<span data-ttu-id="87dba-3310">Eine Ausdrucks Bereichs Variable ist nicht erforderlich, um auf eine andere Bereichs Variable zu verweisen, obwohl eine solche Variable einen zweifelhaften Wert aufweisen kann.</span><span class="sxs-lookup"><span data-stu-id="87dba-3310">An expression range variable is not required to reference another range variable, although such a variable may be of dubious value.</span></span> <span data-ttu-id="87dba-3311">Der Ausdruck, der einer Ausdrucks Bereichs Variablen zugewiesen ist, muss als Wert klassifiziert werden und muss implizit in den Typ der Bereichs Variablen konvertiert werden können, falls angegeben.</span><span class="sxs-lookup"><span data-stu-id="87dba-3311">The expression assigned to an expression range variable must be classified as a value and must be implicitly convertible to the type of the range variable, if given.</span></span>

<span data-ttu-id="87dba-3312">Nur in einem Let-Operator darf der Typ einer Ausdrucks Bereichs Variablen angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3312">Only in a Let operator may an expression range variable have its type specified.</span></span> <span data-ttu-id="87dba-3313">In anderen Operatoren oder, wenn der Typ nicht angegeben ist, wird der Typ der Bereichs Variablen mithilfe des lokalen Variablen Typs abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="87dba-3313">In other operators, or if its type is not specified, then local variable type inference is used to determine the type of the range variable.</span></span>

<span data-ttu-id="87dba-3314">Eine Bereichs Variable muss den Regeln zum Deklarieren von lokalen Variablen in Bezug auf shadowingfolgen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3314">A range variable must follow the rules for declaring local variables in respect to shadowing.</span></span> <span data-ttu-id="87dba-3315">Daher kann eine Bereichs Variable den Namen einer lokalen Variablen oder eines Parameters in der einschließenden Methode oder einer anderen Bereichs Variablen nicht ausblenden (es sei denn, der Abfrage Operator blendet alle aktuellen Bereichs Variablen im Gültigkeitsbereich explizit aus).</span><span class="sxs-lookup"><span data-stu-id="87dba-3315">Thus, a range variable cannot hide the name of a local variable or parameter in the enclosing method or another range variable (unless the query operator specifically hides all current range variables in scope).</span></span>


### <a name="queryable-types"></a><span data-ttu-id="87dba-3316">Abfragbare Typen</span><span class="sxs-lookup"><span data-stu-id="87dba-3316">Queryable Types</span></span>

<span data-ttu-id="87dba-3317">Abfrage Ausdrücke werden implementiert, indem der Ausdruck in Aufrufe bekannter Methoden für einen Auflistungstyp übersetzt wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-3317">Query expressions are implemented by translating the expression into calls to well-known methods on a collection type.</span></span> <span data-ttu-id="87dba-3318">Diese klar definierten Methoden definieren den Elementtyp der abfragbaren Auflistung sowie die Ergebnistypen von Abfrage Operatoren, die für die Auflistung ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3318">These well-defined methods define the element type of the queryable collection as well as the result types of query operators executed on the collection.</span></span> <span data-ttu-id="87dba-3319">Jeder Abfrage Operator gibt die Methode oder Methoden an, in die der Abfrage Operator allgemein übersetzt wird, obwohl die jeweilige Übersetzung von der Implementierung abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3319">Each query operator specifies the method or methods that the query operator is generally translated into, although the specific translation is implementation dependent.</span></span> <span data-ttu-id="87dba-3320">Die-Methoden werden in der Spezifikation mit einem allgemeinen Format angegeben, das wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="87dba-3320">The methods are given in the specification using a general format that looks like:</span></span>

```vb
Function Select(selector As Func(Of T, R)) As CR
```

<span data-ttu-id="87dba-3321">Folgendes gilt für die-Methoden:</span><span class="sxs-lookup"><span data-stu-id="87dba-3321">The following applies to the methods:</span></span>

* <span data-ttu-id="87dba-3322">Die Methode muss eine Instanz oder ein Erweiterungs Mitglied des Auflistungs Typs sein, und es muss darauf zugegriffen werden können.</span><span class="sxs-lookup"><span data-stu-id="87dba-3322">The method must be an instance or extension member of the collection type and must be accessible.</span></span>

* <span data-ttu-id="87dba-3323">Die Methode kann generisch sein, vorausgesetzt, es ist möglich, alle Typargumente abzuleiten.</span><span class="sxs-lookup"><span data-stu-id="87dba-3323">The method may be generic, provided that is possible to infer all type arguments.</span></span>

* <span data-ttu-id="87dba-3324">Die Methode kann überladen werden. in diesem Fall wird die Überladungs Auflösung verwendet, um die exakt zu verwendende Methode zu bestimmen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3324">The method may be overloaded, in which case overload resolution is used to determine the exactly method to use.</span></span>

* <span data-ttu-id="87dba-3325">Anstelle des Delegaten `Func`-Typs kann ein anderer Delegattyp verwendet werden, vorausgesetzt, dass Sie die gleiche Signatur, einschließlich Rückgabetyp, als übereinstimmenden `Func`-Typ aufweist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3325">Another delegate type may be used in place of the delegate `Func` type, provided that it has the same signature, including return type, as the matching `Func` type.</span></span>

* <span data-ttu-id="87dba-3326">Der Typ `System.Linq.Expressions.Expression(Of D)` kann anstelle des Delegaten `Func`-Typs verwendet werden, vorausgesetzt, dass es sich bei `D` um einen Delegattyp mit derselben Signatur, einschließlich Rückgabetyp, als übereinstimmenden `Func`-Typ handelt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3326">The type `System.Linq.Expressions.Expression(Of D)` may be used in place of the delegate `Func` type, provided that `D` is a delegate type that has the same signature, including return type, as the matching `Func` type.</span></span>

* <span data-ttu-id="87dba-3327">Der Typ `T` stellt den Elementtyp der Eingabe Auflistung dar.</span><span class="sxs-lookup"><span data-stu-id="87dba-3327">The type `T` represents the element type of the input collection.</span></span> <span data-ttu-id="87dba-3328">Alle von einem Auflistungstyp definierten Methoden müssen den gleichen Eingabe Elementtyp aufweisen, damit der Auflistungstyp abgefragt werden können.</span><span class="sxs-lookup"><span data-stu-id="87dba-3328">All of the methods defined by a collection type must have the same input element type for the collection type to be queryable.</span></span>

* <span data-ttu-id="87dba-3329">Der Typ `S` stellt den Elementtyp der zweiten Eingabe Auflistung im Fall von Abfrage Operatoren dar, die Joins ausführen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3329">The type `S` represents the element type of the second input collection in the case of query operators that perform joins.</span></span>

* <span data-ttu-id="87dba-3330">Der Typ `K` stellt einen Schlüsseltyp im Fall von Abfrage Operatoren dar, die über einen Satz von Bereichs Variablen verfügen, die als Schlüssel fungieren.</span><span class="sxs-lookup"><span data-stu-id="87dba-3330">The type `K` represents a key type in the case of query operators that have a set of range variables that act as keys.</span></span>

* <span data-ttu-id="87dba-3331">Der Typ `N` stellt einen Typ dar, der als numerischer Typ verwendet wird (es kann sich jedoch immer noch um einen benutzerdefinierten Typ und nicht um einen systeminternen numerischen Typ handeln).</span><span class="sxs-lookup"><span data-stu-id="87dba-3331">The type `N` represents a type that is used as a numeric type (although it could still be a user-defined type and not an intrinsic numeric type).</span></span>

* <span data-ttu-id="87dba-3332">Der Typ `B` stellt einen Typ dar, der in einem booleschen Ausdruck verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="87dba-3332">The type `B` represents a type that can be used in a Boolean expression.</span></span>

* <span data-ttu-id="87dba-3333">Der Typ `R` stellt den Elementtyp der Ergebnis Auflistung dar, wenn der Abfrage Operator eine Ergebnis Auflistung erzeugt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3333">The type `R` represents the element type of the result collection, if the query operator produces a result collection.</span></span> <span data-ttu-id="87dba-3334">`R` hängt von der Anzahl der Bereichs Variablen im Gültigkeitsbereich am Ende des Abfrage Operators ab.</span><span class="sxs-lookup"><span data-stu-id="87dba-3334">`R` depends on the number of range variables in scope at the conclusion of the query operator.</span></span> <span data-ttu-id="87dba-3335">Wenn sich eine einzelne Bereichs Variable im Gültigkeitsbereich befindet, ist `R` der Typ der Bereichs Variablen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3335">If a single range variable is in scope, then `R` is the type of that range variable.</span></span> <span data-ttu-id="87dba-3336">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="87dba-3336">In the example</span></span>

  ```vb
  Dim custNames = From c In Customers
                  Select c.Name
  ```

  <span data-ttu-id="87dba-3337">Das Ergebnis der Abfrage ist ein Sammlungstyp mit dem Elementtyp `String`.</span><span class="sxs-lookup"><span data-stu-id="87dba-3337">the result of the query will be a collection type with an element type of `String`.</span></span> <span data-ttu-id="87dba-3338">Wenn sich mehrere Bereichs Variablen im Gültigkeitsbereich befinden, ist `R` ein anonymer Typ, der alle Bereichs Variablen im Gültigkeitsbereich als `Key`-Felder enthält.</span><span class="sxs-lookup"><span data-stu-id="87dba-3338">If multiple range variables are in scope, then `R` is an anonymous type that contains all of the range variables in scope as `Key` fields.</span></span> <span data-ttu-id="87dba-3339">Im folgenden Beispiel</span><span class="sxs-lookup"><span data-stu-id="87dba-3339">In the example:</span></span>

  ```vb
  Dim custNames = From c In Customers, o In c.Orders 
                  Select Name = c.Name, ProductName = o.ProductName
  ```

  <span data-ttu-id="87dba-3340">Das Ergebnis der Abfrage ist ein Sammlungstyp mit einem Elementtyp eines anonymen Typs mit einer schreibgeschützten Eigenschaft mit dem Namen "`Name`" vom Typ "`String`" und einer schreibgeschützten Eigenschaft mit dem Namen "`ProductName`" vom Typ "`String`".</span><span class="sxs-lookup"><span data-stu-id="87dba-3340">the result of the query will be a collection type with an element type of an anonymous type with a read-only property named `Name` of type `String` and a read-only property named `ProductName` of type `String`.</span></span>

  <span data-ttu-id="87dba-3341">In einem Abfrage Ausdruck sind anonyme Typen, die generiert werden, um Bereichs Variablen zu enthalten, *transparent*, was bedeutet, dass Bereichs Variablen immer ohne Qualifizierung verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="87dba-3341">Within a query expression, anonymous types generated to contain range variables are *transparent*, which means that range variables are always available without qualification.</span></span> <span data-ttu-id="87dba-3342">Im vorherigen Beispiel konnte beispielsweise auf die Bereichs Variablen `c` und `o` ohne Qualifizierung im `Select`-Abfrage Operator zugegriffen werden, obwohl der Elementtyp der Eingabe Auflistung ein anonymer Typ war.</span><span class="sxs-lookup"><span data-stu-id="87dba-3342">For example, in the previous example the range variables `c` and `o` could be accessed without qualification in the `Select` query operator, even though the input collection's element type was an anonymous type.</span></span>

* <span data-ttu-id="87dba-3343">Der Typ `CX` stellt einen Sammlungstyp dar, nicht notwendigerweise den Eingabe Sammlungstyp, dessen Elementtyp ein Typ ist `X`.</span><span class="sxs-lookup"><span data-stu-id="87dba-3343">The type `CX` represents a collection type, not necessarily the input collection type, whose element type is some type `X`.</span></span>

<span data-ttu-id="87dba-3344">Ein abfragbarer Auflistungstyp muss eine der folgenden Bedingungen erfüllen:</span><span class="sxs-lookup"><span data-stu-id="87dba-3344">A queryable collection type must satisfy one of the following conditions, in order of preference:</span></span>

* <span data-ttu-id="87dba-3345">Es muss eine konforme `Select`-Methode definieren.</span><span class="sxs-lookup"><span data-stu-id="87dba-3345">It must define a conforming `Select` method.</span></span>

* <span data-ttu-id="87dba-3346">Es muss eine der folgenden Methoden aufweisen:</span><span class="sxs-lookup"><span data-stu-id="87dba-3346">It must have have one of the following methods</span></span>

  ```vb
  Function AsEnumerable() As CT
  Function AsQueryable() As CT
  ```

  <span data-ttu-id="87dba-3347">, die aufgerufen werden kann, um eine abfragbare Auflistung zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="87dba-3347">which can be called to obtain a queryable collection.</span></span> <span data-ttu-id="87dba-3348">Wenn beide Methoden bereitgestellt werden, wird `AsQueryable` als `AsEnumerable` bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3348">If both methods are provided, `AsQueryable` is preferred over `AsEnumerable`.</span></span>

* <span data-ttu-id="87dba-3349">Er muss über eine-Methode verfügen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3349">It must have a method</span></span>

  ```vb
  Function Cast(Of T)() As CT
  ```

  <span data-ttu-id="87dba-3350">, der mit dem Typ der Bereichs Variablen aufgerufen werden kann, um eine abfragbare Auflistung zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="87dba-3350">which can be called with the type of the range variable to produce a queryable collection.</span></span>

<span data-ttu-id="87dba-3351">Da die Bestimmung des Elementtyps einer Auflistung unabhängig von einem tatsächlichen Methodenaufruf auftritt, kann die Anwendbarkeit spezifischer Methoden nicht bestimmt werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3351">Because determining the element type of a collection occurs independently of an actual method invocation, the applicability of specific methods cannot be determined.</span></span> <span data-ttu-id="87dba-3352">Wenn Sie den Elementtyp einer Auflistung ermitteln, wenn Instanzmethoden vorhanden sind, die mit bekannten Methoden identisch sind, werden daher alle Erweiterungs Methoden ignoriert, die bekannten Methoden entsprechen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3352">Thus, when determining the element type of a collection if there are instance methods that match well-known methods, then any extension methods that match well-known methods are ignored.</span></span>

<span data-ttu-id="87dba-3353">Die Abfrage Operator Übersetzung erfolgt in der Reihenfolge, in der die Abfrage Operatoren im Ausdruck auftreten.</span><span class="sxs-lookup"><span data-stu-id="87dba-3353">Query operator translation occurs in the order in which the query operators occur in the expression.</span></span> <span data-ttu-id="87dba-3354">Es ist nicht erforderlich, dass ein Auflistungs Objekt alle Methoden implementiert, die von allen Abfrage Operatoren benötigt werden, obwohl jedes Auflistungs Objekt zumindest den `Select`-Abfrage Operator unterstützen muss.</span><span class="sxs-lookup"><span data-stu-id="87dba-3354">It is not necessary for a collection object to implement all of the methods needed by all the query operators, although every collection object must at least support the `Select` query operator.</span></span> <span data-ttu-id="87dba-3355">Wenn eine erforderliche Methode nicht vorhanden ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-3355">If a needed method is not present, a compile-time error occurs.</span></span> <span data-ttu-id="87dba-3356">Wenn bekannte Methodennamen gebunden werden, werden nicht--Methoden für den Zweck der Mehrfachvererbung in Schnittstellen und der Bindungsmethoden Bindung ignoriert, obwohl die Semantik für die Schatten-Semantik weiterhin gilt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3356">When binding well-known method names, non-methods are ignored for the purpose of multiple inheritance in interfaces and extension method binding, although shadowing semantics still apply.</span></span> <span data-ttu-id="87dba-3357">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3357">For example:</span></span>

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

### <a name="default-query-indexer"></a><span data-ttu-id="87dba-3358">Standardinfrageindexer</span><span class="sxs-lookup"><span data-stu-id="87dba-3358">Default Query Indexer</span></span>

<span data-ttu-id="87dba-3359">Jeder abfragbare Auflistungstyp, dessen Elementtyp `T` ist und der nicht bereits über eine Default-Eigenschaft verfügt, wird als Standard Eigenschaft der folgenden allgemeinen Form angesehen:</span><span class="sxs-lookup"><span data-stu-id="87dba-3359">Every queryable collection type whose element type is `T` and does not already have a default property is considered to have a default property of the following general form:</span></span>

```vb
Public ReadOnly Default Property Item(index As Integer) As T
    Get
        Return Me.ElementAtOrDefault(index)
    End Get
End Property
```

<span data-ttu-id="87dba-3360">Auf die Default-Eigenschaft kann nur mit der standardmäßigen Eigenschaften Zugriffs Syntax verwiesen werden. auf die Default-Eigenschaft kann nicht anhand des Namens verwiesen werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3360">The default property can only be referred to using the default property access syntax; the default property cannot be referred to by name.</span></span> <span data-ttu-id="87dba-3361">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3361">For example:</span></span>

```vb
Dim customers As IEnumerable(Of Customer) = ...
Dim customerThree = customers(2)

' Error, no such property
Dim customerFour = customers.Item(4)
```

<span data-ttu-id="87dba-3362">Wenn der Auflistungstyp nicht über einen `ElementAtOrDefault`-Member verfügt, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-3362">If the collection type does not have an `ElementAtOrDefault` member, a compile-time error will occur.</span></span>

### <a name="from-query-operator"></a><span data-ttu-id="87dba-3363">From Query-Operator</span><span class="sxs-lookup"><span data-stu-id="87dba-3363">From Query Operator</span></span>

<span data-ttu-id="87dba-3364">Der `From`-Abfrage Operator führt eine Sammlungs Bereichs Variable ein, die die einzelnen Elemente einer Auflistung darstellt, die abgefragt werden soll.</span><span class="sxs-lookup"><span data-stu-id="87dba-3364">The `From` query operator introduces a collection range variable that represents the individual members of a collection to be queried.</span></span>

```antlr
FromQueryOperator
    : LineTerminator? 'From' LineTerminator? CollectionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="87dba-3365">Der Abfrage Ausdruck lautet z. b.:</span><span class="sxs-lookup"><span data-stu-id="87dba-3365">For example, the query expression:</span></span>

```vb
From c As Customer In Customers ...
```

<span data-ttu-id="87dba-3366">kann als äquivalent zu betrachtet werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3366">can be thought of as equivalent to</span></span>

```vb
For Each c As Customer In Customers
        ...
Next c
```

<span data-ttu-id="87dba-3367">Wenn ein `From`-Abfrage Operator mehrere Sammlungs Bereichs Variablen deklariert oder nicht der erste `From`-Abfrage Operator im Abfrage Ausdruck ist, wird jede neue Sammlungs Bereichs *Variable mit dem* vorhandenen Satz von Bereichs Variablen miteinander verknüpft.</span><span class="sxs-lookup"><span data-stu-id="87dba-3367">When a `From` query operator declares multiple collection range variables or is not the first `From` query operator in the query expression, each new collection range variable is *cross joined* to the existing set of range variables.</span></span> <span data-ttu-id="87dba-3368">Das Ergebnis ist, dass die Abfrage über das Kreuz Produkt aller Elemente in den verbundenen Auflistungen ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-3368">The result is that the query is evaluated over the cross-product of all the elements in the joined collections.</span></span> <span data-ttu-id="87dba-3369">Der Ausdruck lautet z. b.:</span><span class="sxs-lookup"><span data-stu-id="87dba-3369">For example, the expression:</span></span>

```vb
From c In Customers _
From e In Employees _
...
```

<span data-ttu-id="87dba-3370">kann als äquivalent zu betrachtet werden:</span><span class="sxs-lookup"><span data-stu-id="87dba-3370">can be thought of as equivalent to:</span></span>

```vb
For Each c In Customers
    For Each e In Employees
            ...
    Next e
Next c
```

<span data-ttu-id="87dba-3371">und ist genau Äquivalent zu:</span><span class="sxs-lookup"><span data-stu-id="87dba-3371">and is exactly equivalent to:</span></span>

```vb
From c In Customers, e In Employees ...
```

<span data-ttu-id="87dba-3372">Die in vorherigen Abfrage Operatoren eingeführten Bereichs Variablen können in einem späteren `From`-Abfrage Operator verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3372">The range variables introduced in previous query operators can be used in a later `From` query operator.</span></span> <span data-ttu-id="87dba-3373">Im folgenden Abfrage Ausdruck bezieht sich z. b. der zweite `From`-Abfrage Operator auf den Wert der ersten Bereichs Variablen:</span><span class="sxs-lookup"><span data-stu-id="87dba-3373">For example, in the following query expression the second `From` query operator refers to the value of the first range variable:</span></span>

```vb
From c As Customer In Customers _
From o As Order In c.Orders _
Select c.Name, o
```

<span data-ttu-id="87dba-3374">Mehrere Bereichs Variablen in einem `From`-Abfrage Operator oder mehrere `From`-Abfrage Operatoren werden nur unterstützt, wenn der Auflistungstyp eine oder beide der folgenden Methoden enthält:</span><span class="sxs-lookup"><span data-stu-id="87dba-3374">Multiple range variables in a `From` query operator or multiple `From` query operators are only supported if the collection type contains one or both of the following methods:</span></span>

```vb
Function SelectMany(selector As Func(Of T, CR)) As CR
Function SelectMany(selector As Func(Of T, CS), _
                          resultsSelector As Func(Of T, S, R)) As CR
```

<span data-ttu-id="87dba-3375">Der Code</span><span class="sxs-lookup"><span data-stu-id="87dba-3375">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs, y In ys ...
```

<span data-ttu-id="87dba-3376">wird im Allgemeinen in übersetzt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3376">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.SelectMany( _
        Function(x As Integer) ys, _
        Function(x As Integer, y As Integer) New With {x, y})...
```

<span data-ttu-id="87dba-3377">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-3377">__Note.__</span></span> <span data-ttu-id="87dba-3378">`From` ist kein reserviertes Wort.</span><span class="sxs-lookup"><span data-stu-id="87dba-3378">`From` is not a reserved word.</span></span>


### <a name="join-query-operator"></a><span data-ttu-id="87dba-3379">Join-Abfrage Operator</span><span class="sxs-lookup"><span data-stu-id="87dba-3379">Join Query Operator</span></span>

<span data-ttu-id="87dba-3380">Der `Join`-Abfrage Operator verknüpft vorhandene Bereichs Variablen mit einer neuen Sammlungs Bereichs Variablen und erzeugt eine einzelne Auflistung, deren Elemente auf Grundlage eines Gleichheits Ausdrucks verknüpft wurden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3380">The `Join` query operator joins existing range variables with a new collection range variable, producing a single collection whose elements have been joined together based on an equality expression.</span></span>

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

<span data-ttu-id="87dba-3381">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3381">For example:</span></span>

```vb
Dim customersAndOrders = _
    From cust In Customers _
    Join ord In Orders On cust.ID Equals ord.CustomerID
```

<span data-ttu-id="87dba-3382">Der Gleichheits Ausdruck ist stärker eingeschränkt als ein regulärer Gleichheits Ausdruck:</span><span class="sxs-lookup"><span data-stu-id="87dba-3382">The equality expression is more restricted than a regular equality expression:</span></span>

* <span data-ttu-id="87dba-3383">Beide Ausdrücke müssen als Wert klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3383">Both expressions must be classified as a value.</span></span>

* <span data-ttu-id="87dba-3384">Beide Ausdrücke müssen auf mindestens eine Bereichs Variable verweisen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3384">Both expressions must reference at least one range variable.</span></span>

* <span data-ttu-id="87dba-3385">Auf die im Join-Abfrage Operator deklarierte Bereichs Variable muss von einem der Ausdrücke verwiesen werden, und dieser Ausdruck darf nicht auf andere Bereichs Variablen verweisen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3385">The range variable declared in the join query operator must be referenced by one of the expressions, and that expression must not reference any other range variables.</span></span>

<span data-ttu-id="87dba-3386">Wenn die Typen der beiden Ausdrücke nicht exakt denselben Typ haben,</span><span class="sxs-lookup"><span data-stu-id="87dba-3386">If the types of the two expressions are not the exact same type, then</span></span>

* <span data-ttu-id="87dba-3387">Wenn der Gleichheits Operator für die beiden Typen definiert ist, können beide Ausdrücke implizit in diesen konvertiert werden. er ist nicht `Object` und konvertiert dann beide Ausdrücke in diesen Typ.</span><span class="sxs-lookup"><span data-stu-id="87dba-3387">If the equality operator is defined for the two types, both expressions are implicitly convertible to it, and it is not `Object`, then convert both expressions to that type.</span></span>

* <span data-ttu-id="87dba-3388">Andernfalls konvertieren Sie beide Ausdrücke in diesen Typ, wenn ein dominanter Typ vorhanden ist, in den beide Ausdrücke implizit konvertiert werden können.</span><span class="sxs-lookup"><span data-stu-id="87dba-3388">Otherwise, if there is a dominant type that both expressions can be implicitly converted to, then convert both expressions to that type.</span></span>

* <span data-ttu-id="87dba-3389">Andernfalls tritt ein Kompilierungsfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-3389">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="87dba-3390">Die Ausdrücke werden mithilfe von Hash Werten verglichen (d. h. durch Aufrufen von `GetHashCode()`) und nicht durch die Verwendung von Gleichheits Operatoren.</span><span class="sxs-lookup"><span data-stu-id="87dba-3390">The expressions are compared using hash values (i.e. by calling `GetHashCode()`) rather than by using equality operators for efficiency.</span></span> <span data-ttu-id="87dba-3391">Ein `Join`-Abfrage Operator kann im selben Operator mehrere Joins oder Gleichheits Bedingungen ausführen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3391">A `Join` query operator may do multiple joins or equality conditions in the same operator.</span></span> <span data-ttu-id="87dba-3392">Ein `Join`-Abfrage Operator wird nur unterstützt, wenn der Auflistungstyp eine-Methode enthält:</span><span class="sxs-lookup"><span data-stu-id="87dba-3392">A `Join` query operator is only supported if the collection type contains a method:</span></span>

```vb
Function Join(inner As CS, _
                  outerSelector As Func(Of T, K), _
                  innerSelector As Func(Of S, K), _
                  resultSelector As Func(Of T, S, R)) As CR
```

<span data-ttu-id="87dba-3393">Der Code</span><span class="sxs-lookup"><span data-stu-id="87dba-3393">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs _
            Join y In ys On x Equals y _
            ...
```

<span data-ttu-id="87dba-3394">wird im Allgemeinen in übersetzt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3394">is generally translated to</span></span>

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

<span data-ttu-id="87dba-3395">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-3395">__Note.__</span></span> <span data-ttu-id="87dba-3396">`Join`, `On` und `Equals` sind keine reservierten Wörter.</span><span class="sxs-lookup"><span data-stu-id="87dba-3396">`Join`, `On` and `Equals` are not reserved words.</span></span>


### <a name="let-query-operator"></a><span data-ttu-id="87dba-3397">Let Query-Operator</span><span class="sxs-lookup"><span data-stu-id="87dba-3397">Let Query Operator</span></span>

<span data-ttu-id="87dba-3398">Der `Let`-Abfrage Operator führt eine Ausdrucks Bereichs Variable ein.</span><span class="sxs-lookup"><span data-stu-id="87dba-3398">The `Let` query operator introduces an expression range variable.</span></span> <span data-ttu-id="87dba-3399">Dies ermöglicht das Berechnen eines zwischen Werts einmal, der in späteren Abfrage Operatoren mehrmals verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-3399">This allows calculating an intermediate value once that will be used multiple times in later query operators.</span></span>

```antlr
LetQueryOperator
    : LineTerminator? 'Let' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="87dba-3400">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3400">For example:</span></span>

```vb
Dim taxedPrices = _
    From o In Orders _
    Let tax = o.Price * 0.088 _
    Where tax > 3.50 _
    Select o.Price, tax, total = o.Price + tax
```

<span data-ttu-id="87dba-3401">kann als äquivalent zu betrachtet werden:</span><span class="sxs-lookup"><span data-stu-id="87dba-3401">can be thought of as equivalent to:</span></span>

```vb
For Each o In Orders
    Dim tax = o.Price * 0.088
    ...
Next o
```

<span data-ttu-id="87dba-3402">Ein `Let`-Abfrage Operator wird nur unterstützt, wenn der Auflistungstyp eine-Methode enthält:</span><span class="sxs-lookup"><span data-stu-id="87dba-3402">A `Let` query operator is only supported if the collection type contains a method:</span></span>

```vb
Function Select(selector As Func(Of T, R)) As CR
```

<span data-ttu-id="87dba-3403">Der Code</span><span class="sxs-lookup"><span data-stu-id="87dba-3403">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Let y = x * 10 _
            ...
```

<span data-ttu-id="87dba-3404">wird im Allgemeinen in übersetzt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3404">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="select-query-operator"></a><span data-ttu-id="87dba-3405">SELECT Query-Operator</span><span class="sxs-lookup"><span data-stu-id="87dba-3405">Select Query Operator</span></span>

<span data-ttu-id="87dba-3406">Der `Select`-Abfrage Operator ist wie der `Let`-Abfrage Operator, da er Ausdrucks Bereichs Variablen einführt. ein `Select`-Abfrage Operator verbirgt jedoch die aktuell verfügbaren Bereichs Variablen, anstatt Sie zu hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3406">The `Select` query operator is like the `Let` query operator in that it introduces expression range variables; however, a `Select` query operator hides the currently available range variables instead of adding to them.</span></span> <span data-ttu-id="87dba-3407">Außerdem wird der Typ einer Ausdrucks Bereichs Variablen, die mit einem `Select`-Abfrage Operator eingeführt wurde, immer mithilfe von Regeln für den lokalen variablentyprückschluss abgeleitet. ein expliziter Typ kann nicht angegeben werden, und wenn kein Typ abgeleitet werden kann, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-3407">Also, the type of an expression range variable introduced by a `Select` query operator is always inferred using local variable type inference rules; an explicit type cannot be specified, and if no type can be inferred, a compile-time error occurs.</span></span>

```antlr
SelectQueryOperator
    : LineTerminator? 'Select' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="87dba-3408">Beispielsweise ist in der Abfrage:</span><span class="sxs-lookup"><span data-stu-id="87dba-3408">For example, in the query:</span></span>

```vb
Dim smiths = _
    From cust In Customers _
    Select name = cust.name _
    Where name.EndsWith("Smith")
```

<span data-ttu-id="87dba-3409">der `Where`-Abfrage Operator hat nur Zugriff auf die vom `Select`-Operator eingeführte `name`-Bereichs Variable. Wenn der `Where`-Operator versucht hat, auf `cust` zu verweisen, ist ein Kompilierzeitfehler aufgetreten.</span><span class="sxs-lookup"><span data-stu-id="87dba-3409">the `Where` query operator only has access to the `name` range variable introduced by the `Select` operator; if the `Where` operator had tried to reference `cust`, a compile-time error would have occurred.</span></span>

<span data-ttu-id="87dba-3410">Anstatt die Namen der Bereichs Variablen explizit anzugeben, kann ein `Select`-Abfrage Operator die Namen der Bereichs Variablen ableiten, wobei die gleichen Regeln wie für Ausdrücke zum Erstellen von anonymen Typen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3410">Instead of explicitly specifying the names of the range variables, a `Select` query operator can infer the names of the range variables, using the same rules as anonymous type object creation expressions.</span></span> <span data-ttu-id="87dba-3411">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3411">For example:</span></span>

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.name, ord.ProductName _
        Where name.EndsWith("Smith")
```

<span data-ttu-id="87dba-3412">Wenn der Name der Bereichs Variablen nicht angegeben wird und kein Name abgeleitet werden kann, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-3412">If the name of the range variable is not supplied and a name cannot be inferred, a compile-time error occurs.</span></span> <span data-ttu-id="87dba-3413">Wenn der `Select`-Abfrage Operator nur einen einzelnen Ausdruck enthält, tritt kein Fehler auf, wenn ein Name für diese Bereichs Variable nicht abgeleitet werden kann, aber die Bereichs Variable namenlos ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3413">If the `Select` query operator contains only a single expression, no error occurs if a name for that range variable cannot be inferred but the range variable is nameless.</span></span>  <span data-ttu-id="87dba-3414">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3414">For example:</span></span>

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.Name & " bought " & ord.ProductName _
        Take 10
```

<span data-ttu-id="87dba-3415">Wenn eine Mehrdeutigkeit in einem `Select`-Abfrage Operator zwischen dem Zuweisen eines Namens zu einer Bereichs Variablen und einem Gleichheits Ausdruck vorliegt, wird die namens Zuweisung bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3415">If there is an ambiguity in a `Select` query operator between assigning a name to a range variable and an equality expression, the name assignment is preferred.</span></span> <span data-ttu-id="87dba-3416">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3416">For example:</span></span>

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

<span data-ttu-id="87dba-3417">Jeder Ausdruck im `Select`-Abfrage Operator muss als Wert klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3417">Each expression in the `Select` query operator must be classified as a value.</span></span> <span data-ttu-id="87dba-3418">Ein `Select`-Abfrage Operator wird nur unterstützt, wenn der Auflistungstyp eine-Methode enthält:</span><span class="sxs-lookup"><span data-stu-id="87dba-3418">A `Select` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Select(selector As Func(Of T, R)) As CR
```

<span data-ttu-id="87dba-3419">Der Code</span><span class="sxs-lookup"><span data-stu-id="87dba-3419">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Select x, y = x * 10 _
            ...
```

<span data-ttu-id="87dba-3420">wird im Allgemeinen in übersetzt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3420">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="distinct-query-operator"></a><span data-ttu-id="87dba-3421">Unterschiedlicher Abfrage Operator</span><span class="sxs-lookup"><span data-stu-id="87dba-3421">Distinct Query Operator</span></span>

<span data-ttu-id="87dba-3422">Der `Distinct`-Abfrage Operator schränkt die Werte in einer Auflistung nur auf diejenigen mit unterschiedlichen Werten ein, die durch Vergleichen des Elementtyps auf Gleichheit festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3422">The `Distinct` query operator restricts the values in a collection only to those with distinct values, as determined by comparing the element type for equality.</span></span>

```antlr
DistinctQueryOperator
    : LineTerminator? 'Distinct' LineTerminator?
    ;
```

<span data-ttu-id="87dba-3423">Beispielsweise lautet die Abfrage:</span><span class="sxs-lookup"><span data-stu-id="87dba-3423">For example, the query:</span></span>

```vb
Dim distinctCustomerPrice = _
    From cust In Customers, ord In cust.Orders _
    Select cust.Name, ord.Price _
    Distinct
```

<span data-ttu-id="87dba-3424">gibt nur eine Zeile für jede einzelne Kopplung von Kunden Name und Bestellpreis zurück, auch wenn der Kunde über mehrere Bestellungen mit demselben Preis verfügt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3424">will only return one row for each distinct pairing of customer name and order price, even if the customer has multiple orders with the same price.</span></span> <span data-ttu-id="87dba-3425">Ein `Distinct`-Abfrage Operator wird nur unterstützt, wenn der Auflistungstyp eine-Methode enthält:</span><span class="sxs-lookup"><span data-stu-id="87dba-3425">A `Distinct` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Distinct() As CT
```

<span data-ttu-id="87dba-3426">Der Code</span><span class="sxs-lookup"><span data-stu-id="87dba-3426">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Distinct _
            ...
```

<span data-ttu-id="87dba-3427">wird im Allgemeinen in übersetzt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3427">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = xs.Distinct()...
```

<span data-ttu-id="87dba-3428">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-3428">__Note.__</span></span> <span data-ttu-id="87dba-3429">`Distinct` ist kein reserviertes Wort.</span><span class="sxs-lookup"><span data-stu-id="87dba-3429">`Distinct` is not a reserved word.</span></span>


### <a name="where-query-operator"></a><span data-ttu-id="87dba-3430">WHERE-Abfrage Operator</span><span class="sxs-lookup"><span data-stu-id="87dba-3430">Where Query Operator</span></span>

<span data-ttu-id="87dba-3431">Der `Where`-Abfrage Operator schränkt die Werte in einer Auflistung auf die Werte ein, die eine bestimmte Bedingung erfüllen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3431">The `Where` query operator restricts the values in a collection to those that satisfy a given condition.</span></span>

```antlr
WhereQueryOperator
    : LineTerminator? 'Where' LineTerminator? BooleanExpression
    ;
```

<span data-ttu-id="87dba-3432">Ein `Where`-Abfrage Operator nimmt einen booleschen Ausdruck an, der für jeden Satz von Bereichs Variablen Werten ausgewertet wird. Wenn der Wert des Ausdrucks true ist, werden die Werte in der Ausgabe Auflistung angezeigt, andernfalls werden die Werte übersprungen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3432">A `Where` query operator takes a Boolean expression that is evaluated for each set of range variable values; if the value of the expression is true, then the values appear in the output collection, otherwise the values are skipped.</span></span> <span data-ttu-id="87dba-3433">Der Abfrage Ausdruck lautet z. b.:</span><span class="sxs-lookup"><span data-stu-id="87dba-3433">For example, the query expression:</span></span>

```vb
From cust In Customers, ord In Orders _
Where cust.ID = ord.CustomerID _
...
```

<span data-ttu-id="87dba-3434">kann als Äquivalent zur schsted-Schleife angesehen werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3434">can be thought of as equivalent to the nested loop</span></span>

```vb
For Each cust In Customers
    For Each ord In Orders
            If cust.ID = ord.CustomerID Then
                ...
            End If
    Next ord
Next cust
```

<span data-ttu-id="87dba-3435">Ein `Where`-Abfrage Operator wird nur unterstützt, wenn der Auflistungstyp eine-Methode enthält:</span><span class="sxs-lookup"><span data-stu-id="87dba-3435">A `Where` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Where(predicate As Func(Of T, B)) As CT
```

<span data-ttu-id="87dba-3436">Der Code</span><span class="sxs-lookup"><span data-stu-id="87dba-3436">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Where x < 10 _
            ...
```

<span data-ttu-id="87dba-3437">wird im Allgemeinen in übersetzt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3437">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x As Integer) x < 10)...
```

<span data-ttu-id="87dba-3438">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-3438">__Note.__</span></span> <span data-ttu-id="87dba-3439">`Where` ist kein reserviertes Wort.</span><span class="sxs-lookup"><span data-stu-id="87dba-3439">`Where` is not a reserved word.</span></span>


### <a name="partition-query-operators"></a><span data-ttu-id="87dba-3440">Partitions Abfrage Operatoren</span><span class="sxs-lookup"><span data-stu-id="87dba-3440">Partition Query Operators</span></span>

```antlr
PartitionQueryOperator
    : LineTerminator? 'Take' LineTerminator? Expression
    | LineTerminator? 'Take' 'While' LineTerminator? BooleanExpression
    | LineTerminator? 'Skip' LineTerminator? Expression
    | LineTerminator? 'Skip' 'While' LineTerminator? BooleanExpression
    ;
```

<span data-ttu-id="87dba-3441">Der `Take`-Abfrage Operator führt zu den ersten `n` Elementen einer Auflistung.</span><span class="sxs-lookup"><span data-stu-id="87dba-3441">The `Take` query operator results in the first `n` elements of a collection.</span></span> <span data-ttu-id="87dba-3442">Bei Verwendung mit dem Modifizierer "`While`" führt der `Take`-Operator zu den ersten `n` Elementen einer Auflistung, die einen booleschen Ausdruck erfüllen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3442">When used with the `While` modifier, the `Take` operator results in the first `n` elements of a collection that satisfy a Boolean expression.</span></span> <span data-ttu-id="87dba-3443">Der `Skip`-Operator überspringt die ersten `n`-Elemente einer Auflistung und gibt dann den Rest der Auflistung zurück.</span><span class="sxs-lookup"><span data-stu-id="87dba-3443">The `Skip` operator skips the first `n` elements of a collection and then returns the remainder of the collection.</span></span>  <span data-ttu-id="87dba-3444">Bei Verwendung in Verbindung mit dem Modifizierer "`While`" überspringt der `Skip`-Operator die ersten `n` Elemente einer Auflistung, die einen booleschen Ausdruck erfüllen, und gibt dann den Rest der Auflistung zurück.</span><span class="sxs-lookup"><span data-stu-id="87dba-3444">When used in conjunction with the `While` modifier, the `Skip` operator skips the first `n` elements of a collection that satisfy a Boolean expression and then returns the rest of the collection.</span></span> <span data-ttu-id="87dba-3445">Die Ausdrücke in einem `Take`-oder `Skip`-Abfrage Operator müssen als Wert klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3445">The expressions in a `Take` or `Skip` query operator must be classified as a value.</span></span>

<span data-ttu-id="87dba-3446">Ein `Take`-Abfrage Operator wird nur unterstützt, wenn der Auflistungstyp eine-Methode enthält:</span><span class="sxs-lookup"><span data-stu-id="87dba-3446">A `Take` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Take(count As N) As CT
```

<span data-ttu-id="87dba-3447">Ein `Skip`-Abfrage Operator wird nur unterstützt, wenn der Auflistungstyp eine-Methode enthält:</span><span class="sxs-lookup"><span data-stu-id="87dba-3447">A `Skip` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Skip(count As N) As CT
```

<span data-ttu-id="87dba-3448">Ein `Take While`-Abfrage Operator wird nur unterstützt, wenn der Auflistungstyp eine-Methode enthält:</span><span class="sxs-lookup"><span data-stu-id="87dba-3448">A `Take While` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function TakeWhile(predicate As Func(Of T, B)) As CT
```

<span data-ttu-id="87dba-3449">Ein `Skip While`-Abfrage Operator wird nur unterstützt, wenn der Auflistungstyp eine-Methode enthält:</span><span class="sxs-lookup"><span data-stu-id="87dba-3449">A `Skip While` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function SkipWhile(predicate As Func(Of T, B)) As CT
```

<span data-ttu-id="87dba-3450">Der Code</span><span class="sxs-lookup"><span data-stu-id="87dba-3450">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Skip 10 _
            Take 5 _
            Skip While x < 10 _
            Take While x > 5 _
            ...
```

<span data-ttu-id="87dba-3451">wird im Allgemeinen in übersetzt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3451">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Skip(10). _
        Take(5). _
        SkipWhile(Function(x) x < 10). _
        TakeWhile(Function(x) x > 5)...
```

<span data-ttu-id="87dba-3452">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-3452">__Note.__</span></span> <span data-ttu-id="87dba-3453">`Take` und `Skip` sind keine reservierten Wörter.</span><span class="sxs-lookup"><span data-stu-id="87dba-3453">`Take` and `Skip` are not reserved words.</span></span>


### <a name="order-by-query-operator"></a><span data-ttu-id="87dba-3454">Order by-Abfrage Operator</span><span class="sxs-lookup"><span data-stu-id="87dba-3454">Order By Query Operator</span></span>

<span data-ttu-id="87dba-3455">Der `Order By`-Abfrage Operator sortiert die Werte, die in den Bereichs Variablen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3455">The `Order By` query operator orders the values that appear in the range variables.</span></span> 

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

<span data-ttu-id="87dba-3456">Ein `Order By`-Abfrage Operator übernimmt Ausdrücke, die die Schlüsselwerte angeben, die zum Sortieren der Iterations Variablen verwendet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3456">An `Order By` query operator takes expressions that specify the key values that should be used to order the iteration variables.</span></span> <span data-ttu-id="87dba-3457">Die folgende Abfrage gibt z. b. die nach Preis sortierten Produkte zurück:</span><span class="sxs-lookup"><span data-stu-id="87dba-3457">For example, the following query returns products sorted by price:</span></span>

```vb
Dim productsByPrice = _
    From p In Products _
    Order By p.Price _
    Select p.Name
```

<span data-ttu-id="87dba-3458">Eine Reihenfolge kann als `Ascending` gekennzeichnet werden. in diesem Fall liegen kleinere Werte vor größeren Werten, oder `Descending`. in diesem Fall kommen größere Werte vor kleineren Werten vor.</span><span class="sxs-lookup"><span data-stu-id="87dba-3458">An ordering can be marked as `Ascending`, in which case smaller values come before larger values, or `Descending`, in which case larger values come before smaller values.</span></span> <span data-ttu-id="87dba-3459">Der Standardwert für eine Reihenfolge, wenn kein Wert angegeben ist `Ascending`.</span><span class="sxs-lookup"><span data-stu-id="87dba-3459">The default for an ordering if none is specified is `Ascending`.</span></span> <span data-ttu-id="87dba-3460">Beispielsweise gibt die folgende Abfrage Produkte nach Preis sortiert nach Preis mit dem teuersten Produkt zuerst zurück:</span><span class="sxs-lookup"><span data-stu-id="87dba-3460">For example, the following query returns products sorted by price with the most expensive product first:</span></span>

```vb
Dim productsByPriceDesc = _
    From p In Products _
    Order By p.Price Descending _
    Select p.Name
```

<span data-ttu-id="87dba-3461">Der `Order By`-Abfrage Operator kann mehrere Ausdrücke für die Reihenfolge angeben. in diesem Fall wird die Auflistung in einer geordneten Weise angeordnet.</span><span class="sxs-lookup"><span data-stu-id="87dba-3461">The `Order By` query operator may specify multiple expressions for ordering, in which case the collection is ordered in a nested manner.</span></span> <span data-ttu-id="87dba-3462">Mit der folgenden Abfrage werden z. b. Kunden nach Bundesstaat und dann nach Ort innerhalb jedes Bundesstaats und dann nach Postleitzahl innerhalb der einzelnen Städte sortiert:</span><span class="sxs-lookup"><span data-stu-id="87dba-3462">For example, the following query orders customers by state, then by city within each state and then by ZIP code within each city:</span></span>

```vb
Dim customersByLocation = _
    From c In Customers _
    Order By c.State, c.City, c.ZIP _
    Select c.Name, c.State, c.City, c.ZIP
```

<span data-ttu-id="87dba-3463">Die Ausdrücke in einem `Order By`-Abfrage Operator müssen als Wert klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3463">The expressions in an `Order By` query operator must be classified as a value.</span></span> <span data-ttu-id="87dba-3464">Ein `Order By`-Abfrage Operator wird nur unterstützt, wenn der Auflistungstyp mindestens eine der folgenden Methoden enthält:</span><span class="sxs-lookup"><span data-stu-id="87dba-3464">An `Order By` query operator is supported only if the collection type contains one or both of the following methods:</span></span>

```vb
Function OrderBy(keySelector As Func(Of T, K)) As CT
Function OrderByDescending(keySelector As Func(Of T, K)) As CT
```

<span data-ttu-id="87dba-3465">Der Rückgabetyp `CT` muss eine *geordnete*Auflistung sein.</span><span class="sxs-lookup"><span data-stu-id="87dba-3465">The return type `CT` must be an *ordered collection*.</span></span> <span data-ttu-id="87dba-3466">Eine geordnete Auflistung ist ein Sammlungstyp, der eine oder beide Methoden enthält:</span><span class="sxs-lookup"><span data-stu-id="87dba-3466">An ordered collection is a collection type that contains one or both of the methods:</span></span>

```vb
Function ThenBy(keySelector As Func(Of T, K)) As CT
Function ThenByDescending(keySelector As Func(Of T, K)) As CT
```

<span data-ttu-id="87dba-3467">Der Code</span><span class="sxs-lookup"><span data-stu-id="87dba-3467">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Order By x Ascending, x Mod 2 Descending _
            ...
```

<span data-ttu-id="87dba-3468">wird im Allgemeinen in übersetzt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3468">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.OrderBy(Function(x) x).ThenByDescending(Function(x) x Mod 2)...
```

<span data-ttu-id="87dba-3469">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-3469">__Note.__</span></span> <span data-ttu-id="87dba-3470">Da Abfrage Operatoren einfache Syntax Methoden zuordnen, die einen bestimmten Abfrage Vorgang implementieren, wird die Beibehaltung der Reihenfolge nicht von der Sprache vorgegeben und durch die Implementierung des Operators selbst bestimmt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3470">Because query operators simply map syntax to methods that implement a particular query operation, order preservation is not dictated by the language and is determined by the implementation of the operator itself.</span></span> <span data-ttu-id="87dba-3471">Dies ist sehr ähnlich wie bei benutzerdefinierten Operatoren darin, dass die Implementierung, die den Additions Operator für einen benutzerdefinierten numerischen Typ überlädt, möglicherweise keine Aktion ausführt, die einer Addition ähnelt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3471">This is very similar to user-defined operators in that the implementation to overload the addition operator for a user-defined numeric type may not perform anything resembling an addition.</span></span> <span data-ttu-id="87dba-3472">Um die Vorhersagbarkeit beizubehalten, wird natürlich nicht empfohlen, etwas zu implementieren, das nicht den Erwartungen der Benutzer entspricht.</span><span class="sxs-lookup"><span data-stu-id="87dba-3472">Of course, to preserve predictability, implementing something that does not match user expectations is not recommended.</span></span>

<span data-ttu-id="87dba-3473">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-3473">__Note.__</span></span> <span data-ttu-id="87dba-3474">`Order` und `By` sind keine reservierten Wörter.</span><span class="sxs-lookup"><span data-stu-id="87dba-3474">`Order` and `By` are not reserved words.</span></span>


### <a name="group-by-query-operator"></a><span data-ttu-id="87dba-3475">Group by-Abfrage Operator</span><span class="sxs-lookup"><span data-stu-id="87dba-3475">Group By Query Operator</span></span>

<span data-ttu-id="87dba-3476">Der `Group By`-Abfrage Operator gruppiert die Bereichs Variablen im Gültigkeitsbereich basierend auf einem oder mehreren Ausdrücken und erzeugt dann basierend auf diesen Gruppierungen neue Bereichs Variablen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3476">The `Group By` query operator groups the range variables in scope based on one or more expressions, and then produces new range variables based on those groupings.</span></span>

```antlr
GroupByQueryOperator
    : LineTerminator? 'Group' ( LineTerminator? ExpressionRangeVariableDeclarationList )?
      LineTerminator? 'By' LineTerminator? ExpressionRangeVariableDeclarationList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="87dba-3477">Beispielsweise werden mit der folgenden Abfrage alle Kunden nach `State` gruppiert, und anschließend werden die Anzahl und das durchschnittliche Alter der einzelnen Gruppen berechnet:</span><span class="sxs-lookup"><span data-stu-id="87dba-3477">For example, the following query groups all customers by `State`, and then computes the count and average age of each group:</span></span>

```vb
Dim averageAges = _
    From cust In Customers _
    Group By cust.State _
    Into Count(), Average(cust.Age)
```

<span data-ttu-id="87dba-3478">Der `Group By`-Abfrage Operator hat drei Klauseln: die optionale `Group`-Klausel, die `By`-Klausel und die `Into`-Klausel.</span><span class="sxs-lookup"><span data-stu-id="87dba-3478">The `Group By` query operator has three clauses: the optional `Group` clause, the `By` clause, and the `Into` clause.</span></span> <span data-ttu-id="87dba-3479">Die `Group`-Klausel hat dieselbe Syntax und Auswirkung wie ein `Select`-Abfrage Operator, mit dem Unterschied, dass Sie sich nur auf die in der `Into`-Klausel verfügbaren Bereichs Variablen und nicht auf die `By`-Klausel auswirkt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3479">The `Group` clause has the same syntax and effect as a `Select` query operator, except that it only affects the range variables available in the `Into` clause and not the `By` clause.</span></span> <span data-ttu-id="87dba-3480">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3480">For example:</span></span>

```vb
Dim averageAges = _
    From cust In Customers _
    Group cust.Age By cust.State _
    Into Count(), Average(Age)
```

<span data-ttu-id="87dba-3481">Die `By`-Klausel deklariert Ausdrucks Bereichs Variablen, die als Schlüsselwerte in der Gruppierungs Operation verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3481">The `By` clause declares expression range variables that are used as key values in the grouping operation.</span></span> <span data-ttu-id="87dba-3482">Die `Into`-Klausel ermöglicht die Deklaration von Ausdrucks Bereichs Variablen, die Aggregationen für die einzelnen Gruppen berechnen, die durch die `By`-Klausel gebildet werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3482">The `Into` clause allows the declaration of expression range variables that calculate aggregations over each of the groups formed by the `By` clause.</span></span> <span data-ttu-id="87dba-3483">Innerhalb der `Into`-Klausel kann der Ausdrucks Bereichs Variable nur ein Ausdruck zugewiesen werden, bei dem es sich um einen Methodenaufruf einer *Aggregatfunktion*handelt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3483">Within the `Into` clause, the expression range variable can only be assigned an expression which is a method invocation of an *aggregate function*.</span></span> <span data-ttu-id="87dba-3484">Eine Aggregatfunktion ist eine Funktion für den Auflistungstyp der Gruppe (bei der es sich nicht unbedingt um denselben Auflistungstyp der ursprünglichen Auflistung handeln kann), der wie eine der folgenden Methoden aussieht:</span><span class="sxs-lookup"><span data-stu-id="87dba-3484">An aggregate function is a function on the collection type of the group (which may not necessarily be the same collection type of the original collection) which looks like either of the following methods:</span></span>

```vb
Function _name_() As _type_
Function _name_(selector As Func(Of T, R)) As R
```

<span data-ttu-id="87dba-3485">Wenn eine Aggregatfunktion ein Delegatargument annimmt, kann der Aufruf Ausdruck einen Argument Ausdruck aufweisen, der als Wert klassifiziert werden muss.</span><span class="sxs-lookup"><span data-stu-id="87dba-3485">If an aggregate function takes a delegate argument, then the invocation expression can have an argument expression that must be classified as a value.</span></span>  <span data-ttu-id="87dba-3486">Der Argument Ausdruck kann die Bereichs Variablen verwenden, die sich im Gültigkeitsbereich befinden. innerhalb des Aufrufes einer Aggregatfunktion stellen diese Bereichs Variablen die Werte in der Gruppe dar, die gebildet werden, und nicht alle Werte in der Auflistung.</span><span class="sxs-lookup"><span data-stu-id="87dba-3486">The argument expression can use the range variables that are in scope; within the call to an aggregate function, those range variables represent the values in the group being formed, not all of the values in the collection.</span></span> <span data-ttu-id="87dba-3487">Im ursprünglichen Beispiel in diesem Abschnitt berechnet die Funktion "`Average`" beispielsweise den Durchschnitt der Nutzungszeiten der Kunden pro Bundesstaat und nicht für alle Kunden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3487">For example, in the original example in this section the `Average` function calculates the average of the customers' ages per state rather than for all of the customers together.</span></span>

<span data-ttu-id="87dba-3488">Bei allen Auflistungs Typen wird davon ausgegangen, dass die Aggregatfunktion `Group` definiert ist, die keine Parameter annimmt und einfach die Gruppe zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3488">All collection types are considered to have the aggregate function `Group` defined on it, which takes no parameters and simply returns the group.</span></span> <span data-ttu-id="87dba-3489">Weitere Standard Aggregatfunktionen, die von einem Sammlungstyp bereitgestellt werden können, sind:</span><span class="sxs-lookup"><span data-stu-id="87dba-3489">Other standard aggregate functions that a collection type may provide are:</span></span>

<span data-ttu-id="87dba-3490">`Count` und `LongCount`, die die Anzahl der Elemente in der Gruppe oder die Anzahl der Elemente in der Gruppe zurückgeben, die einen booleschen Ausdruck erfüllen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3490">`Count` and `LongCount`, which return the count of the elements in the group or the count of the elements in the group that satisfy a Boolean expression.</span></span> <span data-ttu-id="87dba-3491">`Count` und `LongCount` werden nur unterstützt, wenn der Auflistungstyp eine der folgenden Methoden enthält:</span><span class="sxs-lookup"><span data-stu-id="87dba-3491">`Count` and `LongCount` are supported only if the collection type contains one of the methods:</span></span>

```vb
Function Count() As N
Function Count(selector As Func(Of T, B)) As N
Function LongCount() As N
Function LongCount(selector As Func(Of T, B)) As N
```

<span data-ttu-id="87dba-3492">`Sum`, wodurch die Summe eines Ausdrucks über alle Elemente in der Gruppe zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-3492">`Sum`, which returns the sum of an expression across all the elements in the group.</span></span> <span data-ttu-id="87dba-3493">`Sum` wird nur unterstützt, wenn der Auflistungstyp eine der folgenden Methoden enthält:</span><span class="sxs-lookup"><span data-stu-id="87dba-3493">`Sum` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Sum() As N
Function Sum(selector As Func(Of T, N)) As N
```

<span data-ttu-id="87dba-3494">`Min`, der den minimalen Wert eines Ausdrucks über alle Elemente in der Gruppe zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3494">`Min` which returns the minimum value of an expression across all the elements in the group.</span></span> <span data-ttu-id="87dba-3495">`Min` wird nur unterstützt, wenn der Auflistungstyp eine der folgenden Methoden enthält:</span><span class="sxs-lookup"><span data-stu-id="87dba-3495">`Min` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Min() As N
Function Min(selector As Func(Of T, N)) As N
```

<span data-ttu-id="87dba-3496">`Max`, wodurch der Höchstwert eines Ausdrucks über alle Elemente in der Gruppe zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-3496">`Max`, which returns the maximum value of an expression across all the elements in the group.</span></span> <span data-ttu-id="87dba-3497">`Max` wird nur unterstützt, wenn der Auflistungstyp eine der folgenden Methoden enthält:</span><span class="sxs-lookup"><span data-stu-id="87dba-3497">`Max` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Max() As N
Function Max(selector As Func(Of T, N)) As N
```

<span data-ttu-id="87dba-3498">`Average`, wodurch der Durchschnitt eines Ausdrucks über alle Elemente in der Gruppe zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-3498">`Average`, which returns the average of an expression across all the elements in the group.</span></span> <span data-ttu-id="87dba-3499">`Average` wird nur unterstützt, wenn der Auflistungstyp eine der folgenden Methoden enthält:</span><span class="sxs-lookup"><span data-stu-id="87dba-3499">`Average` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Average() As N
Function Average(selector As Func(Of T, N)) As N
```

<span data-ttu-id="87dba-3500">`Any`, der bestimmt, ob eine Gruppe Member enthält oder ob ein boolescher Ausdruck für ein beliebiges Element in der Gruppe true ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3500">`Any`, which determines whether a group contains members or if a Boolean expression is true for any element in the group.</span></span> <span data-ttu-id="87dba-3501">`Any` gibt einen Wert zurück, der in einem booleschen Ausdruck verwendet werden kann, und wird nur unterstützt, wenn der Auflistungstyp eine der-Methoden enthält:</span><span class="sxs-lookup"><span data-stu-id="87dba-3501">`Any` returns a value that can be used in a Boolean expression and is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Any() As B
Function Any(predicate As Func(Of T, B)) As B
```

<span data-ttu-id="87dba-3502">`All`, der bestimmt, ob ein boolescher Ausdruck für alle Elemente in der Gruppe "true" ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3502">`All`, which determines whether a Boolean expression is true for all elements in the group.</span></span> <span data-ttu-id="87dba-3503">`All` gibt einen Wert zurück, der in einem booleschen Ausdruck verwendet werden kann, und wird nur unterstützt, wenn der Auflistungstyp eine Methode enthält:</span><span class="sxs-lookup"><span data-stu-id="87dba-3503">`All` returns a value that can be used in a Boolean expression and is supported only if the collection type contains a method:</span></span>

```vb
Function All(predicate As Func(Of T, B)) As B
```

<span data-ttu-id="87dba-3504">Nach einem `Group By`-Abfrage Operator sind die Bereichs Variablen, die sich zuvor im Gültigkeitsbereich befinden, ausgeblendet, und die von den Klauseln `By` und `Into` eingeführten Bereichs Variablen sind verfügbar.</span><span class="sxs-lookup"><span data-stu-id="87dba-3504">After a `Group By` query operator, the range variables previously in scope are hidden, and the range variables introduced by the `By` and `Into` clauses are available.</span></span> <span data-ttu-id="87dba-3505">Ein `Group By`-Abfrage Operator wird nur unterstützt, wenn der Auflistungstyp die-Methode enthält:</span><span class="sxs-lookup"><span data-stu-id="87dba-3505">A `Group By` query operator is supported only if the collection type contains the method:</span></span>

```vb
Function GroupBy(keySelector As Func(Of T, K), _
                      resultSelector As Func(Of K, CT, R)) As CR
```

<span data-ttu-id="87dba-3506">Bereichs Variablen Deklarationen in der `Group`-Klausel werden nur unterstützt, wenn der Auflistungstyp die Methode enthält:</span><span class="sxs-lookup"><span data-stu-id="87dba-3506">Range variable declarations in the `Group` clause are supported only if the collection type contains the method:</span></span>

```vb
Function GroupBy(keySelector As Func(Of T, K), _
                      elementSelector As Func(Of T, S), _
                      resultSelector As Func(Of K, CS, R)) As CR
```

<span data-ttu-id="87dba-3507">Der Code</span><span class="sxs-lookup"><span data-stu-id="87dba-3507">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Group y = x * 10, z = x / 10 By evenOdd = x Mod 2 _
            Into Sum(y), Average(z) _
            ...
```

<span data-ttu-id="87dba-3508">wird im Allgemeinen in übersetzt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3508">is generally translated to</span></span>

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

<span data-ttu-id="87dba-3509">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-3509">__Note.__</span></span> <span data-ttu-id="87dba-3510">`Group`, `By` und `Into` sind keine reservierten Wörter.</span><span class="sxs-lookup"><span data-stu-id="87dba-3510">`Group`, `By`, and `Into` are not reserved words.</span></span>


### <a name="aggregate-query-operator"></a><span data-ttu-id="87dba-3511">Aggregat Abfrage Operator</span><span class="sxs-lookup"><span data-stu-id="87dba-3511">Aggregate Query Operator</span></span>

<span data-ttu-id="87dba-3512">Der `Aggregate`-Abfrage Operator führt eine ähnliche Funktion wie der `Group By`-Operator aus, außer er ermöglicht das aggregierten von Gruppen, die bereits gebildet wurden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3512">The `Aggregate` query operator performs a similar function as the `Group By` operator, except it allows aggregating over groups that have already been formed.</span></span> <span data-ttu-id="87dba-3513">Da die Gruppe bereits gebildet wurde, verbirgt die `Into`-Klausel eines `Aggregate`-Abfrage Operators nicht die Bereichs Variablen im Gültigkeitsbereich. (auf diese Weise ist `Aggregate` eher ähnlich wie ein `Let`, und `Group By` ist eher ein `Select`).</span><span class="sxs-lookup"><span data-stu-id="87dba-3513">Because the group has already been formed, the `Into` clause of an `Aggregate` query operator does not hide the range variables in scope (in this way, `Aggregate` is more like a `Let`, and `Group By` is more like a `Select`).</span></span>

```antlr
AggregateQueryOperator
    : LineTerminator? 'Aggregate' LineTerminator? CollectionRangeVariableDeclaration QueryOperator*
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="87dba-3514">Die folgende Abfrage aggregiert z. b. die Summe aller Bestellungen, die von Kunden in Washington gestellt werden:</span><span class="sxs-lookup"><span data-stu-id="87dba-3514">For example, the following query aggregates the total of all the orders placed by customers in Washington:</span></span>

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Into Sum(order.Total)
```

<span data-ttu-id="87dba-3515">Das Ergebnis dieser Abfrage ist eine Auflistung, deren Elementtyp ein anonymer Typ mit einer Eigenschaft namens "`cust`" ist, die als `Customer` typisiert ist, und eine Eigenschaft mit dem Namen "`Sum`" als "`Integer`"</span><span class="sxs-lookup"><span data-stu-id="87dba-3515">The result of this query is a collection whose element type is an anonymous type with a property named `cust` typed as `Customer` and a property named `Sum` typed as `Integer`.</span></span>

<span data-ttu-id="87dba-3516">Im Gegensatz zu `Group By` können zusätzliche Abfrage Operatoren zwischen den Klauseln `Aggregate` und `Into` platziert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3516">Unlike `Group By`, additional query operators can be placed between the `Aggregate` and `Into` clauses.</span></span> <span data-ttu-id="87dba-3517">Zwischen einer `Aggregate`-Klausel und dem Ende der `Into`-Klausel können alle Bereichs Variablen im Gültigkeitsbereich verwendet werden, einschließlich derjenigen, die von der `Aggregate`-Klausel deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3517">Between an `Aggregate` clause and the end of the `Into` clause, all range variables in scope, including those declared by the `Aggregate` clause can be used.</span></span> <span data-ttu-id="87dba-3518">Die folgende Abfrage aggregiert z. b. die Gesamtsumme aller Bestellungen, die von Kunden in Washington vor 2006 platziert werden:</span><span class="sxs-lookup"><span data-stu-id="87dba-3518">For example, the following query aggregates the sum total of all the orders placed by customers in Washington before 2006:</span></span>

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum = Sum(order.Total)
```

<span data-ttu-id="87dba-3519">Der `Aggregate`-Operator kann auch zum Starten eines Abfrage Ausdrucks verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3519">The `Aggregate` operator can also be used to start a query expression.</span></span> <span data-ttu-id="87dba-3520">In diesem Fall ist das Ergebnis des Abfrage Ausdrucks der einzelne Wert, der durch die `Into`-Klausel berechnet wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-3520">In this case, the result of the query expression will be the single value computed by the `Into` clause.</span></span> <span data-ttu-id="87dba-3521">Die folgende Abfrage berechnet z. b. die Summe aller Bestell Summen vor dem 1. Januar 2006:</span><span class="sxs-lookup"><span data-stu-id="87dba-3521">For example, the following query calculates the sum of all the order totals before January 1st, 2006:</span></span>

```vb
Dim ordersTotal = _
    Aggregate order In Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum(order.Total)
```

<span data-ttu-id="87dba-3522">Das Ergebnis der Abfrage ist ein einzelner `Integer`-Wert.</span><span class="sxs-lookup"><span data-stu-id="87dba-3522">The result of the query is a single `Integer` value.</span></span> <span data-ttu-id="87dba-3523">Es ist immer ein `Aggregate`-Abfrage Operator verfügbar (obwohl die Aggregatfunktion auch verfügbar sein muss, damit der Ausdruck gültig ist).</span><span class="sxs-lookup"><span data-stu-id="87dba-3523">An `Aggregate` query operator is always available (although the aggregate function must be also be available for the expression to be valid).</span></span> <span data-ttu-id="87dba-3524">Der Code</span><span class="sxs-lookup"><span data-stu-id="87dba-3524">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    Aggregate x In xs _
    Where x < 5 _
    Into Sum()
```

<span data-ttu-id="87dba-3525">wird im Allgemeinen in übersetzt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3525">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x) x < 5).Sum()
```

<span data-ttu-id="87dba-3526">__Beachten Sie.__  `Aggregate`</span><span class="sxs-lookup"><span data-stu-id="87dba-3526">__Note.__ `Aggregate`</span></span> <span data-ttu-id="87dba-3527">und `Into` sind keine reservierten Wörter.</span><span class="sxs-lookup"><span data-stu-id="87dba-3527">and `Into` are not reserved words.</span></span>


### <a name="group-join-query-operator"></a><span data-ttu-id="87dba-3528">Group Join Query-Operator</span><span class="sxs-lookup"><span data-stu-id="87dba-3528">Group Join Query Operator</span></span>

<span data-ttu-id="87dba-3529">Der `Group Join`-Abfrage Operator kombiniert die Funktionen der Abfrage Operatoren `Join` und `Group By` zu einem einzigen Operator.</span><span class="sxs-lookup"><span data-stu-id="87dba-3529">The `Group Join` query operator combines the functions of the `Join` and `Group By` query operators into a single operator.</span></span> <span data-ttu-id="87dba-3530">`Group Join` verknüpft zwei Auflistungen basierend auf übereinstimmenden Schlüsseln, die aus den Elementen extrahiert wurden, und gruppiert alle Elemente auf der rechten Seite des Joins, die mit einem bestimmten Element auf der linken Seite des Joins übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3530">`Group Join` joins two collections based on matching keys extracted from the elements, grouping together all of the elements on the right side of the join that match a particular element on the left side of the join.</span></span> <span data-ttu-id="87dba-3531">Folglich erzeugt der-Operator eine Reihe von hierarchischen Ergebnissen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3531">Thus, the operator produces a set of hierarchical results.</span></span>

```antlr
GroupJoinQueryOperator
    : LineTerminator? 'Group' 'Join' LineTerminator? CollectionRangeVariableDeclaration
      JoinOrGroupJoinQueryOperator? LineTerminator? 'On' LineTerminator? JoinConditionList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="87dba-3532">Beispielsweise werden mit der folgenden Abfrage Elemente erstellt, die den Namen eines einzelnen Kunden, eine Gruppe aller Bestellungen und die Gesamtmenge aller Bestellungen enthalten:</span><span class="sxs-lookup"><span data-stu-id="87dba-3532">For example, the following query produces elements that contain a single customer's name, a group of all of their orders, and the total amount of all of those orders:</span></span>

```vb
Dim custsWithOrders = _
    From cust In Customers _
    Group Join order In Orders On cust.ID Equals order.CustomerID _ 
    Into Orders = Group, OrdersTotal = Sum(order.Total) _
    Select cust.Name, Orders, OrdersTotal
```

<span data-ttu-id="87dba-3533">Das Ergebnis der Abfrage ist eine Auflistung, deren Elementtyp ein anonymer Typ mit drei Eigenschaften ist: `Name`, typisiert als `String`, `Orders` typisiert als Auflistung, deren Elementtyp `Order` und `OrdersTotal`, typisiert als `Integer`.</span><span class="sxs-lookup"><span data-stu-id="87dba-3533">The result of the query is a collection whose element type is an anonymous type with three properties: `Name`, typed as `String`, `Orders` typed as a collection whose element type is `Order`, and `OrdersTotal`, typed as `Integer`.</span></span> <span data-ttu-id="87dba-3534">Ein `Group Join`-Abfrage Operator wird nur unterstützt, wenn der Auflistungstyp die-Methode enthält:</span><span class="sxs-lookup"><span data-stu-id="87dba-3534">A `Group Join` query operator is supported only if the collection type contains the method:</span></span>

```vb
Function GroupJoin(inner As CS, _
                         outerSelector As Func(Of T, K), _
                         innerSelector As Func(Of S, K), _
                         resultSelector As Func(Of T, CS, R)) As CR
```

<span data-ttu-id="87dba-3535">Der Code</span><span class="sxs-lookup"><span data-stu-id="87dba-3535">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs _
            Group Join y in ys On x Equals y _
            Into g = Group _
            ...
```

<span data-ttu-id="87dba-3536">wird im Allgemeinen in übersetzt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3536">is generally translated to</span></span>

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

<span data-ttu-id="87dba-3537">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-3537">__Note.__</span></span> <span data-ttu-id="87dba-3538">`Group`, `Join` und `Into` sind keine reservierten Wörter.</span><span class="sxs-lookup"><span data-stu-id="87dba-3538">`Group`, `Join`, and `Into` are not reserved words.</span></span>


## <a name="conditional-expressions"></a><span data-ttu-id="87dba-3539">Bedingte Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-3539">Conditional Expressions</span></span>

<span data-ttu-id="87dba-3540">Ein bedingter `If`-Ausdruck testet einen Ausdruck und gibt einen Wert zurück.</span><span class="sxs-lookup"><span data-stu-id="87dba-3540">A conditional `If` expression tests an expression and returns a value.</span></span>

```antlr
ConditionalExpression
    : 'If' OpenParenthesis BooleanExpression Comma Expression Comma Expression CloseParenthesis
    | 'If' OpenParenthesis Expression Comma Expression CloseParenthesis
    ;
```

<span data-ttu-id="87dba-3541">Anders als die Lauf Zeitfunktion von `IIF` wertet ein bedingter Ausdruck jedoch bei Bedarf nur seine Operanden aus.</span><span class="sxs-lookup"><span data-stu-id="87dba-3541">Unlike the `IIF` runtime function, however, a conditional expression only evaluates its operands if necessary.</span></span> <span data-ttu-id="87dba-3542">So löst z. b. der Ausdruck `If(c Is Nothing, c.Name, "Unknown")` keine Ausnahme aus, wenn der Wert von `c` `Nothing` ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3542">Thus, for example, the expression `If(c Is Nothing, c.Name, "Unknown")` will not throw an exception if the value of `c` is `Nothing`.</span></span> <span data-ttu-id="87dba-3543">Der bedingte Ausdruck verfügt über zwei Formen: eine mit zwei Operanden und eine, die drei Operanden annimmt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3543">The conditional expression has two forms: one that takes two operands and one that takes three operands.</span></span>

<span data-ttu-id="87dba-3544">Wenn drei Operanden bereitgestellt werden, müssen alle drei Ausdrücke als Werte klassifiziert werden, und der erste Operand muss ein boolescher Ausdruck sein.</span><span class="sxs-lookup"><span data-stu-id="87dba-3544">If three operands are provided, all three expressions must be classified as values, and the first operand must be a Boolean expression.</span></span> <span data-ttu-id="87dba-3545">Wenn das Ergebnis des Ausdrucks "true" ist, ist der zweite Ausdruck das Ergebnis des Operators. andernfalls ist der dritte Ausdruck das Ergebnis des Operators.</span><span class="sxs-lookup"><span data-stu-id="87dba-3545">If the result is of the expression is true, then the second expression will be the result of the operator, otherwise the third expression will be the result of the operator.</span></span> <span data-ttu-id="87dba-3546">Der Ergebnistyp des Ausdrucks ist der bestimmende Typ zwischen den Typen des zweiten und dritten Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="87dba-3546">The result type of the expression is the dominant type between the types of the second and third expression.</span></span> <span data-ttu-id="87dba-3547">Wenn kein dominanter Typ vorhanden ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-3547">If there is no dominant type, then a compile-time error occurs.</span></span>

<span data-ttu-id="87dba-3548">Wenn zwei Operanden bereitgestellt werden, müssen beide Operanden als Werte klassifiziert werden, und der erste Operand muss entweder ein Referenztyp oder ein Werte zulässt-Werttyp sein.</span><span class="sxs-lookup"><span data-stu-id="87dba-3548">If two operands are provided, both of the operands must be classified as values, and the first operand must be either a reference type or a nullable value type.</span></span> <span data-ttu-id="87dba-3549">Der Ausdruck `If(x, y)` wird dann so ausgewertet, als wäre der Ausdruck `If(x IsNot Nothing, x, y)` mit zwei Ausnahmen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3549">The expression `If(x, y)` is then evaluated as if was the expression `If(x IsNot Nothing, x, y)`, with two exceptions.</span></span> <span data-ttu-id="87dba-3550">Zuerst wird der erste Ausdruck nur einmal ausgewertet, und zweitens, wenn der Typ des zweiten Operanden ein Werttyp ist, der keine NULL-Werte zulässt, und der Typ des ersten Operanden ist, wird der `?` aus dem Typ des ersten Operanden entfernt, wenn der bestimmende Typ für das Ergebnis bestimmt wird. der Typ des Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="87dba-3550">First, the first expression is only ever evaluated once, and second, if the second operand's type is a non-nullable value type and the first operand's type is, the `?` is removed from the type of the first operand when determining the dominant type for the result type of the expression.</span></span> <span data-ttu-id="87dba-3551">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3551">For example:</span></span>

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

<span data-ttu-id="87dba-3552">In beiden Formen des Ausdrucks wird der Typ, wenn ein Operand `Nothing` ist, nicht zum Bestimmen des vorherrschenden Typs verwendet.</span><span class="sxs-lookup"><span data-stu-id="87dba-3552">In both forms of the expression, if an operand is `Nothing`, its type is not used to determine the dominant type.</span></span> <span data-ttu-id="87dba-3553">Im Fall des Ausdrucks `If(<expression>, Nothing, Nothing)` gilt der vorherrschende Typ als `Object`.</span><span class="sxs-lookup"><span data-stu-id="87dba-3553">In the case of the expression `If(<expression>, Nothing, Nothing)`, the dominant type is considered to be `Object`.</span></span>


## <a name="xml-literal-expressions"></a><span data-ttu-id="87dba-3554">XML-Literale Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-3554">XML Literal Expressions</span></span>

<span data-ttu-id="87dba-3555">Ein XML-Literalausdruck stellt einen XML-Wert (Extensible Markup Language) 1,0 dar.</span><span class="sxs-lookup"><span data-stu-id="87dba-3555">An XML literal expression represents an XML (eXtensible Markup Language) 1.0 value.</span></span>

```antlr
XMLLiteralExpression
    : XMLDocument
    | XMLElement
    | XMLProcessingInstruction
    | XMLComment
    | XMLCDATASection
    ;
```

<span data-ttu-id="87dba-3556">Das Ergebnis eines XML-Literalen Ausdrucks ist ein Wert, der als einer der Typen aus dem `System.Xml.Linq`-Namespace typisiert ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3556">The result of an XML literal expression is a value typed as one of the types from the `System.Xml.Linq` namespace.</span></span> <span data-ttu-id="87dba-3557">Wenn die Typen in diesem Namespace nicht verfügbar sind, führt ein XML-Literalausdruck zu einem Kompilierzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="87dba-3557">If the types in that namespace are not available, then an XML literal expression will cause a compile-time error.</span></span> <span data-ttu-id="87dba-3558">Die Werte werden durch Konstruktoraufrufe generiert, die aus dem XML-Literalausdruck übersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3558">The values are generated through constructor calls translated from the XML literal expression.</span></span> <span data-ttu-id="87dba-3559">Beispielsweise ist der Code:</span><span class="sxs-lookup"><span data-stu-id="87dba-3559">For example, the code:</span></span>

```vb
Dim book As System.Xml.Linq.XElement = _
    <book title="My book"></book>
```

<span data-ttu-id="87dba-3560">entspricht ungefähr dem Code:</span><span class="sxs-lookup"><span data-stu-id="87dba-3560">is roughly equivalent to the code:</span></span>

```vb
Dim book As System.Xml.Linq.XElement = _
    New System.Xml.Linq.XElement( _
        "book", _
        New System.Xml.Linq.XAttribute("title", "My book"))
```

<span data-ttu-id="87dba-3561">Ein XML-Literalausdruck kann die Form eines XML-Dokuments, ein XML-Element, eine XML-Verarbeitungsanweisung, einen XML-Kommentar oder einen CDATA-Abschnitt annehmen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3561">An XML literal expression can take the form of an XML document, an XML element, an XML processing instruction, an XML comment, or a CDATA section.</span></span>

<span data-ttu-id="87dba-3562">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-3562">__Note.__</span></span> <span data-ttu-id="87dba-3563">Diese Spezifikation enthält nur eine ausreichende Beschreibung von XML, um das Verhalten der Visual Basic Sprache zu beschreiben.</span><span class="sxs-lookup"><span data-stu-id="87dba-3563">This specification contains only enough of a description of XML to describe the behavior of the Visual Basic language.</span></span> <span data-ttu-id="87dba-3564">Weitere Informationen zu XML finden Sie unter http://www.w3.org/TR/REC-xml/.</span><span class="sxs-lookup"><span data-stu-id="87dba-3564">More information on XML can be found at http://www.w3.org/TR/REC-xml/.</span></span>


### <a name="lexical-rules"></a><span data-ttu-id="87dba-3565">Lexikalische Regeln</span><span class="sxs-lookup"><span data-stu-id="87dba-3565">Lexical rules</span></span>

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

<span data-ttu-id="87dba-3566">XML-Literale Ausdrücke werden mithilfe der lexikalischen Regeln von XML interpretiert, anstelle der lexikalischen Regeln regulärer Visual Basic Codes.</span><span class="sxs-lookup"><span data-stu-id="87dba-3566">XML literal expressions are interpreted using the lexical rules of XML instead of the lexical rules of regular Visual Basic code.</span></span> <span data-ttu-id="87dba-3567">Die beiden Regelsätze unterscheiden sich in der Regel in folgenden Punkten:</span><span class="sxs-lookup"><span data-stu-id="87dba-3567">The two sets of rules generally differ in the following ways:</span></span>

* <span data-ttu-id="87dba-3568">Leerraum ist in XML signifikant.</span><span class="sxs-lookup"><span data-stu-id="87dba-3568">White space is significant in XML.</span></span> <span data-ttu-id="87dba-3569">Daher gibt die Grammatik für XML-Literalausdrücke explizit an, wo Leerraum zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3569">As a result, the grammar for XML literal expressions explicitly states where white space is allowed.</span></span> <span data-ttu-id="87dba-3570">Leerzeichen werden nicht beibehalten, es sei denn, Sie treten im Kontext von Zeichendaten in einem Element auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-3570">Whitespace is not preserved, except when it occurs in the context of character data within an element.</span></span> <span data-ttu-id="87dba-3571">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3571">For example:</span></span>

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

* <span data-ttu-id="87dba-3572">XML-zeileendeleerräume werden entsprechend der XML-Spezifikation normalisiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-3572">XML end-of-line whitespace is normalized according to the XML specification.</span></span>

* <span data-ttu-id="87dba-3573">Bei XML muss die Groß- und Kleinschreibung beachtet werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3573">XML is case-sensitive.</span></span> <span data-ttu-id="87dba-3574">Schlüsselwörter müssen exakt mit der Groß-/Kleinschreibung übereinstimmen, andernfalls tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-3574">Keywords must match casing exactly, or else a compile-time error will occur.</span></span>

* <span data-ttu-id="87dba-3575">Zeilen Abschluss Zeichen werden als Leerzeichen in XML betrachtet.</span><span class="sxs-lookup"><span data-stu-id="87dba-3575">Line terminators are considered white space in XML.</span></span> <span data-ttu-id="87dba-3576">Folglich werden in XML-Literalausdrücken keine Zeilen Fortsetzungs Zeichen benötigt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3576">As a result, no line-continuation characters are needed in XML literal expressions.</span></span>

* <span data-ttu-id="87dba-3577">XML akzeptiert keine Zeichen voller Breite.</span><span class="sxs-lookup"><span data-stu-id="87dba-3577">XML does not accept full-width characters.</span></span> <span data-ttu-id="87dba-3578">Wenn Zeichen in voller Breite verwendet werden, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-3578">If full-width characters are used, a compile-time error will occur.</span></span>


### <a name="embedded-expressions"></a><span data-ttu-id="87dba-3579">Eingebettete Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-3579">Embedded expressions</span></span>

<span data-ttu-id="87dba-3580">XML-Literalausdrücke können *eingebettete Ausdrücke*enthalten.</span><span class="sxs-lookup"><span data-stu-id="87dba-3580">XML literal expressions can contain *embedded expressions*.</span></span> <span data-ttu-id="87dba-3581">Ein eingebetteter Ausdruck ist ein Visual Basic Ausdruck, der ausgewertet und zum Ausfüllen eines oder mehrerer Werte am Speicherort des eingebetteten Ausdrucks verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-3581">An embedded expression is a Visual Basic expression that is evaluated and used to fill in one or more values at the location of embedded expression.</span></span>

```antlr
XMLEmbeddedExpression
    : '<' '%' '=' LineTerminator? Expression LineTerminator? '%' '>'
    ;
```

<span data-ttu-id="87dba-3582">Im folgenden Code wird z. b. die Zeichenfolge `John Smith` als Wert des XML-Elements platziert:</span><span class="sxs-lookup"><span data-stu-id="87dba-3582">For example, the following code places the string `John Smith` as the value of the XML element:</span></span>

```vb
Dim name as String = "John Smith"
Dim element As System.Xml.Linq.XElement = <customer><%= name %></customer>
```

<span data-ttu-id="87dba-3583">Ausdrücke können in eine Reihe von Kontexten eingebettet werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3583">Expressions can be embedded in a number of contexts.</span></span> <span data-ttu-id="87dba-3584">Der folgende Code erzeugt z. b. ein Element mit dem Namen `customer`:</span><span class="sxs-lookup"><span data-stu-id="87dba-3584">For example, the following code produces an element named `customer`:</span></span>

```vb
Dim name As String = "customer"
Dim element As System.Xml.Linq.XElement = <<%= name %>>John Smith</>
```

<span data-ttu-id="87dba-3585">Jeder Kontext, in dem ein eingebetteter Ausdruck verwendet werden kann, gibt die Typen an, die akzeptiert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3585">Each context where an embedded expression can be used specifies the types that will be accepted.</span></span> <span data-ttu-id="87dba-3586">Im Kontext des Ausdrucks Teils eines eingebetteten Ausdrucks gelten die normalen lexikalischen Regeln für Visual Basic Code weiterhin, sodass z. b. Zeilen Fortsetzungen verwendet werden müssen:</span><span class="sxs-lookup"><span data-stu-id="87dba-3586">When within the context of the expression part of an embedded expression, the normal lexical rules for Visual Basic code still apply so, for example, line continuations must be used:</span></span>

```vb
' Visual Basic expression uses line continuation, XML does not
Dim element As System.Xml.Linq.XElement = _
    <<%= name & _
          name %>>John 
                     Smith</>
```


### <a name="xml-documents"></a><span data-ttu-id="87dba-3587">XML-Dokumente</span><span class="sxs-lookup"><span data-stu-id="87dba-3587">XML Documents</span></span>

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

<span data-ttu-id="87dba-3588">Ein XML-Dokument führt zu einem Wert, der als `System.Xml.Linq.XDocument` typisiert ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3588">An XML document results in a value typed as `System.Xml.Linq.XDocument`.</span></span> <span data-ttu-id="87dba-3589">Anders als bei der XML 1,0-Spezifikation sind XML-Dokumente in XML-Literalausdrücken zum Angeben des XML-Dokument Prologs erforderlich. XML-Literale Ausdrücke ohne XML-Dokument Prolog werden als ihre einzelne Entität interpretiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-3589">Unlike the XML 1.0 specification, XML documents in XML literal expressions are required to specify the XML document prologue; XML literal expressions without the XML document prologue are interpreted as their individual entity.</span></span> <span data-ttu-id="87dba-3590">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3590">For example:</span></span>

```vb
Dim doc As System.Xml.Linq.XDocument = _
    <?xml version="1.0"?>
    <?instruction?>
    <customer>Bob</>

Dim pi As System.Xml.Linq.XProcessingInstruction = _
    <?instruction?>
```

<span data-ttu-id="87dba-3591">Ein XML-Dokument kann einen eingebetteten Ausdruck enthalten, dessen Typ einen beliebigen Typ aufweisen kann. zur Laufzeit muss das Objekt jedoch die Anforderungen des `XDocument`-Konstruktors erfüllen, oder es tritt ein Laufzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-3591">An XML document can contain an embedded expression whose type can be any type; at runtime, however, the object must satisfy the requirements of the `XDocument` constructor or a run-time error will occur.</span></span>

<span data-ttu-id="87dba-3592">Im Gegensatz zu regulären XML-Dokumenten unterstützen XML-Dokument Ausdrücke keine DTDs (Dokumenttyp Deklarationen).</span><span class="sxs-lookup"><span data-stu-id="87dba-3592">Unlike regular XML, XML document expressions do not support DTDs (Document Type Declarations).</span></span> <span data-ttu-id="87dba-3593">Außerdem wird das Codierungs Attribut, sofern angegeben, ignoriert, da die Codierung des XML-Literalen Ausdrucks immer mit der Codierung der Quelldatei identisch ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3593">Also, the encoding attribute, if supplied, will be ignored since the encoding of the Xml literal expression is always the same as the encoding of the source file itself.</span></span>

<span data-ttu-id="87dba-3594">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-3594">__Note.__</span></span> <span data-ttu-id="87dba-3595">Obwohl das Encoding-Attribut ignoriert wird, ist es immer noch ein gültiges Attribut, um die Möglichkeit aufrechtzuerhalten, gültige XML 1,0-Dokumente in den Quellcode aufzunehmen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3595">Although the encoding attribute is ignored, it is still valid attribute in order to maintain the ability to include any valid Xml 1.0 documents in source code.</span></span>


### <a name="xml-elements"></a><span data-ttu-id="87dba-3596">XML-Elemente</span><span class="sxs-lookup"><span data-stu-id="87dba-3596">XML Elements</span></span>

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

<span data-ttu-id="87dba-3597">Ein XML-Element führt zu einem Wert, der als `System.Xml.Linq.XElement` typisiert ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3597">An XML element results in a value typed as `System.Xml.Linq.XElement`.</span></span> <span data-ttu-id="87dba-3598">Im Gegensatz zum regulären XML-Code können XML-Elemente den Namen im schließenden Tag weglassen, und das aktuelle am meisten Nested Element wird geschlossen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3598">Unlike regular XML, XML elements can omit the name in the closing tag and the current most-nested element will be closed.</span></span> <span data-ttu-id="87dba-3599">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3599">For example:</span></span>

```vb
Dim name = <name>Bob</>
```

<span data-ttu-id="87dba-3600">Attribut Deklarationen in einem XML-Element führen zu Werten, die als `System.Xml.Linq.XAttribute` eingegeben werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3600">Attribute declarations in an XML element result in values typed as `System.Xml.Linq.XAttribute`.</span></span> <span data-ttu-id="87dba-3601">Attributwerte werden entsprechend der XML-Spezifikation normalisiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-3601">Attribute values are normalized according to the XML specification.</span></span> <span data-ttu-id="87dba-3602">Wenn der Wert eines Attributs `Nothing` ist, wird das Attribut nicht erstellt, sodass der Attribut Wertausdruck nicht auf `Nothing` geprüft werden muss.</span><span class="sxs-lookup"><span data-stu-id="87dba-3602">When the value of an attribute is `Nothing` the attribute will not be created, so the attribute value expression will not have to be checked for `Nothing`.</span></span> <span data-ttu-id="87dba-3603">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3603">For example:</span></span>

```vb
Dim expr = Nothing

' Throws null argument exception
Dim direct = New System.Xml.Linq.XElement( _
    "Name", _
    New System.Xml.Linq.XAttribute("Length", expr))

' Doesn't throw exception, the result is <Name/>
Dim literal = <Name Length=<%= expr %>/>
```

<span data-ttu-id="87dba-3604">XML-Elemente und-Attribute können an den folgenden Stellen die folgenden Elemente enthalten:</span><span class="sxs-lookup"><span data-stu-id="87dba-3604">XML elements and attributes can contain nested expressions in the following places:</span></span>

<span data-ttu-id="87dba-3605">Der Name des Elements. in diesem Fall muss der eingebettete Ausdruck ein Wert eines Typs sein, der implizit in `System.Xml.Linq.XName` konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="87dba-3605">The name of the element, in which case the embedded expression must be a value of a type implicitly convertible to `System.Xml.Linq.XName`.</span></span> <span data-ttu-id="87dba-3606">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3606">For example:</span></span>

```vb
Dim name = <<%= "name" %>>Bob</>
```

<span data-ttu-id="87dba-3607">Der Name eines Attributs des Elements. in diesem Fall muss der eingebettete Ausdruck ein Wert eines Typs sein, der implizit in `System.Xml.Linq.XName` konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="87dba-3607">The name of an attribute of the element, in which case the embedded expression must be a value of a type implicitly convertible to `System.Xml.Linq.XName`.</span></span> <span data-ttu-id="87dba-3608">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3608">For example:</span></span>

```vb
Dim name = <name <%= "length" %>="3">Bob</>
```

<span data-ttu-id="87dba-3609">Der Wert eines Attributs des Elements. in diesem Fall kann der eingebettete Ausdruck ein Wert eines beliebigen Typs sein.</span><span class="sxs-lookup"><span data-stu-id="87dba-3609">The value of an attribute of the element, in which case the embedded expression can be a value of any type.</span></span> <span data-ttu-id="87dba-3610">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3610">For example:</span></span>

```vb
Dim name = <name length=<%= 3 %>>Bob</>
```

<span data-ttu-id="87dba-3611">Ein Attribut des-Elements. in diesem Fall kann der eingebettete Ausdruck ein Wert eines beliebigen Typs sein.</span><span class="sxs-lookup"><span data-stu-id="87dba-3611">An attribute of the element, in which case the embedded expression can be a value of any type.</span></span> <span data-ttu-id="87dba-3612">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3612">For example:</span></span>

```vb
Dim name = <name <%= new XAttribute("length", 3) %>>Bob</>
```

<span data-ttu-id="87dba-3613">Der Inhalt des Elements. in diesem Fall kann der eingebettete Ausdruck ein Wert eines beliebigen Typs sein.</span><span class="sxs-lookup"><span data-stu-id="87dba-3613">The content of the element, in which case the embedded expression can be a value of any type.</span></span> <span data-ttu-id="87dba-3614">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3614">For example:</span></span>

```vb
Dim name = <name><%= "Bob" %></>
```

<span data-ttu-id="87dba-3615">Wenn der Typ des eingebetteten Ausdrucks `Object()` ist, wird das Array als ParamArray an den `XElement`-Konstruktor übergeben.</span><span class="sxs-lookup"><span data-stu-id="87dba-3615">If the type of the embedded expression is `Object()`, the array will be passed as a paramarray to the `XElement` constructor.</span></span>


### <a name="xml-namespaces"></a><span data-ttu-id="87dba-3616">XML-Namespaces</span><span class="sxs-lookup"><span data-stu-id="87dba-3616">XML Namespaces</span></span>

<span data-ttu-id="87dba-3617">XML-Elemente können XML-Namespace Deklarationen enthalten, wie in der Spezifikation für XML-Namespaces 1,0 definiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-3617">XML elements can contain XML namespace declarations, as defined by the XML namespaces 1.0 specification.</span></span>

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

<span data-ttu-id="87dba-3618">Die Einschränkungen beim Definieren der Namespaces `xml` und `xmlns` werden erzwungen und verursachen Kompilierzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="87dba-3618">The restrictions on defining the namespaces `xml` and `xmlns` are enforced and will produce compile-time errors.</span></span> <span data-ttu-id="87dba-3619">XML-Namespace Deklarationen können keinen eingebetteten Ausdruck für ihren Wert aufweisen. der angegebene Wert muss ein nicht leeres Zeichenfolgenliteralzeichen sein.</span><span class="sxs-lookup"><span data-stu-id="87dba-3619">XML namespace declarations cannot have an embedded expression for their value; the value supplied must be a non-empty string literal.</span></span> <span data-ttu-id="87dba-3620">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3620">For example:</span></span>

```vb
' Declares a valid namespace
Dim customer = <db:customer xmlns:db="http://example.org/database">Bob</>

' Error: xmlns cannot be re-defined
Dim bad1 = <elem xmlns:xmlns="http://example.org/namespace"/>

' Error: cannot have an embedded expression
Dim bad2 = <elem xmlns:db=<%= "http://example.org/database" %>>Bob</>
```

<span data-ttu-id="87dba-3621">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-3621">__Note.__</span></span> <span data-ttu-id="87dba-3622">Diese Spezifikation enthält nur eine ausreichende Beschreibung des XML-Namespace, um das Verhalten der Visual Basic Sprache zu beschreiben.</span><span class="sxs-lookup"><span data-stu-id="87dba-3622">This specification contains only enough of a description of XML namespace to describe the behavior of the Visual Basic language.</span></span> <span data-ttu-id="87dba-3623">Weitere Informationen zu XML-Namespaces finden Sie unter http://www.w3.org/TR/REC-xml-names/.</span><span class="sxs-lookup"><span data-stu-id="87dba-3623">More information on XML namespaces can be found at http://www.w3.org/TR/REC-xml-names/.</span></span>

<span data-ttu-id="87dba-3624">XML-Element-und-Attributnamen können mithilfe von Namespace Namen qualifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3624">XML element and attribute names can be qualified using namespace names.</span></span> <span data-ttu-id="87dba-3625">Namespaces werden als in regulärem XML-Code gebunden, mit der Ausnahme, dass alle auf der Dateiebene deklarierten Namespace Importe als in einem Kontext deklariert werden, der die Deklaration einschließt, die selbst von allen von der Kompilierung deklarierten Namespace Importen eingeschlossen ist. Umgebung.</span><span class="sxs-lookup"><span data-stu-id="87dba-3625">Namespaces are bound as in regular XML, with the exception that any namespace imports declared at the file level are considered to be declared in a context enclosing the declaration, which is itself enclosed by any namespace imports declared by the compilation environment.</span></span> <span data-ttu-id="87dba-3626">Wenn kein Namespace Name gefunden werden kann, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="87dba-3626">If a namespace name cannot be found, a compile-time error occurs.</span></span> <span data-ttu-id="87dba-3627">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3627">For example:</span></span>

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

<span data-ttu-id="87dba-3628">In einem-Element deklarierte XML-Namespaces gelten nicht für XML-Literale innerhalb von eingebetteten Ausdrücken.</span><span class="sxs-lookup"><span data-stu-id="87dba-3628">XML namespaces declared in an element do not apply to XML literals inside embedded expressions.</span></span> <span data-ttu-id="87dba-3629">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3629">For example:</span></span>

```vb
' Error: Namespace prefix 'db' is not declared
Dim customer = _
    <db:customer xmlns:db="http://example.org/database">
        <%= <db:customer>Bob</> %>
    </>
```

<span data-ttu-id="87dba-3630">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-3630">__Note.__</span></span> <span data-ttu-id="87dba-3631">Dies liegt daran, dass der eingebettete Ausdruck etwas sein kann, einschließlich eines Funktions Aufrufes.</span><span class="sxs-lookup"><span data-stu-id="87dba-3631">This is because the embedded expression can be anything, including a function call.</span></span> <span data-ttu-id="87dba-3632">Wenn der Funktions Aufrufausdruck einen XML-Literalausdruck enthielt, ist nicht klar, ob Programmierer erwarten, dass der XML-Namespace angewendet oder ignoriert wird.</span><span class="sxs-lookup"><span data-stu-id="87dba-3632">If the function call contained an XML literal expression, it is not clear whether programmers would expect the XML namespace to be applied or ignored.</span></span>


### <a name="xml-processing-instructions"></a><span data-ttu-id="87dba-3633">XML-Verarbeitungsanweisungen</span><span class="sxs-lookup"><span data-stu-id="87dba-3633">XML Processing Instructions</span></span>

<span data-ttu-id="87dba-3634">Eine XML-Verarbeitungsanweisung führt zu einem Wert, der als `System.Xml.Linq.XProcessingInstruction` typisiert ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3634">An XML processing instruction results in a value typed as `System.Xml.Linq.XProcessingInstruction`.</span></span> <span data-ttu-id="87dba-3635">XML-Verarbeitungsanweisungen können keine eingebetteten Ausdrücke enthalten, da es sich um eine gültige Syntax innerhalb der Verarbeitungsanweisung handelt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3635">XML processing instructions cannot contain embedded expressions, as they are valid syntax within the processing instruction.</span></span>

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

### <a name="xml-comments"></a><span data-ttu-id="87dba-3636">XML-Kommentare</span><span class="sxs-lookup"><span data-stu-id="87dba-3636">XML Comments</span></span>

<span data-ttu-id="87dba-3637">Ein XML-Kommentar führt zu einem Wert, der als `System.Xml.Linq.XComment` typisiert ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3637">An XML comment results in a value typed as `System.Xml.Linq.XComment`.</span></span> <span data-ttu-id="87dba-3638">XML-Kommentare dürfen keine eingebetteten Ausdrücke enthalten, da es sich um eine gültige Syntax im Kommentar handelt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3638">XML comments cannot contain embedded expressions, as they are valid syntax within the comment.</span></span>

```antlr
XMLComment
    : '<' '!' '-' '-' XMLCommentCharacter* '-' '-' '>'
    ;

XMLCommentCharacter
    : '<Any XMLCharacter except dash (0x002D)>'
    | '-' '<Any XMLCharacter except dash (0x002D)>'
    ;
```

### <a name="cdata-sections"></a><span data-ttu-id="87dba-3639">CDATA-Abschnitte</span><span class="sxs-lookup"><span data-stu-id="87dba-3639">CDATA sections</span></span>

<span data-ttu-id="87dba-3640">Ein CDATA-Abschnitt führt zu einem Wert, der als `System.Xml.Linq.XCData` typisiert ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3640">A CDATA section results in a value typed as `System.Xml.Linq.XCData`.</span></span> <span data-ttu-id="87dba-3641">CDATA-Abschnitte können keine eingebetteten Ausdrücke enthalten, da es sich um eine gültige Syntax im CDATA-Abschnitt handelt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3641">CDATA sections cannot contain embedded expressions, as they are valid syntax within the CDATA section.</span></span>

```antlr
XMLCDATASection
    : '<' '!' ( 'CDATA' '[' XMLCDATASectionString? ']' )? '>'
    ;

XMLCDATASectionString
    : '<Any XMLString that does not contain the string "]]>">'
    ;
```

## <a name="xml-member-access-expressions"></a><span data-ttu-id="87dba-3642">XML-Member-Zugriffs Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="87dba-3642">XML Member Access Expressions</span></span>

<span data-ttu-id="87dba-3643">Ein XML-Member-Zugriffs Ausdruck greift auf die Member eines XML-Werts zu.</span><span class="sxs-lookup"><span data-stu-id="87dba-3643">An XML member access expression accesses the members of an XML value.</span></span>

```antlr
XMLMemberAccessExpression
    : Expression '.' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? IdentifierOrKeyword
    | Expression '.' '.' '.' LineTerminator? '<' XMLQualifiedName '>'
    ;
```

<span data-ttu-id="87dba-3644">Es gibt drei Typen von XML-Member-Zugriffs Ausdrücken:</span><span class="sxs-lookup"><span data-stu-id="87dba-3644">There are three types of XML member access expressions:</span></span>

* <span data-ttu-id="87dba-3645">*Element Zugriff*, bei dem ein XML-Name einem einzelnen Punkt folgt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3645">*Element access*, in which an XML name follows a single dot.</span></span> <span data-ttu-id="87dba-3646">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3646">For example:</span></span>

  ```vb
  Dim customer = _
      <customer>
          <name>Bob</>
      </>
  Dim customerName = customer.<name>.Value
  ```

  <span data-ttu-id="87dba-3647">Der Element Zugriff wird der-Funktion zugeordnet:</span><span class="sxs-lookup"><span data-stu-id="87dba-3647">Element access maps to the function:</span></span>

  ```vb
  Function Elements(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XNode)
  ```

  <span data-ttu-id="87dba-3648">Das obige Beispiel entspricht dem folgenden Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3648">So the above example is equivalent to:</span></span>

  ```vb
  Dim customerName = customer.Elements("name").Value
  ```

* <span data-ttu-id="87dba-3649">*Attribut Zugriff*, bei dem ein Visual Basic Bezeichner einem Punkt und einem at-Zeichen folgt, oder wenn ein XML-Name einem Punkt und einem @-Zeichen folgt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3649">*Attribute access*, in which a Visual Basic identifier follows a dot and an at sign, or an XML name follows a dot and an at sign.</span></span> <span data-ttu-id="87dba-3650">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3650">For example:</span></span>

  ```vb
  Dim customer = <customer age="30"/>
  Dim customerAge = customer.@age
  ```

  <span data-ttu-id="87dba-3651">Der Attribut Zugriff wird der-Funktion zugeordnet:</span><span class="sxs-lookup"><span data-stu-id="87dba-3651">Attribute access maps to the function:</span></span>

  ```vb
  Function AttributeValue(name As System.Xml.Linq.XName) as String
  ```

  <span data-ttu-id="87dba-3652">Das obige Beispiel entspricht dem folgenden Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3652">So the above example is equivalent to:</span></span>

  ```vb
  Dim customerAge = customer.AttributeValue("age")
  ```

  <span data-ttu-id="87dba-3653">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-3653">__Note.__</span></span> <span data-ttu-id="87dba-3654">Die `AttributeValue`-Erweiterungsmethode (sowie die zugehörige Erweiterungs Eigenschaft `Value`) ist zurzeit nicht in einer Assembly definiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-3654">The `AttributeValue` extension method (as well as the related extension property `Value`) is not currently defined in any assembly.</span></span> <span data-ttu-id="87dba-3655">Wenn die Erweiterungs Mitglieder benötigt werden, werden Sie automatisch in der erstellten Assembly definiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-3655">If the extension members are needed, they are automatically defined in the assembly being produced.</span></span>

* <span data-ttu-id="87dba-3656">Nachfolger *, bei*dem ein XML-Name drei Punkte folgt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3656">*Descendents access*, in which an XML names follows three dots.</span></span> <span data-ttu-id="87dba-3657">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3657">For example:</span></span>

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

  <span data-ttu-id="87dba-3658">Der Zugriff auf Nachfolger Elemente wird der-Funktion zugeordnet:</span><span class="sxs-lookup"><span data-stu-id="87dba-3658">Descendents access maps to the function:</span></span>

  ```vb
  Function Descendents(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XElement)
  ```

  <span data-ttu-id="87dba-3659">Das obige Beispiel entspricht dem folgenden Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3659">So the above example is equivalent to:</span></span>

  ```vb
  Dim customers = company.Descendants("customer")
  ```

<span data-ttu-id="87dba-3660">Der Basis Ausdruck eines XML-Member-Zugriffs Ausdrucks muss ein-Wert sein und muss den folgenden Typ aufweisen:</span><span class="sxs-lookup"><span data-stu-id="87dba-3660">The base expression of an XML member access expression must be a value and must be of the type:</span></span>

* <span data-ttu-id="87dba-3661">, Wenn ein Element oder Nachfolger, `System.Xml.Linq.XContainer` oder ein abgeleiteter Typ, oder `System.Collections.Generic.IEnumerable(Of T)` oder ein abgeleiteter Typ, wobei `T` `System.Xml.Linq.XContainer` oder ein abgeleiteter Typ ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3661">If an element or descendents access,  `System.Xml.Linq.XContainer` or a derived type, or `System.Collections.Generic.IEnumerable(Of T)` or a derived type, where `T` is `System.Xml.Linq.XContainer` or a derived type.</span></span>

* <span data-ttu-id="87dba-3662">Wenn ein Attribut Zugriff, `System.Xml.Linq.XElement` oder ein abgeleiteter Typ oder `System.Collections.Generic.IEnumerable(Of T)` oder ein abgeleiteter Typ, wobei `T` `System.Xml.Linq.XElement` oder ein abgeleiteter Typ ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3662">If an attribute access, `System.Xml.Linq.XElement` or a derived type, or `System.Collections.Generic.IEnumerable(Of T)` or a derived type, where `T` is `System.Xml.Linq.XElement` or a derived type.</span></span>

<span data-ttu-id="87dba-3663">Namen in XML-Member-Zugriffs Ausdrücken dürfen nicht leer sein.</span><span class="sxs-lookup"><span data-stu-id="87dba-3663">Names in XML member access expressions cannot be empty.</span></span> <span data-ttu-id="87dba-3664">Sie können mit einem Namespace qualifiziert sein, wobei alle durch Importe definierten Namespaces verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3664">They can be namespace qualified, using any namespaces defined by imports.</span></span> <span data-ttu-id="87dba-3665">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3665">For example:</span></span>

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

<span data-ttu-id="87dba-3666">Leerraum ist nach dem Punkt (n) in einem XML-Element Zugriffs Ausdruck oder zwischen den spitzen Klammern und dem Namen nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="87dba-3666">Whitespace is not allowed after the dot(s) in an XML member access expression, or between the angle brackets and the name.</span></span> <span data-ttu-id="87dba-3667">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="87dba-3667">For example:</span></span>

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

<span data-ttu-id="87dba-3668">Wenn die Typen im `System.Xml.Linq`-Namespace nicht verfügbar sind, führt ein XML-Member-Zugriffs Ausdruck zu einem Kompilierzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="87dba-3668">If the types in the `System.Xml.Linq` namespace are not available, then an XML member access expression will cause a compile-time error.</span></span>


## <a name="await-operator"></a><span data-ttu-id="87dba-3669">Await-Operator</span><span class="sxs-lookup"><span data-stu-id="87dba-3669">Await Operator</span></span>

<span data-ttu-id="87dba-3670">Der Erwartungs Operator bezieht sich auf Async-Methoden, die im Abschnitt " [Async-Methoden](statements.md#async-methods)" beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3670">The await operator is related to async methods, which are described in Section [Async Methods](statements.md#async-methods).</span></span>

```antlr
AwaitOperatorExpression
    : 'Await' Expression
    ;
```

<span data-ttu-id="87dba-3671">`Await` ist ein reserviertes Wort, wenn die unmittelbar einschließende Methode oder der Lambda-Ausdruck, in dem Sie angezeigt wird, einen `Async`-Modifizierer aufweist und der `Await` nach diesem `Async`-Modifizierer erscheint. Es ist nicht an anderer Stelle reserviert.</span><span class="sxs-lookup"><span data-stu-id="87dba-3671">`Await` is a reserved word if the immediately enclosing method or lambda expression in which it appears has an `Async` modifier, and if the `Await` appears after that `Async` modifier; it is unreserved elsewhere.</span></span> <span data-ttu-id="87dba-3672">Es ist auch nicht in Präprozessordirektiven reserviert.</span><span class="sxs-lookup"><span data-stu-id="87dba-3672">It is also unreserved in preprocessor directives.</span></span> <span data-ttu-id="87dba-3673">Der Erwartungs Operator ist nur im Text einer Methode oder eines Lambda-Ausdrucks zulässig, bei dem es sich um ein reserviertes Wort handelt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3673">The await operator is only allowed in the body of a method or lambda expressions where it is a reserved word.</span></span> <span data-ttu-id="87dba-3674">Innerhalb der unmittelbar einschließenden Methode oder des Lambda-Ausdrucks kann ein Erwartungs Ausdruck nicht innerhalb des Texts eines `Catch`-oder `Finally`-Blocks oder innerhalb des Texts einer `SyncLock`-Anweisung oder innerhalb eines Abfrage Ausdrucks auftreten.</span><span class="sxs-lookup"><span data-stu-id="87dba-3674">Within the immediately enclosing method or lambda, an await expression may not occur inside the body of a `Catch` or `Finally` block, nor inside the body of a `SyncLock` statement, nor inside a query expression.</span></span>

<span data-ttu-id="87dba-3675">Der Erwartungs Operator nimmt einen einzelnen Ausdruck an, der als Wert klassifiziert werden muss und *dessen Typ ein* erwartbarer Typ sein muss, oder `Object`.</span><span class="sxs-lookup"><span data-stu-id="87dba-3675">The await operator takes a single expression which must be classified as a value and whose type must be an *awaitable* type, or `Object`.</span></span> <span data-ttu-id="87dba-3676">Wenn der Typ `Object` ist, wird die gesamte Verarbeitung bis zur Laufzeit verzögert.</span><span class="sxs-lookup"><span data-stu-id="87dba-3676">If its type is `Object` then all processing is deferred until run-time.</span></span> <span data-ttu-id="87dba-3677">Ein Typ `C` wird als erwartbar bezeichnet, wenn Folgendes zutrifft:</span><span class="sxs-lookup"><span data-stu-id="87dba-3677">A type `C` is said to be awaitable if all of the following are true:</span></span>

* <span data-ttu-id="87dba-3678">`C` enthält eine barrierefreie Instanz oder Erweiterungsmethode mit dem Namen `GetAwaiter`, die über keine Argumente verfügt und einen Typ `E` zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3678">`C` contains an accessible instance or extension method named `GetAwaiter` which has no arguments and which returns some type `E`;</span></span>

* <span data-ttu-id="87dba-3679">`E` enthält eine lesbare Instanz oder Erweiterungs Eigenschaft mit dem Namen `IsCompleted`, die keine Argumente annimmt und den Typ "Boolean" aufweist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3679">`E` contains a readable instance or extension property named `IsCompleted` which takes no arguments and has type Boolean;</span></span>

* <span data-ttu-id="87dba-3680">`E` enthält eine barrierefreie Instanz oder Erweiterungsmethode mit dem Namen `GetResult`, die keine Argumente annimmt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3680">`E` contains an accessible instance or extension method named `GetResult` which takes no arguments;</span></span>

* <span data-ttu-id="87dba-3681">`E` implementiert entweder `System.Runtime.CompilerServices.INotifyCompletion` oder `ICriticalNotifyCompletion`.</span><span class="sxs-lookup"><span data-stu-id="87dba-3681">`E` implements either `System.Runtime.CompilerServices.INotifyCompletion` or `ICriticalNotifyCompletion`.</span></span>

<span data-ttu-id="87dba-3682">Wenn `GetResult` ein `Sub` war, wird der Erwartungs Ausdruck als void klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="87dba-3682">If `GetResult` was a `Sub`, then the await expression is classified as void.</span></span> <span data-ttu-id="87dba-3683">Andernfalls wird der Erwartungs Ausdruck als Wert klassifiziert, und sein Typ ist der Rückgabetyp der `GetResult`-Methode.</span><span class="sxs-lookup"><span data-stu-id="87dba-3683">Otherwise, the await expression is classified as a value and its type is the return type of the `GetResult` method.</span></span>

<span data-ttu-id="87dba-3684">Im folgenden finden Sie ein Beispiel für eine Klasse, die gewartet werden kann:</span><span class="sxs-lookup"><span data-stu-id="87dba-3684">Here is an example of a class that can be awaited:</span></span>

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

<span data-ttu-id="87dba-3685">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="87dba-3685">__Note.__</span></span> <span data-ttu-id="87dba-3686">Bibliotheks Autoren sollten das Muster befolgen, dass Sie den Fortsetzungs Delegaten auf dem gleichen `SynchronizationContext` aufrufen, weil ihre `OnCompleted` selbst aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="87dba-3686">Library authors are recommended to follow the pattern that they invoke the continuation delegate on the same `SynchronizationContext` as their `OnCompleted` was itself invoked on.</span></span> <span data-ttu-id="87dba-3687">Außerdem sollte der Wiederaufnahme Delegat nicht synchron innerhalb der `OnCompleted`-Methode ausgeführt werden, da dies zu einem Stapelüberlauf führen kann: Stattdessen sollte der Delegat für die nachfolgende Ausführung in die Warteschlange eingereiht werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3687">Also, the resumption delegate should not be executed synchronously within the `OnCompleted` method since that can lead to stack overflow: instead, the delegate should be queued for subsequent execution.</span></span>

<span data-ttu-id="87dba-3688">Wenn die Ablauf Steuerung einen `Await`-Operator erreicht, sieht das Verhalten wie folgt aus.</span><span class="sxs-lookup"><span data-stu-id="87dba-3688">When control flow reaches an `Await` operator, behavior is as follows.</span></span>

1.  <span data-ttu-id="87dba-3689">Die `GetAwaiter`-Methode des Erwartungs Operanden wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3689">The `GetAwaiter` method of the await operand is invoked.</span></span> <span data-ttu-id="87dba-3690">Das Ergebnis dieses aufnamens wird als *akellner*bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="87dba-3690">The result of this invocation is termed the *awaiter*.</span></span>

2.  <span data-ttu-id="87dba-3691">Die `IsCompleted`-Eigenschaft des akellners wird abgerufen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3691">The awaiter's `IsCompleted` property is retrieved.</span></span> <span data-ttu-id="87dba-3692">Wenn das Ergebnis true ist, gilt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="87dba-3692">If the result is true then:</span></span>

    21. <span data-ttu-id="87dba-3693">Die `GetResult`-Methode des akellners wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="87dba-3693">The `GetResult` method of the awaiter is invoked.</span></span> <span data-ttu-id="87dba-3694">Wenn `GetResult` eine Funktion ist, ist der Wert des Erwartungs Ausdrucks der Rückgabewert dieser Funktion.</span><span class="sxs-lookup"><span data-stu-id="87dba-3694">If `GetResult` was a function, then the value of the await expression is the return value of this function.</span></span>

3.  <span data-ttu-id="87dba-3695">Wenn die isabgeschlossene-Eigenschaft nicht wahr ist, gilt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="87dba-3695">If the IsCompleted property isn't true then:</span></span>

    31. <span data-ttu-id="87dba-3696">Entweder wird entweder `ICriticalNotifyCompletion.UnsafeOnCompleted` auf dem akellner aufgerufen (wenn der Typ des akellons `E` `ICriticalNotifyCompletion`) oder `INotifyCompletion.OnCompleted` implementiert (andernfalls).</span><span class="sxs-lookup"><span data-stu-id="87dba-3696">Either `ICriticalNotifyCompletion.UnsafeOnCompleted` is invoked on the awaiter (if the awaiter's type `E` implements `ICriticalNotifyCompletion`) or `INotifyCompletion.OnCompleted` (otherwise).</span></span> <span data-ttu-id="87dba-3697">In beiden Fällen übergibt Sie einen *Wiederaufnahme* -Delegaten, der der aktuellen Instanz der Async-Methode zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="87dba-3697">In both cases it passes a *resumption delegate* associated with the current instance of the async method.</span></span>

    32. <span data-ttu-id="87dba-3698">Der Kontrollpunkt der aktuellen Async-Methoden Instanz wird angehalten, und die Ablauf Steuerung wird im *aktuellen* Aufrufer (definiert im Abschnitt [Async-Methoden](statements.md#async-methods)) fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="87dba-3698">The control point of the current async method instance is suspended, and control flow resumes in the *current caller* (defined in Section [Async Methods](statements.md#async-methods)).</span></span>

    33. <span data-ttu-id="87dba-3699">Wenn später der Wiederaufnahme Delegat aufgerufen wird,</span><span class="sxs-lookup"><span data-stu-id="87dba-3699">If later the resumption delegate is invoked,</span></span>

        331. <span data-ttu-id="87dba-3700">der Wiederherstellungs Delegat stellt zuerst `System.Threading.Thread.CurrentThread.ExecutionContext` für den Zeitpunkt wieder her, an dem `OnCompleted` aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="87dba-3700">the resumption delegate first restores `System.Threading.Thread.CurrentThread.ExecutionContext` to what it was at the time `OnCompleted` was called,</span></span>
        332. <span data-ttu-id="87dba-3701">Anschließend wird die Ablauf Steuerung am Steuerungspunkt der Async-Methoden Instanz fortgesetzt (siehe Abschnitt [Async-Methoden](statements.md#async-methods)).</span><span class="sxs-lookup"><span data-stu-id="87dba-3701">then it resumes control flow at the control point of the async method instance (see Section [Async Methods](statements.md#async-methods)),</span></span>
        333. <span data-ttu-id="87dba-3702">Dabei wird die `GetResult`-Methode des akellners aufgerufen, wie in 2,1 oben.</span><span class="sxs-lookup"><span data-stu-id="87dba-3702">where it calls the `GetResult` method of the awaiter, as in 2.1 above.</span></span>

<span data-ttu-id="87dba-3703">Wenn der Erwartungs Operand ein Type-Objekt aufweist, wird dieses Verhalten bis zur Laufzeit verzögert:</span><span class="sxs-lookup"><span data-stu-id="87dba-3703">If the await operand has type Object, then this behavior is deferred until runtime:</span></span>

- <span data-ttu-id="87dba-3704">Schritt 1 wird durch Aufrufen von getakellner () ohne Argumente erreicht. Daher kann die Bindung zur Laufzeit an Instanzmethoden erfolgen, die optionale Parameter akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="87dba-3704">Step 1 is accomplished by calling GetAwaiter() with no arguments; it may therefore bind at runtime to instance methods which take optional parameters.</span></span>
- <span data-ttu-id="87dba-3705">Schritt 2 wird durch Abrufen der isabgeschlossene ()-Eigenschaft ohne Argumente und durch das Versuch einer systeminternen Konvertierung in einen booleschen Wert erreicht.</span><span class="sxs-lookup"><span data-stu-id="87dba-3705">Step 2 is accomplished by retrieving the IsCompleted() property with no arguments, and by attempting an intrinsic conversion to Boolean.</span></span>
- <span data-ttu-id="87dba-3706">Schritt 3. a wird durch den Versuch `TryCast(awaiter, ICriticalNotifyCompletion)` erreicht. wenn dies fehlschlägt, `DirectCast(awaiter, INotifyCompletion)`.</span><span class="sxs-lookup"><span data-stu-id="87dba-3706">Step 3.a is accomplished by attempting `TryCast(awaiter, ICriticalNotifyCompletion)`, and if this fails then `DirectCast(awaiter, INotifyCompletion)`.</span></span>

<span data-ttu-id="87dba-3707">Der Wiederaufnahme Delegat wurde in 3 übertragen. ein kann nur einmal aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="87dba-3707">The resumption delegate passed in 3.a may only be invoked once.</span></span> <span data-ttu-id="87dba-3708">Wenn Sie mehrmals aufgerufen wird, ist das Verhalten nicht definiert.</span><span class="sxs-lookup"><span data-stu-id="87dba-3708">If it is invoked more than once, the behavior is undefined.</span></span>

