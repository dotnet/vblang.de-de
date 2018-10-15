# <a name="general-concepts"></a><span data-ttu-id="8e236-101">Allgemeine Konzepte</span><span class="sxs-lookup"><span data-stu-id="8e236-101">General Concepts</span></span>

<span data-ttu-id="8e236-102">In diesem Kapitel werden einige Konzepte, die erforderlich sind, um die Semantik der Microsoft Visual Basic-Sprache zu verstehen.</span><span class="sxs-lookup"><span data-stu-id="8e236-102">This chapter covers a number of concepts that are required to understand the semantics of the Microsoft Visual Basic language.</span></span> <span data-ttu-id="8e236-103">Viele der Konzepte sollten auf Visual Basic-Programmierer oder C/C++-Programmierer vertraut sein, aber ihre genauen Definitionen unterschiedlich.</span><span class="sxs-lookup"><span data-stu-id="8e236-103">Many of the concepts should be familiar to Visual Basic programmers or C/C++ programmers, but their precise definitions may differ.</span></span>

## <a name="declarations"></a><span data-ttu-id="8e236-104">Deklarationen</span><span class="sxs-lookup"><span data-stu-id="8e236-104">Declarations</span></span>

<span data-ttu-id="8e236-105">Visual Basic-Programms benannten Entitäten besteht.</span><span class="sxs-lookup"><span data-stu-id="8e236-105">A Visual Basic program is made up of named entities.</span></span> <span data-ttu-id="8e236-106">Diese Entitäten werden eingeführt, über *Deklarationen* und die "Bedeutung" des Programms darstellen.</span><span class="sxs-lookup"><span data-stu-id="8e236-106">These entities are introduced through *declarations* and represent the "meaning" of the program.</span></span>

<span data-ttu-id="8e236-107">Auf der obersten Ebene *Namespaces* sind Entitäten, die anderen Entitäten, z. B. geschachtelte Namespaces und Typen zu organisieren.</span><span class="sxs-lookup"><span data-stu-id="8e236-107">At a top level, *namespaces* are entities that organize other entities, such as nested namespaces and types.</span></span> <span data-ttu-id="8e236-108">*Typen* sind Entitäten, die Werte beschreiben, und Definieren von ausführbarem Code.</span><span class="sxs-lookup"><span data-stu-id="8e236-108">*Types* are entities that describe values and define executable code.</span></span> <span data-ttu-id="8e236-109">Typen möglicherweise enthalten geschachtelte Typen und Typmember.</span><span class="sxs-lookup"><span data-stu-id="8e236-109">Types may contain nested types and type members.</span></span> <span data-ttu-id="8e236-110">*Typmember* sind Konstanten, Variablen, Methoden, Operatoren, Eigenschaften, Ereignisse, Enumerationswerte und Konstruktoren.</span><span class="sxs-lookup"><span data-stu-id="8e236-110">*Type members* are constants, variables, methods, operators, properties, events, enumeration values, and constructors.</span></span>

<span data-ttu-id="8e236-111">Definiert eine Entität, die andere Entitäten enthalten, kann ein *Deklarationsabschnitt*.</span><span class="sxs-lookup"><span data-stu-id="8e236-111">An entity that can contain other entities defines a *declaration space*.</span></span> <span data-ttu-id="8e236-112">Entitäten sind in einem Deklarationsabschnitt über Deklarationen oder Vererbung eingeführt. die enthaltende Deklarationsabschnitt wird aufgerufen, der Entitäten *Deklarationskontext*.</span><span class="sxs-lookup"><span data-stu-id="8e236-112">Entities are introduced into a declaration space either through declarations or inheritance; the containing declaration space is called the entities' *declaration context*.</span></span> <span data-ttu-id="8e236-113">Deklarieren eine Entität in einem Deklarationsabschnitt im Gegenzug definiert einen neuen Deklarationsabschnitt, der weitere geschachtelte Entitätsdeklarationen enthalten kann. Daher bilden die Deklarationen in einem Programm eine Hierarchie der Deklaration Leerzeichen.</span><span class="sxs-lookup"><span data-stu-id="8e236-113">Declaring an entity in a declaration space in turn defines a new declaration space that can contain further nested entity declarations; thus, the declarations in a program form a hierarchy of declaration spaces.</span></span>

<span data-ttu-id="8e236-114">Mit Ausnahme von ist es im Fall von überladenen Typmembern ungültig für Deklarationen identisch benannte Entitäten der gleichen Art in der gleichen Deklarationskontext eingeführt.</span><span class="sxs-lookup"><span data-stu-id="8e236-114">Except in the case of overloaded type members, it is invalid for declarations to introduce identically named entities of the same kind into the same declaration context.</span></span> <span data-ttu-id="8e236-115">Darüber hinaus kann ein Deklarationsabschnitt nie verschiedene Arten von Entitäten mit dem gleichen Namen enthalten; Beispielsweise kann ein Deklarationsabschnitt niemals eine Variable und eine Methode mit dem gleichen Namen enthalten.</span><span class="sxs-lookup"><span data-stu-id="8e236-115">Additionally, a declaration space may never contain different kinds of entities with the same name; for example, a declaration space may never contain a variable and a method by the same name.</span></span>

<span data-ttu-id="8e236-116">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="8e236-116">__Note.__</span></span> <span data-ttu-id="8e236-117">Es kann in anderen Sprachen eine Deklaration Platz zu schaffen, die verschiedene Arten von Entitäten mit demselben Namen enthält (beispielsweise, wenn die Sprache Groß-/Kleinschreibung beachtet ist und andere Deklarationen, die basierend auf Groß-/Kleinschreibung ermöglicht) möglich sein.</span><span class="sxs-lookup"><span data-stu-id="8e236-117">It may be possible in other languages to create a declaration space that contains different kinds of entities with the same name (for example, if the language is case sensitive and allows different declarations based on casing).</span></span> <span data-ttu-id="8e236-118">In diesem Fall wird die zugänglichste Entität, die an diesen Namen gebunden betrachtet. Wenn mehr als einen Typ der Entität am häufigsten zugegriffen werden kann, ist der Name nicht eindeutig.</span><span class="sxs-lookup"><span data-stu-id="8e236-118">In that situation, the most accessible entity is considered bound to that name; if more than one type of entity is most accessible then the name is ambiguous.</span></span> <span data-ttu-id="8e236-119">`Public` ist zugänglicher als `Protected Friend`, `Protected Friend` ist zugänglicher als `Protected` oder `Friend`, und `Protected` oder `Friend` ist zugänglicher als `Private`.</span><span class="sxs-lookup"><span data-stu-id="8e236-119">`Public` is more accessible than `Protected Friend`, `Protected Friend` is more accessible than `Protected` or `Friend`, and `Protected` or `Friend` is more accessible than `Private`.</span></span>

<span data-ttu-id="8e236-120">Deklarationsbereich eines Namespace ist "beendet, öffnen Sie", also zwei Namespacedeklarationen mit den gleichen vollqualifizierten Namen zum selben Deklarationsabschnitt beitragen.</span><span class="sxs-lookup"><span data-stu-id="8e236-120">The declaration space of a namespace is "open ended," so two namespace declarations with the same fully qualified name contribute to the same declaration space.</span></span> <span data-ttu-id="8e236-121">Im folgenden Beispiel wird die beiden Namespacedeklarationen, die zum selben Deklarationsabschnitt, deklarieren zwei Klassen mit den vollqualifizierten Namen in diesem Fall beitragen `Data.Customer` und `Data.Order:`</span><span class="sxs-lookup"><span data-stu-id="8e236-121">In the example below, the two namespace declarations contribute to the same declaration space, in this case declaring two classes with the fully qualified names `Data.Customer` and `Data.Order:`</span></span>

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

<span data-ttu-id="8e236-122">Da die beiden Deklarationen zum selben Deklarationsabschnitt beitragen möchten, würde ein Kompilierungsfehler auftreten, wenn es sich bei jeweils eine Deklaration einer Klasse mit dem gleichen Namen enthalten.</span><span class="sxs-lookup"><span data-stu-id="8e236-122">Because the two declarations contribute to the same declaration space, a compile-time error would occur if each contained a declaration of a class with the same name.</span></span>

### <a name="overloading-and-signatures"></a><span data-ttu-id="8e236-123">Überladen und Signaturen</span><span class="sxs-lookup"><span data-stu-id="8e236-123">Overloading and Signatures</span></span>

<span data-ttu-id="8e236-124">Ist die einzige Möglichkeit, Dateien mit identischem Namen Entitäten auf der gleichen Art in einem Deklarationsabschnitt *überladen*.</span><span class="sxs-lookup"><span data-stu-id="8e236-124">The only way to declare identically named entities of the same kind in a declaration space is through *overloading*.</span></span> <span data-ttu-id="8e236-125">Nur Methoden, Operatoren, Instanzkonstruktoren und Eigenschaften können überladen werden.</span><span class="sxs-lookup"><span data-stu-id="8e236-125">Only methods, operators, instance constructors, and properties may be overloaded.</span></span>

<span data-ttu-id="8e236-126">Überladene Typmember müssen es sich um eindeutige Signaturen verfügen.</span><span class="sxs-lookup"><span data-stu-id="8e236-126">Overloaded type members must possess unique signatures.</span></span> <span data-ttu-id="8e236-127">Die Signatur eines Typmembers besteht aus der die Anzahl der Typparameter, und die Anzahl und Typen der Parameter für den Member.</span><span class="sxs-lookup"><span data-stu-id="8e236-127">The signature of a type member consists of the number of type parameters, and the number and types of the member's parameters.</span></span> <span data-ttu-id="8e236-128">Konvertierungsoperatoren umfassen auch den Rückgabetyp des Operators, in der Signatur.</span><span class="sxs-lookup"><span data-stu-id="8e236-128">Conversion operators also include the return type of the operator in the signature.</span></span>

<span data-ttu-id="8e236-129">Im folgenden sind nicht Teil der Signatur eines Members, und daher kann nicht überladen werden, auf:</span><span class="sxs-lookup"><span data-stu-id="8e236-129">The following are not part of a member's signature, and hence cannot be overloaded on:</span></span>

* <span data-ttu-id="8e236-130">Modifizierer auf einen Typmember (z. B. `Shared` oder `Private`).</span><span class="sxs-lookup"><span data-stu-id="8e236-130">Modifiers to a type member (for example, `Shared` or `Private`).</span></span>

* <span data-ttu-id="8e236-131">Modifizierer auf einen Parameter (z. B. `ByVal` oder `ByRef`).</span><span class="sxs-lookup"><span data-stu-id="8e236-131">Modifiers to a parameter (for example, `ByVal` or `ByRef`).</span></span>

* <span data-ttu-id="8e236-132">Die Namen der Parameter.</span><span class="sxs-lookup"><span data-stu-id="8e236-132">The names of the parameters.</span></span>

* <span data-ttu-id="8e236-133">Der Rückgabetyp einer Methode oder -Operator (außer den Konvertierungsoperatoren) oder den Typ des Elements, einer Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="8e236-133">The return type of a method or operator (except for conversion operators) or the element type of a property.</span></span>

* <span data-ttu-id="8e236-134">Einschränkungen für einen Typparameter.</span><span class="sxs-lookup"><span data-stu-id="8e236-134">Constraints on a type parameter.</span></span>

<span data-ttu-id="8e236-135">Das folgende Beispiel zeigt eine Reihe von Deklarationen der überladenen Methode zusammen mit ihren Signaturen.</span><span class="sxs-lookup"><span data-stu-id="8e236-135">The following example shows a set of overloaded method declarations along with their signatures.</span></span> <span data-ttu-id="8e236-136">Diese Deklaration wäre nicht gültig, da mehrere die Methodendeklarationen mit identische Signaturen haben.</span><span class="sxs-lookup"><span data-stu-id="8e236-136">This declaration would not be valid since several of the method declarations have identical signatures.</span></span>

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

<span data-ttu-id="8e236-137">Es ist zulässig, einen generischen Typ definieren, der mit identischen Signaturen basierend auf den angegebenen Typargumente Elemente enthalten kann.</span><span class="sxs-lookup"><span data-stu-id="8e236-137">It is valid to define a generic type that may contain members with identical signatures based on the type arguments supplied.</span></span> <span data-ttu-id="8e236-138">Regeln der überladungsauflösung werden zum Testen und zu unterscheiden zwischen solche Überladungen, obwohl es gibt möglicherweise Situationen, in denen es unmöglich, zu unterscheiden ist.</span><span class="sxs-lookup"><span data-stu-id="8e236-138">Overload resolution rules are used to try and disambiguate between such overloads, although there may be situations in which it is impossible to disambiguate.</span></span> <span data-ttu-id="8e236-139">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-139">For example:</span></span>

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

## <a name="scope"></a><span data-ttu-id="8e236-140">Bereich</span><span class="sxs-lookup"><span data-stu-id="8e236-140">Scope</span></span>

<span data-ttu-id="8e236-141">Die *Bereich* einer Entität namens ist ein Satz von allen Deklaration Leerzeichen in dem es möglich ist, die auf diesen Namen ohne Qualifikation verweisen.</span><span class="sxs-lookup"><span data-stu-id="8e236-141">The *scope* of an entity's name is the set of all declaration spaces within which it is possible to refer to that name without qualification.</span></span> <span data-ttu-id="8e236-142">Im Allgemeinen ist der Geltungsbereich eines Entitätsnamens der gesamte Deklarationskontext; die Deklaration einer Entität kann allerdings geschachtelte Deklarationen von Entitäten mit dem gleichen Namen enthalten.</span><span class="sxs-lookup"><span data-stu-id="8e236-142">In general, the scope of an entity's name is its entire declaration context; however, an entity's declaration may contain nested declarations of entities with the same name.</span></span> <span data-ttu-id="8e236-143">In diesem Fall die verschachtelter Entity *Shadows*, bzw. ausgeblendet, die äußere Entität und Zugriff auf die Entität Shadowing ist nur möglich, durch die Qualifizierung.</span><span class="sxs-lookup"><span data-stu-id="8e236-143">In that case, the nested entity *shadows*, or hides, the outer entity, and access to the shadowed entity is only possible through qualification.</span></span>

<span data-ttu-id="8e236-144">Shadowing über Schachtelung tritt auf, in Namespaces oder Typen in Namespaces geschachtelt, in andere Typen geschachtelten Typen und im Textkörper der Typmember.</span><span class="sxs-lookup"><span data-stu-id="8e236-144">Shadowing through nesting occurs in namespaces or types nested within namespaces, in types nested within other types, and in the bodies of type members.</span></span> <span data-ttu-id="8e236-145">Shadowings durch die Schachtelung von Deklarationen immer auftritt implizit. Es ist keine explizite Syntax erforderlich.</span><span class="sxs-lookup"><span data-stu-id="8e236-145">Shadowing through the nesting of declarations always occurs implicitly; no explicit syntax is required.</span></span>

<span data-ttu-id="8e236-146">Im folgenden Beispiel in der `F` -Methode, die Instanzvariable `i` wird durch die lokale Variable überschattet `i`, aber noch innerhalb der `G` -Methode, `i` weiterhin auf die Instanzvariable verweist.</span><span class="sxs-lookup"><span data-stu-id="8e236-146">In the following example, within the `F` method, the instance variable `i` is shadowed by the local variable `i`, but within the `G` method, `i` still refers to the instance variable.</span></span>

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

<span data-ttu-id="8e236-147">Wenn Sie einen Namen in einem äußeren Bereich ein Namen in einem inneren Gültigkeitsbereich verdeckt führt Shadowing für es alle überladene Vorkommen dieses Namens.</span><span class="sxs-lookup"><span data-stu-id="8e236-147">When a name in an inner scope hides a name in an outer scope, it shadows all overloaded occurrences of that name.</span></span> <span data-ttu-id="8e236-148">Im folgenden Beispiel der Aufruf `F(1)` Ruft die `F` in deklarierten `Inner` da alle äußeren Vorkommen von `F` werden durch die innere Deklaration ausgeblendet.</span><span class="sxs-lookup"><span data-stu-id="8e236-148">In the following example, the call `F(1)` invokes the `F` declared in `Inner` because all outer occurrences of `F` are hidden by the inner declaration.</span></span> <span data-ttu-id="8e236-149">Aus demselben Grund, den Aufruf `F("Hello")` ist fehlerhaft.</span><span class="sxs-lookup"><span data-stu-id="8e236-149">For the same reason, the call `F("Hello")` is in error.</span></span>

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

## <a name="inheritance"></a><span data-ttu-id="8e236-150">Vererbung</span><span class="sxs-lookup"><span data-stu-id="8e236-150">Inheritance</span></span>

<span data-ttu-id="8e236-151">Eine vererbungsbeziehung wird in der ein Typ (der *abgeleitet* Typ) von einem anderen abgeleitet ist (die *Basis* Typ), sodass der abgeleitete Typ Deklarationsabschnitt implizit den zugegriffen werden kann enthält nicht-Konstruktor-Typmember und geschachtelte Typen von seinem Basistyp.</span><span class="sxs-lookup"><span data-stu-id="8e236-151">An inheritance relationship is one in which one type (the *derived* type) derives from another (the *base* type), such that the derived type's declaration space implicitly contains the accessible non-constructor type members and nested types of its base type.</span></span> <span data-ttu-id="8e236-152">Im folgenden Beispiel Klasse `A` ist die Basisklasse der `B`, und `B` ergibt sich aus `A`.</span><span class="sxs-lookup"><span data-stu-id="8e236-152">In the following example, class `A` is the base class of `B`, and `B` is derived from `A`.</span></span>

```vb
Class A
End Class

Class B
    Inherits A
End Class
```

<span data-ttu-id="8e236-153">Da `A` ist nicht explizit keine Basisklasse angeben, der Basisklasse ist implizit `Object`.</span><span class="sxs-lookup"><span data-stu-id="8e236-153">Since `A` does not explicitly specify a base class, its base class is implicitly `Object`.</span></span>

<span data-ttu-id="8e236-154">Im folgenden sind wichtige Aspekte bei der Vererbung:</span><span class="sxs-lookup"><span data-stu-id="8e236-154">The following are important aspects of inheritance:</span></span>

* <span data-ttu-id="8e236-155">Vererbung ist transitiv.</span><span class="sxs-lookup"><span data-stu-id="8e236-155">Inheritance is transitive.</span></span> <span data-ttu-id="8e236-156">Wenn Typ *C* Typ abgeleitet ist *B*, und geben *B* Typ abgeleitet ist *ein*, Typ *C* erbt die Geben Sie im Typ deklarierten Member *B* sowie die Typmember in Typ deklariert *ein*.</span><span class="sxs-lookup"><span data-stu-id="8e236-156">If type *C* is derived from type *B*, and type *B* is derived from type *A*, type *C* inherits the type members declared in type *B* as well as the type members declared in type *A*.</span></span>

* <span data-ttu-id="8e236-157">Ein abgeleiteter Typ erweitert, aber nicht einschränken, dessen Basistyp.</span><span class="sxs-lookup"><span data-stu-id="8e236-157">A derived type extends, but cannot narrow, its base type.</span></span> <span data-ttu-id="8e236-158">Ein abgeleiteter Typ kann neue Member von Typen, hinzufügen und überschatten geerbten Typmember, aber die Definition eines Members des geerbten Typs kann nicht entfernt.</span><span class="sxs-lookup"><span data-stu-id="8e236-158">A derived type can add new type members, and it can shadow inherited type members, but it cannot remove the definition of an inherited type member.</span></span>

* <span data-ttu-id="8e236-159">Da eine Instanz eines Typs auf alle Typmember von seinem Basistyp enthält, ist immer eine Konvertierung eines abgeleiteten Typs in den Basistyp vorhanden.</span><span class="sxs-lookup"><span data-stu-id="8e236-159">Because an instance of a type contains all of the type members of its base type, a conversion always exists from a derived type to its base type.</span></span>

* <span data-ttu-id="8e236-160">Alle Typen müssen einen Basistyp, mit Ausnahme des Datentyps `Object`.</span><span class="sxs-lookup"><span data-stu-id="8e236-160">All types must have a base type, except for the type `Object`.</span></span> <span data-ttu-id="8e236-161">Daher `Object` ist die ultimative Basistyp aller Typen und alle Typen können konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="8e236-161">Thus, `Object` is the ultimate base type of all types, and all types can be converted to it.</span></span>

