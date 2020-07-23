---
title: Bearbeiten von nuget-Metadaten
description: In diesem Dokument wird beschrieben, wie die Projektoptionen verwendet werden, um nuget-Metadaten für Multiplattform-Bibliotheken zu bearbeiten. Dabei werden sowohl erforderliche als auch optionale Metadaten erläutert.
ms.prod: xamarin
ms.assetid: 147BA370-67A7-4E6C-BF17-AA7C536C0A48
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 858d2a2399e1d294767b8afad36502b809955224
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86933274"
---
# <a name="editing-nuget-metadata"></a>Bearbeiten von nuget-Metadaten

_Verwenden Sie die Projektoptionen, um nuget-Metadaten für Multiplattform-Bibliotheken zu bearbeiten._

Bibliotheksprojekt Typen (z. b. PCL oder .NET Standard oder der neue nuget-Projekttyp) enthalten im Fenster " **Projektoptionen** " den Abschnitt " **nuget-Paket** ".

Der **Metadatenabschnitt** konfiguriert die Werte, die in der [ **nuspec** -Datei für das nuget-Paket Manifest](https://docs.microsoft.com/nuget/create-packages/creating-a-package#the-role-and-structure-of-the-nuspec-file)verwendet werden.

## <a name="required-information"></a>Erforderliche Informationen

Die Registerkarte **Allgemein** enthält vier Felder, die eingegeben werden müssen, um ein nuget-Paket zu generieren:

[![Erforderliches Metadatenfenster für nuget-Paket](metadata-images/metadata-general-sml.png)](metadata-images/metadata-general.png#lightbox)

- **ID** – der Paket Bezeichner, der innerhalb von NuGet.org eindeutig sein sollte (oder wo das Paket verteilt wird). Befolgen Sie diese [Anleitung](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) , und verwenden Sie nur Zeichen, die in einer URL gültig sind (ohne Leerzeichen, und vermeiden Sie die meisten Sonderzeichen).
- **Version** – wählen Sie eine Versionsnummer aus, [die den nuget-Versions Regeln](https://docs.microsoft.com/nuget/create-packages/dependency-versions)entspricht.
- **Autoren** – durch Trennzeichen getrennte Liste von Namen.
- **Beschreibung** – Übersicht über die Features des Pakets, das angezeigt wird, wenn Benutzer das Paket auswählen.

> [!NOTE]
> Beachten Sie, dass die Versionsnummer erhöht werden muss, wenn neue Versionen für die Verteilung an nuget oder andere Benutzer aufgebaut werden.

Weitere Informationen finden Sie in der [Referenz zu den erforderlichen Elementen](https://docs.microsoft.com/nuget/schema/nuspec#required-metadata-elements) sowie in den folgenden detaillierten Anweisungen zum [Auswählen eines eindeutigen Paket Bezeichners und Festlegen der Versionsnummer](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) und [Festlegen eines Pakettyps](https://docs.microsoft.com/nuget/create-packages/creating-a-package#setting-a-package-type).

> [!IMPORTANT]
> Alle Felder auf dieser Registerkarte müssen eingegeben werden. Andernfalls wird eine Fehlermeldung angezeigt: _"das Projekt verfügt nicht über nuget-Metadaten, sodass kein nuget-Paket erstellt wird. Nuget-Paket Metadaten können im Metadatenabschnitt in den Projektoptionen angegeben werden_ .

## <a name="optional-metadata"></a>Optionale Metadaten

Die Registerkarte **Details** enthält optionale Felder, die in der nuget-Paket Manifest-Datei enthalten sein sollen.

[![Optionales Metadatenfenster des nuget-Pakets](metadata-images/metadata-detail-sml.png)](metadata-images/metadata-detail.png#lightbox)

Weitere Informationen zu den erforderlichen und optionalen Feldern finden Sie in der [optionalen Element Referenz](https://docs.microsoft.com/nuget/schema/nuspec#optional-metadata-elements) .

> [!NOTE]
> Wenn das nuget-Paket auf [NuGet.org](https://www.nuget.org) verteilt wird, wird empfohlen, so viele Informationen wie möglich bereitzustellen.

## <a name="related-links"></a>Ähnliche Themen

- [. nuspec-Referenz](https://docs.microsoft.com/nuget/schema/nuspec#general-form-and-schema)
