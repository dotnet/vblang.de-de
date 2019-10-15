---
ms.openlocfilehash: 8f250f8cee957fa60e7970ace250e8d7ce145fd7
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306170"
---
# <a name="expressions"></a>Ausdrücke

Ein Ausdruck ist eine Sequenz von Operatoren und Operanden, die eine Berechnung eines Werts angibt oder eine Variable oder Konstante bezeichnet. In diesem Kapitel werden die Syntax, die Reihenfolge der Auswertung von Operanden und Operatoren sowie die Bedeutung von Ausdrücken definiert.

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

Jeder Ausdruck wird als einer der folgenden klassifiziert:

* *Ein-Wert.* Jeder Wert verfügt über einen zugeordneten Typ.

* *Eine Variable.* Jede Variable verfügt über einen zugeordneten Typ, nämlich den deklarierten Typ der Variablen.

* *Ein Namespace.* Ein Ausdruck mit dieser Klassifizierung kann nur als linke Seite eines Element Zugriffs angezeigt werden. In jedem anderen Kontext verursacht ein Ausdruck, der als Namespace klassifiziert ist, einen Kompilierzeitfehler.

* *Ein-Typ.* Ein Ausdruck mit dieser Klassifizierung kann nur als linke Seite eines Element Zugriffs angezeigt werden. In jedem anderen Kontext verursacht ein Ausdruck, der als Typ klassifiziert ist, einen Kompilierzeitfehler.

* *Eine Methoden Gruppe,* bei der es sich um einen Satz von Methoden handelt, die mit demselben Namen überladen werden. Eine Methoden Gruppe kann über einen zugeordneten Ziel Ausdruck und eine zugeordnete Typargument Liste verfügen.

* *Ein Methoden Zeiger,* der den Speicherort einer Methode darstellt. Einem Methoden Zeiger können ein zugeordneter Ziel Ausdruck und eine zugeordnete Typargument Liste zugeordnet sein.

* *Eine Lambda-Methode,* bei der es sich um eine anonyme Methode handelt.

* *Eine Eigenschaften Gruppe,* bei der es sich um einen Satz von Eigenschaften handelt, die mit demselben Namen überladen werden. Einer Eigenschaften Gruppe kann ein zugeordneter Ziel Ausdruck zugeordnet sein.

* *Ein Eigenschaften Zugriff.* Jeder Eigenschaften Zugriff verfügt über einen zugeordneten Typ, nämlich den Typ der Eigenschaft. Ein Eigenschaften Zugriff kann über einen zugeordneten Ziel Ausdruck verfügen.

* *Ein spät gebundener Zugriff,* der den Zugriff auf eine Methode oder Eigenschaft darstellt, der bis zur Laufzeit verzögert wird. Einem spät gebundenen Zugriff können ein zugeordneter Ziel Ausdruck und eine zugeordnete Typargument Liste zugeordnet werden. Der Typ eines spät gebundenen Zugriffs ist immer `Object`.

* *Ein Ereignis Zugriff.* Jedem Ereignis Zugriff ist ein Typ zugeordnet, nämlich der Typ des Ereignisses. Ein Ereignis Zugriff kann über einen zugeordneten Ziel Ausdruck verfügen. Ein Ereignis Zugriff wird möglicherweise als erstes Argument der Anweisungen `RaiseEvent`, `AddHandler` und `RemoveHandler` angezeigt. In jedem anderen Kontext verursacht ein Ausdruck, der als Ereignis Zugriff klassifiziert ist, einen Kompilierzeitfehler.

* *Ein arrayliteralwert,* der die Anfangswerte eines Arrays darstellt, dessen Typ noch nicht bestimmt wurde.

* *Blutung.* Dies tritt auf, wenn der Ausdruck ein Aufruf einer Unterroutine oder ein Erwartungs Operator Ausdruck ohne Ergebnis ist. Ein als void klassifiziert Ausdruck ist nur im Kontext einer Aufruf Anweisung oder einer Erwartungs Anweisung gültig.

* *Ein Standardwert.* Nur die Literale `Nothing` erzeugt diese Klassifizierung.

Das Endergebnis eines Ausdrucks ist in der Regel ein Wert oder eine Variable, wobei die anderen Kategorien von Ausdrücken als Zwischenwerte fungieren, die nur in bestimmten Kontexten zulässig sind.

Beachten Sie, dass Ausdrücke, deren Typ ein Typparameter ist, in Anweisungen und Ausdrücken verwendet werden können, die erfordern, dass der Typ eines Ausdrucks bestimmte Merkmale aufweist (z. b. als Verweistyp, Werttyp, ableiten von einem Typ usw.), wenn die Einschränkungen auferlegt werden. Diese Eigenschaften werden vom Typparameter erfüllt.

### <a name="expression-reclassification"></a>Neuklassifizierung von Ausdrücken

Normalerweise tritt ein Kompilierzeitfehler auf, wenn ein Ausdruck in einem Kontext verwendet wird, der eine Klassifizierung erfordert, die sich von der des Ausdrucks unterscheidet, z. b. wenn versucht wird, einen Wert einem Literalwert zuzuweisen. In vielen Fällen ist es jedoch möglich, die Klassifizierung eines Ausdrucks durch den Prozess der *Neuklassifizierung*zu ändern.

Wenn die Neuklassifizierung erfolgreich ist, wird die Neuklassifizierung als Erweiterung oder Einschränkung bewertet. Sofern nicht anders angegeben, werden alle Neuklassifizierungen in dieser Liste erweitert.

Die folgenden Typen von Ausdrücken können neu klassifiziert werden:

* Eine Variable kann als Wert neu klassifiziert werden. Der in der Variable gespeicherte Wert wird abgerufen.

* Eine Methoden Gruppe kann als Wert neu klassifiziert werden. Der Methoden Gruppen Ausdruck wird als Aufruf Ausdruck mit dem zugeordneten Ziel Ausdruck und der Typparameter Liste und leeren Klammern interpretiert (d. h. `f` wird als `f()` interpretiert, und `f(Of Integer)` wird als `f(Of Integer)()` interpretiert). Diese Neuklassifizierung kann dazu führen, dass der Ausdruck weiter als void neu klassifiziert wird.

* Ein Methoden Zeiger kann als Wert neu klassifiziert werden. Diese Neuklassifizierung kann nur im Kontext einer Konvertierung auftreten, bei der der Zieltyp bekannt ist. Der Methoden Zeiger Ausdruck wird als Argument für einen delegatinstanzizierungsausdruck des entsprechenden Typs mit der zugeordneten Typargument Liste interpretiert. Zum Beispiel:
    
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

* Eine Lambda-Methode kann als Wert neu klassifiziert werden. Wenn die Neuklassifizierung im Kontext einer Konvertierung erfolgt, bei der der Zieltyp bekannt ist, kann eine von zwei Neuklassifizierungen auftreten:
    
  1. Wenn der Zieltyp ein Delegattyp ist, wird die Lambda-Methode als Argument für einen delegatkonstruktions-Ausdruck des entsprechenden Typs interpretiert.
    
  2. Wenn der Zieltyp `System.Linq.Expressions.Expression(Of T)` und `T` ein Delegattyp ist, wird die Lambda-Methode so interpretiert, als ob Sie im delegaterstellungs-Ausdruck für `T` verwendet und anschließend in eine Ausdrucks Baumstruktur konvertiert wurde.
    
  Eine Async-oder Iterator-Lambda-Methode kann nur als Argument für einen delegatkonstruktions-Ausdruck interpretiert werden, wenn der Delegat keine ByRef-Parameter aufweist.
    
  Wenn die Konvertierung von einem der Parametertypen eines Delegaten in die entsprechenden Lambda-Parametertypen eine einschränkende Konvertierung ist, wird die Neuklassifizierung als Einschränkungs Methode bewertet. Andernfalls wird Sie erweitert.
    
  __Nebenbei.__ Die genaue Übersetzung zwischen Lambda-Methoden und Ausdrucks Baumstrukturen wird möglicherweise nicht Zwischenversionen des Compilers korrigiert und geht über den Rahmen dieser Spezifikation hinaus. Für Microsoft Visual Basic 11,0 können alle Lambda-Ausdrücke in Ausdrucks Baumstrukturen konvertiert werden, die den folgenden Einschränkungen unterliegen: (1) 1.  Nur einzeilige Lambda-Ausdrücke ohne ByRef-Parameter können in Ausdrucks Baumstrukturen konvertiert werden. Der einzeiligen `Sub`-Lambdas können nur Aufruf Anweisungen in Ausdrucks Baumstrukturen konvertiert werden. (2) anonyme typausdrücke können nicht in Ausdrucks Baumstrukturen konvertiert werden, wenn ein früherer Feldinitialisierer verwendet wird, um einen nachfolgenden Feldinitialisierer zu initialisieren, z. b. `New With {.a=1, .b=.a}`. (3) objektinitialisiererausdrücke können nicht in Ausdrucks Baumstrukturen konvertiert werden, wenn ein Member des aktuellen Objekts, das initialisiert wird, in einem der Feldinitialisierer verwendet wird, z. b. `New C1 With {.a=1, .b=.Method1()}`. (4) mehrdimensionale Array Erstellungs Ausdrücke können nur in Ausdrucks Baumstrukturen konvertiert werden, wenn Sie Ihren Elementtyp explizit deklarieren. (5) spät Bindungs Ausdrücke können nicht in Ausdrucks Baumstrukturen konvertiert werden. (6) Wenn eine Variable oder ein Feld ByRef an einen Aufruf Ausdruck übergeben wird, aber nicht denselben Typ wie der ByRef-Parameter hat, oder wenn eine Eigenschaft ByRef übergeben wird, ist die normale VB-Semantik, dass eine Kopie des Arguments ByRef übergeben und der endgültige Wert dann kopiert wird.  zurück in die Variable oder das Feld oder die Eigenschaft. In Ausdrucks Baumstrukturen findet das Zurückkopieren nicht statt. (7) alle diese Einschränkungen gelten auch für die in der Tabelle genannten Lambda-Ausdrücke.
    
  Wenn der Zieltyp nicht bekannt ist, wird die Lambda-Methode als Argument für einen delegatinstanzizierungsausdruck eines anonymen Delegattyps mit derselben Signatur der Lambda-Methode interpretiert. Wenn eine strikte Semantik verwendet wird und der Typ eines beliebigen Parameters weggelassen wird, tritt ein Kompilierzeitfehler auf. Andernfalls wird von `Object` ein fehlender Parametertyp ersetzt. Zum Beispiel:
    
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

* Eine Eigenschaften Gruppe kann als Eigenschaften Zugriff neu klassifiziert werden. Der Eigenschafts Gruppen Ausdruck wird als Index Ausdruck mit leeren Klammern interpretiert (d. h. `f` wird als `f()` interpretiert).

* Ein Eigenschaftenzugriff kann als Wert neu klassifiziert werden. Der Eigenschafts Zugriffs Ausdruck wird als Aufruf Ausdruck der `Get`-Zugriffsmethode der-Eigenschaft interpretiert. Wenn die Eigenschaft über keinen Getter verfügt, tritt ein Kompilierzeitfehler auf.

* Ein spät gebundener Zugriff kann als spät gebundene Methode oder spät gebundener Eigenschaften Zugriff neu klassifiziert werden. In einer Situation, in der ein spät gebundener Zugriff sowohl als Methoden Zugriff als auch als Eigenschaften Zugriff neu klassifiziert werden kann, wird die Neuklassifizierung an einen Eigenschaften Zugriff bevorzugt.

* Ein spät gebundener Zugriff kann als Wert neu klassifiziert werden.

* Ein Arrayliterale kann als Wert neu klassifiziert werden. Der Typ des Werts wird wie folgt bestimmt:

  1. Wenn die Neuklassifizierung im Kontext einer Konvertierung erfolgt, bei der der Zieltyp bekannt ist und der Zieltyp ein Arraytyp ist, wird das arrayliteralformat als ein Wert vom Typ T () neu klassifiziert. Wenn der Zieltyp "`System.Collections.Generic.IList(Of T)`", "`IReadOnlyList(Of T)`", "`ICollection(Of T)`", "`IReadOnlyCollection(Of T)`" oder "`IEnumerable(Of T)`" ist und das Arrayliterale eine Schachtelungs Ebene aufweist, wird das Array-Literalformat als Wert des Typs `T()` neu klassifiziert.

  2. Andernfalls wird das Arrayliteral in einen Wert umklassifiziert, dessen Typ ein Array von Rang ist, das gleich der Schachtelungs Schachtelung ist, wobei der Elementtyp durch den vorherrschenden Typ der Elemente im Initialisierer bestimmt wird. Wenn kein dominanter Typ bestimmt werden kann, wird `Object` verwendet. Zum Beispiel:

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

  __Nebenbei.__ Es gibt eine geringfügige Änderung des Verhaltens zwischen Version 9,0 und Version 10,0 der Sprache. Vor 10,0 hat sich die Initialisierer von Array Elementen nicht auf den Typrückschluss für lokale Variablen ausgewirkt, und dies geschieht nun. @No__t-0 hätte daher `Object()` als Typ von `a` in Version 9,0 der Sprache und `Integer()` in Version 10,0 abgeleitet.

  Die Neuklassifizierung interpretiert dann das Arrayliterale als Ausdruck für die Array Erstellung neu. Die Beispiele:

  ```vb
  Dim x As Double = { 1, 2, 3, 4 }
  Dim y = { "a", "b" }
  ```

  sind äquivalent zu:

  ```vb
  Dim x As Double = New Double() { 1, 2, 3, 4 }
  Dim y = New String() { "a", "b" }
  ```

  Die Neuklassifizierung wird als Einschränkungs Einschränkung eingestuft, wenn eine Konvertierung eines Element Ausdrucks in den Array Elementtyp einschränkend ist. Andernfalls wird es als Erweiterung bewertet.

* Der Standardwert `Nothing` kann als Wert neu klassifiziert werden. In einem Kontext, in dem der Zieltyp bekannt ist, ist das Ergebnis der Standardwert des Zieltyps. In einem Kontext, in dem der Zieltyp nicht bekannt ist, ist das Ergebnis ein NULL-Wert vom Typ `Object`.

Ein Namespace Ausdruck, ein Typausdruck, ein Ereignis Zugriffs Ausdruck oder ein void-Ausdruck können nicht neu klassifiziert werden. Mehrere Neuklassifizierungen können gleichzeitig durchgeführt werden. Zum Beispiel:

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

In diesem Fall wird der Eigenschafts Gruppen Ausdruck `P` zuerst von einer Eigenschaften Gruppe zu einem Eigenschaften Zugriff neu klassifiziert und dann von einem Eigenschaften Zugriff auf einen Wert neu klassifiziert. Die geringste Anzahl von Neuklassifizierungen wird ausgeführt, um eine gültige Klassifizierung im Kontext zu erreichen.

## <a name="constant-expressions"></a>Konstante Ausdrücke

Ein *konstanter Ausdruck* ist ein Ausdruck, dessen Wert zur Kompilierzeit vollständig ausgewertet werden kann.

```antlr
ConstantExpression
    : Expression
    ;
```

Der Typ eines konstanten Ausdrucks kann "`Byte`", "`SByte`", "`UShort`", "`Short`", "`UInteger`", "`Integer`", "`ULong`", "`Long`", "`Char`", "`Single`", "@no__t-@no__t", "3", "@no__t Die folgenden Konstrukte sind in konstanten Ausdrücken zulässig:

* Literale (einschließlich `Nothing`).

* Verweise auf Konstante Typmember oder Konstante lokale Elemente.

* Verweise auf Member von Enumerationstypen.

* Teil Ausdrücke in Klammern.

* Umwandlungs Ausdrücke, wenn der Zieltyp einem der oben aufgeführten Typen entspricht. Umwandlungen von und aus `String` sind eine Ausnahme von dieser Regel und sind nur für NULL-Werte zulässig, da `String`-Konvertierungen immer in der aktuellen Kultur der Ausführungsumgebung zur Laufzeit durchgeführt werden. Beachten Sie, dass Konstante Umwandlungs Ausdrücke nur systeminterne Konvertierungen verwenden können.

* Die unären Operatoren "`+`", "`-`" und "`Not`", sofern der Operand und das Ergebnis einen oben aufgeführten Typ enthalten.

* Der `+`, `-`, `*` `^`, `Mod`, `/`, `\`, `<<`, `>>`, `&`, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 und 0 binäre Operatoren Gibt an, dass alle Operanden und Ergebnisse einen oben aufgeführten Typ haben.

* Der bedingte Operator if gibt an, dass jeder Operand und jedes Ergebnis einen oben aufgelisteten Typ hat.

* Die folgenden Lauf Zeitfunktionen: `Microsoft.VisualBasic.Strings.ChrW`; `Microsoft.VisualBasic.Strings.Chr`, wenn der Konstante Wert zwischen 0 und 128 liegt. `Microsoft.VisualBasic.Strings.AscW`, wenn die Konstante Zeichenfolge nicht leer ist. `Microsoft.VisualBasic.Strings.Asc`, wenn die Konstante Zeichenfolge nicht leer ist.

Die folgenden Konstrukte sind in konstanten Ausdrücken *nicht* zulässig:

* Implizite Bindung durch einen `With`-Kontext.

Konstante Ausdrücke eines ganzzahligen Typs ("`ULong`", "`Long`", "`UInteger`", "`Integer`", "`UShort`", "`Short`", "`SByte`" oder "`Byte`") können implizit in einen engeren ganzzahligen Typ konvertiert werden. Konstante Ausdrücke vom Typ `Double` können implizit in t-9, wenn der Wert des konstanten Ausdrucks innerhalb des Bereichs des Zieltyps liegt. Diese einschränkenden Konvertierungen sind unabhängig davon zulässig, ob eine einschränkend sein oder strikte Semantik verwendet wird.


## <a name="late-bound-expressions"></a>Spät gebundene Ausdrücke

Wenn das Ziel eines Element Zugriffs Ausdrucks oder Index Ausdrucks vom Typ `Object` ist, wird die Verarbeitung des Ausdrucks möglicherweise bis zur Laufzeit verzögert. Das Verzögern der Verarbeitung auf diese Weise wird als *späte Bindung*bezeichnet. Bei der späten Bindung können `Object`-Variablen auf *typlose* Weise verwendet werden, wobei die gesamte Auflösung von Membern auf dem tatsächlichen Lauf Zeittyp des Werts in der Variablen basiert. Wenn eine strikte Semantik von der Kompilierungs Umgebung oder `Option Strict` angegeben wird, verursacht die späte Bindung einen Kompilierzeitfehler. Nicht öffentliche Member werden bei einer späten Bindung ignoriert, einschließlich für den Zweck der Überladungs Auflösung. Beachten Sie, dass im Gegensatz zum früh gebundenen Fall das Aufrufen von oder das Zugreifen auf ein `Shared`-Element spät gebunden bewirkt, dass das Aufruf Ziel zur Laufzeit ausgewertet wird. Wenn der Ausdruck ein Aufruf Ausdruck für ein Element ist, das auf `System.Object` definiert ist, wird die späte Bindung nicht durchgeführt.

Im Allgemeinen werden spät gebundene Zugriffe zur Laufzeit aufgelöst, indem der Bezeichner für den tatsächlichen Lauf Zeittyp des Ausdrucks gesucht wird. Wenn die Suche nach einem spät gebundenen Member zur Laufzeit fehlschlägt, wird eine Ausnahme vom Typ "`System.MissingMemberException`" ausgelöst. Da die Nachrichten für spät gebundene Elemente ausschließlich vom Lauf Zeittyp des zugeordneten Ziel Ausdrucks ausgeführt werden, ist der Lauf Zeittyp eines Objekts nie eine Schnittstelle. Daher ist es unmöglich, auf Schnittstellenmember in einem spät gebundenen Member-Zugriffs Ausdruck zuzugreifen.

Die Argumente für einen Zugriff auf spät gebundene Elemente werden in der Reihenfolge ausgewertet, in der Sie im Member-Zugriffs Ausdruck angezeigt werden: nicht in der Reihenfolge, in der Parameter im spät gebundenen Member deklariert werden. Dieses Unterschied wird im folgenden Beispiel veranschaulicht:

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

Dieser Code zeigt Folgendes an:

```console
Early-bound: xy
Late-bound: yx
```

Da die Auflösung spät gebundener Überladungen für den Lauf Zeittyp der Argumente erfolgt, kann es vorkommen, dass ein Ausdruck unterschiedliche Ergebnisse erzeugt, je nachdem, ob er zur Kompilierzeit oder zur Laufzeit ausgewertet wird. Dieses Unterschied wird im folgenden Beispiel veranschaulicht:

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

Dieser Code zeigt Folgendes an:

```console
F(Base)
F(Derived)
```

## <a name="simple-expressions"></a>Einfache Ausdrücke

Einfache Ausdrücke sind Literale, Ausdrücke in Klammern, Instanzausdrücke oder einfache namens Ausdrücke.

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

Literale Ausdrücke Werten den Wert aus, der vom Literalwert dargestellt wird. Ein Literalausdruck wird als Wert klassifiziert, ausgenommen der Literalwert `Nothing`, der als Standardwert klassifiziert wird.

```antlr
LiteralExpression
    : Literal
    ;
```

### <a name="parenthesized-expressions"></a>Ausdrücke in Klammern

Ein Ausdruck in Klammern besteht aus einem Ausdruck, der in Klammern eingeschlossen ist. Ein Ausdruck in Klammern wird als Wert klassifiziert, und der eingeschlossene Ausdruck muss als Wert klassifiziert werden. Ein Ausdruck in Klammern ergibt den Wert des Ausdrucks innerhalb der Klammern.

```antlr
ParenthesizedExpression
    : OpenParenthesis Expression CloseParenthesis
    ;
```

### <a name="instance-expressions"></a>Instanzausdrücke

Ein *Instanzausdruck* ist das Schlüsselwort `Me`. Sie kann nur innerhalb des Texts einer nicht freigegebenen Methode, eines Konstruktors oder einer Eigenschafts Zugriffsmethode verwendet werden. Sie wird als Wert klassifiziert. Das Schlüsselwort `Me` stellt die Instanz des Typs dar, der die ausgeführte Methode oder den Eigenschaften Accessor enthält. Wenn ein Konstruktor einen anderen Konstruktor (Abschnitts [Konstruktoren](type-members.md#constructors)) explizit aufruft, kann `Me` erst nach diesem Konstruktoraufruf verwendet werden, da die Instanz noch nicht erstellt wurde.

```antlr
InstanceExpression
    : 'Me'
    ;
```

### <a name="simple-name-expressions"></a>Einfache namens Ausdrücke

Ein *einfacher namens Ausdruck* besteht aus einem einzelnen Bezeichner, gefolgt von einer optionalen Typargument Liste.

```antlr
SimpleNameExpression
    : Identifier ( OpenParenthesis 'Of' TypeArgumentList CloseParenthesis )?
    ;
```

Der Name wird durch die folgenden "Regeln für einfache Namensauflösung" aufgelöst und klassifiziert:

1.  Beginnend mit dem unmittelbar einschließenden Block und fortsetzen mit jedem einschließenden äußeren Block (sofern vorhanden), bezieht sich der Bezeichner auf den Wert, wenn der Bezeichner mit dem Namen einer lokalen Variablen, einer statischen Variable, einer Konstanten lokalen Methode, eines methodentypparameters oder eines Parameters übereinstimmt. entsprechende Entität.

    Wenn der Bezeichner mit einer lokalen Variablen, einer statischen Variablen oder einer Konstanten lokalen Konstante übereinstimmt und eine Typargument Liste angegeben wurde, tritt ein Kompilierzeitfehler auf. Wenn der Bezeichner mit einem Methodentypparameter übereinstimmt und eine Typargument Liste bereitgestellt wurde, erfolgt keine Übereinstimmung, und die Auflösung wird Wenn der Bezeichner mit einer lokalen Variablen übereinstimmt, ist die lokale Variable, die übereinstimmt, die implizite Funktion, oder `Get`-Accessor gibt eine lokale Variable zurück, und der Ausdruck ist Teil eines Aufruf Ausdrucks, einer Aufruf Anweisung oder eines `AddressOf`-Ausdrucks, dann keine Übereinstimmung. Tritt auf und die Auflösung wird fortgesetzt

    Der Ausdruck wird als Variable klassifiziert, wenn es sich um eine lokale Variable, eine statische Variable oder einen Parameter handelt. Der Ausdruck wird als Typ klassifiziert, wenn es sich um einen Methodentypparameter handelt. Der Ausdruck wird als Wert klassifiziert, wenn es sich um eine Konstante local handelt.

2.  Wenn eine Suche des Bezeichners im Typ eine Entsprechung mit einem zugänglichen Member erzeugt, werden für jeden in der-Spalte enthaltenden Typ, der den Ausdruck enthält, beginnend mit dem innersten und zum äußersten Wert zurückgegeben:

    21. Wenn der übereinstimmende Typmember ein Typparameter ist, wird das Ergebnis als-Typ klassifiziert und ist der passende Typparameter. Wenn eine Typargument Liste angegeben wurde, erfolgt keine Entsprechung, und die Auflösung wird fortgesetzt.
    22. Andernfalls ist das Ergebnis, wenn der Typ der unmittelbar einschließende Typ und die Suche einen nicht freigegebenen Typmember identifiziert, identisch mit dem Element Zugriff auf das Formular `Me.E(Of A)`, wobei `E` der Bezeichner und `A` die Typargument Liste ist. , falls vorhanden.
    23. Andernfalls ist das Ergebnis genau das gleiche wie ein Element Zugriff auf das Formular `T.E(Of A)`, wobei `T` der Typ ist, der das übereinstimmende Element enthält, `E` der Bezeichner ist, und `A` ist die Typargument Liste (falls vorhanden). In diesem Fall ist es ein Fehler für den Bezeichner, auf einen nicht freigegebenen Member zu verweisen.

3.  Führen Sie für jeden schsted Namespace, beginnend am innersten und zum äußersten Namespace, die folgenden Schritte aus:

    31. Wenn der Namespace einen zugreif baren Typ mit dem angegebenen Namen enthält und die gleiche Anzahl von Typparametern aufweist, die in der Typargument Liste angegeben sind (sofern vorhanden), verweist der Bezeichner auf diesen Typ und wird als Typ klassifiziert.
    32. Andernfalls bezieht sich der Bezeichner auf diesen Namespace und wird als Namespace klassifiziert, wenn keine Typargument Liste angegeben wurde und der Namespace einen Namespace-Member mit dem angegebenen Namen enthält.
    33. Wenn der Namespace andernfalls mindestens ein zugreif bares Standardmodul enthält und eine Elementnamen Suche des Bezeichners eine barrierefreie Übereinstimmung in genau einem Standardmodul erzeugt, ist das Ergebnis genau das gleiche wie ein Member-Zugriff im Formular `M.E(Of A)`, wobei @no_ _T-1 ist das Standardmodul, das den passenden Member enthält, `E` der Bezeichner ist, und `A` ist die Typargument Liste, falls vorhanden. Wenn der Bezeichner mit zugänglichen Typmembern in mehr als einem Standardmodul übereinstimmt, tritt ein Kompilierzeitfehler auf.

4.  Wenn die Quelldatei mindestens eine Import-Aliase aufweist und der Bezeichner mit dem Namen einer dieser Dateien übereinstimmt, verweist der Bezeichner auf diesen Namespace oder Typ. Wenn eine Typargument Liste angegeben wird, tritt ein Kompilierzeitfehler auf.

5. Wenn die Quelldatei, die den Namen Verweis enthält, mindestens einen Import hat:

    51. Wenn der Bezeichner in genau einem Import den Namen eines barrierefreien Typs mit der gleichen Anzahl von Typparametern, die in der Typargument Liste angegeben sind (sofern vorhanden), oder einem Typmember übereinstimmt, verweist der Bezeichner auf diesen Typ oder Typmember. Wenn der Bezeichner in mehr als einem Import mit der gleichen Anzahl von Typparametern übereinstimmt, die in der Typargument Liste, sofern vorhanden, oder einem zugänglichen Typmember angegeben wurde, tritt ein Kompilierzeitfehler auf.
    52. Andernfalls bezieht sich der Bezeichner auf diesen Namespace, wenn keine Typargument Liste angegeben wurde und der Bezeichner genau einem Import den Namen eines Namespaces mit barrierefreien Typen entspricht. Wenn keine Typargument Liste angegeben wurde und der Bezeichner in mehr als einem Import den Namen eines Namespace mit zugänglichen Typen findet, tritt ein Kompilierzeitfehler auf.
    53. Wenn die Importe ein oder mehrere barrierefreie Standardmodule enthalten und eine Element Namenssuche des Bezeichners eine barrierefreie Übereinstimmung in genau einem Standardmodul erzeugt, ist das Ergebnis genau das gleiche wie ein Member-Zugriff im Formular `M.E(Of A)`, wobei @no__ t-1 ist das Standardmodul, das den passenden Member enthält, `E` der Bezeichner ist, und `A` ist die Typargument Liste, falls vorhanden. Wenn der Bezeichner mit zugänglichen Typmembern in mehr als einem Standardmodul übereinstimmt, tritt ein Kompilierzeitfehler auf.

6.  Wenn in der Kompilierungs Umgebung mindestens eine Import-Aliase definiert ist und der Bezeichner mit dem Namen einer dieser Elemente übereinstimmt, verweist der Bezeichner auf diesen Namespace oder Typ. Wenn eine Typargument Liste angegeben wird, tritt ein Kompilierzeitfehler auf.

7. Wenn in der Kompilierungs Umgebung mindestens ein Import definiert ist:

    71. Wenn der Bezeichner in genau einem Import den Namen eines barrierefreien Typs mit der gleichen Anzahl von Typparametern, die in der Typargument Liste angegeben sind (sofern vorhanden), oder einem Typmember übereinstimmt, verweist der Bezeichner auf diesen Typ oder Typmember. Wenn der Bezeichner in mehr als einem Import mit der gleichen Anzahl von Typparametern übereinstimmt, die in der Typargument Liste, sofern vorhanden, oder einem Typmember angegeben wurde, tritt ein Kompilierzeitfehler auf.
    72. Andernfalls bezieht sich der Bezeichner auf diesen Namespace, wenn keine Typargument Liste angegeben wurde und der Bezeichner genau einem Import den Namen eines Namespaces mit barrierefreien Typen entspricht. Wenn keine Typargument Liste angegeben wurde und der Bezeichner in mehr als einem Import den Namen eines Namespace mit zugänglichen Typen findet, tritt ein Kompilierzeitfehler auf.
    73. Wenn die Importe ein oder mehrere barrierefreie Standardmodule enthalten und eine Element Namenssuche des Bezeichners eine barrierefreie Übereinstimmung in genau einem Standardmodul erzeugt, ist das Ergebnis genau das gleiche wie ein Member-Zugriff im Formular `M.E(Of A)`, wobei @no__ t-1 ist das Standardmodul, das den passenden Member enthält, `E` der Bezeichner ist, und `A` ist die Typargument Liste, falls vorhanden. Wenn der Bezeichner mit zugänglichen Typmembern in mehr als einem Standardmodul übereinstimmt, tritt ein Kompilierzeitfehler auf.

8. Andernfalls ist der vom Bezeichner angegebene Name nicht definiert.

Ein einfacher namens Ausdruck, der nicht definiert ist, ist ein Kompilierzeitfehler.

Normalerweise kann ein Name nur einmal in einem bestimmten Namespace vorkommen. Da Namespaces jedoch über mehrere .NET-Assemblys hinweg deklariert werden können, kann es vorkommen, dass zwei Assemblys einen Typ mit demselben voll qualifizierten Namen definieren. In diesem Fall wird ein Typ, der im aktuellen Satz von Quelldateien deklariert ist, von einem in einer externen .NET-Assembly deklarierten Typ bevorzugt. Andernfalls ist der Name mehrdeutig, und es gibt keine Möglichkeit, den Namen zu unterscheiden.


### <a name="addressof-expressions"></a>AddressOf-Ausdrücke

Ein Ausdruck vom Typ "`AddressOf`" wird verwendet, um einen Methoden Zeiger zu entwickeln. Der Ausdruck besteht aus dem `AddressOf`-Schlüsselwort und einem Ausdruck, der als Methoden Gruppe oder spät gebundener Zugriff klassifiziert werden muss. Die Methoden Gruppe kann nicht auf Konstruktoren verweisen.

Das Ergebnis wird als Methoden Zeiger klassifiziert, mit dem gleichen zugeordneten Ziel Ausdruck und der gleichen Typargument Liste (sofern vorhanden) als Methoden Gruppe.

```antlr
AddressOfExpression
    : 'AddressOf' Expression
    ;
