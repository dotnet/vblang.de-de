---
ms.openlocfilehash: a9499e51a67a9b311ae54410c001f8bd22c30d65
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306067"
---
# <a name="overloaded-method-resolution"></a>Überladene Methoden Auflösung

In der Praxis sind die Regeln zum Bestimmen der Überladungs Auflösung darauf ausgerichtet, die Überladung zu ermitteln, die den tatsächlich angegebenen Argumenten am nächsten liegt. Wenn eine Methode vorhanden ist, deren Parametertypen den Argument Typen entsprechen, ist diese Methode offensichtlich die nächstgelegene Methode. Wenn Sie dies tun, ist eine Methode näher als eine Methode, wenn alle Parametertypen schmaler oder gleich der Parametertypen der anderen Methode sind. Wenn keine der Methoden Parameter schmaler als die andere ist, gibt es keine Möglichkeit, zu bestimmen, welche Methode näher an den Argumenten liegt.

__Nebenbei.__ Bei der Überladungs Auflösung wird der erwartete Rückgabetyp der Methode nicht berücksichtigt. 

Beachten Sie auch, dass die Reihenfolge der tatsächlichen und formalen Parameter aufgrund der Syntax für benannte Parameter möglicherweise nicht identisch ist.

Wenn eine Methoden Gruppe angegeben ist, wird die am meisten anwendbare Methode in der Gruppe für eine Argumentliste anhand der folgenden Schritte bestimmt. Wenn nach dem Anwenden eines bestimmten Schritts keine Mitglieder im Satz verbleiben, tritt ein Kompilierzeitfehler auf. Wenn nur ein Member im Satz verbleibt, ist dieses Element der am meisten anwendbare Member. Die Schritte lauten wie folgt:

1.  Wenn keine Typargumente angegeben wurden, wenden Sie zunächst den Typrückschluss auf alle Methoden an, die über Typparameter verfügen. Wenn der Typrückschluss für eine Methode erfolgreich ist, werden die abherleitet Typargumente für diese spezielle Methode verwendet. Wenn bei der Typrückschluss für eine Methode ein Fehler auftritt, wird diese Methode aus dem Satz entfernt.

