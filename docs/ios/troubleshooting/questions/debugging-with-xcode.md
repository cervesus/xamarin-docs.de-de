---
title: Debuggen von Xamarin.iOS-apps mit Xcode
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5FDDEDB3-AEB9-4D9C-9F7B-FEFAA9AF0031
ms.technology: xamarin-ios
author: migueldeicaza
ms.author: miguel
ms.date: 02/22/2018
ms.openlocfilehash: e0127d4b24236d350e5fa967110316544c320d0f
ms.sourcegitcommit: bf05041cc74fb05fd906746b8ca4d1403fc5cc7a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514707"
---
# <a name="debugging-xamarinios-apps-with-xcode"></a>Debuggen von Xamarin.iOS-apps mit Xcode

Es gibt möglicherweise Szenarien, in dem Sie Xcode zu verwenden, um Teile Ihrer Xamarin.iOS-Anwendung debuggen möchten. Sie sind nicht beim Debuggen von .NET Code, werden Sie immer noch in der Lage zum Debuggen von nativem Code und verwendet die systemeigenen Schnellansichten in Xcode.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Es gibt zwar keine integrierte Unterstützung für Xcode Debuggen in Visual Studio für Mac, können Sie die folgenden Schritte aus, um dies zu erreichen:

1. Erstellen Sie eine iOS-app mit Xcode, mit der gleichen Paket-ID wie in Ihrer Xamarin-app.
   
    - Bündelbezeichner Ihres Xamarin.iOS-Projekts finden Sie dazu die **"Info.plist"** Datei:

        ![Bearbeiten Sie die Datei "Info.plist"](debugging-with-xcode-images/vsmac-infoplist.png "Info.list bearbeiten")

    - Legen Sie in Xcode die Bündel-ID beim Erstellen des Projekts, oder indem Sie Ihr Ziel im Projekt auswählen:

        ![Festlegen der Bündel-ID in Xcode](debugging-with-xcode-images/xcode-bundle.png "Festlegen der Bündel-ID in Xcode")

2. Ändern des Xcode-Projekts starten, anstatt automatisch beim Starten der app gewartet:

    - Öffnen der **-Schema-Bereich bearbeiten** dazu **Product > Schema > Schema bearbeiten** oder mithilfe der **Cmd⌘ + <** Tastenkombination.

    - Wählen Sie die **ausführen** Schema und im rechten Bereich können Sie sollte **starten** Optionen. Wählen Sie **warten, bis zu startenden ausführbaren Datei** , und klicken Sie auf **schließen**.

        ![Warten Sie, bis zu startenden ausführbaren Datei](debugging-with-xcode-images/xcode-schemes.png "warten, bis zu startenden ausführbaren Datei")

3. Führen Sie das Xcode-Projekt.

    Dadurch wird die dummy-Xcode-app auf Ihrem Gerät installiert, aber wird nicht gestartet.

4. Führen Sie die Xamarin-app.

    Xcode sollte an die Xamarin-app anfügen, wenn er gestartet wird.

### <a name="caveats"></a>Hinweise

Möglicherweise müssen Sie für die Xamarin.iOS-app eine kleine Änderung vornehmen, jedes Mal, wenn Sie starten. Andernfalls, Visual Studio für Mac erkennt, dass die app nicht erstellt werden muss, *und* bereits installiert ist, und es wird nicht über die Xcode-dummy-app neu.

## <a name="alternative---using-lldb"></a>Alternative – verwenden lldb

Wenn Sie mit der Verwendung vertraut sind **Lldb** über die Befehlszeile, besteht eine viel einfachere Lösung.

Geben Sie in der Shell den folgenden Befehl ein:

```bash
touch ~/.mtouch-launch-with-lldb
```

Sie erhalten Anweisungen in der **Anwendungsausgabe** -Fenster auf die Vorgehensweise, aber grundsätzlich gilt beim Ausführen der Anwendung, Sie werden mit **Lldb** über die Befehlszeile zum Debuggen Ihrer Anwendung.
