---
ms.openlocfilehash: 1476d208b3ca2cc7f47a458549455f64a3512e80
ms.sourcegitcommit: 6eca149bdc736113e0adb709212bd266c9503c33
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2019
ms.locfileid: "47426688"
---
# <a name="conversions"></a>Konvertierungen

Konvertierung ist der Prozess der Ändern eines Werts von einem Typ in einen anderen. Z. B. ein Wert vom Typ `Integer` konvertiert werden kann, um einen Wert vom Typ `Double`, oder ein Wert vom Typ `Derived` konvertiert werden kann, um einen Wert vom Typ `Base`, vorausgesetzt, dass `Base` und `Derived` sind sowohl Klassen als auch `Derived`erbt `Base`. Konvertierungen erfordern möglicherweise nicht den Wert selbst ändern (wie im zweiten Beispiel) oder erfordern möglicherweise die wesentlichen Änderungen in die wertedarstellung (wie im vorherigen Beispiel).

Konvertierungen können erweiternde oder einschränkende sein. Ein *eine erweiternde Konvertierung* ist eine Konvertierung von einem Typ in einen anderen Typ, dessen Wertdomäne mindestens so groß, wenn nicht größer als der ursprünglichen Domäne des Typs Wert ist. Erweiterungskonvertierungen sollte nie fehlschlagen. Ein *einschränkende Konvertierung* ist eine Konvertierung von einem Typ in einen anderen Typ, dessen Wertdomäne ist, Wert kleiner als des ursprüngliche Typs Domäne oder ausreichend unabhängig vom stagingstatus dieser zusätzlichen Vorsicht beim, Ausführen der Konvertierung (für beispielsweise bei der Konvertierung von `Integer` zu `String`). Einschränkende Konvertierungen, die zu Datenverlust führen können, kann Fehler auftreten.

Die identitätskonvertierung (d. h. eine Konvertierung von einem Typ an sich selbst) und Standard-Wert-Konvertierung (d. h. eine Konvertierung von `Nothing`) für alle Typen definiert sind.

## <a name="implicit-and-explicit-conversions"></a>Implizite und explizite Konvertierungen

Konvertierungen können es sich um *implizite* oder *explizite*. Implizite Konvertierungen treten ohne eine besondere Syntax. Folgendes ist ein Beispiel für die implizite Konvertierung des ein `Integer` -Werts in einen `Long` Wert:

```vb
Module Test
    Sub Main()
        Dim intValue As Integer = 123
        Dim longValue As Long = intValue

        Console.WriteLine(intValue & " = " & longValue)
    End Sub
End Module
```

Explizite Konvertierungen erfordern dagegen auf Umwandlungsoperatoren. Es wird versucht, führen Sie eine explizite Konvertierung in einen Wert ohne ein Cast-Operator bewirkt, dass einen Fehler während der Kompilierung. Im folgenden Beispiel wird eine explizite Konvertierung konvertiert eine `Long` -Werts in einen `Integer` Wert.

```vb
Module Test
    Sub Main()
        Dim longValue As Long = 134
        Dim intValue As Integer = CInt(longValue)

        Console.WriteLine(longValue & " = " & intValue)
    End Sub
End Module
```

Der Satz von impliziten Konvertierungen hängt von der kompilierungsumgebung und die `Option Strict` Anweisung. Wenn die strikte Semantik verwendet werden, kann die erweiternde Konvertierungen implizit auftreten. Wenn die Semantik verwendet werden, können implizit alle erweiterungskonvertierungen und einschränkende Konvertierungen (das heißt, alle Konvertierungen) auftreten.

## <a name="boolean-conversions"></a>Boolesche Konvertierungen

Obwohl `Boolean` ist kein numerischer Typ, es verfügt über einschränkende Konvertierungen in und aus der numerischen Typen, als handele es sich um einen enumerierten Typ. Das Literal `True` konvertiert, dem Literal `255` für `Byte`, `65535` für `UShort`, `4294967295` für `UInteger`, `18446744073709551615` für `ULong`, und klicken Sie auf den Ausdruck `-1` für `SByte`, `Short`, `Integer`, `Long`, `Decimal`, `Single`, und `Double`. Das Literal `False` konvertiert, dem Literal `0`. Numerischer Wert 0 konvertiert, dem Literal `False`. Alle anderen numerischen Werte zu konvertieren, dem Literal `True`.

Es ist eine einschränkende Konvertierung von booleschen Wert in eine Zeichenfolge, Konvertierung in `System.Boolean.TrueString` oder `System.Boolean.FalseString`. Es gibt auch eine einschränkende Konvertierung von `String` zu `Boolean`: Wenn die Zeichenfolge gleich `TrueString` oder `FalseString` (in der aktuellen Kultur, Groß-und Kleinschreibung nicht) verwendet dann den entsprechenden Wert; andernfalls versucht wird, analysiert die Zeichenfolge als ein numerischen Typ (in Hexadezimal oder Oktal Wenn möglich ist, andernfalls als "float") und verwendet die oben genannten Regeln; Andernfalls löst `System.InvalidCastException`.

## <a name="numeric-conversions"></a>Numerische Konvertierungen

Numerische Konvertierungen zwischen den Typen vorhanden `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single` und `Double`, und alle Enumerationstypen. Wenn konvertiert wird, werden die aufgelistete Typen behandelt, als wären sie deren zugrunde liegenden Typen. Bei der Konvertierung in einen enumerierten Typ ist der Quellwert nicht erforderlich, den Satz von Werten, die definiert, in dem enumerierten Typ entsprechen. Zum Beispiel:

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

* Für eine Konvertierung aus einem numerischen Typ in einen größeren numerischen Typ ist der Wert einfach in größeren Typ konvertiert. Konvertierungen von `UInteger`, `Integer`, `ULong`, `Long`, oder `Decimal` zu `Single` oder `Double` gerundet auf die nächste `Single` oder `Double` Wert. Während dieser Konvertierung einem Genauigkeitsverlust kommen kann, wird er nie eine Größenordnung verloren gehen.

* Für eine Konvertierung in einen ganzzahligen Typ in einen anderen ganzzahligen Typ oder von `Single`, `Double`, oder `Decimal` in einen ganzzahligen Typ, das Ergebnis hängt davon ab, ob Überprüfungen auf Ganzzahlüberlauf auf ist:

  *Wenn Ganzzahlüberlauf überprüft wird:*

  * Wenn die Quelle ein ganzzahliger Typ ist, ist die Konvertierung erfolgreich, wenn das Quellargument innerhalb des Bereichs des Zieltyps liegt. Die Konvertierung löst eine `System.OverflowException` -Ausnahme aus, wenn das Quellargument außerhalb des Bereichs des Zieltyps liegt.

  * Wenn die Quelle ist `Single`, `Double`, oder `Decimal`Quellwert wird gerundet, auf den nächsten ganzzahligen Wert auf- oder und dieser ganzzahlige Wert wird das Ergebnis der Konvertierung. Wenn der Quellwert gleichermaßen nahe bei zwei ganzzahlige Werte ist, wird der Wert auf den Wert gerundet, die eine gerade Zahl am wenigsten signifikanten Ziffern aufweist. Wenn der erzeugte Integralwert sich außerhalb des Bereichs des Zieltyps, ist eine `System.OverflowException` Ausnahme ausgelöst.

  *Wenn Ganzzahlüberlauf nicht aktiviert ist:*

  * Wenn die Quelle ein ganzzahliger Typ ist, wird die Konvertierung immer erfolgreich und besteht nur aus einer verwirft die höchstwertigen Bits des Quellwerts.

  * Wenn die Quelle ist `Single`, `Double`, oder `Decimal`, die Konvertierung immer erfolgreich, und nur der Rundung des Quellwerts für den nächsten ganzzahligen Wert besteht. Wenn der Quellwert gleichermaßen nahe bei zwei ganzzahlige Werte ist, wird der Wert immer auf den Wert gerundet, die eine gerade Zahl am wenigsten signifikanten Ziffern aufweist.

