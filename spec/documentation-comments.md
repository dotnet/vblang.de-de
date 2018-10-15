# <a name="documentation-comments"></a><span data-ttu-id="65ff3-101">Kommentare zur Dokumentation</span><span class="sxs-lookup"><span data-stu-id="65ff3-101">Documentation Comments</span></span>

<span data-ttu-id="65ff3-102">Dokumentationskommentare sind speziell formatierten Kommentaren in der Quelle, die analysiert werden können, um die Dokumentation über den Code zu erzeugen, die sie verbunden sind.</span><span class="sxs-lookup"><span data-stu-id="65ff3-102">Documentation comments are specially formatted comments in the source that can be analyzed to produce documentation about the code they are attached to.</span></span> <span data-ttu-id="65ff3-103">Das grundlegende Format für Dokumentationskommentare ist XML.</span><span class="sxs-lookup"><span data-stu-id="65ff3-103">The basic format for documentation comments is XML.</span></span> <span data-ttu-id="65ff3-104">Wenn kann der kompilierte Code mit Dokumentationskommentaren, der Compiler optional auch eine XML-Datei ausgeben, die die Gesamtsumme der die Dokumentationskommentare in der Quelle darstellt.</span><span class="sxs-lookup"><span data-stu-id="65ff3-104">When the compiling code with documentation comments, the compiler may optionally emit an XML file that represents the sum total of the documentation comments in the source.</span></span> <span data-ttu-id="65ff3-105">Diese XML-Datei kann dann von anderen Tools verwendet werden, um Druck oder online-Dokumentation zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="65ff3-105">This XML file can then be used by other tools to produce printed or online documentation.</span></span>

<span data-ttu-id="65ff3-106">In diesem Kapitel wird beschrieben, Kommentare aus Dokumenten und empfohlene XML-Tags für die Verwendung mit Kommentare aus Dokumenten.</span><span class="sxs-lookup"><span data-stu-id="65ff3-106">This chapter describes document comments and recommended XML tags to use with document comments.</span></span>

## <a name="documentation-comment-format"></a><span data-ttu-id="65ff3-107">Dokumentationskommentare im Standardformat</span><span class="sxs-lookup"><span data-stu-id="65ff3-107">Documentation Comment Format</span></span>

<span data-ttu-id="65ff3-108">Kommentare aus Dokumenten sind besondere Kommentare, die mit beginnen `'''`, drei einfache Anführungszeichen ein.</span><span class="sxs-lookup"><span data-stu-id="65ff3-108">Document comments are special comments that begin with `'''`, three single quote marks.</span></span> <span data-ttu-id="65ff3-109">Sie müssen unmittelbar voranstehen, den Typ (z. B. eine Klasse, Delegat oder Schnittstelle) oder den Typmember (z. B. ein Feld, Ereignis, Eigenschaft oder Methode), den sie dokumentieren.</span><span class="sxs-lookup"><span data-stu-id="65ff3-109">They must immediately precede the type (such as a class, delegate, or interface) or type member (such as a field, event, property, or method) that they document.</span></span> <span data-ttu-id="65ff3-110">Ein Dokumentkommentar für eine partielle Methodendeklaration wird durch den Dokumentkommentar für die Methode, die Text enthält, liefert ersetzt werden, sofern vorhanden.</span><span class="sxs-lookup"><span data-stu-id="65ff3-110">A document comment on a partial method declaration will be replaced by the document comment on the method that supplies its body, if there is one.</span></span> <span data-ttu-id="65ff3-111">Alle angrenzenden Dokumentkommentare werden zusammen angefügt, um ein einzelnes Dokumentkommentar zu erzeugen.</span><span class="sxs-lookup"><span data-stu-id="65ff3-111">All adjacent document comments are appended together to produce a single document comment.</span></span> <span data-ttu-id="65ff3-112">Es ist ein Zeichen, Leerzeichen folgt die `'''` -Zeichen, und klicken Sie dann diese Leerzeichen nicht in die Verkettung enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="65ff3-112">If there is a whitespace character following the `'''` characters, then that whitespace character is not included in the concatenation.</span></span> <span data-ttu-id="65ff3-113">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="65ff3-113">For example:</span></span>

```vb
''' <remarks>
''' Class <c>Point</c> models a point in a two-dimensional plane.
''' </remarks>
Public Class Point 
   ''' <remarks>
   ''' Method <c>Draw</c> renders the point.
   ''' </remarks>
   Sub Draw()
   End Sub
End Class
```

<span data-ttu-id="65ff3-114">Dokumentationskommentare muss werden wohlgeformtes XML gemäß http://www.w3.org/TR/REC-xml.</span><span class="sxs-lookup"><span data-stu-id="65ff3-114">Documentation comments must be well formed XML according to http://www.w3.org/TR/REC-xml.</span></span> <span data-ttu-id="65ff3-115">Wenn der XML-Code nicht wohlgeformt ist, wird eine Warnung generiert, und die Dokumentationsdatei enthält einen Kommentar, der besagt, dass ein Fehler aufgetreten ist.</span><span class="sxs-lookup"><span data-stu-id="65ff3-115">If the XML is not well formed, a warning is generated and the documentation file will contain a comment saying that an error was encountered.</span></span>

<span data-ttu-id="65ff3-116">Obwohl Entwickler können ihren eigenen Satz von Tags zu erstellen sind, wird eine empfohlene Sammlung im nächsten Abschnitt definiert.</span><span class="sxs-lookup"><span data-stu-id="65ff3-116">Although developers are free to create their own set of tags, a recommended set is defined in the next section.</span></span> <span data-ttu-id="65ff3-117">Einige der empfohlenen Tags haben eine besondere Bedeutung:</span><span class="sxs-lookup"><span data-stu-id="65ff3-117">Some of the recommended tags have special meanings:</span></span>

* <span data-ttu-id="65ff3-118">Die `<param>` Tag wird verwendet, um Parameter zu beschreiben.</span><span class="sxs-lookup"><span data-stu-id="65ff3-118">The `<param>` tag is used to describe parameters.</span></span> <span data-ttu-id="65ff3-119">Der Parameter, die gemäß einer `<param>` Tag muss vorhanden sein und alle Parameter des Typmembers müssen in der Dokumentationskommentar beschrieben.</span><span class="sxs-lookup"><span data-stu-id="65ff3-119">The parameter specified by a `<param>` tag must exist and all parameters of the type member must be described in the documentation comment.</span></span> <span data-ttu-id="65ff3-120">Wenn eine der Bedingungen nicht zutrifft, gibt der Compiler eine Warnung aus.</span><span class="sxs-lookup"><span data-stu-id="65ff3-120">If either condition is not true, the compiler issues a warning.</span></span>

* <span data-ttu-id="65ff3-121">Das `cref`-Attribut kann an jedes Tag angefügt werden, um einen Verweis auf ein Codeelement bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="65ff3-121">The `cref` attribute can be attached to any tag to provide a reference to a code element.</span></span> <span data-ttu-id="65ff3-122">Das Code-Element muss vorhanden sein; zum Zeitpunkt der Kompilierung ersetzt der Compiler den Namen, mit der ID-Zeichenfolge, die den Member darstellt.</span><span class="sxs-lookup"><span data-stu-id="65ff3-122">The code element must exist; at compile-time the compiler replaces the name with the ID string representing the member.</span></span> <span data-ttu-id="65ff3-123">Wenn der Code-Element nicht vorhanden ist, gibt der Compiler eine Warnung aus.</span><span class="sxs-lookup"><span data-stu-id="65ff3-123">If the code element does not exist, the compiler issues a warning.</span></span> <span data-ttu-id="65ff3-124">Bei der Suche für ein Namen im beschrieben eine `cref` Attribut, das Compiler-Hinsicht `Imports` Anweisungen, die innerhalb der enthaltenden Quelldatei angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="65ff3-124">When looking for a name described in a `cref` attribute, the compiler respects `Imports` statements that appear within the containing source file.</span></span>

