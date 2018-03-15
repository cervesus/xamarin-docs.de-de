---
title: Xamarin.Android Errors Matrix
ms.topic: article
ms.prod: xamarin
ms.assetid: 7EBE4C01-8EFC-4B7E-97BA-D879994F59AB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/13/2018
ms.openlocfilehash: d113e7747f656eda2bedb88574f4b2027616add8
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
# <a name="xamarinandroid-errors-matrix"></a>Xamarin.Android Errors Matrix

## <a name="errors-reference"></a>Fehler-Referenz

Dieses Dokument enthält einige Informationen zu den verschiedenen Fehlercodes von Xamarin.

|Kategorie|Beschreibung|
|--- |--- |
|XA0xxx|Mandroid Fehler|
|XA1xxx|Kopieren der Datei / symbolische Verknüpfungen (Projekt beziehen) Fehler|
|XA2xxx|Linkerfehler|
|XA3xxx|AOT-Fehler|
|XA4xxx|Generierung von Codefehlern|
|XA5xxx|GCC und Toolkette-Fehler|
|XA6xxx|Mandroid interne Tools Fehler|
|XA7xxx|Reserviert|
|XA8xxx|Reserviert|
|XA9xxx|Lizenzierungsfehlern|


## <a name="error-codes"></a>Fehlercodes

### <a name="xa0xxx-errors"></a>XA0xxx-Fehler

