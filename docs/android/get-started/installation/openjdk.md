---
title: Microsoft OpenJDK-Verteilung (Vorschauversion)
description: Eine Schritt-für-Schritt-Anleitung für die Konfiguration der Microsoft OpenJDK-Verteilung.
ms.prod: xamarin
ms.assetid: B5F8503D-F4D1-44CB-8B29-187D1E20C979
ms.technology: xamarin-android
author: vyedin
ms.author: vyedin
ms.date: 07/22/2018
ms.openlocfilehash: 6c1346918ca6881e551f6c5b89ab16ad13d3f804
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242513"
---
# <a name="microsofts-openjdk-distribution-preview"></a>Microsoft OpenJDK-Verteilung (Vorschauversion)

_Dieses Handbuch beschreibt die Schritte zum Wechsel auf die Vorschauversion der Microsoft OpenJDK-Verteilung._

![Feature der Vorschauversion](~/media/shared/preview.png)

## <a name="overview"></a>Übersicht

Beginnend mit Visual Studio 15.9 und Visual Studio für Mac 7.7 wird Visual Studio-Tools für Xamarin von Oracle's JDK zu einer kompakten OpenJDK -Version wechseln, die ausschließlich für die Android-Entwicklung gedacht ist:

![Neuer Workflow mit einer Webvorschau von OpenJDK in VS 15.8 Preview 5](openjdk-images/openjdk.png)

Die Vorteile dieses Wechsels sind:

- Es steht Ihnen immer eine OpenJDK-Version zur Verfügung, die für die Android-Entwicklung verwendet werden kann.

- Das Herunterladen von JDK 9 oder 10 hat keine Auswirkungen auf die Entwicklung.

- Erheblich geringere Downloadgröße und weniger Speicherbedarf.

- Keine weiteren Probleme mit Servern und Installationsprogrammen von Drittanbietern.

Wenn Sie früher auf die verbesserte Version umsteigen möchten, stehen Ihnen Builds der Microsoft OpenJDK-Verteilung zum Testen unter Windows und Mac zur Verfügung. Der Einrichtungsprozess wird im Folgenden beschrieben, und Sie können jederzeit wieder zum Oracle JDK zurückkehren.

## <a name="download"></a>Herunterladen

Laden Sie zunächst den richtigen Build für Ihr System herunter:

- **Mac** &ndash; https://dl.xamarin.com/OpenJDK/mac/microsoft-dist-openjdk-1.8.0.9.zip
- **Windows x86** &ndash; https://dl.xamarin.com/OpenJDK/win32/microsoft-dist-openjdk-1.8.0.9.zip
- **Windows x64** &ndash; https://dl.xamarin.com/OpenJDK/win64/microsoft-dist-openjdk-1.8.0.9.zip

## <a name="configure"></a>Konfigurieren

Entzippen Sie ihn im richtigen Speicherort:

- **Mac** &ndash; **$HOME/Library/Developer/Xamarin/jdk/microsoft_dist_openjdk_1.8.0.9**
- **Windows** &ndash; **C:\\Programme\\Android\\jdk\\microsoft_dist_openjdk_1.8.0.9**

> [!IMPORTANT]
> Dieses Beispiel verwendet Build 1.8.0.9; die Version, die Sie herunterladen, kann jedoch neuer sein.

Ordnen Sie die IDE dem neuen JDK zu:

- **Mac** &ndash; Klicken Sie auf **Tools > SDK Manager > Speicherorte**, und ändern Sie den Speicherort von **Java SDK (JDK)** in den vollständigen Pfad der OpenJDK-Installation. Im folgenden Beispiel ist dieser Pfad folgendermaßen festgelegt: **$HOME/Library/Developer/Xamarin/jdk/microsoft_dist_openjdk_1.8.0.9**.

![Festlegen des JDK-Pfads für die Microsoft OpenJDK-Verteilung auf dem Mac](openjdk-images/vsm.png)

- **Windows** &ndash; Klicken Sie auf **Werkzeuge > Optionen > Xamarin > Android-Einstellungen**, und ändern Sie den **Java Development Kit-Speicherort** in den vollständigen Pfad der OpenJDK-Installation. Im folgenden Beispiel ist dieser Pfad folgendermaßen festgelegt **C:\\Programmdateien\\Android\\Jdk\\microsoft_dist_openjdk_1.8.0.9**:

![Festlegen des JDK-Pfads für die Microsoft OpenJDK-Verteilung für Windows](openjdk-images/vs.png)

## <a name="revert"></a>Zurücksetzen

Um zum Oracle JDK zurückzukehren, ändern Sie den Speicherort des Java SDK in den zuvor verwendeten Oracle JDK-Pfad und bauen Sie die Lösung neu auf. Auf Mac können Sie den Oracle JDK-Pfad wiederherstellen, indem Sie auf **Auf Standardwerte zurücksetzen** klicken.

Wenn Sie Probleme mit der Microsoft OpenJDK-Verteilung haben, melden Sie diese bitte über das Feedback-Tool in Ihrer IDE, damit die Probleme schnell nachverfolgt und behoben werden können.

## <a name="known-issues--planned-fix-dates"></a>Bekannte Probleme & Daten für geplante Fixes

Die `JAVA_HOME`-Umgebungsvariable wird möglicherweise nicht ordnungsgemäß zum SDK und zum Geräte-Manager exportiert. Zum Umgehen des Problems können Sie diese Umgebungsvariable auf den OpenJDK-Speicherort auf Ihrem Computer festlegen. Dieses Problem wird in den Preview-Versionen 15.9 behoben.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben Sie erfahren, wie Sie Ihre IDE so konfigurieren, dass sie die Vorschauversion der Microsoft OpenJDK-Verteilung verwendet, die im späteren Verlauf von 2018 veröffentlicht werden soll.