* <span data-ttu-id="65ff3-125">Die `<summary>` richtet sich an Tag durch eine Dokumentations-Viewer verwendet werden, um weitere Informationen über einen Typ oder Member anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="65ff3-125">The `<summary>` tag is intended to be used by a documentation viewer to display additional information about a type or member.</span></span>

<span data-ttu-id="65ff3-126">Beachten Sie, dass die Dokumentationsdatei keine vollständigen Informationen über einen Typ und Member, ausschließlich in die Kommentare aus Dokumenten enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="65ff3-126">Note that the documentation file does not provide full information about a type and members, only what is contained in the document comments.</span></span> <span data-ttu-id="65ff3-127">Weitere Informationen über einen Typ oder Member zu erhalten, muss die Dokumentationsdatei zusammen mit Reflektion für den tatsächlichen Typ oder Member verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="65ff3-127">To get more information about a type or member, the documentation file must be used in conjunction with reflection on the actual type or member.</span></span>

## <a name="recommended-tags"></a><span data-ttu-id="65ff3-128">Empfohlene tags</span><span class="sxs-lookup"><span data-stu-id="65ff3-128">Recommended tags</span></span>

<span data-ttu-id="65ff3-129">Der Dokumentations-Generator muss akzeptiert und verarbeitet alle Tags, die gemäß den Regeln der XML gültig ist.</span><span class="sxs-lookup"><span data-stu-id="65ff3-129">The documentation generator must accept and process any tag that is valid according to the rules of XML.</span></span> <span data-ttu-id="65ff3-130">Die folgenden Tags stellen häufig verwendete Funktionen in der Benutzerdokumentation bereit:</span><span class="sxs-lookup"><span data-stu-id="65ff3-130">The following tags provide commonly used functionality in user documentation:</span></span>

<span data-ttu-id="65ff3-131">`<c>` Legt Text in einer Code-ähnliche Schriftart fest.</span><span class="sxs-lookup"><span data-stu-id="65ff3-131">`<c>` Sets text in a code-like font</span></span>

<span data-ttu-id="65ff3-132">`<code>` Legt eine oder mehrere Zeilen von Code oder Programmausgabe Quellausgabe in einer Code-ähnliche Schriftart</span><span class="sxs-lookup"><span data-stu-id="65ff3-132">`<code>` Sets one or more lines of source code or program output in a code-like font</span></span>

<span data-ttu-id="65ff3-133">`<example>` Gibt an, Beispiel</span><span class="sxs-lookup"><span data-stu-id="65ff3-133">`<example>` Indicates an example</span></span>

<span data-ttu-id="65ff3-134">`<exception>` Identifiziert die Ausnahmen, die eine Methode ausgelöst werden können</span><span class="sxs-lookup"><span data-stu-id="65ff3-134">`<exception>` Identifies the exceptions a method can throw</span></span>

<span data-ttu-id="65ff3-135">`<include>` Enthält ein externes XML-Dokument</span><span class="sxs-lookup"><span data-stu-id="65ff3-135">`<include>` Includes an external XML document</span></span>

<span data-ttu-id="65ff3-136">`<list>` Erstellt eine Liste oder Tabelle</span><span class="sxs-lookup"><span data-stu-id="65ff3-136">`<list>` Creates a list or table</span></span>

<span data-ttu-id="65ff3-137">`<para>` Ermöglicht der Struktur, die Text hinzugefügt werden</span><span class="sxs-lookup"><span data-stu-id="65ff3-137">`<para>` Permits structure to be added to text</span></span>

<span data-ttu-id="65ff3-138">`<param>` Beschreibt einen Parameter für eine Methode oder Konstruktor</span><span class="sxs-lookup"><span data-stu-id="65ff3-138">`<param>` Describes a parameter for a method or constructor</span></span>

<span data-ttu-id="65ff3-139">`<paramref>` Gibt an, dass ein Wort ein Parametername ist.</span><span class="sxs-lookup"><span data-stu-id="65ff3-139">`<paramref>` Identifies that a word is a parameter name</span></span>

<span data-ttu-id="65ff3-140">`<permission>` Dokumentiert die Security Zugriff auf ein Element</span><span class="sxs-lookup"><span data-stu-id="65ff3-140">`<permission>` Documents the security accessibility of a member</span></span>

<span data-ttu-id="65ff3-141">`<remarks>` Beschreibt einen Typ</span><span class="sxs-lookup"><span data-stu-id="65ff3-141">`<remarks>` Describes a type</span></span>

<span data-ttu-id="65ff3-142">`<returns>` Beschreibt den Rückgabewert einer Methode</span><span class="sxs-lookup"><span data-stu-id="65ff3-142">`<returns>` Describes the return value of a method</span></span>

<span data-ttu-id="65ff3-143">`<see>` Gibt an, einen link</span><span class="sxs-lookup"><span data-stu-id="65ff3-143">`<see>` Specifies a link</span></span>

<span data-ttu-id="65ff3-144">`<seealso>` Generiert einen Eintrag auch finden Sie unter</span><span class="sxs-lookup"><span data-stu-id="65ff3-144">`<seealso>` Generates a See Also entry</span></span>

<span data-ttu-id="65ff3-145">`<summary>` Beschreibt einen Member eines Typs</span><span class="sxs-lookup"><span data-stu-id="65ff3-145">`<summary>` Describes a member of a type</span></span>

<span data-ttu-id="65ff3-146">`<typeparam>` Beschreibt einen Typparameter</span><span class="sxs-lookup"><span data-stu-id="65ff3-146">`<typeparam>` Describes a type parameter</span></span>

<span data-ttu-id="65ff3-147">`<value>` Beschreibt eine Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="65ff3-147">`<value>` Describes a property</span></span>

### <a name="ltcgt"></a><span data-ttu-id="65ff3-148">&lt;c&gt;</span><span class="sxs-lookup"><span data-stu-id="65ff3-148">&lt;c&gt;</span></span>

<span data-ttu-id="65ff3-149">Dieses Tag gibt an, dass ein Fragment von Text in eine Beschreibung eine Schriftart, die für einen Codeblock verwendet verwenden soll.</span><span class="sxs-lookup"><span data-stu-id="65ff3-149">This tag specifies that a fragment of text within a description should use a font like that used for a block of code.</span></span> <span data-ttu-id="65ff3-150">(Verwenden Sie für die Feldlinien des tatsächlichen Code, `<code>`.)</span><span class="sxs-lookup"><span data-stu-id="65ff3-150">(For lines of actual code, use `<code>`.)</span></span>

<span data-ttu-id="65ff3-151">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-151">__Syntax:__</span></span>

```xml
<c>text to be set like code</c>
```

<span data-ttu-id="65ff3-152">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-152">__Example:__</span></span>

```vb
''' <remarks>
''' Class <c>Point</c> models a point in a two-dimensional plane.
''' </remarks>
Public Class Point 
End Class
```

### <a name="ltcodegt"></a><span data-ttu-id="65ff3-153">&lt;Code&gt;</span><span class="sxs-lookup"><span data-stu-id="65ff3-153">&lt;code&gt;</span></span>

