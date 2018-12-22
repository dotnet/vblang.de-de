### <a name="overloaded-method-resolution"></a><span data-ttu-id="986af-101">Die überladene Methodenauflösung</span><span class="sxs-lookup"><span data-stu-id="986af-101">Overloaded Method Resolution</span></span>

<span data-ttu-id="986af-102">In der Praxis sind die Regeln zum Bestimmen der Auflösung von funktionsüberladungen vorgesehen, die Überladung gefunden, die "nächstgelegene", um die tatsächlichen Argumente angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="986af-102">In practice, the rules for determining overload resolution are intended to find the overload that is "closest" to the actual arguments supplied.</span></span> <span data-ttu-id="986af-103">Ist eine Methode, deren die Argumenttypen entsprechen, ist diese Methode offensichtlich am nächsten.</span><span class="sxs-lookup"><span data-stu-id="986af-103">If there is a method whose parameter types match the argument types, then that method is obviously the closest.</span></span> <span data-ttu-id="986af-104">Was ausschließt, dass eine Methode genauer als ein anderes ist, wenn alle die Parametertypen schmaler als (oder gleich) die Parametertypen der anderen Methode.</span><span class="sxs-lookup"><span data-stu-id="986af-104">Barring that, one method is closer than another if all of its parameter types are narrower than (or the same as) the parameter types of the other method.</span></span> <span data-ttu-id="986af-105">Wenn weder Methodenparameter geringer ist als das andere sind, besteht keine Möglichkeit für zu bestimmen, welche Methode näher auf die Argumente.</span><span class="sxs-lookup"><span data-stu-id="986af-105">If neither method's parameters are narrower than the other, then there is no way for to determine which method is closer to the arguments.</span></span>

<span data-ttu-id="986af-106">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="986af-106">__Note.__</span></span> <span data-ttu-id="986af-107">Auflösung von funktionsüberladungen berücksichtigt nicht den erwarteten Rückgabetyp der Methode.</span><span class="sxs-lookup"><span data-stu-id="986af-107">Overload resolution does not take into account the expected return type of the method.</span></span> 

<span data-ttu-id="986af-108">Beachten Sie, dass aufgrund der Syntax benannter Parameter, die Reihenfolge des tatsächlichen und formalen Parameter möglicherweise nicht identisch sein.</span><span class="sxs-lookup"><span data-stu-id="986af-108">Also note that because of the named parameter syntax, the ordering of the actual and formal parameters may not be the same.</span></span>

<span data-ttu-id="986af-109">Wenn eine Methodengruppe, wird die am besten geeignete Methode in der Gruppe für eine Argumentliste bestimmt mithilfe der folgenden Schritte.</span><span class="sxs-lookup"><span data-stu-id="986af-109">Given a method group, the most applicable method in the group for an argument list is determined using the following steps.</span></span> <span data-ttu-id="986af-110">Wenn Sie nach dem Anwenden einer bestimmten Stufe, keine Elemente in der Gruppe bleiben, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="986af-110">If, after applying a particular step, no members remain in the set, then a compile-time error occurs.</span></span> <span data-ttu-id="986af-111">Wenn nur ein Element im Satz bleibt, ist dieser Member die am besten geeignete Member.</span><span class="sxs-lookup"><span data-stu-id="986af-111">If only one member remains in the set, then that member is the most applicable member.</span></span> <span data-ttu-id="986af-112">Die Schritte sind:</span><span class="sxs-lookup"><span data-stu-id="986af-112">The steps are:</span></span>

1.  <span data-ttu-id="986af-113">Wenn keine Typargumente angegeben wurden, gelten Sie Typrückschluss zunächst für alle Methoden, die über Typparameter verfügen.</span><span class="sxs-lookup"><span data-stu-id="986af-113">First, if no type arguments have been supplied, apply type inference to any methods which have type parameters.</span></span> <span data-ttu-id="986af-114">Wenn ein Typrückschluss für eine Methode erfolgreich ist, werden die abgeleiteten Typargumente für die jeweilige Methode verwendet.</span><span class="sxs-lookup"><span data-stu-id="986af-114">If type inference succeeds for a method, then the inferred type arguments are used for that particular method.</span></span> <span data-ttu-id="986af-115">Wenn Typrückschluss für eine Methode ein Fehler auftritt, wird diese Methode aus der Gruppe gelöscht.</span><span class="sxs-lookup"><span data-stu-id="986af-115">If type inference fails for a method, then that method is eliminated from the set.</span></span>

