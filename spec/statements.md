---
ms.openlocfilehash: e49e116e60e724bcd8f1148c8aad9d11dfc92e74
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306056"
---
# <a name="statements"></a>Anweisungen

-Anweisungen stellen ausführbaren Code dar.

```antlr
Statement
    : LabelDeclarationStatement
    | LocalDeclarationStatement
    | WithStatement
    | SyncLockStatement
    | EventStatement
    | AssignmentStatement
    | InvocationStatement
    | ConditionalStatement
    | LoopStatement
    | ErrorHandlingStatement
    | BranchStatement
    | ArrayHandlingStatement
    | UsingStatement
    | AwaitStatement
    | YieldStatement
    ;
```

__Nebenbei.__ Der Microsoft Visual Basic-Compiler lässt nur Anweisungen zu, die mit einem Schlüsselwort oder einem Bezeichner beginnen. Folglich ist beispielsweise die Aufruf Anweisung "`Call (Console).WriteLine`" zulässig, aber die Aufruf Anweisung "`(Console).WriteLine`" ist nicht.

## <a name="control-flow"></a>Ablaufsteuerung

Die Ablauf *Steuerung* ist die Sequenz, in der-Anweisungen und-Ausdrücke ausgeführt werden. Die Ausführungsreihenfolge hängt von der jeweiligen Anweisung oder dem Ausdruck ab.

