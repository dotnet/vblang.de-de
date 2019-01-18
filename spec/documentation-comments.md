---
ms.openlocfilehash: 8a36506d9fce1605cf3758536f51782ea7680e84
ms.sourcegitcommit: 6eca149bdc736113e0adb709212bd266c9503c33
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2019
ms.locfileid: "47426662"
---
# <a name="documentation-comments"></a>Kommentare zur Dokumentation

Dokumentationskommentare sind speziell formatierten Kommentaren in der Quelle, die analysiert werden können, um die Dokumentation über den Code zu erzeugen, die sie verbunden sind. Das grundlegende Format für Dokumentationskommentare ist XML. Wenn kann der kompilierte Code mit Dokumentationskommentaren, der Compiler optional auch eine XML-Datei ausgeben, die die Gesamtsumme der die Dokumentationskommentare in der Quelle darstellt. Diese XML-Datei kann dann von anderen Tools verwendet werden, um Druck oder online-Dokumentation zu erstellen.

In diesem Kapitel wird beschrieben, Kommentare aus Dokumenten und empfohlene XML-Tags für die Verwendung mit Kommentare aus Dokumenten.

## <a name="documentation-comment-format"></a>Dokumentationskommentare im Standardformat

Kommentare aus Dokumenten sind besondere Kommentare, die mit beginnen `'''`, drei einfache Anführungszeichen ein. Sie müssen unmittelbar voranstehen, den Typ (z. B. eine Klasse, Delegat oder Schnittstelle) oder den Typmember (z. B. ein Feld, Ereignis, Eigenschaft oder Methode), den sie dokumentieren. Ein Dokumentkommentar für eine partielle Methodendeklaration wird durch den Dokumentkommentar für die Methode, die Text enthält, liefert ersetzt werden, sofern vorhanden. Alle angrenzenden Dokumentkommentare werden zusammen angefügt, um ein einzelnes Dokumentkommentar zu erzeugen. Es ist ein Zeichen, Leerzeichen folgt die `'''` -Zeichen, und klicken Sie dann diese Leerzeichen nicht in die Verkettung enthalten ist. Zum Beispiel:

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

Dokumentationskommentare muss werden wohlgeformtes XML gemäß http://www.w3.org/TR/REC-xml. Wenn der XML-Code nicht wohlgeformt ist, wird eine Warnung generiert, und die Dokumentationsdatei enthält einen Kommentar, der besagt, dass ein Fehler aufgetreten ist.

Obwohl Entwickler können ihren eigenen Satz von Tags zu erstellen sind, wird eine empfohlene Sammlung im nächsten Abschnitt definiert. Einige der empfohlenen Tags haben eine besondere Bedeutung:

* Die `<param>` Tag wird verwendet, um Parameter zu beschreiben. Der Parameter, die gemäß einer `<param>` Tag muss vorhanden sein und alle Parameter des Typmembers müssen in der Dokumentationskommentar beschrieben. Wenn eine der Bedingungen nicht zutrifft, gibt der Compiler eine Warnung aus.

* Das `cref`-Attribut kann an jedes Tag angefügt werden, um einen Verweis auf ein Codeelement bereitzustellen. Das Code-Element muss vorhanden sein; zum Zeitpunkt der Kompilierung ersetzt der Compiler den Namen, mit der ID-Zeichenfolge, die den Member darstellt. Wenn der Code-Element nicht vorhanden ist, gibt der Compiler eine Warnung aus. Bei der Suche für ein Namen im beschrieben eine `cref` Attribut, das Compiler-Hinsicht `Imports` Anweisungen, die innerhalb der enthaltenden Quelldatei angezeigt werden.

* Die `<summary>` richtet sich an Tag durch eine Dokumentations-Viewer verwendet werden, um weitere Informationen über einen Typ oder Member anzuzeigen.

Beachten Sie, dass die Dokumentationsdatei keine vollständigen Informationen über einen Typ und Member, ausschließlich in die Kommentare aus Dokumenten enthalten ist. Weitere Informationen über einen Typ oder Member zu erhalten, muss die Dokumentationsdatei zusammen mit Reflektion für den tatsächlichen Typ oder Member verwendet werden.

## <a name="recommended-tags"></a>Empfohlene tags

