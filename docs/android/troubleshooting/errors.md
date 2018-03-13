---
title: Xamarin.Android Errors Matrix
ms.topic: article
ms.prod: xamarin
ms.assetid: 7EBE4C01-8EFC-4B7E-97BA-D879994F59AB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 1c9284f3db1c503b5c88f0d310f916f4c9e3363d
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="xamarinandroid-errors-matrix"></a>Xamarin.Android Errors Matrix

## <a name="errors-reference"></a>Fehler-Referenz

Dieses Dokument enthält einige Informationen zu den verschiedenen Fehlercodes von Xamarin.

<table>
    <thead>
        <tr>
            <td>Kategorie</td>
            <td>Beschreibung</td>
        </tr>
    </thead>
    <tbody>
        <tr><td>XA0xxx</td><td><code>mandroid</code> Fehler</td></tr>
        <tr><td>XA1xxx</td><td>Kopieren der Datei / symbolische Verknüpfungen (Projekt beziehen) Fehler</td></tr>
        <tr><td>XA2xxx</td><td>Linkerfehler</td></tr>
        <tr><td>XA3xxx</td><td>AOT-Fehler</td></tr>
        <tr><td>XA4xxx</td><td>Generierung von Codefehlern</td></tr>
        <tr><td>XA5xxx</td><td>GCC und Toolkette-Fehler</td></tr>
        <tr><td>XA6xxx</td><td><code>mandroid</code> Interne Tools-Fehler</td></tr>
        <tr><td>XA7xxx</td><td>Reserviert</td></tr>
        <tr><td>XA8xxx</td><td>Reserviert</td></tr>
        <tr><td>XA9xxx</td><td>Lizenzierungsfehlern</td></tr>
    </tbody>
</table>

## <a name="error-codes"></a>Fehlercodes

### <a name="xa0xxx-errors"></a>XA0xxx-Fehler

<table>
    <thead>
        <tr><td>Fehlercode</td><td>Beschreibung</td></tr>
    </thead>
    <tbody>
        <tr><td>XA0000</td><td>Unerwarteter Fehler: Bitte geben Sie einen Fehlerbericht an <a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA0001</td><td>"-Devname ohne Eingreifen gerätespezifischen bereitgestellt wurde</td></tr>
        <tr><td>XA0002</td><td>Die Umgebungsvariable '{0}' konnte nicht analysiert werden.</td></tr>
        <tr><td>XA0003</td><td>Anwendung "{0} .exe" steht in Konflikt mit einem Namen der Assembly (.dll) SDK oder Produkt.</td></tr>
        <tr><td>XA0004</td><td>Die Logik der neuen Refcounting erfordert Sgen zu aktiviert werden.</td></tr>
        <tr><td>XA0005</td><td>Das Ausgabeverzeichnis '{0}' ist nicht vorhanden.</td></tr>
        <tr><td>XA0006</td><td>Keine entwickl Plattform zum '{0}' vorhanden ist, verwenden Sie – Plattform = Plattform zum Angeben des SDKS</td></tr>
        <tr><td>XA0007</td><td>Die stammassembly '{0}' ist nicht vorhanden.</td></tr>
        <tr><td>XA0008</td><td>Sie sollten eine stammassembly bereitstellen.</td></tr>
        <tr><td>XA0009</td><td>Fehler beim Laden von Assemblys: {0}</td></tr>
        <tr><td>XA0010</td><td>Die Befehlszeilenargumente konnte nicht analysiert werden: {0}</td></tr>
        <tr><td>XA0011</td><td>{0} wurde als MonoTouch unterstützt, während neuere Common Language Runtime (\ {1\}) erstellt.</td></tr>
        <tr><td>XA0012</td><td>Unvollständige Daten werden bereitgestellt, um abgeschlossen `{0}`.</td></tr>
        <tr><td>XA0013</td><td>Profilerstellungsunterstützung muss Sgen zu aktiviert werden</td></tr>
        <tr><td>XA0014</td><td>Erstellen von Anwendungen für ARMv6 unterstützt iOS {0} nicht.</td></tr>
        <tr><td>XA0020</td><td>Mandroid Pfad konnte nicht ermittelt werden.</td></tr>
        <tr><td>XA0100</td><td>EmbeddedNativeLibrary '{0}' ist ungültig in Android-Anwendungsprojekt. Verwenden Sie stattdessen AndroidNativeLibrary.</td></tr>
    </tbody>
