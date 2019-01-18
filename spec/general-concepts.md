---
ms.openlocfilehash: be4090cc1ee294342645029e327d33627e8473fe
ms.sourcegitcommit: 6eca149bdc736113e0adb709212bd266c9503c33
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2019
ms.locfileid: "49461355"
---
# <a name="general-concepts"></a>Allgemeine Konzepte

In diesem Kapitel werden einige Konzepte, die erforderlich sind, um die Semantik der Microsoft Visual Basic-Sprache zu verstehen. Viele der Konzepte sollten auf Visual Basic-Programmierer oder C/C++-Programmierer vertraut sein, aber ihre genauen Definitionen unterschiedlich.

## <a name="declarations"></a>Deklarationen

Visual Basic-Programms benannten Entitäten besteht. Diese Entitäten werden eingeführt, über *Deklarationen* und die "Bedeutung" des Programms darstellen.

Auf der obersten Ebene *Namespaces* sind Entitäten, die anderen Entitäten, z. B. geschachtelte Namespaces und Typen zu organisieren. *Typen* sind Entitäten, die Werte beschreiben, und Definieren von ausführbarem Code. Typen möglicherweise enthalten geschachtelte Typen und Typmember. *Typmember* sind Konstanten, Variablen, Methoden, Operatoren, Eigenschaften, Ereignisse, Enumerationswerte und Konstruktoren.

Definiert eine Entität, die andere Entitäten enthalten, kann ein *Deklarationsabschnitt*. Entitäten sind in einem Deklarationsabschnitt über Deklarationen oder Vererbung eingeführt. die enthaltende Deklarationsabschnitt wird aufgerufen, der Entitäten *Deklarationskontext*. Deklarieren eine Entität in einem Deklarationsabschnitt im Gegenzug definiert einen neuen Deklarationsabschnitt, der weitere geschachtelte Entitätsdeklarationen enthalten kann. Daher bilden die Deklarationen in einem Programm eine Hierarchie der Deklaration Leerzeichen.

Mit Ausnahme von ist es im Fall von überladenen Typmembern ungültig für Deklarationen identisch benannte Entitäten der gleichen Art in der gleichen Deklarationskontext eingeführt. Darüber hinaus kann ein Deklarationsabschnitt nie verschiedene Arten von Entitäten mit dem gleichen Namen enthalten; Beispielsweise kann ein Deklarationsabschnitt niemals eine Variable und eine Methode mit dem gleichen Namen enthalten.

__Beachten Sie.__ Es kann in anderen Sprachen eine Deklaration Platz zu schaffen, die verschiedene Arten von Entitäten mit demselben Namen enthält (beispielsweise, wenn die Sprache Groß-/Kleinschreibung beachtet ist und andere Deklarationen, die basierend auf Groß-/Kleinschreibung ermöglicht) möglich sein. In diesem Fall wird die zugänglichste Entität, die an diesen Namen gebunden betrachtet. Wenn mehr als einen Typ der Entität am häufigsten zugegriffen werden kann, ist der Name nicht eindeutig. `Public` ist zugänglicher als `Protected Friend`, `Protected Friend` ist zugänglicher als `Protected` oder `Friend`, und `Protected` oder `Friend` ist zugänglicher als `Private`.

Deklarationsbereich eines Namespace ist "beendet, öffnen Sie", also zwei Namespacedeklarationen mit den gleichen vollqualifizierten Namen zum selben Deklarationsabschnitt beitragen. Im folgenden Beispiel wird die beiden Namespacedeklarationen, die zum selben Deklarationsabschnitt, deklarieren zwei Klassen mit den vollqualifizierten Namen in diesem Fall beitragen `Data.Customer` und `Data.Order:`

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

Da die beiden Deklarationen zum selben Deklarationsabschnitt beitragen möchten, würde ein Kompilierungsfehler auftreten, wenn es sich bei jeweils eine Deklaration einer Klasse mit dem gleichen Namen enthalten.

### <a name="overloading-and-signatures"></a>Überladen und Signaturen

Ist die einzige Möglichkeit, Dateien mit identischem Namen Entitäten auf der gleichen Art in einem Deklarationsabschnitt *überladen*. Nur Methoden, Operatoren, Instanzkonstruktoren und Eigenschaften können überladen werden.

Überladene Typmember müssen es sich um eindeutige Signaturen verfügen. Die Signatur eines Typmembers besteht aus der die Anzahl der Typparameter, und die Anzahl und Typen der Parameter für den Member. Konvertierungsoperatoren umfassen auch den Rückgabetyp des Operators, in der Signatur.

Im folgenden sind nicht Teil der Signatur eines Members, und daher kann nicht überladen werden, auf:

* Modifizierer auf einen Typmember (z. B. `Shared` oder `Private`).

* Modifizierer auf einen Parameter (z. B. `ByVal` oder `ByRef`).

* Die Namen der Parameter.

* Der Rückgabetyp einer Methode oder -Operator (außer den Konvertierungsoperatoren) oder den Typ des Elements, einer Eigenschaft.

* Einschränkungen für einen Typparameter.

Das folgende Beispiel zeigt eine Reihe von Deklarationen der überladenen Methode zusammen mit ihren Signaturen. Diese Deklaration wäre nicht gültig, da mehrere die Methodendeklarationen mit identische Signaturen haben.

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

Es ist zulässig, einen generischen Typ definieren, der mit identischen Signaturen basierend auf den angegebenen Typargumente Elemente enthalten kann. Regeln der überladungsauflösung werden zum Testen und zu unterscheiden zwischen solche Überladungen, obwohl es gibt möglicherweise Situationen, in denen es unmöglich, zu unterscheiden ist. Zum Beispiel:

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

## <a name="scope"></a>Bereich

Die *Bereich* einer Entität namens ist ein Satz von allen Deklaration Leerzeichen in dem es möglich ist, die auf diesen Namen ohne Qualifikation verweisen. Im Allgemeinen ist der Geltungsbereich eines Entitätsnamens der gesamte Deklarationskontext; die Deklaration einer Entität kann allerdings geschachtelte Deklarationen von Entitäten mit dem gleichen Namen enthalten. In diesem Fall die verschachtelter Entity *Shadows*, bzw. ausgeblendet, die äußere Entität und Zugriff auf die Entität Shadowing ist nur möglich, durch die Qualifizierung.

Shadowing über Schachtelung tritt auf, in Namespaces oder Typen in Namespaces geschachtelt, in andere Typen geschachtelten Typen und im Textkörper der Typmember. Shadowings durch die Schachtelung von Deklarationen immer auftritt implizit. Es ist keine explizite Syntax erforderlich.

Im folgenden Beispiel in der `F` -Methode, die Instanzvariable `i` wird durch die lokale Variable überschattet `i`, aber noch innerhalb der `G` -Methode, `i` weiterhin auf die Instanzvariable verweist.

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

Wenn Sie einen Namen in einem äußeren Bereich ein Namen in einem inneren Gültigkeitsbereich verdeckt führt Shadowing für es alle überladene Vorkommen dieses Namens. Im folgenden Beispiel der Aufruf `F(1)` Ruft die `F` in deklarierten `Inner` da alle äußeren Vorkommen von `F` werden durch die innere Deklaration ausgeblendet. Aus demselben Grund, den Aufruf `F("Hello")` ist fehlerhaft.

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

Eine vererbungsbeziehung wird in der ein Typ (der *abgeleitet* Typ) von einem anderen abgeleitet ist (die *Basis* Typ), sodass der abgeleitete Typ Deklarationsabschnitt implizit den zugegriffen werden kann enthält nicht-Konstruktor-Typmember und geschachtelte Typen von seinem Basistyp. Im folgenden Beispiel Klasse `A` ist die Basisklasse der `B`, und `B` ergibt sich aus `A`.

```vb
Class A
End Class

Class B
    Inherits A
End Class
```

Da `A` ist nicht explizit keine Basisklasse angeben, der Basisklasse ist implizit `Object`.

Im folgenden sind wichtige Aspekte bei der Vererbung:

* Vererbung ist transitiv. Wenn Typ *C* Typ abgeleitet ist *B*, und geben *B* Typ abgeleitet ist *ein*, Typ *C* erbt die Geben Sie im Typ deklarierten Member *B* sowie die Typmember in Typ deklariert *ein*.

* Ein abgeleiteter Typ erweitert, aber nicht einschränken, dessen Basistyp. Ein abgeleiteter Typ kann neue Member von Typen, hinzufügen und überschatten geerbten Typmember, aber die Definition eines Members des geerbten Typs kann nicht entfernt.

* Da eine Instanz eines Typs auf alle Typmember von seinem Basistyp enthält, ist immer eine Konvertierung eines abgeleiteten Typs in den Basistyp vorhanden.

* Alle Typen müssen einen Basistyp, mit Ausnahme des Datentyps `Object`. Daher `Object` ist die ultimative Basistyp aller Typen und alle Typen können konvertiert werden kann.

* Zirkularität in Ableitung ist nicht zulässig. D. h. wenn ein Typ `B` von einem Typ abgeleitet `A`, es ist ein Fehler für den Typ `A` direkt oder indirekt vom Typ ableiten `B`.

