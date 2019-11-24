---
ms.openlocfilehash: 7aef52145a71bff1d489772e81eb786a9dbd23d1
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306198"
---
# <a name="conversions"></a>Konvertierungen

Bei der Konvertierung wird ein Wert von einem Typ in einen anderen geändert. Beispielsweise kann ein Wert vom Typ "`Integer`" in einen Wert vom Typ "`Double`" konvertiert werden, oder es kann ein Wert vom Typ "`Derived`" in einen Wert vom Typ "`Base`" konvertiert werden. dabei wird davon ausgegangen, dass `Base` und `Derived` beide Klassen sind und `Derived` von `Base`erbt. Bei Konvertierungen ist es möglicherweise nicht erforderlich, dass sich der Wert selbst ändert (wie im letzteren Beispiel), oder Sie erfordern möglicherweise bedeutende Änderungen in der Wert Darstellung (wie im vorherigen Beispiel).

Konvertierungen können entweder erweitert oder einschränkend sein. Eine *erweiternde Konvertierung* ist eine Konvertierung von einem Typ in einen anderen Typ, dessen Wert Domäne mindestens so groß ist, wenn er nicht größer ist als die Wert Domäne des ursprünglichen Typs. Erweiternde Konvertierungen sollten nie fehlschlagen. Eine einschränkende *Konvertierung* ist eine Konvertierung von einem Typ in einen anderen Typ, dessen Wert Domäne entweder kleiner ist als die Wert Domäne des ursprünglichen Typs oder ausreichend ohne Beziehung, die bei der Konvertierung erforderlich ist (z. b. bei der Konvertierung von `Integer` in `String`). Einschränkende Konvertierungen, die einen Informationsverlust verursachen können, können fehlschlagen.

Die Identitäts Konvertierung (d. h. eine Konvertierung von einem Typ in sich selbst) und die Standardwert Konvertierung (d. h. eine Konvertierung von `Nothing`) werden für alle Typen definiert.

## <a name="implicit-and-explicit-conversions"></a>Implizite und explizite Konvertierungen

Konvertierungen können entweder *implizit* oder *explizit*sein. Implizite Konvertierungen treten ohne besondere Syntax auf. Im folgenden finden Sie ein Beispiel für die implizite Konvertierung eines `Integer` Werts in einen `Long` Wert:

```vb
Module Test
    Sub Main()
        Dim intValue As Integer = 123
        Dim longValue As Long = intValue

        Console.WriteLine(intValue & " = " & longValue)
    End Sub
End Module
```

Explizite Konvertierungen erfordern hingegen Umwandlungs Operatoren. Der Versuch, eine explizite Konvertierung für einen Wert ohne Cast Operator durchzuführen, verursacht einen Kompilierzeitfehler. Im folgenden Beispiel wird eine explizite Konvertierung verwendet, um einen `Long` Wert in einen `Integer`-Wert zu konvertieren.

```vb
Module Test
    Sub Main()
        Dim longValue As Long = 134
        Dim intValue As Integer = CInt(longValue)

        Console.WriteLine(longValue & " = " & intValue)
    End Sub
End Module
```

Der Satz impliziter Konvertierungen hängt von der Kompilierungs Umgebung und der `Option Strict`-Anweisung ab. Wenn eine strikte Semantik verwendet wird, können nur erweiternde Konvertierungen implizit auftreten. Wenn eine einschränkend sein Semantik verwendet wird, können alle Erweiterungs-und einschränkenden Konvertierungen (d. h. alle Konvertierungen) implizit auftreten.

## <a name="boolean-conversions"></a>Boolesche Konvertierungen

Obwohl `Boolean` kein numerischer Typ ist, verfügt er über einschränkende Konvertierungen in und aus den numerischen Typen, als ob es sich um einen enumerierten Typ handelt. Der Literal`True` konvertiert in den Literal`255` für `Byte`, `65535` für `UShort`, `4294967295` `UInteger``18446744073709551615`, `ULong`für `-1` und den Ausdrucks `SByte`für `Short`, `Integer`, `Long`, `Decimal`, `Single`, `Double`und. Der Literal`False` konvertiert in den Literal`0`. Ein numerischer Wert von 0 (null) wird in die Literale `False` Alle anderen numerischen Werte werden in die Literale `True`konvertiert.

Es gibt eine einschränkende Konvertierung von einem booleschen Wert in eine Zeichenfolge, die entweder in `System.Boolean.TrueString` oder `System.Boolean.FalseString`konvertiert wird. Es gibt auch eine einschränkende Konvertierung von `String` in `Boolean`: Wenn die Zeichenfolge gleich `TrueString` oder `FalseString` ist (in der aktuellen Kultur, ohne Berücksichtigung der Groß-/Kleinschreibung), wird der entsprechende Wert verwendet. Andernfalls wird versucht, die Zeichenfolge als numerischen Typ (in Hexadezimal oder oktal, wenn möglich, andernfalls als float) zu analysieren und die obigen Regeln zu verwenden. Andernfalls wird `System.InvalidCastException`ausgelöst.

## <a name="numeric-conversions"></a>Numerische Konvertierungen

Numerische Konvertierungen zwischen den Typen `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single` und `Double`sowie alle aufgelisteten Typen. Beim Konvertieren werden Enumerationstypen so behandelt, als wären Sie Ihre zugrunde liegenden Typen. Beim Umrechnen in einen enumerierten Typ muss der Quellwert nicht mit dem Satz von Werten übereinstimmen, die im enumerierten Typ definiert sind. Beispiel:

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

Numerische Konvertierungen werden zur Laufzeit wie folgt verarbeitet:

* Bei einer Konvertierung eines numerischen Typs in einen umfassenderen numerischen Typ wird der Wert einfach in den umfassenderen Typ konvertiert. Konvertierungen von `UInteger`, `Integer`, `ULong`, `Long`oder `Decimal` zu `Single` oder `Double` werden auf den nächstgelegenen `Single` oder `Double` Wert gerundet. Diese Konvertierung kann zwar zu einem Genauigkeits Verlust führen, es wird jedoch nie ein Größen Verlust verursacht.

* Für eine Konvertierung eines ganzzahligen Typs in einen anderen ganzzahligen Typ oder von `Single`, `Double`oder `Decimal` in einen ganzzahligen Typ hängt das Ergebnis davon ab, ob die Überprüfung der ganzzahligen Überlauf erfolgt:

  *Wenn der ganzzahlige Überlauf geprüft wird:*

  * Wenn die Quelle ein ganzzahliger Typ ist, wird die Konvertierung erfolgreich ausgeführt, wenn das Quell Argument innerhalb des Bereichs des Zieltyps liegt. Die Konvertierung löst eine `System.OverflowException` Ausnahme aus, wenn das Quell Argument außerhalb des Bereichs des Zieltyps liegt.

  * Wenn die Quelle `Single`, `Double`oder `Decimal`ist, wird der Quellwert auf den nächstgelegenen ganzzahligen Wert aufgerundet oder auf ihn herab gerundet, und dieser ganzzahlige Wert wird das Ergebnis der Konvertierung. Wenn der Quellwert gleich nah bei zwei ganzzahligen Werten ist, wird der Wert auf den Wert gerundet, der über eine gerade Zahl in der am wenigsten signifikanten Ziffern Position verfügt. Wenn sich der resultierende ganzzahlige Wert außerhalb des Bereichs des Zieltyps befindet, wird eine `System.OverflowException` Ausnahme ausgelöst.

  *Wenn der ganzzahlige Überlauf nicht geprüft wird:*

  * Wenn die Quelle ein ganzzahliger Typ ist, wird die Konvertierung immer erfolgreich ausgeführt und besteht lediglich darin, die signifikantesten Bits des Quell Werts zu verwerfen.

  * Wenn die Quelle `Single`, `Double`oder `Decimal`ist, ist die Konvertierung immer erfolgreich und besteht lediglich aus der Rundung des Quell Werts auf den nächstgelegenen ganzzahligen Wert. Wenn der Quellwert gleich nah bei zwei ganzzahligen Werten ist, wird der Wert immer auf den Wert gerundet, der über eine gerade Zahl in der am wenigsten wichtigen Ziffern Position verfügt.