* <span data-ttu-id="8e236-162">Zirkularität in Ableitung ist nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="8e236-162">Circularity in derivation is not permitted.</span></span> <span data-ttu-id="8e236-163">D. h. wenn ein Typ `B` von einem Typ abgeleitet `A`, es ist ein Fehler für den Typ `A` direkt oder indirekt vom Typ ableiten `B`.</span><span class="sxs-lookup"><span data-stu-id="8e236-163">That is, when a type `B` derives from a type `A`, it is an error for type `A` to derive directly or indirectly from type `B`.</span></span>

* <span data-ttu-id="8e236-164">Ein Typ kann nicht direkt oder indirekt von einem darin geschachtelten Typ abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="8e236-164">A type may not directly or indirectly derive from a type nested within it.</span></span>

<span data-ttu-id="8e236-165">Im folgenden Beispiel wird einen Fehler während der Kompilierung, da die Klassen zirkulär voneinander abhängig sind.</span><span class="sxs-lookup"><span data-stu-id="8e236-165">The following example produces a compile-time error because the classes circularly depend on each other.</span></span>

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

<span data-ttu-id="8e236-166">Im folgenden Beispiel wird auch einen Fehler während der Kompilierung, da `B` indirekt von der geschachtelten Klasse abgeleitet ist `C` über Klasse `A`.</span><span class="sxs-lookup"><span data-stu-id="8e236-166">The following example also produces a compile-time error because `B` indirectly derives from its nested class `C` through class `A`.</span></span>

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

<span data-ttu-id="8e236-167">Im nächste Beispiel ergibt keinen Fehler, da Klasse `A` nicht von der Klasse abgeleitet `B`.</span><span class="sxs-lookup"><span data-stu-id="8e236-167">The next example does not produce an error because class `A` does not derive from class `B`.</span></span>

```vb
Class A
    Class B
        Inherits A
    End Class 
End Class
```

### <a name="mustinherit-and-notinheritable-classes"></a><span data-ttu-id="8e236-168">MustInherit und NotInheritable-Klassen</span><span class="sxs-lookup"><span data-stu-id="8e236-168">MustInherit and NotInheritable Classes</span></span>

<span data-ttu-id="8e236-169">Ein `MustInherit` Klasse ist ein unvollständiger Typ, der nur als Basistyp verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="8e236-169">A `MustInherit` class is an incomplete type that can act only as a base type.</span></span> <span data-ttu-id="8e236-170">Ein `MustInherit` Klasse kann nicht instanziiert werden, daher wird ein Fehler, die `New` Operator auf eine.</span><span class="sxs-lookup"><span data-stu-id="8e236-170">A `MustInherit` class cannot be instantiated, so it is an error to use the `New` operator on one.</span></span> <span data-ttu-id="8e236-171">Es ist zulässig, die Variablen deklarieren, `MustInherit` Klassen; diese Variablen können nur zugewiesen werden `Nothing` oder ein Wert, der von einer Klasse abgeleitet ist die `MustInherit` Klasse.</span><span class="sxs-lookup"><span data-stu-id="8e236-171">It is valid to declare variables of `MustInherit` classes; such variables can only be assigned `Nothing` or a value that is of a class derived from the `MustInherit` class.</span></span>

<span data-ttu-id="8e236-172">Bei eine normale Klasse abgeleitet ist eine `MustInherit` -Klasse, die reguläre Klasse muss außer Kraft setzen alle geerbten `MustOverride` Member.</span><span class="sxs-lookup"><span data-stu-id="8e236-172">When a regular class is derived from a `MustInherit` class, the regular class must override all inherited `MustOverride` members.</span></span> <span data-ttu-id="8e236-173">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-173">For example:</span></span>

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

<span data-ttu-id="8e236-174">Die `MustInherit` Klasse `A` führt eine `MustOverride` Methode `F`.</span><span class="sxs-lookup"><span data-stu-id="8e236-174">The `MustInherit` class `A` introduces a `MustOverride` method `F`.</span></span> <span data-ttu-id="8e236-175">Klasse `B` führt eine zusätzliche Methode `G`, jedoch keine Implementierung von `F`.</span><span class="sxs-lookup"><span data-stu-id="8e236-175">Class `B` introduces an additional method `G`, but does not provide an implementation of `F`.</span></span> <span data-ttu-id="8e236-176">Klasse `B` muss daher ebenfalls deklariert werden `MustInherit`.</span><span class="sxs-lookup"><span data-stu-id="8e236-176">Class `B` must therefore also be declared `MustInherit`.</span></span> <span data-ttu-id="8e236-177">Klasse `C` überschreibt `F` und eine tatsächliche Implementierung bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="8e236-177">Class `C` overrides `F` and provides an actual implementation.</span></span> <span data-ttu-id="8e236-178">Da es sich um nicht ausstehende `MustOverride` Member in Klasse `C`, es ist nicht erforderlich sein `MustInherit`.</span><span class="sxs-lookup"><span data-stu-id="8e236-178">Since there are no outstanding `MustOverride` members in class `C`, it is not required to be `MustInherit`.</span></span>

<span data-ttu-id="8e236-179">Ein `NotInheritable` ist eine Klasse, die von der eine andere Klasse kann nicht abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="8e236-179">A `NotInheritable` class is a class from which another class cannot be derived.</span></span> <span data-ttu-id="8e236-180">`NotInheritable` Klassen werden in erster Linie zum Verhindern von unbeabsichtigten ableitungen.</span><span class="sxs-lookup"><span data-stu-id="8e236-180">`NotInheritable` classes are primarily used to prevent unintended derivation.</span></span>

<span data-ttu-id="8e236-181">In diesem Beispiel-Klasse `B` ist falsch, weil er versucht, für die Ableitung der `NotInheritable` Klasse `A`.</span><span class="sxs-lookup"><span data-stu-id="8e236-181">In this example, class `B` is in error because it attempts to derive from the `NotInheritable` class `A`.</span></span> <span data-ttu-id="8e236-182">Eine Klasse kann nicht markiert werden, beide `MustInherit` und `NotInheritable`.</span><span class="sxs-lookup"><span data-stu-id="8e236-182">A class cannot be marked both `MustInherit` and `NotInheritable`.</span></span>

```vb
NotInheritable Class A
End Class

Class B
    ' Error, a class cannot derive from a NotInheritable class.
    Inherits A
End Class
```

### <a name="interfaces-and-multiple-inheritance"></a><span data-ttu-id="8e236-183">Schnittstellen und Mehrfachvererbung</span><span class="sxs-lookup"><span data-stu-id="8e236-183">Interfaces and Multiple Inheritance</span></span>

<span data-ttu-id="8e236-184">Im Gegensatz zu anderen Typen, die nur von einem einzelnen Basistyp abgeleitet werden, kann eine Schnittstelle von mehreren Basisschnittstellen abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="8e236-184">Unlike other types, which only derive from a single base type, an interface may derive from multiple base interfaces.</span></span> <span data-ttu-id="8e236-185">Aus diesem Grund kann eine Schnittstelle einen mit dem gleichen Namen Typmember von unterschiedliche Basisschnittstellen erben.</span><span class="sxs-lookup"><span data-stu-id="8e236-185">Because of this, an interface can inherit an identically named type member from different base interfaces.</span></span> <span data-ttu-id="8e236-186">In diesem Fall die Multiplizieren Sie geerbte Name ist in der abgeleiteten Schnittstelle nicht verfügbar, und verweisen auf diesen Typ Member durch die abgeleitete Schnittstelle bewirkt, dass einen Fehler während der Kompilierung, unabhängig von Signaturen und überladen.</span><span class="sxs-lookup"><span data-stu-id="8e236-186">In such a case, the multiply-inherited name is not available in the derived interface, and referring to any of those type members through the derived interface causes a compile-time error, regardless of signatures or overloading.</span></span> <span data-ttu-id="8e236-187">In Konflikt stehende Member des Typs müssen stattdessen über einen Namen für die Basisschnittstelle verwiesen werden.</span><span class="sxs-lookup"><span data-stu-id="8e236-187">Instead, conflicting type members must be referenced through a base interface name.</span></span>

<span data-ttu-id="8e236-188">Im folgenden Beispiel die ersten beiden Anweisungen verursachen Kompilierzeitfehler, da der Multiplizieren Sie geerbte Member `Count` ist nicht verfügbar in der Schnittstelle `IListCounter`:</span><span class="sxs-lookup"><span data-stu-id="8e236-188">In the following example, the first two statements cause compile-time errors because the multiply-inherited member `Count` is not available in interface `IListCounter`:</span></span>

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

<span data-ttu-id="8e236-189">Wie im Beispiel gezeigt wird, wird die Mehrdeutigkeit aufgelöst, indem Sie eine Umwandlung `x` in den entsprechenden Basisschnittstelle-Typ.</span><span class="sxs-lookup"><span data-stu-id="8e236-189">As illustrated by the example, the ambiguity is resolved by casting `x` to the appropriate base interface type.</span></span> <span data-ttu-id="8e236-190">Solche Umwandlungen haben keine Laufzeit; Sie bestehen lediglich zum Anzeigen der Instanz als weniger abgeleiteten Typ zum Zeitpunkt der Kompilierung aus.</span><span class="sxs-lookup"><span data-stu-id="8e236-190">Such casts have no run-time costs; they merely consist of viewing the instance as a less-derived type at compile time.</span></span>

<span data-ttu-id="8e236-191">Wenn von der gleichen Basisschnittstelle über mehrere Pfade ein single-Typelmember geerbt wird, wird der Typmember behandelt, als ob sie nur einmal übernommen wurden.</span><span class="sxs-lookup"><span data-stu-id="8e236-191">When a single type member is inherited from the same base interface through multiple paths, the type member is treated as if it were only inherited once.</span></span> <span data-ttu-id="8e236-192">Das heißt, enthält die abgeleitete Schnittstelle nur eine Instanz jeder Typmember, die von einer bestimmten Basis Schnittstelle geerbt.</span><span class="sxs-lookup"><span data-stu-id="8e236-192">In other words, the derived interface only contains one instance of each type member inherited from a particular base interface.</span></span> <span data-ttu-id="8e236-193">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-193">For example:</span></span>

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

<span data-ttu-id="8e236-194">Wenn der Name eines Typmembers, die in einem Pfad über die Hierarchie der klassenvererbung überschattet ist, wird der Name in allen Pfaden schattiert.</span><span class="sxs-lookup"><span data-stu-id="8e236-194">If a type member name is shadowed in one path through the inheritance hierarchy, then the name is shadowed in all paths.</span></span> <span data-ttu-id="8e236-195">Im folgenden Beispiel die `IBase.F` Member wird schattiert, durch die `ILeft.F` Member, aber wird nicht überschattet `IRight`:</span><span class="sxs-lookup"><span data-stu-id="8e236-195">In the following example, the `IBase.F` member is shadowed by the `ILeft.F` member, but is not shadowed in `IRight`:</span></span>

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

<span data-ttu-id="8e236-196">Der Aufruf `d.F(1)` wählt `ILeft.F`, auch wenn `IBase.F` angezeigt wird, nicht in den Zugriffspfad ein Shadowing ausgeführt, das durch führt `IRight`.</span><span class="sxs-lookup"><span data-stu-id="8e236-196">The invocation `d.F(1)` selects `ILeft.F`, even though `IBase.F` appears to not be shadowed in the access path that leads through `IRight`.</span></span> <span data-ttu-id="8e236-197">Da Zugriffspfad aus `IDerived` zu `ILeft` zu `IBase` Shadows `IBase.F`, das Element wird auch in den Zugriffspfad aus überschattet `IDerived` zu `IRight` zu `IBase`.</span><span class="sxs-lookup"><span data-stu-id="8e236-197">Because the access path from `IDerived` to `ILeft` to `IBase` shadows `IBase.F`, the member is also shadowed in the access path from `IDerived` to `IRight` to `IBase`.</span></span>

### <a name="shadowing"></a><span data-ttu-id="8e236-198">Shadowing</span><span class="sxs-lookup"><span data-stu-id="8e236-198">Shadowing</span></span>

<span data-ttu-id="8e236-199">Ein abgeleiteter Typ führt Shadowing für den Namen eines Members des geerbten Typs deklarieren Sie es erneut.</span><span class="sxs-lookup"><span data-stu-id="8e236-199">A derived type shadows the name of an inherited type member by re-declaring it.</span></span> <span data-ttu-id="8e236-200">Ein Name-Shadowing entfernt nicht die geerbten Typmember mit diesem Namen. Es werden alle geerbten Typmember mit diesem Namen nur in der abgeleiteten Klasse nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="8e236-200">Shadowing a name does not remove the inherited type members with that name; it merely makes all of the inherited type members with that name unavailable in the derived class.</span></span> <span data-ttu-id="8e236-201">Die shadowing-Deklaration kann jede Art von Entität sein.</span><span class="sxs-lookup"><span data-stu-id="8e236-201">The shadowing declaration may be any type of entity.</span></span>

<span data-ttu-id="8e236-202">Entitäten sind überladen werden können, können zwei Formen des shadowings auswählen.</span><span class="sxs-lookup"><span data-stu-id="8e236-202">Entities than can be overloaded can choose one of two forms of shadowing.</span></span> <span data-ttu-id="8e236-203">*Nach dem Namen Shadowing* angegeben ist, mit der `Shadows` Schlüsselwort.</span><span class="sxs-lookup"><span data-stu-id="8e236-203">*Shadowing by name* is specified using the `Shadows` keyword.</span></span> <span data-ttu-id="8e236-204">Eine Entität, die nach dem Namen Shadowing durchführt, wird alles, was mit diesem Namen in der Basisklasse, einschließlich aller Überladungen ausgeblendet.</span><span class="sxs-lookup"><span data-stu-id="8e236-204">An entity that shadows by name hides everything by that name in the base class, including all overloads.</span></span> <span data-ttu-id="8e236-205">*Durch den Namen und derselben Signatur Shadowing* angegeben ist, mit der `Overloads` Schlüsselwort.</span><span class="sxs-lookup"><span data-stu-id="8e236-205">*Shadowing by name and signature* is specified using the `Overloads` keyword.</span></span> <span data-ttu-id="8e236-206">Eine Entität, die durch den Namen und derselben Signatur führt Shadowing für alles, was mit diesem Namen mit der gleichen Signatur wie die Entität wird ausgeblendet.</span><span class="sxs-lookup"><span data-stu-id="8e236-206">An entity that shadows by name and signature hides everything by that name with the same signature as the entity.</span></span> <span data-ttu-id="8e236-207">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-207">For example:</span></span>

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

<span data-ttu-id="8e236-208">Shadowing einer Methode mit einem `ParamArray` Argument nach Namen und derselben Signatur verbirgt, nur die einzelne Signatur, die nicht alle möglichen erweiterten Signaturen.</span><span class="sxs-lookup"><span data-stu-id="8e236-208">Shadowing a method with a `ParamArray` argument by name and signature hides only the individual signature, not all possible expanded signatures.</span></span> <span data-ttu-id="8e236-209">Dies gilt auch, wenn die Signatur der Methode shadowing der nicht erweiterte Signatur der Shadowing-Methode entspricht.</span><span class="sxs-lookup"><span data-stu-id="8e236-209">This is true even if the signature of the shadowing method matches the unexpanded signature of the shadowed method.</span></span> <span data-ttu-id="8e236-210">Im folgenden Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-210">The following example:</span></span>

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

<span data-ttu-id="8e236-211">Druckt `Base`, auch wenn `Derived.F` hat die gleiche Signatur wie die nicht erweiterte Form der `Base.F`.</span><span class="sxs-lookup"><span data-stu-id="8e236-211">prints `Base`, even though `Derived.F` has the same signature as the unexpanded form of `Base.F`.</span></span>

<span data-ttu-id="8e236-212">Im Gegensatz dazu eine Methode mit einem `ParamArray` Argument nur führt Shadowing für Methoden mit derselben Signatur nicht alle möglichen erweiterten Signaturen.</span><span class="sxs-lookup"><span data-stu-id="8e236-212">Conversely, a method with a `ParamArray` argument only shadows methods with the same signature, not all possible expanded signatures.</span></span> <span data-ttu-id="8e236-213">Im folgenden Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-213">The following example:</span></span>

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

<span data-ttu-id="8e236-214">Druckt `Base`, auch wenn `Derived.F` verfügt über eine erweiterte Form, die die gleiche Signatur wie `Base.F`.</span><span class="sxs-lookup"><span data-stu-id="8e236-214">prints `Base`, even though `Derived.F` has an expanded form that has the same signature as `Base.F`.</span></span>

<span data-ttu-id="8e236-215">Ein shadowing-Methode oder Eigenschaft an, der nicht `Shadows` oder `Overloads` geht davon aus `Overloads` , wenn die Methode oder Eigenschaft deklariert ist `Overrides`, `Shadows` andernfalls.</span><span class="sxs-lookup"><span data-stu-id="8e236-215">A shadowing method or property that does not specify `Shadows` or `Overloads` assumes `Overloads` if the method or property is declared `Overrides`, `Shadows` otherwise.</span></span> <span data-ttu-id="8e236-216">Wenn ein Mitglied einer Gruppe von überladenen Entitäten gibt an, die `Shadows` oder `Overloads` -Schlüsselwort, alle angegeben.</span><span class="sxs-lookup"><span data-stu-id="8e236-216">If one member of a set of overloaded entities specifies the `Shadows` or `Overloads` keyword, they all must specify it.</span></span> <span data-ttu-id="8e236-217">Die `Shadows` und `Overloads` Schlüsselwörter können nicht gleichzeitig angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="8e236-217">The `Shadows` and `Overloads` keywords cannot be specified at the same time.</span></span> <span data-ttu-id="8e236-218">Weder `Shadows` noch `Overloads` kann in einem Standardmodul angegeben werden; die Elemente in einem Standardmodul implizit von der geerbten Member Shadowing `Object`.</span><span class="sxs-lookup"><span data-stu-id="8e236-218">Neither `Shadows` nor `Overloads` can be specified in a standard module; members in a standard module implicitly shadow members inherited from `Object`.</span></span>

<span data-ttu-id="8e236-219">Es ist zulässig, die Schatten der Name eines Typmembers, wurde, die über schnittstellenvererbung multiply-geerbt (und dies ist dadurch nicht verfügbar), sodass der Name verfügbar in der abgeleiteten Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="8e236-219">It is valid to shadow the name of a type member that has been multiply-inherited through interface inheritance (and which is thereby unavailable), thus making the name available in the derived interface.</span></span>

<span data-ttu-id="8e236-220">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-220">For example:</span></span>

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

<span data-ttu-id="8e236-221">Da es sich bei Methoden zu Schattenkopien geerbte Methoden zulässig sind, ist es möglich, für eine Klasse enthält mehrere `Overridable` Methoden, mit der gleichen Signatur.</span><span class="sxs-lookup"><span data-stu-id="8e236-221">Because methods are allowed to shadow inherited methods, it is possible for a class to contain several `Overridable` methods with the same signature.</span></span> <span data-ttu-id="8e236-222">Dies ist keiner Mehrdeutigkeiten, da nur die am stärksten abgeleiteten Methode sichtbar ist.</span><span class="sxs-lookup"><span data-stu-id="8e236-222">This does not present an ambiguity problem, since only the most-derived method is visible.</span></span> <span data-ttu-id="8e236-223">Im folgenden Beispiel die `C` und `D` Klassen enthalten zwei `Overridable` Methoden, mit der gleichen Signatur:</span><span class="sxs-lookup"><span data-stu-id="8e236-223">In the following example, the `C` and `D` classes contain two `Overridable` methods with the same signature:</span></span>

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