```

## <a name="type-expressions"></a>Typausdrücke

Ein *Typausdruck* ist ein `GetType`-Ausdruck, ein `TypeOf...Is`-Ausdruck, ein `Is`-Ausdruck oder ein `GetXmlNamespace`-Ausdruck.

```antlr
TypeExpression
    : GetTypeExpression
    | TypeOfIsExpression
    | IsExpression
    | GetXmlNamespaceExpression
    ;
```

### <a name="gettype-expressions"></a>GetType-Ausdrücke

Ein `GetType`-Ausdruck besteht aus dem Schlüsselwort `GetType` und dem Namen eines Typs.

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

Ein `GetType`-Ausdruck wird als Wert klassifiziert, und sein Wert ist die Reflektionsklasse (`System.Type`), die den *gettypeer-Typnamen*darstellt. Wenn " *gettypeer* Type" ein Typparameter ist, gibt der Ausdruck das `System.Type`-Objekt zurück, das dem Typargument entspricht, das für den Typparameter zur Laufzeit angegeben wird.

Der *gettypeer-Name* hat zwei Möglichkeiten:

* Es darf `System.Void` sein, die einzige Stelle in der Sprache, auf die auf diesen Typnamen verwiesen werden kann.

* Möglicherweise handelt es sich um einen konstruierten generischen Typ mit den Typargumenten. Dadurch kann der `GetType`-Ausdruck das `System.Type`-Objekt zurückgeben, das dem generischen Typ selbst entspricht.

Das folgende Beispiel veranschaulicht den `GetType`-Ausdruck:

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

Die resultierende Ausgabe lautet wie folgt:

```console
Int32
Int32
String
Double[]
```


### <a name="typeofis-expressions"></a>Typeof... Is-Ausdrücke

Ein `TypeOf...Is`-Ausdruck wird verwendet, um zu überprüfen, ob der Lauf Zeittyp eines Werts mit einem bestimmten Typ kompatibel ist. Der erste Operand muss als Wert klassifiziert werden, darf keine neu klassifizierte Lambda-Methode sein und muss einen Verweistyp oder einen nicht eingeschränkten typparametertyp aufweisen. Der zweite Operand muss ein Typname sein. Das Ergebnis des Ausdrucks wird als Wert klassifiziert und ist ein `Boolean`-Wert. Der Ausdruck wird zu `True` ausgewertet, wenn der Lauf Zeittyp des Operanden Identitäts-, Standard-, Verweis-, Array-, Werttyp-oder Typparameter Konvertierung in den Typ aufweist, andernfalls `False`. Ein Kompilierzeitfehler tritt auf, wenn keine Konvertierung zwischen dem Typ des Ausdrucks und dem spezifischen Typ vorhanden ist.

```antlr
TypeOfIsExpression
    : 'TypeOf' Expression 'Is' LineTerminator? TypeName
    ;
```

### <a name="is-expressions"></a>Is-Ausdrücke

Ein Ausdruck vom Typ "`Is`" oder "`IsNot`" wird für einen Verweis Gleichheits Vergleich verwendet.

```antlr
IsExpression
    : Expression 'Is' LineTerminator? Expression
    | Expression 'IsNot' LineTerminator? Expression
    ;
```

Jeder Ausdruck muss als Wert klassifiziert werden, und der Typ jedes Ausdrucks muss ein Verweistyp, ein uneingeschränkter typparametertyp oder ein Werte zulässt-Werttyp sein. Wenn der Typ eines Ausdrucks ein uneingeschränkter typparametertyp oder ein Werte zulässt-Werttyp ist, muss der andere Ausdruck der Literalwert `Nothing` sein.

Das Ergebnis wird als Wert klassifiziert und als `Boolean` typisiert. Ein `Is`-Vorgang wird als `True` ausgewertet, wenn beide Werte auf dieselbe Instanz verweisen oder beide Werte `Nothing` oder `False` sind. Ein `IsNot`-Vorgang wird als `False` ausgewertet, wenn beide Werte auf dieselbe Instanz verweisen oder beide Werte `Nothing` oder `True` sind.


### <a name="getxmlnamespace-expressions"></a>GetXmlNamespace-Ausdrücke

Ein `GetXmlNamespace`-Ausdruck besteht aus dem Schlüsselwort `GetXmlNamespace` und dem Namen eines XML-Namespace, der von der Quelldatei oder der Kompilierungs Umgebung deklariert wird.

```antlr
GetXmlNamespaceExpression
    : 'GetXmlNamespace' OpenParenthesis XMLNamespaceName? CloseParenthesis
    ;
```

Ein `GetXmlNamespace`-Ausdruck wird als Wert klassifiziert, und sein Wert ist eine Instanz von `System.Xml.Linq.XNamespace`, die den *xmlNamespaceName*darstellt. Wenn dieser Typ nicht verfügbar ist, tritt ein Kompilierzeitfehler auf.

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

Alles zwischen den Klammern wird als Teil des Namespace namens betrachtet, sodass XML-Regeln, wie z. b. Leerzeichen, angewendet werden. Zum Beispiel:

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

Der XML-Namespace Ausdruck kann auch weggelassen werden. in diesem Fall gibt der Ausdruck das Objekt zurück, das den XML-Standard Namespace darstellt.


## <a name="member-access-expressions"></a>Memberzugriffsausdrücke

Ein Member-Zugriffs Ausdruck wird für den Zugriff auf einen Member einer Entität verwendet.

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

Ein Element Zugriff auf das Formular `E.I(Of A)`, wobei `E` ein Ausdruck, ein nicht Array-Typname, das Schlüsselwort `Global` oder ausgelassen und `I` ein Bezeichner mit einer optionalen Typargument Liste `A` ist, wird wie folgt ausgewertet und klassifiziert:

1. Wenn `E` weggelassen wird, wird der Ausdruck aus der direkt enthaltenden `With`-Anweisung `E` ersetzt, und der Element Zugriff wird durchgeführt. Wenn keine `With`-Anweisung enthalten ist, tritt ein Kompilierzeitfehler auf.

2. Wenn `E` als Namespace klassifiziert ist oder `E` das Schlüsselwort `Global` ist, erfolgt die Element Suche im Kontext des angegebenen Namespace. Wenn `I` der Name eines zugreif baren Members dieses Namespace mit derselben Anzahl von Typparametern ist, die in der Typargument Liste angegeben wurden (sofern vorhanden), ist das Ergebnis dieser Member. Das Ergebnis wird abhängig vom Member als Namespace oder Typ klassifiziert. Andernfalls tritt ein Kompilierungsfehler auf.

3. Wenn `E` ein Typ oder ein Ausdruck ist, der als-Typ klassifiziert ist, erfolgt die Element Suche im Kontext des angegebenen Typs. Wenn `I` der Name eines barrierefreien Members von `E` ist, wird `E.I` wie folgt ausgewertet und klassifiziert:

    31. Wenn `I` das Schlüsselwort `New` und `E` keine Enumeration ist, tritt ein Kompilierzeitfehler auf.
    32. Wenn `I` einen Typ mit der gleichen Anzahl von Typparametern identifiziert, der in der Typargument Liste angegeben wurde (sofern vorhanden), ist das Ergebnis dieser Typ.
    33. Wenn `I` eine oder mehrere Methoden identifiziert, ist das Ergebnis eine Methoden Gruppe mit der zugeordneten Typargument Liste und keinem zugeordneten Ziel Ausdruck.
    34. Wenn `I` eine oder mehrere Eigenschaften identifiziert und keine Typargument Liste angegeben wurde, ist das Ergebnis eine Eigenschaften Gruppe ohne zugeordneten Ziel Ausdruck.
    35. Wenn `I` eine freigegebene Variable identifiziert und keine Typargument Liste angegeben wurde, ist das Ergebnis entweder eine Variable oder ein Wert. Wenn die Variable schreibgeschützt ist und der Verweis außerhalb des freigegebenen Konstruktors des Typs auftritt, in dem die Variable deklariert ist, ist das Ergebnis der Wert der freigegebenen Variablen `I` in `E`. Andernfalls ist das Ergebnis die freigegebene Variable `I` in `E`.
    36. Wenn `I` ein frei gegebenes Ereignis identifiziert und keine Typargument Liste angegeben wurde, ist das Ergebnis ein Ereignis Zugriff ohne zugeordneten Ziel Ausdruck.
    37. Wenn `I` eine Konstante identifiziert und keine Typargument Liste angegeben wurde, ist das Ergebnis der Wert dieser Konstante.
    38. Wenn `I` einen Enumerationsmember identifiziert und keine Typargument Liste angegeben wurde, ist das Ergebnis der Wert dieses Enumerationsmembers.
    39. Andernfalls ist `E.I` ein ungültiger Element Verweis, und es tritt ein Kompilierzeitfehler auf.

4. Wenn `E` als Variable oder Wert klassifiziert ist, der Typ, der `T` ist, wird die Element Suche im Kontext von `T` ausgeführt. Wenn `I` der Name eines barrierefreien Members von `T` ist, wird `E.I` wie folgt ausgewertet und klassifiziert:

    41. Wenn `I` das Schlüsselwort `New`, `E` `Me`, `MyBase` oder `MyClass` ist und keine Typargumente angegeben wurden, ist das Ergebnis eine Methoden Gruppe, die die Instanzkonstruktoren des Typs `E` mit dem zugeordneten Ziel Ausdruck `E` darstellt. und keine Typargument Liste. Andernfalls tritt ein Kompilierungsfehler auf.
    42. Wenn `I` eine oder mehrere Methoden identifiziert, einschließlich der Erweiterungs Methoden, wenn `T` nicht `Object` ist, ist das Ergebnis eine Methoden Gruppe mit der zugeordneten Typargument Liste und einem zugeordneten Ziel Ausdruck `E`.
    43. Wenn `I` eine oder mehrere Eigenschaften identifiziert und keine Typargumente angegeben wurden, ist das Ergebnis eine Eigenschaften Gruppe mit einem zugeordneten Ziel Ausdruck `E`.
    44. Wenn `I` eine freigegebene Variable oder eine Instanzvariable identifiziert und keine Typargumente angegeben wurden, ist das Ergebnis entweder eine Variable oder ein Wert. Wenn die Variable schreibgeschützt ist und der Verweis außerhalb eines Konstruktors der Klasse erfolgt, in der die Variable für die Art der Variablen (freigegeben oder Instanz) als geeignet deklariert ist, ist das Ergebnis der Wert der Variablen `I` in dem Objekt, auf das von verwiesen wird @no__ t-1. Wenn `T` ein Referenztyp ist, ist das Ergebnis die Variable `I` in dem Objekt, auf das von `E` verwiesen wird. Andernfalls ist das Ergebnis eine Variable, wenn `T` ein Werttyp ist und der Ausdruck `E` als Variable klassifiziert ist. Andernfalls ist das Ergebnis ein-Wert.
    45. Wenn `I` ein Ereignis identifiziert und keine Typargumente angegeben wurden, ist das Ergebnis ein Ereignis Zugriff mit einem zugeordneten Ziel Ausdruck `E`.
    46. Wenn `I` eine Konstante identifiziert und keine Typargumente angegeben wurden, ist das Ergebnis der Wert dieser Konstante.
    47. Wenn `I` einen Enumerationsmember identifiziert und keine Typargumente angegeben wurden, ist das Ergebnis der Wert dieses Enumerationsmembers.
    48. Wenn `T` `Object` ist, ist das Ergebnis eine spät gebundene Member-Suche, die als spät gebundener Zugriff mit der zugeordneten Typargument Liste und einem zugeordneten Ziel Ausdruck `E` klassifiziert ist.

5. Andernfalls ist `E.I` ein ungültiger Element Verweis, und es tritt ein Kompilierzeitfehler auf.

Ein Element Zugriff auf das Formular `MyClass.I(Of A)` entspricht `Me.I(Of A)`, aber alle Elemente, auf die zugegriffen wird, werden so behandelt, als wären die Member nicht über schreibbar. Folglich wird das Element, auf das zugegriffen wird, nicht durch den Lauf Zeittyp des Werts beeinflusst, auf den der Member zugreift.

Ein Element Zugriff auf das Formular `MyBase.I(Of A)` entspricht `CType(Me, T).I(Of A)`, wobei `T` der direkte Basistyp des Typs ist, der den Member Access-Ausdruck enthält. Alle Methodenaufrufe werden so behandelt, als ob die aufgerufene Methode nicht über schreibbar ist. Diese Form des Member-Zugriffs wird auch als *Basis Zugriff*bezeichnet.

Im folgenden Beispiel wird veranschaulicht, wie sich `Me`, `MyBase` und `MyClass` in Beziehung setzen:

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

Dieser Code gibt Folgendes aus:

```console
MoreDerived.F
Derived.F
Derived.F
```

Wenn ein Member-Zugriffs Ausdruck mit dem-Schlüsselwort `Global` beginnt, stellt das-Schlüsselwort den äußersten unbenannten Namespace dar. Dies ist in Situationen nützlich, in denen eine Deklaration einen einschließenden Namespace überschattet. Das Schlüsselwort "`Global`" ermöglicht das "Escapezeichen" im äußersten Namespace in dieser Situation. Zum Beispiel:

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

Im obigen Beispiel ist der erste Methoden Aufrufwert ungültig, da der Bezeichner `System` an die-Klasse `System` und nicht an den-Namespace `System` gebunden ist. Die einzige Möglichkeit, auf den Namespace "`System`" zuzugreifen, ist die Verwendung von `Global`, um den äußersten Namespace zu verwenden.

Wenn das Element, auf das zugegriffen wird, freigegeben wird, ist jeder Ausdruck auf der linken Seite des Zeitraums überflüssig und wird nicht ausgewertet, es sei denn, der Element Zugriff wurde spät gebunden. Beachten Sie z. B. folgenden Code:

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

Er gibt `The value of F is: 10` aus, da die Funktion `ReturnC` nicht aufgerufen werden muss, um eine Instanz von `C` für den Zugriff auf den freigegebenen Member `F` bereitzustellen.


### <a name="identical-type-and-member-names"></a>Identische Typen-und Elementnamen

Es ist nicht ungewöhnlich, dass Sie Member mit dem gleichen Namen wie deren Typ benennen. In dieser Situation kann jedoch ein unbequyes namens ausblenden auftreten:

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

Im vorherigen Beispiel wird der einfache Name `Color` in `DefaultColor` an die Instanzeigenschaft anstatt an den-Typ gebunden. Da in einem freigegebenen Member nicht auf einen Instanzmember verwiesen werden kann, ist dies normalerweise ein Fehler.

In diesem Fall kann jedoch eine spezielle Regel auf den-Typ zugreifen. Wenn der Basis Ausdruck eines Element Zugriffs Ausdrucks ein einfacher Name ist und an eine Konstante, ein Feld, eine Eigenschaft, eine lokale Variable oder einen Parameter gebunden ist, deren Typ denselben Namen hat, kann der Basis Ausdruck entweder auf den Member oder den Typ verweisen. Dies kann niemals zu Mehrdeutigkeit führen, da die Member, auf die von beiden eines zugegriffen werden kann, identisch sind.

### <a name="default-instances"></a>Standard Instanzen

In einigen Fällen haben Klassen, die von einer gemeinsamen Basisklasse abgeleitet sind, in der Regel nur eine einzige Instanz. Beispielsweise verfügen die meisten Fenster, die auf einer Benutzeroberfläche angezeigt werden, immer nur über eine Instanz, die auf dem Bildschirm angezeigt wird. Um die Arbeit mit diesen Klassentypen zu vereinfachen, können Visual Basic automatisch *Standard Instanzen* der Klassen generieren, die eine einzelne, leicht referenzierte Instanz für jede Klasse bereitstellen.

Standard Instanzen werden immer für eine Typen *Familie* erstellt, nicht für einen bestimmten Typ. Anstatt eine Standard Instanz für eine Klasse Form1 zu erstellen, die von Form abgeleitet wird, werden Standard Instanzen für alle Klassen erstellt, die vom Formular abgeleitet werden. Dies bedeutet, dass jede einzelne Klasse, die von der Basisklasse abgeleitet wird, nicht speziell für eine Standard Instanz gekennzeichnet werden muss.

Die Standard Instanz einer Klasse wird durch eine vom Compiler generierte Eigenschaft dargestellt, die die Standard Instanz dieser Klasse zurückgibt. Die Eigenschaft, die als Member einer Klasse mit dem Namen " *Group class* " generiert wurde, die das zuordnen und zerstören von Standard Instanzen für alle Klassen verwaltet, die von der jeweiligen Basisklasse abgeleitet sind. Beispielsweise können alle standardinstanzeigenschaften von Klassen, die von `Form` abgeleitet sind, in der `MyForms`-Klasse gesammelt werden. Wenn eine Instanz der Group-Klasse vom Ausdruck `My.Forms` zurückgegeben wird, greift der folgende Code auf die Standard Instanzen abgeleiteter Klassen zu `Form1` und `Form2`:

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

Standard Instanzen werden erst erstellt, wenn der erste Verweis darauf besteht. das Abrufen der Eigenschaft, die die Standard Instanz darstellt, bewirkt, dass die Standard Instanz erstellt wird, wenn Sie nicht bereits erstellt wurde oder `Nothing` festgelegt wurde. Um zu testen, ob eine Standard Instanz vorhanden ist, wird die Standard Instanz nicht erstellt, wenn eine Standard Instanz das Ziel eines `Is`-oder `IsNot`-Operators ist. Daher ist es möglich, zu testen, ob eine Standard Instanz `Nothing` oder ein anderer Verweis ist, ohne dass die Standard Instanz erstellt wird.

Standard Instanzen sollen auf einfache Weise von außerhalb der Klasse mit der Standard Instanz auf die Standard Instanz verweisen. Die Verwendung einer Standard Instanz aus einer Klasse, die diese definiert, kann Verwirrung verursachen, wenn auf die Instanz verwiesen wird, d. h. die Standard Instanz oder die aktuelle Instanz. Der folgende Code ändert z. b. nur den Wert `x` in der Standard Instanz, auch wenn er von einer anderen Instanz aufgerufen wird. Daher würde der Code den Wert `5` anstelle von `10` ausgeben:

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

Um diese Art von Verwirrung zu vermeiden, ist es nicht zulässig, in einer Instanzmethode des Typs der Standard Instanz auf eine Standard Instanz zu verweisen.

#### <a name="default-instances-and-type-names"></a>Standard Instanzen und Typnamen

Auf eine Standard Instanz kann auch direkt über den Namen des Typs zugegriffen werden. In diesem Fall wird in jedem Ausdrucks Kontext, in dem der Typname nicht zulässig ist, der Ausdruck `E`, wobei `E` den voll qualifizierten Namen der Klasse mit einer Standard Instanz darstellt, in `E'` geändert, wobei `E'` einen Ausdruck darstellt, der abruft. die standardinstanzeigenschaft. Wenn beispielsweise Standard Instanzen für Klassen, die von `Form` abgeleitet sind, den Zugriff auf die Standard Instanz über den Typnamen zulassen, entspricht der folgende Code dem Code im vorherigen Beispiel:

```vb
Module Main
    Sub Main()
        Form1.x = 10
        Console.WriteLine(Form2.y)
    End Sub
End Module
```

Dies bedeutet auch, dass eine Standard Instanz, die über den Namen des Typs zugänglich ist, auch über den Typnamen zugewiesen werden kann. Der folgende Code legt z. b. die Standard Instanz von `Form1` auf `Nothing` fest:

```vb
Module Main
    Sub Main()
        Form1 = Nothing
    End Sub
End Module
```

Beachten Sie, dass die Bedeutung von "`E.I`" `E` eine Klasse darstellt und `I` einen freigegebenen Member darstellt, der sich nicht ändert. Ein solcher Ausdruck greift weiterhin direkt aus der Klasseninstanz auf den freigegebenen Member zu und verweist nicht auf die Standard Instanz.

#### <a name="group-classes"></a>Gruppen Klassen

Das Attribut "`Microsoft.VisualBasic.MyGroupCollectionAttribute`" gibt die Gruppenklasse für eine Familie von Standard Instanzen an. Das-Attribut verfügt über vier Parameter:

* Der Parameter "`TypeToCollect`" gibt die Basisklasse für die Gruppe an. Alle instanziier baren Klassen ohne geöffnete Typparameter, die von einem Typ mit diesem Namen abgeleitet werden (unabhängig von Typparametern), verfügen automatisch über eine Standard Instanz.

* Der-Parameter `CreateInstanceMethodName` gibt die Methode an, die in der Group-Klasse aufgerufen wird, um eine neue-Instanz in einer standardinstanzeigenschaft zu erstellen

* Der-Parameter `DisposeInstanceMethodName` gibt die Methode an, die in der Group-Klasse aufgerufen wird, um eine standardinstanzeigenschaft zu verwerfen, wenn der Wert `Nothing` der standardinstanzeigenschaft zugewiesen wird

* Der-Parameter `DefaultInstanceAlias` ist der Ausdruck `E'`, um den Klassennamen zu ersetzen, wenn der Zugriff auf die Standard Instanzen direkt über den Typnamen möglich ist. Wenn dieser Parameter `Nothing` oder eine leere Zeichenfolge ist, kann auf Standard Instanzen für diesen Gruppentyp nicht direkt über den Namen des Typs zugegriffen werden. (__Hinweis:__ In allen aktuellen Implementierungen der Visual Basic Sprache wird der Parameter "`DefaultInstanceAlias`" ignoriert, außer im vom Compiler bereitgestellten Code.)

Mehrere Typen können in derselben Gruppe gesammelt werden, indem die Namen der Typen und Methoden in den ersten drei Parametern mithilfe von Kommas getrennt werden. In jedem Parameter muss die gleiche Anzahl von Elementen vorhanden sein, und die Listenelemente werden in der Reihenfolge abgeglichen. Die folgende Attribut Deklaration sammelt beispielsweise Typen, die von `C1`, `C2` oder `C3` abgeleitet sind, in einer einzelnen Gruppe:

```vb
<Microsoft.VisualBasic.MyGroupCollection("C1, C2, C3", _
    "CreateC1, CreateC2, CreateC3", _
    "DisposeC1, DisposeC2, DisposeC3", "My.Cs")>
Public NotInheritable Class MyCs
    ...
End Class
```

Die Signatur der Create-Methode muss die Form `Shared Function <Name>(Of T As {New, <Type>})(Instance Of T) As T` aufweisen. Die verwerfen-Methode muss die Form `Shared Sub <Name>(Of T As <Type>)(ByRef Instance Of T)` aufweisen. Die Group-Klasse für das Beispiel im vorherigen Abschnitt könnte daher wie folgt deklariert werden:

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

Wenn eine Quelldatei eine abgeleitete Klasse `Form1` deklariert hat, wäre die generierte Gruppenklasse Äquivalent zu:

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

### <a name="extension-method-collection"></a>Erweiterungs Methoden Auflistung

Erweiterungs Methoden für den Member-Zugriffs Ausdruck `E.I` werden gesammelt, indem alle Erweiterungs Methoden mit dem Namen `I`, die im aktuellen Kontext verfügbar sind, erfasst werden:

1. Zuerst wird jeder eingegebene Typ, der den Ausdruck enthält, geprüft, beginnend mit dem innersten und dem äußersten.
2. Anschließend wird jeder eingegebene Namespace geprüft, beginnend am innersten und zum äußersten Namespace.
3. Anschließend werden die Importe in der Quelldatei geprüft.
4. Anschließend werden die von der Kompilierungs Umgebung definierten Importe geprüft.

Eine Erweiterungsmethode wird nur gesammelt, wenn eine erweiternde Native Konvertierung vom Ziel Ausdruckstyp in den Typ des ersten Parameters der Erweiterungsmethode vorhanden ist. Und im Gegensatz zu regulären Simple Name Expression-Bindungen sammelt die Suche *alle* Erweiterungs Methoden. die Auflistung wird nicht angehalten, wenn eine Erweiterungsmethode gefunden wird. Zum Beispiel:

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

In diesem Beispiel werden beide als Erweiterungs Methoden betrachtet, obwohl `N2C1Extensions.M1` vor `N1C1Extensions.M1` gefunden wurde. Nachdem alle Erweiterungs Methoden erfasst wurden, werden Sie dann in den *Cursor*aufgenommen. Currying nimmt das Ziel des Erweiterungs Methoden Aufrufes an und wendet es auf den Aufrufen der Erweiterungsmethode an, was zu einer neuen Methoden Signatur führt, bei der der erste Parameter entfernt wurde (da er angegeben wurde). Zum Beispiel:

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

Im obigen Beispiel ist das Curry-Ergebnis der Anwendung von `v` auf `Ext1.M` die Methoden Signatur `Sub M(y As Integer)`.

Zusätzlich zum Entfernen des ersten Parameters der Erweiterungsmethode entfernt Currying auch alle Methodentypparameter, die Teil des Typs des ersten Parameters sind. Wenn eine Erweiterungsmethode mit dem Methodentypparameter verwendet wird, wird der Typrückschluss auf den ersten Parameter angewendet, und das Ergebnis wird für alle Typparameter korrigiert, die abgeleitet werden. Wenn der Typrückschluss fehlschlägt, wird die Methode ignoriert. Zum Beispiel:

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

Im obigen Beispiel ist das Curry-Ergebnis der Anwendung von `v` auf `Ext1.M` die Methoden Signatur `Sub M(Of U)(y As U)`, da der Typparameter `T` als Ergebnis der Currying abgeleitet und nun korrigiert wurde. Da der Typparameter `U` nicht als Teil der Currying abgeleitet wurde, bleibt er ein offener Parameter. Ebenso, weil der Typparameter `T` als Ergebnis der Anwendung von `v` auf `Ext2.M` abgeleitet wird, wird der Parametertyp `y` als `Integer` korrigiert. Er wird nicht als beliebiger anderer Typ abgeleitet. Bei der Erstellung der Signatur werden alle Einschränkungen außer `New`-Einschränkungen ebenfalls angewendet. Wenn die Einschränkungen nicht erfüllt werden oder von einem Typ abhängen, der nicht als Teil von Currying abgeleitet wurde, wird die Erweiterungsmethode ignoriert. Zum Beispiel:

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

