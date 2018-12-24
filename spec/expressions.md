# <a name="expressions"></a>Ausdrücke

Ein Ausdruck ist eine Sequenz von Operatoren und Operanden, der angibt, dass einer Berechnung eines Werts festgelegt oder eine Variable oder Konstante. In diesem Kapitel wird die Syntax, die Reihenfolge der Auswertung von Operanden und Operatoren und Bedeutung von Ausdrücken definiert.

```antlr
Expression
    : SimpleExpression
    | TypeExpression
    | MemberAccessExpression
    | DictionaryAccessExpression
    | InvocationExpression
    | IndexExpression
    | NewExpression
    | CastExpression
    | OperatorExpression
    | ConditionalExpression
    | LambdaExpression
    | QueryExpression
    | XMLLiteralExpression
    | XMLMemberAccessExpression
    ;
```

## <a name="expression-classifications"></a>Ausdrucksklassifizierungen

Jeder Ausdruck wird als eine der folgenden klassifiziert:

* *Ein-Wert.* Jeder Wert verfügt über einen zugeordneten Typ.

* *Eine Variable.* Jede Variable hat einen zugeordneten Typ, d. h. den deklarierten Typ der Variablen.

* *Ein Namespace.* Ein Ausdruck mit dieser Klassifizierung kann nur als linke Seite der Memberzugriff stehen. In einem anderen Kontext bewirkt, dass ein Ausdruck, der als Namespace klassifiziert einen Fehler während der Kompilierung.

* *Ein Typ.* Ein Ausdruck mit dieser Klassifizierung kann nur als linke Seite der Memberzugriff stehen. In einem anderen Kontext bewirkt, dass ein Ausdruck, der als Typ klassifiziert einen Fehler während der Kompilierung.

* *Eine Methodengruppe* ist eine Gruppe von Methoden, die auf dem gleichen Namen zu überladen. Eine Methodengruppe kann es sich um einen Ausdruck für die zielbereitstellungsumgebung und einer Argumentliste für den zugeordneten Typ haben.

* *Ein Methodenzeiger* steht für den Speicherort einer Methode. Ein Methodenzeiger kann es sich um einen Ausdruck für die zielbereitstellungsumgebung und einer Argumentliste für den zugeordneten Typ haben.

* *Ein Lambda-Methode,* einer anonymen Methode ist.

* *Eine Eigenschaftengruppe* ist eine Gruppe von Eigenschaften, die auf dem gleichen Namen zu überladen. Eine Eigenschaftengruppe möglicherweise einen zielbereitstellungsumgebung-Ausdruck.

* *Ein Eigenschaftenzugriff.* Jedem Eigenschaftenzugriff ist einen zugeordneten Typ, d. h. den Typ der Eigenschaft. Ein Eigenschaftenzugriff kann es sich um ein zielbereitstellungsumgebung Ausdruck enthalten.

* *Ein spät gebunden Zugriff,* steht für eine Methode oder Eigenschaft zugreifen, die verzögert, bis zur Laufzeit. Ein spät gebundener Zugriff kann es sich um einen Ausdruck für die zielbereitstellungsumgebung und einer Argumentliste für den zugeordneten Typ haben. Der Typ, der ein spät gebundener Zugriff ist immer `Object`.

* *Ein Event-Zugriff.* Jedes Ereigniszugriff verfügt über einen zugeordneten Typ, d. h. den Typ des Ereignisses. Ein Ereignis kann einen Ausdruck für die zielbereitstellungsumgebung zugreifen. Ein Ereigniszugriff möglicherweise angezeigt, als das erste Argument von der `RaiseEvent`, `AddHandler`, und `RemoveHandler` Anweisungen. In einem anderen Kontext bewirkt, dass ein Ausdruck, der klassifiziert als Zugriffs-Ereignis einen Fehler während der Kompilierung.

* *Ein Arrayliteral* steht für die ursprünglichen Werte eines Arrays, dessen Typ noch nicht festgelegt wurden.

* *"Void".* Dies tritt auf, wenn der Ausdruck einen Aufruf einer Unterroutine oder ein Await-Operator-Ausdruck kein Ergebnis zurückgibt. Ein Ausdruck, der klassifiziert als "void" ist nur im Kontext einer aufrufanweisung oder eine Await-Anweisung gültig.

* *Ein Standardwert.* Nur das Literal `Nothing` erzeugt diese Klassifizierung.

Das Endergebnis eines Ausdrucks ist in der Regel einen Wert oder eine Variable, mit den anderen Kategorien von Ausdrücken, die wiederum als Zwischenwerte, die nur in bestimmten Kontexten zulässig sind.

Beachten Sie, dass die Ausdrücke, dessen Typ um einen Typparameter, verwendet werden können, Anweisungen und Ausdrücke, die den Typ eines Ausdrucks mit bestimmten Eigenschaften (z. B. wird ein Verweistyp, Werttyp, der von einigen Typ usw.) erfordern, wenn die Einschränkungen auferlegt. erfüllen Sie die Eigenschaften für den Typparameter.

### <a name="expression-reclassification"></a>Ausdruck Neuklassifizierung

In der Regel, wenn ein Ausdruck in einem Kontext, die sich von der der Ausdruck klassifiziert werden muss verwendet wird, tritt ein Fehler während der Kompilierung – beispielsweise ein Literal einen Wert zuweisen möchten. Allerdings in vielen Fällen kann ein Ausdruck, der Klassifizierung durch den Prozess der ändern *neuklassifizierung*.

Wenn neuklassifizierung erfolgreich ist, klicken Sie dann die neuklassifizierung gilt als erweiternd oder einschränkend. Sofern nicht anders angegeben, werden alle Umbuchungshistorie in dieser Liste erweitern.

Die folgenden Typen von Ausdrücken können neu klassifiziert werden:

* Eine Variable kann als Wert neu klassifiziert werden. Der in der Variable gespeicherte Wert wird abgerufen.

* Eine Methodengruppe kann als Wert neu klassifiziert werden. Der Gruppierungsausdruck für die Methode als ein Aufrufausdruck mit den zugehörigen Zielausdruck Typparameterliste und leere Klammern interpretiert wird (d. h. `f` als interpretiert `f()` und `f(Of Integer)` als interpretiert`f(Of Integer)()`). Diese neuklassifizierung kann dazu führen, dass der Ausdruck, der weitere neu als ungültig eingestuft wird.

* Ein Methodenzeiger kann als Wert neu klassifiziert werden. Diese neuklassifizierung kann nur im Kontext einer Konvertierung auftreten, wenn der Zieltyp bekannt ist. Die Methode Zeiger-Ausdruck wird als Argument ein Delegatausdruck für die Instanziierung des entsprechenden Typs mit der Argumentliste für den zugeordneten Typ interpretiert. Zum Beispiel:
    
    ```vb
    Delegate Sub D(i As Integer)
    
    Module Test
        Sub F(i As Integer)
        End Sub
    
        Sub Main()
            Dim del As D
    
            ' The next two lines are equivalent.
            del = AddressOf F
            del = New D(AddressOf F)
        End Sub
    End Module
    ```

* Eine Lambda-Methode kann als Wert neu klassifiziert werden. Wenn die neuklassifizierung im Kontext einer Konvertierung auftritt, wenn der Zieltyp bekannt ist, kann eine der beiden Umbuchungshistorie auftreten:
    
  1. Wenn der Zieltyp ein Delegattyp ist, wird der Lambda-Methode als Argument für einen Delegaten-konstruktionsausdruck des entsprechenden Typs interpretiert.
    
  2. Wenn der Zieltyp ist `System.Linq.Expressions.Expression(Of T)`, und `T` ein Delegattyp ist, und klicken Sie dann die Lambda-Methode interpretiert wird, als würde er in Delegatkonstruktion Ausdruck für verwendet werden `T` und anschließend in einen Ausdrucksbaum konvertiert.
    
  Eine Async- oder Iterator-Lambda-Methode kann nur als Argument für einen Delegatkonstruktion Ausdruck interpretiert werden, wenn der Delegat keine ByRef-Parameter.
    
  Wenn die Konvertierung aus allen Parametertypen des Delegaten ab, die entsprechenden Typen des Lambda-Parameter eine einschränkende Konvertierung ist, klicken Sie dann die neuklassifizierung gilt als einschränkende; Andernfalls ist es erweitern.
    
  __Beachten Sie.__ Die genaue Übersetzung zwischen Lambdamethoden und Ausdrucksbaumstrukturen kann nicht zwischen verschiedenen Versionen des Compilers behoben werden und ist nicht Gegenstand dieser Spezifikation. Für Microsoft Visual Basic 11.0 möglicherweise alle Lambda-Ausdrücke in Ausdrucksbaumstrukturen jedoch mit folgenden Einschränkungen konvertiert werden: (1) 1.  Nur einzeilige Lambda-Ausdrücke ohne ByRef-Parameter können in Ausdrucksbaumstrukturen konvertiert werden. Von der einzeiligen `Sub` Lambdas nur Aufruf-Anweisungen können in Ausdrucksbaumstrukturen konvertiert werden. (2) anonymen Typs Ausdrücke können nicht in ausdrucksbäume konvertiert werden, wenn eine frühere Feldinitialisierer verwendet wird, um einen nachfolgenden Feldinitialisierer, z. B. zu initialisieren `New With {.a=1, .b=.a}`. (3) objektinitialisiererausdrücken können nicht konvertiert werden in Ausdrucksbaumstrukturen Wenn ein Element des aktuellen Objekts initialisiert wird eines der Feldinitialisierer verwendet wird z. B. `New C1 With {.a=1, .b=.Method1()}`. (4) ein mehrdimensionales Array erstellen Ausdrücke können nur in Ausdrucksbaumstrukturen konvertiert werden, wenn sie dem Elementtyp explizit deklarieren. (5) Ausdrücken späte Bindung können nicht in ausdrucksbäume konvertiert werden. (6) Wenn ByRef an ein Aufrufausdruck, eine Variable oder ein Feld übergeben wird verfügt jedoch nicht genau den gleichen Typ wie der ByRef-Parameter, oder eine Eigenschaft mit ByRef übergeben wird, werden normal VB-Semantik, dass eine Kopie des Arguments ByRef übergeben wird und der endgültige Wert dann kopiert wird.  in der Variablen oder das Feld oder Eigenschaft. In Ausdrucksbaumstrukturen geschieht das Zurückkopieren nicht. (7) alle diese Einschränkungen gelten für geschachtelte Lambda-Ausdrücke ebenfalls.
    
  Wenn der Zieltyp nicht bekannt ist, wird der Lambda-Methode als Argument für eine Instanziierung Delegatausdruck eines anonymen Delegaten-Typs mit der gleichen Signatur der Lambda-Methode interpretiert. Wenn strenge Semantik verwendet wird, und die Art der Parameter ausgelassen werden, tritt ein Fehler während der Kompilierung; andernfalls `Object` wird für jeden fehlenden Parametertyp ersetzt. Zum Beispiel:
    
  ```vb
  Module Test
      Sub Main()
          ' Type of x will be equivalent to Func(Of Object, Object, Object)
          Dim x = Function(a, b) a + b
  
          ' Type of y will be equivalent to Action(Of Object, Object)
          Dim y = Sub(a, b) Console.WriteLine(a + b)
      End Sub
  End Module
  ```

* Eine Eigenschaftengruppe kann als Eigenschaftszugriff neu klassifiziert werden. Der Eigenschaftsausdruck für die Gruppe wird als ein Indexausdruck mit leeren Klammern interpretiert (d. h. `f` als interpretiert `f()`).

* Ein Eigenschaftenzugriff kann als Wert neu klassifiziert werden. Der Eigenschaftsausdruck der Zugriff wird als ein Aufrufausdruck von interpretiert die `Get` -Accessor der Eigenschaft. Wenn die Eigenschaft keine Get-Methode aufweist, tritt auf, klicken Sie dann ein Fehler während der Kompilierung.

* Ein spät gebundener Zugriff kann als spät gebundene Methode oder der Zugriff auf spät gebundene Eigenschaften neu klassifiziert werden. In einer Situation, in denen für eine spät gebundener Zugriff ein sowohl als eine Methodenzugriff einen Eigenschaftenzugriff neu klassifiziert werden kann, wird die neuklassifizierung auf einen Eigenschaftenzugriff bevorzugt.

* Ein spät gebundener Zugriff kann als Wert neu klassifiziert werden.

* Ein Array-literal kann als Wert neu klassifiziert werden. Der Typ des Werts wird wie folgt bestimmt:

  1. Bei der neuklassifizierung im Kontext einer Konvertierung wird, in denen der Zieltyp ist bekannt und der Zieltyp ist ein Arraytyp, ist das Array-literal als ein Wert vom Typ T() neu klassifiziert. Wenn der Zieltyp ist `System.Collections.Generic.IList(Of T)`, `IReadOnlyList(Of T)`, `ICollection(Of T)`, `IReadOnlyCollection(Of T)`, oder `IEnumerable(Of T)`, das Array-literal ist eine Ebene der Schachtelung, und das Arrayliteral ist als ein Wert vom Typ neu klassifiziert `T()`.

  2. Das Array-literal ist, andernfalls auf einen Wert neu klassifiziert, deren Typ, dass ein Array von Rank gleich der Ebene der Schachtelung verwendet wird ist, mit dem Elementtyp bestimmt anhand des bestimmenden Typs der Elemente im Initialisierer; Wenn kein dominanter Typ bestimmt werden kann, `Object` verwendet wird. Zum Beispiel:

     ```vb
     ' x Is GetType(Double(,,))
     Dim x = { { { 1, 2.0 }, { 3, 4 } }, { { 5, 6 }, { 7, 8 } } }.GetType()
        
     ' y Is GetType(Integer())
     Dim y = { 1, 2, 3 }.GetType()
        
     ' z Is GetType(Object())
     Dim z = { 1, "2" }.GetType()
        
     ' Error: Inconsistent nesting
     Dim a = { { 10 }, { 20, 30 } }.GetType()
     ```

  __Beachten Sie.__ Es ist eine geringfügige Änderung im Verhalten zwischen der Version 9.0 und Version 10.0 der Sprache. Vor 10.0 Elementinitialisierer Array keine Auswirkungen auf die lokalen Variablen den Typrückschluss, und jetzt dies der Fall. Also `Dim a() = { 1, 2, 3 }` abgeleitet haben würde `Object()` als Typ des `a` in Version 9.0 der Sprache und `Integer()` in Version 10.0.

  Klicken Sie dann die neuklassifizierung interpretiert das Array-literal als Ausdruck für die Arrayerstellung. Also in den Beispielen:

  ```vb
  Dim x As Double = { 1, 2, 3, 4 }
  Dim y = { "a", "b" }
  ```

  entsprechen:

  ```vb
  Dim x As Double = New Double() { 1, 2, 3, 4 }
  Dim y = New String() { "a", "b" }
  ```

  Die neuklassifizierung wird beurteilt, als Einschränkung betrachtet, wenn einschränkende Konvertierung von einem Elementausdruck, der den Elementtyp des Arrays ist; Andernfalls wird es als erweiternde beurteilt.

* Der Standardwert `Nothing` können als Wert neu klassifiziert werden. In einem Kontext, in dem der Zieltyp bekannt ist, ist das Ergebnis der Standardwert des Zieltyps. In einem Kontext, in denen der Zieltyp ist nicht bekannt, ist, das Ergebnis ist ein null-Wert des Typs `Object`.

Ein Namespace-Ausdruck, Ausdruck, Ereignisausdruck für den Zugriff oder "void" Ausdruck kann nicht neu klassifiziert werden. Mehrere Umbuchungshistorie können gleichzeitig ausgeführt werden. Zum Beispiel:

```vb
Module Test
    Sub F(i As Integer)
    End Sub

    ReadOnly Property P() As Integer
        Get
        End Get
    End Sub

    Sub Main()
        F(P)
    End Property
End Module
```

In diesem Fall die Eigenschaft Gruppierungsausdruck `P` zunächst aus einer Eigenschaftengruppe auf einen Eigenschaftenzugriff neu klassifiziert ist, und klicken Sie dann aus Zugriff auf eine Eigenschaft auf einen anderen Wert neu klassifiziert. Die geringste Anzahl von Umbuchungshistorie werden ausgeführt, um eine gültige Klassifizierung im Kontext zu erreichen.

## <a name="constant-expressions"></a>Konstante Ausdrücke

Ein *Konstantenausdruck* ist ein Ausdruck, dessen Wert zum Zeitpunkt der Kompilierung vollständig ausgewertet werden kann.

```antlr
ConstantExpression
    : Expression
    ;
```

Der Typ ein konstanter Ausdruck sein kann `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Char`, `Single`, `Double`, `Decimal`, `Date`, `Boolean`, `String`, `Object`, oder ein Enumerationstyp. Die folgenden Konstrukte sind in Konstanten Ausdrücken zulässig:

* Literale (einschließlich `Nothing`).

* Verweise auf Konstantentyp-Member "oder" constant "lokal".

* Verweise auf Member von Enumerationstypen.

* Unterausdrücke in Klammern.

* Umwandlungsausdrücke, vorausgesetzt, dass der Zieltyp eine der oben aufgeführten Typen ist. Umwandlungen in und aus `String` stellen eine Ausnahme von dieser Regel und sind nur für null-Werte zulässig, da `String` Konvertierungen zur Laufzeit immer in der aktuellen Kultur der ausführungsumgebung ausgeführt werden. Beachten Sie, dass Konstanten Umwandlungsausdrücke systeminterne Konvertierungen nur verwenden können.

* Die `+`, `-` und `Not` unäre Operatoren, bereitgestellt, die Operanden und Ergebnis ist ein Typ, der oben aufgeführten.

* Die `+`, `-`, `*`, `^`, `Mod`, `/`, `\`, `<<`, `>>`, `&`, `And`, `Or`, `Xor`, `AndAlso`, `OrElse`, `=`, `<`, `>`, `<>`, `<=`, und `=>` binäre Operatoren angegeben von jeder Operand und Ergebnis ist ein Typ, der oben aufgeführten.

* Der bedingte Operator sofern, jeden Operanden und das Ergebnis ist ein Typ, der oben aufgeführten.

* Die folgenden Funktionen der Laufzeit: `Microsoft.VisualBasic.Strings.ChrW`; `Microsoft.VisualBasic.Strings.Chr` ist von der Konstante Wert zwischen 0 und 128; `Microsoft.VisualBasic.Strings.AscW` ist die Konstante Zeichenfolge nicht leer ist. `Microsoft.VisualBasic.Strings.Asc` Wenn die Konstante Zeichenfolge nicht leer ist.

Die folgenden Konstrukte sind *nicht* in Konstanten Ausdrücken zulässig:

* Implizite Bindung über einen `With` Kontext.

Konstante Ausdrücke ein ganzzahliger Typ (`ULong`, `Long`, `UInteger`, `Integer`, `UShort`, `Short`, `SByte`, oder `Byte`) implizit in einen engeren ganzzahligen Typ konvertiert werden kann und Konstante Ausdrücke vom Typ `Double` kann implizit konvertiert werden, um `Single`, sofern der Wert des konstanten Ausdrucks innerhalb des Bereichs des Zieltyps liegt. Diese einschränkende Konvertierungen sind zulässig, unabhängig davon, ob flexible oder eine strikte Semantik verwendet werden.


## <a name="late-bound-expressions"></a>Spät gebundene Ausdrücke

Wenn das Ziel eines Memberzugriffsausdrucks oder Feldindex-Ausdrucks ist vom Typ `Object`, die Verarbeitung des Ausdrucks möglicherweise erst zur Laufzeit verzögert werden. Verschieben die Verarbeitung auf diese Weise wird aufgerufen, *späte Bindung*. Ermöglicht die späte Bindung `Object` Variablen für die in einem *typenlosen* Weise, in dem alle Auflösung von Elementen der eigentliche Laufzeittyp des Werts in der Variablen hängt. Wenn strikte Semantik, durch die kompilierungsumgebung oder durch angegeben werden `Option Strict`, späte Bindung führt dazu, dass einen Fehler während der Kompilierung. Nicht öffentliche Member werden ignoriert, wenn späte Bindung, einschließlich der im Rahmen der Auflösung von funktionsüberladungen. Beachten Sie, dass ein Unterschied zu früh gebundene, Aufrufen von oder zugreifen auf eine `Shared` spät Datenmember gebundenen führt dazu, dass das Aufrufziel zur Laufzeit ausgewertet werden soll. Wenn der Ausdruck einen Aufruf für ein Element definiert ist `System.Object`, späte Bindung nicht erfolgt.

Im Allgemeinen werden spät gebundene Zugriffe aufgelöst zur Laufzeit durch Aufrufen der Bezeichner für die eigentliche Laufzeittyp des Ausdrucks. Wenn das spät gebundene Membersuche zur Laufzeit ein Fehler auftritt ein `System.MissingMemberException` Ausnahme ausgelöst. Da die spät gebundene Memberlookup erfolgt ausschließlich aus der Laufzeittyp des zugeordneten Zielausdrucks-Laufzeittyp eines Objekts ist nie eine Schnittstelle. Aus diesem Grund ist es unmöglich, die Schnittstellenmember in eine spät gebundene Memberzugriffsausdruck zugreifen.

Die Argumente für eine spät gebundene Member zugreifen, werden in der Reihenfolge des Memberzugriffsausdrucks ausgewertet: nicht in der Reihenfolge in der Parameter im spät gebundene Member deklariert werden. Das folgende Beispiel veranschaulicht diesen Unterschied:

```vb
Class C
    Public Sub f(ByVal x As Integer, ByVal y As Integer)
    End Sub
End Class

Module Module1
    Sub Main()
        Console.Write("Early-bound: ")
        Dim c As C = New C
        c.f(y:=t("y"), x:=t("x"))

        Console.Write(vbCrLf & "Late-bound: ")
        Dim o As Object = New C
        o.f(y:=t("y"), x:=t("x"))
    End Sub

    Function t(ByVal s As String) As Integer
        Console.Write(s)
        Return 0
    End Function
End Module
```

Dieser Code zeigt:

```
Early-bound: xy
Late-bound: yx
```

Da die spät gebundene überladungsauflösung für den Runtime-Typ der Argumente erfolgt, ist es möglich, dass ein Ausdruck möglicherweise zu unterschiedlichen Ergebnissen führen basierend auf der gibt an, ob sie zur Kompilierzeit oder zur Laufzeit ausgewertet wird. Das folgende Beispiel veranschaulicht diesen Unterschied:

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class

Module Test
    Sub F(b As Base)
        Console.WriteLine("F(Base)")
    End Sub

    Sub F(d As Derived)
        Console.WriteLine("F(Derived)")
    End Sub

    Sub Main()
        Dim b As Base = New Derived()
        Dim o As Object = b

        F(b)
        F(o)
    End Sub
End Module
```

Dieser Code zeigt:

```
F(Base)
F(Derived)
```

## <a name="simple-expressions"></a>Einfache Ausdrücke

Einfache Ausdrücke sind Literale, Ausdrücke in Klammern, Instanz Ausdrücke oder Ausdrücke mit einfachen Namen an.

```antlr
SimpleExpression
    : LiteralExpression
    | ParenthesizedExpression
    | InstanceExpression
    | SimpleNameExpression
    | AddressOfExpression
    ;
```

### <a name="literal-expressions"></a>Literale Ausdrücke

Literale Ausdrücke ausgewertet werden auf den Wert, der durch das Literal dargestellt wird. Ein literalen Ausdruck wird als ein Wert, mit Ausnahme des Literals klassifiziert `Nothing`, die als Standardwert klassifiziert wird.

```antlr
LiteralExpression
    : Literal
    ;
```

### <a name="parenthesized-expressions"></a>Ausdrücke in Klammern

Ein Ausdruck in Klammern besteht aus einem Ausdruck in Klammern eingeschlossen. Ein Ausdruck in Klammern wird als Wert klassifiziert, und der eingeschlossene Ausdruck muss als Wert klassifiziert werden. Ein Ausdruck in Klammern, die auf den Wert des Ausdrucks innerhalb der Klammern ausgewertet wird.

```antlr
ParenthesizedExpression
    : OpenParenthesis Expression CloseParenthesis
    ;
```

### <a name="instance-expressions"></a>Instanz-Ausdrücke

