---
title: Daten und Cloud Services in xamarin. IOS-apps
description: In diesem Dokument finden Sie Links zu Anleitungen, in denen beschrieben wird, wie Sie in einer xamarin. IOS-App mit lokalen Daten, icloud und cloudkit arbeiten.
ms.prod: xamarin
ms.assetid: 945719F7-7CE6-4207-BF0F-23195125FC84
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/13/2017
ms.openlocfilehash: 4b3bf1fcc6be7713c87b16ef9ec083189793b324
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73008058"
---
# <a name="data-and-cloud-services-in-xamarinios-apps"></a>Daten und Cloud Services in xamarin. IOS-apps

## <a name="data-accessiosdata-clouddataindexmd"></a>[Datenzugriff](~/ios/data-cloud/data/index.md)

Bei den meisten Anwendungen ist es erforderlich, dass Daten lokal auf dem Gerät gespeichert werden. Wenn die Menge der Daten nicht trivial klein ist, erfordert dies in der Regel eine Datenbank und eine Datenschicht in der Anwendung, um den Zugriff auf die Datenbank zu verwalten. IOS verfügt über das SQLite-Datenbankmodul, das integriert ist, und der Zugriff auf die Daten wird durch die xamarin-Plattform vereinfacht, die das SQLite-Datenanbieter enthält.

## <a name="icloudiosdata-cloudintroduction-to-icloudmd"></a>[iCloud](~/ios/data-cloud/introduction-to-icloud.md)

Mit der icloud-Speicher-API in ios 5 können Anwendungen Benutzer Dokumente und anwendungsspezifische Daten an einem zentralen Speicherort speichern und von allen Geräten des Benutzers aus auf diese Elemente zugreifen.

## <a name="cloudkitiosdata-cloudintro-to-cloudkitmd"></a>[CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

Das cloudkit-Framework optimiert die Entwicklung von Anwendungen, die auf icloud zugreifen. Dies umfasst das Abrufen von Anwendungsdaten und assetrechten sowie die Möglichkeit, Anwendungsinformationen sicher zu speichern. Dieses Kit bietet Benutzern eine Ebene der Anonymität, indem Sie den Zugriff auf Anwendungen mit ihren icloud-IDs ermöglicht, ohne persönliche Informationen zu teilen.