<span data-ttu-id="8e236-224">Es gibt zwei `Overridable` Methoden: eine von Klasse eingeführte `A` und die 1-von-Klasse eingeführt `C`.</span><span class="sxs-lookup"><span data-stu-id="8e236-224">There are two `Overridable` methods here: one introduced by class `A` and the one introduced by class `C`.</span></span> <span data-ttu-id="8e236-225">Die Methode, die von Klasse eingeführte `C` Blendet die Methode, die von der Klasse geerbt `A`.</span><span class="sxs-lookup"><span data-stu-id="8e236-225">The method introduced by class `C` hides the method inherited from class `A`.</span></span> <span data-ttu-id="8e236-226">Daher die `Overrides` -Deklaration in der Klasse `D` überschreibt die Methode, die von Klasse eingeführte `C`, und es ist nicht möglich, für die Klasse `D` , die von Klasse eingeführte Methode überschreiben `A`.</span><span class="sxs-lookup"><span data-stu-id="8e236-226">Thus, the `Overrides` declaration in class `D` overrides the method introduced by class `C`, and it is not possible for class `D` to override the method introduced by class `A`.</span></span> <span data-ttu-id="8e236-227">Das Beispiel erzeugt die Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="8e236-227">The example produces the output:</span></span>

```
B.F
B.F
D.F
D.F
```

<span data-ttu-id="8e236-228">Es ist möglich, rufen Sie die ausgeblendeten `Overridable` Methode, indem Sie den Zugriff auf eine Instanz der Klasse `D` über einen weniger abgeleiteten Typ in der die Methode nicht ausgeblendet ist.</span><span class="sxs-lookup"><span data-stu-id="8e236-228">It is possible to invoke the hidden `Overridable` method by accessing an instance of class `D` through a less-derived type in which the method is not hidden.</span></span>

<span data-ttu-id="8e236-229">Es ist nicht zulässig, für das Shadowing einer `MustOverride` -Methode, da in den meisten Fällen dies die Klasse nicht verwendbar machen würden.</span><span class="sxs-lookup"><span data-stu-id="8e236-229">It is not valid to shadow a `MustOverride` method, because in most cases this would make the class unusable.</span></span> <span data-ttu-id="8e236-230">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-230">For example:</span></span>

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

<span data-ttu-id="8e236-231">In diesem Fall die Klasse `MoreDerived` ist erforderlich, um das Überschreiben der `MustOverride` Methode `Base.F`, aber da die Klasse `Derived` Schatten `Base.F`, dies ist nicht möglich.</span><span class="sxs-lookup"><span data-stu-id="8e236-231">In this case, the class `MoreDerived` is required to override the `MustOverride` method `Base.F`, but because the class `Derived` shadows `Base.F`, this is not possible.</span></span> <span data-ttu-id="8e236-232">Es gibt keine Möglichkeit, deklarieren Sie eine gültige untergeordnete Klasse der `Derived`.</span><span class="sxs-lookup"><span data-stu-id="8e236-232">There is no way to declare a valid descendent of `Derived`.</span></span>

<span data-ttu-id="8e236-233">Im Gegensatz zum shadowing eines Namens in einem äußeren Bereich, führt das shadowing eine barrierefreie Name aus einem geerbten Gültigkeitsbereich einer Warnung gemeldet werden, wie im folgenden Beispiel aus:</span><span class="sxs-lookup"><span data-stu-id="8e236-233">In contrast to shadowing a name from an outer scope, shadowing an accessible name from an inherited scope causes a warning to be reported, as in the following example:</span></span>

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

<span data-ttu-id="8e236-234">Die Deklaration der Methode `F` in Klasse `Derived` bewirkt, dass eine Warnung gemeldet werden.</span><span class="sxs-lookup"><span data-stu-id="8e236-234">The declaration of method `F` in class `Derived` causes a warning to be reported.</span></span> <span data-ttu-id="8e236-235">Shadowing einer geerbten Namens ist ausdrücklich keinen Fehler, da, die separate Entwicklung von Basisklassen ausgeschlossen würden.</span><span class="sxs-lookup"><span data-stu-id="8e236-235">Shadowing an inherited name is specifically not an error, since that would preclude separate evolution of base classes.</span></span> <span data-ttu-id="8e236-236">Z. B. die oben genannten Situation möglicherweise haben vorkommen, da eine neuere Version der Klasse `Base` eingeführt, eine Methode `F` , das war nicht in einer früheren Version der Klasse vorhanden.</span><span class="sxs-lookup"><span data-stu-id="8e236-236">For example, the above situation might have come about because a later version of class `Base` introduced a method `F` that was not present in an earlier version of the class.</span></span> <span data-ttu-id="8e236-237">War der oben genannten Fall eines Fehlers *alle* Änderung an einer Basisklasse in einer separaten Versionen Klassenbibliothek kann dazu führen, abgeleitete Klassen ungültig werden.</span><span class="sxs-lookup"><span data-stu-id="8e236-237">Had the above situation been an error, *any* change made to a base class in a separately versioned class library could potentially cause derived classes to become invalid.</span></span>

<span data-ttu-id="8e236-238">Die Warnung verursacht hat, einen geerbten Namen Shadowing kann behoben werden, mithilfe der `Shadows` oder `Overloads` Modifizierer:</span><span class="sxs-lookup"><span data-stu-id="8e236-238">The warning caused by shadowing an inherited name can be eliminated through use of the `Shadows` or `Overloads` modifier:</span></span>

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

<span data-ttu-id="8e236-239">Die `Shadows` Modifizierer gibt an, dass den geerbten Member zu überschatten.</span><span class="sxs-lookup"><span data-stu-id="8e236-239">The `Shadows` modifier indicates the intention to shadow the inherited member.</span></span> <span data-ttu-id="8e236-240">Es ist kein Fehler an die `Shadows` oder `Overloads` Modifizierer ab, wenn kein Typ Membername zum Shadowing vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="8e236-240">It is not an error to specify the `Shadows` or `Overloads` modifier if there is no type member name to shadow.</span></span>

<span data-ttu-id="8e236-241">Eine Deklaration eines neuen Members führt Shadowing für einen geerbten Member nur innerhalb des Bereichs des neuen Elements wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="8e236-241">A declaration of a new member shadows an inherited member only within the scope of the new member, as in the following example:</span></span>

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

<span data-ttu-id="8e236-242">Im obigen Beispiel ist die Deklaration der Methode `F` in Klasse `Derived` führt Shadowing für die Methode `F` , die aus der Klasse geerbt wurde `Base`, da die neue Methode aber `F` in Klasse `Derived` hat `Private` Zugriff auf ihren Bereich erstreckt sich nicht auf Klasse `MoreDerived`.</span><span class="sxs-lookup"><span data-stu-id="8e236-242">In the example above, the declaration of method `F` in class `Derived` shadows the method `F` that was inherited from class `Base`, but since the new method `F` in class `Derived` has `Private` access, its scope does not extend to class `MoreDerived`.</span></span> <span data-ttu-id="8e236-243">Daher den Aufruf `F()` in `MoreDerived.G` gültig ist und ruft `Base.F`.</span><span class="sxs-lookup"><span data-stu-id="8e236-243">Thus, the call `F()` in `MoreDerived.G` is valid and will invoke `Base.F`.</span></span> <span data-ttu-id="8e236-244">Im Fall von überladenen Typmember wird der gesamte Satz von überladenen Typmember behandelt, als ob sie den am wenigsten restriktiven Zugriff für die Zwecke des shadowings mussten.</span><span class="sxs-lookup"><span data-stu-id="8e236-244">In the case of overloaded type members, the entire set of overloaded type members is treated as if they all had the most permissive access for the purposes of shadowing.</span></span>

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

<span data-ttu-id="8e236-245">In diesem Beispiel, obwohl die Deklaration von `F()` in `Derived` mit deklariert `Private` für den Zugriff auf den überladenen `F(Integer)` mit deklariert `Public` Zugriff.</span><span class="sxs-lookup"><span data-stu-id="8e236-245">In this example, even though the declaration of `F()` in `Derived` is declared with `Private` access, the overloaded `F(Integer)` is declared with `Public` access.</span></span> <span data-ttu-id="8e236-246">Aus diesem Grund für die shadowing, des Namens `F` in `Derived` wird behandelt, als wäre es `Public`, also beide Methoden Schatten `F` in `Base`.</span><span class="sxs-lookup"><span data-stu-id="8e236-246">Therefore, for the purpose of shadowing, the name `F` in `Derived` is treated as if it was `Public`, so both methods shadow `F` in `Base`.</span></span>

## <a name="implementation"></a><span data-ttu-id="8e236-247">Implementierung</span><span class="sxs-lookup"><span data-stu-id="8e236-247">Implementation</span></span>

<span data-ttu-id="8e236-248">Ein *Implementierung* Beziehung vorhanden ist, wenn ein Typ deklariert, dass sie eine Schnittstelle implementiert, und der Typ alle Typmember der Schnittstelle implementiert.</span><span class="sxs-lookup"><span data-stu-id="8e236-248">An *implementation* relationship exists when a type declares that it implements an interface and the type implements all the type members of the interface.</span></span> <span data-ttu-id="8e236-249">Ein Typ, der eine bestimmte Schnittstelle implementiert, die diese Schnittstelle konvertierbar ist.</span><span class="sxs-lookup"><span data-stu-id="8e236-249">A type that implements a particular interface is convertible to that interface.</span></span> <span data-ttu-id="8e236-250">Schnittstellen können nicht instanziiert werden, aber es ist zulässig, deklarieren von Variablen von Schnittstellen; Diese Variablen können nur einen Wert zugewiesen werden, die einer Klasse, die die Schnittstelle implementiert.</span><span class="sxs-lookup"><span data-stu-id="8e236-250">Interfaces cannot be instantiated, but it is valid to declare variables of interfaces; such variables can only be assigned a value that is of a class that implements the interface.</span></span> <span data-ttu-id="8e236-251">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-251">For example:</span></span>

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

<span data-ttu-id="8e236-252">Ein Implementieren einer Schnittstelle mit Multiplizieren Sie geerbte Typmember muss diese Methoden, implementieren, obwohl diese direkt aus der abgeleiteten Schnittstelle implementiert wird nicht zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="8e236-252">A type implementing an interface with multiply-inherited type members must still implement those methods, even though they cannot be accessed directly from the derived interface being implemented.</span></span> <span data-ttu-id="8e236-253">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-253">For example:</span></span>

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

<span data-ttu-id="8e236-254">Sogar `MustInherit` Klassen müssen alle Member der implementierten Schnittstellen Implementierungen bereitstellen; sie können jedoch Implementierung dieser Methoden verzögern, indem Sie sie als deklarieren `MustOverride`.</span><span class="sxs-lookup"><span data-stu-id="8e236-254">Even `MustInherit` classes must provide implementations of all the members of implemented interfaces; however, they can defer implementation of these methods by declaring them as `MustOverride`.</span></span> <span data-ttu-id="8e236-255">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-255">For example:</span></span>

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

<span data-ttu-id="8e236-256">Ein Typ können eine Schnittstelle neu zu implementieren, die der Basistyp implementiert.</span><span class="sxs-lookup"><span data-stu-id="8e236-256">A type may choose to re-implement an interface that its base type implements.</span></span> <span data-ttu-id="8e236-257">Um die Schnittstelle neu zu implementieren, muss den Typ explizit anzugeben, dass sie die Schnittstelle implementiert.</span><span class="sxs-lookup"><span data-stu-id="8e236-257">To re-implement the interface, the type must explicitly state that it implements the interface.</span></span> <span data-ttu-id="8e236-258">Ein Typ, der erneut implementieren einer Schnittstelle können nur einige erneut zu implementieren, jedoch nicht alle, die Member der Schnittstelle – alle Mitglieder, die nicht erneut implementiert weiterhin des Basistyps Implementierung verwenden.</span><span class="sxs-lookup"><span data-stu-id="8e236-258">A type re-implementing an interface may choose to re-implement only some, but not all, of the members of the interface -- any members not re-implemented continue to use the base type's implementation.</span></span> <span data-ttu-id="8e236-259">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-259">For example:</span></span>

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

<span data-ttu-id="8e236-260">In diesem Beispiel gibt:</span><span class="sxs-lookup"><span data-stu-id="8e236-260">This example prints:</span></span>

```
TestDerived.DerivedTest1
TestBase.Test2
```

<span data-ttu-id="8e236-261">Wenn ein abgeleiteter Typ eine Schnittstelle implementiert, deren Basisschnittstellen von Basistypen von den abgeleiteten Typ implementiert werden, kann auch, dass der abgeleitete Typ nur die Typ-Member der Schnittstelle implementieren, die nicht bereits von den grundlegenden Typen implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="8e236-261">When a derived type implements an interface whose base interfaces are implemented by the derived type's base types, the derived type can choose to only implement the interface's type members that are not already implemented by the base types.</span></span> <span data-ttu-id="8e236-262">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-262">For example:</span></span>

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

<span data-ttu-id="8e236-263">Eine Schnittstellenmethode kann auch über eine überschreibbare Methode in einem Basistyp implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="8e236-263">An interface method can also be implemented using an overridable method in a base type.</span></span> <span data-ttu-id="8e236-264">Ein abgeleiteter Typ kann in diesem Fall auch die überschreibbare Methode überschreiben und ändern die Implementierung der Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="8e236-264">In that case, a derived type may also override the overridable method and alter the implementation of the interface.</span></span> <span data-ttu-id="8e236-265">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-265">For example:</span></span>

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

### <a name="implementing-methods"></a><span data-ttu-id="8e236-266">Implementieren von Methoden</span><span class="sxs-lookup"><span data-stu-id="8e236-266">Implementing Methods</span></span>

<span data-ttu-id="8e236-267">Ein Typ *implementiert* einen Typmember von einer implementierten Schnittstelle durch Angabe einer Methode mit einem `Implements` Klausel.</span><span class="sxs-lookup"><span data-stu-id="8e236-267">A type *implements* a type member of an implemented interface by supplying a method with an `Implements` clause.</span></span> <span data-ttu-id="8e236-268">Zwei-Typmember müssen die gleiche Anzahl von Parametern, aller Typen und Modifizierer der Parameter übereinstimmen, einschließlich der Standardwerte der optionalen Parameter, der Rückgabetyp muss übereinstimmen und alle Einschränkungen für Methodenparameter müssen übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="8e236-268">The two type members must have the same number of parameters, all of the types and modifiers of the parameters must match, including the default value of optional parameters, the return type must match, and all of the constraints on method parameters must match.</span></span> <span data-ttu-id="8e236-269">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-269">For example:</span></span>

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

<span data-ttu-id="8e236-270">Eine einzelne Methode kann eine beliebige Anzahl von Schnittstellentypmembern implementieren, wenn alle der oben genannten Kriterien erfüllen.</span><span class="sxs-lookup"><span data-stu-id="8e236-270">A single method may implement any number of interface type members if they all meet the above criteria.</span></span> <span data-ttu-id="8e236-271">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-271">For example:</span></span>

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

<span data-ttu-id="8e236-272">Wenn Sie eine Methode in einer generischen Schnittstelle zu implementieren, muss die implementierende Methode Typargumente angeben, die Typparameter der Schnittstelle entsprechen.</span><span class="sxs-lookup"><span data-stu-id="8e236-272">When implementing a method in a generic interface, the implementing method must supply the type arguments that correspond to the interface's type parameters.</span></span> <span data-ttu-id="8e236-273">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-273">For example:</span></span>

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

<span data-ttu-id="8e236-274">Beachten Sie, dass es möglich, dass eine generische Schnittstelle möglicherweise nicht für einen Satz von Typargumenten implementierbar.</span><span class="sxs-lookup"><span data-stu-id="8e236-274">Note that it is possible that a generic interface may not be implementable for some set of type arguments.</span></span>

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

## <a name="polymorphism"></a><span data-ttu-id="8e236-275">Polymorphismus</span><span class="sxs-lookup"><span data-stu-id="8e236-275">Polymorphism</span></span>

<span data-ttu-id="8e236-276">*Polymorphismus* bietet die Möglichkeit, die die Implementierung einer Methode oder Eigenschaft variieren.</span><span class="sxs-lookup"><span data-stu-id="8e236-276">*Polymorphism* provides the ability to vary the implementation of a method or property.</span></span> <span data-ttu-id="8e236-277">Die gleiche Methode oder Eigenschaft kann mit Polymorphie unterschiedliche Aktionen abhängig von der Runtime-Typ der Instanz ausführen, den sie aufruft.</span><span class="sxs-lookup"><span data-stu-id="8e236-277">With polymorphism, the same method or property can perform different actions depending on the run-time type of the instance that invokes it.</span></span> <span data-ttu-id="8e236-278">Methoden oder Eigenschaften, die polymorph sind heißen *überschreibbare*.</span><span class="sxs-lookup"><span data-stu-id="8e236-278">Methods or properties that are polymorphic are called *overridable*.</span></span> <span data-ttu-id="8e236-279">Im Gegensatz dazu ist die Implementierung einer nicht überschreibbare Methode oder Eigenschaft unveränderlich; die Implementierung ist identisch, ob die Methode oder Eigenschaft, auf einer Instanz der Klasse in aufgerufen wird der sie deklariert ist, oder eine Instanz einer abgeleiteten Klasse.</span><span class="sxs-lookup"><span data-stu-id="8e236-279">By contrast, the implementation of a non-overridable method or property is invariant; the implementation is the same whether the method or property is invoked on an instance of the class in which it is declared or an instance of a derived class.</span></span> <span data-ttu-id="8e236-280">Wenn eine nicht überschreibbare Methode oder Eigenschaft aufgerufen wird, ist die Kompilierzeit-Typ der Instanz der bestimmende Faktor.</span><span class="sxs-lookup"><span data-stu-id="8e236-280">When a non-overridable method or property is invoked, the compile-time type of the instance is the determining factor.</span></span> <span data-ttu-id="8e236-281">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-281">For example:</span></span>

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

<span data-ttu-id="8e236-282">Eine überschreibbare Methode möglicherweise auch `MustOverride`, was bedeutet, dass es kein Methodentext enthält und überschrieben werden muss.</span><span class="sxs-lookup"><span data-stu-id="8e236-282">An overridable method may also be `MustOverride`, which means that it provides no method body and must be overridden.</span></span> <span data-ttu-id="8e236-283">`MustOverride` Methoden dürfen nur `MustInherit` Klassen.</span><span class="sxs-lookup"><span data-stu-id="8e236-283">`MustOverride` methods are only allowed in `MustInherit` classes.</span></span>

<span data-ttu-id="8e236-284">Im folgenden Beispiel die Klasse `Shape` definiert die abstrakte Vorstellung einer geometrischen Formobjekt, das sich selbst zu zeichnen kann:</span><span class="sxs-lookup"><span data-stu-id="8e236-284">In the following example, the class `Shape` defines the abstract notion of a geometrical shape object that can paint itself:</span></span>

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

<span data-ttu-id="8e236-285">Die `Paint` Methode `MustOverride` , da es keine sinnvolle Standardimplementierung ist.</span><span class="sxs-lookup"><span data-stu-id="8e236-285">The `Paint` method is `MustOverride` because there is no meaningful default implementation.</span></span> <span data-ttu-id="8e236-286">Die `Ellipse` und `Box` Klassen sind konkrete `Shape` Implementierungen.</span><span class="sxs-lookup"><span data-stu-id="8e236-286">The `Ellipse` and `Box` classes are concrete `Shape` implementations.</span></span> <span data-ttu-id="8e236-287">Da diese Klassen, nicht sind `MustInherit`, müssen außer Kraft setzen der `Paint` Methode und eine tatsächliche Implementierung bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="8e236-287">Because these classes are not `MustInherit`, they are required to override the `Paint` method and provide an actual implementation.</span></span>

<span data-ttu-id="8e236-288">Es ist ein Fehler für Basiszugriff auf eine `MustOverride` -Methode wie im folgenden Beispiel veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="8e236-288">It is an error for a base access to reference a `MustOverride` method, as the following example demonstrates:</span></span>

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

<span data-ttu-id="8e236-289">Ein Fehler wird gemeldet, für die `MyBase.F()` Aufruf, da er verweist auf eine `MustOverride` Methode.</span><span class="sxs-lookup"><span data-stu-id="8e236-289">An error is reported for the `MyBase.F()` invocation because it references a `MustOverride` method.</span></span>

### <a name="overriding-methods"></a><span data-ttu-id="8e236-290">Überschreiben von Methoden</span><span class="sxs-lookup"><span data-stu-id="8e236-290">Overriding Methods</span></span>

