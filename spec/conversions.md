---
ms.openlocfilehash: 7aef52145a71bff1d489772e81eb786a9dbd23d1
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306198"
---
# <a name="conversions"></a><span data-ttu-id="00c91-101">Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="00c91-101">Conversions</span></span>

<span data-ttu-id="00c91-102">Bei der Konvertierung wird ein Wert von einem Typ in einen anderen geändert.</span><span class="sxs-lookup"><span data-stu-id="00c91-102">Conversion is the process of changing a value from one type to another.</span></span> <span data-ttu-id="00c91-103">Beispielsweise kann ein Wert vom Typ "`Integer`" in einen Wert vom Typ "`Double`" konvertiert werden, oder es kann ein Wert vom Typ "`Derived`" in einen Wert vom Typ "`Base`" konvertiert werden. dabei wird davon ausgegangen, dass `Base` und `Derived` beide Klassen sind und `Derived` von `Base`erbt.</span><span class="sxs-lookup"><span data-stu-id="00c91-103">For example, a value of type `Integer` can be converted to a value of type `Double`, or a value of type `Derived` can be converted to a value of type `Base`, assuming that `Base` and `Derived` are both classes and `Derived` inherits from `Base`.</span></span> <span data-ttu-id="00c91-104">Bei Konvertierungen ist es möglicherweise nicht erforderlich, dass sich der Wert selbst ändert (wie im letzteren Beispiel), oder Sie erfordern möglicherweise bedeutende Änderungen in der Wert Darstellung (wie im vorherigen Beispiel).</span><span class="sxs-lookup"><span data-stu-id="00c91-104">Conversions may not require the value itself to change (as in the latter example), or they may require significant changes in the value representation (as in the former example).</span></span>

<span data-ttu-id="00c91-105">Konvertierungen können entweder erweitert oder einschränkend sein.</span><span class="sxs-lookup"><span data-stu-id="00c91-105">Conversions may either be widening or narrowing.</span></span> <span data-ttu-id="00c91-106">Eine *erweiternde Konvertierung* ist eine Konvertierung von einem Typ in einen anderen Typ, dessen Wert Domäne mindestens so groß ist, wenn er nicht größer ist als die Wert Domäne des ursprünglichen Typs.</span><span class="sxs-lookup"><span data-stu-id="00c91-106">A *widening conversion* is a conversion from a type to another type whose value domain is at least as big, if not bigger, than the original type's value domain.</span></span> <span data-ttu-id="00c91-107">Erweiternde Konvertierungen sollten nie fehlschlagen.</span><span class="sxs-lookup"><span data-stu-id="00c91-107">Widening conversions should never fail.</span></span> <span data-ttu-id="00c91-108">Eine einschränkende *Konvertierung* ist eine Konvertierung von einem Typ in einen anderen Typ, dessen Wert Domäne entweder kleiner ist als die Wert Domäne des ursprünglichen Typs oder ausreichend ohne Beziehung, die bei der Konvertierung erforderlich ist (z. b. bei der Konvertierung von `Integer` in `String`).</span><span class="sxs-lookup"><span data-stu-id="00c91-108">A *narrowing conversion* is a conversion from a type to another type whose value domain is either smaller than the original type's value domain or sufficiently unrelated that extra care must be taken when doing the conversion (for example, when converting from `Integer` to `String`).</span></span> <span data-ttu-id="00c91-109">Einschränkende Konvertierungen, die einen Informationsverlust verursachen können, können fehlschlagen.</span><span class="sxs-lookup"><span data-stu-id="00c91-109">Narrowing conversions, which may entail loss of information, can fail.</span></span>

<span data-ttu-id="00c91-110">Die Identitäts Konvertierung (d. h. eine Konvertierung von einem Typ in sich selbst) und die Standardwert Konvertierung (d. h. eine Konvertierung von `Nothing`) werden für alle Typen definiert.</span><span class="sxs-lookup"><span data-stu-id="00c91-110">The identity conversion (i.e. a conversion from a type to itself) and default value conversion (i.e. a conversion from `Nothing`) are defined for all types.</span></span>

## <a name="implicit-and-explicit-conversions"></a><span data-ttu-id="00c91-111">Implizite und explizite Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="00c91-111">Implicit and Explicit Conversions</span></span>

<span data-ttu-id="00c91-112">Konvertierungen können entweder *implizit* oder *explizit*sein.</span><span class="sxs-lookup"><span data-stu-id="00c91-112">Conversions can be either *implicit* or *explicit*.</span></span> <span data-ttu-id="00c91-113">Implizite Konvertierungen treten ohne besondere Syntax auf.</span><span class="sxs-lookup"><span data-stu-id="00c91-113">Implicit conversions occur without any special syntax.</span></span> <span data-ttu-id="00c91-114">Im folgenden finden Sie ein Beispiel für die implizite Konvertierung eines `Integer` Werts in einen `Long` Wert:</span><span class="sxs-lookup"><span data-stu-id="00c91-114">The following is an example of implicit conversion of an `Integer` value to a `Long` value:</span></span>

```vb
Module Test
    Sub Main()
        Dim intValue As Integer = 123
        Dim longValue As Long = intValue

        Console.WriteLine(intValue & " = " & longValue)
    End Sub
End Module
```

Explizite Konvertierungen erfordern hingegen Umwandlungs Operatoren. Der Versuch, eine explizite Konvertierung für einen Wert ohne Cast Operator durchzuführen, verursacht einen Kompilierzeitfehler. <span data-ttu-id="00c91-117">Im folgenden Beispiel wird eine explizite Konvertierung verwendet, um einen `Long` Wert in einen `Integer`-Wert zu konvertieren.</span><span class="sxs-lookup"><span data-stu-id="00c91-117">The following example uses an explicit conversion to convert a `Long` value to an `Integer` value.</span></span>

```vb
Module Test
    Sub Main()
        Dim longValue As Long = 134
        Dim intValue As Integer = CInt(longValue)

        Console.WriteLine(longValue & " = " & intValue)
    End Sub
End Module
```

Der Satz impliziter Konvertierungen hängt von der Kompilierungs Umgebung und der `Option Strict`-Anweisung ab. Wenn eine strikte Semantik verwendet wird, können nur erweiternde Konvertierungen implizit auftreten. <span data-ttu-id="00c91-120">Wenn eine einschränkend sein Semantik verwendet wird, können alle Erweiterungs-und einschränkenden Konvertierungen (d. h. alle Konvertierungen) implizit auftreten.</span><span class="sxs-lookup"><span data-stu-id="00c91-120">If permissive semantics are being used, all widening and narrowing conversions (in other words, all conversions) may occur implicitly.</span></span>

## <a name="boolean-conversions"></a><span data-ttu-id="00c91-121">Boolesche Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="00c91-121">Boolean Conversions</span></span>

<span data-ttu-id="00c91-122">Obwohl `Boolean` kein numerischer Typ ist, verfügt er über einschränkende Konvertierungen in und aus den numerischen Typen, als ob es sich um einen enumerierten Typ handelt.</span><span class="sxs-lookup"><span data-stu-id="00c91-122">Although `Boolean` is not a numeric type, it does have narrowing conversions to and from the numeric types as if it were an enumerated type.</span></span> <span data-ttu-id="00c91-123">Der Literal`True` konvertiert in den Literal`255` für `Byte`, `65535` für `UShort`, `4294967295` `UInteger``18446744073709551615`, `ULong`für `-1` und den Ausdrucks `SByte`für `Short`, `Integer`, `Long`, `Decimal`, `Single`, `Double`und.</span><span class="sxs-lookup"><span data-stu-id="00c91-123">The literal `True` converts to the literal `255` for `Byte`, `65535` for `UShort`, `4294967295` for `UInteger`, `18446744073709551615` for `ULong`, and to the expression `-1` for `SByte`, `Short`, `Integer`, `Long`, `Decimal`, `Single`, and `Double`.</span></span> <span data-ttu-id="00c91-124">Der Literal`False` konvertiert in den Literal`0`.</span><span class="sxs-lookup"><span data-stu-id="00c91-124">The literal `False` converts to the literal `0`.</span></span> <span data-ttu-id="00c91-125">Ein numerischer Wert von 0 (null) wird in die Literale `False`</span><span class="sxs-lookup"><span data-stu-id="00c91-125">A zero numeric value converts to the literal `False`.</span></span> <span data-ttu-id="00c91-126">Alle anderen numerischen Werte werden in die Literale `True`konvertiert.</span><span class="sxs-lookup"><span data-stu-id="00c91-126">All other numeric values convert to the literal `True`.</span></span>

<span data-ttu-id="00c91-127">Es gibt eine einschränkende Konvertierung von einem booleschen Wert in eine Zeichenfolge, die entweder in `System.Boolean.TrueString` oder `System.Boolean.FalseString`konvertiert wird.</span><span class="sxs-lookup"><span data-stu-id="00c91-127">There is a narrowing conversion from Boolean to String, converting to either `System.Boolean.TrueString` or `System.Boolean.FalseString`.</span></span> <span data-ttu-id="00c91-128">Es gibt auch eine einschränkende Konvertierung von `String` in `Boolean`: Wenn die Zeichenfolge gleich `TrueString` oder `FalseString` ist (in der aktuellen Kultur, ohne Berücksichtigung der Groß-/Kleinschreibung), wird der entsprechende Wert verwendet. Andernfalls wird versucht, die Zeichenfolge als numerischen Typ (in Hexadezimal oder oktal, wenn möglich, andernfalls als float) zu analysieren und die obigen Regeln zu verwenden. Andernfalls wird `System.InvalidCastException`ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="00c91-128">There is also a narrowing conversion from `String` to `Boolean`: if the string was equal to `TrueString` or `FalseString` (in the current culture, case-insensitively) then it uses the appropriate value; otherwise it attempts to parse the string as a numeric type (in hex or octal if possible, otherwise as a float) and uses the above rules; otherwise it throws `System.InvalidCastException`.</span></span>

## <a name="numeric-conversions"></a><span data-ttu-id="00c91-129">Numerische Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="00c91-129">Numeric Conversions</span></span>

<span data-ttu-id="00c91-130">Numerische Konvertierungen zwischen den Typen `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single` und `Double`sowie alle aufgelisteten Typen.</span><span class="sxs-lookup"><span data-stu-id="00c91-130">Numeric conversions exist between the types `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single` and `Double`, and all enumerated types.</span></span> <span data-ttu-id="00c91-131">Beim Konvertieren werden Enumerationstypen so behandelt, als wären Sie Ihre zugrunde liegenden Typen.</span><span class="sxs-lookup"><span data-stu-id="00c91-131">When being converted, enumerated types are treated as if they were their underlying types.</span></span> <span data-ttu-id="00c91-132">Beim Umrechnen in einen enumerierten Typ muss der Quellwert nicht mit dem Satz von Werten übereinstimmen, die im enumerierten Typ definiert sind.</span><span class="sxs-lookup"><span data-stu-id="00c91-132">When converting to an enumerated type, the source value is not required to conform to the set of values defined in the enumerated type.</span></span> <span data-ttu-id="00c91-133">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="00c91-133">For example:</span></span>

```vb
Enum Values
    One
    Two
    Three
End Enum

Module Test
    Sub Main()
        Dim x As Integer = 5

        ' OK, even though there is no enumerated value for 5.
        Dim y As Values = CType(x, Values)
    End Sub
End Module
```

<span data-ttu-id="00c91-134">Numerische Konvertierungen werden zur Laufzeit wie folgt verarbeitet:</span><span class="sxs-lookup"><span data-stu-id="00c91-134">Numeric conversions are processed at run-time as follows:</span></span>

* <span data-ttu-id="00c91-135">Bei einer Konvertierung eines numerischen Typs in einen umfassenderen numerischen Typ wird der Wert einfach in den umfassenderen Typ konvertiert.</span><span class="sxs-lookup"><span data-stu-id="00c91-135">For a conversion from a numeric type to a wider numeric type, the value is simply converted to the wider type.</span></span> <span data-ttu-id="00c91-136">Konvertierungen von `UInteger`, `Integer`, `ULong`, `Long`oder `Decimal` zu `Single` oder `Double` werden auf den nächstgelegenen `Single` oder `Double` Wert gerundet.</span><span class="sxs-lookup"><span data-stu-id="00c91-136">Conversions from `UInteger`, `Integer`, `ULong`, `Long`, or `Decimal` to `Single` or `Double` are rounded to the nearest `Single` or `Double` value.</span></span> <span data-ttu-id="00c91-137">Diese Konvertierung kann zwar zu einem Genauigkeits Verlust führen, es wird jedoch nie ein Größen Verlust verursacht.</span><span class="sxs-lookup"><span data-stu-id="00c91-137">While this conversion may cause a loss of precision, it will never cause a loss of magnitude.</span></span>

* <span data-ttu-id="00c91-138">Für eine Konvertierung eines ganzzahligen Typs in einen anderen ganzzahligen Typ oder von `Single`, `Double`oder `Decimal` in einen ganzzahligen Typ hängt das Ergebnis davon ab, ob die Überprüfung der ganzzahligen Überlauf erfolgt:</span><span class="sxs-lookup"><span data-stu-id="00c91-138">For a conversion from an integral type to another integral type, or from `Single`, `Double`, or `Decimal` to an integral type, the result depends on whether integer overflow checking is on:</span></span>

  <span data-ttu-id="00c91-139">*Wenn der ganzzahlige Überlauf geprüft wird:*</span><span class="sxs-lookup"><span data-stu-id="00c91-139">*If integer overflow is being checked:*</span></span>

  * <span data-ttu-id="00c91-140">Wenn die Quelle ein ganzzahliger Typ ist, wird die Konvertierung erfolgreich ausgeführt, wenn das Quell Argument innerhalb des Bereichs des Zieltyps liegt.</span><span class="sxs-lookup"><span data-stu-id="00c91-140">If the source is an integral type, the conversion succeeds if the source argument is within the range of the destination type.</span></span> <span data-ttu-id="00c91-141">Die Konvertierung löst eine `System.OverflowException` Ausnahme aus, wenn das Quell Argument außerhalb des Bereichs des Zieltyps liegt.</span><span class="sxs-lookup"><span data-stu-id="00c91-141">The conversion throws a `System.OverflowException` exception if the source argument is outside the range of the destination type.</span></span>

  * <span data-ttu-id="00c91-142">Wenn die Quelle `Single`, `Double`oder `Decimal`ist, wird der Quellwert auf den nächstgelegenen ganzzahligen Wert aufgerundet oder auf ihn herab gerundet, und dieser ganzzahlige Wert wird das Ergebnis der Konvertierung.</span><span class="sxs-lookup"><span data-stu-id="00c91-142">If the source is `Single`, `Double`, or `Decimal`, the source value is rounded up or down to the nearest integral value, and this integral value becomes the result of the conversion.</span></span> <span data-ttu-id="00c91-143">Wenn der Quellwert gleich nah bei zwei ganzzahligen Werten ist, wird der Wert auf den Wert gerundet, der über eine gerade Zahl in der am wenigsten signifikanten Ziffern Position verfügt.</span><span class="sxs-lookup"><span data-stu-id="00c91-143">If the source value is equally close to two integral values, the value is rounded to the value that has an even number in the least significant digit position.</span></span> <span data-ttu-id="00c91-144">Wenn sich der resultierende ganzzahlige Wert außerhalb des Bereichs des Zieltyps befindet, wird eine `System.OverflowException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="00c91-144">If the resulting integral value is outside the range of the destination type, a `System.OverflowException` exception is thrown.</span></span>

  <span data-ttu-id="00c91-145">*Wenn der ganzzahlige Überlauf nicht geprüft wird:*</span><span class="sxs-lookup"><span data-stu-id="00c91-145">*If integer overflow is not being checked:*</span></span>

  * <span data-ttu-id="00c91-146">Wenn die Quelle ein ganzzahliger Typ ist, wird die Konvertierung immer erfolgreich ausgeführt und besteht lediglich darin, die signifikantesten Bits des Quell Werts zu verwerfen.</span><span class="sxs-lookup"><span data-stu-id="00c91-146">If the source is an integral type, the conversion always succeeds and simply consists of discarding the most significant bits of the source value.</span></span>

  * <span data-ttu-id="00c91-147">Wenn die Quelle `Single`, `Double`oder `Decimal`ist, ist die Konvertierung immer erfolgreich und besteht lediglich aus der Rundung des Quell Werts auf den nächstgelegenen ganzzahligen Wert.</span><span class="sxs-lookup"><span data-stu-id="00c91-147">If the source is `Single`, `Double`, or `Decimal`, the conversion always succeeds and simply consists of rounding the source value towards the nearest integral value.</span></span> <span data-ttu-id="00c91-148">Wenn der Quellwert gleich nah bei zwei ganzzahligen Werten ist, wird der Wert immer auf den Wert gerundet, der über eine gerade Zahl in der am wenigsten wichtigen Ziffern Position verfügt.</span><span class="sxs-lookup"><span data-stu-id="00c91-148">If the source value is equally close to two integral values, the value is always rounded to the value that has an even number in the least significant digit position.</span></span>

* <span data-ttu-id="00c91-149">Bei einer Konvertierung von `Double` in `Single`wird der `Double` Wert auf den nächsten `Single` Wert gerundet.</span><span class="sxs-lookup"><span data-stu-id="00c91-149">For a conversion from `Double` to `Single`, the `Double` value is rounded to the nearest `Single` value.</span></span> <span data-ttu-id="00c91-150">Wenn der `Double` Wert zu klein ist, um als `Single`darzustellen, wird das Ergebnis positiv 0 (null) oder negativ 0 (null).</span><span class="sxs-lookup"><span data-stu-id="00c91-150">If the `Double` value is too small to represent as a `Single`, the result becomes positive zero or negative zero.</span></span> <span data-ttu-id="00c91-151">Wenn der `Double` Wert zu groß ist, um als `Single`darzustellen, wird das Ergebnis positiv unendlich oder minus unendlich.</span><span class="sxs-lookup"><span data-stu-id="00c91-151">If the `Double` value is too large to represent as a `Single`, the result becomes positive infinity or negative infinity.</span></span> <span data-ttu-id="00c91-152">Wenn der `Double` Wert `NaN`ist, wird das Ergebnis ebenfalls `NaN`.</span><span class="sxs-lookup"><span data-stu-id="00c91-152">If the `Double` value is `NaN`, the result is also `NaN`.</span></span>

* <span data-ttu-id="00c91-153">Bei einer Konvertierung von `Single` oder `Double` zu `Decimal`wird der Quellwert in `Decimal` Darstellung konvertiert und bei Bedarf auf die nächstgelegene Zahl nach dem 28. Dezimaltrennzeichen gerundet.</span><span class="sxs-lookup"><span data-stu-id="00c91-153">For a conversion from `Single` or `Double` to `Decimal`, the source value is converted to `Decimal` representation and rounded to the nearest number after the 28th decimal place if required.</span></span> <span data-ttu-id="00c91-154">Wenn der Quellwert zu klein ist, um als `Decimal`darzustellen, wird das Ergebnis 0 (null).</span><span class="sxs-lookup"><span data-stu-id="00c91-154">If the source value is too small to represent as a `Decimal`, the result becomes zero.</span></span> <span data-ttu-id="00c91-155">Wenn der Quellwert `NaN`, unendlich oder zu groß ist, um als `Decimal`darzustellen, wird eine `System.OverflowException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="00c91-155">If the source value is `NaN`, infinity, or too large to represent as a `Decimal`, a `System.OverflowException` exception is thrown.</span></span>

