---
ms.openlocfilehash: 3d5c1e90283b6d6ec8cdeccd35e32c78f997cc27
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306036"
---
# <a name="type-members"></a>Typmember

Typmember definieren Speicherorte und ausführbaren Code. Dabei kann es sich um Methoden, Konstruktoren, Ereignisse, Konstanten, Variablen und Eigenschaften handeln.

## <a name="interface-method-implementation"></a>Implementierung der Schnittstellen Methode

Methoden, Ereignisse und Eigenschaften können Schnittstellenmember implementieren. Um einen Schnittstellenmember zu implementieren, gibt eine Element Deklaration das `Implements`-Schlüsselwort an und listet mindestens ein Schnittstellenmember auf.

```antlr
ImplementsClause
    : ( 'Implements' ImplementsList )?
    ;

ImplementsList
    : InterfaceMemberSpecifier ( Comma InterfaceMemberSpecifier )*
    ;

InterfaceMemberSpecifier
    : NonArrayTypeName Period IdentifierOrKeyword
    ;
```

Methoden und Eigenschaften, die Schnittstellenmember implementieren, sind implizit `NotOverridable`, es sei denn, Sie deklarieren als `MustOverride`, `Overridable` oder überschreiben ein anderes Element. Es ist ein Fehler, wenn ein Member, der einen Schnittstellenmember implementiert, `Shared` ist. Die Barrierefreiheit eines Members hat keine Auswirkung auf die Möglichkeit, Schnittstellenmember zu implementieren.

Damit eine Schnittstellen Implementierung gültig ist, muss die implementierende Liste des enthaltenden Typs eine Schnittstelle benennen, die einen kompatiblen Member enthält. Ein kompatibles Member ist ein Element, dessen Signatur mit der Signatur des implementierenden Members übereinstimmt. Wenn eine generische Schnittstelle implementiert wird, wird das Typargument, das in der implementierten Klausel bereitgestellt wird, beim Überprüfen der Kompatibilität in die Signatur ersetzt. Zum Beispiel:

```vb
Interface I1(Of T)
    Sub F(x As T)
End Interface

Class C1
    Implements I1(Of Integer)

    Sub F(x As Integer) Implements I1(Of Integer).F
    End Sub
End Class

Class C2(Of U)
    Implements I1(Of U)

    Sub F(x As U) Implements I1(Of U).F
    End Sub
End Class
```

Wenn ein Ereignis, das mit einem Delegattyp deklariert wurde, ein Schnittstellen Ereignis implementiert, dann ist ein kompatibles Ereignis, dessen zugrunde liegender Delegattyp derselbe Typ ist. Andernfalls verwendet das Ereignis den Delegattyp aus dem Schnittstellen Ereignis, das er implementiert. Wenn ein derartiges Ereignis mehrere Schnittstellen Ereignisse implementiert, müssen alle Schnittstellen Ereignisse denselben zugrunde liegenden Delegattyp aufweisen. Zum Beispiel:

```vb
Interface ClickEvents
    Event LeftClick(x As Integer, y As Integer)
    Event RightClick(x As Integer, y As Integer)
End Interface

Class Button
    Implements ClickEvents

    ' OK. Signatures match, delegate type = ClickEvents.LeftClickHandler.
    Event LeftClick(x As Integer, y As Integer) _
        Implements ClickEvents.LeftClick

    ' OK. Signatures match, delegate type = ClickEvents.RightClickHandler.
    Event RightClick(x As Integer, y As Integer) _
        Implements ClickEvents.RightClick
End Class

Class Label
    Implements ClickEvents

    ' Error. Signatures match, but can't be both delegate types.
    Event Click(x As Integer, y As Integer) _
        Implements ClickEvents.LeftClick, ClickEvents.RightClick
End Class
```

Ein Schnittstellenmember in der implementierten Liste wird mithilfe eines Typnamens, eines Zeitraums und eines Bezeichners angegeben. Der Typname muss eine Schnittstelle in der implementierten Liste oder eine Basisschnittstelle einer Schnittstelle in der implementierten Liste sein, und der Bezeichner muss ein Member der angegebenen Schnittstelle sein. Ein einzelner Member kann mehr als einen übereinstimmenden Schnittstellenmember implementieren.

```vb
Interface ILeft
    Sub F()
End Interface

Interface IRight
    Sub F()
End Interface

Class Test
    Implements ILeft, IRight

    Sub F() Implements ILeft.F, IRight.F
    End Sub
End Class
```

Wenn der implementierte Schnittstellenmember in allen explizit implementierten Schnittstellen aufgrund mehrerer Schnittstellen Vererbung nicht verfügbar ist, muss der implementierende Member explizit auf eine Basisschnittstelle verweisen, auf der der Member verfügbar ist. Wenn z. b. `I1` und `I2` einen Member `M` enthalten und `I3` von `I1` und `I2` erbt, implementiert ein Typ, der `I3` implementiert, `I1.M` und `I2.M`. Wenn eine Schnittstelle die Vererbung von geerbten Membern multipliziert, muss ein implementierender Typ die geerbten Member implementieren, und die Member (e) müssen Sie Shadowing durchführen.

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

Class Test
    Implements ILeftRight

    Sub LeftF() Implements ILeft.F
    End Sub

    Sub RightF() Implements IRight.F
    End Sub

    Sub LeftRightF() Implements ILeftRight.F
    End Sub
End Class
```

Wenn die enthaltende Schnittstelle des Schnittstellenmembers generisch ist, müssen die gleichen Typargumente wie die implementierte Schnittstelle bereitgestellt werden. Zum Beispiel:

```vb
Interface I1(Of T)
    Function F() As T
End Interface

Class C1
    Implements I1(Of Integer)
    Implements I1(Of Double)

    Function F1() As Integer Implements I1(Of Integer).F
    End Function

    Function F2() As Double Implements I1(Of Double).F
    End Function

    ' Error: I1(Of String) is not implemented by C1
    Function F3() As String Implements I1(Of String).F
    End Function
End Class

Class C2(Of U)
    Implements I1(Of U)

    Function F() As U Implements I1(Of U).F
    End Function
End Class
```


## <a name="methods"></a>Methoden

Methoden enthalten die ausführbaren Anweisungen eines Programms.

```antlr
MethodMemberDeclaration
    : MethodDeclaration
    | ExternalMethodDeclaration
    ;

InterfaceMethodMemberDeclaration
    : InterfaceMethodDeclaration
    ;

MethodDeclaration
    : SubDeclaration
    | MustOverrideSubDeclaration
    | FunctionDeclaration
    | MustOverrideFunctionDeclaration
    ;

InterfaceMethodDeclaration
    : InterfaceSubDeclaration
    | InterfaceFunctionDeclaration
    ;

SubSignature
    : 'Sub' Identifier TypeParameterList?
      ( OpenParenthesis ParameterList? CloseParenthesis )?
    ;

FunctionSignature
    : 'Function' Identifier TypeParameterList?
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )?
    ;

SubDeclaration
    : Attributes? ProcedureModifier* SubSignature
      HandlesOrImplements? LineTerminator
      Block
      'End' 'Sub' StatementTerminator
    ;

MustOverrideSubDeclaration
    : Attributes? MustOverrideProcedureModifier+ SubSignature
      HandlesOrImplements? StatementTerminator
    ;

InterfaceSubDeclaration
    : Attributes? InterfaceProcedureModifier* SubSignature StatementTerminator
    ;

FunctionDeclaration
    : Attributes? ProcedureModifier* FunctionSignature
      HandlesOrImplements? LineTerminator
      Block
      'End' 'Function' StatementTerminator
    ;

MustOverrideFunctionDeclaration
    : Attributes? MustOverrideProcedureModifier+ FunctionSignature
      HandlesOrImplements? StatementTerminator
    ;

InterfaceFunctionDeclaration
    : Attributes? InterfaceProcedureModifier* FunctionSignature StatementTerminator
    ;

ProcedureModifier
    : AccessModifier | 'Shadows' | 'Shared' | 'Overridable' | 'NotOverridable' | 'Overrides'
    | 'Overloads' | 'Partial' | 'Iterator' | 'Async'
    ;

MustOverrideProcedureModifier
    : ProcedureModifier
    | 'MustOverride'
    ;

InterfaceProcedureModifier
    : 'Shadows' | 'Overloads'
    ;

HandlesOrImplements
    : HandlesClause
    | ImplementsClause
    ;
```

Methoden, die eine optionale Liste von Parametern und einen optionalen Rückgabewert aufweisen, sind entweder freigegeben oder nicht freigegeben. Auf freigegebene Methoden wird über die-Klasse oder Instanzen der-Klasse zugegriffen. Auf nicht freigegebene Methoden, die auch als Instanzmethoden bezeichnet werden, erfolgt der Zugriff über Instanzen der-Klasse. Das folgende Beispiel zeigt eine Klasse `Stack` mit mehreren freigegebenen Methoden (`Clone` und `Flip`) und mehrere Instanzmethoden (`Push`, `Pop` und `ToString`):

```vb
Public Class Stack
    Public Shared Function Clone(s As Stack) As Stack
        ...
    End Function

    Public Shared Function Flip(s As Stack) As Stack
        ...
    End Function

    Public Function Pop() As Object
        ...
    End Function

    Public Sub Push(o As Object)
        ...
    End Sub 

    Public Overrides Function ToString() As String
        ...
    End Function 
End Class 

Module Test
    Sub Main()
        Dim s As Stack = New Stack()
        Dim i As Integer

        While i < 10
            s.Push(i)
        End While

        Dim flipped As Stack = Stack.Flip(s)
        Dim cloned As Stack = Stack.Clone(s)

        Console.WriteLine("Original stack: " & s.ToString())
        Console.WriteLine("Flipped stack: " & flipped.ToString())
        Console.WriteLine("Cloned stack: " & cloned.ToString())
    End Sub
End Module
```

Methoden können überladen werden, was bedeutet, dass mehrere Methoden denselben Namen haben können, solange Sie eindeutige Signaturen aufweisen. Die Signatur einer Methode besteht aus der Anzahl und den Typen ihrer Parameter. Die Signatur einer Methode schließt den Rückgabetyp oder die Parametermodifizierer wie "optional", "ByRef" oder "ParamArray" nicht ein. Das folgende Beispiel zeigt eine-Klasse mit einer Reihe von über Ladungen:

```vb
Module Test
    Sub F()
        Console.WriteLine("F()")
    End Sub 

    Sub F(o As Object)
        Console.WriteLine("F(Object)")
    End Sub

    Sub F(value As Integer)
        Console.WriteLine("F(Integer)")
    End Sub 

    Sub F(a As Integer, b As Integer)
        Console.WriteLine("F(Integer, Integer)")
    End Sub 

    Sub F(values() As Integer)
        Console.WriteLine("F(Integer())")
    End Sub 

    Sub G(s As String, Optional s2 As String = 5)
        Console.WriteLine("G(String, Optional String")
    End Sub

    Sub G(s As String)
        Console.WriteLine("G(String)")
    End Sub


    Sub Main()
        F()
        F(1)
        F(CType(1, Object))
        F(1, 2)
        F(New Integer() { 1, 2, 3 })
        G("hello")
        G("hello", "world")
    End Sub
End Module
```

Die Ausgabe des Programms lautet wie folgt:

```console
F()
F(Integer)
F(Object)
F(Integer, Integer)
F(Integer())
G(String)
G(String, Optional String)
```

Über Ladungen, die sich nur in optionalen Parametern unterscheiden, können für die Versionsverwaltung von Bibliotheken verwendet werden. V1 einer Bibliothek kann beispielsweise eine Funktion mit optionalen Parametern enthalten:

```vb
Sub fopen(fileName As String, Optional accessMode as Integer = 0)
```

Anschließend möchte v2 der Bibliothek einen weiteren optionalen Parameter "Password" hinzufügen. Dies ist ohne Unterbrechung der Quell Kompatibilität zu tun (sodass Anwendungen, für die v1 verwendet wird, neu kompiliert werden können) und ohne Unterbrechung der Binärkompatibilität (also Anwendungen, die Verweis v1 kann jetzt ohne Neukompilierung auf v2 verweisen). So sieht v2 aus:

```vb
Sub fopen(file As String, mode as Integer)
Sub fopen(file As String, Optional mode as Integer = 0, Optional pword As String = "")
```

Beachten Sie, dass optionale Parameter in einer öffentlichen API nicht CLS-kompatibel sind. Sie können jedoch zumindest von Visual Basic und c# 4 und F#verarbeitet werden.



### <a name="regular-async-and-iterator-method-declarations"></a>Reguläre, Async-und Iterator-Methoden Deklarationen

Es gibt zwei Arten von Methoden: *Unterroutinen*, die keine Werte zurückgeben, und *Funktionen*, die dies tun. Der Body-und `End`-Konstruktor einer Methode darf nur ausgelassen werden, wenn die Methode in einer Schnittstelle definiert ist oder den `MustOverride`-Modifizierer aufweist. Wenn für eine Funktion kein Rückgabetyp angegeben wird und eine strikte Semantik verwendet wird, tritt ein Kompilierzeitfehler auf. Andernfalls ist der Typ implizit `Object` oder der Typ des Typzeichens der Methode. Die Zugriffs Domäne des Rückgabe Typs und die Parametertypen einer Methode müssen mit oder einer übergeordneten Zugriffs Domäne der Methode selbst identisch sein.

Eine __reguläre Methode__ ist eine Methode, die weder `Async` noch `Iterator`-Modifizierer ist. Dabei kann es sich um eine Unterroutine oder eine Funktion handeln. Im Abschnitt [reguläre Methoden](statements.md#regular-methods) wird erläutert, was geschieht, wenn eine reguläre Methode aufgerufen wird.

Eine __Iteratormethode__ ist eine mit dem `Iterator`-Modifizierer und kein `Async`-Modifizierer. Er muss eine Funktion sein, und sein Rückgabetyp muss `IEnumerator`, `IEnumerable` oder `IEnumerator(Of T)` oder `IEnumerable(Of T)` für einige `T` sein, und er darf keine `ByRef`-Parameter aufweisen. Abschnitts [Iteratormethoden](statements.md#iterator-methods) gibt an, was geschieht, wenn eine Iteratormethode aufgerufen wird.

Eine __Async-Methode__ ist eine Methode mit dem `Async`-Modifizierer und kein `Iterator`-Modifizierer. Er muss entweder eine Unterroutine oder eine Funktion mit dem Rückgabetyp `Task` oder `Task(Of T)` für einige `T` sein und darf keine `ByRef`-Parameter aufweisen. Im Abschnitt [Async-Methoden](statements.md#async-methods) wird erläutert, was geschieht, wenn eine Async-Methode aufgerufen wird.

Es handelt sich um einen Kompilierzeitfehler, wenn eine Methode nicht eine dieser drei Arten von Methoden ist.

Unterroutine-und Funktions Deklarationen sind ein besonderes Zeichen dafür, dass Ihre Start-und End-Anweisungen jeweils am Anfang einer logischen Zeile beginnen müssen. Außerdem muss der Text einer nicht-`MustOverride`-Unterroutine oder Funktionsdeklaration am Anfang einer logischen Zeile beginnen. Zum Beispiel:

```vb
Module Test
    ' Illegal: Subroutine doesn't start the line
    Public x As Integer : Sub F() : End Sub

    ' Illegal: First statement doesn't start the line
    Sub G() : Console.WriteLine("G")
    End Sub

    ' Illegal: End Sub doesn't start the line
    Sub H() : End Sub
End Module
```


### <a name="external-method-declarations"></a>Externe Methoden Deklarationen

Eine externe Methoden Deklaration führt eine neue Methode ein, deren Implementierung außerhalb des Programms bereitgestellt wird.

```antlr
ExternalMethodDeclaration
    : ExternalSubDeclaration
    | ExternalFunctionDeclaration
    ;

ExternalSubDeclaration
    : Attributes? ExternalMethodModifier* 'Declare' CharsetModifier? 'Sub'
      Identifier LibraryClause AliasClause?
      ( OpenParenthesis ParameterList? CloseParenthesis )? StatementTerminator
    ;

ExternalFunctionDeclaration
    : Attributes? ExternalMethodModifier* 'Declare' CharsetModifier? 'Function'
      Identifier LibraryClause AliasClause?
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )?
      StatementTerminator
    ;

ExternalMethodModifier
    : AccessModifier
    | 'Shadows'
    | 'Overloads'
    ;

CharsetModifier
    : 'Ansi' | 'Unicode' | 'Auto'
    ;

LibraryClause
    : 'Lib' StringLiteral
    ;

AliasClause
    : 'Alias' StringLiteral
    ;
```

Da eine externe Methoden Deklaration keine tatsächliche Implementierung bereitstellt, verfügt sie über keinen Methoden Text oder `End`-Konstrukt. Externe Methoden werden implizit freigegeben, haben möglicherweise keine Typparameter und behandeln keine Ereignisse oder implementieren Schnittstellenmember. Wenn für eine Funktion kein Rückgabetyp angegeben wird und eine strikte Semantik verwendet wird, tritt ein Kompilierzeitfehler auf. Andernfalls ist der Typ implizit `Object` oder der Typ des Typzeichens der Methode. Die Zugriffs Domäne der Rückgabetyp-und Parametertypen einer externen Methode muss mit oder einer übergeordneten Zugriffs Domäne der externen Methode selbst identisch sein.

Die Library-Klausel einer externen Methoden Deklaration gibt den Namen der externen Datei an, die die-Methode implementiert. Die optionale Alias Klausel ist eine Zeichenfolge, die die numerische Ordinalzahl (mit dem Präfix "`#`") oder den Namen der Methode in der externen Datei angibt. Es kann auch ein Einzelzeichen-set-Modifizierer angegeben werden, der den Zeichensatz steuert, der beim Aufrufen der externen Methode zum Mars Hallen von Zeichen folgen verwendet wird. Der `Unicode`-Modifizierer Marshallen alle Zeichen folgen in Unicode-Werte, der `Ansi`-Modifizierer Marshallen alle Zeichen folgen in ANSI-Werte, und der `Auto`-Modifizierer Marshallen die Zeichen folgen gemäß .NET Framework Regeln basierend auf dem Namen der Methode oder dem Aliasnamen, falls angegeben. Wenn kein Modifizierer angegeben wird, ist der Standardwert `Ansi`.

Wenn `Ansi` oder `Unicode` angegeben ist, wird der Methodenname ohne Änderung in der externen Datei gesucht. Wenn `Auto` angegeben ist, hängt die Methodennamen Suche von der Plattform ab. Wenn die Plattform als ANSI (z. b. Windows 95, Windows 98, Windows Me) eingestuft wird, wird der Methodenname ohne Änderung gesucht. Wenn bei der Suche ein Fehler auftritt, wird eine `A` angefügt und die Suche erneut versucht. Wenn die Plattform als Unicode angesehen wird (z. b. Windows NT, Windows 2000, Windows XP), wird eine `W` angehängt und der Name gesucht. Wenn bei der Suche ein Fehler auftritt, wird die Suche ohne den `W` erneut versucht. Zum Beispiel:

