---
title: Arbeiten mit TvOS Registerkarte Leiste Controller in Xamarin
description: Dieses Dokument beschreibt das Arbeiten mit Registerkarte Leiste Controllern in einer TvOS-app mit Xamarin erstellt wurde. Es bietet eine allgemeine über die Ansicht der Registerkartenleisten und erläutert die Leiste Registerelemente, Storyboard-Integration und Registerkartenelemente.
ms.prod: xamarin
ms.assetid: 99A2D7C6-0324-4DE5-B6E9-D39D0BAD8370
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 7ac4d0effc1067b065bad114160dc8648e998dad
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67830781"
---
# <a name="working-with-tvos-tab-bar-controllers-in-xamarin"></a>Arbeiten mit TvOS Registerkarte Leiste Controller in Xamarin

Für viele Arten von TvOS-apps wird die primäre Navigation als Registerkartenleiste angezeigt am oberen Rand des Bildschirms ausgeführt. Der Benutzer Sie mit einer wischbewegung links und rechts in der Liste der möglichen Kategorien und den Inhaltsbereich unter den Änderungen entsprechend der Auswahl des Benutzers.

[![](tab-bars-images/tab01.png "Beispiel-Registerkartenleiste")](tab-bars-images/tab01.png#lightbox)

Die Registerkartenleiste ist standardmäßig transparent und immer am oberen Rand des Bildschirms angezeigt wird. Im Fokus, Registerkartenleiste behandelt die oberen 140 Pixel des Bildschirms, jedoch wird schnell Weg schieben, wenn der Fokus wechselt, um den Inhaltsbereich unten.

<a name="Tab-Bars-in-tvOS" />

## <a name="tab-bars-in-tvos"></a>Registerkartenleisten in tvOS

Die `UITabViewController` funktioniert auf ähnliche Weise und unter TvOS einen ähnlichen Zweck dient, wie in iOS, wobei die folgenden wesentlichen Unterschiede:

- Im Gegensatz zu der Registerkartenleiste für iOS am unteren Rand des Bildschirms angezeigt wird, Registerkartenleisten in TvOS belegen die oberen 140 Pixel des Bildschirms und sind standardmäßig transparent.
- Wenn der Fokus auf der Registerkartenleiste für den Inhaltsbereich unten verlässt, wird der Registerkartenleiste wird schnell schieben Sie vom oberen Rand des Bildschirms und ausgeblendet werden. Der Benutzer kann entweder Tippen Sie einmal auf die Menüschaltfläche, oder wischen Sie auf die [Siri Remote](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote) Registerkarte erneut anzeigen.
- Wischen nach unten auf dem Remotecomputer Siri wird verschiebt den Fokus auf den Inhaltsbereich unter der Registerkarte angezeigt, mit dem ersten [fokussierbare Element](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) im Inhalt angezeigt wird. In diesem Fall wird dadurch die Registerkartenleiste ausgeblendet, sobald der Fokus wechselt.
- Klicken auf die Auswahl einer Kategorie angezeigt, auf die Registerkarte wechselt, Kategorie Inhalt und den Fokus auf das erste fokussierbare Element in dieser Ansicht gewechselt werden.
- Die Anzahl der Kategorien, die in der Registerkartenleiste angezeigt behoben werden sollte und alle Kategorien müssen jederzeit zugänglich sein, eine bestimmte Kategorie sollte nie deaktiviert werden.
- Registerkartenleisten unterstützen keine Anpassung unter TvOS. Darüber hinaus enthalten sie die **weitere** Kategorie (z. B. iOS) Wenn weitere Kategorien als stehen kann auf die Registerkarte passen.

Apple hat die folgenden Vorschläge für die Arbeit mit Registerkartenleisten:

- **Mit Registerkartenleisten logisch zu organisieren Inhalt** -der Registerkartenleiste verwenden, um den Inhalt, mit denen TvOS-app zusammenarbeitet, logisch zu organisieren. Beispielsweise ausgewähltes "," Top-Diagramme "," Purchased "und" suchen.
- **Fügen Sie Badges auf Benutzer der neuen Inhalt informiert** – Optional können Sie einen Badge (ein Rot "-Oval" mit einem weißen Anzahl oder Ausrufezeichen) anzeigen, um den Benutzer des neuen Inhalts in einer Kategorie zu informieren.
- **Verwenden Sie Badges sparsam** – nicht der Registerkartenleiste mit Badges mit überflüssigen Daten gefüllt und nur angezeigt, in dem sie wichtigen Informationen für den Benutzer bereitstellen.
- **Beschränken Sie die Anzahl der Kategorien** –, um Komplexität verringern und halten Sie app verwaltet, nicht überladen Ihre Registerkartenleiste mit Kategorien und stellen Sie sicher, dass alle Kategorien angezeigt werden und nicht überfüllt wird. Einfache, kurze Titel funktionieren am besten.
- **Deaktivieren Sie eine Kategorie keine** -alle Registerkarten (Kategorien) sollten immer sichtbar und aktiviert jederzeit sein. Wenn eine angegebene Registerkarte keinen Inhalt hat, die bereitstellen Sie eine Erklärung, warum für den Benutzer. Die Registerkarte "kauft" werden z. B. leer, wenn der Benutzer keine Einkäufe getätigt hat.

<a name="Tab-Bar-Items" />

## <a name="tab-bar-items"></a>Registerkartenleiste-Elemente

Jede Kategorie (Registerkarte), auf die Registerkarte wird durch ein Element der Registerkartenleiste dargestellt (`UITabBarItem`). Apple hat die folgenden Vorschläge für die Arbeit mit Registerkartenelemente:

- **Verwenden Sie Text basierend Registerkarten** – während das Element der Registerkartenleiste kann als Symbol dargestellt werden, dabei wird Text nur, weil Sie ein präziser Titel einfacher als ein Symbol zu interpretieren ist Apple empfiehlt.
- **Verwenden Sie kurze, aussagekräftige Substantive und Verben** -ein Element der Registerkartenleiste sollte klar relay den Inhalt, der ihn enthält, und funktioniert am besten, wenn es sich um ein einfaches Nomen (z. B. Fotos, Videos und Musik) oder Verben (z. B. "Suche" oder "Play") ist.

<a name="Tab-Bars-and-Storyboards" />

## <a name="tab-bars-and-storyboards"></a>Registerkartenleisten und Storyboards

Die einfachste Möglichkeit zum Arbeiten mit Registerkartenleisten in einer Xamarin.tvOS-app ist so fügen sie der app Benutzeroberfläche, die Verwendung des iOS Designers hinzu.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)
    
