---
title: Mehrere Windows für iPad in xamarin. IOS
description: Fügen Sie Ihren iPad-Anwendungen Unterstützung mehrerer Fenster hinzu.
ms.prod: xamarin
ms.assetid: 524b6f2e-dbdf-11e9-8a34-2a2ae2dbcce4
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/20/2019
ms.openlocfilehash: b3f96f342679d8be6d2f8fbc0ad5962ca675d2a5
ms.sourcegitcommit: 09bc69d7119a04684c9e804c5cb113b8b1bb7dfc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2019
ms.locfileid: "71213804"
---
# <a name="multiple-windows-for-ipad"></a>Mehrere Fenster für iPad

IOS 13 unterstützt jetzt parallele Fenster für dieselbe App auf dem iPad. Dies ermöglicht neue Benutzeroberflächen und Drag & amp; Drop-Interaktionen zwischen Fenstern. In diesem Dokument wird gezeigt, wie Sie Ihre Anwendung zur Unterstützung dieses Features einrichten und diese neuen Features einführen. 

## <a name="project-configuration"></a>Projekt Konfiguration

Wenn Sie Ihr Projekt für mehrere Fenster konfigurieren möchten, `info.plist` ändern Sie `NSUserActivityTypes` den so, dass er mitteilt, dass Ihre APP Aktivitäten dieses `UIApplicationSceneManifest` Typs verarbeitet `UIApplicationSupportsMultipleScenes` und für mehrere Fenster `UISceneConfigurations` aktiviert und ihre Szene mit einem Storyboard.

```xml
<key>NSUserActivityTypes</key>
<array>
    <string>com.xamarin.Gallery.openDetail</string>
</array>
<key>UIApplicationSceneManifest</key>
<dict>
    <key>UIApplicationSupportsMultipleScenes</key>
    <true/>
    <key>UISceneConfigurations</key>
    <dict>
        <key>UIWindowSceneSessionRoleApplication</key>
        <array>
            <dict>
                <key>UISceneConfigurationName</key>
                <string>Default Configuration</string>
                <key>UISceneDelegateClassName</key>
                <string>SceneDelegate</string>
                <key>UISceneStoryboardFile</key>
                <string>Main</string>
            </dict>
        </array>
    </dict>
</dict>
```

## <a name="opening-multiple-windows"></a>Öffnen mehrerer Fenster

Wenn Ihre APP geöffnet ist und auf einem iPad ausgeführt wird, gibt es mehrere Möglichkeiten, mehrere Fenster dieser APP zu öffnen, die IOS-Handles als Standard behandelt.

- **App** verfügbar: Tippen Sie auf das App-Symbol im Dock, um in diesen Modus zu wechseln, während Ihre APP geöffnet ist.
- **Schieben** Sie das App-Symbol von der Andockstelle über den oberen Rand ihrer laufenden app, um ein unverankertes Fenster zu haben.
- **Bildschirm aufteilen** : ziehen Sie das App-Symbol von der Andockstelle an den Rand des iPad-Bildschirms, um ein neues Fenster nebeneinander zu erstellen.

Weitere Interaktionen zur Eingabe eines Modus für mehrere Fenster sind in der Anwendung verfügbar.

Per Drag & amp; Drop **from App** : Verwenden Sie eine Drag-Interaktion `NSUserActivity` innerhalb Ihrer APP, um ein neues-Symbol zu starten, wie in den vorherigen Beispielen das Symbol

Wenn Sie eine Drag & amp; [Drop-Interaktion][0]verwenden, erstellen `NSUserActivity` Sie ein und ordnen die zu über gebenden Daten an das neue Fenster zu, das Sie für die IOS-Abfrage auffordern.

```csharp
public UIDragItem [] GetItemsForBeginningDragSession (UICollectionView collectionView, IUIDragSession session, NSIndexPath indexPath)
{
    var selectedPhoto = GetPhoto (indexPath);

    var userActivity = selectedPhoto.OpenDetailUserActivity ();
    var itemProvider = new NSItemProvider (UIImage.FromFile (selectedPhoto.Name));
    itemProvider.RegisterObject (userActivity, NSItemProviderRepresentationVisibility.All);

    var dragItem = new UIDragItem (itemProvider) {
        LocalObject = selectedPhoto
    };

    return new [] { dragItem };
}
```

Im obigen Code verfügt das Model `selectedPhoto` -Objekt über eine-Methode, um `NSUserActivity` eine `OpenDetailUserActivity()`mit dem Namen zurückzugeben. Wenn die Zieh Bewegung fertig ist, stellt `UIDragItem` das mit dem `userActivity` die über `NSItemProvider`die dar.

**Explizite Aktionen** : Benutzer Gesten auf Schaltflächen oder Links bieten die Möglichkeit, ein neues Fenster zu öffnen.

Im `UIApplication` können Sie einen neuen `UISceneSession` starten, indem Sie `RequestSceneSessionActivation`aufrufen. Wenn bereits eine vorhandene Szene vorhanden ist, sollten Sie diese verwenden. Standardmäßig wird eine neue Szene für Sie erstellt.

```csharp
pubic void ItemSelected(UICollectionView collectionView, NSIndexPath indexPath)
{
    var userActivity = selectedPhoto.OpenDetailUserActivity ();

    UIApplication.SharedApplication.RequestSceneSessionActivation(
        sceneSession: null,
        userActivity: userActivity,
        options: null,
        errorHandler: null
    );
}
```

In diesem Beispiel ist der `userActivity` der einzige Wert, der an die `RequestSceneSessionActivation` -Methode weitergegeben wird, um ein neues Fenster der Anwendung auf der Grundlage einer expliziten Benutzeraktion zu öffnen `ItemSelected` `UICollectionView`. in diesem Fall handelt es sich um einen-Handler.

## <a name="related-links"></a>Verwandte Links

- [Drag & Drop in xamarin. IOS][0]

[0]: ~/ios/platform/introduction-to-ios11/drag-and-drop.md
