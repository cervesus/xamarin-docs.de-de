---
title: Xamarin. Android-Fehler Matrix
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7EBE4C01-8EFC-4B7E-97BA-D879994F59AB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
ms.openlocfilehash: 037b5a8939ab5866a10e7ecc58a1e2a5bdfb5bfd
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757398"
---
# <a name="xamarinandroid-errors-matrix"></a>Xamarin. Android-Fehler Matrix

## <a name="errors-reference"></a>Fehler Referenz

Dieses Dokument enthält einige Informationen zu den verschiedenen Fehlercodes von xamarin.

|Kategorie|Beschreibung|
|--- |--- |
|XA0xxx|mandroid-Fehler|
|XA1xxx|Dateien kopieren/symlinks (projektbezogen) Fehler|
|XA2xxx|Linkerfehler|
|XA3xxx|AOT-Fehler|
|XA4xxx|Code Generierungs Fehler|
|XA5xxx|GCC-und Toolchain-Fehler|
|XA6xxx|Fehler bei den internen mandroid-Tools|
|XA7xxx|Reserviert|
|XA8xxx|Reserviert|
|XA9xxx|Lizenzierungs Fehler|

## <a name="error-codes"></a>Fehlercodes

### <a name="xa0xxx-errors"></a>XA0xxx-Fehler