* Ein Typ kann nicht direkt oder indirekt von einem darin geschachtelten Typ abgeleitet werden.

Im folgenden Beispiel wird einen Fehler während der Kompilierung, da die Klassen zirkulär voneinander abhängig sind.

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

Im folgenden Beispiel wird auch einen Fehler während der Kompilierung, da `B` indirekt von der geschachtelten Klasse abgeleitet ist `C` über Klasse `A`.

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

Im nächste Beispiel ergibt keinen Fehler, da Klasse `A` nicht von der Klasse abgeleitet `B`.

```vb
Class A
    Class B
        Inherits A
    End Class 
End Class
```

### <a name="mustinherit-and-notinheritable-classes"></a>MustInherit und NotInheritable-Klassen

Ein `MustInherit` Klasse ist ein unvollständiger Typ, der nur als Basistyp verwendet werden kann. Ein `MustInherit` Klasse kann nicht instanziiert werden, daher wird ein Fehler, die `New` Operator auf eine. Es ist zulässig, die Variablen deklarieren, `MustInherit` Klassen; diese Variablen können nur zugewiesen werden `Nothing` oder ein Wert, der von einer Klasse abgeleitet ist die `MustInherit` Klasse.

Bei eine normale Klasse abgeleitet ist eine `MustInherit` -Klasse, die reguläre Klasse muss außer Kraft setzen alle geerbten `MustOverride` Member. Zum Beispiel:

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

Die `MustInherit` Klasse `A` führt eine `MustOverride` Methode `F`. Klasse `B` führt eine zusätzliche Methode `G`, jedoch keine Implementierung von `F`. Klasse `B` muss daher ebenfalls deklariert werden `MustInherit`. Klasse `C` überschreibt `F` und eine tatsächliche Implementierung bereitstellt. Da es sich um nicht ausstehende `MustOverride` Member in Klasse `C`, es ist nicht erforderlich sein `MustInherit`.

Ein `NotInheritable` ist eine Klasse, die von der eine andere Klasse kann nicht abgeleitet werden. `NotInheritable` Klassen werden in erster Linie zum Verhindern von unbeabsichtigten ableitungen.

In diesem Beispiel-Klasse `B` ist falsch, weil er versucht, für die Ableitung der `NotInheritable` Klasse `A`. Eine Klasse kann nicht markiert werden, beide `MustInherit` und `NotInheritable`.

```vb
NotInheritable Class A
End Class

Class B
    ' Error, a class cannot derive from a NotInheritable class.
    Inherits A
End Class
```

### <a name="interfaces-and-multiple-inheritance"></a>Schnittstellen und Mehrfachvererbung

Im Gegensatz zu anderen Typen, die nur von einem einzelnen Basistyp abgeleitet werden, kann eine Schnittstelle von mehreren Basisschnittstellen abgeleitet werden. Aus diesem Grund kann eine Schnittstelle einen mit dem gleichen Namen Typmember von unterschiedliche Basisschnittstellen erben. In diesem Fall die Multiplizieren Sie geerbte Name ist in der abgeleiteten Schnittstelle nicht verfügbar, und verweisen auf diesen Typ Member durch die abgeleitete Schnittstelle bewirkt, dass einen Fehler während der Kompilierung, unabhängig von Signaturen und überladen. In Konflikt stehende Member des Typs müssen stattdessen über einen Namen für die Basisschnittstelle verwiesen werden.

Im folgenden Beispiel die ersten beiden Anweisungen verursachen Kompilierzeitfehler, da der Multiplizieren Sie geerbte Member `Count` ist nicht verfügbar in der Schnittstelle `IListCounter`:

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

Wie im Beispiel gezeigt wird, wird die Mehrdeutigkeit aufgelöst, indem Sie eine Umwandlung `x` in den entsprechenden Basisschnittstelle-Typ. Solche Umwandlungen haben keine Laufzeit; Sie bestehen lediglich zum Anzeigen der Instanz als weniger abgeleiteten Typ zum Zeitpunkt der Kompilierung aus.

Wenn von der gleichen Basisschnittstelle über mehrere Pfade ein single-Typelmember geerbt wird, wird der Typmember behandelt, als ob sie nur einmal übernommen wurden. Das heißt, enthält die abgeleitete Schnittstelle nur eine Instanz jeder Typmember, die von einer bestimmten Basis Schnittstelle geerbt. Zum Beispiel:

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

Wenn der Name eines Typmembers, die in einem Pfad über die Hierarchie der klassenvererbung überschattet ist, wird der Name in allen Pfaden schattiert. Im folgenden Beispiel die `IBase.F` Member wird schattiert, durch die `ILeft.F` Member, aber wird nicht überschattet `IRight`:

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

Der Aufruf `d.F(1)` wählt `ILeft.F`, auch wenn `IBase.F` angezeigt wird, nicht in den Zugriffspfad ein Shadowing ausgeführt, das durch führt `IRight`. Da Zugriffspfad aus `IDerived` zu `ILeft` zu `IBase` Shadows `IBase.F`, das Element wird auch in den Zugriffspfad aus überschattet `IDerived` zu `IRight` zu `IBase`.

### <a name="shadowing"></a>Shadowing

Ein abgeleiteter Typ führt Shadowing für den Namen eines Members des geerbten Typs deklarieren Sie es erneut. Ein Name-Shadowing entfernt nicht die geerbten Typmember mit diesem Namen. Es werden alle geerbten Typmember mit diesem Namen nur in der abgeleiteten Klasse nicht verfügbar. Die shadowing-Deklaration kann jede Art von Entität sein.

Entitäten sind überladen werden können, können zwei Formen des shadowings auswählen. *Nach dem Namen Shadowing* angegeben ist, mit der `Shadows` Schlüsselwort. Eine Entität, die nach dem Namen Shadowing durchführt, wird alles, was mit diesem Namen in der Basisklasse, einschließlich aller Überladungen ausgeblendet. *Durch den Namen und derselben Signatur Shadowing* angegeben ist, mit der `Overloads` Schlüsselwort. Eine Entität, die durch den Namen und derselben Signatur führt Shadowing für alles, was mit diesem Namen mit der gleichen Signatur wie die Entität wird ausgeblendet. Zum Beispiel:

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

Shadowing einer Methode mit einem `ParamArray` Argument nach Namen und derselben Signatur verbirgt, nur die einzelne Signatur, die nicht alle möglichen erweiterten Signaturen. Dies gilt auch, wenn die Signatur der Methode shadowing der nicht erweiterte Signatur der Shadowing-Methode entspricht. Im folgenden Beispiel:

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

Druckt `Base`, auch wenn `Derived.F` hat die gleiche Signatur wie die nicht erweiterte Form der `Base.F`.

Im Gegensatz dazu eine Methode mit einem `ParamArray` Argument nur führt Shadowing für Methoden mit derselben Signatur nicht alle möglichen erweiterten Signaturen. Im folgenden Beispiel:

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

Druckt `Base`, auch wenn `Derived.F` verfügt über eine erweiterte Form, die die gleiche Signatur wie `Base.F`.

Ein shadowing-Methode oder Eigenschaft an, der nicht `Shadows` oder `Overloads` geht davon aus `Overloads` , wenn die Methode oder Eigenschaft deklariert ist `Overrides`, `Shadows` andernfalls. Wenn ein Mitglied einer Gruppe von überladenen Entitäten gibt an, die `Shadows` oder `Overloads` -Schlüsselwort, alle angegeben. Die `Shadows` und `Overloads` Schlüsselwörter können nicht gleichzeitig angegeben werden. Weder `Shadows` noch `Overloads` kann in einem Standardmodul angegeben werden; die Elemente in einem Standardmodul implizit von der geerbten Member Shadowing `Object`.

Es ist zulässig, die Schatten der Name eines Typmembers, wurde, die über schnittstellenvererbung multiply-geerbt (und dies ist dadurch nicht verfügbar), sodass der Name verfügbar in der abgeleiteten Schnittstelle.

Zum Beispiel:

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

Da es sich bei Methoden zu Schattenkopien geerbte Methoden zulässig sind, ist es möglich, für eine Klasse enthält mehrere `Overridable` Methoden, mit der gleichen Signatur. Dies ist keiner Mehrdeutigkeiten, da nur die am stärksten abgeleiteten Methode sichtbar ist. Im folgenden Beispiel die `C` und `D` Klassen enthalten zwei `Overridable` Methoden, mit der gleichen Signatur:

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

Es gibt zwei `Overridable` Methoden: eine von Klasse eingeführte `A` und die 1-von-Klasse eingeführt `C`. Die Methode, die von Klasse eingeführte `C` Blendet die Methode, die von der Klasse geerbt `A`. Daher die `Overrides` -Deklaration in der Klasse `D` überschreibt die Methode, die von Klasse eingeführte `C`, und es ist nicht möglich, für die Klasse `D` , die von Klasse eingeführte Methode überschreiben `A`. Das Beispiel erzeugt die Ausgabe:

```
B.F
B.F
D.F
D.F
```

Es ist möglich, rufen Sie die ausgeblendeten `Overridable` Methode, indem Sie den Zugriff auf eine Instanz der Klasse `D` über einen weniger abgeleiteten Typ in der die Methode nicht ausgeblendet ist.

