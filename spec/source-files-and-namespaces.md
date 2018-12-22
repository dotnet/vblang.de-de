# <a name="source-files-and-namespaces"></a><span data-ttu-id="889d6-101">Quelldateien und Namespaces</span><span class="sxs-lookup"><span data-stu-id="889d6-101">Source Files and Namespaces</span></span>

<span data-ttu-id="889d6-102">Eine Visual Basic-Programm besteht aus einen oder mehrere Quelldateien.</span><span class="sxs-lookup"><span data-stu-id="889d6-102">A Visual Basic program consists of one or more source files.</span></span> <span data-ttu-id="889d6-103">Wenn ein Programm kompiliert wird, werden alle Quelldateien zusammen verarbeitet; Daher können die Quelldateien von, möglicherweise in einer kreisförmigen Weise, ohne dass der Vorwärtsdeklaration abhängen.</span><span class="sxs-lookup"><span data-stu-id="889d6-103">When a program is compiled, all of the source files are processed together; thus, source files can depend on each other, possibly in a circular fashion, without any forward-declaration requirement.</span></span> <span data-ttu-id="889d6-104">Die Reihenfolge der Deklarationen in den Programmtext im Text ist im Allgemeinen ohne Bedeutung.</span><span class="sxs-lookup"><span data-stu-id="889d6-104">The textual order of declarations in the program text is generally of no significance.</span></span>

<span data-ttu-id="889d6-105">Eine Quelldatei besteht aus einer optionalen Gruppe von optionsanweisungen, "Import"-Anweisungen und Attribute, die durch einen Namespacetext eingehalten werden.</span><span class="sxs-lookup"><span data-stu-id="889d6-105">A source file consists of an optional set of option statements, import statements, and attributes, which are followed by a namespace body.</span></span> <span data-ttu-id="889d6-106">Die Attribute, die einzelnen entweder müssen die `Assembly` oder `Module` Modifizierer verwenden, gelten für die .NET Framework-Assembly oder das Modul, die durch die Kompilierung erzeugt.</span><span class="sxs-lookup"><span data-stu-id="889d6-106">The attributes, which must each have either the `Assembly` or `Module` modifier, apply to the .NET assembly or module produced by the compilation.</span></span> <span data-ttu-id="889d6-107">Der Text der Quelldatei fungiert als eine implizite Namespacedeklaration für den globalen Namespace, dies bedeutet, dass alle Deklarationen, die auf der obersten Ebene einer Quelldatei im globalen Namespace befinden.</span><span class="sxs-lookup"><span data-stu-id="889d6-107">The body of the source file functions as an implicit namespace declaration for the global namespace, meaning that all declarations at the top level of a source file are placed in the global namespace.</span></span> <span data-ttu-id="889d6-108">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="889d6-108">For example:</span></span>

<span data-ttu-id="889d6-109">FileA.vb:</span><span class="sxs-lookup"><span data-stu-id="889d6-109">FileA.vb:</span></span>

```vb
Class A
End Class
```

<span data-ttu-id="889d6-110">FileB.vb:</span><span class="sxs-lookup"><span data-stu-id="889d6-110">FileB.vb:</span></span>

```vb
Class B
End Class
```

<span data-ttu-id="889d6-111">Die zwei Quelldateien auf den globalen Namespace, in diesem Fall Deklarieren von zwei Klassen mit den voll gekennzeichneten Namen tragen `A` und `B`.</span><span class="sxs-lookup"><span data-stu-id="889d6-111">The two source files contribute to the global namespace, in this case declaring two classes with the fully qualified names `A` and `B`.</span></span> <span data-ttu-id="889d6-112">Da die zwei Quelldateien zum selben Deklarationsabschnitt beitragen möchten, wäre ein Fehler gewesen, wenn jeweils eine Deklaration eines Elements mit dem gleichen Namen enthalten.</span><span class="sxs-lookup"><span data-stu-id="889d6-112">Because the two source files contribute to the same declaration space, it would have been an error if each contained a declaration of a member with the same name.</span></span>

<span data-ttu-id="889d6-113">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="889d6-113">__Note.__</span></span> <span data-ttu-id="889d6-114">Die kompilierungsumgebung kann die Namespacedeklarationen überschreiben, die in denen implizit eine Quelldatei kopiert wird.</span><span class="sxs-lookup"><span data-stu-id="889d6-114">The compilation environment may override the namespace declarations into which a source file is implicitly placed.</span></span>

<span data-ttu-id="889d6-115">Wenn nicht anders angegeben können Anweisungen in Visual Basic-Programms von einem Zeilenabschluss oder von einem Doppelpunkt beendet werden.</span><span class="sxs-lookup"><span data-stu-id="889d6-115">Except where noted, statements within a Visual Basic program can be terminated either by a line terminator or by a colon.</span></span>

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

## <a name="program-startup-and-termination"></a><span data-ttu-id="889d6-116">Programmstart und-Beendigung</span><span class="sxs-lookup"><span data-stu-id="889d6-116">Program Startup and Termination</span></span>

<span data-ttu-id="889d6-117">Programmstart tritt auf, wenn die ausführungsumgebung eine angegebene Methode, ausgeführt wird, wie des Programms die bezeichnet *Einstiegspunkt*.</span><span class="sxs-lookup"><span data-stu-id="889d6-117">Program startup occurs when the execution environment executes a designated method, which is referred to as the program's *entry point*.</span></span> <span data-ttu-id="889d6-118">Diese Einstiegspunktmethode, die immer genannt werden muss `Main`, freigegeben werden müssen, können nicht in einem generischen Typ enthalten sein, darf keine Async-Modifizierer aufweisen und muss einen der folgenden Signaturen aufweisen:</span><span class="sxs-lookup"><span data-stu-id="889d6-118">This entry point method, which must always be named `Main`, must be shared, cannot be contained in a generic type, cannot have the async modifier, and must have one of the following signatures:</span></span>

```vb
Sub Main()
Sub Main(args() As String)
Function Main() As Integer
Function Main(args() As String) As Integer
```

