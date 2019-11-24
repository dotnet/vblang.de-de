---
ms.openlocfilehash: 8599ffc0ad3313f4e7e42da220c22ffa7a37e56b
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306181"
---
# <a name="general-concepts"></a>Allgemeine Konzepte

In diesem Kapitel werden einige Konzepte behandelt, die erforderlich sind, um die Semantik der Sprache von Microsoft Visual Basic zu verstehen. Viele der Konzepte sollten Visual Basic Programmierern oder C/C++ Programmierern vertraut sein, Ihre genauen Definitionen können sich jedoch unterscheiden.

## <a name="declarations"></a>Deklarationen

Ein Visual Basic Programm besteht aus benannten Entitäten. Diese Entitäten werden durch *Deklarationen* eingeführt und stellen die "Bedeutung" des Programms dar.

Auf oberster Ebene sind *Namespaces* Entitäten, die andere Entitäten organisieren, wie z. b. schsted Namespaces und Typen. *Typen* sind Entitäten, die Werte beschreiben und ausführbaren Code definieren. Typen können in Form von Typen und Typmembern enthalten. *Typmember* sind Konstanten, Variablen, Methoden, Operatoren, Eigenschaften, Ereignisse, Enumerationswerte und Konstruktoren.

Eine Entität, die andere Entitäten enthalten kann, definiert einen *Deklarations Bereich*. Entitäten werden entweder über Deklarationen oder Vererbung in einem Deklarations Raum eingeführt. der enthaltende Deklarations Bereich wird als *Deklarations Kontext*der Entität bezeichnet. Durch das Deklarieren einer Entität in einem Deklarations Bereich wird wiederum ein neuer Deklarations Bereich definiert, der weitere geschsted Entitäts Deklarationen enthalten kann. Folglich bilden die Deklarationen in einem Programm eine Hierarchie von Deklarations Räumen.

Außer im Fall von überladenen Typmembern ist es ungültig, dass Deklarationen identisch benannte Entitäten derselben Art in denselben Deklarations Kontext einführen. Außerdem darf ein Deklarations Raum nie verschiedene Arten von Entitäten mit demselben Namen enthalten. Beispielsweise darf ein Deklarations Raum nie eine Variable und eine Methode mit demselben Namen enthalten.

