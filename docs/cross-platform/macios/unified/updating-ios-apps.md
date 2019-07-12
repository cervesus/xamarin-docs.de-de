---
title: Aktualisieren von vorhandenen iOS-Apps
description: Dieses Dokument beschreibt die Schritte, die beachtet werden müssen, um eine Xamarin.iOS-app von der klassischen API, die Unified API zu aktualisieren.
ms.prod: xamarin
ms.assetid: 303C36A8-CBF4-48C0-9412-387E95024CAB
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 4e471aaca2a7a5f247b21dd1c1863a01b062a716
ms.sourcegitcommit: afe9d93373d66eb45d82cabefca83b5733969634
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "67855751"
---
# <a name="updating-existing-ios-apps"></a>Aktualisieren von vorhandenen iOS-Apps

_Um einer vorhandenen Xamarin.iOS-app zum Verwenden der Unified API zu aktualisieren, gehen Sie wie folgt vor._

Aktualisiert eine vorhandene app zum Verwenden der Unified API erfordert Änderungen an der Projektdatei selbst sowie über die Namespaces und APIs, die im Anwendungscode verwendet.

## <a name="the-road-to-64-bits"></a>Der Weg zur 64-Bit

Das neue Unified-APIs sind zur Unterstützung von 64-Bit-Gerät-Architekturen in einer mobilen Xamarin.iOS-Anwendung erforderlich. Ab dem 1. Februar 2015 erfordert Apple an, dass alle neuen Einreichen von Apps für die iTunes App Store 64-Bit-Architekturen zu unterstützen.

Xamarin bietet Tools für Visual Studio für Mac und Visual Studio während der Migration von der klassischen API, die Unified API zu automatisieren, oder Sie können die Projektdateien manuell konvertieren. Während die mit der automatischen Tools hoch empfohlen wird, werden beide Methoden abgedeckt.

### <a name="before-you-start"></a>Bevor Sie beginnen...

Bevor Sie den vorhandenen Code der Unified API aktualisieren, es wird dringend empfohlen, dass Sie alle beseitigen *compilerwarnungen*. Viele *Warnungen* in der klassischen API werden Fehler nach der Migration zu Unified. Beheben diese, bevor Sie beginnen ist einfacher, da compilermeldungen von der klassischen API häufig Hinweise dazu, was Sie aktualisieren bereitstellen.

## <a name="automated-updating"></a>Automatisierte Updates

Sobald die Warnungen behoben wurden, wählen Sie ein vorhandenes iOS-Projekt in Visual Studio für Mac oder Visual Studio, und wählen Sie **Migration zur vereinheitlichten Xamarin.iOS-API** aus der **Projekt** Menü. Beispiel:

![](updating-ios-apps-images/beta-tool1.png "Vereinheitlichten Xamarin.iOS-API migrieren möchten, aus dem Menü \"Projekt\"")

Sie müssen zustimmen, um diese Warnung an, bevor die automatisierte Migration ausgeführt werden kann (Natürlich sollten Sie sicherstellen Sie Sicherungen/Datenquellen-Steuerelement vor diesem Adventure haben):

![](updating-ios-apps-images/beta-tool2.png "Stimmen Sie um diese Warnung zu, bevor die automatisierte Migration ausgeführt werden kann")

Das Tool im Grunde automatisiert alle Schritte der **Update manuell** Abschnitt unten und ist die vorgeschlagene Methode konvertieren Sie ein vorhandenes Xamarin.iOS-Projekt in der Unified API.

## <a name="steps-to-update-manually"></a>So aktualisieren Sie manuell

In diesem Fall, sobald die Warnungen behoben wurden, gehen Sie zum manuellen Aktualisieren der Xamarin.iOS-apps, um die neue Unified API verwenden:

### <a name="1-update-project-type--build-target"></a>1. Update-Projekttyp & Build-Ziel

