---
ms.openlocfilehash: 2e917b7aa27afd8a91e8c289e7454785d01bdda8
ms.sourcegitcommit: 6eca149bdc736113e0adb709212bd266c9503c33
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2019
ms.locfileid: "47426671"
---
# <a name="source-files-and-namespaces"></a>Quelldateien und Namespaces

Eine Visual Basic-Programm besteht aus einen oder mehrere Quelldateien. Wenn ein Programm kompiliert wird, werden alle Quelldateien zusammen verarbeitet; Daher können die Quelldateien von, möglicherweise in einer kreisförmigen Weise, ohne dass der Vorwärtsdeklaration abhängen. Die Reihenfolge der Deklarationen in den Programmtext im Text ist im Allgemeinen ohne Bedeutung.

Eine Quelldatei besteht aus einer optionalen Gruppe von optionsanweisungen, "Import"-Anweisungen und Attribute, die durch einen Namespacetext eingehalten werden. Die Attribute, die einzelnen entweder müssen die `Assembly` oder `Module` Modifizierer verwenden, gelten für die .NET Framework-Assembly oder das Modul, die durch die Kompilierung erzeugt. Der Text der Quelldatei fungiert als eine implizite Namespacedeklaration für den globalen Namespace, dies bedeutet, dass alle Deklarationen, die auf der obersten Ebene einer Quelldatei im globalen Namespace befinden. Zum Beispiel:

FileA.vb:

```vb
Class A
End Class
```

FileB.vb:

```vb
Class B
End Class
```

Die zwei Quelldateien auf den globalen Namespace, in diesem Fall Deklarieren von zwei Klassen mit den voll gekennzeichneten Namen tragen `A` und `B`. Da die zwei Quelldateien zum selben Deklarationsabschnitt beitragen möchten, wäre ein Fehler gewesen, wenn jeweils eine Deklaration eines Elements mit dem gleichen Namen enthalten.

__Beachten Sie.__ Die kompilierungsumgebung kann die Namespacedeklarationen überschreiben, die in denen implizit eine Quelldatei kopiert wird.

Wenn nicht anders angegeben können Anweisungen in Visual Basic-Programms von einem Zeilenabschluss oder von einem Doppelpunkt beendet werden.

```antlr
Start
    : OptionStatement* ImportsStatement* AttributesStatement* NamespaceMemberDeclaration*
    ;

StatementTerminator
    : LineTerminator
    | ':'
    ;

AttributesStatement
    : Attributes StatementTerminator
    ;
```

## <a name="program-startup-and-termination"></a>Programmstart und-Beendigung

Programmstart tritt auf, wenn die ausführungsumgebung eine angegebene Methode, ausgeführt wird, wie des Programms die bezeichnet *Einstiegspunkt*. Diese Einstiegspunktmethode, die immer genannt werden muss `Main`, freigegeben werden müssen, können nicht in einem generischen Typ enthalten sein, darf keine Async-Modifizierer aufweisen und muss einen der folgenden Signaturen aufweisen:

```vb
Sub Main()
Sub Main(args() As String)
Function Main() As Integer
Function Main(args() As String) As Integer
```

Der Zugriff auf die Einstiegspunktmethode ist irrelevant. Wenn ein Programm mehr als ein geeigneter Einstiegspunkt enthält, muss die kompilierungsumgebung einer als Einstiegspunkt festlegen. Andernfalls tritt ein Kompilierungsfehler auf. Die kompilierungsumgebung kann auch eine Einstiegspunktmethode erstellen, sofern noch nicht vorhanden.

Wenn ein Programm beginnt, wenn der Einstiegspunkt auf einen Parameter aufweist, enthält das Argument der ausführungsumgebung vom die Befehlszeilenargumente an das Programm als Zeichenfolgen dargestellt. Wenn der Einstiegspunkt auf einen Rückgabetyp hat `Integer`, und klicken Sie dann der von der Funktion zurückgegebene Wert als Ergebnis des Programms an der ausführungsumgebung zurückgegeben wird.

In jeder anderen Hinsicht Verhalten sich genauso wie andere Methoden einstiegspunktmethoden. Wenn die Ausführung auf den Aufruf der Einstiegspunktmethode, die von der ausführungsumgebung erfolgt, wird das Programm beendet.

## <a name="compilation-options"></a>Kompilierungsoptionen