</table>

### <a name="xa1xxx-errors"></a>XA1xxx-Fehler

<table>
    <thead>
        <tr><td>Fehlercode</td><td>Beschreibung</td></tr>
    </thead>
    <tbody>
        <tr><td>XA1001</td><td>Eine Anwendung wurde im angegebenen Verzeichnis nicht gefunden werden.</td></tr>
        <tr><td>XA1002</td><td>Symbolische Verknüpfungen konnte nicht erstellt werden, werden Dateien kopiert wurden</td></tr>
        <tr><td>XA1003</td><td>Die Anwendung "{0}" konnte nicht beendet werden. Möglicherweise wird die Anwendung manuell beenden.</td></tr>
        <tr><td>XA1004</td><td>Die Liste der installierten Anwendungen konnte nicht abgerufen werden.</td></tr>
        <tr><td>XA1005</td><td>Die Anwendung "{0}" auf dem Gerät "\ {1\}" konnte nicht kill: \ {2\}. Möglicherweise wird die Anwendung manuell beenden.</td></tr>
        <tr><td>XA1006</td><td>Die Anwendung "{0}" auf dem Gerät "\ {1\}" konnte nicht installiert: \ {2\}.</td></tr>
        <tr><td>XA1007</td><td>Fehler beim Starten der Anwendung "{0}" auf dem Gerät "\ {1\}": \ {2\}. Sie können die Anwendung immer noch durch Tippen auf darauf manuell starten.</td></tr>
        <tr><td>XA1008</td><td>Fehler beim Starten Sie des Simulators: {0}</td></tr>
        <tr><td>XA1009</td><td>Die Assembly '{0}', "\ {1\}" konnte nicht kopiert: \ {2\}</td></tr>
        <tr><td>XA1010</td><td>Die Assembly '{0}' konnte nicht geladen werden: \ {1\}</td></tr>
        <tr><td>XA1011</td><td>Fehlende Ressourcendatei konnte nicht hinzugefügt werden: "{0}"</td></tr>
        <tr><td>XA1101</td><td>App konnte nicht gestartet werden.</td></tr>
        <tr><td>XA1102</td><td>Konnte nicht angefügt werden, zu der app (für ihn zu beenden): {0}</td></tr>
        <tr><td>XA1103</td><td>Konnte nicht getrennt</td></tr>
        <tr><td>XA1104</td><td>Paket konnte nicht gesendet: {0}</td></tr>
        <tr><td>XA1105</td><td>Unerwarteter Antworttyp</td></tr>
        <tr><td>XA1106</td><td>Liste der Anwendungen auf dem Gerät konnte nicht abgerufen: Timeout.</td></tr>
        <tr><td>XA1107</td><td>Anwendung konnte nicht gestartet werden</td></tr>
        <tr><td>XA1201</td><td>Den Simulator konnten nicht geladen werden: {0}</td></tr>
        <tr><td>XA1301</td><td>Systemeigene Bibliothek `{0}` (\ {1\}) wurde ignoriert, da sie nicht übereinstimmt, den aktuellen Build Architecture(s) (\ {2\})</td></tr>
    </tbody>
</table>

### <a name="xa2xxx-errors"></a>XA2xxx-Fehler

<table>
    <thead>
        <tr><td>Fehlercode</td><td>Beschreibung</td></tr>
    </thead>
    <tbody>
        <tr><td>XA2001</td><td>Assemblys konnte nicht verknüpft werden.</td></tr>
        <tr><td>XA2002</td><td>Verweis kann nicht aufgelöst werden: {0}</td></tr>
        <tr><td>XA2003</td><td>Option '{0}' wird ignoriert werden, da verknüpfen deaktiviert ist</td></tr>
        <tr><td>XA2004</td><td>Zusätzliche Linker-Definitionsdatei "{0}" konnte nicht gefunden werden.</td></tr>
        <tr><td>XA2005</td><td>Definitionen von "{0}" konnte nicht analysiert werden.</td></tr>
        <tr><td>XA2006</td><td>Verweis auf das Metadatenelement '{0}"(definiert in"\ {1\}") von"\ {2\}"konnte nicht aufgelöst werden.</td></tr>
    </tbody>
</table>

