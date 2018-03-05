---
title: "Anforderungen für Xamarin.Forms"
description: "Plattform- und Systemanforderungen für Xamarin.Forms"
ms.topic: article
ms.prod: xamarin
ms.assetid: eecaf6a5-567c-49b2-ac83-2a195596c5bf
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/19/2017
ms.openlocfilehash: 2eaf4c6180b51a827d8182d87ee2db0fd1726c8d
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-requirements"></a>Anforderungen für Xamarin.Forms

_Plattform- und Entwicklungssystemanforderungen für Xamarin.Forms_

Im Artikel [Installation](~/cross-platform/get-started/installation/index.md) finden Sie eine Übersicht über die Installations- und Setupmethoden, die plattformübergreifend gelten.

## <a name="target-platforms"></a>Zielplattformen

Xamarin.Forms-Anwendungen können für die folgenden Betriebssysteme geschrieben werden:

-  iOS 8 oder höher
-  Android 4.0.3 (API 15) oder höher ([weitere Informationen](#android))
-  Windows 10 Universelle Windows-Plattform ([weitere Informationen](#windows10))
-  Windows 8.1/Windows Phone 8.1 WinRT ([weitere Informationen](#windows))
-  *Windows Phone 8 Silverlight (VERALTET)*

Es wird vorausgesetzt, dass Entwickler Kenntnisse in [portablen Klassenbibliotheken](~/cross-platform/app-fundamentals/pcl.md) und [freigegebenen Projekten](~/cross-platform/app-fundamentals/shared-projects.md) haben.

<a name="android" />

### <a name="android"></a>Android

Auf Ihren Geräten sollten die neuesten Android SDK Tools und die entsprechende Android-API-Plattform installiert sein. Sie können mithilfe des [Android SDK-Managers](~/android/get-started/installation/android-sdk.md) ein Update auf die neuesten Versionen ausführen.

Außerdem **muss** die Ziel-/Kompilierversion für Android-Projekte auf *Zuletzt installierte Plattform verwenden* festgelegt werden. Die Mindestversion kann jedoch auf API 15 festgelegt werden, damit die Unterstützung von Geräten mit Android 4.0.3 und höher weiterhin möglich ist. Diese Werte werden in den **Projektoptionen** festgelegt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**Projektoptionen > Anwendung > Anwendungseigenschaften**

![](installation-images/options-android-vs-sml.png "Buildoptionen für Android in Visual Studio")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

**Erstellen > Allgemein**

![](installation-images/options-general-sml.png "Erstellen > Allgemein")

**Erstellen > Android-Anwendung**

![](installation-images/options-android-sml.png "Erstellen > Android-Anwendung")

-----


<a name="windows10" />

### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Windows 10-UWP-Projekte werden nicht hinzugefügt, wenn eine Projektmappe unter macOS erstellt wird. Eine Anleitung zum Hinzufügen dieser Projekte zu einer vorhandenen Projektmappe finden Sie unter [Application Properties (Hinzufügen einer universellen Windows-Plattform-App (UWP))](~/xamarin-forms/platform/windows/installation/universal.md).


<a name="windows" />

### <a name="windows-81--windows-phone-81-winrt"></a>Windows 8.1/Windows Phone 8.1 WinRT

Windows 8.1/Windows Phone 8.1 WinRT-Projekte werden nicht hinzugefügt, wenn eine Projektmappe unter macOS erstellt wird. Eine Anleitung zum Hinzufügen dieser Projekte zu einer vorhandenen Projektmappe finden Sie unter [Adding a Windows Phone App (Hinzufügen einer Windows Phone-App)](~/xamarin-forms/platform/windows/installation/phone.md) und [Adding a Windows App (Hinzufügen einer Windows-App)](~/xamarin-forms/platform/windows/installation/tablet.md).


## <a name="development-system-requirements"></a>Systemanforderungen für die Bereitstellung

Xamarin.Forms-Apps können unter macOS und Windows entwickelt werden. Mit Windows und Visual Studio können jedoch nur die Windows-Versionen der App erstellt werden.

## <a name="mac-system-requirements"></a>Systemanforderungen für Mac

Mit Visual Studio für Mac lassen sich Xamarin.Forms-Apps für OS X El Capitan (10.11) oder höher entwickeln. Zum Entwickeln von iOS-Apps werden mindestens das iOS 10 SDK und die Installation von Xcode 8 empfohlen.

> [!NOTE]
>  Windows-Apps können nicht unter macOS entwickelt werden.

## <a name="windows-system-requirements"></a>Systemanforderungen für Windows

Xamarin.Forms-Apps für iOS und Android können auf allen Windows-Installationen erstellt werden, die die Xamarin-Entwicklung unterstützen. Dies erfordert Visual Studio 2013 Update 2 oder höher unter Windows 7 oder höher. Für die iOS-Entwicklung ist ein vernetzter Mac erforderlich.

Es gelten zusätzliche Anforderungen für die folgenden Typen von Windows-Apps:

### <a name="universal-windows-platform-uwp"></a>Universelle Windows-Plattform (UWP)

Das Entwickeln von Xamarin.Forms-Apps für die universelle Windows-Plattform erfordert Folgendes:

* Windows 10

* Visual Studio 2015 oder höher

* [Universelle Windows-Entwicklertools](https://dev.windows.com/downloads/windows-10-sdk)

UWP-Projekte sind in Xamarin.Forms-Projektmappen enthalten, die in Visual Studio 2015 und 2017 erstellt wurden.
Sie können einer vorhandenen Xamarin.Forms-Projektmappe auch eine [universelle Windows-Plattform-App (UWP)](~/xamarin-forms/platform/windows/installation/universal.md) hinzufügen.



### <a name="windows-81-and-windows-phone-81-winrt"></a>Windows 8.1- und Windows Phone 8.1 WinRT

Das Entwickeln von Xamarin.Forms-Apps für Windows 8.1 und Windows Phone 8.1 WinRT erfordert Folgendes:

* Windows 8.1

* Visual Studio 2013 Update 2 oder höher
