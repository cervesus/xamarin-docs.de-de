---
title: Beginnen Sie mit IOS 13, ipados 13, tvos 13 und watchos 6
description: In diesem Dokument wird beschrieben, wie Sie das Einrichten von IOS 13-, ipados 13-, tvos 13-und watchos 6-apps mit xamarin einrichten. Darin wird erläutert, wie Sie Xcode 11 herunterladen und Visual Studio für Mac aktualisieren.
ms.prod: xamarin
ms.assetid: 97414545-85D2-433C-9246-63B6763F488A
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/01/2019
ms.openlocfilehash: 9dc6f234c4bc14c3644d953eef0d2e0f397436e5
ms.sourcegitcommit: 61a35d0643eb3bf5adb8f8831da54771d8dde626
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2019
ms.locfileid: "71033074"
---
# <a name="get-started-with-ios-13"></a>Einstieg in ios 13

![Feature der Vorschauversion](~/media/shared/preview.png)

In diesem Dokument wird beschrieben, wie Sie mit dem Entwickeln von xamarin-apps beginnen, die mit Xcode 11 (IOS 13) veröffentlichte APIs aufzurufen. Die Verwendung der Vorschau erfordert macOS-10.14.4 (mujave) oder eine neuere Version.

## <a name="download-and-install"></a>Herunterladen und installieren

1. **Installieren von Xcode 11 Beta** – registrierte Apple-Entwickler können die neueste Version von Xcode 11 Beta aus dem Apple- [Entwickler Portal](https://developer.apple.com/download/) oder dem **App Store**herunterladen und installieren.

2. **Ausführen von Xcode 11 Beta** – Ausführen von Xcode 11 vor dem Aktualisieren und Ausführen von Visual Studio für Mac, da einige von xamarin erforderliche Tools installiert werden.

3. Wählen Sie in Visual Studio für Mac **Visual Studio > nach Updates suchen... aus**, wählen Sie den **Xcode 11-Vorschau** Kanal aus, und installieren Sie die verfügbaren Updates. Wenn dieser Kanal nicht angezeigt wird, stellen Sie sicher, dass Sie bei Ihrem Konto über **Visual Studio > Konto**angemeldet sind....

4. Wählen Sie in Visual Studio für Mac **Visual Studio > Einstellungen > Projekte > SDK-Speicherorte > Apple** aus, und wählen Sie **Xcode-Beta. app**aus.

5. Optionale Wenn Sie diese Vorschau mithilfe von _Xcode 11 Beta 3_evaluieren, müssen Sie die Verknüpfung aktivieren. Klicken Sie mit der rechten Maustaste auf das Projekt, navigieren Sie zu **Optionen > IOS-Build > linkerverhalten** , und legen Sie den Wert des **linkerverhaltens auf nur frameworksdi** Diese Problem Umgehung wird in einer zukünftigen Vorschau nicht benötigt.

6. Optionale **Installieren von IOS 13 auf Ihren IOS-Geräten** – für Gerätetests von apps, die mit Xcode 11 eingeführte APIs verwenden, können registrierte Apple-Entwickler das Betriebssystem auf Ihren Geräten [herunterladen](https://developer.apple.com/download) und installieren. Da sich IOS in der Beta Version befindet, achten Sie darauf, die Installation auf Ihrem primären Gerät durchzuführen.

   > [!TIP]
   > Auch wenn Ihre APP keine neuen APIs verwendet, stellen Sie sicher, dass Sie mit den neuesten Xcode 11-sdys erstellt werden, und testen Sie Sie, um sicherzustellen, dass alles wie erwartet funktioniert. Wenn eine APP keine neuen APIs aufruft, können Sie Sie mit diesen neuen sdchen neu kompilieren und auf Geräten testen, für die noch kein Upgrade auf das neue Betriebssystem durchgeführt wurde.
   >
   > Bevor Sie Ihre Geräte auf die neuesten Betriebssystemversionen von Apple aktualisieren, um Ihre xamarin-apps zu testen, stellen Sie Folgendes sicher:
   >
   > - Lesen Sie die Anmerkungen zu dieser [Version](https://developer.apple.com/download/) für die Betriebssystemupdates.

## <a name="related-links"></a>Verwandte Links

- [XCode herunterladen](https://developer.apple.com/download/)
- [Anmerkungen zu dieser Version von xamarin. IOS Preview](/xamarin/ios/release-notes/12/12.99)
