---
redirect_url: /xamarin/tools/live-player/
title: XAML-Livevorschau
description: Dieses Dokument erläutert, wie Sie mit dem Xamarin Live Player live Preview XAML-Seiten, nehmen Sie Änderungen an der XAML und die Änderungen sofort auf Gerät angezeigt wird.
ms.prod: xamarin
ms.assetid: 86E9A179-21F8-4F3A-A9CE-36F0FC5DB4A8
author: lobrien
ms.author: laobri
ms.date: 08/08/2018
ms.openlocfilehash: 1602c98eceaff607c79400a37c4ace60d5bf8807
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110595"
---
# <a name="xaml-live-previewing"></a>XAML-Livevorschau

Einer der Vorteile von Xamarin Live Player ist die Möglichkeit, live-Vorschau-XAML-Seiten, nehmen Sie Änderungen an den Code in Visual Studio und die Änderungen sofort auf Ihrem Gerät angezeigt werden. Die Livevorschau kann auf Ihrem Android-Gerät oder auf einem Simulator oder Emulator erfolgen. Diese Anleitung veranschaulicht, wie Sie mit der Livevorschau einzelne XAML-Bildschirme anzeigen.

## <a name="requirements"></a>Anforderungen

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Ein Computer, die Windows 7 oder höher ausgeführt wird.
2. Visual Studio 2017 Version 15.4 oder höher mit der **Mobile Entwicklung mit .NET** arbeitsauslastung installiert.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Einen Mac mit OS X 10.11, MacOS Sierra 10.12 oder höher.
2. Visual Studio für Mac 7.2 oder höher. Es wird empfohlen, die neueste Version.

-----

<a name="deploydevice" />

## <a name="deploying-to-device"></a>Bereitstellung auf Gerät

Bevor Sie den Xamarin Live Player mit Ihrem Android-Gerät verwenden können, müssen Sie die Xamarin Live Player-app herunterladen und verbinden Sie es in Visual Studio, wie in beschrieben die [installieren](~/tools/live-player/install.md) Guide. Nachdem Sie Ihr Gerät zu Visual Studio erfolgreich gekoppelt haben, können Sie die live-Vorschau Ihrer XAML-Seite beginnen. 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Öffnen Sie die XAML-Seite, die Sie live-Vorschau im Editor für Visual Studio 2017 möchten:

    ![](live-view-images/vs-image1.png)

2. Legen Sie die Gerätekonfiguration auf **Debuggen** , und wählen Sie aus der Liste der Live Player-Gerät:

    ![](live-view-images/vs-image2.png)

3. Wählen Sie zum Ausführen dieser XAML-Seite als eine Liveansicht auf Ihrem Gerät **Tools > Xamarin Live Player > aktuelle Ansicht der Liveausführung** in der Menüleiste:

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Öffnen Sie die XAML-Seite, die Sie live-Vorschau in Visual Studio für Mac-Editor möchten:

    ![](live-view-images/image1.png)

2. Legen Sie die Gerätekonfiguration auf **Debuggen** , und wählen Sie aus der Liste der Live Player-Gerät:

    ![](live-view-images/image2.png)

3. Wählen Sie zum Ausführen dieser XAML-Seite als eine Liveansicht auf Ihrem Gerät **ausführen > aktuelle Ansicht der Liveausführung** in der Menüleiste:

    ![](live-view-images/image3.png)

-----

## <a name="deploying-to-android-emulator"></a>Für Android-Emulator bereitstellen

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Öffnen Sie die XAML-Seite, die Sie live-Vorschau im Editor für Visual Studio 2017 möchten:

    ![](live-view-images/vs-image1.png)

2. Legen Sie die Gerätekonfiguration auf **Debuggen** für Android, und wählen Sie aus der Liste der Live Player-Gerät:

    ![](live-view-images/vs-image4.png)

3. Wählen Sie zum Ausführen dieser XAML-Seite als eine Liveansicht der Android-Emulator **Tools > Xamarin Live Player > aktuelle Ansicht der Liveausführung** in der Menüleiste:

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Öffnen Sie die XAML-Seite, die Sie live-Vorschau in Visual Studio für Mac-Editor möchten:

    ![](live-view-images/image7.png)

2. Legen Sie die Gerätekonfiguration auf **Debuggen** für Android, und wählen Sie aus der Liste der Live Player-Gerät:

    ![](live-view-images/image6.png)

3. Wählen Sie zum Ausführen dieser XAML-Seite als eine Liveansicht auf Ihrem Gerät ausführen > aktuelle Ansicht der Liveausführung in der Menüleiste:

    ![](live-view-images/image3.png)

-----

## <a name="related-links"></a>Verwandte Links

- [Überblick zu Xamarin Live Player](https://xamarin.com/live)
- [Blogbeitrag](https://blog.xamarin.com/live-player/)
- [Xamarin Live Player-Beispiele](~/tools/live-player/samples.md)
