# <a name="conversions"></a><span data-ttu-id="2bd27-101">Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="2bd27-101">Conversions</span></span>

<span data-ttu-id="2bd27-102">Konvertierung ist der Prozess der Ändern eines Werts von einem Typ in einen anderen.</span><span class="sxs-lookup"><span data-stu-id="2bd27-102">Conversion is the process of changing a value from one type to another.</span></span> <span data-ttu-id="2bd27-103">Z. B. ein Wert vom Typ `Integer` konvertiert werden kann, um einen Wert vom Typ `Double`, oder ein Wert vom Typ `Derived` konvertiert werden kann, um einen Wert vom Typ `Base`, vorausgesetzt, dass `Base` und `Derived` sind sowohl Klassen als auch `Derived`erbt `Base`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-103">For example, a value of type `Integer` can be converted to a value of type `Double`, or a value of type `Derived` can be converted to a value of type `Base`, assuming that `Base` and `Derived` are both classes and `Derived` inherits from `Base`.</span></span> <span data-ttu-id="2bd27-104">Konvertierungen erfordern möglicherweise nicht den Wert selbst ändern (wie im zweiten Beispiel) oder erfordern möglicherweise die wesentlichen Änderungen in die wertedarstellung (wie im vorherigen Beispiel).</span><span class="sxs-lookup"><span data-stu-id="2bd27-104">Conversions may not require the value itself to change (as in the latter example), or they may require significant changes in the value representation (as in the former example).</span></span>

<span data-ttu-id="2bd27-105">Konvertierungen können erweiternde oder einschränkende sein.</span><span class="sxs-lookup"><span data-stu-id="2bd27-105">Conversions may either be widening or narrowing.</span></span> <span data-ttu-id="2bd27-106">Ein *eine erweiternde Konvertierung* ist eine Konvertierung von einem Typ in einen anderen Typ, dessen Wertdomäne mindestens so groß, wenn nicht größer als der ursprünglichen Domäne des Typs Wert ist.</span><span class="sxs-lookup"><span data-stu-id="2bd27-106">A *widening conversion* is a conversion from a type to another type whose value domain is at least as big, if not bigger, than the original type's value domain.</span></span> <span data-ttu-id="2bd27-107">Erweiterungskonvertierungen sollte nie fehlschlagen.</span><span class="sxs-lookup"><span data-stu-id="2bd27-107">Widening conversions should never fail.</span></span> <span data-ttu-id="2bd27-108">Ein *einschränkende Konvertierung* ist eine Konvertierung von einem Typ in einen anderen Typ, dessen Wertdomäne ist, Wert kleiner als des ursprüngliche Typs Domäne oder ausreichend unabhängig vom stagingstatus dieser zusätzlichen Vorsicht beim, Ausführen der Konvertierung (für beispielsweise bei der Konvertierung von `Integer` zu `String`).</span><span class="sxs-lookup"><span data-stu-id="2bd27-108">A *narrowing conversion* is a conversion from a type to another type whose value domain is either smaller than the original type's value domain or sufficiently unrelated that extra care must be taken when doing the conversion (for example, when converting from `Integer` to `String`).</span></span> <span data-ttu-id="2bd27-109">Einschränkende Konvertierungen, die zu Datenverlust führen können, kann Fehler auftreten.</span><span class="sxs-lookup"><span data-stu-id="2bd27-109">Narrowing conversions, which may entail loss of information, can fail.</span></span>

<span data-ttu-id="2bd27-110">Die identitätskonvertierung (d. h. eine Konvertierung von einem Typ an sich selbst) und Standard-Wert-Konvertierung (d. h. eine Konvertierung von `Nothing`) für alle Typen definiert sind.</span><span class="sxs-lookup"><span data-stu-id="2bd27-110">The identity conversion (i.e. a conversion from a type to itself) and default value conversion (i.e. a conversion from `Nothing`) are defined for all types.</span></span>

## <a name="implicit-and-explicit-conversions"></a><span data-ttu-id="2bd27-111">Implizite und explizite Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="2bd27-111">Implicit and Explicit Conversions</span></span>

<span data-ttu-id="2bd27-112">Konvertierungen können es sich um *implizite* oder *explizite*.</span><span class="sxs-lookup"><span data-stu-id="2bd27-112">Conversions can be either *implicit* or *explicit*.</span></span> <span data-ttu-id="2bd27-113">Implizite Konvertierungen treten ohne eine besondere Syntax.</span><span class="sxs-lookup"><span data-stu-id="2bd27-113">Implicit conversions occur without any special syntax.</span></span> <span data-ttu-id="2bd27-114">Folgendes ist ein Beispiel für die implizite Konvertierung des ein `Integer` -Werts in einen `Long` Wert:</span><span class="sxs-lookup"><span data-stu-id="2bd27-114">The following is an example of implicit conversion of an `Integer` value to a `Long` value:</span></span>

```vb
Module Test
    Sub Main()
        Dim intValue As Integer = 123
        Dim longValue As Long = intValue

        Console.WriteLine(intValue & " = " & longValue)
    End Sub
End Module
```

Explizite Konvertierungen erfordern dagegen auf Umwandlungsoperatoren. Es wird versucht, führen Sie eine explizite Konvertierung in einen Wert ohne ein Cast-Operator bewirkt, dass einen Fehler während der Kompilierung. <span data-ttu-id="2bd27-117">Im folgenden Beispiel wird eine explizite Konvertierung konvertiert eine `Long` -Werts in einen `Integer` Wert.</span><span class="sxs-lookup"><span data-stu-id="2bd27-117">The following example uses an explicit conversion to convert a `Long` value to an `Integer` value.</span></span>

```vb
Module Test
    Sub Main()
        Dim longValue As Long = 134
        Dim intValue As Integer = CInt(longValue)

        Console.WriteLine(longValue & " = " & intValue)
    End Sub
End Module
```

Der Satz von impliziten Konvertierungen hängt von der kompilierungsumgebung und die `Option Strict` Anweisung. Wenn die strikte Semantik verwendet werden, kann die erweiternde Konvertierungen implizit auftreten. <span data-ttu-id="2bd27-120">Wenn die Semantik verwendet werden, können implizit alle erweiterungskonvertierungen und einschränkende Konvertierungen (das heißt, alle Konvertierungen) auftreten.</span><span class="sxs-lookup"><span data-stu-id="2bd27-120">If permissive semantics are being used, all widening and narrowing conversions (in other words, all conversions) may occur implicitly.</span></span>

## <a name="boolean-conversions"></a><span data-ttu-id="2bd27-121">Boolesche Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="2bd27-121">Boolean Conversions</span></span>

<span data-ttu-id="2bd27-122">Obwohl `Boolean` ist kein numerischer Typ, es verfügt über einschränkende Konvertierungen in und aus der numerischen Typen, als handele es sich um einen enumerierten Typ.</span><span class="sxs-lookup"><span data-stu-id="2bd27-122">Although `Boolean` is not a numeric type, it does have narrowing conversions to and from the numeric types as if it were an enumerated type.</span></span> <span data-ttu-id="2bd27-123">Das Literal `True` konvertiert, dem Literal `255` für `Byte`, `65535` für `UShort`, `4294967295` für `UInteger`, `18446744073709551615` für `ULong`, und klicken Sie auf den Ausdruck `-1` für `SByte`, `Short`, `Integer`, `Long`, `Decimal`, `Single`, und `Double`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-123">The literal `True` converts to the literal `255` for `Byte`, `65535` for `UShort`, `4294967295` for `UInteger`, `18446744073709551615` for `ULong`, and to the expression `-1` for `SByte`, `Short`, `Integer`, `Long`, `Decimal`, `Single`, and `Double`.</span></span> <span data-ttu-id="2bd27-124">Das Literal `False` konvertiert, dem Literal `0`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-124">The literal `False` converts to the literal `0`.</span></span> <span data-ttu-id="2bd27-125">Numerischer Wert 0 konvertiert, dem Literal `False`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-125">A zero numeric value converts to the literal `False`.</span></span> <span data-ttu-id="2bd27-126">Alle anderen numerischen Werte zu konvertieren, dem Literal `True`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-126">All other numeric values convert to the literal `True`.</span></span>

<span data-ttu-id="2bd27-127">Es ist eine einschränkende Konvertierung von booleschen Wert in eine Zeichenfolge, Konvertierung in `System.Boolean.TrueString` oder `System.Boolean.FalseString`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-127">There is a narrowing conversion from Boolean to String, converting to either `System.Boolean.TrueString` or `System.Boolean.FalseString`.</span></span> <span data-ttu-id="2bd27-128">Es gibt auch eine einschränkende Konvertierung von `String` zu `Boolean`: Wenn die Zeichenfolge gleich `TrueString` oder `FalseString` (in der aktuellen Kultur, Groß-und Kleinschreibung nicht) verwendet dann den entsprechenden Wert; andernfalls versucht wird, analysiert die Zeichenfolge als ein numerischen Typ (in Hexadezimal oder Oktal Wenn möglich ist, andernfalls als "float") und verwendet die oben genannten Regeln; Andernfalls löst `System.InvalidCastException`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-128">There is also a narrowing conversion from `String` to `Boolean`: if the string was equal to `TrueString` or `FalseString` (in the current culture, case-insensitively) then it uses the appropriate value; otherwise it attempts to parse the string as a numeric type (in hex or octal if possible, otherwise as a float) and uses the above rules; otherwise it throws `System.InvalidCastException`.</span></span>

## <a name="numeric-conversions"></a><span data-ttu-id="2bd27-129">Numerische Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="2bd27-129">Numeric Conversions</span></span>

<span data-ttu-id="2bd27-130">Numerische Konvertierungen zwischen den Typen vorhanden `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single` und `Double`, und alle Enumerationstypen.</span><span class="sxs-lookup"><span data-stu-id="2bd27-130">Numeric conversions exist between the types `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single` and `Double`, and all enumerated types.</span></span> <span data-ttu-id="2bd27-131">Wenn konvertiert wird, werden die aufgelistete Typen behandelt, als wären sie deren zugrunde liegenden Typen.</span><span class="sxs-lookup"><span data-stu-id="2bd27-131">When being converted, enumerated types are treated as if they were their underlying types.</span></span> <span data-ttu-id="2bd27-132">Bei der Konvertierung in einen enumerierten Typ ist der Quellwert nicht erforderlich, den Satz von Werten, die definiert, in dem enumerierten Typ entsprechen.</span><span class="sxs-lookup"><span data-stu-id="2bd27-132">When converting to an enumerated type, the source value is not required to conform to the set of values defined in the enumerated type.</span></span> <span data-ttu-id="2bd27-133">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2bd27-133">For example:</span></span>

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

<span data-ttu-id="2bd27-134">Numerische Konvertierungen werden zur Laufzeit wie folgt verarbeitet:</span><span class="sxs-lookup"><span data-stu-id="2bd27-134">Numeric conversions are processed at run-time as follows:</span></span>

* <span data-ttu-id="2bd27-135">Für eine Konvertierung aus einem numerischen Typ in einen größeren numerischen Typ ist der Wert einfach in größeren Typ konvertiert.</span><span class="sxs-lookup"><span data-stu-id="2bd27-135">For a conversion from a numeric type to a wider numeric type, the value is simply converted to the wider type.</span></span> <span data-ttu-id="2bd27-136">Konvertierungen von `UInteger`, `Integer`, `ULong`, `Long`, oder `Decimal` zu `Single` oder `Double` gerundet auf die nächste `Single` oder `Double` Wert.</span><span class="sxs-lookup"><span data-stu-id="2bd27-136">Conversions from `UInteger`, `Integer`, `ULong`, `Long`, or `Decimal` to `Single` or `Double` are rounded to the nearest `Single` or `Double` value.</span></span> <span data-ttu-id="2bd27-137">Während dieser Konvertierung einem Genauigkeitsverlust kommen kann, wird er nie eine Größenordnung verloren gehen.</span><span class="sxs-lookup"><span data-stu-id="2bd27-137">While this conversion may cause a loss of precision, it will never cause a loss of magnitude.</span></span>

