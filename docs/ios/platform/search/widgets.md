---
title: Suchen und Startseite Bildschirm Widget-Verbesserungen in iOS 10
description: Dieses Dokument beschreibt die Erweiterungen, die Apple an die Widgets unter iOS 10, einschließlich Updates für Such- und Startbildschirm-Widgets vorgenommen hat.
ms.prod: xamarin
ms.assetid: D66FD9E1-9E23-4BB6-825C-ED19B8F72A81
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: f693b480fff141c177ed135ced60afd65abd77de
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50102788"
---
# <a name="search-and-home-screen-widget-enhancements-in-ios-10"></a>Suchen und Startseite Bildschirm Widget-Verbesserungen in iOS 10

_Dieser Artikel beschreibt die Erweiterungen, die vom Widget-System unter iOS 10 Apple angefordert hat._

Apple wurden mehrere Erweiterungen für das Widget-System, um sicherzustellen, dass die Widgets auf eine im Hintergrund laufende suchen, die auf dem neuen iOS 10 Sperrbildschirm vorhanden ist. Darüber hinaus enthalten Widgets jetzt eine [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) -Eigenschaft, kann der Entwickler beschrieben, wie viel Inhalt verfügbar ist, und ermöglicht dem Benutzer zum Erweitern und reduzieren den Inhalt.

Widgets (auch bekannt als heute Extensions) sind eine besondere Art von iOS Erweiterung, die eine kleine Menge nützlicher Informationen oder app-spezifische Funktionalität rechtzeitig verfügbar zu machen. Z. B. die News-app verfügt über ein Widget, das die oberen Schlagzeilen zeigt und der Kalender-app bietet zwei verschiedene Widgets: eine zum Anzeigen der heutigen Ereignisse und einer anstehenden Ereignissen angezeigt.

Widgets sind umfassend anpassbar und können die UI-Elemente, z. B. Text, Bilder, Schaltflächen usw. enthalten. Darüber hinaus kann Entwickler weiter das Layout von ihren Widgets anpassen.