<span data-ttu-id="8e236-291">Ein Typ kann *überschreiben* eine geerbte überschreibbare Methode durch Deklarieren einer Methode mit dem gleichen Namen und Signatur aus, und die Deklaration mit der `Overrides` Modifizierer. Es gibt zusätzliche Anforderungen für die unten aufgeführten Methoden überschreiben.</span><span class="sxs-lookup"><span data-stu-id="8e236-291">A type may *override* an inherited overridable method by declaring a method with the same name and , signature, and marking the declaration with the `Overrides` modifier.There are additional requirements on overriding methods, listed below.</span></span> <span data-ttu-id="8e236-292">Während ein `Overridable` Deklaration der Methode führt eine neue Methode, eine `Overrides` Methodendeklaration ersetzt die geerbte Implementierung der Methode.</span><span class="sxs-lookup"><span data-stu-id="8e236-292">Whereas an `Overridable` method declaration introduces a new method, an `Overrides` method declaration replaces the inherited implementation of the method.</span></span>

<span data-ttu-id="8e236-293">Kann die überschreibende Methode deklariert werden `NotOverridable`, die verhindert, dass Sie ein weiteres Überschreiben der Methode in abgeleiteten Typen.</span><span class="sxs-lookup"><span data-stu-id="8e236-293">An overriding method may be declared `NotOverridable`, which prevents any further overriding of the method in derived types.</span></span> <span data-ttu-id="8e236-294">Tatsächlich `NotOverridable` Methoden werden keine weiteren nicht überschreibbaren in abgeleiteten Klassen.</span><span class="sxs-lookup"><span data-stu-id="8e236-294">In effect, `NotOverridable` methods become non-overridable in any further derived classes.</span></span>

<span data-ttu-id="8e236-295">Betrachten Sie das folgende Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-295">Consider the following example:</span></span>

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

<span data-ttu-id="8e236-296">In diesem Beispiel-Klasse `B` bietet zwei `Overrides` Methoden: eine Methode `F` , bei dem die `NotOverridable` -Modifizierer und eine Methode `G` , die jedoch nicht.</span><span class="sxs-lookup"><span data-stu-id="8e236-296">In the example, class `B` provides two `Overrides` methods: a method `F` that has the `NotOverridable` modifier and a method `G` that does not.</span></span> <span data-ttu-id="8e236-297">Verwenden der `NotOverridable` Modifizierer wird verhindert, dass Klasse `C` von weiteren Überschreiben der Methode `F`.</span><span class="sxs-lookup"><span data-stu-id="8e236-297">Use of the `NotOverridable` modifier prevents class `C` from further overriding method `F`.</span></span>

<span data-ttu-id="8e236-298">Kann auch die überschreibende Methode deklariert werden `MustOverride`, auch wenn die Methode, die sie außer Kraft gesetzt wird, nicht deklariert ist `MustOverride`.</span><span class="sxs-lookup"><span data-stu-id="8e236-298">An overriding method may also be declared `MustOverride`, even if the method that it is overriding is not declared `MustOverride`.</span></span> <span data-ttu-id="8e236-299">Dies erfordert, dass die enthaltende Klasse deklariert werden `MustInherit` und eine weitere Klassen abgeleitet, die nicht deklariert werden `MustInherit` müssen die Methode überschreiben.</span><span class="sxs-lookup"><span data-stu-id="8e236-299">This requires that the containing class be declared `MustInherit` and that any further derived classes that are not declared `MustInherit` must override the method.</span></span> <span data-ttu-id="8e236-300">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-300">For example:</span></span>

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

<span data-ttu-id="8e236-301">In diesem Beispiel-Klasse `B` überschreibt `A.F` mit einem `MustOverride` Methode.</span><span class="sxs-lookup"><span data-stu-id="8e236-301">In the example, class `B` overrides `A.F` with a `MustOverride` method.</span></span> <span data-ttu-id="8e236-302">Dies bedeutet, dass alle Klassen abgeleitet `B` außer Kraft setzen müssen `F`, es sei denn, sie deklariert werden `MustInherit` ebenfalls.</span><span class="sxs-lookup"><span data-stu-id="8e236-302">This means that any classes derived from `B` will have to override `F`, unless they are declared `MustInherit` as well.</span></span>

<span data-ttu-id="8e236-303">Ein Fehler während der Kompilierung tritt auf, es sei denn, alle der folgenden von der überschreibenden Methode erfüllt sind:</span><span class="sxs-lookup"><span data-stu-id="8e236-303">A compile-time error occurs unless all of the following are true of an overriding method:</span></span>

* <span data-ttu-id="8e236-304">Der Deklarationskontext enthält eine einzelne zugegriffen werden kann, geerbte Methode mit der gleichen Signatur und der Rückgabetyp (sofern vorhanden) als die überschreibende Methode.</span><span class="sxs-lookup"><span data-stu-id="8e236-304">The declaration context contains a single accessible inherited method with the same signature and return type (if any) as the overriding method.</span></span>
* <span data-ttu-id="8e236-305">Die geerbte Methode überschrieben wird, ist überschreibbar.</span><span class="sxs-lookup"><span data-stu-id="8e236-305">The inherited method being overridden is overridable.</span></span> <span data-ttu-id="8e236-306">Das heißt, ist die geerbte Methode überschrieben wird nicht `Shared` oder `NotOverridable`.</span><span class="sxs-lookup"><span data-stu-id="8e236-306">In other words, the inherited method being overridden is not `Shared` or `NotOverridable`.</span></span>
* <span data-ttu-id="8e236-307">Die Zugriffsdomäne des deklarierten Methode entspricht die Zugriffsdomäne von der geerbten Methode überschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="8e236-307">The accessibility domain of the method being declared is the same as the accessibility domain of the inherited method being overridden.</span></span> <span data-ttu-id="8e236-308">Es gibt eine Ausnahme: ein `Protected Friend` Methode muss überschrieben werden, indem eine `Protected` Methode, wenn die andere Methode in einer anderen Assembly ist, die die überschreibende Methode nicht `Friend` Zugriff auf.</span><span class="sxs-lookup"><span data-stu-id="8e236-308">There is one exception: a `Protected Friend` method must be overridden by a `Protected` method if the other method is in another assembly that the overriding method does not have `Friend` access to.</span></span>
* <span data-ttu-id="8e236-309">Die Parameter der die überschreibende Methode übereinstimmen, außer Kraft gesetzte die Parameter der Methode im Hinblick auf die Verwendung von der `ByVal`, `ByRef`, `ParamArray,` und `Optional` Modifizierern, einschließlich der Werte für optionale Parameter bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="8e236-309">The parameters of the overriding method match the overridden method's parameters in regards to usage of the `ByVal`, `ByRef`, `ParamArray,` and `Optional` modifiers, including the values provided for optional parameters.</span></span>
* <span data-ttu-id="8e236-310">Die Typparameter der überschreibenden Methode überein, die überschriebene Methode Typparameter im Hinblick auf Einschränkungen geben.</span><span class="sxs-lookup"><span data-stu-id="8e236-310">The type parameters of the overriding method match the overridden method's type parameters in regards to type constraints.</span></span>

<span data-ttu-id="8e236-311">Wenn Sie eine Methode in einem generischen Basistyp zu überschreiben, muss die überschreibende Methode die Typargumente angeben, die den Basistyp Parametern entsprechen.</span><span class="sxs-lookup"><span data-stu-id="8e236-311">When overriding a method in a base generic type, the overriding method must supply the type arguments that correspond to the base type parameters.</span></span> <span data-ttu-id="8e236-312">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-312">For example:</span></span>

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

<span data-ttu-id="8e236-313">Beachten Sie, dass es möglich, dass eine überschreibbare Methode in einer generischen Klasse möglicherweise nicht für einige Sätze von Typargumenten überschrieben werden können.</span><span class="sxs-lookup"><span data-stu-id="8e236-313">Note that it is possible that an overridable method in a generic class may not be able to be overridden for some sets of type arguments.</span></span> <span data-ttu-id="8e236-314">Wenn die Methode deklariert wird `MustOverride`, dies bedeutet, dass einige Vererbungskette nicht möglich ggf.</span><span class="sxs-lookup"><span data-stu-id="8e236-314">If the method is declared `MustOverride`, this means that some inheritance chains may not be possible.</span></span> <span data-ttu-id="8e236-315">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-315">For example:</span></span>

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

<span data-ttu-id="8e236-316">Eine Deklaration außer Kraft setzen, kann die überschriebene Basismethode Basiszugriff, wie im folgenden Beispiel mit zugreifen:</span><span class="sxs-lookup"><span data-stu-id="8e236-316">An override declaration can access the overridden base method using a base access, as in the following example:</span></span>

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

Im Beispiel des Aufrufs `MyBase.PrintVariables()` in Klasse `Derived` Ruft die `PrintVariables` Methode deklariert werden, in der Klasse `Base`. Basiszugriff deaktiviert die überschreibbare Aufrufmechanismus und behandelt einfach die Basismethode als nicht überschreibbare Methode. <span data-ttu-id="8e236-319">Hatte den Aufruf im `Derived` wurde geschrieben `CType(Me, Base).PrintVariables()`, wäre Sie rekursiv Aufrufen der `PrintVariables` Methode deklariert werden, `Derived`, nicht zu derjenigen, die in deklariert `Base`.</span><span class="sxs-lookup"><span data-stu-id="8e236-319">Had the invocation in `Derived` been written `CType(Me, Base).PrintVariables()`, it would recursively invoke the `PrintVariables` method declared in `Derived`, not the one declared in `Base`.</span></span>

<span data-ttu-id="8e236-320">Wenn es enthält nur eine `Overrides` Modifizierer kann eine Methode eine andere Methode zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="8e236-320">Only when it includes an `Overrides` modifier can a method override another method.</span></span> <span data-ttu-id="8e236-321">In allen anderen Fällen führt Shadowing für eine Methode mit der gleichen Signatur wie eine geerbte Methode einfach die geerbte Methode, wie im folgenden Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-321">In all other cases, a method with the same signature as an inherited method simply shadows the inherited method, as in the example below:</span></span>

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

<span data-ttu-id="8e236-322">Im Beispiel die Methode `F` in Klasse `Derived` enthält kein `Overrides` Modifizierer und aus diesem Grund wird die Methode nicht überschrieben `F` in Klasse `Base`.</span><span class="sxs-lookup"><span data-stu-id="8e236-322">In the example, the method `F` in class `Derived` does not include an `Overrides` modifier and therefore does not override method `F` in class `Base`.</span></span> <span data-ttu-id="8e236-323">Stattdessen Methode `F` in Klasse `Derived` führt Shadowing für die Methode in Klasse `Base`, und eine Warnung wird ausgegeben, da die Deklaration keine umfasst eine `Shadows` oder `Overloads` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="8e236-323">Rather, method `F` in class `Derived` shadows the method in class `Base`, and a warning is reported because the declaration does not include a `Shadows` or `Overloads` modifier.</span></span>

<span data-ttu-id="8e236-324">Im folgenden Beispiel Methode `F` in Klasse `Derived` führt Shadowing für die überschreibbare Methode `F` Klasse vererbt `Base`:</span><span class="sxs-lookup"><span data-stu-id="8e236-324">In the following example, method `F` in class `Derived` shadows the overridable method `F` inherited from class `Base`:</span></span>

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

<span data-ttu-id="8e236-325">Da die neue Methode `F` in Klasse `Derived` hat `Private` Zugriff, umfasst der Bereich nur des klassentexts von `Derived` und erstreckt sich nicht auf Klasse `MoreDerived`.</span><span class="sxs-lookup"><span data-stu-id="8e236-325">Since the new method `F` in class `Derived` has `Private` access, its scope only includes the class body of `Derived` and does not extend to class `MoreDerived`.</span></span> <span data-ttu-id="8e236-326">Die Deklaration der Methode `F` in Klasse `MoreDerived` aus diesem Grund darf die Methode außer Kraft setzen `F` Klasse vererbt `Base`.</span><span class="sxs-lookup"><span data-stu-id="8e236-326">The declaration of method `F` in class `MoreDerived` is therefore permitted to override the method `F` inherited from class `Base`.</span></span>

<span data-ttu-id="8e236-327">Wenn ein `Overridable` -Methode wird aufgerufen, die am weitesten abgeleitete Implementierung der Instanzenmethode aufgerufen wird, basierend auf dem Typ der Instanz an, unabhängig davon, ob der Aufruf der Methode in der Basisklasse oder die abgeleitete Klasse.</span><span class="sxs-lookup"><span data-stu-id="8e236-327">When an `Overridable` method is invoked, the most derived implementation of the instance method is called, based on the type of the instance, regardless of whether the call is to the method in the base class or the derived class.</span></span> <span data-ttu-id="8e236-328">Die am weitesten abgeleiteten Implementierung von einem `Overridable` Methode `M` in Bezug auf eine Klasse `R` wird wie folgt bestimmt:</span><span class="sxs-lookup"><span data-stu-id="8e236-328">The most derived implementation of an `Overridable` method `M` with respect to a class `R` is determined as follows:</span></span>

* <span data-ttu-id="8e236-329">Wenn `R` enthält, die Einführung von `Overridable` Deklaration `M`, dies ist die am stärksten abgeleiteten Implementierung der `M`.</span><span class="sxs-lookup"><span data-stu-id="8e236-329">If `R` contains the introducing `Overridable` declaration of `M`, this is the most derived implementation of `M`.</span></span>

* <span data-ttu-id="8e236-330">Andernfalls gilt: Wenn `R` enthält eine Überschreibung der `M`, dies ist die am stärksten abgeleiteten Implementierung der `M`.</span><span class="sxs-lookup"><span data-stu-id="8e236-330">Otherwise, if `R` contains an override of `M`, this is the most derived implementation of `M`.</span></span>

* <span data-ttu-id="8e236-331">Andernfalls am meisten Implementierung der abgeleiteten der `M` ist identisch mit der die direkte Basisklasse von `R`.</span><span class="sxs-lookup"><span data-stu-id="8e236-331">Otherwise, the most derived implementation of `M` is the same as that of the direct base class of `R`.</span></span>

## <a name="accessibility"></a><span data-ttu-id="8e236-332">Zugriff</span><span class="sxs-lookup"><span data-stu-id="8e236-332">Accessibility</span></span>

<span data-ttu-id="8e236-333">Eine Deklaration gibt an, die *Barrierefreiheit* der Entität deklariert.</span><span class="sxs-lookup"><span data-stu-id="8e236-333">A declaration specifies the *accessibility* of the entity it declares.</span></span> <span data-ttu-id="8e236-334">Zugriff auf eine Entität ändert sich nicht auf den Rahmen eines Entitätsnamens aus.</span><span class="sxs-lookup"><span data-stu-id="8e236-334">An entity's accessibility does not change the scope of an entity's name.</span></span> <span data-ttu-id="8e236-335">Die *Zugriffsdomäne* einer Deklaration ist ein Satz von allen Deklaration Leerzeichen in der die deklarierte Entität zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="8e236-335">The *accessibility domain* of a declaration is the set of all declaration spaces in which the declared entity is accessible.</span></span>

<span data-ttu-id="8e236-336">Die fünf Zugriffstypen werden `Public`, `Protected`, `Friend`, `Protected Friend`, und `Private`.</span><span class="sxs-lookup"><span data-stu-id="8e236-336">The five access types are `Public`, `Protected`, `Friend`, `Protected Friend`, and `Private`.</span></span> <span data-ttu-id="8e236-337">`Public` ist die höchste Zugriffstyp und die vier anderen Typen sind alle Teilmengen von `Public`.</span><span class="sxs-lookup"><span data-stu-id="8e236-337">`Public` is the most permissive access type, and the four other types are all subsets of `Public`.</span></span> <span data-ttu-id="8e236-338">Der am wenigsten eingeschränkte Zugriffstyp ist `Private`, und die vier anderen Zugriffstypen werden alle eine Obermenge der `Private`.</span><span class="sxs-lookup"><span data-stu-id="8e236-338">The least permissive access type is `Private`, and the four other access types are all supersets of `Private`.</span></span>

```antlr
AccessModifier
    : 'Public'
    | 'Protected'
    | 'Friend'
    | 'Private'
    | 'Protected' 'Friend'
    ;
```

<span data-ttu-id="8e236-339">Die für eine Deklaration angegeben wird über einen optionalen Zugriffsmodifizierer, die möglicherweise `Public`, `Protected`, `Friend`, `Private`, oder der Kombination von `Protected` und `Friend`.</span><span class="sxs-lookup"><span data-stu-id="8e236-339">The access type for a declaration is specified via an optional access modifier, which can be `Public`, `Protected`, `Friend`, `Private`, or the combination of `Protected` and `Friend`.</span></span> <span data-ttu-id="8e236-340">Wenn kein Zugriffsmodifizierer angegeben wird, hängt von der Standardtyp für den Zugriff der Deklarationskontext; die zulässigen Zugriffstypen hängt auch von der Deklarationskontext ab.</span><span class="sxs-lookup"><span data-stu-id="8e236-340">If no access modifier is specified, the default access type depends on the declaration context; the permitted access types also depend on the declaration context.</span></span>

* <span data-ttu-id="8e236-341">Entitäten deklariert, mit der `Public` -Modifizierer aufweisen. `Public` Zugriff.</span><span class="sxs-lookup"><span data-stu-id="8e236-341">Entities declared with the `Public` modifier have `Public` access.</span></span> <span data-ttu-id="8e236-342">Es gibt keine Einschränkungen bei der Verwendung von `Public` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="8e236-342">There are no restrictions on the use of `Public` entities.</span></span>

* <span data-ttu-id="8e236-343">Entitäten deklariert, mit der `Protected` -Modifizierer aufweisen. `Protected` Zugriff.</span><span class="sxs-lookup"><span data-stu-id="8e236-343">Entities declared with the `Protected` modifier have `Protected` access.</span></span> <span data-ttu-id="8e236-344">`Protected` Zugriff kann nur angegeben werden, bei Mitgliedern von Klassen (regulärer-Typmember und geschachtelte Klassen) oder auf `Overridable` Member von Standardmodulen und Strukturen (die muss, können Sie per Definition vererbt werden `System.Object` oder `System.ValueType`).</span><span class="sxs-lookup"><span data-stu-id="8e236-344">`Protected` access can only be specified on members of classes (both regular type members and nested classes) or on `Overridable` members of standard modules and structures (which must, by definition, be inherited from `System.Object` or `System.ValueType`).</span></span> <span data-ttu-id="8e236-345">Ein `Protected` Member ist auf eine abgeleitete Klasse zugegriffen werden kann, vorausgesetzt, dass der Member kein Instanzmember ist oder der Zugriff über eine Instanz der abgeleiteten Klasse erfolgt.</span><span class="sxs-lookup"><span data-stu-id="8e236-345">A `Protected` member is accessible to a derived class, provided that either the member is not an instance member, or the access takes place through an instance of the derived class.</span></span> <span data-ttu-id="8e236-346">`Protected` der Zugriff ist keine Obermenge der `Friend` Zugriff.</span><span class="sxs-lookup"><span data-stu-id="8e236-346">`Protected` access is not a superset of `Friend` access.</span></span>

* <span data-ttu-id="8e236-347">Entitäten deklariert, mit der `Friend` -Modifizierer aufweisen. `Friend` Zugriff.</span><span class="sxs-lookup"><span data-stu-id="8e236-347">Entities declared with the `Friend` modifier have `Friend` access.</span></span> <span data-ttu-id="8e236-348">Eine Entität mit `Friend` Zugriff ist nur innerhalb des Programms, das die Entitätsdeklaration oder alle Assemblys, die erteilt wurden enthält, zugänglich `Friend` über Zugriff auf die `System.Runtime.CompilerServices.InternalsVisibleToAttribute` Attribut.</span><span class="sxs-lookup"><span data-stu-id="8e236-348">An entity with `Friend` access is accessible only within the program that contains the entity declaration or any assemblies that have been given `Friend` access through the `System.Runtime.CompilerServices.InternalsVisibleToAttribute` attribute.</span></span>

