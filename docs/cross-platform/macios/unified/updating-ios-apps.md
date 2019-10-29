---
title: Aktualisieren vorhandener IOS-apps
description: In diesem Dokument werden die Schritte beschrieben, die zum Aktualisieren einer xamarin. IOS-APP von der Classic API auf die Unified API ausgeführt werden müssen.
ms.prod: xamarin
ms.assetid: 303C36A8-CBF4-48C0-9412-387E95024CAB
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 9b531bd095781c80c5f3418725d57f8f6bbb06fd
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73015033"
---
# <a name="updating-existing-ios-apps"></a>Aktualisieren vorhandener IOS-apps

_Führen Sie die folgenden Schritte aus, um eine vorhandene xamarin. IOS-App für die Verwendung des Unified API zu aktualisieren._

Zum Aktualisieren einer vorhandenen App zur Verwendung der Unified API müssen Änderungen an der Projektdatei selbst sowie an den im Anwendungscode verwendeten Namespaces und APIs vorgenommen werden.

## <a name="the-road-to-64-bits"></a>Der Weg zu 64 Bits

Die neuen Unified APIs sind erforderlich, um 64-Bit-Geräte Architekturen aus einer mobilen xamarin. IOS-Anwendung zu unterstützen. Ab dem 1. Februar 2015 erfordert Apple, dass alle neuen App-Übermittlungen im iTunes App Store 64-Bit-Architekturen unterstützen.

Xamarin bietet Tools für Visual Studio für Mac und Visual Studio, um den Migrationsprozess vom Classic API zum Unified API zu automatisieren, oder Sie können die Projektdateien manuell konvertieren. Während die Verwendung der automatischen Tools dringend empfohlen wird, werden in diesem Artikel beide Methoden behandelt.

### <a name="before-you-start"></a>Bevor Sie beginnen...

Bevor Sie den vorhandenen Code auf den Unified API aktualisieren, wird dringend empfohlen, alle *Kompilierungs Warnungen*auszuschließen. Viele *Warnungen* im Classic API werden nach der Migration zu Unified zu Fehlern. Das Beheben von Fehlern, bevor Sie beginnen, ist einfacher, da die compilernachrichten aus den Classic API häufig Hinweise dazu bereitstellen, was aktualisiert werden muss

## <a name="automated-updating"></a>Automatisiertes aktualisieren

Nachdem die Warnungen korrigiert wurden, wählen Sie ein vorhandenes IOS-Projekt in Visual Studio für Mac oder Visual Studio aus, und wählen Sie im Menü **Projekt** die Option **zu xamarin. IOS-Unified API migrieren** aus. Beispiel:

![](updating-ios-apps-images/beta-tool1.png "Choose Migrate to Xamarin.iOS Unified API from the Project menu")

Sie müssen diese Warnung akzeptieren, bevor die automatisierte Migration ausgeführt wird (natürlich sollten Sie sicherstellen, dass Sie über Sicherungen/Quell Code Verwaltung verfügen, bevor Sie mit diesem Adventure beginnen):

![](updating-ios-apps-images/beta-tool2.png "Agree to this warning before the automated migration will run")

Das Tool automatisiert im Grunde alle Schritte, die im Abschnitt **Manuelles Update manuell** beschrieben werden, und ist die empfohlene Methode, ein vorhandenes xamarin. IOS-Projekt in den Unified API zu wandeln.

## <a name="steps-to-update-manually"></a>Schritte zum manuellen Aktualisieren

Nachdem die Warnungen korrigiert wurden, führen Sie die folgenden Schritte aus, um xamarin. IOS-apps manuell zu aktualisieren und die neuen Unified API zu verwenden:

### <a name="1-update-project-type--build-target"></a>1. Projekttyp aktualisieren & Build-Ziel

