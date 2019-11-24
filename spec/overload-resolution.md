---
ms.openlocfilehash: a9499e51a67a9b311ae54410c001f8bd22c30d65
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306067"
---
# <a name="overloaded-method-resolution"></a><span data-ttu-id="72f6e-101">Überladene Methoden Auflösung</span><span class="sxs-lookup"><span data-stu-id="72f6e-101">Overloaded Method Resolution</span></span>

<span data-ttu-id="72f6e-102">In der Praxis sind die Regeln zum Bestimmen der Überladungs Auflösung darauf ausgerichtet, die Überladung zu ermitteln, die den tatsächlich angegebenen Argumenten am nächsten liegt.</span><span class="sxs-lookup"><span data-stu-id="72f6e-102">In practice, the rules for determining overload resolution are intended to find the overload that is "closest" to the actual arguments supplied.</span></span> <span data-ttu-id="72f6e-103">Wenn eine Methode vorhanden ist, deren Parametertypen den Argument Typen entsprechen, ist diese Methode offensichtlich die nächstgelegene Methode.</span><span class="sxs-lookup"><span data-stu-id="72f6e-103">If there is a method whose parameter types match the argument types, then that method is obviously the closest.</span></span> <span data-ttu-id="72f6e-104">Wenn Sie dies tun, ist eine Methode näher als eine Methode, wenn alle Parametertypen schmaler oder gleich der Parametertypen der anderen Methode sind.</span><span class="sxs-lookup"><span data-stu-id="72f6e-104">Barring that, one method is closer than another if all of its parameter types are narrower than (or the same as) the parameter types of the other method.</span></span> <span data-ttu-id="72f6e-105">Wenn keine der Methoden Parameter schmaler als die andere ist, gibt es keine Möglichkeit, zu bestimmen, welche Methode näher an den Argumenten liegt.</span><span class="sxs-lookup"><span data-stu-id="72f6e-105">If neither method's parameters are narrower than the other, then there is no way for to determine which method is closer to the arguments.</span></span>

<span data-ttu-id="72f6e-106">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="72f6e-106">__Note.__</span></span> <span data-ttu-id="72f6e-107">Bei der Überladungs Auflösung wird der erwartete Rückgabetyp der Methode nicht berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="72f6e-107">Overload resolution does not take into account the expected return type of the method.</span></span> 

<span data-ttu-id="72f6e-108">Beachten Sie auch, dass die Reihenfolge der tatsächlichen und formalen Parameter aufgrund der Syntax für benannte Parameter möglicherweise nicht identisch ist.</span><span class="sxs-lookup"><span data-stu-id="72f6e-108">Also note that because of the named parameter syntax, the ordering of the actual and formal parameters may not be the same.</span></span>

<span data-ttu-id="72f6e-109">Wenn eine Methoden Gruppe angegeben ist, wird die am meisten anwendbare Methode in der Gruppe für eine Argumentliste anhand der folgenden Schritte bestimmt.</span><span class="sxs-lookup"><span data-stu-id="72f6e-109">Given a method group, the most applicable method in the group for an argument list is determined using the following steps.</span></span> <span data-ttu-id="72f6e-110">Wenn nach dem Anwenden eines bestimmten Schritts keine Mitglieder im Satz verbleiben, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="72f6e-110">If, after applying a particular step, no members remain in the set, then a compile-time error occurs.</span></span> <span data-ttu-id="72f6e-111">Wenn nur ein Member im Satz verbleibt, ist dieses Element der am meisten anwendbare Member.</span><span class="sxs-lookup"><span data-stu-id="72f6e-111">If only one member remains in the set, then that member is the most applicable member.</span></span> <span data-ttu-id="72f6e-112">Die Schritte lauten wie folgt:</span><span class="sxs-lookup"><span data-stu-id="72f6e-112">The steps are:</span></span>

1.  <span data-ttu-id="72f6e-113">Wenn keine Typargumente angegeben wurden, wenden Sie zunächst den Typrückschluss auf alle Methoden an, die über Typparameter verfügen.</span><span class="sxs-lookup"><span data-stu-id="72f6e-113">First, if no type arguments have been supplied, apply type inference to any methods which have type parameters.</span></span> <span data-ttu-id="72f6e-114">Wenn der Typrückschluss für eine Methode erfolgreich ist, werden die abherleitet Typargumente für diese spezielle Methode verwendet.</span><span class="sxs-lookup"><span data-stu-id="72f6e-114">If type inference succeeds for a method, then the inferred type arguments are used for that particular method.</span></span> <span data-ttu-id="72f6e-115">Wenn bei der Typrückschluss für eine Methode ein Fehler auftritt, wird diese Methode aus dem Satz entfernt.</span><span class="sxs-lookup"><span data-stu-id="72f6e-115">If type inference fails for a method, then that method is eliminated from the set.</span></span>

