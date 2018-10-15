# <a name="introduction"></a>Einführung

Microsoft&reg; Visual Basic&reg; Programmiersprache ist eine allgemeine Programmiersprache für Microsoft .NET Framework. Obwohl es eine intuitiv und einfach zu erlernende Sprache entworfen wurde, ist es auch leistungsstark genug, um die Anforderungen von erfahrenen Programmierern zu erfüllen. Die Programmiersprache Visual Basic verfügt über eine Syntax, die auf Englisch, ähnelt die fördert die Übersichtlichkeit und Lesbarkeit von Visual Basic-Code. Wo immer dies möglich ist, werden sinnvolle Wörter oder Ausdrücke anstelle von Abkürzungen, Akronyme oder Sonderzeichen verwendet. Externe oder nicht benötigte Syntax wird im Allgemeinen zulässig, jedoch nicht erforderlich.

Die Programmiersprache Visual Basic kann es sich um ein stark typisiertes oder eine schwach typisierte Sprache sein. Lose typbindung verzögert Großteil der Last des Typs, die prüft, ob bereits ein Programm ausgeführt wird. Dies schließt nicht nur die typüberprüfung von Konvertierungen, sondern auch von Methodenaufrufen, was bedeutet, dass die Bindung eines Methodenaufrufs bis zur Laufzeit verzögert werden kann. Dies ist nützlich, beim Erstellen von Prototypen oder andere Programme, die in denen entwicklungsgeschwindigkeit wichtiger als die Geschwindigkeit der abfrageausführung ist. Die Visual Basic, die Programmiersprache stark auch bietet typisierte-Semantik, die alle für die typüberprüfung zur Kompilierzeit führt und lässt keine Laufzeit-Bindung von Methodenaufrufen. Dies garantiert die optimale Leistung und trägt dazu bei, dass typkonvertierungen richtig sind. Dies ist nützlich, nach der Erstellung von Produktionsanwendungen in der, die Geschwindigkeit der Ausführung und Ausführung Korrektheit ankommt.

Dieses Dokument beschreibt die Visual Basic-Sprache. Es soll eine vollständige Beschreibung der Sprache statt einer sprachtutorial oder eines Benutzers-Referenzhandbuch sein.

## <a name="grammar-notation"></a>Grammar Notation

Diese Spezifikation beschreibt zwei Grammatiken: eine lexikalische Grammatik und syntaktischen Grammatik. Lexikalische Grammatik definiert, wie Zeichen Formular Token kombiniert werden können. Die syntaktische Grammatik definiert, wie die Token zum Formular Visual Basic-Programme kombiniert werden können. Es gibt auch mehrere sekundäre Grammatiken Vorgänge wie für die bedingte Kompilierung an die vorverarbeitung verwendet.

Die Grammatiken in dieser Spezifikation werden im ANTLR-Format geschrieben: finden Sie unter http://www.antlr.org/.

In Visual Basic-Programmen nicht von Bedeutung ist. Aus Gründen der Einfachheit alle Terminalen in standard Groß-/Kleinschreibung, aber Schreibweise entspricht. Terminals, die druckbaren ASCII-Zeichensatz-Elemente werden durch den entsprechenden ASCII-Zeichen dargestellt. Visual Basic ist ebenfalls mit Breite, Akzent beim Zuordnen von Terminals dem Unicode-Zeichen voller Breite auf ihre halbe Breite Unicode-Äquivalente, aber nur auf Basis ganze-Token übereinstimmen. Ein Token stimmen nicht überein, wenn sie gemischte halber Breite und normale Breite Zeichen enthält.

Zeilenumbrüche und Einzug können zur besseren Lesbarkeit hinzugefügt werden und sind nicht Teil der Produktion.

## <a name="compatibility"></a>Kompatibilität

Ein wichtiges Feature von Programmiersprachen ist die Kompatibilität zwischen verschiedenen Versionen der Sprache. Wenn eine neuere Version einer Sprache nicht den gleichen Code wie eine frühere Version der Sprache akzeptiert oder es anders als die vorherige Version interpretiert, kann dann eine Belastung für einen Programmierer platziert werden bei der Aktualisierung von seinen Codes von einer Version der Sprache in eine andere. Kompatibilität zwischen Versionen muss daher mit Ausnahme von beibehalten werden, wenn der Vorteil für Kunden der Sprache eine Art löschen und zu unübersichtlich wird.

Die folgende Richtlinie gesteuert, wie Änderungen an der Programmiersprache Visual Basic zwischen Versionen. Die Begriff-Sprache, bei der Verwendung in diesem Kontext bezieht sich nur auf die syntaktische und semantische Aspekte der Visual Basic-Sprache selbst und umfasst keine .NET Framework-Klassen enthalten, die als Teil der `Microsoft.VisualBasic` Namespace (und die untergeordneten Namespaces). Alle Klassen in .NET Framework werden durch eine separate Versionen und zur Kompatibilität der Richtlinie würde den Rahmen dieses Dokuments behandelt.

