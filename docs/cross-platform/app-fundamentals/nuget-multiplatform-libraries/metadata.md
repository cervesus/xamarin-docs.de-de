---
title: NuGet-Metadaten bearbeiten
description: "Verwenden Sie die Projektoptionen zum Bearbeiten von NuGet-Metadaten für Multiplattform-Bibliotheken"
ms.topic: article
ms.prod: xamarin
ms.assetid: 147BA370-67A7-4E6C-BF17-AA7C536C0A48
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 8afce6021c2816f354e26ccecd7d0c40ceb2a9bd
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="editing-nuget-metadata"></a>NuGet-Metadaten bearbeiten

_Verwenden Sie die Projektoptionen zum Bearbeiten von NuGet-Metadaten für Multiplattform-Bibliotheken_

Bibliothek Projekttypen (z. B. PCL .NET Standard oder die neuen NuGet-Projekttyp) haben eine **NuGet-Paket** im Abschnitt der **Projektoptionen** Fenster.

Die **Metadaten** Abschnitt konfiguriert die Werte, die verwendet wird, der [ **.nuspec** NuGet-Paket-Manifestdatei](https://docs.microsoft.com/en-us/nuget/create-packages/creating-a-package#the-role-and-structure-of-the-nuspec-file).

## <a name="required-information"></a>Erforderliche Informationen

Die **allgemeine** Registerkarte enthält vier Felder, die eingegeben werden müssen, um ein NuGet-Paket zu generieren:

[ ![](metadata-images/metadata-general-sml.png "Die erforderlichen Metadaten-Fenster von NuGet-Paket")](metadata-images/metadata-general.png)

- **ID** – die Paket-ID, die in Nuget.org (oder ablegen, wo das Paket verteilt werden soll) eindeutig sein sollte. Führen Sie [Anleitung](https://docs.microsoft.com/en-us/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) und nur gültige Zeichen in einer URL (ohne Leerzeichen ein, und vermeiden Sie die meisten Sonderzeichen).
- **Version** – wählen Sie eine Versionsnummer, die konsistent mit [NuGet Versionsregeln](https://docs.microsoft.com/en-us/nuget/create-packages/dependency-versions).
- **Autoren** – durch Trennzeichen getrennte Liste von Namen.
- **Beschreibung** – Übersicht über die Funktionen der Paketkonfiguration der angezeigt wird, wenn der Benutzer das Paket auswählen.

> [!NOTE]
> Denken Sie daran, um die Versionsnummer erhöht, wenn neue Versionen für die Verteilung an NuGet oder andere Benutzer zu erstellen.

Weitere Informationen finden Sie unter der [Elementreferenzen erforderlich](https://docs.microsoft.com/en-us/nuget/schema/nuspec#required-metadata-elements) für Weitere Informationen und wie diese Anweisungen auf detaillierte [eine eindeutige Paket-ID auswählen und zum Festlegen der Versionsnummer](https://docs.microsoft.com/en-us/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) und [ Ein Paket Einstellungstyp](https://docs.microsoft.com/en-us/nuget/create-packages/creating-a-package#setting-a-package-type).

> [!IMPORTANT]
> Alle Felder auf dieser Registerkarte müssen eingegeben werden. Andernfalls wird eine Fehlermeldung angezeigt: _"das Projekt verfügt nicht über NuGet Metadaten also ein NuGet-Paket wird nicht erstellt werden. Metadaten von NuGet-Paketen kann im Abschnitt "Metadaten" im Projekt-Optionen angegeben werden"_

## <a name="optional-metadata"></a>Optionale Metadaten

Die **Details** Registerkarte enthält die optionalen Felder aus, in der Manifestdatei des NuGet-Paket eingeschlossen werden sollen.

[ ![](metadata-images/metadata-detail-sml.png "Optionale Metadaten-Fenster von NuGet-Paket")](metadata-images/metadata-detail.png)

Finden Sie in der [Referenz zu optionalen Elemente](https://docs.microsoft.com/en-us/nuget/schema/nuspec#optional-metadata-elements) für Weitere Informationen zu den erforderlichen und optionalen Feldern.

> [!NOTE]
> Wenn die Verteilung des NuGet-Pakets auf [NuGet.org](https://www.nuget.org) es wird empfohlen, so viele Informationen wie möglich bereitzustellen.


## <a name="related-links"></a>Verwandte Links

- [NUSPEC-Verweis](https://docs.microsoft.com/en-us/nuget/schema/nuspec#general-form-and-schema)