* <span data-ttu-id="2bd27-138">Für eine Konvertierung in einen ganzzahligen Typ in einen anderen ganzzahligen Typ oder von `Single`, `Double`, oder `Decimal` in einen ganzzahligen Typ, das Ergebnis hängt davon ab, ob Überprüfungen auf Ganzzahlüberlauf auf ist:</span><span class="sxs-lookup"><span data-stu-id="2bd27-138">For a conversion from an integral type to another integral type, or from `Single`, `Double`, or `Decimal` to an integral type, the result depends on whether integer overflow checking is on:</span></span>

  <span data-ttu-id="2bd27-139">*Wenn Ganzzahlüberlauf überprüft wird:*</span><span class="sxs-lookup"><span data-stu-id="2bd27-139">*If integer overflow is being checked:*</span></span>

  * <span data-ttu-id="2bd27-140">Wenn die Quelle ein ganzzahliger Typ ist, ist die Konvertierung erfolgreich, wenn das Quellargument innerhalb des Bereichs des Zieltyps liegt.</span><span class="sxs-lookup"><span data-stu-id="2bd27-140">If the source is an integral type, the conversion succeeds if the source argument is within the range of the destination type.</span></span> <span data-ttu-id="2bd27-141">Die Konvertierung löst eine `System.OverflowException` -Ausnahme aus, wenn das Quellargument außerhalb des Bereichs des Zieltyps liegt.</span><span class="sxs-lookup"><span data-stu-id="2bd27-141">The conversion throws a `System.OverflowException` exception if the source argument is outside the range of the destination type.</span></span>

  * <span data-ttu-id="2bd27-142">Wenn die Quelle ist `Single`, `Double`, oder `Decimal`Quellwert wird gerundet, auf den nächsten ganzzahligen Wert auf- oder und dieser ganzzahlige Wert wird das Ergebnis der Konvertierung.</span><span class="sxs-lookup"><span data-stu-id="2bd27-142">If the source is `Single`, `Double`, or `Decimal`, the source value is rounded up or down to the nearest integral value, and this integral value becomes the result of the conversion.</span></span> <span data-ttu-id="2bd27-143">Wenn der Quellwert gleichermaßen nahe bei zwei ganzzahlige Werte ist, wird der Wert auf den Wert gerundet, die eine gerade Zahl am wenigsten signifikanten Ziffern aufweist.</span><span class="sxs-lookup"><span data-stu-id="2bd27-143">If the source value is equally close to two integral values, the value is rounded to the value that has an even number in the least significant digit position.</span></span> <span data-ttu-id="2bd27-144">Wenn der erzeugte Integralwert sich außerhalb des Bereichs des Zieltyps, ist eine `System.OverflowException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="2bd27-144">If the resulting integral value is outside the range of the destination type, a `System.OverflowException` exception is thrown.</span></span>

  <span data-ttu-id="2bd27-145">*Wenn Ganzzahlüberlauf nicht aktiviert ist:*</span><span class="sxs-lookup"><span data-stu-id="2bd27-145">*If integer overflow is not being checked:*</span></span>

  * <span data-ttu-id="2bd27-146">Wenn die Quelle ein ganzzahliger Typ ist, wird die Konvertierung immer erfolgreich und besteht nur aus einer verwirft die höchstwertigen Bits des Quellwerts.</span><span class="sxs-lookup"><span data-stu-id="2bd27-146">If the source is an integral type, the conversion always succeeds and simply consists of discarding the most significant bits of the source value.</span></span>

  * <span data-ttu-id="2bd27-147">Wenn die Quelle ist `Single`, `Double`, oder `Decimal`, die Konvertierung immer erfolgreich, und nur der Rundung des Quellwerts für den nächsten ganzzahligen Wert besteht.</span><span class="sxs-lookup"><span data-stu-id="2bd27-147">If the source is `Single`, `Double`, or `Decimal`, the conversion always succeeds and simply consists of rounding the source value towards the nearest integral value.</span></span> <span data-ttu-id="2bd27-148">Wenn der Quellwert gleichermaßen nahe bei zwei ganzzahlige Werte ist, wird der Wert immer auf den Wert gerundet, die eine gerade Zahl am wenigsten signifikanten Ziffern aufweist.</span><span class="sxs-lookup"><span data-stu-id="2bd27-148">If the source value is equally close to two integral values, the value is always rounded to the value that has an even number in the least significant digit position.</span></span>

* <span data-ttu-id="2bd27-149">Für eine Konvertierung von `Double` zu `Single`, `Double` Wert wird gerundet, um die nächste `Single` Wert.</span><span class="sxs-lookup"><span data-stu-id="2bd27-149">For a conversion from `Double` to `Single`, the `Double` value is rounded to the nearest `Single` value.</span></span> <span data-ttu-id="2bd27-150">Wenn die `Double` Wert ist zu klein, um die Darstellung als eine `Single`, das Ergebnis positiv oder negativ 0 (null).</span><span class="sxs-lookup"><span data-stu-id="2bd27-150">If the `Double` value is too small to represent as a `Single`, the result becomes positive zero or negative zero.</span></span> <span data-ttu-id="2bd27-151">Wenn die `Double` Wert ist zu groß, um die Darstellung als eine `Single`, wird das Ergebnis, positive oder negative Unendlichkeit.</span><span class="sxs-lookup"><span data-stu-id="2bd27-151">If the `Double` value is too large to represent as a `Single`, the result becomes positive infinity or negative infinity.</span></span> <span data-ttu-id="2bd27-152">Wenn die `Double` Wert `NaN`, das Ergebnis ist ebenfalls `NaN`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-152">If the `Double` value is `NaN`, the result is also `NaN`.</span></span>

* <span data-ttu-id="2bd27-153">Für eine Konvertierung von `Single` oder `Double` zu `Decimal`, wird der Quellwert in konvertiert `Decimal` Darstellung und bei Bedarf auf die nächste Zahl nach der 28. Dezimalstelle gerundet.</span><span class="sxs-lookup"><span data-stu-id="2bd27-153">For a conversion from `Single` or `Double` to `Decimal`, the source value is converted to `Decimal` representation and rounded to the nearest number after the 28th decimal place if required.</span></span> <span data-ttu-id="2bd27-154">Wenn der Quellwert zu klein, um die Darstellung als ist eine `Decimal`, wird das Ergebnis 0 (null).</span><span class="sxs-lookup"><span data-stu-id="2bd27-154">If the source value is too small to represent as a `Decimal`, the result becomes zero.</span></span> <span data-ttu-id="2bd27-155">Wenn der Quellwert `NaN`, unendlich oder zu groß, um die Darstellung als eine `Decimal`, `System.OverflowException` Ausnahme wird ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="2bd27-155">If the source value is `NaN`, infinity, or too large to represent as a `Decimal`, a `System.OverflowException` exception is thrown.</span></span>

* <span data-ttu-id="2bd27-156">Für eine Konvertierung von `Double` zu `Single`, `Double` Wert wird gerundet, um die nächste `Single` Wert.</span><span class="sxs-lookup"><span data-stu-id="2bd27-156">For a conversion from `Double` to `Single`, the `Double` value is rounded to the nearest `Single` value.</span></span> <span data-ttu-id="2bd27-157">Wenn die `Double` Wert ist zu klein, um die Darstellung als eine `Single`, das Ergebnis positiv oder negativ 0 (null).</span><span class="sxs-lookup"><span data-stu-id="2bd27-157">If the `Double` value is too small to represent as a `Single`, the result becomes positive zero or negative zero.</span></span> <span data-ttu-id="2bd27-158">Wenn die `Double` Wert ist zu groß, um die Darstellung als eine `Single`, wird das Ergebnis, positive oder negative Unendlichkeit.</span><span class="sxs-lookup"><span data-stu-id="2bd27-158">If the `Double` value is too large to represent as a `Single`, the result becomes positive infinity or negative infinity.</span></span> <span data-ttu-id="2bd27-159">Wenn die `Double` Wert `NaN`, das Ergebnis ist ebenfalls `NaN`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-159">If the `Double` value is `NaN`, the result is also `NaN`.</span></span>

## <a name="reference-conversions"></a><span data-ttu-id="2bd27-160">Verweiskonvertierungen</span><span class="sxs-lookup"><span data-stu-id="2bd27-160">Reference Conversions</span></span>

<span data-ttu-id="2bd27-161">Verweistypen können auf einen Basistyp und umgekehrt konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="2bd27-161">Reference types may be converted to a base type, and vice versa.</span></span> <span data-ttu-id="2bd27-162">Konvertierungen von einem Basistyp in einen stärker abgeleiteten Typ erfolgreich nur zur Laufzeit auf, wenn der konvertierte Wert ein null-Wert, der abgeleitete Typ selbst oder einen stärker abgeleiteten Typ ist.</span><span class="sxs-lookup"><span data-stu-id="2bd27-162">Conversions from a base type to a more derived type only succeed at run time if the value being converted is a null value, the derived type itself, or a more derived type.</span></span>

<span data-ttu-id="2bd27-163">Klassen- und Typen können in und aus einem Schnittstellentyp umgewandelt werden.</span><span class="sxs-lookup"><span data-stu-id="2bd27-163">Class and interface types can be cast to and from any interface type.</span></span> <span data-ttu-id="2bd27-164">Konvertierungen zwischen einem Typ und einem Schnittstellentyp erfolgreich nur zur Laufzeit angezeigt, wenn die tatsächlichen Typen, die Beteiligten eine vererbungs- oder implementierungsbeziehung-Beziehung aufweisen.</span><span class="sxs-lookup"><span data-stu-id="2bd27-164">Conversions between a type and an interface type only succeed at run time if the actual types involved have an inheritance or implementation relationship.</span></span> <span data-ttu-id="2bd27-165">Da ein Schnittstellentyp immer eine Instanz eines Typs enthalten, die von abgeleitet `Object`, ein Schnittstellentyps auch immer umgewandelt werden kann, in und aus `Object`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-165">Because an interface type will always contain an instance of a type that derives from `Object`, an interface type can also always be cast to and from `Object`.</span></span>

<span data-ttu-id="2bd27-166">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="2bd27-166">__Note.__</span></span> <span data-ttu-id="2bd27-167">Es ist kein Fehler konvertieren eine `NotInheritable` Klassen zu und von Schnittstellen, die ihn nicht implementiert ist, da die Klassen, die COM-Klassen darstellen schnittstellenimplementierungen, die nicht erst bekannt sind möglicherweise zur Laufzeit.</span><span class="sxs-lookup"><span data-stu-id="2bd27-167">It is not an error to convert a `NotInheritable` classes to and from interfaces that it does not implement because classes that represent COM classes may have interface implementations that are not known until run time.</span></span> 

<span data-ttu-id="2bd27-168">Fällt eine verweiskonvertierung zur Laufzeit eine `System.InvalidCastException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="2bd27-168">If a reference conversion fails at run time, a `System.InvalidCastException` exception is thrown.</span></span>

### <a name="reference-variance-conversions"></a><span data-ttu-id="2bd27-169">Varianz Verweiskonvertierungen</span><span class="sxs-lookup"><span data-stu-id="2bd27-169">Reference Variance Conversions</span></span>

<span data-ttu-id="2bd27-170">Generische Schnittstellen oder Delegaten können über Variante Typparameter verfügen, die Konvertierungen zwischen kompatiblen Varianten des Typs zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="2bd27-170">Generic interfaces or delegates may have variant type parameters that allow conversions between compatible variants of the type.</span></span> <span data-ttu-id="2bd27-171">Aus diesem Grund wird zur Laufzeit eine Konvertierung von einem Klassentyp oder einem Schnittstellentyp in einen Schnittstellentyp aufweisen, der Variante, die mit einem Schnittstellentyp kompatibel ist, es erbt oder diesen implementiert, erfolgreich ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="2bd27-171">Therefore, at runtime a conversion from a class type or an interface type to an interface type that is variant compatible with an interface type it inherits from or implements will succeed.</span></span> <span data-ttu-id="2bd27-172">Auf ähnliche Weise Delegattypen umgewandelt werden können und Delegattypen aus dem Variant kompatibel.</span><span class="sxs-lookup"><span data-stu-id="2bd27-172">Similarly, delegate types can be cast to and from variant compatible delegate types.</span></span> <span data-ttu-id="2bd27-173">Z. B. der Typ des Delegaten</span><span class="sxs-lookup"><span data-stu-id="2bd27-173">For example, the delegate type</span></span>

```vb
Delegate Function F(Of In A, Out R)(a As A) As R
```

<span data-ttu-id="2bd27-174">eine Konvertierung von können `F(Of Object, Integer)` zu `F(Of String, Integer)`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-174">would allow a conversion from `F(Of Object, Integer)` to `F(Of String, Integer)`.</span></span> <span data-ttu-id="2bd27-175">D. h. einen Delegaten `F` nimmt `Object` kann sicher verwendet werden, als Delegat `F` nimmt `String`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-175">That is, a delegate `F` which takes `Object` may be safely used as a delegate `F` which takes `String`.</span></span> <span data-ttu-id="2bd27-176">Wenn der Delegat aufgerufen wird, wird die Zielmethode wird erwartet, wenn Sie ein Objekt, und eine Zeichenfolge ist ein Objekt.</span><span class="sxs-lookup"><span data-stu-id="2bd27-176">When the delegate is invoked, the target method will be expecting an object, and a string is an object.</span></span>

<span data-ttu-id="2bd27-177">Ein generischer Typ von Delegaten oder dieser Schnittstelle `S(Of S1,...,Sn)` gilt als *Variante kompatibel* mit einem generischen Typ von Schnittstellen oder Delegate `T(Of T1,...,Tn)` wenn:</span><span class="sxs-lookup"><span data-stu-id="2bd27-177">A generic delegate or interface type `S(Of S1,...,Sn)` is said to be *variant compatible* with a generic interface or delegate type `T(Of T1,...,Tn)` if:</span></span>

* <span data-ttu-id="2bd27-178">`S` und `T` bestehen sowohl aus den gleichen generischen Typ `U(Of U1,...,Un)`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-178">`S` and `T` are both constructed from the same generic type `U(Of U1,...,Un)`.</span></span>

* <span data-ttu-id="2bd27-179">Für jeden Typparameter `Ux`:</span><span class="sxs-lookup"><span data-stu-id="2bd27-179">For each type parameter `Ux`:</span></span>

  * <span data-ttu-id="2bd27-180">Wenn der Typparameter, ohne Abweichung dann deklariert wurde `Sx` und `Tx` muss der gleiche Typ sein.</span><span class="sxs-lookup"><span data-stu-id="2bd27-180">If the type parameter was declared without variance then `Sx` and `Tx` must be the same type.</span></span>

  * <span data-ttu-id="2bd27-181">Wenn der Typparameter deklariert wurde `In` und es eine widening-Identität, Standard, Verweis, Array, oder Typ muss parameterkonvertierung aus `Sx` zu `Tx`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-181">If the type parameter was declared `In` then there must be a widening identity, default, reference, array, or type parameter conversion from `Sx` to `Tx`.</span></span>

  * <span data-ttu-id="2bd27-182">Wenn der Typparameter deklariert wurde `Out` und es eine widening-Identität, Standard, Verweis, Array, oder Typ muss parameterkonvertierung aus `Tx` zu `Sx`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-182">If the type parameter was declared `Out` then there must be a widening identity, default, reference, array, or type parameter conversion from `Tx` to `Sx`.</span></span>

<span data-ttu-id="2bd27-183">Wenn von einer Klasse die Daten in eine generische Schnittstelle mit Varianten Typparametern, konvertiert werden sollen, wenn die Klasse mehr als eine Variante Schnittstelle implementiert ist die Konvertierung mehrdeutig, wenn eine nicht Variant-Konvertierung nicht vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="2bd27-183">When converting from a class to a generic interface with variant type parameters, if the class implements more than one variant compatible interface the conversion is ambiguous if there is not a non-variant conversion.</span></span> <span data-ttu-id="2bd27-184">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2bd27-184">For example:</span></span>

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

### <a name="anonymous-delegate-conversions"></a><span data-ttu-id="2bd27-185">Konvertierungen von anonymen Delegaten</span><span class="sxs-lookup"><span data-stu-id="2bd27-185">Anonymous Delegate Conversions</span></span>

<span data-ttu-id="2bd27-186">Wenn ein Ausdruck, der als Lambda-Methode klassifiziert wird erneut klassifiziert als Wert in einem Kontext, wenn kein Zieltyp vorhanden ist (z. B. `Dim x = Function(a As Integer, b As Integer) a + b`), oder wenn der Zieltyp kein Delegattyp ist, ist der Typ des sich ergebenden Ausdrucks ein anonymer Delegat-Typ entspricht die Signatur der Lambda-Methode.</span><span class="sxs-lookup"><span data-stu-id="2bd27-186">When an expression classified as a lambda method is reclassified as a value in a context where there is no target type (for example, `Dim x = Function(a As Integer, b As Integer) a + b`), or where the target type is not a delegate type, the type of the resulting expression is an anonymous delegate type equivalent to the signature of the lambda method.</span></span> <span data-ttu-id="2bd27-187">Dieser anonymen Delegaten eine Konvertierung in einen beliebigen Typ kompatiblen Delegaten enthält: ein kompatiblen Delegattyp ist, jeden Delegattyp, der über einen Delegaterstellungsausdruck mit anonymen Delegaten des Typs erstellt werden kann `Invoke` Methode als Parameter.</span><span class="sxs-lookup"><span data-stu-id="2bd27-187">This anonymous delegate type has a conversion to any compatible delegate type: a compatible delegate type is any delegate type that can be created using a delegate creation expression with the anonymous delegate type's `Invoke` method as a parameter.</span></span> <span data-ttu-id="2bd27-188">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2bd27-188">For example:</span></span>

```vb
' Anonymous delegate type similar to Func(Of Object, Object, Object)
Dim x = Function(x, y) x + y

' OK because delegate type is compatible
Dim y As Func(Of Integer, Integer, Integer) = x
```

<span data-ttu-id="2bd27-189">Beachten Sie, dass die Typen `System.Delegate` und `System.MulticastDelegate` selbst keine gelten Delegattypen (obwohl alle Delegattypen aus ihnen erben).</span><span class="sxs-lookup"><span data-stu-id="2bd27-189">Note that the types `System.Delegate` and `System.MulticastDelegate` are not themselves considered delegate types (even though all delegate types inherit from them).</span></span> <span data-ttu-id="2bd27-190">Beachten Sie, dass die Konvertierung von anonymen Delegaten-Typ in einen kompatiblen Delegattyp nicht mit einer verweiskonvertierung ist.</span><span class="sxs-lookup"><span data-stu-id="2bd27-190">Also, note that the conversion from anonymous delegate type to a compatible delegate type is not a reference conversion.</span></span>

## <a name="array-conversions"></a><span data-ttu-id="2bd27-191">Arraykonvertierungen</span><span class="sxs-lookup"><span data-stu-id="2bd27-191">Array Conversions</span></span>

<span data-ttu-id="2bd27-192">Neben die Konvertierungen, die auf Arrays aufgrund der Tatsache definiert sind, dass sie Verweistypen sind, werden einige spezielle Konvertierungen für Arrays vorhanden.</span><span class="sxs-lookup"><span data-stu-id="2bd27-192">Besides the conversions that are defined on arrays by virtue of the fact that they are reference types, several special conversions exist for arrays.</span></span>

<span data-ttu-id="2bd27-193">Für die einzelnen Typen `A` und `B`, wenn sie beide Verweistypen oder Parameter vom Typ Werttypen nicht bekannt sind, und wenn `A` verfügt über einen Verweis, array oder Parameter-typkonvertierung in `B`, eine Konvertierung aus einem Array von vorhanden ist Typ `A` in ein Array vom Typ `B` mit demselben Rang.</span><span class="sxs-lookup"><span data-stu-id="2bd27-193">For any two types `A` and `B`, if they are both reference types or type parameters not known to be value types, and if `A` has a reference, array, or type parameter conversion to `B`, a conversion exists from an array of type `A` to an array of type `B` with the same rank.</span></span> <span data-ttu-id="2bd27-194">Diese Beziehung wird als bezeichnet *Array-Kovarianz*.</span><span class="sxs-lookup"><span data-stu-id="2bd27-194">This relationship is known as *array covariance*.</span></span> <span data-ttu-id="2bd27-195">Array-Kovarianz vor allem bedeutet, dass ein Element eines Arrays, dessen Elementtyp `B` möglicherweise um ein Element eines Arrays, dessen Elementtyp `A`, vorausgesetzt, dass beide `A` und `B` sind Verweistypen und diese `B` verfügt über eine verweiskonvertierung oder Array-Konvertierung in `A`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-195">Array covariance in particular means that an element of an array whose element type is `B` may actually be an element of an array whose element type is `A`, provided that both `A` and `B` are reference types and that `B` has a reference conversion or array conversion to `A`.</span></span> <span data-ttu-id="2bd27-196">Im folgenden Beispiel ist der zweite Aufruf von `F` bewirkt, dass eine `System.ArrayTypeMismatchException` Ausnahme ausgelöst wird, da der tatsächliche Elementtyp des `b` ist `String`, nicht `Object`:</span><span class="sxs-lookup"><span data-stu-id="2bd27-196">In the following example, the second invocation of `F` causes a `System.ArrayTypeMismatchException` exception to be thrown because the actual element type of `b` is `String`, not `Object`:</span></span>

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