### <a name="kinds-of-compatibility-breaks"></a>Arten von Unterbrechung der Kompatibilität der

In einer idealen Welt wäre Kompatibilitätsgrad 100 % zwischen der vorhandenen Version von Visual Basic und allen zukünftigen Versionen von Visual Basic. Aber es gibt möglicherweise Situationen, in denen möglicherweise die Notwendigkeit, eine Unterbrechung der Kompatibilität die Kosten wiegen, kann es für Programmierer verursachen. Solche Fälle sind:

* *Neue Warnungen.* Einführung in die neue Warnung ist nicht, eine Unterbrechung der Kompatibilität. Aber da viele Entwickler mit "Warnungen als Fehler behandeln" aktiviert kompilieren, zusätzliche Sorgfalt bei der Einführung von Warnungen.

* *Neue Schlüsselwörter.* Einführung in die neuen Schlüsselwörter kann erforderlich sein, wenn neue Sprachfunktionen eingeführt. Angemessene anstrengungen und Schritte erfolgen Schlüsselwörter aus, die die Wahrscheinlichkeit eines Konflikts mit Benutzer IDs zu minimieren und um die vorhandenen Schlüsselwörter verwenden, ist es sinnvoll. Hilfe wird zum upgrade von Projekten von früheren Versionen und keine neue Schlüsselwörter mit Escapezeichen versehen bereitgestellt.

* *Compilerfehler behoben.* Wenn den Compiler Verhalten echtzeiteinschränkungen ein dokumentiertes Verhalten in die eigentliche Spezifikation dieser ist, beheben das Verhalten des Compilers entsprechend der dokumentierte Verhalten möglicherweise erforderlich.

* *Fehler der Spezifikation.* Wenn der Compiler konsistent mit der Sprachspezifikation, aber die eigentliche Spezifikation dieser ist eindeutig falsch ist, ändern die eigentliche Spezifikation dieser und das Verhalten des Compilers kann erforderlich sein. Der Ausdruck "offensichtlich falsche" bedeutet, dass das dokumentierte Verhalten was klare und unzweideutige Mehrheit der Benutzer erwarten Leistungsindikator wird ausgeführt und wünschenswert Verhalten für Benutzer erzeugt.

* *Spezifikation Mehrdeutigkeit.* Wenn die eigentliche Spezifikation dieser sollten darlegen, was geschieht in einer bestimmten Situation jedoch nicht, und der Compiler behandelt die Situation, in einer Weise, die inkonsistente oder offensichtlich falsch (dieselbe Definition aus der vorherigen Punkt) ist, klärende der Spezifikation und korrigieren das Verhalten des Compilers können erforderlich sein. Das heißt, wenn die Spezifikation behandelt Fällen a, b, d und e, lässt jedoch eine Erwähnung von Was geschieht, in der Groß-/Kleinschreibung c, und der Compiler verhält sich falsch, für den Fall, dass c, es erforderlich sein kann, was geschieht in der Groß-/Kleinschreibung c, und ändern Sie das Verhalten des Compilers entsprechend zu dokumentieren. (Beachten Sie, dass wenn die Spezifikation als mehrdeutig war in einer Situation was geschieht, und der Compiler verhält sich in einer Weise, die nicht eindeutig falsch ist, das Verhalten des Compilers ist der de-facto-Spezifikation.)

* *Durchführen von Laufzeitfehlern in Kompilierungsfehler.* In einer Situation, in dem Code 100 ist % zur Laufzeit fehlschlagen (z. B. enthält der Code des Benutzer Fehler eindeutigen) garantiert, kann es wünschenswert sein, einen Fehler während der Kompilierung hinzufügen, der die Situation abfängt.

* *Unterdrücken der Spezifikation.* Wenn von die Sprachspezifikation nicht ausdrücklich zugelassen oder verweigert wird, eine bestimmte Situation und der Compiler werden, behandelt die Situation, in einer Weise, die nicht erwünscht (wenn das Verhalten des Compilers offensichtlich falsch war, würde es einen Spezifikation-Fehler, nicht um eine Spezifikation ist Auslassung), kann es erforderlich sein zu verdeutlichen die Spezifikation, und ändern das Verhalten des Compilers. Zusätzlich zu den üblichen Auswirkungsanalyse werden Änderungen dieser Art weiter eingeschränkt Fälle, in denen die Auswirkungen der Änderung gilt extrem minimal sein, und der Vorteil für Entwickler sehr hoch ist.