1. Starten einer neuen Xamarin.tvOS-app, und wählen Sie **TvOS** > **App** > **im Registerkartenformat App**: 

    [![](tab-bars-images/tab02.png "Wählen Sie die Registerkarten-App")](tab-bars-images/tab02.png#lightbox)
1. Führen Sie alle eingabeaufforderungen zum Erstellen einer neuen Xamarin.tvOS-Lösung.
1. In der **Lösungspad**, doppelklicken Sie auf die `Main.storyboard` und öffnen Sie sie für die Bearbeitung.
1. So ändern Sie die **Symbol** oder **Titel** für eine angegebene Kategorie ist, wählen Sie die **Element der Registerkartenleiste** für die **Ansichtscontroller** in die  **Dokumentgliederung**:

    [![](tab-bars-images/tab03a.png "Die Registerkartenleiste-Element für den Ansichtscontroller, in der Dokumentgliederung")](tab-bars-images/tab03a.png#lightbox)
1. Legen Sie dann die erforderlichen Eigenschaften der **Widget Registerkarte** von der **Eigenschaften-Explorer**: 

    [![](tab-bars-images/tab03.png "Die Registerkarte \"Widget\"")](tab-bars-images/tab03.png#lightbox)
1. Fügen Sie eine neue Kategorie (Registerkarte) hinzu, indem eine **Ansichtscontroller** auf Ihrer Designoberfläche: 

    [![](tab-bars-images/tab04.png "Einen Ansichtscontroller")](tab-bars-images/tab04.png#lightbox)
1. Strg + Klick und ziehen Sie aus der **Registerkarte Ansichtscontroller** mit dem neuen **Ansichtscontroller**.
1. Wählen Sie im Popupmenü **anzeigen Controller** die neue Ansicht als Registerkarte (Kategorie) hinzufügen: 

    [![](tab-bars-images/tab05.png "Wählen Sie die Registerkarte \"")](tab-bars-images/tab05.png#lightbox)
1. Entwerfen Sie das Layout der Benutzeroberfläche für die einzelnen Bereiche der Caterogies Inhalt wie gewohnt, durch das Hinzufügen von UI-Elemente in der iOS-Designer.
1. Machen Sie alle erforderlichen Ereignisse an die Arbeit mit UI-Steuerelemente in C# Code.
1. Benennen Sie die UI-Steuerelementen, die Sie in verfügbar machen möchten C# Code.
1. Speichern Sie die Änderungen.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
1. Starten einer neuen Xamarin.tvOS-app, und wählen Sie **TvOS** > **App** > **im Registerkartenformat App**: 

    [![](tab-bars-images/tab02vs.png "Wählen Sie die Registerkarten-App")](tab-bars-images/tab02vs.png#lightbox)
1. Führen Sie alle eingabeaufforderungen zum Erstellen einer neuen Xamarin.tvOS-Lösung.
1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Main.storyboard` und öffnen Sie sie für die Bearbeitung.
1. So ändern Sie die **Symbol** oder **Titel** für eine angegebene Kategorie ist, wählen Sie die **Element der Registerkartenleiste** für die **Ansichtscontroller** in die  **Dokumentgliederung**:

    [![](tab-bars-images/tab03avs.png "Der Ansichtscontroller, in der Dokumentgliederung")](tab-bars-images/tab03avs.png#lightbox)
1. Legen Sie dann die erforderlichen Eigenschaften der **Widget Registerkarte** von der **Eigenschaften-Explorer**: 

    [![](tab-bars-images/tab03vs.png "Die Registerkarte \"Widget\"")](tab-bars-images/tab03vs.png#lightbox)
1. Um eine neue Kategorie (Registerkarte) hinzuzufügen, ziehen Sie eine **Ansichtscontroller** aus der **Toolbox** und legen Sie es auf Ihrer Designoberfläche: 

    [![](tab-bars-images/tab04vs.png "Einen Ansichtscontroller")](tab-bars-images/tab04vs.png#lightbox)
1. Strg + Klick und ziehen Sie aus der **Registerkarte Ansichtscontroller** mit dem neuen **Ansichtscontroller**.
1. Wählen Sie im Popupmenü **anzeigen Controller** die neue Ansicht als Registerkarte (Kategorie) hinzufügen: 

    [![](tab-bars-images/tab05vs.png "Wählen Sie die Registerkarte \"")](tab-bars-images/tab05vs.png#lightbox)
1. Entwerfen Sie das Layout der Benutzeroberfläche für die einzelnen Bereiche der Caterogies Inhalt wie gewohnt, durch das Hinzufügen von UI-Elemente in der iOS-Designer.
1. Machen Sie alle erforderlichen Ereignisse an die Arbeit mit UI-Steuerelemente in C# Code.
1. Benennen Sie die UI-Steuerelementen, die Sie in verfügbar machen möchten C# Code.
1. Speichern Sie die Änderungen.
    
-----

> [!IMPORTANT]
> Es ist zwar möglich, weisen Sie Ereignisse wie z. B. `TouchUpInside` für ein Benutzeroberflächenelement (wie z. B. eine `UIButton`) in der iOS-Designer, niemals aufgerufen wird da Apple TV besitzt eine Fingereingabe Bildschirm oder Touch-Ereignissen zu unterstützen. Verwenden Sie immer die `Primary Action` Ereignis aus, wenn der Ereignishandler für tvos verwendet. die Elemente der Benutzeroberfläche erstellen.

Weitere Informationen zum Arbeiten mit Storyboards, informieren Sie sich unsere [Hello, TvOS – Kurzanleitung](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Working-with-Tab-Bars" />

## <a name="working-with-tab-bars"></a>Arbeiten mit Registerkartenleisten

Verwenden der `Items` Eigenschaft der `UITabBar` zum Zugriff auf die Auflistung der `UITabBarItems` er als null (0) indiziertes Array enthält. Die `SelectedItem` Eigenschaft gibt zurück, die derzeit ausgewählte Registerkarte (Kategorie) als eine `UITabBarItem`.


<a name="Working-with-Tab-Bar-Items" />

## <a name="working-with-tab-bar-items"></a>Arbeiten mit Elementen der Registerkartenleiste

Um einen Badge auf einer angegebenen Registerkarte (ein Rot Oval mit weißem Text) anzuzeigen, verwenden Sie den folgenden Code:

```csharp
// Display a badge
TabBar.Items [2].BadgeValue = "10";
```

Das würde die folgenden Ergebnisse beim Ausführen erzeugt:

[![](tab-bars-images/tab06.png "Ein Element der Registerkartenleiste mit badge")](tab-bars-images/tab06.png#lightbox)

Verwenden der `Title` Eigenschaft der `UITabBarItem` zum Ändern des Titels und der `Image` Eigenschaft, die das Symbol geändert.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, Entwerfen und Arbeiten mit Registerkarten, in ein Xamarin.tvOS-app.




## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [TvOS Human Interface-Handbücher](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos verwendet.](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
