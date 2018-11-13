---
title: Siri-Verknüpfungen in Xamarin.iOS
description: Dieses Dokument beschreibt die Verwendung von Siri-Verknüpfungen in iOS 12. Es wird erläutert, NSUserActivities, benutzerdefinierte Prioritäten zuweisen von Voice-Verknüpfungen, relevanten Verknüpfungen und mehr.
ms.prod: xamarin
ms.assetid: 86424F79-3A7D-436E-927D-9A3267DA333B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/08/2018
ms.openlocfilehash: f9034799355d01a3ade20a78540d6ecac43d9cc8
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2018
ms.locfileid: "51526896"
---
# <a name="siri-shortcuts-in-xamarinios"></a>Siri-Verknüpfungen in Xamarin.iOS

In [iOS 10](~/ios/platform/sirikit/index.md), Apple führte SiriKit, sodass sie Build-messaging, VoIP-Anruf, Zahlungen, fitnessaktivitäten, zeigt der Buchung und Foto-apps suchen, die mit Siri interagieren.

In [iOS 11](~/ios/platform/introduction-to-ios11/sirikit.md), SiriKit gewonnen, Unterstützung für mehrere Typen von apps und mehr Flexibilität zur Anpassung der Benutzeroberfläche.

iOS 12 fügt Siri-Verknüpfungen, sodass alle Arten von apps, deren Funktionalität siri verfügbar zu machen. Siri lernt, wenn bestimmte app-basierten Aufgaben am wichtigsten sind partitionsverarbeitungsoptionen für den Benutzer, und anhand dieses Wissens zum Vorschlagen von möglichen Aktionen über _Verknüpfungen_. Tippen auf eine Verknüpfung oder durch einen Sprachbefehl aufrufen wird Öffnen einer app oder eine Hintergrundaufgabe ausgeführt.

Verknüpfungen sollte verwendet werden, um die Fähigkeit eines Benutzers zum Ausführen einer Aufgabe – in vielen Fällen ohne die betreffende Anwendung zu öffnen zu beschleunigen.

## <a name="sample-app-soup-chef"></a>Beispiel-app: mehr Licht bei Chef