### <a name="xa3xxx-errors"></a>XA3xxx-Fehler

Hierbei handelt es sich um AOT-Fehler.

<table>
    <thead>
        <tr><td>Fehlercode</td><td>Beschreibung</td></tr>
    </thead>
    <tbody>
        <tr><td>XA3001</td><td>Konnte nicht AOT der Assembly "{0}"</td></tr>
        <tr><td>XA3002</td><td>AOT Einschränkung: Methode "{0}" muss statisch sein, da er mit [MonoPInvokeCallback] ergänzt wird.</td></tr>
        <tr><td>XA3003</td><td>Es wurden widersprüchliche--Debug- und--Llvm-Optionen. Soft-Debuggen ist deaktiviert.</td></tr>
    </tbody>
</table>

### <a name="xa4xxx-errors"></a>XA4xxx-Fehler

Hierbei handelt es sich um Code Generierungsfehler.

<table>
    <thead>
        <tr><td>Fehlercode</td><td>Beschreibung</td></tr>
    </thead>
    <tbody>
        <tr><td>XA4001</td><td>Die Haupt-Vorlage konnte nicht auf expansed werden `{0}`.</td></tr>
        <tr><td>XA4101</td><td>Die Registrierungsstelle eine Signatur für den Typ kann nicht erstellt werden `{0}`.</td></tr>
        <tr><td>XA4102</td><td>Die Registrierungsstelle wurde einen ungültigen Typ gefunden `{0}` in Signatur für die Methode `{2}`. Verwenden Sie stattdessen `{1}`.</td></tr>
        <tr><td>XA4103</td><td>Die Registrierungsstelle wurde einen ungültigen Typ gefunden `{0}` in Signatur für die Methode `{2}`: der Typ implementiert INativeObject, jedoch verfügt nicht über einen Konstruktor, der zwei akzeptiert (IntPtr, Bool) Argumente</td></tr>
        <tr><td>XA4104</td><td>Die Registrierungsstelle nicht für den Typ den Rückgabewert Marshallen `{0}` in Signatur für die Methode `{1}`.</td></tr>
        <tr><td>XA4105</td><td>Die Registrierungsstelle nicht marshallen den Parameter vom Typ `{0}` in Signatur für die Methode `{1}`.</td></tr>
        <tr><td>XA4106</td><td>Die Registrierungsstelle nicht für die Struktur den Rückgabewert Marshallen `{0}` in Signatur für die Methode `{1}`.</td></tr>
        <tr><td>XA4107</td><td>Die Registrierungsstelle nicht marshallen den Parameter vom Typ `{0}` in Signatur für die Methode `{1}`.</td></tr>
        <tr><td>XA4108</td><td>Die Registrierungsstelle den Typ der ObjectiveC für verwalteten Typ kann nicht abgerufen werden `{0}`.</td></tr>
        <tr><td>XA4109</td><td>Fehler beim Kompilieren des Codes generierte Registrierungsstelle. Bitte der Datei eines Fehlerberichts an <a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA4110</td><td>Die Registrierungsstelle nicht die Out-Parameter des Typs Marshallen `{0}` in Signatur für die Methode `{1}`.</td></tr>
        <tr><td>XA4111</td><td>Die Registrierungsstelle eine Signatur für den Typ kann nicht erstellt werden `{0}' in method `\ {1\} ".</td></tr>
        <tr><td>XA4200</td><td>INHALTSFEHLERs kann nur für "Claas"-Typen generiert werden.</td></tr>
        <tr><td>XA4201</td><td>Kann nicht bestimmt JNI-Namen für Typ ' {0} '.</td></tr>
        <tr><td>XA4203</td><td>Der angegebenen Typnamen muss vollqualifiziert sein.</td></tr>
        <tr><td>XA4204</td><td>Kann nicht aufgelöst Schnittstellentyp "{0}". Möglicherweise fehlt ein Assemblyverweis.</td></tr>
        <tr><td>XA4205</td><td>[ExportField] kann nur auf Methoden mit Parametern 0 verwendet werden.</td></tr>
        <tr><td>XA4206</td><td>[Exportieren] kann nicht für einen generischen Typ verwendet werden.</td></tr>
        <tr><td>XA4207</td><td>[ExportField] kann nicht für einen generischen Typ verwendet werden.</td></tr>
        <tr><td>XA4208</td><td>[Java.Interop.ExportFieldAttribute] kann nicht auf eine Methode, die "void" zurückgeben verwendet werden.</td></tr>
        <tr><td>XA4209</td><td>Fehler beim Erstellen von JavaTypeInfo für Klasse: {0} aufgrund von \ {1\}</td></tr>
        <tr><td>XA4210</td><td>Sie müssen einen Verweis auf Mono.Android.Export.dll hinzufügen, wenn Sie ExportAttribute oder ExportFieldAttribute verwenden.</td></tr>
        <tr><td>XA4211</td><td>AndroidManifest.xml //uses-sdk/@android:targetSdkVersion '{0}' ist kleiner als $(TargetFrameworkVersion) "\ {1\}". Mithilfe von API-\ {1\} für Inhaltsfehler Kompilierung.</td></tr>
    </tbody>