* <span data-ttu-id="00c91-156">Bei einer Konvertierung von `Double` in `Single`wird der `Double` Wert auf den nächsten `Single` Wert gerundet.</span><span class="sxs-lookup"><span data-stu-id="00c91-156">For a conversion from `Double` to `Single`, the `Double` value is rounded to the nearest `Single` value.</span></span> <span data-ttu-id="00c91-157">Wenn der `Double` Wert zu klein ist, um als `Single`darzustellen, wird das Ergebnis positiv 0 (null) oder negativ 0 (null).</span><span class="sxs-lookup"><span data-stu-id="00c91-157">If the `Double` value is too small to represent as a `Single`, the result becomes positive zero or negative zero.</span></span> <span data-ttu-id="00c91-158">Wenn der `Double` Wert zu groß ist, um als `Single`darzustellen, wird das Ergebnis positiv unendlich oder minus unendlich.</span><span class="sxs-lookup"><span data-stu-id="00c91-158">If the `Double` value is too large to represent as a `Single`, the result becomes positive infinity or negative infinity.</span></span> <span data-ttu-id="00c91-159">Wenn der `Double` Wert `NaN`ist, wird das Ergebnis ebenfalls `NaN`.</span><span class="sxs-lookup"><span data-stu-id="00c91-159">If the `Double` value is `NaN`, the result is also `NaN`.</span></span>

## <a name="reference-conversions"></a><span data-ttu-id="00c91-160">Verweiskonvertierungen</span><span class="sxs-lookup"><span data-stu-id="00c91-160">Reference Conversions</span></span>

<span data-ttu-id="00c91-161">Verweis Typen können in einen Basistyp konvertiert werden und umgekehrt.</span><span class="sxs-lookup"><span data-stu-id="00c91-161">Reference types may be converted to a base type, and vice versa.</span></span> <span data-ttu-id="00c91-162">Konvertierungen eines Basistyps in einen stärker abgeleiteten Typ werden nur zur Laufzeit erfolgreich ausgeführt, wenn der zu konvertierende Wert ein NULL-Wert, der abgeleitete Typ selbst oder ein stärker abgeleiteter Typ ist.</span><span class="sxs-lookup"><span data-stu-id="00c91-162">Conversions from a base type to a more derived type only succeed at run time if the value being converted is a null value, the derived type itself, or a more derived type.</span></span>

<span data-ttu-id="00c91-163">Klassen-und Schnittstellentypen können in und aus einem beliebigen Schnittstellentyp umgewandelt werden.</span><span class="sxs-lookup"><span data-stu-id="00c91-163">Class and interface types can be cast to and from any interface type.</span></span> <span data-ttu-id="00c91-164">Konvertierungen zwischen einem Typ und einem Schnittstellentyp sind nur zur Laufzeit erfolgreich, wenn die tatsächlich beteiligten Typen eine Vererbungs-oder Implementierungs Beziehung aufweisen.</span><span class="sxs-lookup"><span data-stu-id="00c91-164">Conversions between a type and an interface type only succeed at run time if the actual types involved have an inheritance or implementation relationship.</span></span> <span data-ttu-id="00c91-165">Da ein Schnittstellentyp immer eine Instanz eines Typs enthält, der von `Object`abgeleitet ist, kann ein Schnittstellentyp auch immer in und aus `Object`umgewandelt werden.</span><span class="sxs-lookup"><span data-stu-id="00c91-165">Because an interface type will always contain an instance of a type that derives from `Object`, an interface type can also always be cast to and from `Object`.</span></span>

<span data-ttu-id="00c91-166">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="00c91-166">__Note.__</span></span> <span data-ttu-id="00c91-167">Es ist kein Fehler, eine `NotInheritable` Klassen in und von Schnittstellen zu konvertieren, die nicht implementiert werden, da Klassen, die com-Klassen darstellen, möglicherweise Schnittstellen Implementierungen aufweisen, die bis zur Laufzeit nicht bekannt sind.</span><span class="sxs-lookup"><span data-stu-id="00c91-167">It is not an error to convert a `NotInheritable` classes to and from interfaces that it does not implement because classes that represent COM classes may have interface implementations that are not known until run time.</span></span> 

<span data-ttu-id="00c91-168">Wenn eine Verweis Konvertierung zur Laufzeit fehlschlägt, wird eine `System.InvalidCastException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="00c91-168">If a reference conversion fails at run time, a `System.InvalidCastException` exception is thrown.</span></span>

### <a name="reference-variance-conversions"></a><span data-ttu-id="00c91-169">Verweis Varianz Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="00c91-169">Reference Variance Conversions</span></span>

<span data-ttu-id="00c91-170">Generische Schnittstellen oder Delegaten können über Variante Typparameter verfügen, die Konvertierungen zwischen kompatiblen Varianten des Typs ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="00c91-170">Generic interfaces or delegates may have variant type parameters that allow conversions between compatible variants of the type.</span></span> <span data-ttu-id="00c91-171">Daher ist zur Laufzeit eine Konvertierung von einem Klassentyp oder einem Schnittstellentyp in einen Schnittstellentyp möglich, der Variant mit einem Schnittstellentyp kompatibel ist, der von ihm geerbt oder implementiert wird.</span><span class="sxs-lookup"><span data-stu-id="00c91-171">Therefore, at runtime a conversion from a class type or an interface type to an interface type that is variant compatible with an interface type it inherits from or implements will succeed.</span></span> <span data-ttu-id="00c91-172">Ebenso können Delegattypen in und aus Variant-kompatiblen Delegattypen umgewandelt werden.</span><span class="sxs-lookup"><span data-stu-id="00c91-172">Similarly, delegate types can be cast to and from variant compatible delegate types.</span></span> <span data-ttu-id="00c91-173">Beispielsweise der Delegattyp.</span><span class="sxs-lookup"><span data-stu-id="00c91-173">For example, the delegate type</span></span>

```vb
Delegate Function F(Of In A, Out R)(a As A) As R
```

<span data-ttu-id="00c91-174">ermöglicht eine Konvertierung von `F(Of Object, Integer)` in `F(Of String, Integer)`.</span><span class="sxs-lookup"><span data-stu-id="00c91-174">would allow a conversion from `F(Of Object, Integer)` to `F(Of String, Integer)`.</span></span> <span data-ttu-id="00c91-175">Das heißt, dass ein Delegat, `F` `Object` verwendet, möglicherweise sicher als Delegat verwendet wird `F` der `String`benötigt.</span><span class="sxs-lookup"><span data-stu-id="00c91-175">That is, a delegate `F` which takes `Object` may be safely used as a delegate `F` which takes `String`.</span></span> <span data-ttu-id="00c91-176">Wenn der Delegat aufgerufen wird, erwartet die Ziel Methode ein Objekt, und eine Zeichenfolge ist ein Objekt.</span><span class="sxs-lookup"><span data-stu-id="00c91-176">When the delegate is invoked, the target method will be expecting an object, and a string is an object.</span></span>

<span data-ttu-id="00c91-177">Ein generischer Delegat-oder Schnittstellentyp `S(Of S1,...,Sn)` als *Variant-kompatibel* mit einer generischen Schnittstelle oder einem Delegattyp bezeichnet, `T(Of T1,...,Tn)` if:</span><span class="sxs-lookup"><span data-stu-id="00c91-177">A generic delegate or interface type `S(Of S1,...,Sn)` is said to be *variant compatible* with a generic interface or delegate type `T(Of T1,...,Tn)` if:</span></span>

* <span data-ttu-id="00c91-178">`S` und `T` werden beide aus dem gleichen generischen Typ `U(Of U1,...,Un)`konstruiert.</span><span class="sxs-lookup"><span data-stu-id="00c91-178">`S` and `T` are both constructed from the same generic type `U(Of U1,...,Un)`.</span></span>

* <span data-ttu-id="00c91-179">Für jeden Typparameter `Ux`:</span><span class="sxs-lookup"><span data-stu-id="00c91-179">For each type parameter `Ux`:</span></span>

  * <span data-ttu-id="00c91-180">Wenn der Typparameter ohne Varianz deklariert wurde, müssen `Sx` und `Tx` denselben Typ aufweisen.</span><span class="sxs-lookup"><span data-stu-id="00c91-180">If the type parameter was declared without variance then `Sx` and `Tx` must be the same type.</span></span>

  * <span data-ttu-id="00c91-181">Wenn der Typparameter deklariert wurde `In` dann muss eine erweiternde Identitäts-, Standard-, Verweis-, Array-oder Typparameter Konvertierung von `Sx` in `Tx`erfolgen.</span><span class="sxs-lookup"><span data-stu-id="00c91-181">If the type parameter was declared `In` then there must be a widening identity, default, reference, array, or type parameter conversion from `Sx` to `Tx`.</span></span>

  * <span data-ttu-id="00c91-182">Wenn der Typparameter deklariert wurde `Out` dann muss eine erweiternde Identitäts-, Standard-, Verweis-, Array-oder Typparameter Konvertierung von `Tx` in `Sx`erfolgen.</span><span class="sxs-lookup"><span data-stu-id="00c91-182">If the type parameter was declared `Out` then there must be a widening identity, default, reference, array, or type parameter conversion from `Tx` to `Sx`.</span></span>

<span data-ttu-id="00c91-183">Beim Konvertieren von einer Klasse in eine generische Schnittstelle mit Varianten Typparametern ist die Konvertierung mehrdeutig, wenn keine nicht-Variante Konvertierung vorhanden ist, wenn die Klasse mehr als eine Variant-kompatible Schnittstelle implementiert.</span><span class="sxs-lookup"><span data-stu-id="00c91-183">When converting from a class to a generic interface with variant type parameters, if the class implements more than one variant compatible interface the conversion is ambiguous if there is not a non-variant conversion.</span></span> <span data-ttu-id="00c91-184">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="00c91-184">For example:</span></span>

```vb
Class Base
End Class

Class Derived1
    Inherits Base
End Class

Class Derived2
    Inherits Base
End Class

Class OneAndTwo
    Implements IEnumerable(Of Derived1)
    Implements IEnumerable(Of Derived2)
End Class

Class BaseAndOneAndTwo
    Implements IEnumerable(Of Base)
    Implements IEnumerable(Of Derived1)
    Implements IEnumerable(Of Derived2)
End Class

Module Test
    Sub Main()
        ' Error: conversion is ambiguous
        Dim x As IEnumerable(Of Base) = New OneAndTwo()

        ' OK, will pick up the direct implementation of IEnumerable(Of Base)
        Dim y as IEnumerable(Of Base) = New BaseAndOneAndTwo()
    End Sub
End Module
```

### <a name="anonymous-delegate-conversions"></a><span data-ttu-id="00c91-185">Anonyme delegatkonvertierungen</span><span class="sxs-lookup"><span data-stu-id="00c91-185">Anonymous Delegate Conversions</span></span>

<span data-ttu-id="00c91-186">Wenn ein Ausdruck, der als Lambda-Methode klassifiziert ist, als Wert in einem Kontext neu klassifiziert wird, in dem kein Zieltyp (z. b. `Dim x = Function(a As Integer, b As Integer) a + b`) oder der Zieltyp kein Delegattyp ist, ist der Typ des resultierenden Ausdrucks ein anonymer Delegattyp, der der Signatur der Lambda-Methode entspricht.</span><span class="sxs-lookup"><span data-stu-id="00c91-186">When an expression classified as a lambda method is reclassified as a value in a context where there is no target type (for example, `Dim x = Function(a As Integer, b As Integer) a + b`), or where the target type is not a delegate type, the type of the resulting expression is an anonymous delegate type equivalent to the signature of the lambda method.</span></span> <span data-ttu-id="00c91-187">Dieser anonyme Delegattyp hat eine Konvertierung in einen beliebigen kompatiblen Delegattyp: ein kompatibler Delegattyp ist ein beliebiger Delegattyp, der mithilfe eines delegaterstellungs-Ausdrucks mit der `Invoke`-Methode des anonymen Delegattyps als Parameter erstellt werden kann</span><span class="sxs-lookup"><span data-stu-id="00c91-187">This anonymous delegate type has a conversion to any compatible delegate type: a compatible delegate type is any delegate type that can be created using a delegate creation expression with the anonymous delegate type's `Invoke` method as a parameter.</span></span> <span data-ttu-id="00c91-188">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="00c91-188">For example:</span></span>

```vb
' Anonymous delegate type similar to Func(Of Object, Object, Object)
Dim x = Function(x, y) x + y

' OK because delegate type is compatible
Dim y As Func(Of Integer, Integer, Integer) = x
```

<span data-ttu-id="00c91-189">Beachten Sie, dass die Typen `System.Delegate` und `System.MulticastDelegate` nicht selbst als Delegattypen angesehen werden (auch wenn alle Delegattypen von Ihnen erben).</span><span class="sxs-lookup"><span data-stu-id="00c91-189">Note that the types `System.Delegate` and `System.MulticastDelegate` are not themselves considered delegate types (even though all delegate types inherit from them).</span></span> <span data-ttu-id="00c91-190">Beachten Sie außerdem, dass die Konvertierung eines anonymen Delegattyps in einen kompatiblen Delegattyp keine Verweis Konvertierung ist.</span><span class="sxs-lookup"><span data-stu-id="00c91-190">Also, note that the conversion from anonymous delegate type to a compatible delegate type is not a reference conversion.</span></span>

## <a name="array-conversions"></a><span data-ttu-id="00c91-191">Arraykonvertierungen</span><span class="sxs-lookup"><span data-stu-id="00c91-191">Array Conversions</span></span>

<span data-ttu-id="00c91-192">Neben den Konvertierungen, die für Arrays definiert sind, weil es sich um Verweis Typen handelt, gibt es mehrere spezielle Konvertierungen für Arrays.</span><span class="sxs-lookup"><span data-stu-id="00c91-192">Besides the conversions that are defined on arrays by virtue of the fact that they are reference types, several special conversions exist for arrays.</span></span>

<span data-ttu-id="00c91-193">Bei zwei Typen `A` und `B`, wenn beide Verweis Typen oder Typparameter sind, die keine Werttypen sind, und wenn `A` eine Verweis-, Array-oder Typparameter Konvertierung in `B`hat, ist eine Konvertierung von einem Array vom Typ `A` in ein Array vom Typ `B` mit dem gleichen Rang vorhanden.</span><span class="sxs-lookup"><span data-stu-id="00c91-193">For any two types `A` and `B`, if they are both reference types or type parameters not known to be value types, and if `A` has a reference, array, or type parameter conversion to `B`, a conversion exists from an array of type `A` to an array of type `B` with the same rank.</span></span> <span data-ttu-id="00c91-194">Diese Beziehung wird als *Array Kovarianz*bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="00c91-194">This relationship is known as *array covariance*.</span></span> <span data-ttu-id="00c91-195">Array Kovarianz bedeutet insbesondere, dass ein Element eines Arrays, dessen Elementtyp `B` ist, tatsächlich ein Element eines Arrays sein kann, dessen Elementtyp `A`ist, vorausgesetzt, dass sowohl `A` als auch `B` Verweis Typen sind und dass `B` eine Verweis Konvertierung oder eine Array Konvertierung in `A`aufweist.</span><span class="sxs-lookup"><span data-stu-id="00c91-195">Array covariance in particular means that an element of an array whose element type is `B` may actually be an element of an array whose element type is `A`, provided that both `A` and `B` are reference types and that `B` has a reference conversion or array conversion to `A`.</span></span> <span data-ttu-id="00c91-196">Im folgenden Beispiel bewirkt der zweite Aufruf von `F` bewirkt, dass eine `System.ArrayTypeMismatchException` Ausnahme ausgelöst wird, da der tatsächliche Elementtyp von `b` `String`und nicht `Object`ist:</span><span class="sxs-lookup"><span data-stu-id="00c91-196">In the following example, the second invocation of `F` causes a `System.ArrayTypeMismatchException` exception to be thrown because the actual element type of `b` is `String`, not `Object`:</span></span>

```vb
Module Test
    Sub F(ByRef x As Object)
    End Sub

    Sub Main()
        Dim a(10) As Object
        Dim b() As Object = New String(10) {}
        F(a(0)) ' OK.
        F(b(1)) ' Not allowed: System.ArrayTypeMismatchException.
   End Sub