<span data-ttu-id="65ff3-154">Dieses Tag gibt an, dass eine oder mehrere Zeilen von Code oder Programmausgabe Quellausgabe eine Schriftart mit fester Zeichenbreite verwenden soll.</span><span class="sxs-lookup"><span data-stu-id="65ff3-154">This tag specifies that one or more lines of source code or program output should use a fixed-width font.</span></span> <span data-ttu-id="65ff3-155">(Verwenden Sie für kleine Codefragmente, `<c>`.)</span><span class="sxs-lookup"><span data-stu-id="65ff3-155">(For small code fragments, use `<c>`.)</span></span>

<span data-ttu-id="65ff3-156">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-156">__Syntax:__</span></span>

```xml
<code>source code or program output</code>
```

<span data-ttu-id="65ff3-157">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-157">__Example:__</span></span>

```vb
''' <summary>
''' This method changes the point's location by the given x- and 
''' y-offsets.
''' <example>
''' For example:
''' <code>
'''    Dim p As Point = New Point(3,5)
'''    p.Translate(-1,3)
''' </code>
''' results in <c>p</c>'s having the value (2,8).
''' </example>
''' </summary>
Public Sub Translate(x As Integer, y As Integer)
    Me.x += x
    Me.y += y
End Sub
```

### <a name="ltexamplegt"></a><span data-ttu-id="65ff3-158">&lt;example&gt;</span><span class="sxs-lookup"><span data-stu-id="65ff3-158">&lt;example&gt;</span></span>

<span data-ttu-id="65ff3-159">Dieses Tag ermöglicht Beispielcode in einem Kommentar angezeigt, wie ein Element verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="65ff3-159">This tag allows example code within a comment to show how an element can be used.</span></span> <span data-ttu-id="65ff3-160">Normalerweise umfassen diese immer mit dem Tag `<code>` ebenfalls.</span><span class="sxs-lookup"><span data-stu-id="65ff3-160">Ordinarily, this will involve use of the tag `<code>` as well.</span></span>

<span data-ttu-id="65ff3-161">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-161">__Syntax:__</span></span>

```xml
<example>description</example>
```

<span data-ttu-id="65ff3-162">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-162">__Example:__</span></span>

<span data-ttu-id="65ff3-163">Ein Beispiel finden Sie unter `<code>`.</span><span class="sxs-lookup"><span data-stu-id="65ff3-163">See `<code>` for an example.</span></span>

### <a name="ltexceptiongt"></a><span data-ttu-id="65ff3-164">&lt;exception&gt;</span><span class="sxs-lookup"><span data-stu-id="65ff3-164">&lt;exception&gt;</span></span>

<span data-ttu-id="65ff3-165">Dieses Tag bietet eine Möglichkeit, die Ausnahmen zu dokumentieren, die eine Methode ausgelöst werden kann.</span><span class="sxs-lookup"><span data-stu-id="65ff3-165">This tag provides a way to document the exceptions a method can throw.</span></span>

<span data-ttu-id="65ff3-166">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-166">__Syntax:__</span></span>

```xml
<exception cref="member">description</exception>
```

<span data-ttu-id="65ff3-167">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-167">__Example:__</span></span>

```vb
Public Module DataBaseOperations
    ''' <exception cref="MasterFileFormatCorruptException"></exception>
    ''' <exception cref="MasterFileLockedOpenException"></exception>
    Public Sub ReadRecord(flag As Integer)
        If Flag = 1 Then
            Throw New MasterFileFormatCorruptException()
        ElseIf Flag = 2 Then
            Throw New MasterFileLockedOpenException()
        End If
        ' ...
    End Sub
End Module
```

### <a name="ltincludegt"></a><span data-ttu-id="65ff3-168">&lt;include&gt;</span><span class="sxs-lookup"><span data-stu-id="65ff3-168">&lt;include&gt;</span></span>

<span data-ttu-id="65ff3-169">Dieses Tag wird verwendet, um Informationen aus einer externen wohlgeformtes XML-Dokument enthalten.</span><span class="sxs-lookup"><span data-stu-id="65ff3-169">This tag is used to include information from an external well-formed XML document.</span></span> <span data-ttu-id="65ff3-170">Ein XPath-Ausdruck wird angewendet, im XML-Dokument, um anzugeben, welche XML aus dem Dokument enthalten sein soll.</span><span class="sxs-lookup"><span data-stu-id="65ff3-170">An XPath expression is applied to the XML document to specify what XML should be included from the document.</span></span> <span data-ttu-id="65ff3-171">Die `<include>` Tag wird dann mit den ausgewählten XML-Code aus dem externen Dokument ersetzt.</span><span class="sxs-lookup"><span data-stu-id="65ff3-171">The `<include>` tag is then replaced with the selected XML from the external document.</span></span>

<span data-ttu-id="65ff3-172">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-172">__Syntax:__</span></span>

```xml
<include file="filename" path="xpath">
```

<span data-ttu-id="65ff3-173">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-173">__Example:__</span></span>

<span data-ttu-id="65ff3-174">Wenn der Quellcode eine Deklaration wie der folgenden enthalten:</span><span class="sxs-lookup"><span data-stu-id="65ff3-174">If the source code contained a declaration like the following:</span></span>

```vb
''' <include file="docs.xml" path="extra/class[@name="IntList"]/*" />
```

<span data-ttu-id="65ff3-175">und der externen Datei docs.xml hatte den folgenden Inhalt</span><span class="sxs-lookup"><span data-stu-id="65ff3-175">and the external file docs.xml had the following contents</span></span>

```xml
<?xml version="1.0"?>
<extra>
    <class name="IntList">
        <summary>
            Contains a list of integers.
        </summary>
    </class>
    <class name="StringList">
        <summary>
            Contains a list of strings.
        </summary>
    </class>
</extra>
```

<span data-ttu-id="65ff3-176">Klicken Sie dann ist die gleiche Dokumentation Ausgabe aus, als ob der Quellcode enthalten:</span><span class="sxs-lookup"><span data-stu-id="65ff3-176">then the same documentation is output as if the source code contained:</span></span>

```xml
''' <summary>
''' Contains a list of integers.
''' </summary>
```

### <a name="ltlistgt"></a><span data-ttu-id="65ff3-177">&lt;list&gt;</span><span class="sxs-lookup"><span data-stu-id="65ff3-177">&lt;list&gt;</span></span>

<span data-ttu-id="65ff3-178">Dieses Tag wird verwendet, um eine Liste oder Tabelle der Elemente zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="65ff3-178">This tag is used to create a list or table of items.</span></span> <span data-ttu-id="65ff3-179">Sie enthält eventuell eine `<listheader>` Block, um die Überschriftenzeile einer Tabelle oder einer Definitionsliste zu definieren.</span><span class="sxs-lookup"><span data-stu-id="65ff3-179">It may contain a `<listheader>` block to define the heading row of either a table or definition list.</span></span> <span data-ttu-id="65ff3-180">(Wenn Sie eine Tabelle zu definieren, muss nur ein Eintrag für den Ausdruck in der Überschrift "" angegeben werden.)</span><span class="sxs-lookup"><span data-stu-id="65ff3-180">(When defining a table, only an entry for term in the heading need be supplied.)</span></span>

<span data-ttu-id="65ff3-181">Jedes Element in der Liste wird angegeben, mit einem `<item>` Block.</span><span class="sxs-lookup"><span data-stu-id="65ff3-181">Each item in the list is specified with an `<item>` block.</span></span> <span data-ttu-id="65ff3-182">Wenn Sie eine Definitionsliste zu erstellen, müssen sowohl Begriff und eine Beschreibung angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="65ff3-182">When creating a definition list, both term and description must be specified.</span></span> <span data-ttu-id="65ff3-183">Allerdings muss für eine Tabelle, Liste mit Aufzählungszeichen oder nummerierte Liste, nur die Beschreibung angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="65ff3-183">However, for a table, bulleted list, or numbered list, only description need be specified.</span></span>