```vb
Module Test
    ' All platforms bind to "ExternSub".
    Declare Ansi Sub ExternSub Lib "ExternDLL" ()

    ' All platforms bind to "ExternSub".
    Declare Unicode Sub ExternSub Lib "ExternDLL" ()

    ' ANSI platforms: bind to "ExternSub" then "ExternSubA".
    ' Unicode platforms: bind to "ExternSubW" then "ExternSub".
    Declare Auto Sub ExternSub Lib "ExternDLL" ()
End Module
```

Datentypen, die an externe Methoden übermittelt werden, werden gemäß den .NET Framework Datenmarshallingkonventionen mit einer Ausnahme gemarshallt. Zeichen folgen Variablen, die als Wert (d. h. `ByVal x As String`) übergebenen werden, werden an den OLE-automatisierungbstr-Typ gemarshallt, und Änderungen, die an BSTR in der externen Methode vorgenommen werden, werden im Zeichen folgen Argument wiedergegeben. Dies liegt daran, dass der Typ `String` in externen Methoden änderbar ist und diese besondere Marshalling dieses Verhalten imitiert. Zeichen folgen Parameter, die als Verweis (z. b. `ByRef x As String`) übergebenen werden, werden als Zeiger auf den Typ der OLE-Automatisierungs BSTR gemarshallt. Sie können diese speziellen Verhalten überschreiben, indem Sie das `System.Runtime.InteropServices.MarshalAsAttribute`-Attribut für den Parameter angeben.

Das Beispiel veranschaulicht die Verwendung externer Methoden:

```vb
Class Path
    Declare Function CreateDirectory Lib "kernel32" ( _
        Name As String, sa As SecurityAttributes) As Boolean
    Declare Function RemoveDirectory Lib "kernel32" ( _
        Name As String) As Boolean
    Declare Function GetCurrentDirectory Lib "kernel32" ( _
        BufSize As Integer, Buf As String) As Integer
    Declare Function SetCurrentDirectory Lib "kernel32" ( _
        Name As String) As Boolean
End Class
```


### <a name="overridable-methods"></a>Über schreibbare Methoden

Der `Overridable`-Modifizierer gibt an, dass eine Methode über schreibbar ist. Der `Overrides`-Modifizierer gibt an, dass eine Methode eine über schreibbare Methode des Basistyps mit der gleichen Signatur überschreibt. Der `NotOverridable`-Modifizierer gibt an, dass eine über schreibbare Methode nicht weiter überschrieben werden kann. Der `MustOverride`-Modifizierer gibt an, dass eine Methode in abgeleiteten Klassen überschrieben werden muss.

Bestimmte Kombinationen dieser Modifizierer sind ungültig:

* `Overridable` und `NotOverridable` schließen sich gegenseitig aus und können nicht kombiniert werden.

* `MustOverride` impliziert `Overridable` (und kann daher nicht angegeben werden) und kann nicht mit `NotOverridable` kombiniert werden.

* `NotOverridable` kann nicht mit `Overridable` oder `MustOverride` kombiniert werden und muss mit `Overrides` kombiniert werden.

* `Overrides` impliziert `Overridable` (und kann daher nicht angegeben werden) und kann nicht mit `MustOverride` kombiniert werden.

Außerdem gibt es zusätzliche Einschränkungen für über schreibbare Methoden:

* Eine `MustOverride`-Methode darf keinen Methoden Text oder ein `End`-Konstrukt enthalten, kann keine andere Methode überschreiben und darf nur in `MustInherit`-Klassen vorkommen.

* Wenn eine Methode `Overrides` angibt und keine übereinstimmende Basis Methode zum Überschreiben vorhanden ist, tritt ein Kompilierzeitfehler auf. Eine über schreibende Methode darf nicht `Shadows` angeben.

* Eine Methode kann möglicherweise keine andere Methode überschreiben, wenn die Barrierefreiheits Domäne der über schreibenden Methode nicht gleich der Zugriffs Domäne der Methode ist, die überschrieben wird. Die einzige Ausnahme ist, dass eine Methode, die eine `Protected Friend`-Methode in einer anderen Assembly überschreibt, die keinen `Friend`-Zugriff hat, `Protected` (nicht `Protected Friend`) angeben muss.

* `Private`-Methoden sind möglicherweise nicht `Overridable`, `NotOverridable` oder `MustOverride` und können auch andere Methoden überschreiben.

* Methoden in `NotInheritable`-Klassen dürfen nicht als `Overridable` oder `MustOverride` deklariert werden.

Das folgende Beispiel veranschaulicht die Unterschiede zwischen über schreibbaren und nicht über schreibbaren Methoden:

```vb
Class Base
    Public Sub F()
        Console.WriteLine("Base.F")
    End Sub

    Public Overridable Sub G()
        Console.WriteLine("Base.G")
    End Sub
End Class

Class Derived
    Inherits Base

    Public Shadows Sub F()
        Console.WriteLine("Derived.F")
    End Sub

    Public Overrides Sub G()
        Console.WriteLine("Derived.G")
    End Sub
End Class

Module Test
    Sub Main()
        Dim d As Derived = New Derived()
        Dim b As Base = d

        b.F()
        d.F()
        b.G()
        d.G()
    End Sub
End Module
```

Im Beispiel führt Class `Base` eine Methode ein `F` und eine `Overridable`-Methode `G`. Mit der-Klasse `Derived` wird eine neue Methode `F` eingeführt, wodurch die geerbte `F` shadoassen und außerdem die geerbte Methode `G` überschrieben wird. Das Beispiel führt zur folgenden Ausgabe:

```console
Base.F
Derived.F
Derived.G
Derived.G
```

Beachten Sie, dass die-Anweisung `b.G()` `Derived.G` aufruft, nicht `Base.G`. Dies liegt daran, dass der Lauf Zeittyp der-Instanz (die `Derived` ist) und nicht der Kompilier Zeittyp der-Instanz (`Base`) die tatsächliche Methoden Implementierung bestimmt, die aufgerufen werden soll.

### <a name="shared-methods"></a>Freigegebene Methoden

Der `Shared`-Modifizierer gibt an, dass eine Methode eine frei *gegebene Methode*ist. Eine freigegebene Methode funktioniert nicht für eine bestimmte Instanz eines Typs und kann direkt von einem Typ und nicht über eine bestimmte Instanz eines Typs aufgerufen werden. Es ist jedoch gültig, eine gemeinsame Methode mit einer-Instanz zu qualifizieren. Es ist ungültig, in einer freigegebenen Methode auf `Me`, `MyClass` oder `MyBase` zu verweisen. Freigegebene Methoden dürfen nicht `Overridable`, `NotOverridable` oder `MustOverride` sein, und Sie können keine Methoden überschreiben. In Standardmodulen und Schnittstellen definierte Methoden dürfen nicht `Shared` angeben, da Sie bereits implizit `Shared` sind.

Eine Methode, die in einer Struktur oder Klasse ohne `Shared`-Modifizierer deklariert wird, ist eine *Instanzmethode*. Eine Instanzmethode funktioniert für eine bestimmte Instanz eines Typs. Instanzmethoden können nur über eine Instanz eines Typs aufgerufen werden und können über den `Me`-Ausdruck auf die Instanz verweisen.

Das folgende Beispiel veranschaulicht die Regeln für den Zugriff auf freigegebene Member und Instanzmember:

```vb
Class Test
    Private x As Integer
    Private Shared y As Integer

    Sub F()
        x = 1 ' Ok, same as Me.x = 1.
        y = 1 ' Ok, same as Test.y = 1.
    End Sub

    Shared Sub G()
        x = 1 ' Error, cannot access Me.x.
        y = 1 ' Ok, same as Test.y = 1.
    End Sub

    Shared Sub Main()
        Dim t As Test = New Test()

        t.x = 1 ' Ok.
        t.y = 1 ' Ok.
        Test.x = 1 ' Error, cannot access instance member through type.
        Test.y = 1 ' Ok.
    End Sub
End Class
```

Die Methode `F` zeigt, dass ein Bezeichner in einem Instanzfunktionsmember für den Zugriff auf Instanzmember und freigegebene Member verwendet werden kann. Die Methode `G` zeigt, dass in einem freigegebenen Funktionsmember ein Fehler aufgetreten ist, um über einen Bezeichner auf einen Instanzmember zuzugreifen. Die Methode `Main` zeigt, dass in einem Member-Zugriffs Ausdruck auf Instanzmember über-Instanzen zugegriffen werden muss, auf freigegebene Member kann jedoch über Typen oder Instanzen zugegriffen werden.

### <a name="method-parameters"></a>Methodenparameter

Ein *Parameter* ist eine Variable, die verwendet werden kann, um Informationen an eine Methode zu übergeben. Parameter einer Methode werden durch die Parameterliste der Methode deklariert, die aus einem oder mehreren Parametern besteht, die durch Kommas getrennt sind.

```antlr
ParameterList
    : Parameter ( Comma Parameter )*
    ;

Parameter
    : Attributes? ParameterModifier* ParameterIdentifier ( 'As' TypeName )?
      ( Equals ConstantExpression )?
    ;

ParameterModifier
    : 'ByVal' | 'ByRef' | 'Optional' | 'ParamArray'
    ;

ParameterIdentifier
    : Identifier IdentifierModifiers
    ;
```

Wenn für einen Parameter kein Typ angegeben ist und eine strikte Semantik verwendet wird, tritt ein Kompilierzeitfehler auf. Andernfalls ist der Standardtyp `Object` oder der Typ des Typzeichens des Parameters. Selbst bei einer einschränkend sein Semantik müssen alle Parametertypen angeben, wenn ein Parameter eine `As`-Klausel enthält.

Parameter werden durch die Modifizierer `ByVal`, `ByRef`, `Optional` und `ParamArray` als Wert, Verweis, optional oder ParamArray-Parameter angegeben. Ein Parameter, der `ByRef` oder `ByVal` nicht angibt, ist standardmäßig `ByVal`.

Parameter Namen werden auf den gesamten Text der Methode festgelegt und sind immer öffentlich zugänglich. Ein Methodenaufruf erstellt eine Kopie, die für diesen Aufruf spezifisch ist, und die Argumentliste des aufzurufenden Parameters weist Werte oder Variablen Verweise den neu erstellten Parametern zu. Da externe Methoden Deklarationen und Delegatdeklarationen keinen Text aufweisen, sind doppelte Parameternamen in Parameterlisten zulässig, jedoch nicht empfehlenswert.

Auf den Bezeichner kann der namensmodifizierer, der NULL-Werte zulässt, `?`, um anzugeben, dass er NULL-Werte zulässt, und auch nach arraynamensmodifizierern, um anzugeben, dass es sich um ein Array Sie können kombiniert werden, z. b. "`ByVal x?() As Integer`". Es ist nicht zulässig, explizite Array Begrenzungen zu verwenden. auch wenn der Modifizierer "Werte zulässt Name" vorhanden ist, muss eine `As`-Klausel vorhanden sein.


#### <a name="value-parameters"></a>Wert Parameter

Ein *value-Parameter* wird mit einem expliziten `ByVal`-Modifizierer deklariert. Wenn der `ByVal`-Modifizierer verwendet wird, kann der `ByRef`-Modifizierer nicht angegeben werden. Ein value-Parameter wird mit dem Aufruf des Members, zu dem der Parameter gehört, vorhanden sein, und er wird mit dem Wert des im Aufruf angegebenen Arguments initialisiert. Ein value-Parameter ist bei der Rückgabe des Members nicht mehr vorhanden.

Eine Methode darf einem Wert Parameter neue Werte zuweisen. Solche Zuweisungen wirken sich nur auf den lokalen Speicherort aus, der durch den Wert Parameter dargestellt wird. Sie haben keine Auswirkung auf das tatsächliche Argument, das im Methodenaufruf angegeben ist.

Ein value-Parameter wird verwendet, wenn der Wert eines Arguments an eine Methode übergeben wird, und Änderungen des Parameters haben keine Auswirkung auf das ursprüngliche Argument. Ein value-Parameter verweist auf eine eigene Variable, die sich von der Variablen des entsprechenden Arguments unterscheidet. Diese Variable wird initialisiert, indem der Wert des entsprechenden Arguments kopiert wird. Das folgende Beispiel zeigt eine Methode `F` mit einem value-Parameter mit dem Namen `p`:

```vb
Module Test
    Sub F(p As Integer)
        Console.WriteLine("p = " & p)
        p += 1
    End Sub 

    Sub Main()
        Dim a As Integer = 1

        Console.WriteLine("pre: a = " & a)
        F(a)
        Console.WriteLine("post: a = " & a)
    End Sub
End Module
```

Im Beispiel wird die folgende Ausgabe erzeugt, auch wenn der value-Parameter `p` geändert wird:

```console
pre: a = 1
p = 1
post: a = 1
```

#### <a name="reference-parameters"></a>Verweis Parameter

Ein Verweis Parameter ist ein Parameter, der mit einem `ByRef`-Modifizierer deklariert wird. Wenn der `ByRef`-Modifizierer angegeben ist, kann der `ByVal`-Modifizierer nicht verwendet werden. Ein Verweis Parameter erstellt keinen neuen Speicherort. Stattdessen stellt ein Verweis Parameter die Variable dar, die als Argument in der Methode oder dem Konstruktoraufruf angegeben wird. Konzeptionell ist der Wert eines Verweis Parameters immer mit der zugrunde liegenden Variablen identisch.

Verweis Parameter agieren in zwei Modi, entweder als *Aliase* oder durch Kopieren von Kopier *Vorgängen.*

__Aliase.__ Ein Verweis Parameter wird verwendet, wenn der Parameter als Alias für ein vom Aufrufer bereitgestelltes Argument fungiert. Ein Verweis Parameter definiert nicht selbst eine Variable, sondern verweist stattdessen auf die Variable des entsprechenden Arguments. Änderungen eines Verweis Parameters direkt und wirken sich unmittelbar auf das entsprechende Argument aus. Das folgende Beispiel zeigt eine Methode `Swap` mit zwei Verweis Parametern:

```vb
Module Test
    Sub Swap(ByRef a As Integer, ByRef b As Integer)
        Dim t As Integer = a
        a = b
        b = t
    End Sub 

    Sub Main()
        Dim x As Integer = 1
        Dim y As Integer = 2

        Console.WriteLine("pre: x = " & x & ", y = " & y)
        Swap(x, y)
        Console.WriteLine("post: x = " & x & ", y = " & y)
    End Sub 
End Module
```

Die Ausgabe des Programms lautet wie folgt:

```console
pre: x = 1, y = 2
post: x = 2, y = 1
```

Für den Aufruf der-Methode `Swap` in der-Klasse `Main` stellt `a` `x,` und `b` `y` dar. Folglich hat der Aufruf die Auswirkungen, die Werte von `x` und `y` zu tauschen.

In einer Methode, die Verweis Parameter annimmt, ist es möglich, dass mehrere Namen denselben Speicherort darstellen:

```vb
Module Test
    Private s As String

    Sub F(ByRef a As String, ByRef b As String)
        s = "One"
        a = "Two"
        b = "Three"
    End Sub

    Sub G()
        F(s, s)
    End Sub
End Module
```

Im Beispiel übergibt der Aufruf der Methode `F` in `G` einen Verweis auf `s` für `a` und `b`. Daher verweisen die Namen `s`, `a` und `b` auf denselben Speicherort, und die drei Zuweisungen ändern alle die Instanzvariable `s`.

__Kopiervorgang kopieren.__ Wenn der Typ der Variablen, die an einen Verweis Parameter übergeben wird, nicht mit dem Typ des Verweis Parameters kompatibel ist oder wenn eine nicht-Variable (z. b. eine Eigenschaft) als Argument an einen Verweis Parameter übergeben wird, oder wenn der Aufruf spät gebunden ist, wird ein temporärer die Variable wird zugewiesen und an den Verweis Parameter übergeben. Der Wert, der in der Spalte verwendet wird, wird in diese temporäre Variable kopiert, bevor die-Methode aufgerufen wird. Sie wird zurück in die ursprüngliche Variable (sofern vorhanden, und wenn Sie beschreibbar ist) kopiert, wenn die-Methode zurückgegeben wird. Folglich kann ein Verweis Parameter nicht notwendigerweise einen Verweis auf die genaue Speicherung der übergebenen Variable enthalten, und alle Änderungen am Verweis Parameter werden möglicherweise erst in der Variablen widergespiegelt, wenn die Methode beendet wird. Zum Beispiel:

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class

Module Test
    Sub F(ByRef b As Base)
        b = New Base()
    End Sub

    Property G() As Base
        Get
        End Get
        Set
        End Set
    End Property

    Sub Main()
        Dim d As Derived

        F(G)   ' OK.
        F(d)   ' Throws System.InvalidCastException after F returns.
    End Sub
End Module
```

Beim ersten Aufruf von `F` wird eine temporäre Variable erstellt und der Wert der Eigenschaft `G` zugewiesen und an `F` geleitet. Bei der Rückgabe von `F` wird der Wert in der temporären Variablen der Eigenschaft `G` wieder zugewiesen. Im zweiten Fall wird eine weitere temporäre Variable erstellt und der Wert `d` zugewiesen und an `F` übermittelt. Bei der Rückgabe von `F` wird der Wert in der temporären Variablen zurück in den Typ der Variablen umgewandelt, `Derived` und `d` zugewiesen. Da der zurückgegebene Wert nicht in `Derived` umgewandelt werden kann, wird zur Laufzeit eine Ausnahme ausgelöst.

#### <a name="optional-parameters"></a>Optionale Parameter

Ein optionaler Parameter wird mit dem `Optional`-Modifizierer deklariert. Parameter, die auf einen optionalen Parameter in der Liste formaler Parameter folgen, müssen ebenfalls optional sein. Wenn Sie den `Optional`-Modifizierer für die folgenden Parameter nicht angeben, wird ein Kompilierzeitfehler ausgegeben. Ein optionaler Parameter eines Typs, der NULL-Werte zulässt, `T?` oder ein Typ, der keine NULL-Werte zulässt `T` muss einen konstanten Ausdruck `e` angeben, der als Standardwert verwendet werden soll, wenn kein Argument angegeben wird. Wenn `e` als `Nothing` vom Typ Object ausgewertet wird, wird der Standardwert des *Parameter Typs* als Standardwert für den Parameter verwendet. Andernfalls muss `CType(e, T)` ein konstanter Ausdruck sein, der als Standardwert für den Parameter verwendet wird.

Optionale Parameter sind die einzige Situation, in der ein Initialisierer für einen Parameter gültig ist. Die Initialisierung erfolgt immer als Teil des Aufruf Ausdrucks, nicht im Methoden Text selbst.

```vb
Module Test
    Sub F(x As Integer, Optional y As Integer = 20)
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub

    Sub Main()
        F(10)
        F(30,40)
    End Sub
