---
title: Bearbeiten der Eigenschaften von virtuellen Android-Geräten
description: In diesem Artikel wird erklärt, wie Sie den Android Device Manager zum Bearbeiten der Profileigenschaften eines virtuellen Android-Geräts verwenden.
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 3E33C136-8042-4184-A40C-3200D8CD99CB
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/30/2018
ms.openlocfilehash: ab4e0e79c39d36adcfb29e9659c2c6a924c80470
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73020875"
---
# <a name="editing-android-virtual-device-properties"></a>Bearbeiten der Eigenschaften von virtuellen Android-Geräten

_In diesem Artikel wird erklärt, wie Sie den Android Device Manager zum Bearbeiten der Profileigenschaften eines virtuellen Android-Geräts verwenden._

::: zone pivot="windows"

## <a name="android-device-manager-on-windows"></a>Android Device Manager unter Windows

Der **Android Device Manager** unterstützt das Bearbeiten von individuellen virtuellen Android-Geräteprofileigenschaften. Die Bildschirme **Neues Gerät** und **Device Edit** (Gerät bearbeiten) führen in der ersten Spalte die Eigenschaften des virtuellen Geräts auf. In der zweiten Spalte sind die entsprechenden Werte für jede Eigenschaft enthalten. Dies wird im folgenden Beispiel veranschaulicht: 

[![Beispiel zum Bildschirm „Neues Gerät“](device-properties-images/win/01-new-device-editor-sml.png)](device-properties-images/win/01-new-device-editor.png#lightbox)

Wenn Sie eine Eigenschaft auswählen, wird rechts eine ausführliche Beschreibung dieser Eigenschaft angezeigt. Sie können die *Hardwareprofileigenschaften* und die *AVD-Eigenschaften* bearbeiten. Hardwareprofileigenschaften (z.B. `hw.ramSize` und `hw.accelerometer`) beschreiben die physischen Merkmale des emulierten Geräts. Zu diesen Merkmalen gehören die Bildschirmgröße, die Menge des verfügbaren Arbeitsspeichers und ob ein Beschleunigungsmesser vorhanden ist. AVD-Eigenschaften geben die Vorgänge des virtuellen Android-Geräts bei der Ausführung an. AVD-Eigenschaften können beispielsweise konfiguriert werden, um anzugeben, wie das virtuelle Android-Gerät die Grafikkarte Ihres Entwicklungscomputers für das Rendering verwendet.

Sie können Eigenschaften mithilfe der folgenden Anleitung ändern:

- Aktivieren Sie das Kontrollkästchen auf der rechten Seite einer booleschen Eigenschaft, um diese zu ändern:

    ![Ändern einer booleschen Eigenschaft](device-properties-images/win/02-boolean-value.png)

- Klicken Sie zum Ändern einer *enum*-Eigenschaft (enumerierten Eigenschaft) auf den Dropdownpfeil auf der rechten Seite der Eigenschaft, und wählen Sie einen neuen Wert aus.

    ![Ändern einer enumerierten Eigenschaft](device-properties-images/win/04-enum-value.png)

- Doppelklicken Sie zum Ändern einer Zeichenfolge oder einer ganzzahligen Eigenschaft auf die aktuelle Zeichenfolge oder auf die ganzzahlige Einstellung in der Wertespalte, und geben Sie einen neuen Wert ein.

    ![Ändern einer ganzzahligen Eigenschaft](device-properties-images/win/03-integer-value.png)

::: zone-end
::: zone pivot="macos"

## <a name="android-device-manager-on-macos"></a>Android Device Manager unter macOS

Der **Android Device Manager** unterstützt das Bearbeiten von individuellen virtuellen Android-Geräteprofileigenschaften. Die Bildschirme **Neues Gerät** und **Device Edit** (Gerät bearbeiten) führen in der ersten Spalte die Eigenschaften des virtuellen Geräts auf. In der zweiten Spalte sind die entsprechenden Werte für jede Eigenschaft enthalten. Dies wird im folgenden Beispiel veranschaulicht: 

[![Beispiel zum Bildschirm „Neues Gerät“](device-properties-images/mac/01-new-device-editor-sml.png)](device-properties-images/mac/01-new-device-editor.png#lightbox)

Wenn Sie eine Eigenschaft auswählen, wird rechts eine ausführliche Beschreibung dieser Eigenschaft angezeigt. Sie können die *Hardwareprofileigenschaften* und die *AVD-Eigenschaften* bearbeiten. Hardwareprofileigenschaften (z.B. `hw.ramSize` und `hw.accelerometer`) beschreiben die physischen Merkmale des emulierten Geräts. Zu diesen Merkmalen gehören die Bildschirmgröße, die Menge des verfügbaren Arbeitsspeichers und ob ein Beschleunigungsmesser vorhanden ist. AVD-Eigenschaften geben die Vorgänge des virtuellen Android-Geräts bei der Ausführung an. AVD-Eigenschaften können beispielsweise konfiguriert werden, um anzugeben, wie das virtuelle Android-Gerät die Grafikkarte Ihres Entwicklungscomputers für das Rendering verwendet.

Sie können Eigenschaften mithilfe der folgenden Anleitung ändern:

- Aktivieren Sie das Kontrollkästchen auf der rechten Seite einer booleschen Eigenschaft, um diese zu ändern:

    ![Ändern einer booleschen Eigenschaft](device-properties-images/mac/02-boolean-value.png)

- Klicken Sie zum Ändern einer *enum*-Eigenschaft (enumerierten Eigenschaft) auf das Pulldownmenü auf der rechten Seite der Eigenschaft, und wählen Sie einen neuen Wert aus.

    ![Ändern einer enumerierten Eigenschaft](device-properties-images/mac/04-enum-value.png)

- Doppelklicken Sie zum Ändern einer Zeichenfolge oder einer ganzzahligen Eigenschaft auf die aktuelle Zeichenfolge oder auf die ganzzahlige Einstellung in der Wertespalte, und geben Sie einen neuen Wert ein.

    ![Ändern einer ganzzahligen Eigenschaft](device-properties-images/mac/03-integer-value.png)

::: zone-end

In der folgenden Tabellen werden die Eigenschaften näher erläutert, die in den Bildschirmen **Neues Gerät** und **Device Editor** (Geräte-Editor) aufgeführt sind:

[!include[](~/android/includes/emulator-properties.md)]

Weitere Informationen zu diesen Eigenschaften finden Sie unter [Hardwareprofileigenschaften](https://developer.android.com/studio/run/managing-avds.html#hpproperties).