__Nebenbei.__ Einer der Hauptgründe für das Ausführen von Erweiterungs Methoden besteht darin, dass Abfrage Ausdrücke den Typ der Iterationen ableiten können, bevor die Argumente für eine Abfrage Muster Methode ausgewertet werden. Da die meisten Abfrage Muster Methoden Lambda-Ausdrücke akzeptieren, die den Typrückschluss selbst erfordern, vereinfacht dies den Prozess der Auswertung eines Abfrage Ausdrucks erheblich.

Anders als bei der normalen Schnittstellen Vererbung stehen Erweiterungs Methoden zur Verfügung, die zwei Schnittstellen erweitern, die nicht miteinander in Beziehung stehen, sofern Sie nicht die gleiche geschweifende Signatur aufweisen:

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

Schließlich ist es wichtig zu beachten, dass Erweiterungs Methoden beim Ausführen späterer Bindungen nicht berücksichtigt werden:

```vb
Module Test
    Sub Main()
        Dim o As Object = ...

        ' Ignores extension methods
        o.M1()
    End Sub
End Module
```

## <a name="dictionary-member-access-expressions"></a>Zugriffs Ausdrücke für Wörterbuch Elemente

Ein *Wörterbuch-Member-Zugriffs Ausdruck* wird verwendet, um einen Member einer Auflistung zu suchen. Ein Zugriff auf Wörterbuch Elemente hat die Form `E!I`, wobei `E` ein Ausdruck ist, der als Wert klassifiziert wird, und `I` ein Bezeichner ist.

```antlr
DictionaryAccessExpression
    : Expression? '!' IdentifierOrKeyword
    ;
```

Der Typ des Ausdrucks muss über eine Standard Eigenschaft verfügen, die durch einen einzelnen `String`-Parameter indiziert wird. Der "Dictionary Member Access"-Ausdruck "`E!I`" wird in den Ausdruck `E.D("I")` transformiert, wobei "`D`" die Standard Eigenschaft von "`E`" ist. Zum Beispiel:

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

Wenn ein Ausrufezeichen ohne Ausdruck angegeben wird, wird der Ausdruck aus der direkt enthaltenden `With`-Anweisung angenommen. Wenn keine `With`-Anweisung enthalten ist, tritt ein Kompilierzeitfehler auf.


## <a name="invocation-expressions"></a>Aufrufausdrücke

Ein Aufruf Ausdruck besteht aus einem Aufruf Ziel und einer optionalen Argumentliste.

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

Der Ziel Ausdruck muss als Methoden Gruppe oder als Wert mit einem Delegattyp klassifiziert werden. Wenn der Ziel Ausdruck ein Wert ist, dessen Typ ein Delegattyp ist, wird das Ziel des Aufruf Ausdrucks zur Methoden Gruppe für das `Invoke`-Member des Delegattyps, und der Ziel Ausdruck wird zum zugeordneten Ziel Ausdruck der Methoden Gruppe.

Eine Argumentliste besteht aus zwei Abschnitten: Positions Argumente und benannte Argumente. *Positions Argumente* sind Ausdrücke und müssen allen benannten Argumenten vorangestellt sein. *Benannte Argumente* beginnen mit einem Bezeichner, der Schlüsselwörter entsprechen kann, gefolgt von `:=` und einem Ausdruck.

Wenn die Methoden Gruppe nur eine barrierefreie Methode enthält, einschließlich der Instanz-und Erweiterungs Methoden, und diese Methode keine Argumente annimmt und eine Funktion ist, wird die Methoden Gruppe als Aufruf Ausdruck mit einer leeren Argumentliste interpretiert, und das Ergebnis ist wird als Ziel eines Aufruf Ausdrucks mit der angegebenen Argumentliste (n) verwendet. Zum Beispiel:

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

Andernfalls wird die Überladungs Auflösung auf die Methoden angewendet, um die am häufigsten anwendbare Methode für die angegebene Argumentliste (n) auszuwählen. Wenn die am meisten anwendbare Methode eine Funktion ist, wird das Ergebnis des Aufruf Ausdrucks als Wert klassifiziert, der als Rückgabetyp der Funktion typisiert ist. Wenn die am meisten anwendbare Methode eine Unterroutine ist, wird das Ergebnis als void klassifiziert. Wenn es sich bei der am meisten anwendbaren Methode um eine partielle Methode handelt, die keinen Text enthält, wird der Aufruf Ausdruck ignoriert, und das Ergebnis wird als void klassifiziert.

