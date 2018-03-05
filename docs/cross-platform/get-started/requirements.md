---
title: Systemanforderungen
description: "Voraussetzungen für die Verwendung von Xamarin"
ms.topic: article
ms.prod: xamarin
ms.assetid: dd344d57-18e2-42a5-8c15-3f5be4123c72
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 08/28/2017
ms.openlocfilehash: 024e73ddfe517f6fe9766607fa17efbd5703234c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="system-requirements"></a>Systemanforderungen

_Voraussetzungen für die Verwendung von Xamarin_

Xamarin-Produkte sind abhängig von den Plattform SDKs von Apple und Google, um iOS oder Android als Ziel verwenden zu können, sodass unsere Systemanforderungen ihren entsprechen. Auf dieser Seite werden die Systemkompatibilität für die Xamarin-Plattform und für die empfohlene Entwicklungsumgebung sowie die SDK-Versionen erläutert.

- [Development Environments (Entwicklungsumgebungen)](#devenv)
- [macOS Requirements (macOS-Anforderungen)](#mac)
- [Windows Requirements (Windows-Anforderungen)](#windows)

Weitere Informationen zum Abrufen der Software und der erforderlichen SDKs finden Sie unter [installation instructions (Installationsanweisungen)](#install).

<a name="devenv" />

## <a name="development-environments"></a>Entwicklungsumgebungen

Diese Tabelle zeigt, welche Plattformen mit verschiedenen Entwicklungstool- und Betriebssystemkombinationen erstellt werden können:

[!include[](~/cross-platform/includes/development-environment.html)]


> [!NOTE]
> Zur Entwicklung von iOS auf Windows-Computern muss für die Remotekompilierung und das Debuggen [ein Mac-Computer im Netzwerk verfügbar sein](~/ios/get-started/installation/windows/connecting-to-mac/index.md). Sie können Visual Studio auch innerhalb einer Windows-VM auf einem Mac-Computer ausführen.

<a name="mac" />

## <a name="macos-requirements"></a>macOS-Anforderungen

Die Verwendung eines Mac-Computers für die Xamarin-Entwicklung erfordert folgende Software-/SDK-Versionen. Überprüfen Sie Ihre Betriebssystemversion, und folgen Sie den Anweisungen für den [Xamarin-Installer](#install).

[!include[](~/cross-platform/includes/macos-requirements.html)]

> [!NOTE]
> HINWEIS: Xcode kann auf [developer.apple.com](https://developer.apple.com/xcode/download/) oder über den Mac App Store installiert (und aktualisiert) werden.

### <a name="testing--debugging-on-macos"></a>Testen und Debuggen unter macOS

Mobile Xamarin-Anwendungen können über USB für Tests und das Debuggen auf physischen Geräten bereitgestellt werden (Xamarin.Mac-Apps können direkt auf dem Entwicklungscomputer getestet werden; Apple Watch-Apps werden zunächst auf dem gekoppelten iPhone bereitgestellt).

[!include[](~/cross-platform/includes/macos-testing.html)]


<a name="windows" />

## <a name="windows-requirements"></a>Windows-Anforderungen

Die Verwendung eines Windows-Computers für die Xamarin-Entwicklung erfordert folgende Software-/SDK-Versionen.
Überprüfen Sie Ihre Betriebssystemversion und bestätigen Sie, dass Sie keine *Express*-Version von Visual Studio haben. Falls doch, sollten Sie darüber nachdenken, auf die *Community*-Edition zu aktualisieren.
Die Installer von Visual Studio 2015 und 2017 enthalten eine Option zum automatischen Installieren von Xamarin.

[!include[](~/cross-platform/includes/windows-requirements.html)]


> [!NOTE]
>
>* Xamarin für Visual Studio unterstützt jede Version von Visual Studio 2015 oder 2017 (Community, Professional und Enterprise).
>
>* Die Entwicklung von Xamarin.Forms-Apps für die universelle Windows-Plattform (UWP) erfordert Windows 10 mit Visual Studio 2015 oder 2017.


### <a name="testing--debugging-on-windows"></a>Testen und Debuggen unter Windows

Mobile Xamarin-Anwendungen können über USB für Tests und das Debuggen auf physischen Geräten bereitgestellt werden (iOS-Geräte müssen mit dem Mac-Computer verbunden sein, nicht mit dem Computer, der Visual Studio ausführt).

[!include[](~/cross-platform/includes/windows-testing.html)]


> [!NOTE]
>
>* [Windows Phone 8.1 emulator download (Windows Phone 8.1 Emulatordownload)](https://www.microsoft.com/en-us/download/details.aspx?id=43719).
>* Der Windows Phone 10 Emulator ist im SDK der universellen Windows-Plattform von Visual Studio 2015 enthalten.

<a name="install" />

## <a name="installation-instructions"></a>Installationsanweisungen

Das neueste Xamarin-Release für macOS kann unter [xamarin.com/download](http://xamarin.com/download) heruntergeladen werden. Folgen Sie für Windows den Installationsanweisungen von [Visual Studio 2017](https://docs.microsoft.com/en-us/visualstudio/install/install-visual-studio).

Eine vollständige Liste unserer aktuellen Produktversionen ist auf der [Seite für die aktuellen Releases](http://developer.xamarin.com/releases/current/) verfügbar. Dort sind auch die einzelnen Produktversionen (und Links zu den Anmerkungen zur Version) für unsere Beta- und Alphakanäle erklärt.

Spezifische Anleitungen für die [Installation](~/cross-platform/get-started/installation/index.md) auf den einzelnen Plattformen finden Sie hier:

- [Xamarin.iOS](~/ios/get-started/installation/index.md)
- [Xamarin.Android](~/android/get-started/installation/index.md)
- [Xamarin.Mac](~/mac/get-started/installation.md)

Dort finden Sie auch zusätzliche Informationen über [Xamarin.Forms-Anforderungen und unterstützte Plattformen](~/xamarin-forms/get-started/installation.md).


## <a name="related-links"></a>Verwandte Links

- [Xamarin herunterladen](https://xamarin.com/download/)
- [Aktuelle Releases](https://developer.xamarin.com/releases/current/)
