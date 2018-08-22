---
title: Einschränkungen von Xamarin Live Player
description: Dieses Dokument beschreibt die Einschränkungen von Xamarin Live Player. Es wird erläutert, Anforderungen an das Gerät, enthält es funktioniert mit Projekttypen und andere verschiedenen Themen besprochen.
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
author: topgenorth
ms.author: toopge
ms.date: 08/08/2018
ms.openlocfilehash: 99ed8d06331ac7e423791309da79d72d5a10d70f
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2018
ms.locfileid: "40251239"
---
# <a name="limitations-of-xamarin-live-player"></a>Einschränkungen von Xamarin Live Player

![Feature (Vorschau)](~/media/shared/preview.png)

## <a name="device-requirements"></a>Anforderungen für Geräte

Die Xamarin Live Player-app unterstützt folgende Android-Geräte:

- Android 4.2 oder höher.
- ARM-v7a, ARM-v8a "," ARM64-v8a, x 86- oder x86_64-Prozessor.

## <a name="limitations"></a>Einschränkungen

Es gibt einige Einschränkungen für die Dinge, die Xamarin Live Player ausgeführt werden kann, einschließlich der folgenden Elemente:

### <a name="ios"></a>iOS

Live Player ist nicht verfügbar für iOS.

### <a name="xamarinforms"></a>Xamarin.Forms

- Benutzerdefinierte Renderer werden nicht unterstützt.
- Effekte werden nicht unterstützt.
- Benutzerdefinierte Steuerelemente mit benutzerdefinierten bindbare Eigenschaften werden nicht unterstützt.
- Eingebettete Ressourcen werden nicht unterstützt (d. h. Einbetten von Bildern oder anderen Ressourcen in einer PCL).
- MVVM-Frameworks von Drittanbietern werden (d. h. nicht unterstützt. Prism, Mvvm Cross, Mvvm Light usw.).

### <a name="other-project-types"></a>Andere Projekttypen

- Live Player dient nicht für native Android-Projekte (die Android XML für die Benutzeroberfläche zu verwenden).

### <a name="misc"></a>Sonstiges

- Eingeschränkte Unterstützung für die Reflektion (derzeit einige beliebte NuGet-Pakete, z. B. SQLite und Json.NET betrifft). Andere NuGet-Pakete werden möglicherweise immer noch unterstützt.
- Einige Systemklassen können nicht überschrieben werden (z. B. eine Unterklasse kann nicht implementiert werden).
- Einige Features der Plattform, für die Bereitstellung erforderlich können nicht funktionieren, in die Xamarin Live Player-app (jedoch sie bei allgemeinen Vorgängen, z. B. Photo Gallery-Zugriff konfiguriert wurde).
- Benutzerdefinierte Ziele und Schritte werden ignoriert. Beispielsweise können keine Tools wie Fody, einpassen, AutoFac und AutoMapper integriert werden.
- F#-Projekte werden nicht unterstützt.
- Erweiterte Szenarien mit benutzerdefinierten generische Klassen und Schnittstellen werden möglicherweise nicht unterstützt.

Melden Sie Probleme mit zusätzlichen auf [Bugzilla](https://aka.ms/live-player-report-issue).

## <a name="related-links"></a>Verwandte Links

- [Problembehandlung](~/tools/live-player/troubleshooting.md)
- [Setup (Einrichtung)](~/tools/live-player/install.md)