Für einen früh gebundenen Aufruf Ausdruck werden die Argumente in der Reihenfolge ausgewertet, in der die entsprechenden Parameter in der Ziel Methode deklariert werden. Für einen Ausdruck mit einem spät gebundenen Member werden Sie in der Reihenfolge ausgewertet, in der Sie im Member-Zugriffs Ausdruck angezeigt werden: siehe Abschnitt [spät gebundene Ausdrücke](expressions.md#late-bound-expressions).

## <a name="overloaded-method-resolution"></a>Auflösung der überladenen Methode:
Für die Überladungs Auflösung, eine Spezifizität von Membern/Typen mit einer Argumentliste, Generizität, Anwendbarkeit in Argumentliste, Übergabe von Argumenten und Auswahl von Argumenten für optionale Parameter, bedingte Methoden und das Ableiten von Typargumenten: siehe Abschnitt [ Überladungs Auflösung](overload-resolution.md).

## <a name="index-expressions"></a>Index Ausdrücke

Ein *Index Ausdruck* führt zu einem Array Element oder klassifiziert eine Eigenschaften Gruppe in einen Eigenschaften Zugriff neu. Ein Index Ausdruck besteht aus einem Ausdruck, einer öffnenden Klammer, einer Index Argumentliste und einer schließenden Klammer.

```antlr
IndexExpression
    : Expression OpenParenthesis ArgumentList? CloseParenthesis
    ;
```

Das Ziel des Index Ausdrucks muss entweder als Eigenschaften Gruppe oder als Wert klassifiziert werden. Ein Index Ausdruck wird wie folgt verarbeitet:

* Wenn der Ziel Ausdruck als Wert klassifiziert wird und der Typ kein Arraytyp ist, `Object` oder `System.Array`, muss der Typ über eine Default-Eigenschaft verfügen. Der Index wird für eine Eigenschaften Gruppe ausgeführt, die alle Standardeigenschaften des Typs darstellt. Obwohl es nicht zulässig ist, eine Parameter lose Default-Eigenschaft in Visual Basic zu deklarieren, können andere Sprachen eine solche Eigenschaft deklarieren. Folglich ist das Indizieren einer Eigenschaft ohne Argumente zulässig.

* Wenn der Ausdruck einen Wert eines Array Typs ergibt, muss die Anzahl der Argumente in der Argumentliste mit dem Rang des Arraytyps identisch sein und darf keine benannten Argumente enthalten. Wenn einer der Indizes zur Laufzeit ungültig ist, wird eine Ausnahme vom Typ "`System.IndexOutOfRangeException`" ausgelöst. Jeder Ausdruck muss implizit in den Typ "`Integer`" konvertiert werden können. Das Ergebnis des Index Ausdrucks ist die Variable am angegebenen Index und wird als Variable klassifiziert.

* Wenn der Ausdruck als Eigenschaften Gruppe klassifiziert wird, wird die Überladungs Auflösung verwendet, um zu bestimmen, ob eine der Eigenschaften auf die Index Argumentliste anwendbar ist. Wenn die Eigenschaften Gruppe nur eine Eigenschaft enthält, die über einen `Get`-Accessor verfügt und dieser Accessor keine Argumente annimmt, wird die Eigenschaften Gruppe als Index Ausdruck mit einer leeren Argumentliste interpretiert. Das Ergebnis wird als Ziel des aktuellen Index Ausdrucks verwendet. Wenn keine Eigenschaften anwendbar sind, tritt ein Kompilierzeitfehler auf. Andernfalls führt der Ausdruck zu einem Eigenschaften Zugriff mit dem zugeordneten Ziel Ausdruck (sofern vorhanden) der Eigenschaften Gruppe.

* Wenn der Ausdruck als spät gebundene Eigenschaften Gruppe oder als Wert klassifiziert wird, dessen Typ `Object` oder `System.Array` ist, wird die Verarbeitung des Index Ausdrucks bis zur Laufzeit verzögert, und die Indizierung ist spät gebunden. Der Ausdruck führt dazu, dass ein Eigenschaften Zugriff mit später Bindung als `Object` typisiert ist. Der zugeordnete Ziel Ausdruck ist entweder der Ziel Ausdruck, wenn es sich um einen Wert handelt, oder der zugehörige Ziel Ausdruck der Eigenschaften Gruppe. Zur Laufzeit wird der Ausdruck wie folgt verarbeitet:

* Wenn der Ausdruck als spät gebundene Eigenschaften Gruppe klassifiziert ist, kann der Ausdruck zu einer Methoden Gruppe, einer Eigenschaften Gruppe oder einem Wert führen (wenn der Member eine Instanz oder eine freigegebene Variable ist). Wenn das Ergebnis eine Methoden Gruppe oder eine Eigenschaften Gruppe ist, wird die Überladungs Auflösung auf die Gruppe angewendet, um die richtige Methode für die Argumentliste zu ermitteln. Wenn die Überladungs Auflösung fehlschlägt, wird eine Ausnahme vom Typ `System.Reflection.AmbiguousMatchException` ausgelöst. Anschließend wird das Ergebnis entweder als Eigenschaften Zugriff oder als Aufruf verarbeitet, und das Ergebnis wird zurückgegeben. Wenn der Aufruf einer Unterroutine ist, ist das Ergebnis `Nothing`.

* Wenn der Lauf Zeittyp des Ziel Ausdrucks ein Arraytyp oder `System.Array` ist, ist das Ergebnis des Index Ausdrucks der Wert der Variablen am angegebenen Index.

* Andernfalls muss der Lauf Zeittyp des Ausdrucks über eine Default-Eigenschaft verfügen, und der Index wird für die Eigenschaften Gruppe ausgeführt, die alle Standardeigenschaften für den Typ darstellt. Wenn der Typ keine Standard Eigenschaft aufweist, wird eine Ausnahme vom Typ "`System.MissingMemberException`" ausgelöst.


## <a name="new-expressions"></a>Neue Ausdrücke

Der `New`-Operator wird verwendet, um neue Instanzen von Typen zu erstellen. Es gibt vier Formen von `New`-ausdrücken:

* Objekterstellungs-Ausdrücke werden verwendet, um neue Instanzen von Klassentypen und Werttypen zu erstellen.

* Array Erstellungs Ausdrücke werden verwendet, um neue Instanzen von Array Typen zu erstellen.

* Delegaterstellungs-Ausdrücke (die keine unterschiedliche Syntax aus Objekt Erstellungs Ausdrücken aufweisen) werden verwendet, um neue Instanzen von Delegattypen zu erstellen.

* Anonyme Objekterstellungs Ausdrücke werden verwendet, um neue Instanzen anonymer Klassentypen zu erstellen.

```antlr
NewExpression
    : ObjectCreationExpression
    | ArrayExpression
    | AnonymousObjectCreationExpression
    ;
```

Ein `New`-Ausdruck wird als Wert klassifiziert, und das Ergebnis ist die neue Instanz des-Typs.


### <a name="object-creation-expressions"></a>Objekterstellungs-Ausdrücke

Ein Objekt Erstellungs Ausdruck wird verwendet, um eine neue Instanz eines Klassen Typs oder eines Struktur Typs zu erstellen.

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

Der Typ eines Objekterstellungs-Ausdrucks muss ein Klassentyp, ein Strukturtyp oder ein Typparameter mit einer `New`-Einschränkung sein und darf keine `MustInherit`-Klasse sein. Wenn ein Objekt Erstellungs Ausdruck in der Form `New T(A)` ist, wobei `T` ein Klassentyp oder Strukturtyp ist und `A` eine optionale Argumentliste ist, bestimmt die Überladungs Auflösung den korrekten Konstruktor von `T` zum Aufrufen von. Ein Typparameter mit einer `New`-Einschränkung wird als einzelner Parameter loser Konstruktor betrachtet. Wenn kein Konstruktor aufgerufen werden kann, tritt ein Kompilierzeitfehler auf. Andernfalls führt der Ausdruck zur Erstellung einer neuen Instanz von `T` mithilfe des ausgewählten Konstruktors. Wenn keine Argumente vorhanden sind, können die Klammern ausgelassen werden.

Wo eine Instanz zugewiesen wird, hängt davon ab, ob die Instanz ein Klassentyp oder ein Werttyp ist. `New` Instanzen von Klassentypen werden auf dem System Heap erstellt, während neue Instanzen von Werttypen direkt auf dem Stapel erstellt werden.

Ein Objekt Erstellungs Ausdruck kann optional eine Liste von Elementinitialisierern nach den Konstruktorargumenten angeben. Diesen Member-Initialisierern wird das Schlüsselwort `With` vorangestellt, und die Initialisiererliste wird so interpretiert, als ob Sie im Kontext einer `With`-Anweisung wäre. Beispielsweise mit der-Klasse:

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

ist ungefähr Äquivalent zu:

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

Jeder Initialisierer muss einen Namen angeben, der zugewiesen werden soll, und der Name muss eine nicht-`ReadOnly`-Instanzvariable oder-Eigenschaft des erstellten Typs sein. der Member-Zugriff wird nicht spät gebunden, wenn der erstellte Typ `Object` ist. Initialisierer dürfen das `Key`-Schlüsselwort nicht verwenden. Jedes Element in einem Typ kann nur einmal initialisiert werden. Die initialisiererausdrücke können jedoch aufeinander verweisen. Zum Beispiel:

```vb
Module Test
    Sub Main()
        Dim x As New Customer() With { .Name = "Bob Smith", _
            .Address = .Name & " St." }
    End Sub
End Module
```

Die Initialisierer werden von links nach rechts zugewiesen. Wenn also ein Initialisierer auf einen Member verweist, der noch nicht initialisiert wurde, wird der Wert der Instanzvariablen nach dem Ausführen des Konstruktors angezeigt:

```vb
Module Test
    Sub Main()
        ' The value of Address will be " St." since Name has not been
        ' assigned yet.
        Dim x As New Customer() With { .Address = .Name & " St." }
    End Sub
End Module
```

Initialisierer können eingebettet werden:

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

Wenn der erstellte Typ ein Sammlungstyp ist und über eine Instanzmethode mit dem Namen `Add` (einschließlich Erweiterungs Methoden und freigegebenen Methoden) verfügt, kann der Objekt Erstellungs Ausdruck einen sammlungsinitialisierer angeben, dem das Schlüsselwort `From` vorangestellt ist. Ein Objekt Erstellungs Ausdruck kann nicht gleichzeitig einen Elementinitialisierer und einen Auflistungsinitialisierer angeben. Jedes Element im Auflistungsinitialisierer wird als Argument an einen Aufruf der `Add`-Funktion übermittelt. Zum Beispiel:

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

Wenn ein Element ein Auflistungsinitialisierer selbst ist, wird jedes Element des untergeordneten Auflistungsinitialisierers als einzelnes Argument an die Funktion "`Add`" übermittelt. Beispielsweise Folgendes:

```vb
Dim dict = Dictionary(Of Integer, String) From { { 1, "One" },{ 2, "Two" } }
```

identisch mit folgendem Ausdruck:

```vb
Dim dict = New Dictionary(Of Integer, String)
dict.Add(1, "One")
dict.Add(2, "Two")
```

Diese Erweiterung wird immer durchgeführt und wird nur einmal auf eine Ebene tief geführt; Danach gelten subinitialisierer als Array Literale. Zum Beispiel:

```vb
' Error: List(Of T) does not have an Add method that takes two parameters.
Dim list = New List(Of Integer())() From { { 1, 2 }, { 3, 4 } }

' OK, this initializes the dictionary with (Integer, Integer()) pairs.
Dim dict = New Dictionary(Of Integer, Integer())() From _
        { {  1, { 2, 3 } }, { 3, { 4, 5 } } }
```


### <a name="array-expressions"></a>Array Ausdrücke

Ein Array Ausdruck wird verwendet, um eine neue Instanz eines Arraytyps zu erstellen. Es gibt zwei Typen von Array Ausdrücken: Array Erstellungs Ausdrücke und Array Literale.

#### <a name="array-creation-expressions"></a>Ausdrücke zum Erstellen von Arrays

Wenn ein Initialisierungs Modifizierer für die Array Größe angegeben wird, wird der resultierende Arraytyp abgeleitet, indem jedes einzelne Argument aus der Argumentliste der Array Größen Initialisierung gelöscht wird. Der Wert jedes Arguments bestimmt die obere Grenze der entsprechenden Dimension in der neu zugeordneten Array Instanz. Wenn der Ausdruck über einen nicht leeren Auflistungsinitialisierer verfügt, muss jedes Argument in der Argumentliste eine Konstante sein, und die von der Ausdrucks Liste angegebenen Rang-und Dimensions Längen müssen mit denen des Auflistungsinitialisierers identisch sein.

```vb
Dim a() As Integer = New Integer(2) {}
Dim b() As Integer = New Integer(2) { 1, 2, 3 }
Dim c(,) As Integer = New Integer(1, 2) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error, length/initializer mismatch.
Dim d() As Integer = New Integer(2) { 0, 1, 2, 3 }
```

Wenn ein Initialisierungs Modifizierer für die Array Größe nicht angegeben wird, muss der Typname ein Arraytyp sein, und der Auflistungsinitialisierer muss leer sein oder über die gleiche Anzahl von Schachtelungs Ebenen verfügen wie der Rang des angegebenen Array Typs. Alle Elemente in der innersten Schachtelungs Ebene müssen implizit in den Elementtyp des Arrays konvertiert werden und müssen als Wert klassifiziert werden. Die Anzahl der Elemente in jedem Initialisierer für die Initialisierung einer Initialisierung muss immer mit der Größe der anderen Auflistungen auf derselben Ebene konsistent sein. Die einzelnen Dimensions Längen werden von der Anzahl von Elementen in jeder der entsprechenden Schachtelungs Ebenen des Auflistungsinitialisierers abgeleitet. Wenn der Auflistungsinitialisierer leer ist, ist die Länge der einzelnen Dimensionen gleich 0 (null).

```vb
Dim e() As Integer = New Integer() { 1, 2, 3 }
Dim f(,) As Integer = New Integer(,) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error: Inconsistent numbers of elements!
Dim g(,) As Integer = New Integer(,) { { 1, 2 }, { 4, 5, 6 } }

' Error: Inconsistent levels of nesting!
Dim h(,) As Integer = New Integer(,) { 1, 2, { 3, 4 } }
```

Die äußerste Schachtelungs Ebene eines Auflistungsinitialisierers entspricht der äußersten linken Dimension eines Arrays, und die innerste Schachtelungs Ebene entspricht der äußersten rechten Dimension. Das Beispiel:

```vb
Dim array As Integer(,) = _
    { { 0, 1 }, { 2, 3 }, { 4, 5 }, { 6, 7 }, { 8, 9 } }
```

Entspricht Folgendem:

```vb
Dim array(4, 1) As Integer

array(0, 0) = 0: array(0, 1) = 1
array(1, 0) = 2: array(1, 1) = 3
array(2, 0) = 4: array(2, 1) = 5
array(3, 0) = 6: array(3, 1) = 7
array(4, 0) = 8: array(4, 1) = 9
```

Wenn der Auflistungsinitialisierer leer ist (d. h. einen, der geschweifte Klammern, aber keine Initialisiererliste enthält) und die Begrenzungen der Dimensionen des zu initialisierenden Arrays bekannt sind, stellt der leere sammlungsinitialisierer eine Array Instanz der angegebenen Größe dar. , wo alle Elemente mit dem Standardwert des Elementtyps initialisiert wurden. Wenn die Begrenzungen der Dimensionen des Arrays, das initialisiert wird, nicht bekannt sind, stellt der leere Auflistungsinitialisierer eine Array Instanz dar, in der alle Dimensionen die Größe 0 (null) aufweisen.

Der Rang und die Länge einer Array Instanz der einzelnen Dimensionen sind für die gesamte Lebensdauer der Instanz konstant. Anders ausgedrückt: Es ist nicht möglich, den Rang einer vorhandenen Array Instanz zu ändern, und es ist nicht möglich, die Größe der Dimensionen zu ändern.

#### <a name="array-literals"></a>Arrayliterale

Ein Arrayliterale bezeichnet ein Array, dessen Elementtyp, Rang und Begrenzungen von einer Kombination aus dem Ausdrucks Kontext und einem Auflistungsinitialisierer abgeleitet werden. Dies wird im Abschnitt [Ausdrucks Neuklassifizierung](expressions.md#expression-reclassification)erläutert.

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

Das Format und die Anforderungen für den Auflistungsinitialisierer in einem arrayliteralformat sind identisch mit denen für den Auflistungsinitialisierer in einem Array Erstellungs Ausdruck.

__Nebenbei.__ Ein arrayliteralarray erstellt das Array nicht in und von sich selbst. Stattdessen ist es die Neuklassifizierung des Ausdrucks in einen Wert, der bewirkt, dass das Array erstellt wird. Beispielsweise ist die Konvertierung `CType(new Integer() {1,2,3}, Short())` nicht möglich, da keine Konvertierung von `Integer()` in `Short()`; der Ausdruck `CType({1,2,3},Short())` ist jedoch möglich, da er zuerst das arrayliteralzeichen in den Array Erstellungs Ausdruck `New Short() {1,2,3}` umklassifiziert.


### <a name="delegate-creation-expressions"></a>Delegat-Erstellungs Ausdrücke

Ein delegaterstellungs-Ausdruck wird verwendet, um eine neue Instanz eines Delegattyps zu erstellen. Das Argument eines Delegaten zum Erstellen von Delegaten muss ein Ausdruck sein, der als Methoden Zeiger oder als Lambda-Methode klassifiziert ist.

Wenn das Argument ein Methoden Zeiger ist, muss eine der Methoden, auf die vom Methoden Zeiger verwiesen wird, auf die Signatur des Delegattyps anwendbar sein. Eine Methode `M` gilt für einen Delegattyp `D`, wenn Folgendes zutrifft:

* `M` ist nicht `Partial` oder hat einen Text.

* Sowohl `M` als auch `D` sind Funktionen, oder `D` ist eine Unterroutine.

* `M` und `D` verfügen über die gleiche Anzahl von Parametern.

* Die Parametertypen von `M` verfügen jeweils über eine Konvertierung vom Typ des entsprechenden Parameter Typs von `D`, und ihre Modifizierer (d. h. `ByRef`, `ByVal`) stimmen überein.

* Der Rückgabetyp von `M` ist, falls vorhanden, eine Konvertierung in den Rückgabetyp von `D`.

Wenn der Methoden Zeiger auf einen spät gebundenen Zugriff verweist, wird davon ausgegangen, dass der spät gebundene Zugriff an eine Funktion erfolgt, die über die gleiche Anzahl von Parametern wie der Delegattyp verfügt.

Wenn eine strikte Semantik nicht verwendet wird und nur eine Methode durch den Methoden Zeiger referenziert wird, aber aufgrund der Tatsache, dass Sie über keine Parameter verfügt und der Delegattyp dies tut, gilt die Methode als anwendbar und die Parameter oder der Rückgabewert a wird erneut ignoriert. Zum Beispiel:

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

__Nebenbei.__ Diese Lockerung ist nur zulässig, wenn eine strikte Semantik aufgrund von Erweiterungs Methoden nicht verwendet wird. Da Erweiterungs Methoden nur berücksichtigt werden, wenn eine reguläre Methode nicht anwendbar ist, kann eine Instanzmethode ohne Parameter eine Erweiterungsmethode mit Parametern zum Zweck der Delegaterstellung ausblenden.

Wenn mehr als eine Methode, auf die der Methoden Zeiger verweist, auf den Delegattyp anwendbar ist, wird die Überladungs Auflösung verwendet, um zwischen den Kandidaten Methoden zu wählen. Die Typen der Parameter für den Delegaten werden als Typargumente für den Zweck der Überladungs Auflösung verwendet. Wenn kein Methoden Kandidat am meisten anwendbar ist, tritt ein Kompilierzeitfehler auf. Im folgenden Beispiel wird die lokale Variable mit einem Delegaten initialisiert, der auf die zweite `Square`-Methode verweist, da diese Methode besser auf die Signatur und den Rückgabetyp `DoubleFunc` anwendbar ist.

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

Wenn die zweite `Square`-Methode nicht vorhanden war, wurde die erste `Square`-Methode ausgewählt. Wenn eine strikte Semantik von der Kompilierungs Umgebung oder `Option Strict` angegeben wird, tritt ein Kompilierzeitfehler auf, wenn die spezifischere Methode, auf die der Methoden Zeiger verweist, schmaler ist als die Signatur des Delegaten. Eine Methode `M` gilt als schmaler als ein Delegattyp `D`, wenn Folgendes gilt:

* Der Parametertyp `M` hat eine erweiternde Konvertierung in den entsprechenden Parametertyp `D`.

* Der Rückgabetyp (sofern vorhanden) von `M` hat eine einschränkende Konvertierung in den Rückgabetyp `D`.

Wenn dem Methoden Zeiger Typargumente zugeordnet sind, werden nur Methoden mit der gleichen Anzahl von Typargumenten berücksichtigt. Wenn dem Methoden Zeiger keine Typargumente zugeordnet sind, wird der Typrückschluss verwendet, wenn Signaturen mit einer generischen Methode abgeglichen werden. Anders als bei einem anderen normalen Typrückschluss wird der Rückgabetyp des Delegaten beim Ableiten von Typargumenten verwendet, aber Rückgabe Typen werden immer noch nicht berücksichtigt, wenn die geringste generische Überladung bestimmt wird. Im folgenden Beispiel werden beide Methoden zum Bereitstellen eines Typarguments für einen delegaterstellungs-Ausdruck gezeigt:

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

Im obigen Beispiel wurde ein nicht generischer Delegattyp mithilfe einer generischen Methode instanziiert. Es ist auch möglich, mithilfe einer generischen Methode eine Instanz eines konstruierten Delegattyps zu erstellen. Zum Beispiel:

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

Wenn das Argument für den Ausdruck zur Delegaterstellung eine Lambda-Methode ist, muss die Lambda-Methode auf die Signatur des Delegattyps anwendbar sein. Eine Lambda-Methode `L` ist auf einen Delegattyp anwendbar `D`, wenn Folgendes zutrifft:

* Wenn `L` über Parameter verfügt, verfügt `D` über die gleiche Anzahl von Parametern. (Wenn `L` keine Parameter aufweist, werden die Parameter von `D` ignoriert.)

* Die Parametertypen von `L` verfügen jeweils über eine Konvertierung in den Typ des entsprechenden Parameter Typs von `D`, und ihre Modifizierer (d. h. `ByRef`, `ByVal`) stimmen überein.

* Wenn `D` eine Funktion ist, wird der Rückgabetyp `L` in den Rückgabetyp `D` konvertiert. (Wenn `D` eine Unterroutine ist, wird der Rückgabewert von `L` ignoriert.)

Wenn der Parametertyp eines Parameters von `L` weggelassen wird, wird der Typ des entsprechenden Parameters in `D` abgeleitet. Wenn der-Parameter von `L` Array-oder Werte zulässt-namensmodifizierer aufweist, wird ein Kompilierzeitfehler ausgegeben. Wenn alle Parametertypen von `L` verfügbar sind, wird der Typ des Ausdrucks in der Lambda-Methode abgeleitet. Zum Beispiel:

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

In einigen Situationen, in denen die Delegatsignatur nicht genau mit der Lambda-Methode oder der Methoden Signatur übereinstimmt, unterstützt die .NET Framework die Delegaterstellung möglicherweise nicht System intern. In dieser Situation wird ein Lambda-Methoden Ausdruck verwendet, um die beiden Methoden abzugleichen. Zum Beispiel:

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

Das Ergebnis eines Delegaten zum Erstellen von Delegaten ist eine Delegatinstanz, die auf die passende Methode mit dem zugeordneten Ziel Ausdruck (sofern vorhanden) aus dem Methoden Zeiger Ausdruck verweist. Wenn der Ziel Ausdruck als Werttyp typisiert ist, wird der Werttyp auf den System Heap kopiert, da ein Delegat nur auf eine Methode eines Objekts auf dem Heap verweisen kann. Die Methode und das Objekt, auf die ein Delegat verweist, bleiben für die gesamte Lebensdauer des Delegaten konstant. Anders ausgedrückt: Es ist nicht möglich, das Ziel oder das Objekt eines Delegaten zu ändern, nachdem es erstellt wurde.

### <a name="anonymous-object-creation-expressions"></a>Anonyme Objekt Erstellungs Ausdrücke

Ein Objekt Erstellungs Ausdruck mit Member-Initialisierern kann den Typnamen auch vollständig weglassen.

```antlr
AnonymousObjectCreationExpression
    : 'New' ObjectMemberInitializer
    ;
```

In diesem Fall wird ein anonymer Typ basierend auf den Typen und Namen der Member erstellt, die als Teil des Ausdrucks initialisiert werden. Zum Beispiel:

```vb
Module Test
    Sub Main()
        Dim Customer = New With { .Name = "John Smith", .Age = 34 }

        Console.WriteLine(Customer.Name)
    End Sub
End Module
```

Der von einem anonymen Objekt Erstellungs Ausdruck erstellte Typ ist eine Klasse, die über keinen Namen verfügt, direkt von `Object` erbt und über eine Reihe von Eigenschaften mit demselben Namen wie die in der Liste der Element Initialisierer zugewiesenen Member verfügt. Der Typ der einzelnen Eigenschaften wird mithilfe der gleichen Regeln wie der lokale variablentyprückschluss abgeleitet. Generierte anonyme Typen überschreiben auch `ToString` und geben eine Zeichen folgen Darstellung aller Member und ihrer Werte zurück. (Das genaue Format dieser Zeichenfolge überschreitet den Gültigkeitsbereich dieser Spezifikation).

Standardmäßig sind die vom anonymen Typ generierten Eigenschaften mit Lese-/Schreibzugriff. Es ist möglich, eine Eigenschaft für anonyme Typen mit dem `Key`-Modifizierer als schreibgeschützt zu markieren. Der `Key`-Modifizierer gibt an, dass das Feld verwendet werden kann, um den Wert, den der anonyme Typ darstellt, eindeutig zu identifizieren. Zusätzlich dazu, dass die Eigenschaft schreibgeschützt ist, bewirkt dies auch, dass der anonyme Typ `Equals` und `GetHashCode` überschreibt und die-Schnittstelle `System.IEquatable(Of T)` (die den anonymen Typ für `T` füllt) implementiert. Die Member werden wie folgt definiert:

`Function Equals(obj As Object) As Boolean` und `Function Equals(val As T) As Boolean` werden implementiert, indem überprüft wird, ob die beiden Instanzen denselben Typ haben und dann jedes `Key`-Element mit `Object.Equals` vergleicht. Wenn alle `Key`-Elemente gleich sind, gibt `Equals` `True` zurück, andernfalls gibt `Equals` `False` zurück.

`Function GetHashCode() As Integer` ist so implementiert, dass, wenn `Equals` für zwei Instanzen des anonymen Typs true ist, `GetHashCode` denselben Wert zurückgibt. Der Hashwert beginnt mit einem Ausgangswert und dann für jedes `Key`-Element, in der Reihenfolge multipliziert den Hash mit 31 und fügt den Hashwert des `Key`-Members (bereitgestellt von `GetHashCode`) hinzu, wenn der Member kein Verweistyp oder ein Werte zulässt-Werttyp mit dem Wert `Nothing` ist.

Beispielsweise der in der-Anweisung erstellte-Typ:

```vb
Dim zipState = New With { Key .ZipCode = 98112, .State = "WA" }
```

erstellt eine Klasse, die ungefähr so aussieht (obwohl die genaue Implementierung variieren kann):

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

Um die Situation zu vereinfachen, in der ein anonymer Typ aus den Feldern eines anderen Typs erstellt wird, können Feldnamen in den folgenden Fällen direkt von Ausdrücken abgeleitet werden:

* Ein einfacher namens Ausdruck `x` leitet den Namen `x` ab.

* Ein Member-Zugriffs Ausdruck `x.y` leitet den Namen `y` ab.

* Ein Wörterbuch-Suche-Ausdruck `x!y` leitet den Namen `y` ab.

* Ein Aufruf-oder Index Ausdruck ohne Argumente `x()` leitet den Namen `x` ab.

* Ein XML-Member-Zugriffs Ausdruck `x.<y>`, `x...<y>` `x.@y` leitet den Namen `y` ab.

* Ein XML-Member-Zugriffs Ausdruck, der das Ziel eines Element Zugriffs Ausdrucks ist `x.<y>.z` den Namen `z`.

* Ein XML-Member-Zugriffs Ausdruck, bei dem es sich um das Ziel eines aufzurufenden oder Index Ausdrucks ohne Argumente handelt `x.<y>.z()` den Namen `z`.

* Ein XML-Member-Zugriffs Ausdruck, bei dem es sich um das Ziel eines Aufruf-oder Index Ausdrucks handelt, `x.<y>(0)` den Namen `y` leitet.

Der Initialisierer wird als Zuweisung des Ausdrucks zum abzurufenden Namen interpretiert. Die folgenden Initialisierer sind z. b. äquivalent:

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

Wenn ein Elementname abgeleitet wird, der zu einem Konflikt mit einem vorhandenen Member des Typs führt (z. b. `GetHashCode`), tritt ein Kompilierzeitfehler auf. Im Gegensatz zu regulären Member-Initialisierern können Member-Initialisierer von anonymen Objekten keine Zirkel Verweise aufweisen oder auf einen Member verweisen, bevor dieser initialisiert wurde. Zum Beispiel:

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

Wenn zwei Ausdrücke der anonymen Klassen Erstellung innerhalb derselben Methode auftreten und die gleiche resultierende Form ergeben, wenn die Eigenschaften Reihenfolge, die Eigenschaften Namen und die Eigenschafts Typen alle übereinstimmen, verweisen beide auf dieselbe anonyme Klasse. Der Methoden Bereich einer Instanz oder einer freigegebenen Element Variablen mit einem Initialisierer ist der Konstruktor, in dem die Variable initialisiert wird.

__Nebenbei.__ Es ist möglich, dass ein Compiler anonyme Typen weiter vereinheitlichen kann, z. b. auf Assemblyebene, aber dies kann zu diesem Zeitpunkt nicht darauf beruhen.


## <a name="cast-expressions"></a>Umwandlungs Ausdrücke

Ein Cast Ausdruck wandelt einen Ausdruck in einen angegebenen Typ um. Bestimmte Umwandlungs Schlüsselwörter leiten Ausdrücke in die primitiven Typen um. Drei allgemeine Cast Schlüsselwörter ("`CType`", "`TryCast`" und "`DirectCast`" wandeln einen Ausdruck in einen Typ um.

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

`DirectCast` und `TryCast` weisen ein spezielles Verhalten auf. Aus diesem Grund unterstützen Sie nur native Konvertierungen. Außerdem kann der Zieltyp in einem `TryCast`-Ausdruck kein Werttyp sein. Benutzerdefinierte Konvertierungs Operatoren werden nicht berücksichtigt, wenn `DirectCast` oder `TryCast` verwendet wird. (__Hinweis:__ Der Konvertierungs Satz, der die Unterstützung von "`DirectCast`" und "`TryCast`" hat, ist eingeschränkt, da Sie "Native CLR" Der Zweck von `DirectCast` besteht darin, die Funktionalität der "Unbox"-Anweisung bereitzustellen, während `TryCast` die Funktionalität der "Isinst"-Anweisung bereitstellt. Da Sie CLR-Anweisungen zugeordnet sind, würden die von der CLR nicht direkt unterstützten Konvertierungen den beabsichtigten Zweck zunichte machen.

`DirectCast` konvertiert Ausdrücke, die als `Object` typisiert sind, anders als `CType`. Beim Umrechnen eines Ausdrucks vom Typ "`Object`", dessen Lauf Zeittyp ein primitiver Werttyp ist, löst "`DirectCast`" eine `System.InvalidCastException`-Ausnahme aus, wenn der angegebene Typ nicht mit dem Lauf Zeittyp des Ausdrucks identisch ist, oder ein `System.NullReferenceException`, wenn der Ausdruck als `Nothing` ausgewertet wird. (__Hinweis:__ Wie bereits erwähnt, wird "`DirectCast`" direkt auf der CLR-Anweisung "Unbox" zugeordnet, wenn der Typ des Ausdrucks `Object` ist. Im Gegensatz dazu wandelt sich `CType` in einen Aufrufer zur Laufzeit um, um die Konvertierung durchzuführen, sodass Konvertierungen zwischen primitiven Typen unterstützt werden können. Wenn ein `Object`-Ausdruck in einen primitiven Werttyp konvertiert wird und der Typ der tatsächlichen Instanz dem Zieltyp entspricht, ist `DirectCast` deutlich schneller als `CType`.)

`TryCast` konvertiert Ausdrücke, löst jedoch keine Ausnahme aus, wenn der Ausdruck nicht in den Zieltyp konvertiert werden kann. Stattdessen ergibt `TryCast` den `Nothing`, wenn der Ausdruck nicht zur Laufzeit konvertiert werden kann. (__Hinweis:__ Wie bereits erwähnt, wird `TryCast` direkt auf die CLR-Anweisung "Isinst" zuordnet. Durch die Kombination der Typüberprüfung und der Konvertierung in einen einzelnen Vorgang kann `TryCast` günstiger sein als eine `TypeOf ... Is` und dann eine `CType`.)

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

Wenn keine Konvertierung vom Typ des Ausdrucks in den angegebenen Typ vorhanden ist, tritt ein Kompilierzeitfehler auf. Andernfalls wird der Ausdruck als Wert klassifiziert, und das Ergebnis ist der Wert, der von der Konvertierung erzeugt wird.


## <a name="operator-expressions"></a>Operator Ausdrücke

Es gibt zwei Arten von Operatoren. *Unäre Operatoren* nehmen einen Operanden an und verwenden eine Präfix Notation (z. b. `-x`). *Binäre Operatoren* nehmen zwei Operanden an und verwenden die Infix-Notation (z. b. `x + y`). Mit Ausnahme der relationalen Operatoren, die immer `Boolean` ergeben, führt ein Operator, der für einen bestimmten Typ definiert ist, zu diesem Typ. Die Operanden für einen Operator müssen immer als Wert klassifiziert werden. Das Ergebnis eines Operator Ausdrucks wird als Wert klassifiziert.

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

Wenn ein Ausdruck mehrere binäre Operatoren enthält, steuert die *Rangfolge* der Operatoren die Reihenfolge, in der die einzelnen binären Operatoren ausgewertet werden. Der Ausdruck `x + y * z` wird z.B. als `x + (y * z)` ausgewertet, da der `*`-Operator Vorrang vor dem `+`-Operator hat. In der folgenden Tabelle sind die binären Operatoren in absteigender Rangfolge aufgeführt:


| __Kategorie__     | __Operatoren__                                          | 
|------------------|--------------------------------------------------------|
| Primär          | Alle nicht-Operator Ausdrücke                           |
| Await-            | `Await`                                                |
| Potenzierung   | `^`                                                    |
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

Wenn ein Ausdruck zwei Operatoren mit der gleichen Rangfolge enthält, steuert die *Assoziativität* der Operatoren die Reihenfolge, in der die Vorgänge ausgeführt werden. Alle binären Operatoren sind links assoziativ, was bedeutet, dass Vorgänge von links nach rechts ausgeführt werden. Rangfolge und Assoziativität können mithilfe von Klammern gesteuert werden.

### <a name="object-operands"></a>Objekt Operanden

Zusätzlich zu den regulären Typen, die von den einzelnen Operatoren unterstützt werden, unterstützen alle Operatoren Operanden vom Typ `Object`. Operanden, die auf `Object`-Operanden angewendet werden, werden ähnlich wie Methodenaufrufe für `Object`-Werte behandelt: ein spät gebundener Methodenaufruf kann ausgewählt werden. in diesem Fall bestimmt der Lauf Zeittyp der Operanden und nicht der Kompilier Zeittyp die Gültigkeit und den Typ des Betriebs. Wenn eine strikte Semantik von der Kompilierungs Umgebung oder `Option Strict` angegeben wird, verursachen Operatoren mit Operanden vom Typ "`Object`" einen Kompilierzeitfehler, mit Ausnahme der Operatoren "`TypeOf...Is`", "`Is`" und "`IsNot`".

Wenn die Operator Auflösung festlegt, dass ein Vorgang spät gebunden werden soll, ist das Ergebnis des Vorgangs das Ergebnis der Anwendung des Operators auf die Operandentypen, wenn die Lauf Zeit Typen der Operanden vom Operator unterstützte Typen sind. Der Wert `Nothing` wird als Standardwert des Typs des anderen Operanden in einem binären Operator Ausdruck behandelt. In einem unären Operator Ausdruck oder wenn beide Operanden in einem binären Operator Ausdruck `Nothing` sind, ist der Typ des Vorgangs `Integer` oder der einzige Ergebnistyp des Operators, wenn der Operator nicht `Integer` ergibt. Das Ergebnis des Vorgangs wird immer wieder in `Object` umgewandelt. Wenn die Operanden Typen keinen gültigen Operator aufweisen, wird eine Ausnahme vom Typ "`System.InvalidCastException`" ausgelöst. Konvertierungen werden zur Laufzeit ohne Berücksichtigung der impliziten oder expliziten Konvertierung durchgeführt.

Wenn das Ergebnis einer numerischen binären Operation eine Überlauf Ausnahme erzeugt (unabhängig davon, ob die ganzzahlige Überlauf Überprüfung ein-oder ausgeschaltet ist), wird der Ergebnistyp nach Möglichkeit auf den nächsten umfassenderen numerischen Typ herauf gestuft. Beachten Sie z. B. folgenden Code:

```vb
Module Test
    Sub Main()
        Dim o As Object = CObj(CByte(2)) * CObj(CByte(255))

        Console.WriteLine(o.GetType().ToString() & " = " & o)
    End Sub
End Module
```

Es gibt das folgende Ergebnis aus:

```console
System.Int16 = 512
```

Wenn kein größerer numerischer Typ zum Speichern der Zahl verfügbar ist, wird eine Ausnahme vom Typ "`System.OverflowException`" ausgelöst.

### <a name="operator-resolution"></a>Operator Auflösung

Bei einem Operatortyp und einem Satz von Operanden bestimmt die Operator Auflösung, welcher Operator für die Operanden verwendet werden soll. Beim Auflösen von Operatoren werden benutzerdefinierte Operatoren zunächst mit den folgenden Schritten in Erwägung gezogen:

1. Zunächst werden alle Kandidaten Operatoren gesammelt. Die Kandidaten Operatoren sind alle benutzerdefinierten Operatoren des jeweiligen Operator Typs im Quelltyp und alle benutzerdefinierten Operatoren des jeweiligen Typs im Zieltyp. Wenn der Quelltyp und der Zieltyp verknüpft sind, werden allgemeine Operatoren nur einmal berücksichtigt.

2. Anschließend wird die Überladungs Auflösung auf die Operatoren und Operanden angewendet, um den spezifischsten Operator auszuwählen. Im Fall von binären Operatoren kann dies zu einem spät gebundenen Aufrufer führen.

Beim Erfassen der Kandidaten Operatoren für einen Typ `T?` werden stattdessen die Operatoren vom Typ `T` verwendet. Alle benutzerdefinierten Operatoren von `T`, die nur nicht auf NULL festleg Bare Werttypen einschließen, werden ebenfalls angehoben. Ein angehobene Operator verwendet die NULL-Werte, die auf NULL festgelegt werden können, mit Ausnahme der Rückgabe Typen von `IsTrue` und `IsFalse` (die `Boolean` sein müssen). Aufgenommene Operatoren werden ausgewertet, indem die Operanden in Ihre nicht auf NULL festleg Bare Version umgerechnet werden. Anschließend wird der benutzerdefinierte Operator ausgewertet und der Ergebnistyp in seine Version, die NULL-Werte zulässt, transformiert. Wenn der Äther Operand `Nothing` ist, ist das Ergebnis des Ausdrucks ein Wert von `Nothing`, der als auf NULL festleg Bare Version des Ergebnis Typs typisiert ist. Zum Beispiel:

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

Wenn der Operator ein binärer Operator ist und einer der Operanden ein Referenztyp ist, wird der Operator ebenfalls angehoben, aber jede Bindung an den Operator erzeugt einen Fehler. Zum Beispiel:

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

__Nebenbei.__ Diese Regel ist vorhanden, da berücksichtigt wird, ob in einer zukünftigen Version NULL-propagierende Verweis Typen hinzugefügt werden sollen. in diesem Fall würde sich das Verhalten bei binären Operatoren zwischen den beiden Typen ändern.

Wie bei Konvertierungen werden benutzerdefinierte Operatoren immer als bevorzugte Operatoren bevorzugt.

Beim Auflösen überladener Operatoren kann es Unterschiede zwischen Klassen geben, die in Visual Basic definiert sind, und in anderen Sprachen definierte Klassen:

* In anderen Sprachen können `Not`, `And` und `Or` sowohl als logische Operatoren als auch als bitweise Operatoren überladen werden. Beim Importieren aus einer externen Assembly wird jedes Formular als gültige Überladung für diese Operatoren akzeptiert. Für einen Typ, der sowohl logische als auch bitweise Operatoren definiert, wird jedoch nur die bitweise Implementierung berücksichtigt.

* In anderen Sprachen können `>>` und `<<` sowohl als signierte Operatoren als auch als nicht signierte Operatoren überladen werden. Beim Importieren aus einer externen Assembly wird jedes Formular als gültige Überladung akzeptiert. Für einen Typ, der sowohl signierte als auch nicht signierte Operatoren definiert, wird jedoch nur die signierte Implementierung berücksichtigt.

* Wenn kein benutzerdefinierter Operator für die Operanden am spezifischsten ist, werden intrinsische Operatoren berücksichtigt. Wenn kein System interner Operator für die Operanden definiert ist und der Operand einen typobjekttyp aufweist, wird der Operator spät gebunden aufgelöst. Andernfalls wird ein Fehler bei der Kompilierzeit ausgegeben.

In früheren Versionen von Visual Basic war es ein Fehler, wenn genau ein Operand vom Typ "Object" und keine anwendbaren benutzerdefinierten Operatoren und keine anwendbaren intrinsischen Operatoren vorhanden waren. Ab Visual Basic 11 ist die Lösung spät gebunden. Zum Beispiel:

```vb
Module Module1
  Sub Main()
      Dim p As Object = Nothing
      Dim U As New Uri("http://www.microsoft.com")
      Dim j = U * p  ' is now resolved late-bound
   End Sub
End Module
```

Ein Typ `T` mit einem intrinsischen Operator definiert auch denselben Operator für `T?`. Das Ergebnis des-Operators auf `T?` ist identisch mit dem-Wert für `T`, mit dem Unterschied, dass, wenn einer der beiden Operanden `Nothing` ist, das Ergebnis des Operators `Nothing` ist (d. h., der NULL-Wert wird weitergegeben). Um den Typ eines Vorgangs aufzulösen, wird der `?` aus allen darin befindlichen Operanden entfernt, der Typ des Vorgangs wird bestimmt, und dem Typ des Vorgangs wird ein `?` hinzugefügt, wenn einer der Operanden auf NULL festleg Bare Werttypen wäre. Zum Beispiel:

```vb
Dim v1? As Integer = 10
Dim v2 As Long = 20

' Type of operation will be Long?
Console.WriteLine(v1 + v2)
```

Jeder Operator listet die systeminternen Typen, für die er definiert ist, und den Typ des Vorgangs auf, der mit den Operanden Typen ausgeführt wird. Das Ergebnis des Typs eines intrinsischen Vorgangs folgt den folgenden allgemeinen Regeln:

* Wenn alle Operanden denselben Typ haben und der Operator für den Typ definiert ist, erfolgt keine Konvertierung, und der Operator für diesen Typ wird verwendet.

* Alle Operanden, deren Typ nicht für den-Operator definiert ist, werden mithilfe der folgenden Schritte konvertiert, und der-Operator wird für die neuen Typen aufgelöst:

  * Der Operand wird in den nächstgrößten Typ konvertiert, der sowohl für den Operator als auch den Operanden definiert ist und in den er implizit konvertiert werden kann.

  * Wenn kein solcher Typ vorhanden ist, wird der Operand in den nächst engsten Typ konvertiert, der sowohl für den Operator als auch den Operanden und für den Operanden definiert ist, und an den er implizit konvertierbar ist.

  * Wenn kein solcher Typ vorhanden ist oder die Konvertierung nicht durchgeführt werden kann, tritt ein Kompilierzeitfehler auf.

* Andernfalls werden die Operanden in die breitere der Operandentypen konvertiert, und der Operator für diesen Typ wird verwendet. Wenn der Typ des engeren Operanden nicht implizit in den Typ "breiter Operator" konvertiert werden kann, tritt ein Kompilierzeitfehler auf.

Trotz dieser allgemeinen Regeln gibt es jedoch eine Reihe von besonderen Fällen, die in den operatorergebnistabellen genannt werden.

__Nebenbei.__ Aus Formatierungs Gründen kürzen die Operatortyp Tabellen die vordefinierten Namen auf die ersten beiden Zeichen. "By" ist also `Byte`, "UI" ist `UInteger`, "St" `String` usw. "Err" bedeutet, dass für die angegebenen Operanden Typen kein Vorgang definiert ist.

## <a name="arithmetic-operators"></a>Arithmetische Operatoren

Die Operatoren `*`, `/`, `\`, `^`, `Mod`, `+` und `-` sind die *arithmetischen Operatoren*.

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

Arithmetische Operationen für Gleit Komma Zahlen können mit höherer Genauigkeit als der Ergebnistyp des Vorgangs ausgeführt werden. Beispielsweise unterstützen einige Hardwarearchitekturen einen "Extended"-oder "long Double"-Gleit kommatyp mit größerem Bereich und präziser als der `Double`-Typ und führen implizit alle Gleit Komma Vorgänge mit diesem Typ mit höherer Genauigkeit aus. Hardware Architekturen können zur Durchführung von Gleit Komma Vorgängen mit geringerer Genauigkeit nur zu hohen Leistungseinbußen erstellt werden. anstatt eine Implementierung zum Verlust von Leistung und Genauigkeit zu benötigen, Visual Basic ermöglicht, dass der Typ mit höherer Genauigkeit für alle Gleit Komma Vorgänge verwendet wird. Abgesehen von der Bereitstellung präziseren Ergebnisse hat dies nur selten messbare Auswirkungen. In Ausdrücken der Form `x * y / z`, wobei die Multiplikation ein Ergebnis erzeugt, das außerhalb des `Double`-Bereichs liegt, aber die nachfolgende Division das temporäre Ergebnis wieder in den `Double`-Bereich bringt, die Tatsache, dass der Ausdruck in einem das Format eines höheren Bereichs kann dazu führen, dass anstelle von unendlich ein endliches Ergebnis erzeugt wird.


### <a name="unary-plus-operator"></a>Unärer Plus-Operator

```antlr
UnaryPlusExpression
    : '+' Expression
    ;
```

Der unäre Plus-Operator ist für die Typen "`Byte`", "`SByte`", "`UShort`", "`Short`", "`UInteger`", "`Integer`", "`ULong`", "`Long`" und "0" definiert.

__Vorgangstyp:__


| __Ur__ | __SB__ | __Am__ | __Sh__ | __VORLIEGENDEN__ | __In__ | __ANGETAN__ | __Lo__ | __NÜTZLICHEN__ | __Liga__ | __Si__ | __Do__ | __De__  | __Ch__  | __St__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| Sh | SB | um | Sh | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do | Johannis | 


### <a name="unary-minus-operator"></a>Unärer Minus-Operator

```antlr
UnaryMinusExpression
    : '-' Expression
    ;
```

Der unäre Minus-Operator ist für die folgenden Typen definiert:

`SByte`, `Short`, `Integer`und `Long`. Das Ergebnis wird durch Subtrahieren des Operanden von 0 (null) berechnet. Wenn die ganzzahlige Überlauf Überprüfung auf on und der Wert des Operanden das Maximum negative `SByte`, `Short`, `Integer` oder `Long` ist, wird eine `System.OverflowException`-Ausnahme ausgelöst. Andernfalls ist der Wert, wenn der Wert des Operanden der maximale negative `SByte`, `Short`, `Integer` oder `Long` ist, derselbe Wert, und der Überlauf wird nicht gemeldet.

`Single` und `Double`. Das Ergebnis ist der Wert des Operanden mit dem zugehörigen Vorzeichen, einschließlich der Werte 0 und unendlich. Wenn der Operand "NaN" ist, ist das Ergebnis ebenfalls "NaN".

`Decimal`. installiert haben. Das Ergebnis wird durch Subtrahieren des Operanden von 0 (null) berechnet.

__Vorgangstyp:__

| __Ur__ | __SB__ | __Am__ | __Sh__ | __VORLIEGENDEN__ | __In__ | __ANGETAN__ | __Lo__ | __NÜTZLICHEN__ | __Liga__ | __Si__ | __Do__ | __De__  | __Ch__  | __St__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| Sh | SB | Sh | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do | Johannis | 


### <a name="addition-operator"></a>Additions Operator

Der Additions Operator berechnet die Summe der beiden Operanden.

```antlr
AdditionOperatorExpression
    : Expression '+' LineTerminator? Expression
    ;
```

Der Additions Operator ist für die folgenden Typen definiert:

* `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` und `Long`. Wenn die ganzzahlige Überlauf Überprüfung auf on und die Summe außerhalb des Bereichs des Ergebnis Typs liegt, wird eine Ausnahme vom Typ "`System.OverflowException`" ausgelöst. Andernfalls werden Überläufe nicht gemeldet, und alle signifikanten höherwertigen Bits des Ergebnisses werden verworfen.

* `Single` und `Double`. Die Summe wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.

* `Decimal`. installiert haben. Wenn der resultierende Wert zu groß ist, um im Dezimal Format dargestellt zu werden, wird eine Ausnahme vom Typ "`System.OverflowException`" ausgelöst. Wenn der Ergebniswert zu klein ist, um im Dezimal Format darzustellen, ist das Ergebnis 0 (null).

* `String`. installiert haben. Die beiden Operanden `String` werden zusammen verkettet.

* `Date`. installiert haben. Der `System.DateTime`-Typ definiert überladene Additions Operatoren. Da `System.DateTime` dem intrinsischen `Date`-Typ entspricht, sind diese Operatoren auch für den `Date`-Typ verfügbar.

__Vorgangstyp:__

|        | __Ur__ | __SB__ | __Am__ | __Sh__ | __VORLIEGENDEN__ | __In__ | __ANGETAN__ | __Lo__ | __NÜTZLICHEN__ | __Liga__ | __Si__ | __Do__ | __De__  | __Ch__  | __St__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| __Ur__ | Sh | SB | Sh | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do | Johannis | 
| __SB__ |    | SB | Sh | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do | Johannis | 
| __Am__ |    |    | um | Sh | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do | Johannis | 
| __Sh__ |    |    |    | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do | Johannis | 
| __VORLIEGENDEN__ |    |    |    |    | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do | Johannis | 
| __In__ |    |    |    |    |    | In | Lo | Lo | De | De | Si | Do | Err | Err | Do | Johannis | 
| __ANGETAN__ |    |    |    |    |    |    | UI | Lo | UL | De | Si | Do | Err | Err | Do | Johannis | 
| __Lo__ |    |    |    |    |    |    |    | Lo | De | De | Si | Do | Err | Err | Do | Johannis | 
| __NÜTZLICHEN__ |    |    |    |    |    |    |    |    | UL | De | Si | Do | Err | Err | Do | Johannis | 
| __Liga__ |    |    |    |    |    |    |    |    |    | De | Si | Do | Err | Err | Do | Johannis | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Si | Do | Err | Err | Do | Johannis | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do | Johannis | 
| __De__ |    |    |    |    |    |    |    |    |    |    |    |    | St  | Err | St | Johannis | 
| __Ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | St  | St | Johannis | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | St | Johannis | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | Johannis | 


### <a name="subtraction-operator"></a>Subtraktions Operator

Der Subtraktions Operator subtrahiert den zweiten Operanden vom ersten Operanden.

```antlr
SubtractionOperatorExpression
    : Expression '-' LineTerminator? Expression
    ;
```

Der Subtraktions Operator ist für die folgenden Typen definiert:

* `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` und `Long`. Wenn die ganzzahlige Überlauf Überprüfung auf on und der Unterschied außerhalb des Bereichs des Ergebnis Typs liegt, wird eine Ausnahme vom Typ "`System.OverflowException`" ausgelöst. Andernfalls werden Überläufe nicht gemeldet, und alle signifikanten höherwertigen Bits des Ergebnisses werden verworfen.

* `Single` und `Double`. Der Unterschied wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.

* `Decimal`. installiert haben. Wenn der resultierende Wert zu groß ist, um im Dezimal Format dargestellt zu werden, wird eine Ausnahme vom Typ "`System.OverflowException`" ausgelöst. Wenn der Ergebniswert zu klein ist, um im Dezimal Format darzustellen, ist das Ergebnis 0 (null).

* `Date`. installiert haben. Der `System.DateTime`-Typ definiert überladene Subtraktions Operatoren. Da `System.DateTime` dem intrinsischen `Date`-Typ entspricht, sind diese Operatoren auch für den `Date`-Typ verfügbar.

__Vorgangstyp:__

|        | __Ur__ | __SB__ | __Am__ | __Sh__ | __VORLIEGENDEN__ | __In__ | __ANGETAN__ | __Lo__ | __NÜTZLICHEN__ | __Liga__ | __Si__ | __Do__ | __De__  | __Ch__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __Ur__ | Sh | SB | Sh | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Johannis  | 
| __SB__ |    | SB | Sh | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Johannis  | 
| __Am__ |    |    | um | Sh | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Johannis  | 
| __Sh__ |    |    |    | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Johannis  | 
| __VORLIEGENDEN__ |    |    |    |    | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Johannis  | 
| __In__ |    |    |    |    |    | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Johannis  | 
| __ANGETAN__ |    |    |    |    |    |    | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Johannis  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | De | De | Si | Do | Err | Err | Do  | Johannis  | 
| __NÜTZLICHEN__ |    |    |    |    |    |    |    |    | UL | De | Si | Do | Err | Err | Do  | Johannis  | 
| __Liga__ |    |    |    |    |    |    |    |    |    | De | Si | Do | Err | Err | Do  | Johannis  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Si | Do | Err | Err | Do  | Johannis  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do  | Johannis  | 
| __De__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __Ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Do  | Johannis  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Johannis  | 


### <a name="multiplication-operator"></a>Multiplikations Operator

Der Multiplikations Operator berechnet das Produkt von zwei-Operanden.

```antlr
MultiplicationOperatorExpression
    : Expression '*' LineTerminator? Expression
    ;
```

Der Multiplikations Operator ist für die folgenden Typen definiert:

* `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` und `Long`. Wenn die ganzzahlige Überlauf Überprüfung auf on und das Produkt außerhalb des Bereichs des Ergebnis Typs liegt, wird eine Ausnahme vom Typ "`System.OverflowException`" ausgelöst. Andernfalls werden Überläufe nicht gemeldet, und alle signifikanten höherwertigen Bits des Ergebnisses werden verworfen.

* `Single` und `Double`. Das Produkt wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.

* `Decimal`. installiert haben. Wenn der resultierende Wert zu groß ist, um im Dezimal Format dargestellt zu werden, wird eine Ausnahme vom Typ "`System.OverflowException`" ausgelöst. Wenn der Ergebniswert zu klein ist, um im Dezimal Format darzustellen, ist das Ergebnis 0 (null).

__Vorgangstyp:__

|        | __Ur__ | __SB__ | __Am__ | __Sh__ | __VORLIEGENDEN__ | __In__ | __ANGETAN__ | __Lo__ | __NÜTZLICHEN__ | __Liga__ | __Si__ | __Do__ | __De__  | __Ch__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __Ur__ | Sh | SB | Sh | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Johannis  | 
| __SB__ |    | SB | Sh | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Johannis  | 
| __Am__ |    |    | um | Sh | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Johannis  | 
| __Sh__ |    |    |    | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Johannis  | 
| __VORLIEGENDEN__ |    |    |    |    | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Johannis  | 
| __In__ |    |    |    |    |    | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Johannis  | 
| __ANGETAN__ |    |    |    |    |    |    | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Johannis  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | De | De | Si | Do | Err | Err | Do  | Johannis  | 
| __NÜTZLICHEN__ |    |    |    |    |    |    |    |    | UL | De | Si | Do | Err | Err | Do  | Johannis  | 
| __Liga__ |    |    |    |    |    |    |    |    |    | De | Si | Do | Err | Err | Do  | Johannis  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Si | Do | Err | Err | Do  | Johannis  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do  | Johannis  | 
| __De__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __Ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Do  | Johannis  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Johannis  | 


### <a name="division-operators"></a>Divisions Operatoren

Divisions Operatoren berechnen den Quotienten von zwei Operanden. Es gibt zwei Divisions Operatoren: den regulären Operator (Gleit Komma) und den Operator für die ganzzahlige Division.

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

Der Operator für reguläre Division ist für die folgenden Typen definiert:

* `Single` und `Double`. Der Quotienten wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.

* `Decimal`. installiert haben. Wenn der Wert des rechten Operanden 0 (null) ist, wird eine Ausnahme vom Typ "`System.DivideByZeroException`" ausgelöst. Wenn der resultierende Wert zu groß ist, um im Dezimal Format dargestellt zu werden, wird eine Ausnahme vom Typ "`System.OverflowException`" ausgelöst. Wenn der Ergebniswert zu klein ist, um im Dezimal Format darzustellen, ist das Ergebnis 0 (null). Die Dezimalstellen des Ergebnisses, vor der Rundung, sind die nächstgelegene Skala für die bevorzugte Skala, die ein Ergebnis gleich dem exakten Ergebnis behält.  Die bevorzugte Skala ist die Skala des ersten Operanden abzüglich der Skala des zweiten Operanden.

Gemäß den normalen Operator Auflösungs Regeln würden reguläre Teilungen, die ausschließlich zwischen Operanden von Typen wie `Byte`, `Short`, `Integer` und `Long` stehen, dazu führen, dass beide Operanden in den Typ `Decimal` konvertiert werden. Wenn jedoch die Operator Auflösung für den Divisions Operator ausgeführt wird, wenn keiner der Typen `Decimal` ist, gilt `Double` als schmaler als `Decimal`. Diese Konvention wird befolgt, weil die Division von `Double` effizienter ist als `Decimal`-Division.

__Vorgangstyp:__

|        | __Ur__ | __SB__ | __Am__ | __Sh__ | __VORLIEGENDEN__ | __In__ | __ANGETAN__ | __Lo__ | __NÜTZLICHEN__ | __Liga__ | __Si__ | __Do__ | __De__  | __Ch__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __Ur__ | Do | Do | Do | Do | Do | Do | Do | Do | Do | De | Si | Do | Err | Err | Do  | Johannis  | 
| __SB__ |    | Do | Do | Do | Do | Do | Do | Do | Do | De | Si | Do | Err | Err | Do  | Johannis  | 
| __Am__ |    |    | Do | Do | Do | Do | Do | Do | Do | De | Si | Do | Err | Err | Do  | Johannis  | 
| __Sh__ |    |    |    | Do | Do | Do | Do | Do | Do | De | Si | Do | Err | Err | Do  | Johannis  | 
| __VORLIEGENDEN__ |    |    |    |    | Do | Do | Do | Do | Do | De | Si | Do | Err | Err | Do  | Johannis  | 
| __In__ |    |    |    |    |    | Do | Do | Do | Do | De | Si | Do | Err | Err | Do  | Johannis  | 
| __ANGETAN__ |    |    |    |    |    |    | Do | Do | Do | De | Si | Do | Err | Err | Do  | Johannis  | 
| __Lo__ |    |    |    |    |    |    |    | Do | Do | De | Si | Do | Err | Err | Do  | Johannis  | 
| __NÜTZLICHEN__ |    |    |    |    |    |    |    |    | Do | De | Si | Do | Err | Err | Do  | Johannis  | 
| __Liga__ |    |    |    |    |    |    |    |    |    | De | Si | Do | Err | Err | Do  | Johannis  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Si | Do | Err | Err | Do  | Johannis  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do  | Johannis  | 
| __De__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __Ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Do  | Johannis  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Johannis  | 

Der Operator für die ganzzahlige Division ist für `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` und `Long` definiert. Wenn der Wert des rechten Operanden 0 (null) ist, wird eine Ausnahme vom Typ "`System.DivideByZeroException`" ausgelöst. Die Division rundet das Ergebnis auf NULL, und der absolute Wert des Ergebnisses ist die größtmögliche Ganzzahl, die kleiner ist als der absolute Wert des Quotienten der beiden Operanden. Das Ergebnis ist 0 (null) oder positiv, wenn die beiden Operanden dasselbe Vorzeichen aufweisen, und 0 (null) oder negativ, wenn die beiden Operanden gegenüberliegende Vorzeichen verfügen. Wenn der linke Operand der maximale negative `SByte`, `Short`, `Integer` oder `Long` und der rechte Operand `-1` ist, tritt ein Überlauf auf. Wenn die ganzzahlige Überlauf Überprüfung on ist, wird eine `System.OverflowException`-Ausnahme ausgelöst. Andernfalls wird der Überlauf nicht gemeldet, und das Ergebnis ist stattdessen der Wert des linken Operanden.

__Nebenbei.__ Da die beiden Operanden für nicht signierte Typen immer 0 (null) oder positiv sind, ist das Ergebnis immer 0 (null) oder positiv.  Da das Ergebnis des Ausdrucks immer kleiner oder gleich dem größten der beiden Operanden ist, ist es nicht möglich, dass ein Überlauf auftritt.  Da eine solche ganzzahlige Überlauf Überprüfung nicht für eine ganzzahlige Teilung mit zwei Ganzzahlen ohne Vorzeichen ausgeführt wird. Das Ergebnis ist der-Typ des linken Operanden.


__Vorgangstyp:__

|        | __Ur__ | __SB__ | __Am__ | __Sh__ | __VORLIEGENDEN__ | __In__ | __ANGETAN__ | __Lo__ | __NÜTZLICHEN__ | __Liga__ | __Si__ | __Do__ | __De__  | __Ch__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __Ur__ | Sh | SB | Sh | Sh | In | In | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Johannis  | 
| __SB__ |    | SB | Sh | Sh | In | In | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Johannis  | 
| __Am__ |    |    | um | Sh | US | In | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Johannis  | 
| __Sh__ |    |    |    | Sh | In | In | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Johannis  | 
| __VORLIEGENDEN__ |    |    |    |    | US | In | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Johannis  | 
| __In__ |    |    |    |    |    | In | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Johannis  | 
| __ANGETAN__ |    |    |    |    |    |    | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Johannis  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Johannis  | 
| __NÜTZLICHEN__ |    |    |    |    |    |    |    |    | UL | Lo | Lo | Lo | Err | Err | Lo  | Johannis  | 
| __Liga__ |    |    |    |    |    |    |    |    |    | Lo | Lo | Lo | Err | Err | Lo  | Johannis  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Lo | Lo | Err | Err | Lo  | Johannis  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Lo | Err | Err | Lo  | Johannis  | 
| __De__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __Ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Lo  | Johannis  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Johannis  | 


### <a name="mod-operator"></a>Operator Mod

Der `Mod` (Modulo)-Operator berechnet den Rest der Division zwischen zwei Operanden.

```antlr
ModuloOperatorExpression
    : Expression 'Mod' LineTerminator? Expression
    ;
```

Der `Mod`-Operator ist für die folgenden Typen definiert:

* `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` und `Long`. Das Ergebnis von `x Mod y` ist der Wert, der von `x - (x \ y) * y` erzeugt wird. Wenn `y` 0 (null) ist, wird eine Ausnahme vom Typ "`System.DivideByZeroException`" ausgelöst. Der Modulo-Operator verursacht nie einen Überlauf.

* `Single` und `Double`. Der Rest wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.

* `Decimal`. installiert haben. Wenn der Wert des rechten Operanden 0 (null) ist, wird eine Ausnahme vom Typ "`System.DivideByZeroException`" ausgelöst. Wenn der resultierende Wert zu groß ist, um im Dezimal Format dargestellt zu werden, wird eine Ausnahme vom Typ "`System.OverflowException`" ausgelöst. Wenn der Ergebniswert zu klein ist, um im Dezimal Format darzustellen, ist das Ergebnis 0 (null).

__Vorgangstyp:__

|        | __Ur__ | __SB__ | __Am__ | __Sh__ | __VORLIEGENDEN__ | __In__ | __ANGETAN__ | __Lo__ | __NÜTZLICHEN__ | __Liga__ | __Si__ | __Do__ | __De__  | __Ch__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __Ur__ | Sh | SB | Sh | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Johannis  | 
| __SB__ |    | SB | Sh | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Johannis  | 
| __Am__ |    |    | um | Sh | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Johannis  | 
| __Sh__ |    |    |    | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Johannis  | 
| __VORLIEGENDEN__ |    |    |    |    | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Johannis  | 
| __In__ |    |    |    |    |    | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Johannis  | 
| __ANGETAN__ |    |    |    |    |    |    | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Johannis  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | De | De | Si | Do | Err | Err | Do  | Johannis  | 
| __NÜTZLICHEN__ |    |    |    |    |    |    |    |    | UL | De | Si | Do | Err | Err | Do  | Johannis  | 
| __Liga__ |    |    |    |    |    |    |    |    |    | De | Si | Do | Err | Err | Do  | Johannis  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Si | Do | Err | Err | Do  | Johannis  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do  | Johannis  | 
| __De__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __Ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Do  | Johannis  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Johannis  | 


### <a name="exponentiation-operator"></a>Exponentialoperator

Der Exponentialoperator berechnet den ersten Operanden, der in die Potenz des zweiten Operanden ausgelöst wird.

```antlr
ExponentOperatorExpression
    : Expression '^' LineTerminator? Expression
    ;
```

Der exponentiations Operator ist für den Typ `Double` definiert. Der Wert wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.

__Vorgangstyp:__

|        | __Ur__ | __SB__ | __Am__ | __Sh__ | __VORLIEGENDEN__ | __In__ | __ANGETAN__ | __Lo__ | __NÜTZLICHEN__ | __Liga__ | __Si__ | __Do__ | __De__  | __Ch__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __Ur__ | Do | Do | Do | Do | Do | Do | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Johannis  | 
| __SB__ |    | Do | Do | Do | Do | Do | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Johannis  | 
| __Am__ |    |    | Do | Do | Do | Do | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Johannis  | 
| __Sh__ |    |    |    | Do | Do | Do | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Johannis  | 
| __VORLIEGENDEN__ |    |    |    |    | Do | Do | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Johannis  | 
| __In__ |    |    |    |    |    | Do | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Johannis  | 
| __ANGETAN__ |    |    |    |    |    |    | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Johannis  | 
| __Lo__ |    |    |    |    |    |    |    | Do | Do | Do | Do | Do | Err | Err | Do  | Johannis  | 
| __NÜTZLICHEN__ |    |    |    |    |    |    |    |    | Do | Do | Do | Do | Err | Err | Do  | Johannis  | 
| __Liga__ |    |    |    |    |    |    |    |    |    | Do | Do | Do | Err | Err | Do  | Johannis  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Do | Do | Err | Err | Do  | Johannis  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do  | Johannis  | 
| __De__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __Ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Do  | Johannis  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Johannis  | 


## <a name="relational-operators"></a>Relationale Operatoren

Die *relationalen Operatoren* vergleichen Werte miteinander. Die Vergleichs Operatoren sind `=`, `<>`, `<`, `>`, `<=` und `>=`.

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

Alle relationalen Operatoren führen zu einem `Boolean`-Wert.

Die relationalen Operatoren haben die folgende allgemeine Bedeutung:

* Der `=`-Operator testet, ob die beiden Operanden gleich sind.

* Der `<>`-Operator testet, ob die beiden Operanden nicht gleich sind.

* Der `<`-Operator testet, ob der erste Operand kleiner als der zweite Operand ist.

* Der `>`-Operator testet, ob der erste Operand größer als der zweite Operand ist.

* Der `<=`-Operator testet, ob der erste Operand kleiner als oder gleich dem zweiten Operand ist.

* Der `>=`-Operator testet, ob der erste Operand größer oder gleich dem zweiten Operand ist.

Die relationalen Operatoren sind für die folgenden Typen definiert:

* `Boolean`. installiert haben. Die Operatoren vergleichen die Wahrheits Werte der beiden Operanden. `True` gilt als kleiner als `False`, was mit den numerischen Werten übereinstimmt.

* `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` und `Long`. Die Operatoren vergleichen die numerischen Werte der beiden ganzzahligen Operanden.

* `Single` und `Double`. Die Operanden vergleichen die Operanden gemäß den Regeln des IEEE 754-Standards.

* `Decimal`. installiert haben. Die Operatoren vergleichen die numerischen Werte der zwei Dezimal Operanden.

* `Date`. installiert haben. Die Operatoren geben das Ergebnis des Vergleichs der beiden Datums-/Uhrzeitwerte zurück.

* `Char`. installiert haben. Die Operatoren geben das Ergebnis des Vergleichs der beiden Unicode-Werte zurück.

* `String`. installiert haben. Die Operatoren geben das Ergebnis des Vergleichs der beiden Werte entweder mithilfe eines binären Vergleichs oder eines Text Vergleichs zurück. Der verwendete Vergleich wird von der Kompilierungs Umgebung und der `Option Compare`-Anweisung bestimmt. Ein binärer Vergleich bestimmt, ob der numerische Unicode-Wert jedes Zeichens in jeder Zeichenfolge identisch ist. Ein Textvergleich führt einen Unicode-Textvergleich basierend auf der aktuellen Kultur aus, die für die .NET Framework verwendet wird. Wenn Sie einen Zeichen folgen Vergleich durchgeführt haben, entspricht ein NULL-Wert dem Zeichenfolgenliteralwert `""`.

__Vorgangstyp:__
        
|        | __Ur__ | __SB__ | __Am__ | __Sh__ | __VORLIEGENDEN__ | __In__ | __ANGETAN__ | __Lo__ | __NÜTZLICHEN__ | __Liga__ | __Si__ | __Do__ | __De__  | __Ch__  | __St__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| __Ur__ | Ur | SB | Sh | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Ur | Johannis | 
| __SB__ |    | SB | Sh | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do | Johannis | 
| __Am__ |    |    | um | Sh | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do | Johannis | 
| __Sh__ |    |    |    | Sh | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do | Johannis | 
| __VORLIEGENDEN__ |    |    |    |    | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do | Johannis | 
| __In__ |    |    |    |    |    | In | Lo | Lo | De | De | Si | Do | Err | Err | Do | Johannis | 
| __ANGETAN__ |    |    |    |    |    |    | UI | Lo | UL | De | Si | Do | Err | Err | Do | Johannis | 
| __Lo__ |    |    |    |    |    |    |    | Lo | De | De | Si | Do | Err | Err | Do | Johannis | 
| __NÜTZLICHEN__ |    |    |    |    |    |    |    |    | UL | De | Si | Do | Err | Err | Do | Johannis | 
| __Liga__ |    |    |    |    |    |    |    |    |    | De | Si | Do | Err | Err | Do | Johannis | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Si | Do | Err | Err | Do | Johannis | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do | Johannis | 
| __De__ |    |    |    |    |    |    |    |    |    |    |    |    | De  | Err | De | Johannis | 
| __Ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Ch  | St | Johannis | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | St | Johannis | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | Johannis | 


## <a name="like-operator"></a>Like-Operator

Der `Like`-Operator bestimmt, ob eine Zeichenfolge mit einem angegebenen Muster übereinstimmt.

```antlr
LikeOperatorExpression
    : Expression 'Like' LineTerminator? Expression
    ;
```

Der `Like`-Operator ist für den `String`-Typ definiert. Der erste Operand ist die übereinstimmende Zeichenfolge, und der zweite Operand ist das Muster für die Übereinstimmung. Das Muster besteht aus Unicode-Zeichen. Die folgenden Zeichen folgen haben eine besondere Bedeutung:

* Das Zeichen `?` entspricht einem beliebigen einzelnen Zeichen.

* Das Zeichen `*` entspricht 0 (null) oder mehr Zeichen.

* Das Zeichen `#` entspricht einer beliebigen einzelnen Ziffer (0-9).

* Eine Liste von Zeichen, die von eckigen Klammern (`[ab...]`) umgeben sind, entspricht einem beliebigen einzelnen Zeichen in der Liste.

* Eine Liste von Zeichen, die von eckigen Klammern eingeschlossen und mit vorangestelltem Ausrufezeichen (`[!ab...]`) vorangestellt werden, stimmt mit jedem beliebigen Zeichen in der Zeichen Liste überein.

* Zwei Zeichen in einer Zeichen Liste, getrennt durch einen Bindestrich (`-`), geben einen Bereich von Unicode-Zeichen an, beginnend mit dem ersten Zeichen und endende mit dem zweiten Zeichen. Wenn das zweite Zeichen nicht später in der Sortierreihenfolge als das erste Zeichen ist, tritt eine Lauf Zeit Ausnahme auf. Ein Bindestrich, der am Anfang oder Ende einer Zeichen Liste angezeigt wird, gibt sich selbst an.

Zum Abgleichen der Sonderzeichen Left eckige Klammer (`[`), Fragezeichen (`?`), Nummern Zeichen (`#`) und Sternchen (`*`) müssen eckige Klammern diese einschließen. Die rechte eckige Klammer (`]`) kann nicht innerhalb einer Gruppe verwendet werden, um sich selbst abzugleichen, Sie kann jedoch außerhalb einer Gruppe als einzelnes Zeichen verwendet werden. Die Zeichenfolge `[]` wird als Zeichenfolgenliteralzeichen `""` betrachtet. 

Beachten Sie, dass die Zeichen Vergleiche und die Reihenfolge von Zeichen Listen vom Typ der verwendeten Vergleiche abhängig sind. Wenn binäre Vergleiche verwendet werden, basieren die Zeichen Vergleiche und die Reihenfolge auf den numerischen Unicode-Werten. Bei Verwendung von Text vergleichen basieren die Zeichen Vergleiche und die Reihenfolge auf dem aktuellen Gebiets Schema, das auf dem .NET Framework verwendet wird.

In einigen Sprachen stellen Sonderzeichen im Alphabet zwei separate Zeichen und umgekehrt dar. Beispielsweise verwenden mehrere Sprachen das Zeichen `æ`, um die Zeichen `a` und `e` darzustellen, wenn Sie einander vorkommen, während die Zeichen `^` und `O` zur Darstellung des Zeichens `Ô` verwendet werden können. Bei der Verwendung von Text vergleichen erkennt der `Like`-Operator solche kulturellen äquivalente. In diesem Fall entspricht das Vorkommen des einzelnen Sonderzeichens in pattern oder String der entsprechenden zweistelligen Sequenz in der anderen Zeichenfolge. Ebenso entspricht ein einzelnes Sonderzeichen in einem Muster, das in eckige Klammern eingeschlossen ist (selbst, in einer Liste oder in einem Bereich), mit der entsprechenden zwei Zeichenfolge in der Zeichenfolge und umgekehrt.

In einem `Like`-Ausdruck, bei dem beide Operanden `Nothing` oder ein Operand eine intrinsische Konvertierung in `String` und der andere Operand `Nothing` sind, wird `Nothing` so behandelt, als ob es sich um das leere Zeichenfolgenliteral`""` handelt.

__Vorgangstyp:__

|        | __Ur__ | __SB__ | __Am__ | __Sh__ | __VORLIEGENDEN__ | __In__ | __ANGETAN__ | __Lo__ | __NÜTZLICHEN__ | __Liga__ | __Si__ | __Do__ | __De__  | __Ch__  | __St__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| __Ur__ | St | St | St | St | St | St | St | St | St | St | St | St | St | St | St | Johannis | 
| __SB__ |    | St | St | St | St | St | St | St | St | St | St | St | St | St | St | Johannis | 
| __Am__ |    |    | St | St | St | St | St | St | St | St | St | St | St | St | St | Johannis | 
| __Sh__ |    |    |    | St | St | St | St | St | St | St | St | St | St | St | St | Johannis | 
| __VORLIEGENDEN__ |    |    |    |    | St | St | St | St | St | St | St | St | St | St | St | Johannis | 
| __In__ |    |    |    |    |    | St | St | St | St | St | St | St | St | St | St | Johannis | 
| __ANGETAN__ |    |    |    |    |    |    | St | St | St | St | St | St | St | St | St | Johannis | 
| __Lo__ |    |    |    |    |    |    |    | St | St | St | St | St | St | St | St | Johannis | 
| __NÜTZLICHEN__ |    |    |    |    |    |    |    |    | St | St | St | St | St | St | St | Johannis | 
| __Liga__ |    |    |    |    |    |    |    |    |    | St | St | St | St | St | St | Johannis | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | St | St | St | St | St | Johannis | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | St | St | St | St | Johannis | 
| __De__ |    |    |    |    |    |    |    |    |    |    |    |    | St | St | St | Johannis | 
| __Ch__ |    |    |    |    |    |    |    |    |    |    |    |    |    | St | St | Johannis | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    | St | Johannis | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | Johannis | 


## <a name="concatenation-operator"></a>Verkettungs Operator

```antlr
ConcatenationOperatorExpression
    : Expression '&' LineTerminator? Expression
    ;
```

Der *Verkettungs Operator* ist für alle systeminternen Typen definiert, einschließlich der auf NULL festleg baren Versionen der systeminternen Werttypen. Sie wird auch für die Verkettung zwischen den oben erwähnten Typen und `System.DBNull` definiert, die als `Nothing`-Zeichenfolge behandelt wird. Der Verkettungs Operator konvertiert alle seine Operanden in `String`; im Ausdruck werden alle Konvertierungen in `String` als erweiternd betrachtet, unabhängig davon, ob strikte Semantik verwendet wird. Ein `System.DBNull`-Wert wird in das Literal`Nothing` konvertiert, das als `String` typisiert ist. Ein Werttyp, der auf NULL festgelegt werden kann, dessen Wert `Nothing` ist, wird ebenfalls in das Literale `Nothing` als `String` eingegeben, anstatt einen Laufzeitfehler auszulösen.

Ein Verkettungs Vorgang führt zu einer Zeichenfolge, bei der es sich um die Verkettung der beiden Operanden von links nach rechts handelt. Der Wert `Nothing` wird so behandelt, als ob es sich um das leere Zeichenfolgenliteral`""` handelt.

__Vorgangstyp:__

|        | __Ur__ | __SB__ | __Am__ | __Sh__ | __VORLIEGENDEN__ | __In__ | __ANGETAN__ | __Lo__ | __NÜTZLICHEN__ | __Liga__ | __Si__ | __Do__ | __De__  | __Ch__  | __St__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| __Ur__ | St | St | St | St | St | St | St | St | St | St | St | St | St | St | St | Johannis | 
| __SB__ |    | St | St | St | St | St | St | St | St | St | St | St | St | St | St | Johannis | 
| __Am__ |    |    | St | St | St | St | St | St | St | St | St | St | St | St | St | Johannis | 
| __Sh__ |    |    |    | St | St | St | St | St | St | St | St | St | St | St | St | Johannis | 
| __VORLIEGENDEN__ |    |    |    |    | St | St | St | St | St | St | St | St | St | St | St | Johannis | 
| __In__ |    |    |    |    |    | St | St | St | St | St | St | St | St | St | St | Johannis | 
| __ANGETAN__ |    |    |    |    |    |    | St | St | St | St | St | St | St | St | St | Johannis | 
| __Lo__ |    |    |    |    |    |    |    | St | St | St | St | St | St | St | St | Johannis | 
| __NÜTZLICHEN__ |    |    |    |    |    |    |    |    | St | St | St | St | St | St | St | Johannis | 
| __Liga__ |    |    |    |    |    |    |    |    |    | St | St | St | St | St | St | Johannis | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | St | St | St | St | St | Johannis | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | St | St | St | St | Johannis | 
| __De__ |    |    |    |    |    |    |    |    |    |    |    |    | St | St | St | Johannis | 
| __Ch__ |    |    |    |    |    |    |    |    |    |    |    |    |    | St | St | Johannis | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    | St | Johannis | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | Johannis | 


## <a name="logical-operators"></a>Logische Operatoren

Die Operatoren "`And`", "`Not`", "`Or`" und "`Xor`" werden als logische Operatoren bezeichnet.

```antlr
LogicalOperatorExpression
    : 'Not' Expression
    | Expression 'And' LineTerminator? Expression
    | Expression 'Or' LineTerminator? Expression
    | Expression 'Xor' LineTerminator? Expression
    ;
```

Die logischen Operatoren werden wie folgt ausgewertet:

* Für den `Boolean`-Typ:

  * Ein logischer `And`-Vorgang wird für die beiden Operanden ausgeführt.

  * Ein logischer `Not`-Vorgang wird für den Operanden ausgeführt.

  * Ein logischer `Or`-Vorgang wird für die beiden Operanden ausgeführt.

  * Ein logischer exklusiv-`Or`-Vorgang wird für die beiden Operanden ausgeführt.

* Bei `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long` und allen enumerierten Typen wird der angegebene Vorgang für jedes Bit der binären Darstellung der beiden Operanden ausgeführt:

  * `And`: Das Ergebnisbit ist 1, wenn beide Bits 1 sind. Andernfalls ist das Ergebnisbit 0 (null).

  * `Not`: Das Ergebnisbit ist 1, wenn das Bit 0 (null) ist. Andernfalls ist das Ergebnisbit 1.

  * `Or`: Das Ergebnisbit ist 1, wenn eines der Bits 1 ist. Andernfalls ist das Ergebnisbit 0 (null).

  * `Xor`: Das Ergebnisbit ist 1, wenn eines der beiden Bits gleich 1 ist. Andernfalls ist das Ergebnisbit 0 (d. h. 1 `Xor` 0 = 1, 1 `Xor` 1 = 0).

* Wenn die logischen Operatoren `And` und `Or` für den Typ `Boolean?` angehoben werden, werden Sie auf eine dreiwertige boolesche Logik wie folgt erweitert:

  * `And` ergibt true, wenn beide Operanden true sind. false, wenn einer der Operanden false ist. `Nothing` andernfalls.

  * `Or` wird zu true ausgewertet, wenn einer der beiden Operanden true ist. false gibt an, dass beide Operanden false sind. `Nothing` andernfalls.

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

__Nebenbei.__ Im Idealfall werden die logischen Operatoren `And` und `Or` mithilfe von dreiwertigen Logik für jeden Typ, der in einem booleschen Ausdruck verwendet werden kann (d. h. einen Typ, der `IsTrue` und `IsFalse` implementiert), auf die gleiche Weise wie `AndAlso` und `OrElse` Kurzschluss über alle der Typ, der in einem booleschen Ausdruck verwendet werden kann. Leider wird die dreiwertige Erhöhung nur auf `Boolean?` angewendet, sodass benutzerdefinierte Typen, die dreiwertige Logik benötigen, dies manuell tun müssen, indem Sie `And`-und `Or`-Operatoren für Ihre Version definieren, die NULL-Werte zulässt.

Von diesen Vorgängen können keine über Flüsse durchlaufen werden. Die enumerierten Typoperatoren führen die bitweise Operation für den zugrunde liegenden Typ des Enumerationstyps aus, aber der Rückgabewert ist der enumerierte Typ.

__Nicht Vorgangstyp:__

| __Ur__ | __SB__ | __Am__ | __Sh__ | __VORLIEGENDEN__ | __In__ | __ANGETAN__ | __Lo__ | __NÜTZLICHEN__ | __Liga__ | __Si__ | __Do__ | __De__  | __Ch__  | __St__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| Ur | SB | um | Sh | US | In | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo | Johannis | 

__And-, or-, Xor-Vorgangstyp:__

|        | __Ur__ | __SB__ | __Am__ | __Sh__ | __VORLIEGENDEN__ | __In__ | __ANGETAN__ | __Lo__ | __NÜTZLICHEN__ | __Liga__ | __Si__ | __Do__ | __De__  | __Ch__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __Ur__ | Ur | SB | Sh | Sh | In | In | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Ur  | Johannis  | 
| __SB__ |    | SB | Sh | Sh | In | In | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Johannis  | 
| __Am__ |    |    | um | Sh | US | In | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Johannis  | 
| __Sh__ |    |    |    | Sh | In | In | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Johannis  | 
| __VORLIEGENDEN__ |    |    |    |    | US | In | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Johannis  | 
| __In__ |    |    |    |    |    | In | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Johannis  | 
| __ANGETAN__ |    |    |    |    |    |    | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Johannis  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Johannis  | 
| __NÜTZLICHEN__ |    |    |    |    |    |    |    |    | UL | Lo | Lo | Lo | Err | Err | Lo  | Johannis  | 
| __Liga__ |    |    |    |    |    |    |    |    |    | Lo | Lo | Lo | Err | Err | Lo  | Johannis  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Lo | Lo | Err | Err | Lo  | Johannis  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Lo | Err | Err | Lo  | Johannis  | 
| __De__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __Ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Lo  | Johannis  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Johannis  | 


### <a name="short-circuiting-logical-operators"></a>Kurzschluss logische Operatoren

Die Operatoren "`AndAlso`" und "`OrElse`" sind die Kurzschluss Versionen der logischen Operatoren "`And`" und "`Or`".

```antlr
ShortCircuitLogicalOperatorExpression
    : Expression 'AndAlso' LineTerminator? Expression
    | Expression 'OrElse' LineTerminator? Expression
    ;
```

Aufgrund des kurzen Schluss Verhaltens wird der zweite Operand nicht zur Laufzeit ausgewertet, wenn das Operator Ergebnis nach der Auswertung des ersten Operanden bekannt ist.

Die Kurzschluss logischen Operatoren werden wie folgt ausgewertet:

* Wenn der erste Operand in einem `AndAlso`-Vorgang zu `False` ausgewertet wird oder true von seinem `IsFalse`-Operator zurückgibt, gibt der Ausdruck seinen ersten Operanden zurück. Andernfalls wird der zweite Operand ausgewertet, und ein logischer `And`-Vorgang wird für die beiden Ergebnisse ausgeführt.

* Wenn der erste Operand in einem `OrElse`-Vorgang zu `True` ausgewertet wird oder true von seinem `IsTrue`-Operator zurückgibt, gibt der Ausdruck seinen ersten Operanden zurück. Andernfalls wird der zweite Operand ausgewertet, und ein logischer `Or`-Vorgang wird für die beiden Ergebnisse ausgeführt.

Die Operatoren "`AndAlso`" und "`OrElse`" sind für den Typ "`Boolean`" oder für jeden Typ "`T`" definiert, der die folgenden Operatoren über lädt:

```vb
Public Shared Operator IsTrue(op As T) As Boolean
Public Shared Operator IsFalse(op As T) As Boolean
```

Außerdem wird der entsprechende `And`-oder `Or`-Operator überladen:

```vb
Public Shared Operator And(op1 As T, op2 As T) As T
Public Shared Operator Or(op1 As T, op2 As T) As T
```

Beim Auswerten der Operatoren "`AndAlso`" oder "`OrElse`" wird der erste Operand nur einmal ausgewertet, und der zweite Operand wird entweder nicht genau einmal ausgewertet oder ausgewertet. Beachten Sie z. B. folgenden Code:

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

Es gibt das folgende Ergebnis aus:

```console
And: False True
Or: True False
AndAlso: False
OrElse: True
```

Wenn der erste Operand ein NULL-Wert `Boolean?` ist, wird der zweite Operand ausgewertet, wobei das Ergebnis immer ein NULL-`Boolean?` ist, wenn der erste Operand ein NULL--2-Operator ist. @no__t @no__t

__Vorgangstyp:__

|        | __Ur__ | __SB__ | __Am__ | __Sh__ | __VORLIEGENDEN__ | __In__ | __ANGETAN__ | __Lo__ | __NÜTZLICHEN__ | __Liga__ | __Si__ | __Do__ | __De__  | __Ch__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __Ur__ | Ur | Ur | Ur | Ur | Ur | Ur | Ur | Ur | Ur | Ur | Ur | Ur | Err | Err | Ur  | Johannis  | 
| __SB__ |    | Ur | Ur | Ur | Ur | Ur | Ur | Ur | Ur | Ur | Ur | Ur | Err | Err | Ur  | Johannis  | 
| __Am__ |    |    | Ur | Ur | Ur | Ur | Ur | Ur | Ur | Ur | Ur | Ur | Err | Err | Ur  | Johannis  | 
| __Sh__ |    |    |    | Ur | Ur | Ur | Ur | Ur | Ur | Ur | Ur | Ur | Err | Err | Ur  | Johannis  | 
| __VORLIEGENDEN__ |    |    |    |    | Ur | Ur | Ur | Ur | Ur | Ur | Ur | Ur | Err | Err | Ur  | Johannis  | 
| __In__ |    |    |    |    |    | Ur | Ur | Ur | Ur | Ur | Ur | Ur | Err | Err | Ur  | Johannis  | 
| __ANGETAN__ |    |    |    |    |    |    | Ur | Ur | Ur | Ur | Ur | Ur | Err | Err | Ur  | Johannis  | 
| __Lo__ |    |    |    |    |    |    |    | Ur | Ur | Ur | Ur | Ur | Err | Err | Ur  | Johannis  | 
| __NÜTZLICHEN__ |    |    |    |    |    |    |    |    | Ur | Ur | Ur | Ur | Err | Err | Ur  | Johannis  | 
| __Liga__ |    |    |    |    |    |    |    |    |    | Ur | Ur | Ur | Err | Err | Ur  | Johannis  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Ur | Ur | Err | Err | Ur  | Johannis  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Ur | Err | Err | Ur  | Johannis  | 
| __De__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __Ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Ur  | Johannis  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Johannis  | 


## <a name="shift-operators"></a>Schiebeoperatoren

Die binären Operatoren `<<` und `>>` führen Bitverschiebungs Vorgänge aus.

```antlr
ShiftOperatorExpression
    : Expression '<' '<' LineTerminator? Expression
    | Expression '>' '>' LineTerminator? Expression
    ;
```

Die Operatoren sind für die Typen `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` und `Long` definiert. Im Gegensatz zu den anderen binären Operatoren wird der Ergebnistyp einer Verschiebungs Operation so bestimmt, als wäre der Operator ein unärer Operator mit nur dem linken Operanden. Der Typ des rechten Operanden muss implizit in `Integer` konvertiert werden und wird nicht verwendet, um den Ergebnistyp des Vorgangs zu bestimmen.

Der `<<`-Operator bewirkt, dass die Bits im ersten Operanden um die Anzahl der durch die UMSCHALT Menge angegebenen Stellen nach links verschoben werden. Die höherwertigen Bits außerhalb des Bereichs des Ergebnis Typs werden verworfen, und die in niedriger Reihenfolge frei gewordenen Bitpositionen werden mit Nullen aufgefüllt.

Der `>>`-Operator bewirkt, dass die Bits im ersten Operanden nach rechts um die durch die UMSCHALT Menge angegebene Anzahl von Stellen verschoben werden. Die Bits mit niedriger Ordnung werden verworfen, und die in der Reihenfolge frei gewordenen Bitpositionen werden auf 0 (null) festgelegt, wenn der linke Operand positiv ist. Wenn der linke Operand vom Typ "`Byte`", "`UShort`", "`UInteger`" oder "`ULong`" ist, werden die leeren höherwertigen Bits mit Nullen gefüllt.

Die Shift-Operatoren verschieben die Bits der zugrunde liegenden Darstellung des ersten Operanden um die Menge des zweiten Operanden. Wenn der Wert des zweiten Operanden größer als die Anzahl der Bits im ersten Operanden ist oder negativ ist, wird der UMSCHALT Betrag als `RightOperand And SizeMask` berechnet, wobei `SizeMask` ist:

| __LeftOperand-Typ__  | __SizeMask__ | 
|-----------------------|--------------|
| `Byte`, `SByte`       | 7 (`&H7`)    | 
| `UShort`, `Short`     | 15 (`&HF`)   | 
| `UInteger`, `Integer` | 31 (`&H1F`)  | 
| `ULong`, `Long`       | 63 (`&H3F`)  | 

Wenn die UMSCHALT Menge 0 (null) ist, ist das Ergebnis des Vorgangs mit dem Wert des ersten Operanden identisch. Von diesen Vorgängen können keine über Flüsse durchlaufen werden.

__Vorgangstyp:__


| __Ur__ | __SB__ | __Am__ | __Sh__ | __VORLIEGENDEN__ | __In__ | __ANGETAN__ | __Lo__ | __NÜTZLICHEN__ | __Liga__ | __Si__ | __Do__ | __De__  | __Ch__  | __St__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| Sh | SB | um | Sh | US | In | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo | Johannis | 


## <a name="boolean-expressions"></a>Boolesche Ausdrücke

Ein boolescher Ausdruck ist ein Ausdruck, der getestet werden kann, um festzustellen, ob er true ist, oder ob er false ist.

```antlr
BooleanExpression
    : Expression
    ;
```

Ein Typ `T` kann in einem booleschen Ausdruck verwendet werden, wenn in der Reihenfolge der bevorzugte Werte:

* `T` ist `Boolean` oder `Boolean?`

* `T` hat eine erweiternde Konvertierung in `Boolean`

* `T` hat eine erweiternde Konvertierung in `Boolean?`

* `T` definiert zwei Pseudo Operatoren, `IsTrue` und `IsFalse`.

* `T` hat eine einschränkende Konvertierung in `Boolean?`, bei der keine Konvertierung von `Boolean` in `Boolean?` beteiligt ist.

* `T` hat eine einschränkende Konvertierung in `Boolean`.

__Nebenbei.__ Beachten Sie, dass wenn `Option Strict` deaktiviert ist, ein Ausdruck, der eine einschränkende Konvertierung in `Boolean` aufweist, ohne Kompilierzeitfehler akzeptiert wird, die Sprache jedoch immer noch einen `IsTrue`-Operator bevorzugt, wenn dieser vorhanden ist. Der Grund hierfür ist, dass `Option Strict` nur ändert, was von der Sprache nicht akzeptiert wird, und die tatsächliche Bedeutung eines Ausdrucks nie ändert. Daher muss `IsTrue` unabhängig von `Option Strict` immer über eine einschränkende Konvertierung bevorzugt werden.

Die folgende Klasse definiert z. b. keine erweiternde Konvertierung in `Boolean`. Folglich verursacht die Verwendung in der `If`-Anweisung einen aufzurufenden Operator `IsTrue`.

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

Wenn ein boolescher Ausdruck in `Boolean` oder `Boolean?` eingegeben oder konvertiert wird, ist es true, wenn der Wert `True` ist, und andernfalls false.

Andernfalls ruft ein boolescher Ausdruck den `IsTrue`-Operator auf und gibt `True` zurück, wenn der Operator `True`; zurückgegeben hat. Andernfalls ist Sie false (der `IsFalse`-Operator wird jedoch nie aufgerufen).

Im folgenden Beispiel verfügt `Integer` über eine einschränkende Konvertierung in `Boolean`, sodass eine NULL-`Integer?` eine einschränkende Konvertierung in `Boolean?` (ergibt einen NULL-Wert `Boolean`) und `Boolean` (wodurch eine Ausnahme ausgelöst wird) aufweist. Die einschränkende Konvertierung in `Boolean?` wird bevorzugt. Daher ist der Wert von "`i`" als boolescher Ausdruck `False`.

```vb
Dim i As Integer? = Nothing
If i Then Console.WriteLine()
```


## <a name="lambda-expressions"></a>Lambda-Ausdrücke

Ein *Lambda-Ausdruck* definiert eine anonyme Methode, die als *Lambda-Methode*bezeichnet wird. Mit Lambda-Methoden können "Inline"-Methoden einfach an andere Methoden übergeben werden, die Delegattypen akzeptieren.

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

Das Beispiel:

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

druckt Folgendes:

```console
2 4 6 8
```

Ein Lambda-Ausdruck beginnt mit den optionalen modifiziermetern `Async` oder `Iterator`, gefolgt vom-Schlüsselwort `Function` oder `Sub` und einer Parameterliste. Parameter in einem Lambda-Ausdruck können nicht als `Optional` oder `ParamArray` deklariert werden und können keine Attribute aufweisen. Anders als bei regulären Methoden wird durch das Weglassen eines Parameter Typs für eine Lambda-Methode nicht automatisch `Object` abgeleitet. Stattdessen werden bei der Neuklassifizierung einer Lambda-Methode die ausgelassenen Parametertypen und `ByRef`-Modifizierer aus dem Zieltyp abgeleitet. Im vorherigen Beispiel hätte der Lambda-Ausdruck möglicherweise als `Function(x) x * 2` geschrieben worden, und der Typ von `x` muss `Integer` abgeleitet werden, wenn die Lambda-Methode verwendet wurde, um eine Instanz des `IntFunc`-Delegattyps zu erstellen. Anders als bei der lokalen Variablen Ableitung tritt ein Kompilierzeitfehler auf, wenn ein Lambda-Methoden Parameter einen Typ auslässt, aber über ein Array oder einen änderungsmodifizierer mit Nullwert verfügt.

Ein __regulärer Lambda-Ausdruck__ ist ein Ausdruck, der weder `Async` noch `Iterator`-Modifizierer ist.

Ein __iteratorlambda-Ausdruck__ ist ein Ausdruck mit dem `Iterator`-Modifizierer und kein `Async`-Modifizierer. Es muss eine Funktion sein. Wenn ein Wert neu klassifiziert wird, kann er nur zu einem Wert des Delegattyps neu klassifiziert werden, dessen Rückgabetyp `IEnumerator` oder `IEnumerable` oder `IEnumerator(Of T)` oder `IEnumerable(Of T)` für einige `T` ist und der keine ByRef-Parameter aufweist.

Ein asynchroner __Lambda-Ausdruck__ ist ein Ausdruck mit dem `Async`-Modifizierer und kein `Iterator`-Modifizierer. Ein Async-Sub-Lambda kann nur in einen Wert des subdelegattyps ohne ByRef-Parameter neu klassifiziert werden. Ein asynchroner funktionslambda kann nur in einen Wert des funktionsdelegattyps neu klassifiziert werden, dessen Rückgabetyp `Task` oder `Task(Of T)` für einige `T` ist und der keine ByRef-Parameter aufweist.

Lambda Ausdrücke können entweder einzeilige oder mehrzeilige Zeilen sein. Einzeilige `Function`-Lambda Ausdrücke enthalten einen einzelnen Ausdruck, der den von der Lambda-Methode zurückgegebenen Wert darstellt. Einzeilige `Sub`-Lambda Ausdrücke enthalten eine einzelne Anweisung ohne die schließende `StatementTerminator`. Zum Beispiel:

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

Einzeilige Lambda-Konstrukte binden weniger eng als alle anderen Ausdrücke und Anweisungen. So entspricht beispielsweise "`Function() x + 5`" "`Function() (x+5)"` anstelle von" `(Function() x) + 5` ". Um Mehrdeutigkeit zu vermeiden, darf ein einzeilige `Sub`-Lambda-Ausdruck keine Dim-Anweisung oder eine Bezeichnungs Deklarations Anweisung enthalten. Auch wenn es nicht in Klammern eingeschlossen ist, darf ein Einzeiger `Sub`-Lambda-Ausdruck nicht unmittelbar auf einen Doppelpunkt (":"), einen Member Access-Operator ".", einen Wörterbuch-Member-Zugriffs Operator "!" oder eine öffnende Klammer "(" folgen. Sie darf keine Block-Anweisung (`With`, `SyncLock, If...EndIf`, `While`, `For`, `Do`, `Using`) und `OnError` oder `Resume` enthalten.

__Nebenbei.__ Im Lambda-Ausdruck `Function(i) x=i` wird der Text als *Ausdruck* interpretiert (der testet, ob `x` und `i` gleich sind). Im Lambda-Ausdruck `Sub(i) x=i` wird der Text jedoch als-Anweisung interpretiert (die `i` zu `x` zuweist).

Ein mehrzeilige Lambda-Ausdruck enthält einen Anweisungsblock und muss mit einer entsprechenden `End`-Anweisung (d. h. `End Function` oder `End Sub`) enden. Wie bei regulären Methoden müssen sich die `Function`-oder `Sub`-Anweisung einer mehrzeiligen Lambda-Methode und die `End`-Anweisungen in ihren eigenen Zeilen befinden. Zum Beispiel:

```vb
' Error: Function statement must be on its own line!
Dim x = Sub(x As Integer) : Console.WriteLine(x) : End Sub

' OK
Dim y = Sub(x As Integer)
               Console.WriteLine(x)
          End Sub
```

Mehrzeilige `Function`-Lambda Ausdrücke können einen Rückgabetyp deklarieren, aber keine Attribute darauf ablegen. Wenn ein mehrzeilige `Function`-Lambda Ausdruck keinen Rückgabetyp deklariert, aber der Rückgabetyp aus dem Kontext abgeleitet werden kann, in dem der Lambda-Ausdruck verwendet wird, wird dieser Rückgabetyp verwendet. Andernfalls wird der Rückgabetyp der Funktion wie folgt berechnet:

* In einem regulären Lambda-Ausdruck ist der Rückgabetyp der vorherrschende Typ der Ausdrücke in allen `Return`-Anweisungen im Anweisungsblock.

* In einem asynchronen Lambda-Ausdruck ist der Rückgabetyp `Task(Of T)`, wobei `T` der vorherrschende Typ der Ausdrücke in allen `Return`-Anweisungen im Anweisungsblock ist.

* In einem iteratorlambda-Ausdruck ist der Rückgabetyp `IEnumerable(Of T)`, wobei `T` der vorherrschende Typ der Ausdrücke in allen `Yield`-Anweisungen im Anweisungsblock ist.

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

In allen Fällen, wenn keine `Return`-Anweisungen (bzw. `Yield`) vorhanden sind, oder wenn es keinen vorherrschenden Typ gibt und eine strikte Semantik verwendet wird, tritt ein Kompilierzeitfehler auf. Andernfalls ist der vorherrschende Typ implizit `Object`.

Beachten Sie, dass der Rückgabetyp von allen `Return`-Anweisungen berechnet wird, auch wenn Sie nicht erreichbar sind. Zum Beispiel:

```vb
' Return type is Double
Dim x = Function()
              Return 10
               Return 10.50
          End Function
```

Es gibt keine implizite Rückgabe Variable, da kein Name für die Variable vorhanden ist.

Die-Anweisungsblöcke in mehrzeiligen Lambda-Ausdrücken weisen die folgenden Einschränkungen auf:

* die Anweisungen `On Error` und `Resume` sind nicht zulässig, obwohl `Try`-Anweisungen zulässig sind.

* Statische lokale Variablen können nicht in mehrzeiligen Lambda Ausdrücken deklariert werden.

* Es ist nicht möglich, in einen oder aus dem Anweisungsblock eines mehrzeiligen Lambda-Ausdrucks zu verzweigen, obwohl die normalen Verzweigungs Regeln darin zutreffen. Zum Beispiel:

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

Ein Lambda-Ausdruck entspricht ungefähr einer anonymen Methode, die für den enthaltenden Typ deklariert wurde. Das ursprüngliche Beispiel entspricht ungefähr dem folgenden:

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


### <a name="closures"></a>Accoun

Lambda-Ausdrücke haben Zugriff auf alle Variablen im Gültigkeitsbereich, einschließlich lokaler Variablen oder Parameter, die in der enthaltenden Methode und in Lambda Ausdrücken definiert sind. Wenn ein Lambda-Ausdruck auf eine lokale Variable oder einen lokalen Parameter verweist, erfasst der Lambda-Ausdruck die Variable, auf die in einen Abschluss verwiesen wird. Ein Closure ist ein Objekt, das sich nicht auf dem Stapel, sondern auf dem Heap befindet. Wenn eine Variable aufgezeichnet wird, werden alle Verweise auf die Variable an den Abschluss umgeleitet. Dies ermöglicht es Lambda-Ausdrücken, weiterhin auf lokale Variablen und Parameter zu verweisen, auch nachdem die enthaltende Methode fertig ist. Zum Beispiel:

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

ist ungefähr Äquivalent zu:

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

Bei einem Abschluss wird eine neue Kopie einer lokalen Variablen aufgezeichnet, wenn Sie in den Block eintritt, in dem die lokale Variable deklariert ist, aber die neue Kopie wird mit dem Wert der vorherigen Kopie initialisiert, sofern vorhanden. Zum Beispiel:

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

druckt

```console
1 2 3 4 5 6 7 8 9 10
```

Statt

```console
9 9 9 9 9 9 9 9 9 9
```

Da bei der Eingabe eines-Blocks die-Abschlüsse initialisiert werden müssen, ist es nicht zulässig, 0 (null) in einen-Block mit einem Closure von außerhalb dieses Blocks zu @no__t, obwohl es zulässig ist,-1 in einen Block mit einer Schließung zu @no__t. Zum Beispiel:

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

Da Sie nicht in einem Closure aufgezeichnet werden können, kann Folgendes nicht in einem Lambda-Ausdruck vorkommen:

* Verweis Parameter.

* Instanzausdrücke (`Me`, `MyClass`, `MyBase`), wenn der Typ von `Me` keine Klasse ist.

Die Member eines anonymen typerstellungs Ausdrucks, wenn der Lambda-Ausdruck Teil des Ausdrucks ist. Zum Beispiel:

```vb
' Error: Lambda cannot refer to anonymous type field
Dim x = New With { .a = 12, .b = Function() .a }
```

`ReadOnly`-Instanzvariablen in Instanzkonstruktoren oder `ReadOnly` freigegebene Variablen in freigegebenen Konstruktoren, bei denen die Variablen in einem nicht-Wert-Kontext verwendet werden. Zum Beispiel:

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

Ein *Abfrage Ausdruck* ist ein Ausdruck, *der eine Reihe* von *Abfrage Operatoren* auf die Elemente einer abfragbaren Auflistung anwendet. Der folgende Ausdruck nimmt z. b. eine Auflistung von `Customer`-Objekten an und gibt die Namen aller Kunden im Bundesstaat Washington zurück:

```vb
Dim names = _
    From cust In Customers _
    Where cust.State = "WA" _
    Select cust.Name
```

Ein Abfrage Ausdruck muss mit einem `From`-oder einem `Aggregate`-Operator beginnen und mit einem beliebigen Abfrage Operator enden. Das Ergebnis eines Abfrage Ausdrucks wird als Wert klassifiziert. der Ergebnistyp des Ausdrucks hängt vom Ergebnistyp des letzten Abfrage Operators im Ausdruck ab.

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

### <a name="range-variables"></a>Bereichs Variablen

Einige Abfrage Operatoren führen eine besondere Art von Variablen ein, die als *Bereichs Variable*bezeichnet wird. Bereichs Variablen sind keine echten Variablen. Stattdessen stellen Sie die einzelnen Werte während der Auswertung der Abfrage für die Eingabe Auflistungen dar.

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

Bereichs Variablen werden vom Introducing Query-Operator bis zum Ende eines Abfrage Ausdrucks oder bis zu einem Abfrage Operator, z. b. `Select`, der Sie verbirgt, festgelegt. Beispielsweise in der folgenden Abfrage:

```vb
Dim waCusts = _
    From cust As Customer In Customers _
    Where cust.State = "WA"
```

der `From`-Abfrage Operator führt eine Bereichs Variable `cust` ein, die als `Customer` typisiert ist, die jeden Kunden in der `Customers`-Auflistung darstellt. Der folgende `Where`-Abfrage Operator verweist dann auf die Bereichs Variable `cust` im Filter Ausdruck, um zu bestimmen, ob ein einzelner Kunde aus der resultierenden Auflistung gefiltert werden soll.

Es gibt zwei Typen von Bereichs Variablen: Auflistungs *Bereichs Variablen* und *Ausdrucks Bereichs Variablen*. Sammlungs Bereichs Variablen übernehmen ihre Werte aus den Elementen der Auflistungen, die abgefragt werden. Der Auflistungs Ausdruck in einer Variablen Deklaration für den Sammlungs Bereich muss als Wert klassifiziert werden, dessen Typ abgefragt werden kann. Wenn der Typ einer Variablen für den Sammlungs Bereich weggelassen wird, wird er als Elementtyp des Auflistungs Ausdrucks abgeleitet, oder `Object`, wenn der Auflistungs Ausdruck keinen Elementtyp hat (d. h. nur eine `Cast`-Methode definiert). Wenn der Auflistungs Ausdruck nicht abgefragt werden kann (d. h. der Elementtyp der Auflistung kann nicht abgeleitet werden), wird ein Kompilierzeitfehler ausgegeben.

Eine Ausdrucks Bereichs Variable ist eine Bereichs Variable, deren Wert durch einen Ausdruck und nicht durch eine Auflistung berechnet wird. Im folgenden Beispiel führt der `Select`-Abfrage Operator eine Ausdrucks Bereichs Variable mit dem Namen `cityState` ein, die aus zwei Feldern berechnet wurde:

```vb
Dim cityStates = _
    From cust As Customer In Customers _
    Select cityState = cust.City & "," & cust.State _
    Where cityState.Length() < 10
```

Eine Ausdrucks Bereichs Variable ist nicht erforderlich, um auf eine andere Bereichs Variable zu verweisen, obwohl eine solche Variable einen zweifelhaften Wert aufweisen kann. Der Ausdruck, der einer Ausdrucks Bereichs Variablen zugewiesen ist, muss als Wert klassifiziert werden und muss implizit in den Typ der Bereichs Variablen konvertiert werden können, falls angegeben.

Nur in einem Let-Operator darf der Typ einer Ausdrucks Bereichs Variablen angegeben werden. In anderen Operatoren oder, wenn der Typ nicht angegeben ist, wird der Typ der Bereichs Variablen mithilfe des lokalen Variablen Typs abgeleitet.

Eine Bereichs Variable muss den Regeln zum Deklarieren von lokalen Variablen in Bezug auf shadowingfolgen. Daher kann eine Bereichs Variable den Namen einer lokalen Variablen oder eines Parameters in der einschließenden Methode oder einer anderen Bereichs Variablen nicht ausblenden (es sei denn, der Abfrage Operator blendet alle aktuellen Bereichs Variablen im Gültigkeitsbereich explizit aus).


### <a name="queryable-types"></a>Abfragbare Typen

Abfrage Ausdrücke werden implementiert, indem der Ausdruck in Aufrufe bekannter Methoden für einen Auflistungstyp übersetzt wird. Diese klar definierten Methoden definieren den Elementtyp der abfragbaren Auflistung sowie die Ergebnistypen von Abfrage Operatoren, die für die Auflistung ausgeführt werden. Jeder Abfrage Operator gibt die Methode oder Methoden an, in die der Abfrage Operator allgemein übersetzt wird, obwohl die jeweilige Übersetzung von der Implementierung abhängig ist. Die-Methoden werden in der Spezifikation mit einem allgemeinen Format angegeben, das wie folgt aussieht:

```vb
Function Select(selector As Func(Of T, R)) As CR
```

Folgendes gilt für die-Methoden:

* Die Methode muss eine Instanz oder ein Erweiterungs Mitglied des Auflistungs Typs sein, und es muss darauf zugegriffen werden können.

* Die Methode kann generisch sein, vorausgesetzt, es ist möglich, alle Typargumente abzuleiten.

* Die Methode kann überladen werden. in diesem Fall wird die Überladungs Auflösung verwendet, um die exakt zu verwendende Methode zu bestimmen.

* Anstelle des Delegaten `Func`-Typs kann ein anderer Delegattyp verwendet werden, vorausgesetzt, dass Sie die gleiche Signatur, einschließlich Rückgabetyp, als übereinstimmenden `Func`-Typ aufweist.

* Der Typ `System.Linq.Expressions.Expression(Of D)` kann anstelle des Delegaten `Func`-Typs verwendet werden, vorausgesetzt, dass es sich bei `D` um einen Delegattyp mit derselben Signatur, einschließlich Rückgabetyp, als übereinstimmenden `Func`-Typ handelt.

* Der Typ `T` stellt den Elementtyp der Eingabe Auflistung dar. Alle von einem Auflistungstyp definierten Methoden müssen den gleichen Eingabe Elementtyp aufweisen, damit der Auflistungstyp abgefragt werden können.

* Der Typ `S` stellt den Elementtyp der zweiten Eingabe Auflistung im Fall von Abfrage Operatoren dar, die Joins ausführen.

* Der Typ `K` stellt einen Schlüsseltyp im Fall von Abfrage Operatoren dar, die über einen Satz von Bereichs Variablen verfügen, die als Schlüssel fungieren.

* Der Typ `N` stellt einen Typ dar, der als numerischer Typ verwendet wird (es kann sich jedoch immer noch um einen benutzerdefinierten Typ und nicht um einen systeminternen numerischen Typ handeln).

* Der Typ `B` stellt einen Typ dar, der in einem booleschen Ausdruck verwendet werden kann.

* Der Typ `R` stellt den Elementtyp der Ergebnis Auflistung dar, wenn der Abfrage Operator eine Ergebnis Auflistung erzeugt. `R` hängt von der Anzahl der Bereichs Variablen im Gültigkeitsbereich am Ende des Abfrage Operators ab. Wenn sich eine einzelne Bereichs Variable im Gültigkeitsbereich befindet, ist `R` der Typ der Bereichs Variablen. Im Beispiel

  ```vb
  Dim custNames = From c In Customers
                  Select c.Name
  ```

  Das Ergebnis der Abfrage ist ein Sammlungstyp mit dem Elementtyp `String`. Wenn sich mehrere Bereichs Variablen im Gültigkeitsbereich befinden, ist `R` ein anonymer Typ, der alle Bereichs Variablen im Gültigkeitsbereich als `Key`-Felder enthält. Im folgenden Beispiel

  ```vb
  Dim custNames = From c In Customers, o In c.Orders 
                  Select Name = c.Name, ProductName = o.ProductName
  ```

  Das Ergebnis der Abfrage ist ein Sammlungstyp mit einem Elementtyp eines anonymen Typs mit einer schreibgeschützten Eigenschaft mit dem Namen "`Name`" vom Typ "`String`" und einer schreibgeschützten Eigenschaft mit dem Namen "`ProductName`" vom Typ "`String`".

  In einem Abfrage Ausdruck sind anonyme Typen, die generiert werden, um Bereichs Variablen zu enthalten, *transparent*, was bedeutet, dass Bereichs Variablen immer ohne Qualifizierung verfügbar sind. Im vorherigen Beispiel konnte beispielsweise auf die Bereichs Variablen `c` und `o` ohne Qualifizierung im `Select`-Abfrage Operator zugegriffen werden, obwohl der Elementtyp der Eingabe Auflistung ein anonymer Typ war.

* Der Typ `CX` stellt einen Sammlungstyp dar, nicht notwendigerweise den Eingabe Sammlungstyp, dessen Elementtyp ein Typ ist `X`.

Ein abfragbarer Auflistungstyp muss eine der folgenden Bedingungen erfüllen:

* Es muss eine konforme `Select`-Methode definieren.

* Es muss eine der folgenden Methoden aufweisen:

  ```vb
  Function AsEnumerable() As CT
  Function AsQueryable() As CT
  ```

  , die aufgerufen werden kann, um eine abfragbare Auflistung zu erhalten. Wenn beide Methoden bereitgestellt werden, wird `AsQueryable` als `AsEnumerable` bevorzugt.

* Er muss über eine-Methode verfügen.

  ```vb
  Function Cast(Of T)() As CT
  ```

  , der mit dem Typ der Bereichs Variablen aufgerufen werden kann, um eine abfragbare Auflistung zu erhalten.

Da die Bestimmung des Elementtyps einer Auflistung unabhängig von einem tatsächlichen Methodenaufruf auftritt, kann die Anwendbarkeit spezifischer Methoden nicht bestimmt werden. Wenn Sie den Elementtyp einer Auflistung ermitteln, wenn Instanzmethoden vorhanden sind, die mit bekannten Methoden identisch sind, werden daher alle Erweiterungs Methoden ignoriert, die bekannten Methoden entsprechen.

Die Abfrage Operator Übersetzung erfolgt in der Reihenfolge, in der die Abfrage Operatoren im Ausdruck auftreten. Es ist nicht erforderlich, dass ein Auflistungs Objekt alle Methoden implementiert, die von allen Abfrage Operatoren benötigt werden, obwohl jedes Auflistungs Objekt zumindest den `Select`-Abfrage Operator unterstützen muss. Wenn eine erforderliche Methode nicht vorhanden ist, tritt ein Kompilierzeitfehler auf. Wenn bekannte Methodennamen gebunden werden, werden nicht--Methoden für den Zweck der Mehrfachvererbung in Schnittstellen und der Bindungsmethoden Bindung ignoriert, obwohl die Semantik für die Schatten-Semantik weiterhin gilt. Zum Beispiel:

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

### <a name="default-query-indexer"></a>Standardinfrageindexer

Jeder abfragbare Auflistungstyp, dessen Elementtyp `T` ist und der nicht bereits über eine Default-Eigenschaft verfügt, wird als Standard Eigenschaft der folgenden allgemeinen Form angesehen:

```vb
Public ReadOnly Default Property Item(index As Integer) As T
    Get
        Return Me.ElementAtOrDefault(index)
    End Get
End Property
```

Auf die Default-Eigenschaft kann nur mit der standardmäßigen Eigenschaften Zugriffs Syntax verwiesen werden. auf die Default-Eigenschaft kann nicht anhand des Namens verwiesen werden. Zum Beispiel:

```vb
Dim customers As IEnumerable(Of Customer) = ...
Dim customerThree = customers(2)

' Error, no such property
Dim customerFour = customers.Item(4)
```

Wenn der Auflistungstyp nicht über einen `ElementAtOrDefault`-Member verfügt, tritt ein Kompilierzeitfehler auf.

### <a name="from-query-operator"></a>From Query-Operator

Der `From`-Abfrage Operator führt eine Sammlungs Bereichs Variable ein, die die einzelnen Elemente einer Auflistung darstellt, die abgefragt werden soll.

```antlr
FromQueryOperator
    : LineTerminator? 'From' LineTerminator? CollectionRangeVariableDeclarationList
    ;
```

Der Abfrage Ausdruck lautet z. b.:

```vb
From c As Customer In Customers ...
```

kann als äquivalent zu betrachtet werden.

```vb
For Each c As Customer In Customers
        ...
Next c
```

Wenn ein `From`-Abfrage Operator mehrere Sammlungs Bereichs Variablen deklariert oder nicht der erste `From`-Abfrage Operator im Abfrage Ausdruck ist, wird jede neue Sammlungs Bereichs *Variable mit dem* vorhandenen Satz von Bereichs Variablen miteinander verknüpft. Das Ergebnis ist, dass die Abfrage über das Kreuz Produkt aller Elemente in den verbundenen Auflistungen ausgewertet wird. Der Ausdruck lautet z. b.:

```vb
From c In Customers _
From e In Employees _
...
```

kann als äquivalent zu betrachtet werden:

```vb
For Each c In Customers
    For Each e In Employees
            ...
    Next e
Next c
```

und ist genau Äquivalent zu:

```vb
From c In Customers, e In Employees ...
```

Die in vorherigen Abfrage Operatoren eingeführten Bereichs Variablen können in einem späteren `From`-Abfrage Operator verwendet werden. Im folgenden Abfrage Ausdruck bezieht sich z. b. der zweite `From`-Abfrage Operator auf den Wert der ersten Bereichs Variablen:

```vb
From c As Customer In Customers _
From o As Order In c.Orders _
Select c.Name, o
```

Mehrere Bereichs Variablen in einem `From`-Abfrage Operator oder mehrere `From`-Abfrage Operatoren werden nur unterstützt, wenn der Auflistungstyp eine oder beide der folgenden Methoden enthält:

```vb
Function SelectMany(selector As Func(Of T, CR)) As CR
Function SelectMany(selector As Func(Of T, CS), _
                          resultsSelector As Func(Of T, S, R)) As CR
```

Der Code

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs, y In ys ...
```

wird im Allgemeinen in übersetzt.

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.SelectMany( _
        Function(x As Integer) ys, _
        Function(x As Integer, y As Integer) New With {x, y})...
```

__Nebenbei.__ `From` ist kein reserviertes Wort.


### <a name="join-query-operator"></a>Join-Abfrage Operator

Der `Join`-Abfrage Operator verknüpft vorhandene Bereichs Variablen mit einer neuen Sammlungs Bereichs Variablen und erzeugt eine einzelne Auflistung, deren Elemente auf Grundlage eines Gleichheits Ausdrucks verknüpft wurden.

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

Der Gleichheits Ausdruck ist stärker eingeschränkt als ein regulärer Gleichheits Ausdruck:

* Beide Ausdrücke müssen als Wert klassifiziert werden.

* Beide Ausdrücke müssen auf mindestens eine Bereichs Variable verweisen.

* Auf die im Join-Abfrage Operator deklarierte Bereichs Variable muss von einem der Ausdrücke verwiesen werden, und dieser Ausdruck darf nicht auf andere Bereichs Variablen verweisen.

Wenn die Typen der beiden Ausdrücke nicht exakt denselben Typ haben,

* Wenn der Gleichheits Operator für die beiden Typen definiert ist, können beide Ausdrücke implizit in diesen konvertiert werden. er ist nicht `Object` und konvertiert dann beide Ausdrücke in diesen Typ.

* Andernfalls konvertieren Sie beide Ausdrücke in diesen Typ, wenn ein dominanter Typ vorhanden ist, in den beide Ausdrücke implizit konvertiert werden können.

* Andernfalls tritt ein Kompilierungsfehler auf.

Die Ausdrücke werden mithilfe von Hash Werten verglichen (d. h. durch Aufrufen von `GetHashCode()`) und nicht durch die Verwendung von Gleichheits Operatoren. Ein `Join`-Abfrage Operator kann im selben Operator mehrere Joins oder Gleichheits Bedingungen ausführen. Ein `Join`-Abfrage Operator wird nur unterstützt, wenn der Auflistungstyp eine-Methode enthält:

```vb
Function Join(inner As CS, _
                  outerSelector As Func(Of T, K), _
                  innerSelector As Func(Of S, K), _
                  resultSelector As Func(Of T, S, R)) As CR
```

Der Code

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs _
            Join y In ys On x Equals y _
            ...
```

wird im Allgemeinen in übersetzt.

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

__Nebenbei.__ `Join`, `On` und `Equals` sind keine reservierten Wörter.


### <a name="let-query-operator"></a>Let Query-Operator

Der `Let`-Abfrage Operator führt eine Ausdrucks Bereichs Variable ein. Dies ermöglicht das Berechnen eines zwischen Werts einmal, der in späteren Abfrage Operatoren mehrmals verwendet wird.

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

kann als äquivalent zu betrachtet werden:

```vb
For Each o In Orders
    Dim tax = o.Price * 0.088
    ...
Next o
```

Ein `Let`-Abfrage Operator wird nur unterstützt, wenn der Auflistungstyp eine-Methode enthält:

```vb
Function Select(selector As Func(Of T, R)) As CR
```

Der Code

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Let y = x * 10 _
            ...
```

wird im Allgemeinen in übersetzt.

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="select-query-operator"></a>SELECT Query-Operator

Der `Select`-Abfrage Operator ist wie der `Let`-Abfrage Operator, da er Ausdrucks Bereichs Variablen einführt. ein `Select`-Abfrage Operator verbirgt jedoch die aktuell verfügbaren Bereichs Variablen, anstatt Sie zu hinzuzufügen. Außerdem wird der Typ einer Ausdrucks Bereichs Variablen, die mit einem `Select`-Abfrage Operator eingeführt wurde, immer mithilfe von Regeln für den lokalen variablentyprückschluss abgeleitet. ein expliziter Typ kann nicht angegeben werden, und wenn kein Typ abgeleitet werden kann, tritt ein Kompilierzeitfehler auf.

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

der `Where`-Abfrage Operator hat nur Zugriff auf die vom `Select`-Operator eingeführte `name`-Bereichs Variable. Wenn der `Where`-Operator versucht hat, auf `cust` zu verweisen, ist ein Kompilierzeitfehler aufgetreten.

Anstatt die Namen der Bereichs Variablen explizit anzugeben, kann ein `Select`-Abfrage Operator die Namen der Bereichs Variablen ableiten, wobei die gleichen Regeln wie für Ausdrücke zum Erstellen von anonymen Typen verwendet werden. Zum Beispiel:

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.name, ord.ProductName _
        Where name.EndsWith("Smith")
```

Wenn der Name der Bereichs Variablen nicht angegeben wird und kein Name abgeleitet werden kann, tritt ein Kompilierzeitfehler auf. Wenn der `Select`-Abfrage Operator nur einen einzelnen Ausdruck enthält, tritt kein Fehler auf, wenn ein Name für diese Bereichs Variable nicht abgeleitet werden kann, aber die Bereichs Variable namenlos ist.  Zum Beispiel:

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.Name & " bought " & ord.ProductName _
        Take 10
```

Wenn eine Mehrdeutigkeit in einem `Select`-Abfrage Operator zwischen dem Zuweisen eines Namens zu einer Bereichs Variablen und einem Gleichheits Ausdruck vorliegt, wird die namens Zuweisung bevorzugt. Zum Beispiel:

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

Jeder Ausdruck im `Select`-Abfrage Operator muss als Wert klassifiziert werden. Ein `Select`-Abfrage Operator wird nur unterstützt, wenn der Auflistungstyp eine-Methode enthält:

```vb
Function Select(selector As Func(Of T, R)) As CR
```

Der Code

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Select x, y = x * 10 _
            ...
```

wird im Allgemeinen in übersetzt.

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="distinct-query-operator"></a>Unterschiedlicher Abfrage Operator

Der `Distinct`-Abfrage Operator schränkt die Werte in einer Auflistung nur auf diejenigen mit unterschiedlichen Werten ein, die durch Vergleichen des Elementtyps auf Gleichheit festgelegt werden.

```antlr
DistinctQueryOperator
    : LineTerminator? 'Distinct' LineTerminator?
    ;
```

Beispielsweise lautet die Abfrage:

```vb
Dim distinctCustomerPrice = _
    From cust In Customers, ord In cust.Orders _
    Select cust.Name, ord.Price _
    Distinct
```

gibt nur eine Zeile für jede einzelne Kopplung von Kunden Name und Bestellpreis zurück, auch wenn der Kunde über mehrere Bestellungen mit demselben Preis verfügt. Ein `Distinct`-Abfrage Operator wird nur unterstützt, wenn der Auflistungstyp eine-Methode enthält:

```vb
Function Distinct() As CT
```

Der Code

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Distinct _
            ...
```

wird im Allgemeinen in übersetzt.

```vb
Dim xs() As Integer = ...
Dim zs = xs.Distinct()...
```

__Nebenbei.__ `Distinct` ist kein reserviertes Wort.


### <a name="where-query-operator"></a>WHERE-Abfrage Operator

Der `Where`-Abfrage Operator schränkt die Werte in einer Auflistung auf die Werte ein, die eine bestimmte Bedingung erfüllen.

```antlr
WhereQueryOperator
    : LineTerminator? 'Where' LineTerminator? BooleanExpression
    ;
```

Ein `Where`-Abfrage Operator nimmt einen booleschen Ausdruck an, der für jeden Satz von Bereichs Variablen Werten ausgewertet wird. Wenn der Wert des Ausdrucks true ist, werden die Werte in der Ausgabe Auflistung angezeigt, andernfalls werden die Werte übersprungen. Der Abfrage Ausdruck lautet z. b.:

```vb
From cust In Customers, ord In Orders _
Where cust.ID = ord.CustomerID _
...
```

kann als Äquivalent zur schsted-Schleife angesehen werden.

```vb
For Each cust In Customers
    For Each ord In Orders
            If cust.ID = ord.CustomerID Then
                ...
            End If
    Next ord
Next cust
```

Ein `Where`-Abfrage Operator wird nur unterstützt, wenn der Auflistungstyp eine-Methode enthält:

```vb
Function Where(predicate As Func(Of T, B)) As CT
```

Der Code

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Where x < 10 _
            ...
```

wird im Allgemeinen in übersetzt.

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x As Integer) x < 10)...
```

__Nebenbei.__ `Where` ist kein reserviertes Wort.


### <a name="partition-query-operators"></a>Partitions Abfrage Operatoren

```antlr
PartitionQueryOperator
    : LineTerminator? 'Take' LineTerminator? Expression
    | LineTerminator? 'Take' 'While' LineTerminator? BooleanExpression
    | LineTerminator? 'Skip' LineTerminator? Expression
    | LineTerminator? 'Skip' 'While' LineTerminator? BooleanExpression
    ;
```

Der `Take`-Abfrage Operator führt zu den ersten `n` Elementen einer Auflistung. Bei Verwendung mit dem Modifizierer "`While`" führt der `Take`-Operator zu den ersten `n` Elementen einer Auflistung, die einen booleschen Ausdruck erfüllen. Der `Skip`-Operator überspringt die ersten `n`-Elemente einer Auflistung und gibt dann den Rest der Auflistung zurück.  Bei Verwendung in Verbindung mit dem Modifizierer "`While`" überspringt der `Skip`-Operator die ersten `n` Elemente einer Auflistung, die einen booleschen Ausdruck erfüllen, und gibt dann den Rest der Auflistung zurück. Die Ausdrücke in einem `Take`-oder `Skip`-Abfrage Operator müssen als Wert klassifiziert werden.

Ein `Take`-Abfrage Operator wird nur unterstützt, wenn der Auflistungstyp eine-Methode enthält:

```vb
Function Take(count As N) As CT
```

Ein `Skip`-Abfrage Operator wird nur unterstützt, wenn der Auflistungstyp eine-Methode enthält:

```vb
Function Skip(count As N) As CT
```

Ein `Take While`-Abfrage Operator wird nur unterstützt, wenn der Auflistungstyp eine-Methode enthält:

```vb
Function TakeWhile(predicate As Func(Of T, B)) As CT
```

Ein `Skip While`-Abfrage Operator wird nur unterstützt, wenn der Auflistungstyp eine-Methode enthält:

```vb
Function SkipWhile(predicate As Func(Of T, B)) As CT
```

Der Code

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Skip 10 _
            Take 5 _
            Skip While x < 10 _
            Take While x > 5 _
            ...
```

wird im Allgemeinen in übersetzt.

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Skip(10). _
        Take(5). _
        SkipWhile(Function(x) x < 10). _
        TakeWhile(Function(x) x > 5)...
```

__Nebenbei.__ `Take` und `Skip` sind keine reservierten Wörter.


### <a name="order-by-query-operator"></a>Order by-Abfrage Operator

Der `Order By`-Abfrage Operator sortiert die Werte, die in den Bereichs Variablen angezeigt werden. 

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

Ein `Order By`-Abfrage Operator übernimmt Ausdrücke, die die Schlüsselwerte angeben, die zum Sortieren der Iterations Variablen verwendet werden sollen. Die folgende Abfrage gibt z. b. die nach Preis sortierten Produkte zurück:

```vb
Dim productsByPrice = _
    From p In Products _
    Order By p.Price _
    Select p.Name
```

Eine Reihenfolge kann als `Ascending` gekennzeichnet werden. in diesem Fall liegen kleinere Werte vor größeren Werten, oder `Descending`. in diesem Fall kommen größere Werte vor kleineren Werten vor. Der Standardwert für eine Reihenfolge, wenn kein Wert angegeben ist `Ascending`. Beispielsweise gibt die folgende Abfrage Produkte nach Preis sortiert nach Preis mit dem teuersten Produkt zuerst zurück:

```vb
Dim productsByPriceDesc = _
    From p In Products _
    Order By p.Price Descending _
    Select p.Name
```

Der `Order By`-Abfrage Operator kann mehrere Ausdrücke für die Reihenfolge angeben. in diesem Fall wird die Auflistung in einer geordneten Weise angeordnet. Mit der folgenden Abfrage werden z. b. Kunden nach Bundesstaat und dann nach Ort innerhalb jedes Bundesstaats und dann nach Postleitzahl innerhalb der einzelnen Städte sortiert:

```vb
Dim customersByLocation = _
    From c In Customers _
    Order By c.State, c.City, c.ZIP _
    Select c.Name, c.State, c.City, c.ZIP
```

Die Ausdrücke in einem `Order By`-Abfrage Operator müssen als Wert klassifiziert werden. Ein `Order By`-Abfrage Operator wird nur unterstützt, wenn der Auflistungstyp mindestens eine der folgenden Methoden enthält:

```vb
Function OrderBy(keySelector As Func(Of T, K)) As CT
Function OrderByDescending(keySelector As Func(Of T, K)) As CT
```

Der Rückgabetyp `CT` muss eine *geordnete*Auflistung sein. Eine geordnete Auflistung ist ein Sammlungstyp, der eine oder beide Methoden enthält:

```vb
Function ThenBy(keySelector As Func(Of T, K)) As CT
Function ThenByDescending(keySelector As Func(Of T, K)) As CT
```

Der Code

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Order By x Ascending, x Mod 2 Descending _
            ...
```

wird im Allgemeinen in übersetzt.

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.OrderBy(Function(x) x).ThenByDescending(Function(x) x Mod 2)...
```

__Nebenbei.__ Da Abfrage Operatoren einfache Syntax Methoden zuordnen, die einen bestimmten Abfrage Vorgang implementieren, wird die Beibehaltung der Reihenfolge nicht von der Sprache vorgegeben und durch die Implementierung des Operators selbst bestimmt. Dies ist sehr ähnlich wie bei benutzerdefinierten Operatoren darin, dass die Implementierung, die den Additions Operator für einen benutzerdefinierten numerischen Typ überlädt, möglicherweise keine Aktion ausführt, die einer Addition ähnelt. Um die Vorhersagbarkeit beizubehalten, wird natürlich nicht empfohlen, etwas zu implementieren, das nicht den Erwartungen der Benutzer entspricht.

__Nebenbei.__ `Order` und `By` sind keine reservierten Wörter.


### <a name="group-by-query-operator"></a>Group by-Abfrage Operator

Der `Group By`-Abfrage Operator gruppiert die Bereichs Variablen im Gültigkeitsbereich basierend auf einem oder mehreren Ausdrücken und erzeugt dann basierend auf diesen Gruppierungen neue Bereichs Variablen.

```antlr
GroupByQueryOperator
    : LineTerminator? 'Group' ( LineTerminator? ExpressionRangeVariableDeclarationList )?
      LineTerminator? 'By' LineTerminator? ExpressionRangeVariableDeclarationList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

Beispielsweise werden mit der folgenden Abfrage alle Kunden nach `State` gruppiert, und anschließend werden die Anzahl und das durchschnittliche Alter der einzelnen Gruppen berechnet:

```vb
Dim averageAges = _
    From cust In Customers _
    Group By cust.State _
    Into Count(), Average(cust.Age)
```

Der `Group By`-Abfrage Operator hat drei Klauseln: die optionale `Group`-Klausel, die `By`-Klausel und die `Into`-Klausel. Die `Group`-Klausel hat dieselbe Syntax und Auswirkung wie ein `Select`-Abfrage Operator, mit dem Unterschied, dass Sie sich nur auf die in der `Into`-Klausel verfügbaren Bereichs Variablen und nicht auf die `By`-Klausel auswirkt. Zum Beispiel:

```vb
Dim averageAges = _
    From cust In Customers _
    Group cust.Age By cust.State _
    Into Count(), Average(Age)
```

Die `By`-Klausel deklariert Ausdrucks Bereichs Variablen, die als Schlüsselwerte in der Gruppierungs Operation verwendet werden. Die `Into`-Klausel ermöglicht die Deklaration von Ausdrucks Bereichs Variablen, die Aggregationen für die einzelnen Gruppen berechnen, die durch die `By`-Klausel gebildet werden. Innerhalb der `Into`-Klausel kann der Ausdrucks Bereichs Variable nur ein Ausdruck zugewiesen werden, bei dem es sich um einen Methodenaufruf einer *Aggregatfunktion*handelt. Eine Aggregatfunktion ist eine Funktion für den Auflistungstyp der Gruppe (bei der es sich nicht unbedingt um denselben Auflistungstyp der ursprünglichen Auflistung handeln kann), der wie eine der folgenden Methoden aussieht:

```vb
Function _name_() As _type_
Function _name_(selector As Func(Of T, R)) As R
```

Wenn eine Aggregatfunktion ein Delegatargument annimmt, kann der Aufruf Ausdruck einen Argument Ausdruck aufweisen, der als Wert klassifiziert werden muss.  Der Argument Ausdruck kann die Bereichs Variablen verwenden, die sich im Gültigkeitsbereich befinden. innerhalb des Aufrufes einer Aggregatfunktion stellen diese Bereichs Variablen die Werte in der Gruppe dar, die gebildet werden, und nicht alle Werte in der Auflistung. Im ursprünglichen Beispiel in diesem Abschnitt berechnet die Funktion "`Average`" beispielsweise den Durchschnitt der Nutzungszeiten der Kunden pro Bundesstaat und nicht für alle Kunden.

Bei allen Auflistungs Typen wird davon ausgegangen, dass die Aggregatfunktion `Group` definiert ist, die keine Parameter annimmt und einfach die Gruppe zurückgibt. Weitere Standard Aggregatfunktionen, die von einem Sammlungstyp bereitgestellt werden können, sind:

`Count` und `LongCount`, die die Anzahl der Elemente in der Gruppe oder die Anzahl der Elemente in der Gruppe zurückgeben, die einen booleschen Ausdruck erfüllen. `Count` und `LongCount` werden nur unterstützt, wenn der Auflistungstyp eine der folgenden Methoden enthält:

```vb
Function Count() As N
Function Count(selector As Func(Of T, B)) As N
Function LongCount() As N
Function LongCount(selector As Func(Of T, B)) As N
```

`Sum`, wodurch die Summe eines Ausdrucks über alle Elemente in der Gruppe zurückgegeben wird. `Sum` wird nur unterstützt, wenn der Auflistungstyp eine der folgenden Methoden enthält:

```vb
Function Sum() As N
Function Sum(selector As Func(Of T, N)) As N
```

`Min`, der den minimalen Wert eines Ausdrucks über alle Elemente in der Gruppe zurückgibt. `Min` wird nur unterstützt, wenn der Auflistungstyp eine der folgenden Methoden enthält:

```vb
Function Min() As N
Function Min(selector As Func(Of T, N)) As N
```

`Max`, wodurch der Höchstwert eines Ausdrucks über alle Elemente in der Gruppe zurückgegeben wird. `Max` wird nur unterstützt, wenn der Auflistungstyp eine der folgenden Methoden enthält:

```vb
Function Max() As N
Function Max(selector As Func(Of T, N)) As N
```

`Average`, wodurch der Durchschnitt eines Ausdrucks über alle Elemente in der Gruppe zurückgegeben wird. `Average` wird nur unterstützt, wenn der Auflistungstyp eine der folgenden Methoden enthält:

```vb
Function Average() As N
Function Average(selector As Func(Of T, N)) As N
```

`Any`, der bestimmt, ob eine Gruppe Member enthält oder ob ein boolescher Ausdruck für ein beliebiges Element in der Gruppe true ist. `Any` gibt einen Wert zurück, der in einem booleschen Ausdruck verwendet werden kann, und wird nur unterstützt, wenn der Auflistungstyp eine der-Methoden enthält:

```vb
Function Any() As B
Function Any(predicate As Func(Of T, B)) As B
```

`All`, der bestimmt, ob ein boolescher Ausdruck für alle Elemente in der Gruppe "true" ist. `All` gibt einen Wert zurück, der in einem booleschen Ausdruck verwendet werden kann, und wird nur unterstützt, wenn der Auflistungstyp eine Methode enthält:

```vb
Function All(predicate As Func(Of T, B)) As B
```

Nach einem `Group By`-Abfrage Operator sind die Bereichs Variablen, die sich zuvor im Gültigkeitsbereich befinden, ausgeblendet, und die von den Klauseln `By` und `Into` eingeführten Bereichs Variablen sind verfügbar. Ein `Group By`-Abfrage Operator wird nur unterstützt, wenn der Auflistungstyp die-Methode enthält:

```vb
Function GroupBy(keySelector As Func(Of T, K), _
                      resultSelector As Func(Of K, CT, R)) As CR
```

Bereichs Variablen Deklarationen in der `Group`-Klausel werden nur unterstützt, wenn der Auflistungstyp die Methode enthält:

```vb
Function GroupBy(keySelector As Func(Of T, K), _
                      elementSelector As Func(Of T, S), _
                      resultSelector As Func(Of K, CS, R)) As CR
```

Der Code

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Group y = x * 10, z = x / 10 By evenOdd = x Mod 2 _
            Into Sum(y), Average(z) _
            ...
```

wird im Allgemeinen in übersetzt.

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

__Nebenbei.__ `Group`, `By` und `Into` sind keine reservierten Wörter.


### <a name="aggregate-query-operator"></a>Aggregat Abfrage Operator

Der `Aggregate`-Abfrage Operator führt eine ähnliche Funktion wie der `Group By`-Operator aus, außer er ermöglicht das aggregierten von Gruppen, die bereits gebildet wurden. Da die Gruppe bereits gebildet wurde, verbirgt die `Into`-Klausel eines `Aggregate`-Abfrage Operators nicht die Bereichs Variablen im Gültigkeitsbereich. (auf diese Weise ist `Aggregate` eher ähnlich wie ein `Let`, und `Group By` ist eher ein `Select`).

```antlr
AggregateQueryOperator
    : LineTerminator? 'Aggregate' LineTerminator? CollectionRangeVariableDeclaration QueryOperator*
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

Die folgende Abfrage aggregiert z. b. die Summe aller Bestellungen, die von Kunden in Washington gestellt werden:

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Into Sum(order.Total)
```

Das Ergebnis dieser Abfrage ist eine Auflistung, deren Elementtyp ein anonymer Typ mit einer Eigenschaft namens "`cust`" ist, die als `Customer` typisiert ist, und eine Eigenschaft mit dem Namen "`Sum`" als "`Integer`"

Im Gegensatz zu `Group By` können zusätzliche Abfrage Operatoren zwischen den Klauseln `Aggregate` und `Into` platziert werden. Zwischen einer `Aggregate`-Klausel und dem Ende der `Into`-Klausel können alle Bereichs Variablen im Gültigkeitsbereich verwendet werden, einschließlich derjenigen, die von der `Aggregate`-Klausel deklariert werden. Die folgende Abfrage aggregiert z. b. die Gesamtsumme aller Bestellungen, die von Kunden in Washington vor 2006 platziert werden:

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum = Sum(order.Total)
```

Der `Aggregate`-Operator kann auch zum Starten eines Abfrage Ausdrucks verwendet werden. In diesem Fall ist das Ergebnis des Abfrage Ausdrucks der einzelne Wert, der durch die `Into`-Klausel berechnet wird. Die folgende Abfrage berechnet z. b. die Summe aller Bestell Summen vor dem 1. Januar 2006:

```vb
Dim ordersTotal = _
    Aggregate order In Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum(order.Total)
```

Das Ergebnis der Abfrage ist ein einzelner `Integer`-Wert. Es ist immer ein `Aggregate`-Abfrage Operator verfügbar (obwohl die Aggregatfunktion auch verfügbar sein muss, damit der Ausdruck gültig ist). Der Code

```vb
Dim xs() As Integer = ...
Dim zs = _
    Aggregate x In xs _
    Where x < 5 _
    Into Sum()
```

wird im Allgemeinen in übersetzt.

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x) x < 5).Sum()
```

__Beachten Sie.__  `Aggregate` und `Into` sind keine reservierten Wörter.


### <a name="group-join-query-operator"></a>Group Join Query-Operator

Der `Group Join`-Abfrage Operator kombiniert die Funktionen der Abfrage Operatoren `Join` und `Group By` zu einem einzigen Operator. `Group Join` verknüpft zwei Auflistungen basierend auf übereinstimmenden Schlüsseln, die aus den Elementen extrahiert wurden, und gruppiert alle Elemente auf der rechten Seite des Joins, die mit einem bestimmten Element auf der linken Seite des Joins übereinstimmen. Folglich erzeugt der-Operator eine Reihe von hierarchischen Ergebnissen.

```antlr
GroupJoinQueryOperator
    : LineTerminator? 'Group' 'Join' LineTerminator? CollectionRangeVariableDeclaration
      JoinOrGroupJoinQueryOperator? LineTerminator? 'On' LineTerminator? JoinConditionList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

