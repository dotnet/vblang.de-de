---
ms.openlocfilehash: 8599ffc0ad3313f4e7e42da220c22ffa7a37e56b
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306181"
---
# <a name="general-concepts"></a><span data-ttu-id="0155f-101">Allgemeine Konzepte</span><span class="sxs-lookup"><span data-stu-id="0155f-101">General Concepts</span></span>

<span data-ttu-id="0155f-102">In diesem Kapitel werden einige Konzepte behandelt, die erforderlich sind, um die Semantik der Sprache von Microsoft Visual Basic zu verstehen.</span><span class="sxs-lookup"><span data-stu-id="0155f-102">This chapter covers a number of concepts that are required to understand the semantics of the Microsoft Visual Basic language.</span></span> <span data-ttu-id="0155f-103">Viele der Konzepte sollten Visual Basic Programmierern oder C/C++ Programmierern vertraut sein, Ihre genauen Definitionen können sich jedoch unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="0155f-103">Many of the concepts should be familiar to Visual Basic programmers or C/C++ programmers, but their precise definitions may differ.</span></span>

## <a name="declarations"></a><span data-ttu-id="0155f-104">Deklarationen</span><span class="sxs-lookup"><span data-stu-id="0155f-104">Declarations</span></span>

<span data-ttu-id="0155f-105">Ein Visual Basic Programm besteht aus benannten Entitäten.</span><span class="sxs-lookup"><span data-stu-id="0155f-105">A Visual Basic program is made up of named entities.</span></span> <span data-ttu-id="0155f-106">Diese Entitäten werden durch *Deklarationen* eingeführt und stellen die "Bedeutung" des Programms dar.</span><span class="sxs-lookup"><span data-stu-id="0155f-106">These entities are introduced through *declarations* and represent the "meaning" of the program.</span></span>

<span data-ttu-id="0155f-107">Auf oberster Ebene sind *Namespaces* Entitäten, die andere Entitäten organisieren, wie z. b. schsted Namespaces und Typen.</span><span class="sxs-lookup"><span data-stu-id="0155f-107">At a top level, *namespaces* are entities that organize other entities, such as nested namespaces and types.</span></span> <span data-ttu-id="0155f-108">*Typen* sind Entitäten, die Werte beschreiben und ausführbaren Code definieren.</span><span class="sxs-lookup"><span data-stu-id="0155f-108">*Types* are entities that describe values and define executable code.</span></span> <span data-ttu-id="0155f-109">Typen können in Form von Typen und Typmembern enthalten.</span><span class="sxs-lookup"><span data-stu-id="0155f-109">Types may contain nested types and type members.</span></span> <span data-ttu-id="0155f-110">*Typmember* sind Konstanten, Variablen, Methoden, Operatoren, Eigenschaften, Ereignisse, Enumerationswerte und Konstruktoren.</span><span class="sxs-lookup"><span data-stu-id="0155f-110">*Type members* are constants, variables, methods, operators, properties, events, enumeration values, and constructors.</span></span>

<span data-ttu-id="0155f-111">Eine Entität, die andere Entitäten enthalten kann, definiert einen *Deklarations Bereich*.</span><span class="sxs-lookup"><span data-stu-id="0155f-111">An entity that can contain other entities defines a *declaration space*.</span></span> <span data-ttu-id="0155f-112">Entitäten werden entweder über Deklarationen oder Vererbung in einem Deklarations Raum eingeführt. der enthaltende Deklarations Bereich wird als *Deklarations Kontext*der Entität bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="0155f-112">Entities are introduced into a declaration space either through declarations or inheritance; the containing declaration space is called the entities' *declaration context*.</span></span> <span data-ttu-id="0155f-113">Durch das Deklarieren einer Entität in einem Deklarations Bereich wird wiederum ein neuer Deklarations Bereich definiert, der weitere geschsted Entitäts Deklarationen enthalten kann. Folglich bilden die Deklarationen in einem Programm eine Hierarchie von Deklarations Räumen.</span><span class="sxs-lookup"><span data-stu-id="0155f-113">Declaring an entity in a declaration space in turn defines a new declaration space that can contain further nested entity declarations; thus, the declarations in a program form a hierarchy of declaration spaces.</span></span>

<span data-ttu-id="0155f-114">Außer im Fall von überladenen Typmembern ist es ungültig, dass Deklarationen identisch benannte Entitäten derselben Art in denselben Deklarations Kontext einführen.</span><span class="sxs-lookup"><span data-stu-id="0155f-114">Except in the case of overloaded type members, it is invalid for declarations to introduce identically named entities of the same kind into the same declaration context.</span></span> <span data-ttu-id="0155f-115">Außerdem darf ein Deklarations Raum nie verschiedene Arten von Entitäten mit demselben Namen enthalten. Beispielsweise darf ein Deklarations Raum nie eine Variable und eine Methode mit demselben Namen enthalten.</span><span class="sxs-lookup"><span data-stu-id="0155f-115">Additionally, a declaration space may never contain different kinds of entities with the same name; for example, a declaration space may never contain a variable and a method by the same name.</span></span>

