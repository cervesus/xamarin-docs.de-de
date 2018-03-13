---
title: CPU-Architekturen
description: "Xamarin.Android unterstützt mehrere CPU-Architekturen, einschließlich der 32-Bit und 64-Bit-Geräten. Dieser Artikel beschreibt, wie Sie eine app an eine oder mehrere Android unterstützt CPU-Architekturen abzielen."
ms.topic: article
ms.prod: xamarin
ms.assetid: D4BC889D-9164-49BB-9B7B-F6C4E4E109F1
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 3df6dc72eaed74ad335596d55db8b1295b16f3c2
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="cpu-architectures"></a>CPU-Architekturen

_Xamarin.Android unterstützt mehrere CPU-Architekturen, einschließlich der 32-Bit und 64-Bit-Geräten. Dieser Artikel beschreibt, wie Sie eine app an eine oder mehrere Android unterstützt CPU-Architekturen abzielen._

## <a name="cpu-architectures-overview"></a>Übersicht über die CPU-Architekturen

Bei der Vorbereitung der Anwendung für Version müssen Sie angeben, welche CPU-plattformarchitekturen Ihre app unterstützt. Ein einzelnes APK kann Computercode enthalten, der mehrere unterschiedliche Architekturen unterstützt. Jede Auflistung von architekturspezifischer Code zugeordnet ist ein *Application Binary Interface* (ABI). Jede ABI definiert, wie diese Computercode für die Interaktion mit Android zur Laufzeit erwartet wird.
Weitere Informationen zur Funktionsweise finden Sie unter [mit mehreren Kernen Geräte &amp; Xamarin.Android](~/android/deploy-test/multicore-devices.md).


## <a name="how-to-specify-supported-architectures"></a>Zum Angeben der unterstützter Architekturen

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Normalerweise explizit wählen Sie eine Architektur (oder -Architekturen) Wenn die app konfiguriert ist, für die **Version**. Wenn Ihre app konfiguriert ist, für **Debuggen**, **freigegebenen Laufzeit** und **Use Fast Deployment** Optionen aktiviert sind, wodurch die explizite Architektur Auswahl deaktiviert.

Doppelklicken Sie in Visual Studio auf **Eigenschaften** unterhalb des Projekts in **Projektmappen-Explorer** , und wählen Sie die **Android Options** Seite. Klicken Sie auf die **Verpackung** Registerkarte, und überprüfen Sie, ob **freigegebenen Laufzeit** deaktiviert ist (dies ausschalten können Sie explizit auswählen, welche ABIs unterstützen). Klicken Sie auf die **erweitert** Registerkarte und klicken Sie unter **erweiterte Eigenschaften**, überprüfen Sie die Architekturen, die Sie unterstützen möchten:

[![Auswählen von Armeabi und Armeabi v7a](cpu-architectures-images/vs/01-abi-selections-sml.png)](cpu-architectures-images/vs/01-abi-selections.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Normalerweise explizit wählen Sie eine Architektur (oder -Architekturen) Wenn die app konfiguriert ist, für die **Version**. Wenn Ihre app konfiguriert ist, für **Debuggen**, **Verwenden von freigegebenen Mono / Runtime** und **für die schnelle Assemblybereitstellung** Optionen aktiviert sind, die zu verhindern, dass explizite Architektur Auswahl.

In Visual Studio für Mac, suchen Sie das Projekt in der **Lösung** aufzufüllen, klicken Sie auf das Symbol "Zahnrad" neben Ihrem Projekt aus, und wählen Sie **Optionen**. In der **Projektoptionen** Dialogfeld klicken Sie auf **Android erstellen**. Klicken Sie auf die **allgemeine** Registerkarte, und überprüfen Sie, ob **Verwenden von freigegebenen Mono / Runtime** deaktiviert ist (dies ausschalten können Sie explizit auswählen, welche ABIs unterstützen). Klicken Sie auf die **erweitert** Registerkarte und klicken Sie unter **ABIs unterstützt**, überprüfen Sie die ABIs für die Architekturen, die Sie unterstützen möchten:

[![Auswählen von Armeabi und Armeabi v7a](cpu-architectures-images/xs/01-abi-selections-sml.png)](cpu-architectures-images/xs/01-abi-selections.png#lightbox)

-----


Xamarin.Android unterstützt die folgenden Architekturen:

-   **Armeabi** &ndash; ARM-basierten CPUs, die mindestens den ARMv5TE-Anweisungssatz unterstützt. Beachten Sie, dass `armeabi` ist nicht threadsicher und sollte nicht für Multi-CPU-Geräte verwendet werden.

-   **Armeabi v7a** &ndash; ARM-basierten CPUs mit Gleitkommaoperationen Hardware und Geräte für mehrere CPU-(SMP). Beachten Sie, dass `armeabi-v7a` Computercode kann nicht ARMv5 Geräten ausgeführt werden.

-   **arm64 v8a** &ndash; basierend auf der 64-Bit-Architektur ARMv8 CPUs.

-   **X86** &ndash; CPUs, die die X86 (oder IA-32) unterstützen Befehlssatz. Diese Anweisungssatz entspricht der Pentium Pro, einschließlich der MMX, SSE- und SSE2, SSE3-Anweisungen.

-   **x86_64** CPUs, die die 64-Bit-X86 unterstützen (auch bezeichnet als *X64* und *AMD64*)-Befehlssatz.

Standardmäßig Xamarin.Android `armeabi-v7a` für **Version** erstellt. Diese Einstellung bietet deutlich bessere Leistung als `armeabi`. Wenn das Ziel einer 64-Bit-ARM-Plattform (z. B. die Nexus-9), wählen Sie `arm64-v8a`. Wenn Sie Ihre app um x X86 bereitstellen Gerät wählen `x86`. Wenn das Zielgerät X86 eine 64-Bit-CPU-Architektur verwendet wird, wählen Sie `x86_64`.

## <a name="targeting-multiple-platforms"></a>Mehrere Zielplattformen

Um mehrere CPU-Architekturen abzielen, können Sie mehrere ABI (auf Kosten der größeren Größe für die APK-Datei) auswählen. Können Sie die **generieren Sie ein Paket (.apk) pro ausgewählten ABI** Option (beschrieben [Festlegen von Paketeigenschaften](~/android/deploy-test/release-prep/index.md#Set_Packaging_Properties)) erstellen Sie eine separate APK für jede unterstützte Architektur.

Sie müssen keine wählen **arm64 v8a** oder **x86_64** für 64-Bit-Geräte; 64-Bit-Unterstützung ist nicht erforderlich, um die Ausführung Ihrer Anwendung auf 64-Bit-Hardware. Z. B. die 64-Bit-ARM-Geräten (z. B. die [Nexus 9](http://www.google.com/nexus/9/)) können für die konfigurierten apps ausführen `armeabi-v7a`. Der wichtigste Vorteil von 64-Bit-Unterstützung aktiviert werden, damit Ihre app mehr Arbeitsspeicher können.

> [!NOTE]
> Unterstützung für 64-Bit-Common Language Runtime ist derzeit eine experimentelle Funktion. Denken Sie daran, dass 64-Bit-Laufzeiten gelten *nicht* erforderlich, um Ihre Anwendung auf 64-Bit-Geräten auszuführen. 

## <a name="additional-information"></a>Zusätzliche Informationen

In einigen Situationen müssen Sie möglicherweise eine separate APK für jede Architektur erstellen (zur Reduzierung der Größe Ihrer APK oder Ihrer app Bibliotheken freigegeben hat, die spezifisch für eine bestimmte CPU-Architektur sind).
Weitere Informationen zu diesem Verfahren finden Sie unter [erstellen ABI-spezifische APKs](~/android/deploy-test/building-apps/abi-specific-apks.md).