Es ist nicht zulässig, für das Shadowing einer `MustOverride` -Methode, da in den meisten Fällen dies die Klasse nicht verwendbar machen würden. Zum Beispiel:

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

In diesem Fall die Klasse `MoreDerived` ist erforderlich, um das Überschreiben der `MustOverride` Methode `Base.F`, aber da die Klasse `Derived` Schatten `Base.F`, dies ist nicht möglich. Es gibt keine Möglichkeit, deklarieren Sie eine gültige untergeordnete Klasse der `Derived`.

Im Gegensatz zum shadowing eines Namens in einem äußeren Bereich, führt das shadowing eine barrierefreie Name aus einem geerbten Gültigkeitsbereich einer Warnung gemeldet werden, wie im folgenden Beispiel aus:

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

Die Deklaration der Methode `F` in Klasse `Derived` bewirkt, dass eine Warnung gemeldet werden. Shadowing einer geerbten Namens ist ausdrücklich keinen Fehler, da, die separate Entwicklung von Basisklassen ausgeschlossen würden. Z. B. die oben genannten Situation möglicherweise haben vorkommen, da eine neuere Version der Klasse `Base` eingeführt, eine Methode `F` , das war nicht in einer früheren Version der Klasse vorhanden. War der oben genannten Fall eines Fehlers *alle* Änderung an einer Basisklasse in einer separaten Versionen Klassenbibliothek kann dazu führen, abgeleitete Klassen ungültig werden.

Die Warnung verursacht hat, einen geerbten Namen Shadowing kann behoben werden, mithilfe der `Shadows` oder `Overloads` Modifizierer:

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

Die `Shadows` Modifizierer gibt an, dass den geerbten Member zu überschatten. Es ist kein Fehler an die `Shadows` oder `Overloads` Modifizierer ab, wenn kein Typ Membername zum Shadowing vorhanden ist.

Eine Deklaration eines neuen Members führt Shadowing für einen geerbten Member nur innerhalb des Bereichs des neuen Elements wie im folgenden Beispiel gezeigt:

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

Im obigen Beispiel ist die Deklaration der Methode `F` in Klasse `Derived` führt Shadowing für die Methode `F` , die aus der Klasse geerbt wurde `Base`, da die neue Methode aber `F` in Klasse `Derived` hat `Private` Zugriff auf ihren Bereich erstreckt sich nicht auf Klasse `MoreDerived`. Daher den Aufruf `F()` in `MoreDerived.G` gültig ist und ruft `Base.F`. Im Fall von überladenen Typmember wird der gesamte Satz von überladenen Typmember behandelt, als ob sie den am wenigsten restriktiven Zugriff für die Zwecke des shadowings mussten.

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

In diesem Beispiel, obwohl die Deklaration von `F()` in `Derived` mit deklariert `Private` für den Zugriff auf den überladenen `F(Integer)` mit deklariert `Public` Zugriff. Aus diesem Grund für die shadowing, des Namens `F` in `Derived` wird behandelt, als wäre es `Public`, also beide Methoden Schatten `F` in `Base`.

## <a name="implementation"></a>Implementierung

Ein *Implementierung* Beziehung vorhanden ist, wenn ein Typ deklariert, dass sie eine Schnittstelle implementiert, und der Typ alle Typmember der Schnittstelle implementiert. Ein Typ, der eine bestimmte Schnittstelle implementiert, die diese Schnittstelle konvertierbar ist. Schnittstellen können nicht instanziiert werden, aber es ist zulässig, deklarieren von Variablen von Schnittstellen; Diese Variablen können nur einen Wert zugewiesen werden, die einer Klasse, die die Schnittstelle implementiert. Zum Beispiel:

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

Ein Implementieren einer Schnittstelle mit Multiplizieren Sie geerbte Typmember muss diese Methoden, implementieren, obwohl diese direkt aus der abgeleiteten Schnittstelle implementiert wird nicht zugegriffen werden kann. Zum Beispiel:

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

Sogar `MustInherit` Klassen müssen alle Member der implementierten Schnittstellen Implementierungen bereitstellen; sie können jedoch Implementierung dieser Methoden verzögern, indem Sie sie als deklarieren `MustOverride`. Zum Beispiel:

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

Ein Typ können eine Schnittstelle neu zu implementieren, die der Basistyp implementiert. Um die Schnittstelle neu zu implementieren, muss den Typ explizit anzugeben, dass sie die Schnittstelle implementiert. Ein Typ, der erneut implementieren einer Schnittstelle können nur einige erneut zu implementieren, jedoch nicht alle, die Member der Schnittstelle – alle Mitglieder, die nicht erneut implementiert weiterhin des Basistyps Implementierung verwenden. Zum Beispiel:

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

In diesem Beispiel gibt:

```
TestDerived.DerivedTest1
TestBase.Test2
```

Wenn ein abgeleiteter Typ eine Schnittstelle implementiert, deren Basisschnittstellen von Basistypen von den abgeleiteten Typ implementiert werden, kann auch, dass der abgeleitete Typ nur die Typ-Member der Schnittstelle implementieren, die nicht bereits von den grundlegenden Typen implementiert werden. Zum Beispiel:

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

Eine Schnittstellenmethode kann auch über eine überschreibbare Methode in einem Basistyp implementiert werden. Ein abgeleiteter Typ kann in diesem Fall auch die überschreibbare Methode überschreiben und ändern die Implementierung der Schnittstelle. Zum Beispiel:

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

Ein Typ *implementiert* einen Typmember von einer implementierten Schnittstelle durch Angabe einer Methode mit einem `Implements` Klausel. Zwei-Typmember müssen die gleiche Anzahl von Parametern, aller Typen und Modifizierer der Parameter übereinstimmen, einschließlich der Standardwerte der optionalen Parameter, der Rückgabetyp muss übereinstimmen und alle Einschränkungen für Methodenparameter müssen übereinstimmen. Zum Beispiel:

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

Eine einzelne Methode kann eine beliebige Anzahl von Schnittstellentypmembern implementieren, wenn alle der oben genannten Kriterien erfüllen. Zum Beispiel:

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

Wenn Sie eine Methode in einer generischen Schnittstelle zu implementieren, muss die implementierende Methode Typargumente angeben, die Typparameter der Schnittstelle entsprechen. Zum Beispiel:

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

Beachten Sie, dass es möglich, dass eine generische Schnittstelle möglicherweise nicht für einen Satz von Typargumenten implementierbar.

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

*Polymorphismus* bietet die Möglichkeit, die die Implementierung einer Methode oder Eigenschaft variieren. Die gleiche Methode oder Eigenschaft kann mit Polymorphie unterschiedliche Aktionen abhängig von der Runtime-Typ der Instanz ausführen, den sie aufruft. Methoden oder Eigenschaften, die polymorph sind heißen *überschreibbare*. Im Gegensatz dazu ist die Implementierung einer nicht überschreibbare Methode oder Eigenschaft unveränderlich; die Implementierung ist identisch, ob die Methode oder Eigenschaft, auf einer Instanz der Klasse in aufgerufen wird der sie deklariert ist, oder eine Instanz einer abgeleiteten Klasse. Wenn eine nicht überschreibbare Methode oder Eigenschaft aufgerufen wird, ist die Kompilierzeit-Typ der Instanz der bestimmende Faktor. Zum Beispiel:

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

Eine überschreibbare Methode möglicherweise auch `MustOverride`, was bedeutet, dass es kein Methodentext enthält und überschrieben werden muss. `MustOverride` Methoden dürfen nur `MustInherit` Klassen.

Im folgenden Beispiel die Klasse `Shape` definiert die abstrakte Vorstellung einer geometrischen Formobjekt, das sich selbst zu zeichnen kann:

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

Die `Paint` Methode `MustOverride` , da es keine sinnvolle Standardimplementierung ist. Die `Ellipse` und `Box` Klassen sind konkrete `Shape` Implementierungen. Da diese Klassen, nicht sind `MustInherit`, müssen außer Kraft setzen der `Paint` Methode und eine tatsächliche Implementierung bereitstellen.

Es ist ein Fehler für Basiszugriff auf eine `MustOverride` -Methode wie im folgenden Beispiel veranschaulicht:

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

Ein Fehler wird gemeldet, für die `MyBase.F()` Aufruf, da er verweist auf eine `MustOverride` Methode.

### <a name="overriding-methods"></a>Überschreiben von Methoden

Ein Typ kann *überschreiben* eine geerbte überschreibbare Methode durch Deklarieren einer Methode mit dem gleichen Namen und Signatur aus, und die Deklaration mit der `Overrides` Modifizierer. Es gibt zusätzliche Anforderungen für die unten aufgeführten Methoden überschreiben. Während ein `Overridable` Deklaration der Methode führt eine neue Methode, eine `Overrides` Methodendeklaration ersetzt die geerbte Implementierung der Methode.

Kann die überschreibende Methode deklariert werden `NotOverridable`, die verhindert, dass Sie ein weiteres Überschreiben der Methode in abgeleiteten Typen. Tatsächlich `NotOverridable` Methoden werden keine weiteren nicht überschreibbaren in abgeleiteten Klassen.

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