<span data-ttu-id="889d6-119">Der Zugriff auf die Einstiegspunktmethode ist irrelevant.</span><span class="sxs-lookup"><span data-stu-id="889d6-119">The accessibility of the entry point method is irrelevant.</span></span> <span data-ttu-id="889d6-120">Wenn ein Programm mehr als ein geeigneter Einstiegspunkt enthält, muss die kompilierungsumgebung einer als Einstiegspunkt festlegen.</span><span class="sxs-lookup"><span data-stu-id="889d6-120">If a program contains more than one suitable entry point, the compilation environment must designate one as the entry point.</span></span> <span data-ttu-id="889d6-121">Andernfalls tritt ein Kompilierungsfehler auf.</span><span class="sxs-lookup"><span data-stu-id="889d6-121">Otherwise, a compile-time error occurs.</span></span> <span data-ttu-id="889d6-122">Die kompilierungsumgebung kann auch eine Einstiegspunktmethode erstellen, sofern noch nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="889d6-122">The compilation environment may also create an entry point method if one does not exist.</span></span>

<span data-ttu-id="889d6-123">Wenn ein Programm beginnt, wenn der Einstiegspunkt auf einen Parameter aufweist, enthält das Argument der ausführungsumgebung vom die Befehlszeilenargumente an das Programm als Zeichenfolgen dargestellt.</span><span class="sxs-lookup"><span data-stu-id="889d6-123">When a program begins, if the entry point has a parameter, the argument supplied by the execution environment contains the command-line arguments to the program represented as strings.</span></span> <span data-ttu-id="889d6-124">Wenn der Einstiegspunkt auf einen Rückgabetyp hat `Integer`, und klicken Sie dann der von der Funktion zurückgegebene Wert als Ergebnis des Programms an der ausführungsumgebung zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="889d6-124">If the entry point has a return type of `Integer`, then the value returned from the function is returned to the execution environment as the result of the program.</span></span>

<span data-ttu-id="889d6-125">In jeder anderen Hinsicht Verhalten sich genauso wie andere Methoden einstiegspunktmethoden.</span><span class="sxs-lookup"><span data-stu-id="889d6-125">In all other respects, entry point methods behave in the same manner as other methods.</span></span> <span data-ttu-id="889d6-126">Wenn die Ausführung auf den Aufruf der Einstiegspunktmethode, die von der ausführungsumgebung erfolgt, wird das Programm beendet.</span><span class="sxs-lookup"><span data-stu-id="889d6-126">When execution leaves the invocation of the entry point method made by the execution environment, the program terminates.</span></span>

## <a name="compilation-options"></a><span data-ttu-id="889d6-127">Kompilierungsoptionen</span><span class="sxs-lookup"><span data-stu-id="889d6-127">Compilation Options</span></span>

<span data-ttu-id="889d6-128">Eine Quelldatei kann Kompilierungsoptionen angeben, in der Source-Code mit *option Anweisungen*.</span><span class="sxs-lookup"><span data-stu-id="889d6-128">A source file can specify compilation options in the source code using *option statements*.</span></span>

```antlr
OptionStatement
    : OptionExplicitStatement
    | OptionStrictStatement
    | OptionCompareStatement
    | OptionInferStatement
    ;
```

<span data-ttu-id="889d6-129">Ein `Option` Aussage gilt nur für die Quelldatei, in dem er angezeigt wird, und nur eine für jede Art von `Option` Anweisung darf in einer Quelldatei verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="889d6-129">An `Option` statement applies only to the source file in which it appears, and only one of each type of `Option` statement may appear in a source file.</span></span> <span data-ttu-id="889d6-130">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="889d6-130">For example:</span></span>

```vb
Option Strict On
Option Compare Text
Option Strict Off    ' Not allowed, Option Strict is already specified.
Option Compare Text  ' Not allowed, Option Compare is already specified.
```

<span data-ttu-id="889d6-131">Es gibt vier Kompilierungsoptionen: strikte Typsemantik, explizite Deklaration-Semantik, die Semantik für Vergleichsvorgänge und Typ der lokalen Variablen Typrückschluss-Semantik.</span><span class="sxs-lookup"><span data-stu-id="889d6-131">There are four compilation options: strict type semantics, explicit declaration semantics, comparison semantics, and local variable type inference semantics.</span></span> <span data-ttu-id="889d6-132">Wenn eine Quelldatei nicht mit eine bestimmten beinhaltet `Option` -Anweisung, und klicken Sie dann auf die kompilierungsumgebung bestimmt, welche bestimmten Satz von Semantik verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="889d6-132">If a source file does not include a particular `Option` statement, then the compilation environment determines which particular set of semantics will be used.</span></span> <span data-ttu-id="889d6-133">Es gibt auch eine fünfte Kompilierungsoption,, Überprüfungen auf Ganzzahlüberlauf, die über die kompilierungsumgebung kann nur angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="889d6-133">There is also a fifth compilation option, integer overflow checks, which can only be specified through the compilation environment.</span></span>


### <a name="option-explicit-statement"></a><span data-ttu-id="889d6-134">Option Explicit-Anweisung</span><span class="sxs-lookup"><span data-stu-id="889d6-134">Option Explicit Statement</span></span>

<span data-ttu-id="889d6-135">Die `Option Explicit` -Anweisung bestimmt, ob lokale Variablen implizit deklariert werden können.</span><span class="sxs-lookup"><span data-stu-id="889d6-135">The `Option Explicit` statement determines whether local variables may be implicitly declared.</span></span> <span data-ttu-id="889d6-136">Die Schlüsselwörter `On` oder `Off` kann die Anweisung folgen, wenn keine Angabe gemacht wird, wird standardmäßig `On`.</span><span class="sxs-lookup"><span data-stu-id="889d6-136">The keywords `On` or `Off` may follow the statement; if neither is specified, the default is `On`.</span></span> <span data-ttu-id="889d6-137">Wenn keine Anweisung in einer Datei angegeben ist, bestimmt der kompilierungsumgebung an, das verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="889d6-137">If no statement is specified in a file, the compilation environment determines which will be used.</span></span>

```antlr
OptionExplicitStatement
    : 'Option' 'Explicit' OnOff? StatementTerminator
    ;

OnOff
    : 'On' | 'Off'
    ;
```


<span data-ttu-id="889d6-138">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="889d6-138">__Note.__</span></span> <span data-ttu-id="889d6-139">`Explicit` und `Off` sind keine reservierten Wörter.</span><span class="sxs-lookup"><span data-stu-id="889d6-139">`Explicit` and `Off` are not reserved words.</span></span>

```vb
Option Explicit Off

Module Test
    Sub Main()
        x = 5 ' Valid because Option Explicit is off.
    End Sub
End Module
```

<span data-ttu-id="889d6-140">In diesem Beispiel wird die lokale Variable `x` wird durch Zuweisung implizit deklariert.</span><span class="sxs-lookup"><span data-stu-id="889d6-140">In this example, the local variable `x` is implicitly declared by assigning to it.</span></span> <span data-ttu-id="889d6-141">Der Typ von `x` lautet `Object`.</span><span class="sxs-lookup"><span data-stu-id="889d6-141">The type of `x` is `Object`.</span></span>


