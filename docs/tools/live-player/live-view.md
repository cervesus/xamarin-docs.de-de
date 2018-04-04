---
redirect_url: /xamarin/tools/live-player/
title: XAML Live-Vorschau
description: Testen von Änderungen am Code der app in Echtzeit auf Ihrem IOS- oder Android-Gerät
ms.prod: xamarin
ms.assetid: 86E9A179-21F8-4F3A-A9CE-36F0FC5DB4A8
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 12/21/2017
ms.openlocfilehash: 721266e1ddfe927b33529a9f4d0eb55a008dd4e8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="xaml-live-previewing"></a>XAML Live-Vorschau

Einer der Vorteile von Xamarin Player Live ist die Fähigkeit, live Preview XAML-Seiten, nehmen Sie Änderungen an den Code in Visual Studio und die Änderungen werden sofort auf Ihrem Gerät angezeigt. Die live-Vorschau kann auf Ihrem IOS- oder Android-Gerät oder auf einem Simulator oder Emulator vorgenommen werden. Dieses Handbuch veranschaulicht, wie die live-Vorschau-Funktion verwenden, um einzelne XAML-Bildschirme anzeigen.

## <a name="requirements"></a>Anforderungen

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Ein Computer unter Windows 7 oder höher.
2. Visual Studio 2017 15.4 oder höher mit der **Mobile Entwicklung mit .NET** arbeitsauslastung installiert.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Einen Mac mit OS X 10.11, MacOS 10.12 oder höher.
2. Visual Studio für Mac 7.2 oder höher. Es wird empfohlen, die neueste Version.

-----



<a name="deploydevice" />

## <a name="deploying-to-device"></a>Bereitstellen auf Gerät

Bevor Sie die Xamarin-Live-Player mit Ihres IOS- oder Android-Gerät verwenden können, müssen Sie die Live-Player Xamarin-app herunterladen und verbinden Sie es in Visual Studio, wie in beschrieben die [installieren](~/tools/live-player/install.md) Handbuch. Nachdem Sie Ihr Gerät zu Visual Studio erfolgreich gekoppelt haben, können Sie die live-Vorschau der Verwendung von XAML-Seite beginnen. 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Öffnen Sie die XAML-Seite, die Sie live-Vorschau im Visual Studio-2017 Editor möchten:

    ![](live-view-images/vs-image1.png)

2. Legen Sie die Gerätekonfiguration auf **Debuggen | iPhone** für iOS oder **Debuggen** für Android, und wählen Sie die Live-Player-Gerät aus der Liste:

    ![](live-view-images/vs-image2.png)

3. Wählen Sie zum Ausführen dieser XAML-Seite als eine aktive Ansicht auf Ihrem Gerät **Tools > Xamarin Live Player > Führen Sie aktuellen Ansicht Live** in der Menüleiste:

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Öffnen Sie die XAML-Seite, die Sie Livevorschau in Visual Studio für Mac-Editor möchten:

    ![](live-view-images/image1.png)

2. Legen Sie die Gerätekonfiguration auf **Debuggen | iPhone** für iOS oder **Debuggen** für Android, und wählen Sie die Live-Player-Gerät aus der Liste:

    ![](live-view-images/image2.png)

3. Wählen Sie zum Ausführen dieser XAML-Seite als eine aktive Ansicht auf Ihrem Gerät **ausführen > Führen Sie aktuellen Ansicht Live** in der Menüleiste:

    ![](live-view-images/image3.png)

-----








## <a name="deploying-to-android-emulator"></a>Auf Android-Emulator bereitstellen

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Öffnen Sie die XAML-Seite, die Sie live-Vorschau im Visual Studio-2017 Editor möchten:

    ![](live-view-images/vs-image1.png)

2. Legen Sie die Gerätekonfiguration auf **Debuggen** für Android, und wählen Sie die Live-Player-Gerät aus der Liste:

    ![](live-view-images/vs-image4.png)

3. Wählen Sie zum Ausführen dieser XAML-Seite als eine aktive Ansicht auf dem Android-Emulator **Tools > Xamarin Live Player > Live ausführen aktuelle Ansicht** in der Menüleiste:

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Öffnen Sie die XAML-Seite, die Sie Livevorschau in Visual Studio für Mac-Editor möchten:

    ![](live-view-images/image7.png)

2. Legen Sie die Gerätekonfiguration auf **Debuggen** für Android, und wählen Sie die Live-Player-Gerät aus der Liste:

    ![](live-view-images/image6.png)

3. Wählen Sie zum Ausführen dieser XAML-Seite als eine aktive Ansicht auf Ihrem Gerät ausführen > Live ausführen aktuelle Ansicht in der Menüleiste:

    ![](live-view-images/image3.png)

-----





## <a name="deploying-to-ios-simulator"></a>Bereitstellen auf iOS-Simulator

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Zurzeit besteht keine Unterstützung für die Verwendung von live-XAML-Vorschau auf den Remote-iOS-Simulator unter Windows. Sie sollten stattdessen [auf einem Gerät bereitstellen](#deploydevice).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Öffnen Sie die XAML-Seite, die Sie Livevorschau in Visual Studio für Mac-Editor möchten:

    ![](live-view-images/image1.png)

2. Legen Sie die Gerätekonfiguration auf **Debuggen | iPhoneSimulator** für iOS, und wählen Sie ein iOS-Simulator aus der Liste:

    ![](live-view-images/image2.png)

3. Wählen Sie **ausführen > Führen Sie aktuellen Ansicht Live** in der Menüleiste, starten Sie den Simulator und zeigen die XAML-Seite:

    ![](live-view-images/image4.png)

4. Nachdem der Simulator gestartet wurde, können Sie beginnen, den XAML-Code bearbeiten und Anzeigen einer Vorschau live angezeigt werden:

    ![](live-view-images/image5.png)  

-----








## <a name="related-links"></a>Verwandte Links

- [Überblick zu Xamarin Player Live](https://xamarin.com/live)
- [Blogbeitrag](https://blog.xamarin.com/live-player/)
- [Xamarin Player Live-Beispiele](~/tools/livehttps://developer.xamarin.com/samples.md)