End Module
```

<span data-ttu-id="00c91-197">Aufgrund von Array Kovarianz enthalten Zuweisungen zu Elementen von Verweistyp Arrays eine Lauf Zeit Überprüfung, mit der sichergestellt wird, dass der Wert, der dem Array Element zugewiesen wird, tatsächlich einen zulässigen Typ hat.</span><span class="sxs-lookup"><span data-stu-id="00c91-197">Because of array covariance, assignments to elements of reference type arrays include a run-time check that ensures that the value being assigned to the array element is actually of a permitted type.</span></span>

```vb
Module Test
    Sub Fill(array() As Object, index As Integer, count As Integer, _
            value As Object)
        Dim i As Integer

        For i = index To (index + count) - 1
            array(i) = value
        Next i
    End Sub

    Sub Main()
        Dim strings(100) As String

        Fill(strings, 0, 101, "Undefined")
        Fill(strings, 0, 10, Nothing)
        Fill(strings, 91, 10, 0)
    End Sub
End Module
```

<span data-ttu-id="00c91-198">In diesem Beispiel enthält die Zuweisung zum `array(i)` in der-Methode `Fill` implizit eine Lauf Zeit Überprüfung, mit der sichergestellt wird, dass das Objekt, auf das die Variable verweist `value` entweder `Nothing` oder eine Instanz eines Typs ist, der mit dem tatsächlichen Elementtyp von Array-`array`kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="00c91-198">In this example, the assignment to `array(i)` in method `Fill` implicitly includes a run-time check that ensures that the object referenced by the variable `value` is either `Nothing` or an instance of a type that is compatible with the actual element type of array `array`.</span></span> <span data-ttu-id="00c91-199">In der-Methode `Main``Fill` die ersten beiden Aufrufe der-Methode erfolgreich, aber der dritte Aufruf bewirkt, dass eine `System.ArrayTypeMismatchException` Ausnahme ausgelöst wird, wenn die erste Zuweisung zum `array(i)`ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="00c91-199">In method `Main`, the first two invocations of method `Fill` succeed, but the third invocation causes a `System.ArrayTypeMismatchException` exception to be thrown upon executing the first assignment to `array(i)`.</span></span> <span data-ttu-id="00c91-200">Die Ausnahme tritt auf, weil ein `Integer` nicht in einem `String` Array gespeichert werden kann.</span><span class="sxs-lookup"><span data-stu-id="00c91-200">The exception occurs because an `Integer` cannot be stored in a `String` array.</span></span>

<span data-ttu-id="00c91-201">Wenn einer der Array Elementtypen ein Typparameter ist, dessen Typ zur Laufzeit ein Werttyp ist, wird eine `System.InvalidCastException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="00c91-201">If one of the array element types is a type parameter whose type turns out to be a value type at runtime, a `System.InvalidCastException` exception will be thrown.</span></span> <span data-ttu-id="00c91-202">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="00c91-202">For example:</span></span>

```vb
Module Test
    Sub F(Of T As U, U)(x() As T)
        Dim y() As U = x
    End Sub

    Sub Main()
        ' F will throw an exception because Integer() cannot be
        ' converted to Object()
        F(New Integer() { 1, 2, 3 })
    End Sub
End Module
```

<span data-ttu-id="00c91-203">Konvertierungen sind auch zwischen einem Array eines enumerierten Typs und einem Array des zugrunde liegenden Typs des Enumerationstyps oder eines Arrays eines anderen Enumerationstyps mit demselben zugrunde liegenden Typ vorhanden, vorausgesetzt, die Arrays haben denselben Rang.</span><span class="sxs-lookup"><span data-stu-id="00c91-203">Conversions also exist between an array of an enumerated type and an array of the enumerated type's underlying type or an array of another enumerated type with the same underlying type, provided the arrays have the same rank.</span></span>

```vb
Enum Color As Byte
    Red
    Green
    Blue
End Enum

Module Test
    Sub Main()
        Dim a(10) As Color
        Dim b() As Integer
        Dim c() As Byte

        b = a    ' Error: Integer is not the underlying type of Color
        c = a    ' OK
        a = c    ' OK
    End Sub
End Module
```

<span data-ttu-id="00c91-204">In diesem Beispiel wird ein Array von `Color` in ein und aus einem Array von `Byte`, dem zugrunde liegenden Typ `Color`, konvertiert.</span><span class="sxs-lookup"><span data-stu-id="00c91-204">In this example, an array of `Color` is converted to and from an array of `Byte`, `Color`'s underlying type.</span></span> <span data-ttu-id="00c91-205">Die Konvertierung in ein Array von `Integer`ist jedoch ein Fehler, da `Integer` nicht der zugrunde liegende Typ von `Color`ist.</span><span class="sxs-lookup"><span data-stu-id="00c91-205">The conversion to an array of `Integer`, however, will be an error because `Integer` is not the underlying type of `Color`.</span></span>

<span data-ttu-id="00c91-206">Ein Rang 1-Array vom Typ `A()` auch eine Array Konvertierung in die Auflistungs Schnittstellentypen `IList(Of B)`, `IReadOnlyList(Of B)`, `ICollection(Of B)`, `IReadOnlyCollection(Of B)` und `IEnumerable(Of B)`, die in `System.Collections.Generic`gefunden werden, solange eine der folgenden Punkte zutrifft:</span><span class="sxs-lookup"><span data-stu-id="00c91-206">A rank-1 array of type `A()` also has an array conversion to the collection interface types `IList(Of B)`, `IReadOnlyList(Of B)`, `ICollection(Of B)`, `IReadOnlyCollection(Of B)` and `IEnumerable(Of B)` found in `System.Collections.Generic`, so long as one of the following is true:</span></span>

- <span data-ttu-id="00c91-207">`A` und `B` sind Verweis Typen oder Typparameter, die keine Werttypen sind. und `A` über eine erweiternde Verweis-, Array-oder Typparameter Konvertierung in `B`; noch</span><span class="sxs-lookup"><span data-stu-id="00c91-207">`A` and `B` are both reference types or type parameters not known to be value types; and `A` has a widening reference, array or type parameter conversion to `B`; or</span></span>
- <span data-ttu-id="00c91-208">`A` und `B` sind beide Enumerationstypen desselben zugrunde liegenden Typs. noch</span><span class="sxs-lookup"><span data-stu-id="00c91-208">`A` and `B` are both enumerated types of the same underlying type; or</span></span>
- <span data-ttu-id="00c91-209">eine `A` und `B` ist ein Enumerationstyp, der andere ist der zugrunde liegende Typ.</span><span class="sxs-lookup"><span data-stu-id="00c91-209">one of `A` and `B` is an enumerated type, and the other is its underlying type.</span></span>

<span data-ttu-id="00c91-210">Ein Array vom Typ A mit einem beliebigen Rang verfügt auch über eine Array Konvertierung in die nicht generischen Auflistungs Schnittstellentypen `IList`, `ICollection` und `IEnumerable`, die in `System.Collections`gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="00c91-210">Any array of type A with any rank also has an array conversion to the non-generic collection interface types `IList`, `ICollection` and `IEnumerable` found in `System.Collections`.</span></span>

<span data-ttu-id="00c91-211">Es ist möglich, die resultierenden Schnittstellen mithilfe von `For Each`zu durchlaufen oder die `GetEnumerator` Methoden direkt aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="00c91-211">It is possible to iterate over the resulting interfaces using `For Each`, or through invoking the `GetEnumerator` methods directly.</span></span> <span data-ttu-id="00c91-212">Wenn Rang 1 Arrays generische oder nicht generische Formen von `IList` oder `ICollection`konvertieren, ist es auch möglich, Elemente nach Index zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="00c91-212">In the case of rank-1 arrays converted generic or non-generic forms of `IList` or `ICollection`, it is also possible to get elements by index.</span></span> <span data-ttu-id="00c91-213">Im Fall von Rang 1-Arrays, die in generische oder nicht generische Formen von `IList`konvertiert werden, ist es auch möglich, Elemente nach Index festzulegen. Dies unterliegt den gleichen Kovarianz Überprüfungen des Lauf Zeit Arrays, wie oben beschrieben.</span><span class="sxs-lookup"><span data-stu-id="00c91-213">In the case of rank-1 arrays converted to generic or non-generic forms of `IList`, it is also possible to set elements by index, subject to the same runtime array covariance checks as described above.</span></span> <span data-ttu-id="00c91-214">Das Verhalten aller anderen Schnittstellen Methoden ist durch die VB-Sprachspezifikation nicht definiert. Es liegt an der zugrunde liegenden Laufzeit.</span><span class="sxs-lookup"><span data-stu-id="00c91-214">The behavior of all other interface methods is undefined by the VB language specification; it is up to the underlying runtime.</span></span>

## <a name="value-type-conversions"></a><span data-ttu-id="00c91-215">Werttyp Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="00c91-215">Value Type Conversions</span></span>

<span data-ttu-id="00c91-216">Ein Werttyp Wert kann in einen seiner Basis Verweis Typen oder einen Schnittstellentyp konvertiert werden, der durch einen als *Boxing*bezeichneten Prozess implementiert wird.</span><span class="sxs-lookup"><span data-stu-id="00c91-216">A value type value can be converted to one of its base reference types or an interface type that it implements through a process called *boxing*.</span></span> <span data-ttu-id="00c91-217">Wenn ein Wert für einen Werttyp in einen Kasten konvertiert wird, wird der Wert von der Position kopiert, an der er sich auf den .NET Framework Heap befindet.</span><span class="sxs-lookup"><span data-stu-id="00c91-217">When a value type value is boxed, the value is copied from the location where it lives onto the .NET Framework heap.</span></span> <span data-ttu-id="00c91-218">Ein Verweis auf diesen Speicherort auf dem Heap wird dann zurückgegeben und kann in einer Verweistyp Variablen gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="00c91-218">A reference to this location on the heap is then returned and can be stored in a reference type variable.</span></span> <span data-ttu-id="00c91-219">Dieser Verweis wird *auch als geachtelte Instanz des* Werttyps bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="00c91-219">This reference is also referred to as a *boxed* instance of the value type.</span></span> <span data-ttu-id="00c91-220">Die geachtelte Instanz weist die gleiche Semantik wie ein Verweistyp anstelle eines Werttyps auf.</span><span class="sxs-lookup"><span data-stu-id="00c91-220">The boxed instance has the same semantics as a reference type instead of a value type.</span></span>

<span data-ttu-id="00c91-221">Boxed-Werttypen können durch einen Prozess namens *Unboxing*zurück in ihren ursprünglichen Werttyp konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="00c91-221">Boxed value types can be converted back to their original value type through a process called *unboxing*.</span></span> <span data-ttu-id="00c91-222">Wenn ein eingegestellter Werttyp Unboxing ist, wird der Wert aus dem Heap in einen Variablen Speicherort kopiert.</span><span class="sxs-lookup"><span data-stu-id="00c91-222">When a boxed value type is unboxed, the value is copied from the heap into a variable location.</span></span> <span data-ttu-id="00c91-223">Ab diesem Zeitpunkt wird der Wert so verhält, als ob es sich um einen Werttyp handelt.</span><span class="sxs-lookup"><span data-stu-id="00c91-223">From that point on, it behaves as if it was a value type.</span></span> <span data-ttu-id="00c91-224">Beim Unboxing eines Werttyps muss der Wert ein NULL-Wert oder eine Instanz des Werttyps sein.</span><span class="sxs-lookup"><span data-stu-id="00c91-224">When unboxing a value type, the value must be a null value or an instance of the value type.</span></span> <span data-ttu-id="00c91-225">Andernfalls wird eine `System.InvalidCastException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="00c91-225">Otherwise a `System.InvalidCastException` exception is thrown.</span></span> <span data-ttu-id="00c91-226">Wenn der Wert eine Instanz eines enumerierten Typs ist, kann dieser Wert auch auf den zugrunde liegenden Typ des enumerierten Typs oder einen anderen enumerierten Typ, der denselben zugrunde liegenden Typ aufweist, entpackt werden.</span><span class="sxs-lookup"><span data-stu-id="00c91-226">If the value is an instance of an enumerated type, that value can also be unboxed to the enumerated type's underlying type or another enumerated type that has the same underlying type.</span></span> <span data-ttu-id="00c91-227">Ein NULL-Wert wird so behandelt, als wäre er der Literale `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="00c91-227">A null value is treated as if it were the literal `Nothing`.</span></span>

<span data-ttu-id="00c91-228">Zur Unterstützung von Typen, die NULL-Werte zulassen, wird der Werttyp `System.Nullable(Of T)` bei Boxing und Unboxing besonders behandelt.</span><span class="sxs-lookup"><span data-stu-id="00c91-228">To support nullable value types well, the value type `System.Nullable(Of T)` is treated specially when doing boxing and unboxing.</span></span> <span data-ttu-id="00c91-229">Das Boxing eines Werts vom Typ `Nullable(Of T)` führt zu einem geboxten Wert des Typs `T`, wenn die `HasValue`-Eigenschaft des Werts `True` ist oder der Wert `Nothing`, wenn die `HasValue`-Eigenschaft des Werts `False`ist.</span><span class="sxs-lookup"><span data-stu-id="00c91-229">Boxing a value of type `Nullable(Of T)` results in a boxed value of type `T` if the value's `HasValue` property is `True` or a value of `Nothing` if the value's `HasValue` property is `False`.</span></span> <span data-ttu-id="00c91-230">Das Unboxing eines Werts vom Typ `T`, um `Nullable(Of T)` Ergebnisse in einer Instanz von `Nullable(Of T)`, deren `Value` Eigenschaft der eingepackte Wert und dessen `HasValue`-Eigenschaft `True`ist.</span><span class="sxs-lookup"><span data-stu-id="00c91-230">Unboxing a value of type `T` to `Nullable(Of T)` results in an instance of `Nullable(Of T)` whose `Value` property is the boxed value and whose `HasValue` property is `True`.</span></span> <span data-ttu-id="00c91-231">Der Wert `Nothing` der für alle `T` `Nullable(Of T)` entpackt werden kann, und führt zu einem Wert, dessen `HasValue` Eigenschaft `False`ist.</span><span class="sxs-lookup"><span data-stu-id="00c91-231">The value `Nothing` can be unboxed to `Nullable(Of T)` for any `T` and results in a value whose `HasValue` property is `False`.</span></span> <span data-ttu-id="00c91-232">Da sich geschachtelt-Werttypen wie Verweis Typen Verhalten, ist es möglich, mehrere Verweise auf denselben Wert zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="00c91-232">Because boxed value types behave like reference types, it is possible to create multiple references to the same value.</span></span> <span data-ttu-id="00c91-233">Bei den primitiven Typen und Enumerationstypen ist dies unerheblich, da Instanzen dieser Typen *unveränderlich*sind.</span><span class="sxs-lookup"><span data-stu-id="00c91-233">For the primitive types and enumerated types, this is irrelevant because instances of those types are *immutable*.</span></span> <span data-ttu-id="00c91-234">Das heißt, es ist nicht möglich, eine eingepackte Instanz dieser Typen zu ändern, sodass es nicht möglich ist, die Tatsache zu beobachten, dass mehrere Verweise auf denselben Wert vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="00c91-234">That is, it is not possible to modify a boxed instance of those types, so it is not possible to observe the fact that there are multiple references to the same value.</span></span>

<span data-ttu-id="00c91-235">Strukturen hingegen können änderbar sein, wenn auf ihre Instanzvariablen zugegriffen werden kann oder wenn ihre Methoden oder Eigenschaften ihre Instanzvariablen ändern.</span><span class="sxs-lookup"><span data-stu-id="00c91-235">Structures, on the other hand, may be mutable if its instance variables are accessible or if its methods or properties modify its instance variables.</span></span> <span data-ttu-id="00c91-236">Wenn ein Verweis auf eine geachtelte Struktur verwendet wird, um die Struktur zu ändern, werden alle Verweise auf die geachtelte Struktur die Änderung sehen.</span><span class="sxs-lookup"><span data-stu-id="00c91-236">If one reference to a boxed structure is used to modify the structure, then all references to the boxed structure will see the change.</span></span> <span data-ttu-id="00c91-237">Da dieses Ergebnis möglicherweise unerwartet ist, wird ein als `Object` typisierter Wert automatisch auf dem Heap geklont, anstatt nur die zugehörigen Verweise kopieren zu müssen.</span><span class="sxs-lookup"><span data-stu-id="00c91-237">Because this result may be unexpected, when a value typed as `Object` is copied from one location to another boxed value types will automatically be cloned on the heap instead of merely having their references copied.</span></span> <span data-ttu-id="00c91-238">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="00c91-238">For example:</span></span>