* Für eine Konvertierung von `Double` zu `Single`, `Double` Wert wird gerundet, um die nächste `Single` Wert. Wenn die `Double` Wert ist zu klein, um die Darstellung als eine `Single`, das Ergebnis positiv oder negativ 0 (null). Wenn die `Double` Wert ist zu groß, um die Darstellung als eine `Single`, wird das Ergebnis, positive oder negative Unendlichkeit. Wenn die `Double` Wert `NaN`, das Ergebnis ist ebenfalls `NaN`.

* Für eine Konvertierung von `Single` oder `Double` zu `Decimal`, wird der Quellwert in konvertiert `Decimal` Darstellung und bei Bedarf auf die nächste Zahl nach der 28. Dezimalstelle gerundet. Wenn der Quellwert zu klein, um die Darstellung als ist eine `Decimal`, wird das Ergebnis 0 (null). Wenn der Quellwert `NaN`, unendlich oder zu groß, um die Darstellung als eine `Decimal`, `System.OverflowException` Ausnahme wird ausgelöst.

* Für eine Konvertierung von `Double` zu `Single`, `Double` Wert wird gerundet, um die nächste `Single` Wert. Wenn die `Double` Wert ist zu klein, um die Darstellung als eine `Single`, das Ergebnis positiv oder negativ 0 (null). Wenn die `Double` Wert ist zu groß, um die Darstellung als eine `Single`, wird das Ergebnis, positive oder negative Unendlichkeit. Wenn die `Double` Wert `NaN`, das Ergebnis ist ebenfalls `NaN`.

## <a name="reference-conversions"></a>Verweiskonvertierungen

Verweistypen können auf einen Basistyp und umgekehrt konvertiert werden. Konvertierungen von einem Basistyp in einen stärker abgeleiteten Typ erfolgreich nur zur Laufzeit auf, wenn der konvertierte Wert ein null-Wert, der abgeleitete Typ selbst oder einen stärker abgeleiteten Typ ist.

Klassen- und Typen können in und aus einem Schnittstellentyp umgewandelt werden. Konvertierungen zwischen einem Typ und einem Schnittstellentyp erfolgreich nur zur Laufzeit angezeigt, wenn die tatsächlichen Typen, die Beteiligten eine vererbungs- oder implementierungsbeziehung-Beziehung aufweisen. Da ein Schnittstellentyp immer eine Instanz eines Typs enthalten, die von abgeleitet `Object`, ein Schnittstellentyps auch immer umgewandelt werden kann, in und aus `Object`.

__Beachten Sie.__ Es ist kein Fehler konvertieren eine `NotInheritable` Klassen zu und von Schnittstellen, die ihn nicht implementiert ist, da die Klassen, die COM-Klassen darstellen schnittstellenimplementierungen, die nicht erst bekannt sind möglicherweise zur Laufzeit. 

Fällt eine verweiskonvertierung zur Laufzeit eine `System.InvalidCastException` Ausnahme ausgelöst.

### <a name="reference-variance-conversions"></a>Varianz Verweiskonvertierungen

Generische Schnittstellen oder Delegaten können über Variante Typparameter verfügen, die Konvertierungen zwischen kompatiblen Varianten des Typs zu ermöglichen. Aus diesem Grund wird zur Laufzeit eine Konvertierung von einem Klassentyp oder einem Schnittstellentyp in einen Schnittstellentyp aufweisen, der Variante, die mit einem Schnittstellentyp kompatibel ist, es erbt oder diesen implementiert, erfolgreich ausgeführt. Auf ähnliche Weise Delegattypen umgewandelt werden können und Delegattypen aus dem Variant kompatibel. Z. B. der Typ des Delegaten

```vb
Delegate Function F(Of In A, Out R)(a As A) As R
```

eine Konvertierung von können `F(Of Object, Integer)` zu `F(Of String, Integer)`. D. h. einen Delegaten `F` nimmt `Object` kann sicher verwendet werden, als Delegat `F` nimmt `String`. Wenn der Delegat aufgerufen wird, wird die Zielmethode wird erwartet, wenn Sie ein Objekt, und eine Zeichenfolge ist ein Objekt.

Ein generischer Typ von Delegaten oder dieser Schnittstelle `S(Of S1,...,Sn)` gilt als *Variante kompatibel* mit einem generischen Typ von Schnittstellen oder Delegate `T(Of T1,...,Tn)` wenn:

* `S` und `T` bestehen sowohl aus den gleichen generischen Typ `U(Of U1,...,Un)`.

* Für jeden Typparameter `Ux`:

  * Wenn der Typparameter, ohne Abweichung dann deklariert wurde `Sx` und `Tx` muss der gleiche Typ sein.

  * Wenn der Typparameter deklariert wurde `In` und es eine widening-Identität, Standard, Verweis, Array, oder Typ muss parameterkonvertierung aus `Sx` zu `Tx`.

  * Wenn der Typparameter deklariert wurde `Out` und es eine widening-Identität, Standard, Verweis, Array, oder Typ muss parameterkonvertierung aus `Tx` zu `Sx`.

Wenn von einer Klasse die Daten in eine generische Schnittstelle mit Varianten Typparametern, konvertiert werden sollen, wenn die Klasse mehr als eine Variante Schnittstelle implementiert ist die Konvertierung mehrdeutig, wenn eine nicht Variant-Konvertierung nicht vorhanden ist. Zum Beispiel:

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

### <a name="anonymous-delegate-conversions"></a>Konvertierungen von anonymen Delegaten

Wenn ein Ausdruck, der als Lambda-Methode klassifiziert wird erneut klassifiziert als Wert in einem Kontext, wenn kein Zieltyp vorhanden ist (z. B. `Dim x = Function(a As Integer, b As Integer) a + b`), oder wenn der Zieltyp kein Delegattyp ist, ist der Typ des sich ergebenden Ausdrucks ein anonymer Delegat-Typ entspricht die Signatur der Lambda-Methode. Dieser anonymen Delegaten eine Konvertierung in einen beliebigen Typ kompatiblen Delegaten enthält: ein kompatiblen Delegattyp ist, jeden Delegattyp, der über einen Delegaterstellungsausdruck mit anonymen Delegaten des Typs erstellt werden kann `Invoke` Methode als Parameter. Zum Beispiel:

```vb
' Anonymous delegate type similar to Func(Of Object, Object, Object)
Dim x = Function(x, y) x + y

' OK because delegate type is compatible
Dim y As Func(Of Integer, Integer, Integer) = x
```

Beachten Sie, dass die Typen `System.Delegate` und `System.MulticastDelegate` selbst keine gelten Delegattypen (obwohl alle Delegattypen aus ihnen erben). Beachten Sie, dass die Konvertierung von anonymen Delegaten-Typ in einen kompatiblen Delegattyp nicht mit einer verweiskonvertierung ist.

