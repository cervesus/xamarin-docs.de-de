---
title: Aktualisieren von Xamarin.Mac Unified-Anwendungen auf 64-bit
description: Dieses Handbuch beschreibt, wie Sie Ihre Xamarin.Mac-Anwendungen auf 64-Bit-Ziel zu aktualisieren. Darüber hinaus Beispiele für die Arten von Fehlern, die gefunden werden können, wenn Sie diese Änderung vornehmen.
ms.prod: xamarin
ms.assetid: C3810A74-539C-4FFB-B47F-68CA5F7BCDAD
author: conceptdev
ms.author: crdun
ms.date: 02/22/2018
ms.openlocfilehash: 9bd70fec5d6d3bbbc4855980e1542bd4e486acaa
ms.sourcegitcommit: 849bf6d1c67df943482ebf3c80c456a48eda1e21
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51528519"
---
# <a name="updating-xamarinmac-unified-applications-to-64-bit"></a>Aktualisieren von Xamarin.Mac Unified-Anwendungen auf 64-bit

Seit Januar 2018, Apple erfordert, dass neue [Mac App Store-Übermittlungen abzielen, 64-Bit-](https://developer.apple.com/news/?id=06282017a). Apps, die auf den Mac App Store bereits verfügbar sind, müssen zum abzielen auf 64-Bit-Juni 2018 aktualisiert werden.

Die **Datei** > **neu** Xamarin.Mac-Projektvorlage 64-Bit-Anwendungen in der Standardeinstellung erstellt, damit alle zuletzt erstellten apps bereits 64-Bit-kompatibel sind, und sind keine Änderungen erforderlich.

## <a name="targeting-64-bit"></a>64-Bit-Zielplattformen

1. Öffnen der **Projektoptionen** Fenster für die Xamarin.Mac-app:

   ![Das Kontextmenü für das Projekt](mac-64-bit-images/1-contextual_menu-vsmac.png "im Kontextmenü für das Projekt")

2. Wählen Sie **Mac Build** und **unterstützte Architekturen** zu **X86\_64**:

   [![Wenn die unterstützten Architekturen auf x86_64](mac-64-bit-images/2-project_options-vsmac.png "die unterstützten Architekturen auf x86_64 festlegen")](mac-64-bit-images/2-project_options-vsmac-large.png#lightbox)

3. Wenn Ihre app externen Abhängigkeiten wie native Verweise oder bindungsprojekte aufweist, aktualisieren Sie sie zum abzielen auf 64-Bit ein.

### <a name="errors"></a>Fehler

Beim ersten Erstellen oder Ausführen Ihrer Anwendung mit 64-Bit-Unterstützung, können Sie Link-Fehler Clang oder Common Language Runtime-Probleme auftreten. Diese Fehler können auftreten, wenn die Drittanbieter-Abhängigkeiten, z. B. native Verweise Ihrer Xamarin.Mac Bindungen Projekte oder eine systemweite Frameworks manuell geladen – nicht auf 64-Bit aktualisiert wurden.

> [!TIP]
> Konvertieren das Projekt in 64-Bit ist eine wesentliche Änderung, und es möglicherweise indirekt verschiedener Programmierfehler entdecken. Insbesondere können sie ändern die Größe und Ausrichtung der Datenstrukturen, die p/invoke-Signaturen und systemeigenem Code in Ihrem Projekt verknüpft auswirken würde. Lesen Sie bei einem-Warnungen erhalten Build, und die Anwendung gründlich testen anschließend, um potenzielle Probleme zu erfassen.

#### <a name="example-error-resulting-from-a-dynamically-linked-third-party-dependency-that-does-not-target-64-bit"></a>Beispiel-Fehler durch eine dynamisch verknüpften Drittanbieter-Abhängigkeit, die nicht als 64-Bit-Ziel:

```console
ld : warning : ignoring file PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary, 
file was built for i386 which is not the architecture being linked (x86_64): 
PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary 
```

Dieser Fehler kann während der Laufzeit über folgen `dlopen` zurückgeben `IntPtr.Zero` anstelle einer erwarteten Handles.

#### <a name="example-error-resulting-from-a-statically-linked-third-party-dependency-that-does-not-target-64-bit"></a>Beispiel-Fehler, die aus einer statisch verknüpften Drittanbieter-Abhängigkeit, die nicht auf 64-Bit-ausgerichtet ist:

```console
Undefined symbols for architecture x86_64:
  "_LibraryFunction", referenced from:
     -u command line option
ld: symbol(s) not found for architecture x86_64 
```

Erstellen und erfolgreich ausgeführt werden, aktualisieren Sie diese Abhängigkeiten auf 64-Bit-und kompilieren Sie Ihre app neu.