<span data-ttu-id="2bd27-197">Aufgrund der Array-Kovarianz enthalten Zuweisungen auf Elemente des Verweistyparrays eine laufzeitüberprüfung, die sicherstellt, dass der Wert zugewiesen wird, auf das Arrayelement tatsächlich einen zulässigen Typ ist.</span><span class="sxs-lookup"><span data-stu-id="2bd27-197">Because of array covariance, assignments to elements of reference type arrays include a run-time check that ensures that the value being assigned to the array element is actually of a permitted type.</span></span>

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

<span data-ttu-id="2bd27-198">In diesem Beispiel ist die Zuweisung zu `array(i)` in Methode `Fill` implizit eine laufzeitüberprüfung wird, dass das Objekt sichergestellt, das die Variable verweist, die auch `value` ist entweder `Nothing` oder eine Instanz eines Typs, die kompatibel mit der tatsächliche Elementtyp des Arrays `array`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-198">In this example, the assignment to `array(i)` in method `Fill` implicitly includes a run-time check that ensures that the object referenced by the variable `value` is either `Nothing` or an instance of a type that is compatible with the actual element type of array `array`.</span></span> <span data-ttu-id="2bd27-199">In der Methode `Main`, die ersten beiden Aufrufe der Methode `Fill` erfolgreich ausgeführt werden, aber der dritte Aufruf bewirkt, dass eine `System.ArrayTypeMismatchException` Ausnahme ausgelöst wird, bei der Ausführung der ersten Zuweisung zu `array(i)`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-199">In method `Main`, the first two invocations of method `Fill` succeed, but the third invocation causes a `System.ArrayTypeMismatchException` exception to be thrown upon executing the first assignment to `array(i)`.</span></span> <span data-ttu-id="2bd27-200">Die Ausnahme tritt auf, weil ein `Integer` kann nicht gespeichert werden, einem `String` Array.</span><span class="sxs-lookup"><span data-stu-id="2bd27-200">The exception occurs because an `Integer` cannot be stored in a `String` array.</span></span>

<span data-ttu-id="2bd27-201">Wenn einer der die Arrayelementtypen ein Typparameter ist, dessen Typ erweist sich ein Werttyp zur Laufzeit, eine `System.InvalidCastException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="2bd27-201">If one of the array element types is a type parameter whose type turns out to be a value type at runtime, a `System.InvalidCastException` exception will be thrown.</span></span> <span data-ttu-id="2bd27-202">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2bd27-202">For example:</span></span>

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

<span data-ttu-id="2bd27-203">Konvertierungen zwischen ein Array von ein enumerierter Typ auch vorhanden sein, und ein Array von den enumerierten Typ zugrunde liegenden Typ oder ein Array von einem anderen enumerierten Typ mit dem gleichen zugrunde liegenden Typ, vorausgesetzt die Arrays den gleichen Rang aufweisen.</span><span class="sxs-lookup"><span data-stu-id="2bd27-203">Conversions also exist between an array of an enumerated type and an array of the enumerated type's underlying type or an array of another enumerated type with the same underlying type, provided the arrays have the same rank.</span></span>

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

<span data-ttu-id="2bd27-204">In diesem Beispiel ein Array von `Color` ist in und aus konvertiert ein Array von `Byte`, `Color`zugrunde liegenden Typ.</span><span class="sxs-lookup"><span data-stu-id="2bd27-204">In this example, an array of `Color` is converted to and from an array of `Byte`, `Color`'s underlying type.</span></span> <span data-ttu-id="2bd27-205">Die Konvertierung in ein Array von `Integer`, allerdings wird ein Fehler vorhanden, da `Integer` ist nicht der zugrunde liegende Typ `Color`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-205">The conversion to an array of `Integer`, however, will be an error because `Integer` is not the underlying type of `Color`.</span></span>

<span data-ttu-id="2bd27-206">Ein Rang-1-Array des Typs `A()` verfügt auch über eine Array-Konvertierung in den sammlungsschnittstellentypen `IList(Of B)`, `IReadOnlyList(Of B)`, `ICollection(Of B)`, `IReadOnlyCollection(Of B)` und `IEnumerable(Of B)` finden Sie im `System.Collections.Generic`, solange eine der folgenden Aussagen zutrifft:</span><span class="sxs-lookup"><span data-stu-id="2bd27-206">A rank-1 array of type `A()` also has an array conversion to the collection interface types `IList(Of B)`, `IReadOnlyList(Of B)`, `ICollection(Of B)`, `IReadOnlyCollection(Of B)` and `IEnumerable(Of B)` found in `System.Collections.Generic`, so long as one of the following is true:</span></span>

- <span data-ttu-id="2bd27-207">`A` und `B` sind beide verweisen auf Typen oder Parameter vom Typ nicht bekannt sein Werttypen und `A` verfügt über eine erweiternde Konvertierung eines Verweis "," Array "oder" Typ-Parameter in `B`; oder</span><span class="sxs-lookup"><span data-stu-id="2bd27-207">`A` and `B` are both reference types or type parameters not known to be value types; and `A` has a widening reference, array or type parameter conversion to `B`; or</span></span>
- <span data-ttu-id="2bd27-208">`A` und `B` sind beide Enumerationstypen mit dem gleichen zugrunde liegenden Typ oder</span><span class="sxs-lookup"><span data-stu-id="2bd27-208">`A` and `B` are both enumerated types of the same underlying type; or</span></span>
- <span data-ttu-id="2bd27-209">einer der `A` und `B` ist ein enumerierter Typ, und die andere ist deren zugrunde liegender Typ.</span><span class="sxs-lookup"><span data-stu-id="2bd27-209">one of `A` and `B` is an enumerated type, and the other is its underlying type.</span></span>

<span data-ttu-id="2bd27-210">Ein Array vom Typ A mit jeder Rang verfügt auch über eine Array-Konvertierung in die Schnittstelle nicht generische Auflistungstypen `IList`, `ICollection` und `IEnumerable` finden Sie im `System.Collections`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-210">Any array of type A with any rank also has an array conversion to the non-generic collection interface types `IList`, `ICollection` and `IEnumerable` found in `System.Collections`.</span></span>

<span data-ttu-id="2bd27-211">Es ist möglich, durchlaufen, die sich ergebenden Benutzeroberflächen mit `For Each`, oder durch Aufrufen der `GetEnumerator` Methoden direkt.</span><span class="sxs-lookup"><span data-stu-id="2bd27-211">It is possible to iterate over the resulting interfaces using `For Each`, or through invoking the `GetEnumerator` methods directly.</span></span> <span data-ttu-id="2bd27-212">Im Fall von Rang 1 Arrays konvertiert generische oder nicht generische Formen der `IList` oder `ICollection`, es ist auch möglich, die Elemente nach Index abrufen.</span><span class="sxs-lookup"><span data-stu-id="2bd27-212">In the case of rank-1 arrays converted generic or non-generic forms of `IList` or `ICollection`, it is also possible to get elements by index.</span></span> <span data-ttu-id="2bd27-213">Im Fall von Rang 1-Arrays, die generisch oder nicht generische Formen von konvertiert `IList`, es ist auch möglich, Elemente nach Index festlegen, gelten die gleiche Laufzeit Array-Kovarianz überprüft wird, wie oben beschrieben.</span><span class="sxs-lookup"><span data-stu-id="2bd27-213">In the case of rank-1 arrays converted to generic or non-generic forms of `IList`, it is also possible to set elements by index, subject to the same runtime array covariance checks as described above.</span></span> <span data-ttu-id="2bd27-214">Das Verhalten der anderen Schnittstellenmethoden ist nicht durch die VB-Language-Spezifikation definiert werden; Es ist Aufgabe der zugrunde liegenden Laufzeitumgebung.</span><span class="sxs-lookup"><span data-stu-id="2bd27-214">The behavior of all other interface methods is undefined by the VB language specification; it is up to the underlying runtime.</span></span>

## <a name="value-type-conversions"></a><span data-ttu-id="2bd27-215">Wert Typkonvertierungen</span><span class="sxs-lookup"><span data-stu-id="2bd27-215">Value Type Conversions</span></span>

<span data-ttu-id="2bd27-216">Ein Wert für den Typ konvertiert werden kann, auf dessen Basis Verweistypen oder einen Schnittstellentyp aufweisen, die sie mithilfe des so genannten implementiert *Boxing*.</span><span class="sxs-lookup"><span data-stu-id="2bd27-216">A value type value can be converted to one of its base reference types or an interface type that it implements through a process called *boxing*.</span></span> <span data-ttu-id="2bd27-217">Wenn Sie ein Wert für den Typ mittels Boxing konvertiert wird, wird der Wert aus dem Speicherort kopiert, wo sie auf den .NET Framework-Heap gespeichert.</span><span class="sxs-lookup"><span data-stu-id="2bd27-217">When a value type value is boxed, the value is copied from the location where it lives onto the .NET Framework heap.</span></span> <span data-ttu-id="2bd27-218">Ein Verweis auf diesen Speicherort auf dem Heap wird dann zurückgegeben und kann in einen Verweistyp-Variable gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="2bd27-218">A reference to this location on the heap is then returned and can be stored in a reference type variable.</span></span> <span data-ttu-id="2bd27-219">Dieser Verweis wird auch als bezeichnet ein *geschachtelt* Instanz des Werttyps.</span><span class="sxs-lookup"><span data-stu-id="2bd27-219">This reference is also referred to as a *boxed* instance of the value type.</span></span> <span data-ttu-id="2bd27-220">Die geschachtelte Instanz weist dieselbe Semantik wie ein Verweistyp anstelle eines Werttyps.</span><span class="sxs-lookup"><span data-stu-id="2bd27-220">The boxed instance has the same semantics as a reference type instead of a value type.</span></span>

<span data-ttu-id="2bd27-221">Geschachtelte Werttypen konvertiert werden können, an ihre ursprüngliche Werttyp mithilfe des so genannten *unboxing*.</span><span class="sxs-lookup"><span data-stu-id="2bd27-221">Boxed value types can be converted back to their original value type through a process called *unboxing*.</span></span> <span data-ttu-id="2bd27-222">Wenn ein geschachtelter Werttyp mittels Unboxing konvertiert wird, wird der Wert aus dem Heap in einer Variable Speicherort kopiert.</span><span class="sxs-lookup"><span data-stu-id="2bd27-222">When a boxed value type is unboxed, the value is copied from the heap into a variable location.</span></span> <span data-ttu-id="2bd27-223">Ab diesem Punkt verhält er sich als wäre es ein Werttyp.</span><span class="sxs-lookup"><span data-stu-id="2bd27-223">From that point on, it behaves as if it was a value type.</span></span> <span data-ttu-id="2bd27-224">Unboxing ein Werttyps muss der Wert ein null-Wert oder eine Instanz des Werttyps sein.</span><span class="sxs-lookup"><span data-stu-id="2bd27-224">When unboxing a value type, the value must be a null value or an instance of the value type.</span></span> <span data-ttu-id="2bd27-225">Andernfalls ein `System.InvalidCastException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="2bd27-225">Otherwise a `System.InvalidCastException` exception is thrown.</span></span> <span data-ttu-id="2bd27-226">Wenn der Wert einer Instanz eines enumerierten Typs ist, diesen Wert kann auch mittels Unboxing konvertiert werden, den enumerierten Typ zugrunde liegenden Typs oder einer anderen enumerierten Typ mit dem gleichen zugrunde liegenden Typ.</span><span class="sxs-lookup"><span data-stu-id="2bd27-226">If the value is an instance of an enumerated type, that value can also be unboxed to the enumerated type's underlying type or another enumerated type that has the same underlying type.</span></span> <span data-ttu-id="2bd27-227">Ein null-Wert wird behandelt, als handele es sich um das Literal `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-227">A null value is treated as if it were the literal `Nothing`.</span></span>

<span data-ttu-id="2bd27-228">Zur Unterstützung von auf NULL festlegbare Werttypen auch den Werttyp `System.Nullable(Of T)` speziell bei Aktionen, boxing und unboxing behandelt wird.</span><span class="sxs-lookup"><span data-stu-id="2bd27-228">To support nullable value types well, the value type `System.Nullable(Of T)` is treated specially when doing boxing and unboxing.</span></span> <span data-ttu-id="2bd27-229">Boxing einen Wert vom Typ `Nullable(Of T)` führt einen geschachtelten Wert vom Typ `T` Wenn des Zeitwerts `HasValue` -Eigenschaft ist `True` oder den Wert `Nothing` Wenn des Werts des `HasValue` -Eigenschaft ist `False`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-229">Boxing a value of type `Nullable(Of T)` results in a boxed value of type `T` if the value's `HasValue` property is `True` or a value of `Nothing` if the value's `HasValue` property is `False`.</span></span> <span data-ttu-id="2bd27-230">Unboxing einen Wert vom Typ `T` zu `Nullable(Of T)` führt zu einer Instanz von `Nullable(Of T)` , deren `Value` -Eigenschaft ist der geschachtelte Wert und deren `HasValue` Eigenschaft `True`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-230">Unboxing a value of type `T` to `Nullable(Of T)` results in an instance of `Nullable(Of T)` whose `Value` property is the boxed value and whose `HasValue` property is `True`.</span></span> <span data-ttu-id="2bd27-231">Der Wert `Nothing` können nicht geschachtelt werden, um `Nullable(Of T)` für alle `T` und einen Wert ergibt, deren `HasValue` Eigenschaft `False`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-231">The value `Nothing` can be unboxed to `Nullable(Of T)` for any `T` and results in a value whose `HasValue` property is `False`.</span></span> <span data-ttu-id="2bd27-232">Da geschachtelte Werttypen wie Verweis Verhalten Typen, es ist möglich, mehrere Verweise auf den gleichen Wert zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2bd27-232">Because boxed value types behave like reference types, it is possible to create multiple references to the same value.</span></span> <span data-ttu-id="2bd27-233">Für die primitiven Typen und Enumerationstypen, ist dies nicht relevant, da Instanzen dieser Typen sind *unveränderliche*.</span><span class="sxs-lookup"><span data-stu-id="2bd27-233">For the primitive types and enumerated types, this is irrelevant because instances of those types are *immutable*.</span></span> <span data-ttu-id="2bd27-234">Das heißt, ist es nicht möglich, eine geschachtelte Instanz dieser Typen, zu ändern, sodass es nicht möglich, beobachten Sie die Tatsache, dass es mehrere Verweise auf den gleichen Wert.</span><span class="sxs-lookup"><span data-stu-id="2bd27-234">That is, it is not possible to modify a boxed instance of those types, so it is not possible to observe the fact that there are multiple references to the same value.</span></span>

<span data-ttu-id="2bd27-235">Strukturen, die möglicherweise auf der anderen Seite änderbare sein, wenn die zugehörigen Instanzvariablen zugegriffen werden kann oder seine Methoden und Eigenschaften der Instanzvariablen zu ändern.</span><span class="sxs-lookup"><span data-stu-id="2bd27-235">Structures, on the other hand, may be mutable if its instance variables are accessible or if its methods or properties modify its instance variables.</span></span> <span data-ttu-id="2bd27-236">Wenn ein Verweis auf eine geschachtelte Struktur verwendet wird, um die Struktur zu ändern, klicken Sie dann sehen alle Verweise auf die geschachtelte Struktur die Änderung.</span><span class="sxs-lookup"><span data-stu-id="2bd27-236">If one reference to a boxed structure is used to modify the structure, then all references to the boxed structure will see the change.</span></span> <span data-ttu-id="2bd27-237">Da dieses Ergebnis möglicherweise nicht erwartet werden, wenn ein Wert eingegeben als `Object` wird von einem Speicherort kopiert, auf einer anderen geschachtelten Wert Typen werden automatisch auf dem Heap, anstatt Sie lediglich ihre kopiert Verweise geklont werden.</span><span class="sxs-lookup"><span data-stu-id="2bd27-237">Because this result may be unexpected, when a value typed as `Object` is copied from one location to another boxed value types will automatically be cloned on the heap instead of merely having their references copied.</span></span> <span data-ttu-id="2bd27-238">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2bd27-238">For example:</span></span>

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