Beispielsweise werden mit der folgenden Abfrage Elemente erstellt, die den Namen eines einzelnen Kunden, eine Gruppe aller Bestellungen und die Gesamtmenge aller Bestellungen enthalten:

```vb
Dim custsWithOrders = _
    From cust In Customers _
    Group Join order In Orders On cust.ID Equals order.CustomerID _ 
    Into Orders = Group, OrdersTotal = Sum(order.Total) _
    Select cust.Name, Orders, OrdersTotal
```

Das Ergebnis der Abfrage ist eine Auflistung, deren Elementtyp ein anonymer Typ mit drei Eigenschaften ist: `Name`, typisiert als `String`, `Orders` typisiert als Auflistung, deren Elementtyp `Order` und `OrdersTotal`, typisiert als `Integer`. Ein `Group Join`-Abfrage Operator wird nur unterstützt, wenn der Auflistungstyp die-Methode enthält:

```vb
Function GroupJoin(inner As CS, _
                         outerSelector As Func(Of T, K), _
                         innerSelector As Func(Of S, K), _
                         resultSelector As Func(Of T, CS, R)) As CR
```

Der Code

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs _
            Group Join y in ys On x Equals y _
            Into g = Group _
            ...
```

wird im Allgemeinen in übersetzt.

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

__Nebenbei.__ `Group`, `Join` und `Into` sind keine reservierten Wörter.


## <a name="conditional-expressions"></a>Bedingte Ausdrücke

Ein bedingter `If`-Ausdruck testet einen Ausdruck und gibt einen Wert zurück.

```antlr
ConditionalExpression
    : 'If' OpenParenthesis BooleanExpression Comma Expression Comma Expression CloseParenthesis
    | 'If' OpenParenthesis Expression Comma Expression CloseParenthesis
    ;