Eine Quelldatei kann Kompilierungsoptionen angeben, in der Source-Code mit *option Anweisungen*.

```antlr
OptionStatement
    : OptionExplicitStatement
    | OptionStrictStatement
    | OptionCompareStatement
    | OptionInferStatement
    ;
```

Ein `Option` Aussage gilt nur für die Quelldatei, in dem er angezeigt wird, und nur eine für jede Art von `Option` Anweisung darf in einer Quelldatei verwendet werden. Zum Beispiel:

```vb
Option Strict On
Option Compare Text
Option Strict Off    ' Not allowed, Option Strict is already specified.
Option Compare Text  ' Not allowed, Option Compare is already specified.
```

Es gibt vier Kompilierungsoptionen: strikte Typsemantik, explizite Deklaration-Semantik, die Semantik für Vergleichsvorgänge und Typ der lokalen Variablen Typrückschluss-Semantik. Wenn eine Quelldatei nicht mit eine bestimmten beinhaltet `Option` -Anweisung, und klicken Sie dann auf die kompilierungsumgebung bestimmt, welche bestimmten Satz von Semantik verwendet werden. Es gibt auch eine fünfte Kompilierungsoption,, Überprüfungen auf Ganzzahlüberlauf, die über die kompilierungsumgebung kann nur angegeben werden.


### <a name="option-explicit-statement"></a>Option Explicit-Anweisung

Die `Option Explicit` -Anweisung bestimmt, ob lokale Variablen implizit deklariert werden können. Die Schlüsselwörter `On` oder `Off` kann die Anweisung folgen, wenn keine Angabe gemacht wird, wird standardmäßig `On`. Wenn keine Anweisung in einer Datei angegeben ist, bestimmt der kompilierungsumgebung an, das verwendet wird.

```antlr
OptionExplicitStatement
    : 'Option' 'Explicit' OnOff? StatementTerminator
    ;

OnOff
    : 'On' | 'Off'
    ;
```


__Beachten Sie.__ `Explicit` und `Off` sind keine reservierten Wörter.

```vb
Option Explicit Off

Module Test
    Sub Main()
        x = 5 ' Valid because Option Explicit is off.
    End Sub
End Module
```

In diesem Beispiel wird die lokale Variable `x` wird durch Zuweisung implizit deklariert. Der Typ von `x` lautet `Object`.


### <a name="option-strict-statement"></a>Option Strict Statement

Die `Option Strict` -Anweisung bestimmt, ob Konvertierungen und Vorgänge für `Object` unterliegen strikte oder die flexible Typsemantik und gibt an, ob Typen als implizit typisiert werden `Object` ohne `As` -Klausel angegeben ist. Die-Anweisung darauf folgt möglicherweise durch die Schlüsselwörter `On` oder `Off`; Wenn keine Angabe gemacht wird, der Standardwert ist `On`. Wenn keine Anweisung in einer Datei angegeben ist, bestimmt der kompilierungsumgebung an, das verwendet wird.

```antlr
OptionStrictStatement
    : 'Option' 'Strict' OnOff? StatementTerminator
    ;
```

__Beachten Sie.__ `Strict` und `Off` sind keine reservierten Wörter.

```vb
Option Strict On

Module Test
    Sub Main()
        Dim x ' Error, no type specified.
        Dim o As Object
        Dim b As Byte = o ' Error, narrowing conversion.

        o.F() ' Error, late binding disallowed.
        o = o + 1 ' Error, addition is not defined on Object.
    End Sub
End Module
```

Bei der strikten Semantik sind die folgenden nicht zulässig:

* Einschränkende Konvertierungen ohne einen expliziten Cast-Operator.

* Späte Bindung.

* Vorgänge für Typ `Object` außer `TypeOf`... `Is`, `Is`, und `IsNot`.

* Das Auslassen der `As` -Klausel in einer Deklaration, die nicht über einen abgeleiteten Typ besitzt.


### <a name="option-compare-statement"></a>Option Compare-Anweisung

Die `Option Compare` -Anweisung bestimmt die Semantik der Vergleich von Zeichenfolgen. Vergleich von Zeichenfolgen werden ausgeführt, entweder mithilfe von binäre Vergleiche (in dem der binäre Unicode-Wert, der einzelnen Zeichen ist im Vergleich) oder Textvergleichen (in der die lexikalische Bedeutung der einzelnen Zeichen ist im Vergleich mit der aktuellen Kultur). Wenn keine Anweisung in eine Datei angegeben ist, steuert die kompilierungsumgebung an, die Art des Vergleichs verwendet wird.