Ändern Sie die Projekt Konfiguration in Ihren **csproj** -Dateien von `6BC8ED88-2882-458C-8E55-DFD12B67127B` in `FEACFBD2-3405-455C-9665-78FE426C6842`. Bearbeiten Sie die **csproj** -Datei in einem Text-Editor, und ersetzen Sie das erste Element im `<ProjectTypeGuids>`-Element wie im folgenden dargestellt:

![](updating-ios-apps-images/csproj.png "Edit the csproj file in a text editor, replacing the first item in the ProjectTypeGuids element as shown")

Ändern Sie das **Import** -Element, das `Xamarin.MonoTouch.CSharp.targets` enthält, in `Xamarin.iOS.CSharp.targets`, wie hier gezeigt:

![](updating-ios-apps-images/csproj2.png "Change the Import element that contains Xamarin.MonoTouch.CSharp.targets to Xamarin.iOS.CSharp.targets as shown")

### <a name="2-update-project-references"></a>2. Projekt Verweise aktualisieren

Erweitern Sie den Knoten **Verweise** des IOS-Anwendungs Projekts. Anfänglich wird eine * unterbrochene **MonoTouch** -Referenz ähnlich wie in diesem Screenshot angezeigt (da wir soeben den Projekttyp geändert haben):

![](updating-ios-apps-images/references.png "It will initially show a broken- monotouch reference similar to this screenshot because the project type changed")

Klicken Sie mit der rechten Maustaste auf das IOS-Anwendungsprojekt, um **Verweise zu bearbeiten**, klicken Sie dann auf die **MonoTouch** -Referenz, und löschen Sie Sie mithilfe der roten Schaltfläche "X"

![](updating-ios-apps-images/references-delete-monotouch-sml.png "Right-click on the iOS application project to Edit References, then click on the monotouch reference and delete it using the red X button")

Scrollen Sie nun zum Ende der Verweis Liste, und übertragen Sie die **xamarin. IOS** -Assembly.

![](updating-ios-apps-images/references-add-xamarinios-sml.png "Now scroll to the end of the references list and tick the Xamarin.iOS assembly")

Klicken Sie auf **OK** , um die Projekt Verweis Änderungen zu speichern.

### <a name="3-remove-monotouch-from-namespaces"></a>3. Entfernen von MonoTouch aus Namespaces

Entfernen Sie das **MonoTouch** -Präfix aus Namespaces in `using`-Anweisungen, oder wenn ein Klassenname voll qualifiziert wurde (z. b. `MonoTouch.UIKit` wird nur `UIKit`).

### <a name="4-remap-types"></a>4. Neuzuordnen von Typen

Systemeigene [Typen](~/cross-platform/macios/nativetypes.md) wurden eingeführt, die einige Typen ersetzen, die zuvor verwendet wurden, z. b. Instanzen von `System.Drawing.RectangleF` mit `CoreGraphics.CGRect` (z. b.). Die vollständige Liste der Typen finden Sie auf der Seite [native Typen](~/cross-platform/macios/nativetypes.md) .

### <a name="5-fix-method-overrides"></a>5. korrigieren von Methoden Überschreibungen

Die Signatur einiger `UIKit` Methoden wurde geändert, um die neuen systemeigenen [Typen](~/cross-platform/macios/nativetypes.md) (z. b. `nint`) zu verwenden. Wenn benutzerdefinierte Unterklassen diese Methoden überschreiben, stimmen die Signaturen nicht mehr ab und führen zu Fehlern. Korrigieren Sie diese Methoden Überschreibungen, indem Sie die-Unterklasse so ändern, dass Sie der neuen Signatur mithilfe von systemeigenen Typen entspricht

Beispiele hierfür sind das Ändern von `public override int NumberOfSections (UITableView tableView)`, um `nint` zurückzugeben und das Ändern des Rückgabe Typs und der Parametertypen in `public override int RowsInSection (UITableView tableView, int section)` in `nint`.

## <a name="considerations"></a>Weitere Überlegungen

