---
title: Daten und Cloud Services in xamarin. IOS-apps
description: In diesem Dokument finden Sie Links zu Anleitungen, in denen beschrieben wird, wie Sie in einer xamarin. IOS-App mit lokalen Daten, icloud und cloudkit arbeiten.
ms.prod: xamarin
ms.assetid: 945719F7-7CE6-4207-BF0F-23195125FC84
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/13/2017
ms.openlocfilehash: e9f910804f3da9d173206d51c59e1d7ddd9b90a6
ms.sourcegitcommit: bad1ab3f78d7f94d48511666626b54f8ba155689
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2020
ms.locfileid: "75663511"
---
# <a name="data-and-cloud-services-in-xamarinios-apps"></a>Daten und Cloud Services in xamarin. IOS-apps

## <a name="data-accessiosdata-clouddataindexmd"></a>[Datenzugriff](~/ios/data-cloud/data/index.md)

Bei den meisten Anwendungen ist es erforderlich, dass Daten lokal auf dem Gerät gespeichert werden. Wenn die Menge der Daten nicht trivial klein ist, erfordert dies in der Regel eine Datenbank und eine Datenschicht in der Anwendung, um den Zugriff auf die Datenbank zu verwalten. IOS verfügt über das SQLite-Datenbankmodul, das integriert ist, und der Zugriff auf die Daten wird durch die xamarin-Plattform vereinfacht, die das SQLite-Datenanbieter enthält.

## <a name="icloudiosdata-cloudintroduction-to-icloudmd"></a>[iCloud](~/ios/data-cloud/introduction-to-icloud.md)

Die icloud-Speicher-API ermöglicht Anwendungen das Speichern von Benutzer Dokumenten und anwendungsspezifischen Daten an einem zentralen Ort und das Zugreifen auf diese Elemente von allen Geräten des Benutzers.

## <a name="cloudkitiosdata-cloudintro-to-cloudkitmd"></a>[CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

Das cloudkit-Framework optimiert die Entwicklung von Anwendungen, die auf icloud zugreifen. Dies umfasst das Abrufen von Anwendungsdaten und assetrechten sowie die Möglichkeit, Anwendungsinformationen sicher zu speichern. Dieses Kit bietet Benutzern eine Ebene der Anonymität, indem Sie den Zugriff auf Anwendungen mit ihren icloud-IDs ermöglicht, ohne persönliche Informationen zu teilen.