Ändern Sie die Projektkonfiguration in Ihre **Csproj** Dateien von `6BC8ED88-2882-458C-8E55-DFD12B67127B` zu `FEACFBD2-3405-455C-9665-78FE426C6842`. Bearbeiten der **Csproj** -Datei in einem Text-Editor, und Ersetzen Sie dabei das erste Element in der `<ProjectTypeGuids>` -Element wie gezeigt:

![](updating-ios-apps-images/csproj.png "Bearbeiten Sie die Csproj-Datei in einem Text-Editor, und Ersetzen Sie dabei das erste Element im ProjectTypeGuids-Element wie gezeigt")

Ändern der **Import** -Element mit `Xamarin.MonoTouch.CSharp.targets` zu `Xamarin.iOS.CSharp.targets` wie gezeigt:

![](updating-ios-apps-images/csproj2.png "Ändern Sie das Import-Element, das Xamarin.MonoTouch.CSharp.targets zu Xamarin.iOS.CSharp.targets enthält (siehe)")

### <a name="2-update-project-references"></a>2. Aktualisieren von Projektverweisen

Erweitern Sie des iOS-Anwendungsprojekts **Verweise** Knoten. Anfänglich angezeigt ein * unterbrochen - **Monotouch** Verweis ist (da wir gerade den Projekttyp geändert) etwa so:

![](updating-ios-apps-images/references.png "Es wird zunächst einen unterteilt Monotouch-Verweis etwa so angezeigt, weil der Projekttyp geändert")

Mit der rechten Maustaste auf das iOS-Anwendungsprojekt zu **Verweise bearbeiten**, klicken Sie dann auf die **Monotouch** verweisen, und löschen Sie sie mithilfe der Schaltfläche rotes "X".

![](updating-ios-apps-images/references-delete-monotouch-sml.png "Mit der rechten Maustaste auf das iOS-Anwendungsprojekt, um Verweise bearbeiten, und klicken Sie auf der Monotouch-Verweis und löschen Sie sie über das rote X-Schaltfläche")

Scrollen Sie bis zum Ende der Verweisliste und -Tick der **Xamarin.iOS** Assembly.

![](updating-ios-apps-images/references-add-xamarinios-sml.png "Scrollen Sie bis zum Ende der Verweisliste und -Tick der Xamarin.iOS-assembly")

Drücken Sie **OK** um das Projekt Verweise Änderungen zu speichern.

### <a name="3-remove-monotouch-from-namespaces"></a>3. Entfernen Sie MonoTouch von Namespaces

Entfernen Sie die **MonoTouch** Präfix von Namespaces in `using` Anweisungen oder ganz egal, wo ein Klassenname (z. b. vollqualifizierte wurde hat `MonoTouch.UIKit` wird lediglich `UIKit`).

### <a name="4-remap-types"></a>4. Neuzuordnen von Typen

[Systemeigene Typen](~/cross-platform/macios/nativetypes.md) wurden eingeführt, die einige Typen ersetzen, die zuvor, z. B. Instanzen der verwendet wurden `System.Drawing.RectangleF` mit `CoreGraphics.CGRect` (z. B.). Die vollständige Liste der Typen finden Sie auf die [systemeigene Typen](~/cross-platform/macios/nativetypes.md) Seite.

### <a name="5-fix-method-overrides"></a>5. Beheben Sie die Methodenüberschreibungen

Einige `UIKit` Methoden hatten die Signatur geändert, um die Verwendung der neuen [systemeigene Typen](~/cross-platform/macios/nativetypes.md) (z. B. `nint`). Wenn Sie benutzerdefinierte Unterklassen diese Methoden überschreiben, die Signaturen nicht mehr überein, und führt zu Fehlern. Beheben Sie diese Methode überschreibt, indem die Unterklasse die neue Signatur, die mit systemeigenen Typen entsprechend ändern.

Beispielen zählen die Änderung `public override int NumberOfSections (UITableView tableView)` zurückzugebenden `nint` und ändern beide, Typ und die Parametertypen in zurückgeben `public override int RowsInSection (UITableView tableView, int section)` zu `nint`.