In diesem Beispiel-Klasse `B` bietet zwei `Overrides` Methoden: eine Methode `F` , bei dem die `NotOverridable` -Modifizierer und eine Methode `G` , die jedoch nicht. Verwenden der `NotOverridable` Modifizierer wird verhindert, dass Klasse `C` von weiteren Überschreiben der Methode `F`.

Kann auch die überschreibende Methode deklariert werden `MustOverride`, auch wenn die Methode, die sie außer Kraft gesetzt wird, nicht deklariert ist `MustOverride`. Dies erfordert, dass die enthaltende Klasse deklariert werden `MustInherit` und eine weitere Klassen abgeleitet, die nicht deklariert werden `MustInherit` müssen die Methode überschreiben. Zum Beispiel:

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

In diesem Beispiel-Klasse `B` überschreibt `A.F` mit einem `MustOverride` Methode. Dies bedeutet, dass alle Klassen abgeleitet `B` außer Kraft setzen müssen `F`, es sei denn, sie deklariert werden `MustInherit` ebenfalls.

Ein Fehler während der Kompilierung tritt auf, es sei denn, alle der folgenden von der überschreibenden Methode erfüllt sind:

* Der Deklarationskontext enthält eine einzelne zugegriffen werden kann, geerbte Methode mit der gleichen Signatur und der Rückgabetyp (sofern vorhanden) als die überschreibende Methode.
* Die geerbte Methode überschrieben wird, ist überschreibbar. Das heißt, ist die geerbte Methode überschrieben wird nicht `Shared` oder `NotOverridable`.
* Die Zugriffsdomäne des deklarierten Methode entspricht die Zugriffsdomäne von der geerbten Methode überschrieben wird. Es gibt eine Ausnahme: ein `Protected Friend` Methode muss überschrieben werden, indem eine `Protected` Methode, wenn die andere Methode in einer anderen Assembly ist, die die überschreibende Methode nicht `Friend` Zugriff auf.
* Die Parameter der die überschreibende Methode übereinstimmen, außer Kraft gesetzte die Parameter der Methode im Hinblick auf die Verwendung von der `ByVal`, `ByRef`, `ParamArray,` und `Optional` Modifizierern, einschließlich der Werte für optionale Parameter bereitgestellt.
* Die Typparameter der überschreibenden Methode überein, die überschriebene Methode Typparameter im Hinblick auf Einschränkungen geben.

Wenn Sie eine Methode in einem generischen Basistyp zu überschreiben, muss die überschreibende Methode die Typargumente angeben, die den Basistyp Parametern entsprechen. Zum Beispiel:

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

Beachten Sie, dass es möglich, dass eine überschreibbare Methode in einer generischen Klasse möglicherweise nicht für einige Sätze von Typargumenten überschrieben werden können. Wenn die Methode deklariert wird `MustOverride`, dies bedeutet, dass einige Vererbungskette nicht möglich ggf. Zum Beispiel:

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

Eine Deklaration außer Kraft setzen, kann die überschriebene Basismethode Basiszugriff, wie im folgenden Beispiel mit zugreifen:

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

Im Beispiel des Aufrufs `MyBase.PrintVariables()` in Klasse `Derived` Ruft die `PrintVariables` Methode deklariert werden, in der Klasse `Base`. Basiszugriff deaktiviert die überschreibbare Aufrufmechanismus und behandelt einfach die Basismethode als nicht überschreibbare Methode. Hatte den Aufruf im `Derived` wurde geschrieben `CType(Me, Base).PrintVariables()`, wäre Sie rekursiv Aufrufen der `PrintVariables` Methode deklariert werden, `Derived`, nicht zu derjenigen, die in deklariert `Base`.

Wenn es enthält nur eine `Overrides` Modifizierer kann eine Methode eine andere Methode zu überschreiben. In allen anderen Fällen führt Shadowing für eine Methode mit der gleichen Signatur wie eine geerbte Methode einfach die geerbte Methode, wie im folgenden Beispiel:

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

Im Beispiel die Methode `F` in Klasse `Derived` enthält kein `Overrides` Modifizierer und aus diesem Grund wird die Methode nicht überschrieben `F` in Klasse `Base`. Stattdessen Methode `F` in Klasse `Derived` führt Shadowing für die Methode in Klasse `Base`, und eine Warnung wird ausgegeben, da die Deklaration keine umfasst eine `Shadows` oder `Overloads` Modifizierer.

Im folgenden Beispiel Methode `F` in Klasse `Derived` führt Shadowing für die überschreibbare Methode `F` Klasse vererbt `Base`:

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

Da die neue Methode `F` in Klasse `Derived` hat `Private` Zugriff, umfasst der Bereich nur des klassentexts von `Derived` und erstreckt sich nicht auf Klasse `MoreDerived`. Die Deklaration der Methode `F` in Klasse `MoreDerived` aus diesem Grund darf die Methode außer Kraft setzen `F` Klasse vererbt `Base`.

Wenn ein `Overridable` -Methode wird aufgerufen, die am weitesten abgeleitete Implementierung der Instanzenmethode aufgerufen wird, basierend auf dem Typ der Instanz an, unabhängig davon, ob der Aufruf der Methode in der Basisklasse oder die abgeleitete Klasse. Die am weitesten abgeleiteten Implementierung von einem `Overridable` Methode `M` in Bezug auf eine Klasse `R` wird wie folgt bestimmt:

* Wenn `R` enthält, die Einführung von `Overridable` Deklaration `M`, dies ist die am stärksten abgeleiteten Implementierung der `M`.

* Andernfalls gilt: Wenn `R` enthält eine Überschreibung der `M`, dies ist die am stärksten abgeleiteten Implementierung der `M`.

* Andernfalls am meisten Implementierung der abgeleiteten der `M` ist identisch mit der die direkte Basisklasse von `R`.

## <a name="accessibility"></a>Zugriff

Eine Deklaration gibt an, die *Barrierefreiheit* der Entität deklariert. Zugriff auf eine Entität ändert sich nicht auf den Rahmen eines Entitätsnamens aus. Die *Zugriffsdomäne* einer Deklaration ist ein Satz von allen Deklaration Leerzeichen in der die deklarierte Entität zugegriffen werden kann.

Die fünf Zugriffstypen werden `Public`, `Protected`, `Friend`, `Protected Friend`, und `Private`. `Public` ist die höchste Zugriffstyp und die vier anderen Typen sind alle Teilmengen von `Public`. Der am wenigsten eingeschränkte Zugriffstyp ist `Private`, und die vier anderen Zugriffstypen werden alle eine Obermenge der `Private`.

```antlr
AccessModifier
    : 'Public'
    | 'Protected'
    | 'Friend'
    | 'Private'
    | 'Protected' 'Friend'
    ;
```

Die für eine Deklaration angegeben wird über einen optionalen Zugriffsmodifizierer, die möglicherweise `Public`, `Protected`, `Friend`, `Private`, oder der Kombination von `Protected` und `Friend`. Wenn kein Zugriffsmodifizierer angegeben wird, hängt von der Standardtyp für den Zugriff der Deklarationskontext; die zulässigen Zugriffstypen hängt auch von der Deklarationskontext ab.

* Entitäten deklariert, mit der `Public` -Modifizierer aufweisen. `Public` Zugriff. Es gibt keine Einschränkungen bei der Verwendung von `Public` Entitäten.

* Entitäten deklariert, mit der `Protected` -Modifizierer aufweisen. `Protected` Zugriff. `Protected` Zugriff kann nur angegeben werden, bei Mitgliedern von Klassen (regulärer-Typmember und geschachtelte Klassen) oder auf `Overridable` Member von Standardmodulen und Strukturen (die muss, können Sie per Definition vererbt werden `System.Object` oder `System.ValueType`). Ein `Protected` Member ist auf eine abgeleitete Klasse zugegriffen werden kann, vorausgesetzt, dass der Member kein Instanzmember ist oder der Zugriff über eine Instanz der abgeleiteten Klasse erfolgt. `Protected` der Zugriff ist keine Obermenge der `Friend` Zugriff.

* Entitäten deklariert, mit der `Friend` -Modifizierer aufweisen. `Friend` Zugriff. Eine Entität mit `Friend` Zugriff ist nur innerhalb des Programms, das die Entitätsdeklaration oder alle Assemblys, die erteilt wurden enthält, zugänglich `Friend` über Zugriff auf die `System.Runtime.CompilerServices.InternalsVisibleToAttribute` Attribut.

* Entitäten deklariert, mit der `Protected Friend` Modifizierer verfügt, die Union der `Protected` und `Friend` Zugriff.

* Entitäten deklariert, mit der `Private` -Modifizierer aufweisen. `Private` Zugriff. Ein `Private` Entität kann nur innerhalb der Deklarationskontext, einschließlich der Entitäten, geschachtelte zugegriffen werden.

Der Zugriff auf in einer Deklaration ist nicht auf die Barrierefreiheit der Deklarationskontext abhängig. Z. B. ein Typ deklariert, mit `Private` Zugriff eventuell einen Typmember mit `Public` Zugriff.

