---
title: CPU-Architekturen
description: Xamarin.Android unterstützt mehrere CPU-Architekturen, einschließlich 32-Bit und 64-Bit-Geräten. In diesem Artikel wird erläutert, wie Sie eine app eine oder mehrere Android unterstützte CPU-Architekturen abzielen.
ms.prod: xamarin
ms.assetid: D4BC889D-9164-49BB-9B7B-F6C4E4E109F1
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: f2865858552d4445dff95c85767c41849c19cc29
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122269"
---
# <a name="cpu-architectures"></a>CPU-Architekturen

_Xamarin.Android unterstützt mehrere CPU-Architekturen, einschließlich 32-Bit und 64-Bit-Geräten. In diesem Artikel wird erläutert, wie Sie eine app eine oder mehrere Android unterstützte CPU-Architekturen abzielen._

## <a name="cpu-architectures-overview"></a>Übersicht über die CPU-Architekturen

Wenn Sie Ihre app für Release vorbereiten, müssen Sie angeben, welche CPU-plattformarchitekturen Ihre app unterstützt. Ein einzelnes APK kann Computercode enthalten, der mehrere unterschiedliche Architekturen unterstützt. Jede Sammlung von Architektur-spezifischem Code zugeordnet ist ein *Application Binary Interface* (ABI). Jede ABI wird definiert, wie diese Computercode für die Interaktion mit Android zur Laufzeit erwartet wird.
Weitere Informationen zur Funktionsweise finden Sie unter [Geräte mit mehreren Kernen &amp; Xamarin.Android](~/android/deploy-test/multicore-devices.md).


## <a name="how-to-specify-supported-architectures"></a>Gewusst wie: Angeben der unterstützte Architekturen

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Normalerweise explizit wählen Sie eine Architektur (oder -Architekturen) Wenn Ihre app konfiguriert ist, für die **Version**. Wenn Ihre app konfiguriert ist, für die **Debuggen**, **freigegebene Laufzeit** und **Fast Deployment verwenden** Optionen aktiviert sind, wird der explizite Architektur Auswahl deaktivieren.

Visual Studio mit der Maustaste auf das Projekt unter dem **Projektmappen-Explorer** , und wählen Sie **Eigenschaften**. Unter den **Android-Optionen** Seite überprüfen Sie die **Paketeigenschaften** Abschnitt, und überprüfen Sie, ob **freigegebene Laufzeit** deaktiviert ist (Wenn diese Option ausgeschaltet können Sie explizit Wählen Sie die ABIs unterstützen). Klicken Sie auf die **erweitert** Schaltfläche und klicken Sie unter **unterstützte Architekturen**, überprüfen Sie die Architekturen, die Sie unterstützen möchten:

[![Auswählen von Armeabi und Armeabi-v7a](cpu-architectures-images/vs/01-abi-selections-sml.png)](cpu-architectures-images/vs/01-abi-selections.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Normalerweise explizit wählen Sie eine Architektur (oder -Architekturen) Wenn Ihre app konfiguriert ist, für die **Version**. Wenn Ihre app konfiguriert ist, für die **Debuggen**, **verwenden freigegebene Mono-Laufzeit** und **schnelle Assemblybereitstellung** Optionen aktiviert sind, die zu verhindern, dass explizite Architektur Auswahl.

Suchen Sie in Visual Studio für Mac das Projekt in der **Lösung** aufzufüllen, klicken Sie auf das Zahnradsymbol neben Ihrem Projekt, und wählen Sie **Optionen**. In der **Projektoptionen** Dialogfeld klicken Sie auf **Android-Build**. Klicken Sie auf die **allgemeine** Registerkarte, und überprüfen Sie, ob **verwenden freigegebene Mono-Laufzeit** deaktiviert ist (Wenn diese Option ausgeschaltet können Sie explizit auswählen, welche ABIs unterstützen). Klicken Sie auf die **erweitert** Registerkarte und klicken Sie unter **unterstützter ABIs**, überprüfen Sie die ABIs für die Architekturen, die Sie unterstützen möchten:

[![Auswählen von Armeabi und Armeabi-v7a](cpu-architectures-images/xs/01-abi-selections-sml.png)](cpu-architectures-images/xs/01-abi-selections.png#lightbox)

-----


Xamarin.Android unterstützt die folgenden Architekturen:

-   **Armeabi** &ndash; ARM-basierte CPUs, die mindestens den ARMv5TE-Befehlssatz unterstützen. Beachten Sie, dass `armeabi` ist nicht threadsicher und sollte nicht für Multi-CPU-Geräte verwendet werden.

-   **Armeabi-v7a** &ndash; ARM-basierte CPUs mit Hardware-gleitkommavorgänge und mehrere CPU-(SMP) Geräte. Beachten Sie, dass `armeabi-v7a` Computercode nicht auf ARMv5 Geräten ausgeführt werden.

-   **arm64-v8a** &ndash; basieren auf der ARMv8-Architektur von 64-Bit-CPUs.

-   **X86** &ndash; CPUs, die die X86 (oder IA-32) unterstützen-Anweisungssatz. Diesen Befehlssatz entspricht, die von der Pentium Pro, einschließlich der MMX-, SSE-, SSE2 und SSE3-Anweisungen.

-   **x86_64** CPUs, die die 64-Bit-X86 unterstützen (auch bezeichnet als *X64* und *AMD64*)-Anweisungssatz.

Xamarin.Android standardmäßig `armeabi-v7a` für **Version** erstellt. Diese Einstellung bietet eine wesentlich bessere Leistung als `armeabi`. Wenn Sie eine 64-Bit-ARM-Plattform (z. B. die Nexus 9) verwenden möchten, wählen Sie `arm64-v8a`. Wenn Sie Ihre app für x X86 bereitstellen Gerät, und wählen `x86`. Wenn das Zielgerät X86 eine 64-Bit-CPU-Architektur verwendet wird, wählen Sie `x86_64`.

## <a name="targeting-multiple-platforms"></a>Ansteuern mehrerer Plattformen

Um mehrere CPU-Architekturen als Ziel verwenden, können Sie mehr als eine ABI (auf Kosten der größeren Größe für die APK-Datei) auswählen. Können Sie die **ein Paket (.apk) pro ausgewähltem ABI generieren** Option (beschrieben [Festlegen von Paketeigenschaften](~/android/deploy-test/release-prep/index.md#Set_Packaging_Properties)) erstellen Sie ein separates APK für jede unterstützte Architektur.

Sie müssen keine wählen **arm64-v8a** oder **x86_64** 64-Bit-Geräten als Ziel 64-Bit-Unterstützung ist nicht erforderlich, um die Anwendung auf 64-Bit-Hardware auszuführen. Z. B. die 64-Bit-ARM-Geräte (z. B. die [Nexus 9](http://www.google.com/nexus/9/)) können apps, die für konfigurierte ausführen `armeabi-v7a`. Der wichtigste Vorteil von 64-Bit-Unterstützung aktiviert ist und es für Ihre app aus, um mehr Arbeitsspeicher zu beheben.

> [!NOTE]
> 64-Bit-Runtime-Unterstützung ist derzeit ein experimentelles Feature. Denken Sie daran, dass 64-Bit-Laufzeiten sind *nicht* erforderlich, um Ihre Anwendung auf 64-Bit-Geräten auszuführen. 

## <a name="additional-information"></a>Zusätzliche Informationen

In einigen Situationen müssen Sie möglicherweise ein separates APK für jede Architektur zu erstellen (zum Verringern der Größe Ihrer apk-Datei, oder daran, dass Ihre app Bibliotheken freigegeben hat, die spezifisch für eine bestimmte CPU-Architektur sind).
Weitere Informationen zu diesem Verfahren finden Sie unter [erstellen ABI-spezifischer APKs](~/android/deploy-test/building-apps/abi-specific-apks.md).
