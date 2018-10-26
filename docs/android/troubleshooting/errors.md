---
title: Matrix der Xamarin.Android-Fehler
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7EBE4C01-8EFC-4B7E-97BA-D879994F59AB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
ms.openlocfilehash: b51e8b3d931ccbb511afe7d06d9be66fa104fb46
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121840"
---
# <a name="xamarinandroid-errors-matrix"></a>Matrix der Xamarin.Android-Fehler

## <a name="errors-reference"></a>Referenz zu Fehlern

Dieses Dokument enthält einige Informationen auf die verschiedenen Fehlercodes von Xamarin.

|Kategorie|Beschreibung|
|--- |--- |
|XA0xxx|Mandroid-Fehler|
|XA1xxx|Kopieren der Datei / symbolische Verknüpfungen (Projekt verknüpft) Fehler|
|XA2xxx|Linkerfehler|
|XA3xxx|AOT-Fehler|
|XA4xxx|Fehler in Code-Generierung|
|XA5xxx|GCC und Toolkette-Fehler|
|XA6xxx|Mandroid-interne Tools-Fehler|
|XA7xxx|Reserviert|
|XA8xxx|Reserviert|
|XA9xxx|Lizenzierungsfehler|


## <a name="error-codes"></a>Fehlercodes

### <a name="xa0xxx-errors"></a>XA0xxx-Fehler

