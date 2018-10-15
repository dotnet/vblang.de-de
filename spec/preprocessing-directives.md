# <a name="preprocessing-directives"></a><span data-ttu-id="9d704-101">Präprozessoranweisungen</span><span class="sxs-lookup"><span data-stu-id="9d704-101">Preprocessing Directives</span></span>

<span data-ttu-id="9d704-102">Nach eine Datei lexikalisch analysiert wurde, treten auf verschiedene Arten von Quelle vorverarbeitung.</span><span class="sxs-lookup"><span data-stu-id="9d704-102">Once a file has been lexically analyzed, several kinds of source preprocessing occur.</span></span> <span data-ttu-id="9d704-103">Die wichtigste, bedingte Kompilierung bestimmt die Quelle von der syntaktischen Grammatik verarbeitet wird; zwei andere Arten von Anweisungen, die – externe quelldirektiven und Region-Direktiven--Meta-Informationen zur Quelle bereitzustellen, aber haben keine Auswirkungen auf die Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="9d704-103">The most important, conditional compilation, determines which source is processed by the syntactic grammar; two other types of directives -- external source directives and region directives -- provide meta-information about the source but have no effect on compilation.</span></span>

## <a name="conditional-compilation"></a><span data-ttu-id="9d704-104">Bedingte Kompilierung</span><span class="sxs-lookup"><span data-stu-id="9d704-104">Conditional Compilation</span></span>

<span data-ttu-id="9d704-105">Für die bedingte Kompilierung steuert, ob es sich bei Sequenzen logischer Zeilen in der tatsächlichen Code übersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="9d704-105">Conditional compilation controls whether sequences of logical lines are translated into actual code.</span></span> <span data-ttu-id="9d704-106">Am Anfang des für die bedingte Kompilierung, sind alle logische Zeilen aktiviert. Einschließen von Zeilen in bedingten kompilierungsanweisungen kann jedoch selektiv die Zeilen in der Datei, die veranlassen, die im weiteren Verlauf des Kompilierungsprozesses ignoriert werden deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="9d704-106">At the beginning of conditional compilation, all logical lines are enabled; however, enclosing lines in conditional compilation statements may selectively disable those lines within the file, causing them to be ignored during the rest of the compilation process.</span></span>

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


<span data-ttu-id="9d704-107">Beispielsweise wird das Programm</span><span class="sxs-lookup"><span data-stu-id="9d704-107">For example, the program</span></span>

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

<span data-ttu-id="9d704-108">erzeugt genaue dieselbe Sequenz von Token wie das Programm</span><span class="sxs-lookup"><span data-stu-id="9d704-108">produces the exact same sequence of tokens as the program</span></span>

```vb
Class C
    Sub F()
    End Sub

    Sub I()
    End Sub
End Class
```

<span data-ttu-id="9d704-109">Die Konstante Ausdrücke, die in Anweisungen für bedingte Kompilierung zulässig sind eine Teilmenge der allgemeinen Konstante Ausdrücke.</span><span class="sxs-lookup"><span data-stu-id="9d704-109">The constant expressions allowed in conditional compilation directives are a subset of general constant expressions.</span></span>

<span data-ttu-id="9d704-110">Der Präprozessor kann Leerzeichen und explizite zeilenfortsetzungen vor und nach jedem Token.</span><span class="sxs-lookup"><span data-stu-id="9d704-110">The preprocessor allows whitespace and explicit line continuations before and after every token.</span></span>


### <a name="conditional-constant-directives"></a><span data-ttu-id="9d704-111">Bedingte Konstanten-Direktiven</span><span class="sxs-lookup"><span data-stu-id="9d704-111">Conditional Constant Directives</span></span>

<span data-ttu-id="9d704-112">Bedingungsanweisungen Konstante definieren Sie Konstanten, die in einem separaten für die bedingte Kompilierung Deklarationsbereich beschränkt werden, um die Quelldatei vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="9d704-112">Conditional constant statements define constants that exist in a separate conditional compilation declaration space scoped to the source file.</span></span>

