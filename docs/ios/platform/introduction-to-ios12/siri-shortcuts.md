---
title: Siri-Verknüpfungen in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Siri-Verknüpfungen in IOS 12 verwendet werden. Dabei werden nsuseractivities, benutzerdefinierte Intents, das Zuweisen von sprach Verknüpfungen, relevante Verknüpfungen und mehr erläutert.
ms.prod: xamarin
ms.assetid: 86424F79-3A7D-436E-927D-9A3267DA333B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/08/2018
ms.openlocfilehash: f0927a6d6d5e3b9db6f203f779fbd50a026ce7e8
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "70226566"
---
# <a name="siri-shortcuts-in-xamarinios"></a>Siri-Verknüpfungen in xamarin. IOS

In [IOS 10](~/ios/platform/sirikit/index.md)hat Apple das Sirikit eingeführt, das es ermöglicht, Messaging, VoIP-aufrufen, Zahlungen, Workouts, Fahrten Reservierung und Foto Suche-apps, die mit Siri interagieren, zu erstellen.

In [IOS 11](~/ios/platform/introduction-to-ios11/sirikit.md)hat das Sirikit Unterstützung für mehr App-Typen und mehr Flexibilität bei der Anpassung der Benutzeroberfläche erhalten.

IOS 12 fügt Siri-Verknüpfungen hinzu, sodass alle App-Typen ihre Funktionalität für Siri verfügbar machen können. Siri erfährt, wenn bestimmte App-basierte Aufgaben für den Benutzer am relevantesten sind, und verwendet dieses wissen, umüber Verknüpfungen potenzielle Aktionen vorzuschlagen. Wenn Sie auf eine Verknüpfung tippen oder Sie mit einem Sprachbefehl aufrufen, wird eine APP geöffnet oder eine Hintergrundaufgabe ausgeführt.

Verknüpfungen sollten verwendet werden, um die Fähigkeit eines Benutzers zu beschleunigen, eine gängige Aufgabe zu erledigen – in vielen Fällen, ohne dass die betreffende APP überhaupt geöffnet wird.

## <a name="sample-app-soup-chef"></a>Beispiel-App: Suppe Chef

Um Siri-Verknüpfungen besser zu verstehen, sehen Sie sich die Beispiel-app " [Soup Chef](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-soupchef) " an. Mit Suppe Chef können Benutzer Bestellungen aus einem imaginären Suppen Restaurant platzieren, den Bestellverlauf anzeigen und Ausdrücke definieren, die beim Sortieren von Suppen durch Interaktion mit Siri verwendet werden sollen.

> [!TIP]
> Vor dem Testen von Suppe Chef auf einem IOS 12-Simulator oder-Gerät aktivieren Sie die folgenden beiden Einstellungen, die beim Debuggen von Verknüpfungen nützlich sind:
>
> - Aktivieren Sie in der App " **Einstellungen** " die Option **Entwickler > aktuelle Tastenkombinationen anzeigen**.
> - Aktivieren Sie in der App " **Einstellungen** " die Option **Entwickler > auf Sperrbildschirm Spenden anzeigen**.
>
> Diese Debugeinstellungen erleichtern das Auffinden von zuletzt erstellten (anstelle von vorhergesagten) Verknüpfungen auf dem IOS-Sperrbildschirm und dem Suchbildschirm.

So verwenden Sie die Beispiel-App:

- Installieren und Ausführen der Beispiel-app "Soup Chef" auf einem IOS 12-Simulator oder- [Gerät](#testing-on-device).
- Klicken Sie **+** oben rechts auf die Schaltfläche, um einen neuen Auftrag zu erstellen.
- Wählen Sie eine Art von Suppe aus, geben Sie eine Menge und Optionen an, und tippen Sie auf **Bestellung aufgeben**.
- Tippen Sie auf dem Bildschirm **Bestellverlauf** auf die neu erstellte Reihenfolge, um die zugehörigen Details anzuzeigen.
- Tippen Sie am unteren Rand des Bildschirms für die Bestelldetails **auf zu Siri hinzufügen**.
- Notieren Sie einen sprach Ausdruck, der der Bestellung zugeordnet werden soll, und tippen Sie auf **done**.
- Minimieren Sie den Suppen Chef, rufen Sie Siri auf, und platzieren Sie die Bestellung erneut mit dem sprach Ausdruck, den Sie soeben aufgezeichnet haben.
- Nachdem Siri den Auftrag abgeschlossen hat, öffnen Sie "Soup Chef" erneut, und beachten Sie, dass die neue Bestellung im Bildschirm " **Bestellverlauf** " aufgeführt ist.

Die Beispiel-App veranschaulicht Folgendes:

- [Verwenden Sie eine nsuseractivity-Verknüpfung, um eine APP zu öffnen](#using-an-nsuseractivity-shortcut-to-open-an-app).
- [Verwenden Sie eine benutzerdefinierte Intent-Verknüpfung, um eine Aufgabe auszuführen](#using-a-custom-intent-shortcut-to-perform-a-task).
- [Stellen Sie eine Benutzeroberfläche für eine benutzerdefinierte Absicht bereit](#providing-a-user-interface-for-a-custom-intent).
- [Erstellen Sie eine sprach Verknüpfung](#creating-a-voice-shortcut).

## <a name="infoplist-and-entitlementsplist"></a>Info. plist und Berechtigungen. plist

Schauen Sie sich die Dateien " **Info. plist** " und " **Berechtigungen. plist** " an, bevor Sie sich ausführlicher mit dem "Suppe Chef"-Code befassen.

### <a name="infoplist"></a>Info.plist

Die Datei " **Info. plist** " im Projekt " **soupchef** " definiert den `com.xamarin.SoupChef` **Bündel Bezeichner** als. Diese Bündel-ID wird als Präfix für die Bündel Bezeichner der Intents-und Intents-UI-Erweiterungen verwendet, die weiter unten in diesem Dokument erläutert werden.

Die Datei " **Info. plist** " enthält außerdem Folgendes:

```xml
<key>NSUserActivityTypes</key>
<array>
    <string>OrderSoupIntent</string>
    <string>com.xamarin.SoupChef.viewMenu</string>
</array>
```

Dieses `NSUserActivityTypes` Schlüssel-Wert-Paar gibt an, dass der Suppe Chef weiß `OrderSoupIntent` [`ActivityType`](xref:Foundation.NSUserActivity.ActivityType) , wie ein [`NSUserActivity`](xref:Foundation.NSUserActivity) behandelt werden soll, und eine mit der von "com. xamarin. soupchef. viewmenu".

Aktivitäten und benutzerdefinierte Intents, die im Gegensatz zu Ihren Erweiterungen an die APP selbst übermittelt werden, `AppDelegate` werden in [`UIApplicationDelegate`](xref:UIKit.UIApplicationDelegate) (a [`ContinueUserActivity`](xref:UIKit.UIApplicationDelegate.ContinueUserActivity*) durch die-Methode) behandelt.

### <a name="entitlementsplist"></a>Entitlements.plist

Die Datei " **Berechtigungen. plist** " im Projekt " **soupchef** " enthält Folgendes:

```xml
<key>com.apple.security.application-groups</key>
<array>
    <string>group.com.xamarin.SoupChef</string>
</array>
<key>com.apple.developer.siri</key>
<true/>
```

Diese Konfiguration gibt an, dass die APP die "Group. com. xamarin. soupchef"-App-Gruppe verwendet. Die **soupchefintents** -App-Erweiterung verwendet dieselbe App-Gruppe, mit der die beiden Projekte gemeinsam verwendet werden können.[`NSUserDefaults`](xref:Foundation.NSUserDefaults)
Daten

Der `com.apple.developer.siri` Schlüssel gibt an, dass die APP mit Siri interagiert.

> [!NOTE]
> Die Buildkonfiguration des **soupchef** -Projekts legt **benutzerdefinierte Berechtigungen** auf " **Berechtigungen. plist**" fest.

## <a name="using-an-nsuseractivity-shortcut-to-open-an-app"></a>Verwenden einer nsuseractivity-Verknüpfung zum Öffnen einer APP

Um eine Verknüpfung zu erstellen, mit der eine APP zum Anzeigen bestimmter Inhalte geöffnet `NSUserActivity` wird, erstellen Sie einen, und fügen Sie ihn an den Ansichts Controller für den Bildschirm an, den die Verknüpfung geöffnet werden soll.

### <a name="setting-up-an-nsuseractivity"></a>Einrichten von nsuseractivity

Auf dem Menü Bildschirm wird `SoupMenuViewController` ein `NSUserActivity` erstellt und der-Eigenschaft des Ansichts [`UserActivity`](xref:UIKit.UIResponder.UserActivity) Controllers zugewiesen:

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();
    UserActivity = NSUserActivityHelper.ViewMenuActivity;
}
```

Durch Festlegen `UserActivity` der-Eigenschaft wird die-Aktivität an Siri _gespendet_ . Aus dieser Spende erhält Siri Informationen dazu, wann und wo diese Aktivität für den Benutzer relevant ist, und lernt, Sie in Zukunft besser vorzuschlagen.

`NSUserActivityHelper`ist eine Hilfsprogrammklasse, die in der **soupchef** -Projekt **Mappe in der soupkit** -Klassenbibliothek enthalten ist. Es wird ein `NSUserActivity` erstellt, und es werden verschiedene Eigenschaften für Siri und Search festgelegt:

```csharp
public static string ViewMenuActivityType = "com.xamarin.SoupChef.viewMenu";

public static NSUserActivity ViewMenuActivity {
    get
    {
        var userActivity = new NSUserActivity(ViewMenuActivityType)
        {
            Title = NSBundleHelper.SoupKitBundle.GetLocalizedString("ORDER_LUNCH_TITLE", "View menu activity title"),
            EligibleForSearch = true,
            EligibleForPrediction = true
        };

        var attributes = new CSSearchableItemAttributeSet(NSUserActivityHelper.SearchableItemContentType)
        {
            ThumbnailData = UIImage.FromBundle("tomato").AsPNG(),
            Keywords = ViewMenuSearchableKeywords,
            DisplayName = NSBundleHelper.SoupKitBundle.GetLocalizedString("ORDER_LUNCH_TITLE", "View menu activity title"),
            ContentDescription = NSBundleHelper.SoupKitBundle.GetLocalizedString("VIEW_MENU_CONTENT_DESCRIPTION", "View menu content description")
        };
        userActivity.ContentAttributeSet = attributes;

        var phrase = NSBundleHelper.SoupKitBundle.GetLocalizedString("ORDER_LUNCH_SUGGESTED_PHRASE", "Voice shortcut suggested phrase");
        userActivity.SuggestedInvocationPhrase = phrase;
        return userActivity;
    }
}
```

Beachten Sie insbesondere Folgendes:

- Durch `EligibleForPrediction` festlegen `true` von auf wird angegeben, dass Siri diese Aktivität Vorhersagen und als Verknüpfung verwenden kann.
- Das [`ContentAttributeSet`](xref:Foundation.NSUserActivity.ContentAttributeSet) -Array ist ein [`CSSearchableItemAttributeSet`](xref:CoreSpotlight.CSSearchableItemAttributeSet) Standard, der zum `NSUserActivity` einschließen eines in ios-Suchergebnisse verwendet wird.
- [`SuggestedInvocationPhrase`](xref:Foundation.NSUserActivity.SuggestedInvocationPhrase)ist ein Ausdruck, den der Benutzer von Siri als mögliche Wahl vorschlägt, wenn er einer Verknüpfung einen Ausdruck zuweist.

### <a name="handling-an-nsuseractivity-shortcut"></a>Behandeln von nsuseractivity-Tastenkombinationen

Eine IOS- `NSUserActivity` Anwendung muss die `ContinueUserActivity` -Methode der `AppDelegate` -Klasse überschreiben, um auf Grundlage des `ActivityType` -Felds des übergebenen- `NSUserActivity` Objekts reagieren zu können, um eine von einem Benutzer aufgerufene Verknüpfung zu behandeln:

```csharp
public override bool ContinueUserActivity(UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    // ...
    else if (userActivity.ActivityType == NSUserActivityHelper.ViewMenuActivityType)
    {
        HandleUserActivity();
        return true;
    }
    // ...
}
```

Diese Methode ruft `HandleUserActivity`auf, wobei der Bildschirm auf dem Menü Bildschirm gefunden und aufgerufen wird:

```csharp
void HandleUserActivity()
{
    var rootViewController = Window?.RootViewController as UINavigationController;
    var orderHistoryViewController = rootViewController?.ViewControllers?.FirstOrDefault() as OrderHistoryTableViewController;
    if (orderHistoryViewController is null)
    {
        Console.WriteLine("Failed to access OrderHistoryTableViewController.");
        return;
    }
    var segue = OrderHistoryTableViewController.SegueIdentifiers.SoupMenu;
    orderHistoryViewController.PerformSegue(segue, null);
}
```

### <a name="assigning-a-phrase-to-an-nsuseractivity"></a>Zuweisen eines Ausdrucks zu einer nsuseractivity

Öffnen Sie zum Zuweisen eines Ausdrucks `NSUserActivity`zu einem die IOS- **Einstellungs** -APP, und wählen Sie **Siri & Search > meine**Verknüpfungen aus. Wählen Sie dann die Verknüpfung (in diesem Fall "Bestell Mittag") aus, und notieren Sie sich einen Ausdruck.

Wenn Siri aufgerufen wird und dieser Ausdruck verwendet wird, wird "Soup Chef" auf dem Menü Bildschirm geöffnet.

## <a name="using-a-custom-intent-shortcut-to-perform-a-task"></a>Verwenden einer benutzerdefinierten Intent-Verknüpfung zum Ausführen einer Aufgabe

### <a name="defining-a-custom-intent"></a>Definieren einer benutzerdefinierten Absicht

Erstellen Sie eine benutzerdefinierte Absicht, um eine Verknüpfung bereitzustellen, über die ein Benutzer schnell eine bestimmte Aufgabe für Ihre APP ausführen kann. Eine benutzerdefinierte Absicht stellt eine Aufgabe dar, die ein Benutzer möglicherweise durchführen möchte, die für diese Aufgabe relevanten Parameter und mögliche Antworten, die sich aus der Ausführung der Aufgabe ergeben. Abhängig davon, wie eine benutzerdefinierte Absicht definiert ist, kann der Aufruf der App entweder Ihre APP öffnen oder eine Hintergrundaufgabe ausführen.

Verwenden Sie Xcode 10 zum Erstellen benutzerdefinierter Intents. Im [soupchef-Repository](https://github.com/xamarin/ios-samples/tree/master/ios12/SoupChef)wird die benutzerdefinierte Absicht in **ordersoupintentcodegen**, einem Ziel-C-Projekt, definiert. Öffnen Sie dieses Projekt, und wählen Sie die **Intents. intentdefinition** -Datei im **Projekt Navigator** aus, um die Absicht **ordersoup** anzuzeigen.

Beachten Sie Folgendes:

- Die Absicht hat eine **Kategorie** der **Reihenfolge**. Es gibt verschiedene vordefinierte Kategorien, die für benutzerdefinierte Intents verwendet werden können. Wählen Sie die Option aus, die am ehesten mit der Aufgabe übereinstimmt, die Ihre benutzerdefinierte Absicht ermöglicht. Da es sich hierbei um eine APP für die pinbestellung handelt, verwendet **ordersousetent** die **Reihenfolge**.
- Das Kontrollkästchen **Bestätigung** gibt an, ob Siri vor dem Ausführen der Aufgabe bestätigt werden muss. Für die " **Order Soup** Intent" in Suppe Chef ist diese Option aktiviert, da der Benutzer einen Einkauf durchführen kann.
- Der **Parameter** Abschnitt der intentdefinition-Datei definiert die für eine Verknüpfung relevanten Parameter. Zum Platzieren einer Suppen Bestellung muss Suppe Chef den Typ der Suppe, seine Menge und alle zugehörigen Optionen kennen.
Jeder Parameter weist einen Typ auf. Parameter, der nicht von einem vordefinierten Typ dargestellt werden kann, werden als **Benutzer**definiert festgelegt.
- Die Schnittstelle für shortcutypen beschreibt die verschiedenen Parameterkombinationen, die Siri verwenden kann, wenn Sie die Verknüpfung vorschlagen. Mithilfe der zugehörigen **Titel** und unter **Titel** Abschnitte können Sie die Nachrichten definieren, die Siri beim präsentieren einer vorgeschlagenen Verknüpfung für den Benutzer verwendet.
- Das Kontrollkästchen **unterstützt die Hintergrund Ausführung** , sollte für alle Tastenkombinationen ausgewählt werden, die ausgeführt werden können, ohne dass die APP zur weiteren Benutzerinteraktion geöffnet wird.

### <a name="defining-custom-intent-responses"></a>Definieren von benutzerdefinierten Intent-Antworten

Das **Antwort** Element, das unterhalb der **ordersuppe** -Absicht eingefügt wird, stellt die möglichen Antworten dar, die sich aus einer Suppen Reihenfolge ergeben

Beachten Sie in der Antwort Definition von **ordersoup** Intent Folgendes:

- Die **Eigenschaften** einer Antwort können zum Anpassen der Nachricht verwendet werden, die an den Benutzer zurückgegeben wird. Die Anforderung " **ordersoup** Intent" hat die Eigenschaften " **Suppe** " und " **waittime** "
- Die **Antwort Vorlagen** geben die verschiedenen Erfolgs-und Fehlermeldungen an, die verwendet werden können, um den Status anzugeben, nachdem die Aufgabe einer Absicht abgeschlossen wurde.
- Das Kontrollkästchen **Erfolg** sollte für Antworten ausgewählt werden, die auf Erfolg hinweisen.
- Die **ordersoupintent** Success-Antwort verwendet die Eigenschaften " **Suppe** " und " **waittime** ", um eine benutzerfreundliche und nützliche Meldung zu erhalten, in der beschrieben wird, wann der Suppen Auftrag bereit

### <a name="generating-code-for-the-custom-intent"></a>Erstellen von Code für die benutzerdefinierte Absicht

Durch das Erstellen des Xcode-Projekts, das diese benutzerdefinierte Intent-Definition enthält, generiert Xcode Code, der zur programmgesteuerten Interaktion mit der benutzerdefinierten Absicht und den zugehörigen Antworten verwendet werden kann.

So zeigen Sie den generierten Code an:

- Öffnen Sie appdelegat **. m**.
- Fügen Sie der Header Datei der benutzerdefinierten Absicht einen Import hinzu:`#import "OrderSoupIntent.h"`
- Fügen Sie in einer beliebigen Methode in der-Klasse einen `OrderSoupIntent`Verweis auf hinzu.
- Klicken Sie mit der `OrderSoupIntent` rechten Maustaste auf, und wählen Sie **zu Definition springen**.
- Klicken Sie mit der rechten Maustaste in die neu geöffnete Datei **ordersoupintent. h**, und wählen Sie **in Finder anzeigen aus**.
- Dadurch wird ein **Finder** -Fenster geöffnet, das eine h-und m-Datei mit dem generierten Code enthält.

Dieser generierte Code umfasst Folgendes:

- `OrderSoupIntent`– Eine Klasse, die die benutzerdefinierte Absicht darstellt.
- `OrderSoupIntentHandling`– Ein Protokoll, mit dem die Methoden definiert werden, die verwendet werden, um zu bestätigen, dass die Absicht ausgeführt werden soll, und die Methode, die Sie ausführt.
- `OrderSoupIntentResponseCode`– Eine Aufzählung, die verschiedene Antwortstatus definiert.
- `OrderSoupIntentResponse`– eine Klasse, die die Antwort auf die Ausführung einer Absicht darstellt.

### <a name="creating-a-binding-to-the-custom-intent"></a>Erstellen einer Bindung an die benutzerdefinierte Absicht

Um den Code zu verwenden, der von Xcode in einer xamarin. IOS-APP C# generiert wird, erstellen Sie eine Bindung dafür.

#### <a name="creating-a-static-library-and-c-binding-definitions"></a>Erstellen einer statischen Bibliothek und C# Bindungs Definitionen

Sehen Sie sich im [soupchef-Repository](https://github.com/xamarin/ios-samples/tree/master/ios12/SoupChef)den Ordner **ordersoupintentstaticlib** an, und öffnen Sie das Xcode-Projekt **ordersoupintentstaticlib. xcodeproj** .

Dieses Projekt für eine **statische Cocoa** -Finger Bibliothek enthält die von Xcode generierten Dateien **ordersoupintent. h** und **ordersoupintent. m** .

#### <a name="configuring-the-static-library-project-build-settings"></a>Konfigurieren der Buildeinstellungen für das statische Bibliotheksprojekt

Wählen Sie im Xcode- **Projekt Navigator**das Projekt der obersten Ebene ( **ordersoupintentstaticlib**) aus, und navigieren Sie zu buildphasen **> Kompilieren von Quellen**.
Beachten Sie, dass **ordersoupintent. m** (das **ordersoupintent. h**importiert) hier aufgeführt ist. Beachten Sie in **Link Binary with Libraries**, dass **Intents. Framework** und **Foundation. Framework** enthalten sind.
Wenn diese Einstellungen vorhanden sind, wird das Framework ordnungsgemäß erstellt.

#### <a name="building-the-static-library-and-generating-c-bindings-definitions"></a>Erstellen der statischen Bibliothek und erstellen C# von Bindungs Definitionen

Führen Sie die folgenden Schritte aus, C# um die statische Bibliothek zu erstellen und Bindungs Definitionen dafür zu generieren:

- [Installieren Sie das Ziel Sharpie](https://docs.microsoft.com/xamarin/cross-platform/macios/binding/objective-sharpie/get-started?context=xamarin/mac#installing-objective-sharpie), das Tool, das zum Generieren von Bindungs Definitionen aus den von Xcode erstellten h-und m-Dateien verwendet wird.

- Konfigurieren Sie Ihr System für die Verwendung von Xcode 10-Befehlszeilen Tools:

  > [!WARNING]
  > Das Aktualisieren der ausgewählten Befehlszeilen Tools wirkt sich auf alle installierten Versionen von Xcode auf dem System aus. Wenn Sie die Beispiel-app "Soup Chef" verwendet haben, stellen Sie sicher, dass diese Einstellung auf die ursprüngliche Konfiguration zurückgesetzt wird.

  - Wählen Sie in Xcode die Option **Xcode > Einstellungen > Speicherorte** aus, und legen Sie **Befehlszeilen Tools** auf die aktuelle auf Ihrem System verfügbare Xcode 10-Installation fest.

- Im Terminal `cd` zum Verzeichnis **ordersoupintentstaticlib** .

- Typ `make`, der erstellt:

  - Die statische Bibliothek, **libordersoupintentstaticlib. a**
  - Im Verzeichnis " **Bo** Output" C# sind Bindungen Definitionen:
    - **ApiDefinitions.cs**
    - **StructsAndEnums.cs**

Das **ordersoupintentbinding** -Projekt, das von dieser statischen Bibliothek und den zugehörigen Bindungs Definitionen abhängig ist, erstellt diese Elemente automatisch.
Wenn Sie den obigen Prozess jedoch manuell ausführen, stellen Sie sicher, dass er erwartungsgemäß erstellt wird.

Weitere Informationen zum Erstellen einer statischen Bibliothek und zum Erstellen C# von Bindungs Definitionen mithilfe von Ziel-Sharpie finden Sie in der exemplarischen Vorgehensweise zum [Binden einer IOS-Ziel-C-Bibliothek](https://docs.microsoft.com/xamarin/ios/platform/binding-objective-c/walkthrough?tabs=vsmac#creating-a-static-library) .

#### <a name="creating-a-bindings-library"></a>Erstellen einer Bindungs Bibliothek

Wenn die statische Bibliothek und die C# Bindungs Definitionen erstellt wurden, ist das verbleibende Teil, das erforderlich ist, um den von Xcode generierten Intent-bezogenen Code in einem xamarin. IOS-Projekt zu verarbeiten, eine Bindungs Bibliothek.

Öffnen Sie im " [Soup Chef](https://github.com/xamarin/ios-samples/tree/master/ios12/SoupChef)"-Repository die Datei " **soupchef. sln** ". Diese Lösung enthält unter anderem **ordersoupintentbinding**, eine Bindungs Bibliothek für die oben generierte statische Bibliothek.

Beachten Sie insbesondere, dass dieses Projekt Folgendes umfasst:

- **ApiDefinitions.cs** – eine Datei, die vom Ziel-Sharpie generiert und diesem Projekt hinzugefügt wurde. Die Buildaktion dieser Datei ist auf **objcbindingapidefinition**festgelegt.
- **StructsAndEnums.cs** – eine andere Datei, die vom Ziel-Sharpie generiert und diesem Projekt hinzugefügt wurde. Die Buildaktion dieser Datei ist auf **objcbindingcoresource**festgelegt.
- Ein **nativer Verweis** auf **libordersoupintentstaticlib. a**, der oben erstellten statischen Bibliothek.

> [!NOTE]
> Sowohl **ApiDefinitions.cs** als auch **StructsAndEnums.cs** enthalten `[Watch (5,0), iOS (12,0)]`Attribute wie z. b. Diese Attribute, die vom Ziel-Sharpie generiert wurden, wurden auskommentiert, da Sie für dieses Projekt nicht erforderlich sind.

Weitere Informationen zum Erstellen einer C# Bindungs Bibliothek finden Sie in der exemplarischen Vorgehensweise zum [Binden einer IOS-Ziel-C-Bibliothek](https://docs.microsoft.com/xamarin/ios/platform/binding-objective-c/walkthrough?tabs=vsmac#create-a-xamarinios-binding-project) .

Beachten Sie, dass das Projekt **soupchef** einen Verweis auf **ordersoupintentbinding**enthält, was bedeutet, dass es nun, C#in, die in ihm enthaltenen Klassen, Schnittstellen und Enumerationsmethoden aufrufen kann:

- `OrderSoupIntent`
- `OrderSoupIntentHandling`
- `OrderSoupIntentResponse`
- `OrderSoupIntenseResponseCode`

### <a name="adding-the-intentdefinition-file-to-your-solution"></a>Hinzufügen der intentdefinition-Datei zur Projekt Mappe

In der C# **soupchef** -Projekt Mappe enthält das Projekt **soupkit** den Code, der von der APP und seinen Erweiterungen gemeinsam genutzt wird. Die **Intents. intentdefinition** -Datei wurde im **base. lproj** -Verzeichnis von **soupkit**abgelegt und enthält eine Buildaktion von **Content**. Der Buildprozess kopiert diese Datei in den App Bundle der Suppe Chef, wo Sie erforderlich ist, damit die APP ordnungsgemäß funktioniert.

### <a name="donating-an-intent"></a>Spenden einer Absicht

Damit Siri eine Verknüpfung vorschlagen kann, muss es zuerst verstanden werden, wenn die Verknüpfung relevant ist.

Um Siri dieses Verständnis zu verschaffen, _spendet_ Soup Chef immer dann, wenn der Benutzer eine Suppe Reihenfolge anlegt, eine Absicht an Siri. Basierend auf dieser Spende – bei der Spende, wo Sie gespendet wurde, erfährt der in ihr enthaltene Parameter – Siri, wann die Verknüpfung in Zukunft vorgeschlagen wird.

**Soupchef** verwendet die `SoupOrderDataManager` -Klasse, um Spenden zu platzieren.
Wenn die Methode aufgerufen wird, um eine Suppe für einen Benutzer `PlaceOrder` zu platzieren, ruft [`DonateInteraction`](xref:Intents.INInteraction.DonateInteraction*)die Methode wiederum Folgendes auf:

```csharp
void DonateInteraction(Order order)
{
    var interaction = new INInteraction(order.Intent, null);
    interaction.Identifier = order.Identifier.ToString();
    interaction.DonateInteraction((error) =>
    {
        // ...
    });
}
```

Nach dem Abrufen einer Absicht wird Sie in einem [`INInteraction`](xref:Intents.INInteraction)umschließt.
Der `INInteraction` wird ein[`Identifier`](xref:Intents.INInteraction.Identifier*)
, der der eindeutigen ID der Bestellung entspricht (Dies ist später hilfreich, wenn beabsichtigte Spenden gelöscht werden, die nicht mehr gültig sind). Anschließend wird die Interaktion an Siri gespendet.

Der Aufruf des `order.Intent` Getters ruft `Options`ein `OrderSoupIntent` ab, das die `Soup`Reihenfolge durch Festlegen `Quantity`seines-,-,-und-Bilds darstellt, sowie einen Aufruf Ausdruck, der als Vorschlag verwendet werden soll, wenn der Benutzer einen Ausdruck für die Zuordnung von Siri aufzeichnet. mit der Absicht:

```csharp
public OrderSoupIntent Intent
{
    get
    {
        var orderSoupIntent = new OrderSoupIntent();
        orderSoupIntent.Quantity = new NSNumber(Quantity);
        orderSoupIntent.Soup = new INObject(MenuItem.ItemNameKey, MenuItem.LocalizedString);

        var image = UIImage.FromBundle(MenuItem.IconImageName);
        if (!(image is null))
        {
            var data = image.AsPNG();
            orderSoupIntent.SetImage(INImage.FromData(data), "soup");
        }

        orderSoupIntent.Options = MenuItemOptions
            .ToArray<MenuItemOption>()
            .Select<MenuItemOption, INObject>(arg => new INObject(arg.Value, arg.LocalizedString))
            .ToArray<INObject>();

        var comment = "Suggested phrase for ordering a specific soup";
        var phrase = NSBundleHelper.SoupKitBundle.GetLocalizedString("ORDER_SOUP_SUGGESTED_PHRASE", comment);
        orderSoupIntent.SuggestedInvocationPhrase = String.Format(phrase, MenuItem.LocalizedString);

        return orderSoupIntent;
    }
}
```

#### <a name="removing-invalid-donations"></a>Entfernen ungültiger Spenden

Es ist wichtig, ungültige Spenden zu entfernen, sodass Siri keine hilfreichen oder verwirrenden Vorschläge für die Verknüpfung macht.

In Suppe Chef kann der **Menübefehl konfigurieren** verwendet werden, um ein Menü Element als nicht verfügbar zu markieren. Siri sollte keine Verknüpfung mehr vorschlagen, um ein nicht verfügbares Menü Element zu sortieren `RemoveDonation` . daher `SoupMenuManager` löscht die Methode von das Löschen von Spenden für Menü Elemente, die nicht mehr verfügbar sind. Dies geschieht wie folgt:

- Suchen nach Aufträgen, die dem Menü Element "jetzt nicht verfügbar" zugeordnet sind.
- Ihre Bezeichner werden gepackt.
- Das Löschen von Interaktionen mit dem gleichen Bezeichner.

```csharp
void RemoveDonation(MenuItem menuItem)
{
    if (!menuItem.IsAvailable)
    {
        Order[] orderHistory = OrderManager?.OrderHistory.ToArray<Order>();
        if (orderHistory is null)
        {
            return;
        }

        string[] orderIdentifiersToRemove = orderHistory
            .Where<Order>((order) => order.MenuItem.ItemNameKey == menuItem.ItemNameKey)
            .Select<Order, string>((order) => order.Identifier.ToString())
            .ToArray<string>();

        INInteraction.DeleteInteractions(orderIdentifiersToRemove, (error) =>
        {
            if (!(error is null))
            {
                Console.WriteLine($"Failed to delete interactions with error {error.ToString()}");
            }
            else
            {
                Console.WriteLine("Successfully deleted interactions");
            }
        });
    }
}
```

### <a name="creating-an-intents-extension"></a>Erstellen einer Intents-Erweiterung

Der Code, der ausgeführt wird, wenn SIRI eine Absicht aufruft, wird in eine Intents-Erweiterung eingefügt, die der gleichen Projekt Mappe wie eine vorhandene xamarin. IOS-APP, z. b. "Suppe Chef", als neues Projekt hinzugefügt werden kann. In der **soupchef** -Projekt Mappe heißt die Erweiterung **soupchefintents**.

#### <a name="soupchefintents-infoplist-and-entitlementsplist"></a>Soupchefintents – Info. plist und Berechtigungen. plist

##### <a name="soupchefintents-infoplist"></a>Soupchefintents – Info. plist

Die Datei " **Info. plist** " im Projekt " **soupchefintents** " definiert `com.xamarin.SoupChef.SoupChefIntents`den **Bündel Bezeichner** als.

Die Datei " **Info. plist** " enthält außerdem Folgendes:

```xml
<key>NSExtension</key>
<dict>
    <key>NSExtensionAttributes</key>
    <dict>
        <key>IntentsRestrictedWhileLocked</key>
        <array/>
        <key>IntentsSupported</key>
        <array>
            <string>OrderSoupIntent</string>
        </array>
        <key>IntentsRestrictedWhileProtectedDataUnavailable</key>
        <array/>
    </dict>
    <key>NSExtensionPointIdentifier</key>
    <string>com.apple.intents-service</string>
    <key>NSExtensionPrincipalClass</key>
    <string>IntentHandler</string>
</dict>
```

In der obigen **Info. plist**:

- `IntentsRestrictedWhileLocked`Listet Intents auf, die nur behandelt werden sollen, wenn das Gerät entsperrt ist.
- `IntentsSupported`Listet die Intents auf, die von dieser Erweiterung behandelt werden.
- `NSExtensionPointIdentifier`Gibt den Typ der APP-Erweiterung an (Weitere Informationen finden Sie in [der Apple-Dokumentation](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/AppExtensionKeys.html#//apple_ref/doc/uid/TP40014212-SW15) ).
- `NSExtensionPrincipalClass`Gibt die Klasse an, die zur Verarbeitung von Intents verwendet werden soll, die von dieser Erweiterung unterstützt werden.

##### <a name="soupchefintents-entitlementsplist"></a>Soupchefintents – Berechtigungen. plist

Die "Berechtigungs **. plist** " im Projekt " **soupchefintents** " verfügt über die Funktionen der **App-Gruppen** . Diese Funktion ist für die Verwendung der gleichen App-Gruppe wie für das Projekt " **soupchef** " konfiguriert:

```xml
<key>com.apple.security.application-groups</key>
<array>
    <string>group.com.xamarin.SoupChef</string>
</array>
```

Der Suppe Chef speichert Daten `NSUserDefaults`mit. Um Daten zwischen der APP und der APP-Erweiterung gemeinsam zu nutzen, verweisen Sie in Ihren **Berechtigungen. plist** -Dateien auf dieselbe App-Gruppe.

> [!NOTE]
> Die Buildkonfiguration des Projekts **soupchefintents** legt **benutzerdefinierte Berechtigungen** auf "Berechtigungs **. plist**" fest.

#### <a name="handling-an-ordersoupintent-background-task"></a>Behandeln einer ordersoupintent-Hintergrundaufgabe

Eine Intents-Erweiterung führt die erforderlichen Hintergrundaufgaben für eine Verknüpfung auf der Grundlage einer benutzerdefinierten Absicht aus.

Siri Ruft die [`GetHandler`](xref:Intents.INExtension.GetHandler*) -Methode `IntentHandler` der-Klasse (definiert in " `NSExtensionPrincipalClass` **Info. plist** " als) auf, um eine Instanz einer Klasse `OrderSoupIntentHandling`zu erhalten, die erweitert, die zum `OrderSoupIntent`Verarbeiten eines verwendet werden kann:

```csharp
[Register("IntentHandler")]
public class IntentHandler : INExtension
{
    public override NSObject GetHandler(INIntent intent)
    {
        if (intent is OrderSoupIntent)
        {
            return new OrderSoupIntentHandler();
        }
        throw new Exception("Unhandled intent type: ${intent}");
    }

    protected IntentHandler(IntPtr handle) : base(handle) { }
}
```

`OrderSoupIntentHandler`, definiert im **soupkit** -Projekt von frei gegebenem Code, implementiert zwei wichtige Methoden:

- `ConfirmOrderSoup`– Bestätigt, ob die Aufgabe, die der Absicht zugeordnet ist, tatsächlich ausgeführt werden soll.
- `HandleOrderSoup`– Platziert die Suppen Reihenfolge und antwortet den Benutzer durch Aufrufen des bestandenen Vervollständigungs Handlers.

#### <a name="handling-an-ordersoupintent-that-opens-the-app"></a>Verarbeiten eines ordersoupintent, das die APP öffnet

Eine APP muss die Intents ordnungsgemäß verarbeiten, die nicht im Hintergrund ausgeführt werden.
Diese werden auf die gleiche Weise wie `NSUserActivity` Verknüpfungen in der `ContinueUserActivity` -Methode von `AppDelegate`behandelt:

```csharp
public override bool ContinueUserActivity(UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    var intent = userActivity.GetInteraction()?.Intent as OrderSoupIntent;
    if (!(intent is null))
    {
        HandleIntent(intent);
        return true;
    }
    // ...
}  
```

## <a name="providing-a-user-interface-for-a-custom-intent"></a>Bereitstellen einer Benutzeroberfläche für eine benutzerdefinierte Absicht

Eine Intents-Benutzeroberflächen Erweiterung bietet eine benutzerdefinierte Benutzeroberfläche für eine Intents-Erweiterung. In der **soupchef** -Lösung ist **soupchefintentsui** eine Intents-Benutzeroberflächen Erweiterung, die eine Schnittstelle für **soupchefintents**bereitstellt.

### <a name="soupchefintentsui--infoplist-and-entitlementsplist"></a>Soupchefintentsui – Info. plist und Berechtigungen. plist

#### <a name="soupchefintentsui-infoplist"></a>Soupchefintentsui – Info. plist

Die Datei " **Info. plist** " im Projekt " **soupchefintentsui** " definiert `com.xamarin.SoupChef.SoupChefIntentsui`den **Bündel Bezeichner** als.

Die Datei " **Info. plist** " enthält außerdem Folgendes:

```xml
<key>NSExtension</key>
<dict>
    <key>NSExtensionAttributes</key>
    <dict>
        <key>IntentsSupported</key>
        <array>
            <string>OrderSoupIntent</string>
        </array>
        <!-- ... -->
    </dict>
    <key>NSExtensionPointIdentifier</key>
    <string>com.apple.intents-ui-service</string>
    <key>NSExtensionMainStoryboard</key>
    <string>MainInterface</string>
</dict>
```

In der obigen **Info. plist**:

- `IntentsSupported`Gibt an, `OrderSoupIntent` dass die von dieser Intents-Benutzeroberflächen Erweiterung verarbeitet wird.
- `NSExtensionPointIdentifier`Gibt den Typ der APP-Erweiterung an (Weitere Informationen finden Sie in [der Apple-Dokumentation](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/AppExtensionKeys.html#//apple_ref/doc/uid/TP40014212-SW15) ).
- `NSExtensionMainStoryboard`Gibt das Storyboard an, das die primäre Schnittstelle dieser Erweiterung definiert.

#### <a name="soupchefintentsui-entitlementsplist"></a>Soupchefintentsui – Berechtigungen. plist

Das Projekt **soupchefintentsui** benötigt keine Datei " **Berechtigungen. plist** ".

### <a name="creating-the-user-interface"></a>Erstellen der Benutzeroberfläche

Da in der Datei " **Info. plist** " für **soupchefintentsui** der `NSExtensionMainStoryboard` Schlüssel auf festgelegt ist, wird in `MainInterface`der Datei " **maininterace. Storyboard** " die Schnittstelle für die Intents-Benutzeroberflächen Erweiterung

In diesem Storyboard gibt es einen einzelnen Ansichts Controller des Typs **intentviewcontroller**. Es verweist auf zwei Ansichten:

- **invoiceview**, Typ`InvoiceView`
- **confirmationview**vom Typ`ConfirmOrderView`

> [!NOTE]
> Die Schnittstellen für **invoiceview** und **confirmationview** werden in **Main. Storyboard** als sekundäre Sichten definiert. Der IOS-Designer in Visual Studio für Mac und Visual Studio 2017 bietet keine Unterstützung für das Anzeigen oder bearbeiten sekundärer Sichten. Öffnen Sie hierzu " **Main. Storyboard** " in der Interface Builder von Xcode.

`IntentViewController`implementiert das[`IINUIHostedViewControlling`](xref:IntentsUI.IINUIHostedViewControlling)
Schnittstelle, die verwendet wird, um eine benutzerdefinierte Schnittstelle beim Arbeiten mit Siri Intents bereitzustellen. Die[`ConfigureView`](xref:IntentsUI.INUIHostedViewControlling_Extensions.ConfigureView*)
die-Methode wird aufgerufen, um die-Schnittstelle anzupassen. dabei wird die Bestätigung oder die Rechnung angezeigt, abhängig davon[`INIntentHandlingStatus.Ready`](xref:Intents.INIntentHandlingStatus), ob die Interaktion bestätigt wird ([`INIntentHandlingStatus.Success`](xref:Intents.INIntentHandlingStatus)) oder erfolgreich ausgeführt wurde ():

```csharp
[Export("configureViewForParameters:ofInteraction:interactiveBehavior:context:completion:")]
public void ConfigureView(
    NSSet<INParameter> parameters,
    INInteraction interaction,
    INUIInteractiveBehavior interactiveBehavior,
    INUIHostedViewContext context,
    INUIHostedViewControllingConfigureViewHandler completion)
{
    // ...
    if (interaction.IntentHandlingStatus == INIntentHandlingStatus.Ready)
    {
        desiredSize = DisplayInvoice(order, intent);
    }
    else if(interaction.IntentHandlingStatus == INIntentHandlingStatus.Success)
    {
        var response = interaction.IntentResponse as OrderSoupIntentResponse;
        if (!(response is null))
        {
            desiredSize = DisplayOrderConfirmation(order, intent, response);
        }
    }
    completion(true, parameters, desiredSize);
}
```

> [!TIP]
> Weitere Informationen zur- `ConfigureView` Methode finden Sie in der WWDC 2017-Präsentation von Apple, [What es New in Sirikit](https://developer.apple.com/videos/play/wwdc2017/214/).

## <a name="creating-a-voice-shortcut"></a>Erstellen einer sprach Verknüpfung

Soup Chef bietet eine Schnittstelle, mit der jeder Bestellung eine sprach Verknüpfung zugewiesen werden kann, sodass Sie eine Suppe mit Siri bestellen können. Tatsächlich wird die Schnittstelle, mit der sprach Verknüpfungen aufgezeichnet und zugewiesen werden, von IOS bereitgestellt, und es ist wenig benutzerdefinierter Code erforderlich.

Wenn `OrderDetailViewController`ein Benutzer in die Zeile in Siri der Tabelle **Hinzufügen** tippt, stellt [`RowSelected`](xref:UIKit.UITableViewSource.RowSelected*) die-Methode einen Bildschirm zum Hinzufügen oder Bearbeiten einer sprach Verknüpfung dar:

```csharp
public override void RowSelected(UITableView tableView, NSIndexPath indexPath)
{
    // ...
    else if (TableConfiguration.Sections[indexPath.Section].Type == OrderDetailTableConfiguration.SectionType.VoiceShortcut)
    {
        INVoiceShortcut existingShortcut = VoiceShortcutDataManager?.VoiceShortcutForOrder(Order);
        if (!(existingShortcut is null))
        {
            var editVoiceShortcutViewController = new INUIEditVoiceShortcutViewController(existingShortcut);
            editVoiceShortcutViewController.Delegate = this;
            PresentViewController(editVoiceShortcutViewController, true, null);
        }
        else
        {
            // Since the app isn't yet managing a voice shortcut for
            // this order, present the add view controller
            INShortcut newShortcut = new INShortcut(Order.Intent);
            if (!(newShortcut is null))
            {
                var addVoiceShortcutVC = new INUIAddVoiceShortcutViewController(newShortcut);
                addVoiceShortcutVC.Delegate = this;
                PresentViewController(addVoiceShortcutVC, true, null);
            }
        }
    }
}
```

Je nachdem, ob eine vorhandene sprach Verknüpfung für die aktuell angezeigte Reihenfolge vorhanden ist `RowSelected` , zeigt einen Ansichts Controller [`INUIEditVoiceShortcutViewController`](xref:IntentsUI.INUIEditVoiceShortcutViewController) vom [`INUIAddVoiceShortcutViewController`](xref:IntentsUI.INUIAddVoiceShortcutViewController)Typ oder an.
In jedem Fall `OrderDetailViewController` wird von selbst als der Ansichts `Delegate`Controller festgelegt. aus diesem Grund implementiert es auch[`IINUIAddVoiceShortcutViewControllerDelegate`](xref:IntentsUI.IINUIAddVoiceShortcutViewControllerDelegate)
und [`IINUIEditVoiceShortcutViewControllerDelegate`](xref:IntentsUI.IINUIEditVoiceShortcutViewControllerDelegate).

## <a name="testing-on-device"></a>Testen auf Gerät

Befolgen Sie die Anweisungen unten, um die Suppe Chef auf einem Gerät auszuführen. Lesen Sie auch den [Hinweis zur automatischen](#automatic-provisioning)Bereitstellung.

### <a name="app-group-app-ids-provisioning-profiles"></a>App-Gruppe, App-IDs, Bereitstellungs profile

Gehen Sie im Abschnitt **Zertifikate, IDs & profile** des [Apple Developer Portals](https://developer.apple.com/)wie folgt vor:

- Erstellen Sie eine APP-Gruppe, um Daten zwischen der Suppe Chef-APP und ihren Erweiterungen gemeinsam zu nutzen. Beispiel: **Group. com. YourCompanyName. soupchef**

- Erstellen Sie drei App-IDs: eine für die APP selbst, eine für die Intents-Erweiterung und eine für die Intents-Benutzeroberflächen Erweiterung. Beispiel:

  - App: **com.yourcompanyname.SoupChef**
    - Weisen Sie dieser APP-ID die Funktionen "Sirikit" und " **App Groups** " zu.

  - Intents-Erweiterung: **com. YourCompanyName. soupchef. Intents**
    - Weisen Sie dieser APP-ID die Funktion **App-Gruppen** zu.

  - Intents-Benutzeroberflächen Erweiterung: **com. YourCompanyName. soupchef. intentsui**
    - Diese APP-ID benötigt keine speziellen Funktionen.

- Nachdem Sie die oben genannten App-IDs erstellt haben, bearbeiten Sie die Funktionen der APP- **Gruppen** , die der APP und der Intents-Erweiterung zugewiesen sind, und geben die oben erstellte Anwendungs Gruppe an

- Erstellen Sie drei neue Entwicklungs Bereitstellungs Profile, eine für jede der neuen App-IDs.

- Laden Sie diese Bereitstellungs profile herunter, und doppelklicken Sie darauf, um Sie zu installieren. Wenn Visual Studio für Mac oder Visual Studio 2017 bereits ausgeführt wird, starten Sie ihn neu, um sicherzustellen, dass die neuen Bereitstellungs profile registriert werden.

### <a name="editing-infoplist-entitlementsplist-and-source-code"></a>Bearbeiten von "Info. plist", "Berechtigungen. plist" und Quellcode

Führen Sie in Visual Studio für Mac oder Visual Studio 2017 die folgenden Schritte aus:

- Aktualisieren Sie die verschiedenen **Info. plist** -Dateien in der Projekt Mappe. Legen Sie die Anwendungs-, Intents-Erweiterung und Intents-UI-Erweiterungs **Bündel** -ID auf die oben definierten App-IDs fest:

  - App: **com.yourcompanyname.SoupChef**
  - Intents-Erweiterung: **com. YourCompanyName. soupchef. Intents**
  - Intents-Benutzeroberflächen Erweiterung: **com. YourCompanyName. soupchef. intentsui**

- Aktualisieren Sie die Datei " **Berechtigungen. plist** " für das Projekt " **soupchef** ":
  - Legen Sie für die Funktion **App-Gruppen** die Gruppe auf die neue APP-Gruppe fest, die oben erstellt wurde (im obigen Beispiel war Sie **Group. com. YourCompanyName. soupchef**).
  - Stellen Sie sicher, dass das **Sirikit** aktiviert ist.

- Aktualisieren Sie die Datei " **Berechtigungen. plist** " für das Projekt " **soupchefintents** ":
  - Legen Sie für die Funktion **App-Gruppen** die Gruppe auf die neue APP-Gruppe fest, die oben erstellt wurde (im obigen Beispiel war Sie **Group. com. YourCompanyName. soupchef**).

- Öffnen Sie abschließend **NSUserDefaultsHelper.cs**. Legen Sie `AppGroup` die Variable auf den Wert ihrer neuen App-Gruppe fest (legen Sie diese z `group.com.yourcompanyname.SoupChef`. b. auf fest).

### <a name="configuring-the-build-settings"></a>Konfigurieren der Buildeinstellungen

In Visual Studio für Mac oder Visual Studio 2017:

- Öffnen Sie die Optionen/Eigenschaften für das Projekt **soupchef** . Legen Sie auf der Registerkarte **IOS-Bündel Signierung** die Einstellung **Identität** automatisch und **Bereitstellungs Profil** auf das neue APP-spezifische Bereitstellungs Profil fest, das Sie oben erstellt haben.

- Öffnen Sie die Optionen/Eigenschaften für das Projekt **soupchefintents** . Legen Sie auf der Registerkarte **IOS-Bündel Signierung** die Einstellung **Identität** automatisch und **Bereitstellungs Profil** auf das neue Intents-Erweiterungs spezifische Bereitstellungs Profil fest, das Sie oben erstellt haben.

- Öffnen Sie die Optionen/Eigenschaften für das Projekt **soupchefintentsui** . Legen Sie auf der Registerkarte **IOS-Bündel Signierung** die Einstellung **Identität** automatisch und **Bereitstellungs Profil** auf das neue Intents UI Extension-spezifische Bereitstellungs Profil fest, das Sie oben erstellt haben.

Wenn diese Änderungen vorhanden sind, wird die APP auf einem IOS-Gerät ausgeführt.

### <a name="automatic-provisioning"></a>Automatische Bereitstellung

Beachten Sie, dass Sie die [Automatische](https://docs.microsoft.com/xamarin/ios/get-started/installation/device-provisioning/automatic-provisioning) Bereitstellung verwenden können, um viele dieser Bereitstellungs Aufgaben direkt in der IDE auszuführen.
Bei der automatischen Bereitstellung werden jedoch keine app-Gruppen eingerichtet. Sie müssen die Dateien mit den **Berechtigungen. plist** manuell mit dem Namen der APP-Gruppe konfigurieren, die Sie verwenden möchten. besuchen Sie das Apple Developer Portal, um die APP-Gruppe zu erstellen, und weisen Sie diese APP-Gruppe Jeder APP-ID zu, die durch die automatische Bereitstellung erstellt wurde. die Bereitstellungs profile (app-, Intents-Erweiterung, Intents-Benutzeroberflächen Erweiterung) zum Einbeziehen der neu erstellten App-Gruppe sowie zum herunterladen und Installieren der app.

## <a name="related-links"></a>Verwandte Links

- [Suppe Chef (xamarin)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-soupchef)
- [Suppe Chef (SWIFT)](https://developer.apple.com/documentation/sirikit/accelerating_app_interactions_with_shortcuts?language=objc)
- [Sirikit (Apple)](https://developer.apple.com/sirikit/)
- [Einführung in Siri-Verknüpfungen – WWDC 2018](https://developer.apple.com/videos/play/wwdc2018/211/)
- [Entwickeln für Stimme mit Siri-Verknüpfungen – WWDC 2018](https://developer.apple.com/videos/play/wwdc2018/214/)
- [Siri-Verknüpfungen auf dem Siri-Überwachungs Gesicht – WWDC 2018](https://developer.apple.com/videos/play/wwdc2018/217/)
- [Neues in Sirikit – WWDC 2017](https://developer.apple.com/videos/play/wwdc2017/214/)
- [Erstellen einer Intents-App-Erweiterung (Apple)](https://developer.apple.com/documentation/sirikit/creating_an_intents_app_extension?language=objc)
