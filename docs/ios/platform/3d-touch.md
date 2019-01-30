---
title: Einführung in 3D Touch in Xamarin.iOS
description: Dieser Artikel beschreibt, wie 3D Touch-Gesten eingeführt, mit dem iPhone 6 s und iPhone 6 s Plus. Für diese Gesten aktivieren, Druck Vertraulichkeit "," Peek "und" Pop ", und schnelle Aktionen.
ms.prod: xamarin
ms.assetid: 806D051E-3791-40F7-9776-4E4D3E56F7F3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 60751437b891579c97acee0e032defcca2b510f6
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233860"
---
# <a name="introduction-to-3d-touch-in-xamarinios"></a>Einführung in 3D Touch in Xamarin.iOS

_Dieser Artikel befasst sich mit den neuen iPhone 6 s und iPhone 6 s Plus 3D Touch anwendungsstiftbewegungen in Ihrer app._

[![](3d-touch-images/info01.png "Beispiele für 3D Touch-fähige apps")](3d-touch-images/info01.png#lightbox)

In diesem Artikel bieten und Einführung in die Verwendung der neuen 3D Touch-APIs zum Druck vertrauliche Gesten Ihrer Xamarin.iOS-apps hinzufügen, die auf dem neuen iPhone 6s und iPhone 6 s ausgeführt werden sowie Geräte.

Mit 3D Touch ist eine iPhone-app jetzt nicht nur mitteilen, dass der Benutzer den Bildschirm des Geräts berührt, aber es ist wie viel Druck erkennen, die Benutzer selbst ist, und auf die verschiedenen arbeitsspeicherauslastung reagieren können.

3D Touch bietet die folgenden Funktionen zu Ihrer app:

- [Vertraulichkeit Druck](#Pressure-Sensitivity) : Apps können jetzt wie schwer messen oder Licht der Benutzer berührt den Bildschirm "und" Take-Vorteil mit diesen Informationen.
  Eine app zeichnen kann z. B. eine Linie dicker oder flacher abhängig, wie sehr die Benutzer den Bildschirm berührt vornehmen.
- [Einsehen und Pop](#Peek-and-Pop) -Ihrer-app können nun den Benutzer, die mit den Daten interagieren, ohne aus ihrem aktuellen Kontext Navigieren zu müssen. Durch Drücken auf dem Bildschirm den Bildschirm schwierig, können sie das Element einsehen, die, denen Sie interessieren (z. B. das Anzeigen eine Nachricht in der Vorschau) sind. Durch Drücken von schwieriger, können sie in das Element angezeigt werden.
- [Schnelle Aktionen](#Quick-Actions) -vorstellen von Schnellaktionen wie die Kontextmenüs, die per pop ausgelesen oben können sein, wenn ein Benutzer auf ein Element in einer desktop-app mit der rechten Maustaste.
  Verwenden von Schnellaktionen, können Sie Tastenkombinationen für Funktionen in Ihrer app direkt aus dem app-Symbol auf der Startseite auf Hinzufügen.
- [Testen von 3D Touch im Simulator](#Testing-3D-Touch-in-the-Simulator) – Sie können 3D Touch aktiviert apps im iOS-Simulator testen, mit der richtigen Mac-Hardware.

<a name="Pressure-Sensitivity" />

## <a name="pressure-sensitivity"></a>Druckempfindlichkeit

Wie oben angegeben, mit der neuen Eigenschaften der [UITouch](xref:UIKit.UITouch) -Klasse können Sie messen Druck, die der Benutzer wird auf den iOS-Bildschirm des Geräts angewendet und verwenden Sie diese Informationen in der Benutzeroberfläche. Beispiel dafür wäre, einen Pinselstrich mehr transparent oder undurchsichtig basierend auf der Menge der Druck.

[![](3d-touch-images/pressure01.png "Ein Pinselstrich gerendert wird, mehr als transparent oder undurchsichtig basierend auf der Menge der Druck")](3d-touch-images/pressure01.png#lightbox)

Als Ergebnis von 3D Touch, wenn Ihre app unter iOS 9 (oder höher) ausgeführt wird und das iOS-Gerät kann unterstützenden 3D Touch Änderungen im Druck führt dazu, dass die `TouchesMoved` Ereignis ausgelöst wurde.

Beispielsweise wird bei der Überwachung der `TouchesMoved` Ereignis eine [UIView](xref:UIKit.UIView), Sie können den folgenden Code verwenden, um die aktuelle Auslastung zu erhalten, die der Benutzer auf dem Bildschirm angewendet wird:

```csharp
public override void TouchesMoved (NSSet touches, UIEvent evt)
{
    base.TouchesMoved (touches, evt);
    UITouch touch = touches.AnyObject as UITouch;
    if (touch != null)
    {
        // Get the pressure
        var force = touch.Force;
        var maxForce = touch.MaximumPossibleForce;

        // Do something with the touch and the pressure
        ...
    }
}
```

Die `MaximumPossibleForce` Eigenschaft gibt die höchsten möglichen Wert für die `Force` Eigenschaft der [UITouch](xref:UIKit.UITouch) basierend auf dem iOS-Gerät, das die app ausgeführt wird.

> [!IMPORTANT]
> Bewirkt, dass Änderungen in den Druck der `TouchesMoved` Ereignis ausgelöst wird, auch wenn das "X" / Y-Koordinaten nicht geändert wurden. Aufgrund dieser Änderung im Verhalten Ihrer iOS-apps vorbereitet sein sollte die `TouchesMoved` aufzurufenden Ereignisses häufiger und für die X- / Y-Koordinaten auf der letzten identisch sein `TouchesMoved` aufrufen.




Weitere Informationen finden Sie unter Apple [TouchCanvas: Mithilfe von UITouch effizient und effektiv](https://developer.apple.com/library/prerelease/ios/samplecode/TouchCanvas/) Beispiel-app und [UITouch Class Reference](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITouch_Class/).

<a name="Peek-and-Pop" />

## <a name="peek-and-pop"></a>Peek "und" Pop

3D Touch bietet neue Möglichkeiten für Benutzer mit den Informationen in Ihrer app, die schneller als je zuvor zu interagieren, ohne dass von ihrem aktuellen Speicherort zu navigieren.

Wenn Ihre app eine Tabelle mit Nachrichten angezeigt wird, der Benutzer kann drücken Sie z. B. auf ein Element, dessen Inhalt in einer Überlagerungsansicht Vorschau schwierig (bezieht sich Apple auf als eine *einsehen*).

[![](3d-touch-images/peekandpop01.png "Ein Beispiel für Vorschau Inhalt")](3d-touch-images/peekandpop01.png#lightbox)

Wenn der Benutzer schwieriger klickt, werden sie der regulären Nachrichtenansicht eingeben (Dies wird als bezeichnet *Pop*-Ping in der Ansicht).

### <a name="checking-for-3d-touch-availability"></a>Überprüfen für die Verfügbarkeit von 3D Touch

Bei der Arbeit mit einem [UIViewController]() können Sie den folgenden Code verwenden, um festzustellen, ob die app ausgeführt wird, auf das iOS-Gerät 3D Touch unterstützt:

```csharp
public override void TraitCollectionDidChange(UITraitCollection previousTraitCollection)
{
    //Important: call the base function
    base.TraitCollectionDidChange(previousTraitCollection);

    //See if the new TraitCollection value includes force touch
    if (TraitCollection.ForceTouchCapability == UIForceTouchCapability.Available) {
        //Do something with 3D touch, for instance...
        RegisterForPreviewingWithDelegate (this, View);
        ...
```

Diese Methode kann aufgerufen werden, bevor *oder nach* `ViewDidLoad()`. 

### <a name="handling-peek-and-pop"></a>Behandlung von Peek "und" Pop

Auf einem iOS-Gerät, die 3D Touch verarbeiten kann, können wir eine Instanz von der `UIViewControllerPreviewingDelegate` -Klasse, behandeln die Anzeige von **einsehen** und **Pop** Elementdetails. Angenommen, wir hatten ein Tabellenansichtscontroller aufgerufen `MasterViewController ` können wir den folgenden Code zur Unterstützung **einsehen** und **Pop**:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;

namespace DTouch
{
    public class PreviewingDelegate : UIViewControllerPreviewingDelegate
    {
        #region Computed Properties
        public MasterViewController MasterController { get; set; }
        #endregion

        #region Constructors
        public PreviewingDelegate (MasterViewController masterController)
        {
            // Initialize
            this.MasterController = masterController;
        }

        public PreviewingDelegate (NSObjectFlag t) : base(t)
        {
        }

        public PreviewingDelegate (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        /// Present the view controller for the "Pop" action.
        public override void CommitViewController (IUIViewControllerPreviewing previewingContext, UIViewController viewControllerToCommit)
        {
            // Reuse Peek view controller for details presentation
            MasterController.ShowViewController(viewControllerToCommit,this);
        }

        /// Create a previewing view controller to be shown at "Peek".
        public override UIViewController GetViewControllerForPreview (IUIViewControllerPreviewing previewingContext, CGPoint location)
        {
            // Grab the item to preview
            var indexPath = MasterController.TableView.IndexPathForRowAtPoint (location);
            var cell = MasterController.TableView.CellAt (indexPath);
            var item = MasterController.dataSource.Objects [indexPath.Row];

            // Grab a controller and set it to the default sizes
            var detailViewController = MasterController.Storyboard.InstantiateViewController ("DetailViewController") as DetailViewController;
            detailViewController.PreferredContentSize = new CGSize (0, 0);

            // Set the data for the display
            detailViewController.SetDetailItem (item);
            detailViewController.NavigationItem.LeftBarButtonItem = MasterController.SplitViewController.DisplayModeButtonItem;
            detailViewController.NavigationItem.LeftItemsSupplementBackButton = true;

            // Set the source rect to the cell frame, so everything else is blurred.
            previewingContext.SourceRect = cell.Frame;

            return detailViewController;
        }
        #endregion
    }
}
```

Die `GetViewControllerForPreview` Methode dient zum Ausführen der **einsehen** Vorgang. Sie erhalten Zugriff auf die Zelle und Sichern von Tabellendaten und lädt dann die `DetailViewController` über das aktuelle Storyboard. Durch Festlegen der `PreferredContentSize` um (0,0) werden Fragen wir für den standardmäßigen **einsehen** sichtgröße. Schließlich wir alle Elemente außer der Zelle, die wir Anzeigen von mit unkenntlich `previewingContext.SourceRect = cell.Frame` und ist die neue Ansicht für die Anzeige zurückgegeben.

Die `CommitViewController` verwendet wieder die Ansicht, die wir erstellt, in haben der **einsehen** für die **Pop** anzeigen, wenn der Benutzer schwieriger drückt.

### <a name="registering-for-peek-and-pop"></a>Registrieren für die Peek "und" Pop

Über den Ansicht-Controller, die wir dem Benutzer ermöglichen möchten **einsehen** und **Pop** Elemente aus, wir müssen für diesen Dienst zu registrieren. Im Beispiel oben angegebenen, der ein Tabellenansichtscontroller (`MasterViewController`), verwenden wir den folgenden Code:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Check to see if 3D Touch is available
    if (TraitCollection.ForceTouchCapability == UIForceTouchCapability.Available) {
        // Register for Peek and Pop
        RegisterForPreviewingWithDelegate(new PreviewingDelegate(this), View);
    }
    ...

}
```

Hier der Name der `RegisterForPreviewingWithDelegate` Methode mit einer Instanz von der `PreviewingDelegate` wir oben erstellt haben. Auf iOS-Geräten, die 3D Touch unterstützen, kann der Benutzer auf ein Element einzusehen es schwer drücken. Wenn sie noch schwieriger drücken, wird das Element angezeigt, in diesen standard anzuzeigen.

Weitere Informationen finden Sie unserem [iOS 9 ApplicationShortcuts Beispiel](https://developer.xamarin.com/samples/monotouch/iOS9/ViewControllerPreview/) und Apple [ViewControllerPreviews: Mithilfe der Vorschau-APIs UIViewController](https://developer.apple.com/library/prerelease/ios/samplecode/ViewControllerPreviews/Introduction/Intro.html) Beispiel-app [UIPreviewAction Class Reference](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewAction_Class/), [UIPreviewActionGroup Class Reference](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionGroup_Class/) und [UIPreviewActionItem Referenz-Protokoll](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionItem_Protocol/).

<a name="Quick-Actions" />

## <a name="quick-actions"></a>Schnelle Aktionen

3D Touch und Schnellaktionen verwenden, können Sie allgemeine, schnell und einfach zu Verknüpfungen der Zugriff auf Funktionen in Ihrer app über das Symbol des Home-Bildschirm auf dem iOS-Gerät hinzufügen.

Wie bereits erwähnt, können Sie von Schnellaktionen wie die Kontextmenüs vorstellen, die per pop ausgelesen oben kann sein, wenn ein Benutzer auf ein Element in einer desktop-app mit der rechten Maustaste. Sie sollten die Schnellaktionen verwenden, um Verknüpfungen zu den am häufigsten verwendeten Funktionen oder Features Ihrer app bereitzustellen.

[![](3d-touch-images/quickactions01.png "Ein Beispiel für ein Menü für Schnellaktionen")](3d-touch-images/quickactions01.png#lightbox)


### <a name="defining-static-quick-actions"></a>Definieren von statischen schnelle Aktionen

Wenn eine oder mehrere der am Schnellaktionen, die von Ihrer app erforderlich statisch sind und nicht ändern müssen, können Sie sie in der app definieren `Info.plist` Datei. Bearbeiten Sie diese Datei in einem externen Editor, und fügen Sie die folgenden Schlüssel hinzu:

```xml
<key>UIApplicationShortcutItems</key>
<array>
    <dict>
        <key>UIApplicationShortcutItemIconType</key>
        <string>UIApplicationShortcutIconTypeSearch</string>
        <key>UIApplicationShortcutItemSubtitle</key>
        <string>Will search for an item</string>
        <key>UIApplicationShortcutItemTitle</key>
        <string>Search</string>
        <key>UIApplicationShortcutItemType</key>
        <string>com.company.appname.000</string>
    </dict>
    <dict>
        <key>UIApplicationShortcutItemIconType</key>
        <string>UIApplicationShortcutIconTypeShare</string>
        <key>UIApplicationShortcutItemSubtitle</key>
        <string>Will share an item</string>
        <key>UIApplicationShortcutItemTitle</key>
        <string>Share</string>
        <key>UIApplicationShortcutItemType</key>
        <string>com.company.appname.001</string>
    </dict>
</array>
```

Hier definieren wir zwei statische Schnellaktions-Elemente mit den folgenden Schlüsseln:

- `UIApplicationShortcutItemIconType` : Definiert das Symbol, das als eine der folgenden Werte des Elements für die schnelle Aktion angezeigt wird:
  - `UIApplicationShortcutIconTypeAdd`
  - `UIApplicationShortcutIconTypeAlarm`
  - `UIApplicationShortcutIconTypeAudio`
  - `UIApplicationShortcutIconTypeBookmark`
  - `UIApplicationShortcutIconTypeCapturePhoto`
  - `UIApplicationShortcutIconTypeCaptureVideo`
  - `UIApplicationShortcutIconTypeCloud`
  - `UIApplicationShortcutIconTypeCompose`
  - `UIApplicationShortcutIconTypeConfirmation`
  - `UIApplicationShortcutIconTypeContact`
  - `UIApplicationShortcutIconTypeDate`
  - `UIApplicationShortcutIconTypeFavorite`
  - `UIApplicationShortcutIconTypeHome`
  - `UIApplicationShortcutIconTypeInvitation`
  - `UIApplicationShortcutIconTypeLocation`
  - `UIApplicationShortcutIconTypeLove`
  - `UIApplicationShortcutIconTypeMail`
  - `UIApplicationShortcutIconTypeMarkLocation`
  - `UIApplicationShortcutIconTypeMessage`
  - `UIApplicationShortcutIconTypePause`
  - `UIApplicationShortcutIconTypePlay`
  - `UIApplicationShortcutIconTypeProhibit`
  - `UIApplicationShortcutIconTypeSearch`
  - `UIApplicationShortcutIconTypeShare`
  - `UIApplicationShortcutIconTypeShuffle`
  - `UIApplicationShortcutIconTypeTask`
  - `UIApplicationShortcutIconTypeTaskCompleted`
  - `UIApplicationShortcutIconTypeTime`
  - `UIApplicationShortcutIconTypeUpdate`

        ![](3d-touch-images/uiapplicationshortcuticontype.png "UIApplicationShortcutIconType imagery")

* `UIApplicationShortcutItemSubtitle` -Den Untertitel für das Element definiert.
* `UIApplicationShortcutItemTitle` : Definiert den Titel für das Element.
* `UIApplicationShortcutItemType` -Ist ein Zeichenfolgenwert, den wir zum Identifizieren des Elements in unserer app verwenden. Weitere Informationen finden Sie in folgendem Abschnitt.

> [!IMPORTANT]
> Schnelle Aktion Kontextmenü Elemente, die auf die `Info.plist` Datei kann nicht zugegriffen werden, mit der `Application.ShortcutItems` Eigenschaft. Sie werden nur übergeben, um die `HandleShortcutItem` -Ereignishandler. 





### <a name="identifying-quick-action-items"></a>Identifizieren schnelle Aktionen

Wie oben gezeigt, wenn Sie definiert Ihre Elemente für die schnelle Aktion in Ihrer app `Info.plist`, Sie zugewiesen haben einen Zeichenfolgenwert, der `UIApplicationShortcutItemType` -Taste, um sie zu identifizieren.

Um diesen Bezeichner im Code verwenden vereinfachen, fügen Sie eine Klasse namens `ShortcutIdentifier` zu Ihrer app Projekt, und stellen sie wie folgt aussehen:

```csharp
using System;

namespace AppSearch
{
    public static class ShortcutIdentifier
    {
        public const string First = "com.company.appname.000";
        public const string Second = "com.company.appname.001";
        public const string Third = "com.company.appname.002";
        public const string Fourth = "com.company.appname.003";
    }
}
```

<a name="Handling-a-Quick-Action" />

### <a name="handling-a-quick-action"></a>Eine schnelle Aktion verarbeiten

Als Nächstes müssen Sie Ihrer app ändern `AppDelegate.cs` Datei für die Behandlung des Benutzers Ihrer app-Symbol auf der Startseite auf ein Element der schnellen Aktion auswählen.

Stellen Sie die folgenden Änderungen vor:

```csharp
using System;
...

public UIApplicationShortcutItem LaunchedShortcutItem { get; set; }

public bool HandleShortcutItem(UIApplicationShortcutItem shortcutItem) {
    var handled = false;

    // Anything to process?
    if (shortcutItem == null) return false;

    // Take action based on the shortcut type
    switch (shortcutItem.Type) {
    case ShortcutIdentifier.First:
        Console.WriteLine ("First shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Second:
        Console.WriteLine ("Second shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Third:
        Console.WriteLine ("Third shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Fourth:
        Console.WriteLine ("Forth shortcut selected");
        handled = true;
        break;
    }

    // Return results
    return handled;
}

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    var shouldPerformAdditionalDelegateHandling = true;

    // Get possible shortcut item
    if (launchOptions != null) {
        LaunchedShortcutItem = launchOptions [UIApplication.LaunchOptionsShortcutItemKey] as UIApplicationShortcutItem;
        shouldPerformAdditionalDelegateHandling = (LaunchedShortcutItem == null);
    }

    return shouldPerformAdditionalDelegateHandling;
}

public override void OnActivated (UIApplication application)
{
    // Handle any shortcut item being selected
    HandleShortcutItem(LaunchedShortcutItem);

    // Clear shortcut after it's been handled
    LaunchedShortcutItem = null;
}

public override void PerformActionForShortcutItem (UIApplication application, UIApplicationShortcutItem shortcutItem, UIOperationHandler completionHandler)
{
    // Perform action
    completionHandler(HandleShortcutItem(shortcutItem));
}
```

Zuerst definieren wir eine öffentliche `LaunchedShortcutItem` Eigenschaft, um das letzte Element der ausgewählte schnelle Aktion durch den Benutzer zu verfolgen. Überschreiben Sie dann die `FinishedLaunching` -Methode, und sehen Sie sich Wenn `launchOptions` übergeben wurden und wenn sie ein Element der schnellen Aktion enthält. Wenn dies der Fall ist, speichern wir die schnelle Aktion in der `LaunchedShortcutItem` Eigenschaft.

Als Nächstes überschreiben wir die `OnActivated` Methode und übergeben Sie einen Schnellstart-Element, das ausgewählt der `HandleShortcutItem` Methode, um die reagiert werden. Derzeit werden nur schreiben wir eine Nachricht an die **Konsole**. In einer echten app würden Sie verarbeiten, was ever Aktion erforderlich war. Nachdem die Aktion durchgeführt wurde, die `LaunchedShortcutItem` Eigenschaft deaktiviert ist.

Schließlich, wenn Ihre app bereits ausgeführt wurde, die `PerformActionForShortcutItem` Methode würde aufgerufen werden, um das Element der schnellen Aktion zu verarbeiten, daher müssen wir überschreiben, und rufen Sie unsere `HandleShortcutItem` Methode auch hier.


### <a name="creating-dynamic-quick-action-items"></a>Erstellen von dynamischen Schnellaktions-Elementen

Zusätzlich zum Definieren von statischen schnelle Aktion Elemente in Ihrer app `Info.plist` -Datei können Sie dynamische auf dynamische Schnellaktionen erstellen. Um zwei neue dynamische Schnellaktionen zu definieren, bearbeiten Ihre `AppDelegate.cs` -Datei erneut, und Ändern der `FinishedLaunching` Methode, um das Aussehen wie folgt aus:

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    var shouldPerformAdditionalDelegateHandling = true;

    // Get possible shortcut item
    if (launchOptions != null) {
        LaunchedShortcutItem = launchOptions [UIApplication.LaunchOptionsShortcutItemKey] as UIApplicationShortcutItem;
        shouldPerformAdditionalDelegateHandling = (LaunchedShortcutItem == null);
    }

    // Add dynamic shortcut items
    if (application.ShortcutItems.Length == 0) {
        var shortcut3 = new UIMutableApplicationShortcutItem (ShortcutIdentifier.Third, "Play") {
            LocalizedSubtitle = "Will play an item",
            Icon = UIApplicationShortcutIcon.FromType(UIApplicationShortcutIconType.Play)
        };

        var shortcut4 = new UIMutableApplicationShortcutItem (ShortcutIdentifier.Fourth, "Pause") {
            LocalizedSubtitle = "Will pause an item",
            Icon = UIApplicationShortcutIcon.FromType(UIApplicationShortcutIconType.Pause)
        };

        // Update the application providing the initial 'dynamic' shortcut items.
        application.ShortcutItems = new UIApplicationShortcutItem[]{shortcut3, shortcut4};
    }

    return shouldPerformAdditionalDelegateHandling;
}
```

Nun überprüft werden, finden Sie unter den `application` enthält bereits eine Reihe von dynamisch erstellt `ShortcutItems`, wenn dies nicht der Fall erstellen wir zwei neue `UIMutableApplicationShortcutItem` Objekte zu definieren, die neuen Elemente und Hinzufügen der `ShortcutItems` Array.

Der Code es bereits hinzugefügt der [behandeln eine schnelle Aktion](#Handling-a-Quick-Action) weiter oben behandelt diese dynamischen Schnellaktionen wie bei den statischen Adressen.

Sie können eine Mischung aus statischen und dynamischen Schnellaktions-Elemente (wie wir hier nur) erstellen, Sie sind nicht auf eines dieser Zuordnungsverfahren beschränkt hinzuweisen.

Weitere Informationen finden Sie unserem [iOS 9 ViewControllerPreview Beispiel](https://developer.xamarin.com/samples/monotouch/iOS9/ViewControllerPreview/) und Apple [ApplicationShortcuts: Mithilfe von UIApplicationShortcutItem](https://developer.apple.com/library/prerelease/ios/samplecode/ApplicationShortcuts/) Beispiel-app [UIApplicationShortcutItem Class Reference](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutItem_class/), [UIMutableApplicationShortcutItem Class Reference](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIMutableApplicationShortcutItem_class/) und [ Referenz für die UIApplicationShortcutIcon](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutIcon_Class/).

<a name="Testing-3D-Touch-in-the-Simulator" />

## <a name="testing-3d-touch-in-the-simulator"></a>Testen von 3D Touch im Simulator

Wenn die neueste Version von Xcode und iOS-Simulator auf einem Mac kompatibel, mit der ein Force Touch mit aktivieren Trackpad, können Sie 3D Touch-Funktionalität im Simulator testen.

Um diese Funktionalität zu aktivieren, führen Sie jede app in der simulierten iPhone-Hardware, die 3D Touch unterstützt (iPhone 6 s und höher). Wählen Sie als Nächstes die **Hardware** Menü in der iOS-Simulator, und aktivieren Sie die **Trackpad-Force-Verwendung für 3D Touch** Menüelements:

[![](3d-touch-images/simulator01.png "Wählen Sie das Menü \"Hardware\" in den iOS-Simulator, und aktivieren Sie die Verwendung Trackpad Kraft für 3D Touch-Menüelement")](3d-touch-images/simulator01.png#lightbox)

Mit dieser Funktion aktiv ist können Sie leicht auf der Mac Trackpad drücken, zum Aktivieren von 3D Touch, ebenso wie auf echten iPhone-Hardware.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die neuen 3D Touch-APIs in iOS 9 für das iPhone 6 s und iPhone 6 s zur Verfügung gestellt und. Es behandelt hinzufügen Druck Vertraulichkeit für eine app; verwenden die Peek "und" Pop in-app-Informationen aus dem aktuellen Kontext ohne Navigation schnell angezeigt; und Schnellaktionen mithilfe von Tastenkombinationen zu Ihrer app Bereitstellen der Funktionen am häufigsten verwendet.



## <a name="related-links"></a>Verwandte Links

- [iOS 9 ViewControllerPreview-Beispiel](https://developer.xamarin.com/samples/monotouch/ios9/ViewControllerPreview/)
- [iOS 9 ApplicationShortcuts-Beispiel](https://developer.xamarin.com/samples/monotouch/ios9/ApplicationShortcuts/)
- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Bereiten Sie die iPhone-apps auf 3D Touch](https://developer.apple.com/ios/3d-touch/)