```vb
Class Class1
    Public Value As Integer = 0
End Class

Structure Struct1
    Public Value As Integer
End Structure

Module Test
    Sub Main()
        Dim val1 As Object = New Struct1()
        Dim val2 As Object = val1

        val2.Value = 123

        Dim ref1 As Object = New Class1()
        Dim ref2 As Object = ref1

        ref2.Value = 123

        Console.WriteLine("Values: " & val1.Value & ", " & val2.Value)
        Console.WriteLine("Refs: " & ref1.Value & ", " & ref2.Value)
    End Sub
End Module
```

<span data-ttu-id="00c91-239">Die Ausgabe des Programms lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="00c91-239">The output of the program is:</span></span>

```console
Values: 0, 123
Refs: 123, 123
```

<span data-ttu-id="00c91-240">Die Zuweisung zum-Feld der lokalen Variablen `val2` wirkt sich nicht auf das-Feld der lokalen Variablen `val1` aus, da bei der Zuweisung des geboxten `Struct1` `val2`eine Kopie des Werts erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="00c91-240">The assignment to the field of the local variable `val2` does not impact the field of the local variable `val1` because when the boxed `Struct1` was assigned to `val2`, a copy of the value was made.</span></span> <span data-ttu-id="00c91-241">Im Gegensatz dazu wirkt sich der Zuweisungs `ref2.Value = 123` auf das Objekt aus, das sowohl `ref1` als auch `ref2` verweist.</span><span class="sxs-lookup"><span data-stu-id="00c91-241">In contrast, the assignment `ref2.Value = 123` affects the object that both `ref1` and `ref2` references.</span></span>

<span data-ttu-id="00c91-242">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="00c91-242">__Note.__</span></span> <span data-ttu-id="00c91-243">Das Kopieren der Struktur wird nicht für als `System.ValueType` typisierte als durchgeführt, da es nicht möglich ist, die Bindung von `System.ValueType`zu beenden.</span><span class="sxs-lookup"><span data-stu-id="00c91-243">Structure copying is not done for boxed structures typed as `System.ValueType` because it is not possible to late bind off of `System.ValueType`.</span></span>

<span data-ttu-id="00c91-244">Es gibt eine Ausnahme von der Regel, bei der bei der Zuweisung geschachtelt-Werttypen kopiert werden.</span><span class="sxs-lookup"><span data-stu-id="00c91-244">There is one exception to the rule that boxed value types will be copied on assignment.</span></span> <span data-ttu-id="00c91-245">Wenn ein geschachtelter Werttyp Verweis in einem anderen Typ gespeichert wird, wird der innere Verweis nicht kopiert.</span><span class="sxs-lookup"><span data-stu-id="00c91-245">If a boxed value type reference is stored within another type, the inner reference will not be copied.</span></span> <span data-ttu-id="00c91-246">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="00c91-246">For example:</span></span>

```vb
Structure Struct1
    Public Value As Object
End Structure

Module Test
    Sub Main()
        Dim val1 As Struct1
        Dim val2 As Struct1

        val1.Value = New Struct1()
        val1.Value.Value = 10

        val2 = val1
        val2.Value.Value = 123
        Console.WriteLine("Values: " & val1.Value.Value & ", " & _
            val2.Value.Value)
    End Sub
End Module
```

<span data-ttu-id="00c91-247">Die Ausgabe des Programms lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="00c91-247">The output of the program is:</span></span>

```console
Values: 123, 123
```

<span data-ttu-id="00c91-248">Dies liegt daran, dass der innere geschachtelte Wert beim Kopieren des Werts nicht kopiert wird.</span><span class="sxs-lookup"><span data-stu-id="00c91-248">This is because the inner boxed value is not copied when the value is copied.</span></span> <span data-ttu-id="00c91-249">Daher verfügen sowohl `val1.Value` als auch `val2.Value` über einen Verweis auf denselben geschachtelt-Werttyp.</span><span class="sxs-lookup"><span data-stu-id="00c91-249">Thus, both `val1.Value` and `val2.Value` have a reference to the same boxed value type.</span></span>

<span data-ttu-id="00c91-250">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="00c91-250">__Note.__</span></span> <span data-ttu-id="00c91-251">Die Tatsache, dass die inneren geschachtelten Werttypen nicht kopiert werden, ist eine Einschränkung des .net-Typsystems, um sicherzustellen, dass alle inneren geschachtelten Werttypen kopiert wurden, wenn ein Wert des Typs `Object` kopiert wurde.</span><span class="sxs-lookup"><span data-stu-id="00c91-251">The fact that inner boxed value types are not copied is a limitation of the .NET type system -- to ensure that all inner boxed value types were copied whenever a value of type `Object` was copied would be prohibitively expensive.</span></span>

<span data-ttu-id="00c91-252">Wie bereits beschrieben, können geschachtelte Werttypen nur in ihren ursprünglichen Typ entpackt werden.</span><span class="sxs-lookup"><span data-stu-id="00c91-252">As previously described, boxed value types can only be unboxed to their original type.</span></span> <span data-ttu-id="00c91-253">Gekapselte primitive Typen werden jedoch besonders behandelt, wenn Sie als `Object`eingegeben werden.</span><span class="sxs-lookup"><span data-stu-id="00c91-253">Boxed primitive types, however, are treated specially when typed as `Object`.</span></span> <span data-ttu-id="00c91-254">Sie können in beliebige andere primitive Typen konvertiert werden, in die Sie konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="00c91-254">They can be converted to any other primitive type that they have a conversion to.</span></span> <span data-ttu-id="00c91-255">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="00c91-255">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim o As Object = 5
        Dim b As Byte = CByte(o)  ' Legal
        Console.WriteLine(b) ' Prints 5
    End Sub
End Module
```

<span data-ttu-id="00c91-256">In der Regel konnte der eingepackte `Integer` Wert `5` nicht in eine `Byte` Variable entpackt werden.</span><span class="sxs-lookup"><span data-stu-id="00c91-256">Normally, the boxed `Integer` value `5` could not be unboxed into a `Byte` variable.</span></span> <span data-ttu-id="00c91-257">Da `Integer` und `Byte` primitive Typen sind und über eine Konvertierung verfügen, ist die Konvertierung zulässig.</span><span class="sxs-lookup"><span data-stu-id="00c91-257">However, because `Integer` and `Byte` are primitive types and have a conversion, the conversion is allowed.</span></span>

<span data-ttu-id="00c91-258">Es ist wichtig zu beachten, dass die typumrechnung in eine Schnittstelle von einem generischen Argument abweicht, das auf eine Schnittstelle beschränkt ist.</span><span class="sxs-lookup"><span data-stu-id="00c91-258">It is important to note that converting a value type to an interface is different than a generic argument constrained to an interface.</span></span> <span data-ttu-id="00c91-259">Beim Zugriff auf Schnittstellenmember bei einem eingeschränkten Typparameter (oder beim Aufrufen von Methoden auf `Object`) tritt kein Boxing auf, wie dies beim Konvertieren eines Werttyps in eine Schnittstelle und beim Zugriff auf einen Schnittstellenmember der Fall ist.</span><span class="sxs-lookup"><span data-stu-id="00c91-259">When accessing interface members on a constrained type parameter (or calling methods on `Object`), boxing does not occur as it does when a value type is converted to an interface and an interface member is accessed.</span></span> <span data-ttu-id="00c91-260">Nehmen wir beispielsweise an, dass eine Schnittstelle `ICounter` eine-Methode enthält `Increment` die zum Ändern eines Werts verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="00c91-260">For example, suppose an interface `ICounter` contains a method `Increment` which can be used to modify a value.</span></span> <span data-ttu-id="00c91-261">Wenn `ICounter` als Einschränkung verwendet wird, wird die Implementierung der `Increment`-Methode mit einem Verweis auf die Variable aufgerufen, für die `Increment` aufgerufen wurde, und nicht mit einer geboxten Kopie:</span><span class="sxs-lookup"><span data-stu-id="00c91-261">If `ICounter` is used as a constraint, the implementation of the `Increment` method is called with a reference to the variable that `Increment` was called on, not a boxed copy:</span></span>

```vb
Interface ICounter
    Sub Increment()
    ReadOnly Property Value() As Integer
End Interface

Structure Counter
    Implements ICounter

    Dim _value As Integer

    Property Value() As Integer Implements ICounter.Value
        Get
            Return _value
        End Get
    End Property

    Sub Increment() Implements ICounter.Increment
       value += 1
    End Sub
End Structure

Module Test
      Sub Test(Of T As ICounter)(x As T)
         Console.WriteLine(x.value)
         x.Increment()                     ' Modify x
         Console.WriteLine(x.value)
         CType(x, ICounter).Increment()    ' Modify boxed copy of x
         Console.WriteLine(x.value)
      End Sub

      Sub Main()
         Dim x As Counter
         Test(x)
      End Sub
End Module
```

<span data-ttu-id="00c91-262">Der erste `Increment`-Aufrufe ändert den Wert in der Variablen `x`.</span><span class="sxs-lookup"><span data-stu-id="00c91-262">The first call to `Increment` modifies the value in the variable `x`.</span></span> <span data-ttu-id="00c91-263">Dies entspricht nicht dem zweiten `Increment`-Aufrufwert, der den Wert in einer geachtelten Kopie von `x`ändert.</span><span class="sxs-lookup"><span data-stu-id="00c91-263">This is not equivalent to the second call to `Increment`, which modifies the value in a boxed copy of `x`.</span></span> <span data-ttu-id="00c91-264">Folglich lautet die Ausgabe des Programms wie folgt:</span><span class="sxs-lookup"><span data-stu-id="00c91-264">Thus, the output of the program is:</span></span>

```console
0
1
1
```

### <a name="nullable-value-type-conversions"></a><span data-ttu-id="00c91-265">Typkonvertierungen, die NULL zulassen</span><span class="sxs-lookup"><span data-stu-id="00c91-265">Nullable Value Type Conversions</span></span>

<span data-ttu-id="00c91-266">Ein Werttyp `T` kann in die und von der Werte zulässt-Version des Typs konvertieren, `T?`.</span><span class="sxs-lookup"><span data-stu-id="00c91-266">A value type `T` can convert to and from the nullable version of the type, `T?`.</span></span> <span data-ttu-id="00c91-267">Bei der Konvertierung von `T?` in `T` wird eine `System.InvalidOperationException` Ausnahme ausgelöst, wenn der zu konvertierende Wert `Nothing`ist.</span><span class="sxs-lookup"><span data-stu-id="00c91-267">The conversion from `T?` to `T` throws a `System.InvalidOperationException` exception if the value being converted is `Nothing`.</span></span> <span data-ttu-id="00c91-268">Außerdem wird `T?` eine Konvertierung in einen Typ `S`, wenn `T` eine systeminterne Konvertierung in `S`hat.</span><span class="sxs-lookup"><span data-stu-id="00c91-268">Also, `T?` has a conversion to a type `S` if `T` has an intrinsic conversion to `S`.</span></span> <span data-ttu-id="00c91-269">Wenn `S` ein Werttyp ist, sind die folgenden systeminternen Konvertierungen zwischen `T?` und `S?`vorhanden:</span><span class="sxs-lookup"><span data-stu-id="00c91-269">And if `S` is a value type, then the following intrinsic conversions exist between `T?` and `S?`:</span></span>

* <span data-ttu-id="00c91-270">Eine Konvertierung der gleichen Klassifizierung (Einschränkung oder Erweiterung) von `T?` in `S?`.</span><span class="sxs-lookup"><span data-stu-id="00c91-270">A conversion of the same classification (narrowing or widening) from `T?` to `S?`.</span></span>

* <span data-ttu-id="00c91-271">Eine Konvertierung der gleichen Klassifizierung (Einschränkung oder Erweiterung) von `T` in `S?`.</span><span class="sxs-lookup"><span data-stu-id="00c91-271">A conversion of the same classification (narrowing or widening) from `T` to `S?`.</span></span>

* <span data-ttu-id="00c91-272">Eine einschränkende Konvertierung von `S?` in `T`.</span><span class="sxs-lookup"><span data-stu-id="00c91-272">A narrowing conversion from `S?` to `T`.</span></span>

<span data-ttu-id="00c91-273">Beispielsweise ist eine systeminterne erweiternde Konvertierung von `Integer?` zu `Long?` vorhanden, da eine systeminterne erweiternde Konvertierung von `Integer` zum `Long`besteht:</span><span class="sxs-lookup"><span data-stu-id="00c91-273">For example, an intrinsic widening conversion exists from `Integer?` to `Long?` because an intrinsic widening conversion exists from `Integer` to `Long`:</span></span>

```vb
Dim i As Integer? = 10
Dim l As Long? = i
```

<span data-ttu-id="00c91-274">Wenn beim Umrechnen von `T?` in `S?`der Wert von `T?` `Nothing`ist, wird der Wert `S?` `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="00c91-274">When converting from `T?` to `S?`, if the value of `T?` is `Nothing`, then the value of `S?` will be `Nothing`.</span></span> <span data-ttu-id="00c91-275">Wenn der Wert `T?` oder `S?` `Nothing`ist, wird beim Umrechnen von `S?` in `T` oder `T?` in `S`eine `System.InvalidCastException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="00c91-275">When converting from `S?` to `T` or `T?` to `S`, if the value of `T?` or `S?` is `Nothing`, a `System.InvalidCastException` exception will be thrown.</span></span>

<span data-ttu-id="00c91-276">Aufgrund des Verhaltens des zugrunde liegenden Typs `System.Nullable(Of T)`ist das Ergebnis, wenn ein auf NULL festleg barer Werttyp `T?` ist, ein geachtelter Wert vom Typ `T`, kein geachtelter Wert vom Typ `T?`.</span><span class="sxs-lookup"><span data-stu-id="00c91-276">Because of the behavior of the underlying type `System.Nullable(Of T)`, when a nullable value type `T?` is boxed, the result is a boxed value of type `T`, not a boxed value of type `T?`.</span></span> <span data-ttu-id="00c91-277">Und umgekehrt wird beim Unboxing in einen Werte zulässt-Werttyp `T?`der Wert von `System.Nullable(Of T)`umschließt und `Nothing` in einen NULL-Wert des Typs `T?`entpackt werden.</span><span class="sxs-lookup"><span data-stu-id="00c91-277">And, conversely, when unboxing to a nullable value type `T?`, the value will be wrapped by `System.Nullable(Of T)`, and `Nothing` will be unboxed to a null value of type `T?`.</span></span> <span data-ttu-id="00c91-278">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="00c91-278">For example:</span></span>

```vb
Dim i1? As Integer = Nothing
Dim o1 As Object = i1

Console.WriteLine(o1 Is Nothing)                    ' Will print True
o1 = 10
i1 = CType(o1, Integer?)
Console.WriteLine(i1)                               ' Will print 10
```

<span data-ttu-id="00c91-279">Ein Nebeneffekt dieses Verhaltens besteht darin, dass ein Werttyp, der NULL-Werte zulässt `T?` scheinbar alle Schnittstellen `T`implementiert, da der Typ für die Typumwandlung in eine Schnittstelle konvertiert werden muss.</span><span class="sxs-lookup"><span data-stu-id="00c91-279">A side effect of this behavior is that a nullable value type `T?` appears to implement all of the interfaces of `T`, because converting a value type to an interface requires the type to be boxed.</span></span> <span data-ttu-id="00c91-280">Folglich kann `T?` in alle Schnittstellen konvertiert werden, in die `T` konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="00c91-280">As a result, `T?` is convertible to all the interfaces that `T` is convertible to.</span></span> <span data-ttu-id="00c91-281">Es ist jedoch wichtig zu beachten, dass ein auf NULL festleg barer Werttyp `T?` die Schnittstellen von `T` nicht zum Zweck der generischen Einschränkungs Überprüfung oder Reflektion implementiert.</span><span class="sxs-lookup"><span data-stu-id="00c91-281">It is important to note, however, that a nullable value type `T?` does not actually implement the interfaces of `T` for the purposes of generic constraint checking or reflection.</span></span> <span data-ttu-id="00c91-282">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="00c91-282">For example:</span></span>

```vb
Interface I1
End Interface

Structure T1
    Implements I1
    ...
End Structure

Module Test
    Sub M1(Of T As I1)(ByVal x As T)
    End Sub

    Sub Main()
        Dim x? As T1 = Nothing
        Dim y As I1 = x                ' Valid
        M1(x)                          ' Error: x? does not satisfy I1 constraint
    End Sub