```antlr
OptionCompareStatement
    : 'Option' 'Compare' CompareOption StatementTerminator
    ;

CompareOption
    : 'Binary' | 'Text'
    ;
```

__Beachten Sie.__ `Compare`, `Binary`, und `Text` sind keine reservierten Wörter.

```vb
Option Compare Text

Module Test
    Sub Main()
        Console.WriteLine("a" = "A")    ' Prints True.
    End Sub
End Module
```

In diesem Fall erfolgt der Zeichenfolgenvergleich mithilfe eines Textvergleichs, die Groß-/Kleinschreibung nicht berücksichtigt. Wenn `Option Compare Binary` angegeben worden wäre, und klicken Sie dann diese gedruckt haben, würden `False`.


### <a name="integer-overflow-checks"></a>Überprüfungen auf Ganzzahlüberlauf

Operationen mit ganzen Zahlen können aktiviert oder nicht zur Laufzeit für überlaufbedingungen überprüft werden. Wenn überlaufbedingungen überprüft werden und eine ganzzahlige Operation überläuft, eine `System.OverflowException` Ausnahme ausgelöst. Wenn Überlaufbedingungen nicht aktiviert werden, lösen Überprüfungen auf Ganzzahlüberlauf keine Ausnahme aus. Die kompilierungsumgebung bestimmt, ob diese Option aktiviert oder deaktiviert ist.

### <a name="option-infer-statement"></a>Option Infer-Anweisung

Die `Option Infer` -Anweisung bestimmt, ob der lokale Variablendeklarationen, die keine `As` -Klausel aufweisen, einen abgeleiteten Typ oder einen verwenden `Object`. Die-Anweisung darauf folgt möglicherweise durch die Schlüsselwörter `On` oder `Off`; Wenn keine Angabe gemacht wird, der Standardwert ist `On`. Wenn keine Anweisung in einer Datei angegeben ist, bestimmt der kompilierungsumgebung an, das verwendet wird.

```antlr
OptionInferStatement
    : 'Option' 'Infer' OnOff? StatementTerminator
    ;
```

__Beachten Sie.__ `Infer` und `Off` sind keine reservierten Wörter.

```vb
Option Infer On

Module Test
    Sub Main()
        ' The type of x is Integer
        Dim x = 10

        ' The type of y is String
        Dim y = "abc"
    End Sub
End Module
```


## <a name="imports-statement"></a>Imports-Anweisung

`Imports` Anweisungen importieren die Namen von Entitäten in einer Quelldatei, können die Namen ohne Qualifikation referenziert werden, oder Importieren eines Namespace für die Verwendung in XML-Ausdrücken.

```antlr
ImportsStatement
    : 'Imports' ImportsClauses StatementTerminator
    ;

ImportsClauses
    : ImportsClause ( Comma ImportsClause )*
    ;

ImportsClause
    : AliasImportsClause
    | MembersImportsClause
    | XMLNamespaceImportsClause
    ;
```

In den Memberdeklarationen in einer Quelldatei, die enthält ein `Imports` -Anweisung, die im angegebenen Namespace enthaltenen Typen kann direkt verwiesen werden, wie im folgenden Beispiel gezeigt:

```vb
Imports N1.N2

Namespace N1.N2
    Class A
    End Class
End Namespace 

Namespace N3
    Class B
        Inherits A
    End Class
End Namespace
```

Hier in der Quelldatei, die Typmember Namespace `N1.N2` direkt verfügbar sind, und daher Klasse `N3.B` leitet sich von der Klasse `N1.N2.A`.

`Imports` -Anweisungen müssen nach jedem angezeigt werden `Option` Anweisungen aber vor allen Deklarationen geben. Die kompilierungsumgebung kann auch implizite definieren `Imports` Anweisungen.

`Imports` Anweisungen Namen in einer Quelldatei zur Verfügung stellen, aber nicht alle Elemente in den globalen Namespace Deklarationsabschnitt deklarieren. Der Bereich der Namen von importiert eine `Imports` Anweisung erstreckt sich über die Member-Namespacedeklarationen in der Quelldatei enthalten sind. Im Rahmen einer `Imports` -Anweisung speziell keinen anderen `Imports` -Anweisungen, noch anderen Quelldateien. `Imports` Anweisungen können nicht miteinander verwenden.