Der Dokumentations-Generator muss akzeptiert und verarbeitet alle Tags, die gemäß den Regeln der XML gültig ist. Die folgenden Tags stellen häufig verwendete Funktionen in der Benutzerdokumentation bereit:

`<c>` Legt Text in einer Code-ähnliche Schriftart fest.

`<code>` Legt eine oder mehrere Zeilen von Code oder Programmausgabe Quellausgabe in einer Code-ähnliche Schriftart

`<example>` Gibt an, Beispiel

`<exception>` Identifiziert die Ausnahmen, die eine Methode ausgelöst werden können

`<include>` Enthält ein externes XML-Dokument

`<list>` Erstellt eine Liste oder Tabelle

`<para>` Ermöglicht der Struktur, die Text hinzugefügt werden

`<param>` Beschreibt einen Parameter für eine Methode oder Konstruktor

`<paramref>` Gibt an, dass ein Wort ein Parametername ist.

`<permission>` Dokumentiert die Security Zugriff auf ein Element

`<remarks>` Beschreibt einen Typ

`<returns>` Beschreibt den Rückgabewert einer Methode

`<see>` Gibt an, einen link

`<seealso>` Generiert einen Eintrag auch finden Sie unter

`<summary>` Beschreibt einen Member eines Typs

`<typeparam>` Beschreibt einen Typparameter

`<value>` Beschreibt eine Eigenschaft

### <a name="ltcgt"></a>&lt;c&gt;

Dieses Tag gibt an, dass ein Fragment von Text in eine Beschreibung eine Schriftart, die für einen Codeblock verwendet verwenden soll. (Verwenden Sie für die Feldlinien des tatsächlichen Code, `<code>`.)

__Syntax:__

```xml
<c>text to be set like code</c>
```

__Beispiel:__

```vb
''' <remarks>
''' Class <c>Point</c> models a point in a two-dimensional plane.
''' </remarks>
Public Class Point 
End Class
```

### <a name="ltcodegt"></a>&lt;Code&gt;

Dieses Tag gibt an, dass eine oder mehrere Zeilen von Code oder Programmausgabe Quellausgabe eine Schriftart mit fester Zeichenbreite verwenden soll. (Verwenden Sie für kleine Codefragmente, `<c>`.)

__Syntax:__

```xml
<code>source code or program output</code>
```

__Beispiel:__

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

### <a name="ltexamplegt"></a>&lt;example&gt;

Dieses Tag ermöglicht Beispielcode in einem Kommentar angezeigt, wie ein Element verwendet werden kann. Normalerweise umfassen diese immer mit dem Tag `<code>` ebenfalls.

__Syntax:__

```xml
<example>description</example>
```

__Beispiel:__

Ein Beispiel finden Sie unter `<code>`.

### <a name="ltexceptiongt"></a>&lt;exception&gt;

Dieses Tag bietet eine Möglichkeit, die Ausnahmen zu dokumentieren, die eine Methode ausgelöst werden kann.

__Syntax:__

```xml
<exception cref="member">description</exception>
```

__Beispiel:__

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

### <a name="ltincludegt"></a>&lt;include&gt;

Dieses Tag wird verwendet, um Informationen aus einer externen wohlgeformtes XML-Dokument enthalten. Ein XPath-Ausdruck wird angewendet, im XML-Dokument, um anzugeben, welche XML aus dem Dokument enthalten sein soll. Die `<include>` Tag wird dann mit den ausgewählten XML-Code aus dem externen Dokument ersetzt.

__Syntax:__

```xml
<include file="filename" path="xpath">
```

__Beispiel:__

Wenn der Quellcode eine Deklaration wie der folgenden enthalten:

```vb
''' <include file="docs.xml" path="extra/class[@name="IntList"]/*" />
```

und der externen Datei docs.xml hatte den folgenden Inhalt

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

Klicken Sie dann ist die gleiche Dokumentation Ausgabe aus, als ob der Quellcode enthalten:

```xml
''' <summary>
''' Contains a list of integers.
''' </summary>
```

### <a name="ltlistgt"></a>&lt;list&gt;

Dieses Tag wird verwendet, um eine Liste oder Tabelle der Elemente zu erstellen. Sie enthält eventuell eine `<listheader>` Block, um die Überschriftenzeile einer Tabelle oder einer Definitionsliste zu definieren. (Wenn Sie eine Tabelle zu definieren, muss nur ein Eintrag für den Ausdruck in der Überschrift "" angegeben werden.)