### <a name="option-strict-statement"></a><span data-ttu-id="889d6-142">Option Strict Statement</span><span class="sxs-lookup"><span data-stu-id="889d6-142">Option Strict Statement</span></span>

<span data-ttu-id="889d6-143">Die `Option Strict` -Anweisung bestimmt, ob Konvertierungen und Vorgänge für `Object` unterliegen strikte oder die flexible Typsemantik und gibt an, ob Typen als implizit typisiert werden `Object` ohne `As` -Klausel angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="889d6-143">The `Option Strict` statement determines whether conversions and operations on `Object` are governed by strict or permissive type semantics and whether types are implicitly typed as `Object` if no `As` clause is specified.</span></span> <span data-ttu-id="889d6-144">Die-Anweisung darauf folgt möglicherweise durch die Schlüsselwörter `On` oder `Off`; Wenn keine Angabe gemacht wird, der Standardwert ist `On`.</span><span class="sxs-lookup"><span data-stu-id="889d6-144">The statement may be followed by the keywords `On` or `Off`; if neither is specified, the default is `On`.</span></span> <span data-ttu-id="889d6-145">Wenn keine Anweisung in einer Datei angegeben ist, bestimmt der kompilierungsumgebung an, das verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="889d6-145">If no statement is specified in a file, the compilation environment determines which will be used.</span></span>

```antlr
OptionStrictStatement
    : 'Option' 'Strict' OnOff? StatementTerminator
    ;
```

<span data-ttu-id="889d6-146">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="889d6-146">__Note.__</span></span> <span data-ttu-id="889d6-147">`Strict` und `Off` sind keine reservierten Wörter.</span><span class="sxs-lookup"><span data-stu-id="889d6-147">`Strict` and `Off` are not reserved words.</span></span>

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

<span data-ttu-id="889d6-148">Bei der strikten Semantik sind die folgenden nicht zulässig:</span><span class="sxs-lookup"><span data-stu-id="889d6-148">Under strict semantics, the following are disallowed:</span></span>

* <span data-ttu-id="889d6-149">Einschränkende Konvertierungen ohne einen expliziten Cast-Operator.</span><span class="sxs-lookup"><span data-stu-id="889d6-149">Narrowing conversions without an explicit cast operator.</span></span>

* <span data-ttu-id="889d6-150">Späte Bindung.</span><span class="sxs-lookup"><span data-stu-id="889d6-150">Late binding.</span></span>

* <span data-ttu-id="889d6-151">Vorgänge für Typ `Object` außer `TypeOf`... `Is`, `Is`, und `IsNot`.</span><span class="sxs-lookup"><span data-stu-id="889d6-151">Operations on type `Object` other than `TypeOf`...`Is`, `Is`, and `IsNot`.</span></span>

* <span data-ttu-id="889d6-152">Das Auslassen der `As` -Klausel in einer Deklaration, die nicht über einen abgeleiteten Typ besitzt.</span><span class="sxs-lookup"><span data-stu-id="889d6-152">Omitting the `As` clause in a declaration that does not have an inferred type.</span></span>


### <a name="option-compare-statement"></a><span data-ttu-id="889d6-153">Option Compare-Anweisung</span><span class="sxs-lookup"><span data-stu-id="889d6-153">Option Compare Statement</span></span>

<span data-ttu-id="889d6-154">Die `Option Compare` -Anweisung bestimmt die Semantik der Vergleich von Zeichenfolgen.</span><span class="sxs-lookup"><span data-stu-id="889d6-154">The `Option Compare` statement determines the semantics of string comparisons.</span></span> <span data-ttu-id="889d6-155">Vergleich von Zeichenfolgen werden ausgeführt, entweder mithilfe von binäre Vergleiche (in dem der binäre Unicode-Wert, der einzelnen Zeichen ist im Vergleich) oder Textvergleichen (in der die lexikalische Bedeutung der einzelnen Zeichen ist im Vergleich mit der aktuellen Kultur).</span><span class="sxs-lookup"><span data-stu-id="889d6-155">String comparisons are carried out either using binary comparisons (in which the binary Unicode value of each character is compared) or text comparisons (in which the lexical meaning of each character is compared using the current culture).</span></span> <span data-ttu-id="889d6-156">Wenn keine Anweisung in eine Datei angegeben ist, steuert die kompilierungsumgebung an, die Art des Vergleichs verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="889d6-156">If no statement is specified in a file, the compilation environment controls which type of comparison will be used.</span></span>

```antlr
OptionCompareStatement
    : 'Option' 'Compare' CompareOption StatementTerminator
    ;

CompareOption
    : 'Binary' | 'Text'
    ;
```

<span data-ttu-id="889d6-157">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="889d6-157">__Note.__</span></span> <span data-ttu-id="889d6-158">`Compare`, `Binary`, und `Text` sind keine reservierten Wörter.</span><span class="sxs-lookup"><span data-stu-id="889d6-158">`Compare`, `Binary`, and `Text` are not reserved words.</span></span>

```vb
Option Compare Text

Module Test
    Sub Main()
        Console.WriteLine("a" = "A")    ' Prints True.
    End Sub
End Module
```

<span data-ttu-id="889d6-159">In diesem Fall erfolgt der Zeichenfolgenvergleich mithilfe eines Textvergleichs, die Groß-/Kleinschreibung nicht berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="889d6-159">In this case, the string comparison is done using a text comparison that ignores case differences.</span></span> <span data-ttu-id="889d6-160">Wenn `Option Compare Binary` angegeben worden wäre, und klicken Sie dann diese gedruckt haben, würden `False`.</span><span class="sxs-lookup"><span data-stu-id="889d6-160">If `Option Compare Binary` had been specified, then this would have printed `False`.</span></span>


### <a name="integer-overflow-checks"></a><span data-ttu-id="889d6-161">Überprüfungen auf Ganzzahlüberlauf</span><span class="sxs-lookup"><span data-stu-id="889d6-161">Integer Overflow Checks</span></span>