## <a name="array-conversions"></a>Arraykonvertierungen

Neben die Konvertierungen, die auf Arrays aufgrund der Tatsache definiert sind, dass sie Verweistypen sind, werden einige spezielle Konvertierungen für Arrays vorhanden.

Für die einzelnen Typen `A` und `B`, wenn sie beide Verweistypen oder Parameter vom Typ Werttypen nicht bekannt sind, und wenn `A` verfügt über einen Verweis, array oder Parameter-typkonvertierung in `B`, eine Konvertierung aus einem Array von vorhanden ist Typ `A` in ein Array vom Typ `B` mit demselben Rang. Diese Beziehung wird als bezeichnet *Array-Kovarianz*. Array-Kovarianz vor allem bedeutet, dass ein Element eines Arrays, dessen Elementtyp `B` möglicherweise um ein Element eines Arrays, dessen Elementtyp `A`, vorausgesetzt, dass beide `A` und `B` sind Verweistypen und diese `B` verfügt über eine verweiskonvertierung oder Array-Konvertierung in `A`. Im folgenden Beispiel ist der zweite Aufruf von `F` bewirkt, dass eine `System.ArrayTypeMismatchException` Ausnahme ausgelöst wird, da der tatsächliche Elementtyp des `b` ist `String`, nicht `Object`:

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

Aufgrund der Array-Kovarianz enthalten Zuweisungen auf Elemente des Verweistyparrays eine laufzeitüberprüfung, die sicherstellt, dass der Wert zugewiesen wird, auf das Arrayelement tatsächlich einen zulässigen Typ ist.

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

In diesem Beispiel ist die Zuweisung zu `array(i)` in Methode `Fill` implizit eine laufzeitüberprüfung wird, dass das Objekt sichergestellt, das die Variable verweist, die auch `value` ist entweder `Nothing` oder eine Instanz eines Typs, die kompatibel mit der tatsächliche Elementtyp des Arrays `array`. In der Methode `Main`, die ersten beiden Aufrufe der Methode `Fill` erfolgreich ausgeführt werden, aber der dritte Aufruf bewirkt, dass eine `System.ArrayTypeMismatchException` Ausnahme ausgelöst wird, bei der Ausführung der ersten Zuweisung zu `array(i)`. Die Ausnahme tritt auf, weil ein `Integer` kann nicht gespeichert werden, einem `String` Array.

Wenn einer der die Arrayelementtypen ein Typparameter ist, dessen Typ erweist sich ein Werttyp zur Laufzeit, eine `System.InvalidCastException` Ausnahme ausgelöst. Zum Beispiel:

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

Konvertierungen zwischen ein Array von ein enumerierter Typ auch vorhanden sein, und ein Array von den enumerierten Typ zugrunde liegenden Typ oder ein Array von einem anderen enumerierten Typ mit dem gleichen zugrunde liegenden Typ, vorausgesetzt die Arrays den gleichen Rang aufweisen.

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

In diesem Beispiel ein Array von `Color` ist in und aus konvertiert ein Array von `Byte`, `Color`zugrunde liegenden Typ. Die Konvertierung in ein Array von `Integer`, allerdings wird ein Fehler vorhanden, da `Integer` ist nicht der zugrunde liegende Typ `Color`.

Ein Rang-1-Array des Typs `A()` verfügt auch über eine Array-Konvertierung in den sammlungsschnittstellentypen `IList(Of B)`, `IReadOnlyList(Of B)`, `ICollection(Of B)`, `IReadOnlyCollection(Of B)` und `IEnumerable(Of B)` finden Sie im `System.Collections.Generic`, solange eine der folgenden Aussagen zutrifft:

- `A` und `B` sind beide verweisen auf Typen oder Parameter vom Typ nicht bekannt sein Werttypen und `A` verfügt über eine erweiternde Konvertierung eines Verweis "," Array "oder" Typ-Parameter in `B`; oder
- `A` und `B` sind beide Enumerationstypen mit dem gleichen zugrunde liegenden Typ oder
- einer der `A` und `B` ist ein enumerierter Typ, und die andere ist deren zugrunde liegender Typ.

Ein Array vom Typ A mit jeder Rang verfügt auch über eine Array-Konvertierung in die Schnittstelle nicht generische Auflistungstypen `IList`, `ICollection` und `IEnumerable` finden Sie im `System.Collections`.

Es ist möglich, durchlaufen, die sich ergebenden Benutzeroberflächen mit `For Each`, oder durch Aufrufen der `GetEnumerator` Methoden direkt. Im Fall von Rang 1 Arrays konvertiert generische oder nicht generische Formen der `IList` oder `ICollection`, es ist auch möglich, die Elemente nach Index abrufen. Im Fall von Rang 1-Arrays, die generisch oder nicht generische Formen von konvertiert `IList`, es ist auch möglich, Elemente nach Index festlegen, gelten die gleiche Laufzeit Array-Kovarianz überprüft wird, wie oben beschrieben. Das Verhalten der anderen Schnittstellenmethoden ist nicht durch die VB-Language-Spezifikation definiert werden; Es ist Aufgabe der zugrunde liegenden Laufzeitumgebung.

## <a name="value-type-conversions"></a>Wert Typkonvertierungen

Ein Wert für den Typ konvertiert werden kann, auf dessen Basis Verweistypen oder einen Schnittstellentyp aufweisen, die sie mithilfe des so genannten implementiert *Boxing*. Wenn Sie ein Wert für den Typ mittels Boxing konvertiert wird, wird der Wert aus dem Speicherort kopiert, wo sie auf den .NET Framework-Heap gespeichert. Ein Verweis auf diesen Speicherort auf dem Heap wird dann zurückgegeben und kann in einen Verweistyp-Variable gespeichert werden. Dieser Verweis wird auch als bezeichnet ein *geschachtelt* Instanz des Werttyps. Die geschachtelte Instanz weist dieselbe Semantik wie ein Verweistyp anstelle eines Werttyps.

Geschachtelte Werttypen konvertiert werden können, an ihre ursprüngliche Werttyp mithilfe des so genannten *unboxing*. Wenn ein geschachtelter Werttyp mittels Unboxing konvertiert wird, wird der Wert aus dem Heap in einer Variable Speicherort kopiert. Ab diesem Punkt verhält er sich als wäre es ein Werttyp. Unboxing ein Werttyps muss der Wert ein null-Wert oder eine Instanz des Werttyps sein. Andernfalls ein `System.InvalidCastException` Ausnahme ausgelöst. Wenn der Wert einer Instanz eines enumerierten Typs ist, diesen Wert kann auch mittels Unboxing konvertiert werden, den enumerierten Typ zugrunde liegenden Typs oder einer anderen enumerierten Typ mit dem gleichen zugrunde liegenden Typ. Ein null-Wert wird behandelt, als handele es sich um das Literal `Nothing`.