* <span data-ttu-id="8e236-349">Entitäten deklariert, mit der `Protected Friend` Modifizierer verfügt, die Union der `Protected` und `Friend` Zugriff.</span><span class="sxs-lookup"><span data-stu-id="8e236-349">Entities declared with the `Protected Friend` modifiers have the union of `Protected` and `Friend` access.</span></span>

* <span data-ttu-id="8e236-350">Entitäten deklariert, mit der `Private` -Modifizierer aufweisen. `Private` Zugriff.</span><span class="sxs-lookup"><span data-stu-id="8e236-350">Entities declared with the `Private` modifier have `Private` access.</span></span> <span data-ttu-id="8e236-351">Ein `Private` Entität kann nur innerhalb der Deklarationskontext, einschließlich der Entitäten, geschachtelte zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="8e236-351">A `Private` entity is accessible only within its declaration context, including any nested entities.</span></span>

<span data-ttu-id="8e236-352">Der Zugriff auf in einer Deklaration ist nicht auf die Barrierefreiheit der Deklarationskontext abhängig.</span><span class="sxs-lookup"><span data-stu-id="8e236-352">The accessibility in a declaration does not depend on the accessibility of the declaration context.</span></span> <span data-ttu-id="8e236-353">Z. B. ein Typ deklariert, mit `Private` Zugriff eventuell einen Typmember mit `Public` Zugriff.</span><span class="sxs-lookup"><span data-stu-id="8e236-353">For example, a type declared with `Private` access may contain a type member with `Public` access.</span></span>

<span data-ttu-id="8e236-354">Der folgende Code veranschaulicht verschiedene Eingabehilfen-Domänen:</span><span class="sxs-lookup"><span data-stu-id="8e236-354">The following code demonstrates various accessibility domains:</span></span>

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

<span data-ttu-id="8e236-355">Die Klassen und Member in diesem Beispiel haben die folgenden Eingabehilfen-Domänen:</span><span class="sxs-lookup"><span data-stu-id="8e236-355">The classes and members in this example have the following accessibility domains:</span></span>

* <span data-ttu-id="8e236-356">Die Zugriffsdomäne von `A` und `A.X` ist unbegrenzt.</span><span class="sxs-lookup"><span data-stu-id="8e236-356">The accessibility domain of `A` and `A.X` is unlimited.</span></span>

* <span data-ttu-id="8e236-357">Die Zugriffsdomäne von `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, und `B.C.Y` ist das Programm enthält.</span><span class="sxs-lookup"><span data-stu-id="8e236-357">The accessibility domain of `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, and `B.C.Y` is the containing program.</span></span>

* <span data-ttu-id="8e236-358">Die Zugriffsdomäne von `A.Z` ist `A.`</span><span class="sxs-lookup"><span data-stu-id="8e236-358">The accessibility domain of `A.Z` is `A.`</span></span>

* <span data-ttu-id="8e236-359">Die Zugriffsdomäne von `B.Z`, `B.D`, `B.D.X`, und `B.D.Y` ist `B`, einschließlich `B.C` und `B.D`.</span><span class="sxs-lookup"><span data-stu-id="8e236-359">The accessibility domain of `B.Z`, `B.D`, `B.D.X`, and `B.D.Y` is `B`, including `B.C` and `B.D`.</span></span>

* <span data-ttu-id="8e236-360">Die Zugriffsdomäne von `B.C.Z` ist `B.C`.</span><span class="sxs-lookup"><span data-stu-id="8e236-360">The accessibility domain of `B.C.Z` is `B.C`.</span></span>

* <span data-ttu-id="8e236-361">Die Zugriffsdomäne von `B.D.Z` ist `B.D`.</span><span class="sxs-lookup"><span data-stu-id="8e236-361">The accessibility domain of `B.D.Z` is `B.D`.</span></span>

<span data-ttu-id="8e236-362">Wie im Beispiel wird veranschaulicht, ist die Zugriffsdomäne eines Members nie größer als die des enthaltenden Typs.</span><span class="sxs-lookup"><span data-stu-id="8e236-362">As the example illustrates, the accessibility domain of a member is never larger than that of a containing type.</span></span> <span data-ttu-id="8e236-363">Beispielsweise, obwohl alle `X` Mitglieder `Public` deklariert der Barrierefreiheit, gut `A.X` haben Sie auf Zugriffsdomänen, die von einem enthaltenden Typ eingeschränkt werden.</span><span class="sxs-lookup"><span data-stu-id="8e236-363">For example, even though all `X` members have `Public` declared accessibility, all but `A.X` have accessibility domains that are constrained by a containing type.</span></span>

<span data-ttu-id="8e236-364">Der Zugriff auf `Protected` Member durch eine Instanz des abgeleiteten Typs sein müssen, damit nicht verknüpfte Typen kein Zugriff auf jede andere Instanz hat geschützte Member.</span><span class="sxs-lookup"><span data-stu-id="8e236-364">Access to `Protected` instance members must be through an instance of the derived type so that unrelated types cannot gain access to each other's protected members.</span></span> <span data-ttu-id="8e236-365">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-365">For example:</span></span>

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

<span data-ttu-id="8e236-366">Im obigen Beispiel die Klasse `Guest` hat lediglich Zugriff auf die geschützte `Password` Feld, wenn es mit einer Instanz von qualifiziert wird `Guest`.</span><span class="sxs-lookup"><span data-stu-id="8e236-366">In the above example, the class `Guest` only has access to the protected `Password` field if it is qualified with an instance of `Guest`.</span></span> <span data-ttu-id="8e236-367">Dies verhindert, dass `Guest` vom Zugriff auf die `Password` Feld eine `Employee` Objekt einfach durch die Umwandlung zu `User`.</span><span class="sxs-lookup"><span data-stu-id="8e236-367">This prevents `Guest` from gaining access to the `Password` field of an `Employee` object simply by casting it to `User`.</span></span>

<span data-ttu-id="8e236-368">Zum Zweck der `Protected` Memberzugriff in generischen Typen der Deklarationskontext Typparameter enthält.</span><span class="sxs-lookup"><span data-stu-id="8e236-368">For the purposes of `Protected` member access in generic types, the declaration context includes type parameters.</span></span> <span data-ttu-id="8e236-369">Dies bedeutet, dass ein abgeleiteter Typ mit einem einzelnen Satz von Typargumenten keinen Zugriff auf die `Protected` Member eines abgeleiteten Typs mit einem anderen Satz von Typargumenten.</span><span class="sxs-lookup"><span data-stu-id="8e236-369">This means that a derived type with one set of type arguments does not have access to the `Protected` members of a derived type with a different set of type arguments.</span></span> <span data-ttu-id="8e236-370">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-370">For example:</span></span>

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

<span data-ttu-id="8e236-371">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="8e236-371">__Note.__</span></span> <span data-ttu-id="8e236-372">Der C#-Sprache (und möglicherweise andere Sprachen) ermöglicht den Zugriff auf einen generischen Typ `Protected` Member unabhängig vom Typargumente angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="8e236-372">The C# language (and possibly other languages) allows a generic type to access `Protected` members regardless of what type arguments are supplied.</span></span> <span data-ttu-id="8e236-373">Dies sollte Bedenken gehalten werden, beim Entwerfen generischer Klassen, die enthalten `Protected` Member.</span><span class="sxs-lookup"><span data-stu-id="8e236-373">This should be kept in mind when designing generic classes that contain `Protected` members.</span></span>


### <a name="constituent-types"></a><span data-ttu-id="8e236-374">Enthaltenen Typen</span><span class="sxs-lookup"><span data-stu-id="8e236-374">Constituent Types</span></span>

<span data-ttu-id="8e236-375">Die *enthaltenen Typen* einer Deklaration sind die Typen, die von der Deklaration verwiesen werden.</span><span class="sxs-lookup"><span data-stu-id="8e236-375">The *constituent types* of a declaration are the types that are referenced by the declaration.</span></span> <span data-ttu-id="8e236-376">Der Typ, der eine Konstante, die den Rückgabetyp einer Methode und die Parametertypen eines Konstruktors sind beispielsweise alle enthaltenen Typen.</span><span class="sxs-lookup"><span data-stu-id="8e236-376">For example, the type of a constant, the return type of a method and the parameter types of a constructor are all constituent types.</span></span> <span data-ttu-id="8e236-377">Die Zugriffsdomäne eines einzelnen Typs einer Deklaration muss identisch oder eine Obermenge der Zugriffsdomäne von der Deklaration selbst sein.</span><span class="sxs-lookup"><span data-stu-id="8e236-377">The accessibility domain of a constituent type of a declaration must be the same as or a superset of the accessibility domain of the declaration itself.</span></span> <span data-ttu-id="8e236-378">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-378">For example:</span></span>

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

## <a name="type-and-namespace-names"></a><span data-ttu-id="8e236-379">Namespace-Namen und Typ</span><span class="sxs-lookup"><span data-stu-id="8e236-379">Type and Namespace Names</span></span>

<span data-ttu-id="8e236-380">Viele der Sprachkonstrukte erfordern einen Namespace oder Typ angegeben werden. Diese können über eine qualifizierte Form, der den Namespace oder den Namen des Typs angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="8e236-380">Many language constructs require a namespace or type to be specified; these can be specified by using a qualified form of the namespace or type's name.</span></span> <span data-ttu-id="8e236-381">Ein *qualifizierten Namen* besteht aus einer Reihe von Bezeichner durch Punkte getrennt sind; der Bezeichner auf der rechten Seite eines Zeitraums wird in der Deklaration Speicherplatz auf der linken Seite des Zeitraums vom Bezeichner angegebene aufgelöst.</span><span class="sxs-lookup"><span data-stu-id="8e236-381">A *qualified name* consists of a series of identifiers separated by periods; the identifier on the right side of a period is resolved in the declaration space specified by the identifier on the left side of the period.</span></span>

<span data-ttu-id="8e236-382">Die *voll gekennzeichneten Namen* der einen Namespace oder Typ ist ein qualifizierter Name mit dem Namen aller enthält, Namespaces und Typen.</span><span class="sxs-lookup"><span data-stu-id="8e236-382">The *fully qualified name* of a namespace or type is a qualified name that contains the name of all containing namespaces and types.</span></span> <span data-ttu-id="8e236-383">Das heißt, der vollqualifizierte Name eines Namespace oder Typ ist `N.T`, wobei `T` ist der Name der Entität und `N` ist der vollqualifizierte Name der ihn enthaltenden Entität.</span><span class="sxs-lookup"><span data-stu-id="8e236-383">In other words, the fully qualified name of a namespace or type is `N.T`, where `T` is the name of the entity and `N` is the fully qualified name of its containing entity.</span></span>

<span data-ttu-id="8e236-384">Das folgende Beispiel zeigt mehrere Deklarationen von Namespace und den Typ zusammen mit den zugehörigen voll qualifizierten Namen in den Inline-Kommentaren.</span><span class="sxs-lookup"><span data-stu-id="8e236-384">The example below shows several namespace and type declarations together with their associated fully qualified names in in-line comments.</span></span>

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

<span data-ttu-id="8e236-385">Beachten Sie, dass der Namespace X.Y an zwei verschiedenen Speicherorten im Quellcode deklariert wurde, aber diese beiden partiellen Deklarationen bilden nur einen einzelnen Namespace mit dem Namen X.Y enthält sowohl Klasse D und e-Klasse</span><span class="sxs-lookup"><span data-stu-id="8e236-385">Observe that the namespace X.Y has been declared in two different locations in the source code, but these two partial declarations constitute just a single namespace called X.Y which contains both class D and class E.</span></span>

<span data-ttu-id="8e236-386">In einigen Fällen kann ein qualifizierter Name mit dem Schlüsselwort beginnen `Global`.</span><span class="sxs-lookup"><span data-stu-id="8e236-386">In some situations, a qualified name may begin with the keyword `Global`.</span></span> <span data-ttu-id="8e236-387">Das Schlüsselwort stellt den äußersten unbenannten Namespace, der eignet sich für Situationen, in denen eine Deklaration führt Shadowing für eine einschließende Namespace, dar.</span><span class="sxs-lookup"><span data-stu-id="8e236-387">The keyword represents the unnamed outermost namespace, which is useful in situations where a declaration shadows an enclosing namespace.</span></span> <span data-ttu-id="8e236-388">Die `Global` Schlüsselwort, "Schutz" auf den äußeren Namespace in dieser Situation kann.</span><span class="sxs-lookup"><span data-stu-id="8e236-388">The `Global` keyword allows "escaping" out to the outermost namespace in that situation.</span></span> <span data-ttu-id="8e236-389">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-389">For example:</span></span>

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

<span data-ttu-id="8e236-390">Im obigen Beispiel ist der erste Methodenaufruf ungültig da der Bezeichner `System` auf die Klasse bindet `System`, nicht den Namespace `System`.</span><span class="sxs-lookup"><span data-stu-id="8e236-390">In the above example, the first method call is invalid because the identifier `System` binds to the class `System`, not the namespace `System`.</span></span> <span data-ttu-id="8e236-391">Die einzige Möglichkeit zum Zugriff auf die `System` Namespace ist die Verwendung `Global` , dem äußeren Namespace mit Escapezeichen versehen.</span><span class="sxs-lookup"><span data-stu-id="8e236-391">The only way to access the `System` namespace is to use `Global` to escape out to the outermost namespace.</span></span> <span data-ttu-id="8e236-392">`Global` kann nicht verwendet werden, eine `Imports` Anweisung oder `Namespace` Deklaration.</span><span class="sxs-lookup"><span data-stu-id="8e236-392">`Global` cannot be used in an `Imports` statement or `Namespace` declaration.</span></span>

<span data-ttu-id="8e236-393">Da es sich bei anderen Sprachen einführen können, Typen und Namespaces, die Schlüsselwörter in der Sprache entsprechen, erkennt Visual Basic Schlüsselwörter aufgelistet, die Teil eines qualifizierten Namens werden, solange sie einen Punkt folgen.</span><span class="sxs-lookup"><span data-stu-id="8e236-393">Because other languages may introduce types and namespaces that match keywords in the language, Visual Basic recognizes keywords to be part of a qualified name as long as they follow a period.</span></span> <span data-ttu-id="8e236-394">Schlüsselwörter, die auf diese Weise verwendet werden als Bezeichner behandelt.</span><span class="sxs-lookup"><span data-stu-id="8e236-394">Keywords used in this way are treated as identifiers.</span></span> <span data-ttu-id="8e236-395">Z. B. den vollqualifizierten Bezeichner `X.Default.Class` ist ein gültiger qualifizierter Bezeichner, während `Default.Class` nicht.</span><span class="sxs-lookup"><span data-stu-id="8e236-395">For example, the qualified identifier `X.Default.Class` is a valid qualified identifier, while `Default.Class` is not.</span></span>

### <a name="qualified-name-resolution-for-namespaces-and-types"></a><span data-ttu-id="8e236-396">Qualifizierte namensauflösung für Namespaces und Typen</span><span class="sxs-lookup"><span data-stu-id="8e236-396">Qualified Name Resolution for namespaces and types</span></span>

<span data-ttu-id="8e236-397">Einen qualifizierten Namespace oder Typ-Name des Formulars angegeben `N.R(Of A)`, wobei `R` der äußersten rechten Bezeichner im qualifizierten Namen und `A` ist eine optionale Liste der Typargumente, die folgenden Schritte beschreiben, wie Sie bestimmen, welcher Namespace oder Geben Sie den vollqualifizierten Namen verweist:</span><span class="sxs-lookup"><span data-stu-id="8e236-397">Given a qualified namespace or type name of the form `N.R(Of A)`, where `R` is the rightmost identifier in the qualified name and `A` is an optional type argument list, the following steps describe how to determine to which namespace or type the qualified name refers:</span></span>

1. <span data-ttu-id="8e236-398">Beheben `N`, gemäß den Regeln für entweder qualifizierte oder nicht qualifizierte namensauflösung.</span><span class="sxs-lookup"><span data-stu-id="8e236-398">Resolve `N`, using the rules for either qualified or unqualified name resolution.</span></span>

2. <span data-ttu-id="8e236-399">Wenn Auflösung `N` auf einen Typparameter, löst ein Fehler während der Kompilierung tritt ein, oder ein Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="8e236-399">If resolution of `N` fails, or resolves to a type parameter, a compile-time error occurs.</span></span>

3. <span data-ttu-id="8e236-400">Andernfalls gilt: Wenn `R` entspricht der Name eines Namespace in N und keine Typargumente wurden angegeben, oder `R` entspricht einen zugreifbarer Typ im `N` mit der gleichen Anzahl von Typparametern als Typargumente, sofern vorhanden, klicken Sie dann der qualifizierte Namen bezieht sich auf Dieser Namespace oder Typ.</span><span class="sxs-lookup"><span data-stu-id="8e236-400">Otherwise, if `R` matches the name of a namespace in N and no type arguments were supplied, or `R` matches an accessible type in `N` with the same number of type parameters as type arguments, if any, then the qualified name refers to that namespace or type.</span></span>

4. <span data-ttu-id="8e236-401">Andernfalls gilt: Wenn `N` enthält eine oder mehrere standard-Module, und `R` der Name des ein zugreifbarer Typ mit der gleichen Anzahl von Typparametern als Typargumente, abgeglichen werden, wenn sich ggf. in genau einem Standardmodul, und klicken Sie dann auf den qualifizierten Namen bezieht, Geben Sie ein.</span><span class="sxs-lookup"><span data-stu-id="8e236-401">Otherwise, if `N` contains one or more standard modules, and `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the qualified name refers to that type.</span></span> <span data-ttu-id="8e236-402">Wenn `R` dem Namen der verfügbaren Typen mit der gleichen Anzahl von Typparametern als Typargumente, entspricht, wenn eine in mehr als eine standard-Modul einen Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="8e236-402">If `R` matches the name of accessible types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

5. <span data-ttu-id="8e236-403">Andernfalls tritt ein Kompilierungsfehler auf.</span><span class="sxs-lookup"><span data-stu-id="8e236-403">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="8e236-404">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="8e236-404">__Note.__</span></span> <span data-ttu-id="8e236-405">Eine Auswirkung dieses Prozesses für die Lösung ist, dass es sich bei Typmember nicht Namespaces oder Typen beim Auflösen von Namen für Namespace oder Typ überschatten.</span><span class="sxs-lookup"><span data-stu-id="8e236-405">An implication of this resolution process is that type members do not shadow namespaces or types when resolving namespace or type names.</span></span>

### <a name="unqualified-name-resolution-for-namespaces-and-types"></a><span data-ttu-id="8e236-406">Nicht qualifizierte namensauflösung für Namespaces und Typen</span><span class="sxs-lookup"><span data-stu-id="8e236-406">Unqualified Name Resolution for namespaces and types</span></span>

<span data-ttu-id="8e236-407">Nicht qualifizierte Namen `R(Of A)`, wobei `A` ist eine optionale Liste der Typargumente, die folgenden Schritte beschreiben, wie Sie bestimmen können, welcher Namespace oder Typ der nicht qualifizierte Name bezieht sich:</span><span class="sxs-lookup"><span data-stu-id="8e236-407">Given an unqualified name `R(Of A)`, where `A` is an optional type argument list, the following steps describe how to determine to which namespace or type the unqualified name refers:</span></span>

1. <span data-ttu-id="8e236-408">Wenn R, den Namen der Typparameter der aktuellen Methode entspricht, und es wurden keine Typargumente bereitgestellt, verweist der nicht qualifizierte Name an.</span><span class="sxs-lookup"><span data-stu-id="8e236-408">If R matches the name of a type parameter of the current method, and no type arguments were supplied, then the unqualified name refers to that type parameter.</span></span>

2.  <span data-ttu-id="8e236-409">Für jede geschachtelte Typ mit dem Namensverweis, beginnend mit des innersten Typs und zum äußersten:</span><span class="sxs-lookup"><span data-stu-id="8e236-409">For each nested type containing the name reference, starting from the innermost type and going to the outermost:</span></span>
    21. <span data-ttu-id="8e236-410">Wenn `R` übereinstimmt, den Namen eines Typparameters in der aktuelle Typ und kein Typ Argumente bereitgestellt wurden, der nicht qualifizierte Name auf den Typparameter verweist.</span><span class="sxs-lookup"><span data-stu-id="8e236-410">If `R` matches the name of a type parameter in the current type and no type arguments were supplied, then the unqualified name refers to that type parameter.</span></span>
    22. <span data-ttu-id="8e236-411">Andernfalls gilt: Wenn `R` Übereinstimmungen, die der Namen des eine zugängliche Typ mit der gleichen Anzahl von geschachtelter Typparametern als Typargumente, sofern vorhanden, und klicken Sie dann der nicht qualifizierte Namen bezieht sich auf diesen Typ.</span><span class="sxs-lookup"><span data-stu-id="8e236-411">Otherwise, if `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, then the unqualified name refers to that type.</span></span>