|Fehlercode|Beschreibung|
|--- |--- |
|XA0000|Unerwarteter Fehler: Füllen einer [Fehlerbericht](http://bugzilla.xamarin.com).|
|XA0001|"-Devname ohne gerätespezifischen Aktion bereitgestellt wurde.|
|XA0002|Konnte nicht analysiert werden. die Umgebungsvariable "{0}".|
|XA0003|Name der Anwendung "{0}.exe" steht in Konflikt mit einem Namen der Assembly (.dll) SDK oder eines Produkts.|
|XA0004|Neue referenzzählung Logik erfordert Sgen zu aktiviert werden.|
|XA0005|Das Ausgabeverzeichnis '{0}' ist nicht vorhanden.|
|XA0006|Es gibt keine Entwickler-Plattform unter '{0}", verwenden – Plattform = PLAT zum Angeben des SDK|
|XA0007|Die stammassembly "{0}' ist nicht vorhanden.|
|XA0008|Sie sollten nur eine stammassembly bereitstellen.|
|XA0009|Fehler beim Laden von Assemblys: {0}.|
|XA0010|Die Befehlszeilenargumente konnte nicht analysiert werden: {0}.|
|XA0011|{0} mit einer neueren Laufzeit erstellt wurde ({1}) als MonoTouch unterstützt.|
|XA0012|Unvollständige Daten werden bereitgestellt, abgeschlossen '{0}".|
|XA0013|Profilerstellung erfordert Sgen zu aktiviert werden.|
|XA0014|iOS {0} Erstellen von Anwendungen, die für die ARMv6 Zielplattform nicht unterstützt.|
|XA0020|Mandroid-Pfad konnte nicht ermittelt werden.|
|XA0100|EmbeddedNativeLibrary "{0}' ist ungültig. in der Android-Anwendungsprojekt. Verwenden Sie stattdessen AndroidNativeLibrary.|

### <a name="xa1xxx-errors"></a>XA1xxx-Fehler

|Fehlercode|Beschreibung|
|--- |--- |
|XA1001|Eine Anwendung wurde im angegebenen Verzeichnis nicht gefunden werden.|
|XA1002|Symbolische Verknüpfungen konnte nicht erstellt werden, werden Dateien kopiert wurden.|
|XA1003|Die Anwendung konnte nicht beenden "{0}". Sie müssen möglicherweise die Anwendung manuell zu beenden.|
|XA1004|Die Liste der installierten Anwendungen konnte nicht abgerufen werden.|
|XA1005|Die Anwendung konnte nicht beenden "{0}"auf dem Gerät"{1}': {2}. Sie müssen möglicherweise die Anwendung manuell zu beenden.|
|XA1006|Die Anwendung konnte nicht installiert '{0}"auf dem Gerät"{1}': {2}.|
|XA1007|Fehler beim Starten der Anwendung "{0}"auf dem Gerät"{1}': {2}. Sie können die Anwendung weiterhin manuell durch Tippen gestartet.|
|XA1008|Fehler beim Starten des Simulators: {0}.|
|XA1009|Die Assembly konnte nicht kopiert "{0}'to'{1}': {2}.|
|XA1010|Die Assembly nicht geladen werden kann '{0}': {1}.|
|XA1011|Fehlende Ressourcendatei konnte nicht hinzugefügt werden: "{0}".|
|XA1101|App konnte nicht gestartet werden.|
|XA1102|Konnte nicht angefügt werden, um die app (für die sie beenden möchten): {0}.|
|XA1103|Konnte nicht getrennt werden.|
|XA1104|Paket konnte nicht gesendet: {0}.|
|XA1105|Unerwarteter Antworttyp.|
|XA1106|Liste der Anwendungen konnte nicht auf dem Gerät abgerufen werden: Timeout bei Anforderung.|
|XA1107|Anwendung konnte nicht gestartet.|
|XA1201|Den Simulator konnte nicht geladen werden: {0}.|
|XA1301|Native Bibliothek '{0}"({1}) wurde ignoriert, da sie nicht den aktuellen Build Architecture(s) übereinstimmt ({2}).|

### <a name="xa2xxx-errors"></a>XA2xxx-Fehler

|Fehlercode|Beschreibung|
|--- |--- |
|XA2001|Assemblys konnte nicht verknüpft werden.|
|XA2002|Verweis kann nicht aufgelöst: {0}.|
|XA2003|Option "{0}" wird ignoriert, da die Verknüpfung deaktiviert ist.|
|XA2004|Zusätzliche Linker-Definitionsdatei '{0}' konnte nicht gefunden werden.|
|XA2005|Definitionen von "{0}' konnte nicht analysiert werden.|
|XA2006|Verweis auf das Metadatenelement '{0}"(definiert"{1}") aus"{2}' konnte nicht aufgelöst werden.|

### <a name="xa3xxx-errors"></a>XA3xxx-Fehler

Hierbei handelt es sich um AOT-Fehler.

|Fehlercode|Beschreibung|
|--- |--- |
|XA3001|Konnte kein AOT der Assembly "{0}".|
|XA3002|AOT-Einschränkung: Methode "{0}' muss statisch sein, da sie mit [MonoPInvokeCallback] markiert ist.|
|XA3003|Es wurden widersprüchliche--Debug "und"--Llvm-Optionen. Soft-debugging ist deaktiviert.|


### <a name="xa4xxx-errors"></a>XA4xxx-Fehler

Hierbei handelt es sich um Code schemagenerierung aufgetretenen Fehler.

|Fehlercode|Beschreibung|
|--- |--- |
|XA4001|Die Hauptvorlage konnte nicht auf expansed "{0}".|
|XA4101|Die Registrierungsstelle eine Signatur für den Typ kann nicht erstellt werden kann '{0}".|
|XA4102|Die Registrierungsstelle finden Sie einen ungültigen Typ "{0}"in der Signatur für die Methode"{2}". Mit "{1}" stattdessen.|
|XA4103|Die Registrierungsstelle finden Sie einen ungültigen Typ "{0}"in der Signatur für die Methode"{2}': der Typ INativeObject implementiert, jedoch verfügt nicht über einen Konstruktor, der zwei übernimmt (IntPtr, Bool) Argumente.|
|XA4104|Die Registrierungsstelle nicht marshallen kann den Rückgabewert für Typ '{0}"in der Signatur für die Methode"{1}".|
|XA4105|Die Registrierungsstelle nicht marshallen den Parameter vom Typ "{0}"in der Signatur für die Methode"{1}".|
|XA4106|Die Registrierungsstelle nicht marshallen kann den Rückgabewert für Struktur '{0}"in der Signatur für die Methode"{1}".|
|XA4107|Die Registrierungsstelle nicht marshallen den Parameter vom Typ "{0}"in der Signatur für die Methode"{1}".|
|XA4108|Die Registrierungsstelle den ObjectiveC-Typ für verwalteten Typ kann nicht abgerufen werden kann '{0}".|
|XA4109|Fehler beim Kompilieren des Codes generierte Registrierungsstelle. Melden Sie bitte eine [Fehlerbericht](http://bugzilla.xamarin.com).|
|XA4110|Die Registrierungsstelle nicht marshallen kann den Out-Parameter des Typs "{0}"in der Signatur für die Methode"{1}".|
|XA4111|Die Registrierungsstelle eine Signatur für den Typ kann nicht erstellt werden kann '{0}"in der Methode"{1}".|
|XA4200|INHALTSFEHLERs können nur für 'Claas'-Typen generiert werden.|
|XA4201|JNI-Name für den Typ kann nicht bestimmt {0}.|
|XA4203|Der angegebenen Typnamen muss vollqualifiziert sein.|
|XA4204|Schnittstellentyp kann nicht aufgelöst "{0}". Möglicherweise fehlt ein Assemblyverweis.|
|XA4205|[ExportField] kann nur für Methoden mit 0 Parametern verwendet werden.|
|XA4206|[Exportieren] kann nicht für einen generischen Typ verwendet werden.|
|XA4207|[ExportField] kann nicht für einen generischen Typ verwendet werden.|
|XA4208|[Java.Interop.ExportFieldAttribute] kann nicht verwendet werden, auf eine Methode "void" zurückgeben.|
|XA4209|Fehler beim Erstellen des JavaTypeInfo für Klasse: {0} aufgrund von {1}.|
|XA4210|Sie müssen einen Verweis auf Mono.Android.Export.dll hinzufügen, wenn Sie ExportAttribute oder ExportFieldAttribute verwenden.|
|XA4211|"Androidmanifest.xml" //uses-sdk/@android:targetSdkVersion "{0}'ist kleiner als $(TargetFrameworkVersion)'{1}". Mithilfe von API -{1} Inhaltsfehler Kompilierung.|


### <a name="xa5xxx-errors"></a>XA5xxx-Fehler

|Fehlercode|Beschreibung|
|--- |--- |
|XA5101|Fehlendes '{0}"Compiler. Installieren Sie die Android NDK.|
|XA5102|Fehler bei der Konvertierung aus der Assembly in systemeigenen Code. Melden Sie bitte eine [Fehlerbericht](http://bugzilla.xamarin.com).|
|XA5103|Fehler beim Kompilieren Sie die Datei "{0}". Melden Sie bitte eine [Fehlerbericht](http://bugzilla.xamarin.com).|
|XA5201|Fehler bei der systemeigenen verknüpfen. Überprüfen Sie die Benutzerflags bereitgestellt, um den Gcc: {0}|
|XA5202|Fehler bei der systemeigenen verknüpfen. Überprüfen Sie das Buildprotokoll an.|
|XA5303|Verknüpfen die Warnung Native: {0}.|
|XA5300|Android SDK nicht gefunden oder nicht vollständig installiert.|
|XA5301|Fehlendes 'entfernen'-Tool an. Installieren Sie Xcode "Befehlszeilentools"-Komponente.|
|XA5302|Fehlende 'Dsymutil'-Tool. Installieren Sie Xcode "Befehlszeilentools"-Komponente.|
|XA5203|Fehler beim Generieren von der Debugsymbolen (dSYM-Verzeichnis). Überprüfen Sie das Buildprotokoll an.|
|XA5204|Fehler bei, um die endgültige Binärdatei zu entfernen. Überprüfen Sie das Buildprotokoll an.|
|XA5205|Fehlende 'Aapt'-Tool. Installieren Sie das Android SDK Build-Tools-Paket.|
|XA5206|{0}. Android-Ressourcenverzeichnis {1} ist nicht vorhanden.|
|XA5207|{0}. Java-Bibliotheksdatei {1} ist nicht vorhanden.|
|XA5208|Fehler beim Herunterladen. Laden Sie {0} , und fügen Sie es auf die {1} Verzeichnis.|
|XA5209|Fehler beim Entzippen. Laden Sie {0} und extrahieren Sie es der {1} Verzeichnis.|
|XA5210|{0}. Native Bibliotheksdatei {1} ist nicht vorhanden.|

### <a name="xa6xxx-errors"></a>XA6xxx-Fehler

|Fehlercode|Beschreibung|
|--- |--- |
|XA6001|Cecil ausgeführte Version unterstützt keine Assembly entfernen.|
|XA6002|Konnte keine Assembly entfernen '{0}".|
|XA6003|UnauthorizedAccessException-Meldung.|

### <a name="xa9xxx-errors"></a>XA9xxx-Fehler

Diese Fehlercodes handelt es sich um lizenzierungs- und aktivierungsprozessen Fehler.


#### <a name="xa9000"></a>XA9000

 **Ursache:** Lizenz abgelaufen

 **Während überprüft:** erstellen

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|WARNUNG|WARNUNG|WARNUNG|WARNUNG|WARNUNG|


#### <a name="xa9001"></a>XA9001

 **Ursache:** Testversion ist abgelaufen

 **Während überprüft:** erstellen

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|WARNUNG|WARNUNG|WARNUNG|WARNUNG|WARNUNG|



#### <a name="xa9002"></a>XA9002

 **Ursache:** AndroidJavaSource

 **Während überprüft:** erstellen

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|OK|OK|OK|OK|

 **Ursache:** AndroidJavaLibrary

 **Während überprüft:** erstellen

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|OK|OK|OK|OK|

 **Wenn eine bindungsassembly die JAR-Datei eingebettet ist, wird dies zum Zeitpunkt der Paketerstellung nicht Buildzeit abgefangen.**

 **Ursache:** AndroidNativeLibrary

 **Während überprüft:** erstellen

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|OK|OK|OK|OK|


#### <a name="xa9003"></a>XA9003

 **Ursache:** System.Runtime.Serialization

 **Während überprüft:** Paket

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|FEHLER|OK|OK|OK|

 **Ursache:** System.ServiceModel.Web

 **Während überprüft:** Paket

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|FEHLER|OK|OK|OK|

 **Ursache:** Mono.Data.Tds

 **Während überprüft:** erstellen

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|FEHLER|OK|OK|OK|

 **Dies ist "System.Data.dll", verweist dies zulässig ist**

 **Ursache:** Mono.Android.Export

 **Während überprüft:** erstellen

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|OK|OK|OK|OK|



#### <a name="xa9004"></a>XA9004

 **Ursache:** – profilerstellung

 **Während überprüft:** Paket

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|FEHLER|OK|OK|OK|



#### <a name="xa9005"></a>XA9005

 **Ursache:** größenbeschränkung (32 kb).

 **Während überprüft:** Paket

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|OK|-|-|-|



#### <a name="xa9006"></a>XA9006

 **Ursache:** Namespace "System.Data.SqlClient".

 **Während überprüft:** Paket

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|FEHLER|OK|OK|OK|


#### <a name="xa9008"></a>XA9008

 **Ursache:** über Befehlszeile erstellen.

 **Während überprüft:** erstellen

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|FEHLER|OK|OK|OK|


#### <a name="xa9009"></a>XA9009

 **Ursache:** Seriennummer fehlt.

 **Während überprüft:** testable

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|FEHLER|FEHLER|FEHLER|FEHLER|


#### <a name="xa9010"></a>XA9010

 **Ursache:** ungültige "ProductID".

 **Während überprüft:** erstellen

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|FEHLER|FEHLER|FEHLER|FEHLER|

Äquivalent zu XA9018.



#### <a name="xa9011"></a>XA9011

 **Ursache:** Fehler beim Aktualisieren der Lizenzdatei (für den neuen Dateiformat vor).

 **Während überprüft:** Aktivierung

|Starter|Indie|Business(Trial)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|FEHLER|FEHLER|FEHLER|FEHLER|FEHLER|

#### <a name="xa9012"></a>XA9012

 **Ursache:** kein Internet

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

 **Ursache:** Ungültige Gültigkeitsdauer des Aktivierungscodes

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