End Module
```

Die Ausgabe des Programms lautet wie folgt:

```console
x = 10, y = 20
x = 30, y = 40
```

Optionale Parameter dürfen nicht in Delegaten oder Ereignis Deklarationen oder in Lambda Ausdrücken angegeben werden.

#### <a name="paramarray-parameters"></a>ParamArray-Parameter

`ParamArray`-Parameter werden mit dem `ParamArray`-Modifizierer deklariert. Wenn der `ParamArray`-Modifizierer vorhanden ist, muss der `ByVal`-Modifizierer angegeben werden, und kein anderer Parameter kann den `ParamArray`-Modifizierer verwenden. Der `ParamArray`-Parametertyp muss ein eindimensionales Array sein, und er muss der letzte Parameter in der Parameterliste sein.

Ein `ParamArray`-Parameter stellt eine unbestimmte Anzahl von Parametern des Typs der `ParamArray` dar. Innerhalb der Methode selbst wird der Parameter "`ParamArray`" als sein deklarierter Typ behandelt und hat keine besondere Semantik. Ein `ParamArray`-Parameter ist implizit optional, wobei der Standardwert ein leeres eindimensionales Array vom Typ des `ParamArray` ist.

Ein `ParamArray` ermöglicht das Angeben von Argumenten in einem Methodenaufruf auf eine von zwei Arten:

* Das für einen `ParamArray` angegebene Argument kann ein einzelner Ausdruck eines Typs sein, der zum `ParamArray`-Typ erweitert wird. In diesem Fall verhält sich die `ParamArray` genau wie ein value-Parameter.

* Alternativ kann der Aufruf NULL oder mehr Argumente für den `ParamArray` angeben, wobei jedes Argument ein Ausdruck eines Typs ist, der implizit in den Elementtyp von `ParamArray` konvertiert werden kann. In diesem Fall erstellt der Aufruf eine Instanz des `ParamArray`-Typs mit einer Länge, die der Anzahl der Argumente entspricht, initialisiert die Elemente der Array Instanz mit den angegebenen Argument Werten und verwendet die neu erstellte Array Instanz als die tatsächliche gestritten.

Mit der Ausnahme, dass eine Variable Anzahl von Argumenten in einem Aufruf zugelassen wird, entspricht ein `ParamArray` genau einem Wert Parameter desselben Typs, wie im folgenden Beispiel veranschaulicht.

```vb
Module Test
    Sub F(ParamArray args() As Integer)
        Dim i As Integer

        Console.Write("Array contains " & args.Length & " elements:")
        For Each i In args
            Console.Write(" " & i)
        Next i
        Console.WriteLine()
    End Sub

    Sub Main()
        Dim a As Integer() = { 1, 2, 3 }

        F(a)
        F(10, 20, 30, 40)
        F()
    End Sub
End Module
```

Das Beispiel erzeugt die Ausgabe.

```console
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

Beim ersten Aufruf von `F` wird das Array `a` einfach als Wert Parameter übergeben. Der zweite Aufruf von `F` erstellt automatisch ein Array mit vier Elementen mit den angegebenen Element Werten und übergibt diese Array Instanz als Wert Parameter. Ebenso erstellt der dritte Aufruf von `F` ein Array mit null Elementen und übergibt diese Instanz als Wert Parameter. Der zweite und der dritte Aufruf entsprechen genau dem Schreiben:

```vb
F(New Integer() {10, 20, 30, 40})
F(New Integer() {})
```

`ParamArray`-Parameter dürfen nicht in Delegaten-oder Ereignis Deklarationen angegeben werden.

### <a name="event-handling"></a>Ereignisbehandlung

Methoden können Ereignisse deklarativ verarbeiten, die von Objekten in einer Instanz oder freigegebenen Variablen ausgelöst werden. Zum Behandeln von Ereignissen gibt eine Methoden Deklaration das `Handles`-Schlüsselwort an und listet mindestens ein Ereignis auf.

```antlr
HandlesClause
    : ( 'Handles' EventHandlesList )?
    ;

EventHandlesList
    : EventMemberSpecifier ( Comma EventMemberSpecifier )*
    ;

EventMemberSpecifier
    : Identifier Period IdentifierOrKeyword
    | 'MyBase' Period IdentifierOrKeyword
    | 'MyClass' Period IdentifierOrKeyword
    | 'Me' Period IdentifierOrKeyword
    ;
```

Ein Ereignis in der `Handles`-Liste wird durch zwei Bezeichner angegeben, die durch einen bestimmten Zeitraum voneinander getrennt sind:

* Der erste Bezeichner muss eine Instanz oder eine freigegebene Variable im enthaltenden Typ sein, der den `WithEvents`-Modifizierer oder das `MyBase`-oder `MyClass`-oder `Me`-Schlüsselwort angibt. Andernfalls tritt ein Kompilierzeitfehler auf. Diese Variable enthält das-Objekt, das die von dieser Methode behandelten Ereignisse aufhebt.

* Der zweite Bezeichner muss einen Member vom Typ des ersten Bezeichners angeben. Der Member muss ein Ereignis sein, und er kann freigegeben sein. Wenn für den ersten Bezeichner eine freigegebene Variable angegeben wird, muss das Ereignis freigegeben werden, oder es muss ein Fehler auftreten.

Eine Handlermethode `M` wird als gültiger Ereignishandler für ein Ereignis `E` betrachtet, wenn die Anweisung `AddHandler E, AddressOf M` ebenfalls gültig wäre. Anders als bei einer `AddHandler`-Anweisung ermöglichen explizite Ereignishandler jedoch die Behandlung eines Ereignisses mit einer Methode ohne Argumente, unabhängig davon, ob strikte Semantik verwendet wird.

```vb
Option Strict On

Class C1
    Event E(x As Integer)
End Class

Class C2
    withEvents C1 As New C1()

    ' Valid
    Sub M1() Handles C1.E
    End Sub

    Sub M2()
        ' Invalid
        AddHandler C1.E, AddressOf M1
    End Sub
End Class
```

Ein einzelnes Element kann mehrere übereinstimmende Ereignisse verarbeiten, und mehrere Methoden können ein einzelnes Ereignis verarbeiten. Die Barrierefreiheit einer Methode hat keine Auswirkung auf die Möglichkeit, Ereignisse zu behandeln. Das folgende Beispiel zeigt, wie eine Methode Ereignisse behandeln kann:

```vb
Class Raiser
    Event E1()

    Sub Raise()
        RaiseEvent E1
    End Sub
End Class

Module Test
    WithEvents x As Raiser

    Sub E1Handler() Handles x.E1
        Console.WriteLine("Raised")
    End Sub

    Sub Main()
        x = New Raiser()
        x.Raise()
        x.Raise()
    End Sub
End Module
```

Dadurch wird Folgendes gedruckt:

```console
Raised
Raised
```

Ein Typ erbt alle Ereignishandler, die vom Basistyp bereitgestellt werden. Ein abgeleiteter Typ kann in keiner Weise die Ereignis Zuordnungen ändern, die er von seinen Basis Typen erbt, kann jedoch dem Ereignis weitere Handler hinzufügen.


### <a name="extension-methods"></a>Erweiterungsmethoden

Methoden können Typen von außerhalb der Typdeklaration mithilfe von *Erweiterungs Methoden*hinzugefügt werden. Erweiterungs Methoden sind Methoden, auf die das `System.Runtime.CompilerServices.ExtensionAttribute`-Attribut angewendet wird. Sie können nur in Standardmodulen deklariert werden und müssen über mindestens einen Parameter verfügen, der den Typ angibt, der von der Methode erweitert wird. Mit der folgenden Erweiterungsmethode wird z. b. der-Typ `String` erweitert:

```vb
Imports System.Runtime.CompilerServices

Module StringExtensions
    <Extension> _
    Sub Print(s As String)
        Console.WriteLine(s)
    End Sub
End Module
```

__Nebenbei.__ Obwohl Visual Basic Erweiterungs Methoden für die Deklaration in einem Standardmodul erfordert, C# können andere Sprachen, wie z. b., Sie in anderen Arten von Typen deklarieren. Solange die Methoden den hier beschriebenen anderen Konventionen folgen und der enthaltende Typ kein offener generischer Typ ist und nicht instanziiert werden kann, wird Visual Basic die Erweiterungs Methoden erkennen.

Wenn eine Erweiterungsmethode aufgerufen wird, wird die Instanz, für die Sie aufgerufen wird, an den ersten Parameter übergeben. Der erste Parameter kann nicht als "`Optional`" oder "`ParamArray`" deklariert werden. Jeder Typ, einschließlich eines Typparameters, kann als erster Parameter einer Erweiterungsmethode angezeigt werden. Mit den folgenden Methoden werden z. b. die Typen `Integer()`, jeder Typ, der `System.Collections.Generic.IEnumerable(Of T)` implementiert, und ein beliebiger Typ erweitert:

```vb
Imports System.Runtime.CompilerServices

Module Extensions
    <Extension> _
    Sub PrintArray(a() As Integer)
        ...
    End Sub

    <Extension> _
    Sub PrintList(Of T)(a As IEnumerable(Of T))
        ...
    End Sub

    <Extension> _
    Sub Print(Of T)(a As T)
        ...
    End Sub
End Module
```

Wie das vorherige Beispiel zeigt, können Schnittstellen erweitert werden. Schnittstellen Erweiterungs Methoden stellen die Implementierung der-Methode bereit, sodass Typen, die eine Schnittstelle implementieren, für die Erweiterungs Methoden definiert sind, immer noch die von der Schnittstelle deklarierten Member implementieren. Zum Beispiel:

```vb
Imports System.Runtime.CompilerServices

Interface IAction
  Sub DoAction()
End Interface

Module IActionExtensions 
    <Extension> _
    Public Sub DoAnotherAction(i As IAction) 
        i.DoAction()
    End Sub
End Module

Class C
  Implements IAction

  Sub DoAction() Implements IAction.DoAction
    ...
  End Sub

  ' ERROR: Cannot implement extension method IAction.DoAnotherAction
  Sub DoAnotherAction() Implements IAction.DoAnotherAction
    ...
  End Sub
End Class
```

Erweiterungs Methoden können auch Typeinschränkungen für ihre Typparameter aufweisen, und genau wie bei generischen Methoden ohne Erweiterung kann das Typargument abgeleitet werden:

```vb
Imports System.Runtime.CompilerServices

Module IEnumerableComparableExtensions
    <Extension> _
    Public Function Sort(Of T As IComparable(Of T))(i As IEnumerable(Of T)) _
        As IEnumerable(Of T)
        ...
    End Function
End Module
```

Auf Erweiterungs Methoden kann auch über implizite Instanzausdrücke innerhalb des erweiterten Typs zugegriffen werden:

```vb
Imports System.Runtime.CompilerServices

Class C1
    Sub M1()
        Me.M2()
        M2()
    End Sub
End Class

Module C1Extensions
    <Extension>
    Sub M2(c As C1)
        ...
    End Sub
End Module
```

Für den Zugriff auf die Barrierefreiheit werden Erweiterungs Methoden auch als Member des Standardmoduls behandelt, in dem Sie deklariert werden. Sie haben keinen zusätzlichen Zugriff auf die Member des Typs, den Sie über den Zugriff haben, den Sie aufgrund ihres Deklarations Kontexts haben.

Erweiterungs Methoden sind nur verfügbar, wenn sich die Standardmodul Methode im Gültigkeitsbereich befindet. Andernfalls wird der ursprüngliche Typ anscheinend nicht erweitert. Zum Beispiel:

```vb
Imports System.Runtime.CompilerServices

Class C1
End Class

Namespace N1
    Module C1Extensions
        <Extension> _
        Sub M1(c As C1)
            ...
        End Sub
    End Module
End Namespace

Module Test
    Sub Main()
        Dim c As New C1()

        ' Error: c has no member named "M1"
        c.M1()
    End Sub
End Module
```

Ein Verweis auf einen Typ, wenn nur eine Erweiterungsmethode für den Typ verfügbar ist, erzeugt trotzdem einen Kompilierzeitfehler.

Es ist wichtig zu beachten, dass Erweiterungs Methoden als Member des Typs in allen Kontexten angesehen werden, in denen Member gebunden werden, z. b. das stark typisierte `For Each`-Muster. Zum Beispiel:

```vb
Imports System.Runtime.CompilerServices

Class C1
End Class

Class C1Enumerator
    ReadOnly Property Current() As C1
        Get
            ...
        End Get
    End Property

    Function MoveNext() As Boolean
        ...
    End Function
End Class

Module C1Extensions
    <Extension> _
    Function GetEnumerator(c As C1) As C1Enumerator
        ...
    End Function
End Module

Module Test
    Sub Main()
        Dim c As New C1()

        ' Valid
        For Each o As Object In c
            ...
        Next o
    End Sub
End Module
```

Delegaten können auch erstellt werden, die auf Erweiterungs Methoden verweisen. Daher ist der Code:

```vb
Delegate Sub D1()

Module Test
    Sub Main()
        Dim s As String = "Hello, World!"
        Dim d As D1

        d = AddressOf s.Print
        d()
    End Sub
End Module
```

ist ungefähr Äquivalent zu:

```vb
Delegate Sub D1()

Module Test
    Sub Main()
      Dim s As String = "Hello, World!"
      Dim d As D1

      d = CType([Delegate].CreateDelegate(GetType(D1), s, _
                GetType(StringExtensions).GetMethod("Print")), D1)
      d()
    End Sub
End Module
```

__Nebenbei.__ In Visual Basic wird normalerweise eine Prüfung auf einen instanzmethodenaufruf eingefügt, der bewirkt, dass ein `System.NullReferenceException` auftritt, wenn die Instanz, für die die Methode aufgerufen wird, `Nothing` ist. Im Fall von Erweiterungs Methoden gibt es keine effiziente Möglichkeit, diese Überprüfung einzufügen, sodass Erweiterungs Methoden explizit nach `Nothing` suchen müssen. 

__Nebenbei.__ Ein Werttyp wird beim übergeben als `ByVal`-Argument an einen Parameter übergeben, der als Schnittstelle typisiert ist.  Dies impliziert, dass Nebeneffekte der Erweiterungsmethode auf eine Kopie der Struktur anstatt auf den ursprünglichen angewendet werden. Obwohl die Sprache keine Einschränkungen für das erste Argument einer Erweiterungsmethode enthält, wird empfohlen, dass Erweiterungs Methoden nicht zum Erweitern von Werttypen verwendet werden, oder dass beim Erweitern von Werttypen der erste Parameter `ByRef` übergeben wird, um die Nebeneffekte zu gewährleisten. Arbeiten Sie mit dem ursprünglichen Wert.

### <a name="partial-methods"></a>Partielle Methoden

Eine *partielle Methode* ist eine Methode, die eine Signatur, aber nicht den Text der Methode angibt. Der Text der Methode kann durch eine andere Methoden Deklaration mit demselben Namen und derselben Signatur angegeben werden, höchstwahrscheinlich in einer anderen partiellen Deklaration des Typs. Zum Beispiel:

a. vb:

```vb
' Designer generated code
Public Partial Class MyForm
    Private Partial Sub ValidateControls()
    End Sub

    Public Sub New()
        ' Initialize controls
        ...

        ValidateControls()
    End Sub    
End Class
```

b. vb:

```vb
Public Partial Class MyForm
    Public Sub ValidateControls()
        ' Validation logic goes here
        ...
    End Sub
End Class
```

In diesem Beispiel deklariert eine partielle Deklaration der-Klasse `MyForm` eine partielle Methode `ValidateControls` ohne Implementierung. Der Konstruktor in der partiellen Deklaration Ruft die partielle Methode auf, auch wenn kein Text in der Datei angegeben ist. Die andere partielle Deklaration von `MyForm` stellt dann die Implementierung der-Methode bereit.

Partielle Methoden können aufgerufen werden, unabhängig davon, ob ein Text bereitgestellt wurde. Wenn kein Methoden Text angegeben wird, wird der-Rückruf ignoriert. Zum Beispiel:

```vb
Public Class C1
    Private Partial Sub M1()
    End Sub

    Public Sub New()
        ' Since no implementation is supplied, this call will not be made.
        M1()
    End Sub
End Class
```

Alle Ausdrücke, die als Argumente an einen nicht ignorierten partiellen Methodenaufrufe übermittelt werden, werden ebenfalls ignoriert und nicht ausgewertet. (__Hinweis:__ Dies bedeutet, dass partielle Methoden eine sehr effiziente Methode zur Bereitstellung von Verhalten sind, das über zwei partielle Typen definiert ist, da die partiellen Methoden keine Kosten haben, wenn Sie nicht verwendet werden.)

Die Deklaration der partiellen Methode muss als `Private` deklariert werden und muss immer eine Unterroutine sein, ohne dass Anweisungen im Textkörper angezeigt werden. Partielle Methoden können nicht selbst Schnittstellen Methoden implementieren, obwohl die Methode, die Ihren Text bereitstellt, möglich ist.

Nur eine Methode kann einen Text für eine partielle Methode bereitstellen. Eine Methode, die einen Text für eine partielle Methode bereitstellt, muss die gleiche Signatur wie die partielle Methode, dieselben Einschränkungen für alle Typparameter, dieselben deklarationmodifizierer und die gleichen Parameter-und Typparameter Namen aufweisen. Attribute der partiellen Methode und die Methode, die Ihren Text bereitstellt, werden zusammengeführt, ebenso wie alle Attribute der Methoden Parameter. Ebenso wird die Liste der Ereignisse, die von den Methoden behandelt werden, zusammengeführt. Zum Beispiel:

```vb
Class C1
    Event E1()
    Event E2()

    Private Partial Sub S() Handles Me.E1
    End Sub

    ' Handles both E1 and E2
    Private Sub S() Handles Me.E2
        ...
    End Sub
End Class
```

## <a name="constructors"></a>Konstruktoren

*Konstruktoren* sind spezielle Methoden, die die Kontrolle über die Initialisierung ermöglichen. Sie werden ausgeführt, nachdem das Programm begonnen oder eine Instanz eines Typs erstellt wurde. Im Gegensatz zu anderen Membern werden Konstruktoren nicht geerbt, und es wird kein Name in den Deklarations Bereich eines Typs eingeführt. Konstruktoren können nur von Ausdrücken zum Erstellen von Objekten oder vom .NET Framework aufgerufen werden. Sie werden möglicherweise nie direkt aufgerufen.

__Nebenbei.__ Konstruktoren haben bei der Zeilen Platzierung die gleiche Einschränkung wie bei den Unterroutinen. Die beginnende Anweisung, End-Anweisung und Block müssen alle am Anfang einer logischen Zeile angezeigt werden.

```antlr
ConstructorMemberDeclaration
    : Attributes? ConstructorModifier* 'Sub' 'New'
      ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block?
      'End' 'Sub' StatementTerminator
    ;

ConstructorModifier
    : AccessModifier
    | 'Shared'
    ;
```

### <a name="instance-constructors"></a>Instanzkonstruktoren

