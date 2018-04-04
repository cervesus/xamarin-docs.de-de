---
title: Arbeiten mit Registerkartenleiste-Controller
description: Dieser Artikel umfasst das Entwerfen und Verwenden von innerhalb einer app Xamarin.tvOS Registerkarte Leiste Controller.
ms.prod: xamarin
ms.assetid: 99A2D7C6-0324-4DE5-B6E9-D39D0BAD8370
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 1536e37830f3b2a1e2a83c7bf5039909062d092b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-tab-bar-controller"></a>Arbeiten mit Registerkartenleiste-Controller

_Dieser Artikel umfasst das Entwerfen und Verwenden von innerhalb einer app Xamarin.tvOS Registerkarte Leiste Controller._

Für viele Arten von apps für tvos. außerdem wurden wird die primäre Navigation als eine Registerkartenleiste angezeigt am oberen Rand des Bildschirms ausgeführt. Der Benutzer Kundenkarte links und rechts in der Liste der möglichen Kategorien und Inhaltsbereichs unterhalb der ändert sich entsprechend der Auswahl des Benutzers.

[![](tab-bars-images/tab01.png "Beispiel-Registerkartenleiste")](tab-bars-images/tab01.png#lightbox)

Die Registerkartenleiste ist standardmäßig transparent und immer am oberen Rand des Bildschirms angezeigt. Im Fokus, eine Registerkartenleiste befasst sich mit den oberen 140 Pixel des Bildschirms, jedoch wird schnell unterwegs eingeblendet, sobald der Fokus auf den Inhalt im Bereich unten.

<a name="Tab-Bars-in-tvOS" />

## <a name="tab-bars-in-tvos"></a>Registerkarte "Balken in tvos. außerdem wurden

Die `UITabViewController` funktioniert auf ähnliche Weise und für tvos. außerdem wurden einen ähnlichen Zweck dient, wie in iOS, mit die folgenden wesentlichen Unterschiede:

- Anders als die Registerkartenleiste unter iOS, die am unteren Rand des Bildschirms angezeigt wird, Registerkarte Balken in tvos. außerdem wurden die obersten 140 Pixel des Bildschirms belegen und sind standardmäßig transparent.
- Wenn der Fokus auf die Registerkartenleiste für den Inhaltsbereich unten verlässt, wird die Registerkartenleiste wird schnell schieben Sie vom oberen Rand des Bildschirms und ausgeblendet werden. Der Benutzer kann entweder Tippen Sie auf die Menüschaltfläche einmal oder Streifen oben auf der [Siri Remote](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote) auf die Registerkartenleiste wieder anzuzeigen.
- Streifen unten auf der Remoteinstanz Siri wird Verschieben des Fokus auf den Inhalt im Bereich unterhalb der Registerkartenleiste, mit dem ersten [Element den Fokus erhalten kann](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) im Inhalt angezeigt wird. Dadurch werden erneut, die Registerkartenleiste ausblenden, sobald der Fokus wechselt.
- Wählen Sie eine Kategorie, die in der Registerkartenleiste angezeigt durch Klicken wechselt die mit dem Inhalt der Kategorie und der Fokus zum ersten Element den Fokus erhalten kann in dieser Ansicht gewechselt werden werden.
- Die Anzahl der Kategorien in der Registerkartenleiste angezeigt korrigiert werden soll, und alle Kategorien jederzeit zugegriffen werden soll, eine bestimmte Kategorie sollte nie deaktiviert werden.
- Registerkarte "Balken unterstützen keine Anpassung für tvos. außerdem wurden. Darüber hinaus enthalten sie die **weitere** Kategorie (z. B. iOS) Wenn weitere Kategorien als stehen kann in der Registerkartenleiste passen.

Apple hat die folgenden Vorschläge für die Arbeit mit Registerkarte Balken:

- **Verwendung Registerkarte Balken logisch organisiert Inhalt** -verwenden Sie die Registerkartenleiste logisch Organisation von Inhalten, die Ihre app tvos. außerdem wurden mit arbeitet. Beispielsweise Top, Top-Diagrammen, erworben und Suche.
- **Informieren Sie Benutzer der neuen Inhalt Signale hinzufügen** -Optional können Sie ein Signal (ein Rot Oval mit einem weißen Anzahl oder Ausrufezeichen) anzeigen, um den Benutzer, der neue Inhalt in einer Kategorie zu informieren.
- **Signale sparsam verwenden** – nicht die Registerkartenleiste mit Signale überlasten und nur anzeigen, in denen sie wichtigen Informationen für den Benutzer bereitstellen.
- **Beschränken Sie die Anzahl der Kategorien** : Informationen zum Komplexität zu reduzieren und verhindert, dass Sie app verwaltet, nicht überladen der Registerkartenleiste mit Kategorien und stellen Sie sicher, dass alle Kategorien angezeigt werden und nicht überfüllt wird. Einfache, short Titel funktionieren am besten.
- **Deaktivieren Sie eine Kategorie keine** -aller Registerkarten (Rubriken) sollte immer sichtbar und aktiviert jederzeit sein. Wenn eine angegebene Registerkarte keinen Inhalt, warum eine Erklärung für den Benutzer bereitgestellt hat. Die Registerkarte "kauft" werden z. B. leer, wenn der Benutzer keine Einkäufe getätigt hat.

<a name="Tab-Bar-Items" />

## <a name="tab-bar-items"></a>Registerkartenleiste-Elemente

Jede Kategorie (Registerkarte) in der Registerkartenleiste wird durch ein Registerkartenelement-Balken dargestellt (`UITabBarItem`). Apple hat die folgenden Vorschläge für die Arbeit mit Leiste Registerkartenelemente:

- **Verwenden Sie die Registerkarten des Text-Basis** -während der Leiste Registerkartenelement wird als Symbol dargestellt werden können, Apple empfiehlt die Verwendung von Text, da ein präziser Titel einfacher als ein Symbol zu interpretieren ist.
- **Verwenden Sie kurze, sinnvolle Nomen oder Verben** -ein Registerkartenelement-Leiste sollte klar Weiterleiten der Inhalte, die es enthält und funktioniert am besten, wenn es sich um ein einfaches Nomen (z. B. Fotos, Videos und Musik) oder Verben (z. B. Suche oder Play) ist.

<a name="Tab-Bars-and-Storyboards" />

## <a name="tab-bars-and-storyboards"></a>Registerkarte "Balken und Storyboards

Die einfachste Möglichkeit zum Arbeiten mit Registerkarte Balken in einer app Xamarin.tvOS werden diese Benutzeroberfläche der Anwendung, die mit der iOS-Designer hinzufügen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)
    