Der folgende Code veranschaulicht verschiedene Eingabehilfen-Domänen:

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

Die Klassen und Member in diesem Beispiel haben die folgenden Eingabehilfen-Domänen:

* Die Zugriffsdomäne von `A` und `A.X` ist unbegrenzt.

* Die Zugriffsdomäne von `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, und `B.C.Y` ist das Programm enthält.

* Die Zugriffsdomäne von `A.Z` ist `A.`

* Die Zugriffsdomäne von `B.Z`, `B.D`, `B.D.X`, und `B.D.Y` ist `B`, einschließlich `B.C` und `B.D`.

* Die Zugriffsdomäne von `B.C.Z` ist `B.C`.

* Die Zugriffsdomäne von `B.D.Z` ist `B.D`.

Wie im Beispiel wird veranschaulicht, ist die Zugriffsdomäne eines Members nie größer als die des enthaltenden Typs. Beispielsweise, obwohl alle `X` Mitglieder `Public` deklariert der Barrierefreiheit, gut `A.X` haben Sie auf Zugriffsdomänen, die von einem enthaltenden Typ eingeschränkt werden.

Der Zugriff auf `Protected` Member durch eine Instanz des abgeleiteten Typs sein müssen, damit nicht verknüpfte Typen kein Zugriff auf jede andere Instanz hat geschützte Member. Zum Beispiel:

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

Im obigen Beispiel die Klasse `Guest` hat lediglich Zugriff auf die geschützte `Password` Feld, wenn es mit einer Instanz von qualifiziert wird `Guest`. Dies verhindert, dass `Guest` vom Zugriff auf die `Password` Feld eine `Employee` Objekt einfach durch die Umwandlung zu `User`.

Zum Zweck der `Protected` Memberzugriff in generischen Typen der Deklarationskontext Typparameter enthält. Dies bedeutet, dass ein abgeleiteter Typ mit einem einzelnen Satz von Typargumenten keinen Zugriff auf die `Protected` Member eines abgeleiteten Typs mit einem anderen Satz von Typargumenten. Zum Beispiel:

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

__Beachten Sie.__ Der C#-Sprache (und möglicherweise andere Sprachen) ermöglicht den Zugriff auf einen generischen Typ `Protected` Member unabhängig vom Typargumente angegeben werden. Dies sollte Bedenken gehalten werden, beim Entwerfen generischer Klassen, die enthalten `Protected` Member.


### <a name="constituent-types"></a>Enthaltenen Typen

Die *enthaltenen Typen* einer Deklaration sind die Typen, die von der Deklaration verwiesen werden. Der Typ, der eine Konstante, die den Rückgabetyp einer Methode und die Parametertypen eines Konstruktors sind beispielsweise alle enthaltenen Typen. Die Zugriffsdomäne eines einzelnen Typs einer Deklaration muss identisch oder eine Obermenge der Zugriffsdomäne von der Deklaration selbst sein. Zum Beispiel:

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

## <a name="type-and-namespace-names"></a>Namespace-Namen und Typ

Viele der Sprachkonstrukte erfordern einen Namespace oder Typ angegeben werden. Diese können über eine qualifizierte Form, der den Namespace oder den Namen des Typs angegeben werden. Ein *qualifizierten Namen* besteht aus einer Reihe von Bezeichner durch Punkte getrennt sind; der Bezeichner auf der rechten Seite eines Zeitraums wird in der Deklaration Speicherplatz auf der linken Seite des Zeitraums vom Bezeichner angegebene aufgelöst.

Die *voll gekennzeichneten Namen* der einen Namespace oder Typ ist ein qualifizierter Name mit dem Namen aller enthält, Namespaces und Typen. Das heißt, der vollqualifizierte Name eines Namespace oder Typ ist `N.T`, wobei `T` ist der Name der Entität und `N` ist der vollqualifizierte Name der ihn enthaltenden Entität.

Das folgende Beispiel zeigt mehrere Deklarationen von Namespace und den Typ zusammen mit den zugehörigen voll qualifizierten Namen in den Inline-Kommentaren.

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

Beachten Sie, dass der Namespace X.Y an zwei verschiedenen Speicherorten im Quellcode deklariert wurde, aber diese beiden partiellen Deklarationen bilden nur einen einzelnen Namespace mit dem Namen X.Y enthält sowohl Klasse D und e-Klasse

In einigen Fällen kann ein qualifizierter Name mit dem Schlüsselwort beginnen `Global`. Das Schlüsselwort stellt den äußersten unbenannten Namespace, der eignet sich für Situationen, in denen eine Deklaration führt Shadowing für eine einschließende Namespace, dar. Die `Global` Schlüsselwort, "Schutz" auf den äußeren Namespace in dieser Situation kann. Zum Beispiel:

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

Im obigen Beispiel ist der erste Methodenaufruf ungültig da der Bezeichner `System` auf die Klasse bindet `System`, nicht den Namespace `System`. Die einzige Möglichkeit zum Zugriff auf die `System` Namespace ist die Verwendung `Global` , dem äußeren Namespace mit Escapezeichen versehen. `Global` kann nicht verwendet werden, eine `Imports` Anweisung oder `Namespace` Deklaration.

Da es sich bei anderen Sprachen einführen können, Typen und Namespaces, die Schlüsselwörter in der Sprache entsprechen, erkennt Visual Basic Schlüsselwörter aufgelistet, die Teil eines qualifizierten Namens werden, solange sie einen Punkt folgen. Schlüsselwörter, die auf diese Weise verwendet werden als Bezeichner behandelt. Z. B. den vollqualifizierten Bezeichner `X.Default.Class` ist ein gültiger qualifizierter Bezeichner, während `Default.Class` nicht.

### <a name="qualified-name-resolution-for-namespaces-and-types"></a>Qualifizierte namensauflösung für Namespaces und Typen

Einen qualifizierten Namespace oder Typ-Name des Formulars angegeben `N.R(Of A)`, wobei `R` der äußersten rechten Bezeichner im qualifizierten Namen und `A` ist eine optionale Liste der Typargumente, die folgenden Schritte beschreiben, wie Sie bestimmen, welcher Namespace oder Geben Sie den vollqualifizierten Namen verweist:

1. Beheben `N`, gemäß den Regeln für entweder qualifizierte oder nicht qualifizierte namensauflösung.

2. Wenn Auflösung `N` auf einen Typparameter, löst ein Fehler während der Kompilierung tritt ein, oder ein Fehler auftritt.

3. Andernfalls gilt: Wenn `R` entspricht der Name eines Namespace in N und keine Typargumente wurden angegeben, oder `R` entspricht einen zugreifbarer Typ im `N` mit der gleichen Anzahl von Typparametern als Typargumente, sofern vorhanden, klicken Sie dann der qualifizierte Namen bezieht sich auf Dieser Namespace oder Typ.

4. Andernfalls gilt: Wenn `N` enthält eine oder mehrere standard-Module, und `R` der Name des ein zugreifbarer Typ mit der gleichen Anzahl von Typparametern als Typargumente, abgeglichen werden, wenn sich ggf. in genau einem Standardmodul, und klicken Sie dann auf den qualifizierten Namen bezieht, Geben Sie ein. Wenn `R` dem Namen der verfügbaren Typen mit der gleichen Anzahl von Typparametern als Typargumente, entspricht, wenn eine in mehr als eine standard-Modul einen Fehler während der Kompilierung auftritt.

5. Andernfalls tritt ein Kompilierungsfehler auf.

__Beachten Sie.__ Eine Auswirkung dieses Prozesses für die Lösung ist, dass es sich bei Typmember nicht Namespaces oder Typen beim Auflösen von Namen für Namespace oder Typ überschatten.

### <a name="unqualified-name-resolution-for-namespaces-and-types"></a>Nicht qualifizierte namensauflösung für Namespaces und Typen

Nicht qualifizierte Namen `R(Of A)`, wobei `A` ist eine optionale Liste der Typargumente, die folgenden Schritte beschreiben, wie Sie bestimmen können, welcher Namespace oder Typ der nicht qualifizierte Name bezieht sich:

1. Wenn R, den Namen der Typparameter der aktuellen Methode entspricht, und es wurden keine Typargumente bereitgestellt, verweist der nicht qualifizierte Name an.

2.  Für jede geschachtelte Typ mit dem Namensverweis, beginnend mit des innersten Typs und zum äußersten:
    1. Wenn `R` übereinstimmt, den Namen eines Typparameters in der aktuelle Typ und kein Typ Argumente bereitgestellt wurden, der nicht qualifizierte Name auf den Typparameter verweist.
    2. Andernfalls gilt: Wenn `R` Übereinstimmungen, die der Namen des eine zugängliche Typ mit der gleichen Anzahl von geschachtelter Typparametern als Typargumente, sofern vorhanden, und klicken Sie dann der nicht qualifizierte Namen bezieht sich auf diesen Typ.

3. Für jeden geschachtelten Namespace, der die Namensverweis, beginnend mit des innersten-Namespaces und dem äußeren Namespace navigieren enthält:
    1. Wenn `R` Übereinstimmungen, die der Namen der einem geschachtelten Namespace im aktuellen Namespace und keine Liste der Typargumente wird, angegeben, wird der nicht qualifizierte Name auf diesen geschachtelten Namespace verweist.
    2. Andernfalls gilt: Wenn `R` der ein zugreifbarer Typ mit der gleichen Anzahl von Typparametern als Typargumente, wenn vorhanden, im aktuellen Namespace, dann lautet der nicht qualifizierte Name bezieht sich auf diesen Typ entspricht.
    3. Wenn der Namespace, eine oder mehrere zugänglich Standardmodulen enthält, andernfalls und `R` entspricht Sie den Namen eines verfügbaren geschachtelten Typs mit der gleichen Anzahl von Typparametern als Typargumente, ggf. in genau einem Standardmodul, und klicken Sie dann den nicht qualifizierten Name bezieht sich auf dieses geschachtelten Typs. Wenn `R` der Name des zugegriffen werden kann geschachtelte Typen, mit der gleichen Anzahl von Typparametern als Typargumente, abgeglichen werden, wenn eine in mehr als eine standard-Modul einen Fehler während der Kompilierung auftritt.

4. Wenn die Quelldatei ein oder mehrere Importaliase verfügt und `R` mit dem übereinstimmt, der nicht qualifizierte Name auf den Importalias verweist. Wenn eine Liste der Typargumente angegeben wird, tritt ein Fehler während der Kompilierung.

5. Wenn die Quelldatei, die mit dem Namensverweis ein oder mehrere Importe aufweist:
    1. Wenn `R` der ein zugreifbarer Typ mit der gleichen Anzahl von Typparametern als Typargumente, ggf. in genau eine importieren, und klicken Sie dann auf den nicht qualifizierten Namen bezieht sich auf diesen Typ entspricht. Wenn `R` der Name des ein zugreifbarer Typ mit der gleichen Anzahl von Typparametern als Typargumente, abgeglichen werden, wenn ggf. in mehrere Importvorgänge und alle nicht denselben Typ, sind ein Fehler während der Kompilierung auftritt.
    2. Wenn keine Liste der Typargumente angegeben wurde, andernfalls und `R` verfügbaren Typen in genau einem Import, und klicken Sie dann der nicht qualifizierte Namen, Namespace bezieht sich der Name eines Namespace entspricht. Wenn keine Liste der Typargumente angegeben wurde und `R` Übereinstimmungen, die der Namen eines Namespaces mit verfügbaren Typen in mehr als einem Import und alle sind nicht denselben Namespace, ein Fehler während der Kompilierung auftritt.
    3. Andernfalls, wenn die Importe eine oder mehrere zugänglich Standardmodule, enthalten und `R` entspricht, den Namen der verfügbaren geschachtelten Typs mit der gleichen Anzahl von Typparametern als Typargumente, die ggf. in genau einem Standardmodul, und klicken Sie dann den nicht qualifizierten Namen bezieht sich auf diesen Typ. Wenn `R` der Name des zugegriffen werden kann geschachtelte Typen, mit der gleichen Anzahl von Typparametern als Typargumente, abgeglichen werden, wenn eine in mehr als eine standard-Modul einen Fehler während der Kompilierung auftritt.

6. Wenn die kompilierungsumgebung eine oder mehrere Importaliase definiert und `R` mit dem übereinstimmt, der nicht qualifizierte Name auf den Importalias verweist. Wenn eine Liste der Typargumente angegeben wird, tritt ein Fehler während der Kompilierung.

7. Wenn der kompilierungsumgebung ein oder mehrere Importe definiert:
    1. Wenn `R` der ein zugreifbarer Typ mit der gleichen Anzahl von Typparametern als Typargumente, ggf. in genau eine importieren, und klicken Sie dann auf den nicht qualifizierten Namen bezieht sich auf diesen Typ entspricht. Wenn `R` der Name des ein zugreifbarer Typ mit der gleichen Anzahl von Typparametern als Typargumente, abgeglichen werden, wenn eine in mehr als ein Import ein Fehler während der Kompilierung auftritt.
    2. Wenn keine Liste der Typargumente angegeben wurde, andernfalls und `R` verfügbaren Typen in genau einem Import, und klicken Sie dann der nicht qualifizierte Namen, Namespace bezieht sich der Name eines Namespace entspricht. Wenn keine Liste der Typargumente angegeben wurde und `R` entspricht der Name eines Namespace mit verfügbaren Typen in mehr als einem Import ein Fehler während der Kompilierung auftritt.
    3. Andernfalls, wenn die Importe eine oder mehrere zugänglich Standardmodule, enthalten und `R` entspricht, den Namen der verfügbaren geschachtelten Typs mit der gleichen Anzahl von Typparametern als Typargumente, die ggf. in genau einem Standardmodul, und klicken Sie dann den nicht qualifizierten Namen bezieht sich auf diesen Typ. Wenn `R` der Name des zugegriffen werden kann geschachtelte Typen, mit der gleichen Anzahl von Typparametern als Typargumente, abgeglichen werden, wenn eine in mehr als eine standard-Modul einen Fehler während der Kompilierung auftritt.

8. Andernfalls tritt ein Kompilierungsfehler auf.

__Beachten Sie.__ Eine Auswirkung dieses Prozesses für die Lösung ist, dass es sich bei Typmember nicht Namespaces oder Typen beim Auflösen von Namen für Namespace oder Typ überschatten.

Normalerweise kann ein Namen in einem bestimmten Namespace nur einmal vorkommen. Da Namespaces mehreren .NET Assemblys deklariert werden können, ist es jedoch möglich, dass eine Situation, in denen zwei Assemblys für einen Typ mit den gleichen vollqualifizierten Namen definieren. In diesem Fall wird ein Typ, der deklariert, die in den aktuellen Satz von Quelldateien über einem Typ deklariert, die in einer externen .NET-Assembly bevorzugt. Andernfalls der Name ist mehrdeutig, und es gibt keine Möglichkeit, um den Namen zu unterscheiden.

## <a name="variables"></a>Variablen

Ein *Variable* stellt einen Speicherort. Jede Variable hat einen Typ, der bestimmt, welche Werte können in der Variablen gespeichert. Da Visual Basic eine typsicheren Sprache ist, jede Variable in einem Programm verfügt über einen Typ und die Sprache wird sichergestellt, dass die Werte in gespeichert sind Variablen immer des entsprechenden Typs. Variablen werden immer auf den Standardwert von ihrem Typ initialisiert, bevor alle Verweise auf die Variable, die ausgeführt werden kann. Es ist nicht möglich ist, nicht initialisierten Speicher zuzugreifen.

## <a name="generic-types-and-methods"></a>Generische Typen und Methoden

Typen (außer Standardmodulen und Enumerationstypen) und Methoden deklarieren können *Typparameter*, welche Typen sind, die nicht, bis eine Instanz des Typs bereitgestellt werden deklariert ist, oder die Methode wird aufgerufen. Typen und Methoden mit den beiden Typparametern werden auch bezeichnet als *generische Typen* und *generische Methoden*, da der Typ oder die Methode generisch, ohne spezifische Kenntnisse geschrieben werden muss die Typen, die durch Code angegeben werden, die den Typ oder eine Methode verwendet.

__Beachten Sie.__ Zu diesem Zeitpunkt, obwohl die Methoden und Delegaten können generisch ist, werden Eigenschaften, Ereignisse und Operatoren können nicht generisch sein selbst. Sie können jedoch die Typparameter von der enthaltenden Klasse verwenden.

Aus der Perspektive des generischen Typs oder -Methode ist ein Typparameter ein Platzhaltertyp, der mit einem tatsächlichen Typ gefüllt werden wird, wenn der Typ oder eine Methode verwendet wird. Typargumente werden ersetzt die Typparameter in den Typ oder Methode an dem Punkt, an dem der Typ oder eine Methode verwendet wird. Beispielsweise kann eine generische Stapelklasse als implementiert werden:

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

Deklarationen, mit denen die `Stack(Of ItemType)` Klasse muss für den Typparameter ein Typargument angeben `ItemType`. Dieser Typ wird dann ausgefüllt, wo `ItemType` wird verwendet, in der Klasse:

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

Typparameter können auf Typ oder Methodendeklarationen, angegeben werden. Jeden von Typparameter ist, einen Bezeichner für ein Argument vom Typ ein Platzhalter, der zum Erstellen eines konstruierten Typs oder der Methode bereitgestellt wird. Im Gegensatz dazu ist ein Argument vom Typ der tatsächliche Typ, der für den Typparameter ersetzt wird, wenn ein generischer Typ oder eine Methode verwendet wird.

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

Jeder Typparameter in einen Typ oder eine Methodendeklaration definiert einen Namen im Deklarationsbereich dieses Typs oder der Methode. Daher keine es den gleichen Namen wie einen anderen Typparameter, einen Typmember, einen Methodenparameter oder eine lokale Variable. Der Bereich eines Typparameters auf einen Typ oder Methode ist der gesamte Typ oder Methode. Da der Typparameter für die gesamte Typdeklaration definiert sind, können geschachtelte Typen äußeren Typ-Parameter verwenden. Dies bedeutet auch, dass der Parameter vom Typ müssen immer angegeben werden, wenn Sie den Zugriff auf Typen, die in generischen Typen geschachtelt:

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

Im Gegensatz zu anderen Member einer Klasse werden die Parameter vom Typ nicht geerbt. Der Typparameter in einem Typ können nur mit ihrem einfachen Namen verwiesen werden; Das heißt, sie mit dem enthaltenden Typnamen qualifizierten nicht möglich. Obwohl das Gegenteil können Mitglied Programmierstil, die Typparameter in einem geschachtelten Typ oder ausblenden Typparameter des äußeren Typs deklariert:

```vb
Class Outer(Of T)
    Class Inner(Of T)
        Public t1 As T    ' Refers to Inner's T
    End Class
