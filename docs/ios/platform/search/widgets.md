---
title: Verbesserungen bei der Suche und dem Startbildschirm Widget in ios 10
description: In diesem Dokument werden die Verbesserungen beschrieben, die Apple an den Widgets in ios 10 vorgenommen hat, einschließlich Aktualisierungen der Such-und Startbildschirm Widgets.
ms.prod: xamarin
ms.assetid: D66FD9E1-9E23-4BB6-825C-ED19B8F72A81
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: ca6ccce934b32fa0d7e48cd8f295d9acefe6e121
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031515"
---
# <a name="search-and-home-screen-widget-enhancements-in-ios-10"></a>Verbesserungen bei der Suche und dem Startbildschirm Widget in ios 10

_In diesem Artikel werden die Verbesserungen erläutert, die Apple in ios 10 am Widget-System vorgenommen hat._

Apple hat mehrere Verbesserungen am Widget-System eingeführt, um sicherzustellen, dass die Widgets in jedem Hintergrund, der auf dem neuen IOS 10-Sperrbildschirm vorhanden ist, hervorragend aussehen. Außerdem enthalten Widgets nun eine [ncwidgetdisplaymode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) -Eigenschaft, mit der Entwickler beschreiben können, wie viel Inhalt verfügbar ist und der Benutzer den Inhalt erweitern und reduzieren kann.

Widgets (auch als "heute Extensions" bezeichnet) sind eine besondere Art von IOS-Erweiterungen, die eine kleine Menge nützlicher Informationen anzeigen oder die APP-spezifische Funktionalität rechtzeitig verfügbar machen. Beispielsweise verfügt die News-App über ein Widget, das die Top-Schlagzeilen anzeigt, und die Kalender-APP bietet zwei verschiedene Widgets: eine zum Anzeigen der aktuellen Ereignisse und eine zum Anzeigen der anstehenden Ereignisse.

Widgets sind hochgradig anpassbar und enthalten möglicherweise Elemente der Benutzeroberfläche, z. b. Text, Bilder, Schaltflächen usw. Außerdem kann der Entwickler das Layout seiner Widgets weiter anpassen.

