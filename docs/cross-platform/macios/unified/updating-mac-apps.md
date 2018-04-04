---
title: Aktualisieren vorhandener Mac-Apps
description: Führen Sie diese Schritte, um eine vorhandene Xamarin.Mac-app zum Verwenden der API Unified zu aktualisieren.
ms.prod: xamarin
ms.assetid: 26673CC5-C1E5-4BAC-BEF4-9A386B296FD5
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: bdd3b01212d8c1c9949d6aa183c38c83d0afe831
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="updating-existing-mac-apps"></a>Aktualisieren vorhandener Mac-Apps

_Führen Sie diese Schritte, um eine vorhandene Xamarin.Mac-app zum Verwenden der API Unified zu aktualisieren._

Aktualisieren einer vorhandenen app zum Verwenden der API Unified erfordert Änderungen an der Projektdatei selbst sowie Änderungen an den Namespaces und APIs, die in den Code der Anwendung verwendet.

## <a name="the-road-to-64-bits"></a>Die Straße auf 64 Bit

Die neue Unified-APIs sind erforderlich, um die 64-Bit-Gerät-Architekturen aus einer Anwendung Xamarin.Mac unterstützen. Ab dem 1. Februar 2015 erfordert Apple an, dass alle neuen app-Übermittlungen an Mac App Store auf 64-Bit-Architekturen unterstützen.

Xamarin bietet Tools für Visual Studio für Mac und Visual Studio zum Automatisieren des Migrationsvorgangs aus der klassischen-API die einheitliche API, oder können Sie die Projektdateien manuell konvertieren. Während die mithilfe der automatischen Tools hoch vorgeschlagen wird, werden beide Methoden abgedeckt.

### <a name="before-you-start"></a>Sie vor dem Start...

Bevor Sie den vorhandenen Code in die einheitliche API aktualisieren, es wird dringend empfohlen, dass Sie alle beseitigen *compilerwarnungen*. Viele *Warnungen* in der klassischen-API werden Fehler nach der Migration zu Unified. Behebens, bevor Sie beginnen ist einfacher, da der Compiler Nachrichten aus der klassischen-API häufig Hinweise zu aktualisieren bereitgestellt werden.

## <a name="automated-updating"></a>Automatische Aktualisierung

Sobald die Warnungen behoben haben, wählen Sie ein vorhandenes Mac-Projekt in Visual Studio für Mac oder Visual Studio, und wählen Sie **Migrate to Xamarin.Mac einheitliche API** aus der **Projekt** Menü. Zum Beispiel:

![](updating-mac-apps-images/beta-tool1.png "Xamarin.Mac einheitliche API über das Menü Projekt migrieren möchten")

Akzeptieren Sie diese Warnung an, damit die automatisierte Migration ausgeführt werden müssen (Natürlich sollten Sie sicherstellen müssen Sicherungen/quellcodeverwaltung vor diesem Adventure):

![](updating-mac-apps-images/migrate01.png "Diese Warnung stimmen Sie müssen, bevor die automatisierte Migration ausgeführt werden zu")

Es gibt zwei unterstützte Zielframework-Typen, die ausgewählt werden können, wenn der einheitliche API in einer Anwendung Xamarin.Mac verwenden:

- **Xamarin.Mac Mobile Framework** -Dies ist dasselbe optimierten .NET Framework von Xamarin.iOS und Xamarin.Android unterstützen eine Teilmenge der vollständigen verwendeten **Desktop** Framework. Dies ist die empfohlene Framework, weil es kleineren durchschnittliche Binärdateien aufgrund überlegene verknüpfen Verhalten bereitstellt.
- **Xamarin.Mac .NET 4.5 Framework** -dieses Framework wird erneut, eine Teilmenge der **Desktop** Framework. Allerdings um anzupassen, deaktiviert sehr viel geringer ist der vollständigen **Desktop** Framework-Version der **Mobile** Framework und sollten _"nur Geschäfts-"_ mit den meisten NuGet-Pakete oder 3rd Party-Bibliotheken. Dies ermöglicht es dem Entwickler Standard nutzen **Desktop** Assemblys während der größeren Anwendung Pakete weiterhin mit einer unterstützten Frameworks, jedoch diese Option erzeugt werden. Dies ist die empfohlene Framework, in denen 3rd Party .NET Assemblys verwendet werden, die nicht mit kompatibel sind die **Xamarin.Mac Mobile Framework**. Eine Liste der unterstützten Assemblys, finden Sie in unserem [Assemblys](~/cross-platform/internals/available-assemblies.md) Dokumentation.