|Fehlercode|Beschreibung|
|--- |--- |
|XA0000|Unerwarteter Fehler: Füllen einer [Bericht](http://bugzilla.xamarin.com).|
|XA0001|"-Devname ohne Eingreifen gerätespezifischen bereitgestellt wurde.|
|XA0002|Die Umgebungsvariable '{0}' konnte nicht analysiert werden.|
|XA0003|Anwendung "{0} .exe" steht in Konflikt mit einem Namen der Assembly (.dll) SDK oder Produkt.|
|XA0004|Die Logik der neuen Refcounting erfordert Sgen zu aktiviert werden.|
|XA0005|Das Ausgabeverzeichnis '{0}' ist nicht vorhanden.|
|XA0006|Keine entwickl Plattform zum '{0}' vorhanden ist, verwenden Sie – Plattform = Plattform zum Angeben des SDKS|
|XA0007|Die stammassembly '{0}' ist nicht vorhanden.|
|XA0008|Sie sollten eine stammassembly bereitstellen.|
|XA0009|Fehler beim Laden von Assemblys: {0}.|
|XA0010|Die Befehlszeilenargumente konnte nicht analysiert werden: {0}.|
|XA0011|{0} wurde in einer neueren Runtime (\ {1\}) erstellt, als MonoTouch unterstützt.|
|XA0012|Unvollständige Daten werden bereitgestellt, um {0} abzuschließen.|
|XA0013|Profilerstellungsunterstützung erfordert Sgen zu aktiviert werden.|
|XA0014|Erstellen von Anwendungen für ARMv6 unterstützt iOS {0} nicht.|
|XA0020|Mandroid Pfad konnte nicht ermittelt werden.|
|XA0100|EmbeddedNativeLibrary '{0}' ist ungültig in Android-Anwendungsprojekt. Verwenden Sie stattdessen AndroidNativeLibrary.|

### <a name="xa1xxx-errors"></a>XA1xxx-Fehler

|Fehlercode|Beschreibung|
|--- |--- |
|XA1001|Eine Anwendung wurde im angegebenen Verzeichnis nicht gefunden werden.|
|XA1002|Symbolische Verknüpfungen konnte nicht erstellt werden, werden Dateien kopiert wurden.|
|XA1003|Die Anwendung "{0}" konnte nicht beendet werden. Möglicherweise wird die Anwendung manuell beenden.|
|XA1004|Die Liste der installierten Anwendungen konnte nicht abgerufen werden.|
|XA1005|Die Anwendung "{0}" auf dem Gerät "\ {1\}" konnte nicht kill: \ {2\}. Möglicherweise wird die Anwendung manuell beenden.|
|XA1006|Die Anwendung "{0}" auf dem Gerät "\ {1\}" konnte nicht installiert: \ {2\}.|
|XA1007|Fehler beim Starten der Anwendung "{0}" auf dem Gerät "\ {1\}": \ {2\}. Sie können die Anwendung immer noch durch Tippen auf darauf manuell starten.|
|XA1008|Fehler beim Starten Sie des Simulators: {0}.|
|XA1009|Die Assembly '{0}', "\ {1\}" konnte nicht kopiert: \ {2\}.|
|XA1010|Die Assembly '{0}' konnte nicht geladen werden: \ {1\}.|
|XA1011|Fehlende Ressourcendatei konnte nicht hinzugefügt werden: "{0}".|
|XA1101|App konnte nicht gestartet werden.|
|XA1102|Konnte nicht angefügt werden, zu der app (für ihn zu beenden): {0}.|
|XA1103|Konnte nicht getrennt werden.|
|XA1104|Paket konnte nicht gesendet: {0}.|
|XA1105|Unerwarteter Antworttyp.|
|XA1106|Liste der Anwendungen auf dem Gerät konnte nicht abgerufen: Timeout.|
|XA1107|Anwendung konnte nicht gestartet werden.|
|XA1201|Den Simulator konnten nicht geladen werden: {0}.|
|XA1301|Systemeigene Bibliothek (\ {1\}) "{0}" wurde ignoriert, da sie nicht die aktuellen Build Architecture(s) (\ {2\}) übereinstimmt.|

### <a name="xa2xxx-errors"></a>XA2xxx-Fehler

|Fehlercode|Beschreibung|
|--- |--- |
|XA2001|Assemblys können nicht verknüpft werden.|
|XA2002|Verweis kann nicht aufgelöst werden: {0}.|
|XA2003|Option '{0}' wird ignoriert, da verknüpfen deaktiviert ist.|
|XA2004|Zusätzliche Linker-Definitionsdatei "{0}" konnte nicht gefunden werden.|
|XA2005|Definitionen von "{0}" konnte nicht analysiert werden.|
|XA2006|Verweis auf das Metadatenelement '{0}"(definiert in"\ {1\}") von"\ {2\}"konnte nicht aufgelöst werden.|

### <a name="xa3xxx-errors"></a>XA3xxx-Fehler

Hierbei handelt es sich um AOT-Fehler.

|Fehlercode|Beschreibung|
|--- |--- |
|XA3001|Konnte nicht AOT der Assembly "{0}".|
|XA3002|AOT Einschränkung: Methode "{0}" muss statisch sein, da er mit [MonoPInvokeCallback] ergänzt wird.|
|XA3003|Es wurden widersprüchliche--Debug- und--Llvm-Optionen. Soft-Debuggen ist deaktiviert.|


### <a name="xa4xxx-errors"></a>XA4xxx-Fehler

Hierbei handelt es sich um Code Generierungsfehler.

|Fehlercode|Beschreibung|
|--- |--- |
|XA4001|Die Haupt-Vorlage konnte nicht expansed in "{0}" werden.|
|XA4101|Die Registrierungsstelle kann keine Signatur für den Typ '{0}' erstellt werden.|
|XA4102|Die Registrierungsstelle hat einen ungültigen Typ "{0}" in Signatur für Methode "\ {2\}" gefunden. Verwenden Sie stattdessen "\ {1\}".|
|XA4103|Die Registrierungsstelle gefunden einen ungültigen Typ "{0}" in der Signatur für Methode "\ {2\}": der Typ implementiert INativeObject, jedoch verfügt nicht über einen Konstruktor, der zwei akzeptiert (IntPtr, Bool) Argumente.|
|XA4104|Die Registrierungsstelle Marshallen nicht für Typ '{0}' in der Signatur für die Methode "\ {1\}" den Rückgabewert.|
|XA4105|Die Registrierungsstelle Marshallen nicht die Parameter vom Typ '{0}' in der Signatur für die Methode "\ {1\}".|
|XA4106|Die Registrierungsstelle Marshallen nicht für Struktur '{0}' in der Signatur für die Methode "\ {1\}" den Rückgabewert.|
|XA4107|Die Registrierungsstelle Marshallen nicht die Parameter vom Typ '{0}' in der Signatur für die Methode "\ {1\}".|
|XA4108|Die Registrierungsstelle kann nicht den Typ der ObjectiveC für verwalteten Typ '{0}' abgerufen werden.|
|XA4109|Fehler beim Kompilieren des Codes generierte Registrierungsstelle. Starten Sie bitte eine [Bericht](http://bugzilla.xamarin.com).|
|XA4110|Die Registrierungsstelle Marshallen nicht die Out-Parameter vom Typ '{0}' in der Signatur für die Methode "\ {1\}".|
|XA4111|Die Registrierungsstelle kann keine Signatur für den Typ "{0}" in Methode '\ {1\}' erstellt werden.|
|XA4200|INHALTSFEHLERs kann nur für "Claas"-Typen generiert werden.|
|XA4201|Kann nicht bestimmt JNI-Namen für Typ ' {0} '.|
|XA4203|Der angegebenen Typnamen muss vollqualifiziert sein.|
|XA4204|Kann nicht aufgelöst Schnittstellentyp "{0}". Möglicherweise fehlt ein Assemblyverweis.|
|XA4205|[ExportField] kann nur auf Methoden mit Parametern 0 verwendet werden.|
|XA4206|[Exportieren] kann nicht für einen generischen Typ verwendet werden.|
|XA4207|[ExportField] kann nicht für einen generischen Typ verwendet werden.|
|XA4208|[Java.Interop.ExportFieldAttribute] kann nicht auf eine Methode, die "void" zurückgeben verwendet werden.|
|XA4209|Fehler beim Erstellen von JavaTypeInfo für Klasse: {0} aufgrund von \ {1\}.|
|XA4210|Sie müssen einen Verweis auf Mono.Android.Export.dll hinzufügen, wenn Sie ExportAttribute oder ExportFieldAttribute verwenden.|
|XA4211|AndroidManifest.xml //uses-sdk/@android:targetSdkVersion '{0}' ist kleiner als $(TargetFrameworkVersion) "\ {1\}". Mithilfe von API-\ {1\} für Inhaltsfehler Kompilierung.|


### <a name="xa5xxx-errors"></a>XA5xxx-Fehler

|Fehlercode|Beschreibung|
|--- |--- |
|XA5101|Fehlende '{0}' des Compilers. Installieren Sie Android-NDK.|
|XA5102|Fehler bei der Konvertierung aus der Assembly in systemeigenen Code. Starten Sie bitte eine [Bericht](http://bugzilla.xamarin.com).|
|XA5103|Fehler beim Kompilieren der Datei "{0}". Starten Sie bitte eine [Bericht](http://bugzilla.xamarin.com).|
|XA5201|Fehler bei der systemeigenen verknüpfen. Überprüfen Sie die Benutzerflags bereitgestellt, um den Gcc: {0}|
|XA5202|Fehler bei der systemeigenen verknüpfen. Überprüfen Sie das Buildprotokoll an.|
|XA5303|Systemeigene verknüpfen Warnung: {0}.|
|XA5300|Android-SDK nicht gefunden oder nicht vollständig installiert.|
|XA5301|Tool für den fehlenden "entfernen". Installieren Sie Xcode "Befehlszeilentools"-Komponente.|
|XA5302|Fehlende "Dsymutil"-Tool. Installieren Sie Xcode "Befehlszeilentools"-Komponente.|
|XA5203|Fehler beim Generieren von der Debugsymbolen (dSYM Directory). Überprüfen Sie das Buildprotokoll an.|
|XA5204|Fehler bei, um die endgültige Binärdatei zu entfernen. Überprüfen Sie das Buildprotokoll an.|
|XA5205|Fehlende "Aapt"-Tool. Installieren Sie das Paket für Android-SDK-Buildtools.|
|XA5206|{0}. Android Ressource Directory \ {1\} ist nicht vorhanden.|
|XA5207|{0}. Java-Bibliothek Datei \ {1\} ist nicht vorhanden.|
|XA5208|Fehler beim Download. Herunterladen von {0}, und fügen Sie sie in das Verzeichnis \ {1\}.|
|XA5209|Fehler beim Dekomprimieren. Herunterladen von {0}, und extrahieren Sie es in das Verzeichnis \ {1\}.|
|XA5210|{0}. Systemeigene Bibliothek Datei \ {1\} ist nicht vorhanden.|

### <a name="xa6xxx-errors"></a>XA6xxx-Fehler

|Fehlercode|Beschreibung|
|--- |--- |
|XA6001|Ausgeführte Version eines Cecil unterstützt keine Assembly entfernen.|
|XA6002|Assembly "{0}" konnte nicht entfernt werden.|
|XA6003|UnauthorizedAccessException-Meldung.|

### <a name="xa9xxx-errors"></a>XA9xxx-Fehler

Diese Fehlercodes werden Fehler bei der Lizenzierung und Aktivierung.


#### <a name="xa9000"></a>XA9000

 **Ursache:** -Lizenz abgelaufen

 **Während überprüft:** erstellen

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|WARNUNG|WARNUNG|WARNUNG|WARNUNG|WARNUNG|


#### <a name="xa9001"></a>XA9001

 **Ursache:** Testversion abgelaufen

 **Während überprüft:** erstellen

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|WARNUNG|WARNUNG|WARNUNG|WARNUNG|WARNUNG|



#### <a name="xa9002"></a>XA9002

 **Cause:** AndroidJavaSource

 **Während überprüft:** erstellen

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|OK|OK|OK|OK|

 **Cause:** AndroidJavaLibrary

 **Während überprüft:** erstellen

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|OK|OK|OK|OK|

 **Verfügt die bindungsassembly eine die JAR eingebettet, wird dies zur des Pakets, nicht die Buildzeit abgefangen.**

 **Cause:** AndroidNativeLibrary

 **Während überprüft:** erstellen

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|OK|OK|OK|OK|


#### <a name="xa9003"></a>XA9003

 **Cause:** System.Runtime.Serialization

 **Während überprüft:** Paket

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|FEHLER|OK|OK|OK|

 **Cause:** System.ServiceModel.Web

 **Während überprüft:** Paket

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|FEHLER|OK|OK|OK|

 **Ursache:** Mono.Data.Tds

 **Während überprüft:** erstellen

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|FEHLER|OK|OK|OK|

 **Dies wird durch "System.Data.dll", verwiesen, die zulässig ist**

 **Ursache:** Mono.Android.Export

 **Während überprüft:** erstellen

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|OK|OK|OK|OK|



#### <a name="xa9004"></a>XA9004

 **Ursache:** --profilerstellung

 **Während überprüft:** Paket

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|FEHLER|OK|OK|OK|



#### <a name="xa9005"></a>XA9005

 **Ursache:** maximale Größe (32 kb).

 **Während überprüft:** Paket

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|OK|-|-|-|



#### <a name="xa9006"></a>XA9006

 **Ursache:** System.Data.SqlClient-Namespace.

 **Während überprüft:** Paket

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|FEHLER|OK|OK|OK|


#### <a name="xa9008"></a>XA9008

 **Ursache:** erstellen über die Befehlszeile.

 **Während überprüft:** erstellen

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|FEHLER|OK|OK|OK|


#### <a name="xa9009"></a>XA9009

 **Ursache:** Seriennummer fehlt.

 **Während überprüft:** Untestable

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|FEHLER|FEHLER|FEHLER|FEHLER|


#### <a name="xa9010"></a>XA9010

 **Ursache:** ungültige "ProductID".

 **Während überprüft:** erstellen

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|FEHLER|FEHLER|FEHLER|FEHLER|

XA9018 entspricht.



#### <a name="xa9011"></a>XA9011

 **Ursache:** Fehler beim Aktualisieren der Lizenzdatei (in das neue Dateiformat).

 **Während überprüft:** Aktivierung

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|FEHLER|FEHLER|FEHLER|FEHLER|

#### <a name="xa9012"></a>XA9012

 **Ursache:** keine Internet

 **Während überprüft:** Aktivierung

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|FEHLER|FEHLER|FEHLER|FEHLER|


#### <a name="xa9013"></a>XA9013

 **Ursache:** Unbekannter Fehler

 **Während überprüft:** Aktivierung

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|FEHLER|FEHLER|FEHLER|FEHLER|


#### <a name="xa9014"></a>XA9014

 **Ursache:** Aktivierungscode ungültig

 **Während überprüft:** Aktivierung

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|FEHLER|FEHLER|FEHLER|FEHLER|


#### <a name="xa9017"></a>XA9017

 **Ursache:** Aktivierungsserver eine gültige Lizenz nicht zurückgeben kann.

 **Während überprüft:** Aktivierung

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|FEHLER|FEHLER|FEHLER|FEHLER|


#### <a name="xa9018"></a>XA9018

**Ursache:** ungültige Lizenz

**Während überprüft:** erstellen

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|-|-|FEHLER|-|-|

