# <a name="overloaded-method-resolution"></a>Die überladene Methodenauflösung

In der Praxis sind die Regeln zum Bestimmen der Auflösung von funktionsüberladungen vorgesehen, die Überladung gefunden, die "nächstgelegene", um die tatsächlichen Argumente angegeben ist. Ist eine Methode, deren die Argumenttypen entsprechen, ist diese Methode offensichtlich am nächsten. Was ausschließt, dass eine Methode genauer als ein anderes ist, wenn alle die Parametertypen schmaler als (oder gleich) die Parametertypen der anderen Methode. Wenn weder Methodenparameter geringer ist als das andere sind, besteht keine Möglichkeit für zu bestimmen, welche Methode näher auf die Argumente.

__Beachten Sie.__ Auflösung von funktionsüberladungen berücksichtigt nicht den erwarteten Rückgabetyp der Methode. 

Beachten Sie, dass aufgrund der Syntax benannter Parameter, die Reihenfolge des tatsächlichen und formalen Parameter möglicherweise nicht identisch sein.

Wenn eine Methodengruppe, wird die am besten geeignete Methode in der Gruppe für eine Argumentliste bestimmt mithilfe der folgenden Schritte. Wenn Sie nach dem Anwenden einer bestimmten Stufe, keine Elemente in der Gruppe bleiben, tritt ein Fehler während der Kompilierung. Wenn nur ein Element im Satz bleibt, ist dieser Member die am besten geeignete Member. Die Schritte sind:

1.  Wenn keine Typargumente angegeben wurden, gelten Sie Typrückschluss zunächst für alle Methoden, die über Typparameter verfügen. Wenn ein Typrückschluss für eine Methode erfolgreich ist, werden die abgeleiteten Typargumente für die jeweilige Methode verwendet. Wenn Typrückschluss für eine Methode ein Fehler auftritt, wird diese Methode aus der Gruppe gelöscht.