<span data-ttu-id="65ff3-184">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-184">__Syntax:__</span></span>

```xml
<list type="bullet" | "number" | "table">
    <listheader>
        <term>term</term>
        <description>description</description>
    </listheader>
    <item>
        <term>term</term>
        <description>description</description>
    </item>
    ...
    <item>
        <term>term</term>
        <description>description</description>
    </item>
</list>
```

<span data-ttu-id="65ff3-185">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-185">__Example:__</span></span>

```vb
Public Class TestClass
    ''' <remarks>
    ''' Here is an example of a bulleted list:
    ''' <list type="bullet">
    '''     <item>
    '''        <description>Item 1.</description>
    '''     </item>
    '''     <item>
    '''         <description>Item 2.</description>
    '''     </item>
    ''' </list>
    ''' </remarks>
    Public Shared Sub Main()
    End Sub
End Class
```

### <a name="ltparagt"></a><span data-ttu-id="65ff3-186">&lt;para&gt;</span><span class="sxs-lookup"><span data-stu-id="65ff3-186">&lt;para&gt;</span></span>

<span data-ttu-id="65ff3-187">Dieses Tag ist für die Verwendung in anderen Tags, wie z. B. `<remarks>` oder `<returns>`, und lässt die Struktur, die Text hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="65ff3-187">This tag is for use inside other tags, such as `<remarks>` or `<returns>`, and permits structure to be added to text.</span></span>

<span data-ttu-id="65ff3-188">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-188">__Syntax:__</span></span>

```xml
<para>content</para>
```

<span data-ttu-id="65ff3-189">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-189">__Example:__</span></span>

```vb
''' <summary>
''' This is the entry point of the Point class testing program.
''' <para>This program tests each method and operator, and
''' is intended to be run after any non-trvial maintenance has
''' been performed on the Point class.</para>
''' </summary>
Public Shared Sub Main()
End Sub
```

### <a name="ltparamgt"></a><span data-ttu-id="65ff3-190">&lt;param&gt;</span><span class="sxs-lookup"><span data-stu-id="65ff3-190">&lt;param&gt;</span></span>

<span data-ttu-id="65ff3-191">Dieses Tag beschreibt einen Parameter für eine Methode, den Konstruktor oder die indizierte Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="65ff3-191">This tag describes a parameter for a method, constructor, or indexed property.</span></span>

<span data-ttu-id="65ff3-192">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-192">__Syntax:__</span></span>

```xml
<param name="name">description</param>
```

<span data-ttu-id="65ff3-193">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-193">__Example:__</span></span>

```vb
''' <summary>
''' This method changes the point's location to the given
''' coordinates.
''' </summary>
''' <param name="x"><c>x</c> is the new x-coordinate.</param>
''' <param name="y"><c>y</c> is the new y-coordinate.</param>
Public Sub Move(x As Integer, y As Integer)
    Me.x = x
    Me.y = y
End Sub
```

### <a name="ltparamrefgt"></a><span data-ttu-id="65ff3-194">&lt;paramref&gt;</span><span class="sxs-lookup"><span data-stu-id="65ff3-194">&lt;paramref&gt;</span></span>

<span data-ttu-id="65ff3-195">Dieses Tag gibt an, dass ein Wort ein Parameter ist.</span><span class="sxs-lookup"><span data-stu-id="65ff3-195">This tag indicates that a word is a parameter.</span></span> <span data-ttu-id="65ff3-196">Die Dokumentationsdatei kann verarbeitet werden, um diesen Parameter auf unterschiedliche Weise zu formatieren.</span><span class="sxs-lookup"><span data-stu-id="65ff3-196">The documentation file can be processed to format this parameter in some distinct way.</span></span>

<span data-ttu-id="65ff3-197">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-197">__Syntax:__</span></span>

```xml
<paramref name="name"/>
```

<span data-ttu-id="65ff3-198">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-198">__Example:__</span></span>

```vb
''' <summary>
''' This constructor initializes the new Point to
''' (<paramref name="x"/>,<paramref name="y"/>).
''' </summary>
''' <param name="x"><c>x</c> is the new Point's x-coordinate.</param>
''' <param name="y"><c>y</c> is the new Point's y-coordinate.</param>
Public Sub New(x As Integer, y As Integer)
    Me.x = x
    Me.y = y
End Sub
```

### <a name="ltpermissiongt"></a><span data-ttu-id="65ff3-199">&lt;permission&gt;</span><span class="sxs-lookup"><span data-stu-id="65ff3-199">&lt;permission&gt;</span></span>

<span data-ttu-id="65ff3-200">Dieses Tag dokumentiert die Security Zugriff auf ein Element</span><span class="sxs-lookup"><span data-stu-id="65ff3-200">This tag documents the security accessibility of a member</span></span>

<span data-ttu-id="65ff3-201">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-201">__Syntax:__</span></span>

```xml
<permission cref="member">description</permission>
```

<span data-ttu-id="65ff3-202">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-202">__Example:__</span></span>

```vb
''' <permission cref="System.Security.PermissionSet">Everyone can
''' access this method.</permission>
Public Shared Sub Test()
End Sub
```

### <a name="ltremarksgt"></a><span data-ttu-id="65ff3-203">&lt;remarks&gt;</span><span class="sxs-lookup"><span data-stu-id="65ff3-203">&lt;remarks&gt;</span></span>

<span data-ttu-id="65ff3-204">Dieses Tag Gibt allgemeine Informationen über einen Typ an.</span><span class="sxs-lookup"><span data-stu-id="65ff3-204">This tag specifies overview information about a type.</span></span> <span data-ttu-id="65ff3-205">(Verwenden `<summary>` um der Member eines Typs beschreiben.)</span><span class="sxs-lookup"><span data-stu-id="65ff3-205">(Use `<summary>` to describe the members of a type.)</span></span>

<span data-ttu-id="65ff3-206">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-206">__Syntax:__</span></span>

```xml
<remarks>description</remarks>
```

<span data-ttu-id="65ff3-207">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-207">__Example:__</span></span>

```vb
''' <remarks>
''' Class <c>Point</c> models a point in a two-dimensional plane.
''' </remarks>
Public Class Point 
End Class
```

### <a name="ltreturnsgt"></a><span data-ttu-id="65ff3-208">&lt;returns&gt;</span><span class="sxs-lookup"><span data-stu-id="65ff3-208">&lt;returns&gt;</span></span>

<span data-ttu-id="65ff3-209">Dieses Tag wird der Rückgabewert einer Methode beschrieben.</span><span class="sxs-lookup"><span data-stu-id="65ff3-209">This tag describes the return value of a method.</span></span>

<span data-ttu-id="65ff3-210">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-210">__Syntax:__</span></span>

```xml
<returns>description</returns>
```

<span data-ttu-id="65ff3-211">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-211">__Example:__</span></span>

```vb
''' <summary>
''' Report a point's location as a string.
''' </summary>
''' <returns>
''' A string representing a point's location, in the form (x,y), without
''' any leading, training, or embedded whitespace.
''' </returns>
Public Overrides Function ToString() As String
    Return "(" & x & "," & y & ")"
End Sub
```

### <a name="ltseegt"></a><span data-ttu-id="65ff3-212">&lt;see&gt;</span><span class="sxs-lookup"><span data-stu-id="65ff3-212">&lt;see&gt;</span></span>