3. <span data-ttu-id="8e236-412">Für jeden geschachtelten Namespace, der die Namensverweis, beginnend mit des innersten-Namespaces und dem äußeren Namespace navigieren enthält:</span><span class="sxs-lookup"><span data-stu-id="8e236-412">For each nested namespace containing the name reference, starting from the innermost namespace and going to the outermost namespace:</span></span>
    31. <span data-ttu-id="8e236-413">Wenn `R` Übereinstimmungen, die der Namen der einem geschachtelten Namespace im aktuellen Namespace und keine Liste der Typargumente wird, angegeben, wird der nicht qualifizierte Name auf diesen geschachtelten Namespace verweist.</span><span class="sxs-lookup"><span data-stu-id="8e236-413">If `R` matches the name of a nested namespace in the current namespace and no type argument list is supplied, then the unqualified name refers to that nested namespace.</span></span>
    32. <span data-ttu-id="8e236-414">Andernfalls gilt: Wenn `R` der ein zugreifbarer Typ mit der gleichen Anzahl von Typparametern als Typargumente, wenn vorhanden, im aktuellen Namespace, dann lautet der nicht qualifizierte Name bezieht sich auf diesen Typ entspricht.</span><span class="sxs-lookup"><span data-stu-id="8e236-414">Otherwise, if `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in the current namespace, then the unqualified name refers to that type.</span></span>
    33. <span data-ttu-id="8e236-415">Wenn der Namespace, eine oder mehrere zugänglich Standardmodulen enthält, andernfalls und `R` entspricht Sie den Namen eines verfügbaren geschachtelten Typs mit der gleichen Anzahl von Typparametern als Typargumente, ggf. in genau einem Standardmodul, und klicken Sie dann den nicht qualifizierten Name bezieht sich auf dieses geschachtelten Typs.</span><span class="sxs-lookup"><span data-stu-id="8e236-415">Otherwise, if the namespace contains one or more accessible standard modules, and `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the unqualified name refers to that nested type.</span></span> <span data-ttu-id="8e236-416">Wenn `R` der Name des zugegriffen werden kann geschachtelte Typen, mit der gleichen Anzahl von Typparametern als Typargumente, abgeglichen werden, wenn eine in mehr als eine standard-Modul einen Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="8e236-416">If `R` matches the name of accessible nested types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

4. <span data-ttu-id="8e236-417">Wenn die Quelldatei ein oder mehrere Importaliase verfügt und `R` mit dem übereinstimmt, der nicht qualifizierte Name auf den Importalias verweist.</span><span class="sxs-lookup"><span data-stu-id="8e236-417">If the source file has one or more import aliases, and `R` matches the name of one of them, then the unqualified name refers to that import alias.</span></span> <span data-ttu-id="8e236-418">Wenn eine Liste der Typargumente angegeben wird, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="8e236-418">If a type argument list is supplied, a compile-time error occurs.</span></span>

5. <span data-ttu-id="8e236-419">Wenn die Quelldatei, die mit dem Namensverweis ein oder mehrere Importe aufweist:</span><span class="sxs-lookup"><span data-stu-id="8e236-419">If the source file containing the name reference has one or more imports:</span></span>
    51. <span data-ttu-id="8e236-420">Wenn `R` der ein zugreifbarer Typ mit der gleichen Anzahl von Typparametern als Typargumente, ggf. in genau eine importieren, und klicken Sie dann auf den nicht qualifizierten Namen bezieht sich auf diesen Typ entspricht.</span><span class="sxs-lookup"><span data-stu-id="8e236-420">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in exactly one import, then the unqualified name refers to that type.</span></span> <span data-ttu-id="8e236-421">Wenn `R` der Name des ein zugreifbarer Typ mit der gleichen Anzahl von Typparametern als Typargumente, abgeglichen werden, wenn ggf. in mehrere Importvorgänge und alle nicht denselben Typ, sind ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="8e236-421">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in more than one import and all are not the same type, a compile-time error occurs.</span></span>
    52. <span data-ttu-id="8e236-422">Wenn keine Liste der Typargumente angegeben wurde, andernfalls und `R` verfügbaren Typen in genau einem Import, und klicken Sie dann der nicht qualifizierte Namen, Namespace bezieht sich der Name eines Namespace entspricht.</span><span class="sxs-lookup"><span data-stu-id="8e236-422">Otherwise, if no type argument list was supplied and `R` matches the name of a namespace with accessible types in exactly one import, then the unqualified name refers to that namespace.</span></span> <span data-ttu-id="8e236-423">Wenn keine Liste der Typargumente angegeben wurde und `R` Übereinstimmungen, die der Namen eines Namespaces mit verfügbaren Typen in mehr als einem Import und alle sind nicht denselben Namespace, ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="8e236-423">If no type argument list was supplied and `R` matches the name of a namespace with accessible types in more than one import and all are not the same namespace, a compile-time error occurs.</span></span>
    53. <span data-ttu-id="8e236-424">Andernfalls, wenn die Importe eine oder mehrere zugänglich Standardmodule, enthalten und `R` entspricht, den Namen der verfügbaren geschachtelten Typs mit der gleichen Anzahl von Typparametern als Typargumente, die ggf. in genau einem Standardmodul, und klicken Sie dann den nicht qualifizierten Namen bezieht sich auf diesen Typ.</span><span class="sxs-lookup"><span data-stu-id="8e236-424">Otherwise, if the imports contain one or more accessible standard modules, and `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the unqualified name refers to that type.</span></span> <span data-ttu-id="8e236-425">Wenn `R` der Name des zugegriffen werden kann geschachtelte Typen, mit der gleichen Anzahl von Typparametern als Typargumente, abgeglichen werden, wenn eine in mehr als eine standard-Modul einen Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="8e236-425">If `R` matches the name of accessible nested types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

6. <span data-ttu-id="8e236-426">Wenn die kompilierungsumgebung eine oder mehrere Importaliase definiert und `R` mit dem übereinstimmt, der nicht qualifizierte Name auf den Importalias verweist.</span><span class="sxs-lookup"><span data-stu-id="8e236-426">If the compilation environment defines one or more import aliases, and `R` matches the name of one of them, then the unqualified name refers to that import alias.</span></span> <span data-ttu-id="8e236-427">Wenn eine Liste der Typargumente angegeben wird, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="8e236-427">If a type argument list is supplied, a compile-time error occurs.</span></span>

7. <span data-ttu-id="8e236-428">Wenn der kompilierungsumgebung ein oder mehrere Importe definiert:</span><span class="sxs-lookup"><span data-stu-id="8e236-428">If the compilation environment defines one or more imports:</span></span>
    71. <span data-ttu-id="8e236-429">Wenn `R` der ein zugreifbarer Typ mit der gleichen Anzahl von Typparametern als Typargumente, ggf. in genau eine importieren, und klicken Sie dann auf den nicht qualifizierten Namen bezieht sich auf diesen Typ entspricht.</span><span class="sxs-lookup"><span data-stu-id="8e236-429">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in exactly one import, then the unqualified name refers to that type.</span></span> <span data-ttu-id="8e236-430">Wenn `R` der Name des ein zugreifbarer Typ mit der gleichen Anzahl von Typparametern als Typargumente, abgeglichen werden, wenn eine in mehr als ein Import ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="8e236-430">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in more than one import, a compile-time error occurs.</span></span>
    72. <span data-ttu-id="8e236-431">Wenn keine Liste der Typargumente angegeben wurde, andernfalls und `R` verfügbaren Typen in genau einem Import, und klicken Sie dann der nicht qualifizierte Namen, Namespace bezieht sich der Name eines Namespace entspricht.</span><span class="sxs-lookup"><span data-stu-id="8e236-431">Otherwise, if no type argument list was supplied and `R` matches the name of a namespace with accessible types in exactly one import, then the unqualified name refers to that namespace.</span></span> <span data-ttu-id="8e236-432">Wenn keine Liste der Typargumente angegeben wurde und `R` entspricht der Name eines Namespace mit verfügbaren Typen in mehr als einem Import ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="8e236-432">If no type argument list was supplied and `R` matches the name of a namespace with accessible types in more than one import, a compile-time error occurs.</span></span>
    73. <span data-ttu-id="8e236-433">Andernfalls, wenn die Importe eine oder mehrere zugänglich Standardmodule, enthalten und `R` entspricht, den Namen der verfügbaren geschachtelten Typs mit der gleichen Anzahl von Typparametern als Typargumente, die ggf. in genau einem Standardmodul, und klicken Sie dann den nicht qualifizierten Namen bezieht sich auf diesen Typ.</span><span class="sxs-lookup"><span data-stu-id="8e236-433">Otherwise, if the imports contain one or more accessible standard modules, and `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the unqualified name refers to that type.</span></span> <span data-ttu-id="8e236-434">Wenn `R` der Name des zugegriffen werden kann geschachtelte Typen, mit der gleichen Anzahl von Typparametern als Typargumente, abgeglichen werden, wenn eine in mehr als eine standard-Modul einen Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="8e236-434">If `R` matches the name of accessible nested types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

8. <span data-ttu-id="8e236-435">Andernfalls tritt ein Kompilierungsfehler auf.</span><span class="sxs-lookup"><span data-stu-id="8e236-435">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="8e236-436">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="8e236-436">__Note.__</span></span> <span data-ttu-id="8e236-437">Eine Auswirkung dieses Prozesses für die Lösung ist, dass es sich bei Typmember nicht Namespaces oder Typen beim Auflösen von Namen für Namespace oder Typ überschatten.</span><span class="sxs-lookup"><span data-stu-id="8e236-437">An implication of this resolution process is that type members do not shadow namespaces or types when resolving namespace or type names.</span></span>

<span data-ttu-id="8e236-438">Normalerweise kann ein Namen in einem bestimmten Namespace nur einmal vorkommen.</span><span class="sxs-lookup"><span data-stu-id="8e236-438">Normally, a name can only occur once in a particular namespace.</span></span> <span data-ttu-id="8e236-439">Da Namespaces mehreren .NET Assemblys deklariert werden können, ist es jedoch möglich, dass eine Situation, in denen zwei Assemblys für einen Typ mit den gleichen vollqualifizierten Namen definieren.</span><span class="sxs-lookup"><span data-stu-id="8e236-439">However, because namespaces can be declared across multiple .NET assemblies, it is possible to have a situation where two assemblies define a type with the same fully qualified name.</span></span> <span data-ttu-id="8e236-440">In diesem Fall wird ein Typ, der deklariert, die in den aktuellen Satz von Quelldateien über einem Typ deklariert, die in einer externen .NET-Assembly bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="8e236-440">In that case, a type declared in the current set of source files is preferred over a type declared in an external .NET assembly.</span></span> <span data-ttu-id="8e236-441">Andernfalls der Name ist mehrdeutig, und es gibt keine Möglichkeit, um den Namen zu unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="8e236-441">Otherwise, the name is ambiguous and there is no way to disambiguate the name.</span></span>

## <a name="variables"></a><span data-ttu-id="8e236-442">Variablen</span><span class="sxs-lookup"><span data-stu-id="8e236-442">Variables</span></span>

<span data-ttu-id="8e236-443">Ein *Variable* stellt einen Speicherort.</span><span class="sxs-lookup"><span data-stu-id="8e236-443">A *variable* represents a storage location.</span></span> <span data-ttu-id="8e236-444">Jede Variable hat einen Typ, der bestimmt, welche Werte können in der Variablen gespeichert.</span><span class="sxs-lookup"><span data-stu-id="8e236-444">Every variable has a type that determines what values can be stored in the variable.</span></span> <span data-ttu-id="8e236-445">Da Visual Basic eine typsicheren Sprache ist, jede Variable in einem Programm verfügt über einen Typ und die Sprache wird sichergestellt, dass die Werte in gespeichert sind Variablen immer des entsprechenden Typs.</span><span class="sxs-lookup"><span data-stu-id="8e236-445">Because Visual Basic is a type-safe language, every variable in a program has a type and the language guarantees that values stored in variables are always of the appropriate type.</span></span> <span data-ttu-id="8e236-446">Variablen werden immer auf den Standardwert von ihrem Typ initialisiert, bevor alle Verweise auf die Variable, die ausgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="8e236-446">Variables are always initialized to the default value of their type before any reference to the variable can be made.</span></span> <span data-ttu-id="8e236-447">Es ist nicht möglich ist, nicht initialisierten Speicher zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="8e236-447">It is not possible to access uninitialized memory.</span></span>

## <a name="generic-types-and-methods"></a><span data-ttu-id="8e236-448">Generische Typen und Methoden</span><span class="sxs-lookup"><span data-stu-id="8e236-448">Generic Types and Methods</span></span>

<span data-ttu-id="8e236-449">Typen (außer Standardmodulen und Enumerationstypen) und Methoden deklarieren können *Typparameter*, welche Typen sind, die nicht, bis eine Instanz des Typs bereitgestellt werden deklariert ist, oder die Methode wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="8e236-449">Types (except for standard modules and enumerated types) and methods can declare *type parameters*, which are types that will not be provided until an instance of the type is declared or the method is invoked.</span></span> <span data-ttu-id="8e236-450">Typen und Methoden mit den beiden Typparametern werden auch bezeichnet als *generische Typen* und *generische Methoden*, da der Typ oder die Methode generisch, ohne spezifische Kenntnisse geschrieben werden muss die Typen, die durch Code angegeben werden, die den Typ oder eine Methode verwendet.</span><span class="sxs-lookup"><span data-stu-id="8e236-450">Types and methods with type parameters are also known as *generic types* and *generic methods*, respectively, because the type or method must be written generically, without specific knowledge of the types that will be supplied by code that uses the type or method.</span></span>

<span data-ttu-id="8e236-451">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="8e236-451">__Note.__</span></span> <span data-ttu-id="8e236-452">Zu diesem Zeitpunkt, obwohl die Methoden und Delegaten können generisch ist, werden Eigenschaften, Ereignisse und Operatoren können nicht generisch sein selbst.</span><span class="sxs-lookup"><span data-stu-id="8e236-452">At this time, even though methods and delegates can be generic, properties, events and operators cannot be generic themselves.</span></span> <span data-ttu-id="8e236-453">Sie können jedoch die Typparameter von der enthaltenden Klasse verwenden.</span><span class="sxs-lookup"><span data-stu-id="8e236-453">They may, however, use type parameters from the containing class.</span></span>

<span data-ttu-id="8e236-454">Aus der Perspektive des generischen Typs oder -Methode ist ein Typparameter ein Platzhaltertyp, der mit einem tatsächlichen Typ gefüllt werden wird, wenn der Typ oder eine Methode verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="8e236-454">From the perspective of the generic type or method, a type parameter is a placeholder type that will be filled in with an actual type when the type or method is used.</span></span> <span data-ttu-id="8e236-455">Typargumente werden ersetzt die Typparameter in den Typ oder Methode an dem Punkt, an dem der Typ oder eine Methode verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="8e236-455">Type arguments are substituted for the type parameters in the type or method at the point at which the type or method is used.</span></span> <span data-ttu-id="8e236-456">Beispielsweise kann eine generische Stapelklasse als implementiert werden:</span><span class="sxs-lookup"><span data-stu-id="8e236-456">For example, a generic stack class could be implemented as:</span></span>

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

<span data-ttu-id="8e236-457">Deklarationen, mit denen die `Stack(Of ItemType)` Klasse muss für den Typparameter ein Typargument angeben `ItemType`.</span><span class="sxs-lookup"><span data-stu-id="8e236-457">Declarations that use the `Stack(Of ItemType)` class must supply a type argument for the type parameter `ItemType`.</span></span> <span data-ttu-id="8e236-458">Dieser Typ wird dann ausgefüllt, wo `ItemType` wird verwendet, in der Klasse:</span><span class="sxs-lookup"><span data-stu-id="8e236-458">This type is then filled in wherever `ItemType` is used within the class:</span></span>

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

### <a name="type-parameters"></a><span data-ttu-id="8e236-459">Typparameter</span><span class="sxs-lookup"><span data-stu-id="8e236-459">Type Parameters</span></span>

<span data-ttu-id="8e236-460">Typparameter können auf Typ oder Methodendeklarationen, angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="8e236-460">Type parameters may be supplied on type or method declarations.</span></span> <span data-ttu-id="8e236-461">Jeden von Typparameter ist, einen Bezeichner für ein Argument vom Typ ein Platzhalter, der zum Erstellen eines konstruierten Typs oder der Methode bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="8e236-461">Each type parameter is an identifier which is a place-holder for a type argument that is supplied to create a constructed type or method.</span></span> <span data-ttu-id="8e236-462">Im Gegensatz dazu ist ein Argument vom Typ der tatsächliche Typ, der für den Typparameter ersetzt wird, wenn ein generischer Typ oder eine Methode verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="8e236-462">By contrast, a type argument is the actual type that is substituted for the type parameter when a generic type or method is used.</span></span>

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

<span data-ttu-id="8e236-463">Jeder Typparameter in einen Typ oder eine Methodendeklaration definiert einen Namen im Deklarationsbereich dieses Typs oder der Methode.</span><span class="sxs-lookup"><span data-stu-id="8e236-463">Each type parameter in a type or method declaration defines a name in the declaration space of that type or method.</span></span> <span data-ttu-id="8e236-464">Daher keine es den gleichen Namen wie einen anderen Typparameter, einen Typmember, einen Methodenparameter oder eine lokale Variable.</span><span class="sxs-lookup"><span data-stu-id="8e236-464">Thus, it cannot have the same name as another type parameter, a type member, a method parameter, or a local variable.</span></span> <span data-ttu-id="8e236-465">Der Bereich eines Typparameters auf einen Typ oder Methode ist der gesamte Typ oder Methode.</span><span class="sxs-lookup"><span data-stu-id="8e236-465">The scope of a type parameter on a type or method is the entire type or method.</span></span> <span data-ttu-id="8e236-466">Da der Typparameter für die gesamte Typdeklaration definiert sind, können geschachtelte Typen äußeren Typ-Parameter verwenden.</span><span class="sxs-lookup"><span data-stu-id="8e236-466">Because type parameters are scoped to the entire type declaration, nested types can use outer type parameters.</span></span> <span data-ttu-id="8e236-467">Dies bedeutet auch, dass der Parameter vom Typ müssen immer angegeben werden, wenn Sie den Zugriff auf Typen, die in generischen Typen geschachtelt:</span><span class="sxs-lookup"><span data-stu-id="8e236-467">This also means that type parameters must always be specified when accessing types nested inside generic types:</span></span>

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

<span data-ttu-id="8e236-468">Im Gegensatz zu anderen Member einer Klasse werden die Parameter vom Typ nicht geerbt.</span><span class="sxs-lookup"><span data-stu-id="8e236-468">Unlike other members of a class, type parameters are not inherited.</span></span> <span data-ttu-id="8e236-469">Der Typparameter in einem Typ können nur mit ihrem einfachen Namen verwiesen werden; Das heißt, sie mit dem enthaltenden Typnamen qualifizierten nicht möglich.</span><span class="sxs-lookup"><span data-stu-id="8e236-469">Type parameters in a type can only be referred to by their simple name; in other words, they cannot be qualified with the containing type name.</span></span> <span data-ttu-id="8e236-470">Obwohl das Gegenteil können Mitglied Programmierstil, die Typparameter in einem geschachtelten Typ oder ausblenden Typparameter des äußeren Typs deklariert:</span><span class="sxs-lookup"><span data-stu-id="8e236-470">Although it is bad programming style, the type parameters in a nested type can hide a member or type parameter declared in the outer type:</span></span>