End Module
```

## <a name="string-conversions"></a><span data-ttu-id="00c91-283">Zeichen folgen Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="00c91-283">String Conversions</span></span>

<span data-ttu-id="00c91-284">Das `Char` in `String` führt zu einer Zeichenfolge, deren erstes Zeichen der Zeichen Wert ist.</span><span class="sxs-lookup"><span data-stu-id="00c91-284">Converting `Char` into `String` results in a string whose first character is the character value.</span></span> <span data-ttu-id="00c91-285">Wenn `String` in `Char` umgewandelt wird, ergibt sich ein Zeichen, dessen Wert das erste Zeichen der Zeichenfolge ist.</span><span class="sxs-lookup"><span data-stu-id="00c91-285">Converting `String` into `Char` results in a character whose value is the first character of the string.</span></span> <span data-ttu-id="00c91-286">Wenn ein Array von `Char` in `String` umgewandelt wird, ergibt sich eine Zeichenfolge, deren Zeichen die Elemente des Arrays sind.</span><span class="sxs-lookup"><span data-stu-id="00c91-286">Converting an array of `Char` into `String` results in a string whose characters are the elements of the array.</span></span> <span data-ttu-id="00c91-287">Wenn `String` in ein Array von `Char` umgewandelt wird, ergibt sich ein Zeichen Array, dessen Elemente die Zeichen der Zeichenfolge sind.</span><span class="sxs-lookup"><span data-stu-id="00c91-287">Converting `String` into an array of `Char` results in an array of characters whose elements are the characters of the string.</span></span>

<span data-ttu-id="00c91-288">Die genauen Konvertierungen zwischen `String` und `Boolean`, `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`, `Date`und umgekehrt überschreiten den Rahmen dieser Spezifikation und sind durch eine Ausnahme von einem Detail abhängig.</span><span class="sxs-lookup"><span data-stu-id="00c91-288">The exact conversions between `String` and `Boolean`, `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`, `Date`, and vice versa, are beyond the scope of this specification and are implementation dependent with the exception of one detail.</span></span> <span data-ttu-id="00c91-289">Zeichen folgen Konvertierungen sollten immer die aktuelle Kultur der Laufzeitumgebung in Erwägung gezogen werden.</span><span class="sxs-lookup"><span data-stu-id="00c91-289">String conversions always consider the current culture of the run-time environment.</span></span> <span data-ttu-id="00c91-290">Daher müssen Sie zur Laufzeit ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="00c91-290">As such, they must be performed at run time.</span></span>

## <a name="widening-conversions"></a><span data-ttu-id="00c91-291">Erweiternde Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="00c91-291">Widening Conversions</span></span>

<span data-ttu-id="00c91-292">Erweiternde Konvertierungen nie Überlauf, können aber einen Genauigkeits Verlust verursachen.</span><span class="sxs-lookup"><span data-stu-id="00c91-292">Widening conversions never overflow but may entail a loss of precision.</span></span> <span data-ttu-id="00c91-293">Die folgenden Konvertierungen sind erweiternde Konvertierungen:</span><span class="sxs-lookup"><span data-stu-id="00c91-293">The following conversions are widening conversions:</span></span>

<span data-ttu-id="00c91-294">__Identitäts-/Standardkonvertierungen__</span><span class="sxs-lookup"><span data-stu-id="00c91-294">__Identity/Default conversions__</span></span>

* <span data-ttu-id="00c91-295">Von einem Typ in sich selbst.</span><span class="sxs-lookup"><span data-stu-id="00c91-295">From a type to itself.</span></span>

* <span data-ttu-id="00c91-296">Von einem anonymen Delegattyp, der für die Neuklassifizierung einer Lambda-Methode generiert wurde, in einen beliebigen Delegattyp mit einer identischen Signatur.</span><span class="sxs-lookup"><span data-stu-id="00c91-296">From an anonymous delegate type generated for a lambda method reclassification to any delegate type with an identical signature.</span></span>

* <span data-ttu-id="00c91-297">Vom Literal`Nothing` bis zu einem Typ.</span><span class="sxs-lookup"><span data-stu-id="00c91-297">From the literal `Nothing` to a type.</span></span>

<span data-ttu-id="00c91-298">__Numerische Konvertierungen__</span><span class="sxs-lookup"><span data-stu-id="00c91-298">__Numeric conversions__</span></span>

* <span data-ttu-id="00c91-299">Von `Byte` bis `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`oder `Double`.</span><span class="sxs-lookup"><span data-stu-id="00c91-299">From `Byte` to `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="00c91-300">Von `SByte` bis `Short`, `Integer`, `Long`, `Decimal`, `Single`oder `Double`.</span><span class="sxs-lookup"><span data-stu-id="00c91-300">From `SByte` to `Short`, `Integer`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="00c91-301">Von `UShort` bis `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`oder `Double`.</span><span class="sxs-lookup"><span data-stu-id="00c91-301">From `UShort` to `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="00c91-302">Von `Short` bis `Integer`, `Long`, `Decimal`, `Single` oder `Double`.</span><span class="sxs-lookup"><span data-stu-id="00c91-302">From `Short` to `Integer`, `Long`, `Decimal`, `Single` or `Double`.</span></span>

* <span data-ttu-id="00c91-303">Von `UInteger` bis `ULong`, `Long`, `Decimal`, `Single`oder `Double`.</span><span class="sxs-lookup"><span data-stu-id="00c91-303">From `UInteger` to `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="00c91-304">Von `Integer` bis `Long`, `Decimal`, `Single` oder `Double`.</span><span class="sxs-lookup"><span data-stu-id="00c91-304">From `Integer` to `Long`, `Decimal`, `Single` or `Double`.</span></span>