2.  Entfernen Sie als nächstes alle Elemente aus dem Satz, auf die nicht zugegriffen werden kann (Abschnitt [Anwendbarkeit auf Argument Liste](overload-resolution.md#applicability-to-argument-list)), und führen Sie die Argumentliste aus.

3.  Wenn mindestens ein Argument `AddressOf` oder Lambda-Ausdrücke ist, berechnen Sie dann die delegataufruseebenen für jedes derartige Argument wie unten beschrieben. Wenn der schlechteste (niedrigste) aufrusegrad in `N` schlimmer ist als der niedrigste Wert für die delegatentspannungsstufe in `M`, entfernen Sie `N` aus dem Satz. Die delegatentspannungsstufen lauten wie folgt:

    31. *Fehler delegatauffüllungsebene* --, wenn die `AddressOf` oder der Lambda nicht in den Delegattyp konvertiert werden kann.
  
    32. Einschränkende *delegatlockerung von Rückgabetyp oder Parametern* : Wenn das Argument `AddressOf` oder ein Lambda mit einem deklarierten Typ ist und die Konvertierung des Rückgabe Typs in den delegatrückgabetyp einschränkend ist; oder wenn das Argument ein reguläres Lambda ist und die Konvertierung von einem der Rückgabe Ausdrücke in den Delegattyp des Delegaten einschränkend ist, oder wenn das Argument ein Async-Lambda ist und der delegatrückgabetyp `Task(Of T)` ist, und die Konvertierung von allen Rückgabe Ausdrücken in @No __t-3 ist einschränkend. oder wenn es sich bei dem Argument um einen iteratorlambda handelt und der Delegattyp `IEnumerator(Of T)` oder `IEnumerable(Of T)` ist und die Konvertierung von seinen Yield-Operanden in `T` einschränkend ist.

    33. *Erweiternde delegatentspannung zu Delegaten ohne Signatur,* wenn der Delegattyp `System.Delegate` oder `System.MultiCastDelegate` oder `System.Object` ist.

    34. *Drop Return oder Arguments delegatentspannung* --wenn das Argument `AddressOf` oder ein Lambda mit einem deklarierten Rückgabetyp und der Delegattyp keinen Rückgabetyp hat. oder wenn es sich bei dem Argument um einen Lambda-Wert mit mindestens einem Rückgabe Ausdruck handelt und der Delegattyp keinen Rückgabetyp hat. oder wenn das Argument `AddressOf` oder Lambda ohne Parameter ist und der Delegattyp über Parameter verfügt.

    35. *Erweiternde delegatlockerung des Rückgabe Typs* , wenn das Argument `AddressOf` oder ein Lambda mit einem deklarierten Rückgabetyp ist und eine erweiternde Konvertierung von seinem Rückgabetyp zum Delegaten erfolgt. oder wenn es sich bei dem Argument um einen regulären Lambda-Ausdruck handelt, bei dem die Konvertierung von allen Rückgabe Ausdrücken in den Delegattyp des Delegaten eine Erweiterung oder Identität mit mindestens einer Erweiterung ist oder wenn das Argument ein Async-Lambda ist und der Delegat `Task(Of T)` oder `Task` ist und die Konvertierung von allen Rückgabe Ausdrücken in `T` @ no__t-5 @ no__t-6 eine Erweiterung oder Identität mit mindestens einer Erweiterung ist. oder wenn es sich bei dem Argument um einen iteratorlambda handelt und der Delegat `IEnumerator(Of T)` oder `IEnumerable(Of T)` oder `IEnumerator` oder 0 ist, und die Konvertierung von allen Rückgabe Ausdrücken in 1 @ no__t-12 @ no__t-13 eine Erweiterung oder Identität mit mindestens einer Erweiterung ist.

    36. *Lockerung des Identitäts* Delegaten: Wenn das Argument ein `AddressOf` oder ein Lambda ist, das exakt mit dem Delegaten übereinstimmt, ohne die Werte zu erweitern, einzuschränken oder zu verwerfen oder Ergebnisse zurückgibt oder zurückgibt. Wenn für einige Member der Gruppe keine einschränkenden Konvertierungen erforderlich sind, damit Sie auf die Argumente angewendet werden können, entfernen Sie dann alle Member, die dies tun. Zum Beispiel:

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

4.  Als nächstes erfolgt die Löschung auf der Grundlage der Einschränkung wie folgt. (Beachten Sie, dass, wenn Option Strict aktiviert ist, alle Member, die einschränkende Elemente erfordern, bereits inanwendbar (Abschnitt [Anwendbarkeit auf Argument Liste](overload-resolution.md#applicability-to-argument-list)) und durch Schritt 2 entfernt wurden.)

    41. Wenn für einige Instanzmember der Gruppe nur einschränkende Konvertierungen erforderlich sind, bei denen der Argumenttyp `Object` ist, dann entfernen Sie alle anderen Elemente.
    42. Wenn die Gruppe mehr als einen Member enthält, der nur von `Object` beschränkt werden muss, wird der Aufruf Ziel Ausdruck als spät gebundener Methoden Zugriff neu klassifiziert (und es wird ein Fehler ausgegeben, wenn der Typ, der die Methoden Gruppe enthält, eine Schnittstelle ist oder eine der zutreffenden Member waren Erweiterungs Mitglieder).
    43. Wenn es Kandidaten gibt, die nur eine Einschränkung von numerischen Literalen erfordern, wählen Sie anhand der folgenden Schritte die spezifischsten unter allen verbleibenden Kandidaten aus. Wenn der Gewinner nur die Einschränkung von numerischen Literalen erfordert, wird er als Ergebnis der Überladungs Auflösung ausgewählt. andernfalls handelt es sich um einen Fehler.

    __Nebenbei.__ Die Begründung für diese Regel ist, dass ein Programm, bei dem es sich um eine lose Typisierung handelt (d. h., die meisten oder alle Variablen als `Object` deklariert sind), schwierig sein kann, wenn viele Konvertierungen von `Object` einschränkend sind. Anstatt dass die Überladungs Auflösung in vielen Situationen fehlschlägt (erfordert die starke Typisierung der Argumente für den Methodenaufruf), wird die Auflösung der entsprechenden überladenen Methode bis zur Laufzeit verzögert. Dadurch kann der lose typisierte-Befehl ohne zusätzliche Umwandlungen erfolgreich ausgeführt werden. Ein unglücklicher Nebeneffekt hiervon ist jedoch, dass die Ausführung des spät gebundenen Aufrufes erfordert, dass das CallTarget in `Object` umgewandelt wird. Bei einem Struktur Wert bedeutet dies, dass der Wert in einen temporären konvertiert werden muss. Wenn die Methode, die schließlich aufgerufen wird, versucht, ein Feld der Struktur zu ändern, geht diese Änderung verloren, sobald die Methode zurückgegeben wird. Schnittstellen werden von dieser speziellen Regel ausgeschlossen, da die späte Bindung immer für die Member der Lauf Zeit Klasse oder des Struktur Typs aufgelöst wird, die möglicherweise andere Namen als die Member der Schnittstellen, die Sie implementieren.

5.  Wenn als nächstes Instanzmethoden in der Menge verbleiben, die keine Einschränkung erfordern, entfernen Sie alle Erweiterungs Methoden aus dem Satz. Zum Beispiel:

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

    __Nebenbei.__ Erweiterungs Methoden werden ignoriert, wenn anwendbare Instanzmethoden vorhanden sind, um zu gewährleisten, dass das Hinzufügen eines Imports (der möglicherweise neue Erweiterungs Methoden in den Gültigkeitsbereich bringt) keinen-Rückruf für eine vorhandene Instanzmethode zum erneuten Binden an eine Erweiterungsmethode auslöst. Aufgrund des umfassenden Umfangs einiger Erweiterungs Methoden (d. h. der für Schnittstellen und/oder Typparameter definierten) ist dies ein sicherer Ansatz für die Bindung an Erweiterungs Methoden.

6.  Wenn die beiden Member der Menge `M` und `N` sind, sind `M` *genauer (Abschnitts* Genauigkeit von Membern/Typen, bei denen [eine Argumentliste angegeben](overload-resolution.md#specificity-of-memberstypes-given-an-argument-list)ist) als `N` bei Angabe der Argumentliste. entfernen Sie `N` aus dem Satz. Wenn mehr als ein Member in der Menge verbleiben und die übrigen Member nicht gleichermaßen spezifisch sind, wenn die Argumentliste angegeben ist, tritt ein Kompilierzeitfehler auf.

7.  Wenden Sie andernfalls in der angegebenen Reihenfolge die folgenden Regeln für die Trennung von Regeln an, wenn Sie zwei Member der Gruppe `M` und `N` verwenden:

    71. Wenn `M` keinen ParamArray-Parameter hat, aber `N`, oder wenn beides, aber `M` weniger Argumente an den ParamArray-Parameter übergibt, als `N` dies tut, entfernen Sie `N` aus dem Satz. Zum Beispiel:

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

        Im obigen Beispiel wird die folgende Ausgabe erzeugt:

        ```console
        F(Object, Object())
        F(Object, Object, Object())
        F(Object, Object, Object())
        G(Object)
        ```

        __Nebenbei.__ Wenn eine Klasse eine Methode mit einem ParamArray-Parameter deklariert, ist es nicht ungewöhnlich, dass Sie auch einige der erweiterten Formulare als reguläre Methoden einschließen. Auf diese Weise ist es möglich, die Zuordnung einer Array Instanz zu vermeiden, die auftritt, wenn eine erweiterte Form einer Methode mit einem ParamArray-Parameter aufgerufen wird.

    72. Wenn `M` in einem stärker abgeleiteten Typ als `N` definiert ist, entfernen Sie `N` aus dem Satz. Zum Beispiel:

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

        Diese Regel gilt auch für die Typen, für die Erweiterungs Methoden definiert sind. Zum Beispiel:

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

    73. Wenn `M` und `N` Erweiterungs Methoden sind und der Zieltyp von `M` eine Klasse oder Struktur ist und der Zieltyp `N` eine Schnittstelle ist, entfernen Sie `N` aus dem Satz. Zum Beispiel:

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

    74. Wenn `M` und `N` Erweiterungs Methoden sind und der Zieltyp von `M` und `N` nach Typparameter Ersetzung identisch ist, und der Zieltyp von `M` vor der Typparameter Ersetzung keine Typparameter, sondern den Zieltyp `N` führt und dann weniger Typparameter als der Zieltyp `N` aus, entfernen Sie `N` aus dem Satz. Zum Beispiel:

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

    75. Entfernen Sie vor dem Ersetzen von Typargumenten, wenn `M` *weniger generisch* ist (Abschnitts [Generizität](overload-resolution.md#genericity)) als `N`, entfernen Sie `N` aus dem Satz.

    76. Wenn `M` keine Erweiterungsmethode ist und `N` ist, entfernen Sie `N` aus dem Satz.

    77. Wenn `M` und `N` Erweiterungs Methoden sind und `M` vor `N` gefunden wurde (Abschnitts [Erweiterungs Methoden](expressions.md#extension-method-collection)Auflistung), entfernen Sie `N` aus dem Satz. Zum Beispiel:

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

        Wenn die Erweiterungs Methoden im gleichen Schritt gefunden wurden, sind diese Erweiterungs Methoden mehrdeutig. Der Aufruf kann immer mit dem Namen des Standardmoduls, das die Erweiterungsmethode enthält, eindeutig sein und die Erweiterungsmethode aufrufen, als ob es sich um einen regulären Member handelt. Zum Beispiel:

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

    78. Wenn `M` und `N` einen Typrückschluss erforderlich sind, um Typargumente zu liefern, und `M` nicht erfordert, dass der bestimmende Typ für die Typargumente bestimmt wird (d. h. jeder der Typargumente, die zu einem einzelnen Typ abgeleitet wurden), aber `N` hat `N` entfernt. aus dem Satz.

        __Nebenbei.__ Diese Regel stellt sicher, dass die Überladungs Auflösung, die in früheren Versionen erfolgreich war (bei der das Ableiten mehrerer Typen für ein Typargument zu einem Fehler führen würde), weiterhin die gleichen Ergebnisse erzeugt.

    79. Wenn die Überladungs Auflösung durchgeführt wird, um das Ziel eines Ausdrucks der Delegaterstellung aus einem `AddressOf`-Ausdruck aufzulösen, und sowohl der Delegat als auch der `M` Funktionen sind, während `N` eine Unterroutine ist, wird `N` aus dem Satz entfernt. Ebenso gilt, wenn sowohl der Delegat als auch der `M` Unterroutinen sind, während `N` eine Funktion ist, `N` aus dem Satz auszuschließen.

    710. Wenn `M` keine optionalen Parameter Standardwerte anstelle von expliziten Argumenten verwendet hat, aber `N` hat, entfernen Sie `N` aus dem Satz.

    711. Wenn `M` *eine größere Tiefe von Genericity* (Abschnitts [Generizität](overload-resolution.md#genericity)) als `N` hat, bevor Typargumente ersetzt wurden, entfernen Sie `N` aus dem Satz.

8. Andernfalls ist der Aufruf mehrdeutig, und ein Kompilierzeitfehler tritt auf.

#### <a name="specificity-of-memberstypes-given-an-argument-list"></a>Spezifizität von Membern/Typen mit einer Argumentliste

Ein Element `M` wird als *gleich* `N` betrachtet, wenn eine Argumentliste `A` ist, wenn die Signaturen identisch sind oder jeder Parametertyp in `M` mit dem entsprechenden Parametertyp in `N` übereinstimmt.

__Nebenbei.__ Zwei Member können aufgrund von Erweiterungs Methoden in einer Methoden Gruppe mit der gleichen Signatur enden. Zwei Member können auch gleich spezifisch sein, haben aber aufgrund von Typparametern oder ParamArray-Erweiterungen nicht die gleiche Signatur.

Ein Member `M` gilt als *spezifischere* als `N`, wenn die Signaturen unterschiedlich sind und mindestens ein Parametertyp in `M` spezifischer als ein Parametertyp in `N` ist und kein Parametertyp in `N` spezifischer als ein Parameter ist. Geben Sie `M` ein. Wenn ein Parameter paar `Mj` und `Nj` ist, das mit einem Argument `Aj` übereinstimmt, wird der Typ des `Mj` als *spezifischere* angesehen als der Typ von `Nj`, wenn eine der folgenden Bedingungen zutrifft:

* Es gibt eine erweiternde Konvertierung vom Typ `Mj` in den Typ `Nj`. (__Hinweis:__ Da Parametertypen in diesem Fall ohne Berücksichtigung des eigentlichen Arguments verglichen werden, wird die erweiternde Konvertierung von konstanten Ausdrücken in einen numerischen Typ, in den der Wert passt, in diesem Fall nicht berücksichtigt.)

* `Aj` ist die Literale `0`, `Mj` ein numerischer Typ und `Nj` ein enumerierter Typ ist. (__Hinweis:__ Diese Regel ist erforderlich, da die Literale `0` zu einem beliebigen enumerierten Typ zurückgesetzt wird. Da ein enumerierter Typ auf den zugrunde liegenden Typ zurückgesetzt wird, bedeutet dies, dass bei der Überladungs Auflösung auf `0` standardmäßig Enumerationstypen für numerische Typen bevorzugt werden. Wir haben viel Feedback erhalten, dass dieses Verhalten kontraintuitiv war.)

* `Mj` und `Nj` sind numerische Typen, und `Mj` kommt vor `Nj` in der Liste `Byte`, `SByte`, `Short`, `UShort`, `Integer`, `UInteger`, 0, 1, 2, 3, 4. (__Hinweis:__ Die Regel zu den numerischen Typen ist nützlich, da die numerischen Typen mit und ohne Vorzeichen einer bestimmten Größe nur einschränkende Konvertierungen zwischen diesen Typen aufweisen. Die obige Regel unterbricht die Verknüpfung zwischen den beiden Typen zugunsten des "natürlichen" numerischen Typs. Dies ist besonders wichtig bei der Überladungs Auflösung für einen Typ, der zu den numerischen Typen mit Vorzeichen und ohne Vorzeichen einer bestimmten Größe erweitert wird, z. b. einem numerischen Literalwert, der in beides passt.)

* `Mj` und `Nj` sind delegatfunktionstypen, und der Rückgabetyp von `Mj` ist spezifischer als der Rückgabetyp `Nj`, wenn `Aj` als Lambda-Methode klassifiziert ist, und `Mj` oder `Nj` `System.Linq.Expressions.Expression(Of T)` ist, dann das Typargument des Typs (vorausgesetzt, dass es ein ist). Delegattyp) wird durch den Typ ersetzt, der verglichen wird.

* `Mj` ist identisch mit dem Typ von `Aj`, und `Nj` ist nicht. (__Hinweis:__ Es ist interessant zu beachten, dass sich die vorherige Regel gering C#fügig von unter C# scheidet, in der erfordert, dass die delegatfunktionstypen identische Parameterlisten aufweisen, bevor Rückgabe Typen verglichen werden, Visual Basic jedoch nicht.)

#### <a name="genericity"></a>Generika

Ein Element `M` ist so festgelegt, dass es *weniger generisch* als ein Mitglied `N` ist, wie im folgenden dargestellt:

1. Wenn für jedes Paar von übereinstimmenden Parametern `Mj` und `Nj` `Mj` in Bezug auf Typparameter in der Methode weniger oder gleichmäßig generisch als `Nj` ist und mindestens ein `Mj` in Bezug auf Typparameter in der Methode weniger generisch ist.
2. Andernfalls ist für jedes Paar von übereinstimmenden Parametern `Mj` und `Nj` `Mj` kleiner oder gleichmäßig generisch als `Nj` in Bezug auf Typparameter für den Typ, und mindestens ein `Mj` ist in Bezug auf Typparameter des Typs weniger generisch. , `M` ist weniger generisch als `N`.

Ein Parameter `M` gilt als gleichermaßen generisch für einen Parameter `N`, wenn die Typen `Mt` und `Nt` auf Typparameter verweisen oder beide nicht auf Typparameter verweisen. `M` gilt als weniger generisch als `N`, wenn `Mt` nicht auf einen Typparameter verweist und `Nt` dies tut.

Zum Beispiel:

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

Erweiterungs Methoden-Typparameter, die während der Currying korrigiert wurden, werden als Typparameter für den Typ betrachtet, nicht als Typparameter für die Methode. Zum Beispiel:

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

#### <a name="depth-of-genericity"></a>Tiefe der Generika

Ein Element `M` wird so festgelegt, dass es eine *größere Tiefe von Generika* als ein Mitglied `N` hat, wenn für jedes Paar von übereinstimmenden Parametern `Mj` und `Nj` `Mj` eine größere oder gleiche *Tiefe von Generika* als `Nj` aufweist und mindestens ein `Mj` hat eine größere Tiefe von Generika. Die Tiefe der Generizität wird wie folgt definiert:

* Alle anderen als ein Typparameter haben eine größere Tiefe der Generizität als ein Typparameter.

* Ein konstruierter Typ hat rekursiv eine größere Tiefe von Generika als ein anderer konstruierter Typ (mit der gleichen Anzahl von Typargumenten), wenn mindestens ein Typargument eine größere Tiefe von Generizität aufweist und kein Typargument weniger tief als der entsprechende Typ aufweist. Argument in der anderen.

* Ein Arraytyp hat eine größere Tiefe von Generika als ein anderer Arraytyp (mit der gleichen Anzahl von Dimensionen), wenn der Elementtyp des ersten-Elements eine größere Tiefe der Generizität aufweist als der Elementtyp der zweiten.

Zum Beispiel:

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

### <a name="applicability-to-argument-list"></a>Anwendbarkeit für Argument Liste

Eine Methode gilt *für einen* Satz von Typargumenten, Positions Argumenten und benannte Argumente, wenn die Methode mithilfe der Argumentlisten aufgerufen werden kann. Die Argumentlisten werden wie folgt mit den Parameterlisten verglichen:

1. Vergleichen Sie zuerst jedes Positions Argument entsprechend der Liste der Methoden Parameter. Wenn mehr Positions Argumente als Parameter vorhanden sind und der letzte Parameter kein ParamArray ist, ist die Methode nicht anwendbar. Andernfalls wird der ParamArray-Parameter mit Parametern des paramarray-Elementtyps erweitert, sodass er mit der Anzahl der positionellen Argumente übereinstimmt. Wenn ein Positions Argument weggelassen wird, das in ein ParamArray-Array wechselt, ist die Methode nicht anwendbar.
2. Vergleichen Sie als nächstes jedes benannte Argument mit einem Parameter mit dem angegebenen Namen. Wenn eines der benannten Argumente nicht übereinstimmt, mit einem ParamArray-Parameter übereinstimmt oder mit einem Argument übereinstimmt, das bereits mit einem anderen positionellen oder benannten Argument übereinstimmt, ist die Methode nicht anwendbar.
3. Wenn die Typargumente angegeben wurden, werden Sie als nächstes mit der Typparameter Liste abgeglichen. Wenn die beiden Listen nicht die gleiche Anzahl von Elementen aufweisen, ist die Methode nicht anwendbar, es sei denn, die Typargument Liste ist leer. Wenn die Typargument Liste leer ist, wird der Typrückschluss verwendet, um die Typargument Liste zu versuchen und abzuleiten. Wenn das Ableiten von Typen fehlschlägt, ist die Methode nicht anwendbar. Andernfalls werden die Typargumente an der Stelle der Typparameter in der Signatur ausgefüllt. Wenn nicht übereinstimmende Parameter nicht optional sind, ist die Methode nicht anwendbar.
4. Wenn die Argument Ausdrücke nicht implizit in die Typen der entsprechenden Parameter konvertiert werden können, ist die Methode nicht anwendbar.
5. Wenn ein Parameter ByRef ist und es keine implizite Konvertierung vom Typ des Parameters in den Typ des Arguments gibt, ist die Methode nicht anwendbar.
6. Wenn Typargumente gegen die Einschränkungen der Methode verstoßen (einschließlich der aus Schritt 3 abgelegten Typargumente), ist die Methode nicht anwendbar. Zum Beispiel:

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

Wenn ein einzelner Argument Ausdruck mit einem ParamArray-Parameter übereinstimmt und der Typ des Argument Ausdrucks sowohl in den Typ des ParamArray-Parameters als auch in den ParamArray-Elementtyp konvertiert werden kann, ist die-Methode sowohl in den erweiterten als auch in nicht erweiterten Formularen anwendbar. mit zwei Ausnahmen. Wenn die Konvertierung vom Typ des Argument Ausdrucks in den ParamArray-Typ einschränkend ist, ist die-Methode nur in der erweiterten Form anwendbar. Wenn es sich bei dem Argument Ausdruck um die Literale `Nothing` handelt, ist die Methode nur in ihrer nicht erweiterten Form anwendbar. Zum Beispiel:

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

Im obigen Beispiel wird die folgende Ausgabe erzeugt:

```console
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

Im ersten und letzten Aufruf von `F` ist die normale Form von `F` anwendbar, da eine erweiternde Konvertierung vom Argumenttyp zum Parametertyp vorhanden ist (beide sind vom Typ `Object()`), und das Argument wird als regulärer Wert Parameter übergeben. Im zweiten und dritten Aufruf ist die normale Form von `F` nicht anwendbar, da keine erweiternde Konvertierung vom Argumenttyp zum Parametertyp vorhanden ist (Konvertierungen von `Object` in `Object()` sind einschränkend). Allerdings ist die erweiterte Form von `F` anwendbar, und ein 1-Element `Object()` wird durch den Aufruf erstellt. Das einzelne Element des Arrays wird mit dem angegebenen Argument Wert initialisiert (der selbst ist ein Verweis auf einen `Object()`).

### <a name="passing-arguments-and-picking-arguments-for-optional-parameters"></a>Übergeben von Argumenten und Auswählen von Argumenten für optionale Parameter

Wenn ein Parameter ein Wert Parameter ist, muss der übereinstimmende Argument Ausdruck als Wert klassifiziert werden. Der Wert wird in den Typ des Parameters konvertiert und zur Laufzeit als Parameter übergeben. Wenn es sich bei dem Parameter um einen Verweis Parameter handelt und der übereinstimmende Argument Ausdruck als Variable klassifiziert ist, deren Typ mit dem-Parameter übereinstimmt, wird ein Verweis auf die Variable zur Laufzeit als Parameter übergeben.

Andernfalls, wenn der übereinstimmende Argument Ausdruck als Variablen-, Wert-oder Eigenschafts Zugriff klassifiziert wird, wird eine temporäre Variable vom Typ des Parameters zugeordnet. Vor dem Methodenaufruf zur Laufzeit wird der Argument Ausdruck als Wert neu klassifiziert, in den Typ des Parameters konvertiert und der temporären Variablen zugewiesen. Anschließend wird ein Verweis auf die temporäre Variable als Parameter übergeben. Nachdem der Methodenaufruf ausgewertet wurde und der Argument Ausdruck als Variablen-oder Eigenschaften Zugriff klassifiziert ist, wird die temporäre Variable dem Variablen Ausdruck oder dem Eigenschafts Zugriffs Ausdruck zugewiesen. Wenn der Eigenschafts Zugriffs Ausdruck keinen `Set`-Accessor aufweist, wird die Zuweisung nicht durchgeführt.

Bei optionalen Parametern, bei denen kein Argument angegeben wurde, wählt der Compiler wie unten beschrieben Argumente aus. In allen Fällen testet er nach der generischen Typersetzung den Parametertyp.

* Wenn der optionale Parameter das-Attribut `System.Runtime.CompilerServices.CallerLineNumber` hat und der Aufruf von einem Speicherort im Quellcode aus erfolgt, und ein numerisches wahrsten, das die Zeilennummer dieses Speicher Orts darstellt, eine intrinsische Konvertierung in den Parametertyp aufweist, wird das numerische Literale verwendet. Wenn der Aufruf mehrere Zeilen umfasst, ist die Wahl der zu verwendenden Zeile von der Implementierung abhängig.

* Wenn der optionale Parameter das-Attribut `System.Runtime.CompilerServices.CallerFilePath` hat und der Aufruf von einem Speicherort im Quellcode aus erfolgt und ein Zeichenfolgenliteral, das den Dateipfad des Speicher Orts darstellt, eine intrinsische Konvertierung in den Parametertyp aufweist, wird das Zeichenfolgenliteralzeichen verwendet Das Format des Dateipfads ist implementierungsabhängig.

* Wenn der optionale-Parameter das-Attribut `System.Runtime.CompilerServices.CallerMemberName` hat und der Aufruf innerhalb des Texts eines Typmembers oder eines Attributs liegt, das auf einen beliebigen Teil dieses Typmembers angewendet wird, und ein zeichenfolgenliteralelement, das den Elementnamen darstellt, eine intrinsische Konvertierung in den Parametertyp aufweist. , wird das Zeichenfolgenliterale verwendet. Bei aufrufen, die Teil von Eigenschaftenaccessoren oder benutzerdefinierten Ereignis Handlern sind, ist der verwendete Elementname der der Eigenschaft oder des Ereignisses. Bei aufrufen, die Teil eines Operators oder Konstruktors sind, wird ein Implementierungs spezifischer Name verwendet.

Wenn keines der oben genannten Bedingungen zutrifft, wird der Standardwert des optionalen Parameters verwendet (oder `Nothing`, wenn kein Standardwert angegeben ist). Wenn mehr als eine der oben genannten Bedingungen zutrifft, ist die Entscheidung, welche verwendet werden soll, implementierungsabhängig.

Die Attribute "`CallerLineNumber`" und "`CallerFilePath`" sind für die Protokollierung nützlich. Der `CallerMemberName` ist für die Implementierung von `INotifyPropertyChanged` nützlich. Hier finden Sie Beispiele.

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

Zusätzlich zu den oben genannten optionalen Parametern werden von Microsoft Visual Basic auch einige zusätzliche optionale Parameter erkannt, wenn Sie aus Metadaten importiert werden (d. h. aus einem dll-Verweis):

* Beim Importieren aus Metadaten behandelt Visual Basic auch den Parameter `<Optional>` als Hinweis darauf, dass der Parameter optional ist: auf diese Weise ist es möglich, eine Deklaration zu importieren, die über einen optionalen Parameter, aber keinen Standardwert verfügt, obwohl dies nicht ausgedrückt werden kann. Verwenden des `Optional`-Schlüssel Worts.

* Wenn der optionale-Parameter das-Attribut `Microsoft.VisualBasic.CompilerServices.OptionCompareAttribute` hat und das numerische Literale 1 oder 0 eine Konvertierung in den-Parametertyp aufweist, verwendet der Compiler als Argument entweder das Literale 1, wenn `Option Compare Text` wirksam ist, oder das Literale 0, wenn `Optional Compare Binary` wirksam ist.

* Wenn der optionale-Parameter das-Attribut `System.Runtime.CompilerServices.IDispatchConstantAttribute` und den Typ `Object` aufweist und keinen Standardwert angibt, verwendet der Compiler das Argument `New System.Runtime.InteropServices.DispatchWrapper(Nothing)`.

* Wenn der optionale-Parameter das-Attribut `System.Runtime.CompilerServices.IUnknownConstantAttribute` und den Typ `Object` aufweist und keinen Standardwert angibt, verwendet der Compiler das Argument `New System.Runtime.InteropServices.UnknownWrapper(Nothing)`.

* Wenn der optionale Parameter den Typ `Object` aufweist und keinen Standardwert angibt, verwendet der Compiler das Argument `System.Reflection.Missing.Value`.

### <a name="conditional-methods"></a>Bedingte Methoden

Wenn die Ziel Methode, auf die ein Aufruf Ausdruck verweist, eine Unterroutine ist, die kein Member einer Schnittstelle ist, und die Methode über ein oder mehrere `System.Diagnostics.ConditionalAttribute`-Attribute verfügt, hängt die Auswertung des Ausdrucks von den zu diesem Zeitpunkt definierten Konstanten für bedingte Kompilierung ab. in der Quelldatei. Jede Instanz des-Attributs gibt eine Zeichenfolge an, die eine Konstante für eine bedingte Kompilierung benennt. Jede bedingte Kompilierungs Konstante wird ausgewertet, als wäre sie Teil einer Anweisung für die bedingte Kompilierung. Wenn die Konstante zu `True` ausgewertet wird, wird der Ausdruck normalerweise zur Laufzeit ausgewertet. Wenn die Konstante zu `False` ausgewertet wird, wird der Ausdruck überhaupt nicht ausgewertet.

Bei der Suche nach dem-Attribut wird die am meisten abgeleitete Deklaration einer über schreibbaren Methode geprüft.

__Nebenbei.__ Das-Attribut ist für Funktionen oder Schnittstellen Methoden ungültig und wird ignoriert, wenn es bei beiden Methoden angegeben wird. Daher werden bedingte Methoden nur in aufrufenden Anweisungen angezeigt.

### <a name="type-argument-inference"></a>Typargument Rückschluss

Wenn eine Methode mit Typparametern aufgerufen wird, ohne Typargumente anzugeben, wird ein *Typargument abgeleitet* , um die Typargumente für den Aufruf abzuleiten. Dadurch wird eine natürlichere Syntax zum Aufrufen einer Methode mit Typparametern ermöglicht, wenn die Typargumente trivial abgeleitet werden können. Beispielsweise mit der folgenden Methoden Deklaration:

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

Es ist möglich, die `Choose`-Methode aufzurufen, ohne explizit ein Typargument anzugeben:

```vb
' calls Choose(Of Integer)
Dim i As Integer = Util.Choose(True, 5, 213)
' calls Choose(Of String)
Dim s As String = Util.Choose(False, "a", "b") 
```

Durch die Typargument Ableitung werden die Typargumente `Integer` und `String` von den Argumenten der Methode bestimmt.

Typargument Rückschluss findet *vor* der Neuklassifizierung von Ausdrücken in Lambda-Methoden oder Methoden Zeigern in der Argumentliste statt, da bei der Neuklassifizierung dieser beiden Arten von Ausdrücken der Typ des Parameters möglicherweise bekannt ist.  Wenn ein Satz von Argumenten `A1,...,An`, ein Satz übereinstimmender Parameter `P1,...,Pn` und ein Satz von Methodentypparametern `T1,...,Tn`, werden die Abhängigkeiten zwischen den Argumenten und den Methodentypparametern zuerst wie folgt erfasst:

* Wenn `An` das `Nothing`-literalist, werden keine Abhängigkeiten generiert.

* Wenn `An` eine Lambda-Methode und der Typ von `Pn` ein konstruierter Delegattyp oder `System.Linq.Expressions.Expression(Of T)` ist, wobei `T` ein konstruierter Delegattyp ist.

* Wenn der Typ eines Lambda-Methoden Parameters vom Typ des entsprechenden Parameters `Pn` abgeleitet wird und der Typ des Parameters von einem Methodentypparameter `Tn` abhängt, hat `An` eine Abhängigkeit von `Tn`.

* Wenn der Typ eines Lambda-Methoden Parameters angegeben wird und der Typ des entsprechenden Parameters `Pn` von einem Methodentypparameter `Tn` abhängt, hat `Tn` eine Abhängigkeit von `An`.

* Wenn der Rückgabetyp von `Pn` von einem Methodentypparameter `Tn` abhängt, weist `Tn` eine Abhängigkeit von `An` auf.

* Wenn `An` ein Methoden Zeiger ist und der Typ von `Pn` ein konstruierter Delegattyp ist,

* Wenn der Rückgabetyp von `Pn` von einem Methodentypparameter `Tn` abhängt, weist `Tn` eine Abhängigkeit von `An` auf.

* Wenn `Pn` ein konstruierter Typ ist und der Typ von `Pn` von einem Methodentypparameter `Tn` abhängt, hat `Tn` eine Abhängigkeit von `An`.

* Andernfalls wird keine Abhängigkeit generiert.

Nach dem Sammeln von Abhängigkeiten werden alle Argumente, die keine Abhängigkeiten aufweisen, entfernt. Wenn ein Methodentypparameter keine ausgehenden Abhängigkeiten hat (d. h., der Methodentypparameter ist nicht von einem Argument abhängig), schlägt der Typrückschluss fehl. Andernfalls werden die übrigen Argumente und Methodentypparameter in stark verbundene Komponenten gruppiert. Bei einer stark verbundenen Komponente handelt es sich um einen Satz von Argumenten und Methoden Typen Parametern, bei denen ein beliebiges Element in der Komponente über Abhängigkeiten von anderen Elementen erreichbar ist.

Die stark verbundenen Komponenten werden dann in topologischer Reihenfolge topologisch sortiert und verarbeitet:

* Wenn die stark typisierte Komponente nur ein Element enthält,

  * Wenn das Element bereits als abgeschlossen markiert wurde, überspringen Sie es.

  * Wenn das Element ein Argument ist, fügen Sie dem Methodentyp Parameter, die von ihm abhängen, Typhinweise aus dem-Argument hinzu, und markieren Sie das Element als "Complete". Wenn es sich bei dem Argument um eine Lambda-Methode mit Parametern handelt, die noch abherzuwerende Typen benötigen, wird für die Typen dieser Parameter `Object` abgeleitet.

  * Wenn es sich bei dem Element um einen Methodentypparameter handelt, leiten Sie den Methodentypparameter ab, damit er der bestimmende Typ der Argumenttyp Hinweise ist, und markieren Sie das Element als Complete. Wenn für einen Typhinweis eine Array Element Einschränkung festgestellt wird, werden nur Konvertierungen berücksichtigt, die zwischen Arrays des angegebenen Typs gültig sind (d. h. kovariante und systeminterne Array Konvertierungen). Wenn ein Typhinweis eine generische Argument Einschränkung aufweist, werden nur Identitäts Konvertierungen berücksichtigt. Wenn kein dominanter Typ ausgewählt werden kann, schlägt das ableiten fehl. Wenn Lambda-Methoden Argument Typen von diesem Methodentypparameter abhängen, wird der Typ an die Lambda-Methode weitergegeben.

* Wenn die stark typisierte Komponente mehr als ein Element enthält, enthält die Komponente einen Zyklen.

  * Konvertieren Sie für jeden Methodentypparameter, bei dem es sich um ein Element in der Komponente handelt, wenn der Methodentypparameter von einem Argument abhängt, das nicht als abgeschlossen markiert ist, diese Abhängigkeit in eine-Bestätigung, die am Ende des Rückschluss Prozesses geprüft wird.

  * Starten Sie den Rückschluss Prozess an dem Punkt, an dem die stark typisierten Komponenten ermittelt wurden.

Wenn der Typrückschluss für alle Methodentypparameter erfolgreich ist, werden alle Abhängigkeiten, die in Assertionen geändert wurden, geprüft. Eine-Übersetzung ist erfolgreich, wenn der Typ des Arguments implizit in den abzurufenden Typ des methodentypparameters konvertiert werden kann. Wenn eine-Übersetzung fehlschlägt, schlägt das Typargument Rückschluss fehl.

Bei Angabe eines Argument Typs `Ta` für ein Argument `A` und einem Parametertyp `Tp` für einen Parameter `P` werden Typhinweise wie folgt generiert:

* Wenn `Tp` keine Methodentypparameter umfasst, werden keine Hinweise generiert.

* Wenn `Tp` und `Ta` Array Typen desselben Rang sind, ersetzen Sie `Ta` und `Tp` durch die Elementtypen `Ta` und `Tp`, und starten Sie diesen Prozess mit einer Array Element Einschränkung neu.

* Wenn `Tp` ein Methodentypparameter ist, wird `Ta` als typanhinweis mit der aktuellen Einschränkung (sofern vorhanden) hinzugefügt.

* Wenn `A` eine Lambda-Methode und `Tp` ein konstruierter Delegattyp oder `System.Linq.Expressions.Expression(Of T)` ist, wobei `T` ein konstruierter Delegattyp ist, ersetzen Sie für jeden Lambda-Methoden Parametertyp `TL` und den entsprechenden delegatparametertyp `TD` `Ta` durch @no__ t-7 und `Tp` mit `TD` und starten Sie den Prozess ohne Einschränkung neu. Ersetzen Sie dann `Ta` durch den Rückgabetyp der Lambda-Methode und:

  * Wenn `A` eine reguläre Lambda-Methode ist, ersetzen Sie `Tp` durch den Rückgabetyp des Delegattyps.
  * Wenn `A` eine Async-Lambda-Methode und der Rückgabetyp des Delegattyps die Form `Task(Of T)` für einige `T` ist, ersetzen Sie `Tp` durch diese `T`;
  * Wenn `A` eine iteratorlambda-Methode und der Rückgabetyp des Delegattyps die Form `IEnumerator(Of T)` oder `IEnumerable(Of T)` für einige `T` ist, ersetzen Sie `Tp` durch diese `T`.
  * Starten Sie als nächstes den Prozess ohne Einschränkung neu.

* Wenn `A` ein Methoden Zeiger und `Tp` ein konstruierter Delegattyp ist, verwenden Sie die Parametertypen von `Tp`, um zu bestimmen, welche Methode auf `Tp` am meisten anwendbar ist. Wenn eine Methode am meisten anwendbar ist, ersetzen Sie `Ta` durch den Rückgabetyp der Methode und `Tp` durch den Rückgabetyp des Delegattyps, und starten Sie den Prozess ohne Einschränkung neu.

* Andernfalls muss `Tp` ein konstruierter Typ sein. Wenn `TG`, der generische Typ von `Tp`,

  * Wenn `Ta` `TG` ist, von `TG` erbt oder den Typ `TG` genau einmal implementiert, dann ersetzen Sie für jedes übereinstimmende Typargument `Tax` aus `Ta` und `Tpx` von `Tp` `Ta` durch `Tax` und 0 durch 1, und starten Sie den verarbeiten Sie mit einer generischen Argument Einschränkung.

  * Andernfalls schlägt der Typrückschluss für die generische Methode fehl.

Der Erfolg des Typrückschlusses gewährleistet nicht in und von sich, dass die Methode anwendbar ist.