```vb
Class Outer(Of T)
    Class Inner(Of T)
        Public t1 As T    ' Refers to Inner's T
    End Class
End Class
```

<span data-ttu-id="8e236-471">Typen und Methoden, möglicherweise basierend auf der Anzahl der Typparameter überladen werden (oder *Stelligkeit*), die die Typen oder Methoden zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="8e236-471">Types and methods may be overloaded based on the number of type parameters (or *arity*) that the types or methods declare.</span></span> <span data-ttu-id="8e236-472">Beispielsweise sind die folgenden Deklarationen zulässig:</span><span class="sxs-lookup"><span data-stu-id="8e236-472">For example, the following declarations are legal:</span></span>

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

<span data-ttu-id="8e236-473">Bei Typen sind Überladungen für die Anzahl der angegebenen Typargumente immer zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="8e236-473">In the case of types, overloads are always matched against the number of type arguments specified.</span></span> <span data-ttu-id="8e236-474">Dies ist hilfreich, wenn Sie generische und nicht generischen Klassen zusammen im selben Programm verwenden:</span><span class="sxs-lookup"><span data-stu-id="8e236-474">This is useful when using both generic and non-generic classes together in the same program:</span></span>

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

<span data-ttu-id="8e236-475">Regeln für Methoden, die für Typparameter überladen werden im Abschnitt zur überladungsauflösung behandelt.</span><span class="sxs-lookup"><span data-stu-id="8e236-475">Rules for methods overloaded on type parameters are covered in the section on method overload resolution.</span></span>

<span data-ttu-id="8e236-476">Innerhalb der enthaltenden Deklaration gelten als Typparameter vollständige Typen.</span><span class="sxs-lookup"><span data-stu-id="8e236-476">Within the containing declaration, type parameters are considered full types.</span></span> <span data-ttu-id="8e236-477">Da ein Typparameter mit vielen verschiedenen tatsächliche Typargumenten instanziiert werden kann, über Typparameter verfügen, leicht unterschiedliche Vorgänge und Einschränkungen als andere Arten, wie im folgenden beschrieben:</span><span class="sxs-lookup"><span data-stu-id="8e236-477">Since a type parameter can be instantiated with many different actual type arguments, type parameters have slightly different operations and restrictions than other types as described below:</span></span>

* <span data-ttu-id="8e236-478">Ein Typparameter kann nicht verwendet werden, direkt auf eine Basisklasse oder Schnittstelle zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="8e236-478">A type parameter cannot be used directly to declare a base class or interface.</span></span>

* <span data-ttu-id="8e236-479">Die Regeln für die Suche nach Membern auf, dass die Parameter der Einschränkungen, sofern vorhanden, abhängig, die auf den Typparameter angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="8e236-479">The rules for member lookup on type parameters depend on the constraints, if any, applied to the type parameter.</span></span>

* <span data-ttu-id="8e236-480">Die verfügbaren Konvertierungen für ein Typparameter hängen von den Einschränkungen, die ggf. auf die Typparameter angewendet.</span><span class="sxs-lookup"><span data-stu-id="8e236-480">The available conversions for a type parameter depend on the constraints, if any, applied to the type parameters.</span></span>

* <span data-ttu-id="8e236-481">In Ermangelung einer `Structure` Einschränkung ein Wert mit einem Typ, dargestellt durch einen Typparameter kann verglichen werden mit `Nothing` mit `Is` und `IsNot`.</span><span class="sxs-lookup"><span data-stu-id="8e236-481">In the absence of a `Structure` constraint, a value with a type represented by a type parameter can be compared with `Nothing` using `Is` and `IsNot`.</span></span>

* <span data-ttu-id="8e236-482">Ein Typparameter kann nur verwendet werden, eine `New` Ausdruck, wenn der Typparameter, durch eingeschränkt wird eine `New` oder ein `Structure` Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="8e236-482">A type parameter can only be used in a `New` expression if the type parameter is constrained by a `New` or a `Structure` constraint.</span></span>

* <span data-ttu-id="8e236-483">Ein Typparameter kann nicht an einer beliebigen Stelle verwendet werden, in eine Attribut-Ausnahme in einem `GetType` Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="8e236-483">A type parameter cannot be used anywhere within an attribute exception within a `GetType` expression.</span></span>

* <span data-ttu-id="8e236-484">Typparameter können als Typargumente auf andere generische Typen und Parametern verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8e236-484">Type parameters can be used as type arguments to other generic types and parameters.</span></span>

<span data-ttu-id="8e236-485">Im folgende Beispiel ist ein generischer Typ, der erweitert die `Stack(Of ItemType)` Klasse:</span><span class="sxs-lookup"><span data-stu-id="8e236-485">The following example is a generic type that extends the `Stack(Of ItemType)` class:</span></span>

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

<span data-ttu-id="8e236-486">Wenn eine Deklaration ein Typargument stellt `MyStack`, dasselbe Typargument gelten für `Stack` ebenfalls.</span><span class="sxs-lookup"><span data-stu-id="8e236-486">When a declaration supplies a type argument to `MyStack`, the same type argument will be applied to `Stack` as well.</span></span>

<span data-ttu-id="8e236-487">Als Typ sind Typparameter ausschließlich ein Kompilierzeit-Konstrukt.</span><span class="sxs-lookup"><span data-stu-id="8e236-487">As a type, type parameters are purely a compile-time construct.</span></span> <span data-ttu-id="8e236-488">Zur Laufzeit ist jeden von Typparameter auf einen Typ zur Laufzeit gebunden, die durch Angabe von Typargument für generischen Deklaration angegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="8e236-488">At run-time, each type parameter is bound to a run-time type that was specified by supplying a type argument to the generic declaration.</span></span> <span data-ttu-id="8e236-489">Daher wird der Typ einer Variablen mit einem Typparameter deklariert zur Laufzeit, einen nicht generischen Typ oder einen bestimmten konstruierten Typ sein.</span><span class="sxs-lookup"><span data-stu-id="8e236-489">Thus, the type of a variable declared with a type parameter will, at run-time, be a non-generic type or a specific constructed type.</span></span> <span data-ttu-id="8e236-490">Die Ausführung zur Laufzeit alle Anweisungen und Ausdrücke, die im Zusammenhang mit Typparametern verwendet den tatsächlichen Typ, der übergeben wurde, als Typargument für diesen Parameter.</span><span class="sxs-lookup"><span data-stu-id="8e236-490">The run-time execution of all statements and expressions involving type parameters uses the actual type that was supplied as the type argument for that parameter.</span></span>


### <a name="type-constraints"></a><span data-ttu-id="8e236-491">Typeinschränkungen</span><span class="sxs-lookup"><span data-stu-id="8e236-491">Type Constraints</span></span>

<span data-ttu-id="8e236-492">Da ein Typargument einen beliebigen Typ im Typsystem sein kann, kann nicht keine Annahmen über einen Typparameter einer generischen Typ- oder stellen werden.</span><span class="sxs-lookup"><span data-stu-id="8e236-492">Because a type argument can be any type in the type system, a generic type or method cannot make any assumptions about a type parameter.</span></span> <span data-ttu-id="8e236-493">Daher gelten die Elemente eines Typparameters werden die Elemente des Typs `Object`, da alle Typen abgeleitet `Object`.</span><span class="sxs-lookup"><span data-stu-id="8e236-493">Thus, the members of a type parameter are considered to be the members of the type `Object`, since all types derive from `Object`.</span></span>

<span data-ttu-id="8e236-494">Im Fall einer Sammlung wie `Stack(Of ItemType)`, dies möglicherweise nicht besonders wichtig, Einschränkung, aber es gibt möglicherweise Fälle, in denen ein generischer Typ eine Annahme über die Typen machen wollen, die als Typargumente angegeben.</span><span class="sxs-lookup"><span data-stu-id="8e236-494">In the case of a collection like `Stack(Of ItemType)`, this fact may not be a particularly important restriction, but there may be cases where a generic type may wish to make an assumption about the types that will be supplied as type arguments.</span></span> <span data-ttu-id="8e236-495">*Typeinschränkungen* von Typparametern, die einschränken, welche Typen wie ein Typparameter angegeben werden, und Zulassen von generischen Typen oder Methoden mehr über den Typparameter annehmen platziert werden kann.</span><span class="sxs-lookup"><span data-stu-id="8e236-495">*Type constraints* can be placed on type parameters that restrict which types can be supplied as a type parameter and allow generic types or methods to assume more about type parameters.</span></span>

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

<span data-ttu-id="8e236-496">In diesem Beispiel die `DisposableStack(Of ItemType)` schränkt den Typparameter nur Typen, die die Schnittstelle implementieren `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="8e236-496">In this example, the `DisposableStack(Of ItemType)` constrains its type parameter to only types that implement the interface `System.IDisposable`.</span></span> <span data-ttu-id="8e236-497">Daher können sie implementieren eine `Dispose` -Methode, die alle Objekte frei bleibt weiterhin in der Warteschlange.</span><span class="sxs-lookup"><span data-stu-id="8e236-497">As a result, it can implement a `Dispose` method that disposes any objects still left in the queue.</span></span>

<span data-ttu-id="8e236-498">Eine Einschränkung für einen muss eine der speziellen Einschränkungen `Class`, `Structure`, oder `New`, oder es muss ein Typ `T` , in denen:</span><span class="sxs-lookup"><span data-stu-id="8e236-498">A type constraint must be one of the special constraints `Class`, `Structure`, or `New`, or it must be a type `T` where:</span></span>

* <span data-ttu-id="8e236-499">`T` ist eine Klasse, Schnittstelle oder ein Typparameter.</span><span class="sxs-lookup"><span data-stu-id="8e236-499">`T` is a class, an interface, or a type parameter.</span></span>

* <span data-ttu-id="8e236-500">`T` ist nicht `NotInheritable`.</span><span class="sxs-lookup"><span data-stu-id="8e236-500">`T` is not `NotInheritable`.</span></span>

* <span data-ttu-id="8e236-501">`T` ist kein oder ein Typ von einem der folgenden speziellen Typen geerbt: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, oder `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="8e236-501">`T` is not one of, or a type inherited from one of, the following special types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span>

* <span data-ttu-id="8e236-502">`T` ist nicht `Object`.</span><span class="sxs-lookup"><span data-stu-id="8e236-502">`T` is not `Object`.</span></span> <span data-ttu-id="8e236-503">Da alle Typen abgeleitet `Object`, eine solche Einschränkung hätte keine Auswirkung, wenn sie zulässig wären.</span><span class="sxs-lookup"><span data-stu-id="8e236-503">Since all types derive from `Object`, such a constraint would have no effect if it were permitted.</span></span>

* <span data-ttu-id="8e236-504">`T` muss mindestens dieselben zugriffsmöglichkeiten bieten wie die generischen Typ oder Methode, die deklariert wird.</span><span class="sxs-lookup"><span data-stu-id="8e236-504">`T` must be at least as accessible as the generic type or method being declared.</span></span>

<span data-ttu-id="8e236-505">Mehrere Einschränkungen können für einen einzelnen Typparameter angegeben werden, indem typeinschränkungen in geschweiften Klammern einschließen (`{}`)...</span><span class="sxs-lookup"><span data-stu-id="8e236-505">Multiple type constraints can be specified for a single type parameter by enclosing the type constraints in curly braces (`{}`)..</span></span> <span data-ttu-id="8e236-506">Nur eine Einschränkung für einen bestimmten Typ-Parameter kann eine Klasse sein.</span><span class="sxs-lookup"><span data-stu-id="8e236-506">Only one type constraint for a given type parameter can be a class.</span></span> <span data-ttu-id="8e236-507">Es ist ein Fehler zum Kombinieren einer `Structure` speziellen Einschränkung mit einer Einschränkung der benannten Klasse oder die `Class` speziellen Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="8e236-507">It is an error to combine a `Structure` special constraint with a named class constraint or the `Class` special constraint.</span></span>

```vb
Class ControlFactory(Of T As {Control, New})
    ...
End Class
```

<span data-ttu-id="8e236-508">Einschränkungen können die enthaltenen Typen oder eines der Typparameter der enthaltenden Typen.</span><span class="sxs-lookup"><span data-stu-id="8e236-508">Type constraints can use the containing types or any of the containing types' type parameters.</span></span> <span data-ttu-id="8e236-509">Im folgenden Beispiel erfordert die Einschränkung an, dass das angegebene Typargument eine generische Schnittstelle mit sich selbst als Typargument implementiert:</span><span class="sxs-lookup"><span data-stu-id="8e236-509">In the following example, the constraint requires that the type argument supplied implements a generic interface using itself as a type argument:</span></span>

```vb
Class Sorter(Of V As IComparable(Of V))
    ...
End Class
```

<span data-ttu-id="8e236-510">Die spezielle typeinschränkung `Class` schränkt das angegebene Typargument beliebige Verweistypen.</span><span class="sxs-lookup"><span data-stu-id="8e236-510">The special type constraint `Class` constrains the supplied type argument to any reference type.</span></span>

<span data-ttu-id="8e236-511">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="8e236-511">__Note.__</span></span> <span data-ttu-id="8e236-512">Die spezielle typeinschränkung `Class` eine-Schnittstelle erfüllt werden kann.</span><span class="sxs-lookup"><span data-stu-id="8e236-512">The special type constraint `Class` can be satisfied by an interface.</span></span> <span data-ttu-id="8e236-513">Und eine Struktur kann eine Schnittstelle implementieren.</span><span class="sxs-lookup"><span data-stu-id="8e236-513">And a structure can implement an interface.</span></span> <span data-ttu-id="8e236-514">Daher die Einschränkung `(Of T As U, U As Class)` kann mit "T" einer Struktur zufrieden sein (dem erfüllt nicht die `Class` speziellen Einschränkung), und "U" eine Schnittstelle, die er implementiert (erfüllt die der `Class` speziellen Einschränkung).</span><span class="sxs-lookup"><span data-stu-id="8e236-514">Therefore, the constraint `(Of T As U, U As Class)` might be satisfied with "T" a structure (which does not satisfy the `Class` special constraint), and "U" an interface that it implements (which does satisfy the `Class` special constraint).</span></span>

<span data-ttu-id="8e236-515">Die spezielle typeinschränkung `Structure` schränkt das angegebene Typargument jeder Werttyp außer `System.Nullable(Of T)`.</span><span class="sxs-lookup"><span data-stu-id="8e236-515">The special type constraint `Structure` constrains the supplied type argument to any value type except `System.Nullable(Of T)`.</span></span>

<span data-ttu-id="8e236-516">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="8e236-516">__Note.__</span></span> <span data-ttu-id="8e236-517">Struktur-Einschränkungen lassen keine `System.Nullable(Of T)` so, dass es nicht möglich, geben Sie `System.Nullable(Of T)` als Typargument an sich selbst.</span><span class="sxs-lookup"><span data-stu-id="8e236-517">Structure constraints do not allow `System.Nullable(Of T)` so that it is not possible to supply `System.Nullable(Of T)` as a type argument to itself.</span></span>

<span data-ttu-id="8e236-518">Die spezielle typeinschränkung `New` erfordert, dass das angegebene Typargument einen zugänglichen parameterlosen Konstruktor aufweisen muss und kann nicht deklariert werden `MustInherit`.</span><span class="sxs-lookup"><span data-stu-id="8e236-518">The special type constraint `New` requires that the supplied type argument must have an accessible parameterless constructor and cannot be declared `MustInherit`.</span></span> <span data-ttu-id="8e236-519">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-519">For example:</span></span>

```vb
Class Factory(Of T As New)
    Function CreateInstance() As T
        Return New T()
    End Function
End Class
```

<span data-ttu-id="8e236-520">Einschränkung für einen Klassentyp ist erforderlich, dass das angegebene Typargument entweder befinden, geben Sie als die oder von ihm erben.</span><span class="sxs-lookup"><span data-stu-id="8e236-520">A class type constraint requires that the supplied type argument must either be that type as or inherit from it.</span></span> <span data-ttu-id="8e236-521">Eine schnittstelleneinschränkung-Typ erfordert, dass das angegebene Typargument diese Schnittstelle implementieren muss.</span><span class="sxs-lookup"><span data-stu-id="8e236-521">An interface type constraint requires that the supplied type argument must implement that interface.</span></span> <span data-ttu-id="8e236-522">Eine Einschränkung für einen Parameter erfordert, dass das angegebene Typargument abgeleitet werden muss, oder alle die Grenzen für den entsprechenden Typparameter angegeben implementieren.</span><span class="sxs-lookup"><span data-stu-id="8e236-522">A type parameter constraint requires that the supplied type argument must derive from or implement all of the bounds given for the matching type parameter.</span></span> <span data-ttu-id="8e236-523">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-523">For example:</span></span>

```vb
Class List(Of T)
    Sub AddRange(Of S As T)(collection As IEnumerable(Of S))
        ...
    End Sub
End Class
```

<span data-ttu-id="8e236-524">In diesem Beispiel ist der Typparameter `S` auf `AddRange` ist beschränkt auf den Typparameter `T` von `List`.</span><span class="sxs-lookup"><span data-stu-id="8e236-524">In this example, the type parameter `S` on `AddRange` is constrained to the type parameter `T` of `List`.</span></span> <span data-ttu-id="8e236-525">Dies bedeutet, dass eine `List(Of Control)` einschränken würden `AddRange`der Typparameter in einen beliebigen Typ, der oder erbt von `Control`.</span><span class="sxs-lookup"><span data-stu-id="8e236-525">This means that a `List(Of Control)` would constrain `AddRange`'s type parameter to any type that is or inherits from `Control`.</span></span>

<span data-ttu-id="8e236-526">Eine Einschränkung für einen Parameter `Of S As T` wird aufgelöst, indem transitiv Hinzufügen aller `T`Einschränkungen auf `S`, die keine besonderen Einschränkungen (`Class`, `Structure`, `New`).</span><span class="sxs-lookup"><span data-stu-id="8e236-526">A type parameter constraint `Of S As T` is resolved by transitively adding all of `T`'s constraints onto `S`, other than the special constraints (`Class`, `Structure`, `New`).</span></span> <span data-ttu-id="8e236-527">Es ist ein Fehler, wenn zirkuläre Einschränkungen (z. B. `Of S As T, T As S`).</span><span class="sxs-lookup"><span data-stu-id="8e236-527">It is an error to have circular constraints (e.g. `Of S As T, T As S`).</span></span> <span data-ttu-id="8e236-528">Es ist ein Fehler, die eine Einschränkung für einen Parameter haben die selbst verfügt über die `Structure` Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="8e236-528">It is an error to have a type parameter constraint which itself has the `Structure` constraint.</span></span> <span data-ttu-id="8e236-529">Nach dem Hinzufügen von Einschränkungen, ist es möglich, dass eine Anzahl von Situationen auftreten:</span><span class="sxs-lookup"><span data-stu-id="8e236-529">After adding constraints, it is possible that a number of special situations may occur:</span></span>

* <span data-ttu-id="8e236-530">Wenn mehrere klasseneinschränkungen vorhanden ist, die am stärksten abgeleitete Klasse gilt die Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="8e236-530">If multiple class constraints exist, the most derived class is considered to be the constraint.</span></span> <span data-ttu-id="8e236-531">Wenn eine oder mehrere klasseneinschränkungen keine vererbungsbeziehung haben, wird die Einschränkung ist unmöglich machen, und es ist ein Fehler.</span><span class="sxs-lookup"><span data-stu-id="8e236-531">If one or more class constraints have no inheritance relationship, the constraint is unsatisfiable and it is an error.</span></span>

 * <span data-ttu-id="8e236-532">Wenn ein Typparameter kombiniert eine `Structure` speziellen Einschränkung mit einer Einschränkung der benannten Klasse oder die `Class` speziellen Einschränkung ist ein Fehler.</span><span class="sxs-lookup"><span data-stu-id="8e236-532">If a type parameter combines a `Structure` special constraint with a named class constraint or the `Class` special constraint, it is an error.</span></span> <span data-ttu-id="8e236-533">Eine Class-Einschränkung möglicherweise `NotInheritable`, in diesem Fall keine abgeleiteten Typen gegen diese Einschränkung werden akzeptiert, und es ist ein Fehler.</span><span class="sxs-lookup"><span data-stu-id="8e236-533">A class constraint may be `NotInheritable`, in which case no derived types of that constraint are accepted and it is an error.</span></span>