Zur Unterstützung von auf NULL festlegbare Werttypen auch den Werttyp `System.Nullable(Of T)` speziell bei Aktionen, boxing und unboxing behandelt wird. Boxing einen Wert vom Typ `Nullable(Of T)` führt einen geschachtelten Wert vom Typ `T` Wenn des Zeitwerts `HasValue` -Eigenschaft ist `True` oder den Wert `Nothing` Wenn des Werts des `HasValue` -Eigenschaft ist `False`. Unboxing einen Wert vom Typ `T` zu `Nullable(Of T)` führt zu einer Instanz von `Nullable(Of T)` , deren `Value` -Eigenschaft ist der geschachtelte Wert und deren `HasValue` Eigenschaft `True`. Der Wert `Nothing` können nicht geschachtelt werden, um `Nullable(Of T)` für alle `T` und einen Wert ergibt, deren `HasValue` Eigenschaft `False`. Da geschachtelte Werttypen wie Verweis Verhalten Typen, es ist möglich, mehrere Verweise auf den gleichen Wert zu erstellen. Für die primitiven Typen und Enumerationstypen, ist dies nicht relevant, da Instanzen dieser Typen sind *unveränderliche*. Das heißt, ist es nicht möglich, eine geschachtelte Instanz dieser Typen, zu ändern, sodass es nicht möglich, beobachten Sie die Tatsache, dass es mehrere Verweise auf den gleichen Wert.

Strukturen, die möglicherweise auf der anderen Seite änderbare sein, wenn die zugehörigen Instanzvariablen zugegriffen werden kann oder seine Methoden und Eigenschaften der Instanzvariablen zu ändern. Wenn ein Verweis auf eine geschachtelte Struktur verwendet wird, um die Struktur zu ändern, klicken Sie dann sehen alle Verweise auf die geschachtelte Struktur die Änderung. Da dieses Ergebnis möglicherweise nicht erwartet werden, wenn ein Wert eingegeben als `Object` wird von einem Speicherort kopiert, auf einer anderen geschachtelten Wert Typen werden automatisch auf dem Heap, anstatt Sie lediglich ihre kopiert Verweise geklont werden. Zum Beispiel:

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

Die Ausgabe des Programms lautet:

```
Values: 0, 123
Refs: 123, 123
```

Die Zuweisung auf das Feld der lokalen Variablen `val2` wirkt sich nicht auf das Feld der lokalen Variablen `val1` da bei den mittels Boxing `Struct1` zugewiesen wurde `val2`, eine Kopie des Werts vorgenommen wurde. Im Gegensatz dazu sind die Zuweisung `ref2.Value = 123` wirkt sich auf das Objekt, das sowohl `ref1` und `ref2` verweisen.

__Beachten Sie.__ Struktur das Kopieren erfolgt nicht für geschachtelte Strukturen, die als typisierte `System.ValueType` da es nicht möglich, die spätes Binden der ist `System.ValueType`.

Es gibt eine Ausnahme zur Regel, die Wert geschachtelt, die Typen bei Zuweisung kopiert werden. Wenn ein geschachtelter Werttyp-Verweis in einem anderen Typ gespeichert ist, wird der innere Verweis nicht kopiert werden. Zum Beispiel:

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

Die Ausgabe des Programms lautet:

```
Values: 123, 123
```

Dies ist, da es sich bei der innere geschachtelte Wert nicht kopiert wird, wenn der Wert kopiert werden. Daher `val1.Value` und `val2.Value` einen Verweis auf den gleichen geschachtelten Werttyp.

__Beachten Sie.__ Die Tatsache, dass die inneren geschachtelten Werttypen nicht kopiert werden ist eine Einschränkung von .NET eingeben, um sicherzustellen, dass alle inneren geschachtelten Werttypen kopiert wurden, wenn ein Wert vom Typ System `Object` kopiert wurde wäre viel zu aufwendig.

Wie zuvor beschrieben, der geschachtelte Wert kann nur in ihren ursprünglichen Typ mittels Unboxing konvertiert werden. Primitive Typen, jedoch werden behandelt, speziell wenn-typisiert `Object`. Sie können in alle anderen primitiven Typen konvertiert werden, denen sich eine Konvertierung in befindet. Zum Beispiel:

```vb
Module Test
    Sub Main()
        Dim o As Object = 5
        Dim b As Byte = CByte(o)  ' Legal
        Console.WriteLine(b) ' Prints 5
    End Sub
End Module
```

In der Regel die geschachtelte `Integer` Wert `5` konnte nicht in mittels Unboxing zurückkonvertiert werden eine `Byte` Variable. Aber da `Integer` und `Byte` primitive Typen und eine Konvertierung die Konvertierung zulässig ist.

Es ist wichtig zu beachten, dass einen Werttyp an eine Schnittstelle konvertieren anders als generisches Argument auf eine Schnittstelle beschränkt. Beim Zugriff auf Schnittstellenmember für einen eingeschränkten Typparameter (oder Aufrufen von Methoden für `Object`), Boxing erfolgt nicht, wie bei der ein Werttyp konvertiert wird, auf eine Schnittstelle und Schnittstellenmember zugegriffen wird. Nehmen wir beispielsweise an eine Schnittstelle `ICounter` enthält eine Methode `Increment` die können verwendet werden, um einen Wert zu ändern. Wenn `ICounter` dient als eine Einschränkung, die Implementierung der `Increment` Methode wird aufgerufen, mit einem Verweis auf die Variable, die `Increment` für nicht auf eine geschachtelte Kopie wurde aufgerufen:

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

Der erste Aufruf `Increment` ändert den Wert in der Variablen `x`. Dies entspricht nicht der zweite Aufruf von `Increment`, die geändert, dass des Wert in eine geschachtelte Kopie von `x`. Daher ist die Ausgabe des Programms:

```
0
1
1
```

### <a name="nullable-value-type-conversions"></a>NULL-Wert-Typkonvertierungen

Ein Werttyp `T` können konvertieren in und aus der auf NULL festlegbare Version des Typs `T?`. Die Konvertierung von `T?` zu `T` löst eine `System.InvalidOperationException` -Ausnahme aus, wenn der konvertierte Wert ist `Nothing`. Darüber hinaus `T?` verfügt über eine Konvertierung in einen Typ `S` Wenn `T` verfügt über eine integrierte Konvertierung in `S`. Und wenn `S` ein Werttyp ist, und klicken Sie dann die folgenden systeminternen Konvertierungen zwischen bestehen `T?` und `S?`:

* Eine Konvertierung der gleichen Klassifizierung (einschränken oder erweitern) aus `T?` zu `S?`.

* Eine Konvertierung der gleichen Klassifizierung (einschränken oder erweitern) aus `T` zu `S?`.

* Eine einschränkende Konvertierung von `S?` zu `T`.

Z. B. eine erweiternde Konvertierung vorhanden ist, aus `Integer?` zu `Long?` , da eine erweiternde Konvertierung von vorhanden `Integer` zu `Long`:

```vb
Dim i As Integer? = 10
Dim l As Long? = i
```

Beim Konvertieren von `T?` zu `S?`, wenn der Wert des `T?` ist `Nothing`, klicken Sie dann den Wert der `S?` werden `Nothing`. Beim Konvertieren von `S?` zu `T` oder `T?` zu `S`, wenn der Wert des `T?` oder `S?` ist `Nothing`, `System.InvalidCastException` Ausnahme wird ausgelöst.

Aufgrund des Verhaltens des zugrunde liegenden Typs `System.Nullable(Of T)`, wenn ein Werttyp `T?` ist geschachtelt, die das Ergebnis ist einen geschachtelten Wert vom Typ `T`, nicht auf einen geschachtelten Wert vom Typ `T?`. Und im Gegenzug, eine auf NULL festlegbaren Werttyp Unboxing `T?`, der Wert von umbrochen `System.Nullable(Of T)`, und `Nothing` wird mittels Unboxing konvertiert, um einen null-Wert des Typs `T?`. Zum Beispiel:

```vb
Dim i1? As Integer = Nothing
Dim o1 As Object = i1

Console.WriteLine(o1 Is Nothing)                    ' Will print True
o1 = 10
i1 = CType(o1, Integer?)
Console.WriteLine(i1)                               ' Will print 10
```

Ein Nebeneffekt, dass dieses Verhalten ist, geben Sie ein NULL-Wert `T?` angezeigt wird, implementiert alle Schnittstellen der `T`, da den Typ der zu schachtelnde konvertiert einen Werttyp an eine Schnittstelle benötigt werden. Daher `T?` konvertiert werden kann, alle Schnittstellen, die `T` konvertierbar ist. Es ist wichtig, aber beachten Sie, dass ein NULL-Wert `T?` implementiert die Schnittstellen nicht tatsächlich `T` im Rahmen der Überprüfung von generischen Einschränkungen oder Reflektion. Zum Beispiel:

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

## <a name="string-conversions"></a>Zeichenfolgenkonvertierungen

Konvertieren von `Char` in `String` führt zu einer Zeichenfolge, deren erste Zeichen der Zeichenwert ist. Konvertieren von `String` in `Char` führt zu einem Zeichen, dessen Wert das erste Zeichen der Zeichenfolge ist. Konvertiert ein Array von `Char` in `String` führt zu einer Zeichenfolge, deren Zeichen die Elemente des Arrays sind. Konvertieren von `String` in ein Array von `Char` führt ein Array von Zeichen, dessen Elemente die Zeichen der Zeichenfolge sind.

Die genaue Konvertierungen zwischen `String` und `Boolean`, `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`, `Date`, und umgekehrt, sind Gegenstand dieser Spezifikation und Implementierung mit Ausnahme von einem Details hängen. Zeichenfolgenkonvertierungen sollten Sie immer die aktuelle Kultur der Runtime-Umgebung. Daher müssen sie zur Laufzeit ausgeführt werden.

## <a name="widening-conversions"></a>Erweiterungskonvertierungen

Erweiternde Konvertierungen nie überlaufen jedoch können ein Genauigkeitsverlust gelten. Die folgenden Konvertierungen sind erweiternde Konvertierungen auf:

__Identität/standardkonvertierungen__

* Von einem Typ an sich selbst.

* Von ein anonymen Delegaten-Typ, die für einen Lambda-Methode neuklassifizierung der jedem Delegattyp mit einer identischen Signatur generiert werden.

* Durch das Literal `Nothing` auf einen Typ.

__Numerische Konvertierungen__

* Von `Byte` zu `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, oder `Double`.

* Von `SByte` zu `Short`, `Integer`, `Long`, `Decimal`, `Single`, oder `Double`.

* Von `UShort` zu `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, oder `Double`.

* Von `Short` zu `Integer`, `Long`, `Decimal`, `Single` oder `Double`.

* Von `UInteger` zu `ULong`, `Long`, `Decimal`, `Single`, oder `Double`.

* Von `Integer` zu `Long`, `Decimal`, `Single` oder `Double`.

* Von `ULong` zu `Decimal`, `Single`, oder `Double`.

* Von `Long` zu `Decimal`, `Single` oder `Double`.

* Von `Decimal` zu `Single` oder `Double`.

* Von `Single` zu `Double`.

* Durch das Literal `0` für einen enumerierten Typ. (__Beachten.__ Die Konvertierung von `0` auf einen beliebigen enumerierten Typ ist zur Vereinfachung von Tests Flags erweitern. Wenn z. B. `Values` ist ein enumerierter Typ mit einem Wert `One`, testen Sie eine Variable `v` des Typs `Values` Satz: `(v And Values.One) = 0`.)

* Einen enumerierten Typ in den zugrunde liegenden numerischen Typ oder in einen numerischen Datentyp, dem der zugrunde liegenden numerische Typ eine erweiternde Konvertierung in verfügt.

* Aus einem konstanten Ausdruck vom Typ `ULong`, `Long`, `UInteger`, `Integer`, `UShort`, `Short`, `Byte`, oder `SByte` ein schmaler bereitgestellt der Wert des konstanten Ausdrucks wird in der der Bereich des Zieltyps. (__Beachten.__ Konvertierungen von `UInteger` oder `Integer` zu `Single`, `ULong` oder `Long` zu `Single` oder `Double`, oder `Decimal` zu `Single` oder `Double` zu einem Genauigkeitsverlust führen können soll, aber nie Führen Sie einen Verlust der Größe. Die anderen erweiterungskonvertierungen numerischen verlieren keine Informationen.)

__Verweiskonvertierungen__

* Von einem Referenztyp zu einem Basistyp.

* Von einem Referenztyp in einen Schnittstellentyp, vorausgesetzt, dass der Typ der Schnittstelle oder eine Variante Schnittstelle implementiert.

* Von einem Schnittstellentyp in `Object`.

* Von einem Schnittstellentyp in einen Typ variant-kompatible Schnittstelle.

* Delegieren Sie aus einem Delegattyp auf einen Variant-kompatibler Typ aus. (__Beachten.__ Viele andere verweiskonvertierungen sind von diesen Regeln impliziert. Z. B. anonyme Delegaten sind Verweistypen, die von erben `System.MulticastDelegate`; Arraytypen sind Verweistypen, die von erben `System.Array`, anonyme Typen sind Referenztypen, die von erben `System.Object`.)

__Anonyme Delegaten-Konvertierungen__

* Von einem anonymen-Delegattyp einen Lambda-Methode begründet jedem größeren Delegattyp generiert.

__Arraykonvertierungen__

* Von einem Arraytyp `S` mit einem Elementtyp `Se` in einen Arraytyp `T` mit einem Elementtyp `Te`, sofern alle der folgenden Bedingungen erfüllt sind:

  * `S` und `T` unterscheiden sich nur hinsichtlich der Elementtyp.

  * Beide `Se` und `Te` sind Verweistypen oder bekanntermaßen Typparameter ein Verweistyp sein muss.

  * Eine erweiternde Verweis, Array oder Typ konvertieren Parameter vorhanden ist, von `Se` zu `Te`.

* Von einem Arraytyp `S` mit einem enumerierten Elementtyp `Se` in einen Arraytyp `T` mit einem Elementtyp `Te`, sofern alle der folgenden Bedingungen erfüllt sind:

  * `S` und `T` unterscheiden sich nur hinsichtlich der Elementtyp.

  * `Te` ist der zugrunde liegenden Typ des `Se`.

* Von einem Arraytyp `S` von Rang 1 mit einem enumerierten Elementtyp `Se`zu `System.Collections.Generic.IList(Of Te)`, `IReadOnlyList(Of Te)`, `ICollection(Of Te)`, `IReadOnlyCollection(Of Te)`, und `IEnumerable(Of Te)`, sofern eine der folgenden Aussagen zutrifft:

  * Beide `Se` und `Te` sind Verweistypen oder Parameter vom Typ bekannt als Referenz Typ und einem erweiternde Verweis, array oder typparameterumwandlung vorhanden ist, von `Se` zu `Te`; oder

  * `Te` ist der zugrunde liegenden Typ des `Se`; oder

  * `Te` ist identisch mit `Se`

__Wert typkonvertierungen__

* Von einem Werttyp in einen Basistyp.

* Von einem Werttyp in einen Schnittstellentyp, den der Typ implementiert.