* Bei einer Konvertierung von `Double` in `Single`wird der `Double` Wert auf den nächsten `Single` Wert gerundet. Wenn der `Double` Wert zu klein ist, um als `Single`darzustellen, wird das Ergebnis positiv 0 (null) oder negativ 0 (null). Wenn der `Double` Wert zu groß ist, um als `Single`darzustellen, wird das Ergebnis positiv unendlich oder minus unendlich. Wenn der `Double` Wert `NaN`ist, wird das Ergebnis ebenfalls `NaN`.

* Bei einer Konvertierung von `Single` oder `Double` zu `Decimal`wird der Quellwert in `Decimal` Darstellung konvertiert und bei Bedarf auf die nächstgelegene Zahl nach dem 28. Dezimaltrennzeichen gerundet. Wenn der Quellwert zu klein ist, um als `Decimal`darzustellen, wird das Ergebnis 0 (null). Wenn der Quellwert `NaN`, unendlich oder zu groß ist, um als `Decimal`darzustellen, wird eine `System.OverflowException` Ausnahme ausgelöst.

* Bei einer Konvertierung von `Double` in `Single`wird der `Double` Wert auf den nächsten `Single` Wert gerundet. Wenn der `Double` Wert zu klein ist, um als `Single`darzustellen, wird das Ergebnis positiv 0 (null) oder negativ 0 (null). Wenn der `Double` Wert zu groß ist, um als `Single`darzustellen, wird das Ergebnis positiv unendlich oder minus unendlich. Wenn der `Double` Wert `NaN`ist, wird das Ergebnis ebenfalls `NaN`.

## <a name="reference-conversions"></a>Verweiskonvertierungen

Verweis Typen können in einen Basistyp konvertiert werden und umgekehrt. Konvertierungen eines Basistyps in einen stärker abgeleiteten Typ werden nur zur Laufzeit erfolgreich ausgeführt, wenn der zu konvertierende Wert ein NULL-Wert, der abgeleitete Typ selbst oder ein stärker abgeleiteter Typ ist.

Klassen-und Schnittstellentypen können in und aus einem beliebigen Schnittstellentyp umgewandelt werden. Konvertierungen zwischen einem Typ und einem Schnittstellentyp sind nur zur Laufzeit erfolgreich, wenn die tatsächlich beteiligten Typen eine Vererbungs-oder Implementierungs Beziehung aufweisen. Da ein Schnittstellentyp immer eine Instanz eines Typs enthält, der von `Object`abgeleitet ist, kann ein Schnittstellentyp auch immer in und aus `Object`umgewandelt werden.

__Nebenbei.__ Es ist kein Fehler, eine `NotInheritable` Klassen in und von Schnittstellen zu konvertieren, die nicht implementiert werden, da Klassen, die com-Klassen darstellen, möglicherweise Schnittstellen Implementierungen aufweisen, die bis zur Laufzeit nicht bekannt sind. 

Wenn eine Verweis Konvertierung zur Laufzeit fehlschlägt, wird eine `System.InvalidCastException` Ausnahme ausgelöst.

### <a name="reference-variance-conversions"></a>Verweis Varianz Konvertierungen

Generische Schnittstellen oder Delegaten können über Variante Typparameter verfügen, die Konvertierungen zwischen kompatiblen Varianten des Typs ermöglichen. Daher ist zur Laufzeit eine Konvertierung von einem Klassentyp oder einem Schnittstellentyp in einen Schnittstellentyp möglich, der Variant mit einem Schnittstellentyp kompatibel ist, der von ihm geerbt oder implementiert wird. Ebenso können Delegattypen in und aus Variant-kompatiblen Delegattypen umgewandelt werden. Beispielsweise der Delegattyp.

```vb
Delegate Function F(Of In A, Out R)(a As A) As R
```

ermöglicht eine Konvertierung von `F(Of Object, Integer)` in `F(Of String, Integer)`. Das heißt, dass ein Delegat, `F` `Object` verwendet, möglicherweise sicher als Delegat verwendet wird `F` der `String`benötigt. Wenn der Delegat aufgerufen wird, erwartet die Ziel Methode ein Objekt, und eine Zeichenfolge ist ein Objekt.

Ein generischer Delegat-oder Schnittstellentyp `S(Of S1,...,Sn)` als *Variant-kompatibel* mit einer generischen Schnittstelle oder einem Delegattyp bezeichnet, `T(Of T1,...,Tn)` if:

* `S` und `T` werden beide aus dem gleichen generischen Typ `U(Of U1,...,Un)`konstruiert.

* Für jeden Typparameter `Ux`:

  * Wenn der Typparameter ohne Varianz deklariert wurde, müssen `Sx` und `Tx` denselben Typ aufweisen.

  * Wenn der Typparameter deklariert wurde `In` dann muss eine erweiternde Identitäts-, Standard-, Verweis-, Array-oder Typparameter Konvertierung von `Sx` in `Tx`erfolgen.

  * Wenn der Typparameter deklariert wurde `Out` dann muss eine erweiternde Identitäts-, Standard-, Verweis-, Array-oder Typparameter Konvertierung von `Tx` in `Sx`erfolgen.

Beim Konvertieren von einer Klasse in eine generische Schnittstelle mit Varianten Typparametern ist die Konvertierung mehrdeutig, wenn keine nicht-Variante Konvertierung vorhanden ist, wenn die Klasse mehr als eine Variant-kompatible Schnittstelle implementiert. Beispiel:

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

### <a name="anonymous-delegate-conversions"></a>Anonyme delegatkonvertierungen

Wenn ein Ausdruck, der als Lambda-Methode klassifiziert ist, als Wert in einem Kontext neu klassifiziert wird, in dem kein Zieltyp (z. b. `Dim x = Function(a As Integer, b As Integer) a + b`) oder der Zieltyp kein Delegattyp ist, ist der Typ des resultierenden Ausdrucks ein anonymer Delegattyp, der der Signatur der Lambda-Methode entspricht. Dieser anonyme Delegattyp hat eine Konvertierung in einen beliebigen kompatiblen Delegattyp: ein kompatibler Delegattyp ist ein beliebiger Delegattyp, der mithilfe eines delegaterstellungs-Ausdrucks mit der `Invoke`-Methode des anonymen Delegattyps als Parameter erstellt werden kann Beispiel:

```vb
' Anonymous delegate type similar to Func(Of Object, Object, Object)
Dim x = Function(x, y) x + y

' OK because delegate type is compatible
Dim y As Func(Of Integer, Integer, Integer) = x
```