<span data-ttu-id="2bd27-239">Die Ausgabe des Programms lautet:</span><span class="sxs-lookup"><span data-stu-id="2bd27-239">The output of the program is:</span></span>

```
Values: 0, 123
Refs: 123, 123
```

<span data-ttu-id="2bd27-240">Die Zuweisung auf das Feld der lokalen Variablen `val2` wirkt sich nicht auf das Feld der lokalen Variablen `val1` da bei den mittels Boxing `Struct1` zugewiesen wurde `val2`, eine Kopie des Werts vorgenommen wurde.</span><span class="sxs-lookup"><span data-stu-id="2bd27-240">The assignment to the field of the local variable `val2` does not impact the field of the local variable `val1` because when the boxed `Struct1` was assigned to `val2`, a copy of the value was made.</span></span> <span data-ttu-id="2bd27-241">Im Gegensatz dazu sind die Zuweisung `ref2.Value = 123` wirkt sich auf das Objekt, das sowohl `ref1` und `ref2` verweisen.</span><span class="sxs-lookup"><span data-stu-id="2bd27-241">In contrast, the assignment `ref2.Value = 123` affects the object that both `ref1` and `ref2` references.</span></span>

<span data-ttu-id="2bd27-242">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="2bd27-242">__Note.__</span></span> <span data-ttu-id="2bd27-243">Struktur das Kopieren erfolgt nicht für geschachtelte Strukturen, die als typisierte `System.ValueType` da es nicht möglich, die spätes Binden der ist `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-243">Structure copying is not done for boxed structures typed as `System.ValueType` because it is not possible to late bind off of `System.ValueType`.</span></span>

<span data-ttu-id="2bd27-244">Es gibt eine Ausnahme zur Regel, die Wert geschachtelt, die Typen bei Zuweisung kopiert werden.</span><span class="sxs-lookup"><span data-stu-id="2bd27-244">There is one exception to the rule that boxed value types will be copied on assignment.</span></span> <span data-ttu-id="2bd27-245">Wenn ein geschachtelter Werttyp-Verweis in einem anderen Typ gespeichert ist, wird der innere Verweis nicht kopiert werden.</span><span class="sxs-lookup"><span data-stu-id="2bd27-245">If a boxed value type reference is stored within another type, the inner reference will not be copied.</span></span> <span data-ttu-id="2bd27-246">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2bd27-246">For example:</span></span>

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

<span data-ttu-id="2bd27-247">Die Ausgabe des Programms lautet:</span><span class="sxs-lookup"><span data-stu-id="2bd27-247">The output of the program is:</span></span>

```
Values: 123, 123
```

<span data-ttu-id="2bd27-248">Dies ist, da es sich bei der innere geschachtelte Wert nicht kopiert wird, wenn der Wert kopiert werden.</span><span class="sxs-lookup"><span data-stu-id="2bd27-248">This is because the inner boxed value is not copied when the value is copied.</span></span> <span data-ttu-id="2bd27-249">Daher `val1.Value` und `val2.Value` einen Verweis auf den gleichen geschachtelten Werttyp.</span><span class="sxs-lookup"><span data-stu-id="2bd27-249">Thus, both `val1.Value` and `val2.Value` have a reference to the same boxed value type.</span></span>

<span data-ttu-id="2bd27-250">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="2bd27-250">__Note.__</span></span> <span data-ttu-id="2bd27-251">Die Tatsache, dass die inneren geschachtelten Werttypen nicht kopiert werden ist eine Einschränkung von .NET eingeben, um sicherzustellen, dass alle inneren geschachtelten Werttypen kopiert wurden, wenn ein Wert vom Typ System `Object` kopiert wurde wäre viel zu aufwendig.</span><span class="sxs-lookup"><span data-stu-id="2bd27-251">The fact that inner boxed value types are not copied is a limitation of the .NET type system -- to ensure that all inner boxed value types were copied whenever a value of type `Object` was copied would be prohibitively expensive.</span></span>

<span data-ttu-id="2bd27-252">Wie zuvor beschrieben, der geschachtelte Wert kann nur in ihren ursprünglichen Typ mittels Unboxing konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="2bd27-252">As previously described, boxed value types can only be unboxed to their original type.</span></span> <span data-ttu-id="2bd27-253">Primitive Typen, jedoch werden behandelt, speziell wenn-typisiert `Object`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-253">Boxed primitive types, however, are treated specially when typed as `Object`.</span></span> <span data-ttu-id="2bd27-254">Sie können in alle anderen primitiven Typen konvertiert werden, denen sich eine Konvertierung in befindet.</span><span class="sxs-lookup"><span data-stu-id="2bd27-254">They can be converted to any other primitive type that they have a conversion to.</span></span> <span data-ttu-id="2bd27-255">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2bd27-255">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim o As Object = 5
        Dim b As Byte = CByte(o)  ' Legal
        Console.WriteLine(b) ' Prints 5
    End Sub
End Module
```

<span data-ttu-id="2bd27-256">In der Regel die geschachtelte `Integer` Wert `5` konnte nicht in mittels Unboxing zurückkonvertiert werden eine `Byte` Variable.</span><span class="sxs-lookup"><span data-stu-id="2bd27-256">Normally, the boxed `Integer` value `5` could not be unboxed into a `Byte` variable.</span></span> <span data-ttu-id="2bd27-257">Aber da `Integer` und `Byte` primitive Typen und eine Konvertierung die Konvertierung zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="2bd27-257">However, because `Integer` and `Byte` are primitive types and have a conversion, the conversion is allowed.</span></span>

<span data-ttu-id="2bd27-258">Es ist wichtig zu beachten, dass einen Werttyp an eine Schnittstelle konvertieren anders als generisches Argument auf eine Schnittstelle beschränkt.</span><span class="sxs-lookup"><span data-stu-id="2bd27-258">It is important to note that converting a value type to an interface is different than a generic argument constrained to an interface.</span></span> <span data-ttu-id="2bd27-259">Beim Zugriff auf Schnittstellenmember für einen eingeschränkten Typparameter (oder Aufrufen von Methoden für `Object`), Boxing erfolgt nicht, wie bei der ein Werttyp konvertiert wird, auf eine Schnittstelle und Schnittstellenmember zugegriffen wird.</span><span class="sxs-lookup"><span data-stu-id="2bd27-259">When accessing interface members on a constrained type parameter (or calling methods on `Object`), boxing does not occur as it does when a value type is converted to an interface and an interface member is accessed.</span></span> <span data-ttu-id="2bd27-260">Nehmen wir beispielsweise an eine Schnittstelle `ICounter` enthält eine Methode `Increment` die können verwendet werden, um einen Wert zu ändern.</span><span class="sxs-lookup"><span data-stu-id="2bd27-260">For example, suppose an interface `ICounter` contains a method `Increment` which can be used to modify a value.</span></span> <span data-ttu-id="2bd27-261">Wenn `ICounter` dient als eine Einschränkung, die Implementierung der `Increment` Methode wird aufgerufen, mit einem Verweis auf die Variable, die `Increment` für nicht auf eine geschachtelte Kopie wurde aufgerufen:</span><span class="sxs-lookup"><span data-stu-id="2bd27-261">If `ICounter` is used as a constraint, the implementation of the `Increment` method is called with a reference to the variable that `Increment` was called on, not a boxed copy:</span></span>

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

<span data-ttu-id="2bd27-262">Der erste Aufruf `Increment` ändert den Wert in der Variablen `x`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-262">The first call to `Increment` modifies the value in the variable `x`.</span></span> <span data-ttu-id="2bd27-263">Dies entspricht nicht der zweite Aufruf von `Increment`, die geändert, dass des Wert in eine geschachtelte Kopie von `x`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-263">This is not equivalent to the second call to `Increment`, which modifies the value in a boxed copy of `x`.</span></span> <span data-ttu-id="2bd27-264">Daher ist die Ausgabe des Programms:</span><span class="sxs-lookup"><span data-stu-id="2bd27-264">Thus, the output of the program is:</span></span>

```
0
1
1
```

### <a name="nullable-value-type-conversions"></a><span data-ttu-id="2bd27-265">NULL-Wert-Typkonvertierungen</span><span class="sxs-lookup"><span data-stu-id="2bd27-265">Nullable Value Type Conversions</span></span>

<span data-ttu-id="2bd27-266">Ein Werttyp `T` können konvertieren in und aus der auf NULL festlegbare Version des Typs `T?`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-266">A value type `T` can convert to and from the nullable version of the type, `T?`.</span></span> <span data-ttu-id="2bd27-267">Die Konvertierung von `T?` zu `T` löst eine `System.InvalidOperationException` -Ausnahme aus, wenn der konvertierte Wert ist `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-267">The conversion from `T?` to `T` throws a `System.InvalidOperationException` exception if the value being converted is `Nothing`.</span></span> <span data-ttu-id="2bd27-268">Darüber hinaus `T?` verfügt über eine Konvertierung in einen Typ `S` Wenn `T` verfügt über eine integrierte Konvertierung in `S`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-268">Also, `T?` has a conversion to a type `S` if `T` has an intrinsic conversion to `S`.</span></span> <span data-ttu-id="2bd27-269">Und wenn `S` ein Werttyp ist, und klicken Sie dann die folgenden systeminternen Konvertierungen zwischen bestehen `T?` und `S?`:</span><span class="sxs-lookup"><span data-stu-id="2bd27-269">And if `S` is a value type, then the following intrinsic conversions exist between `T?` and `S?`:</span></span>

* <span data-ttu-id="2bd27-270">Eine Konvertierung der gleichen Klassifizierung (einschränken oder erweitern) aus `T?` zu `S?`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-270">A conversion of the same classification (narrowing or widening) from `T?` to `S?`.</span></span>

* <span data-ttu-id="2bd27-271">Eine Konvertierung der gleichen Klassifizierung (einschränken oder erweitern) aus `T` zu `S?`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-271">A conversion of the same classification (narrowing or widening) from `T` to `S?`.</span></span>

* <span data-ttu-id="2bd27-272">Eine einschränkende Konvertierung von `S?` zu `T`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-272">A narrowing conversion from `S?` to `T`.</span></span>

<span data-ttu-id="2bd27-273">Z. B. eine erweiternde Konvertierung vorhanden ist, aus `Integer?` zu `Long?` , da eine erweiternde Konvertierung von vorhanden `Integer` zu `Long`:</span><span class="sxs-lookup"><span data-stu-id="2bd27-273">For example, an intrinsic widening conversion exists from `Integer?` to `Long?` because an intrinsic widening conversion exists from `Integer` to `Long`:</span></span>

```vb
Dim i As Integer? = 10
Dim l As Long? = i
```

<span data-ttu-id="2bd27-274">Beim Konvertieren von `T?` zu `S?`, wenn der Wert des `T?` ist `Nothing`, klicken Sie dann den Wert der `S?` werden `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-274">When converting from `T?` to `S?`, if the value of `T?` is `Nothing`, then the value of `S?` will be `Nothing`.</span></span> <span data-ttu-id="2bd27-275">Beim Konvertieren von `S?` zu `T` oder `T?` zu `S`, wenn der Wert des `T?` oder `S?` ist `Nothing`, `System.InvalidCastException` Ausnahme wird ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="2bd27-275">When converting from `S?` to `T` or `T?` to `S`, if the value of `T?` or `S?` is `Nothing`, a `System.InvalidCastException` exception will be thrown.</span></span>

<span data-ttu-id="2bd27-276">Aufgrund des Verhaltens des zugrunde liegenden Typs `System.Nullable(Of T)`, wenn ein Werttyp `T?` ist geschachtelt, die das Ergebnis ist einen geschachtelten Wert vom Typ `T`, nicht auf einen geschachtelten Wert vom Typ `T?`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-276">Because of the behavior of the underlying type `System.Nullable(Of T)`, when a nullable value type `T?` is boxed, the result is a boxed value of type `T`, not a boxed value of type `T?`.</span></span> <span data-ttu-id="2bd27-277">Und im Gegenzug, eine auf NULL festlegbaren Werttyp Unboxing `T?`, der Wert von umbrochen `System.Nullable(Of T)`, und `Nothing` wird mittels Unboxing konvertiert, um einen null-Wert des Typs `T?`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-277">And, conversely, when unboxing to a nullable value type `T?`, the value will be wrapped by `System.Nullable(Of T)`, and `Nothing` will be unboxed to a null value of type `T?`.</span></span> <span data-ttu-id="2bd27-278">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2bd27-278">For example:</span></span>

```vb
Dim i1? As Integer = Nothing
Dim o1 As Object = i1