__Auf NULL festlegbaren Werttyp-Konvertierungen__

* Von einem Typ `T` in den Typ `T?`.

* Von einem Typ `T?` auf einen Typ `S?`, wobei es eine erweiternde Konvertierung vom Typ ist `T` in den Typ `S`.

* Von einem Typ `T` auf einen Typ `S?`, wobei es eine erweiternde Konvertierung vom Typ ist `T` in den Typ `S`.

* Von einem Typ `T?` Geben Sie auf eine Schnittstelle, die den Typ `T` implementiert.

__Zeichenfolgenkonvertierungen__

* Von `Char` zu `String`.

* Von `Char()` zu `String`.

__Typkonvertierungen für Parameter__

* Von einem Typparameter um `Object`.

* Von einem Typparameter eine schnittstelleneinschränkung-Typ oder eine Schnittstelle-Variante, die mit einer Schnittstelle typeinschränkung kompatibel.

* Von einem Typparameter an eine Schnittstelle, die durch eine klasseneinschränkung implementiert.

* Von einem Typparameter eine Schnittstelle-Variante, die kompatibel mit einer Schnittstelle, die durch eine klasseneinschränkung implementiert.

* Von einem Typparameter ein Class-Einschränkung oder einem Basistyp von der Class-Einschränkung.

* Von einem Typparameter `T` zu einer Einschränkung eines Typparameters `Tx`, oder etwas `Tx` verfügt über eine erweiternde Konvertierung in.

## <a name="narrowing-conversions"></a>Eingrenzungskonvertierungen

Einschränkende Konvertierungen sind Konvertierungen, die nachgewiesen werden können nicht immer erfolgreich ist, Konvertierungen, die bekannt ist, dass Sie möglicherweise Informationen verloren gehen und Konvertierungen domänenübergreifend Typen ausreichend verschieden sind, sollten einschränkende Notation an. Die folgenden Konvertierungen werden als einschränkende Konvertierungen klassifiziert:

__Boolesche Konvertierungen__

* Von `Boolean` zu `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, oder `Double`.

* Von `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, oder `Double` zu `Boolean`.

__Numerische Konvertierungen__

* Von `Byte` zu `SByte`.

* Von `SByte` zu `Byte`, `UShort`, `UInteger`, oder `ULong`.

* Von `UShort` zu `Byte`, `SByte`, oder `Short`.

* Von `Short` zu `Byte`, `SByte`, `UShort`, `UInteger`, oder `ULong`.

* Von `UInteger` zu `Byte`, `SByte`, `UShort`, `Short`, oder `Integer`.

* Von `Integer` zu `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, oder `ULong`.

* Von `ULong` zu `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, oder `Long`.

* Von `Long` zu `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, oder `ULong`.

* Von `Decimal` zu `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, oder `Long`.

* Von `Single` zu `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, oder `Decimal`.

* Von `Double` zu `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, oder `Single`.

* Aus einem numerischen Typ für einen enumerierten Typ.

* Einen enumerierten Typ in einen numerischen Typ ist der zugrunde liegenden numerische Typ eine einschränkende Konvertierung in.

* Einen enumerierten Typ in einen anderen enumerierten Typ.

__Verweiskonvertierungen__

* Von einem Referenztyp zu einem stärker abgeleiteten Typ.

* Von einem Klassentyp in einen Schnittstellentyp angegeben, dass der Klassentyp der Schnittstellentyp oder eine Variante der Schnittstelle Typ kompatibel nicht implementiert.

* Von einem Schnittstellentyp in einen Klassentyp.

* Typ in einen anderen Schnittstellentyp, sofern von einer Schnittstelle ist keine vererbungsbeziehung zwischen den beiden Typen und vorausgesetzt, dass sie nicht variant kompatibel sind.

__Anonyme Delegaten-Konvertierungen__

* Von einem anonymen Delegaten-Typ für einen Lambda-Methode neuklassifizierung von jedem schmaler Delegattyp generiert.

__Arraykonvertierungen__

* Von einem Arraytyp `S` mit einem Elementtyp `Se`, in einen Arraytyp `T` mit einem Elementtyp `Te`, vorausgesetzt, dass alle der folgenden Bedingungen erfüllt sind:

  * `S` und `T` unterscheiden sich nur hinsichtlich der Elementtyp.
  * Beide `Se` und `Te` sind Verweistypen oder Parameter vom Typ nicht bekanntermaßen Werttypen.
  * Eine einschränkende Verweis, Array oder typparameterumwandlung vorhanden ist, von `Se` zu `Te`.

* Von einem Arraytyp `S` mit einem Elementtyp `Se` in einen Arraytyp `T` mit einem enumerierten Elementtyp `Te`, sofern alle der folgenden Bedingungen erfüllt sind:

  * `S` und `T` unterscheiden sich nur hinsichtlich der Elementtyp.
  * `Se` ist der zugrunde liegenden Typ des `Te` , oder sie sind beide unterschiedlichen Enumerationstypen, die den gleichen zugrunde liegenden Typ aufweisen.

* Von einem Arraytyp `S` von Rang 1 mit einem enumerierten Elementtyp `Se`zu `IList(Of Te)`, `IReadOnlyList(Of Te)`, `ICollection(Of Te)`, `IReadOnlyCollection(Of Te)` und `IEnumerable(Of Te)`, sofern eine der folgenden Aussagen zutrifft:

  * Beide `Se` und `Te` sind Verweistypen oder bekanntermaßen Typparameter ein Verweistyp sein und eine einschränkende Verweis, Array oder typparameterumwandlung vorhanden ist, von `Se` zu `Te`; oder
  * `Se` ist der zugrunde liegenden Typ des `Te`, oder sie sind beide unterschiedlichen Enumerationstypen, die den gleichen zugrunde liegenden Typ aufweisen.

__Wert typkonvertierungen__

* Von einem Referenztyp zu einem stärker abgeleiteten Werttyp.

* Von einem Schnittstellentyp in einen Werttyp sofern der Werttyp den Schnittstellentyp implementiert.

__Auf NULL festlegbaren Werttyp-Konvertierungen__

* Von einem Typ `T?` auf einen Typ `T`.

* Von einem Typ `T?` auf einen Typ `S?`, bei dem es eine einschränkende Konvertierung vom Typ ist `T` in den Typ `S`.

* Von einem Typ `T` auf einen Typ `S?`, bei dem es eine einschränkende Konvertierung vom Typ ist `T` in den Typ `S`.

* Von einem Typ `S?` auf einen Typ `T`, bei dem es eine Konvertierung vom Typ erfolgt `S` in den Typ `T`.

__Zeichenfolgenkonvertierungen__

* Von `String` zu `Char`.

* Von `String` zu `Char()`.

* Von `String` zu `Boolean` und `Boolean` zu `String`.