|Fehlercode|Beschreibung|
|--- |--- |
|XA0000|Unerwarteter Fehler: füllen Sie einen [Fehlerbericht](https://github.com/xamarin/xamarin-android/issues/new)aus.|
|XA0001|"-devname wurde ohne gerätespezifische Aktion bereitgestellt.|
|XA0002|Die Umgebungsvariable "{0}" konnte nicht analysiert werden.|
|XA0003|Der Anwendungsname{0}". exe" steht in Konflikt mit einem SDK-oder produktassemblynamen (. dll).|
|XA0004|Die neue refcounting-Logik erfordert auch, dass Sgen aktiviert ist.|
|XA0005|Das Ausgabeverzeichnis "{0}" ist nicht vorhanden.|
|XA0006|Es gibt keine Debug-Plattform unter "{0}", verwenden Sie "--Platform = Plat", um das SDK anzugeben.|
|XA0007|Die Stammassembly '{0}' ist nicht vorhanden.|
|XA0008|Sie sollten nur eine Stammassembly bereitstellen.|
|XA0009|Fehler beim Laden von Assemblys: {0}.|
|XA0010|Die Befehlszeilenargumente konnten nicht analysiert werden: {0}.|
|XA0011|{0}wurde mit einer neueren Laufzeit ({1}) erstellt, als MonoTouch unterstützt.|
|XA0012|Zum Vervollständigen von "{0}" sind unvollständige Daten angegeben.|
|XA0013|Die Profil Erstellungs Unterstützung erfordert auch, dass Sgen aktiviert ist.|
|XA0014|IOS {0} unterstützt nicht das Entwickeln von Anwendungen, die auf ARMv6 abzielen.|
|XA0020|Der mandroid-Pfad konnte nicht bestimmt werden.|
|XA0100|Embedendnativelibrary '{0}' ist im Android-Anwendungsprojekt ungültig. Verwenden Sie stattdessen "androidnativelibrary".|

### <a name="xa1xxx-errors"></a>XA1xxx-Fehler

|Fehlercode|Beschreibung|
|--- |--- |
|XA1001|Eine Anwendung wurde im angegebenen Verzeichnis nicht gefunden.|
|XA1002|Es konnten keine symlinks erstellt werden, Dateien wurden kopiert.|
|XA1003|Die Anwendung "{0}" konnte nicht beendet werden. Möglicherweise müssen Sie die Anwendung manuell beenden.|
|XA1004|Die Liste der installierten Anwendungen konnte nicht gefunden werden.|
|XA1005|Die Anwendung '{0}' auf dem Gerät '{1}' konnte nicht beendet werden {2}:. Möglicherweise müssen Sie die Anwendung manuell beenden.|
|XA1006|Die Anwendung "{0}" konnte nicht auf dem Gerät "{1}" installiert werden {2}:.|
|XA1007|Fehler beim Starten der Anwendung '{0}' auf dem Gerät '{1}': {2}. Sie können die Anwendung trotzdem manuell starten, indem Sie darauf tippen.|
|XA1008|Fehler beim Starten des Simulators {0}:.|
|XA1009|Die Assembly "{0}" konnte nicht nach "{1}" kopiert werden {2}: "".|
|XA1010|Die Assembly '{0}' konnte nicht geladen werden {1}:.|
|XA1011|Fehlende Ressourcen Datei konnte nicht hinzugefügt werden{0}: "".|
|XA1101|Die APP konnte nicht gestartet werden.|
|XA1102|An die APP konnte nicht angefügt werden (um Sie zu {0}beenden):.|
|XA1103|Trennen nicht möglich.|
|XA1104|Paket konnte nicht gesendet werden {0}:.|
|XA1105|Unerwarteter Antworttyp.|
|XA1106|Die Liste der Anwendungen auf dem Gerät konnte nicht gefunden werden: Timeout bei der Anforderung.|
|XA1107|Fehler beim Starten der Anwendung.|
|XA1201|Der Simulator konnte nicht geladen werden {0}:.|
|XA1301|Die native Bibliothek{0}' '{1}() wurde ignoriert, da Sie nicht mit der aktuellen buildarchitektur (en{2}) () identisch ist.|

### <a name="xa2xxx-errors"></a>XA2xxx-Fehler

|Fehlercode|Beschreibung|
|--- |--- |
|XA2001|Assemblys konnten nicht verknüpft werden.|
|XA2002|Der Verweis kann nicht aufgelöst {0}werden:.|
|XA2003|Die Option{0}"" wird ignoriert, da die Verknüpfung deaktiviert ist.|
|XA2004|Die zusätzliche Linker-Definitions{0}Datei "" wurde nicht gefunden.|
|XA2005|Definitionen von '{0}' konnten nicht analysiert werden.|
|XA2006|Der Verweis auf das Metadatenelement '{0}' (in '{1}' definiert{2}) von ' ' konnte nicht aufgelöst werden.|

### <a name="xa3xxx-errors"></a>XA3xxx-Fehler

Dies sind AOT-Fehler.

|Fehlercode|Beschreibung|
|--- |--- |
|XA3001|Die Assembly "{0}" konnte nicht gefunden werden.|
|XA3002|AOT-Einschränkung: Die{0}-Methode muss statisch sein, da Sie mit [monopinvokecallback] versehen ist.|
|XA3003|In Konflikt stehende--Debug-und--llvm-Optionen. Das weiche Debuggen ist deaktiviert.|

### <a name="xa4xxx-errors"></a>XA4xxx-Fehler

Dabei handelt es sich um Code Generierungs Fehler.

|Fehlercode|Beschreibung|
|--- |--- |
|XA4001|Die Hauptvorlage konnte nicht in "{0}" erweitert werden.|
|XA4101|Die Registrierungsstelle kann keine Signatur für den Typ{0}"" erstellen.|
|XA4102|Die Registrierungsstelle hat einen ungültigen{0}Typ "" in der Signatur{2}für die Methode "" gefunden. Verwenden Sie{1}stattdessen "".|
|XA4103|Die Registrierungsstelle hat einen ungültigen{0}Typ "" in der Signatur{2}für die Methode "" gefunden: Der Typ implementiert inativeobject, aber keinen Konstruktor, der zwei (IntPtr, bool) Argumente annimmt.|
|XA4104|Die Registrierungsstelle kann den Rückgabewert für den Typ{0}"" in der Signatur für{1}die Methode "" nicht Mars Hallen.|
|XA4105|Die Registrierungsstelle kann den Parameter vom Typ "{0}" in der Signatur für die{1}Methode "" nicht Mars Hallen.|
|XA4106|Die Registrierungsstelle kann den Rückgabewert für die Struktur{0}"" in der Signatur für{1}die Methode "" nicht Mars Hallen.|
|XA4107|Die Registrierungsstelle kann den Parameter vom Typ "{0}" in der Signatur für die{1}Methode "" nicht Mars Hallen.|
|XA4108|Die Registrierungsstelle kann den objectivec-Typ für den verwalteten{0}Typ ' ' nicht abrufen.|
|XA4109|Der generierte Registrierungscode konnte nicht kompiliert werden. Melden Sie einen [Fehlerbericht](http://bugzilla.xamarin.com).|
|XA4110|Die Registrierungsstelle kann den out-Parameter vom Typ{0}"" in der Signatur für{1}die Methode "" nicht Mars Hallen.|
|XA4111|Die Registrierungsstelle kann keine Signatur für den Typ{0}"" in der{1}-Methode erstellen.|
|XA4200|ACW kann nur für ' CLAAS '-Typen generiert werden.|
|XA4201|Der jni-Name für den Typ {0}kann nicht ermittelt werden.|
|XA4203|Der angegebene Typname muss voll qualifiziert sein.|
|XA4204|Der Schnittstellentyp "{0}" kann nicht aufgelöst werden. Möglicherweise fehlt ein Assemblyverweis.|
|XA4205|[Exportfield] kann nur für Methoden mit 0-Parametern verwendet werden.|
|XA4206|[Export] kann nicht für einen generischen Typ verwendet werden.|
|XA4207|[Exportfield] kann nicht für einen generischen Typ verwendet werden.|
|XA4208|[Java. Interop. exportfieldattribute] kann nicht in einer Methode verwendet werden, die "void" zurückgibt.|
|XA4209|Fehler beim Erstellen von javatypeingefo für Klasse {0} : aufgrund {1}von.|
|XA4210|Sie müssen einen Verweis auf Mono. Android. Export. dll hinzufügen, wenn Sie Export Attribute oder exportfieldattribute verwenden.|
|XA4211|Androidmanifest. XML //uses-sdk/@android:targetSdkVersion '{0}' ist kleiner als $ (TargetFrameworkVersion) '{1}'. Verwenden der API{1} -für die ACW-Kompilierung.|

### <a name="xa5xxx-errors"></a>XA5xxx-Fehler

|Fehlercode|Beschreibung|
|--- |--- |
|XA5101|Der Compiler{0}' ' fehlt. Installieren Sie das Android-NDK.|
|XA5102|Fehler bei der Konvertierung von einer Assembly in systemeigenen Code. Melden Sie einen [Fehlerbericht](http://bugzilla.xamarin.com).|
|XA5103|Die Datei "{0}" konnte nicht kompiliert werden. Melden Sie einen [Fehlerbericht](http://bugzilla.xamarin.com).|
|XA5201|Fehler beim nativen verknüpfen. Überprüfen Sie die für gcc bereitgestellten Benutzerflags:{0}|
|XA5202|Fehler beim nativen verknüpfen. Überprüfen Sie das Buildprotokoll.|
|XA5303|Native Verknüpfungs Warnung {0}:.|
|XA5300|Android SDK nicht gefunden oder nicht vollständig installiert.|
|XA5301|Fehlendes ' Strip '-Tool. Installieren Sie die Xcode-Komponente "Befehlszeilen Tools".|
|XA5302|Fehlendes dsymutil-Tool. Installieren Sie die Xcode-Komponente "Befehlszeilen Tools".|
|XA5203|Fehler beim Generieren der Debugsymbole (dsym-Verzeichnis). Überprüfen Sie das Buildprotokoll.|
|XA5204|Fehler beim Entfernen der letzten Binärdatei. Überprüfen Sie das Buildprotokoll.|
|XA5205|Fehlendes ' AAPT '-Tool. Installieren Sie das Android SDK Build-Tools-Paket.|
|XA5206|{0} Das Android- {1} Ressourcenverzeichnis ist nicht vorhanden.|
|XA5207|{0} Die Java- {1} Bibliotheksdatei ist nicht vorhanden.|
|XA5208|Fehler beim herunterladen. Laden Sie {0} es herunter, und fügen {1} Sie es in das Verzeichnis ein.|
|XA5209|Fehler beim Entzippen. Laden Sie {0} es herunter, und extrahieren {1} Sie es in das Verzeichnis.|
|XA5210|{0} Die native Bibliotheks {1} Datei ist nicht vorhanden.|

### <a name="xa6xxx-errors"></a>XA6xxx-Fehler

|Fehlercode|Beschreibung|
|--- |--- |
|XA6001|Die laufende Version von Cecil unterstützt das Entfernen von Assemblys nicht|
|XA6002|Die Assembly "{0}" konnte nicht entfernt werden.|
|XA6003|UnauthorizedAccessException-Nachricht.|

### <a name="xa9xxx-errors"></a>XA9xxx-Fehler

Diese Fehlercodes sind Lizenzierungs-und Aktivierungs Fehler.

#### <a name="xa9000"></a>XA9000

 **Zufügen** Lizenz abgelaufen

 **Aktiviert während:** Build

|Einsteiger|Min|Business (Testversion)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|WARNING|WARNING|WARNING|WARNING|WARNING|

#### <a name="xa9001"></a>XA9001

 **Zufügen** Testversion abgelaufen

 **Aktiviert während:** Build

|Einsteiger|Min|Business (Testversion)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|WARNING|WARNING|WARNING|WARNING|WARNING|

#### <a name="xa9002"></a>XA9002

 **Zufügen** AndroidJavaSource

 **Aktiviert während:** Build

|Einsteiger|Min|Business (Testversion)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|Fehler|OK|OK|OK|OK|

 **Zufügen** AndroidJavaLibrary

 **Aktiviert während:** Build

|Einsteiger|Min|Business (Testversion)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|Fehler|OK|OK|OK|OK|

 **Wenn die JAR-Datei einer bindungsassembly eingebettet ist, wird dies zur Paketzeit und nicht zur Buildzeit abgefangen.**

 **Zufügen** AndroidNativeLibrary

 **Aktiviert während:** Build

|Einsteiger|Min|Business (Testversion)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|Fehler|OK|OK|OK|OK|

#### <a name="xa9003"></a>XA9003

 **Zufügen** System.Runtime.Serialization

 **Aktiviert während:** Package

|Einsteiger|Min|Business (Testversion)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|Fehler|Fehler|OK|OK|OK|

 **Zufügen** System.ServiceModel.Web

 **Aktiviert während:** Package

|Einsteiger|Min|Business (Testversion)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|Fehler|Fehler|OK|OK|OK|

 **Zufügen** Mono. Data. TDS

 **Aktiviert während:** Build

|Einsteiger|Min|Business (Testversion)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|Fehler|Fehler|OK|OK|OK|

 **Auf dies wird von "System. Data. dll" verwiesen, was zulässig ist.**

 **Zufügen** Mono.Android.Export

 **Aktiviert während:** Build

|Einsteiger|Min|Business (Testversion)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|Fehler|OK|OK|OK|OK|

#### <a name="xa9004"></a>XA9004

 **Ursache:** --Profilerstellung

 **Aktiviert während:** Package

|Einsteiger|Min|Business (Testversion)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|Fehler|Fehler|OK|OK|OK|

#### <a name="xa9005"></a>XA9005

 **Zufügen** Größenbeschränkung (32 KB).

 **Aktiviert während:** Package

|Einsteiger|Min|Business (Testversion)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|Fehler|OK|-|-|-|

#### <a name="xa9006"></a>XA9006

 **Zufügen** System. Data. SqlClient-Namespace.

 **Aktiviert während:** Package

|Einsteiger|Min|Business (Testversion)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|Fehler|Fehler|OK|OK|OK|

#### <a name="xa9008"></a>XA9008

 **Zufügen** Der Aufbau von der Befehlszeile aus.

 **Aktiviert während:** Build

|Einsteiger|Min|Business (Testversion)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|Fehler|Fehler|OK|OK|OK|

#### <a name="xa9009"></a>XA9009

 **Zufügen** Fehlende Seriennummer.

 **Aktiviert während:** Testable

|Einsteiger|Min|Business (Testversion)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|Fehler|Fehler|Fehler|Fehler|Fehler|

#### <a name="xa9010"></a>XA9010

 **Zufügen** Ungültige ProductID.

 **Aktiviert während:** Build

|Einsteiger|Min|Business (Testversion)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|Fehler|Fehler|Fehler|Fehler|Fehler|

Äquivalent zu XA9018.

#### <a name="xa9011"></a>XA9011

 **Zufügen** Fehler beim Aktualisieren der Lizenzdatei (in das neue Dateiformat).

 **Aktiviert während:** Aktivierung

|Einsteiger|Min|Business (Testversion)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|Fehler|Fehler|Fehler|Fehler|Fehler|

#### <a name="xa9012"></a>XA9012

 **Zufügen** Kein Internet

 **Aktiviert während:** Aktivierung

|Einsteiger|Min|Business (Testversion)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|Fehler|Fehler|Fehler|Fehler|Fehler|

#### <a name="xa9013"></a>XA9013

 **Zufügen** Unbekannter Fehler.

 **Aktiviert während:** Aktivierung

|Einsteiger|Min|Business (Testversion)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|Fehler|Fehler|Fehler|Fehler|Fehler|

#### <a name="xa9014"></a>XA9014

 **Zufügen** Ungültiger Aktivierungs Code

 **Aktiviert während:** Aktivierung

|Einsteiger|Min|Business (Testversion)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|Fehler|Fehler|Fehler|Fehler|Fehler|

#### <a name="xa9017"></a>XA9017

 **Zufügen** Der Aktivierungsserver gibt keine gültige Lizenz zurück.

 **Aktiviert während:** Aktivierung

|Einsteiger|Min|Business (Testversion)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|Fehler|Fehler|Fehler|Fehler|Fehler|

#### <a name="xa9018"></a>XA9018

**Zufügen** Ungültige Lizenz

**Aktiviert während:** Build

|Einsteiger|Min|Business (Testversion)|Geschäftlich|Enterprise|
|--- |--- |--- |--- |--- |
|-|-|Fehler|-|-|