Console.WriteLine(o1 Is Nothing)                    ' Will print True
o1 = 10
i1 = CType(o1, Integer?)
Console.WriteLine(i1)                               ' Will print 10
```

<span data-ttu-id="2bd27-279">Ein Nebeneffekt, dass dieses Verhalten ist, geben Sie ein NULL-Wert `T?` angezeigt wird, implementiert alle Schnittstellen der `T`, da den Typ der zu schachtelnde konvertiert einen Werttyp an eine Schnittstelle benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="2bd27-279">A side effect of this behavior is that a nullable value type `T?` appears to implement all of the interfaces of `T`, because converting a value type to an interface requires the type to be boxed.</span></span> <span data-ttu-id="2bd27-280">Daher `T?` konvertiert werden kann, alle Schnittstellen, die `T` konvertierbar ist.</span><span class="sxs-lookup"><span data-stu-id="2bd27-280">As a result, `T?` is convertible to all the interfaces that `T` is convertible to.</span></span> <span data-ttu-id="2bd27-281">Es ist wichtig, aber beachten Sie, dass ein NULL-Wert `T?` implementiert die Schnittstellen nicht tatsächlich `T` im Rahmen der Überprüfung von generischen Einschränkungen oder Reflektion.</span><span class="sxs-lookup"><span data-stu-id="2bd27-281">It is important to note, however, that a nullable value type `T?` does not actually implement the interfaces of `T` for the purposes of generic constraint checking or reflection.</span></span> <span data-ttu-id="2bd27-282">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2bd27-282">For example:</span></span>

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

## <a name="string-conversions"></a><span data-ttu-id="2bd27-283">Zeichenfolgenkonvertierungen</span><span class="sxs-lookup"><span data-stu-id="2bd27-283">String Conversions</span></span>

<span data-ttu-id="2bd27-284">Konvertieren von `Char` in `String` führt zu einer Zeichenfolge, deren erste Zeichen der Zeichenwert ist.</span><span class="sxs-lookup"><span data-stu-id="2bd27-284">Converting `Char` into `String` results in a string whose first character is the character value.</span></span> <span data-ttu-id="2bd27-285">Konvertieren von `String` in `Char` führt zu einem Zeichen, dessen Wert das erste Zeichen der Zeichenfolge ist.</span><span class="sxs-lookup"><span data-stu-id="2bd27-285">Converting `String` into `Char` results in a character whose value is the first character of the string.</span></span> <span data-ttu-id="2bd27-286">Konvertiert ein Array von `Char` in `String` führt zu einer Zeichenfolge, deren Zeichen die Elemente des Arrays sind.</span><span class="sxs-lookup"><span data-stu-id="2bd27-286">Converting an array of `Char` into `String` results in a string whose characters are the elements of the array.</span></span> <span data-ttu-id="2bd27-287">Konvertieren von `String` in ein Array von `Char` führt ein Array von Zeichen, dessen Elemente die Zeichen der Zeichenfolge sind.</span><span class="sxs-lookup"><span data-stu-id="2bd27-287">Converting `String` into an array of `Char` results in an array of characters whose elements are the characters of the string.</span></span>

<span data-ttu-id="2bd27-288">Die genaue Konvertierungen zwischen `String` und `Boolean`, `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`, `Date`, und umgekehrt, sind Gegenstand dieser Spezifikation und Implementierung mit Ausnahme von einem Details hängen.</span><span class="sxs-lookup"><span data-stu-id="2bd27-288">The exact conversions between `String` and `Boolean`, `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`, `Date`, and vice versa, are beyond the scope of this specification and are implementation dependent with the exception of one detail.</span></span> <span data-ttu-id="2bd27-289">Zeichenfolgenkonvertierungen sollten Sie immer die aktuelle Kultur der Runtime-Umgebung.</span><span class="sxs-lookup"><span data-stu-id="2bd27-289">String conversions always consider the current culture of the run-time environment.</span></span> <span data-ttu-id="2bd27-290">Daher müssen sie zur Laufzeit ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="2bd27-290">As such, they must be performed at run time.</span></span>

## <a name="widening-conversions"></a><span data-ttu-id="2bd27-291">Erweiterungskonvertierungen</span><span class="sxs-lookup"><span data-stu-id="2bd27-291">Widening Conversions</span></span>

<span data-ttu-id="2bd27-292">Erweiternde Konvertierungen nie überlaufen jedoch können ein Genauigkeitsverlust gelten.</span><span class="sxs-lookup"><span data-stu-id="2bd27-292">Widening conversions never overflow but may entail a loss of precision.</span></span> <span data-ttu-id="2bd27-293">Die folgenden Konvertierungen sind erweiternde Konvertierungen auf:</span><span class="sxs-lookup"><span data-stu-id="2bd27-293">The following conversions are widening conversions:</span></span>

<span data-ttu-id="2bd27-294">__Identität/standardkonvertierungen__</span><span class="sxs-lookup"><span data-stu-id="2bd27-294">__Identity/Default conversions__</span></span>

* <span data-ttu-id="2bd27-295">Von einem Typ an sich selbst.</span><span class="sxs-lookup"><span data-stu-id="2bd27-295">From a type to itself.</span></span>

* <span data-ttu-id="2bd27-296">Von ein anonymen Delegaten-Typ, die für einen Lambda-Methode neuklassifizierung der jedem Delegattyp mit einer identischen Signatur generiert werden.</span><span class="sxs-lookup"><span data-stu-id="2bd27-296">From an anonymous delegate type generated for a lambda method reclassification to any delegate type with an identical signature.</span></span>

* <span data-ttu-id="2bd27-297">Durch das Literal `Nothing` auf einen Typ.</span><span class="sxs-lookup"><span data-stu-id="2bd27-297">From the literal `Nothing` to a type.</span></span>

<span data-ttu-id="2bd27-298">__Numerische Konvertierungen__</span><span class="sxs-lookup"><span data-stu-id="2bd27-298">__Numeric conversions__</span></span>

* <span data-ttu-id="2bd27-299">Von `Byte` zu `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, oder `Double`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-299">From `Byte` to `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="2bd27-300">Von `SByte` zu `Short`, `Integer`, `Long`, `Decimal`, `Single`, oder `Double`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-300">From `SByte` to `Short`, `Integer`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="2bd27-301">Von `UShort` zu `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, oder `Double`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-301">From `UShort` to `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="2bd27-302">Von `Short` zu `Integer`, `Long`, `Decimal`, `Single` oder `Double`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-302">From `Short` to `Integer`, `Long`, `Decimal`, `Single` or `Double`.</span></span>

* <span data-ttu-id="2bd27-303">Von `UInteger` zu `ULong`, `Long`, `Decimal`, `Single`, oder `Double`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-303">From `UInteger` to `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="2bd27-304">Von `Integer` zu `Long`, `Decimal`, `Single` oder `Double`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-304">From `Integer` to `Long`, `Decimal`, `Single` or `Double`.</span></span>

* <span data-ttu-id="2bd27-305">Von `ULong` zu `Decimal`, `Single`, oder `Double`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-305">From `ULong` to `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="2bd27-306">Von `Long` zu `Decimal`, `Single` oder `Double`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-306">From `Long` to `Decimal`, `Single` or `Double`.</span></span>

* <span data-ttu-id="2bd27-307">Von `Decimal` zu `Single` oder `Double`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-307">From `Decimal` to `Single` or `Double`.</span></span>

* <span data-ttu-id="2bd27-308">Von `Single` zu `Double`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-308">From `Single` to `Double`.</span></span>

* <span data-ttu-id="2bd27-309">Durch das Literal `0` für einen enumerierten Typ.</span><span class="sxs-lookup"><span data-stu-id="2bd27-309">From the literal `0` to an enumerated type.</span></span> <span data-ttu-id="2bd27-310">(__Beachten.__</span><span class="sxs-lookup"><span data-stu-id="2bd27-310">(__Note.__</span></span> <span data-ttu-id="2bd27-311">Die Konvertierung von `0` auf einen beliebigen enumerierten Typ ist zur Vereinfachung von Tests Flags erweitern.</span><span class="sxs-lookup"><span data-stu-id="2bd27-311">The conversion from `0` to any enumerated type is widening to simplify testing flags.</span></span> <span data-ttu-id="2bd27-312">Wenn z. B. `Values` ist ein enumerierter Typ mit einem Wert `One`, testen Sie eine Variable `v` des Typs `Values` Satz: `(v And Values.One) = 0`.)</span><span class="sxs-lookup"><span data-stu-id="2bd27-312">For example, if `Values` is an enumerated type with a value `One`, you could test a variable `v` of type `Values` by saying `(v And Values.One) = 0`.)</span></span>

* <span data-ttu-id="2bd27-313">Einen enumerierten Typ in den zugrunde liegenden numerischen Typ oder in einen numerischen Datentyp, dem der zugrunde liegenden numerische Typ eine erweiternde Konvertierung in verfügt.</span><span class="sxs-lookup"><span data-stu-id="2bd27-313">From an enumerated type to its underlying numeric type, or to a numeric type that its underlying numeric type has a widening conversion to.</span></span>

* <span data-ttu-id="2bd27-314">Aus einem konstanten Ausdruck vom Typ `ULong`, `Long`, `UInteger`, `Integer`, `UShort`, `Short`, `Byte`, oder `SByte` ein schmaler bereitgestellt der Wert des konstanten Ausdrucks wird in der der Bereich des Zieltyps.</span><span class="sxs-lookup"><span data-stu-id="2bd27-314">From a constant expression of type `ULong`, `Long`, `UInteger`, `Integer`, `UShort`, `Short`, `Byte`, or `SByte` to a narrower type, provided the value of the constant expression is within the range of the destination type.</span></span> <span data-ttu-id="2bd27-315">(__Beachten.__</span><span class="sxs-lookup"><span data-stu-id="2bd27-315">(__Note.__</span></span> <span data-ttu-id="2bd27-316">Konvertierungen von `UInteger` oder `Integer` zu `Single`, `ULong` oder `Long` zu `Single` oder `Double`, oder `Decimal` zu `Single` oder `Double` zu einem Genauigkeitsverlust führen können soll, aber nie Führen Sie einen Verlust der Größe.</span><span class="sxs-lookup"><span data-stu-id="2bd27-316">Conversions from `UInteger` or `Integer` to `Single`, `ULong` or `Long` to `Single` or `Double`, or `Decimal` to `Single` or `Double` may cause a loss of precision, but will never cause a loss of magnitude.</span></span> <span data-ttu-id="2bd27-317">Die anderen erweiterungskonvertierungen numerischen verlieren keine Informationen.)</span><span class="sxs-lookup"><span data-stu-id="2bd27-317">The other widening numeric conversions never lose any information.)</span></span>

<span data-ttu-id="2bd27-318">__Verweiskonvertierungen__</span><span class="sxs-lookup"><span data-stu-id="2bd27-318">__Reference conversions__</span></span>

* <span data-ttu-id="2bd27-319">Von einem Referenztyp zu einem Basistyp.</span><span class="sxs-lookup"><span data-stu-id="2bd27-319">From a reference type to a base type.</span></span>

* <span data-ttu-id="2bd27-320">Von einem Referenztyp in einen Schnittstellentyp, vorausgesetzt, dass der Typ der Schnittstelle oder eine Variante Schnittstelle implementiert.</span><span class="sxs-lookup"><span data-stu-id="2bd27-320">From a reference type to an interface type, provided that the type implements the interface or a variant compatible interface.</span></span>

* <span data-ttu-id="2bd27-321">Von einem Schnittstellentyp in `Object`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-321">From an interface type to `Object`.</span></span>

* <span data-ttu-id="2bd27-322">Von einem Schnittstellentyp in einen Typ variant-kompatible Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="2bd27-322">From an interface type to a variant compatible interface type.</span></span>

* <span data-ttu-id="2bd27-323">Delegieren Sie aus einem Delegattyp auf einen Variant-kompatibler Typ aus.</span><span class="sxs-lookup"><span data-stu-id="2bd27-323">From a delegate type to a variant compatible delegate type.</span></span> <span data-ttu-id="2bd27-324">(__Beachten.__</span><span class="sxs-lookup"><span data-stu-id="2bd27-324">(__Note.__</span></span> <span data-ttu-id="2bd27-325">Viele andere verweiskonvertierungen sind von diesen Regeln impliziert.</span><span class="sxs-lookup"><span data-stu-id="2bd27-325">Many other reference conversions are implied by these rules.</span></span> <span data-ttu-id="2bd27-326">Z. B. anonyme Delegaten sind Verweistypen, die von erben `System.MulticastDelegate`; Arraytypen sind Verweistypen, die von erben `System.Array`, anonyme Typen sind Referenztypen, die von erben `System.Object`.)</span><span class="sxs-lookup"><span data-stu-id="2bd27-326">For example, anonymous delegates are reference types that inherit from `System.MulticastDelegate`; array types are reference types that inherit from `System.Array`; anonymous types are reference types that inherit from `System.Object`.)</span></span>

<span data-ttu-id="2bd27-327">__Anonyme Delegaten-Konvertierungen__</span><span class="sxs-lookup"><span data-stu-id="2bd27-327">__Anonymous Delegate conversions__</span></span>

* <span data-ttu-id="2bd27-328">Von einem anonymen-Delegattyp einen Lambda-Methode begründet jedem größeren Delegattyp generiert.</span><span class="sxs-lookup"><span data-stu-id="2bd27-328">From an anonymous delegate type generated for a lambda method reclassification to any wider delegate type.</span></span>

<span data-ttu-id="2bd27-329">__Arraykonvertierungen__</span><span class="sxs-lookup"><span data-stu-id="2bd27-329">__Array conversions__</span></span>

* <span data-ttu-id="2bd27-330">Von einem Arraytyp `S` mit einem Elementtyp `Se` in einen Arraytyp `T` mit einem Elementtyp `Te`, sofern alle der folgenden Bedingungen erfüllt sind:</span><span class="sxs-lookup"><span data-stu-id="2bd27-330">From an array type `S` with an element type `Se` to an array type `T` with an element type `Te`, provided all of the following are true:</span></span>

  * <span data-ttu-id="2bd27-331">`S` und `T` unterscheiden sich nur hinsichtlich der Elementtyp.</span><span class="sxs-lookup"><span data-stu-id="2bd27-331">`S` and `T` differ only in element type.</span></span>

  * <span data-ttu-id="2bd27-332">Beide `Se` und `Te` sind Verweistypen oder bekanntermaßen Typparameter ein Verweistyp sein muss.</span><span class="sxs-lookup"><span data-stu-id="2bd27-332">Both `Se` and `Te` are reference types or are type parameters known to be a reference type.</span></span>

  * <span data-ttu-id="2bd27-333">Eine erweiternde Verweis, Array oder Typ konvertieren Parameter vorhanden ist, von `Se` zu `Te`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-333">A widening reference, array, or type parameter conversion exists from `Se` to `Te`.</span></span>

* <span data-ttu-id="2bd27-334">Von einem Arraytyp `S` mit einem enumerierten Elementtyp `Se` in einen Arraytyp `T` mit einem Elementtyp `Te`, sofern alle der folgenden Bedingungen erfüllt sind:</span><span class="sxs-lookup"><span data-stu-id="2bd27-334">From an array type `S` with an enumerated element type `Se` to an array type `T` with an element type `Te`, provided all of the following are true:</span></span>

  * <span data-ttu-id="2bd27-335">`S` und `T` unterscheiden sich nur hinsichtlich der Elementtyp.</span><span class="sxs-lookup"><span data-stu-id="2bd27-335">`S` and `T` differ only in element type.</span></span>

  * <span data-ttu-id="2bd27-336">`Te` ist der zugrunde liegenden Typ des `Se`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-336">`Te` is the underlying type of `Se`.</span></span>

* <span data-ttu-id="2bd27-337">Von einem Arraytyp `S` von Rang 1 mit einem enumerierten Elementtyp `Se`zu `System.Collections.Generic.IList(Of Te)`, `IReadOnlyList(Of Te)`, `ICollection(Of Te)`, `IReadOnlyCollection(Of Te)`, und `IEnumerable(Of Te)`, sofern eine der folgenden Aussagen zutrifft:</span><span class="sxs-lookup"><span data-stu-id="2bd27-337">From an array type `S` of rank 1 with an enumerated element type `Se`, to `System.Collections.Generic.IList(Of Te)`, `IReadOnlyList(Of Te)`, `ICollection(Of Te)`, `IReadOnlyCollection(Of Te)`, and `IEnumerable(Of Te)`, provided one of the following is true:</span></span>

  * <span data-ttu-id="2bd27-338">Beide `Se` und `Te` sind Verweistypen oder Parameter vom Typ bekannt als Referenz Typ und einem erweiternde Verweis, array oder typparameterumwandlung vorhanden ist, von `Se` zu `Te`; oder</span><span class="sxs-lookup"><span data-stu-id="2bd27-338">Both `Se` and `Te` are reference types or are type parameters known to be a reference type, and a widening reference, array, or type parameter conversion exists from `Se` to `Te`; or</span></span>

  * <span data-ttu-id="2bd27-339">`Te` ist der zugrunde liegenden Typ des `Se`; oder</span><span class="sxs-lookup"><span data-stu-id="2bd27-339">`Te` is the underlying type of `Se`; or</span></span>

  * <span data-ttu-id="2bd27-340">`Te` ist identisch mit `Se`</span><span class="sxs-lookup"><span data-stu-id="2bd27-340">`Te` is identical to `Se`</span></span>

<span data-ttu-id="2bd27-341">__Wert typkonvertierungen__</span><span class="sxs-lookup"><span data-stu-id="2bd27-341">__Value Type conversions__</span></span>

* <span data-ttu-id="2bd27-342">Von einem Werttyp in einen Basistyp.</span><span class="sxs-lookup"><span data-stu-id="2bd27-342">From a value type to a base type.</span></span>

* <span data-ttu-id="2bd27-343">Von einem Werttyp in einen Schnittstellentyp, den der Typ implementiert.</span><span class="sxs-lookup"><span data-stu-id="2bd27-343">From a value type to an interface type that the type implements.</span></span>

<span data-ttu-id="2bd27-344">__Auf NULL festlegbaren Werttyp-Konvertierungen__</span><span class="sxs-lookup"><span data-stu-id="2bd27-344">__Nullable Value Type conversions__</span></span>

* <span data-ttu-id="2bd27-345">Von einem Typ `T` in den Typ `T?`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-345">From a type `T` to the type `T?`.</span></span>

* <span data-ttu-id="2bd27-346">Von einem Typ `T?` auf einen Typ `S?`, wobei es eine erweiternde Konvertierung vom Typ ist `T` in den Typ `S`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-346">From a type `T?` to a type `S?`, where there is a widening conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="2bd27-347">Von einem Typ `T` auf einen Typ `S?`, wobei es eine erweiternde Konvertierung vom Typ ist `T` in den Typ `S`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-347">From a type `T` to a type `S?`, where there is a widening conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="2bd27-348">Von einem Typ `T?` Geben Sie auf eine Schnittstelle, die den Typ `T` implementiert.</span><span class="sxs-lookup"><span data-stu-id="2bd27-348">From a type `T?` to an interface type that the type `T` implements.</span></span>

<span data-ttu-id="2bd27-349">__Zeichenfolgenkonvertierungen__</span><span class="sxs-lookup"><span data-stu-id="2bd27-349">__String conversions__</span></span>

* <span data-ttu-id="2bd27-350">Von `Char` zu `String`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-350">From `Char` to `String`.</span></span>

