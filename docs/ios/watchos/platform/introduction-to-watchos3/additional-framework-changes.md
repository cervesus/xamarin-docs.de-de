---
title: Zusätzliche WatchOS 3 Frameworks Änderungen
description: Dieser Artikel umfasst zusätzliche, kleinere Änderungen oder Erweiterungen von vorhandenen Frameworks für WatchOS 3.
ms.prod: xamarin
ms.assetid: FE93796E-F699-4B14-B37D-D39F9D48E81E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 572aaff9d010ec1ec1f48db2878859e445e2fb20
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="additional-watchos-3-frameworks-changes"></a>Zusätzliche WatchOS 3 Frameworks Änderungen

_Dieser Artikel umfasst zusätzliche, kleinere Änderungen oder Erweiterungen von vorhandenen Frameworks für WatchOS 3._

Zusätzlich zu den wichtigen Änderungen für iOS machte Apple Änderungen und Verbesserungen für mehrere vorhandene Frameworks in WatchOS 3.


## <a name="core-data"></a>Core-Daten

Im folgenden aufgeführten Erweiterungen werden Framework Core-Daten für Überwachung OS-3 vorgenommen:

- Stamm [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) Objekte unterstützt gleichzeitige zugesteht und ohne Serialisierung abrufen.
- Die [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) Klasse verwaltet einen Pool von SQLite-Datenspeichern.
- Die [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) Objekte mit SQLite-Datenspeicher in die WAL Journal Modus Unterstützung der neuen Abfrage Generation Funktion, auf dem verwalteten Objekt Kontexten (MOC) auf bestimmte Datenbankversionen zum Abrufen der zukünftigen angeheftet werden können und Fehlgeschlagene Transaktionen.
- Mithilfe der allgemeinen `NSPersistenceContainer` zu verweisen die `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) und andere Ressourcen Core Daten.
- Mehrere neue Hilfsmethoden wurden hinzugefügt, um `NSManagedObject` einfacher zu Abrufvorgängen auszuführen und Unterklassen zu erstellen.

Weitere Informationen finden Sie in der Apple- [Core Daten Frameworkverweis](https://developer.apple.com/reference/coredata).


## <a name="core-motion"></a>Core-Bewegung

Im folgenden aufgeführten Erweiterungen werden Framework Core Bewegung für Überwachung OS-3 vorgenommen:

- Das neue Gerätebewegung Ereignis verwendet den Beschleunigungsmesser und die Gyroskop, um während des Verschiebens und Ausrichtung Updates bereitzustellen. die app-app kann für diesen aktualisiert (mit Raten von bis zu 100 Hz) registrieren.
- Das neue Schrittzähler Ereignis bietet schnelle Echtzeit Benachrichtigungen, wenn der Benutzer hält und wieder ausgeführt. Verwenden der [CMPedometer](https://developer.apple.com/reference/coremotion/cmpedometer) für Vorder- oder Hintergrund Schrittzähler Ereignisse registrieren.


## <a name="foundation"></a>Foundation

Die folgenden Verbesserungen werden der Foundation-Frameworks für Überwachung OS-3 vorgenommen:

- Verwenden Sie die neue [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) Klasse, um das Datum und Uhrzeit Intervall-Berechnungen, z. B. Dauer, für den Vergleich von Intervallen und Tests für Intervall Schnittmengen vornehmen.
- Mehrere neue Eigenschaften hinzugefügt wurden die [NSLocal](https://developer.apple.com/reference/foundation/nslocale) Klasse, um lokale Daten und die verfügbaren Anzeigeformate abzurufen.
- Verwenden Sie die neue [NSMeasuerment](https://developer.apple.com/reference/foundation/nsmeasurement) -Klasse zum Konvertieren zwischen verschiedenen Einheiten des Measure (Maßeinheit) oder Berechnungen auf den Werten in anderen UOMs.
- Verwenden Sie die neue [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) Klasse, um lokalisierte Messungen für die Anzeige für den Endbenutzer zu formatieren.
- Verwenden Sie die neue [NSUnit](https://developer.apple.com/reference/foundation/nsunit) und [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) Klassen zum Darstellen von bestimmten UOMs.


## <a name="healthkit"></a>HealthKit

Im folgenden aufgeführten Erweiterungen werden HealthKit Framework für die Überwachung OS-3 vorgenommen:

- Verwenden Sie die neue [HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) Klasse an die `ActivityType` und `LocationType` von einer Herausforderung.
- Die neue [HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) und die `WheelchairUse` Methode der [HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore) Klasse wurden für die Arbeit mit Rollstuhl hinzugefügt Zustandsdaten beziehen.
- Neue Metadaten-Schlüssel für Weather Typen wurden hinzugefügt (wie z. B. `HKWeatherConditionClear` und `HKWeatherConditionCloudy`) und Trainings-Typen (z. B. `HKWorkoutActivityTypeFlexibility` und `HKWorkoutActivityTypeWheelchairRunPace`) hinzugefügt wurden.


## <a name="homekit"></a>HomeKit

Im folgenden aufgeführten Erweiterungen werden HomeKit Framework für die Überwachung OS-3 vorgenommen:

- Die Möglichkeit zum Anzeigen und interagieren mit HomeKit verbunden IP hinzugefügt Kameras.
- Mehrere neue Dienste und Merkmale hinzugefügt.
- Mehr Kontext und Konfiguration von Zubehör primären Dienste und Verknüpfen von Services hinzugefügt.


## <a name="passkit"></a>PassKit

Im folgenden aufgeführten Erweiterungen werden PassKit Framework für die Überwachung OS-3 vorgenommen:

- Erweitert das Framework zur Unterstützung von sicheren, in der app Zahlungen auf der Apple Watch physischen waren und Dienste.
- Die folgenden Klassen sind jetzt verfügbar: [PKPayment](https://developer.apple.com/reference/passkit/pkpayment), [PKPaymentMethod](https://developer.apple.com/reference/passkit/pkpaymentmethod), [PKPaymentRequest](https://developer.apple.com/reference/passkit/pkpaymentrequest) und [PKPaymentToken](https://developer.apple.com/reference/passkit/pkpaymenttoken)


## <a name="uikit"></a>UIKit

Im folgenden aufgeführten Erweiterungen werden UIKit Framework für die Überwachung OS-3 vorgenommen:

- Zur Unterstützung von dynamischen Typ in Bezeichnungen Textfelder und Textfelder verwenden Sie die neue `PreferredFontForTextStyle` Methode der `UIFont` Klasse.
- Die `ColorWithDisplayP3` -Methode zur Unterstützung der Breite Farbskala hinzugefügt wurde.


## <a name="related-links"></a>Verwandte Links

- [Erste Schritte (Beispiel)](https://developer.xamarin.com/samples/monotouch/WatchKit/)
- [Was ist neu in WatchOS 3](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