Beachten Sie, dass die Typen `System.Delegate` und `System.MulticastDelegate` nicht selbst als Delegattypen angesehen werden (auch wenn alle Delegattypen von Ihnen erben). Beachten Sie außerdem, dass die Konvertierung eines anonymen Delegattyps in einen kompatiblen Delegattyp keine Verweis Konvertierung ist.

## <a name="array-conversions"></a>Arraykonvertierungen

Neben den Konvertierungen, die für Arrays definiert sind, weil es sich um Verweis Typen handelt, gibt es mehrere spezielle Konvertierungen für Arrays.

Bei zwei Typen `A` und `B`, wenn beide Verweis Typen oder Typparameter sind, die keine Werttypen sind, und wenn `A` eine Verweis-, Array-oder Typparameter Konvertierung in `B`hat, ist eine Konvertierung von einem Array vom Typ `A` in ein Array vom Typ `B` mit dem gleichen Rang vorhanden. Diese Beziehung wird als *Array Kovarianz*bezeichnet. Array Kovarianz bedeutet insbesondere, dass ein Element eines Arrays, dessen Elementtyp `B` ist, tatsächlich ein Element eines Arrays sein kann, dessen Elementtyp `A`ist, vorausgesetzt, dass sowohl `A` als auch `B` Verweis Typen sind und dass `B` eine Verweis Konvertierung oder eine Array Konvertierung in `A`aufweist. Im folgenden Beispiel bewirkt der zweite Aufruf von `F` bewirkt, dass eine `System.ArrayTypeMismatchException` Ausnahme ausgelöst wird, da der tatsächliche Elementtyp von `b` `String`und nicht `Object`ist:

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

Aufgrund von Array Kovarianz enthalten Zuweisungen zu Elementen von Verweistyp Arrays eine Lauf Zeit Überprüfung, mit der sichergestellt wird, dass der Wert, der dem Array Element zugewiesen wird, tatsächlich einen zulässigen Typ hat.

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

In diesem Beispiel enthält die Zuweisung zum `array(i)` in der-Methode `Fill` implizit eine Lauf Zeit Überprüfung, mit der sichergestellt wird, dass das Objekt, auf das die Variable verweist `value` entweder `Nothing` oder eine Instanz eines Typs ist, der mit dem tatsächlichen Elementtyp von Array-`array`kompatibel ist. In der-Methode `Main``Fill` die ersten beiden Aufrufe der-Methode erfolgreich, aber der dritte Aufruf bewirkt, dass eine `System.ArrayTypeMismatchException` Ausnahme ausgelöst wird, wenn die erste Zuweisung zum `array(i)`ausgeführt wird. Die Ausnahme tritt auf, weil ein `Integer` nicht in einem `String` Array gespeichert werden kann.

Wenn einer der Array Elementtypen ein Typparameter ist, dessen Typ zur Laufzeit ein Werttyp ist, wird eine `System.InvalidCastException` Ausnahme ausgelöst. Beispiel:

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

Konvertierungen sind auch zwischen einem Array eines enumerierten Typs und einem Array des zugrunde liegenden Typs des Enumerationstyps oder eines Arrays eines anderen Enumerationstyps mit demselben zugrunde liegenden Typ vorhanden, vorausgesetzt, die Arrays haben denselben Rang.

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

In diesem Beispiel wird ein Array von `Color` in ein und aus einem Array von `Byte`, dem zugrunde liegenden Typ `Color`, konvertiert. Die Konvertierung in ein Array von `Integer`ist jedoch ein Fehler, da `Integer` nicht der zugrunde liegende Typ von `Color`ist.

Ein Rang 1-Array vom Typ `A()` auch eine Array Konvertierung in die Auflistungs Schnittstellentypen `IList(Of B)`, `IReadOnlyList(Of B)`, `ICollection(Of B)`, `IReadOnlyCollection(Of B)` und `IEnumerable(Of B)`, die in `System.Collections.Generic`gefunden werden, solange eine der folgenden Punkte zutrifft:

- `A` und `B` sind Verweis Typen oder Typparameter, die keine Werttypen sind. und `A` über eine erweiternde Verweis-, Array-oder Typparameter Konvertierung in `B`; noch
- `A` und `B` sind beide Enumerationstypen desselben zugrunde liegenden Typs. noch
- eine `A` und `B` ist ein Enumerationstyp, der andere ist der zugrunde liegende Typ.

Ein Array vom Typ A mit einem beliebigen Rang verfügt auch über eine Array Konvertierung in die nicht generischen Auflistungs Schnittstellentypen `IList`, `ICollection` und `IEnumerable`, die in `System.Collections`gefunden werden.

Es ist möglich, die resultierenden Schnittstellen mithilfe von `For Each`zu durchlaufen oder die `GetEnumerator` Methoden direkt aufzurufen. Wenn Rang 1 Arrays generische oder nicht generische Formen von `IList` oder `ICollection`konvertieren, ist es auch möglich, Elemente nach Index zu erhalten. Im Fall von Rang 1-Arrays, die in generische oder nicht generische Formen von `IList`konvertiert werden, ist es auch möglich, Elemente nach Index festzulegen. Dies unterliegt den gleichen Kovarianz Überprüfungen des Lauf Zeit Arrays, wie oben beschrieben. Das Verhalten aller anderen Schnittstellen Methoden ist durch die VB-Sprachspezifikation nicht definiert. Es liegt an der zugrunde liegenden Laufzeit.

## <a name="value-type-conversions"></a>Werttyp Konvertierungen

Ein Werttyp Wert kann in einen seiner Basis Verweis Typen oder einen Schnittstellentyp konvertiert werden, der durch einen als *Boxing*bezeichneten Prozess implementiert wird. Wenn ein Wert für einen Werttyp in einen Kasten konvertiert wird, wird der Wert von der Position kopiert, an der er sich auf den .NET Framework Heap befindet. Ein Verweis auf diesen Speicherort auf dem Heap wird dann zurückgegeben und kann in einer Verweistyp Variablen gespeichert werden. Dieser Verweis wird *auch als geachtelte Instanz des* Werttyps bezeichnet. Die geachtelte Instanz weist die gleiche Semantik wie ein Verweistyp anstelle eines Werttyps auf.

Boxed-Werttypen können durch einen Prozess namens *Unboxing*zurück in ihren ursprünglichen Werttyp konvertiert werden. Wenn ein eingegestellter Werttyp Unboxing ist, wird der Wert aus dem Heap in einen Variablen Speicherort kopiert. Ab diesem Zeitpunkt wird der Wert so verhält, als ob es sich um einen Werttyp handelt. Beim Unboxing eines Werttyps muss der Wert ein NULL-Wert oder eine Instanz des Werttyps sein. Andernfalls wird eine `System.InvalidCastException` Ausnahme ausgelöst. Wenn der Wert eine Instanz eines enumerierten Typs ist, kann dieser Wert auch auf den zugrunde liegenden Typ des enumerierten Typs oder einen anderen enumerierten Typ, der denselben zugrunde liegenden Typ aufweist, entpackt werden. Ein NULL-Wert wird so behandelt, als wäre er der Literale `Nothing`.