<span data-ttu-id="889d6-162">Operationen mit ganzen Zahlen können aktiviert oder nicht zur Laufzeit für überlaufbedingungen überprüft werden.</span><span class="sxs-lookup"><span data-stu-id="889d6-162">Integer operations can either be checked or not checked for overflow conditions at run time.</span></span> <span data-ttu-id="889d6-163">Wenn überlaufbedingungen überprüft werden und eine ganzzahlige Operation überläuft, eine `System.OverflowException` Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="889d6-163">If overflow conditions are checked and an integer operation overflows, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="889d6-164">Wenn Überlaufbedingungen nicht aktiviert werden, lösen Überprüfungen auf Ganzzahlüberlauf keine Ausnahme aus.</span><span class="sxs-lookup"><span data-stu-id="889d6-164">If overflow conditions are not checked, integer operation overflows do not throw an exception.</span></span> <span data-ttu-id="889d6-165">Die kompilierungsumgebung bestimmt, ob diese Option aktiviert oder deaktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="889d6-165">The compilation environment determines whether this option is on or off.</span></span>

### <a name="option-infer-statement"></a><span data-ttu-id="889d6-166">Option Infer-Anweisung</span><span class="sxs-lookup"><span data-stu-id="889d6-166">Option Infer Statement</span></span>

<span data-ttu-id="889d6-167">Die `Option Infer` -Anweisung bestimmt, ob der lokale Variablendeklarationen, die keine `As` -Klausel aufweisen, einen abgeleiteten Typ oder einen verwenden `Object`.</span><span class="sxs-lookup"><span data-stu-id="889d6-167">The `Option Infer` statement determines whether local variable declarations that have no `As` clause have an inferred type or use `Object`.</span></span> <span data-ttu-id="889d6-168">Die-Anweisung darauf folgt möglicherweise durch die Schlüsselwörter `On` oder `Off`; Wenn keine Angabe gemacht wird, der Standardwert ist `On`.</span><span class="sxs-lookup"><span data-stu-id="889d6-168">The statement may be followed by the keywords `On` or `Off`; if neither is specified, the default is `On`.</span></span> <span data-ttu-id="889d6-169">Wenn keine Anweisung in einer Datei angegeben ist, bestimmt der kompilierungsumgebung an, das verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="889d6-169">If no statement is specified in a file, the compilation environment determines which will be used.</span></span>

```antlr
OptionInferStatement
    : 'Option' 'Infer' OnOff? StatementTerminator
    ;
```

<span data-ttu-id="889d6-170">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="889d6-170">__Note.__</span></span> <span data-ttu-id="889d6-171">`Infer` und `Off` sind keine reservierten Wörter.</span><span class="sxs-lookup"><span data-stu-id="889d6-171">`Infer` and `Off` are not reserved words.</span></span>

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


## <a name="imports-statement"></a><span data-ttu-id="889d6-172">Imports-Anweisung</span><span class="sxs-lookup"><span data-stu-id="889d6-172">Imports Statement</span></span>

<span data-ttu-id="889d6-173">`Imports` Anweisungen importieren die Namen von Entitäten in einer Quelldatei, können die Namen ohne Qualifikation referenziert werden, oder Importieren eines Namespace für die Verwendung in XML-Ausdrücken.</span><span class="sxs-lookup"><span data-stu-id="889d6-173">`Imports` statements import the names of entities into a source file, allowing the names to be referenced without qualification, or import a namespace for use in XML expressions.</span></span>

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

<span data-ttu-id="889d6-174">In den Memberdeklarationen in einer Quelldatei, die enthält ein `Imports` -Anweisung, die im angegebenen Namespace enthaltenen Typen kann direkt verwiesen werden, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="889d6-174">Within member declarations in a source file that contains an `Imports` statement, the types contained in the given namespace can be referenced directly, as seen in the following example:</span></span>

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

<span data-ttu-id="889d6-175">Hier in der Quelldatei, die Typmember Namespace `N1.N2` direkt verfügbar sind, und daher Klasse `N3.B` leitet sich von der Klasse `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="889d6-175">Here, within the source file, the type members of namespace `N1.N2` are directly available, and thus class `N3.B` derives from class `N1.N2.A`.</span></span>

<span data-ttu-id="889d6-176">`Imports` -Anweisungen müssen nach jedem angezeigt werden `Option` Anweisungen aber vor allen Deklarationen geben.</span><span class="sxs-lookup"><span data-stu-id="889d6-176">`Imports` statements must appear after any `Option` statements but before any type declarations.</span></span> <span data-ttu-id="889d6-177">Die kompilierungsumgebung kann auch implizite definieren `Imports` Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="889d6-177">The compilation environment may also define implicit `Imports` statements.</span></span>

<span data-ttu-id="889d6-178">`Imports` Anweisungen Namen in einer Quelldatei zur Verfügung stellen, aber nicht alle Elemente in den globalen Namespace Deklarationsabschnitt deklarieren.</span><span class="sxs-lookup"><span data-stu-id="889d6-178">`Imports` statements make names available in a source file, but do not declare anything in the global namespace's declaration space.</span></span> <span data-ttu-id="889d6-179">Der Bereich der Namen von importiert eine `Imports` Anweisung erstreckt sich über die Member-Namespacedeklarationen in der Quelldatei enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="889d6-179">The scope of the names imported by an `Imports` statement extends over the namespace member declarations contained in the source file.</span></span> <span data-ttu-id="889d6-180">Im Rahmen einer `Imports` -Anweisung speziell keinen anderen `Imports` -Anweisungen, noch anderen Quelldateien.</span><span class="sxs-lookup"><span data-stu-id="889d6-180">The scope of an `Imports` statement specifically does not include other `Imports` statements, nor does it include other source files.</span></span> <span data-ttu-id="889d6-181">`Imports` Anweisungen können nicht miteinander verwenden.</span><span class="sxs-lookup"><span data-stu-id="889d6-181">`Imports` statements may not refer to one another.</span></span>

<span data-ttu-id="889d6-182">In diesem Beispiel ist die letzte `Imports` Anweisung befindet sich in der Fehler, da er durch den ersten Importalias nicht betroffen ist.</span><span class="sxs-lookup"><span data-stu-id="889d6-182">In this example, the last `Imports` statement is in error because it is not affected by the first import alias.</span></span>

```vb
Imports R1 = N1 ' OK.
Imports R2 = N1.N2 ' OK.
Imports R3 = R1.N2 ' Error: Can't refer to R1.

