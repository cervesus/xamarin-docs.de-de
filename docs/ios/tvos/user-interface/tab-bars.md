---
title: Arbeiten mit tvos-Registerkarten leisten-Controllern in xamarin
description: In diesem Dokument wird beschrieben, wie Sie mit Tabstopp leisten Controllern in einer mit xamarin erstellten tvos-App arbeiten. Er bietet eine allgemeine Übersicht über die Registerkarten leisten und erläutert Registerkarten, die storyboardintegration und Registerkarten Elemente.
ms.prod: xamarin
ms.assetid: 99A2D7C6-0324-4DE5-B6E9-D39D0BAD8370
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: df19dcf542bd3a62a696c0d7d533b4e14390336e
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768999"
---
# <a name="working-with-tvos-tab-bar-controllers-in-xamarin"></a>Arbeiten mit tvos-Registerkarten leisten-Controllern in xamarin

Für viele Arten von tvos-apps wird die primäre Navigation als Registerkarte angezeigt, die im oberen Bildschirmbereich ausgeführt wird. Der Benutzer wird nach links und rechts in die Liste der möglichen Kategorien und der Inhalts Bereich unterhalb der Änderungen geändert, um die Auswahl des Benutzers widerzuspiegeln.

[![](tab-bars-images/tab01.png "Beispiel Registerkarten Leiste")](tab-bars-images/tab01.png#lightbox)

Die Registerkarten Leiste ist standardmäßig übersichtlich und wird immer am oberen Rand des Bildschirms angezeigt. Im Fokusbereich deckt eine Registerkarten Leiste die oberen 140 Pixel des Bildschirms ab, wird jedoch schnell verschoben, wenn sich der Fokus auf den folgenden Inhalts Bereich verschiebt.

<a name="Tab-Bars-in-tvOS" />

## <a name="tab-bars-in-tvos"></a>Registerkarten leisten in tvos

Die `UITabViewController` funktioniert auf ähnliche Weise und hat bei tvos einen ähnlichen Zweck wie in IOS und bietet die folgenden wichtigen Unterschiede:

- Anders als bei der Registerkarten Leiste auf Ios, die unten auf dem Bildschirm angezeigt wird, belegen Registerkarten in tvos die oberen 140 Pixel des Bildschirms und sind standardmäßig übersichtlich.
- Wenn der Fokus die Registerkarten Leiste für den folgenden Inhalts Bereich verlässt, wird die Registerkarten Leiste schnell vom oberen Bildschirmrand bewegt und ausgeblendet. Der Benutzer kann entweder einmal auf die Menü Schaltfläche tippen oder auf der [Siri-Remote](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote) Taste nach oben klicken, um die Registerkarten Leiste erneut anzuzeigen.
- Wenn Sie auf der Siri-Remote-Taste nach unten ziehen, wird der Fokus auf den Inhalts Bereich unterhalb der Registerkarten Leiste zum ersten Fokus [baren Element](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) in dem angezeigten Inhalt verschoben. Dadurch wird die Registerkarten Leiste ausgeblendet, sobald sich der Fokus verschiebt.
- Durch Klicken auf eine in der Registerkarten Leiste angezeigte Kategorie wird der Inhalt der Kategorie gewechselt, und der Fokus wird auf das erste Fokussier Bare Element in dieser Ansicht umgestellt.
- Die Anzahl der Kategorien, die auf der Registerkarten Leiste angezeigt werden, sollte korrigiert sein, und alle Kategorien sollten jederzeit verfügbar sein. eine bestimmte Kategorie sollte nie deaktiviert werden.
- Die Anpassung an tvos wird von Registerkarten leisten nicht unterstützt. Darüber hinaus wird nicht die **Weitere** Kategorie angezeigt (z. b. IOS), wenn mehr Kategorien vorhanden sind, als in die Registerkarten Leiste passen.

Apple hat die folgenden Vorschläge zum Arbeiten mit Registerkarten:

- **Verwenden von Registerkarten zum logischen organisieren von Inhalten** : Verwenden Sie die Registerkarten Leiste, um den Inhalt logisch zu organisieren, mit dem Ihre tvos-APP arbeitet. Beispiel: "vorgestellte, erste Diagramme", "gekauft" und "suchen".
- **Hinzufügen von Badge zum Benachrichtigen von Benutzern über neue Inhalte** : Optional können Sie einen Badge (ein rotes Oval mit einer weißen Zahl oder einen Ausrufezeichen) anzeigen, um den Benutzer über neue Inhalte in einer Kategorie zu informieren.
- **Verwenden** Sie die Kennzeichen sparsam. die Registerkarten Leiste wird nicht mit den Abzeichen versehen, und Sie werden nur angezeigt, wenn Sie dem Benutzer wichtige Informationen bereitstellen.
- **Begrenzen der Anzahl von Kategorien** : um die Komplexität zu reduzieren und Ihre APP zu verwalten, überladen Sie Ihre Registerkarten Leiste nicht mit Kategorien, und stellen Sie sicher, dass alle Kategorien sichtbar und nicht überfüllt sind. Einfache, kurze Titel funktionieren am besten.
- **Deaktivieren Sie eine Kategorie** . alle Registerkarten (Kategorien) sollten immer sichtbar und aktiviert sein. Wenn eine bestimmte Registerkarte keinen Inhalt enthält, sollten Sie eine Erläuterung für den Benutzer bereitstellen. Beispielsweise ist die Registerkarte Käufe leer, wenn der Benutzer keine Einkäufe getätigt hat.

<a name="Tab-Bar-Items" />

## <a name="tab-bar-items"></a>Registerkarten Elemente

Jede Kategorie (Registerkarte) in der Registerkarten Leiste wird durch ein Tabstopp leisten`UITabBarItem`Element () dargestellt. Apple hat die folgenden Vorschläge zum Arbeiten mit Registerkarten Elementen:

- **Textbasierte Registerkarten verwenden** : während das Element der Registerkarten Leiste als Symbol dargestellt werden kann, schlägt Apple die Verwendung von Text nur vor, da ein präziser Titel leichter zu interpretieren ist als ein Symbol.
- **Verwenden Sie kurze, sinnvolle Nomen oder Verben** . ein Tabstopp leisten Element sollte den darin enthaltenen Inhalt eindeutig weiterleiten und am besten funktionieren, wenn es sich um ein einfaches Substantiv (z. b. Fotos, Filme oder Musik) oder Verben (z. b. Suchen oder spielen) handelt.

<a name="Tab-Bars-and-Storyboards" />

## <a name="tab-bars-and-storyboards"></a>Registerkarten und Storyboards

Die einfachste Möglichkeit, mit Tabstopp leisten in einer xamarin. tvos-APP zu arbeiten, besteht darin, Sie mithilfe des IOS-Designers zur Benutzeroberfläche der APP hinzuzufügen.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Starten Sie eine neue xamarin. tvos-APP, und wählen Sie die **tvos** >  >  **-App im** **Register**Kartenformat aus: 

    [![](tab-bars-images/tab02.png "Auswählen der APP im Registerkarten Format")](tab-bars-images/tab02.png#lightbox)
1. Befolgen Sie alle Aufforderungen, um eine neue xamarin. tvos-Lösung zu erstellen.
1. Doppelklicken Sie im **Lösungspad**auf die `Main.storyboard` Datei, und öffnen Sie Sie zur Bearbeitung.
1. Um das **Symbol** oder den **Titel** für eine bestimmte Kategorie zu ändern, wählen Sie in der **Dokument**Gliederung das Element der **Registerkarten Leiste** für den **Ansichts Controller** aus:

    [![](tab-bars-images/tab03a.png "Das Element der Registerkarten Leiste für den Ansichts Controller in der Dokument Gliederung.")](tab-bars-images/tab03a.png#lightbox)
1. Legen Sie dann im **Eigenschaften-Explorer**auf der **Registerkarte widget** die erforderlichen Eigenschaften fest: 

    [![](tab-bars-images/tab03.png "Die Widget-Registerkarte")](tab-bars-images/tab03.png#lightbox)
1. Legen Sie zum Hinzufügen einer neuen Kategorie (Registerkarte) einen **Ansichts Controller** auf der Entwurfs Oberfläche ab: 

    [![](tab-bars-images/tab04.png "Einen Ansichts Controller")](tab-bars-images/tab04.png#lightbox)
1. Klicken und ziehen Sie von der **Registerkarte Ansicht Controller** auf den neuen **Ansichts Controller**.
1. Wählen Sie im Popup Fenster **Controller anzeigen** aus, um die neue Ansicht als Registerkarte (Kategorie) hinzuzufügen: 

    [![](tab-bars-images/tab05.png "Registerkarte auswählen")](tab-bars-images/tab05.png#lightbox)
1. Entwerfen Sie das Layout der Benutzeroberfläche für jeden Inhalt Bereich der einzelnen Kategorien wie üblich, indem Sie Benutzeroberflächen Elemente im IOS-Designer hinzufügen.
1. Machen Sie alle erforderlichen Ereignisse verfügbar, um mit ihren UI C# -Steuerelementen im Code zu arbeiten.
1. Benennen Sie alle UI-Steuerelemente, die Sie C# im Code verfügbar machen möchten.
1. Speichern Sie die Änderungen.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Starten Sie eine neue xamarin. tvos-APP, und wählen Sie die **tvos** >  >  **-App im** **Register**Kartenformat aus: 

    [![](tab-bars-images/tab02vs.png "Auswählen der APP im Registerkarten Format")](tab-bars-images/tab02vs.png#lightbox)
1. Befolgen Sie alle Aufforderungen, um eine neue xamarin. tvos-Lösung zu erstellen.
1. Doppelklicken Sie im **Projektmappen-Explorer**auf die `Main.storyboard` Datei, und öffnen Sie Sie zur Bearbeitung.
1. Um das **Symbol** oder den **Titel** für eine bestimmte Kategorie zu ändern, wählen Sie in der **Dokument**Gliederung das Element der **Registerkarten Leiste** für den **Ansichts Controller** aus:

    [![](tab-bars-images/tab03avs.png "Der Ansichts Controller in der Dokument Gliederung")](tab-bars-images/tab03avs.png#lightbox)
1. Legen Sie dann im **Eigenschaften-Explorer**auf der **Registerkarte widget** die erforderlichen Eigenschaften fest: 

    [![](tab-bars-images/tab03vs.png "Die Widget-Registerkarte")](tab-bars-images/tab03vs.png#lightbox)
1. Ziehen Sie zum Hinzufügen einer neuen Kategorie (Registerkarte) einen **Ansichts Controller** aus der **Toolbox** , und legen Sie ihn auf der Entwurfs Oberfläche ab: 

    [![](tab-bars-images/tab04vs.png "Einen Ansichts Controller")](tab-bars-images/tab04vs.png#lightbox)
1. Klicken und ziehen Sie von der **Registerkarte Ansicht Controller** auf den neuen **Ansichts Controller**.
1. Wählen Sie im Popup Fenster **Controller anzeigen** aus, um die neue Ansicht als Registerkarte (Kategorie) hinzuzufügen: 

    [![](tab-bars-images/tab05vs.png "Registerkarte auswählen")](tab-bars-images/tab05vs.png#lightbox)
1. Entwerfen Sie das Layout der Benutzeroberfläche für jeden Inhalt Bereich der einzelnen Kategorien wie üblich, indem Sie Benutzeroberflächen Elemente im IOS-Designer hinzufügen.
1. Machen Sie alle erforderlichen Ereignisse verfügbar, um mit ihren UI C# -Steuerelementen im Code zu arbeiten.
1. Benennen Sie alle UI-Steuerelemente, die Sie C# im Code verfügbar machen möchten.
1. Speichern Sie die Änderungen.

-----

> [!IMPORTANT]
> Obwohl es möglich ist, Ereignisse wie z `TouchUpInside` . b. einem Benutzeroberflächen Element (z `UIButton`. b.) im IOS-Designer zuzuweisen, wird es nie aufgerufen, da Apple TV keinen Touchscreen hat oder touchereignisse unterstützt. Sie sollten das `Primary Action` -Ereignis immer verwenden, wenn Sie Ereignishandler für tvos-Benutzeroberflächen Elemente erstellen.

Weitere Informationen zum Arbeiten mit Storyboards finden Sie in unserer [Hello-, tvos-Schnellstarthandbuch](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Working-with-Tab-Bars" />

## <a name="working-with-tab-bars"></a>Arbeiten mit Registerkarten

Verwenden Sie `Items` die-Eigenschaft `UITabBar` des, um auf die `UITabBarItems` Auflistung der-Objekte zuzugreifen, die als NULL (0) indiziertes Array enthalten sind. Die `SelectedItem` -Eigenschaft gibt die aktuell ausgewählte Registerkarte (Kategorie) `UITabBarItem`als zurück.

<a name="Working-with-Tab-Bar-Items" />

## <a name="working-with-tab-bar-items"></a>Arbeiten mit Registerkarten Elementen

Verwenden Sie den folgenden Code, um einen Badge auf einer bestimmten Registerkarte (einem roten Oval mit weißem Text) anzuzeigen:

```csharp
// Display a badge
TabBar.Items [2].BadgeValue = "10";
```

Dies führt zu folgenden Ergebnissen, wenn Sie ausgeführt werden:

[![](tab-bars-images/tab06.png "Ein Tabstopp leisten Element mit Badge")](tab-bars-images/tab06.png#lightbox)

Verwenden `UITabBarItem` Sie `Title` die-Eigenschaft des, um den Titel und `Image` die-Eigenschaft zu ändern, um das Symbol zu ändern.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das Entwerfen und arbeiten mit dem Tabstopp leisten Controller in einer xamarin. tvos-App behandelt.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Leitfaden zur APP-Programmierung für tvos](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