In diesem Beispiel ist die letzte `Imports` Anweisung befindet sich in der Fehler, da er durch den ersten Importalias nicht betroffen ist.

```vb
Imports R1 = N1 ' OK.
Imports R2 = N1.N2 ' OK.
Imports R3 = R1.N2 ' Error: Can't refer to R1.

Namespace N1.N2
End Namespace
```

__Beachten Sie.__ Der Namespace oder Typ Namen in `Imports` Anweisungen immer behandelt, als ob sie den vollqualifizierten sind. Das heißt, der am weitesten links stehende Bezeichner in einem Namespace oder Typ Namen löst immer im globalen Namespace und der Rest der Lösung, die gemäß den normalen Regeln zur namensauflösung fortgesetzt. Dies ist der einzige Ort, in der Sprache, die eine solche Regel gilt. die Regel stellt sicher, dass ein Name nicht vollständig von Qualifikation ausgeblendet werden kann. Ohne die Regel ein Wenn Sie ein Namen im globalen Namespace in einer bestimmten Quelle-Datei ausgeblendet wurden wäre nicht möglich, geben Sie keine Namen aus diesem Namespace qualifizierten so es.

In diesem Beispiel die `Imports` immer die Anweisung verweist auf die globale `System` -Namespace und nicht auf die Klasse in der Quelldatei.

```vb
Imports System   ' Imports the namespace, not the class.

Class System
End Class
```


### <a name="import-aliases"></a>Importaliase

Ein *Importalias* definiert einen Alias für einen Namespace oder Typ.

```antlr
AliasImportsClause
    : Identifier Equals TypeName
    ;
```

```vb
Imports A = N1.N2.A

Namespace N1.N2
    Class A
    End Class
End Namespace

Namespace N3
    Class B
        Inherits A
    End Class
End Namespace
```

Hier in der Quelldatei `A` ist ein Alias für `N1.N2.A`, und daher `N3.B` leitet sich von der Klasse `N1.N2.A`. Die gleiche Wirkung erhalten Sie durch Erstellen eines Alias `R` für `N1.N2` verweisen Sie einfach auf `R.A`:

```vb
Imports R = N1.N2

Namespace N3
    Class B
        Inherits R.A
    End Class
End Namespace
```

Der Bezeichner eines Import-Alias muss innerhalb des Deklarationsabschnitts des globalen Namespaces (nicht nur den globalen Namespace-Deklarationen in der Quelldatei, in dem der Importalias definiert wird) eindeutig sein, obwohl er keinen Namen im globalen Namespaces deklariert ist Deklarationsabschnitt.

__Beachten Sie.__ Deklarationen in einem Modul führen Namen in der enthaltenden Deklarationsabschnitt. Daher ist es für eine Deklaration in einem Modul, haben den gleichen Namen wie ein Importalias gültig, obwohl die Deklaration des Namens im Deklarationsbereich mit der zugegriffen werden kann.

```vb
' Error: Alias A conflicts with typename A
Imports A = N3.A

Class A
End Class

Namespace N3
    Class A
    End Class
End Namespace
```

Hier ist der globale Namespace bereits ein Element enthält `A`, daher wird ein Fehler für ein Importalias für diesen Bezeichner verwenden. Es ist ebenso einen Fehler für zwei oder mehr Importaliase in der gleichen Quelldatei Aliase mit demselben Namen zu deklarieren.

Ein Importalias kann einen Alias für jeden Namespace oder Typ erstellen. Zugreifen auf einen Namespace oder Typ über einen Alias führt genau dieselbe Ergebnisse wie beim Zugriff auf den Namespace oder Typ durch den deklarierten Namen.

```vb
Imports R1 = N1
Imports R2 = N1.N2

Namespace N1.N2
    Class A
    End Class
End Namespace

Namespace N3
    Class B
        Private a As N1.N2.A
        Private b As R1.N2.A
        Private c As R2.A
    End Class
End Namespace
```

Hier wird die Namen `N1.N2.A`, `R1.N2.A`, und `R2.A` sind äquivalent, und alle auf die Klasse verweisen, dessen vollqualifizierter Name ist `N1.N2.A`.