*Instanzkonstruktoren* initialisieren Instanzen eines Typs und werden von der .NET Framework ausgeführt, wenn eine Instanz erstellt wird. Die Parameterliste eines Konstruktors unterliegt den gleichen Regeln wie die Parameterliste einer Methode. Instanzkonstruktoren können überladen werden.

Alle Konstruktoren in Verweis Typen müssen einen anderen Konstruktor aufrufen. Wenn der Aufruf explizit ist, muss es sich um die erste Anweisung im konstruktormethodentext handeln. Die Anweisung kann entweder einen anderen der Instanzkonstruktoren des Typs aufrufen (z. b. "`Me.New(...)`" oder "`MyClass.New(...)`"), oder wenn es sich nicht um eine Struktur handelt, kann er einen Instanzkonstruktor des Basistyps des Typs aufrufen (z. b. `MyBase.New(...)`). Ein Konstruktor kann sich nicht selbst aufrufen. Wenn ein Konstruktor einen anderen Konstruktor aufruft, ist `MyBase.New()` implizit. Wenn kein Parameter loser Basistyp Konstruktor vorhanden ist, tritt ein Kompilierzeitfehler auf. Da `Me` erst nach dem Aufruf eines Basisklassenkonstruktors als konstruiert betrachtet wird, können die Parameter für eine Konstruktoraufruf-Anweisung weder implizit noch explizit auf `Me`, `MyClass` oder `MyBase` verweisen.

Wenn die erste Anweisung eines Konstruktors die Form `MyBase.New(...)` hat, führt der Konstruktor implizit die Initialisierungen aus, die von den Variableninitialisierern der Instanzvariablen angegeben werden, die im-Typ deklariert werden. Dies entspricht einer Sequenz von Zuweisungen, die unmittelbar nach dem Aufrufen des Konstruktors des direkten Basistyps ausgeführt werden. Diese Reihenfolge stellt sicher, dass alle basisinstanzvariablen durch ihre Variableninitialisierer initialisiert werden, bevor Anweisungen ausgeführt werden, die auf die Instanz zugreifen können. Zum Beispiel:

```vb
Class A
    Protected x As Integer = 1
End Class

Class B
    Inherits A

    Private y As Integer = x

    Public Sub New()
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub
End Class
```

Wenn `New B()` zum Erstellen einer Instanz von `B` verwendet wird, wird die folgende Ausgabe erzeugt:

```console
x = 1, y = 1
```

Der Wert von `y` ist `1`, da der Variableninitialisierer ausgeführt wird, nachdem der Basisklassenkonstruktor aufgerufen wurde. Variableninitialisierer werden in der Text Reihenfolge ausgeführt, in der Sie in der Typdeklaration angezeigt werden.

Wenn ein Typ nur `Private`-Konstruktoren deklariert, ist es im Allgemeinen nicht möglich, dass andere Typen vom Typ abgeleitet werden, oder dass Instanzen des Typs erstellt werden. die einzige Ausnahme sind Typen, die innerhalb des Typs geschachtelt sind. `Private`-Konstruktoren werden häufig in Typen verwendet, die nur `Shared`-Member enthalten.

Wenn ein Typ keine Instanzkonstruktordeklarationen enthält, wird automatisch ein Standardkonstruktor bereitgestellt. Der Standardkonstruktor ruft einfach den Parameter losen Konstruktor des direkten Basistyps auf. Wenn der direkte Basistyp keinen zugänglichen Parameter losen Konstruktor hat, tritt ein Kompilierzeitfehler auf. Der deklarierte Zugriffstyp für den Standardkonstruktor ist `Public`, es sei denn, der Typ ist `MustInherit`. in diesem Fall ist der Standardkonstruktor `Protected`.

__Nebenbei.__ Der Standard Zugriff für den Standardkonstruktor eines `MustInherit`-Typs ist `Protected`, da `MustInherit`-Klassen nicht direkt erstellt werden können. Es gibt also keinen Punkt, an dem der Standardkonstruktor `Public` ist.

Im folgenden Beispiel wird ein Standardkonstruktor bereitgestellt, da die-Klasse keine Konstruktordeklarationen enthält:

```vb
Class Message
    Dim sender As Object
    Dim text As String
End Class
```

Daher entspricht das Beispiel genau folgendem:

```vb
Class Message
    Dim sender As Object
    Dim text As String

    Sub New()
    End Sub
End Class
```

Standardkonstruktoren, die in eine vom Designer generierte Klasse ausgegeben werden, die mit dem-Attribut `Microsoft.VisualBasic.CompilerServices.DesignerGeneratedAttribute` gekennzeichnet ist, werden die-Methode `Sub InitializeComponent()`, sofern vorhanden, nach dem-Aufrufer des basiskonstruktors aufruft. (__Hinweis:__ Dadurch können vom Designer generierte Dateien, z. b. die vom WinForms-Designer erstellten Dateien, den Konstruktor in der Designer Datei weglassen. Dies ermöglicht es dem Programmierer, ihn selbst anzugeben, wenn dies der Fall ist.)

### <a name="shared-constructors"></a>Freigegebene Konstruktoren

Frei *gegebene Konstruktoren* initialisieren die freigegebenen Variablen eines Typs. Sie werden ausgeführt, nachdem die Ausführung des Programms begonnen hat, jedoch vor allen Verweisen auf einen Member des Typs. Ein frei gegebener Konstruktor gibt den `Shared`-Modifizierer an, es sei denn, er befindet sich in einem Standardmodul. in diesem Fall wird der Modifizierer `Shared` impliziert.

Anders als Instanzkonstruktoren haben gemeinsam genutzte Konstruktoren impliziten öffentlichen Zugriff, haben keine Parameter und können keine anderen Konstruktoren aufrufen. Vor der ersten Anweisung in einem freigegebenen Konstruktor führt der freigegebene Konstruktor implizit die Initialisierungen aus, die von den Variableninitialisierern der im-Typ deklarierten freigegebenen Variablen angegeben werden. Dies entspricht einer Sequenz von Zuweisungen, die unmittelbar nach dem Eintrag in den Konstruktor ausgeführt werden. Die Variableninitialisierer werden in der Text Reihenfolge ausgeführt, in der Sie in der Typdeklaration angezeigt werden.

Das folgende Beispiel zeigt eine `Employee`-Klasse mit einem freigegebenen Konstruktor, der eine freigegebene Variable initialisiert:

```vb
Imports System.Data

Class Employee
    Private Shared ds As DataSet

    Shared Sub New()
        ds = New DataSet()
    End Sub

    Public Name As String
    Public Salary As Decimal
End Class
```

Ein separater frei gegebener Konstruktor ist für jeden geschlossenen generischen Typ vorhanden. Da der freigegebene Konstruktor für jeden geschlossenen Typ genau einmal ausgeführt wird, können Sie Laufzeitüberprüfungen für den Typparameter erzwingen, der zur Kompilierzeit nicht über Einschränkungen überprüft werden kann. Der folgende Typ verwendet beispielsweise einen freigegebenen Konstruktor, um zu erzwingen, dass der Typparameter `Integer` oder `Double` ist:

```vb
Class EnumHolder(Of T)
    Shared Sub New() 
        If Not GetType(T).IsEnum() Then
            Throw New ArgumentException("T must be an enumerated type.")
        End If
    End Sub
End Class
```

Genau, wenn freigegebene Konstruktoren ausgeführt werden, ist dies größtenteils implementierungsabhängig, obwohl mehrere Garantien bereitgestellt werden, wenn ein gemeinsam genutzter Konstruktor explizit definiert ist:

* Freigegebene Konstruktoren werden vor dem ersten Zugriff auf ein statisches Feld des Typs ausgeführt.

* Freigegebene Konstruktoren werden vor dem ersten Aufruf einer statischen Methode des Typs ausgeführt.

* Freigegebene Konstruktoren werden vor dem ersten Aufruf eines Konstruktors für den Typ ausgeführt.

Die oben aufgeführten Garantien gelten nicht für die Situation, in der ein gemeinsam genutzter Konstruktor implizit für freigegebene Initialisierer erstellt wird. Die Ausgabe des folgenden Beispiels ist unsicher, da die genaue Reihenfolge des Ladens und somit der freigegebenen Konstruktorausführung nicht definiert ist:

```vb
Module Test
    Sub Main()
        A.F()
        B.F()
    End Sub
End Module

Class A
    Shared Sub New()
        Console.WriteLine("Init A")
    End Sub

    Public Shared Sub F()
        Console.WriteLine("A.F")
    End Sub
End Class

Class B
    Shared Sub New()
        Console.WriteLine("Init B")
    End Sub

    Public Shared Sub F()
        Console.WriteLine("B.F")
    End Sub
End Class
```

Die Ausgabe kann eine der folgenden sein:

```console
Init A
A.F
Init B
B.F
```

oder

```console
Init B
Init A
A.F
B.F
```

Im Gegensatz dazu wird im folgenden Beispiel eine vorhersagbare Ausgabe erzeugt. Beachten Sie, dass der `Shared`-Konstruktor für die Klasse `A` nie ausgeführt wird, auch wenn die Klasse `B` davon abgeleitet ist:

```vb
Module Test
    Sub Main()
        B.G()
    End Sub
End Module

Class A
    Shared Sub New()
        Console.WriteLine("Init A")
    End Sub
End Class

Class B
    Inherits A

    Shared Sub New()
        Console.WriteLine("Init B")
    End Sub

    Public Shared Sub G()
        Console.WriteLine("B.G")
    End Sub
End Class
```

Ausgabe:

```console
Init B
B.G
```

Es ist auch möglich, zirkuläre Abhängigkeiten zu erstellen, die es ermöglichen, dass `Shared`-Variablen mit Variableninitialisierern im Standardwert Zustand beobachtet werden, wie im folgenden Beispiel gezeigt:

```vb
Class A
    Public Shared X As Integer = B.Y + 1
End Class

Class B
    Public Shared Y As Integer = A.X + 1

    Shared Sub Main()
        Console.WriteLine("X = " & A.X & ", Y = " & B.Y)
    End Sub
End Class
```

Dies erzeugt die Ausgabe:

```console
X = 1, Y = 2
```

Um die `Main`-Methode auszuführen, lädt das System zuerst die Klasse `B`. Der `Shared`-Konstruktor der Klasse `B` berechnet den Anfangswert von `Y`, der rekursiv bewirkt, dass die Klasse `A` geladen wird, da auf den Wert von `A.X` verwiesen wird. Der `Shared`-Konstruktor der Klasse `A` bewirkt, dass der Anfangswert von `X` berechnet wird. Dadurch wird der *Standard* Wert `Y` abgerufen, der 0 (null) ist. `A.X` wird daher mit `1` initialisiert. Der Ladevorgang von `A` ist abgeschlossen, wobei die Berechnung des Anfangs Werts von `Y` zurückgegeben wird, dessen Ergebnis `2` ist.

Hätte sich die `Main`-Methode stattdessen in der Klasse `A` befunden, hätte das Beispiel die folgende Ausgabe erzeugt:

```console
X = 2, Y = 1
```

Vermeiden Sie zirkuläre Verweise in `Shared`-Variableninitialisierern, da es im Allgemeinen nicht möglich ist, die Reihenfolge zu bestimmen, in der Klassen mit solchen Verweisen geladen werden

## <a name="events"></a>Ereignisse

Ereignisse werden verwendet, um Code über ein bestimmtes Vorkommen zu benachrichtigen. Eine Ereignis Deklaration besteht aus einem Bezeichner, entweder einem Delegattyp oder einer Parameterliste, und einer optionalen `Implements`-Klausel.

```antlr
EventMemberDeclaration
    : RegularEventMemberDeclaration
    | CustomEventMemberDeclaration
    ;

RegularEventMemberDeclaration
    : Attributes? EventModifiers* 'Event'
      Identifier ParametersOrType ImplementsClause? StatementTerminator
    ;

InterfaceEventMemberDeclaration
    : Attributes? InterfaceEventModifiers* 'Event'
      Identifier ParametersOrType StatementTerminator
    ;

ParametersOrType
    : ( OpenParenthesis ParameterList? CloseParenthesis )?
    | 'As' NonArrayTypeName
    ;

EventModifiers
    : AccessModifier
    | 'Shadows'
    | 'Shared'
    ;

InterfaceEventModifiers
    : 'Shadows'
    ;
```

Wenn ein Delegattyp angegeben wird, weist der Delegattyp möglicherweise keinen Rückgabetyp auf. Wenn eine Parameterliste angegeben wird, enthält Sie möglicherweise keine `Optional`-oder `ParamArray`-Parameter. Die Zugriffs Domäne der Parametertypen und/oder des Delegattyps muss mit der Zugriffs Domäne des Ereignisses identisch sein oder eine übergeordnete Gruppe sein. Ereignisse können durch Angabe des `Shared`-Modifizierers freigegeben werden.

Zusätzlich zu dem Elementnamen, der dem Deklarations Bereich des Typs hinzugefügt wird, werden von einer Ereignis Deklaration implizit mehrere andere Member deklariert. Wenn ein Ereignis mit dem Namen "`X`" angegeben wird, werden dem Deklarations Raum die folgenden Elemente hinzugefügt:

* Wenn das Formular der Deklaration eine Methoden Deklaration ist, wird eine eingefügte Delegatklasse mit dem Namen "`XEventHandler`" eingeführt. Die Klassen für den eingefügten Delegaten stimmen mit der Methoden Deklaration überein und haben denselben Zugriff wie das Ereignis. Die Attribute in der Parameterliste gelten für die Parameter der Delegatklasse.

* Eine `Private`-Instanzvariable, die als Delegat mit dem Namen `XEvent` typisiert ist.

* Zwei Methoden namens "`add_X`" und "`remove_X`", die nicht aufgerufen, überschrieben oder überladen werden können.

Wenn ein Typ versucht, einen Namen zu deklarieren, der mit einem der obigen Namen übereinstimmt, führt dies zu einem Kompilierzeitfehler, und die impliziten `add_X`-und `remove_X`-Deklarationen werden für den Zweck der namens Bindung ignoriert. Es ist nicht möglich, die eingeführten Member außer Kraft zu setzen oder zu überladen, obwohl es möglich ist, Sie in abgeleiteten Typen zu schattieren. Beispielsweise die Klassen Deklaration

```vb
Class Raiser
    Public Event Constructed(i As Integer)
End Class
```

entspricht der folgenden Deklaration

```vb
Class Raiser
    Public Delegate Sub ConstructedEventHandler(i As Integer)

    Protected ConstructedEvent As ConstructedEventHandler

    Public Sub add_Constructed(d As ConstructedEventHandler)
        ConstructedEvent = _
            CType( _
                [Delegate].Combine(ConstructedEvent, d), _
                    Raiser.ConstructedEventHandler)
    End Sub

    Public Sub remove_Constructed(d As ConstructedEventHandler)
        ConstructedEvent = _
            CType( _
                [Delegate].Remove(ConstructedEvent, d), _
                    Raiser.ConstructedEventHandler)
    End Sub
End Class
```

Das Deklarieren eines Ereignisses ohne Angabe eines Delegattyps ist die einfachste und kompakteste Syntax, hat aber den Nachteil, dass für jedes Ereignis ein neuer Delegattyp deklariert wird. Im folgenden Beispiel werden z. b. drei verborgene Delegattypen erstellt, auch wenn alle drei Ereignisse dieselbe Parameterliste aufweisen:

```vb
Public Class Button
    Public Event Click(sender As Object, e As EventArgs)
    Public Event DoubleClick(sender As Object, e As EventArgs)
    Public Event RightClick(sender As Object, e As EventArgs)
End Class
```

Im folgenden Beispiel verwenden die-Ereignisse einfach denselben Delegaten, `EventHandler`:

```vb
Public Delegate Sub EventHandler(sender As Object, e As EventArgs)

Public Class Button
    Public Event Click As EventHandler
    Public Event DoubleClick As EventHandler
    Public Event RightClick As EventHandler
End Class
```

Ereignisse können auf eine von zwei Arten behandelt werden: statisch oder dynamisch. Statisch Behandlungsereignisse sind einfacher und benötigen nur eine `WithEvents`-Variable und eine `Handles`-Klausel. Im folgenden Beispiel behandelt Class `Form1` das-Ereignis statisch `Click` des-Objekts `Button`:

```vb
Public Class Form1
    Public WithEvents Button1 As New Button()

    Public Sub Button1_Click(sender As Object, e As EventArgs) _
           Handles Button1.Click
        Console.WriteLine("Button1 was clicked!")
    End Sub
End Class
```

Dynamisch Behandlungsereignisse sind komplexer, da das Ereignis explizit verbunden werden muss und in Code getrennt werden muss. Die-Anweisung `AddHandler` fügt einen Handler für ein Ereignis hinzu, und die-Anweisung `RemoveHandler` entfernt einen Handler für ein Ereignis. Das nächste Beispiel zeigt eine Klasse `Form1`, die `Button1_Click` als Ereignishandler für das `Click`-Ereignis von `Button1` hinzufügt:

```vb
Public Class Form1
    Public Sub New()
        ' Add Button1_Click as an event handler for Button1's Click event.
        AddHandler Button1.Click, AddressOf Button1_Click
    End Sub 

    Private Button1 As Button = New Button()

    Sub Button1_Click(sender As Object, e As EventArgs)
        Console.WriteLine("Button1 was clicked!")
    End Sub

    Public Sub Disconnect()
        RemoveHandler Button1.Click, AddressOf Button1_Click
    End Sub 
End Class
```

In der-Methode `Disconnect` wird der Ereignishandler entfernt.


### <a name="custom-events"></a>Benutzerdefinierte Ereignisse

Wie bereits im vorherigen Abschnitt erläutert, definieren Ereignis Deklarationen implizit ein Feld, eine `add_`-Methode und eine `remove_`-Methode, die zum Nachverfolgen von Ereignis Handlern verwendet wird. In einigen Situationen kann es jedoch wünschenswert sein, benutzerdefinierten Code zum Nachverfolgen von Ereignis Handlern bereitzustellen. Wenn eine Klasse z. b. 40-Ereignisse definiert, von denen nur wenige behandelt werden, kann die Verwendung einer Hash Tabelle anstelle von 40 Feldern, um die Handler für jedes Ereignis zu verfolgen, effizienter sein. *Benutzerdefinierte Ereignisse* ermöglichen das explizite Definieren der `add_X`-Methode und der `remove_X`-Methode, wodurch benutzerdefinierter Speicher für Ereignishandler aktiviert wird.

Benutzerdefinierte Ereignisse werden auf die gleiche Weise deklariert, wie Ereignisse, die einen Delegattyp angeben, deklariert werden, mit der Ausnahme, dass das Schlüsselwort `Custom` dem Schlüsselwort `Event` vorangestellt sein muss. Eine benutzerdefinierte Ereignis Deklaration enthält drei Deklarationen: eine `AddHandler`-Deklaration, eine `RemoveHandler`-Deklaration und eine `RaiseEvent`-Deklaration. Keine der Deklarationen kann einen Modifizierer aufweisen, obwohl Sie über Attribute verfügen können.