<span data-ttu-id="8e236-534">Der Typ kann eine der sein oder ein Typ aus, die folgenden speziellen Typen geerbt: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, oder `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="8e236-534">The type may be one of, or a type inherited from, the following special types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span> <span data-ttu-id="8e236-535">In diesem Fall wird nur der Typ oder einen Typ geerbt, akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="8e236-535">In that case, only the type, or a type inherited from it, is accepted.</span></span> <span data-ttu-id="8e236-536">Ein Typparameter, beschränkt auf einem dieser Typen kann nur die Konvertierungen zulässig verwenden die `DirectCast` Operator.</span><span class="sxs-lookup"><span data-stu-id="8e236-536">A type parameter constrained to one of these types can only use the conversions allowed by the `DirectCast` operator.</span></span> <span data-ttu-id="8e236-537">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-537">For example:</span></span>

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

<span data-ttu-id="8e236-538">Darüber hinaus kann kein Typparameter, der auf einen Werttyp aufgrund eines der oben genannten lockerungen für die eingeschränkte alle auf diesem Typ definierten Methoden aufrufen.</span><span class="sxs-lookup"><span data-stu-id="8e236-538">Additionally, a type parameter constrained to a value type due to one of the above relaxations cannot call any methods defined on that value type.</span></span> <span data-ttu-id="8e236-539">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-539">For example:</span></span>

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

<span data-ttu-id="8e236-540">Wenn die Einschränkung, nach der Ersetzung als eines Arraytyps am Ende, ist ebenfalls ein kovariant Arraytyp zulässig.</span><span class="sxs-lookup"><span data-stu-id="8e236-540">If the constraint, after substitution, ends up as an array type, any covariant array type is allowed as well.</span></span> <span data-ttu-id="8e236-541">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-541">For example:</span></span>

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

<span data-ttu-id="8e236-542">Ein Typparameter mit einer Klassen- oder schnittstelleneinschränkung haben dieselben Member wie diese Klasse oder Schnittstelle Einschränkung gilt.</span><span class="sxs-lookup"><span data-stu-id="8e236-542">A type parameter with a class or interface constraint is considered to have the same members as that class or interface constraint.</span></span> <span data-ttu-id="8e236-543">Wenn ein Typparameter mehrere Einschränkungen verfügt, gilt der Typparameter die Vereinigung aller Elemente der Einschränkungen haben.</span><span class="sxs-lookup"><span data-stu-id="8e236-543">If a type parameter has multiple constraints, then the type parameter is considered to have the union of all the members of the constraints.</span></span> <span data-ttu-id="8e236-544">Wenn Elemente mit dem gleichen Namen in mehr als eine Einschränkung vorhanden sind, und klicken Sie dann Elemente, in der folgenden Reihenfolge ausgeblendet werden: die klasseneinschränkung Blendet Member in der schnittstelleneinschränkungen an, die Ausblenden von Elementen in `System.ValueType` (Wenn `Structure` Einschränkung angegeben ist), der Member in verborgen `Object`.</span><span class="sxs-lookup"><span data-stu-id="8e236-544">If there are members with the same name in more than one constraint, then members are hidden in the following order: the class constraint hides members in interface constraints, which hide members in `System.ValueType` (if `Structure` constraint is specified), which hides members in `Object`.</span></span> <span data-ttu-id="8e236-545">Wenn ein Element mit dem gleichen Namen in mehr als eine schnittstelleneinschränkung angezeigt wird der Member ist nicht verfügbar ist (wie in der mehrfachvererbung-Schnittstelle), und der Type-Parameter muss in die gewünschte Schnittstelle umgewandelt werden.</span><span class="sxs-lookup"><span data-stu-id="8e236-545">If a member with the same name appears in more than one interface constraint the member is unavailable (as in multiple interface inheritance) and the type parameter must be cast to the desired interface.</span></span> <span data-ttu-id="8e236-546">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-546">For example:</span></span>

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

<span data-ttu-id="8e236-547">Wenn der Typparameter als Typargumente angeben, müssen die Typparameter der Einschränkungen der entsprechende Typparameter erfüllen.</span><span class="sxs-lookup"><span data-stu-id="8e236-547">When supplying type parameters as type arguments, the type parameters must satisfy the constraints of the matching type parameters.</span></span>

```vb
Class Base(Of T As Class)
End Class

Class Derived(Of V)
    ' Error: V does not satisfy the constraints of T
    Inherits Base(Of V)
End Class
```

<span data-ttu-id="8e236-548">Werte einen eingeschränkten Typparameter können verwendet werden, auf die Instanz-Elemente, einschließlich Instanzmethoden zur Verfügung, in der Einschränkung angegeben.</span><span class="sxs-lookup"><span data-stu-id="8e236-548">Values of a constrained type parameter can be used to access the instance members, including instance methods, specified in the constraint.</span></span>

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

### <a name="type-parameter-variance"></a><span data-ttu-id="8e236-549">Varianz der Type-Parameter</span><span class="sxs-lookup"><span data-stu-id="8e236-549">Type Parameter Variance</span></span>

<span data-ttu-id="8e236-550">Ein Typparameter eine Schnittstelle oder eine Typdeklaration für den Delegaten kann optional angeben, ein *Varianz Modifizierer*.</span><span class="sxs-lookup"><span data-stu-id="8e236-550">A type parameter in an interface or a delegate type declaration can optionally specify a *variance modifier*.</span></span> <span data-ttu-id="8e236-551">Typparameter mit varianzmodifizierer einschränken, wie der Typparameter in der Schnittstellen oder Delegate-Typ verwendet werden kann, aber einen generischen Schnittstellen oder Delegate-Typ in einem anderen generischen Typ mit Argumenten der variant-kompatibler Typ konvertiert werden können.</span><span class="sxs-lookup"><span data-stu-id="8e236-551">Type parameters with variance modifiers restrict how the type parameter can be used in the interface or delegate type but allow a generic interface or delegate type to be converted to another generic type with variant compatible type arguments.</span></span> <span data-ttu-id="8e236-552">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-552">For example:</span></span>

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

<span data-ttu-id="8e236-553">Generische Schnittstellen, die über Typparameter mit varianzmodifizierer weisen einige Einschränkungen auf:</span><span class="sxs-lookup"><span data-stu-id="8e236-553">Generic interfaces that have type parameters with variance modifiers have several restrictions:</span></span>

* <span data-ttu-id="8e236-554">Eine Ereignisdeklaration, die angibt, eine Parameterliste darf nicht enthalten (aber eine benutzerdefinierte Ereignisdeklaration oder eine Ereignisdeklaration mit einem Delegattypen ist zulässig).</span><span class="sxs-lookup"><span data-stu-id="8e236-554">They cannot contain an event declaration that specifies a parameter list (but a custom event declaration or an event declaration with a delegate type is allowed).</span></span>

* <span data-ttu-id="8e236-555">Sie können keine geschachtelte Klasse, Struktur oder enumerierten Typ enthalten.</span><span class="sxs-lookup"><span data-stu-id="8e236-555">They cannot contain a nested class, structure, or enumerated type.</span></span>

<span data-ttu-id="8e236-556">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="8e236-556">__Note.__</span></span> <span data-ttu-id="8e236-557">Diese Einschränkungen sind darauf zurückzuführen, dass Typen, die in generischen Typen geschachtelt sind, implizit die generischen Parameter ihres übergeordneten Elements zu kopieren.</span><span class="sxs-lookup"><span data-stu-id="8e236-557">These restrictions are due to the fact that types nested in generic types implicitly copy the generic parameters of their parent.</span></span> <span data-ttu-id="8e236-558">Im Fall von geschachtelten Klassen, Strukturen oder Enumerationstypen diese Arten von Typen varianzmodifizierer für ihre Typparameter nicht möglich.</span><span class="sxs-lookup"><span data-stu-id="8e236-558">In the case of nested classes, structures, or enumerated types, those kinds of types cannot have variance modifiers on their type parameters.</span></span> <span data-ttu-id="8e236-559">Im Fall einer Ereignisdeklaration mit einer Parameterliste, die generierten geschachtelte Delegate-Klasse möglicherweise verwirrend erscheinen Fehler, wenn ein Typ, der angezeigt wird, die in verwendet werden eine `In` Position (d. h. einen Parametertyp) wird verwendet, eine `Out` Position (d. h. die Typ des Ereignisses).</span><span class="sxs-lookup"><span data-stu-id="8e236-559">In the case of an event declaration with a parameter list, the generated nested delegate class could have confusing errors when a type that appears to be used in an `In` position (i.e. a parameter type) is actually used in an `Out` position (i.e. the type of the event).</span></span>

<span data-ttu-id="8e236-560">Ein Typparameter, die mit der Out-Modifizierer deklariert wird ist *kovariant*.</span><span class="sxs-lookup"><span data-stu-id="8e236-560">A type parameter that is declared with the Out modifier is *covariant*.</span></span> <span data-ttu-id="8e236-561">Informell wird ein kovarianter Typparameter kann nur in einer Ausgabeposition – d. h. ein Wert, der vom Typ Schnittstellen oder Delegate zurückgegeben werden – verwendet werden und kann nicht in einer Eingabe Position verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8e236-561">Informally, a covariant type parameter can only be used in an output position -- i.e. a value that is being returned from the interface or delegate type -- and cannot be used in an input position.</span></span> <span data-ttu-id="8e236-562">Ein Typ `T` gilt *gültige kovariant* wenn:</span><span class="sxs-lookup"><span data-stu-id="8e236-562">A type `T` is considered to be *valid covariantly* if:</span></span>

* <span data-ttu-id="8e236-563">`T` ist eine Klasse, Struktur oder enumerierten Typ an.</span><span class="sxs-lookup"><span data-stu-id="8e236-563">`T` is a class, structure, or enumerated type.</span></span>

* <span data-ttu-id="8e236-564">`T` ist nicht generische Delegaten oder dieser Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="8e236-564">`T` is non-generic delegate or interface type.</span></span>

* <span data-ttu-id="8e236-565">`T` ist ein Arraytyp, dessen Elementtyp kovariant gültig ist.</span><span class="sxs-lookup"><span data-stu-id="8e236-565">`T` is an array type whose element type is valid covariantly.</span></span>

* <span data-ttu-id="8e236-566">`T` ist ein Typparameter, die als nicht deklariert wurde ein `Out` Typparameter.</span><span class="sxs-lookup"><span data-stu-id="8e236-566">`T` is a type parameter which was not declared as an `Out` type parameter.</span></span>

* <span data-ttu-id="8e236-567">`T` wird von ein konstruierter Typ von Schnittstellen oder Delegate `X(Of P1,...,Pn)` mit Typargumenten `A1,...,An` so, dass:</span><span class="sxs-lookup"><span data-stu-id="8e236-567">`T` is a constructed interface or delegate type `X(Of P1,...,Pn)` with type arguments `A1,...,An` such that:</span></span>

  * <span data-ttu-id="8e236-568">Wenn `Pi` wurde dann als ein Out-Parameter vom Typ deklariert `Ai` kovariant ist gültig.</span><span class="sxs-lookup"><span data-stu-id="8e236-568">If `Pi` was declared as an Out type parameter then `Ai` is valid covariantly.</span></span>

  * <span data-ttu-id="8e236-569">Wenn `Pi` wurde dann als Typ In-Parameter deklariert `Ai` gültige kontravariant ist.</span><span class="sxs-lookup"><span data-stu-id="8e236-569">If `Pi` was declared as an In type parameter then `Ai` is valid contravariantly.</span></span>

<span data-ttu-id="8e236-570">Folgendes muss in einen Typ von Schnittstellen oder Delegate kovariant gültig sein:</span><span class="sxs-lookup"><span data-stu-id="8e236-570">The following must be valid covariantly in an interface or delegate type:</span></span>

* <span data-ttu-id="8e236-571">Die Basisschnittstelle einer Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="8e236-571">The base interface of an interface.</span></span>

* <span data-ttu-id="8e236-572">Der Rückgabetyp einer Funktion oder der Typ des Delegaten.</span><span class="sxs-lookup"><span data-stu-id="8e236-572">The return type of a function or the delegate type.</span></span>

* <span data-ttu-id="8e236-573">Der Typ einer Eigenschaft liegt eine `Get` Accessor.</span><span class="sxs-lookup"><span data-stu-id="8e236-573">The type of a property if there is a `Get` accessor.</span></span>

* <span data-ttu-id="8e236-574">Der Typ any `ByRef` Parameter.</span><span class="sxs-lookup"><span data-stu-id="8e236-574">The type of any `ByRef` parameter.</span></span>

<span data-ttu-id="8e236-575">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-575">For example:</span></span>

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

<span data-ttu-id="8e236-576">__Beachten Sie.__</span><span class="sxs-lookup"><span data-stu-id="8e236-576">__Note.__</span></span> <span data-ttu-id="8e236-577">`Out` ist ein reserviertes Wort.</span><span class="sxs-lookup"><span data-stu-id="8e236-577">`Out` is not a reserved word.</span></span>

<span data-ttu-id="8e236-578">Ein Typparameter, der mit dem In-Modifizierer deklariert ist ist *kontravariant*.</span><span class="sxs-lookup"><span data-stu-id="8e236-578">A type parameter that is declared with the In modifier is *contravariant*.</span></span> <span data-ttu-id="8e236-579">Informell ein kontravarianten Typparameter kann nur in einer Eingabe Position – d. h. ein Wert, der in der in dem Schnittstellen oder Delegate-Typ übergeben wird – verwendet werden und kann nicht in einer Ausgabeposition verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8e236-579">Informally, a contravariant type parameter can only be used in an input position -- i.e. a value that is being passed in to the interface or delegate type -- and cannot be used in an output position.</span></span> <span data-ttu-id="8e236-580">Ein Typ `T` gilt *gültige kontravariant* wenn:</span><span class="sxs-lookup"><span data-stu-id="8e236-580">A type `T` is considered to be *valid contravariantly* if:</span></span>

* <span data-ttu-id="8e236-581">`T` ist eine Klasse, Struktur oder enumerierten Typ an.</span><span class="sxs-lookup"><span data-stu-id="8e236-581">`T` is a class, structure, or enumerated type.</span></span>

* <span data-ttu-id="8e236-582">`T` ist eine nicht generische Delegaten oder dieser Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="8e236-582">`T` is a non-generic delegate or interface type.</span></span>

* <span data-ttu-id="8e236-583">`T` ist ein Arraytyp, dessen Elementtyp gültige kontravariant ist.</span><span class="sxs-lookup"><span data-stu-id="8e236-583">`T` is an array type whose element type is valid contravariantly.</span></span>

* <span data-ttu-id="8e236-584">`T` ist ein Typparameter, der nicht als Typ In-Parameter deklariert wurde.</span><span class="sxs-lookup"><span data-stu-id="8e236-584">`T` is a type parameter which was not declared as an In type parameter.</span></span>

* <span data-ttu-id="8e236-585">`T` wird von ein konstruierter Typ von Schnittstellen oder Delegate `X(Of P1,...,Pn)` mit Typargumenten `A1,...,An` so, dass:</span><span class="sxs-lookup"><span data-stu-id="8e236-585">`T` is a constructed interface or delegate type `X(Of P1,...,Pn)` with type arguments `A1,...,An` such that:</span></span>

  * <span data-ttu-id="8e236-586">Wenn `Pi` deklariert wurde, als ein `Out` Typparameter dann `Ai` gültige kontravariant ist.</span><span class="sxs-lookup"><span data-stu-id="8e236-586">If `Pi` was declared as an `Out` type parameter then `Ai` is valid contravariantly.</span></span>

  * <span data-ttu-id="8e236-587">Wenn `Pi` deklariert wurde, als ein `In` Typparameter dann `Ai` kovariant ist gültig.</span><span class="sxs-lookup"><span data-stu-id="8e236-587">If `Pi` was declared as an `In` type parameter then `Ai` is valid covariantly.</span></span>

<span data-ttu-id="8e236-588">Folgendes muss gültige kontravariant in einer Schnittstellen oder Delegate-Typ sein:</span><span class="sxs-lookup"><span data-stu-id="8e236-588">The following must be valid contravariantly in an interface or delegate type:</span></span>

* <span data-ttu-id="8e236-589">Der Typ eines Parameters.</span><span class="sxs-lookup"><span data-stu-id="8e236-589">The type of a parameter.</span></span>

* <span data-ttu-id="8e236-590">Eine Einschränkung für einen auf die Typparameter einer Methode.</span><span class="sxs-lookup"><span data-stu-id="8e236-590">A type constraint on a method type parameter.</span></span>

* <span data-ttu-id="8e236-591">Der Typ einer Eigenschaft, wenn sie verfügt über eine `Set` Accessor.</span><span class="sxs-lookup"><span data-stu-id="8e236-591">The type of a property if it has a `Set` accessor.</span></span>

* <span data-ttu-id="8e236-592">Der Typ eines Ereignisses.</span><span class="sxs-lookup"><span data-stu-id="8e236-592">The type of an event.</span></span>

<span data-ttu-id="8e236-593">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e236-593">For example:</span></span>

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

<span data-ttu-id="8e236-594">Werden Sie in dem Fall, in denen ein Typ muss gültig sein, kontravariant und kovariant (z. B. eine Eigenschaft mit einem `Get` und `Set` Accessor oder ein `ByRef` Parameter), ein Varianten Typparameter kann nicht verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8e236-594">In the case where a type must be valid be contravariantly and covariantly (such as a property with both a `Get` and `Set` accessor or a `ByRef` parameter), a variant type parameter cannot be used.</span></span>


<span data-ttu-id="8e236-595">CO - und -Kontravarianz bieten Anstieg zu einem Problem"Mehrdeutigkeit Raute".</span><span class="sxs-lookup"><span data-stu-id="8e236-595">Co- and contra-variance give rise to a "diamond ambiguity problem".</span></span> <span data-ttu-id="8e236-596">Betrachten Sie folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="8e236-596">Consider the following code:</span></span>

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

<span data-ttu-id="8e236-597">Die Klasse `C` konvertiert werden kann, um `IEnumerable(Of Object)` in zwei Möglichkeiten, die beide über kovariante Konvertierung von `IEnumerable(Of String)` und über kovariante Konvertierung von `IEnumerable(Of Exception)`.</span><span class="sxs-lookup"><span data-stu-id="8e236-597">The class `C` can be converted to `IEnumerable(Of Object)` in two ways, both through covariant conversion from `IEnumerable(Of String)` and through covariant conversion from `IEnumerable(Of Exception)`.</span></span> <span data-ttu-id="8e236-598">Die CLR gibt nicht an die der beiden Methoden aufgerufen wird, `c.GetEnumerator()`.</span><span class="sxs-lookup"><span data-stu-id="8e236-598">The CLR does not specify which of the two methods will be called by `c.GetEnumerator()`.</span></span> <span data-ttu-id="8e236-599">In der Regel jedes Mal, wenn eine Klasse wird deklariert, um eine kovariante Schnittstelle mit zwei generischen Argumenten zu implementieren, die einen gemeinsamen Obertyp aufweisen (z. B. in diesem Fall `String` und `Exception` haben Sie die gemeinsamen Obertyp `Object`), oder eine Klasse deklariert ist implementieren eine kontravariante Schnittstelle mit zwei generischen Argumenten, die einen allgemeinen Untertyp aufweisen, dann ist Mehrdeutigkeiten auftreten können.</span><span class="sxs-lookup"><span data-stu-id="8e236-599">In general, whenever a class is declared to implement a covariant interface with two different generic arguments that have a common supertype (e.g. in this case `String` and `Exception` have the common supertype `Object`), or a class is declared to implement a contravariant interface with two different generic arguments that have a common subtype, then ambiguity is likely to arise.</span></span> <span data-ttu-id="8e236-600">Der Compiler gibt eine Warnung für solche Deklarationen.</span><span class="sxs-lookup"><span data-stu-id="8e236-600">The compiler gives a warning on such declarations.</span></span>
