---
title: Aktualisieren vorhandene iOS-Apps
description: "Führen Sie diese Schritte, um eine vorhandene Xamarin.iOS-app zum Verwenden der API Unified zu aktualisieren."
ms.topic: article
ms.prod: xamarin
ms.assetid: 303C36A8-CBF4-48C0-9412-387E95024CAB
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 3114b2abdc96bc7d122ca0b86e1c53977f6df1be
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="updating-existing-ios-apps"></a>Aktualisieren vorhandene iOS-Apps

_Führen Sie diese Schritte, um eine vorhandene Xamarin.iOS-app zum Verwenden der API Unified zu aktualisieren._

Aktualisieren einer vorhandenen app zum Verwenden der API Unified erfordert Änderungen an der Projektdatei selbst sowie Änderungen an den Namespaces und APIs, die in den Code der Anwendung verwendet.

## <a name="the-road-to-64-bits"></a>Die Straße auf 64 Bit

Die neuen APIs Unified sind erforderlich, um die 64-Bit-Gerät-Architekturen aus einem Xamarin.iOS-mobile-Anwendung unterstützen. Ab dem 1. Februar 2015 erfordert Apple an, dass alle neuen app-Übermittlungen im iTunes App Store auf 64-Bit-Architekturen unterstützen.

Xamarin bietet Tools für Visual Studio für Mac und Visual Studio zum Automatisieren des Migrationsvorgangs aus der klassischen-API die einheitliche API, oder können Sie die Projektdateien manuell konvertieren. Während die mithilfe der automatischen Tools hoch vorgeschlagen wird, werden beide Methoden abgedeckt.

### <a name="before-you-start"></a>Sie vor dem Start...

Bevor Sie den vorhandenen Code in die einheitliche API aktualisieren, es wird dringend empfohlen, dass Sie alle beseitigen *compilerwarnungen*. Viele *Warnungen* in der klassischen-API werden Fehler nach der Migration zu Unified. Behebens, bevor Sie beginnen ist einfacher, da der Compiler Nachrichten aus der klassischen-API häufig Hinweise zu aktualisieren bereitgestellt werden.

## <a name="automated-updating"></a>Automatische Aktualisierung

Sobald die Warnungen behoben haben, wählen Sie ein vorhandenes iOS-Projekt in Visual Studio für Mac oder Visual Studio, und wählen Sie **Migrate to Xamarin.iOS einheitliche API** aus der **Projekt** Menü. Zum Beispiel:

![](updating-ios-apps-images/beta-tool1.png "Xamarin.iOS einheitliche API über das Menü Projekt migrieren möchten")

Akzeptieren Sie diese Warnung an, damit die automatisierte Migration ausgeführt werden müssen (Natürlich sollten Sie sicherstellen müssen Sicherungen/quellcodeverwaltung vor diesem Adventure):

![](updating-ios-apps-images/beta-tool2.png "Diese Warnung stimmen Sie müssen, bevor die automatisierte Migration ausgeführt werden zu")

Das Tool automatisiert im Grunde genommen alle Schritte in der **Update manuell** Abschnitt unten, und ist die vorgeschlagene Methode zum Konvertieren eines vorhandenen Xamarin.iOS-Projekts in das einheitliche API.

## <a name="steps-to-update-manually"></a>So aktualisieren Sie manuell

Erneut, nachdem die Warnungen behoben wurden, gehen Sie folgendermaßen vor Xamarin.iOS-apps zur Verwendung von der neuen API zum Unified manuell zu aktualisieren:

### <a name="1-update-project-type--build-target"></a>1. Update-Projekttyp & Build-Ziel

Ändern Sie die Projektkonfiguration in Ihre **Csproj** Dateien von `6BC8ED88-2882-458C-8E55-DFD12B67127B` auf `FEACFBD2-3405-455C-9665-78FE426C6842`. Bearbeiten der **Csproj** -Datei in einem Text-Editor, und Ersetzen Sie dabei das erste Element in der `<ProjectTypeGuids>` Element wie gezeigt:

![](updating-ios-apps-images/csproj.png "Bearbeiten Sie die Csproj-Datei in einem Text-Editor, und Ersetzen Sie dabei das erste Element in das ProjectTypeGuids-Element entsprechend")

Ändern der **Import** Element, das enthält `Xamarin.MonoTouch.CSharp.targets` auf `Xamarin.iOS.CSharp.targets` wie gezeigt:

![](updating-ios-apps-images/csproj2.png "Ändern Sie das Import-Element, das um Xamarin.iOS.CSharp.targets Xamarin.MonoTouch.CSharp.targets enthält, wie gezeigt")

### <a name="2-update-project-references"></a>2. Aktualisieren von Projektverweisen

Erweitern Sie des iOS-Anwendungsprojekt **Verweise** Knoten. Es wird zunächst angezeigt eine * unterbrochen - **Monotouch** Verweis ähnlich wie in diesem Screenshot (da es den Projekttyp soeben geändert haben):

![](updating-ios-apps-images/references.png "Es wird zunächst einen unterbrochene Monotouch Verweis ähnlich wie in diesem Screenshot angezeigt werden, weil der Projekttyp geändert")

Mit der rechten Maustaste auf das iOS-Anwendungsprojekt auf **Verweise bearbeiten**, klicken Sie dann auf die **Monotouch** verweisen, und löschen Sie ihn mithilfe der Schaltfläche "rotes"X"".

![](updating-ios-apps-images/references-delete-monotouch-sml.png "Mit der rechten Maustaste auf das iOS-Anwendungsprojekt auf Verweise bearbeiten und dann klicken Sie auf den Verweis Monotouch und löschen Sie ihn mit dem roten X-Schaltfläche")

Führen Sie einen Bildlauf bis zum Ende der Verweisliste und Teilstriche der **Xamarin.iOS** Assembly.

![](updating-ios-apps-images/references-add-xamarinios-sml.png "Führen Sie einen Bildlauf bis zum Ende der Verweisliste und Teilstriche Xamarin.iOS assembly")

Drücken Sie **OK** um das Projekt Verweise Änderungen zu speichern.

### <a name="3-remove-monotouch-from-namespaces"></a>3. Entfernen von MonoTouch von Namespaces

Entfernen Sie die **MonoTouch** Präfix von Namespaces in `using` Anweisungen oder ablegen, wo ein Klassenname (z. b. vollqualifizierte wurde hat `MonoTouch.UIKit` wird nur `UIKit`).

### <a name="4-remap-types"></a>4. Neuzuordnen von Typen

[Systemeigene Typen](~/cross-platform/macios/nativetypes.md) wurden eingeführt, die einige Typen ersetzen, die zuvor, z. B. Instanzen der verwendet wurden `System.Drawing.RectangleF` mit `CoreGraphics.CGRect` (zum Beispiel). Die vollständige Liste der Typen finden Sie auf der [systemeigene Typen](~/cross-platform/macios/nativetypes.md) Seite.

### <a name="5-fix-method-overrides"></a>5. Beheben von Methodenüberschreibungen

Einige `UIKit` Methoden weisen jedoch die Signatur geändert, um die neuen [systemeigene Typen](~/cross-platform/macios/nativetypes.md) (z. B. `nint`). Wenn Sie benutzerdefinierte Unterklassen dieser Methoden überschreiben, die Signaturen nicht mehr entsprechen, und führen zu Fehlern. Beheben Sie diese Methode überschreibt, indem die Unterklasse entsprechend die neue Signatur, die mit systemeigenen Typen ändern.

Beispiele umfassen das Ändern `public override int NumberOfSections (UITableView tableView)` zurückzugebenden `nint` und Ändern von Typ und die Parametertypen in zurückgeben `public override int RowsInSection (UITableView tableView, int section)` auf `nint`.

## <a name="considerations"></a>Weitere Überlegungen