<span data-ttu-id="0155f-116">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="0155f-116">__Note.__</span></span> <span data-ttu-id="0155f-117">In anderen Sprachen kann es möglich sein, einen Deklarations Bereich zu erstellen, der verschiedene Arten von Entitäten mit demselben Namen enthält (z. b. Wenn bei der Sprache die Groß-/Kleinschreibung beachtet wird und verschiedene Deklarationen basierend auf der Groß-/Kleinschreibung</span><span class="sxs-lookup"><span data-stu-id="0155f-117">It may be possible in other languages to create a declaration space that contains different kinds of entities with the same name (for example, if the language is case sensitive and allows different declarations based on casing).</span></span> <span data-ttu-id="0155f-118">In dieser Situation wird die am meisten zugängliche Entität an diesen Namen gebunden. Wenn mehr als ein Entitätstyp am meisten zugänglich ist, ist der Name mehrdeutig.</span><span class="sxs-lookup"><span data-stu-id="0155f-118">In that situation, the most accessible entity is considered bound to that name; if more than one type of entity is most accessible then the name is ambiguous.</span></span> <span data-ttu-id="0155f-119">`Public` ist besser zugänglich als `Protected Friend`, `Protected Friend` ist besser zugänglich als `Protected` oder `Friend`, und `Protected` oder `Friend` ist besser zugänglich als `Private`.</span><span class="sxs-lookup"><span data-stu-id="0155f-119">`Public` is more accessible than `Protected Friend`, `Protected Friend` is more accessible than `Protected` or `Friend`, and `Protected` or `Friend` is more accessible than `Private`.</span></span>

<span data-ttu-id="0155f-120">Der Deklarations Raum eines Namespaces ist "Open End", sodass zwei Namespace Deklarationen mit demselben voll qualifizierten Namen zum selben Deklarations Bereich beitragen.</span><span class="sxs-lookup"><span data-stu-id="0155f-120">The declaration space of a namespace is "open ended," so two namespace declarations with the same fully qualified name contribute to the same declaration space.</span></span> <span data-ttu-id="0155f-121">Im folgenden Beispiel tragen die beiden Namespace Deklarationen zum selben Deklarations Bereich bei, in diesem Fall werden zwei Klassen mit den voll qualifizierten Namen `Data.Customer` und `Data.Order:`</span><span class="sxs-lookup"><span data-stu-id="0155f-121">In the example below, the two namespace declarations contribute to the same declaration space, in this case declaring two classes with the fully qualified names `Data.Customer` and `Data.Order:`</span></span>

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

<span data-ttu-id="0155f-122">Da die beiden Deklarationen zum gleichen Deklarations Bereich beitragen, tritt ein Kompilierzeitfehler auf, wenn jeder eine Deklaration einer Klasse mit dem gleichen Namen enthielt.</span><span class="sxs-lookup"><span data-stu-id="0155f-122">Because the two declarations contribute to the same declaration space, a compile-time error would occur if each contained a declaration of a class with the same name.</span></span>

### <a name="overloading-and-signatures"></a><span data-ttu-id="0155f-123">Überladen und Signaturen</span><span class="sxs-lookup"><span data-stu-id="0155f-123">Overloading and Signatures</span></span>

<span data-ttu-id="0155f-124">Die einzige Möglichkeit, identisch benannte Entitäten derselben Art in einem Deklarations Bereich zu deklarieren, ist das *überladen*.</span><span class="sxs-lookup"><span data-stu-id="0155f-124">The only way to declare identically named entities of the same kind in a declaration space is through *overloading*.</span></span> <span data-ttu-id="0155f-125">Nur Methoden, Operatoren, Instanzkonstruktoren und Eigenschaften können überladen werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-125">Only methods, operators, instance constructors, and properties may be overloaded.</span></span>

<span data-ttu-id="0155f-126">Überladene Typmember müssen eindeutige Signaturen aufweisen.</span><span class="sxs-lookup"><span data-stu-id="0155f-126">Overloaded type members must possess unique signatures.</span></span> <span data-ttu-id="0155f-127">Die Signatur eines Typmembers besteht aus der Anzahl der Typparameter und der Anzahl und den Typen der Parameter des Elements.</span><span class="sxs-lookup"><span data-stu-id="0155f-127">The signature of a type member consists of the number of type parameters, and the number and types of the member's parameters.</span></span> <span data-ttu-id="0155f-128">Konvertierungs Operatoren enthalten außerdem den Rückgabetyp des Operators in der Signatur.</span><span class="sxs-lookup"><span data-stu-id="0155f-128">Conversion operators also include the return type of the operator in the signature.</span></span>

<span data-ttu-id="0155f-129">Die folgenden Elemente sind nicht Teil der Signatur eines Members und können daher nicht überladen werden:</span><span class="sxs-lookup"><span data-stu-id="0155f-129">The following are not part of a member's signature, and hence cannot be overloaded on:</span></span>

* <span data-ttu-id="0155f-130">Modifiziererer in einen Typmember (z. b. `Shared` oder `Private`).</span><span class="sxs-lookup"><span data-stu-id="0155f-130">Modifiers to a type member (for example, `Shared` or `Private`).</span></span>

* <span data-ttu-id="0155f-131">Modifizierer in einen Parameter (z. b. `ByVal` oder `ByRef`).</span><span class="sxs-lookup"><span data-stu-id="0155f-131">Modifiers to a parameter (for example, `ByVal` or `ByRef`).</span></span>

* <span data-ttu-id="0155f-132">Die Namen der Parameter.</span><span class="sxs-lookup"><span data-stu-id="0155f-132">The names of the parameters.</span></span>

* <span data-ttu-id="0155f-133">Der Rückgabetyp einer Methode oder eines Operators (mit Ausnahme von Konvertierungs Operatoren) oder der Elementtyp einer Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="0155f-133">The return type of a method or operator (except for conversion operators) or the element type of a property.</span></span>

* <span data-ttu-id="0155f-134">Einschränkungen für einen Typparameter.</span><span class="sxs-lookup"><span data-stu-id="0155f-134">Constraints on a type parameter.</span></span>

<span data-ttu-id="0155f-135">Das folgende Beispiel zeigt eine Reihe von überladenen Methoden Deklarationen zusammen mit ihren Signaturen.</span><span class="sxs-lookup"><span data-stu-id="0155f-135">The following example shows a set of overloaded method declarations along with their signatures.</span></span> <span data-ttu-id="0155f-136">Diese Deklaration ist nicht gültig, da einige der Methoden Deklarationen identische Signaturen aufweisen.</span><span class="sxs-lookup"><span data-stu-id="0155f-136">This declaration would not be valid since several of the method declarations have identical signatures.</span></span>

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

<span data-ttu-id="0155f-137">Es ist zulässig, einen generischen Typ zu definieren, der auf der Grundlage der angegebenen Typargumente Member mit identischen Signaturen enthalten kann.</span><span class="sxs-lookup"><span data-stu-id="0155f-137">It is valid to define a generic type that may contain members with identical signatures based on the type arguments supplied.</span></span> <span data-ttu-id="0155f-138">Regeln zur Überladungs Auflösung werden verwendet, um zu versuchen, zwischen solchen über Ladungen zu unterscheiden. es kann jedoch Situationen geben, in denen es nicht möglich ist, die Mehrdeutigkeit zu beheben.</span><span class="sxs-lookup"><span data-stu-id="0155f-138">Overload resolution rules are used to try and disambiguate between such overloads, although there may be situations in which it is impossible to disambiguate.</span></span> <span data-ttu-id="0155f-139">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-139">For example:</span></span>

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

## <a name="scope"></a><span data-ttu-id="0155f-140">Gültigkeitsbereich</span><span class="sxs-lookup"><span data-stu-id="0155f-140">Scope</span></span>

<span data-ttu-id="0155f-141">Der Gültigkeits *Bereich* eines Entitäts namens ist der Satz aller Deklarations Räume, in denen es möglich ist, ohne Qualifizierung auf diesen Namen zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="0155f-141">The *scope* of an entity's name is the set of all declaration spaces within which it is possible to refer to that name without qualification.</span></span> <span data-ttu-id="0155f-142">Im Allgemeinen ist der Gültigkeitsbereich eines Entitäts namens der gesamte Deklarations Kontext. Allerdings kann die Deklaration einer Entität eine Entität mit dem gleichen Namen enthalten.</span><span class="sxs-lookup"><span data-stu-id="0155f-142">In general, the scope of an entity's name is its entire declaration context; however, an entity's declaration may contain nested declarations of entities with the same name.</span></span> <span data-ttu-id="0155f-143">In diesem Fall ist die *schattenentität mit der schattenentität*ausgeblendet oder ausgeblendet, und der Zugriff auf die Schatten Entität ist nur über die Qualifizierung möglich.</span><span class="sxs-lookup"><span data-stu-id="0155f-143">In that case, the nested entity *shadows*, or hides, the outer entity, and access to the shadowed entity is only possible through qualification.</span></span>

<span data-ttu-id="0155f-144">Die Schachtelung durch Schachtelung erfolgt in Namespaces oder Typen, die in Namespaces geschachtelt sind, in Typen, die in anderen Typen geschachtelt sind, und in den Texttypen.</span><span class="sxs-lookup"><span data-stu-id="0155f-144">Shadowing through nesting occurs in namespaces or types nested within namespaces, in types nested within other types, and in the bodies of type members.</span></span> <span data-ttu-id="0155f-145">Shadothrough der Schachtelung von Deklarationen tritt immer implizit auf; Es ist keine explizite Syntax erforderlich.</span><span class="sxs-lookup"><span data-stu-id="0155f-145">Shadowing through the nesting of declarations always occurs implicitly; no explicit syntax is required.</span></span>

<span data-ttu-id="0155f-146">Im folgenden Beispiel wird in der `F`-Methode die Instanzvariable `i` von der lokalen Variablen `i`schattiert, aber innerhalb der `G`-Methode verweist `i` weiterhin auf die-Instanzvariable.</span><span class="sxs-lookup"><span data-stu-id="0155f-146">In the following example, within the `F` method, the instance variable `i` is shadowed by the local variable `i`, but within the `G` method, `i` still refers to the instance variable.</span></span>

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

<span data-ttu-id="0155f-147">Wenn ein Name in einem inneren Gültigkeitsbereich einen Namen in einem äußeren Gültigkeitsbereich verbirgt, werden alle überladenen Vorkommen dieses Namens ausgeblendet.</span><span class="sxs-lookup"><span data-stu-id="0155f-147">When a name in an inner scope hides a name in an outer scope, it shadows all overloaded occurrences of that name.</span></span> <span data-ttu-id="0155f-148">Im folgenden Beispiel ruft der Aufruf `F(1)` die in `Inner` deklarierte `F` auf, da alle äußeren Vorkommen von `F` von der inneren Deklaration ausgeblendet werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-148">In the following example, the call `F(1)` invokes the `F` declared in `Inner` because all outer occurrences of `F` are hidden by the inner declaration.</span></span> <span data-ttu-id="0155f-149">Aus demselben Grund ist der `F("Hello")` Fehler aufgetreten.</span><span class="sxs-lookup"><span data-stu-id="0155f-149">For the same reason, the call `F("Hello")` is in error.</span></span>

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

## <a name="inheritance"></a><span data-ttu-id="0155f-150">Vererbung</span><span class="sxs-lookup"><span data-stu-id="0155f-150">Inheritance</span></span>

<span data-ttu-id="0155f-151">Eine Vererbungs Beziehung ist eine Vererbungs Beziehung, bei der ein Typ (der *abgeleitete* Typ) von einem anderen (dem *Basistyp* ) abgeleitet wird, sodass der Deklarations Bereich des abgeleiteten Typs implizit die zugänglichen Member des Typs "Non-Constructor" und die zugehörigen Typen des Basistyps enthält.</span><span class="sxs-lookup"><span data-stu-id="0155f-151">An inheritance relationship is one in which one type (the *derived* type) derives from another (the *base* type), such that the derived type's declaration space implicitly contains the accessible non-constructor type members and nested types of its base type.</span></span> <span data-ttu-id="0155f-152">Im folgenden Beispiel ist Class `A` die Basisklasse von `B`und `B` von `A`abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="0155f-152">In the following example, class `A` is the base class of `B`, and `B` is derived from `A`.</span></span>

```vb
Class A
End Class

Class B
    Inherits A
End Class
```

<span data-ttu-id="0155f-153">Da in `A` nicht explizit eine Basisklasse angegeben ist, wird die zugehörige Basisklasse implizit `Object`.</span><span class="sxs-lookup"><span data-stu-id="0155f-153">Since `A` does not explicitly specify a base class, its base class is implicitly `Object`.</span></span>

<span data-ttu-id="0155f-154">Im folgenden finden Sie wichtige Aspekte der Vererbung:</span><span class="sxs-lookup"><span data-stu-id="0155f-154">The following are important aspects of inheritance:</span></span>

* <span data-ttu-id="0155f-155">Vererbung ist transitiv.</span><span class="sxs-lookup"><span data-stu-id="0155f-155">Inheritance is transitive.</span></span> <span data-ttu-id="0155f-156">Wenn Typ *c* vom Typ *b*abgeleitet ist und Typ *b* vom Typ *a*abgeleitet ist, erbt Typ *c* die Typmember, die in Typ *b* deklariert sind, sowie die Typmember, die in Typ *a*deklariert wurden.</span><span class="sxs-lookup"><span data-stu-id="0155f-156">If type *C* is derived from type *B*, and type *B* is derived from type *A*, type *C* inherits the type members declared in type *B* as well as the type members declared in type *A*.</span></span>

* <span data-ttu-id="0155f-157">Ein abgeleiteter Typ erweitert seinen Basistyp, kann ihn aber nicht eingrenzen.</span><span class="sxs-lookup"><span data-stu-id="0155f-157">A derived type extends, but cannot narrow, its base type.</span></span> <span data-ttu-id="0155f-158">Ein abgeleiteter Typ kann neue Typmember hinzufügen, und er kann Schatten vererbte Typmember überschatten, aber er kann die Definition eines geerbten Typmembers nicht entfernen.</span><span class="sxs-lookup"><span data-stu-id="0155f-158">A derived type can add new type members, and it can shadow inherited type members, but it cannot remove the definition of an inherited type member.</span></span>

* <span data-ttu-id="0155f-159">Da eine Instanz eines Typs alle Typmember des Basistyps enthält, ist immer eine Konvertierung von einem abgeleiteten Typ in den Basistyp vorhanden.</span><span class="sxs-lookup"><span data-stu-id="0155f-159">Because an instance of a type contains all of the type members of its base type, a conversion always exists from a derived type to its base type.</span></span>

* <span data-ttu-id="0155f-160">Alle Typen müssen über einen Basistyp verfügen, mit Ausnahme des Typs `Object`.</span><span class="sxs-lookup"><span data-stu-id="0155f-160">All types must have a base type, except for the type `Object`.</span></span> <span data-ttu-id="0155f-161">Daher ist `Object` der ultimative Basistyp aller Typen, und alle Typen können in diese konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-161">Thus, `Object` is the ultimate base type of all types, and all types can be converted to it.</span></span>

* <span data-ttu-id="0155f-162">Zirkularität bei der Ableitung ist nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="0155f-162">Circularity in derivation is not permitted.</span></span> <span data-ttu-id="0155f-163">Das heißt, wenn ein Typ `B` von einem Typ `A`abgeleitet ist, ist dies ein Fehler für den Typ `A`, der direkt oder indirekt vom Typ `B`abgeleitet werden soll.</span><span class="sxs-lookup"><span data-stu-id="0155f-163">That is, when a type `B` derives from a type `A`, it is an error for type `A` to derive directly or indirectly from type `B`.</span></span>

* <span data-ttu-id="0155f-164">Ein Typ kann nicht direkt oder indirekt von einem darin geschachtelten Typ abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-164">A type may not directly or indirectly derive from a type nested within it.</span></span>

<span data-ttu-id="0155f-165">Im folgenden Beispiel wird ein Kompilierzeitfehler erzeugt, da die Klassen zirkulär voneinander abhängig sind.</span><span class="sxs-lookup"><span data-stu-id="0155f-165">The following example produces a compile-time error because the classes circularly depend on each other.</span></span>

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

<span data-ttu-id="0155f-166">Im folgenden Beispiel wird auch ein Kompilierzeitfehler erzeugt, da `B` indirekt von der zugehörigen `C` Klasse abgeleitet wird, die durch die Klasse `A`wird.</span><span class="sxs-lookup"><span data-stu-id="0155f-166">The following example also produces a compile-time error because `B` indirectly derives from its nested class `C` through class `A`.</span></span>

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

<span data-ttu-id="0155f-167">Im nächsten Beispiel wird kein Fehler erzeugt, da die Klasse `A` nicht von der Klasse `B`abgeleitet ist.</span><span class="sxs-lookup"><span data-stu-id="0155f-167">The next example does not produce an error because class `A` does not derive from class `B`.</span></span>

```vb
Class A
    Class B
        Inherits A
    End Class 
End Class
```

### <a name="mustinherit-and-notinheritable-classes"></a><span data-ttu-id="0155f-168">MustInherit-und notvererable-Klassen</span><span class="sxs-lookup"><span data-stu-id="0155f-168">MustInherit and NotInheritable Classes</span></span>

<span data-ttu-id="0155f-169">Eine `MustInherit` Klasse ist ein unvollständiger Typ, der nur als Basistyp fungieren kann.</span><span class="sxs-lookup"><span data-stu-id="0155f-169">A `MustInherit` class is an incomplete type that can act only as a base type.</span></span> <span data-ttu-id="0155f-170">Eine `MustInherit` Klasse kann nicht instanziiert werden, daher ist es ein Fehler, den `New` Operator für einen zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="0155f-170">A `MustInherit` class cannot be instantiated, so it is an error to use the `New` operator on one.</span></span> <span data-ttu-id="0155f-171">Es ist zulässig, Variablen von `MustInherit` Klassen zu deklarieren. Diese Variablen können nur `Nothing` oder einem Wert zugewiesen werden, der von der Klasse `MustInherit` abgeleitet ist.</span><span class="sxs-lookup"><span data-stu-id="0155f-171">It is valid to declare variables of `MustInherit` classes; such variables can only be assigned `Nothing` or a value that is of a class derived from the `MustInherit` class.</span></span>

<span data-ttu-id="0155f-172">Wenn eine reguläre Klasse von einer `MustInherit` Klasse abgeleitet wird, muss die reguläre Klasse alle geerbten `MustOverride` Member überschreiben.</span><span class="sxs-lookup"><span data-stu-id="0155f-172">When a regular class is derived from a `MustInherit` class, the regular class must override all inherited `MustOverride` members.</span></span> <span data-ttu-id="0155f-173">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-173">For example:</span></span>

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

<span data-ttu-id="0155f-174">Mit der `MustInherit`-Klasse `A` wird eine `MustOverride`-Methode `F`eingeführt.</span><span class="sxs-lookup"><span data-stu-id="0155f-174">The `MustInherit` class `A` introduces a `MustOverride` method `F`.</span></span> <span data-ttu-id="0155f-175">In Class `B` wird eine zusätzliche Methode `G`eingeführt, es wird jedoch keine Implementierung von `F`bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="0155f-175">Class `B` introduces an additional method `G`, but does not provide an implementation of `F`.</span></span> <span data-ttu-id="0155f-176">Klassen `B` müssen daher auch als `MustInherit`deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-176">Class `B` must therefore also be declared `MustInherit`.</span></span> <span data-ttu-id="0155f-177">Class `C` überschreibt `F` und stellt eine tatsächliche Implementierung bereit.</span><span class="sxs-lookup"><span data-stu-id="0155f-177">Class `C` overrides `F` and provides an actual implementation.</span></span> <span data-ttu-id="0155f-178">Da keine ausstehenden `MustOverride` Member in der Klasse `C`vorhanden sind, müssen Sie nicht `MustInherit`werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-178">Since there are no outstanding `MustOverride` members in class `C`, it is not required to be `MustInherit`.</span></span>

<span data-ttu-id="0155f-179">Eine `NotInheritable` Klasse ist eine Klasse, von der eine andere Klasse nicht abgeleitet werden kann.</span><span class="sxs-lookup"><span data-stu-id="0155f-179">A `NotInheritable` class is a class from which another class cannot be derived.</span></span> <span data-ttu-id="0155f-180">`NotInheritable` Klassen werden hauptsächlich verwendet, um eine unbeabsichtigte Ableitung zu verhindern.</span><span class="sxs-lookup"><span data-stu-id="0155f-180">`NotInheritable` classes are primarily used to prevent unintended derivation.</span></span>

<span data-ttu-id="0155f-181">In diesem Beispiel ist die Klasse `B` fehlerhaft, da Sie versucht, von der `NotInheritable` Klasse `A`abzuleiten.</span><span class="sxs-lookup"><span data-stu-id="0155f-181">In this example, class `B` is in error because it attempts to derive from the `NotInheritable` class `A`.</span></span> <span data-ttu-id="0155f-182">Eine Klasse kann nicht sowohl `MustInherit` als auch `NotInheritable`gekennzeichnet werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-182">A class cannot be marked both `MustInherit` and `NotInheritable`.</span></span>

```vb
NotInheritable Class A
End Class

Class B
    ' Error, a class cannot derive from a NotInheritable class.
    Inherits A
End Class
```

### <a name="interfaces-and-multiple-inheritance"></a><span data-ttu-id="0155f-183">Schnittstellen und mehrfache Vererbung</span><span class="sxs-lookup"><span data-stu-id="0155f-183">Interfaces and Multiple Inheritance</span></span>

<span data-ttu-id="0155f-184">Im Gegensatz zu anderen Typen, die nur von einem einzelnen Basistyp abgeleitet werden, kann eine Schnittstelle von mehreren Basis Schnittstellen abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-184">Unlike other types, which only derive from a single base type, an interface may derive from multiple base interfaces.</span></span> <span data-ttu-id="0155f-185">Aus diesem Grund kann eine Schnittstelle ein identisch benanntes Typmember von unterschiedlichen Basis Schnittstellen erben.</span><span class="sxs-lookup"><span data-stu-id="0155f-185">Because of this, an interface can inherit an identically named type member from different base interfaces.</span></span> <span data-ttu-id="0155f-186">In einem solchen Fall ist der mehrfach geerbte Name in der abgeleiteten Schnittstelle nicht verfügbar, und der Verweis auf diese Typmember durch die abgeleitete Schnittstelle verursacht einen Kompilierzeitfehler, unabhängig von Signaturen oder überladen.</span><span class="sxs-lookup"><span data-stu-id="0155f-186">In such a case, the multiply-inherited name is not available in the derived interface, and referring to any of those type members through the derived interface causes a compile-time error, regardless of signatures or overloading.</span></span> <span data-ttu-id="0155f-187">Stattdessen müssen in Konflikt stehende Typmember durch einen Basis Schnittstellennamen referenziert werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-187">Instead, conflicting type members must be referenced through a base interface name.</span></span>

<span data-ttu-id="0155f-188">Im folgenden Beispiel verursachen die ersten beiden-Anweisungen Kompilierzeitfehler, da die `Count` mit multiplizierter Vererbung in der-Schnittstelle `IListCounter`nicht verfügbar ist:</span><span class="sxs-lookup"><span data-stu-id="0155f-188">In the following example, the first two statements cause compile-time errors because the multiply-inherited member `Count` is not available in interface `IListCounter`:</span></span>

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

<span data-ttu-id="0155f-189">Wie im Beispiel gezeigt, wird die Mehrdeutigkeit durch umwandeln `x` in den entsprechenden Basis Schnittstellentyp aufgelöst.</span><span class="sxs-lookup"><span data-stu-id="0155f-189">As illustrated by the example, the ambiguity is resolved by casting `x` to the appropriate base interface type.</span></span> <span data-ttu-id="0155f-190">Solche Umwandlungen haben keine Lauf Zeit Kosten. Sie bestehen lediglich aus dem Anzeigen der Instanz als weniger abgeleiteter Typ zur Kompilierzeit.</span><span class="sxs-lookup"><span data-stu-id="0155f-190">Such casts have no run-time costs; they merely consist of viewing the instance as a less-derived type at compile time.</span></span>

<span data-ttu-id="0155f-191">Wenn ein einzelner Typmember von derselben Basisschnittstelle durch mehrere Pfade geerbt wird, wird der Typmember so behandelt, als wäre er nur einmal geerbt worden.</span><span class="sxs-lookup"><span data-stu-id="0155f-191">When a single type member is inherited from the same base interface through multiple paths, the type member is treated as if it were only inherited once.</span></span> <span data-ttu-id="0155f-192">Anders ausgedrückt: die abgeleitete Schnittstelle enthält nur eine Instanz jedes Typmembers, der von einer bestimmten Basisschnittstelle geerbt wurde.</span><span class="sxs-lookup"><span data-stu-id="0155f-192">In other words, the derived interface only contains one instance of each type member inherited from a particular base interface.</span></span> <span data-ttu-id="0155f-193">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-193">For example:</span></span>

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

<span data-ttu-id="0155f-194">Wenn ein Typmember-Name in einem Pfad über die Vererbungs Hierarchie schattiert wird, wird der Name in allen Pfaden schattiert.</span><span class="sxs-lookup"><span data-stu-id="0155f-194">If a type member name is shadowed in one path through the inheritance hierarchy, then the name is shadowed in all paths.</span></span> <span data-ttu-id="0155f-195">Im folgenden Beispiel wird der `IBase.F` Member vom `ILeft.F` Member schattiert, aber nicht in `IRight`schattiert:</span><span class="sxs-lookup"><span data-stu-id="0155f-195">In the following example, the `IBase.F` member is shadowed by the `ILeft.F` member, but is not shadowed in `IRight`:</span></span>

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

<span data-ttu-id="0155f-196">Der Aufruf `d.F(1)` wählt `ILeft.F`aus, obwohl `IBase.F` anscheinend nicht in den Zugriffs Pfad schattiert wird, der durch `IRight`führt.</span><span class="sxs-lookup"><span data-stu-id="0155f-196">The invocation `d.F(1)` selects `ILeft.F`, even though `IBase.F` appears to not be shadowed in the access path that leads through `IRight`.</span></span> <span data-ttu-id="0155f-197">Da der Zugriffs Pfad von `IDerived` auf `ILeft`, um Schatten `IBase.F`zu `IBase`, wird das Element auch in den Zugriffs Pfad von `IDerived` auf `IRight` `IBase`.</span><span class="sxs-lookup"><span data-stu-id="0155f-197">Because the access path from `IDerived` to `ILeft` to `IBase` shadows `IBase.F`, the member is also shadowed in the access path from `IDerived` to `IRight` to `IBase`.</span></span>

### <a name="shadowing"></a><span data-ttu-id="0155f-198">Shadowing</span><span class="sxs-lookup"><span data-stu-id="0155f-198">Shadowing</span></span>

<span data-ttu-id="0155f-199">Ein abgeleiteter Typ überschattet den Namen eines geerbten Typmembers durch erneutes deklarieren.</span><span class="sxs-lookup"><span data-stu-id="0155f-199">A derived type shadows the name of an inherited type member by re-declaring it.</span></span> <span data-ttu-id="0155f-200">Beim shadolieren eines Namens werden die geerbten Typmember mit diesem Namen nicht entfernt. Es macht lediglich alle geerbten Typmember mit diesem Namen in der abgeleiteten Klasse nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="0155f-200">Shadowing a name does not remove the inherited type members with that name; it merely makes all of the inherited type members with that name unavailable in the derived class.</span></span> <span data-ttu-id="0155f-201">Die Shadowing-Deklaration kann ein beliebiger Entitätstyp sein.</span><span class="sxs-lookup"><span data-stu-id="0155f-201">The shadowing declaration may be any type of entity.</span></span>

<span data-ttu-id="0155f-202">Entitäten, die überladen werden können, können eine von zwei Formen der shadodown-Option auswählen.</span><span class="sxs-lookup"><span data-stu-id="0155f-202">Entities than can be overloaded can choose one of two forms of shadowing.</span></span> <span data-ttu-id="0155f-203">*Shadowingby Name* wird mit dem `Shadows`-Schlüsselwort angegeben.</span><span class="sxs-lookup"><span data-stu-id="0155f-203">*Shadowing by name* is specified using the `Shadows` keyword.</span></span> <span data-ttu-id="0155f-204">Eine Entität, die nach Name Shadowing durch den Namen in der Basisklasse verbirgt, einschließlich aller über Ladungen.</span><span class="sxs-lookup"><span data-stu-id="0155f-204">An entity that shadows by name hides everything by that name in the base class, including all overloads.</span></span> <span data-ttu-id="0155f-205">*Shadowingby Name und Signature werden* mithilfe des `Overloads`-Schlüssel Worts angegeben.</span><span class="sxs-lookup"><span data-stu-id="0155f-205">*Shadowing by name and signature* is specified using the `Overloads` keyword.</span></span> <span data-ttu-id="0155f-206">Eine Entität, die durch den Namen und die Signatur Shadowing durch diesen Namen mit derselben Signatur wie die Entität verbirgt.</span><span class="sxs-lookup"><span data-stu-id="0155f-206">An entity that shadows by name and signature hides everything by that name with the same signature as the entity.</span></span> <span data-ttu-id="0155f-207">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-207">For example:</span></span>

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

<span data-ttu-id="0155f-208">Beim shadoochen einer Methode mit einem `ParamArray`-Argument nach Name und Signatur werden nur die einzelnen Signaturen ausgeblendet, nicht alle möglichen erweiterten Signaturen.</span><span class="sxs-lookup"><span data-stu-id="0155f-208">Shadowing a method with a `ParamArray` argument by name and signature hides only the individual signature, not all possible expanded signatures.</span></span> <span data-ttu-id="0155f-209">Dies gilt auch, wenn die Signatur der Shadowing-Methode mit der nicht erweiterten Signatur der Shadowing-Methode übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="0155f-209">This is true even if the signature of the shadowing method matches the unexpanded signature of the shadowed method.</span></span> <span data-ttu-id="0155f-210">Im Beispiel unten geschieht Folgendes:</span><span class="sxs-lookup"><span data-stu-id="0155f-210">The following example:</span></span>

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

<span data-ttu-id="0155f-211">gibt `Base`aus, auch wenn `Derived.F` die gleiche Signatur aufweist wie die nicht erweiterte Form von `Base.F`.</span><span class="sxs-lookup"><span data-stu-id="0155f-211">prints `Base`, even though `Derived.F` has the same signature as the unexpanded form of `Base.F`.</span></span>

<span data-ttu-id="0155f-212">Umgekehrt gibt eine Methode mit einem `ParamArray`-Argument nur Methoden mit derselben Signatur, nicht alle möglichen erweiterten Signaturen.</span><span class="sxs-lookup"><span data-stu-id="0155f-212">Conversely, a method with a `ParamArray` argument only shadows methods with the same signature, not all possible expanded signatures.</span></span> <span data-ttu-id="0155f-213">Im Beispiel unten geschieht Folgendes:</span><span class="sxs-lookup"><span data-stu-id="0155f-213">The following example:</span></span>

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

<span data-ttu-id="0155f-214">gibt `Base`aus, auch wenn `Derived.F` ein erweitertes Formular hat, das über dieselbe Signatur wie `Base.F`verfügt.</span><span class="sxs-lookup"><span data-stu-id="0155f-214">prints `Base`, even though `Derived.F` has an expanded form that has the same signature as `Base.F`.</span></span>

<span data-ttu-id="0155f-215">Eine shadowingmethode oder Eigenschaft, die weder `Shadows` noch `Overloads` angibt, `Overloads` `Shadows`, wenn die Methode oder Eigenschaft als `Overrides`deklariert ist, andernfalls.</span><span class="sxs-lookup"><span data-stu-id="0155f-215">A shadowing method or property that does not specify `Shadows` or `Overloads` assumes `Overloads` if the method or property is declared `Overrides`, `Shadows` otherwise.</span></span> <span data-ttu-id="0155f-216">Wenn ein Element eines Satzes von überladenen Entitäten die `Shadows` oder `Overloads`-Schlüsselwort angibt, müssen alle diese angeben.</span><span class="sxs-lookup"><span data-stu-id="0155f-216">If one member of a set of overloaded entities specifies the `Shadows` or `Overloads` keyword, they all must specify it.</span></span> <span data-ttu-id="0155f-217">Die Schlüsselwörter "`Shadows`" und "`Overloads`" können nicht gleichzeitig angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-217">The `Shadows` and `Overloads` keywords cannot be specified at the same time.</span></span> <span data-ttu-id="0155f-218">Weder `Shadows` noch `Overloads` können in einem Standardmodul angegeben werden. Member in einem Standardmodul schattenelemente, die von `Object`geerbt wurden, implizit.</span><span class="sxs-lookup"><span data-stu-id="0155f-218">Neither `Shadows` nor `Overloads` can be specified in a standard module; members in a standard module implicitly shadow members inherited from `Object`.</span></span>

<span data-ttu-id="0155f-219">Es ist zulässig, den Namen eines Typmembers zu schattieren, der durch die Schnittstellen Vererbung (und somit nicht verfügbar ist) multipliziert-geerbt wurde, sodass der Name in der abgeleiteten Schnittstelle verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="0155f-219">It is valid to shadow the name of a type member that has been multiply-inherited through interface inheritance (and which is thereby unavailable), thus making the name available in the derived interface.</span></span>

<span data-ttu-id="0155f-220">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-220">For example:</span></span>

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

<span data-ttu-id="0155f-221">Da Methoden zum überschatten geerbte Methoden zugelassen werden, kann eine Klasse mehrere `Overridable` Methoden mit der gleichen Signatur enthalten.</span><span class="sxs-lookup"><span data-stu-id="0155f-221">Because methods are allowed to shadow inherited methods, it is possible for a class to contain several `Overridable` methods with the same signature.</span></span> <span data-ttu-id="0155f-222">Dies stellt kein mehrdeutigkeitsproblem dar, da nur die am meisten abgeleitete Methode sichtbar ist.</span><span class="sxs-lookup"><span data-stu-id="0155f-222">This does not present an ambiguity problem, since only the most-derived method is visible.</span></span> <span data-ttu-id="0155f-223">Im folgenden Beispiel enthalten die Klassen `C` und `D` zwei `Overridable` Methoden mit der gleichen Signatur:</span><span class="sxs-lookup"><span data-stu-id="0155f-223">In the following example, the `C` and `D` classes contain two `Overridable` methods with the same signature:</span></span>

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

<span data-ttu-id="0155f-224">Hier gibt es zwei `Overridable` Methoden: eine Klasse, die von Class `A` eingeführt wurde, und die von Class `C`eingeführte.</span><span class="sxs-lookup"><span data-stu-id="0155f-224">There are two `Overridable` methods here: one introduced by class `A` and the one introduced by class `C`.</span></span> <span data-ttu-id="0155f-225">Die von Class eingeführte Methode `C` blendet die Methode aus, die von der Klasse `A`geerbt wurde.</span><span class="sxs-lookup"><span data-stu-id="0155f-225">The method introduced by class `C` hides the method inherited from class `A`.</span></span> <span data-ttu-id="0155f-226">Folglich überschreibt die `Overrides` Deklaration in Class `D` die von Class `C`eingeführte Methode, und es ist nicht möglich, dass Klassen `D` die Methode überschreiben, die von Class `A`eingeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="0155f-226">Thus, the `Overrides` declaration in class `D` overrides the method introduced by class `C`, and it is not possible for class `D` to override the method introduced by class `A`.</span></span> <span data-ttu-id="0155f-227">Das Beispiel erzeugt die Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="0155f-227">The example produces the output:</span></span>

```console
B.F
B.F
D.F
D.F
```

<span data-ttu-id="0155f-228">Es ist möglich, die ausgeblendete `Overridable` Methode aufzurufen, indem Sie auf eine Instanz der-Klasse zugreifen `D` durch einen weniger abgeleiteten Typ, in dem die Methode nicht ausgeblendet ist.</span><span class="sxs-lookup"><span data-stu-id="0155f-228">It is possible to invoke the hidden `Overridable` method by accessing an instance of class `D` through a less-derived type in which the method is not hidden.</span></span>

<span data-ttu-id="0155f-229">Es ist nicht zulässig, eine `MustOverride` Methode zu schattieren, da dies in den meisten Fällen dazu führen würde, dass die Klasse unbrauchbar wird.</span><span class="sxs-lookup"><span data-stu-id="0155f-229">It is not valid to shadow a `MustOverride` method, because in most cases this would make the class unusable.</span></span> <span data-ttu-id="0155f-230">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-230">For example:</span></span>

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

<span data-ttu-id="0155f-231">In diesem Fall ist die Klasse `MoreDerived` erforderlich, um die `MustOverride`-Methode `Base.F`zu überschreiben, aber da die Klasse `Derived` Schatten `Base.F`, ist dies nicht möglich.</span><span class="sxs-lookup"><span data-stu-id="0155f-231">In this case, the class `MoreDerived` is required to override the `MustOverride` method `Base.F`, but because the class `Derived` shadows `Base.F`, this is not possible.</span></span> <span data-ttu-id="0155f-232">Es gibt keine Möglichkeit, einen gültigen Nachfolger `Derived`zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="0155f-232">There is no way to declare a valid descendent of `Derived`.</span></span>

<span data-ttu-id="0155f-233">Im Gegensatz zum shadoochen eines Namens aus einem äußeren Gültigkeitsbereich bewirkt das shadoochen eines barrierefreien namens aus einem geerbten Bereich, dass eine Warnung ausgegeben wird, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="0155f-233">In contrast to shadowing a name from an outer scope, shadowing an accessible name from an inherited scope causes a warning to be reported, as in the following example:</span></span>

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

<span data-ttu-id="0155f-234">Die Deklaration der-Methode `F` in der-Klasse `Derived` bewirkt, dass eine Warnung gemeldet wird.</span><span class="sxs-lookup"><span data-stu-id="0155f-234">The declaration of method `F` in class `Derived` causes a warning to be reported.</span></span> <span data-ttu-id="0155f-235">Ein shadodown eines geerbten Namens ist kein Fehler, da dies die getrennte Weiterentwicklung von Basisklassen ausschließen würde.</span><span class="sxs-lookup"><span data-stu-id="0155f-235">Shadowing an inherited name is specifically not an error, since that would preclude separate evolution of base classes.</span></span> <span data-ttu-id="0155f-236">Die obige Situation könnte z. b. der Fall sein, weil eine neuere Version der Klasse `Base` eine Methode `F` eingeführt hat, die in einer früheren Version der Klasse nicht vorhanden war.</span><span class="sxs-lookup"><span data-stu-id="0155f-236">For example, the above situation might have come about because a later version of class `Base` introduced a method `F` that was not present in an earlier version of the class.</span></span> <span data-ttu-id="0155f-237">Wäre die oben aufgeführte Situation ein Fehler gewesen, kann jede Änderung an einer Basisklasse in einer separaten Klassenbibliothek möglicherweise dazu führen, *dass* abgeleitete Klassen ungültig werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-237">Had the above situation been an error, *any* change made to a base class in a separately versioned class library could potentially cause derived classes to become invalid.</span></span>

<span data-ttu-id="0155f-238">Die Warnung, die durch shadoassen eines geerbten Namens verursacht wurde, kann durch die Verwendung des `Shadows` oder `Overloads` Modifizierers gelöscht werden:</span><span class="sxs-lookup"><span data-stu-id="0155f-238">The warning caused by shadowing an inherited name can be eliminated through use of the `Shadows` or `Overloads` modifier:</span></span>

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

<span data-ttu-id="0155f-239">Der `Shadows` Modifizierer gibt die Absicht an, den geerbten Member zu schattieren.</span><span class="sxs-lookup"><span data-stu-id="0155f-239">The `Shadows` modifier indicates the intention to shadow the inherited member.</span></span> <span data-ttu-id="0155f-240">Es ist kein Fehler, den `Shadows` oder `Overloads` Modifizierer anzugeben, wenn kein Typmember Name zum Schatten vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="0155f-240">It is not an error to specify the `Shadows` or `Overloads` modifier if there is no type member name to shadow.</span></span>

<span data-ttu-id="0155f-241">Eine Deklaration eines neuen Members schattent einen geerbten Member nur innerhalb des Bereichs des neuen Members, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="0155f-241">A declaration of a new member shadows an inherited member only within the scope of the new member, as in the following example:</span></span>

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

<span data-ttu-id="0155f-242">Im obigen Beispiel wird die Deklaration der-Methode `F` in der-Klasse `Derived` Shadowing für die-Methode `F`, die von Class `Base`geerbt wurde, aber da die neue Methode `F` in der-Klasse `Derived` Zugriff hat, wird deren Bereich nicht auf Class `Private` erweitert.`MoreDerived`</span><span class="sxs-lookup"><span data-stu-id="0155f-242">In the example above, the declaration of method `F` in class `Derived` shadows the method `F` that was inherited from class `Base`, but since the new method `F` in class `Derived` has `Private` access, its scope does not extend to class `MoreDerived`.</span></span> <span data-ttu-id="0155f-243">Daher ist der Aufruf `F()` in `MoreDerived.G` gültig und wird `Base.F`aufrufen.</span><span class="sxs-lookup"><span data-stu-id="0155f-243">Thus, the call `F()` in `MoreDerived.G` is valid and will invoke `Base.F`.</span></span> <span data-ttu-id="0155f-244">Im Fall von überladenen Typmembern wird der gesamte Satz von überladenen Typmembern so behandelt, als ob Sie alle den meisten Berechtigungen für shadodown haben.</span><span class="sxs-lookup"><span data-stu-id="0155f-244">In the case of overloaded type members, the entire set of overloaded type members is treated as if they all had the most permissive access for the purposes of shadowing.</span></span>

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

<span data-ttu-id="0155f-245">Obwohl die Deklaration von `F()` in `Derived` mit `Private` Access deklariert ist, wird in diesem Beispiel die überladene `F(Integer)` mit `Public` Access deklariert.</span><span class="sxs-lookup"><span data-stu-id="0155f-245">In this example, even though the declaration of `F()` in `Derived` is declared with `Private` access, the overloaded `F(Integer)` is declared with `Public` access.</span></span> <span data-ttu-id="0155f-246">Daher wird der Name `F` in `Derived` so behandelt, als ob er `Public`wäre, sodass beide Methoden Schatten `F` in `Base`werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-246">Therefore, for the purpose of shadowing, the name `F` in `Derived` is treated as if it was `Public`, so both methods shadow `F` in `Base`.</span></span>

## <a name="implementation"></a><span data-ttu-id="0155f-247">Implementierung</span><span class="sxs-lookup"><span data-stu-id="0155f-247">Implementation</span></span>

<span data-ttu-id="0155f-248">Eine *Implementierungs* Beziehung ist vorhanden, wenn ein Typ deklariert, dass eine Schnittstelle implementiert wird und der Typ alle Typmember der Schnittstelle implementiert.</span><span class="sxs-lookup"><span data-stu-id="0155f-248">An *implementation* relationship exists when a type declares that it implements an interface and the type implements all the type members of the interface.</span></span> <span data-ttu-id="0155f-249">Ein Typ, der eine bestimmte Schnittstelle implementiert, kann in diese Schnittstelle konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-249">A type that implements a particular interface is convertible to that interface.</span></span> <span data-ttu-id="0155f-250">Schnittstellen können nicht instanziiert werden, aber es ist zulässig, Variablen der Schnittstellen zu deklarieren. solchen Variablen kann nur ein Wert zugewiesen werden, der eine Klasse ist, die die-Schnittstelle implementiert.</span><span class="sxs-lookup"><span data-stu-id="0155f-250">Interfaces cannot be instantiated, but it is valid to declare variables of interfaces; such variables can only be assigned a value that is of a class that implements the interface.</span></span> <span data-ttu-id="0155f-251">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-251">For example:</span></span>

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

<span data-ttu-id="0155f-252">Ein Typ, der eine Schnittstelle mit multiplizierten Typmembern implementiert, muss diese Methoden dennoch implementieren, auch wenn nicht direkt von der implementierten Schnittstelle aus auf Sie zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="0155f-252">A type implementing an interface with multiply-inherited type members must still implement those methods, even though they cannot be accessed directly from the derived interface being implemented.</span></span> <span data-ttu-id="0155f-253">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-253">For example:</span></span>

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

<span data-ttu-id="0155f-254">Auch `MustInherit` Klassen müssen Implementierungen aller Member der implementierten Schnittstellen bereitstellen. Allerdings können Sie die Implementierung dieser Methoden verzögern, indem Sie Sie als `MustOverride`deklarieren.</span><span class="sxs-lookup"><span data-stu-id="0155f-254">Even `MustInherit` classes must provide implementations of all the members of implemented interfaces; however, they can defer implementation of these methods by declaring them as `MustOverride`.</span></span> <span data-ttu-id="0155f-255">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-255">For example:</span></span>

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

<span data-ttu-id="0155f-256">Ein Typ kann eine Schnittstelle, die vom Basistyp implementiert wird, erneut implementieren.</span><span class="sxs-lookup"><span data-stu-id="0155f-256">A type may choose to re-implement an interface that its base type implements.</span></span> <span data-ttu-id="0155f-257">Um die-Schnittstelle erneut zu implementieren, muss der Typ explizit angeben, dass die-Schnittstelle implementiert wird.</span><span class="sxs-lookup"><span data-stu-id="0155f-257">To re-implement the interface, the type must explicitly state that it implements the interface.</span></span> <span data-ttu-id="0155f-258">Ein Typ, der eine Schnittstelle erneut implementiert, kann die Implementierung nur einiger, aber nicht aller Member der Schnittstelle wieder implementieren. alle Member, die nicht erneut implementiert werden, verwenden weiterhin die Implementierung des Basistyps.</span><span class="sxs-lookup"><span data-stu-id="0155f-258">A type re-implementing an interface may choose to re-implement only some, but not all, of the members of the interface -- any members not re-implemented continue to use the base type's implementation.</span></span> <span data-ttu-id="0155f-259">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-259">For example:</span></span>

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

<span data-ttu-id="0155f-260">In diesem Beispiel wird Folgendes gedruckt:</span><span class="sxs-lookup"><span data-stu-id="0155f-260">This example prints:</span></span>

```console
TestDerived.DerivedTest1
TestBase.Test2
```

<span data-ttu-id="0155f-261">Wenn ein abgeleiteter Typ eine Schnittstelle implementiert, deren Basis Schnittstellen von den Basis Typen des abgeleiteten Typs implementiert werden, kann der abgeleitete Typ festlegen, dass nur die Typmember der Schnittstelle implementiert werden, die von den Basis Typen nicht bereits implementiert wurden.</span><span class="sxs-lookup"><span data-stu-id="0155f-261">When a derived type implements an interface whose base interfaces are implemented by the derived type's base types, the derived type can choose to only implement the interface's type members that are not already implemented by the base types.</span></span> <span data-ttu-id="0155f-262">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-262">For example:</span></span>

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

<span data-ttu-id="0155f-263">Eine Schnittstellen Methode kann auch mithilfe einer über schreibbaren Methode in einem Basistyp implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-263">An interface method can also be implemented using an overridable method in a base type.</span></span> <span data-ttu-id="0155f-264">In diesem Fall kann ein abgeleiteter Typ auch die über schreibbare Methode überschreiben und die Implementierung der Schnittstelle ändern.</span><span class="sxs-lookup"><span data-stu-id="0155f-264">In that case, a derived type may also override the overridable method and alter the implementation of the interface.</span></span> <span data-ttu-id="0155f-265">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-265">For example:</span></span>

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

### <a name="implementing-methods"></a><span data-ttu-id="0155f-266">Implementieren von Methoden</span><span class="sxs-lookup"><span data-stu-id="0155f-266">Implementing Methods</span></span>

<span data-ttu-id="0155f-267">Ein Typ *implementiert* einen Typmember einer implementierten Schnittstelle durch Bereitstellen einer Methode mit einer `Implements`-Klausel.</span><span class="sxs-lookup"><span data-stu-id="0155f-267">A type *implements* a type member of an implemented interface by supplying a method with an `Implements` clause.</span></span> <span data-ttu-id="0155f-268">Die beiden Typmember müssen die gleiche Anzahl von Parametern aufweisen, alle Typen und Modifizierer der Parameter müssen übereinstimmen, einschließlich des Standardwerts optionaler Parameter, der Rückgabetyp muss übereinstimmen, und alle Einschränkungen für Methoden Parameter müssen übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0155f-268">The two type members must have the same number of parameters, all of the types and modifiers of the parameters must match, including the default value of optional parameters, the return type must match, and all of the constraints on method parameters must match.</span></span> <span data-ttu-id="0155f-269">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-269">For example:</span></span>

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

<span data-ttu-id="0155f-270">Eine einzelne Methode kann eine beliebige Anzahl von Schnittstellentypmembern implementieren, wenn Sie alle die oben aufgeführten Kriterien erfüllen.</span><span class="sxs-lookup"><span data-stu-id="0155f-270">A single method may implement any number of interface type members if they all meet the above criteria.</span></span> <span data-ttu-id="0155f-271">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-271">For example:</span></span>

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

<span data-ttu-id="0155f-272">Wenn eine Methode in einer generischen Schnittstelle implementiert wird, muss die implementierende Methode die Typargumente bereitstellen, die den Typparametern der Schnittstelle entsprechen.</span><span class="sxs-lookup"><span data-stu-id="0155f-272">When implementing a method in a generic interface, the implementing method must supply the type arguments that correspond to the interface's type parameters.</span></span> <span data-ttu-id="0155f-273">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-273">For example:</span></span>

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

<span data-ttu-id="0155f-274">Beachten Sie, dass es möglich ist, dass eine generische Schnittstelle für einen Satz von Typargumenten möglicherweise nicht implementierbar ist.</span><span class="sxs-lookup"><span data-stu-id="0155f-274">Note that it is possible that a generic interface may not be implementable for some set of type arguments.</span></span>

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

## <a name="polymorphism"></a><span data-ttu-id="0155f-275">Polymorphismus</span><span class="sxs-lookup"><span data-stu-id="0155f-275">Polymorphism</span></span>

<span data-ttu-id="0155f-276">*Polymorphismus* bietet die Möglichkeit, die Implementierung einer Methode oder Eigenschaft zu verändern.</span><span class="sxs-lookup"><span data-stu-id="0155f-276">*Polymorphism* provides the ability to vary the implementation of a method or property.</span></span> <span data-ttu-id="0155f-277">Bei Polymorphie kann dieselbe Methode oder Eigenschaft je nach Lauf Zeittyp der Instanz, von der Sie aufgerufen wird, verschiedene Aktionen ausführen.</span><span class="sxs-lookup"><span data-stu-id="0155f-277">With polymorphism, the same method or property can perform different actions depending on the run-time type of the instance that invokes it.</span></span> <span data-ttu-id="0155f-278">Methoden oder Eigenschaften, die polymorph sind, werden als *über schreibbare*bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="0155f-278">Methods or properties that are polymorphic are called *overridable*.</span></span> <span data-ttu-id="0155f-279">Im Gegensatz dazu ist die Implementierung einer nicht über schreibbaren Methode oder Eigenschaft invariant. die Implementierung ist identisch, unabhängig davon, ob die Methode oder Eigenschaft für eine Instanz der Klasse, in der Sie deklariert ist, oder für eine Instanz einer abgeleiteten Klasse aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="0155f-279">By contrast, the implementation of a non-overridable method or property is invariant; the implementation is the same whether the method or property is invoked on an instance of the class in which it is declared or an instance of a derived class.</span></span> <span data-ttu-id="0155f-280">Wenn eine nicht über schreibbare Methode oder Eigenschaft aufgerufen wird, ist der Kompilier Zeittyp der Instanz der bestimmende Faktor.</span><span class="sxs-lookup"><span data-stu-id="0155f-280">When a non-overridable method or property is invoked, the compile-time type of the instance is the determining factor.</span></span> <span data-ttu-id="0155f-281">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-281">For example:</span></span>

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

<span data-ttu-id="0155f-282">Eine über schreibbare Methode kann auch `MustOverride`sein, was bedeutet, dass Sie keinen Methoden Text bereitstellt und überschrieben werden muss.</span><span class="sxs-lookup"><span data-stu-id="0155f-282">An overridable method may also be `MustOverride`, which means that it provides no method body and must be overridden.</span></span> <span data-ttu-id="0155f-283">`MustOverride` Methoden sind nur in `MustInherit` Klassen zulässig.</span><span class="sxs-lookup"><span data-stu-id="0155f-283">`MustOverride` methods are only allowed in `MustInherit` classes.</span></span>

<span data-ttu-id="0155f-284">Im folgenden Beispiel definiert die-Klasse `Shape` den abstrakten Begriff eines geometrischen Shape-Objekts, das sich selbst zeichnen kann:</span><span class="sxs-lookup"><span data-stu-id="0155f-284">In the following example, the class `Shape` defines the abstract notion of a geometrical shape object that can paint itself:</span></span>

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

<span data-ttu-id="0155f-285">Die `Paint`-Methode ist `MustOverride`, da keine sinnvolle Standard Implementierung vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="0155f-285">The `Paint` method is `MustOverride` because there is no meaningful default implementation.</span></span> <span data-ttu-id="0155f-286">Die Klassen `Ellipse` und `Box` sind konkrete `Shape` Implementierungen.</span><span class="sxs-lookup"><span data-stu-id="0155f-286">The `Ellipse` and `Box` classes are concrete `Shape` implementations.</span></span> <span data-ttu-id="0155f-287">Da diese Klassen nicht `MustInherit`werden, müssen Sie die `Paint`-Methode überschreiben und eine tatsächliche Implementierung bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="0155f-287">Because these classes are not `MustInherit`, they are required to override the `Paint` method and provide an actual implementation.</span></span>

<span data-ttu-id="0155f-288">Es ist ein Fehler für einen Basis Zugriff, der auf eine `MustOverride` Methode verweist, wie im folgenden Beispiel veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="0155f-288">It is an error for a base access to reference a `MustOverride` method, as the following example demonstrates:</span></span>

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

<span data-ttu-id="0155f-289">Für den `MyBase.F()` Aufruf wird ein Fehler gemeldet, weil er auf eine `MustOverride` Methode verweist.</span><span class="sxs-lookup"><span data-stu-id="0155f-289">An error is reported for the `MyBase.F()` invocation because it references a `MustOverride` method.</span></span>

### <a name="overriding-methods"></a><span data-ttu-id="0155f-290">Überschreiben von Methoden</span><span class="sxs-lookup"><span data-stu-id="0155f-290">Overriding Methods</span></span>

<span data-ttu-id="0155f-291">Ein Typ kann eine geerbte über schreibbare Methode durch Deklarieren einer Methode mit dem gleichen Namen und der gleichen Signatur über *Schreiben* und die Deklaration mit dem `Overrides` Modifizierer markieren. Es gibt zusätzliche Anforderungen für das Überschreiben von Methoden, die unten aufgeführt sind.</span><span class="sxs-lookup"><span data-stu-id="0155f-291">A type may *override* an inherited overridable method by declaring a method with the same name and , signature, and marking the declaration with the `Overrides` modifier.There are additional requirements on overriding methods, listed below.</span></span> <span data-ttu-id="0155f-292">Während eine `Overridable` Methoden Deklaration eine neue Methode einführt, ersetzt eine `Overrides` Methoden Deklaration die geerbte Implementierung der-Methode.</span><span class="sxs-lookup"><span data-stu-id="0155f-292">Whereas an `Overridable` method declaration introduces a new method, an `Overrides` method declaration replaces the inherited implementation of the method.</span></span>

<span data-ttu-id="0155f-293">Eine über schreibende Methode kann `NotOverridable`deklariert werden. Dadurch wird verhindert, dass die Methode in abgeleiteten Typen weiter überschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="0155f-293">An overriding method may be declared `NotOverridable`, which prevents any further overriding of the method in derived types.</span></span> <span data-ttu-id="0155f-294">In der Tat werden `NotOverridable` Methoden in allen weiteren abgeleiteten Klassen nicht über schreibbar.</span><span class="sxs-lookup"><span data-stu-id="0155f-294">In effect, `NotOverridable` methods become non-overridable in any further derived classes.</span></span>

<span data-ttu-id="0155f-295">Betrachten Sie das folgende Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-295">Consider the following example:</span></span>

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

<span data-ttu-id="0155f-296">Im Beispiel stellt Class `B` zwei `Overrides` Methoden bereit: eine Methode `F`, die über den `NotOverridable`-Modifizierer und eine Methode `G` verfügt.</span><span class="sxs-lookup"><span data-stu-id="0155f-296">In the example, class `B` provides two `Overrides` methods: a method `F` that has the `NotOverridable` modifier and a method `G` that does not.</span></span> <span data-ttu-id="0155f-297">Die Verwendung des-Modifizierers `NotOverridable` verhindert, dass die-Klasse `C` weitere Methoden `F`überschreibt.</span><span class="sxs-lookup"><span data-stu-id="0155f-297">Use of the `NotOverridable` modifier prevents class `C` from further overriding method `F`.</span></span>

<span data-ttu-id="0155f-298">Eine über schreibende Methode kann auch als `MustOverride`deklariert werden, auch wenn die Methode, die Sie überschreibt, nicht als `MustOverride`deklariert ist.</span><span class="sxs-lookup"><span data-stu-id="0155f-298">An overriding method may also be declared `MustOverride`, even if the method that it is overriding is not declared `MustOverride`.</span></span> <span data-ttu-id="0155f-299">Dies erfordert, dass die enthaltende Klasse `MustInherit` deklariert wird und dass alle weiteren abgeleiteten Klassen, die nicht als `MustInherit` deklariert sind, die-Methode überschreiben müssen.</span><span class="sxs-lookup"><span data-stu-id="0155f-299">This requires that the containing class be declared `MustInherit` and that any further derived classes that are not declared `MustInherit` must override the method.</span></span> <span data-ttu-id="0155f-300">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-300">For example:</span></span>

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

<span data-ttu-id="0155f-301">Im Beispiel überschreibt Class `B` `A.F` mit einer `MustOverride`-Methode.</span><span class="sxs-lookup"><span data-stu-id="0155f-301">In the example, class `B` overrides `A.F` with a `MustOverride` method.</span></span> <span data-ttu-id="0155f-302">Dies bedeutet, dass alle Klassen, die von `B` abgeleitet werden, `F`außer Kraft setzen müssen, es sei denn, Sie werden ebenfalls als `MustInherit` deklariert.</span><span class="sxs-lookup"><span data-stu-id="0155f-302">This means that any classes derived from `B` will have to override `F`, unless they are declared `MustInherit` as well.</span></span>

<span data-ttu-id="0155f-303">Ein Kompilierzeitfehler tritt auf, es sei denn, für eine über schreibende Methode gilt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="0155f-303">A compile-time error occurs unless all of the following are true of an overriding method:</span></span>

* <span data-ttu-id="0155f-304">Der Deklarations Kontext enthält eine einzelne barrierefreie Methode mit der gleichen Signatur und dem gleichen Rückgabetyp (sofern vorhanden) als über schreibende Methode.</span><span class="sxs-lookup"><span data-stu-id="0155f-304">The declaration context contains a single accessible inherited method with the same signature and return type (if any) as the overriding method.</span></span>
* <span data-ttu-id="0155f-305">Die geerbte Methode, die überschrieben wird, ist über schreibbar.</span><span class="sxs-lookup"><span data-stu-id="0155f-305">The inherited method being overridden is overridable.</span></span> <span data-ttu-id="0155f-306">Anders ausgedrückt: die geerbte Methode, die überschrieben wird, ist nicht `Shared` oder `NotOverridable`.</span><span class="sxs-lookup"><span data-stu-id="0155f-306">In other words, the inherited method being overridden is not `Shared` or `NotOverridable`.</span></span>
* <span data-ttu-id="0155f-307">Die Zugriffs Domäne der deklarierten Methode ist identisch mit der Zugriffs Domäne der geerbten Methode, die überschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="0155f-307">The accessibility domain of the method being declared is the same as the accessibility domain of the inherited method being overridden.</span></span> <span data-ttu-id="0155f-308">Es gibt eine Ausnahme: eine `Protected Friend` Methode muss von einer `Protected`-Methode überschrieben werden, wenn die andere Methode in einer anderen Assembly enthalten ist, auf die die über schreibende Methode nicht `Friend` Zugriff hat.</span><span class="sxs-lookup"><span data-stu-id="0155f-308">There is one exception: a `Protected Friend` method must be overridden by a `Protected` method if the other method is in another assembly that the overriding method does not have `Friend` access to.</span></span>
* <span data-ttu-id="0155f-309">Die Parameter der über schreibenden Methode entsprechen den Parametern der überschriebenen Methode in Bezug auf die Verwendung der Modifizierers `ByVal`, `ByRef`, `ParamArray,` und `Optional`, einschließlich der Werte, die für optionale Parameter bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-309">The parameters of the overriding method match the overridden method's parameters in regards to usage of the `ByVal`, `ByRef`, `ParamArray,` and `Optional` modifiers, including the values provided for optional parameters.</span></span>
* <span data-ttu-id="0155f-310">Die Typparameter der über schreibenden Methode entsprechen den Typparametern der überschriebenen Methode in Bezug auf Typeinschränkungen.</span><span class="sxs-lookup"><span data-stu-id="0155f-310">The type parameters of the overriding method match the overridden method's type parameters in regards to type constraints.</span></span>

<span data-ttu-id="0155f-311">Beim Überschreiben einer Methode in einem generischen Basistyp muss die über schreibende Methode die Typargumente bereitstellen, die den Basistyp Parametern entsprechen.</span><span class="sxs-lookup"><span data-stu-id="0155f-311">When overriding a method in a base generic type, the overriding method must supply the type arguments that correspond to the base type parameters.</span></span> <span data-ttu-id="0155f-312">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-312">For example:</span></span>

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

<span data-ttu-id="0155f-313">Beachten Sie, dass eine über schreibbare Methode in einer generischen Klasse möglicherweise für einige Sätze von Typargumenten nicht überschrieben werden kann.</span><span class="sxs-lookup"><span data-stu-id="0155f-313">Note that it is possible that an overridable method in a generic class may not be able to be overridden for some sets of type arguments.</span></span> <span data-ttu-id="0155f-314">Wenn die Methode als `MustOverride`deklariert ist, bedeutet dies, dass einige Vererbungs Ketten möglicherweise nicht möglich sind.</span><span class="sxs-lookup"><span data-stu-id="0155f-314">If the method is declared `MustOverride`, this means that some inheritance chains may not be possible.</span></span> <span data-ttu-id="0155f-315">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-315">For example:</span></span>

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

<span data-ttu-id="0155f-316">Eine Überschreibungs Deklaration kann mithilfe eines Basis Zugriffs auf die überschriebene Basis Methode zugreifen, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="0155f-316">An override declaration can access the overridden base method using a base access, as in the following example:</span></span>

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

Im Beispiel ruft der Aufruf von `MyBase.PrintVariables()` in Class `Derived` die in Class `Base`deklarierte `PrintVariables` Methode auf. Durch einen Basis Zugriff wird der über schreibbare Aufruf Mechanismus deaktiviert und die Basis Methode einfach als nicht über schreibbare Methode behandelt. <span data-ttu-id="0155f-319">Hätte der Aufruf in `Derived` geschrieben `CType(Me, Base).PrintVariables()`, würde er die in `Derived`deklarierte `PrintVariables` Methode rekursiv aufrufen, nicht die in `Base`deklarierte Methode.</span><span class="sxs-lookup"><span data-stu-id="0155f-319">Had the invocation in `Derived` been written `CType(Me, Base).PrintVariables()`, it would recursively invoke the `PrintVariables` method declared in `Derived`, not the one declared in `Base`.</span></span>

<span data-ttu-id="0155f-320">Nur wenn Sie einen `Overrides` Modifizierer enthält, kann eine Methode eine andere Methode überschreiben.</span><span class="sxs-lookup"><span data-stu-id="0155f-320">Only when it includes an `Overrides` modifier can a method override another method.</span></span> <span data-ttu-id="0155f-321">In allen anderen Fällen Shadowing eine Methode, die die gleiche Signatur wie eine geerbte Methode hat, einfach die geerbte Methode, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="0155f-321">In all other cases, a method with the same signature as an inherited method simply shadows the inherited method, as in the example below:</span></span>

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

<span data-ttu-id="0155f-322">In diesem Beispiel enthält die-Methode `F` in Class `Derived` keinen `Overrides`-Modifizierer und überschreibt daher nicht die-Methode `F` in der Klasse `Base`.</span><span class="sxs-lookup"><span data-stu-id="0155f-322">In the example, the method `F` in class `Derived` does not include an `Overrides` modifier and therefore does not override method `F` in class `Base`.</span></span> <span data-ttu-id="0155f-323">Die Methode `F` in der Klasse `Derived` Shadowing der Methode in der Klasse `Base`, und es wird eine Warnung ausgegeben, da die Deklaration keinen `Shadows` oder `Overloads` Modifizierer enthält.</span><span class="sxs-lookup"><span data-stu-id="0155f-323">Rather, method `F` in class `Derived` shadows the method in class `Base`, and a warning is reported because the declaration does not include a `Shadows` or `Overloads` modifier.</span></span>

<span data-ttu-id="0155f-324">Im folgenden Beispiel wird die Methode `F` in der-Klasse `Derived` Shadowing der über schreibbaren-Methode `F` von Class `Base`geerbt:</span><span class="sxs-lookup"><span data-stu-id="0155f-324">In the following example, method `F` in class `Derived` shadows the overridable method `F` inherited from class `Base`:</span></span>

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

<span data-ttu-id="0155f-325">Da die neue Methode `F` in der Klasse `Derived` über `Private` Zugriff verfügt, umfasst ihr Bereich nur den Klassen Text `Derived` und wird nicht auf die Klasse `MoreDerived`erweitert.</span><span class="sxs-lookup"><span data-stu-id="0155f-325">Since the new method `F` in class `Derived` has `Private` access, its scope only includes the class body of `Derived` and does not extend to class `MoreDerived`.</span></span> <span data-ttu-id="0155f-326">Die Deklaration der-Methode `F` in der-Klasse `MoreDerived` kann daher die-`F` Methode überschreiben, die von der-Klasse `Base`geerbt wurde.</span><span class="sxs-lookup"><span data-stu-id="0155f-326">The declaration of method `F` in class `MoreDerived` is therefore permitted to override the method `F` inherited from class `Base`.</span></span>

<span data-ttu-id="0155f-327">Wenn eine `Overridable` Methode aufgerufen wird, wird die am meisten abgeleitete Implementierung der Instanzmethode basierend auf dem Typ der Instanz aufgerufen, unabhängig davon, ob der Aufruf an die Methode in der Basisklasse oder der abgeleiteten Klasse erfolgt.</span><span class="sxs-lookup"><span data-stu-id="0155f-327">When an `Overridable` method is invoked, the most derived implementation of the instance method is called, based on the type of the instance, regardless of whether the call is to the method in the base class or the derived class.</span></span> <span data-ttu-id="0155f-328">Die am weitesten abgeleitete Implementierung einer `Overridable` Methode `M` in Bezug auf eine Klasse `R` wie folgt bestimmt werden:</span><span class="sxs-lookup"><span data-stu-id="0155f-328">The most derived implementation of an `Overridable` method `M` with respect to a class `R` is determined as follows:</span></span>

* <span data-ttu-id="0155f-329">Wenn `R` die Introducing `Overridable` Deklaration von `M`enthält, handelt es sich hierbei um die am meisten abgeleitete Implementierung `M`.</span><span class="sxs-lookup"><span data-stu-id="0155f-329">If `R` contains the introducing `Overridable` declaration of `M`, this is the most derived implementation of `M`.</span></span>

* <span data-ttu-id="0155f-330">Wenn `R` eine außer Kraft Setzung von `M`enthält, handelt es sich hierbei um die am meisten abgeleitete Implementierung von `M`.</span><span class="sxs-lookup"><span data-stu-id="0155f-330">Otherwise, if `R` contains an override of `M`, this is the most derived implementation of `M`.</span></span>

* <span data-ttu-id="0155f-331">Andernfalls ist die am meisten abgeleitete Implementierung von `M` identisch mit der der direkten Basisklasse von `R`.</span><span class="sxs-lookup"><span data-stu-id="0155f-331">Otherwise, the most derived implementation of `M` is the same as that of the direct base class of `R`.</span></span>

## <a name="accessibility"></a><span data-ttu-id="0155f-332">Zugriff</span><span class="sxs-lookup"><span data-stu-id="0155f-332">Accessibility</span></span>

<span data-ttu-id="0155f-333">Eine Deklaration gibt den Zugriff auf die Entität an *, die Sie* deklariert.</span><span class="sxs-lookup"><span data-stu-id="0155f-333">A declaration specifies the *accessibility* of the entity it declares.</span></span> <span data-ttu-id="0155f-334">Der Zugriff einer Entität ändert nicht den Gültigkeitsbereich eines Entitäts namens.</span><span class="sxs-lookup"><span data-stu-id="0155f-334">An entity's accessibility does not change the scope of an entity's name.</span></span> <span data-ttu-id="0155f-335">Die *Barrierefreiheits Domäne* einer Deklaration ist der Satz aller Deklarations Bereiche, in denen die deklarierte Entität zugänglich ist.</span><span class="sxs-lookup"><span data-stu-id="0155f-335">The *accessibility domain* of a declaration is the set of all declaration spaces in which the declared entity is accessible.</span></span>

<span data-ttu-id="0155f-336">Die fünf Zugriffs Typen sind `Public`, `Protected`, `Friend`, `Protected Friend`und `Private`.</span><span class="sxs-lookup"><span data-stu-id="0155f-336">The five access types are `Public`, `Protected`, `Friend`, `Protected Friend`, and `Private`.</span></span> <span data-ttu-id="0155f-337">`Public` ist der sicherste Zugriffstyp, und die vier anderen Typen sind alle Teilmengen von `Public`.</span><span class="sxs-lookup"><span data-stu-id="0155f-337">`Public` is the most permissive access type, and the four other types are all subsets of `Public`.</span></span> <span data-ttu-id="0155f-338">Der geringstmöglichen Zugriffstyp ist `Private`, und die vier anderen Zugriffs Typen sind alle übergeordnete `Private`.</span><span class="sxs-lookup"><span data-stu-id="0155f-338">The least permissive access type is `Private`, and the four other access types are all supersets of `Private`.</span></span>

```antlr
AccessModifier
    : 'Public'
    | 'Protected'
    | 'Friend'
    | 'Private'
    | 'Protected' 'Friend'
    ;
```

<span data-ttu-id="0155f-339">Der Zugriffstyp für eine Deklaration wird über einen optionalen Zugriffsmodifizierer angegeben, der `Public`, `Protected`, `Friend`, `Private`oder die Kombination aus `Protected` und `Friend`sein kann.</span><span class="sxs-lookup"><span data-stu-id="0155f-339">The access type for a declaration is specified via an optional access modifier, which can be `Public`, `Protected`, `Friend`, `Private`, or the combination of `Protected` and `Friend`.</span></span> <span data-ttu-id="0155f-340">Wenn kein Zugriffsmodifizierer angegeben ist, hängt der Standard Zugriffstyp vom Deklarations Kontext ab. die zulässigen Zugriffs Typen sind auch vom Deklarations Kontext abhängig.</span><span class="sxs-lookup"><span data-stu-id="0155f-340">If no access modifier is specified, the default access type depends on the declaration context; the permitted access types also depend on the declaration context.</span></span>

* <span data-ttu-id="0155f-341">Mit dem `Public`-Modifizierer deklarierte Entitäten haben `Public` Zugriff.</span><span class="sxs-lookup"><span data-stu-id="0155f-341">Entities declared with the `Public` modifier have `Public` access.</span></span> <span data-ttu-id="0155f-342">Es gibt keine Einschränkungen hinsichtlich der Verwendung von `Public` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="0155f-342">There are no restrictions on the use of `Public` entities.</span></span>

* <span data-ttu-id="0155f-343">Mit dem `Protected`-Modifizierer deklarierte Entitäten haben `Protected` Zugriff.</span><span class="sxs-lookup"><span data-stu-id="0155f-343">Entities declared with the `Protected` modifier have `Protected` access.</span></span> <span data-ttu-id="0155f-344">`Protected` Zugriff kann nur für Member von Klassen angegeben werden (sowohl reguläre Typmember als auch Unterklassen) oder für `Overridable` Member von Standardmodulen und-Strukturen (die definitionsgemäß von `System.Object` oder `System.ValueType`geerbt werden müssen).</span><span class="sxs-lookup"><span data-stu-id="0155f-344">`Protected` access can only be specified on members of classes (both regular type members and nested classes) or on `Overridable` members of standard modules and structures (which must, by definition, be inherited from `System.Object` or `System.ValueType`).</span></span> <span data-ttu-id="0155f-345">Auf einen `Protected` Member kann von einer abgeleiteten Klasse zugegriffen werden, vorausgesetzt, der Member ist kein Instanzmember, oder der Zugriff erfolgt über eine Instanz der abgeleiteten Klasse.</span><span class="sxs-lookup"><span data-stu-id="0155f-345">A `Protected` member is accessible to a derived class, provided that either the member is not an instance member, or the access takes place through an instance of the derived class.</span></span> <span data-ttu-id="0155f-346">`Protected` Zugriff ist keine übergeordnete `Friend` Zugriffs.</span><span class="sxs-lookup"><span data-stu-id="0155f-346">`Protected` access is not a superset of `Friend` access.</span></span>

* <span data-ttu-id="0155f-347">Mit dem `Friend`-Modifizierer deklarierte Entitäten haben `Friend` Zugriff.</span><span class="sxs-lookup"><span data-stu-id="0155f-347">Entities declared with the `Friend` modifier have `Friend` access.</span></span> <span data-ttu-id="0155f-348">Auf eine Entität mit `Friend` Access kann nur innerhalb des Programms zugegriffen werden, das die Entitäts Deklaration oder Assemblys enthält, die über das `System.Runtime.CompilerServices.InternalsVisibleToAttribute`-Attribut `Friend` Zugriff erhalten haben.</span><span class="sxs-lookup"><span data-stu-id="0155f-348">An entity with `Friend` access is accessible only within the program that contains the entity declaration or any assemblies that have been given `Friend` access through the `System.Runtime.CompilerServices.InternalsVisibleToAttribute` attribute.</span></span>

* <span data-ttu-id="0155f-349">Entitäten, die mit den `Protected Friend` modifizierermodifizierer deklariert wurden, haben die Vereinigung `Protected` und `Friend` Zugriffs</span><span class="sxs-lookup"><span data-stu-id="0155f-349">Entities declared with the `Protected Friend` modifiers have the union of `Protected` and `Friend` access.</span></span>

* <span data-ttu-id="0155f-350">Mit dem `Private`-Modifizierer deklarierte Entitäten haben `Private` Zugriff.</span><span class="sxs-lookup"><span data-stu-id="0155f-350">Entities declared with the `Private` modifier have `Private` access.</span></span> <span data-ttu-id="0155f-351">Auf eine `Private` Entität kann nur innerhalb Ihres Deklarations Kontexts, einschließlich geschachtelter Entitäten, zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-351">A `Private` entity is accessible only within its declaration context, including any nested entities.</span></span>

<span data-ttu-id="0155f-352">Die Barrierefreiheit in einer-Deklaration hängt nicht von der Barrierefreiheit des Deklarations Kontexts ab.</span><span class="sxs-lookup"><span data-stu-id="0155f-352">The accessibility in a declaration does not depend on the accessibility of the declaration context.</span></span> <span data-ttu-id="0155f-353">Beispielsweise kann ein Typ, der mit `Private` Access deklariert wurde, einen Typmember mit `Public` Access enthalten.</span><span class="sxs-lookup"><span data-stu-id="0155f-353">For example, a type declared with `Private` access may contain a type member with `Public` access.</span></span>

<span data-ttu-id="0155f-354">Der folgende Code veranschaulicht verschiedene Barrierefreiheits Domänen:</span><span class="sxs-lookup"><span data-stu-id="0155f-354">The following code demonstrates various accessibility domains:</span></span>

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

<span data-ttu-id="0155f-355">Die Klassen und Member in diesem Beispiel verfügen über die folgenden Barrierefreiheits Domänen:</span><span class="sxs-lookup"><span data-stu-id="0155f-355">The classes and members in this example have the following accessibility domains:</span></span>

* <span data-ttu-id="0155f-356">Die Zugriffs Domäne von `A` und `A.X` ist unbegrenzt.</span><span class="sxs-lookup"><span data-stu-id="0155f-356">The accessibility domain of `A` and `A.X` is unlimited.</span></span>

* <span data-ttu-id="0155f-357">Die Zugriffs Domäne `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`und `B.C.Y` ist das enthaltende Programm.</span><span class="sxs-lookup"><span data-stu-id="0155f-357">The accessibility domain of `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, and `B.C.Y` is the containing program.</span></span>

* <span data-ttu-id="0155f-358">Die Zugriffs Domäne `A.Z` ist `A.`</span><span class="sxs-lookup"><span data-stu-id="0155f-358">The accessibility domain of `A.Z` is `A.`</span></span>

* <span data-ttu-id="0155f-359">Die Zugriffs Domäne `B.Z`, `B.D`, `B.D.X`und `B.D.Y` ist `B`, einschließlich `B.C` und `B.D`.</span><span class="sxs-lookup"><span data-stu-id="0155f-359">The accessibility domain of `B.Z`, `B.D`, `B.D.X`, and `B.D.Y` is `B`, including `B.C` and `B.D`.</span></span>

* <span data-ttu-id="0155f-360">Die Zugriffs Domäne `B.C.Z` ist `B.C`.</span><span class="sxs-lookup"><span data-stu-id="0155f-360">The accessibility domain of `B.C.Z` is `B.C`.</span></span>

* <span data-ttu-id="0155f-361">Die Zugriffs Domäne `B.D.Z` ist `B.D`.</span><span class="sxs-lookup"><span data-stu-id="0155f-361">The accessibility domain of `B.D.Z` is `B.D`.</span></span>

<span data-ttu-id="0155f-362">Wie das Beispiel zeigt, ist die Zugriffs Domäne eines Members nie größer als die eines enthaltenden Typs.</span><span class="sxs-lookup"><span data-stu-id="0155f-362">As the example illustrates, the accessibility domain of a member is never larger than that of a containing type.</span></span> <span data-ttu-id="0155f-363">Wenn z. b. alle `X` Member über `Public` deklarierte Barrierefreiheit verfügen, verfügen alle außer `A.X` über Barrierefreiheits Domänen, die durch einen enthaltenden Typ eingeschränkt werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-363">For example, even though all `X` members have `Public` declared accessibility, all but `A.X` have accessibility domains that are constrained by a containing type.</span></span>

<span data-ttu-id="0155f-364">Der Zugriff auf `Protected` Instanzmember muss über eine Instanz des abgeleiteten Typs erfolgen, damit nicht verknüpfte Typen keinen Zugriff auf die geschützten Member der anderen erhalten.</span><span class="sxs-lookup"><span data-stu-id="0155f-364">Access to `Protected` instance members must be through an instance of the derived type so that unrelated types cannot gain access to each other's protected members.</span></span> <span data-ttu-id="0155f-365">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-365">For example:</span></span>

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

<span data-ttu-id="0155f-366">Im obigen Beispiel hat die-Klasse `Guest` nur Zugriff auf das geschützte `Password` Feld, wenn Sie mit einer Instanz von `Guest`qualifiziert ist.</span><span class="sxs-lookup"><span data-stu-id="0155f-366">In the above example, the class `Guest` only has access to the protected `Password` field if it is qualified with an instance of `Guest`.</span></span> <span data-ttu-id="0155f-367">Dadurch wird verhindert, dass `Guest` Zugriff auf das `Password` Feld eines `Employee` Objekts erhält, indem Sie es einfach in `User`umwandeln.</span><span class="sxs-lookup"><span data-stu-id="0155f-367">This prevents `Guest` from gaining access to the `Password` field of an `Employee` object simply by casting it to `User`.</span></span>

<span data-ttu-id="0155f-368">Für den `Protected` Member-Zugriff in generischen Typen enthält der Deklarations Kontext Typparameter.</span><span class="sxs-lookup"><span data-stu-id="0155f-368">For the purposes of `Protected` member access in generic types, the declaration context includes type parameters.</span></span> <span data-ttu-id="0155f-369">Dies bedeutet, dass ein abgeleiteter Typ mit einem Satz von Typargumenten keinen Zugriff auf die `Protected` Member eines abgeleiteten Typs mit einem anderen Satz von Typargumenten hat.</span><span class="sxs-lookup"><span data-stu-id="0155f-369">This means that a derived type with one set of type arguments does not have access to the `Protected` members of a derived type with a different set of type arguments.</span></span> <span data-ttu-id="0155f-370">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-370">For example:</span></span>

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

<span data-ttu-id="0155f-371">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="0155f-371">__Note.__</span></span> <span data-ttu-id="0155f-372">Die C# Sprache (und möglicherweise andere Sprachen) ermöglicht einem generischen Typ, auf `Protected` Member zuzugreifen, unabhängig davon, welche Typargumente angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-372">The C# language (and possibly other languages) allows a generic type to access `Protected` members regardless of what type arguments are supplied.</span></span> <span data-ttu-id="0155f-373">Dies sollte beim Entwerfen generischer Klassen, die `Protected` Member enthalten, berücksichtigt werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-373">This should be kept in mind when designing generic classes that contain `Protected` members.</span></span>


### <a name="constituent-types"></a><span data-ttu-id="0155f-374">Konstituierende Typen</span><span class="sxs-lookup"><span data-stu-id="0155f-374">Constituent Types</span></span>

<span data-ttu-id="0155f-375">Die *einzelnen* Typen einer Deklaration sind die Typen, auf die von der Deklaration verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="0155f-375">The *constituent types* of a declaration are the types that are referenced by the declaration.</span></span> <span data-ttu-id="0155f-376">Der Typ einer Konstanten, der Rückgabetyp einer Methode und die Parametertypen eines Konstruktors sind z. b. alle konstituierenden Typen.</span><span class="sxs-lookup"><span data-stu-id="0155f-376">For example, the type of a constant, the return type of a method and the parameter types of a constructor are all constituent types.</span></span> <span data-ttu-id="0155f-377">Die Zugriffs Domäne eines eingebenden Typs einer Deklaration muss mit oder einer übergeordneten Zugriffs Domäne der Deklaration selbst identisch sein.</span><span class="sxs-lookup"><span data-stu-id="0155f-377">The accessibility domain of a constituent type of a declaration must be the same as or a superset of the accessibility domain of the declaration itself.</span></span> <span data-ttu-id="0155f-378">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-378">For example:</span></span>

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

## <a name="type-and-namespace-names"></a><span data-ttu-id="0155f-379">Typen-und Namespace Namen</span><span class="sxs-lookup"><span data-stu-id="0155f-379">Type and Namespace Names</span></span>

<span data-ttu-id="0155f-380">Viele Sprachkonstrukte erfordern, dass ein Namespace oder ein Typ angegeben wird. Diese können mithilfe eines qualifizierten Formulars des Namespace oder des Typnamens angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-380">Many language constructs require a namespace or type to be specified; these can be specified by using a qualified form of the namespace or type's name.</span></span> <span data-ttu-id="0155f-381">Ein *qualifizierter Name* besteht aus einer Reihe von Bezeichner, die durch Zeiträume getrennt sind. der Bezeichner auf der rechten Seite eines Zeitraums wird in dem Deklarations Bereich aufgelöst, der durch den Bezeichner auf der linken Seite des Zeitraums angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="0155f-381">A *qualified name* consists of a series of identifiers separated by periods; the identifier on the right side of a period is resolved in the declaration space specified by the identifier on the left side of the period.</span></span>

<span data-ttu-id="0155f-382">Der *voll qualifizierte Name* eines Namespaces oder Typs ist ein qualifizierter Name, der den Namen aller enthaltenden Namespaces und Typen enthält.</span><span class="sxs-lookup"><span data-stu-id="0155f-382">The *fully qualified name* of a namespace or type is a qualified name that contains the name of all containing namespaces and types.</span></span> <span data-ttu-id="0155f-383">Anders ausgedrückt: der voll qualifizierte Name eines Namespaces oder Typs ist `N.T`, wobei `T` der Name der Entität und `N` der voll qualifizierte Name der enthaltenden Entität ist.</span><span class="sxs-lookup"><span data-stu-id="0155f-383">In other words, the fully qualified name of a namespace or type is `N.T`, where `T` is the name of the entity and `N` is the fully qualified name of its containing entity.</span></span>

<span data-ttu-id="0155f-384">Das folgende Beispiel zeigt mehrere Namespace-und Typdeklarationen zusammen mit den zugehörigen voll qualifizierten Namen in Inline Kommentaren.</span><span class="sxs-lookup"><span data-stu-id="0155f-384">The example below shows several namespace and type declarations together with their associated fully qualified names in in-line comments.</span></span>

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

<span data-ttu-id="0155f-385">Beachten Sie, dass der Namespace x. y an zwei verschiedenen Speicherorten im Quellcode deklariert wurde. diese beiden partiellen Deklarationen stellen jedoch nur einen einzelnen Namespace mit dem Namen X. y dar, der sowohl die Klasse D als auch die Klasse E enthält.</span><span class="sxs-lookup"><span data-stu-id="0155f-385">Observe that the namespace X.Y has been declared in two different locations in the source code, but these two partial declarations constitute just a single namespace called X.Y which contains both class D and class E.</span></span>

<span data-ttu-id="0155f-386">In einigen Situationen kann ein qualifizierter Name mit dem Schlüsselwort `Global`beginnen.</span><span class="sxs-lookup"><span data-stu-id="0155f-386">In some situations, a qualified name may begin with the keyword `Global`.</span></span> <span data-ttu-id="0155f-387">Das Schlüsselwort stellt den unbenannten äußersten Namespace dar. Dies ist in Situationen nützlich, in denen eine Deklaration einen einschließenden Namespace Shadowing macht.</span><span class="sxs-lookup"><span data-stu-id="0155f-387">The keyword represents the unnamed outermost namespace, which is useful in situations where a declaration shadows an enclosing namespace.</span></span> <span data-ttu-id="0155f-388">Das `Global`-Schlüsselwort ermöglicht das "Escapezeichen" im äußersten Namespace in dieser Situation.</span><span class="sxs-lookup"><span data-stu-id="0155f-388">The `Global` keyword allows "escaping" out to the outermost namespace in that situation.</span></span> <span data-ttu-id="0155f-389">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-389">For example:</span></span>

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

<span data-ttu-id="0155f-390">Im obigen Beispiel ist der erste Methoden Aufrufwert ungültig, da der be`System` Zeichner an die-Klasse `System`und nicht an den Namespace `System`gebunden ist.</span><span class="sxs-lookup"><span data-stu-id="0155f-390">In the above example, the first method call is invalid because the identifier `System` binds to the class `System`, not the namespace `System`.</span></span> <span data-ttu-id="0155f-391">Die einzige Möglichkeit, auf den `System`-Namespace zuzugreifen, ist die Verwendung `Global`, um den äußersten Namespace zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="0155f-391">The only way to access the `System` namespace is to use `Global` to escape out to the outermost namespace.</span></span> <span data-ttu-id="0155f-392">`Global` kann nicht in einer `Imports` Anweisung oder `Namespace` Deklaration verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-392">`Global` cannot be used in an `Imports` statement or `Namespace` declaration.</span></span>

<span data-ttu-id="0155f-393">Da in anderen Sprachen Typen und Namespaces eingeführt werden können, die mit Schlüsselwörtern in der Sprache identisch sind, Visual Basic erkannt, dass Schlüsselwörter als Teil eines qualifizierten Namens erkannt werden, solange Sie einem bestimmten Zeitraum entsprechen.</span><span class="sxs-lookup"><span data-stu-id="0155f-393">Because other languages may introduce types and namespaces that match keywords in the language, Visual Basic recognizes keywords to be part of a qualified name as long as they follow a period.</span></span> <span data-ttu-id="0155f-394">Auf diese Weise verwendete Schlüsselwörter werden als Bezeichner behandelt.</span><span class="sxs-lookup"><span data-stu-id="0155f-394">Keywords used in this way are treated as identifiers.</span></span> <span data-ttu-id="0155f-395">Der qualifizierte Bezeichner `X.Default.Class` z. b. ein gültiger qualifizierter Bezeichner, während `Default.Class` nicht ist.</span><span class="sxs-lookup"><span data-stu-id="0155f-395">For example, the qualified identifier `X.Default.Class` is a valid qualified identifier, while `Default.Class` is not.</span></span>

### <a name="qualified-name-resolution-for-namespaces-and-types"></a><span data-ttu-id="0155f-396">Qualifizierte Namensauflösung für Namespaces und Typen</span><span class="sxs-lookup"><span data-stu-id="0155f-396">Qualified Name Resolution for namespaces and types</span></span>

<span data-ttu-id="0155f-397">Wenn ein qualifizierter Namespace-oder Typname der Form `N.R(Of A)`ist, wobei `R` der am weitesten rechts Bezeichner im qualifizierten Namen und `A` eine optionale Typargument Liste ist, wird in den folgenden Schritten beschrieben, wie Sie bestimmen, auf welchen Namespace oder Typ der qualifizierte Name verweist:</span><span class="sxs-lookup"><span data-stu-id="0155f-397">Given a qualified namespace or type name of the form `N.R(Of A)`, where `R` is the rightmost identifier in the qualified name and `A` is an optional type argument list, the following steps describe how to determine to which namespace or type the qualified name refers:</span></span>

1. <span data-ttu-id="0155f-398">Lösen Sie `N`mit den Regeln für die qualifizierte oder nicht qualifizierte Namensauflösung.</span><span class="sxs-lookup"><span data-stu-id="0155f-398">Resolve `N`, using the rules for either qualified or unqualified name resolution.</span></span>

2. <span data-ttu-id="0155f-399">Wenn die Auflösung von `N` fehlschlägt oder zu einem Typparameter aufgelöst wird, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="0155f-399">If resolution of `N` fails, or resolves to a type parameter, a compile-time error occurs.</span></span>

3. <span data-ttu-id="0155f-400">Andernfalls gilt: Wenn `R` dem Namen eines Namespaces in N entspricht und keine Typargumente angegeben wurden, oder `R` mit einem zugänglichen Typ in `N` mit derselben Anzahl von Typparametern wie Typargumenten übereinstimmt, verweist der qualifizierte Name auf diesen Namespace oder Typ.</span><span class="sxs-lookup"><span data-stu-id="0155f-400">Otherwise, if `R` matches the name of a namespace in N and no type arguments were supplied, or `R` matches an accessible type in `N` with the same number of type parameters as type arguments, if any, then the qualified name refers to that namespace or type.</span></span>

4. <span data-ttu-id="0155f-401">Andernfalls bezieht sich der qualifizierte Name auf diesen Typ, wenn `N` ein oder mehrere Standardmodule enthält und `R` mit dem Namen eines barrierefreien Typs identisch ist, der mit der gleichen Anzahl von Typparametern wie Typargumenten übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="0155f-401">Otherwise, if `N` contains one or more standard modules, and `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the qualified name refers to that type.</span></span> <span data-ttu-id="0155f-402">Wenn `R` mit dem Namen der zugänglichen Typen übereinstimmt, die dieselbe Anzahl von Typparametern wie Typargumente haben, tritt ggf. in mehr als einem Standardmodul ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="0155f-402">If `R` matches the name of accessible types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

5. <span data-ttu-id="0155f-403">Andernfalls tritt ein Kompilierungsfehler auf.</span><span class="sxs-lookup"><span data-stu-id="0155f-403">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="0155f-404">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="0155f-404">__Note.__</span></span> <span data-ttu-id="0155f-405">Eine Implikation dieses Auflösungsprozesses ist, dass Typmember beim Auflösen von Namespace-oder Typnamen keine Schatten-Namespaces oder-Typen verwenden.</span><span class="sxs-lookup"><span data-stu-id="0155f-405">An implication of this resolution process is that type members do not shadow namespaces or types when resolving namespace or type names.</span></span>

### <a name="unqualified-name-resolution-for-namespaces-and-types"></a><span data-ttu-id="0155f-406">Nicht qualifizierte Namensauflösung für Namespaces und Typen</span><span class="sxs-lookup"><span data-stu-id="0155f-406">Unqualified Name Resolution for namespaces and types</span></span>

<span data-ttu-id="0155f-407">Bei einem nicht qualifizierten Namen `R(Of A)`, bei dem `A` eine optionale Typargument Liste ist, wird in den folgenden Schritten beschrieben, wie Sie bestimmen, auf welchen Namespace oder Typ der nicht qualifizierte Name verweist:</span><span class="sxs-lookup"><span data-stu-id="0155f-407">Given an unqualified name `R(Of A)`, where `A` is an optional type argument list, the following steps describe how to determine to which namespace or type the unqualified name refers:</span></span>

1. <span data-ttu-id="0155f-408">Wenn R mit dem Namen eines Typparameters der aktuellen Methode übereinstimmt und keine Typargumente angegeben wurden, verweist der nicht qualifizierte Name auf diesen Typparameter.</span><span class="sxs-lookup"><span data-stu-id="0155f-408">If R matches the name of a type parameter of the current method, and no type arguments were supplied, then the unqualified name refers to that type parameter.</span></span>

2.  <span data-ttu-id="0155f-409">Für jeden Typ, der den Namen Verweis enthält, beginnend mit dem innersten Typ und dem äußersten:</span><span class="sxs-lookup"><span data-stu-id="0155f-409">For each nested type containing the name reference, starting from the innermost type and going to the outermost:</span></span>
    1. <span data-ttu-id="0155f-410">Wenn `R` dem Namen eines Typparameters im aktuellen Typ entspricht und keine Typargumente angegeben wurden, verweist der nicht qualifizierte Name auf diesen Typparameter.</span><span class="sxs-lookup"><span data-stu-id="0155f-410">If `R` matches the name of a type parameter in the current type and no type arguments were supplied, then the unqualified name refers to that type parameter.</span></span>
    2. <span data-ttu-id="0155f-411">Andernfalls bezieht sich der nicht qualifizierte Name auf diesen Typ, wenn `R` dem Namen eines zugreif baren, zugreif baren Typs mit der gleichen Anzahl von Typparametern wie Typargumenten entspricht.</span><span class="sxs-lookup"><span data-stu-id="0155f-411">Otherwise, if `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, then the unqualified name refers to that type.</span></span>

3. <span data-ttu-id="0155f-412">Für jeden eingefügten Namespace, der den Namen Verweis enthält, beginnend mit dem innersten Namespace und durchgehen zum äußersten Namespace:</span><span class="sxs-lookup"><span data-stu-id="0155f-412">For each nested namespace containing the name reference, starting from the innermost namespace and going to the outermost namespace:</span></span>
    1. <span data-ttu-id="0155f-413">Wenn `R` im aktuellen Namespace mit dem Namen eines im aktuellen Namespace übereinstimmt und keine Typargument Liste angegeben ist, bezieht sich der nicht qualifizierte Name auf diesen schsted Namespace.</span><span class="sxs-lookup"><span data-stu-id="0155f-413">If `R` matches the name of a nested namespace in the current namespace and no type argument list is supplied, then the unqualified name refers to that nested namespace.</span></span>
    2. <span data-ttu-id="0155f-414">Andernfalls bezieht sich der nicht qualifizierte Name auf diesen Typ, wenn `R` dem Namen eines barrierefreien Typs mit der gleichen Anzahl von Typparametern als Typargumente entspricht (sofern vorhanden).</span><span class="sxs-lookup"><span data-stu-id="0155f-414">Otherwise, if `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in the current namespace, then the unqualified name refers to that type.</span></span>
    3. <span data-ttu-id="0155f-415">Andernfalls bezieht sich der nicht qualifizierte Name, wenn der Namespace mindestens ein zugreif bares Standardmodul enthält, und `R` dem Namen eines zugreif baren, nicht qualifizierten Typs mit der gleichen Anzahl von Typparametern wie Typargumenten, sofern vorhanden, in genau einem Standardmodul.</span><span class="sxs-lookup"><span data-stu-id="0155f-415">Otherwise, if the namespace contains one or more accessible standard modules, and `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the unqualified name refers to that nested type.</span></span> <span data-ttu-id="0155f-416">Wenn `R` den Namen von zugreif baren, zugreif baren Typen mit der gleichen Anzahl von Typparametern wie Typargumenten, sofern vorhanden, in mehr als einem Standardmodul abgleicht, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="0155f-416">If `R` matches the name of accessible nested types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

4. <span data-ttu-id="0155f-417">Wenn die Quelldatei mindestens einen Alias Alias aufweist und `R` mit dem Namen einer dieser Dateien übereinstimmt, verweist der nicht qualifizierte Name auf diesen importieralias.</span><span class="sxs-lookup"><span data-stu-id="0155f-417">If the source file has one or more import aliases, and `R` matches the name of one of them, then the unqualified name refers to that import alias.</span></span> <span data-ttu-id="0155f-418">Wenn eine Typargument Liste angegeben wird, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="0155f-418">If a type argument list is supplied, a compile-time error occurs.</span></span>

5. <span data-ttu-id="0155f-419">Wenn die Quelldatei, die den Namen Verweis enthält, mindestens einen Import hat:</span><span class="sxs-lookup"><span data-stu-id="0155f-419">If the source file containing the name reference has one or more imports:</span></span>
    1. <span data-ttu-id="0155f-420">Wenn `R` dem Namen eines barrierefreien Typs mit der gleichen Anzahl von Typparametern wie Typargumenten entspricht (sofern vorhanden), bezieht sich der nicht qualifizierte Name auf diesen Typ.</span><span class="sxs-lookup"><span data-stu-id="0155f-420">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in exactly one import, then the unqualified name refers to that type.</span></span> <span data-ttu-id="0155f-421">Wenn `R` dem Namen eines barrierefreien Typs mit der gleichen Anzahl von Typparametern wie Typargumenten entspricht (sofern vorhanden), tritt bei mehr als einem Import und bei allen nicht der gleiche Typ auf. ein Kompilierzeitfehler tritt auf.</span><span class="sxs-lookup"><span data-stu-id="0155f-421">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in more than one import and all are not the same type, a compile-time error occurs.</span></span>
    2. <span data-ttu-id="0155f-422">Andernfalls bezieht sich der nicht qualifizierte Name auf diesen Namespace, wenn keine Typargument Liste angegeben wurde und `R` mit dem Namen eines Namespaces mit barrierefreien Typen in genau einem Import übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="0155f-422">Otherwise, if no type argument list was supplied and `R` matches the name of a namespace with accessible types in exactly one import, then the unqualified name refers to that namespace.</span></span> <span data-ttu-id="0155f-423">Wenn keine Typargument Liste angegeben wurde und `R` mit dem Namen eines Namespaces mit barrierefreien Typen in mehr als einem Import übereinstimmt und alle nicht denselben Namespace haben, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="0155f-423">If no type argument list was supplied and `R` matches the name of a namespace with accessible types in more than one import and all are not the same namespace, a compile-time error occurs.</span></span>
    3. <span data-ttu-id="0155f-424">Wenn die Importe ein oder mehrere barrierefreie Standardmodule enthalten und `R` mit dem Namen eines zugreif baren, zugreif baren Typs mit der gleichen Anzahl von Typparametern wie Typargumente übereinstimmt (sofern vorhanden), bezieht sich der nicht qualifizierte Name in genau einem Standardmodul auf diesen Typ.</span><span class="sxs-lookup"><span data-stu-id="0155f-424">Otherwise, if the imports contain one or more accessible standard modules, and `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the unqualified name refers to that type.</span></span> <span data-ttu-id="0155f-425">Wenn `R` den Namen von zugreif baren, zugreif baren Typen mit der gleichen Anzahl von Typparametern wie Typargumenten, sofern vorhanden, in mehr als einem Standardmodul abgleicht, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="0155f-425">If `R` matches the name of accessible nested types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

6. <span data-ttu-id="0155f-426">Wenn in der Kompilierungs Umgebung mindestens eine Import-Aliase definiert ist und `R` mit dem Namen einer dieser Elemente übereinstimmt, verweist der nicht qualifizierte Name auf diesen importieralias.</span><span class="sxs-lookup"><span data-stu-id="0155f-426">If the compilation environment defines one or more import aliases, and `R` matches the name of one of them, then the unqualified name refers to that import alias.</span></span> <span data-ttu-id="0155f-427">Wenn eine Typargument Liste angegeben wird, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="0155f-427">If a type argument list is supplied, a compile-time error occurs.</span></span>

7. <span data-ttu-id="0155f-428">Wenn in der Kompilierungs Umgebung mindestens ein Import definiert ist:</span><span class="sxs-lookup"><span data-stu-id="0155f-428">If the compilation environment defines one or more imports:</span></span>
    1. <span data-ttu-id="0155f-429">Wenn `R` dem Namen eines barrierefreien Typs mit der gleichen Anzahl von Typparametern wie Typargumenten entspricht (sofern vorhanden), bezieht sich der nicht qualifizierte Name auf diesen Typ.</span><span class="sxs-lookup"><span data-stu-id="0155f-429">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in exactly one import, then the unqualified name refers to that type.</span></span> <span data-ttu-id="0155f-430">Wenn `R` dem Namen eines barrierefreien Typs mit der gleichen Anzahl von Typparametern wie Typargumenten entspricht (sofern vorhanden), tritt in mehr als einem Import ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="0155f-430">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in more than one import, a compile-time error occurs.</span></span>
    2. <span data-ttu-id="0155f-431">Andernfalls bezieht sich der nicht qualifizierte Name auf diesen Namespace, wenn keine Typargument Liste angegeben wurde und `R` mit dem Namen eines Namespaces mit barrierefreien Typen in genau einem Import übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="0155f-431">Otherwise, if no type argument list was supplied and `R` matches the name of a namespace with accessible types in exactly one import, then the unqualified name refers to that namespace.</span></span> <span data-ttu-id="0155f-432">Wenn keine Typargument Liste angegeben wurde und `R` dem Namen eines Namespaces mit barrierefreien Typen in mehr als einem Import entspricht, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="0155f-432">If no type argument list was supplied and `R` matches the name of a namespace with accessible types in more than one import, a compile-time error occurs.</span></span>
    3. <span data-ttu-id="0155f-433">Wenn die Importe ein oder mehrere barrierefreie Standardmodule enthalten und `R` mit dem Namen eines zugreif baren, zugreif baren Typs mit der gleichen Anzahl von Typparametern wie Typargumente übereinstimmt (sofern vorhanden), bezieht sich der nicht qualifizierte Name in genau einem Standardmodul auf diesen Typ.</span><span class="sxs-lookup"><span data-stu-id="0155f-433">Otherwise, if the imports contain one or more accessible standard modules, and `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the unqualified name refers to that type.</span></span> <span data-ttu-id="0155f-434">Wenn `R` den Namen von zugreif baren, zugreif baren Typen mit der gleichen Anzahl von Typparametern wie Typargumenten, sofern vorhanden, in mehr als einem Standardmodul abgleicht, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="0155f-434">If `R` matches the name of accessible nested types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

8. <span data-ttu-id="0155f-435">Andernfalls tritt ein Kompilierungsfehler auf.</span><span class="sxs-lookup"><span data-stu-id="0155f-435">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="0155f-436">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="0155f-436">__Note.__</span></span> <span data-ttu-id="0155f-437">Eine Implikation dieses Auflösungsprozesses ist, dass Typmember beim Auflösen von Namespace-oder Typnamen keine Schatten-Namespaces oder-Typen verwenden.</span><span class="sxs-lookup"><span data-stu-id="0155f-437">An implication of this resolution process is that type members do not shadow namespaces or types when resolving namespace or type names.</span></span>

<span data-ttu-id="0155f-438">Normalerweise kann ein Name nur einmal in einem bestimmten Namespace vorkommen.</span><span class="sxs-lookup"><span data-stu-id="0155f-438">Normally, a name can only occur once in a particular namespace.</span></span> <span data-ttu-id="0155f-439">Da Namespaces jedoch über mehrere .NET-Assemblys hinweg deklariert werden können, kann es vorkommen, dass zwei Assemblys einen Typ mit demselben voll qualifizierten Namen definieren.</span><span class="sxs-lookup"><span data-stu-id="0155f-439">However, because namespaces can be declared across multiple .NET assemblies, it is possible to have a situation where two assemblies define a type with the same fully qualified name.</span></span> <span data-ttu-id="0155f-440">In diesem Fall wird ein Typ, der im aktuellen Satz von Quelldateien deklariert ist, von einem in einer externen .NET-Assembly deklarierten Typ bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="0155f-440">In that case, a type declared in the current set of source files is preferred over a type declared in an external .NET assembly.</span></span> <span data-ttu-id="0155f-441">Andernfalls ist der Name mehrdeutig, und es gibt keine Möglichkeit, den Namen zu unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="0155f-441">Otherwise, the name is ambiguous and there is no way to disambiguate the name.</span></span>

## <a name="variables"></a><span data-ttu-id="0155f-442">Variablen</span><span class="sxs-lookup"><span data-stu-id="0155f-442">Variables</span></span>

<span data-ttu-id="0155f-443">Eine *Variable* stellt einen Speicherort dar.</span><span class="sxs-lookup"><span data-stu-id="0155f-443">A *variable* represents a storage location.</span></span> <span data-ttu-id="0155f-444">Jede Variable verfügt über einen Typ, der bestimmt, welche Werte in der Variablen gespeichert werden können.</span><span class="sxs-lookup"><span data-stu-id="0155f-444">Every variable has a type that determines what values can be stored in the variable.</span></span> <span data-ttu-id="0155f-445">Da Visual Basic eine typsichere Sprache ist, weist jede Variable in einem Programm einen-Typ auf, und die-Sprache stellt sicher, dass in Variablen gespeicherte Werte immer den entsprechenden Typ haben.</span><span class="sxs-lookup"><span data-stu-id="0155f-445">Because Visual Basic is a type-safe language, every variable in a program has a type and the language guarantees that values stored in variables are always of the appropriate type.</span></span> <span data-ttu-id="0155f-446">Variablen werden immer mit dem Standardwert ihres Typs initialisiert, bevor ein Verweis auf die Variable erstellt werden kann.</span><span class="sxs-lookup"><span data-stu-id="0155f-446">Variables are always initialized to the default value of their type before any reference to the variable can be made.</span></span> <span data-ttu-id="0155f-447">Es ist nicht möglich, auf nicht initialisierten Speicher zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="0155f-447">It is not possible to access uninitialized memory.</span></span>

## <a name="generic-types-and-methods"></a><span data-ttu-id="0155f-448">Generische Typen und Methoden</span><span class="sxs-lookup"><span data-stu-id="0155f-448">Generic Types and Methods</span></span>

<span data-ttu-id="0155f-449">Typen (außer bei Standardmodulen und Enumerationstypen) und Methoden können *Typparameter*deklarieren, bei denen es sich um Typen handelt, die nicht bereitgestellt werden, bis eine Instanz des Typs deklariert oder die-Methode aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="0155f-449">Types (except for standard modules and enumerated types) and methods can declare *type parameters*, which are types that will not be provided until an instance of the type is declared or the method is invoked.</span></span> <span data-ttu-id="0155f-450">Typen und Methoden mit Typparametern werden auch als *generische Typen* und *generische Methoden*bezeichnet, da der Typ oder die Methode generisch geschrieben werden muss, ohne dass bestimmte Kenntnisse der Typen vorliegen, die von Code bereitgestellt werden, der den Typ oder die Methode verwendet.</span><span class="sxs-lookup"><span data-stu-id="0155f-450">Types and methods with type parameters are also known as *generic types* and *generic methods*, respectively, because the type or method must be written generically, without specific knowledge of the types that will be supplied by code that uses the type or method.</span></span>

<span data-ttu-id="0155f-451">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="0155f-451">__Note.__</span></span> <span data-ttu-id="0155f-452">Obwohl Methoden und Delegaten generisch sein können, können Eigenschaften, Ereignisse und Operatoren zu diesem Zeitpunkt nicht generisch sein.</span><span class="sxs-lookup"><span data-stu-id="0155f-452">At this time, even though methods and delegates can be generic, properties, events and operators cannot be generic themselves.</span></span> <span data-ttu-id="0155f-453">Sie können jedoch Typparameter der enthaltenden Klasse verwenden.</span><span class="sxs-lookup"><span data-stu-id="0155f-453">They may, however, use type parameters from the containing class.</span></span>

<span data-ttu-id="0155f-454">Aus der Perspektive des generischen Typs oder der generischen Methode ist ein Typparameter ein Platzhalter, der mit einem tatsächlichen Typ ausgefüllt wird, wenn der Typ oder die Methode verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="0155f-454">From the perspective of the generic type or method, a type parameter is a placeholder type that will be filled in with an actual type when the type or method is used.</span></span> <span data-ttu-id="0155f-455">Typargumente ersetzen an dem Punkt, an dem der Typ oder die Methode verwendet wird, die Typparameter in dem Typ oder der Methode.</span><span class="sxs-lookup"><span data-stu-id="0155f-455">Type arguments are substituted for the type parameters in the type or method at the point at which the type or method is used.</span></span> <span data-ttu-id="0155f-456">Eine generische Stapel Klasse könnte z. b. wie folgt implementiert werden:</span><span class="sxs-lookup"><span data-stu-id="0155f-456">For example, a generic stack class could be implemented as:</span></span>

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

<span data-ttu-id="0155f-457">Deklarationen, die die `Stack(Of ItemType)`-Klasse verwenden, müssen ein Typargument für den Typparameter `ItemType`bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="0155f-457">Declarations that use the `Stack(Of ItemType)` class must supply a type argument for the type parameter `ItemType`.</span></span> <span data-ttu-id="0155f-458">Dieser Typ wird dann ausgefüllt, wenn `ItemType` in der-Klasse verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="0155f-458">This type is then filled in wherever `ItemType` is used within the class:</span></span>

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

### <a name="type-parameters"></a><span data-ttu-id="0155f-459">Typparameter</span><span class="sxs-lookup"><span data-stu-id="0155f-459">Type Parameters</span></span>

<span data-ttu-id="0155f-460">Typparameter können in Typ-oder Methoden Deklarationen angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-460">Type parameters may be supplied on type or method declarations.</span></span> <span data-ttu-id="0155f-461">Jeder Typparameter ist ein Bezeichner, der ein Platzhalter für ein Typargument ist, das zum Erstellen eines konstruierten Typs oder einer Methode bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="0155f-461">Each type parameter is an identifier which is a place-holder for a type argument that is supplied to create a constructed type or method.</span></span> <span data-ttu-id="0155f-462">Im Gegensatz dazu ist ein Typargument der tatsächliche Typ, der den Typparameter ersetzt, wenn ein generischer Typ oder eine generische Methode verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="0155f-462">By contrast, a type argument is the actual type that is substituted for the type parameter when a generic type or method is used.</span></span>

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

<span data-ttu-id="0155f-463">Jeder Typparameter in einer Typ-oder Methoden Deklaration definiert einen Namen im Deklarations Raum dieses Typs oder dieser Methode.</span><span class="sxs-lookup"><span data-stu-id="0155f-463">Each type parameter in a type or method declaration defines a name in the declaration space of that type or method.</span></span> <span data-ttu-id="0155f-464">Daher kann er nicht denselben Namen wie ein anderer Typparameter, ein Typmember, ein Methoden Parameter oder eine lokale Variable haben.</span><span class="sxs-lookup"><span data-stu-id="0155f-464">Thus, it cannot have the same name as another type parameter, a type member, a method parameter, or a local variable.</span></span> <span data-ttu-id="0155f-465">Der Gültigkeitsbereich eines Typparameters für einen Typ oder eine Methode ist der gesamte Typ oder die Methode.</span><span class="sxs-lookup"><span data-stu-id="0155f-465">The scope of a type parameter on a type or method is the entire type or method.</span></span> <span data-ttu-id="0155f-466">Da Typparameter auf die gesamte Typdeklaration festgelegt sind, können für die Typen der äußeren Typparameter verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-466">Because type parameters are scoped to the entire type declaration, nested types can use outer type parameters.</span></span> <span data-ttu-id="0155f-467">Dies bedeutet auch, dass Typparameter immer beim Zugriff auf Typen angegeben werden müssen, die in generischen Typen geschachtelt sind:</span><span class="sxs-lookup"><span data-stu-id="0155f-467">This also means that type parameters must always be specified when accessing types nested inside generic types:</span></span>

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

<span data-ttu-id="0155f-468">Im Gegensatz zu anderen Membern einer Klasse werden Typparameter nicht geerbt.</span><span class="sxs-lookup"><span data-stu-id="0155f-468">Unlike other members of a class, type parameters are not inherited.</span></span> <span data-ttu-id="0155f-469">Auf Typparameter in einem Typ kann nur mit dem einfachen Namen verwiesen werden. mit anderen Worten, Sie können nicht mit dem enthaltenden Typnamen qualifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-469">Type parameters in a type can only be referred to by their simple name; in other words, they cannot be qualified with the containing type name.</span></span> <span data-ttu-id="0155f-470">Obwohl es sich um einen ungültigen Programmierstil handelt, können die Typparameter in einem Schraffurtyp einen Member oder Typparameter ausblenden, der im äußeren Typ deklariert ist:</span><span class="sxs-lookup"><span data-stu-id="0155f-470">Although it is bad programming style, the type parameters in a nested type can hide a member or type parameter declared in the outer type:</span></span>

```vb
Class Outer(Of T)
    Class Inner(Of T)
        Public t1 As T    ' Refers to Inner's T
    End Class
End Class
```

<span data-ttu-id="0155f-471">Typen und Methoden können auf Grundlage der Anzahl von Typparametern (oder der *Arität*), die von den Typen oder Methoden deklariert werden, überladen werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-471">Types and methods may be overloaded based on the number of type parameters (or *arity*) that the types or methods declare.</span></span> <span data-ttu-id="0155f-472">Beispielsweise sind die folgenden Deklarationen zulässig:</span><span class="sxs-lookup"><span data-stu-id="0155f-472">For example, the following declarations are legal:</span></span>

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

<span data-ttu-id="0155f-473">Im Fall von Typen werden über Ladungen immer mit der Anzahl der angegebenen Typargumente abgeglichen.</span><span class="sxs-lookup"><span data-stu-id="0155f-473">In the case of types, overloads are always matched against the number of type arguments specified.</span></span> <span data-ttu-id="0155f-474">Dies ist nützlich, wenn sowohl generische als auch nicht generische Klassen im gleichen Programm verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="0155f-474">This is useful when using both generic and non-generic classes together in the same program:</span></span>

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

<span data-ttu-id="0155f-475">Regeln für Methoden, die für Typparameter überladen werden, werden im Abschnitt zur Auflösung von Methoden Überladungen behandelt.</span><span class="sxs-lookup"><span data-stu-id="0155f-475">Rules for methods overloaded on type parameters are covered in the section on method overload resolution.</span></span>

<span data-ttu-id="0155f-476">In der enthaltenden Deklaration werden Typparameter als vollständige Typen angesehen.</span><span class="sxs-lookup"><span data-stu-id="0155f-476">Within the containing declaration, type parameters are considered full types.</span></span> <span data-ttu-id="0155f-477">Da ein Typparameter mit vielen verschiedenen tatsächlichen Typargumenten instanziiert werden kann, haben Typparameter etwas andere Vorgänge und Einschränkungen als andere Typen, wie unten beschrieben:</span><span class="sxs-lookup"><span data-stu-id="0155f-477">Since a type parameter can be instantiated with many different actual type arguments, type parameters have slightly different operations and restrictions than other types as described below:</span></span>

* <span data-ttu-id="0155f-478">Ein Typparameter kann nicht direkt verwendet werden, um eine Basisklasse oder Schnittstelle zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="0155f-478">A type parameter cannot be used directly to declare a base class or interface.</span></span>

* <span data-ttu-id="0155f-479">Die Regeln für die Element Suche für Typparameter hängen von den Einschränkungen ab, die ggf. auf den Typparameter angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-479">The rules for member lookup on type parameters depend on the constraints, if any, applied to the type parameter.</span></span>

* <span data-ttu-id="0155f-480">Die verfügbaren Konvertierungen für einen Typparameter hängen von den Einschränkungen ab, die ggf. auf die Typparameter angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-480">The available conversions for a type parameter depend on the constraints, if any, applied to the type parameters.</span></span>

* <span data-ttu-id="0155f-481">Wenn keine `Structure` Einschränkung vorhanden ist, kann ein Wert mit einem Typ, der durch einen Typparameter dargestellt wird, mit `Nothing` mithilfe von `Is` und `IsNot`verglichen werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-481">In the absence of a `Structure` constraint, a value with a type represented by a type parameter can be compared with `Nothing` using `Is` and `IsNot`.</span></span>

* <span data-ttu-id="0155f-482">Ein Typparameter kann nur in einem `New` Ausdruck verwendet werden, wenn der Typparameter durch eine `New` oder eine `Structure` Einschränkung eingeschränkt wird.</span><span class="sxs-lookup"><span data-stu-id="0155f-482">A type parameter can only be used in a `New` expression if the type parameter is constrained by a `New` or a `Structure` constraint.</span></span>

* <span data-ttu-id="0155f-483">Ein Typparameter kann nicht an einer beliebigen Stelle innerhalb einer Attribut Ausnahme innerhalb eines `GetType` Ausdrucks verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-483">A type parameter cannot be used anywhere within an attribute exception within a `GetType` expression.</span></span>

* <span data-ttu-id="0155f-484">Typparameter können als Typargumente für andere generische Typen und Parameter verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-484">Type parameters can be used as type arguments to other generic types and parameters.</span></span>

<span data-ttu-id="0155f-485">Das folgende Beispiel ist ein generischer Typ, der die `Stack(Of ItemType)`-Klasse erweitert:</span><span class="sxs-lookup"><span data-stu-id="0155f-485">The following example is a generic type that extends the `Stack(Of ItemType)` class:</span></span>

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

<span data-ttu-id="0155f-486">Wenn eine Deklaration ein Typargument für `MyStack`bereitstellt, wird dasselbe Typargument auch auf `Stack` angewendet.</span><span class="sxs-lookup"><span data-stu-id="0155f-486">When a declaration supplies a type argument to `MyStack`, the same type argument will be applied to `Stack` as well.</span></span>

<span data-ttu-id="0155f-487">Typparameter sind ein reines Kompilierzeit Konstrukt.</span><span class="sxs-lookup"><span data-stu-id="0155f-487">As a type, type parameters are purely a compile-time construct.</span></span> <span data-ttu-id="0155f-488">Zur Laufzeit wird jeder Typparameter an einen Lauf Zeittyp gebunden, der durch Bereitstellen eines Typarguments an die generische Deklaration angegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="0155f-488">At run-time, each type parameter is bound to a run-time type that was specified by supplying a type argument to the generic declaration.</span></span> <span data-ttu-id="0155f-489">Daher ist der Typ einer Variablen, die mit einem Typparameter deklariert wird, zur Laufzeit ein nicht generischer Typ oder ein bestimmter konstruierter Typ.</span><span class="sxs-lookup"><span data-stu-id="0155f-489">Thus, the type of a variable declared with a type parameter will, at run-time, be a non-generic type or a specific constructed type.</span></span> <span data-ttu-id="0155f-490">Die Lauf Zeit Ausführung aller Anweisungen und Ausdrücke, die Typparameter betreffen, verwendet den eigentlichen Typ, der als Typargument für diesen Parameter angegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="0155f-490">The run-time execution of all statements and expressions involving type parameters uses the actual type that was supplied as the type argument for that parameter.</span></span>


### <a name="type-constraints"></a><span data-ttu-id="0155f-491">Typeinschränkungen</span><span class="sxs-lookup"><span data-stu-id="0155f-491">Type Constraints</span></span>

<span data-ttu-id="0155f-492">Da ein Typargument ein beliebiger Typ im Typsystem sein kann, kann ein generischer Typ oder eine generische Methode keine Annahmen über einen Typparameter treffen.</span><span class="sxs-lookup"><span data-stu-id="0155f-492">Because a type argument can be any type in the type system, a generic type or method cannot make any assumptions about a type parameter.</span></span> <span data-ttu-id="0155f-493">Folglich werden die Member eines Typparameters als Member des Typs `Object`betrachtet, da alle Typen von `Object`abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-493">Thus, the members of a type parameter are considered to be the members of the type `Object`, since all types derive from `Object`.</span></span>

<span data-ttu-id="0155f-494">Im Fall einer Auflistung wie `Stack(Of ItemType)`ist diese Tatsache möglicherweise keine besonders wichtige Einschränkung, aber es kann vorkommen, dass ein generischer Typ eine Annahme über die Typen machen möchte, die als Typargumente bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-494">In the case of a collection like `Stack(Of ItemType)`, this fact may not be a particularly important restriction, but there may be cases where a generic type may wish to make an assumption about the types that will be supplied as type arguments.</span></span> <span data-ttu-id="0155f-495">*Typeinschränkungen* können für Typparameter festgelegt werden, die einschränken, welche Typen als Typparameter bereitgestellt werden können, und generischen Typen oder Methoden ermöglichen, mehr über Typparameter zu übernehmen.</span><span class="sxs-lookup"><span data-stu-id="0155f-495">*Type constraints* can be placed on type parameters that restrict which types can be supplied as a type parameter and allow generic types or methods to assume more about type parameters.</span></span>

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

<span data-ttu-id="0155f-496">In diesem Beispiel schränkt der `DisposableStack(Of ItemType)` den Typparameter auf Typen ein, die die-Schnittstelle implementieren `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="0155f-496">In this example, the `DisposableStack(Of ItemType)` constrains its type parameter to only types that implement the interface `System.IDisposable`.</span></span> <span data-ttu-id="0155f-497">Folglich kann eine `Dispose` Methode implementiert werden, die alle in der Warteschlange verbleibenden Objekte freigibt.</span><span class="sxs-lookup"><span data-stu-id="0155f-497">As a result, it can implement a `Dispose` method that disposes any objects still left in the queue.</span></span>

<span data-ttu-id="0155f-498">Eine Typeinschränkung muss eine der besonderen Einschränkungen `Class`, `Structure`oder `New`sein, oder es muss sich um einen Typ handeln `T` in dem Folgendes gilt:</span><span class="sxs-lookup"><span data-stu-id="0155f-498">A type constraint must be one of the special constraints `Class`, `Structure`, or `New`, or it must be a type `T` where:</span></span>

* <span data-ttu-id="0155f-499">`T` ist eine Klasse, eine Schnittstelle oder ein Typparameter.</span><span class="sxs-lookup"><span data-stu-id="0155f-499">`T` is a class, an interface, or a type parameter.</span></span>

* <span data-ttu-id="0155f-500">`T` ist nicht `NotInheritable`.</span><span class="sxs-lookup"><span data-stu-id="0155f-500">`T` is not `NotInheritable`.</span></span>

* <span data-ttu-id="0155f-501">`T` ist weder eine von noch ein Typ, der von einem von geerbt wurde, die folgenden speziellen Typen: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`oder `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="0155f-501">`T` is not one of, or a type inherited from one of, the following special types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span>

* <span data-ttu-id="0155f-502">`T` ist nicht `Object`.</span><span class="sxs-lookup"><span data-stu-id="0155f-502">`T` is not `Object`.</span></span> <span data-ttu-id="0155f-503">Da alle Typen von `Object`abgeleitet sind, hätte eine solche Einschränkung keine Auswirkung, wenn Sie zulässig wäre.</span><span class="sxs-lookup"><span data-stu-id="0155f-503">Since all types derive from `Object`, such a constraint would have no effect if it were permitted.</span></span>

* <span data-ttu-id="0155f-504">`T` muss mindestens so zugänglich sein, wie der generische Typ oder die Methode, die deklariert wird.</span><span class="sxs-lookup"><span data-stu-id="0155f-504">`T` must be at least as accessible as the generic type or method being declared.</span></span>

<span data-ttu-id="0155f-505">Mehrere Typeinschränkungen können für einen einzelnen Typparameter angegeben werden, indem die Typeinschränkungen in geschweiften Klammern (`{}`) eingeschlossen werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-505">Multiple type constraints can be specified for a single type parameter by enclosing the type constraints in curly braces (`{}`)..</span></span> <span data-ttu-id="0155f-506">Nur eine Typeinschränkung für einen angegebenen Typparameter kann eine Klasse sein.</span><span class="sxs-lookup"><span data-stu-id="0155f-506">Only one type constraint for a given type parameter can be a class.</span></span> <span data-ttu-id="0155f-507">Es ist ein Fehler, eine `Structure` besondere Einschränkung mit einer benannten Klassen Einschränkung oder der `Class` speziellen Einschränkung zu kombinieren.</span><span class="sxs-lookup"><span data-stu-id="0155f-507">It is an error to combine a `Structure` special constraint with a named class constraint or the `Class` special constraint.</span></span>

```vb
Class ControlFactory(Of T As {Control, New})
    ...
End Class
```

<span data-ttu-id="0155f-508">Typeinschränkungen können die enthaltenden Typen oder die Typparameter der enthaltenden Typen verwenden.</span><span class="sxs-lookup"><span data-stu-id="0155f-508">Type constraints can use the containing types or any of the containing types' type parameters.</span></span> <span data-ttu-id="0155f-509">Im folgenden Beispiel erfordert die-Einschränkung, dass das angegebene Typargument eine generische Schnittstelle implementiert, die sich selbst als Typargument verwendet:</span><span class="sxs-lookup"><span data-stu-id="0155f-509">In the following example, the constraint requires that the type argument supplied implements a generic interface using itself as a type argument:</span></span>

```vb
Class Sorter(Of V As IComparable(Of V))
    ...
End Class
```

<span data-ttu-id="0155f-510">Die besondere Typeinschränkung `Class` das angegebene Typargument auf einen beliebigen Verweistyp beschränkt.</span><span class="sxs-lookup"><span data-stu-id="0155f-510">The special type constraint `Class` constrains the supplied type argument to any reference type.</span></span>

<span data-ttu-id="0155f-511">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="0155f-511">__Note.__</span></span> <span data-ttu-id="0155f-512">Die besondere typeinschränkungs `Class` kann durch eine Schnittstelle erfüllt werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-512">The special type constraint `Class` can be satisfied by an interface.</span></span> <span data-ttu-id="0155f-513">Und eine-Struktur kann eine Schnittstelle implementieren.</span><span class="sxs-lookup"><span data-stu-id="0155f-513">And a structure can implement an interface.</span></span> <span data-ttu-id="0155f-514">Daher kann die Einschränkungs `(Of T As U, U As Class)` mit "t" einer Struktur erfüllt werden (die die besondere Einschränkung von `Class` nicht erfüllt), und "U" ist eine Schnittstelle, die Sie implementiert (was die `Class` besondere Einschränkung erfüllt).</span><span class="sxs-lookup"><span data-stu-id="0155f-514">Therefore, the constraint `(Of T As U, U As Class)` might be satisfied with "T" a structure (which does not satisfy the `Class` special constraint), and "U" an interface that it implements (which does satisfy the `Class` special constraint).</span></span>

<span data-ttu-id="0155f-515">Die besondere Typeinschränkung `Structure` das angegebene Typargument auf einen beliebigen Werttyp außer `System.Nullable(Of T)`beschränkt.</span><span class="sxs-lookup"><span data-stu-id="0155f-515">The special type constraint `Structure` constrains the supplied type argument to any value type except `System.Nullable(Of T)`.</span></span>

<span data-ttu-id="0155f-516">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="0155f-516">__Note.__</span></span> <span data-ttu-id="0155f-517">Struktur Einschränkungen lassen keine `System.Nullable(Of T)` zu, sodass es nicht möglich ist, `System.Nullable(Of T)` als Typargument für sich selbst bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="0155f-517">Structure constraints do not allow `System.Nullable(Of T)` so that it is not possible to supply `System.Nullable(Of T)` as a type argument to itself.</span></span>

<span data-ttu-id="0155f-518">Die besondere typeinschränkungs `New` erfordert, dass das angegebene Typargument über einen zugänglichen Parameter losen Konstruktor verfügen muss und nicht als `MustInherit`deklariert werden kann.</span><span class="sxs-lookup"><span data-stu-id="0155f-518">The special type constraint `New` requires that the supplied type argument must have an accessible parameterless constructor and cannot be declared `MustInherit`.</span></span> <span data-ttu-id="0155f-519">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-519">For example:</span></span>

```vb
Class Factory(Of T As New)
    Function CreateInstance() As T
        Return New T()
    End Function
End Class
```

<span data-ttu-id="0155f-520">Eine Klassentyp Einschränkung erfordert, dass das angegebene Typargument entweder den Typ oder davon erben muss.</span><span class="sxs-lookup"><span data-stu-id="0155f-520">A class type constraint requires that the supplied type argument must either be that type as or inherit from it.</span></span> <span data-ttu-id="0155f-521">Eine Schnittstellentyp Einschränkung erfordert, dass das angegebene Typargument diese Schnittstelle implementieren muss.</span><span class="sxs-lookup"><span data-stu-id="0155f-521">An interface type constraint requires that the supplied type argument must implement that interface.</span></span> <span data-ttu-id="0155f-522">Eine Typparameter Einschränkung erfordert, dass das angegebene Typargument von abgeleitet ist oder alle für den übereinstimmenden Typparameter angegebenen Begrenzungen implementiert.</span><span class="sxs-lookup"><span data-stu-id="0155f-522">A type parameter constraint requires that the supplied type argument must derive from or implement all of the bounds given for the matching type parameter.</span></span> <span data-ttu-id="0155f-523">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-523">For example:</span></span>

```vb
Class List(Of T)
    Sub AddRange(Of S As T)(collection As IEnumerable(Of S))
        ...
    End Sub
End Class
```

<span data-ttu-id="0155f-524">In diesem Beispiel wird der Typparameter `S` auf `AddRange` auf den Typparameter `T` `List`beschränkt.</span><span class="sxs-lookup"><span data-stu-id="0155f-524">In this example, the type parameter `S` on `AddRange` is constrained to the type parameter `T` of `List`.</span></span> <span data-ttu-id="0155f-525">Dies bedeutet, dass ein `List(Of Control)` `AddRange`den Typparameter auf jeden Typ einschränkt, der von `Control`erbt.</span><span class="sxs-lookup"><span data-stu-id="0155f-525">This means that a `List(Of Control)` would constrain `AddRange`'s type parameter to any type that is or inherits from `Control`.</span></span>

<span data-ttu-id="0155f-526">Eine Typparameter Einschränkung `Of S As T` durch transitiv das Hinzufügen aller `T`Einschränkungen `S`, außer den besonderen Einschränkungen (`Class`, `Structure``New`), aufgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-526">A type parameter constraint `Of S As T` is resolved by transitively adding all of `T`'s constraints onto `S`, other than the special constraints (`Class`, `Structure`, `New`).</span></span> <span data-ttu-id="0155f-527">Es ist ein Fehler, zirkuläre Einschränkungen (z. b. `Of S As T, T As S`) zu haben.</span><span class="sxs-lookup"><span data-stu-id="0155f-527">It is an error to have circular constraints (e.g. `Of S As T, T As S`).</span></span> <span data-ttu-id="0155f-528">Es ist ein Fehler, eine Typparameter Einschränkung zu haben, die selbst über die `Structure`-Einschränkung verfügt.</span><span class="sxs-lookup"><span data-stu-id="0155f-528">It is an error to have a type parameter constraint which itself has the `Structure` constraint.</span></span> <span data-ttu-id="0155f-529">Nach dem Hinzufügen von Einschränkungen ist es möglich, dass eine Reihe von besonderen Situationen eintreten kann:</span><span class="sxs-lookup"><span data-stu-id="0155f-529">After adding constraints, it is possible that a number of special situations may occur:</span></span>

* <span data-ttu-id="0155f-530">Wenn mehrere Klassen Einschränkungen vorhanden sind, gilt die am meisten abgeleitete Klasse als Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="0155f-530">If multiple class constraints exist, the most derived class is considered to be the constraint.</span></span> <span data-ttu-id="0155f-531">Wenn eine oder mehrere Klassen Einschränkungen nicht über eine Vererbungs Beziehung verfügen, kann die Einschränkung nicht wieder hergestellt werden, und es handelt sich um einen Fehler.</span><span class="sxs-lookup"><span data-stu-id="0155f-531">If one or more class constraints have no inheritance relationship, the constraint is unsatisfiable and it is an error.</span></span>

 * <span data-ttu-id="0155f-532">Wenn ein Typparameter eine `Structure` besondere Einschränkung mit einer benannten Klassen Einschränkung oder der `Class` speziellen Einschränkung kombiniert, ist dies ein Fehler.</span><span class="sxs-lookup"><span data-stu-id="0155f-532">If a type parameter combines a `Structure` special constraint with a named class constraint or the `Class` special constraint, it is an error.</span></span> <span data-ttu-id="0155f-533">Eine Klassen Einschränkung kann `NotInheritable`werden. in diesem Fall werden keine abgeleiteten Typen dieser Einschränkung akzeptiert, und es handelt sich um einen Fehler.</span><span class="sxs-lookup"><span data-stu-id="0155f-533">A class constraint may be `NotInheritable`, in which case no derived types of that constraint are accepted and it is an error.</span></span>

<span data-ttu-id="0155f-534">Der Typ kann ein oder ein Typ sein, der von geerbt wurde, die folgenden speziellen Typen: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`oder `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="0155f-534">The type may be one of, or a type inherited from, the following special types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span> <span data-ttu-id="0155f-535">In diesem Fall wird nur der Typ oder ein von ihm geerbte Typ akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="0155f-535">In that case, only the type, or a type inherited from it, is accepted.</span></span> <span data-ttu-id="0155f-536">Ein Typparameter, der auf einen dieser Typen beschränkt ist, kann nur die Konvertierungen verwenden, die vom `DirectCast` Operator zugelassen werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-536">A type parameter constrained to one of these types can only use the conversions allowed by the `DirectCast` operator.</span></span> <span data-ttu-id="0155f-537">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-537">For example:</span></span>

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

<span data-ttu-id="0155f-538">Außerdem kann ein Typparameter, der aufgrund einer der oben genannten Lockerungen auf einen Werttyp beschränkt ist, keine Methoden aufrufen, die für diesen Werttyp definiert sind.</span><span class="sxs-lookup"><span data-stu-id="0155f-538">Additionally, a type parameter constrained to a value type due to one of the above relaxations cannot call any methods defined on that value type.</span></span> <span data-ttu-id="0155f-539">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-539">For example:</span></span>

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

<span data-ttu-id="0155f-540">Wenn die Einschränkung nach der Ersetzung als Arraytyp endet, ist auch ein beliebiger kovariant-Arraytyp zulässig.</span><span class="sxs-lookup"><span data-stu-id="0155f-540">If the constraint, after substitution, ends up as an array type, any covariant array type is allowed as well.</span></span> <span data-ttu-id="0155f-541">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-541">For example:</span></span>

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

<span data-ttu-id="0155f-542">Ein Typparameter mit einer Klassen-oder Schnittstellen Einschränkung gilt als die gleichen Member wie diese Klassen-oder Schnittstellen Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="0155f-542">A type parameter with a class or interface constraint is considered to have the same members as that class or interface constraint.</span></span> <span data-ttu-id="0155f-543">Wenn ein Typparameter mehrere Einschränkungen aufweist, wird der Typparameter als die Vereinigung aller Member der Einschränkungen betrachtet.</span><span class="sxs-lookup"><span data-stu-id="0155f-543">If a type parameter has multiple constraints, then the type parameter is considered to have the union of all the members of the constraints.</span></span> <span data-ttu-id="0155f-544">Wenn in mehr als einer Einschränkung Member mit demselben Namen vorhanden sind, werden Member in der folgenden Reihenfolge ausgeblendet: die Klassen Einschränkung blendet Member in Schnittstellen Einschränkungen aus, die Member in `System.ValueType` ausblenden (wenn `Structure` Einschränkung angegeben ist), wodurch Elemente in `Object`ausgeblendet werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-544">If there are members with the same name in more than one constraint, then members are hidden in the following order: the class constraint hides members in interface constraints, which hide members in `System.ValueType` (if `Structure` constraint is specified), which hides members in `Object`.</span></span> <span data-ttu-id="0155f-545">Wenn ein Member mit demselben Namen in mehr als einer Schnittstellen Einschränkung angezeigt wird, ist der Member nicht verfügbar (wie bei der mehrfach Schnittstellen Vererbung), und der Typparameter muss in die gewünschte Schnittstelle umgewandelt werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-545">If a member with the same name appears in more than one interface constraint the member is unavailable (as in multiple interface inheritance) and the type parameter must be cast to the desired interface.</span></span> <span data-ttu-id="0155f-546">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-546">For example:</span></span>

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

<span data-ttu-id="0155f-547">Beim Bereitstellen von Typparametern als Typargumente müssen die Typparameter die Einschränkungen der übereinstimmenden Typparameter erfüllen.</span><span class="sxs-lookup"><span data-stu-id="0155f-547">When supplying type parameters as type arguments, the type parameters must satisfy the constraints of the matching type parameters.</span></span>

```vb
Class Base(Of T As Class)
End Class

Class Derived(Of V)
    ' Error: V does not satisfy the constraints of T
    Inherits Base(Of V)
End Class
```

<span data-ttu-id="0155f-548">Werte eines eingeschränkten Typparameters können für den Zugriff auf die Instanzmember, einschließlich der in der Einschränkung angegebenen Instanzmethoden, verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-548">Values of a constrained type parameter can be used to access the instance members, including instance methods, specified in the constraint.</span></span>

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

### <a name="type-parameter-variance"></a><span data-ttu-id="0155f-549">Typparameter Varianz</span><span class="sxs-lookup"><span data-stu-id="0155f-549">Type Parameter Variance</span></span>

<span data-ttu-id="0155f-550">Ein Typparameter in einer Schnittstelle oder eine delegattypdeklaration kann optional einen *Varianz-Modifizierer*angeben.</span><span class="sxs-lookup"><span data-stu-id="0155f-550">A type parameter in an interface or a delegate type declaration can optionally specify a *variance modifier*.</span></span> <span data-ttu-id="0155f-551">Typparameter mit Varianz modifiziermetern beschränken, wie der Typparameter in der Schnittstelle oder im Delegattyp verwendet werden kann, aber die Konvertierung einer generischen Schnittstelle oder eines Delegattyps in einen anderen generischen Typ mit Varianten kompatiblen Typargumenten ermöglicht</span><span class="sxs-lookup"><span data-stu-id="0155f-551">Type parameters with variance modifiers restrict how the type parameter can be used in the interface or delegate type but allow a generic interface or delegate type to be converted to another generic type with variant compatible type arguments.</span></span> <span data-ttu-id="0155f-552">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-552">For example:</span></span>

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

<span data-ttu-id="0155f-553">Generische Schnittstellen, die über Typparameter mit Varianz modifiziermetern verfügen, haben mehrere Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="0155f-553">Generic interfaces that have type parameters with variance modifiers have several restrictions:</span></span>

* <span data-ttu-id="0155f-554">Sie können keine Ereignis Deklaration enthalten, die eine Parameterliste angibt (aber eine benutzerdefinierte Ereignis Deklaration oder eine Ereignis Deklaration mit einem Delegattyp ist zulässig).</span><span class="sxs-lookup"><span data-stu-id="0155f-554">They cannot contain an event declaration that specifies a parameter list (but a custom event declaration or an event declaration with a delegate type is allowed).</span></span>

* <span data-ttu-id="0155f-555">Sie dürfen keine Klassen, Strukturen oder Enumerationstypen enthalten.</span><span class="sxs-lookup"><span data-stu-id="0155f-555">They cannot contain a nested class, structure, or enumerated type.</span></span>

<span data-ttu-id="0155f-556">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="0155f-556">__Note.__</span></span> <span data-ttu-id="0155f-557">Diese Einschränkungen sind darauf zurückzuführen, dass Typen, die in generischen Typen enthalten sind, implizit die generischen Parameter ihrer übergeordneten Elemente kopieren.</span><span class="sxs-lookup"><span data-stu-id="0155f-557">These restrictions are due to the fact that types nested in generic types implicitly copy the generic parameters of their parent.</span></span> <span data-ttu-id="0155f-558">Im Fall von Klassen-, Struktur-oder Enumerationstypen können diese Arten von Typen keine Varianz Modifizierer für ihre Typparameter aufweisen.</span><span class="sxs-lookup"><span data-stu-id="0155f-558">In the case of nested classes, structures, or enumerated types, those kinds of types cannot have variance modifiers on their type parameters.</span></span> <span data-ttu-id="0155f-559">Im Fall einer Ereignis Deklaration mit einer Parameterliste kann die generierte geschaltete Delegatklasse verwirrende Fehler aufweisen, wenn ein Typ, der in einer `In` Position (d. h. in einem Parametertyp) angezeigt wird, tatsächlich an einer `Out` Position (d. h. dem Ereignistyp) verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="0155f-559">In the case of an event declaration with a parameter list, the generated nested delegate class could have confusing errors when a type that appears to be used in an `In` position (i.e. a parameter type) is actually used in an `Out` position (i.e. the type of the event).</span></span>

<span data-ttu-id="0155f-560">Ein Typparameter, der mit dem out-Modifizierer deklariert wird, ist *kovariant*.</span><span class="sxs-lookup"><span data-stu-id="0155f-560">A type parameter that is declared with the Out modifier is *covariant*.</span></span> <span data-ttu-id="0155f-561">Informell kann ein kovarianter Typparameter nur in einer Ausgabe Position verwendet werden, d. h. ein Wert, der von der Schnittstelle oder vom Delegattyp zurückgegeben wird, und kann nicht an einer Eingabe Position verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-561">Informally, a covariant type parameter can only be used in an output position -- i.e. a value that is being returned from the interface or delegate type -- and cannot be used in an input position.</span></span> <span data-ttu-id="0155f-562">Eine Typ`T` wird als *gültig* betrachtet, wenn Folgendes gilt:</span><span class="sxs-lookup"><span data-stu-id="0155f-562">A type `T` is considered to be *valid covariantly* if:</span></span>

* <span data-ttu-id="0155f-563">`T` ist eine Klasse, eine Struktur oder ein enumerierter Typ.</span><span class="sxs-lookup"><span data-stu-id="0155f-563">`T` is a class, structure, or enumerated type.</span></span>

* <span data-ttu-id="0155f-564">`T` ist ein nicht generischer Delegat-oder Schnittstellentyp.</span><span class="sxs-lookup"><span data-stu-id="0155f-564">`T` is non-generic delegate or interface type.</span></span>

* <span data-ttu-id="0155f-565">`T` ist ein Arraytyp, dessen Elementtyp kovarianter ist.</span><span class="sxs-lookup"><span data-stu-id="0155f-565">`T` is an array type whose element type is valid covariantly.</span></span>

* <span data-ttu-id="0155f-566">`T` ist ein Typparameter, der nicht als `Out` Typparameter deklariert wurde.</span><span class="sxs-lookup"><span data-stu-id="0155f-566">`T` is a type parameter which was not declared as an `Out` type parameter.</span></span>

* <span data-ttu-id="0155f-567">`T` ist eine erstellte Schnittstelle oder ein Delegattyp `X(Of P1,...,Pn)` mit Typargumenten `A1,...,An` Folgendes:</span><span class="sxs-lookup"><span data-stu-id="0155f-567">`T` is a constructed interface or delegate type `X(Of P1,...,Pn)` with type arguments `A1,...,An` such that:</span></span>

  * <span data-ttu-id="0155f-568">Wenn `Pi` als out-Typparameter deklariert wurde, ist `Ai` kovariant gültig.</span><span class="sxs-lookup"><span data-stu-id="0155f-568">If `Pi` was declared as an Out type parameter then `Ai` is valid covariantly.</span></span>

  * <span data-ttu-id="0155f-569">Wenn `Pi` als im Typparameter deklariert wurde, ist `Ai` kontra varianter.</span><span class="sxs-lookup"><span data-stu-id="0155f-569">If `Pi` was declared as an In type parameter then `Ai` is valid contravariantly.</span></span>

<span data-ttu-id="0155f-570">Folgendes muss in einer Schnittstelle oder einem Delegattyp gültig sein:</span><span class="sxs-lookup"><span data-stu-id="0155f-570">The following must be valid covariantly in an interface or delegate type:</span></span>

* <span data-ttu-id="0155f-571">Die Basisschnittstelle einer Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="0155f-571">The base interface of an interface.</span></span>

* <span data-ttu-id="0155f-572">Der Rückgabetyp einer Funktion oder des Delegattyps.</span><span class="sxs-lookup"><span data-stu-id="0155f-572">The return type of a function or the delegate type.</span></span>

* <span data-ttu-id="0155f-573">Der Typ einer Eigenschaft, wenn ein `Get` Accessor vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="0155f-573">The type of a property if there is a `Get` accessor.</span></span>

* <span data-ttu-id="0155f-574">Der Typ eines beliebigen `ByRef` Parameters.</span><span class="sxs-lookup"><span data-stu-id="0155f-574">The type of any `ByRef` parameter.</span></span>

<span data-ttu-id="0155f-575">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-575">For example:</span></span>

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

<span data-ttu-id="0155f-576">__Nebenbei.__</span><span class="sxs-lookup"><span data-stu-id="0155f-576">__Note.__</span></span> <span data-ttu-id="0155f-577">`Out` ist kein reserviertes Wort.</span><span class="sxs-lookup"><span data-stu-id="0155f-577">`Out` is not a reserved word.</span></span>

<span data-ttu-id="0155f-578">Ein Typparameter, der mit dem in-Modifizierer deklariert wird, ist *kontra Variant*.</span><span class="sxs-lookup"><span data-stu-id="0155f-578">A type parameter that is declared with the In modifier is *contravariant*.</span></span> <span data-ttu-id="0155f-579">Ein kontra varianter Typparameter kann nur in einer Eingabe Position verwendet werden, d. h. ein Wert, der an die Schnittstelle oder den Delegattyp übergeben wird, und kann nicht in einer Ausgabe Position verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-579">Informally, a contravariant type parameter can only be used in an input position -- i.e. a value that is being passed in to the interface or delegate type -- and cannot be used in an output position.</span></span> <span data-ttu-id="0155f-580">Eine `T` vom Typ wird als *gültig* betrachtet, wenn Folgendes gilt:</span><span class="sxs-lookup"><span data-stu-id="0155f-580">A type `T` is considered to be *valid contravariantly* if:</span></span>

* <span data-ttu-id="0155f-581">`T` ist eine Klasse, eine Struktur oder ein enumerierter Typ.</span><span class="sxs-lookup"><span data-stu-id="0155f-581">`T` is a class, structure, or enumerated type.</span></span>

* <span data-ttu-id="0155f-582">`T` ist ein nicht generischer Delegat-oder Schnittstellentyp.</span><span class="sxs-lookup"><span data-stu-id="0155f-582">`T` is a non-generic delegate or interface type.</span></span>

* <span data-ttu-id="0155f-583">`T` ist ein Arraytyp, dessen Elementtyp kontra varianter ist.</span><span class="sxs-lookup"><span data-stu-id="0155f-583">`T` is an array type whose element type is valid contravariantly.</span></span>

* <span data-ttu-id="0155f-584">`T` ist ein Typparameter, der nicht als im Typparameter deklariert wurde.</span><span class="sxs-lookup"><span data-stu-id="0155f-584">`T` is a type parameter which was not declared as an In type parameter.</span></span>

* <span data-ttu-id="0155f-585">`T` ist eine erstellte Schnittstelle oder ein Delegattyp `X(Of P1,...,Pn)` mit Typargumenten `A1,...,An` Folgendes:</span><span class="sxs-lookup"><span data-stu-id="0155f-585">`T` is a constructed interface or delegate type `X(Of P1,...,Pn)` with type arguments `A1,...,An` such that:</span></span>

  * <span data-ttu-id="0155f-586">Wenn `Pi` als `Out` Typparameter deklariert wurde, ist `Ai` kontra varianter.</span><span class="sxs-lookup"><span data-stu-id="0155f-586">If `Pi` was declared as an `Out` type parameter then `Ai` is valid contravariantly.</span></span>

  * <span data-ttu-id="0155f-587">Wenn `Pi` als `In` Typparameter deklariert wurde, ist `Ai` kovariant gültig.</span><span class="sxs-lookup"><span data-stu-id="0155f-587">If `Pi` was declared as an `In` type parameter then `Ai` is valid covariantly.</span></span>

<span data-ttu-id="0155f-588">Folgendes muss bei einer Schnittstelle oder einem Delegattyp überwiegend gültig sein:</span><span class="sxs-lookup"><span data-stu-id="0155f-588">The following must be valid contravariantly in an interface or delegate type:</span></span>

* <span data-ttu-id="0155f-589">Der Typ eines Parameters.</span><span class="sxs-lookup"><span data-stu-id="0155f-589">The type of a parameter.</span></span>

* <span data-ttu-id="0155f-590">Eine Typeinschränkung für einen Methodentypparameter.</span><span class="sxs-lookup"><span data-stu-id="0155f-590">A type constraint on a method type parameter.</span></span>

* <span data-ttu-id="0155f-591">Der Typ einer Eigenschaft, wenn Sie über einen `Set` Accessor verfügt.</span><span class="sxs-lookup"><span data-stu-id="0155f-591">The type of a property if it has a `Set` accessor.</span></span>

* <span data-ttu-id="0155f-592">Der Typ eines Ereignisses.</span><span class="sxs-lookup"><span data-stu-id="0155f-592">The type of an event.</span></span>

<span data-ttu-id="0155f-593">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0155f-593">For example:</span></span>

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

<span data-ttu-id="0155f-594">In dem Fall, in dem ein Typ gültig sein muss, muss er gegen überdies und kovarianter sein (z. b. eine Eigenschaft mit einem `Get`-und `Set`-Accessor oder einem `ByRef`-Parameter), kann kein Variant-Typparameter verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0155f-594">In the case where a type must be valid be contravariantly and covariantly (such as a property with both a `Get` and `Set` accessor or a `ByRef` parameter), a variant type parameter cannot be used.</span></span>


<span data-ttu-id="0155f-595">Ko-und Kontra Varianz können zu einem "Diamond mehrdeutigkeitsproblem" führen.</span><span class="sxs-lookup"><span data-stu-id="0155f-595">Co- and contra-variance give rise to a "diamond ambiguity problem".</span></span> <span data-ttu-id="0155f-596">Betrachten Sie folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="0155f-596">Consider the following code:</span></span>

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

<span data-ttu-id="0155f-597">Die-Klasse `C` kann auf zwei Arten in `IEnumerable(Of Object)` konvertiert werden, beide durch kovariante Konvertierung von `IEnumerable(Of String)` und durch kovariante Konvertierung aus `IEnumerable(Of Exception)`.</span><span class="sxs-lookup"><span data-stu-id="0155f-597">The class `C` can be converted to `IEnumerable(Of Object)` in two ways, both through covariant conversion from `IEnumerable(Of String)` and through covariant conversion from `IEnumerable(Of Exception)`.</span></span> <span data-ttu-id="0155f-598">Die CLR gibt nicht an, welche der beiden Methoden von `c.GetEnumerator()`aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="0155f-598">The CLR does not specify which of the two methods will be called by `c.GetEnumerator()`.</span></span> <span data-ttu-id="0155f-599">Im Allgemeinen gilt: Wenn eine Klasse deklariert wird, um eine kovariante Schnittstelle mit zwei unterschiedlichen generischen Argumenten zu implementieren, die einen gemeinsamen Obertyp aufweisen (z. b. in diesem Fall `String` und `Exception` über die gemeinsame Obertyp-`Object`verfügen), oder wenn eine Klasse deklariert wird, um eine kontra Variante Schnittstelle mit zwei unterschiedlichen generischen Argumenten zu implementieren, die einen</span><span class="sxs-lookup"><span data-stu-id="0155f-599">In general, whenever a class is declared to implement a covariant interface with two different generic arguments that have a common supertype (e.g. in this case `String` and `Exception` have the common supertype `Object`), or a class is declared to implement a contravariant interface with two different generic arguments that have a common subtype, then ambiguity is likely to arise.</span></span> <span data-ttu-id="0155f-600">Der Compiler gibt eine Warnung für solche Deklarationen aus.</span><span class="sxs-lookup"><span data-stu-id="0155f-600">The compiler gives a warning on such declarations.</span></span>