```

Anders als die Lauf Zeitfunktion von `IIF` wertet ein bedingter Ausdruck jedoch bei Bedarf nur seine Operanden aus. So löst z. b. der Ausdruck `If(c Is Nothing, c.Name, "Unknown")` keine Ausnahme aus, wenn der Wert von `c` `Nothing` ist. Der bedingte Ausdruck verfügt über zwei Formen: eine mit zwei Operanden und eine, die drei Operanden annimmt.

Wenn drei Operanden bereitgestellt werden, müssen alle drei Ausdrücke als Werte klassifiziert werden, und der erste Operand muss ein boolescher Ausdruck sein. Wenn das Ergebnis des Ausdrucks "true" ist, ist der zweite Ausdruck das Ergebnis des Operators. andernfalls ist der dritte Ausdruck das Ergebnis des Operators. Der Ergebnistyp des Ausdrucks ist der bestimmende Typ zwischen den Typen des zweiten und dritten Ausdrucks. Wenn kein dominanter Typ vorhanden ist, tritt ein Kompilierzeitfehler auf.

Wenn zwei Operanden bereitgestellt werden, müssen beide Operanden als Werte klassifiziert werden, und der erste Operand muss entweder ein Referenztyp oder ein Werte zulässt-Werttyp sein. Der Ausdruck `If(x, y)` wird dann so ausgewertet, als wäre der Ausdruck `If(x IsNot Nothing, x, y)` mit zwei Ausnahmen. Zuerst wird der erste Ausdruck nur einmal ausgewertet, und zweitens, wenn der Typ des zweiten Operanden ein Werttyp ist, der keine NULL-Werte zulässt, und der Typ des ersten Operanden ist, wird der `?` aus dem Typ des ersten Operanden entfernt, wenn der bestimmende Typ für das Ergebnis bestimmt wird. der Typ des Ausdrucks. Zum Beispiel:

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

In beiden Formen des Ausdrucks wird der Typ, wenn ein Operand `Nothing` ist, nicht zum Bestimmen des vorherrschenden Typs verwendet. Im Fall des Ausdrucks `If(<expression>, Nothing, Nothing)` gilt der vorherrschende Typ als `Object`.


## <a name="xml-literal-expressions"></a>XML-Literale Ausdrücke

Ein XML-Literalausdruck stellt einen XML-Wert (Extensible Markup Language) 1,0 dar.

```antlr
XMLLiteralExpression
    : XMLDocument
    | XMLElement
    | XMLProcessingInstruction
    | XMLComment
    | XMLCDATASection
    ;