1. Starten Sie eine neue Xamarin.tvOS-app, und wählen Sie **tvos. außerdem wurden** > **App** > **im Registerkartenformat App**: 

    [![](tab-bars-images/tab02.png "Wählen Sie im Registerkartenformat App")](tab-bars-images/tab02.png#lightbox)
1. Führen Sie die Anweisungen zum Erstellen einer neuen Xamarin.tvOS-Lösung.
1. In der **Lösung Pad**, doppelklicken Sie auf die `Main.storyboard` Datei und öffnet ihn zur Bearbeitung.
1. So ändern Sie die **Symbol** oder **Titel** nach einer bestimmten Kategorie auswählen der **Leiste Registerkartenelement** für die **View Controller** in der  **Dokumentgliederung**:

    [![](tab-bars-images/tab03a.png "Die Registerkartenleiste Element, für die View-Controller in der Dokumentgliederung")](tab-bars-images/tab03a.png#lightbox)
1. Legen Sie dann die erforderlichen Eigenschaften der **Registerkarte "Widget"** von der **Eigenschaften-Explorer**: 

    [![](tab-bars-images/tab03.png "Die Registerkarte "Widget"")](tab-bars-images/tab03.png#lightbox)
1. Fügen Sie eine neue Kategorie (Registerkarte) hinzu, indem eine **Modellansichtcontroller** auf der Entwurfsoberfläche: 

    [![](tab-bars-images/tab04.png "Eine View-Controller")](tab-bars-images/tab04.png#lightbox)
1. Steuerelement klicken und ziehen Sie aus der **Registerkarte-View-Controller** mit dem neuen **Modellansichtcontroller**.
1. Wählen Sie im Popupmenü **anzeigen Controller** So fügen Sie die neue Ansicht als Registerkarte (Kategorie "") hinzu: 

    [![](tab-bars-images/tab05.png "Wählen Sie die Registerkarte "")](tab-bars-images/tab05.png#lightbox)
1. Entwerfen des Layouts für jede Caterogies Inhaltsbereichs als normaler Schrift, die Benutzeroberfläche durch Hinzufügen von UI-Elemente in der iOS-Designer.
1. Machen Sie alle erforderlichen Ereignisse zur Bearbeitung von UI-Steuerelemente in C#-Code verfügbar.
1. Benennen Sie alle UI-Steuerelemente, die Sie in C#-Code verfügbar machen möchten.
1. Speichern Sie die Änderungen.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. Starten Sie eine neue Xamarin.tvOS-app, und wählen Sie **tvos. außerdem wurden** > **App** > **im Registerkartenformat App**: 

    [![](tab-bars-images/tab02vs.png "Wählen Sie im Registerkartenformat App")](tab-bars-images/tab02vs.png#lightbox)
1. Führen Sie die Anweisungen zum Erstellen einer neuen Xamarin.tvOS-Lösung.
1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Main.storyboard` Datei und öffnet ihn zur Bearbeitung.
1. So ändern Sie die **Symbol** oder **Titel** nach einer bestimmten Kategorie auswählen der **Leiste Registerkartenelement** für die **View Controller** in der  **Dokumentgliederung**:

    [![](tab-bars-images/tab03avs.png "Die View-Controller in der Dokumentgliederung")](tab-bars-images/tab03avs.png#lightbox)
1. Legen Sie dann die erforderlichen Eigenschaften der **Registerkarte "Widget"** von der **Eigenschaften-Explorer**: 

    [![](tab-bars-images/tab03vs.png "Die Registerkarte "Widget"")](tab-bars-images/tab03vs.png#lightbox)
1. Um eine neue Kategorie (Registerkarte) hinzuzufügen, ziehen Sie eine **Modellansichtcontroller** aus der **Toolbox** und legen Sie sie auf der Entwurfsoberfläche: 

    [![](tab-bars-images/tab04vs.png "Eine View-Controller")](tab-bars-images/tab04vs.png#lightbox)
1. Steuerelement klicken und ziehen Sie aus der **Registerkarte-View-Controller** mit dem neuen **Modellansichtcontroller**.
1. Wählen Sie im Popupmenü **anzeigen Controller** So fügen Sie die neue Ansicht als Registerkarte (Kategorie "") hinzu: 

    [![](tab-bars-images/tab05vs.png "Wählen Sie die Registerkarte "")](tab-bars-images/tab05vs.png#lightbox)
1. Entwerfen des Layouts für jede Caterogies Inhaltsbereichs als normaler Schrift, die Benutzeroberfläche durch Hinzufügen von UI-Elemente in iOS-Designer.
1. Machen Sie alle erforderlichen Ereignisse zur Bearbeitung von UI-Steuerelemente in C#-Code verfügbar.
1. Benennen Sie alle UI-Steuerelemente, die Sie in C#-Code verfügbar machen möchten.
1. Speichern Sie die Änderungen.
    
-----

> [!IMPORTANT]
> Während es möglich ist, z. B. Ereignisse weisen `TouchUpInside` auf ein UI-Element (z. B. eine `UIButton`) in der iOS-Designer, niemals aufgerufen wird da Apple TV Fingereingabe Bildschirm oder Berührungsereignisse unterstützen keine. Sie sollten immer verwenden die `Primary Action ` Ereignis aus, wenn Ereignishandler für tvos. außerdem wurden die Elemente der Benutzeroberfläche erstellen.

Weitere Informationen zum Arbeiten mit Storyboards finden Sie unter unsere [Hello tvos. außerdem wurden Quick Start Guide](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Working-with-Tab-Bars" />

## <a name="working-with-tab-bars"></a>Arbeiten mit Registerkarte Balken

Verwenden der `Items` Eigenschaft von der `UITabBar` zum Zugriff auf die Auflistung der `UITabBarItems` sie als null (0) indiziertes Array enthält. Die `SelectedItem` Eigenschaft gibt zurück, die derzeit ausgewählte Registerkarte (Kategorie "") als eine `UITabBarItem`.


<a name="Working-with-Tab-Bar-Items" />

## <a name="working-with-tab-bar-items"></a>Arbeiten mit Elementen Registerkartenleiste

Um ein Signal auf einer angegebenen Registerkarte (ein Rot Oval mit weißem Text) anzuzeigen, verwenden Sie den folgenden Code ein:

```csharp
// Display a badge
TabBar.Items [2].BadgeValue = "10";
```

Erzeugt die folgende Ergebnisse beim Ausführen ein:

[![](tab-bars-images/tab06.png "Ein Registerkartenelement-Leiste mit badge")](tab-bars-images/tab06.png#lightbox)

Verwenden der `Title` Eigenschaft von der `UITabBarItem` so ändern Sie den Titel und die `Image` Eigenschaft, die das Symbol "geändert.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, Entwerfen und Verwenden von innerhalb einer app Xamarin.tvOS Registerkarte Leiste Controller.




## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos. außerdem wurden Handbücher für interaktive Workflowdienste-Schnittstelle](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos. außerdem wurden](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
