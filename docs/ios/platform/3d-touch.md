---
title: "Einführung in 3D Touch"
description: "In diesem Artikel erläutert die Verwendung der neuen iPhone 6 s und iPhone 6 s Plus 3D Touch Aktionen in Ihrer app."
ms.topic: article
ms.prod: xamarin
ms.assetid: 806D051E-3791-40F7-9776-4E4D3E56F7F3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: d544647a2718d6b511551f4341dee51b2c68941f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="introduction-to-3d-touch"></a>Einführung in 3D Touch

_In diesem Artikel erläutert die Verwendung der neuen iPhone 6 s und iPhone 6 s Plus 3D Touch Aktionen in Ihrer app._

[![](3d-touch-images/info01.png "Beispiele für die 3D Touch-fähige apps")](3d-touch-images/info01.png#lightbox)

Dieser Artikel bietet eine Einführung in die Verwendung der neuen 3D Touch-APIs vertrauliche Gesten Druck auf Ihre apps Xamarin.iOS hinzufügen, die auf dem neuen iPhone 6 s und iPhone 6 s ausgeführt werden, und Plus-Geräte.

Mit 3D Touch ist eine iPhone-app jetzt nicht nur feststellen, dass der Benutzer das Gerät Bildschirm berührt, aber dies ist möglich, wie viel Druck sinnvoll ist, der Benutzer selbst ist, und auf die verschiedenen arbeitsspeicherauslastung reagieren können.

3D Touch bietet die folgenden Funktionen zur app:

- [Druck Empfindlichkeit](#Pressure-Sensitivity) - Apps können jetzt wie starke messen oder berührt Licht der Benutzer den Bildschirm, und ergreifen Sie Vorteil dieser Daten.
  Beispielsweise kann eine zeichnen-app eine Linie dicker oder flacher basierend auf der Festplatte wie der Benutzer den Bildschirm berührt ausführen.
- [Peek and Pop](#Peek-and-Pop) -Ihrer app können nun den Benutzer seine Daten interagieren, ohne außerhalb des aktuellen Kontexts navigieren kann. Durch Drücken von Festplatte auf dem Bildschirm den Bildschirm, können sie mit dem Element einsehen, die sie interessiert (z. B. das Ausführen einer Vorschau für eine Nachricht). Durch Drücken von schwieriger, können sie in das Element abrufen.
- [Schnelle Aktionen](#Quick-Actions) -vorstellen von Schnellaktionen wie die Kontextmenüs, die per pop ausgelesen von Volltextkatalogen können sein, wenn ein Benutzer in einer desktop-app auf ein Element klickt.
  Mithilfe von Schnellaktionen, können Sie Verknüpfungen mit Funktionen in Ihrer app direkt über das app-Symbol auf der Startseite hinzufügen.
- [Testen 3D Touch im Simulator](#Testing-3D-Touch-in-the-Simulator) – Sie können 3D Touch aktiviert apps im iOS-Simulator testen, mit der richtigen Mac-Hardware.

<a name="Pressure-Sensitivity" />

## <a name="pressure-sensitivity"></a>Druck Sensitivität

Wie oben angegeben, mit der neuen Eigenschaften der [UITouch](https://developer.xamarin.com/api/type/UIKit.UITouch/) Klasse können Sie messen Sie die speicherauslastung, die der Benutzer auf dem iOS-Gerät Bildschirm anwendet und verwenden Sie diese Informationen in der Benutzeroberfläche. Z. B. vornehmen einen Pinselstrich mehr transparent oder undurchsichtig basierend auf Druck.

[![](3d-touch-images/pressure01.png "Ein Pinselstrich gerendert wird, mehr als transparent oder undurchsichtig Grundlage Druck")](3d-touch-images/pressure01.png#lightbox)

Als Ergebnis 3D Touch, wenn Ihre app unter iOS 9 (oder höher) ausgeführt wird und das iOS-Gerät kann unterstützenden 3D Touch Änderungen bei ungenügendem führt dazu, dass die `TouchesMoved` Ereignis ausgelöst wurde.

Beispielsweise, wenn die Überwachung der `TouchesMoved` -Ereignis ein [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/), können Sie den folgenden Code um die Auslastung des aktuellen abzurufen, die der Benutzer auf dem Bildschirm anwendet:

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

Die `MaximumPossibleForce` Eigenschaft gibt die höchsten möglichen Wert für die `Force` Eigenschaft von der [UITouch](https://developer.xamarin.com/api/type/UIKit.UITouch/) basierend auf dem iOS-Gerät, das die app ausgeführt wird.

> [!IMPORTANT]
> **Hinweis:** Änderungen in Druck führt dazu, dass die `TouchesMoved` Ereignis ausgelöst wurde, auch wenn das "X" / Y-Koordinaten, nicht geändert wurden. Aufgrund dieser Änderung im Verhalten, sollte Ihre iOS-apps für vorbereitet werden die `TouchesMoved` aufzurufenden Ereignisses häufiger und für die X- / Y-Koordinaten mit dem letzten identisch sein `TouchesMoved` aufrufen.




Weitere Informationen finden Sie in der Apple- [TouchCanvas: UITouch mithilfe von effizienten und effektiven](https://developer.apple.com/library/prerelease/ios/samplecode/TouchCanvas/) Beispiel-app und [UITouch Class Reference](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITouch_Class/).

<a name="Peek-and-Pop" />

## <a name="peek-and-pop"></a>Peek / "und" Pop

3D Touch bietet neue Möglichkeiten für einen Benutzer Informationen innerhalb der app, die schneller als je zuvor, interagieren, ohne dass von ihrem aktuellen Speicherort zu navigieren.

Wenn Ihre app eine Tabelle mit Nachrichten angezeigt wird, der Benutzer zu drücken hart auf ein Element, um seinen Inhalt in einer Überlagerungsansicht in der Vorschau anzeigen (Apple bezieht sich auf als eine *einsehen*).

[![](3d-touch-images/peekandpop01.png "Ein Beispiel der Peeking am Inhalt")](3d-touch-images/peekandpop01.png#lightbox)

Wenn der Benutzer schwieriger drückt, werden sie der regulären Nachrichtenansicht eingeben (Dies wird als bezeichnet *Pop*-Ping in der Ansicht).

### <a name="checking-for-3d-touch-availability"></a>Überprüfung auf 3D Touch-Verfügbarkeit

Bei der Arbeit mit einem [UIViewController]() können Sie den folgenden Code um zu überprüfen, ob die app ausgeführt wird, auf das iOS-Gerät 3D Touch unterstützt:

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

### <a name="handling-peek-and-pop"></a>Behandeln von Peek / "und" Pop

Auf einem iOS-Gerät, die 3D Touch verarbeiten kann, können wir eine Instanz von der `UIViewControllerPreviewingDelegate` Klasse zur Behandlung der Dialogfeldanzeige **einsehen** und **Pop** Artikeldetails. Angenommen, wenn wir hatten View-Controller eine Tabelle namens `MasterViewController ` können wir den folgenden Code zur Unterstützung **einsehen** und **Pop**:

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

Die `GetViewControllerForPreview` Methode wird zum Ausführen der **einsehen** Vorgang. Er erhält Zugriff auf die Zelle, und Sichern von Tabellendaten und lädt dann die `DetailViewController` über das aktuelle Storyboard. Durch Festlegen der `PreferredContentSize` (0,0) sind bitten wir für den standardmäßigen **einsehen** Größe anzeigen. Schließlich Weichzeichnen wir alle Elemente außer der Zelle, die wir Anzeigen von mit `previewingContext.SourceRect = cell.Frame` und wir die neue Ansicht für die Anzeige zurückzukehren.

Die `CommitViewController` wiederverwendet die Sicht erstellt wird in der **einsehen** für die **Pop** anzeigen, wenn der Benutzer schwieriger drückt.

### <a name="registering-for-peek-and-pop"></a>Registrieren für Peek / "und" Pop

Über die View-Controller, die es dem Benutzer ermöglichen möchten **einsehen** und **Pop** Elemente aus, für diesen Dienst registrieren müssen. In der oben genannte Beispiel von einer Tabelle-View-Controller (`MasterViewController`), würden wir den folgenden Code verwenden:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Check to see if 3D Touch is available
    if (TraitCollection.ForceTouchCapability == UIForceTouchCapability.Available) {
        // Regiser for Peek and Pop
        RegisterForPreviewingWithDelegate(new PreviewingDelegate(this), View);
    }
    ...

}
```

Wir sind hier Aufrufen der `RegisterForPreviewingWithDelegate` Methode mit einer Instanz von der `PreviewingDelegate` oben erstellten. Auf iOS-Geräten, die 3D Touch unterstützen, kann der Benutzer auf ein Element, um Peek es schwer drücken. Wenn sie noch schwerer drücken, wird das Element hinein standard Pop anzeigen.

Weitere Informationen finden Sie unter unsere [iOS 9 ApplicationShortcuts Beispiel](https://developer.xamarin.com/samples/monotouch/iOS9/ViewControllerPreview/) und Apple [ViewControllerPreviews: verwenden die Vorschau der APIs UIViewController](https://developer.apple.com/library/prerelease/ios/samplecode/ViewControllerPreviews/Introduction/Intro.html) Beispiel-app [ Klassenreferenz UIPreviewAction](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewAction_Class/), [UIPreviewActionGroup Klassenreferenz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionGroup_Class/) und [UIPreviewActionItem Protokollreferenz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionItem_Protocol/).

<a name="Quick-Actions" />

## <a name="quick-actions"></a>Schnelle Aktionen

3D Touch und Schnellaktionen verwenden, können Sie allgemeine, schnell und einfach zu Verknüpfungen der Zugriff auf Funktionen in Ihrer app über das Symbol des Home-Bildschirm auf dem iOS-Gerät hinzufügen.

Wie bereits erwähnt, können Sie Schnellaktionen wie die Kontextmenüs vorstellen, die per pop ausgelesen von Volltextkatalogen kann sein, wenn ein Benutzer in einer desktop-app auf ein Element klickt. Sie sollten Schnellaktionen zum Bereitstellen von Verknüpfungen zu den am häufigsten verwendeten Funktionen oder Funktionen der app verwenden.

[![](3d-touch-images/quickactions01.png "Ein Beispiel für eine schnelle Aktionsmenü")](3d-touch-images/quickactions01.png#lightbox)


### <a name="defining-static-quick-actions"></a>Definieren von statischen schnelle Aktionen

Wenn eine oder mehrere der Schnellaktionen, die von Ihrer Anwendung erforderlich statisch sind und nicht ändern müssen, können Sie sie definieren, in der app `Info.plist` Datei. Bearbeiten Sie diese Datei in einen externen Editor, und fügen Sie die folgenden Schlüssel:

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

- `UIApplicationShortcutItemIconType` -Definiert das Symbol ", das vom Element" Schnellaktions "als einen der folgenden Werte angezeigt werden:
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
* `UIApplicationShortcutItemTitle` -Definiert den Titel für das Element.
* `UIApplicationShortcutItemType` -Ist ein Zeichenfolgenwert, den zum Identifizieren des Elements in dieser app verwendet werden. Weitere Informationen finden Sie in folgendem Abschnitt.

> [!IMPORTANT]
> **Hinweis:** Schnellaktions Verknüpfung-Elemente, die festgelegt werden, in der `Info.plist` Datei kann nicht zugegriffen werden, mit der `Application.ShortcutItems` Eigenschaft. Sie sind nur im übergeben, um die `HandleShortcutItem` -Ereignishandler. 





### <a name="identifying-quick-action-items"></a>Identifizieren von Quick-Aktionselemente

Wie oben gezeigt, wenn Sie definiert die schnelle-Aktionselemente in Ihrer app `Info.plist`, Ihnen zugewiesen einen Zeichenfolgenwert, der `UIApplicationShortcutItemType` -Taste, um sie zu identifizieren.

Um diesen Bezeichner mit Code arbeiten vereinfachen, fügen Sie eine Klasse mit dem Namen `ShortcutIdentifier` auf Ihre app Projekt, und stellen sie wie folgt aussehen:

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

### <a name="handling-a-quick-action"></a>Behandlung von einem Schnellaktions

Als Nächstes müssen Sie Ihre app ändern `AppDelegate.cs` Datei, die Benutzer, die Auswahl eines Schnellaktions aus Ihrer app-Symbol auf der Startseite zu behandeln.

Nehmen Sie die folgenden Änderungen vor:

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

Zuerst definieren wir eine öffentliche `LaunchedShortcutItem` Eigenschaft, um das letzte Element der ausgewählten Schnellaktions vom Benutzer zu verfolgen. Wir überschreiben Sie anschließend die `FinishedLaunching` -Methode und sehen, wenn `launchOptions` übergeben wurde und wenn sie ein Schnellaktions-Element enthält. Wenn dies der Fall ist, speichern wir die Schnellaktions in der `LaunchedShortcutItem` Eigenschaft.

Als Nächstes überschreiben die `OnActivated` Methode und übergeben einer Schnellstart Element aus, um ausgewählt die `HandleShortcutItem` Methode, um die abgearbeitet werden. Derzeit schreiben wir nur eine Nachricht an die **Konsole**. In eine wirkliche app wäre Sie verarbeiten, was ever Aktion erforderlich war. Nachdem die Aktion ausgeführt wurde, die `LaunchedShortcutItem` Eigenschaft deaktiviert ist.

Abschließend, wenn die app bereits ausgeführt wurde, die `PerformActionForShortcutItem` Methode würde aufgerufen werden, um die schnelle Aktionselement, über das behandeln, daher muss außer Kraft setzen, und rufen Sie unsere `HandleShortcutItem` Methode auch hier.


### <a name="creating-dynamic-quick-action-items"></a>Erstellen von dynamischen Quick-Aktionselemente

Zusätzlich zum Definieren von statischen Schnellaktions Elemente in Ihrer app `Info.plist` -Datei können Sie dynamische auf dynamische Schnellaktionen erstellen. Um zwei neue dynamische schnelle Aktionen zu definieren, bearbeiten Ihre `AppDelegate.cs` Datei erneut, und ändern die `FinishedLaunching` Methode gesucht werden soll, wie folgt:

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

Jetzt überprüft werden, finden Sie unter der `application` enthält bereits eine Reihe von dynamisch erstellte `ShortcutItems`, sofern dies nicht der Fall wir erstellen zwei neue `UIMutableApplicationShortcutItem` von Objekten in die neuen Elemente zu definieren und Hinzufügen der `ShortcutItems` Array.

Den Code wir bereits hinzugefügt, der [Behandlung von einem Schnellaktions](#Handling-a-Quick-Action) obigen Abschnitt behandelt diese dynamische schnelle Aktionen genau wie die statische.

Beachten Sie, dass eine Mischung aus statischen und dynamischen Quick-Aktionselemente kann (wie wir hier tun werden) erstellt, Sie sind nicht auf eines dieser Zuordnungsverfahren beschränkt.

Weitere Informationen finden Sie unsere [iOS 9 ViewControllerPreview Beispiel](https://developer.xamarin.com/samples/monotouch/iOS9/ViewControllerPreview/) und finden Sie in der Apple- [ApplicationShortcuts: mithilfe von UIApplicationShortcutItem](https://developer.apple.com/library/prerelease/ios/samplecode/ApplicationShortcuts/) Beispiel-app [ Klassenreferenz UIApplicationShortcutItem](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutItem_class/), [UIMutableApplicationShortcutItem Klassenreferenz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIMutableApplicationShortcutItem_class/) und [UIApplicationShortcutIcon Klassenreferenz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutIcon_Class/).

<a name="Testing-3D-Touch-in-the-Simulator" />

## <a name="testing-3d-touch-in-the-simulator"></a>Testen 3D Touch im Simulator

Wenn mit der neuesten Version von Xcode und den iOS-Simulator auf einem Mac kompatibel, mit der ein Force berühren Trackpad zu aktivieren, können Sie 3D Touch-Funktionalität im Simulator testen.

Um diese Funktionalität zu aktivieren, führen Sie jede app in simulierten iPhone-Hardware, die 3D Touch unterstützt (iPhone 6 s und höher). Wählen Sie als Nächstes die **Hardware** in der iOS-Simulator, und aktivieren Sie im Menü der **verwenden Trackpad Force für 3D Touch** Menüelement:

[![](3d-touch-images/simulator01.png "Wählen Sie im Menü Hardware im iOS-Simulator aus, und Aktivieren der "Force" verwenden Trackpad für 3D Touch-Menüelement")](3d-touch-images/simulator01.png#lightbox)

Mit dieser Funktion aktiv ist können Sie schwieriger auf dem Mac Trackpad drücken, um 3D Touch ebenso wie auf echten iPhone Hardware zu aktivieren.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde eingeführt, die neue 3D Touch-APIs in iOS 9 für iPhone 6 s und iPhone 6 s zur Verfügung gestellt und. Es fallen hinzufügen Druckempfindlichkeit an eine app; Verwenden Peek / "und" Pop, um schnell in app-Informationen aus dem aktuellen Kontext ohne Navigation anzuzeigen; und Verknüpfungen zu Ihrer app bereitstellen mithilfe eines Schnellaktionen des am häufigsten verwendeten Funktionen.



## <a name="related-links"></a>Verwandte Links

- [iOS 9 ViewControllerPreview-Beispiel](https://developer.xamarin.com/samples/monotouch/ios9/ViewControllerPreview/)
- [iOS 9 ApplicationShortcuts-Beispiel](https://developer.xamarin.com/samples/monotouch/ios9/ApplicationShortcuts/)
- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Bereiten Sie Ihrer iPhone-apps für die 3D Touch vor](https://developer.apple.com/ios/3d-touch/)
