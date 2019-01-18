---
ms.openlocfilehash: 7ad305f4b85bce12f174511af5e5f0aa62a7cc0e
ms.sourcegitcommit: 6eca149bdc736113e0adb709212bd266c9503c33
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2019
ms.locfileid: "47426667"
---
# <a name="preprocessing-directives"></a>Präprozessoranweisungen

Nach eine Datei lexikalisch analysiert wurde, treten auf verschiedene Arten von Quelle vorverarbeitung. Die wichtigste, bedingte Kompilierung bestimmt die Quelle von der syntaktischen Grammatik verarbeitet wird; zwei andere Arten von Anweisungen, die – externe quelldirektiven und Region-Direktiven--Meta-Informationen zur Quelle bereitzustellen, aber haben keine Auswirkungen auf die Kompilierung.

## <a name="conditional-compilation"></a>Bedingte Kompilierung

Für die bedingte Kompilierung steuert, ob es sich bei Sequenzen logischer Zeilen in der tatsächlichen Code übersetzt werden. Am Anfang des für die bedingte Kompilierung, sind alle logische Zeilen aktiviert. Einschließen von Zeilen in bedingten kompilierungsanweisungen kann jedoch selektiv die Zeilen in der Datei, die veranlassen, die im weiteren Verlauf des Kompilierungsprozesses ignoriert werden deaktivieren.

```antlr
CCStart
    : CCStatement*
    ;

CCStatement
    : CCConstantDeclaration
    | CCIfGroup
    | LogicalLine
    ;

CCExpression
    : LiteralExpression
    | CCParenthesizedExpression
    | CCSimpleNameExpression
    | CCCastExpression
    | CCOperatorExpression
    | CCConditionalExpression
    ;

CCParenthesizedExpression
    : '(' CCExpression ')'
    ;

CCSimpleNameExpression
    : Identifier
    ;

CCCastExpression
    : 'DirectCast' '(' CCExpression ',' TypeName ')'
    | 'TryCast' '(' CCExpression ',' TypeName ')'
    | 'CType' '(' CCExpression ',' TypeName ')'
    | CastTarget '(' CCExpression ')'
    ;

CCOperatorExpression
    : CCUnaryOperator CCExpression
    | CCExpression CCBinaryOperator CCExpression
    ;

CCUnaryOperator
    : '+' | '-' | 'Not'
    ;

CCBinaryOperator
    : '+'     | '-'     | '*'   | '/'       | '\\'     | 'Mod' | '^' | '='
    | '<' '>' | '<'     | '>'   | '<' '='   | '>' '='  | '&'
    | 'And'   | 'Or'    | 'Xor' | 'AndAlso' | 'OrElse'
    | '<' '<' | '>' '>'
    ;

CCConditionalExpression
    : 'If' '(' CCExpression ',' CCExpression ',' CCExpression ')'
    | 'If' '(' CCExpression ',' CCExpression ')'
    ;
```


Beispielsweise wird das Programm

```vb
#Const A = True
#Const B = False

Class C

#If A Then
    Sub F()
    End Sub
#Else
    Sub G()
    End Sub
#End If

#If B Then
    Sub H()
    End Sub
#Else
    Sub I()
    End Sub
#End If

End Class
```

erzeugt genaue dieselbe Sequenz von Token wie das Programm

```vb
Class C
    Sub F()
    End Sub

    Sub I()
    End Sub
End Class
```

Die Konstante Ausdrücke, die in Anweisungen für bedingte Kompilierung zulässig sind eine Teilmenge der allgemeinen Konstante Ausdrücke.

Der Präprozessor kann Leerzeichen und explizite zeilenfortsetzungen vor und nach jedem Token.


### <a name="conditional-constant-directives"></a>Bedingte Konstanten-Direktiven

Bedingungsanweisungen Konstante definieren Sie Konstanten, die in einem separaten für die bedingte Kompilierung Deklarationsbereich beschränkt werden, um die Quelldatei vorhanden sind.

```antlr
CCConstantDeclaration
    : '#' 'Const' Identifier '=' CCExpression LineTerminator
    ;
```