Wenn Sie z. b. einen Additions Operator (Section Additions [Operator](expressions.md#addition-operator)) auswerten, wird zuerst der linke Operand ausgewertet, dann der rechte Operand und dann der Operator selbst. -Blöcke werden ausgeführt (Abschnitts [Blöcke und Bezeichnungen](statements.md#blocks-and-labels)), indem Sie zuerst Ihre erste unter Anweisung ausführen und dann nacheinander durch die-Anweisungen des-Blocks fortfahren.

Implizit in dieser Reihenfolge ist das Konzept eines *Steuerungs Punkts*, bei dem es sich um den nächsten auszuführenden Vorgang handelt. Wenn eine Methode aufgerufen wird (oder "aufgerufen"), wird eine *Instanz* der Methode erstellt. Eine Methoden Instanz besteht aus einer eigenen Kopie der Methoden Parameter und lokalen Variablen sowie einem eigenen Kontrollpunkt.

### <a name="regular-methods"></a>Reguläre Methoden

Im folgenden finden Sie ein Beispiel für eine reguläre Methode.

```vb
Function Test() As Integer
    Console.WriteLine("hello")
    Return 1
End Sub

Dim x = Test()    ' invokes the function, prints "hello", assigns 1 to x
```

Wenn eine reguläre Methode aufgerufen wird,

1. Zuerst wird eine Instanz der Methode erstellt, die für diesen Aufruf spezifisch ist. Diese Instanz enthält eine Kopie aller Parameter und lokalen Variablen der Methode.
2. Anschließend werden alle zugehörigen Parameter mit den angegebenen Werten und allen lokalen Variablen für die Standardwerte ihrer Typen initialisiert.
3. Im Fall eines `Function` wird auch eine implizite lokale Variable initialisiert, die als *Funktions Rückgabe Variable* bezeichnet wird, deren Name der Name der Funktion ist, deren Typ der Rückgabetyp der Funktion und dessen ursprünglicher Wert der Standardwert des Typs ist.
4. Der Kontrollpunkt der Methoden Instanz wird dann bei der ersten Anweisung des Methoden Texts festgelegt, und der Methoden Text beginnt sofort mit der Ausführung (Abschnitts [Blöcke und Bezeichnungen](statements.md#blocks-and-labels)).

Wenn die Ablauf Steuerung den Methoden Text normal beendet, indem Sie den `End Function` oder `End Sub` erreicht, der das Ende markiert, oder durch eine explizite `Return`-oder `Exit`-Anweisung, wird die Ablauf Steuerung an den Aufrufer der Methoden Instanz zurückgegeben. Wenn eine Funktions Rückgabe Variable vorhanden ist, ist das Ergebnis des auffalls der endgültige Wert dieser Variablen.

Wenn die Ablauf Steuerung den Methoden Text durch eine nicht behandelte Ausnahme beendet, wird diese Ausnahme an den Aufrufer weitergegeben.

Nachdem die Ablauf Steuerung beendet wurde, gibt es keine Live Verweise mehr auf die Methoden Instanz. Wenn die Methoden Instanz nur Verweise auf Ihre Kopie von lokalen Variablen oder Parametern enthielt, werden Sie möglicherweise in die Garbage Collection aufgenommen.

### <a name="iterator-methods"></a>Iteratormethoden

Iteratormethoden werden als bequeme Methode zum Generieren einer Sequenz verwendet, die von der `For Each`-Anweisung verwendet werden kann. Iteratormethoden verwenden die `Yield`-Anweisung (Section [yield-Anweisung](statements.md#yield-statement)), um Elemente der Sequenz bereitzustellen. (Eine Iteratormethode ohne `Yield`-Anweisungen erzeugt eine leere Sequenz). Im folgenden finden Sie ein Beispiel für eine Iteratormethode:

```vb
Iterator Function Test() As IEnumerable(Of Integer)
    Console.WriteLine("hello")
    Yield 1
    Yield 2
End Function

Dim en = Test()
For Each x In en          ' prints "hello" before the first x
    Console.WriteLine(x)  ' prints "1" and then "2"
Next
```

Wenn eine Iteratormethode aufgerufen wird, deren Rückgabetyp `IEnumerator(Of T)` ist,

1. Zuerst wird eine Instanz der Iteratormethode erstellt, die für diesen Aufruf spezifisch ist. Diese Instanz enthält eine Kopie aller Parameter und lokalen Variablen der Methode.
2. Anschließend werden alle zugehörigen Parameter mit den angegebenen Werten und allen lokalen Variablen für die Standardwerte ihrer Typen initialisiert.
3. Eine implizite lokale Variable wird auch als *iteratoraktuelle Variable*initialisiert, deren Typ `T` und dessen ursprünglicher Wert der Standardwert des Typs ist.
4. Der Kontrollpunkt der Methoden Instanz wird dann bei der ersten Anweisung des Methoden Texts festgelegt.
5. Anschließend wird ein *Iteratorobjekt* erstellt, das dieser Methoden Instanz zugeordnet ist. Das Iteratorobjekt implementiert den deklarierten Rückgabetyp und weist das Verhalten auf, wie unten beschrieben.
6. Die Ablauf Steuerung wird dann *sofort* im Aufrufer fortgesetzt, und das Ergebnis des Aufrufers ist das Iteratorobjekt. Beachten Sie, dass diese Übertragung erfolgt, ohne die iteratormethodeninstanz zu beenden, und führt nicht dazu, dass die Handler schließlich ausgeführt werden. Auf die Methoden Instanz wird weiterhin durch das Iteratorobjekt verwiesen, und es wird keine Garbage Collection durchgeführt, solange ein Live Verweis auf das Iteratorobjekt vorhanden ist.

Beim Zugriff auf die `Current`-Eigenschaft des iteratorobjekts wird die *aktuelle Variable* des aufrubers zurückgegeben.

Wenn die `MoveNext`-Methode des iteratorobjekts aufgerufen wird, erstellt der Aufruf keine neue Methoden Instanz. Stattdessen wird die vorhandene Methoden Instanz (und deren Steuerungspunkt und lokale Variablen und Parameter)-die Instanz verwendet, die beim ersten Aufrufen der Iteratormethode erstellt wurde. Die Ablauf Steuerung nimmt die Ausführung am Steuerungspunkt dieser Methoden Instanz wieder auf und verläuft wie gewohnt durch den Text der Iteratormethode.

Wenn die `Dispose`-Methode des iteratorobjekts aufgerufen wird, wird wieder die vorhandene Methoden Instanz verwendet. Die Ablauf Steuerung wird am Steuerungspunkt dieser Methoden Instanz fortgesetzt, verhält sich jedoch sofort so, als ob eine `Exit Function`-Anweisung der nächste Vorgang wäre.

Die obigen Beschreibungen des Verhaltens des Aufrufs von `MoveNext` oder `Dispose` für ein Iteratorobjekt gelten nur, wenn alle vorherigen Aufrufe von `MoveNext` oder `Dispose` für dieses Iteratorobjekt bereits an ihre Aufrufer zurückgegeben wurden. Wenn dies nicht der Fall ist, ist das Verhalten nicht definiert.

Wenn die Ablauf Steuerung den iteratormethodentext normal beendet, indem die `End Function` erreicht wird, die das Ende oder eine explizite `Return`-oder `Exit`-Anweisung erreicht haben, muss Sie dies im Kontext eines aufhebers der `MoveNext`-oder `Dispose`-Funktion für einen Iterator durchgeführt haben. , um die iteratormethodeninstanz fortzusetzen, und Sie verwendet die-Methoden Instanz, die beim ersten Aufruf der Iteratormethode erstellt wurde. Der Kontrollpunkt dieser Instanz bleibt bei der `End Function`-Anweisung, und die Ablauf Steuerung wird im Aufrufer fortgesetzt. Wenn die Wiederaufnahme durch einen Aufruf von `MoveNext` erfolgt ist, wird der Wert `False` an den Aufrufer zurückgegeben.

Wenn die Ablauf Steuerung den iteratormethodentext über eine nicht behandelte Ausnahme beendet, wird die Ausnahme an den Aufrufer weitergegeben, der wiederum entweder ein Aufruf von `MoveNext` oder `Dispose` ist.

Wie bei den anderen möglichen Rückgabe Typen einer Iteratorfunktion

* Wenn eine Iteratormethode aufgerufen wird, deren Rückgabetyp für einige `T` `IEnumerable(Of T)` ist, wird zuerst eine-Instanz erstellt, die für diesen Aufruf der Iteratormethode gilt, und Sie wird mit den angegebenen Werten initialisiert. Das Ergebnis des aufbilden ist ein Objekt, das den Rückgabetyp implementiert. Wenn die `GetEnumerator`-Methode dieses Objekts aufgerufen wird, erstellt Sie eine-Instanz, die für diesen Aufruf von `GetEnumerator`-mit allen Parametern und lokalen Variablen in der Methode spezifisch ist. Die Parameter werden für die bereits gespeicherten Werte initialisiert, und Sie werden wie für die oben genannte Iteratormethode fortgesetzt.
* Wenn eine Iteratormethode aufgerufen wird, deren Rückgabetyp die nicht generische Schnittstelle `IEnumerator` ist, ist das Verhalten genau so wie bei `IEnumerator(Of Object)`.
* Wenn eine Iteratormethode aufgerufen wird, deren Rückgabetyp die nicht generische Schnittstelle `IEnumerable` ist, ist das Verhalten genau so wie bei `IEnumerable(Of Object)`.

### <a name="async-methods"></a>Asynchrone Methoden

Async-Methoden sind eine bequeme Methode zum Ausführen von Aufgaben mit langer Ausführungszeit, ohne beispielsweise die Benutzeroberfläche einer Anwendung zu blockieren. Async ist für *Asynchronous* kurz. Dies bedeutet, dass der Aufrufer der Async-Methode die Ausführung umgehend fort setzt. der endgültige Abschluss der-Instanz der Async-Methode kann jedoch zu einem späteren Zeitpunkt in der Zukunft auftreten. Gemäß Konvention werden Async-Methoden mit dem Suffix "Async" benannt.

```vb
Async Function TestAsync() As Task(Of String)
    Console.WriteLine("hello")
    Await Task.Delay(100)
    Return "world"
End Function

Dim t = TestAsync()         ' prints "hello"
Console.WriteLine(Await t)  ' prints "world"
```

__Nebenbei.__ Async-Methoden werden *nicht* in einem Hintergrund Thread ausgeführt. Stattdessen ermöglichen Sie es einer Methode, sich selbst durch den `Await`-Operator anzuhalten und zu planen, als Reaktion auf ein Ereignis fortgesetzt zu werden.

Wenn eine Async-Methode aufgerufen wird

1. Zuerst wird eine Instanz der Async-Methode erstellt, die für diesen Aufruf spezifisch ist. Diese Instanz enthält eine Kopie aller Parameter und lokalen Variablen der Methode.
2. Anschließend werden alle zugehörigen Parameter mit den angegebenen Werten und allen lokalen Variablen für die Standardwerte ihrer Typen initialisiert.
3. Bei einer Async-Methode mit dem Rückgabetyp `Task(Of T)` für einige `T` wird auch eine implizite lokale Variable initialisiert, die als *Task Rückgabe Variable*bezeichnet wird, deren Typ `T` und deren ursprünglicher Wert der Standardwert `T` ist.
4. Wenn die Async-Methode eine `Function` mit dem Rückgabetyp `Task` oder `Task(Of T)` für einige `T` ist, wird ein Objekt dieses Typs implizit erstellt, das dem aktuellen Aufruf zugeordnet ist. Dies wird als *Async-Objekt* bezeichnet und stellt die zukünftige Arbeit dar, die ausgeführt wird, indem die Instanz der Async-Methode ausgeführt wird. Wenn das Steuerelement im Aufrufer dieser Async-Methoden Instanz fortgesetzt wird, empfängt der Aufrufer dieses Async-Objekt als Ergebnis des Aufrufens.
5. Der Kontrollpunkt der-Instanz wird dann bei der ersten Anweisung des asynchronen Methoden Texts festgelegt und beginnt sofort mit der Ausführung des Methoden Texts von dort (Abschnitts [Blöcke und Bezeichnungen](statements.md#blocks-and-labels)).

__Fort-und aktueller Aufrufer__

Wie im Abschnitt "section Erwartungs [Operator](expressions.md#await-operator)" ausführlich beschrieben, kann die Ausführung eines `Await`-Ausdrucks den Steuerungspunkt der Methoden Instanz aussetzen, sodass die Ablauf Steuerung an anderer Stelle weitergeleitet wird. Die Ablauf Steuerung kann später auf dem Steuerungspunkt derselben Instanz durch Aufrufen eines- *Wiederaufnahme*Delegaten fortgesetzt werden. Beachten Sie, dass diese Unterbrechung durchgeführt wird, ohne dass die Async-Methode beendet wird, und führt nicht dazu, dass die Handler ausgeführt werden. Auf die Methoden Instanz wird immer noch durch den Wiederaufnahme Delegaten und das Ergebnis `Task` oder `Task(Of T)` verwiesen (sofern vorhanden), und es wird keine Garbage Collection durchgeführt, solange ein Live Verweis auf den Delegaten oder das Ergebnis vorhanden ist.

Es ist hilfreich, sich die-Anweisung `Dim x = Await WorkAsync()` ungefähr als syntaktische Kurzform für Folgendes vorzustellen:

```vb
Dim temp = WorkAsync().GetAwaiter()
If Not temp.IsCompleted Then
       temp.OnCompleted(resumptionDelegate)
       Return [task]
       CONT:   ' invocation of 'resumptionDelegate' will resume here
End If
Dim x = temp.GetResult()
```

Im folgenden Beispiel wird der *aktuelle* Aufrufer der-Methoden Instanz entweder als ursprünglicher Aufrufer oder als Aufrufer des Wiederaufnahme Delegaten definiert, je nachdem, welcher Wert aktueller ist.

Wenn die Ablauf Steuerung den asynchronen Methoden Text beendet, indem die `End Sub` oder `End Function` erreicht wird, die das Ende oder eine explizite `Return`-oder `Exit`-Anweisung oder eine nicht behandelte Ausnahme erreichen, wird der Kontrollpunkt der Instanz auf das Ende der Methode festgelegt. Das Verhalten hängt dann vom Rückgabetyp der Async-Methode ab.

* Im Fall eines `Async Function` mit dem Rückgabetyp `Task`:
  1. Wenn die Ablauf Steuerung durch eine nicht behandelte Ausnahme beendet wird, wird der Status des asynchronen Objekts auf `TaskStatus.Faulted` festgelegt, und die Eigenschaft `Exception.InnerException` wird auf die Ausnahme festgelegt (außer: bestimmte von der Implementierung definierte Ausnahmen wie `OperationCanceledException` ändern Sie in `TaskStatus.Canceled`). Die Ablauf Steuerung wird im aktuellen Aufrufer fortgesetzt.
  2. Andernfalls wird der Status des Async-Objekts auf `TaskStatus.Completed` festgelegt. Die Ablauf Steuerung wird im aktuellen Aufrufer fortgesetzt.

     (__Hinweis:__ Der ganze Punkt der Aufgabe und die Art und Weise, wie asynchrone Methoden interessant werden, besteht darin, dass bei der Fertigstellung einer Aufgabe für alle Methoden, die darauf warten, ihre Wiederverwendungs Delegaten ausgeführt werden, d. h., Sie werden die Blockierung aufgehoben.

* Im Fall eines `Async Function` mit dem Rückgabetyp `Task(Of T)` für einige `T`: das Verhalten ist wie oben angegeben, mit der Ausnahme, dass in nicht Ausnahmefällen die `Result`-Eigenschaft des Async-Objekts auch auf den endgültigen Wert der Task Rückgabe Variablen festgelegt wird.

* Im Fall eines `Async Sub`:
  1. Wenn die Ablauf Steuerung durch eine nicht behandelte Ausnahme beendet wird, wird diese Ausnahme in Implementierungs spezifischer Weise an die Umgebung weitergegeben. Die Ablauf Steuerung wird im aktuellen Aufrufer fortgesetzt.
  2. Andernfalls wird die Ablauf Steuerung einfach im aktuellen Aufrufer fortgesetzt.

#### <a name="async-sub"></a>Async Sub

Es gibt ein Microsoft-spezifisches Verhalten einer `Async Sub`.

Wenn `SynchronizationContext.Current` zu Beginn des aufzurufenden `Nothing` ist, werden alle nicht behandelten Ausnahmen von einem asynchronen Sub an den Thread Pool gesendet.

Wenn `SynchronizationContext.Current` zu Beginn des aufgerufenen nicht `Nothing` ist, wird `OperationStarted()` in diesem Kontext vor dem Anfang der Methode und `OperationCompleted()` nach dem Ende aufgerufen. Außerdem werden alle nicht behandelten Ausnahmen bereitgestellt, damit Sie für den Synchronisierungs Kontext erneut ausgelöst werden.

Dies bedeutet, dass bei UI-Anwendungen für eine `Async Sub`, die im UI-Thread aufgerufen wird, alle Ausnahmen, die nicht behandelt werden können, den UI-Thread erneut bereitstellen.

#### <a name="mutable-structures-in-async-and-iterator-methods"></a>Änderbare Strukturen in Async-und Iterator-Methoden

Änderbare Strukturen im Allgemeinen gelten als ungültige Praktiken und werden von Async-oder Iteratormethoden nicht unterstützt. Insbesondere wird jeder Aufruf einer Async-oder Iterator-Methode in einer-Struktur implizit auf eine *Kopie* dieser Struktur angewendet, die zum Zeitpunkt des aufhebers kopiert wird. Wenn Sie also beispielsweise

```vb
Structure S
       Dim x As Integer
       Async Sub Mutate()
           x = 2
       End Sub
End Structure

Dim s As New S With {.x = 1}
s.Mutate()
Console.WriteLine(s.x)   ' prints "1"
```

### <a name="blocks-and-labels"></a>Blöcke und Bezeichnungen

Eine Gruppe ausführbarer Anweisungen wird als Anweisungsblock bezeichnet. Die Ausführung eines Anweisungsblocks beginnt mit der ersten Anweisung im-Block. Nachdem eine Anweisung ausgeführt wurde, wird die nächste Anweisung in lexikalischer Reihenfolge ausgeführt, es sei denn, eine Anweisung überträgt an anderer Stelle die Ausführung, oder es tritt eine Ausnahme auf.

Innerhalb eines Anweisungsblocks ist die Division von Anweisungen in logischen Zeilen mit Ausnahme von Bezeichnungs Deklarations Anweisungen nicht signifikant. Eine Bezeichnung ist ein Bezeichner, der eine bestimmte Position innerhalb des Anweisungsblocks identifiziert, die als Ziel einer Verzweigungs Anweisung (z. b. `GoTo`) verwendet werden kann.

```antlr
Block
    : Statements*
    ;

LabelDeclarationStatement
    : LabelName ':'
    ;

LabelName
    : Identifier
    | IntLiteral
    ;

Statements
    : Statement? ( ':' Statement? )*
    ;
```


Bezeichnungs Deklarations Anweisungen müssen am Anfang einer logischen Zeile und Bezeichnungen entweder ein Bezeichner oder ein ganzzahliges Literalzeichen sein. Da sowohl Bezeichnungs Deklarations Anweisungen als auch Aufruf Anweisungen aus einem einzelnen Bezeichner bestehen können, wird ein einzelner Bezeichner am Anfang einer lokalen Zeile immer als Bezeichnungs Deklarations Anweisung angesehen. Auf Bezeichnungs Deklarations Anweisungen muss immer ein Doppelpunkt folgen, auch wenn keine-Anweisungen in derselben logischen Zeile folgen.

Bezeichnungen verfügen über einen eigenen Deklarations Bereich und stören andere Bezeichner nicht. Das folgende Beispiel ist gültig und verwendet die Name-Variable `x` sowohl als Parameter als auch als Bezeichnung.

```vb
Function F(x As Integer) As Integer
    If x >= 0 Then
        GoTo x
    End If
    x = -x
x: 
    Return x
End Function
```

Der Gültigkeitsbereich einer Bezeichnung ist der Text der Methode, in der Sie enthalten ist.

Zur besseren Lesbarkeit werden Anweisungs Produktionen, die mehrere unter Anweisungen einbeziehen, als eine einzelne Produktion in dieser Spezifikation behandelt, auch wenn sich die untergeordneten Anweisungen jeweils in einer gekennzeichneten Zeile befinden können.


### <a name="local-variables-and-parameters"></a>Lokale Variablen und Parameter

In den vorstehenden Abschnitten wird erläutert, wie und wann Methoden Instanzen erstellt werden und wie Sie mit den Kopien der lokalen Variablen und Parameter einer Methode erstellt werden. Außerdem wird jedes Mal, wenn der Text einer Schleife eingegeben wird, eine neue Kopie aus jeder in dieser Schleife deklarierten lokalen Variable erstellt, wie in Abschnitts [Schleifen Anweisungen](statements.md#loop-statements)beschrieben, und die-Methoden Instanz enthält jetzt diese Kopie der lokalen Variablen anstelle der vorherigen Kopie.

Alle lokalen Variablen werden mit dem Standardwert ihres Typs initialisiert. Lokale Variablen und Parameter sind immer öffentlich zugänglich. Es ist ein Fehler, in einer Textposition, die der Deklaration vorangestellt ist, auf eine lokale Variable zu verweisen, wie im folgenden Beispiel veranschaulicht:

```vb
Class A
    Private i As Integer = 0

    Sub F()
        i = 1
        Dim i As Integer       ' Error, use precedes declaration.
        i = 2
    End Sub

    Sub G()
        Dim a As Integer = 1
        Dim b As Integer = a   ' This is valid.
    End Sub
End Class
```

In der obigen Methode `F` verweist die erste Zuweisung an `i` nicht auf das im äußeren Gültigkeitsbereich deklarierte Feld. Stattdessen verweist Sie auf die lokale Variable und ist fehlerhaft, da Sie sich im textext vor der Deklaration der Variablen befindet. In der `G`-Methode verweist eine nachfolgende Variablen Deklaration auf eine lokale Variable, die in einer früheren Variablen Deklaration innerhalb derselben lokalen Variablen Deklaration deklariert ist.

Jeder Block in einer Methode erstellt einen Deklarations Raum für lokale Variablen. Namen werden in diesem Deklarations Raum durch lokale Variablen Deklarationen im Methoden Text und durch die Parameterliste der Methode eingeführt, die Namen in den Deklarations Bereich des äußersten Blocks einführt. -Blöcke lassen das shadodown von Namen durch Schachtelung nicht zu: Nachdem ein Name in einem-Block deklariert wurde, kann der Name nicht mehr in geschachtelten-Blöcken deklariert werden.

Im folgenden Beispiel sind die Methoden `F` und `G` fehlerhaft, da der Name `i` im äußeren Block deklariert ist und nicht im Inneren Block erneut deklariert werden kann. Allerdings sind die Methoden `H` und `I` gültig, da die beiden `i` in separaten nicht in-Blöcken deklarierten Blöcken deklariert werden.

```vb
Class A
    Sub F()
        Dim i As Integer = 0
        If True Then
               Dim i As Integer = 1
        End If
    End Sub

    Sub G()
        If True Then
            Dim i As Integer = 0
        End If
        Dim i As Integer = 1
    End Sub 

    Sub H()
        If True Then
            Dim i As Integer = 0
        End If
        If True Then
            Dim i As Integer = 1
        End If
    End Sub

    Sub I() 
        For i As Integer = 0 To 9
            H()
        Next i

        For i As Integer = 0 To 9
            H()
        Next i
    End Sub 
End Class
```

Wenn die Methode eine Funktion ist, wird eine spezielle lokale Variable implizit im Deklarations Bereich des Methoden Texts mit dem gleichen Namen deklariert wie die Methode, die den Rückgabewert der Funktion darstellt. Die lokale Variable hat eine spezielle namens Auflösungs Semantik, wenn Sie in Ausdrücken verwendet wird. Wenn die lokale Variable in einem Kontext verwendet wird, der einen als Methoden Gruppe klassifizierten Ausdruck erwartet (z. b. ein Aufruf Ausdruck), wird der Name in die Funktion und nicht in die lokale Variable aufgelöst. Zum Beispiel:

```vb
Function F(i As Integer) As Integer
    If i = 0 Then
        F = 1        ' Sets the return value.
    Else
        F = F(i - 1) ' Recursive call.
    End If
End Function
```

Die Verwendung von Klammern kann mehrdeutige Situationen verursachen (z. b. "`F(1)`", wobei "`F`" eine Funktion ist, deren Rückgabetyp ein eindimensionales Array ist). in allen mehrdeutigen Situationen wird der Name in die Funktion und nicht in die lokale Variable aufgelöst. Zum Beispiel:

```vb
Function F(i As Integer) As Integer()
    If i = 0 Then
        F = new Integer(2) { 1, 2, 3 }
    Else
        F = F(i - 1) ' Recursive call, not an index.
    End If
End Function
```

Wenn die Ablauf Steuerung den Methoden Text verlässt, wird der Wert der lokalen Variablen an den Aufruf Ausdruck zurückgegeben. Wenn es sich bei der Methode um eine Unterroutine handelt, gibt es keine implizite lokale Variable, und die Steuerung kehrt einfach zum Aufruf Ausdruck zurück.

## <a name="local-declaration-statements"></a>Lokale Deklarations Anweisungen

Eine lokale Deklarations Anweisung deklariert eine neue lokale Variable, eine lokale Konstante oder eine statische Variable. *Lokale Variablen* und *lokale Konstanten* entsprechen Instanzvariablen und Konstanten, die auf die-Methode bezogen sind und auf die gleiche Weise deklariert werden. *Statische Variablen* ähneln `Shared`-Variablen und werden mithilfe des `Static`-Modifizierers deklariert.

```antlr
LocalDeclarationStatement
    : LocalModifier VariableDeclarators StatementTerminator
    ;

LocalModifier
    : 'Static' | 'Dim' | 'Const'
    ;
```

Statische Variablen sind lokale Variablen, deren Wert über Aufrufe der Methode hinweg beibehalten wird. Statische Variablen, die in nicht freigegebenen Methoden deklariert werden, sind pro Instanz: jede Instanz des Typs, der die Methode enthält, verfügt über eine eigene Kopie der statischen Variablen. Statische Variablen, die in `Shared`-Methoden deklariert werden, sind pro Typ. Es ist nur eine Kopie der statischen Variablen für alle Instanzen vorhanden. Während lokale Variablen bei jedem Eintrag in die-Methode auf den Standardwert ihres Typs initialisiert werden, werden statische Variablen nur mit dem Standardwert ihres Typs initialisiert, wenn der Typ oder die Typinstanz initialisiert wird. Statische Variablen dürfen nicht in Strukturen oder generischen Methoden deklariert werden.

Lokale Variablen, lokale Konstanten und statische Variablen haben immer öffentliche Barrierefreiheit und können keine Zugriffsmodifizierer angeben. Wenn für eine lokale Deklarations Anweisung kein Typ angegeben ist, bestimmen die folgenden Schritte den Typ der lokalen Deklaration:

1. Wenn die Deklaration ein Typzeichen aufweist, ist der Typ des Typzeichens der Typ der lokalen Deklaration.

2. Wenn die lokale Deklaration eine lokale Konstante ist, oder wenn die lokale Deklaration eine lokale Variable mit einem Initialisierer ist, und ein lokaler Variablen-Typrückschluss verwendet wird, wird der Typ der lokalen Deklaration vom Typ des Initialisierers abgeleitet. Wenn sich der Initialisierer auf die lokale Deklaration bezieht, tritt ein Kompilierzeitfehler auf. (Für lokale Konstanten sind Initialisierer erforderlich.)

3. Wenn eine strikte Semantik nicht verwendet wird, ist der Typ der lokalen Deklarations Anweisung implizit `Object`.

4. Andernfalls tritt ein Kompilierungsfehler auf.

Wenn für eine lokale Deklarations Anweisung, die eine Array Größe oder einen Arraytypmodifizierer aufweist, kein Typ angegeben ist, ist der Typ der lokalen Deklaration ein Array mit dem angegebenen Rang, und die vorherigen Schritte werden verwendet, um den Elementtyp des Arrays zu bestimmen. Wenn der Typrückschluss für lokale Variablen verwendet wird, muss der Typ des Initialisierers ein Arraytyp mit derselben Array Form (d.h. Arraytypmodifizierer) als lokale Deklarations Anweisung sein. Beachten Sie, dass es möglicherweise immer noch ein Arraytyp ist. Zum Beispiel:

```vb
Option Infer On

Module Test
    Sub Main()
        ' Error: initializer is not an array type
        Dim x() = 1

        ' Type is Integer()
        Dim y() = New Integer() {}

        ' Type is Integer()()
        Dim z() = New Integer()() {}

        ' Type is Integer()()()

        Dim a()() = New Integer()()() {}

        ' Error: initializer does not have same array shape
        Dim b()() = New Integer(,)() {}
    End Sub
End Module
```

Wenn für eine lokale Deklarations Anweisung mit einem Typmodifizierer, der NULL-Werte zulässt, kein Typ angegeben ist, ist der Typ der lokalen Deklaration die auf NULL festleg Bare Version des abhergenten Typs oder der abhergelegte Typ selbst, wenn er bereits einen Werttyp aufweist, der NULL  Wenn es sich bei dem abhergestellten Typ nicht um einen Werttyp handelt, der NULL-Werte zulässt, tritt ein Kompilierzeitfehler auf. Wenn sowohl ein Typmodifizierer, der NULL-Werte zulässt, als auch eine Array Größe oder ein Arraytypmodifizierer in einer lokalen Deklarations Anweisung ohne Typ platziert werden, wird der Typmodifizierer, der NULL-Werte zulässt, auf den Elementtyp des Arrays angewendet, und die vorherigen Schritte werden verwendet, um die ELEME zu bestimmen NT-Typ.

Variableninitialisierer für lokale Deklarations Anweisungen entsprechen Zuweisungs Anweisungen, die an der Textposition der Deklaration platziert werden. Wenn die Ausführung über die lokale Deklarations Anweisung verzweigt, wird der Variableninitialisierer daher nicht ausgeführt. Wenn die lokale Deklarations Anweisung mehrmals ausgeführt wird, wird der Variableninitialisierer gleich oft ausgeführt. Statische Variablen führen den Initialisierer nur beim ersten Mal aus. Wenn beim Initialisieren einer statischen Variablen eine Ausnahme auftritt, wird die statische Variable als initialisiert mit dem Standardwert des Typs der statischen Variablen betrachtet.

Das folgende Beispiel zeigt die Verwendung von Initialisierern:

```vb
Module Test
    Sub F()
        Static x As Integer = 5

        Console.WriteLine("Static variable x = " & x)
        x += 1
    End Sub

    Sub Main()
        Dim i As Integer

        For i = 1 to 3
            F()
        Next i

        i = 3
label:
        Dim y As Integer = 8

        If i > 0 Then
            Console.WriteLine("Local variable y = " & y)
            y -= 1
            i -= 1
            GoTo label
        End If
    End Sub
End Module
```

Dieses Programm druckt Folgendes:

```console
Static variable x = 5
Static variable x = 6
Static variable x = 7
Local variable y = 8
Local variable y = 8
Local variable y = 8
```

Initialisierer für statische lokale Variablen sind Thread sicher und vor Ausnahmen während der Initialisierung geschützt. Wenn bei einem statischen lokalen Initialisierer eine Ausnahme auftritt, weist das statische lokale den Standardwert auf und wird nicht initialisiert. Ein statischer lokaler Initialisierer

```vb
Module Test
    Sub F()
        Static x As Integer = 5
    End Sub
End Module
```

für die folgende Syntax:

```vb
Imports System.Threading
Imports Microsoft.VisualBasic.CompilerServices

Module Test
    Class InitFlag
        Public State As Short
    End Class

    Private xInitFlag As InitFlag = New InitFlag()

    Sub F()
        Dim x As Integer

        If xInitFlag.State <> 1 Then
            Monitor.Enter(xInitFlag)
            Try
                If xInitFlag.State = 0 Then
                    xInitFlag.State = 2
                    x = 5
                Else If xInitFlag.State = 2 Then
                    Throw New IncompleteInitialization()
                End If
            Finally
                xInitFlag.State = 1
                Monitor.Exit(xInitFlag)
            End Try
        End If
    End Sub
End Module
```

Lokale Variablen, lokale Konstanten und statische Variablen werden auf den Anweisungsblock festgelegt, in dem Sie deklariert werden. Statische Variablen sind ein besonderes Zeichen darin, dass ihre Namen nur einmal in der gesamten Methode verwendet werden können. Beispielsweise ist es nicht zulässig, zwei statische Variablen Deklarationen mit demselben Namen anzugeben, auch wenn Sie sich in unterschiedlichen Blöcken befinden.


### <a name="implicit-local-declarations"></a>Implizite lokale Deklarationen

Zusätzlich zu den lokalen Deklarations Anweisungen können auch lokale Variablen implizit durch Verwendung von deklariert werden. Ein einfacher namens Ausdruck, der einen Namen verwendet, der nicht in etwas anderes aufgelöst wird, deklariert eine lokale Variable mit diesem Namen. Zum Beispiel:

```vb
Option Explicit Off

Module Test
    Sub Main()
        x = 10
        y = 20
        Console.WriteLine(x + y)
    End Sub
End Module
```

Die implizite lokale Deklaration findet nur in Ausdrucks Kontexten statt, die einen als Variable klassifizierten Ausdruck akzeptieren können. Eine Ausnahme von dieser Regel ist, dass eine lokale Variable möglicherweise nicht implizit deklariert wird, wenn Sie das Ziel eines Funktionsaufruf Ausdrucks, eines Indizierungs Ausdrucks oder eines Element Zugriffs Ausdrucks ist.

Implizite lokale Variablen werden so behandelt, als wären Sie am Anfang der enthaltenden Methode deklariert. Folglich werden Sie immer auf den gesamten Methoden Text beschränkt, auch wenn Sie in einem Lambda-Ausdruck deklariert sind. Beispielsweise folgender Code:

```vb
Option Explicit Off 

Module Test
    Sub Main()
        Dim x = Sub()
                    a = 10
                End Sub
        Dim y = Sub()
                    Console.WriteLine(a)
                End Sub

        x()
        y()
    End Sub
End Module
```

druckt den Wert `10`. Implizite lokale Variablen werden als `Object` typisiert, wenn kein Typzeichen an den Variablennamen angefügt wurde. Andernfalls ist der Typ der Variablen der Typ des Typzeichens. Der Typrückschluss für lokale Variablen wird nicht für implizite lokale Variablen verwendet.

Wenn eine explizite lokale Deklaration durch die Kompilierungs Umgebung oder `Option Explicit` angegeben wird, müssen alle lokalen Variablen explizit deklariert werden, und die implizite Variablen Deklaration ist nicht zulässig.

## <a name="with-statement"></a>With-Anweisung

Eine `With`-Anweisung lässt mehrere Verweise auf die Member eines Ausdrucks zu, ohne den Ausdruck mehrmals anzugeben.

```antlr
WithStatement
    : 'With' Expression StatementTerminator
      Block?
      'End' 'With' StatementTerminator
    ;
```

Der Ausdruck muss als Wert klassifiziert werden und wird nach dem Einzug in den-Block einmal ausgewertet. Innerhalb des `With`-Anweisungsblocks wird ein Member-Zugriffs Ausdruck oder ein Wörterbuch Zugriffs Ausdruck, der mit einem Punkt oder einem Ausrufezeichen beginnt, ausgewertet, als wäre der `With`-Ausdruck vorangestellt. Zum Beispiel:

```vb
Structure Test
    Public x As Integer

    Function F() As Integer
        Return 10
    End Sub
End Structure

Module TestModule
    Sub Main()
        Dim y As Test

        With y
            .x = 10
            Console.WriteLine(.x)
            .x = .F()
        End With
    End Sub
End Module
```

Die Verzweigung in einen `With`-Anweisungsblock von außerhalb des Blocks ist ungültig.


## <a name="synclock-statement"></a>SyncLock-Anweisung

Mit einer `SyncLock`-Anweisung können Anweisungen für einen Ausdruck synchronisiert werden, wodurch sichergestellt wird, dass die gleichen Anweisungen nicht gleichzeitig von mehreren Ausführungs Ausführungsthreads ausgeführt werden.

```antlr
SyncLockStatement
    : 'SyncLock' Expression StatementTerminator
      Block?
      'End' 'SyncLock' StatementTerminator
    ;
```

Der Ausdruck muss als Wert klassifiziert werden und wird nach dem Einzug in den Block einmal ausgewertet. Wenn Sie den `SyncLock`-Block eingeben, wird die `Shared`-Methode `System.Threading.Monitor.Enter` für den angegebenen Ausdruck aufgerufen, der blockiert, bis der Ausführungs Thread eine exklusive Sperre für das vom Ausdruck zurückgegebene Objekt aufweist. Der Typ des Ausdrucks in einer `SyncLock`-Anweisung muss ein Verweistyp sein. Zum Beispiel:

```vb
Class Test
    Private count As Integer = 0

    Public Function Add() As Integer
        SyncLock Me
            count += 1
            Add = count
        End SyncLock
    End Function

    Public Function Subtract() As Integer
        SyncLock Me
            count -= 1
            Subtract = count
        End SyncLock
    End Function
End Class
```

Im obigen Beispiel wird auf der spezifischen Instanz der-Klasse `Test` synchronisiert, um sicherzustellen, dass nicht mehr als ein Ausführungs Thread für eine bestimmte Instanz von der count-Variablen zu einem Zeitpunkt hinzugefügt oder von diesem subtrahiert werden kann.

Der `SyncLock`-Block ist implizit in einer `Try`-Anweisung enthalten, deren `Finally`-Block die `Shared`-Methode `System.Threading.Monitor.Exit` für den Ausdruck aufruft. Dadurch wird sichergestellt, dass die Sperre freigegeben wird, auch wenn eine Ausnahme ausgelöst wird. Daher ist es ungültig, von außerhalb des Blocks in einen `SyncLock`-Block zu verzweigen, und ein `SyncLock`-Block wird als einzelne Anweisung für `Resume` und `Resume Next` behandelt. Das obige Beispiel entspricht dem folgenden Code:

```vb
Class Test
    Private count As Integer = 0

    Public Function Add() As Integer
        Try
            System.Threading.Monitor.Enter(Me)

            count += 1
            Add = count
        Finally
            System.Threading.Monitor.Exit(Me)
        End Try
    End Function

    Public Function Subtract() As Integer
        Try
            System.Threading.Monitor.Enter(Me)

            count -= 1
            Subtract = count
        Finally
            System.Threading.Monitor.Exit(Me)
        End Try
    End Function
End Class
```


## <a name="event-statements"></a>Ereignis Anweisungen

Die Anweisungen `RaiseEvent`, `AddHandler` und `RemoveHandler` lösen Ereignisse aus und behandeln Ereignisse dynamisch.

```antlr
EventStatement
    : RaiseEventStatement
    | AddHandlerStatement
    | RemoveHandlerStatement
    ;
```

### <a name="raiseevent-statement"></a>RaiseEvent-Anweisung

Eine `RaiseEvent`-Anweisung benachrichtigt Ereignishandler, dass ein bestimmtes Ereignis aufgetreten ist.

```antlr
RaiseEventStatement
    : 'RaiseEvent' IdentifierOrKeyword
      ( OpenParenthesis ArgumentList? CloseParenthesis )? StatementTerminator
    ;
```

Der einfache namens Ausdruck in einer `RaiseEvent`-Anweisung wird als Element Suche auf `Me` interpretiert. Folglich wird `RaiseEvent x` so interpretiert, als ob es `RaiseEvent Me.x` wäre. Das Ergebnis des Ausdrucks muss als Ereignis Zugriff für ein Ereignis klassifiziert werden, das in der Klasse selbst definiert ist. für Basis Typen definierte Ereignisse können nicht in einer `RaiseEvent`-Anweisung verwendet werden.

Die `RaiseEvent`-Anweisung wird als Aufruf der `Invoke`-Methode des Delegaten des Ereignisses verarbeitet, wobei die angegebenen Parameter verwendet werden (sofern vorhanden). Wenn der Wert des Delegaten `Nothing` ist, wird keine Ausnahme ausgelöst. Wenn keine Argumente vorhanden sind, können die Klammern ausgelassen werden. Zum Beispiel:

```vb
Class Raiser
    Public Event E1(Count As Integer)

    Public Sub Raise()
        Static RaiseCount As Integer = 0

        RaiseCount += 1
        RaiseEvent E1(RaiseCount)
    End Sub
End Class

Module Test
    Private WithEvents x As Raiser

    Private Sub E1Handler(Count As Integer) Handles x.E1
        Console.WriteLine("Raise #" & Count)
    End Sub

    Public Sub Main()
        x = New Raiser
        x.Raise()        ' Prints "Raise #1".
        x.Raise()        ' Prints "Raise #2".
        x.Raise()        ' Prints "Raise #3".
    End Sub
End Module
```

Die oben genannte Klasse `Raiser` ist äquivalent zu:

```vb
Class Raiser
    Public Event E1(Count As Integer)

    Public Sub Raise()
        Static RaiseCount As Integer = 0
        Dim TemporaryDelegate As E1EventHandler

        RaiseCount += 1

        ' Use a temporary to avoid a race condition.
        TemporaryDelegate = E1Event
        If Not TemporaryDelegate Is Nothing Then
            TemporaryDelegate.Invoke(RaiseCount)
        End If
    End Sub
End Class
```


### <a name="addhandler-and-removehandler-statements"></a>AddHandler-und RemoveHandler-Anweisungen

Obwohl die meisten Ereignishandler automatisch über `WithEvents`-Variablen verknüpft werden, ist es möglicherweise notwendig, Ereignishandler zur Laufzeit dynamisch hinzuzufügen und zu entfernen. Dies wird von `AddHandler`-und `RemoveHandler`-Anweisung durchführen.

```antlr
AddHandlerStatement
    : 'AddHandler' Expression Comma Expression StatementTerminator
    ;

RemoveHandlerStatement
    : 'RemoveHandler' Expression Comma Expression StatementTerminator
    ;
```

Jede Anweisung erfordert zwei Argumente: das erste Argument muss ein Ausdruck sein, der als Ereignis Zugriff klassifiziert ist, und das zweite Argument muss ein Ausdruck sein, der als Wert klassifiziert ist. Der Typ des zweiten Arguments muss der Delegattyp sein, der mit dem Ereignis Zugriff verknüpft ist. Zum Beispiel:

```vb
Public Class Form1
    Public Sub New()
        ' Add Button1_Click as an event handler for Button1's Click event.
        AddHandler Button1.Click, AddressOf Button1_Click
    End Sub 

    Private Button1 As Button = New Button()

    Sub Button1_Click(sender As Object, e As EventArgs)
        Console.WriteLine("Button1 was clicked!")
    End Sub

    Public Sub Disconnect()
        RemoveHandler Button1.Click, AddressOf Button1_Click
    End Sub
End Class
```

Bei einem Ereignis `E,` ruft die-Anweisung die relevante `add_E`-oder `remove_E`-Methode für die-Instanz auf, um den Delegaten als Handler für das-Ereignis hinzuzufügen oder zu entfernen. Folglich ist der obige Code äquivalent zu:

```vb
Public Class Form1
    Public Sub New()
        Button1.add_Click(AddressOf Button1_Click)
    End Sub 

    Private Button1 As Button = New Button()

    Sub Button1_Click(sender As Object, e As EventArgs)
        Console.WriteLine("Button1 was clicked!")
    End Sub

    Public Sub Disconnect()
        Button1.remove_Click(AddressOf Button1_Click)
    End Sub
End Class
```


## <a name="assignment-statements"></a>Zuweisungs Anweisungen

Eine Zuweisungsanweisung weist den Wert eines Ausdrucks einer Variablen zu. Es gibt mehrere Arten der Zuweisung.

```antlr
AssignmentStatement
    : RegularAssignmentStatement
    | CompoundAssignmentStatement
    | MidAssignmentStatement
    ;
```

### <a name="regular-assignment-statements"></a>Reguläre Zuweisungs Anweisungen

Eine einfache Zuweisungsanweisung speichert das Ergebnis eines Ausdrucks in einer Variablen.

```antlr
RegularAssignmentStatement
    : Expression Equals Expression StatementTerminator
    ;
```

Der Ausdruck auf der linken Seite des Zuweisungs Operators muss als Variable oder Eigenschafts Zugriff klassifiziert werden, während der Ausdruck auf der rechten Seite des Zuweisungs Operators als Wert klassifiziert werden muss. Der Typ des Ausdrucks muss implizit in den Typ der Variablen oder des Eigenschafts Zugriffs konvertiert werden können.

Wenn die Variable, die in zugewiesen wird, ein Array Element eines Verweis Typs ist, wird eine Lauf Zeit Überprüfung durchgeführt, um sicherzustellen, dass der Ausdruck mit dem Array Elementtyp kompatibel ist. Im folgenden Beispiel bewirkt die letzte Zuweisung, dass eine `System.ArrayTypeMismatchException` ausgelöst wird, da eine Instanz von `ArrayList` nicht in einem Element eines `String`-Arrays gespeichert werden kann.

```vb
Dim sa(10) As String
Dim oa As Object() = sa
oa(0) = Nothing         ' This is allowed.
oa(1) = "Hello"         ' This is allowed.
oa(2) = New ArrayList() ' System.ArrayTypeMismatchException is thrown.
```

Wenn der Ausdruck auf der linken Seite des Zuweisungs Operators als Variable klassifiziert wird, speichert die Zuweisungsanweisung den Wert in der Variablen. Wenn der Ausdruck als Eigenschaften Zugriff klassifiziert ist, wird der Eigenschafts Zugriff durch die Zuweisungsanweisung in einen Aufruf des `Set`-Accessors der Eigenschaft umgewandelt, wobei der Wert durch den value-Parameter ersetzt wird. Zum Beispiel:

```vb
Module Test
    Private PValue As Integer

    Public Property P As Integer
        Get
            Return PValue
        End Get

        Set (Value As Integer)
            PValue = Value
        End Set
    End Property

    Sub Main()
        ' The following two lines are equivalent.
        P = 10
        set_P(10)
    End Sub
End Module
```

Wenn das Ziel der Variablen oder des Eigenschaften Zugriffs als Werttyp typisiert ist, aber nicht als Variable klassifiziert ist, tritt ein Kompilierzeitfehler auf. Zum Beispiel:

```vb
Structure S
    Public F As Integer
End Structure

Class C
    Private PValue As S

    Public Property P As S
        Get
            Return PValue
        End Get

        Set (Value As S)
            PValue = Value
        End Set
    End Property
End Class

Module Test
    Sub Main()
        Dim ct As C = New C()
        Dim rt As Object = new C()

        ' Compile-time error: ct.P not classified as variable.
        ct.P.F = 10

        ' Run-time exception.
        rt.P.F = 10
    End Sub
End Module
```

Beachten Sie, dass die Semantik der Zuweisung vom Typ der Variablen oder Eigenschaft abhängt, der Sie zugewiesen wird. Wenn die Variable, der Sie zugewiesen wird, ein Werttyp ist, kopiert die Zuweisung den Wert des Ausdrucks in die Variable. Wenn die Variable, der Sie zugewiesen wird, ein Verweistyp ist, kopiert die Zuweisung den Verweis und nicht den Wert selbst in die Variable. Wenn der Typ der Variablen `Object` ist, wird die Zuweisungs Semantik bestimmt, ob der Werttyp zur Laufzeit ein Werttyp oder ein Verweistyp ist.


__Nebenbei.__ Bei systeminternen Typen wie `Integer` und `Date` sind Verweis-und Wert Zuweisungs Semantik identisch, da die Typen unveränderlich sind. Folglich ist es für die Sprache frei, die Verweis Zuweisung für systeminterne schachtelte Typen als Optimierung zu verwenden. Aus Sicht des Werts ist das Ergebnis identisch.

Da das Gleichheitszeichen (`=`) sowohl für die Zuweisung als auch für Gleichheit verwendet wird, besteht eine Mehrdeutigkeit zwischen einer einfachen Zuweisung und einer Aufruf Anweisung in Situationen wie `x = y.ToString()`. In all diesen Fällen hat die Zuweisungsanweisung Vorrang vor dem Gleichheits Operator. Dies bedeutet, dass der Beispiel Ausdruck als `x = (y.ToString())` und nicht als `(x = y).ToString()` interpretiert wird.


### <a name="compound-assignment-statements"></a>Verbund Zuweisungs Anweisungen

Eine *Verbund Zuweisungsanweisung* hat die Form `V op= E` (wobei `op` ein gültiger binärer Operator ist).

```antlr
CompoundAssignmentStatement
    : Expression CompoundBinaryOperator LineTerminator? Expression StatementTerminator
    ;

CompoundBinaryOperator
    : '^' '=' | '*' '=' | '/' '=' | '\\' '=' | '+' '=' | '-' '='
    | '&' '=' | '<' '<' '=' | '>' '>' '='
    ;
```

Der Ausdruck auf der linken Seite des Zuweisungs Operators muss als Variablen-oder Eigenschafts Zugriff klassifiziert werden, während der Ausdruck auf der rechten Seite des Zuweisungs Operators als Wert klassifiziert werden muss. Die Verbund Zuweisungsanweisung entspricht der-Anweisung `V = V op E` mit dem Unterschied, dass die Variable auf der linken Seite des Verbund Zuweisungs Operators nur einmal ausgewertet wird. Dieses Unterschied wird im folgenden Beispiel veranschaulicht:

```vb
Module Test
    Function GetIndex() As Integer
        Console.WriteLine("Getting index")
        Return 1
    End Function

    Sub Main()
        Dim a(2) As Integer

        Console.WriteLine("Simple assignment")
        a(GetIndex()) = a(GetIndex()) + 1

        Console.WriteLine("Compound assignment")
        a(GetIndex()) += 1
    End Sub
End Module
```

Der Ausdruck "`a(GetIndex())`" wird für die einfache Zuweisung zweimal ausgewertet, aber nur einmal für die Verbund Zuweisung, daher druckt der Code Folgendes:

```console
Simple assignment
Getting index
Getting index
Compound assignment
Getting index
```


### <a name="mid-assignment-statement"></a>Mid-Zuweisungsanweisung

Eine `Mid`-Zuweisungsanweisung weist eine Zeichenfolge einer anderen Zeichenfolge zu. Die linke Seite der Zuweisung hat dieselbe Syntax wie ein Aufrufs`Microsoft.VisualBasic.Strings.Mid`.

```antlr
MidAssignmentStatement
    : 'Mid' '$'? OpenParenthesis Expression Comma Expression
      ( Comma Expression )? CloseParenthesis Equals Expression StatementTerminator
    ;
```

Das erste Argument ist das Ziel der Zuweisung und muss als Variable oder Eigenschafts Zugriff klassifiziert werden, dessen Typ implizit in und aus `String` konvertiert werden kann. Der zweite Parameter ist die 1-basierte Startposition, die entspricht, wo die Zuweisung in der Ziel Zeichenfolge beginnen soll und als Wert klassifiziert werden muss, dessen Typ implizit in `Integer` konvertiert werden muss. Der optionale dritte Parameter ist die Anzahl der Zeichen aus dem rechtsseitigen Wert, der der Ziel Zeichenfolge zugewiesen werden soll. er muss als Wert klassifiziert werden, dessen Typ implizit in `Integer` konvertiert werden kann. Die Rechte Seite ist die Quell Zeichenfolge und muss als Wert klassifiziert werden, dessen Typ implizit in `String` konvertiert werden kann. Die Rechte Seite wird ggf. auf den length-Parameter gekürzt und ersetzt die Zeichen in der Zeichenfolge auf der linken Seite, beginnend bei der Startposition. Wenn die Zeichenfolge der rechten Seite weniger Zeichen als der dritte Parameter enthielt, werden nur die Zeichen aus der rechten Zeichenfolge kopiert.

Im folgenden Beispiel wird `ab123fg` angezeigt:

```vb
Module Test
    Sub Main()
        Dim s1 As String = "abcdefg"
        Dim s2 As String = "1234567"

        Mid$(s1, 3, 3) = s2
        Console.WriteLine(s1)
    End Sub
End Module
```

__Nebenbei.__ `Mid` ist kein reserviertes Wort.


## <a name="invocation-statements"></a>Aufruf Anweisungen

Eine Aufruf Anweisung ruft eine Methode auf, der das optionale Schlüsselwort `Call` vorangestellt ist. Die Aufruf Anweisung wird auf die gleiche Weise wie der Funktionsaufruf Ausdruck verarbeitet, mit einigen Unterschieden, die unten angegeben sind. Der Aufruf Ausdruck muss als Wert oder als void klassifiziert werden. Alle Werte, die sich aus der Auswertung des Aufruf Ausdrucks ergeben, werden verworfen.

Wenn das Schlüsselwort `Call` weggelassen wird, muss der Aufruf Ausdruck mit einem Bezeichner oder Schlüsselwort oder mit `.` innerhalb eines `With`-Blocks beginnen. So ist beispielsweise "`Call 1.ToString()`" eine gültige Anweisung, "`1.ToString()`" jedoch nicht. (Beachten Sie, dass Aufruf Ausdrücke in einem Ausdrucks Kontext auch nicht mit einem Bezeichner beginnen müssen. Beispielsweise ist "`Dim x = 1.ToString()`" eine gültige-Anweisung).

Es gibt einen weiteren Unterschied zwischen den Aufruf Anweisungen und Aufruf Ausdrücken: Wenn eine Aufruf Anweisung eine Argumentliste enthält, wird dies immer als Argumentliste des Auflistungs Ausdrucks verwendet. Das folgende Beispiel veranschaulicht den Unterschied:

```vb
Module Test
    Sub Main()
        Call {Function() 15}(0)
        ' error: (0) is taken as argument list, but array is not invokable

        Call ({Function() 15}(0))
        ' valid, since the invocation statement has no argument list

        Dim x = {Function() 15}(0)
        ' valid as an expression, since (0) is taken as an array-indexing

        Call f("a")
        ' error: ("a") is taken as argument list to the invocation of f

        Call f()("a")
        ' valid, since () is the argument list for the invocation of f

        Dim y = f("a")
        ' valid as an expression, since f("a") is interpreted as f()("a")
    End Sub

    Sub f() As Func(Of String,String)
        Return Function(x) x
    End Sub
End Module
```

```antlr
InvocationStatement
    : 'Call'? InvocationExpression StatementTerminator
    ;
```

## <a name="conditional-statements"></a>Bedingte Anweisungen

Bedingte Anweisungen ermöglichen die bedingte Ausführung von Anweisungen auf der Grundlage von Ausdrücken, die zur Laufzeit ausgewertet werden.

```antlr
ConditionalStatement
    : IfStatement
    | SelectStatement
    ;
```

### <a name="ifthenelse-statements"></a>Wenn... Dann... Else-Anweisungen

Eine `If...Then...Else`-Anweisung ist die grundlegende bedingte Anweisung.

```antlr
IfStatement
    : BlockIfStatement
    | LineIfThenStatement
    ;

BlockIfStatement
    : 'If' BooleanExpression 'Then'? StatementTerminator
      Block?
      ElseIfStatement*
      ElseStatement?
      'End' 'If' StatementTerminator
    ;

ElseIfStatement
    : ElseIf BooleanExpression 'Then'? StatementTerminator
      Block?
    ;

ElseStatement
    : 'Else' StatementTerminator
      Block?
    ;

LineIfThenStatement
    : 'If' BooleanExpression 'Then' Statements ( 'Else' Statements )? StatementTerminator
    ;
    
ElseIf      
    : 'ElseIf'      
    | 'Else' 'If'   
   ;
```

Jeder Ausdruck in einer `If...Then...Else`-Anweisung muss ein boolescher Ausdruck sein, und zwar gemäß [booleschen Abschnitts Ausdrücken](expressions.md#boolean-expressions). (Hinweis: Dies erfordert nicht, dass der Ausdruck einen booleschen Typ hat). Wenn der Ausdruck in der `If`-Anweisung True ist, werden die Anweisungen, die vom `If`-Block eingeschlossen werden, ausgeführt. Wenn der Ausdruck false ist, wird jeder der `ElseIf`-Ausdrücke ausgewertet. Wenn einer der `ElseIf`-Ausdrücke als true ausgewertet wird, wird der entsprechende-Block ausgeführt. Wenn kein Ausdruck zu true ausgewertet wird und ein `Else`-Block vorhanden ist, wird der `Else`-Block ausgeführt. Nachdem die Ausführung eines-Blocks abgeschlossen ist, wird die Ausführung an das Ende der `If...Then...Else`-Anweisung weitergeleitet.

Die Zeilen Version der `If`-Anweisung verfügt über einen einzelnen Satz von Anweisungen, die ausgeführt werden sollen, wenn der `If`-Ausdruck `True` ist, und ein optionaler Satz von Anweisungen, die ausgeführt werden sollen, wenn der Ausdruck `False` ist. Zum Beispiel:

```vb
Module Test
    Sub Main()
        Dim a As Integer = 10
        Dim b As Integer = 20

        ' Block If statement.
        If a < b Then
            a = b
        Else
            b = a
        End If

        ' Line If statement
        If a < b Then a = b Else b = a
    End Sub
End Module
```

Die Zeilen Version der if-Anweisung bindet weniger eng als ":", und der `Else` bindet an die lexikalisch nächstliegende `If`, die von der Syntax zugelassen wird. Die folgenden zwei Versionen sind z. b. äquivalent:

```vb
If True Then _
If True Then Console.WriteLine("a") Else Console.WriteLine("b") _
Else Console.WriteLine("c") : Console.WriteLine("d")

If True Then
    If True Then
        Console.WriteLine("a")
    Else
        Console.WriteLine("b")
    End If
    Console.WriteLine("c") : Console.WriteLine("d")
End If
```

Alle Anweisungen außer Bezeichnungs Deklarations Anweisungen sind innerhalb einer Line `If`-Anweisung zulässig, einschließlich Block Anweisungen. Allerdings dürfen Sie lineterminators nicht als "Status-Terminatoren" verwenden, außer in mehrzeiligen Lambda Ausdrücken. Zum Beispiel:

```vb
' Allowed, since it uses : instead of LineTerminator to separate statements
If b Then With New String("a"(0),5) : Console.WriteLine(.Length) : End With

' Disallowed, since it uses a LineTerminator
If b then With New String("a"(0), 5)
              Console.WriteLine(.Length)
          End With

' Allowed, since it only uses LineTerminator inside a multi-line lambda
If b Then Call Sub()
                   Console.WriteLine("a")
               End Sub.Invoke()
```

### <a name="select-case-statements"></a>Select Case-Anweisungen

Eine `Select Case`-Anweisung führt Anweisungen auf der Grundlage des Werts eines Ausdrucks aus.

```antlr
SelectStatement
    : 'Select' 'Case'? Expression StatementTerminator
      CaseStatement*
      CaseElseStatement?
      'End' 'Select' StatementTerminator
    ;

CaseStatement
    : 'Case' CaseClauses StatementTerminator
      Block?
    ;

CaseClauses
    : CaseClause ( Comma CaseClause )*
    ;

CaseClause
    : ( 'Is' LineTerminator? )? ComparisonOperator LineTerminator? Expression
    | Expression ( 'To' Expression )?
    ;

ComparisonOperator
    : '=' | '<' '>' | '<' | '>' | '>' '=' | '<' '='
    ;

CaseElseStatement
    : 'Case' 'Else' StatementTerminator
      Block?
    ;
```

Der Ausdruck muss als Wert klassifiziert werden. Wenn eine `Select Case`-Anweisung ausgeführt wird, wird der `Select`-Ausdruck zuerst ausgewertet, und die `Case`-Anweisungen werden dann in der Reihenfolge der Text Deklaration ausgewertet. Die erste `Case`-Anweisung, die als `True` ausgewertet wird, wird der-Block ausgeführt. Wenn keine `Case`-Anweisung als `True` ausgewertet wird und eine `Case Else`-Anweisung vorhanden ist, wird dieser Block ausgeführt. Nachdem die Ausführung eines Blocks abgeschlossen ist, wird die Ausführung an das Ende der `Select`-Anweisung weitergeleitet.

Das Ausführen eines `Case`-Blocks ist nicht zulässig, um zum nächsten switchabschnitt zu wechseln. Dadurch wird eine häufige Klasse von Fehlern verhindert, die in anderen Sprachen auftreten, wenn eine `Case`-abschließende Anweisung versehentlich ausgelassen wird. Dieses Verhalten wird im folgenden Beispiel veranschaulicht:

```vb
Module Test
    Sub Main()
        Dim x As Integer = 10

        Select Case x
            Case 5
                Console.WriteLine("x = 5")
            Case 10
                Console.WriteLine("x = 10")
            Case 20 - 10
                Console.WriteLine("x = 20 - 10")
            Case 30
                Console.WriteLine("x = 30")
        End Select
    End Sub
End Module
```

Der Code druckt:

```console
x = 10
```

Obwohl `Case 10` und `Case 20 - 10` für denselben Wert auswählen, wird `Case 10` ausgeführt, da es sich vor `Case 20 - 10` textueller handelt. Wenn die nächste `Case` erreicht wird, wird die Ausführung nach der `Select`-Anweisung fortgesetzt.

Eine `Case`-Klausel kann zwei Formen annehmen. Ein Formular ist ein optionales `Is`-Schlüsselwort, ein Vergleichs Operator und ein Ausdruck. Der Ausdruck wird in den Typ des `Select`-Ausdrucks konvertiert. Wenn der Ausdruck nicht implizit in den Typ des `Select`-Ausdrucks konvertiert werden kann, tritt ein Kompilierzeitfehler auf. Wenn der `Select`-Ausdruck *E*ist, der Vergleichs Operator *op*ist und der `Case`-Ausdruck *E1*ist, wird der Fall als *E op E1*ausgewertet. Der Operator muss für die Typen der beiden Ausdrücke gültig sein. Andernfalls tritt ein Kompilierzeitfehler auf.

Das andere Formular ist ein Ausdruck, auf den optional das Schlüsselwort `To` und ein zweiter Ausdruck folgt. Beide Ausdrücke werden in den Typ des `Select`-Ausdrucks konvertiert. Wenn einer der Ausdrücke nicht implizit in den Typ des `Select`-Ausdrucks konvertiert werden kann, tritt ein Kompilierzeitfehler auf. Wenn der `Select`-Ausdruck `E` ist, der erste `Case`-Ausdruck `E1` und der zweite `Case`-Ausdruck `E2` ist, wird der `Case` entweder als `E = E1` ausgewertet (wenn kein `E2` angegeben ist) oder `(E >= E1) And (E <= E2)`. Die Operatoren müssen für die Typen der beiden Ausdrücke gültig sein. Andernfalls tritt ein Kompilierzeitfehler auf.


## <a name="loop-statements"></a>Schleifen Anweisungen

Schleifen Anweisungen ermöglichen die wiederholte Ausführung der-Anweisungen im Textkörper.

```antlr
LoopStatement
    : WhileStatement
    | DoLoopStatement
    | ForStatement
    | ForEachStatement
    ;
```

Jedes Mal, wenn ein Schleifen Text eingegeben wird, wird eine neue Kopie von allen lokalen Variablen, die in diesem Text deklariert sind, erstellt, die mit den vorherigen Werten der Variablen initialisiert werden. Jeder Verweis auf eine Variable im Schleifen Text verwendet die zuletzt hergestellte Kopie. Der folgende Code zeigt ein Beispiel:

```vb
Module Test
    Sub Main()
        Dim lambdas As New List(Of Action)
        Dim x = 1

        For i = 1 To 3
            x = i
            Dim y = x
            lambdas.Add(Sub() Console.WriteLine(x & y))
        Next

        For Each lambda In lambdas
            lambda()
        Next
    End Sub
End Module
```

Der Code erzeugt die Ausgabe:

```console
31    32    33
```

Wenn der Schleifen Text ausgeführt wird, wird die aktuelle Kopie der Variablen verwendet. Beispielsweise verweist die-Anweisung `Dim y = x` auf die neueste Kopie von `y` und die ursprüngliche Kopie von `x`. Wenn ein Lambda-Ausdruck erstellt wird, wird er unabhängig davon, welche Kopie einer Variablen zum Zeitpunkt der Erstellung aktuell war, gespeichert. Daher verwendet jedes Lambda die gleiche freigegebene Kopie von `x`, aber eine andere Kopie von `y`. Am Ende des Programms wird beim Ausführen der Lambdas eine freigegebene Kopie von `x`, auf die Sie alle verweisen, jetzt am endgültigen Wert 3 angezeigt.

Beachten Sie Folgendes: Wenn keine Lambdas-oder LINQ-Ausdrücke vorhanden sind, ist es unmöglich zu wissen, dass eine neue Kopie für den Schleifen Eintrag erstellt wird. In diesem Fall vermeiden Compileroptimierungen das Erstellen von Kopien. Beachten Sie auch, dass es nicht zulässig ist,-0 in eine Schleife zu @no__t, die Lambdas-oder LINQ-Ausdrücke enthält.


### <a name="whileend-while-and-doloop-statements"></a>While... Ende und Do... Schleifen Anweisungen

Eine `While`-oder `Do`-Schleifen Anweisung, die auf einem booleschen Ausdruck basiert.

```antlr
WhileStatement
    : 'While' BooleanExpression StatementTerminator
      Block?
      'End' 'While' StatementTerminator
    ;

DoLoopStatement
    : DoTopLoopStatement
    | DoBottomLoopStatement
    ;

DoTopLoopStatement
    : 'Do' ( WhileOrUntil BooleanExpression )? StatementTerminator
      Block?
      'Loop' StatementTerminator
    ;

DoBottomLoopStatement
    : 'Do' StatementTerminator
      Block?
      'Loop' WhileOrUntil BooleanExpression StatementTerminator
    ;

WhileOrUntil
    : 'While' | 'Until'
    ;
```

Eine `While`-Schleifen Anweisung wird so lange Schleifen Schleifen, wie der boolesche Ausdruck zu true ausgewertet wird. eine `Do`-Schleifen Anweisung kann eine komplexere Bedingung enthalten. Ein Ausdruck kann nach dem `Do`-Schlüsselwort oder nach dem Schlüsselwort `Loop` platziert werden, aber nicht nach beiden. Der boolesche Ausdruck wird als [boolesche Ausdrücke](expressions.md#boolean-expressions)pro Abschnitt ausgewertet. (Hinweis: Dies erfordert nicht, dass der Ausdruck einen booleschen Typ hat). Es ist auch zulässig, überhaupt keinen Ausdruck anzugeben; in diesem Fall wird die Schleife nie beendet. Wenn der Ausdruck nach `Do` platziert wird, wird er ausgewertet, bevor der Schleifen Block für jede Iterationen ausgeführt wird. Wenn der Ausdruck nach `Loop` platziert wird, wird er nach der Ausführung des Schleifen Blocks bei jeder Iterationen ausgewertet. Wenn Sie den Ausdruck nach `Loop` platzieren, wird daher eine weitere Schleife generiert, als die Platzierung nach `Do`. Das folgende Beispiel veranschaulicht dieses Verhalten:

```vb
Module Test
    Sub Main()
        Dim x As Integer

        x = 3
        Do While x = 1
            Console.WriteLine("First loop")
        Loop

        Do
            Console.WriteLine("Second loop")
        Loop While x = 1
    End Sub
End Module
```

Der Code erzeugt die Ausgabe:

```console
Second Loop
```

Im Fall der ersten Schleife wird die Bedingung ausgewertet, bevor die Schleife ausgeführt wird. Im Fall der zweiten Schleife wird die Bedingung nach Ausführung der Schleife ausgeführt. Der bedingte Ausdruck muss entweder ein `While`-Schlüsselwort oder ein `Until`-Schlüsselwort als Präfix aufweisen. Der erste unterbricht die Schleife, wenn die Bedingung als false ausgewertet wird, letzteres, wenn die Bedingung als true ausgewertet wird.

__Nebenbei.__ `Until` ist kein reserviertes Wort.


### <a name="fornext-statements"></a>Für... Nächste Anweisungen

Eine `For...Next`-Anweisung, die auf einem Satz von Begrenzungen basiert. Eine `For`-Anweisung gibt eine Schleifen Steuerungs Variable, einen unteren gebundenen Ausdruck, einen oberen gebundenen Ausdruck und einen optionalen Schrittwert Ausdruck an. Die Schleifen Steuerungs Variable wird entweder durch einen Bezeichner angegeben, gefolgt von einer optionalen `As`-Klausel oder einem Ausdruck.

```antlr
ForStatement
    : 'For' LoopControlVariable Equals Expression 'To' Expression
      ( 'Step' Expression )? StatementTerminator
      Block?
      ( 'Next' NextExpressionList? StatementTerminator )?
    ;

LoopControlVariable
    : Identifier ( IdentifierModifiers 'As' TypeName )?
    | Expression
    ;

NextExpressionList
    : Expression ( Comma Expression )*
    ;
```

Gemäß den folgenden Regeln verweist die Schleifen Steuerungs Variable entweder auf eine neue lokale Variable, die für diese `For...Next`-Anweisung spezifisch ist, oder auf eine bereits vorhandene Variable oder auf einen Ausdruck.

* Wenn die Schleifen Steuerungs Variable ein Bezeichner mit einer `As`-Klausel ist, definiert der Bezeichner eine neue lokale Variable des in der `As`-Klausel angegebenen Typs, die auf die gesamte `For`-Schleife beschränkt ist.

* Wenn die Schleifen Steuerungs Variable ein Bezeichner ohne eine `As`-Klausel ist, wird der Bezeichner zuerst mithilfe der Regeln für einfache Namensauflösung aufgelöst (siehe Abschnitt [einfache namens Ausdrücke](expressions.md#simple-name-expressions)), ausgenommen, dass dieses Vorkommen des Bezeichners nicht in und von selbst bewirkt, dass eine implizite lokale Variable erstellt wird (Abschnitt [implizite lokale Deklarationen](statements.md#implicit-local-declarations)).

  * Wenn diese Auflösung erfolgreich ist und das Ergebnis als Variable klassifiziert wird, ist die Schleifen Steuerungs Variable diese bereits vorhandene Variable.

  * Wenn die Auflösung fehlschlägt oder die Auflösung erfolgreich ist und das Ergebnis als Typ klassifiziert ist, dann gilt Folgendes:
    * Wenn der Typrückschluss für lokale Variablen verwendet wird, definiert der Bezeichner eine neue lokale Variable, deren Typ aus den gebundenen und Schritt Ausdrücken abgeleitet wird, die auf die gesamte `For`-Schleife beschränkt sind.
    * Wenn der Typrückschluss für lokale Variablen nicht verwendet wird, aber die implizite lokale Deklaration ist, wird eine implizite lokale Variable erstellt, deren Gültigkeitsbereich die gesamte Methode (Abschnitt [implizite lokale Deklarationen](statements.md#implicit-local-declarations)) ist, und die Schleifen Steuerungs Variable verweist auf dieses bereits vorhandene Variable;
    * Wenn weder eine lokale Variable-Typrückschluss noch implizite lokale Deklarationen verwendet werden, ist dies ein Fehler.

  * Wenn die Auflösung mit etwas, das entweder als Typ oder als Variable klassifiziert ist, erfolgreich ist, handelt es sich um einen Fehler.

* Wenn die Schleifen Steuerungs Variable ein Ausdruck ist, muss der Ausdruck als Variable klassifiziert werden.

Eine Schleifen Steuerungs Variable kann nicht von einer anderen einschließenden `For...Next`-Anweisung verwendet werden. Der Typ der Schleifen Steuerungsvariablen einer `For`-Anweisung bestimmt den Typ der Iterationen und muss einer der folgenden sein:

* `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`
* Ein enumerierter Typ
* `Object`
* Ein Typ `T` mit den folgenden Operatoren, wobei `B` ein Typ ist, der in einem booleschen Ausdruck verwendet werden kann:

```vb
Public Shared Operator >= (op1 As T, op2 As T) As B
Public Shared Operator <= (op1 As T, op2 As T) As B
Public Shared Operator - (op1 As T, op2 As T) As T
Public Shared Operator + (op1 As T, op2 As T) As T
```

Die gebundenen und Schritt Ausdrücke müssen implizit in den Typ der Schleifen Steuerungsvariablen konvertiert werden und müssen als Werte klassifiziert werden. Zum Zeitpunkt der Kompilierung wird der Typ der Schleifen Steuerungsvariablen abgeleitet, indem der breiteste Typ zwischen den Ausdrucks Typen für untere Grenze, obere Grenze und Schritt ausgewählt wird. Wenn es keine erweiternde Konvertierung zwischen zwei Typen gibt, tritt ein Kompilierzeitfehler auf.

Wenn der Typ der Schleifen Steuerungsvariablen zur Laufzeit `Object` ist, wird der iterationstyp zum Zeitpunkt der Kompilierung mit zwei Ausnahmen abgeleitet. Erstens: Wenn die gebundenen und Schritt Ausdrücke alle ganzzahligen Typen sind, aber keinen größten Typ aufweisen, wird der breiteste Typ abgeleitet, der alle drei Typen umfasst. Wenn der Typ der Schleifen Steuerungsvariablen als `String` abgeleitet wird, wird stattdessen `Double` abgeleitet. Wenn zur Laufzeit kein Schleifen Steuerungstyp bestimmt werden kann oder wenn einer der Ausdrücke nicht in den Schleifen Steuerungstyp konvertiert werden kann, tritt ein `System.InvalidCastException` auf. Sobald ein Schleifen Steuerungstyp am Anfang der Schleife ausgewählt wurde, wird während der gesamten Iterationen derselbe Typ verwendet, unabhängig von Änderungen, die an dem Wert in der Schleifen Steuerungsvariablen vorgenommen werden.

Eine `For`-Anweisung muss durch eine entsprechende `Next`-Anweisung geschlossen werden. Eine `Next`-Anweisung ohne Variable entspricht der innersten geöffneten `For`-Anweisung, während eine `Next`-Anweisung mit einer oder mehreren Schleifen Steuerungsvariablen von links nach rechts mit den `For`-Schleifen übereinstimmt, die mit den einzelnen Variablen übereinstimmen. Wenn eine Variable mit einer `For`-Schleife übereinstimmt, die zu diesem Zeitpunkt nicht die am meisten gebräuchliche Schleife ist, ergibt sich ein Kompilierzeitfehler.

Am Anfang der Schleife werden die drei Ausdrücke in der Text Reihenfolge ausgewertet, und der untere gebundene Ausdruck wird der Schleifen Steuerungsvariablen zugewiesen. Wenn der Schrittwert weggelassen wird, ist er implizit das Literale `1`, das in den Typ der Schleifen Steuerungsvariablen konvertiert wird. Die drei Ausdrücke werden nur am Anfang der Schleife ausgewertet.

Am Anfang jeder Schleife wird die Steuerelement Variable verglichen, um festzustellen, ob Sie größer als der Endpunkt ist, wenn der Schritt Ausdruck positiv ist, oder kleiner als der Endpunkt, wenn der Schritt Ausdruck negativ ist. Wenn dies der Fall ist, wird die `For`-Schleife beendet. Andernfalls wird der Schleifen Block ausgeführt. Wenn die Schleifen Steuerungs Variable kein primitiver Typ ist, wird der Vergleichs Operator bestimmt, ob der Ausdruck `step >= step - step` true oder false ist. Bei der `Next`-Anweisung wird der Schrittwert der Steuerelement Variablen hinzugefügt, und die Ausführung wird an den Anfang der Schleife zurückgegeben.

Beachten Sie, dass bei jeder Iterations Schleife des Schleifen Blocks *keine* neue Kopie der Schleifen Steuerungs Variable erstellt wird. In dieser Hinsicht unterscheidet sich die `For`-Anweisung von `For Each` (Abschnitt [für jeden... Next-Anweisungen](statements.md#for-eachnext-statements)).

Es ist nicht zulässig, von außerhalb der Schleife in eine `For`-Schleife zu verzweigen.


### <a name="for-eachnext-statements"></a>For each... Nächste Anweisungen

Eine `For Each...Next`-Anweisung, die auf den Elementen in einem Ausdruck basiert. Eine `For Each`-Anweisung gibt eine Schleifen Steuerungs Variable und einen Enumeratorausdruck an. Die Schleifen Steuerungs Variable wird entweder durch einen Bezeichner angegeben, gefolgt von einer optionalen `As`-Klausel oder einem Ausdruck.

```antlr
ForEachStatement
    : 'For' 'Each' LoopControlVariable 'In' LineTerminator? Expression StatementTerminator
      Block?
      ( 'Next' NextExpressionList? StatementTerminator )?
    ;
```

Befolgen Sie dieselben Regeln wie `For...Next`-Anweisungen (Abschnitt [für... Next-Anweisungen](statements.md#fornext-statements)), bezieht sich die Schleifen Steuerungs Variable auf eine neue lokale Variable, die für diese spezifisch ist... Next-Anweisung, oder zu einer bereits vorhandenen Variablen oder zu einem Ausdruck.

Der Enumeratorausdruck muss als Wert klassifiziert werden, und sein Typ muss ein Sammlungstyp oder `Object` sein. Wenn der Typ des Enumeratorausdrucks `Object` ist, wird die gesamte Verarbeitung bis zur Laufzeit verzögert. Andernfalls muss eine Konvertierung vom Elementtyp der Auflistung bis zum Typ der Schleifen Steuerungsvariablen vorhanden sein.

Die Schleifen Steuerungs Variable kann nicht von einer anderen einschließenden `For Each`-Anweisung verwendet werden. Eine `For Each`-Anweisung muss durch eine entsprechende `Next`-Anweisung geschlossen werden. Eine `Next`-Anweisung ohne eine Schleifen Steuerungs Variable entspricht der innersten geöffneten `For Each`. Eine `Next`-Anweisung mit einer oder mehreren Schleifen Steuerungsvariablen findet von links nach rechts mit den `For Each`-Schleifen überein, die dieselbe Schleifen Steuerungs Variable aufweisen. Wenn eine Variable mit einer `For Each`-Schleife übereinstimmt, die zu diesem Zeitpunkt nicht die am meisten gebräuchliche Schleife ist, tritt ein Kompilierzeitfehler auf.

Ein Typ `C` als *Auflistungstyp* bezeichnet wird, wenn einer der folgenden Typen ist:

* Folgendes gilt für Folgendes:
  * `C` enthält eine barrierefreie Instanz, eine freigegebene Methode oder eine Erweiterungsmethode mit der Signatur `GetEnumerator()`, die einen Typ `E` zurückgibt.
  * `E` enthält eine barrierefreie Instanz, eine freigegebene Methode oder eine Erweiterungsmethode mit der Signatur `MoveNext()` und dem Rückgabetyp `Boolean`.
  * `E` enthält eine barrierefreie Instanz oder eine freigegebene Eigenschaft mit dem Namen `Current`, die über einen Getter verfügt. Der Typ dieser Eigenschaft ist der Elementtyp des Auflistungs Typs.

* Es implementiert die-Schnittstelle `System.Collections.Generic.IEnumerable(Of T)`. in diesem Fall wird der Elementtyp der Auflistung als `T` betrachtet.

* Es implementiert die-Schnittstelle `System.Collections.IEnumerable`. in diesem Fall wird der Elementtyp der Auflistung als `Object` betrachtet.

Im folgenden finden Sie ein Beispiel für eine Klasse, die aufgelistet werden kann:

```vb
Public Class IntegerCollection
    Private integers(10) As Integer

    Public Class IntegerCollectionEnumerator
        Private collection As IntegerCollection
        Private index As Integer = -1

        Friend Sub New(c As IntegerCollection)
            collection = c
        End Sub

        Public Function MoveNext() As Boolean
            index += 1

            Return index <= 10
        End Function

        Public ReadOnly Property Current As Integer
            Get
                If index < 0 OrElse index > 10 Then
                    Throw New System.InvalidOperationException()
                End If

                Return collection.integers(index)
            End Get
        End Property
    End Class

    Public Sub New()
        Dim i As Integer

        For i = 0 To 10
            integers(i) = I
        Next i
    End Sub

    Public Function GetEnumerator() As IntegerCollectionEnumerator
        Return New IntegerCollectionEnumerator(Me)
    End Function
End Class
```

Bevor die Schleife beginnt, wird der Enumeratorausdruck ausgewertet. Wenn der Typ des Ausdrucks das Entwurfsmuster nicht erfüllt, wird der Ausdruck in `System.Collections.IEnumerable` oder `System.Collections.Generic.IEnumerable(Of T)` umgewandelt. Wenn der Ausdruckstyp die generische Schnittstelle implementiert, wird die generische Schnittstelle zur Kompilierzeit bevorzugt, aber die nicht generische Schnittstelle wird zur Laufzeit bevorzugt. Wenn der Ausdruckstyp die generische Schnittstelle mehrmals implementiert, wird die Anweisung als mehrdeutig angesehen und ein Kompilierzeitfehler auftritt.

__Nebenbei.__ Die nicht generische Schnittstelle wird im spät gebundenen Fall bevorzugt, da das Auswählen der generischen Schnittstelle bedeuten würde, dass alle Aufrufe der Schnittstellen Methoden Typparameter einschließen würden. Da es nicht möglich ist, die übereinstimmenden Typargumente zur Laufzeit zu erkennen, müssten alle diese Aufrufe mithilfe von spät gebundenen aufrufen erfolgen. Dies ist langsamer als das Aufrufen der nicht generischen-Schnittstelle, da die nicht generische-Schnittstelle mithilfe von Kompilierzeit aufrufen aufgerufen werden kann.

`GetEnumerator` wird für den resultierenden Wert aufgerufen, und der Rückgabewert der Funktion wird in einem temporären gespeichert. Am Anfang jeder Iterationen wird `MoveNext` für den temporären aufgerufen. Wenn `False` zurückgegeben wird, wird die Schleife beendet. Andernfalls wird jede Iterations Schleife wie folgt ausgeführt:

1. Wenn die Schleifen Steuerungs Variable eine neue lokale Variable (anstatt eine bereits vorhandene) identifiziert hat, wird eine neue Kopie dieser lokalen Variablen erstellt. Bei der aktuellen Iterationen verweisen alle Verweise innerhalb des Schleifen Blocks auf diese Kopie.
2. Die `Current`-Eigenschaft wird abgerufen und in den Typ der Schleifen Steuerungsvariablen umgewandelt (unabhängig davon, ob die Konvertierung implizit oder explizit erfolgt) und der Schleifen Steuerungsvariablen zugewiesen.
3. Der Schleifen Block wird ausgeführt.

__Nebenbei.__ Es gibt eine geringfügige Änderung des Verhaltens zwischen Version 10,0 und 11,0 der Sprache. Vor 11,0 wurde für jede Iterations Schleife *keine* neue Iterations Variable erstellt. Dieser Unterschied ist nur sichtbar, wenn die Iterations Variable von einem Lambda-oder LINQ-Ausdruck aufgezeichnet wird, der dann nach der-Schleife aufgerufen wird:

```vb
Dim lambdas As New List(Of Action)
For Each x In {1,2,3}
   lambdas.Add(Sub() Console.WriteLine(x)
Next
lambdas(0).Invoke()
lambdas(1).Invoke()
lambdas(2).Invoke()
```

Bis zu Visual Basic 10,0 wurde eine Warnung zur Kompilierzeit erzeugt, und "3" wurde dreimal gedruckt. Der Grund war, dass nur eine einzelne "x"-Variable von allen Iterationen der Schleife gemeinsam verwendet wurde, und alle drei Lambdas haben dasselbe "x" erfasst, und zum Zeitpunkt der Ausführung der Lambda-Ausdrücke enthielt Sie die Zahl 3. Ab Visual Basic 11,0 wird "1, 2, 3" ausgegeben. Dies liegt daran, dass jeder Lambda-Ausdruck eine andere Variable "x" erfasst.


__Nebenbei.__ Das aktuelle Element der Iterationen wird in den Typ der Schleifen Steuerungsvariablen konvertiert, auch wenn die Konvertierung explizit ist, da es keine bequeme Stelle gibt, um einen Konvertierungs Operator in der Anweisung einzuführen. Dies war besonders problematisch beim Arbeiten mit dem mittlerweile veralteten Typ `System.Collections.ArrayList`, da der Elementtyp `Object` ist. Dies hätte die erforderlichen Umwandlungen in einer großen Zahl von Schleifen enthalten, was uns als ideal empfunden hat. Ironischerweise ermöglichte Generika das Erstellen einer stark typisierten Auflistung, `System.Collections.Generic.List(Of T)`, was uns möglicherweise zum überdenken dieses Entwurfs Punkts geführt hat, aber aus Kompatibilitätsgründen kann dies jetzt nicht geändert werden.


Wenn die `Next`-Anweisung erreicht wird, wird die Ausführung an den Anfang der Schleife zurückgegeben. Wenn eine Variable nach dem `Next`-Schlüsselwort angegeben wird, muss Sie mit der ersten Variablen nach `For Each` identisch sein. Beachten Sie z. B. folgenden Code:

```vb
Module Test
    Sub Main()
        Dim i As Integer
        Dim c As IntegerCollection = New IntegerCollection()

        For Each i In c
            Console.WriteLine(i)
        Next i
    End Sub
End Module
```

Dies entspricht dem folgenden Code:

```vb
Module Test
    Sub Main()
        Dim i As Integer
        Dim c As IntegerCollection = New IntegerCollection()

        Dim e As IntegerCollection.IntegerCollectionEnumerator

        e = c.GetEnumerator()
        While e.MoveNext()
            i = e.Current

            Console.WriteLine(i)
        End While
    End Sub
End Module
```

Wenn der Typ `E` des Enumerators `System.IDisposable` implementiert, wird der Enumerator beim Beenden der Schleife gelöscht, indem die `Dispose`-Methode aufgerufen wird. Dadurch wird sichergestellt, dass die vom Enumerator gehaltenen Ressourcen freigegeben werden. Wenn die Methode, die die `For Each`-Anweisung enthält, keine unstrukturierte Fehlerbehandlung verwendet, wird die `For Each`-Anweisung in eine `Try`-Anweisung umschließt, wobei die `Dispose`-Methode in den `Finally` aufgerufen wird, um die Bereinigung sicherzustellen.

__Nebenbei.__ Der `System.Array`-Typ ist ein Sammlungstyp, und da alle Array Typen von `System.Array` abgeleitet sind, ist jeder arraytypausdruck in einer `For Each`-Anweisung zulässig. Bei eindimensionalen Arrays listet die `For Each`-Anweisung die Array Elemente in zunehmender Index Reihenfolge auf, beginnend mit Index 0 und endet mit Indexlänge-1. Bei mehrdimensionalen Arrays werden die Indizes der äußersten rechten Dimension zuerst angehoben.

Beispielsweise druckt der folgende Code `1 2 3 4`:

```vb
Module Test
    Sub Main()
        Dim x(,) As Integer = { { 1, 2 }, { 3, 4 } }
        Dim i As Integer

        For Each i In x
            Console.Write(i & " ")
        Next i
    End Sub
End Module
```

Es ist nicht zulässig, von außerhalb des Blocks in einen `For Each`-Anweisungsblock zu verzweigen.


## <a name="exception-handling-statements"></a>Ausnahme behandlungsanweisungen

Visual Basic unterstützt die strukturierte Ausnahmebehandlung und die unstrukturierte Ausnahmebehandlung. In einer Methode kann nur ein Stil der Ausnahmebehandlung verwendet werden, aber die `Error`-Anweisung kann bei der strukturierten Ausnahmebehandlung verwendet werden. Wenn eine Methode beide Arten der Ausnahmebehandlung verwendet, wird ein Fehler bei der Kompilierzeit ausgegeben.

```antlr
ErrorHandlingStatement
    : StructuredErrorStatement
    | UnstructuredErrorStatement
    ;
```

### <a name="structured-exception-handling-statements"></a>Strukturierte Ausnahme behandlungsanweisungen

Die strukturierte Ausnahmebehandlung ist eine Methode zur Behandlung von Fehlern durch Deklarieren expliziter Blöcke, in denen bestimmte Ausnahmen behandelt werden. Die strukturierte Ausnahmebehandlung erfolgt über eine `Try`-Anweisung.

```antlr
StructuredErrorStatement
    : ThrowStatement
    | TryStatement
    ;

TryStatement
    : 'Try' StatementTerminator
      Block?
      CatchStatement*
      FinallyStatement?
      'End' 'Try' StatementTerminator
    ;
```


Zum Beispiel:

```vb
Module Test
    Sub ThrowException()
        Throw New Exception()
    End Sub

    Sub Main()
        Try
            ThrowException()
        Catch e As Exception
            Console.WriteLine("Caught exception!")
        Finally
            Console.WriteLine("Exiting try.")
        End Try
    End Sub
End Module
```

Eine `Try`-Anweisung besteht aus drei Arten von Blöcken: Try-Blöcke, catch-Blöcke und schließlich-Blöcke. Ein *try-Block* ist ein Anweisungsblock, der die auszuführenden-Anweisungen enthält. Ein *catch-Block* ist ein Anweisungsblock, der eine Ausnahme behandelt. Ein letztes *Block* ist ein Anweisungsblock, der-Anweisungen enthält, die ausgeführt werden, wenn die `Try`-Anweisung beendet wird, unabhängig davon, ob eine Ausnahme aufgetreten ist und behandelt wurde. Eine `Try`-Anweisung, die nur einen try-Block und einen schließlich-Block enthalten kann, muss mindestens einen catch-Block oder schließlich einen Block enthalten. Es ist ungültig, die Ausführung explizit in einen try-Block zu übertragen, mit Ausnahme von innerhalb eines catch-Blocks in derselben Anweisung.


#### <a name="finally-blocks"></a>Finally-Blöcke

Ein `Finally`-Block wird immer ausgeführt, wenn die Ausführung einen beliebigen Teil der `Try`-Anweisung verlässt. Es ist keine explizite Aktion erforderlich, um den `Finally`-Block auszuführen. Wenn die Ausführung die `Try`-Anweisung verlässt, führt das System automatisch den Block "`Finally`" aus und überträgt anschließend die Ausführung an das gewünschte Ziel. Der `Finally`-Block wird ausgeführt, unabhängig davon, wie die Ausführung die `Try`-Anweisung verlässt: über das Ende des `Try`-Blocks bis zum Ende eines `Catch`-Blocks, über eine `Exit Try`-Anweisung, über eine `GoTo`-Anweisung oder durch die Behandlung einer ausgelösten Ausnahme.

Beachten Sie, dass der `Await`-Ausdruck in einer Async-Methode und die `Yield`-Anweisung in einer Iteratormethode dazu führen können, dass die Ablauf Steuerung in der Async-oder Iterator-Methoden Instanz angehalten und in einer anderen Methoden Instanz fortgesetzt werden kann. Dies ist jedoch nur eine Unterbrechung der Ausführung und umfasst nicht das Beenden der entsprechenden asynchronen Methode oder iteratormethodeninstanz, weshalb keine `Finally`-Blöcke ausgeführt werden.

Die explizite Übertragung der Ausführung in einen `Finally`-Block ist ungültig. Es ist auch ungültig, die Ausführung aus einem `Finally`-Block außer über eine Ausnahme zu übertragen.

```antlr
FinallyStatement
    : 'Finally' StatementTerminator
      Block?
    ;
```

#### <a name="catch-blocks"></a>catch-Blöcke

Wenn bei der Verarbeitung des `Try`-Blocks eine Ausnahme auftritt, wird jede `Catch`-Anweisung in der Text Reihenfolge untersucht, um zu bestimmen, ob die Ausnahme behandelt wird.

```antlr
CatchStatement
    : 'Catch' ( Identifier ( 'As' NonArrayTypeName )? )?
      ( 'When' BooleanExpression )? StatementTerminator
      Block?
    ;
```

Der in einer `Catch`-Klausel angegebene Bezeichner stellt die ausgelöste Ausnahme dar. Wenn der Bezeichner eine `As`-Klausel enthält, wird der Bezeichner als innerhalb des lokalen Deklarations Raums des `Catch`-Blocks deklariert. Andernfalls muss der Bezeichner eine lokale Variable (keine statische Variable) sein, die in einem enthaltenden Block definiert wurde.

Eine `Catch`-Klausel ohne Bezeichner fängt alle Ausnahmen ab, die von `System.Exception` abgeleitet werden. Eine `Catch`-Klausel mit einem Bezeichner fängt nur Ausnahmen ab, deren Typen mit dem Bezeichnertyp übereinstimmen oder davon abgeleitet sind. Der Typ muss `System.Exception` oder ein von `System.Exception` abgeleiteter Typ sein. Wenn eine Ausnahme abgefangen wird, die von `System.Exception` abgeleitet ist, wird ein Verweis auf das Ausnahme Objekt in dem Objekt gespeichert, das von der Funktion `Microsoft.VisualBasic.Information.Err` zurückgegeben wird.

Eine `Catch`-Klausel mit einer `When`-Klausel fängt nur Ausnahmen ab, wenn der Ausdruck zu `True`; ausgewertet wird. der Typ des Ausdrucks muss ein boolescher Ausdruck sein, wie [boolesche Ausdrücke](expressions.md#boolean-expressions)pro Abschnitt. Eine `When`-Klausel wird nur angewendet, nachdem der Typ der Ausnahme überprüft wurde, und der Ausdruck verweist möglicherweise auf den Bezeichner, der die Ausnahme darstellt, wie in diesem Beispiel veranschaulicht:

```vb
Module Test
    Sub Main()
        Dim i As Integer = 5

        Try
            Throw New ArgumentException()
        Catch e As OverflowException When i = 5
            Console.WriteLine("First handler")
        Catch e As ArgumentException When i = 4
            Console.WriteLine("Second handler")
        Catch When i = 5
            Console.WriteLine("Third handler")
        End Try

    End Sub
End Module
```

In diesem Beispiel wird Folgendes gedruckt:

```console
Third handler
```

Wenn eine `Catch`-Klausel die Ausnahme behandelt, wird die Ausführung an den `Catch`-Block übertragen. Am Ende des `Catch`-Blocks wird die Ausführung an die erste Anweisung nach der `Try`-Anweisung übertragen. Die `Try`-Anweisung behandelt keine Ausnahmen, die in einem `Catch`-Block ausgelöst werden. Wenn die Ausnahme von keiner `Catch`-Klausel behandelt wird, wird die Ausführung an einen vom System festgelegten Speicherort übertragen.

Die explizite Übertragung der Ausführung in einen `Catch`-Block ist ungültig.

Die Filter in WHEN-Klauseln werden normalerweise ausgewertet, bevor die Ausnahme ausgelöst wird. Beispielsweise druckt der folgende Code "Filter, abschließend, catch".

```vb
Sub Main()
   Try
       Foo()
   Catch ex As Exception When F()
       Console.WriteLine("Catch")
   End Try
End Sub

Sub Foo()
    Try
        Throw New Exception
    Finally
        Console.WriteLine("Finally")
    End Try
End Sub

Function F() As Boolean
    Console.WriteLine("Filter")
    Return True
End Function
```

 Asynchrone und Iteratormethoden bewirken jedoch, dass alle schließlich darin enthaltenen Blöcke vor allen Filtern außerhalb von ausgeführt werden. Wenn der obige Code beispielsweise `Async Sub Foo()` hätte, wäre die Ausgabe "endlich, Filter, catch".


#### <a name="throw-statement"></a>Throw-Anweisung

Die `Throw`-Anweisung löst eine Ausnahme aus, die durch eine Instanz eines Typs dargestellt wird, der von `System.Exception` abgeleitet ist.

```antlr
ThrowStatement
    : 'Throw' Expression? StatementTerminator
    ;
```

Wenn der Ausdruck nicht als Wert klassifiziert ist oder kein von `System.Exception` abgeleiteter Typ ist, tritt ein Kompilierzeitfehler auf. Wenn der Ausdruck zur Laufzeit zu einem NULL-Wert ausgewertet wird, wird stattdessen eine Ausnahme vom Typ "`System.NullReferenceException`" ausgelöst.

Eine `Throw`-Anweisung kann den Ausdruck innerhalb eines catch-Blocks einer `Try`-Anweisung weglassen, solange kein dazwischenliegende Block vorhanden ist. In diesem Fall löst die-Anweisung die Ausnahme erneut aus, die gerade im catch-Block behandelt wird. Zum Beispiel:

```vb
Sub Test(x As Integer)
    Try
        Throw New Exception()
    Catch
        If x = 0 Then
            Throw    ' OK, rethrows exception from above.
        Else
            Try
                If x = 1 Then
                    Throw   ' OK, rethrows exception from above.
                End If
            Finally
                Throw    ' Invalid, inside of a Finally.
            End Try
        End If
    End Try
End Sub
```


### <a name="unstructured-exception-handling-statements"></a>Unstrukturierte Ausnahme behandlungsanweisungen

Bei der unstrukturierten Ausnahmebehandlung handelt es sich um eine Methode zur Behandlung von Fehlern, die beim Auftreten einer Ausnahme beim Auftreten von Anweisungen zum Verzweigen Die unstrukturierte Ausnahmebehandlung wird mithilfe von drei Anweisungen implementiert: der `Error`-Anweisung, der `On Error`-Anweisung und der `Resume`-Anweisung.

```antlr
UnstructuredErrorStatement
    : ErrorStatement
    | OnErrorStatement
    | ResumeStatement
    ;
```

Zum Beispiel:

```vb
Module Test
    Sub ThrowException()
        Error 5
    End Sub

    Sub Main()
        On Error GoTo GotException

        ThrowException()
        Exit Sub

GotException:
        Console.WriteLine("Caught exception!")
        Resume Next
    End Sub
End Module
```

Wenn eine Methode eine unstrukturierte Ausnahmebehandlung verwendet, wird ein einzelner strukturierter Ausnahmehandler für die gesamte Methode eingerichtet, die alle Ausnahmen abfängt. (Beachten Sie, dass sich dieser Handler in Konstruktoren nicht über den aufzurufenden Aufrufe von `New` am Anfang des Konstruktors erstreckt.) Die-Methode verfolgt dann den letzten Speicherort des Ausnahme Handlers und die letzte Ausnahme, die ausgelöst wurde. Beim Einstieg in die-Methode werden der Speicherort des Ausnahme Handlers und die Ausnahme auf `Nothing` festgelegt. Wenn eine Ausnahme in einer Methode ausgelöst wird, die eine unstrukturierte Ausnahmebehandlung verwendet, wird ein Verweis auf das Ausnahme Objekt in dem Objekt gespeichert, das von der Funktion zurückgegeben wird `Microsoft.VisualBasic.Information.Err`.

Unstrukturierte Fehler behandlungsanweisungen sind in Iterator-oder Async-Methoden nicht zulässig.


#### <a name="error-statement"></a>Error-Anweisung

Eine `Error`-Anweisung löst eine `System.Exception`-Ausnahme aus, die eine Visual Basic 6-Ausnahme Nummer enthält. Der Ausdruck muss als Wert klassifiziert werden, und sein Typ muss implizit in `Integer` konvertiert werden können.

```antlr
ErrorStatement
    : 'Error' Expression StatementTerminator
    ;
```

#### <a name="on-error-statement"></a>On Error-Anweisung

Eine `On Error`-Anweisung ändert den letzten Ausnahme Behandlungs Zustand.

```antlr
OnErrorStatement
    : 'On' 'Error' ErrorClause StatementTerminator
    ;

ErrorClause
    : 'GoTo' '-' '1'
    | 'GoTo' '0'
    | GoToStatement
    | 'Resume' 'Next'
    ;
```

Sie kann auf eine von vier Arten verwendet werden:

* `On Error GoTo -1` setzt die letzte Ausnahme auf `Nothing` zurück.

* `On Error GoTo 0` setzt den letzten Speicherort des Ausnahme Handlers auf `Nothing` zurück.

* `On Error GoTo LabelName` legt die Bezeichnung als den letzten Speicherort des Ausnahme Handlers fest. Diese Anweisung kann nicht in einer Methode verwendet werden, die einen Lambda-oder Abfrage Ausdruck enthält.

* `On Error Resume Next` erstellt das `Resume Next`-Verhalten als den letzten Speicherort des Ausnahme Handlers.


#### <a name="resume-statement"></a>Resume-Anweisung

Eine `Resume`-Anweisung gibt die Ausführung an die-Anweisung zurück, die die letzte Ausnahme ausgelöst hat.

```antlr
ResumeStatement
    : 'Resume' ResumeClause? StatementTerminator
    ;

ResumeClause
    : 'Next'
    | LabelName
    ;
```

Wenn der `Next`-Modifizierer angegeben ist, wird die Ausführung an die Anweisung zurückgegeben, die nach der Anweisung ausgeführt worden wäre, die die letzte Ausnahme verursacht hat. Wenn ein Bezeichnungs Name angegeben ist, wird die Ausführung an die Bezeichnung zurückgegeben.

Da die `SyncLock`-Anweisung einen impliziten strukturierten Fehler Behandlungs Block enthält, haben `Resume` und `Resume Next` besondere Verhaltensweisen für Ausnahmen, die in `SyncLock`-Anweisungen auftreten. `Resume` gibt die Ausführung bis zum Anfang der `SyncLock`-Anweisung zurück, während `Resume Next` die Ausführung an die nächste Anweisung nach der `SyncLock`-Anweisung zurückgibt. Beachten Sie z. B. folgenden Code:

```vb
Class LockClass
End Class

Module Test
    Sub Main()
        Dim FirstTime As Boolean = True
        Dim Lock As LockClass = New LockClass()

        On Error GoTo Handler

        SyncLock Lock
            Console.WriteLine("Before exception")
            Throw New Exception()
            Console.WriteLine("After exception")
        End SyncLock

        Console.WriteLine("After SyncLock")
        Exit Sub

Handler:
        If FirstTime Then
            FirstTime = False
            Resume
        Else
            Resume Next
        End If
    End Sub
End Module
```

Das folgende Ergebnis wird ausgegeben.

```console
Before exception
Before exception
After SyncLock
```

Beim ersten Mal durch die `SyncLock`-Anweisung gibt `Resume` die Ausführung bis zum Anfang der `SyncLock`-Anweisung zurück. Beim zweiten Mal durch die `SyncLock`-Anweisung gibt `Resume Next` die Ausführung bis zum Ende der `SyncLock`-Anweisung zurück. `Resume` und `Resume Next` sind innerhalb einer `SyncLock`-Anweisung nicht zulässig.

Wenn eine `Resume`-Anweisung ausgeführt wird, wird in allen Fällen die letzte Ausnahme auf `Nothing` festgelegt. Wenn eine `Resume`-Anweisung ohne letzte Ausnahme ausgeführt wird, löst die Anweisung eine `System.Exception`-Ausnahme aus, die die Visual Basic Fehlernummer `20` (ohne Fehler fortsetzen) enthält.


## <a name="branch-statements"></a>Verzweigungs Anweisungen

Branchanweisungen ändern den Ausführungs Ablauf in einer Methode. Es gibt sechs Verzweigungs Anweisungen:

1. Eine `GoTo`-Anweisung bewirkt, dass die Ausführung in der-Methode auf die angegebene Bezeichnung übertragen wird. Es ist nicht zulässig,-0 in einen `Try`-, `Using`-, `SyncLock`-, `With`-, `For`-oder `For Each`-Block zu @no__t, und auch nicht in einen Schleifen Block, wenn eine lokale Variable dieses Blocks in einem Lambda-oder LINQ-Ausdruck aufgezeichnet wird.
2. Eine `Exit`-Anweisung überträgt die Ausführung an die nächste Anweisung nach dem Ende der direkt enthaltenden Block Anweisung der angegebenen Art. Wenn der Block der Methoden Block ist, beendet die Ablauf Steuerung die Methode, wie im Abschnitt Ablauf [Steuerung](statements.md#control-flow)beschrieben. Wenn die `Exit`-Anweisung nicht in der Art von Block enthalten ist, der in der-Anweisung angegeben ist, tritt ein Kompilierzeitfehler auf.
3. Eine `Continue`-Anweisung überträgt die Ausführung an das Ende der unmittelbar enthaltenden Block Schleifen Anweisung der angegebenen Art. Wenn die `Continue`-Anweisung nicht in der Art von Block enthalten ist, der in der-Anweisung angegeben ist, tritt ein Kompilierzeitfehler auf.
4. Eine `Stop`-Anweisung bewirkt, dass eine Debugger-Ausnahme auftritt.
5. Mit einer `End`-Anweisung wird das Programm beendet. Finalizer werden vor dem Herunterfahren ausgeführt, aber die letzten Blöcke aller aktuell ausgeführten `Try`-Anweisungen werden nicht ausgeführt. Diese Anweisung kann nicht in Programmen verwendet werden, die keine ausführbare Datei sind (z. b. DLLs).
6. Eine `Return`-Anweisung ohne Ausdruck entspricht einer `Exit Sub`-oder `Exit Function`-Anweisung. Eine `Return`-Anweisung mit einem Ausdruck ist nur in einer regulären Methode zulässig, die eine Funktion ist, oder in einer asynchronen Methode, bei der es sich um eine Funktion mit dem Rückgabetyp `Task(Of T)` für einige `T` handelt. Der Ausdruck muss als Wert klassifiziert werden, der implizit in die *Funktions Rückgabe Variable* (im Fall regulärer Methoden) oder in die *Aufgaben Rückgabe Variable* (im Fall von Async-Methoden) konvertiert werden kann. Das Verhalten besteht darin, den Ausdruck auszuwerten, ihn in der Rückgabe Variablen zu speichern und dann eine implizite `Exit Function`-Anweisung auszuführen.

```antlr
BranchStatement
    : GoToStatement
    | ExitStatement
    | ContinueStatement
    | StopStatement
    | EndStatement
    | ReturnStatement
    ;

GoToStatement
    : 'GoTo' LabelName StatementTerminator
    ;

ExitStatement
    : 'Exit' ExitKind StatementTerminator
    ;

ExitKind
    : 'Do' | 'For' | 'While' | 'Select' | 'Sub' | 'Function' | 'Property' | 'Try'
    ;

ContinueStatement
    : 'Continue' ContinueKind StatementTerminator
    ;

ContinueKind
    : 'Do' | 'For' | 'While'
    ;

StopStatement
    : 'Stop' StatementTerminator
    ;

EndStatement
    : 'End' StatementTerminator
    ;

ReturnStatement
    : 'Return' Expression? StatementTerminator
    ;
```

## <a name="array-handling-statements"></a>Array behandlungsanweisungen

Zwei Anweisungen vereinfachen das Arbeiten mit Arrays: `ReDim`-Anweisungen und `Erase`-Anweisungen.

```antlr
ArrayHandlingStatement
    : RedimStatement
    | EraseStatement
    ;
```

### <a name="redim-statement"></a>ReDim-Anweisung

Eine `ReDim`-Anweisung instanziiert neue Arrays.

```antlr
RedimStatement
    : 'ReDim' 'Preserve'? RedimClauses StatementTerminator
    ;

RedimClauses
    : RedimClause ( Comma RedimClause )*
    ;

RedimClause
    : Expression ArraySizeInitializationModifier
    ;
```

Jede Klausel in der Anweisung muss als Variable oder Eigenschafts Zugriff klassifiziert werden, deren Typ ein Arraytyp oder `Object` ist, und eine Liste von Array Begrenzungen folgt. Die Anzahl der Begrenzungen muss mit dem Typ der Variablen übereinstimmen. eine beliebige Anzahl von Begrenzungen ist für `Object` zulässig. Zur Laufzeit wird ein Array für jeden Ausdruck von links nach rechts mit den angegebenen Begrenzungen instanziiert und dann der Variablen oder Eigenschaft zugewiesen. Wenn der Variablentyp `Object` ist, entspricht die Anzahl der Dimensionen der angegebenen Anzahl von Dimensionen, und der Array Elementtyp ist `Object`. Wenn die angegebene Anzahl von Dimensionen zur Laufzeit nicht mit der Variablen oder Eigenschaft kompatibel ist, tritt ein Kompilierzeitfehler auf. Zum Beispiel:

```vb
Module Test
    Sub Main()
        Dim o As Object
        Dim b() As Byte
        Dim i(,) As Integer

        ' The next two statements are equivalent.
        ReDim o(10,30)
        o = New Object(10, 30) {}

        ' The next two statements are equivalent.
        ReDim b(10)
        b = New Byte(10) {}

        ' Error: Incorrect number of dimensions.
        ReDim i(10, 30, 40)
    End Sub
End Module
```

Wenn das `Preserve`-Schlüsselwort angegeben ist, müssen die Ausdrücke auch als-Wert klassifiziert werden, und die neue Größe für jede Dimension, mit Ausnahme von ganz rechts, muss mit der Größe des vorhandenen Arrays identisch sein. Die Werte im vorhandenen Array werden in das neue Array kopiert: Wenn das neue Array kleiner ist, werden die vorhandenen Werte verworfen. Wenn das neue Array größer ist, werden die zusätzlichen Elemente mit dem Standardwert des Elementtyps des Arrays initialisiert. Beachten Sie z. B. folgenden Code:

```vb
Module Test
    Sub Main()
        Dim x(5, 5) As Integer

        x(3, 3) = 3

        ReDim Preserve x(5, 6)
        Console.WriteLine(x(3, 3) & ", " & x(3, 6))
    End Sub
End Module
```

Es gibt das folgende Ergebnis aus:

```console
3, 0
```

Wenn der vorhandene Array Verweis zur Laufzeit ein NULL-Wert ist, wird kein Fehler angegeben. Wenn sich die Größe einer Dimension ändert, wird eine `System.ArrayTypeMismatchException` ausgelöst.

__Nebenbei.__ `Preserve` ist kein reserviertes Wort.


### <a name="erase-statement"></a>Erase-Anweisung

Eine `Erase`-Anweisung legt jede der in der-Anweisung angegebenen Array Variablen oder Eigenschaften auf `Nothing` fest. Jeder Ausdruck in der Anweisung muss als Variablen oder Eigenschafts Zugriff klassifiziert werden, deren Typ ein Arraytyp oder `Object` ist. Zum Beispiel:

```vb
Module Test
    Sub Main()
        Dim x() As Integer = New Integer(5) {}

        ' The following two statements are equivalent.
        Erase x
        x = Nothing
    End Sub
End Module
```

```antlr
EraseStatement
    : 'Erase' EraseExpressions StatementTerminator
    ;

EraseExpressions
    : Expression ( Comma Expression )*
    ;
```

## <a name="using-statement"></a>Using-Anweisung

Instanzen von Typen werden automatisch vom Garbage Collector freigegeben, wenn eine Auflistung ausgeführt wird und keine Live Verweise auf die Instanz gefunden werden. Wenn ein Typ in einer besonders wertvollen und knappen Ressource (z. b. Datenbankverbindungen oder Datei Handles) enthalten ist, ist es möglicherweise nicht wünschenswert, bis zum nächsten Garbage Collection eine bestimmte Instanz des Typs zu bereinigen, der nicht mehr verwendet wird. Um eine einfachere Methode zum Freigeben von Ressourcen vor einer Auflistung bereitzustellen, kann ein Typ die `System.IDisposable`-Schnittstelle implementieren. Ein Typ, der eine `Dispose`-Methode verfügbar macht, die aufgerufen werden kann, um zu erzwingen, dass wertvolle Ressourcen sofort freigegeben werden, wie z. b.:

```vb
Module Test
    Sub Main()
        Dim x As DBConnection = New DBConnection("...")

        ' Do some work
        ...

        x.Dispose()        ' Free the connection
    End Sub
End Module
```

Die `Using`-Anweisung automatisiert das Abrufen einer Ressource, das Ausführen eines Satzes von Anweisungen und das anschließende verwerfen der Ressource. Die-Anweisung kann zwei Formen annehmen: in einer ist die Ressource eine lokale Variable, die als Teil der-Anweisung deklariert und als reguläre Anweisung der lokalen Variablen Deklaration behandelt wird. in der anderen ist die Ressource das Ergebnis eines Ausdrucks.

```antlr
UsingStatement
    : 'Using' UsingResources StatementTerminator
      Block?
      'End' 'Using' StatementTerminator
    ;

UsingResources
    : VariableDeclarators
    | Expression
    ;
```

Wenn die Ressource eine Deklaration der lokalen Variablen Deklaration ist, muss der Typ der lokalen Variablen Deklaration ein Typ sein, der implizit in `System.IDisposable` konvertiert werden kann. Die deklarierten lokalen Variablen sind schreibgeschützt, sind auf den `Using`-Anweisungsblock festgelegt und müssen einen Initialisierer enthalten. Wenn die Ressource das Ergebnis eines Ausdrucks ist, muss der Ausdruck als Wert klassifiziert werden und muss einen Typ aufweisen, der implizit in `System.IDisposable` konvertiert werden kann. Der Ausdruck wird nur einmal am Anfang der Anweisung ausgewertet.

Der `Using`-Block ist implizit in einer `Try`-Anweisung enthalten, deren schließlich-Block die-Methode `IDisposable.Dispose` für die Ressource aufruft. Dadurch wird sichergestellt, dass die Ressource auch dann verworfen wird, wenn eine Ausnahme ausgelöst wird. Daher ist es ungültig, von außerhalb des Blocks in einen `Using`-Block zu verzweigen, und ein `Using`-Block wird als einzelne Anweisung für `Resume` und `Resume Next` behandelt. Wenn die Ressource `Nothing` ist, wird kein "`Dispose`" aufgerufen. Das Beispiel:

```vb
Using f As C = New C()
    ...
End Using
```

identisch mit folgendem Ausdruck:

```vb
Dim f As C = New C()
Try
    ...
Finally
    If f IsNot Nothing Then
        f.Dispose()
    End If
End Try
```

Eine `Using`-Anweisung, die eine Deklaration der lokalen Variablen Deklaration aufweist, kann mehrere Ressourcen gleichzeitig abrufen. Dies entspricht der Anweisung von `Using`-Anweisungen.  Beispielsweise eine `Using`-Anweisung in der Form:

```vb
Using r1 As R = New R(), r2 As R = New R()
    r1.F()
    r2.F()
End Using
```

identisch mit folgendem Ausdruck:

```vb
Using r1 As R = New R()
    Using r2 As R = New R()
        r1.F()
        r2.F()
    End Using
End Using
```


## <a name="await-statement"></a>Erwartungs Anweisung

Eine Erwartungs Anweisung hat dieselbe Syntax wie der Ausdruck eines Erwartungs Operators (Section-Erwartungs [Operator](expressions.md#await-operator)), ist nur in Methoden zulässig, die ebenfalls Erwartungs Ausdrücke zulassen, und weist das gleiche Verhalten wie ein Erwartungs Operator Ausdruck auf.

Sie kann jedoch entweder als Wert oder als void klassifiziert werden. Alle Werte, die sich aus der Auswertung des Ausdrucks des Erwartungs Operators ergeben, werden verworfen.

```antlr
AwaitStatement
    : AwaitOperatorExpression StatementTerminator
    ;
```

## <a name="yield-statement"></a>Yield-Anweisung

Yield-Anweisungen beziehen sich auf Iteratormethoden, die im Abschnitt [Iteratormethoden](statements.md#iterator-methods)beschrieben werden.

```antlr
YieldStatement
    : 'Yield' Expression StatementTerminator
    ;
```

`Yield` ist ein reserviertes Wort, wenn die unmittelbar einschließende Methode oder der Lambda-Ausdruck, in dem Sie angezeigt wird, einen `Iterator`-Modifizierer aufweist und der `Yield` nach diesem `Iterator`-Modifizierer erscheint. Es ist nicht an anderer Stelle reserviert. Es ist auch nicht in Präprozessordirektiven reserviert. Die yield-Anweisung ist nur im Text einer Methode oder eines Lambda-Ausdrucks zulässig, bei der es sich um ein reserviertes Wort handelt. Innerhalb der unmittelbar einschließenden Methode oder des Lambda-Ausdrucks tritt die yield-Anweisung möglicherweise nicht innerhalb des Texts eines `Catch`-oder `Finally`-Blocks oder im Text einer `SyncLock`-Anweisung auf.

Die yield-Anweisung nimmt einen einzelnen Ausdruck an, der als Wert klassifiziert werden muss und dessen Typ implizit in den Typ der *aktuellen Variable des Iterators* (Abschnitts [Iteratormethoden](statements.md#iterator-methods)) der umschließenden Iteratormethode konvertiert werden kann.

Die Ablauf Steuerung erreicht immer nur eine `Yield`-Anweisung, wenn die `MoveNext`-Methode für ein Iteratorobjekt aufgerufen wird. (Dies ist darauf zurückzuführen, dass eine iteratormethodeninstanz nur die Anweisungen ausführt, weil die `MoveNext`-oder `Dispose`-Methoden für ein Iteratorobjekt aufgerufen werden. die `Dispose`-Methode führt nur Code in `Finally`-Blöcken aus, wobei `Yield` nicht zulässig ist).

Wenn eine `Yield`-Anweisung ausgeführt wird, wird der Ausdruck ausgewertet und in der *aktuellen Iterator-Variablen* der iteratormethodeninstanz gespeichert, die diesem Iteratorobjekt zugeordnet ist. Der Wert `True` wird an den aufrufende Instanz von `MoveNext` zurückgegeben, und der Kontrollpunkt dieser Instanz hält den Fortschritt bis zum nächsten Aufruf von `MoveNext` für das Iteratorobjekt an.