[![](widgets-images/widgets01.png "Beispiel-widgets")](widgets-images/widgets01.png#lightbox)

Es gibt zwei wichtigsten stellen, die ein Benutzer anzeigen und interagieren mit der app-Widgets kann:

- **Der Bildschirm zum Suchen von** -Benutzer können die finden sie besonders nützlich für ihren Bildschirm zum Suchen von Widgets hinzufügen. Der Bildschirm zum Suchen von erfolgt durch Wischen nach rechts auf der Startseite und die Sperre Bildschirmen.
- **Der Startbildschirm** -aus der Startbildschirm, die Benutzer kann Verwenden von 3D Touch Liste der Schnellaktionen durch Anwenden von Druck auf das Symbol der app zu öffnen. Der app-Widgets werden über die schnelle Aktion-Liste angezeigt. Informieren Sie sich unsere [Einführung in 3D Touch](~/ios/platform/3d-touch.md) Dokumentation zu informieren.

## <a name="widgets-developer-suggestions"></a>Vorschläge für Widgets-Entwickler

Der Entwickler sollte im Idealfall immer versuchen und Entwerfen von Widgets, die der Benutzer ihren Search-Bildschirmen hinzufügen möchten. Aus diesem Grund hat Apple die folgenden Vorschläge:

- **Erstellen Sie eine gute, Glanceable Erfahrung** -Benutzers soll Widgets, die zur Verfügung kurze glanceable von statusaktualisierungen oder können sie einfache Aufgaben schnell ausführen. Dadurch wird die richtige Menge an Informationen und Interaktivität ein unverzichtbares bereitstellen. Wann immer möglich, können Sie Benutzer zum Ausführen von einer bestimmten Aufgabe mit einem einzigen fingertipp. Da keine Widgets schwenken oder Bildlauf unterstützen, müssen dies darüber hinaus in die Widget Geräts Entwurf berücksichtigt werden.
- **Inhalte schnell anzeigen** -Widgets dienen als glanceable, damit der Benutzer warten auf Inhalt geladen wird, sobald ein Widget angezeigt wird, nie erhalten soll. Widgets sollte ihren Inhalt zwischenspeichern, lokal, sodass sie aktuelle Inhalte anzeigen können, während neue Inhalte im Hintergrund geladen wird.
- **Geben Sie entsprechende Auffüllung und Rändern** -Widgets sollte nie suchen Rechenzentrum weiter aufzurüsten, daher nicht Erweitern von Inhalt an den Rändern des Widget Geräts anzeigen. Es sollte immer einen mehrere große Pixel-Rand zwischen den Rändern und der Inhalt vorhanden sein. Apple empfiehlt außerdem verwenden das Symbol der app, im oberen Bereich des Widgets, wie eine Führungslinie angezeigt. Wenn das Widget ein Rasterlayout vorweisen, sicherzustellen Sie, dass ordnungsgemäße Abstand zwischen den Elementen im Raster, und wiederholen Sie die Anzahl der Elemente, die vier Max zu beschränken.
- **Anpassungsfähige Layouts verwenden** -ein Widget Geräts Breite variieren basierend auf dem Gerät auf und die Ausrichtung des Geräts. Ein Widget Geräts Höhe kann auch basierend auf variieren, wenn es in einem Collapsed (Standard) oder die erweiterte (nicht durch alle Widgets unterstützt)-Status angezeigt wird. Ein Widget reduziert hat es sich um eine Höhe von etwa 2,5 standard iOS Tabellenzeilen. Der Entwickler kann die Größe für ein Widget erweitert anfordern, jedoch sollte im Idealfall kleiner als die Höhe des Bildschirms sein. Das Widget sollte in den Zustand "Collapsed" nur wichtige, eigenständige Informationen angezeigt werden. Wenn erweitert, das Widget zusätzliche Informationen werden, die den Hauptinhalt gezeigt im Zustand "Collapsed" verbessert angezeigt sollen. Widgets, die in der Liste der schnellen Aktion angezeigt werden nur im Zustand "Collapsed".
- **Nicht Anpassen des Widget-Hintergrund** -Widgets auf einen hellen, weichgezeichnete Hintergrund, die vom System bereitgestellten angezeigt werden. Dies erfolgt zum fördern der Konsistenz zwischen Widgets und verbessern die Lesbarkeit ihrer Inhalte. Verwenden Sie ein Bild als Hintergrund Widget, da es mit des Benutzers sperren und Startbildschirm Hintergrundbilder miteinander in Konflikt geraten kann.
- **Verwenden Sie die Systemschriftart in Schwarz oder dunkelgrau** : Anzeige von Text in einem Widget, die Systemschriftart funktioniert am besten. Die Schriftart sollte in eine Schwarz oder dunkel graue Farbe, die vor dem Hintergrund für kleine und weichgezeichnete Widget abhebt.
- **Bereitstellen der App den Zugriff bei der entsprechenden** -Widget Geräts sollten immer separat von ihrer app arbeiten, die jedoch wenn tiefere Funktionalität erforderlich ist, das Widget sollte in der Lage, starten Sie die app zum Anzeigen oder bearbeiten eine bestimmte Information. Fügen Sie eine Schaltfläche "Öffnen der app" nie einfach durch den Benutzer der Inhalt selbst Tippen Sie auf zulassen und nie open a 3rd party app.
- **Wählen Sie ein eindeutiges, präzise Namen des Widgets** : das Symbol der app und des Widget-Namen werden immer über das Widget die Inhalte angezeigt. Apple empfiehlt, mit der App-Namen für die primäre Gadget und eine klare und kompakte Namen für alle anderen bereitgestellten. Wenn Sie einen benutzerdefinierten Widgettitel bereitstellen zu können, sollten sie den Namen der app (z. B. Karten in Ihrer Nähe, Zuordnungen, Restaurants usw.) vorangestellt ist.
- **Zu informieren, wenn die Authentifizierung Mehrwert** : Wenn zusätzliche Funktionalität oder Informationen steht nur, wenn der Benutzer authentifiziert wird und angemeldet sind, dies dem Benutzer angezeigt. Eine Tour Freigabe-app kann z. B. "Melden Sie sich beim Buch Fahrt".
- **Wählen Sie eine schnelle Aktion Listenwidget** – Wenn die app mehr als ein Widget bereitstellt, der Entwickler muss wählen Sie das um anzuzeigen, wenn der Benutzer die Liste der schnellen Aktionen durch Anwenden von Druck auf das Symbol der app mithilfe von 3D Touch öffnet.

Weitere Informationen zum Arbeiten mit Widgets, informieren Sie sich unsere [Einführung in die Erweiterungen](~/ios/platform/extensions.md), [Einführung in 3D Touch](~/ios/platform/3d-touch.md) Dokumentation und Apple [App-Erweiterung Programming Guide](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html).

## <a name="working-with-vibrancy"></a>Arbeiten mit Dynamik

Dynamik wird sichergestellt, dass ein Widget Geräts Text lesbar, die Darstellung auf das Widget die Light weichgezeichnete Hintergrund (bereitgestellt vom System) bleibt. Bevor Sie iOS 10, verwendet der Entwickler eine [NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect) für das Widget die Dynamik. Zum Beispiel:

```csharp
// DEPRECATED: Get Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreateForNotificationCenter ();
```

Dies wurde als veraltet markiert werden iOS 10 und ersetzt werden soll, entweder mit einem [WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect) oder [WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect). Zum Beispiel:

```csharp
// Get Primary Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreatePrimaryVibrancyEffectForNotificationCenter ();

// Get Secondary Widget Vibrancy Effect
var vibrancy2 = UIVibrancyEffect.CreateSecondaryVibrancyEffectForNotificationCenter ();
```

## <a name="working-with-collapsed-and-expanded-widgets"></a>Arbeiten mit zu- bzw. aufgeklappt Widgets

Neue IOS 10, Widgets enthalten nun die eine [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) -Eigenschaft, kann der Entwickler beschrieben, wie viel Inhalt verfügbar ist, und ermöglicht dem Benutzer zum Erweitern und reduzieren den Inhalt.

Wenn ein Widget anfänglich angezeigt wird, ist es in einem Collapsed-Zustand. Ein Widget reduziert hat es sich um eine Höhe von etwa 2,5 standard iOS Tabellenzeilen. Der Entwickler kann die Größe für ein Widget erweitert anfordern, jedoch sollte im Idealfall kleiner als die Höhe des Bildschirms sein. 

Das Widget sollte in den Zustand "Collapsed" nur wichtige, eigenständige Informationen angezeigt werden. Wenn erweitert, das Widget zusätzliche Informationen werden, die den Hauptinhalt gezeigt im Zustand "Collapsed" verbessert angezeigt sollen. Z. B. die Wetter-app zeigt die aktuellen Wetterbedingungen Wenn reduziert, und fügt die stündlichen prognostizieren, wenn die Kategorie erweitert.

Widgets, die in der Liste der schnellen Aktion angezeigt werden nur im Zustand "Collapsed". Wenn die app mehr als ein Widget bereitstellt, sollten Entwickler auswählen, die ein, um anzuzeigen, wenn der Benutzer die Liste der schnellen Aktionen durch Anwenden von Druck auf das Symbol der app mithilfe von 3D Touch öffnet.

Im folgende Beispiel wird einer einfachen heute-Erweiterung (Widget), der die Zustände reduziert und erweitert behandelt:

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

Sehen Sie sich der bestimmte Anzeigemodus Widget-Code im Detail an. Um dem System zu informieren, dass dieses Widget den erweiterte-Zustand unterstützt, wird wie folgt vor:

```csharp
// Tell widget it can be expanded
ExtensionContext.SetWidgetLargestAvailableDisplayMode (NCWidgetDisplayMode.Expanded);
```

Rufen Sie das Widget die aktuellen Anzeigemodus befindet, wird wie folgt vor:

```csharp
ExtensionContext.GetWidgetActiveDisplayMode()
```

Rufen Sie die maximale Größe für die reduzierte oder erweitert Zustand, wird wie folgt vor:

```csharp
// Get the maximum size
var maxSize = ExtensionContext.GetWidgetMaximumSize (NCWidgetDisplayMode.Expanded);
```

Und um die statusänderung (Anzeigemodus) zu behandeln, verwendet:

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

Zusätzlich zum Festlegen der angeforderten Größe für jeden Zustand (reduziert oder erweitert), aktualisiert sie auch den Inhalt, der angezeigt wird, um die neue Größe zu entsprechen.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde behandelt die Erweiterungen, die Apple an das System Widgets unter iOS 10 vorgenommen hat und gezeigt, wie sie in Xamarin.iOS zu implementieren.



## <a name="related-links"></a>Verwandte Links

- [iOS 10-Beispiele](https://developer.xamarin.com/samples/ios/iOS10/)
- [Einführung in Erweiterungen](~/ios/platform/extensions.md)
- [Einführung in 3D Touch](~/ios/platform/3d-touch.md)
- [Programmierhandbuch für App-Erweiterung](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html)