Namespace N1.N2
End Namespace
```

<span data-ttu-id="889d6-183">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="889d6-183">__Note.__</span></span> <span data-ttu-id="889d6-184">Der Namespace oder Typ Namen in `Imports` Anweisungen immer behandelt, als ob sie den vollqualifizierten sind.</span><span class="sxs-lookup"><span data-stu-id="889d6-184">The namespace or type names that appear in `Imports` statements are always treated as if they are fully qualified.</span></span> <span data-ttu-id="889d6-185">Das heißt, der am weitesten links stehende Bezeichner in einem Namespace oder Typ Namen löst immer im globalen Namespace und der Rest der Lösung, die gemäß den normalen Regeln zur namensauflösung fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="889d6-185">That is, the leftmost identifier in a namespace or type name always resolves in the global namespace and the rest of the resolution proceeds according to normal name resolution rules.</span></span> <span data-ttu-id="889d6-186">Dies ist der einzige Ort, in der Sprache, die eine solche Regel gilt. die Regel stellt sicher, dass ein Name nicht vollständig von Qualifikation ausgeblendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="889d6-186">This is the only place in the language that applies such a rule; the rule ensures that a name cannot be completely hidden from qualification.</span></span> <span data-ttu-id="889d6-187">Ohne die Regel ein Wenn Sie ein Namen im globalen Namespace in einer bestimmten Quelle-Datei ausgeblendet wurden wäre nicht möglich, geben Sie keine Namen aus diesem Namespace qualifizierten so es.</span><span class="sxs-lookup"><span data-stu-id="889d6-187">Without the rule, if a name in the global namespace were hidden in a particular source file, it would be impossible to specify any names from that namespace in a qualified way.</span></span>

<span data-ttu-id="889d6-188">In diesem Beispiel die `Imports` immer die Anweisung verweist auf die globale `System` -Namespace und nicht auf die Klasse in der Quelldatei.</span><span class="sxs-lookup"><span data-stu-id="889d6-188">In this example, the `Imports` statement always refers to the global `System` namespace, and not the class in the source file.</span></span>

```vb
Imports System   ' Imports the namespace, not the class.

Class System
End Class
```


### <a name="import-aliases"></a><span data-ttu-id="889d6-189">Importaliase</span><span class="sxs-lookup"><span data-stu-id="889d6-189">Import Aliases</span></span>

<span data-ttu-id="889d6-190">Ein *Importalias* definiert einen Alias für einen Namespace oder Typ.</span><span class="sxs-lookup"><span data-stu-id="889d6-190">An *import alias* defines an alias for a namespace or type.</span></span>

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

<span data-ttu-id="889d6-191">Hier in der Quelldatei `A` ist ein Alias für `N1.N2.A`, und daher `N3.B` leitet sich von der Klasse `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="889d6-191">Here, within the source file, `A` is an alias for `N1.N2.A`, and thus class `N3.B` derives from class `N1.N2.A`.</span></span> <span data-ttu-id="889d6-192">Die gleiche Wirkung erhalten Sie durch Erstellen eines Alias `R` für `N1.N2` verweisen Sie einfach auf `R.A`:</span><span class="sxs-lookup"><span data-stu-id="889d6-192">The same effect can be obtained by creating an alias `R` for `N1.N2` and then referencing `R.A`:</span></span>

```vb
Imports R = N1.N2

Namespace N3
    Class B
        Inherits R.A
    End Class
End Namespace
```

<span data-ttu-id="889d6-193">Der Bezeichner eines Import-Alias muss innerhalb des Deklarationsabschnitts des globalen Namespaces (nicht nur den globalen Namespace-Deklarationen in der Quelldatei, in dem der Importalias definiert wird) eindeutig sein, obwohl er keinen Namen im globalen Namespaces deklariert ist Deklarationsabschnitt.</span><span class="sxs-lookup"><span data-stu-id="889d6-193">The identifier of an import alias must be unique within the declaration space of the global namespace (not just the global namespace declaration in the source file in which the import alias is defined), even though it does not declare a name in the global namespace's declaration space.</span></span>

<span data-ttu-id="889d6-194">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="889d6-194">__Note.__</span></span> <span data-ttu-id="889d6-195">Deklarationen in einem Modul führen Namen in der enthaltenden Deklarationsabschnitt.</span><span class="sxs-lookup"><span data-stu-id="889d6-195">Declarations in a module do not introduce names into the containing declaration space.</span></span> <span data-ttu-id="889d6-196">Daher ist es für eine Deklaration in einem Modul, haben den gleichen Namen wie ein Importalias gültig, obwohl die Deklaration des Namens im Deklarationsbereich mit der zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="889d6-196">Thus, it is valid for a declaration in a module to have the same name as an import alias, even though the declaration's name will be accessible in the containing declaration space.</span></span>

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

<span data-ttu-id="889d6-197">Hier ist der globale Namespace bereits ein Element enthält `A`, daher wird ein Fehler für ein Importalias für diesen Bezeichner verwenden.</span><span class="sxs-lookup"><span data-stu-id="889d6-197">Here, the global namespace already contains a member `A`, so it is an error for an import alias to use that identifier.</span></span> <span data-ttu-id="889d6-198">Es ist ebenso einen Fehler für zwei oder mehr Importaliase in der gleichen Quelldatei Aliase mit demselben Namen zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="889d6-198">It is likewise an error for two or more import aliases in the same source file to declare aliases by the same name.</span></span>

<span data-ttu-id="889d6-199">Ein Importalias kann einen Alias für jeden Namespace oder Typ erstellen.</span><span class="sxs-lookup"><span data-stu-id="889d6-199">An import alias can create an alias for any namespace or type.</span></span> <span data-ttu-id="889d6-200">Zugreifen auf einen Namespace oder Typ über einen Alias führt genau dieselbe Ergebnisse wie beim Zugriff auf den Namespace oder Typ durch den deklarierten Namen.</span><span class="sxs-lookup"><span data-stu-id="889d6-200">Accessing a namespace or type through an alias yields exactly the same result as accessing the namespace or type through its declared name.</span></span>

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