Jedes Element in der Liste wird angegeben, mit einem `<item>` Block. Wenn Sie eine Definitionsliste zu erstellen, müssen sowohl Begriff und eine Beschreibung angegeben werden. Allerdings muss für eine Tabelle, Liste mit Aufzählungszeichen oder nummerierte Liste, nur die Beschreibung angegeben werden.

__Syntax:__

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

__Beispiel:__

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

### <a name="ltparagt"></a>&lt;para&gt;

Dieses Tag ist für die Verwendung in anderen Tags, wie z. B. `<remarks>` oder `<returns>`, und lässt die Struktur, die Text hinzugefügt werden.

__Syntax:__

```xml
<para>content</para>
```

__Beispiel:__

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

### <a name="ltparamgt"></a>&lt;param&gt;

Dieses Tag beschreibt einen Parameter für eine Methode, den Konstruktor oder die indizierte Eigenschaft.

__Syntax:__

```xml
<param name="name">description</param>
```

__Beispiel:__

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

### <a name="ltparamrefgt"></a>&lt;paramref&gt;

Dieses Tag gibt an, dass ein Wort ein Parameter ist. Die Dokumentationsdatei kann verarbeitet werden, um diesen Parameter auf unterschiedliche Weise zu formatieren.

__Syntax:__

```xml
<paramref name="name"/>
```

__Beispiel:__

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

### <a name="ltpermissiongt"></a>&lt;permission&gt;

Dieses Tag dokumentiert die Security Zugriff auf ein Element

__Syntax:__

```xml
<permission cref="member">description</permission>
```

__Beispiel:__

```vb
''' <permission cref="System.Security.PermissionSet">Everyone can
''' access this method.</permission>
Public Shared Sub Test()
End Sub
```

### <a name="ltremarksgt"></a>&lt;remarks&gt;

Dieses Tag Gibt allgemeine Informationen über einen Typ an. (Verwenden `<summary>` um der Member eines Typs beschreiben.)

__Syntax:__

```xml
<remarks>description</remarks>
```

__Beispiel:__

```vb
''' <remarks>
''' Class <c>Point</c> models a point in a two-dimensional plane.
''' </remarks>
Public Class Point 
End Class
```

### <a name="ltreturnsgt"></a>&lt;returns&gt;

Dieses Tag wird der Rückgabewert einer Methode beschrieben.

__Syntax:__

```xml
<returns>description</returns>
```

__Beispiel:__

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

### <a name="ltseegt"></a>&lt;see&gt;

Dieses Tag ermöglicht einen Link im Text angegeben werden. (Verwenden `<seealso>` um Text anzugeben, dass in einem Abschnitt Siehe auch angezeigt werden.)

__Syntax:__

```xml
<see cref="member"/>
```

__Beispiel:__

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

### <a name="ltseealsogt"></a>&lt;seealso&gt;

Dieses Tag wird einen Eintrag im Abschnitt Siehe auch generiert. (Verwenden `<see>` auf einen Link im Text angegeben.)

__Syntax:__

```xml
<seealso cref="member"/>
```

__Beispiel:__

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

### <a name="ltsummarygt"></a>&lt;summary&gt;

Dieses Tag beschreibt einen Typmember an. (Verwenden `<remarks>` um einen Typ selbst zu beschreiben.)

__Syntax:__

```xml
<summary>description</summary>
```

__Beispiel:__

```vb
''' <summary>
''' This constructor initializes the new Point to (0,0).
''' </summary>
Public Sub New()
    Me.New(0,0)
End Sub
```

### <a name="lttypeparamgt"></a>&lt;typeparam&gt;

Dieses Tag beschreibt einen Typparameter an.

__Syntax:__

```xml
<typeparam name="name">description</typeparam>
```

__Beispiel:__

```vb
''' <typeparam name="T">
''' The base item type. Must implement IComparable.
''' </typeparam>
Public Class ItemManager(Of T As IComparable)
End Class
```

### <a name="ltvaluegt"></a>&lt;value&gt;

Dieses Tag wird eine Eigenschaft beschreibt.

__Syntax:__

```xml
<value>property description</value>
```

__Beispiel:__

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

## <a name="id-strings"></a>ID-Zeichenfolgen