Zur Unterstützung von Typen, die NULL-Werte zulassen, wird der Werttyp `System.Nullable(Of T)` bei Boxing und Unboxing besonders behandelt. Das Boxing eines Werts vom Typ `Nullable(Of T)` führt zu einem geboxten Wert des Typs `T`, wenn die `HasValue`-Eigenschaft des Werts `True` ist oder der Wert `Nothing`, wenn die `HasValue`-Eigenschaft des Werts `False`ist. Das Unboxing eines Werts vom Typ `T`, um `Nullable(Of T)` Ergebnisse in einer Instanz von `Nullable(Of T)`, deren `Value` Eigenschaft der eingepackte Wert und dessen `HasValue`-Eigenschaft `True`ist. Der Wert `Nothing` der für alle `T` `Nullable(Of T)` entpackt werden kann, und führt zu einem Wert, dessen `HasValue` Eigenschaft `False`ist. Da sich geschachtelt-Werttypen wie Verweis Typen Verhalten, ist es möglich, mehrere Verweise auf denselben Wert zu erstellen. Bei den primitiven Typen und Enumerationstypen ist dies unerheblich, da Instanzen dieser Typen *unveränderlich*sind. Das heißt, es ist nicht möglich, eine eingepackte Instanz dieser Typen zu ändern, sodass es nicht möglich ist, die Tatsache zu beobachten, dass mehrere Verweise auf denselben Wert vorhanden sind.

Strukturen hingegen können änderbar sein, wenn auf ihre Instanzvariablen zugegriffen werden kann oder wenn ihre Methoden oder Eigenschaften ihre Instanzvariablen ändern. Wenn ein Verweis auf eine geachtelte Struktur verwendet wird, um die Struktur zu ändern, werden alle Verweise auf die geachtelte Struktur die Änderung sehen. Da dieses Ergebnis möglicherweise unerwartet ist, wird ein als `Object` typisierter Wert automatisch auf dem Heap geklont, anstatt nur die zugehörigen Verweise kopieren zu müssen. Beispiel:

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

Die Ausgabe des Programms lautet wie folgt:

```console
Values: 0, 123
Refs: 123, 123
```

Die Zuweisung zum-Feld der lokalen Variablen `val2` wirkt sich nicht auf das-Feld der lokalen Variablen `val1` aus, da bei der Zuweisung des geboxten `Struct1` `val2`eine Kopie des Werts erstellt wurde. Im Gegensatz dazu wirkt sich der Zuweisungs `ref2.Value = 123` auf das Objekt aus, das sowohl `ref1` als auch `ref2` verweist.

__Nebenbei.__ Das Kopieren der Struktur wird nicht für als `System.ValueType` typisierte als durchgeführt, da es nicht möglich ist, die Bindung von `System.ValueType`zu beenden.

Es gibt eine Ausnahme von der Regel, bei der bei der Zuweisung geschachtelt-Werttypen kopiert werden. Wenn ein geschachtelter Werttyp Verweis in einem anderen Typ gespeichert wird, wird der innere Verweis nicht kopiert. Beispiel:

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

Die Ausgabe des Programms lautet wie folgt:

```console
Values: 123, 123
```

Dies liegt daran, dass der innere geschachtelte Wert beim Kopieren des Werts nicht kopiert wird. Daher verfügen sowohl `val1.Value` als auch `val2.Value` über einen Verweis auf denselben geschachtelt-Werttyp.

__Nebenbei.__ Die Tatsache, dass die inneren geschachtelten Werttypen nicht kopiert werden, ist eine Einschränkung des .net-Typsystems, um sicherzustellen, dass alle inneren geschachtelten Werttypen kopiert wurden, wenn ein Wert des Typs `Object` kopiert wurde.

Wie bereits beschrieben, können geschachtelte Werttypen nur in ihren ursprünglichen Typ entpackt werden. Gekapselte primitive Typen werden jedoch besonders behandelt, wenn Sie als `Object`eingegeben werden. Sie können in beliebige andere primitive Typen konvertiert werden, in die Sie konvertiert werden. Beispiel:

```vb
Module Test
    Sub Main()
        Dim o As Object = 5
        Dim b As Byte = CByte(o)  ' Legal
        Console.WriteLine(b) ' Prints 5
    End Sub
End Module
```

In der Regel konnte der eingepackte `Integer` Wert `5` nicht in eine `Byte` Variable entpackt werden. Da `Integer` und `Byte` primitive Typen sind und über eine Konvertierung verfügen, ist die Konvertierung zulässig.

Es ist wichtig zu beachten, dass die typumrechnung in eine Schnittstelle von einem generischen Argument abweicht, das auf eine Schnittstelle beschränkt ist. Beim Zugriff auf Schnittstellenmember bei einem eingeschränkten Typparameter (oder beim Aufrufen von Methoden auf `Object`) tritt kein Boxing auf, wie dies beim Konvertieren eines Werttyps in eine Schnittstelle und beim Zugriff auf einen Schnittstellenmember der Fall ist. Nehmen wir beispielsweise an, dass eine Schnittstelle `ICounter` eine-Methode enthält `Increment` die zum Ändern eines Werts verwendet werden kann. Wenn `ICounter` als Einschränkung verwendet wird, wird die Implementierung der `Increment`-Methode mit einem Verweis auf die Variable aufgerufen, für die `Increment` aufgerufen wurde, und nicht mit einer geboxten Kopie:

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

Der erste `Increment`-Aufrufe ändert den Wert in der Variablen `x`. Dies entspricht nicht dem zweiten `Increment`-Aufrufwert, der den Wert in einer geachtelten Kopie von `x`ändert. Folglich lautet die Ausgabe des Programms wie folgt:

```console
0
1
1
```

### <a name="nullable-value-type-conversions"></a>Typkonvertierungen, die NULL zulassen

Ein Werttyp `T` kann in die und von der Werte zulässt-Version des Typs konvertieren, `T?`. Bei der Konvertierung von `T?` in `T` wird eine `System.InvalidOperationException` Ausnahme ausgelöst, wenn der zu konvertierende Wert `Nothing`ist. Außerdem wird `T?` eine Konvertierung in einen Typ `S`, wenn `T` eine systeminterne Konvertierung in `S`hat. Wenn `S` ein Werttyp ist, sind die folgenden systeminternen Konvertierungen zwischen `T?` und `S?`vorhanden:

* Eine Konvertierung der gleichen Klassifizierung (Einschränkung oder Erweiterung) von `T?` in `S?`.

* Eine Konvertierung der gleichen Klassifizierung (Einschränkung oder Erweiterung) von `T` in `S?`.

* Eine einschränkende Konvertierung von `S?` in `T`.

Beispielsweise ist eine systeminterne erweiternde Konvertierung von `Integer?` zu `Long?` vorhanden, da eine systeminterne erweiternde Konvertierung von `Integer` zum `Long`besteht:

```vb
Dim i As Integer? = 10
Dim l As Long? = i
```

Wenn beim Umrechnen von `T?` in `S?`der Wert von `T?` `Nothing`ist, wird der Wert `S?` `Nothing`. Wenn der Wert `T?` oder `S?` `Nothing`ist, wird beim Umrechnen von `S?` in `T` oder `T?` in `S`eine `System.InvalidCastException` Ausnahme ausgelöst.

Aufgrund des Verhaltens des zugrunde liegenden Typs `System.Nullable(Of T)`ist das Ergebnis, wenn ein auf NULL festleg barer Werttyp `T?` ist, ein geachtelter Wert vom Typ `T`, kein geachtelter Wert vom Typ `T?`. Und umgekehrt wird beim Unboxing in einen Werte zulässt-Werttyp `T?`der Wert von `System.Nullable(Of T)`umschließt und `Nothing` in einen NULL-Wert des Typs `T?`entpackt werden. Beispiel:

```vb
Dim i1? As Integer = Nothing
Dim o1 As Object = i1

Console.WriteLine(o1 Is Nothing)                    ' Will print True
o1 = 10
i1 = CType(o1, Integer?)
Console.WriteLine(i1)                               ' Will print 10
```

Ein Nebeneffekt dieses Verhaltens besteht darin, dass ein Werttyp, der NULL-Werte zulässt `T?` scheinbar alle Schnittstellen `T`implementiert, da der Typ für die Typumwandlung in eine Schnittstelle konvertiert werden muss. Folglich kann `T?` in alle Schnittstellen konvertiert werden, in die `T` konvertiert werden kann. Es ist jedoch wichtig zu beachten, dass ein auf NULL festleg barer Werttyp `T?` die Schnittstellen von `T` nicht zum Zweck der generischen Einschränkungs Überprüfung oder Reflektion implementiert. Beispiel:

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

## <a name="string-conversions"></a>Zeichen folgen Konvertierungen

Das `Char` in `String` führt zu einer Zeichenfolge, deren erstes Zeichen der Zeichen Wert ist. Wenn `String` in `Char` umgewandelt wird, ergibt sich ein Zeichen, dessen Wert das erste Zeichen der Zeichenfolge ist. Wenn ein Array von `Char` in `String` umgewandelt wird, ergibt sich eine Zeichenfolge, deren Zeichen die Elemente des Arrays sind. Wenn `String` in ein Array von `Char` umgewandelt wird, ergibt sich ein Zeichen Array, dessen Elemente die Zeichen der Zeichenfolge sind.

Die genauen Konvertierungen zwischen `String` und `Boolean`, `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`, `Date`und umgekehrt überschreiten den Rahmen dieser Spezifikation und sind durch eine Ausnahme von einem Detail abhängig. Zeichen folgen Konvertierungen sollten immer die aktuelle Kultur der Laufzeitumgebung in Erwägung gezogen werden. Daher müssen Sie zur Laufzeit ausgeführt werden.

## <a name="widening-conversions"></a>Erweiternde Konvertierungen

Erweiternde Konvertierungen nie Überlauf, können aber einen Genauigkeits Verlust verursachen. Die folgenden Konvertierungen sind erweiternde Konvertierungen:

__Identitäts-/Standardkonvertierungen__

* Von einem Typ in sich selbst.

* Von einem anonymen Delegattyp, der für die Neuklassifizierung einer Lambda-Methode generiert wurde, in einen beliebigen Delegattyp mit einer identischen Signatur.

* Vom Literal`Nothing` bis zu einem Typ.

__Numerische Konvertierungen__

* Von `Byte` bis `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`oder `Double`.

* Von `SByte` bis `Short`, `Integer`, `Long`, `Decimal`, `Single`oder `Double`.

* Von `UShort` bis `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`oder `Double`.

* Von `Short` bis `Integer`, `Long`, `Decimal`, `Single` oder `Double`.

* Von `UInteger` bis `ULong`, `Long`, `Decimal`, `Single`oder `Double`.

* Von `Integer` bis `Long`, `Decimal`, `Single` oder `Double`.

* Von `ULong` bis `Decimal`, `Single`oder `Double`.

* Von `Long` bis `Decimal`, `Single` oder `Double`.

* Von `Decimal` bis `Single` oder `Double`.

* Von `Single` bis `Double`.

* Vom Literal`0` bis zu einem enumerierten Typ. (__Hinweis:__ Die Konvertierung von `0` in einen beliebigen Enumerationstyp wird erweitert, um testflags zu vereinfachen. Wenn `Values` z. b. ein Enumerationstyp mit einem Wert `One`ist, können Sie eine Variable `v` vom Typ `Values` testen, indem Sie `(v And Values.One) = 0`sagen.)

* Von einem enumerierten Typ zum zugrunde liegenden numerischen Typ oder zu einem numerischen Typ, für den der zugrunde liegende numerische Typ eine erweiternde Konvertierung in aufweist.

* Aus einem konstanten Ausdruck vom Typ `ULong`, `Long`, `UInteger`, `Integer`, `UShort`, `Short`, `Byte`oder `SByte` zu einem engeren Typ, wenn der Wert des konstanten Ausdrucks innerhalb des Bereichs des Zieltyps liegt. (__Hinweis:__ Konvertierungen von `UInteger` oder `Integer` in `Single`, `ULong` oder `Long` `Single` oder `Double`oder `Decimal` `Single` oder `Double` können zu einem Genauigkeits Verlust führen. Dies führt jedoch nie zu einem Verlust der Größe. Die anderen erweiternden numerischen Konvertierungen verlieren niemals Informationen.)

__Verweis Konvertierungen__

* Von einem Referenztyp zu einem Basistyp.

* Von einem Referenztyp zu einem Schnittstellentyp, vorausgesetzt, dass der Typ die-Schnittstelle oder eine Variant-kompatible Schnittstelle implementiert.

* Von einem Schnittstellentyp zu `Object`.

* Von einem Schnittstellentyp zu einem Variant-kompatiblen Schnittstellentyp.

* Von einem Delegattyp zu einem Variant-kompatiblen Delegattyp. (__Hinweis:__ Viele andere Verweis Konvertierungen werden von diesen Regeln impliziert. Anonyme Delegaten sind z. b. Referenztypen, die von `System.MulticastDelegate`erben; Array Typen sind Verweis Typen, die von `System.Array`erben; anonyme Typen sind Verweis Typen, die von `System.Object`erben.)

__Anonyme delegatkonvertierungen__

* Von einem anonymen Delegattyp, der für die Neuklassifizierung einer Lambda-Methode generiert wird, zu einem beliebigen breiteren Delegattyp.

__Array Konvertierungen__

* Von einem Arraytyp `S` mit einem Elementtyp `Se` zu einem Arraytyp, der mit einem Elementtyp `Te``T` wird, wenn Folgendes zutrifft:

  * `S` und `T` unterscheiden sich nur in Elementtyp.

  * Sowohl `Se` als auch `Te` sind Verweis Typen oder sind Typparameter, die als Verweistyp bekannt sind.

  * Eine erweiternde Verweis-, Array-oder Typparameter Konvertierung ist von `Se` in `Te`vorhanden.

* Von einem Arraytyp `S` mit einem enumerierten Elementtyp `Se` zu einem Arraytyp, der mit einem Elementtyp `Te``T` ist, wenn alle folgenden Punkte zutreffen:

  * `S` und `T` unterscheiden sich nur in Elementtyp.

  * `Te` ist der zugrunde liegende Typ `Se`.

* Von einem Arraytyp `S` von Rang 1 mit einem enumerierten Elementtyp `Se`zum `System.Collections.Generic.IList(Of Te)`, `IReadOnlyList(Of Te)`, `ICollection(Of Te)`, `IReadOnlyCollection(Of Te)`und `IEnumerable(Of Te)`wird eine der folgenden Angaben angezeigt:

  * Sowohl `Se` als auch `Te` sind Verweis Typen, oder sind Typparameter, die als Verweistyp bekannt sind, und ein erweiternde Verweis-, Array-oder Typparameter Konvertierung ist von `Se` in `Te`vorhanden. noch

  * `Te` ist der zugrunde liegende Typ `Se`. noch

  * `Te` ist identisch mit `Se`

__Werttyp Konvertierungen__

* Von einem Werttyp zu einem Basistyp.

* Von einem Werttyp zu einem Schnittstellentyp, den der Typ implementiert.