</table>

### <a name="xa5xxx-errors"></a>XA5xxx-Fehler

<table>
    <thead>
        <tr><td>Fehlercode</td><td>Beschreibung</td></tr>
    </thead>
    <tbody>
        <tr><td>XA5101</td><td>Fehlende '{0}' des Compilers. Installieren Sie Android-NDK.</td></tr>
        <tr><td>XA5102</td><td>Fehler bei der Konvertierung aus der Assembly in systemeigenen Code. Bitte der Datei eines Fehlerberichts an <a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA5103</td><td>Fehler beim Kompilieren der Datei "{0}". Bitte der Datei eines Fehlerberichts an <a href="http://bugzilla.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA5201</td><td>Fehler bei der systemeigenen verknüpfen. Überprüfen Sie die Benutzerflags bereitgestellt, um den Gcc: {0}</td></tr>
        <tr><td>XA5202</td><td>Fehler bei der systemeigenen verknüpfen. Überprüfen Sie das Buildprotokoll an.</td></tr>
        <tr><td>XA5303</td><td>Systemeigene verknüpfen Warnung: {0}</td></tr>
        <tr><td>XA5300</td><td>Android-SDK nicht gefunden oder nicht vollständig installiert.</td></tr>
        <tr><td>XA5301</td><td>Tool für den fehlenden "entfernen". Installieren Sie Xcode "Befehlszeilentools"-Komponente</td></tr>
        <tr><td>XA5302</td><td>Fehlende "Dsymutil"-Tool. Installieren Sie Xcode "Befehlszeilentools"-Komponente</td></tr>
        <tr><td>XA5203</td><td>Fehler beim Generieren von der Debugsymbolen (dSYM Directory). Überprüfen Sie das Buildprotokoll an.</td></tr>
        <tr><td>XA5204</td><td>Fehler bei, um die endgültige Binärdatei zu entfernen. Überprüfen Sie das Buildprotokoll an.</td></tr>
        <tr><td>XA5205</td><td>Fehlende "Aapt"-Tool. Installieren Sie das Paket für Android-SDK-Buildtools.</td></tr>
        <tr><td>XA5206</td><td>{0}. Android Ressource Directory \ {1\} ist nicht vorhanden.</td></tr>
        <tr><td>XA5207</td><td>{0}. Java-Bibliothek Datei \ {1\} ist nicht vorhanden.</td></tr>
        <tr><td>XA5208</td><td>Fehler beim Download. Herunterladen von {0}, und fügen Sie sie in das Verzeichnis \ {1\}.</td></tr>
        <tr><td>XA5209</td><td>Fehler beim Dekomprimieren. Herunterladen von {0}, und extrahieren Sie es in das Verzeichnis \ {1\}.</td></tr>
        <tr><td>XA5210</td><td>{0}. Systemeigene Bibliothek Datei \ {1\} ist nicht vorhanden.</td></tr>
    </tbody>
</table>

### <a name="xa6xxx-errors"></a>XA6xxx-Fehler

<table>
    <thead>
        <tr><td>Fehlercode</td><td>Beschreibung</td></tr>
    </thead>
    <tbody>
        <tr><td>XA6001</td><td>Ausgeführte Version eines Cecil unterstützt keine Assembly entfernen</td></tr>
        <tr><td>XA6002</td><td>Assembly konnte nicht entfernt `{0}`.</td></tr>
        <tr><td>XA6003</td><td>UnauthorizedAccessException Message]</td></tr>
    </tbody>