```antlr
CustomEventMemberDeclaration
    : Attributes? EventModifiers* 'Custom' 'Event'
      Identifier 'As' TypeName ImplementsClause? StatementTerminator
      EventAccessorDeclaration+
      'End' 'Event' StatementTerminator
    ;

EventAccessorDeclaration
    : AddHandlerDeclaration
    | RemoveHandlerDeclaration
    | RaiseEventDeclaration
    ;

AddHandlerDeclaration
    : Attributes? 'AddHandler'
      OpenParenthesis ParameterList CloseParenthesis LineTerminator
      Block?
      'End' 'AddHandler' StatementTerminator
    ;

RemoveHandlerDeclaration
    : Attributes? 'RemoveHandler'
      OpenParenthesis ParameterList CloseParenthesis LineTerminator
      Block?
      'End' 'RemoveHandler' StatementTerminator
    ;

RaiseEventDeclaration
    : Attributes? 'RaiseEvent'
      OpenParenthesis ParameterList CloseParenthesis LineTerminator
      Block?
      'End' 'RaiseEvent' StatementTerminator
    ;
```

Zum Beispiel:

```vb
Class Test
    Private Handlers As EventHandler

    Public Custom Event TestEvent As EventHandler
        AddHandler(value As EventHandler)
            Handlers = CType([Delegate].Combine(Handlers, value), _
                EventHandler)
        End AddHandler

        RemoveHandler(value as EventHandler)
            Handlers = CType([Delegate].Remove(Handlers, value), _
                EventHandler)
        End RemoveHandler

        RaiseEvent(sender As Object, e As EventArgs)
            Dim TempHandlers As EventHandler = Handlers

            If TempHandlers IsNot Nothing Then
                TempHandlers(sender, e)
            End If
        End RaiseEvent
    End Event
End Class
```

Die `AddHandler`-und `RemoveHandler`-Deklaration nehmen einen `ByVal`-Parameter an, der den Delegattyp des Ereignisses aufweisen muss. Wenn eine `AddHandler`-oder `RemoveHandler`-Anweisung ausgeführt wird (oder eine `Handles`-Klausel automatisch ein Ereignis behandelt), wird die entsprechende Deklaration aufgerufen. Die `RaiseEvent`-Deklaration übernimmt dieselben Parameter wie der Ereignis Delegat und wird aufgerufen, wenn eine `RaiseEvent`-Anweisung ausgeführt wird. Alle Deklarationen müssen bereitgestellt werden und als Unterroutinen angesehen werden.

Beachten Sie, dass die Deklarationen "`AddHandler`", "`RemoveHandler`" und "`RaiseEvent`" die gleiche Einschränkung in der Zeilen Platzierung aufweisen, die unter Die beginnende Anweisung, End-Anweisung und Block müssen alle am Anfang einer logischen Zeile angezeigt werden.

Zusätzlich zu dem Elementnamen, der dem Deklarations Bereich des Typs hinzugefügt wird, deklariert eine benutzerdefinierte Ereignis Deklaration implizit mehrere andere Member. Wenn ein Ereignis mit dem Namen "`X`" angegeben wird, werden dem Deklarations Raum die folgenden Elemente hinzugefügt:

* Eine Methode mit dem Namen "`add_X`", die der `AddHandler`-Deklaration entspricht.

* Eine Methode mit dem Namen "`remove_X`", die der `RemoveHandler`-Deklaration entspricht.

* Eine Methode mit dem Namen "`fire_X`", die der `RaiseEvent`-Deklaration entspricht.

Wenn ein Typ versucht, einen Namen zu deklarieren, der mit einem der obigen Namen übereinstimmt, führt dies zu einem Kompilierzeitfehler, und die impliziten Deklarationen werden für die namens Bindung ignoriert. Es ist nicht möglich, die eingeführten Member außer Kraft zu setzen oder zu überladen, obwohl es möglich ist, Sie in abgeleiteten Typen zu schattieren.

__Nebenbei.__ `Custom` ist kein reserviertes Wort.

#### <a name="custom-events-in-winrt-assemblies"></a>Benutzerdefinierte Ereignisse in WinRT-Assemblys

Ab Microsoft Visual Basic 11,0 werden Ereignisse, die in einer Datei deklariert sind, die mit `/target:winmdobj` kompiliert wurde, oder in einer Schnittstelle in einer solchen Datei deklariert und dann an anderer Stelle implementiert, etwas anders behandelt.

* Externe Tools, die zum Erstellen von winmd verwendet werden, gestatten in der Regel nur bestimmte Delegattypen, z. b. `System.EventHandler(Of T)` oder `System.TypedEventHandle(Of T, U)`, und andere werden nicht zugelassen.

* Das Feld "`XEvent`" weist den Typ `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationTokenTable(Of T)` auf, wobei "`T`" der Delegattyp ist.

* Der AddHandler-Accessor gibt einen `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationToken` zurück, und der RemoveHandler-Accessor nimmt einen einzelnen Parameter desselben Typs an.

Im folgenden finden Sie ein Beispiel für ein solches benutzerdefiniertes Ereignis.

```vb
Imports System.Runtime.InteropServices.WindowsRuntime

Public NotInheritable Class ClassInWinMD
    Private XEvent As EventRegistrationTokenTable(Of EventHandler(Of Integer))

    Public Custom Event X As EventHandler(Of Integer)
        AddHandler(handler As EventHandler(Of Integer))
            Return EventRegistrationTokenTable(Of EventHandler(Of Integer)).
                   GetOrCreateEventRegistrationTokenTable(XEvent).
                   AddEventHandler(handler)
        End AddHandler

        RemoveHandler(token As EventRegistrationToken)
            EventRegistrationTokenTable(Of EventHandler(Of Integer)).
                GetOrCreateEventRegistrationTokenTable(XEvent).
                RemoveEventHandler(token)
        End RemoveHandler

        RaiseEvent(sender As Object, i As Integer)
            Dim table = EventRegistrationTokenTable(Of EventHandler(Of Integer)).
                GetOrCreateEventRegistrationTokenTable(XEvent).
                InvocationList
            If table IsNot Nothing Then table(sender, i)
        End RaiseEvent
    End Event
End Class
```


## <a name="constants"></a>Konstanten

Eine *Konstante* ist ein konstanter Wert, der ein Member eines Typs ist.

```antlr
ConstantMemberDeclaration
    : Attributes? ConstantModifier* 'Const' ConstantDeclarators StatementTerminator
    ;

ConstantModifier
    : AccessModifier
    | 'Shadows'
    ;

ConstantDeclarators
    : ConstantDeclarator ( Comma ConstantDeclarator )*
    ;

ConstantDeclarator
    : Identifier ( 'As' TypeName )? Equals ConstantExpression StatementTerminator
    ;
```

Konstanten werden implizit freigegeben. Wenn die Deklaration eine `As`-Klausel enthält, gibt die-Klausel den Typ des Members an, der von der Deklaration eingeführt wird. Wenn der Typ weggelassen wird, wird der Typ der Konstante abgeleitet. Der Typ einer Konstante darf nur ein primitiver Typ oder `Object` sein. Wenn eine Konstante als `Object` eingegeben wird und kein Typzeichen vorhanden ist, ist der tatsächliche Typ der Konstante der Typ des konstanten Ausdrucks. Andernfalls ist der Typ der Konstante der Typ des Typzeichens der Konstante.

Das folgende Beispiel zeigt eine Klasse mit dem Namen `Constants` mit zwei öffentlichen Konstanten:

```vb
Class Constants
    Public Const A As Integer = 1
    Public Const B As Integer = A + 1
End Class
```

Auf Konstanten kann über die-Klasse zugegriffen werden, wie im folgenden Beispiel gezeigt, das die Werte `Constants.A` und `Constants.B` ausgibt.

```vb
Module Test
    Sub Main()
        Console.WriteLine(Constants.A & ", " & Constants.B)
    End Sub 
End Module
```

Eine Konstante Deklaration, die mehrere Konstanten deklariert, entspricht mehreren Deklarationen von einzelnen Konstanten. Im folgenden Beispiel werden drei Konstanten in einer Deklarations Anweisung deklariert.

```vb
Class A
    Protected Const x As Integer = 1, y As Long = 2, z As Short = 3
End Class
```

Diese Deklaration entspricht Folgendem:

```vb
Class A
    Protected Const x As Integer = 1
    Protected Const y As Long = 2
    Protected Const z As Short = 3
End Class
```

Die Zugriffs Domäne des Typs der Konstante muss mit oder einer übergeordneten Zugriffs Domäne der Konstanten identisch sein. Der Konstante Ausdruck muss einen Wert des Konstanten Typs oder eines Typs liefern, der implizit in den Typ der Konstante konvertiert werden kann. Der Konstante Ausdruck darf nicht zirkulär sein. Dies bedeutet, dass eine Konstante nicht in Bezug auf sich selbst definiert werden kann.

Der Compiler wertet die Konstanten Deklarationen automatisch in der entsprechenden Reihenfolge aus. Im folgenden Beispiel wertet der Compiler zuerst `Y` aus, dann `Z` und schließlich `X`, wobei die Werte 10, 11 bzw. 12 erzeugt werden.

```vb
Class A
    Public Const X As Integer = B.Z + 1
    Public Const Y As Integer = 10
End Class

Class B
    Public Const Z As Integer = A.Y + 1
End Class
```

Wenn ein symbolischer Name für einen konstanten Wert gewünscht ist, aber der Typ des Werts in einer Konstanten Deklaration nicht zulässig ist oder wenn der Wert zur Kompilierzeit nicht durch einen konstanten Ausdruck berechnet werden kann, kann stattdessen eine schreibgeschützte Variable verwendet werden.


## <a name="instance-and-shared-variables"></a>Instanz und freigegebene Variablen

Eine Instanz oder eine freigegebene Variable ist ein Member eines Typs, in dem Informationen gespeichert werden können.

```antlr
VariableMemberDeclaration
    : Attributes? VariableModifier+ VariableDeclarators StatementTerminator
    ;

VariableModifier
    : AccessModifier
    | 'Shadows'
    | 'Shared'
    | 'ReadOnly'
    | 'WithEvents'
    | 'Dim'
    ;

VariableDeclarators
    : VariableDeclarator ( Comma VariableDeclarator )*
    ;

VariableDeclarator
    : VariableIdentifiers 'As' ObjectCreationExpression
    | VariableIdentifiers ( 'As' TypeName )? ( Equals Expression )?
    ;

VariableIdentifiers
    : VariableIdentifier ( Comma VariableIdentifier )*
    ;

VariableIdentifier
    : Identifier IdentifierModifiers
    ;
```

Der `Dim`-Modifizierer muss angegeben werden, wenn keine Modifizierer angegeben werden, aber andernfalls weggelassen werden. Eine einzelne Variablen Deklaration kann mehrere Variablen Deklaratoren enthalten. jeder Variablen Deklarator führt eine neue Instanz oder einen freigegebenen Member ein.

Wenn ein Initialisierer angegeben ist, kann nur eine Instanz oder eine freigegebene Variable vom Variablen Deklarator deklariert werden:

```vb
Class Test
    Dim a, b, c, d As Integer = 10  ' Invalid: multiple initialization
End Class
```

Diese Einschränkung gilt nicht für Objektinitialisierer:

```vb
Class Test
    Dim a, b, c, d As New Collection() ' OK
End Class
```

Eine Variable, die mit dem `Shared`-Modifizierer deklariert wird, ist eine frei *gegebene Variable*. Eine freigegebene Variable identifiziert genau einen Speicherort, unabhängig von der Anzahl der Instanzen des Typs, die erstellt werden. Eine freigegebene Variable ist vorhanden, wenn ein Programm mit der Ausführung beginnt, und ist nicht mehr vorhanden, wenn das Programm beendet wird.

Eine freigegebene Variable wird nur für Instanzen eines bestimmten geschlossenen generischen Typs freigegeben. Das Programm lautet z. b.:

```vb
Class C(Of V) 
    Shared InstanceCount As Integer = 0

    Public Sub New()  
        InstanceCount += 1 
    End Sub

    Public Shared ReadOnly Property Count() As Integer 
        Get
            Return InstanceCount
        End Get
    End Property
End Class

Class Application 
    Shared Sub Main() 
        Dim x1 As New C(Of Integer)()
        Console.WriteLine(C(Of Integer).Count)

        Dim x2 As New C(Of Double)() 
        Console.WriteLine(C(Of Integer).Count)

        Dim x3 As New C(Of Integer)() 
        Console.WriteLine(C(Of Integer).Count)
    End Sub
End Class
```

Druckt Folgendes:

```console
1
1
2
```

Eine Variable, die ohne den `Shared`-Modifizierer deklariert wird, wird als *Instanzvariable*bezeichnet. Jede Instanz einer Klasse enthält eine separate Kopie aller Instanzvariablen der Klasse. Eine Instanzvariable eines Verweis Typs kommt in Kraft, wenn eine neue Instanz dieses Typs erstellt wird, und ist nicht mehr vorhanden, wenn keine Verweise auf diese Instanz vorhanden sind und die `Finalize`-Methode ausgeführt wurde. Eine Instanzvariable eines Werttyps hat genau die gleiche Lebensdauer wie die Variable, zu der Sie gehört. Anders ausgedrückt: Wenn eine Variable eines Werttyps vorhanden ist oder nicht mehr vorhanden ist, wird die Instanzvariable des Werttyps verwendet.

Wenn der Deklarator eine `As`-Klausel enthält, gibt die-Klausel den Typ der Member an, die von der Deklaration eingeführt wurden. Wenn der Typ weggelassen wird und eine strikte Semantik verwendet wird, tritt ein Kompilierzeitfehler auf. Andernfalls ist der Typ der Member implizit `Object` oder der Typ des Typzeichens des Members.

__Nebenbei.__ In der Syntax gibt es keine Mehrdeutigkeit: Wenn ein Deklarator einen Typ auslässt, wird immer der Typ eines folgenden Deklarators verwendet.

Die Zugriffs Domäne einer Instanz oder eines Typ-oder Array Elementtyps der freigegebenen Variablen muss mit oder einer übergeordneten Zugriffs Domäne der Instanz oder der freigegebenen Variablen selbst identisch sein.

Im folgenden Beispiel wird eine `Color`-Klasse gezeigt, die über interne Instanzvariablen mit dem Namen `redPart`, `greenPart` und `bluePart` verfügt:

```vb
Class Color
    Friend redPart As Short
    Friend bluePart As Short
    Friend greenPart As Short

    Public Sub New(red As Short, blue As Short, green As Short)
        redPart = red
        bluePart = blue
        greenPart = green
    End Sub
End Class
```


### <a name="read-only-variables"></a>Schreibgeschützte Variablen

Wenn eine Instanz oder eine freigegebene Variablen Deklaration einen `ReadOnly`-Modifizierer enthält, können Zuweisungen zu den Variablen, die von der Deklaration eingeführt wurden, nur als Teil der Deklaration oder in einem Konstruktor in derselben Klasse auftreten. Insbesondere sind Zuweisungen zu einer schreibgeschützten Instanz oder einer freigegebenen Variablen nur in den folgenden Situationen zulässig:

* In der Variablen Deklaration, die die Instanz oder die freigegebene Variable (durch Einschließen eines variableninitialisierers in die Deklaration) einführt.

* Bei einer Instanzvariablen in den Instanzkonstruktoren der-Klasse, die die Variablen Deklaration enthält. Der Zugriff auf die Instanzvariable ist nur auf eine nicht qualifizierte Weise möglich oder über `Me` oder `MyClass`.

* Für eine freigegebene Variable im freigegebenen Konstruktor der-Klasse, die die Deklaration der freigegebenen Variablen enthält.

Eine freigegebene, schreibgeschützte Variable ist nützlich, wenn ein symbolischer Name für einen konstanten Wert erwünscht ist, aber wenn der Typ des Werts in einer Konstanten Deklaration nicht zulässig ist oder wenn der Wert nicht zur Kompilierzeit durch einen konstanten Ausdruck berechnet werden kann.

Es folgt ein Beispiel für die erste Anwendung dieser Anwendung, bei der freigegebene Farben als `ReadOnly` deklariert werden, um zu verhindern, dass Sie von anderen Programmen geändert werden:

```vb
Class Color
    Friend redPart As Short
    Friend bluePart As Short
    Friend greenPart As Short

    Public Sub New(red As Short, blue As Short, green As Short)
        redPart = red
        bluePart = blue
        greenPart = green
    End Sub 

    Public Shared ReadOnly Red As Color = New Color(&HFF, 0, 0)
    Public Shared ReadOnly Blue As Color = New Color(0, &HFF, 0)
    Public Shared ReadOnly Green As Color = New Color(0, 0, &HFF)
    Public Shared ReadOnly White As Color = New Color(&HFF, &HFF, &HFF)
End Class
```

Konstanten und schreibgeschützte freigegebene Variablen weisen eine unterschiedliche Semantik auf. Wenn ein Ausdruck auf eine Konstante verweist, wird der Wert der Konstante zur Kompilierzeit abgerufen. Wenn jedoch ein Ausdruck auf eine schreibgeschützte, freigegebene Variable verweist, wird der Wert der freigegebenen Variablen bis zur Laufzeit nicht abgerufen. Beachten Sie die folgende Anwendung, die aus zwei separaten Programmen besteht.

file1. vb:

```vb
Namespace Program1
    Public Class Utils
        Public Shared ReadOnly X As Integer = 1
    End Class
End Namespace
```

file2. vb:

```vb
Namespace Program2
    Module Test
        Sub Main()
            Console.WriteLine(Program1.Utils.X)
        End Sub
    End Module
End Namespace
```

Die Namespaces `Program1` und `Program2` bezeichnen zwei Programme, die separat kompiliert werden. Da Variable `Program1.Utils.X` als `Shared ReadOnly` deklariert ist, wird der Wert, der von der `Console.WriteLine`-Anweisung ausgegeben wird, zur Kompilierzeit nicht bekannt, sondern zur Laufzeit abgerufen. Wenn der Wert `X` geändert und `Program1` erneut kompiliert wird, gibt die `Console.WriteLine`-Anweisung den neuen Wert auch dann aus, wenn `Program2` nicht erneut kompiliert wird. Wenn `X` jedoch eine Konstante war, wurde der Wert von `X` zum Zeitpunkt der Kompilierung von `Program2` abgerufen, und hätte von Änderungen in `Program1` bis zum erneuten Kompilieren von `Program2` nicht beeinträchtigt.

### <a name="withevents-variables"></a>Widervents-Variablen

Ein Typ kann deklarieren, dass er eine Reihe von Ereignissen verarbeitet, die von einer seiner Instanzen oder freigegebenen Variablen ausgelöst werden, indem die Instanz oder die freigegebene Variable, die die Ereignisse auslöst, mit dem `WithEvents`-Modifizierer deklariert wird Zum Beispiel:

```vb
Class Raiser
    Public Event E1()

    Public Sub Raise()
        RaiseEvent E1
    End Sub
End Class

Module Test
    Private WithEvents x As Raiser

    Private Sub E1Handler() Handles x.E1
        Console.WriteLine("Raised")
    End Sub

    Public Sub Main()
        x = New Raiser()
    End Sub
End Module
```