* <span data-ttu-id="2bd27-351">Von `Char()` zu `String`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-351">From `Char()` to `String`.</span></span>

<span data-ttu-id="2bd27-352">__Typkonvertierungen für Parameter__</span><span class="sxs-lookup"><span data-stu-id="2bd27-352">__Type Parameter conversions__</span></span>

* <span data-ttu-id="2bd27-353">Von einem Typparameter um `Object`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-353">From a type parameter to `Object`.</span></span>

* <span data-ttu-id="2bd27-354">Von einem Typparameter eine schnittstelleneinschränkung-Typ oder eine Schnittstelle-Variante, die mit einer Schnittstelle typeinschränkung kompatibel.</span><span class="sxs-lookup"><span data-stu-id="2bd27-354">From a type parameter to an interface type constraint or any interface variant compatible with an interface type constraint.</span></span>

* <span data-ttu-id="2bd27-355">Von einem Typparameter an eine Schnittstelle, die durch eine klasseneinschränkung implementiert.</span><span class="sxs-lookup"><span data-stu-id="2bd27-355">From a type parameter to an interface implemented by a class constraint.</span></span>

* <span data-ttu-id="2bd27-356">Von einem Typparameter eine Schnittstelle-Variante, die kompatibel mit einer Schnittstelle, die durch eine klasseneinschränkung implementiert.</span><span class="sxs-lookup"><span data-stu-id="2bd27-356">From a type parameter to an interface variant compatible with an interface implemented by a class constraint.</span></span>

* <span data-ttu-id="2bd27-357">Von einem Typparameter ein Class-Einschränkung oder einem Basistyp von der Class-Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="2bd27-357">From a type parameter to a class constraint, or a base type of the class constraint.</span></span>

* <span data-ttu-id="2bd27-358">Von einem Typparameter `T` zu einer Einschränkung eines Typparameters `Tx`, oder etwas `Tx` verfügt über eine erweiternde Konvertierung in.</span><span class="sxs-lookup"><span data-stu-id="2bd27-358">From a type parameter `T` to a type parameter constraint `Tx`, or anything `Tx` has a widening conversion to.</span></span>

## <a name="narrowing-conversions"></a><span data-ttu-id="2bd27-359">Eingrenzungskonvertierungen</span><span class="sxs-lookup"><span data-stu-id="2bd27-359">Narrowing Conversions</span></span>

<span data-ttu-id="2bd27-360">Einschränkende Konvertierungen sind Konvertierungen, die nachgewiesen werden können nicht immer erfolgreich ist, Konvertierungen, die bekannt ist, dass Sie möglicherweise Informationen verloren gehen und Konvertierungen domänenübergreifend Typen ausreichend verschieden sind, sollten einschränkende Notation an.</span><span class="sxs-lookup"><span data-stu-id="2bd27-360">Narrowing conversions are conversions that cannot be proved to always succeed, conversions that are known to possibly lose information, and conversions across domains of types sufficiently different to merit narrowing notation.</span></span> <span data-ttu-id="2bd27-361">Die folgenden Konvertierungen werden als einschränkende Konvertierungen klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="2bd27-361">The following conversions are classified as narrowing conversions:</span></span>

<span data-ttu-id="2bd27-362">__Boolesche Konvertierungen__</span><span class="sxs-lookup"><span data-stu-id="2bd27-362">__Boolean conversions__</span></span>

* <span data-ttu-id="2bd27-363">Von `Boolean` zu `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, oder `Double`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-363">From `Boolean` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="2bd27-364">Von `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, oder `Double` zu `Boolean`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-364">From `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double` to `Boolean`.</span></span>

<span data-ttu-id="2bd27-365">__Numerische Konvertierungen__</span><span class="sxs-lookup"><span data-stu-id="2bd27-365">__Numeric conversions__</span></span>

* <span data-ttu-id="2bd27-366">Von `Byte` zu `SByte`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-366">From `Byte` to `SByte`.</span></span>

* <span data-ttu-id="2bd27-367">Von `SByte` zu `Byte`, `UShort`, `UInteger`, oder `ULong`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-367">From `SByte` to `Byte`, `UShort`, `UInteger`, or `ULong`.</span></span>

* <span data-ttu-id="2bd27-368">Von `UShort` zu `Byte`, `SByte`, oder `Short`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-368">From `UShort` to `Byte`, `SByte`, or `Short`.</span></span>

* <span data-ttu-id="2bd27-369">Von `Short` zu `Byte`, `SByte`, `UShort`, `UInteger`, oder `ULong`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-369">From `Short` to `Byte`, `SByte`, `UShort`, `UInteger`, or `ULong`.</span></span>

* <span data-ttu-id="2bd27-370">Von `UInteger` zu `Byte`, `SByte`, `UShort`, `Short`, oder `Integer`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-370">From `UInteger` to `Byte`, `SByte`, `UShort`, `Short`, or `Integer`.</span></span>

* <span data-ttu-id="2bd27-371">Von `Integer` zu `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, oder `ULong`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-371">From `Integer` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, or `ULong`.</span></span>

* <span data-ttu-id="2bd27-372">Von `ULong` zu `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, oder `Long`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-372">From `ULong` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, or `Long`.</span></span>

* <span data-ttu-id="2bd27-373">Von `Long` zu `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, oder `ULong`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-373">From `Long` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, or `ULong`.</span></span>

* <span data-ttu-id="2bd27-374">Von `Decimal` zu `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, oder `Long`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-374">From `Decimal` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, or `Long`.</span></span>

* <span data-ttu-id="2bd27-375">Von `Single` zu `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, oder `Decimal`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-375">From `Single` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, or `Decimal`.</span></span>

* <span data-ttu-id="2bd27-376">Von `Double` zu `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, oder `Single`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-376">From `Double` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, or `Single`.</span></span>

* <span data-ttu-id="2bd27-377">Aus einem numerischen Typ für einen enumerierten Typ.</span><span class="sxs-lookup"><span data-stu-id="2bd27-377">From a numeric type to an enumerated type.</span></span>

* <span data-ttu-id="2bd27-378">Einen enumerierten Typ in einen numerischen Typ ist der zugrunde liegenden numerische Typ eine einschränkende Konvertierung in.</span><span class="sxs-lookup"><span data-stu-id="2bd27-378">From an enumerated type to a numeric type its underlying numeric type has a narrowing conversion to.</span></span>

* <span data-ttu-id="2bd27-379">Einen enumerierten Typ in einen anderen enumerierten Typ.</span><span class="sxs-lookup"><span data-stu-id="2bd27-379">From an enumerated type to another enumerated type.</span></span>

<span data-ttu-id="2bd27-380">__Verweiskonvertierungen__</span><span class="sxs-lookup"><span data-stu-id="2bd27-380">__Reference conversions__</span></span>

* <span data-ttu-id="2bd27-381">Von einem Referenztyp zu einem stärker abgeleiteten Typ.</span><span class="sxs-lookup"><span data-stu-id="2bd27-381">From a reference type to a more derived type.</span></span>

* <span data-ttu-id="2bd27-382">Von einem Klassentyp in einen Schnittstellentyp angegeben, dass der Klassentyp der Schnittstellentyp oder eine Variante der Schnittstelle Typ kompatibel nicht implementiert.</span><span class="sxs-lookup"><span data-stu-id="2bd27-382">From a class type to an interface type, provided the class type does not implement the interface type or an interface type variant compatible with it.</span></span>

* <span data-ttu-id="2bd27-383">Von einem Schnittstellentyp in einen Klassentyp.</span><span class="sxs-lookup"><span data-stu-id="2bd27-383">From an interface type to a class type.</span></span>

* <span data-ttu-id="2bd27-384">Typ in einen anderen Schnittstellentyp, sofern von einer Schnittstelle ist keine vererbungsbeziehung zwischen den beiden Typen und vorausgesetzt, dass sie nicht variant kompatibel sind.</span><span class="sxs-lookup"><span data-stu-id="2bd27-384">From an interface type to another interface type, provided there is no inheritance relationship between the two types and provided they are not variant compatible.</span></span>

<span data-ttu-id="2bd27-385">__Anonyme Delegaten-Konvertierungen__</span><span class="sxs-lookup"><span data-stu-id="2bd27-385">__Anonymous Delegate conversions__</span></span>

* <span data-ttu-id="2bd27-386">Von einem anonymen Delegaten-Typ für einen Lambda-Methode neuklassifizierung von jedem schmaler Delegattyp generiert.</span><span class="sxs-lookup"><span data-stu-id="2bd27-386">From an anonymous delegate type generated for a lambda method reclassification to any narrower delegate type.</span></span>

<span data-ttu-id="2bd27-387">__Arraykonvertierungen__</span><span class="sxs-lookup"><span data-stu-id="2bd27-387">__Array conversions__</span></span>

* <span data-ttu-id="2bd27-388">Von einem Arraytyp `S` mit einem Elementtyp `Se`, in einen Arraytyp `T` mit einem Elementtyp `Te`, vorausgesetzt, dass alle der folgenden Bedingungen erfüllt sind:</span><span class="sxs-lookup"><span data-stu-id="2bd27-388">From an array type `S` with an element type `Se`, to an array type `T` with an element type `Te`, provided that all of the following are true:</span></span>

  * <span data-ttu-id="2bd27-389">`S` und `T` unterscheiden sich nur hinsichtlich der Elementtyp.</span><span class="sxs-lookup"><span data-stu-id="2bd27-389">`S` and `T` differ only in element type.</span></span>
  * <span data-ttu-id="2bd27-390">Beide `Se` und `Te` sind Verweistypen oder Parameter vom Typ nicht bekanntermaßen Werttypen.</span><span class="sxs-lookup"><span data-stu-id="2bd27-390">Both `Se` and `Te` are reference types or are type parameters not known to be value types.</span></span>
  * <span data-ttu-id="2bd27-391">Eine einschränkende Verweis, Array oder typparameterumwandlung vorhanden ist, von `Se` zu `Te`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-391">A narrowing reference, array, or type parameter conversion exists from `Se` to `Te`.</span></span>

* <span data-ttu-id="2bd27-392">Von einem Arraytyp `S` mit einem Elementtyp `Se` in einen Arraytyp `T` mit einem enumerierten Elementtyp `Te`, sofern alle der folgenden Bedingungen erfüllt sind:</span><span class="sxs-lookup"><span data-stu-id="2bd27-392">From an array type `S` with an element type `Se` to an array type `T` with an enumerated element type `Te`, provided all of the following are true:</span></span>

  * <span data-ttu-id="2bd27-393">`S` und `T` unterscheiden sich nur hinsichtlich der Elementtyp.</span><span class="sxs-lookup"><span data-stu-id="2bd27-393">`S` and `T` differ only in element type.</span></span>
  * <span data-ttu-id="2bd27-394">`Se` ist der zugrunde liegenden Typ des `Te` , oder sie sind beide unterschiedlichen Enumerationstypen, die den gleichen zugrunde liegenden Typ aufweisen.</span><span class="sxs-lookup"><span data-stu-id="2bd27-394">`Se` is the underlying type of `Te` , or they are both different enumerated types that share the same underlying type.</span></span>

* <span data-ttu-id="2bd27-395">Von einem Arraytyp `S` von Rang 1 mit einem enumerierten Elementtyp `Se`zu `IList(Of Te)`, `IReadOnlyList(Of Te)`, `ICollection(Of Te)`, `IReadOnlyCollection(Of Te)` und `IEnumerable(Of Te)`, sofern eine der folgenden Aussagen zutrifft:</span><span class="sxs-lookup"><span data-stu-id="2bd27-395">From an array type `S` of rank 1 with an enumerated element type `Se`, to `IList(Of Te)`, `IReadOnlyList(Of Te)`, `ICollection(Of Te)`, `IReadOnlyCollection(Of Te)` and `IEnumerable(Of Te)`, provided one of the following is true:</span></span>

  * <span data-ttu-id="2bd27-396">Beide `Se` und `Te` sind Verweistypen oder bekanntermaßen Typparameter ein Verweistyp sein und eine einschränkende Verweis, Array oder typparameterumwandlung vorhanden ist, von `Se` zu `Te`; oder</span><span class="sxs-lookup"><span data-stu-id="2bd27-396">Both `Se` and `Te` are reference types or are type parameters known to be a reference type, and a narrowing reference, array, or type parameter conversion exists from `Se` to `Te`; or</span></span>
  * <span data-ttu-id="2bd27-397">`Se` ist der zugrunde liegenden Typ des `Te`, oder sie sind beide unterschiedlichen Enumerationstypen, die den gleichen zugrunde liegenden Typ aufweisen.</span><span class="sxs-lookup"><span data-stu-id="2bd27-397">`Se` is the underlying type of `Te`, or they are both different enumerated types that share the same underlying type.</span></span>

<span data-ttu-id="2bd27-398">__Wert typkonvertierungen__</span><span class="sxs-lookup"><span data-stu-id="2bd27-398">__Value type conversions__</span></span>

* <span data-ttu-id="2bd27-399">Von einem Referenztyp zu einem stärker abgeleiteten Werttyp.</span><span class="sxs-lookup"><span data-stu-id="2bd27-399">From a reference type to a more derived value type.</span></span>

* <span data-ttu-id="2bd27-400">Von einem Schnittstellentyp in einen Werttyp sofern der Werttyp den Schnittstellentyp implementiert.</span><span class="sxs-lookup"><span data-stu-id="2bd27-400">From an interface type to a value type, provided the value type implements the interface type.</span></span>

<span data-ttu-id="2bd27-401">__Auf NULL festlegbaren Werttyp-Konvertierungen__</span><span class="sxs-lookup"><span data-stu-id="2bd27-401">__Nullable Value Type conversions__</span></span>

* <span data-ttu-id="2bd27-402">Von einem Typ `T?` auf einen Typ `T`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-402">From a type `T?` to a type `T`.</span></span>

* <span data-ttu-id="2bd27-403">Von einem Typ `T?` auf einen Typ `S?`, bei dem es eine einschränkende Konvertierung vom Typ ist `T` in den Typ `S`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-403">From a type `T?` to a type `S?`, where there is a narrowing conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="2bd27-404">Von einem Typ `T` auf einen Typ `S?`, bei dem es eine einschränkende Konvertierung vom Typ ist `T` in den Typ `S`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-404">From a type `T` to a type `S?`, where there is a narrowing conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="2bd27-405">Von einem Typ `S?` auf einen Typ `T`, bei dem es eine Konvertierung vom Typ erfolgt `S` in den Typ `T`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-405">From a type `S?` to a type `T`, where there is a conversion from the type `S` to the type `T`.</span></span>

<span data-ttu-id="2bd27-406">__Zeichenfolgenkonvertierungen__</span><span class="sxs-lookup"><span data-stu-id="2bd27-406">__String conversions__</span></span>

* <span data-ttu-id="2bd27-407">Von `String` zu `Char`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-407">From `String` to `Char`.</span></span>

* <span data-ttu-id="2bd27-408">Von `String` zu `Char()`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-408">From `String` to `Char()`.</span></span>

* <span data-ttu-id="2bd27-409">Von `String` zu `Boolean` und `Boolean` zu `String`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-409">From `String` to `Boolean` and from `Boolean` to `String`.</span></span>

* <span data-ttu-id="2bd27-410">Konvertierungen zwischen `String` und `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, oder `Double`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-410">Conversions between `String` and `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="2bd27-411">Von `String` zu `Date` und `Date` zu `String`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-411">From `String` to `Date` and from `Date` to `String`.</span></span>

<span data-ttu-id="2bd27-412">__Typkonvertierungen für Parameter__</span><span class="sxs-lookup"><span data-stu-id="2bd27-412">__Type Parameter conversions__</span></span>

* <span data-ttu-id="2bd27-413">Von `Object` auf einen Typparameter.</span><span class="sxs-lookup"><span data-stu-id="2bd27-413">From `Object` to a type parameter.</span></span>

* <span data-ttu-id="2bd27-414">Parameter in einen Schnittstellentyp, den Type-Parameter angegeben, ist von einem Typ nicht beschränkt auf die Schnittstelle oder beschränkt auf eine Klasse, die diese Schnittstelle implementiert.</span><span class="sxs-lookup"><span data-stu-id="2bd27-414">From a type parameter to an interface type, provided the type parameter is not constrained to that interface or constrained to a class that implements that interface.</span></span>

