---
title: CPU-Architekturen
description: Xamarin. Android unterstützt verschiedene CPU-Architekturen, einschließlich 32-Bit-und 64-Bit-Geräten. In diesem Artikel wird erläutert, wie Sie eine APP für eine oder mehrere mit Android unterstützte CPU-Architekturen ausrichten.
ms.prod: xamarin
ms.assetid: D4BC889D-9164-49BB-9B7B-F6C4E4E109F1
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/30/2019
ms.openlocfilehash: 16e805488969aadb0d0b8aa5c892248b7fa403c9
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69521204"
---
# <a name="cpu-architectures"></a>CPU-Architekturen

_Xamarin. Android unterstützt verschiedene CPU-Architekturen, einschließlich 32-Bit-und 64-Bit-Geräten. In diesem Artikel wird erläutert, wie Sie eine APP für eine oder mehrere mit Android unterstützte CPU-Architekturen ausrichten._

## <a name="cpu-architectures-overview"></a>Übersicht über CPU-Architekturen

Wenn Sie Ihre APP für die Veröffentlichung vorbereiten, müssen Sie angeben, welche Platt Form-CPU-Architekturen von ihrer App unterstützt werden. Ein einzelnes APK kann Computercode enthalten, der mehrere unterschiedliche Architekturen unterstützt. Jede Sammlung von Architektur spezifischem Code ist einer *Anwendungs Binärdatei Schnittstelle* (ABI) zugeordnet. Jede ABI definiert, wie der Computercode zur Laufzeit mit Android interagieren soll.
Weitere Informationen zur Funktionsweise finden Sie unter [Multi-Core Devices &amp; xamarin. Android](~/android/deploy-test/multicore-devices.md).


## <a name="how-to-specify-supported-architectures"></a>Angeben von unterstützten Architekturen

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

In der Regel wählen Sie explizit eine Architektur (oder Architekturen) aus, wenn Ihre APP für die **Freigabe**konfiguriert ist. Wenn Ihre APP für das **Debuggen**konfiguriert ist, werden die Optionen **Shared Runtime verwenden** und **schnelle Bereitstellung verwenden** aktiviert, um die explizite Architektur Auswahl zu deaktivieren.

Klicken Sie in Visual Studio mit der rechten Maustaste unter dem **Projektmappen-Explorer** auf das Projekt, und wählen Sie **Eigenschaften**aus. Überprüfen Sie auf der Seite **Android-Optionen** den Abschnitt **Paketierungs Eigenschaften** , und vergewissern Sie sich, dass **use Shared Runtime** deaktiviert ist (durch das Ausschalten können Sie explizit auswählen, welche ABIS unterstützt werden sollen). Klicken Sie auf die Schaltfläche **erweitert** , und überprüfen Sie unter **unterstützte Architekturen**die Architekturen, die Sie unterstützen möchten:

[![Auswählen von ARMEABI und ARMEABI-v7a](cpu-architectures-images/vs/01-abi-selections-sml.png)](cpu-architectures-images/vs/01-abi-selections.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

In der Regel wählen Sie explizit eine Architektur (oder Architekturen) aus, wenn Ihre APP für die **Freigabe**konfiguriert ist. Wenn Ihre APP für das **Debuggen**konfiguriert ist, werden die **Bereitstellungs** Optionen **Shared mono runtime** und fast Assembly verwenden aktiviert, um eine explizite Architektur Auswahl zu verhindern.

Suchen Sie in Visual Studio für Mac das Projekt im lösungspad, klicken Sie auf das Zahnrad Symbol neben dem Projekt, und wählen Sie **Optionen**aus. Klicken Sie im Dialogfeld **Projektoptionen** auf **Android-Build**. Klicken Sie auf die Registerkarte **Allgemein** , und vergewissern Sie sich, dass frei **gegebene Mono-Laufzeit verwenden** deaktiviert ist (durch das Ausschalten können Sie explizit auswählen, welche ABIS unterstützt werden). Klicken Sie auf die Registerkarte **erweitert** , und überprüfen Sie unter **unterstützte ABIS**den ABIS für die Architekturen, die Sie unterstützen möchten:

[![Auswählen von ARMEABI und ARMEABI-v7a](cpu-architectures-images/xs/01-abi-selections-sml.png)](cpu-architectures-images/xs/01-abi-selections.png#lightbox)

-----


Xamarin.Android unterstützt die folgenden Architekturen:

- **ARMEABI** &ndash; ARM-basierte CPUs, die mindestens den ARMv5TE-Anweisungs Satz unterstützen. Beachten Sie `armeabi` , dass nicht Thread sicher ist und nicht auf Multi-CPU-Geräten verwendet werden darf.

> [!NOTE]
> Ab [xamarin. Android 9,2](https://docs.microsoft.com/xamarin/android/release-notes/9/9.2#removal-of-support-for-armeabi-cpu-architecture) `armeabi` wird nicht mehr unterstützt.

- **ARMEABI-v7a** &ndash; ARM-basierte CPUs mit Hardware-Gleit Komma Vorgängen und SMP-Geräten (Multiple CPU). Beachten Sie `armeabi-v7a` , dass der Computercode nicht auf ARMv5-Geräten ausgeführt werden kann.

- **arm64-V8A** &ndash; CPUs, die auf der 64-Bit-ARMv8-Architektur basieren.

- **x86** &ndash; CPUs, die den x86-(oder IA-32)-Anweisungs Satz unterstützen. Dieser Anweisungs Satz entspricht dem des Pentium Pro, einschließlich der Anweisungen MMX, SSE, SSE2 und SSE3.

- **x86_64** CPUs, die den 64-Bit-(auch als *x64* und *amd64*bezeichnet) Anweisungs Satz unterstützen.

Xamarin. Android ist Standard `armeabi-v7a` mäßig für **Releasebuilds** . Diese Einstellung bietet eine wesentlich bessere Leistung `armeabi`als. Wenn Sie eine 64-Bit-ARM-Plattform (z. b. Nexus 9) als `arm64-v8a`Ziel auswählen, wählen Sie aus. Wenn Sie Ihre APP auf einem x86-Gerät bereitstellen, `x86`wählen Sie aus. Wenn das x86-Ziel Gerät eine 64-Bit-CPU-Architektur `x86_64`verwendet, wählen Sie aus.

## <a name="targeting-multiple-platforms"></a>Mehrere Plattformen als Ziel

Um mehrere CPU-Architekturen als Ziel zu wählen, können Sie mehr als eine ABI auswählen (auf Kosten der größeren APK-Dateigröße). Sie können die Option **ein Paket (. apk) pro ausgewählter ABI generieren** (beschrieben unter Festlegen von Paket [Eigenschaften](~/android/deploy-test/release-prep/index.md#Set_Packaging_Properties)) verwenden, um ein separates APK für jede unterstützte Architektur zu erstellen.

Sie müssen nicht **arm64-V8A** oder **x86_64** auswählen, damit Sie auf 64-Bit-Geräte abzielen. die 64-Bit-Unterstützung ist nicht erforderlich, um Ihre APP auf 64-Bit-Hardware auszuführen. Beispielsweise können 64-Bit-ARM-Geräte (z. b. [Nexus 9](http://www.google.com/nexus/9/)) apps ausführen `armeabi-v7a`, die für konfiguriert sind. Der Hauptvorteil der Aktivierung der 64-Bit-Unterstützung besteht darin, dass Sie es Ihrer APP ermöglichen, mehr Arbeitsspeicher zu beheben.

> [!NOTE]
> Ab August 2018 sind neue Apps erforderlich, um auf API-Ebene 26 abzielen, und ab August 2019 sind Apps [zum Bereitstellen von 64-Bit-Versionen erforderlich](https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html), zusätzlich zu der 32-Bit-Version.

## <a name="additional-information"></a>Zusätzliche Informationen

In einigen Situationen müssen Sie möglicherweise ein separates APK für jede Architektur erstellen (um die Größe Ihres APK zu verringern, oder weil Ihre APP über freigegebene Bibliotheken verfügt, die für eine bestimmte CPU-architekturspezifisch sind).
Weitere Informationen zu diesem Ansatz finden Sie unter [Erstellen von ABI-spezifischen API-Anwendungen](~/android/deploy-test/building-apps/abi-specific-apks.md).