End Class
```

Typen und Methoden, möglicherweise basierend auf der Anzahl der Typparameter überladen werden (oder *Stelligkeit*), die die Typen oder Methoden zu deklarieren. Beispielsweise sind die folgenden Deklarationen zulässig:

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

Bei Typen sind Überladungen für die Anzahl der angegebenen Typargumente immer zugeordnet. Dies ist hilfreich, wenn Sie generische und nicht generischen Klassen zusammen im selben Programm verwenden:

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

Regeln für Methoden, die für Typparameter überladen werden im Abschnitt zur überladungsauflösung behandelt.

Innerhalb der enthaltenden Deklaration gelten als Typparameter vollständige Typen. Da ein Typparameter mit vielen verschiedenen tatsächliche Typargumenten instanziiert werden kann, über Typparameter verfügen, leicht unterschiedliche Vorgänge und Einschränkungen als andere Arten, wie im folgenden beschrieben:

* Ein Typparameter kann nicht verwendet werden, direkt auf eine Basisklasse oder Schnittstelle zu deklarieren.

* Die Regeln für die Suche nach Membern auf, dass die Parameter der Einschränkungen, sofern vorhanden, abhängig, die auf den Typparameter angewendet werden.

* Die verfügbaren Konvertierungen für ein Typparameter hängen von den Einschränkungen, die ggf. auf die Typparameter angewendet.

* In Ermangelung einer `Structure` Einschränkung ein Wert mit einem Typ, dargestellt durch einen Typparameter kann verglichen werden mit `Nothing` mit `Is` und `IsNot`.

* Ein Typparameter kann nur verwendet werden, eine `New` Ausdruck, wenn der Typparameter, durch eingeschränkt wird eine `New` oder ein `Structure` Einschränkung.

* Ein Typparameter kann nicht an einer beliebigen Stelle verwendet werden, in eine Attribut-Ausnahme in einem `GetType` Ausdruck.

* Typparameter können als Typargumente auf andere generische Typen und Parametern verwendet werden.

Im folgende Beispiel ist ein generischer Typ, der erweitert die `Stack(Of ItemType)` Klasse:

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

Wenn eine Deklaration ein Typargument stellt `MyStack`, dasselbe Typargument gelten für `Stack` ebenfalls.

Als Typ sind Typparameter ausschließlich ein Kompilierzeit-Konstrukt. Zur Laufzeit ist jeden von Typparameter auf einen Typ zur Laufzeit gebunden, die durch Angabe von Typargument für generischen Deklaration angegeben wurde. Daher wird der Typ einer Variablen mit einem Typparameter deklariert zur Laufzeit, einen nicht generischen Typ oder einen bestimmten konstruierten Typ sein. Die Ausführung zur Laufzeit alle Anweisungen und Ausdrücke, die im Zusammenhang mit Typparametern verwendet den tatsächlichen Typ, der übergeben wurde, als Typargument für diesen Parameter.


### <a name="type-constraints"></a>Typeinschränkungen

Da ein Typargument einen beliebigen Typ im Typsystem sein kann, kann nicht keine Annahmen über einen Typparameter einer generischen Typ- oder stellen werden. Daher gelten die Elemente eines Typparameters werden die Elemente des Typs `Object`, da alle Typen abgeleitet `Object`.

Im Fall einer Sammlung wie `Stack(Of ItemType)`, dies möglicherweise nicht besonders wichtig, Einschränkung, aber es gibt möglicherweise Fälle, in denen ein generischer Typ eine Annahme über die Typen machen wollen, die als Typargumente angegeben. *Typeinschränkungen* von Typparametern, die einschränken, welche Typen wie ein Typparameter angegeben werden, und Zulassen von generischen Typen oder Methoden mehr über den Typparameter annehmen platziert werden kann.

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

In diesem Beispiel die `DisposableStack(Of ItemType)` schränkt den Typparameter nur Typen, die die Schnittstelle implementieren `System.IDisposable`. Daher können sie implementieren eine `Dispose` -Methode, die alle Objekte frei bleibt weiterhin in der Warteschlange.

Eine Einschränkung für einen muss eine der speziellen Einschränkungen `Class`, `Structure`, oder `New`, oder es muss ein Typ `T` , in denen:

* `T` ist eine Klasse, Schnittstelle oder ein Typparameter.

* `T` ist nicht `NotInheritable`.

* `T` ist kein oder ein Typ von einem der folgenden speziellen Typen geerbt: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, oder `System.ValueType`.

* `T` ist nicht `Object`. Da alle Typen abgeleitet `Object`, eine solche Einschränkung hätte keine Auswirkung, wenn sie zulässig wären.

* `T` muss mindestens dieselben zugriffsmöglichkeiten bieten wie die generischen Typ oder Methode, die deklariert wird.

Mehrere Einschränkungen können für einen einzelnen Typparameter angegeben werden, indem typeinschränkungen in geschweiften Klammern einschließen (`{}`)... Nur eine Einschränkung für einen bestimmten Typ-Parameter kann eine Klasse sein. Es ist ein Fehler zum Kombinieren einer `Structure` speziellen Einschränkung mit einer Einschränkung der benannten Klasse oder die `Class` speziellen Einschränkung.

```vb
Class ControlFactory(Of T As {Control, New})
    ...