__Typkonvertierungen, die NULL zulassen__

* Von einem Typ `T` zum Typ `T?`.

* Von einem Typ `T?` zu einem Typ `S?`, bei dem eine erweiternde Konvertierung vom Typ `T` in den Typ `S`erfolgt.

* Von einem Typ `T` zu einem Typ `S?`, bei dem eine erweiternde Konvertierung vom Typ `T` in den Typ `S`erfolgt.

* Von einem-Typ `T?` zu einem Schnittstellentyp, den der Typ `T` implementiert.

__Zeichen folgen Konvertierungen__

* Von `Char` bis `String`.

* Von `Char()` bis `String`.

__Typparameter Konvertierungen__

* Von einem Typparameter zum `Object`.

* Von einem Typparameter zu einer Schnittstellentyp Einschränkung oder einer beliebigen Schnittstellen Variante, die mit einer Schnittstellentyp Einschränkung kompatibel ist.

* Von einem Typparameter zu einer Schnittstelle, die von einer Klassen Einschränkung implementiert wird.

* Von einem Typparameter zu einer Schnittstellen Variante, die mit einer Schnittstelle kompatibel ist, die durch eine Klassen Einschränkung implementiert wird.

* Von einem Typparameter zu einer Klassen Einschränkung oder einem Basistyp der Klassen Einschränkung.

* Von einem Typparameter `T` zu einer Typparameter Einschränkung `Tx`, oder irgendetwas `Tx` über eine erweiternde Konvertierung zu verfügt.

## <a name="narrowing-conversions"></a>Einschränkende Konvertierungen

Einschränkende Konvertierungen sind Konvertierungen, die nicht als immer erfolgreich erwiesen werden können, Konvertierungen, die bekanntermaßen Informationen verlieren, und Konvertierungen zwischen verschiedenen Domänen, die sich ausreichend von der einschränkenden Schreibweise unterscheiden. Die folgenden Konvertierungen werden als einschränkende Konvertierungen klassifiziert:

__Boolesche Konvertierungen__

* Von `Boolean` bis `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`oder `Double`.

* Von `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`oder `Double` `Boolean`.

__Numerische Konvertierungen__

* Von `Byte` bis `SByte`.

* Von `SByte` bis `Byte`, `UShort`, `UInteger`oder `ULong`.

* Von `UShort` bis `Byte`, `SByte`oder `Short`.

* Von `Short` bis `Byte`, `SByte`, `UShort`, `UInteger`oder `ULong`.

* Von `UInteger` bis `Byte`, `SByte`, `UShort`, `Short`oder `Integer`.

* Von `Integer` bis `Byte`, `SByte`, `UShort`, `Short`, `UInteger`oder `ULong`.

* Von `ULong` bis `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`oder `Long`.

* Von `Long` bis `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`oder `ULong`.

* Von `Decimal` bis `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`oder `Long`.

* Von `Single` bis `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`oder `Decimal`.

* Von `Double` bis `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`oder `Single`.

* Von einem numerischen Typ zu einem enumerierten Typ.

* Von einem enumerierten Typ zu einem numerischen Typ hat der zugrunde liegende numerische Typ eine einschränkende Konvertierung in.

* Von einem enumerierten Typ zu einem anderen Enumerationstyp.

__Verweis Konvertierungen__

* Von einem Referenztyp zu einem stärker abgeleiteten Typ.

* Von einem Klassentyp zu einem Schnittstellentyp, wenn der Klassentyp den Schnittstellentyp oder eine mit diesem kompatible Schnittstellentyp Variante nicht implementiert.

* Von einem Schnittstellentyp zu einem Klassentyp.

* Von einem Schnittstellentyp zu einem anderen Schnittstellentyp, sofern keine Vererbungs Beziehung zwischen den beiden Typen besteht und vorausgesetzt, dass Sie nicht Variant-kompatibel sind.

__Anonyme delegatkonvertierungen__

* Von einem anonymen Delegattyp, der für die Neuklassifizierung einer Lambda-Methode generiert wurde, in einen beliebigen engeren Delegattyp.

__Array Konvertierungen__

* Von einem Arraytyp `S` mit einem Elementtyp `Se`in einen Arraytyp, der mit einem Elementtyp `Te``T` ist, vorausgesetzt, dass alle folgenden Punkte zutreffen:

  * `S` und `T` unterscheiden sich nur in Elementtyp.
  * Sowohl `Se` als auch `Te` sind Verweis Typen oder sind Typparameter, die keine Werttypen sind.
  * Eine einschränkende Verweis-, Array-oder Typparameter Konvertierung ist von `Se` in `Te`vorhanden.

* Von einem Arraytyp `S` mit einem Elementtyp `Se` zu einem Arraytyp, der mit einem enumerierten Elementtyp `Te``T`, wenn alle folgenden Punkte zutreffen:

  * `S` und `T` unterscheiden sich nur in Elementtyp.
  * `Se` ist der zugrunde liegende Typ `Te`, oder es handelt sich um unterschiedliche Enumerationstypen, die denselben zugrunde liegenden Typ haben.

* Von einem Arraytyp `S` von Rang 1 mit einem enumerierten Elementtyp `Se`zum `IList(Of Te)`, `IReadOnlyList(Of Te)`, `ICollection(Of Te)`, `IReadOnlyCollection(Of Te)` und `IEnumerable(Of Te)`wird eine der folgenden Angaben angezeigt:

  * Sowohl `Se` als auch `Te` sind Verweis Typen, oder sind Typparameter, die als Verweistyp bekannt sind, und eine einschränkende Verweis-, Array-oder Typparameter Konvertierung ist von `Se` in `Te`vorhanden. noch
  * `Se` ist der zugrunde liegende Typ `Te`, oder es handelt sich um unterschiedliche Enumerationstypen, die denselben zugrunde liegenden Typ haben.

__Werttyp Konvertierungen__

* Von einem Referenztyp zu einem stärker abgeleiteten Werttyp.

* Von einem Schnittstellentyp zu einem Werttyp, sofern der Werttyp den Schnittstellentyp implementiert.

__Typkonvertierungen, die NULL zulassen__

* Von einem Typ `T?` auf einen Typ `T`.

* Von einem Typ `T?` zu einem Typ `S?`, bei dem eine einschränkende Konvertierung vom Typ `T` in den Typ `S`vorliegt.

* Von einem Typ `T` zu einem Typ `S?`, bei dem eine einschränkende Konvertierung vom Typ `T` in den Typ `S`vorliegt.

* Von einem Typ `S?` zu einem Typ `T`, bei dem eine Konvertierung vom Typ `S` in den Typ `T`erfolgt.

__Zeichen folgen Konvertierungen__

* Von `String` bis `Char`.

* Von `String` bis `Char()`.

* Von `String` bis `Boolean` und von `Boolean` bis `String`.

* Konvertierungen zwischen `String` und `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`oder `Double`.

* Von `String` bis `Date` und von `Date` bis `String`.

__Typparameter Konvertierungen__

* Von `Object` bis zu einem Typparameter.

* Von einem Typparameter zu einem Schnittstellentyp, wenn der Typparameter nicht auf diese Schnittstelle beschränkt ist oder nicht auf eine Klasse beschränkt ist, die diese Schnittstelle implementiert.

* Von einem Schnittstellentyp zu einem Typparameter.

* Von einem Typparameter zu einem abgeleiteten Typ einer Klassen Einschränkung.