```

Das Ergebnis eines XML-Literalen Ausdrucks ist ein Wert, der als einer der Typen aus dem `System.Xml.Linq`-Namespace typisiert ist. Wenn die Typen in diesem Namespace nicht verfügbar sind, führt ein XML-Literalausdruck zu einem Kompilierzeitfehler. Die Werte werden durch Konstruktoraufrufe generiert, die aus dem XML-Literalausdruck übersetzt werden. Beispielsweise ist der Code:

```vb
Dim book As System.Xml.Linq.XElement = _
    <book title="My book"></book>
```

entspricht ungefähr dem Code:

```vb
Dim book As System.Xml.Linq.XElement = _
    New System.Xml.Linq.XElement( _
        "book", _
        New System.Xml.Linq.XAttribute("title", "My book"))
```

Ein XML-Literalausdruck kann die Form eines XML-Dokuments, ein XML-Element, eine XML-Verarbeitungsanweisung, einen XML-Kommentar oder einen CDATA-Abschnitt annehmen.

__Nebenbei.__ Diese Spezifikation enthält nur eine ausreichende Beschreibung von XML, um das Verhalten der Visual Basic Sprache zu beschreiben. Weitere Informationen zu XML finden Sie unter http://www.w3.org/TR/REC-xml/.


### <a name="lexical-rules"></a>Lexikalische Regeln

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

XML-Literale Ausdrücke werden mithilfe der lexikalischen Regeln von XML interpretiert, anstelle der lexikalischen Regeln regulärer Visual Basic Codes. Die beiden Regelsätze unterscheiden sich in der Regel in folgenden Punkten:

* Leerraum ist in XML signifikant. Daher gibt die Grammatik für XML-Literalausdrücke explizit an, wo Leerraum zulässig ist. Leerzeichen werden nicht beibehalten, es sei denn, Sie treten im Kontext von Zeichendaten in einem Element auf. Zum Beispiel:

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

* XML-zeileendeleerräume werden entsprechend der XML-Spezifikation normalisiert.

* Bei XML muss die Groß- und Kleinschreibung beachtet werden. Schlüsselwörter müssen exakt mit der Groß-/Kleinschreibung übereinstimmen, andernfalls tritt ein Kompilierzeitfehler auf.

* Zeilen Abschluss Zeichen werden als Leerzeichen in XML betrachtet. Folglich werden in XML-Literalausdrücken keine Zeilen Fortsetzungs Zeichen benötigt.

* XML akzeptiert keine Zeichen voller Breite. Wenn Zeichen in voller Breite verwendet werden, tritt ein Kompilierzeitfehler auf.


### <a name="embedded-expressions"></a>Eingebettete Ausdrücke

XML-Literalausdrücke können *eingebettete Ausdrücke*enthalten. Ein eingebetteter Ausdruck ist ein Visual Basic Ausdruck, der ausgewertet und zum Ausfüllen eines oder mehrerer Werte am Speicherort des eingebetteten Ausdrucks verwendet wird.

```antlr
XMLEmbeddedExpression
    : '<' '%' '=' LineTerminator? Expression LineTerminator? '%' '>'
    ;