* Konvertierungen zwischen `String` und `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, oder `Double`.

* Von `String` zu `Date` und `Date` zu `String`.

__Typkonvertierungen für Parameter__

* Von `Object` auf einen Typparameter.

* Parameter in einen Schnittstellentyp, den Type-Parameter angegeben, ist von einem Typ nicht beschränkt auf die Schnittstelle oder beschränkt auf eine Klasse, die diese Schnittstelle implementiert.

* Von einem Schnittstellentyp für einen Typparameter.

* Von einem Typparameter für einen abgeleiteten Typ von einer Class-Einschränkung.

* Von einem Typparameter `T` was immer einer Einschränkung eines Typparameters `Tx` verfügt über eine einschränkende Konvertierung in.

## <a name="type-parameter-conversions"></a>Typkonvertierungen für Parameter

Typparameter Konvertierungen werden durch die Einschränkungen, bestimmt, wenn vorhanden, legen Sie auf diese. Ein Typparameter `T` immer auf sich selbst konvertiert werden kann, in und aus `Object`, und aus einem Schnittstellentyp. Beachten Sie, dass bei den Datentyp `T` ist ein Werttyp zur Laufzeit, Konvertieren von `T` zu `Object` oder ein Schnittstellentyp wird ein Boxing-Konvertierung und Konvertieren von `Object` oder eine Schnittstelle, um geben `T` werden ein unboxing die Konvertierung. Ein Typparameter mit einer Class-Einschränkung `C` definiert zusätzliche Konvertierungen aus der Typparameter `C` und deren Basisklassen, und umgekehrt. Ein Typparameter `T` mit einer Einschränkung eines Typparameters `Tx` definiert eine Konvertierung in `Tx` und alles, was `Tx` in konvertiert.

Ein Array, dessen Elementtyp ein Typparameter eine schnittstelleneinschränkung ist `I` hat die gleichen arraykonvertierungen von kovarianten als ein Array, dessen Elementtyp `I`, vorausgesetzt, dass der Typparameter verfügt auch über eine `Class` oder Einschränkung (-Klasse Da nur Verweis-Arrayelementtypen kovariant sein können). Ein Array, dessen Elementtyp ein Typparameter mit einer Class-Einschränkung ist `C` hat die gleichen arraykonvertierungen von kovarianten als ein Array, dessen Elementtyp `C`.

Die oben genannten Regeln für Konvertierungen erlauben keine Konvertierungen von uneingeschränkte Typparameter auf Typen von nicht-Schnittstelle kann überraschend sein. Der Grund dafür ist, um Verwechslungen über die Semantik der solche Konvertierungen. Betrachten Sie beispielsweise die folgende Deklaration:

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long 
        Return CLng(t)    ' Error, explicit conversion not permitted
    End Function
End Class
```

Wenn die Konvertierung von `T` zu `Integer` erteilt wurden, wird einfach erwartet, `X(Of Integer).F(7)` zurück `7L`. Es wird jedoch nicht der Fall, da numerische Konvertierungen nur gelten, wenn die Typen bekannt ist, dass zum Zeitpunkt der Kompilierung ein Zahlenwert angegeben werden. Um die Semantik machen muss klare und im obige Beispiel stattdessen geschrieben werden:

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long
        Return CLng(CObj(t))    ' OK, conversions permitted
    End Function