Der Import gibt an, den genauen Namen der der Namespace oder Typ, der er einen Alias erstellt, werden. Dies muss den genauen den vollqualifizierten Namen, Namespaces oder Typs sein: die normalen Regeln für die Auflösung von qualifizierten Namen (sodass z. B. Zugriff auf die Member einer Basisklasse durch eine abgeleitete Klasse) wird nicht verwendet.

Wenn ein Import-Alias verweist auf einen Typ oder Namespace, der von diesen Regeln nicht aufgelöst werden kann, klicken Sie dann die Import-Anweisung wird ignoriert (und der Compiler gibt eine Warnung).

Darüber hinaus darf nicht der Verweis auf ein offener generischer Typ sein – alle generische Typen müssen gültige Typargumente angegeben haben, und alle Typargumente müssen von der oben genannten Regeln aufgelöst werden können. Eine falsche Bindung eines generischen Typs handelt es sich um einen Fehler.

```vb
Imports A = G              ' error: since G is an open generic type
Imports B = G(Of Integer)  ' okay
Imports C = Derived.Nested ' warning: Derived.Nested isn't itself a type
Imports D = G(Of Derived.Nested) ' error: Derived.Nested isn't found

Class G(Of T) : End Class

Class Base
    Class Nested : End Class
End Class

Class Derived : Inherits Base
End Class

Module Module1
    Sub Main()
        Dim x As C               ' error: "C" wasn't succesfully defined
        Dim y As Derived.Nested  ' okay
    End Sub
End Module
```

Deklarationen in der Quelldatei können den Namen des Importalias überschatten.

```vb
Imports R = N1.N2

Namespace N1.N2
    Class A
    End Class
End Namespace

Namespace N3
    Class R
    End Class

    Class B
        Inherits R.A  ' Error, R has no member A
    End Class
End Namespace
```

Im vorherigen Beispiel den Verweis auf `R.A` in der Deklaration der `B` verursacht einen Fehler, da `R` bezieht sich auf `N3.R`, nicht `N1.N2`.

Ein Importalias stellt innerhalb einer bestimmten Quelldatei ein alias zur Verfügung, aber es trägt nicht die neuen Member zu den zugrunde liegenden Deklarationsabschnitt. Das heißt, ein Importalias ist nicht transitiv, aber stattdessen wirkt sich nur auf die Quelldatei, in der er auftritt.

File1.vb:

```vb
Imports R = N1.N2

Namespace N1.N2
    Class A
    End Class
End Namespace
```

File2.vb:

```vb
Class B
    Inherits R.A ' Error, R unknown.
End Class
```

Im obigen Beispiel da im Rahmen der Importalias wird `R` erstreckt sich auch nur auf Deklarationen in der Quelldatei, in dem es enthalten ist, `R` ist in der zweiten Quelldatei unbekannt.


### <a name="namespace-imports"></a>Namespaceimporte

Ein *Namespaceimport* importiert alle Elemente eines Namespace oder Typ, den Bezeichner für jedes Element der Namespace oder Typ ohne Qualifikation verwendet werden können. Bei Typen ermöglicht ein Namespaceimport nur Zugriff auf freigegebenen Member des Typs ohne den Klassennamen zu qualifizieren. Insbesondere können die Mitglieder von enumerierten Typen ohne Qualifikation verwendet werden.

```antlr
MembersImportsClause
    : TypeName
    ;
```

Zum Beispiel:

```vb
Imports Colors

Enum Colors
    Red
    Green
    Blue
End Enum

Module M1
    Sub Main()
        Dim c As Colors = Red
    End Sub
End Module
```

Im Gegensatz zu einem Importalias weist einen Namespace importiert keine Einschränkungen auf den Namen, die importiert und können importieren, Namespaces und Typen, deren Bezeichner bereits im globalen Namespace deklariert werden. Die Namen durch einen regulären Import importiert werden durch Importaliase und Deklarationen in der Quelldatei schattiert.

Im folgenden Beispiel `A` bezieht sich auf `N3.A` statt `N1.N2.A` in Memberdeklarationen in der `N3` Namespace.

```vb
Imports N1.N2

Namespace N1.N2
    Class A
    End Class
End Namespace

Namespace N3
    Class A
    End Class

    Class B
        Inherits A
    End Class
End Namespace
```

