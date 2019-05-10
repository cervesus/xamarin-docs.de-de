---
title: Anforderungen für Xamarin.Forms
description: Plattform- und Systemanforderungen für Xamarin.Forms
ms.prod: xamarin
ms.assetid: eecaf6a5-567c-49b2-ac83-2a195596c5bf
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/01/2019
ms.openlocfilehash: 89afb106320ce77e86a66f2c78bd6e32de8c38f3
ms.sourcegitcommit: be9658de032f3893741261f16162a664952ce178
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2019
ms.locfileid: "64986992"
---
# <a name="xamarinforms-requirements"></a>Anforderungen für Xamarin.Forms

_Plattform- und Entwicklungssystemanforderungen für Xamarin.Forms_

Im Artikel [Installation](installation/index.md) finden Sie eine Übersicht über die Installations- und Setupmethoden, die plattformübergreifend gelten.

## <a name="target-platforms"></a>Zielplattformen

Xamarin.Forms-Anwendungen können für die folgenden Betriebssysteme geschrieben werden:

- iOS 8 oder höher
- Android 5.0 (API 21) oder höher ([mehr](#android))
- Windows 10 Universelle Windows-Plattform ([weitere Informationen](#windows10))

Davon aus, dass Entwickler Kenntnisse verfügen [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md).

### <a name="additional-platform-support"></a>Unterstützung für zusätzliche Plattformen

Der Status dieser Plattformen ist auf der [GitHub-Seite zu Xamarin.Forms](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support) verfügbar:

- Samsung Tizen
- macOS
- GTK#
- WPF

### <a name="android"></a>Android

Auf Ihren Geräten sollten die neuesten Android SDK Tools und die entsprechende Android-API-Plattform installiert sein. Sie können mithilfe des [Android SDK-Managers](~/android/get-started/installation/android-sdk.md) ein Update auf die neuesten Versionen ausführen.

Außerdem **muss** die Ziel-/Kompilierversion für Android-Projekte auf *Zuletzt installierte Plattform verwenden* festgelegt werden. Die Mindestversion kann jedoch auf API 19 festgelegt werden, damit die Unterstützung von Geräten mit Android 4.4 und höher weiterhin möglich ist. Diese Werte werden in den **Projektoptionen** festgelegt:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

**Projektoptionen > Anwendung > Anwendungseigenschaften**

![Buildoptionen für Android in Visual Studio](requirements-images/options-android-vs-sml.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

**Erstellen > Allgemein**

![Wählen Sie das aktuelle Zielframework](requirements-images/options-general-sml.png)

**Erstellen > Android-Anwendung**

![Wählen Sie die niedrigste und Android-Versionen für Ihre app](requirements-images/options-android-sml.png)

-----

## <a name="development-system-requirements"></a>Systemanforderungen für die Bereitstellung

Xamarin.Forms-Apps können unter macOS und Windows entwickelt werden. Mit Windows und Visual Studio können jedoch nur die Windows-Versionen der App erstellt werden.

## <a name="mac-system-requirements"></a>Systemanforderungen für Mac

Sie können Visual Studio für Mac verwenden, zum Entwickeln von Xamarin.Forms-apps unter MacOS High Sierra (10.13) oder höher. Es wird empfohlen, zum Entwickeln von iOS-apps mit mindestens das iOS 10 SDK und Xcode 9 installiert.

> [!NOTE]
>  Windows-Apps können nicht unter macOS entwickelt werden.

## <a name="windows-system-requirements"></a>Systemanforderungen für Windows

Xamarin.Forms-Apps für iOS und Android können auf allen Windows-Installationen erstellt werden, die die Xamarin-Entwicklung unterstützen. Hierfür ist Visual Studio 2017 oder höher unter Windows 7 oder höher erforderlich. Für die iOS-Entwicklung ist ein vernetzter Mac erforderlich.

<a name="windows10" />

### <a name="universal-windows-platform-uwp"></a>Universelle Windows-Plattform (UWP)

Das Entwickeln von Xamarin.Forms-Apps für die universelle Windows-Plattform erfordert Folgendes:

- Windows 10 (neueste Version zu empfehlen, mindestens der Fall Creators Update)

- Visual Studio-2019 empfohlen (Visual Studio 2017 Version 15.8 minimale)

- [Windows 10 SDK](https://dev.windows.com/downloads/windows-10-sdk)

Sie können einer vorhandenen Xamarin.Forms-Projektmappe jederzeit eine [universelle Windows-Plattform-App (UWP)](~/xamarin-forms/platform/windows/installation/index.md) hinzufügen.

## <a name="deprecated-platforms"></a>Nicht mehr unterstützte Plattformen

Diese Plattformen werden nicht unterstützt, wenn Sie Xamarin.Forms, 3.0 oder höher verwenden:

- *Windows 8.1 / Windows Phone 8.1 WinRT*
- *Windows Phone 8 Silverlight*