End Class
```

## <a name="user-defined-conversions"></a>Benutzerdefinierte Konvertierungen

*Systeminterne Konvertierungen* sind Konvertierungen, die von der Programmiersprache (z. B. in dieser Spezifikation aufgeführt), während definiert *benutzerdefinierte Konvertierungen* werden durch Überladung definiert die `CType` Operator. Wenn zwischen Typen konvertiert werden soll, wenn keine systeminternen Konvertierungen gelten, werden benutzerdefinierte Konvertierungen angesehen. Wenn es gibt eine benutzerdefinierte Konvertierung, die *spezifischste* für die Quelle und Ziel-Typen, klicken Sie dann die benutzerdefinierte Konvertierung verwendet werden. Andernfalls führt ein Fehler während der Kompilierung. Die spezifischsten Konvertierung wird, deren Operanden "nächstgelegene" in den Quelltyp und, dessen Ergebnistyp "nächstgelegene" in den Zieltyp. Wenn Sie welche benutzerdefinierte Konvertierung verwenden zu bestimmen, wird die spezifischste erweiternde Konvertierung verwendet. Wenn keine widening ist die Konvertierung am spezifischsten, die spezifischste einschränkende Konvertierung verwendet wird. Wenn nicht am genauesten einschränkende Konvertierung vorhanden ist, klicken Sie dann die Konvertierung nicht definiert ist und ein Fehler während der Kompilierung auftritt.

Die folgenden Abschnitte enthalten, wie die spezifischsten Konvertierungen bestimmt werden. Sie können die folgenden Begriffe verwendet:

Wenn eine systeminterne erweiternde Konvertierung vorhanden ist, von einem Typ `A` auf einen Typ `B`, und wenn weder `A` noch `B` Schnittstellen sind `A` ist *darin enthaltenen* von `B`, und `B` *umfasst* `A`.

Die *umfassendste* Typ in einen Satz von Typen ist, der eine Typ, der alle anderen Typen in der Menge umfasst. Wenn kein Typ auf alle anderen Typen umfasst, hat dann die Gruppe keine umfassendste. Intuitiv ausgedrückt ist der umfassendste Typ "größten"-Typs in der Gruppe: der eine Typ, in dem alle anderen Typen über eine erweiternde Konvertierung konvertiert werden kann.

Die *die darin enthaltenen* Typ in einen Satz von Typen ist, der eine Typ, der von allen anderen Typen in der Gruppe Datenbankprotokolls enthalten ist. Wenn kein Typ von allen anderen Typen Datenbankprotokolls enthalten ist, hat die Gruppe nicht am häufigsten Typ einschließt. Intuitiv ausgedrückt ist der am stärksten umfasste Typ den "kleinsten" Datentyp in der Gruppe: der eine Typ, der in jeder der anderen Typen über eine einschränkende Konvertierung konvertiert werden kann.

Wenn die benutzerdefinierte Konvertierungen mit Kandidat für einen Typ sammeln `T?`, definierten Operatoren für die benutzerdefinierte Konvertierung `T` stattdessen verwendet. Wenn der Typ konvertiert wird, um auch NULL-Werte zulassen, und klicken Sie dann eine der `T`des benutzerdefinierten Operatoren für Konvertierungen, bei denen nur die nicht auf NULL festlegbare Werttypen aufgehoben werden. Einen Konvertierungsoperator `T` zu `S` transformiert wird, um eine Konvertierung von `T?` zu `S?` und wird durch die Konvertierung ausgewertet `T?` zu `T`, falls erforderlich, und klicken Sie dann die benutzerdefinierte Konvertierung auswerten Operator aus `T` zu `S` und dann wandle `S` zu `S?`, falls erforderlich. Wenn der konvertierte Wert ist `Nothing`, ein transformierten Konvertierungsoperator konvertiert jedoch direkt in einen Wert von `Nothing` als `S?`. Zum Beispiel:

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

Beim Auflösen von Konvertierungen, die eine benutzerdefinierte werden Operatoren für Konvertierungen immer über transformierten Konvertierungsoperatoren bevorzugt. Zum Beispiel:

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

1. Zunächst wird der Wert aus einer Datenquelle in der Operandentyp, verwenden eine Konvertierung, bei Bedarf konvertiert.

2. Anschließend wird die benutzerdefinierte Konvertierung aufgerufen.

3. Schließlich wird das Ergebnis der eine benutzerdefinierte Konvertierung in den Zieltyp an, mit der eine Konvertierung, bei Bedarf konvertiert.

Es ist wichtig zu beachten, dass die Auswertung einer benutzerdefinierten Konvertierung umfasst nie mehr als eine benutzerdefinierte Konvertierung-Operator.

### <a name="most-specific-widening-conversion"></a>Am spezifischsten ist eine erweiternde Konvertierung

Bestimmen die spezifischste benutzerdefinierte erweiternde Operator für die Konvertierung zwischen zwei Typen erfolgt mithilfe der folgenden Schritte:

1. Zunächst werden alle die Candidate-Konvertierungsoperatoren gesammelt. Die Candidate-Konvertierungsoperatoren sind alle benutzerdefinierten erweiternde Konvertierungsoperatoren des Quelltyps und alle benutzerdefinierten erweiternde Konvertierungsoperatoren in den Zieltyp.

2. Anschließend werden alle Operatoren für unzulässige Konvertierung aus dem Satz entfernt werden. Ein Konvertierungsoperator ist auf einem Quell- und Zieltyp anwendbar, wenn systeminterne erweiternde Konvertierungsoperator aus einer Datenquelle in den Operandentyp wird und ein systeminternen erweiternde Konvertierungsoperator aus dem Ergebnis des Operators, in den Zieltyp. Wenn es keine entsprechenden Konvertierungsoperatoren sind, besteht keine spezifischste eine erweiternde Konvertierung.

3. Anschließend wird die spezifischste Quelltyp, der die entsprechenden Konvertierungsoperatoren bestimmt:

   * Wenn keines der Konvertierungsoperatoren direkt aus dem Quelltyp konvertieren, ist der Quelltyp einen möglichst spezifischen Quelltyp.

   * Anderenfalls ist ein möglichst spezifische Quelltyp der am stärksten umfasste Typ in der kombinierten Gruppe von Typen die Konvertierungsoperatoren. Wenn Nein, die die darin enthaltenen kann Typ gefunden werden, dann gibt es keine spezifischste eine erweiternde Konvertierung.

4. Anschließend wird die spezifischste Zieltyp, der die entsprechenden Konvertierungsoperatoren bestimmt:

   * Wenn keines der Konvertierungsoperatoren direkt in den Zieltyp konvertieren, ist der Zieltyp der spezifischste Zieltyp.

   * Anderenfalls ist die spezifischste Zieltyp der umfassendste Typ in der kombinierten Gruppe von Zieltypen die Konvertierungsoperatoren. Wenn kein umfassendste Typ gefunden werden kann, besteht keine spezifischste eine erweiternde Konvertierung.

5. Klicken Sie dann, wenn genau ein Konvertierungsoperator von einen möglichst spezifischen Quelltyp in einen möglichst spezifischen Zieltyp konvertiert werden soll, ist dies die spezifischste Konvertierungsoperator. Wenn mehr als einen solchen Operator vorhanden ist, ist keine spezifischste erweiternde Konvertierung vorhanden.

### <a name="most-specific-narrowing-conversion"></a>Spezifischste einschränkende Konvertierung

Bestimmen die spezifischste benutzerdefinierte einschränkende Operator für die Konvertierung zwischen zwei Typen erfolgt mithilfe der folgenden Schritte:

1. Zunächst werden alle die Candidate-Konvertierungsoperatoren gesammelt. Die Candidate-Konvertierungsoperatoren sind alle Operatoren benutzerdefinierte Konvertierung in den Quelltyp und alle Operatoren benutzerdefinierte Konvertierung in den Zieltyp.

2. Anschließend werden alle Operatoren für unzulässige Konvertierung aus dem Satz entfernt werden. Ein Konvertierungsoperator ist auf einem Quell- und Zieltyp anwendbar, wenn Konvertierungsoperator aus einer Datenquelle in den Operandentyp wird und ein Operator Konvertierung aus dem Ergebnis des Operators, in den Zieltyp. Wenn es keine entsprechenden Konvertierungsoperatoren sind, besteht keine spezifischste einschränkende Konvertierung.

3. Anschließend wird die spezifischste Quelltyp, der die entsprechenden Konvertierungsoperatoren bestimmt:

   * Wenn keines der Konvertierungsoperatoren direkt aus dem Quelltyp konvertieren, ist der Quelltyp einen möglichst spezifischen Quelltyp.

   * Andernfalls, wenn keines der Operatoren für die Konvertierung von Datentypen, die den Quelltyp umfassen konvertieren, ist ein möglichst spezifische Quelltyp der am stärksten umfasste Typ in der kombinierten Gruppe von Quelltypen diese Konvertierungsoperatoren. Wenn Nein, die die darin enthaltenen kann Typ gefunden werden, und klicken Sie dann keine spezifischste einschränkende Konvertierung besteht.

   * Anderenfalls ist ein möglichst spezifische Quelltyp der umfassendste Typ in der kombinierten Gruppe von Typen die Konvertierungsoperatoren. Wenn kein umfassendste Typ gefunden werden kann, besteht keine spezifischste einschränkende Konvertierung.

4. Anschließend wird die spezifischste Zieltyp, der die entsprechenden Konvertierungsoperatoren bestimmt:

   * Wenn keines der Konvertierungsoperatoren direkt in den Zieltyp konvertieren, ist der Zieltyp der spezifischste Zieltyp.

   * Andernfalls, wenn keines der Operatoren für die Konvertierung in Typen, die durch den Typ des Datenbankprotokolls enthalten sind konvertieren, ist ein möglichst spezifische Zieltyp der umfassendste Typ in der kombinierten Gruppe von Quelltypen diese Konvertierungsoperatoren. Wenn kein umfassendste Typ gefunden werden kann, besteht keine spezifischste einschränkende Konvertierung.

   * Anderenfalls ist die spezifischste Zieltyp der am stärksten umfasste Typ in der kombinierten Gruppe von Zieltypen die Konvertierungsoperatoren. Wenn Nein, die die darin enthaltenen kann Typ gefunden werden, und klicken Sie dann keine spezifischste einschränkende Konvertierung besteht.

5. Klicken Sie dann, wenn genau ein Konvertierungsoperator von einen möglichst spezifischen Quelltyp in einen möglichst spezifischen Zieltyp konvertiert werden soll, ist dies die spezifischste Konvertierungsoperator. Wenn mehr als einen solchen Operator vorhanden ist, besteht keine spezifischste einschränkende Konvertierung.

## <a name="native-conversions"></a>Systemeigene Konvertierungen

Einige der Konvertierungen werden als klassifiziert *systemeigene Konvertierungen* , da sie standardmäßig von .NET Framework unterstützt werden. Diese Konvertierungen, die mithilfe des optimiert werden können, sind die `DirectCast` und `TryCast` Konvertierungsoperatoren sowie andere speziellen Verhaltensweisen. Die Konvertierungen, die als systemeigene Konvertierungen sind klassifiziert: Identität Konvertierungen, standardkonvertierungen, Verweis-, arraykonvertierungen, typkonvertierungen Wert und typkonvertierungen-Parameter.

## <a name="dominant-type"></a>Der bestimmende Typ

Wenn einen Satz von Typen, es ist häufig erforderlich in Situationen wie z. B. Typrückschluss, um zu bestimmen, die *bestimmende Typ* des Satzes. Der bestimmende Typ, der einen Satz von Typen wird durch die zuvor entfernt, denen einen oder mehrere Typen keine implizite Konvertierung in Typen bestimmt. Es sind keine Typen links an diesem Punkt ein, ist es kein dominanter Typ. Der bestimmende Typ ist, und klicken Sie dann die meisten der verbleibenden Typen einschließt. Liegt mehr als ein eingeben, wird am häufigsten einschließt, dann gibt es kein dominanter Typ.
