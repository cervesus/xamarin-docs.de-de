---
title: Aktualisieren vorhandener IOS-apps
description: In diesem Dokument werden die Schritte beschrieben, die zum Aktualisieren einer xamarin. IOS-APP von der Classic API auf die Unified API ausgeführt werden müssen.
ms.prod: xamarin
ms.assetid: 303C36A8-CBF4-48C0-9412-387E95024CAB
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: b0999ff6fc3b3042827f11ae1e127ef7bb9fedfe
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68509614"
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

![](updating-ios-apps-images/beta-tool1.png "Wählen Sie im Menü Projekt die Option zu xamarin. IOS-Unified API migrieren aus.")

Sie müssen diese Warnung akzeptieren, bevor die automatisierte Migration ausgeführt wird (natürlich sollten Sie sicherstellen, dass Sie über Sicherungen/Quell Code Verwaltung verfügen, bevor Sie mit diesem Adventure beginnen):

![](updating-ios-apps-images/beta-tool2.png "Diese Warnung zustimmen, bevor die automatisierte Migration ausgeführt wird")

Das Tool automatisiert im Grunde alle Schritte, die im Abschnitt **Manuelles Update manuell** beschrieben werden, und ist die empfohlene Methode, ein vorhandenes xamarin. IOS-Projekt in den Unified API zu wandeln.

## <a name="steps-to-update-manually"></a>Schritte zum manuellen Aktualisieren

Nachdem die Warnungen korrigiert wurden, führen Sie die folgenden Schritte aus, um xamarin. IOS-apps manuell zu aktualisieren und die neuen Unified API zu verwenden:

### <a name="1-update-project-type--build-target"></a>1. Projekttyp & Build-Ziel aktualisieren

Ändern Sie den Projekt Geschmack in Ihren **csproj** - `6BC8ED88-2882-458C-8E55-DFD12B67127B` Dateien `FEACFBD2-3405-455C-9665-78FE426C6842`von in. Bearbeiten Sie die **csproj** -Datei in einem Text-Editor, und ersetzen Sie `<ProjectTypeGuids>` das erste Element im-Element wie gezeigt:

![](updating-ios-apps-images/csproj.png "Bearbeiten Sie die CSPROJ-Datei in einem Text-Editor, und ersetzen Sie das erste Element im projecttypguids-Element wie gezeigt.")

Ändern Sie  `Xamarin.MonoTouch.CSharp.targets` dasImport-Element,dasenthält,wieimfolgenden`Xamarin.iOS.CSharp.targets` gezeigt:

![](updating-ios-apps-images/csproj2.png "Ändern Sie das Import-Element, das xamarin. MonoTouch. CSharp. targets enthält, in xamarin. IOS. CSharp. targets, wie hier gezeigt.")

### <a name="2-update-project-references"></a>2. Projekt Verweise aktualisieren

Erweitern Sie den Knoten **Verweise** des IOS-Anwendungs Projekts. Anfänglich wird eine * unterbrochene **MonoTouch** -Referenz ähnlich wie in diesem Screenshot angezeigt (da wir soeben den Projekttyp geändert haben):

![](updating-ios-apps-images/references.png "Anfänglich wird ein fehlerhafter MonoTouch-Verweis angezeigt, der diesem Screenshot ähnelt, weil der Projekttyp geändert wurde.")

Klicken Sie mit der rechten Maustaste auf das IOS-Anwendungsprojekt, um **Verweise zu bearbeiten**, klicken Sie dann auf die **MonoTouch** -Referenz, und löschen Sie Sie mithilfe der roten Schaltfläche "X"

![](updating-ios-apps-images/references-delete-monotouch-sml.png "Klicken Sie mit der rechten Maustaste auf das IOS-Anwendungsprojekt, um Verweise zu bearbeiten, und klicken Sie dann auf die MonoTouch-Referenz, und löschen Sie Sie mithilfe der roten X")

Scrollen Sie nun zum Ende der Verweis Liste, und übertragen Sie die **xamarin. IOS** -Assembly.

![](updating-ios-apps-images/references-add-xamarinios-sml.png "Scrollen Sie nun zum Ende der Verweis Liste, und führen Sie die xamarin. IOS-Assembly aus.")

Klicken Sie auf **OK** , um die Projekt Verweis Änderungen zu speichern.

### <a name="3-remove-monotouch-from-namespaces"></a>3. MonoTouch aus Namespaces entfernen

Entfernen Sie das **MonoTouch** -Präfix aus Namespaces in-Anweisungen oder an jedem Ort, an `using` dem ein Klassenname voll qualifiziert wurde (z.b. `MonoTouch.UIKit`wird gerade `UIKit`).

### <a name="4-remap-types"></a>4. Neuzuordnen von Typen

Systemeigene [Typen](~/cross-platform/macios/nativetypes.md) wurden eingeführt, die einige Typen ersetzen, die zuvor verwendet wurden, z. `System.Drawing.RectangleF` b `CoreGraphics.CGRect` . Instanzen von mit (z. b.). Die vollständige Liste der Typen finden Sie auf der Seite [native Typen](~/cross-platform/macios/nativetypes.md) .

### <a name="5-fix-method-overrides"></a>5. Korrigieren von Methoden Überschreibungen

Die `UIKit` Signatur einiger Methoden wurde geändert, um die neuen systemeigenen [Typen](~/cross-platform/macios/nativetypes.md) (z `nint`. b.) zu verwenden. Wenn benutzerdefinierte Unterklassen diese Methoden überschreiben, stimmen die Signaturen nicht mehr ab und führen zu Fehlern. Korrigieren Sie diese Methoden Überschreibungen, indem Sie die-Unterklasse so ändern, dass Sie der neuen Signatur mithilfe von systemeigenen Typen entspricht

Beispiele hierfür sind `public override int NumberOfSections (UITableView tableView)` das ändern `nint` von zum zurückgeben und Ändern von Rückgabetyp `nint`-und Parametertypen in in `public override int RowsInSection (UITableView tableView, int section)` .

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