## <a name="considerations"></a>Weitere Überlegungen

Die folgenden Aspekte sollten bei der Konvertierung eines vorhandenen Xamarin.iOS-Projekts aus der klassischen API auf die neue einheitliche API, wenn diese app mindestens eine Komponente oder NuGet-Paket benötigt berücksichtigt werden.

### <a name="components"></a>Komponenten

Jede Komponente, die Sie in Ihrer Anwendung aufgenommen haben, müssen auch die Unified API aktualisiert werden, oder Sie erhalten einen Konflikt auf, wenn Sie versuchen, zu kompilieren. Ersetzen Sie die aktuelle Version mit einer neuen Version von der Xamarin Component Store, die die Unified API unterstützt, und führen Sie einen bereinigten Build, für eine beliebige Komponente enthalten. Jede Komponente, die noch nicht vom Autor, konvertiert wurde, wird eine 32-Bit nur im Komponentenspeicher Warnung angezeigt.

### <a name="nuget-support"></a>NuGet-Unterstützung

Während wir Änderungen an NuGet mit der Unified API-Unterstützung funktioniert hat, wurde keine neue Version von NuGet, damit wir zum Abrufen von NuGet, um die neuen APIs erkennt auswerten.

Bis zu diesem Zeitpunkt an, wie die Komponenten müssen Sie zum Wechseln von alle NuGet-Paket, das Sie in Ihrem Projekt auf eine Version aufgenommen haben, die Unified-APIs unterstützt, und führen anschließend einen bereinigten Build.

> [!IMPORTANT]
> Wenn Sie einen Fehler in der Form haben _"Fehler 3 kann nicht sowohl"monotouch.dll"und"Xamarin.iOS.dll"im gleichen Xamarin.iOS-Projekt enthalten – explizit 'Xamarin.iOS.dll' verwiesen wird, solange"monotouch.dll"verwiesen wird" Xxx "," Version = 0.0.000, Kultur = Neutral, PublicKeyToken = Null'"_ nach der Konvertierung Ihrer Anwendung auf die Unified-APIs, es liegt in der Regel müssen entweder eine Komponente oder ein NuGet-Paket im Projekt, das nicht der Unified API aktualisiert wurde. Sie müssen zum Entfernen des vorhandenen Komponente/NuGet-Pakets, ein update auf eine Version, die Unified-APIs unterstützt, und führen Sie einen bereinigten Build.

## <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Aktivieren die 64-Bit-Builds von Xamarin.iOS-Apps

Für eine mobile Xamarin.iOS-Anwendung, die in der Unified API konvertiert wurde, muss der Entwickler weiterhin das Erstellen der Anwendung für 64-Bit-Computer aus der app-Optionen zu aktivieren. Finden Sie unter den **Aktivieren von 64 Bit Builds von Xamarin.iOS-Apps** von der [32/64 Bit Platform Considerations](~/cross-platform/macios/32-and-64/index.md#enable-64) Dokument für detaillierte Anweisungen zum Aktivieren von 64-Bit-builds.

## <a name="finishing-up"></a>Wird abgeschlossen

Unabhängig davon, ob Sie die automatische oder manuelle Methode verwenden, um Ihre Xamarin.iOS-Anwendung vom klassischen Bereitstellungsmodell auf die Unified-APIs zu konvertieren möchten, stehen mehrere Instanzen, die darüber hinaus manuelle Eingriffe erfordern. Informieren Sie sich unsere [Tipps zum Aktualisieren von Code, der Unified API](~/cross-platform/macios/unified/updating-tips.md) Dokument für bekannte Probleme und problemumgehungen.

## <a name="related-links"></a>Verwandte Links

- [Tipps zum Aktualisieren von Code für Unified API](~/cross-platform/macios/unified/updating-tips.md)
- [Arbeiten mit nativen Typen in plattformübergreifenden Apps](~/cross-platform/macios/native-types-cross-platform.md)
- [Klassischen Vs Unified API-Unterschiede](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
