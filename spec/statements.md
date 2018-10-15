# <a name="statements"></a>Anweisungen

Anweisungen darstellen ausführbaren Code.

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

__Beachten Sie.__ Microsoft Visual Basic-Compiler lässt nur Anweisungen, die mit einem Schlüsselwort oder einen Bezeichner zu starten. Also z. B. der aufrufanweisung "`Call (Console).WriteLine`"ist zulässig, aber der aufrufanweisung"`(Console).WriteLine`" nicht.

## <a name="control-flow"></a>Ablaufsteuerung

*Ablaufsteuerung* ist die Sequenz, die in der Anweisungen und Ausdrücke ausgeführt werden. Die Reihenfolge der Ausführung hängt davon ab, die bestimmte Anweisung oder den Ausdruck.

Beispielsweise wird beim Auswerten der Addition-Operator (Abschnitt [Additionsoperator](expressions.md#addition-operator)), zuerst der linke Operand ausgewertet wird, klicken Sie dann der Rechte Operand, und klicken Sie dann der Operator selbst. -Blocke ausgeführt werden (Abschnitt [Blöcke und Bezeichnungen](statements.md#blocks-and-labels)) zuerst Ausführen ihrer ersten-unteranweisung ist nicht möglich, und dann fortfahren einzeln durch die Anweisungen des Blocks.

In diesem implizite Sortierung ist das Konzept der ein *, Utility control Point*, dies ist der nächste Vorgang ausgeführt werden. Wenn eine Methode aufgerufen (oder "namens" ist), wir sagen erstellt eine *Instanz* der Methode. Methodeninstanz besteht eine eigene Kopie der Parameter der Methode und lokale Variablen und eine eigene Kontrollpunkt aus.

### <a name="regular-methods"></a>Reguläre Methoden

Hier ist ein Beispiel für eine normale Methode

```vb
Function Test() As Integer
    Console.WriteLine("hello")
    Return 1
End Sub

Dim x = Test()    ' invokes the function, prints "hello", assigns 1 to x
```

Wenn eine normale Methode aufgerufen wird,

1. Zuerst wird eine Instanz der Methode für diesen Aufruf spezifischen erstellt. Diese Instanz enthält eine Kopie aller Parameter und lokalen Variablen der Methode.
2. Klicken Sie dann werden der Parameter für die angegebenen Werte und alle zugehörigen lokalen Variablen auf die Standardwerte der Typen initialisiert.
3. Im Fall von einer `Function`, eine implizite lokale Variable ist initialisiert wird aufgerufen, die *Funktion Rückgabevariablen* , deren Name den Namen der Funktion ist, dessen Typ ist der Rückgabetyp der Funktion und, dessen erste ist der Standardwert dieses Typs.
4. Kontrollpunkt für die Methodeninstanz wird bei der ersten Anweisung des Methodentexts festgelegt, und der Methodentext beginnt sofort von dort aus ausgeführt (Abschnitt [Blöcke und Bezeichnungen](statements.md#blocks-and-labels)).

Wenn ablaufsteuerung beendet wird den Methodentext normalerweise - durch erreichen die `End Function` oder `End Sub` , die Ende, markiert oder durch eine explizite `Return` oder `Exit` Statement - ablaufsteuerung zurückgibt, an den Aufrufer der Methodeninstanz. Bei eine Funktion Rückgabevariablen wird ist das Ergebnis des Aufrufs der endgültige Wert dieser Variablen an.

Wenn die ablaufsteuerung den Methodentext mithilfe einer nicht behandelten Ausnahme beendet wird, wird diese Ausnahme an den Aufrufer weitergegeben.

Nachdem der ablaufsteuerung beendet wurde, sind nicht mehr alle aktiven Verweise auf die Instanz. Wenn die Instanz nur Verweise auf eine Kopie der lokalen Variablen oder Parameter gehalten wird, können sie Garbage Collection bereinigt werden.

### <a name="iterator-methods"></a>Iterator-Methoden

Iterator-Methoden werden verwendet, um eine Sequenz, einem generieren, die von genutzt werden können auf einfache Weise die `For Each` Anweisung. Iterator-Methoden verwenden die `Yield` Anweisung (Abschnitt [Yield-Anweisung](statements.md#yield-statement)), geben Sie die Elemente der Sequenz. (Eine Iteratormethode ohne `Yield` Anweisungen erzeugt eine leere Sequenz). Hier ist ein Beispiel einer Iteratormethode:

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

Wenn eine Iteratormethode aufgerufen wird, deren Rückgabetyp `IEnumerator(Of T)`,

1. Zuerst wird eine Instanz der Iteratormethode für diesen Aufruf spezifischen erstellt. Diese Instanz enthält eine Kopie aller Parameter und lokalen Variablen der Methode.
2. Klicken Sie dann werden der Parameter für die angegebenen Werte und alle zugehörigen lokalen Variablen auf die Standardwerte der Typen initialisiert.
3. Eine implizite lokale Variable ist initialisiert wird aufgerufen, die *aktuelle Iteratorvariable*, dessen Typ `T` und, dessen erste ist der Standardwert dieses Typs.
4. Kontrollpunkt für die Methodeninstanz wird bei der ersten Anweisung des Methodentexts festgelegt.
5. Ein *Iterator-Objekt* wird dann erstellt, mit dieser Instanz verknüpft ist. Das Iterator-Objekt implementiert den deklarierte Rückgabetyp und verhält sich wie unten beschrieben.
6. Ablaufsteuerung wird dann fortgesetzt *sofort* im Aufrufer und das Ergebnis des Aufrufs ist das Iterator-Objekt. Beachten Sie, dass diese Übertragung realisiert wird, ohne eine Instanz der Iterator beendet, und führt nicht dazu, dass finally-Handler ausgeführt. Die Methodeninstanz wird weiterhin von dem Iterator-Objekt verwiesen, und wird keine Garbage Collection verschoben werden, sofern es vorhanden einen aktiven Verweis auf das Iterator-Objekt ist.

Wenn des Iterator-Objekts `Current` -Eigenschaft zugegriffen wird, die *aktuelle Variable* des Aufrufs wird zurückgegeben.

Wenn die Iterator-Objekt `MoveNext` -Methode wird aufgerufen, die der Aufruf erstellt eine neue Methodeninstanz nicht. Stattdessen wird die vorhandene Methodeninstanz verwendet (und der Kontrollpunkt und lokale Variablen und Parameter) – die Instanz, die erstellt wurde, wenn die Iteratormethode zuerst aufgerufen wurde. Ablaufsteuerung, wird die Ausführung an den Kontrollpunkt der dieser Instanz fortgesetzt, und in den Text der Iteratormethode wie gewohnt fortgesetzt.

Wenn die Iterator-Objekt `Dispose` -Methode wird aufgerufen, wird erneut die vorhandene Methodeninstanz verwendet. Steuern Sie Flow wird fortgesetzt, an der Kontrollpunkt dieses Methodeninstanz, aber dann sofort verhält sich wie ein `Exit Function` Anweisung wurden den nächsten Vorgang.

Der obigen Beschreibung des Verhaltens für das Aufrufen von `MoveNext` oder `Dispose` auf nur ein Iterator-Objekt angewendet werden soll, wenn alle vorherigen Aufrufe von `MoveNext` oder `Dispose` für das iteratorobjekt haben bereits an den Aufrufer zurückgegeben. Wenn sie dies nicht getan haben, ist das Verhalten nicht definiert.

Wenn ablaufsteuerung beendet wird den Text der Iterator-Methode normal über erreichen die `End Function` , die Ende, markiert oder durch eine explizite `Return` oder `Exit` Anweisung: sie müssen diesen Schritt ausgeführt haben im Kontext von einem Aufruf der `MoveNext` oder `Dispose` Funktion von einem iteratorobjekt zum Fortsetzen der Iteratorinstanz-Methode, und es wird bereits verwenden die Methodeninstanz, die erstellt wurde, wenn die Iteratormethode zuerst aufgerufen wurde. Der Kontrollpunkt der Instanz beibehalten. bei der `End Function` -Anweisung und der Flow wieder im Aufrufer; und wenn er durch einen Aufruf von fortgesetzt wurde, hatte `MoveNext` klicken Sie dann den Wert `False` an den Aufrufer zurückgegeben wird.

Wenn die ablaufsteuerung beendet der Iterator Methodentext über eine nicht behandelte Ausnahme ist, wird die Ausnahme weitergegeben wird, an den Aufrufer, die es entweder eines Aufrufs der `MoveNext` oder `Dispose`.

Wie für die anderen möglichen Rückgabetypen einer Iterator-Funktion,

* Wenn eine Iteratormethode aufgerufen wird, deren Rückgabetyp `IEnumerable(Of T)` für einige `T`, eine Instanz zuerst erstellt wird – speziell für diesen Aufruf der Iteratormethode--aller Parameter in der Methode, und sie werden mit den angegebenen Werten initialisiert. Das Ergebnis des Aufrufs ist ein ein Objekt, das den Rückgabetyp implementiert. Sollte dieses Objekts `GetEnumerator` Methode aufgerufen werden, erstellt er eine Instanz – für diesen Aufruf von spezifischen `GetEnumerator` --aller Parameter und lokalen Variablen in der Methode. Die Parameter für Werte, die bereits initialisiert, und fährt fort, wie der oben angegebenen Iteratormethode.
* Wenn eine Iteratormethode aufgerufen wird, deren Rückgabetyp, die nicht generische Schnittstelle ist `IEnumerator`, das Verhalten ist, genau wie bei `IEnumerator(Of Object)`.
* Wenn eine Iteratormethode aufgerufen wird, deren Rückgabetyp, die nicht generische Schnittstelle ist `IEnumerable`, das Verhalten ist, genau wie bei `IEnumerable(Of Object)`.

### <a name="async-methods"></a>Asynchrone Methoden

Async-Methoden sind eine einfache Möglichkeit, eine langfristig ausgeführte Arbeit zu tun, ohne zu blockieren, z. B. die Benutzeroberfläche einer Anwendung. Async ist die Kurzform für *asynchrone* – Dies bedeutet, dass der Aufrufer der Async-Methode wird sofort die Ausführung fortgesetzt, aber die letztendliche Fertigstellung der Async-Methode-Instanz möglicherweise zu einem späteren Zeitpunkt in der Zukunft. Gemäß der Konvention werden asynchrone Methoden mit dem Suffix "Async" benannt.

```vb
Async Function TestAsync() As Task(Of String)
    Console.WriteLine("hello")
    Await Task.Delay(100)
    Return "world"
End Function

Dim t = TestAsync()         ' prints "hello"
Console.WriteLine(Await t)  ' prints "world"
```

__Beachten Sie.__ Async-Methoden sind *nicht* in einem Hintergrundthread ausführen. Stattdessen können sie eine Methode, um durch Anhalten der `Await` Operator und Zeitplan selbst als Reaktion auf ein Ereignis fortgesetzt werden soll.

Wenn eine asynchrone Methode aufgerufen wird

1. Zuerst wird eine Instanz der Async-Methode für diesen Aufruf spezifischen erstellt. Diese Instanz enthält eine Kopie aller Parameter und lokalen Variablen der Methode.
2. Klicken Sie dann werden der Parameter für die angegebenen Werte und alle zugehörigen lokalen Variablen auf die Standardwerte der Typen initialisiert.
3. Im Fall von einer Async-Methode, mit dem Rückgabetyp `Task(Of T)` für einige `T`, eine implizite lokale Variable ist initialisiert wird aufgerufen, die *aufgabenrückgabevariable*, dessen Typ `T` und dessen erste Wert ist der Standardwert `T`.
4. Wenn die Async-Methode ist eine `Function` mit Rückgabetyp `Task` oder `Task(Of T)` für einige `T`, klicken Sie dann ein Objekt dieses Typs implizit erstellt, den aktuellen Aufruf zugeordnet. Dies wird als bezeichnet ein *Async-Objekt* und stellt dar, die zukünftige Arbeit, die durch Ausführen der Instanz der Async-Methode durchgeführt wird. Wenn Steuerelement im Aufrufer dieser Instanz der Async-Methode fortgesetzt wird, erhalten der Aufrufer dieser Async-Objekt als Ergebnis aufgerufen.
5. Steuerungspunkts für das der Instanz wird bei der ersten Anweisung des Hauptteils des Async-Methode festgelegt, und startet umgehend den Methodentext dort ausgeführt (Abschnitt [Blöcke und Bezeichnungen](statements.md#blocks-and-labels)).

__Wiederaufnahmedelegat und aktuellen Aufrufer__

Finden Sie im Abschnitt [Await-Operator](expressions.md#await-operator), Ausführung einer `Await` Ausdruck hat die Möglichkeit, unterbrechen die Methodeninstanz Steuerungspunkts für das Verlassen der ablaufsteuerung, um an anderer Stelle zu wechseln. Ablaufsteuerung kann später fortsetzen, an der gleichen Instanz des Steuerungspunkts für das durch Aufruf einer *Wiederaufnahmedelegat*. Beachten Sie, dass dieses Aussetzens durchgeführt werden, ohne die Async-Methode wird beendet, und führt nicht dazu, dass finally-Handler ausgeführt. Beide Wiederaufnahmedelegat wird noch die Methodeninstanz verweisen und die `Task` oder `Task(Of T)` führen (sofern vorhanden), und nicht Garbage gesammelt, sofern live Referenz vorhanden zu delegieren, oder führen.

Es ist hilfreich, stellen Sie sich vor der Anweisung `Dim x = Await WorkAsync()` ungefähr als syntaktische Kurzform für die folgenden:

```vb
Dim temp = WorkAsync().GetAwaiter()
If Not temp.IsCompleted Then
       temp.OnCompleted(resumptionDelegate)
       Return [task]
       CONT:   ' invocation of 'resumptionDelegate' will resume here
End If
Dim x = temp.GetResult()
```

Im folgenden wird die *aktuellen Aufrufer* der Methode Instanz definiert ist, als der ursprüngliche Aufrufer oder der Aufrufer den Wiederaufnahmedelegat je aktueller ist.

Wenn ablaufsteuerung beendet den Text für das Async-Methode – über erreicht wird die `End Sub` oder `End Function` , die Ende, markiert oder durch eine explizite `Return` oder `Exit` -Anweisung oder über eine nicht behandelte Ausnahme--Steuerungspunkts für das der Instanz auf festgelegt ist das Ende der Methode. Verhalten, hängt vom Rückgabetyp der asynchronen Methode.

* Im Fall von einem `Async Function` mit Rückgabetyp `Task`:
  1. Wenn beendet wird, über eine nicht behandelte Ausnahme, klicken Sie dann die asynchronen den Status des Objekts ablaufsteuerung nastaven NA hodnotu `TaskStatus.Faulted` und die zugehörige `Exception.InnerException` -Eigenschaftensatz auf die Ausnahme (mit Ausnahme von: bestimmte Ausnahmen Implementierung definiert, wie z. B. `OperationCanceledException` ändern Sie ihn in `TaskStatus.Canceled`). Flow wieder in den aktuellen Aufrufer.
  2. Andernfalls wird der asynchronen den Status des Objekts auf festgelegt `TaskStatus.Completed`. Flow wieder in den aktuellen Aufrufer.

     (__Beachten.__ Die gesamte Punkt der Aufgabe und Async-Methoden interessant, woraus ist, wird eine Aufgabe abgeschlossen und alle Methoden, die auf ihn warten wurden haben, werden derzeit Wiederaufnahme Delegate ausgeführt, d. h. werden sie entsperrt werden.)

* Im Fall von einer `Async Function` mit Rückgabetyp `Task(Of T)` für einige `T`: das Verhalten ist als oben, außer, dass in nicht-Ausnahme des asynchronen Objekts Fällen `Result` -Eigenschaft auch auf den endgültigen Wert, der die aufgabenrückgabevariable festgelegt ist.

* Im Fall von einem `Async Sub`:
  1. Wenn beendet wird, über eine nicht behandelte Ausnahme, klicken Sie dann die Ausnahme ablaufsteuerung werden in der Umgebung auf implementierungsspezifische weitergegeben. Flow wieder in den aktuellen Aufrufer.
  2. Andernfalls wird ablaufsteuerung einfach in den aktuellen Aufrufer fortgesetzt werden.

#### <a name="async-sub"></a>Async-Unterfunktion.

Es gibt einige Microsoft-spezifische Verhalten einer `Async Sub`.

Wenn `SynchronizationContext.Current` ist `Nothing` zu Beginn des Aufrufs, klicken Sie dann jede nicht behandelten Ausnahmen einer Async-Unterfunktion werden im Verteilungsverlauf Threadpool der Warteschleife hinzu.

Wenn `SynchronizationContext.Current` nicht `Nothing` am Anfang des Aufrufs, klicken Sie dann `OperationStarted()` für diesen Kontext vor dem Start der Methode aufgerufen wird und `OperationCompleted()` nach dem Ende. Darüber hinaus werden alle Ausnahmefehler bereitgestellt werden, auf den Synchronisierungskontext erneut ausgelöst wird.

Dies bedeutet, dass in UI-Anwendungen, für eine `Async Sub` , die im UI-Thread aufgerufen wird, ein Fehler auftritt, behandeln, Ausnahmen werden erneut den UI-Thread abgelegt werden.

#### <a name="mutable-structures-in-async-and-iterator-methods"></a>Änderbare Strukturen in Async-und iteratormethoden

Änderbare Strukturen im Allgemeinen werden als ungültiges Verfahren angesehen, und sie werden von Async- oder Iterator-Methoden nicht unterstützt. Jeder Aufruf einer Async- oder Iterator-Methode in einer Struktur werden insbesondere implizit ausgeführt, auf eine *Kopie* dieser Struktur, die zu der Zeitpunkt des Aufrufs kopiert werden. Daher ist es beispielsweise bei

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

Eine Gruppe von ausführbaren Anweisungen wird einen Anweisungsblock aufgerufen. Die Ausführung der Anweisungsblock beginnt mit der ersten Anweisung im-Block. Nachdem eine Anweisung ausgeführt wurde, wird die nächste Anweisung im lexikalischen Reihenfolge ausgeführt, es sei denn, eine Anweisung überträgt die Ausführung an einem anderen Ort oder eine Ausnahme auftritt.

Innerhalb eines Blocks Anweisung ist nicht die Division von Anweisungen für logische Zeilen mit Ausnahme von Bezeichnung deklarationsanweisungen erhebliche. Eine Bezeichnung ist ein Bezeichner, der eine bestimmte Position innerhalb der Anweisungsblock identifiziert, die als Ziel einer Verzweigung-Anweisung wie z. B. verwendet werden können `GoTo`.

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


Deklarationsanweisungen Bezeichnung müssen am Anfang einer logischen Zeile angezeigt werden und Bezeichnungen können entweder ein Bezeichner oder ein Integer-literal. Da sowohl die Bezeichnung deklarationsanweisungen als auch die Aufruf-Anweisungen einen einzelnen Bezeichner enthalten können, wird am Anfang eine lokale Zeile ein einzelnen Bezeichner immer eine deklarationsanweisung Bezeichnung angesehen. Deklarationsanweisungen Bezeichnung müssen immer von einem Doppelpunkt folgen, selbst wenn keine Anweisungen in derselben logischen Zeile folgen.

Bezeichnungen müssen ihre eigenen Deklarationsabschnitt und verursachen keine Konflikte mit anderen Bezeichnern. Im folgenden Beispiel ist gültig und wird verwendet, die Name-Variable `x` als Parameter sowohl als Bezeichnung.

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

Der Umfang einer Bezeichnung wird der Text der Methode, die sie enthält.

Aus Gründen der besseren Lesbarkeit werden Anweisung Produktionen, bei denen mehrere Substatements als einer einzigen Produktion in dieser Spezifikation behandelt, obwohl die Substatements jedes selbst in einer Zeile mit Bezeichnung sein können.


### <a name="local-variables-and-parameters"></a>Lokale Variablen und Parameter

Details wie der vorherigen Abschnitte. und wenn Methodeninstanzen erstellt werden und mit ihnen die Kopien der lokalen Variablen und Parameter einer Methode. Darüber hinaus jedes Mal, die der Text einer Schleife erreicht wird, eine neue Kopie wird erstellt für jede lokale Variable, die innerhalb dieser Schleife deklariert werden, wie im Abschnitt beschrieben [Schleifenanweisungen](statements.md#loop-statements), und die Methodeninstanz enthält nun diese Kopie der lokalen Variablen anstatt als die vorherigen Kopie.

Alle lokalen Variablen werden mit Standardwert des Typs initialisiert. Lokale Variablen und Parameter sind immer öffentlich zugegriffen werden kann. Es ist ein Fehler in der Lage, Text, der vor der Deklaration einer lokalen Variablen verweisen, wie im folgende Beispiel veranschaulicht:

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

In der `F` oben genannte Methode, die erste Zuweisung zu `i` insbesondere verweist nicht auf das Feld im äußeren Bereich deklariert. Stattdessen verweist er auf die lokale Variable, und Fehler ist, da die Deklaration der Variablen textlich vorausgehenden. In der `G` -Methode, eine nachfolgenden Variablendeklaration bezieht sich auf eine lokale Variable, die in der eine früheren Deklaration in der gleichen Deklaration lokaler Variablen deklariert.

Jeder Block in einer Methode erstellt eine Deklaration Speicherplatz für lokale Variablen. Namen werden in diesen Deklarationsabschnitt über Deklarationen von lokalen Variablen im Methodentext und der Parameterliste der Methode, die Namen in der äußersten Blocks Deklarationsabschnitt führt eingeführt. Blöcke nicht shadowing von Namen durch die Schachtelung zulassen: Nachdem Sie ein Namen in einem Block deklariert wurde, der Name kann nicht erneut deklariert werden in einem geschachtelten Block.

Im folgenden Beispiel die `F` und `G` Methoden sind Fehler, da der Name `i` im äußeren-Block deklariert wird und nicht im inneren Block erneut deklariert werden. Allerdings die `H` und `I` Methoden sind gültig. da die beiden `i`des werden in separaten nicht geschachtelten Blöcken deklariert.

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

Wenn die Methode eine Funktion ist, wird eine spezielle lokale Variable implizit in den Methodentext Deklarationsabschnitt mit dem gleichen Namen wie die Methode, die den Rückgabewert der Funktion darstellt deklariert. Die lokale Variable ist sondername Auflösung Semantik, wenn in Ausdrücken verwendet. Wenn die lokale Variable in einem Kontext verwendet wird, die einen Ausdruck, der als eine Methodengruppe, z. B. ein Aufrufausdruck klassifiziert erwartet löst den Namen der Funktion und nicht auf die lokale Variable. Zum Beispiel:

```vb
Function F(i As Integer) As Integer
    If i = 0 Then
        F = 1        ' Sets the return value.
    Else
        F = F(i - 1) ' Recursive call.
    End If
End Function
```

Die Verwendung von Klammern kann dazu führen, dass mehrdeutige Situationen (z. B. `F(1)`, wobei `F` ist eine Funktion, deren Rückgabetyp ein eindimensionales Array ist); in allen Situationen nicht eindeutig, der Name aufgelöst wird, um die Funktion anstatt der lokalen Variablen. Zum Beispiel:

```vb
Function F(i As Integer) As Integer()
    If i = 0 Then
        F = new Integer(2) { 1, 2, 3 }
    Else
        F = F(i - 1) ' Recursive call, not an index.
    End If
End Function
```

Bei der ablaufsteuerung des Methodentexts verlässt, wird der Wert der lokalen Variablen zurück, der Aufrufausdruck übergeben. Wenn die Methode eine Subroutine ist, gibt es keine solche implizite lokale Variable ist und Steuerelement auf der Aufrufausdruck gibt einfach auftragsantwortnachrichten zurück.

## <a name="local-declaration-statements"></a>Lokale Deklarationsanweisungen

Eine lokalen deklarationsanweisung deklariert eine neue lokale Variable, eine lokale Konstante oder eine statische Variable. *Lokale Variablen* und *lokale Konstanten* sind gleichwertig mit der von Instanzvariablen und Konstanten, die an die Methode beschränkt, und auf die gleiche Weise deklariert werden. *Statische Variablen* ähneln `Shared` mithilfe von Variablen und sind deklariert die `Static` Modifizierer.

```antlr
LocalDeclarationStatement
    : LocalModifier VariableDeclarators StatementTerminator
    ;

LocalModifier
    : 'Static' | 'Dim' | 'Const'
    ;
```

Statische Variablen handelt es sich um lokale Variablen, die über Aufrufe der Methode ihren Wert beibehalten. Statische Variablen, die innerhalb von nicht freigegebenen Methoden deklariert werden, pro Instanz: jede Instanz des Typs, der die Methode enthält, hat eine eigene Kopie der statischen Variable. Statische Variablen deklariert in `Shared` Methoden sind pro Typ; es ist nur eine Kopie der statischen Variablen für alle Instanzen. Während der lokale Variablen des Typs der Standardwert bei jeder Eintrag in die Methode initialisiert werden, sind statische Variablen auf den Standardwert des Typs nur initialisiert, wenn der Typ oder die Instanz initialisiert wird. Statische Variablen können nicht in Strukturen oder generischen Methoden deklariert werden.

Lokale Variablen, lokale Konstanten und statische Variablen immer öffentliche zugreifbarkeit besitzen, und können keinen Zugriffsmodifizierer angeben. Wenn kein Typ in einer lokalen deklarationsanweisung angegeben wird, bestimmen die folgenden Schritte aus den Typ der lokalen Deklaration:

1. Wenn die Deklaration ein Typzeichen, ist der Typ des Zeichens Typ den Typ der lokalen Deklaration.

2. Wenn die lokale Deklaration eine lokale Konstante ist, oder wenn die lokale Deklaration ist eine lokale Variable mit einem Initialisierer und lokaler Variablen Typrückschluss verwendet wird, wird der Typ der lokalen Deklaration aus dem Typ des Initialisierers abgeleitet. Wenn der Initialisierer der lokalen Deklaration verweist, tritt auf, ein Fehler während der Kompilierung. (Lokale Konstanten müssen initialisiert werden.)

3. Wenn strikte Semantik nicht verwendet werden, ist implizit der Typ des von der lokalen deklarationsanweisung `Object`.

4. Andernfalls tritt ein Kompilierungsfehler auf.

Wenn kein Typ in einer lokalen deklarationsanweisung, die eine Arraygröße oder ein Array der Modifizierer verfügt angegeben ist, klicken Sie dann der Typ der lokalen Deklaration ist ein Array mit dem angegebenen Rang und die vorherigen Schritte werden verwendet, um den Elementtyp des Arrays zu bestimmen. Wenn lokale Variable Typrückschluss verwendet wird, muss der Typ des Initialisierers einen Arraytyp mit der gleichen Array-Form (d. h. Arraymodifizierer Typ) wie die Deklaration von lokalen-Anweisung sein. Beachten Sie, dass es möglich, dass der Typ des abgeleiteten Elements noch ein Arraytyp sein kann. Zum Beispiel:

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

Wenn kein Typ in einer lokalen deklarationsanweisung angegeben wird, besitzt den Typmodifizierer für einen NULL-Werte zulässt, und klicken Sie dann der Typ der lokalen Deklaration ist der auf NULL festlegbare Version des abgeleiteten Typs oder der abgeleitete Typ selbst aus, wenn sie bereits ein NULL-Wert eingeben.  Wenn der abgeleitete Typ kein Werttyp, der auf NULL festgelegt werden kann ist, tritt ein Fehler während der Kompilierung. Wenn sowohl den Typmodifizierer für einen NULL-Werte zulässt und ein Array-Größe oder Array Typmodifizierer auf einer lokalen deklarationsanweisung ohne Typ gespeichert werden, klicken Sie dann der Modifizierer nullable-Typ gilt, die den Elementtyp des Arrays angewendet, und die vorherigen Schritte werden verwendet, um zu bestimmen, die eleme NT-Typ.

Variableninitialisierern auf lokale deklarationsanweisungen sind gleichwertig mit zuweisungsanweisungen, die am Text Speicherort der Deklaration platziert werden. Wenn die Ausführung über die lokale deklarationsanweisung verzweigt werden soll, wird die Variableninitialisierer daher nicht ausgeführt. Die Variableninitialisierer wird ausgeführt, wenn die Deklaration von lokalen-Anweisung mehr als einmal ausgeführt wird, eine gleiche Anzahl an, wie oft. Statische Variablen führen ihre Initialisierer nur beim ersten. Wenn eine Ausnahme tritt auf, bei der Initialisierung einer statischen Variablen, gilt die statische Variable mit dem Standardwert des Typs für die statische Variable initialisiert.

Das folgende Beispiel zeigt die Verwendung der Initialisierer:

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

Dieses Programm druckt:

```
Static variable x = 5
Static variable x = 6
Static variable x = 7
Local variable y = 8
Local variable y = 8
Local variable y = 8
```

Initialisierer für statische lokale Variablen sind threadsicher und vor Ausnahmen geschützt, während der Initialisierung. Wenn während einer statischen lokalen Initialisierung eine Ausnahme auftritt, wird der statische lokalen hat den Standardwert zurück, und nicht initialisiert werden. Eine lokale statische Initialisierung

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

Lokale Variablen, lokale Konstanten und statische Variablen beziehen sich auf die Anweisung blockiert wird, in dem sie deklariert werden. Statische Variablen sind spezielle, da ihre Namen nur einmal in die gesamte Methode verwendet werden können. Beispielsweise ist es auf zwei Deklarationen mit statische Variable mit dem gleichen Namen zu geben, auch wenn sie in verschiedene Blöcke sind ungültig.


### <a name="implicit-local-declarations"></a>Implizite lokale Deklarationen

Zusätzlich zu Anweisungen, lokale Deklaration können lokale Variablen auch implizit durch die Verwendung deklariert werden. Ein einfacher Name-Ausdruck, der einen Namen verwendet, der nicht auf etwas anderes behebt deklariert eine lokale Variable mit diesem Namen. Zum Beispiel:

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

Implizite Deklaration von lokalen tritt nur in ausdruckskontexten, die einen Ausdruck, der als Variable klassifiziert akzeptieren kann. Die Ausnahme von dieser Regel ist, dass eine lokale Variable nicht implizit deklariert werden kann, wenn es das Ziel des Aufrufs Funktionsausdruck, Indizierung Ausdruck oder eines Memberzugriffsausdrucks ist.

Implizite "lokal" werden behandelt, als ob sie am Anfang der enthaltenden Methode deklariert werden. Daher sind sie immer auf den gesamten Methodentext, beschränkt, auch wenn innerhalb eines Lambdaausdrucks deklariert. Beispielsweise folgender Code:

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

Gibt den Wert `10`. Implizite "lokal" werden als typisierte `Object` Wenn keine Typzeichen wurde angefügt, um den Namen der Variablen ist; andernfalls ist des Typs der Variablen den Typ des Typzeichen. Lokale Variablen Typrückschluss wird nicht für implizite "lokal" verwendet.

Wenn explizite Deklaration von lokalen, durch die kompilierungsumgebung oder durch angegeben wird `Option Explicit`, alle lokale Variablen explizit deklariert werden müssen, und implizite Deklaration von Variablen ist nicht zulässig.

## <a name="with-statement"></a>Mit-Anweisung

Ein `With` -Anweisung können mehrere Verweise auf die Member eines Ausdrucks ohne Angabe des Ausdrucks mehrmals.

```antlr
WithStatement
    : 'With' Expression StatementTerminator
      Block?
      'End' 'With' StatementTerminator
    ;
```

Der Ausdruck als Wert klassifiziert werden muss und beim Eintritt in den Block einmal ausgewertet. Innerhalb der `With` Anweisungsblock, ein Memberzugriffsausdruck oder den Ausdruck für den Wörterbuch ab, die mit einem Punkt oder einem Ausrufezeichen wird ausgewertet, als ob die `With` Ausdruck vorangestellt es. Zum Beispiel:

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

Es ist unzulässig, eine Verzweigung in einem `With` Anweisungsblock von außerhalb des Blocks.


## <a name="synclock-statement"></a>SyncLock-Anweisung

Ein `SyncLock` -Anweisung können die Anweisungen auf einem Ausdruck, synchronisiert werden, der sicherstellt, dass mehrere Ausführungsthreads gleichzeitig nicht die gleichen Anweisungen ausführen.

```antlr
SyncLockStatement
    : 'SyncLock' Expression StatementTerminator
      Block?
      'End' 'SyncLock' StatementTerminator
    ;
```

Der Ausdruck als Wert klassifiziert werden muss und beim Einstieg in den Block einmal ausgewertet. Bei der Eingabe der `SyncLock` Block, der `Shared` Methode `System.Threading.Monitor.Enter` wird aufgerufen, in dem angegebenen Ausdruck, die blockiert, bis der Thread der Ausführung eine exklusive Sperre für das Objekt, das vom Ausdruck zurückgegeben wurde. Der Typ des Ausdrucks in einer `SyncLock` -Anweisung muss ein Verweistyp sein. Zum Beispiel:

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

Das obige Beispiel synchronisiert wird, auf die jeweilige Instanz der Klasse `Test` um sicherzustellen, dass nicht mehr als einen Thread der Ausführung kann hinzufügen oder davon subtrahiert der Variablen "Count" zu einem Zeitpunkt für eine bestimmte Instanz.

Die `SyncLock` Block implizit enthalten ist ein `Try` Anweisung, deren `Finally` Aufrufe der `Shared` Methode `System.Threading.Monitor.Exit` für den Ausdruck. Dadurch wird sichergestellt, dass die Sperre aufgehoben wird, selbst wenn eine Ausnahme ausgelöst wird. Daher ist ungültig in branch ein `SyncLock` Blockieren von außerhalb des Blocks auf, und ein `SyncLock` Block wird als einzelne Anweisung behandelt, im Rahmen `Resume` und `Resume Next`. Im obige Beispiel entspricht dem folgenden Code:

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


## <a name="event-statements"></a>Event-Anweisungen

Die `RaiseEvent`, `AddHandler`, und `RemoveHandler` Anweisungen Ereignisse auslösen und Behandeln von Ereignissen dynamisch.

```antlr
EventStatement
    : RaiseEventStatement
    | AddHandlerStatement
    | RemoveHandlerStatement
    ;
```

### <a name="raiseevent-statement"></a>RaiseEvent-Anweisung

Ein `RaiseEvent` Anweisung benachrichtigt, Ereignishandler, die ein bestimmtes Ereignis aufgetreten ist.

```antlr
RaiseEventStatement
    : 'RaiseEvent' IdentifierOrKeyword
      ( OpenParenthesis ArgumentList? CloseParenthesis )? StatementTerminator
    ;
```

Der einfache Namensausdruck in einer `RaiseEvent` Anweisung wird als eine Suche nach Membern auf interpretiert `Me`. Daher `RaiseEvent x` wird interpretiert, als wäre er `RaiseEvent Me.x`. Das Ergebnis des Ausdrucks muss als Zugriffs-Ereignis für ein Ereignis in der Klasse selbst definierten klassifiziert werden; Ereignisse für Basistypen definierten können nicht verwendet werden, einem `RaiseEvent` Anweisung.

Die `RaiseEvent` -Anweisung verarbeitet wird, als Aufruf an die `Invoke` Methode des Ereignisses-Delegaten, mit der bereitgestellten Parameter, sofern vorhanden. Falls der Wert des Delegaten `Nothing`, wird keine Ausnahme ausgelöst. Wenn keine Argumente vorhanden sind, können die Klammern weggelassen werden. Zum Beispiel:

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

Die Klasse `Raiser` höher ist äquivalent zu:

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


### <a name="addhandler-and-removehandler-statements"></a>AddHandler und RemoveHandler-Anweisungen

Obwohl die meisten Ereignishandler über automatisch eingebunden werden `WithEvents` Variablen, es kann erforderlich sein, dynamisch hinzufügen und Entfernen von Ereignishandlern zur Laufzeit. `AddHandler` und `RemoveHandler` Anweisungen dazu.

```antlr
AddHandlerStatement
    : 'AddHandler' Expression Comma Expression StatementTerminator
    ;

RemoveHandlerStatement
    : 'RemoveHandler' Expression Comma Expression StatementTerminator
    ;
```

Jede Anweisung akzeptiert zwei Argumente: das erste Argument muss ein Ausdruck, der als ein Ereigniszugriff eingestuft wird und das zweite Argument muss ein Ausdruck, der als Wert klassifiziert wird. Das zweite Argument Typ muss es sich um den Typ des Delegaten, mit der Ereignis verknüpft sein. Zum Beispiel:

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

Ein Ereignis `E,` die Anweisung ruft die relevante `add_E` oder `remove_E` Methode für die Instanz zum Hinzufügen oder entfernen den Delegaten als Handler für das Ereignis. Daher entspricht der obige Code:

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


## <a name="assignment-statements"></a>Zuweisungsanweisungen

Eine zuweisungsanweisung weist den Wert eines Ausdrucks zu einer Variablen an. Es gibt mehrere Typen von Zuweisung.

```antlr
AssignmentStatement
    : RegularAssignmentStatement
    | CompoundAssignmentStatement
    | MidAssignmentStatement
    ;
```

### <a name="regular-assignment-statements"></a>Reguläre Zuweisungsanweisungen

Eine einfache zuweisungsanweisung speichert das Ergebnis eines Ausdrucks in einer Variablen an.

```antlr
RegularAssignmentStatement
    : Expression Equals Expression StatementTerminator
    ;
```

Während der Ausdruck auf der rechten Seite des Zuweisungsoperators als Wert klassifiziert werden muss, muss der Ausdruck auf der linken Seite des Zuweisungsoperators als Variable oder ein Eigenschaftenzugriff klassifiziert werden. Der Typ des Ausdrucks muss implizit in den Typ des Zugriffs Variable oder eine Eigenschaft sein.

Wenn die Variable zugewiesen wird, in ein Arrayelement einen Verweistyp handelt, wird eine laufzeitüberprüfung ausgeführt werden, um sicherzustellen, dass der Ausdruck mit dem Arrayelement Typ kompatibel ist. Im folgenden Beispiel wird die letzte Zuweisung einer `System.ArrayTypeMismatchException` da ausgelöst wird eine Instanz von `ArrayList` kann nicht gespeichert werden, in einem Element des eine `String` Array.

```vb
Dim sa(10) As String
Dim oa As Object() = sa
oa(0) = Nothing         ' This is allowed.
oa(1) = "Hello"         ' This is allowed.
oa(2) = New ArrayList() ' System.ArrayTypeMismatchException is thrown.
```

Wenn der Ausdruck auf der linken Seite des Zuweisungsoperators als Variable klassifiziert ist, klicken Sie dann die zuweisungsanweisung den Wert in der Variablen gespeichert. Wenn der Ausdruck wird als Eigenschaftszugriff klassifiziert, und klicken Sie dann die zuweisungsanweisung der Eigenschaftenzugriff in einen Aufruf wandelt der `Set` -Accessor der Eigenschaft mit dem Wert für den Value-Parameter ersetzt. Zum Beispiel:

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

Wenn das Ziel des Zugriffs Variable oder eine Eigenschaft ist als ein Wert eingegeben, aber nicht als Variable klassifiziert, tritt ein Fehler während der Kompilierung. Zum Beispiel:

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

Beachten Sie, dass die Semantik der Zuweisung, die für den Typ der Variablen oder Eigenschaft, die sie zugewiesen wird, ist, abhängig sind. Wenn die Variable, die sie zugewiesen wird, ist, ein Werttyp ist, kopiert die Zuweisung den Wert des Ausdrucks in die Variable an. Wenn die Variable, die sie zugewiesen wird, ist, ein Verweistyp ist, kopiert die Zuweisung den Verweis, nicht den Wert selbst, in die Variable an. Wenn der Typ der Variablen ist `Object`, die Zuweisungssemantik werden bestimmt, indem Sie, ob der Typ des Werts zur Laufzeit ein Werttyp oder ein Verweistyp ist.


__Beachten Sie.__ Für systeminterne wie Typen `Integer` und `Date`, Verweis- und Zuweisungssemantik sind identisch, da die Typen unveränderlich sind. Daher ist die Sprache Zuweisung eines Verweises auf die systeminternen Typen als Optimierung verwenden. Hinsichtlich der Wert ist das Ergebnis identisch.

Da das Gleichheitszeichen (`=`) wird verwendet, sowohl für Zuweisungen und Gleichheit, besteht eine Mehrdeutigkeit zwischen einer einfachen Zuweisung und einer aufrufanweisung in Situationen wie z. B. `x = y.ToString()`. In allen diesen Fällen hat die zuweisungsanweisung Vorrang vor den Equality-Operator. Dies bedeutet, dass der Beispielausdruck, als interpretiert wird `x = (y.ToString())` statt `(x = y).ToString()`.


### <a name="compound-assignment-statements"></a>Verbundanweisungen für Zuordnung

Ein *zusammengesetzte zuweisungsanweisung* hat das Format `V op= E` (, in denen `op` ist ein gültiger binärer Operator).

```antlr
CompoundAssignmentStatement
    : Expression CompoundBinaryOperator LineTerminator? Expression StatementTerminator
    ;

CompoundBinaryOperator
    : '^' '=' | '*' '=' | '/' '=' | '\\' '=' | '+' '=' | '-' '='
    | '&' '=' | '<' '<' '=' | '>' '>' '='
    ;
```

Der Ausdruck auf der linken Seite des Zuweisungsoperators muss als eine Variable oder Eigenschaftszugriff, klassifiziert werden, während der Ausdruck auf der rechten Seite des Zuweisungsoperators als Wert klassifiziert werden muss. Die verbundzuweisung-Anweisung entspricht der Anweisung `V = V op E` mit dem Unterschied, dass die Variable auf der linken Seite des Operators verbundzuweisung nur einmal ausgewertet. Das folgende Beispiel veranschaulicht diesen Unterschied:

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

Der Ausdruck `a(GetIndex())` wird zweimal für einfache Zuweisung jedoch nur ausgewertet, nachdem für die verbundzuweisung, also der Code ausgibt:

```
Simple assignment
Getting index
Getting index
Compound assignment
Getting index
```


### <a name="mid-assignment-statement"></a>Mid Zuweisungsanweisung

Ein `Mid` zuweisungsanweisung weist eine Zeichenfolge in eine andere Zeichenfolge. Die linke Seite der Zuweisung enthält die gleiche Syntax wie ein Aufruf an die Funktion `Microsoft.VisualBasic.Strings.Mid`.

```antlr
MidAssignmentStatement
    : 'Mid' '$'? OpenParenthesis Expression Comma Expression
      ( Comma Expression )? CloseParenthesis Equals Expression StatementTerminator
    ;
```

Das erste Argument ist das Ziel der Zuweisung und muss als eine Variable oder ein Eigenschaftenzugriff, dessen Typ ist implizit konvertierbar in und aus, klassifiziert `String`. Der zweite Parameter ist die 1-basierte Anfangsposition an, in dem die Zuweisung in der Zielzeichenfolge beginnen soll, und muss als Wert, dessen Typ implizit in muss, klassifiziert werden, das entspricht `Integer`. Dritte optionale Parameter ist die Anzahl von Zeichen aus der rechten Seite Wert, der in der Zielzeichenfolge zugewiesen und muss als ein Wert, dessen Typ implizit in klassifiziert `Integer`. Die rechte Seite ist die Quellzeichenfolge und muss als ein Wert, dessen Typ implizit in klassifiziert `String`. Rechts auf der Length-Parameter, abgeschnitten wird, wenn angegeben, und ersetzt die Zeichen in der linken Seite-Zeichenfolge, beginnend ab der Startposition. Wenn die rechte Seite Zeichenfolge weniger Zeichen als dritten Parameter enthalten, werden nur die Zeichen aus der Zeichenfolge rechts kopiert.

Das folgende Beispiel zeigt `ab123fg`:

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

__Beachten Sie.__ `Mid` ist ein reserviertes Wort.


## <a name="invocation-statements"></a>Aufruf-Anweisungen

Eine aufrufanweisung Ruft eine Methode, die das optionale Schlüsselwort vorangestellt `Call`. Der aufrufanweisung wird in die gleiche Weise wie der Aufrufausdruck Funktion, mit einigen Unterschieden, die unten aufgeführten verarbeitet. Der Aufrufausdruck muss als Wert oder "void" klassifiziert werden. Jeder Wert aus der Auswertung der Aufrufausdruck wird verworfen.

Wenn die `Call` -Schlüsselwort ausgelassen wird, und klicken Sie dann der Aufrufausdruck muss ein Bezeichner oder ein Schlüsselwort oder mit beginnen `.` innerhalb einer `With` Block. Daher, z. B. "`Call 1.ToString()`" eine gültige Anweisung jedoch "`1.ToString()`" nicht. (Beachten Sie, dass in einem Ausdruckskontext Aufrufausdrücke auch nicht mit einem Bezeichner beginnen müssen. Z. B. "`Dim x = 1.ToString()`" eine gültige Anweisung).

Es ist ein weiterer Unterschied zwischen dem Aufruf Anweisungen und das Aufrufen von Ausdrücken: Falls eine aufrufanweisung enthält eine Argumentliste, dies immer als der Argumentliste des Aufrufs verwendet. Das folgende Beispiel veranschaulicht den Unterschied:

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

## <a name="conditional-statements"></a>Bedingungsanweisungen

Bedingte Anweisungen ermöglichen das bedingte Ausführung von Anweisungen, die basierend auf Ausdrücken, die zur Laufzeit ausgewertet.

```antlr
ConditionalStatement
    : IfStatement
    | SelectStatement
    ;
```

### <a name="ifthenelse-statements"></a>If... Klicken Sie dann... Else-Anweisungen

Ein `If...Then...Else` ist die grundlegende bedingten Anweisung.

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

Jeder Ausdruck in einem `If...Then...Else` Anweisung muss einem booleschen Ausdruck gemäß Abschnitt [boolesche Ausdrücke](expressions.md#boolean-expressions). (Hinweis: Dies erfordert keine den Ausdruck, der Boolesch sein). Wenn der Ausdruck in der `If` -Anweisung ist "true", die Anweisungen, die von der `If` Block ausgeführt werden. Wenn der Ausdruck "false" ist jede der `ElseIf` Ausdrücke ausgewertet wird. Wenn eine von der `ElseIf` Ausdrücke, die auf "true" ausgewertet wird, der entsprechenden Block ausgeführt wird. Wenn kein Ausdruck wird zu True ausgewertet, und es gibt eine `Else` Block, der `Else` Block wird ausgeführt. Nach ein-Block die Ausführung abgeschlossen ist, übergibt die Ausführung bis zum Ende der `If...Then...Else` Anweisung.

Die Zeilenversion des der `If` Anweisung verfügt über einen Satz von Anweisungen ausgeführt werden, wenn die `If` Ausdruck ist `True` und einer optionalen Gruppe von Anweisungen, die ausgeführt werden, wenn der Ausdruck ist `False`. Zum Beispiel:

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

Die Zeilenversion zurück, wenn die Anweisung bindet weniger streng als ":", und die zugehörige `Else` bindet an die lexikalisch am nächsten vorangehenden `If` , durch die Syntax zulässig ist. Die folgenden beiden Versionen sind beispielsweise äquivalent:

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

Alle Anweisungen als Bezeichnung deklarationsanweisungen sind innerhalb einer Zeile zulässig `If` -Anweisung, einschließlich der Block von Anweisungen. Sie können jedoch nicht verwenden LineTerminators als StatementTerminators außer in mehrzeiligen Lambda-Ausdrücke. Zum Beispiel:

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

Ein `Select Case` -Anweisung führt Anweisungen basierend auf dem Wert eines Ausdrucks.

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

Der Ausdruck muss als Wert klassifiziert werden. Wenn eine `Select Case` -Anweisung wird ausgeführt, die `Select` Ausdrucks zuerst ausgewertet, und die `Case` Anweisungen werden in der Reihenfolge der Text der Deklaration ausgewertet. Die erste `Case` Anweisung, die überprüft `True` verfügt über einen Block ausgeführt. Wenn kein `Case` Anweisung ergibt `True` und es gibt eine `Case Else` -Anweisung, der Block ausgeführt wird. Nach der Ausführung ein Blocks beendet hat, übergibt die Ausführung bis zum Ende der `Select` Anweisung.

Ausführung einer `Case` Block ist auf "mit dem nächsten Switch-Abschnitt fortfahren" nicht zulässig. Dies verhindert, dass eine allgemeine Klasse von Fehlern, die auftreten, in anderen Sprachen bei der ein `Case` Anweisung beenden versehentlich ausgelassen wird. Dieses Verhalten wird im folgenden Beispiel veranschaulicht:

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

Vom Code ausgegeben:

```
x = 10
```

Obwohl `Case 10` und `Case 20 - 10` Option für den gleichen Wert `Case 10` wird ausgeführt, da ihm vorausgeht `Case 20 - 10` textlich. Wenn die nächste `Case` wird erreicht, die Ausführung wird fortgesetzt, nachdem die `Select` Anweisung.

Ein `Case` -Klausel zwei Formen annehmen kann. Ein Formular ist eine optionale `Is` Schlüsselwort, ein Vergleichsoperator und einen Ausdruck. Der Ausdruck wird in den Typ des konvertiert die `Select` Ausdruck; Wenn der Ausdruck nicht implizit in den Typ des der `Select` Ausdruck ein Fehler während der Kompilierung auftritt. Wenn die `Select` Ausdruck ist *E*, der Vergleichsoperator ist *Op*, und die `Case` Ausdruck *E1*, wird die Groß-/Kleinschreibung als bewertet *E OP E1*. Der Operator muss für die Datentypen der beiden Ausdrücke gültig sein; andernfalls tritt ein Kompilierungsfehler auf.

Die andere Form ist ein Ausdruck, optional gefolgt von dem Schlüsselwort `To` und einen zweiten Ausdruck. Beide Ausdrücke werden in den Typ des konvertiert die `Select` Ausdruck; ist einer der Ausdrücke nicht implizit in den Typ des der `Select` Ausdruck ein Fehler während der Kompilierung auftritt. Wenn die `Select` Ausdruck ist `E`, die erste `Case` Ausdruck ist `E1`, und das zweite `Case` Ausdruck ist `E2`, `Case` wird ausgewertet, entweder als `E = E1` (wenn keine `E2`angegeben ist) oder `(E >= E1) And (E <= E2)`. Die Operatoren müssen für die Datentypen der beiden Ausdrücke gültig sein. andernfalls tritt ein Kompilierungsfehler auf.


## <a name="loop-statements"></a>Schleifenanweisungen

Schleifenanweisungen ermöglichen die wiederholte Ausführung der Anweisungen in ihrem Textkörper.

```antlr
LoopStatement
    : WhileStatement
    | DoLoopStatement
    | ForStatement
    | ForEachStatement
    ;
```

Jedes Mal, wenn ein Schleifenkörper eingegeben wird, erfolgt eine neue Kopie aller lokalen Variablen, die in dieses Texts, auf die vorherigen Werte der Variablen initialisiert deklariert. Jeder Verweis auf eine Variable innerhalb der Schleife wird die am häufigsten vor kurzem wurden Kopie verwendet. Dieser Code zeigt ein Beispiel:

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

```
31    32    33
```

Wenn der Inhalt der Schleife ausgeführt wird, wird welcher Kopie der Variable aktuell ist. Z. B. die Anweisung `Dim y = x` bezieht sich auf die letzte Kopie `y` und die ursprüngliche Version der `x`. Gespeichert werden und wenn ein Lambda-Ausdruck erstellt wird, welche Kopie einer Variablen zum Zeitpunkt wurde, die sie erstellt wurde. Aus diesem Grund jede Lambda verwendet die gleiche freigegebene Kopie `x`, aber eine andere Kopie der `y`. Am Ende des Programms, wenn sie den Lambda-Ausdrücke ausgeführt wird, gemeinsam genutzt Kopie `x` , dass sie alle auf verweisen befindet sich nun bei den endgültigen Wert 3.

Beachten Sie, wenn kein Lambda-Ausdrücke oder eine LINQ-Ausdrücke vorhanden sind, dann ist es unmöglich zu wissen, dass eine neue Kopie auf Schleife Eintrag vorgenommen wird. In der Tat compileroptimierungen vermieden werden Kopien in diesem Fall. Beachten Sie auch, dass es nicht zulässig, `GoTo` in einer Schleife, die Lambda-Ausdrücken oder LINQ-Ausdrücke enthält.


### <a name="whileend-while-and-doloop-statements"></a>Während... End beim und möchten... Schleifenanweisungen

Ein `While` oder `Do` Schleife von anweisungsschleifen basierend auf einem booleschen Ausdruck.

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

Ein `While` Schleifenanweisung Schleifen, solange der boolesche Ausdruck True ergibt; eine `Do` Loop-Anweisung kann eine komplexere Bedingung enthalten. Nach ein Ausdruck platziert werden die `Do` Schlüsselwort oder nach der `Loop` -Schlüsselwort, aber nicht beide nach. Der boolesche Ausdruck wird ausgewertet, gemäß Abschnitt [boolesche Ausdrücke](expressions.md#boolean-expressions). (Hinweis: Dies erfordert keine den Ausdruck, der Boolesch sein). Es ist auch zulässig, die kein Ausdruck angeben. In diesem Fall wird die Schleife nie beendet werden. Wenn der Ausdruck, nach dem platziert wird `Do`, es vor der Ausführung des Schleifenblocks bei jeder Iteration ausgewertet werden. Wenn der Ausdruck, nach dem platziert wird `Loop`, es nach dem Ausführen des Schleifenblocks bei jeder Iteration ausgewertet werden. Platzieren den Ausdruck nach `Loop` generiert daher eine weitere Schleife als Platzierung nach dem `Do`. Das folgende Beispiel veranschaulicht dieses Verhalten:

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

```
Second Loop
```

Bei der ersten Schleife wird die Bedingung ausgewertet, bevor die Schleife ausgeführt wird. Bei der zweiten Schleife wird die Bedingung ausgeführt, nachdem die Schleife wird ausgeführt. Der bedingte Ausdruck muss mit einem Präfix einen `While` Schlüsselwort oder einem `Until` Schlüsselwort. Die erste unterbricht die Schleife, ergibt die Bedingung auf "false", der zweite Wert wird bei der Bedingung "true" ausgewertet wird.

__Beachten Sie.__ `Until` ist ein reserviertes Wort.


### <a name="fornext-statements"></a>Für... Nächste Anweisungen

Ein `For...Next` anweisungsschleifen basierend auf einem Satz von Grenzen. Ein `For` -Anweisung gibt, eine Schleifensteuerungsvariable, einen Ausdruck für die untere Grenze, einen Ausdruck für die obere Grenze und ein optionaler Schritt-Wert-Ausdruck. Die Loop-Steuerelementvariable angegeben ist, entweder über einen Bezeichner gefolgt von einem optionalen `As` -Klausel oder einen Ausdruck ein.

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

Gemäß den folgenden Regeln, die Schleifensteuerungsvariable bezieht sich entweder um eine neue lokale Variable, die bestimmten dieser `For...Next` -Anweisung oder eine vorhandene Variable oder einen Ausdruck.

* Die Loop-Steuerelementvariable ist ein Bezeichner mit einer `As` -Klausel der Bezeichner definiert eine neue lokale Variable des Typs im angegebenen die `As` -Klausel, beschränkt auf die gesamte `For` Schleife.

* Die Loop-Steuerelementvariable ist eine ID ohne eine `As` -Klausel, und klicken Sie dann auf den Bezeichner wird zuerst aufgelöst, mit der einfachen Regeln zur namensauflösung (finden Sie im Abschnitt [einfache Namen Ausdrücke](expressions.md#simple-name-expressions)), ausgenommen, die dieses Auftretens von Der Bezeichner nicht in und von sich selbst würde eine implizite lokale Variable erstellt werden soll (Abschnitt [implizite lokale Deklarationen](statements.md#implicit-local-declarations)).

  * Wenn diese Auflösung erfolgreich ist, und das Ergebnis wird als Variable klassifiziert, erfolgt die Loop-Steuerelementvariable, bereits vorhandene Variablen ab.

  * Wenn die Auflösung fehlschlägt oder wenn die Auflösung erfolgreich ist, und klicken Sie dann das Ergebnis wird als Typ klassifiziert:
    * Wenn lokale Variable Typrückschluss verwendet wird, definiert der Bezeichner eine neue lokale Variable, deren Typ aus der Grenze abgeleitet ist, und der step-Ausdrücke, die den gesamten Bereich `For` -Schleife
    * Wenn nicht lokale Variablen Typrückschluss verwendet wird, aber implizite Deklaration von lokalen, wird eine implizite lokale Variable erstellt wird, dessen Bereich die gesamte Methode ist (Abschnitt [implizite lokale Deklarationen](statements.md#implicit-local-declarations)), und die Schleifensteuerungsvariable bezieht sich auf diesen bereits vorhandenen Variablen.
    * Wenn weder die lokale Variable Typrückschluss als auch die implizite lokale Deklarationen verwendet werden, ist es ein Fehler auf.

  * Wenn Auflösung mit einem als weder auf einen Typ als auch auf eine Variable klassifiziert erfolgreich ist, ist es ein Fehler auf.

* Wenn die Schleifensteuerungsvariable ein Ausdruck ist, muss der Ausdruck als Variable klassifiziert werden.

Eine Schleifensteuerungsvariable kann nicht verwendet werden, indem Sie einen anderen einschließen `For...Next` Anweisung. Der Typ, der die Loop-Steuerelementvariable, der eine `For` -Anweisung bestimmt den Typ der Iteration und muss:

* `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`
* Ein enumerierter Typ
* `Object`
* Ein `T` , besitzt die folgenden Operatoren, wobei `B` ist ein Typ, der in einen booleschen Ausdruck verwendet werden kann:

```vb
Public Shared Operator >= (op1 As T, op2 As T) As B
Public Shared Operator <= (op1 As T, op2 As T) As B
Public Shared Operator - (op1 As T, op2 As T) As T
Public Shared Operator + (op1 As T, op2 As T) As T
```

Die Grenze und die Step-Ausdrücke müssen implizit in den Typ der Schleifensteuerungsvariablen konvertiert werden, und müssen klassifiziert werden, als Werte. Zum Zeitpunkt der Kompilierung wird der Typ der Schleifensteuerungsvariablen abgeleitet, durch den umfangreichsten Typ die Untergrenze, Obergrenze und Ausdruckstypen Schritt auswählen. Wenn keine widening-Konvertierung zwischen zwei Typen vorhanden ist, und klicken Sie dann ein Fehler während der Kompilierung auftritt.

Laufzeit, wenn der Typ der Schleifensteuerungsvariablen `Object`, und klicken Sie dann der Typ der Iteration abgeleitet wird der gleiche wie zum Zeitpunkt der Kompilierung mit zwei Ausnahmen. Wenn zunächst die Grenze auf Step-Ausdrücke sind "all" von ganzzahligen Typen jedoch keine allgemeinsten Typ haben, und allgemeinsten Typ, der umfasst alle drei Typen werden abgeleitet werden. Und dann der Typ der Schleifensteuerungsvariablen abgeleitet `String`, `Double` wird stattdessen abgeleitet werden. Wenn Sie zur Laufzeit keine Schleife Steuerelementtyp bestimmt werden kann, oder wenn einer der Ausdrücke für den Steuerelementtyp "Schleife" konvertiert werden kann eine `System.InvalidCastException` erfolgt. Sobald ein Schleifentyp des Steuerelements am Anfang der Schleife ausgewählt wurde, wird während der Iteration, unabhängig von Änderungen an den Wert in der Loop-Steuerelementvariable desselben Typs verwendet.

Ein `For` Anweisung muss geschlossen werden, durch eine entsprechende `Next` Anweisung. Ein `Next` -Anweisung ohne eine Variable entspricht die innerste Open `For` -Anweisung, während eine `Next` -Anweisung mit der eine oder mehrere Variablen der Schleife-Steuerelement, von links nach rechts, entspricht die `For` Schleifen, die jede Variable entsprechen. Wenn eine Variable entspricht einem `For` Schleife, die nicht die am häufigsten geschachtelte Schleife zu diesem Zeitpunkt ist ein Fehler während der Kompilierung führt.

Am Anfang der Schleife, die drei Ausdrücke werden ausgewertet, in der Reihenfolge im Text, und der Ausdruck für die untere Grenze der Loop-Steuerelementvariable zugewiesen wird. Wenn der Step-Wert weggelassen wird, ist implizit das Literal `1`, in den Typ der Schleifensteuerungsvariablen konvertiert. Die drei Ausdrücke werden nur am Anfang der Schleife ausgewertet.

Am Anfang des foreach-Schleife, ist die Steuerelementvariable verglichen, um festzustellen, ob es größer als der Endpunkt ist die Schrittausdruck positiv oder kleiner als der Endpunkt, wenn der Schrittausdruck negativ ist. Wenn es sich handelt, die `For` Schleife beendet wird; andernfalls der Schleifenblock wird ausgeführt. Wenn die Schleifensteuerungsvariable kein primitiver Typ ist, richtet sich der Vergleichsoperator nach, ob der Ausdruck `step >= step - step` ist "true" oder "false". Auf der `Next` -Anweisung Step-Wert ist die Steuerelementvariable hinzugefügt und an den Anfang der Schleife zurückgibt.

Beachten Sie, dass eine neue Kopie der Loop-Steuerelementvariable *nicht* bei jeder Iteration der Schleifenblock erstellt. In dieser Hinsicht die `For` Anweisung unterscheidet sich von `For Each` (Abschnitt [For Each…Next Nächste Anweisungen](statements.md#for-eachnext-statements)).

Es ist nicht zulässig, die eine Verzweigung in einem `For` eine Schleife von außerhalb der Schleife.


### <a name="for-eachnext-statements"></a>Für jede... Nächste Anweisungen

Ein `For Each...Next` anweisungsschleifen auf der Grundlage der Elemente in einem Ausdruck. Ein `For Each` -Anweisung gibt, eine Schleifensteuerungsvariable und ein Enumeratorausdruck. Die Loop-Steuerelementvariable angegeben ist, entweder über einen Bezeichner gefolgt von einem optionalen `As` -Klausel oder einen Ausdruck ein.

```antlr
ForEachStatement
    : 'For' 'Each' LoopControlVariable 'In' LineTerminator? Expression StatementTerminator
      Block?
      ( 'Next' NextExpressionList? StatementTerminator )?
    ;
```

Befolgen die gleichen Regeln wie `For...Next` Anweisungen (Abschnitt [für... Nächste Anweisungen](statements.md#fornext-statements)), die Schleifensteuerungsvariable verweist entweder auf eine neue lokale Variable, die bestimmten dieser für jede... Next-Anweisung, oder eine vorhandene Variable oder einen Ausdruck.

Enumeratorausdruck muss als Wert klassifiziert werden, und der Typ muss vom Auflistungstyp oder `Object`. Wenn der Typ des Enumeratorausdrucks `Object`, und klicken Sie dann die gesamte Verarbeitung bis zur Laufzeit verzögert wird. Andernfalls muss eine Konvertierung vom Elementtyp der Auflistung in den Typ der Schleifensteuerungsvariablen vorhanden sein.

Die Schleifensteuerungsvariable kann nicht verwendet werden, indem Sie einen anderen einschließen `For Each` Anweisung. Ein `For Each` Anweisung muss geschlossen werden, durch eine entsprechende `Next` Anweisung. Ein `Next` -Anweisung ohne eine Schleifensteuerungsvariable entspricht der innersten, offenen `For Each`. Ein `Next` -Anweisung mit der eine oder mehrere Variablen der Schleife-Steuerelement, von links nach rechts, entspricht die `For Each` Schleifen, die die gleichen Loop-Steuerelementvariable haben. Wenn eine Variable entspricht einer `For Each` Schleife, die nicht die am häufigsten geschachtelte Schleife zu diesem Zeitpunkt ist ein Fehler während der Kompilierung auftritt.

Ein Typ `C` gilt eine *Auflistungstyp* eine von:

* Die Folgendes zutrifft:
  * `C` enthält eine zugängliche Instanzmethode, freigegeben oder eine Erweiterungsmethode mit der Signatur `GetEnumerator()` , die einen Typ zurückgibt `E`.
  * `E` enthält eine zugängliche Instanzmethode, freigegeben oder eine Erweiterungsmethode mit der Signatur `MoveNext()` sowie des Rückgabetyps `Boolean`.
  * `E` enthält eine zugängliche Instanzmethode oder eine freigegebene Eigenschaft, die mit dem Namen `Current` , die einen Getter hat. Der Typ dieser Eigenschaft ist der Elementtyp des Auflistungstyps.

* Sie implementiert die Schnittstelle `System.Collections.Generic.IEnumerable(Of T)`, wobei der Elementtyp der Sammlung betrachtet wird `T`.

* Sie implementiert die Schnittstelle `System.Collections.IEnumerable`, wobei der Elementtyp der Sammlung betrachtet wird `Object`.

Es folgt ein Beispiel für eine Klasse, die aufgelistet werden kann:

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

Vor dem Beginn die Schleife wird der Enumeratorausdruck ausgewertet. Wenn der Typ des Ausdrucks erfüllt nicht die Entwurfsmuster, wird der Ausdruck umgewandelt wird `System.Collections.IEnumerable` oder `System.Collections.Generic.IEnumerable(Of T)`. Wenn der Ausdruckstyp die generische Schnittstelle implementiert, die generische Schnittstelle wird zum Zeitpunkt der Kompilierung bevorzugt, aber die nicht generische Schnittstelle wird zur Laufzeit bevorzugt. Wenn der Ausdruckstyp mehrere Male die generische Schnittstelle implementiert, die Anweisung gilt nicht eindeutig, und ein Fehler während der Kompilierung auftritt.

__Beachten Sie.__ Die nicht generische Schnittstelle wird im spät gebundenen Fall bevorzugt, da die generische Schnittstelle auswählen bedeutet, dass alle Aufrufe der Schnittstellenmethoden Typparameter erforderlich machen würde. Da es nicht wissen, den entsprechenden Argumenten zur Laufzeit zu geben, müssten alle Aufrufe an. verwenden die spät gebundene Aufrufe. Dies wäre langsamer als die nicht generische Schnittstelle aufrufen, da die nicht generische Schnittstelle mithilfe von Aufrufen der Kompilierung aufgerufen werden kann.

`GetEnumerator` wird aufgerufen, auf den sich ergebenden Wert und der Rückgabetyp Wert der Funktion wird in ein temporäres gespeichert. Und dann am Anfang jeder Iteration `MoveNext` für die temporäre aufgerufen wird. Wenn sie zurückgibt `False`, die Schleife wird beendet. Andernfalls wird jede Iteration der Schleife wie folgt ausgeführt:

1. Wenn die Schleifensteuerungsvariable eine neue lokale Variable (anstelle einer bereits vorhandenen) identifiziert, wird dann eine neue Kopie dieser lokalen Variablen erstellt. Für die aktuelle Iteration werden alle Verweise innerhalb des Schleifenblocks auf diese Kopie verweisen.
2. Die `Current` Eigenschaft wird abgerufen, umgewandelt in den Typ der Schleifensteuerungsvariablen (unabhängig davon, ob die Konvertierung implizit oder explizit), und die Schleifensteuerungsvariable zugewiesen.
3. Die Schleifenblock wird ausgeführt.

__Beachten Sie.__ Es ist eine geringfügige Änderung im Verhalten zwischen der Version 10.0 und 11.0 der Sprache. Vor dem 11.0, wurde eine neue Iterationsvariable *nicht* erstellt für jede Iteration der Schleife. Dieser Unterschied ist die Observable-Objekt nur dann, wenn die Iterationsvariable erfasst wird, durch einen Lambda-Ausdruck oder eine LINQ-Ausdruck, der dann nach der Schleife aufgerufen:

```vb
Dim lambdas As New List(Of Action)
For Each x In {1,2,3}
   lambdas.Add(Sub() Console.WriteLine(x)
Next
lambdas(0).Invoke()
lambdas(1).Invoke()
lambdas(2).Invoke()
```

Bis zu Visual Basic 10.0, dies wurde eine Warnung ausgegeben, zum Zeitpunkt der Kompilierung und gedruckt "3" drei Mal. Das war, da gab es nur eine einzelne Variable "X" freigegeben, die für alle Iterationen der Schleife, und alle drei Lambdas erfasst das gleiche "X" mit der Zeit, die den Lambda-Ausdrücke ausgeführt wurden sie dann die Zahl 3 gespeichert. Ab Visual Basic-11.0 gibt sie "1, 2, 3". Das ist da jeder Lambda "X" über eine andere Variable erfasst.


__Beachten Sie.__ Das aktuelle Element der Iteration wird in den Typ der Schleifensteuerungsvariablen konvertiert, auch wenn die Konvertierung explizit ist, da keine praktische Möglichkeit besteht, einen Konvertierungsoperator in der Anweisung einzuführen. Dies war besonders problematischen, bei der Arbeit mit der jetzt veralteten Typ `System.Collections.ArrayList`, da der Elementtyp ist der `Object`. Dies müsste erforderlichen Umwandlungen in sehr viele Schleifen, etwa die unserer Meinung nach war nicht ideal. Ironischerweise Generika aktiviert die Erstellung einer stark typisierten Auflistung und `System.Collections.Generic.List(Of T)`, die möglicherweise haben wir denken Sie an diesem Punkt Design, aber aus Gründen der Kompatibilität kann nicht dadurch jetzt geändert werden.


Wenn die `Next` Ausführung-Anweisung erreicht wird, zurückgegeben werden, am Anfang der Schleife. Wenn eine Variable angegeben wird, nach der `Next` -Schlüsselwort, es muss identisch sein als die erste Variable nach der `For Each`. Beachten Sie z. B. folgenden Code:

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

Wenn der Typ `E` des Enumerators implementiert `System.IDisposable`, und klicken Sie dann der Enumerator, beim Beenden der schleifenstatus verworfen wird durch Aufrufen der `Dispose` Methode. Dadurch wird sichergestellt, dass der Enumerator reservierten Ressourcen freigegeben werden. Wenn die Methode mit der `For Each` Anweisung nicht strukturierte Fehlerbehandlung, nicht verwendet und dann die `For Each` Anweisung umgeben eine `Try` -Anweisung mit der `Dispose` Methode mit dem Namen der `Finally` auf Bereinigung zu gewährleisten.

__Beachten Sie.__ Die `System.Array` Typ ist ein Sammlungstyp und da Arrays alle Typen abgeleitet `System.Array`, jeder Ausdruck der Array-Typ ist zulässig, einem `For Each` Anweisung. Für eindimensionale Arrays die `For Each` Anweisung listet die Elemente in aufsteigender Indexreihenfolge, mit dem Index 0 beginnt und endet mit Index-Länge - 1. Für mehrdimensionale Arrays werden die Indizes von die Dimension ganz rechts zuerst erhöht.

Der folgende code beispielsweise druckt `1 2 3 4`:

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

Es ist nicht zulässig, die eine Verzweigung in einem `For Each` Anweisungsblock von außerhalb des Blocks.


## <a name="exception-handling-statements"></a>Behandlung von Ausnahmen-Anweisungen

Visual Basic unterstützt strukturierte und unstrukturierte Ausnahmebehandlung. Nur eine Art der Ausnahmebehandlung kann verwendet werden, in einer Methode, aber die `Error` Anweisung kann in die strukturierte Ausnahmebehandlung verwendet werden. Wenn eine Methode für beide Arten der Ausnahmebehandlung verwendet, führt ein Fehler während der Kompilierung.

```antlr
ErrorHandlingStatement
    : StructuredErrorStatement
    | UnstructuredErrorStatement
    ;
```

### <a name="structured-exception-handling-statements"></a>Strukturierte Ausnahmebehandlung-Anweisungen

Strukturierte Ausnahmebehandlung ist eine Methode zur Handhabung von Fehlern durch das Deklarieren von expliziten Blöcke, die in denen bestimmte Ausnahmen behandelt werden. Strukturierte Ausnahmebehandlung erfolgt über eine `Try` Anweisung.

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

Ein `Try` Anweisung besteht aus drei verschiedenen Arten von Blöcken: try-Blöcke, catch-Blöcken, und finally-Blöcken. Ein *try-Block* ist ein Anweisungsblock ein, die die auszuführenden Anweisungen enthält. Ein *catch-Block* ein Anweisungsblock aus, die die Ausnahme behandelt wird. Ein *finally-block* ist ein Anweisungsblock mit Anweisungen aus, um die Ausführung werden die `Try` Anweisung beendet wird, unabhängig davon, ob eine Ausnahme aufgetreten ist, und behandelt wurde. Ein `Try` -Anweisung, die nur einen Try-Block und einem finally enthalten kann blockieren, müssen mindestens einen Catch-Block enthalten oder finally-block. Es ist unzulässig, die Ausführung explizit in einen Try-Block mit Ausnahme von in einem Catch-Block in der gleichen Anweisung übertragen.


#### <a name="finally-blocks"></a>Finally-Blöcke

Ein `Finally` Block wird immer ausgeführt, wenn die Ausführung eines beliebigen Teils der `Try` Anweisung. Zur Ausführung ist keine bestimmte Handlung erforderlich. die `Finally` des Blocks aufheben; Wenn die Ausführung verlässt die `Try` -Anweisung, die das System wird automatisch ausgeführt. die `Finally` blockieren, und klicken Sie dann die Ausführung an das vorgesehene Ziel übertragen. Die `Finally` Block wird ausgeführt, unabhängig davon, wie die Ausführung erfolgt die `Try` Anweisung: bis zum Ende der `Try` blockieren, bis zum Ende einer `Catch` blockieren, über eine `Exit Try` Anweisung, über eine `GoTo` -Anweisung oder durch eine ausgelöste Ausnahme nicht behandelt.

Beachten Sie, dass die `Await` Ausdruck in einer Async-Methode und die `Yield` -Anweisung in eine Iteratormethode, kann dazu führen, dass ablaufsteuerung in die Async- oder Iterator-Methodeninstanz anhalten und fortsetzen in einer anderen Methodeninstanz. Dies ist jedoch lediglich führt zu einer Unterbrechung der Ausführung und umfasst jedoch nicht das Beenden der Instanz entsprechende Async-Methode oder ein Iterator-Methode, und verursacht also keine `Finally` Blöcke ausgeführt werden.

Ist ungültig, die explizit die Ausführung in übertragen einer `Finally` blockieren; es ist ebenfalls ungültig. um die Ausführung von übertragen ein `Finally` block, außer durch eine Ausnahme.

```antlr
FinallyStatement
    : 'Finally' StatementTerminator
      Block?
    ;
```

#### <a name="catch-blocks"></a>catch-Blöcke

Wenn eine Ausnahme, während der Verarbeitung auftritt der `Try` blockieren, von denen jede `Catch` Anweisung wird untersucht, in der Reihenfolge im Text zu bestimmen, ob sie die Ausnahme behandelt.

```antlr
CatchStatement
    : 'Catch' ( Identifier ( 'As' NonArrayTypeName )? )?
      ( 'When' BooleanExpression )? StatementTerminator
      Block?
    ;
```

Der Bezeichner angegeben wird, einem `Catch` Klausel stellt die Ausnahme dar, die ausgelöst wurde. Wenn der Bezeichner enthält ein `As` gilt als-Klausel, und klicken Sie dann auf den Bezeichner deklariert werden, in der `Catch` lokalen Deklarationsabschnitt des Blocks. Andernfalls muss der Bezeichner eine lokale Variable (nicht statische Variable), die in einem enthaltenden Block definiert wurde.

Ein `Catch` -Klausel ohne Bezeichner fängt alle Ausnahmen, die von abgeleiteten `System.Exception`. Ein `Catch` Klausel mit einer ID, fängt nur Ausnahmen, deren Typen entsprechen den oder vom Typ des Bezeichners abgeleitet. Der Typ muss `System.Exception`, oder ein abgeleiteter Typ von `System.Exception`. Wann wird eine Ausnahme, die von abgeleitet `System.Exception`, ein Verweis auf das Ausnahmeobjekt wird gespeichert, in dem Objekt, das von der Funktion zurückgegebene `Microsoft.VisualBasic.Information.Err`.

Ein `Catch` -Klausel mit einer `When` -Klausel nur Ausnahmen abgefangen, wenn der Ausdruck ergibt `True`; der Typ des Ausdrucks muss einen booleschen Ausdruck gemäß Abschnitt [boolesche Ausdrücke](expressions.md#boolean-expressions). Ein `When` -Klausel wird nur angewendet, nachdem der Typ der Ausnahme überprüft, und der Ausdruck bezieht sich möglicherweise auf den Bezeichner, der die Ausnahme darstellt wie dieses Beispiel veranschaulicht:

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

In diesem Beispiel gibt:

```
Third handler
```

Wenn eine `Catch` Klausel wird die Ausnahme behandelt, überträgt die Ausführung an die `Catch` Block. Am Ende der `Catch` blockieren, Ausführung Datenübertragungen an den der ersten Anweisung nach der `Try` Anweisung. Die `Try` Anweisung werden Ausnahmen nicht behandeln eine `Catch` Block. Wenn kein `Catch` -Klausel die Ausnahme behandelt, überträgt der Ausführung an einem Speicherort, der durch das System bestimmt.

Es ist nicht zulässig, die explizit die Ausführung in übertragen einer `Catch` Block.

Die Filter in Klauseln wenn normalerweise vor der Ausnahme, die ausgelöst wird, ausgewertet werden. Der folgende Code gibt beispielsweise "Filter, schließlich Catch".

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

 Async-und Iteratormethoden führt allerdings alle finally-Blöcke in der sie vor dem Filter außerhalb ausgeführt werden. Beispielsweise hätte der obige Code `Async Sub Foo()`, lautet die Ausgabe "Schließlich zu filtern, Catch".


#### <a name="throw-statement"></a>Throw-Anweisung

Die `Throw` -Anweisung löst eine Ausnahme, die von einer Instanz von abgeleiteten Typs dargestellt wird `System.Exception`.

```antlr
ThrowStatement
    : 'Throw' Expression? StatementTerminator
    ;
```

Wenn der Ausdruck nicht als Wert klassifiziert wird oder kein Typ abgeleitet sind ist `System.Exception`, und klicken Sie dann ein Fehler während der Kompilierung auftritt. Wenn der Ausdruck mit einem null-Wert zur Laufzeit ausgewertet und dann eine `System.NullReferenceException` Ausnahme ausgelöst.

Ein `Throw` Anweisung kann weglassen, den Ausdruck in einem Catch-Block, der eine `Try` finally-Anweisung, solange es ist keine dazwischen liegenden block. In diesem Fall löst die Ausnahme, die gerade verarbeitet wird, im Catch-Block von die Anweisung erneut aus. Zum Beispiel:

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


### <a name="unstructured-exception-handling-statements"></a>Unstrukturierte Ausnahmebehandlung-Anweisungen

Unstrukturierte Ausnahmebehandlung ist eine Methode zur Handhabung von Fehlern durch, der angibt, Anweisungen, um zum Verzweigen, wenn eine Ausnahme auftritt. Unstrukturierte Ausnahmebehandlung wird mithilfe von drei Anweisungen implementiert: die `Error` -Anweisung, die `On Error` -Anweisung, und die `Resume` Anweisung.

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

Wenn eine Methode nicht strukturierte Ausnahmebehandlung verwendet wird, wird ein einzelnes strukturierter Ausnahmehandler für die gesamte Methode eingerichtet, die alle Ausnahmen abfängt. (Beachten Sie, dass in Konstruktoren, die dieser Handler nicht über den Aufruf an den Aufruf hinausreicht `New` am Anfang des Konstruktors.) Die Methode nachverfolgung von klicken Sie dann den Speicherort für die letzte Ausnahme-Handler und die letzte Ausnahme, die ausgelöst wurde. Am Anfang der Methode, den Speicherort der Ausnahme-Handler und die Ausnahme sind festgelegt auf `Nothing`. Wenn eine Ausnahme in einer Methode ausgelöst wird, die unstrukturierte Ausnahmebehandlung verwendet wird, befindet sich ein Verweis auf das Ausnahmeobjekt in das Objekt, das von der Funktion zurückgegebene `Microsoft.VisualBasic.Information.Err`.

Unstrukturierte Fehlerbehandlung-Anweisungen sind in Iterator- oder Async-Methoden nicht zulässig.


#### <a name="error-statement"></a>Error-Anweisung

Ein `Error` Anweisung löst einen `System.Exception` Ausnahme, die mit einer Anzahl der Visual Basic 6-Ausnahme. Der Ausdruck muss als Wert klassifiziert werden, und der Typ muss implizit in `Integer`.

```antlr
ErrorStatement
    : 'Error' Expression StatementTerminator
    ;
```

#### <a name="on-error-statement"></a>On Error-Anweisung

Ein `On Error` Anweisung ändert die its most recent State für die Behandlung von Ausnahmen.

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

Sie können auf vier Arten verwendet werden:

* `On Error GoTo -1` setzt die letzte Ausnahme auf `Nothing`.

* `On Error GoTo 0` Speicherort für die letzte Ausnahme-Handler setzt `Nothing`.

* `On Error GoTo LabelName` Legt die Bezeichnung als Speicherort für die letzte Ausnahme-Handler. Diese Anweisung kann nicht in einer Methode verwendet werden, die einen Lambda- oder Abfrageausdruck enthält.

* `On Error Resume Next` Richtet die `Resume Next` Verhalten als Speicherort für die letzte Ausnahme-Handler.


#### <a name="resume-statement"></a>Resume-Anweisung

Ein `Resume` Anweisung setzt die Ausführung der Anweisung, die die letzte Ausnahme verursacht hat.

```antlr
ResumeStatement
    : 'Resume' ResumeClause? StatementTerminator
    ;

ResumeClause
    : 'Next'
    | LabelName
    ;
```

Wenn die `Next` Ausführung Modifizierer angegeben ist, zurückgegeben werden, um die Anweisung, die nach der Anweisung ausgeführt worden wären, die die letzte Ausnahme verursacht hat. Wenn Sie ein Bezeichnungsnamen angegeben ist, gibt die Ausführung an die Bezeichnung.

Da die `SyncLock` -Anweisung enthält einen implizite strukturierten Fehlerbehandlung Block `Resume` und `Resume Next` haben spezielle Verhaltensweisen für Ausnahmen, die in auftreten `SyncLock` Anweisungen. `Resume` Gibt die Ausführung an den Anfang der `SyncLock` -Anweisung während `Resume Next` gibt die Ausführung an die nächste Anweisung nach der `SyncLock` Anweisung. Beachten Sie z. B. folgenden Code:

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

Das folgende Ergebnis ausgegeben.

```
Before exception
Before exception
After SyncLock
```

Beim ersten Durchlaufen der `SyncLock` Anweisung `Resume` gibt die Ausführung an den Anfang der `SyncLock` Anweisung. Beim zweiten Durchlaufen der `SyncLock` Anweisung `Resume Next` gibt die Ausführung bis zum Ende der `SyncLock` Anweisung. `Resume` und `Resume Next` dürfen nicht innerhalb einer `SyncLock` Anweisung.

In allen Fällen bei einem `Resume` -Anweisung ausgeführt wird, ist die letzte Ausnahme festgelegt `Nothing`. Wenn eine `Resume` -Anweisung wird ausgeführt, ohne die letzte eine Ausnahme ist, löst die Anweisung eine `System.Exception` Ausnahme, die mit der Anzahl der Visual Basic-Fehler `20` (ohne Fehler fortsetzen).


## <a name="branch-statements"></a>Branchanweisungen

Branchanweisungen ändern Sie den Ablauf der Ausführung in einer Methode. Es gibt sechs branchanweisungen ein:

1. Ein `GoTo` Anweisung wird die Ausführung an der angegebenen Bezeichnung in der Methode zu übertragen. Es ist nicht zulässig, `GoTo` in einem `Try`, `Using`, `SyncLock`, `With`, `For` oder `For Each` Block noch in einer Schleife der Blöcke, wenn eine lokale Variable des Blocks in einem Lambda-Ausdruck oder eine LINQ-Ausdruck erfasst wird.
2. Ein `Exit` -Anweisung überträgt die Ausführung an die nächste Anweisung nach dem Ende des direkt enthaltenden Block-Anweisung der angegebenen Art. Wenn der Block ist die Methode, ablaufsteuerung beendet die Methode an, wie im Abschnitt beschrieben [Ablaufsteuerung](statements.md#control-flow). Wenn die `Exit` Anweisung befindet sich nicht in der Art von Block angegeben in der Anweisung ein Fehler während der Kompilierung auftritt.
3. Ein `Continue` -Anweisung überträgt die Ausführung bis zum Ende des direkt enthaltenden Block Loop-Anweisung der angegebenen Art. Wenn die `Continue` Anweisung befindet sich nicht in der Art von Block angegeben in der Anweisung ein Fehler während der Kompilierung auftritt.
4. Ein `Stop` Anweisung bewirkt, dass eine Debuggerausnahme auftritt.
5. Ein `End` -Anweisung beendet die Anwendung. Finalizer werden vor dem Herunterfahren ausgeführt, aber die finally-Blöcken der derzeit ausgeführten `Try` Anweisungen werden nicht ausgeführt. Diese Anweisung kann nicht in Programmen verwendet werden, die keine ausführbare Datei (z. B. DLLs) sind.
6. Ein `Return` Anweisung ohne Ausdruck entspricht einer `Exit Sub` oder `Exit Function` Anweisung. Ein `Return` -Anweisung mit einem Ausdruck ist nur zulässig, in eine normale Methode, die eine Funktion ist oder in einer asynchronen Methode, die eine Funktion mit Rückgabetyp `Task(Of T)` für einige `T`. Der Ausdruck muss klassifiziert werden, als ein Wert, der implizit in den *Funktion Rückgabevariablen* (im Fall von regulären Methoden) oder die *aufgabenrückgabevariable* (im Fall von Async-Methoden) . Das Verhalten ist der Ausdruck, ausgewertet werden soll Sie in der Rückgabevariablen zu speichern, anschließend führen Sie ein implizites `Exit Function` Anweisung.

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

## <a name="array-handling-statements"></a>Array-Fehlerbehandlungsanweisungen

Zwei Anweisungen vereinfachen die Arbeit mit Arrays: `ReDim` Anweisungen und `Erase` Anweisungen.

```antlr
ArrayHandlingStatement
    : RedimStatement
    | EraseStatement
    ;
```

### <a name="redim-statement"></a>ReDim-Anweisung

Ein `ReDim` Anweisung instanziiert die neuen Arrays.

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

Jede Klausel in der Anweisung muss als eine Variable oder ein Eigenschaftenzugriff, dessen Typ ein Arraytyp ist, klassifiziert oder `Object`, und eine Liste der Arraygrenzen gefolgt werden. Die Anzahl der Grenzen muss mit dem Datentyp der Variablen konsistent sein; ist eine beliebige Anzahl von Grenzen für zulässig `Object`. Ein Array ist zur Laufzeit instanziiert, die für die einzelnen Ausdrücke von links nach rechts mit der angegebenen Begrenzungen und klicken Sie dann die Variable oder Eigenschaft zugewiesen. Wenn der Variablentyp ist `Object`, die Anzahl der Dimensionen ist die Anzahl der angegebenen Dimensionen und den Elementtyp des Arrays ist `Object`. Tritt auf, ein Fehler während der Kompilierung, wenn die angegebene Anzahl von Dimensionen, die zur Laufzeit mit der Variablen oder Eigenschaft nicht kompatibel ist. Zum Beispiel:

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

Wenn die `Preserve` Schlüsselwort angegeben ist, und klicken Sie dann die Ausdrücke zudem klassifizierbare als Wert muss, und die neue Größe für jede Dimension, mit Ausnahme des letzten muss die Größe des vorhandenen Arrays identisch sein. Die Werte im vorhandenen Array in das neue Array kopiert werden: Wenn das neue Array kleiner ist, werden die vorhandenen Werte verworfen. Wenn das neue Array größer ist, werden zusätzliche Elemente auf den Standardwert, der den Elementtyp des Arrays initialisiert. Beachten Sie z. B. folgenden Code:

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

Gibt das folgende Ergebnis:

```
3, 0
```

Die vorhandenen arrayverweises ein null-Wert zur Laufzeit ist, dass kein Fehler erhält. Als die Dimension, auf die ganz rechts, wenn die Größe einer Dimension geändert wird eine `System.ArrayTypeMismatchException` ausgelöst.

__Beachten Sie.__ `Preserve` ist ein reserviertes Wort.


### <a name="erase-statement"></a>Erase-Anweisung

Ein `Erase` Anweisung legt alle Arrayvariablen oder Eigenschaften, die in der Anweisung angegebene `Nothing`. Jeder Ausdruck in der Anweisung muss klassifiziert werden, wie eine Variable oder eine Eigenschaft, deren Typ ein Arraytyp ist, oder `Object`. Zum Beispiel:

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

Instanzen von Typen werden automatisch vom Garbage Collector freigegeben, wenn eine Sammlung ausgeführt wird, und keine aktiven Verweise auf die Instanz gefunden werden. Wenn ein Typ für eine besonders nützliche und knappe Ressource (z. B. Datenbankverbindungen oder Dateihandles) enthält, kann es nicht wünschenswert sein, warten Sie, bis die nächste Garbagecollection einer bestimmten Instanz des Typs bereinigen, die nicht mehr verwendet wird. Um eine einfache Möglichkeit des Freigebens von Ressourcen, bevor Sie eine Sammlung bereitstellen, kann ein Typ implementieren die `System.IDisposable` Schnittstelle. Ein Typ, der ist dies der Fall ist, stellt eine `Dispose` -Methode, die aufgerufen werden kann, um wertvolle Ressourcen freigegeben werden, sofort, die als solche zu erzwingen:

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

Die `Using` Anweisung automatisiert den Prozess der Erwerb einer Ressourcensatzes, eine Reihe von Anweisungen ausgeführt und dann die Ressource freigegeben. Die Anweisung kann zwei Formen annehmen: ein Element die Ressource ist eine lokale Variable, die als Teil der Anweisung deklariert und als einer regulären lokalen Variablendeklaration-Anweisung behandelt Bei der anderen ist die Ressource für das Ergebnis eines Ausdrucks.

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

Wenn die Ressource wird von einer lokalen Variablendeklaration-Anweisung, und klicken Sie dann der Typ des der Deklaration lokaler Variablen muss ein Typ, der implizit in konvertiert werden kann `System.IDisposable`. Die deklarierten lokalen Variablen sind schreibgeschützt, beschränkt auf die `Using` Anweisung blockieren und muss einen Initialisierer enthalten. Wenn die Ressource das Ergebnis eines Ausdrucks ist wird der Ausdruck muss als Wert klassifiziert werden und muss von einem Typ sein, der implizit konvertiert werden kann `System.IDisposable`. Der Ausdruck wird zu Beginn der Anweisung nur einmal ausgewertet.

Die `Using` Block befindet sich implizit durch eine `Try` dessen finally-block Anweisung ruft die Methode `IDisposable.Dispose` für die Ressource. Dadurch wird sichergestellt, dass die Ressource freigegeben wird, selbst wenn eine Ausnahme ausgelöst wird. Daher ist ungültig in branch ein `Using` Blockieren von außerhalb des Blocks auf, und ein `Using` Block wird als einzelne Anweisung behandelt, im Rahmen `Resume` und `Resume Next`. Wenn die Ressource ist `Nothing`, klicken Sie dann ohne Aufruf `Dispose` erfolgt. Das Beispiel:

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

Ein `Using` -Anweisung mit einer lokalen Variablendeklaration-Anweisung kann mehrere Ressourcen abrufen, zu einem Zeitpunkt, d.h. entspricht dies geschachtelten `Using` Anweisungen.  Z. B. eine `Using` -Anweisung der Form:

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


## <a name="await-statement"></a>Await-Anweisung

Eine Await-Anweisung weist die gleiche Syntax als Ausdruck "await"-Operator (Abschnitt [Await-Operator](expressions.md#await-operator)), ist nur in Methoden, die auch, Await-Ausdrücken ermöglichen zulässig und hat das gleiche Verhalten wie ein Await-Operator-Ausdruck.

Allerdings können sie als Wert oder "void" klassifiziert werden. Jeder Wert mit dem Ergebnis der Auswertung des Ausdrucks "await"-Operator wird verworfen.

```antlr
AwaitStatement
    : AwaitOperatorExpression StatementTerminator
    ;
```

## <a name="yield-statement"></a>Yield-Anweisung

Yield-Anweisungen beziehen sich auf die Iterator-Methoden, die in Abschnitt beschrieben werden [Iteratormethoden](statements.md#iterator-methods).

```antlr
YieldStatement
    : 'Yield' Expression StatementTerminator
    ;
```

`Yield` ist ein reserviertes Wort, verfügt die unmittelbar einschließende Methode oder der Lambda-Ausdruck, in dem er angezeigt wird, ein `Iterator` Modifizierer, und wenn die `Yield` angezeigt wird, `Iterator` Modifizierer; es an anderer Stelle nicht reserviert ist. Es ist auch in präprozessoranweisungen nicht reserviert. Die Yield-Anweisung ist nur im Text einer Methode oder einem Lambda-Ausdruck zulässig, in denen es sich um ein reserviertes Wort ist. In der unmittelbar einschließenden Methode oder einem Lambdaausdruck, die Yield-Anweisung kann nicht auftreten, innerhalb des Texts einer `Catch` oder `Finally` Block noder innerhalb der Text der ein `SyncLock` Anweisung.

Die Yield-Anweisung erhält einen einzelnen Ausdruck, der als Wert klassifiziert werden muss und dessen Typ implizit in den Typ des, der *aktuelle Iteratorvariable* (Abschnitt [Iteratormethoden](statements.md#iterator-methods)) von der Einschließen von Iterator-Methode.

Ablaufsteuerung nur erreicht eine `Yield` Anweisung bei der `MoveNext` Methode für ein Iterator-Objekt aufgerufen wird. (Dies ist, da eine Instanz der Iterator-Methode immer nur die Anweisungen aufgrund von ausgeführt wird die `MoveNext` oder `Dispose` Methoden, die von einem iteratorobjekt aufgerufen wird und die `Dispose` Methode wird nur einmal ausführen von Code in `Finally` -Blöcken, in dem `Yield` ist nicht zulässig).

Wenn eine `Yield` Anweisung ausgeführt wird, wird der Ausdruck ausgewertet und in gespeicherten der *aktuelle Iteratorvariable* von der Methode, mit dem iteratorobjekt verknüpfte Iteratorinstanz. Der Wert `True` wird zurückgegeben, um die aufrufende Instanz für `MoveNext`, und der Kontrollpunkt der dieser Instanz beendet wird, gelangt sind, bis des nächsten Aufrufs von `MoveNext` auf dem Iterator-Objekt.