Ausführliche Informationen zum Ziel-Frameworks und die Auswirkungen der Auswahl eines bestimmten Ziels für Ihre Anwendung Xamarin.Mac finden Sie unsere [Zielframeworks](~/mac/platform/target-framework.md) Dokumentation. 

Das Tool automatisiert im Grunde genommen alle Schritte in der **Update manuell** Abschnitt unten, und ist die vorgeschlagene Methode zum Konvertieren eines vorhandenen Xamarin.Mac-Projekts in das einheitliche API.

## <a name="steps-to-update-manually"></a>So aktualisieren Sie manuell

Erneut, nachdem die Warnungen behoben wurden, gehen Sie folgendermaßen vor Xamarin.Mac-apps zur Verwendung von der neuen API zum Unified manuell zu aktualisieren:

### <a name="1-update-project-type--build-target"></a>1. Update-Projekttyp & Build-Ziel

Ändern Sie die Projektkonfiguration in Ihre **Csproj** Dateien von `42C0BBD9-55CE-4FC1-8D90-A7348ABAFB23` auf `A3F8F2AB-B479-4A4A-A458-A89E7DC349F1`. Bearbeiten der **Csproj** -Datei in einem Text-Editor, und Ersetzen Sie dabei das erste Element in der `<ProjectTypeGuids>` Element wie gezeigt:

![](updating-mac-apps-images/csproj.png "Bearbeiten Sie die Csproj-Datei in einem Text-Editor, und Ersetzen Sie dabei das erste Element in das ProjectTypeGuids-Element entsprechend")

Ändern der **Import** Element, das enthält `Xamarin.Mac.targets` auf `Xamarin.Mac.CSharp.targets` wie gezeigt:

![](updating-mac-apps-images/csproj2.png "Ändern Sie das Import-Element, das um Xamarin.Mac.CSharp.targets Xamarin.Mac.targets enthält, wie gezeigt")

Fügen Sie die folgenden Zeilen des Codes nach der `<AssemblyName>` Element:

```xml
<TargetFrameworkVersion>v2.0</TargetFrameworkVersion>
<TargetFrameworkIdentifier>Xamarin.Mac</TargetFrameworkIdentifier>

```

Beispiel:

![](updating-mac-apps-images/csproj3.png "Fügen Sie diese Codezeilen hinter dem Element < AssemblyName > hinzu.")

### <a name="2-update-project-references"></a>2. Aktualisieren von Projektverweisen

Erweitern Sie die Mac-Anwendungsprojekt **Verweise** Knoten. Es wird zunächst angezeigt eine * unterbrochen - **XamMac** Verweis ähnlich wie in diesem Screenshot (da es den Projekttyp soeben geändert haben):

![](updating-mac-apps-images/references.png "Es wird zunächst einen Verweis unterteilt XamMac ähnlich wie in diesem Screenshot angezeigt")

Klicken Sie auf die **Zahnradsymbol** neben der **XamMac** Eintrag, und wählen **löschen** So entfernen Sie den fehlerhaften Verweis.

Als Nächstes mit der rechten Maustaste auf die **Verweise** Ordner in der **Projektmappen-Explorer** , und wählen Sie **Verweise bearbeiten**. Führen Sie einen Bildlauf zum Ende der Liste der Verweise, und aktivieren Sie das Kontrollkästchen neben dem **Xamarin.Mac**.

![](updating-mac-apps-images/references2.png "Führen Sie einen Bildlauf zum Ende der Liste der Verweise, und aktivieren Sie das Kontrollkästchen neben dem Xamarin.Mac")

Drücken Sie **OK** um das Projekt Verweise Änderungen zu speichern.

### <a name="3-remove-monomac-from-namespaces"></a>3. Entfernen von MonoMac von Namespaces

Entfernen Sie die **MonoMac** Präfix von Namespaces in `using` Anweisungen oder ablegen, wo ein Klassenname (z. b. vollqualifizierte wurde hat `MonoMac.AppKit` wird nur `AppKit`).

### <a name="4-remap-types"></a>4. Neuzuordnen von Typen

[Systemeigene Typen](~/cross-platform/macios/nativetypes.md) wurden eingeführt, die einige Typen ersetzen, die zuvor, z. B. Instanzen der verwendet wurden `System.Drawing.RectangleF` mit `CoreGraphics.CGRect` (zum Beispiel). Die vollständige Liste der Typen finden Sie auf der [systemeigene Typen](~/cross-platform/macios/nativetypes.md) Seite.