In diesem Beispiel behandelt die-Methode `E1Handler` das Ereignis `E1`, das von der Instanz des Typs `Raiser` ausgelöst wird, der in der Instanzvariable `x` gespeichert ist.

Der `WithEvents`-Modifizierer bewirkt, dass die Variable mit einem führenden Unterstrich umbenannt und durch eine Eigenschaft mit dem gleichen Namen ersetzt wird, der das Ereignis einrichtet. Wenn der Name der Variablen beispielsweise `F` ist, wird Sie in `_F` umbenannt, und eine Eigenschaft `F` wird implizit deklariert. Wenn eine Kollision zwischen dem neuen Namen der Variablen und einer anderen Deklaration vorliegt, wird ein Kompilierzeitfehler gemeldet. Alle Attribute, die auf die Variable angewendet werden, werden auf die umbenannte Variable übertragen.

Die implizite Eigenschaft, die von einer `WithEvents`-Deklaration erstellt wurde, kümmert sich um das Verknüpfen und Aufheben der entsprechenden Ereignishandler. Wenn der Variablen ein Wert zugewiesen wird, ruft die-Eigenschaft zuerst die `remove`-Methode für das-Ereignis auf der-Instanz auf, die sich derzeit in der-Variable befindet (wobei der vorhandene Ereignishandler ggf. unverknüpft ist). Als nächstes wird die Zuweisung vorgenommen, und die-Eigenschaft ruft die `add`-Methode für das-Ereignis auf der neuen Instanz in der-Variable auf (verknüpft den neuen Ereignishandler). Der folgende Code entspricht dem obigen Code für das Standardmodul `Test`:

```vb
Module Test
    Private _x As Raiser

    Public Property x() As Raiser
        Get
            Return _x
        End Get

        Set (Value As Raiser)
            ' Unhook any existing handlers.
            If _x IsNot Nothing Then
                RemoveHandler _x.E1, AddressOf E1Handler
            End If

            ' Change value.
            _x = Value

            ' Hook-up new handlers.
            If _x IsNot Nothing Then
                AddHandler _x.E1, AddressOf E1Handler
            End If
        End Set
    End Property

    Sub E1Handler()
        Console.WriteLine("Raised")
    End Sub

    Sub Main()
        x = New Raiser()
    End Sub
End Module
```

Es ist nicht zulässig, eine Instanz oder eine freigegebene Variable als `WithEvents` zu deklarieren, wenn die Variable als Struktur typisiert ist. Darüber hinaus darf `WithEvents` nicht in einer Struktur angegeben werden, und `WithEvents` und `ReadOnly` können nicht kombiniert werden.

### <a name="variable-initializers"></a>Variableninitialisierer

Deklarationen von Instanzen und freigegebenen Variablen in Klassen und Instanzen Variablen Deklarationen (aber nicht freigegebene Variablen Deklarationen) in Strukturen können Variableninitialisierer enthalten. Für Variablen vom Typ "`Shared`" entsprechen Variableninitialisierern Zuweisungs Anweisungen, die nach Beginn des Programms ausgeführt werden, aber vor dem ersten Verweis auf die Variable "`Shared`". Bei Instanzvariablen entsprechen Variableninitialisierern Zuweisungs Anweisungen, die ausgeführt werden, wenn eine Instanz der-Klasse erstellt wird. Strukturen können keine instanzvariableninitialisierer aufweisen, da ihre Parameter losen Konstruktoren nicht geändert werden können.

Betrachten Sie das folgende Beispiel:

```vb
Class Test
    Public Shared x As Double = Math.Sqrt(2.0)
    Public i As Integer = 100
    Public s As String = "Hello"
End Class

Module TestModule
    Sub Main()
        Dim a As New Test()

        Console.WriteLine("x = " & Test.x & ", i = " & a.i & ", s = " & a.s)
    End Sub
End Module
```

Das Beispiel führt zur folgenden Ausgabe:

```console
x = 1.4142135623731, i = 100, s = Hello
```

Eine Zuweisung zu "`x`" tritt auf, wenn die Klasse geladen wird und Zuweisungen zu `i` und `s` auftreten, wenn eine neue Instanz der Klasse erstellt wird.

Es ist hilfreich, Variablen Initialisierer als Zuweisungs Anweisungen zu betrachten, die automatisch in den-Block des Konstruktors des Typs eingefügt werden. Das folgende Beispiel enthält mehrere instanzvariableninitialisierer.

```vb
Class A
    Private x As Integer = 1
    Private y As Integer = -1
    Private count As Integer

    Public Sub New()
        count = 0
    End Sub

    Public Sub New(n As Integer)
        count = n
    End Sub
End Class

Class B
    Inherits A

    Private sqrt2 As Double = Math.Sqrt(2.0)
    Private items As ArrayList = New ArrayList(100)
    Private max As Integer

    Public Sub New()
        Me.New(100)
        items.Add("default")
    End Sub

    Public Sub New(n As Integer)
        MyBase.New(n - 1)
        max = n
    End Sub
End Class
```

Das Beispiel entspricht dem unten gezeigten Code, wobei jeder Kommentar eine automatisch eingefügte Anweisung angibt.

```vb
Class A
    Private x, y, count As Integer

    Public Sub New()
        MyBase.New ' Invoke object() constructor.
        x = 1 ' This is a variable initializer.
        y = -1 ' This is a variable initializer.
        count = 0
    End Sub

    Public Sub New(n As Integer)
        MyBase.New ' Invoke object() constructor. 
        x = 1 ' This is a variable initializer.
        y = - 1 ' This is a variable initializer.
        count = n
    End Sub
End Class

Class B
    Inherits A

    Private sqrt2 As Double
    Private items As ArrayList
    Private max As Integer

    Public Sub New()
        Me.New(100) 
        items.Add("default")
    End Sub

    Public Sub New(n As Integer)
        MyBase.New(n - 1) 
        sqrt2 = Math.Sqrt(2.0) ' This is a variable initializer.
        items = New ArrayList(100) ' This is a variable initializer.
        max = n
    End Sub
End Class
```

Alle Variablen werden mit dem Standardwert ihres Typs initialisiert, bevor Variablen Initialisierer ausgeführt werden. Zum Beispiel:

```vb
Class Test
    Public Shared b As Boolean
    Public i As Integer
End Class

Module TestModule
    Sub Main()
        Dim t As New Test()
        Console.WriteLine("b = " & Test.b & ", i = " & t.i)
    End Sub
End Module
```

Da `b` automatisch mit dem Standardwert initialisiert wird, wenn die-Klasse geladen wird und `i` automatisch mit dem Standardwert initialisiert wird, wenn eine Instanz der-Klasse erstellt wird, erzeugt der vorangehende Code die folgende Ausgabe:

```console
b = False, i = 0
```

Jeder Variableninitialisierer muss einen Wert des Variablen Typs oder eines Typs liefern, der implizit in den Typ der Variablen konvertiert werden kann. Ein Variableninitialisierer kann zirkulär sein oder auf eine Variable verweisen, die danach initialisiert wird. in diesem Fall ist der Wert der Variablen, auf die verwiesen wird, der Standardwert für den Initialisierer. Ein solcher Initialisierer weist einen zweifelhaften Wert auf.

Es gibt drei Formen von Variableninitialisierern: reguläre Initialisierer, arraygrößeninitialisierer und Objektinitialisierer. Die ersten beiden Formulare werden nach einem Gleichheitszeichen angezeigt, das auf den Typnamen folgt. die beiden beiden Formulare sind Teil der Deklaration selbst. Für eine bestimmte Deklaration kann nur eine Form des Initialisierers verwendet werden.

#### <a name="regular-initializers"></a>Reguläre Initialisierer

Ein *regulärer Initialisierer* ist ein Ausdruck, der implizit in den Typ der Variablen konvertiert werden kann. Sie wird nach einem Gleichheitszeichen angezeigt, das auf den Typnamen folgt und als Wert klassifiziert werden muss. Zum Beispiel:

```vb
Module Test
    Dim x As Integer = 10
    Dim y As Integer = 20

    Sub Main()
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub
End Module
```

Dieses Programm erzeugt die Ausgabe:

```console
x = 10, y = 20
```

Wenn eine Variablen Deklaration einen regulären Initialisierer aufweist, kann jeweils nur eine Variable deklariert werden. Zum Beispiel:

```vb
Module Test
    Sub Main()
        ' OK, only one variable declared at a time.
        Dim x As Integer = 10, y As Integer = 20

        ' Error: Can't initialize multiple variables at once.
        Dim a, b As Integer = 10
    End Sub
End Module
```

#### <a name="object-initializers"></a>Objektinitialisierer

Ein *Objektinitialisierer* wird mit einem Objekt Erstellungs Ausdruck an der Stelle des Typnamens angegeben. Ein Objektinitialisierer entspricht einem regulären Initialisierer, der das Ergebnis des Ausdrucks zur Objekt Erstellung der Variablen zuweist. So

```vb
Module TestModule
    Sub Main()
        Dim x As New Test(10)
    End Sub
End Module
```

für die folgende Syntax:

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test(10)
    End Sub
End Module
```

Die Klammer in einem Objektinitialisierer wird immer als Argumentliste für den Konstruktor und nie als Arraytypmodifizierer interpretiert. Ein Variablenname mit einem Objektinitialisierer kann keinen Arraytypmodifizierer oder einen Typmodifizierer, der NULL-Werte zulässt, aufweisen.

#### <a name="array-size-initializers"></a>Initialisierer für Array Größe

Ein *arraygrößeninitialisierer* ist ein Modifizierer für den Namen der Variablen, der einen Satz von Dimensions Obergrenzen bietet, der durch Ausdrücke gekennzeichnet ist.

```antlr
ArraySizeInitializationModifier
    : OpenParenthesis BoundList CloseParenthesis ArrayTypeModifiers?
    ;

BoundList
    : Bound ( Comma Bound )*
    ;

Bound
    : Expression
    | '0' 'To' Expression
    ;
```

Die oberen gebundenen Ausdrücke müssen als Werte klassifiziert werden und müssen implizit in `Integer` konvertiert werden können. Der Satz von oberen Begrenzungen entspricht einem Variableninitialisierer eines Ausdrucks der Array Erstellung mit den angegebenen Obergrenzen. Die Anzahl der Dimensionen des Arraytyps wird aus dem arraygrößeninitialisierer abgeleitet. So

```vb
Module Test
    Sub Main()
        Dim x(5, 10) As Integer
    End Sub
End Module
```

für die folgende Syntax:

```vb
Module Test
    Sub Main()
        Dim x As Integer(,) = New Integer(5, 10) {}
    End Sub
End Module
```

Alle oberen Begrenzungen müssen größer oder gleich-1 sein, und für alle Dimensionen muss eine obere Grenze angegeben werden. Wenn der Elementtyp des zu initialisierenden Arrays selbst ein Arraytyp ist, werden die Arraytypmodifizierer auf der rechten Seite des Initialisierers der Array Größe angezeigt. Beispiel:

```vb
Module Test
    Sub Main()
        Dim x(5,10)(,,) As Integer
    End Sub
End Module
```

deklariert eine lokale Variable `x`, deren Typ ein zweidimensionales Array von dreidimensionalen Arrays von `Integer` ist, die auf ein Array mit den Begrenzungen `0..5` in der ersten Dimension und `0..10` in der zweiten Dimension initialisiert werden. Es ist nicht möglich, einen arraygrößeninitialisierer zu verwenden, um die Elemente einer Variablen zu initialisieren, deren Typ ein Array von Arrays ist.

Eine Variablen Deklaration mit einem Initialisierer mit Array Größe kann keinen Arraytypmodifizierer für den Typ oder einen regulären Initialisierer aufweisen.


### <a name="systemmarshalbyrefobject-classes"></a>System. MarshalByRefObject-Klassen

Klassen, die von der-Klasse `System.MarshalByRefObject` abgeleitet werden, werden über Kontext Grenzen hinweg gemarshallt, und zwar mithilfe von Proxys (d. h. durch Verweis) anstatt durch Kopieren (d. h. durch Wert) Dies bedeutet, dass eine Instanz einer solchen Klasse möglicherweise keine echte Instanz ist, sondern stattdessen ein Stub sein kann, der Variablen Zugriffe und Methodenaufrufe über eine Kontext Grenze hinweg marshunrätet.

Folglich ist es nicht möglich, einen Verweis auf den Speicherort von Variablen zu erstellen, die für solche Klassen definiert sind. Dies bedeutet, dass Variablen, die als von `System.MarshalByRefObject` abgeleitete Klassen eingegeben werden, nicht an Verweis Parameter und Methoden und Variablen von Variablen, die als Werttypen typisiert sind, möglicherweise nicht zugegriffen werden können. Stattdessen Visual Basic die Variablen, die für solche Klassen definiert sind, so behandelt, als wären Sie Eigenschaften (da die Einschränkungen bei Eigenschaften identisch sind).

Es gibt eine Ausnahme von dieser Regel: ein Member, der implizit oder explizit mit `Me` qualifiziert ist, ist von den oben genannten Einschränkungen ausgenommen, da `Me` immer garantiert ein tatsächliches Objekt und kein Proxy ist.

## <a name="properties"></a>Eigenschaften

*Eigenschaften* sind eine natürliche Erweiterung von Variablen. Beide sind benannte Member mit zugeordneten Typen, und die Syntax für den Zugriff auf Variablen und Eigenschaften ist identisch. Im Gegensatz zu Variablen bezeichnen Eigenschaften jedoch keine Speicherorte. Stattdessen verfügen Eigenschaften über *Accessoren*, die die auszuführenden Anweisungen angeben, um ihre Werte zu lesen oder zu schreiben.

Eigenschaften werden mit Eigenschafts Deklarationen definiert. Der erste Teil einer Eigenschafts Deklaration ähnelt einer Feld Deklaration. Der zweite Teil enthält einen `Get`-Accessor und/oder einen `Set`-Accessor.

```antlr
PropertyMemberDeclaration
    : RegularPropertyMemberDeclaration
    | MustOverridePropertyMemberDeclaration
    | AutoPropertyMemberDeclaration
    ;

PropertySignature
    : 'Property'
      Identifier ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )?
    ;

RegularPropertyMemberDeclaration
    : Attributes? PropertyModifier* PropertySignature
      ImplementsClause? LineTerminator
      PropertyAccessorDeclaration+
      'End' 'Property' StatementTerminator
    ;

MustOverridePropertyMemberDeclaration
    : Attributes? MustOverridePropertyModifier+ PropertySignature
      ImplementsClause? StatementTerminator
    ;

AutoPropertyMemberDeclaration
    : Attributes? AutoPropertyModifier* 'Property' Identifier
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )? ( Equals Expression )?
      ImplementsClause? LineTerminator
    | Attributes? AutoPropertyModifier* 'Property' Identifier
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      'As' Attributes? 'New'
      ( NonArrayTypeName ( OpenParenthesis ArgumentList? CloseParenthesis )? )?
      ObjectCreationExpressionInitializer?
      ImplementsClause? LineTerminator
    ;

InterfacePropertyMemberDeclaration
    : Attributes? InterfacePropertyModifier* PropertySignature StatementTerminator
    ;

AutoPropertyModifier
    : AccessModifier
    | 'Shadows'
    | 'Shared'
    | 'Overridable'
    | 'NotOverridable'
    | 'Overrides'
    | 'Overloads'
    ;

PropertyModifier
    : AutoPropertyModifier
    | 'Default'
    | 'ReadOnly'
    | 'WriteOnly'
    | 'Iterator'
    ;

MustOverridePropertyModifier
    : PropertyModifier
    | 'MustOverride'
    ;

InterfacePropertyModifier
    : 'Shadows'
    | 'Overloads'
    | 'Default'
    | 'ReadOnly'
    | 'WriteOnly'
    ;

PropertyAccessorDeclaration
    : PropertyGetDeclaration
    | PropertySetDeclaration
    ;
```

Im folgenden Beispiel definiert die Klasse `Button` eine Eigenschaft `Caption`.

```vb
Public Class Button
    Private captionValue As String

    Public Property Caption() As String
        Get
            Return captionValue
        End Get

        Set (Value As String)
            captionValue = value
            Repaint()
        End Set
    End Property

    ...
End Class
```

Das folgende Beispiel zeigt die Verwendung der Eigenschaft "`Caption`" basierend auf der `Button`-Klasse:

```vb
Dim okButton As Button = New Button()

okButton.Caption = "OK" ' Invokes Set accessor.
Dim s As String = okButton.Caption ' Invokes Get accessor.
```

Hier wird der `Set`-Accessor aufgerufen, indem der-Eigenschaft ein Wert zugewiesen wird, und der `Get`-Accessor wird aufgerufen, indem auf die-Eigenschaft in einem Ausdruck verwiesen wird.

Wenn für eine Eigenschaft kein Typ angegeben ist und eine strikte Semantik verwendet wird, tritt ein Kompilierzeitfehler auf. Andernfalls ist der Typ der Eigenschaft implizit `Object` oder der Typ des Typzeichens der Eigenschaft. Eine Eigenschafts Deklaration kann entweder einen `Get`-Accessor enthalten, der den Wert der-Eigenschaft abruft, einen `Set`-Accessor, der den Wert der-Eigenschaft speichert, oder beides. Da eine Eigenschaft Methoden implizit deklariert, kann eine Eigenschaft mit denselben modifiziererobjekten wie eine Methode deklariert werden. Wenn die Eigenschaft in einer Schnittstelle definiert oder mit dem `MustOverride`-Modifizierer definiert ist, müssen der Eigenschaften Text und das `End`-Konstrukt ausgelassen werden. Andernfalls tritt ein Kompilierzeitfehler auf.

Die Index Parameterliste bildet die Signatur der-Eigenschaft, sodass Eigenschaften möglicherweise für Index Parameter überladen werden, jedoch nicht für den Typ der Eigenschaft. Die Index Parameterliste ist die gleiche wie bei einer regulären Methode. Allerdings kann keiner der Parameter mit dem `ByRef`-Modifizierer geändert werden, und keiner der Parameter kann `Value` lauten (der für den impliziten value-Parameter im `Set`-Accessor reserviert ist).

Eine Eigenschaft kann wie folgt deklariert werden:

* Wenn die-Eigenschaft keinen Eigenschaftentyp-Modifizierer angibt, muss die-Eigenschaft sowohl einen `Get`-Accessor als auch einen `Set`-Accessor aufweisen. Die Eigenschaft wird als Lese-/Schreibeigenschaft bezeichnet.

* Wenn die-Eigenschaft den `ReadOnly`-Modifizierer angibt, muss die-Eigenschaft über einen `Get`-Accessor verfügen, der möglicherweise nicht über einen `Set`-Accessor verfügt. Die Eigenschaft wird als schreibgeschützte Eigenschaft bezeichnet. Es handelt sich um einen Kompilierzeitfehler für eine schreibgeschützte Eigenschaft, die das Ziel einer Zuweisung ist.

* Wenn die-Eigenschaft den `WriteOnly`-Modifizierer angibt, muss die-Eigenschaft über einen `Set`-Accessor verfügen, der möglicherweise nicht über einen `Get`-Accessor verfügt. Die Eigenschaft wird als schreibgeschützte Eigenschaft bezeichnet. Es ist ein Kompilierzeitfehler, der auf eine schreibgeschützte Eigenschaft in einem Ausdruck verweist, außer als Ziel einer Zuweisung oder als Argument für eine Methode.

Die `Get`-und `Set`-Accessoren einer Eigenschaft sind keine unterschiedlichen Member, und es ist nicht möglich, die Accessoren einer Eigenschaft separat zu deklarieren. Im folgenden Beispiel wird keine einzige Lese-/Schreibeigenschaft deklariert. Stattdessen werden zwei Eigenschaften mit demselben Namen deklariert, ein Schreib geschützter und ein Schreib geschützter Wert:

```vb
Class A
    Private nameValue As String

    ' Error, contains a duplicate member name.
    Public ReadOnly Property Name() As String 
        Get
            Return nameValue
        End Get
    End Property

    ' Error, contains a duplicate member name.
    Public WriteOnly Property Name() As String 
        Set (Value As String)
            nameValue = value
        End Set
    End Property