Die Deklarationsabschnitt ist etwas Besonderes, da keine explizite Deklaration von Konstanten für bedingte Kompilierung ist erforderlich – Konstanten für bedingte können implizit in eine Direktive für bedingte Kompilierung definiert werden.

Vor dem wird ein Wert zugewiesen, eine Konstante für bedingte Kompilierung hat den Wert `Nothing`. Wenn eine Konstante für bedingte Kompilierung ein Wert zugewiesen, der ein konstanter Ausdruck sein muss ist, wird der Typ der Konstante der Typ des Werts zugewiesen werden. Eine Konstante für bedingte Kompilierung möglicherweise mehrere Male in einer Quelldatei neu definiert werden.

Der folgende Code gibt beispielsweise nur die Zeichenfolge `about to print value` und den Wert der `Test`.

```vb
Module M1
    Sub PrintValue(Test As Integer)

#Const DebugCode = True

#If DebugCode Then
        Console.WriteLine("about to print value")
#End If

#Const DebugCode = False

        Console.WriteLine(Test)

#If DebugCode Then
        Console.WriteLine("printed value")
#End If

    End Sub
End Module
```

Die kompilierungsumgebung kann auch Konstanten für bedingte in einem Deklarationsabschnitt für die bedingte Kompilierung definieren.


### <a name="conditional-compilation-directives"></a>Anweisungen für die bedingte Kompilierung

Anweisungen für bedingte Kompilierung steuern, für die bedingte Kompilierung.

```antlr
CCIfGroup
    : '#' 'If' CCExpression 'Then'? LineTerminator CCStatement*
    CCElseIfGroup* CCElseGroup? '#' 'End' 'If' LineTerminator
    ;

CCElseIfGroup
    : '#' ElseIf CCExpression 'Then'? LineTerminator CCStatement*
    ;

CCElseGroup
    : '#' 'Else' LineTerminator CCStatement*
    ;
```

Konstanten für bedingte können nur Konstante Ausdrücke und bedingte Kompilierungskonstanten verweisen. Jede der Konstante Ausdrücke in einer einzelnen für die bedingte Kompilierung Gruppe ausgewertet und konvertiert die `Boolean` Typ in der Reihenfolge vom ersten bis zum letzten, bis eine des bedingten Ausdrücke im Text ergibt `True`. Wenn ein Ausdruck nicht konvertierbar ist `Boolean`, einem Fehler während der Kompilierung führt. Semantik und binäre Zeichenfolgenvergleiche werden immer verwendet, bei der Auswertung des konstanten Ausdrücken für bedingte Kompilierung, unabhängig davon, ob `Option` Direktiven oder Einstellungen für die Kompilierung.

Alle Zeilen in die Gruppe, einschließlich geschachtelter Bedingte Kompilierungsdirektiven eingeschlossen sind deaktiviert, mit Ausnahme der Zeilen zwischen die Anweisung mit der `True` Ausdruck und der nächsten bedingten Anweisung der Gruppe oder Linien zwischen den `Else`Anweisung und die `End If` Anweisung Wenn ein `Else` wird in der Gruppe und alle Ausdrücke ausgewertet `False`.

In diesem Beispiel ist der Aufruf von `WriteToLog` in die `Trace` Direktive für bedingte Kompilierung wird nicht verarbeitet werden, da die umgebenden `Debug` bedingte Kompilierungsdirektive ergibt `False`.

```vb
#Const Debug = False   ' Debugging off
#Const Trace = True    ' Tracing on

Class PurchaseTransaction
    Sub Commit()

#If Debug Then
        CheckConsistency()
#If Trace Then
        WriteToLog(Me.ToString())
#End If
#End If
        ...
    End Sub
End Class
```


## <a name="external-source-directives"></a>Externe Quelldirektiven

Eine Quelldatei kann es sich um externe quelldirektiven enthalten, die eine Zuordnung zwischen Quellzeilen und Text für die Quelle extern angeben.