End Class
```

Einschränkungen können die enthaltenen Typen oder eines der Typparameter der enthaltenden Typen. Im folgenden Beispiel erfordert die Einschränkung an, dass das angegebene Typargument eine generische Schnittstelle mit sich selbst als Typargument implementiert:

```vb
Class Sorter(Of V As IComparable(Of V))
    ...
End Class
```

Die spezielle typeinschränkung `Class` schränkt das angegebene Typargument beliebige Verweistypen.

__Beachten Sie.__ Die spezielle typeinschränkung `Class` eine-Schnittstelle erfüllt werden kann. Und eine Struktur kann eine Schnittstelle implementieren. Daher die Einschränkung `(Of T As U, U As Class)` kann mit "T" einer Struktur zufrieden sein (dem erfüllt nicht die `Class` speziellen Einschränkung), und "U" eine Schnittstelle, die er implementiert (erfüllt die der `Class` speziellen Einschränkung).

Die spezielle typeinschränkung `Structure` schränkt das angegebene Typargument jeder Werttyp außer `System.Nullable(Of T)`.

__Beachten Sie.__ Struktur-Einschränkungen lassen keine `System.Nullable(Of T)` so, dass es nicht möglich, geben Sie `System.Nullable(Of T)` als Typargument an sich selbst.

Die spezielle typeinschränkung `New` erfordert, dass das angegebene Typargument einen zugänglichen parameterlosen Konstruktor aufweisen muss und kann nicht deklariert werden `MustInherit`. Zum Beispiel:

```vb
Class Factory(Of T As New)
    Function CreateInstance() As T
        Return New T()
    End Function
End Class
```

Einschränkung für einen Klassentyp ist erforderlich, dass das angegebene Typargument entweder befinden, geben Sie als die oder von ihm erben. Eine schnittstelleneinschränkung-Typ erfordert, dass das angegebene Typargument diese Schnittstelle implementieren muss. Eine Einschränkung für einen Parameter erfordert, dass das angegebene Typargument abgeleitet werden muss, oder alle die Grenzen für den entsprechenden Typparameter angegeben implementieren. Zum Beispiel:

```vb
Class List(Of T)
    Sub AddRange(Of S As T)(collection As IEnumerable(Of S))
        ...
    End Sub