* Von einem Typparameter `T` in eine Typparameter Einschränkung, `Tx` eine einschränkende Konvertierung in aufweist.

## <a name="type-parameter-conversions"></a>Typparameter Konvertierungen

Typparameter Konvertierungen werden durch die Einschränkungen festgelegt, die ggf. für Sie festgelegt werden. Ein Typparameter `T` kann immer in sich selbst, in `Object`und in einen beliebigen Schnittstellentyp konvertiert werden. Beachten Sie, dass, wenn der Typ `T` zur Laufzeit ein Werttyp ist, die Konvertierung von `T` in `Object` oder einen Schnittstellentyp eine Boxing-Konvertierung ist und die Konvertierung von `Object` oder einem Schnittstellentyp in `T` eine Unboxing-Konvertierung ist. Ein Typparameter mit einer Klassen Einschränkungs `C` definiert zusätzliche Konvertierungen vom Typparameter in `C` und seine Basisklassen und umgekehrt. Ein Typparameter, der mit einer Typparameter Einschränkung `T` `Tx` definiert eine Konvertierung in `Tx` und alle Elemente, die in `Tx` konvertiert werden.

Ein Array, dessen Elementtyp ein Typparameter mit einer Schnittstellen Einschränkung ist, `I` die gleichen kovariant-Array Konvertierungen wie ein Array mit dem Elementtyp `I`aufweisen, vorausgesetzt, dass der Typparameter auch eine `Class` oder eine Klassen Einschränkung hat (da nur Verweis Array Element-Typen kovariant sein können). Ein Array, dessen Elementtyp ein Typparameter mit einer Klassen Einschränkung ist `C` hat dieselben kovarianten Array Konvertierungen wie ein Array, dessen Elementtyp `C`ist.

Die obigen Konvertierungsregeln erlauben keine Konvertierung von nicht eingeschränkten Typparametern in nicht-Schnittstellentypen, was über rasch sein kann. Der Grund hierfür ist die Vermeidung von Verwirrung bezüglich der Semantik solcher Konvertierungen. Betrachten Sie beispielsweise die folgende Deklaration:

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long 
        Return CLng(t)    ' Error, explicit conversion not permitted
    End Function
End Class
```

Wenn die Konvertierung von `T` in `Integer` zulässig war, kann man leicht davon ausgehen, dass `X(Of Integer).F(7)` `7L`zurückgeben würde. Dies würde jedoch nicht der Fall sein, da numerische Konvertierungen nur berücksichtigt werden, wenn die Typen bekanntermaßen zur Kompilierzeit numerisch sind. Damit die Semantik eindeutig ist, muss das obige Beispiel stattdessen geschrieben werden:

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long
        Return CLng(CObj(t))    ' OK, conversions permitted
    End Function
End Class
```

## <a name="user-defined-conversions"></a>Benutzerdefinierte Konvertierungen

Systeminterne *Konvertierungen* sind Konvertierungen, die von der Sprache definiert werden (d. h. in dieser Spezifikation aufgelistet), während *benutzerdefinierte Konvertierungen* durch Überladen des `CType` Operators definiert werden. Wenn bei der Konvertierung zwischen Typen keine systeminternen Konvertierungen anwendbar sind, werden benutzerdefinierte Konvertierungen berücksichtigt. Wenn eine benutzerdefinierte Konvertierung für die Quell-und Zieltypen *am spezifischsten* ist, wird die benutzerdefinierte Konvertierung verwendet. Andernfalls wird ein Fehler bei der Kompilierzeit ausgegeben. Die spezifischere Konvertierung ist die, deren Operand dem Quelltyp am nächsten ist, und dessen Ergebnistyp dem Zieltyp am nächsten ist. Wenn Sie bestimmen, welche benutzerdefinierte Konvertierung verwendet werden soll, wird die spezifischere erweiternde Konvertierung verwendet. Wenn keine erweiternde Konvertierung am spezifischsten ist, wird die spezifischere einschränkende Konvertierung verwendet. Wenn keine spezifische einschränkende Konvertierung vorliegt, ist die Konvertierung nicht definiert, und es tritt ein Kompilierzeitfehler auf.

In den folgenden Abschnitten wird erläutert, wie die spezifischsten Konvertierungen bestimmt werden. Dabei werden die folgenden Begriffe verwendet:

Wenn eine systeminterne erweiternde Konvertierung von einem Typ `A` in einen Typ `B`vorhanden ist, und wenn weder `A` noch `B` Schnittstellen sind, wird `A` durch `B`*eingeschlossen* , und `B` *umfasst* `A`.

Der *umfassendste* Typ in einer Reihe von Typen ist der einzige Typ, der alle anderen Typen im Satz umfasst. Wenn kein einzelner Typ alle anderen Typen umfasst, hat der Satz keinen ganz umfassenden Typ. Der umfassendste Typ ist in intuitiver Hinsicht der "größte" Typ im Satz. der einzige Typ, auf den jeder der anderen Typen durch eine erweiternde Konvertierung konvertiert werden kann.

Der *am häufigsten* in einem Satz von Typen eingeschlossenen Typ ist ein Typ, der von allen anderen Typen im Satz eingeschlossen wird. Wenn kein einzelner Typ von allen anderen Typen eingeschlossen wird, hat der Satz nicht den meisten Typ. Der umfassendste Typ ist in intuitiver Hinsicht der "kleinste" Typ im Satz. der einzige Typ, der durch eine einschränkende Konvertierung in jeden der anderen Typen konvertiert werden kann.

Beim Erfassen der benutzerdefinierten benutzerdefinierten Konvertierungen für einen Typ `T?`werden stattdessen die von `T` definierten benutzerdefinierten Konvertierungs Operatoren verwendet. Wenn der Typ, in den konvertiert wird, auch ein Werttyp ist, der NULL-Werte zulässt, werden alle benutzerdefinierten Konvertierungs Operatoren von `T`, die nur nicht auf NULL festleg Bare Werttypen einschließen. Ein Konvertierungs Operator von `T` in `S` wird als Konvertierung von `T?` in `S?` angehoben und ausgewertet, indem `T?` bei Bedarf in `T`konvertiert wird. Anschließend wird der benutzerdefinierte Konvertierungs Operator von `T` zu `S` ausgewertet und `S` bei Bedarf in `S?`konvertiert. Wenn der Wert, der konvertiert wird, `Nothing`ist, konvertiert ein Operator mit erhöhten Konvertierungen jedoch direkt in den Wert `Nothing`, der als `S?`typisiert ist. Beispiel:

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

Beim Auflösen von Konvertierungen werden benutzerdefinierte Konvertierungs Operatoren immer für gesteigerte Konvertierungs Operatoren bevorzugt. Beispiel:

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

Zur Laufzeit kann das Auswerten einer benutzerdefinierten Konvertierung bis zu drei Schritte umfassen:

1. Zuerst wird der Wert mithilfe einer systeminternen Konvertierung aus dem Quelltyp in den Operanden konvertiert.

2. Anschließend wird die benutzerdefinierte Konvertierung aufgerufen.

3. Schließlich wird das Ergebnis der benutzerdefinierten Konvertierung mithilfe einer systeminternen Konvertierung in den Zieltyp konvertiert, falls erforderlich.

Beachten Sie unbedingt, dass die Auswertung einer benutzerdefinierten Konvertierung nie mehr als einen benutzerdefinierten Konvertierungs Operator einschließt.

