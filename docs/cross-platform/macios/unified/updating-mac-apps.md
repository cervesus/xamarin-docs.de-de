---
title: Aktualisieren vorhandener Mac-apps
description: In diesem Dokument werden die Schritte beschrieben, die zum Aktualisieren einer xamarin. Mac-app von der Classic API auf die Unified API ausgeführt werden müssen.
ms.prod: xamarin
ms.assetid: 26673CC5-C1E5-4BAC-BEF4-9A386B296FD5
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 43498c0609fdbe6dba59b9ed5926c9c58b72d4db
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70280868"
---
# <a name="updating-existing-mac-apps"></a>Aktualisieren vorhandener Mac-apps

Zum Aktualisieren einer vorhandenen App zur Verwendung der Unified API müssen Änderungen an der Projektdatei selbst sowie an den im Anwendungscode verwendeten Namespaces und APIs vorgenommen werden.

## <a name="the-road-to-64-bits"></a>Der Weg zu 64 Bits

Die neuen Unified APIs sind erforderlich, um 64-Bit-Geräte Architekturen aus einer xamarin. Mac-Anwendung zu unterstützen. Ab dem 1. Februar 2015 erfordert Apple, dass alle neuen App-Übermittlungen im Mac App Store 64-Bit-Architekturen unterstützen.

Xamarin bietet Tools für Visual Studio für Mac und Visual Studio, um den Migrationsprozess vom Classic API zum Unified API zu automatisieren, oder Sie können die Projektdateien manuell konvertieren. Während die Verwendung der automatischen Tools dringend empfohlen wird, werden in diesem Artikel beide Methoden behandelt.

### <a name="before-you-start"></a>Bevor Sie beginnen...

Bevor Sie den vorhandenen Code auf den Unified API aktualisieren, wird dringend empfohlen, alle *Kompilierungs Warnungen*auszuschließen. Viele *Warnungen* im Classic API werden nach der Migration zu Unified zu Fehlern. Das Beheben von Fehlern, bevor Sie beginnen, ist einfacher, da die compilernachrichten aus den Classic API häufig Hinweise dazu bereitstellen, was aktualisiert werden muss

## <a name="automated-updating"></a>Automatisiertes aktualisieren

Nachdem die Warnungen korrigiert wurden, wählen Sie ein vorhandenes Mac-Projekt in Visual Studio für Mac oder Visual Studio aus, und wählen Sie im Menü **Projekt** die Option **zu xamarin. Unified API Mac migrieren** aus. Beispiel:

![](updating-mac-apps-images/beta-tool1.png "Wählen Sie im Menü Projekt die Option zu xamarin. Unified API Mac Migrieren aus.")

Sie müssen diese Warnung akzeptieren, bevor die automatisierte Migration ausgeführt wird (natürlich sollten Sie sicherstellen, dass Sie über Sicherungen/Quell Code Verwaltung verfügen, bevor Sie mit diesem Adventure beginnen):

![](updating-mac-apps-images/migrate01.png "Diese Warnung zustimmen, bevor die automatisierte Migration ausgeführt wird")

Es gibt zwei unterstützte Ziel Framework-Typen, die ausgewählt werden können, wenn die Unified API in einer xamarin. Mac-Anwendung verwendet wird:

- **Xamarin. Mac Mobile Framework** : Dies ist das gleiche optimierte .NET Framework, das von xamarin. IOS und xamarin. Android verwendet wird und unterstützt eine Teilmenge des vollständigen **Desktop** -Frameworks. Dies ist das empfohlene Framework, da es kleinere durchschnittliche Binärdateien aufgrund eines überlegenen Verknüpfungs Verhaltens bereitstellt.
- **Xamarin. Mac .NET 4,5 Framework** : Dieses Framework ist erneut, eine Teilmenge des **Desktop-** Frameworks. Allerdings wird das vollständige **Desktop** Framework weniger als das **Mobile** Framework verwendet, und es sollte mit den meisten nuget-Paketen oder Bibliotheken von Drittanbietern _"einfach arbeiten"_ . Dadurch kann der **Entwickler standardmäßige** Desktopassemblys nutzen, während er weiterhin ein unterstütztes Framework verwendet. diese Option erzeugt jedoch größere Anwendungspakete. Dies ist das empfohlene Framework, bei dem .NET-Assemblys von Drittanbietern verwendet werden, die nicht mit dem **mobilen xamarin. Mac-Framework**kompatibel sind. Eine Liste der unterstützten Assemblys finden Sie [in unserer](~/cross-platform/internals/available-assemblies.md) assemblydokumentation.

