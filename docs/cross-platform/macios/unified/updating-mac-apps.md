---
title: Aktualisieren von vorhandenen Mac-Apps
description: Dieses Dokument beschreibt die Schritte, die zum Aktualisieren einer Xamarin.Mac-app von der klassischen API, die Unified API befolgt werden müssen.
ms.prod: xamarin
ms.assetid: 26673CC5-C1E5-4BAC-BEF4-9A386B296FD5
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 5e6034b079bba5e884872e4f2096d677fd3641d0
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61213525"
---
# <a name="updating-existing-mac-apps"></a>Aktualisieren von vorhandenen Mac-Apps

Aktualisiert eine vorhandene app zum Verwenden der Unified API erfordert Änderungen an der Projektdatei selbst sowie über die Namespaces und APIs, die im Anwendungscode verwendet.

## <a name="the-road-to-64-bits"></a>Der Weg zur 64-Bit

Das neue Unified-APIs sind zur Unterstützung von 64-Bit-Gerätearchitektur aus einer Xamarin.Mac-Anwendung erforderlich. Ab dem 1. Februar 2015 erfordert Apple an, dass alle neuen Einreichen von Apps für den Mac App Store 64-Bit-Architekturen zu unterstützen.

Xamarin bietet Tools für Visual Studio für Mac und Visual Studio während der Migration von der klassischen API, die Unified API zu automatisieren, oder Sie können die Projektdateien manuell konvertieren. Während die mit der automatischen Tools hoch empfohlen wird, werden beide Methoden abgedeckt.

### <a name="before-you-start"></a>Bevor Sie beginnen...

Bevor Sie den vorhandenen Code der Unified API aktualisieren, es wird dringend empfohlen, dass Sie alle beseitigen *compilerwarnungen*. Viele *Warnungen* in der klassischen API werden Fehler nach der Migration zu Unified. Beheben diese, bevor Sie beginnen ist einfacher, da compilermeldungen von der klassischen API häufig Hinweise dazu, was Sie aktualisieren bereitstellen.

## <a name="automated-updating"></a>Automatisierte Updates

Sobald die Warnungen behoben wurden, wählen Sie ein vorhandenes Projekt für den Mac in Visual Studio für Mac oder Visual Studio, und wählen Sie **Migration zur vereinheitlichten Xamarin.Mac-API** aus der **Projekt** Menü. Zum Beispiel:

![](updating-mac-apps-images/beta-tool1.png "Vereinheitlichten Xamarin.Mac-API migrieren möchten, aus dem Menü \"Projekt\"")

Sie müssen zustimmen, um diese Warnung an, bevor die automatisierte Migration ausgeführt werden kann (Natürlich sollten Sie sicherstellen Sie Sicherungen/Datenquellen-Steuerelement vor diesem Adventure haben):

![](updating-mac-apps-images/migrate01.png "Stimmen Sie um diese Warnung zu, bevor die automatisierte Migration ausgeführt werden kann")

Es gibt zwei unterstützte Zielframework-Typen, die bei der Verwendung der Unified API in einer Xamarin.Mac-Anwendung ausgewählt werden können:

- **Xamarin.Mac Mobile Framework** – Dies ist der derselben optimierten .NET Framework, die von Xamarin.iOS und Xamarin.Android unterstützt eine Teilmenge des vollständigen verwendet **Desktop** Framework. Dies ist das empfohlene Framework, da es aufgrund von überlegene Linkverhalten Durchschnitt kleinere Binärdateien bereitstellt.
- **Xamarin.Mac .NET 4.5 Framework** -dieses Framework ist in diesem Fall eine Teilmenge der **Desktop** Framework. Allerdings es kürzt aus viel weniger des vollständigen **Desktop** Framework-Version der **Mobile** Framework und sollten _"einfach so funktionieren"_ mit den meisten NuGet-Pakete oder 3. Bibliotheken von Drittanbietern. Dadurch kann der Entwickler Standard nutzen **Desktop** Assemblys, während Sie weiterhin mithilfe eines unterstützten Frameworks, aber diese Option größere Anwendungspakete erzeugt. Dies ist das empfohlene Framework, in denen 3rd Party .NET Assemblys verwendet werden, die nicht kompatibel mit der **Xamarin.Mac Mobile Framework**. Eine Liste der unterstützten Assemblys, informieren Sie sich unsere [Assemblys](~/cross-platform/internals/available-assemblies.md) Dokumentation.