End Class
```

Da zwei Member, die in derselben Klasse deklariert werden, nicht denselben Namen haben können, verursacht das Beispiel einen Kompilierzeitfehler.

Standardmäßig ist der Zugriff auf die `Get`-und `Set`-Accessoren der Eigenschaft identisch mit der Barrierefreiheit der Eigenschaft selbst. Allerdings können die Accessoren "`Get`" und "`Set`" auch den Zugriff separat von der Eigenschaft angeben. In diesem Fall muss der Zugriff auf einen Accessor restriktiver sein als der Zugriff auf die-Eigenschaft, und nur ein Accessor kann eine andere Barrierefreiheits Stufe als die-Eigenschaft aufweisen. Zugriffs Typen werden wie folgt als mehr oder weniger restriktiv angesehen:

* `Private` ist restriktiver als `Public`, `Protected Friend`, `Protected` oder `Friend`.

* `Friend` ist restriktiver als `Protected Friend` oder `Public`.

* `Protected` ist restriktiver als `Protected Friend` oder `Public`.

* `Protected Friend` ist restriktiver als `Public`.

Wenn auf einen der Accessoren einer Eigenschaft zugegriffen werden kann, der andere jedoch nicht, wird die Eigenschaft so behandelt, als ob Sie schreibgeschützt oder schreibgeschützt wäre. Zum Beispiel:

```vb
Class A
    Public Property P() As Integer
        Get
            ...
        End Get

        Private Set (Value As Integer)
            ...
        End Set
    End Property
End Class

Module Test
    Sub Main()
        Dim a As A = New A()

        ' Error: A.P is read-only in this context.
        a.P = 10
    End Sub
End Module
```

Wenn ein abgeleiteter Typ einen Shadowing für eine Eigenschaft durchführt, verbirgt die abgeleitete Eigenschaft die Shadowing-Eigenschaft in Bezug auf das Lesen und schreiben Im folgenden Beispiel blendet die `P`-Eigenschaft in `B` die `P`-Eigenschaft in `A` in Bezug auf Lese-und Schreibvorgänge aus:

```vb
Class A
    Public WriteOnly Property P() As Integer
        Set (Value As Integer)
        End Set
    End Property
End Class

Class B
    Inherits A

    Public Shadows ReadOnly Property P() As Integer
       Get
       End Get
    End Property
End Class

Module Test
    Sub Main()
        Dim x As B = New B

        B.P = 10     ' Error, B.P is read-only.
    End Sub
End Module
```

Die Zugriffs Domäne des Rückgabe Typs oder der Parametertypen muss mit oder einer übergeordneten Zugriffs Domäne der Eigenschaft selbst identisch sein. Eine Eigenschaft darf nur über einen `Set`-Accessor und einen `Get`-Accessor verfügen.

Mit Ausnahme der Unterschiede in der Deklaration und der Aufruf Syntax Verhalten sich `Overridable`-, `NotOverridable`-, `Overrides`-, `MustOverride`-und `MustInherit`-Eigenschaften genau wie `Overridable`-, `NotOverridable`-, `Overrides`-, `MustOverride`-und `MustInherit`-Methoden. Wenn eine Eigenschaft überschrieben wird, muss die über schreibende Eigenschaft denselben Typ aufweisen (Lese-/Schreibzugriff, schreibgeschützt, schreibgeschützt). Eine `Overridable`-Eigenschaft darf keinen `Private`-Accessor enthalten.

Im folgenden Beispiel ist `X` eine schreibgeschützte Eigenschaft `Overridable`. `Y` ist eine Lese-/Schreibeigenschaft vom `Overridable`, und `Z` ist eine `MustOverride`-Eigenschaft mit Lese-/Schreibzugriff.

```vb
MustInherit Class A
    Private _y As Integer

    Public Overridable ReadOnly Property X() As Integer
        Get
            Return 0
        End Get
    End Property

    Public Overridable Property Y() As Integer
        Get
            Return _y
         End Get
        Set (Value As Integer)
            _y = value
        End Set
    End Property

    Public MustOverride Property Z() As Integer
End Class
```

Da `Z` `MustOverride` ist, muss die enthaltende Klasse `A` `MustInherit` deklariert werden.

Im Gegensatz dazu wird eine Klasse, die von der Klasse `A` abgeleitet ist, im folgenden dargestellt:

```vb
Class B
    Inherits A

    Private _z As Integer

    Public Overrides ReadOnly Property X() As Integer
        Get
            Return MyBase.X + 1
        End Get
    End Property

    Public Overrides Property Y() As Integer
        Get
            Return MyBase.Y
        End Get
        Set (Value As Integer)
            If value < 0 Then
                MyBase.Y = 0
            Else
                MyBase.Y = Value
            End If
        End Set
    End Property

    Public Overrides Property Z() As Integer
        Get
            Return _z
        End Get
        Set (Value As Integer)
            _z = Value
        End Set
    End Property
End Class
```

Hier überschreiben die Deklarationen von Eigenschaften `X`, `Y` und `Z` die Basis Eigenschaften. Jede Eigenschaften Deklaration stimmt genau mit den zugriffsmodifizierertypen und dem Namen der entsprechenden geerbten Eigenschaft überein. Der `Get`-Accessor der Eigenschaft `X` und der `Set`-Accessor der Eigenschaft `Y` verwenden das `MyBase`-Schlüsselwort für den Zugriff auf die geerbten Eigenschaften. Die Deklaration der Eigenschaft "`Z`" überschreibt die Eigenschaft "`MustOverride`". Folglich gibt es keine ausstehenden `MustOverride`-Member in der Klasse "`B`", und `B` darf eine reguläre Klasse sein.

Eigenschaften können verwendet werden, um die Initialisierung einer Ressource zu verzögern, bis zu dem Zeitpunkt, zu dem Sie erstmals referenziert wird. Zum Beispiel:

```vb
Imports System.IO

Public Class ConsoleStreams
    Private Shared reader As TextReader
    Private Shared writer As TextWriter
    Private Shared errors As TextWriter

    Public Shared ReadOnly Property [In]() As TextReader
        Get
            If reader Is Nothing Then
                reader = Console.In
            End If
            Return reader
        End Get
    End Property

    Public Shared ReadOnly Property Out() As TextWriter
        Get
            If writer Is Nothing Then
                writer = Console.Out
            End If
            Return writer
        End Get
    End Property

    Public Shared ReadOnly Property [Error]() As TextWriter
        Get
            If errors Is Nothing Then
                errors = Console.Error
            End If
            Return errors
        End Get
    End Property
End Class
```

Die `ConsoleStreams`-Klasse enthält drei Eigenschaften, `In`, `Out` und `Error`, die jeweils die standardmäßigen Eingabe-, Ausgabe-und Fehler Geräte darstellen. Wenn diese Member als Eigenschaften verfügbar gemacht werden, kann die `ConsoleStreams`-Klasse Ihre Initialisierung verzögern, bis Sie tatsächlich verwendet werden. Wenn Sie z. b. zuerst auf die `Out`-Eigenschaft verweisen, wie in `ConsoleStreams.Out.WriteLine("hello, world")`, wird die zugrunde liegende `TextWriter` für das Ausgabegerät initialisiert. Wenn die Anwendung jedoch keinen Verweis auf die Eigenschaften `In` und `Error` erstellt, werden für diese Geräte keine Objekte erstellt.


### <a name="get-accessor-declarations"></a>Get-Accessor-Deklarationen

Ein `Get`-Accessor (Getter) wird mithilfe einer Eigenschaft `Get`-Deklaration deklariert. Eine Eigenschaft `Get`-Deklaration besteht aus dem Schlüsselwort `Get`, gefolgt von einem Anweisungsblock. Wenn eine Eigenschaft mit dem Namen "`P`" angegeben ist, wird eine Methode mit dem Namen "`get_P`" von einer `Get`-Accessor-Deklaration implizit mit denselben modifizierertypen und Parameterlisten wie die-Eigenschaft deklariert. Wenn der Typ eine Deklaration mit diesem Namen enthält, wird ein Fehler bei der Kompilierzeit ausgegeben, die implizite Deklaration wird jedoch zum Zweck der namens Bindung ignoriert.

Eine spezielle lokale Variable, die implizit im Deklarations Bereich des `Get`-Accessor mit dem gleichen Namen wie die-Eigenschaft deklariert wird, stellt den Rückgabewert der-Eigenschaft dar. Die lokale Variable hat eine spezielle namens Auflösungs Semantik, wenn Sie in Ausdrücken verwendet wird. Wenn die lokale Variable in einem Kontext verwendet wird, der einen Ausdruck erwartet, der als Methoden Gruppe klassifiziert ist (z. b. ein Aufruf Ausdruck), wird der Name in die Funktion und nicht in die lokale Variable aufgelöst. Zum Beispiel:

```vb
ReadOnly Property F(i As Integer) As Integer
    Get
        If i = 0 Then
            F = 1    ' Sets the return value.
        Else
            F = F(i - 1) ' Recursive call.
        End If
    End Get
End Property
```

Die Verwendung von Klammern kann mehrdeutige Situationen verursachen (z. b. `F(1)`, wenn `F` eine Eigenschaft ist, deren Typ ein eindimensionales Array ist). In allen mehrdeutigen Situationen wird der Name in die-Eigenschaft und nicht in die lokale Variable aufgelöst. Zum Beispiel:

```vb
ReadOnly Property F(i As Integer) As Integer()
    Get
        If i = 0 Then
            F = new Integer(2) { 1, 2, 3 }
        Else
            F = F(i - 1) ' Recursive call, not index.
        End If
    End Get
End Property
```

Wenn die Ablauf Steuerung den `Get`-Accessor-Text verlässt, wird der Wert der lokalen Variablen an den Aufruf Ausdruck zurückgegeben. Da das Aufrufen eines `Get`-Accessoren konzeptionell Äquivalent zum Lesen des Werts einer Variablen ist, wird er als ungültiges Programmier Format angesehen, damit `Get`-Accessoren Observable-Nebeneffekte haben, wie im folgenden Beispiel veranschaulicht:

```vb
Class Counter
    Private Value As Integer

    Public ReadOnly Property NextValue() As Integer
        Get
            Value += 1
            Return Value
        End Get
    End Property
End Class
```

Der Wert der `NextValue`-Eigenschaft hängt von der Häufigkeit ab, mit der zuvor auf die Eigenschaft zugegriffen wurde. Folglich erzeugt der Zugriff auf die-Eigenschaft einen beobachtbaren Nebeneffekt, und die-Eigenschaft sollte stattdessen als-Methode implementiert werden.

Die "No Side Effects"-Konvention für `Get`-Accessoren bedeutet nicht, dass `Get`-Accessoren immer geschrieben werden sollten, um nur in Variablen gespeicherte Werte zurückzugeben. In der Tat berechnen `Get`-Accessoren häufig den Wert einer Eigenschaft, indem Sie auf mehrere Variablen zugreifen oder Methoden aufrufen. Ein ordnungsgemäß entworfener `Get`-Accessor führt jedoch keine Aktionen aus, die Observable-Änderungen im Status des Objekts bewirken.

__Nebenbei.__ `Get`-Accessoren haben bei der Zeilen Platzierung die gleiche Einschränkung wie bei den Unterroutinen. Die beginnende Anweisung, End-Anweisung und Block müssen alle am Anfang einer logischen Zeile angezeigt werden.

```antlr
PropertyGetDeclaration
    : Attributes? AccessModifier? 'Get' LineTerminator
      Block?
      'End' 'Get' StatementTerminator
    ;
```

### <a name="set-accessor-declarations"></a>Set-Accessor-Deklarationen

Ein `Set`-Accessor (Setter) wird mithilfe einer Eigenschaften Satz Deklaration deklariert. Eine Eigenschafts Satz Deklaration besteht aus dem Schlüsselwort `Set`, einer optionalen Parameterliste und einem Anweisungsblock. Wenn eine Eigenschaft mit dem Namen "`P`" angegeben ist, deklariert eine Setter-Deklaration implizit eine Methode mit dem Namen "`set_P`" mit denselben Modifizierers und Parameterlisten wie die-Eigenschaft. Wenn der Typ eine Deklaration mit diesem Namen enthält, wird ein Fehler bei der Kompilierzeit ausgegeben, die implizite Deklaration wird jedoch zum Zweck der namens Bindung ignoriert.

Wenn eine Parameterliste angegeben ist, muss Sie über einen Member verfügen, dieser Member muss über keine Modifizierer verfügen, außer `ByVal`, und sein Typ muss mit dem Typ der Eigenschaft identisch sein. Der-Parameter stellt den festzulegenden Eigenschafts Wert dar. Wenn der-Parameter ausgelassen wird, wird ein Parameter mit dem Namen "`Value`" implizit deklariert.

__Nebenbei.__ `Set`-Accessoren haben bei der Zeilen Platzierung die gleiche Einschränkung wie bei den Unterroutinen. Die beginnende Anweisung, End-Anweisung und Block müssen alle am Anfang einer logischen Zeile angezeigt werden.

```antlr
PropertySetDeclaration
    : Attributes? AccessModifier? 'Set'
      ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block?
      'End' 'Set' StatementTerminator
    ;
```

### <a name="default-properties"></a>Standardeigenschaften

Eine-Eigenschaft, die den-Modifizierer angibt, `Default` als *Standard Eigenschaft*bezeichnet wird. Jeder Typ, der Eigenschaften zulässt, kann über eine Default-Eigenschaft verfügen, einschließlich Schnittstellen. Auf die Default-Eigenschaft kann verwiesen werden, ohne dass die Instanz mit dem Namen der Eigenschaft qualifiziert werden muss. Folglich, wenn eine Klasse

```vb
Class Test
    Public Default ReadOnly Property Item(i As Integer) As Integer
        Get
            Return i
        End Get
    End Property
End Class
```

Der Code

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test()
        Dim y As Integer

        y = x(10)
    End Sub
End Module
```

für die folgende Syntax:

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test()
        Dim y As Integer

        y = x.Item(10)
    End Sub
End Module
```

Nachdem eine Eigenschaft `Default` deklariert wurde, werden alle Eigenschaften, die für diesen Namen in der Vererbungs Hierarchie überladen werden, als Standard Eigenschaft fest, unabhängig davon, ob Sie als `Default` deklariert wurden oder nicht. Das Deklarieren einer Eigenschaft `Default` in einer abgeleiteten Klasse, wenn die Basisklasse, die eine Standard Eigenschaft mit einem anderen Namen deklariert hat, keine weiteren Modifizierer wie `Shadows` oder `Overrides` erfordert. Dies liegt daran, dass die Standard Eigenschaft keine Identität oder Signatur aufweist und daher nicht schattiert oder überladen werden kann. Zum Beispiel:

```vb
Class Base
    Public ReadOnly Default Property Item(i As Integer) As Integer
        Get
            Console.WriteLine("Base = " & i)
        End Get
    End Property
End Class

Class Derived
    Inherits Base

    ' This hides Item, but does not change the default property.
    Public Shadows ReadOnly Property Item(i As Integer) As Integer
        Get
            Console.WriteLine("Derived = " & i)
        End Get
    End Property
End Class

Class MoreDerived
    Inherits Derived

    ' This declares a new default property, but not Item.
    ' This does not need to be declared Shadows
    Public ReadOnly Default Property Value(i As Integer) As Integer
        Get
            Console.WriteLine("MoreDerived = " & i)
        End Get
    End Property
End Class

Module Test
    Sub Main()
        Dim x As MoreDerived = New MoreDerived()
        Dim y As Integer
        Dim z As Derived = x

        y = x(10)        ' Calls MoreDerived.Value.
        y = x.Item(10)   ' Calls Derived.Item
        y = z(10)        ' Calls Base.Item
    End Sub
End Module
```

Dieses Programm erzeugt die Ausgabe:

```console
MoreDerived = 10
Derived = 10
Base = 10
```

Alle Standardeigenschaften, die innerhalb eines Typs deklariert werden, müssen denselben Namen aufweisen, und aus Gründen der Übersichtlichkeit muss den `Default`-Modifizierer angeben. Da eine Standard Eigenschaft ohne Index Parameter eine mehrdeutige Situation verursachen würde, wenn Instanzen der enthaltenden Klasse zugewiesen werden, müssen die Standardeigenschaften über Index Parameter verfügen. Wenn eine Eigenschaft, die mit einem bestimmten Namen überladen wird, den `Default`-Modifizierer enthält, müssen alle Eigenschaften, die für diesen Namen überladen werden, Sie angeben. Standardeigenschaften dürfen nicht `Shared` sein, und mindestens ein Accessor der Eigenschaft darf nicht `Private` sein.

### <a name="automatically-implemented-properties"></a>Automatisch implementierte Eigenschaften

Wenn eine Eigenschaft die Deklaration eines Accessors auslässt, wird automatisch eine Implementierung der-Eigenschaft bereitgestellt, es sei denn, die Eigenschaft wird in einer Schnittstelle deklariert oder als `MustOverride` deklariert. Nur Lese-/Schreibeigenschaften ohne Argumente können automatisch implementiert werden. Andernfalls tritt ein Kompilierzeitfehler auf.

Eine automatisch implementierte Eigenschaft `x`, auch wenn Sie eine andere Eigenschaft überschreibt, führt eine private lokale Variable ein `_x` mit dem gleichen Typ wie die Eigenschaft. Wenn eine Kollision zwischen dem Namen der lokalen Variablen und einer anderen Deklaration vorliegt, wird ein Kompilierzeitfehler gemeldet. Der `Get`-Accessor der automatisch implementierten Eigenschaft gibt den Wert des lokalen und den `Set`-Accessor der Eigenschaft zurück, der den Wert des lokalen festlegt. Beispielsweise ist die Deklaration:

```vb
Public Property x() As Integer
```

ist ungefähr Äquivalent zu:

```vb
Private _x As Integer
Public Property x() As Integer
    Get
        Return _x
    End Get
    Set (value As Integer)
        _x = value
    End Set
