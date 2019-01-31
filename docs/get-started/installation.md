---
title: Anforderungen für Xamarin.Forms
description: Plattform- und Systemanforderungen für Xamarin.Forms
ms.prod: xamarin
ms.assetid: eecaf6a5-567c-49b2-ac83-2a195596c5bf
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/23/2018
ms.openlocfilehash: 504f20f6575e559d7c4965643b74b407d5e84de8
ms.sourcegitcommit: 817d26585093cd180a36b28179eb354b0eb900b3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55293029"
---
# <a name="xamarinforms-requirements"></a>Anforderungen für Xamarin.Forms

_Plattform- und Entwicklungssystemanforderungen für Xamarin.Forms_

Im Artikel [Installation](~/cross-platform/get-started/installation/index.md) finden Sie eine Übersicht über die Installations- und Setupmethoden, die plattformübergreifend gelten.

## <a name="target-platforms"></a>Zielplattformen

Xamarin.Forms-Anwendungen können für die folgenden Betriebssysteme geschrieben werden:

- iOS 8 oder höher
- Android 4.4 (API 19) oder höher ([weitere Informationen](#android))
- Windows 10 Universelle Windows-Plattform ([weitere Informationen](#windows10))

Es wird vorausgesetzt, dass Entwickler mit [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md) und [freigegebenen Projekten](~/cross-platform/app-fundamentals/shared-projects.md) vertraut sind.

### <a name="additional-platform-support"></a>Unterstützung für zusätzliche Plattformen

Der Status dieser Plattformen ist auf der [GitHub-Seite zu Xamarin.Forms](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support) verfügbar:

- Samsung Tizen
- macOS
- GTK#
- WPF

### <a name="platforms-from-earlier-versions"></a>Frühere Plattformversionen

Diese Plattformversionen werden bei der Verwendung von Xamarin.Forms 3.0 nicht unterstützt:

- *Windows 8.1 / Windows Phone 8.1 WinRT*
- *Windows Phone 8 Silverlight*

### <a name="android"></a>Android

Auf Ihren Geräten sollten die neuesten Android SDK Tools und die entsprechende Android-API-Plattform installiert sein. Sie können mithilfe des [Android SDK-Managers](~/android/get-started/installation/android-sdk.md) ein Update auf die neuesten Versionen ausführen.

Außerdem **muss** die Ziel-/Kompilierversion für Android-Projekte auf *Zuletzt installierte Plattform verwenden* festgelegt werden. Die Mindestversion kann jedoch auf API 19 festgelegt werden, damit die Unterstützung von Geräten mit Android 4.4 und höher weiterhin möglich ist. Diese Werte werden in den **Projektoptionen** festgelegt:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

**Projektoptionen > Anwendung > Anwendungseigenschaften**

![](installation-images/options-android-vs-sml.png "Buildoptionen für Android in Visual Studio")

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

**Erstellen > Allgemein**

![](installation-images/options-general-sml.png "Erstellen > Allgemein")

**Erstellen > Android-Anwendung**

![](installation-images/options-android-sml.png "Erstellen > Android-Anwendung")

-----

## <a name="development-system-requirements"></a>Systemanforderungen für die Bereitstellung

Xamarin.Forms-Apps können unter macOS und Windows entwickelt werden. Mit Windows und Visual Studio können jedoch nur die Windows-Versionen der App erstellt werden.

## <a name="mac-system-requirements"></a>Systemanforderungen für Mac

Mit Visual Studio für Mac lassen sich Xamarin.Forms-Apps für OS X El Capitan (10.11) oder höher entwickeln. Zum Entwickeln von iOS-Apps werden mindestens das iOS 10 SDK und die Installation von Xcode 8 empfohlen.

> [!NOTE]
>  Windows-Apps können nicht unter macOS entwickelt werden.

## <a name="windows-system-requirements"></a>Systemanforderungen für Windows

Xamarin.Forms-Apps für iOS und Android können auf allen Windows-Installationen erstellt werden, die die Xamarin-Entwicklung unterstützen. Hierfür ist Visual Studio 2017 oder höher unter Windows 7 oder höher erforderlich. Für die iOS-Entwicklung ist ein vernetzter Mac erforderlich.

<a name="windows10" />

### <a name="universal-windows-platform-uwp"></a>Universelle Windows-Plattform (UWP)

Das Entwickeln von Xamarin.Forms-Apps für die universelle Windows-Plattform erfordert Folgendes:

- Windows 10 (Fall Creators Update empfohlen)

- Visual Studio 2017

- [Windows 10 SDK](https://dev.windows.com/downloads/windows-10-sdk)

UWP-Projekte sind in Xamarin.Forms-Projektmappen enthalten, die in Visual Studio 2017 erstellt wurden. Nicht enthalten sind Projektmappen, die in Visual Studio für Mac erstellt wurden.
Sie können einer vorhandenen Xamarin.Forms-Projektmappe jederzeit eine [universelle Windows-Plattform-App (UWP)](~/xamarin-forms/platform/windows/installation/index.md) hinzufügen.