2.  <span data-ttu-id="986af-116">Eliminieren Sie als Nächstes alle Member aus der Gruppe, die nicht zugegriffen werden kann oder nicht anwendbar sind (Abschnitt [Anwendbarkeit auf Argumentliste](overload-resolution.md#applicability-to-argument-list)) an die Argumentliste</span><span class="sxs-lookup"><span data-stu-id="986af-116">Next, eliminate all members from the set that are inaccessible or not applicable (Section [Applicability To Argument List](overload-resolution.md#applicability-to-argument-list)) to the argument list</span></span>

3.  <span data-ttu-id="986af-117">Weiter, wenn eine oder mehrere Argumente sind `AddressOf` oder Lambda-Ausdrücke, berechnen Sie dann die *delegieren die Lockerung Ebenen* für jedes Argument wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="986af-117">Next, if one or more arguments are `AddressOf` or lambda expressions, then calculate the *delegate relaxation levels* for each such argument as below.</span></span> <span data-ttu-id="986af-118">Wenn die schlechteste (niedrigste Priorität) die Lockerung Ebene Delegieren `N` schlechter ist als der untersten Ebene der Delegat die Lockerung in `M`, beseitigen Sie `N` aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="986af-118">If the worst (lowest) delegate relaxation level in `N` is worse than the lowest delegate relaxation level in `M`, then eliminate `N` from the set.</span></span> <span data-ttu-id="986af-119">Die Delegat die Lockerung Ebenen lauten wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="986af-119">The delegate relaxation levels are as follows:</span></span>

    31. <span data-ttu-id="986af-120">*Delegat "Error"-Ebene Lockerung* : Wenn die `AddressOf` oder Lambda kann nicht in den Delegattyp konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="986af-120">*Error delegate relaxation level* -- if the `AddressOf` or lambda cannot be converted to the delegate type.</span></span>
  
    32. <span data-ttu-id="986af-121">*Einschränkende delegatlockerung Rückgabetyp oder Parameter* : Wenn das Argument ist `AddressOf` oder ein Lambda-Ausdruck, mit dem deklarierten Typ und die Konvertierung von ihren Rückgabetyp an den Delegaten zurückgeben Typ einschränkend; oder wenn das Argument mit einem regulären Lambda-Ausdruck ist und die Konvertierung von einem der Rückgabeausdrücke des Objekts an den Delegaten zurück, Typ führt zu einer Einschränkung, oder wenn das Argument eines Async-Lambdas und der Delegat, der Rückgabewert ist `Task(Of T)` und die Konvertierung von einem der Rückgabeausdrücke des Objekts zu `T` einschränkend; oder, wenn Das Argument ist ein Iterator-Lambda und den Rückgabetyp des Delegaten `IEnumerator(Of T)` oder `IEnumerable(Of T)` und die Konvertierung aus der "yield"-Operanden durch, um `T` einschränkend.</span><span class="sxs-lookup"><span data-stu-id="986af-121">*Narrowing delegate relaxation of return type or parameters* -- if the argument is `AddressOf` or a lambda with a declared type and the conversion from its return type to the delegate return type is narrowing; or if the argument is a regular lambda and the conversion from any of its return expressions to the delegate return type is narrowing, or if the argument is an async lambda and the delegate return type is `Task(Of T)` and the conversion from any of its return expressions to `T` is narrowing; or if the argument is an iterator lambda and the delegate return type `IEnumerator(Of T)` or `IEnumerable(Of T)` and the conversion from any of its yield operands to `T` is narrowing.</span></span>

    33. <span data-ttu-id="986af-122">*Erweiternde delegatlockerung ohne Signatur Delegieren* : Wenn ein Delegattyp ist `System.Delegate` oder `System.MultiCastDelegate` oder `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="986af-122">*Widening delegate relaxation to delegate without signature* -- if delegate type is `System.Delegate` or `System.MultiCastDelegate` or `System.Object`.</span></span>

    34. <span data-ttu-id="986af-123">*Drop zurückgegeben werden sollen oder Argumente delegatlockerung* : Wenn das Argument ist `AddressOf` oder ein Lambda-Ausdrucks mit Rückgabetyp deklariert und der Delegattyp verfügt nicht über einen Rückgabetyp; oder wenn das Argument ein Lambda-Ausdruck mit einem ist oder mehr, Ausdrücke und Typ des Delegaten zurückgeben verfügt nicht über einen Rückgabetyp; oder wenn das Argument ist `AddressOf` oder Lambda ohne Parameter und der Typ des Delegaten über Parameter verfügt.</span><span class="sxs-lookup"><span data-stu-id="986af-123">*Drop return or arguments delegate relaxation* -- if the argument is `AddressOf` or a lambda with a declared return type and the delegate type lacks a return type; or if the argument is a lambda with one or more return expressions and the delegate type lacks a return type; or if the argument is `AddressOf` or lambda with no parameters and the delegate type has parameters.</span></span>

    35. <span data-ttu-id="986af-124">*Erweiternde delegatlockerung des Rückgabetyps* : Wenn das Argument ist `AddressOf` oder einen Lambda-Ausdruck mit einem Rückgabetyp deklariert, und es ist eine erweiternde Konvertierung von der Rückgabetyp des Delegaten; oder wenn das Argument mit einem regulären Lambda-Ausdruck ist, in dem die Konvertierung vom alle Rückgabeausdrücke Rückgabetyp des Delegaten wird erweiternd oder Identität mit mindestens einer zu erweiternden; oder wenn das Argument ein asynchrones Lambda ist und der Delegat `Task(Of T)` oder `Task` und die Konvertierung von alle Rückgabeausdrücke zu `T` / `Object` erweiternd oder Identität mit mindestens einer zu erweiternden; ist oder wenn Das Argument ist ein Iterator-Lambda, und der Delegat `IEnumerator(Of T)` oder `IEnumerable(Of T)` oder `IEnumerator` oder `IEnumerable` und die Konvertierung von alle Rückgabeausdrücke zu `T` / `Object` ist erweiternd oder die Identität mit mindestens einer zu erweiternden.</span><span class="sxs-lookup"><span data-stu-id="986af-124">*Widening delegate relaxation of return type* -- if the argument is `AddressOf` or a lambda with a declared return type, and there is a widening conversion from its return type to that of the delegate; or if the argument is a regular lambda where the conversion from all return expressions to the delegate return type is widening or identity with at least one widening; or if the argument is an async lambda and the delegate is `Task(Of T)` or `Task` and the conversion from all return expressions to `T`/`Object` respectively is widening or identity with at least one widening; or if the argument is an iterator lambda and the delegate is `IEnumerator(Of T)` or `IEnumerable(Of T)` or `IEnumerator` or `IEnumerable` and the conversion from all return expressions to `T`/`Object` is widening or identity with at least one widening.</span></span>

    36. <span data-ttu-id="986af-125">*Identität delegatlockerung* : Wenn das Argument ist ein `AddressOf` oder ein Lambda-Ausdruck die Delegaten genau ohne entspricht erweiternde oder einschränkende oder Löschen von Parametern oder gibt zurück oder -Werten ergibt. Als Nächstes einige Elemente der Menge ist nicht erforderlich, einschränkende Konvertierungen auf eines der Argumente angewendet werden, und beseitigen alle Elemente, die dann.</span><span class="sxs-lookup"><span data-stu-id="986af-125">*Identity delegate relaxation* -- if the argument is an `AddressOf` or a lambda which matches the delegate exactly, with no widening or narrowing or dropping of parameters or returns or yields.Next, if some members of the set do not requiring narrowing conversions to be applicable to any of the arguments, then eliminate all members that do.</span></span> <span data-ttu-id="986af-126">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="986af-126">For example:</span></span>

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

4.  <span data-ttu-id="986af-127">Als Nächstes wird die Eliminierung von Duplikaten basierend auf der wie folgt einschränken durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="986af-127">Next, elimination is done based on narrowing as follows.</span></span> <span data-ttu-id="986af-128">(Beachten Sie, dass, wenn Option Strict On ist, klicken Sie dann alle Elemente, die erfordern einschränkende bereits passende gehalten werden (Abschnitt [Anwendbarkeit auf Argumentliste](overload-resolution.md#applicability-to-argument-list)) und Schritt2 entfernt.)</span><span class="sxs-lookup"><span data-stu-id="986af-128">(Note that, if Option Strict is On, then all members that require narrowing have already been judged inapplicable (Section [Applicability To Argument List](overload-resolution.md#applicability-to-argument-list)) and removed by Step 2.)</span></span>

    41. <span data-ttu-id="986af-129">Wenn nur eine Instanz-Elemente der Menge einschränkende Konvertierungen erfordern, in dem der Ausdruck Argumenttyp ist `Object`, beseitigen Sie alle anderen Elemente.</span><span class="sxs-lookup"><span data-stu-id="986af-129">If some instance members of the set only require narrowing conversions where the argument expression type is `Object`, then eliminate all other members.</span></span>
    42. <span data-ttu-id="986af-130">Wenn der Satz mehr als ein Element enthält, müssen Sie nur über einschränkende `Object`, wie eine spät gebundene Methode klicken Sie dann auf der Ziel-Aufrufausdruck neu klassifiziert (und ein Fehler wird ausgegeben, wenn der Typ, der die Methodengruppe enthält eine Schnittstelle ist, oder wenn eines der die entsprechenden Member wurden Erweiterungsmember).</span><span class="sxs-lookup"><span data-stu-id="986af-130">If the set contains more than one member which requires narrowing only from `Object`, then the invocation target expression is reclassified as a late-bound method access (and an error is given if the type containing the method group is an interface, or if any of the applicable members were extension members).</span></span>
    43. <span data-ttu-id="986af-131">Wenn es Kandidaten für alle, die nur erfordern Einschränken von numerischen Literalen, ausgewählt haben dann die spezifischste für alle verbliebenen Kandidaten durch die folgenden Schritte aus.</span><span class="sxs-lookup"><span data-stu-id="986af-131">If there are any candidates that only require narrowing from numeric literals, then chose the most specific among all remaining candidates by the steps below.</span></span> <span data-ttu-id="986af-132">Wenn der Gewinner erforderlich sind, nur Einschränken von numerischen Literalen, wird es als Ergebnis der Auflösung von funktionsüberladungen ausgewählt; Andernfalls handelt es sich um einen Fehler.</span><span class="sxs-lookup"><span data-stu-id="986af-132">If the winner requires only narrowing from numeric literals, then it is picked as the result of overload resolution; otherwise it is an error.</span></span>

    <span data-ttu-id="986af-133">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="986af-133">__Note.__</span></span> <span data-ttu-id="986af-134">Die Begründung für diese Regel ist dies, die aus, wenn ein Programm lose typisierten ist (d. h. als die meisten oder alle Variablen deklariert werden `Object`), Auflösung von funktionsüberladungen kann schwierig sein, wenn viele Konvertierungen von `Object` einschränkende sind.</span><span class="sxs-lookup"><span data-stu-id="986af-134">The justification for this rule is that if a program is loosely-typed (that is, most or all variables are declared as `Object`), overload resolution can be difficult when many conversions from `Object` are narrowing.</span></span> <span data-ttu-id="986af-135">Anstatt der überladungsauflösung, die in vielen Situationen (starke Typisierung der Argumente für Aufruf der Methode erforderlich ist), Auflösung fehl, die die entsprechende überladen wird die aufzurufende Methode bis zur Laufzeit verzögert.</span><span class="sxs-lookup"><span data-stu-id="986af-135">Rather than have the overload resolution fail in many situations (requiring strong typing of the arguments to the method call), resolution the appropriate overloaded method to call is deferred until run time.</span></span> <span data-ttu-id="986af-136">Dadurch wird den Aufruf lose typisierten ohne zusätzliche Umwandlungen erfolgreich.</span><span class="sxs-lookup"><span data-stu-id="986af-136">This allows the loosely-typed call to succeed without additional casts.</span></span> <span data-ttu-id="986af-137">Ein unglücklicher Nebeneffekt, allerdings ist diese ausführen, die den spät gebundenen Aufruf erfordert das Aufrufziel zum Umwandeln `Object`.</span><span class="sxs-lookup"><span data-stu-id="986af-137">An unfortunate side-effect of this, however, is that performing the late-bound call requires casting the call target to `Object`.</span></span> <span data-ttu-id="986af-138">Im Fall einer Struktur-Wert bedeutet dies, dass der Wert in ein temporäres geschachtelt werden muss.</span><span class="sxs-lookup"><span data-stu-id="986af-138">In the case of a structure value, this means that the value must be boxed to a temporary.</span></span> <span data-ttu-id="986af-139">Wenn die letztlich aufgerufene Methode versucht, ein Feld in der Struktur zu ändern, werden diese Änderung verloren, sobald die Methode zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="986af-139">If the method eventually called tries to change a field of the structure, this change will be lost once the method returns.</span></span> <span data-ttu-id="986af-140">Schnittstellen werden von dieser speziellen Regel ausgeschlossen, da es sich bei späte Bindung immer mit den Membern der Klasse oder Struktur Laufzeittyp, löst die andere Namen als den Membern der Schnittstellen haben können, die sie implementieren.</span><span class="sxs-lookup"><span data-stu-id="986af-140">Interfaces are excluded from this special rule because late binding always resolves against the members of the runtime class or structure type, which may have different names than the members of the interfaces they implement.</span></span>

5.  <span data-ttu-id="986af-141">Als Nächstes Wenn Instanzenmethoden in der Menge der keine einschränkende erforderlich sind verbleiben, beseitigen Sie alle Erweiterungsmethoden aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="986af-141">Next, if any instance methods remain in the set which do not require narrowing, then eliminate all extension methods from the set.</span></span> <span data-ttu-id="986af-142">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="986af-142">For example:</span></span>

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

    <span data-ttu-id="986af-143">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="986af-143">__Note.__</span></span> <span data-ttu-id="986af-144">Erweiterungsmethoden werden ignoriert, wenn entsprechende Instanz-Methoden, um sicherzustellen, dass das Hinzufügen eines Imports (die neuen Erweiterungsmethoden in den Gültigkeitsbereich einzubinden möglicherweise) keinen Aufruf auf eine vorhandene Instanzmethode einer Erweiterungsmethode erneut binden bewirken vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="986af-144">Extension methods are ignored if there are applicable instance methods to guarantee that adding an import (that might bring new extension methods into scope) will not cause a call on an existing instance method to rebind to an extension method.</span></span> <span data-ttu-id="986af-145">Wenn den umfassenden Rahmen einige Erweiterungsmethoden (d. h. auf Schnittstellen und/oder Typparameter definiert), ist dies ein sicherer Ansatz für die Bindung an die Erweiterungsmethoden.</span><span class="sxs-lookup"><span data-stu-id="986af-145">Given the broad scope of some extension methods (i.e. those defined on interfaces and/or type parameters), this is a safer approach to binding to extension methods.</span></span>

6.  <span data-ttu-id="986af-146">Weiter, If, erhalten alle zwei Elemente der Menge `M` und `N`, `M` mehr *bestimmte* (Abschnitt [Detailgenauigkeit der Member/Typen, die eine Argumentliste](overload-resolution.md#specificity-of-memberstypes-given-an-argument-list)) als `N`angesichts die Argumentliste, beseitigen `N` aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="986af-146">Next, if, given any two members of the set `M` and `N`, `M` is more *specific* (Section [Specificity of members/types given an argument list](overload-resolution.md#specificity-of-memberstypes-given-an-argument-list)) than `N` given the argument list, eliminate `N` from the set.</span></span> <span data-ttu-id="986af-147">Wenn mehr als ein Element im Satz bleibt die übrigen Elemente nicht gleichmäßig spezifisch erhält die Argumentliste sind und führt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="986af-147">If more than one member remains in the set and the remaining members are not equally specific given the argument list, a compile-time error results.</span></span>

7.  <span data-ttu-id="986af-148">Andernfalls erhält zwei Elemente der Menge, `M` und `N`, gelten die folgenden gleichwertigen geringfügiger-Regeln, in der Reihenfolge:</span><span class="sxs-lookup"><span data-stu-id="986af-148">Otherwise, given any two members of the set, `M` and `N`, apply the following tie-breaking rules, in order:</span></span>

    71. <span data-ttu-id="986af-149">Wenn `M` verfügt nicht über einen ParamArray-Parameter, aber `N` der Fall ist, oder wenn beides aber `M` übergibt weniger Argumente an den ParamArray-Parameter als `N` der Fall ist, klicken Sie dann zu beseitigen `N` aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="986af-149">If `M` does not have a ParamArray parameter but `N` does, or if both do but `M` passes fewer arguments into the ParamArray parameter than `N` does, then eliminate `N` from the set.</span></span> <span data-ttu-id="986af-150">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="986af-150">For example:</span></span>

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

        <span data-ttu-id="986af-151">Das obige Beispiel erzeugt die folgende Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="986af-151">The above example produces the following output:</span></span>

        ```
        F(Object, Object())
        F(Object, Object, Object())
        F(Object, Object, Object())
        G(Object)
        ```

        <span data-ttu-id="986af-152">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="986af-152">__Note.__</span></span> <span data-ttu-id="986af-153">Wenn eine Klasse eine Methode mit einem Paramarray-Parameter deklariert, ist es nicht ungewöhnlich, dass auch einige der erweiterten Formen als reguläre Methoden eingeschlossen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="986af-153">When a class declares a method with a paramarray parameter, it is not uncommon to also include some of the expanded forms as regular methods.</span></span> <span data-ttu-id="986af-154">-Instanz, die bei der erweiterten Zustand einer Methode mit einem Paramarray-Parameter tritt auf, wird aufgerufen, wodurch es möglich ist, vermeiden Sie die Zuordnung eines Arrays.</span><span class="sxs-lookup"><span data-stu-id="986af-154">By doing so it is possible to avoid the allocation of an array instance that occurs when an expanded form of a method with a paramarray parameter is invoked.</span></span>

    72. <span data-ttu-id="986af-155">Wenn `M` wird definiert, in einem stärker abgeleiteten Typ als `N`, beseitigen `N` aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="986af-155">If `M` is defined in a more derived type than `N`, eliminate `N` from the set.</span></span> <span data-ttu-id="986af-156">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="986af-156">For example:</span></span>

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

        <span data-ttu-id="986af-157">Diese Regel gilt auch, Typen, denen für die Erweiterungsmethoden definiert sind.</span><span class="sxs-lookup"><span data-stu-id="986af-157">This rule also applies to the types that extension methods are defined on.</span></span> <span data-ttu-id="986af-158">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="986af-158">For example:</span></span>

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

    73. <span data-ttu-id="986af-159">Wenn `M` und `N` sind Erweiterungsmethoden und der Zieltyp des `M` ist eine Klasse oder Struktur und der Zieltyp des `N` ist eine Schnittstelle, beseitigen `N` aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="986af-159">If `M` and `N` are extension methods and the target type of `M` is a class or structure and the target type of `N` is an interface, eliminate `N` from the set.</span></span> <span data-ttu-id="986af-160">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="986af-160">For example:</span></span>

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

    74. <span data-ttu-id="986af-161">Wenn `M` und `N` sind Erweiterungsmethoden und der Zieltyp des `M` und `N` identisch sind, nach der Ersetzung von Typparametern und der Zieltyp des `M` vor Typ parameterersetzung enthält nicht Geben Sie Parameter, aber der Zieltyp des `N` ist, und verfügt über weniger Typparameter als der Zieltyp des `N`, beseitigen `N` aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="986af-161">If `M` and `N` are extension methods, and the target type of `M` and `N` are identical after type parameter substitution, and the target type of `M` before type parameter substitution does not contain type parameters but the target type of `N` does, then has fewer type parameters than the target type of `N`, eliminate `N` from the set.</span></span> <span data-ttu-id="986af-162">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="986af-162">For example:</span></span>

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

    75. <span data-ttu-id="986af-163">Vor dem Typ Argumente haben wurde ersetzt, wenn `M` ist *weniger generische* (Abschnitt [Generizität](overload-resolution.md#genericity)) als `N`, beseitigen `N` aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="986af-163">Before type arguments have been substituted, if `M` is *less generic* (Section [Genericity](overload-resolution.md#genericity)) than `N`, eliminate `N` from the set.</span></span>

    76. <span data-ttu-id="986af-164">Wenn `M` ist keine Erweiterungsmethode und `N` ist, beseitigen `N` aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="986af-164">If `M` is not an extension method and `N` is, eliminate `N` from the set.</span></span>

    77. <span data-ttu-id="986af-165">Wenn `M` und `N` sind Erweiterungsmethoden und `M` wurde gefunden, bevor Sie `N` (Abschnitt [-Methode der Erweiterungsauflistung](overload-resolution.md#extension-method-collection)), zu beseitigen `N` aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="986af-165">If `M` and `N` are extension methods and `M` was found before `N` (Section [Extension Method Collection](overload-resolution.md#extension-method-collection)), eliminate `N` from the set.</span></span> <span data-ttu-id="986af-166">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="986af-166">For example:</span></span>

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

        <span data-ttu-id="986af-167">Wenn die Erweiterungsmethoden in demselben Schritt gefunden wurden, sind diese Erweiterungsmethoden nicht eindeutig.</span><span class="sxs-lookup"><span data-stu-id="986af-167">If the extension methods were found in the same step, then those extension methods are ambiguous.</span></span> <span data-ttu-id="986af-168">Der Aufruf kann immer eindeutig gemacht werden, mit dem Namen des standard-Moduls, enthält die Erweiterungsmethode und rufen Sie die Erweiterungsmethode, als wäre es ein reguläres Element.</span><span class="sxs-lookup"><span data-stu-id="986af-168">The call may always be disambiguated using the name of the standard module containing the extension method and calling the extension method as if it was a regular member.</span></span> <span data-ttu-id="986af-169">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="986af-169">For example:</span></span>

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

    78. <span data-ttu-id="986af-170">Wenn `M` und `N` sowohl der Typrückschluss zum Erzeugen von Typargumenten, erforderlich und `M` war nicht erforderlich, Bestimmen des bestimmenden Typs eines seiner Typargumente an (d. h. jeweils die Typargumente für einen einzelnen abgeleitet), aber `N`, beseitigen `N` aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="986af-170">If `M` and `N` both required type inference to produce type arguments, and `M` did not require determining the dominant type for any of its type arguments (i.e. each the type arguments inferred to a single type), but `N` did, eliminate `N` from the set.</span></span>

        <span data-ttu-id="986af-171">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="986af-171">__Note.__</span></span> <span data-ttu-id="986af-172">Diese Regel stellt sicher, Auflösung von funktionsüberladungen, die in früheren Versionen war erfolgreich (wobei das Ableiten von mehreren Typen für ein Typargument einen Fehler verursachen würden), werden weiterhin die gleichen Ergebnisse zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="986af-172">This rule ensures that overload resolution that succeeded in previous versions (where inferring multiple types for a type argument would cause an error), continue to produce the same results.</span></span>

    79. <span data-ttu-id="986af-173">Wenn das Ziel eines eines Ausdrucks von der Behebung überladungsauflösung erfolgt eine `AddressOf` Ausdruck und sowohl der Delegat, und `M` sind Funktionen, die beim `N` ist eine Unterroutine namens beseitigen `N` aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="986af-173">If overload resolution is being done to resolve the target of a delegate-creation expression from an `AddressOf` expression, and both the delegate and `M` are functions while `N` is a subroutine, eliminate `N` from the set.</span></span> <span data-ttu-id="986af-174">Ebenso, wenn sowohl der Delegat, und `M` -Unterroutinen, sind während `N` ist eine Funktion, beseitigen `N` aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="986af-174">Likewise, if both the delegate and `M` are subroutines, while `N` is a function, eliminate `N` from the set.</span></span>

    710. <span data-ttu-id="986af-175">Wenn `M` wurde keine Standardwerte optionaler Parameter anstelle der expliziten Argumente verwendet, aber `N` , beseitigen Sie `N` aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="986af-175">If `M` did not use any optional parameter defaults in place of explicit arguments, but `N` did, then eliminate `N` from the set.</span></span>

    711. <span data-ttu-id="986af-176">Vor dem Typ Argumente haben wurde ersetzt, wenn `M` hat *detaillierter der generizität* (Abschnitt [Generizität](overload-resolution.md#genericity)) als `N`, beseitigen Sie `N` aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="986af-176">Before type arguments have been substituted, if `M` has *greater depth of genericity* (Section [Genericity](overload-resolution.md#genericity)) than `N`, then eliminate `N` from the set.</span></span>

8. <span data-ttu-id="986af-177">Andernfalls der Aufruf ist mehrdeutig, und ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="986af-177">Otherwise, the call is ambiguous and a compile-time error occurs.</span></span>

#### <a name="specificity-of-memberstypes-given-an-argument-list"></a><span data-ttu-id="986af-178">Spezifischer Member/Typen, die eine Argumentliste</span><span class="sxs-lookup"><span data-stu-id="986af-178">Specificity of members/types given an argument list</span></span>

<span data-ttu-id="986af-179">Ein Member `M` gilt *gleichermaßen bestimmte* als `N`, eine Argumentliste `A`, wenn deren Signaturen identisch sind oder geben Sie jeden Parameter in `M` ist identisch mit den entsprechenden Parametertyp in `N`.</span><span class="sxs-lookup"><span data-stu-id="986af-179">A member `M` is considered *equally specific* as `N`, given an argument-list `A`, if their signatures are the same or if each parameter type in `M` is the same as the corresponding parameter type in `N`.</span></span>

<span data-ttu-id="986af-180">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="986af-180">__Note.__</span></span> <span data-ttu-id="986af-181">Zwei Member können in Methodengruppe mit der gleichen Signatur aufgrund von Erweiterungsmethoden enden.</span><span class="sxs-lookup"><span data-stu-id="986af-181">Two members can end up in a method group with the same signature due to extension methods.</span></span> <span data-ttu-id="986af-182">Zwei Member können auch gleichermaßen spezifisch sein, jedoch nicht über die gleiche Signatur aufgrund von Typparameter oder Paramarray-Erweiterung.</span><span class="sxs-lookup"><span data-stu-id="986af-182">Two members can also be equally specific but not have the same signature due to type parameters or paramarray expansion.</span></span>

<span data-ttu-id="986af-183">Ein Member `M` gilt *spezifischere* als `N` Wenn ihre Signaturen unterscheiden, und geben Sie mindestens einen Parameter in `M` sind präziser als ein Parametertyp in `N`, und es wird kein Parametertyp in `N` sind präziser als ein Parametertyp in `M`.</span><span class="sxs-lookup"><span data-stu-id="986af-183">A member `M` is considered *more specific* than `N` if their signatures are different and at least one parameter type in `M` is more specific than a parameter type in `N`, and no parameter type in `N` is more specific than a parameter type in `M`.</span></span> <span data-ttu-id="986af-184">Erhalten ein Paar von Parametern `Mj` und `Nj` , die ein Argument entspricht `Aj`, den Typ des `Mj` gilt *spezifischere* als den Typ des `Nj` Wenn eines der folgenden Bedingungen erfüllt ist:</span><span class="sxs-lookup"><span data-stu-id="986af-184">Given a pair of parameters `Mj` and `Nj` that matches an argument `Aj`, the type of `Mj` is considered *more specific* than the type of `Nj` if one of the following conditions is true:</span></span>

* <span data-ttu-id="986af-185">Es existiert eine erweiternde Konvertierung vom Typ des `Mj` in den Typ `Nj`.</span><span class="sxs-lookup"><span data-stu-id="986af-185">There exists a widening conversion from the type of `Mj` to the type `Nj`.</span></span> <span data-ttu-id="986af-186">(__Beachten.__</span><span class="sxs-lookup"><span data-stu-id="986af-186">(__Note.__</span></span> <span data-ttu-id="986af-187">Da Parametertypen in diesem Fall ohne Berücksichtigung der tatsächlichen Arguments verglichen werden, wird die erweiternde Konvertierung von Konstante Ausdrücke in einen numerischen Datentyp, der in der Wert passt, nicht in diesem Fall berücksichtigt.)</span><span class="sxs-lookup"><span data-stu-id="986af-187">Because parameters types are being compared without regard to the actual argument in this case, the widening conversion from constant expressions to a numeric type the value fits into is not considered in this case.)</span></span>

* <span data-ttu-id="986af-188">`Aj` besteht aus dem Literal `0`, `Mj` ist ein numerischer Typ und `Nj` ist ein enumerierter Typ.</span><span class="sxs-lookup"><span data-stu-id="986af-188">`Aj` is the literal `0`, `Mj` is a numeric type and `Nj` is an enumerated type.</span></span> <span data-ttu-id="986af-189">(__Beachten.__</span><span class="sxs-lookup"><span data-stu-id="986af-189">(__Note.__</span></span> <span data-ttu-id="986af-190">Diese Regel ist erforderlich, da das Literal `0` auf einen beliebigen enumerierten Typ erweitert wird.</span><span class="sxs-lookup"><span data-stu-id="986af-190">This rule is necessary because the literal `0` widens to any enumerated type.</span></span> <span data-ttu-id="986af-191">Da ein enumerierter Typ, in dem zugrunde liegenden Typ erweitert wird, bedeutet dies, dass der überladungsauflösung für `0` wird standardmäßig vorziehen Enumerationstypen numerische Typen.</span><span class="sxs-lookup"><span data-stu-id="986af-191">Since an enumerated type widens to its underlying type, this means that overload resolution on `0` will, by default, prefer enumerated types over numeric types.</span></span> <span data-ttu-id="986af-192">Wir erhalten viel Feedback, dass dieses Verhalten nicht intuitiv ist.)</span><span class="sxs-lookup"><span data-stu-id="986af-192">We received a lot of feedback that this behavior was counterintuitive.)</span></span>

* <span data-ttu-id="986af-193">`Mj` und `Nj` sind sowohl numerischen Typen und `Mj` ist älter als `Nj` in der Liste `Byte`, `SByte`, `Short`, `UShort`, `Integer`, `UInteger`, `Long`, `ULong`, `Decimal`, `Single`, `Double`.</span><span class="sxs-lookup"><span data-stu-id="986af-193">`Mj` and `Nj` are both numeric types, and `Mj` comes earlier than `Nj` in the list `Byte`, `SByte`, `Short`, `UShort`, `Integer`, `UInteger`, `Long`, `ULong`, `Decimal`, `Single`, `Double`.</span></span> <span data-ttu-id="986af-194">(__Beachten.__</span><span class="sxs-lookup"><span data-stu-id="986af-194">(__Note.__</span></span> <span data-ttu-id="986af-195">Die Regel zu numerischen Typen ist hilfreich, da nur die numerischen Typen mit und ohne Vorzeichen einer bestimmten Größe haben einschränkende Konvertierungen zwischen ihnen.</span><span class="sxs-lookup"><span data-stu-id="986af-195">The rule about the numeric types is useful because the signed and unsigned numeric types of a particular size only have narrowing conversions between them.</span></span> <span data-ttu-id="986af-196">Die oben genannten Regel unterbricht die Verbindung zwischen den beiden Typen durch weitere "natürliche" numerischen Typs.</span><span class="sxs-lookup"><span data-stu-id="986af-196">The above rule breaks the tie between the two types in favor of the more "natural" numeric type.</span></span> <span data-ttu-id="986af-197">Dies ist besonders wichtig bei der überladungsauflösung für einen Typ ausführen, die in mit und ohne Vorzeichen numerischen Typ der einer bestimmten Größe, z. B. ein numerisches Literal erweitert wird, das sowohl geeignet ist.)</span><span class="sxs-lookup"><span data-stu-id="986af-197">This is particularly important when doing overload resolution on a type that widens to both the signed and unsigned numeric types of a particular size, for example, a numeric literal that fits into both.)</span></span>

* <span data-ttu-id="986af-198">`Mj` und `Nj` sind Delegaten, Funktionstypen und den Rückgabetyp der `Mj` spezifischer als der Rückgabetyp der `Nj` Wenn `Aj` wird als Lambda-Methode, klassifiziert und `Mj` oder `Nj` ist `System.Linq.Expressions.Expression(Of T)`, klicken Sie dann Das Typargument des Typs (vorausgesetzt, dass es sich um einen Delegattyp handelt) wird für den verglichenen Typ ersetzt.</span><span class="sxs-lookup"><span data-stu-id="986af-198">`Mj` and `Nj` are delegate function types and the return type of `Mj` is more specific than the return type of `Nj` If `Aj` is classified as a lambda method, and `Mj` or `Nj` is `System.Linq.Expressions.Expression(Of T)`, then the type argument of the type (assuming it is a delegate type) is substituted for the type being compared.</span></span>

* <span data-ttu-id="986af-199">`Mj` ist identisch mit den Typ des `Aj`, und `Nj` nicht.</span><span class="sxs-lookup"><span data-stu-id="986af-199">`Mj` is identical to the type of `Aj`, and `Nj` is not.</span></span> <span data-ttu-id="986af-200">(__Beachten.__</span><span class="sxs-lookup"><span data-stu-id="986af-200">(__Note.__</span></span> <span data-ttu-id="986af-201">Es ist interessant, beachten Sie, dass die vorherige Regel unterscheidet sich geringfügig vom C# -Code in diesem C# -Code erfordert, dass es sich bei Delegattypen-Funktion verwenden dieselben Parameterlisten vor dem Vergleich von Rückgabetypen, Visual Basic jedoch nicht.)</span><span class="sxs-lookup"><span data-stu-id="986af-201">It is interesting to note that the previous rule differs slightly from C#, in that C# requires that the delegate function types have identical parameter lists before comparing return types, while Visual Basic does not.)</span></span>

#### <a name="genericity"></a><span data-ttu-id="986af-202">Generizität</span><span class="sxs-lookup"><span data-stu-id="986af-202">Genericity</span></span>

<span data-ttu-id="986af-203">Ein Member `M` wird als bestimmt *weniger generische* als Mitglied `N` wie folgt:</span><span class="sxs-lookup"><span data-stu-id="986af-203">A member `M` is determined to be *less generic* than a member `N` as follows:</span></span>

1. <span data-ttu-id="986af-204">IF, für jedes Paar übereinstimmender Parameter `Mj` und `Nj`, `Mj` ist kleiner als oder gleich als generische `Nj` in Bezug auf die Typparameter für die Methode, und mindestens eine `Mj` ist in Bezug auf Geben Sie kleiner generisch die Parameter der Methode.</span><span class="sxs-lookup"><span data-stu-id="986af-204">If, for each pair of matching parameters `Mj` and `Nj`, `Mj` is less or equally generic than `Nj` with respect to type parameters on the method, and at least one `Mj` is less generic with respect to type parameters on the method.</span></span>
2. <span data-ttu-id="986af-205">Andernfalls gilt: Wenn für jedes Paar übereinstimmender Parameter `Mj` und `Nj`, `Mj` ist kleiner als oder gleich als generische `Nj` in Bezug auf die Typparameter für den Typ und mindestens eine `Mj` ist in Bezug auf Geben Sie kleiner generisch Parameter für den Typ dann `M` ist kleiner als generisch `N`.</span><span class="sxs-lookup"><span data-stu-id="986af-205">Otherwise, if for each pair of matching parameters `Mj` and `Nj`, `Mj` is less or equally generic than `Nj` with respect to type parameters on the type, and at least one `Mj` is less generic with respect to type parameters on the type, then `M` is less generic than `N`.</span></span>

<span data-ttu-id="986af-206">Ein Parameter `M` gilt gleichermaßen für einen Parameter generisch `N` Wenn ihre Typen `Mt` und `Nt` beide Typparameter verweisen oder beide Typparameter verweisen nicht.</span><span class="sxs-lookup"><span data-stu-id="986af-206">A parameter `M` is considered to be equally generic to a parameter `N` if their types `Mt` and `Nt` both refer to type parameters or both don't refer to type parameters.</span></span> <span data-ttu-id="986af-207">`M` gilt als weniger generische `N` Wenn `Mt` verweist nicht auf einen Typparameter und `Nt` ist.</span><span class="sxs-lookup"><span data-stu-id="986af-207">`M` is considered to be less generic than `N` if `Mt` does not refer to a type parameter and `Nt` does.</span></span>

<span data-ttu-id="986af-208">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="986af-208">For example:</span></span>

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

<span data-ttu-id="986af-209">Typparameter Erweiterung-Methode an, die während der currying behoben wurden, werden als Typparameter den Typ, der keine Typparameter der Methode betrachtet.</span><span class="sxs-lookup"><span data-stu-id="986af-209">Extension method type parameters that were fixed during currying are considered type parameters on the type, not type parameters on the method.</span></span> <span data-ttu-id="986af-210">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="986af-210">For example:</span></span>

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

#### <a name="depth-of-genericity"></a><span data-ttu-id="986af-211">Tiefe der generizität</span><span class="sxs-lookup"><span data-stu-id="986af-211">Depth of genericity</span></span>

<span data-ttu-id="986af-212">Ein Member `M` wird damit bestimmt *detaillierter der generizität* als Mitglied `N` If, für jedes Paar übereinstimmender Parameter `Mj` und `Nj`, `Mj` ist größer oder gleich *Tiefe der generizität* als `Nj`, und mindestens eine `Mj` wird detaillierter der generizität ist.</span><span class="sxs-lookup"><span data-stu-id="986af-212">A member `M` is determined to have *greater depth of genericity* than a member `N` if, for each pair of matching parameters  `Mj` and `Nj`, `Mj` has greater or equal *depth of genericity* than `Nj`, and at least one `Mj` is has greater depth of genericity.</span></span> <span data-ttu-id="986af-213">Tiefe der generizität ist wie folgt definiert:</span><span class="sxs-lookup"><span data-stu-id="986af-213">Depth of genericity is defined as follows:</span></span>

* <span data-ttu-id="986af-214">Etwas anderes als einen Typparameter verfügt über mehr Tiefe der generizität als einen Typparameter;</span><span class="sxs-lookup"><span data-stu-id="986af-214">Anything other than a type parameter has greater depth of genericity than a type parameter;</span></span>

* <span data-ttu-id="986af-215">Rekursiv, verfügt über ein konstruierter Typ detaillierter der generizität als eine andere konstruierten Typ (mit derselben Anzahl von Typargumenten) Wenn mindestens ein Typargument detaillierter der generizität und kein Argument vom Typ weniger Tiefe als der entsprechende Typ hat Argument in der anderen.</span><span class="sxs-lookup"><span data-stu-id="986af-215">Recursively, a constructed type has greater depth of genericity than another constructed type (with the same number of type arguments) if at least one type argument has greater depth of genericity and no type argument has less depth than the corresponding type argument in the other.</span></span>

* <span data-ttu-id="986af-216">Array-Typ hat detaillierter der generizität als einen anderen Arraytyp (mit derselben Anzahl von Dimensionen) aus, wenn der Elementtyp des ersten detaillierter der generizität als Typ des Elements der zweiten ist.</span><span class="sxs-lookup"><span data-stu-id="986af-216">An array type has greater depth of genericity than another array type (with the same number of dimensions) if the element type of the first has greater depth of genericity than the element type of the second.</span></span>

<span data-ttu-id="986af-217">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="986af-217">For example:</span></span>

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

### <a name="applicability-to-argument-list"></a><span data-ttu-id="986af-218">Anwendbarkeit auf Argumentliste</span><span class="sxs-lookup"><span data-stu-id="986af-218">Applicability To Argument List</span></span>

<span data-ttu-id="986af-219">Eine Methode ist *anwendbar* auf einen Satz von Typargumenten, positionelle Argumente und benannte Argumente, wenn die Methode aufgerufen werden kann, die Argumentlisten verwenden.</span><span class="sxs-lookup"><span data-stu-id="986af-219">A method is *applicable* to a set of type arguments, positional arguments, and named arguments if the method can be invoked using the argument lists.</span></span> <span data-ttu-id="986af-220">Das Argument, die Listen der-Parameter verglichen werden, sind wie folgt aufgeführt:</span><span class="sxs-lookup"><span data-stu-id="986af-220">The argument lists are matched against the parameter lists as follows:</span></span>

1. <span data-ttu-id="986af-221">Erstens entsprechen Sie jedes positionelle Argument in der Reihenfolge der Liste der Parameter der Methode.</span><span class="sxs-lookup"><span data-stu-id="986af-221">First, match each positional argument in order to the list of method parameters.</span></span> <span data-ttu-id="986af-222">Wenn weitere positionelle Argumente als Parameter vorhanden sind, und der letzte Parameter ist nicht mit einem Paramarray, ist die Methode nicht anwendbar.</span><span class="sxs-lookup"><span data-stu-id="986af-222">If there are more positional arguments than parameters and the last parameter is not a paramarray, the method is not applicable.</span></span> <span data-ttu-id="986af-223">Andernfalls wird der Paramarray-Parameter mit Parametern vom Typ Paramarray-Elements mit der Anzahl von positionellen Argumenten übereinstimmen erweitert.</span><span class="sxs-lookup"><span data-stu-id="986af-223">Otherwise, the paramarray parameter is expanded with parameters of the paramarray element type to match the number of positional arguments.</span></span> <span data-ttu-id="986af-224">Wenn ein positionelles Argument weggelassen wird, die in einem Paramarray wechseln würde, die Methode ist nicht anwendbar.</span><span class="sxs-lookup"><span data-stu-id="986af-224">If a positional argument is omitted that would go into a paramarray, the method is not applicable.</span></span>
2. <span data-ttu-id="986af-225">Als Nächstes entsprechen Sie jedes benanntes Argument auf einen Parameter mit dem angegebenen Namen.</span><span class="sxs-lookup"><span data-stu-id="986af-225">Next, match each named argument to a parameter with the given name.</span></span> <span data-ttu-id="986af-226">Wenn eines der benannten Argumente findet keine Übereinstimmung, einen Paramarray-Parameter entspricht oder ein Argument, das bereits mit einer anderen mit Feldern fester Breite oder benannten Argument abgeglichen entspricht, ist die Methode nicht anwendbar.</span><span class="sxs-lookup"><span data-stu-id="986af-226">If one of the named arguments fails to match, matches a paramarray parameter, or matches an argument already matched with another positional or named argument, the method is not applicable.</span></span>
3. <span data-ttu-id="986af-227">Wenn die Typargumente angegeben wurden, werden sie als Nächstes mit der Typparameterliste abgeglichen.</span><span class="sxs-lookup"><span data-stu-id="986af-227">Next, if type arguments have been specified, they are matched against the type parameter list .</span></span> <span data-ttu-id="986af-228">Wenn die beiden Listen nicht die gleiche Anzahl von Elementen verfügen, ist die Methode nicht anwendbar, wenn die Liste der Typargumente leer ist.</span><span class="sxs-lookup"><span data-stu-id="986af-228">If the two lists do not have the same number of elements, the method is not applicable, unless the type argument list is empty.</span></span> <span data-ttu-id="986af-229">Wenn die Liste der Typargumente leer ist, wird Typrückschluss verwendet, um zu testen und die Liste der Typargumente abzuleiten.</span><span class="sxs-lookup"><span data-stu-id="986af-229">If the type argument list is empty, type inference is used to try and infer the type argument list.</span></span> <span data-ttu-id="986af-230">Wenn der Typrückschluss schlägt fehl, ist die Methode nicht anwendbar.</span><span class="sxs-lookup"><span data-stu-id="986af-230">If type inferencing fails, the method is not applicable.</span></span> <span data-ttu-id="986af-231">Andernfalls werden die Typargumente anstelle der Typparameter in der Signatur gefüllt. Wenn der Parameter, die nicht abgeglichen wurden nicht optional sind, ist die Methode nicht anwendbar.</span><span class="sxs-lookup"><span data-stu-id="986af-231">Otherwise, the type arguments are filled in the place of the type parameters in the signature.If parameters that have not been matched are not optional, the method is not applicable.</span></span>
4. <span data-ttu-id="986af-232">Wenn die Argumentausdrücke nicht implizit in den Typen der Parameter sind sie übereinstimmen, und klicken Sie dann die Methode ist nicht anwendbar.</span><span class="sxs-lookup"><span data-stu-id="986af-232">If the argument expressions are not implicitly convertible to the types of the parameters they match, then the method is not applicable.</span></span>
5. <span data-ttu-id="986af-233">Wenn ein Parameter ByRef ist und keine implizite Konvertierung vom Typ des Parameters in den Typ des Arguments steht, ist die Methode nicht anwendbar.</span><span class="sxs-lookup"><span data-stu-id="986af-233">If a parameter is ByRef, and there is not an implicit conversion from the type of the parameter to the type of the argument, then the method is not applicable.</span></span>
6. <span data-ttu-id="986af-234">Wenn die Typargumente (einschließlich der abgeleiteten Typargumenten aus Schritt 3) der Methode-Einschränkungen verletzen, ist die Methode nicht anwendbar.</span><span class="sxs-lookup"><span data-stu-id="986af-234">If type arguments violate the method's constraints (including the inferred type arguments from Step 3), the method is not applicable.</span></span> <span data-ttu-id="986af-235">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="986af-235">For example:</span></span>

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

<span data-ttu-id="986af-236">Wenn ein einzelnes Argumentausdruck mit einen Paramarray-Parameter übereinstimmt, und der Typ des Argumentausdrucks sowohl den Typ des Paramarray-Parameter und der Elementtyp Paramarray konvertierbar ist, gilt die Methode in der erweiterten und nicht erweiterten Formen, mit zwei Ausnahmen.</span><span class="sxs-lookup"><span data-stu-id="986af-236">If a single argument expression matches a paramarray parameter and the type of the argument expression is convertible to both the type of the paramarray parameter and the paramarray element type, the method is applicable in both its expanded and unexpanded forms, with two exceptions.</span></span> <span data-ttu-id="986af-237">Wenn die Konvertierung vom Typ des Argumentausdrucks in den ParamArraytyp einschränkend ist, ist die Methode nur anwendbar, in erweiterter Form.</span><span class="sxs-lookup"><span data-stu-id="986af-237">If the conversion from the type of the argument expression to the paramarray type is narrowing, then the method is only applicable in its expanded form.</span></span> <span data-ttu-id="986af-238">Expression-Arguments überschreitet `Nothing`, und klicken Sie dann die Methode nur anwendbar, in der nicht erweiterte Form ist.</span><span class="sxs-lookup"><span data-stu-id="986af-238">If the argument expression is the literal `Nothing`, then the method is only applicable in its unexpanded form.</span></span> <span data-ttu-id="986af-239">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="986af-239">For example:</span></span>

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

<span data-ttu-id="986af-240">Das obige Beispiel erzeugt die folgende Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="986af-240">The above example produces the following output:</span></span>

```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

<span data-ttu-id="986af-241">In der ersten und letzten Aufrufen von `F`, die normale Form dar `F` ist anwendbar, da eine erweiternde Konvertierung des Argumenttyps in den Parametertyp vorhanden ist (beide sind vom Typ `Object()`), und das Argument wird als regulärer Wert übergeben. der Parameter.</span><span class="sxs-lookup"><span data-stu-id="986af-241">In the first and last invocations of `F`, the normal form of `F` is applicable because a widening conversion exists from the argument type to the parameter type (both are of type `Object()`), and the argument is passed as a regular value parameter.</span></span> <span data-ttu-id="986af-242">In der zweiten und dritten aufrufen, die normale Form von `F` ist nicht anwendbar, da keine widening-Konvertierung des Argumenttyps in den Parametertyp vorhanden ist (Konvertierungen von `Object` zu `Object()` einschränkende sind).</span><span class="sxs-lookup"><span data-stu-id="986af-242">In the second and third invocations, the normal form of `F` is not applicable because no widening conversion exists from the argument type to the parameter type (conversions from `Object` to `Object()` are narrowing).</span></span> <span data-ttu-id="986af-243">Allerdings die erweiterte Form von `F` gilt, und eine von einem Element `Object()` wird durch den Aufruf erstellt.</span><span class="sxs-lookup"><span data-stu-id="986af-243">However, the expanded form of `F` is applicable, and a one-element `Object()` is created by the invocation.</span></span> <span data-ttu-id="986af-244">Das einzige Element des Arrays mit dem angegebenen Argumentwert initialisiert wird (die wiederum ist ein Verweis auf ein `Object()`).</span><span class="sxs-lookup"><span data-stu-id="986af-244">The single element of the array is initialized with the given argument value (which itself is a reference to an `Object()`).</span></span>

### <a name="passing-arguments-and-picking-arguments-for-optional-parameters"></a><span data-ttu-id="986af-245">Übergeben von Argumenten, und wählen die Argumente für optionale Parameter</span><span class="sxs-lookup"><span data-stu-id="986af-245">Passing Arguments, and Picking Arguments for Optional Parameters</span></span>

<span data-ttu-id="986af-246">Wenn ein Parameter einen Werteparameter ist, muss der entsprechende Argumentausdruck als Wert klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="986af-246">If a parameter is a value parameter, the matching argument expression must be classified as a value.</span></span> <span data-ttu-id="986af-247">Der Wert wird in den Typ des Parameters konvertiert und als Parameter zur Laufzeit übergeben.</span><span class="sxs-lookup"><span data-stu-id="986af-247">The value is converted to the type of the parameter and passed in as the parameter at run time.</span></span> <span data-ttu-id="986af-248">Wenn der Parameter ein Verweisparameter ist, und der entsprechenden Argumentausdruck wird als eine Variable, deren Typ der Parameter entspricht, klassifiziert, wird Sie dann ein Verweis auf die Variable in als Parameter zur Laufzeit übergeben.</span><span class="sxs-lookup"><span data-stu-id="986af-248">If the parameter is a reference parameter and the matching argument expression is classified as a variable whose type is the same as the parameter, then a reference to the variable is passed in as the parameter at run time.</span></span>

<span data-ttu-id="986af-249">Andernfalls, wenn der entsprechende Argumentausdruck als eine Variable, Wert oder der Zugriff auf Eigenschaften klassifiziert wird, wird eine temporäre Variable des Typs des Parameters zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="986af-249">Otherwise, if the matching argument expression is classified as a variable, value, or property access, then a temporary variable of the type of the parameter is allocated.</span></span> <span data-ttu-id="986af-250">Vor dem Aufruf der Methode zur Laufzeit wird der Argumentausdruck als Wert neu klassifiziert, in den Typ des Parameters konvertiert und der temporären Variablen zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="986af-250">Before the method invocation at run time, the argument expression is reclassified as a value, converted to the type of the parameter, and assigned to the temporary variable.</span></span> <span data-ttu-id="986af-251">Dann wird ein Verweis auf die temporäre Variable in als Parameter übergeben.</span><span class="sxs-lookup"><span data-stu-id="986af-251">Then a reference to the temporary variable is passed in as the parameter.</span></span> <span data-ttu-id="986af-252">Nach dem Aufruf der Methode ausgewertet wird, wenn der Argumentausdruck als eine Variable oder Eigenschaftszugriff klassifiziert wird, wird der Variablen Ausdruck oder der Eigenschaftsausdruck für den Zugriff der temporären Variablen zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="986af-252">After the method invocation is evaluated, if the argument expression is classified as a variable or property access, the temporary variable is assigned to the variable expression or the property access expression.</span></span> <span data-ttu-id="986af-253">Wenn der Eigenschaftsausdruck für den Zugriff kein `Set` -Accessor, und klicken Sie dann auf die Zuordnung wird nicht ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="986af-253">If the property access expression has no `Set` accessor, then the assignment is not performed.</span></span>

<span data-ttu-id="986af-254">Für optionale Parameter, in denen ein Argument nicht bereitgestellt wurde, wählt der Compiler Argumente an, wie unten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="986af-254">For optional parameters where an argument has not been provided, the compiler picks arguments as described below.</span></span> <span data-ttu-id="986af-255">In allen Fällen testet es nach dem Ersetzen des generischen Typs mit dem Parametertyp.</span><span class="sxs-lookup"><span data-stu-id="986af-255">In all cases it tests against the parameter type after generic type substitution.</span></span>

* <span data-ttu-id="986af-256">Wenn der optionale Parameter das Attribut hat `System.Runtime.CompilerServices.CallerLineNumber`, und der Aufruf ist von einem Ort im Quellcode und ein numerisches Literal, diesen Speicherort die Zeilennummer darstellt, wurde eine Konvertierung in den Parametertyp, dann wird die numerische Literale verwendet.</span><span class="sxs-lookup"><span data-stu-id="986af-256">If the optional parameter has the attribute `System.Runtime.CompilerServices.CallerLineNumber`, and the invocation is from a location in source code, and a numeric literal representing that location's line number has an intrinsic conversion to the parameter type, then the numeric literal is used.</span></span> <span data-ttu-id="986af-257">Wenn der Aufruf mehrere Zeilen erstreckt, ist die Auswahl der zu verwendende der Linie implementierungsabhängig.</span><span class="sxs-lookup"><span data-stu-id="986af-257">If the invocation spans multiple lines, then the choice of which line to use is implementation-dependent.</span></span>

* <span data-ttu-id="986af-258">Wenn der optionale Parameter das Attribut hat `System.Runtime.CompilerServices.CallerFilePath`, und der Aufruf ist von einem Ort im Quellcode, und ein Zeichenfolgenliteral, des Speicherorts Dateipfad darstellt, ist eine Konvertierung in den Parametertyp, dann wird das Zeichenfolgenliteral verwendet.</span><span class="sxs-lookup"><span data-stu-id="986af-258">If the optional parameter has the attribute `System.Runtime.CompilerServices.CallerFilePath`, and the invocation is from a location in source code, and a string literal representing that location's file path has an intrinsic conversion to the parameter type, then the string literal is used.</span></span> <span data-ttu-id="986af-259">Das Format des Dateipfads ist implementierungsabhängig.</span><span class="sxs-lookup"><span data-stu-id="986af-259">The format of the file path is implementation-dependent.</span></span>

* <span data-ttu-id="986af-260">Wenn der optionale Parameter das Attribut hat `System.Runtime.CompilerServices.CallerMemberName`, und der Aufruf ist innerhalb des Texts eines Typmembers oder in einem Attribut auf einen beliebigen Teil dieses Typmember angewendet und ein Zeichenfolgenliteral darstellt, Elementnamen hat eine Konvertierung für den Parameter Geben Sie ein, und klicken Sie dann das Zeichenfolgenliteral verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="986af-260">If the optional parameter has the attribute `System.Runtime.CompilerServices.CallerMemberName`, and the invocation is within the body of a type member or in an attribute applied to any part of that type member, and a string literal representing that member name has an intrinsic conversion to the parameter type, then the string literal is used.</span></span> <span data-ttu-id="986af-261">Für Aufrufe, die Teil von Eigenschaftenaccessoren oder benutzerdefinierten Ereignishandlern sind, ist der Elementname verwendet, die der Eigenschaft oder Ereignis selbst.</span><span class="sxs-lookup"><span data-stu-id="986af-261">For invocations that are part of property accessors or custom event handlers, then the member name used is that of the property or event itself.</span></span> <span data-ttu-id="986af-262">Aufrufe, die Teil eines Operator oder den Konstruktor, wird eine implementierungsspezifische Name verwendet.</span><span class="sxs-lookup"><span data-stu-id="986af-262">For invocations that are part of an operator or constructor, then an implementation-specific name is used.</span></span>

<span data-ttu-id="986af-263">Wenn keine der oben genannten Bedingungen zutrifft, klicken Sie dann den optionalen Parameter Standardwert wird verwendet (oder `Nothing` , wenn kein Standardwert angegeben ist).</span><span class="sxs-lookup"><span data-stu-id="986af-263">If none of the above apply, then the optional parameter's default value is used (or `Nothing` if no default value is supplied).</span></span> <span data-ttu-id="986af-264">Und wenn mehr als eine der oben genannten Bedingungen zutrifft, und klicken Sie dann die Wahl, welches ist implementierungsabhängig.</span><span class="sxs-lookup"><span data-stu-id="986af-264">And if more than one of the above apply, then the choice of which to use is implementation-dependent.</span></span>

<span data-ttu-id="986af-265">Die `CallerLineNumber` und `CallerFilePath` Attribute eignen sich für die Protokollierung.</span><span class="sxs-lookup"><span data-stu-id="986af-265">The `CallerLineNumber` and `CallerFilePath` attributes are useful for logging.</span></span> <span data-ttu-id="986af-266">Die `CallerMemberName` eignet sich für die Implementierung `INotifyPropertyChanged`.</span><span class="sxs-lookup"><span data-stu-id="986af-266">The `CallerMemberName` is useful for implementing `INotifyPropertyChanged`.</span></span> <span data-ttu-id="986af-267">Hier sind Beispiele.</span><span class="sxs-lookup"><span data-stu-id="986af-267">Here are examples.</span></span>

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

<span data-ttu-id="986af-268">Zusätzlich zu den oben aufgeführten optionalen Parameter erkennt Microsoft Visual Basic auch einige zusätzliche optionale Parameter, wenn sie aus den Metadaten (z. B. aus einem DLL-Verweis) importiert werden:</span><span class="sxs-lookup"><span data-stu-id="986af-268">In addition to the optional parameters above, Microsoft Visual Basic also recognizes some additional optional parameters if they are imported from metadata (i.e. from a DLL reference):</span></span>

* <span data-ttu-id="986af-269">Beim Importieren von Metadaten, die Parameter auch von Visual Basic behandelt `<Optional>` wie darauf hin, dass es sich bei der Parameter optional ist: auf diese Weise es möglich ist, eine Deklaration mit einem optionalen Parameter, aber kein Standardwert zugewiesen wurde, zu importieren, obwohl dies nicht möglich Mithilfe von ausgedrückt die `Optional` Schlüsselwort.</span><span class="sxs-lookup"><span data-stu-id="986af-269">Upon importing from metadata, Visual Basic also treats the parameter `<Optional>` as indicative that the parameter is optional: in this way it is possible to import a declaration which has an optional parameter but no default value, even though this can't be expressed using the `Optional` keyword.</span></span>

* <span data-ttu-id="986af-270">Wenn der optionale Parameter das Attribut hat `Microsoft.VisualBasic.CompilerServices.OptionCompareAttribute`, die numerische Literale 1 oder 0 ist eine Konvertierung in den Parametertyp, und der Compiler verwendet als Argument entweder dem literalen 1, wenn `Option Compare Text` gültig ist, oder 0, wenn das literal ist `Optional Compare Binary` ist aktiviert.</span><span class="sxs-lookup"><span data-stu-id="986af-270">If the optional parameter has the attribute `Microsoft.VisualBasic.CompilerServices.OptionCompareAttribute`, and the numeric literal 1 or 0 has a conversion to the parameter type, then the compiler uses as argument either the literal 1 if `Option Compare Text` is in effect, or the literal 0 if `Optional Compare Binary` is in effect.</span></span>

* <span data-ttu-id="986af-271">Wenn der optionale Parameter das Attribut hat `System.Runtime.CompilerServices.IDispatchConstantAttribute`, und es weist den Typ `Object`, es gibt keinen Standardwert, und der Compiler verwendet das Argument `New System.Runtime.InteropServices.DispatchWrapper(Nothing)`.</span><span class="sxs-lookup"><span data-stu-id="986af-271">If the optional parameter has the attribute `System.Runtime.CompilerServices.IDispatchConstantAttribute`, and it has type `Object`, and it does not specify a default value, then the compiler uses the argument `New System.Runtime.InteropServices.DispatchWrapper(Nothing)`.</span></span>

* <span data-ttu-id="986af-272">Wenn der optionale Parameter das Attribut hat `System.Runtime.CompilerServices.IUnknownConstantAttribute`, und es weist den Typ `Object`, es gibt keinen Standardwert, und der Compiler verwendet das Argument `New System.Runtime.InteropServices.UnknownWrapper(Nothing)`.</span><span class="sxs-lookup"><span data-stu-id="986af-272">If the optional parameter has the attribute `System.Runtime.CompilerServices.IUnknownConstantAttribute`, and it has type `Object`, and it does not specify a default value, then the compiler uses the argument `New System.Runtime.InteropServices.UnknownWrapper(Nothing)`.</span></span>

* <span data-ttu-id="986af-273">Wenn der optionale Parameter Typ hat `Object`, es gibt keinen Standardwert, und der Compiler verwendet das Argument `System.Reflection.Missing.Value`.</span><span class="sxs-lookup"><span data-stu-id="986af-273">If the optional parameter has type `Object`, and it does not specify a default value, then the compiler uses the argument `System.Reflection.Missing.Value`.</span></span>

### <a name="conditional-methods"></a><span data-ttu-id="986af-274">Bedingte Methoden</span><span class="sxs-lookup"><span data-stu-id="986af-274">Conditional Methods</span></span>

<span data-ttu-id="986af-275">Wenn die Zielmethode, die auf den ein Aufrufausdruck verweist eine Unterroutine ist, der kein Member einer Schnittstelle ist, und wenn die Methode eine oder mehrere `System.Diagnostics.ConditionalAttribute` Attribute Auswertung des Ausdrucks hängt von den definiert Konstanten für bedingte Kompilierung Dieser Punkt in der Quelldatei.</span><span class="sxs-lookup"><span data-stu-id="986af-275">If the target method to which an invocation expression refers is a subroutine that is not a member of an interface and if the method has one or more `System.Diagnostics.ConditionalAttribute` attributes, evaluation of the expression depends on the conditional compilation constants defined at that point in the source file.</span></span> <span data-ttu-id="986af-276">Jede Instanz des Attributs gibt eine Zeichenfolge, die eine Konstante für bedingte Kompilierung Namen an.</span><span class="sxs-lookup"><span data-stu-id="986af-276">Each instance of the attribute specifies a string, which names a conditional compilation constant.</span></span> <span data-ttu-id="986af-277">Jede Konstante für bedingte Kompilierung wird ausgewertet, als handele es sich um Teil einer bedingten kompilierungsanweisung.</span><span class="sxs-lookup"><span data-stu-id="986af-277">Each conditional compilation constant is evaluated as if it were part of a conditional compilation statement.</span></span> <span data-ttu-id="986af-278">Wenn die Konstante ergibt `True`, der Ausdruck wird normalerweise zur Laufzeit ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="986af-278">If the constant evaluates to `True`, the expression is evaluated normally at run time.</span></span> <span data-ttu-id="986af-279">Wenn die Konstante ergibt `False`, der Ausdruck ist überhaupt nicht ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="986af-279">If the constant evaluates to `False`, the expression is not evaluated at all.</span></span>

<span data-ttu-id="986af-280">Wenn für das Attribut zu suchen, ist die am stärksten abgeleitete Deklaration eine überschreibbare Methode aktiviert.</span><span class="sxs-lookup"><span data-stu-id="986af-280">When looking for the attribute, the most derived declaration of an overridable method is checked.</span></span>

<span data-ttu-id="986af-281">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="986af-281">__Note.__</span></span> <span data-ttu-id="986af-282">Das Attribut gilt nicht für Funktionen oder Schnittstellenmethoden und wird ignoriert, wenn für beide Arten der Methode angegeben.</span><span class="sxs-lookup"><span data-stu-id="986af-282">The attribute is not valid on functions or interface methods and is ignored if specified on either kind of method.</span></span> <span data-ttu-id="986af-283">Bedingte Methoden werden daher nur in Aufruf Anweisungen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="986af-283">Thus, conditional methods will only appear in invocation statements.</span></span>

### <a name="type-argument-inference"></a><span data-ttu-id="986af-284">Typrückschluss-Argument</span><span class="sxs-lookup"><span data-stu-id="986af-284">Type Argument Inference</span></span>

<span data-ttu-id="986af-285">Wenn eine Methode mit Parametern aufgerufen wird, ohne Angabe von Typargumenten, *Argument Typrückschluss* dient zum Testen und Typargumente für den Aufruf ableiten.</span><span class="sxs-lookup"><span data-stu-id="986af-285">When a method with type parameters is called without specifying type arguments, *type argument inference* is used to try and infer type arguments for the call.</span></span> <span data-ttu-id="986af-286">Dies ermöglicht eine natürlichere Syntax zum Aufrufen einer Methode mit den beiden Typparametern, wenn die Typargumente Trivial abgeleitet werden können verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="986af-286">This allows a more natural syntax to be used for calling a method with type parameters when the type arguments can be trivially inferred.</span></span> <span data-ttu-id="986af-287">Betrachten Sie z. B. die folgende Methodendeklaration:</span><span class="sxs-lookup"><span data-stu-id="986af-287">For example, given the following method declaration:</span></span>

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

<span data-ttu-id="986af-288">Es ist möglich, rufen Sie die `Choose` Methode ohne explizite Angabe ein Argument vom Typ:</span><span class="sxs-lookup"><span data-stu-id="986af-288">it is possible to invoke the `Choose` method without explicitly specifying a type argument:</span></span>

```vb
' calls Choose(Of Integer)
Dim i As Integer = Util.Choose(True, 5, 213)
' calls Choose(Of String)
Dim s As String = Util.Choose(False, "a", "b") 
```

<span data-ttu-id="986af-289">Mithilfe des Typrückschlusses Argument die Typargumente `Integer` und `String` werden aus den Argumenten der Methode bestimmt.</span><span class="sxs-lookup"><span data-stu-id="986af-289">Through type argument inference, the type arguments `Integer` and `String` are determined from the arguments to the method.</span></span>

<span data-ttu-id="986af-290">Typargument Typrückschluss erfolgt, *vor* neuklassifizierung der Ausdruck für Lambdamethoden oder Methodenzeiger in der Argumentliste ausgeführt wird, da eine neuklassifizierung der diese zwei Arten von Ausdrücken den Typ des erfordern möglicherweise die der Parameter nicht bekannt sein.</span><span class="sxs-lookup"><span data-stu-id="986af-290">Type argument inference occurs *before* expression reclassification is performed on lambda methods or method pointers in the argument list, since reclassification of those two kinds of expressions may require the type of the parameter to be known.</span></span>  <span data-ttu-id="986af-291">Bei einem gegebenen Satz von Argumenten `A1,...,An`, eine Reihe übereinstimmender Parameter `P1,...,Pn` und einen Satz von der Methode, die Typparameter `T1,...,Tn`, werden die Abhängigkeiten zwischen den Argumenten und die Typparameter der Methode zunächst wie folgt erfasst:</span><span class="sxs-lookup"><span data-stu-id="986af-291">Given a set of arguments `A1,...,An`, a set of matching parameters `P1,...,Pn` and a set of method type parameters `T1,...,Tn`, the dependencies between the arguments and method type parameters are first collected as follows:</span></span>

* <span data-ttu-id="986af-292">Wenn `An` ist die `Nothing` Zeichenliteral ohne Abhängigkeiten generiert werden.</span><span class="sxs-lookup"><span data-stu-id="986af-292">If `An` is the `Nothing` literal, no dependencies are generated.</span></span>

* <span data-ttu-id="986af-293">Wenn `An` ist ein Lambda-Methode und der Typ des `Pn` ist ein Typ des erstellten Delegaten oder `System.Linq.Expressions.Expression(Of T)`, wobei `T` ist ein Typ des erstellten Delegaten,</span><span class="sxs-lookup"><span data-stu-id="986af-293">If `An` is a lambda method and the type of `Pn` is a constructed delegate type or `System.Linq.Expressions.Expression(Of T)`, where `T` is a constructed delegate type,</span></span>

* <span data-ttu-id="986af-294">Wenn Sie der Typ der Parameter der Lambda-Methode vom Typ des entsprechenden Parameters abgeleitet werden, werden `Pn`, und der Typ des Parameters hängt von Typparameter einer Methode `Tn`, klicken Sie dann `An` weist eine Abhängigkeit `Tn`.</span><span class="sxs-lookup"><span data-stu-id="986af-294">If the type of a lambda method parameter will be inferred from the type of the corresponding parameter `Pn`, and the type of the parameter depends on a method type parameter `Tn`, then `An` has a dependency on `Tn`.</span></span>

* <span data-ttu-id="986af-295">Wenn der Typ des Lambda-Parameter der Methode angegeben ist und der Typ des entsprechenden Parameters `Pn` hängt von der Typparameter einer Methode `Tn`, klicken Sie dann `Tn` weist eine Abhängigkeit `An`.</span><span class="sxs-lookup"><span data-stu-id="986af-295">If the type of a lambda method parameter is specified and the type of the corresponding parameter `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="986af-296">Wenn der Rückgabetyp der `Pn` hängt von der Typparameter einer Methode `Tn`, klicken Sie dann `Tn` weist eine Abhängigkeit `An`.</span><span class="sxs-lookup"><span data-stu-id="986af-296">If the return type of `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="986af-297">Wenn `An` ist ein Methodenzeiger und der Typ des `Pn` ist ein Typ des erstellten Delegaten,</span><span class="sxs-lookup"><span data-stu-id="986af-297">If `An` is a method pointer and the type of `Pn` is a constructed delegate type,</span></span>

* <span data-ttu-id="986af-298">Wenn der Rückgabetyp der `Pn` hängt von der Typparameter einer Methode `Tn`, klicken Sie dann `Tn` weist eine Abhängigkeit `An`.</span><span class="sxs-lookup"><span data-stu-id="986af-298">If the return type of `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="986af-299">Wenn `Pn` ist ein konstruierter Typ und den Typ des `Pn` hängt von der Typparameter einer Methode `Tn`, klicken Sie dann `Tn` weist eine Abhängigkeit `An`.</span><span class="sxs-lookup"><span data-stu-id="986af-299">If `Pn` is a constructed type and the type of `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="986af-300">Andernfalls wird keine Abhängigkeit generiert.</span><span class="sxs-lookup"><span data-stu-id="986af-300">Otherwise, no dependency is generated.</span></span>

<span data-ttu-id="986af-301">Nach dem Sammeln von Abhängigkeiten, werden alle Argumente, die keine Abhängigkeiten haben, entfernt.</span><span class="sxs-lookup"><span data-stu-id="986af-301">After collecting dependencies, any arguments that have no dependencies are eliminated.</span></span> <span data-ttu-id="986af-302">Wenn alle Typparameter der Methode keine ausgehenden Abhängigkeiten haben (d. h. die Typparameter der Methode ein Argument hängt nicht), schlägt ein Typrückschluss.</span><span class="sxs-lookup"><span data-stu-id="986af-302">If any method type parameters have no outgoing dependencies (i.e. the method type parameter does not depend on an argument), then type inference fails.</span></span> <span data-ttu-id="986af-303">Andernfalls werden die übrigen Argumente und die Typparameter der Methode zu stark angeschlossenen Komponenten gruppiert.</span><span class="sxs-lookup"><span data-stu-id="986af-303">Otherwise, the remaining arguments and method type parameters are grouped into strongly connected components.</span></span> <span data-ttu-id="986af-304">Eine eng verbundene Komponente ist einen Satz von Typargumenten und Typparametern der Methode, wobei jedes Element in der Komponente über Abhängigkeiten von anderen Elementen.</span><span class="sxs-lookup"><span data-stu-id="986af-304">A strongly connected component is a set of arguments and method type parameters, where any element in the component is reachable via dependencies on other elements.</span></span>

<span data-ttu-id="986af-305">Stark angeschlossenen Komponenten dann topologisch sortiert und in topologischer Reihenfolge verarbeitet:</span><span class="sxs-lookup"><span data-stu-id="986af-305">The strongly connected components are then topologically sorted and processed in topological order:</span></span>

* <span data-ttu-id="986af-306">Wenn die stark typisierte Komponente nur ein einziges Element enthält,</span><span class="sxs-lookup"><span data-stu-id="986af-306">If the strongly typed component contains only one element,</span></span>

  * <span data-ttu-id="986af-307">Wenn das Element bereits abgeschlossen markiert wurde, fahren Sie es.</span><span class="sxs-lookup"><span data-stu-id="986af-307">If the element has already been marked complete, skip it.</span></span>

  * <span data-ttu-id="986af-308">Wenn das Element ein Argument ist, fügen Sie TypHinweise aus dem Argument an die Methode Typparameter, die von ihm abhängig und das Element als abgeschlossen zu markieren.</span><span class="sxs-lookup"><span data-stu-id="986af-308">If the element is an argument, then add type hints from the argument to the method type parameters that depend on it and mark the element as complete.</span></span> <span data-ttu-id="986af-309">Wenn das Argument eine Lambda-Methode mit Parametern, die dennoch müssen abgeleitete Typen, anschließende folgern `Object` für die Typen dieser Parameter.</span><span class="sxs-lookup"><span data-stu-id="986af-309">If the argument is a lambda method with parameters that still need inferred types, then infer `Object` for the types of those parameters.</span></span>

  * <span data-ttu-id="986af-310">Wenn das Element die Typparameter einer Methode ist, klicken Sie dann die Typparameter der Methode der bestimmende Typ für das Argument TypHinweise und das Element als abgeschlossen markieren abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="986af-310">If the element is a method type parameter, then infer the method type parameter to be the dominant type among the argument type hints and mark the element as complete.</span></span> <span data-ttu-id="986af-311">Wenn auf ein typhinweis eine Einschränkung der Array-Element ist, gelten nur die Konvertierungen, die zwischen Arrays mit den angegebenen Typ gültig sind (z. B. kovariante und systeminterne arraykonvertierungen).</span><span class="sxs-lookup"><span data-stu-id="986af-311">If a type hint has an array element restriction on it, then only conversions that are valid between arrays of the given type are considered (i.e. covariant and intrinsic array conversions).</span></span> <span data-ttu-id="986af-312">Wenn ein typhinweis eine generisches Argument-Einschränkung aufweist, werden nur die Identity-Konvertierungen berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="986af-312">If a type hint has a generic argument restriction on it, then only identity conversions are considered.</span></span> <span data-ttu-id="986af-313">Typrückschluss schlägt fehl, wenn kein dominanter Typ ausgewählt werden kann.</span><span class="sxs-lookup"><span data-stu-id="986af-313">If no dominant type can be chosen, inference fails.</span></span> <span data-ttu-id="986af-314">Wenn alle Argumenttypen für Lambda-Methode die Typparameter dieser Methode abhängig sind, wird der Typ der Lambda-Methode weitergegeben.</span><span class="sxs-lookup"><span data-stu-id="986af-314">If any lambda method argument types depend on this method type parameter, the type is propagated to the lambda method.</span></span>

* <span data-ttu-id="986af-315">Wenn die stark typisierte Komponente mehr als ein Element enthält, enthält die Komponente eine Schleife.</span><span class="sxs-lookup"><span data-stu-id="986af-315">If the strongly typed component contains more than one element, then the component contains a cycle.</span></span>

  * <span data-ttu-id="986af-316">Für jeden Typparameter der Methode, die ein Element in der Komponente ist, wenn der Typparameter der Methode ein Argument abhängig ist, die nicht als abgeschlossen markiert ist, konvertieren diese Abhängigkeit in eine Assertion, die am Ende des Rückschlussprozesses von geprüft wird.</span><span class="sxs-lookup"><span data-stu-id="986af-316">For each method type parameter that is an element in the component, if the method type parameter depends on an argument that is not marked complete, convert that dependency into an assertion that will be checked at the end of the inference process.</span></span>

  * <span data-ttu-id="986af-317">Starten Sie den Typrückschluss-Prozess, an dem Punkt, an dem die stark typisierten Komponenten ermittelt wurden.</span><span class="sxs-lookup"><span data-stu-id="986af-317">Restart the inference process at the point at which the strongly typed components were determined.</span></span>

<span data-ttu-id="986af-318">Wenn ein Typrückschluss für alle Typparameter der Methode erfolgreich ist, werden alle Abhängigkeiten, die in den Assertionen geändert wurden überprüft.</span><span class="sxs-lookup"><span data-stu-id="986af-318">If type inference succeeds for all of the method type parameters, then any dependencies that were changed into assertions are checked.</span></span> <span data-ttu-id="986af-319">Eine Assertion ist erfolgreich, wenn der Typ des Arguments implizit in den abgeleiteten Typ, der der Typparameter der Methode.</span><span class="sxs-lookup"><span data-stu-id="986af-319">An assertion succeeds if the type of the argument is implicitly convertible to the inferred type of the method type parameter.</span></span> <span data-ttu-id="986af-320">Wenn eine Assertion fehlschlägt, schlägt Typrückschluss-Argument.</span><span class="sxs-lookup"><span data-stu-id="986af-320">If an assertion fails, then type argument inference fails.</span></span>

<span data-ttu-id="986af-321">Mit einem Argument Typ `Ta` für ein Argument `A` und Parametertyp `Tp` für einen Parameter `P`, TypHinweise typgeprüft werden wie folgt generiert:</span><span class="sxs-lookup"><span data-stu-id="986af-321">Given an argument type `Ta` for an argument `A` and a parameter type `Tp` for a parameter `P`, type hints are generated as follows:</span></span>

* <span data-ttu-id="986af-322">Wenn `Tp` beinhaltet jedoch keine Methodenparameter des Typs keine Hinweise generiert werden.</span><span class="sxs-lookup"><span data-stu-id="986af-322">If `Tp` does not involve any method type parameters then no hints are generated.</span></span>

* <span data-ttu-id="986af-323">Wenn `Tp` und `Ta` werden Arraytypen, der den gleichen Rang, und Ersetzen `Ta` und `Tp` mit den Elementtypen der `Ta` und `Tp` und starten Sie diesen Prozess mit einer Array-Element-Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="986af-323">If `Tp` and `Ta` are array types of the same rank, then replace `Ta` and `Tp` with the element types of `Ta` and `Tp` and restart this process with an array element restriction.</span></span>

* <span data-ttu-id="986af-324">Wenn `Tp` ist der Typparameter einer Methode `Ta` als einen typhinweis, mit der aktuellen Einschränkung, hinzugefügt wird, sofern vorhanden.</span><span class="sxs-lookup"><span data-stu-id="986af-324">If `Tp` is a method type parameter, then `Ta` is added as a type hint with the current restriction, if any.</span></span>

* <span data-ttu-id="986af-325">Wenn `A` ist eine Lambda-Methode und `Tp` ist ein Typ des erstellten Delegaten oder `System.Linq.Expressions.Expression(Of T)`, wobei `T` ist ein erstellten Delegaten-Typ, für jeden Typ von Lambda-Methode Parameter `TL` und die entsprechenden Parameter Delegattyp`TD`, ersetzen Sie dies `Ta` mit `TL` und `Tp` mit `TD` und starten Sie den Prozess ohne Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="986af-325">If `A` is a lambda method and `Tp` is a constructed delegate type or `System.Linq.Expressions.Expression(Of T)`, where `T` is a constructed delegate type, for each lambda method parameter type `TL` and corresponding delegate parameter type `TD`, replace `Ta` with `TL` and `Tp` with `TD` and restart the process with no restriction.</span></span> <span data-ttu-id="986af-326">Ersetzen Sie `Ta` mit dem Rückgabetyp der Lambda-Methode und:</span><span class="sxs-lookup"><span data-stu-id="986af-326">Then, replace `Ta` with the return type of the lambda method and:</span></span>

  * <span data-ttu-id="986af-327">Wenn `A` ist eine reguläre Lambda-Methode ersetzen `Tp` mit dem Rückgabetyp des Delegattyps;</span><span class="sxs-lookup"><span data-stu-id="986af-327">if `A` is a regular lambda method, replace `Tp` with the return type of the delegate type;</span></span>
  * <span data-ttu-id="986af-328">Wenn `A` ist eine Async-Lambda-Methode und der Rückgabetyp des Delegattyps hat die Form `Task(Of T)` für einige `T`, ersetzen Sie dies `Tp` , `T`;</span><span class="sxs-lookup"><span data-stu-id="986af-328">if `A` is an async lambda method and the return type of the delegate type has form `Task(Of T)` for some `T`, replace `Tp` with that `T`;</span></span>
  * <span data-ttu-id="986af-329">Wenn `A` ist eine Lambda-Iterator-Methode und der Rückgabetyp des Delegattyps hat die Form `IEnumerator(Of T)` oder `IEnumerable(Of T)` für einige `T`, ersetzen Sie dies `Tp` , `T`.</span><span class="sxs-lookup"><span data-stu-id="986af-329">if `A` is an iterator lambda method and the return type of the delegate type has form `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`, replace `Tp` with that `T`.</span></span>
  * <span data-ttu-id="986af-330">Als Nächstes starten Sie den Prozess ohne Einschränkung an.</span><span class="sxs-lookup"><span data-stu-id="986af-330">Next, restart the process with no restriction.</span></span>

* <span data-ttu-id="986af-331">Wenn `A` ist ein Methodenzeiger und `Tp` eine erstellte Delegattyp, verwenden Sie die Parametertypen der `Tp` um zu bestimmen, welche Methode gezeigt ist die am ehesten auf `Tp`.</span><span class="sxs-lookup"><span data-stu-id="986af-331">If `A` is a method pointer and `Tp` is a constructed delegate type, use the parameter types of `Tp` to determine which method pointed is most applicable to `Tp`.</span></span> <span data-ttu-id="986af-332">Ersetzen Sie bei eine Methode, die am besten geeignete wird `Ta` mit dem Rückgabetyp der Methode und `Tp` mit dem Rückgabetyp des Delegattyps und Neustart des Prozesses ohne Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="986af-332">If there is a method that is most applicable, replace `Ta` with the return type of the method and `Tp` with the return type of the delegate type and restart the process with no restriction.</span></span>

* <span data-ttu-id="986af-333">Andernfalls `Tp` muss ein konstruierter Typ sein.</span><span class="sxs-lookup"><span data-stu-id="986af-333">Otherwise, `Tp` must be a constructed type.</span></span> <span data-ttu-id="986af-334">Erhält `TG`, der generische Typ der `Tp`,</span><span class="sxs-lookup"><span data-stu-id="986af-334">Given `TG`, the generic type of `Tp`,</span></span>

  * <span data-ttu-id="986af-335">Wenn `Ta` ist `TG`, erbt von `TG`, oder den Typ implementiert `TG` genau einmal für jedes übereinstimmende Typargument dann `Tax` aus `Ta` und `Tpx` aus `Tp`, Ersetzen`Ta` mit `Tax` und `Tp` mit `Tpx` und starten Sie den Prozess mit einem generischen Argument-Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="986af-335">If `Ta` is `TG`, inherits from `TG`, or implements the type `TG` exactly once, then for each matching type argument `Tax` from `Ta` and `Tpx` from `Tp`, replace `Ta` with `Tax` and `Tp` with `Tpx` and restart the process with a generic argument restriction.</span></span>

  * <span data-ttu-id="986af-336">Andernfalls schlägt den Typrückschluss, für die generische Methode.</span><span class="sxs-lookup"><span data-stu-id="986af-336">Otherwise, type inference fails for the generic method.</span></span>

<span data-ttu-id="986af-337">Der Erfolg des Typrückschlusses garantiert nicht, in und von sich selbst, dass die Methode gilt.</span><span class="sxs-lookup"><span data-stu-id="986af-337">The success of type inference does not, in and of itself, guarantee that the method is applicable.</span></span>