2.  <span data-ttu-id="72f6e-116">Entfernen Sie als nächstes alle Elemente aus dem Satz, auf die nicht zugegriffen werden kann (Abschnitt [Anwendbarkeit auf Argument Liste](overload-resolution.md#applicability-to-argument-list)), und führen Sie die Argumentliste aus.</span><span class="sxs-lookup"><span data-stu-id="72f6e-116">Next, eliminate all members from the set that are inaccessible or not applicable (Section [Applicability To Argument List](overload-resolution.md#applicability-to-argument-list)) to the argument list</span></span>

3.  <span data-ttu-id="72f6e-117">Wenn ein oder mehrere Argumente `AddressOf` oder Lambda-Ausdrücke sind, berechnen Sie dann die *delegatentspannungsebenen* für jedes derartige Argument wie unten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="72f6e-117">Next, if one or more arguments are `AddressOf` or lambda expressions, then calculate the *delegate relaxation levels* for each such argument as below.</span></span> <span data-ttu-id="72f6e-118">Wenn der schlechteste (niedrigste) aufrusegrad in `N` schlimmer ist als der niedrigste Wert für die delegatentspannungsstufe in `M`, entfernen Sie `N` aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="72f6e-118">If the worst (lowest) delegate relaxation level in `N` is worse than the lowest delegate relaxation level in `M`, then eliminate `N` from the set.</span></span> <span data-ttu-id="72f6e-119">Die delegatentspannungsstufen lauten wie folgt:</span><span class="sxs-lookup"><span data-stu-id="72f6e-119">The delegate relaxation levels are as follows:</span></span>

    31. <span data-ttu-id="72f6e-120">*Fehler delegatauffüllungsebene* --, wenn die `AddressOf` oder der Lambda nicht in den Delegattyp konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="72f6e-120">*Error delegate relaxation level* -- if the `AddressOf` or lambda cannot be converted to the delegate type.</span></span>
  
    32. <span data-ttu-id="72f6e-121">Einschränkende *delegatlockerung von Rückgabetyp oder Parametern* : Wenn das Argument `AddressOf` ist, oder wenn ein Lambda mit einem deklarierten Typ und die Konvertierung des Rückgabe Typs in den delegatrückgabetyp einschränkend ist; oder wenn das Argument ein reguläres Lambda ist und die Konvertierung von einem der Rückgabe Ausdrücke in den Delegattyp des Delegaten einschränkend ist, oder wenn das Argument ein Async-Lambda ist und der delegatrückgabetyp `Task(Of T)` ist und die Konvertierung von Rückgabe Ausdrücken in `T` einschränkend ist. oder wenn es sich bei dem Argument um einen iteratorlambda handelt und der Delegattyp `IEnumerator(Of T)` oder `IEnumerable(Of T)`, und die Konvertierung von seinen Yield-Operanden in `T` einzuschränken.</span><span class="sxs-lookup"><span data-stu-id="72f6e-121">*Narrowing delegate relaxation of return type or parameters* -- if the argument is `AddressOf` or a lambda with a declared type and the conversion from its return type to the delegate return type is narrowing; or if the argument is a regular lambda and the conversion from any of its return expressions to the delegate return type is narrowing, or if the argument is an async lambda and the delegate return type is `Task(Of T)` and the conversion from any of its return expressions to `T` is narrowing; or if the argument is an iterator lambda and the delegate return type `IEnumerator(Of T)` or `IEnumerable(Of T)` and the conversion from any of its yield operands to `T` is narrowing.</span></span>

    33. <span data-ttu-id="72f6e-122">*Erweiternde delegatentspannung zu Delegaten ohne Signatur,* wenn der Delegattyp `System.Delegate` oder `System.MultiCastDelegate` oder `System.Object`ist.</span><span class="sxs-lookup"><span data-stu-id="72f6e-122">*Widening delegate relaxation to delegate without signature* -- if delegate type is `System.Delegate` or `System.MultiCastDelegate` or `System.Object`.</span></span>

    34. <span data-ttu-id="72f6e-123">Verwerfen von *Drop Return oder Arguments* Delegaten--wenn das Argument `AddressOf` oder ein Lambda mit einem deklarierten Rückgabetyp und der Delegattyp keinen Rückgabetyp hat. oder wenn es sich bei dem Argument um einen Lambda-Wert mit mindestens einem Rückgabe Ausdruck handelt und der Delegattyp keinen Rückgabetyp hat. oder, wenn das Argument `AddressOf` oder Lambda ohne Parameter ist und der Delegattyp über Parameter verfügt.</span><span class="sxs-lookup"><span data-stu-id="72f6e-123">*Drop return or arguments delegate relaxation* -- if the argument is `AddressOf` or a lambda with a declared return type and the delegate type lacks a return type; or if the argument is a lambda with one or more return expressions and the delegate type lacks a return type; or if the argument is `AddressOf` or lambda with no parameters and the delegate type has parameters.</span></span>

    35. <span data-ttu-id="72f6e-124">*Erweiternde delegatlockerung des Rückgabe Typs* , wenn das Argument `AddressOf` oder ein Lambda mit einem deklarierten Rückgabetyp ist und eine erweiternde Konvertierung von seinem Rückgabetyp in den des Delegaten erfolgt; oder wenn es sich bei dem Argument um einen regulären Lambda-Ausdruck handelt, bei dem die Konvertierung von allen Rückgabe Ausdrücken in den Delegattyp des Delegaten eine Erweiterung oder Identität mit mindestens einer Erweiterung ist oder wenn das Argument ein Async-Lambda ist und der Delegat `Task(Of T)` oder `Task` ist, und die Konvertierung von allen Rückgabe Ausdrücken in `T`/`Object` bzw. die Identität mit mindestens einer Erweiterung. oder wenn es sich bei dem Argument um einen iteratorlambda handelt und der Delegat `IEnumerator(Of T)` oder `IEnumerable(Of T)` oder `IEnumerator` oder `IEnumerable`, und die Konvertierung von allen Rückgabe Ausdrücken in `T`/`Object` der Erweiterung oder Identität mit mindestens einer Erweiterung ist.</span><span class="sxs-lookup"><span data-stu-id="72f6e-124">*Widening delegate relaxation of return type* -- if the argument is `AddressOf` or a lambda with a declared return type, and there is a widening conversion from its return type to that of the delegate; or if the argument is a regular lambda where the conversion from all return expressions to the delegate return type is widening or identity with at least one widening; or if the argument is an async lambda and the delegate is `Task(Of T)` or `Task` and the conversion from all return expressions to `T`/`Object` respectively is widening or identity with at least one widening; or if the argument is an iterator lambda and the delegate is `IEnumerator(Of T)` or `IEnumerable(Of T)` or `IEnumerator` or `IEnumerable` and the conversion from all return expressions to `T`/`Object` is widening or identity with at least one widening.</span></span>

    36. <span data-ttu-id="72f6e-125">*Lockerung des Identitäts* Delegaten: Wenn es sich bei dem Argument um eine `AddressOf` oder einen Lambda handelt, die exakt mit dem Delegaten übereinstimmt, ohne die Werte zu erweitern, einzuschränken oder zu verwerfen oder Ergebnisse zurückgibt. Wenn für einige Member der Gruppe keine einschränkenden Konvertierungen erforderlich sind, damit Sie auf die Argumente angewendet werden können, entfernen Sie dann alle Member, die dies tun.</span><span class="sxs-lookup"><span data-stu-id="72f6e-125">*Identity delegate relaxation* -- if the argument is an `AddressOf` or a lambda which matches the delegate exactly, with no widening or narrowing or dropping of parameters or returns or yields.Next, if some members of the set do not requiring narrowing conversions to be applicable to any of the arguments, then eliminate all members that do.</span></span> <span data-ttu-id="72f6e-126">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="72f6e-126">For example:</span></span>

    ```vb
    Sub f(x As Object)
    End Sub

    Sub f(x As Short)
    End Sub

    Sub f(x As Short())
    End Sub

    f("5") ' picks the Object overload, since String->Short is narrowing
    f(5)   ' picks the Object overload, since Integer->Short is narrowing
    f({5}) ' picks the Object overload, since Integer->Short is narrowing
    f({})  ' a tie-breaker rule subsequent to [3] picks the Short() overload

    ```

4.  <span data-ttu-id="72f6e-127">Als nächstes erfolgt die Löschung auf der Grundlage der Einschränkung wie folgt.</span><span class="sxs-lookup"><span data-stu-id="72f6e-127">Next, elimination is done based on narrowing as follows.</span></span> <span data-ttu-id="72f6e-128">(Beachten Sie, dass, wenn Option Strict aktiviert ist, alle Member, die einschränkende Elemente erfordern, bereits inanwendbar (Abschnitt [Anwendbarkeit auf Argument Liste](overload-resolution.md#applicability-to-argument-list)) und durch Schritt 2 entfernt wurden.)</span><span class="sxs-lookup"><span data-stu-id="72f6e-128">(Note that, if Option Strict is On, then all members that require narrowing have already been judged inapplicable (Section [Applicability To Argument List](overload-resolution.md#applicability-to-argument-list)) and removed by Step 2.)</span></span>

    41. <span data-ttu-id="72f6e-129">Wenn für einige Instanzmember der Gruppe nur einschränkende Konvertierungen erforderlich sind, bei denen der Argumenttyp `Object`ist, dann entfernen Sie alle anderen Elemente.</span><span class="sxs-lookup"><span data-stu-id="72f6e-129">If some instance members of the set only require narrowing conversions where the argument expression type is `Object`, then eliminate all other members.</span></span>
    42. <span data-ttu-id="72f6e-130">Wenn die Gruppe mehr als ein Element enthält, das nur von `Object`beschränkt werden muss, wird der Aufruf Ziel Ausdruck als spät gebundener Methoden Zugriff neu klassifiziert (und es wird ein Fehler ausgegeben, wenn der Typ, der die Methoden Gruppe enthält, eine Schnittstelle ist oder wenn eines der anwendbaren Member Erweiterungs Mitglieder war).</span><span class="sxs-lookup"><span data-stu-id="72f6e-130">If the set contains more than one member which requires narrowing only from `Object`, then the invocation target expression is reclassified as a late-bound method access (and an error is given if the type containing the method group is an interface, or if any of the applicable members were extension members).</span></span>
    43. <span data-ttu-id="72f6e-131">Wenn es Kandidaten gibt, die nur eine Einschränkung von numerischen Literalen erfordern, wählen Sie anhand der folgenden Schritte die spezifischsten unter allen verbleibenden Kandidaten aus.</span><span class="sxs-lookup"><span data-stu-id="72f6e-131">If there are any candidates that only require narrowing from numeric literals, then chose the most specific among all remaining candidates by the steps below.</span></span> <span data-ttu-id="72f6e-132">Wenn der Gewinner nur die Einschränkung von numerischen Literalen erfordert, wird er als Ergebnis der Überladungs Auflösung ausgewählt. andernfalls handelt es sich um einen Fehler.</span><span class="sxs-lookup"><span data-stu-id="72f6e-132">If the winner requires only narrowing from numeric literals, then it is picked as the result of overload resolution; otherwise it is an error.</span></span>

    <span data-ttu-id="72f6e-133">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="72f6e-133">__Note.__</span></span> <span data-ttu-id="72f6e-134">Die Begründung für diese Regel ist, dass ein Programm, bei dem es sich um eine lose Typisierung handelt (d. h., die meisten oder alle Variablen als `Object`deklariert sind), schwierig sein kann, wenn viele Konvertierungen von `Object` einschränkend sind.</span><span class="sxs-lookup"><span data-stu-id="72f6e-134">The justification for this rule is that if a program is loosely-typed (that is, most or all variables are declared as `Object`), overload resolution can be difficult when many conversions from `Object` are narrowing.</span></span> <span data-ttu-id="72f6e-135">Anstatt dass die Überladungs Auflösung in vielen Situationen fehlschlägt (erfordert die starke Typisierung der Argumente für den Methodenaufruf), wird die Auflösung der entsprechenden überladenen Methode bis zur Laufzeit verzögert.</span><span class="sxs-lookup"><span data-stu-id="72f6e-135">Rather than have the overload resolution fail in many situations (requiring strong typing of the arguments to the method call), resolution the appropriate overloaded method to call is deferred until run time.</span></span> <span data-ttu-id="72f6e-136">Dadurch kann der lose typisierte-Befehl ohne zusätzliche Umwandlungen erfolgreich ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="72f6e-136">This allows the loosely-typed call to succeed without additional casts.</span></span> <span data-ttu-id="72f6e-137">Ein unglücklicher Nebeneffekt dieses Werts ist jedoch, dass das Durchführen des später gebundenen Aufrufes erfordert, dass das CallTarget in `Object`umgewandelt wird.</span><span class="sxs-lookup"><span data-stu-id="72f6e-137">An unfortunate side-effect of this, however, is that performing the late-bound call requires casting the call target to `Object`.</span></span> <span data-ttu-id="72f6e-138">Bei einem Struktur Wert bedeutet dies, dass der Wert in einen temporären konvertiert werden muss.</span><span class="sxs-lookup"><span data-stu-id="72f6e-138">In the case of a structure value, this means that the value must be boxed to a temporary.</span></span> <span data-ttu-id="72f6e-139">Wenn die Methode, die schließlich aufgerufen wird, versucht, ein Feld der Struktur zu ändern, geht diese Änderung verloren, sobald die Methode zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="72f6e-139">If the method eventually called tries to change a field of the structure, this change will be lost once the method returns.</span></span> <span data-ttu-id="72f6e-140">Schnittstellen werden von dieser speziellen Regel ausgeschlossen, da die späte Bindung immer für die Member der Lauf Zeit Klasse oder des Struktur Typs aufgelöst wird, die möglicherweise andere Namen als die Member der Schnittstellen, die Sie implementieren.</span><span class="sxs-lookup"><span data-stu-id="72f6e-140">Interfaces are excluded from this special rule because late binding always resolves against the members of the runtime class or structure type, which may have different names than the members of the interfaces they implement.</span></span>

5.  <span data-ttu-id="72f6e-141">Wenn als nächstes Instanzmethoden in der Menge verbleiben, die keine Einschränkung erfordern, entfernen Sie alle Erweiterungs Methoden aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="72f6e-141">Next, if any instance methods remain in the set which do not require narrowing, then eliminate all extension methods from the set.</span></span> <span data-ttu-id="72f6e-142">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="72f6e-142">For example:</span></span>

    ```vb
    Imports System.Runtime.CompilerServices

    Class C3
        Sub M1(d As Integer)
        End Sub
    End Class

    Module C3Extensions
        <Extension> _
        Sub M1(c3 As C3, c As Long)
        End Sub

        <Extension> _
        Sub M1(c3 As C3, c As Short)
        End Sub
    End Module

    Module Test
        Sub Main()
            Dim c As New C3()
            Dim sVal As Short = 10
            Dim lVal As Long = 20

            ' Calls C3.M1, since C3.M1 is applicable.
            c.M1(sVal)

            ' Calls C3Extensions.M1 since C3.M1 requires a narrowing conversion
            c.M1(lVal)
        End Sub
    End Module
    ```

    <span data-ttu-id="72f6e-143">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="72f6e-143">__Note.__</span></span> <span data-ttu-id="72f6e-144">Erweiterungs Methoden werden ignoriert, wenn anwendbare Instanzmethoden vorhanden sind, um zu gewährleisten, dass das Hinzufügen eines Imports (der möglicherweise neue Erweiterungs Methoden in den Gültigkeitsbereich bringt) keinen-Rückruf für eine vorhandene Instanzmethode zum erneuten Binden an eine Erweiterungsmethode auslöst.</span><span class="sxs-lookup"><span data-stu-id="72f6e-144">Extension methods are ignored if there are applicable instance methods to guarantee that adding an import (that might bring new extension methods into scope) will not cause a call on an existing instance method to rebind to an extension method.</span></span> <span data-ttu-id="72f6e-145">Aufgrund des umfassenden Umfangs einiger Erweiterungs Methoden (d. h. der für Schnittstellen und/oder Typparameter definierten) ist dies ein sicherer Ansatz für die Bindung an Erweiterungs Methoden.</span><span class="sxs-lookup"><span data-stu-id="72f6e-145">Given the broad scope of some extension methods (i.e. those defined on interfaces and/or type parameters), this is a safer approach to binding to extension methods.</span></span>

6.  <span data-ttu-id="72f6e-146">Wenn als nächstes zwei Member der Set-`M` und `N`, ist `M` *spezifischer* (Abschnitts [Spezifizität von Membern/Typen, wenn eine Argumentliste angegeben](overload-resolution.md#specificity-of-memberstypes-given-an-argument-list)ist) als `N` der Argumentliste, entfernen Sie `N` aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="72f6e-146">Next, if, given any two members of the set `M` and `N`, `M` is more *specific* (Section [Specificity of members/types given an argument list](overload-resolution.md#specificity-of-memberstypes-given-an-argument-list)) than `N` given the argument list, eliminate `N` from the set.</span></span> <span data-ttu-id="72f6e-147">Wenn mehr als ein Member in der Menge verbleiben und die übrigen Member nicht gleichermaßen spezifisch sind, wenn die Argumentliste angegeben ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="72f6e-147">If more than one member remains in the set and the remaining members are not equally specific given the argument list, a compile-time error results.</span></span>

7.  <span data-ttu-id="72f6e-148">Wenden Sie andernfalls in der angegebenen Reihenfolge die folgenden Regeln für die Trennung von Regeln an, wenn Sie zwei Member der Gruppe, `M` und `N`haben:</span><span class="sxs-lookup"><span data-stu-id="72f6e-148">Otherwise, given any two members of the set, `M` and `N`, apply the following tie-breaking rules, in order:</span></span>

    71. <span data-ttu-id="72f6e-149">Wenn `M` über keinen ParamArray-Parameter verfügt, aber `N` bewirkt, oder wenn beide Aktionen, aber `M` weniger Argumente an den ParamArray-Parameter übergeben als `N`, dann entfernen Sie `N` aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="72f6e-149">If `M` does not have a ParamArray parameter but `N` does, or if both do but `M` passes fewer arguments into the ParamArray parameter than `N` does, then eliminate `N` from the set.</span></span> <span data-ttu-id="72f6e-150">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="72f6e-150">For example:</span></span>

        ```vb
        Module Test
            Sub F(a As Object, ParamArray b As Object())
                Console.WriteLine("F(Object, Object())")
            End Sub

            Sub F(a As Object, b As Object, ParamArray c As Object())
                Console.WriteLine("F(Object, Object, Object())")
            End Sub

           Sub G(Optional a As Object = Nothing)
              Console.WriteLine("G(Object)")
           End Sub

           Sub G(ParamArray a As Object())
              Console.WriteLine("G(Object())")
           End Sub    Sub Main()
                F(1)
                F(1, 2)
                F(1, 2, 3)
              G()
            End Sub
        End Module
        ```

        <span data-ttu-id="72f6e-151">Im obigen Beispiel wird die folgende Ausgabe erzeugt:</span><span class="sxs-lookup"><span data-stu-id="72f6e-151">The above example produces the following output:</span></span>

        ```console
        F(Object, Object())
        F(Object, Object, Object())
        F(Object, Object, Object())
        G(Object)
        ```

        <span data-ttu-id="72f6e-152">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="72f6e-152">__Note.__</span></span> <span data-ttu-id="72f6e-153">Wenn eine Klasse eine Methode mit einem ParamArray-Parameter deklariert, ist es nicht ungewöhnlich, dass Sie auch einige der erweiterten Formulare als reguläre Methoden einschließen.</span><span class="sxs-lookup"><span data-stu-id="72f6e-153">When a class declares a method with a paramarray parameter, it is not uncommon to also include some of the expanded forms as regular methods.</span></span> <span data-ttu-id="72f6e-154">Auf diese Weise ist es möglich, die Zuordnung einer Array Instanz zu vermeiden, die auftritt, wenn eine erweiterte Form einer Methode mit einem ParamArray-Parameter aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="72f6e-154">By doing so it is possible to avoid the allocation of an array instance that occurs when an expanded form of a method with a paramarray parameter is invoked.</span></span>

    72. <span data-ttu-id="72f6e-155">Wenn `M` in einem stärker abgeleiteten Typ als `N`definiert ist, entfernen Sie `N` aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="72f6e-155">If `M` is defined in a more derived type than `N`, eliminate `N` from the set.</span></span> <span data-ttu-id="72f6e-156">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="72f6e-156">For example:</span></span>

        ```vb
        Class Base
            Sub F(Of T, U)(x As T, y As U)
            End Sub
        End Class

        Class Derived
            Inherits Base

            Overloads Sub F(Of T, U)(x As U, y As T)
            End Sub
        End Class

        Module Test
            Sub Main()
                Dim d As New Derived()

                ' Calls Derived.F
                d.F(10, 10)
            End Sub
        End Module
        ```

        <span data-ttu-id="72f6e-157">Diese Regel gilt auch für die Typen, für die Erweiterungs Methoden definiert sind.</span><span class="sxs-lookup"><span data-stu-id="72f6e-157">This rule also applies to the types that extension methods are defined on.</span></span> <span data-ttu-id="72f6e-158">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="72f6e-158">For example:</span></span>

        ```vb
        Imports System.Runtime.CompilerServices

        Class Base
        End Class

        Class Derived
            Inherits Base
        End Class

        Module BaseExt
            <Extension> _
            Sub M(b As Base, x As Integer)
            End Sub
        End Module

        Module DerivedExt
            <Extension> _
            Sub M(d As Derived, x As Integer)
            End Sub
        End Module

        Module Test
            Sub Main()
                Dim b As New Base()
                Dim d As New Derived()

                ' Calls BaseExt.M
                b.M(10)

                ' Calls DerivedExt.M 
                d.M(10)
            End Sub
        End Module
        ```

    73. <span data-ttu-id="72f6e-159">Wenn `M` und `N` Erweiterungs Methoden sind und der Zieltyp von `M` eine Klasse oder Struktur ist und der Zieltyp `N` eine Schnittstelle ist, entfernen Sie `N` aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="72f6e-159">If `M` and `N` are extension methods and the target type of `M` is a class or structure and the target type of `N` is an interface, eliminate `N` from the set.</span></span> <span data-ttu-id="72f6e-160">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="72f6e-160">For example:</span></span>

        ```vb
        Imports System.Runtime.CompilerServices

        Interface I1
        End Interface

        Class C1
            Implements I1
        End Class

        Module Ext1
            <Extension> _
            Sub M(i As I1, x As Integer)
            End Sub
        End Module

        Module Ext2
            <Extension> _
            Sub M(c As C1, y As Integer)
            End Sub
        End Module

        Module Test
            Sub Main()
                Dim c As New C1()

                ' Calls Ext2.M, because Ext1.M is hidden since it extends
                ' an interface.
                c.M(10)

                ' Calls Ext1.M
                CType(c, I1).M(10)
            End Sub
        End Module
        ```

    74. <span data-ttu-id="72f6e-161">Wenn `M` und `N` Erweiterungs Methoden sind und der Zieltyp von `M` und `N` nach der Typparameter Ersetzung identisch sind, und der Zieltyp `M` vor Typparameter Ersetzung keine Typparameter enthält, aber der Zieltyp `N` hat, dann hat weniger Typparameter als der Zieltyp `N`.`N`</span><span class="sxs-lookup"><span data-stu-id="72f6e-161">If `M` and `N` are extension methods, and the target type of `M` and `N` are identical after type parameter substitution, and the target type of `M` before type parameter substitution does not contain type parameters but the target type of `N` does, then has fewer type parameters than the target type of `N`, eliminate `N` from the set.</span></span> <span data-ttu-id="72f6e-162">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="72f6e-162">For example:</span></span>

        ```vb
        Imports System.Runtime.CompilerServices

        Module Module1
            Sub Main()
                Dim x As Integer = 1
                x.f(1) ' Calls first "f" extension method

                Dim y As New Dictionary(Of Integer, Integer)
                y.g(1) ' Ambiguity error
            End Sub

            <Extension()> Sub f(x As Integer, z As Integer)
            End Sub

            <Extension()> Sub f(Of T)(x As T, z As T)
            End Sub

            <Extension()> Sub g(Of T)(y As Dictionary(Of T, Integer), z As T)
            End Sub

            <Extension()> Sub g(Of T)(y As Dictionary(Of T, T), z As T)
            End Sub
        End Module
        ```

    75. <span data-ttu-id="72f6e-163">Entfernen Sie vor dem Ersetzen von Typargumenten, wenn `M` *weniger generisch* (Abschnitts [Generizität](overload-resolution.md#genericity)) als `N`ist, `N` aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="72f6e-163">Before type arguments have been substituted, if `M` is *less generic* (Section [Genericity](overload-resolution.md#genericity)) than `N`, eliminate `N` from the set.</span></span>

    76. <span data-ttu-id="72f6e-164">Wenn `M` keine Erweiterungsmethode ist und `N` ist, entfernen Sie `N` aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="72f6e-164">If `M` is not an extension method and `N` is, eliminate `N` from the set.</span></span>

    77. <span data-ttu-id="72f6e-165">Wenn `M` und `N` Erweiterungs Methoden sind und `M` vor `N` (Abschnitts [Erweiterungs Methoden](expressions.md#extension-method-collection)Auflistung) gefunden wurde, entfernen Sie `N` aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="72f6e-165">If `M` and `N` are extension methods and `M` was found before `N` (Section [Extension Method Collection](expressions.md#extension-method-collection)), eliminate `N` from the set.</span></span> <span data-ttu-id="72f6e-166">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="72f6e-166">For example:</span></span>

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
                Sub M1(c As C1, y As Integer)
                End Sub
            End Module
        End Namespace

        Namespace N1.N2.N3
            Module Test
                Sub Main()
                    Dim x As New C1()

                    ' Calls N2C1Extensions.M1
                    x.M1(10)
                End Sub
            End Module
        End Namespace
        ```

        <span data-ttu-id="72f6e-167">Wenn die Erweiterungs Methoden im gleichen Schritt gefunden wurden, sind diese Erweiterungs Methoden mehrdeutig.</span><span class="sxs-lookup"><span data-stu-id="72f6e-167">If the extension methods were found in the same step, then those extension methods are ambiguous.</span></span> <span data-ttu-id="72f6e-168">Der Aufruf kann immer mit dem Namen des Standardmoduls, das die Erweiterungsmethode enthält, eindeutig sein und die Erweiterungsmethode aufrufen, als ob es sich um einen regulären Member handelt.</span><span class="sxs-lookup"><span data-stu-id="72f6e-168">The call may always be disambiguated using the name of the standard module containing the extension method and calling the extension method as if it was a regular member.</span></span> <span data-ttu-id="72f6e-169">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="72f6e-169">For example:</span></span>

        ```vb
        Imports System.Runtime.CompilerServices

        Class C1
        End Class

        Module C1ExtA
            <Extension> _
            Sub M(c As C1)
            End Sub
        End Module

        Module C1ExtB
            <Extension> _
            Sub M(c As C1)
            End Sub
        End Module

        Module Main
            Sub Test()
                Dim c As New C1()

                C1.M()               ' Ambiguous between C1ExtA.M and BExtB.M
                C1ExtA.M(c)          ' Calls C1ExtA.M
                C1ExtB.M(c)          ' Calls C1ExtB.M
            End Sub
        End Module
        ```

    78. <span data-ttu-id="72f6e-170">Wenn `M` und `N` beiden erforderlichen Typrückschluss, um Typargumente zu liefern, und `M` nicht erfordert, dass der bestimmende Typ für die Typargumente bestimmt wird (d.h. jedes der Typargumente, die zu einem einzelnen Typ abgeleitet wurden), `N` aber die `N` aus dem Satz auszuschließen.</span><span class="sxs-lookup"><span data-stu-id="72f6e-170">If `M` and `N` both required type inference to produce type arguments, and `M` did not require determining the dominant type for any of its type arguments (i.e. each the type arguments inferred to a single type), but `N` did, eliminate `N` from the set.</span></span>

        <span data-ttu-id="72f6e-171">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="72f6e-171">__Note.__</span></span> <span data-ttu-id="72f6e-172">Diese Regel stellt sicher, dass die Überladungs Auflösung, die in früheren Versionen erfolgreich war (bei der das Ableiten mehrerer Typen für ein Typargument zu einem Fehler führen würde), weiterhin die gleichen Ergebnisse erzeugt.</span><span class="sxs-lookup"><span data-stu-id="72f6e-172">This rule ensures that overload resolution that succeeded in previous versions (where inferring multiple types for a type argument would cause an error), continue to produce the same results.</span></span>

    79. <span data-ttu-id="72f6e-173">Wenn die Überladungs Auflösung durchgeführt wird, um das Ziel eines Ausdrucks der Delegaterstellung aus einem `AddressOf` Ausdruck aufzulösen, und sowohl der Delegat als auch `M` Funktionen sind, während `N` eine Unterroutine ist, `N` aus dem Satz auszuschließen.</span><span class="sxs-lookup"><span data-stu-id="72f6e-173">If overload resolution is being done to resolve the target of a delegate-creation expression from an `AddressOf` expression, and both the delegate and `M` are functions while `N` is a subroutine, eliminate `N` from the set.</span></span> <span data-ttu-id="72f6e-174">Ebenso gilt, wenn sowohl der Delegat als auch der `M` Unterroutinen sind, während `N` eine Funktion ist, die `N` aus dem Satz auszuschließen.</span><span class="sxs-lookup"><span data-stu-id="72f6e-174">Likewise, if both the delegate and `M` are subroutines, while `N` is a function, eliminate `N` from the set.</span></span>

    710. <span data-ttu-id="72f6e-175">Wenn `M` keine optionalen Parameter Standardwerte anstelle von expliziten Argumenten verwendet hat, aber `N` dies getan hat, entfernen Sie `N` aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="72f6e-175">If `M` did not use any optional parameter defaults in place of explicit arguments, but `N` did, then eliminate `N` from the set.</span></span>

    711. <span data-ttu-id="72f6e-176">Wenn `M` vor dem Ersetzen von Typargumenten *eine größere Tiefe von Generika* (Abschnitts [Generizität](overload-resolution.md#genericity)) als `N`aufweist, entfernen Sie `N` aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="72f6e-176">Before type arguments have been substituted, if `M` has *greater depth of genericity* (Section [Genericity](overload-resolution.md#genericity)) than `N`, then eliminate `N` from the set.</span></span>

8. <span data-ttu-id="72f6e-177">Andernfalls ist der Aufruf mehrdeutig, und ein Kompilierzeitfehler tritt auf.</span><span class="sxs-lookup"><span data-stu-id="72f6e-177">Otherwise, the call is ambiguous and a compile-time error occurs.</span></span>

#### <a name="specificity-of-memberstypes-given-an-argument-list"></a><span data-ttu-id="72f6e-178">Spezifizität von Membern/Typen mit einer Argumentliste</span><span class="sxs-lookup"><span data-stu-id="72f6e-178">Specificity of members/types given an argument list</span></span>

<span data-ttu-id="72f6e-179">Ein Element `M` wird bei Angabe eines Arguments-List-`A`*gleichermaßen als gleich* `N`betrachtet, wenn die Signaturen identisch sind oder jeder Parametertyp in `M` mit dem entsprechenden Parametertyp in `N`identisch ist.</span><span class="sxs-lookup"><span data-stu-id="72f6e-179">A member `M` is considered *equally specific* as `N`, given an argument-list `A`, if their signatures are the same or if each parameter type in `M` is the same as the corresponding parameter type in `N`.</span></span>

<span data-ttu-id="72f6e-180">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="72f6e-180">__Note.__</span></span> <span data-ttu-id="72f6e-181">Zwei Member können aufgrund von Erweiterungs Methoden in einer Methoden Gruppe mit der gleichen Signatur enden.</span><span class="sxs-lookup"><span data-stu-id="72f6e-181">Two members can end up in a method group with the same signature due to extension methods.</span></span> <span data-ttu-id="72f6e-182">Zwei Member können auch gleich spezifisch sein, haben aber aufgrund von Typparametern oder ParamArray-Erweiterungen nicht die gleiche Signatur.</span><span class="sxs-lookup"><span data-stu-id="72f6e-182">Two members can also be equally specific but not have the same signature due to type parameters or paramarray expansion.</span></span>

<span data-ttu-id="72f6e-183">Ein Element `M` gilt als *spezifischere* als `N`, wenn die Signaturen unterschiedlich sind und mindestens ein Parametertyp in `M` spezifischer als ein Parametertyp in `N`ist und kein Parametertyp in `N` spezifischer als ein Parametertyp in `M`ist.</span><span class="sxs-lookup"><span data-stu-id="72f6e-183">A member `M` is considered *more specific* than `N` if their signatures are different and at least one parameter type in `M` is more specific than a parameter type in `N`, and no parameter type in `N` is more specific than a parameter type in `M`.</span></span> <span data-ttu-id="72f6e-184">Wenn ein paar von Parametern `Mj` und `Nj`, das mit einem Argument `Aj`übereinstimmt, wird der Typ der `Mj` als *spezifischere* angesehen als der Typ `Nj`, wenn eine der folgenden Bedingungen zutrifft:</span><span class="sxs-lookup"><span data-stu-id="72f6e-184">Given a pair of parameters `Mj` and `Nj` that matches an argument `Aj`, the type of `Mj` is considered *more specific* than the type of `Nj` if one of the following conditions is true:</span></span>

* <span data-ttu-id="72f6e-185">Es gibt eine erweiternde Konvertierung vom Typ `Mj` in den Typ `Nj`.</span><span class="sxs-lookup"><span data-stu-id="72f6e-185">There exists a widening conversion from the type of `Mj` to the type `Nj`.</span></span> <span data-ttu-id="72f6e-186">(__Hinweis:__</span><span class="sxs-lookup"><span data-stu-id="72f6e-186">(__Note.__</span></span> <span data-ttu-id="72f6e-187">Da Parametertypen in diesem Fall ohne Berücksichtigung des eigentlichen Arguments verglichen werden, wird die erweiternde Konvertierung von konstanten Ausdrücken in einen numerischen Typ, in den der Wert passt, in diesem Fall nicht berücksichtigt.)</span><span class="sxs-lookup"><span data-stu-id="72f6e-187">Because parameters types are being compared without regard to the actual argument in this case, the widening conversion from constant expressions to a numeric type the value fits into is not considered in this case.)</span></span>

* <span data-ttu-id="72f6e-188">`Aj` ist die Literale `0`, `Mj` ist ein numerischer Typ, und `Nj` ist ein enumerierter Typ.</span><span class="sxs-lookup"><span data-stu-id="72f6e-188">`Aj` is the literal `0`, `Mj` is a numeric type and `Nj` is an enumerated type.</span></span> <span data-ttu-id="72f6e-189">(__Hinweis:__</span><span class="sxs-lookup"><span data-stu-id="72f6e-189">(__Note.__</span></span> <span data-ttu-id="72f6e-190">Diese Regel ist erforderlich, da die Literale `0` zu einem beliebigen enumerierten Typ umwächst.</span><span class="sxs-lookup"><span data-stu-id="72f6e-190">This rule is necessary because the literal `0` widens to any enumerated type.</span></span> <span data-ttu-id="72f6e-191">Da sich ein Enumerationstyp auf den zugrunde liegenden Typ ausdehnt, bedeutet dies, dass bei der Überladungs Auflösung auf `0` standardmäßig Enumerationstypen für numerische Typen bevorzugt werden.</span><span class="sxs-lookup"><span data-stu-id="72f6e-191">Since an enumerated type widens to its underlying type, this means that overload resolution on `0` will, by default, prefer enumerated types over numeric types.</span></span> <span data-ttu-id="72f6e-192">Wir haben viel Feedback erhalten, dass dieses Verhalten kontraintuitiv war.)</span><span class="sxs-lookup"><span data-stu-id="72f6e-192">We received a lot of feedback that this behavior was counterintuitive.)</span></span>

* <span data-ttu-id="72f6e-193">`Mj` und `Nj` sind numerische Typen, und `Mj` ist älter als `Nj` in der Liste `Byte`, `SByte`, `Short`, `UShort`, `Integer`, `UInteger`, `Long`, `ULong`, `Decimal`, `Single`, `Double`</span><span class="sxs-lookup"><span data-stu-id="72f6e-193">`Mj` and `Nj` are both numeric types, and `Mj` comes earlier than `Nj` in the list `Byte`, `SByte`, `Short`, `UShort`, `Integer`, `UInteger`, `Long`, `ULong`, `Decimal`, `Single`, `Double`.</span></span> <span data-ttu-id="72f6e-194">(__Hinweis:__</span><span class="sxs-lookup"><span data-stu-id="72f6e-194">(__Note.__</span></span> <span data-ttu-id="72f6e-195">Die Regel zu den numerischen Typen ist nützlich, da die numerischen Typen mit und ohne Vorzeichen einer bestimmten Größe nur einschränkende Konvertierungen zwischen diesen Typen aufweisen.</span><span class="sxs-lookup"><span data-stu-id="72f6e-195">The rule about the numeric types is useful because the signed and unsigned numeric types of a particular size only have narrowing conversions between them.</span></span> <span data-ttu-id="72f6e-196">Die obige Regel unterbricht die Verknüpfung zwischen den beiden Typen zugunsten des "natürlichen" numerischen Typs.</span><span class="sxs-lookup"><span data-stu-id="72f6e-196">The above rule breaks the tie between the two types in favor of the more "natural" numeric type.</span></span> <span data-ttu-id="72f6e-197">Dies ist besonders wichtig bei der Überladungs Auflösung für einen Typ, der zu den numerischen Typen mit Vorzeichen und ohne Vorzeichen einer bestimmten Größe erweitert wird, z. b. einem numerischen Literalwert, der in beides passt.)</span><span class="sxs-lookup"><span data-stu-id="72f6e-197">This is particularly important when doing overload resolution on a type that widens to both the signed and unsigned numeric types of a particular size, for example, a numeric literal that fits into both.)</span></span>

* <span data-ttu-id="72f6e-198">`Mj` und `Nj` sind delegatfunktionstypen, und der Rückgabetyp von `Mj` ist spezifischer als der Rückgabetyp `Nj` wenn `Aj` als Lambda-Methode klassifiziert wird und `Mj` oder `Nj` `System.Linq.Expressions.Expression(Of T)`ist, wird das Typargument des Typs (vorausgesetzt, dass es sich um einen Delegattyp handelt) für den verglichenen Typ ersetzt.</span><span class="sxs-lookup"><span data-stu-id="72f6e-198">`Mj` and `Nj` are delegate function types and the return type of `Mj` is more specific than the return type of `Nj` If `Aj` is classified as a lambda method, and `Mj` or `Nj` is `System.Linq.Expressions.Expression(Of T)`, then the type argument of the type (assuming it is a delegate type) is substituted for the type being compared.</span></span>

* <span data-ttu-id="72f6e-199">`Mj` ist identisch mit dem Typ von `Aj`, und `Nj` ist nicht.</span><span class="sxs-lookup"><span data-stu-id="72f6e-199">`Mj` is identical to the type of `Aj`, and `Nj` is not.</span></span> <span data-ttu-id="72f6e-200">(__Hinweis:__</span><span class="sxs-lookup"><span data-stu-id="72f6e-200">(__Note.__</span></span> <span data-ttu-id="72f6e-201">Es ist interessant zu beachten, dass sich die vorherige Regel gering C#fügig von unter C# scheidet, in der erfordert, dass die delegatfunktionstypen identische Parameterlisten aufweisen, bevor Rückgabe Typen verglichen werden, Visual Basic jedoch nicht.)</span><span class="sxs-lookup"><span data-stu-id="72f6e-201">It is interesting to note that the previous rule differs slightly from C#, in that C# requires that the delegate function types have identical parameter lists before comparing return types, while Visual Basic does not.)</span></span>

#### <a name="genericity"></a><span data-ttu-id="72f6e-202">Generika</span><span class="sxs-lookup"><span data-stu-id="72f6e-202">Genericity</span></span>

<span data-ttu-id="72f6e-203">Ein Element `M` wird so festgelegt, dass es *weniger generisch* ist als ein Element `N` wie folgt:</span><span class="sxs-lookup"><span data-stu-id="72f6e-203">A member `M` is determined to be *less generic* than a member `N` as follows:</span></span>

1. <span data-ttu-id="72f6e-204">Wenn für jedes Paar von übereinstimmenden Parametern `Mj` und `Nj`ist `Mj` kleiner oder gleich generisch als `Nj` in Bezug auf Typparameter in der Methode, und mindestens eine `Mj` ist in Bezug auf Typparameter in der Methode weniger generisch.</span><span class="sxs-lookup"><span data-stu-id="72f6e-204">If, for each pair of matching parameters `Mj` and `Nj`, `Mj` is less or equally generic than `Nj` with respect to type parameters on the method, and at least one `Mj` is less generic with respect to type parameters on the method.</span></span>
2. <span data-ttu-id="72f6e-205">Andernfalls gilt: Wenn für jedes Paar von übereinstimmenden Parametern `Mj` und `Nj`ist `Mj` kleiner oder gleichmäßiger generisch als `Nj` in Bezug auf Typparameter des Typs, und mindestens ein `Mj` ist in Bezug auf Typparameter des Typs weniger generisch. `M` ist weniger generisch als `N`.</span><span class="sxs-lookup"><span data-stu-id="72f6e-205">Otherwise, if for each pair of matching parameters `Mj` and `Nj`, `Mj` is less or equally generic than `Nj` with respect to type parameters on the type, and at least one `Mj` is less generic with respect to type parameters on the type, then `M` is less generic than `N`.</span></span>

<span data-ttu-id="72f6e-206">Ein Parameter `M` gilt als gleichermaßen generisch für einen Parameter `N`, wenn die Typen `Mt` und `Nt` beide auf Typparameter verweisen oder beide nicht auf Typparameter verweisen.</span><span class="sxs-lookup"><span data-stu-id="72f6e-206">A parameter `M` is considered to be equally generic to a parameter `N` if their types `Mt` and `Nt` both refer to type parameters or both don't refer to type parameters.</span></span> <span data-ttu-id="72f6e-207">`M` gilt als weniger generisch als `N`, wenn `Mt` nicht auf einen Typparameter verweist und `Nt`.</span><span class="sxs-lookup"><span data-stu-id="72f6e-207">`M` is considered to be less generic than `N` if `Mt` does not refer to a type parameter and `Nt` does.</span></span>

<span data-ttu-id="72f6e-208">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="72f6e-208">For example:</span></span>

```vb
Class C1(Of T)
    Sub S1(Of U)(x As U, y As T)
    End Sub

    Sub S1(Of U)(x As U, y As U)
    End Sub

    Sub S2(x As Integer, y As T)
    End Sub

    Sub S2(x As T, y As T)
    End Sub
End Class

Module Test
    Sub Main()
        Dim x As C1(Of Integer) = New C1(Of Integer)

        x.S1(10, 10)    ' Calls S1(U, T)
        x.S2(10, 10)    ' Calls S2(Integer, T)
    End Sub
End Module
```

<span data-ttu-id="72f6e-209">Erweiterungs Methoden-Typparameter, die während der Currying korrigiert wurden, werden als Typparameter für den Typ betrachtet, nicht als Typparameter für die Methode.</span><span class="sxs-lookup"><span data-stu-id="72f6e-209">Extension method type parameters that were fixed during currying are considered type parameters on the type, not type parameters on the method.</span></span> <span data-ttu-id="72f6e-210">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="72f6e-210">For example:</span></span>

```vb
Imports System.Runtime.CompilerServices

Module Ext1
    <Extension> _
    Sub M1(Of T, U)(x As T, y As U, z As U)
    End Sub
End Module

Module Ext2
    <Extension> _
    Sub M1(Of T, U)(x As T, y As U, z As T)
    End Sub
End Module

Module Test
    Sub Main()
        Dim i As Integer = 10

        i.M1(10, 10)
    End Sub
End Module
```

#### <a name="depth-of-genericity"></a><span data-ttu-id="72f6e-211">Tiefe der Generika</span><span class="sxs-lookup"><span data-stu-id="72f6e-211">Depth of genericity</span></span>

<span data-ttu-id="72f6e-212">Ein Element `M` wird so bestimmt, dass es eine *größere Tiefe von Generika* als ein Element `N` hat, wenn für jedes Paar von übereinstimmenden Parametern `Mj` und `Nj``Mj` eine größere oder gleiche *Tiefe von Generika* als `Nj`aufweist und mindestens eine `Mj` eine größere Tiefe von Generika hat.</span><span class="sxs-lookup"><span data-stu-id="72f6e-212">A member `M` is determined to have *greater depth of genericity* than a member `N` if, for each pair of matching parameters  `Mj` and `Nj`, `Mj` has greater or equal *depth of genericity* than `Nj`, and at least one `Mj` is has greater depth of genericity.</span></span> <span data-ttu-id="72f6e-213">Die Tiefe der Generizität wird wie folgt definiert:</span><span class="sxs-lookup"><span data-stu-id="72f6e-213">Depth of genericity is defined as follows:</span></span>

* <span data-ttu-id="72f6e-214">Alle anderen als ein Typparameter haben eine größere Tiefe der Generizität als ein Typparameter.</span><span class="sxs-lookup"><span data-stu-id="72f6e-214">Anything other than a type parameter has greater depth of genericity than a type parameter;</span></span>

* <span data-ttu-id="72f6e-215">Ein konstruierter Typ hat rekursiv eine größere Tiefe von Generika als ein anderer konstruierter Typ (mit der gleichen Anzahl von Typargumenten), wenn mindestens ein Typargument eine größere Tiefe von Generizität aufweist und kein Typargument weniger tief als der entsprechende Typ aufweist. Argument in der anderen.</span><span class="sxs-lookup"><span data-stu-id="72f6e-215">Recursively, a constructed type has greater depth of genericity than another constructed type (with the same number of type arguments) if at least one type argument has greater depth of genericity and no type argument has less depth than the corresponding type argument in the other.</span></span>

* <span data-ttu-id="72f6e-216">Ein Arraytyp hat eine größere Tiefe von Generika als ein anderer Arraytyp (mit der gleichen Anzahl von Dimensionen), wenn der Elementtyp des ersten-Elements eine größere Tiefe der Generizität aufweist als der Elementtyp der zweiten.</span><span class="sxs-lookup"><span data-stu-id="72f6e-216">An array type has greater depth of genericity than another array type (with the same number of dimensions) if the element type of the first has greater depth of genericity than the element type of the second.</span></span>

<span data-ttu-id="72f6e-217">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="72f6e-217">For example:</span></span>

```vb
Module Test

    Sub f(Of T)(x As Task(Of T))
    End Sub

    Sub f(Of T)(x As T)
    End Sub

    Sub Main()
        Dim x As Task(Of Integer) = Nothing
        f(x)            ' Calls the first overload
    End Sub
End Module
```

### <a name="applicability-to-argument-list"></a><span data-ttu-id="72f6e-218">Anwendbarkeit für Argument Liste</span><span class="sxs-lookup"><span data-stu-id="72f6e-218">Applicability To Argument List</span></span>

<span data-ttu-id="72f6e-219">Eine Methode gilt *für einen* Satz von Typargumenten, Positions Argumenten und benannte Argumente, wenn die Methode mithilfe der Argumentlisten aufgerufen werden kann.</span><span class="sxs-lookup"><span data-stu-id="72f6e-219">A method is *applicable* to a set of type arguments, positional arguments, and named arguments if the method can be invoked using the argument lists.</span></span> <span data-ttu-id="72f6e-220">Die Argumentlisten werden wie folgt mit den Parameterlisten verglichen:</span><span class="sxs-lookup"><span data-stu-id="72f6e-220">The argument lists are matched against the parameter lists as follows:</span></span>

1. <span data-ttu-id="72f6e-221">Vergleichen Sie zuerst jedes Positions Argument entsprechend der Liste der Methoden Parameter.</span><span class="sxs-lookup"><span data-stu-id="72f6e-221">First, match each positional argument in order to the list of method parameters.</span></span> <span data-ttu-id="72f6e-222">Wenn mehr Positions Argumente als Parameter vorhanden sind und der letzte Parameter kein ParamArray ist, ist die Methode nicht anwendbar.</span><span class="sxs-lookup"><span data-stu-id="72f6e-222">If there are more positional arguments than parameters and the last parameter is not a paramarray, the method is not applicable.</span></span> <span data-ttu-id="72f6e-223">Andernfalls wird der ParamArray-Parameter mit Parametern des paramarray-Elementtyps erweitert, sodass er mit der Anzahl der positionellen Argumente übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="72f6e-223">Otherwise, the paramarray parameter is expanded with parameters of the paramarray element type to match the number of positional arguments.</span></span> <span data-ttu-id="72f6e-224">Wenn ein Positions Argument weggelassen wird, das in ein ParamArray-Array wechselt, ist die Methode nicht anwendbar.</span><span class="sxs-lookup"><span data-stu-id="72f6e-224">If a positional argument is omitted that would go into a paramarray, the method is not applicable.</span></span>
2. <span data-ttu-id="72f6e-225">Vergleichen Sie als nächstes jedes benannte Argument mit einem Parameter mit dem angegebenen Namen.</span><span class="sxs-lookup"><span data-stu-id="72f6e-225">Next, match each named argument to a parameter with the given name.</span></span> <span data-ttu-id="72f6e-226">Wenn eines der benannten Argumente nicht übereinstimmt, mit einem ParamArray-Parameter übereinstimmt oder mit einem Argument übereinstimmt, das bereits mit einem anderen positionellen oder benannten Argument übereinstimmt, ist die Methode nicht anwendbar.</span><span class="sxs-lookup"><span data-stu-id="72f6e-226">If one of the named arguments fails to match, matches a paramarray parameter, or matches an argument already matched with another positional or named argument, the method is not applicable.</span></span>
3. <span data-ttu-id="72f6e-227">Wenn die Typargumente angegeben wurden, werden Sie als nächstes mit der Typparameter Liste abgeglichen.</span><span class="sxs-lookup"><span data-stu-id="72f6e-227">Next, if type arguments have been specified, they are matched against the type parameter list .</span></span> <span data-ttu-id="72f6e-228">Wenn die beiden Listen nicht die gleiche Anzahl von Elementen aufweisen, ist die Methode nicht anwendbar, es sei denn, die Typargument Liste ist leer.</span><span class="sxs-lookup"><span data-stu-id="72f6e-228">If the two lists do not have the same number of elements, the method is not applicable, unless the type argument list is empty.</span></span> <span data-ttu-id="72f6e-229">Wenn die Typargument Liste leer ist, wird der Typrückschluss verwendet, um die Typargument Liste zu versuchen und abzuleiten.</span><span class="sxs-lookup"><span data-stu-id="72f6e-229">If the type argument list is empty, type inference is used to try and infer the type argument list.</span></span> <span data-ttu-id="72f6e-230">Wenn das Ableiten von Typen fehlschlägt, ist die Methode nicht anwendbar.</span><span class="sxs-lookup"><span data-stu-id="72f6e-230">If type inferencing fails, the method is not applicable.</span></span> <span data-ttu-id="72f6e-231">Andernfalls werden die Typargumente an der Stelle der Typparameter in der Signatur ausgefüllt. Wenn nicht übereinstimmende Parameter nicht optional sind, ist die Methode nicht anwendbar.</span><span class="sxs-lookup"><span data-stu-id="72f6e-231">Otherwise, the type arguments are filled in the place of the type parameters in the signature.If parameters that have not been matched are not optional, the method is not applicable.</span></span>
4. <span data-ttu-id="72f6e-232">Wenn die Argument Ausdrücke nicht implizit in die Typen der entsprechenden Parameter konvertiert werden können, ist die Methode nicht anwendbar.</span><span class="sxs-lookup"><span data-stu-id="72f6e-232">If the argument expressions are not implicitly convertible to the types of the parameters they match, then the method is not applicable.</span></span>
5. <span data-ttu-id="72f6e-233">Wenn ein Parameter ByRef ist und es keine implizite Konvertierung vom Typ des Parameters in den Typ des Arguments gibt, ist die Methode nicht anwendbar.</span><span class="sxs-lookup"><span data-stu-id="72f6e-233">If a parameter is ByRef, and there is not an implicit conversion from the type of the parameter to the type of the argument, then the method is not applicable.</span></span>
6. <span data-ttu-id="72f6e-234">Wenn Typargumente gegen die Einschränkungen der Methode verstoßen (einschließlich der aus Schritt 3 abgelegten Typargumente), ist die Methode nicht anwendbar.</span><span class="sxs-lookup"><span data-stu-id="72f6e-234">If type arguments violate the method's constraints (including the inferred type arguments from Step 3), the method is not applicable.</span></span> <span data-ttu-id="72f6e-235">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="72f6e-235">For example:</span></span>

```vb
Module Module1
    Sub Main()
        f(Of Integer)(New Exception)
        ' picks the first overload (narrowing),
        ' since the second overload (widening) violates constraints 
    End Sub

    Sub f(Of T)(x As IComparable)
    End Sub

    Sub f(Of T As Class)(x As Object)
    End Sub
End Module
```

<span data-ttu-id="72f6e-236">Wenn ein einzelner Argument Ausdruck mit einem ParamArray-Parameter übereinstimmt und der Typ des Argument Ausdrucks sowohl in den Typ des ParamArray-Parameters als auch in den ParamArray-Elementtyp konvertiert werden kann, ist die-Methode sowohl in den erweiterten als auch in nicht erweiterten Formularen anwendbar. mit zwei Ausnahmen.</span><span class="sxs-lookup"><span data-stu-id="72f6e-236">If a single argument expression matches a paramarray parameter and the type of the argument expression is convertible to both the type of the paramarray parameter and the paramarray element type, the method is applicable in both its expanded and unexpanded forms, with two exceptions.</span></span> <span data-ttu-id="72f6e-237">Wenn die Konvertierung vom Typ des Argument Ausdrucks in den ParamArray-Typ einschränkend ist, ist die-Methode nur in der erweiterten Form anwendbar.</span><span class="sxs-lookup"><span data-stu-id="72f6e-237">If the conversion from the type of the argument expression to the paramarray type is narrowing, then the method is only applicable in its expanded form.</span></span> <span data-ttu-id="72f6e-238">Wenn es sich bei dem Argument Ausdruck um die Literale `Nothing`handelt, ist die Methode nur in ihrer nicht erweiterten Form anwendbar.</span><span class="sxs-lookup"><span data-stu-id="72f6e-238">If the argument expression is the literal `Nothing`, then the method is only applicable in its unexpanded form.</span></span> <span data-ttu-id="72f6e-239">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="72f6e-239">For example:</span></span>

```vb
Module Test
    Sub F(ParamArray a As Object())
        Dim o As Object

        For Each o In a
            Console.Write(o.GetType().FullName)
            Console.Write(" ")
        Next o
        Console.WriteLine()
    End Sub

    Sub Main()
        Dim a As Object() = { 1, "Hello", 123.456 }
        Dim o As Object = a

        F(a)
        F(CType(a, Object))
        F(o)
        F(CType(o, Object()))
    End Sub
End Module
```

<span data-ttu-id="72f6e-240">Im obigen Beispiel wird die folgende Ausgabe erzeugt:</span><span class="sxs-lookup"><span data-stu-id="72f6e-240">The above example produces the following output:</span></span>

```console
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

<span data-ttu-id="72f6e-241">In den ersten und letzten Aufrufen von `F`ist die normale Form von `F` anwendbar, weil eine erweiternde Konvertierung vom Argumenttyp zum Parametertyp vorhanden ist (beide sind vom Typ `Object()`), und das Argument wird als regulärer Wert Parameter übergeben.</span><span class="sxs-lookup"><span data-stu-id="72f6e-241">In the first and last invocations of `F`, the normal form of `F` is applicable because a widening conversion exists from the argument type to the parameter type (both are of type `Object()`), and the argument is passed as a regular value parameter.</span></span> <span data-ttu-id="72f6e-242">Im zweiten und dritten Aufruf ist die normale Form von `F` nicht anwendbar, da keine erweiternde Konvertierung vom Argumenttyp zum Parametertyp vorhanden ist (Konvertierungen von `Object` in `Object()` sind einschränkende).</span><span class="sxs-lookup"><span data-stu-id="72f6e-242">In the second and third invocations, the normal form of `F` is not applicable because no widening conversion exists from the argument type to the parameter type (conversions from `Object` to `Object()` are narrowing).</span></span> <span data-ttu-id="72f6e-243">Allerdings ist die erweiterte Form von `F` anwendbar, und ein `Object()` mit einem Element wird durch den Aufruf erstellt.</span><span class="sxs-lookup"><span data-stu-id="72f6e-243">However, the expanded form of `F` is applicable, and a one-element `Object()` is created by the invocation.</span></span> <span data-ttu-id="72f6e-244">Das einzelne Element des Arrays wird mit dem angegebenen Argument Wert initialisiert (der selbst ein Verweis auf einen `Object()`ist).</span><span class="sxs-lookup"><span data-stu-id="72f6e-244">The single element of the array is initialized with the given argument value (which itself is a reference to an `Object()`).</span></span>

### <a name="passing-arguments-and-picking-arguments-for-optional-parameters"></a><span data-ttu-id="72f6e-245">Übergeben von Argumenten und Auswählen von Argumenten für optionale Parameter</span><span class="sxs-lookup"><span data-stu-id="72f6e-245">Passing Arguments, and Picking Arguments for Optional Parameters</span></span>

<span data-ttu-id="72f6e-246">Wenn ein Parameter ein Wert Parameter ist, muss der übereinstimmende Argument Ausdruck als Wert klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="72f6e-246">If a parameter is a value parameter, the matching argument expression must be classified as a value.</span></span> <span data-ttu-id="72f6e-247">Der Wert wird in den Typ des Parameters konvertiert und zur Laufzeit als Parameter übergeben.</span><span class="sxs-lookup"><span data-stu-id="72f6e-247">The value is converted to the type of the parameter and passed in as the parameter at run time.</span></span> <span data-ttu-id="72f6e-248">Wenn es sich bei dem Parameter um einen Verweis Parameter handelt und der übereinstimmende Argument Ausdruck als Variable klassifiziert ist, deren Typ mit dem-Parameter übereinstimmt, wird ein Verweis auf die Variable zur Laufzeit als Parameter übergeben.</span><span class="sxs-lookup"><span data-stu-id="72f6e-248">If the parameter is a reference parameter and the matching argument expression is classified as a variable whose type is the same as the parameter, then a reference to the variable is passed in as the parameter at run time.</span></span>

<span data-ttu-id="72f6e-249">Andernfalls, wenn der übereinstimmende Argument Ausdruck als Variablen-, Wert-oder Eigenschafts Zugriff klassifiziert wird, wird eine temporäre Variable vom Typ des Parameters zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="72f6e-249">Otherwise, if the matching argument expression is classified as a variable, value, or property access, then a temporary variable of the type of the parameter is allocated.</span></span> <span data-ttu-id="72f6e-250">Vor dem Methodenaufruf zur Laufzeit wird der Argument Ausdruck als Wert neu klassifiziert, in den Typ des Parameters konvertiert und der temporären Variablen zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="72f6e-250">Before the method invocation at run time, the argument expression is reclassified as a value, converted to the type of the parameter, and assigned to the temporary variable.</span></span> <span data-ttu-id="72f6e-251">Anschließend wird ein Verweis auf die temporäre Variable als Parameter übergeben.</span><span class="sxs-lookup"><span data-stu-id="72f6e-251">Then a reference to the temporary variable is passed in as the parameter.</span></span> <span data-ttu-id="72f6e-252">Nachdem der Methodenaufruf ausgewertet wurde und der Argument Ausdruck als Variablen-oder Eigenschaften Zugriff klassifiziert ist, wird die temporäre Variable dem Variablen Ausdruck oder dem Eigenschafts Zugriffs Ausdruck zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="72f6e-252">After the method invocation is evaluated, if the argument expression is classified as a variable or property access, the temporary variable is assigned to the variable expression or the property access expression.</span></span> <span data-ttu-id="72f6e-253">Wenn der Eigenschafts Zugriffs Ausdruck keinen `Set` Accessor aufweist, wird die Zuweisung nicht durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="72f6e-253">If the property access expression has no `Set` accessor, then the assignment is not performed.</span></span>

<span data-ttu-id="72f6e-254">Bei optionalen Parametern, bei denen kein Argument angegeben wurde, wählt der Compiler wie unten beschrieben Argumente aus.</span><span class="sxs-lookup"><span data-stu-id="72f6e-254">For optional parameters where an argument has not been provided, the compiler picks arguments as described below.</span></span> <span data-ttu-id="72f6e-255">In allen Fällen testet er nach der generischen Typersetzung den Parametertyp.</span><span class="sxs-lookup"><span data-stu-id="72f6e-255">In all cases it tests against the parameter type after generic type substitution.</span></span>

* <span data-ttu-id="72f6e-256">Wenn der optionale Parameter das-Attribut `System.Runtime.CompilerServices.CallerLineNumber`hat und der Aufruf von einem Speicherort im Quellcode aus erfolgt, und ein numerisches wahrsten, das die Zeilennummer dieses Speicher Orts darstellt, eine intrinsische Konvertierung in den Parametertyp aufweist, wird das numerische Literale verwendet.</span><span class="sxs-lookup"><span data-stu-id="72f6e-256">If the optional parameter has the attribute `System.Runtime.CompilerServices.CallerLineNumber`, and the invocation is from a location in source code, and a numeric literal representing that location's line number has an intrinsic conversion to the parameter type, then the numeric literal is used.</span></span> <span data-ttu-id="72f6e-257">Wenn der Aufruf mehrere Zeilen umfasst, ist die Wahl der zu verwendenden Zeile von der Implementierung abhängig.</span><span class="sxs-lookup"><span data-stu-id="72f6e-257">If the invocation spans multiple lines, then the choice of which line to use is implementation-dependent.</span></span>

* <span data-ttu-id="72f6e-258">Wenn der optionale-Parameter das-Attribut `System.Runtime.CompilerServices.CallerFilePath`hat und der Aufruf von einem Speicherort im Quellcode aus erfolgt und ein Zeichenfolgenliteral, das den Dateipfad des Speicher Orts darstellt, eine intrinsische Konvertierung in den Parametertyp aufweist, wird das Zeichenfolgenliterale verwendet</span><span class="sxs-lookup"><span data-stu-id="72f6e-258">If the optional parameter has the attribute `System.Runtime.CompilerServices.CallerFilePath`, and the invocation is from a location in source code, and a string literal representing that location's file path has an intrinsic conversion to the parameter type, then the string literal is used.</span></span> <span data-ttu-id="72f6e-259">Das Format des Dateipfads ist implementierungsabhängig.</span><span class="sxs-lookup"><span data-stu-id="72f6e-259">The format of the file path is implementation-dependent.</span></span>

* <span data-ttu-id="72f6e-260">Wenn der optionale-Parameter das-Attribut `System.Runtime.CompilerServices.CallerMemberName`hat und der Aufruf innerhalb des Texts eines Typmembers oder eines Attributs liegt, das auf einen beliebigen Teil dieses Typmembers angewendet wird, und ein zeichenfolgenliteralelement, das den Elementnamen darstellt, eine intrinsische Konvertierung in den Parametertyp hat, wird das Zeichenfolgenliteral</span><span class="sxs-lookup"><span data-stu-id="72f6e-260">If the optional parameter has the attribute `System.Runtime.CompilerServices.CallerMemberName`, and the invocation is within the body of a type member or in an attribute applied to any part of that type member, and a string literal representing that member name has an intrinsic conversion to the parameter type, then the string literal is used.</span></span> <span data-ttu-id="72f6e-261">Bei aufrufen, die Teil von Eigenschaftenaccessoren oder benutzerdefinierten Ereignis Handlern sind, ist der verwendete Elementname der der Eigenschaft oder des Ereignisses.</span><span class="sxs-lookup"><span data-stu-id="72f6e-261">For invocations that are part of property accessors or custom event handlers, then the member name used is that of the property or event itself.</span></span> <span data-ttu-id="72f6e-262">Bei aufrufen, die Teil eines Operators oder Konstruktors sind, wird ein Implementierungs spezifischer Name verwendet.</span><span class="sxs-lookup"><span data-stu-id="72f6e-262">For invocations that are part of an operator or constructor, then an implementation-specific name is used.</span></span>

<span data-ttu-id="72f6e-263">Wenn keines der oben genannten Bedingungen zutrifft, wird der Standardwert des optionalen Parameters verwendet (oder `Nothing`, wenn kein Standardwert angegeben ist).</span><span class="sxs-lookup"><span data-stu-id="72f6e-263">If none of the above apply, then the optional parameter's default value is used (or `Nothing` if no default value is supplied).</span></span> <span data-ttu-id="72f6e-264">Wenn mehr als eine der oben genannten Bedingungen zutrifft, ist die Entscheidung, welche verwendet werden soll, implementierungsabhängig.</span><span class="sxs-lookup"><span data-stu-id="72f6e-264">And if more than one of the above apply, then the choice of which to use is implementation-dependent.</span></span>

<span data-ttu-id="72f6e-265">Die `CallerLineNumber`-und `CallerFilePath` Attribute sind für die Protokollierung nützlich.</span><span class="sxs-lookup"><span data-stu-id="72f6e-265">The `CallerLineNumber` and `CallerFilePath` attributes are useful for logging.</span></span> <span data-ttu-id="72f6e-266">Der `CallerMemberName` ist für die Implementierung von `INotifyPropertyChanged`nützlich.</span><span class="sxs-lookup"><span data-stu-id="72f6e-266">The `CallerMemberName` is useful for implementing `INotifyPropertyChanged`.</span></span> <span data-ttu-id="72f6e-267">Hier finden Sie Beispiele.</span><span class="sxs-lookup"><span data-stu-id="72f6e-267">Here are examples.</span></span>

```vb
Sub Log(msg As String,
        <CallerFilePath> Optional file As String = Nothing,
        <CallerLineNumber> Optional line As Integer? = Nothing)
    Console.WriteLine("{0}:{1} - {2}", file, line, msg)
End Sub

WriteOnly Property p As Integer
    Set(value As Integer)
        Notify(_p, value)
    End Set
End Property

Private _p As Integer

Sub Notify(Of T As IEquatable(Of T))(ByRef v1 As T, v2 As T,
        <CallerMemberName> Optional prop As String = Nothing)
    If v1 IsNot Nothing AndAlso v1.Equals(v2) Then Return
    If v1 Is Nothing AndAlso v2 Is Nothing Then Return
    v1 = v2
    RaiseEvent PropertyChanged(Me, New PropertyChangedEventArgs(prop))
End Sub
```

<span data-ttu-id="72f6e-268">Zusätzlich zu den oben genannten optionalen Parametern werden von Microsoft Visual Basic auch einige zusätzliche optionale Parameter erkannt, wenn Sie aus Metadaten importiert werden (d. h. aus einem dll-Verweis):</span><span class="sxs-lookup"><span data-stu-id="72f6e-268">In addition to the optional parameters above, Microsoft Visual Basic also recognizes some additional optional parameters if they are imported from metadata (i.e. from a DLL reference):</span></span>

* <span data-ttu-id="72f6e-269">Beim Importieren aus Metadaten behandelt Visual Basic auch den Parameter `<Optional>` als Hinweis darauf, dass der Parameter optional ist: auf diese Weise ist es möglich, eine Deklaration zu importieren, die einen optionalen Parameter, aber keinen Standardwert aufweist. Dies ist auch dann nicht möglich, wenn dies mit dem `Optional`-Schlüsselwort ausgedrückt werden kann.</span><span class="sxs-lookup"><span data-stu-id="72f6e-269">Upon importing from metadata, Visual Basic also treats the parameter `<Optional>` as indicative that the parameter is optional: in this way it is possible to import a declaration which has an optional parameter but no default value, even though this can't be expressed using the `Optional` keyword.</span></span>

* <span data-ttu-id="72f6e-270">Wenn der optionale-Parameter das-Attribut `Microsoft.VisualBasic.CompilerServices.OptionCompareAttribute`und das numerische Literale 1 oder 0 eine Konvertierung in den Parametertyp aufweist, verwendet der Compiler als Argument entweder das Literale 1, wenn `Option Compare Text` wirksam ist, oder das Literale 0, wenn `Optional Compare Binary` wirksam ist.</span><span class="sxs-lookup"><span data-stu-id="72f6e-270">If the optional parameter has the attribute `Microsoft.VisualBasic.CompilerServices.OptionCompareAttribute`, and the numeric literal 1 or 0 has a conversion to the parameter type, then the compiler uses as argument either the literal 1 if `Option Compare Text` is in effect, or the literal 0 if `Optional Compare Binary` is in effect.</span></span>

* <span data-ttu-id="72f6e-271">Wenn der optionale-Parameter das-Attribut `System.Runtime.CompilerServices.IDispatchConstantAttribute`hat und der Typ `Object`und kein Standardwert angegeben ist, verwendet der Compiler das Argument `New System.Runtime.InteropServices.DispatchWrapper(Nothing)`.</span><span class="sxs-lookup"><span data-stu-id="72f6e-271">If the optional parameter has the attribute `System.Runtime.CompilerServices.IDispatchConstantAttribute`, and it has type `Object`, and it does not specify a default value, then the compiler uses the argument `New System.Runtime.InteropServices.DispatchWrapper(Nothing)`.</span></span>

* <span data-ttu-id="72f6e-272">Wenn der optionale-Parameter das-Attribut `System.Runtime.CompilerServices.IUnknownConstantAttribute`hat und der Typ `Object`und kein Standardwert angegeben ist, verwendet der Compiler das Argument `New System.Runtime.InteropServices.UnknownWrapper(Nothing)`.</span><span class="sxs-lookup"><span data-stu-id="72f6e-272">If the optional parameter has the attribute `System.Runtime.CompilerServices.IUnknownConstantAttribute`, and it has type `Object`, and it does not specify a default value, then the compiler uses the argument `New System.Runtime.InteropServices.UnknownWrapper(Nothing)`.</span></span>

* <span data-ttu-id="72f6e-273">Wenn der optionale Parameter den Typ `Object`hat und keinen Standardwert angibt, verwendet der Compiler das Argument `System.Reflection.Missing.Value`.</span><span class="sxs-lookup"><span data-stu-id="72f6e-273">If the optional parameter has type `Object`, and it does not specify a default value, then the compiler uses the argument `System.Reflection.Missing.Value`.</span></span>

### <a name="conditional-methods"></a><span data-ttu-id="72f6e-274">Bedingte Methoden</span><span class="sxs-lookup"><span data-stu-id="72f6e-274">Conditional Methods</span></span>

<span data-ttu-id="72f6e-275">Wenn die Ziel Methode, auf die ein Aufruf Ausdruck verweist, eine Unterroutine ist, die kein Member einer Schnittstelle ist, und wenn die Methode über ein oder mehrere `System.Diagnostics.ConditionalAttribute` Attribute verfügt, hängt die Auswertung des Ausdrucks von den Konstanten für die bedingte Kompilierung ab, die an diesem Punkt in der Quelldatei definiert sind.</span><span class="sxs-lookup"><span data-stu-id="72f6e-275">If the target method to which an invocation expression refers is a subroutine that is not a member of an interface and if the method has one or more `System.Diagnostics.ConditionalAttribute` attributes, evaluation of the expression depends on the conditional compilation constants defined at that point in the source file.</span></span> <span data-ttu-id="72f6e-276">Jede Instanz des-Attributs gibt eine Zeichenfolge an, die eine Konstante für eine bedingte Kompilierung benennt.</span><span class="sxs-lookup"><span data-stu-id="72f6e-276">Each instance of the attribute specifies a string, which names a conditional compilation constant.</span></span> <span data-ttu-id="72f6e-277">Jede bedingte Kompilierungs Konstante wird ausgewertet, als wäre sie Teil einer Anweisung für die bedingte Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="72f6e-277">Each conditional compilation constant is evaluated as if it were part of a conditional compilation statement.</span></span> <span data-ttu-id="72f6e-278">Wenn die Konstante als `True`ausgewertet wird, wird der Ausdruck normalerweise zur Laufzeit ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="72f6e-278">If the constant evaluates to `True`, the expression is evaluated normally at run time.</span></span> <span data-ttu-id="72f6e-279">Wenn die Konstante als `False`ausgewertet wird, wird der Ausdruck überhaupt nicht ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="72f6e-279">If the constant evaluates to `False`, the expression is not evaluated at all.</span></span>

<span data-ttu-id="72f6e-280">Bei der Suche nach dem-Attribut wird die am meisten abgeleitete Deklaration einer über schreibbaren Methode geprüft.</span><span class="sxs-lookup"><span data-stu-id="72f6e-280">When looking for the attribute, the most derived declaration of an overridable method is checked.</span></span>

<span data-ttu-id="72f6e-281">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="72f6e-281">__Note.__</span></span> <span data-ttu-id="72f6e-282">Das-Attribut ist für Funktionen oder Schnittstellen Methoden ungültig und wird ignoriert, wenn es bei beiden Methoden angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="72f6e-282">The attribute is not valid on functions or interface methods and is ignored if specified on either kind of method.</span></span> <span data-ttu-id="72f6e-283">Daher werden bedingte Methoden nur in aufrufenden Anweisungen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="72f6e-283">Thus, conditional methods will only appear in invocation statements.</span></span>

### <a name="type-argument-inference"></a><span data-ttu-id="72f6e-284">Typargument Rückschluss</span><span class="sxs-lookup"><span data-stu-id="72f6e-284">Type Argument Inference</span></span>

<span data-ttu-id="72f6e-285">Wenn eine Methode mit Typparametern aufgerufen wird, ohne Typargumente anzugeben, wird ein *Typargument abgeleitet* , um die Typargumente für den Aufruf abzuleiten.</span><span class="sxs-lookup"><span data-stu-id="72f6e-285">When a method with type parameters is called without specifying type arguments, *type argument inference* is used to try and infer type arguments for the call.</span></span> <span data-ttu-id="72f6e-286">Dadurch wird eine natürlichere Syntax zum Aufrufen einer Methode mit Typparametern ermöglicht, wenn die Typargumente trivial abgeleitet werden können.</span><span class="sxs-lookup"><span data-stu-id="72f6e-286">This allows a more natural syntax to be used for calling a method with type parameters when the type arguments can be trivially inferred.</span></span> <span data-ttu-id="72f6e-287">Beispielsweise mit der folgenden Methoden Deklaration:</span><span class="sxs-lookup"><span data-stu-id="72f6e-287">For example, given the following method declaration:</span></span>

```vb
Module Util
    Function Choose(Of T)(b As Boolean, first As T, second As T) As T
        If b Then
            Return first
        Else
            Return second
        End If
    End Function
End Class
```

<span data-ttu-id="72f6e-288">Es ist möglich, die `Choose`-Methode aufzurufen, ohne explizit ein Typargument anzugeben:</span><span class="sxs-lookup"><span data-stu-id="72f6e-288">it is possible to invoke the `Choose` method without explicitly specifying a type argument:</span></span>

```vb
' calls Choose(Of Integer)
Dim i As Integer = Util.Choose(True, 5, 213)
' calls Choose(Of String)
Dim s As String = Util.Choose(False, "a", "b") 
```

<span data-ttu-id="72f6e-289">Durch Typargument Rückschluss werden die Typargumente `Integer` und `String` von den Argumenten der Methode bestimmt.</span><span class="sxs-lookup"><span data-stu-id="72f6e-289">Through type argument inference, the type arguments `Integer` and `String` are determined from the arguments to the method.</span></span>

<span data-ttu-id="72f6e-290">Typargument Rückschluss findet *vor* der Neuklassifizierung von Ausdrücken in Lambda-Methoden oder Methoden Zeigern in der Argumentliste statt, da bei der Neuklassifizierung dieser beiden Arten von Ausdrücken der Typ des Parameters möglicherweise bekannt ist.</span><span class="sxs-lookup"><span data-stu-id="72f6e-290">Type argument inference occurs *before* expression reclassification is performed on lambda methods or method pointers in the argument list, since reclassification of those two kinds of expressions may require the type of the parameter to be known.</span></span>  <span data-ttu-id="72f6e-291">Wenn ein Satz von Argumenten `A1,...,An`, ein Satz von übereinstimmenden Parametern `P1,...,Pn` und ein Satz von Methodentypparametern `T1,...,Tn`, werden die Abhängigkeiten zwischen den Argumenten und den Methodentypparametern zuerst wie folgt erfasst:</span><span class="sxs-lookup"><span data-stu-id="72f6e-291">Given a set of arguments `A1,...,An`, a set of matching parameters `P1,...,Pn` and a set of method type parameters `T1,...,Tn`, the dependencies between the arguments and method type parameters are first collected as follows:</span></span>

* <span data-ttu-id="72f6e-292">Wenn `An` `Nothing` Literalwert ist, werden keine Abhängigkeiten generiert.</span><span class="sxs-lookup"><span data-stu-id="72f6e-292">If `An` is the `Nothing` literal, no dependencies are generated.</span></span>

* <span data-ttu-id="72f6e-293">Wenn `An` eine Lambda-Methode und der Typ der `Pn` ein konstruierter Delegattyp oder `System.Linq.Expressions.Expression(Of T)`ist, wobei `T` ein konstruierter Delegattyp ist,</span><span class="sxs-lookup"><span data-stu-id="72f6e-293">If `An` is a lambda method and the type of `Pn` is a constructed delegate type or `System.Linq.Expressions.Expression(Of T)`, where `T` is a constructed delegate type,</span></span>

* <span data-ttu-id="72f6e-294">Wenn der Typ eines Lambda-Methoden Parameters vom Typ des entsprechenden Parameters `Pn`abgeleitet wird und der Typ des Parameters von einem Methodentypparameter `Tn`abhängt, hat `An` eine Abhängigkeit von `Tn`.</span><span class="sxs-lookup"><span data-stu-id="72f6e-294">If the type of a lambda method parameter will be inferred from the type of the corresponding parameter `Pn`, and the type of the parameter depends on a method type parameter `Tn`, then `An` has a dependency on `Tn`.</span></span>

* <span data-ttu-id="72f6e-295">Wenn der Typ eines Lambda-Methoden Parameters angegeben wird und der Typ des entsprechenden Parameters `Pn` von einem Methodentypparameter `Tn`abhängt, hat `Tn` eine Abhängigkeit von `An`.</span><span class="sxs-lookup"><span data-stu-id="72f6e-295">If the type of a lambda method parameter is specified and the type of the corresponding parameter `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="72f6e-296">Wenn der Rückgabetyp von `Pn` von einem Methodentypparameter `Tn`abhängt, hat `Tn` eine Abhängigkeit von `An`.</span><span class="sxs-lookup"><span data-stu-id="72f6e-296">If the return type of `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="72f6e-297">Wenn `An` ein Methoden Zeiger ist und der Typ der `Pn` ein konstruierter Delegattyp ist,</span><span class="sxs-lookup"><span data-stu-id="72f6e-297">If `An` is a method pointer and the type of `Pn` is a constructed delegate type,</span></span>

* <span data-ttu-id="72f6e-298">Wenn der Rückgabetyp von `Pn` von einem Methodentypparameter `Tn`abhängt, hat `Tn` eine Abhängigkeit von `An`.</span><span class="sxs-lookup"><span data-stu-id="72f6e-298">If the return type of `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="72f6e-299">Wenn `Pn` ein konstruierter Typ ist und der Typ der `Pn` von einem Methodentypparameter `Tn`abhängt, hat `Tn` eine Abhängigkeit von `An`.</span><span class="sxs-lookup"><span data-stu-id="72f6e-299">If `Pn` is a constructed type and the type of `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="72f6e-300">Andernfalls wird keine Abhängigkeit generiert.</span><span class="sxs-lookup"><span data-stu-id="72f6e-300">Otherwise, no dependency is generated.</span></span>

<span data-ttu-id="72f6e-301">Nach dem Sammeln von Abhängigkeiten werden alle Argumente, die keine Abhängigkeiten aufweisen, entfernt.</span><span class="sxs-lookup"><span data-stu-id="72f6e-301">After collecting dependencies, any arguments that have no dependencies are eliminated.</span></span> <span data-ttu-id="72f6e-302">Wenn ein Methodentypparameter keine ausgehenden Abhängigkeiten hat (d. h., der Methodentypparameter ist nicht von einem Argument abhängig), schlägt der Typrückschluss fehl.</span><span class="sxs-lookup"><span data-stu-id="72f6e-302">If any method type parameters have no outgoing dependencies (i.e. the method type parameter does not depend on an argument), then type inference fails.</span></span> <span data-ttu-id="72f6e-303">Andernfalls werden die übrigen Argumente und Methodentypparameter in stark verbundene Komponenten gruppiert.</span><span class="sxs-lookup"><span data-stu-id="72f6e-303">Otherwise, the remaining arguments and method type parameters are grouped into strongly connected components.</span></span> <span data-ttu-id="72f6e-304">Bei einer stark verbundenen Komponente handelt es sich um einen Satz von Argumenten und Methoden Typen Parametern, bei denen ein beliebiges Element in der Komponente über Abhängigkeiten von anderen Elementen erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="72f6e-304">A strongly connected component is a set of arguments and method type parameters, where any element in the component is reachable via dependencies on other elements.</span></span>

<span data-ttu-id="72f6e-305">Die stark verbundenen Komponenten werden dann in topologischer Reihenfolge topologisch sortiert und verarbeitet:</span><span class="sxs-lookup"><span data-stu-id="72f6e-305">The strongly connected components are then topologically sorted and processed in topological order:</span></span>

* <span data-ttu-id="72f6e-306">Wenn die stark typisierte Komponente nur ein Element enthält,</span><span class="sxs-lookup"><span data-stu-id="72f6e-306">If the strongly typed component contains only one element,</span></span>

  * <span data-ttu-id="72f6e-307">Wenn das Element bereits als abgeschlossen markiert wurde, überspringen Sie es.</span><span class="sxs-lookup"><span data-stu-id="72f6e-307">If the element has already been marked complete, skip it.</span></span>

  * <span data-ttu-id="72f6e-308">Wenn das Element ein Argument ist, fügen Sie dem Methodentyp Parameter, die von ihm abhängen, Typhinweise aus dem-Argument hinzu, und markieren Sie das Element als "Complete".</span><span class="sxs-lookup"><span data-stu-id="72f6e-308">If the element is an argument, then add type hints from the argument to the method type parameters that depend on it and mark the element as complete.</span></span> <span data-ttu-id="72f6e-309">Wenn es sich bei dem Argument um eine Lambda-Methode mit Parametern handelt, die nach wie vor abgeleitet werden müssen, leiten Sie `Object` für die Typen dieser Parameter ab.</span><span class="sxs-lookup"><span data-stu-id="72f6e-309">If the argument is a lambda method with parameters that still need inferred types, then infer `Object` for the types of those parameters.</span></span>

  * <span data-ttu-id="72f6e-310">Wenn es sich bei dem Element um einen Methodentypparameter handelt, leiten Sie den Methodentypparameter ab, damit er der bestimmende Typ der Argumenttyp Hinweise ist, und markieren Sie das Element als Complete.</span><span class="sxs-lookup"><span data-stu-id="72f6e-310">If the element is a method type parameter, then infer the method type parameter to be the dominant type among the argument type hints and mark the element as complete.</span></span> <span data-ttu-id="72f6e-311">Wenn für einen Typhinweis eine Array Element Einschränkung festgestellt wird, werden nur Konvertierungen berücksichtigt, die zwischen Arrays des angegebenen Typs gültig sind (d. h. kovariante und systeminterne Array Konvertierungen).</span><span class="sxs-lookup"><span data-stu-id="72f6e-311">If a type hint has an array element restriction on it, then only conversions that are valid between arrays of the given type are considered (i.e. covariant and intrinsic array conversions).</span></span> <span data-ttu-id="72f6e-312">Wenn ein Typhinweis eine generische Argument Einschränkung aufweist, werden nur Identitäts Konvertierungen berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="72f6e-312">If a type hint has a generic argument restriction on it, then only identity conversions are considered.</span></span> <span data-ttu-id="72f6e-313">Wenn kein dominanter Typ ausgewählt werden kann, schlägt das ableiten fehl.</span><span class="sxs-lookup"><span data-stu-id="72f6e-313">If no dominant type can be chosen, inference fails.</span></span> <span data-ttu-id="72f6e-314">Wenn Lambda-Methoden Argument Typen von diesem Methodentypparameter abhängen, wird der Typ an die Lambda-Methode weitergegeben.</span><span class="sxs-lookup"><span data-stu-id="72f6e-314">If any lambda method argument types depend on this method type parameter, the type is propagated to the lambda method.</span></span>

* <span data-ttu-id="72f6e-315">Wenn die stark typisierte Komponente mehr als ein Element enthält, enthält die Komponente einen Zyklen.</span><span class="sxs-lookup"><span data-stu-id="72f6e-315">If the strongly typed component contains more than one element, then the component contains a cycle.</span></span>

  * <span data-ttu-id="72f6e-316">Konvertieren Sie für jeden Methodentypparameter, bei dem es sich um ein Element in der Komponente handelt, wenn der Methodentypparameter von einem Argument abhängt, das nicht als abgeschlossen markiert ist, diese Abhängigkeit in eine-Bestätigung, die am Ende des Rückschluss Prozesses geprüft wird.</span><span class="sxs-lookup"><span data-stu-id="72f6e-316">For each method type parameter that is an element in the component, if the method type parameter depends on an argument that is not marked complete, convert that dependency into an assertion that will be checked at the end of the inference process.</span></span>

  * <span data-ttu-id="72f6e-317">Starten Sie den Rückschluss Prozess an dem Punkt, an dem die stark typisierten Komponenten ermittelt wurden.</span><span class="sxs-lookup"><span data-stu-id="72f6e-317">Restart the inference process at the point at which the strongly typed components were determined.</span></span>

<span data-ttu-id="72f6e-318">Wenn der Typrückschluss für alle Methodentypparameter erfolgreich ist, werden alle Abhängigkeiten, die in Assertionen geändert wurden, geprüft.</span><span class="sxs-lookup"><span data-stu-id="72f6e-318">If type inference succeeds for all of the method type parameters, then any dependencies that were changed into assertions are checked.</span></span> <span data-ttu-id="72f6e-319">Eine-Übersetzung ist erfolgreich, wenn der Typ des Arguments implizit in den abzurufenden Typ des methodentypparameters konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="72f6e-319">An assertion succeeds if the type of the argument is implicitly convertible to the inferred type of the method type parameter.</span></span> <span data-ttu-id="72f6e-320">Wenn eine-Übersetzung fehlschlägt, schlägt das Typargument Rückschluss fehl.</span><span class="sxs-lookup"><span data-stu-id="72f6e-320">If an assertion fails, then type argument inference fails.</span></span>

<span data-ttu-id="72f6e-321">Wenn ein Argumenttyp `Ta` für ein Argument `A` und ein Parametertyp für einen Parameter `P``Tp` werden, werden die typanhinweise wie folgt generiert:</span><span class="sxs-lookup"><span data-stu-id="72f6e-321">Given an argument type `Ta` for an argument `A` and a parameter type `Tp` for a parameter `P`, type hints are generated as follows:</span></span>

* <span data-ttu-id="72f6e-322">Wenn `Tp` keinen Methodentypparameter einschließt, werden keine Hinweise generiert.</span><span class="sxs-lookup"><span data-stu-id="72f6e-322">If `Tp` does not involve any method type parameters then no hints are generated.</span></span>

* <span data-ttu-id="72f6e-323">Wenn `Tp` und `Ta` Array Typen desselben Rang sind, ersetzen Sie `Ta` und `Tp` durch die Elementtypen `Ta` und `Tp`, und starten Sie diesen Prozess mit einer Array Element Einschränkung neu.</span><span class="sxs-lookup"><span data-stu-id="72f6e-323">If `Tp` and `Ta` are array types of the same rank, then replace `Ta` and `Tp` with the element types of `Ta` and `Tp` and restart this process with an array element restriction.</span></span>

* <span data-ttu-id="72f6e-324">Wenn `Tp` ein Methodentypparameter ist, wird `Ta` als Typhinweis mit der aktuellen Einschränkung hinzugefügt, sofern vorhanden.</span><span class="sxs-lookup"><span data-stu-id="72f6e-324">If `Tp` is a method type parameter, then `Ta` is added as a type hint with the current restriction, if any.</span></span>

* <span data-ttu-id="72f6e-325">Wenn `A` eine Lambda-Methode ist und `Tp` ein konstruierter Delegattyp oder `System.Linq.Expressions.Expression(Of T)`ist, wobei `T` ein konstruierter Delegattyp ist, ersetzen Sie für jeden Lambda-Methoden Parametertyp `TL` und den entsprechenden delegatparametertyp `TD``Ta` durch `TL`, und starten Sie den Prozess ohne Einschränkung neu.`Tp``TD`</span><span class="sxs-lookup"><span data-stu-id="72f6e-325">If `A` is a lambda method and `Tp` is a constructed delegate type or `System.Linq.Expressions.Expression(Of T)`, where `T` is a constructed delegate type, for each lambda method parameter type `TL` and corresponding delegate parameter type `TD`, replace `Ta` with `TL` and `Tp` with `TD` and restart the process with no restriction.</span></span> <span data-ttu-id="72f6e-326">Ersetzen Sie dann `Ta` durch den Rückgabetyp der Lambda-Methode und:</span><span class="sxs-lookup"><span data-stu-id="72f6e-326">Then, replace `Ta` with the return type of the lambda method and:</span></span>

  * <span data-ttu-id="72f6e-327">Wenn `A` eine reguläre Lambda-Methode ist, ersetzen Sie `Tp` durch den Rückgabetyp des Delegattyps.</span><span class="sxs-lookup"><span data-stu-id="72f6e-327">if `A` is a regular lambda method, replace `Tp` with the return type of the delegate type;</span></span>
  * <span data-ttu-id="72f6e-328">Wenn `A` eine Async-Lambda-Methode ist und der Rückgabetyp des Delegattyps Formular `Task(Of T)` für einige `T`ist, ersetzen Sie `Tp` durch diese `T`;</span><span class="sxs-lookup"><span data-stu-id="72f6e-328">if `A` is an async lambda method and the return type of the delegate type has form `Task(Of T)` for some `T`, replace `Tp` with that `T`;</span></span>
  * <span data-ttu-id="72f6e-329">Wenn `A` eine iteratorlambda-Methode ist und der Rückgabetyp des Delegattyps Formular `IEnumerator(Of T)` oder `IEnumerable(Of T)` für einige `T`ist, ersetzen Sie `Tp` durch diese `T`.</span><span class="sxs-lookup"><span data-stu-id="72f6e-329">if `A` is an iterator lambda method and the return type of the delegate type has form `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`, replace `Tp` with that `T`.</span></span>
  * <span data-ttu-id="72f6e-330">Starten Sie als nächstes den Prozess ohne Einschränkung neu.</span><span class="sxs-lookup"><span data-stu-id="72f6e-330">Next, restart the process with no restriction.</span></span>

* <span data-ttu-id="72f6e-331">Wenn `A` ein Methoden Zeiger und `Tp` ein konstruierter Delegattyp ist, verwenden Sie die Parametertypen von `Tp`, um zu bestimmen, welche Methode auf `Tp`am meisten anwendbar ist.</span><span class="sxs-lookup"><span data-stu-id="72f6e-331">If `A` is a method pointer and `Tp` is a constructed delegate type, use the parameter types of `Tp` to determine which method pointed is most applicable to `Tp`.</span></span> <span data-ttu-id="72f6e-332">Wenn eine Methode am meisten anwendbar ist, ersetzen Sie `Ta` durch den Rückgabetyp der Methode und `Tp` mit dem Rückgabetyp des Delegattyps, und starten Sie den Prozess ohne Einschränkung neu.</span><span class="sxs-lookup"><span data-stu-id="72f6e-332">If there is a method that is most applicable, replace `Ta` with the return type of the method and `Tp` with the return type of the delegate type and restart the process with no restriction.</span></span>

* <span data-ttu-id="72f6e-333">Andernfalls muss `Tp` ein konstruierter Typ sein.</span><span class="sxs-lookup"><span data-stu-id="72f6e-333">Otherwise, `Tp` must be a constructed type.</span></span> <span data-ttu-id="72f6e-334">Der generische Typ `Tp`, `TG`</span><span class="sxs-lookup"><span data-stu-id="72f6e-334">Given `TG`, the generic type of `Tp`,</span></span>

  * <span data-ttu-id="72f6e-335">Wenn `Ta` `TG`ist, von `TG`erbt oder den Typ `TG` genau einmal implementiert, dann für jedes übereinstimmende Typargument `Tax` von `Ta` und `Tpx` `Tp`durch `Ta` ersetzen und `Tax` durch `Tp` und den Prozess mit einer generischen Argument Einschränkung neu starten.`Tpx`</span><span class="sxs-lookup"><span data-stu-id="72f6e-335">If `Ta` is `TG`, inherits from `TG`, or implements the type `TG` exactly once, then for each matching type argument `Tax` from `Ta` and `Tpx` from `Tp`, replace `Ta` with `Tax` and `Tp` with `Tpx` and restart the process with a generic argument restriction.</span></span>

  * <span data-ttu-id="72f6e-336">Andernfalls schlägt der Typrückschluss für die generische Methode fehl.</span><span class="sxs-lookup"><span data-stu-id="72f6e-336">Otherwise, type inference fails for the generic method.</span></span>

<span data-ttu-id="72f6e-337">Der Erfolg des Typrückschlusses gewährleistet nicht in und von sich, dass die Methode anwendbar ist.</span><span class="sxs-lookup"><span data-stu-id="72f6e-337">The success of type inference does not, in and of itself, guarantee that the method is applicable.</span></span>
