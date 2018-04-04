---
title: Suchen und Startbildschirm Widget-Erweiterungen
description: Dieser Artikel behandelt die Erweiterungen, die mit dem Widget-System in iOS 10 Apple vereinbart hat.
ms.prod: xamarin
ms.assetid: D66FD9E1-9E23-4BB6-825C-ED19B8F72A81
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: e7a64738f29ab2b5c62659d721beb50db7c9adb5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="search-and-home-screen-widget-enhancements"></a>Suchen und Startbildschirm Widget-Erweiterungen

_Dieser Artikel behandelt die Erweiterungen, die mit dem Widget-System in iOS 10 Apple vereinbart hat._


Apple wurde eingeführt, mehrere Erweiterungen für das System Widget, um sicherzustellen, dass die Widgets auf eine im Hintergrund laufende hervorragend aussehen, die auf dem neuen iOS-Sperrbildschirm 10 vorhanden ist. Darüber hinaus enthalten Widgets jetzt eine [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) -Eigenschaft, kann der Entwickler wird beschrieben, wie viel Inhalt verfügbar ist, und ermöglicht es dem Benutzer zum Erweitern und reduzieren den Inhalt.

Widgets (auch bekannt als heute Extensions) sind eine besondere Art von iOS Erweiterung, die eine kleine Menge von nützlichen Informationen anzuzeigen oder app-spezifische Funktionalität rechtzeitig verfügbar zu machen. Beispielsweise die Nachrichten-app hat ein Widget, das die Schlagzeilen anzeigt und die Kalender-app bietet zwei verschiedene Widgets: eine zum Anzeigen der heutigen Ereignisse und eine anzuzeigende bevorstehender Ereignisse.

Widgets sind umfassend anpassbar und können die UI-Elemente, z. B. Text, Bilder, Schaltflächen usw. enthalten. Darüber hinaus kann Entwickler das Layout von ihren Widgets weiter anpassen.