Ausführliche Informationen zu Zielframeworks und die Auswirkungen auf die ein bestimmtes Ziel für die Xamarin.Mac-Anwendung auswählen, finden Sie unserem [Zielframeworks](~/mac/platform/target-framework.md) Dokumentation. 

Das Tool im Grunde automatisiert alle Schritte der **Update manuell** Abschnitt unten und ist die vorgeschlagene Methode konvertieren Sie ein vorhandenes Xamarin.Mac-Projekt in der Unified API.

## <a name="steps-to-update-manually"></a>So aktualisieren Sie manuell

In diesem Fall, sobald die Warnungen behoben wurden, gehen Sie zum manuellen Aktualisieren von Xamarin.Mac-apps, um die neue Unified API verwenden:

### <a name="1-update-project-type--build-target"></a>1. Update-Projekttyp & Build-Ziel

Ändern Sie die Projektkonfiguration in Ihre **Csproj** Dateien von `42C0BBD9-55CE-4FC1-8D90-A7348ABAFB23` zu `A3F8F2AB-B479-4A4A-A458-A89E7DC349F1`. Bearbeiten der **Csproj** -Datei in einem Text-Editor, und Ersetzen Sie dabei das erste Element in der `<ProjectTypeGuids>` -Element wie gezeigt:

![](updating-mac-apps-images/csproj.png "Bearbeiten Sie die Csproj-Datei in einem Text-Editor, und Ersetzen Sie dabei das erste Element im ProjectTypeGuids-Element wie gezeigt")

Ändern der **Import** -Element mit `Xamarin.Mac.targets` zu `Xamarin.Mac.CSharp.targets` wie gezeigt:

![](updating-mac-apps-images/csproj2.png "Ändern Sie das Import-Element, das Xamarin.Mac.targets zu Xamarin.Mac.CSharp.targets enthält (siehe)")

Fügen Sie die folgenden Zeilen des Codes nach der `<AssemblyName>` Element:

```xml
<TargetFrameworkVersion>v2.0</TargetFrameworkVersion>
<TargetFrameworkIdentifier>Xamarin.Mac</TargetFrameworkIdentifier>

```

Beispiel:

![](updating-mac-apps-images/csproj3.png "Fügen Sie diese Codezeilen hinter dem Element < AssemblyName > hinzu.")

### <a name="2-update-project-references"></a>2. Aktualisieren von Projektverweisen

Erweitern Sie die Mac-Anwendungsprojekt **Verweise** Knoten. Anfänglich angezeigt ein * unterbrochen - **XamMac** Verweis ist (da wir gerade den Projekttyp geändert) etwa so:

![](updating-mac-apps-images/references.png "Zunächst einen Verweis unterteilt XamMac ähnlich wie in diesem Screenshot angezeigt")

Klicken Sie auf die **Zahnradsymbol** neben der **XamMac** Eintrag, und wählen **löschen** So entfernen Sie den fehlerhaften Verweis.

Als Nächstes mit der rechten Maustaste auf die **Verweise** Ordner in der **Projektmappen-Explorer** , und wählen Sie **Verweise bearbeiten**. Führen Sie einen Bildlauf zum unteren Rand der Liste der Verweise, und aktivieren Sie das Kontrollkästchen neben dem **Xamarin.Mac**.

![](updating-mac-apps-images/references2.png "Führen Sie einen Bildlauf zum unteren Rand der Liste der Verweise, und aktivieren Sie das Kontrollkästchen neben dem Xamarin.Mac")

Drücken Sie **OK** um das Projekt Verweise Änderungen zu speichern.

### <a name="3-remove-monomac-from-namespaces"></a>3. Entfernen von MonoMac von Namespaces

Entfernen Sie die **MonoMac** Präfix von Namespaces in `using` Anweisungen oder ganz egal, wo ein Klassenname (z. b. vollqualifizierte wurde hat `MonoMac.AppKit` wird lediglich `AppKit`).

### <a name="4-remap-types"></a>4. Neuzuordnen von Typen

[Systemeigene Typen](~/cross-platform/macios/nativetypes.md) wurden eingeführt, die einige Typen ersetzen, die zuvor, z. B. Instanzen der verwendet wurden `System.Drawing.RectangleF` mit `CoreGraphics.CGRect` (z. B.). Die vollständige Liste der Typen finden Sie auf die [systemeigene Typen](~/cross-platform/macios/nativetypes.md) Seite.

### <a name="5-fix-method-overrides"></a>5. Beheben Sie die Methodenüberschreibungen