Ein *Instanz Ausdruck* ist das Schlüsselwort `Me`. Es kann nur innerhalb des Texts für einen Accessor nicht freigegeben, Methode, Konstruktor oder einer Eigenschaft verwendet werden. Es wird als Wert klassifiziert. Das Schlüsselwort `Me` stellt die Instanz des Typs mit der Methode oder Eigenschaft Accessor, der ausgeführt wird. Wenn ein Konstruktor explizit einen anderen Konstruktor aufruft (Abschnitt [Konstruktoren](type-members.md#constructors)), `Me` nicht erst nach dem den Konstruktoraufruf verwendet werden, da die Instanz noch nicht erstellt wurde.

```antlr
InstanceExpression
    : 'Me'
    ;
```

### <a name="simple-name-expressions"></a>Ausdrücke für einfache Namen

Ein *einfachen Namensausdruck* besteht aus einem einzelnen Bezeichner gefolgt von einer optionalen Liste der Typargumente.

```antlr
SimpleNameExpression
    : Identifier ( OpenParenthesis 'Of' TypeArgumentList CloseParenthesis )?
    ;
```

Der Name aufgelöst und durch die folgenden "einfache Namen Auflösungsregeln" klassifiziert:

1.  Beginnend mit dem unmittelbar einschließenden block und mit jeder Block mit einschließenden äußeren (sofern vorhanden), fortfahren, wenn der Bezeichner der Name eines lokalen Variablen, statischen Variablen, Konstanten lokalen Variable übereinstimmt, Methode geben Sie Parameter oder -Parameter, und klicken Sie dann auf der Bezeichner bezieht die entsprechende Entität.

    Wenn eine lokale Variable, die statische Variable oder Konstante lokale mit der ID übereinstimmt, und eine Liste der Typargumente bereitgestellt wurde, tritt ein Fehler während der Kompilierung. Wenn der Typparameter einer Methode mit der ID übereinstimmt, und eine Liste der Typargumente bereitgestellt wurde, wird keine Übereinstimmung gefunden, und Auflösung wird fortgesetzt. Wenn eine lokale Variable mit der ID übereinstimmt, wird der lokalen Variablen zugeordnet ist die implizite-Funktion oder `Get` Accessor zurückzugeben, lokale Variable, und der Ausdruck ist Teil eines Aufrufausdrucks, aufrufanweisung, oder ein `AddressOf` Ausdruck dann keine Übereinstimmung kommt und Auflösung wird fortgesetzt.

    Der Ausdruck wird als Variable klassifiziert, wenn es sich um eine lokale Variable, die statische Variable oder Parameter handelt. Der Ausdruck wird als Typ klassifiziert, wenn es sich um einen Methodenparameter des Typs ist. Der Ausdruck wird als Wert klassifiziert, wenn es sich um eine Konstante lokale handelt.

2.  Für jeden geschachtelten Typ mit dem Ausdruck wird beginnend mit der innersten und möchte die äußerste, wenn eine Suche des Bezeichners in den Typ eine Übereinstimmung mit einem Member zugegriffen werden kann:

    21. Wenn der entsprechende Typmember einen Typparameter ist, klicken Sie dann das Ergebnis wird als Typ klassifiziert und ist der entsprechende Typparameter. Wenn eine Liste der Typargumente angegeben wurde, wird keine Übereinstimmung gefunden, und Auflösung wird fortgesetzt.
    22. Andernfalls, wenn der Typ der unmittelbar einschließenden Typ ist und die Suche einen nicht freigegebenen Typmember gibt, klicken Sie dann das Ergebnis ist identisch mit der ein Memberzugriff des Formulars `Me.E(Of A)`, wobei `E` ist der Bezeichner und `A` ist die Liste der Typargumente , falls vorhanden.
    23. Das Ergebnis ist, andernfalls genau identisch mit der ein Memberzugriff des Formulars `T.E(Of A)`, wobei `T` ist der Typ des entsprechenden Elements mit `E` ist der Bezeichner, und `A` ist die Liste der Typargumente, sofern vorhanden. In diesem Fall ist es ein Fehler für den Bezeichner zum Verweisen auf einen nicht freigegebenen Member.

3.  Führen Sie für jeden geschachtelten Namespace, beginnend mit der innersten und möchte den äußersten-Namespace folgende Schritte aus:

    31. Wenn der Namespace einen zugreifbarer Typ mit dem angegebenen Namen enthält und die gleiche Anzahl von Typparametern aufweist, wie in der Liste der Typargumente, sofern vorhanden, angegeben wurde, und klicken Sie dann den Bezeichner bezieht sich auf diesen Typ und wird als Typ klassifiziert.
    32. Andernfalls, wenn keine Liste der Typargumente angegeben wurde, und der Namespace eine Namespace-Members mit dem angegebenen Namen enthält, klicken Sie dann der Bezeichner bezieht sich auf diesen Namespace und wird als Namespace klassifiziert.
    33. Andernfalls, wenn der Namespace eine oder mehrere zugänglich Standardmodulen enthält und eine Suche nach Membern-Name des Bezeichners genau einem Standardmodul eine Übereinstimmung zugegriffen werden kann erzeugt, klicken Sie dann das Ergebnis entspricht genau ein Memberzugriff des Formulars `M.E(Of A)`, in denen `M` ist das standard-Modul, enthält das entsprechende Element `E` ist der Bezeichner, und `A` ist die Liste der Typargumente, sofern vorhanden. Wenn zugänglich Typmember in mehr als ein Standardmodul mit der ID übereinstimmt, tritt ein Fehler während der Kompilierung.

4.  Wenn die Quelldatei eine oder mehrere Importaliase hat und der Bezeichner mit dem übereinstimmt, und klicken Sie dann der Bezeichner auf diesen Namespace oder Typ verweist. Wenn eine Liste der Typargumente angegeben wird, tritt ein Fehler während der Kompilierung.

5. Wenn die Quelldatei, die mit dem Namensverweis ein oder mehrere Importe aufweist:

    51. Wenn der Bezeichner in genau einem entspricht importieren den Namen eines Typs zugegriffen werden kann, mit der gleichen Anzahl von Typparametern, wie in der Liste der Typargumente, sofern vorhanden, oder einen Typmember angegeben wurde, und klicken Sie dann der Bezeichner bezieht sich auf diesen Typ oder Typmember. Wenn der Bezeichner in mehr als ein Import mit dem Namen ein zugreifbarer Typ mit der gleichen Anzahl von Typparametern übereinstimmt, wie in der Liste der Typargumente, angegeben wurde, sofern vorhanden, oder ein Typmember zugegriffen werden kann, ein Fehler während der Kompilierung auftritt.
    52. Andernfalls verweist, wenn keine Liste der Typargumente angegeben wurde, und dem Bezeichner, der in genau einem Import den Namen eines Namespaces mit verfügbaren Typen übereinstimmt, klicken Sie dann der Bezeichner zu diesem Namespace. Wenn keine Liste der Typargumente angegeben wurde, und dem Bezeichner, der in mehr als einem Import den Namen eines Namespaces mit verfügbaren Typen übereinstimmt, tritt ein Fehler während der Kompilierung.
    53. Andernfalls, wenn der Import eine oder mehrere zugänglich Standardmodule enthält und eine Suche nach Membern-Name des Bezeichners genau einem Standardmodul eine Übereinstimmung zugegriffen werden kann erzeugt, klicken Sie dann das Ergebnis entspricht genau ein Memberzugriff des Formulars `M.E(Of A)`, wobei `M` ist das standard-Modul, enthält das entsprechende Element `E` ist der Bezeichner, und `A` ist die Liste der Typargumente, sofern vorhanden. Wenn zugänglich Typmember in mehr als ein Standardmodul mit der ID übereinstimmt, tritt ein Fehler während der Kompilierung.

6.  Wenn der kompilierungsumgebung eine oder mehrere Importaliase definiert und der Bezeichner mit dem übereinstimmt, und klicken Sie dann der Bezeichner auf diesen Namespace oder Typ verweist. Wenn eine Liste der Typargumente angegeben wird, tritt ein Fehler während der Kompilierung.

7. Wenn der kompilierungsumgebung ein oder mehrere Importe definiert:

    71. Wenn der Bezeichner in genau einem entspricht importieren den Namen eines Typs zugegriffen werden kann, mit der gleichen Anzahl von Typparametern, wie in der Liste der Typargumente, sofern vorhanden, oder einen Typmember angegeben wurde, und klicken Sie dann der Bezeichner bezieht sich auf diesen Typ oder Typmember. Wenn der Bezeichner in mehr als ein Import mit dem Namen übereinstimmt ein zugreifbarer Typ mit der gleichen Anzahl von Typparametern, wie in der Liste der Typargumente, die ggf. bereitgestellt wurde oder ein Typmember, ein Fehler während der Kompilierung auftritt.
    72. Andernfalls verweist, wenn keine Liste der Typargumente angegeben wurde, und dem Bezeichner, der in genau einem Import den Namen eines Namespaces mit verfügbaren Typen übereinstimmt, klicken Sie dann der Bezeichner zu diesem Namespace. Wenn keine Liste der Typargumente angegeben wurde, und dem Bezeichner, der in mehr als einem Import den Namen eines Namespaces mit verfügbaren Typen übereinstimmt, tritt ein Fehler während der Kompilierung.
    73. Andernfalls, wenn der Import eine oder mehrere zugänglich Standardmodule enthält und eine Suche nach Membern-Name des Bezeichners genau einem Standardmodul eine Übereinstimmung zugegriffen werden kann erzeugt, klicken Sie dann das Ergebnis entspricht genau ein Memberzugriff des Formulars `M.E(Of A)`, wobei `M` ist das standard-Modul, enthält das entsprechende Element `E` ist der Bezeichner, und `A` ist die Liste der Typargumente, sofern vorhanden. Wenn zugänglich Typmember in mehr als ein Standardmodul mit der ID übereinstimmt, tritt ein Fehler während der Kompilierung.

8. Andernfalls ist die vom Bezeichner angegebene Name nicht definiert.

Ein einfacher Name-Ausdruck, der nicht definiert ist, ist ein Fehler während der Kompilierung.

Normalerweise kann ein Namen in einem bestimmten Namespace nur einmal vorkommen. Da Namespaces mehreren .NET Assemblys deklariert werden können, ist es jedoch möglich, dass eine Situation, in denen zwei Assemblys für einen Typ mit den gleichen vollqualifizierten Namen definieren. In diesem Fall wird ein Typ, der deklariert, die in den aktuellen Satz von Quelldateien über einem Typ deklariert, die in einer externen .NET-Assembly bevorzugt. Andernfalls der Name ist mehrdeutig, und es gibt keine Möglichkeit, um den Namen zu unterscheiden.


### <a name="addressof-expressions"></a>AddressOf-Ausdrücke

Ein `AddressOf` Ausdruck wird verwendet, um einen Methodenzeiger zu erzeugen. Der Ausdruck besteht aus den `AddressOf` -Schlüsselwort und ein Ausdruck, der als einer Methodengruppe oder einer spät gebundener Zugriff klassifiziert werden muss. Die Methodengruppe kann nicht an die Konstruktoren verweisen.

Das Ergebnis wird als Methodenzeiger, mit dem gleichen zugeordneten Zielausdruck und die Liste der Typargumente (sofern vorhanden) als die Methodengruppe klassifiziert.

```antlr
AddressOfExpression
    : 'AddressOf' Expression
    ;
```

## <a name="type-expressions"></a>Typenausdruck

Ein *geben Ausdruck* ist eine `GetType` Ausdruck eine `TypeOf...Is` Ausdruck eine `Is` Ausdruck oder ein `GetXmlNamespace` Ausdruck.

```antlr
TypeExpression
    : GetTypeExpression
    | TypeOfIsExpression
    | IsExpression
    | GetXmlNamespaceExpression
    ;
```

### <a name="gettype-expressions"></a>GetType-Ausdrücke

Ein `GetType` Ausdruck besteht aus dem Schlüsselwort `GetType` und den Namen eines Typs.

```antlr
GetTypeExpression
    : 'GetType' OpenParenthesis GetTypeTypeName CloseParenthesis
    ;

GetTypeTypeName
    : TypeName
    | QualifiedOpenTypeName
    ;

QualifiedOpenTypeName
    : Identifier TypeArityList? (Period IdentifierOrKeyword TypeArityList?)*
    | 'Global' Period IdentifierOrKeyword TypeArityList?
      (Period IdentifierOrKeyword TypeArityList?)*
    ;

TypeArityList
    : OpenParenthesis 'Of' CommaList? CloseParenthesis
    ;

CommaList
    : Comma Comma*
    ;
```

Ein `GetType` Ausdruck wird als Wert klassifiziert, und sein Wert ist die Reflektion (`System.Type`)-Klasse, die stellt seine *GetTypeTypeName*. Wenn die *GetTypeTypeName* Typparameter ist ein, wird der Ausdruck zurückgeben, die `System.Type` Objekt, das das Typargument für den Parameter zur Laufzeit entspricht.

Die *GetTypeTypeName* ist etwas Besonderes, gibt es zwei Möglichkeiten:

* Es ist zulässig, werden `System.Void`, der einzige Ort in der Sprache, in dem dieser Typname verwiesen werden kann kann.

* Es kann es sich um einen konstruierten generischen Typ mit den Typargumenten ausgelassen sein. Dadurch wird die `GetType` zurückzugebende Ausdruck die `System.Type` Objekt, das den generischen Typ selbst entspricht.

Das folgende Beispiel zeigt die `GetType` Ausdruck:

```vb
Module Test
    Sub Main()
        Dim t As Type() = { GetType(Integer), GetType(System.Int32), _
            GetType(String), GetType(Double()) }
        Dim i As Integer

        For i = 0 To t.Length - 1
            Console.WriteLine(t(i).Name)
        Next i
    End Sub
End Module
```

Die entstandene Ausgabe ist:

```
Int32
Int32
String
Double[]
```


### <a name="typeofis-expressions"></a>TypeOf... Ausdrücke

Ein `TypeOf...Is` Ausdruck wird verwendet, um zu überprüfen, ob der Laufzeittyp eines Werts mit einem angegebenen Typ kompatibel ist. Der erste Operand muss als Wert klassifiziert werden, nicht möglich, eine klassifizierter Lambda-Methode und muss ein Verweistyp oder ein Parametertyp beschränkten Typ sein. Der zweite Operand muss ein Typname sein. Das Ergebnis des Ausdrucks wird als Wert klassifiziert, und ist eine `Boolean` Wert. Der Ausdruck wird zu `True` verfügt der Laufzeittyp der Operanden eine Identität, Standard, Verweis, Array, Werttyp oder typparameterumwandlung in den Typ `False` andernfalls. Ein Fehler während der Kompilierung tritt auf, wenn keine Konvertierung zwischen den Typ des Ausdrucks und des angegebenen Typs vorhanden ist.

```antlr
TypeOfIsExpression
    : 'TypeOf' Expression 'Is' LineTerminator? TypeName
    ;
```

### <a name="is-expressions"></a>Ausdrücke

Ein `Is` oder `IsNot` Ausdruck wird verwendet, um einen Verweisgleichheitsvergleich führen.

```antlr
IsExpression
    : Expression 'Is' LineTerminator? Expression
    | Expression 'IsNot' LineTerminator? Expression
    ;
```

Jeder Ausdruck muss als Wert klassifiziert werden, und der Typ jedes Ausdrucks muss ein Verweistyp, ein Parametertyp beschränkten Typ oder NULL-Werte zulassen. Der Typ eines Ausdrucks einer beschränkten Typ-Parametertyp oder der Werttyp ist, aber der andere Ausdruck muss, das Literal `Nothing`.

Das Ergebnis wird als Wert klassifiziert, und wird als eingegeben `Boolean`. Ein `Is` Operation ergibt `True` Wenn beide Werte beziehen sich auf derselben Instanz oder beide Werte sind `Nothing`, oder `False` andernfalls. Ein `IsNot` Operation ergibt `False` Wenn beide Werte beziehen sich auf derselben Instanz oder beide Werte sind `Nothing`, oder `True` andernfalls.


### <a name="getxmlnamespace-expressions"></a>GetXmlNamespace-Ausdrücke

Ein `GetXmlNamespace` Ausdruck besteht aus dem Schlüsselwort `GetXmlNamespace` und den Namen eines XML-Namespace deklariert, indem der quellumgebung für Datei oder die Kompilierung.

```antlr
GetXmlNamespaceExpression
    : 'GetXmlNamespace' OpenParenthesis XMLNamespaceName? CloseParenthesis
    ;
```

Ein `GetXmlNamespace` Ausdruck wird als Wert klassifiziert, und sein Wert ist eine Instanz der `System.Xml.Linq.XNamespace` darstellt, die die *XMLNamespaceName*. Wenn Sie diesen Typ nicht verfügbar ist, und klicken Sie dann ein Fehler während der Kompilierung erfolgt.

Zum Beispiel:

```vb
Imports <xmlns:db="http://example.org/database">

Module Test
    Sub Main()
        Dim db = GetXmlNamespace(db)

        ' The following are equivalent
        Dim customer1 = _
            New System.Xml.Linq.XElement(db + "customer", "Bob")
        Dim customer2 = <db:customer>Bob</>
    End Sub
End Module
```

Alles, was zwischen den Klammern wird als Teil der den Namespacenamen, betrachtet, also z. B. Leerzeichen XML-Regeln an. Zum Beispiel:

```vb
Imports <xmlns:db-ns="http://example.org/database">

Module Test
    Sub Main()

        ' Error, XML name expected
        Dim db1 = GetXmlNamespace( db-ns )

        ' Error, ')' expected
        Dim db2 = GetXmlNamespace(db _
            )

        ' OK.
        Dim db3 = GetXmlNamespace(db-ns)
    End Sub
End Module
```

Der XML-Namespace-Ausdruck kann in diesem Fall gibt der Ausdruck für das Objekt, das von der XML-Standardnamespace, auch weggelassen werden.


## <a name="member-access-expressions"></a>Memberzugriffsausdrücke

Ein Memberzugriffsausdruck wird verwendet, Zugriff auf einen Member einer Entität.

```antlr
MemberAccessExpression
    : MemberAccessBase? Period IdentifierOrKeyword
      ( OpenParenthesis 'Of' TypeArgumentList CloseParenthesis )?
    ;

MemberAccessBase
    : Expression
    | NonArrayTypeName
    | 'Global'
    | 'MyClass'
    | 'MyBase'
    ;
```

Ein Memberzugriff des Formulars `E.I(Of A)`, wobei `E` ist ein Ausdruck, einen Typnamen mit nicht-Array, das Schlüsselwort `Global`, oder nicht angegeben und `I` ist ein Bezeichner mit einer optionalen Liste der Typargumente `A`, ausgewertet und klassifiziert wie folgt:

1. Wenn `E` weggelassen wird, klicken Sie dann den Ausdruck aus, die direkt mit `With` Anweisung wird durch ersetzt `E` und der Memberzugriff ausgeführt wird. Wenn es keine enthält ist `With` -Anweisung ein Fehler während der Kompilierung auftritt.

2. Wenn `E` wird als Namespace klassifiziert oder `E` ist das Schlüsselwort `Global`, und klicken Sie dann die Membersuche im Kontext des angegebenen Namespace ausgeführt wird. Wenn `I` ist der Name des einen verfügbaren Member dieses Namespace mit der gleichen Anzahl von Typparametern, wie in der Liste der Typargumente, sofern vorhanden, angegeben wurde, lautet das Ergebnis dieses Members. Das Ergebnis wird als ein Namespace oder Typ abhängig von der Members klassifiziert. Andernfalls tritt ein Kompilierungsfehler auf.

3. Wenn `E` ist ein Typ oder ein Ausdruck, der als Typ klassifiziert, und klicken Sie dann die Membersuche im Kontext des angegebenen Typs ausgeführt wird. Wenn `I` ist der Name eines Mitglieds zugegriffen werden kann, der `E`, klicken Sie dann `E.I` ausgewertet und wie folgt klassifiziert:

    31. Wenn `I` ist das Schlüsselwort `New` und `E` ist keine Enumeration dar, und klicken Sie dann ein Fehler während der Kompilierung auftritt.
    32. Wenn `I` identifiziert einen Typ mit der gleichen Anzahl von Typparametern, wie in der Liste der Typargumente, sofern vorhanden, angegeben wurde, lautet das Ergebnis dieses Typs.
    33. Wenn `I` eine oder mehrere Methoden, identifiziert, und klicken Sie dann das Ergebnis einer Methodengruppe mit der Argumentliste für den zugeordneten Typ und kein Ausdruck für die zielbereitstellungsumgebung ist.
    34. Wenn `I` identifiziert eine oder mehrere Eigenschaften und keine Argumentliste bereitgestellt wurde, und klicken Sie dann das Ergebnis ist eine Eigenschaftengruppe ohne zielbereitstellungsumgebung-Ausdruck.
    35. Wenn `I` identifiziert eine freigegebene Variable und keine Argumentliste bereitgestellt wurde, und klicken Sie dann das Ergebnis ist eine Variable oder einen Wert. Wenn die Variable schreibgeschützt ist, ist der Verweis außerhalb des gemeinsam genutzte Konstruktors des Typs erfolgt, in dem die Variable wird deklariert, und das Ergebnis der Wert, der die freigegebene Variable ist `I` in `E`. Das Ergebnis ist, andernfalls die freigegebene Variable `I` in `E`.
    36. Wenn `I` identifiziert eines freigegebenen Ereignisses und keine Argumentliste bereitgestellt wurde, das Ergebnis ist ein Ereigniszugriff ohne zielbereitstellungsumgebung-Ausdruck.
    37. Wenn `I` identifiziert eine Konstante und keine Argumentliste bereitgestellt wurde, und klicken Sie dann das Ergebnis ist der Wert dieser Konstanten.
    38. Wenn `I` identifiziert einen Enumerationsmember und keine Argumentliste bereitgestellt wurde, und klicken Sie dann das Ergebnis ist der Wert dieses Elements der Enumeration.
    39. Andernfalls `E.I` ist ein ungültiger Memberverweis, und ein Fehler während der Kompilierung auftritt.

4. Wenn `E` wird als eine Variable oder ein-Wert, dessen Typ von klassifiziert `T`, und klicken Sie dann die Membersuche im Kontext des erfolgt `T`. Wenn `I` ist der Name eines Mitglieds zugegriffen werden kann, der `T`, klicken Sie dann `E.I` ausgewertet und wie folgt klassifiziert:

    41. Wenn `I` ist das Schlüsselwort `New`, `E` ist `Me`, `MyBase`, oder `MyClass`, es wurden keine Typargumente bereitgestellt, und das Ergebnis ist eine Methodengruppe, die die Instanzkonstruktoren der den Typ des darstellt`E`mit einer zugeordneten Zielausdruck `E` und keine Liste der Typargumente. Andernfalls tritt ein Kompilierungsfehler auf.
    42. Wenn `I` identifiziert eine oder mehrere Methoden, einschließlich Erweiterungsmethoden, wenn `T` nicht `Object`, lautet das Ergebnis einer Methodengruppe mit der Argumentliste für den zugeordneten Typ und eine zugeordnete Zielausdruck `E`.
    43. Wenn `I` eine oder mehrere Eigenschaften und keine Argumente bereitgestellt wurden, Typ identifiziert werden, lautet das Ergebnis eine Eigenschaftengruppe mit einer zugeordneten Zielausdruck `E`.
    44. Wenn `I` identifiziert, eine freigegebene Variable oder eine Instanzvariable und keinen Typ Argumente bereitgestellt wurden, und klicken Sie dann das Ergebnis entweder eine Variable oder ein Wert ist. Wenn die Variable schreibgeschützt ist, ist der Verweis findet einen Konstruktor der Klasse in der Variablen für die Art der Variablen ("shared" oder "Instanz") deklariert wird, und das Ergebnis der Wert der Variablen ist `I` in das Objekt, das auf die verwiesen wird durch `E`. Wenn `T` ein Verweistyp ist, lautet das Ergebnis der Variablen `I` in das Objekt, das auf `E`. Andernfalls gilt: Wenn `T` ist ein Werttyp und der Ausdruck `E` als Variable klassifiziert, wird das Ergebnis einer Variablen; andernfalls ist das Ergebnis ein Wert ist.
    45. Wenn `I` bezeichnet ein Ereignis und keine Argumente wurden bereitgestellt, die das Ergebnis ist ein Ereigniszugriff mit einer zugeordneten Zielausdruck `E`.
    46. Wenn `I` identifiziert eine Konstante und keine Argumente bereitgestellt wurden, Typ, lautet das Ergebnis den Wert dieser Konstanten.
    47. Wenn `I` identifiziert einen Enumerationsmember und keine Argumente bereitgestellt wurden, Typ, lautet das Ergebnis den Wert dieses Elements der Enumeration.
    48. Wenn `T` ist `Object`, lautet das Ergebnis einer spät gebundenen Elementsuche als ein spät gebundener Zugriff mit der Argumentliste für den zugeordneten Typ und eine zugeordnete Zielausdruck klassifiziert `E`.

5. Andernfalls `E.I` ist ein ungültiger Memberverweis, und ein Fehler während der Kompilierung auftritt.

Ein Memberzugriff des Formulars `MyClass.I(Of A)` entspricht `Me.I(Of A)`, aber alle Elemente, die darauf zugegriffen werden so behandelt, als ob die Elemente nicht mehr überschrieben werden. Das Element, das zugegriffen wird daher nicht von der Laufzeittyp des Werts beeinflusst werden auf dem das Element zugegriffen wird.

Ein Memberzugriff des Formulars `MyBase.I(Of A)` entspricht `CType(Me, T).I(Of A)` , in denen `T` ist der direkte Basistyp des Typs mit dem Ausdruck für den Elementzugriff. Alle zugehörigen Methodenaufrufe werden behandelt, als ob die aufgerufene Methode nicht überschreibbaren ist. Diese Form der Memberzugriff ist die Abkürzung eine *base Access*.

Im folgende Beispiel wird veranschaulicht, wie `Me`, `MyBase` und `MyClass` beziehen:

```vb
Class Base
    Public Overridable Sub F()
        Console.WriteLine("Base.F")
    End Sub
End Class

Class Derived
    Inherits Base

    Public Overrides Sub F()
        Console.WriteLine("Derived.F")
    End Sub

    Public Sub G()
        MyClass.F()
    End Sub
End Class

Class MoreDerived
    Inherits Derived

    Public Overrides Sub F()
        Console.WriteLine("MoreDerived.F")
    End Sub

    Public Sub H()
        MyBase.F()
    End Sub
End Class

Module Test
    Sub Main()
        Dim x As MoreDerived = new MoreDerived()

        x.F()
        x.G()
        x.H()
    End Sub

End Module
```

Dieser Code gibt:

```
MoreDerived.F
Derived.F
Derived.F
```

Wenn ein Memberzugriffsausdruck beginnt mit dem Schlüsselwort `Global`, das Schlüsselwort darstellt, der äußersten unbenannten Namespace, der die eignet sich für Situationen, in denen eine Deklaration führt Shadowing für eine einschließende Namespace. Die `Global` Schlüsselwort, "Schutz" auf den äußeren Namespace in dieser Situation kann. Zum Beispiel:

```vb
Class System
End Class

Module Test
    Sub Main()
        ' Error: Class System does not contain Console
        System.Console.WriteLine("Hello, world!") 


        ' Legal, binds to System in outermost namespace
        Global.System.Console.WriteLine("Hello, world!") 
    End Sub
End Module
```

Im obigen Beispiel ist der erste Methodenaufruf ungültig da der Bezeichner `System` auf die Klasse bindet `System`, nicht den Namespace `System`. Die einzige Möglichkeit zum Zugriff auf die `System` Namespace ist die Verwendung `Global` , dem äußeren Namespace mit Escapezeichen versehen.

Wenn das Element, auf die zugegriffen wird, freigegeben ist, wird jeder Ausdruck auf der linken Seite des Zeitraums ist überflüssig und wird nicht ausgewertet werden, es sei denn, der Mitglied Zugriff erfolgt spät gebunden. Beachten Sie z. B. folgenden Code:

```vb
Class C
    Public Shared F As Integer = 10
End Class

Module Test
    Public Function ReturnC() As C
        Console.WriteLine("Returning a new instance of C.")
        Return New C()
    End Function

    Public Sub Main()
        Console.WriteLine("The value of F is: " & ReturnC().F)
    End Sub
End Module
```

Gibt `The value of F is: 10` da die Funktion `ReturnC` muss nicht aufgerufen werden, um eine Instanz von bieten `C` Zugriff auf den freigegebenen Member `F`.


### <a name="identical-type-and-member-names"></a>Gleichen Typs und Elementnamen

Es ist nicht ungewöhnlich, mit Name-Elemente, die mit dem gleichen Namen wie der Typ. In diesem Fall kann allerdings unpraktisch Namen auftreten:

```vb
Enum Color
    Red
    Green
    Yellow
End Enum

Class Test
    ReadOnly Property Color() As Color
        Get
            Return Color.Red
        End Get
    End Property

    Shared Function DefaultColor() As Color
        Return Color.Green    ' Binds to the instance property!
    End Function
End Class
```

Im vorherigen Beispiel ist der einfache Name `Color` in `DefaultColor` bindet an die Instanzeigenschaft den Typ. Da ein Instanzmember in einen freigegebenen Member verwiesen werden kann, würde dies normalerweise ein Fehler sein.

Allerdings kann eine bestimmte Regel in diesem Fall den Zugriff auf den Typ aus. Wenn der Basis eines Memberzugriffsausdrucks Ausdruck ein einfacher Name ist und an eine Konstante, Feld, Eigenschaft, lokale Variable oder Parameter bindet, dessen Typ den gleichen Namen hat, kann die Basisausdruck entweder auf das Element oder den Typ verweisen. Dies kann sich nie in Mehrdeutigkeit führen, da die Elemente, die von einer zugegriffen werden können, die identisch sind.

### <a name="default-instances"></a>Standard-Instanzen

In einigen Fällen Klassen, die von der eine allgemeine Basisklasse in der Regel oder immer nur eine einzige Instanz aufweisen. Beispielsweise verfügen die meisten Windows immer nur in einer Benutzeroberfläche gezeigt eine Instanz, die auf dem Bildschirm angezeigt wird, zu einem beliebigen Zeitpunkt. Zur Vereinfachung der Arbeit mit diesen Typen von Klassen, Visual Basic können automatisch generieren *Standardinstanzen* der Klassen, die eine einzelne Instanz einfach auf die verwiesen wird für jede Klasse bereitstellen.

Standard-Instanzen werden immer für erstellt eine *Familie* von Typen und nicht für einen bestimmten Typ. Statt eine Standardinstanz einer Klasse Form1, die von Form abgeleitet wird, werden also Standardinstanzen für alle Klassen, die von Form abgeleitet erstellt. Dies bedeutet, dass jeder einzelnen Klasse, die von der Basisklasse abgeleitet ist keine besonders gekennzeichnet werden, um eine Standardinstanz.

Durch eine vom Compiler generierte Eigenschaft, die die Standardinstanz dieser Klasse zurückgibt, wird die Standardinstanz einer Klasse dargestellt. Wird aufgerufen, die Eigenschaft als Member einer Klasse generiert die *Klasse gruppieren* , verwaltet zuordnen und Zerstören von Standardinstanzen für alle Klassen aus der bestimmten Basisklasse abgeleitet. Beispielsweise alle Eigenschaften der Instanz der Klassen stammen aus `Form` möglicherweise in den erfassten der `MyForms` Klasse. Wenn eine Instanz der Gruppenklasse, durch den Ausdruck zurückgegeben wird `My.Forms`, und klicken Sie dann der folgende Code die Standardinstanzen von abgeleiteten Klassen greift auf `Form1` und `Form2`:

```vb
Class Form1
    Inherits Form
    Public x As Integer
End Class

Class Form2
    Inherits Form
    Public y As Integer
End Class

Module Main
    Sub Main()
        My.Forms.Form1.x = 10
        Console.WriteLine(My.Forms.Form2.y)
    End Sub
End Module
```

Standardinstanzen werden bis der erste Verweis auf diese nicht erstellt werden. Abrufen der Eigenschaft für die Standardinstanz bewirkt, dass die Standardinstanz erstellt werden, wenn er noch nicht erstellt wurde, oder auf festgelegt wurde `Nothing`. Um zuzulassen, testen das Vorhandensein einer Standardinstanz, wenn eine Standardinstanz das Ziel ist eine `Is` oder `IsNot` Operator an mit, die Standardinstanz wird nicht erstellt werden. Daher ist es möglich, zu überprüfen, ob eine Standardinstanz ist `Nothing` oder einige andere Verweis, ohne dass die Standardinstanz erstellt werden.

Standard-Instanzen zu vereinfachen, um mit der Standardinstanz von außerhalb der Klasse zu verweisen, die die Standardinstanz dienen. Mit einer Standardinstanz von innerhalb einer Klasse, die es definiert, kann für Verwirrung Sorgen darüber, welche Instanz, z. B. die Standardinstanz oder die aktuelle Instanz gemeint ist. Der folgende Code ändert beispielsweise nur den Wert `x` in der Standardinstanz, auch wenn es aufgerufen wird von einer anderen Instanz. Daher würde der Code den Wert gedruckt `5` anstelle von `10`:

```vb
Class Form1
    Inherits Form

    Public x As Integer = 5

    Public Sub ChangeX()
        Form1.x = 10
    End Sub
End Class

Module Main
    Sub Main()
        Dim f As Form1 = New Form1()
        f.ChangeX()
        Console.WriteLine(f.x)
    End Sub
End Module
```

Um diese Art von Verwirrung zu vermeiden, ist es nicht zum Verweisen auf die Standardinstanz des Typs eine Standardinstanz von innerhalb einer Instanzenmethode gültig.

#### <a name="default-instances-and-type-names"></a>Standard-Instanzen und Typnamen

Eine Standardinstanz kann auch direkt über den Namen des Typs zugegriffen werden. In diesem Fall in einem beliebigen Ausdruckskontext verwendet, in dem der Name ist den Ausdruck nicht zulässig `E`, wobei `E` steht für den vollqualifizierten Namen der Klasse mit einer Standardinstanz, die geändert wird, um `E'`, wobei `E'` darstellt Ein Ausdruck, der die Standardeigenschaft für die Instanz abruft. Z. B. wenn der Standardwert für die Instanzen von abgeleiteten Klassen `Form` können Sie die Standardinstanz über den Namen, und klicken Sie dann der folgende Code den Code im vorherigen Beispiel entspricht:

```vb
Module Main
    Sub Main()
        Form1.x = 10
        Console.WriteLine(Form2.y)
    End Sub
End Module
```

Dies bedeutet auch, dass eine Standardinstanz, die über den Namen des Typs zugegriffen werden kann auch über den Namen des Typs zugewiesen werden. Im folgenden Code wird z. B. die Standardinstanz von `Form1` zu `Nothing`:

```vb
Module Main
    Sub Main()
        Form1 = Nothing
    End Sub
End Module
```

Beachten Sie, dass die Bedeutung der `E.I` wurden `E` stellt eine Klasse und `I` darstellt, die ein freigegebener Member nicht geändert. Ein solcher Ausdruck weiterhin greift auf den freigegebenen Member direkt aus die Instanz der Klasse und nicht auf die Standardinstanz verweist.

#### <a name="group-classes"></a>Von Gruppenklassen

Die `Microsoft.VisualBasic.MyGroupCollectionAttribute` Attribut gibt an, die Gruppe-Klasse für eine Standard-Instanzen. Das Attribut verfügt über vier Parameter:

* Der Parameter `TypeToCollect` gibt die Basisklasse für die Gruppe an. Alle instanziierbare Klassen ohne offene Typparameter, die von einem Typ mit diesem Namen (unabhängig vom Typparameter) abgeleitet werden, werden automatisch eine Standardinstanz zugewiesen.

* Der Parameter `CreateInstanceMethodName` gibt die Methode aufrufen, in der Gruppenklasse, um eine neue Instanz in eine Standardeigenschaft für die Instanz zu erstellen.

* Der Parameter `DisposeInstanceMethodName` gibt die Methode aufrufen, in der Gruppenklasse in der eine Standardeigenschaft für die Instanz zu verwerfen, wenn die Standard-Instance-Eigenschaft der Wert zugewiesen wird `Nothing`.

* Der Parameter `DefaultInstanceAlias` Besonderheiten des Ausdrucks `E'` , für den Namen der Klasse zu ersetzen, wenn die Standardinstanzen direkt über ihren Typnamen zugänglich sind. Wenn dieser Parameter ist `Nothing` oder eine leere Zeichenfolge, Standard-Instanzen auf diesem Gruppentyp nicht direkt über den Namen des Typs zugegriffen werden kann. (__Beachten.__ Bei allen aktuellen Implementierungen der Visual Basic-Sprache die `DefaultInstanceAlias` Parameter wird ignoriert, außer im Compiler bereitgestellten Code.)

Mehrere Typen können in der gleichen Gruppe erfasst werden, durch die Namen von Typen und Methoden in der ersten drei Parameter dabei mit Kommas trennen. Muss die gleiche Anzahl von Elementen in jeder Parameter vorhanden sein, und die Listenelemente der Reihe nach abgeglichen werden. Die folgende Attributdeklaration erfasst beispielsweise Typen, die abgeleitet `C1`, `C2` oder `C3` in einer einzelnen Gruppe:

```vb
<Microsoft.VisualBasic.MyGroupCollection("C1, C2, C3", _
    "CreateC1, CreateC2, CreateC3", _
    "DisposeC1, DisposeC2, DisposeC3", "My.Cs")>
Public NotInheritable Class MyCs
    ...
End Class
```

Die Signatur der Create-Methode muss im Format `Shared Function <Name>(Of T As {New, <Type>})(Instance Of T) As T`. Die Dispose-Methode muss im Format `Shared Sub <Name>(Of T As <Type>)(ByRef Instance Of T)`. Daher kann die Gruppe-Klasse für das Beispiel im vorherigen Abschnitt wie folgt deklariert werden:

```vb
<Microsoft.VisualBasic.MyGroupCollection("Form", "Create", _
    "Dispose", "My.Forms")> _
Public NotInheritable Class MyForms
    Private Shared Function Create(Of T As {New, Form}) _
        (Instance As T) As T
        If Instance Is Nothing Then
            Return New T()
        Else
            Return Instance
        End If
    End Function

    Private Shared Sub Dispose(Of T As Form)(ByRef Instance As T)
        Instance.Close()
        Instance = Nothing
    End Sub
End Class
```

Wenn eine Quelldatei mit eine abgeleitete Klasse deklariert `Form1`, wäre ist die Gruppe der generierten Klasse entspricht:

```vb
<Microsoft.VisualBasic.MyGroupCollection("Form", "Create", _
    "Dispose", "My.Forms")> _
Public NotInheritable Class MyForms
    Private Shared Function Create(Of T As {New, Form}) _
        (Instance As T) As T
        If Instance Is Nothing Then
            Return New T()
        Else
            Return Instance
        End If
    End Function

    Private Shared Sub Dispose(Of T As Form)(ByRef Instance As T)
        Instance.Close()
        Instance = Nothing
    End Sub

    Private m_Form1 As Form1

    Public Property Form1() As Form1
        Get
            Return Create(m_Form1)
        End Get
        Set (Value As Form1)
            If Value IsNot Nothing AndAlso Value IsNot m_Form1 Then
                Throw New ArgumentException( _
                    "Property can only be set to Nothing.")
            End If
            Dispose(m_Form1)
        End Set
    End Property
End Class
```

### <a name="extension-method-collection"></a>Erweiterungsauflistung-Methode

Erweiterungsmethoden für das Element zugreifen Ausdruck `E.I` werden gesammelt, indem Sie die Zusammenstellung der Erweiterungsmethoden mit dem Namen `I` , die im aktuellen Kontext verfügbar sind:

1. Zunächst wird jeden geschachtelter Typ mit dem Ausdruck überprüft, beginnend mit der innersten und zum äußersten.
2. Anschließend wird jeden geschachtelten Namespace überprüft, beginnend mit der innersten und dem äußeren Namespace navigieren.
3. Anschließend werden die Importe in der Quelldatei überprüft.
4. Anschließend werden die Importe, die definiert, die von der kompilierungsumgebung überprüft.

Nur dann, wenn eine erweiternde native Konvertierung vom Typ Ziel-Ausdrucks in den Typ des ersten Parameters der Erweiterungsmethode ist eine Erweiterungsmethode gesammelt. Und im Gegensatz zu regulären einfachen Namen ausdrucksbindung, die Suche erfasst *alle* Erweiterungsmethoden, die Auflistung wird nicht beendet, wenn eine Erweiterungsmethode gefunden wird. Zum Beispiel:

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
        Sub M1(c As C1, y As Double)
        End Sub
    End Module
End Namespace

Namespace N1.N2.N3
    Module Test
        Sub Main()
            Dim x As New C1()

            ' Calls N1C1Extensions.M1
            x.M1(10)
        End Sub
    End Module
End Namespace
```

In diesem Beispiel, obwohl `N2C1Extensions.M1` gefunden wird, bevor `N1C1Extensions.M1`, beide als Erweiterungsmethoden betrachtet. Sobald alle Erweiterungsmethoden erfasst wurden, sind sie *mit Currying*. Currying wird das Ziel der Aufruf der Erweiterungsmethode, und wendet sie auf der Aufruf der Erweiterungsmethode, wodurch die Signatur einer neuen Methode mit dem ersten Parameter entfernt (weil er angegeben wurde). Zum Beispiel:

```vb
Imports System.Runtime.CompilerServices

Module Ext1
    <Extension> _
    Sub M(x As Integer, y As Integer)
    End Sub
End Module

Module Ext2
    <Extension> _
    Sub M(x As Integer, y As Double)
    End Sub
End Module

Module Main
    Sub Test()
        Dim v As Integer = 10

        ' The curried method signatures considered are:
        '        Ext1.M(y As Integer)
        '        Ext2.M(y As Double)
        v.M(10)
    End Sub
End Module
```

Im obigen Beispiel, das mit Currying Ergebnis des Anwendens `v` zu `Ext1.M` ist die Signatur der Methode `Sub M(y As Integer)`.

Zusätzlich zum Entfernen des ersten Parameters der Erweiterungsmethode, entfernt das currying auch Typparameter Methode, die Teil des Typs des ersten Parameters sind. Wenn eine Erweiterungsmethode mit Methodentypparameter currying, Typrückschluss auf den ersten Parameter angewendet wird, und das Ergebnis ist festgelegt, für alle Typparameter, die abgeleitet werden. Wenn der Typrückschluss schlägt fehl, wird die Methode ignoriert. Zum Beispiel:

```vb
Imports System.Runtime.CompilerServices

Module Ext1
    <Extension> _
    Sub M(Of T, U)(x As T, y As U)
    End Sub
End Module

Module Ext2
    <Extension> _
    Sub M(Of T)(x As T, y As T)
    End Sub
End Module

Module Main
    Sub Test()
        Dim v As Integer = 10

        ' The curried method signatures considered are:
        '        Ext1.M(Of U)(y As U)
        '        Ext2.M(y As Integer)
        v.M(10)
    End Sub
End Module
```

Im obigen Beispiel, das mit Currying Ergebnis des Anwendens `v` zu `Ext1.M` ist die Signatur der Methode `Sub M(Of U)(y As U)`, da der Typparameter `T` wird als Ergebnis der currying abgeleitet und ist nun behoben. Da der Typparameter `U` wurde nicht per Rückschluss abgeleitet als Teil der currying, bleibt er ein open-Parameter. Auf ähnliche Weise, da der Typparameter `T` wird als Ergebnis der Anwendung abgeleitet `v` zu `Ext2.M`, der Typ des Parameters `y` wird als fester `Integer`. Es wird nicht abgeleitet werden, einen anderen Typ sein. Wenn die Signatur, die alle Einschränkungen, mit Ausnahme von currying `New` Einschränkungen werden ebenfalls angewendet. Wenn die Einschränkungen nicht erfüllt sind, oder Sie richten sich nach der ein Typ, der nicht als Teil der currying abgeleitet wurde, wird die Erweiterungsmethode ignoriert. Zum Beispiel:

```vb
Imports System.Runtime.CompilerServices

Module Ext1
    <Extension> _
    Sub M1(Of T As Structure)(x As T, y As Integer)
    End Sub

    <Extension> _
    Sub M2(Of T As U, U)(x As T, y As U)
    End Sub
End Module

Module Main
    Sub Test()
        Dim s As String = "abc"

        ' Error: String does not satisfy the Structure constraint
        s.M1(10)

        ' Error: T depends on U, which cannot be inferred
        s.M2(10)
    End Sub
End Module
```

__Beachten Sie.__ Einer der Hauptgründe, warum dies von Erweiterungsmethoden currying ist, dass es sich um Abfrageausdrücke zum Ableiten des Typs der Iteration vor der Auswertung der Argumente für eine Abfragemethode-Muster ermöglicht. Da die meisten Abfragemethoden Muster Lambda-Ausdrücken in Anspruch die Typrückschluss selbst erforderlich sind nehmen, vereinfacht dies deutlich das Auswerten eines Abfrageausdrucks.

Im Gegensatz zu normalen schnittstellenvererbung sind Erweiterungsmethoden, die zwei Schnittstellen zu erweitern, die nicht miteinander beziehen verfügbar, solange sie nicht die gleiche Curry-Signatur verfügen:

```vb
Imports System.Runtime.CompilerServices

Interface I1
End Interface

Interface I2
End Interface

Class C1
    Implements I1, I2
End Class

Module I1Ext
    <Extension> _
    Sub M1(i As I1, x As Integer)
    End Sub

    <Extension> _
    Sub M2(i As I1, x As Integer)
    End Sub
End Module

Module I2Ext
    <Extension> _
    Sub M1(i As I2, x As Integer)
    End Sub

    <Extension> _
    Sub M2(I As I2, x As Double)
    End Sub
End Module

Module Main
    Sub Test()
        Dim c As New C1()

        ' Error: M is ambiguous between I1Ext.M1 and I2Ext.M1.
        c.M1(10)

        ' Calls I1Ext.M2
        c.M2(10)
    End Sub
End Module
```

Schließlich ist es wichtig, diese Erweiterung ist, denken Sie daran, die Methoden nicht berücksichtigt werden, bei der späten Bindung:

```vb
Module Test
    Sub Main()
        Dim o As Object = ...

        ' Ignores extension methods
        o.M1()
    End Sub
End Module
```

## <a name="dictionary-member-access-expressions"></a>Memberzugriffsausdrücken

Ein *Wörterbuch Memberzugriffsausdruck* wird verwendet, um ein Mitglied einer Sammlung zu suchen. Eine wörterbuchmemberzugriff nimmt die Form `E!I`, wobei `E` ist ein Ausdruck, der als Wert klassifiziert wird und `I` ist ein Bezeichner.

```antlr
DictionaryAccessExpression
    : Expression? '!' IdentifierOrKeyword
    ;
```

Der Typ des Ausdrucks müssen durch eine einzelne indizierte Standardeigenschaft `String` Parameter. Das Wörterbuch-Memberzugriffsausdruck `E!I` transformiert wird, in dem Ausdruck `E.D("I")`, wobei `D` ist die Standardeigenschaft des `E`. Zum Beispiel:

```vb
Class Keys
    Public ReadOnly Default Property Item(s As String) As Integer
        Get
            Return 10
        End Get
    End Property 
End Class

Module Test
    Sub Main()
        Dim x As Keys = new Keys()
        Dim y As Integer
        ' The two statements are equivalent.
        y = x!abc
        y = x("abc")
    End Sub
End Module
```

Wenn kein Ausdruck, der den Ausdruck aus, die direkt mit einem Ausrufezeichen angegeben wird `With` Anweisung wird davon ausgegangen. Wenn es keine enthält ist `With` -Anweisung ein Fehler während der Kompilierung auftritt.


## <a name="invocation-expressions"></a>Aufrufausdrücke

Ein Aufrufausdruck besteht aus einem Aufrufziel und einer optionalen Argumentliste.

```antlr
InvocationExpression
    : Expression ( OpenParenthesis ArgumentList? CloseParenthesis )?
    ;

ArgumentList
    : PositionalArgumentList
    | PositionalArgumentList Comma NamedArgumentList
    | NamedArgumentList
    ;

PositionalArgumentList
    : Expression? ( Comma Expression? )*
    ;

NamedArgumentList
    : IdentifierOrKeyword ColonEquals Expression
      ( Comma IdentifierOrKeyword ColonEquals Expression )*
    ;
```

Der Zielausdruck muss klassifiziert werden, als einer Methodengruppe oder einen Wert, dessen Typ ein Delegattyp ist. Wenn der Zielausdruck ist ein Wert, dessen Typ ein Delegattyp, und klicken Sie dann das Ziel der Aufrufausdruck die Methodengruppe für wird die `Invoke` Mitglied der Delegattyp und den Zielausdruck wird der zugeordnete Zielausdruck der-Methode Gruppe.

Eine Argumentliste besteht aus zwei Abschnitten: positionelle Argumente und benannte Argumente. *Positionelle Argumente* sind Ausdrücke und müssen vor benannten Argumenten vorangestellt. *Benannte Argumente* starten mit einer ID, die Schlüsselwörter, gefolgt von abgleichen können `:=` und einen Ausdruck.

Wenn die Methodengruppe nur über eine zugängliche Methode enthält, einschließlich der sowohl die Instanz als auch die Erweiterung Methoden und die Methode keine Argumente akzeptiert und ist eine Funktion, klicken Sie dann die Methodengruppe wird als ein Aufrufausdruck mit einer leeren Argumentliste interpretiert und das Ergebnis ist als Ziel eines Aufrufausdrucks mit der angegebenen Argumente verwendet werden. Zum Beispiel:

```vb
Class C1
    Function M1() As Integer()
        Return New Integer() { 1, 2, 3 }
    End Sub
End Class

Module Test
    Sub Main()
        Dim c As New C1()

        ' Prints 3
        Console.WriteLine(c.M1(2))
    End Sub
End Module
```

Andernfalls wird die Auflösung von funktionsüberladungen an die Methoden, die am besten geeignete Methode für die angegebenen Argumente auszuwählen angewendet. Ist die am besten geeignete Methode eine Funktion, wird das Ergebnis des Aufrufausdrucks als Wert als den Rückgabetyp der Funktion klassifiziert. Wenn die am besten geeignete Methode eine Subroutine ist, wird das Ergebnis als "void" klassifiziert. Ist die am besten geeignete Methode für eine partielle Methode, die ohne Nachrichtentext, der Aufrufausdruck wird ignoriert, und das Ergebnis wird als "void" klassifiziert.

Für einen Aufrufausdruck früh gebundener sind die Argumente in der Reihenfolge ausgewertet, in denen die entsprechenden Parameter in der Zielmethode deklariert werden. Für spät gebundene Memberzugriffsausdruck, werden sie in der Reihenfolge in der der Ausdruck für den Elementzugriff ausgewertet: finden Sie im Abschnitt [Late-Bound Expressions](expressions.md#late-bound-expressions).

## <a name="overloaded-method-resolution"></a>Lösung für die überladene Methode:
Overload Resolution, Detailgenauigkeit der Member/Typen, die ein Argument erhält aufzulisten, die Generizität, Anwendbarkeit Argumentliste, übergeben von Argumenten und Auswählen der Argumente für optionale Parameter, bedingte Methoden und Typrückschluss Argument: finden Sie im Abschnitt [ Auflösen der Überladung](overload-resolution.md).

## <a name="index-expressions"></a>Indizieren von Ausdrücken

Ein *indizieren Ausdruck* führt zu einem Arrayelement oder Klassifizierung eine Eigenschaftengruppe in einen Eigenschaftenzugriff. Ein Indexausdruck besteht aus, in der Reihenfolge, ein Ausdruck, eine öffnende Klammer ein, eine Argumentliste für den Index und eine schließende Klammer ein.

```antlr
IndexExpression
    : Expression OpenParenthesis ArgumentList? CloseParenthesis
    ;
```

Das Ziel der Indexausdruck muss als ein Wert oder eine Eigenschaftengruppe klassifiziert werden. Ein Indexausdruck wird wie folgt verarbeitet:

* Wenn der Zielausdruck als Wert klassifiziert wird und dessen Typ kein Arraytyp ist `Object`, oder `System.Array`, der Typ muss eine Default-Eigenschaft aufweisen. Der Index wird für eine Eigenschaftengruppe ausgeführt, die alle die Standardeigenschaften des Typs darstellt. Obwohl es nicht zulässig, eine parameterlosen Standardeigenschaft in Visual Basic deklariert ist, können andere Sprachen, deklarieren eine solche Eigenschaft. Daher ist die Indizierung einer Eigenschaft ohne Argumente zulässig.

* Wenn der Ausdruck einen Wert eines Arraytyps ergibt, wird die Anzahl der Argumente in der Argumentliste muss identisch mit den Rang des Arraytyps und umfassen möglicherweise nicht die benannten Argumente. Wenn einer der Indizes zur Laufzeit ungültig sind eine `System.IndexOutOfRangeException` Ausnahme ausgelöst. Jeder Ausdruck muss implizit in den Typ `Integer`. Das Ergebnis des Indexausdrucks ist die Variable am angegebenen Index und wird als Variable klassifiziert.

* Wenn der Ausdruck als eine Eigenschaftengruppe klassifiziert ist, wird Auflösung von funktionsüberladungen verwendet, um zu bestimmen, ob eine der Eigenschaften an die Argumentliste Index anwendbar ist. Wenn die Eigenschaftengruppe nur eine Eigenschaft enthält, besitzt eine `Get` -Accessor und wenn diese Zugriffsmethode akzeptiert keine Argumente, die Eigenschaftengruppe wird als ein Indexausdruck mit einer leeren Argumentliste interpretiert. Das Ergebnis wird als Ziel für den aktuellen Indexausdruck verwendet. Wenn keine Eigenschaften anwendbar sind, tritt ein Fehler während der Kompilierung. Andernfalls führt der Ausdruck ein Eigenschaftszugriff mit den zugehörigen Zielausdruck (sofern vorhanden) der Eigenschaftengruppe.

* Wenn der Ausdruck klassifiziert ist, als eine Gruppe von spät gebundenen Eigenschaft oder als ein Wert, dessen Typ `Object` oder `System.Array`, die Verarbeitung des Indexausdrucks wird verzögert, bis zur Laufzeit und die Indizierung wird spät gebunden. Die Ergebnisse des Ausdrucks in einer Eigenschaft mit später Bindung den Zugriff als typisierte `Object`. Die zielbereitstellungsumgebung-Ausdruck ist der Zielausdruck, wenn er ein Wert ist, oder der zugehörigen Zielausdruck der Eigenschaftengruppe. Zur Laufzeit wird der Ausdruck wie folgt verarbeitet:

* Wenn der Ausdruck als spät gebundene Eigenschaftengruppe klassifiziert ist, möglicherweise der Ausdruck in einer Methodengruppe, eine Eigenschaftengruppe oder einen Wert (wenn das Element eine Instanz oder freigegebene Variable ist). Ist das Ergebnis einer Methodengruppe oder einer Eigenschaftengruppe, wird die Auflösung von funktionsüberladungen auf die Gruppe aus, um zu bestimmen, die richtige Methode für die Argumentliste angewendet. Wenn die Auflösung von funktionsüberladungen ein Fehler auftritt, eine `System.Reflection.AmbiguousMatchException` Ausnahme ausgelöst. Klicken Sie dann das Ergebnis als Zugriff auf eine Eigenschaft oder ein Aufruf verarbeitet wird, und das Ergebnis wird zurückgegeben. Wenn der Aufruf eine Subroutine ist, wird das Ergebnis ist `Nothing`.

* Die Run-Time-Typ, der den Zielausdruck ist ein Arraytyp oder `System.Array`, das Ergebnis des Indexausdrucks ist der Wert der Variablen am angegebenen Index.

* Andernfalls die Run-Time-Typ des Ausdrucks muss eine Default-Eigenschaft haben, und der Index wird ausgeführt, auf die Eigenschaftengruppe, die alle die Standardeigenschaften für den Typ darstellt. Wenn der Typ keine Standardeigenschaft hat ein `System.MissingMemberException` Ausnahme ausgelöst.


## <a name="new-expressions"></a>New-Ausdrücke

Die `New` Operator wird verwendet, um neue Instanzen von Typen zu erstellen. Es gibt vier Arten von `New` Ausdrücke:

* Objekterstellung-Ausdrücke werden verwendet, um neue Instanzen der Klasse und Werttypen zu erstellen.

* Für die Arrayerstellung-Ausdrücke werden verwendet, um neue Instanzen von Arraytypen zu erstellen.

* Delegaterstellung Ausdrücke (die eine unterschiedliche Syntax von Objekt-und Arrayerstellung Ausdrücken keine) werden verwendet, um neue Instanzen von Delegaten erstellt.

* Anonyme objekterstellung-Ausdrücke werden verwendet, um neue Instanzen der anonyme Klassentypen zu erstellen.

```antlr
NewExpression
    : ObjectCreationExpression
    | ArrayExpression
    | AnonymousObjectCreationExpression
    ;
```

Ein `New` Ausdruck wird als Wert klassifiziert, und das Ergebnis ist die neue Instanz des Typs.


### <a name="object-creation-expressions"></a>Objekterstellung Ausdrücke

Ein Ausdruck für die objekterstellung wird verwendet, um eine neue Instanz der ein Klassen- oder Strukturtyp zu erstellen.

```antlr
ObjectCreationExpression
    : 'New' NonArrayTypeName ( OpenParenthesis ArgumentList? CloseParenthesis )?
      ObjectCreationExpressionInitializer?
    ;

ObjectCreationExpressionInitializer
    : ObjectMemberInitializer
    | ObjectCollectionInitializer
    ;

ObjectMemberInitializer
    : 'With' OpenCurlyBrace FieldInitializerList CloseCurlyBrace
    ;

FieldInitializerList
    : FieldInitializer ( Comma FieldInitializer )*
    ;

FieldInitializer
    : 'Key'? ('.' IdentifierOrKeyword Equals )? Expression
    ;

ObjectCollectionInitializer
    : 'From' CollectionInitializer
    ;

CollectionInitializer
    : OpenCurlyBrace CollectionElementList? CloseCurlyBrace
    ;

CollectionElementList
    : CollectionElement ( Comma CollectionElement )*
    ;

CollectionElement
    : Expression
    | CollectionInitializer
    ;
```

Der Typ, der eine Objekterstellungsausdruck muss ein Klassentyp, einen Strukturtyp oder ein Typparameter mit einer `New` Einschränkung und kann kein `MustInherit` Klasse. Erhält ein Objekterstellungsausdruck des Formulars `New T(A)`, wobei `T` ist eine Klassen- oder Strukturtyp und `A` ist eine optionale Argumentliste Auflösung von funktionsüberladungen bestimmt den korrekten Konstruktor von `T` aufrufen. Ein Typparameter mit einer `New` Einschränkung gilt als einen einzelnen, parameterlosen Konstruktor haben. Wenn kein Konstruktor aufgerufen werden kann, tritt ein Fehler während der Kompilierung; Andernfalls führt der Ausdruck zur Erstellung einer neuen Instanz von `T` mit dem ausgewählten Konstruktor. Wenn keine Argumente vorhanden sind, können die Klammern weggelassen werden.

Der eine Instanz zugeordnet ist, hängt davon ab, ob die Instanz eines Klassentyps oder ein Werttyp ist. `New` Instanzen von Klassentypen werden auf den Systemheap, erstellt, während neue Instanzen von Werttypen, direkt auf dem Stapel erstellt werden.

Ein Ausdruck für die objekterstellung kann optional eine Liste der Standardmember-Initialisierer nach dem Konstruktorargument angeben. Diese Standardmember-Initialisierer werden mit dem Schlüsselwort mit dem Präfix `With`, und die Initialisiererliste wird interpretiert, als wäre es im Rahmen einer `With` Anweisung. Betrachten Sie z. B. die Klasse ein:

```vb
Class Customer
    Dim Name As String
    Dim Address As String
End Class
```

Der Code:

```vb
Module Test
    Sub Main()
        Dim x As New Customer() With { .Name = "Bob Smith", _
            .Address = "123 Main St." }
    End Sub
End Module
```

ist ungefähr gleich ist:

```vb
Module Test
    Sub Main()
        Dim x, _t1 As Customer

        _t1 = New Customer()
        With _t1
            .Name = "Bob Smith"
            .Address = "123 Main St."
        End With

        x = _t1
    End Sub
End Module
```

Jeder Initialisierer muss angeben, dass ein Name zugewiesen, und es muss der Name einer nicht -`ReadOnly` Instanzvariable oder eine Eigenschaft des Typs erstellt wird; der Memberzugriff wird nicht spät gebunden werden, wenn der Typ der zu erstellenden `Object`. Initialisierer können Sie nicht die `Key` Schlüsselwort. Jedes Element in einem Typ kann nur einmal initialisiert werden. Die Initialisierer-Ausdrücke können jedoch aufeinander verweisen. Zum Beispiel:

```vb
Module Test
    Sub Main()
        Dim x As New Customer() With { .Name = "Bob Smith", _
            .Address = .Name & " St." }
    End Sub
End Module
```

Die Initialisierer links nach rechts zugewiesen ist, also wenn Sie ein Initialisierer für einen Member bezieht, die noch nicht initialisiert wurde, anzeigen, wird Wert was auch immer die Instanzvariable, nachdem der Konstruktor ausgeführt:

```vb
Module Test
    Sub Main()
        ' The value of Address will be " St." since Name has not been
        ' assigned yet.
        Dim x As New Customer() With { .Address = .Name & " St." }
    End Sub
End Module
```

Initialisierer können geschachtelt werden:

```vb
Class Customer
    Dim Name As String
    Dim Address As Address
    Dim Age As Integer
End Class

Class Address
    Dim Street As String
    Dim City As String
    Dim State As String
    Dim ZIP As String
End Class

Module Test
    Sub Main()
        Dim c As New Customer() With { _
            .Name = "John Smith", _
            .Address = New Address() With { _
                .Street = "23 Main St.", _
                .City = "Peoria", _
                .State = "IL", _
                .ZIP = "13934" }, _
            .Age = 34 }
    End Sub
End Module
```

Wenn der erstellte Typ wird vom Auflistungstyp und verfügt über eine Instanzmethode mit der Bezeichnung `Add` (einschließlich Erweiterungsmethoden und freigegebene Methoden), geben Sie dann der Ausdruck für die objekterstellung kann einen Auflistungsinitialisierer, mit dem Präfix werden mit dem Schlüsselwort `From`. Ein Ausdruck für die objekterstellung Memberinitialisierer und einem Auflistungsinitialisierer nicht angegeben werden. Jedes Element im Auflistungsinitialisierer als Argument übergeben wird, auf einen Aufruf der `Add` Funktion. Zum Beispiel:

```vb
Dim list = New List(Of Integer)() From { 1, 2, 3, 4 }
```

identisch mit folgendem Ausdruck:

```vb
Dim list = New List(Of Integer)()
list.Add(1)
list.Add(2)
list.Add(3)
```

Wenn ein Element eines Auflistungsinitialisierers selbst ist, wird jedes Element des Initialisierers unterauflistung übergeben werden, als einzelne Argument an die `Add` Funktion. Z. B. Folgendes:

```vb
Dim dict = Dictionary(Of Integer, String) From { { 1, "One" },{ 2, "Two" } }
```

identisch mit folgendem Ausdruck:

```vb
Dim dict = New Dictionary(Of Integer, String)
dict.Add(1, "One")
dict.Add(2, "Two")
```

Diese Erweiterung ist erfolgt immer und immer nur eine Ebene tief; Danach gelten die untergeordnete Initialisierer Array-Literale. Zum Beispiel:

```vb
' Error: List(Of T) does not have an Add method that takes two parameters.
Dim list = New List(Of Integer())() From { { 1, 2 }, { 3, 4 } }

' OK, this initializes the dictionary with (Integer, Integer()) pairs.
Dim dict = New Dictionary(Of Integer, Integer())() From _
        { {  1, { 2, 3 } }, { 3, { 4, 5 } } }
```


### <a name="array-expressions"></a>Array-Ausdrücke

Ein Arrayausdruck wird verwendet, um eine neue Instanz der Array-Typ zu erstellen. Es gibt zwei Arten von Array-Ausdrücke: Array erstellen Ausdrücke und Array-Literale.

#### <a name="array-creation-expressions"></a>Arrayausdrücke-Erstellung

Wenn ein Array-Größe Initialisierung-Modifizierer angegeben ist, wird der resultierende Arraytyp abgeleitet, durch das Löschen der einzelnen der einzelnen Argumente aus der Argumentliste von Array Größe Initialisierung. Der Wert jedes Argument bestimmt die obere Grenze der entsprechenden Dimension in der neu zugewiesenen Arrayinstanz fest. Wenn der Ausdruck einen nicht leeren Auflistungsinitialisierer, jedes Argument in der Argumentliste muss eine Konstante sein, und der Rang jede Dimension angegebenen Länge angegeben wird, indem Sie die Liste der Ausdrücke müssen mit dem der Auflistungsinitialisierer.

```vb
Dim a() As Integer = New Integer(2) {}
Dim b() As Integer = New Integer(2) { 1, 2, 3 }
Dim c(,) As Integer = New Integer(1, 2) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error, length/initializer mismatch.
Dim d() As Integer = New Integer(2) { 0, 1, 2, 3 }
```

Wenn ein Array-Größe Initialisierung-Modifizierer nicht angegeben wird, klicken Sie dann der Typname muss ein Arraytyp sein und der Auflistungsinitialisierer muss leer sein oder die gleiche Anzahl von Schachtelungsebenen als der Rang des angegebenen Arraytyps. Alle Elemente in der innersten Schachtelungsebene muss implizit in den Typ des Elements des Arrays sein und muss als Wert klassifiziert werden. Die Anzahl der Elemente in jede geschachtelte Auflistungsinitialisierer muss immer konsistent mit der Größe der anderen Sammlungen auf der gleichen Ebene sein. Die Längen der einzelnen Dimension werden von der Anzahl der Elemente in jedem von der entsprechenden Schachtelungsebenen des auflistungs-bzw. Arrayinitialisierers hergeleitet. Wenn der Auflistungsinitialisierer leer ist, ist die Länge jeder Dimension 0 (null).

```vb
Dim e() As Integer = New Integer() { 1, 2, 3 }
Dim f(,) As Integer = New Integer(,) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error: Inconsistent numbers of elements!
Dim g(,) As Integer = New Integer(,) { { 1, 2 }, { 4, 5, 6 } }

' Error: Inconsistent levels of nesting!
Dim h(,) As Integer = New Integer(,) { 1, 2, { 3, 4 } }
```

Die äußerste Schachtelungsebene eines Auflistungsinitialisierers entsprechen der am weitesten links stehende Dimension eines Arrays, und die innerste Schachtelungsebene entspricht die Dimension ganz rechts. Beispiel:

```vb
Dim array As Integer(,) = _
    { { 0, 1 }, { 2, 3 }, { 4, 5 }, { 6, 7 }, { 8, 9 } }
```

Ist äquivalent zu folgendem:

```vb
Dim array(4, 1) As Integer

array(0, 0) = 0: array(0, 1) = 1
array(1, 0) = 2: array(1, 1) = 3
array(2, 0) = 4: array(2, 1) = 5
array(3, 0) = 6: array(3, 1) = 7
array(4, 0) = 8: array(4, 1) = 9
```

Wenn der Auflistungsinitialisierer ist leer (d. h. eine, die geschweifte Klammern enthält), aber keine Initialisierungsliste und die Grenzen des der Dimensionen des Arrays initialisiert wird, sind bekannt, der Initialisierer für die leere Auflistung stellt eine Arrayinstanz der angegebenen Größe in dem alle Elemente auf den Typ des Elements Standardwert initialisiert haben. Wenn die Begrenzungen der Dimensionen des Arrays initialisiert wird, nicht bekannt sind, stellt die leere Auflistung-Initialisierer eine Array-Instanz, die in der alle Dimensionen Größe 0 (null) sind dar.

Rang und die Länge jeder Dimension einer Arrayinstanz bleiben während der gesamten Lebensdauer der Instanz zur Verfügung. Das heißt, es ist nicht möglich, den Rang einer vorhandenen Instanz des Arrays zu ändern, noch ist es möglich, die Größe seiner Dimensionen ändern.

#### <a name="array-literals"></a>Arrayliterale

Ein Arrayliteral gibt ein Array, dessen Elementtyp, Rang und Grenzen aus einer Kombination des Kontexts Ausdruck und einem Auflistungsinitialisierer abgeleitet werden. Dies wird im Abschnitt erläutert [Ausdruck Neuklassifizierung](expressions.md#expression-reclassification).

```antlr
ArrayExpression
    : ArrayCreationExpression
    | ArrayLiteralExpression
    ;

ArrayCreationExpression
    : 'New' NonArrayTypeName ArrayNameModifier CollectionInitializer
    ;

ArrayLiteralExpression
    : CollectionInitializer
    ;
```

Zum Beispiel:

```vb
' array of integers
Dim a = {1, 2, 3}

' array of shorts
Dim b = {1S, 2S, 3S}

' array of shorts whose type is taken from the context
Dim c As Short() = {1, 2, 3}

' array of type Integer(,)
Dim d = {{1, 0}, {0, 1}}

' jagged array of rank ()()
Dim e = {({1, 0}), ({0, 1})}

' error: inconsistent rank
Dim f = {{1}, {2, 3}}

' error: inconsistent rank
Dim g = {1, {2}}
```

Das Format und die Anforderungen für den Auflistungsinitialisierer in einem Arrayliteral entspricht genau der für den Auflistungsinitialisierer in einem Arrayerstellungsausdruck.

__Beachten Sie.__ Ein Array-literal erstellt das Array an und für sich keine, Stattdessen ist es die neuklassifizierung des Ausdrucks in einen Wert, der bewirkt, dass das Array, das erstellt werden. Z. B. die Konvertierung `CType(new Integer() {1,2,3}, Short())` ist nicht möglich, da keine von Konvertierung `Integer()` zu `Short()`, aber der Ausdruck `CType({1,2,3},Short())` ist möglich, da sie zuerst das Array-literal in den Ausdruck zur Arrayerstellung Klassifizierung `New Short() {1,2,3}`.


### <a name="delegate-creation-expressions"></a>Delegaterstellung Ausdrücke

Ein Ausdruck eines wird verwendet, um eine neue Instanz eines Delegattyps zu erstellen. Das Argument eines eines Ausdrucks muss es sich um einen Ausdruck, der als ein Methodenzeiger oder einen Lambda-Methode klassifiziert sein.

Wenn das Argument einer Methodenzeiger ist, muss gilt für die Signatur des Delegattyps eine der Methoden auf, die durch die Zeiger auf den verwiesen wird. Eine Methode `M` gilt für einen Delegattyp `D` wenn:

* `M` ist kein `Partial` oder Text.

* Beide `M` und `D` sind Funktionen, oder `D` ist eine Unterroutine.

* `M` und `D` die gleiche Anzahl von Parametern aufweisen.

* Die Parametertypen der `M` haben jeweils eine Konvertierung vom Typ des entsprechenden Parametertyps des `D`, und die Modifizierer (d. h. `ByRef`, `ByVal`) übereinstimmen.

* Der Rückgabetyp der `M`, sofern vorhanden, hat es sich um eine Konvertierung in den Rückgabetyp der `D`.

Wenn die Zeiger auf ein spät gebundener Zugriff verweist, wird der spät gebundener Zugriff angenommen, dass auf eine Funktion sein, die die gleiche Anzahl von Parametern wie der Delegattyp.

Wenn die strenge Semantik nicht verwendet wird und es wird nur eine Methode, die durch die Zeiger auf den verwiesen wird, aber es ist nicht anwendbar aufgrund der Tatsache, dass es besitzt keine Parameter ist der Typ des Delegaten, und die Methode wird als anwendbar betrachtet und die Parameter oder Rückgabewerte einer RE ignoriert einfach. Zum Beispiel:

```vb
Delegate Sub F(x As Integer)

Module Test
    Sub M()
    End Sub

    Sub Main()
        ' Valid
        Dim x As F = AddressOf M
    End Sub
End Module
```

__Beachten Sie.__ Die Lockerung ist nur zulässig, wenn aufgrund von Erweiterungsmethoden nicht strikte Semantik verwendet werden. Da Erweiterungsmethoden nur betrachtet werden, wenn eine normale Methode ungültig war, ist es möglich, für eine Instanzmethode ohne Parameter aus, um eine Erweiterungsmethode mit Parametern für die Erstellung des Delegaten auszublenden.

Wenn mehr als eine Methode, die durch die Zeiger auf den verwiesen wird in den Delegattyp anwendbar und anschließend überladungsauflösung wird verwendet, um zwischen den möglichen Methoden auszuwählen. Die Typen der Parameter für den Delegaten werden als die Typen der Argumente für die Zwecke der Auflösung von funktionsüberladungen verwendet. Wenn kein Kandidat für eine Methode am besten geeignete ein Fehler während der Kompilierung auftritt ist. Im folgenden Beispiel wird die lokale Variable mit einem Delegaten, der auf die zweite verweist initialisiert `Square` Methode, da diese Methode in der Methodensignatur und rückgabeanweisung Typ des ist `DoubleFunc`.

```vb
Delegate Function DoubleFunc(x As Double) As Double

Module Test
    Function Square(x As Single) As Single
        Return x * x
    End Function 

    Function Square(x As Double) As Double
        Return x * x
    End Function

    Sub Main()
        Dim a As New DoubleFunc(AddressOf Square)
    End Sub
End Module
```

Haben Sie die zweite `Square` -Methode nicht vorhanden war, werden die ersten `Square` Methode würde gewählt wurde. Wenn strikte Semantik, durch die kompilierungsumgebung oder durch angegeben werden `Option Strict`, und klicken Sie dann ein Fehler während der Kompilierung tritt auf, wenn die spezifischste Methode, die auf die verwiesen wird durch die Zeiger auf den schmaler als die Signatur des Delegaten ist. Eine Methode `M` gilt schmaler als einen Delegattyp `D` wenn:

* Der Parametertyp `M` verfügt über eine erweiternde Konvertierung in den entsprechenden Parametertyps des `D`.

* Oder der Rückgabetyp, falls vorhanden, der `M` verfügt über eine einschränkende Konvertierung in den Rückgabetyp der `D`.

Wenn Argumente des Typs die Zeiger auf den zugeordnet sind, gelten nur für Methoden mit der gleichen Anzahl von Typargumenten. Wenn keine Typargumente die Zeiger auf den zugeordnet sind, wird ein Typrückschluss verwendet, beim Abgleich von Signaturen für eine generische Methode. Im Gegensatz zu anderen normalen Typrückschluss der Rückgabetyp des Delegaten wird verwendet, wenn die Typargumente ableiten, aber Rückgabetypen sind immer noch nicht berücksichtigt, wenn die am wenigsten generische Überladung zu bestimmen. Das folgende Beispiel zeigt die beiden Möglichkeiten zum Bereitstellen von Typargument für einen Delegaterstellungsausdruck:

```vb
Delegate Function D(s As String, i As Integer) As Integer
Delegate Function E() As Integer

Module Test
    Public Function F(Of T)(s As String, t1 As T) As T
    End Function

    Public Function G(Of T)() As T
    End Function

    Sub Main()
        Dim d1 As D = AddressOf f(Of Integer)    ' OK, type arg explicit
        Dim d2 As D = AddressOf f                ' OK, type arg inferred

        Dim e1 As E = AddressOf g(Of Integer)    ' OK, type arg explicit
        Dim e2 As E = AddressOf g                ' OK, infer from return
  End Sub
End Module
```

Im obigen Beispiel wurde ein nicht generischer Delegattyp mit einer generischen Methode instanziiert. Es ist auch möglich, zum Erstellen einer Instanz eines erstellten Delegattyps mit einer generischen Methode. Zum Beispiel:

```vb
Delegate Function Predicate(Of U)(u1 As U, u2 As U) As Boolean

Module Test
    Function Compare(Of T)(t1 As List(of T), t2 As List(of T)) As Boolean
        ...
    End Function

    Sub Main()
        Dim p As Predicate(Of List(Of Integer))
        p = AddressOf Compare(Of Integer)
    End Sub
End Module
```

Wenn das Argument für die delegaterstellung Ausdruck einer Lambda-Methode ist, muss der Lambda-Methode für die Signatur des Delegattyps sein. Ein Lambda-Methode `L` gilt für einen Delegattyp `D` wenn:

* Wenn `L` verfügt über Parameter, `D` hat die gleiche Anzahl von Parametern. (Wenn `L` enthält keine Parameter, die Parameter der `D` werden ignoriert.)

* Die Parametertypen der `L` haben jeweils eine Konvertierung in den Typ des entsprechenden Parametertyps des `D`, und die Modifizierer (d. h. `ByRef`, `ByVal`) übereinstimmen.

* Wenn `D` ist eine Funktion, die den Rückgabetyp der `L` verfügt über eine Konvertierung in den Rückgabetyp der `D`. (Wenn `D` ist eine Unterroutine, die den Rückgabewert der `L` wird ignoriert.)

Wenn der Parameter einen Parameter vom Typ `L` weggelassen wird, klicken Sie dann den Typ des entsprechenden Parameters in `D` abgeleitet wird, wenn der Parameter der `L` Array oder NULL-Werte zulässt, Name-Modifizierer, wird ein Fehler während der Kompilierung ausgegeben. Einmal alle die Parametertypen der `L` sind verfügbar, und klicken Sie dann der Typ des Ausdrucks in der Lambda-Methode abgeleitet wird. Zum Beispiel:

```vb
Delegate Function F(x As Integer, y As Long) As Long

Module Test
    Sub Main()
        ' b inferred to Integer, c and return type inferred to Long
        Dim a As F = Function(b, c) b + c

        ' e and return type inferred to Integer, f inferred to Long
        Dim d As F = Function(e, f) e + CInt(f)
    End Sub
End Module
```

In einigen Situationen, in denen Signatur des Delegaten nicht genau der Lambda-Methode oder die Signatur der Methode übereinstimmt, kann .NET Framework die Delegateerstellung nicht nativ unterstützt. In diesem Fall wird ein Lambda-Ausdruck-Methode verwendet, mit die beiden Methoden übereinstimmen. Zum Beispiel:

```vb
Delegate Function IntFunc(x As Integer) As Integer

Module Test
    Function SquareString(x As String) As String
        Return CInt(x) * CInt(x)
    End Function 

    Sub Main()
        ' The following two lines are equivalent
        Dim a As New IntFunc(AddressOf SquareString)
        Dim b As New IntFunc( _
            Function(x As Integer) CInt(SquareString(CStr(x))))
    End Sub
End Module
```

Das Ergebnis eines Ausdrucks eines ist eine Delegatinstanz, die sich auf die entsprechende Methode mit den zugehörigen Zielausdruck (sofern vorhanden) aus der Methode Zeigerausdruck bezieht. Wenn der Zielausdruck als Werttyp typisiert ist, wird der Werttyp auf dem Systemheap kopiert, daran, dass ein Delegat nur auf eine Methode eines Objekts auf dem Heap kann. Der Methode und des Objekts, auf die ein Delegat verweist, bleiben konstant, während der gesamten Lebensdauer des Delegaten. Das heißt, ist es nicht möglich, das Ziel oder ein Objekt eines Delegaten zu ändern, nachdem es erstellt wurde.

### <a name="anonymous-object-creation-expressions"></a>Anonyme Objekterstellung-Ausdrücke

Ein Ausdruck der objekterstellung mit Memberinitialisierer kann der Typname auch ganz weglassen.

```antlr
AnonymousObjectCreationExpression
    : 'New' ObjectMemberInitializer
    ;
```

In diesem Fall wird ein anonymer Typ erstellt, basierend auf den Typen und Namen der Member als Teil des Ausdrucks initialisiert. Zum Beispiel:

```vb
Module Test
    Sub Main()
        Dim Customer = New With { .Name = "John Smith", .Age = 34 }

        Console.WriteLine(Customer.Name)
    End Sub
End Module
```

Der Typ, der durch einen Ausdruck für die anonyme objekterstellung erstellt ist, eine Klasse, die keinen Namen hat, erbt direkt von `Object`, und enthält eine Reihe von Eigenschaften mit dem gleichen Namen wie die Elemente in der Liste der Member-Initialisierer zugewiesen. Der Typ jeder Eigenschaft wird mit den gleichen Regeln als lokale Variable Typrückschluss abgeleitet. Generierte, anonyme Typen überschreiben auch `ToString`, eine Zeichenfolgendarstellung für alle Elemente und deren Werte zurückgeben. (Das genaue Format dieser Zeichenfolge ist Gegenstand dieser Spezifikation).

Standardmäßig sind die Eigenschaften des anonymen Typs vom Lese-/ Schreibzugriff. Es ist möglich, eine Eigenschaft eines anonymen Typs als schreibgeschützt zu markieren, mithilfe der `Key` Modifizierer. Die `Key` %(Dateiname) gibt an, dass das Feld verwendet werden kann, zur eindeutigen Identifizierung den Wert des anonymen Typs darstellt. Nicht nur die Eigenschaft schreibgeschützt, bewirkt außerdem, dass den anonymen Typ überschreiben `Equals` und `GetHashCode` und implementieren die Schnittstelle `System.IEquatable(Of T)` (Ausfüllen des anonymen Typs für `T`). Die Elemente werden wie folgt definiert:

`Function Equals(obj As Object) As Boolean` und `Function Equals(val As T) As Boolean` werden implementiert, überprüfen, dass die beiden Instanzen des gleichen Typs sind, und klicken Sie dann verglichen wird, jede `Key` Member mit `Object.Equals`. Wenn alle `Key` Elemente gleich sind, klicken Sie dann `Equals` gibt `True`, andernfalls `Equals` gibt `False`.

`Function GetHashCode() As Integer` wird implementiert, dass, auch wenn `Equals` ist "true" für zwei Instanzen des anonymen Typs, `GetHashCode` wird derselbe Wert zurückgegeben. Der Hash wird gestartet, mit einem Ausgangswert, und klicken Sie dann für jede `Key` Member, in der Reihenfolge den Hash durch 31 multipliziert und fügt die `Key` Hashwert des Members (gebotenen `GetHashCode`) ist das Element kein Verweistyp oder Werttyp mit dem Wert des `Nothing`.

Beispielsweise muss der Typ in der Anweisung erstellt:

```vb
Dim zipState = New With { Key .ZipCode = 98112, .State = "WA" }
```

erstellt eine Klasse, die in etwa wie folgt aussieht (obwohl es sich um eine genaue Implementierung möglicherweise unterscheiden):

```vb
Friend NotInheritable Class $Anonymous1
    Implements IEquatable(Of $Anonymous1)

    Private ReadOnly _zipCode As Integer
    Private _state As String

    Public Sub New(zipCode As Integer, state As String)
        _zipCode = zipcode
        _state = state
    End Sub

    Public ReadOnly Property ZipCode As Integer
        Get
            Return _zipCode
        End Get
    End Property

    Public Property State As String
        Get
            Return _state
        End Get
        Set (value As Integer)
            _state = value
        End Set
    End Property

    Public Overrides Function Equals(obj As Object) As Boolean
        Dim val As $Anonymous1 = TryCast(obj, $Anonymous1)
        Return Equals(val)
    End Function

    Public Overloads Function Equals(val As $Anonymous1) As Boolean _
        Implements IEquatable(Of $Anonymous1).Equals

        If val Is Nothing Then 
            Return False
        End If

        If Not Object.Equals(_zipCode, val._zipCode) Then 
            Return False
        End If

        Return True
    End Function

    Public Overrides Function GetHashCode() As Integer
        Dim hash As Integer = 0

        hash = hash Xor _zipCode.GetHashCode()

        Return hash
    End Function

    Public Overrides Function ToString() As String
        Return "{ Key .ZipCode = " & _zipCode & ", .State = " & _state & " }"
    End Function
End Class
```

Um die Situation zu vereinfachen, in ein anonymer Typ aus den Feldern eines anderen Typs erstellt wird, können die Feldnamen direkt in Ausdrücken in den folgenden Fällen abgeleitet werden:

* Einem einfachen Namensausdruck `x` leitet den Namen `x`.

* Ein Memberzugriffsausdruck `x.y` leitet den Namen `y`.

* Ein Wörterbuch Lookup-Ausdruck `x!y` leitet den Namen `y`.

* Ein Ausdruck aufrufen oder einen Index ohne Argumente `x()` leitet den Namen `x`.

* Ein XML-Memberzugriffsausdruck `x.<y>`, `x...<y>`, `x.@y` leitet den Namen `y`.

* Ein XML-Memberzugriffsausdruck, der das Ziel eines Memberzugriffsausdrucks ist `x.<y>.z` leitet den Namen `z`.

* Ein XML-Memberzugriffsausdruck, der das Ziel eines Aufrufs oder Index Ausdrucks ohne Argumente ist `x.<y>.z()` leitet den Namen `z`.

* Ein XML-Memberzugriffsausdruck, der das Ziel eines Aufrufs oder Index-Ausdrucks ist `x.<y>(0)` leitet den Namen `y`.

Die Initialisierung wird als eine Zuweisung des Ausdrucks, der dem abgeleiteten Namen interpretiert. Die folgenden Initialisierer sind beispielsweise äquivalent:

```vb
Class Address
    Public Street As String
    Public City As String
    Public State As String
    Public ZIP As String
End Class

Class C1
    Sub Test(a As Address)
        Dim cityState1 = New With { .City = a.City, .State = a.State }
        Dim cityState2 = New With { a.City, a.State }
    End Sub
End Class
```

Wenn Sie ein Elementnamen abgeleitet wird, die steht in Konflikt mit einem vorhandenen Member des Typs, z. B. `GetHashCode`, tritt ein, auf ein Fehler zur Kompilierzeit. Im Gegensatz zu regulären Standardmember-Initialisierer zulassen anonyme objekterstellung Ausdrücke nicht Member Initialisierer keine Zirkelverweise verwendet oder auf einen Member verweisen, bevor es initialisiert wurde. Zum Beispiel:

```vb
Module Test
    Sub Main()
        ' Error: Circular references
        Dim x = New With { .a = .b, .b = .a }

        ' Error: Referring to .b before it has been assigned to
        Dim y = New With { .a = .b, .b = 10 }

        ' Error: Referring to .a before it has been assigned to
        Dim z = New With { .a = .a }
    End Sub
End Module
```

Wenn zwei anonyme Klasse erstellen Ausdrücke innerhalb der gleichen Methode auftreten, und die gleiche resultierende Form – ergeben, wenn die Reihenfolge der Eigenschaft, die Eigenschaftennamen und die Eigenschaft Typen alle überein: werden sie beide auf dieselbe anonyme Klasse verweisen. Der Methodenbereich, der eine Instanz oder einen freigegebenen Member-Variable mit einem Initialisierer ist der Konstruktor, in dem die Variable initialisiert wird.

__Beachten Sie.__ Es ist möglich, dass ein Compiler anonyme vereinheitlichen auswählen kann Typen weiter, z. B. wie auf der Assemblyebene, aber dies kann nicht als zuverlässig betrachtet werden zu diesem Zeitpunkt.


## <a name="cast-expressions"></a>CAST-Ausdrücke

Cast-Ausdruck Wandelt einen Ausdruck in einen angegebenen Typ. Bestimmte Umwandlungsschlüsselwörter coerce-Ausdrücke in den primitiven Typen gehört. Drei allgemeine Umwandlungsschlüsselwörter `CType`, `TryCast` und `DirectCast`, einen Ausdruck in einen Typ umgewandelt werden soll.

```antlr
CastExpression
    : 'DirectCast' OpenParenthesis Expression Comma TypeName CloseParenthesis
    | 'TryCast' OpenParenthesis Expression Comma TypeName CloseParenthesis
    | 'CType' OpenParenthesis Expression Comma TypeName CloseParenthesis
    | CastTarget OpenParenthesis Expression CloseParenthesis
    ;

CastTarget
    : 'CBool' | 'CByte' | 'CChar'  | 'CDate'  | 'CDec' | 'CDbl' | 'CInt'
    | 'CLng'  | 'CObj'  | 'CSByte' | 'CShort' | 'CSng' | 'CStr' | 'CUInt'
    | 'CULng' | 'CUShort'
    ;
```

`DirectCast` und `TryCast` spezielle Verhalten aufweisen. Aus diesem Grund unterstützen sie nur systemeigene Konvertierungen. Darüber hinaus den Zieltyp in einer `TryCast` Ausdruck handelt es sich nicht um einen Werttyp. Benutzerdefinierte Operatoren für die Konvertierung nicht beim berücksichtigt `DirectCast` oder `TryCast` verwendet wird. (__Beachten.__ Die Konvertierung festgelegt, die `DirectCast` und `TryCast` Unterstützung sind eingeschränkt, da diese Konvertierungen von "systemeigener CLR" implementieren. Der Zweck der `DirectCast` wird zum Bereitstellen der Funktionen der Anweisung "Unboxing", während der Zweck der `TryCast` besteht darin, die Funktionalität der Anweisung "Isinst" bereitzustellen. Da sie auf der CLR-Anweisungen zuordnen, widerspricht Unterstützung von Konvertierungen, die nicht direkt von der CLR unterstützt den beabsichtigten Zweck.)

`DirectCast` Konvertiert die Ausdrücke, die als typisiert sind `Object` anders als bei `CType`. Bei der Konvertierung eines Ausdrucks vom Typ `Object` , dessen Typ zur Laufzeit ist ein primitiver Wert `DirectCast` löst eine `System.InvalidCastException` -Ausnahme aus, wenn der angegebene Typ nicht identisch mit der Run-Time-Typ des Ausdrucks ist oder eine `System.NullReferenceException` Wenn der Ausdruck ergibt `Nothing`. (__Beachten.__ Wie bereits erwähnt, `DirectCast` Maps direkt auf der CLR-Anweisung "Unboxing" Wenn der Typ des Ausdrucks ist `Object`. Im Gegensatz dazu `CType` wird in einen Aufruf an ein Laufzeit-Hilfsprogramm für die Konvertierung erforderlich sind, sodass Konvertierungen zwischen primitiven Typen unterstützt werden können. Im Fall bei einer `Object` Ausdruck konvertiert wird auf einen primitive-Wert-Typ und den Typ der tatsächlichen Instanz Übereinstimmung den Zieltyp `DirectCast` wird erheblich schneller sein als `CType`.)

`TryCast` Konvertiert die Ausdrücke, aber löst keine Ausnahme aus, wenn der Ausdruck in den Zieltyp konvertiert werden kann. Stattdessen `TryCast` führt zu `Nothing` Wenn der Ausdruck zur Laufzeit konvertiert werden kann. (__Beachten.__ Wie bereits erwähnt, `TryCast` direkt auf der CLR-Anweisung "Isinst" zugeordnet. Durch die Kombination der typüberprüfung und die Konvertierung in einen einzelnen Vorgang, `TryCast` möglich günstiger als auch eine `TypeOf ... Is` und dann eine `CType`.)

Zum Beispiel:

```vb
Interface ITest
    Sub Test()
End Interface

Module Test
    Sub Convert(o As Object)
        Dim i As ITest = TryCast(o, ITest)

        If i IsNot Nothing Then
            i.Test()
        End If
    End Sub
End Module
```

Wenn keine Konvertierung vom Typ des Ausdrucks in den angegebenen Typ vorhanden ist, tritt ein Fehler während der Kompilierung. Andernfalls wird der Ausdruck wird als Wert klassifiziert, und das Ergebnis ist der Wert, der von der Konvertierung erstellt.


## <a name="operator-expressions"></a>Operatorausdrücke

Es gibt zwei Arten von Operatoren. *Unäre Operatoren* verwenden einen Operanden aus, und verwenden Sie die Präfixnotation (z. B. `-x`). *Binäre Operatoren* umfassen zwei Operanden, und verwenden Sie die Infix-Notation (z. B. `x + y`). Mit Ausnahme von relationalen Operatoren, die ergeben immer `Boolean`, einen Operator für ein bestimmter Typ dieses Typs führt definiert. Die Operanden zu einem Operator müssen immer als Wert klassifiziert werden; Das Ergebnis eines Ausdrucks Operator wird als Wert klassifiziert.

```antlr
OperatorExpression
    : ArithmeticOperatorExpression
    | RelationalOperatorExpression
    | LikeOperatorExpression
    | ConcatenationOperatorExpression
    | ShortCircuitLogicalOperatorExpression
    | LogicalOperatorExpression
    | ShiftOperatorExpression
    | AwaitOperatorExpression
    ;
```

### <a name="operator-precedence-and-associativity"></a>Operatorrangfolge und Assoziativität

Wenn ein Ausdruck mit mehreren binäre Operatoren, enthält die *Rangfolge* steuert, der Operatoren die Reihenfolge, in dem die einzelnen binären Operatoren ausgewertet werden. Der Ausdruck `x + y * z` wird z.B. als `x + (y * z)` ausgewertet, da der `*`-Operator Vorrang vor dem `+`-Operator hat. In der folgende Tabelle werden die binären Operatoren in absteigender Rangfolge aufgelistet:


| __Kategorie__     | __Operatoren__                                          | 
|------------------|--------------------------------------------------------|
| Primär          | Alle nicht-Operator-Ausdrücke                           |
| Await-            | `Await`                                                |
| Exponentiation   | `^`                                                    |
| Unäre Negation   | `+`, `-`                                               |
| Multiplikativ   | `*`, `/`                                               |
| Ganzzahldivision | `\`                                                    |
| Modulooperator          | `Mod`                                                  |
| Additiv         | `+`, `-`                                               |
| Verkettung    | `&`                                                    |
| Shift            | `<<`, `>>`                                             |
| Relational       | `=`, `<>`, `<`, `>`, `<=`, `>=`, `Like`, `Is`, `IsNot` |
| Logisches NOT      | `Not`                                                  |
| Logisches AND      | `And`, `AndAlso`                                       |
| Logisches OR       | `Or`, `OrElse`                                         |
| Logisches XOR      | `Xor`                                                  |

Wenn ein Ausdruck zwei Operatoren mit gleicher Rangfolge, enthält die *Assoziativität* steuert, der Operatoren die Reihenfolge, in dem die Vorgänge ausgeführt werden. Alle binäre Operatoren sind linksassoziativ, was bedeutet, dass Vorgänge von links nach rechts ausgeführt werden. Rangfolge und Assoziativität können mit der Ausdrücke in Klammern gesteuert werden.

### <a name="object-operands"></a>Objekt Operanden

Zusätzlich zu den regulären Typen, die jeder Operator unterstützt, unterstützt alle Operatoren Operanden vom Typ `Object`. Operatoren angewendet werden, um `Object` Operanden auf ähnliche Weise behandelt werden, um die Methodenaufrufe, die auf `Object` Werte: ein spät gebundenen Methodenaufruf kann ausgewählt werden, in diesem Fall die Gültigkeit der Kompilierzeittyp, anstatt den Laufzeittyp der Operanden bestimmt und der Typ des Vorgangs. Wenn strikte Semantik, durch die kompilierungsumgebung oder durch angegeben werden `Option Strict`, alle Operatoren mit Operanden vom Typ `Object` dazu führen, dass einen Fehler während der Kompilierung mit Ausnahme der `TypeOf...Is`, `Is` und `IsNot` Operatoren.

Operator-Lösung fest, dass es sich bei ein Vorgang spät gebundene ausgeführt werden soll, ist das Ergebnis des Vorgangs das Ergebnis der Anwendung des Operators, der die Operandentypen, wenn die Runtime-Typen der Operanden Typen sind, die durch den Operator unterstützt werden. Der Wert `Nothing` so behandelt, als der Standardwert des Typs von der andere Operand in einem Ausdruck für binäre Operatoren. In einem Ausdruck für unäre Operatoren oder wenn beide Operanden sind `Nothing` in einem binären Operator-Ausdruck, der Typ des Vorgangs ist `Integer` oder der nur Ergebnistyp, der den Operator an, wenn der Operator nicht führt `Integer`. Das Ergebnis des Vorgangs wird immer dann an umgewandelt `Object`. Wenn die Operandentypen kein gültigen Operator, haben eine `System.InvalidCastException` Ausnahme ausgelöst. Konvertierungen zur Laufzeit werden ausgeführt, unabhängig von, ob sie implizite oder explizite befinden.

Wenn das Ergebnis einer numerischen binären Operation eine Überlaufausnahme auslösen (unabhängig davon, ob Überprüfungen auf Ganzzahlüberlauf aktiviert oder deaktiviert ist) erzeugt wird, wird der Ergebnistyp nach Möglichkeit auf den nächsten größeren numerischen Typ heraufgestuft. Beachten Sie z. B. folgenden Code:

```vb
Module Test
    Sub Main()
        Dim o As Object = CObj(CByte(2)) * CObj(CByte(255))

        Console.WriteLine(o.GetType().ToString() & " = " & o)
    End Sub
End Module
```

Gibt das folgende Ergebnis:

```
System.Int16 = 512
```

Wenn keine größeren numerischer Typ verfügbar ist, für die Anzahl, ist eine `System.OverflowException` Ausnahme ausgelöst.

### <a name="operator-resolution"></a>Operator-Lösung

Wenn ein Operatortyp und einen Satz von Operanden, bestimmt Operator Auflösung, welche Operators, der für den Operanden. Beim Auflösen von Operatoren werden benutzerdefinierte Operatoren zuerst mit den folgenden Schritten berücksichtigt werden:

1. Zunächst werden alle möglichen Operatoren gesammelt. Die Candidate-Operatoren sind alle benutzerdefinierten Operatoren die bestimmten Operator-Typs in den Quelltyp und alle benutzerdefinierten Operatoren des bestimmten Typs in den Zieltyp. Wenn die Quell- und Zieltyp verknüpft sind, werden allgemeine Operatoren nur einmal berücksichtigt.

2. Anschließend wird die Auflösung von funktionsüberladungen, die Operatoren und Operanden, um das spezifischste Operator auszuwählen angewendet. Im Fall von binären Operatoren kann dies zu einer spät gebundenen Aufruf führen.

Wenn die Candidate-Operatoren für einen Typ sammeln `T?`, die Operatoren des Typs `T` stattdessen verwendet. Einer der `T`des benutzerdefinierten Operatoren, bei denen nur die nicht auf NULL festlegbare Werttypen werden auch aufgehoben. Ein Operator transformierten verwendet die NULL-Werte zulässt Version des Werttypen an, mit der Ausnahme die Rückgabetypen der `IsTrue` und `IsFalse` (muss `Boolean`). Transformierten Operatoren werden ausgewertet, durch die Konvertierung von Operanden in ihre Version ist keine NULL-Werte zulässt, evaluieren den benutzerdefinierten Operator, und klicken Sie dann das Ergebnis zu konvertieren und geben Sie dann auf die Version der NULL-Werte zulässt. Wenn dieses Operand ist `Nothing`, das Ergebnis des Ausdrucks ist ein Wert von `Nothing` als der auf NULL festlegbare Version des Ergebnistyps typisiert ist. Zum Beispiel:

```vb
Structure T
    ...
End Structure

Structure S
    Public Shared Operator +(ByVal op1 As S, ByVal op2 As T) As T
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim x As S?
        Dim y, z As T?

        ' Valid, as S + T = T is lifted to S? + T? = T?
        z = x + y 
    End Sub
End Module
```

Wenn der Operator ein binärer Operator ist und einer der Operanden Verweistyp ist, auch der Operator angehoben wird, aber eine Bindung an den Operator erzeugt einen Fehler. Zum Beispiel:

```vb
Structure S1
    Public F1 As Integer

    Public Shared Operator +(left As S1, right As String) As S1
       ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim a? As S1
        Dim s As String
        
        ' Error: '+' is not defined for S1? and String
        a = a + s
    End Sub
End Module
```

__Beachten Sie.__ Diese Regel vorhanden ist, da gab es berücksichtigt, ob wir möchten Verweistypen Null-Weitergabe in einer zukünftigen Version wird in der Groß-/Kleinschreibung des Verhaltens im Fall von binären Operatoren zwischen den beiden Typen ändern würde.

Wie bei Konvertierungen werden benutzerdefinierte Operatoren, immer über transformierten Operatoren bevorzugt.

Beim Auflösen von Operatoren überladen werden, möglicherweise gibt es Unterschiede zwischen Klassen, die in Visual Basic definiert und in anderen Sprachen definiert:

* In anderen Sprachen `Not`, `And`, und `Or` kann sowohl als logische und bitweise Operatoren überladen werden. Nach dem Importieren aus einer externen Assembly ist eine der Formen als gültige Überladung für diese Operatoren akzeptiert. Allerdings wird für einen Typ, der logische und bitweise Operatoren definiert, nur die bitweise Implementierung berücksichtigt werden.

* In anderen Sprachen `>>` und `<<` kann sowohl als mit und ohne Vorzeichen Operatoren überladen werden. Nach dem Importieren aus einer externen Assembly wird entweder Formular als gültige Überladung akzeptiert. Für einen Typ, der mit und ohne Vorzeichen Operatoren definiert: wird jedoch nur die signierte Implementierung berücksichtigt werden.

* Wenn keine benutzerdefinierten Operator auf die Operanden am spezifischsten ist, werden dann systeminterne Operatoren betrachtet. Wenn keine systeminternen Operators aus der für den Operanden definiert ist, und entweder Operand vom Typ Objekt ist wird dann der Operator spät gebundene aufgelöst wird. Andernfalls führt ein Fehler während der Kompilierung.

In früheren Versionen von Visual Basic Wenn es genau ein Operand vom Typ "Object" und keine anwendbaren benutzerdefinierte Operatoren und keine anwendbaren systeminternen Operatoren wurde, war es ein Fehler. Ab Visual Basic-11 ist es jetzt spät gebundene behoben. Zum Beispiel:

```vb
Module Module1
  Sub Main()
      Dim p As Object = Nothing
      Dim U As New Uri("http://www.microsoft.com")
      Dim j = U * p  ' is now resolved late-bound
   End Sub
End Module
```

Ein Typ `T` , bei dem eine systeminterne Funktion Operator definiert auch dieser Operator für `T?`. Das Ergebnis des Operators für `T?` werden dieselbe wie für `T`, mit dem Unterschied, dass wenn ein Operand `Nothing`, das Ergebnis des Operators `Nothing` (d. h. der null-Wert wird weitergegeben). Im Rahmen der Auflösen des Typs eines Vorgangs der `?` wird entfernt, der Typ des Vorgangs über alle Operanden, die sie haben, wird bestimmt, und ein `?` wird in den Typ des Vorgangs hinzugefügt, wenn einer der Operanden auf NULL festlegbare Werttypen wurden. Zum Beispiel:

```vb
Dim v1? As Integer = 10
Dim v2 As Long = 20

' Type of operation will be Long?
Console.WriteLine(v1 + v2)
```

Jeder Operator werden die systeminternen Typen, die, denen Sie für definiert ist, und den Typ des ausgeführten Vorgangs die Operandentypen aufgeführt. Das Ergebnis des Typs eines systeminterne Vorgangs: folgende allgemeine Regeln

* Wenn alle Operanden vom selben Typ sind und der Operator für den Typ definiert ist, erfolgt keine Konvertierung, und der Operator für diesen Typ wird verwendet.

* Alle Operanden, dessen Typ nicht für den Operator definiert ist, wird konvertiert mithilfe der folgenden Schritte aus, und der Operator ist für die neuen Typen aufgelöst:

  * Der Operand wird konvertiert, zum nächsten allgemeinsten Typ, der für den Operanden und den Operator definiert ist und bei denen es implizit konvertiert werden.

  * Wenn kein solcher Typ vorhanden ist, wird der Operand den Typ des nächsten engsten, der für den Operanden und den Operator definiert ist und bei denen es implizit konvertiert.

  * Wenn kein solcher Typ vorhanden ist, oder die Konvertierung kann nicht durchgeführt, tritt ein Fehler während der Kompilierung.

* Andernfalls werden die Operanden konvertiert, auf das breiter die Operandentypen und den Operator für diesen Typ wird verwendet. Wenn der engeren Operandentyp in größeren Operatortyp implizit konvertiert werden kann, tritt auf, ein Fehler während der Kompilierung.

Trotz dieser allgemeinen Regeln gibt es jedoch eine Anzahl von Sonderfälle in den Ergebnistabellen Operator aufgerufen.

__Beachten Sie.__ Formatierungsgründen, abkürzen der Operator den Tabellen des vordefinierten Namen, den ersten beiden Zeichen. Wird also "von" `Byte`, ist der "UI" `UInteger`, ist "St" `String`usw. "Err" bedeutet, die dass es keinen Vorgang für die angegebene Operandentypen definiert ist.

## <a name="arithmetic-operators"></a>Arithmetische Operatoren

Die `*`, `/`, `\`, `^`, `Mod`, `+`, und `-` Operatoren sind die *arithmetische Operatoren*.

```antlr
ArithmeticOperatorExpression
    : UnaryPlusExpression
    | UnaryMinusExpression
    | AdditionOperatorExpression
    | SubtractionOperatorExpression
    | MultiplicationOperatorExpression
    | DivisionOperatorExpression
    | ModuloOperatorExpression
    | ExponentOperatorExpression
    ;
```

Arithmetische Operationen mit Gleitkommazahlen können mit einer höheren Genauigkeit als der Ergebnistyp des Vorgangs ausgeführt werden. Beispielsweise unterstützen einige Hardwarearchitekturen einen "erweiterten" oder "long double" vom Typ Gleitkommazahlen mit größeren Bereich und Genauigkeit als den `Double` geben, und führen Sie alle Operationen mit Gleitkommazahlen mit diesem Typ mit höherer Genauigkeit implizit. Hardware-Architekturen können zum Ausführen von Operations mit Gleitkommazahlen mit geringerer Genauigkeit nur eine übermäßige Kosten in Bezug auf Leistung vorgenommen werden; anstatt eine Implementierung, die in Anspruch genommenen Leistung und Genauigkeit zu benötigen, ermöglicht Visual Basic den Typ mit höherer Genauigkeit, die für alle Operationen mit Gleitkommazahlen verwendet werden. Als die Bereitstellung eine genauere Ergebnisse, hat dies nur selten keine messbaren Auswirkungen. In Ausdrücken des Formulars `x * y / z`, in denen die Multiplikation für ein Ergebnis erzeugt, die außerhalb der `Double` Bereich, aber die nachfolgende Division wird das temporäre Ergebnis wieder in die `Double` liegen, die Tatsache, dass der Ausdruck in einem höheren Bereich ausgewertet Format kann dazu führen, dass eine endliche Ergebnis unendlich erstellt werden.


### <a name="unary-plus-operator"></a>Unärer Plus-Operator

```antlr
UnaryPlusExpression
    : '+' Expression
    ;
```

Der unäre plus -Operator ist definiert, für die `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Single`, `Double`, und `Decimal` Typen .

__Vorgangstyp:__


| __BO__ | __SB__ | __Durch__ | __Sh__ | __USA__ | __In__ | __BENUTZEROBERFLÄCHE__ | __Lo__ | __UL__ | __de__ | __SI__ | __Do__ | __Da__  | __CH__  | __St__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| Sh | SB | um | Sh | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do | Ob | 


### <a name="unary-minus-operator"></a>Unäres Minus-Operator

```antlr
UnaryMinusExpression
    : '-' Expression
    ;
```

Der unäre Minusoperator ist für die folgenden Typen definiert:

`SByte`, `Short`, `Integer`und `Long`. Das Ergebnis wird durch Subtrahieren der Operand von 0 (null) berechnet. Wenn Überprüfungen auf Ganzzahlüberlauf eingeschaltet und der Wert des Operanden die maximale Negative ist `SByte`, `Short`, `Integer`, oder `Long`, `System.OverflowException` Ausnahme ausgelöst. Wenn der Wert des Operanden um die maximale negativ ist, andernfalls `SByte`, `Short`, `Integer`, oder `Long`, das Ergebnis ist dieser Wert und der Überlauf wird nicht gemeldet.

`Single` und `Double`. Das Ergebnis ist der Wert des Operanden mit umgekehrtem Vorzeichen umkehren, einschließlich der Werte 0 und unendlich. Wenn der Operand NaN ist, ist das Ergebnis auch NaN.

`Decimal`. Das Ergebnis wird durch Subtrahieren der Operand von 0 (null) berechnet.

__Vorgangstyp:__

| __BO__ | __SB__ | __Durch__ | __Sh__ | __USA__ | __In__ | __BENUTZEROBERFLÄCHE__ | __Lo__ | __UL__ | __de__ | __SI__ | __Do__ | __Da__  | __CH__  | __St__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| Sh | SB | Sh | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do | Ob | 


### <a name="addition-operator"></a>Addition-Operator

Der Additionsoperator berechnet die Summe der beiden Operanden.

```antlr
AdditionOperatorExpression
    : Expression '+' LineTerminator? Expression
    ;
```

Der Additionsoperator ist für die folgenden Typen definiert:

* `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` und `Long`. Wenn Überprüfungen auf Ganzzahlüberlauf auf und die Summe außerhalb des Bereichs von den Ergebnistyp ist einer `System.OverflowException` Ausnahme ausgelöst. Anderenfalls Überläufe werden nicht gemeldet, und alle signifikanten höherwertigen Bits des Ergebnisses werden verworfen.

* `Single` und `Double`. Die Summe wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.

* `Decimal`. Wenn der resultierende Wert für die Darstellung im decimal-Format zu groß ist eine `System.OverflowException` Ausnahme ausgelöst. Wenn der Ergebniswert zur Darstellung im Dezimalformat zu klein ist, ist das Ergebnis 0.

* `String`. Die beiden `String` Operanden miteinander verkettet.

* `Date`. Die `System.DateTime` Typ definiert überladenen Addition-Operatoren. Da `System.DateTime` entspricht, auf die systeminternen `Date` geben, diese Operatoren steht auch auf die `Date` Typ.

__Vorgangstyp:__

|        | __BO__ | __SB__ | __Durch__ | __Sh__ | __USA__ | __In__ | __BENUTZEROBERFLÄCHE__ | __Lo__ | __UL__ | __de__ | __SI__ | __Do__ | __Da__  | __CH__  | __St__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| __BO__ | Sh | SB | Sh | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __SB__ |    | SB | Sh | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __Durch__ |    |    | um | Sh | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do | Ob | 
| __Sh__ |    |    |    | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __USA__ |    |    |    |    | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do | Ob | 
| __In__ |    |    |    |    |    | In | Lo | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __BENUTZEROBERFLÄCHE__ |    |    |    |    |    |    | UI | Lo | UL | De | Si | Do | Err | Err | Do | Ob | 
| __Lo__ |    |    |    |    |    |    |    | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __UL__ |    |    |    |    |    |    |    |    | UL | De | Si | Do | Err | Err | Do | Ob | 
| __de__ |    |    |    |    |    |    |    |    |    | De | Si | Do | Err | Err | Do | Ob | 
| __SI__ |    |    |    |    |    |    |    |    |    |    | Si | Do | Err | Err | Do | Ob | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do | Ob | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | St  | Err | St | Ob | 
| __CH__ |    |    |    |    |    |    |    |    |    |    |    |    |     | St  | St | Ob | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | St | Ob | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | Ob | 


### <a name="subtraction-operator"></a>Subtraktionsoperator

Der Subtraktionsoperator subtrahiert den zweiten Operanden vom ersten Operanden.

```antlr
SubtractionOperatorExpression
    : Expression '-' LineTerminator? Expression
    ;
```

Der Subtraktionsoperator ist für die folgenden Typen definiert:

* `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` und `Long`. Wenn Überprüfungen auf Ganzzahlüberlauf eingeschaltet und der Unterschied, außerhalb des Bereichs von den Ergebnistyp ist einer `System.OverflowException` Ausnahme ausgelöst. Anderenfalls Überläufe werden nicht gemeldet, und alle signifikanten höherwertigen Bits des Ergebnisses werden verworfen.

* `Single` und `Double`. Der Unterschied wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.

* `Decimal`. Wenn der resultierende Wert für die Darstellung im decimal-Format zu groß ist eine `System.OverflowException` Ausnahme ausgelöst. Wenn der Ergebniswert zur Darstellung im Dezimalformat zu klein ist, ist das Ergebnis 0.

* `Date`. Die `System.DateTime` Typ definiert überladene Subtraktionsoperatoren. Da `System.DateTime` entspricht, auf die systeminternen `Date` geben, diese Operatoren steht auch auf die `Date` Typ.

__Vorgangstyp:__

|        | __BO__ | __SB__ | __Durch__ | __Sh__ | __USA__ | __In__ | __BENUTZEROBERFLÄCHE__ | __Lo__ | __UL__ | __de__ | __SI__ | __Do__ | __Da__  | __CH__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __BO__ | Sh | SB | Sh | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __SB__ |    | SB | Sh | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __Durch__ |    |    | um | Sh | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __Sh__ |    |    |    | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __USA__ |    |    |    |    | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __In__ |    |    |    |    |    | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __BENUTZEROBERFLÄCHE__ |    |    |    |    |    |    | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __de__ |    |    |    |    |    |    |    |    |    | De | Si | Do | Err | Err | Do  | Ob  | 
| __SI__ |    |    |    |    |    |    |    |    |    |    | Si | Do | Err | Err | Do  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do  | Ob  | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __CH__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Do  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="multiplication-operator"></a>Multiplikationsoperator

Der Multiplikationsoperator berechnet das Produkt der beiden Operanden.

```antlr
MultiplicationOperatorExpression
    : Expression '*' LineTerminator? Expression
    ;
```

Der Multiplikationsoperator ist für die folgenden Typen definiert:

* `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` und `Long`. Wenn die Überprüfungen auf Ganzzahlüberlauf ist, und das Produkt ist außerhalb des Bereichs des Ergebnistyps, eine `System.OverflowException` Ausnahme ausgelöst. Anderenfalls Überläufe werden nicht gemeldet, und alle signifikanten höherwertigen Bits des Ergebnisses werden verworfen.

* `Single` und `Double`. Das Produkt wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.

* `Decimal`. Wenn der resultierende Wert für die Darstellung im decimal-Format zu groß ist eine `System.OverflowException` Ausnahme ausgelöst. Wenn der Ergebniswert zur Darstellung im Dezimalformat zu klein ist, ist das Ergebnis 0.

__Vorgangstyp:__

|        | __BO__ | __SB__ | __Durch__ | __Sh__ | __USA__ | __In__ | __BENUTZEROBERFLÄCHE__ | __Lo__ | __UL__ | __de__ | __SI__ | __Do__ | __Da__  | __CH__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __BO__ | Sh | SB | Sh | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __SB__ |    | SB | Sh | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __Durch__ |    |    | um | Sh | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __Sh__ |    |    |    | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __USA__ |    |    |    |    | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __In__ |    |    |    |    |    | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __BENUTZEROBERFLÄCHE__ |    |    |    |    |    |    | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __de__ |    |    |    |    |    |    |    |    |    | De | Si | Do | Err | Err | Do  | Ob  | 
| __SI__ |    |    |    |    |    |    |    |    |    |    | Si | Do | Err | Err | Do  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do  | Ob  | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __CH__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Do  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="division-operators"></a>Divisionsoperator

Divisionsoperator berechnet den Quotienten aus zwei Operanden. Es gibt zwei Divisionsoperatoren: der reguläre (Gleitkomma-) Divisionsoperator und der Ganzzahl-Divisionsoperator.

```antlr
DivisionOperatorExpression
    : FPDivisionOperatorExpression
    | IntegerDivisionOperatorExpression
    ;

FPDivisionOperatorExpression
    : Expression '/' LineTerminator? Expression
    ;

IntegerDivisionOperatorExpression
    : Expression '\\' LineTerminator? Expression
    ;
```

Der reguläre Divisionsoperator ist für die folgenden Typen definiert:

* `Single` und `Double`. Der Quotient ist gemäß den Regeln der IEEE 754-Arithmetik berechnet.

* `Decimal`. Wenn der Wert des rechten Operanden NULL ist, ist eine `System.DivideByZeroException` Ausnahme ausgelöst. Wenn der resultierende Wert für die Darstellung im decimal-Format zu groß ist eine `System.OverflowException` Ausnahme ausgelöst. Wenn der Ergebniswert zur Darstellung im Dezimalformat zu klein ist, ist das Ergebnis 0 (null). Die Dezimalstellen des Ergebnisses wird vor der Rundung, sind keine Dezimalstellen am nächsten an die bevorzugten skalierungsgruppen die ein Ergebnis wird auf das genaue Ergebnis zu erhalten.  Die bevorzugte Skalierung sind keine Dezimalstellen von der erste Operand weniger die Skalierung des zweiten Operanden.

Gemäß den Regeln für normale Operator Auflösung, reguläre Division ausschließlich zwischen den Operanden der Datentypen, z. B. `Byte`, `Short`, `Integer`, und `Long` würde dazu führen, dass beide Operanden in den Typ konvertiert werden `Decimal`. Operator-namensauflösung in der Divisionsoperator jedoch bei Aktionen, wenn kein Typ ist `Decimal`, `Double` gilt schmaler als `Decimal`. Diese Konvention eingehalten wird, da `Double` Division ist effizienter als `Decimal` Division.

__Vorgangstyp:__

|        | __BO__ | __SB__ | __Durch__ | __Sh__ | __USA__ | __In__ | __BENUTZEROBERFLÄCHE__ | __Lo__ | __UL__ | __de__ | __SI__ | __Do__ | __Da__  | __CH__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __BO__ | Do | Do | Do | Do | Do | Do | Do | Do | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __SB__ |    | Do | Do | Do | Do | Do | Do | Do | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __Durch__ |    |    | Do | Do | Do | Do | Do | Do | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __Sh__ |    |    |    | Do | Do | Do | Do | Do | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __USA__ |    |    |    |    | Do | Do | Do | Do | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __In__ |    |    |    |    |    | Do | Do | Do | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __BENUTZEROBERFLÄCHE__ |    |    |    |    |    |    | Do | Do | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Do | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __de__ |    |    |    |    |    |    |    |    |    | De | Si | Do | Err | Err | Do  | Ob  | 
| __SI__ |    |    |    |    |    |    |    |    |    |    | Si | Do | Err | Err | Do  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do  | Ob  | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __CH__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Do  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 

Der Ganzzahl-Divisionsoperator ist für definiert `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, und `Long`. Wenn der Wert des rechten Operanden NULL ist, ist eine `System.DivideByZeroException` Ausnahme ausgelöst. Die Division rundet das Ergebnis in Richtung 0 (null), und der Absolute Wert des Ergebnisses wird die größte ganze Zahl möglich, die kleiner als der Absolute Wert des Quotienten aus zwei Operanden ist. Das Ergebnis ist 0 (null) oder positiv, wenn die beiden Operanden dasselbe Zeichen, und 0 (null) oder negativ, wenn die beiden Operanden entgegengesetzten signiert haben. Der linke Operand ist der maximale Negative `SByte`, `Short`, `Integer`, oder `Long`, und der Rechte Operand ist `-1`, ein Überlauf auftritt, wenn Überprüfungen auf Ganzzahlüberlauf on ist, eine `System.OverflowException` Ausnahme ausgelöst. Andernfalls der Überlauf wird nicht gemeldet, und das Ergebnis wird stattdessen der Wert des linken Operanden.

__Beachten Sie.__ Wenn die beiden Operanden für die Typen ohne Vorzeichen ist immer 0 (null), oder positiv ist, das Ergebnis immer 0 (null) oder positiv ist.  Da das Ergebnis des Ausdrucks immer kleiner als oder gleich der größten der beiden Operanden sind, ist es nicht möglich, ein Überlauf auftreten.  Überprüfungen auf Ganzzahlüberlauf wird daher für die Ganzzahldivision mit zwei Ganzzahlen ohne Vorzeichen nicht ausgeführt. Das Ergebnis ist der Typ des linken Operanden.


__Vorgangstyp:__

|        | __BO__ | __SB__ | __Durch__ | __Sh__ | __USA__ | __In__ | __BENUTZEROBERFLÄCHE__ | __Lo__ | __UL__ | __de__ | __SI__ | __Do__ | __Da__  | __CH__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __BO__ | Sh | SB | Sh | Sh | In | In | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __SB__ |    | SB | Sh | Sh | In | In | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Durch__ |    |    | um | Sh | US | In | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Sh__ |    |    |    | Sh | In | In | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __USA__ |    |    |    |    | US | In | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __In__ |    |    |    |    |    | In | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __BENUTZEROBERFLÄCHE__ |    |    |    |    |    |    | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __de__ |    |    |    |    |    |    |    |    |    | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __SI__ |    |    |    |    |    |    |    |    |    |    | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Lo | Err | Err | Lo  | Ob  | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __CH__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Lo  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="mod-operator"></a>Operator Mod

Die `Mod` (modulo)-Operator berechnet den Rest der Division zwischen zwei Operanden.

```antlr
ModuloOperatorExpression
    : Expression 'Mod' LineTerminator? Expression
    ;
```

Die `Mod` Operator ist für die folgenden Typen definiert:

* `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` und `Long`. Das Ergebnis des `x Mod y` wird durch der Wert erzeugt `x - (x \ y) * y`. Wenn `y` ist 0 (null), eine `System.DivideByZeroException` Ausnahme ausgelöst. Der modulo-Operator, löst nie einen Überlauf.

* `Single` und `Double`. Im weiteren Verlauf wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.

* `Decimal`. Wenn der Wert des rechten Operanden NULL ist, ist eine `System.DivideByZeroException` Ausnahme ausgelöst. Wenn der resultierende Wert für die Darstellung im decimal-Format zu groß ist eine `System.OverflowException` Ausnahme ausgelöst. Wenn der Ergebniswert zur Darstellung im Dezimalformat zu klein ist, ist das Ergebnis 0 (null).

__Vorgangstyp:__

|        | __BO__ | __SB__ | __Durch__ | __Sh__ | __USA__ | __In__ | __BENUTZEROBERFLÄCHE__ | __Lo__ | __UL__ | __de__ | __SI__ | __Do__ | __Da__  | __CH__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __BO__ | Sh | SB | Sh | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __SB__ |    | SB | Sh | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __Durch__ |    |    | um | Sh | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __Sh__ |    |    |    | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __USA__ |    |    |    |    | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __In__ |    |    |    |    |    | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __BENUTZEROBERFLÄCHE__ |    |    |    |    |    |    | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __de__ |    |    |    |    |    |    |    |    |    | De | Si | Do | Err | Err | Do  | Ob  | 
| __SI__ |    |    |    |    |    |    |    |    |    |    | Si | Do | Err | Err | Do  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do  | Ob  | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __CH__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Do  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="exponentiation-operator"></a>Exponentialoperator

Der Potenzierungsoperator berechnet der erste Operand, der die Potenz des zweiten Operanden.

```antlr
ExponentOperatorExpression
    : Expression '^' LineTerminator? Expression
    ;
```

Der Potenzierungsoperator ist für den Typ definiert `Double`. Der Wert wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.

__Vorgangstyp:__

|        | __BO__ | __SB__ | __Durch__ | __Sh__ | __USA__ | __In__ | __BENUTZEROBERFLÄCHE__ | __Lo__ | __UL__ | __de__ | __SI__ | __Do__ | __Da__  | __CH__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __BO__ | Do | Do | Do | Do | Do | Do | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __SB__ |    | Do | Do | Do | Do | Do | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __Durch__ |    |    | Do | Do | Do | Do | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __Sh__ |    |    |    | Do | Do | Do | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __USA__ |    |    |    |    | Do | Do | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __In__ |    |    |    |    |    | Do | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __BENUTZEROBERFLÄCHE__ |    |    |    |    |    |    | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Do | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __de__ |    |    |    |    |    |    |    |    |    | Do | Do | Do | Err | Err | Do  | Ob  | 
| __SI__ |    |    |    |    |    |    |    |    |    |    | Do | Do | Err | Err | Do  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do  | Ob  | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __CH__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Do  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


## <a name="relational-operators"></a>Operatoren (relational)

Die *relationalen Operatoren* Werte miteinander verglichen werden soll. Die Vergleichsoperatoren `=`, `<>`, `<`, `>`, `<=`, und `>=`.

```antlr
RelationalOperatorExpression
    : Expression '=' LineTerminator? Expression
    | Expression '<' '>' LineTerminator? Expression
    | Expression '<' LineTerminator? Expression
    | Expression '>' LineTerminator? Expression
    | Expression '<' '=' LineTerminator? Expression
    | Expression '>' '=' LineTerminator? Expression
    ;
```

Alle relationalen Operatoren führen eine `Boolean` Wert.

Die relationalen Operatoren haben folgende Bedeutung: Allgemeine:

* Die `=` -Operator testet, ob, ob die beiden Operanden gleich sind.

* Die `<>` -Operator testet, ob, ob die beiden Operanden nicht gleich sind.

* Die `<` -Operator testet, ob der erste Operand kleiner als der zweite Operand.

* Die `>` -Operator testet, ob, ob der erste Operand größer als der zweite Operand ist.

* Die `<=` -Operator testet, ob, ob der erste Operand kleiner als oder gleich der zweite Operand ist.

* Die `>=` -Operator testet, ob, ob der erste Operand größer als oder gleich dem zweiten Operand ist.

Die relationalen Operatoren werden für die folgenden Typen definiert:

* `Boolean`. Die Operatoren vergleichen Sie die Wahrheit-Werte der beiden Operanden. `True` gilt als kleiner als `False`, der mit den entsprechenden numerischen Werten entspricht.

* `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` und `Long`. Die Operatoren vergleichen die numerischen Werte der beiden ganzzahligen Operanden.

* `Single` und `Double`. Die Operatoren vergleichen die Operanden gemäß den Regeln der IEEE 754-standard.

* `Decimal`. Die Operatoren vergleichen die numerischen Werte der beiden Operanden decimal.

* `Date`. Die Operatoren geben das Ergebnis des Vergleichs die zwei Datum/Uhrzeit-Werte zurück.

* `Char`. Die Operatoren geben das Ergebnis des Vergleichs der zwei Unicode-Werte zurück.

* `String`. Die Operatoren geben das Ergebnis des Vergleichs die beiden Werte, die über einen binären Vergleich oder ein Textvergleich zurück. Der Vergleich verwendet, richtet sich nach der kompilierungsumgebung und die `Option Compare` Anweisung. Ein binärer Vergleich bestimmt, ob es sich bei der numerische Unicode-Wert jedes Zeichens in jeder Zeichenfolge übereinstimmt. Ein Textvergleich ist einen Unicode-Vergleich, Text basierend auf der aktuellen Kultur verwendet, die auf .NET Framework. Bei einem Zeichenfolgenvergleich, ein null-Wert entspricht der literalen Zeichenfolge `""`.

__Vorgangstyp:__
        
|        | __BO__ | __SB__ | __Durch__ | __Sh__ | __USA__ | __In__ | __BENUTZEROBERFLÄCHE__ | __Lo__ | __UL__ | __de__ | __SI__ | __Do__ | __Da__  | __CH__  | __St__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| __BO__ | BO | SB | Sh | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | BO | Ob | 
| __SB__ |    | SB | Sh | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __Durch__ |    |    | um | Sh | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do | Ob | 
| __Sh__ |    |    |    | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __USA__ |    |    |    |    | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do | Ob | 
| __In__ |    |    |    |    |    | In | Lo | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __BENUTZEROBERFLÄCHE__ |    |    |    |    |    |    | UI | Lo | UL | De | Si | Do | Err | Err | Do | Ob | 
| __Lo__ |    |    |    |    |    |    |    | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __UL__ |    |    |    |    |    |    |    |    | UL | De | Si | Do | Err | Err | Do | Ob | 
| __de__ |    |    |    |    |    |    |    |    |    | De | Si | Do | Err | Err | Do | Ob | 
| __SI__ |    |    |    |    |    |    |    |    |    |    | Si | Do | Err | Err | Do | Ob | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do | Ob | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | Da  | Err | Da | Ob | 
| __CH__ |    |    |    |    |    |    |    |    |    |    |    |    |     | CH  | St | Ob | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | St | Ob | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | Ob | 


## <a name="like-operator"></a>Like-Operator

Die `Like` Operator bestimmt, ob eine Zeichenfolge mit einem angegebenes Muster übereinstimmt.

```antlr
LikeOperatorExpression
    : Expression 'Like' LineTerminator? Expression
    ;
```

Die `Like` Operator ist definiert, für die `String` Typ. Der erste Operand wird die Zeichenfolge abgeglichen wird, und der zweite Operand ist das Muster für den Abgleich. Das Muster besteht aus Unicode-Zeichen. Die folgende Sequenz von Zeichen haben eine besondere Bedeutung:

* Das Zeichen `?` entspricht einem beliebigen einzelnes Zeichen.

* Das Zeichen `*` entspricht null oder mehr Zeichen.

* Das Zeichen `#` entspricht eine beliebige einzelne Ziffer (0-9).

* Eine Liste von Zeichen, die in Klammern eingeschlossen (`[ab...]`) entspricht einem beliebigen einzelnes Zeichen in der Liste.

* Eine Liste von Zeichen von Klammern umgeben, und das Präfix durch ein Ausrufezeichen (`[!ab...]`) entspricht einem beliebigen einzelnes Zeichen nicht in der Zeichenliste.

* Zwei Zeichen in einer Zeichenliste durch einen Bindestrich getrennt (`-`) Geben Sie einen Bereich von Unicode-Zeichen, mit dem ersten Zeichen beginnt und endet mit dem zweiten Zeichen. Wenn das zweite Zeichen nicht höher als das erste Zeichen in der Sortierreihenfolge ist, tritt eine Laufzeitausnahme. Ein Bindestrich, der am Anfang oder Ende einer Zeichenliste wird angezeigt, gibt selbst.

Entsprechend Sonderzeichen für die öffnende Klammer (`[`), Fragezeichen (`?`), Nummernzeichen (`#`), und das Sternchen (`*`), eckige Klammern eingeschlossen werden müssen. Die Rechte eckige Klammer (`]`) kann nicht innerhalb einer Gruppe verwendet werden, entsprechend selbst, aber es kann als ein einzelnes Zeichen außerhalb einer Gruppe verwendet werden. Die Zeichensequenz `[]` gilt das Zeichenfolgenliteral `""`. 

Beachten Sie, dass Zeichenvergleiche und Sortierung für Zeichenlisten abhängig von dem Typ von Vergleichen verwendet wird. Wenn die binäre Vergleiche verwendet werden, basieren Zeichenvergleiche und Sortierung auf den numerischen Unicode-Werten. Wenn Textvergleiche verwendet werden, basieren Zeichenvergleiche und Sortierung auf dem aktuellen Gebietsschema, das auf .NET Framework verwendet wird.

In einigen Sprachen können Sonderzeichen in der Alphabet stellen zwei separate Zeichen und umgekehrt. Mehrere Sprachen verwenden Sie z. B. das Zeichen `æ` zur Darstellung der Zeichen `a` und `e` Wenn sie angezeigt werden, während die Zeichen `^` und `O` können verwendet werden, um die Darstellung des Zeichens `Ô`. Wenn Textvergleiche, die `Like` Operator erkennt diese kulturellen Äquivalenzen. In diesem Fall entspricht ein Vorkommen der einzelnen speziellen Zeichen in einem Muster oder Zeichenfolge das entsprechende zwei Zeichen bestehende Folge in die andere Zeichenfolge. Auf ähnliche Weise ein einzelnes spezielle Zeichen im Muster in Klammern eingeschlossen (allein in einer Liste oder in einem Bereich) entspricht der entspricht zwei Zeichen bestehende Folge in der Zeichenfolge und umgekehrt.

In einem `Like` Ausdruck, in denen beide Operanden sind `Nothing` oder einer der Operanden eine Konvertierung zu `String` und der andere Operand `Nothing`, `Nothing` wird behandelt, als handele es sich um ein literal für die leere Zeichenfolge `""`.

__Vorgangstyp:__

|        | __BO__ | __SB__ | __Durch__ | __Sh__ | __USA__ | __In__ | __BENUTZEROBERFLÄCHE__ | __Lo__ | __UL__ | __de__ | __SI__ | __Do__ | __Da__  | __CH__  | __St__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| __BO__ | St | St | St | St | St | St | St | St | St | St | St | St | St | St | St | Ob | 
| __SB__ |    | St | St | St | St | St | St | St | St | St | St | St | St | St | St | Ob | 
| __Durch__ |    |    | St | St | St | St | St | St | St | St | St | St | St | St | St | Ob | 
| __Sh__ |    |    |    | St | St | St | St | St | St | St | St | St | St | St | St | Ob | 
| __USA__ |    |    |    |    | St | St | St | St | St | St | St | St | St | St | St | Ob | 
| __In__ |    |    |    |    |    | St | St | St | St | St | St | St | St | St | St | Ob | 
| __BENUTZEROBERFLÄCHE__ |    |    |    |    |    |    | St | St | St | St | St | St | St | St | St | Ob | 
| __Lo__ |    |    |    |    |    |    |    | St | St | St | St | St | St | St | St | Ob | 
| __UL__ |    |    |    |    |    |    |    |    | St | St | St | St | St | St | St | Ob | 
| __de__ |    |    |    |    |    |    |    |    |    | St | St | St | St | St | St | Ob | 
| __SI__ |    |    |    |    |    |    |    |    |    |    | St | St | St | St | St | Ob | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | St | St | St | St | Ob | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | St | St | St | Ob | 
| __CH__ |    |    |    |    |    |    |    |    |    |    |    |    |    | St | St | Ob | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    | St | Ob | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | Ob | 


## <a name="concatenation-operator"></a>Operator für zeichenfolgenverkettung

```antlr
ConcatenationOperatorExpression
    : Expression '&' LineTerminator? Expression
    ;
```

Die *Verkettungsoperator* für alle systeminternen Typen, einschließlich der auf NULL festlegbare Versionen der systeminterne Werttypen definiert ist. Es wird auch für die Verkettung zwischen den oben genannten Typen definiert und `System.DBNull`, die so behandelt, als eine `Nothing` Zeichenfolge. Der Operator für Verkettungen konvertiert alle Operanden um `String`; im Ausdruck alle Konvertierungen in `String` erweiternde werden, unabhängig davon, ob die strikte Semantik verwendet werden, gelten. Ein `System.DBNull` konvertierte Wert wird das Literal `Nothing` als `String`. Ein NULL-Wert, dessen Wert `Nothing` ist auch in das Literal konvertiert `Nothing` als `String`, anstatt einen Laufzeitfehler auszulösen.

Eine Verkettungsoperation führt zu einer Zeichenfolge, die die Verkettung der beiden Operanden in der Reihenfolge von links nach rechts ist. Der Wert `Nothing` wird behandelt, als handele es sich um ein literal für die leere Zeichenfolge `""`.

__Vorgangstyp:__

|        | __BO__ | __SB__ | __Durch__ | __Sh__ | __USA__ | __In__ | __BENUTZEROBERFLÄCHE__ | __Lo__ | __UL__ | __de__ | __SI__ | __Do__ | __Da__  | __CH__  | __St__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| __BO__ | St | St | St | St | St | St | St | St | St | St | St | St | St | St | St | Ob | 
| __SB__ |    | St | St | St | St | St | St | St | St | St | St | St | St | St | St | Ob | 
| __Durch__ |    |    | St | St | St | St | St | St | St | St | St | St | St | St | St | Ob | 
| __Sh__ |    |    |    | St | St | St | St | St | St | St | St | St | St | St | St | Ob | 
| __USA__ |    |    |    |    | St | St | St | St | St | St | St | St | St | St | St | Ob | 
| __In__ |    |    |    |    |    | St | St | St | St | St | St | St | St | St | St | Ob | 
| __BENUTZEROBERFLÄCHE__ |    |    |    |    |    |    | St | St | St | St | St | St | St | St | St | Ob | 
| __Lo__ |    |    |    |    |    |    |    | St | St | St | St | St | St | St | St | Ob | 
| __UL__ |    |    |    |    |    |    |    |    | St | St | St | St | St | St | St | Ob | 
| __de__ |    |    |    |    |    |    |    |    |    | St | St | St | St | St | St | Ob | 
| __SI__ |    |    |    |    |    |    |    |    |    |    | St | St | St | St | St | Ob | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | St | St | St | St | Ob | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | St | St | St | Ob | 
| __CH__ |    |    |    |    |    |    |    |    |    |    |    |    |    | St | St | Ob | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    | St | Ob | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | Ob | 


## <a name="logical-operators"></a>Logische Operatoren

Die `And`, `Not`, `Or`, und `Xor` Operatoren werden als die logischen Operatoren bezeichnet.

```antlr
LogicalOperatorExpression
    : 'Not' Expression
    | Expression 'And' LineTerminator? Expression
    | Expression 'Or' LineTerminator? Expression
    | Expression 'Xor' LineTerminator? Expression
    ;
```

Die logischen Operatoren sind wie folgt ausgewertet:

* Für die `Boolean` Typ:

  * Eine logische `And` Vorgang wird für seine beiden Operanden ausgeführt.

  * Eine logische `Not` Vorgang wird für Operanden ausgeführt.

  * Eine logische `Or` Vorgang wird für seine beiden Operanden ausgeführt.

  * Einen logischen exklusiv -`Or` Vorgang wird für seine beiden Operanden ausgeführt.

* Für `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, und alle Enumerationstypen, der angegebene Vorgang wird ausgeführt, für jedes Bit für die binäre Darstellung von der zwei Operand(s):

  * `And`: Das Ergebnisbit ist 1, wenn beide Bits 1 sind; Andernfalls ist das Ergebnisbit 0 auf.

  * `Not`: Das Ergebnisbit ist 1, wenn das Bit 0 ist; Andernfalls ist das Ergebnis 1.

  * `Or`: Das Ergebnisbit ist 1, wenn jedes Bit 1 ist; Andernfalls ist das Ergebnisbit 0 auf.

  * `Xor`: Das Ergebnisbit ist 1, wenn jedes Bit 1, aber nicht beide Bits ist; Andernfalls ist das Ergebnisbit 0 (d. h. 1 `Xor` 0 = 1, 1 `Xor` 1 = 0).

* Wenn die logischen Operatoren `And` und `Or` aufgehoben werden, für den Typ `Boolean?`, sie wurden erweitert, um boolesche Logik mit drei Werten als solche umfassen:

  * `And` ergibt "true", wenn beide Operanden wahr sind; "false", wenn einer der Operanden falsch ist; `Nothing` andernfalls.

  * `Or` ergibt "true", wenn ein Operand true ist; "false" werden beide Operanden sind falsch; `Nothing` andernfalls.

Zum Beispiel:

```vb
Module Test
    Sub Main()
        Dim x?, y? As Boolean

        x = Nothing
        y = True 

        If x Or y Then
            ' Will execute
        End If
    End Sub
End Module
```

__Beachten Sie.__ Im Idealfall die logischen Operatoren `And` und `Or` würde aufgehoben werden, mithilfe von dreiwertige Logik für jeden Typ, der in einen booleschen Ausdruck verwendet werden kann (d. h. einen Typ, der implementiert `IsTrue` und `IsFalse`), in der gleichen Weise wie `AndAlso` und `OrElse` Kurzschluss für jeden Typ, der in einen booleschen Ausdruck verwendet werden kann. Leider dreiwertige anheben wird nur angewendet, um `Boolean?`, sodass eine benutzerdefinierte Typen, die gewünschte dreiwertige Logik manuell, durch die Definition ausführen müssen `And` und `Or` Operatoren für ihre Version ist NULL-Werte zulässt.

Keine Überläufe sind von diesen Vorgängen möglich. Die enumerierten Typoperatoren führen bitweise Operation mit den zugrunde liegenden Typ des enumerierten Typs, aber der Rückgabewert ist die enumerierten Typ.

__Nicht Vorgangstyp:__

| __BO__ | __SB__ | __Durch__ | __Sh__ | __USA__ | __In__ | __BENUTZEROBERFLÄCHE__ | __Lo__ | __UL__ | __de__ | __SI__ | __Do__ | __Da__  | __CH__  | __St__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| BO | SB | um | Sh | US | In | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo | Ob | 

__Und, oder geben der Xor-Vorgang:__

|        | __BO__ | __SB__ | __Durch__ | __Sh__ | __USA__ | __In__ | __BENUTZEROBERFLÄCHE__ | __Lo__ | __UL__ | __de__ | __SI__ | __Do__ | __Da__  | __CH__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __BO__ | BO | SB | Sh | Sh | In | In | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | BO  | Ob  | 
| __SB__ |    | SB | Sh | Sh | In | In | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Durch__ |    |    | um | Sh | US | In | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Sh__ |    |    |    | Sh | In | In | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __USA__ |    |    |    |    | US | In | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __In__ |    |    |    |    |    | In | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __BENUTZEROBERFLÄCHE__ |    |    |    |    |    |    | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __de__ |    |    |    |    |    |    |    |    |    | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __SI__ |    |    |    |    |    |    |    |    |    |    | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Lo | Err | Err | Lo  | Ob  | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __CH__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Lo  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="short-circuiting-logical-operators"></a>Kurzschließen von logischen Operatoren

Die `AndAlso` und `OrElse` Operatoren sind die verkürzte Versionen der `And` und `Or` logische Operatoren.

```antlr
ShortCircuitLogicalOperatorExpression
    : Expression 'AndAlso' LineTerminator? Expression
    | Expression 'OrElse' LineTerminator? Expression
    ;
```

Aufgrund des kurzen kurzschließende Verhaltens ist der zweite Operand nicht zur Laufzeit ausgewertet, wenn das Ergebnis der Operator bekannt ist, nach der Auswertung der erste Operand.

Die verkürzte logische Operatoren werden wie folgt ausgewertet:

* Wenn der erste Operand in eine `AndAlso` Operation ergibt `False` fest oder gibt "true", aus der `IsFalse` -Operator, der Ausdruck gibt den ersten Operanden zurück. Andernfalls der zweite Operand ist, ausgewertet und eine logische `And` Vorgang für die beiden Ergebnisse ausgeführt wird.

* Wenn der erste Operand in eine `OrElse` Operation ergibt `True` fest oder gibt "true", aus der `IsTrue` -Operator, der Ausdruck gibt den ersten Operanden zurück. Andernfalls der zweite Operand ist, ausgewertet und eine logische `Or` Vorgang für die beiden Ergebnisse ausgeführt wird.

Die `AndAlso` und `OrElse` Operatoren sind für den Typ definierte `Boolean`, oder für einen beliebigen Typ `T` methodenüberladungen, die folgenden Operatoren:

```vb
Public Shared Operator IsTrue(op As T) As Boolean
Public Shared Operator IsFalse(op As T) As Boolean
```

sowie das Überladen zulässt, die entsprechende `And` oder `Or` Operator:

```vb
Public Shared Operator And(op1 As T, op2 As T) As T
Public Shared Operator Or(op1 As T, op2 As T) As T
```

Beim Auswerten der `AndAlso` oder `OrElse` Operatoren, der erste Operand wird nur einmal ausgewertet und der zweite Operand ist entweder nicht ausgewertet oder genau einmal ausgewertet. Beachten Sie z. B. folgenden Code:

```vb
Module Test
    Function TrueValue() As Boolean
        Console.Write(" True")
        Return True
    End Function

    Function FalseValue() As Boolean
        Console.Write(" False")
        Return False
    End Function

    Sub Main()
        Console.Write("And:")
        If FalseValue() And TrueValue() Then
        End If
        Console.WriteLine()

        Console.Write("Or:")
        If TrueValue() Or FalseValue() Then
        End If
        Console.WriteLine()

        Console.Write("AndAlso:")
        If FalseValue() AndAlso TrueValue() Then
        End If
        Console.WriteLine()

        Console.Write("OrElse:")
        If TrueValue() OrElse FalseValue() Then
        End If
        Console.WriteLine()
    End Sub
End Module
```

Gibt das folgende Ergebnis:

```
And: False True
Or: True False
AndAlso: False
OrElse: True
```

In der transformierten Form der `AndAlso` und `OrElse` Operatoren, wenn der erste Operand ein NULL-Wert wurde `Boolean?`, der zweite Operand ausgewertet werden, das Ergebnis ist immer ein NULL-Wert `Boolean?`.

__Vorgangstyp:__

|        | __BO__ | __SB__ | __Durch__ | __Sh__ | __USA__ | __In__ | __BENUTZEROBERFLÄCHE__ | __Lo__ | __UL__ | __de__ | __SI__ | __Do__ | __Da__  | __CH__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __BO__ | BO | BO | BO | BO | BO | BO | BO | BO | BO | BO | BO | BO | Err | Err | BO  | Ob  | 
| __SB__ |    | BO | BO | BO | BO | BO | BO | BO | BO | BO | BO | BO | Err | Err | BO  | Ob  | 
| __Durch__ |    |    | BO | BO | BO | BO | BO | BO | BO | BO | BO | BO | Err | Err | BO  | Ob  | 
| __Sh__ |    |    |    | BO | BO | BO | BO | BO | BO | BO | BO | BO | Err | Err | BO  | Ob  | 
| __USA__ |    |    |    |    | BO | BO | BO | BO | BO | BO | BO | BO | Err | Err | BO  | Ob  | 
| __In__ |    |    |    |    |    | BO | BO | BO | BO | BO | BO | BO | Err | Err | BO  | Ob  | 
| __BENUTZEROBERFLÄCHE__ |    |    |    |    |    |    | BO | BO | BO | BO | BO | BO | Err | Err | BO  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | BO | BO | BO | BO | BO | Err | Err | BO  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | BO | BO | BO | BO | Err | Err | BO  | Ob  | 
| __de__ |    |    |    |    |    |    |    |    |    | BO | BO | BO | Err | Err | BO  | Ob  | 
| __SI__ |    |    |    |    |    |    |    |    |    |    | BO | BO | Err | Err | BO  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | BO | Err | Err | BO  | Ob  | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __CH__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | BO  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


## <a name="shift-operators"></a>Schiebeoperatoren

Die binären Operatoren `<<` und `>>` Bit wechselnden Vorgänge ausführen.

```antlr
ShiftOperatorExpression
    : Expression '<' '<' LineTerminator? Expression
    | Expression '>' '>' LineTerminator? Expression
    ;
```

Die Operatoren definiert sind, für die `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` und `Long` Typen. Im Gegensatz zu anderen binären Operatoren bestimmt der Ergebnistyp eines schiebevorgangs, als ob der Operator ein unäroperator nur der linke Operand war. Der Typ des rechten Operanden muss implizit in `Integer` und wird bei der Bestimmung der Ergebnistyp des Vorgangs nicht verwendet.

Die `<<` bewirkt, dass die Bits in den ersten Operanden verschoben werden sollen die Anzahl der Stellen, die gemäß den Betrag der Verschiebung nach links. Die höherwertigen Bits außerhalb des Bereichs des Ergebnistyps werden verworfen, und die niederwertigen frei gewordenen Bitpositionen werden mit Nullen aufgefüllt.

Die `>>` Operator bewirkt, dass die Bits in den ersten Operanden verschoben werden sollen, nach rechts die Anzahl der Stellen, die gemäß den Betrag der Verschiebung. Die niederwertigen Bits verworfen, und auf eine falls negativ oder 0 (null), wenn der linke Operand positiv ist, werden die frei gewordenen Bitpositionen mit höherwertigen festgelegt. Wenn der linke Operand vom Typ `Byte`, `UShort`, `UInteger`, oder `ULong` der freien höherwertigen Bits werden mit Nullen aufgefüllt.

Die Schiebeoperatoren verschieben die Bits für die zugrunde liegende Darstellung des ersten Operanden durch die Menge des zweiten Operanden. Wenn der Wert des zweiten Operanden ist größer als die Anzahl der Bits in den ersten Operanden oder negativ ist, berechnet den Betrag der Verschiebung als `RightOperand And SizeMask` , in denen `SizeMask` ist:

| __LeftOperand-Typ__  | __SizeMask__ | 
|-----------------------|--------------|
| `Byte`, `SByte`       | 7 (`&H7`)    | 
| `UShort`, `Short`     | 15 (`&HF`)   | 
| `UInteger`, `Integer` | 31 (`&H1F`)  | 
| `ULong`, `Long`       | 63 (`&H3F`)  | 

Wenn Sie den Betrag der Verschiebung auf 0 (null) ist, ist das Ergebnis des Vorgangs mit dem Wert des ersten Operanden. Keine Überläufe sind von diesen Vorgängen möglich.

__Vorgangstyp:__


| __BO__ | __SB__ | __Durch__ | __Sh__ | __USA__ | __In__ | __BENUTZEROBERFLÄCHE__ | __Lo__ | __UL__ | __de__ | __SI__ | __Do__ | __Da__  | __CH__  | __St__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| Sh | SB | um | Sh | US | In | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo | Ob | 


## <a name="boolean-expressions"></a>Boolesche Ausdrücke

Ein boolescher Ausdruck ist ein Ausdruck, der getestet werden kann, um festzustellen, ob es "true" ist, oder wenn sie falsch ist.

```antlr
BooleanExpression
    : Expression
    ;
```

Ein Typ `T` kann in einen booleschen Ausdruck verwendet werden, sofern es sich um Reihenfolge ihrer Priorität:

* `T` ist `Boolean` oder `Boolean?`

* `T` verfügt über eine erweiternde Konvertierung in `Boolean`

* `T` verfügt über eine erweiternde Konvertierung in `Boolean?`

* `T` definiert zwei Pseudo-Operatoren, `IsTrue` und `IsFalse`.

* `T` verfügt über eine einschränkende Konvertierung in `Boolean?` , ist eine Konvertierung von nicht beinhalten `Boolean` zu `Boolean?`.

* `T` verfügt über eine einschränkende Konvertierung in `Boolean`.

__Beachten Sie.__ Es ist interessant, beachten Sie, dass bei `Option Strict` deaktiviert ist, einen Ausdruck mit eine einschränkende Konvertierung in `Boolean` wird akzeptiert, ohne dass ein Fehler während der Kompilierung, aber der Sprache weiterhin bevorzugen eine `IsTrue` Operator, wenn es vorhanden ist. Grund hierfür ist, `Option Strict` nur geändert werden, was wird nicht von der Sprache akzeptiert und ist nie ändert die eigentliche Bedeutung eines Ausdrucks. Daher `IsTrue` muss immer über eine einschränkende Konvertierung, bevorzugt werden, unabhängig von `Option Strict`.

Die folgende Klasse definiert z. B. keine erweiternde Konvertierung in `Boolean`. Als Ergebnis der Verwendung in der die `If` Anweisung bewirkt, dass einen Aufruf der `IsTrue` Operator.

```vb
Class MyBool
    Public Shared Widening Operator CType(b As Boolean) As MyBool
        ...
    End Operator

    Public Shared Narrowing Operator CType(b As MyBool) As Boolean
        ...
    End Operator

    Public Shared Operator IsTrue(b As MyBool) As Boolean
        ...
    End Operator

    Public Shared Operator IsFalse(b As MyBool) As Boolean
        ...
    End Operator
End Class

Module Test
    Sub Main()
        Dim b As New MyBool

        If b Then Console.WriteLine("True")
    End Sub
End Module
```

Wenn als typisiert oder konvertiert ein boolescher Ausdruck `Boolean` oder `Boolean?`, wenn der Wert "true" ist `True` und "false" andernfalls.

Andernfalls ein boolescher Ausdruck ruft die `IsTrue` Operator und gibt `True` Operator zurückgegeben `True`; andernfalls "false" ist (jedoch nie aufruft, die `IsFalse` Operator).

Im folgenden Beispiel `Integer` verfügt über eine einschränkende Konvertierung in `Boolean`, sodass ein NULL-Wert `Integer?` verfügt über eine einschränkende Konvertierung in beide `Boolean?` (Rückgabe von Null `Boolean`) und `Boolean` (die löst eine Ausnahme aus). Die einschränkende Konvertierung in `Boolean?` wird empfohlen, und daher den Wert des "`i`" als booleschen Ausdruck ist daher `False`.

```vb
Dim i As Integer? = Nothing
If i Then Console.WriteLine()
```


## <a name="lambda-expressions"></a>Lambda-Ausdrücke

Ein *Lambda-Ausdruck* definiert eine anonyme Methode wird aufgerufen, eine *Lambda-Methode*. Lambda-Methoden erleichtern die "inline"-Methoden an andere Methoden übergeben, die Delegattypen zu verwenden.

```antlr
LambdaExpression
    : SingleLineLambda
    | MultiLineLambda
    ;

SingleLineLambda
    : LambdaModifier* 'Function' ( OpenParenthesis ParameterList? CloseParenthesis )? Expression
    | 'Sub' ( OpenParenthesis ParameterList? CloseParenthesis )? Statement
    ;

MultiLineLambda
    : MultiLineFunctionLambda
    | MultiLineSubLambda
    ;

MultiLineFunctionLambda
    : LambdaModifier? 'Function' ( OpenParenthesis ParameterList? CloseParenthesis )? ( 'As' TypeName )? LineTerminator
      Block
      'End' 'Function'
    ;

MultiLineSubLambda
    : LambdaModifier? 'Sub' ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block
      'End' 'Sub'
    ;

LambdaModifier
    : 'Async' | 'Iterator'
    ;
```

Beispiel:

```vb
Module Test
    Delegate Function IntFunc(x As Integer) As Integer

    Sub Apply(a() As Integer, func As IntFunc)
        For index As Integer = 0 To a.Length - 1
            a(index) = func(a(index))
        Next index
    End Sub

    Sub Main()
        Dim a() As Integer = { 1, 2, 3, 4 }

        Apply(a, Function(x As Integer) x * 2)

        For Each value In a
            Console.Write(value & " ")
        Next value
    End Sub
End Module
```

wird ausgegeben:

```
2 4 6 8
```

Ein Lambda-Ausdruck beginnt mit der optionale Modifizierer `Async` oder `Iterator`, gefolgt vom Schlüsselwort `Function` oder `Sub` und einer Parameterliste. Parameter in einem Lambdaausdruck können nicht deklariert werden `Optional` oder `ParamArray` und darf keine Attribute aufweisen. Im Gegensatz zu regulären Methoden, das Auslassen der Parametertyp für eine Lambda-Methode automatisch leitet nicht `Object`. Stattdessen bei eine Lambda-Methode ist neu klassifiziert, die ausgelassenen Parametertypen und `ByRef` Modifizierer aus dem Zieltyp abgeleitet werden. Im vorherigen Beispiel der Lambda-Ausdruck ließe als `Function(x) x * 2`, und es würde den Typ des abgeleitet haben `x` sein `Integer` Wenn der Lambda-Methode wurde zum Erstellen einer Instanz von verwendet die `IntFunc` Delegattyp. Im Gegensatz zu lokalen Variablen Typrückschluss Wenn Lambda-Parameter der Methode einen Typ lässt, aber verfügt über ein Array oder NULL-Werte zulassen Namen Modifizierer verwenden, tritt ein Fehler während der Kompilierung.

Ein __regulären Lambda-Ausdruck__ ist ein URI mit keiner `Async` noch `Iterator` Modifizierer.

Ein __Iterator-Lambdaausdruck__ ist ein URI mit der `Iterator` -Modifizierer und keine `Async` Modifizierer. Es muss eine Funktion sein. Wenn sie auf einen anderen Wert neu klassifiziert werden, es kann nur neu auf einen Wert der Delegattyp, dessen Rückgabetyp `IEnumerator`, oder `IEnumerable`, oder `IEnumerator(Of T)` oder `IEnumerable(Of T)` für einige `T`, verfügt über keine ByRef-Parameter.

Ein __Async-Lambdaausdruck__ ist ein URI mit der `Async` -Modifizierer und keine `Iterator` Modifizierer. Ein Async Sub-Lambda kann nur auf Sub-Delegattyp ohne ByRef-Parameter den Wert neu klassifiziert werden. Das Lambda einer Async-Funktion kann nur auf einen Wert der Funktion Delegattyp, dessen Rückgabetyp, neu `Task` oder `Task(Of T)` für einige `T`, verfügt über keine ByRef-Parameter.

Lambda-Ausdrücke können entweder ein- oder mehrzeiligen sein. Einzeilige `Function` Lambda-Ausdrücke enthalten einen einzelnen Ausdruck, der den von der Lambda-Methode zurückgegebenen Wert darstellt. Einzeilige `Sub` Lambda-Ausdrücke enthalten eine einzelne Anweisung ohne dem abschließenden `StatementTerminator`. Zum Beispiel:

```vb
Module Test
    Sub Do(a() As Integer, action As Action(Of Integer))
        For index As Integer = 0 To a.Length - 1
            action(a(index))
        Next index
    End Sub

    Sub Main()
        Dim a() As Integer = { 1, 2, 3, 4 }

        Do(a, Sub(x As Integer) Console.WriteLine(x))
    End Sub
End Module
```

Einzeilige Lambda erstellt Bindung weniger streng als alle anderen Ausdrücke und Anweisungen an. Also z. B. "`Function() x + 5`"ist gleich"`Function() (x+5)"` statt"`(Function() x) + 5`". Um Mehrdeutigkeit zu einem einzeiligen verhindern `Sub` Lambda-Ausdruck darf keine, eine Dim-Anweisung oder eine Bezeichnung-Declaration-Anweisung. Darüber hinaus, wenn es in einem einzeiligen Klammern eingeschlossen ist `Sub` Lambda-Ausdruck kann nicht unmittelbar folgen ein Doppelpunkt ":", ein Memberzugriffsoperator ".", ein Wörterbuch-Memberzugriffsoperator "!" oder eine öffnende Klammer "(". Enthält keine Block-Anweisung (`With`, `SyncLock, If...EndIf`, `While`, `For`, `Do`, `Using`) noch `OnError` noch `Resume`.

__Beachten Sie.__ Im Lambda-Ausdruck `Function(i) x=i`, als Text interpretiert eine *Ausdruck* (welche Tests, ob `x` und `i` gleich sind). Aber im Lambda-Ausdruck `Sub(i) x=i`, wird der Text als Anweisung interpretiert (die weist `i` zu `x`).

Ein mehrzeiligen Lambda-Ausdruck enthält einen Anweisungsblock und enden mit einem entsprechenden `End` Anweisung (d. h. `End Function` oder `End Sub`). Wie bei regulärer Methoden einer mehrzeiligen Lambda-Methode des `Function` oder `Sub` Anweisung und `End` -Anweisungen müssen in eigenen Zeilen sein. Zum Beispiel:

```vb
' Error: Function statement must be on its own line!
Dim x = Sub(x As Integer) : Console.WriteLine(x) : End Sub

' OK
Dim y = Sub(x As Integer)
               Console.WriteLine(x)
          End Sub
```

Mehrzeilige `Function` Lambda-Ausdrücke können einen Rückgabetyp deklarieren, aber Attribute können nicht eingefügt werden, darauf. Wenn ein mehrzeiliges `Function` Lambda-Ausdruck einen Rückgabetyp deklariert, aber der Rückgabetyp abgeleitet werden kann, aus dem Kontext, in dem der Lambda-Ausdruck wird verwendet, wird dieser Rückgabetyp verwendet. Andernfalls wird der Rückgabetyp der Funktion wie folgt berechnet:

* In einem regulären Lambda-Ausdruck, der Rückgabetyp ist der bestimmende Typ der Ausdrücke in alle der `Return` Anweisungen in der Anweisungsblock.

* In einer Async-Lambda-Ausdruck, der Rückgabetyp ist `Task(Of T)` , in denen `T` ist der bestimmende Typ der Ausdrücke in alle der `Return` Anweisungen in der Anweisungsblock.

* In einem Iterator Lambda-Ausdruck, der Rückgabetyp ist `IEnumerable(Of T)` , in denen `T` ist der bestimmende Typ der Ausdrücke in alle der `Yield` Anweisungen in der Anweisungsblock.

Zum Beispiel:

```vb
Function f(min As Integer, max As Integer) As IEnumerable(Of Integer)
    If min > max Then Throw New ArgumentException()
    Dim x = Iterator Function()
                  For i = min To max
                    Yield i
                Next
               End Function

    ' infers x to be a delegate with return type IEnumerable(Of Integer)
    Return x()
End Function
```

In allen Fällen gibt es keine `Return` (bzw. `Yield`)-Anweisungen, oder wenn kein dominanter Typ zwischen ihnen vorhanden ist und strenge Semantik verwendet wird, tritt ein Fehler während der Kompilierung ist; andernfalls ist des bestimmenden Typs implizit `Object`.

Beachten Sie, dass der Rückgabetyp von allen berechnet wird `Return` -Anweisungen, selbst wenn sie nicht erreichbar sind. Zum Beispiel:

```vb
' Return type is Double
Dim x = Function()
              Return 10
               Return 10.50
          End Function
```

Es gibt keine implizite Rückgabevariablen auf, da kein Name für die Variable vorhanden ist.

Die Anweisungsblöcken in mehrzeiligen Lambda-Ausdrücke gelten die folgenden Einschränkungen:

* `On Error` und `Resume` Anweisungen sind nicht zulässig, obwohl `Try` -Anweisungen sind zulässig.

* Statische lokale Variablen können nicht in mehrzeiligen Lambda-Ausdrücken deklariert werden.

* Kann nicht in oder aus der Anweisungsblock eines mehrzeiligen Lambda-Ausdrucks, verzweigt, obwohl die normalen Verzweigungen Regeln darin beziehen. Zum Beispiel:

  ```vb
  Label1:
  Dim x = Sub()
                 ' Error: Cannot branch out
                 GoTo Label1

                 ' OK: Wholly within the lamba.
                 GoTo Label2:
            Label2:
            End Sub

  ' Error: Cannot branch in
  GoTo Label2
  ```

Ein Lambda-Ausdruck entspricht etwa einer anonymen Methode, die für den enthaltenden Typ deklariert. Im ersten Beispiel ist ungefähr gleich:

```vb
Module Test
    Delegate Function IntFunc(x As Integer) As Integer

    Sub Apply(a() As Integer, func As IntFunc)
        For index As Integer = 0 To a.Length - 1
            a(index) = func(a(index))
        Next index
    End Sub

    Function $Lambda1(x As Integer) As Integer
        Return x * 2
    End Function

    Sub Main()
        Dim a() As Integer = { 1, 2, 3, 4 }

        Apply(a, AddressOf $Lambda1)

        For Each value In a
            Console.Write(value & " ")
        Next value
    End Sub
End Module
```


### <a name="closures"></a>Closures

Lambda-Ausdrücke haben Zugriff auf alle Variablen im Bereich, einschließlich lokale Variablen oder Parameter, die bei der enthaltenden Methode und Lambda-Ausdrücken definiert. Wenn ein Lambda-Ausdruck auf eine lokale Variable oder Parameter verwiesen wird, erfasst der Lambda-Ausdruck die Variable in einen Closure bezeichnet wird. Als Closure ist ein Objekt, das auf dem Heap anstelle von auf dem Stapel befindet, und wenn eine Variable erfasst sind, werden alle Verweise auf die Variable auf den Abschluss umgeleitet. Dadurch können Lambda-Ausdrücke, um den Vorgang fortzusetzen, um auf lokale Variablen und Parameter verweisen, auch nach die enthaltende Methode abgeschlossen ist. Zum Beispiel:

```vb
Module Test
    Delegate Function D() As Integer

    Function M() As D
        Dim x As Integer = 10
        Return Function() x
    End Function

    Sub Main()
        Dim y As D = M()

        ' Prints 10
        Console.WriteLine(y())
    End Sub
End Module
```

ist ungefähr gleich ist:

```vb
Module Test
    Delegate Function D() As Integer

    Class $Closure1
        Public x As Integer

        Function $Lambda1() As Integer
            Return x
        End Function
    End Class

    Function M() As D
        Dim c As New $Closure1()
        c.x = 10
        Return AddressOf c.$Lambda1
    End Function

    Sub Main()
        Dim y As D = M()

        ' Prints 10
        Console.WriteLine(y())
    End Sub
End Module
```

Als Closure erfasst eine neue Kopie von einer lokalen Variable jedes Mal, es den Block in dem gibt, die lokale Variable wird deklariert, aber die neue Kopie wird mit dem Wert der vorherigen Kopie, initialisiert, sofern vorhanden. Zum Beispiel:

```vb
Module Test
    Delegate Function D() As Integer

    Function M() As D()
        Dim a(9) As D

        For i As Integer = 0 To 9
            Dim x
            a(i) = Function() x
            x += 1
        Next i

        Return a
    End Function

    Sub Main()
        Dim y() As D = M()

        For i As Integer = 0 To 9
            Console.Write(y(i)() & " ")
        Next i
    End Sub
End Module
```

Druckt

```
1 2 3 4 5 6 7 8 9 10
```

Statt

```
9 9 9 9 9 9 9 9 9 9
```

Da Closures initialisiert werden, wenn Sie einen Block eingeben müssen, es ist nicht zulässig, `GoTo` in einen Block mit einer Schließung von außerhalb von diesem Block zwar erlaubt sind `Resume` in einen Block mit einer Closure. Zum Beispiel:

```vb
Module Test
    Sub Main()
        Dim a = 10

        If a = 10 Then
L1:
            Dim x = Function() a

            ' Valid, source is within block
            GoTo L2
L2:
        End If

        ' ERROR: target is inside block with closure
        GoTo L1
    End Sub
End Module
```

Da sie nicht in einen Closure erfasst werden können, kann nicht innerhalb eines Lambda-Ausdrucks Folgendes angezeigt:

* Reference-Parameter.

* Instanz von Ausdrücken (`Me`, `MyClass`, `MyBase`), wenn der Typ des `Me` ist keine Klasse.

Die Member eines anonymen typerstellung-Ausdrucks, wenn der Lambda-Ausdruck Teil des Ausdrucks ist. Zum Beispiel:

```vb
' Error: Lambda cannot refer to anonymous type field
Dim x = New With { .a = 12, .b = Function() .a }
```

`ReadOnly` Instanzvariablen in Instanzkonstruktoren oder `ReadOnly` freigegebene Variablen im shared-Konstruktoren, in denen die Variablen in einem Kontext ohne Wert verwendet werden. Zum Beispiel:

```vb
Class C1
    ReadOnly F1 As Integer

    Sub New()
        ' Valid, doesn't modify F1
        Dim x = Function() F1

        ' Error, tries to modify F1
        Dim f = Function() ModifyValue(F1)
    End Sub

    Sub ModifyValue(ByRef x As Integer)
    End Sub
End Class
```

## <a name="query-expressions"></a>Abfrageausdrücke

Ein *Abfrageausdruck* ist ein Ausdruck, der eine Reihe von gilt *Abfrageoperatoren* auf die Elemente einer *abgefragt werden* Auflistung. Der folgende Ausdruck wird beispielsweise eine Auflistung von `Customer` -Objekte und gibt die Namen aller Kunden in den Bundesstaat Washington zurück:

```vb
Dim names = _
    From cust In Customers _
    Where cust.State = "WA" _
    Select cust.Name
```

Ein Abfrageausdruck muss mit beginnen eine `From` oder `Aggregate` Operator und kann mit jeder Abfrage-Operator enden. Das Ergebnis eines Abfrageausdrucks wird als Wert klassifiziert. der Ergebnistyp des Ausdrucks hängt von der Ergebnistyp des letzten Abfrageoperator im Ausdruck.

```antlr
QueryExpression
    : FromOrAggregateQueryOperator QueryOperator*
    ;

FromOrAggregateQueryOperator
    : FromQueryOperator
    | AggregateQueryOperator
    ;

QueryOperator
    : FromQueryOperator
    | AggregateQueryOperator
    | SelectQueryOperator
    | DistinctQueryOperator
    | WhereQueryOperator
    | OrderByQueryOperator
    | PartitionQueryOperator
    | LetQueryOperator
    | GroupByQueryOperator
    | JoinOrGroupJoinQueryOperator
    ;

JoinOrGroupJoinQueryOperator
    : JoinQueryOperator
    | GroupJoinQueryOperator
    ;
```

### <a name="range-variables"></a>Bereichsvariablen

Einige Abfrageoperatoren einer besonderen Art von Variable mit dem Namen einer *Bereichsvariable*. Bereichsvariablen sind nicht die echten Variablen. Stattdessen stellen sie die einzelnen Werte während der Auswertung der Abfrage über die eingabeauflistungen dar.

```antlr
CollectionRangeVariableDeclarationList
    : CollectionRangeVariableDeclaration ( Comma CollectionRangeVariableDeclaration )*
    ;

CollectionRangeVariableDeclaration
    : Identifier ( 'As' TypeName )? 'In' LineTerminator? Expression
    ;

ExpressionRangeVariableDeclarationList
    : ExpressionRangeVariableDeclaration ( Comma ExpressionRangeVariableDeclaration )*
    ;

ExpressionRangeVariableDeclaration
    : Identifier ( 'As' TypeName )? Equals Expression
    ;
```

Bereichsvariablen beziehen sich vom Einleitungsoperator Abfrage an das Ende eines Abfrageausdrucks oder auf einen Abfrageoperator wie z. B. `Select` , die blendet diese aus. Z. B. in der folgenden Abfrage

```vb
Dim waCusts = _
    From cust As Customer In Customers _
    Where cust.State = "WA"
```

die `From` Abfrage-Operator führt eine Bereichsvariable `cust` als `Customer` , darstellt, dass alle Kunden in der `Customers` Auflistung. Die folgenden `Where` -Abfrage-Operator bezieht sich dann auf die Bereichsvariable `cust` im Filterausdruck zu bestimmen, ob Sie einen einzelnen Kunden aus der resultierenden Auflistung zu filtern.

Es gibt zwei Arten der Bereichsvariablen: *Bereich sammlungsvariablen* und *Ausdruck Bereichsvariablen*. Sammlung von Bereichsvariablen dauern, deren Werte aus den Elementen der Sammlungen, die abgefragt wird. Des sammlungsausdrucks im Auflistung Deklaration einer Bereichsvariablen muss als Wert klassifiziert werden, dessen Typ abgefragt werden wird. Wenn der Typ einer Bereichsvariablen für die Sammlung weggelassen wird, wird abgeleitet, die den Elementtyp des sammlungsausdrucks sein oder `Object` des sammlungsausdrucks keinen Elementtyp (definiert z. B. nur eine `Cast` Methode). Wenn die Auflistungsausdruck nicht abgefragt werden (d. h. der Elementtyp der Sammlung kann nicht abgeleitet werden), einem Fehler während der Kompilierung führt.

Eine Bereichsvariable Ausdruck ist eine Bereichsvariable, die von einer Sammlung, sondern ein Ausdruck, dessen Wert berechnet wird. Im folgenden Beispiel die `Select` Abfrage-Operator führt eine Ausdruck Range-Variable, die mit dem Namen `cityState` aus zwei Felder berechnet:

```vb
Dim cityStates = _
    From cust As Customer In Customers _
    Select cityState = cust.City & "," & cust.State _
    Where cityState.Length() < 10
```

Eine Bereichsvariable Ausdruck ist nicht auf einem anderen Range-Variable, erforderlich, obwohl eine solche Variable fragwürdiger sein kann. Der Ausdruck zugewiesen werden, auf die Bereichsvariable ein Ausdruck muss als Wert klassifiziert werden und muss implizit in den Typ der Bereichsvariablen, wenn angegeben.

Nur in einer Let-Operator möglicherweise eine Bereichsvariable für den Ausdruck, den angegebenen Typ. In anderen Operatoren oder wenn der Typ ist nicht angegeben, lokale Variablen Typrückschluss wird verwendet, um den Typ der Bereichsvariablen zu bestimmen.

Eine Bereichsvariable muss den Regeln zum Deklarieren von lokaler Variablen in Bezug auf das shadowing folgen. Eine Bereichsvariable daher ausblenden nicht den Namen der lokalen Variable oder Parameter in die einschließende Methode oder einem anderen Bereichsvariable, (es sei denn, der Abfrage-Operator insbesondere alle aktuellen Bereichsvariablen im Bereich ausgeblendet).


### <a name="queryable-types"></a>Abfragbare Typen

Abfrageausdrücke werden implementiert, durch die Übersetzung des Ausdrucks in Aufrufe an bekannten auf einen Auflistungstyp. Diese klar definierten Methoden definiert den Typ des Elements der Auflistung abgefragt werden, sowie die Ergebnistypen der Abfrageoperatoren, die in der Auflistung ausgeführt. Jede Abfrage-Operator gibt die Methode oder die Methoden, denen der Abfrage-Operator in der Regel übersetzt wird, auch wenn der bestimmte Übersetzung hängt von der Implementierung ist. Die Methoden werden in der Spezifikation, die mit einem allgemeinen Format, das aussieht wie angegeben:

```vb
Function Select(selector As Func(Of T, R)) As CR
```

Die Methoden gilt Folgendes:

* Die Methode muss eine Instanz oder Erweiterungsmember des Auflistungstyps sein und muss zugänglich sein.

* Die Methode generisch ist, möglicherweise, bereitgestellt, die möglich, alle Typargumente abzuleiten.

* Die Methode möglicherweise überladen werden, in dem Fall Auflösung von funktionsüberladungen verwendet wird, um zu bestimmen, die genau zu verwendende Methode.

* Einen anderen Delegattyp kann verwendet werden, anstelle der Delegat `Func` eingeben, vorausgesetzt, dass die gleiche Signatur hat, einschließlich der Rückgabetyp, wie die entsprechenden `Func` Typ.

* Der Typ `System.Linq.Expressions.Expression(Of D)` kann verwendet werden, anstelle der Delegat `Func` Typ bereitgestellt, die `D` ist ein Delegattyp, der die gleiche Signatur, einschließlich der Rückgabetyp, wie die entsprechenden hat `Func` Typ.

* Der Typ `T` den Elementtyp der eingabeauflistung darstellt. Alle von einem Auflistungstyp definierten Methoden müssen den gleichen input-Element-Typ für den Auflistungstyp abgefragt werden sollen.

* Der Typ `S` stellt den Elementtyp der zweiten Eingabe Auflistung im Fall von Abfrageoperatoren, die Joins auszuführen.

* Der Typ `K` stellt einen Schlüsseltyp im Fall von Abfrageoperatoren, die einen Satz von Bereichsvariablen, die als Schlüssel verwendet.

* Der Typ `N` stellt einen Typ, der einen numerischen Typ verwendet wird (obwohl sie weiterhin einen benutzerdefinierten Typ und nicht mit einem systeminternen numerischen Typ sein kann).

* Der Typ `B` stellt einen Typ, der in einen booleschen Ausdruck verwendet werden kann.

* Der Typ `R` den Elementtyp der Ergebnissammlung, darstellt, wenn die Abfrage-Operator eine ergebnisauflistung erzeugt. `R` hängt von der Anzahl der Bereichsvariablen im Bereich am Ende der Abfrage-Operator. Wenn eine Variable für die einzelnen Bereich im Bereich ist `R` ist der Typ von dieser Bereichsvariablen. Im Beispiel

  ```vb
  Dim custNames = From c In Customers
                  Select c.Name
  ```

  Das Ergebnis der Abfrage werden mit einem Element vom Auflistungstyp `String`. Wenn mehrere Bereichsvariablen im Bereich, dann ist `R` ist ein anonymer Typ, der alle von der Bereichsvariablen im Bereich wie enthält `Key` Felder. Im folgenden Beispiel

  ```vb
  Dim custNames = From c In Customers, o In c.Orders 
                  Select Name = c.Name, ProductName = o.ProductName
  ```

  Das Ergebnis der Abfrage werden mit einem Elementtyp eines anonymen Typs mit einer nur-Lese Eigenschaft, die mit dem Namen vom Auflistungstyp `Name` des Typs `String` und eine schreibgeschützte Eigenschaft, die mit dem Namen `ProductName` des Typs `String`.

  In einem Abfrageausdruck, anonyme Typen generiert, um die Bereichsvariablen enthalten sind *transparent*, d. h., die Variablen liegen stehen immer zur Verfügung, ohne Qualifizierung. Im vorherigen Beispiel z. B. die Bereichsvariablen `c` und `o` zugegriffen werden kann, ohne Qualifikation in die `Select` -Abfrage-Operator, obwohl der eingabeauflistung Elementtyp eines anonymen Typs wurde.

* Der Typ `CX` stellt einen Auflistungstyp, nicht unbedingt der Eingabesammlung, dessen Elementtyp eine Art ist `X`.

Ein abfragbare Auflistung-Typ muss eine der folgenden Bedingungen, in der Reihenfolge ihrer Priorität erfüllen:

* Müssen sie definieren, eine Übereinstimmung `Select` Methode.

* Sie haben benötigen einen der folgenden Methoden

  ```vb
  Function AsEnumerable() As CT
  Function AsQueryable() As CT
  ```

  Das kann zum Abrufen einer abfragbaren Auflistung aufgerufen werden. Wenn beide Methoden bereitgestellt werden, `AsQueryable` vorzuziehen ist `AsEnumerable`.

* Es muss eine Methode verfügen.

  ```vb
  Function Cast(Of T)() As CT
  ```

  die mit dem Typ der Bereichsvariablen zum Erzeugen einer abfragbaren Auflistung aufgerufen werden kann.

Da bestimmen den Typ des Elements einer Auflistung unabhängig von der eine tatsächliche Methodenaufruf auftritt, kann die Anwendbarkeit der spezifischen Methoden bestimmt werden. Daher werden beim Ermitteln den Typ des Elements einer Auflistung an, ob es sind Instanzmethoden, die bekannte Methoden entsprechen, Erweiterungsmethoden, die bekannte Methoden entsprechen dann ignoriert.

Übersetzung von Standardabfrageoperatoren tritt auf, in der Reihenfolge, in der die Abfrageoperatoren im Ausdruck auftreten. Es ist nicht erforderlich für ein Auflistungsobjekt, alle Methoden, die von allen die Abfrageoperatoren, benötigt implementieren, obwohl jedes Objekt mindestens unterstützen muss die `Select` -Abfrage-Operator. Wenn eine erforderliche Methode nicht vorhanden ist, tritt ein Fehler während der Kompilierung. Beim Binden von bekannten Namen werden nicht-Methoden für mehrfache Vererbung in Schnittstellen und Erweiterungsmethode, die Bindung ignoriert, obwohl shadowing Semantik weiterhin gelten. Zum Beispiel:

```vb
Class Q1
    Public Function [Select](selector As Func(Of Integer, Integer)) As Q1
    End Function
End Class

Class Q2
    Inherits Q1

    Public [Select] As Integer
End Class

Module Test
    Sub Main()
        Dim qs As New Q2()

        ' Error: Q2.Select still hides Q1.Select
        Dim zs = From q In qs Select q
    End Sub
End Module
```

### <a name="default-query-indexer"></a>Standardindexer für die Abfrage

Jede abfragbare Auflistung-Typ, dessen Elementtyp `T` und noch keinen Standardwert Eigenschaft gilt eine Standardeigenschaft, die folgende allgemeine Form aufweisen:

```vb
Public ReadOnly Default Property Item(index As Integer) As T
    Get
        Return Me.ElementAtOrDefault(index)
    End Get
End Property
```

Die Default-Eigenschaft kann nur mit der Syntax der Standardeigenschaft Zugriff verwiesen werden; Die Standardeigenschaft kann nicht namentlich verwiesen werden. Zum Beispiel:

```vb
Dim customers As IEnumerable(Of Customer) = ...
Dim customerThree = customers(2)

' Error, no such property
Dim customerFour = customers.Item(4)
```

Wenn der Auflistungstyp kein `ElementAtOrDefault` Element ein Fehler während der Kompilierung erfolgt.

### <a name="from-query-operator"></a>Von Abfrage-Operator

Die `From` Abfrage-Operator stellt eine Auflistung Bereichsvariable, die die einzelnen Mitglieder einer Sammlung, die abgefragt werden darstellt.

```antlr
FromQueryOperator
    : LineTerminator? 'From' LineTerminator? CollectionRangeVariableDeclarationList
    ;
```

Um beispielsweise den Abfrageausdruck:

```vb
From c As Customer In Customers ...
```

kann als gleich betrachtet werden

```vb
For Each c As Customer In Customers
        ...
Next c
```

Wenn eine `From` -Abfrage-Operator deklariert mehrere Auflistung Bereichsvariablen oder ist nicht der erste `From` im Abfrageausdruck-Abfrage-Operator, jedes neue Bereichsvariable für die Sammlung ist *Cross verknüpft* auf den vorhandenen Satz von Bereichsvariablen. Das Ergebnis ist, dass die Abfrage über das Kreuzprodukt aller Elemente in den verknüpften Sammlungen ausgewertet wird. Beispielsweise der Ausdruck:

```vb
From c In Customers _
From e In Employees _
...
```

kann als gleich betrachtet werden:

```vb
For Each c In Customers
    For Each e In Employees
            ...
    Next e
Next c
```

und genau gleich ist:

```vb
From c In Customers, e In Employees ...
```

Die Bereichsvariablen eingeführt, die in vorherigen Abfrageoperatoren können verwendet werden, in einem späteren `From` -Abfrage-Operator. In der folgende Abfrageausdruck beispielsweise das zweite `From` -Abfrage-Operator bezieht sich auf den Wert der ersten Bereichsvariablen:

```vb
From c As Customer In Customers _
From o As Order In c.Orders _
Select c.Name, o
```

Mehrere Bereichsvariablen in einem `From` Operator oder mehrere Abfragen `From` Abfrageoperatoren werden nur unterstützt, wenn der Typ der Auflistung, eine oder beide der folgenden Methoden enthält:

```vb
Function SelectMany(selector As Func(Of T, CR)) As CR
Function SelectMany(selector As Func(Of T, CS), _
                          resultsSelector As Func(Of T, S, R)) As CR
```

Der code

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs, y In ys ...
```

wird in der Regel in übersetzt.

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.SelectMany( _
        Function(x As Integer) ys, _
        Function(x As Integer, y As Integer) New With {x, y})...
```

__Beachten Sie.__ `From` ist ein reserviertes Wort.


### <a name="join-query-operator"></a>JOIN-Abfrage-Operator

Die `Join` Abfrage-Operator verknüpft vorhandenen Bereichsvariablen für eine neue Auflistung Bereichsvariable, erzeugt eine einzelne Sammlung, deren Elemente miteinander verknüpft wurde, basierend auf einem gleichheitsausdruck auf.

```antlr
JoinQueryOperator
    : LineTerminator? 'Join' LineTerminator? CollectionRangeVariableDeclaration
      JoinOrGroupJoinQueryOperator? LineTerminator? 'On' LineTerminator? JoinConditionList
    ;

JoinConditionList
    : JoinCondition ( 'And' LineTerminator? JoinCondition )*
    ;

JoinCondition
    : Expression 'Equals' LineTerminator? Expression
    ;
```

Zum Beispiel:

```vb
Dim customersAndOrders = _
    From cust In Customers _
    Join ord In Orders On cust.ID Equals ord.CustomerID
```

Der gleichheitsausdruck auf ist stärker eingeschränkt als eine reguläre gleichheitsausdruck:

* Beide Ausdrücke müssen als Wert klassifiziert werden.

* Beide Ausdrücke müssen mindestens eine Bereichsvariable verweisen.

* Die Bereichsvariable, deklariert im Join-Abfrage-Operator von einem der Ausdrücke verwiesen werden muss und dass der Ausdruck keine anderen Bereichsvariablen verweisen, muss.

Wenn die Typen der beiden Ausdrücke nicht exakt denselben Typ haben, klicken Sie dann

* Wenn der Gleichheitsoperator ist für die beiden Typen definiert, beide Ausdrücke implizit konvertierbar in es sind und es nicht ist `Object`, beide Ausdrücke dann in diesen Typ zu konvertieren.

* Andernfalls liegt ein bestimmende Typ, dem beide Ausdrücke in implizit konvertiert werden können, klicken Sie dann konvertieren Sie beide Ausdrücke auf diesen Typ.

* Andernfalls tritt ein Kompilierungsfehler auf.

Die Ausdrücke werden mithilfe von Hashwerten verglichen (z. B. durch Aufrufen von `GetHashCode()`), nicht für Effizienz mithilfe von Gleichheitsoperatoren. Ein `Join` -Abfrage-Operator kann mehrere Joins oder Gleichheit-Bedingungen in denselben Operator ist. Ein `Join` -Abfrage-Operator wird nur unterstützt, wenn der Typ der Auflistung eine Methode enthält:

```vb
Function Join(inner As CS, _
                  outerSelector As Func(Of T, K), _
                  innerSelector As Func(Of S, K), _
                  resultSelector As Func(Of T, S, R)) As CR
```

Der code

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs _
            Join y In ys On x Equals y _
            ...
```

wird in der Regel in übersetzt.

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.Join( _
        ys, _
        Function(x As Integer) x, _
        Function(y As Integer) y, _
        Function(x As Integer, y As Integer) New With {x, y})...
```

__Beachten Sie.__ `Join`, `On` und `Equals` sind keine reservierten Wörter.


### <a name="let-query-operator"></a>Let-Abfrage-Operator

Die `Let` Abfrage-Operator führt eine Bereichsvariable für den Ausdruck. Dadurch wird ein Zwischenwert berechnet, sobald, die mehrere Male in späteren Abfrageoperatoren verwendet wird.

```antlr
LetQueryOperator
    : LineTerminator? 'Let' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

Zum Beispiel:

```vb
Dim taxedPrices = _
    From o In Orders _
    Let tax = o.Price * 0.088 _
    Where tax > 3.50 _
    Select o.Price, tax, total = o.Price + tax
```

kann als gleich betrachtet werden:

```vb
For Each o In Orders
    Dim tax = o.Price * 0.088
    ...
Next o
```

Ein `Let` -Abfrage-Operator wird nur unterstützt, wenn der Typ der Auflistung eine Methode enthält:

```vb
Function Select(selector As Func(Of T, R)) As CR
```

Der code

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Let y = x * 10 _
            ...
```

wird in der Regel in übersetzt.

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="select-query-operator"></a>SELECT-Abfrage-Operator

Die `Select` -Abfrage-Operator ist, wie die `Let` -Abfrage-Operator, da es Ausdruck Bereichsvariablen; führt jedoch eine `Select` Abfrage-Operator wird ausgeblendet, die derzeit verfügbaren Bereichsvariablen anstatt sie hinzuzufügen. Auch der Typ einer Ausdruck-Bereichsvariablen eingeführt, die von einem `Select` -Abfrage-Operator wird immer abgeleitet mithilfe der lokalen Variablen Typrückschlussregeln entspricht; ein expliziter Typ kann nicht angegeben werden, und wenn kein Typ abgeleitet werden kann, tritt ein Fehler während der Kompilierung.

```antlr
SelectQueryOperator
    : LineTerminator? 'Select' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

Beispielsweise ist in der Abfrage:

```vb
Dim smiths = _
    From cust In Customers _
    Select name = cust.name _
    Where name.EndsWith("Smith")
```

die `Where` -Abfrage-Operator hat nur Zugriff auf die `name` von eingeführten Bereichsvariable der `Select` Operator; Wenn die `Where` Operator würde, käme es auf `cust`, würde ein Kompilierung ein Fehler aufgetreten.

Anstatt Sie explizit die Namen der Bereichsvariablen, eine `Select` Abfrage-Operator kann die Namen der Bereichsvariablen, ableiten mithilfe derselben Regeln als Ausdrücke in anonymen Typ-Objekt erstellen. Zum Beispiel:

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.name, ord.ProductName _
        Where name.EndsWith("Smith")
```

Wenn der Name der Bereichsvariablen nicht angegeben wird, und ein Name kann nicht abgeleitet werden, tritt ein Fehler während der Kompilierung. Wenn die `Select` -Abfrage-Operator enthält nur einen einzelnen Ausdruck, wird kein Fehler auftritt, wenn Sie ein Namen für die Range-Variable kann nicht abgeleitet werden, aber die Bereichsvariable ist namenlosen.  Zum Beispiel:

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.Name & " bought " & ord.ProductName _
        Take 10
```

Bei eine Mehrdeutigkeit in einem `Select` zwischen Zuweisen eines Namens zu einer Range-Variable und einem gleichheitsausdruck-Abfrage-Operator, der die namenszuweisung wird bevorzugt. Zum Beispiel:

```vb
Dim badCustNames = _
      From c In Customers _
        Let name = "John Smith" _
      Select name = c.Name ' Creates a range variable named "name"


Dim goodCustNames = _
      From c In Customers _
        Let name = "John Smith" _
      Select match = (name = c.Name)
```

Jeder Ausdruck in der `Select` -Abfrage-Operator muss als Wert klassifiziert werden. Ein `Select` -Abfrage-Operator wird nur unterstützt, wenn der Typ der Auflistung eine Methode enthält:

```vb
Function Select(selector As Func(Of T, R)) As CR
```

Der code

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Select x, y = x * 10 _
            ...
```

wird in der Regel in übersetzt.

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="distinct-query-operator"></a>DISTINCT-Abfrage-Operator

Die `Distinct` Abfrage-Operator schränkt die Werte in einer Sammlung nur für Benutzer mit unterschiedlichen Werten vom Typ des Elements, auf Gleichheit verglichen.

```antlr
DistinctQueryOperator
    : LineTerminator? 'Distinct' LineTerminator?
    ;
```

Um beispielsweise die Abfrage:

```vb
Dim distinctCustomerPrice = _
    From cust In Customers, ord In cust.Orders _
    Select cust.Name, ord.Price _
    Distinct
```

Gibt eine Zeile für jede unterschiedliche Paarung ergibt sich der Preis für Neukunden Name und die Reihenfolge, nur zurück, selbst wenn der Kunde mehrere Bestellungen mit den gleichen Preis hat. Ein `Distinct` -Abfrage-Operator wird nur unterstützt, wenn der Typ der Auflistung eine Methode enthält:

```vb
Function Distinct() As CT
```

Der code

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Distinct _
            ...
```

wird in der Regel in übersetzt.

```vb
Dim xs() As Integer = ...
Dim zs = xs.Distinct()...
```

__Beachten Sie.__ `Distinct` ist ein reserviertes Wort.


### <a name="where-query-operator"></a>In dem Abfrage-Operator

Die `Where` Abfrage-Operator schränkt die Werte in einer Auflistung, die eine angegebene Bedingung erfüllen.

```antlr
WhereQueryOperator
    : LineTerminator? 'Where' LineTerminator? BooleanExpression
    ;
```

Ein `Where` -Abfrage-Operator akzeptiert einen booleschen Ausdruck, der für jeden Satz von Variablenwerten Bereich ausgewertet wird, wenn der Wert des Ausdrucks "true", und klicken Sie dann die Werte in der Output-Auflistung angezeigt werden, andernfalls die Werte werden übersprungen. Um beispielsweise den Abfrageausdruck:

```vb
From cust In Customers, ord In Orders _
Where cust.ID = ord.CustomerID _
...
```

kann auf die geschachtelte Schleife als gleichwertig betrachtet werden

```vb
For Each cust In Customers
    For Each ord In Orders
            If cust.ID = ord.CustomerID Then
                ...
            End If
    Next ord
Next cust
```

Ein `Where` -Abfrage-Operator wird nur unterstützt, wenn der Typ der Auflistung eine Methode enthält:

```vb
Function Where(predicate As Func(Of T, B)) As CT
```

Der code

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Where x < 10 _
            ...
```

wird in der Regel in übersetzt.

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x As Integer) x < 10)...
```

__Beachten Sie.__ `Where` ist ein reserviertes Wort.


### <a name="partition-query-operators"></a>Partition-Abfrageoperatoren

```antlr
PartitionQueryOperator
    : LineTerminator? 'Take' LineTerminator? Expression
    | LineTerminator? 'Take' 'While' LineTerminator? BooleanExpression
    | LineTerminator? 'Skip' LineTerminator? Expression
    | LineTerminator? 'Skip' 'While' LineTerminator? BooleanExpression
    ;
```

Die `Take` Abfrageergebnisse Operator in der ersten `n` Elemente einer Auflistung. Bei Verwendung mit der `While` Modifizierer, die `Take` Operatorergebnissen in der ersten `n` Elemente einer Auflistung, die einen booleschen Ausdruck erfüllen. Die `Skip` Operator überspringt die ersten `n` Elemente einer Auflistung und gibt dann den Rest der Auflistung zurück.  Bei Verwendung in Verbindung mit der `While` Modifizierer, die `Skip` Operator überspringt die ersten `n` Elemente einer Auflistung, die einen booleschen Ausdruck erfüllen, und gibt dann den Rest der Auflistung zurück. Die Ausdrücke in einem `Take` oder `Skip` -Abfrage-Operator muss als Wert klassifiziert werden.

Ein `Take` -Abfrage-Operator wird nur unterstützt, wenn der Typ der Auflistung eine Methode enthält:

```vb
Function Take(count As N) As CT
```

Ein `Skip` -Abfrage-Operator wird nur unterstützt, wenn der Typ der Auflistung eine Methode enthält:

```vb
Function Skip(count As N) As CT
```

Ein `Take While` -Abfrage-Operator wird nur unterstützt, wenn der Typ der Auflistung eine Methode enthält:

```vb
Function TakeWhile(predicate As Func(Of T, B)) As CT
```

Ein `Skip While` -Abfrage-Operator wird nur unterstützt, wenn der Typ der Auflistung eine Methode enthält:

```vb
Function SkipWhile(predicate As Func(Of T, B)) As CT
```

Der code

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Skip 10 _
            Take 5 _
            Skip While x < 10 _
            Take While x > 5 _
            ...
```

wird in der Regel in übersetzt.

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Skip(10). _
        Take(5). _
        SkipWhile(Function(x) x < 10). _
        TakeWhile(Function(x) x > 5)...
```

__Beachten Sie.__ `Take` und `Skip` sind keine reservierten Wörter.


### <a name="order-by-query-operator"></a>Order By-Abfrage-Operator

Die `Order By` Abfrageoperator sortiert die Werte, die in den Bereichsvariablen angezeigt werden. 

```antlr
OrderByQueryOperator
    : LineTerminator? 'Order' 'By' LineTerminator? OrderExpressionList
    ;

OrderExpressionList
    : OrderExpression ( Comma OrderExpression )*
    ;

OrderExpression
    : Expression Ordering?
    ;

Ordering
    : 'Ascending' | 'Descending'
    ;
```

Ein `Order By` -Abfrage-Operator verwendet, Ausdrücke, die die Schlüsselwerte angeben, die zum Sortieren der Iterationsvariablen verwendet werden soll. Die folgende Abfrage gibt z. B. Produkte nach Preis sortiert:

```vb
Dim productsByPrice = _
    From p In Products _
    Order By p.Price _
    Select p.Name
```

Eine Sortierung kann gekennzeichnet werden, als `Ascending`, kleinere Werte in diesem Fall vor dem größere Werte stammen oder `Descending`, größere Werte in diesem Fall vor kleineren Werten zu kommen. Der Standardwert für eine Sortierung, wenn keine Angabe erfolgt ist `Ascending`. Die folgende Abfrage gibt z. B. Produkte, zuerst nach Preis, mit dem die teuersten Produkt sortiert:

```vb
Dim productsByPriceDesc = _
    From p In Products _
    Order By p.Price Descending _
    Select p.Name
```

Die `Order By` Abfrage-Operator kann angeben, mehrere Ausdrücke für die Reihenfolge, in diesem Fall wird die Auflistung in geschachtelte Weise sortiert. Die folgende Abfrage wird z. B. Kunden nach Bundesstaat, klicken Sie dann nach Stadt in jeden Status und dann nach Postleitzahl in jeder Stadt sortiert:

```vb
Dim customersByLocation = _
    From c In Customers _
    Order By c.State, c.City, c.ZIP _
    Select c.Name, c.State, c.City, c.ZIP
```

Die Ausdrücke in einer `Order By` -Abfrage-Operator muss als Wert klassifiziert werden. Ein `Order By` -Abfrage-Operator wird nur unterstützt, wenn der Typ der Auflistung eine oder beide der folgenden Methoden enthält:

```vb
Function OrderBy(keySelector As Func(Of T, K)) As CT
Function OrderByDescending(keySelector As Func(Of T, K)) As CT
```

Der Rückgabetyp `CT` muss ein *geordnete Auflistung*. Eine geordnete Auflistung ist eine Collection-Typ, der mindestens eine der beiden Methoden enthält:

```vb
Function ThenBy(keySelector As Func(Of T, K)) As CT
Function ThenByDescending(keySelector As Func(Of T, K)) As CT
```

Der code

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Order By x Ascending, x Mod 2 Descending _
            ...
```

wird in der Regel in übersetzt.

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.OrderBy(Function(x) x).ThenByDescending(Function(x) x Mod 2)...
```

__Beachten Sie.__ Da Abfrageoperatoren einfach Syntax Methoden, die einen bestimmten Abfragevorgang zu implementieren zuzuordnen, Beibehaltung der Reihenfolge wird nicht von der Sprache vorgegeben, und richtet sich nach der Implementierung der der Operator selbst. Dies ist eine benutzerdefinierte Operatoren sehr ähnlich, dass die Implementierung, die für einen benutzerdefinierten numerischen Typ den Additionsoperator zu überladen, kann jede Aktion ähnlich wie eine Erweiterung nicht ausführen. Natürlich wird zum Beibehalten der Vorhersagbarkeit implementieren, die nicht die Erwartungen der Benutzer entspricht nicht empfohlen.

__Beachten Sie.__ `Order` und `By` sind keine reservierten Wörter.


### <a name="group-by-query-operator"></a>Group By Abfrageoperator

Die `Group By` -Abfrage-Operator gruppiert der Bereichsvariablen im Bereich, die basierend auf einem oder mehreren Ausdrücken Bereich liegen, und klicken Sie dann erstellt neue Variablen basierend auf dieser Gruppierungen.

```antlr
GroupByQueryOperator
    : LineTerminator? 'Group' ( LineTerminator? ExpressionRangeVariableDeclarationList )?
      LineTerminator? 'By' LineTerminator? ExpressionRangeVariableDeclarationList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

Z. B. die folgende Abfrage gruppiert alle Kunden von `State`, und klicken Sie dann berechnet die Anzahl und durchschnittliche Age der einzelnen Gruppen:

```vb
Dim averageAges = _
    From cust In Customers _
    Group By cust.State _
    Into Count(), Average(cust.Age)
```

Die `Group By` Abfrage-Operator verfügt über drei Klauseln: der optionale `Group` -Klausel, die `By` -Klausel und die `Into` Klausel. Die `Group` -Klausel besitzt die gleiche Syntax und die gleiche Wirkung wie das eine `Select` -Abfrage-Operator, mit dem Unterschied, dass sie gilt nur für die Bereichsvariablen, die zur Verfügung, in der `Into` Klausel und nicht die `By` Klausel. Zum Beispiel:

```vb
Dim averageAges = _
    From cust In Customers _
    Group cust.Age By cust.State _
    Into Count(), Average(Age)
```

Die `By` -Klausel deklariert Ausdruck Bereichsvariablen, die als Schlüsselwerte in den Gruppierungsvorgang verwendet werden. Die `Into` -Klausel ermöglicht die Deklaration des Ausdrucks Bereichsvariablen, die über den kombinierten Gruppen Aggregationen Berechnen der `By` Klausel. In der `Into` -Klausel, die Bereichsvariable der Ausdruck kann nur zugewiesen werden einen Ausdruck, der Aufruf einer Methode wird von einer *Aggregatfunktion*. Eine Aggregatfunktion handelt es sich um eine Funktion auf die Auflistung der Gruppe (der nicht unbedingt den gleichen Sammlungstyp der ursprünglichen Auflistung sein kann) und das aussieht wie eine der folgenden Methoden:

```vb
Function _name_() As _type_
Function _name_(selector As Func(Of T, R)) As R
```

Wenn eine Aggregatfunktion ein Delegatargument akzeptiert, kann der Aufrufausdruck einen Argumentausdruck enthalten, der als Wert klassifiziert werden muss.  Der Argumentausdruck können die Bereichsvariablen, die im Bereich befinden; im Aufruf für eine Aggregatfunktion stellen diesen Bereichsvariablen die Werte in der Gruppe wird gebildet, nicht alle Werte in der Auflistung dar. Z. B. in das ursprüngliche Beispiel in diesem Abschnitt die `Average` -Funktion berechnet den Durchschnitt der Kunden Zugriffe pro Status und nicht für alle Kunden zusammen.

Sämtliche Sammlungstypen gelten, haben die Aggregatfunktion `Group` definiert, die keine Parameter und gibt einfach die Gruppe zurück. Andere standard-Aggregatfunktionen, die ein Auflistungstyp liefern sind:

`Count` und `LongCount`, womit die Anzahl der Elemente zurückgegeben, in der Gruppe oder die Anzahl der Elemente in der Gruppe, die einen booleschen Ausdruck erfüllen. `Count` und `LongCount` werden nur unterstützt, wenn der Typ der Auflistung eine der Methoden enthält:

```vb
Function Count() As N
Function Count(selector As Func(Of T, B)) As N
Function LongCount() As N
Function LongCount(selector As Func(Of T, B)) As N
```

`Sum`, die die Summe eines Ausdrucks auf alle Elemente in der Gruppe zurückgibt. `Sum` wird nur unterstützt, wenn der Typ der Auflistung eine der Methoden enthält:

```vb
Function Sum() As N
Function Sum(selector As Func(Of T, N)) As N
```

`Min` den minimale Wert eines Ausdrucks auf alle Elemente in der Gruppe zurückgegeben. `Min` wird nur unterstützt, wenn der Typ der Auflistung eine der Methoden enthält:

```vb
Function Min() As N
Function Min(selector As Func(Of T, N)) As N
```

`Max`, womit den maximalen Wert eines Ausdrucks auf alle Elemente in der Gruppe. `Max` wird nur unterstützt, wenn der Typ der Auflistung eine der Methoden enthält:

```vb
Function Max() As N
Function Max(selector As Func(Of T, N)) As N
```

`Average`, die gibt den Mittelwert eines Ausdrucks auf alle Elemente in der Gruppe zurück. `Average` wird nur unterstützt, wenn der Typ der Auflistung eine der Methoden enthält:

```vb
Function Average() As N
Function Average(selector As Func(Of T, N)) As N
```

`Any`, der bestimmt, ob eine Gruppe Elemente enthält oder ein boolescher Ausdruck für jedes Element in der Gruppe "true" ist. `Any` Gibt einen Wert, der in einem booleschen Ausdruck verwendet werden kann und wird nur unterstützt, wenn der Typ der Auflistung eine der Methoden enthält:

```vb
Function Any() As B
Function Any(predicate As Func(Of T, B)) As B
```

`All`, der bestimmt, ob ein boolescher Ausdruck für alle Elemente in der Gruppe "true" ist. `All` Gibt einen Wert, der in einem booleschen Ausdruck verwendet werden kann und wird nur unterstützt, wenn der Typ der Auflistung eine Methode enthält:

```vb
Function All(predicate As Func(Of T, B)) As B
```

Nach einem `Group By` -Abfrage-Operator, der Bereichsvariablen zuvor im Bereich ausgeblendet werden, und die Bereichsvariablen eingeführt, durch die `By` und `Into` Klauseln sind verfügbar. Ein `Group By` -Abfrage-Operator wird nur unterstützt, wenn der Typ der Auflistung die Methode enthält:

```vb
Function GroupBy(keySelector As Func(Of T, K), _
                      resultSelector As Func(Of K, CT, R)) As CR
```

Variablendeklarationen in den Bereich der `Group` -Klausel werden unterstützt, nur, wenn der Typ der Auflistung die Methode enthält:

```vb
Function GroupBy(keySelector As Func(Of T, K), _
                      elementSelector As Func(Of T, S), _
                      resultSelector As Func(Of K, CS, R)) As CR
```

Der code

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Group y = x * 10, z = x / 10 By evenOdd = x Mod 2 _
            Into Sum(y), Average(z) _
            ...
```

wird in der Regel in übersetzt.

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.GroupBy( _
        Function(x As Integer) x Mod 2, _
        Function(x As Integer) New With {.y = x * 10, .z = x / 10}, _
        Function(evenOdd, group) New With { _
            evenOdd, _
            .Sum = group.Sum(Function(e) e.y), _
            .Average = group.Average(Function(e) e.z)})...
```

__Beachten Sie.__ `Group`, `By`, und `Into` sind keine reservierten Wörter.


### <a name="aggregate-query-operator"></a>Aggregate-Abfrage-Operator

Die `Aggregate` -Abfrage-Operator hat eine ähnliche Funktion wie der `Group By` -Operator, mit der Ausnahme ermöglicht aggregieren über Gruppen, die bereits gebildet wurden. Da die Gruppe bereits gebildet wurde, die `Into` -Klausel einer `Aggregate` Abfrage-Operator wird nicht der Bereichsvariablen im Bereich ausgeblendet (auf diese Weise `Aggregate` eher wie eine `Let`, und `Group By` eher wie eine `Select`).

```antlr
AggregateQueryOperator
    : LineTerminator? 'Aggregate' LineTerminator? CollectionRangeVariableDeclaration QueryOperator*
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

Die folgende Abfrage wird z. B. die Summe aller Bestellungen von Kunden in Washington, USA aggregiert:

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Into Sum(order.Total)
```

Das Ergebnis dieser Abfrage ist eine Auflistung, dessen Elementtyp, ist ein anonymer Typ mit einer Eigenschaft mit dem Namen `cust` als `Customer` und eine Eigenschaft mit dem Namen `Sum` als `Integer`.

Im Gegensatz zu `Group By`, zusätzlichen Abfrageoperatoren platziert werden können, zwischen den `Aggregate` und `Into` Klauseln. Zwischen einer `Aggregate` -Klausel und dem Ende der `Into` -Klausel, die alle Bereichsvariablen im Bereich, einschließlich der deklariert, indem die `Aggregate` -Klausel kann verwendet werden. Z. B. die folgende Abfrage Aggregate die Gesamtmenge aller Bestellungen platziert von Kunden in Washington, USA vor 2006:

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum = Sum(order.Total)
```

Die `Aggregate` Operator kann auch zu einem Abfrageausdruck verwendet werden. In diesem Fall wird das Ergebnis des Abfrageausdrucks werden den einzelnen Wert berechnet, indem die `Into` Klausel. Die folgende Abfrage wird z. B. die Summe der Auftragssummen vor dem 1. Januar 2006 berechnet:

```vb
Dim ordersTotal = _
    Aggregate order In Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum(order.Total)
```

Das Ergebnis der Abfrage ist eine einzelne `Integer` Wert. Ein `Aggregate` -Abfrage-Operator ist immer verfügbar (obwohl die aggregate-Funktion werden zudem muss für den Ausdruck gültig ist). Der code

```vb
Dim xs() As Integer = ...
Dim zs = _
    Aggregate x In xs _
    Where x < 5 _
    Into Sum()
```

wird in der Regel in übersetzt.

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x) x < 5).Sum()
```

__Beachten Sie.__ `Aggregate` und `Into` sind keine reservierten Wörter.


### <a name="group-join-query-operator"></a>Group Join-Abfrage-Operator

Die `Group Join` -Abfrage-Operator kombiniert die Funktionen des die `Join` und `Group By` Abfrageoperatoren in einen einzelnen Operator. `Group Join` verknüpft zwei Auflistungen, die auf Grundlage übereinstimmender Schlüssel extrahiert aus den Elementen, das Gruppieren aller Elemente auf der rechten Seite des Joins, die ein bestimmtes Element auf der linken Seite des Joins entsprechen. Daher wird der Operator einen Satz von hierarchische Ergebnisse erzeugt.

```antlr
GroupJoinQueryOperator
    : LineTerminator? 'Group' 'Join' LineTerminator? CollectionRangeVariableDeclaration
      JoinOrGroupJoinQueryOperator? LineTerminator? 'On' LineTerminator? JoinConditionList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

Die folgende Abfrage erzeugt z. B. Elemente, die einen einzelnen Kunden Namen, eine Gruppe von alle Bestellungen und die Gesamtmenge aller die Bestellungen enthalten:

```vb
Dim custsWithOrders = _
    From cust In Customers _
    Group Join order In Orders On cust.ID Equals order.CustomerID _ 
    Into Orders = Group, OrdersTotal = Sum(order.Total) _
    Select cust.Name, Orders, OrdersTotal
```

Das Ergebnis der Abfrage ist eine Auflistung, dessen Elementtyp ein anonymer Typ mit den drei Eigenschaften ist: `Name`, als typisierte `String`, `Orders` als eine Auflistung, dessen Elementtyp `Order`, und `OrdersTotal`, typisierte als `Integer`. Ein `Group Join` -Abfrage-Operator wird nur unterstützt, wenn der Typ der Auflistung die Methode enthält:

```vb
Function GroupJoin(inner As CS, _
                         outerSelector As Func(Of T, K), _
                         innerSelector As Func(Of S, K), _
                         resultSelector As Func(Of T, CS, R)) As CR
```

Der code

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs _
            Group Join y in ys On x Equals y _
            Into g = Group _
            ...
```

wird in der Regel in übersetzt.

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.GroupJoin( _
        ys, _
        Function(x As Integer) x, _
        Function(y As Integer) y, _
        Function(x, group) New With {x, .g = group})...
```

__Beachten Sie.__ `Group`, `Join`, und `Into` sind keine reservierten Wörter.


## <a name="conditional-expressions"></a>Bedingte Ausdrücke

Eine bedingte `If` Ausdruck einen Ausdruck und gibt einen Wert zurück.

```antlr
ConditionalExpression
    : 'If' OpenParenthesis BooleanExpression Comma Expression Comma Expression CloseParenthesis
    | 'If' OpenParenthesis Expression Comma Expression CloseParenthesis
    ;
```

Im Gegensatz zu den `IIF` Laufzeitfunktion, jedoch ein bedingter Ausdruck nur wertet Operanden, die bei Bedarf. Also z. B. der Ausdruck `If(c Is Nothing, c.Name, "Unknown")` wird keine Ausnahme ausgelöst, wenn der Wert des `c` ist `Nothing`. Der bedingte Ausdruck weist zwei Formen: eine, die zwei Operanden und eine, die akzeptiert verfügt über drei Operanden.

Wenn drei Operanden bereitgestellt werden, müssen alle drei Ausdrücke als Werte klassifiziert, und der erste Operand muss ein boolescher Ausdruck sein. Wenn das Ergebnis des Ausdrucks ist "true", dann ist des zweiten Ausdrucks das Ergebnis des Operators, andernfalls der dritte Ausdruck werden das Ergebnis des Operators. Der Ergebnistyp des Ausdrucks ist der bestimmende Typ zwischen den Typen des zweiten und dritten Ausdrucks. Ist kein dominanter Typ, tritt ein Fehler während der Kompilierung.

Wenn zwei Operanden bereitgestellt werden, müssen beide Operanden als Werte klassifiziert, und der erste Operand muss ein Verweistyp oder ein Werttyp sein. Der Ausdruck `If(x, y)` wird dann ausgewertet, als ob der Ausdruck wurde `If(x IsNot Nothing, x, y)`, mit zwei Ausnahmen. Zuerst der erste Ausdruck wird immer nur ausgewertet, einmal und dann ein zweites, wenn der zweite Operand ein NULL-Werte ist und der erste Operand ist, die `?` wird aus dem Typ des ersten Operanden entfernt, wenn es sich bei den bestimmenden Typ für die Bestimmung der der Ergebnistyp des Ausdrucks. Zum Beispiel:

```vb
Module Test
    Sub Main()
        Dim x?, y As Integer
        Dim a?, b As Long

        a = If(x, a)        ' Result type: Long?
        y = If(x, 0)        ' Result type: Integer
    End Sub
End Module
```

In beiden Formen des Ausdrucks, wenn ein Operand `Nothing`, dessen Typ wird nicht verwendet, um zu bestimmen, den bestimmenden Typ. Wenn der Ausdruck `If(<expression>, Nothing, Nothing)`, der bestimmende Typ gilt `Object`.


## <a name="xml-literal-expressions"></a>XML-Literale Ausdrücke

Ein XML-Literalen Ausdruck stellt eine XML (eXtensible Markup Language) 1.0-Wert.

```antlr
XMLLiteralExpression
    : XMLDocument
    | XMLElement
    | XMLProcessingInstruction
    | XMLComment
    | XMLCDATASection
    ;
```

Das Ergebnis eines XML-Literalen Ausdrucks ist ein Wert, der als einen der Typen aus den `System.Xml.Linq` Namespace. Wenn die Typen in diesem Namespace nicht verfügbar sind, klicken Sie dann verursacht eine XML-Literalen Ausdrucks während der Kompilierung einen Fehler. Die Werte werden durch Konstruktoraufrufe aus der XML-Literale Ausdruck übersetzt generiert. Beispielsweise kann der Code:

```vb
Dim book As System.Xml.Linq.XElement = _
    <book title="My book"></book>
```

entspricht ungefähr der Code:

```vb
Dim book As System.Xml.Linq.XElement = _
    New System.Xml.Linq.XElement( _
        "book", _
        New System.Xml.Linq.XAttribute("title", "My book"))
```

Ein XML-Literalen Ausdruck kann es sich um die Form des XML-Dokument, ein XML-Element, eine XML-verarbeitungsanweisung, ein XML-Kommentar oder einem CDATA-Abschnitt haben.

__Beachten Sie.__ Diese Spezifikation enthält nur aus einer Beschreibung des XML-Codes, um das Verhalten von Visual Basic-Sprache zu beschreiben. Weitere Informationen zu XML finden Sie unter http://www.w3.org/TR/REC-xml/.


### <a name="lexical-rules"></a>Lexikalischen Regeln

```antlr
XMLCharacter
    : '<Unicode tab character (0x0009)>'
    | '<Unicode linefeed character (0x000A)>'
    | '<Unicode carriage return character (0x000D)>'
    | '<Unicode characters 0x0020 - 0xD7FF>'
    | '<Unicode characters 0xE000 - 0xFFFD>'
    | '<Unicode characters 0x10000 - 0x10FFFF>'
    ;

XMLString
    : XMLCharacter+
    ;

XMLWhitespace
    : XMLWhitespaceCharacter+
    ;

XMLWhitespaceCharacter
    : '<Unicode carriage return character (0x000D)>'
    | '<Unicode linefeed character (0x000A)>'
    | '<Unicode space character (0x0020)>'
    | '<Unicode tab character (0x0009)>'
    ;

XMLNameCharacter
    : XMLLetter
    | XMLDigit
    | '.'
    | '-'
    | '_'
    | ':'
    | XMLCombiningCharacter
    | XMLExtender
    ;

XMLNameStartCharacter
    : XMLLetter
    | '_'
    | ':'
    ;

XMLName
    : XMLNameStartCharacter XMLNameCharacter*
    ;

XMLLetter
    : '<Unicode character as defined in the Letter production of the XML 1.0 specification>'
    ;

XMLDigit
    : '<Unicode character as defined in the Digit production of the XML 1.0 specification>'
    ;

XMLCombiningCharacter
    : '<Unicode character as defined in the CombiningChar production of the XML 1.0 specification>'
    ;

XMLExtender
    : '<Unicode character as defined in the Extender production of the XML 1.0 specification>'
    ;
```

XML-Literale Ausdrücke werden mit den lexikalischen Regeln der XML anstelle von den lexikalischen Regeln der regulären Visual Basic-Code interpretiert. Die zwei Sätze von Regeln unterscheiden sich in der Regel auf folgende Weise:

* Leerraum ist wichtig, im XML-Format. Daher gibt die Grammatik für XML-Literale Ausdrücke explizit an, in denen Leerraum zulässig ist. Leerzeichen werden nicht beibehalten, außer wenn es im Kontext von Zeichendaten in einem Element auftritt. Zum Beispiel:

  ```vb
  ' The following element preserves no whitespace
  Dim e1 = _
      <customer>
          <name>Bob</>
      </>

  ' The following element preserves all of the whitespace
  Dim e2 = _
      <customer>
          Bob
      </>
  ```

* XML-End-of-Line-Leerraum wird anhand der XML-Spezifikation normalisiert.

* Bei XML muss die Groß- und Kleinschreibung beachtet werden. Schlüsselwörter müssen die Groß-/Kleinschreibung genau übereinstimmen. andernfalls tritt ein Fehler während der Kompilierung auf.

* Zeilenabschlusszeichen werden Leerzeichen in XML-Datei berücksichtigt. Daher sind keine Zeilenfortsetzungszeichen in XML-Literale Ausdrücke erforderlich.

* XML akzeptiert keine Zeichen voller Breite. Wenn Zeichen voller Breite verwendet werden, wird ein Fehler während der Kompilierung auftreten.


### <a name="embedded-expressions"></a>Eingebettete Ausdrücke

XML-Literale Ausdrücke dürfen *eingebetteten Ausdrücken*. Ein eingebetteter Ausdruck ist eine Visual Basic-Ausdruck, der ausgewertet und eine oder mehrere Werte an der Position des eingebetteten Ausdrucks verwendet.

```antlr
XMLEmbeddedExpression
    : '<' '%' '=' LineTerminator? Expression LineTerminator? '%' '>'
    ;
```

Der folgende Code wird z. B. die Zeichenfolge `John Smith` als Wert für das XML-Element:

```vb
Dim name as String = "John Smith"
Dim element As System.Xml.Linq.XElement = <customer><%= name %></customer>
```

Ausdrücke können in verschiedenen Kontexten eingebettet werden. Der folgende Code erzeugt z. B. ein Element mit dem Namen `customer`:

```vb
Dim name As String = "customer"
Dim element As System.Xml.Linq.XElement = <<%= name %>>John Smith</>
```

Jeder Kontext, in denen ein eingebetteter Ausdruck verwendet werden kann, gibt die Typen, die akzeptiert werden. Wenn im Kontext der Ausdruck, der Teil eines eingebetteten Ausdrucks können müssen die normalen lexikalischen Regeln für Visual Basic-Code gelten also weiterhin, z. B. zeilenfortsetzungen verwendet werden:

```vb
' Visual Basic expression uses line continuation, XML does not
Dim element As System.Xml.Linq.XElement = _
    <<%= name & _
          name %>>John 
                     Smith</>
```


### <a name="xml-documents"></a>XML-Dokumenten

```antlr
XMLDocument
    : XMLDocumentPrologue XMLMisc* XMLDocumentBody XMLMisc*
    ;

XMLDocumentPrologue
    : '<' '?' 'xml' XMLVersion XMLEncoding? XMLStandalone? XMLWhitespace? '?' '>'
    ;

XMLVersion
    : XMLWhitespace 'version' XMLWhitespace? '=' XMLWhitespace? XMLVersionNumberValue
    ;

XMLVersionNumberValue
    : SingleQuoteCharacter '1' '.' '0' SingleQuoteCharacter
    | DoubleQuoteCharacter '1' '.' '0' DoubleQuoteCharacter
    ;

XMLEncoding
    : XMLWhitespace 'encoding' XMLWhitespace? '=' XMLWhitespace? XMLEncodingNameValue
    ;

XMLEncodingNameValue
    : SingleQuoteCharacter XMLEncodingName SingleQuoteCharacter
    | DoubleQuoteCharacter XMLEncodingName DoubleQuoteCharacter
    ;

XMLEncodingName
    : XMLLatinAlphaCharacter XMLEncodingNameCharacter*
    ;

XMLEncodingNameCharacter
    : XMLUnderscoreCharacter
    | XMLLatinAlphaCharacter
    | XMLNumericCharacter
    | XMLPeriodCharacter
    | XMLDashCharacter
    ;

XMLLatinAlphaCharacter
    : '<Unicode Latin alphabetic character (0x0041-0x005a, 0x0061-0x007a)>'
    ;

XMLNumericCharacter
    : '<Unicode digit character (0x0030-0x0039)>'
    ;

XMLHexNumericCharacter
    : XMLNumericCharacter
    | '<Unicode Latin hex alphabetic character (0x0041-0x0046, 0x0061-0x0066)>'
    ;

XMLPeriodCharacter
    : '<Unicode period character (0x002e)>'
    ;

XMLUnderscoreCharacter
    : '<Unicode underscore character (0x005f)>'
    ;

XMLDashCharacter
    : '<Unicode dash character (0x002d)>'
    ;

XMLStandalone
    : XMLWhitespace 'standalone' XMLWhitespace? '=' XMLWhitespace? XMLYesNoValue
    ;

XMLYesNoValue
    : SingleQuoteCharacter XMLYesNo SingleQuoteCharacter
    | DoubleQuoteCharacter XMLYesNo DoubleQuoteCharacter
    ;

XMLYesNo
    : 'yes'
    | 'no'
    ;

XMLMisc
    : XMLComment
    | XMLProcessingInstruction
    | XMLWhitespace
    ;

XMLDocumentBody
    : XMLElement
    | XMLEmbeddedExpression
    ;
```

Ein XML-Dokument einen Wert, der als ergibt `System.Xml.Linq.XDocument`. Im Gegensatz zu XML 1.0-Spezifikation werden XML-Dokumenten in XML-Literale Ausdrücke erforderlich, um den XML-Dokument Prolog angeben; XML-Literale Ausdrücke ohne den Prolog des XML-Dokument sind als die einzelne Entität interpretiert. Zum Beispiel:

```vb
Dim doc As System.Xml.Linq.XDocument = _
    <?xml version="1.0"?>
    <?instruction?>
    <customer>Bob</>

Dim pi As System.Xml.Linq.XProcessingInstruction = _
    <?instruction?>
```

Ein XML-Dokument kann einen eingebetteten Ausdruck enthalten, dessen Typ auf einen beliebigen Typ sein kann. zur Laufzeit jedoch das Objekt muss den Anforderungen entsprechen den `XDocument` Konstruktor oder einen Laufzeitfehler tritt auf.

Im Gegensatz zu regulären XML unterstützen Ausdrücke für XML-Dokument keine DTDs (Document Type Deklarationen). Darüber hinaus wird das encoding-Attribut, wenn angegeben, ignoriert, da die Codierung des XML-Literalen Ausdrucks immer identisch mit der Codierung der Quelldatei selbst ist.

__Beachten Sie.__ Obwohl das encoding-Attribut ignoriert wird, ist es noch gültig-Attribut, um die Möglichkeit, enthalten keine gültige Xml 1.0-Dokumente im Quellcode zu gewährleisten.


### <a name="xml-elements"></a>XML-Elemente

```antlr
XMLElement
    : XMLEmptyElement
    | XMLElementStart XMLContent XMLElementEnd
    ;

XMLEmptyElement
    : '<' XMLQualifiedNameOrExpression XMLAttribute* XMLWhitespace? '/' '>'
    ;

XMLElementStart
    : '<' XMLQualifiedNameOrExpression XMLAttribute* XMLWhitespace? '>'
    ;

XMLElementEnd
    : '<' '/' '>'
    | '<' '/' XMLQualifiedName XMLWhitespace? '>'
    ;

XMLContent
    : XMLCharacterData? ( XMLNestedContent XMLCharacterData? )+
    ;

XMLCharacterData
    : '<Any XMLCharacterDataString that does not contain the string "]]>">'
    ;

XMLCharacterDataString
    : '<Any Unicode character except < or &>'+
    ;

XMLNestedContent
    : XMLElement
    | XMLReference
    | XMLCDATASection
    | XMLProcessingInstruction
    | XMLComment
    | XMLEmbeddedExpression
    ;

XMLAttribute
    : XMLWhitespace XMLAttributeName XMLWhitespace? '=' XMLWhitespace? XMLAttributeValue
    | XMLWhitespace XMLEmbeddedExpression
    ;

XMLAttributeName
    : XMLQualifiedNameOrExpression
    | XMLNamespaceAttributeName
    ;

XMLAttributeValue
    : DoubleQuoteCharacter XMLAttributeDoubleQuoteValueCharacter* DoubleQuoteCharacter
    | SingleQuoteCharacter XMLAttributeSingleQuoteValueCharacter* SingleQuoteCharacter
    | XMLEmbeddedExpression
    ;

XMLAttributeDoubleQuoteValueCharacter
    : '<Any XMLCharacter except <, &, or DoubleQuoteCharacter>'
    | XMLReference
    ;

XMLAttributeSingleQuoteValueCharacter
    : '<Any XMLCharacter except <, &, or SingleQuoteCharacter>'
    | XMLReference
    ;

XMLReference
    : XMLEntityReference
    | XMLCharacterReference
    ;

XMLEntityReference
    : '&' XMLEntityName ';'
    ;

XMLEntityName
    : 'lt' | 'gt' | 'amp' | 'apos' | 'quot'
    ;

XMLCharacterReference
    : '&' '#' XMLNumericCharacter+ ';'
    | '&' '#' 'x' XMLHexNumericCharacter+ ';'
    ;
```

Ein XML-Element einen Wert, der als ergibt `System.Xml.Linq.XElement`. Im Gegensatz zu regulären XML XML-Elemente können die Namen in das schließende Tag weglassen, und das aktuelle Element für die meisten geschachtelten wird geschlossen. Zum Beispiel:

```vb
Dim name = <name>Bob</>
```

Deklarationen in einer XML-Element-Ergebnis in Werte vom Typ Attribut `System.Xml.Linq.XAttribute`. Attributwerte werden gemäß der XML-Spezifikation normalisiert. Wenn der Wert eines Attributs ist `Nothing` das Attribut nicht erstellt werden, sodass der Attribut-Wert-Ausdruck nicht überprüft werden soll `Nothing`. Zum Beispiel:

```vb
Dim expr = Nothing

' Throws null argument exception
Dim direct = New System.Xml.Linq.XElement( _
    "Name", _
    New System.Xml.Linq.XAttribute("Length", expr))

' Doesn't throw exception, the result is <Name/>
Dim literal = <Name Length=<%= expr %>/>
```

XML-Elemente und Attribute können geschachtelte Ausdrücke in den folgenden Orten enthalten:

Der Name des Elements in diesem Fall den eingebetteten Ausdruck muss einen Wert eines Typs, der implizit in sein `System.Xml.Linq.XName`. Zum Beispiel:

```vb
Dim name = <<%= "name" %>>Bob</>
```

Der Name eines Attributs des Elements in diesem Fall den eingebetteten Ausdruck muss einen Wert eines Typs, der implizit in sein `System.Xml.Linq.XName`. Zum Beispiel:

```vb
Dim name = <name <%= "length" %>="3">Bob</>
```

Der Wert eines Attributs des Elements in diesem Fall den eingebetteten Ausdruck einen Wert eines beliebigen Typs sein kann. Zum Beispiel:

```vb
Dim name = <name length=<%= 3 %>>Bob</>
```

Ein Attribut des Elements in diesem Fall den eingebetteten Ausdruck einen Wert eines beliebigen Typs sein kann. Zum Beispiel:

```vb
Dim name = <name <%= new XAttribute("length", 3) %>>Bob</>
```

Der Inhalt des Elements in diesem Fall den eingebetteten Ausdruck einen Wert eines beliebigen Typs sein kann. Zum Beispiel:

```vb
Dim name = <name><%= "Bob" %></>
```

Wenn der Typ des eingebetteten Ausdrucks `Object()`, wird das Array als ein Paramarray zum Übergeben der `XElement` Konstruktor.


### <a name="xml-namespaces"></a>XML-Namespaces

XML-Elemente können XML-Namespace-Deklarationen, enthalten, wie in der XML-Namespaces-1.0-Spezifikation definiert.

```antlr
XMLNamespaceAttributeName
    : XMLPrefixedNamespaceAttributeName
    | XMLDefaultNamespaceAttributeName
    ;

XMLPrefixedNamespaceAttributeName
    : 'xmlns' ':' XMLNamespaceName
    ;

XMLDefaultNamespaceAttributeName
    : 'xmlns'
    ;

XMLNamespaceName
    : XMLNamespaceNameStartCharacter XMLNamespaceNameCharacter*
    ;

XMLNamespaceNameStartCharacter
    : '<Any XMLNameCharacter except :>'
    ;

XMLNamespaceNameCharacter
    : XMLLetter
    | '_'
    ;

XMLQualifiedNameOrExpression
    : XMLQualifiedName
    | XMLEmbeddedExpression
    ;

XMLQualifiedName
    : XMLPrefixedName
    | XMLUnprefixedName
    ;

XMLPrefixedName
    : XMLNamespaceName ':' XMLNamespaceName
    ;

XMLUnprefixedName
    : XMLNamespaceName
    ;
```

Die Einschränkungen für die Namespaces definieren `xml` und `xmlns` werden erzwungen, und Fehler während der Kompilierung erzeugt. XML-Namespacedeklaration dürfen keinen eingebetteten Ausdruck für ihren Wert besitzen. Der angegebene Wert muss es sich um ein nicht leeres Zeichenfolgenliteral sein. Zum Beispiel:

```vb
' Declares a valid namespace
Dim customer = <db:customer xmlns:db="http://example.org/database">Bob</>

' Error: xmlns cannot be re-defined
Dim bad1 = <elem xmlns:xmlns="http://example.org/namespace"/>

' Error: cannot have an embedded expression
Dim bad2 = <elem xmlns:db=<%= "http://example.org/database" %>>Bob</>
```

__Beachten Sie.__ Diese Spezifikation enthält nur genug eine Beschreibung der XML-Namespace, um das Verhalten von Visual Basic-Sprache zu beschreiben. Weitere Informationen zu XML-Namespaces finden Sie unter http://www.w3.org/TR/REC-xml-names/.

XML-Namen für Element- und Attributnamen können mithilfe von Namespacenamen qualifiziert werden. Namespaces wie reguläre XML-Code gebunden sind, werden mit der Ausnahme, die keinem Namespace importiert, die auf Dateiebene deklariert deklariert werden, in einem Kontext einschließen der Deklaration, die selbst von alle Namespace-Importe deklariert, indem die Kompilierung eingeschlossen wird berücksichtigt Umgebung. Wenn ein Namespacename kann nicht gefunden werden, tritt ein Fehler während der Kompilierung. Zum Beispiel:

```vb
Imports System.Xml.Linq
Imports <xmlns:db="http://example.org/database">

Module Test
    Sub Main()
        ' Binds to the imported namespace above.
        Dim c1 = <db:customer>Bob</>

        ' Binds to the namespace declaration in the element
        Dim c2 = _
            <db:customer xmlns:db="http://example.org/database-other">Mary</>

        ' Binds to the inner namespace declaration
        Dim c3 = _
            <database xmlns:db="http://example.org/database-one">
                <db:customer xmlns:db="http://example.org/database-two">Joe</>
            </>

        ' Error: namespace db2 cannot be found
        Dim c4 = _
            <db2:customer>Jim</>
    End Sub
End Module
```

XML-Namespaces in einem Element deklariert gelten nicht für XML-Literale in eingebettete Ausdrücke. Zum Beispiel:

```vb
' Error: Namespace prefix 'db' is not declared
Dim customer = _
    <db:customer xmlns:db="http://example.org/database">
        <%= <db:customer>Bob</> %>
    </>
```

__Beachten Sie.__ Das liegt der eingebettete Ausdruck einschließlich eines Funktionsaufrufs sein kann. Wenn der Funktionsaufruf einen XML-Literalen Ausdruck enthalten, ist es nicht klar, ob die Programmierer erwarten den XML-Namespace, angewendet oder ignoriert werden soll.


### <a name="xml-processing-instructions"></a>XML-Verarbeitungsanweisungen

Eine XML-verarbeitungsanweisung ergibt einen Wert, der als `System.Xml.Linq.XProcessingInstruction`. XML-verarbeitungsanweisungen können nicht eingebettete Ausdrücke enthalten, da sie gültige Syntax in die verarbeitungsanweisung sind.

```antlr
XMLProcessingInstruction
    : '<' '?' XMLProcessingTarget ( XMLWhitespace XMLProcessingValue? )? '?' '>'
    ;

XMLProcessingTarget
    : '<Any XMLName except a casing permutation of the string "xml">'
    ;

XMLProcessingValue
    : '<Any XMLString that does not contain a question-mark followed by ">">'
    ;
```

### <a name="xml-comments"></a>XML-Kommentare

Ein XML-Kommentar ergibt einen Wert, der als `System.Xml.Linq.XComment`. XML-Kommentare können nicht eingebettete Ausdrücke enthalten, wie gültige Syntax innerhalb des Kommentars.

```antlr
XMLComment
    : '<' '!' '-' '-' XMLCommentCharacter* '-' '-' '>'
    ;

XMLCommentCharacter
    : '<Any XMLCharacter except dash (0x002D)>'
    | '-' '<Any XMLCharacter except dash (0x002D)>'
    ;
```

### <a name="cdata-sections"></a>CDATA-Abschnitte

Ein CDATA-Abschnitt ergibt einen Wert, der als `System.Xml.Linq.XCData`. CDATA-Abschnitte können nicht eingebettete Ausdrücke enthalten, wie sie gültige Syntax innerhalb des CDATA-Abschnitts sind.

```antlr
XMLCDATASection
    : '<' '!' ( 'CDATA' '[' XMLCDATASectionString? ']' )? '>'
    ;

XMLCDATASectionString
    : '<Any XMLString that does not contain the string "]]>">'
    ;
```

## <a name="xml-member-access-expressions"></a>XML-Memberzugriffsausdrücke

Ein XML-Memberzugriffsausdruck greift auf die Elemente eines XML-Werts.

```antlr
XMLMemberAccessExpression
    : Expression '.' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? IdentifierOrKeyword
    | Expression '.' '.' '.' LineTerminator? '<' XMLQualifiedName '>'
    ;
```

Es gibt drei Arten von XML-Memberzugriffsausdrücke:

* *Elementzugriff*, in dem ein XML-Name einen einzigen Punkt folgt. Zum Beispiel:

  ```vb
  Dim customer = _
      <customer>
          <name>Bob</>
      </>
  Dim customerName = customer.<name>.Value
  ```

  Elementzugriff wird an die Funktion:

  ```vb
  Function Elements(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XNode)
  ```

  Damit das obige Beispiel entspricht:

  ```vb
  Dim customerName = customer.Elements("name").Value
  ```

* *Attribut Zugriff*, in dem ein Visual Basic-Bezeichner einen Punkt folgt und eine Anmeldung oder eine XML-Name einen Punkt folgt und ein at-Zeichen. Zum Beispiel:

  ```vb
  Dim customer = <customer age="30"/>
  Dim customerAge = customer.@age
  ```

  Attribut-Access-Karten für die Funktion:

  ```vb
  Function AttributeValue(name As System.Xml.Linq.XName) as String
  ```

  Damit das obige Beispiel entspricht:

  ```vb
  Dim customerAge = customer.AttributeValue("age")
  ```

  __Beachten Sie.__ Die `AttributeValue` Erweiterungsmethode (sowie die zugehörigen Erweiterungseigenschaft `Value`) derzeit nicht in einer beliebigen Assembly definiert ist. Wenn die Erweiterung Elemente erforderlich sind, werden sie automatisch in der Assembly erstellt wird definiert.

* *Zugriff für Nachfolger*, in dem ein XML-Namen die drei Punkte folgt. Zum Beispiel:

  ```vb
  Dim company = _
      <company>
          <customers>
              <customer>Bob</>
              <customer>Mary</>
              <customer>Joe</>
          </>
      </>
  Dim customers = company...<customer>
  ```

  Zugriff für Nachfolger ordnet die Funktion:

  ```vb
  Function Descendents(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XElement)
  ```

  Damit das obige Beispiel entspricht:

  ```vb
  Dim customers = company.Descendants("customer")
  ```

Der Basis einer XML-Memberzugriffsausdruck Ausdruck muss ein Wert sein und muss vom Typ sein:

* Wenn ein Element oder ein Nachfolger zugreifen zu können, `System.Xml.Linq.XContainer` oder eines abgeleiteten Typs oder `System.Collections.Generic.IEnumerable(Of T)` oder eines abgeleiteten Typs, in denen `T` ist `System.Xml.Linq.XContainer` oder einen abgeleiteten Typ.

* Wenn einer Zugriff auf das Attribut `System.Xml.Linq.XElement` oder eines abgeleiteten Typs oder `System.Collections.Generic.IEnumerable(Of T)` oder eines abgeleiteten Typs, in denen `T` ist `System.Xml.Linq.XElement` oder einen abgeleiteten Typ.

Namen in XML-Memberzugriffsausdrücke darf nicht leer sein. Sie können den Namespace qualifiziert, mit der alle Namespaces, die durch Importe definiert werden. Zum Beispiel:

```vb
Imports <xmlns:db="http://example.org/database">

Module Test
    Sub Main()
        Dim customer = _
            <db:customer>
                <db:name>Bob</>
            </>
        Dim name = customer.<db:name>
    End Sub
End Module
```

Nach der Dot(s) in eine XML-Memberzugriffsausdruck oder zwischen den spitzen Klammern und dem Namen sind keine Leerzeichen zugelassen. Zum Beispiel:

```vb
Dim customer = _
    <customer age="30">
        <name>Bob</>
    </>
' All the following are error cases
Dim age = customer.@ age
Dim name = customer.< name >
Dim names = customer...< name >
```

Wenn die Typen in der `System.Xml.Linq` Namespace sind nicht verfügbar, und klicken Sie dann eine XML-Memberzugriffsausdruck führt dazu, dass einen Fehler während der Kompilierung.


## <a name="await-operator"></a>Await-Operator

Der Operator "await" bezieht sich auf asynchrone Methoden, die im Abschnitt beschriebenen [Async-Methoden](statements.md#async-methods).

```antlr
AwaitOperatorExpression
    : 'Await' Expression
    ;
```

`Await` ist ein reserviertes Wort, verfügt die unmittelbar einschließende Methode oder der Lambda-Ausdruck, in dem er angezeigt wird, ein `Async` Modifizierer, und wenn die `Await` angezeigt wird, `Async` Modifizierer; es an anderer Stelle nicht reserviert ist. Es ist auch in präprozessoranweisungen nicht reserviert. Der Await-Operator darf nur im Text einer Methode oder einem Lambda-Ausdrücke, die, in denen es sich um ein reserviertes Wort ist. In der unmittelbar einschließenden Methode oder einem Lambdaausdruck, ein Await-Ausdruck kann nicht auftreten, innerhalb des Texts einer `Catch` oder `Finally` Block noder innerhalb der Text der ein `SyncLock` Anweisung noch innerhalb eines Abfrageausdrucks.

Der Operator "await" nimmt einen einzelnen Ausdruck, der als Wert klassifiziert werden muss und, dessen Typ muss, eine *awaitable* Typ oder `Object`. Wenn der Typ ist `Object` und klicken Sie dann die gesamte Verarbeitung bis zur Laufzeit verzögert wird. Ein Typ `C` gilt als "awaitable", wenn alle der folgenden Bedingungen erfüllt sind:

* `C` enthält eine zugängliche Instanz- oder Erweiterungsmethode-Methode, mit dem Namen `GetAwaiter` die verfügt über keine Argumente und womit eine Art `E`;

* `E` enthält eine lesbare Instanz- oder Erweiterungsmethode-Eigenschaft, mit dem Namen `IsCompleted` der akzeptiert keine Argumente und weist den Typ Boolean;

* `E` enthält eine zugängliche Instanz- oder Erweiterungsmethode-Methode, mit dem Namen `GetResult` der akzeptiert keine Argumente.

* `E` implementiert eine `System.Runtime.CompilerServices.INotifyCompletion` oder `ICriticalNotifyCompletion`.

Wenn `GetResult` wurde eine `Sub`, und klicken Sie dann die "await"-Ausdruck als "void" eingestuft wird. Andernfalls wird der Await-Ausdruck als Wert klassifiziert und sein Typ ist der Rückgabetyp der der `GetResult` Methode.

Hier ist ein Beispiel für eine Klasse, die erwartet werden kann:

```vb
Class MyTask(Of T)
    Function GetAwaiter() As MyTaskAwaiter(Of T)
        Return New MyTaskAwaiter With {.m_Task = Me}
    End Function

    ...
End Class

Structure MyTaskAwaiter(Of T)
    Implements INotifyCompletion

    Friend m_Task As MyTask(Of T)

    ReadOnly Property IsCompleted As Boolean
        Get
            Return m_Task.IsCompleted
        End Get
    End Property

    Sub OnCompleted(r As Action) Implements INotifyCompletion.OnCompleted
        ' r is the "resumptionDelegate"
        Dim sc = SynchronizationContext.Current
        If sc Is Nothing Then
            m_Task.ContinueWith(Sub() r())
        Else
            m_Task.ContinueWith(Sub() sc.Post(Sub() r(), Nothing))
        End If
    End Sub

    Function GetResult() As T
        If m_Task.IsCanceled Then Throw New TaskCanceledException(m_Task)
        If m_Task.IsFaulted Then Throw m_Task.Exception.InnerException
        Return m_Task.Result
    End Function
End Structure
```

__Beachten Sie.__ Autoren von Klassenbibliotheken werden empfohlen, die dem Muster folgen, bereit, die sie auf dem gleichen Fortsetzungsdelegaten `SynchronizationContext` als ihre `OnCompleted` selbst auf aufgerufen wurde. Darüber hinaus der Wiederaufnahmedelegat sollte nicht ausgeführt werden synchron innerhalb der `OnCompleted` Methode, da, die auf einen Stapelüberlauf führen kann: stattdessen den Delegat sollte in der Warteschlange zur nachfolgenden Ausführung.

Wenn Steuerung Flow erreicht eine `Await` Operator Verhalten lautet wie folgt.

1.  Die `GetAwaiter` Methode des "await"-Operanden wird aufgerufen. Das Ergebnis dieser Aufruf wird als bezeichnet die *"awaiter"*.

2.  Der "awaiter" `IsCompleted` Eigenschaft wird abgerufen. Wenn das Ergebnis "true" ist dann:

    21. Die `GetResult` Methode von der "awaiter" wird aufgerufen. Wenn `GetResult` war dies eine Funktion, und klicken Sie dann der Wert des der Await-Ausdruck dieser Funktion zurückgegeben wird.

3.  Wenn die IsCompleted-Eigenschaft nicht "true" ist dann:

    31. Entweder `ICriticalNotifyCompletion.UnsafeOnCompleted` im "awaiter" aufgerufen wird (wenn der "awaiter"-Typ `E` implementiert `ICriticalNotifyCompletion`) oder `INotifyCompletion.OnCompleted` (andernfalls). In beiden Fällen wird es übergibt eine *Wiederaufnahmedelegat* verknüpft ist, mit der aktuellen Instanz der Async-Methode.

    32. Der Kontrollpunkt der aktuellen Instanz der Async-Methode wird angehalten, und Steuern der Flow wird fortgesetzt, in der *aktuellen Aufrufer* (im Abschnitt definiert [Async-Methoden](statements.md#async-methods)).

    33. Wenn später der Wiederaufnahmedelegat aufgerufen wird,

        331. zuerst stellt der Wiederaufnahmedelegat wieder her `System.Threading.Thread.CurrentThread.ExecutionContext` war zum Zeitpunkt `OnCompleted` aufgerufen wurde,
        332. und dann die ablaufsteuerung an den Kontrollpunkt der asynchronen Methodeninstanz fortgesetzt (siehe Abschnitt [Async-Methoden](statements.md#async-methods)),
        333. Ruft, in dem die `GetResult` Methode der "awaiter", wie in der oben genannten 2.1.

Wenn der Operand des "await" Typs "Object" aufweist, wird dieses Verhalten bis zur Laufzeit verzögert:

- Schritt 1 wird durch die aufrufende GetAwaiter() ohne Argumente erreicht; kann daher zur Laufzeit, um Instanzmethoden bindet, bindet es die optionalen Parameter zu akzeptieren.
- Schritt 2 erfolgt durch Abrufen der Eigenschaft IsCompleted() ohne Argumente und es wird versucht, eine Konvertierung in einen booleschen Wert.
- Schritt 3.a wird erreicht, indem Sie versuchen, `TryCast(awaiter, ICriticalNotifyCompletion)`, wenn auch dieser dann `DirectCast(awaiter, INotifyCompletion)`.

Der Wiederaufnahmedelegat 3.a übergeben kann nur einmal aufgerufen werden. Wenn sie mehr als einmal aufgerufen wird, ist das Verhalten nicht definiert.