Wenn mehr als einem importierter Namespace enthält Member mit demselben Namen (dieser Name ist kein andernfalls durch einen Importalias oder eine Deklaration schattiert), wird ein Verweis auf diesen Namen ist mehrdeutig und verursacht einen Fehler während der Kompilierung.

```vb
Imports N1
Imports N2

Namespace N1
    Class A
    End Class
End Namespace 

Namespace N2
    Class A
    End Class
End Namespace 

Namespace N3
    Class B
        Inherits A ' Error, A is ambiguous.
    End Class
End Namespace
```

Im obigen Beispiel beide `N1` und `N2` -Member enthalten `A`. Da `N3` importiert, verweisen auf `A` in `N3` verursacht einen Fehler während der Kompilierung. In diesem Fall der Konflikt kann aufgelöst werden, entweder über die Qualifikation von Verweisen auf `A`, oder durch die Einführung eines Importalias, der einen bestimmten wählt `A`, wie im folgenden Beispiel:

```vb
Imports N1
Imports N2
Imports A = N1.A

Namespace N3
    Class B
        Inherits A ' A means N1.A.
    End Class
End Namespace
```

Nur Namespaces, Klassen, Strukturen, Enumerationstypen und standard-Module importiert werden können.


### <a name="xml-namespace-imports"></a>XML-Namespace-Importe

Ein *XML-Namespaceimport* definiert einen Namespace oder den Standardnamespace für den nicht qualifizierten XML-Ausdrücke, die innerhalb der Kompilierungseinheit.

```antlr
XMLNamespaceImportsClause
    : '<' XMLNamespaceAttributeName XMLWhitespace? Equals XMLWhitespace?
      XMLNamespaceValue '>'
    ;

XMLNamespaceValue
    : DoubleQuoteCharacter XMLAttributeDoubleQuoteValueCharacter* DoubleQuoteCharacter
    | SingleQuoteCharacter XMLAttributeSingleQuoteValueCharacter* SingleQuoteCharacter
    ;
```

Zum Beispiel:

```vb
Imports <xmlns:db="http://example.org/database">

Module Test
    Sub Main()
        ' db namespace is "http://example.org/database"
        Dim x = <db:customer><db:Name>Bob</></>

        Console.WriteLine(x.<db:Name>)
    End Sub
End Module
```

Ein XML-Namespace den Standardnamespace, einschließlich kann nur einmal für einen bestimmten Satz von Importen definiert werden. Zum Beispiel:

```vb
Imports <xmlns:db="http://example.org/database-one">
' Error: namespace db is already defined
Imports <xmlns:db="http://example.org/database-two">
```


## <a name="namespaces"></a>Namespaces

Visual Basic-Programme werden mithilfe von Namespaces organisiert. Namespaces sowohl intern ein Programm zu organisieren als auch die Möglichkeit, die Programmelemente verfügbar, um andere Programme gemacht werden zu organisieren.

Im Gegensatz zu anderen Entitäten-Namespaces sind offen und kann deklariert werden mehrmals innerhalb desselben Programms und für viele Programme, mit jeder Deklaration Elemente denselben Namespace beiträgt. Im folgenden Beispiel die beiden Namespacedeklarationen, die zum selben Deklarationsabschnitt, deklarieren zwei Klassen mit den voll gekennzeichneten Namen beitragen `N1.N2.A` und `N1.N2.B`.

```vb
Namespace N1.N2
    Class A
    End Class
End Namespace

Namespace N1.N2
    Class B
    End Class
End Namespace
```

Da die beiden Deklarationen zum selben Deklarationsabschnitt beitragen möchten, wäre es, dass ein Fehler, wenn jeder eine Deklaration eines Elements mit dem gleichen Namen enthalten.

Es ist ein globaler Namespace, die keinen Namen und, dessen geschachtelte Namespaces und Typen ohne Qualifikation immer zugegriffen werden können. Der Gültigkeitsbereich eines Namespace im globalen Namespace deklariert ist, das gesamte Programmtext. Andernfalls der Rahmen eines Typs oder Namespaces deklariert, in einem Namespace, dessen vollqualifizierter Name ist `N` ist der Programmtext von jeder Namespace, der den vollqualifizierten Namen des Namespaces, deren entsprechenden beginnt mit `N` oder `N` selbst. (Beachten Sie, dass ein Compiler Deklarationen in einem bestimmten Namespace standardmäßig platzieren kann. Dies wird nicht die Tatsache, dass immer noch ein globaler, unbenannter Namespace vorhanden ist geändert.)