2.  Eliminieren Sie als Nächstes alle Member aus der Gruppe, die nicht zugegriffen werden kann oder nicht anwendbar sind (Abschnitt [Anwendbarkeit auf Argumentliste](overload-resolution.md#applicability-to-argument-list)) an die Argumentliste

3.  Weiter, wenn eine oder mehrere Argumente sind `AddressOf` oder Lambda-Ausdrücke, berechnen Sie dann die *delegieren die Lockerung Ebenen* für jedes Argument wie unten gezeigt. Wenn die schlechteste (niedrigste Priorität) die Lockerung Ebene Delegieren `N` schlechter ist als der untersten Ebene der Delegat die Lockerung in `M`, beseitigen Sie `N` aus dem Satz. Die Delegat die Lockerung Ebenen lauten wie folgt aus:

    31. *Delegat "Error"-Ebene Lockerung* : Wenn die `AddressOf` oder Lambda kann nicht in den Delegattyp konvertiert werden.
  
    32. *Einschränkende delegatlockerung Rückgabetyp oder Parameter* : Wenn das Argument ist `AddressOf` oder ein Lambda-Ausdruck, mit dem deklarierten Typ und die Konvertierung von ihren Rückgabetyp an den Delegaten zurückgeben Typ einschränkend; oder wenn das Argument mit einem regulären Lambda-Ausdruck ist und die Konvertierung von einem der Rückgabeausdrücke des Objekts an den Delegaten zurück, Typ führt zu einer Einschränkung, oder wenn das Argument eines Async-Lambdas und der Delegat, der Rückgabewert ist `Task(Of T)` und die Konvertierung von einem der Rückgabeausdrücke des Objekts zu `T` einschränkend; oder, wenn Das Argument ist ein Iterator-Lambda und den Rückgabetyp des Delegaten `IEnumerator(Of T)` oder `IEnumerable(Of T)` und die Konvertierung aus der "yield"-Operanden durch, um `T` einschränkend.

    33. *Erweiternde delegatlockerung ohne Signatur Delegieren* : Wenn ein Delegattyp ist `System.Delegate` oder `System.MultiCastDelegate` oder `System.Object`.

    34. *Drop zurückgegeben werden sollen oder Argumente delegatlockerung* : Wenn das Argument ist `AddressOf` oder ein Lambda-Ausdrucks mit Rückgabetyp deklariert und der Delegattyp verfügt nicht über einen Rückgabetyp; oder wenn das Argument ein Lambda-Ausdruck mit einem ist oder mehr, Ausdrücke und Typ des Delegaten zurückgeben verfügt nicht über einen Rückgabetyp; oder wenn das Argument ist `AddressOf` oder Lambda ohne Parameter und der Typ des Delegaten über Parameter verfügt.

    35. *Erweiternde delegatlockerung des Rückgabetyps* : Wenn das Argument ist `AddressOf` oder einen Lambda-Ausdruck mit einem Rückgabetyp deklariert, und es ist eine erweiternde Konvertierung von der Rückgabetyp des Delegaten; oder wenn das Argument mit einem regulären Lambda-Ausdruck ist, in dem die Konvertierung vom alle Rückgabeausdrücke Rückgabetyp des Delegaten wird erweiternd oder Identität mit mindestens einer zu erweiternden; oder wenn das Argument ein asynchrones Lambda ist und der Delegat `Task(Of T)` oder `Task` und die Konvertierung von alle Rückgabeausdrücke zu `T` / `Object` erweiternd oder Identität mit mindestens einer zu erweiternden; ist oder wenn Das Argument ist ein Iterator-Lambda, und der Delegat `IEnumerator(Of T)` oder `IEnumerable(Of T)` oder `IEnumerator` oder `IEnumerable` und die Konvertierung von alle Rückgabeausdrücke zu `T` / `Object` ist erweiternd oder die Identität mit mindestens einer zu erweiternden.

    36. *Identität delegatlockerung* : Wenn das Argument ist ein `AddressOf` oder ein Lambda-Ausdruck die Delegaten genau ohne entspricht erweiternde oder einschränkende oder Löschen von Parametern oder gibt zurück oder -Werten ergibt. Als Nächstes einige Elemente der Menge ist nicht erforderlich, einschränkende Konvertierungen auf eines der Argumente angewendet werden, und beseitigen alle Elemente, die dann. Zum Beispiel:

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

4.  Als Nächstes wird die Eliminierung von Duplikaten basierend auf der wie folgt einschränken durchgeführt. (Beachten Sie, dass, wenn Option Strict On ist, klicken Sie dann alle Elemente, die erfordern einschränkende bereits passende gehalten werden (Abschnitt [Anwendbarkeit auf Argumentliste](overload-resolution.md#applicability-to-argument-list)) und Schritt2 entfernt.)

    41. Wenn nur eine Instanz-Elemente der Menge einschränkende Konvertierungen erfordern, in dem der Ausdruck Argumenttyp ist `Object`, beseitigen Sie alle anderen Elemente.
    42. Wenn der Satz mehr als ein Element enthält, müssen Sie nur über einschränkende `Object`, wie eine spät gebundene Methode klicken Sie dann auf der Ziel-Aufrufausdruck neu klassifiziert (und ein Fehler wird ausgegeben, wenn der Typ, der die Methodengruppe enthält eine Schnittstelle ist, oder wenn eines der die entsprechenden Member wurden Erweiterungsmember).
    43. Wenn es Kandidaten für alle, die nur erfordern Einschränken von numerischen Literalen, ausgewählt haben dann die spezifischste für alle verbliebenen Kandidaten durch die folgenden Schritte aus. Wenn der Gewinner erforderlich sind, nur Einschränken von numerischen Literalen, wird es als Ergebnis der Auflösung von funktionsüberladungen ausgewählt; Andernfalls handelt es sich um einen Fehler.

    __Beachten Sie.__ Die Begründung für diese Regel ist dies, die aus, wenn ein Programm lose typisierten ist (d. h. als die meisten oder alle Variablen deklariert werden `Object`), Auflösung von funktionsüberladungen kann schwierig sein, wenn viele Konvertierungen von `Object` einschränkende sind. Anstatt der überladungsauflösung, die in vielen Situationen (starke Typisierung der Argumente für Aufruf der Methode erforderlich ist), Auflösung fehl, die die entsprechende überladen wird die aufzurufende Methode bis zur Laufzeit verzögert. Dadurch wird den Aufruf lose typisierten ohne zusätzliche Umwandlungen erfolgreich. Ein unglücklicher Nebeneffekt, allerdings ist diese ausführen, die den spät gebundenen Aufruf erfordert das Aufrufziel zum Umwandeln `Object`. Im Fall einer Struktur-Wert bedeutet dies, dass der Wert in ein temporäres geschachtelt werden muss. Wenn die letztlich aufgerufene Methode versucht, ein Feld in der Struktur zu ändern, werden diese Änderung verloren, sobald die Methode zurückgibt. Schnittstellen werden von dieser speziellen Regel ausgeschlossen, da es sich bei späte Bindung immer mit den Membern der Klasse oder Struktur Laufzeittyp, löst die andere Namen als den Membern der Schnittstellen haben können, die sie implementieren.

5.  Als Nächstes Wenn Instanzenmethoden in der Menge der keine einschränkende erforderlich sind verbleiben, beseitigen Sie alle Erweiterungsmethoden aus dem Satz. Zum Beispiel:

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

    __Beachten Sie.__ Erweiterungsmethoden werden ignoriert, wenn entsprechende Instanz-Methoden, um sicherzustellen, dass das Hinzufügen eines Imports (die neuen Erweiterungsmethoden in den Gültigkeitsbereich einzubinden möglicherweise) keinen Aufruf auf eine vorhandene Instanzmethode einer Erweiterungsmethode erneut binden bewirken vorhanden sind. Wenn den umfassenden Rahmen einige Erweiterungsmethoden (d. h. auf Schnittstellen und/oder Typparameter definiert), ist dies ein sicherer Ansatz für die Bindung an die Erweiterungsmethoden.

6.  Weiter, If, erhalten alle zwei Elemente der Menge `M` und `N`, `M` mehr *bestimmte* (Abschnitt [Detailgenauigkeit der Member/Typen, die eine Argumentliste](overload-resolution.md#specificity-of-memberstypes-given-an-argument-list)) als `N`angesichts die Argumentliste, beseitigen `N` aus dem Satz. Wenn mehr als ein Element im Satz bleibt die übrigen Elemente nicht gleichmäßig spezifisch erhält die Argumentliste sind und führt ein Fehler während der Kompilierung.

7.  Andernfalls erhält zwei Elemente der Menge, `M` und `N`, gelten die folgenden gleichwertigen geringfügiger-Regeln, in der Reihenfolge:

    71. Wenn `M` verfügt nicht über einen ParamArray-Parameter, aber `N` der Fall ist, oder wenn beides aber `M` übergibt weniger Argumente an den ParamArray-Parameter als `N` der Fall ist, klicken Sie dann zu beseitigen `N` aus dem Satz. Zum Beispiel:

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

        Das obige Beispiel erzeugt die folgende Ausgabe:

        ```
        F(Object, Object())
        F(Object, Object, Object())
        F(Object, Object, Object())
        G(Object)
        ```

        __Beachten Sie.__ Wenn eine Klasse eine Methode mit einem Paramarray-Parameter deklariert, ist es nicht ungewöhnlich, dass auch einige der erweiterten Formen als reguläre Methoden eingeschlossen werden sollen. -Instanz, die bei der erweiterten Zustand einer Methode mit einem Paramarray-Parameter tritt auf, wird aufgerufen, wodurch es möglich ist, vermeiden Sie die Zuordnung eines Arrays.

    72. Wenn `M` wird definiert, in einem stärker abgeleiteten Typ als `N`, beseitigen `N` aus dem Satz. Zum Beispiel:

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

        Diese Regel gilt auch, Typen, denen für die Erweiterungsmethoden definiert sind. Zum Beispiel:

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

    73. Wenn `M` und `N` sind Erweiterungsmethoden und der Zieltyp des `M` ist eine Klasse oder Struktur und der Zieltyp des `N` ist eine Schnittstelle, beseitigen `N` aus dem Satz. Zum Beispiel:

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

    74. Wenn `M` und `N` sind Erweiterungsmethoden und der Zieltyp des `M` und `N` identisch sind, nach der Ersetzung von Typparametern und der Zieltyp des `M` vor Typ parameterersetzung enthält nicht Geben Sie Parameter, aber der Zieltyp des `N` ist, und verfügt über weniger Typparameter als der Zieltyp des `N`, beseitigen `N` aus dem Satz. Zum Beispiel:

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

    75. Vor dem Typ Argumente haben wurde ersetzt, wenn `M` ist *weniger generische* (Abschnitt [Generizität](overload-resolution.md#genericity)) als `N`, beseitigen `N` aus dem Satz.

    76. Wenn `M` ist keine Erweiterungsmethode und `N` ist, beseitigen `N` aus dem Satz.

    77. Wenn `M` und `N` sind Erweiterungsmethoden und `M` wurde gefunden, bevor Sie `N` (Abschnitt [-Methode der Erweiterungsauflistung](expressions.md#extension-method-collection)), zu beseitigen `N` aus dem Satz. Zum Beispiel:

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

        Wenn die Erweiterungsmethoden in demselben Schritt gefunden wurden, sind diese Erweiterungsmethoden nicht eindeutig. Der Aufruf kann immer eindeutig gemacht werden, mit dem Namen des standard-Moduls, enthält die Erweiterungsmethode und rufen Sie die Erweiterungsmethode, als wäre es ein reguläres Element. Zum Beispiel:

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

    78. Wenn `M` und `N` sowohl der Typrückschluss zum Erzeugen von Typargumenten, erforderlich und `M` war nicht erforderlich, Bestimmen des bestimmenden Typs eines seiner Typargumente an (d. h. jeweils die Typargumente für einen einzelnen abgeleitet), aber `N`, beseitigen `N` aus dem Satz.

        __Beachten Sie.__ Diese Regel stellt sicher, Auflösung von funktionsüberladungen, die in früheren Versionen war erfolgreich (wobei das Ableiten von mehreren Typen für ein Typargument einen Fehler verursachen würden), werden weiterhin die gleichen Ergebnisse zu erzielen.

    79. Wenn das Ziel eines eines Ausdrucks von der Behebung überladungsauflösung erfolgt eine `AddressOf` Ausdruck und sowohl der Delegat, und `M` sind Funktionen, die beim `N` ist eine Unterroutine namens beseitigen `N` aus dem Satz. Ebenso, wenn sowohl der Delegat, und `M` -Unterroutinen, sind während `N` ist eine Funktion, beseitigen `N` aus dem Satz.

    710. Wenn `M` wurde keine Standardwerte optionaler Parameter anstelle der expliziten Argumente verwendet, aber `N` , beseitigen Sie `N` aus dem Satz.

    711. Vor dem Typ Argumente haben wurde ersetzt, wenn `M` hat *detaillierter der generizität* (Abschnitt [Generizität](overload-resolution.md#genericity)) als `N`, beseitigen Sie `N` aus dem Satz.

8. Andernfalls der Aufruf ist mehrdeutig, und ein Fehler während der Kompilierung auftritt.

#### <a name="specificity-of-memberstypes-given-an-argument-list"></a>Spezifischer Member/Typen, die eine Argumentliste

Ein Member `M` gilt *gleichermaßen bestimmte* als `N`, eine Argumentliste `A`, wenn deren Signaturen identisch sind oder geben Sie jeden Parameter in `M` ist identisch mit den entsprechenden Parametertyp in `N`.

__Beachten Sie.__ Zwei Member können in Methodengruppe mit der gleichen Signatur aufgrund von Erweiterungsmethoden enden. Zwei Member können auch gleichermaßen spezifisch sein, jedoch nicht über die gleiche Signatur aufgrund von Typparameter oder Paramarray-Erweiterung.

Ein Member `M` gilt *spezifischere* als `N` Wenn ihre Signaturen unterscheiden, und geben Sie mindestens einen Parameter in `M` sind präziser als ein Parametertyp in `N`, und es wird kein Parametertyp in `N` sind präziser als ein Parametertyp in `M`. Erhalten ein Paar von Parametern `Mj` und `Nj` , die ein Argument entspricht `Aj`, den Typ des `Mj` gilt *spezifischere* als den Typ des `Nj` Wenn eines der folgenden Bedingungen erfüllt ist:

* Es existiert eine erweiternde Konvertierung vom Typ des `Mj` in den Typ `Nj`. (__Beachten.__ Da Parametertypen in diesem Fall ohne Berücksichtigung der tatsächlichen Arguments verglichen werden, wird die erweiternde Konvertierung von Konstante Ausdrücke in einen numerischen Datentyp, der in der Wert passt, nicht in diesem Fall berücksichtigt.)

* `Aj` besteht aus dem Literal `0`, `Mj` ist ein numerischer Typ und `Nj` ist ein enumerierter Typ. (__Beachten.__ Diese Regel ist erforderlich, da das Literal `0` auf einen beliebigen enumerierten Typ erweitert wird. Da ein enumerierter Typ, in dem zugrunde liegenden Typ erweitert wird, bedeutet dies, dass der überladungsauflösung für `0` wird standardmäßig vorziehen Enumerationstypen numerische Typen. Wir erhalten viel Feedback, dass dieses Verhalten nicht intuitiv ist.)

* `Mj` und `Nj` sind sowohl numerischen Typen und `Mj` ist älter als `Nj` in der Liste `Byte`, `SByte`, `Short`, `UShort`, `Integer`, `UInteger`, `Long`, `ULong`, `Decimal`, `Single`, `Double`. (__Beachten.__ Die Regel zu numerischen Typen ist hilfreich, da nur die numerischen Typen mit und ohne Vorzeichen einer bestimmten Größe haben einschränkende Konvertierungen zwischen ihnen. Die oben genannten Regel unterbricht die Verbindung zwischen den beiden Typen durch weitere "natürliche" numerischen Typs. Dies ist besonders wichtig bei der überladungsauflösung für einen Typ ausführen, die in mit und ohne Vorzeichen numerischen Typ der einer bestimmten Größe, z. B. ein numerisches Literal erweitert wird, das sowohl geeignet ist.)

* `Mj` und `Nj` sind Delegaten, Funktionstypen und den Rückgabetyp der `Mj` spezifischer als der Rückgabetyp der `Nj` Wenn `Aj` wird als Lambda-Methode, klassifiziert und `Mj` oder `Nj` ist `System.Linq.Expressions.Expression(Of T)`, klicken Sie dann Das Typargument des Typs (vorausgesetzt, dass es sich um einen Delegattyp handelt) wird für den verglichenen Typ ersetzt.

* `Mj` ist identisch mit den Typ des `Aj`, und `Nj` nicht. (__Beachten.__ Es ist interessant, beachten Sie, dass die vorherige Regel unterscheidet sich geringfügig vom C# -Code in diesem C# -Code erfordert, dass es sich bei Delegattypen-Funktion verwenden dieselben Parameterlisten vor dem Vergleich von Rückgabetypen, Visual Basic jedoch nicht.)

#### <a name="genericity"></a>Generizität

Ein Member `M` wird als bestimmt *weniger generische* als Mitglied `N` wie folgt:

1. IF, für jedes Paar übereinstimmender Parameter `Mj` und `Nj`, `Mj` ist kleiner als oder gleich als generische `Nj` in Bezug auf die Typparameter für die Methode, und mindestens eine `Mj` ist in Bezug auf Geben Sie kleiner generisch die Parameter der Methode.
2. Andernfalls gilt: Wenn für jedes Paar übereinstimmender Parameter `Mj` und `Nj`, `Mj` ist kleiner als oder gleich als generische `Nj` in Bezug auf die Typparameter für den Typ und mindestens eine `Mj` ist in Bezug auf Geben Sie kleiner generisch Parameter für den Typ dann `M` ist kleiner als generisch `N`.

Ein Parameter `M` gilt gleichermaßen für einen Parameter generisch `N` Wenn ihre Typen `Mt` und `Nt` beide Typparameter verweisen oder beide Typparameter verweisen nicht. `M` gilt als weniger generische `N` Wenn `Mt` verweist nicht auf einen Typparameter und `Nt` ist.

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

Typparameter Erweiterung-Methode an, die während der currying behoben wurden, werden als Typparameter den Typ, der keine Typparameter der Methode betrachtet. Zum Beispiel:

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

#### <a name="depth-of-genericity"></a>Tiefe der generizität

Ein Member `M` wird damit bestimmt *detaillierter der generizität* als Mitglied `N` If, für jedes Paar übereinstimmender Parameter `Mj` und `Nj`, `Mj` ist größer oder gleich *Tiefe der generizität* als `Nj`, und mindestens eine `Mj` wird detaillierter der generizität ist. Tiefe der generizität ist wie folgt definiert:

* Etwas anderes als einen Typparameter verfügt über mehr Tiefe der generizität als einen Typparameter;

* Rekursiv, verfügt über ein konstruierter Typ detaillierter der generizität als eine andere konstruierten Typ (mit derselben Anzahl von Typargumenten) Wenn mindestens ein Typargument detaillierter der generizität und kein Argument vom Typ weniger Tiefe als der entsprechende Typ hat Argument in der anderen.

* Array-Typ hat detaillierter der generizität als einen anderen Arraytyp (mit derselben Anzahl von Dimensionen) aus, wenn der Elementtyp des ersten detaillierter der generizität als Typ des Elements der zweiten ist.

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

### <a name="applicability-to-argument-list"></a>Anwendbarkeit auf Argumentliste

Eine Methode ist *anwendbar* auf einen Satz von Typargumenten, positionelle Argumente und benannte Argumente, wenn die Methode aufgerufen werden kann, die Argumentlisten verwenden. Das Argument, die Listen der-Parameter verglichen werden, sind wie folgt aufgeführt:

1. Erstens entsprechen Sie jedes positionelle Argument in der Reihenfolge der Liste der Parameter der Methode. Wenn weitere positionelle Argumente als Parameter vorhanden sind, und der letzte Parameter ist nicht mit einem Paramarray, ist die Methode nicht anwendbar. Andernfalls wird der Paramarray-Parameter mit Parametern vom Typ Paramarray-Elements mit der Anzahl von positionellen Argumenten übereinstimmen erweitert. Wenn ein positionelles Argument weggelassen wird, die in einem Paramarray wechseln würde, die Methode ist nicht anwendbar.
2. Als Nächstes entsprechen Sie jedes benanntes Argument auf einen Parameter mit dem angegebenen Namen. Wenn eines der benannten Argumente findet keine Übereinstimmung, einen Paramarray-Parameter entspricht oder ein Argument, das bereits mit einer anderen mit Feldern fester Breite oder benannten Argument abgeglichen entspricht, ist die Methode nicht anwendbar.
3. Wenn die Typargumente angegeben wurden, werden sie als Nächstes mit der Typparameterliste abgeglichen. Wenn die beiden Listen nicht die gleiche Anzahl von Elementen verfügen, ist die Methode nicht anwendbar, wenn die Liste der Typargumente leer ist. Wenn die Liste der Typargumente leer ist, wird Typrückschluss verwendet, um zu testen und die Liste der Typargumente abzuleiten. Wenn der Typrückschluss schlägt fehl, ist die Methode nicht anwendbar. Andernfalls werden die Typargumente anstelle der Typparameter in der Signatur gefüllt. Wenn der Parameter, die nicht abgeglichen wurden nicht optional sind, ist die Methode nicht anwendbar.
4. Wenn die Argumentausdrücke nicht implizit in den Typen der Parameter sind sie übereinstimmen, und klicken Sie dann die Methode ist nicht anwendbar.
5. Wenn ein Parameter ByRef ist und keine implizite Konvertierung vom Typ des Parameters in den Typ des Arguments steht, ist die Methode nicht anwendbar.
6. Wenn die Typargumente (einschließlich der abgeleiteten Typargumenten aus Schritt 3) der Methode-Einschränkungen verletzen, ist die Methode nicht anwendbar. Zum Beispiel:

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

Wenn ein einzelnes Argumentausdruck mit einen Paramarray-Parameter übereinstimmt, und der Typ des Argumentausdrucks sowohl den Typ des Paramarray-Parameter und der Elementtyp Paramarray konvertierbar ist, gilt die Methode in der erweiterten und nicht erweiterten Formen, mit zwei Ausnahmen. Wenn die Konvertierung vom Typ des Argumentausdrucks in den ParamArraytyp einschränkend ist, ist die Methode nur anwendbar, in erweiterter Form. Expression-Arguments überschreitet `Nothing`, und klicken Sie dann die Methode nur anwendbar, in der nicht erweiterte Form ist. Zum Beispiel:

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

Das obige Beispiel erzeugt die folgende Ausgabe:

```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

In der ersten und letzten Aufrufen von `F`, die normale Form dar `F` ist anwendbar, da eine erweiternde Konvertierung des Argumenttyps in den Parametertyp vorhanden ist (beide sind vom Typ `Object()`), und das Argument wird als regulärer Wert übergeben. der Parameter. In der zweiten und dritten aufrufen, die normale Form von `F` ist nicht anwendbar, da keine widening-Konvertierung des Argumenttyps in den Parametertyp vorhanden ist (Konvertierungen von `Object` zu `Object()` einschränkende sind). Allerdings die erweiterte Form von `F` gilt, und eine von einem Element `Object()` wird durch den Aufruf erstellt. Das einzige Element des Arrays mit dem angegebenen Argumentwert initialisiert wird (die wiederum ist ein Verweis auf ein `Object()`).

### <a name="passing-arguments-and-picking-arguments-for-optional-parameters"></a>Übergeben von Argumenten, und wählen die Argumente für optionale Parameter

Wenn ein Parameter einen Werteparameter ist, muss der entsprechende Argumentausdruck als Wert klassifiziert werden. Der Wert wird in den Typ des Parameters konvertiert und als Parameter zur Laufzeit übergeben. Wenn der Parameter ein Verweisparameter ist, und der entsprechenden Argumentausdruck wird als eine Variable, deren Typ der Parameter entspricht, klassifiziert, wird Sie dann ein Verweis auf die Variable in als Parameter zur Laufzeit übergeben.

Andernfalls, wenn der entsprechende Argumentausdruck als eine Variable, Wert oder der Zugriff auf Eigenschaften klassifiziert wird, wird eine temporäre Variable des Typs des Parameters zugeordnet. Vor dem Aufruf der Methode zur Laufzeit wird der Argumentausdruck als Wert neu klassifiziert, in den Typ des Parameters konvertiert und der temporären Variablen zugewiesen. Dann wird ein Verweis auf die temporäre Variable in als Parameter übergeben. Nach dem Aufruf der Methode ausgewertet wird, wenn der Argumentausdruck als eine Variable oder Eigenschaftszugriff klassifiziert wird, wird der Variablen Ausdruck oder der Eigenschaftsausdruck für den Zugriff der temporären Variablen zugewiesen. Wenn der Eigenschaftsausdruck für den Zugriff kein `Set` -Accessor, und klicken Sie dann auf die Zuordnung wird nicht ausgeführt.

Für optionale Parameter, in denen ein Argument nicht bereitgestellt wurde, wählt der Compiler Argumente an, wie unten beschrieben. In allen Fällen testet es nach dem Ersetzen des generischen Typs mit dem Parametertyp.

* Wenn der optionale Parameter das Attribut hat `System.Runtime.CompilerServices.CallerLineNumber`, und der Aufruf ist von einem Ort im Quellcode und ein numerisches Literal, diesen Speicherort die Zeilennummer darstellt, wurde eine Konvertierung in den Parametertyp, dann wird die numerische Literale verwendet. Wenn der Aufruf mehrere Zeilen erstreckt, ist die Auswahl der zu verwendende der Linie implementierungsabhängig.

* Wenn der optionale Parameter das Attribut hat `System.Runtime.CompilerServices.CallerFilePath`, und der Aufruf ist von einem Ort im Quellcode, und ein Zeichenfolgenliteral, des Speicherorts Dateipfad darstellt, ist eine Konvertierung in den Parametertyp, dann wird das Zeichenfolgenliteral verwendet. Das Format des Dateipfads ist implementierungsabhängig.

* Wenn der optionale Parameter das Attribut hat `System.Runtime.CompilerServices.CallerMemberName`, und der Aufruf ist innerhalb des Texts eines Typmembers oder in einem Attribut auf einen beliebigen Teil dieses Typmember angewendet und ein Zeichenfolgenliteral darstellt, Elementnamen hat eine Konvertierung für den Parameter Geben Sie ein, und klicken Sie dann das Zeichenfolgenliteral verwendet wird. Für Aufrufe, die Teil von Eigenschaftenaccessoren oder benutzerdefinierten Ereignishandlern sind, ist der Elementname verwendet, die der Eigenschaft oder Ereignis selbst. Aufrufe, die Teil eines Operator oder den Konstruktor, wird eine implementierungsspezifische Name verwendet.

Wenn keine der oben genannten Bedingungen zutrifft, klicken Sie dann den optionalen Parameter Standardwert wird verwendet (oder `Nothing` , wenn kein Standardwert angegeben ist). Und wenn mehr als eine der oben genannten Bedingungen zutrifft, und klicken Sie dann die Wahl, welches ist implementierungsabhängig.

Die `CallerLineNumber` und `CallerFilePath` Attribute eignen sich für die Protokollierung. Die `CallerMemberName` eignet sich für die Implementierung `INotifyPropertyChanged`. Hier sind Beispiele.

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

Zusätzlich zu den oben aufgeführten optionalen Parameter erkennt Microsoft Visual Basic auch einige zusätzliche optionale Parameter, wenn sie aus den Metadaten (z. B. aus einem DLL-Verweis) importiert werden:

* Beim Importieren von Metadaten, die Parameter auch von Visual Basic behandelt `<Optional>` wie darauf hin, dass es sich bei der Parameter optional ist: auf diese Weise es möglich ist, eine Deklaration mit einem optionalen Parameter, aber kein Standardwert zugewiesen wurde, zu importieren, obwohl dies nicht möglich Mithilfe von ausgedrückt die `Optional` Schlüsselwort.

* Wenn der optionale Parameter das Attribut hat `Microsoft.VisualBasic.CompilerServices.OptionCompareAttribute`, die numerische Literale 1 oder 0 ist eine Konvertierung in den Parametertyp, und der Compiler verwendet als Argument entweder dem literalen 1, wenn `Option Compare Text` gültig ist, oder 0, wenn das literal ist `Optional Compare Binary` ist aktiviert.

* Wenn der optionale Parameter das Attribut hat `System.Runtime.CompilerServices.IDispatchConstantAttribute`, und es weist den Typ `Object`, es gibt keinen Standardwert, und der Compiler verwendet das Argument `New System.Runtime.InteropServices.DispatchWrapper(Nothing)`.

* Wenn der optionale Parameter das Attribut hat `System.Runtime.CompilerServices.IUnknownConstantAttribute`, und es weist den Typ `Object`, es gibt keinen Standardwert, und der Compiler verwendet das Argument `New System.Runtime.InteropServices.UnknownWrapper(Nothing)`.

* Wenn der optionale Parameter Typ hat `Object`, es gibt keinen Standardwert, und der Compiler verwendet das Argument `System.Reflection.Missing.Value`.

### <a name="conditional-methods"></a>Bedingte Methoden

Wenn die Zielmethode, die auf den ein Aufrufausdruck verweist eine Unterroutine ist, der kein Member einer Schnittstelle ist, und wenn die Methode eine oder mehrere `System.Diagnostics.ConditionalAttribute` Attribute Auswertung des Ausdrucks hängt von den definiert Konstanten für bedingte Kompilierung Dieser Punkt in der Quelldatei. Jede Instanz des Attributs gibt eine Zeichenfolge, die eine Konstante für bedingte Kompilierung Namen an. Jede Konstante für bedingte Kompilierung wird ausgewertet, als handele es sich um Teil einer bedingten kompilierungsanweisung. Wenn die Konstante ergibt `True`, der Ausdruck wird normalerweise zur Laufzeit ausgewertet. Wenn die Konstante ergibt `False`, der Ausdruck ist überhaupt nicht ausgewertet.

Wenn für das Attribut zu suchen, ist die am stärksten abgeleitete Deklaration eine überschreibbare Methode aktiviert.

__Beachten Sie.__ Das Attribut gilt nicht für Funktionen oder Schnittstellenmethoden und wird ignoriert, wenn für beide Arten der Methode angegeben. Bedingte Methoden werden daher nur in Aufruf Anweisungen angezeigt.

### <a name="type-argument-inference"></a>Typrückschluss-Argument

Wenn eine Methode mit Parametern aufgerufen wird, ohne Angabe von Typargumenten, *Argument Typrückschluss* dient zum Testen und Typargumente für den Aufruf ableiten. Dies ermöglicht eine natürlichere Syntax zum Aufrufen einer Methode mit den beiden Typparametern, wenn die Typargumente Trivial abgeleitet werden können verwendet werden soll. Betrachten Sie z. B. die folgende Methodendeklaration:

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

Es ist möglich, rufen Sie die `Choose` Methode ohne explizite Angabe ein Argument vom Typ:

```vb
' calls Choose(Of Integer)
Dim i As Integer = Util.Choose(True, 5, 213)
' calls Choose(Of String)
Dim s As String = Util.Choose(False, "a", "b") 
```

Mithilfe des Typrückschlusses Argument die Typargumente `Integer` und `String` werden aus den Argumenten der Methode bestimmt.

Typargument Typrückschluss erfolgt, *vor* neuklassifizierung der Ausdruck für Lambdamethoden oder Methodenzeiger in der Argumentliste ausgeführt wird, da eine neuklassifizierung der diese zwei Arten von Ausdrücken den Typ des erfordern möglicherweise die der Parameter nicht bekannt sein.  Bei einem gegebenen Satz von Argumenten `A1,...,An`, eine Reihe übereinstimmender Parameter `P1,...,Pn` und einen Satz von der Methode, die Typparameter `T1,...,Tn`, werden die Abhängigkeiten zwischen den Argumenten und die Typparameter der Methode zunächst wie folgt erfasst:

* Wenn `An` ist die `Nothing` Zeichenliteral ohne Abhängigkeiten generiert werden.

* Wenn `An` ist ein Lambda-Methode und der Typ des `Pn` ist ein Typ des erstellten Delegaten oder `System.Linq.Expressions.Expression(Of T)`, wobei `T` ist ein Typ des erstellten Delegaten,

* Wenn Sie der Typ der Parameter der Lambda-Methode vom Typ des entsprechenden Parameters abgeleitet werden, werden `Pn`, und der Typ des Parameters hängt von Typparameter einer Methode `Tn`, klicken Sie dann `An` weist eine Abhängigkeit `Tn`.

* Wenn der Typ des Lambda-Parameter der Methode angegeben ist und der Typ des entsprechenden Parameters `Pn` hängt von der Typparameter einer Methode `Tn`, klicken Sie dann `Tn` weist eine Abhängigkeit `An`.

* Wenn der Rückgabetyp der `Pn` hängt von der Typparameter einer Methode `Tn`, klicken Sie dann `Tn` weist eine Abhängigkeit `An`.

* Wenn `An` ist ein Methodenzeiger und der Typ des `Pn` ist ein Typ des erstellten Delegaten,

* Wenn der Rückgabetyp der `Pn` hängt von der Typparameter einer Methode `Tn`, klicken Sie dann `Tn` weist eine Abhängigkeit `An`.

* Wenn `Pn` ist ein konstruierter Typ und den Typ des `Pn` hängt von der Typparameter einer Methode `Tn`, klicken Sie dann `Tn` weist eine Abhängigkeit `An`.

* Andernfalls wird keine Abhängigkeit generiert.

Nach dem Sammeln von Abhängigkeiten, werden alle Argumente, die keine Abhängigkeiten haben, entfernt. Wenn alle Typparameter der Methode keine ausgehenden Abhängigkeiten haben (d. h. die Typparameter der Methode ein Argument hängt nicht), schlägt ein Typrückschluss. Andernfalls werden die übrigen Argumente und die Typparameter der Methode zu stark angeschlossenen Komponenten gruppiert. Eine eng verbundene Komponente ist einen Satz von Typargumenten und Typparametern der Methode, wobei jedes Element in der Komponente über Abhängigkeiten von anderen Elementen.

Stark angeschlossenen Komponenten dann topologisch sortiert und in topologischer Reihenfolge verarbeitet:

* Wenn die stark typisierte Komponente nur ein einziges Element enthält,

  * Wenn das Element bereits abgeschlossen markiert wurde, fahren Sie es.

  * Wenn das Element ein Argument ist, fügen Sie TypHinweise aus dem Argument an die Methode Typparameter, die von ihm abhängig und das Element als abgeschlossen zu markieren. Wenn das Argument eine Lambda-Methode mit Parametern, die dennoch müssen abgeleitete Typen, anschließende folgern `Object` für die Typen dieser Parameter.

  * Wenn das Element die Typparameter einer Methode ist, klicken Sie dann die Typparameter der Methode der bestimmende Typ für das Argument TypHinweise und das Element als abgeschlossen markieren abgeleitet werden. Wenn auf ein typhinweis eine Einschränkung der Array-Element ist, gelten nur die Konvertierungen, die zwischen Arrays mit den angegebenen Typ gültig sind (z. B. kovariante und systeminterne arraykonvertierungen). Wenn ein typhinweis eine generisches Argument-Einschränkung aufweist, werden nur die Identity-Konvertierungen berücksichtigt. Typrückschluss schlägt fehl, wenn kein dominanter Typ ausgewählt werden kann. Wenn alle Argumenttypen für Lambda-Methode die Typparameter dieser Methode abhängig sind, wird der Typ der Lambda-Methode weitergegeben.

* Wenn die stark typisierte Komponente mehr als ein Element enthält, enthält die Komponente eine Schleife.

  * Für jeden Typparameter der Methode, die ein Element in der Komponente ist, wenn der Typparameter der Methode ein Argument abhängig ist, die nicht als abgeschlossen markiert ist, konvertieren diese Abhängigkeit in eine Assertion, die am Ende des Rückschlussprozesses von geprüft wird.

  * Starten Sie den Typrückschluss-Prozess, an dem Punkt, an dem die stark typisierten Komponenten ermittelt wurden.

Wenn ein Typrückschluss für alle Typparameter der Methode erfolgreich ist, werden alle Abhängigkeiten, die in den Assertionen geändert wurden überprüft. Eine Assertion ist erfolgreich, wenn der Typ des Arguments implizit in den abgeleiteten Typ, der der Typparameter der Methode. Wenn eine Assertion fehlschlägt, schlägt Typrückschluss-Argument.

Mit einem Argument Typ `Ta` für ein Argument `A` und Parametertyp `Tp` für einen Parameter `P`, TypHinweise typgeprüft werden wie folgt generiert:

* Wenn `Tp` beinhaltet jedoch keine Methodenparameter des Typs keine Hinweise generiert werden.

* Wenn `Tp` und `Ta` werden Arraytypen, der den gleichen Rang, und Ersetzen `Ta` und `Tp` mit den Elementtypen der `Ta` und `Tp` und starten Sie diesen Prozess mit einer Array-Element-Einschränkung.

* Wenn `Tp` ist der Typparameter einer Methode `Ta` als einen typhinweis, mit der aktuellen Einschränkung, hinzugefügt wird, sofern vorhanden.

* Wenn `A` ist eine Lambda-Methode und `Tp` ist ein Typ des erstellten Delegaten oder `System.Linq.Expressions.Expression(Of T)`, wobei `T` ist ein erstellten Delegaten-Typ, für jeden Typ von Lambda-Methode Parameter `TL` und die entsprechenden Parameter Delegattyp`TD`, ersetzen Sie dies `Ta` mit `TL` und `Tp` mit `TD` und starten Sie den Prozess ohne Einschränkung. Ersetzen Sie `Ta` mit dem Rückgabetyp der Lambda-Methode und:

  * Wenn `A` ist eine reguläre Lambda-Methode ersetzen `Tp` mit dem Rückgabetyp des Delegattyps;
  * Wenn `A` ist eine Async-Lambda-Methode und der Rückgabetyp des Delegattyps hat die Form `Task(Of T)` für einige `T`, ersetzen Sie dies `Tp` , `T`;
  * Wenn `A` ist eine Lambda-Iterator-Methode und der Rückgabetyp des Delegattyps hat die Form `IEnumerator(Of T)` oder `IEnumerable(Of T)` für einige `T`, ersetzen Sie dies `Tp` , `T`.
  * Als Nächstes starten Sie den Prozess ohne Einschränkung an.

* Wenn `A` ist ein Methodenzeiger und `Tp` eine erstellte Delegattyp, verwenden Sie die Parametertypen der `Tp` um zu bestimmen, welche Methode gezeigt ist die am ehesten auf `Tp`. Ersetzen Sie bei eine Methode, die am besten geeignete wird `Ta` mit dem Rückgabetyp der Methode und `Tp` mit dem Rückgabetyp des Delegattyps und Neustart des Prozesses ohne Einschränkung.

* Andernfalls `Tp` muss ein konstruierter Typ sein. Erhält `TG`, der generische Typ der `Tp`,

  * Wenn `Ta` ist `TG`, erbt von `TG`, oder den Typ implementiert `TG` genau einmal für jedes übereinstimmende Typargument dann `Tax` aus `Ta` und `Tpx` aus `Tp`, Ersetzen`Ta` mit `Tax` und `Tp` mit `Tpx` und starten Sie den Prozess mit einem generischen Argument-Einschränkung.

  * Andernfalls schlägt den Typrückschluss, für die generische Methode.

Der Erfolg des Typrückschlusses garantiert nicht, in und von sich selbst, dass die Methode gilt.