</table>

### <a name="xa9xxx-errors"></a>XA9xxx-Fehler

Diese Fehlercodes werden Fehler bei der Lizenzierung und Aktivierung.



#### <a name="xa9000"></a>XA9000

 **Ursache:** -Lizenz abgelaufen

 **Während überprüft:** erstellen

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Geschäftlich</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>WARNUNG</td>
            <td>WARNUNG</td>
            <td>WARNUNG</td>
            <td>WARNUNG</td>
            <td>WARNUNG</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9001"></a>XA9001

 **Ursache:** Testversion abgelaufen

 **Während überprüft:** erstellen

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Geschäftlich</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>WARNUNG</td>
            <td>WARNUNG</td>
            <td>WARNUNG</td>
            <td>WARNUNG</td>
            <td>WARNUNG</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9002"></a>XA9002

 **Cause:** AndroidJavaSource

 **Während überprüft:** erstellen

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Geschäftlich</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>FEHLER</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **Cause:** AndroidJavaLibrary

 **Während überprüft:** erstellen

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Geschäftlich</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>FEHLER</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **Verfügt die bindungsassembly eine die JAR eingebettet, wird dies zur des Pakets, nicht die Buildzeit abgefangen.**

 **Cause:** AndroidNativeLibrary

 **Während überprüft:** erstellen

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Geschäftlich</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>FEHLER</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9003"></a>XA9003

 **Cause:** System.Runtime.Serialization

 **Während überprüft:** Paket

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Geschäftlich</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **Cause:** System.ServiceModel.Web

 **Während überprüft:** Paket

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Geschäftlich</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **Ursache:** Mono.Data.Tds

 **Während überprüft:** erstellen

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Geschäftlich</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **Dies wird durch "System.Data.dll", verwiesen, die zulässig ist**

 **Ursache:** Mono.Android.Export

 **Während überprüft:** erstellen

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Geschäftlich</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>FEHLER</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9004"></a>XA9004

 **Ursache:** --profilerstellung

 **Während überprüft:** Paket

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Geschäftlich</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9005"></a>XA9005

 **Ursache:** maximale Größe (32 kb)

 **Während überprüft:** Paket

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Geschäftlich</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>FEHLER</td>
            <td>OK</td>
            <td>-</td>
            <td>-</td>
            <td>-</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9006"></a>XA9006

 **Ursache:** System.Data.SqlClient-Namespace

 **Während überprüft:** Paket

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Geschäftlich</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9008"></a>XA9008

 **Ursache:** erstellen über die Befehlszeile

 **Während überprüft:** erstellen

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Geschäftlich</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9009"></a>XA9009

 **Ursache:** Seriennummer fehlt

 **Während überprüft:** Untestable

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Geschäftlich</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>FEHLER</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9010"></a>XA9010

 **Ursache:** ungültige "ProductID"

 **Während überprüft:** erstellen

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Geschäftlich</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>FEHLER</td>
        </tr>
    </tbody>
</table>

Entspricht der XA9018



#### <a name="xa9011"></a>XA9011

 **Ursache:** Fehler beim Aktualisieren der Lizenzdatei (in das neue Dateiformat)

 **Während überprüft:** Aktivierung

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Geschäftlich</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>FEHLER</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9012"></a>XA9012

 **Ursache:** keine Internet

 **Während überprüft:** Aktivierung

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Geschäftlich</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>FEHLER</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9013"></a>XA9013

 **Ursache:** Unbekannter Fehler

 **Während überprüft:** Aktivierung

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Geschäftlich</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>FEHLER</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9014"></a>XA9014

 **Ursache:** Aktivierungscode ungültig

 **Während überprüft:** Aktivierung

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Geschäftlich</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>FEHLER</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9017"></a>XA9017

 **Ursache:** Aktivierungsserver keine gültige Lizenz zurückgeben

 **Während überprüft:** Aktivierung

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Geschäftlich</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>FEHLER</td>
            <td>FEHLER</td>
        </tr>
    </tbody>
</table>


#### <a name="xa9018"></a>XA9018

**Ursache:** ungültige Lizenz

**Während überprüft:** erstellen

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Geschäftlich</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>-</td>
            <td>-</td>
            <td>FEHLER</td>
            <td>-</td>
            <td>-</td>
        </tr>
    </tbody>
</table>