Ausführliche Informationen zu Ziel-Frameworks und den Auswirkungen der Auswahl eines bestimmten Ziels für Ihre xamarin. Mac-Anwendung finden Sie in der Dokumentation zu den [Ziel-Frameworks](~/mac/platform/target-framework.md) . 

Das Tool automatisiert im Grunde alle Schritte, die im Abschnitt **Manuelles Update manuell** beschrieben werden, und ist die empfohlene Methode, ein vorhandenes xamarin. Mac-Projekt in den Unified API zu wandeln.

## <a name="steps-to-update-manually"></a>Schritte zum manuellen Aktualisieren

Nachdem die Warnungen korrigiert wurden, führen Sie die folgenden Schritte aus, um xamarin. Mac-apps manuell zu aktualisieren und die neuen Unified API zu verwenden:

### <a name="1-update-project-type--build-target"></a>1. Projekttyp & Build-Ziel aktualisieren

Ändern Sie den Projekt Geschmack in Ihren **csproj** - `42C0BBD9-55CE-4FC1-8D90-A7348ABAFB23` Dateien `A3F8F2AB-B479-4A4A-A458-A89E7DC349F1`von in. Bearbeiten Sie die **csproj** -Datei in einem Text-Editor, und ersetzen Sie `<ProjectTypeGuids>` das erste Element im-Element wie gezeigt:

![](updating-mac-apps-images/csproj.png "Bearbeiten Sie die CSPROJ-Datei in einem Text-Editor, und ersetzen Sie das erste Element im projecttypguids-Element wie gezeigt.")

Ändern Sie `Xamarin.Mac.targets` dasImport-Element,dasenthält,wieimfolgenden`Xamarin.Mac.CSharp.targets` gezeigt:

![](updating-mac-apps-images/csproj2.png "Ändern Sie das Import-Element, das xamarin. Mac. targets enthält, in xamarin. Mac. CSharp. targets, wie hier gezeigt.")

Fügen Sie nach dem `<AssemblyName>` -Element die folgenden Codezeilen hinzu:

```xml
<TargetFrameworkVersion>v2.0</TargetFrameworkVersion>
<TargetFrameworkIdentifier>Xamarin.Mac</TargetFrameworkIdentifier>

```

Beispiel:

![Fügen Sie nach dem Element AssemblyName > die \<folgenden Codezeilen hinzu.](updating-mac-apps-images/csproj3.png)

### <a name="2-update-project-references"></a>2. Projekt Verweise aktualisieren

Erweitern Sie den Knoten **Verweise** des Mac-Anwendungs Projekts. Anfänglich wird ein * fehlerhafter **xammac** -Verweis angezeigt, der diesem Screenshot ähnelt (da wir soeben den Projekttyp geändert haben):

![](updating-mac-apps-images/references.png "Anfänglich wird ein fehlerhafter xammac-Verweis angezeigt, der diesem Screenshot ähnelt.")

Klicken Sie neben dem Eintrag **xammac** auf das **Zahnrad Symbol** , und wählen Sie **Löschen** aus, um die beschädigte Referenz zu entfernen.

Klicken Sie anschließend im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Verweise** , und wählen Sie **Verweise bearbeiten**aus. Scrollen Sie zum Ende der Liste der Verweise, und platzieren Sie eine Prüfung neben **xamarin. Mac**.

![](updating-mac-apps-images/references2.png "Scrollen Sie zum Ende der Liste der Verweise, und platzieren Sie eine Prüfung neben xamarin. Mac.")

Klicken Sie auf **OK** , um die Projekt Verweis Änderungen zu speichern.

### <a name="3-remove-monomac-from-namespaces"></a>3. Entfernen von monomac aus Namespaces

Entfernen Sie das **monomac** -Präfix aus Namespaces in-Anweisungen oder an jedem Ort, an `using` dem ein Klassenname voll qualifiziert wurde (z.b. `MonoMac.AppKit`wird gerade `AppKit`).

### <a name="4-remap-types"></a>4. Neuzuordnen von Typen

Systemeigene [Typen](~/cross-platform/macios/nativetypes.md) wurden eingeführt, die einige Typen ersetzen, die zuvor verwendet wurden, z. `System.Drawing.RectangleF` b `CoreGraphics.CGRect` . Instanzen von mit (z. b.). Die vollständige Liste der Typen finden Sie auf der Seite [native Typen](~/cross-platform/macios/nativetypes.md) .

### <a name="5-fix-method-overrides"></a>5. Korrigieren von Methoden Überschreibungen