[![](widgets-images/widgets01.png "Beispiel-widgets")](widgets-images/widgets01.png#lightbox)

Es gibt zwei wichtigsten Bereiche, die ein Benutzer anzeigen und interagieren mit einer app Widgets:

- **Der Bildschirm zum Suchen von** -Benutzer können die sie nützlichsten auf ihren Bildschirm zum Suchen von gefunden Widgets hinzufügen. Der Bildschirm zum Suchen von erfolgt durch ein Lesegerät rechts auf der Startseite und die Sperre Bildschirmen.
- **Der Startbildschirm** -aus der Startbildschirm, der Benutzer können 3D Touch um der Liste "Aktionen" schnelle durch Anwenden von Druck auf das Symbol für die app zu öffnen. Eine app Widgets werden oberhalb der Liste der schnelle Aktionen angezeigt. Wenden Sie sich unsere [Einführung in 3D Touch](~/ios/platform/3d-touch.md) Dokumentation weitere Informationen.

## <a name="widgets-developer-suggestions"></a>Widgets Developer-Vorschlägen

Der Entwickler sollte idealerweise immer versuchen und Entwerfen von Widgets, die der Benutzer für ihre Suche Bildschirme hinzufügen möchten. Zu diesem Zweck hat Apple die folgenden Vorschläge:

- **Erstellen Sie eine gute, anzeigbare Erfahrung** -Benutzers Widgets, mit denen enthalten eine kurze, anzeigbare Informationen von statusaktualisierungen oder ermöglicht es ihnen, schnell einfache Aufgaben werden soll. Dadurch wird die richtige Menge an Informationen und Interaktivität ein unverzichtbares bereitstellen. Wann immer möglich, können Sie den Benutzer zum Ausführen eines bestimmten Tasks mit einem einfachen tippen. Da Widgets schwenken oder einen Bildlauf nicht unterstützt wird, wird dies darüber hinaus müssen in das Widget Entwurf berücksichtigt werden.
- **Inhalte schnell anzeigen** -Widgets dienen als anzeigbare, sodass der Benutzer sollte nie warten, dass Inhalt geladen wird, sobald ein Widget angezeigt wird. Widgets sollte ihren Inhalt zwischenspeichern, lokal, sodass sie aktuelle Inhalt nicht angezeigt werden können, während neuer Inhalte im Hintergrund geladen wird.
- **Geben Sie entsprechende Auffüllung und Rändern** -Widgets sollte nie suchen Rechenzentrum weiter aufzurüsten, vermeiden Sie Inhalt an den Rändern des Widgets Sicht zu erweitern. Es sollte immer einen mehrere Pixel breit Rand zwischen den Rändern und der Inhalt vorhanden sein. Apple empfiehlt, auch die app-Symbol, die im oberen Bereich des Widgets, wie eine Ausrichtung führt angezeigt. Wenn das Widget ein gitternetzlayout präsentiert, sicherzustellen Sie, dass es richtigen Abstand zwischen den Elementen im Raster, und versuchen Sie begrenzen die Anzahl der Elemente in vier Max.
- **Verwenden Sie anwendbare Layouts** -A-Widget Breite variiert, je nachdem, auf das Gerät dieses ausgeführt wird und die Ausrichtung des Geräts. Höhe des Widgets kann auch basierend auf variieren, wenn sie in einem Collapsed (Standard) oder erweitert (nicht durch alle Widgets unterstützt) angezeigt wird. Ein Widget reduziert hat eine Höhe von ungefähr 2,5 standard iOS Tabellenzeilen. Der Entwickler kann die Größe für ein Widget erweitert anfordern, aber es sollte idealerweise niedriger sein als die Höhe des Bildschirms. Das Widget sollte in den Zustand "reduziert" nur wichtige, eigenständige Informationen angezeigt werden. Wenn erweitert, das Widget zusätzliche Informationen werden, die der primären Inhalt im Status "reduziert" verbessert angezeigt sollen. Widgets angezeigt, in der Liste der schnelle Aktionen werden nur im Status "reduziert".
- **Nicht Anpassen des Widgets Hintergrund** -Widgets in einem Hintergrundthread hell, weichgezeichnete, die vom System bereitgestellte angezeigt werden. Dies erfolgt, um Konsistenz zwischen Widgets heraufstufen und verbessern die Lesbarkeit des Inhalts. Verwenden Sie ein Bild als Hintergrund Widget, da er mit der Benutzer sperren und der Startbildschirm Hintergrundbilder miteinander in Konflikt geraten kann.
- **Verwenden Sie die System-Schriftart in Schwarz oder dunkelgrau** – Anzeigen von Text in einem Widget "", der Systemschriftart funktioniert am besten. Die Schriftart sollte in einer schwarzen oder dunklen graue Farbe Untergrund Widget hell, weichgezeichnete einzigartig sein.
- **App Zugriff beim entsprechenden bieten** -Widget sollten immer separat aus ihrer app arbeiten, jedoch, wenn eine umfassendere Funktionen erforderlich ist, das Widget kann zum Starten der app anzeigen oder bearbeiten eine bestimmte Information. Nie eine Schaltfläche "Öffnen der app" einschließen einfach ermöglicht dem Benutzer die Inhalte selbst Tippen Sie auf, und öffnen Sie niemals einen 3rd party app.
- **Wählen Sie einen löschen, präzise Namen des Widgets** -Symbol für die app und das Widget-Name werden immer über den Widget-Inhalte angezeigt. Apple wird vorgeschlagen, mit der App-Namen für die primäre Widget und eine klare und prägnante Namen für alle weiteren bietet. Bei der Bereitstellung von einem benutzerdefinierten Widget-Titel, sollten sie die app-Namen (z. B. Zuordnungen in der Nähe, Maps Restaurants usw.) vorangestellt werden.
- **Zu informieren, wenn Authentifizierung profitieren** - Zusatzfunktionen oder Informationen zur Verfügung nur, wenn der Benutzer authentifiziert wurde und dies angemeldet, für den Benutzer vorhanden. Beispielsweise eine Freigabe-app my Say "Melden Sie sich bei Buch fuhr" fuhr.
- **Wählen Sie eine schnelle Aktion Liste Widget** -Wenn die app mehrere Widget bereitstellt, sollten Entwickler zu präsentieren, wenn der Benutzer sich schnell Aktionsliste bringt, durch Anwenden von Druck auf das Symbol für die app 3D Touch mit auswählen.

Weitere Informationen zum Arbeiten mit Widgets finden Sie unter unsere [Einführung in die Erweiterungen](~/ios/platform/extensions.md), [Einführung in 3D Touch](~/ios/platform/3d-touch.md) Dokumentation und Apple [App-Erweiterung-Programmierhandbuch](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html).

## <a name="working-with-vibrancy"></a>Arbeiten mit Dynamik

Dynamik wird sichergestellt, dass ein Widget Text lesbar, wenn auf das Widget Licht, weichgezeichnete Hintergrund (vom System bereitgestellten) dargestellt bleibt. Vor iOS 10, würde der Entwickler verwenden ein [NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect) für das Widget Dynamik. Zum Beispiel:

```csharp
// DEPRECATED: Get Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreateForNotificationCenter ();
```

Dies wurde als veraltet markiert werden iOS 10 und sollten ersetzt werden, entweder mit einem [WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect) oder ein [WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect). Zum Beispiel:

```csharp
// Get Primary Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreatePrimaryVibrancyEffectForNotificationCenter ();

// Get Secondary Widget Vibrancy Effect
var vibrancy2 = UIVibrancyEffect.CreateSecondaryVibrancyEffectForNotificationCenter ();
```

## <a name="working-with-collapsed-and-expanded-widgets"></a>Arbeiten mit reduzierten und erweiterten Widgets

Neu für iOS 10, Widgets jetzt enthalten eine [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) -Eigenschaft, kann der Entwickler wird beschrieben, wie viel Inhalt verfügbar ist, und ermöglicht es dem Benutzer zum Erweitern und reduzieren den Inhalt.

Wenn ein Widget anfänglich angezeigt wird, ist es in einem Zustand reduziert. Ein Widget reduziert hat eine Höhe von ungefähr 2,5 standard iOS Tabellenzeilen. Der Entwickler kann die Größe für ein Widget erweitert anfordern, aber es sollte idealerweise niedriger sein als die Höhe des Bildschirms. 

Das Widget sollte in den Zustand "reduziert" nur wichtige, eigenständige Informationen angezeigt werden. Wenn erweitert, das Widget zusätzliche Informationen werden, die der primären Inhalt im Status "reduziert" verbessert angezeigt sollen. Beispielsweise Wetter-app zeigt die aktuelle Wettervorhersage Wenn reduziert, und fügt die stündlich prognostizieren, wenn die Kategorie erweitert.

Widgets angezeigt, in der Liste der schnelle Aktionen werden nur im Status "reduziert". Wenn die app mehrere Widget bereitstellt, sollten Entwickler zu präsentieren, wenn der Benutzer sich schnell Aktionsliste bringt, durch Anwenden von Druck auf das Symbol für die app 3D Touch mit auswählen.

Im folgende Beispiel wird eine einfache heute Erweiterung (Widget), die die Zustände reduziert und erweitert behandelt:

```csharp
using System;
using NotificationCenter;
using Foundation;
using UIKit;
using CoreGraphics;

namespace MonkeyAbout
{
    public partial class TodayViewController : UIViewController, INCWidgetProviding
    {
        protected TodayViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Tell widget it can be expanded
            ExtensionContext.SetWidgetLargestAvailableDisplayMode (NCWidgetDisplayMode.Expanded);

            // Get the maximum size
            var maxSize = ExtensionContext.GetWidgetMaximumSize (NCWidgetDisplayMode.Expanded);
        }

        [Export ("widgetPerformUpdateWithCompletionHandler:")]
        public void WidgetPerformUpdate (Action<NCUpdateResult> completionHandler)
        {
            // Take action based on the display mode
            switch (ExtensionContext.GetWidgetActiveDisplayMode()) {
            case NCWidgetDisplayMode.Compact:
                Content.Text = "Let's Monkey About!";
                break;
            case NCWidgetDisplayMode.Expanded:
                Content.Text = "Gorilla!!!!";
                break;
            }

            // Report results
            // If an error is encoutered, use NCUpdateResultFailed
            // If there's no update required, use NCUpdateResultNoData
            // If there's an update, use NCUpdateResultNewData
            completionHandler (NCUpdateResult.NewData);
        }

        [Export ("widgetActiveDisplayModeDidChange:withMaximumSize:")]
        public void WidgetActiveDisplayModeDidChange (NCWidgetDisplayMode activeDisplayMode, CGSize maxSize)
        {
            // Take action based on the display mode
            switch (activeDisplayMode) {
            case NCWidgetDisplayMode.Compact:
                PreferredContentSize = maxSize;
                Content.Text = "Let's Monkey About!";
                break;
            case NCWidgetDisplayMode.Expanded:
                PreferredContentSize = new CGSize (0, 200);
                Content.Text = "Gorilla!!!!";
                break;
            }
        }

    }
}
```

Betrachten Sie das Widget Anzeigemodus bestimmten Code im Detail. Um dem System zu informieren, dass dieses Widget den Expanded-Status unterstützt, wird wie folgt vor:

```csharp
// Tell widget it can be expanded
ExtensionContext.SetWidgetLargestAvailableDisplayMode (NCWidgetDisplayMode.Expanded);
```

Um das Widget aktuelle Anzeigemodus zu erhalten, wird wie folgt vor:

```csharp
ExtensionContext.GetWidgetActiveDisplayMode()
```

Zum Abrufen der maximalen Größe für den Status der reduziert oder erweitert, wird wie folgt vor:

```csharp
// Get the maximum size
var maxSize = ExtensionContext.GetWidgetMaximumSize (NCWidgetDisplayMode.Expanded);
```

Und behandeln Sie den Status (Anzeigemodus) ändern verwendet:

```csharp
[Export ("widgetActiveDisplayModeDidChange:withMaximumSize:")]
public void WidgetActiveDisplayModeDidChange (NCWidgetDisplayMode activeDisplayMode, CGSize maxSize)
{
    // Take action based on the display mode
    switch (activeDisplayMode) {
    case NCWidgetDisplayMode.Compact:
        PreferredContentSize = maxSize;
        Content.Text = "Let's Monkey About!";
        break;
    case NCWidgetDisplayMode.Expanded:
        PreferredContentSize = new CGSize (0, 200);
        Content.Text = "Gorilla!!!!";
        break;
    }
}
```

Zusätzlich zum Festlegen der angeforderten Größe für jeden Zustand (reduziert oder erweitert), wird auch den Inhalt wird angezeigt, um die neue Größe angleichen aktualisiert.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel behandelt die Erweiterungen, die mit dem Widget-System in iOS 10 Apple vereinbart hat und gezeigt, wie sie in der Xamarin.iOS implementieren.



## <a name="related-links"></a>Verwandte Links

- [iOS-10-Beispiele](https://developer.xamarin.com/samples/ios/iOS10/)
- [Einführung in die Erweiterungen](~/ios/platform/extensions.md)
- [Einführung in 3D Touch](~/ios/platform/3d-touch.md)
- [Programmierhandbuch für App-Erweiterung](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html)