Um Tastenkombinationen für Siri besser zu verstehen, sehen Sie sich die [mehr Licht bei Chef](https://developer.xamarin.com/samples/monotouch/ios12/SoupChef/) Beispiel-app. Mehr Licht bei Chef können Benutzer Bestellungen von einer imaginären mehr Licht bei Restaurant, ihren Bestellverlauf anzeigen und Ausdrücke zu verwenden, wenn mehr Licht bei Sortierung durch die Interaktion mit Siri zu definieren.

> [!TIP]
> Aktivieren Sie vor dem Testen mehr Licht bei Chef auf einem iOS-12-Simulator oder ein Gerät die folgenden beiden Einstellungen, die hilfreich, beim Debuggen von Verknüpfungen sind:
>
> - In der **Einstellungen** -app aktivieren **Developer > aktuellen Tastenkombinationen anzeigen**.
> - In der **Einstellungen** -app aktivieren **Developer > Anzeige Spenden auf Sperrbildschirm**.
>
> Diese Einstellungen stellen es leicht auffindbar kürzlich erstellten Debuggen (anstelle von vorhergesagter) Verknüpfungen auf dem iOS-Sperrbildschirm und Suchbildschirm.

So verwenden Sie die Beispiel-app

- Installieren und Ausführen des Beispiel-app auf einem iOS-12-Simulator für die mehr Licht bei Chef oder [Gerät](#testing-on-device).
- Klicken Sie auf die **+** -Schaltfläche in der rechten oberen Ecke um einen neuen Auftrag zu erstellen.
- Wählen Sie einen Typ, der mehr Licht bei, geben eine Menge und Optionen, und tippen Sie auf **Bestellung aufgeben**.
- Auf der **Bestellverlauf** Bildschirm, tippen Sie auf den neu erstellten Auftrag um seine Details anzuzeigen.
- Tippen Sie am unteren Rand der Order Details-Bildschirm, auf **siri hinzufügen**.
- Zeichnen Sie einen Voice-Ausdruck, um die Reihenfolge zugeordnet, und tippen Sie auf **Fertig**.
- Minimieren Sie mehr Licht bei Chef zu, rufen Sie Siri und platzieren Sie den Auftrag erneut aus, durch die Verwendung des Sprach-Ausdrucks, die Sie gerade notiert haben.
- Nach der Reihenfolge von Siri abgeschlossen ist, mehr Licht bei Chef erneut zu öffnen, und beachten Sie, dass die neue Reihenfolge aufgeführt ist, auf die **Bestellverlauf** Bildschirm.

Die Beispiel-app veranschaulicht, wie Sie:

- [Verwenden Sie eine NSUserActivity-Verknüpfung zum Öffnen einer app](#using-an-nsuseractivity-shortcut-to-open-an-app).
- [Verwenden Sie eine benutzerdefinierte beabsichtigte Verknüpfung zum Ausführen einer Aufgabe](#using-a-custom-intent-shortcut-to-perform-a-task).
- [Geben Sie eine Benutzeroberfläche für eine benutzerdefinierte Absicht](#providing-a-user-interface-for-a-custom-intent).
- [Erstellen Sie eine Verknüpfung Voice](#creating-a-voice-shortcut).

## <a name="infoplist-and-entitlementsplist"></a>Datei "Info.plist" und "Entitlements.plist"

Bevor ausführlich erläutere besser, den Code mehr Licht bei Chef, sehen Sie sich die **"Info.plist"** und **"Entitlements.plist"** Dateien.

### <a name="infoplist"></a>Info.plist

Die **"Info.plist"** Datei die **SoupChef** Projekt definiert die **Bündel-ID** als `com.xamarin.SoupChef`. Diese Bündel-ID wird als Präfix verwendet werden, für die Bundle-Bezeichner des Intents und Intents UI Erweiterungen, die weiter unten in diesem Dokument.

Die **"Info.plist"** -Datei enthält außerdem die folgenden:

```xml
<key>NSUserActivityTypes</key>
<array>
    <string>OrderSoupIntent</string>
    <string>com.xamarin.SoupChef.viewMenu</string>
</array>
```

Dies `NSUserActivityTypes` Schlüssel/Wert-Paar gibt an, dass mehr Licht bei Chef behandeln kann ein `OrderSoupIntent`, und ein [ `NSUserActivity` ](https://developer.xamarin.com/api/type/Foundation.NSUserActivity/) müssen eine [ `ActivityType` ](https://developer.xamarin.com/api/property/Foundation.NSUserActivity.ActivityType/) von "com.xamarin.SoupChef.viewMenu".

In Aktivitäten und benutzerdefinierte Intents zusammen, die an die app selbst und die Erweiterungen nicht behandelt werden die `AppDelegate` (eine [ `UIApplicationDelegate` ](https://developer.xamarin.com/api/type/UIKit.UIApplicationDelegate/)) durch die [ `ContinueUserActivity` ](https://developer.xamarin.com/api/member/UIKit.UIApplicationDelegate.ContinueUserActivity/) Methode.

### <a name="entitlementsplist"></a>Entitlements.plist

Die **"Entitlements.plist"** Datei die **SoupChef** -Projekt enthält die folgenden:

```xml
<key>com.apple.security.application-groups</key>
<array>
    <string>group.com.xamarin.SoupChef</string>
</array>
<key>com.apple.developer.siri</key>
<true/>
```

Diese Konfiguration gibt an, dass die app die app-Gruppe "group.com.xamarin.SoupChef" verwendet. Die **SoupChefIntents** -app-Erweiterung verwendet diese derselben app-Gruppe, dem die beiden Projekte freigeben kann [`NSUserDefaults`](https://developer.xamarin.com/api/type/Foundation.NSUserDefaults/)
Daten

Die `com.apple.developer.siri` Schlüssel angegeben, dass die app mit Siri interagiert.

> [!NOTE]
> Die **SoupChef** Build des Projekts legt **benutzerdefinierte Berechtigungen** zu **"Entitlements.plist"**.

## <a name="using-an-nsuseractivity-shortcut-to-open-an-app"></a>Mithilfe einer NSUserActivity-Verknüpfung zum Öffnen einer app

Um eine Verknüpfung zu erstellen, die eine app, um bestimmte Inhalte anzuzeigen geöffnet wird, erstellen Sie eine `NSUserActivity` , und fügen Sie es an der View-Controller für den Bildschirm, Sie die Verknüpfung zu öffnen möchten.

### <a name="setting-up-an-nsuseractivity"></a>Das Einrichten einer NSUserActivity

Klicken Sie auf dem Bildschirm "im Menü" `SoupMenuViewController` erstellt eine `NSUserActivity` und weist sie des ansichtscontrollers [ `UserActivity` ](https://developer.xamarin.com/api/property/UIKit.UIResponder.UserActivity/) Eigenschaft:

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();
    UserActivity = NSUserActivityHelper.ViewMenuActivity;
}
```

Festlegen der `UserActivity` Eigenschaft _übergibt Sie_ Aktivität siri. Aus diesem Spende erhält Siri Informationen wann und wo diese Aktivität für den Benutzer relevant und lernt, besser in der Zukunft vorschlagen.

`NSUserActivityHelper` eine Hilfsprogrammklasse befindet sich in der **SoupChef** Lösung in die **SoupKit** -Klassenbibliothek. Erstellt eine `NSUserActivity` und verschiedene Eigenschaften, die im Zusammenhang mit Siri und Suche festgelegt:

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

- Festlegen von `EligibleForPrediction` zu `true` gibt an, dass Siri kann diese Aktivität vorherzusagen und zeigen es als eine Verknüpfung.
- Die [ `ContentAttributeSet` ](https://developer.xamarin.com/api/property/Foundation.NSUserActivity.ContentAttributeSet/) Array ist ein Standard [ `CSSearchableItemAttributeSet` ](https://developer.xamarin.com/api/type/CoreSpotlight.CSSearchableItemAttributeSet/) verwendet, um das Einschließen einer `NSUserActivity` in den Suchergebnissen für iOS.
- [`SuggestedInvocationPhrase`](https://developer.xamarin.com/api/property/Foundation.NSUserActivity.SuggestedInvocationPhrase/) Um einen Ausdruck handelt, den von Siri für den Benutzer als potenzielle Option, beim Zuweisen eines Ausdrucks zu einer Verknüpfung vorgeschlagen wird an.

### <a name="handling-an-nsuseractivity-shortcut"></a>Behandeln eine Verknüpfung NSUserActivity

Behandelt ein `NSUserActivity` Verknüpfung, die von einem Benutzer aufgerufen werden, eine iOS-Anwendung muss überschreiben die `ContinueUserActivity` -Methode der der `AppDelegate` -Klasse, reagieren auf Grundlage der `ActivityType` Feld der übergegebenen `NSUserActivity` Objekt:

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

Diese Methode ruft `HandleUserActivity`, die sucht nach der Segue zum Menü Bildschirm und aufgerufen wird:

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

### <a name="assigning-a-phrase-to-an-nsuseractivity"></a>Zuweisen von einem Ausdruck zu einem NSUserActivity

Einen Ausdruck, der zuweisen eine `NSUserActivity`, öffnen Sie das iOS **Einstellungen** app, und wählen Sie **Siri und Suche > eigene Verknüpfungen**. Wählen Sie die Verknüpfung (in diesem Fall "Order Mittagessen") und Aufzeichnen eines Ausdrucks.

Siri aufrufen und die Verwendung dieses Ausdrucks werden mehr Licht bei Chef zum Menü Bildschirm geöffnet.

## <a name="using-a-custom-intent-shortcut-to-perform-a-task"></a>Verwenden eine benutzerdefinierte beabsichtigte Verknüpfung zum Ausführen einer Aufgabe

### <a name="defining-a-custom-intent"></a>Definieren eine benutzerdefinierte Absicht

Um eine Verknüpfung zu ermöglichen, die einem Benutzer ermöglicht, eine bestimmte Aufgabe, die im Zusammenhang mit Ihrer app schnell abgeschlossen werden, erstellen Sie eine benutzerdefinierte Absicht. Eine benutzerdefinierte Absicht stellt eine Aufgabe, die Benutzer möglicherweise abgeschlossen, Parameter, die für diese Aufgabe und mögliche Antworten, die durch die Ausführung der Aufgabe relevant möchte. Je nachdem, wie eine benutzerdefinierte Absicht definiert ist kann es aufrufen öffnen Sie Ihre app oder eine Hintergrundaufgabe ausführen.

Verwenden Sie Xcode-10, um benutzerdefinierte Prioritäten zu erstellen. In der [SoupChef Repository](https://github.com/xamarin/ios-samples/tree/master/ios12/SoupChef), wird die benutzerdefinierte Absicht in definiert **OrderSoupIntentCodeGen**, ein Objective-C-Projekt. Öffnen Sie das Projekt, und wählen die **Intents.intentdefinition** Datei die **Projektnavigator** an die **OrderSoup** Absicht.

Beachten Sie Folgendes:

- Das Ziel verfügt über eine **Kategorie** von **Reihenfolge**. Es gibt verschiedene vordefinierte Kategorien, die für benutzerdefinierte Prioritäten verwendet werden können. Wählen Sie diejenige, die die Aufgabe Sie benutzerdefinierte virtuelle können am ehesten entspricht. Da dies eine Sortierung app mehr Licht bei ist **OrderSoupIntent** verwendet **Reihenfolge**.
- Die **Bestätigung** Kontrollkästchen gibt an, und zwar unabhängig davon, ob Siri Bestätigung vor dem Ausführen der Aufgabe angefordert werden muss. Für die **Reihenfolge mehr Licht bei** beabsichtigte in mehr Licht bei Chef diese Option aktiviert ist, da der Benutzer einen Kauf tätigen werden.
- Die **Parameter** -Abschnitt der Datei .intentdefinition definiert die relevanten Parameter für eine Verknüpfung. Um einen Auftrag mehr Licht bei platzieren möchten, müssen mehr Licht bei Chef, den Typ des mehr Licht bei, die die Menge und die zugehörigen Optionen kennen.
Jeder Parameter über einen Typ verfügt; Parameter, der über einen vordefinierten Typ dargestellt werden kann, werden als festgelegt **benutzerdefinierte**.
- Die **Verknüpfung Typen** Schnittstelle beschreibt die verschiedenen Parameterkombinationen Siri bei der Verknüpfung zu Ihrem vorgeschlagen können. Die zugeordnete **Titel** und **Untertitel** Abschnitte ermöglichen es Ihnen, die die Nachrichten definieren Siri verwenden, wenn eine vorgeschlagene Verknüpfung für dem Benutzer zu präsentieren.
- Die **unterstützt die Ausführung im Hintergrund** Kontrollkästchen sollte für jede Tastenkombination, die ausgeführt werden kann, ohne die app weiter seitens der Benutzer zu öffnen.

### <a name="defining-custom-intent-responses"></a>Definieren von benutzerdefinierten beabsichtigte Antworten

Die **Antwort** Element geschachtelt unter der **OrderSoup** Absicht darstellt, die möglichen Antworten, die aus einer Bestellung mehr Licht bei.

In der **OrderSoup** Antwortdefinition die Absicht, beachten Sie Folgendes:

- Einer Antwort des **Eigenschaften** dienen zum Anpassen der Nachricht zurück an den Benutzer übermittelt. Die **OrderSoup** beabsichtigte Antwort **mehr Licht bei** und **WaitTime** Eigenschaften.
- Die **Response-Vorlagen** Geben Sie die verschiedenen Erfolgs- und Fehlermeldungen, die verwendet werden können, um Status anzugeben, nachdem ein Intent Task abgeschlossen wurde.
- Die **Erfolg** Kontrollkästchen sollte ausgewählt sein, für Antworten, die Erfolg zu melden.
 - Die **OrderSoupIntent** Erfolgsantwort verwendet die **mehr Licht bei** und **WaitTime** Eigenschaften, um eine benutzerfreundliche und nützliche Meldung beschreibt, wann der Auftrag mehr Licht bei bereit stehen.

### <a name="generating-code-for-the-custom-intent"></a>Generieren von Code für die benutzerdefinierte Absicht

Beim Erstellen des Xcode-Projekts, enthält diese benutzerdefinierte beabsichtigte Definition bewirkt, dass Xcode, um Code zu generieren, die für die programmgesteuerte Interaktion mit der benutzerdefinierten Absicht und Antworten verwendet werden können.

Dieser generierte Code anzeigen zu können:

- Open **"appdelegate.m"**.
- Fügen Sie einen Import in die benutzerdefinierte Absicht-Headerdatei hinzu: `#import "OrderSoupIntent.h"`
- Fügen Sie in einer beliebigen Methode in der Klasse, die einen Verweis auf `OrderSoupIntent`.
- Mit der rechten Maustaste auf `OrderSoupIntent` , und wählen Sie **fahren Sie mit der Definition**.
- Mit der rechten Maustaste in der Datei neu geöffnete **OrderSoupIntent.h**, und wählen Sie **im Finder anzeigen**.
- Dadurch wird geöffnet, eine **Finder** Fenster, das eine h- und fehlerfrei-Datei, die mit den generierten Code enthält.

Dieser generierte Code enthält:

- `OrderSoupIntent` – Eine Klasse, die benutzerdefinierte Absicht darstellt.
- `OrderSoupIntentHandling` – Ein Protokoll, das definiert, die Methoden, die verwendet werden, um sicherzustellen, dass das Ziel ausgeführt werden soll, und die Methode, die sie tatsächlich ausgeführt wird.
- `OrderSoupIntentResponseCode` : Eine Enumeration, die verschiedenen Status der Antwort definiert.
- `OrderSoupIntentResponse` – eine Klasse, die die Antwort auf eine Tabellensperre für die Ausführung darstellt.

### <a name="creating-a-binding-to-the-custom-intent"></a>Erstellen einer Bindung an die benutzerdefinierte intent

Um den generierten Code von Xcode in einer Xamarin.iOS-app verwenden zu können, erstellen Sie eine C# für diese Bindung.

#### <a name="creating-a-static-library-and-c-binding-definitions"></a>Erstellen einer statischen Bibliothek und C# Bindungsdefinitionen

In der [SoupChef Repository](https://github.com/xamarin/ios-samples/tree/master/ios12/SoupChef), werfen Sie einen Blick die **OrderSoupIntentStaticLib** Ordner, und Öffnen der **OrderSoupIntentStaticLib.xcodeproj** Xcode-Projekt.

Dies **Cocoa Touch statische Bibliothek** -Projekt enthält die **OrderSoupIntent.h** und **OrderSoupIntent.m** Dateien, die von Xcode generiert.

#### <a name="configuring-the-static-library-project-build-settings"></a>Konfigurieren die statische Bibliothek Buildeinstellungen des Projekts

In Xcode **Projektnavigator**, wählen Sie die Projekte auf oberster Ebene **OrderSoupIntentStaticLib**, und navigieren Sie zu **Build Phases > Quellen kompilieren**.
Beachten Sie, dass **OrderSoupIntent.m** (welche Importe **OrderSoupIntent.h**) hier aufgeführt. In **Binärdatei mit Verknüpfungsbibliotheken**, beachten Sie, dass **Intents.framework** und **Foundation.framework** sind enthalten.
Mit diesen Einstellungen vorhanden wird das Framework ordnungsgemäß erstellt.

#### <a name="building-the-static-library-and-generating-c-bindings-definitions"></a>Erstellen die statische Bibliothek, und Generieren von C# Bindungen-Definitionen

Erstellen Sie die statische Bibliothek, und generieren C# Bindungen Definitionen, gehen Sie folgendermaßen vor:

- [Installieren Sie die Ziel-Sharpie](https://docs.microsoft.com/xamarin/cross-platform/macios/binding/objective-sharpie/get-started?context=xamarin/mac#installing-objective-sharpie), das Tool zum Generieren von Bindungen Definitionen aus den h- und fehlerfrei-Dateien, die von Xcode generiert wurde.

- Konfigurieren Sie Ihr System für die Xcode-10-Befehlszeilentools zu verwenden:

    > [!WARNING]
    > Aktualisieren die ausgewählten Befehlszeilentools wirkt sich auf alle installierten Versionen von Xcode auf Ihrem System aus. Wenn Sie fertig sind mit den mehr Licht bei Chef Beispiel-app, achten Sie darauf, diese wiederherzustellen auf seine ursprüngliche Konfiguration festlegen.

    - Wählen Sie in Xcode **Xcode > Voreinstellungen > Speicherorte** und legen Sie **Befehlszeilentools** für die aktuelle Xcode-10-Installation auf Ihrem System verfügbar.

- Im Terminal `cd` auf die **OrderSoupIntentStaticLib** Verzeichnis.

- Typ `make`, bei welchen Builds:

    - Die statische Bibliothek, **libOrderSoupIntentStaticLib.a**
    - In der **Bo** Ausgabeverzeichnis C# Bindungen Definitionen:
        - **ApiDefinitions.cs**
        - **StructsAndEnums.cs**

Die **OrderSoupIntentBindings** -Projekt, das diese statische Bibliothek und die zugehörigen Bindungen Definitionen verwendet, erstellt diese Elemente automatisch.
Manuell über das oben beschriebene Verfahren ausführen wird jedoch sichergestellt, dass sie erstellt werden, wie erwartet.

Weitere Informationen zum Erstellen einer statischen Bibliothek, und erstellen mithilfe von Ziel Sharpie C# Bindungen Definitionen, sehen Sie sich die [Binden einer iOS Objective-C-Bibliothek](https://docs.microsoft.com/xamarin/ios/platform/binding-objective-c/walkthrough?tabs=vsmac#creating-a-static-library) Exemplarische Vorgehensweise.

#### <a name="creating-a-bindings-library"></a>Erstellen einer bindungsbibliothek für

Mit der statischen Bibliothek und die C# Bindungen Definitionen erstellt haben, die verbleibende Information erforderlich, die von Xcode generierte nutzen Intent-bezogenem Code in einem Xamarin.iOS-Projekt ist eine bindungsbibliothek.

In der [mehr Licht bei Chef-Repository](https://github.com/xamarin/ios-samples/tree/master/ios12/SoupChef), öffnen Sie die **SoupChef.sln** Datei. Unter anderem diese Lösung umfasst **OrderSoupIntentBinding**, eine bindungsbibliothek mit die statische Bibliothek, die zuvor generierten.

Achten Sie insbesondere, dass dieses Projekt enthält:

- **ApiDefinitions.cs** – eine Datei, die vom Ziel Sharpie oben generiert, und diesem Projekt hinzugefügt. Diese Datei **Buildvorgang** nastaven NA hodnotu **ObjcBindingApiDefinition**.
- **StructsAndEnums.cs** – eine andere Datei, die vom Ziel Sharpie oben generiert und an diesem Projekt hinzugefügt. Diese Datei **Buildvorgang** nastaven NA hodnotu **ObjcBindingCoreSource**.
- Ein **nativen Verweis** zu **libOrderSoupIntentStaticLib.a**, die statische Bibliothek, die weiter oben erstellt.

> [!NOTE]
> Beide **ApiDefinitions.cs** und **StructsAndEnums.cs** enthalten Attribute wie z. B. `[Watch (5,0), iOS (12,0)]`. Diese Attribute, die vom Ziel Sharpie, generiert wurden auskommentiert, da sie nicht für dieses Projekt erforderlich sind.

Weitere Informationen zum Erstellen einer C# bindungsbibliothek, verschaffen Sie sich einen Blick auf die [Binden einer iOS Objective-C-Bibliothek](https://docs.microsoft.com/xamarin/ios/platform/binding-objective-c/walkthrough?tabs=vsmac#create-a-xamarinios-binding-project) Exemplarische Vorgehensweise.

Beachten Sie, dass die **SoupChef** -Projekt enthält einen Verweis auf **OrderSoupIntentBinding**, d. h., die er jetzt, im zugreifen können C#, Klassen, Schnittstellen und Enumerationen, die es enthält:

- `OrderSoupIntent`
- `OrderSoupIntentHandling`
- `OrderSoupIntentResponse`
- `OrderSoupIntenseResponseCode`

### <a name="adding-the-intentdefinition-file-to-your-solution"></a>Hinzufügen der .intentdefinition-Datei der Projektmappe

In der C# **SoupChef** Lösung, die **SoupKit** Projekt enthält Codes gemeinsam auf die app und die zugehörigen Erweiterungen. Die **Intents.intentdefinition** Datei befindet sich in der **Base.lproj** Verzeichnis **SoupKit**, und verfügt über eine **Buildvorgang** von **Content**. Der Buildprozess kopiert diese Datei in mehr Licht bei Chef-app-Bündel, wenn es erforderlich, für die app ordnungsgemäß ist.

### <a name="donating-an-intent"></a>Das Spenden ein Intent-Objekt

In der Reihenfolge für Siri, um eine Verknüpfung vorzuschlagen müssen sie zuerst verstehen, wenn die Verknüpfung relevant ist.

Siri erteilen, diese zu verstehen, mehr Licht bei Chef _übergibt Sie_ Intent siri jedes Mal der Benutzer platziert einen Auftrag mehr Licht bei. Basierend auf diesem Spende –, wenn es für wohltätige Zwecke gespendet wurde, in dem sie gespendet wurde, das die Parameter, die es enthält – Siri lernt, wenn die Verknüpfung in der Zukunft vorschlagen.

**SoupChef** verwendet die `SoupOrderDataManager` -Klasse die Spenden.
Wenn ein Benutzer, eine Soup bestellen der `PlaceOrder` Methode wiederum ruft [ `DonateInteraction` ](https://developer.xamarin.com/api/member/Intents.INInteraction.DonateInteraction/):

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

Nach dem Abrufen der Intent, es ist innerhalb einer [ `INInteraction` ](https://developer.xamarin.com/api/type/Intents.INInteraction/).
Die `INInteraction` erhält ein [`Identifier`](https://developer.xamarin.com/api/property/Intents.INInteraction.Identifier/)
Die eindeutige ID des Auftrags (Dies wird später bei hilfreich sein beabsichtigte Spenden zu löschen, die nicht mehr gültig sind) entspricht. Anschließend wird die Interaktion siri gespendet.

Der Aufruf von der `order.Intent` Get-Methode ruft eine `OrderSoupIntent` , das die Reihenfolge darstellt, durch Festlegen der `Quantity`, `Soup`, `Options`, und Bild und einem Ausdruck aufrufen, die als einen Vorschlag verwendet werden soll, wenn der Benutzer einen Ausdruck für Siri zuordnen aufgezeichnet mit der Absicht:

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

#### <a name="removing-invalid-donations"></a>Entfernen von ungültigen Spenden

Es ist wichtig, die Spenden zu entfernen, die nicht mehr gültig sind, damit, dass Siri nicht hilfreich oder widersprüchlichen Verknüpfung Vorschläge.

In mehr Licht bei Chef, die **konfigurieren Sie im Menü** Bildschirm kann verwendet werden, um ein Menü Element als nicht verfügbar markiert. Siri sollten eine Verknüpfung, um ein nicht verfügbar-Menüelement, order nicht mehr empfehlen daher die `RemoveDonation` -Methode der `SoupMenuManager` löscht Spenden für Menüelemente, die nicht mehr verfügbar sind. Dies geschieht durch:

- Suchen Aufträge mit dem Menüelement für die jetzt nicht mehr verfügbar.
- Gespeichert, deren IDs.
- Löschen von Interaktionen, die diese dieselbe ID haben.

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

### <a name="creating-an-intents-extension"></a>Erstellt eine Intents-Erweiterung

Der Code, der ausgeführt wird, wenn Sie Siri Intent aufgerufen haben, befindet sich im eine Intents-Erweiterung, die als ein neues Projekt auf der gleichen Projektmappe wie eine vorhandene Xamarin.iOS-app wie z. B. mehr Licht bei Chef hinzugefügt werden können. In der **SoupChef** Lösung, die Erweiterung wird aufgerufen, **SoupChefIntents**.

#### <a name="soupchefintents-infoplist-and-entitlementsplist"></a>SoupChefIntents – Datei "Info.plist" und "Entitlements.plist"

##### <a name="soupchefintents-infoplist"></a>SoupChefIntents – Datei "Info.plist"

Die **"Info.plist"** in die **SoupChefIntents** Projekt definiert die **Bündel-ID** als `com.xamarin.SoupChef.SoupChefIntents`.

Die **"Info.plist"** -Datei enthält außerdem die folgenden:

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

In der obigen **"Info.plist"**:

- `IntentsRestrictedWhileLocked` Listet Intent-Elemente, die nur behandelt werden soll, wenn das Gerät entsperrt wird.
- `IntentsSupported` Listet die Absichten von dieser Erweiterung verarbeitet.
- `NSExtensionPointIdentifier` Gibt den Typ des app-Erweiterung (finden Sie unter [Apple Dokumentation](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/AppExtensionKeys.html#//apple_ref/doc/uid/TP40014212-SW15) Informationen).
- `NSExtensionPrincipalClass` Gibt die Klasse, die zum Verarbeiten von Intents, die von dieser Erweiterung unterstützt verwendet werden soll.

##### <a name="soupchefintents-entitlementsplist"></a>SoupChefIntents – "Entitlements.plist"

Die **"Entitlements.plist"** in die **SoupChefIntents** Projekt verfügt über die **App-Gruppen** Funktion. Diese Funktion ist für die Verwendung die gleichen app-Gruppe als konfiguriert die **SoupChef** Projekt:

```xml
<key>com.apple.security.application-groups</key>
<array>
    <string>group.com.xamarin.SoupChef</string>
</array>
```

Mehr Licht bei Chef speichert Daten mit `NSUserDefaults`. Um die Freigabe von Daten zwischen der app und die app-Erweiterung, die sie verweisen in der gleichen app-Gruppe ihre **"Entitlements.plist"** Dateien.

> [!NOTE]
> Die **SoupChefIntents** Build des Projekts legt **benutzerdefinierte Berechtigungen** zu **"Entitlements.plist"**.

#### <a name="handling-an-ordersoupintent-background-task"></a>Eine Hintergrundaufgabe OrderSoupIntent behandeln

Eine Intents-Erweiterung führt die erforderlichen Hintergrundaufgaben für eine Verknüpfung basierend auf einer benutzerdefinierten Absicht.

Siri-Aufrufe der [ `GetHandler` ](https://developer.xamarin.com/api/member/Intents.INExtension.GetHandler/) -Methode der der `IntentHandler` Klasse (definiert im **"Info.plist"** als die `NSExtensionPrincipalClass`) um eine Instanz einer Klasse, die erweitert `OrderSoupIntentHandling`, verwendet werden kann, behandelt ein `OrderSoupIntent`:

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

`OrderSoupIntentHandler`, definiert der **SoupKit** Projekt mit freigegebenen Code implementiert zwei wichtige Methoden:

- `ConfirmOrderSoup` – Wird bestätigt, und zwar unabhängig davon, ob die Aufgabe verknüpft ist, mit der Absicht tatsächlich ausgeführt werden soll
- `HandleOrderSoup` – Die mehr Licht bei bestellt und antwortet dem Benutzer durch den Aufruf übergebenen Abschlusshandler

#### <a name="handling-an-ordersoupintent-that-opens-the-app"></a>Ein OrderSoupIntent behandeln, die die app geöffnet wird.

Eine app muss ordnungsgemäß Intents verarbeiten, die nicht im Hintergrund ausgeführt werden.
Diese werden verarbeitet, auf die gleiche Weise wie `NSUserActivity` Verknüpfungen in der `ContinueUserActivity` -Methode der `AppDelegate`:

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

Eine Intents-Benutzeroberflächenerweiterung stellt eine benutzerdefinierte Benutzeroberfläche für eine Intents-Erweiterung bereit. In der **SoupChef** Lösung **SoupChefIntentsUI** wird eine Intents-Benutzeroberflächenerweiterung, die eine Schnittstelle für bietet **SoupChefIntents**.

### <a name="soupchefintentsui--infoplist-and-entitlementsplist"></a>SoupChefIntentsUI – Datei "Info.plist" und "Entitlements.plist"

#### <a name="soupchefintentsui-infoplist"></a>SoupChefIntentsUI – Datei "Info.plist"

Die **"Info.plist"** in die **SoupChefIntentsUI** Projekt definiert die **Bündel-ID** als `com.xamarin.SoupChef.SoupChefIntentsui`.

Die **"Info.plist"** -Datei enthält außerdem die folgenden:

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

In der obigen **"Info.plist"**:

- `IntentsSupported` Gibt an, dass die `OrderSoupIntent` erfolgt durch dieses Intents-Benutzeroberflächenerweiterung.
- `NSExtensionPointIdentifier` Gibt den Typ des app-Erweiterung (finden Sie unter [Apple Dokumentation](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/AppExtensionKeys.html#//apple_ref/doc/uid/TP40014212-SW15) Informationen).
- `NSExtensionMainStoryboard` Gibt an, das Storyboard, das die primäre Schnittstelle dieser Erweiterung definiert.

#### <a name="soupchefintentsui-entitlementsplist"></a>SoupChefIntentsUI – "Entitlements.plist"

Die **SoupChefIntentsUI** Projekt muss kein **"Entitlements.plist"** Datei.

### <a name="creating-the-user-interface"></a>Erstellen der Benutzeroberfläche

Da die **"Info.plist"** für **SoupChefIntentsUI** legt diese fest der `NSExtensionMainStoryboard` um `MainInterface`, **MainInterace.storyboard** Datei definiert die Schnittstelle für die Intents-Benutzeroberflächenerweiterung.

In diesem Storyboard, es wird ein Einzelansicht-Controller, des Typs **IntentViewController**. Es verweist auf zwei Ansichten:

- **InvoiceView**, des Typs `InvoiceView`
- **ConfirmationView**, des Typs `ConfirmOrderView`

> [!NOTE]
> Die Schnittstellen für **InvoiceView** und **ConfirmationView** in definiert **"Main.Storyboard"** als sekundären Ansichten. Der iOS-Designer in Visual Studio für Mac und Visual Studio 2017 bietet keine Unterstützung zum Anzeigen oder Bearbeiten von sekundären Ansichten. Öffnen Sie zu diesem Zweck **"Main.Storyboard"** in Interface Builder von Xcode.

`IntentViewController` implementiert die [`IINUIHostedViewControlling`](https://developer.xamarin.com/api/type/IntentsUI.IINUIHostedViewControlling/)
-Schnittstelle, um eine benutzerdefinierte Schnittstelle bereitzustellen, bei der Arbeit mit der Absicht "Siri" verwendet. Die [`ConfigureView`](https://developer.xamarin.com/api/member/IntentsUI.INUIHostedViewControlling_Extensions.ConfigureView/)
Methode wird aufgerufen, um die Benutzeroberfläche, die Anzeige der Bestätigung oder der Rechnung, je nachdem, ob die Aktivität bestätigt wird ist anpassen ([`INIntentHandlingStatus.Ready`](https://developer.xamarin.com/api/type/Intents.INIntentHandlingStatus/)) oder erfolgreich ausgeführt wurde ([ `INIntentHandlingStatus.Success`](https://developer.xamarin.com/api/type/Intents.INIntentHandlingStatus/)):

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
> Weitere Informationen zu den `ConfigureView` -Methode, Präsentation von Apple WWDC 2017, [Neuigkeiten in SiriKit](https://developer.apple.com/videos/play/wwdc2017/214/).

## <a name="creating-a-voice-shortcut"></a>Erstellen einer Sprach-Verknüpfung

Mehr Licht bei Chef bietet eine-Schnittstelle, um jede Bestellung eine Voice-Tastenkombination zuweisen sodass Reihenfolge mehr Licht bei mit Siri. In der Tat die Schnittstelle zum Aufzeichnen und Voice-Tastenkombinationen zuweisen, wird von iOS bereitgestellten und erfordert wenig benutzerdefinierten Code.

In `OrderDetailViewController`, wenn ein Benutzer der Tabelle tippt **siri hinzufügen** Zeile, die [ `RowSelected` ](https://developer.xamarin.com/api/member/UIKit.UITableViewSource.RowSelected/) Methode stellt einen Bildschirm zum Hinzufügen oder bearbeiten eine Voice-Verknüpfung:

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

Basierend auf, und zwar unabhängig davon, ob eine vorhandene Voice-Verknüpfung für die aktuell angezeigten Reihenfolge vorhanden ist `RowSelected` stellt einen ansichtscontroller vom Typ [ `INUIEditVoiceShortcutViewController` ](https://developer.xamarin.com/api/type/IntentsUI.INUIEditVoiceShortcutViewController/) oder [ `INUIAddVoiceShortcutViewController` ](https://developer.xamarin.com/api/type/IntentsUI.INUIAddVoiceShortcutViewController/).
In jedem Fall `OrderDetailViewController` richtet sich selbsttätig als des ansichtscontrollers `Delegate`, ist es implementiert auch [`IINUIAddVoiceShortcutViewControllerDelegate`](https://developer.xamarin.com/api/type/IntentsUI.IINUIAddVoiceShortcutViewControllerDelegate/)
und [ `IINUIEditVoiceShortcutViewControllerDelegate` ](https://developer.xamarin.com/api/type/IntentsUI.IINUIEditVoiceShortcutViewControllerDelegate/).

## <a name="testing-on-device"></a>Testen auf Gerät

Um mehr Licht bei Chef auf einem Gerät auszuführen, führen Sie die folgenden Anweisungen aus. Lesen Sie auch die [Hinweis zur automatischen Bereitstellung](#automatic-provisioning).

### <a name="app-group-app-ids-provisioning-profiles"></a>App-Gruppe, App-IDs, Bereitstellungsprofile

In der **Certificates, IDs & Profiles** Teil der [Apple Developer Portal](https://developer.apple.com/), gehen Sie folgendermaßen vor:

- Erstellen Sie eine app-Gruppe zum Freigeben von Daten zwischen der app mehr Licht bei Chef und die zugehörigen Erweiterungen. Zum Beispiel: **group.com.yourcompanyname.SoupChef**

- Erstellen Sie drei App-IDs: eine für die app selbst und für die Intents-Erweiterung für die Intents-Benutzeroberflächenerweiterung. Zum Beispiel:

    - App: **com.yourcompanyname.SoupChef**
        - Weisen Sie zu dieser App-ID, die SiriKit und **App-Gruppen** Funktionen.

    - Intents-Erweiterung: **com.yourcompanyname.SoupChef.Intents**
        - Weisen Sie dieser App-ID, die **App-Gruppen** Funktion.

    - Intents-Benutzeroberflächenerweiterung: **com.yourcompanyname.SoupChef.Intentsui**
        - Diese App-ID benötigt keine speziellen Funktionen.

- Bearbeiten Sie nach dem Erstellen der oben aufgeführten App-IDs, die **App-Gruppen** Funktion zugewiesen wird, für die app sowie die Intents-Erweiterung, die bestimmte app-Gruppe angeben, die oben erstellt haben.

- Erstellen Sie drei neue Entwicklung bereitstellungsprofile, eines für jeden der neuen App-IDs.

- Laden Sie diese bereitstellungsprofile aus, und doppelklicken Sie auf jedes Ereignis, um es zu installieren. Wenn es sich bei Visual Studio für Mac oder Visual Studio 2017 bereits ausgeführt wird, neu starten Sie, um sicherzustellen, dass sie die neue bereitstellungsprofile registriert.

### <a name="editing-infoplist-entitlementsplist-and-source-code"></a>Bearbeiten der Datei "Info.plist", "Entitlements.plist" und Quellcode

Führen Sie folgende Schritte aus, in Visual Studio für Mac oder Visual Studio 2017:

- Aktualisieren Sie die verschiedenen **"Info.plist"** Dateien in der Projektmappe. Legen Sie die app, die Intents-Erweiterung und die Intents-Benutzeroberflächenerweiterung **Bündel-ID** für die oben definierte App-IDs:

    - App: **com.yourcompanyname.SoupChef**
    - Intents-Erweiterung: **com.yourcompanyname.SoupChef.Intents**
    - Intents-Benutzeroberflächenerweiterung: **com.yourcompanyname.SoupChef.Intentsui**

- Update der **"Entitlements.plist"** -Datei für die **SoupChef** Projekt:
    - Für die **App-Gruppen** -Funktion die Gruppe auf die oben erstellte neue app-Gruppe festgelegt (im obigen Beispiel war **group.com.yourcompanyname.SoupChef**).
    - Stellen Sie sicher, dass **SiriKit** aktiviert ist.

- Update der **"Entitlements.plist"** -Datei für die **SoupChefIntents** Projekt:
    - Für die **App-Gruppen** -Funktion die Gruppe auf die oben erstellte neue app-Gruppe festgelegt (im obigen Beispiel war **group.com.yourcompanyname.SoupChef**).

- Öffnen Sie abschließend **NSUserDefaultsHelper.cs**. Legen Sie die `AppGroup` Variable auf den Wert der neuen app-Gruppe (Legen Sie sie z. B. auf `group.com.yourcompanyname.SoupChef`).

### <a name="configuring-the-build-settings"></a>Konfigurieren von Einstellungen der Buildkonfiguration

In Visual Studio für Mac oder Visual Studio 2017:

- Öffnen Sie die Optionen/Eigenschaften für die **SoupChef** Projekt. Auf der **iOS Bundle-Signierung** Registerkarte **Signierungsidentität** für die automatische und **Bereitstellungsprofil** erstellen ein Profil für die neue app-spezifische Bereitstellung oben erstellt haben.

- Öffnen Sie die Optionen/Eigenschaften für die **SoupChefIntents** Projekt. Auf der **iOS Bundle-Signierung** Registerkarte **Signierungsidentität** für die automatische und **Bereitstellungsprofil** auf die neuen Absichten erweiterungsspezifische Bereitstellungsprofil, die Sie erstellt haben weiter oben.

- Öffnen Sie die Optionen/Eigenschaften für die **SoupChefIntentsUI** Projekt. Auf der **iOS Bundle-Signierung** Registerkarte **Signierungsidentität** für die automatische und **Bereitstellungsprofil** zum Intent-Elemente der Benutzeroberfläche für die neue Bereitstellung der erweiterungsspezifischen erstellen ein Profil weiter oben erstellt.

Diese Änderungen vorgenommen wird die app auf einem iOS-Gerät ausgeführt.

### <a name="automatic-provisioning"></a>Automatische Bereitstellung

Beachten Sie, mit denen Sie [automatische Bereitstellung](https://docs.microsoft.com/en-us/xamarin/ios/get-started/installation/device-provisioning/automatic-provisioning) zu viele Aufgaben direkt in der IDE-Bereitstellung zu erreichen.
Allerdings ist die automatische Bereitstellung von app-Gruppen nicht festgelegt. Sie müssen manuell konfigurieren, die **"Entitlements.plist"** Dateien mit dem Namen der app-Gruppe, die Sie gerne verwenden würden, finden Sie im Apple Developer Portal, um die app-Gruppe zu erstellen, weisen Sie diese app-Gruppe zu einzelnen App-ID, die automatisch erstellt Bereitstellung, generieren die bereitstellungsprofile (app, Intents-Erweiterung, Intents-Benutzeroberflächenerweiterung), enthalten die neu erstellte app-Gruppe, und das Herunterladen und installieren Sie sie neu.

## <a name="related-links"></a>Verwandte Links

- [Mehr Licht bei Chef (Xamarin)](https://developer.xamarin.com/samples/monotouch/ios12/SoupChef/)
- [Mehr Licht bei Chef (Swift)](https://developer.apple.com/documentation/sirikit/accelerating_app_interactions_with_shortcuts?language=objc)
- [SiriKit (Apple)](https://developer.apple.com/sirikit/)
- [Einführung in die Siri Verknüpfungen – WWDC 2018](https://developer.apple.com/videos/play/wwdc2018/211/)
- [Erstellen für die Sprache mit Siri Verknüpfungen – WWDC 2018](https://developer.apple.com/videos/play/wwdc2018/214/)
- [Siri-Verknüpfungen auf dem Zifferblatt von Siri – WWDC 2018](https://developer.apple.com/videos/play/wwdc2018/217/)
- [Neuerungen in SiriKit – WWDC 2017](https://developer.apple.com/videos/play/wwdc2017/214/)
- [Erstellt eine Intents-App-Erweiterung (Apple)](https://developer.apple.com/documentation/sirikit/creating_an_intents_app_extension?language=objc)
