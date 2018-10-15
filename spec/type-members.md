# <a name="type-members"></a>Typmember

Typmember Speicherorte und ausführbaren Code zu definieren. Sie können Methoden, Konstruktoren, Ereignisse, Konstanten, Variablen und Eigenschaften sein.

## <a name="interface-method-implementation"></a>Schnittstellenimplementierung-Methode

Methoden, Ereignisse und Eigenschaften können Schnittstellenmember implementieren. Um ein Schnittstellenmember zu implementieren, eine Memberdeklaration gibt an, die `Implements` Schlüsselwort und eine oder mehrere Schnittstellen-Member aufgeführt.

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

Methoden und Eigenschaften, die Schnittstellenmember zu implementieren sind implizit `NotOverridable` , sofern deklariert, um `MustOverride`, `Overridable`, oder einen anderen Member überschreiben. Es ist ein Fehler für ein Element, das Implementieren eines Schnittstellenmembers werden `Shared`. Einen Member zugegriffen hat keine Auswirkungen auf ihre Möglichkeit, die Schnittstellenmember implementieren.

Eine Implementierung der Schnittstelle gültig ist muss die Implementierungsliste den enthaltenden Typ eine Schnittstelle benennen, die einen kompatiblen Member enthält. Ein kompatibles Element ist eine, deren Signatur der Signatur des implementierenden Members entspricht. Wenn eine generische Schnittstelle implementiert wird, wird das Typargument in der Implements-Klausel beim Überprüfen der Hardwarekompatibilität in die Signatur ersetzt. Zum Beispiel:

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

Wenn ein Ereignis mit deklariert ein Delegattyp ein Schnittstellenereignisses implementiert wird, und dann eine kompatible-Ereignis ist eines, dessen zugrunde liegenden Delegattyp vom gleichen Typ ist. Andernfalls verwendet das Ereignis den Typ des Delegaten aus der Schnittstellenereignis, das implementiert wird. Wenn einem solchen mehrere Schnittstellenereignisse implementiert, müssen alle Schnittstellenereignisse den gleichen zugrunde liegenden Delegattyp. Zum Beispiel:

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

Ein Schnittstellenmember in der Implementierungsliste wird, verwenden einen Namen, einem Punkt und ein Bezeichner angegeben. Der Typname muss eine Schnittstelle in der Implementierungsliste oder eine Basisschnittstelle einer Schnittstelle in der Implementierungsliste werden, und der Bezeichner muss ein Mitglied der angegebenen Schnittstelle sein. Ein einzelnes Element kann mehr als eine übereinstimmende Schnittstellenmember zu implementieren.

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

Wenn der Schnittstellenmember implementiert wird aufgrund nicht verfügbar in allen explizit implementierte Schnittstellen mehrfachvererbung-Schnittstelle ist, muss der implementierende Member explizit eine Basisschnittstelle verweisen, auf der das Element verfügbar ist. Z. B. wenn `I1` und `I2` -Member enthalten `M`, und `I3` erbt `I1` und `I2`, ein Typ implementieren `I3` implementieren `I1.M` und `I2.M`. Wenn eine Schnittstelle vererbte Member überschattet mehrfach, müssen einen Implementierungstyp die geerbten Member und die Mitglieder, die sie shadowing implementieren.

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

Wenn die enthaltende Schnittstelle des Schnittstellenmembers implementiert werden generisch ist, wird vom selben Typ Argumente, wie die Schnittstelle implementiert wird, angegeben werden muss. Zum Beispiel:

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

Methoden enthalten, die ausführbaren Anweisungen eines Programms.

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

Methoden, die eine optionale Liste von Parametern und einer optionalen Rückgabewert verfügen, freigegebene oder nicht freigegeben. Freigegebene Methoden über die Klasse oder Instanzen der Klasse zugegriffen werden. Nicht freigegebene Methoden Instanzenmethoden, so genannte durch Instanzen der Klasse zugegriffen werden. Das folgende Beispiel zeigt eine Klasse `Stack` , die mehrere freigegebene Methoden aufweist (`Clone` und `Flip`), mehrere Methoden und Instanzenmethoden (`Push`, `Pop`, und `ToString`):

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

Methoden können überladen werden, was bedeutet, dass mehrere Methoden den gleichen Namen haben können, solange sie eindeutige Signaturen aufweisen. Die Signatur einer Methode besteht aus der die Anzahl und Typen ihrer Parameter. Die Signatur einer Methode umfasst nicht ausdrücklich den Rückgabetyp oder Parametermodifizierern, z. B. Optional "," ByRef "oder" ParamArray. Das folgende Beispiel zeigt eine Klasse mit einer Reihe von Überladungen:

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

Die Ausgabe des Programms lautet:

```vb
F()
F(Integer)
F(Object)
F(Integer, Integer)
F(Integer())
G(String)
G(String, Optional String)
```

Überladungen, die nur durch optionale Parameter unterscheiden, können für die "versionsverwaltung" von Bibliotheken verwendet werden. Version 1 einer Bibliothek könnte z. B. eine Funktion mit optionalen Parametern enthalten:

```vb
Sub fopen(fileName As String, Optional accessMode as Integer = 0)
```

Klicken Sie dann das Hinzufügen von weiteren optionalen Parameters "Password" v2 der Bibliothek möchte, und zu tun, ohne wichtige Source-Kompatibilität (also Anwendungen, die zum Auswählen des v1 verwendet neu kompiliert werden können) und ohne binäre Kompatibilität (also Anwendungen, die auf verwendet, möchte Verweis v1 können nun mit v2 ohne erneute Kompilierung zu verweisen). Dies ist eine v2 wird:

```vb
Sub fopen(file As String, mode as Integer)
Sub fopen(file As String, Optional mode as Integer = 0, Optional pword As String = "")
```

Beachten Sie, dass optionale Parameter in eine öffentliche API nicht CLS-kompatibel sind. Sie können jedoch mindestens von Visual Basic und C# 4 und f# verwendet werden.



### <a name="regular-async-and-iterator-method-declarations"></a>Reguläre, Async- und Iterator-Methodendeklarationen

Es gibt zwei Arten von Methoden: *Unterroutinen*, die keine Werte, zurückgeben und *Funktionen*, führen Sie die. Der Text und `End` Konstrukt einer Methode kann nur ausgelassen werden, wenn die Methode in einer Schnittstelle definiert wird, oder über die `MustOverride` Modifizierer. Wenn kein Rückgabetyp, auf eine Funktion angegeben wird und strenge Semantik verwendet wird, tritt ein Fehler während der Kompilierung; Andernfalls ist der Typ implizit `Object` oder den Typ der Methode Typzeichen. Die Zugriffsdomäne von den Rückgabetyp und Parametertypen einer Methode muss es sich um die identisch oder eine Obermenge der Zugriffsdomäne von der Methode selbst sein.