* <span data-ttu-id="2bd27-415">Von einem Schnittstellentyp für einen Typparameter.</span><span class="sxs-lookup"><span data-stu-id="2bd27-415">From an interface type to a type parameter.</span></span>

* <span data-ttu-id="2bd27-416">Von einem Typparameter für einen abgeleiteten Typ von einer Class-Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="2bd27-416">From a type parameter to a derived type of a class constraint.</span></span>

* <span data-ttu-id="2bd27-417">Von einem Typparameter `T` was immer einer Einschränkung eines Typparameters `Tx` verfügt über eine einschränkende Konvertierung in.</span><span class="sxs-lookup"><span data-stu-id="2bd27-417">From a type parameter `T` to anything a type parameter constraint `Tx` has a narrowing conversion to.</span></span>

## <a name="type-parameter-conversions"></a><span data-ttu-id="2bd27-418">Typkonvertierungen für Parameter</span><span class="sxs-lookup"><span data-stu-id="2bd27-418">Type Parameter Conversions</span></span>

<span data-ttu-id="2bd27-419">Typparameter Konvertierungen werden durch die Einschränkungen, bestimmt, wenn vorhanden, legen Sie auf diese.</span><span class="sxs-lookup"><span data-stu-id="2bd27-419">Type parameters' conversions are determined by the constraints, if any, put on them.</span></span> <span data-ttu-id="2bd27-420">Ein Typparameter `T` immer auf sich selbst konvertiert werden kann, in und aus `Object`, und aus einem Schnittstellentyp.</span><span class="sxs-lookup"><span data-stu-id="2bd27-420">A type parameter `T` can always be converted to itself, to and from `Object`, and to and from any interface type.</span></span> <span data-ttu-id="2bd27-421">Beachten Sie, dass bei den Datentyp `T` ist ein Werttyp zur Laufzeit, Konvertieren von `T` zu `Object` oder ein Schnittstellentyp wird ein Boxing-Konvertierung und Konvertieren von `Object` oder eine Schnittstelle, um geben `T` werden ein unboxing die Konvertierung.</span><span class="sxs-lookup"><span data-stu-id="2bd27-421">Note that if the type `T` is a value type at run-time, converting from `T` to `Object` or an interface type will be a boxing conversion and converting from `Object` or an interface type to `T` will be an unboxing conversion.</span></span> <span data-ttu-id="2bd27-422">Ein Typparameter mit einer Class-Einschränkung `C` definiert zusätzliche Konvertierungen aus der Typparameter `C` und deren Basisklassen, und umgekehrt.</span><span class="sxs-lookup"><span data-stu-id="2bd27-422">A type parameter with a class constraint `C` defines additional conversions from the type parameter to `C` and its base classes, and vice versa.</span></span> <span data-ttu-id="2bd27-423">Ein Typparameter `T` mit einer Einschränkung eines Typparameters `Tx` definiert eine Konvertierung in `Tx` und alles, was `Tx` in konvertiert.</span><span class="sxs-lookup"><span data-stu-id="2bd27-423">A type parameter `T` with a type parameter constraint `Tx` defines a conversion to `Tx` and anything `Tx` converts to.</span></span>

<span data-ttu-id="2bd27-424">Ein Array, dessen Elementtyp ein Typparameter eine schnittstelleneinschränkung ist `I` hat die gleichen arraykonvertierungen von kovarianten als ein Array, dessen Elementtyp `I`, vorausgesetzt, dass der Typparameter verfügt auch über eine `Class` oder Einschränkung (-Klasse Da nur Verweis-Arrayelementtypen kovariant sein können).</span><span class="sxs-lookup"><span data-stu-id="2bd27-424">An array whose element type is a type parameter with an interface constraint `I` has the same covariant array conversions as an array whose element type is `I`, provided that the type parameter also has a `Class` or class constraint (since only reference array element types can be covariant).</span></span> <span data-ttu-id="2bd27-425">Ein Array, dessen Elementtyp ein Typparameter mit einer Class-Einschränkung ist `C` hat die gleichen arraykonvertierungen von kovarianten als ein Array, dessen Elementtyp `C`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-425">An array whose element type is a type parameter with a class constraint `C` has the same covariant array conversions as an array whose element type is `C`.</span></span>

<span data-ttu-id="2bd27-426">Die oben genannten Regeln für Konvertierungen erlauben keine Konvertierungen von uneingeschränkte Typparameter auf Typen von nicht-Schnittstelle kann überraschend sein.</span><span class="sxs-lookup"><span data-stu-id="2bd27-426">The above conversions rules do not permit conversions from unconstrained type parameters to non-interface types, which may be surprising.</span></span> <span data-ttu-id="2bd27-427">Der Grund dafür ist, um Verwechslungen über die Semantik der solche Konvertierungen.</span><span class="sxs-lookup"><span data-stu-id="2bd27-427">The reason for this is to prevent confusion about the semantics of such conversions.</span></span> <span data-ttu-id="2bd27-428">Betrachten Sie beispielsweise die folgende Deklaration:</span><span class="sxs-lookup"><span data-stu-id="2bd27-428">For example, consider the following declaration:</span></span>

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long 
        Return CLng(t)    ' Error, explicit conversion not permitted
    End Function
End Class
```

<span data-ttu-id="2bd27-429">Wenn die Konvertierung von `T` zu `Integer` erteilt wurden, wird einfach erwartet, `X(Of Integer).F(7)` zurück `7L`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-429">If the conversion of `T` to `Integer` were permitted, one might easily expect that `X(Of Integer).F(7)` would return `7L`.</span></span> <span data-ttu-id="2bd27-430">Es wird jedoch nicht der Fall, da numerische Konvertierungen nur gelten, wenn die Typen bekannt ist, dass zum Zeitpunkt der Kompilierung ein Zahlenwert angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="2bd27-430">However, it would not, because numeric conversions are only considered when the types are known to be numeric at compile time.</span></span> <span data-ttu-id="2bd27-431">Um die Semantik machen muss klare und im obige Beispiel stattdessen geschrieben werden:</span><span class="sxs-lookup"><span data-stu-id="2bd27-431">In order to make the semantics clear, the above example must instead be written:</span></span>

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long
        Return CLng(CObj(t))    ' OK, conversions permitted
    End Function
End Class
```

## <a name="user-defined-conversions"></a><span data-ttu-id="2bd27-432">Benutzerdefinierte Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="2bd27-432">User-Defined Conversions</span></span>

<span data-ttu-id="2bd27-433">*Systeminterne Konvertierungen* sind Konvertierungen, die von der Programmiersprache (z. B. in dieser Spezifikation aufgeführt), während definiert *benutzerdefinierte Konvertierungen* werden durch Überladung definiert die `CType` Operator.</span><span class="sxs-lookup"><span data-stu-id="2bd27-433">*Intrinsic conversions* are conversions defined by the language (i.e. listed in this specification), while *user-defined conversions* are defined by overloading the `CType` operator.</span></span> <span data-ttu-id="2bd27-434">Wenn zwischen Typen konvertiert werden soll, wenn keine systeminternen Konvertierungen gelten, werden benutzerdefinierte Konvertierungen angesehen.</span><span class="sxs-lookup"><span data-stu-id="2bd27-434">When converting between types, if no intrinsic conversions are applicable then user-defined conversions will be considered.</span></span> <span data-ttu-id="2bd27-435">Wenn es gibt eine benutzerdefinierte Konvertierung, die *spezifischste* für die Quelle und Ziel-Typen, klicken Sie dann die benutzerdefinierte Konvertierung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="2bd27-435">If there is a user-defined conversion that is *most specific* for the source and target types, then the user-defined conversion will be used.</span></span> <span data-ttu-id="2bd27-436">Andernfalls führt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="2bd27-436">Otherwise, a compile-time error results.</span></span> <span data-ttu-id="2bd27-437">Die spezifischsten Konvertierung wird, deren Operanden "nächstgelegene" in den Quelltyp und, dessen Ergebnistyp "nächstgelegene" in den Zieltyp.</span><span class="sxs-lookup"><span data-stu-id="2bd27-437">The most specific conversion is the one whose operand is "closest" to the source type and whose result type is "closest" to the target type.</span></span> <span data-ttu-id="2bd27-438">Wenn Sie welche benutzerdefinierte Konvertierung verwenden zu bestimmen, wird die spezifischste erweiternde Konvertierung verwendet. Wenn keine widening ist die Konvertierung am spezifischsten, die spezifischste einschränkende Konvertierung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="2bd27-438">When determining what user-defined conversion to use, the most specific widening conversion will be used; if no widening conversion is most specific, the most specific narrowing conversion will be used.</span></span> <span data-ttu-id="2bd27-439">Wenn nicht am genauesten einschränkende Konvertierung vorhanden ist, klicken Sie dann die Konvertierung nicht definiert ist und ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="2bd27-439">If there is no most specific narrowing conversion, then the conversion is undefined and a compile-time error occurs.</span></span>

<span data-ttu-id="2bd27-440">Die folgenden Abschnitte enthalten, wie die spezifischsten Konvertierungen bestimmt werden.</span><span class="sxs-lookup"><span data-stu-id="2bd27-440">The following sections cover how the most specific conversions are determined.</span></span> <span data-ttu-id="2bd27-441">Sie können die folgenden Begriffe verwendet:</span><span class="sxs-lookup"><span data-stu-id="2bd27-441">They use the following terms:</span></span>

<span data-ttu-id="2bd27-442">Wenn eine systeminterne erweiternde Konvertierung vorhanden ist, von einem Typ `A` auf einen Typ `B`, und wenn weder `A` noch `B` Schnittstellen sind `A` ist *darin enthaltenen* von `B`, und `B` *umfasst* `A`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-442">If an intrinsic widening conversion exists from a type `A` to a type `B`, and if neither `A` nor `B` are interfaces, then `A` is *encompassed* by `B`, and `B` *encompasses* `A`.</span></span>

<span data-ttu-id="2bd27-443">Die *umfassendste* Typ in einen Satz von Typen ist, der eine Typ, der alle anderen Typen in der Menge umfasst.</span><span class="sxs-lookup"><span data-stu-id="2bd27-443">The *most encompassing* type in a set of types is the one type that encompasses all other types in the set.</span></span> <span data-ttu-id="2bd27-444">Wenn kein Typ auf alle anderen Typen umfasst, hat dann die Gruppe keine umfassendste.</span><span class="sxs-lookup"><span data-stu-id="2bd27-444">If no single type encompasses all other types, then the set has no most encompassing type.</span></span> <span data-ttu-id="2bd27-445">Intuitiv ausgedrückt ist der umfassendste Typ "größten"-Typs in der Gruppe: der eine Typ, in dem alle anderen Typen über eine erweiternde Konvertierung konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="2bd27-445">In intuitive terms, the most encompassing type is the "largest" type in the set -- the one type to which each of the other types can be converted through a widening conversion.</span></span>

<span data-ttu-id="2bd27-446">Die *die darin enthaltenen* Typ in einen Satz von Typen ist, der eine Typ, der von allen anderen Typen in der Gruppe Datenbankprotokolls enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="2bd27-446">The *most encompassed* type in a set of types is the one type that is encompassed by all other types in the set.</span></span> <span data-ttu-id="2bd27-447">Wenn kein Typ von allen anderen Typen Datenbankprotokolls enthalten ist, hat die Gruppe nicht am häufigsten Typ einschließt.</span><span class="sxs-lookup"><span data-stu-id="2bd27-447">If no single type is encompassed by all other types, then the set has no most encompassed type.</span></span> <span data-ttu-id="2bd27-448">Intuitiv ausgedrückt ist der am stärksten umfasste Typ den "kleinsten" Datentyp in der Gruppe: der eine Typ, der in jeder der anderen Typen über eine einschränkende Konvertierung konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="2bd27-448">In intuitive terms, the most encompassed type is the "smallest" type in the set -- the one type that can be converted to each of the other types through a narrowing conversion.</span></span>

<span data-ttu-id="2bd27-449">Wenn die benutzerdefinierte Konvertierungen mit Kandidat für einen Typ sammeln `T?`, definierten Operatoren für die benutzerdefinierte Konvertierung `T` stattdessen verwendet.</span><span class="sxs-lookup"><span data-stu-id="2bd27-449">When collecting the candidate user-defined conversions for a type `T?`, the user-defined conversion operators defined by `T` are used instead.</span></span> <span data-ttu-id="2bd27-450">Wenn der Typ konvertiert wird, um auch NULL-Werte zulassen, und klicken Sie dann eine der `T`des benutzerdefinierten Operatoren für Konvertierungen, bei denen nur die nicht auf NULL festlegbare Werttypen aufgehoben werden.</span><span class="sxs-lookup"><span data-stu-id="2bd27-450">If the type being converted to is also a nullable value type, then any of `T`'s user-defined conversions operators that involve only non-nullable value types are lifted.</span></span> <span data-ttu-id="2bd27-451">Einen Konvertierungsoperator `T` zu `S` transformiert wird, um eine Konvertierung von `T?` zu `S?` und wird durch die Konvertierung ausgewertet `T?` zu `T`, falls erforderlich, und klicken Sie dann die benutzerdefinierte Konvertierung auswerten Operator aus `T` zu `S` und dann wandle `S` zu `S?`, falls erforderlich.</span><span class="sxs-lookup"><span data-stu-id="2bd27-451">A conversion operator from `T` to `S` is lifted to be a conversion from `T?` to `S?` and is evaluated by converting `T?` to `T`, if necessary, then evaluating the user-defined conversion operator from `T` to `S` and then converting `S` to `S?`, if necessary.</span></span> <span data-ttu-id="2bd27-452">Wenn der konvertierte Wert ist `Nothing`, ein transformierten Konvertierungsoperator konvertiert jedoch direkt in einen Wert von `Nothing` als `S?`.</span><span class="sxs-lookup"><span data-stu-id="2bd27-452">If the value being converted is `Nothing`, however, a lifted conversion operator converts directly into a value of `Nothing` typed as `S?`.</span></span> <span data-ttu-id="2bd27-453">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2bd27-453">For example:</span></span>

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

<span data-ttu-id="2bd27-454">Beim Auflösen von Konvertierungen, die eine benutzerdefinierte werden Operatoren für Konvertierungen immer über transformierten Konvertierungsoperatoren bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="2bd27-454">When resolving conversions, user-defined conversions operators are always preferred over lifted conversion operators.</span></span> <span data-ttu-id="2bd27-455">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2bd27-455">For example:</span></span>

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

<span data-ttu-id="2bd27-456">Zur Laufzeit kann das Auswerten einer benutzerdefinierten Konvertierung bis zu drei Schritte umfassen:</span><span class="sxs-lookup"><span data-stu-id="2bd27-456">At run-time, evaluating a user-defined conversion can involve up to three steps:</span></span>

1. <span data-ttu-id="2bd27-457">Zunächst wird der Wert aus einer Datenquelle in der Operandentyp, verwenden eine Konvertierung, bei Bedarf konvertiert.</span><span class="sxs-lookup"><span data-stu-id="2bd27-457">First, the value is converted from the source type to the operand type using an intrinsic conversion, if necessary.</span></span>

2. <span data-ttu-id="2bd27-458">Anschließend wird die benutzerdefinierte Konvertierung aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="2bd27-458">Then, the user-defined conversion is invoked.</span></span>

3. <span data-ttu-id="2bd27-459">Schließlich wird das Ergebnis der eine benutzerdefinierte Konvertierung in den Zieltyp an, mit der eine Konvertierung, bei Bedarf konvertiert.</span><span class="sxs-lookup"><span data-stu-id="2bd27-459">Finally, the result of the user-defined conversion is converted to the target type using an intrinsic conversion, if necessary.</span></span>

<span data-ttu-id="2bd27-460">Es ist wichtig zu beachten, dass die Auswertung einer benutzerdefinierten Konvertierung umfasst nie mehr als eine benutzerdefinierte Konvertierung-Operator.</span><span class="sxs-lookup"><span data-stu-id="2bd27-460">It is important to note that evaluation of a user-defined conversion will never involve more than one user-defined conversion operator.</span></span>

### <a name="most-specific-widening-conversion"></a><span data-ttu-id="2bd27-461">Am spezifischsten ist eine erweiternde Konvertierung</span><span class="sxs-lookup"><span data-stu-id="2bd27-461">Most Specific Widening Conversion</span></span>

