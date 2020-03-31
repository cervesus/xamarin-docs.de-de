---
title: Testen, Optimieren und Bereitstellen von Xamarin.Android-Apps
description: Dieser Abschnitt umfasst Leitfäden, in denen erklärt wird, wie Sie eine Anwendung testen, ihre Leistung optimieren, sie auf die Veröffentlichung vorbereiten, sie mit einem Zertifikat signieren und in einem App-Store veröffentlichen.
ms.prod: xamarin
ms.assetid: 568C0B85-EFF3-AF6F-5605-95055193D367
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/25/2018
ms.openlocfilehash: 4b7d3d19ce8766ccdbfc41163fcad44074e832b8
ms.sourcegitcommit: ec112800a76089ab1db66fe24b8bbcc510e067b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "80159761"
---
# <a name="deployment-and-testing"></a>Bereitstellung und Testen

Dieser Abschnitt umfasst Leitfäden, in denen erklärt wird, wie Sie eine Anwendung testen, ihre Leistung optimieren, sie auf die Veröffentlichung vorbereiten, sie mit einem Zertifikat signieren und in einem App Store veröffentlichen.

## <a name="application-package-sizes"></a>[Application Package Sizes (Anwendungspaketgrößen)](app-package-size.md)

In diesem Artikel werden die Bestandteile eines Xamarin.Android-Anwendungspakets sowie die zugehörigen Strategien untersucht, die für die effiziente Paketbereitstellung während der Debug- und Releasephasen bei der Entwicklung verwendet werden können.

## <a name="apply-changes"></a>[Änderungen übernehmen](apply-changes.md)

In dieser Anleitung erhalten Sie Informationen zum Feature „Änderungen übernehmen“, mit dem Sie Änderungen, die an Ressourcen vorgenommen wurden, für Ihre App übernehmen können, ohne sie neu starten zu müssen.

## <a name="building-apps"></a>[Erstellen von Apps](building-apps/index.md)

In diesem Abschnitt wird beschrieben, wie der Erstellungsprozess funktioniert und wie ABI-spezifische APKs erstellt werden.

## <a name="command-line-emulator"></a>[Command Line Emulator (Befehlszeilenemulator)](command-line-emulator.md)

In diesem Artikel wird das Starten des Emulators über die Befehlszeile kurz erklärt.

## <a name="debugging"></a>[Debuggen](~/android/deploy-test/debugging/index.md)

Die Leitfäden in diesem Abschnitt sollen Sie beim Debuggen Ihrer App mithilfe von Android-Emulators, physischen Android-Geräten und dem Debugprotokoll unterstützen.

## <a name="setting-the-debuggable-attribute"></a>[Festlegen des Debuggable-Attributs](~/android/deploy-test/debuggable-attribute.md)

In diesem Artikel wird erläutert, wie das Attribut „Debuggable“ festgelegt wird, damit Tools wie `adb` mit der JVM kommunizieren können.

## <a name="environment"></a>[Umgebung](environment.md)

Dieser Artikel beschreibt die Xamarin.Android-Ausführungsumgebung sowie die Android-Systemeigenschaften, die die Programmausführung beeinflussen.

## <a name="gdb"></a>[GDB](gdb.md)

In diesem Artikel wird erklärt, wie `gdb` für das Debuggen einer Xamarin.Android-Anwendung verwendet wird.

## <a name="installing-a-system-app"></a>[Installing a System App (Installieren einer System-App)](install-system-app.md)

Dieser Leitfaden erläutert, wie eine Xamarin.Android-App als Systemanwendung auf einem Android-Gerät oder als Teil eines benutzerdefinierten ROM-Speichers installiert wird.

## <a name="linking-on-android"></a>[Linking on Android (Verknüpfung unter Android)](linker.md)

In diesem Artikel wir der von Xamarin.Android verwendete Verknüpfungsprozess zur Verminderung der letztendlichen Größe einer Anwendung erläutert. Es werden die unterschiedlichen Verknüpfungsstufen beschrieben, die ausgeführt werden können, und es werden Empfehlungen sowie Tipps zur Problembehebung bereitgestellt, um Fehler zu minimieren, die von der Verwendung des Linkers ausgingen.

## <a name="xamarinandroid-performance"></a>[Xamarin.Android-Leistung](~/android/deploy-test/performance.md)

Es gibt viele Techniken zum Verbessern der Leistung von Anwendungen, die mit Xamarin.Android erstellt wurden. Wenn Sie diese Methoden kombinieren, können Sie die CPU-Auslastung und die Speichermenge, die von einer Anwendung verwendet werden, erheblich reduzieren.

## <a name="profiling-android-apps"></a>[Profiling Android Apps (Profilerstellung für Android-Apps)](~/android/deploy-test/profiling.md)

In diesem Leitfaden wird erklärt, wie Profilerstellungstools zum Untersuchen der Leistung und Arbeitsspeicherauslastung einer Android-App verwendet werden.

## <a name="preparing-an-application-for-release"></a>[Preparing an Application for Release (Vorbereiten einer Anwendung auf die Veröffentlichung)](~/android/deploy-test/release-prep/index.md)

Nachdem eine Anwendung codiert und getestet wurde, ist es erforderlich, ein Paket zur Verteilung vorzubereiten. Die erste Aufgabe bei der Vorbereitung dieses Pakets besteht darin, die Anwendung zur Veröffentlichung zu erstellen, was hauptsächlich mit dem Festlegen einiger Anwendungsattribute verbunden ist.

## <a name="signing-the-android-application-package"></a>[Signieren des Android-Anwendungspakets](~/android/deploy-test/signing/index.md)

Erfahren Sie, wie Sie eine Android-Signierungsidentität, ein neues Signaturzertifikat für Android-Anwendungen erstellen und die Anwendung mit dem Signaturzertifikat signieren. Außerdem wird in diesem Artikel erläutert, wie Sie die App auf einen Datenträger für eine *Ad-Hoc*-Verteilung exportieren. Das resultierende APK kann ohne Verwendung eines App Stores auf Android-Geräten quergeladen werden.

## <a name="publishing-an-application"></a>[Veröffentlichen einer Anwendung](~/android/deploy-test/publishing/index.md)

In dieser Artikelreihe werden die Schritte erläutert, die zur öffentlichen Verteilung einer mit Xamarin.Android erstellten Anwendung nötig sind. Sie können Kanäle wie E-Mails, private Webserver, Google Play oder den Amazon App Store für Android zur Verteilung nutzen.