<span data-ttu-id="65ff3-213">Dieses Tag ermöglicht einen Link im Text angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="65ff3-213">This tag allows a link to be specified within text.</span></span> <span data-ttu-id="65ff3-214">(Verwenden `<seealso>` um Text anzugeben, dass in einem Abschnitt Siehe auch angezeigt werden.)</span><span class="sxs-lookup"><span data-stu-id="65ff3-214">(Use `<seealso>` to indicate text that is to appear in a See Also section.)</span></span>

<span data-ttu-id="65ff3-215">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-215">__Syntax:__</span></span>

```xml
<see cref="member"/>
```

<span data-ttu-id="65ff3-216">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-216">__Example:__</span></span>

```vb
''' <summary>
''' This method changes the point's location to the given
''' coordinates.
''' </summary>
''' <see cref="Translate"/>
Public Sub Move(x As Integer, y As Integer)
    Me.x = x
    Me.y = y
End Sub

''' <summary>
''' This method changes the point's location by the given x- and
''' y-offsets.
''' </summary>
''' <see cref="Move"/>
Public Sub Translate(x As Integer, y As Integer)
    Me.x += x
    Me.y += y
End Sub
```

### <a name="ltseealsogt"></a><span data-ttu-id="65ff3-217">&lt;seealso&gt;</span><span class="sxs-lookup"><span data-stu-id="65ff3-217">&lt;seealso&gt;</span></span>

<span data-ttu-id="65ff3-218">Dieses Tag wird einen Eintrag im Abschnitt Siehe auch generiert.</span><span class="sxs-lookup"><span data-stu-id="65ff3-218">This tag generates an entry for the See Also section.</span></span> <span data-ttu-id="65ff3-219">(Verwenden `<see>` auf einen Link im Text angegeben.)</span><span class="sxs-lookup"><span data-stu-id="65ff3-219">(Use `<see>` to specify a link from within text.)</span></span>

<span data-ttu-id="65ff3-220">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-220">__Syntax:__</span></span>

```xml
<seealso cref="member"/>
```

<span data-ttu-id="65ff3-221">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-221">__Example:__</span></span>

```vb
''' <summary>
''' This method determines whether two Points have the same location.
''' </summary>
''' <seealso cref="operator=="/>
''' <seealso cref="operator!="/>
Public Overrides Function Equals(o As Object) As Boolean
    ' ...
End Function
```

### <a name="ltsummarygt"></a><span data-ttu-id="65ff3-222">&lt;summary&gt;</span><span class="sxs-lookup"><span data-stu-id="65ff3-222">&lt;summary&gt;</span></span>

<span data-ttu-id="65ff3-223">Dieses Tag beschreibt einen Typmember an.</span><span class="sxs-lookup"><span data-stu-id="65ff3-223">This tag describes a type member.</span></span> <span data-ttu-id="65ff3-224">(Verwenden `<remarks>` um einen Typ selbst zu beschreiben.)</span><span class="sxs-lookup"><span data-stu-id="65ff3-224">(Use `<remarks>` to describe a type itself.)</span></span>

<span data-ttu-id="65ff3-225">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-225">__Syntax:__</span></span>

```xml
<summary>description</summary>
```

<span data-ttu-id="65ff3-226">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-226">__Example:__</span></span>

```vb
''' <summary>
''' This constructor initializes the new Point to (0,0).
''' </summary>
Public Sub New()
    Me.New(0,0)
End Sub
```

### <a name="lttypeparamgt"></a><span data-ttu-id="65ff3-227">&lt;typeparam&gt;</span><span class="sxs-lookup"><span data-stu-id="65ff3-227">&lt;typeparam&gt;</span></span>

<span data-ttu-id="65ff3-228">Dieses Tag beschreibt einen Typparameter an.</span><span class="sxs-lookup"><span data-stu-id="65ff3-228">This tag describes a type parameter.</span></span>

<span data-ttu-id="65ff3-229">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-229">__Syntax:__</span></span>

```xml
<typeparam name="name">description</typeparam>
```

<span data-ttu-id="65ff3-230">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-230">__Example:__</span></span>

```vb
''' <typeparam name="T">
''' The base item type. Must implement IComparable.
''' </typeparam>
Public Class ItemManager(Of T As IComparable)
End Class
```

### <a name="ltvaluegt"></a><span data-ttu-id="65ff3-231">&lt;Wert&gt;</span><span class="sxs-lookup"><span data-stu-id="65ff3-231">&lt;value&gt;</span></span>

<span data-ttu-id="65ff3-232">Dieses Tag wird eine Eigenschaft beschreibt.</span><span class="sxs-lookup"><span data-stu-id="65ff3-232">This tag describes a property.</span></span>

<span data-ttu-id="65ff3-233">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-233">__Syntax:__</span></span>

```xml
<value>property description</value>
```

<span data-ttu-id="65ff3-234">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="65ff3-234">__Example:__</span></span>

```vb
''' <value>
''' Property <c>X</c> represents the point's x-coordinate.
''' </value>
Public Property X() As Integer
    Get
        Return _x
    End Get
    Set (Value As Integer)
        _x = Value
    End Set
End Property
```

## <a name="id-strings"></a><span data-ttu-id="65ff3-235">ID-Zeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="65ff3-235">ID Strings</span></span>

<span data-ttu-id="65ff3-236">Wenn die Dokumentationsdatei zu generieren, generiert der Compiler eine ID-Zeichenfolge für jedes Element im Quellcode, der mit einem Dokumentationskommentar gekennzeichnet ist, der eindeutig identifiziert.</span><span class="sxs-lookup"><span data-stu-id="65ff3-236">When generating the documentation file, the compiler generates an ID string for each element in the source code that is tagged with a documentation comment that uniquely identifies it.</span></span> <span data-ttu-id="65ff3-237">Diese ID-Zeichenfolge kann von externen Tools verwendet werden, um zu identifizieren, welches Element in einer kompilierten Assembly zugeordneten Dokumentkommentar entspricht.</span><span class="sxs-lookup"><span data-stu-id="65ff3-237">This ID string can be used by external tools to identify which element in a compiled assembly corresponds to the document comment.</span></span>

<span data-ttu-id="65ff3-238">ID-Zeichenfolgen werden wie folgt generiert:</span><span class="sxs-lookup"><span data-stu-id="65ff3-238">ID strings are generated as follows:</span></span>

<span data-ttu-id="65ff3-239">In der Zeichenfolge wird kein Leerraum platziert.</span><span class="sxs-lookup"><span data-stu-id="65ff3-239">No white space is placed in the string.</span></span>

<span data-ttu-id="65ff3-240">Der erste Teil der Zeichenfolge identifiziert die Art von Member dokumentiert wird, über ein einzelnes Zeichen, gefolgt von einem Doppelpunkt.</span><span class="sxs-lookup"><span data-stu-id="65ff3-240">The first part of the string identifies the kind of member being documented, via a single character followed by a colon.</span></span> <span data-ttu-id="65ff3-241">Die folgenden Arten von Membern definiert sind, mit dem zugehörigen Zeichen in Klammern dahinter: Ereignisse (E), Felder (F), Sie Methoden, einschließlich Konstruktoren und Operatoren (M), (N)-Namespaces, Eigenschaften (P) und Typen (T).</span><span class="sxs-lookup"><span data-stu-id="65ff3-241">The following kinds of members are defined, with the corresponding character in parenthesis after it: events (E), fields (F), methods including constructors and operators (M), namespaces (N), properties (P) and types (T).</span></span> <span data-ttu-id="65ff3-242">Fehler beim Generieren der ID-Zeichenfolge und der Rest der Zeichenfolge enthält Informationen über den Fehler, gibt ein Ausrufezeichen (!) an.</span><span class="sxs-lookup"><span data-stu-id="65ff3-242">An exclamation point (!) indicates an error occurred while generating the ID string, and the rest of the string provides information about the error.</span></span>

