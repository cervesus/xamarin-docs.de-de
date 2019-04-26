---
title: Bearbeiten Sie NuGet-Metadaten
description: Dieses Dokument beschreibt, wie Sie die Optionen für das Projekt zu verwenden, um NuGet-Metadaten für die plattformübergreifende Bibliotheken zu bearbeiten. Es wird erläutert, erforderliche und optionale Metadaten.
ms.prod: xamarin
ms.assetid: 147BA370-67A7-4E6C-BF17-AA7C536C0A48
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 3680b02003a844668b0b5c97e5d4c0d296ae3500
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61266869"
---
# <a name="editing-nuget-metadata"></a>Bearbeiten Sie NuGet-Metadaten

_Verwenden Sie Optionen für das Projekt, um NuGet-Metadaten für die plattformübergreifende Bibliotheken zu bearbeiten._

Bibliothek Projekttypen zur Verfügung (wie etwa PCL oder .NET Standard, oder der neue Projekttyp von NuGet) haben eine **NuGet-Paket** im Abschnitt der **Projektoptionen** Fenster.

Die **Metadaten** Abschnitt konfiguriert, die Werte in der [ **NuSpec** Manifestdatei des NuGet-Pakets](https://docs.microsoft.com/nuget/create-packages/creating-a-package#the-role-and-structure-of-the-nuspec-file).

## <a name="required-information"></a>Erforderliche Informationen

Die **allgemeine** Registerkarte enthält vier Felder, die eingegeben werden müssen, um ein NuGet-Paket zu generieren:

[![](metadata-images/metadata-general-sml.png "Die erforderlichen Metadaten-Fenster von NuGet-Paket")](metadata-images/metadata-general.png#lightbox)

- **ID** – die Paket-ID, die in Nuget.org (oder ganz egal, wo das Paket verteilt wird) eindeutig sein sollte. Gehen Sie folgendermaßen vor [Anleitungen](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) und verwenden Sie nur die Zeichen, die in einer URL gültige sind (ohne Leerzeichen ein, und vermeiden Sie die meisten Sonderzeichen).
- **Version** – wählen Sie eine Versionsnummer an, die konsistent mit [NuGet Versionsregeln](https://docs.microsoft.com/nuget/create-packages/dependency-versions).
- **Autoren** : durch Trennzeichen getrennte Liste von Namen.
- **Beschreibung** – Überblick über die Paket Features wird angezeigt, wenn der Benutzer das Paket auswählen.

> [!NOTE]
> Denken Sie daran, die Versionsnummer erhöht, wenn neue Versionen für die Verteilung an NuGet oder andere Benutzer zu erstellen.

Weitere Informationen finden Sie unter den [Elementreferenz erforderlich](https://docs.microsoft.com/nuget/schema/nuspec#required-metadata-elements) für Weitere Informationen und wie diese Anleitungen finden Sie auf [Auswählen eines eindeutigen paketbezeichners und Festlegen der Versionsnummer](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) und [ Festlegen eines Pakettyps](https://docs.microsoft.com/nuget/create-packages/creating-a-package#setting-a-package-type).

> [!IMPORTANT]
> Alle Felder auf dieser Registerkarte müssen eingegeben werden; Andernfalls wird eine Fehlermeldung angezeigt: _"Das Projekt NuGet Metadaten muss nicht so ein NuGet-Paket wird nicht erstellt werden. NuGet-Paketmetadaten kann im Metadatenabschnitt in den Projekteigenschaften angegeben, werden"_

## <a name="optional-metadata"></a>Optionale Metadaten

Die **Details** Registerkarte enthält die optionalen Felder in die Manifestdatei des NuGet-Pakets eingeschlossen werden.

[![](metadata-images/metadata-detail-sml.png "Optionale Metadaten-Fenster von NuGet-Paket")](metadata-images/metadata-detail.png#lightbox)

Finden Sie in der [Referenz zu optionalen Elemente](https://docs.microsoft.com/nuget/schema/nuspec#optional-metadata-elements) für Weitere Informationen zu den erforderlichen und optionalen Feldern.

> [!NOTE]
> Wenn Sie das NuGet-Paket verteilt wird, wird auf [NuGet.org](https://www.nuget.org) es wird empfohlen, so viele Informationen wie möglich bereitstellen.


## <a name="related-links"></a>Verwandte Links

- [NUSPEC-Verweis](https://docs.microsoft.com/nuget/schema/nuspec#general-form-and-schema)