Beachten Sie die folgenden Überlegungen, wenn Sie ein vorhandenes xamarin. IOS-Projekt vom Classic API in den neuen Unified API umstellen, wenn diese APP von mindestens einer Komponente oder einem nuget-Paket abhängig ist.

### <a name="components"></a>Komponenten

Jede Komponente, die in der Anwendung enthalten ist, muss ebenfalls auf den Unified API aktualisiert werden, oder es tritt ein Konflikt auf, wenn Sie versuchen, eine Kompilierung auszuführen. Ersetzen Sie für jede enthaltene Komponente die aktuelle Version durch eine neue Version aus dem xamarin-Komponenten Speicher, der die Unified API unterstützt, und führen Sie einen sauberen Build aus. Jede Komponente, die noch nicht vom Autor konvertiert wurde, zeigt im Komponenten Speicher eine Warnung mit nur 32 Bit an.

### <a name="nuget-support"></a>NuGet-Unterstützung

Obwohl wir Änderungen an nuget beigetragen haben, um mit der Unified API-Unterstützung zu arbeiten, gibt es noch keine neue Version von nuget. daher evaluieren wir, wie Sie nuget zum Erkennen der neuen APIs erhalten.

Bis zu diesem Zeitpunkt müssen Sie, genau wie die Komponenten, ein beliebiges nuget-Paket, das Sie in Ihrem Projekt enthalten haben, in eine Version wechseln, die die vereinheitlichten APIs unterstützt, und anschließend einen sauberen Build ausführen.

> [!IMPORTANT]
> Wenn ein Fehler in der Form _"Fehler 3 kann nicht sowohl ' MonoTouch. dll ' als auch ' xamarin. IOS. dll ' im gleichen xamarin. IOS-Projekt enthalten ist, wird explizit auf '" "" "" "" "" "" ". neutral, PublicKeyToken = null ' "_ nach der Umstellung der Anwendung in die vereinheitlichten APIs liegt dies in der Regel daran, dass eine Komponente oder ein nuget-Paket im Projekt vorhanden ist, das nicht auf den Unified API aktualisiert wurde. Sie müssen die vorhandene Komponente bzw. nuget entfernen, ein Update auf eine Version durchführen, die die vereinheitlichten APIs unterstützt, und einen sauberen Build durchführen.

## <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Aktivieren von 64-Bit-Builds von xamarin. IOS-apps

Für eine Mobile xamarin. IOS-Anwendung, die in die Unified API konvertiert wurde, muss der Entwickler weiterhin das Entwickeln der Anwendung für 64-Bit-Computer aus den Optionen der APP aktivieren. Ausführliche Anweisungen zum Aktivieren von 64-Bit-Builds finden Sie im Dokument " **Aktivieren von 64-Bit-Builds von xamarin. IOS-apps** " des Dokuments mit der [32/64-Bit-Plattform](~/cross-platform/macios/32-and-64/index.md#enable-64)

## <a name="finishing-up"></a>Wird fertiggestellt

Unabhängig davon, ob Sie die automatische oder manuelle Methode zum Konvertieren Ihrer xamarin. IOS-Anwendung von der klassischen in die vereinheitlichten APIs verwenden, gibt es mehrere Instanzen, die einen weiteren manuellen Eingriff erfordern. Informationen zu bekannten Problemen und Arbeitsaufgaben finden Sie [in den Tipps zum Aktualisieren von Code auf das Unified API](~/cross-platform/macios/unified/updating-tips.md) Dokument.

## <a name="related-links"></a>Verwandte Links

- [Tipps zum Aktualisieren von Code für Unified API](~/cross-platform/macios/unified/updating-tips.md)
- [Arbeiten mit nativen Typen in plattformübergreifenden Apps](~/cross-platform/macios/native-types-cross-platform.md)
- [Unterschiede bei klassischem vs Unified API](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