```antlr
CCConstantDeclaration
    : '#' 'Const' Identifier '=' CCExpression LineTerminator
    ;
```

<span data-ttu-id="9d704-113">Die Deklarationsabschnitt ist etwas Besonderes, da keine explizite Deklaration von Konstanten für bedingte Kompilierung ist erforderlich – Konstanten für bedingte können implizit in eine Direktive für bedingte Kompilierung definiert werden.</span><span class="sxs-lookup"><span data-stu-id="9d704-113">The declaration space is special in that no explicit declaration of conditional compilation constants is necessary -- conditional constants can be implicitly defined in a conditional compilation directive.</span></span>

<span data-ttu-id="9d704-114">Vor dem wird ein Wert zugewiesen, eine Konstante für bedingte Kompilierung hat den Wert `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="9d704-114">Prior to being assigned a value, a conditional compilation constant has the value `Nothing`.</span></span> <span data-ttu-id="9d704-115">Wenn eine Konstante für bedingte Kompilierung ein Wert zugewiesen, der ein konstanter Ausdruck sein muss ist, wird der Typ der Konstante der Typ des Werts zugewiesen werden.</span><span class="sxs-lookup"><span data-stu-id="9d704-115">When a conditional compilation constant is assigned a value, which must be a constant expression, the type of the constant becomes the type of the value being assigned to it.</span></span> <span data-ttu-id="9d704-116">Eine Konstante für bedingte Kompilierung möglicherweise mehrere Male in einer Quelldatei neu definiert werden.</span><span class="sxs-lookup"><span data-stu-id="9d704-116">A conditional compilation constant may be redefined multiple times throughout a source file.</span></span>

<span data-ttu-id="9d704-117">Der folgende Code gibt beispielsweise nur die Zeichenfolge `about to print value` und den Wert der `Test`.</span><span class="sxs-lookup"><span data-stu-id="9d704-117">For example, the following code prints only the string `about to print value` and the value of `Test`.</span></span>

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

<span data-ttu-id="9d704-118">Die kompilierungsumgebung kann auch Konstanten für bedingte in einem Deklarationsabschnitt für die bedingte Kompilierung definieren.</span><span class="sxs-lookup"><span data-stu-id="9d704-118">The compilation environment may also define conditional constants in a conditional compilation declaration space.</span></span>


### <a name="conditional-compilation-directives"></a><span data-ttu-id="9d704-119">Anweisungen für die bedingte Kompilierung</span><span class="sxs-lookup"><span data-stu-id="9d704-119">Conditional Compilation Directives</span></span>

<span data-ttu-id="9d704-120">Anweisungen für bedingte Kompilierung steuern, für die bedingte Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="9d704-120">Conditional compilation directives control conditional compilation.</span></span>

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

<span data-ttu-id="9d704-121">Konstanten für bedingte können nur Konstante Ausdrücke und bedingte Kompilierungskonstanten verweisen.</span><span class="sxs-lookup"><span data-stu-id="9d704-121">Conditional constants can only reference constant expressions and conditional compilation constants.</span></span> <span data-ttu-id="9d704-122">Jede der Konstante Ausdrücke in einer einzelnen für die bedingte Kompilierung Gruppe ausgewertet und konvertiert die `Boolean` Typ in der Reihenfolge vom ersten bis zum letzten, bis eine des bedingten Ausdrücke im Text ergibt `True`.</span><span class="sxs-lookup"><span data-stu-id="9d704-122">Each of the constant expressions within a single conditional compilation group is evaluated and converted to the `Boolean` type in textual order from first to last until one of the conditional expressions evaluates to `True`.</span></span> <span data-ttu-id="9d704-123">Wenn ein Ausdruck nicht konvertierbar ist `Boolean`, einem Fehler während der Kompilierung führt.</span><span class="sxs-lookup"><span data-stu-id="9d704-123">If an expression is not convertible to `Boolean`, a compile-time error results.</span></span> <span data-ttu-id="9d704-124">Semantik und binäre Zeichenfolgenvergleiche werden immer verwendet, bei der Auswertung des konstanten Ausdrücken für bedingte Kompilierung, unabhängig davon, ob `Option` Direktiven oder Einstellungen für die Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="9d704-124">Permissive semantics and binary string comparisons are always used when evaluating conditional compilation constant expressions, regardless of any `Option` directives or compilation environment settings.</span></span>

<span data-ttu-id="9d704-125">Alle Zeilen in die Gruppe, einschließlich geschachtelter Bedingte Kompilierungsdirektiven eingeschlossen sind deaktiviert, mit Ausnahme der Zeilen zwischen die Anweisung mit der `True` Ausdruck und der nächsten bedingten Anweisung der Gruppe oder Linien zwischen den `Else`Anweisung und die `End If` Anweisung Wenn ein `Else` wird in der Gruppe und alle Ausdrücke ausgewertet `False`.</span><span class="sxs-lookup"><span data-stu-id="9d704-125">All lines enclosed by the group, including nested conditional compilation directives, are disabled except for lines between the statement containing the `True` expression and the next conditional statement of the group, or lines between the `Else` statement and the `End If` statement if an `Else` appears in the group and all of the expressions evaluate to `False`.</span></span>

<span data-ttu-id="9d704-126">In diesem Beispiel ist der Aufruf von `WriteToLog` in die `Trace` Direktive für bedingte Kompilierung wird nicht verarbeitet werden, da die umgebenden `Debug` bedingte Kompilierungsdirektive ergibt `False`.</span><span class="sxs-lookup"><span data-stu-id="9d704-126">In this example, the call to `WriteToLog` in the `Trace` conditional compilation directive is not processed because the surrounding `Debug` conditional compilation directive evaluates to `False`.</span></span>

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


## <a name="external-source-directives"></a><span data-ttu-id="9d704-127">Externe Quelldirektiven</span><span class="sxs-lookup"><span data-stu-id="9d704-127">External Source Directives</span></span>

<span data-ttu-id="9d704-128">Eine Quelldatei kann es sich um externe quelldirektiven enthalten, die eine Zuordnung zwischen Quellzeilen und Text für die Quelle extern angeben.</span><span class="sxs-lookup"><span data-stu-id="9d704-128">A source file may include external source directives that indicate a mapping between source lines and text external to the source.</span></span>

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

<span data-ttu-id="9d704-129">Externe quelldirektiven haben keine Auswirkungen auf die Kompilierung und können nicht geschachtelt werden.</span><span class="sxs-lookup"><span data-stu-id="9d704-129">External source directives have no effect on compilation and may not be nested.</span></span> <span data-ttu-id="9d704-130">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="9d704-130">For example:</span></span>

```vb
Module Test
    Sub Main()

