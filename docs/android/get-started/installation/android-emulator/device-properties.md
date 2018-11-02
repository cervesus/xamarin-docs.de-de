---
title: Bearbeiten der Eigenschaften von virtuellen Android-Geräten
description: In diesem Artikel wird erklärt, wie Sie den Android Device Manager zum Bearbeiten der Profileigenschaften eines virtuellen Android-Geräts verwenden.
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 3E33C136-8042-4184-A40C-3200D8CD99CB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/30/2018
ms.openlocfilehash: 9007157cfd96b82a5781b3bdc3ffb4fe63f4e422
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119617"
---
# <a name="editing-android-virtual-device-properties"></a>Bearbeiten der Eigenschaften von virtuellen Android-Geräten

_In diesem Artikel wird erklärt, wie Sie den Android Device Manager zum Bearbeiten der Profileigenschaften eines virtuellen Android-Geräts verwenden._

::: zone pivot="windows"

## <a name="android-device-manager-on-windows"></a>Android Device Manager unter Windows

Der **Android Device Manager** unterstützt das Bearbeiten von individuellen virtuellen Android-Geräteprofileigenschaften. Die Bildschirme **Neues Gerät** und **Device Edit** (Gerät bearbeiten) führen in der ersten Spalte die Eigenschaften des virtuellen Geräts auf. In der zweiten Spalte sind die entsprechenden Werte für jede Eigenschaft enthalten. Dies wird im folgenden Beispiel veranschaulicht: 

[![Beispiel zum Bildschirm „Neues Gerät“](device-properties-images/win/01-new-device-editor-sml.png)](device-properties-images/win/01-new-device-editor.png#lightbox)

Wenn Sie eine Eigenschaft auswählen, wird rechts eine ausführliche Beschreibung dieser Eigenschaft angezeigt. Sie können die *Hardwareprofileigenschaften* und die *AVD-Eigenschaften* bearbeiten. Hardwareprofileigenschaften (z.B. `hw.ramSize` und `hw.accelerometer`) beschreiben die physischen Merkmale des emulierten Geräts. Zu diesen Merkmalen gehören die Bildschirmgröße, die Menge des verfügbaren Arbeitsspeichers und ob ein Beschleunigungsmesser vorhanden ist. AVD-Eigenschaften geben die Vorgänge des virtuellen Android-Geräts bei der Ausführung an. AVD-Eigenschaften können beispielsweise konfiguriert werden, um anzugeben, wie das virtuelle Android-Gerät die Grafikkarte Ihres Entwicklungscomputers für das Rendering verwendet.

Sie können Eigenschaften mithilfe der folgenden Anleitung ändern:

-   Aktivieren Sie das Kontrollkästchen auf der rechten Seite einer booleschen Eigenschaft, um diese zu ändern:

    ![Ändern einer booleschen Eigenschaft](device-properties-images/win/02-boolean-value.png)

-   Klicken Sie zum Ändern einer *enumerierten* Eigenschaft auf den Dropdownpfeil auf der rechten Seite der Eigenschaft, und wählen Sie einen neuen Wert aus.

    ![Ändern einer enumerierten Eigenschaft](device-properties-images/win/04-enum-value.png)

-   Doppelklicken Sie zum Ändern einer Zeichenfolge oder einer ganzzahligen Eigenschaft auf die aktuelle Zeichenfolge oder auf die ganzzahlige Einstellung in der Wertespalte, und geben Sie einen neuen Wert ein.

    ![Ändern einer ganzzahligen Eigenschaft](device-properties-images/win/03-integer-value.png)

::: zone-end
::: zone pivot="macos"

## <a name="android-device-manager-on-macos"></a>Android Device Manager unter macOS

Der **Android Device Manager** unterstützt das Bearbeiten von individuellen virtuellen Android-Geräteprofileigenschaften. Die Bildschirme **Neues Gerät** und **Device Edit** (Gerät bearbeiten) führen in der ersten Spalte die Eigenschaften des virtuellen Geräts auf. In der zweiten Spalte sind die entsprechenden Werte für jede Eigenschaft enthalten. Dies wird im folgenden Beispiel veranschaulicht: 

[![Beispiel zum Bildschirm „Neues Gerät“](device-properties-images/mac/01-new-device-editor-sml.png)](device-properties-images/mac/01-new-device-editor.png#lightbox)

Wenn Sie eine Eigenschaft auswählen, wird rechts eine ausführliche Beschreibung dieser Eigenschaft angezeigt. Sie können die *Hardwareprofileigenschaften* und die *AVD-Eigenschaften* bearbeiten. Hardwareprofileigenschaften (z.B. `hw.ramSize` und `hw.accelerometer`) beschreiben die physischen Merkmale des emulierten Geräts. Zu diesen Merkmalen gehören die Bildschirmgröße, die Menge des verfügbaren Arbeitsspeichers und ob ein Beschleunigungsmesser vorhanden ist. AVD-Eigenschaften geben die Vorgänge des virtuellen Android-Geräts bei der Ausführung an. AVD-Eigenschaften können beispielsweise konfiguriert werden, um anzugeben, wie das virtuelle Android-Gerät die Grafikkarte Ihres Entwicklungscomputers für das Rendering verwendet.

Sie können Eigenschaften mithilfe der folgenden Anleitung ändern:

-   Aktivieren Sie das Kontrollkästchen auf der rechten Seite einer booleschen Eigenschaft, um diese zu ändern:

    ![Ändern einer booleschen Eigenschaft](device-properties-images/mac/02-boolean-value.png)

-   Klicken Sie zum Ändern einer *enumerierten* Eigenschaft auf das Pulldownmenü auf der rechten Seite der Eigenschaft, und wählen Sie einen neuen Wert aus.

    ![Ändern einer enumerierten Eigenschaft](device-properties-images/mac/04-enum-value.png)

-   Doppelklicken Sie zum Ändern einer Zeichenfolge oder einer ganzzahligen Eigenschaft auf die aktuelle Zeichenfolge oder auf die ganzzahlige Einstellung in der Wertespalte, und geben Sie einen neuen Wert ein.

    ![Ändern einer ganzzahligen Eigenschaft](device-properties-images/mac/03-integer-value.png)

::: zone-end

In der folgenden Tabellen werden die Eigenschaften näher erläutert, die in den Bildschirmen **Neues Gerät** und **Device Editor** (Geräte-Editor) aufgeführt sind:

[!include[](~/android/includes/emulator-properties.md)]

Weitere Informationen zu diesen Eigenschaften finden Sie unter [Hardwareprofileigenschaften](https://developer.android.com/studio/run/managing-avds.html#hpproperties).

