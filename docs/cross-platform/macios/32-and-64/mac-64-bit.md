---
title: Aktualisieren von Unified xamarin. Mac-Anwendungen auf 64-Bit
description: In diesem Leitfaden wird beschrieben, wie Sie Ihre xamarin. Mac-Anwendungen auf das 64-Bit-Ziel aktualisieren. Es enthält auch Beispiele für die Arten von Fehlern, die bei dieser Änderung auftreten können.
ms.prod: xamarin
ms.assetid: C3810A74-539C-4FFB-B47F-68CA5F7BCDAD
author: conceptdev
ms.author: crdun
ms.date: 02/22/2018
ms.openlocfilehash: 5539bab417c5efc0064cd1753cb74c7524463ee5
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "70765919"
---
# <a name="updating-xamarinmac-unified-applications-to-64-bit"></a>Aktualisieren von Unified xamarin. Mac-Anwendungen auf 64-Bit

Ab dem 2018 erfordert Apple, dass neue [Mac-App Store-Übermittlungen das 64-Bit-Ziel](https://developer.apple.com/news/?id=06282017a)haben. Apps, die bereits im Mac App Store verfügbar sind, müssen bis zum 2018. Juni auf das 64-Bit-Ziel aktualisiert werden.

Mit der **Datei**  > **neuen** xamarin. Mac-Projektvorlage werden standardmäßig 64-Bit-Anwendungen erstellt, sodass alle zuletzt erstellten Apps bereits 64 Bit-kompatibel sind und keine Änderungen erfordern.

## <a name="targeting-64-bit"></a>Zielversion 64-Bit

1. Öffnen Sie das Fenster " **Projektoptionen** " für Ihre xamarin. Mac-app:

   ![Das Kontextmenü für das Projekt.](mac-64-bit-images/1-contextual_menu-vsmac.png "Das Kontextmenü für das Projekt.")

2. Wählen Sie **Mac Build** aus, und legen Sie **unterstützte Architekturen** auf **x86 \_64**

   [![Festlegen der unterstützten Architekturen auf x86_64](mac-64-bit-images/2-project_options-vsmac.png "Festlegen der unterstützten Architekturen auf x86_64")](mac-64-bit-images/2-project_options-vsmac-large.png#lightbox)

3. Wenn Ihre APP externe Abhängigkeiten aufweist, z. b. systemeigene Verweise oder Bindungs Projekte, aktualisieren Sie Sie auf das 64-Bit-Ziel.

### <a name="errors"></a>Fehler

Wenn Sie die Anwendung zum ersten Mal mit 64-Bit-Unterstützung erstellen oder ausführen, treten möglicherweise Verknüpfungs Fehler von clang-oder Lauf Zeitproblemen auf. Diese Fehler können auftreten, wenn Abhängigkeiten von Drittanbietern – z. b. systemeigene Verweise in ihren xamarin. Mac-oder Bindungs Projekten oder manuell geladene systemweite Frameworks – nicht auf 64-Bit aktualisiert wurden.

> [!TIP]
> Das Umwandeln Ihres Projekts in 64-Bit ist eine wesentliche Änderung und kann indirekt verschiedene Programmierfehler aufdecken. Insbesondere kann die Größe und die Ausrichtung von Datenstrukturen geändert werden, was sich auf p/aufrufen-Signaturen und systemeigenen Code auswirken würde, der in Ihrem Projekt verknüpft ist. Überprüfen Sie ggf. vorhandene Buildwarnungen, und testen Sie die Anwendung im Anschluss, um potenzielle Probleme zu beheben

#### <a name="example-error-resulting-from-a-dynamically-linked-third-party-dependency-that-does-not-target-64-bit"></a>Beispiel Fehler, der sich aus einer dynamisch verknüpften Drittanbieter-Abhängigkeit ergibt, die nicht 64-Bit als Ziel hat:

```console
ld : warning : ignoring file PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary, 
file was built for i386 which is not the architecture being linked (x86_64): 
PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary 
```

Dieser Fehler kann während der Laufzeit auftreten, indem `dlopen` `IntPtr.Zero` anstelle eines erwarteten Handles zurückgibt.

#### <a name="example-error-resulting-from-a-statically-linked-third-party-dependency-that-does-not-target-64-bit"></a>Beispiel Fehler, der sich aus einer statisch verknüpften Drittanbieter-Abhängigkeit ergibt, die nicht 64-Bit als Ziel hat:

```console
Undefined symbols for architecture x86_64:
  "_LibraryFunction", referenced from:
     -u command line option
ld: symbol(s) not found for architecture x86_64 
```

Aktualisieren Sie diese Abhängigkeiten auf 64-Bit, und kompilieren Sie Ihre APP erneut, um Sie erfolgreich zu erstellen und auszuführen.