End Property
```

Wie bei Variablen Deklarationen kann eine automatisch implementierte Eigenschaft einen Initialisierer enthalten. Zum Beispiel:

```vb
Public Property x() As Integer = 10
Public Shared Property y() As New Customer() With { .Name = "Bob" }
```

__Nebenbei.__ Wenn eine automatisch implementierte Eigenschaft initialisiert wird, wird Sie über die-Eigenschaft und nicht über das zugrunde liegende Feld initialisiert. Das heißt, dass über schreibende Eigenschaften die Initialisierung bei Bedarf abfangen können.

Arrayinitialisierer sind für automatisch implementierte Eigenschaften zulässig, mit dem Unterschied, dass es keine Möglichkeit gibt, die Array Begrenzungen explizit anzugeben.  Zum Beispiel:

```vb
' Valid
Property x As Integer() = {1, 2, 3}
Property y As Integer(,) = {{1, 2, 3}, {12, 13, 14}, {11, 10, 9}}

' Invalid
Property x4(5) As Short
```

### <a name="iterator-properties"></a>Iteratoreigenschaften

Eine *iteratoreigenschaft* ist eine Eigenschaft mit dem `Iterator`-Modifizierer. Sie wird aus demselben Grund verwendet, aus dem eine Iteratormethode (Abschnitts [Iteratormethoden](statements.md#iterator-methods)) verwendet wird. Dies ist eine bequeme Methode zum Generieren einer Sequenz, die von der `For Each`-Anweisung verwendet werden kann. Der `Get`-Accessor einer iteratoreigenschaft wird auf die gleiche Weise interpretiert wie eine Iteratormethode.

Eine iteratoreigenschaft muss über einen expliziten `Get`-Accessor verfügen, und ihr Typ muss `IEnumerator` oder `IEnumerable` oder `IEnumerator(Of T)` oder `IEnumerable(Of T)` für einige `T` sein.

Im folgenden finden Sie ein Beispiel für eine iteratoreigenschaft:

```vb
Class Family
    Property Daughters As New List(Of String) From {"Beth", "Diane"}
    Property Sons As New List(Of String) From {"Abe", "Carl"}

    ReadOnly Iterator Property Children As IEnumerable(Of String)
        Get
            For Each name In Daughters : Yield name : Next
            For Each name In Sons : Yield name : Next
        End Get
    End Property
End Class

Module Module1
    Sub Main()
        Dim x As New Family
        For Each c In x.Children
            Console.WriteLine(c) ' prints Beth, Diane, Abe, Carl
        Next
    End Sub
End Module
```

## <a name="operators"></a>Operatoren

*Operatoren* sind Methoden, die die Bedeutung eines vorhandenen Visual Basic Operators für die enthaltende Klasse definieren. Wenn der-Operator in einem Ausdruck auf die-Klasse angewendet wird, wird der-Operator in einen aufzurufenden Operator in der-Klasse kompiliert. Die Definition eines Operators für eine Klasse wird auch als *überladen* des Operators bezeichnet.

```antlr
OperatorDeclaration
    : Attributes? OperatorModifier* 'Operator' OverloadableOperator
      OpenParenthesis ParameterList CloseParenthesis
      ( 'As' Attributes? TypeName )? LineTerminator
      Block?
      'End' 'Operator' StatementTerminator
    ;

OperatorModifier
    : 'Public' | 'Shared' | 'Overloads' | 'Shadows' | 'Widening' | 'Narrowing'
    ;

OverloadableOperator
    : '+' | '-' | '*' | '/' | '\\' | '&' | 'Like' | 'Mod' | 'And' | 'Or' | 'Xor'
    | '^' | '<' '<' | '>' '>' | '=' | '<' '>' | '>' | '<' | '>' '=' | '<' '='
    | 'Not' | 'IsTrue' | 'IsFalse' | 'CType'
    ;
```

Es ist nicht möglich, einen Operator zu überladen, der bereits vorhanden ist. in der Praxis gilt dies hauptsächlich für Konvertierungs Operatoren. Beispielsweise ist es nicht möglich, die Konvertierung von einer abgeleiteten Klasse in eine Basisklasse zu überladen:

```vb
Class Base
End Class

Class Derived
    ' Cannot redefine conversion from Derived to Base,
    ' conversion will be ignored.
    Public Shared Widening Operator CType(s As Derived) As Base
        ...
    End Operator
End Class
```

Operatoren können auch im allgemeinen Sinn des Worts überladen werden:

```vb
Class Base
    Public Shared Widening Operator CType(b As Base) As Integer
        ...
    End Operator

    Public Shared Narrowing Operator CType(i As Integer) As Base
        ...
    End Operator
End Class
```

Operator Deklarationen fügen Namen nicht explizit dem Deklarations Bereich des enthaltenden Typs hinzu. Allerdings deklarieren Sie implizit eine entsprechende Methode, beginnend mit den Zeichen "op_". In den folgenden Abschnitten werden die entsprechenden Methodennamen mit den einzelnen Operatoren aufgelistet.

Es gibt drei Klassen von Operatoren, die definiert werden können: unäre Operatoren, binäre Operatoren und Konvertierungs Operatoren. Alle Operator Deklarationen haben bestimmte Einschränkungen gemeinsam:

* Operator Deklarationen müssen immer `Public` und `Shared` sein. Der `Public`-Modifizierer kann in Kontexten ausgelassen werden, in denen der-Modifizierer angenommen wird.

* Die Parameter eines Operators können nicht `ByRef`, `Optional` oder `ParamArray` deklariert werden.

* Der Typ mindestens eines der Operanden oder der Rückgabewert muss der Typ sein, der den Operator enthält.

* Für Operatoren ist keine Funktions Rückgabe Variable definiert. Daher muss die `Return`-Anweisung verwendet werden, um Werte aus einem Operator Text zurückzugeben.

Die einzige Ausnahme dieser Einschränkungen gilt für Werttypen, die auf NULL festgelegt werden können. Da auf NULL festleg Bare Werttypen keine tatsächliche Typdefinition aufweisen, kann ein Werttyp benutzerdefinierte Operatoren für die Werte zulässt-Version des Typs deklarieren. Wenn Sie bestimmen, ob ein Typ einen bestimmten benutzerdefinierten Operator deklarieren kann, werden die `?`-Modifizierer zuerst aus allen Typen gelöscht, die an der Deklaration für die Gültigkeits Überprüfung beteiligt sind. Diese Lockerung gilt nicht für den Rückgabetyp der Operatoren "`IsTrue`" und "`IsFalse`". Sie müssen weiterhin `Boolean`, nicht jedoch `Boolean?` zurückgeben.

Die Rangfolge und Assoziativität eines Operators können nicht durch eine Operator Deklaration geändert werden.

__Nebenbei.__ Operatoren haben bei der Zeilen Platzierung die gleiche Einschränkung wie bei den Unterroutinen. Die beginnende Anweisung, End-Anweisung und Block müssen alle am Anfang einer logischen Zeile angezeigt werden.


### <a name="unary-operators"></a>Unäre Operatoren

Die folgenden unären Operatoren können überladen werden:

* Der unäre Plus-Operator `+` (entsprechende Methode: `op_UnaryPlus`)

* Der unäre Minus Operator `-` (entsprechende Methode: `op_UnaryNegation`)

* Der logische `Not`-Operator (entsprechende Methode: `op_OnesComplement`)

* Die Operatoren "`IsTrue`" und "`IsFalse`" (entsprechende Methoden: `op_True`, `op_False`)

Alle überladenen unären Operatoren müssen einen einzelnen Parameter des enthaltenden Typs annehmen und können einen beliebigen Typ zurückgeben, mit Ausnahme von `IsTrue` und `IsFalse`, die `Boolean` zurückgeben müssen. Wenn der enthaltende Typ ein generischer Typ ist, müssen die Typparameter den Typparametern des enthaltenden Typs entsprechen. Ein auf ein Objekt angewendeter

```vb
Structure Complex
    ...

    Public Shared Operator +(v As Complex) As Complex
        Return v
    End Operator
End Structure
```

Wenn ein Typ einen der `IsTrue` oder `IsFalse` über lädt, muss er auch die andere überladen. Wenn nur eine überladen ist, ergibt sich ein Kompilierzeitfehler.

__Nebenbei.__ `IsTrue` und `IsFalse` sind keine reservierten Wörter.

### <a name="binary-operators"></a>Binäre Operatoren

Die folgenden binären Operatoren können überladen werden:

* Die Addition `+`, Subtraktion `-`, Multiplikation `*`, Division `/`, ganzzahlige Division `\`, Modulo `Mod`-und exponentiations-`^`-Operatoren (entsprechende Methode: `op_Addition`, `op_Subtraction`, `op_Multiply`, 0, 1, @no_ _T-12, 3)

* Die relationalen Operatoren `=`, `<>`, `<`, `>`, `<=`, `>=` (entsprechende Methoden: `op_Equality`, `op_Inequality`, `op_LessThan`, `op_GreaterThan`, 0, 1). __Nebenbei.__ Der Gleichheits Operator kann überladen werden, aber der Zuweisungs Operator (nur in Zuweisungs Anweisungen verwendet) kann nicht überladen werden.

* Der `Like`-Operator (entsprechende Methode: `op_Like`)

* Der Verkettungs Operator `&` (entsprechende Methode: `op_Concatenate`).

* Die logischen Operatoren "`And`", "`Or`" und "`Xor`" (entsprechende Methoden: `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`)

* Die Shift-Operatoren `<<` und `>>` (entsprechende Methoden: `op_LeftShift`, `op_RightShift`)

Alle überladenen binären Operatoren müssen den enthaltenden Typ als einen der Parameter verwenden. Wenn der enthaltende Typ ein generischer Typ ist, müssen die Typparameter den Typparametern des enthaltenden Typs entsprechen. Die Shift-Operatoren schränken diese Regel weiter ein, sodass der erste Parameter vom enthaltenden Typ sein muss. der zweite Parameter muss immer den Typ `Integer` aufweisen.

Die folgenden binären Operatoren müssen paarweise deklariert werden:

* Operator `=` und Operator `<>`

* Operator `>` und Operator `<`

* Operator `>=` und Operator `<=`

Wenn eines der Paare deklariert ist, muss das andere ebenfalls mit übereinstimmenden Parameter-und Rückgabe Typen deklariert werden, andernfalls tritt ein Kompilierzeitfehler auf. (__Hinweis:__ Der Zweck der Verwendung von paarweise zugeordneten Deklarationen relationaler Operatoren besteht darin, zumindest einen minimalen Grad an logischer Konsistenz in überladenen Operatoren sicherzustellen.)

Im Gegensatz zu den relationalen Operatoren wird das Überladen von Operatoren der Division und der ganzzahligen Division dringend davon abgeraten, auch wenn kein Fehler vorliegt. (__Hinweis:__ Im Allgemeinen sollten die beiden Arten der Division ganz eindeutig sein: ein Typ, der die Division unterstützt, ist entweder eine Ganzzahl (in diesem Fall sollte `\`) oder nicht (in diesem Fall sollte `/` unterstützt werden). Wir haben uns als Fehler beim Definieren beider Operatoren bewährt, aber da ihre Sprachen in der Regel nicht zwischen zwei Arten von Abteilungen unterscheiden, wie Visual Basic, haben wir gespürt, dass es am sichersten ist, die Übung zuzulassen, aber dringend abzuraten.)

Verbund Zuweisungs Operatoren können nicht direkt überladen werden. Stattdessen verwendet der Verbund Zuweisungs Operator den überladenen Operator, wenn der entsprechende binäre Operator überladen wird. Zum Beispiel:

```vb
Structure Complex
    ...

    Public Shared Operator +(x As Complex, y As Complex) _
        As Complex
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim c1, c2 As Complex
        ' Calls the overloaded + operator
        c1 += c2
    End Sub
End Module
```

### <a name="conversion-operators"></a>Konvertierungsoperatoren

Konvertierungs Operatoren definieren neue Konvertierungen zwischen Typen. Diese neuen Konvertierungen werden als *benutzerdefinierte Konvertierungen*bezeichnet. Ein Konvertierungs Operator konvertiert von einem Quelltyp, der durch den Parametertyp des Konvertierungs Operators angegeben ist, in einen Zieltyp, der durch den Rückgabetyp des Konvertierungs Operators angegeben wird. Konvertierungen müssen entweder erweitert oder einschränkend klassifiziert werden. Eine Konvertierungs Operator Deklaration, die das Schlüsselwort "`Widening`" enthält, führt eine benutzerdefinierte erweiternde Konvertierung ein (entsprechende Methode: `op_Implicit`). Eine Konvertierungs Operator Deklaration, die das Schlüsselwort "`Narrowing`" enthält, führt eine benutzerdefinierte einschränkende Konvertierung ein (entsprechende Methode: `op_Explicit`).

Im Allgemeinen sollten benutzerdefinierte erweiternde Konvertierungen so entworfen werden, dass nie Ausnahmen ausgelöst werden, und es werden niemals Informationen verloren. Wenn eine benutzerdefinierte Konvertierung Ausnahmen verursachen kann (z. b. weil das Quell Argument außerhalb des gültigen Bereichs liegt) oder Informationen verloren geht (z. b. das Verwerfen von großen Bits), sollte diese Konvertierung als einschränkende Konvertierung definiert werden. Im folgenden Beispiel

```vb
Structure Digit
    Dim value As Byte

    Public Sub New(value As Byte)
        if value < 0 OrElse value > 9 Then Throw New ArgumentException()
        Me.value = value
    End Sub

    Public Shared Widening Operator CType(d As Digit) As Byte
        Return d.value
    End Operator

    Public Shared Narrowing Operator CType(b As Byte) As Digit
        Return New Digit(b)
    End Operator
End Structure
```

die Konvertierung von `Digit` in `Byte` ist eine erweiternde Konvertierung, da Sie nie Ausnahmen auslöst oder Informationen verliert, aber die Konvertierung von `Byte` in `Digit` ist eine einschränkende Konvertierung, da `Digit` nur eine Teilmenge der möglichen Werte von darstellen kann. `Byte`.

Im Gegensatz zu allen anderen Typmembern, die überladen werden können, enthält die Signatur eines Konvertierungs Operators den Zieltyp der Konvertierung. Dies ist der einzige Typmember, für den der Rückgabetyp an der Signatur teilnimmt. Die erweiternde oder einschränkende Klassifizierung eines Konvertierungs Operators ist jedoch nicht Teil der Signatur des Operators. Daher kann eine Klasse oder Struktur nicht sowohl einen erweiterungskonvertierungs Operator als auch einen einschränkenden Konvertierungs Operator mit denselben Quell-und Zieltyp deklarieren.

Ein benutzerdefinierter Konvertierungs Operator muss entweder in oder aus dem enthaltenden Typ konvertieren. es ist z. b. möglich, dass eine Klasse `C` eine Konvertierung von `C` in `Integer` und von `Integer` in `C`, aber nicht von `Integer` in `Boolean` definiert. Wenn der enthaltende Typ ein generischer Typ ist, müssen die Typparameter den Typparametern des enthaltenden Typs entsprechen. Außerdem ist es nicht möglich, eine intrinsische (d. h. Nichtbenutzer definierte) Konvertierung neu zu definieren. Folglich kann ein Typ keine Konvertierung deklarieren, wobei Folgendes gilt:

* Der Quelltyp und der Zieltyp sind identisch.

* Sowohl der Quelltyp als auch der Zieltyp sind nicht der Typ, der den Konvertierungs Operator definiert.

* Der Quelltyp oder der Zieltyp ist ein Schnittstellentyp.

* Der Quelltyp und die Zieltypen sind durch Vererbung verknüpft (einschließlich `Object`).

Die einzige Ausnahme dieser Regeln gilt für Werttypen, die auf NULL festgelegt werden können. Da auf NULL festleg Bare Werttypen keine tatsächliche Typdefinition aufweisen, kann ein Werttyp benutzerdefinierte Konvertierungen für die Werte zulässt-Version des Typs deklarieren. Wenn Sie bestimmen, ob ein Typ eine bestimmte benutzerdefinierte Konvertierung deklarieren kann, werden die `?`-Modifizierer zuerst aus allen Typen gelöscht, die an der Deklaration für die Gültigkeits Überprüfung beteiligt sind. Daher ist die folgende Deklaration gültig, da `S` eine Konvertierung von `S` in `T` definieren kann:

```vb
Structure T
    ...
End Structure

Structure S
    Public Shared Widening Operator CType(ByVal v As S?) As T
    ...
    End Operator
End Structure
```

Die folgende Deklaration ist jedoch nicht gültig, da die Struktur `S` keine Konvertierung von `S` in `S` definieren kann:

```vb
Structure S
    Public Shared Widening Operator CType(ByVal v As S) As S?
        ...
    End Operator
End Structure
```

### <a name="operator-mapping"></a>Operator Zuordnung

Da der Satz von Operatoren, der Visual Basic unterstützt, möglicherweise nicht genau mit dem Satz von Operatoren übereinstimmt, die andere Sprachen auf der .NET Framework, werden einige Operatoren bei der Definition oder Verwendung speziell anderen Operatoren zugeordnet. Dies gilt insbesondere in folgenden Fällen:

* Wenn Sie einen integralen Divisions Operator definieren, wird automatisch ein regulärer Divisions Operator definiert (nur aus anderen Sprachen verwendbar), der den integralen Divisions Operator aufruft.

* Das Überladen der Operatoren "`Not`", "`And`" und "`Or`" überlastet nur den bitweisen Operator aus der Perspektive anderer Sprachen, die zwischen logischen und bitweisen Operatoren unterscheiden.

* Eine Klasse, die nur die logischen Operatoren in einer Sprache überlädt, die zwischen logischen und bitweisen Operatoren unterscheidet (d. h. eine Sprache, die `op_LogicalNot`, `op_LogicalAnd` und `op_LogicalOr` für `Not`, `And` bzw. `Or` verwendet), wird Ihr logisches Operatoren, die den Visual Basic logischen Operatoren zugeordnet sind. Wenn sowohl die logischen als auch die bitweisen Operatoren überladen werden, werden nur die bitweisen Operatoren verwendet.

* Durch Überladen der Operatoren `<<` und `>>` werden nur die signierten Operatoren aus der Perspektive anderer Sprachen überladen, die zwischen signierten und unsignierten Schiebe Operatoren unterscheiden.

* Bei einer Klasse, die nur einen Ganzzahl ohne Vorzeichen Shift-Operator über lädt, wird der unsignierte Shift-Operator dem entsprechenden Visual Basic Shift-Operator zugeordnet. Wenn sowohl ein Ganzzahl ohne Vorzeichen-als auch ein signed Shift-Operator überladen wird, wird nur der signierte Schiebe Operator verwendet.
