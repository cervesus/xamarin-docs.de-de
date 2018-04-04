---
title: Arbeiten mit der übergeordneten Anwendung
description: Freigeben von Daten zwischen der app für iOS "und" Überwachen in WatchOS 1
ms.prod: xamarin
ms.assetid: 9AD29833-E9CC-41A3-95D2-8A655FF0B511
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 769847cccb3e21fea4d8f45d8e5d0c0fb59bdd43
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-the-parent-application"></a>Arbeiten mit der übergeordneten Anwendung

_Freigeben von Daten zwischen der app für iOS "und" Überwachen in WatchOS 1_

> [!IMPORTANT]
> Zugriff auf die übergeordnete Anwendung ausschließlich über den folgenden Beispielen, funktioniert auf WatchOS 1 Watch-apps.


Es gibt verschiedene Möglichkeiten für die Kommunikation zwischen dem Watch-app und die iOS-app, der im Paket mit:

- Überwachen-Erweiterungen können [Aufrufen einer Methode](#code) für die übergeordnete-app, die im Hintergrund ausgeführt, auf dem iPhone wird.

- Überwachen-Erweiterungen können [Freigeben von einem Speicherort](#storage) mit der übergeordneten iPhone-app.

- Verwenden Übergabe übergeben von Daten aus einem Blick oder eine Benachrichtigung an die Watch-app, die Benutzer an eine bestimmte Schnittstelle Controller in der app zu senden.

Die übergeordnete App wird manchmal auch als die App-Container bezeichnet.


<a name="code" />

## <a name="run-code"></a>Ausführen von Code

Kommunikation zwischen einem Watch-Erweiterung, und die übergeordnete iPhone-app wird veranschaulicht, der [GpsWatch Beispiel](https://developer.xamarin.com/samples/GpsWatch).
Watch-Erweiterung kann anfordern, dass die übergeordnete iOS-app möchten eine Verarbeitung auf seinen Namen mithilfe der `OpenParentApplication` Methode.

Dies ist besonders nützlich für lang ausgeführter Vorgänge (einschließlich Anforderungen über das Netzwerk) - nur das übergeordnete Element für iOS-app nutzen kann im Hintergrund zu verarbeiten, um diese Aufgaben abgeschlossen, und speichern die abgerufenen Daten an einem Ort zugegriffen werden kann, um die Watch-Erweiterung.



### <a name="watch-kit-app-extension"></a>Überwachen-Kit-App-Erweiterung

Rufen Sie die `WKInterfaceController.OpenParentApplication` in Ihre Watch-app-Erweiterung. Es gibt eine `bool` , der angibt, ob die GET-Anforderung erfolgreich gesendet wurde. Überprüfen Sie die `error` Parameter in der Antwort `Action` bestimmen, ob alle ist aufgetreten, während der Ausführung der Methode, die in der iPhone-app ausgeführt wird.

```csharp
WKInterfaceController.OpenParentApplication (new NSDictionary (), (replyInfo, error) => {
    if(error != null) {
        Console.WriteLine (error);
        return;
    }
    Console.WriteLine ("parent app responded");
    // do something with replyInfo[] dictionary
});
```


### <a name="ios-app"></a>iOS App

Alle Aufrufe von einer Watch-app-Erweiterung über die iPhone-app weitergeleitet `HandleWatchKitExtensionRequest` Methode.
Wenn Sie, die unterschiedliche Anforderungen in die Watch-app, und klicken Sie dann diese Methode zum Abfragen müssen die `userInfo` Wörterbuch bestimmen, wie die Anforderung zu verarbeiten.


```csharp
[Register ("AppDelegate")]
public partial class AppDelegate : UIApplicationDelegate
{
    // ... other AppDelegate methods
    public override void HandleWatchKitExtensionRequest
        (UIApplication application, NSDictionary userInfo, Action<NSDictionary> reply)
    {
        var status = 2;
        // do something in the background, and respond
        reply (new NSDictionary (
            "count", NSNumber.FromInt32 ((int)status),
            "value2", new NSString("some-info")
            ));
    }
}
```


<a name="storage" />

## <a name="shared-storage"></a>Freigegebener Speicher

Wenn Sie konfigurieren eine [app-Gruppe](~/ios/watchos/app-fundamentals/app-groups.md) und iOS 8-Erweiterungen (einschließlich Watch-Erweiterungen) Daten für die übergeordnete app freigeben können.

<a name="nsuserdefaults" />

### <a name="nsuserdefaults"></a>NSUserDefaults

Der folgende Code kann in Watch-app-Erweiterung und die übergeordnete iPhone-app geschrieben werden, sodass sie einen gemeinsamen Satz von verweisen können `NSUserDefaults`:

```csharp
NSUserDefaults shared = new NSUserDefaults(
        "group.com.your-company.watchstuff",
        NSUserDefaultsType.SuiteName);

// set values
shared.SetInt (2, "count");
shared.Synchronize ();

// get values
shared.Synchronize ();
var count = shared.IntForKey ("count");
```

<a name="files" />

### <a name="files"></a>Dateien

Die iOS-app "und" Überwachen der Erweiterung kann auch Dateien, die über einen gemeinsamen Dateipfad freigeben.

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =
            FileManager.GetContainerUrl ("group.com.your-company.watchstuff");
var appGroupContainerPath = appGroupContainer.Path;
Console.WriteLine ("agcpath: " + appGroupContainerPath);
// use the path to create and update files
```

Hinweis: Wenn der Pfad ist `null` überprüfen Sie dann die [app-Gruppenkonfiguration](~/ios/watchos/app-fundamentals/app-groups.md) sicherstellen, dass die provisioning Profile ordnungsgemäß konfiguriert wurden und auf dem Entwicklungscomputer heruntergeladen und installiert wurden.

Weitere Informationen finden Sie unter der [App Gruppenverwaltungsfunktionen](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) Dokumentation.

## <a name="wormholesharp"></a>WormHoleSharp

Eine beliebte Open-Source-Mechanismus für WatchOS 1 (basierend auf [MMWormHole](https://github.com/mutualmobile/MMWormhole)), Daten oder Befehle zwischen der übergeordneten Anwendung und die Watch-app übergeben.

Sie können eine app-Gruppe wie folgt in der iOS-app Wurmloch konfigurieren und beobachten die Erweiterung:

```csharp
// AppDelegate (iOS) or InterfaceController (watch extension)
Wormhole wormHole;
// ...
wormHole = new Wormhole ("group.com.your-company.watchstuff", "messageDir");
```

Die C#-Version herunterladen [WormHoleSharp](https://github.com/Clancey/WormHoleSharp).



## <a name="related-links"></a>Verwandte Links

- [GpsWatch (Beispiel)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [WormHoleSharp (Beispiel)](https://github.com/Clancey/WormHoleSharp)
- [Apple WKInterfaceController Verweis](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:)
- [Apple Freigeben von Daten mit der enthaltenden App](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