### <a name="5-fix-method-overrides"></a>5. Beheben von Methodenüberschreibungen

Einige `AppKit` Methoden weisen jedoch die Signatur geändert, um die neuen [systemeigene Typen](~/cross-platform/macios/nativetypes.md) (z. B. `nint`). Wenn Sie benutzerdefinierte Unterklassen dieser Methoden überschreiben, die Signaturen nicht mehr entsprechen, und führen zu Fehlern. Beheben Sie diese Methode überschreibt, indem die Unterklasse entsprechend die neue Signatur, die mit systemeigenen Typen ändern. 

## <a name="considerations"></a>Weitere Überlegungen

Folgendes sollte beim Konvertieren eines vorhandenen Xamarin.Mac-Projekts von der klassische API an die neue einheitliche API, wenn die app-Komponente oder NuGet-Paket verwendet berücksichtigt werden. 

### <a name="components"></a>Komponenten

Jede Komponente, die Sie in Ihrer Anwendung aufgenommen haben, müssen auch die einheitliche API aktualisiert werden, oder Sie erhalten einen Konflikt auf, wenn Sie versuchen, zu kompilieren. Für eine beliebige Komponente enthalten eine neue Version aus dem Speicher der Xamarin-Komponente, die die einheitliche API unterstützt die aktuelle Version ersetzt, und führen Sie einen bereinigten Build aus. Jede Komponente, die noch nicht vom Autor konvertiert wurde, wird eine Warnung nur in der Komponentenspeicher 32-Bit angezeigt.

### <a name="nuget-support"></a>NuGet-Unterstützung

Während es Änderungen zu NuGet zur Bearbeitung der einheitliche API-Unterstützung beigetragen hat, hat nicht stattgefunden eine neue Version von NuGet, damit wir das NuGet erkennt die neuen APIs zu evaluieren. 

Bis zu diesem Zeitpunkt wird genau wie die Komponenten müssen Sie wechseln alle NuGet-Paket, das Sie in Ihrem Projekt auf eine Version aufgenommen haben, die Unified-APIs unterstützt, und führen Sie anschließend einen bereinigten Build.

> [!IMPORTANT]
> Haben einen Fehler in der Form _"Fehler 3 kann nicht im selben Projekt Xamarin.Mac"monomac.dll"und"Xamarin.Mac.dll"enthalten – explizit"Xamarin.Mac.dll"verwiesen wird, während"monomac.dll"verwiesen wird" Xxx, Version = 0.0.000, Culture = Neutral, PublicKeyToken = Null'"_ nach dem Konvertieren der anwendungskennworts an die Unified-APIs, es liegt in der Regel müssen eine Komponente oder die NuGet-Paket in das Projekt, das nicht auf die einheitliche API aktualisiert wurde. Sie müssen die vorhandene Komponente/NuGet entfernen, update auf eine Version, die Unified-APIs unterstützt, und führen Sie einen bereinigten Build.

## <a name="enabling-64-bit-builds-of-xamarinmac-apps"></a>Aktivieren die 64-Bit-Builds von Xamarin.Mac-Apps

Für einen mobilen Xamarin.Mac-Anwendung, die in die einheitliche API konvertiert wurde, muss der Entwickler noch die Erstellung der Anwendung für 64-Bit-Computer aus der app-Optionen zu aktivieren. Finden Sie unter der **aktivieren 64 Bit-erstellt von Xamarin.Mac Apps** von der [32/64-Bit-Plattform Überlegungen](~/cross-platform/macios/32-and-64/index.md) Dokument Weitere ausführliche Anweisungen zur Aktivierung von 64-Bit erstellt.
    
## <a name="finishing-up"></a>Fertig stellen

Unabhängig davon, ob Sie die automatische oder manuelle Methode verwenden, um Ihre Anwendung Xamarin.Mac in der klassischen an die Unified-APIs zu konvertieren möchten, stehen mehrere Instanzen, die darüber hinaus manuelle Eingriffe erfordern. Finden Sie in unserer [Tipps zum Aktualisieren von Code an die API Unified](~/cross-platform/macios/unified/updating-tips.md) dokumentieren Sie Informationen zu bekannten Problemen und problemumgehungen.

## <a name="related-links"></a>Verwandte Links

- [Tipps zum Aktualisieren von Code für Unified API](~/cross-platform/macios/unified/updating-tips.md)
- [Arbeiten mit nativen Typen in plattformübergreifenden Apps](~/cross-platform/macios/native-types-cross-platform.md)
- [Klassische Vs einheitliche API-Unterschiede](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