<span data-ttu-id="65ff3-243">Der zweite Teil der Zeichenfolge ist der vollqualifizierte Name des Elements ab, die im globalen Namespace.</span><span class="sxs-lookup"><span data-stu-id="65ff3-243">The second part of the string is the fully qualified name of the element, starting at the global namespace.</span></span> <span data-ttu-id="65ff3-244">Der Name des Elements, dessen einschließenden Typen und Namespace sind durch Punkte getrennt.</span><span class="sxs-lookup"><span data-stu-id="65ff3-244">The name of the element, its enclosing type(s), and namespace are separated by periods.</span></span> <span data-ttu-id="65ff3-245">Wenn der Name des Elements selbst Punkte enthält, werden sie ersetzt durch das Nummernzeichen (#).</span><span class="sxs-lookup"><span data-stu-id="65ff3-245">If the name of the item itself has periods, they are replaced by the pound sign (#).</span></span> <span data-ttu-id="65ff3-246">(Es wird vorausgesetzt, dass kein Element dieses Zeichen im Namen hat.) Der Name eines Typs mit den beiden Typparametern endet mit einem ' (Apostroph) gefolgt von einer Zahl, die die Anzahl der Typparameter für den Typ darstellt.</span><span class="sxs-lookup"><span data-stu-id="65ff3-246">(It is assumed that no element has this character in its name.) The name of a type with type parameters ends with a backquote (\`) followed by a number that represents the number of type parameters on the type.</span></span> <span data-ttu-id="65ff3-247">Es ist wichtig, die zu merken, da geschachtelte Typen Zugriff auf die Typparameter der Typen haben, die sie enthält geschachtelte Typen werden implizit die Typparameter der enthaltenden Typen enthalten, und diese Typen, in deren Typ Parameter Summen in diesem gezählt werden Fall.</span><span class="sxs-lookup"><span data-stu-id="65ff3-247">It is important to remember that because nested types have access to the type parameters of the types containing them, nested types implicitly contain the type parameters of their containing types, and those types are counted in their type parameter totals in this case.</span></span>

<span data-ttu-id="65ff3-248">Listen Sie für die Methoden und Eigenschaften mit Argumenten das Argument folgt in Klammern eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="65ff3-248">For methods and properties with arguments, the argument list follows, enclosed in parentheses.</span></span> <span data-ttu-id="65ff3-249">Für diejenigen ohne Argumente werden keine Klammern verwendet.</span><span class="sxs-lookup"><span data-stu-id="65ff3-249">For those without arguments, the parentheses are omitted.</span></span> <span data-ttu-id="65ff3-250">Die Argumente werden durch Kommas voneinander getrennt.</span><span class="sxs-lookup"><span data-stu-id="65ff3-250">The arguments are separated by commas.</span></span> <span data-ttu-id="65ff3-251">Die Codierung jedes Arguments entspricht einer Signatur CLI wie folgt: Argumente werden durch ihren vollqualifizierten Namen dargestellt.</span><span class="sxs-lookup"><span data-stu-id="65ff3-251">The encoding of each argument is the same as a CLI signature, as follows: Arguments are represented by their fully qualified name.</span></span> <span data-ttu-id="65ff3-252">Z. B. `Integer` wird `System.Int32`, `String` wird `System.String`, `Object` wird `System.Object`und so weiter.</span><span class="sxs-lookup"><span data-stu-id="65ff3-252">For example, `Integer` becomes `System.Int32`, `String` becomes `System.String`, `Object` becomes `System.Object`, and so on.</span></span> <span data-ttu-id="65ff3-253">Argumente, die mit der `ByRef` -Modifizierer aufweisen. ein "@" nach ihren Namen eingeben.</span><span class="sxs-lookup"><span data-stu-id="65ff3-253">Arguments having the `ByRef` modifier have a '@' following their type name.</span></span> <span data-ttu-id="65ff3-254">Argumente, die mit der `ByVal`, `Optional` oder `ParamArray` Modifizierer haben keine besondere Schreibweise.</span><span class="sxs-lookup"><span data-stu-id="65ff3-254">Arguments having the `ByVal`, `Optional` or `ParamArray` modifier have no special notation.</span></span> <span data-ttu-id="65ff3-255">Argumente, die Arrays werden als dargestellt `[lowerbound:size, ..., lowerbound:size]` , in denen die Anzahl von Kommas ist Rang minus 1, und die unteren Grenzen und die Größe jeder Dimension, sofern bekannt, Dezimal dargestellt werden.</span><span class="sxs-lookup"><span data-stu-id="65ff3-255">Arguments that are arrays are represented as `[lowerbound:size, ..., lowerbound:size]` where the number of commas is the rank - 1, and the lower bounds and size of each dimension, if known, are represented in decimal.</span></span> <span data-ttu-id="65ff3-256">Wenn die untere Grenze oder die Größe nicht angegeben ist, wird es weggelassen.</span><span class="sxs-lookup"><span data-stu-id="65ff3-256">If a lower bound or size is not specified, it is omitted.</span></span> <span data-ttu-id="65ff3-257">Wenn die untere Grenze und die Größe für eine bestimmte Dimension ausgelassen werden, kann der Doppelpunkt (:) ebenfalls ausgelassen werden.</span><span class="sxs-lookup"><span data-stu-id="65ff3-257">If the lower bound and size for a particular dimension are omitted, the ':' is omitted as well.</span></span> <span data-ttu-id="65ff3-258">Arrays von Arrays werden von einem dargestellt "`[]`" pro Ebene.</span><span class="sxs-lookup"><span data-stu-id="65ff3-258">Arrays of arrays are represented by one "`[]`" per level.</span></span>

### <a name="id-string-examples"></a><span data-ttu-id="65ff3-259">Beispiele für die ID-Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="65ff3-259">ID string examples</span></span>

<span data-ttu-id="65ff3-260">In den folgenden Beispielen wird jede zeigen ein Fragment des VB-Code zusammen mit der ID-Zeichenfolge, die von jedem Quellelement für einen Dokumentationskommentar erstellt:</span><span class="sxs-lookup"><span data-stu-id="65ff3-260">The following examples each show a fragment of VB code, along with the ID string produced from each source element capable of having a documentation comment:</span></span>

<span data-ttu-id="65ff3-261">Typen werden mit dem vollqualifizierten Namen dargestellt.</span><span class="sxs-lookup"><span data-stu-id="65ff3-261">Types are represented using their fully qualified name.</span></span>

```vb
Enum Color
    Red
    Blue
    Green
End Enum

Namespace Acme
    Interface IProcess
    End Interface

    Structure ValueType
        ...
    End Structure

    Class Widget
        Public Class NestedClass
        End Class

        Public Interface IMenuItem
        End Interface

        Public Delegate Sub Del(i As Integer)

        Public Enum Direction
            North
            South
            East
            West
        End Enum
    End Class
End Namespace

"T:Color"
"T:Acme.IProcess"
"T:Acme.ValueType"
"T:Acme.Widget"
"T:Acme.Widget.NestedClass"
"T:Acme.Widget.IMenuItem"
"T:Acme.Widget.Del"
"T:Acme.Widget.Direction"
```

<span data-ttu-id="65ff3-262">Felder werden durch ihren vollqualifizierten Namen dargestellt.</span><span class="sxs-lookup"><span data-stu-id="65ff3-262">Fields are represented by their fully qualified name.</span></span>

```vb
Namespace Acme
    Structure ValueType
        Private total As Integer
    End Structure

    Class Widget
        Public Class NestedClass
            Private value As Integer
        End Class

        Private message As String
        Private Shared defaultColor As Color
        Private Const PI As Double = 3.14159
        Protected ReadOnly monthlyAverage As Double
        Private array1() As Long
        Private array2(,) As Widget
    End Class
End Namespace

"F:Acme.ValueType.total"
"F:Acme.Widget.NestedClass.value"
"F:Acme.Widget.message"
"F:Acme.Widget.defaultColor"
"F:Acme.Widget.PI"
"F:Acme.Widget.monthlyAverage"
"F:Acme.Widget.array1"
"F:Acme.Widget.array2"
```

<span data-ttu-id="65ff3-263">Konstruktoren.</span><span class="sxs-lookup"><span data-stu-id="65ff3-263">Constructors.</span></span>

```vb
Namespace Acme
    Class Widget
        Shared Sub New()
        End Sub

        Public Sub New()
        End Sub

        Public Sub New(s As String)
        End Sub
    End Class
End Namespace

"M:Acme.Widget.#cctor"
"M:Acme.Widget.#ctor"
"M:Acme.Widget.#ctor(System.String)"
```

<span data-ttu-id="65ff3-264">-Methoden.</span><span class="sxs-lookup"><span data-stu-id="65ff3-264">Methods.</span></span>

```vb
Namespace Acme
    Structure ValueType
        Public Sub M(i As Integer)
        End Sub
    End Structure

    Class Widget
        Public Class NestedClass
            Public Sub M(i As Integer)
            End Sub
        End Class

        Public Shared Sub M0()
        End Sub

        Public Sub M1(c As Char, ByRef f As Float, _
            ByRef v As ValueType)
        End Sub

        Public Sub M2(x1() As Short, x2(,) As Integer, _
            x3()() As Long)
        End Sub

        Public Sub M3(x3()() As Long, x4()(,,) As Widget)
        End Sub

        Public Sub M4(Optional i As Integer = 1)

        Public Sub M5(ParamArray args() As Object)
        End Sub
    End Class
End Namespace

"M:Acme.ValueType.M(System.Int32)"
"M:Acme.Widget.NestedClass.M(System.Int32)"
"M:Acme.Widget.M0"
"M:Acme.Widget.M1(System.Char,System.Single@,Acme.ValueType@)"
"M:Acme.Widget.M2(System.Int16[],System.Int32[0:,0:],System.Int64[][])"
"M:Acme.Widget.M3(System.Int64[][],Acme.Widget[0:,0:,0:][])"
"M:Acme.Widget.M4(System.Int32)"
"M:Acme.Widget.M5(System.Object[])"
```

<span data-ttu-id="65ff3-265">Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="65ff3-265">Properties.</span></span>

```vb
Namespace Acme
    Class Widget
        Public Property Width() As Integer
            Get
            End Get
            Set (Value As Integer)
            End Set
        End Property

        Public Default Property Item(i As Integer) As Integer
            Get
            End Get
            Set (Value As Integer)
            End Set
        End Property

        Public Default Property Item(s As String, _
            i As Integer) As Integer
            Get
            End Get
            Set (Value As Integer)
            End Set
        End Property
    End Class
End Namespace

"P:Acme.Widget.Width"
"P:Acme.Widget.Item(System.Int32)"
"P:Acme.Widget.Item(System.String,System.Int32)"
```

<span data-ttu-id="65ff3-266">Ereignisse</span><span class="sxs-lookup"><span data-stu-id="65ff3-266">Events</span></span>   

```vb
Namespace Acme
    Class Widget
        Public Event AnEvent As EventHandler
        Public Event AnotherEvent()
    End Class
End Namespace

"E:Acme.Widget.AnEvent"
"E:Acme.Widget.AnotherEvent"
```

<span data-ttu-id="65ff3-267">Operatoren.</span><span class="sxs-lookup"><span data-stu-id="65ff3-267">Operators.</span></span>

```vb
Namespace Acme
    Class Widget
        Public Shared Operator +(x As Widget) As Widget
        End Operator

        Public Shared Operator +(x1 As Widget, x2 As Widget) As Widget
        End Operator
    End Class
End Namespace

"M:Acme.Widget.op_UnaryPlus(Acme.Widget)"
"M:Acme.Widget.op_Addition(Acme.Widget,Acme.Widget)"
```

<span data-ttu-id="65ff3-268">Konvertierungsoperatoren verfügen über einen nachgestellten `~` gefolgt von den Rückgabetyp.</span><span class="sxs-lookup"><span data-stu-id="65ff3-268">Conversion operators have a trailing `~` followed by the return type.</span></span>

```vb
Namespace Acme
    Class Widget
        Public Shared Narrowing Operator CType(x As Widget) As _
            Integer
        End Operator

        Public Shared Widening Operator CType(x As Widget) As Long
        End Operator
    End Class
End Namespace

"M:Acme.Widget.op_Explicit(Acme.Widget)~System.Int32"
"M:Acme.Widget.op_Implicit(Acme.Widget)~System.Int64"
```

## <a name="documentation-comments-example"></a><span data-ttu-id="65ff3-269">Beispiel für eine Referenzdokumentation Kommentare</span><span class="sxs-lookup"><span data-stu-id="65ff3-269">Documentation comments example</span></span>

<span data-ttu-id="65ff3-270">Das folgende Beispiel zeigt den Quellcode einer `Point` Klasse:</span><span class="sxs-lookup"><span data-stu-id="65ff3-270">The following example shows the source code of a `Point` class:</span></span>

```vb
Namespace Graphics
    ''' <remarks>
    ''' Class <c>Point</c> models a point in a two-dimensional
    ''' plane.
    ''' </remarks>
    Public Class Point
        ''' <summary>
        ''' Instance variable <c>x</c> represents the point's x-coordinate.
        ''' </summary>
        Private _x As Integer

        ''' <summary>
        ''' Instance variable <c>y</c> represents the point's y-coordinate.
        ''' </summary>
        Private _y As Integer

        ''' <value>
        ''' Property <c>X</c> represents the point's x-coordinate.
        ''' </value>
        Public Property X() As Integer
            Get
                Return _x
            End Get
            Set(Value As Integer)
                _x = Value
            End Set
        End Property

        ''' <value>
        ''' Property <c>Y</c> represents the point's y-coordinate.
        ''' </value>
        Public Property Y() As Integer
            Get
                Return _y
            End Get
            Set(Value As Integer)
                _y = Value
            End Set
        End Property

        ''' <summary>
        ''' This constructor initializes the new Point to (0,0).
        ''' </summary>
        Public Sub New()
            Me.New(0, 0)
        End Sub

        ''' <summary>
        ''' This constructor initializes the new Point to
        ''' (<paramref name="x"/>,<paramref name="y"/>).
        ''' </summary>
        ''' <param name="x"><c>x</c> is the new Point's
        ''' x-coordinate.</param>
        ''' <param name="y"><c>y</c> is the new Point's
        ''' y-coordinate.</param>
        Public Sub New(x As Integer, y As Integer)
            Me.X = x
            Me.Y = y
        End Sub

        ''' <summary>
        ''' This method changes the point's location to the given
        ''' coordinates.
        ''' </summary>
        ''' <param name="x"><c>x</c> is the new x-coordinate.</param>
        ''' <param name="y"><c>y</c> is the new y-coordinate.</param>
        ''' <see cref="Translate"/>
        Public Sub Move(x As Integer, y As Integer)
            Me.X = x
            Me.Y = y
        End Sub

        ''' <summary>
        ''' This method changes the point's location by the given x- and
        ''' y-offsets.
        ''' <example>
        ''' For example:
        ''' <code>
        '''    Dim p As Point = New Point(3, 5)
        '''    p.Translate(-1, 3)
        ''' </code>
        ''' results in <c>p</c>'s having the value (2,8).
        ''' </example>
        ''' </summary>
        ''' <param name="x"><c>x</c> is the relative x-offset.</param>
        ''' <param name="y"><c>y</c> is the relative y-offset.</param>
        ''' <see cref="Move"/>
        Public Sub Translate(x As Integer, y As Integer)
            Me.X += x
            Me.Y += y
        End Sub

        ''' <summary>
        ''' This method determines whether two Points have the same
        ''' location.
        ''' </summary>
        ''' <param name="o"><c>o</c> is the object to be compared to the
        ''' current object.</param>
        ''' <returns>
        ''' True if the Points have the same location and they have the
        ''' exact same type; otherwise, false.
        ''' </returns>
        ''' <seealso cref="Operator op_Equality"/>
        ''' <seealso cref="Operator op_Inequality"/>
        Public Overrides Function Equals(o As Object) As Boolean
            If o Is Nothing Then
                Return False
            End If
            If o Is Me Then
                Return True
            End If
            If Me.GetType() Is o.GetType() Then
                Dim p As Point = CType(o, Point)
                Return (X = p.X) AndAlso (Y = p.Y)
            End If
            Return False
        End Function

        ''' <summary>
        ''' Report a point's location as a string.
        ''' </summary>
        ''' <returns>
        ''' A string representing a point's location, in the form
        ''' (x,y), without any leading, training, or embedded whitespace.
        ''' </returns>
        Public Overrides Function ToString() As String
            Return "(" & X & "," & Y & ")"
        End Function

        ''' <summary>
        ''' This operator determines whether two Points have the
        ''' same location.
        ''' </summary>
        ''' <param name="p1"><c>p1</c> is the first Point to be compared.
        ''' </param>
        ''' <param name="p2"><c>p2</c> is the second Point to be compared.
        ''' </param>
        ''' <returns>
        ''' True if the Points have the same location and they 
        ''' have the exact same type; otherwise, false.
        ''' </returns>
        ''' <seealso cref="Equals"/>
        ''' <seealso cref="op_Inequality"/>
        Public Shared Operator =(p1 As Point, p2 As Point) As Boolean
            If p1 Is Nothing OrElse p2 Is Nothing Then
                Return False
            End If
            If p1.GetType() Is p2.GetType() Then
                Return (p1.X = p2.X) AndAlso (p1.Y = p2.Y)
            End If
            Return False
        End Operator

        ''' <summary>
        ''' This operator determines whether two Points have the
        ''' same location.
        ''' </summary>
        ''' <param name="p1"><c>p1</c> is the first Point to be comapred.
        ''' </param>
        ''' <param name="p2"><c>p2</c> is the second Point to be compared.
        ''' </param>
        ''' <returns>
        ''' True if the Points do not have the same location and
        ''' the exact same type; otherwise, false.
        ''' </returns>
        ''' <seealso cref="Equals"/>
        ''' <seealso cref="op_Equality"/>
        Public Shared Operator <>(p1 As Point, p2 As Point) As Boolean
            Return Not p1 = p2
        End Operator

        ''' <summary>
        ''' This is the entry point of the Point class testing program.
        ''' <para>This program tests each method and operator, and
        ''' is intended to be run after any non-trvial maintenance has
        ''' been performed on the Point class.</para>
        ''' </summary>
        Public Shared Sub Main()
            ' class test code goes here
        End Sub
    End Class
End Namespace
```

<span data-ttu-id="65ff3-271">Dies ist die Ausgabe erzeugt, wenn den Quellcode für die Klasse `Point`, wie oben gezeigt:</span><span class="sxs-lookup"><span data-stu-id="65ff3-271">Here is the output produced when given the source code for class `Point`, shown above:</span></span>

```xml
<?xml version="1.0"?>
<doc>
    <assembly>
        <name>Point</name>
    </assembly>
    <members>
        <member name="T:Graphics.Point">
            <remarks>Class <c>Point</c> models a point in a
            two-dimensional plane. </remarks>
        </member>
        <member name="F:Graphics.Point.x">
            <summary>Instance variable <c>x</c> represents the point's
            x-coordinate.</summary>
        </member>
        <member name="F:Graphics.Point.y">
            <summary>Instance variable <c>y</c> represents the point's
            y-coordinate.</summary>
        </member>
        <member name="M:Graphics.Point.#ctor">
            <summary>This constructor initializes the new Point to
            (0,0).</summary>
        </member>
        <member name="M:Graphics.Point.#ctor(System.Int32,System.Int32)">
            <summary>This constructor initializes the new Point to
            (<paramref name="x"/>,<paramref name="y"/>).</summary>
            <param><c>x</c> is the new Point's x-coordinate.</param>
            <param><c>y</c> is the new Point's y-coordinate.</param>
        </member>
        <member name="M:Graphics.Point.Move(System.Int32,System.Int32)">
            <summary>This method changes the point's location to
            the given coordinates.</summary>
            <param><c>x</c> is the new x-coordinate.</param>
            <param><c>y</c> is the new y-coordinate.</param>
            <see cref=
            "M:Graphics.Point.Translate(System.Int32,System.Int32)"/>
        </member>
        <member name=
        "M:Graphics.Point.Translate(System.Int32,System.Int32)">
            <summary>This method changes the point's location by the given
            x- and y-offsets.
            <example>For example:
            <code>
            Point p = new Point(3,5);
            p.Translate(-1,3);
            </code>
            results in <c>p</c>'s having the value (2,8).
            </example>
            </summary>
            <param><c>x</c> is the relative x-offset.</param>
            <param><c>y</c> is the relative y-offset.</param>
            <see cref="M:Graphics.Point.Move(System.Int32,System.Int32)"/>
        </member>
        <member name="M:Graphics.Point.Equals(System.Object)">
            <summary>This method determines whether two Points have the
            same location.</summary>
            <param><c>o</c> is the object to be compared to the current
            object.</param>
            <returns>True if the Points have the same location and they
            have the exact same type; otherwise, false.</returns>
            <seealso cref=
            "M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"
            />
            <seealso cref=
           "M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"
            />
        </member>
        <member name="M:Graphics.Point.ToString">
            <summary>Report a point's location as a string.</summary>
            <returns>A string representing a point's location, in the form
            (x,y), without any leading, training, or embedded
            whitespace.</returns>
        </member>
        <member name=
        "M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points have the same location and they
            have the exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso cref=
           "M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"
            />
        </member>
        <member name=
        "M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points do not have the same location and
            the exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso cref=
            "M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"
            />
        </member>
        <member name="M:Graphics.Point.Main">
            <summary>This is the entry point of the Point class testing
            program.
            <para>This program tests each method and operator, and
            is intended to be run after any non-trvial maintenance has
            been performed on the Point class.</para>
            </summary>
        </member>
        <member name="P:Graphics.Point.X">
            <value>Property <c>X</c> represents the point's
            x-coordinate.</value>
        </member>
        <member name="P:Graphics.Point.Y">
            <value>Property <c>Y</c> represents the point's
            y-coordinate.</value>
        </member>
    </members>
</doc>
```
