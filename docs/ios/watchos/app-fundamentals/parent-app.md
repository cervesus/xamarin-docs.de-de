---
title: Arbeiten mit der übergeordneten watchos-Anwendung in xamarin
description: In diesem Dokument wird beschrieben, wie Sie eine übergeordnete watchos-Anwendung in xamarin arbeiten. Es werden watchkit-App-Erweiterungen, IOS-apps, frei gegebener Speicher und mehr erläutert.
ms.prod: xamarin
ms.assetid: 9AD29833-E9CC-41A3-95D2-8A655FF0B511
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 08637fe13b28565e7c6ab1c89b291c6db3b81025
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73001473"
---
# <a name="working-with-the-watchos-parent-application-in-xamarin"></a>Arbeiten mit der übergeordneten watchos-Anwendung in xamarin

> [!IMPORTANT]
> Der Zugriff auf die übergeordnete Anwendung in den folgenden Beispielen funktioniert nur bei watchos 1 Watch-apps.

Es gibt verschiedene Möglichkeiten für die Kommunikation zwischen der Watch-APP und der IOS-APP, mit der Sie gebündelt ist:

- Watch-Erweiterungen können eine Methode für die übergeordnete APP [aufzurufen](#code) , die im Hintergrund auf dem iPhone ausgeführt wird.

- Überwachungs Erweiterungen können [einen Speicherort](#storage) für die übergeordnete iPhone-App freigeben.

- Übergeben Sie mithilfe von Handoff Daten von einem Blick oder einer Benachrichtigung an die Watch-APP, und senden Sie den Benutzer an einen bestimmten Schnittstellen Controller in der app.

Die übergeordnete APP wird manchmal auch als Container-App bezeichnet.

<a name="code" />

## <a name="run-code"></a>Ausführen von Code

Die Kommunikation zwischen einer Watch-Erweiterung und der übergeordneten iPhone-App wird im [gpswatch-Beispiel](https://docs.microsoft.com/samples/xamarin/ios-samples/watchkit-gpswatch)veranschaulicht.
Mithilfe der `OpenParentApplication`-Methode kann die übergeordnete IOS-APP die übergeordnete IOS-App anweisen, einige Verarbeitungsschritte auszuführen.

Dies ist besonders nützlich für Aufgaben mit langer Ausführungszeit (einschließlich Netzwerk Anforderungen). nur die übergeordnete IOS-App kann die Hintergrundverarbeitung nutzen, um diese Aufgaben abzuschließen und die abgerufenen Daten an einem Speicherort zu speichern, auf den die Watch-Erweiterung Zugriff hat.

### <a name="watch-kit-app-extension"></a>Watch-Kit-App-Erweiterung

Nennen Sie den `WKInterfaceController.OpenParentApplication` in ihrer Watch-App-Erweiterung. Sie gibt einen `bool` zurück, der angibt, ob die Methoden Anforderung erfolgreich gesendet wurde. Überprüfen Sie den `error`-Parameter in der Antwort `Action`, um zu bestimmen, ob während der in der iPhone-App ausgeführt Methode aufgetreten ist.

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

### <a name="ios-app"></a>IOS-App

Alle Aufrufe von einer Watch-App-Erweiterung werden durch die `HandleWatchKitExtensionRequest`-Methode der iPhone-App weitergeleitet.
Wenn Sie in der Watch-App unterschiedliche Anforderungen vornehmen, muss diese Methode das `userInfo` Wörterbuch Abfragen, um zu bestimmen, wie die Anforderung verarbeitet werden soll.

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

## <a name="shared-storage"></a>Frei gegebener Speicher

Wenn Sie eine [App-Gruppe](~/ios/watchos/app-fundamentals/app-groups.md) konfigurieren, können IOS 8-Erweiterungen (einschließlich der Überwachungs Erweiterungen) Daten für die übergeordnete App freigeben.

<a name="nsuserdefaults" />

### <a name="nsuserdefaults"></a>NSUserDefaults

Der folgende Code kann in der Watch-App-Erweiterung und der übergeordneten iPhone-App geschrieben werden, sodass Sie auf einen allgemeinen Satz von `NSUserDefaults`verweisen können:

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

Die IOS-APP und die Watch-Erweiterung können auch Dateien mit einem gemeinsamen Dateipfad freigeben.

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =
            FileManager.GetContainerUrl ("group.com.your-company.watchstuff");
var appGroupContainerPath = appGroupContainer.Path;
Console.WriteLine ("agcpath: " + appGroupContainerPath);
// use the path to create and update files
```

Hinweis: Wenn der Pfad `null` ist, überprüfen Sie die [Konfiguration der APP-Gruppe](~/ios/watchos/app-fundamentals/app-groups.md) , um sicherzustellen, dass die Bereitstellungs profile ordnungsgemäß konfiguriert wurden und auf dem Entwicklungs Computer heruntergeladen bzw. installiert wurden.

Weitere Informationen finden Sie in der Dokumentation zu [App-Gruppenfunktionen](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) .

## <a name="wormholesharp"></a>Wormholesharp

Ein beliebter Open-Source-Mechanismus für watchos 1 (basierend auf [mmwormhole](https://github.com/mutualmobile/MMWormhole)) zum Übergeben von Daten oder Befehlen zwischen der übergeordneten APP und der Watch-app.

Sie können das Wormhole mithilfe einer APP-Gruppe wie dieser in ihrer IOS-APP und der Erweiterung "Watch" konfigurieren:

```csharp
// AppDelegate (iOS) or InterfaceController (watch extension)
Wormhole wormHole;
// ...
wormHole = new Wormhole ("group.com.your-company.watchstuff", "messageDir");
```

Laden Sie C# die Version [wormholesharp](https://github.com/Clancey/WormHoleSharp)herunter.

## <a name="related-links"></a>Verwandte Links

- [Gpswatch (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Wormholesharp (Beispiel)](https://github.com/Clancey/WormHoleSharp)
- [Wkinterfakecontroller-Referenz für Apple](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:)
- [Apple-Freigabe von Daten für Ihre enthaltende App](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