#ExternalSource("c:\wwwroot\inetpub\test.aspx", 30)
        Console.WriteLine("In test.aspx")
#End ExternalSource

    End Sub
End Module
```


## <a name="region-directives"></a><span data-ttu-id="9d704-131">Region-Direktiven</span><span class="sxs-lookup"><span data-stu-id="9d704-131">Region Directives</span></span>

<span data-ttu-id="9d704-132">Bereichsdirektiven Gruppe von Zeilen Quellcode, haben keine anderen Auswirkungen auf die Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="9d704-132">Region directives group lines of source code but have no other effect on compilation.</span></span> <span data-ttu-id="9d704-133">Die gesamte Gruppe kann, in der integrierten Entwicklungsumgebung (IDE) reduziert und ausgeblendet oder erweitert und angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="9d704-133">The entire group can be collapsed and hidden, or expanded and viewed, in the integrated development environment (IDE).</span></span>

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

<span data-ttu-id="9d704-134">Regionen können geschachtelt werden.</span><span class="sxs-lookup"><span data-stu-id="9d704-134">Regions may be nested.</span></span> <span data-ttu-id="9d704-135">Region-Direktiven sind speziell, können sie weder gestartet noch innerhalb eines Methodentexts beenden und sie müssen die Blockstruktur des Programms berücksichtigen.</span><span class="sxs-lookup"><span data-stu-id="9d704-135">Region directives are special in that they can neither start nor terminate within a method body, and they must respect the block structure of the program.</span></span> <span data-ttu-id="9d704-136">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="9d704-136">For example:</span></span>

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


## <a name="external-checksum-directives"></a><span data-ttu-id="9d704-137">External Checksum-Direktiven</span><span class="sxs-lookup"><span data-stu-id="9d704-137">External Checksum Directives</span></span>

<span data-ttu-id="9d704-138">Eine Quelldatei kann eine externe Checksum-Direktive einschließen, die angibt, welche Prüfsumme für eine Datei, die in einer externen Quelle-Direktive verwiesen ausgegeben werden soll.</span><span class="sxs-lookup"><span data-stu-id="9d704-138">A source file may include an external checksum directive that indicates what checksum should be emitted for a file referenced in an external source directive.</span></span> <span data-ttu-id="9d704-139">In jeder anderen Hinsicht haben externe quelldirektiven keine Auswirkungen auf die Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="9d704-139">In all other respects external source directives have no effect on compilation.</span></span>

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

<span data-ttu-id="9d704-140">Eine externe Checksum-Direktive enthält, den Dateinamen der externen Datei, eine globally unique Identifier (GUID), die die Datei und die Prüfsumme für die Datei zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="9d704-140">An external checksum directive contains the filename of the external file, a globally unique identifier (GUID) associated with the file and the checksum for the file.</span></span> <span data-ttu-id="9d704-141">Die GUID ist als eine Zeichenfolgenkonstante im Format "{Xxxxxxxx-Xxxx-Xxxx-Xxxx-Xxxxxxxxxxxx}", angegeben, wobei x eine hexadezimale Ziffer ist.</span><span class="sxs-lookup"><span data-stu-id="9d704-141">The GUID is specified as a string constant of the form "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}", where x is a hexadecimal digit.</span></span> <span data-ttu-id="9d704-142">Die Prüfsumme wird als eine Zeichenfolgenkonstante im Format "Xxxx...", angegeben, wobei x eine hexadezimale Ziffer ist.</span><span class="sxs-lookup"><span data-stu-id="9d704-142">The checksum is specified as a string constant of the form "xxxx...", where x is a hexadecimal digit.</span></span> <span data-ttu-id="9d704-143">Die Anzahl der Ziffern in einer Prüfsumme muss eine gerade Zahl sein.</span><span class="sxs-lookup"><span data-stu-id="9d704-143">The number of digits in a checksum must be an even number.</span></span>

<span data-ttu-id="9d704-144">Eine externe Datei möglicherweise mehrere externe Checksum-Anweisungen, die mit ihm verknüpft ist, vorausgesetzt, dass alle Werte GUID und der Prüfsumme genau übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="9d704-144">An external file may have multiple external checksum directives associated with it provided that all of the GUID and checksum values match exactly.</span></span> <span data-ttu-id="9d704-145">Wenn der Name der externen Datei den Namen einer Datei, die kompiliert wird übereinstimmt, wird die Prüfsumme zugunsten des Compilers prüfsummenberechnung ignoriert.</span><span class="sxs-lookup"><span data-stu-id="9d704-145">If the name of the external file matches the name of a file being compiled, the checksum is ignored in favor of the compiler's checksum calculation.</span></span>

<span data-ttu-id="9d704-146">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="9d704-146">For example:</span></span>

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

