---
title: Verknüpfen von Xamarin.iOS-Apps
description: In diesem Dokument wird der Xamarin.iOS-Linker beschrieben, der zum Löschen von nicht verwendetem Code aus einer Xamarin.iOS-App verwendet wird, um deren Größe zu verringern.
ms.prod: xamarin
ms.assetid: 3A4B2178-F264-0E93-16D1-8C63C940B2F9
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/24/2017
ms.openlocfilehash: 7769e3d02acc9f1522c6028f88f37c1f522866af
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936759"
---
# <a name="linking-xamarinios-apps"></a>Verknüpfen von Xamarin.iOS-Apps

Beim Erstellen Ihrer Anwendung ruft Visual Studio für Mac oder Visual Studio ein Tool namens **mtouch** auf, das einen Linker für verwalteten Code enthält. Es wird verwendet, um die Funktionen, die die Anwendung nicht verwendet, aus den Klassenbibliotheken zu entfernen. Ziel ist es, die Größe der Anwendung so zu reduzieren, dass sie nur mit den notwendigen Bits ausgeliefert wird.

Der Linker verwendet eine statische Analyse, um die verschiedenen Codepfade zu bestimmen, denen Ihre Anwendung folgen kann. Es ist ein bisschen aufwändig, da der Linker jedes Detail jeder Assembly durchgehen muss, um sicherzustellen, dass nichts Auffindbares entfernt wird. Der Linker ist nicht für die Simulatorbuilds standardmäßig aktiviert, um die Buildzeit beim Debuggen zu verkürzen. Da er jedoch kleinere Anwendungen generiert, kann er die AOT-Kompilierung und das Hochladen auf das Gerät beschleunigen. Alle *Geräte- (Release-)builds* verwenden den Linker standardmäßig.

Da der Linker ein statisches Tool ist, kann er nicht für Inklusionstypen und -methoden markiert werden, die durch Reflektion aufgerufen oder dynamisch instanziiert werden. Es gibt zur Umgehung der Einschränkung mehrere Optionen.

<a name="Linker_Behavior"></a>

## <a name="linker-behavior"></a>Linkerverhalten

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Der Verknüpfungsprozess kann in **Projektoptionen** über das Dropdownmenü „Linkerverhalten“ angepasst werden. Um auf dieses iOS-Projekt zuzugreifen, doppelklicken Sie darauf, und navigieren Sie zu **iOS-Build > Linkeroptionen**, wie unten dargestellt:

[![Linkeroptionen](linker-images/image1.png)](linker-images/image1.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Der Verknüpfungsprozess kann in Visual Studio in **Projektoptionen** über das Dropdownmenü „Linkerverhalten“ angepasst werden.

Führen Sie folgende Schritte aus:

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den **Projektnamen**, und wählen Sie **Eigenschaften** aus:

    ![Im Projektmappen-Explorer mit der rechten Maustaste auf Projektnamen klicken und „Eigenschaften“ auswählen](linker-images/linking01w.png)
2. Wählen Sie in den **Projekteigenschaften** die Option **iOS-Build** aus:

    ![„iOS-Build“ auswählen](linker-images/linking02w.png)
3. Befolgen Sie die nachstehenden Anweisungen, um die Verknüpfungsoptionen zu ändern.

-----

Die drei angebotenen Hauptoptionen werden im Folgenden beschrieben:

### <a name="dont-link"></a>Nicht verknüpfen

Das Deaktivieren von Verknüpfungen stellt sicher, dass keine Assemblys verändert werden. Aus Leistungsgründen ist dies die Standardeinstellung, wenn Ihre IDE auf den iOS-Simulator abzielt. Für Gerätebuilds sollte dies nur zur Problemumgehung verwendet werden, wenn der Linker einen Fehler enthält, der die Ausführung Ihrer Anwendung verhindert. Wenn Ihre Anwendung nur mit *-nolink* funktioniert, reichen Sie einen [Fehlerbericht](https://github.com/xamarin/xamarin-macios/issues/new) ein.

Dies entspricht der Option *-nolink* bei Verwendung des Befehlszeilentools mtouch.

<a name="Link_SDK_assemblies_only"></a>

### <a name="link-sdk-assemblies-only"></a>Nur SDK-Assemblys verknüpfen

In diesem Modus lässt der Linker Ihre Assemblys unangetastet und reduziert die Größe der SDK-Assemblys (d. h. was mit Xamarin.iOS ausgeliefert wird), indem er alles entfernt, was Ihre Anwendung nicht verwendet. Dies ist die Standardeinstellung, wenn Ihre IDE auf iOS-Geräte abzielt.

Dies ist die einfachste Option, da keine Änderungen an Ihrem Code erforderlich sind. Der Unterschied bei der Verknüpfung von allem ist, dass der Linker in diesem Modus einige Optimierungen nicht durchführen kann. Es handelt sich also um einen Kompromiss zwischen dem Aufwand für die Verknüpfung von allem und der endgültigen Anwendungsgröße.

Dies entspricht der Option *-linksdk* bei Verwendung des Befehlszeilentools mtouch.

<a name="Link_all_assemblies"></a>

### <a name="link-all-assemblies"></a>Alle Assemblys verknüpfen

Beim Verknüpfen von allem kann der Linker alle seine Optimierungen nutzen, um die Anwendung so klein wie möglich zu gestalten. Er ändert den Benutzercode, der bei jeder Verwendung von Funktionen, die die statische Analyse des Linkers nicht erkennen kann, gestört werden kann. In solchen Fällen, wie z.B. bei Webdiensten, Reflektion oder Serialisierung, kann es sein, dass einige Anpassungen in Ihrer Anwendung erforderlich sind, um alles zu verknüpfen.

Dies entspricht der Option *-linkall* bei Verwendung des Befehlszeilentools **mtouch**.

<a name="Controlling_the_Linker"></a>

## <a name="controlling-the-linker"></a>Steuern des Linkers

Bei Verwendung des Linkers wird mitunter Code entfernt, den Sie möglicherweise dynamisch, auch indirekt, aufgerufen haben. Um diese Fälle abzudecken, bietet der Linker einige Funktionen und Optionen, die Ihnen eine bessere Steuerung seiner Aktionen ermöglichen.

<a name="Preserving_Code"></a>

### <a name="preserving-code"></a>Beibehalten von Code

Beim Einsatz des Linkers kann gelegentlich Code entfernt werden, den Sie möglicherweise dynamisch aufgerufen haben, entweder mit „System.Reflection.MemberInfo.Invoke“ oder indem Sie Ihre Methoden mit dem `[Export]`-Attribut nach Objective-C exportieren und den Selektor dann manuell aufrufen.

In diesen Fällen können Sie den Linker anweisen, entweder ganze Klassen zu verwenden oder einzelne Mitglieder beizubehalten, indem Sie das `[Xamarin.iOS.Foundation.Preserve]`-Attribut entweder auf Klassen- oder Memberebene anwenden. Jeder Member, der nicht statisch durch die Anwendung verknüpft ist, muss entfernt werden. Dieses Attribut wird daher verwendet, um Member zu kennzeichnen, auf die nicht statisch verwiesen wird, die aber dennoch von Ihrer Anwendung benötigt werden.

Wenn Sie beispielsweise Typen dynamisch instanziieren, sollten Sie den Standardkonstruktor Ihrer Typen beibehalten. Wenn Sie die XML-Serialisierung verwenden, sollten Sie die Eigenschaften Ihrer Typen beibehalten.

Sie können dieses Attribut auf alle Member eines Typs oder auf den Typ selbst anwenden. Wenn Sie den gesamten Typ beibehalten möchten, können Sie die Syntax `[Preserve
(AllMembers = true)]` für den Typ verwenden.

In einigen Fällen sollten Sie bestimmte Member beibehalten, jedoch nur, wenn der Containertyp beibehalten wurde. Verwenden Sie in diesen Fällen `[Preserve (Conditional=true)]`.

Wenn Sie keine Abhängigkeit von den Xamarin-Bibliotheken wünschen, z.B. wenn Sie eine plattformübergreifende Klassenbibliothek (PCL) erstellen, können Sie dieses Attribut weiter verwenden.

Zu diesem Zweck müssen Sie lediglich eine „PreserveAttribute“-Klasse deklarieren und wie folgt in Ihrem Code verwenden:

```csharp
public sealed class PreserveAttribute : System.Attribute {
    public bool AllMembers;
    public bool Conditional;
}
```

Es spielt keine Rolle, in welchem Namespace dies definiert ist, denn der Linker sucht dieses Attribut nach Typnamen.

 <a name="Skipping_Assemblies"></a>

### <a name="skipping-assemblies"></a>Überspringen von Assemblys

Es ist möglich, Assemblys anzugeben, die vom Linkerprozess ausgeschlossen werden sollen, während andere Assemblys normal verknüpft werden können. Dies ist hilfreich, wenn das Anwenden von `[Preserve]` auf einige Assemblys unmöglich ist (z.B. Code von Drittanbietern) oder als vorübergehende Behelfslösung für einen Fehler.

Dies entspricht der Option `--linkskip` bei Verwendung des Befehlszeilentools mtouch.

Wenn Sie bei Verwenden der Option **Alle Assemblys verknüpfen** dem Linker angeben möchten, dass er ganze Assemblys überspringen soll, fügen Sie in die Optionen unter **Weitere mtouch-Argumente** für Ihre oberste Assembly Folgendes ein:

```csharp
--linkskip=NameOfAssemblyToSkipWithoutFileExtension
```

Wenn Sie möchten, dass der Linker mehrere Assemblys überspringt, fügen Sie mehrere `linkskip`-Argumente hinzu:

```csharp
--linkskip=NameOfFirstAssembly --linkskip=NameOfSecondAssembly
```

Es gibt keine Benutzeroberfläche für diese Option, aber sie kann im Visual Studio für Mac-Dialogfeld „Projektoptionen“ oder im Visual Studio-Bereich „Projekteigenschaften“ im Textfeld **Weitere mtouch-Argumente** angegeben werden. (z. B. verknüpft *--linkskip=mscorlib* „mscorlib.dll“ nicht, andere Assemblys in der Projektmappe dagegen schon).

<a name="Disabling_Link_Away"></a>

### <a name="disabling-link-away"></a>Deaktivieren von „LinkAway“

Der Linker entfernt Code, der sehr wahrscheinlich nicht auf Geräten verwendet wird, weil er z.B. nicht unterstützt oder erlaubt ist. In seltenen Fällen ist es möglich, dass eine Anwendung oder Bibliothek von diesem (funktionierenden oder nicht funktionierenden) Code abhängig ist. Ab Xamarin.iOS 5.0.1 kann der Linker angewiesen werden, diese Optimierung zu überspringen.

Dies entspricht der Option *-nolinkaway* bei Verwendung des Befehlszeilentools mtouch.

Es gibt keine Benutzeroberfläche für diese Option, aber sie kann im Visual Studio für Mac-Dialogfeld „Projektoptionen“ oder im Visual Studio-Bereich „Projekteigenschaften“ im Textfeld **Weitere mtouch-Argumente** angegeben werden. (z. B. entfernt *--nolinkaway* nicht den zusätzlichen Code [ca. 100 KB]).

### <a name="marking-your-assembly-as-linker-ready"></a>Markieren der Assembly als linkerfähig

Benutzer können wählen, ob sie nur die SDK-Assemblys verknüpfen möchten und keine Verknüpfung mit ihrem Code vornehmen möchten.  Dies bedeutet auch, dass Bibliotheken von Drittanbietern, die nicht Teil des Haupt-SDK von Xamarin sind, nicht verknüpft werden.

Dies geschieht üblicherweise, weil sie `[Preserve]`-Attribute ihrem Code nicht manuell hinzufügen möchten.  Der Nebeneffekt ist, dass Bibliotheken von Drittanbietern nicht verknüpft werden. Das ist im Allgemeinen eine sinnvolle Standardeinstellung, da es nicht möglich ist zu wissen, ob eine Bibliothek von Drittanbietern linkerfähig ist oder nicht.

Wenn Sie eine Bibliothek in Ihrem Projekt haben oder Entwickler von wiederverwendbaren Bibliotheken sind und möchten, dass der Linker Ihre Assembly als verknüpfbar behandelt, müssen Sie nur das Attribut für die Assembly-Ebene [`LinkerSafe`](xref:Foundation.LinkerSafeAttribute) wie folgt hinzufügen:

```csharp
[assembly:LinkerSafe]
```

Ihre Bibliothek muss dafür nicht unbedingt auf die Xamarin-Bibliotheken verweisen.  Wenn Sie beispielsweise eine portable Klassenbibliothek erstellen, die auf anderen Plattformen ausgeführt wird, können Sie trotzdem ein `LinkerSafe`-Attribut verwenden.
Der Xamarin-Linker schlägt das `LinkerSafe`-Attribut nach Name und nicht nach seinem tatsächlichen Typ nach.  Das bedeutet, dass Sie diesen Code schreiben können, der auch funktioniert:

```csharp
[assembly:LinkerSafe]
// ... assembly attribute should be at top, before source
class LinkerSafeAttribute : System.Attribute {}
```

## <a name="custom-linker-configuration"></a>Benutzerdefinierte Linkerkonfiguration

Befolgen Sie die [Anweisungen zum Erstellen einer Linkerkonfigurationsdatei](~/cross-platform/deploy-test/linker.md).

## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Linkerkonfiguration](~/cross-platform/deploy-test/linker.md)
- [Verknüpfung auf dem Mac](~/mac/deploy-test/linker.md)
- [Verknüpfung unter Android](~/android/deploy-test/linker.md)