```

Im folgenden Code wird z. b. die Zeichenfolge `John Smith` als Wert des XML-Elements platziert:

```vb
Dim name as String = "John Smith"
Dim element As System.Xml.Linq.XElement = <customer><%= name %></customer>
```

Ausdrücke können in eine Reihe von Kontexten eingebettet werden. Der folgende Code erzeugt z. b. ein Element mit dem Namen `customer`:

```vb
Dim name As String = "customer"
Dim element As System.Xml.Linq.XElement = <<%= name %>>John Smith</>
```

Jeder Kontext, in dem ein eingebetteter Ausdruck verwendet werden kann, gibt die Typen an, die akzeptiert werden. Im Kontext des Ausdrucks Teils eines eingebetteten Ausdrucks gelten die normalen lexikalischen Regeln für Visual Basic Code weiterhin, sodass z. b. Zeilen Fortsetzungen verwendet werden müssen:

```vb
' Visual Basic expression uses line continuation, XML does not
Dim element As System.Xml.Linq.XElement = _
    <<%= name & _
          name %>>John 
                     Smith</>
```


### <a name="xml-documents"></a>XML-Dokumente

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

Ein XML-Dokument führt zu einem Wert, der als `System.Xml.Linq.XDocument` typisiert ist. Anders als bei der XML 1,0-Spezifikation sind XML-Dokumente in XML-Literalausdrücken zum Angeben des XML-Dokument Prologs erforderlich. XML-Literale Ausdrücke ohne XML-Dokument Prolog werden als ihre einzelne Entität interpretiert. Zum Beispiel:

```vb
Dim doc As System.Xml.Linq.XDocument = _
    <?xml version="1.0"?>
    <?instruction?>
    <customer>Bob</>

Dim pi As System.Xml.Linq.XProcessingInstruction = _
    <?instruction?>
```

Ein XML-Dokument kann einen eingebetteten Ausdruck enthalten, dessen Typ einen beliebigen Typ aufweisen kann. zur Laufzeit muss das Objekt jedoch die Anforderungen des `XDocument`-Konstruktors erfüllen, oder es tritt ein Laufzeitfehler auf.

Im Gegensatz zu regulären XML-Dokumenten unterstützen XML-Dokument Ausdrücke keine DTDs (Dokumenttyp Deklarationen). Außerdem wird das Codierungs Attribut, sofern angegeben, ignoriert, da die Codierung des XML-Literalen Ausdrucks immer mit der Codierung der Quelldatei identisch ist.

__Nebenbei.__ Obwohl das Encoding-Attribut ignoriert wird, ist es immer noch ein gültiges Attribut, um die Möglichkeit aufrechtzuerhalten, gültige XML 1,0-Dokumente in den Quellcode aufzunehmen.


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

Ein XML-Element führt zu einem Wert, der als `System.Xml.Linq.XElement` typisiert ist. Im Gegensatz zum regulären XML-Code können XML-Elemente den Namen im schließenden Tag weglassen, und das aktuelle am meisten Nested Element wird geschlossen. Zum Beispiel:

```vb
Dim name = <name>Bob</>
```

Attribut Deklarationen in einem XML-Element führen zu Werten, die als `System.Xml.Linq.XAttribute` eingegeben werden. Attributwerte werden entsprechend der XML-Spezifikation normalisiert. Wenn der Wert eines Attributs `Nothing` ist, wird das Attribut nicht erstellt, sodass der Attribut Wertausdruck nicht auf `Nothing` geprüft werden muss. Zum Beispiel:

```vb
Dim expr = Nothing

' Throws null argument exception
Dim direct = New System.Xml.Linq.XElement( _
    "Name", _
    New System.Xml.Linq.XAttribute("Length", expr))

' Doesn't throw exception, the result is <Name/>
Dim literal = <Name Length=<%= expr %>/>
```

XML-Elemente und-Attribute können an den folgenden Stellen die folgenden Elemente enthalten:

Der Name des Elements. in diesem Fall muss der eingebettete Ausdruck ein Wert eines Typs sein, der implizit in `System.Xml.Linq.XName` konvertiert werden kann. Zum Beispiel:

```vb
Dim name = <<%= "name" %>>Bob</>
```

Der Name eines Attributs des Elements. in diesem Fall muss der eingebettete Ausdruck ein Wert eines Typs sein, der implizit in `System.Xml.Linq.XName` konvertiert werden kann. Zum Beispiel:

```vb
Dim name = <name <%= "length" %>="3">Bob</>
```

Der Wert eines Attributs des Elements. in diesem Fall kann der eingebettete Ausdruck ein Wert eines beliebigen Typs sein. Zum Beispiel:

```vb
Dim name = <name length=<%= 3 %>>Bob</>
```

Ein Attribut des-Elements. in diesem Fall kann der eingebettete Ausdruck ein Wert eines beliebigen Typs sein. Zum Beispiel:

```vb
Dim name = <name <%= new XAttribute("length", 3) %>>Bob</>
```

Der Inhalt des Elements. in diesem Fall kann der eingebettete Ausdruck ein Wert eines beliebigen Typs sein. Zum Beispiel:

```vb
Dim name = <name><%= "Bob" %></>
```

Wenn der Typ des eingebetteten Ausdrucks `Object()` ist, wird das Array als ParamArray an den `XElement`-Konstruktor übergeben.


### <a name="xml-namespaces"></a>XML-Namespaces

XML-Elemente können XML-Namespace Deklarationen enthalten, wie in der Spezifikation für XML-Namespaces 1,0 definiert.

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

Die Einschränkungen beim Definieren der Namespaces `xml` und `xmlns` werden erzwungen und verursachen Kompilierzeitfehler. XML-Namespace Deklarationen können keinen eingebetteten Ausdruck für ihren Wert aufweisen. der angegebene Wert muss ein nicht leeres Zeichenfolgenliteralzeichen sein. Zum Beispiel:

```vb
' Declares a valid namespace
Dim customer = <db:customer xmlns:db="http://example.org/database">Bob</>

' Error: xmlns cannot be re-defined
Dim bad1 = <elem xmlns:xmlns="http://example.org/namespace"/>

' Error: cannot have an embedded expression
Dim bad2 = <elem xmlns:db=<%= "http://example.org/database" %>>Bob</>
```

__Nebenbei.__ Diese Spezifikation enthält nur eine ausreichende Beschreibung des XML-Namespace, um das Verhalten der Visual Basic Sprache zu beschreiben. Weitere Informationen zu XML-Namespaces finden Sie unter http://www.w3.org/TR/REC-xml-names/.

XML-Element-und-Attributnamen können mithilfe von Namespace Namen qualifiziert werden. Namespaces werden als in regulärem XML-Code gebunden, mit der Ausnahme, dass alle auf der Dateiebene deklarierten Namespace Importe als in einem Kontext deklariert werden, der die Deklaration einschließt, die selbst von allen von der Kompilierung deklarierten Namespace Importen eingeschlossen ist. Umgebung. Wenn kein Namespace Name gefunden werden kann, tritt ein Kompilierzeitfehler auf. Zum Beispiel:

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

In einem-Element deklarierte XML-Namespaces gelten nicht für XML-Literale innerhalb von eingebetteten Ausdrücken. Zum Beispiel:

```vb
' Error: Namespace prefix 'db' is not declared
Dim customer = _
    <db:customer xmlns:db="http://example.org/database">
        <%= <db:customer>Bob</> %>
    </>
```

__Nebenbei.__ Dies liegt daran, dass der eingebettete Ausdruck etwas sein kann, einschließlich eines Funktions Aufrufes. Wenn der Funktions Aufrufausdruck einen XML-Literalausdruck enthielt, ist nicht klar, ob Programmierer erwarten, dass der XML-Namespace angewendet oder ignoriert wird.


### <a name="xml-processing-instructions"></a>XML-Verarbeitungsanweisungen

Eine XML-Verarbeitungsanweisung führt zu einem Wert, der als `System.Xml.Linq.XProcessingInstruction` typisiert ist. XML-Verarbeitungsanweisungen können keine eingebetteten Ausdrücke enthalten, da es sich um eine gültige Syntax innerhalb der Verarbeitungsanweisung handelt.

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

Ein XML-Kommentar führt zu einem Wert, der als `System.Xml.Linq.XComment` typisiert ist. XML-Kommentare dürfen keine eingebetteten Ausdrücke enthalten, da es sich um eine gültige Syntax im Kommentar handelt.

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

Ein CDATA-Abschnitt führt zu einem Wert, der als `System.Xml.Linq.XCData` typisiert ist. CDATA-Abschnitte können keine eingebetteten Ausdrücke enthalten, da es sich um eine gültige Syntax im CDATA-Abschnitt handelt.

```antlr
XMLCDATASection
    : '<' '!' ( 'CDATA' '[' XMLCDATASectionString? ']' )? '>'
    ;

XMLCDATASectionString
    : '<Any XMLString that does not contain the string "]]>">'
    ;
```

## <a name="xml-member-access-expressions"></a>XML-Member-Zugriffs Ausdrücke

Ein XML-Member-Zugriffs Ausdruck greift auf die Member eines XML-Werts zu.

```antlr
XMLMemberAccessExpression
    : Expression '.' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? IdentifierOrKeyword
    | Expression '.' '.' '.' LineTerminator? '<' XMLQualifiedName '>'
    ;
```

Es gibt drei Typen von XML-Member-Zugriffs Ausdrücken:

* *Element Zugriff*, bei dem ein XML-Name einem einzelnen Punkt folgt. Zum Beispiel:

  ```vb
  Dim customer = _
      <customer>
          <name>Bob</>
      </>
  Dim customerName = customer.<name>.Value
  ```

  Der Element Zugriff wird der-Funktion zugeordnet:

  ```vb
  Function Elements(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XNode)
  ```

  Das obige Beispiel entspricht dem folgenden Beispiel:

  ```vb
  Dim customerName = customer.Elements("name").Value
  ```

* *Attribut Zugriff*, bei dem ein Visual Basic Bezeichner einem Punkt und einem at-Zeichen folgt, oder wenn ein XML-Name einem Punkt und einem @-Zeichen folgt. Zum Beispiel:

  ```vb
  Dim customer = <customer age="30"/>
  Dim customerAge = customer.@age
  ```

  Der Attribut Zugriff wird der-Funktion zugeordnet:

  ```vb
  Function AttributeValue(name As System.Xml.Linq.XName) as String
  ```

  Das obige Beispiel entspricht dem folgenden Beispiel:

  ```vb
  Dim customerAge = customer.AttributeValue("age")
  ```

  __Nebenbei.__ Die `AttributeValue`-Erweiterungsmethode (sowie die zugehörige Erweiterungs Eigenschaft `Value`) ist zurzeit nicht in einer Assembly definiert. Wenn die Erweiterungs Mitglieder benötigt werden, werden Sie automatisch in der erstellten Assembly definiert.

* Nachfolger *, bei*dem ein XML-Name drei Punkte folgt. Zum Beispiel:

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

  Der Zugriff auf Nachfolger Elemente wird der-Funktion zugeordnet:

  ```vb
  Function Descendents(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XElement)
  ```

  Das obige Beispiel entspricht dem folgenden Beispiel:

  ```vb
  Dim customers = company.Descendants("customer")
  ```

Der Basis Ausdruck eines XML-Member-Zugriffs Ausdrucks muss ein-Wert sein und muss den folgenden Typ aufweisen:

* , Wenn ein Element oder Nachfolger, `System.Xml.Linq.XContainer` oder ein abgeleiteter Typ, oder `System.Collections.Generic.IEnumerable(Of T)` oder ein abgeleiteter Typ, wobei `T` `System.Xml.Linq.XContainer` oder ein abgeleiteter Typ ist.

* Wenn ein Attribut Zugriff, `System.Xml.Linq.XElement` oder ein abgeleiteter Typ oder `System.Collections.Generic.IEnumerable(Of T)` oder ein abgeleiteter Typ, wobei `T` `System.Xml.Linq.XElement` oder ein abgeleiteter Typ ist.

Namen in XML-Member-Zugriffs Ausdrücken dürfen nicht leer sein. Sie können mit einem Namespace qualifiziert sein, wobei alle durch Importe definierten Namespaces verwendet werden. Zum Beispiel:

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

Leerraum ist nach dem Punkt (n) in einem XML-Element Zugriffs Ausdruck oder zwischen den spitzen Klammern und dem Namen nicht zulässig. Zum Beispiel:

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

Wenn die Typen im `System.Xml.Linq`-Namespace nicht verfügbar sind, führt ein XML-Member-Zugriffs Ausdruck zu einem Kompilierzeitfehler.


## <a name="await-operator"></a>Await-Operator

Der Erwartungs Operator bezieht sich auf Async-Methoden, die im Abschnitt " [Async-Methoden](statements.md#async-methods)" beschrieben werden.

```antlr
AwaitOperatorExpression
    : 'Await' Expression
    ;
```

`Await` ist ein reserviertes Wort, wenn die unmittelbar einschließende Methode oder der Lambda-Ausdruck, in dem Sie angezeigt wird, einen `Async`-Modifizierer aufweist und der `Await` nach diesem `Async`-Modifizierer erscheint. Es ist nicht an anderer Stelle reserviert. Es ist auch nicht in Präprozessordirektiven reserviert. Der Erwartungs Operator ist nur im Text einer Methode oder eines Lambda-Ausdrucks zulässig, bei dem es sich um ein reserviertes Wort handelt. Innerhalb der unmittelbar einschließenden Methode oder des Lambda-Ausdrucks kann ein Erwartungs Ausdruck nicht innerhalb des Texts eines `Catch`-oder `Finally`-Blocks oder innerhalb des Texts einer `SyncLock`-Anweisung oder innerhalb eines Abfrage Ausdrucks auftreten.

Der Erwartungs Operator nimmt einen einzelnen Ausdruck an, der als Wert klassifiziert werden muss und *dessen Typ ein* erwartbarer Typ sein muss, oder `Object`. Wenn der Typ `Object` ist, wird die gesamte Verarbeitung bis zur Laufzeit verzögert. Ein Typ `C` wird als erwartbar bezeichnet, wenn Folgendes zutrifft:

* `C` enthält eine barrierefreie Instanz oder Erweiterungsmethode mit dem Namen `GetAwaiter`, die über keine Argumente verfügt und einen Typ `E` zurückgibt.

* `E` enthält eine lesbare Instanz oder Erweiterungs Eigenschaft mit dem Namen `IsCompleted`, die keine Argumente annimmt und den Typ "Boolean" aufweist.

* `E` enthält eine barrierefreie Instanz oder Erweiterungsmethode mit dem Namen `GetResult`, die keine Argumente annimmt.

* `E` implementiert entweder `System.Runtime.CompilerServices.INotifyCompletion` oder `ICriticalNotifyCompletion`.

Wenn `GetResult` ein `Sub` war, wird der Erwartungs Ausdruck als void klassifiziert. Andernfalls wird der Erwartungs Ausdruck als Wert klassifiziert, und sein Typ ist der Rückgabetyp der `GetResult`-Methode.

Im folgenden finden Sie ein Beispiel für eine Klasse, die gewartet werden kann:

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

__Nebenbei.__ Bibliotheks Autoren sollten das Muster befolgen, dass Sie den Fortsetzungs Delegaten auf dem gleichen `SynchronizationContext` aufrufen, weil ihre `OnCompleted` selbst aufgerufen wurde. Außerdem sollte der Wiederaufnahme Delegat nicht synchron innerhalb der `OnCompleted`-Methode ausgeführt werden, da dies zu einem Stapelüberlauf führen kann: Stattdessen sollte der Delegat für die nachfolgende Ausführung in die Warteschlange eingereiht werden.

Wenn die Ablauf Steuerung einen `Await`-Operator erreicht, sieht das Verhalten wie folgt aus.

1.  Die `GetAwaiter`-Methode des Erwartungs Operanden wird aufgerufen. Das Ergebnis dieses aufnamens wird als *akellner*bezeichnet.

2.  Die `IsCompleted`-Eigenschaft des akellners wird abgerufen. Wenn das Ergebnis true ist, gilt Folgendes:

    21. Die `GetResult`-Methode des akellners wird aufgerufen. Wenn `GetResult` eine Funktion ist, ist der Wert des Erwartungs Ausdrucks der Rückgabewert dieser Funktion.

3.  Wenn die isabgeschlossene-Eigenschaft nicht wahr ist, gilt Folgendes:

    31. Entweder wird entweder `ICriticalNotifyCompletion.UnsafeOnCompleted` auf dem akellner aufgerufen (wenn der Typ des akellons `E` `ICriticalNotifyCompletion`) oder `INotifyCompletion.OnCompleted` implementiert (andernfalls). In beiden Fällen übergibt Sie einen *Wiederaufnahme* -Delegaten, der der aktuellen Instanz der Async-Methode zugeordnet ist.

    32. Der Kontrollpunkt der aktuellen Async-Methoden Instanz wird angehalten, und die Ablauf Steuerung wird im *aktuellen* Aufrufer (definiert im Abschnitt [Async-Methoden](statements.md#async-methods)) fortgesetzt.

    33. Wenn später der Wiederaufnahme Delegat aufgerufen wird,

        331. der Wiederherstellungs Delegat stellt zuerst `System.Threading.Thread.CurrentThread.ExecutionContext` für den Zeitpunkt wieder her, an dem `OnCompleted` aufgerufen wurde.
        332. Anschließend wird die Ablauf Steuerung am Steuerungspunkt der Async-Methoden Instanz fortgesetzt (siehe Abschnitt [Async-Methoden](statements.md#async-methods)).
        333. Dabei wird die `GetResult`-Methode des akellners aufgerufen, wie in 2,1 oben.

Wenn der Erwartungs Operand ein Type-Objekt aufweist, wird dieses Verhalten bis zur Laufzeit verzögert:

- Schritt 1 wird durch Aufrufen von getakellner () ohne Argumente erreicht. Daher kann die Bindung zur Laufzeit an Instanzmethoden erfolgen, die optionale Parameter akzeptieren.
- Schritt 2 wird durch Abrufen der isabgeschlossene ()-Eigenschaft ohne Argumente und durch das Versuch einer systeminternen Konvertierung in einen booleschen Wert erreicht.
- Schritt 3. a wird durch den Versuch `TryCast(awaiter, ICriticalNotifyCompletion)` erreicht. wenn dies fehlschlägt, `DirectCast(awaiter, INotifyCompletion)`.

Der Wiederaufnahme Delegat wurde in 3 übertragen. ein kann nur einmal aufgerufen werden. Wenn Sie mehrmals aufgerufen wird, ist das Verhalten nicht definiert.

