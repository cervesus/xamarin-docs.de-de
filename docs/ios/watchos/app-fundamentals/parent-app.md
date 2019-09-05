---
title: Arbeiten mit dem WatchOS übergeordnete Anwendung in Xamarin
description: Dieses Dokument beschreibt das Arbeiten mit einer WatchOS übergeordnete Anwendung in Xamarin. Es wird erläutert, WatchKit-app-Erweiterungen, iOS-apps, freigegebenen Speicher und mehr.
ms.prod: xamarin
ms.assetid: 9AD29833-E9CC-41A3-95D2-8A655FF0B511
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: 0049d69caabce545b2813dbd2b3905fe96f28fed
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292728"
---
# <a name="working-with-the-watchos-parent-application-in-xamarin"></a>Arbeiten mit dem WatchOS übergeordnete Anwendung in Xamarin

> [!IMPORTANT]
> Zugreifen auf die übergeordnete Anwendung nur mit den folgenden Beispielen funktioniert auf Watch-apps für WatchOS 1.


Es gibt verschiedene Möglichkeiten für die Kommunikation zwischen der Watch-app und dem iOS-app, der sie mit gebündelt wird:

- Watch-Erweiterungen können eine Methode für die übergeordnete APP [aufzurufen](#code) , die im Hintergrund auf dem iPhone ausgeführt wird.

- Sehen Sie sich Erweiterungen können [freigeben einen Speicherort](#storage) mit der übergeordneten iPhone-app.

- Übergeben Sie mithilfe von Handoff Daten von einem Blick oder einer Benachrichtigung an die Watch-APP, und senden Sie den Benutzer an einen bestimmten Schnittstellen Controller in der app.

Die übergeordnete-App wird manchmal auch als der Container-App bezeichnet.


<a name="code" />

## <a name="run-code"></a>Ausführen von Code

Kommunikation zwischen eine Watch-Erweiterung und der übergeordneten iPhone-app wird veranschaulicht, der [GpsWatch Beispiel](https://docs.microsoft.com/samples/xamarin/ios-samples/watchkit-gpswatch).
Die Watch-Erweiterung kann anfordern, dass die übergeordnete iOS-app können Sie einige Verarbeitungsschritte auf seinen Namen mithilfe der `OpenParentApplication` Methode.

Dies ist besonders nützlich für lang andauernde Aufgaben (einschließlich netzwerkanforderungen): nur das übergeordnete iOS-app nutzen kann, der hintergrundverarbeitung um führen diese Aufgaben aus, und speichern die abgerufenen Daten an einem Speicherort auf der Watch-Erweiterung zugreifen kann.



### <a name="watch-kit-app-extension"></a>Sehen Sie sich App-Erweiterung für Kit

Rufen Sie die `WKInterfaceController.OpenParentApplication` in Ihrer Watch-app-Erweiterung. Gibt eine `bool` , der angibt, ob es sich bei den GET-Anforderung gesendet wurde, erfolgreich oder nicht. Überprüfen Sie die `error` Parameter in der Antwort `Action` bestimmen, ob alle ist aufgetreten, während der Ausführung der Methode, die in der iPhone-app ausgeführt wird.

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


### <a name="ios-app"></a>iOS-App

Alle Aufrufe eine Watch-app-Erweiterung über der iPhone-app weitergeleitet `HandleWatchKitExtensionRequest` Methode.
Wenn diese Methode müssen Abfragen nutzen Sie unterschiedliche Anforderungen in der Watch-app die `userInfo` Wörterbuch, um zu bestimmen, wie die Anforderung zu verarbeiten.


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

Wenn Sie konfigurieren eine [app-Gruppe](~/ios/watchos/app-fundamentals/app-groups.md) und iOS 8-Erweiterungen (einschließlich der Watch-Erweiterungen) Daten mit der übergeordneten Anwendung gemeinsam nutzen können.

<a name="nsuserdefaults" />

### <a name="nsuserdefaults"></a>NSUserDefaults

Der folgende Code kann in der Watch-app-Erweiterung und der übergeordneten iPhone-app geschrieben werden, sodass sie einen gemeinsamen Satz von verweisen können `NSUserDefaults`:

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

Die iOS-app "und" Überwachen-Erweiterung kann auch Dateien mit einem gemeinsamen Dateipfad freigeben.

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =
            FileManager.GetContainerUrl ("group.com.your-company.watchstuff");
var appGroupContainerPath = appGroupContainer.Path;
Console.WriteLine ("agcpath: " + appGroupContainerPath);
// use the path to create and update files
```

Hinweis: Wenn der Pfad ist `null` überprüfen Sie dann die [app-Gruppenkonfiguration](~/ios/watchos/app-fundamentals/app-groups.md) sicherstellen, dass die bereitstellungsprofile richtig konfiguriert wurden und auf dem Entwicklungscomputer heruntergeladen und installiert wurden.

Weitere Informationen finden Sie unter den [App-Gruppen-Funktionen](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) Dokumentation.

## <a name="wormholesharp"></a>WormHoleSharp

Eine beliebte Open-Source-Mechanismus für WatchOS 1 (basierend auf [MMWormHole](https://github.com/mutualmobile/MMWormhole)), Daten- oder befehlsbindungen zwischen der übergeordneten-app und der Watch-app übergeben.

Sie können "wormhole", die über eine app-Gruppe wie folgt in Ihre iOS-app konfigurieren und überwachen die Erweiterung:

```csharp
// AppDelegate (iOS) or InterfaceController (watch extension)
Wormhole wormHole;
// ...
wormHole = new Wormhole ("group.com.your-company.watchstuff", "messageDir");
```

Herunterladen der C# Version [WormHoleSharp](https://github.com/Clancey/WormHoleSharp).



## <a name="related-links"></a>Verwandte Links

- [GpsWatch (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [WormHoleSharp (Beispiel)](https://github.com/Clancey/WormHoleSharp)
- [Apple WKInterfaceController Verweis](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:)
- [Apple Freigeben von Daten mit Ihrer App mit](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
