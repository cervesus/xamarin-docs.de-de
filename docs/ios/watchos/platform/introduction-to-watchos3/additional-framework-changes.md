---
title: Weitere WatchOS 3 Frameworks Änderungen
description: Dieses Dokument beschreibt die verschiedenen frameworkänderungen mit WatchOS 3 und wie Sie in Xamarin verwendet werden. Kerndaten, Core während der Übertragung, Foundation, HealthKit, HomeKit, PassKit und UIKit werden erläutert.
ms.prod: xamarin
ms.assetid: FE93796E-F699-4B14-B37D-D39F9D48E81E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: e3eb4e3454aeab08d1333c5dbc3d4808fa4d676c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61224512"
---
# <a name="additional-watchos-3-frameworks-changes"></a>Weitere WatchOS 3 Frameworks Änderungen

_Dieser Artikel enthält zusätzliche, kleinere Änderungen oder Verbesserungen an vorhandenen Frameworks für WatchOS 3._

Zusätzlich zu den wichtigsten Änderungen an IOS-hat Apple Änderungen und Verbesserungen für mehrere vorhandene Frameworks, die in WatchOS 3.


## <a name="core-data"></a>Core-Daten

Die folgenden Verbesserungen wurden für die Kerndaten-Framework für watchos 3 vorgenommen werden:

- Stamm [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) -Objekte unterstützt gleichzeitige Fehlerbehandlung und ohne Serialisierung abrufen.
- Die [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) Klasse verwaltet einen Pool mit SQLite-Datenspeicher.
- Die [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) Objekte mit SQLite-Datenspeicher in die WAL-Journal-Modus-Unterstützung der neuen Abfrage Generation Funktion, in denen verwaltete Objekt Kontexten (MOC) für bestimmte Datenbankversionen zum Abrufen der zukünftigen angeheftet werden können und Fehlgeschlagene Transaktionen an.
- Mithilfe der allgemeinen `NSPersistenceContainer` zu verweisen die `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) und anderen Konfigurationsressourcen Core Data.
- Mehrere neue Hilfsmethoden hinzugefügt wurden `NSManagedObject` abrufen und Erstellen von Unterklassen zu vereinfachen.

Weitere Informationen finden Sie unter Apple [Core Daten Frameworkverweis](https://developer.apple.com/reference/coredata).


## <a name="core-motion"></a>Core-Bewegung

Die folgenden Verbesserungen wurden für das Motion-Core-Framework für watchos 3 vorgenommen werden:

- Das neue Gerät Motion-Ereignis werden der Beschleunigungsmesser und das Gyroskop verwendet, um während der Übertragung und die Ausrichtung Updates bereitzustellen. Die app kann für dieses Update (mit Raten von bis zu 100 Hz) registrieren.
- Das neue Schrittzähler Ereignis ermöglicht einen schnellen und in Echtzeit Benachrichtigungen, wenn der Benutzer hält und wieder gestartet. Verwenden der [CMPedometer](https://developer.apple.com/reference/coremotion/cmpedometer) für Vorder- oder Hintergrund Schrittzähler Ereignisse registrieren.


## <a name="foundation"></a>Foundation

Die folgenden Verbesserungen wurden der Foundation-Frameworks für Watch OS-3 vorgenommen werden:

- Verwenden Sie die neue [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) Klasse um Datum und Uhrzeit-Intervall Berechnungen wie Dauer, für den Vergleich von Abständen, und für die Interval-Schnittpunkte zu testen.
- Mehrere neue Eigenschaften hinzugefügt wurden die [NSLocal](https://developer.apple.com/reference/foundation/nslocale) Klasse, um lokale Informationen und die verfügbaren Anzeigeformate abzurufen.
- Verwenden Sie die neue [NSMeasurement](https://developer.apple.com/reference/foundation/nsmeasurement) -Klasse zum Konvertieren zwischen verschiedenen Einheiten von Measures (Maßeinheit), oder führen Berechnungen auf den Werten in verschiedenen UOMs.
- Verwenden Sie die neue [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) Klasse, um lokalisierte Messungen für die Anzeige für den Endbenutzer zu formatieren.
- Verwenden Sie die neue [NSUnit](https://developer.apple.com/reference/foundation/nsunit) und [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) Klassen zum Darstellen von bestimmten UOMs.


## <a name="healthkit"></a>HealthKit

Die folgenden Verbesserungen wurden HealthKit Framework für die Überwachung von OS-3 vorgenommen werden:

- Verwenden Sie die neue [HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) Klasse an die `ActivityType` und `LocationType` von einer Herausforderung.
- Die neue [HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) und `WheelchairUse` -Methode der der [HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore) Klasse für die Arbeit mit Rollstuhl hinzugefügt wurden integritätsdaten beziehen.
- Neue Metadatenschlüssel für das Wetter-Typen hinzugefügt wurden (z. B. `HKWeatherConditionClear` und `HKWeatherConditionCloudy`) und Trainings-Typen (z. B. `HKWorkoutActivityTypeFlexibility` und `HKWorkoutActivityTypeWheelchairRunPace`) wurden hinzugefügt.


## <a name="homekit"></a>HomeKit

Die folgenden Verbesserungen wurden für das HomeKit-Framework für Watch OS-3 vorgenommen werden:

- Hinzugefügt, die Möglichkeit zum Anzeigen von und Interaktion mit HomeKit verbunden-IP-Kameras.
- Mehrere neue Dienste und Eigenschaften hinzugefügt.
- Mehr Kontext und Konfiguration von primären und linkdiensten Zubehör von hinzugefügt.


## <a name="passkit"></a>PassKit

Die folgenden Verbesserungen wurden auf dem PassKit-Framework für Watch OS-3 vorgenommen werden:

- Erweitert das Framework zur Unterstützung von Zahlungen für sicherer, in-app auf der Apple Watch von sowohl physischen waren und Diensten.
- Die folgenden Klassen sind jetzt verfügbar: [PKPayment](https://developer.apple.com/reference/passkit/pkpayment), [PKPaymentMethod](https://developer.apple.com/reference/passkit/pkpaymentmethod), [PKPaymentRequest](https://developer.apple.com/reference/passkit/pkpaymentrequest) und [PKPaymentToken](https://developer.apple.com/reference/passkit/pkpaymenttoken)


## <a name="uikit"></a>UIKit

Die folgenden Verbesserungen wurden die UIKit-Framework für überwachen OS-3 vorgenommen werden:

- Zur Unterstützung von Typ "Dynamic" in Bezeichnungen Textfelder und Text-Felder verwenden Sie die neue `PreferredFontForTextStyle` Methode der `UIFont` Klasse.
- Die `ColorWithDisplayP3` Methode wurde hinzugefügt, um Breite Farbskala zu unterstützen.


## <a name="related-links"></a>Verwandte Links

- [Erste Schritte (Beispiel)](https://developer.xamarin.com/samples/monotouch/WatchKit/)
- [Neuerungen in WatchOS 3](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