[![](widgets-images/widgets01.png "Example widgets")](widgets-images/widgets01.png#lightbox)

Es gibt zwei Haupt Orte, an denen ein Benutzer die Widgets einer App anzeigen und mit ihnen interagieren kann:

- **Der Bildschirm für die Suche** : Benutzer können die Widgets hinzufügen, die für Ihren Suchbildschirm am nützlichsten sind. Der Zugriff auf den Suchbildschirm erfolgt durch Schwenken direkt auf dem Start-und dem Sperrbildschirm.
- **Der Startbildschirm** : der Benutzer kann auf dem Startbildschirm die 3D-Toucheingabe verwenden, um die Liste schneller Aktionen zu öffnen, indem er den Druck auf das Symbol der APP anwendet. Die Widgets einer App werden oberhalb der Liste schneller Aktionen angezeigt. Weitere Informationen finden Sie [in der Einführung in die 3D-](~/ios/platform/3d-touch.md) Dokumentation.

## <a name="widgets-developer-suggestions"></a>Widgets Entwickler Vorschläge

Im Idealfall sollte der Entwickler immer versuchen, Widgets zu entwerfen, die der Benutzer zu den Such Bildschirmen hinzufügen möchte. Zu diesem Zweck hat Apple die folgenden Vorschläge:

- **Erstellen Sie eine großartige,** benutzerfreundliche benutzerfreundliche Darstellung, die kurze, Zustands gesteuerte Informationen von Statusaktualisierungen bereitstellt, oder Sie können einfache Aufgaben schnell ausführen. Dadurch wird die richtige Menge an Informationen und Interaktivität als wesentlich bereitgestellt. Ermöglichen Sie es dem Benutzer nach Möglichkeit, eine bestimmte Aufgabe mit einem einzigen Tap auszuführen. Da Widgets das Schwenken oder Scrollen nicht unterstützen, muss dies im widgetentwurf berücksichtigt werden.
- **Schnelles Anzeigen von Inhalten** : Widgets sind so konzipiert, dass Sie ausgeblendet werden können, sodass der Benutzer niemals auf das Laden von Inhalten warten muss, sobald ein Widget angezeigt wird. Widgets sollten ihren Inhalt lokal zwischenspeichern, damit Sie aktuelle Inhalte anzeigen können, während im Hintergrund neue Inhalte geladen werden.
- Geben Sie entsprechende Auffüll- **und Rand Ränder** an. Widgets sollten nie überfüllt aussehen. vermeiden Sie daher die Erweiterung von Inhalten an die Ränder der Ansicht eines Widgets. Zwischen den Rändern und dem Inhalt sollte immer ein Pixel breiter Rand vorhanden sein. Apple schlägt außerdem vor, das App-Symbol, das am Anfang des Widgets angezeigt wird, als Ausrichtungs Handbuch zu verwenden. Wenn das Widget ein Raster Layout darstellt, stellen Sie sicher, dass zwischen den Elementen im Raster eine ordnungsgemäße Auffüll Zeichen besteht, und versuchen Sie, die Anzahl der Elemente auf vier Maximalwerte zu beschränken.
- **Anpassbare Layouts verwenden** : die Breite eines Widgets variiert je nach dem Gerät, auf dem es ausgeführt wird, und der Ausrichtung des Geräts. Die Höhe eines Widgets kann auch abhängig davon variieren, ob es in einem reduzierten (Standard-) oder erweiterten (nicht von allen Widgets unterstützten) Zustand angezeigt wird. Ein reduziertes Widget hat eine Höhe von ungefähr zwei und einer halben Standard-IOS-Tabellenzeile. Der Entwickler kann die Größe für ein erweitertes Widget anfordern, sollte jedoch idealerweise kleiner sein als die Höhe des Bildschirms. Im reduzierten Zustand sollte das Widget nur wichtige, eigenständige Informationen anzeigen. Wenn Sie erweitert ist, sollte das Widget zusätzliche Informationen anzeigen, mit denen der im reduzierten Zustand angezeigte primäre Inhalt erweitert wird. Widgets, die in der Liste schneller Aktionen angezeigt werden, sind nur im reduzierten Zustand.
- **Passen Sie die Hintergrund Widgets des Widgets nicht** an, wenn der Hintergrund vom System bereitgestellt wird. Dies geschieht, um die Konsistenz zwischen Widgets zu fördern und die Lesbarkeit Ihrer Inhalte zu verbessern. Vermeiden Sie die Verwendung eines Bilds als Widget-Hintergrund, da es mit dem Sperr-und Startbildschirm-Bildschirm des Benutzers kollidieren könnte.
- **Verwenden Sie die System Schriftart in schwarz oder dunkelgrau** : Wenn Sie Text in einem Widget anzeigen, funktioniert die System Schriftart am besten. Die Schriftart sollte sich in einer schwarz-oder dunkelgrauen Farbe befinden, damit Sie mit dem hellen, unscharfen widgehintergrund hervorgeht.
- **Bereitstellen von App-Zugriff, wenn das entsprechende** Widget immer separat von der app ausgeführt werden soll. Wenn jedoch eine tiefere Funktionalität erforderlich ist, sollte das Widget die app starten können, um bestimmte Informationen anzuzeigen oder zu bearbeiten. Fügen Sie niemals die Schaltfläche "APP öffnen" ein, lassen Sie den Benutzer einfach das Tippen auf den Inhalt selbst, und öffnen Sie niemals eine Drittanbieter-app.
- **Wählen Sie einen eindeutigen, präzisen widgenamen** aus. das Symbol der APP und der Name des Widgets werden immer über den Inhalt des Widgets angezeigt. Apple schlägt vor, den APP-Namen für das primäre Widget und einen eindeutigen, präzisen Namen für alle anderen Benutzer zu verwenden. Beim Bereitstellen eines benutzerdefinierten widgetitels sollte dem Namen der APP ein Präfix vorangestellt werden (z. b. Karten in der Nähe, Maps-Restaurants usw.).
- Informieren Sie sich darüber, wann der Wert von der **Authentifizierung hinzugefügt** wird, wenn zusätzliche Funktionen oder Informationen nur verfügbar sind, wenn der Benutzer authentifiziert und angemeldet ist. Beispielsweise kann eine Fahrt Freigabe-App beispielsweise "Anmelden bei buchfahrt" lauten.
- **Wählen Sie ein Widget für die schnell Aktionsliste** aus. wenn die APP mehr als ein Widget bereitstellt, sollte der Entwickler den Typ auswählen, der angezeigt werden soll, wenn der Benutzer die Liste schneller Aktionen aufführt, indem er mithilfe von 3D-Fingerabdruck auf das Symbol der APP zeigt.

Weitere Informationen zum Arbeiten mit Widgets finden Sie in unserer [Einführung in Extensions](~/ios/platform/extensions.md), [Einführung in die 3D-](~/ios/platform/3d-touch.md) Fingereingabe Dokumentation und in das [Programmier Handbuch zur APP-Erweiterung](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html)von Apple.

## <a name="working-with-vibrancy"></a>Arbeiten mit vibrancy

Vibrancy stellt sicher, dass der Text eines Widgets lesbar bleibt, wenn er im hell-und unscharfe Hintergrund des Widgets angezeigt wird (vom System bereitgestellt). Vor IOS 10 hat der Entwickler eine [notificationcentervibrancyeffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect) für die vibrancy des Widgets verwendet. Beispiel:

```csharp
// DEPRECATED: Get Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreateForNotificationCenter ();
```

Dies ist in ios 10 veraltet und sollte entweder durch eine [widgetprimaryvibrancyeffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect) oder eine [widgetsecondaryvibrancyeffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect)ersetzt werden. Beispiel:

```csharp
// Get Primary Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreatePrimaryVibrancyEffectForNotificationCenter ();

// Get Secondary Widget Vibrancy Effect
var vibrancy2 = UIVibrancyEffect.CreateSecondaryVibrancyEffectForNotificationCenter ();
```

## <a name="working-with-collapsed-and-expanded-widgets"></a>Arbeiten mit reduzierten und erweiterten Widgets

In ios 10 gibt es jetzt eine [ncwidgetdisplaymode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) -Eigenschaft, mit der Entwickler beschreiben können, wie viel Inhalt verfügbar ist und der Benutzer den Inhalt erweitern und reduzieren kann.

Wenn ein Widget anfänglich angezeigt wird, befindet es sich in einem reduzierten Zustand. Ein reduziertes Widget hat eine Höhe von ungefähr zwei und einer halben Standard-IOS-Tabellenzeile. Der Entwickler kann die Größe für ein erweitertes Widget anfordern, sollte jedoch idealerweise kleiner sein als die Höhe des Bildschirms.

Im reduzierten Zustand sollte das Widget nur wichtige, eigenständige Informationen anzeigen. Wenn Sie erweitert ist, sollte das Widget zusätzliche Informationen anzeigen, mit denen der im reduzierten Zustand angezeigte primäre Inhalt erweitert wird. Die Wetter-APP zeigt z. b. die aktuellen Wetterbedingungen an, wenn Sie reduziert ist, und fügt die stündliche Vorhersage hinzu

Widgets, die in der Liste schneller Aktionen angezeigt werden, sind nur im reduzierten Zustand. Wenn die APP mehr als ein Widget bereitstellt, sollte der Entwickler den Typ auswählen, der angezeigt werden soll, wenn der Benutzer die Liste schneller Aktionen aufführt, indem er mithilfe von 3D-Fingereingabe den Druck auf das Symbol der APP anwendet.

Das folgende Beispiel ist eine einfache heutige Erweiterung (Widget), die den reduzierten und erweiterten Status behandelt:

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

Sehen Sie sich den Code im Widget-Anzeigemodus im Detail an. Um dem System mitzuteilen, dass dieses Widget den erweiterten Status unterstützt, wird Folgendes verwendet:

```csharp
// Tell widget it can be expanded
ExtensionContext.SetWidgetLargestAvailableDisplayMode (NCWidgetDisplayMode.Expanded);
```

Um den aktuellen Anzeigemodus des Widgets zu erhalten, wird Folgendes verwendet:

```csharp
ExtensionContext.GetWidgetActiveDisplayMode()
```

Um die maximale Größe für den reduzierten oder erweiterten Zustand zu erhalten, wird Folgendes verwendet:

```csharp
// Get the maximum size
var maxSize = ExtensionContext.GetWidgetMaximumSize (NCWidgetDisplayMode.Expanded);
```

Und um den Status (Anzeigemodus) zu ändern, wird Folgendes verwendet:

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

Zusätzlich zum Festlegen der angeforderten Größe für jeden Zustand (reduziert oder erweitert) wird auch der angezeigte Inhalt aktualisiert, damit er der neuen Größe entspricht.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Verbesserungen erläutert, die Apple in ios 10 am Widget-System vorgenommen hat, und es wurde gezeigt, wie Sie in xamarin. IOS implementiert werden.

## <a name="related-links"></a>Verwandte Links

- [IOS 10-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
- [Einführung in Erweiterungen](~/ios/platform/extensions.md)
- [Einführung in 3D-Fingereingabe](~/ios/platform/3d-touch.md)
- [Programmier Handbuch für die APP-Erweiterung](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html)
