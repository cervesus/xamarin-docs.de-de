---
title: Einführung in 3D-Fingereingabe in xamarin. IOS
description: In diesem Artikel wird beschrieben, wie Sie 3D-Touchgesten verwenden, die mit iPhone 6S und iPhone 6S Plus eingeführt wurden. Diese Gesten ermöglichen Druckempfindlichkeit, Peek und Pop sowie schnelle Aktionen.
ms.prod: xamarin
ms.assetid: 806D051E-3791-40F7-9776-4E4D3E56F7F3
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/19/2017
ms.openlocfilehash: 46db28dab32f14fa476b9fbb42b788feb669aa74
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291867"
---
# <a name="introduction-to-3d-touch-in-xamarinios"></a>Einführung in 3D-Fingereingabe in xamarin. IOS

_In diesem Artikel wird die Verwendung der neuen iPhone 6S-und iPhone 6S plus 3D-Touchgesten in Ihrer APP behandelt._

[![](3d-touch-images/info01.png "Beispiele für 3D-apps mit aktiviertem Touchscreen")](3d-touch-images/info01.png#lightbox)

Dieser Artikel bietet eine Einführung in die Verwendung der neuen 3D-touchapis, um Ihren xamarin. IOS-apps, die auf den neuen iPhone 6S-und iPhone 6S Plus-Geräten ausgeführt werden, druckempfindliche Gesten hinzuzufügen.

Mit 3D-Toucheingabe kann eine iPhone-APP jetzt nicht nur erkennen, dass der Benutzer den Bildschirm des Geräts berührt, sondern es kann auch festgelegt werden, wie viel Druck der Benutzer ausübt und auf die unterschiedlichen Druck Ebenen reagiert.

der 3D-Fingerabdruck bietet die folgenden Features für Ihre APP:

- Unterscheidung nach [Druck](#Pressure-Sensitivity) : Apps können nun Messen, wie schwer oder hell der Benutzer den Bildschirm berührt und diese Informationen nutzt.
  Beispielsweise kann eine Zeichnungs-App abhängig von der Art und Weise, in der der Benutzer den Bildschirm berührt, zu einem dickeren oder dünneren.
- [Peek und Pop](#Peek-and-Pop) : Ihre APP kann jetzt den Benutzer mit seinen Daten interagieren, ohne aus dem aktuellen Kontext navigieren zu müssen. Wenn Sie auf dem Bildschirm auf den Bildschirm hart drücken, können Sie das gewünschte Element einsehen (z. b. die Vorschau einer Nachricht). Durch das Drücken von "Harder" können Sie in das Element eingeblendet werden.
- [Schnelle Aktionen](#Quick-Actions) : Stellen Sie sich schnell Aktionen wie die Kontextmenüs vor, die angezeigt werden können, wenn ein Benutzer mit der rechten Maustaste auf ein Element in einer Desktop-App klickt.
  Mithilfe von schnellen Aktionen können Sie Funktionen in Ihrer APP direkt über das App-Symbol auf dem Startbildschirm Verknüpfungen hinzufügen.
- [Testen von 3D](#Testing-3D-Touch-in-the-Simulator) -Finger Eingaben im Simulator: mit der richtigen Macintosh-Hardware können Sie im IOS-Simulator apps mit aktiviertem Touchscreen testen.

<a name="Pressure-Sensitivity" />

## <a name="pressure-sensitivity"></a>Druckempfindlichkeit

Wie bereits erwähnt, können Sie mithilfe neuer Eigenschaften der [uitouch](xref:UIKit.UITouch) -Klasse den Druck Aufwand Messen, den der Benutzer auf den Bildschirm des IOS-Geräts anwendet, und diese Informationen auf der Benutzeroberfläche verwenden. Beispielsweise wird ein Pinselstrich basierend auf der Druckmenge besser durchsichtig oder nicht transparent gemacht.

[![](3d-touch-images/pressure01.png "Ein Pinselstrich, der je nach Menge des Drucks als durchsichtig oder nicht transparent gerendert wird")](3d-touch-images/pressure01.png#lightbox)

Wenn Ihre APP auf IOS 9 (oder höher) ausgeführt wird und das IOS-Gerät die 3D-Fingereingabe unterstützen kann, wird das `TouchesMoved` Ereignis durch Änderungen der Druckvorgänge ausgelöst.

Wenn Sie z. b. `TouchesMoved` das-Ereignis einer [UIView](xref:UIKit.UIView)überwachen, können Sie den folgenden Code verwenden, um den aktuellen Druck zu erhalten, den der Benutzer auf den Bildschirm anwendet:

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

Die `MaximumPossibleForce` -Eigenschaft gibt den höchstmöglichen Wert für `Force` die-Eigenschaft des [uitouch](xref:UIKit.UITouch) basierend auf dem IOS-Gerät zurück, auf dem die app ausgeführt wird.

> [!IMPORTANT]
> Änderungen bei der Belastung bewirken, `TouchesMoved` dass das Ereignis ausgelöst wird, auch wenn sich die X/Y-Koordinaten nicht geändert haben. Aufgrund dieser Änderung des Verhaltens sollten ihre IOS-apps darauf vorbereitet sein, dass `TouchesMoved` das Ereignis häufiger aufgerufen wird und die X/Y-Koordinaten mit dem letzten `TouchesMoved` Aufruf übereinstimmen.




Weitere Informationen finden Sie in der touchcanvas [von Apple: Die effiziente und effektive](https://developer.apple.com/library/prerelease/ios/samplecode/TouchCanvas/) Verwendung der Beispiel-APP und der [uitouch-Klassenreferenz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITouch_Class/)mithilfe von uitouch.

<a name="Peek-and-Pop" />

## <a name="peek-and-pop"></a>Peek und Pop

der 3D-Fingerabdruck bietet Benutzern neue Möglichkeiten, um schneller als je zuvor mit Informationen in Ihrer APP zu interagieren, ohne von Ihrem aktuellen Speicherort zu navigieren.

Wenn Ihre APP z. b. eine Tabelle mit Nachrichten anzeigt, kann der Benutzer auf ein Element hart drücken, um seine Inhalte in einer über Lagerungs Ansicht anzuzeigen (was Apple als *Peek*bezeichnet).

[![](3d-touch-images/peekandpop01.png "Beispiel für das Einsehen von Inhalten")](3d-touch-images/peekandpop01.png#lightbox)

Wenn der Benutzer härter drückt, wird er in die reguläre Meldungs Ansicht (in der Ansicht als *Pop*-Ping bezeichnet) eingegeben.

### <a name="checking-for-3d-touch-availability"></a>Verfügbarkeit von 3D-Finger Eingaben wird überprüft

Beim Arbeiten mit einem `UIViewController` können Sie den folgenden Code verwenden, um festzustellen, ob das IOS-Gerät, auf dem die app ausgeführt wird, 3D-Fingereingabe unterstützt

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

Diese Methode kann vor *oder nach* `ViewDidLoad()`aufgerufen werden.

### <a name="handling-peek-and-pop"></a>Behandeln von Peek und Pop

Auf einem IOS-Gerät, das 3D-toucheingaben verarbeiten kann, können wir eine `UIViewControllerPreviewingDelegate` Instanz der-Klasse verwenden, um die Anzeige von **Peek** -und **Pop** -Element Details zu verarbeiten. Wenn beispielsweise ein Tabellen Ansichts Controller mit dem Namen `MasterViewController` verwendet wird, können wir den folgenden Code verwenden, um **Peek** und **Pop**zu unterstützen:

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

Die `GetViewControllerForPreview` -Methode wird verwendet, um den **Peek** -Vorgang auszuführen. Er erhält Zugriff auf die Tabellenzelle und die `DetailViewController` Sicherungsdaten und lädt dann aus dem aktuellen Storyboard. Wenn `PreferredContentSize` Sie auf (0,0) festlegen, wird die standardmäßige **Peek** -Ansichts Größe angefordert. Schließlich werden alle Elemente, mit `previewingContext.SourceRect = cell.Frame` denen die Zelle angezeigt wird, weich, und wir geben die neue Ansicht für die Anzeige zurück.

Der `CommitViewController` verwendet die Ansicht, die wir in der **Peek** -Ansicht für die **Pop** -Ansicht erstellt haben, wenn der Benutzer eine härtere Taste drückt.

### <a name="registering-for-peek-and-pop"></a>Registrieren für Peek und Pop

Auf dem Ansichts Controller, in dem der Benutzer die **Peek** -und **Pop** -Elemente einsehen soll, müssen Sie sich für diesen Dienst registrieren. Im obigen Beispiel eines Tabellen Ansichts Controllers (`MasterViewController`) verwenden wir den folgenden Code:

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

Hier rufen wir die `RegisterForPreviewingWithDelegate` -Methode mit einer Instanz von auf, die `PreviewingDelegate` wir oben erstellt haben. Auf IOS-Geräten, die 3D-Finger Eingaben unterstützen, kann der Benutzer hart auf ein Element klicken, um es zu sehen. Wenn Sie noch schwieriger drücken, wird das Element in der Standard Anzeige Ansicht angezeigt.

Weitere Informationen finden Sie in unserem Beispiel für das [IOS 9-applicationverknüpfungen](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-viewcontrollerpreview) und in den viewcontrollerpreviews von [Apple: ](https://developer.apple.com/library/prerelease/ios/samplecode/ViewControllerPreviews/Introduction/Intro.html) Mithilfe der UIViewController-Vorschau-API-Beispiel-APP, der [uipreviewaction-Klassenreferenz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewAction_Class/), der [uipreviewaction Group-Klassenreferenz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionGroup_Class/) und der [uipreviewaction Item-Protokoll Referenz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionItem_Protocol/).

<a name="Quick-Actions" />

## <a name="quick-actions"></a>Schnelle Aktionen

Mithilfe von 3D-toucheingaben und schnellen Aktionen können Sie in der APP über das Startbildschirm Symbol auf dem IOS-Gerät allgemeine, schnelle und leicht zu verwendbare Verknüpfungen zu Funktionen in Ihrer APP hinzufügen.

Wie bereits erwähnt, können Sie sich schnell Aktionen wie die Kontextmenüs vorstellen, die per Popup angezeigt werden können, wenn ein Benutzer mit der rechten Maustaste auf ein Element in einer Desktop-App klickt. Sie sollten schnelle Aktionen verwenden, um Verknüpfungen zu den gängigsten Funktionen oder Features Ihrer APP bereitzustellen.

[![](3d-touch-images/quickactions01.png "Beispiel für ein schnelles Aktionsmenü")](3d-touch-images/quickactions01.png#lightbox)


### <a name="defining-static-quick-actions"></a>Definieren statischer schneller Aktionen

Wenn eine oder mehrere der für Ihre APP erforderlichen schnellen Aktionen statisch sind und nicht geändert werden müssen, können Sie Sie in der `Info.plist` Datei der app definieren. Bearbeiten Sie diese Datei in einem externen Editor, und fügen Sie die folgenden Schlüssel hinzu:

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

Hier definieren wir zwei statische Elemente für schnelle Aktionen mit den folgenden Schlüsseln:

- `UIApplicationShortcutItemIconType`: Definiert das Symbol, das vom Element für die schnelle Aktion als einer der folgenden Werte angezeigt wird:
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

  ![](3d-touch-images/uiapplicationshortcuticontype.png "Uiapplicationshortcuticontype-Bilder")

- `UIApplicationShortcutItemSubtitle`: Definiert den Untertitel für das Element.
- `UIApplicationShortcutItemTitle`: Definiert den Titel des Elements.
- `UIApplicationShortcutItemType`-Ist ein Zeichen folgen Wert, der zum Identifizieren des Elements in unserer App verwendet wird. Weitere Informationen finden Sie in folgendem Abschnitt.

> [!IMPORTANT]
> Auf die in der `Info.plist` Datei festgelegten Quick Action-Verknüpfungs Elemente kann nicht mit der `Application.ShortcutItems` -Eigenschaft zugegriffen werden. Sie werden nur an den `HandleShortcutItem` -Ereignishandler übermittelt.





### <a name="identifying-quick-action-items"></a>Erkennen von schnellen Aktions Elementen

Wie oben gezeigt, haben Sie beim Definieren der schnell Aktions Elemente in Ihrer APP `Info.plist`dem `UIApplicationShortcutItemType` Schlüssel einen Zeichen folgen Wert zugewiesen, um Sie zu identifizieren.

Fügen Sie dem Projekt Ihrer APP eine-Klasse mit dem Namen `ShortcutIdentifier` hinzu, um diese Bezeichner einfacher im Code arbeiten zu lassen.

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

### <a name="handling-a-quick-action"></a>Umgang mit einer schnellen Aktion

Als nächstes müssen Sie die `AppDelegate.cs` Datei Ihrer APP ändern, damit Sie den Benutzer behandelt, der auf dem Startbildschirm auf dem Symbol ihrer App ein schnelles Aktions Element auswählt.

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

Zuerst definieren wir eine öffentliche `LaunchedShortcutItem` Eigenschaft, um das zuletzt ausgewählte Element der schnellen Aktion vom Benutzer zu verfolgen. Dann überschreiben wir die `FinishedLaunching` -Methode und über `launchOptions` prüfen, ob Sie erfolgreich waren und ob Sie ein schnelles Aktions Element enthalten. Wenn dies der Fall ist, speichern wir die schnelle Aktion `LaunchedShortcutItem` in der Eigenschaft.

Als nächstes überschreiben wir `OnActivated` die-Methode und übergeben ein beliebiges ausgewähltes `HandleShortcutItem` Schnellstart Element an die Methode, für die eine Aktion ausgeführt wird. Zurzeit wird nur eine Nachricht in die **Konsole**geschrieben. In einer echten APP würden Sie die erforderliche Aktion behandeln. Nachdem die Aktion ausgeführt wurde, wird die `LaunchedShortcutItem` -Eigenschaft gelöscht.

Wenn Ihre APP bereits ausgeführt wurde, wird die `PerformActionForShortcutItem` -Methode aufgerufen, um das Element für die schnelle Aktion zu verarbeiten. Daher müssen wir die Methode überschreiben und hier auch die- `HandleShortcutItem` Methode aufrufen.


### <a name="creating-dynamic-quick-action-items"></a>Erstellen dynamischer schneller Aktions Elemente

Neben der Definition statischer schneller Aktions Elemente in der `Info.plist` Datei der App können Sie dynamische schnelle Aktionen erstellen. Wenn Sie zwei neue dynamische schnelle Aktionen definieren möchten, `AppDelegate.cs` bearbeiten Sie die Datei erneut `FinishedLaunching` , und ändern Sie die Methode so, dass Sie wie folgt aussieht:

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

Nun überprüfen wir, ob `application` das bereits einen dynamisch erstellten `ShortcutItems`Satz enthält, wenn wir nicht zwei neue `UIMutableApplicationShortcutItem` Objekte erstellen, um die neuen Elemente zu definieren und Sie dem `ShortcutItems` Array hinzuzufügen.

Der Code, den wir bereits im obigen Abschnitt [Handling a Quick Action](#Handling-a-Quick-Action) hinzugefügt haben, behandelt diese dynamischen schnellen Aktionen genau wie die statischen.

Beachten Sie, dass Sie eine Mischung aus statischen und dynamischen schnell Aktions Elementen erstellen können (wie hier beschrieben), Sie sind nicht auf eine der beiden Optionen beschränkt.

Weitere Informationen finden Sie in unserem [Beispiel für IOS 9 viewcontrollerpreview](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-viewcontrollerpreview) , und sehen [Sie sich die applicationtastenkombinationen von Apple an: Mithilfe](https://developer.apple.com/library/prerelease/ios/samplecode/ApplicationShortcuts/) der uiapplicationshortcutitem-Beispiel-APP, der [uiapplicationshortcutitem-Klassenreferenz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutItem_class/), der [uimutableapplicationshortcutitem-Klassenreferenz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIMutableApplicationShortcutItem_class/) und der [uiapplicationshortcuticon-Klassenreferenz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutIcon_Class/).

<a name="Testing-3D-Touch-in-the-Simulator" />

## <a name="testing-3d-touch-in-the-simulator"></a>Testen der 3D-Fingereingabe im Simulator

Wenn Sie die neueste Version von Xcode und den IOS-Simulator auf einem kompatiblen Mac mit einem Force Touch "Trackpad aktivieren" verwenden, können Sie die 3D-Touchfunktionalität im Simulator testen.

Um diese Funktionalität zu aktivieren, führen Sie jede app in simulierter iPhone-Hardware aus, die 3D-Fingereingabe (iPhone 6S und höher) unterstützt. Wählen Sie als nächstes im IOS-Simulator das Menü **Hardware** aus, und aktivieren Sie das Menü Element **Trackpad-Force für 3D-Toucheingabe verwenden** :

[![](3d-touch-images/simulator01.png "Wählen Sie im IOS-Simulator das Menü Hardware aus, und aktivieren Sie das Menü Element Trackpad-Force für 3D-Toucheingabe verwenden.")](3d-touch-images/simulator01.png#lightbox)

Wenn Sie diese Funktion aktiv haben, können Sie auf der Trackpad-Seite von Mac auf "schwerer" klicken, um die 3D-Toucheingabe zu aktivieren.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die neuen 3D-Fingereingabe-APIs eingeführt, die in ios 9 für iPhone 6S und iPhone 6S Plus zur Verfügung gestellt wurden. Es wurde das Hinzufügen von Druck Sensitivität zu einer APP abgedeckt. Verwenden von Peek und Pop, um schnell in-app-Informationen aus dem aktuellen Kontext ohne Navigation anzuzeigen. und verwenden Sie schnelle Aktionen, um Verknüpfungen zu den am häufigsten verwendeten Features Ihrer APP bereitzustellen.



## <a name="related-links"></a>Verwandte Links

- [IOS 9 viewcontrollerpreview-Beispiel](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-viewcontrollerpreview)
- [Beispiel für IOS 9 applicationverknüpfungen](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-applicationshortcuts)
- [IOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Bringen Sie Ihre iPhone-Apps für die 3D-Fingereingabe bereit](https://developer.apple.com/ios/3d-touch/)