<span data-ttu-id="889d6-201">Hier wird die Namen `N1.N2.A`, `R1.N2.A`, und `R2.A` sind äquivalent, und alle auf die Klasse verweisen, dessen vollqualifizierter Name ist `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="889d6-201">Here, the names `N1.N2.A`, `R1.N2.A`, and `R2.A` are equivalent, and all refer to the class whose fully qualified name is `N1.N2.A`.</span></span>

<span data-ttu-id="889d6-202">Der Import gibt an, den genauen Namen der der Namespace oder Typ, der er einen Alias erstellt, werden.</span><span class="sxs-lookup"><span data-stu-id="889d6-202">The import specifies the exact name of the namespace or type to which it is creating an alias.</span></span> <span data-ttu-id="889d6-203">Dies muss den genauen den vollqualifizierten Namen, Namespaces oder Typs sein: die normalen Regeln für die Auflösung von qualifizierten Namen (sodass z. B. Zugriff auf die Member einer Basisklasse durch eine abgeleitete Klasse) wird nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="889d6-203">This must be the exact fully qualified name of that namespace or type: it does not use the normal rules for qualified name resolution (which for instance allow access to the members of a base class through a derived class).</span></span>

<span data-ttu-id="889d6-204">Wenn ein Import-Alias verweist auf einen Typ oder Namespace, der von diesen Regeln nicht aufgelöst werden kann, klicken Sie dann die Import-Anweisung wird ignoriert (und der Compiler gibt eine Warnung).</span><span class="sxs-lookup"><span data-stu-id="889d6-204">If an import alias points to a type or namespace which cannot be resolved by these rules, then the import statement is ignored (and the compiler gives a warning).</span></span>

<span data-ttu-id="889d6-205">Darüber hinaus darf nicht der Verweis auf ein offener generischer Typ sein – alle generische Typen müssen gültige Typargumente angegeben haben, und alle Typargumente müssen von der oben genannten Regeln aufgelöst werden können.</span><span class="sxs-lookup"><span data-stu-id="889d6-205">Also, the reference cannot be to an open generic type -- all generic types must have valid type arguments supplied, and all type arguments must be resolvable by the rules above.</span></span> <span data-ttu-id="889d6-206">Eine falsche Bindung eines generischen Typs handelt es sich um einen Fehler.</span><span class="sxs-lookup"><span data-stu-id="889d6-206">Any incorrect binding of a generic type is an error.</span></span>

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

<span data-ttu-id="889d6-207">Deklarationen in der Quelldatei können den Namen des Importalias überschatten.</span><span class="sxs-lookup"><span data-stu-id="889d6-207">Declarations in the source file may shadow the import alias name.</span></span>

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

<span data-ttu-id="889d6-208">Im vorherigen Beispiel den Verweis auf `R.A` in der Deklaration der `B` verursacht einen Fehler, da `R` bezieht sich auf `N3.R`, nicht `N1.N2`.</span><span class="sxs-lookup"><span data-stu-id="889d6-208">In the preceding example the reference to `R.A` in the declaration of `B` causes an error because `R` refers to `N3.R`, not `N1.N2`.</span></span>

<span data-ttu-id="889d6-209">Ein Importalias stellt innerhalb einer bestimmten Quelldatei ein alias zur Verfügung, aber es trägt nicht die neuen Member zu den zugrunde liegenden Deklarationsabschnitt.</span><span class="sxs-lookup"><span data-stu-id="889d6-209">An import alias makes an alias available within a particular source file, but it does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="889d6-210">Das heißt, ein Importalias ist nicht transitiv, aber stattdessen wirkt sich nur auf die Quelldatei, in der er auftritt.</span><span class="sxs-lookup"><span data-stu-id="889d6-210">In other words, an import alias is not transitive, but rather affects only the source file in which it occurs.</span></span>

<span data-ttu-id="889d6-211">file1.vb:</span><span class="sxs-lookup"><span data-stu-id="889d6-211">File1.vb:</span></span>

```vb
Imports R = N1.N2

Namespace N1.N2
    Class A
    End Class
End Namespace
```

<span data-ttu-id="889d6-212">file2.vb:</span><span class="sxs-lookup"><span data-stu-id="889d6-212">File2.vb:</span></span>

```vb
Class B
    Inherits R.A ' Error, R unknown.
End Class
```

<span data-ttu-id="889d6-213">Im obigen Beispiel da im Rahmen der Importalias wird `R` erstreckt sich auch nur auf Deklarationen in der Quelldatei, in dem es enthalten ist, `R` ist in der zweiten Quelldatei unbekannt.</span><span class="sxs-lookup"><span data-stu-id="889d6-213">In the above example, because the scope of the import alias that introduces `R` only extends to declarations in the source file in which it is contained, `R` is unknown in the second source file.</span></span>


### <a name="namespace-imports"></a><span data-ttu-id="889d6-214">Namespaceimporte</span><span class="sxs-lookup"><span data-stu-id="889d6-214">Namespace Imports</span></span>

<span data-ttu-id="889d6-215">Ein *Namespaceimport* importiert alle Elemente eines Namespace oder Typ, den Bezeichner für jedes Element der Namespace oder Typ ohne Qualifikation verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="889d6-215">A *namespace import* imports all of the members of a namespace or type, allowing the identifier of each member of the namespace or type to be used without qualification.</span></span> <span data-ttu-id="889d6-216">Bei Typen ermöglicht ein Namespaceimport nur Zugriff auf freigegebenen Member des Typs ohne den Klassennamen zu qualifizieren.</span><span class="sxs-lookup"><span data-stu-id="889d6-216">In the case of types, a namespace import only allows access to the shared members of the type without requiring qualification of the class name.</span></span> <span data-ttu-id="889d6-217">Insbesondere können die Mitglieder von enumerierten Typen ohne Qualifikation verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="889d6-217">In particular, it allows the members of enumerated types to be used without qualification.</span></span>

```antlr
MembersImportsClause
    : TypeName
    ;
