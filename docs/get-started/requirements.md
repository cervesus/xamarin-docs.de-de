---
title: Xamarin. Formular Anforderungen
description: Anforderungen an die Plattform und das Entwicklungssystem für Xamarin. Forms.
ms.prod: xamarin
ms.assetid: eecaf6a5-567c-49b2-ac83-2a195596c5bf
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/16/2019
no-loc:
- Xamarin
- Xamarin.Forms
- Xamarin.Android
- Xamarin.Essentials
- Xamarin.iOS
- Xamarin.Mac
ms.openlocfilehash: d12daa358917399fc5fd1febf02d4f96a647f360
ms.sourcegitcommit: 6f09bc2b760e76a61a854f55d6a87c4f421ac6c8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2020
ms.locfileid: "75607853"
---
# <a name="opno-locxamarinforms-requirements"></a>Xamarin. Formular Anforderungen

_Anforderungen an die Plattform und das Entwicklungssystem für Xamarin. Forms._

Im Artikel [Installation](installation/index.md) finden Sie eine Übersicht über die Installations- und Setupmethoden, die plattformübergreifend gelten.

## <a name="target-platforms"></a>Zielplattformen

Xamarin. Formular Anwendungen können für die folgenden Betriebssysteme geschrieben werden:

- IOS 9 oder höher
- Android 4.4 (API 19) oder höher ([weitere Informationen](#android))
- Windows 10 Universelle Windows-Plattform ([weitere Informationen](#windows10))

Android 5,0 (API 21) wird jedoch als minimale API empfohlen. Dadurch wird die vollständige Kompatibilität mit allen Android-Unterstützungs Bibliotheken sichergestellt, während die meisten Android-Geräte als Ziel verwendet werden.

Es wird davon ausgegangen, dass Entwickler Vertrautheit mit [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)haben.

### <a name="additional-platform-support"></a>Unterstützung für zusätzliche Plattformen

Der Status dieser Plattformen ist auf dem [Xamarinverfügbar. Formulare GitHub](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support):

- Samsung Tizen
- macOS
- GTK#
- WPF

### <a name="android"></a>Android

Auf Ihren Geräten sollten die neuesten Android SDK Tools und die entsprechende Android-API-Plattform installiert sein. Sie können mithilfe des [Android SDK-Managers](~/android/get-started/installation/android-sdk.md) ein Update auf die neuesten Versionen ausführen.

Außerdem **muss** die Ziel-/Kompilierversion für Android-Projekte auf *Zuletzt installierte Plattform verwenden* festgelegt werden. Die Mindestversion kann jedoch auf API 19 festgelegt werden, damit die Unterstützung von Geräten mit Android 4.4 und höher weiterhin möglich ist. Diese Werte werden in den **Projektoptionen** festgelegt:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

**Projektoptionen > Anwendung > Anwendungseigenschaften**

![Android-Buildoptionen in Visual Studio](requirements-images/options-android-vs-sml.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

**Erstellen > Allgemein**

![Wählen Sie das neueste Ziel Framework aus.](requirements-images/options-general-sml.png)

**Erstellen > Android-Anwendung**

![Wählen Sie mindestens die Android-Ziel Versionen für Ihre APP aus.](requirements-images/options-android-sml.png)

-----

## <a name="development-system-requirements"></a>Systemanforderungen für die Bereitstellung

Xamarin. Formular-Apps können unter macOS und Windows entwickelt werden. Mit Windows und Visual Studio können jedoch nur die Windows-Versionen der App erstellt werden.

## <a name="mac-system-requirements"></a>Systemanforderungen für Mac

Sie können Visual Studio für Mac verwenden, um Xamarinzu entwickeln. Formular-apps auf macOS High Sierra (10,13) oder höher. Zum Entwickeln von IOS-apps empfiehlt es sich, die neueste Version von Xcode, IOS und macOS zu verwenden. Informationen zu bestimmten Versions Anforderungen finden Sie in den neuesten [Xamarin. IOS](/xamarin/ios/release-notes/)-Versions Anmerkungen.

> [!NOTE]
> Windows-Apps können nicht unter macOS entwickelt werden.

## <a name="windows-system-requirements"></a>Systemanforderungen für Windows

Xamarin. Formular-Apps für IOS und Android können auf jeder Windows-Installation erstellt werden, die die Xamarin Entwicklung unterstützt. Verwenden Sie die neueste Version von Visual Studio, um die aktuellen Platt Form Features vollständig zu unterstützen. 

Ein vernetzter Mac ist für die IOS-Entwicklung mit der aktuellen Version von Xcode und der von Apple angegebenen Mindestversion von macOS erforderlich.

<a name="windows10" />

### <a name="universal-windows-platform-uwp"></a>Universelle Windows-Plattform (UWP)

Entwickeln von Xamarin. Für Forms-Apps für UWP ist Folgendes erforderlich:

- Windows 10 (neueste Version empfohlen, Fall Creators Update minimal)

- Visual Studio 2019 empfohlen

- [Windows 10 SDK](https://dev.windows.com/downloads/windows-10-sdk)

Sie können [eine universelle Windows-Plattform-app (UWP)](~/xamarin-forms/platform/windows/installation/index.md) zu einer vorhandenen Xamarinhinzufügen. Formular Projekt Mappe zu einem beliebigen Zeitpunkt.

## <a name="deprecated-platforms"></a>Veraltete Plattformen

Diese Plattformen werden bei Verwendung Xamarinnicht unterstützt. Formulare 3,0 oder höher:

- *Windows 8.1 / Windows Phone 8.1 WinRT*
- *Windows Phone 8 Silverlight*