Wenn die Dokumentationsdatei zu generieren, generiert der Compiler eine ID-Zeichenfolge für jedes Element im Quellcode, der mit einem Dokumentationskommentar gekennzeichnet ist, der eindeutig identifiziert. Diese ID-Zeichenfolge kann von externen Tools verwendet werden, um zu identifizieren, welches Element in einer kompilierten Assembly zugeordneten Dokumentkommentar entspricht.

ID-Zeichenfolgen werden wie folgt generiert:

In der Zeichenfolge wird kein Leerraum platziert.

Der erste Teil der Zeichenfolge identifiziert die Art von Member dokumentiert wird, über ein einzelnes Zeichen, gefolgt von einem Doppelpunkt. Die folgenden Arten von Membern definiert sind, mit dem zugehörigen Zeichen in Klammern dahinter: Ereignisse (E), Felder (F), Sie Methoden, einschließlich Konstruktoren und Operatoren (M), (N)-Namespaces, Eigenschaften (P) und Typen (T). Fehler beim Generieren der ID-Zeichenfolge und der Rest der Zeichenfolge enthält Informationen über den Fehler, gibt ein Ausrufezeichen (!) an.

Der zweite Teil der Zeichenfolge ist der vollqualifizierte Name des Elements ab, die im globalen Namespace. Der Name des Elements, dessen einschließenden Typen und Namespace sind durch Punkte getrennt. Wenn der Name des Elements selbst Punkte enthält, werden sie ersetzt durch das Nummernzeichen (#). (Es wird vorausgesetzt, dass kein Element dieses Zeichen im Namen hat.) Der Name eines Typs mit den beiden Typparametern endet mit einem ' (Apostroph) gefolgt von einer Zahl, die die Anzahl der Typparameter für den Typ darstellt. Es ist wichtig, die zu merken, da geschachtelte Typen Zugriff auf die Typparameter der Typen haben, die sie enthält geschachtelte Typen werden implizit die Typparameter der enthaltenden Typen enthalten, und diese Typen, in deren Typ Parameter Summen in diesem gezählt werden Fall.

Listen Sie für die Methoden und Eigenschaften mit Argumenten das Argument folgt in Klammern eingeschlossen. Für diejenigen ohne Argumente werden keine Klammern verwendet. Die Argumente werden durch Kommas voneinander getrennt. Die Codierung jedes Arguments ist identisch mit einer CLI-Signatur wie folgt: Argumente werden durch ihre vollqualifizierten Namen dargestellt. Z. B. `Integer` wird `System.Int32`, `String` wird `System.String`, `Object` wird `System.Object`und so weiter. Argumente, die mit der `ByRef` -Modifizierer aufweisen. ein "@" nach ihren Namen eingeben. Argumente, die mit der `ByVal`, `Optional` oder `ParamArray` Modifizierer haben keine besondere Schreibweise. Argumente, die Arrays werden als dargestellt `[lowerbound:size, ..., lowerbound:size]` , in denen die Anzahl von Kommas ist Rang minus 1, und die unteren Grenzen und die Größe jeder Dimension, sofern bekannt, Dezimal dargestellt werden. Wenn die untere Grenze oder die Größe nicht angegeben ist, wird es weggelassen. Wenn die untere Grenze und die Größe für eine bestimmte Dimension ausgelassen werden, kann der Doppelpunkt (:) ebenfalls ausgelassen werden. Arrays von Arrays werden von einem dargestellt "`[]`" pro Ebene.

### <a name="id-string-examples"></a>Beispiele für die ID-Zeichenfolge

In den folgenden Beispielen wird jede zeigen ein Fragment des VB-Code zusammen mit der ID-Zeichenfolge, die von jedem Quellelement für einen Dokumentationskommentar erstellt:

Typen werden mit dem vollqualifizierten Namen dargestellt.

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

Felder werden durch ihren vollqualifizierten Namen dargestellt.

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

Konstruktoren.

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

-Methoden.

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

Eigenschaften.

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

Ereignisse   

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

Operatoren.

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

Konvertierungsoperatoren verfügen über einen nachgestellten `~` gefolgt von den Rückgabetyp.

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

## <a name="documentation-comments-example"></a>Beispiel für eine Referenzdokumentation Kommentare

Das folgende Beispiel zeigt den Quellcode einer `Point` Klasse:

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

Dies ist die Ausgabe erzeugt, wenn den Quellcode für die Klasse `Point`, wie oben gezeigt:

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