Folgendes sollte beim Konvertieren eines vorhandenen Xamarin.iOS-Projekts von der klassische API an die neue einheitliche API, wenn die app-Komponente oder NuGet-Paket verwendet berücksichtigt werden.

### <a name="components"></a>Komponenten

Jede Komponente, die Sie in Ihrer Anwendung aufgenommen haben, müssen auch die einheitliche API aktualisiert werden, oder Sie erhalten einen Konflikt auf, wenn Sie versuchen, zu kompilieren. Für eine beliebige Komponente enthalten eine neue Version aus dem Speicher der Xamarin-Komponente, die die einheitliche API unterstützt die aktuelle Version ersetzt, und führen Sie einen bereinigten Build aus. Jede Komponente, die noch nicht vom Autor konvertiert wurde, wird eine Warnung nur in der Komponentenspeicher 32-Bit angezeigt.

### <a name="nuget-support"></a>NuGet-Unterstützung

Während es Änderungen zu NuGet zur Bearbeitung der einheitliche API-Unterstützung beigetragen hat, hat nicht stattgefunden eine neue Version von NuGet, damit wir das NuGet erkennt die neuen APIs zu evaluieren.

Bis zu diesem Zeitpunkt wird genau wie die Komponenten müssen Sie wechseln alle NuGet-Paket, das Sie in Ihrem Projekt auf eine Version aufgenommen haben, die Unified-APIs unterstützt, und führen Sie anschließend einen bereinigten Build.

> [!IMPORTANT]
> **Hinweis:** haben einen Fehler in der Form _"Fehler 3 kann nicht im selben Projekt Xamarin.iOS"monotouch.dll"und"Xamarin.iOS.dll"enthalten – explizit"Xamarin.iOS.dll"verwiesen wird, während"monotouch.dll"verwiesen wird" Xxx Version = 0.0.000, Culture = Neutral, PublicKeyToken = Null'"_ nach dem Konvertieren der anwendungskennworts an die Unified-APIs, es liegt in der Regel müssen eine Komponente oder die NuGet-Paket in das Projekt, das nicht auf die einheitliche API aktualisiert wurde. Sie müssen die vorhandene Komponente/NuGet entfernen, update auf eine Version, die Unified-APIs unterstützt, und führen Sie einen bereinigten Build.

## <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Aktivieren die 64-Bit-Builds von Xamarin.iOS-Apps

Für eine Xamarin.iOS mobile Anwendung, die in die einheitliche API konvertiert wurde, muss der Entwickler noch die Erstellung der Anwendung für 64-Bit-Computer aus der app-Optionen zu aktivieren. Finden Sie unter der **aktivieren 64 Bit-erstellt von Xamarin.iOS Apps** von der [32/64-Bit-Plattform Überlegungen](~/cross-platform/macios/32-and-64/index.md#enable-64) Dokument Weitere ausführliche Anweisungen zur Aktivierung von 64-Bit erstellt.

## <a name="finishing-up"></a>Fertig stellen

Unabhängig davon, ob Sie die automatische oder manuelle Methode verwenden, um Ihre Anwendung Xamarin.iOS in der klassischen an die Unified-APIs zu konvertieren möchten, stehen mehrere Instanzen, die darüber hinaus manuelle Eingriffe erfordern. Finden Sie in unserer [Tipps zum Aktualisieren von Code an die API Unified](~/cross-platform/macios/unified/updating-tips.md) dokumentieren Sie Informationen zu bekannten Problemen und problemumgehungen.

## <a name="related-links"></a>Verwandte Links

- [Tipps zum Aktualisieren von Code für Unified API](~/cross-platform/macios/unified/updating-tips.md)
- [Arbeiten mit nativen Typen in plattformübergreifenden Apps](~/cross-platform/macios/native-types-cross-platform.md)
- [Klassische Vs einheitliche API-Unterschiede](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
- [Migrieren zu einheitliche API (video)](http://university.xamarin.com/lightninglectures/migrating-to-the-unified-api)