End Class
```

In diesem Beispiel ist der Typparameter `S` auf `AddRange` ist beschränkt auf den Typparameter `T` von `List`. Dies bedeutet, dass eine `List(Of Control)` einschränken würden `AddRange`der Typparameter in einen beliebigen Typ, der oder erbt von `Control`.

Eine Einschränkung für einen Parameter `Of S As T` wird aufgelöst, indem transitiv Hinzufügen aller `T`Einschränkungen auf `S`, die keine besonderen Einschränkungen (`Class`, `Structure`, `New`). Es ist ein Fehler, wenn zirkuläre Einschränkungen (z. B. `Of S As T, T As S`). Es ist ein Fehler, die eine Einschränkung für einen Parameter haben die selbst verfügt über die `Structure` Einschränkung. Nach dem Hinzufügen von Einschränkungen, ist es möglich, dass eine Anzahl von Situationen auftreten:

* Wenn mehrere klasseneinschränkungen vorhanden ist, die am stärksten abgeleitete Klasse gilt die Einschränkung. Wenn eine oder mehrere klasseneinschränkungen keine vererbungsbeziehung haben, wird die Einschränkung ist unmöglich machen, und es ist ein Fehler.

 * Wenn ein Typparameter kombiniert eine `Structure` speziellen Einschränkung mit einer Einschränkung der benannten Klasse oder die `Class` speziellen Einschränkung ist ein Fehler. Eine Class-Einschränkung möglicherweise `NotInheritable`, in diesem Fall keine abgeleiteten Typen gegen diese Einschränkung werden akzeptiert, und es ist ein Fehler.

Der Typ kann eine der sein oder ein Typ aus, die folgenden speziellen Typen geerbt: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, oder `System.ValueType`. In diesem Fall wird nur der Typ oder einen Typ geerbt, akzeptiert. Ein Typparameter, beschränkt auf einem dieser Typen kann nur die Konvertierungen zulässig verwenden die `DirectCast` Operator. Zum Beispiel:

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

Darüber hinaus kann kein Typparameter, der auf einen Werttyp aufgrund eines der oben genannten lockerungen für die eingeschränkte alle auf diesem Typ definierten Methoden aufrufen. Zum Beispiel:

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

Wenn die Einschränkung, nach der Ersetzung als eines Arraytyps am Ende, ist ebenfalls ein kovariant Arraytyp zulässig. Zum Beispiel:

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

Ein Typparameter mit einer Klassen- oder schnittstelleneinschränkung haben dieselben Member wie diese Klasse oder Schnittstelle Einschränkung gilt. Wenn ein Typparameter mehrere Einschränkungen verfügt, gilt der Typparameter die Vereinigung aller Elemente der Einschränkungen haben. Wenn Elemente mit dem gleichen Namen in mehr als eine Einschränkung vorhanden sind, und klicken Sie dann Elemente, in der folgenden Reihenfolge ausgeblendet werden: die klasseneinschränkung Blendet Member in der schnittstelleneinschränkungen an, die Ausblenden von Elementen in `System.ValueType` (Wenn `Structure` Einschränkung angegeben ist), der Member in verborgen `Object`. Wenn ein Element mit dem gleichen Namen in mehr als eine schnittstelleneinschränkung angezeigt wird der Member ist nicht verfügbar ist (wie in der mehrfachvererbung-Schnittstelle), und der Type-Parameter muss in die gewünschte Schnittstelle umgewandelt werden. Zum Beispiel:

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

Wenn der Typparameter als Typargumente angeben, müssen die Typparameter der Einschränkungen der entsprechende Typparameter erfüllen.

```vb
Class Base(Of T As Class)
End Class

Class Derived(Of V)
    ' Error: V does not satisfy the constraints of T
    Inherits Base(Of V)
End Class
```

Werte einen eingeschränkten Typparameter können verwendet werden, auf die Instanz-Elemente, einschließlich Instanzmethoden zur Verfügung, in der Einschränkung angegeben.

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

### <a name="type-parameter-variance"></a>Varianz der Type-Parameter

Ein Typparameter eine Schnittstelle oder eine Typdeklaration für den Delegaten kann optional angeben, ein *Varianz Modifizierer*. Typparameter mit varianzmodifizierer einschränken, wie der Typparameter in der Schnittstellen oder Delegate-Typ verwendet werden kann, aber einen generischen Schnittstellen oder Delegate-Typ in einem anderen generischen Typ mit Argumenten der variant-kompatibler Typ konvertiert werden können. Zum Beispiel:

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

Generische Schnittstellen, die über Typparameter mit varianzmodifizierer weisen einige Einschränkungen auf:

* Eine Ereignisdeklaration, die angibt, eine Parameterliste darf nicht enthalten (aber eine benutzerdefinierte Ereignisdeklaration oder eine Ereignisdeklaration mit einem Delegattypen ist zulässig).

* Sie können keine geschachtelte Klasse, Struktur oder enumerierten Typ enthalten.

__Beachten Sie.__ Diese Einschränkungen sind darauf zurückzuführen, dass Typen, die in generischen Typen geschachtelt sind, implizit die generischen Parameter ihres übergeordneten Elements zu kopieren. Im Fall von geschachtelten Klassen, Strukturen oder Enumerationstypen diese Arten von Typen varianzmodifizierer für ihre Typparameter nicht möglich. Im Fall einer Ereignisdeklaration mit einer Parameterliste, die generierten geschachtelte Delegate-Klasse möglicherweise verwirrend erscheinen Fehler, wenn ein Typ, der angezeigt wird, die in verwendet werden eine `In` Position (d. h. einen Parametertyp) wird verwendet, eine `Out` Position (d. h. die Typ des Ereignisses).

Ein Typparameter, die mit der Out-Modifizierer deklariert wird ist *kovariant*. Informell wird ein kovarianter Typparameter kann nur in einer Ausgabeposition – d. h. ein Wert, der vom Typ Schnittstellen oder Delegate zurückgegeben werden – verwendet werden und kann nicht in einer Eingabe Position verwendet werden. Ein Typ `T` gilt *gültige kovariant* wenn:

* `T` ist eine Klasse, Struktur oder enumerierten Typ an.

* `T` ist nicht generische Delegaten oder dieser Schnittstelle.

* `T` ist ein Arraytyp, dessen Elementtyp kovariant gültig ist.

* `T` ist ein Typparameter, die als nicht deklariert wurde ein `Out` Typparameter.

* `T` wird von ein konstruierter Typ von Schnittstellen oder Delegate `X(Of P1,...,Pn)` mit Typargumenten `A1,...,An` so, dass:

  * Wenn `Pi` wurde dann als ein Out-Parameter vom Typ deklariert `Ai` kovariant ist gültig.

  * Wenn `Pi` wurde dann als Typ In-Parameter deklariert `Ai` gültige kontravariant ist.

Folgendes muss in einen Typ von Schnittstellen oder Delegate kovariant gültig sein:

* Die Basisschnittstelle einer Schnittstelle.

* Der Rückgabetyp einer Funktion oder der Typ des Delegaten.

* Der Typ einer Eigenschaft liegt eine `Get` Accessor.

* Der Typ any `ByRef` Parameter.

Zum Beispiel:

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

__Beachten Sie.__ `Out` ist ein reserviertes Wort.

Ein Typparameter, der mit dem In-Modifizierer deklariert ist ist *kontravariant*. Informell ein kontravarianten Typparameter kann nur in einer Eingabe Position – d. h. ein Wert, der in der in dem Schnittstellen oder Delegate-Typ übergeben wird – verwendet werden und kann nicht in einer Ausgabeposition verwendet werden. Ein Typ `T` gilt *gültige kontravariant* wenn:

* `T` ist eine Klasse, Struktur oder enumerierten Typ an.

* `T` ist eine nicht generische Delegaten oder dieser Schnittstelle.

* `T` ist ein Arraytyp, dessen Elementtyp gültige kontravariant ist.

* `T` ist ein Typparameter, der nicht als Typ In-Parameter deklariert wurde.

* `T` wird von ein konstruierter Typ von Schnittstellen oder Delegate `X(Of P1,...,Pn)` mit Typargumenten `A1,...,An` so, dass:

  * Wenn `Pi` deklariert wurde, als ein `Out` Typparameter dann `Ai` gültige kontravariant ist.

  * Wenn `Pi` deklariert wurde, als ein `In` Typparameter dann `Ai` kovariant ist gültig.

Folgendes muss gültige kontravariant in einer Schnittstellen oder Delegate-Typ sein:

* Der Typ eines Parameters.

* Eine Einschränkung für einen auf die Typparameter einer Methode.

* Der Typ einer Eigenschaft, wenn sie verfügt über eine `Set` Accessor.

* Der Typ eines Ereignisses.

Zum Beispiel:

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

Werden Sie in dem Fall, in denen ein Typ muss gültig sein, kontravariant und kovariant (z. B. eine Eigenschaft mit einem `Get` und `Set` Accessor oder ein `ByRef` Parameter), ein Varianten Typparameter kann nicht verwendet werden.


CO - und -Kontravarianz bieten Anstieg zu einem Problem"Mehrdeutigkeit Raute". Betrachten Sie folgenden Code:

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

Die Klasse `C` konvertiert werden kann, um `IEnumerable(Of Object)` in zwei Möglichkeiten, die beide über kovariante Konvertierung von `IEnumerable(Of String)` und über kovariante Konvertierung von `IEnumerable(Of Exception)`. Die CLR gibt nicht an die der beiden Methoden aufgerufen wird, `c.GetEnumerator()`. In der Regel jedes Mal, wenn eine Klasse wird deklariert, um eine kovariante Schnittstelle mit zwei generischen Argumenten zu implementieren, die einen gemeinsamen Obertyp aufweisen (z. B. in diesem Fall `String` und `Exception` haben Sie die gemeinsamen Obertyp `Object`), oder eine Klasse deklariert ist implementieren eine kontravariante Schnittstelle mit zwei generischen Argumenten, die einen allgemeinen Untertyp aufweisen, dann ist Mehrdeutigkeiten auftreten können. Der Compiler gibt eine Warnung für solche Deklarationen.