Die `AppKit` Signatur einiger Methoden wurde geändert, um die neuen systemeigenen [Typen](~/cross-platform/macios/nativetypes.md) (z `nint`. b.) zu verwenden. Wenn benutzerdefinierte Unterklassen diese Methoden überschreiben, stimmen die Signaturen nicht mehr ab und führen zu Fehlern. Korrigieren Sie diese Methoden Überschreibungen, indem Sie die-Unterklasse so ändern, dass Sie der neuen Signatur mithilfe von systemeigenen Typen entspricht 

## <a name="considerations"></a>Weitere Überlegungen

Beachten Sie die folgenden Überlegungen, wenn Sie ein vorhandenes xamarin. Mac-Projekt vom Classic API in den neuen Unified API umstellen, wenn diese APP von mindestens einer Komponente oder einem nuget-Paket abhängig ist. 

### <a name="components"></a>Komponenten

Jede Komponente, die in der Anwendung enthalten ist, muss ebenfalls auf den Unified API aktualisiert werden, oder es tritt ein Konflikt auf, wenn Sie versuchen, eine Kompilierung auszuführen. Ersetzen Sie für jede enthaltene Komponente die aktuelle Version durch eine neue Version aus dem xamarin-Komponenten Speicher, der die Unified API unterstützt, und führen Sie einen sauberen Build aus. Jede Komponente, die noch nicht vom Autor konvertiert wurde, zeigt im Komponenten Speicher eine Warnung mit nur 32 Bit an.

### <a name="nuget-support"></a>NuGet-Unterstützung

Obwohl wir Änderungen an nuget beigetragen haben, um mit der Unified API-Unterstützung zu arbeiten, gibt es noch keine neue Version von nuget. daher evaluieren wir, wie Sie nuget zum Erkennen der neuen APIs erhalten. 

Bis zu diesem Zeitpunkt müssen Sie, genau wie die Komponenten, ein beliebiges nuget-Paket, das Sie in Ihrem Projekt enthalten haben, in eine Version wechseln, die die vereinheitlichten APIs unterstützt, und anschließend einen sauberen Build ausführen.

> [!IMPORTANT]
> Wenn ein Fehler in der Form _"Fehler 3 kann nicht sowohl" monomac. dll "als auch" xamarin. Mac. dll "im gleichen xamarin. Mac-Projekt enthalten ist, wird explizit auf" xamarin. Mac. dll "verwiesen, und auf" monomac. dll "wird von ' xxx, Version = 0.0.000, Culture = neutral verwiesen. PublicKeyToken = null ' '_ nach der Umstellung der Anwendung in die vereinheitlichten APIs ist dies in der Regel darauf zurückzuführen, dass eine Komponente oder ein nuget-Paket im Projekt vorhanden ist, das nicht auf den Unified API aktualisiert wurde. Sie müssen die vorhandene Komponente bzw. nuget entfernen, ein Update auf eine Version durchführen, die die vereinheitlichten APIs unterstützt, und einen sauberen Build durchführen.

## <a name="enabling-64-bit-builds-of-xamarinmac-apps"></a>Aktivieren von 64-Bit-Builds von xamarin. Mac-apps

Für eine Mobile xamarin. Mac-Anwendung, die in die Unified API konvertiert wurde, muss der Entwickler weiterhin das Entwickeln der Anwendung für 64-Bit-Computer aus den Optionen der APP aktivieren. Ausführliche Anweisungen zum Aktivieren von 64-Bit-Builds finden Sie im Dokument " **Aktivieren von 64-Bit-Builds von xamarin. Mac-apps** " des Dokuments mit der [32/64-Bit-Plattform](~/cross-platform/macios/32-and-64/index.md)

## <a name="finishing-up"></a>Wird fertiggestellt

Unabhängig davon, ob Sie die automatische oder manuelle Methode zum Konvertieren Ihrer xamarin. Mac-Anwendung aus dem klassischen Modell in die vereinheitlichten APIs verwenden möchten, gibt es mehrere Instanzen, die einen weiteren manuellen Eingriff erfordern. Informationen zu bekannten Problemen und Arbeitsaufgaben finden Sie [in den Tipps zum Aktualisieren von Code auf das Unified API](~/cross-platform/macios/unified/updating-tips.md) Dokument.

## <a name="related-links"></a>Verwandte Links

- [Tipps zum Aktualisieren von Code für Unified API](~/cross-platform/macios/unified/updating-tips.md)
- [Arbeiten mit nativen Typen in plattformübergreifenden Apps](~/cross-platform/macios/native-types-cross-platform.md)
- [Unterschiede bei klassischem vs Unified API](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