* <span data-ttu-id="00c91-305">Von `ULong` bis `Decimal`, `Single`oder `Double`.</span><span class="sxs-lookup"><span data-stu-id="00c91-305">From `ULong` to `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="00c91-306">Von `Long` bis `Decimal`, `Single` oder `Double`.</span><span class="sxs-lookup"><span data-stu-id="00c91-306">From `Long` to `Decimal`, `Single` or `Double`.</span></span>

* <span data-ttu-id="00c91-307">Von `Decimal` bis `Single` oder `Double`.</span><span class="sxs-lookup"><span data-stu-id="00c91-307">From `Decimal` to `Single` or `Double`.</span></span>

* <span data-ttu-id="00c91-308">Von `Single` bis `Double`.</span><span class="sxs-lookup"><span data-stu-id="00c91-308">From `Single` to `Double`.</span></span>

* <span data-ttu-id="00c91-309">Vom Literal`0` bis zu einem enumerierten Typ.</span><span class="sxs-lookup"><span data-stu-id="00c91-309">From the literal `0` to an enumerated type.</span></span> <span data-ttu-id="00c91-310">(__Hinweis:__</span><span class="sxs-lookup"><span data-stu-id="00c91-310">(__Note.__</span></span> <span data-ttu-id="00c91-311">Die Konvertierung von `0` in einen beliebigen Enumerationstyp wird erweitert, um testflags zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="00c91-311">The conversion from `0` to any enumerated type is widening to simplify testing flags.</span></span> <span data-ttu-id="00c91-312">Wenn `Values` z. b. ein Enumerationstyp mit einem Wert `One`ist, können Sie eine Variable `v` vom Typ `Values` testen, indem Sie `(v And Values.One) = 0`sagen.)</span><span class="sxs-lookup"><span data-stu-id="00c91-312">For example, if `Values` is an enumerated type with a value `One`, you could test a variable `v` of type `Values` by saying `(v And Values.One) = 0`.)</span></span>

* <span data-ttu-id="00c91-313">Von einem enumerierten Typ zum zugrunde liegenden numerischen Typ oder zu einem numerischen Typ, für den der zugrunde liegende numerische Typ eine erweiternde Konvertierung in aufweist.</span><span class="sxs-lookup"><span data-stu-id="00c91-313">From an enumerated type to its underlying numeric type, or to a numeric type that its underlying numeric type has a widening conversion to.</span></span>

* <span data-ttu-id="00c91-314">Aus einem konstanten Ausdruck vom Typ `ULong`, `Long`, `UInteger`, `Integer`, `UShort`, `Short`, `Byte`oder `SByte` zu einem engeren Typ, wenn der Wert des konstanten Ausdrucks innerhalb des Bereichs des Zieltyps liegt.</span><span class="sxs-lookup"><span data-stu-id="00c91-314">From a constant expression of type `ULong`, `Long`, `UInteger`, `Integer`, `UShort`, `Short`, `Byte`, or `SByte` to a narrower type, provided the value of the constant expression is within the range of the destination type.</span></span> <span data-ttu-id="00c91-315">(__Hinweis:__</span><span class="sxs-lookup"><span data-stu-id="00c91-315">(__Note.__</span></span> <span data-ttu-id="00c91-316">Konvertierungen von `UInteger` oder `Integer` in `Single`, `ULong` oder `Long` `Single` oder `Double`oder `Decimal` `Single` oder `Double` können zu einem Genauigkeits Verlust führen. Dies führt jedoch nie zu einem Verlust der Größe.</span><span class="sxs-lookup"><span data-stu-id="00c91-316">Conversions from `UInteger` or `Integer` to `Single`, `ULong` or `Long` to `Single` or `Double`, or `Decimal` to `Single` or `Double` may cause a loss of precision, but will never cause a loss of magnitude.</span></span> <span data-ttu-id="00c91-317">Die anderen erweiternden numerischen Konvertierungen verlieren niemals Informationen.)</span><span class="sxs-lookup"><span data-stu-id="00c91-317">The other widening numeric conversions never lose any information.)</span></span>

<span data-ttu-id="00c91-318">__Verweis Konvertierungen__</span><span class="sxs-lookup"><span data-stu-id="00c91-318">__Reference conversions__</span></span>

* <span data-ttu-id="00c91-319">Von einem Referenztyp zu einem Basistyp.</span><span class="sxs-lookup"><span data-stu-id="00c91-319">From a reference type to a base type.</span></span>

* <span data-ttu-id="00c91-320">Von einem Referenztyp zu einem Schnittstellentyp, vorausgesetzt, dass der Typ die-Schnittstelle oder eine Variant-kompatible Schnittstelle implementiert.</span><span class="sxs-lookup"><span data-stu-id="00c91-320">From a reference type to an interface type, provided that the type implements the interface or a variant compatible interface.</span></span>

* <span data-ttu-id="00c91-321">Von einem Schnittstellentyp zu `Object`.</span><span class="sxs-lookup"><span data-stu-id="00c91-321">From an interface type to `Object`.</span></span>

* <span data-ttu-id="00c91-322">Von einem Schnittstellentyp zu einem Variant-kompatiblen Schnittstellentyp.</span><span class="sxs-lookup"><span data-stu-id="00c91-322">From an interface type to a variant compatible interface type.</span></span>

* <span data-ttu-id="00c91-323">Von einem Delegattyp zu einem Variant-kompatiblen Delegattyp.</span><span class="sxs-lookup"><span data-stu-id="00c91-323">From a delegate type to a variant compatible delegate type.</span></span> <span data-ttu-id="00c91-324">(__Hinweis:__</span><span class="sxs-lookup"><span data-stu-id="00c91-324">(__Note.__</span></span> <span data-ttu-id="00c91-325">Viele andere Verweis Konvertierungen werden von diesen Regeln impliziert.</span><span class="sxs-lookup"><span data-stu-id="00c91-325">Many other reference conversions are implied by these rules.</span></span> <span data-ttu-id="00c91-326">Anonyme Delegaten sind z. b. Referenztypen, die von `System.MulticastDelegate`erben; Array Typen sind Verweis Typen, die von `System.Array`erben; anonyme Typen sind Verweis Typen, die von `System.Object`erben.)</span><span class="sxs-lookup"><span data-stu-id="00c91-326">For example, anonymous delegates are reference types that inherit from `System.MulticastDelegate`; array types are reference types that inherit from `System.Array`; anonymous types are reference types that inherit from `System.Object`.)</span></span>

<span data-ttu-id="00c91-327">__Anonyme delegatkonvertierungen__</span><span class="sxs-lookup"><span data-stu-id="00c91-327">__Anonymous Delegate conversions__</span></span>

* <span data-ttu-id="00c91-328">Von einem anonymen Delegattyp, der für die Neuklassifizierung einer Lambda-Methode generiert wird, zu einem beliebigen breiteren Delegattyp.</span><span class="sxs-lookup"><span data-stu-id="00c91-328">From an anonymous delegate type generated for a lambda method reclassification to any wider delegate type.</span></span>

<span data-ttu-id="00c91-329">__Array Konvertierungen__</span><span class="sxs-lookup"><span data-stu-id="00c91-329">__Array conversions__</span></span>

* <span data-ttu-id="00c91-330">Von einem Arraytyp `S` mit einem Elementtyp `Se` zu einem Arraytyp, der mit einem Elementtyp `Te``T` wird, wenn Folgendes zutrifft:</span><span class="sxs-lookup"><span data-stu-id="00c91-330">From an array type `S` with an element type `Se` to an array type `T` with an element type `Te`, provided all of the following are true:</span></span>

  * <span data-ttu-id="00c91-331">`S` und `T` unterscheiden sich nur in Elementtyp.</span><span class="sxs-lookup"><span data-stu-id="00c91-331">`S` and `T` differ only in element type.</span></span>

  * <span data-ttu-id="00c91-332">Sowohl `Se` als auch `Te` sind Verweis Typen oder sind Typparameter, die als Verweistyp bekannt sind.</span><span class="sxs-lookup"><span data-stu-id="00c91-332">Both `Se` and `Te` are reference types or are type parameters known to be a reference type.</span></span>

  * <span data-ttu-id="00c91-333">Eine erweiternde Verweis-, Array-oder Typparameter Konvertierung ist von `Se` in `Te`vorhanden.</span><span class="sxs-lookup"><span data-stu-id="00c91-333">A widening reference, array, or type parameter conversion exists from `Se` to `Te`.</span></span>

* <span data-ttu-id="00c91-334">Von einem Arraytyp `S` mit einem enumerierten Elementtyp `Se` zu einem Arraytyp, der mit einem Elementtyp `Te``T` ist, wenn alle folgenden Punkte zutreffen:</span><span class="sxs-lookup"><span data-stu-id="00c91-334">From an array type `S` with an enumerated element type `Se` to an array type `T` with an element type `Te`, provided all of the following are true:</span></span>

  * <span data-ttu-id="00c91-335">`S` und `T` unterscheiden sich nur in Elementtyp.</span><span class="sxs-lookup"><span data-stu-id="00c91-335">`S` and `T` differ only in element type.</span></span>

  * <span data-ttu-id="00c91-336">`Te` ist der zugrunde liegende Typ `Se`.</span><span class="sxs-lookup"><span data-stu-id="00c91-336">`Te` is the underlying type of `Se`.</span></span>

* <span data-ttu-id="00c91-337">Von einem Arraytyp `S` von Rang 1 mit einem enumerierten Elementtyp `Se`zum `System.Collections.Generic.IList(Of Te)`, `IReadOnlyList(Of Te)`, `ICollection(Of Te)`, `IReadOnlyCollection(Of Te)`und `IEnumerable(Of Te)`wird eine der folgenden Angaben angezeigt:</span><span class="sxs-lookup"><span data-stu-id="00c91-337">From an array type `S` of rank 1 with an enumerated element type `Se`, to `System.Collections.Generic.IList(Of Te)`, `IReadOnlyList(Of Te)`, `ICollection(Of Te)`, `IReadOnlyCollection(Of Te)`, and `IEnumerable(Of Te)`, provided one of the following is true:</span></span>

  * <span data-ttu-id="00c91-338">Sowohl `Se` als auch `Te` sind Verweis Typen, oder sind Typparameter, die als Verweistyp bekannt sind, und ein erweiternde Verweis-, Array-oder Typparameter Konvertierung ist von `Se` in `Te`vorhanden. noch</span><span class="sxs-lookup"><span data-stu-id="00c91-338">Both `Se` and `Te` are reference types or are type parameters known to be a reference type, and a widening reference, array, or type parameter conversion exists from `Se` to `Te`; or</span></span>

  * <span data-ttu-id="00c91-339">`Te` ist der zugrunde liegende Typ `Se`. noch</span><span class="sxs-lookup"><span data-stu-id="00c91-339">`Te` is the underlying type of `Se`; or</span></span>

  * <span data-ttu-id="00c91-340">`Te` ist identisch mit `Se`</span><span class="sxs-lookup"><span data-stu-id="00c91-340">`Te` is identical to `Se`</span></span>

<span data-ttu-id="00c91-341">__Werttyp Konvertierungen__</span><span class="sxs-lookup"><span data-stu-id="00c91-341">__Value Type conversions__</span></span>

* <span data-ttu-id="00c91-342">Von einem Werttyp zu einem Basistyp.</span><span class="sxs-lookup"><span data-stu-id="00c91-342">From a value type to a base type.</span></span>

* <span data-ttu-id="00c91-343">Von einem Werttyp zu einem Schnittstellentyp, den der Typ implementiert.</span><span class="sxs-lookup"><span data-stu-id="00c91-343">From a value type to an interface type that the type implements.</span></span>

<span data-ttu-id="00c91-344">__Typkonvertierungen, die NULL zulassen__</span><span class="sxs-lookup"><span data-stu-id="00c91-344">__Nullable Value Type conversions__</span></span>

* <span data-ttu-id="00c91-345">Von einem Typ `T` zum Typ `T?`.</span><span class="sxs-lookup"><span data-stu-id="00c91-345">From a type `T` to the type `T?`.</span></span>

* <span data-ttu-id="00c91-346">Von einem Typ `T?` zu einem Typ `S?`, bei dem eine erweiternde Konvertierung vom Typ `T` in den Typ `S`erfolgt.</span><span class="sxs-lookup"><span data-stu-id="00c91-346">From a type `T?` to a type `S?`, where there is a widening conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="00c91-347">Von einem Typ `T` zu einem Typ `S?`, bei dem eine erweiternde Konvertierung vom Typ `T` in den Typ `S`erfolgt.</span><span class="sxs-lookup"><span data-stu-id="00c91-347">From a type `T` to a type `S?`, where there is a widening conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="00c91-348">Von einem-Typ `T?` zu einem Schnittstellentyp, den der Typ `T` implementiert.</span><span class="sxs-lookup"><span data-stu-id="00c91-348">From a type `T?` to an interface type that the type `T` implements.</span></span>

<span data-ttu-id="00c91-349">__Zeichen folgen Konvertierungen__</span><span class="sxs-lookup"><span data-stu-id="00c91-349">__String conversions__</span></span>

* <span data-ttu-id="00c91-350">Von `Char` bis `String`.</span><span class="sxs-lookup"><span data-stu-id="00c91-350">From `Char` to `String`.</span></span>

* <span data-ttu-id="00c91-351">Von `Char()` bis `String`.</span><span class="sxs-lookup"><span data-stu-id="00c91-351">From `Char()` to `String`.</span></span>

<span data-ttu-id="00c91-352">__Typparameter Konvertierungen__</span><span class="sxs-lookup"><span data-stu-id="00c91-352">__Type Parameter conversions__</span></span>

* <span data-ttu-id="00c91-353">Von einem Typparameter zum `Object`.</span><span class="sxs-lookup"><span data-stu-id="00c91-353">From a type parameter to `Object`.</span></span>

* <span data-ttu-id="00c91-354">Von einem Typparameter zu einer Schnittstellentyp Einschränkung oder einer beliebigen Schnittstellen Variante, die mit einer Schnittstellentyp Einschränkung kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="00c91-354">From a type parameter to an interface type constraint or any interface variant compatible with an interface type constraint.</span></span>

* <span data-ttu-id="00c91-355">Von einem Typparameter zu einer Schnittstelle, die von einer Klassen Einschränkung implementiert wird.</span><span class="sxs-lookup"><span data-stu-id="00c91-355">From a type parameter to an interface implemented by a class constraint.</span></span>

* <span data-ttu-id="00c91-356">Von einem Typparameter zu einer Schnittstellen Variante, die mit einer Schnittstelle kompatibel ist, die durch eine Klassen Einschränkung implementiert wird.</span><span class="sxs-lookup"><span data-stu-id="00c91-356">From a type parameter to an interface variant compatible with an interface implemented by a class constraint.</span></span>

* <span data-ttu-id="00c91-357">Von einem Typparameter zu einer Klassen Einschränkung oder einem Basistyp der Klassen Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="00c91-357">From a type parameter to a class constraint, or a base type of the class constraint.</span></span>

* <span data-ttu-id="00c91-358">Von einem Typparameter `T` zu einer Typparameter Einschränkung `Tx`, oder irgendetwas `Tx` über eine erweiternde Konvertierung zu verfügt.</span><span class="sxs-lookup"><span data-stu-id="00c91-358">From a type parameter `T` to a type parameter constraint `Tx`, or anything `Tx` has a widening conversion to.</span></span>

## <a name="narrowing-conversions"></a><span data-ttu-id="00c91-359">Einschränkende Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="00c91-359">Narrowing Conversions</span></span>

<span data-ttu-id="00c91-360">Einschränkende Konvertierungen sind Konvertierungen, die nicht als immer erfolgreich erwiesen werden können, Konvertierungen, die bekanntermaßen Informationen verlieren, und Konvertierungen zwischen verschiedenen Domänen, die sich ausreichend von der einschränkenden Schreibweise unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="00c91-360">Narrowing conversions are conversions that cannot be proved to always succeed, conversions that are known to possibly lose information, and conversions across domains of types sufficiently different to merit narrowing notation.</span></span> <span data-ttu-id="00c91-361">Die folgenden Konvertierungen werden als einschränkende Konvertierungen klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="00c91-361">The following conversions are classified as narrowing conversions:</span></span>

<span data-ttu-id="00c91-362">__Boolesche Konvertierungen__</span><span class="sxs-lookup"><span data-stu-id="00c91-362">__Boolean conversions__</span></span>

* <span data-ttu-id="00c91-363">Von `Boolean` bis `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`oder `Double`.</span><span class="sxs-lookup"><span data-stu-id="00c91-363">From `Boolean` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="00c91-364">Von `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`oder `Double` `Boolean`.</span><span class="sxs-lookup"><span data-stu-id="00c91-364">From `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double` to `Boolean`.</span></span>

<span data-ttu-id="00c91-365">__Numerische Konvertierungen__</span><span class="sxs-lookup"><span data-stu-id="00c91-365">__Numeric conversions__</span></span>

* <span data-ttu-id="00c91-366">Von `Byte` bis `SByte`.</span><span class="sxs-lookup"><span data-stu-id="00c91-366">From `Byte` to `SByte`.</span></span>

* <span data-ttu-id="00c91-367">Von `SByte` bis `Byte`, `UShort`, `UInteger`oder `ULong`.</span><span class="sxs-lookup"><span data-stu-id="00c91-367">From `SByte` to `Byte`, `UShort`, `UInteger`, or `ULong`.</span></span>

* <span data-ttu-id="00c91-368">Von `UShort` bis `Byte`, `SByte`oder `Short`.</span><span class="sxs-lookup"><span data-stu-id="00c91-368">From `UShort` to `Byte`, `SByte`, or `Short`.</span></span>

* <span data-ttu-id="00c91-369">Von `Short` bis `Byte`, `SByte`, `UShort`, `UInteger`oder `ULong`.</span><span class="sxs-lookup"><span data-stu-id="00c91-369">From `Short` to `Byte`, `SByte`, `UShort`, `UInteger`, or `ULong`.</span></span>

* <span data-ttu-id="00c91-370">Von `UInteger` bis `Byte`, `SByte`, `UShort`, `Short`oder `Integer`.</span><span class="sxs-lookup"><span data-stu-id="00c91-370">From `UInteger` to `Byte`, `SByte`, `UShort`, `Short`, or `Integer`.</span></span>

* <span data-ttu-id="00c91-371">Von `Integer` bis `Byte`, `SByte`, `UShort`, `Short`, `UInteger`oder `ULong`.</span><span class="sxs-lookup"><span data-stu-id="00c91-371">From `Integer` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, or `ULong`.</span></span>

* <span data-ttu-id="00c91-372">Von `ULong` bis `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`oder `Long`.</span><span class="sxs-lookup"><span data-stu-id="00c91-372">From `ULong` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, or `Long`.</span></span>

* <span data-ttu-id="00c91-373">Von `Long` bis `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`oder `ULong`.</span><span class="sxs-lookup"><span data-stu-id="00c91-373">From `Long` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, or `ULong`.</span></span>

* <span data-ttu-id="00c91-374">Von `Decimal` bis `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`oder `Long`.</span><span class="sxs-lookup"><span data-stu-id="00c91-374">From `Decimal` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, or `Long`.</span></span>

* <span data-ttu-id="00c91-375">Von `Single` bis `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`oder `Decimal`.</span><span class="sxs-lookup"><span data-stu-id="00c91-375">From `Single` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, or `Decimal`.</span></span>

* <span data-ttu-id="00c91-376">Von `Double` bis `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`oder `Single`.</span><span class="sxs-lookup"><span data-stu-id="00c91-376">From `Double` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, or `Single`.</span></span>

* <span data-ttu-id="00c91-377">Von einem numerischen Typ zu einem enumerierten Typ.</span><span class="sxs-lookup"><span data-stu-id="00c91-377">From a numeric type to an enumerated type.</span></span>

* <span data-ttu-id="00c91-378">Von einem enumerierten Typ zu einem numerischen Typ hat der zugrunde liegende numerische Typ eine einschränkende Konvertierung in.</span><span class="sxs-lookup"><span data-stu-id="00c91-378">From an enumerated type to a numeric type its underlying numeric type has a narrowing conversion to.</span></span>

* <span data-ttu-id="00c91-379">Von einem enumerierten Typ zu einem anderen Enumerationstyp.</span><span class="sxs-lookup"><span data-stu-id="00c91-379">From an enumerated type to another enumerated type.</span></span>

<span data-ttu-id="00c91-380">__Verweis Konvertierungen__</span><span class="sxs-lookup"><span data-stu-id="00c91-380">__Reference conversions__</span></span>

* <span data-ttu-id="00c91-381">Von einem Referenztyp zu einem stärker abgeleiteten Typ.</span><span class="sxs-lookup"><span data-stu-id="00c91-381">From a reference type to a more derived type.</span></span>

* <span data-ttu-id="00c91-382">Von einem Klassentyp zu einem Schnittstellentyp, wenn der Klassentyp den Schnittstellentyp oder eine mit diesem kompatible Schnittstellentyp Variante nicht implementiert.</span><span class="sxs-lookup"><span data-stu-id="00c91-382">From a class type to an interface type, provided the class type does not implement the interface type or an interface type variant compatible with it.</span></span>

* <span data-ttu-id="00c91-383">Von einem Schnittstellentyp zu einem Klassentyp.</span><span class="sxs-lookup"><span data-stu-id="00c91-383">From an interface type to a class type.</span></span>

* <span data-ttu-id="00c91-384">Von einem Schnittstellentyp zu einem anderen Schnittstellentyp, sofern keine Vererbungs Beziehung zwischen den beiden Typen besteht und vorausgesetzt, dass Sie nicht Variant-kompatibel sind.</span><span class="sxs-lookup"><span data-stu-id="00c91-384">From an interface type to another interface type, provided there is no inheritance relationship between the two types and provided they are not variant compatible.</span></span>

<span data-ttu-id="00c91-385">__Anonyme delegatkonvertierungen__</span><span class="sxs-lookup"><span data-stu-id="00c91-385">__Anonymous Delegate conversions__</span></span>

* <span data-ttu-id="00c91-386">Von einem anonymen Delegattyp, der für die Neuklassifizierung einer Lambda-Methode generiert wurde, in einen beliebigen engeren Delegattyp.</span><span class="sxs-lookup"><span data-stu-id="00c91-386">From an anonymous delegate type generated for a lambda method reclassification to any narrower delegate type.</span></span>

<span data-ttu-id="00c91-387">__Array Konvertierungen__</span><span class="sxs-lookup"><span data-stu-id="00c91-387">__Array conversions__</span></span>

* <span data-ttu-id="00c91-388">Von einem Arraytyp `S` mit einem Elementtyp `Se`in einen Arraytyp, der mit einem Elementtyp `Te``T` ist, vorausgesetzt, dass alle folgenden Punkte zutreffen:</span><span class="sxs-lookup"><span data-stu-id="00c91-388">From an array type `S` with an element type `Se`, to an array type `T` with an element type `Te`, provided that all of the following are true:</span></span>

  * <span data-ttu-id="00c91-389">`S` und `T` unterscheiden sich nur in Elementtyp.</span><span class="sxs-lookup"><span data-stu-id="00c91-389">`S` and `T` differ only in element type.</span></span>
  * <span data-ttu-id="00c91-390">Sowohl `Se` als auch `Te` sind Verweis Typen oder sind Typparameter, die keine Werttypen sind.</span><span class="sxs-lookup"><span data-stu-id="00c91-390">Both `Se` and `Te` are reference types or are type parameters not known to be value types.</span></span>
  * <span data-ttu-id="00c91-391">Eine einschränkende Verweis-, Array-oder Typparameter Konvertierung ist von `Se` in `Te`vorhanden.</span><span class="sxs-lookup"><span data-stu-id="00c91-391">A narrowing reference, array, or type parameter conversion exists from `Se` to `Te`.</span></span>

* <span data-ttu-id="00c91-392">Von einem Arraytyp `S` mit einem Elementtyp `Se` zu einem Arraytyp, der mit einem enumerierten Elementtyp `Te``T`, wenn alle folgenden Punkte zutreffen:</span><span class="sxs-lookup"><span data-stu-id="00c91-392">From an array type `S` with an element type `Se` to an array type `T` with an enumerated element type `Te`, provided all of the following are true:</span></span>

  * <span data-ttu-id="00c91-393">`S` und `T` unterscheiden sich nur in Elementtyp.</span><span class="sxs-lookup"><span data-stu-id="00c91-393">`S` and `T` differ only in element type.</span></span>
  * <span data-ttu-id="00c91-394">`Se` ist der zugrunde liegende Typ `Te`, oder es handelt sich um unterschiedliche Enumerationstypen, die denselben zugrunde liegenden Typ haben.</span><span class="sxs-lookup"><span data-stu-id="00c91-394">`Se` is the underlying type of `Te` , or they are both different enumerated types that share the same underlying type.</span></span>

* <span data-ttu-id="00c91-395">Von einem Arraytyp `S` von Rang 1 mit einem enumerierten Elementtyp `Se`zum `IList(Of Te)`, `IReadOnlyList(Of Te)`, `ICollection(Of Te)`, `IReadOnlyCollection(Of Te)` und `IEnumerable(Of Te)`wird eine der folgenden Angaben angezeigt:</span><span class="sxs-lookup"><span data-stu-id="00c91-395">From an array type `S` of rank 1 with an enumerated element type `Se`, to `IList(Of Te)`, `IReadOnlyList(Of Te)`, `ICollection(Of Te)`, `IReadOnlyCollection(Of Te)` and `IEnumerable(Of Te)`, provided one of the following is true:</span></span>

  * <span data-ttu-id="00c91-396">Sowohl `Se` als auch `Te` sind Verweis Typen, oder sind Typparameter, die als Verweistyp bekannt sind, und eine einschränkende Verweis-, Array-oder Typparameter Konvertierung ist von `Se` in `Te`vorhanden. noch</span><span class="sxs-lookup"><span data-stu-id="00c91-396">Both `Se` and `Te` are reference types or are type parameters known to be a reference type, and a narrowing reference, array, or type parameter conversion exists from `Se` to `Te`; or</span></span>
  * <span data-ttu-id="00c91-397">`Se` ist der zugrunde liegende Typ `Te`, oder es handelt sich um unterschiedliche Enumerationstypen, die denselben zugrunde liegenden Typ haben.</span><span class="sxs-lookup"><span data-stu-id="00c91-397">`Se` is the underlying type of `Te`, or they are both different enumerated types that share the same underlying type.</span></span>

<span data-ttu-id="00c91-398">__Werttyp Konvertierungen__</span><span class="sxs-lookup"><span data-stu-id="00c91-398">__Value type conversions__</span></span>

* <span data-ttu-id="00c91-399">Von einem Referenztyp zu einem stärker abgeleiteten Werttyp.</span><span class="sxs-lookup"><span data-stu-id="00c91-399">From a reference type to a more derived value type.</span></span>

* <span data-ttu-id="00c91-400">Von einem Schnittstellentyp zu einem Werttyp, sofern der Werttyp den Schnittstellentyp implementiert.</span><span class="sxs-lookup"><span data-stu-id="00c91-400">From an interface type to a value type, provided the value type implements the interface type.</span></span>

<span data-ttu-id="00c91-401">__Typkonvertierungen, die NULL zulassen__</span><span class="sxs-lookup"><span data-stu-id="00c91-401">__Nullable Value Type conversions__</span></span>

* <span data-ttu-id="00c91-402">Von einem Typ `T?` auf einen Typ `T`.</span><span class="sxs-lookup"><span data-stu-id="00c91-402">From a type `T?` to a type `T`.</span></span>

* <span data-ttu-id="00c91-403">Von einem Typ `T?` zu einem Typ `S?`, bei dem eine einschränkende Konvertierung vom Typ `T` in den Typ `S`vorliegt.</span><span class="sxs-lookup"><span data-stu-id="00c91-403">From a type `T?` to a type `S?`, where there is a narrowing conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="00c91-404">Von einem Typ `T` zu einem Typ `S?`, bei dem eine einschränkende Konvertierung vom Typ `T` in den Typ `S`vorliegt.</span><span class="sxs-lookup"><span data-stu-id="00c91-404">From a type `T` to a type `S?`, where there is a narrowing conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="00c91-405">Von einem Typ `S?` zu einem Typ `T`, bei dem eine Konvertierung vom Typ `S` in den Typ `T`erfolgt.</span><span class="sxs-lookup"><span data-stu-id="00c91-405">From a type `S?` to a type `T`, where there is a conversion from the type `S` to the type `T`.</span></span>

<span data-ttu-id="00c91-406">__Zeichen folgen Konvertierungen__</span><span class="sxs-lookup"><span data-stu-id="00c91-406">__String conversions__</span></span>

* <span data-ttu-id="00c91-407">Von `String` bis `Char`.</span><span class="sxs-lookup"><span data-stu-id="00c91-407">From `String` to `Char`.</span></span>

* <span data-ttu-id="00c91-408">Von `String` bis `Char()`.</span><span class="sxs-lookup"><span data-stu-id="00c91-408">From `String` to `Char()`.</span></span>

* <span data-ttu-id="00c91-409">Von `String` bis `Boolean` und von `Boolean` bis `String`.</span><span class="sxs-lookup"><span data-stu-id="00c91-409">From `String` to `Boolean` and from `Boolean` to `String`.</span></span>

* <span data-ttu-id="00c91-410">Konvertierungen zwischen `String` und `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`oder `Double`.</span><span class="sxs-lookup"><span data-stu-id="00c91-410">Conversions between `String` and `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="00c91-411">Von `String` bis `Date` und von `Date` bis `String`.</span><span class="sxs-lookup"><span data-stu-id="00c91-411">From `String` to `Date` and from `Date` to `String`.</span></span>