```antlr
ESDStart
    : ExternalSourceStatement*
    ;

ExternalSourceStatement
    : ExternalSourceGroup
    | LogicalLine
    ;

ExternalSourceGroup
    : '#' 'ExternalSource' '(' StringLiteral ',' IntLiteral ')' LineTerminator
      LogicalLine* '#' 'End' 'ExternalSource' LineTerminator
    ;
```

Externe quelldirektiven haben keine Auswirkungen auf die Kompilierung und können nicht geschachtelt werden. Zum Beispiel:

```vb
Module Test
    Sub Main()

#ExternalSource("c:\wwwroot\inetpub\test.aspx", 30)
        Console.WriteLine("In test.aspx")
#End ExternalSource

    End Sub
End Module
```


## <a name="region-directives"></a>Region-Direktiven

Bereichsdirektiven Gruppe von Zeilen Quellcode, haben keine anderen Auswirkungen auf die Kompilierung. Die gesamte Gruppe kann, in der integrierten Entwicklungsumgebung (IDE) reduziert und ausgeblendet oder erweitert und angezeigt werden.

```antlr
RegionStart
    : RegionStatement*
    ;

RegionStatement
    : RegionGroup
    | LogicalLine
    ;

RegionGroup
    : '#' 'Region' StringLiteral LineTerminator
      RegionStatement*
      '#' 'End' 'Region' LineTerminator
    ;
```

Regionen können geschachtelt werden. Region-Direktiven sind speziell, können sie weder gestartet noch innerhalb eines Methodentexts beenden und sie müssen die Blockstruktur des Programms berücksichtigen. Zum Beispiel:

```vb
Module Test
#Region "Startup code - do not edit"
    Sub Main()
    End Sub
#End Region

End Module


' Error due to Region directives breaking the block structure
Class C
#Region "Fred"
End Class
#End Region
```


## <a name="external-checksum-directives"></a>External Checksum-Direktiven

Eine Quelldatei kann eine externe Checksum-Direktive einschließen, die angibt, welche Prüfsumme für eine Datei, die in einer externen Quelle-Direktive verwiesen ausgegeben werden soll. In jeder anderen Hinsicht haben externe quelldirektiven keine Auswirkungen auf die Kompilierung.

```antlr
ExternalChecksumStart
    : ExternalChecksumStatement*
    ;

ExternalChecksumStatement
    : '#' 'ExternalChecksum' '('
      StringLiteral ',' StringLiteral ',' StringLiteral
      ')' LineTerminator
    ;
```

Eine externe Checksum-Direktive enthält, den Dateinamen der externen Datei, eine globally unique Identifier (GUID), die die Datei und die Prüfsumme für die Datei zugeordnet wird. Die GUID ist als eine Zeichenfolgenkonstante im Format "{Xxxxxxxx-Xxxx-Xxxx-Xxxx-Xxxxxxxxxxxx}", angegeben, wobei x eine hexadezimale Ziffer ist. Die Prüfsumme wird als eine Zeichenfolgenkonstante im Format "Xxxx...", angegeben, wobei x eine hexadezimale Ziffer ist. Die Anzahl der Ziffern in einer Prüfsumme muss eine gerade Zahl sein.

Eine externe Datei möglicherweise mehrere externe Checksum-Anweisungen, die mit ihm verknüpft ist, vorausgesetzt, dass alle Werte GUID und der Prüfsumme genau übereinstimmen. Wenn der Name der externen Datei den Namen einer Datei, die kompiliert wird übereinstimmt, wird die Prüfsumme zugunsten des Compilers prüfsummenberechnung ignoriert.

Zum Beispiel:

```vb
#ExternalChecksum("c:\wwwroot\inetpub\test.aspx", _
    "{12345678-1234-1234-1234-123456789abc}", _
    "1a2b3c4e5f617239a49b9a9c0391849d34950f923fab9484")

Module Test
    Sub Main()

#ExternalSource("c:\wwwroot\inetpub\test.aspx", 30)
        Console.WriteLine("In test.aspx")
#End ExternalSource

    End Sub
End Module
```

