---
title: Aktualisieren von Xamarin.Mac Unified Anwendungen auf 64-bit
description: Dieses Handbuch beschreibt die abzielen auf 64-Bit-Anwendungen Xamarin.Mac aktualisieren
ms.prod: xamarin
ms.assetid: C3810A74-539C-4FFB-B47F-68CA5F7BCDAD
author: bradumbaugh
ms.author: brumbaug
ms.date: 02/22/2018
ms.openlocfilehash: 558edbdee5adfe57205c7f76b35a0538c78b927f
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="updating-xamarinmac-unified-applications-to-64-bit"></a>Aktualisieren von Xamarin.Mac Unified Anwendungen auf 64-bit

Seit Januar 2018, Apple erfordert, dass neue [Mac App Store-Übermittlungen abzielen, 64-Bit-](https://developer.apple.com/news/?id=06282017a). Apps, die auf dem Mac App Store bereits verfügbar müssen auf 64-Bit-Ziel von Juni 2018 aktualisiert werden.

Die **Datei** > **neu** Xamarin.Mac-Projektvorlage erstellt 64-Bit-Anwendungen standardmäßig, daher werden apps, die zuletzt erstellten bereits 64-Bit-kompatibel und erfordern keine Änderungen.

## <a name="targeting-64-bit"></a>64-Bit-Ziel

1. Öffnen der **Projektoptionen** -Fenster sind Ihre Xamarin.Mac-app:

   ![Kontextmenü für das Projekt](mac-64-bit-images/1-contextual_menu-vsmac.png "im Kontextmenü für das Projekt")

2. Wählen Sie **Mac erstellen** und legen Sie **unterstützt Architekturen** auf **X86\_64**:

   [![Festlegen der unterstützten Architekturen, x86_64](mac-64-bit-images/2-project_options-vsmac.png "unterstützten Architekturen, x86_64 festlegen")](mac-64-bit-images/2-project_options-vsmac-large.png#lightbox)

3. Wenn Ihre app externen Abhängigkeiten, z. B. systemeigene Verweise oder Bindung Projekte verfügt, aktualisieren Sie diese auf 64-Bit-Ziel.

### <a name="errors"></a>Fehler

Zum ersten Mal erstellen oder Ausführen der Anwendung mit 64-Bit-Unterstützung können Link Fehler von Clang oder Laufzeit Probleme auftreten. Diese Fehler können auftreten, wenn die Drittanbieter-Abhängigkeiten – z. B. systemeigene Verweise im Xamarin.Mac oder Bindungen Projekte oder eine systemweite Frameworks manuell geladen – nicht auf 64-Bit aktualisiert wurden.

> [!TIP]
> Konvertieren das Projekt in 64-Bit ist eine wesentliche Änderung, und möglicherweise verschiedene Programmierfehler indirekt aufdecken. Insbesondere können sie ändern die Größe und Ausrichtung der Datenstrukturen, die Signaturen p/Invoke aufrufen und systemeigenen Code in Ihrem Projekt verknüpfte auswirken würde. Sollten Sie die Buildwarnungen erhält und Testen der Anwendung throughly anschließend zum Abfangen von potenziellen Probleme.

#### <a name="example-error-resulting-from-a-dynamically-linked-third-party-dependency-that-does-not-target-64-bit"></a>Beispiel-Fehler, die eine dynamisch verknüpfte Drittanbieter-Abhängigkeit, die nicht auf 64-Bit-Ziel hat gemeldet:

```console
ld : warning : ignoring file PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary, 
file was built for i386 which is not the architecture being linked (x86_64): 
PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary 
```

Dieser Fehler kann zur Laufzeit durch folgen `dlopen` zurückgeben `IntPtr.Zero` anstelle eines erwarteten Handles.

#### <a name="example-error-resulting-from-a-statically-linked-third-party-dependency-that-does-not-target-64-bit"></a>Beispiel-Fehler aufgrund einer statisch verknüpften Drittanbieter-Abhängigkeit, die nicht auf 64-Bit-Ziel hat:

```console
Undefined symbols for architecture x86_64:
  "_LibraryFunction", referenced from:
     -u command line option
ld: symbol(s) not found for architecture x86_64 
```

Zum Erstellen und erfolgreich ausgeführt werden, müssen diese Abhängigkeiten auf 64-Bit aktualisieren und neu kompilieren der app.