* *Neue Features.* Im Allgemeinen sollten Einführung in die neuen Features nicht vorhandene Teile der Sprachspezifikation oder des vorhandenen Verhaltens des Compilers ändern. In der Situation, in dem ein neues Feature eingeführt erfordert eine Änderung der vorhandenen Language-Spezifikation, ist eine solche Kompatibilität unterbrochen sinnvoll, nur, wenn die Auswirkungen äußerst gering sein wäre und der Vorteil, dass das Feature hoch ist.

* *Sicherheit.* In außergewöhnlichen Fällen erfordern die Sicherheitsrisiken, die eine Kompatibilität Unterbrechung, z. B. das Entfernen oder ändern eine Funktion, die grundsätzlich unsicher ist, und ein klar Sicherheitsrisiko für Benutzer.

Die folgenden Situationen sind nicht zulässigen Gründe für die Einführung von Kompatibilität unterbrochen:

* *Verhalten nicht erwünscht oder zum Feiern.* Language Design oder Compiler-Verhalten ist sinnvoll, aber als nicht erwünscht oder im Nachhinein zum Feiern ist keine Begründung für die Abwärtskompatibilität zu unterbrechen. Stattdessen muss die Sprache als veraltet, beschriebene Vorgang unten verwendet werden.

* *Irgendetwas anderes.* Andernfalls bleibt Compilerverhalten abwärtskompatibel.

### <a name="impact-criteria"></a>Auswirkungen von Kriterien

Beim beurteilen, ob eine Unterbrechung der Kompatibilität akzeptabel wäre werden verschiedene Kriterien verwendet, um zu bestimmen, was die Auswirkungen der Änderung sein könnte. Je größer die Auswirkung, unterbricht je höher die Messlatte für die die Kompatibilität zu akzeptieren.

Die Kriterien sind:

* Was ist der Umfang der Änderung? Das heißt, wie viele Programme wahrscheinlich betroffen sein werden? Wie viele Benutzer betroffen werden können? Werden erläutert, wie allgemeine sie Code schreiben, die von der Änderung betroffen sind?

* Sind problemumgehungen vorhanden, um das gleiche Verhalten vor der Änderung zu erhalten?

* Wie offensichtlich ist die Änderung? Erhalten Benutzer unmittelbar Feedback, die etwas geändert hat oder ihre Programme einfach weiter ausgeführt, anders?

* Kann die Änderung während des Upgrades angemessen werden behandelt? Ist es möglich, ein Tool zu schreiben, finden die Situation, in der die Änderung, mit der perfekten Genauigkeit auftritt, und ändern Sie den Code, um die Änderung zu umgehen können?

* Was ist das Feedback über die Änderung?

### <a name="language-deprecation"></a>Language-Einstellung

Im Laufe der Zeit Teile der Sprache oder der Compiler möglicherweise als veraltet markiert werden. Wie bereits zuvor erwähnt, ist es nicht akzeptabel, Kompatibilität, um diese als veraltet markierte Funktionen entfernen aufgehoben werden. Stattdessen müssen die folgenden Schritte erfasst werden:

1. Erhalten eine Funktion, die in Version vorhanden ist *ein* von Visual Studio Feedback aus der Benutzercommunity auf veraltete Features und vollständige Beachten Sie, dass angegeben wird, bevor alle veralteten Entscheidung getroffen wird, angeforderten werden muss. Der Prozess für die Einstellung möglicherweise rückgängig gemacht oder zu einem beliebigen Zeitpunkt basierend auf Communityfeedback Benutzer abgebrochen.

2. Vollversion (d. h. keine Point-Release) *B* von Visual Studio muss mit dem Compiler-Warnungen, die als veraltet markierte Nutzung warnen freigegeben werden. Die Warnungen, die in der Standardeinstellung befinden müssen, und können deaktiviert werden. Die veralteten müssen eindeutig in der Produktdokumentation enthalten und im Internet dokumentiert werden.

3. Eine vollständige Version *C* von Visual Studio muss mit dem Compiler-Warnungen, die nicht deaktiviert werden freigegeben werden.

4. Eine vollständige Version *D* von Visual Studio müssen anschließend freigegeben werden mit den veralteten compilerwarnungen, die in den Compiler-Fehler konvertiert. Die Version von *D* muss erfolgen, nach dem Ende der Mainstream-Support-Phase (5 Jahre zum Zeitpunkt dieses Artikels) Version *ein*.

5. Zum Schluss eine Version *E* des können Visual Studio freigegeben werden, die die Compilerfehler entfernt.

Änderungen, die nicht innerhalb dieses veraltete Frameworks behandelt werden können, sind nicht zulässig.