__Nebenbei.__ In anderen Sprachen kann es möglich sein, einen Deklarations Bereich zu erstellen, der verschiedene Arten von Entitäten mit demselben Namen enthält (z. b. Wenn bei der Sprache die Groß-/Kleinschreibung beachtet wird und verschiedene Deklarationen basierend auf der Groß-/Kleinschreibung In dieser Situation wird die am meisten zugängliche Entität an diesen Namen gebunden. Wenn mehr als ein Entitätstyp am meisten zugänglich ist, ist der Name mehrdeutig. `Public` ist besser zugänglich als `Protected Friend`, `Protected Friend` ist besser zugänglich als `Protected` oder `Friend`, und `Protected` oder `Friend` ist besser zugänglich als `Private`.

Der Deklarations Raum eines Namespaces ist "Open End", sodass zwei Namespace Deklarationen mit demselben voll qualifizierten Namen zum selben Deklarations Bereich beitragen. Im folgenden Beispiel tragen die beiden Namespace Deklarationen zum selben Deklarations Bereich bei, in diesem Fall werden zwei Klassen mit den voll qualifizierten Namen `Data.Customer` und `Data.Order:`

```vb
Namespace Data
    Class Customer
    End Class
End Namespace

Namespace Data
    Class Order
    End Class
End Namespace
```

Da die beiden Deklarationen zum gleichen Deklarations Bereich beitragen, tritt ein Kompilierzeitfehler auf, wenn jeder eine Deklaration einer Klasse mit dem gleichen Namen enthielt.

### <a name="overloading-and-signatures"></a>Überladen und Signaturen

Die einzige Möglichkeit, identisch benannte Entitäten derselben Art in einem Deklarations Bereich zu deklarieren, ist das *überladen*. Nur Methoden, Operatoren, Instanzkonstruktoren und Eigenschaften können überladen werden.

Überladene Typmember müssen eindeutige Signaturen aufweisen. Die Signatur eines Typmembers besteht aus der Anzahl der Typparameter und der Anzahl und den Typen der Parameter des Elements. Konvertierungs Operatoren enthalten außerdem den Rückgabetyp des Operators in der Signatur.

Die folgenden Elemente sind nicht Teil der Signatur eines Members und können daher nicht überladen werden:

* Modifiziererer in einen Typmember (z. b. `Shared` oder `Private`).

* Modifizierer in einen Parameter (z. b. `ByVal` oder `ByRef`).

* Die Namen der Parameter.

* Der Rückgabetyp einer Methode oder eines Operators (mit Ausnahme von Konvertierungs Operatoren) oder der Elementtyp einer Eigenschaft.

* Einschränkungen für einen Typparameter.

Das folgende Beispiel zeigt eine Reihe von überladenen Methoden Deklarationen zusammen mit ihren Signaturen. Diese Deklaration ist nicht gültig, da einige der Methoden Deklarationen identische Signaturen aufweisen.

```vb
Interface ITest
    Sub F1()                              ' Signature is ().
    Sub F2(x As Integer)                  ' Signature is (Integer).
    Sub F3(ByRef x As Integer)            ' Signature is (Integer).
    Sub F4(x As Integer, y As Integer)    ' Signature is (Integer, Integer).
    Function F5(s As String) As Integer   ' Signature is (String).
    Function F6(x As Integer) As Integer  ' Signature is (Integer).
    Sub F7(a() As String)                 ' Signature is (String()).
    Sub F8(ParamArray a() As String)      ' Signature is (String()).
    Sub F9(Of T)()                        ' Signature is !1().
    Sub F10(Of T, U)(x As T, y As U)      ' Signature is !2(!1, !2)
    Sub F11(Of U, T)(x As T, y As U)      ' Signature is !2(!2, !1)
    Sub F12(Of T)(x As T)                 ' Signature is !1(!1)
    Sub F13(Of T As IDisposable)(x As T)  ' Signature is !1(!1)
End Interface
```

Es ist zulässig, einen generischen Typ zu definieren, der auf der Grundlage der angegebenen Typargumente Member mit identischen Signaturen enthalten kann. Regeln zur Überladungs Auflösung werden verwendet, um zu versuchen, zwischen solchen über Ladungen zu unterscheiden. es kann jedoch Situationen geben, in denen es nicht möglich ist, die Mehrdeutigkeit zu beheben. Beispiel:

```vb
Class C(Of T)
    Sub F(x As Integer)
    End Sub

    Sub F(x As T)
    End Sub

    Sub G(Of U)(x As T, y As U)
    End Sub

    Sub G(Of U)(x As U, y As T)
    End Sub
End Class

Module Test
    Sub Main()
        Dim x As New C(Of Integer)
        x.F(10)                   ' Calls C(Of T).F(Integer)
        x.G(Of Integer)(10, 10)    ' Error: Can't choose between overloads
    End Sub
End Module
```

## <a name="scope"></a>Gültigkeitsbereich

Der Gültigkeits *Bereich* eines Entitäts namens ist der Satz aller Deklarations Räume, in denen es möglich ist, ohne Qualifizierung auf diesen Namen zu verweisen. Im Allgemeinen ist der Gültigkeitsbereich eines Entitäts namens der gesamte Deklarations Kontext. Allerdings kann die Deklaration einer Entität eine Entität mit dem gleichen Namen enthalten. In diesem Fall ist die *schattenentität mit der schattenentität*ausgeblendet oder ausgeblendet, und der Zugriff auf die Schatten Entität ist nur über die Qualifizierung möglich.

Die Schachtelung durch Schachtelung erfolgt in Namespaces oder Typen, die in Namespaces geschachtelt sind, in Typen, die in anderen Typen geschachtelt sind, und in den Texttypen. Shadothrough der Schachtelung von Deklarationen tritt immer implizit auf; Es ist keine explizite Syntax erforderlich.

Im folgenden Beispiel wird in der `F`-Methode die Instanzvariable `i` von der lokalen Variablen `i`schattiert, aber innerhalb der `G`-Methode verweist `i` weiterhin auf die-Instanzvariable.

```vb
Class Test
    Private i As Integer = 0

    Sub F()
        Dim i As Integer = 1
    End Sub

    Sub G()
        i = 1
    End Sub
End Class
```

Wenn ein Name in einem inneren Gültigkeitsbereich einen Namen in einem äußeren Gültigkeitsbereich verbirgt, werden alle überladenen Vorkommen dieses Namens ausgeblendet. Im folgenden Beispiel ruft der Aufruf `F(1)` die in `Inner` deklarierte `F` auf, da alle äußeren Vorkommen von `F` von der inneren Deklaration ausgeblendet werden. Aus demselben Grund ist der `F("Hello")` Fehler aufgetreten.

```vb
Class Outer
    Shared Sub F(i As Integer)
    End Sub

    Shared Sub F(s As String)
    End Sub

    Class Inner
        Shared Sub F(l As Long)
        End Sub

        Sub G()
            F(1) ' Invokes Outer.Inner.F.
            F("Hello") ' Error.
        End Sub
    End Class
End Class
```

## <a name="inheritance"></a>Vererbung

Eine Vererbungs Beziehung ist eine Vererbungs Beziehung, bei der ein Typ (der *abgeleitete* Typ) von einem anderen (dem *Basistyp* ) abgeleitet wird, sodass der Deklarations Bereich des abgeleiteten Typs implizit die zugänglichen Member des Typs "Non-Constructor" und die zugehörigen Typen des Basistyps enthält. Im folgenden Beispiel ist Class `A` die Basisklasse von `B`und `B` von `A`abgeleitet.

```vb
Class A
End Class

Class B
    Inherits A
End Class
```

Da in `A` nicht explizit eine Basisklasse angegeben ist, wird die zugehörige Basisklasse implizit `Object`.

Im folgenden finden Sie wichtige Aspekte der Vererbung:

* Vererbung ist transitiv. Wenn Typ *c* vom Typ *b*abgeleitet ist und Typ *b* vom Typ *a*abgeleitet ist, erbt Typ *c* die Typmember, die in Typ *b* deklariert sind, sowie die Typmember, die in Typ *a*deklariert wurden.

* Ein abgeleiteter Typ erweitert seinen Basistyp, kann ihn aber nicht eingrenzen. Ein abgeleiteter Typ kann neue Typmember hinzufügen, und er kann Schatten vererbte Typmember überschatten, aber er kann die Definition eines geerbten Typmembers nicht entfernen.

* Da eine Instanz eines Typs alle Typmember des Basistyps enthält, ist immer eine Konvertierung von einem abgeleiteten Typ in den Basistyp vorhanden.

* Alle Typen müssen über einen Basistyp verfügen, mit Ausnahme des Typs `Object`. Daher ist `Object` der ultimative Basistyp aller Typen, und alle Typen können in diese konvertiert werden.

* Zirkularität bei der Ableitung ist nicht zulässig. Das heißt, wenn ein Typ `B` von einem Typ `A`abgeleitet ist, ist dies ein Fehler für den Typ `A`, der direkt oder indirekt vom Typ `B`abgeleitet werden soll.

* Ein Typ kann nicht direkt oder indirekt von einem darin geschachtelten Typ abgeleitet werden.

Im folgenden Beispiel wird ein Kompilierzeitfehler erzeugt, da die Klassen zirkulär voneinander abhängig sind.

```vb
Class A
    Inherits B
End Class

Class B
    Inherits C
End Class

Class C
    Inherits A
End Class
```

Im folgenden Beispiel wird auch ein Kompilierzeitfehler erzeugt, da `B` indirekt von der zugehörigen `C` Klasse abgeleitet wird, die durch die Klasse `A`wird.

```vb
Class A
    Inherits B.C
End Class

Class B
    Inherits A

    Public Class C
    End Class 
End Class
```

Im nächsten Beispiel wird kein Fehler erzeugt, da die Klasse `A` nicht von der Klasse `B`abgeleitet ist.

```vb
Class A
    Class B
        Inherits A
    End Class 
End Class
```

### <a name="mustinherit-and-notinheritable-classes"></a>MustInherit-und notvererable-Klassen

Eine `MustInherit` Klasse ist ein unvollständiger Typ, der nur als Basistyp fungieren kann. Eine `MustInherit` Klasse kann nicht instanziiert werden, daher ist es ein Fehler, den `New` Operator für einen zu verwenden. Es ist zulässig, Variablen von `MustInherit` Klassen zu deklarieren. Diese Variablen können nur `Nothing` oder einem Wert zugewiesen werden, der von der Klasse `MustInherit` abgeleitet ist.

Wenn eine reguläre Klasse von einer `MustInherit` Klasse abgeleitet wird, muss die reguläre Klasse alle geerbten `MustOverride` Member überschreiben. Beispiel:

```vb
MustInherit Class A
    Public MustOverride Sub F()
End Class

MustInherit Class B
    Inherits A

    Public Sub G()
    End Sub
End Class 

Class C
    Inherits B

    Public Overrides Sub F()
    End Sub 
End Class
```

Mit der `MustInherit`-Klasse `A` wird eine `MustOverride`-Methode `F`eingeführt. In Class `B` wird eine zusätzliche Methode `G`eingeführt, es wird jedoch keine Implementierung von `F`bereitgestellt. Klassen `B` müssen daher auch als `MustInherit`deklariert werden. Class `C` überschreibt `F` und stellt eine tatsächliche Implementierung bereit. Da keine ausstehenden `MustOverride` Member in der Klasse `C`vorhanden sind, müssen Sie nicht `MustInherit`werden.

Eine `NotInheritable` Klasse ist eine Klasse, von der eine andere Klasse nicht abgeleitet werden kann. `NotInheritable` Klassen werden hauptsächlich verwendet, um eine unbeabsichtigte Ableitung zu verhindern.

In diesem Beispiel ist die Klasse `B` fehlerhaft, da Sie versucht, von der `NotInheritable` Klasse `A`abzuleiten. Eine Klasse kann nicht sowohl `MustInherit` als auch `NotInheritable`gekennzeichnet werden.

```vb
NotInheritable Class A
End Class

Class B
    ' Error, a class cannot derive from a NotInheritable class.
    Inherits A
End Class
```

### <a name="interfaces-and-multiple-inheritance"></a>Schnittstellen und mehrfache Vererbung

Im Gegensatz zu anderen Typen, die nur von einem einzelnen Basistyp abgeleitet werden, kann eine Schnittstelle von mehreren Basis Schnittstellen abgeleitet werden. Aus diesem Grund kann eine Schnittstelle ein identisch benanntes Typmember von unterschiedlichen Basis Schnittstellen erben. In einem solchen Fall ist der mehrfach geerbte Name in der abgeleiteten Schnittstelle nicht verfügbar, und der Verweis auf diese Typmember durch die abgeleitete Schnittstelle verursacht einen Kompilierzeitfehler, unabhängig von Signaturen oder überladen. Stattdessen müssen in Konflikt stehende Typmember durch einen Basis Schnittstellennamen referenziert werden.

Im folgenden Beispiel verursachen die ersten beiden-Anweisungen Kompilierzeitfehler, da die `Count` mit multiplizierter Vererbung in der-Schnittstelle `IListCounter`nicht verfügbar ist:

```vb
Interface IList
    Property Count() As Integer
End Interface

Interface ICounter
    Sub Count(i As Integer)
End Interface

Interface IListCounter
    Inherits IList
    Inherits ICounter 
End Interface 

Module Test
    Sub F(x As IListCounter)
        x.Count(1)                  ' Error, Count is not available.
        x.Count = 1                 ' Error, Count is not available.
        CType(x, IList).Count = 1   ' Ok, invokes IList.Count.
        CType(x, ICounter).Count(1) ' Ok, invokes ICounter.Count.
    End Sub 
End Module
```

Wie im Beispiel gezeigt, wird die Mehrdeutigkeit durch umwandeln `x` in den entsprechenden Basis Schnittstellentyp aufgelöst. Solche Umwandlungen haben keine Lauf Zeit Kosten. Sie bestehen lediglich aus dem Anzeigen der Instanz als weniger abgeleiteter Typ zur Kompilierzeit.

Wenn ein einzelner Typmember von derselben Basisschnittstelle durch mehrere Pfade geerbt wird, wird der Typmember so behandelt, als wäre er nur einmal geerbt worden. Anders ausgedrückt: die abgeleitete Schnittstelle enthält nur eine Instanz jedes Typmembers, der von einer bestimmten Basisschnittstelle geerbt wurde. Beispiel:

```vb
Interface IBase
    Sub F(i As Integer)
End Interface

Interface ILeft
    Inherits IBase
End Interface

Interface IRight
    Inherits IBase
End Interface

Interface IDerived
    Inherits ILeft, IRight
End Interface

Class Derived
    Implements IDerived

    ' Only have to implement F once.
    Sub F(i As Integer) Implements IDerived.F
    End Sub
End Class
```

Wenn ein Typmember-Name in einem Pfad über die Vererbungs Hierarchie schattiert wird, wird der Name in allen Pfaden schattiert. Im folgenden Beispiel wird der `IBase.F` Member vom `ILeft.F` Member schattiert, aber nicht in `IRight`schattiert:

```vb
Interface IBase
    Sub F(i As Integer)
End Interface 

Interface ILeft
    Inherits IBase

    Shadows Sub F(i As Integer)
End Interface 

Interface IRight
    Inherits IBase

    Sub G()
End Interface 

Interface IDerived
    Inherits ILeft, IRight 
End Interface 

Class Test
    Sub H(d As IDerived)
        d.F(1)                  ' Invokes ILeft.F.
        CType(d, IBase).F(1)    ' Invokes IBase.F.
        CType(d, ILeft).F(1)    ' Invokes ILeft.F.
        CType(d, IRight).F(1)   ' Invokes IBase.F.
    End Sub 
End Class
```

Der Aufruf `d.F(1)` wählt `ILeft.F`aus, obwohl `IBase.F` anscheinend nicht in den Zugriffs Pfad schattiert wird, der durch `IRight`führt. Da der Zugriffs Pfad von `IDerived` auf `ILeft`, um Schatten `IBase.F`zu `IBase`, wird das Element auch in den Zugriffs Pfad von `IDerived` auf `IRight` `IBase`.

### <a name="shadowing"></a>Shadowing

Ein abgeleiteter Typ überschattet den Namen eines geerbten Typmembers durch erneutes deklarieren. Beim shadolieren eines Namens werden die geerbten Typmember mit diesem Namen nicht entfernt. Es macht lediglich alle geerbten Typmember mit diesem Namen in der abgeleiteten Klasse nicht verfügbar. Die Shadowing-Deklaration kann ein beliebiger Entitätstyp sein.

Entitäten, die überladen werden können, können eine von zwei Formen der shadodown-Option auswählen. *Shadowingby Name* wird mit dem `Shadows`-Schlüsselwort angegeben. Eine Entität, die nach Name Shadowing durch den Namen in der Basisklasse verbirgt, einschließlich aller über Ladungen. *Shadowingby Name und Signature werden* mithilfe des `Overloads`-Schlüssel Worts angegeben. Eine Entität, die durch den Namen und die Signatur Shadowing durch diesen Namen mit derselben Signatur wie die Entität verbirgt. Beispiel:

```vb
Class Base
    Sub F()
    End Sub

    Sub F(i As Integer)
    End Sub

    Sub G()
    End Sub

    Sub G(i As Integer)
    End Sub
End Class

Class Derived
    Inherits Base

    ' Only hides F(Integer).
    Overloads Sub F(i As Integer)
    End Sub

    ' Hides G() and G(Integer).
    Shadows Sub G(i As Integer)
    End Sub
End Class

Module Test
    Sub Main()
        Dim x As New Derived()

        x.F() ' Calls Base.F().
        x.G() ' Error: Missing parameter.
    End Sub
End Module
```

Beim shadoochen einer Methode mit einem `ParamArray`-Argument nach Name und Signatur werden nur die einzelnen Signaturen ausgeblendet, nicht alle möglichen erweiterten Signaturen. Dies gilt auch, wenn die Signatur der Shadowing-Methode mit der nicht erweiterten Signatur der Shadowing-Methode übereinstimmt. Im Beispiel unten geschieht Folgendes:

```vb
Class Base
    Sub F(ParamArray x() As Integer)
        Console.WriteLine("Base")
    End Sub
End Class

Class Derived 
    Inherits Base

    Overloads Sub F(x() As Integer)
        Console.WriteLine("Derived")
    End Sub
End Class

Module Test
    Sub Main
        Dim d As New Derived()
        d.F(10)
    End Sub
End Module
```

gibt `Base`aus, auch wenn `Derived.F` die gleiche Signatur aufweist wie die nicht erweiterte Form von `Base.F`.

Umgekehrt gibt eine Methode mit einem `ParamArray`-Argument nur Methoden mit derselben Signatur, nicht alle möglichen erweiterten Signaturen. Im Beispiel unten geschieht Folgendes:

```vb
Class Base
    Sub F(x As Integer)
        Console.WriteLine("Base")
    End Sub
End Class

Class Derived
    Inherits Base

    Overloads Sub F(ParamArray x() As Integer)
        Console.WriteLine("Derived")
    End Sub
End Class

Module Test
    Sub Main()
        Dim d As New Derived()
        d.F(10)
    End Sub
End Module
```

gibt `Base`aus, auch wenn `Derived.F` ein erweitertes Formular hat, das über dieselbe Signatur wie `Base.F`verfügt.

Eine shadowingmethode oder Eigenschaft, die weder `Shadows` noch `Overloads` angibt, `Overloads` `Shadows`, wenn die Methode oder Eigenschaft als `Overrides`deklariert ist, andernfalls. Wenn ein Element eines Satzes von überladenen Entitäten die `Shadows` oder `Overloads`-Schlüsselwort angibt, müssen alle diese angeben. Die Schlüsselwörter "`Shadows`" und "`Overloads`" können nicht gleichzeitig angegeben werden. Weder `Shadows` noch `Overloads` können in einem Standardmodul angegeben werden. Member in einem Standardmodul schattenelemente, die von `Object`geerbt wurden, implizit.

Es ist zulässig, den Namen eines Typmembers zu schattieren, der durch die Schnittstellen Vererbung (und somit nicht verfügbar ist) multipliziert-geerbt wurde, sodass der Name in der abgeleiteten Schnittstelle verfügbar ist.

Beispiel:

```vb
Interface ILeft
    Sub F()
End Interface

Interface IRight
    Sub F()
End Interface

Interface ILeftRight
    Inherits ILeft, IRight

    Shadows Sub F()
End Interface

Module Test
    Sub G(i As ILeftRight)
        i.F() ' Calls ILeftRight.F.
        CType(i, ILeft).F() ' Calls ILeft.F.
        CType(i, IRight).F() ' Calls IRight.F.
    End Sub
End Module
```

Da Methoden zum überschatten geerbte Methoden zugelassen werden, kann eine Klasse mehrere `Overridable` Methoden mit der gleichen Signatur enthalten. Dies stellt kein mehrdeutigkeitsproblem dar, da nur die am meisten abgeleitete Methode sichtbar ist. Im folgenden Beispiel enthalten die Klassen `C` und `D` zwei `Overridable` Methoden mit der gleichen Signatur:

```vb
Class A
    Public Overridable Sub F()
        Console.WriteLine("A.F")
    End Sub 
End Class 

Class B
    Inherits A

    Public Overrides Sub F()
        Console.WriteLine("B.F")
    End Sub 
End Class 

Class C
    Inherits B

    Public Shadows Overridable Sub F()
        Console.WriteLine("C.F")
    End Sub 
End Class 

Class D
    Inherits C

    Public Overrides Sub F()
        Console.WriteLine("D.F")
    End Sub 
End Class 

Module Test
    Sub Main()
        Dim d As New D()
        Dim a As A = d
        Dim b As B = d
        Dim c As C = d
        a.F()
        b.F()
        c.F()
        d.F()
    End Sub 
End Module
```

Hier gibt es zwei `Overridable` Methoden: eine Klasse, die von Class `A` eingeführt wurde, und die von Class `C`eingeführte. Die von Class eingeführte Methode `C` blendet die Methode aus, die von der Klasse `A`geerbt wurde. Folglich überschreibt die `Overrides` Deklaration in Class `D` die von Class `C`eingeführte Methode, und es ist nicht möglich, dass Klassen `D` die Methode überschreiben, die von Class `A`eingeführt wurde. Das Beispiel erzeugt die Ausgabe:

```console
B.F
B.F
D.F
D.F
```

Es ist möglich, die ausgeblendete `Overridable` Methode aufzurufen, indem Sie auf eine Instanz der-Klasse zugreifen `D` durch einen weniger abgeleiteten Typ, in dem die Methode nicht ausgeblendet ist.

Es ist nicht zulässig, eine `MustOverride` Methode zu schattieren, da dies in den meisten Fällen dazu führen würde, dass die Klasse unbrauchbar wird. Beispiel:

```vb
MustInherit Class Base
    Public MustOverride Sub F()
End Class

MustInherit Class Derived
    Inherits Base

    Public Shadows Sub F()
    End Sub
End Class

Class MoreDerived
    Inherits Derived

    ' Error: MustOverride method Base.F is not overridden.
End Class
```

In diesem Fall ist die Klasse `MoreDerived` erforderlich, um die `MustOverride`-Methode `Base.F`zu überschreiben, aber da die Klasse `Derived` Schatten `Base.F`, ist dies nicht möglich. Es gibt keine Möglichkeit, einen gültigen Nachfolger `Derived`zu deklarieren.

Im Gegensatz zum shadoochen eines Namens aus einem äußeren Gültigkeitsbereich bewirkt das shadoochen eines barrierefreien namens aus einem geerbten Bereich, dass eine Warnung ausgegeben wird, wie im folgenden Beispiel gezeigt:

```vb
Class Base
    Public Sub F()
    End Sub

    Private Sub G()
    End Sub 
End Class

Class Derived
    Inherits Base

    Public Sub F() ' Warning: shadowing an inherited name.
    End Sub

    Public Sub G() ' No warning, Base.G is not accessible here.
    End Sub
End Class
```

Die Deklaration der-Methode `F` in der-Klasse `Derived` bewirkt, dass eine Warnung gemeldet wird. Ein shadodown eines geerbten Namens ist kein Fehler, da dies die getrennte Weiterentwicklung von Basisklassen ausschließen würde. Die obige Situation könnte z. b. der Fall sein, weil eine neuere Version der Klasse `Base` eine Methode `F` eingeführt hat, die in einer früheren Version der Klasse nicht vorhanden war. Wäre die oben aufgeführte Situation ein Fehler gewesen, kann jede Änderung an einer Basisklasse in einer separaten Klassenbibliothek möglicherweise dazu führen, *dass* abgeleitete Klassen ungültig werden.

Die Warnung, die durch shadoassen eines geerbten Namens verursacht wurde, kann durch die Verwendung des `Shadows` oder `Overloads` Modifizierers gelöscht werden:

```vb
Class Base
    Public Sub F()
    End Sub 
End Class 

Class Derived
    Inherits Base

    Public Shadows Sub F() 'OK.
    End Sub
End Class
```

Der `Shadows` Modifizierer gibt die Absicht an, den geerbten Member zu schattieren. Es ist kein Fehler, den `Shadows` oder `Overloads` Modifizierer anzugeben, wenn kein Typmember Name zum Schatten vorhanden ist.

Eine Deklaration eines neuen Members schattent einen geerbten Member nur innerhalb des Bereichs des neuen Members, wie im folgenden Beispiel gezeigt:

```vb
Class Base
    Public Shared Sub F()
    End Sub 
End Class 

Class Derived
    Inherits Base

    Private Shared Shadows Sub F() ' Shadows Base.F in class Derived only.
    End Sub 
End Class 

Class MoreDerived
    Inherits Derived

    Shared Sub G()
        F() ' Invokes Base.F.
    End Sub 
End Class
```

Im obigen Beispiel wird die Deklaration der-Methode `F` in der-Klasse `Derived` Shadowing für die-Methode `F`, die von Class `Base`geerbt wurde, aber da die neue Methode `F` in der-Klasse `Derived` Zugriff hat, wird deren Bereich nicht auf Class `Private` erweitert.`MoreDerived` Daher ist der Aufruf `F()` in `MoreDerived.G` gültig und wird `Base.F`aufrufen. Im Fall von überladenen Typmembern wird der gesamte Satz von überladenen Typmembern so behandelt, als ob Sie alle den meisten Berechtigungen für shadodown haben.

```vb
Class Base
    Public Sub F()
    End Sub
End Class

Class Derived
    Inherits Base

    Private Shadows Sub F()
    End Sub

    Public Shadows Sub F(i As Integer)
    End Sub
End Class

Class MoreDerived
    Inherits Derived

    Public Sub G()
        F()   ' Error. No accessible member with this signature.
    End Sub
End Class
```

Obwohl die Deklaration von `F()` in `Derived` mit `Private` Access deklariert ist, wird in diesem Beispiel die überladene `F(Integer)` mit `Public` Access deklariert. Daher wird der Name `F` in `Derived` so behandelt, als ob er `Public`wäre, sodass beide Methoden Schatten `F` in `Base`werden.

## <a name="implementation"></a>Implementierung

Eine *Implementierungs* Beziehung ist vorhanden, wenn ein Typ deklariert, dass eine Schnittstelle implementiert wird und der Typ alle Typmember der Schnittstelle implementiert. Ein Typ, der eine bestimmte Schnittstelle implementiert, kann in diese Schnittstelle konvertiert werden. Schnittstellen können nicht instanziiert werden, aber es ist zulässig, Variablen der Schnittstellen zu deklarieren. solchen Variablen kann nur ein Wert zugewiesen werden, der eine Klasse ist, die die-Schnittstelle implementiert. Beispiel:

```vb
Interface ITestable
    Function Test(value As Byte) As Boolean
End Interface

Class TestableClass
    Implements ITestable

    Function Test(value As Byte) As Boolean Implements ITestable.Test
        Return value > 128
    End Function
End Class

Module Test
    Sub F()
        Dim x As ITestable = New TestableClass
        Dim b As Boolean

        b = x.Test(34)
    End Sub
End Module
```

Ein Typ, der eine Schnittstelle mit multiplizierten Typmembern implementiert, muss diese Methoden dennoch implementieren, auch wenn nicht direkt von der implementierten Schnittstelle aus auf Sie zugegriffen werden kann. Beispiel:

```vb
Interface ILeft
    Sub Test()
End Interface

Interface IRight
    Sub Test()
End Interface

Interface ILeftRight
    Inherits ILeft, IRight
End Interface

Class LeftRight
    Implements ILeftRight

    ' Has to reference ILeft explicitly.
    Sub TestLeft() Implements ILeft.Test
    End Sub

    ' Has to reference IRight explicitly.
    Sub TestRight() Implements IRight.Test
    End Sub

    ' Error: Test is not available in ILeftRight.
    Sub TestLeftRight() Implements ILeftRight.Test
    End Sub
End Class
```

Auch `MustInherit` Klassen müssen Implementierungen aller Member der implementierten Schnittstellen bereitstellen. Allerdings können Sie die Implementierung dieser Methoden verzögern, indem Sie Sie als `MustOverride`deklarieren. Beispiel:

```vb
Interface ITest
    Sub Test1()
    Sub Test2()
End Interface

MustInherit Class TestBase
    Implements ITest

    ' Provides an implementation.
    Sub Test1() Implements ITest.Test1
    End Sub

    ' Defers implementation.
    MustOverride Sub Test2() Implements ITest.Test2
End Class

Class TestDerived
    Inherits TestBase

    ' Have to implement MustOverride method.
    Overrides Sub Test2()
    End Sub
End Class
```

Ein Typ kann eine Schnittstelle, die vom Basistyp implementiert wird, erneut implementieren. Um die-Schnittstelle erneut zu implementieren, muss der Typ explizit angeben, dass die-Schnittstelle implementiert wird. Ein Typ, der eine Schnittstelle erneut implementiert, kann die Implementierung nur einiger, aber nicht aller Member der Schnittstelle wieder implementieren. alle Member, die nicht erneut implementiert werden, verwenden weiterhin die Implementierung des Basistyps. Beispiel:

```vb
Class TestBase
    Implements ITest

    Sub Test1() Implements ITest.Test1
        Console.WriteLine("TestBase.Test1")
    End Sub

    Sub Test2() Implements ITest.Test2
        Console.WriteLine("TestBase.Test2")
    End Sub
End Class

Class TestDerived
    Inherits TestBase
    Implements ITest  ' Required to re-implement

    Sub DerivedTest1() Implements ITest.Test1
        Console.WriteLine("TestDerived.DerivedTest1")
    End Sub
End Class

Module Test
    Sub Main()
        Dim Test As ITest = New TestDerived()
        Test.Test1()
        Test.Test2()
    End Sub
End Module
```

In diesem Beispiel wird Folgendes gedruckt:

```console
TestDerived.DerivedTest1
TestBase.Test2
```

Wenn ein abgeleiteter Typ eine Schnittstelle implementiert, deren Basis Schnittstellen von den Basis Typen des abgeleiteten Typs implementiert werden, kann der abgeleitete Typ festlegen, dass nur die Typmember der Schnittstelle implementiert werden, die von den Basis Typen nicht bereits implementiert wurden. Beispiel:

```vb
Interface IBase
    Sub Base()
End Interface

Interface IDerived
    Inherits IBase

    Sub Derived()
End Interface

Class Base
    Implements IBase

    Public Sub Base() Implements IBase.Base
    End Sub
End Class

Class Derived
    Inherits Base
    Implements IDerived

    ' Required: IDerived.Derived not implemented by Base.
    Public Sub Derived() Implements IDerived.Derived
    End Sub
End Class
```

Eine Schnittstellen Methode kann auch mithilfe einer über schreibbaren Methode in einem Basistyp implementiert werden. In diesem Fall kann ein abgeleiteter Typ auch die über schreibbare Methode überschreiben und die Implementierung der Schnittstelle ändern. Beispiel:

```vb
Class Base
    Implements ITest

    Public Sub Test1() Implements ITest.Test1
        Console.WriteLine("TestBase.Test1")
    End Sub

    Public Overridable Sub Test2() Implements ITest.Test2
        Console.WriteLine("TestBase.Test2")
    End Sub
End Class

Class Derived
    Inherits Base

    ' Overrides base implementation.
    Public Overrides Sub Test2()
        Console.WriteLine("TestDerived.Test2")
    End Sub
End Class
```

### <a name="implementing-methods"></a>Implementieren von Methoden

Ein Typ *implementiert* einen Typmember einer implementierten Schnittstelle durch Bereitstellen einer Methode mit einer `Implements`-Klausel. Die beiden Typmember müssen die gleiche Anzahl von Parametern aufweisen, alle Typen und Modifizierer der Parameter müssen übereinstimmen, einschließlich des Standardwerts optionaler Parameter, der Rückgabetyp muss übereinstimmen, und alle Einschränkungen für Methoden Parameter müssen übereinstimmen. Beispiel:

```vb
Interface ITest
    Sub F(ByRef x As Integer)
    Sub G(Optional y As Integer = 20)
    Sub H(Paramarray z() As Integer)
End Interface

Class Test
    Implements ITest

    ' Error: ByRef/ByVal mismatch.
    Sub F(x As Integer) Implements ITest.F
    End Sub

    ' Error: Defaults do not match.
    Sub G(Optional y As Integer = 10) Implements ITest.G
    End Sub

    ' Error: Paramarray does not match.
    Sub H(z() As Integer) Implements ITest.H
    End Sub
End Class
```

Eine einzelne Methode kann eine beliebige Anzahl von Schnittstellentypmembern implementieren, wenn Sie alle die oben aufgeführten Kriterien erfüllen. Beispiel:

```vb
Interface ITest
    Sub F(i As Integer)
    Sub G(i As Integer)
End Interface

Class Test

    Implements ITest

    Sub F(i As Integer) Implements ITest.F, ITest.G
    End Sub
End Class
```

Wenn eine Methode in einer generischen Schnittstelle implementiert wird, muss die implementierende Methode die Typargumente bereitstellen, die den Typparametern der Schnittstelle entsprechen. Beispiel:

```vb
Interface I1(Of U, V) 
    Sub M(x As U, y As List(Of V)) 
End Interface

Class C1(Of W, X)
    Implements I1(Of W, X)

    ' W corresponds to U and X corresponds to V
    Public Sub M(x As W, y As List(Of X)) Implements I1(Of W, X).M
    End Sub 
End Class

Class C2
    Implements I1(Of String, Integer)

    ' String corresponds to U and Integer corresponds to V
    Public Sub M(x As String, y As List(Of Integer)) _
        Implements I1(Of String, Integer).M
    End Sub
End Class
```

Beachten Sie, dass es möglich ist, dass eine generische Schnittstelle für einen Satz von Typargumenten möglicherweise nicht implementierbar ist.

```vb
Interface I1(Of T, U)
    Sub S1(x As T)
    Sub S1(y As U)
End Interface

Class C1
    ' Unable to implement because I1.S1 has two identical signatures
    Implements I1(Of Integer, Integer)
End Class
```

## <a name="polymorphism"></a>Polymorphismus

*Polymorphismus* bietet die Möglichkeit, die Implementierung einer Methode oder Eigenschaft zu verändern. Bei Polymorphie kann dieselbe Methode oder Eigenschaft je nach Lauf Zeittyp der Instanz, von der Sie aufgerufen wird, verschiedene Aktionen ausführen. Methoden oder Eigenschaften, die polymorph sind, werden als *über schreibbare*bezeichnet. Im Gegensatz dazu ist die Implementierung einer nicht über schreibbaren Methode oder Eigenschaft invariant. die Implementierung ist identisch, unabhängig davon, ob die Methode oder Eigenschaft für eine Instanz der Klasse, in der Sie deklariert ist, oder für eine Instanz einer abgeleiteten Klasse aufgerufen wird. Wenn eine nicht über schreibbare Methode oder Eigenschaft aufgerufen wird, ist der Kompilier Zeittyp der Instanz der bestimmende Faktor. Beispiel:

```vb
Class Base
    Public Overridable Property X() As Integer
        Get
        End Get

        Set
        End Set
    End Property
End Class

Class Derived
    Inherits Base

    Public Overrides Property X() As Integer
        Get
        End Get

        Set
        End Set
    End Property
End Class

Module Test
    Sub F()
        Dim Z As Base

        Z = New Base()
        Z.X = 10            ' Calls Base.X
        Z = New Derived()
        Z.X = 10            ' Calls Derived.X
    End Sub
End Module
```

Eine über schreibbare Methode kann auch `MustOverride`sein, was bedeutet, dass Sie keinen Methoden Text bereitstellt und überschrieben werden muss. `MustOverride` Methoden sind nur in `MustInherit` Klassen zulässig.

Im folgenden Beispiel definiert die-Klasse `Shape` den abstrakten Begriff eines geometrischen Shape-Objekts, das sich selbst zeichnen kann:

```vb
MustInherit Public Class Shape
    Public MustOverride Sub Paint(g As Graphics, r As Rectangle)
End Class 

Public Class Ellipse
    Inherits Shape

    Public Overrides Sub Paint(g As Graphics, r As Rectangle)
        g.drawEllipse(r)
    End Sub 
End Class 

Public Class Box
    Inherits Shape

    Public Overrides Sub Paint(g As Graphics, r As Rectangle)
        g.drawRect(r)
    End Sub 
End Class
```

Die `Paint`-Methode ist `MustOverride`, da keine sinnvolle Standard Implementierung vorhanden ist. Die Klassen `Ellipse` und `Box` sind konkrete `Shape` Implementierungen. Da diese Klassen nicht `MustInherit`werden, müssen Sie die `Paint`-Methode überschreiben und eine tatsächliche Implementierung bereitstellen.

Es ist ein Fehler für einen Basis Zugriff, der auf eine `MustOverride` Methode verweist, wie im folgenden Beispiel veranschaulicht:

```vb
MustInherit Class A
    Public MustOverride Sub F()
End Class

Class B
    Inherits A

    Public Overrides Sub F()
        MyBase.F() ' Error, MyBase.F is MustOverride.
    End Sub 
End Class
```

Für den `MyBase.F()` Aufruf wird ein Fehler gemeldet, weil er auf eine `MustOverride` Methode verweist.

### <a name="overriding-methods"></a>Überschreiben von Methoden

Ein Typ kann eine geerbte über schreibbare Methode durch Deklarieren einer Methode mit dem gleichen Namen und der gleichen Signatur über *Schreiben* und die Deklaration mit dem `Overrides` Modifizierer markieren. Es gibt zusätzliche Anforderungen für das Überschreiben von Methoden, die unten aufgeführt sind. Während eine `Overridable` Methoden Deklaration eine neue Methode einführt, ersetzt eine `Overrides` Methoden Deklaration die geerbte Implementierung der-Methode.

Eine über schreibende Methode kann `NotOverridable`deklariert werden. Dadurch wird verhindert, dass die Methode in abgeleiteten Typen weiter überschrieben wird. In der Tat werden `NotOverridable` Methoden in allen weiteren abgeleiteten Klassen nicht über schreibbar.

Betrachten Sie das folgende Beispiel:

```vb
Class A
    Public Overridable Sub F()
        Console.WriteLine("A.F")
    End Sub

    Public Overridable Sub G()
        Console.WriteLine("A.G")
    End Sub
End Class

Class B
    Inherits A

    Public Overrides NotOverridable Sub F()
        Console.WriteLine("B.F")
    End Sub

    Public Overrides Sub G()
        Console.WriteLine("B.G")
    End Sub
End Class

Class C
    Inherits B

    Public Overrides Sub G()
        Console.WriteLine("C.G")
    End Sub
End Class
```

Im Beispiel stellt Class `B` zwei `Overrides` Methoden bereit: eine Methode `F`, die über den `NotOverridable`-Modifizierer und eine Methode `G` verfügt. Die Verwendung des-Modifizierers `NotOverridable` verhindert, dass die-Klasse `C` weitere Methoden `F`überschreibt.

Eine über schreibende Methode kann auch als `MustOverride`deklariert werden, auch wenn die Methode, die Sie überschreibt, nicht als `MustOverride`deklariert ist. Dies erfordert, dass die enthaltende Klasse `MustInherit` deklariert wird und dass alle weiteren abgeleiteten Klassen, die nicht als `MustInherit` deklariert sind, die-Methode überschreiben müssen. Beispiel:

```vb
Class A
    Public Overridable Sub F()
        Console.WriteLine("A.F")
    End Sub
End Class

MustInherit Class B
    Inherits A

    Public Overrides MustOverride Sub F()
End Class
```

Im Beispiel überschreibt Class `B` `A.F` mit einer `MustOverride`-Methode. Dies bedeutet, dass alle Klassen, die von `B` abgeleitet werden, `F`außer Kraft setzen müssen, es sei denn, Sie werden ebenfalls als `MustInherit` deklariert.

Ein Kompilierzeitfehler tritt auf, es sei denn, für eine über schreibende Methode gilt Folgendes:

* Der Deklarations Kontext enthält eine einzelne barrierefreie Methode mit der gleichen Signatur und dem gleichen Rückgabetyp (sofern vorhanden) als über schreibende Methode.
* Die geerbte Methode, die überschrieben wird, ist über schreibbar. Anders ausgedrückt: die geerbte Methode, die überschrieben wird, ist nicht `Shared` oder `NotOverridable`.
* Die Zugriffs Domäne der deklarierten Methode ist identisch mit der Zugriffs Domäne der geerbten Methode, die überschrieben wird. Es gibt eine Ausnahme: eine `Protected Friend` Methode muss von einer `Protected`-Methode überschrieben werden, wenn die andere Methode in einer anderen Assembly enthalten ist, auf die die über schreibende Methode nicht `Friend` Zugriff hat.
* Die Parameter der über schreibenden Methode entsprechen den Parametern der überschriebenen Methode in Bezug auf die Verwendung der Modifizierers `ByVal`, `ByRef`, `ParamArray,` und `Optional`, einschließlich der Werte, die für optionale Parameter bereitgestellt werden.
* Die Typparameter der über schreibenden Methode entsprechen den Typparametern der überschriebenen Methode in Bezug auf Typeinschränkungen.

Beim Überschreiben einer Methode in einem generischen Basistyp muss die über schreibende Methode die Typargumente bereitstellen, die den Basistyp Parametern entsprechen. Beispiel:

```vb
Class Base(Of U, V) 
    Public Overridable Sub M(x As U, y As List(Of V)) 
    End Sub
End Class

Class Derived(Of W, X)
    Inherits Base(Of W, X)

    ' W corresponds to U and X corresponds to V
    Public Overrides Sub M(x As W, y As List(Of X)) 
    End Sub 
End Class

Class MoreDerived
    Inherits Derived(Of String, Integer)

    ' String corresponds to U and Integer corresponds to V
    Public Overrides Sub M(x As String, y As List(Of Integer))
    End Sub
End Class
```

Beachten Sie, dass eine über schreibbare Methode in einer generischen Klasse möglicherweise für einige Sätze von Typargumenten nicht überschrieben werden kann. Wenn die Methode als `MustOverride`deklariert ist, bedeutet dies, dass einige Vererbungs Ketten möglicherweise nicht möglich sind. Beispiel:

```vb
MustInherit Class Base(Of T, U)
    Public MustOverride Sub S1(x As T)
    Public MustOverride Sub S1(y As U)
End Class

Class Derived
    Inherits Base(Of Integer, Integer)

    ' Error: Can't override both S1's at once
    Public Overrides Sub S1(x As Integer)
    End Sub
End Class
```

Eine Überschreibungs Deklaration kann mithilfe eines Basis Zugriffs auf die überschriebene Basis Methode zugreifen, wie im folgenden Beispiel gezeigt:

```vb
Class Base
    Private x As Integer

    Public Overridable Sub PrintVariables()
        Console.WriteLine("x = " & x)
    End Sub
End Class

Class Derived
    Inherits Base

    Private y As Integer

    Public Overrides Sub PrintVariables()
        MyBase.PrintVariables()
        Console.WriteLine("y = " & y)
    End Sub
End Class
```

Im Beispiel ruft der Aufruf von `MyBase.PrintVariables()` in Class `Derived` die in Class `Base`deklarierte `PrintVariables` Methode auf. Durch einen Basis Zugriff wird der über schreibbare Aufruf Mechanismus deaktiviert und die Basis Methode einfach als nicht über schreibbare Methode behandelt. Hätte der Aufruf in `Derived` geschrieben `CType(Me, Base).PrintVariables()`, würde er die in `Derived`deklarierte `PrintVariables` Methode rekursiv aufrufen, nicht die in `Base`deklarierte Methode.

Nur wenn Sie einen `Overrides` Modifizierer enthält, kann eine Methode eine andere Methode überschreiben. In allen anderen Fällen Shadowing eine Methode, die die gleiche Signatur wie eine geerbte Methode hat, einfach die geerbte Methode, wie im folgenden Beispiel gezeigt:

```vb
Class Base
    Public Overridable Sub F()
    End Sub
End Class

Class Derived
    Inherits Base

    Public Overridable Sub F() ' Warning, shadowing inherited F().
    End Sub
End Class
```

In diesem Beispiel enthält die-Methode `F` in Class `Derived` keinen `Overrides`-Modifizierer und überschreibt daher nicht die-Methode `F` in der Klasse `Base`. Die Methode `F` in der Klasse `Derived` Shadowing der Methode in der Klasse `Base`, und es wird eine Warnung ausgegeben, da die Deklaration keinen `Shadows` oder `Overloads` Modifizierer enthält.

Im folgenden Beispiel wird die Methode `F` in der-Klasse `Derived` Shadowing der über schreibbaren-Methode `F` von Class `Base`geerbt:

```vb
Class Base
    Public Overridable Sub F()
    End Sub
End Class

Class Derived
    Inherits Base

    Private Shadows Sub F() ' Shadows Base.F within Derived.
    End Sub
End Class

Class MoreDerived
    Inherits Derived

    Public Overrides Sub F() ' Ok, overrides Base.F.
    End Sub
End Class
```

Da die neue Methode `F` in der Klasse `Derived` über `Private` Zugriff verfügt, umfasst ihr Bereich nur den Klassen Text `Derived` und wird nicht auf die Klasse `MoreDerived`erweitert. Die Deklaration der-Methode `F` in der-Klasse `MoreDerived` kann daher die-`F` Methode überschreiben, die von der-Klasse `Base`geerbt wurde.

Wenn eine `Overridable` Methode aufgerufen wird, wird die am meisten abgeleitete Implementierung der Instanzmethode basierend auf dem Typ der Instanz aufgerufen, unabhängig davon, ob der Aufruf an die Methode in der Basisklasse oder der abgeleiteten Klasse erfolgt. Die am weitesten abgeleitete Implementierung einer `Overridable` Methode `M` in Bezug auf eine Klasse `R` wie folgt bestimmt werden:

* Wenn `R` die Introducing `Overridable` Deklaration von `M`enthält, handelt es sich hierbei um die am meisten abgeleitete Implementierung `M`.

* Wenn `R` eine außer Kraft Setzung von `M`enthält, handelt es sich hierbei um die am meisten abgeleitete Implementierung von `M`.

* Andernfalls ist die am meisten abgeleitete Implementierung von `M` identisch mit der der direkten Basisklasse von `R`.

## <a name="accessibility"></a>Zugriff

Eine Deklaration gibt den Zugriff auf die Entität an *, die Sie* deklariert. Der Zugriff einer Entität ändert nicht den Gültigkeitsbereich eines Entitäts namens. Die *Barrierefreiheits Domäne* einer Deklaration ist der Satz aller Deklarations Bereiche, in denen die deklarierte Entität zugänglich ist.

Die fünf Zugriffs Typen sind `Public`, `Protected`, `Friend`, `Protected Friend`und `Private`. `Public` ist der sicherste Zugriffstyp, und die vier anderen Typen sind alle Teilmengen von `Public`. Der geringstmöglichen Zugriffstyp ist `Private`, und die vier anderen Zugriffs Typen sind alle übergeordnete `Private`.

```antlr
AccessModifier
    : 'Public'
    | 'Protected'
    | 'Friend'
    | 'Private'
    | 'Protected' 'Friend'
    ;
```

Der Zugriffstyp für eine Deklaration wird über einen optionalen Zugriffsmodifizierer angegeben, der `Public`, `Protected`, `Friend`, `Private`oder die Kombination aus `Protected` und `Friend`sein kann. Wenn kein Zugriffsmodifizierer angegeben ist, hängt der Standard Zugriffstyp vom Deklarations Kontext ab. die zulässigen Zugriffs Typen sind auch vom Deklarations Kontext abhängig.

* Mit dem `Public`-Modifizierer deklarierte Entitäten haben `Public` Zugriff. Es gibt keine Einschränkungen hinsichtlich der Verwendung von `Public` Entitäten.

* Mit dem `Protected`-Modifizierer deklarierte Entitäten haben `Protected` Zugriff. `Protected` Zugriff kann nur für Member von Klassen angegeben werden (sowohl reguläre Typmember als auch Unterklassen) oder für `Overridable` Member von Standardmodulen und-Strukturen (die definitionsgemäß von `System.Object` oder `System.ValueType`geerbt werden müssen). Auf einen `Protected` Member kann von einer abgeleiteten Klasse zugegriffen werden, vorausgesetzt, der Member ist kein Instanzmember, oder der Zugriff erfolgt über eine Instanz der abgeleiteten Klasse. `Protected` Zugriff ist keine übergeordnete `Friend` Zugriffs.

* Mit dem `Friend`-Modifizierer deklarierte Entitäten haben `Friend` Zugriff. Auf eine Entität mit `Friend` Access kann nur innerhalb des Programms zugegriffen werden, das die Entitäts Deklaration oder Assemblys enthält, die über das `System.Runtime.CompilerServices.InternalsVisibleToAttribute`-Attribut `Friend` Zugriff erhalten haben.

* Entitäten, die mit den `Protected Friend` modifizierermodifizierer deklariert wurden, haben die Vereinigung `Protected` und `Friend` Zugriffs

* Mit dem `Private`-Modifizierer deklarierte Entitäten haben `Private` Zugriff. Auf eine `Private` Entität kann nur innerhalb Ihres Deklarations Kontexts, einschließlich geschachtelter Entitäten, zugegriffen werden.

Die Barrierefreiheit in einer-Deklaration hängt nicht von der Barrierefreiheit des Deklarations Kontexts ab. Beispielsweise kann ein Typ, der mit `Private` Access deklariert wurde, einen Typmember mit `Public` Access enthalten.

Der folgende Code veranschaulicht verschiedene Barrierefreiheits Domänen:

```vb
Public Class A
    Public Shared X As Integer
    Friend Shared Y As Integer
    Private Shared Z As Integer
End Class

Friend Class B
    Public Shared X As Integer
    Friend Shared Y As Integer
    Private Shared Z As Integer

    Public Class C
        Public Shared X As Integer
        Friend Shared Y As Integer
        Private Shared Z As Integer
    End Class

    Private Class D
        Public Shared X As Integer
        Friend Shared Y As Integer
        Private Shared Z As Integer
    End Class
End Class
```

Die Klassen und Member in diesem Beispiel verfügen über die folgenden Barrierefreiheits Domänen:

* Die Zugriffs Domäne von `A` und `A.X` ist unbegrenzt.

* Die Zugriffs Domäne `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`und `B.C.Y` ist das enthaltende Programm.

* Die Zugriffs Domäne `A.Z` ist `A.`

* Die Zugriffs Domäne `B.Z`, `B.D`, `B.D.X`und `B.D.Y` ist `B`, einschließlich `B.C` und `B.D`.

* Die Zugriffs Domäne `B.C.Z` ist `B.C`.

* Die Zugriffs Domäne `B.D.Z` ist `B.D`.

Wie das Beispiel zeigt, ist die Zugriffs Domäne eines Members nie größer als die eines enthaltenden Typs. Wenn z. b. alle `X` Member über `Public` deklarierte Barrierefreiheit verfügen, verfügen alle außer `A.X` über Barrierefreiheits Domänen, die durch einen enthaltenden Typ eingeschränkt werden.

Der Zugriff auf `Protected` Instanzmember muss über eine Instanz des abgeleiteten Typs erfolgen, damit nicht verknüpfte Typen keinen Zugriff auf die geschützten Member der anderen erhalten. Beispiel:

```vb
Class User
    Protected Password As String
End Class

Class Employee
    Inherits User
End Class

Class Guest
    Inherits User

    Public Function GetPassword(u As User) As String
        ' Error: protected access has to go through derived type.
        Return U.Password
    End Function
End Class
```

Im obigen Beispiel hat die-Klasse `Guest` nur Zugriff auf das geschützte `Password` Feld, wenn Sie mit einer Instanz von `Guest`qualifiziert ist. Dadurch wird verhindert, dass `Guest` Zugriff auf das `Password` Feld eines `Employee` Objekts erhält, indem Sie es einfach in `User`umwandeln.

Für den `Protected` Member-Zugriff in generischen Typen enthält der Deklarations Kontext Typparameter. Dies bedeutet, dass ein abgeleiteter Typ mit einem Satz von Typargumenten keinen Zugriff auf die `Protected` Member eines abgeleiteten Typs mit einem anderen Satz von Typargumenten hat. Beispiel:

```vb
Class Base(Of T)
    Protected x As T
End Class

Class Derived(Of T)
    Inherits Base(Of T)

    Public Sub F(y As Derived(Of String))
        ' Error: Derived(Of T) cannot access Derived(Of String)'s 
        '     protected members
        y.x = "a"
    End Sub
End Class
```

__Nebenbei.__ Die C# Sprache (und möglicherweise andere Sprachen) ermöglicht einem generischen Typ, auf `Protected` Member zuzugreifen, unabhängig davon, welche Typargumente angegeben werden. Dies sollte beim Entwerfen generischer Klassen, die `Protected` Member enthalten, berücksichtigt werden.


### <a name="constituent-types"></a>Konstituierende Typen

Die *einzelnen* Typen einer Deklaration sind die Typen, auf die von der Deklaration verwiesen wird. Der Typ einer Konstanten, der Rückgabetyp einer Methode und die Parametertypen eines Konstruktors sind z. b. alle konstituierenden Typen. Die Zugriffs Domäne eines eingebenden Typs einer Deklaration muss mit oder einer übergeordneten Zugriffs Domäne der Deklaration selbst identisch sein. Beispiel:

```vb
Public Class X
    Private Class Y
    End Class

    ' Error: Exposing private class Y outside of X.
    Public Function Z() As Y
    End Function

    ' Valid: Not exposing outside of X.
    Private Function A() As Y
    End Function
End Class

Friend Class B
    Private Class C
    End Class

    ' Error: Exposing private class Y outside of B.
    Public Function D() As C
    End Function
End Class
```

## <a name="type-and-namespace-names"></a>Typen-und Namespace Namen

Viele Sprachkonstrukte erfordern, dass ein Namespace oder ein Typ angegeben wird. Diese können mithilfe eines qualifizierten Formulars des Namespace oder des Typnamens angegeben werden. Ein *qualifizierter Name* besteht aus einer Reihe von Bezeichner, die durch Zeiträume getrennt sind. der Bezeichner auf der rechten Seite eines Zeitraums wird in dem Deklarations Bereich aufgelöst, der durch den Bezeichner auf der linken Seite des Zeitraums angegeben wird.

Der *voll qualifizierte Name* eines Namespaces oder Typs ist ein qualifizierter Name, der den Namen aller enthaltenden Namespaces und Typen enthält. Anders ausgedrückt: der voll qualifizierte Name eines Namespaces oder Typs ist `N.T`, wobei `T` der Name der Entität und `N` der voll qualifizierte Name der enthaltenden Entität ist.

Das folgende Beispiel zeigt mehrere Namespace-und Typdeklarationen zusammen mit den zugehörigen voll qualifizierten Namen in Inline Kommentaren.

```vb
Class A            ' A.
End Class

Namespace X        ' X.
    Class B        ' X.B.
        Class C    ' X.B.C.
        End Class
    End Class

    Namespace Y    ' X.Y.
        Class D    ' X.Y.D.
        End Class
    End Namespace 
End Namespace 

Namespace X.Y      ' X.Y.
    Class E        ' X.Y.E.
    End Class
End Namespace
```

Beachten Sie, dass der Namespace x. y an zwei verschiedenen Speicherorten im Quellcode deklariert wurde. diese beiden partiellen Deklarationen stellen jedoch nur einen einzelnen Namespace mit dem Namen X. y dar, der sowohl die Klasse D als auch die Klasse E enthält.

In einigen Situationen kann ein qualifizierter Name mit dem Schlüsselwort `Global`beginnen. Das Schlüsselwort stellt den unbenannten äußersten Namespace dar. Dies ist in Situationen nützlich, in denen eine Deklaration einen einschließenden Namespace Shadowing macht. Das `Global`-Schlüsselwort ermöglicht das "Escapezeichen" im äußersten Namespace in dieser Situation. Beispiel:

```vb
Namespace NS1
    Class System
    End Class

    Module Test
        Sub Main()
            ' Error: Class System does not contain Int32
            Dim x As System.Int32


            ' Legal, binds to System in outermost namespace
            Dim y As Global.System.Int32
        End Sub
    End Module
End Namespace
```

Im obigen Beispiel ist der erste Methoden Aufrufwert ungültig, da der be`System` Zeichner an die-Klasse `System`und nicht an den Namespace `System`gebunden ist. Die einzige Möglichkeit, auf den `System`-Namespace zuzugreifen, ist die Verwendung `Global`, um den äußersten Namespace zu verwenden. `Global` kann nicht in einer `Imports` Anweisung oder `Namespace` Deklaration verwendet werden.

Da in anderen Sprachen Typen und Namespaces eingeführt werden können, die mit Schlüsselwörtern in der Sprache identisch sind, Visual Basic erkannt, dass Schlüsselwörter als Teil eines qualifizierten Namens erkannt werden, solange Sie einem bestimmten Zeitraum entsprechen. Auf diese Weise verwendete Schlüsselwörter werden als Bezeichner behandelt. Der qualifizierte Bezeichner `X.Default.Class` z. b. ein gültiger qualifizierter Bezeichner, während `Default.Class` nicht ist.

### <a name="qualified-name-resolution-for-namespaces-and-types"></a>Qualifizierte Namensauflösung für Namespaces und Typen

Wenn ein qualifizierter Namespace-oder Typname der Form `N.R(Of A)`ist, wobei `R` der am weitesten rechts Bezeichner im qualifizierten Namen und `A` eine optionale Typargument Liste ist, wird in den folgenden Schritten beschrieben, wie Sie bestimmen, auf welchen Namespace oder Typ der qualifizierte Name verweist:

1. Lösen Sie `N`mit den Regeln für die qualifizierte oder nicht qualifizierte Namensauflösung.

2. Wenn die Auflösung von `N` fehlschlägt oder zu einem Typparameter aufgelöst wird, tritt ein Kompilierzeitfehler auf.

3. Andernfalls gilt: Wenn `R` dem Namen eines Namespaces in N entspricht und keine Typargumente angegeben wurden, oder `R` mit einem zugänglichen Typ in `N` mit derselben Anzahl von Typparametern wie Typargumenten übereinstimmt, verweist der qualifizierte Name auf diesen Namespace oder Typ.

4. Andernfalls bezieht sich der qualifizierte Name auf diesen Typ, wenn `N` ein oder mehrere Standardmodule enthält und `R` mit dem Namen eines barrierefreien Typs identisch ist, der mit der gleichen Anzahl von Typparametern wie Typargumenten übereinstimmt. Wenn `R` mit dem Namen der zugänglichen Typen übereinstimmt, die dieselbe Anzahl von Typparametern wie Typargumente haben, tritt ggf. in mehr als einem Standardmodul ein Kompilierzeitfehler auf.

5. Andernfalls tritt ein Kompilierungsfehler auf.

__Nebenbei.__ Eine Implikation dieses Auflösungsprozesses ist, dass Typmember beim Auflösen von Namespace-oder Typnamen keine Schatten-Namespaces oder-Typen verwenden.

### <a name="unqualified-name-resolution-for-namespaces-and-types"></a>Nicht qualifizierte Namensauflösung für Namespaces und Typen

Bei einem nicht qualifizierten Namen `R(Of A)`, bei dem `A` eine optionale Typargument Liste ist, wird in den folgenden Schritten beschrieben, wie Sie bestimmen, auf welchen Namespace oder Typ der nicht qualifizierte Name verweist:

1. Wenn R mit dem Namen eines Typparameters der aktuellen Methode übereinstimmt und keine Typargumente angegeben wurden, verweist der nicht qualifizierte Name auf diesen Typparameter.

2.  Für jeden Typ, der den Namen Verweis enthält, beginnend mit dem innersten Typ und dem äußersten:
    1. Wenn `R` dem Namen eines Typparameters im aktuellen Typ entspricht und keine Typargumente angegeben wurden, verweist der nicht qualifizierte Name auf diesen Typparameter.
    2. Andernfalls bezieht sich der nicht qualifizierte Name auf diesen Typ, wenn `R` dem Namen eines zugreif baren, zugreif baren Typs mit der gleichen Anzahl von Typparametern wie Typargumenten entspricht.

3. Für jeden eingefügten Namespace, der den Namen Verweis enthält, beginnend mit dem innersten Namespace und durchgehen zum äußersten Namespace:
    1. Wenn `R` im aktuellen Namespace mit dem Namen eines im aktuellen Namespace übereinstimmt und keine Typargument Liste angegeben ist, bezieht sich der nicht qualifizierte Name auf diesen schsted Namespace.
    2. Andernfalls bezieht sich der nicht qualifizierte Name auf diesen Typ, wenn `R` dem Namen eines barrierefreien Typs mit der gleichen Anzahl von Typparametern als Typargumente entspricht (sofern vorhanden).
    3. Andernfalls bezieht sich der nicht qualifizierte Name, wenn der Namespace mindestens ein zugreif bares Standardmodul enthält, und `R` dem Namen eines zugreif baren, nicht qualifizierten Typs mit der gleichen Anzahl von Typparametern wie Typargumenten, sofern vorhanden, in genau einem Standardmodul. Wenn `R` den Namen von zugreif baren, zugreif baren Typen mit der gleichen Anzahl von Typparametern wie Typargumenten, sofern vorhanden, in mehr als einem Standardmodul abgleicht, tritt ein Kompilierzeitfehler auf.

4. Wenn die Quelldatei mindestens einen Alias Alias aufweist und `R` mit dem Namen einer dieser Dateien übereinstimmt, verweist der nicht qualifizierte Name auf diesen importieralias. Wenn eine Typargument Liste angegeben wird, tritt ein Kompilierzeitfehler auf.

5. Wenn die Quelldatei, die den Namen Verweis enthält, mindestens einen Import hat:
    1. Wenn `R` dem Namen eines barrierefreien Typs mit der gleichen Anzahl von Typparametern wie Typargumenten entspricht (sofern vorhanden), bezieht sich der nicht qualifizierte Name auf diesen Typ. Wenn `R` dem Namen eines barrierefreien Typs mit der gleichen Anzahl von Typparametern wie Typargumenten entspricht (sofern vorhanden), tritt bei mehr als einem Import und bei allen nicht der gleiche Typ auf. ein Kompilierzeitfehler tritt auf.
    2. Andernfalls bezieht sich der nicht qualifizierte Name auf diesen Namespace, wenn keine Typargument Liste angegeben wurde und `R` mit dem Namen eines Namespaces mit barrierefreien Typen in genau einem Import übereinstimmt. Wenn keine Typargument Liste angegeben wurde und `R` mit dem Namen eines Namespaces mit barrierefreien Typen in mehr als einem Import übereinstimmt und alle nicht denselben Namespace haben, tritt ein Kompilierzeitfehler auf.
    3. Wenn die Importe ein oder mehrere barrierefreie Standardmodule enthalten und `R` mit dem Namen eines zugreif baren, zugreif baren Typs mit der gleichen Anzahl von Typparametern wie Typargumente übereinstimmt (sofern vorhanden), bezieht sich der nicht qualifizierte Name in genau einem Standardmodul auf diesen Typ. Wenn `R` den Namen von zugreif baren, zugreif baren Typen mit der gleichen Anzahl von Typparametern wie Typargumenten, sofern vorhanden, in mehr als einem Standardmodul abgleicht, tritt ein Kompilierzeitfehler auf.

6. Wenn in der Kompilierungs Umgebung mindestens eine Import-Aliase definiert ist und `R` mit dem Namen einer dieser Elemente übereinstimmt, verweist der nicht qualifizierte Name auf diesen importieralias. Wenn eine Typargument Liste angegeben wird, tritt ein Kompilierzeitfehler auf.

7. Wenn in der Kompilierungs Umgebung mindestens ein Import definiert ist:
    1. Wenn `R` dem Namen eines barrierefreien Typs mit der gleichen Anzahl von Typparametern wie Typargumenten entspricht (sofern vorhanden), bezieht sich der nicht qualifizierte Name auf diesen Typ. Wenn `R` dem Namen eines barrierefreien Typs mit der gleichen Anzahl von Typparametern wie Typargumenten entspricht (sofern vorhanden), tritt in mehr als einem Import ein Kompilierzeitfehler auf.
    2. Andernfalls bezieht sich der nicht qualifizierte Name auf diesen Namespace, wenn keine Typargument Liste angegeben wurde und `R` mit dem Namen eines Namespaces mit barrierefreien Typen in genau einem Import übereinstimmt. Wenn keine Typargument Liste angegeben wurde und `R` dem Namen eines Namespaces mit barrierefreien Typen in mehr als einem Import entspricht, tritt ein Kompilierzeitfehler auf.
    3. Wenn die Importe ein oder mehrere barrierefreie Standardmodule enthalten und `R` mit dem Namen eines zugreif baren, zugreif baren Typs mit der gleichen Anzahl von Typparametern wie Typargumente übereinstimmt (sofern vorhanden), bezieht sich der nicht qualifizierte Name in genau einem Standardmodul auf diesen Typ. Wenn `R` den Namen von zugreif baren, zugreif baren Typen mit der gleichen Anzahl von Typparametern wie Typargumenten, sofern vorhanden, in mehr als einem Standardmodul abgleicht, tritt ein Kompilierzeitfehler auf.

8. Andernfalls tritt ein Kompilierungsfehler auf.

__Nebenbei.__ Eine Implikation dieses Auflösungsprozesses ist, dass Typmember beim Auflösen von Namespace-oder Typnamen keine Schatten-Namespaces oder-Typen verwenden.

Normalerweise kann ein Name nur einmal in einem bestimmten Namespace vorkommen. Da Namespaces jedoch über mehrere .NET-Assemblys hinweg deklariert werden können, kann es vorkommen, dass zwei Assemblys einen Typ mit demselben voll qualifizierten Namen definieren. In diesem Fall wird ein Typ, der im aktuellen Satz von Quelldateien deklariert ist, von einem in einer externen .NET-Assembly deklarierten Typ bevorzugt. Andernfalls ist der Name mehrdeutig, und es gibt keine Möglichkeit, den Namen zu unterscheiden.

## <a name="variables"></a>Variablen

Eine *Variable* stellt einen Speicherort dar. Jede Variable verfügt über einen Typ, der bestimmt, welche Werte in der Variablen gespeichert werden können. Da Visual Basic eine typsichere Sprache ist, weist jede Variable in einem Programm einen-Typ auf, und die-Sprache stellt sicher, dass in Variablen gespeicherte Werte immer den entsprechenden Typ haben. Variablen werden immer mit dem Standardwert ihres Typs initialisiert, bevor ein Verweis auf die Variable erstellt werden kann. Es ist nicht möglich, auf nicht initialisierten Speicher zuzugreifen.

## <a name="generic-types-and-methods"></a>Generische Typen und Methoden

Typen (außer bei Standardmodulen und Enumerationstypen) und Methoden können *Typparameter*deklarieren, bei denen es sich um Typen handelt, die nicht bereitgestellt werden, bis eine Instanz des Typs deklariert oder die-Methode aufgerufen wird. Typen und Methoden mit Typparametern werden auch als *generische Typen* und *generische Methoden*bezeichnet, da der Typ oder die Methode generisch geschrieben werden muss, ohne dass bestimmte Kenntnisse der Typen vorliegen, die von Code bereitgestellt werden, der den Typ oder die Methode verwendet.

__Nebenbei.__ Obwohl Methoden und Delegaten generisch sein können, können Eigenschaften, Ereignisse und Operatoren zu diesem Zeitpunkt nicht generisch sein. Sie können jedoch Typparameter der enthaltenden Klasse verwenden.

Aus der Perspektive des generischen Typs oder der generischen Methode ist ein Typparameter ein Platzhalter, der mit einem tatsächlichen Typ ausgefüllt wird, wenn der Typ oder die Methode verwendet wird. Typargumente ersetzen an dem Punkt, an dem der Typ oder die Methode verwendet wird, die Typparameter in dem Typ oder der Methode. Eine generische Stapel Klasse könnte z. b. wie folgt implementiert werden:

```vb
Public Class Stack(Of ItemType)
    Protected Items(0 To 99) As ItemType
    Protected CurrentIndex As Integer = 0

    Public Sub Push(data As ItemType)
        If CurrentIndex = 100 Then
            Throw New ArgumentException("Stack is full.")
        End If

        Items(CurrentIndex) = Data
        CurrentIndex += 1
    End Sub

    Public Function Pop() As ItemType
        If CurrentIndex = 0 Then
            Throw New ArgumentException("Stack is empty.")
        End If

        CurrentIndex -= 1
        Return Items(CurrentIndex + 1) 
    End Function
End Class
```

Deklarationen, die die `Stack(Of ItemType)`-Klasse verwenden, müssen ein Typargument für den Typparameter `ItemType`bereitstellen. Dieser Typ wird dann ausgefüllt, wenn `ItemType` in der-Klasse verwendet wird:

```vb
Option Strict On

Module Test
    Sub Main()
        Dim s1 As New Stack(Of Integer)()
        Dim s2 As New Stack(Of Double)()

        s1.Push(10.10)   ' Error: Stack(Of Integer).Push takes an Integer
        s2.Push(10.10)   ' OK: Stack(Of Double).Push takes a Double
        Console.WriteLine(s2.Pop().GetType().ToString()) ' Prints: Double
    End Sub
End Module
```

### <a name="type-parameters"></a>Typparameter

Typparameter können in Typ-oder Methoden Deklarationen angegeben werden. Jeder Typparameter ist ein Bezeichner, der ein Platzhalter für ein Typargument ist, das zum Erstellen eines konstruierten Typs oder einer Methode bereitgestellt wird. Im Gegensatz dazu ist ein Typargument der tatsächliche Typ, der den Typparameter ersetzt, wenn ein generischer Typ oder eine generische Methode verwendet wird.

```antlr
TypeParameterList
    : OpenParenthesis 'Of' TypeParameter ( Comma TypeParameter )* CloseParenthesis
    ;

TypeParameter
    : VarianceModifier? Identifier TypeParameterConstraints?
    ;

VarianceModifier
    : 'In' | 'Out'
    ;
```

Jeder Typparameter in einer Typ-oder Methoden Deklaration definiert einen Namen im Deklarations Raum dieses Typs oder dieser Methode. Daher kann er nicht denselben Namen wie ein anderer Typparameter, ein Typmember, ein Methoden Parameter oder eine lokale Variable haben. Der Gültigkeitsbereich eines Typparameters für einen Typ oder eine Methode ist der gesamte Typ oder die Methode. Da Typparameter auf die gesamte Typdeklaration festgelegt sind, können für die Typen der äußeren Typparameter verwendet werden. Dies bedeutet auch, dass Typparameter immer beim Zugriff auf Typen angegeben werden müssen, die in generischen Typen geschachtelt sind:

```vb
Public Class Outer(Of T)
    Public Class Inner
        Public Sub F(x As T)
            ...
        End Sub
    End Class
End Class

Module Test
    Sub Main()
        Dim x As New Outer(Of Integer).Inner()
        ...
    End Sub
End Module
```

Im Gegensatz zu anderen Membern einer Klasse werden Typparameter nicht geerbt. Auf Typparameter in einem Typ kann nur mit dem einfachen Namen verwiesen werden. mit anderen Worten, Sie können nicht mit dem enthaltenden Typnamen qualifiziert werden. Obwohl es sich um einen ungültigen Programmierstil handelt, können die Typparameter in einem Schraffurtyp einen Member oder Typparameter ausblenden, der im äußeren Typ deklariert ist:

```vb
Class Outer(Of T)
    Class Inner(Of T)
        Public t1 As T    ' Refers to Inner's T
    End Class
End Class
```

Typen und Methoden können auf Grundlage der Anzahl von Typparametern (oder der *Arität*), die von den Typen oder Methoden deklariert werden, überladen werden. Beispielsweise sind die folgenden Deklarationen zulässig:

```vb
Module C
    Sub M()
    End Sub

    Sub M(Of T)()
    End Sub

    Sub M(Of T, U)()
    End Sub
End Module

Structure C(Of T)
    Dim x As T
End Structure

Class C(Of T, U)
End Class
```

Im Fall von Typen werden über Ladungen immer mit der Anzahl der angegebenen Typargumente abgeglichen. Dies ist nützlich, wenn sowohl generische als auch nicht generische Klassen im gleichen Programm verwendet werden:

```vb
Class Queue 
End Class      

Class Queue(Of T)
End Class

Class X
    Dim q1 As Queue                 ' Non-generic queue
    Dim q2 As Queue(Of Integer)     ' Generic queue
End Class
```

Regeln für Methoden, die für Typparameter überladen werden, werden im Abschnitt zur Auflösung von Methoden Überladungen behandelt.

In der enthaltenden Deklaration werden Typparameter als vollständige Typen angesehen. Da ein Typparameter mit vielen verschiedenen tatsächlichen Typargumenten instanziiert werden kann, haben Typparameter etwas andere Vorgänge und Einschränkungen als andere Typen, wie unten beschrieben:

* Ein Typparameter kann nicht direkt verwendet werden, um eine Basisklasse oder Schnittstelle zu deklarieren.

* Die Regeln für die Element Suche für Typparameter hängen von den Einschränkungen ab, die ggf. auf den Typparameter angewendet werden.

* Die verfügbaren Konvertierungen für einen Typparameter hängen von den Einschränkungen ab, die ggf. auf die Typparameter angewendet werden.

* Wenn keine `Structure` Einschränkung vorhanden ist, kann ein Wert mit einem Typ, der durch einen Typparameter dargestellt wird, mit `Nothing` mithilfe von `Is` und `IsNot`verglichen werden.

* Ein Typparameter kann nur in einem `New` Ausdruck verwendet werden, wenn der Typparameter durch eine `New` oder eine `Structure` Einschränkung eingeschränkt wird.

* Ein Typparameter kann nicht an einer beliebigen Stelle innerhalb einer Attribut Ausnahme innerhalb eines `GetType` Ausdrucks verwendet werden.

* Typparameter können als Typargumente für andere generische Typen und Parameter verwendet werden.

Das folgende Beispiel ist ein generischer Typ, der die `Stack(Of ItemType)`-Klasse erweitert:

```vb
Class MyStack(Of ItemType)
    Inherits Stack(Of ItemType)

    Public ReadOnly Property Size() As Integer
        Get
            Return CurrentIndex
        End Get
    End Property
End Class
```

Wenn eine Deklaration ein Typargument für `MyStack`bereitstellt, wird dasselbe Typargument auch auf `Stack` angewendet.

Typparameter sind ein reines Kompilierzeit Konstrukt. Zur Laufzeit wird jeder Typparameter an einen Lauf Zeittyp gebunden, der durch Bereitstellen eines Typarguments an die generische Deklaration angegeben wurde. Daher ist der Typ einer Variablen, die mit einem Typparameter deklariert wird, zur Laufzeit ein nicht generischer Typ oder ein bestimmter konstruierter Typ. Die Lauf Zeit Ausführung aller Anweisungen und Ausdrücke, die Typparameter betreffen, verwendet den eigentlichen Typ, der als Typargument für diesen Parameter angegeben wurde.


### <a name="type-constraints"></a>Typeinschränkungen

Da ein Typargument ein beliebiger Typ im Typsystem sein kann, kann ein generischer Typ oder eine generische Methode keine Annahmen über einen Typparameter treffen. Folglich werden die Member eines Typparameters als Member des Typs `Object`betrachtet, da alle Typen von `Object`abgeleitet werden.

Im Fall einer Auflistung wie `Stack(Of ItemType)`ist diese Tatsache möglicherweise keine besonders wichtige Einschränkung, aber es kann vorkommen, dass ein generischer Typ eine Annahme über die Typen machen möchte, die als Typargumente bereitgestellt werden. *Typeinschränkungen* können für Typparameter festgelegt werden, die einschränken, welche Typen als Typparameter bereitgestellt werden können, und generischen Typen oder Methoden ermöglichen, mehr über Typparameter zu übernehmen.

```antlr
TypeParameterConstraints
    : 'As' Constraint
    | 'As' OpenCurlyBrace ConstraintList CloseCurlyBrace
    ;

ConstraintList
    : Constraint ( Comma Constraint )*
    ;

Constraint
    : TypeName
    | 'New'
    | 'Structure'
    | 'Class'
    ;
```


```vb
Public Class DisposableStack(Of ItemType As IDisposable)
    Implements IDisposable

    Private _items(0 To 99) As ItemType
    Private _currentIndex As Integer = 0

    Public Sub Push(data As ItemType)
        ...
    End Sub

    Public Function Pop() As ItemType
        ...
    End Function

    Private Sub Dispose() Implements IDisposable.Dispose
        For Each item As IDisposable In _items
            If item IsNot Nothing Then
                item.Dispose()
            End If
        Next item
    End Sub
End Class
```

In diesem Beispiel schränkt der `DisposableStack(Of ItemType)` den Typparameter auf Typen ein, die die-Schnittstelle implementieren `System.IDisposable`. Folglich kann eine `Dispose` Methode implementiert werden, die alle in der Warteschlange verbleibenden Objekte freigibt.

Eine Typeinschränkung muss eine der besonderen Einschränkungen `Class`, `Structure`oder `New`sein, oder es muss sich um einen Typ handeln `T` in dem Folgendes gilt:

* `T` ist eine Klasse, eine Schnittstelle oder ein Typparameter.

* `T` ist nicht `NotInheritable`.

* `T` ist weder eine von noch ein Typ, der von einem von geerbt wurde, die folgenden speziellen Typen: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`oder `System.ValueType`.

* `T` ist nicht `Object`. Da alle Typen von `Object`abgeleitet sind, hätte eine solche Einschränkung keine Auswirkung, wenn Sie zulässig wäre.

* `T` muss mindestens so zugänglich sein, wie der generische Typ oder die Methode, die deklariert wird.

Mehrere Typeinschränkungen können für einen einzelnen Typparameter angegeben werden, indem die Typeinschränkungen in geschweiften Klammern (`{}`) eingeschlossen werden. Nur eine Typeinschränkung für einen angegebenen Typparameter kann eine Klasse sein. Es ist ein Fehler, eine `Structure` besondere Einschränkung mit einer benannten Klassen Einschränkung oder der `Class` speziellen Einschränkung zu kombinieren.

```vb
Class ControlFactory(Of T As {Control, New})
    ...
End Class
```

Typeinschränkungen können die enthaltenden Typen oder die Typparameter der enthaltenden Typen verwenden. Im folgenden Beispiel erfordert die-Einschränkung, dass das angegebene Typargument eine generische Schnittstelle implementiert, die sich selbst als Typargument verwendet:

```vb
Class Sorter(Of V As IComparable(Of V))
    ...
End Class
```

Die besondere Typeinschränkung `Class` das angegebene Typargument auf einen beliebigen Verweistyp beschränkt.

__Nebenbei.__ Die besondere typeinschränkungs `Class` kann durch eine Schnittstelle erfüllt werden. Und eine-Struktur kann eine Schnittstelle implementieren. Daher kann die Einschränkungs `(Of T As U, U As Class)` mit "t" einer Struktur erfüllt werden (die die besondere Einschränkung von `Class` nicht erfüllt), und "U" ist eine Schnittstelle, die Sie implementiert (was die `Class` besondere Einschränkung erfüllt).

Die besondere Typeinschränkung `Structure` das angegebene Typargument auf einen beliebigen Werttyp außer `System.Nullable(Of T)`beschränkt.

__Nebenbei.__ Struktur Einschränkungen lassen keine `System.Nullable(Of T)` zu, sodass es nicht möglich ist, `System.Nullable(Of T)` als Typargument für sich selbst bereitzustellen.

Die besondere typeinschränkungs `New` erfordert, dass das angegebene Typargument über einen zugänglichen Parameter losen Konstruktor verfügen muss und nicht als `MustInherit`deklariert werden kann. Beispiel:

```vb
Class Factory(Of T As New)
    Function CreateInstance() As T
        Return New T()
    End Function
End Class
```

Eine Klassentyp Einschränkung erfordert, dass das angegebene Typargument entweder den Typ oder davon erben muss. Eine Schnittstellentyp Einschränkung erfordert, dass das angegebene Typargument diese Schnittstelle implementieren muss. Eine Typparameter Einschränkung erfordert, dass das angegebene Typargument von abgeleitet ist oder alle für den übereinstimmenden Typparameter angegebenen Begrenzungen implementiert. Beispiel:

```vb
Class List(Of T)
    Sub AddRange(Of S As T)(collection As IEnumerable(Of S))
        ...
    End Sub
End Class
```

In diesem Beispiel wird der Typparameter `S` auf `AddRange` auf den Typparameter `T` `List`beschränkt. Dies bedeutet, dass ein `List(Of Control)` `AddRange`den Typparameter auf jeden Typ einschränkt, der von `Control`erbt.

Eine Typparameter Einschränkung `Of S As T` durch transitiv das Hinzufügen aller `T`Einschränkungen `S`, außer den besonderen Einschränkungen (`Class`, `Structure``New`), aufgelöst werden. Es ist ein Fehler, zirkuläre Einschränkungen (z. b. `Of S As T, T As S`) zu haben. Es ist ein Fehler, eine Typparameter Einschränkung zu haben, die selbst über die `Structure`-Einschränkung verfügt. Nach dem Hinzufügen von Einschränkungen ist es möglich, dass eine Reihe von besonderen Situationen eintreten kann:

* Wenn mehrere Klassen Einschränkungen vorhanden sind, gilt die am meisten abgeleitete Klasse als Einschränkung. Wenn eine oder mehrere Klassen Einschränkungen nicht über eine Vererbungs Beziehung verfügen, kann die Einschränkung nicht wieder hergestellt werden, und es handelt sich um einen Fehler.

 * Wenn ein Typparameter eine `Structure` besondere Einschränkung mit einer benannten Klassen Einschränkung oder der `Class` speziellen Einschränkung kombiniert, ist dies ein Fehler. Eine Klassen Einschränkung kann `NotInheritable`werden. in diesem Fall werden keine abgeleiteten Typen dieser Einschränkung akzeptiert, und es handelt sich um einen Fehler.

Der Typ kann ein oder ein Typ sein, der von geerbt wurde, die folgenden speziellen Typen: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`oder `System.ValueType`. In diesem Fall wird nur der Typ oder ein von ihm geerbte Typ akzeptiert. Ein Typparameter, der auf einen dieser Typen beschränkt ist, kann nur die Konvertierungen verwenden, die vom `DirectCast` Operator zugelassen werden. Beispiel:

```vb
MustInherit Class Base(Of T)
    MustOverride Sub S1(Of U As T)(x As U)
End Class

Class Derived
    Inherits Base(Of Integer)

    ' The constraint of U must be Integer, which is normally not allowed.
    Overrides Sub S1(Of U As Integer)(x As U)
        Dim y As Integer = x    ' OK
        Dim z As Long = x       ' Error: Can't convert
    End Sub
End Class
```

Außerdem kann ein Typparameter, der aufgrund einer der oben genannten Lockerungen auf einen Werttyp beschränkt ist, keine Methoden aufrufen, die für diesen Werttyp definiert sind. Beispiel:

```vb
Class C1(Of T)
    Overridable Sub F(Of G As T)(x As G)
    End Sub
End Class

Class C2
    Inherits C1(Of IntPtr)

    Overrides Sub F(Of G As IntPtr)(ByVal x As G)
        ' Error: Cannot access structure members
         x.ToInt32()
    End Sub
End Class
```

Wenn die Einschränkung nach der Ersetzung als Arraytyp endet, ist auch ein beliebiger kovariant-Arraytyp zulässig. Beispiel:

```vb
Module Test
    Class B
    End Class

    Class D
        Inherits B
    End Class

    Function F(Of T, U As T)(x As U) As T
        Return x
    End Function

    Sub Main()
        Dim a(9) As B
        Dim b(9) As D

        a = F(Of B(), D())(b)
    End Sub
End Module
```

Ein Typparameter mit einer Klassen-oder Schnittstellen Einschränkung gilt als die gleichen Member wie diese Klassen-oder Schnittstellen Einschränkung. Wenn ein Typparameter mehrere Einschränkungen aufweist, wird der Typparameter als die Vereinigung aller Member der Einschränkungen betrachtet. Wenn in mehr als einer Einschränkung Member mit demselben Namen vorhanden sind, werden Member in der folgenden Reihenfolge ausgeblendet: die Klassen Einschränkung blendet Member in Schnittstellen Einschränkungen aus, die Member in `System.ValueType` ausblenden (wenn `Structure` Einschränkung angegeben ist), wodurch Elemente in `Object`ausgeblendet werden. Wenn ein Member mit demselben Namen in mehr als einer Schnittstellen Einschränkung angezeigt wird, ist der Member nicht verfügbar (wie bei der mehrfach Schnittstellen Vererbung), und der Typparameter muss in die gewünschte Schnittstelle umgewandelt werden. Beispiel:

```vb
Class C1
    Sub S1(x As Integer)
    End Sub
End Class

Interface I1
    Sub S1(x As Integer)
End Interface

Interface I2
    Sub S1(y As Double)
End Interface

Module Test
    Sub T1(Of T As {C1, I1, I2})()
        Dim a As T
        a.S1(10)       ' Calls C1.S1, which is preferred
        a.S1(10.10)    ' Also calls C1.S1, class is still preferred
    End Sub

    Sub T2(Of T As {I1, I2})()
        Dim a As T
        a.S1(10)    ' Error: Call is ambiguous between I1.S1, I2.S1
    End Sub
End Module
```

Beim Bereitstellen von Typparametern als Typargumente müssen die Typparameter die Einschränkungen der übereinstimmenden Typparameter erfüllen.

```vb
Class Base(Of T As Class)
End Class

Class Derived(Of V)
    ' Error: V does not satisfy the constraints of T
    Inherits Base(Of V)
End Class
```

Werte eines eingeschränkten Typparameters können für den Zugriff auf die Instanzmember, einschließlich der in der Einschränkung angegebenen Instanzmethoden, verwendet werden.

```vb
Interface IPrintable
    Sub Print()
End Interface

Class Printer(Of V As IPrintable)
    Sub PrintOne(v1 As V)
        V1.Print()
    End Sub
End Class
```

### <a name="type-parameter-variance"></a>Typparameter Varianz

Ein Typparameter in einer Schnittstelle oder eine delegattypdeklaration kann optional einen *Varianz-Modifizierer*angeben. Typparameter mit Varianz modifiziermetern beschränken, wie der Typparameter in der Schnittstelle oder im Delegattyp verwendet werden kann, aber die Konvertierung einer generischen Schnittstelle oder eines Delegattyps in einen anderen generischen Typ mit Varianten kompatiblen Typargumenten ermöglicht Beispiel:

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class

Module Test
    Sub Main()
        Dim x As IEnumerable(Of Derived) = ...

        ' OK, as IEnumerable(Of Base) is variant compatible
        ' with IEnumerable(Of Derived)
        Dim y As IEnumerable(Of Base) = x
    End Sub
End Module
```

Generische Schnittstellen, die über Typparameter mit Varianz modifiziermetern verfügen, haben mehrere Einschränkungen:

* Sie können keine Ereignis Deklaration enthalten, die eine Parameterliste angibt (aber eine benutzerdefinierte Ereignis Deklaration oder eine Ereignis Deklaration mit einem Delegattyp ist zulässig).

* Sie dürfen keine Klassen, Strukturen oder Enumerationstypen enthalten.

__Nebenbei.__ Diese Einschränkungen sind darauf zurückzuführen, dass Typen, die in generischen Typen enthalten sind, implizit die generischen Parameter ihrer übergeordneten Elemente kopieren. Im Fall von Klassen-, Struktur-oder Enumerationstypen können diese Arten von Typen keine Varianz Modifizierer für ihre Typparameter aufweisen. Im Fall einer Ereignis Deklaration mit einer Parameterliste kann die generierte geschaltete Delegatklasse verwirrende Fehler aufweisen, wenn ein Typ, der in einer `In` Position (d. h. in einem Parametertyp) angezeigt wird, tatsächlich an einer `Out` Position (d. h. dem Ereignistyp) verwendet wird.

Ein Typparameter, der mit dem out-Modifizierer deklariert wird, ist *kovariant*. Informell kann ein kovarianter Typparameter nur in einer Ausgabe Position verwendet werden, d. h. ein Wert, der von der Schnittstelle oder vom Delegattyp zurückgegeben wird, und kann nicht an einer Eingabe Position verwendet werden. Eine Typ`T` wird als *gültig* betrachtet, wenn Folgendes gilt:

* `T` ist eine Klasse, eine Struktur oder ein enumerierter Typ.

* `T` ist ein nicht generischer Delegat-oder Schnittstellentyp.

* `T` ist ein Arraytyp, dessen Elementtyp kovarianter ist.

* `T` ist ein Typparameter, der nicht als `Out` Typparameter deklariert wurde.

* `T` ist eine erstellte Schnittstelle oder ein Delegattyp `X(Of P1,...,Pn)` mit Typargumenten `A1,...,An` Folgendes:

  * Wenn `Pi` als out-Typparameter deklariert wurde, ist `Ai` kovariant gültig.

  * Wenn `Pi` als im Typparameter deklariert wurde, ist `Ai` kontra varianter.

Folgendes muss in einer Schnittstelle oder einem Delegattyp gültig sein:

* Die Basisschnittstelle einer Schnittstelle.

* Der Rückgabetyp einer Funktion oder des Delegattyps.

* Der Typ einer Eigenschaft, wenn ein `Get` Accessor vorhanden ist.

* Der Typ eines beliebigen `ByRef` Parameters.

Beispiel:

```vb
Delegate Function D(Of Out T, U)(x As U) As T

Interface I1(Of Out T)
End Interface

Interface I2(Of Out T)
    Inherits I1(Of T)

    ' OK, T is only used in an Out position
    Function M1(x As I1(Of T)) As T

    ' Error: T is used in an In position
    Function M2(x As T) As T
End Interface
```

__Nebenbei.__ `Out` ist kein reserviertes Wort.

Ein Typparameter, der mit dem in-Modifizierer deklariert wird, ist *kontra Variant*. Ein kontra varianter Typparameter kann nur in einer Eingabe Position verwendet werden, d. h. ein Wert, der an die Schnittstelle oder den Delegattyp übergeben wird, und kann nicht in einer Ausgabe Position verwendet werden. Eine `T` vom Typ wird als *gültig* betrachtet, wenn Folgendes gilt:

* `T` ist eine Klasse, eine Struktur oder ein enumerierter Typ.

* `T` ist ein nicht generischer Delegat-oder Schnittstellentyp.

* `T` ist ein Arraytyp, dessen Elementtyp kontra varianter ist.

* `T` ist ein Typparameter, der nicht als im Typparameter deklariert wurde.

* `T` ist eine erstellte Schnittstelle oder ein Delegattyp `X(Of P1,...,Pn)` mit Typargumenten `A1,...,An` Folgendes:

  * Wenn `Pi` als `Out` Typparameter deklariert wurde, ist `Ai` kontra varianter.

  * Wenn `Pi` als `In` Typparameter deklariert wurde, ist `Ai` kovariant gültig.

Folgendes muss bei einer Schnittstelle oder einem Delegattyp überwiegend gültig sein:

* Der Typ eines Parameters.

* Eine Typeinschränkung für einen Methodentypparameter.

* Der Typ einer Eigenschaft, wenn Sie über einen `Set` Accessor verfügt.

* Der Typ eines Ereignisses.

Beispiel:

```vb
Delegate Function D(Of T, In U)(x As U) As T

Interface I1(Of In T)
End Interface

Interface I2(Of In T)
    ' OK, T is only used in an In position
    Sub M1(x As I1(Of T))

    ' Error: T is used in an Out position
    Function M2() As T
End Interface
```

In dem Fall, in dem ein Typ gültig sein muss, muss er gegen überdies und kovarianter sein (z. b. eine Eigenschaft mit einem `Get`-und `Set`-Accessor oder einem `ByRef`-Parameter), kann kein Variant-Typparameter verwendet werden.


Ko-und Kontra Varianz können zu einem "Diamond mehrdeutigkeitsproblem" führen. Betrachten Sie folgenden Code:

```vb
Class C
    Implements IEnumerable(Of String)
    Implements IEnumerable(Of Exception)
     
    Public Function GetEnumerator1() As IEnumerator(Of String) _
       Implements IEnumerable(Of String).GetEnumerator
       Console.WriteLine("string")
    End Function
     
    Public Function GetEnumerator2() As IEnumerator(Of Exception) _
       Implements IEnumerable(Of Execption).GetEnumerator
       Console.WriteLine("exception")
    End Function
End Class
     
Dim c As IEnumerable(Of Object) = New C
c.GetEnumerator()
```

Die-Klasse `C` kann auf zwei Arten in `IEnumerable(Of Object)` konvertiert werden, beide durch kovariante Konvertierung von `IEnumerable(Of String)` und durch kovariante Konvertierung aus `IEnumerable(Of Exception)`. Die CLR gibt nicht an, welche der beiden Methoden von `c.GetEnumerator()`aufgerufen wird. Im Allgemeinen gilt: Wenn eine Klasse deklariert wird, um eine kovariante Schnittstelle mit zwei unterschiedlichen generischen Argumenten zu implementieren, die einen gemeinsamen Obertyp aufweisen (z. b. in diesem Fall `String` und `Exception` über die gemeinsame Obertyp-`Object`verfügen), oder wenn eine Klasse deklariert wird, um eine kontra Variante Schnittstelle mit zwei unterschiedlichen generischen Argumenten zu implementieren, die einen Der Compiler gibt eine Warnung für solche Deklarationen aus.
