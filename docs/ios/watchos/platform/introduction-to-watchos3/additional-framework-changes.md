---
title: Weitere Änderungen an watchos 3-Frameworks
description: In diesem Dokument werden verschiedene Framework-Änderungen beschrieben, die mit watchos 3 eingeführt wurden, und es wird erläutert, wie Sie in xamarin damit arbeiten. Wichtige Daten, Core Motion, Foundation, healthkit, homekit, passkit und UIKit werden erörtert.
ms.prod: xamarin
ms.assetid: FE93796E-F699-4B14-B37D-D39F9D48E81E
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: 34f192938ac583e39232312377142015aa6d3811
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70287563"
---
# <a name="additional-watchos-3-frameworks-changes"></a>Weitere Änderungen an watchos 3-Frameworks

_In diesem Artikel werden zusätzliche, kleinere Änderungen oder Verbesserungen an vorhandenen Frameworks für watchos 3 behandelt._

Zusätzlich zu den wichtigsten Änderungen an IOS hat Apple Änderungen und Verbesserungen an mehreren vorhandenen Frameworks in watchos 3 vorgenommen.


## <a name="core-data"></a>Kerndaten

Die folgenden Verbesserungen wurden an dem Core Data Framework für Watch OS 3 vorgenommen:

- Stamm- [nsmanagedobjectcontext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) -Objekte unterstützen das gleichzeitige fehlschlagen und Abrufen ohne Serialisierung.
- Die [nspersistentstorecoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) -Klasse verwaltet einen Pool mit SQLite-Daten speichern.
- Die [nsmanagedobjectcontext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) -Objekte mit SQLite-Daten speichern im Wal-Journal Modus unterstützen die neue Abfrage Generierungs Funktion, bei der verwaltete Objekt Kontexte (Managed Object Context, MUC) an bestimmte Daten Bank Versionen angeheftet werden können, um zukünftige Abruf-und fehlerverarbeitungs Vorgänge durchzusetzen
- Verwenden Sie die allgemeine Ebene `NSPersistenceContainer` , um auf `NSPersistentStoreCoordinator`das, [nsmanagedobjectmodel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) und andere Kern Daten Konfigurations Ressourcen zu verweisen.
- Es wurden mehrere neue Hilfsmethoden hinzugefügt `NSManagedObject` , um die Ausführung von Abrufe und das Erstellen von Unterklassen zu vereinfachen.

Weitere Informationen finden Sie in der [Core Data Framework-Referenz](https://developer.apple.com/reference/coredata)von Apple.


## <a name="core-motion"></a>Kern Bewegung

Die folgenden Verbesserungen wurden an dem Kern Motion-Framework für Watch OS 3 vorgenommen:

- Das neue Gerät Motion-Ereignis verwendet den Beschleunigungsmesser und den Gyroskop, um Bewegungs-und Orientierungs Aktualisierungen bereitzustellen. Die APP kann sich für dieses Update registrieren (mit einer Rate von bis zu 100 Hz).
- Das neue Ereignis "Peer Domain" ermöglicht schnelle Echt Zeit Benachrichtigungen, wenn der Benutzer anhält und die Ausführung fortsetzt. Verwenden Sie [cmpedometer](https://developer.apple.com/reference/coremotion/cmpedometer) , um für Vordergrund-oder hintergrundpeer-Peer-Ereignisse zu registrieren.


## <a name="foundation"></a>Neu

Die folgenden Verbesserungen wurden an Foundation Framework für Watch OS 3 vorgenommen:

- Verwenden Sie die neue [nsdateinterval](https://developer.apple.com/reference/foundation/nsdateinterval) -Klasse, um Datums-und Zeitintervall Berechnungen zu erstellen, z. b. Dauer Zeiten, zum Vergleichen von Intervallen und zum Testen von Intervall interabschnitten.
- Der [nslocal](https://developer.apple.com/reference/foundation/nslocale) -Klasse wurden mehrere neue Eigenschaften hinzugefügt, um lokale Informationen und die verfügbaren Anzeige Formate abzurufen.
- Verwenden Sie die neue [NSMeasurement](https://developer.apple.com/reference/foundation/nsmeasurement)-Klasse, um zwischen verschiedenen Maßeinheiten (Unit of Measure, UOM) zu konvertieren oder Berechnungen für Werte in verschiedenen UOMS auszuführen.
- Verwenden Sie die neue [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter)-Klasse, um lokalisierte Messungen zu formatieren, die dem Endbenutzer angezeigt werden sollen.
- Verwenden Sie die neuen [nsunit](https://developer.apple.com/reference/foundation/nsunit) -und [nsdimension](https://developer.apple.com/reference/foundation/nsdimension) -Klassen, um bestimmte UOMS darzustellen.


## <a name="healthkit"></a>HealthKit

Die folgenden Verbesserungen wurden am healthkit-Framework für Watch OS 3 vorgenommen:

- Verwenden Sie die neue [hkworkoutconfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) `ActivityType` -Klasse, um `LocationType` und für ein Training anzugeben.
- Das neue [hkwheelchairuseobject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) und die `WheelchairUse` -Methode der [hkhealthstore](https://developer.apple.com/reference/healthkit/hkhealthstore) -Klasse wurden zum Arbeiten mit den im Rollstuhl bezogenen Integritäts Daten hinzugefügt.
- Neue Metadatenschlüssel wurden für Wetter Typen ( `HKWeatherConditionClear` z. b. und `HKWeatherConditionCloudy`) hinzugefügt, und es `HKWorkoutActivityTypeFlexibility` wurden `HKWorkoutActivityTypeWheelchairRunPace`Trainings Typen (z. b. und) hinzugefügt.


## <a name="homekit"></a>HomeKit

Die folgenden Verbesserungen wurden am homekit-Framework für Watch OS 3 vorgenommen:

- Die Möglichkeit zum Anzeigen und interagieren mit mit homekit verbundenen IP-Kameras wurde hinzugefügt.
- Es wurden mehrere neue Dienste und Eigenschaften hinzugefügt.
- Der Zubehör der primären Dienste und Verknüpfungs Dienste hat mehr Kontext und Konfiguration hinzugefügt.


## <a name="passkit"></a>PassKit

Die folgenden Verbesserungen wurden an dem passkit-Framework für Watch OS 3 vorgenommen:

- Erweitert das Framework, um sichere, in-App-Zahlungen für die Apple Watch physischer waren und Dienste zu unterstützen.
- Die folgenden Klassen sind jetzt verfügbar: [Pkpayment](https://developer.apple.com/reference/passkit/pkpayment), [pkpaymentmethod](https://developer.apple.com/reference/passkit/pkpaymentmethod), [pkpaymentrequest](https://developer.apple.com/reference/passkit/pkpaymentrequest) und [pkpaymenttoken](https://developer.apple.com/reference/passkit/pkpaymenttoken)


## <a name="uikit"></a>UIKit

Die folgenden Verbesserungen wurden an dem UIKit-Framework für Watch OS 3 vorgenommen:

- Zur Unterstützung des dynamischen Typs in Bezeichnungen verwenden Textfelder und Textfelder die `PreferredFontForTextStyle` neue-Methode `UIFont` der-Klasse.
- Die `ColorWithDisplayP3` -Methode wurde hinzugefügt, um Wide Color zu unterstützen.


## <a name="related-links"></a>Verwandte Links

- [watchos-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS%20watchos)
- [Neues in watchos 3](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