In diesem Beispiel die Klasse `B` sehen, dass die Klasse `A` da `B`des Namespace `N1.N2.N3` ist im Prinzip innerhalb des Namespaces geschachtelt `N1.N2`.

```vb
Namespace N1.N2
    Class A
    End Class
End Namespace

Namespace N1.N2.N3
    Class B
        Inherits A
    End Class
End Namespace
```

### <a name="namespace-declarations"></a>Namespacedeklarationen

Es gibt drei Arten des Namespace-Deklaration.

```antlr
NamespaceDeclaration
    : 'Namespace' NamespaceName StatementTerminator
      NamespaceMemberDeclaration*
      'End' 'Namespace' StatementTerminator
    ;

NamespaceName
    : RelativeNamespaceName
    | 'Global'
    | 'Global' '.' RelativeNamespaceName
    ;

RelativeNamespaceName
    : Identifier ( Period IdentifierOrKeyword )*
    ;
```

Die erste Form beginnt mit dem Schlüsselwort `Namespace` gefolgt von der ein relativer Namespacename. Wenn der relativer Namespacename qualifiziert ist, wird die Namespacedeklaration behandelt, als ob sie in den Namespacedeklarationen, die für jeden Namen in den qualifizierten Namen lexikalisch geschachtelt ist. Beispielsweise sind die folgenden beiden Namespaces semantisch gleichwertig:

```vb
Namespace N1.N2
    Class A
    End Class

    Class B
    End Class
End Namespace 

Namespace N1
    Namespace N2
        Class A
        End Class

        Class B
        End Class
    End Namespace
End Namespace
```

Die zweite Form beginnt mit den Schlüsselwörtern `Namespace Global`. Es wird behandelt, als ob alle seine Memberdeklarationen im globalen unbenannte Namespace – unabhängig von der alle Standardwerte, die von der kompilierungsumgebung bereitgestellten lexikalisch platziert wurden. Diese Form der Namespace-Deklaration kann nicht in jeder anderen Namespacedeklaration lexikalisch geschachtelt werden.

Die dritte Form beginnt mit den Schlüsselwörtern `Namespace Global` gefolgt von einem qualifizierten Bezeichner `N`. Es wird behandelt, als handele es sich um eine Namespace-Deklaration, der das erste Formular "`Namespace N`", die im globalen unbenannte Namespace – unabhängig von der alle Standardwerte, die von der kompilierungsumgebung bereitgestellten lexikalisch platziert wurde. Diese Form der Namespace-Deklaration kann nicht in jeder anderen Namespacedeklaration lexikalisch geschachtelt werden.

```vb
Namespace Global       ' Puts N1.A in the global namespace
    Namespace N1
        Class A
        End Class
    End Namespace
End Namespace

Namespace Global.N1    ' Equivalent to the above
    Class A
    End Class
End Namespace

Namespace N1           ' May or may not be equivalent to the above,
    Class A            ' depending on defaults provided by the
    End Class          ' compilation environment
End Namespace
```

Beim Umgang mit den Elementen eines Namespace ist es nicht wichtig, in ein bestimmter Member deklariert wird. Wenn zwei Programme eine Entität mit dem gleichen Namen im selben Namespace definieren, tritt beim Versuch, den Namen im Namespace auflösen, einem Mehrdeutigkeitsfehler.

Namespaces sind per Definition `Public`, sodass eine Namespace-Deklaration keine Zugriffsmodifizierer enthalten kann.


### <a name="namespace-members"></a>Namespace-Elemente

Namespace-Mitglieder können nur Namespacedeklarationen werden und Typdeklarationen dar. Deklarationen von Typen möglicherweise `Public` oder `Friend` Zugriff. Der Standardzugriff für Typen ist `Friend` Zugriff.

```antlr
NamespaceMemberDeclaration
    : NamespaceDeclaration
    | TypeDeclaration
    ;

TypeDeclaration
    : ModuleDeclaration
    | NonModuleDeclaration
    ;

NonModuleDeclaration
    : EnumDeclaration
    | StructureDeclaration
    | InterfaceDeclaration
    | ClassDeclaration
    | DelegateDeclaration
    ;
```