<span data-ttu-id="2bd27-462">Bestimmen die spezifischste benutzerdefinierte erweiternde Operator für die Konvertierung zwischen zwei Typen erfolgt mithilfe der folgenden Schritte:</span><span class="sxs-lookup"><span data-stu-id="2bd27-462">Determining the most specific user-defined widening conversion operator between two types is accomplished using the following steps:</span></span>

1. <span data-ttu-id="2bd27-463">Zunächst werden alle die Candidate-Konvertierungsoperatoren gesammelt.</span><span class="sxs-lookup"><span data-stu-id="2bd27-463">First, all of the candidate conversion operators are collected.</span></span> <span data-ttu-id="2bd27-464">Die Candidate-Konvertierungsoperatoren sind alle benutzerdefinierten erweiternde Konvertierungsoperatoren des Quelltyps und alle benutzerdefinierten erweiternde Konvertierungsoperatoren in den Zieltyp.</span><span class="sxs-lookup"><span data-stu-id="2bd27-464">The candidate conversion operators are all of the user-defined widening conversion operators in the source type and all of the user-defined widening conversion operators in the target type.</span></span>

2. <span data-ttu-id="2bd27-465">Anschließend werden alle Operatoren für unzulässige Konvertierung aus dem Satz entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="2bd27-465">Then, all non-applicable conversion operators are removed from the set.</span></span> <span data-ttu-id="2bd27-466">Ein Konvertierungsoperator ist auf einem Quell- und Zieltyp anwendbar, wenn systeminterne erweiternde Konvertierungsoperator aus einer Datenquelle in den Operandentyp wird und ein systeminternen erweiternde Konvertierungsoperator aus dem Ergebnis des Operators, in den Zieltyp.</span><span class="sxs-lookup"><span data-stu-id="2bd27-466">A conversion operator is applicable to a source type and target type if there is an intrinsic widening conversion operator from the source type to the operand type and there is an intrinsic widening conversion operator from the result of the operator to the target type.</span></span> <span data-ttu-id="2bd27-467">Wenn es keine entsprechenden Konvertierungsoperatoren sind, besteht keine spezifischste eine erweiternde Konvertierung.</span><span class="sxs-lookup"><span data-stu-id="2bd27-467">If there are no applicable conversion operators, then there is no most specific widening conversion.</span></span>

3. <span data-ttu-id="2bd27-468">Anschließend wird die spezifischste Quelltyp, der die entsprechenden Konvertierungsoperatoren bestimmt:</span><span class="sxs-lookup"><span data-stu-id="2bd27-468">Then, the most specific source type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="2bd27-469">Wenn keines der Konvertierungsoperatoren direkt aus dem Quelltyp konvertieren, ist der Quelltyp einen möglichst spezifischen Quelltyp.</span><span class="sxs-lookup"><span data-stu-id="2bd27-469">If any of the conversion operators convert directly from the source type, then the source type is the most specific source type.</span></span>

   * <span data-ttu-id="2bd27-470">Anderenfalls ist ein möglichst spezifische Quelltyp der am stärksten umfasste Typ in der kombinierten Gruppe von Typen die Konvertierungsoperatoren.</span><span class="sxs-lookup"><span data-stu-id="2bd27-470">Otherwise, the most specific source type is the most encompassed type in the combined set of source types of the conversion operators.</span></span> <span data-ttu-id="2bd27-471">Wenn Nein, die die darin enthaltenen kann Typ gefunden werden, dann gibt es keine spezifischste eine erweiternde Konvertierung.</span><span class="sxs-lookup"><span data-stu-id="2bd27-471">If no most encompassed type can be found, then there is no most specific widening conversion.</span></span>

4. <span data-ttu-id="2bd27-472">Anschließend wird die spezifischste Zieltyp, der die entsprechenden Konvertierungsoperatoren bestimmt:</span><span class="sxs-lookup"><span data-stu-id="2bd27-472">Then, the most specific target type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="2bd27-473">Wenn keines der Konvertierungsoperatoren direkt in den Zieltyp konvertieren, ist der Zieltyp der spezifischste Zieltyp.</span><span class="sxs-lookup"><span data-stu-id="2bd27-473">If any of the conversion operators convert directly to the target type, then the target type is the most specific target type.</span></span>

   * <span data-ttu-id="2bd27-474">Anderenfalls ist die spezifischste Zieltyp der umfassendste Typ in der kombinierten Gruppe von Zieltypen die Konvertierungsoperatoren.</span><span class="sxs-lookup"><span data-stu-id="2bd27-474">Otherwise, the most specific target type is the most encompassing type in the combined set of target types of the conversion operators.</span></span> <span data-ttu-id="2bd27-475">Wenn kein umfassendste Typ gefunden werden kann, besteht keine spezifischste eine erweiternde Konvertierung.</span><span class="sxs-lookup"><span data-stu-id="2bd27-475">If no most encompassing type can be found, then there is no most specific widening conversion.</span></span>

5. <span data-ttu-id="2bd27-476">Klicken Sie dann, wenn genau ein Konvertierungsoperator von einen möglichst spezifischen Quelltyp in einen möglichst spezifischen Zieltyp konvertiert werden soll, ist dies die spezifischste Konvertierungsoperator.</span><span class="sxs-lookup"><span data-stu-id="2bd27-476">Then, if exactly one conversion operator converts from the most specific source type to the most specific target type, then this is the most specific conversion operator.</span></span> <span data-ttu-id="2bd27-477">Wenn mehr als einen solchen Operator vorhanden ist, ist keine spezifischste erweiternde Konvertierung vorhanden.</span><span class="sxs-lookup"><span data-stu-id="2bd27-477">If more than one such operator exists, then there is no most specific widening conversion.</span></span>

### <a name="most-specific-narrowing-conversion"></a><span data-ttu-id="2bd27-478">Spezifischste einschränkende Konvertierung</span><span class="sxs-lookup"><span data-stu-id="2bd27-478">Most Specific Narrowing Conversion</span></span>

<span data-ttu-id="2bd27-479">Bestimmen die spezifischste benutzerdefinierte einschränkende Operator für die Konvertierung zwischen zwei Typen erfolgt mithilfe der folgenden Schritte:</span><span class="sxs-lookup"><span data-stu-id="2bd27-479">Determining the most specific user-defined narrowing conversion operator between two types is accomplished using the following steps:</span></span>

1. <span data-ttu-id="2bd27-480">Zunächst werden alle die Candidate-Konvertierungsoperatoren gesammelt.</span><span class="sxs-lookup"><span data-stu-id="2bd27-480">First, all of the candidate conversion operators are collected.</span></span> <span data-ttu-id="2bd27-481">Die Candidate-Konvertierungsoperatoren sind alle Operatoren benutzerdefinierte Konvertierung in den Quelltyp und alle Operatoren benutzerdefinierte Konvertierung in den Zieltyp.</span><span class="sxs-lookup"><span data-stu-id="2bd27-481">The candidate conversion operators are all of the user-defined conversion operators in the source type and all of the user-defined conversion operators in the target type.</span></span>

2. <span data-ttu-id="2bd27-482">Anschließend werden alle Operatoren für unzulässige Konvertierung aus dem Satz entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="2bd27-482">Then, all non-applicable conversion operators are removed from the set.</span></span> <span data-ttu-id="2bd27-483">Ein Konvertierungsoperator ist auf einem Quell- und Zieltyp anwendbar, wenn Konvertierungsoperator aus einer Datenquelle in den Operandentyp wird und ein Operator Konvertierung aus dem Ergebnis des Operators, in den Zieltyp.</span><span class="sxs-lookup"><span data-stu-id="2bd27-483">A conversion operator is applicable to a source type and target type if there is an intrinsic conversion operator from the source type to the operand type and there is an intrinsic conversion operator from the result of the operator to the target type.</span></span> <span data-ttu-id="2bd27-484">Wenn es keine entsprechenden Konvertierungsoperatoren sind, besteht keine spezifischste einschränkende Konvertierung.</span><span class="sxs-lookup"><span data-stu-id="2bd27-484">If there are no applicable conversion operators, then there is no most specific narrowing conversion.</span></span>

3. <span data-ttu-id="2bd27-485">Anschließend wird die spezifischste Quelltyp, der die entsprechenden Konvertierungsoperatoren bestimmt:</span><span class="sxs-lookup"><span data-stu-id="2bd27-485">Then, the most specific source type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="2bd27-486">Wenn keines der Konvertierungsoperatoren direkt aus dem Quelltyp konvertieren, ist der Quelltyp einen möglichst spezifischen Quelltyp.</span><span class="sxs-lookup"><span data-stu-id="2bd27-486">If any of the conversion operators convert directly from the source type, then the source type is the most specific source type.</span></span>

   * <span data-ttu-id="2bd27-487">Andernfalls, wenn keines der Operatoren für die Konvertierung von Datentypen, die den Quelltyp umfassen konvertieren, ist ein möglichst spezifische Quelltyp der am stärksten umfasste Typ in der kombinierten Gruppe von Quelltypen diese Konvertierungsoperatoren.</span><span class="sxs-lookup"><span data-stu-id="2bd27-487">Otherwise, if any of the conversion operators convert from types that encompass the source type, then the most specific source type is the most encompassed type in the combined set of source types of those conversion operators.</span></span> <span data-ttu-id="2bd27-488">Wenn Nein, die die darin enthaltenen kann Typ gefunden werden, und klicken Sie dann keine spezifischste einschränkende Konvertierung besteht.</span><span class="sxs-lookup"><span data-stu-id="2bd27-488">If no most encompassed type can be found, then there is no most specific narrowing conversion.</span></span>

   * <span data-ttu-id="2bd27-489">Anderenfalls ist ein möglichst spezifische Quelltyp der umfassendste Typ in der kombinierten Gruppe von Typen die Konvertierungsoperatoren.</span><span class="sxs-lookup"><span data-stu-id="2bd27-489">Otherwise, the most specific source type is the most encompassing type in the combined set of source types of the conversion operators.</span></span> <span data-ttu-id="2bd27-490">Wenn kein umfassendste Typ gefunden werden kann, besteht keine spezifischste einschränkende Konvertierung.</span><span class="sxs-lookup"><span data-stu-id="2bd27-490">If no most encompassing type can be found, then there is no most specific narrowing conversion.</span></span>

4. <span data-ttu-id="2bd27-491">Anschließend wird die spezifischste Zieltyp, der die entsprechenden Konvertierungsoperatoren bestimmt:</span><span class="sxs-lookup"><span data-stu-id="2bd27-491">Then, the most specific target type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="2bd27-492">Wenn keines der Konvertierungsoperatoren direkt in den Zieltyp konvertieren, ist der Zieltyp der spezifischste Zieltyp.</span><span class="sxs-lookup"><span data-stu-id="2bd27-492">If any of the conversion operators convert directly to the target type, then the target type is the most specific target type.</span></span>

   * <span data-ttu-id="2bd27-493">Andernfalls, wenn keines der Operatoren für die Konvertierung in Typen, die durch den Typ des Datenbankprotokolls enthalten sind konvertieren, ist ein möglichst spezifische Zieltyp der umfassendste Typ in der kombinierten Gruppe von Quelltypen diese Konvertierungsoperatoren.</span><span class="sxs-lookup"><span data-stu-id="2bd27-493">Otherwise, if any of the conversion operators convert to types that are encompassed by the target type, then the most specific target type is the most encompassing type in the combined set of source types of those conversion operators.</span></span> <span data-ttu-id="2bd27-494">Wenn kein umfassendste Typ gefunden werden kann, besteht keine spezifischste einschränkende Konvertierung.</span><span class="sxs-lookup"><span data-stu-id="2bd27-494">If no most encompassing type can be found, then there is no most specific narrowing conversion.</span></span>

   * <span data-ttu-id="2bd27-495">Anderenfalls ist die spezifischste Zieltyp der am stärksten umfasste Typ in der kombinierten Gruppe von Zieltypen die Konvertierungsoperatoren.</span><span class="sxs-lookup"><span data-stu-id="2bd27-495">Otherwise, the most specific target type is the most encompassed type in the combined set of target types of the conversion operators.</span></span> <span data-ttu-id="2bd27-496">Wenn Nein, die die darin enthaltenen kann Typ gefunden werden, und klicken Sie dann keine spezifischste einschränkende Konvertierung besteht.</span><span class="sxs-lookup"><span data-stu-id="2bd27-496">If no most encompassed type can be found, then there is no most specific narrowing conversion.</span></span>

5. <span data-ttu-id="2bd27-497">Klicken Sie dann, wenn genau ein Konvertierungsoperator von einen möglichst spezifischen Quelltyp in einen möglichst spezifischen Zieltyp konvertiert werden soll, ist dies die spezifischste Konvertierungsoperator.</span><span class="sxs-lookup"><span data-stu-id="2bd27-497">Then, if exactly one conversion operator converts from the most specific source type to the most specific target type, then this is the most specific conversion operator.</span></span> <span data-ttu-id="2bd27-498">Wenn mehr als einen solchen Operator vorhanden ist, besteht keine spezifischste einschränkende Konvertierung.</span><span class="sxs-lookup"><span data-stu-id="2bd27-498">If more than one such operator exists, then there is no most specific narrowing conversion.</span></span>

## <a name="native-conversions"></a><span data-ttu-id="2bd27-499">Systemeigene Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="2bd27-499">Native Conversions</span></span>

<span data-ttu-id="2bd27-500">Einige der Konvertierungen werden als klassifiziert *systemeigene Konvertierungen* , da sie standardmäßig von .NET Framework unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="2bd27-500">Several of the conversions are classified as *native conversions* because they are supported natively by the .NET Framework.</span></span> <span data-ttu-id="2bd27-501">Diese Konvertierungen, die mithilfe des optimiert werden können, sind die `DirectCast` und `TryCast` Konvertierungsoperatoren sowie andere speziellen Verhaltensweisen.</span><span class="sxs-lookup"><span data-stu-id="2bd27-501">These conversions are ones that can be optimized through the use of the `DirectCast` and `TryCast` conversion operators, as well as other special behaviors.</span></span> <span data-ttu-id="2bd27-502">Die Konvertierungen, die als systemeigene Konvertierungen sind klassifiziert: Identität Konvertierungen, standardkonvertierungen, Verweis-, arraykonvertierungen, typkonvertierungen Wert und typkonvertierungen-Parameter.</span><span class="sxs-lookup"><span data-stu-id="2bd27-502">The conversions classified as native conversions are: identity conversions, default conversions, reference conversions, array conversions, value type conversions, and type parameter conversions.</span></span>

## <a name="dominant-type"></a><span data-ttu-id="2bd27-503">Der bestimmende Typ</span><span class="sxs-lookup"><span data-stu-id="2bd27-503">Dominant Type</span></span>

<span data-ttu-id="2bd27-504">Wenn einen Satz von Typen, es ist häufig erforderlich in Situationen wie z. B. Typrückschluss, um zu bestimmen, die *bestimmende Typ* des Satzes.</span><span class="sxs-lookup"><span data-stu-id="2bd27-504">Given a set of types, it is often necessary in situations such as type inference to determine the *dominant type* of the set.</span></span> <span data-ttu-id="2bd27-505">Der bestimmende Typ, der einen Satz von Typen wird durch die zuvor entfernt, denen einen oder mehrere Typen keine implizite Konvertierung in Typen bestimmt.</span><span class="sxs-lookup"><span data-stu-id="2bd27-505">The dominant type of a set of types is determined by first removing any types that one or more other types do not have an implicit conversion to.</span></span> <span data-ttu-id="2bd27-506">Es sind keine Typen links an diesem Punkt ein, ist es kein dominanter Typ.</span><span class="sxs-lookup"><span data-stu-id="2bd27-506">If there are no types left at this point, there is no dominant type.</span></span> <span data-ttu-id="2bd27-507">Der bestimmende Typ ist, und klicken Sie dann die meisten der verbleibenden Typen einschließt.</span><span class="sxs-lookup"><span data-stu-id="2bd27-507">The dominant type is then the most encompassed of the remaining types.</span></span> <span data-ttu-id="2bd27-508">Liegt mehr als ein eingeben, wird am häufigsten einschließt, dann gibt es kein dominanter Typ.</span><span class="sxs-lookup"><span data-stu-id="2bd27-508">If there is more than one type that is most encompassed, then there is no dominant type.</span></span>