<span data-ttu-id="00c91-412">__Typparameter Konvertierungen__</span><span class="sxs-lookup"><span data-stu-id="00c91-412">__Type Parameter conversions__</span></span>

* <span data-ttu-id="00c91-413">Von `Object` bis zu einem Typparameter.</span><span class="sxs-lookup"><span data-stu-id="00c91-413">From `Object` to a type parameter.</span></span>

* <span data-ttu-id="00c91-414">Von einem Typparameter zu einem Schnittstellentyp, wenn der Typparameter nicht auf diese Schnittstelle beschränkt ist oder nicht auf eine Klasse beschränkt ist, die diese Schnittstelle implementiert.</span><span class="sxs-lookup"><span data-stu-id="00c91-414">From a type parameter to an interface type, provided the type parameter is not constrained to that interface or constrained to a class that implements that interface.</span></span>

* <span data-ttu-id="00c91-415">Von einem Schnittstellentyp zu einem Typparameter.</span><span class="sxs-lookup"><span data-stu-id="00c91-415">From an interface type to a type parameter.</span></span>

* <span data-ttu-id="00c91-416">Von einem Typparameter zu einem abgeleiteten Typ einer Klassen Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="00c91-416">From a type parameter to a derived type of a class constraint.</span></span>

* <span data-ttu-id="00c91-417">Von einem Typparameter `T` in eine Typparameter Einschränkung, `Tx` eine einschränkende Konvertierung in aufweist.</span><span class="sxs-lookup"><span data-stu-id="00c91-417">From a type parameter `T` to anything a type parameter constraint `Tx` has a narrowing conversion to.</span></span>

## <a name="type-parameter-conversions"></a><span data-ttu-id="00c91-418">Typparameter Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="00c91-418">Type Parameter Conversions</span></span>

<span data-ttu-id="00c91-419">Typparameter Konvertierungen werden durch die Einschränkungen festgelegt, die ggf. für Sie festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="00c91-419">Type parameters' conversions are determined by the constraints, if any, put on them.</span></span> <span data-ttu-id="00c91-420">Ein Typparameter `T` kann immer in sich selbst, in `Object`und in einen beliebigen Schnittstellentyp konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="00c91-420">A type parameter `T` can always be converted to itself, to and from `Object`, and to and from any interface type.</span></span> <span data-ttu-id="00c91-421">Beachten Sie, dass, wenn der Typ `T` zur Laufzeit ein Werttyp ist, die Konvertierung von `T` in `Object` oder einen Schnittstellentyp eine Boxing-Konvertierung ist und die Konvertierung von `Object` oder einem Schnittstellentyp in `T` eine Unboxing-Konvertierung ist.</span><span class="sxs-lookup"><span data-stu-id="00c91-421">Note that if the type `T` is a value type at run-time, converting from `T` to `Object` or an interface type will be a boxing conversion and converting from `Object` or an interface type to `T` will be an unboxing conversion.</span></span> <span data-ttu-id="00c91-422">Ein Typparameter mit einer Klassen Einschränkungs `C` definiert zusätzliche Konvertierungen vom Typparameter in `C` und seine Basisklassen und umgekehrt.</span><span class="sxs-lookup"><span data-stu-id="00c91-422">A type parameter with a class constraint `C` defines additional conversions from the type parameter to `C` and its base classes, and vice versa.</span></span> <span data-ttu-id="00c91-423">Ein Typparameter, der mit einer Typparameter Einschränkung `T` `Tx` definiert eine Konvertierung in `Tx` und alle Elemente, die in `Tx` konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="00c91-423">A type parameter `T` with a type parameter constraint `Tx` defines a conversion to `Tx` and anything `Tx` converts to.</span></span>

<span data-ttu-id="00c91-424">Ein Array, dessen Elementtyp ein Typparameter mit einer Schnittstellen Einschränkung ist, `I` die gleichen kovariant-Array Konvertierungen wie ein Array mit dem Elementtyp `I`aufweisen, vorausgesetzt, dass der Typparameter auch eine `Class` oder eine Klassen Einschränkung hat (da nur Verweis Array Element-Typen kovariant sein können).</span><span class="sxs-lookup"><span data-stu-id="00c91-424">An array whose element type is a type parameter with an interface constraint `I` has the same covariant array conversions as an array whose element type is `I`, provided that the type parameter also has a `Class` or class constraint (since only reference array element types can be covariant).</span></span> <span data-ttu-id="00c91-425">Ein Array, dessen Elementtyp ein Typparameter mit einer Klassen Einschränkung ist `C` hat dieselben kovarianten Array Konvertierungen wie ein Array, dessen Elementtyp `C`ist.</span><span class="sxs-lookup"><span data-stu-id="00c91-425">An array whose element type is a type parameter with a class constraint `C` has the same covariant array conversions as an array whose element type is `C`.</span></span>

<span data-ttu-id="00c91-426">Die obigen Konvertierungsregeln erlauben keine Konvertierung von nicht eingeschränkten Typparametern in nicht-Schnittstellentypen, was über rasch sein kann.</span><span class="sxs-lookup"><span data-stu-id="00c91-426">The above conversions rules do not permit conversions from unconstrained type parameters to non-interface types, which may be surprising.</span></span> <span data-ttu-id="00c91-427">Der Grund hierfür ist die Vermeidung von Verwirrung bezüglich der Semantik solcher Konvertierungen.</span><span class="sxs-lookup"><span data-stu-id="00c91-427">The reason for this is to prevent confusion about the semantics of such conversions.</span></span> <span data-ttu-id="00c91-428">Betrachten Sie beispielsweise die folgende Deklaration:</span><span class="sxs-lookup"><span data-stu-id="00c91-428">For example, consider the following declaration:</span></span>

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long 
        Return CLng(t)    ' Error, explicit conversion not permitted
    End Function
End Class
```

<span data-ttu-id="00c91-429">Wenn die Konvertierung von `T` in `Integer` zulässig war, kann man leicht davon ausgehen, dass `X(Of Integer).F(7)` `7L`zurückgeben würde.</span><span class="sxs-lookup"><span data-stu-id="00c91-429">If the conversion of `T` to `Integer` were permitted, one might easily expect that `X(Of Integer).F(7)` would return `7L`.</span></span> <span data-ttu-id="00c91-430">Dies würde jedoch nicht der Fall sein, da numerische Konvertierungen nur berücksichtigt werden, wenn die Typen bekanntermaßen zur Kompilierzeit numerisch sind.</span><span class="sxs-lookup"><span data-stu-id="00c91-430">However, it would not, because numeric conversions are only considered when the types are known to be numeric at compile time.</span></span> <span data-ttu-id="00c91-431">Damit die Semantik eindeutig ist, muss das obige Beispiel stattdessen geschrieben werden:</span><span class="sxs-lookup"><span data-stu-id="00c91-431">In order to make the semantics clear, the above example must instead be written:</span></span>

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long
        Return CLng(CObj(t))    ' OK, conversions permitted
    End Function
End Class
```

## <a name="user-defined-conversions"></a><span data-ttu-id="00c91-432">Benutzerdefinierte Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="00c91-432">User-Defined Conversions</span></span>

<span data-ttu-id="00c91-433">Systeminterne *Konvertierungen* sind Konvertierungen, die von der Sprache definiert werden (d. h. in dieser Spezifikation aufgelistet), während *benutzerdefinierte Konvertierungen* durch Überladen des `CType` Operators definiert werden.</span><span class="sxs-lookup"><span data-stu-id="00c91-433">*Intrinsic conversions* are conversions defined by the language (i.e. listed in this specification), while *user-defined conversions* are defined by overloading the `CType` operator.</span></span> <span data-ttu-id="00c91-434">Wenn bei der Konvertierung zwischen Typen keine systeminternen Konvertierungen anwendbar sind, werden benutzerdefinierte Konvertierungen berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="00c91-434">When converting between types, if no intrinsic conversions are applicable then user-defined conversions will be considered.</span></span> <span data-ttu-id="00c91-435">Wenn eine benutzerdefinierte Konvertierung für die Quell-und Zieltypen *am spezifischsten* ist, wird die benutzerdefinierte Konvertierung verwendet.</span><span class="sxs-lookup"><span data-stu-id="00c91-435">If there is a user-defined conversion that is *most specific* for the source and target types, then the user-defined conversion will be used.</span></span> <span data-ttu-id="00c91-436">Andernfalls wird ein Fehler bei der Kompilierzeit ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="00c91-436">Otherwise, a compile-time error results.</span></span> <span data-ttu-id="00c91-437">Die spezifischere Konvertierung ist die, deren Operand dem Quelltyp am nächsten ist, und dessen Ergebnistyp dem Zieltyp am nächsten ist.</span><span class="sxs-lookup"><span data-stu-id="00c91-437">The most specific conversion is the one whose operand is "closest" to the source type and whose result type is "closest" to the target type.</span></span> <span data-ttu-id="00c91-438">Wenn Sie bestimmen, welche benutzerdefinierte Konvertierung verwendet werden soll, wird die spezifischere erweiternde Konvertierung verwendet. Wenn keine erweiternde Konvertierung am spezifischsten ist, wird die spezifischere einschränkende Konvertierung verwendet.</span><span class="sxs-lookup"><span data-stu-id="00c91-438">When determining what user-defined conversion to use, the most specific widening conversion will be used; if no widening conversion is most specific, the most specific narrowing conversion will be used.</span></span> <span data-ttu-id="00c91-439">Wenn keine spezifische einschränkende Konvertierung vorliegt, ist die Konvertierung nicht definiert, und es tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="00c91-439">If there is no most specific narrowing conversion, then the conversion is undefined and a compile-time error occurs.</span></span>

<span data-ttu-id="00c91-440">In den folgenden Abschnitten wird erläutert, wie die spezifischsten Konvertierungen bestimmt werden.</span><span class="sxs-lookup"><span data-stu-id="00c91-440">The following sections cover how the most specific conversions are determined.</span></span> <span data-ttu-id="00c91-441">Dabei werden die folgenden Begriffe verwendet:</span><span class="sxs-lookup"><span data-stu-id="00c91-441">They use the following terms:</span></span>

<span data-ttu-id="00c91-442">Wenn eine systeminterne erweiternde Konvertierung von einem Typ `A` in einen Typ `B`vorhanden ist, und wenn weder `A` noch `B` Schnittstellen sind, wird `A` durch `B`*eingeschlossen* , und `B` *umfasst* `A`.</span><span class="sxs-lookup"><span data-stu-id="00c91-442">If an intrinsic widening conversion exists from a type `A` to a type `B`, and if neither `A` nor `B` are interfaces, then `A` is *encompassed* by `B`, and `B` *encompasses* `A`.</span></span>

<span data-ttu-id="00c91-443">Der *umfassendste* Typ in einer Reihe von Typen ist der einzige Typ, der alle anderen Typen im Satz umfasst.</span><span class="sxs-lookup"><span data-stu-id="00c91-443">The *most encompassing* type in a set of types is the one type that encompasses all other types in the set.</span></span> <span data-ttu-id="00c91-444">Wenn kein einzelner Typ alle anderen Typen umfasst, hat der Satz keinen ganz umfassenden Typ.</span><span class="sxs-lookup"><span data-stu-id="00c91-444">If no single type encompasses all other types, then the set has no most encompassing type.</span></span> <span data-ttu-id="00c91-445">Der umfassendste Typ ist in intuitiver Hinsicht der "größte" Typ im Satz. der einzige Typ, auf den jeder der anderen Typen durch eine erweiternde Konvertierung konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="00c91-445">In intuitive terms, the most encompassing type is the "largest" type in the set -- the one type to which each of the other types can be converted through a widening conversion.</span></span>

<span data-ttu-id="00c91-446">Der *am häufigsten* in einem Satz von Typen eingeschlossenen Typ ist ein Typ, der von allen anderen Typen im Satz eingeschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="00c91-446">The *most encompassed* type in a set of types is the one type that is encompassed by all other types in the set.</span></span> <span data-ttu-id="00c91-447">Wenn kein einzelner Typ von allen anderen Typen eingeschlossen wird, hat der Satz nicht den meisten Typ.</span><span class="sxs-lookup"><span data-stu-id="00c91-447">If no single type is encompassed by all other types, then the set has no most encompassed type.</span></span> <span data-ttu-id="00c91-448">Der umfassendste Typ ist in intuitiver Hinsicht der "kleinste" Typ im Satz. der einzige Typ, der durch eine einschränkende Konvertierung in jeden der anderen Typen konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="00c91-448">In intuitive terms, the most encompassed type is the "smallest" type in the set -- the one type that can be converted to each of the other types through a narrowing conversion.</span></span>

<span data-ttu-id="00c91-449">Beim Erfassen der benutzerdefinierten benutzerdefinierten Konvertierungen für einen Typ `T?`werden stattdessen die von `T` definierten benutzerdefinierten Konvertierungs Operatoren verwendet.</span><span class="sxs-lookup"><span data-stu-id="00c91-449">When collecting the candidate user-defined conversions for a type `T?`, the user-defined conversion operators defined by `T` are used instead.</span></span> <span data-ttu-id="00c91-450">Wenn der Typ, in den konvertiert wird, auch ein Werttyp ist, der NULL-Werte zulässt, werden alle benutzerdefinierten Konvertierungs Operatoren von `T`, die nur nicht auf NULL festleg Bare Werttypen einschließen.</span><span class="sxs-lookup"><span data-stu-id="00c91-450">If the type being converted to is also a nullable value type, then any of `T`'s user-defined conversions operators that involve only non-nullable value types are lifted.</span></span> <span data-ttu-id="00c91-451">Ein Konvertierungs Operator von `T` in `S` wird als Konvertierung von `T?` in `S?` angehoben und ausgewertet, indem `T?` bei Bedarf in `T`konvertiert wird. Anschließend wird der benutzerdefinierte Konvertierungs Operator von `T` zu `S` ausgewertet und `S` bei Bedarf in `S?`konvertiert.</span><span class="sxs-lookup"><span data-stu-id="00c91-451">A conversion operator from `T` to `S` is lifted to be a conversion from `T?` to `S?` and is evaluated by converting `T?` to `T`, if necessary, then evaluating the user-defined conversion operator from `T` to `S` and then converting `S` to `S?`, if necessary.</span></span> <span data-ttu-id="00c91-452">Wenn der Wert, der konvertiert wird, `Nothing`ist, konvertiert ein Operator mit erhöhten Konvertierungen jedoch direkt in den Wert `Nothing`, der als `S?`typisiert ist.</span><span class="sxs-lookup"><span data-stu-id="00c91-452">If the value being converted is `Nothing`, however, a lifted conversion operator converts directly into a value of `Nothing` typed as `S?`.</span></span> <span data-ttu-id="00c91-453">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="00c91-453">For example:</span></span>

```vb
Structure S
    ...
End Structure

Structure T
    Public Shared Widening Operator CType(ByVal v As T) As S
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim x As T?
        Dim y As S?

        y = x                ' Legal: y is still null
        x = New T()
        y = x                ' Legal: Converts from T to S
    End Sub
End Module
```

<span data-ttu-id="00c91-454">Beim Auflösen von Konvertierungen werden benutzerdefinierte Konvertierungs Operatoren immer für gesteigerte Konvertierungs Operatoren bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="00c91-454">When resolving conversions, user-defined conversions operators are always preferred over lifted conversion operators.</span></span> <span data-ttu-id="00c91-455">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="00c91-455">For example:</span></span>

```vb
Structure S
    ...
End Structure

Structure T
    Public Shared Widening Operator CType(ByVal v As T) As S
        ...
    End Operator

    Public Shared Widening Operator CType(ByVal v As T?) As S?
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim x As T?
        Dim y As S?

        y = x                ' Calls user-defined conversion, not lifted conversion
    End Sub
End Module
```

<span data-ttu-id="00c91-456">Zur Laufzeit kann das Auswerten einer benutzerdefinierten Konvertierung bis zu drei Schritte umfassen:</span><span class="sxs-lookup"><span data-stu-id="00c91-456">At run-time, evaluating a user-defined conversion can involve up to three steps:</span></span>

1. <span data-ttu-id="00c91-457">Zuerst wird der Wert mithilfe einer systeminternen Konvertierung aus dem Quelltyp in den Operanden konvertiert.</span><span class="sxs-lookup"><span data-stu-id="00c91-457">First, the value is converted from the source type to the operand type using an intrinsic conversion, if necessary.</span></span>

