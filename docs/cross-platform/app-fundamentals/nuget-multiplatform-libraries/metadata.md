---
title: Bearbeiten von nuget-Metadaten
description: In diesem Dokument wird beschrieben, wie die Projektoptionen verwendet werden, um nuget-Metadaten für Multiplattform-Bibliotheken zu bearbeiten. Dabei werden sowohl erforderliche als auch optionale Metadaten erläutert.
ms.prod: xamarin
ms.assetid: 147BA370-67A7-4E6C-BF17-AA7C536C0A48
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 5965b3f78bfc086b1cc6030a02f1cabafaae29d3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016740"
---
# <a name="editing-nuget-metadata"></a>Bearbeiten von nuget-Metadaten

_Verwenden Sie die Projektoptionen, um nuget-Metadaten für Multiplattform-Bibliotheken zu bearbeiten._

Bibliotheksprojekt Typen (z. b. PCL oder .NET Standard oder der neue nuget-Projekttyp) enthalten im Fenster " **Projektoptionen** " den Abschnitt " **nuget-Paket** ".

Der **Metadatenabschnitt** konfiguriert die Werte, die in der [ **nuspec** -Datei für das nuget-Paket Manifest](https://docs.microsoft.com/nuget/create-packages/creating-a-package#the-role-and-structure-of-the-nuspec-file)verwendet werden.

## <a name="required-information"></a>Erforderliche Informationen

Die Registerkarte **Allgemein** enthält vier Felder, die eingegeben werden müssen, um ein nuget-Paket zu generieren:

[![](metadata-images/metadata-general-sml.png "NuGet package required metadata window")](metadata-images/metadata-general.png#lightbox)

- **ID** – der Paket Bezeichner, der innerhalb von Nuget.org eindeutig sein sollte (oder wo das Paket verteilt wird). Befolgen Sie diese [Anleitung](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) , und verwenden Sie nur Zeichen, die in einer URL gültig sind (ohne Leerzeichen, und vermeiden Sie die meisten Sonderzeichen).
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

[![](metadata-images/metadata-detail-sml.png "NuGet package optional metadata window")](metadata-images/metadata-detail.png#lightbox)

Weitere Informationen zu den erforderlichen und optionalen Feldern finden Sie in der [optionalen Element Referenz](https://docs.microsoft.com/nuget/schema/nuspec#optional-metadata-elements) .

> [!NOTE]
> Wenn das nuget-Paket auf [NuGet.org](https://www.nuget.org) verteilt wird, wird empfohlen, so viele Informationen wie möglich bereitzustellen.

## <a name="related-links"></a>Verwandte Links

- [NUSPEC-Verweis](https://docs.microsoft.com/nuget/schema/nuspec#general-form-and-schema)