### <a name="most-specific-widening-conversion"></a>Spezifischere erweiternde Konvertierung

Die Ermittlung des spezifischsten benutzerdefinierten erweiterungskonvertierungsoperators zwischen zwei Typen erfolgt mithilfe der folgenden Schritte:

1. Zuerst werden alle Kandidaten Konvertierungs Operatoren gesammelt. Die Operatoren für die Kandidaten Konvertierung sind alle benutzerdefinierten Erweiterungs Operatoren im Quelltyp und alle benutzerdefinierten Erweiterungs Operatoren im Zieltyp.

2. Anschließend werden alle nicht anwendbaren Konvertierungs Operatoren aus dem Satz entfernt. Ein Konvertierungs Operator ist auf einen Quelltyp und einen Zieltyp anwendbar, wenn es einen intrinsischen erweiterbaren Konvertierungs Operator vom Quelltyp zum Operanden-Typ gibt und es einen intrinsischen erweiterbaren Konvertierungs Operator vom Ergebnis des Operators zum Zieltyp gibt. Wenn keine anwendbaren Konvertierungs Operatoren vorhanden sind, gibt es keine spezifischere erweiternde Konvertierung.

3. Anschließend wird der spezifischere Quelltyp der anwendbaren Konvertierungs Operatoren bestimmt:

   * Wenn ein Konvertierungs Operator direkt aus dem Quelltyp konvertiert wird, ist der Quelltyp der spezifischere Quelltyp.

   * Andernfalls ist der spezifischere Quelltyp der am häufigsten in der kombinierten Gruppe von Quell Typen der Konvertierungs Operatoren eingeschlossenen Typ. Wenn kein Typ gefunden werden kann, der sich in der angegebenen Erweiterung befindet, gibt es keine spezifischere Erweiterungs Konvertierung.

4. Anschließend wird der spezifischere Zieltyp der anwendbaren Konvertierungs Operatoren bestimmt:

   * Wenn ein Konvertierungs Operator direkt in den Zieltyp konvertiert wird, ist der Zieltyp der spezifischere Zieltyp.

   * Andernfalls ist der spezifischere Zieltyp der umfassendste Typ in der kombinierten Gruppe von Zieltypen der Konvertierungs Operatoren. Wenn kein umfassender Typ gefunden werden kann, gibt es keine spezifischere erweiternde Konvertierung.

5. Wenn dann genau ein Konvertierungs Operator vom spezifischsten Quelltyp in den spezifischsten Zieltyp konvertiert wird, ist dies der spezifischere Konvertierungs Operator. Wenn mehr als ein solcher Operator vorhanden ist, gibt es keine spezifischere erweiternde Konvertierung.

### <a name="most-specific-narrowing-conversion"></a>Die spezifischere einschränkende Konvertierung

Die Ermittlung des spezifischsten benutzerdefinierten einschränkenden Konvertierungs Operators zwischen zwei Typen erfolgt mithilfe der folgenden Schritte:

1. Zuerst werden alle Kandidaten Konvertierungs Operatoren gesammelt. Die Operatoren für die Kandidaten Konvertierung sind alle benutzerdefinierten Konvertierungs Operatoren im Quelltyp und alle benutzerdefinierten Konvertierungs Operatoren im Zieltyp.

2. Anschließend werden alle nicht anwendbaren Konvertierungs Operatoren aus dem Satz entfernt. Ein Konvertierungs Operator ist auf einen Quelltyp und einen Zieltyp anwendbar, wenn ein System interner Konvertierungs Operator vom Quelltyp zum Operanden-Typ vorhanden ist und ein System interner Konvertierungs Operator vom Ergebnis des Operators zum Zieltyp vorhanden ist. Wenn keine anwendbaren Konvertierungs Operatoren vorhanden sind, gibt es keine spezifischere einschränkende Konvertierung.

3. Anschließend wird der spezifischere Quelltyp der anwendbaren Konvertierungs Operatoren bestimmt:

   * Wenn ein Konvertierungs Operator direkt aus dem Quelltyp konvertiert wird, ist der Quelltyp der spezifischere Quelltyp.

   * Andernfalls handelt es sich beim Konvertieren eines Konvertierungs Operators aus Typen, die den Quelltyp einschließen, um den am meisten spezifisierten Typ in der kombinierten Gruppe von Quell Typen dieser Konvertierungs Operatoren. Wenn kein Typ gefunden werden kann, der sich in der gleichen Form befindet, ist keine besonders einschränkende Konvertierung vorhanden.

   * Andernfalls ist der spezifischere Quelltyp der umfassendste Typ in der kombinierten Gruppe von Quell Typen der Konvertierungs Operatoren. Wenn kein umfassender Typ gefunden werden kann, gibt es keine spezifischere einschränkende Konvertierung.

4. Anschließend wird der spezifischere Zieltyp der anwendbaren Konvertierungs Operatoren bestimmt:

   * Wenn ein Konvertierungs Operator direkt in den Zieltyp konvertiert wird, ist der Zieltyp der spezifischere Zieltyp.

   * Wenn ein Konvertierungs Operator in Typen konvertiert wird, die durch den Zieltyp eingeschlossen werden, dann ist der spezifischere Zieltyp der umfassendste Typ in der kombinierten Gruppe von Quell Typen dieser Konvertierungs Operatoren. Wenn kein umfassender Typ gefunden werden kann, gibt es keine spezifischere einschränkende Konvertierung.

   * Andernfalls ist der spezifischere Zieltyp der Typ, der in der kombinierten Gruppe von Zieltypen der Konvertierungs Operatoren am häufigsten enthalten ist. Wenn kein Typ gefunden werden kann, der sich in der gleichen Form befindet, ist keine besonders einschränkende Konvertierung vorhanden.

5. Wenn dann genau ein Konvertierungs Operator vom spezifischsten Quelltyp in den spezifischsten Zieltyp konvertiert wird, ist dies der spezifischere Konvertierungs Operator. Wenn mehr als ein solcher Operator vorhanden ist, gibt es keine spezifischere einschränkende Konvertierung.

## <a name="native-conversions"></a>Native Konvertierungen

Einige der Konvertierungen werden als systemeigene *Konvertierungen* klassifiziert, da Sie von der .NET Framework nativ unterstützt werden. Diese Konvertierungen können mithilfe der Konvertierungs Operatoren `DirectCast` und `TryCast` sowie anderer spezieller Verhaltensweisen optimiert werden. Die als Native Konvertierungen klassifizierten Konvertierungen sind: Identitäts Konvertierungen, Standard Konvertierungen, Verweis Konvertierungen, Array Konvertierungen, Werttyp Konvertierungen und Typparameter Konvertierungen.

## <a name="dominant-type"></a>Dominanter Typ

Wenn ein Satz von Typen vorliegt, ist es häufig erforderlich, dass in Situationen wie z. b. der Typrückschluss der bestimmende *Typ* der Menge bestimmt wird. Der vorherrschende Typ eines Satzes von Typen wird bestimmt, indem zuerst alle Typen entfernt werden, für die ein oder mehrere andere Typen keine implizite Konvertierung in aufweisen. Wenn an dieser Stelle keine Typen übrig sind, gibt es keinen vorherrschenden Typ. Der bestimmende Typ ist dann die meisten der verbleibenden Typen. Wenn mehr als ein Typ vorhanden ist, der am meisten eingeschlossen ist, gibt es keinen vorherrschenden Typ.