Ein __normale Methode__ ist ein URI mit keiner `Async` noch `Iterator` Modifizierer. Es kann eine Unterroutine oder Funktion sein. Abschnitt [reguläre Methoden](statements.md#regular-methods) erläutert, was geschieht, wenn eine normale Methode aufgerufen wird.

Ein __Iteratormethode__ ist ein URI mit der `Iterator` -Modifizierer und keine `Async` Modifizierer. Es muss eine Funktion, und der Rückgabetyp muss `IEnumerator`, `IEnumerable`, oder `IEnumerator(Of T)` oder `IEnumerable(Of T)` für einige `T`, und es muss keine `ByRef` Parameter. Abschnitt [Iteratormethoden](statements.md#iterator-methods) erläutert, was geschieht, wenn eine Iteratormethode aufgerufen wird.

Ein __Async-Methode__ ist ein URI mit der `Async` -Modifizierer und keine `Iterator` Modifizierer. Es muss entweder eine Unterroutine oder eine Funktion mit Rückgabetyp `Task` oder `Task(Of T)` für einige `T`, und Sie müssen keine `ByRef` Parameter. Abschnitt [Async-Methoden](statements.md#async-methods) erläutert, was geschieht, wenn eine asynchrone Methode aufgerufen wird.

Es ist ein Fehler während der Kompilierung, wenn eine Methode nicht mit einer der folgenden drei Arten von Methode ist.

Unterroutine und Funktionsdeklarationen sind spezielle, da jede ihren Anfang und Ende-Anweisungen am Anfang einer logischen Zeile beginnen müssen. Darüber hinaus wird der Text von einer nicht -`MustOverride` Unterroutine oder Funktion-Deklaration muss am Anfang einer logischen Zeile beginnen. Zum Beispiel:

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


### <a name="external-method-declarations"></a>Externe Methodendeklarationen

Eine Deklaration für die externe Methode führt eine neue Methode, deren Implementierung für die Anwendung extern bereitgestellt wird.

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

Da eine externe Methodendeklaration keine Implementierungen bietet, verfügt er über keinen Methodentext oder `End` zu erstellen. Externe Methoden sind implizit freigegeben, möglicherweise nicht verfügen über Typparameter, und möglicherweise nicht verarbeiten von Ereignissen oder Implementieren von Schnittstellenmembern. Wenn kein Rückgabetyp, auf eine Funktion angegeben wird und strenge Semantik verwendet wird, tritt ein Fehler während der Kompilierung. Andernfalls ist der Typ implizit `Object` oder den Typ der Methode Typzeichen. Die Zugriffsdomäne von den Rückgabetyp und Parametertypen von einer externen Methode muss es sich um die identisch oder eine Obermenge der Zugriffsdomäne von der externen Methode selbst sein.

Die Bibliothek-Klausel einer Deklaration für eine externe Methode gibt den Namen der externen Datei, die die Methode implementiert. Die optionale Alias-Klausel ist eine Zeichenfolge, der angibt, die numerische Ordinalzahl (das Präfix einer `#` Zeichen) oder den Namen der Methode in der externen Datei. Ein einzelnen Zeichen Set-Modifizierer kann auch angegeben werden, der bestimmt, dass des Zeichensatzes, das zum Marshallen von Zeichenfolgen während eines Aufrufs an die externe Methode verwendet. Die `Unicode` Modifizierer marshallt alle Zeichenfolgen in Unicode-Werten, die `Ansi` Modifizierer marshallt alle Zeichenfolgen in ANSI-Werte, und die `Auto` Modifizierer marshallt Zeichenfolgen gemäß der .NET Framework-Regeln, die basierend auf den Namen der Methode oder der Aliasname angegeben. Wenn kein Modifizierer angegeben ist, wird standardmäßig `Ansi`.

Wenn `Ansi` oder `Unicode` angegeben ist, wird der Name der Methode in der externen Datei ohne weitere Änderungen gesucht wird. Wenn `Auto` angegeben ist, wird die Methode Namenssuche, die von der Plattform abhängig ist. Wenn die Plattform werden ANSI (z. B. Windows 95, Windows 98, Windows ME) betrachtet wird, wird der Name der Methode ohne weitere Änderungen nachgeschlagen. Wenn die Suche fehlschlägt, eine `A` angefügt wird und die Suche nach erneuter Versuch gestartet. Wenn die Plattform als Unicode (z. B. Windows NT, Windows 2000, Windows XP), gilt ein `W` angefügt wird und der Name gesucht wird. Wenn die Suche ein Fehler auftritt, wird die Suche erneut ohne versucht die `W`. Zum Beispiel:

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

Datentypen, die an externe Methoden übergeben werden, werden entsprechend den .NET Framework Data marshalling Konventionen mit einer Ausnahme gemarshallt. String-Variablen, die als Wert übergeben werden (d. h. `ByVal x As String`) gemarshallt werden der OLE-Automatisierung BSTR-Typ, und Änderungen an der BSTR in der externen Methode wieder im Zeichenfolgenargument widergespiegelt werden. Grund hierfür ist der Typ `String` in externen Methoden ist änderbar und diese spezielle marshalling imitiert dieses Verhalten. String-Parameter, die als Verweis übergeben werden (d. h. `ByRef x As String`) werden als Zeiger auf den OLE-Automatisierung BSTR-Typ gemarshallt. Es ist möglich, überschreiben Sie diese speziellen Verhaltensweisen durch Angabe der `System.Runtime.InteropServices.MarshalAsAttribute` Attribut für den Parameter.

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


### <a name="overridable-methods"></a>Überschreibbare Methoden

Die `Overridable` Modifizierer gibt an, dass eine Methode überschrieben werden kann. Die `Overrides` Modifizierer gibt an, dass eine Methode eine Basistyp überschreibbare Methode überschreibt, die die gleiche Signatur aufweist. Die `NotOverridable` Modifizierer gibt an, dass eine überschreibbare Methode nicht weiter überschrieben werden kann. Die `MustOverride` Modifizierer gibt an, dass eine Methode in abgeleiteten Klassen überschrieben werden muss.

Bestimmte Kombinationen dieser Modifizierer sind nicht gültig:

* `Overridable` und `NotOverridable` gegenseitig und können nicht kombiniert werden.

* `MustOverride` impliziert `Overridable` (und damit sie nicht angeben) und können nicht kombiniert werden, mit `NotOverridable`.

* `NotOverridable` können nicht kombiniert werden, mit `Overridable` oder `MustOverride` und muss zusammen mit `Overrides`.

* `Overrides` impliziert `Overridable` (und damit sie nicht angeben) und können nicht kombiniert werden, mit `MustOverride`.

Es gibt auch zusätzliche Beschränkungen auf überschreibbaren Methoden:

* Ein `MustOverride` Methode darf keinen Methodentext oder `End` erstellen, können keine andere Methode überschreiben und darf nur in `MustInherit` Klassen.

* Wenn eine Methode angibt `Overrides` und es gibt keine übereinstimmende Basismethode außer Kraft zu setzenden ein Fehler während der Kompilierung auftritt. Eine überschreibende Methode kann keine angeben `Shadows`.

* Eine Methode kann keine andere Methode überschreiben, ist die überschreibende Methode Zugriffsdomäne nicht entspricht die Zugriffsdomäne von der Methode überschrieben wird. Die einzige Ausnahme ist, die eine Methode zum Überschreiben einer `Protected Friend` -Methode in einer anderen Assembly, die nicht `Friend` Zugriff angeben muss `Protected` (nicht `Protected Friend`).

* `Private` Methoden möglicherweise nicht `Overridable`, `NotOverridable`, oder `MustOverride`, noch können sie andere Methoden überschreiben.

* Methoden in `NotInheritable` Klassen können nicht deklariert werden `Overridable` oder `MustOverride`.

Im folgende Beispiel werden die Unterschiede zwischen überschreibbare und nicht überschreibbaren Methoden veranschaulicht:

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

In diesem Beispiel-Klasse `Base` führt eine Methode `F` und `Overridable` Methode `G`. Die Klasse `Derived` führt eine neue Methode `F`, daher shadowing der geerbten `F`, und außerdem überschreibt die geerbte Methode `G`. Das Beispiel führt zur folgenden Ausgabe:

```
Base.F
Derived.F
Derived.G
Derived.G
```

Beachten Sie, dass die Anweisung `b.G()` ruft `Derived.G`, nicht `Base.G`. Dies ist, da der Laufzeittyp der Instanz (die `Derived`) anstelle der Kompilierzeit-Typ der Instanz (d.h. `Base`) bestimmt die Implementierung der tatsächlichen Methode aufrufen.

### <a name="shared-methods"></a>Freigegebene Methoden

Die `Shared` Modifizierer gibt an, dass eine Methode eine *freigegebene Methode*. Eine freigegebene Methode führt keine Vorgänge für eine bestimmte Instanz eines Typs und kann direkt von einem Typ und nicht über eine bestimmte Instanz eines Typs aufgerufen werden. Es ist jedoch gültig ist, eine Instanz zu verwenden, um eine freigegebene Methode zu qualifizieren. Es ist unzulässig, verweisen auf `Me`, `MyClass`, oder `MyBase` in einer freigegebenen Methode. Freigegebene Methoden möglicherweise nicht `Overridable`, `NotOverridable`, oder `MustOverride`, und sie dürfen die Methoden nicht überschreiben. Methoden, die definiert, die in standard-Module und Schnittstellen können keine angeben `Shared`, da sie implizit sind `Shared` bereits.

Eine Methode deklariert werden, in einer Struktur oder Klasse, ohne dass eine `Shared` Modifizierer ist ein *Instanzmethode*. Eine Instanzmethode funktioniert für eine bestimmte Instanz eines Typs. Instanzmethoden kann nur über eine Instanz eines Typs aufgerufen werden und bezieht sich möglicherweise auf die Instanz mithilfe der `Me` Ausdruck.

Das folgende Beispiel veranschaulicht die Regeln für den Zugriff auf freigegebene als auch Instanzmember:

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

Methode `F` zeigt an, dass in ein Instanzmember-Funktion, ein Bezeichner für den Zugriff auf Instanzmember und freigegebene Member verwendet werden kann. Methode `G` zeigt an, dass in einem freigegebenen Funktionsmember, ein Fehler auf einen Instanzmember zuzugreifen, über einen Bezeichner kann. Methode `Main` zeigt, dass in einem Memberzugriffsausdruck Instanzmember über Instanzen zugegriffen werden müssen, aber gemeinsam genutzten Member über Typen oder Instanzen zugegriffen werden kann.

### <a name="method-parameters"></a>Methodenparameter

Ein *Parameter* ist eine Variable, die verwendet werden kann, um Informationen in und aus einer Methode zu übergeben. Parameter einer Methode werden von der Parameterliste der Methode deklariert, besteht aus einem oder mehreren Parametern, die durch Kommas getrennt.

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

Wenn kein Typ für einen Parameter angegeben ist, und strikte Semantik verwendet werden, tritt ein Fehler während der Kompilierung. Andernfalls ist der Standardtyp `Object` oder den Typ des Parameters-Typzeichen. Sogar unter Semantik, wenn ein Parameter enthält einen `As` -Klausel, alle Parameter müssen Typen angeben.

Parameter als Wert, optional, Verweis oder Paramarray-Parameter angegeben werden, indem Sie die Modifizierer `ByVal`, `ByRef`, `Optional`, und `ParamArray`bzw. Ein Parameter, der nicht `ByRef` oder `ByVal` standardmäßig `ByVal`.

Parameternamen definiert und gelten für den gesamten Text der Methode und sind immer öffentlich zugegriffen werden kann. Aufruf einer Methode erstellt eine Kopie, die speziell für diesen Aufruf, der Parameter, und die Argumentliste des Aufrufs weist Werte oder Variablenverweise an die neu erstellte Parameter. Da externe Methodendeklarationen und Delegatdeklarationen ohne Text aufweisen, sind doppelte Parameternamen in Parameterlisten zulässig, aber nicht empfohlen.

Der Bezeichner der Modifizierer auf NULL festlegbare Name folgt möglicherweise `?` , um anzugeben, dass es NULL-Werte zulässt, und auch Namen Arraymodifizierer, um anzugeben, dass die It ein Array ist. Sie können kombiniert werden, z. B. "`ByVal x?() As Integer`". Es ist nicht mit expliziten Arraygrenzen zulässig. auch wenn der Modifizierer auf NULL festlegbare Namen vorhanden ist wird eine `As` Klausel muss vorhanden sein.


#### <a name="value-parameters"></a>Wert-Parametern

Ein *"Value"-Parameter* wird deklariert, mit einem expliziten `ByVal` Modifizierer. Wenn die `ByVal` -Modifizierer wird verwendet, die `ByRef` Modifizierer dürfen nicht angegeben werden. Ein Werteparameter wird mit dem Aufruf des Members der Parameter gehört zu und initialisiert wird, mit dem Wert des Arguments im Aufruf angegeben. Ein Value-Parameter wird bei der Rückgabe des Elements gelöscht.

Eine Methode ist zulässig, einen Wertparameter neue Werte zuweisen. Diese Zuweisungen wirken sich nur auf den Speicherort der lokalen Speicherressource, dargestellt durch den Wertparameter; Sie haben keine Auswirkungen auf das tatsächliche Argument im Methodenaufruf angegeben.

Ein Value-Parameter wird verwendet, wenn der Wert eines Arguments in eine Methode übergeben, und Änderungen an den Parameter sich nicht auf den ursprünglichen Arguments wirken. Ein Value-Parameter verweist auf eine eigene Variable, eine Variable, die sich aus der Variablen des entsprechenden Arguments unterscheidet. Diese Variable wird initialisiert, indem der Wert des entsprechenden Arguments kopiert. Das folgende Beispiel zeigt eine Methode `F` , die über einen Werteparameter, die mit dem Namen verfügt `p`:

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

Das Beispiel erzeugt der folgenden Ausgabe, auch wenn den Value-Parameter `p` geändert wird:

```
pre: a = 1
p = 1
post: a = 1
```

#### <a name="reference-parameters"></a>Verweisparameter

Ein Verweisparameter ist ein Parameter, die mit dem deklariert eine `ByRef` Modifizierer. Wenn die `ByRef` -Modifizierer wird angegeben, die `ByVal` Modifizierer kann nicht verwendet werden. Ein Verweisparameter wird nicht mit einen neuen Speicherort erstellt. Stattdessen stellt ein Verweisparameter die Variable als Argument im Aufruf Methode oder des Konstruktors angegeben. Vom Konzept her ist der Wert eines Verweisparameters immer identisch mit der zugrunde liegenden Variablen.

Verweisparameter agieren in zwei Modi, entweder als *Aliase* oder über *Kopie in Kopie zurück.*

__Aliase.__ Ein Verweisparameter wird verwendet, wenn der Parameter als Alias für ein vom Aufrufer bereitgestellten Argument fungiert. Ein Verweisparameter definiert nicht selbst eine Variable, sondern verweist auf die Variable des entsprechenden Arguments. Änderungen an Verweisparameter Auswirkungen direkt und unmittelbar das entsprechende Argument auf. Das folgende Beispiel zeigt eine Methode `Swap` über zwei Verweisparameter verfügt:

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

Die Ausgabe des Programms lautet:

```
pre: x = 1, y = 2
post: x = 2, y = 1
```

Für den Aufruf der Methode `Swap` in Klasse `Main`, `a` stellt `x,` und `b` stellt `y`. Der Aufruf ist daher die Auswirkungen der Tauschen der Werte der `x` und `y`.

In eine Methode, nimmt die Parameter verweisen ist es möglich, dass mehrere Namen am gleichen Speicherort dargestellt:

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

Im Beispiel der Aufruf der Methode `F` in `G` übergibt einen Verweis auf `s` für beide `a` und `b`. Daher ist es bei diesen Aufruf, der die Namen `s`, `a`, und `b` verweisen alle auf den gleichen Speicherort aus, und die drei alle Zuweisungen ändern Sie die Instanzvariable `s`.

__Kopieren Sie sich an Kopie zurück.__ Wenn der Typ der Variablen an Verweisparameter übergeben werden nicht mit dem Typ der Verweisparameter, kompatibel ist oder wenn eine nicht-Variable (z. B. eine Eigenschaft) als Argument an einen Verweisparameter übergeben wird oder ist der Aufruf spät gebunden, und klicken Sie dann auf eine temporäre Variable zugeordnet und der Verweisparameter übergeben. Der Wert, der übergeben wird, bevor die Methode aufgerufen wird, und wieder die ursprüngliche Variable kopiert werden (sofern vorhanden und nicht schreibgeschützt ist) beim Beenden der Methode in diese temporäre Variable kopiert. Daher ein Verweisparameter darf keine unbedingt einen Verweis auf den genauen Speicher, der die Variable übergeben werden, und alle Änderungen an der Verweisparameter werden in der Variablen nicht wirksam, bis die Methode beendet wird. Zum Beispiel:

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

Bei den ersten Aufruf des `F`, eine temporäre Variable erstellt wird und der Wert der Eigenschaft `G` zugewiesen ist, und übergeben Sie in `F`. Bei der Rückgabe von `F`, der Wert in der temporären Variablen wird zugewiesen, an die Eigenschaft des `G`. Im zweiten Fall wird eine andere temporäre Variable erstellt und der Wert der `d` zugewiesen ist, und übergeben Sie in `F`. Bei der Rückgabe von `F`, der Wert in der temporären Variablen wieder in den Typ der Variablen umgewandelt wird `Derived`, zugewiesen `d`. Da der Wert zurück übergeben werden nicht konvertiert werden kann `Derived`, zur Laufzeit eine Ausnahme ausgelöst.

#### <a name="optional-parameters"></a>Optionale Parameter

Ein optionaler Parameter ist deklariert, mit der `Optional` Modifizierer. Parameter, die einen optionalen Parameter in der Liste der formalen Parameter folgen müssen ebenfalls optional sein; Fehler beim Angeben der `Optional` Modifizierer in den folgenden Parametern löst einen Fehler während der Kompilierung. Ein optionaler Parameter für einige geben nullable-Typ `T?` oder NULL-Typ `T` muss einen konstanten Ausdruck angeben `e` als Standardwert verwendet werden soll, kein Argument angegeben ist. Wenn `e` ergibt `Nothing` von Typ "Object" wird der Standardwert von der *Parametertyp* wird als Standardwert für den Parameter verwendet werden. Andernfalls `CType(e, T)` muss ein konstanter Ausdruck sein, und es ist als der Standardwert für den Parameter.

Optionale Parameter sind die einzige Situation, in der ein Initialisierer für einen Parameter gültig ist. Die Initialisierung erfolgt immer als Teil der Aufrufausdruck, nicht innerhalb des Methodentexts selbst.

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

Die Ausgabe des Programms lautet:

```
x = 10, y = 20
x = 30, y = 40
```

Optionale Parameter können nicht in Deklarationen für Delegat- oder Ereignisparameter noch in Lambda-Ausdrücke angegeben werden.

#### <a name="paramarray-parameters"></a>ParamArray-Parameter

`ParamArray` Parameter werden deklariert, mit der `ParamArray` Modifizierer. Wenn die `ParamArray` Modifizierer vorhanden ist, die `ByVal` Modifizierer muss angegeben werden, und keine anderen Parameter können Sie die `ParamArray` Modifizierer. Die `ParamArray` Typ des Parameters muss ein eindimensionales Array sein, und es muss der letzte Parameter in der Parameterliste sein.

Ein `ParamArray` Parameter darstellt, eine unbestimmte Anzahl von Parametern des Typs von der `ParamArray`. In der Methode selbst eine `ParamArray` Parameter als seinem deklarierten Typ behandelt und besitzt keine spezielle Semantik. Ein `ParamArray` -Parameter ist implizit optional, Standardwert eine leere eindimensionalen Arrays des Typs von der `ParamArray`.

Ein `ParamArray` können die Argumente in eine von zwei Arten in einem Methodenaufruf angegeben werden:

* Das Argument für die ein `ParamArray` kann ein einzelner Ausdruck eines Typs, die erweitert werden kann, werden die `ParamArray` Typ. In diesem Fall die `ParamArray` verhält sich genau wie ein Value-Parameter.

* Der Aufruf kann auch angeben, NULL oder mehr Argumente für die `ParamArray`, wobei jedes Argument einen Ausdruck eines Typs, der implizit in den Typ des Elements, der die `ParamArray`. In diesem Fall der Aufruf erstellt eine Instanz der `ParamArray` Typ mit einer Länge, die Anzahl der Argumente, die Elemente des Arrays mit den Werten des angegebenen Arguments Instanz, initialisiert und verwendet das neu erstellte Array-Instanz als die tatsächliche entsprechen Argument.

Mit Ausnahme von ermöglichen, dass eine Variable Anzahl von Argumenten in einem Aufruf einer `ParamArray` entspricht genau eine Value-Parameter des gleichen Typs wie das folgende Beispiel veranschaulicht.

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

Das Beispiel erzeugt die Ausgabe

```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

Der erste Aufruf der `F` einfach das Array übergibt `a` als ein Value-Parameter. Der zweite Aufruf von `F` automatisch erstellt ein Array mit vier Elementen mit den Werten des angegebenen Elements und übergibt diese Arrayinstanz als ein Value-Parameter. Entsprechend der dritte Aufruf von `F` erstellt ein Array mit 0 (null) Elementen, und übergibt diese Instanz als ein Value-Parameter. Die zweite und dritte Aufrufe sind wie folgt:

```vb
F(New Integer() {10, 20, 30, 40})
F(New Integer() {})
```

`ParamArray` in Deklarationen von Delegat- oder Ereignisparameter können keine Parameter angegeben werden.

### <a name="event-handling"></a>Ereignisbehandlung

Methoden können deklarativ von Objekten in der Instanz oder freigegebene Variablen ausgelösten Ereignisse behandeln. Zum Verarbeiten von Ereignissen, die Deklaration einer Methode gibt die `Handles` Schlüsselwort und führt eine oder mehrere Ereignisse.

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

Ein Ereignis in der `Handles` mithilfe von zwei Bezeichnern, die durch einen Punkt getrennten Liste angegeben:

* Der erste Bezeichner muss eine Instanz oder freigegebene Variable in der enthaltende Typ, der angibt, die `WithEvents` Modifizierer oder `MyBase` oder `MyClass` oder `Me` Schlüsselwort; andernfalls ein Kompilierungsfehler tritt auf. Diese Variable enthält das Objekt, das die Ereignisse behandelt, die von dieser Methode ausgelöst wird.

* Der zweite Bezeichner muss ein Mitglied der Typ des der erste Bezeichner angeben. Das Element muss ein Ereignis, und freigegeben werden kann. Wenn eine freigegebene Variable für den ersten Bezeichner angegeben wird, klicken Sie dann das Ereignis freigegeben werden muss, oder ein Fehler ausgegeben.

Eine Handlermethode `M` gilt als einen gültigen-Ereignishandler für ein Ereignis `E` Wenn die Anweisung `AddHandler E, AddressOf M` wäre auch gültig. Im Gegensatz zu einer `AddHandler` -Anweisung, ermöglichen jedoch explizite Ereignishandler behandeln eines Ereignisses mit einer Methode ohne Argumente, unabhängig davon, ob strikte Semantik oder nicht verwendet werden:

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

Ein einzelnes Element mehrere übereinstimmende Ereignisse behandeln, und mehrere Methoden können ein einzelnes Ereignis zu behandeln. Zugriff auf eine Methode hat keine Auswirkungen auf die Möglichkeit, Ereignisse zu behandeln. Das folgende Beispiel zeigt, wie eine Methode Ereignisse behandeln kann:

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

Dies wird ausgegeben:

```
Raised
Raised
```

Ein Typ erbt alle Ereignishandler, die von seinem Basistyp bereitgestellt. Ein abgeleiteter Typ kann nicht in keiner Weise die Ereignis-Zuordnungen geändert, erbt von Basistypen, sondern fügen Sie möglicherweise zusätzliche Handler zum Ereignis.


### <a name="extension-methods"></a>Erweiterungsmethoden

Methoden können hinzugefügt werden, auf die Typen von außerhalb des Deklaration mit *Erweiterungsmethoden*. Erweiterungsmethoden sind Methoden, mit der `System.Runtime.CompilerServices.ExtensionAttribute` Attribut angewendet werden. Sie können nur im standard-Modulen deklariert werden und müssen mindestens ein Parameter gibt an, dass der Typ der Methode erweitert. Die folgende Erweiterungsmethode erweitert beispielsweise den Typ `String`:

```vb
Imports System.Runtime.CompilerServices

Module StringExtensions
    <Extension> _
    Sub Print(s As String)
        Console.WriteLine(s)
    End Sub
End Module
```

__Beachten Sie.__ Obwohl Visual Basic Erweiterungsmethoden deklariert werden, in einem Standardmodul erforderlich sind, können andere Sprachen wie c# werden in anderen Arten von Typen deklariert werden. Solange die Methoden folgen, die anderen hier beschriebenen Konventionen und die mit Typ ein offener generischer Typ ist, und kann nicht instanziiert werden, Visual Basic erkennt die Erweiterungsmethoden.

Wenn eine Erweiterungsmethode aufgerufen wird, wird die Instanz, die, der Sie auf aufgerufen wird, wird, auf den ersten Parameter übergeben. Der erste Parameter kann nicht deklariert werden `Optional` oder `ParamArray`. Der erste Parameter einer Erweiterungsmethode kann beliebigen Typs, einschließlich der einen Parameter vom Typ sein. Die folgenden Methoden erweitern, z. B. die Typen `Integer()`, jeder Typ, der implementiert `System.Collections.Generic.IEnumerable(Of T)`, und alle Typen überhaupt:

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

Wie im vorherige Beispiel wird gezeigt, können die Schnittstellen erweitert werden. Schnittstelle Erweiterungsmethoden Geben Sie die Implementierung der Methode, damit Typen, die eine Schnittstelle zu implementieren, die Erweiterungsmethoden, die nach wie vor nur definiert wurde ursprünglich von der Schnittstelle deklarierten Member implementieren. Zum Beispiel:

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

Erweiterungsmethoden können auch Einschränkungen für ihre Typparameter und, ebenso wie mit nicht-generische Erweiterungsmethoden Typargument abgeleitet werden kann:

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

Erweiterungsmethoden können auch über implizite ausdrucksinstanzen innerhalb des Typs, der erweitert wird zugegriffen werden:

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

Für die Zwecke der Barrierefreiheit werden die Erweiterungsmethoden bereit, die als Mitglieder des standard-Moduls sie deklariert werden in – er besitzt keinen zusätzlichen Zugriff auf die Member des Typs, den sie über den Zugriff, die sie aufgrund ihrer Deklarationskontext haben erweitern werden auch behandelt.

Erweiterungsmethoden sind nur verfügbar, wenn die standard modulmethode im Gültigkeitsbereich befindet. Andernfalls wird der ursprüngliche Typ nicht angezeigt, die erweitert wurden. Zum Beispiel:

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

Auf einen Typ verweist, wenn nur eine Erweiterungsmethode für den Typ verfügbar ist, wird weiterhin einen Fehler während der Kompilierung erstellt.

Es ist wichtig zu beachten, dass Erweiterungsmethoden betrachtet werden Member des Typs in allen Kontexten, in denen Elemente, z. B. die stark typisierte gebunden sind `For Each` Muster. Zum Beispiel:

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

Auch können Delegaten erstellt werden, die Erweiterungsmethoden verweisen, werden. Daher ist der Code:

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

ist ungefähr gleich ist:

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

__Beachten Sie.__ Visual Basic fügt normalerweise eine Überprüfung auf eine Instanz-Methodenaufruf, der bewirkt, dass eine `System.NullReferenceException` auftreten, wenn die Instanz die Methode aufgerufen wird `Nothing`. Im Fall von Erweiterungsmethoden, es gibt keine effiziente Methode, diese Überprüfung eingefügt, sodass Erweiterungsmethoden explizite Prüfung müssen `Nothing`. 

__Beachten Sie.__ Wird ein Werttyp geschachtelt werden, beim Übergeben als wird eine `ByVal` Argument an einen Parameter typisiert als eine Schnittstelle.  Dies bedeutet, dass die Nebenwirkungen der Erweiterungsmethode eine Kopie der Struktur anstelle der ursprünglichen ausgeführt wird. Während die Sprache keine Einschränkungen für das erste Argument einer Erweiterungsmethode platziert, es empfiehlt sich, dass Erweiterungsmethoden bereit, die nicht zum Erweitern von Werttypen verwendet werden oder wenn Sie Werttypen zu erweitern, der erste Parameter übergeben wird `ByRef` um sicherzustellen, dass diese Seite Effekte werden auf den ursprünglichen Wert.

### <a name="partial-methods"></a>Partielle Methoden

Ein *partielle Methode* ist eine Methode, die eine Signatur jedoch nicht den Text der Methode angibt. Der Text der Methode kann von einer anderen Deklaration der Methode mit dem gleichen Namen und eine Signatur, die sehr wahrscheinlich bereits in eine andere partielle Deklaration des Typs bereitgestellt werden. Zum Beispiel:

a.vb:

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

b.vb:

```vb
Public Partial Class MyForm
    Public Sub ValidateControls()
        ' Validation logic goes here
        ...
    End Sub
End Class
```

In diesem Beispiel ist eine partielle Deklaration der Klasse `MyForm` deklariert eine partielle Methode `ValidateControls` ohne Implementierung. Obwohl es kein Nachrichtentext, der in der Datei angegeben ist, ruft der Konstruktor in der partiellen Deklaration der partielle Methode, an. Die andere partielle Deklaration `MyForm` dann stellt die Implementierung der Methode.

Partielle Methoden können aufgerufen werden, unabhängig davon, ob ein Textkörper angegeben wurde. Wenn kein Methodentext angegeben wird, wird der Aufruf ignoriert. Zum Beispiel:

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

Any-Ausdrücke, die als Argumente für einen partiellen Methodenaufruf übergeben werden, die ignoriert wird, sind auch ignoriert und nicht ausgewertet. (__Beachten.__ Dies bedeutet, dass partielle Methoden sind eine sehr effiziente Möglichkeit für die Bereitstellung von Verhalten, das über zwei partielle Typen definiert ist, da die partiellen Methoden ohne Kosten haben, wenn sie nicht verwendet werden.)

Deklaration der partiellen Methode muss deklariert werden, als `Private` und muss immer eine Unterroutine mit keine Anweisungen im Text. Partielle Methoden können nicht selbst Schnittstellenmethoden, implementieren, jedoch können die Methode, die ihren Text bereitstellt.

Nur eine Methode kann es sich um Text zu einer partiellen Methode bereitstellen. Eine Methode aus, geben Sie einen Text für eine partielle Methode müssen die gleiche Signatur wie die partielle Methode, die dieselben Einschränkungen für Typparameter, die gleichen deklarationsmodifizierer, und die gleichen Parameter und Typparameternamen. Attribute für die partielle Methode und die Methode, die Text bereitstellt werden zusammengeführt, wie Attribute auf die Methoden-Parameter. Auf ähnliche Weise wird die Liste der Ereignisse, die die Methoden handhaben zusammengeführt. Zum Beispiel:

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

*Konstruktoren* sind spezielle Methoden, die Kontrolle über die Initialisierung zu ermöglichen. Sie werden ausgeführt, nachdem das Programm beginnt, oder wenn eine Instanz eines Typs erstellt wird. Im Gegensatz zu anderen Membern Konstruktoren nicht geerbt werden, und führen einen Namen nicht in Deklarationsabschnitt des Typs. Konstruktoren können nur von Objekt-und Arrayerstellung Ausdrücken oder von .NET Framework aufgerufen werden; Sie können nicht direkt aufgerufen werden.

__Beachten Sie.__ Konstruktoren haben die gleiche Einschränkung auf Zeile Platzierung, die Unterroutinen. Der Anfang-Anweisung, die End-Anweisung und die Block müssen alle am Anfang einer logischen Zeile angezeigt werden.

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

*Instanzkonstruktoren* Instanzen eines Typs initialisieren und von .NET Framework ausgeführt werden, wenn eine Instanz erstellt wird. Die Parameterliste eines Konstruktors unterliegt den gleichen Regeln wie die Parameterliste einer Methode ab. Instanzkonstruktoren können überladen werden.

Alle Konstruktoren in Verweistypen müssen es sich um einen anderen Konstruktor aufrufen. Wenn der Aufruf explizit ist, muss er die erste Anweisung in den Text des Konstruktors-Methode. Die Anweisung kann entweder Aufrufen einer anderen Instanzkonstruktoren "des Typs" – z. B. `Me.New(...)` oder `MyClass.New(...)` – oder wenn es sich nicht um eine Struktur ist können sie einem Instanzenkonstruktor der Basistyp des Typs aufrufen – z. B. `MyBase.New(...)`. Es ist nicht zulässig für einen Konstruktor selbst aufrufen. Wenn ein Konstruktor einen Aufruf an einen anderen Konstruktor lässt `MyBase.New()` ist implizit. Wenn Sie keinen parameterlosen Konstruktor vorhanden ist, tritt auf, ein Fehler während der Kompilierung. Da `Me` gilt als nicht erstellt werden soll, bis nach dem Aufruf eines Basisklassenkonstruktors, nicht die Parameter für eine aufrufanweisung Konstruktor verweisen können `Me`, `MyClass`, oder `MyBase` implizit oder explizit.

Wenn ein Konstruktor für die erste Anweisung besitzt das Format `MyBase.New(...)`, der Konstruktor führt implizit die Initialisierungen, die von der Instanzvariablen, die in den Typ deklariert die Variable Initialisierer angegeben. Dies entspricht einer Sequenz von Zuweisungen, die ausgeführt werden, sofort nach dem Aufrufen des Konstruktors direkten Basistyp. Diese Reihenfolge wird sichergestellt, dass alle Basisinstanz Variablen durch ihre Variableninitialisierern initialisiert werden, bevor alle Anweisungen, die Zugriff auf die Instanz ausgeführt werden. Zum Beispiel:

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

Wenn `New B()` dient zum Erstellen einer Instanz von `B`, wird die folgende Ausgabe generiert:

```
x = 1, y = 1
```

Der Wert des `y` ist `1` da die Variableninitialisierer ausgeführt wird, nach dem Aufruf des Basisklassenkonstruktors. Variableninitialisierern werden in der Reihenfolge im Text ausgeführt, die sie in der Typdeklaration angezeigt werden.

Wenn ein Typ deklariert nur `Private` Konstruktoren, es ist nicht möglich im Allgemeinen für andere Typen von dem Typ abgeleitet sind, oder erstellen Instanzen des Typs; die einzige Ausnahme ist der Typ geschachtelten Typen. `Private` Konstruktoren werden am häufigsten in Typen, die nur `Shared` Member.

Wenn ein Typ keine Instanz-Deklarationen enthält, wird automatisch ein Standardkonstruktor bereitgestellt. Die Standard-Konstruktor ruft einfach den parameterlosen Konstruktor der direkten Basisklasse. Wenn direkte Basistyp nicht über einen zugänglichen parameterlosen Konstruktor verfügt, tritt ein Fehler während der Kompilierung. Wird von der deklarierten Zugriffstyp für den Standardkonstruktor `Public` , wenn der Typ ist `MustInherit`, in diesem Fall der Standardkonstruktor wird `Protected`.

__Beachten Sie.__ Der Standardzugriff für eine `MustInherit` Standardkonstruktor des Typs ist `Protected` da `MustInherit` Klassen können nicht direkt erstellt werden. Es gibt also keinen Sinn, sodass den Standardkonstruktor `Public`.

Im folgenden Beispiel wird ein Standardkonstruktor bereitgestellt, da die Klasse keine Deklarationen enthält:

```vb
Class Message
    Dim sender As Object
    Dim text As String
End Class
```

Daher ist das Beispiel genau äquivalent zu folgendem:

```vb
Class Message
    Dim sender As Object
    Dim text As String

    Sub New()
    End Sub
End Class
```

Standardkonstruktoren, die in einem Designer ausgegeben werden generiert, mit dem Attribut markierten Klasse `Microsoft.VisualBasic.CompilerServices.DesignerGeneratedAttribute` rufen Sie die Methode `Sub InitializeComponent()`, sofern es vorhanden, nach dem Aufruf des Basiskonstruktors ist. (__Beachten.__ Dadurch können Designer generierte Dateien, z. B. den im Windows Forms-Designer, um den Konstruktor in die Designer-Datei zu unterdrücken. Dies ermöglicht den Programmierer, die sie selbst, geben Sie bei Bedarf wechselseitig.)

### <a name="shared-constructors"></a>Shared-Konstruktoren

*Shared-Konstruktoren* Initialisieren eines Typs gemeinsam genutzt, Variablen, nachdem das Programm wird ausgeführt, aber vor verweisen auf ein Member des Typs ausgeführt werden. Gibt an, ein shared-Konstruktor die `Shared` Modifizierer, sofern dies nicht in einem Standardmodul in diesem Fall die `Shared` Modifizierer wird impliziert.

Anders als Instanzkonstruktoren shared-Konstruktoren verfügen über implizite öffentlichen Zugriff, darf keine Parameter, und keine anderen Konstruktoren aufgerufen werden können. Vor der ersten Anweisung in einem freigegebenen Konstruktor führt der gemeinsam genutzte Konstruktor implizit die Initialisierungen, die gemäß der Variableninitialisierern einzelne shared-Variable, die in den Typ deklariert. Dies entspricht einer Sequenz von Zuweisungen, die bei der Eingabe an den Konstruktor sofort ausgeführt werden. Die Variable Initialisierer werden in der Reihenfolge im Text ausgeführt, die sie in der Typdeklaration angezeigt werden.

Das folgende Beispiel zeigt eine `Employee` Klasse mit einem shared-Konstruktor, der eine freigegebene Variable initialisiert:

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

Für jede geschlossener generischer Typ ist ein separater freigegebener Konstruktor vorhanden. Da der gemeinsam genutzte Konstruktor ausgeführt wird, genau einmal für jeden Typ geschlossen, die es ist eine bequeme Möglichkeit, Überprüfungen zur Laufzeit für den Typparameter zu erzwingen, die zum Zeitpunkt der Kompilierung über die Einschränkungen nicht überprüft werden kann. Beispielsweise verwendet der folgende Typ einen shared-Konstruktor erzwingen, dass der Typparameter `Integer` oder `Double`:

```vb
Class EnumHolder(Of T)
    Shared Sub New() 
        If Not GetType(T).IsEnum() Then
            Throw New ArgumentException("T must be an enumerated type.")
        End If
    End Sub
End Class
```

Genau bei shared-Konstruktoren ausgeführt werden ist größtenteils hängt von der Implementierung, obwohl mehrere Garantien bereitgestellt werden, wenn ein shared-Konstruktor explizit definiert ist:

* Shared-Konstruktoren werden vor den ersten Zugriff auf ein statisches Feld des Typs ausgeführt.

* Shared-Konstruktoren werden vor den ersten Aufruf einer statischen Methode des Typs ausgeführt.

* Shared-Konstruktoren werden vor den ersten Aufruf des Konstruktors für den Typ ausgeführt.

Die oben genannten Garantien gelten nicht in der Situation, in denen ein shared-Konstruktor implizit für freigegebene Initialisierer erstellt wird. Die Ausgabe aus dem folgenden Beispiel ist unsicher, da die genaue Reihenfolge der laden und aus diesem Grund der gemeinsam genutzte Konstruktor Ausführung ist nicht definiert:

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

```
Init A
A.F
Init B
B.F
```

oder

```
Init B
Init A
A.F
B.F
```

Im Gegensatz dazu wird im folgende Beispiel vorhersagbare Ausgabe erzeugt. Beachten Sie, dass die `Shared` Konstruktor für die Klasse `A` nie ausgeführt wird, obwohl Klasse `B` werden von dieser abgeleitet:

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

```
Init B
B.G
```

Es ist auch möglich, um zirkuläre Abhängigkeiten zu erstellen, mit denen `Shared` Variablen mit Variableninitialisierern an, die in ihrer standardmäßigen beachtet werden Wert, Status, wie im folgenden Beispiel gezeigt:

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

```
X = 1, Y = 2
```

Zum Ausführen der `Main` -Methode, das System zuerst lädt Klasse `B`. Die `Shared` Konstruktor der Klasse `B` wird zum Berechnen des Anfangswert des `Y`, bewirkt, dass der rekursiv Klasse `A` geladen werden, da der Wert des `A.X` verwiesen wird. Die `Shared` Konstruktor der Klasse `A` wiederum wird zum Berechnen des Anfangswert des `X`, und dies der Fall ist, ruft der *Standard* Wert `Y`, d.h. 0 (null). `A.X` wird so initialisiert `1`. Die während des Ladevorgangs `A` dann abgeschlossen, und die Berechnung des Anfangswerts der `Y`, wird das Ergebnis der `2`.

Mussten die `Main` Methode stattdessen wurde gefunden in Klasse `A`, das Beispiel würde die folgende Ausgabe erzeugt haben:

```
X = 2, Y = 1
```

Vermeiden von Zirkelbezügen in `Shared` Variableninitialisierern, da sie sich im Allgemeinen unmöglich, um zu bestimmen, die Reihenfolge, in die Klassen, die solche Verweise werden geladen.

## <a name="events"></a>Ereignisse

Ereignisse werden verwendet, um Code eines bestimmten Auftretens zu benachrichtigen. Eine Ereignisdeklaration besteht aus einem Bezeichner, der entweder einen Delegattyp aufweisen oder einer Parameterliste, und eine optionale `Implements` Klausel.

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

Wenn ein Delegattyp angegeben wird, kann der Delegattyp einen Rückgabetyp keine. Wenn eine Liste von Parametern angegeben wird, es darf keine `Optional` oder `ParamArray` Parameter. Die Zugriffsdomäne des Parametertypen und/oder der Delegattyp muss identisch oder eine Obermenge der, die Zugriffsdomäne des Ereignisses. Ereignisse können gemeinsam genutzt werden, durch Angabe der `Shared` Modifizierer.

Zusätzlich zu den Namen des Members des Typs Deklarationsabschnitt hinzugefügt wird eine Ereignisdeklaration implizit mehrere andere Member deklariert. Ein Ereignis mit dem Namen `X`, werden die folgenden Elemente zum Deklarationsabschnitt hinzugefügt:

* Ist die Form der Deklaration einer Methodendeklaration, eine geschachtelte Delegatklasse mit dem Namen `XEventHandler` eingeführt. Die geschachtelte Delegate-Klasse die Deklaration der Methode entspricht, und hat den gleichen Zugriff wie das Ereignis. Die Attribute in der Parameterliste gelten für die Parameter, der die Delegate-Klasse.

* Ein `Private` Instanzvariable eingegeben wird, wie der Delegat, mit der Bezeichnung `XEvent`.

* Zwei Methoden namens `add_X` und `remove_X` die kann nicht aufgerufen, überschrieben oder überladen.

Wenn ein Typ versucht, einen Namen zu deklarieren, die einen der oben genannten Namen entspricht, ein Fehler während der Kompilierung ausgegeben, und die implizite `add_X` und `remove_X` Deklarationen werden zum Zweck der Bindungen ignoriert. Es ist nicht überschrieben oder überladen keines der Elemente eingeführt, obwohl es möglich, diese in abgeleiteten Typen zu überschatten. Beispielsweise werden in der Klassendeklaration

```vb
Class Raiser
    Public Event Constructed(i As Integer)
End Class
```

Die folgende Deklaration entspricht

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

Deklarieren eines Ereignisses ohne einen Delegattyp ist die einfachste und möglichst kompakte Syntax, jedoch hat den Nachteil, dass einen neuer Delegattyp für jedes Ereignis zu deklarieren. Beispielsweise werden im folgenden Beispiel drei verborgene Delegattypen erstellt, obwohl alle drei Ereignisse mit derselben Parameterliste haben:

```vb
Public Class Button
    Public Event Click(sender As Object, e As EventArgs)
    Public Event DoubleClick(sender As Object, e As EventArgs)
    Public Event RightClick(sender As Object, e As EventArgs)
End Class
```

Im folgenden Beispiel verwenden Sie die Ereignisse einfach denselben Delegaten `EventHandler`:

```vb
Public Delegate Sub EventHandler(sender As Object, e As EventArgs)

Public Class Button
    Public Event Click As EventHandler
    Public Event DoubleClick As EventHandler
    Public Event RightClick As EventHandler
End Class
```

Ereignisse können auf zwei Arten bearbeitet werden: statisch oder dynamisch. Statische Behandlung von Ereignissen ist einfacher und erfordert lediglich eine `WithEvents` Variable und ein `Handles` Klausel. Im folgenden Beispiel Klasse `Form1` statisch behandelt das Ereignis `Click` Objekts `Button`:

```vb
Public Class Form1
    Public WithEvents Button1 As New Button()

    Public Sub Button1_Click(sender As Object, e As EventArgs) _
           Handles Button1.Click
        Console.WriteLine("Button1 was clicked!")
    End Sub
End Class
```

Behandeln von Ereignissen ist komplexer, da das Ereignis muss explizit verbunden und die Verbindung im Code getrennt. Die Anweisung `AddHandler` Fügt einen Handler für ein Ereignis, und die Anweisung `RemoveHandler` entfernt einen Handler für ein Ereignis. Das nächste Beispiel zeigt eine Klasse `Form1` , addiert `Button1_Click` als Ereignishandler für `Button1`des `Click` Ereignis:

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

In der Methode `Disconnect`, wird der Ereignishandler entfernt.


### <a name="custom-events"></a>Benutzerdefinierte Ereignisse

Wie im vorherigen Abschnitt erläutert wird, definieren Sie Ereignisdeklarationen implizit ein Feld, eine `add_` -Methode, und ein `remove_` Methode, die verwendet werden, um zu verfolgen-Ereignishandler. In einigen Fällen kann jedoch sie benutzerdefinierten Code bereitstellen, für die nachverfolgung von Ereignishandlern wünschenswert. Wenn eine Klasse vierzig Ereignisse definiert, von denen nur wenige je anstelle von 40 Felder zum Nachverfolgen einer Hashtabelle verarbeitet werden, können der Handler für jedes Ereignis z. B. effizienter sein. *Benutzerdefinierte Ereignisse* ermöglichen die `add_X` und `remove_X` Methoden, um explizit definiert werden, für die benutzerdefinierten Speicher für Ereignishandler ermöglicht.

Benutzerdefinierte Ereignisse deklariert werden, auf die gleiche Weise, dass Ereignisse, die angeben, einen Delegattyp, mit der Ausnahme deklariert werden, die das Schlüsselwort `Custom` muss vor stehen die `Event` Schlüsselwort. Eine benutzerdefinierte Ereignisdeklaration enthält drei Deklarationen: ein `AddHandler` Deklaration einer `RemoveHandler` Deklaration und ein `RaiseEvent` Deklaration. Keiner der Deklarationen können Modifizierer, verfügt zwar über Attribute verfügen können.

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

Die `AddHandler` und `RemoveHandler` Deklaration nehmen Sie an einer `ByVal` -Parameter, der von den Delegattyp des Ereignisses sein muss. Wenn ein `AddHandler` oder `RemoveHandler` -Anweisung ausgeführt wird (oder ein `Handles` -Klausel automatisch behandelt ein Ereignis), wird die entsprechende Deklaration aufgerufen werden. Die `RaiseEvent` Deklaration weist die gleichen Parameter wie der Ereignisdelegat und wird aufgerufen, wenn eine `RaiseEvent` -Anweisung ausgeführt wird. Alle Deklarationen müssen bereitgestellt werden und gelten als Unterroutinen.

Beachten Sie, dass `AddHandler`, `RemoveHandler` und `RaiseEvent` Deklarationen haben die gleiche Einschränkung Zeile Platzierung, die Unterroutinen verfügen. Der Anfang-Anweisung, die End-Anweisung und die Block müssen alle am Anfang einer logischen Zeile angezeigt werden.

Zusätzlich zu den Namen des Members des Typs Deklarationsabschnitt hinzugefügt wird eine benutzerdefinierte Ereignisdeklaration implizit mehrere andere Member deklariert. Ein Ereignis mit dem Namen `X`, werden die folgenden Elemente zum Deklarationsabschnitt hinzugefügt:

* Eine Methode namens `add_X`, entspricht die `AddHandler` Deklaration.

* Eine Methode namens `remove_X`, entspricht die `RemoveHandler` Deklaration.

* Eine Methode namens `fire_X`, entspricht die `RaiseEvent` Deklaration.

Wenn ein Typ versucht, einen Namen zu deklarieren, der einen der oben genannten Namen entspricht, ein Fehler während der Kompilierung führt und die implizite Deklarationen werden zum Zweck der Bindungen ignoriert. Es ist nicht überschrieben oder überladen keines der Elemente eingeführt, obwohl es möglich, diese in abgeleiteten Typen zu überschatten.

__Beachten Sie.__ `Custom` ist ein reserviertes Wort.

#### <a name="custom-events-in-winrt-assemblies"></a>Benutzerdefinierte Ereignisse in WinRT-Assemblys

Ab Microsoft Visual Basic 11.0, Ereignisse, die in einer Datei deklariert, die mit kompiliert `/target:winmdobj`, oder in einer Schnittstelle in eine solche Datei deklariert und implementiert dann an einem anderen Ort, ein wenig anders behandelt werden.

* Externe Tools verwendet, um die Winmd erstellen lässt in der Regel nur für bestimmte Delegattypen wie z. B. `System.EventHandler(Of T)` oder `System.TypedEventHandle(Of T, U)`, und nicht zu, wenn andere Benutzer.

* Die `XEvent` Feld weist den Typ `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationTokenTable(Of T)` , in denen `T` ist der Delegattyp.

* Der AddHandler-Accessor gibt einen `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationToken`, und die RemoveHandler-Accessor nimmt einen einzelnen Parameter desselben Typs.

Hier ist ein Beispiel für solche ein benutzerdefiniertes Ereignis aus.

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

Ein *Konstanten* ist ein konstanter Wert, der ein Member eines Typs ist.

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

Konstanten werden implizit gemeinsam genutzt. Wenn die Deklaration enthält eine `As` -Klausel, die Klausel gibt den Typ des Elements durch die Deklaration eingeführt. Wenn der Typ ausgelassen wird, und klicken Sie dann der Typ der Konstante abgeleitet wird. Der Typ einer Konstante kann nur ein primitiver Typ sein oder `Object`. Wenn eine Konstante als typisiert ist `Object` und kein Typzeichen vorhanden ist, der tatsächliche Typ der Konstanten wird der Typ des konstanten Ausdrucks. Andernfalls ist der Typ der Konstante der Typ der Konstante des Typs Zeichens.

Das folgende Beispiel zeigt eine Klasse namens `Constants` , bei dem zwei öffentliche Konstanten:

```vb
Class Constants
    Public Const A As Integer = 1
    Public Const B As Integer = A + 1
End Class
```

Konstanten können Sie über die Klasse, wie im folgenden Beispiel an, die die Werte der druckt zugreifen `Constants.A` und `Constants.B`.

```vb
Module Test
    Sub Main()
        Console.WriteLine(Constants.A & ", " & Constants.B)
    End Sub 
End Module
```

Deklaration eine Konstante, die mehrere Konstanten deklariert entspricht mehreren Deklarationen der einzelnen Konstanten. Im folgende Beispiel werden drei Konstanten in einer deklarationsanweisung deklariert.

```vb
Class A
    Protected Const x As Integer = 1, y As Long = 2, z As Short = 3
End Class
```

Diese Deklaration ist äquivalent zu folgendem:

```vb
Class A
    Protected Const x As Integer = 1
    Protected Const y As Long = 2
    Protected Const z As Short = 3
End Class
```

Die Zugriffsdomäne des Typs der Konstanten muss identisch oder eine Obermenge der Zugriffsdomäne von die Konstante selbst sein. Der Konstante Ausdruck muss es sich um einen Wert, der den Typ der Konstante oder einen Typ, der implizit in den Typ der Konstante liefern. Der Konstante Ausdruck darf nicht zirkulär sein; eine Konstante kann, also nicht hinsichtlich sich selbst definiert werden.

Der Compiler wertet automatisch die Konstanten Deklarationen in der richtigen Reihenfolge. Im folgenden Beispiel wertet der Compiler zunächst `Y`, klicken Sie dann `Z`, und schließlich `X`, die Werten 10, 11 und 12, bzw. zu erzeugen.

```vb
Class A
    Public Const X As Integer = B.Z + 1
    Public Const Y As Integer = 10
End Class

Class B
    Public Const Z As Integer = A.Y + 1
End Class
```

Wenn ein symbolische Namen für einen konstanten Wert erwünscht ist, aber der Typ des Werts ist nicht zulässig in einer Konstantendeklaration oder wenn der Wert zum Zeitpunkt der Kompilierung durch einen konstanten Ausdruck berechnet werden kann, kann stattdessen eine schreibgeschützte Variable verwendet werden.


## <a name="instance-and-shared-variables"></a>-Instanz und freigegebene Variablen

Eine Instanz oder freigegebene Variable ist ein Member eines Typs, das Informationen speichern kann.

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

Die `Dim` Modifizierer muss angegeben werden, wenn kein Modifizierer angegeben sind, jedoch werden, andernfalls ausgelassen können. Eine einzige Variablendeklaration möglicherweise mehrere Variablendeklaratoren enthalten; Jeder Variablen Deklarator führt eine neue Instanz oder einen freigegebenen Member.

Wenn ein Initialisierer angegeben ist, kann nur eine Instanz oder freigegebene Variable von der Variable Deklarator deklariert werden:

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

Eine Variable mit dem `Shared` Modifizierer ist ein *freigegebene Variable*. Eine freigegebene Variable gibt genau einen Speicherort unabhängig von der Anzahl der Instanzen des Typs, die erstellt werden. Eine freigegebene Variable wird erstellt, wenn die Ausführung ein Programms beginnt, und es ist nicht mehr vorhanden sein, wenn das Programm beendet wird.

Eine freigegebene Variable wird nur von Instanzen eines bestimmten geschlossenen generischen Typs gemeinsam genutzt. Um beispielsweise das Programm:

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

Druckt:

```
1
1
2
```

Eine Variable deklariert, ohne die `Shared` Modifizierer wird aufgerufen, eine *Instanzvariable*. Jede Instanz einer Klasse enthält eine separate Kopie aller Instanz Variablen der Klasse. Eine Instanzvariable für ein Verweistyp wird erstellt, wenn eine neue Instanz der, Typ erstellt wurde, und ist nicht mehr vorhanden sein, wenn keine Verweise auf diese Instanz vorhanden sind und die `Finalize` Methode ausgeführt wurde. Eine Instanzvariable für ein Werttyp ist genau die gleiche Lebensdauer wie die Variable an, zu der er gehört. Das heißt, wenn eine Variable eines Werttyps erstellt wird, oder ist nicht mehr vorhanden ist, steigt auch die Instanzenvariable des Werttyps.

Wenn der Deklarator enthält ein `As` -Klausel, die Klausel gibt den Typ der Member durch die Deklaration eingeführt. Wenn der Typ ausgelassen wird und strenge Semantik verwendet wird, tritt ein Fehler während der Kompilierung. Andernfalls ist der Typ der Elemente implizit `Object` oder den Typ der Member-Typzeichen.

__Beachten Sie.__ Liegt keine Mehrdeutigkeit in der Syntax: Wenn ein Deklarator ein weggelassen wird, wird immer verwendet, den Typ der folgenden Deklaration.

Die Zugriffsdomäne von einer Instanz oder freigegebene Variable Typ oder Elementtyp des Arrays muss identisch oder eine Obermenge der Zugriffsdomäne von der Instanz oder freigegebene Variable sich selbst sein.

Das folgende Beispiel zeigt eine `Color` -Klasse, die interne Instanz mit dem Namen Variablen `redPart`, `greenPart`, und `bluePart`:

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

Wenn eine Instanz oder freigegebene-Variablendeklaration enthält eine `ReadOnly` Modifizierer-Zuweisungen für die Variablen, die durch die Deklaration eingeführt können nur auftreten, als Teil der Deklaration oder in einem Konstruktor derselben Klasse. Insbesondere dürfen Zuweisungen zu einer schreibgeschützten Instanz oder freigegebene Variable nur in den folgenden Situationen:

* In der Variablendeklaration, die die Instanz oder freigegebene Variable eingeführt werden (durch Einbeziehen von einem Variableninitialisierer in der Deklaration).

* Für eine Instanzenvariable in den Instanzkonstruktoren der Klasse, die die Variablendeklaration enthält. Die Instanzvariable kann nur zugegriffen werden, auf einen nicht qualifizierten Weise oder über `Me` oder `MyClass`.

* Für eine freigegebene Variable in der gemeinsam genutzte Konstruktor der Klasse, die die freigegebene Variablendeklaration enthält.

Eine freigegebene schreibgeschützte-Variable ist hilfreich, wenn ein symbolische Namen für einen konstanten Wert gewünscht ist, aber wenn der Typ des Werts in einer Konstantendeklaration nicht zulässig ist, oder wenn der Wert durch einen konstanten Ausdruck nicht zur Kompilierzeit berechnet werden kann.

Ein Beispiel für die erste eine solche Anwendung folgt, in welche Farbe gemeinsam verwendeter Variablen deklariert werden `ReadOnly` zu verhindern, dass sie von anderen Programmen geändert wird:

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

Konstanten und schreibgeschützte freigegebene Variablen haben unterschiedliche Semantiken auf. Wenn eine Konstante in ein Ausdruck verwiesen wird, wird der Wert der Konstanten zur Kompilierzeit abgerufen, aber wenn ein Ausdruck eine schreibgeschützte freigegebene Variable verweist, ist der Wert der freigegebene Variablen nicht bis zur Laufzeit abgerufen. Erwägen Sie die folgende Anwendung, die aus zwei verschiedenen Programmen besteht.

file1.vb:

```vb
Namespace Program1
    Public Class Utils
        Public Shared ReadOnly X As Integer = 1
    End Class
End Namespace
```

file2.vb:

```vb
Namespace Program2
    Module Test
        Sub Main()
            Console.WriteLine(Program1.Utils.X)
        End Sub
    End Module
End Namespace
```

Die Namespaces `Program1` und `Program2` kennzeichnen die beiden Programme, die separat kompiliert werden. Da Variablen `Program1.Utils.X` ist als deklariert `Shared ReadOnly`, Ausgabe des Werts durch die `Console.WriteLine` Anweisung zum Zeitpunkt der Kompilierung nicht bekannt ist, aber stattdessen zur Laufzeit abgerufen wird. Also wenn der Wert des `X` geändert wird und `Program1` erneut kompiliert wird, die `Console.WriteLine` Anweisung gibt der neue Wert, selbst wenn `Program2` nicht neu kompiliert. Aber wenn `X` war eine Konstante, die den Wert der `X` würde abgerufen haben, wurden zum Zeitpunkt `Program2` kompiliert wurde, und würden geblieben sind nicht betroffen von Änderungen in `Program1` bis `Program2` neu kompiliert wurde.

### <a name="withevents-variables"></a>WithEvents-Variablen

Einen Typ zu deklarieren kann, dass sie eine Gruppe von Ereignissen, die durch eine der zugehörigen Instanz oder freigegebene Variablen ausgelöst wird, durch die Deklaration der Instanz oder freigegebene Variable behandelt, die die Ereignisse mit löst die `WithEvents` Modifizierer. Zum Beispiel:

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

In diesem Beispiel ist die Methode `E1Handler` behandelt das Ereignis `E1` , der von der Instanz des Typs ausgelöst wird `Raiser` in der Instanzvariable gespeicherte `x`.

Die `WithEvents` Modifizierer bewirkt, dass die Variable mit einem führenden Unterstrich umbenannt und durch eine Eigenschaft mit dem gleichen Namen, die die ereigniseinbindung ersetzt werden. Wenn Sie den Namen der Variablen wird z. B. `F`, es wird umbenannt in `_F` und eine Eigenschaft `F` wird implizit deklariert. Liegt ein Konflikt zwischen dem neuen Variablennamen und eine andere Deklaration vor, wird ein Kompilierzeitfehler gemeldet. Alle Attribute der Variablen angewendet werden auf die umbenannte Variable übernommen.

Die implizite von erstellte Eigenschaft eine `WithEvents` Deklaration verbinden und lösen die entsprechenden Ereignishandler kümmert. Wenn ein Wert der Variablen zugewiesen ist, ruft die Eigenschaft zunächst die `remove` Methode für das Ereignis für die Instanz derzeit in der Variablen (Trennen des den vorhandenen Ereignishandler, sofern vorhanden). Als Nächstes die Zuweisung erfolgt und die Eigenschaft ruft die `add` Methode für das Ereignis auf der neuen Instanz in der Variablen (Einbinden von den neuen Ereignishandler). Der folgende Code entspricht der Code oben für das standard-Modul `Test`:

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

Es ist nicht zulässig, deklarieren eine Instanz oder freigegebene Variable als `WithEvents` Wenn die Variable als Struktur typisiert ist. Darüber hinaus `WithEvents` dürfen nicht in einer Struktur angegeben werden und `WithEvents` und `ReadOnly` können nicht kombiniert werden.

### <a name="variable-initializers"></a>Variableninitialisierern

-Instanz und freigegebene Variablendeklarationen in Klassen und Instanz Variablendeklarationen (aber nicht freigegeben Variablendeklarationen) in Strukturen können Variableninitialisierern enthalten. Für `Shared` Variablen Variableninitialisierern entsprechen zuweisungsanweisungen, die ausgeführt werden, nachdem das Programm, jedoch bevor beginnt die `Shared` Variable ist zunächst auf die verwiesen wird. Beispielsweise entsprechen Variablen Variableninitialisierern zuweisungsanweisungen, die ausgeführt werden, wenn eine Instanz der Klasse erstellt wird. Strukturen können keine Instanz Variableninitialisierern verwenden, da die parameterlosen Konstruktoren nicht geändert werden können.

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

```
x = 1.4142135623731, i = 100, s = Hello
```

Eine Zuweisung zu `x` tritt auf, wenn die Klasse geladen wird, und Zuweisungen `i` und `s` auftreten, wenn eine neue Instanz der Klasse erstellt wird.

Es ist sinnvoll, Variableninitialisierern als zuweisungsanweisungen vorstellen, die im Konstruktor des Typs-Block eingefügt werden. Das folgende Beispiel enthält mehrere Instanz Variableninitialisierern.

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

Das Beispiel entspricht dem folgenden Code, wobei jeder Kommentar eine automatisch eingefügte Anweisung angibt.

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

Alle Variablen werden auf den Standardwert von ihrem Typ initialisiert, bevor alle Variableninitialisierern ausgeführt werden. Zum Beispiel:

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

Da `b` wird automatisch auf den Standardwert initialisiert, wenn die Klasse geladen wird und `i` wird automatisch auf den Standardwert initialisiert, wenn eine Instanz der Klasse erstellt wird, erzeugt der vorhergehende Code die folgende Ausgabe:

```
b = False, i = 0
```

Jede Variableninitialisierer muss einen Wert, der den Typ der Variablen oder eines Typs, der implizit in den Typ der Variable ist liefern. Variableninitialisierer zirkulär sein oder finden Sie in einer Variablen, die hinter ihm, in dem Fall wird der Wert der Variablen, auf die verwiesen wird der Standardwert für die Zwecke des Initialisierers ist initialisiert wird. Solche ein Initialisierer ist fragwürdiger.

Es gibt drei Arten von Variableninitialisierern: reguläre Initialisierer Arraygröße Initialisierer und Objektinitialisierer. Die ersten beiden Formen angezeigt werden, nach einem Gleichheitszeichen, die den Typnamen folgt, die letzten beiden sind Teil der Deklaration selbst. Nur eine Art der Initialisierer kann in einer Deklaration verwendet werden.

#### <a name="regular-initializers"></a>Reguläre Initialisierer

Ein *regulären Initialisierer* ist ein Ausdruck, der implizit in den Typ der Variablen konvertiert werden. Er wird nach einem Gleichheitszeichen, das folgt des Typnamens und muss als Wert klassifiziert werden. Zum Beispiel:

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

```vb
x = 10, y = 20
```

Wenn eine Variablendeklaration einen regulären Initialisierer hat, kann jeweils nur eine einzelne Variable deklariert werden. Zum Beispiel:

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

Ein *Objektinitialisierer* wird mithilfe einer Objekterstellungsausdruck anstelle der Typname angegeben. Ein Objektinitialisierer ist gleichbedeutend mit der eine reguläre Initialisierung der Variablen das Ergebnis der Objekterstellungsausdruck zuweisen. So

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

Die Klammer in einem Objektinitialisierer wird immer als Argumentliste für den Konstruktor und niemals als Arraymodifizierer-Typ interpretiert. Ein Variablenname mit einem Objektinitialisierer kein Array der Modifizierer oder den Typmodifizierer für einen NULL-Werte zulässt.

#### <a name="array-size-initializers"></a>Größe des Arrays Initialisierer

Ein *Arraygröße Initialisierer* ist ein Modifizierer für den Namen der Variablen, die einen Satz von Dimension Obergrenzen, gekennzeichnet durch Ausdrücke bietet.

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

Die obere Grenze-Ausdrücke müssen klassifiziert werden, als Werte und muss implizit in `Integer`. Der Satz von Obergrenzen ist gleichbedeutend mit einem Variableninitialisierer eines Ausdrucks für die Arrayerstellung mit der angegebenen oberen Grenzen. Die Anzahl der Dimensionen des Arraytyps wird von der Größe Arrayinitialisierer abgeleitet. So

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

Alle oberen Grenzen muss gleich oder größer als-1 sein, und alle Dimensionen müssen eine Obergrenze angegeben haben. Wenn der Elementtyp des Arrays initialisiert wird selbst einen Arraytyp ist, wechseln Sie die Array-Typ-Modifizierer, rechts von der Größe des Arrays Initialisierer. Beispiel:

```vb
Module Test
    Sub Main()
        Dim x(5,10)(,,) As Integer
    End Sub
End Module
```

deklariert eine lokale Variable `x` , dessen Typ ist ein zweidimensionales Array von dreidimensionalen Arrays `Integer`, auf ein Array mit den Grenzen des initialisierten `0..5` in der ersten Dimension und `0..10` in der zweiten Dimension. Es ist nicht möglich, einen Arrayinitialisierer der Größe zu verwenden, um die Elemente einer Variablen zu initialisieren, dessen Typ ein Array aus Arrays ist.

Die Deklaration eine Variable mit einem Initialisierer für die Größe des Arrays kann keinen Array der Modifizierer für den Typ oder einen regulären Initialisierer haben.


### <a name="systemmarshalbyrefobject-classes"></a>System.MarshalByRefObject-Klassen

Klassen, die von der Klasse abgeleitet sind `System.MarshalByRefObject` Kontextgrenzen hinweg proxybasierte gemarshallt werden (d. h. als Verweis) statt durch Kopieren (, also nach Wert). Dies bedeutet, dass eine Instanz einer solchen Klasse möglicherweise nicht in eine Instanz der "true", aber stattdessen nur ggf. ein Stub, der Variablen marshallt greift auf Methodenaufrufe, die über eine Kontextgrenze hinweg.

Daher ist es nicht möglich, einen Verweis auf den Speicherort der Variablen für diese Klassen zu erstellen. Dies bedeutet, dass Variablen typisiert, abgeleitete Klassen `System.MarshalByRefObject` kann nicht zum Verweisen auf Parameter, und die Methoden und Variablen von Variablen wie Werttypen nicht zugegriffen werden können, übergeben werden. Visual Basic behandelt stattdessen Variablen, die für solche Klassen definiert werden, als wären sie Eigenschaften (da die Einschränkungen in den Eigenschaften identisch sind).

Es gibt eine Ausnahme von dieser Regel: ein Element mit implizit oder explizit qualifiziert `Me` unterliegt der oben beschriebenen Einschränkungen, da `Me` ist immer garantiert ein tatsächliches Objekt, nicht über einen Proxy.

## <a name="properties"></a>Eigenschaften

*Eigenschaften* sind eine natürliche Erweiterung von Variablen; beide sind benannte Member mit zugeordneten Typen und die Syntax für den Zugriff auf Variablen und Eigenschaften entspricht. Im Gegensatz zu den Variablen keinen jedoch Eigenschaften Speicherorte an. Müssen Sie Eigenschaften stattdessen *Accessoren*, die die Anweisungen ausführen, um das Lesen oder Schreiben deren Werte angeben.

Eigenschaften werden mit Eigenschaftendeklarationen definiert. Der erste Teil einer Eigenschaftendeklaration ähnelt einer Felddeklaration. Der zweite Teil enthält eine `Get` Accessor und/oder einen `Set` Accessor.

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

Im folgenden Beispiel wird die `Button` -Klasse definiert eine `Caption` Eigenschaft.

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

Auf der Grundlage der `Button` oben gezeigte Klasse, die folgenden ist ein Beispiel für die Verwendung von der `Caption` Eigenschaft:

```vb
Dim okButton As Button = New Button()

okButton.Caption = "OK" ' Invokes Set accessor.
Dim s As String = okButton.Caption ' Invokes Get accessor.
```

Hier ist die `Set` -Accessor wird aufgerufen, durch das Zuweisen eines Werts der Eigenschaft und die `Get` Accessor wird aufgerufen, indem Sie auf die Eigenschaft in einem Ausdruck verweisen.

Wenn kein Typ für eine Eigenschaft angegeben wird und strenge Semantik verwendet wird, tritt ein Fehler während der Kompilierung; Andernfalls ist der Typ der Eigenschaft implizit `Object` oder den Typ der Eigenschaft Typzeichen. Eine Eigenschaftendeklaration darf entweder eine `Get` Accessor, der den Wert der Eigenschaft abgerufen wird, eine `Set` -Accessor, der den Wert der Eigenschaft oder sowohl speichert. Da eine Eigenschaft Methoden implizit deklariert wird, kann eine Eigenschaft mit den gleichen Modifizierern als eine Methode deklariert werden. Wenn die Eigenschaft in einer Schnittstelle definiert wird, oder mit definiert die `MustOverride` Modifizierer, der den Eigenschaftentext und die `End` Konstrukt muss ausgelassen werden kann; andernfalls ein Kompilierungsfehler tritt auf.

Die Parameterliste für den Index verwendet wird, bildet die Signatur der Eigenschaft, damit die Eigenschaften für Indexparameter jedoch nicht für den Typ der Eigenschaft überladen werden können. Die Liste der Indexparameter ist identisch mit dem eine normale Methode. Keiner der Parameter kann jedoch geändert werden, mit der `ByRef` -Modifizierer und keine von ihnen unter Umständen werden mit dem Namen `Value` (vorbehalten für den impliziten Wertparameter in der `Set` Accessor).

Eine Eigenschaft kann wie folgt deklariert werden:

* Wenn die Eigenschaft keine Eigenschaft der Modifizierer gibt an, die Eigenschaft benötigen Sie Folgendes ein `Get` Accessor und einen `Set` Accessor. Die Eigenschaft gilt eine Lese-/ Schreibzugriff-Eigenschaft.

* Wenn die Eigenschaft gibt an, die `ReadOnly` Modifizierer die Eigenschaft müssen eine `Get` Accessor und dürfen keine `Set` Accessor. Die Eigenschaft gilt nur-Lese Eigenschaft. Es ist ein Fehler während der Kompilierung für eine schreibgeschützte Eigenschaft, die das Ziel einer Zuweisung sein.

* Wenn die Eigenschaft gibt an, die `WriteOnly` Modifizierer die Eigenschaft müssen eine `Set` Accessor und dürfen keine `Get` Accessor. Die Eigenschaft gilt nur-Schreiben-Eigenschaft. Es ist ein Fehler während der Kompilierung, um eine Nur-Schreiben-Eigenschaft in einem Ausdruck außer als Ziel einer Zuweisung oder als Argument an eine Methode zu verweisen.

Die `Get` und `Set` Accessor einer Eigenschaft sind nicht unterschiedliche Elemente aus, und es ist nicht möglich, um die Accessoren der Eigenschaft separat zu deklarieren. Im folgende Beispiel wird eine einzelne Lese-/ Schreibzugriff-Eigenschaft nicht deklarieren. Stattdessen deklariert zwei Eigenschaften mit dem gleichen Namen, eine schreibgeschützte und lesegeschützte:

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

Da zwei Elemente, die in der gleichen Klasse deklariert den gleichen Namen haben können, wird im Beispiel wird einen Fehler während der Kompilierung.

Standardmäßig wird der Zugriff auf einer Eigenschaft des `Get` und `Set` Accessoren entspricht dem der Zugriff auf die Eigenschaft selbst. Allerdings die `Get` und `Set` Accessoren können auch angeben, Barrierefreiheit, getrennt von der Eigenschaft. In diesem Fall wird der Zugriff auf einen Accessor muss restriktiver sein als die Zugriffsebene der Eigenschaft, und nur einen Accessor haben eine andere Zugriffsebene aus der Eigenschaft. Zugriffstypen werden mehr oder weniger restriktive wie folgt berücksichtigt:

* `Private` ist stärker eingeschränkt als `Public`, `Protected Friend`, `Protected`, oder `Friend`.

* `Friend` ist stärker eingeschränkt als `Protected Friend` oder `Public`.

* `Protected` ist stärker eingeschränkt als `Protected Friend` oder `Public`.

* `Protected Friend` ist stärker eingeschränkt als `Public`.

Wenn eine der die Zugriffsmethoden einer Eigenschaft zugegriffen werden kann, aber der andere Controller nicht ist, wird die Eigenschaft behandelt, als wäre es schreibgeschützt oder lesegeschützt. Zum Beispiel:

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

Wenn ein abgeleiteter Typ führt Shadowing für eine Eigenschaft, blendet die abgeleitete Eigenschaft die Shadowing-Eigenschaft in Bezug auf das Lesen und schreiben. Im folgenden Beispiel die `P` -Eigenschaft in `B` Blendet die `P` -Eigenschaft in `A` in Bezug auf das Lesen und schreiben:

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

Die Zugriffsdomäne des Rückgabetyp oder Parametertypen muss identisch oder eine Obermenge der Zugriffsdomäne von der Eigenschaft selbst sein. Eine Eigenschaft kann nur eine haben `Set` -Accessor und einen `Get` Accessor.

Mit Ausnahme der Unterschiede in der Deklarations- und Aufrufsyntax Syntax `Overridable`, `NotOverridable`, `Overrides`, `MustOverride`, und `MustInherit` Eigenschaften verhalten sich genau wie `Overridable`, `NotOverridable`, `Overrides`, `MustOverride`, und `MustInherit` Methoden. Wenn eine Eigenschaft überschrieben wird, muss die überschreibende Eigenschaft desselben Typs (Lese-/ Schreibzugriff, schreibgeschützten, lesegeschützten) sein. Ein `Overridable` Eigenschaft darf nicht enthalten eine `Private` Accessor.

Im folgenden Beispiel `X` ist ein `Overridable` schreibgeschützte Eigenschaft, `Y` ist ein `Overridable` Lese-/ Schreibzugriff-Eigenschaft und `Z` ist eine `MustOverride` Lese-/ Schreibzugriff-Eigenschaft.

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

Da `Z` ist `MustOverride`, wird die enthaltende Klasse `A` muss deklariert werden `MustInherit`.

Im Gegensatz dazu eine Klasse, die von Klasse abgeleitet ist `A` ist unten dargestellt:

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

Hier wird die Deklarationen der Eigenschaften `X`,`Y`, und `Z` überschreiben Sie die grundlegenden Eigenschaften. Jede Eigenschaftendeklaration entspricht genau der Zugriffsmodifizierer, Typ und Namen der entsprechenden geerbte Eigenschaften. Die `Get` -Accessor der Eigenschaft `X` und `Set` -Accessor der Eigenschaft `Y` verwenden die `MyBase` Schlüsselwort, um die geerbten Eigenschaften zuzugreifen. Die Deklaration der Eigenschaft `Z` überschreibt die `MustOverride` Eigenschaft – es gibt also keine ausstehenden `MustOverride` Member in Klasse `B`, und `B` ist zulässig, werden von einer normalen Klasse.

Eigenschaften können verwendet werden, um die Initialisierung einer Ressource bis zum Moment zu verzögern, die es erstmalig verwiesen wird. Zum Beispiel:

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

Die `ConsoleStreams` -Klasse enthält drei Eigenschaften `In`, `Out`, und `Error`, die Eingabe, Ausgabe und Fehler Geräten bzw. darstellen. Durch diese Member als Eigenschaften verfügbar macht, die `ConsoleStreams` Klasse kann die Initialisierung verzögert, bis sie tatsächlich verwendet werden. Z. B. bei der ersten Verweis auf die `Out` Eigenschaft, wie im `ConsoleStreams.Out.WriteLine("hello, world")`, den zugrunde liegenden `TextWriter` für das Ausgabegerät initialisiert wird. Aber wenn die Anwendung keinen Verweis auf die `In` und `Error` Eigenschaften, keine Objekte für diese Geräte erstellt werden.


### <a name="get-accessor-declarations"></a>Get Accessordeklarationen

Ein `Get` Accessors (Getters) deklariert wird, indem Sie eine Eigenschaft `Get` Deklaration. Eine Eigenschaft `Get` besteht aus der Deklaration des Schlüsselworts `Get` gefolgt von einem Anweisungsblock. Erhalten eine Eigenschaft namens `P`, `Get` Zugriffsmethoden-Deklaration deklariert implizit eine Methode mit dem Namen `get_P` mit dem gleichen Modifizierer, den Typ und die Parameterliste als Eigenschaft. Wenn der Typ eine Deklaration mit diesem Namen enthält, die implizite Deklaration wird ignoriert, für die Zwecke namensbindung ergibt ein Fehler während der Kompilierung.

Eine spezielle lokale Variable, die implizit in deklariert ist die `Get` Deklarationsabschnitt der Accessor-Body, mit dem gleichen Namen wie die Eigenschaft stellt den Rückgabewert der Eigenschaft dar. Die lokale Variable ist sondername Auflösung Semantik, wenn in Ausdrücken verwendet. Wenn die lokale Variable in einem Kontext verwendet wird, die einen Ausdruck erwartet, der als eine Methodengruppe, z. B. ein Aufrufausdruck klassifiziert ist, löst den Namen an die Funktion und nicht an die lokale Variable. Zum Beispiel:

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

Die Verwendung von Klammern kann dazu führen, dass mehrdeutige Situationen (z. B. `F(1)` , in denen `F` ist eine Eigenschaft, deren Typ ein eindimensionales Array ist). In allen Situationen nicht eindeutig löst den Namen der Eigenschaft anstelle der lokalen Variablen. Zum Beispiel:

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

Wenn Steuerelement Flow / / Blätter der `Get` Accessortext hinausgehen, der Wert der lokalen Variablen wird an der Aufrufausdruck übergeben. Da Aufrufen einer `Get` Accessor Konzept entspricht dem Lesen des Werts einer Variablen, gilt dies Unzulässiger Programmierstil für `Get` Accessoren Observable Nebeneffekte haben, wie im folgenden Beispiel dargestellt:

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

Der Wert des der `NextValue` Eigenschaft hängt die Anzahl der Male, die die Eigenschaft zuvor zugegriffen wurde. Klicken Sie daher Zugriff auf die Eigenschaft einen Observable Nebeneffekt erzeugt, und die Eigenschaft sollte stattdessen als eine Methode implementiert werden.

Die "keine Nebeneffekte" Konvention für `Get` Accessoren bedeutet nicht, dass `Get` Accessoren sollte stets so verfasst werden einfach in Variablen gespeicherten Werte zurückgeben. In der Tat `Get` Accessoren häufig berechnen Sie den Wert einer Eigenschaft, indem Sie Zugriff auf mehrere Variablen oder Methoden aufrufen. Allerdings ein ordnungsgemäß entwickeltes `Get` Accessor führt keine Aktionen, die dazu führen, wahrnehmbare Änderungen in den Zustand des Objekts dass.

__Beachten Sie.__ `Get` Accessoren haben die gleiche Einschränkung Zeile Platzierung, die Unterroutinen aufweisen. Der Anfang-Anweisung, die End-Anweisung und die Block müssen alle am Anfang einer logischen Zeile angezeigt werden.

```antlr
PropertyGetDeclaration
    : Attributes? AccessModifier? 'Get' LineTerminator
      Block?
      'End' 'Get' StatementTerminator
    ;
```

### <a name="set-accessor-declarations"></a>Set-Accessor-Deklarationen

Ein `Set` Accessor (Setter) deklariert wird, indem Sie eine Eigenschaftsdeklaration für die Gruppe. Eine Eigenschaftsdeklaration für die Gruppe besteht aus dem Schlüsselwort `Set`, eine Liste optionaler Parameter, und einen Anweisungsblock. Erhalten eine Eigenschaft namens `P`, eine Setter-Deklaration deklariert implizit eine Methode mit dem Namen `set_P` mit dem gleichen Modifizierer und der Parameterliste als Eigenschaft. Wenn der Typ eine Deklaration mit diesem Namen enthält, die implizite Deklaration wird ignoriert, für die Zwecke namensbindung ergibt ein Fehler während der Kompilierung.

Bei Verwendung eine Parameterliste angegeben wird, muss einen Member, in diesen Member müssen keine Modifizierer mit Ausnahme von `ByVal`, und der Typ muss den Typ der Eigenschaft identisch sein. Der Parameter repräsentiert den Wert der Eigenschaft festgelegt wird. Wenn der Parameter ausgelassen wird, ein Parameter mit dem Namen `Value` wird implizit deklariert.

__Beachten Sie.__ `Set` Accessoren haben die gleiche Einschränkung Zeile Platzierung, die Unterroutinen aufweisen. Der Anfang-Anweisung, die End-Anweisung und die Block müssen alle am Anfang einer logischen Zeile angezeigt werden.

```antlr
PropertySetDeclaration
    : Attributes? AccessModifier? 'Set'
      ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block?
      'End' 'Set' StatementTerminator
    ;
```

### <a name="default-properties"></a>Standardeigenschaften

Eine Eigenschaft, die den Modifizierer gibt an, `Default` heißt eine *Standardeigenschaft*. Jeder Typ, der Eigenschaften kann möglicherweise eine Standardeigenschaft, einschließlich der Schnittstellen. Die Standardeigenschaft kann verwiesen werden, ohne die Instanz mit dem Namen der Eigenschaft zu qualifizieren. Daher erhalten eine Klasse

```vb
Class Test
    Public Default ReadOnly Property Item(i As Integer) As Integer
        Get
            Return i
        End Get
    End Property
End Class
```

Der code

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

Nachdem eine Eigenschaft deklariert ist `Default`, werden alle auf diesen Namen in der Vererbungshierarchie überladenen Eigenschaften der Standardeigenschaft, ob sie deklariert wurden `Default` oder nicht. Deklarieren einer Eigenschaft `Default` in einer abgeleiteten Klasse, wenn die Basisklasse der Klasse deklariert eine Standardeigenschaft unter einem anderen Namen erfordert keine andere Modifizierer wie z. B. `Shadows` oder `Overrides`. Dies ist da die Standardeigenschaft keine Identitäten und keine Signatur vorhanden ist und daher schattiert oder überladen werden kann. Zum Beispiel:

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

Dieses Programm wird die Ausgabe erzeugen:

```
MoreDerived = 10
Derived = 10
Base = 10
```

Alle Standard-Eigenschaften, die innerhalb eines Typs deklariert muss den gleichen Namen aufweisen und müssen aus Gründen der Übersichtlichkeit angeben der `Default` Modifizierer. Da eine Standardeigenschaft ohne Index-Parameter eine mehrdeutige Situation führen würde, wenn Instanzen von der enthaltenden Klasse zuweisen, müssen die Standardeigenschaften Indexparameter aufweisen. Darüber hinaus, wenn Überladen einer Eigenschaft auf ein bestimmter Namen umfasst die `Default` Modifizierer verwenden, alle auf diesen Namen überladene Eigenschaften angegeben. Standardeigenschaften möglicherweise nicht `Shared`, und mindestens einen Accessor der Eigenschaft darf nicht sein `Private`.

### <a name="automatically-implemented-properties"></a>Automatisch implementierte Eigenschaften

Falls eine Eigenschaft alle Accessoren Deklaration weggelassen wird, eine Implementierung der Eigenschaft wird automatisch bereitgestellt werden, wenn die Eigenschaft in einer Schnittstelle deklariert wird oder deklariert `MustOverride`. Nur Lese-/Schreibeigenschaften ohne Argumente können automatisch implementiert werden; andernfalls tritt ein Kompilierungsfehler auf.

Eine automatisch implementierte Eigenschaft `x`, sogar einer anderen Eigenschaft zu überschreiben, führt eine private lokale Variable `_x` mit dem gleichen Typ wie die Eigenschaft. Liegt ein Konflikt zwischen der lokalen Variablen-Namen und eine andere Deklaration vor, wird ein Kompilierzeitfehler gemeldet. Der automatisch implementierten Eigenschaft `Get` Accessor gibt den Wert der lokalen und der Eigenschaft `Set` Accessor, der den Wert der lokalen festlegt. Beispielsweise ist die Deklaration:

```vb
Public Property x() As Integer
```

ist ungefähr gleich ist:

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

Wie bei Variablendeklarationen, kann eine automatisch implementierte Eigenschaft einen Initialisierer enthalten. Zum Beispiel:

```vb
Public Property x() As Integer = 10
Public Shared Property y() As New Customer() With { .Name = "Bob" }
```

__Beachten Sie.__ Wenn eine automatisch implementierte Eigenschaft initialisiert wird, wird er durch die Eigenschaft, nicht das zugrunde liegende Feld initialisiert. Dies ist daher Überschreiben von Eigenschaften die Initialisierung abfangen kann, wenn dies erforderlich.

Arrayinitialisierer sind für automatisch implementierte Eigenschaften zulässig, außer dass es keine Möglichkeit gibt, die Arraygrenzen explizit angeben.  Zum Beispiel:

```vb
' Valid
Property x As Integer() = {1, 2, 3}
Property y As Integer(,) = {{1, 2, 3}, {12, 13, 14}, {11, 10, 9}}

' Invalid
Property x4(5) As Short
```

### <a name="iterator-properties"></a>Iterator-Eigenschaften

Ein *Iterator Eigenschaft* ist eine Eigenschaft mit dem `Iterator` Modifizierer. Dient aus demselben Grund eine Iteratormethode (Abschnitt [Iteratormethoden](statements.md#iterator-methods)) verwendet wird – als einfache Möglichkeit zum Generieren einer Sequenz, die von genutzt werden kann die `For Each` Anweisung. Die `Get` Accessor ein Iterator-Eigenschaft wird auf die gleiche Weise wie eine Iteratormethode interpretiert.

Eine Iterator-Eigenschaft muss einen expliziten haben `Get` -Accessor, und der Typ muss `IEnumerator`, oder `IEnumerable`, oder `IEnumerator(Of T)` oder `IEnumerable(Of T)` für einige `T`.

Hier ist ein Beispiel für einen Iterator-Eigenschaft:

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

*Operatoren* sind Methoden, die die Bedeutung eines vorhandenen Visual Basic-Operators für die enthaltende Klasse zu definieren. Wenn der Operator auf die Klasse in einem Ausdruck angewendet wird, wird der Operator in einen Aufruf der Standardabfrageoperator-Methode, die in der Klasse definiert kompiliert. Definieren einen Operator aus, für eine Klasse, auch bekannt als ist *überladen* den Operator.

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

Es ist nicht möglich, einen Operator überladen, der bereits vorhanden ist; in der Praxis ist gilt dies vor allem für Konvertierungsoperatoren. Beispielsweise ist es nicht möglich, die die Konvertierung von einer abgeleiteten Klasse in einer Basisklasse überladen:

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

Operatoren können auch im allgemeinen Sinn des Wortes überladen werden:

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

Operatordeklarationen sind Namen nicht explizit Deklarationsabschnitt des enthaltenden Typs hinzufügen. jedoch sollten sie eine entsprechende Methode, das die Zeichen "Op_" implizit deklarieren. Den folgenden Abschnitten werden der entsprechenden Methodennamen mit jeder Operator.

Es gibt drei Klassen von Operatoren, die definiert werden können: unäre Operatoren, binäre Operatoren und Konvertierungsoperatoren. Alle Operatordeklarationen teilen sich bestimmte Einschränkungen:

* Operatordeklarationen sein `Public` und `Shared`. Die `Public` Modifizierer kann in Kontexten, in dem der Modifizierer wird davon ausgegangen, dass, weggelassen werden.

* Die Parameter der Bediener können nicht deklariert werden `ByRef`, `Optional` oder `ParamArray`.

* Der Typ von mindestens einer der Operanden oder den Rückgabewert muss den Typ, der den Operator enthält.

* Es ist keine Funktion Rückgabevariablen, die für die Operatoren definiert. Aus diesem Grund die `Return` -Anweisung zum Zurückgeben von Werten aus einem Operator Text verwendet werden muss.

Die einzige Ausnahme für diese Einschränkungen gilt für auf NULL festlegbare Werttypen. Da auf NULL festlegbare Werttypen keine Definition der tatsächliche Typ verfügen, kann ein Werttyp benutzerdefinierten Operatoren, die auf NULL festlegbare Version des Typs deklarieren. Beim bestimmen, ob es sich bei einen bestimmten benutzerdefinierten Operator, einen Typ deklarieren, kann die `?` Modifizierer sind im Rahmen der Prüfung der Gültigkeit auf alle Typen in der Deklaration beteiligten zuerst gelöscht. Die Lockerung gelten nicht für den Rückgabetyp der der `IsTrue` und `IsFalse` Operatoren; sie müssen weiterhin zurückgeben `Boolean`, nicht `Boolean?`.

Die Rangfolge und Assoziativität von einem Operator können nicht von einem Operatordeklaration geändert werden.

__Beachten Sie.__ Operatoren weisen die gleiche Einschränkung Zeile Platzierung, die Unterroutinen verfügen. Der Anfang-Anweisung, die End-Anweisung und die Block müssen alle am Anfang einer logischen Zeile angezeigt werden.


### <a name="unary-operators"></a>Unäre Operatoren

Die folgenden Unäroperatoren können überladen werden:

* Der unäre plus -Operator `+` (entsprechende-Methode: `op_UnaryPlus`)

* Der unäre Operator minus `-` (entsprechende-Methode: `op_UnaryNegation`)

* Die logische `Not` Operator (entsprechende-Methode: `op_OnesComplement`)

* Die `IsTrue` und `IsFalse` Operatoren (entsprechenden Methoden: `op_True`, `op_False`)

Alle der überladene unäre Operatoren muss einen einzelnen Parameter des enthaltenden Typs und kann einen beliebigen Typ zurückgeben, mit Ausnahme von `IsTrue` und `IsFalse`, muss die zurückgeben, `Boolean`. Wenn der enthaltende Typ ein generischer Typ ist, müssen die Typparameter des enthaltenden Typs-Typ-Parametern übereinstimmen. Ein auf ein Objekt angewendeter

```vb
Structure Complex
    ...

    Public Shared Operator +(v As Complex) As Complex
        Return v
    End Operator
End Structure
```

Wenn ein Typ eines `IsTrue` oder `IsFalse`, und klicken Sie dann sie auf der anderen überladen muss. Wenn nur eine überladen ist, führt ein Fehler während der Kompilierung.

__Beachten Sie.__ `IsTrue` und `IsFalse` sind keine reservierten Wörter.

### <a name="binary-operators"></a>Binäre Operatoren

Die folgenden binären Operatoren können überladen werden:

* Die Hinzufügung `+`, Subtraktion `-`, Multiplikation `*`, Division `/`, ganzzahligen Division `\`, modulo `Mod` als auch die Potenzierung `^` Operatoren (entsprechende-Methode: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_IntegerDivision`, `op_Modulus`, `op_Exponent`)

* Die relationalen Operatoren `=`, `<>`, `<`, `>`, `<=`, `>=` (entsprechenden Methoden: `op_Equality`, `op_Inequality`, `op_LessThan`, `op_GreaterThan`, `op_LessThanOrEqual` , `op_GreaterThanOrEqual`). __Beachten Sie.__ Während der Equality-Operator überladen werden kann, nicht der Zuweisungsoperator (nur in zuweisungsanweisungen verwendet) nicht überladen werden.

* Die `Like` Operator (entsprechende-Methode: `op_Like`)

* Der Operator für Verkettungen `&` (entsprechende-Methode: `op_Concatenate`)

* Die logische `And`, `Or` und `Xor` Operatoren (entsprechenden Methoden: `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`)

* Die Schiebeoperatoren `<<` und `>>` (entsprechenden Methoden: `op_LeftShift`, `op_RightShift`)

Alle überladene binäre Operatoren müssen den enthaltenden Typ als einen der Parameter verwenden. Wenn der enthaltende Typ ein generischer Typ ist, müssen die Typparameter des enthaltenden Typs-Typ-Parametern übereinstimmen. Die Schiebeoperatoren weiter einschränken, diese Regel, dass den ersten Parameter für den enthaltenden Typ aufweisen müssen; der zweite Parameter muss stets vom Typ `Integer`.

Die folgenden binären Operatoren müssen als Paar deklariert werden:

* Operator `=` and -Operator `<>`

* Operator `>` and -Operator `<`

* Operator `>=` and -Operator `<=`

Wenn der beiden deklariert wird, klicken Sie dann die andere muss ebenfalls deklariert werden mit übereinstimmenden Parameter- und Rückgabetypen oder ein Fehler während der Kompilierung führt. (__Beachten.__ Der Zweck der gekoppelte Deklarationen von relationalen Operatoren erfordern ist testen und stellen Sie mindestens ein Mindestmaß an überladenen Operatoren logische Konsistenz sicher.)

Im Gegensatz zu relationalen Operatoren wird überladen der Division und die Divisionsoperatoren der ganzzahligen dringend abgeraten, jedoch nicht um einen Fehler. (__Beachten.__ Im Allgemeinen muss die beiden Arten von Abteilung völlig unterschiedliche: ein Typ, unterstützt der Division, ist entweder ganzzahlige (in diesem Fall es unterstützen soll `\`) oder nicht (in diesem Fall es unterstützen soll `/`). Wir als somit einen Fehler, wenn beide Operatoren definieren, aber da die Sprache nicht in der Regel zwischen zwei Typen der Abteilung wie wie Visual Basic unterschieden werden, wir sind der Ansicht, dass es am sichersten, können die Methode jedoch dringend davon abgeraten, es war.)

Verbundzuweisung, die Operatoren können nicht direkt überladen werden. Wenn der entsprechende binäre Operator überladen ist, wird der Verbundzuweisungsoperator den überladenen Operator verwenden. Zum Beispiel:

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

Konvertierungsoperatoren definieren neue Konvertierungen zwischen Typen. Diese neuen Konvertierungen heißen *benutzerdefinierte Konvertierungen*. Ein Konvertierungsoperator konvertiert aus einer Datenquelle, die von der Parametertyp des Konvertierungsoperators in einen Zieltyp, angegeben durch den Rückgabetyp der Operator für die Konvertierung angegeben. Konvertierungen müssen als erweiternd oder einschränkend klassifiziert werden. Eine Konvertierung Operator-Deklaration, enthält die `Widening` -Schlüsselwort Führt eine benutzerdefinierte widening-Konvertierung (entsprechende-Methode: `op_Implicit`). Eine Konvertierung Operator-Deklaration, enthält die `Narrowing` -Schlüsselwort Führt eine benutzerdefinierte einschränkende Konvertierung (entsprechende-Methode: `op_Explicit`).

Im Allgemeinen sollten benutzerdefinierte erweiternde Konvertierungen entworfen werden, um keine Ausnahmen auslösen und keine Informationen verlieren. Wenn eine benutzerdefinierte Konvertierung dazu führen, Ausnahmen dass kann (z. B. weil das Quellargument außerhalb des gültigen Bereichs ist) oder Verlust von Informationen (z. B. das höherwertige Bits werden verworfen), und klicken Sie dann auf diese Konvertierung als eine einschränkende Konvertierung definiert werden sollte. Im folgenden Beispiel

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

die Konvertierung von `Digit` zu `Byte` ist eine erweiternde Konvertierung, da sie nie Ausnahmen auslöst oder Informationen, aber die Konvertierung von verliert `Byte` zu `Digit` ist eine einschränkende Konvertierung seit `Digit` können nur darstellen einer die Teilmenge der möglichen Werte einer `Byte`.

Im Gegensatz zu allen anderen Typmember, die überladen werden können, enthält die Signatur eines Konvertierungsoperators den Zieltyp der Konvertierung. Dies ist der einzige Typmember, die für den Rückgabetyp in der Signatur beteiligt ist. Die erweiternde oder einschränkende Klassifizierung eines Konvertierungsoperators ist jedoch nicht Teil der Signatur des Operators. Eine Klasse oder Struktur kann daher nicht sowohl eine erweiternde Konvertierungsoperator als auch eine einschränkende Konvertierungsoperator mit derselben Quelle und Zieltypen deklarieren.

Operator für die benutzerdefinierte Konvertierung muss zum oder vom enthaltenden Typ konvertieren: Es ist beispielsweise möglich, dass eine Klasse `C` definieren Sie eine Konvertierung von `C` zu `Integer` und `Integer` zu `C`, jedoch nicht aus `Integer` zu `Boolean`. Wenn der enthaltende Typ ein generischer Typ ist, müssen die Typparameter des enthaltenden Typs-Typ-Parametern übereinstimmen. Darüber hinaus ist es nicht möglich, um eine Konvertierung (d. h. nicht-benutzerdefiniert) neu zu definieren. Daher Deklarieren eines Typs kann keine Konvertierung, in denen:

* Quell- und Zieltyp sind identisch.

* Sowohl die Quell- und Zieltyp sind nicht der Typ, der den Konvertierungsoperator definiert.

* Der Quelltyp oder Zieltyp ist ein Schnittstellentyp.

* Die Quell- und Zieltypen sind durch Vererbung verwandt sind (einschließlich `Object`).

Diese Regeln die einzige Ausnahme gilt für auf NULL festlegbare Werttypen. Da auf NULL festlegbare Werttypen keine Definition der tatsächliche Typ verfügen, kann ein Werttyp benutzerdefinierte Konvertierungen, die auf NULL festlegbare Version des Typs deklarieren. Beim bestimmen, ob eine bestimmte benutzerdefinierte Konvertierung, einen Typ deklarieren, kann die `?` Modifizierer sind im Rahmen der Prüfung der Gültigkeit auf alle Typen in der Deklaration beteiligten zuerst gelöscht. Daher ist die folgende Deklaration gültig da `S` können definieren, eine Konvertierung von `S` zu `T`:

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

Die folgende Deklaration ist gültig, jedoch nicht, da Struktur `S` kann nicht definiert eine Konvertierung von `S` zu `S`:

```vb
Structure S
    Public Shared Widening Operator CType(ByVal v As S) As S?
        ...
    End Operator
End Structure
```

### <a name="operator-mapping"></a>Operator-Zuordnung

Da es sich bei der Satz von Standardabfrageoperatoren, die Visual Basic unterstützt nicht genau dem Satz von Standardabfrageoperatoren, andere Sprachen in .NET Framework möglicherweise übereinstimmen, werden einige Operatoren zugeordnet, speziell auf andere Operatoren definiert oder verwendet wird. Dies gilt insbesondere in folgenden Fällen:

* Definieren eines Operators ganzzahligen Division wird automatisch eine reguläre Divisionsoperator (nur in anderen Sprachen verwendet werden) definiert, die den ganzzahligen Divisionsoperator aufgerufen wird.

* Das Überladen der `Not`, `And`, und `Or` Operatoren werden nur den bitweisen Operator aus der Perspektive der anderen Sprachen, die Unterscheidung zwischen logische und bitweise Operatoren überladen.

* Eine Klasse, die nur die logischen Operatoren in einer anderen Sprache Überladungen, die zwischen logische und bitweise Operatoren unterscheidet (z. B. ein Sprachen, die verwendet `op_LogicalNot`, `op_LogicalAnd`, und `op_LogicalOr` für `Not`, `And`, und `Or`bzw.) müssen ihre logischen Operatoren, die auf die logischen Operatoren mit Visual Basic zugeordnet. Wenn die logischen und bitweisen Operatoren überladen werden, werden nur die bitweisen Operatoren verwendet werden.

* Das Überladen der `<<` und `>>` Operatoren werden nur die mit Operatoren aus der Perspektive der anderen Sprachen, die Unterscheidung zwischen mit und ohne Vorzeichen Shift-Operatoren überladen.

* Eine Klasse, die nur einen nicht signierte Shift-Operator überlädt, wird den nicht signierte Shift-Operator, der auf der entsprechenden Visual Basic-Shift-Operator zugeordnet haben. Wenn sowohl ein nicht signierter und signierte Shift-Operator überladen ist, wird nur der signierten Shift-Operator verwendet werden.