2. <span data-ttu-id="00c91-458">Anschließend wird die benutzerdefinierte Konvertierung aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="00c91-458">Then, the user-defined conversion is invoked.</span></span>

3. <span data-ttu-id="00c91-459">Schließlich wird das Ergebnis der benutzerdefinierten Konvertierung mithilfe einer systeminternen Konvertierung in den Zieltyp konvertiert, falls erforderlich.</span><span class="sxs-lookup"><span data-stu-id="00c91-459">Finally, the result of the user-defined conversion is converted to the target type using an intrinsic conversion, if necessary.</span></span>

<span data-ttu-id="00c91-460">Beachten Sie unbedingt, dass die Auswertung einer benutzerdefinierten Konvertierung nie mehr als einen benutzerdefinierten Konvertierungs Operator einschließt.</span><span class="sxs-lookup"><span data-stu-id="00c91-460">It is important to note that evaluation of a user-defined conversion will never involve more than one user-defined conversion operator.</span></span>

### <a name="most-specific-widening-conversion"></a><span data-ttu-id="00c91-461">Spezifischere erweiternde Konvertierung</span><span class="sxs-lookup"><span data-stu-id="00c91-461">Most Specific Widening Conversion</span></span>

<span data-ttu-id="00c91-462">Die Ermittlung des spezifischsten benutzerdefinierten erweiterungskonvertierungsoperators zwischen zwei Typen erfolgt mithilfe der folgenden Schritte:</span><span class="sxs-lookup"><span data-stu-id="00c91-462">Determining the most specific user-defined widening conversion operator between two types is accomplished using the following steps:</span></span>

1. <span data-ttu-id="00c91-463">Zuerst werden alle Kandidaten Konvertierungs Operatoren gesammelt.</span><span class="sxs-lookup"><span data-stu-id="00c91-463">First, all of the candidate conversion operators are collected.</span></span> <span data-ttu-id="00c91-464">Die Operatoren für die Kandidaten Konvertierung sind alle benutzerdefinierten Erweiterungs Operatoren im Quelltyp und alle benutzerdefinierten Erweiterungs Operatoren im Zieltyp.</span><span class="sxs-lookup"><span data-stu-id="00c91-464">The candidate conversion operators are all of the user-defined widening conversion operators in the source type and all of the user-defined widening conversion operators in the target type.</span></span>

2. <span data-ttu-id="00c91-465">Anschließend werden alle nicht anwendbaren Konvertierungs Operatoren aus dem Satz entfernt.</span><span class="sxs-lookup"><span data-stu-id="00c91-465">Then, all non-applicable conversion operators are removed from the set.</span></span> <span data-ttu-id="00c91-466">Ein Konvertierungs Operator ist auf einen Quelltyp und einen Zieltyp anwendbar, wenn es einen intrinsischen erweiterbaren Konvertierungs Operator vom Quelltyp zum Operanden-Typ gibt und es einen intrinsischen erweiterbaren Konvertierungs Operator vom Ergebnis des Operators zum Zieltyp gibt.</span><span class="sxs-lookup"><span data-stu-id="00c91-466">A conversion operator is applicable to a source type and target type if there is an intrinsic widening conversion operator from the source type to the operand type and there is an intrinsic widening conversion operator from the result of the operator to the target type.</span></span> <span data-ttu-id="00c91-467">Wenn keine anwendbaren Konvertierungs Operatoren vorhanden sind, gibt es keine spezifischere erweiternde Konvertierung.</span><span class="sxs-lookup"><span data-stu-id="00c91-467">If there are no applicable conversion operators, then there is no most specific widening conversion.</span></span>

3. <span data-ttu-id="00c91-468">Anschließend wird der spezifischere Quelltyp der anwendbaren Konvertierungs Operatoren bestimmt:</span><span class="sxs-lookup"><span data-stu-id="00c91-468">Then, the most specific source type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="00c91-469">Wenn ein Konvertierungs Operator direkt aus dem Quelltyp konvertiert wird, ist der Quelltyp der spezifischere Quelltyp.</span><span class="sxs-lookup"><span data-stu-id="00c91-469">If any of the conversion operators convert directly from the source type, then the source type is the most specific source type.</span></span>

   * <span data-ttu-id="00c91-470">Andernfalls ist der spezifischere Quelltyp der am häufigsten in der kombinierten Gruppe von Quell Typen der Konvertierungs Operatoren eingeschlossenen Typ.</span><span class="sxs-lookup"><span data-stu-id="00c91-470">Otherwise, the most specific source type is the most encompassed type in the combined set of source types of the conversion operators.</span></span> <span data-ttu-id="00c91-471">Wenn kein Typ gefunden werden kann, der sich in der angegebenen Erweiterung befindet, gibt es keine spezifischere Erweiterungs Konvertierung.</span><span class="sxs-lookup"><span data-stu-id="00c91-471">If no most encompassed type can be found, then there is no most specific widening conversion.</span></span>

4. <span data-ttu-id="00c91-472">Anschließend wird der spezifischere Zieltyp der anwendbaren Konvertierungs Operatoren bestimmt:</span><span class="sxs-lookup"><span data-stu-id="00c91-472">Then, the most specific target type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="00c91-473">Wenn ein Konvertierungs Operator direkt in den Zieltyp konvertiert wird, ist der Zieltyp der spezifischere Zieltyp.</span><span class="sxs-lookup"><span data-stu-id="00c91-473">If any of the conversion operators convert directly to the target type, then the target type is the most specific target type.</span></span>

   * <span data-ttu-id="00c91-474">Andernfalls ist der spezifischere Zieltyp der umfassendste Typ in der kombinierten Gruppe von Zieltypen der Konvertierungs Operatoren.</span><span class="sxs-lookup"><span data-stu-id="00c91-474">Otherwise, the most specific target type is the most encompassing type in the combined set of target types of the conversion operators.</span></span> <span data-ttu-id="00c91-475">Wenn kein umfassender Typ gefunden werden kann, gibt es keine spezifischere erweiternde Konvertierung.</span><span class="sxs-lookup"><span data-stu-id="00c91-475">If no most encompassing type can be found, then there is no most specific widening conversion.</span></span>

5. <span data-ttu-id="00c91-476">Wenn dann genau ein Konvertierungs Operator vom spezifischsten Quelltyp in den spezifischsten Zieltyp konvertiert wird, ist dies der spezifischere Konvertierungs Operator.</span><span class="sxs-lookup"><span data-stu-id="00c91-476">Then, if exactly one conversion operator converts from the most specific source type to the most specific target type, then this is the most specific conversion operator.</span></span> <span data-ttu-id="00c91-477">Wenn mehr als ein solcher Operator vorhanden ist, gibt es keine spezifischere erweiternde Konvertierung.</span><span class="sxs-lookup"><span data-stu-id="00c91-477">If more than one such operator exists, then there is no most specific widening conversion.</span></span>

### <a name="most-specific-narrowing-conversion"></a><span data-ttu-id="00c91-478">Die spezifischere einschränkende Konvertierung</span><span class="sxs-lookup"><span data-stu-id="00c91-478">Most Specific Narrowing Conversion</span></span>

<span data-ttu-id="00c91-479">Die Ermittlung des spezifischsten benutzerdefinierten einschränkenden Konvertierungs Operators zwischen zwei Typen erfolgt mithilfe der folgenden Schritte:</span><span class="sxs-lookup"><span data-stu-id="00c91-479">Determining the most specific user-defined narrowing conversion operator between two types is accomplished using the following steps:</span></span>

1. <span data-ttu-id="00c91-480">Zuerst werden alle Kandidaten Konvertierungs Operatoren gesammelt.</span><span class="sxs-lookup"><span data-stu-id="00c91-480">First, all of the candidate conversion operators are collected.</span></span> <span data-ttu-id="00c91-481">Die Operatoren für die Kandidaten Konvertierung sind alle benutzerdefinierten Konvertierungs Operatoren im Quelltyp und alle benutzerdefinierten Konvertierungs Operatoren im Zieltyp.</span><span class="sxs-lookup"><span data-stu-id="00c91-481">The candidate conversion operators are all of the user-defined conversion operators in the source type and all of the user-defined conversion operators in the target type.</span></span>

2. <span data-ttu-id="00c91-482">Anschließend werden alle nicht anwendbaren Konvertierungs Operatoren aus dem Satz entfernt.</span><span class="sxs-lookup"><span data-stu-id="00c91-482">Then, all non-applicable conversion operators are removed from the set.</span></span> <span data-ttu-id="00c91-483">Ein Konvertierungs Operator ist auf einen Quelltyp und einen Zieltyp anwendbar, wenn ein System interner Konvertierungs Operator vom Quelltyp zum Operanden-Typ vorhanden ist und ein System interner Konvertierungs Operator vom Ergebnis des Operators zum Zieltyp vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="00c91-483">A conversion operator is applicable to a source type and target type if there is an intrinsic conversion operator from the source type to the operand type and there is an intrinsic conversion operator from the result of the operator to the target type.</span></span> <span data-ttu-id="00c91-484">Wenn keine anwendbaren Konvertierungs Operatoren vorhanden sind, gibt es keine spezifischere einschränkende Konvertierung.</span><span class="sxs-lookup"><span data-stu-id="00c91-484">If there are no applicable conversion operators, then there is no most specific narrowing conversion.</span></span>

3. <span data-ttu-id="00c91-485">Anschließend wird der spezifischere Quelltyp der anwendbaren Konvertierungs Operatoren bestimmt:</span><span class="sxs-lookup"><span data-stu-id="00c91-485">Then, the most specific source type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="00c91-486">Wenn ein Konvertierungs Operator direkt aus dem Quelltyp konvertiert wird, ist der Quelltyp der spezifischere Quelltyp.</span><span class="sxs-lookup"><span data-stu-id="00c91-486">If any of the conversion operators convert directly from the source type, then the source type is the most specific source type.</span></span>

   * <span data-ttu-id="00c91-487">Andernfalls handelt es sich beim Konvertieren eines Konvertierungs Operators aus Typen, die den Quelltyp einschließen, um den am meisten spezifisierten Typ in der kombinierten Gruppe von Quell Typen dieser Konvertierungs Operatoren.</span><span class="sxs-lookup"><span data-stu-id="00c91-487">Otherwise, if any of the conversion operators convert from types that encompass the source type, then the most specific source type is the most encompassed type in the combined set of source types of those conversion operators.</span></span> <span data-ttu-id="00c91-488">Wenn kein Typ gefunden werden kann, der sich in der gleichen Form befindet, ist keine besonders einschränkende Konvertierung vorhanden.</span><span class="sxs-lookup"><span data-stu-id="00c91-488">If no most encompassed type can be found, then there is no most specific narrowing conversion.</span></span>

   * <span data-ttu-id="00c91-489">Andernfalls ist der spezifischere Quelltyp der umfassendste Typ in der kombinierten Gruppe von Quell Typen der Konvertierungs Operatoren.</span><span class="sxs-lookup"><span data-stu-id="00c91-489">Otherwise, the most specific source type is the most encompassing type in the combined set of source types of the conversion operators.</span></span> <span data-ttu-id="00c91-490">Wenn kein umfassender Typ gefunden werden kann, gibt es keine spezifischere einschränkende Konvertierung.</span><span class="sxs-lookup"><span data-stu-id="00c91-490">If no most encompassing type can be found, then there is no most specific narrowing conversion.</span></span>

4. <span data-ttu-id="00c91-491">Anschließend wird der spezifischere Zieltyp der anwendbaren Konvertierungs Operatoren bestimmt:</span><span class="sxs-lookup"><span data-stu-id="00c91-491">Then, the most specific target type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="00c91-492">Wenn ein Konvertierungs Operator direkt in den Zieltyp konvertiert wird, ist der Zieltyp der spezifischere Zieltyp.</span><span class="sxs-lookup"><span data-stu-id="00c91-492">If any of the conversion operators convert directly to the target type, then the target type is the most specific target type.</span></span>

   * <span data-ttu-id="00c91-493">Wenn ein Konvertierungs Operator in Typen konvertiert wird, die durch den Zieltyp eingeschlossen werden, dann ist der spezifischere Zieltyp der umfassendste Typ in der kombinierten Gruppe von Quell Typen dieser Konvertierungs Operatoren.</span><span class="sxs-lookup"><span data-stu-id="00c91-493">Otherwise, if any of the conversion operators convert to types that are encompassed by the target type, then the most specific target type is the most encompassing type in the combined set of source types of those conversion operators.</span></span> <span data-ttu-id="00c91-494">Wenn kein umfassender Typ gefunden werden kann, gibt es keine spezifischere einschränkende Konvertierung.</span><span class="sxs-lookup"><span data-stu-id="00c91-494">If no most encompassing type can be found, then there is no most specific narrowing conversion.</span></span>

   * <span data-ttu-id="00c91-495">Andernfalls ist der spezifischere Zieltyp der Typ, der in der kombinierten Gruppe von Zieltypen der Konvertierungs Operatoren am häufigsten enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="00c91-495">Otherwise, the most specific target type is the most encompassed type in the combined set of target types of the conversion operators.</span></span> <span data-ttu-id="00c91-496">Wenn kein Typ gefunden werden kann, der sich in der gleichen Form befindet, ist keine besonders einschränkende Konvertierung vorhanden.</span><span class="sxs-lookup"><span data-stu-id="00c91-496">If no most encompassed type can be found, then there is no most specific narrowing conversion.</span></span>

5. <span data-ttu-id="00c91-497">Wenn dann genau ein Konvertierungs Operator vom spezifischsten Quelltyp in den spezifischsten Zieltyp konvertiert wird, ist dies der spezifischere Konvertierungs Operator.</span><span class="sxs-lookup"><span data-stu-id="00c91-497">Then, if exactly one conversion operator converts from the most specific source type to the most specific target type, then this is the most specific conversion operator.</span></span> <span data-ttu-id="00c91-498">Wenn mehr als ein solcher Operator vorhanden ist, gibt es keine spezifischere einschränkende Konvertierung.</span><span class="sxs-lookup"><span data-stu-id="00c91-498">If more than one such operator exists, then there is no most specific narrowing conversion.</span></span>

## <a name="native-conversions"></a><span data-ttu-id="00c91-499">Native Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="00c91-499">Native Conversions</span></span>

<span data-ttu-id="00c91-500">Einige der Konvertierungen werden als systemeigene *Konvertierungen* klassifiziert, da Sie von der .NET Framework nativ unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="00c91-500">Several of the conversions are classified as *native conversions* because they are supported natively by the .NET Framework.</span></span> <span data-ttu-id="00c91-501">Diese Konvertierungen können mithilfe der Konvertierungs Operatoren `DirectCast` und `TryCast` sowie anderer spezieller Verhaltensweisen optimiert werden.</span><span class="sxs-lookup"><span data-stu-id="00c91-501">These conversions are ones that can be optimized through the use of the `DirectCast` and `TryCast` conversion operators, as well as other special behaviors.</span></span> <span data-ttu-id="00c91-502">Die als Native Konvertierungen klassifizierten Konvertierungen sind: Identitäts Konvertierungen, Standard Konvertierungen, Verweis Konvertierungen, Array Konvertierungen, Werttyp Konvertierungen und Typparameter Konvertierungen.</span><span class="sxs-lookup"><span data-stu-id="00c91-502">The conversions classified as native conversions are: identity conversions, default conversions, reference conversions, array conversions, value type conversions, and type parameter conversions.</span></span>

## <a name="dominant-type"></a><span data-ttu-id="00c91-503">Dominanter Typ</span><span class="sxs-lookup"><span data-stu-id="00c91-503">Dominant Type</span></span>

<span data-ttu-id="00c91-504">Wenn ein Satz von Typen vorliegt, ist es häufig erforderlich, dass in Situationen wie z. b. der Typrückschluss der bestimmende *Typ* der Menge bestimmt wird.</span><span class="sxs-lookup"><span data-stu-id="00c91-504">Given a set of types, it is often necessary in situations such as type inference to determine the *dominant type* of the set.</span></span> <span data-ttu-id="00c91-505">Der vorherrschende Typ eines Satzes von Typen wird bestimmt, indem zuerst alle Typen entfernt werden, für die ein oder mehrere andere Typen keine implizite Konvertierung in aufweisen.</span><span class="sxs-lookup"><span data-stu-id="00c91-505">The dominant type of a set of types is determined by first removing any types that one or more other types do not have an implicit conversion to.</span></span> <span data-ttu-id="00c91-506">Wenn an dieser Stelle keine Typen übrig sind, gibt es keinen vorherrschenden Typ.</span><span class="sxs-lookup"><span data-stu-id="00c91-506">If there are no types left at this point, there is no dominant type.</span></span> <span data-ttu-id="00c91-507">Der bestimmende Typ ist dann die meisten der verbleibenden Typen.</span><span class="sxs-lookup"><span data-stu-id="00c91-507">The dominant type is then the most encompassed of the remaining types.</span></span> <span data-ttu-id="00c91-508">Wenn mehr als ein Typ vorhanden ist, der am meisten eingeschlossen ist, gibt es keinen vorherrschenden Typ.</span><span class="sxs-lookup"><span data-stu-id="00c91-508">If there is more than one type that is most encompassed, then there is no dominant type.</span></span>