```

<span data-ttu-id="889d6-218">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="889d6-218">For example:</span></span>

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

<span data-ttu-id="889d6-219">Im Gegensatz zu einem Importalias weist einen Namespace importiert keine Einschränkungen auf den Namen, die importiert und können importieren, Namespaces und Typen, deren Bezeichner bereits im globalen Namespace deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="889d6-219">Unlike an import alias, a namespace import has no restrictions on the names it imports and may import namespaces and types whose identifiers are already declared within the global namespace.</span></span> <span data-ttu-id="889d6-220">Die Namen durch einen regulären Import importiert werden durch Importaliase und Deklarationen in der Quelldatei schattiert.</span><span class="sxs-lookup"><span data-stu-id="889d6-220">The names imported by a regular import are shadowed by import aliases and declarations in the source file.</span></span>

<span data-ttu-id="889d6-221">Im folgenden Beispiel `A` bezieht sich auf `N3.A` statt `N1.N2.A` in Memberdeklarationen in der `N3` Namespace.</span><span class="sxs-lookup"><span data-stu-id="889d6-221">In the following example, `A` refers to `N3.A` rather than `N1.N2.A` within member declarations in the `N3` namespace.</span></span>

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

<span data-ttu-id="889d6-222">Wenn mehr als einem importierter Namespace enthält Member mit demselben Namen (dieser Name ist kein andernfalls durch einen Importalias oder eine Deklaration schattiert), wird ein Verweis auf diesen Namen ist mehrdeutig und verursacht einen Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="889d6-222">When more than one imported namespace contains members by the same name (and that name is not otherwise shadowed by an import alias or declaration), a reference to that name is ambiguous and causes a compile-time error.</span></span>

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

<span data-ttu-id="889d6-223">Im obigen Beispiel beide `N1` und `N2` -Member enthalten `A`.</span><span class="sxs-lookup"><span data-stu-id="889d6-223">In the above example, both `N1` and `N2` contain a member `A`.</span></span> <span data-ttu-id="889d6-224">Da `N3` importiert, verweisen auf `A` in `N3` verursacht einen Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="889d6-224">Because `N3` imports both, referencing `A` in `N3` causes a compile-time error.</span></span> <span data-ttu-id="889d6-225">In diesem Fall der Konflikt kann aufgelöst werden, entweder über die Qualifikation von Verweisen auf `A`, oder durch die Einführung eines Importalias, der einen bestimmten wählt `A`, wie im folgenden Beispiel:</span><span class="sxs-lookup"><span data-stu-id="889d6-225">In this situation, the conflict can be resolved either through qualification of references to `A`, or by introducing an import alias that picks a particular `A`, as in the following example:</span></span>

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

<span data-ttu-id="889d6-226">Nur Namespaces, Klassen, Strukturen, Enumerationstypen und standard-Module importiert werden können.</span><span class="sxs-lookup"><span data-stu-id="889d6-226">Only namespaces, classes, structures, enumerated types, and standard modules may be imported.</span></span>


### <a name="xml-namespace-imports"></a><span data-ttu-id="889d6-227">XML-Namespace-Importe</span><span class="sxs-lookup"><span data-stu-id="889d6-227">XML Namespace Imports</span></span>

<span data-ttu-id="889d6-228">Ein *XML-Namespaceimport* definiert einen Namespace oder den Standardnamespace für den nicht qualifizierten XML-Ausdrücke, die innerhalb der Kompilierungseinheit.</span><span class="sxs-lookup"><span data-stu-id="889d6-228">An *XML namespace import* defines a namespace or the default namespace for unqualified XML expressions contained within the compilation unit.</span></span>

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

<span data-ttu-id="889d6-229">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="889d6-229">For example:</span></span>

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

<span data-ttu-id="889d6-230">Ein XML-Namespace den Standardnamespace, einschließlich kann nur einmal für einen bestimmten Satz von Importen definiert werden.</span><span class="sxs-lookup"><span data-stu-id="889d6-230">An XML namespace, including the default namespace, can only be defined once for a particular set of imports.</span></span> <span data-ttu-id="889d6-231">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="889d6-231">For example:</span></span>

```vb
Imports <xmlns:db="http://example.org/database-one">
' Error: namespace db is already defined
Imports <xmlns:db="http://example.org/database-two">
```


## <a name="namespaces"></a><span data-ttu-id="889d6-232">Namespaces</span><span class="sxs-lookup"><span data-stu-id="889d6-232">Namespaces</span></span>

<span data-ttu-id="889d6-233">Visual Basic-Programme werden mithilfe von Namespaces organisiert.</span><span class="sxs-lookup"><span data-stu-id="889d6-233">Visual Basic programs are organized using namespaces.</span></span> <span data-ttu-id="889d6-234">Namespaces sowohl intern ein Programm zu organisieren als auch die Möglichkeit, die Programmelemente verfügbar, um andere Programme gemacht werden zu organisieren.</span><span class="sxs-lookup"><span data-stu-id="889d6-234">Namespaces both internally organize a program as well as organize the way program elements are exposed to other programs.</span></span>

<span data-ttu-id="889d6-235">Im Gegensatz zu anderen Entitäten-Namespaces sind offen und kann deklariert werden mehrmals innerhalb desselben Programms und für viele Programme, mit jeder Deklaration Elemente denselben Namespace beiträgt.</span><span class="sxs-lookup"><span data-stu-id="889d6-235">Unlike other entities, namespaces are open-ended, and may be declared multiple times within the same program and across many programs, with each declaration contributing members to the same namespace.</span></span> <span data-ttu-id="889d6-236">Im folgenden Beispiel die beiden Namespacedeklarationen, die zum selben Deklarationsabschnitt, deklarieren zwei Klassen mit den voll gekennzeichneten Namen beitragen `N1.N2.A` und `N1.N2.B`.</span><span class="sxs-lookup"><span data-stu-id="889d6-236">In the following example, the two namespace declarations contribute to the same declaration space, declaring two classes with the fully qualified names `N1.N2.A` and `N1.N2.B`.</span></span>

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

<span data-ttu-id="889d6-237">Da die beiden Deklarationen zum selben Deklarationsabschnitt beitragen möchten, wäre es, dass ein Fehler, wenn jeder eine Deklaration eines Elements mit dem gleichen Namen enthalten.</span><span class="sxs-lookup"><span data-stu-id="889d6-237">Because the two declarations contribute to the same declaration space, it would be an error if each contained a declaration of a member with the same name.</span></span>

<span data-ttu-id="889d6-238">Es ist ein globaler Namespace, die keinen Namen und, dessen geschachtelte Namespaces und Typen ohne Qualifikation immer zugegriffen werden können.</span><span class="sxs-lookup"><span data-stu-id="889d6-238">There is a global namespace that has no name and whose nested namespaces and types can always be accessed without qualification.</span></span> <span data-ttu-id="889d6-239">Der Gültigkeitsbereich eines Namespace im globalen Namespace deklariert ist, das gesamte Programmtext.</span><span class="sxs-lookup"><span data-stu-id="889d6-239">The scope of a namespace member declared in the global namespace is the entire program text.</span></span> <span data-ttu-id="889d6-240">Andernfalls der Rahmen eines Typs oder Namespaces deklariert, in einem Namespace, dessen vollqualifizierter Name ist `N` ist der Programmtext von jeder Namespace, der den vollqualifizierten Namen des Namespaces, deren entsprechenden beginnt mit `N` oder `N` selbst.</span><span class="sxs-lookup"><span data-stu-id="889d6-240">Otherwise, the scope of a type or namespace declared in a namespace whose fully qualified name is `N` is the program text of each namespace whose corresponding namespace's fully qualified name begins with `N` or is `N` itself.</span></span> <span data-ttu-id="889d6-241">(Beachten Sie, dass ein Compiler Deklarationen in einem bestimmten Namespace standardmäßig platzieren kann.</span><span class="sxs-lookup"><span data-stu-id="889d6-241">(Note that a compiler can choose to put declarations in a particular namespace by default.</span></span> <span data-ttu-id="889d6-242">Dies wird nicht die Tatsache, dass immer noch ein globaler, unbenannter Namespace vorhanden ist geändert.)</span><span class="sxs-lookup"><span data-stu-id="889d6-242">This does not alter the fact that there is still a global, unnamed namespace.)</span></span>

<span data-ttu-id="889d6-243">In diesem Beispiel die Klasse `B` sehen, dass die Klasse `A` da `B`des Namespace `N1.N2.N3` ist im Prinzip innerhalb des Namespaces geschachtelt `N1.N2`.</span><span class="sxs-lookup"><span data-stu-id="889d6-243">In this example, the class `B` can see the class `A` because `B`'s namespace `N1.N2.N3` is conceptually nested within the namespace `N1.N2`.</span></span>

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

### <a name="namespace-declarations"></a><span data-ttu-id="889d6-244">Namespacedeklarationen</span><span class="sxs-lookup"><span data-stu-id="889d6-244">Namespace Declarations</span></span>

<span data-ttu-id="889d6-245">Es gibt drei Arten des Namespace-Deklaration.</span><span class="sxs-lookup"><span data-stu-id="889d6-245">There are three forms of namespace declaration.</span></span>

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

<span data-ttu-id="889d6-246">Die erste Form beginnt mit dem Schlüsselwort `Namespace` gefolgt von der ein relativer Namespacename.</span><span class="sxs-lookup"><span data-stu-id="889d6-246">The first form starts with the keyword `Namespace` followed by a relative namespace name.</span></span> <span data-ttu-id="889d6-247">Wenn der relativer Namespacename qualifiziert ist, wird die Namespacedeklaration behandelt, als ob sie in den Namespacedeklarationen, die für jeden Namen in den qualifizierten Namen lexikalisch geschachtelt ist.</span><span class="sxs-lookup"><span data-stu-id="889d6-247">If the relative namespace name is qualified, the namespace declaration is treated as if it is lexically nested within namespace declarations corresponding to each name in the qualified name.</span></span> <span data-ttu-id="889d6-248">Beispielsweise sind die folgenden beiden Namespaces semantisch gleichwertig:</span><span class="sxs-lookup"><span data-stu-id="889d6-248">For example, the following two namespaces are semantically equivalent:</span></span>

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

<span data-ttu-id="889d6-249">Die zweite Form beginnt mit den Schlüsselwörtern `Namespace Global`.</span><span class="sxs-lookup"><span data-stu-id="889d6-249">The second form starts with the keywords `Namespace Global`.</span></span> <span data-ttu-id="889d6-250">Es wird behandelt, als ob alle seine Memberdeklarationen im globalen unbenannte Namespace – unabhängig von der alle Standardwerte, die von der kompilierungsumgebung bereitgestellten lexikalisch platziert wurden.</span><span class="sxs-lookup"><span data-stu-id="889d6-250">It is treated as if all its member declarations were lexically placed in the global unnamed namespace -- regardless of any defaults provided by the compilation environment.</span></span> <span data-ttu-id="889d6-251">Diese Form der Namespace-Deklaration kann nicht in jeder anderen Namespacedeklaration lexikalisch geschachtelt werden.</span><span class="sxs-lookup"><span data-stu-id="889d6-251">This form of namespace declaration may not be lexically nested within any other namespace declaration.</span></span>

<span data-ttu-id="889d6-252">Die dritte Form beginnt mit den Schlüsselwörtern `Namespace Global` gefolgt von einem qualifizierten Bezeichner `N`.</span><span class="sxs-lookup"><span data-stu-id="889d6-252">The third form starts with the keywords `Namespace Global` followed by a qualified identifier `N`.</span></span> <span data-ttu-id="889d6-253">Es wird behandelt, als handele es sich um eine Namespace-Deklaration, der das erste Formular "`Namespace N`", die im globalen unbenannte Namespace – unabhängig von der alle Standardwerte, die von der kompilierungsumgebung bereitgestellten lexikalisch platziert wurde.</span><span class="sxs-lookup"><span data-stu-id="889d6-253">It is treated as if it were a namespace declaration of the first form "`Namespace N`" that was lexically placed in the global unnamed namespace -- regardless of any defaults provided by the compilation environment.</span></span> <span data-ttu-id="889d6-254">Diese Form der Namespace-Deklaration kann nicht in jeder anderen Namespacedeklaration lexikalisch geschachtelt werden.</span><span class="sxs-lookup"><span data-stu-id="889d6-254">This form of namespace declaration may not be lexically nested within any other namespace declaration.</span></span>

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

<span data-ttu-id="889d6-255">Beim Umgang mit den Elementen eines Namespace ist es nicht wichtig, in ein bestimmter Member deklariert wird.</span><span class="sxs-lookup"><span data-stu-id="889d6-255">When dealing with the members of a namespace, it is not important where a particular member is declared.</span></span> <span data-ttu-id="889d6-256">Wenn zwei Programme eine Entität mit dem gleichen Namen im selben Namespace definieren, tritt beim Versuch, den Namen im Namespace auflösen, einem Mehrdeutigkeitsfehler.</span><span class="sxs-lookup"><span data-stu-id="889d6-256">If two programs define an entity with the same name in the same namespace, attempting to resolve the name in the namespace causes an ambiguity error.</span></span>

<span data-ttu-id="889d6-257">Namespaces sind per Definition `Public`, sodass eine Namespace-Deklaration keine Zugriffsmodifizierer enthalten kann.</span><span class="sxs-lookup"><span data-stu-id="889d6-257">Namespaces are by definition `Public`, so a namespace declaration cannot include any access modifiers.</span></span>


### <a name="namespace-members"></a><span data-ttu-id="889d6-258">Namespace-Elemente</span><span class="sxs-lookup"><span data-stu-id="889d6-258">Namespace Members</span></span>

<span data-ttu-id="889d6-259">Namespace-Mitglieder können nur Namespacedeklarationen werden und Typdeklarationen dar.</span><span class="sxs-lookup"><span data-stu-id="889d6-259">Namespace members can only be namespace declarations and type declarations.</span></span> <span data-ttu-id="889d6-260">Deklarationen von Typen möglicherweise `Public` oder `Friend` Zugriff.</span><span class="sxs-lookup"><span data-stu-id="889d6-260">Type declarations may have `Public` or `Friend` access.</span></span> <span data-ttu-id="889d6-261">Der Standardzugriff für Typen ist `Friend` Zugriff.</span><span class="sxs-lookup"><span data-stu-id="889d6-261">The default access for types is `Friend` access.</span></span>

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