Einige `AppKit` Methoden hatten die Signatur geändert, um die Verwendung der neuen [systemeigene Typen](~/cross-platform/macios/nativetypes.md) (z. B. `nint`). Wenn Sie benutzerdefinierte Unterklassen diese Methoden überschreiben, die Signaturen nicht mehr überein, und führt zu Fehlern. Beheben Sie diese Methode überschreibt, indem die Unterklasse die neue Signatur, die mit systemeigenen Typen entsprechend ändern. 

## <a name="considerations"></a>Weitere Überlegungen

Die folgenden Aspekte sollten bei der Konvertierung eines vorhandenen Xamarin.Mac-Projekts aus der klassischen API auf die neue einheitliche API, wenn diese app mindestens eine Komponente oder NuGet-Paket benötigt berücksichtigt werden. 

### <a name="components"></a>Komponenten

Jede Komponente, die Sie in Ihrer Anwendung aufgenommen haben, müssen auch die Unified API aktualisiert werden, oder Sie erhalten einen Konflikt auf, wenn Sie versuchen, zu kompilieren. Ersetzen Sie die aktuelle Version mit einer neuen Version von der Xamarin Component Store, die die Unified API unterstützt, und führen Sie einen bereinigten Build, für eine beliebige Komponente enthalten. Jede Komponente, die noch nicht vom Autor, konvertiert wurde, wird eine 32-Bit nur im Komponentenspeicher Warnung angezeigt.

### <a name="nuget-support"></a>NuGet-Unterstützung

Während wir Änderungen an NuGet mit der Unified API-Unterstützung funktioniert hat, wurde keine neue Version von NuGet, damit wir zum Abrufen von NuGet, um die neuen APIs erkennt auswerten. 

Bis zu diesem Zeitpunkt an, wie die Komponenten müssen Sie zum Wechseln von alle NuGet-Paket, das Sie in Ihrem Projekt auf eine Version aufgenommen haben, die Unified-APIs unterstützt, und führen anschließend einen bereinigten Build.

> [!IMPORTANT]
> Wenn Sie einen Fehler in der Form haben _"Fehler 3 kann nicht sowohl"monomac.dll"und"Xamarin.Mac.dll"im gleichen Xamarin.Mac-Projekt enthalten – explizit"Xamarin.Mac.dll"verwiesen wird, solange 'monomac.dll' verwiesen wird" Xxx "," Version 0.0.000, Culture = = Neutral, PublicKeyToken = Null'"_ nach der Konvertierung Ihrer Anwendung auf die Unified-APIs, es liegt in der Regel müssen entweder eine Komponente oder ein NuGet-Paket im Projekt, das nicht der Unified API aktualisiert wurde. Sie müssen zum Entfernen des vorhandenen Komponente/NuGet-Pakets, ein update auf eine Version, die Unified-APIs unterstützt, und führen Sie einen bereinigten Build.

## <a name="enabling-64-bit-builds-of-xamarinmac-apps"></a>Aktivieren die 64-Bit-Builds von Xamarin.Mac-Apps

Für eine mobile Xamarin.Mac-Anwendung, die in der Unified API konvertiert wurde, muss der Entwickler weiterhin das Erstellen der Anwendung für 64-Bit-Computer aus der app-Optionen zu aktivieren. Finden Sie unter den **Aktivieren von 64 Bit Builds von Xamarin.Mac-Apps** von der [32/64 Bit Platform Considerations](~/cross-platform/macios/32-and-64/index.md) Dokument für detaillierte Anweisungen zum Aktivieren von 64-Bit-builds.
    
## <a name="finishing-up"></a>Wird abgeschlossen

Unabhängig davon, ob Sie die automatische oder manuelle Methode verwenden, um Ihrer Xamarin.Mac-Anwendung vom klassischen Bereitstellungsmodell auf die Unified-APIs zu konvertieren möchten, stehen mehrere Instanzen, die darüber hinaus manuelle Eingriffe erfordern. Informieren Sie sich unsere [Tipps zum Aktualisieren von Code, der Unified API](~/cross-platform/macios/unified/updating-tips.md) Dokument für bekannte Probleme und problemumgehungen.

## <a name="related-links"></a>Verwandte Links

- [Tipps zum Aktualisieren von Code für Unified API](~/cross-platform/macios/unified/updating-tips.md)
- [Arbeiten mit nativen Typen in plattformübergreifenden Apps](~/cross-platform/macios/native-types-cross-platform.md)
- [Klassischen Vs Unified API-Unterschiede](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
