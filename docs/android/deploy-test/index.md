---
title: Testen, Optimieren und Bereitstellen von Xamarin.Android-Apps
description: Dieser Abschnitt umfasst Leitfäden, in denen erklärt wird, wie Sie eine Anwendung testen, ihre Leistung optimieren, sie auf die Veröffentlichung vorbereiten, sie mit einem Zertifikat signieren und in einem App-Store veröffentlichen.
ms.prod: xamarin
ms.assetid: 568C0B85-EFF3-AF6F-5605-95055193D367
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 4c9ab8c14db131427329cef51e7b74e982a1c7b8
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103685"
---
# <a name="deployment-and-testing"></a>Bereitstellung und Testen

Dieser Abschnitt umfasst Leitfäden, in denen erklärt wird, wie Sie eine Anwendung testen, ihre Leistung optimieren, sie auf die Veröffentlichung vorbereiten, sie mit einem Zertifikat signieren und in einem App Store veröffentlichen.


##  <a name="application-package-sizesapp-package-sizemd"></a>[Application Package Sizes (Anwendungspaketgrößen)](app-package-size.md)

In diesem Artikel werden die Bestandteile eines Xamarin.Android-Anwendungspakets sowie die zugehörigen Strategien untersucht, die für die effiziente Paketbereitstellung während der Debug- und Releasephasen bei der Entwicklung verwendet werden können.

##  <a name="building-appsbuilding-appsindexmd"></a>[Building Apps (Erstellen von Apps)](building-apps/index.md)

In diesem Abschnitt wird beschrieben, wie der Erstellungsprozess funktioniert und wie ABI-spezifische APKs erstellt werden.

##  <a name="command-line-emulatorcommand-line-emulatormd"></a>[Command Line Emulator (Befehlszeilenemulator)](command-line-emulator.md)

In diesem Artikel wird das Starten des Emulators über die Befehlszeile kurz erklärt.

## <a name="debuggingandroiddeploy-testdebuggingindexmd"></a>[Debuggen](~/android/deploy-test/debugging/index.md)

Die Leitfäden in diesem Abschnitt sollen Sie beim Debuggen Ihrer App mithilfe von Android-Emulators, physischen Android-Geräten und dem Debugprotokoll unterstützen.

##  <a name="setting-the-debuggable-attributeandroiddeploy-testdebuggable-attributemd"></a>[Festlegen des Debuggable-Attributs](~/android/deploy-test/debuggable-attribute.md)

In diesem Artikel wird erläutert, wie das Attribut „Debuggable“ festgelegt wird, damit Tools wie `adb` mit der JVM kommunizieren können.

##  <a name="environmentenvironmentmd"></a>[Umgebung](environment.md)

Dieser Artikel beschreibt die Xamarin.Android-Ausführungsumgebung sowie die Android-Systemeigenschaften, die die Programmausführung beeinflussen.

##  <a name="gdbgdbmd"></a>[GDB](gdb.md)

In diesem Artikel wird erklärt, wie `gdb` für das Debuggen einer Xamarin.Android-Anwendung verwendet wird.

##  <a name="installing-a-system-appinstall-system-appmd"></a>[Installing a System App (Installieren einer System-App)](install-system-app.md)

Dieser Leitfaden erläutert, wie eine Xamarin.Android-App als Systemanwendung auf einem Android-Gerät oder als Teil eines benutzerdefinierten ROM-Speichers installiert wird.

##  <a name="linking-on-androidlinkermd"></a>[Linking on Android (Verknüpfung unter Android)](linker.md)

In diesem Artikel wir der von Xamarin.Android verwendete Verknüpfungsprozess zur Verminderung der letztendlichen Größe einer Anwendung erläutert. Es werden die unterschiedlichen Verknüpfungsstufen beschrieben, die ausgeführt werden können, und es werden Empfehlungen sowie Tipps zur Problembehebung bereitgestellt, um Fehler zu minimieren, die von der Verwendung des Linkers ausgingen.

## <a name="xamarinandroid-performanceandroiddeploy-testperformancemd"></a>[Xamarin.Android-Leistung](~/android/deploy-test/performance.md)

Es gibt viele Techniken zum Verbessern der Leistung von Anwendungen, die mit Xamarin.Android erstellt wurden. Wenn Sie diese Methoden kombinieren, können Sie die CPU-Auslastung und die Speichermenge, die von einer Anwendung verwendet werden, erheblich reduzieren.

## <a name="profiling-android-appsandroiddeploy-testprofilingmd"></a>[Profiling Android Apps (Profilerstellung für Android-Apps)](~/android/deploy-test/profiling.md)

In diesem Leitfaden wird erklärt, wie Profilerstellungstools zum Untersuchen der Leistung und Arbeitsspeicherauslastung einer Android-App verwendet werden.


## <a name="preparing-an-application-for-releaseandroiddeploy-testrelease-prepindexmd"></a>[Preparing an Application for Release (Vorbereiten einer Anwendung auf die Veröffentlichung)](~/android/deploy-test/release-prep/index.md)

Nachdem eine Anwendung codiert und getestet wurde, ist es erforderlich, ein Paket zur Verteilung vorzubereiten. Die erste Aufgabe bei der Vorbereitung dieses Pakets besteht darin, die Anwendung zur Veröffentlichung zu erstellen, was hauptsächlich mit dem Festlegen einiger Anwendungsattribute verbunden ist.

## <a name="signing-the-android-application-packageandroiddeploy-testsigningindexmd"></a>[Signieren des Android-Anwendungspakets](~/android/deploy-test/signing/index.md)

Erfahren Sie, wie Sie eine Android-Signierungsidentität, ein neues Signaturzertifikat für Android-Anwendungen erstellen und die Anwendung mit dem Signaturzertifikat signieren. Außerdem wird in diesem Artikel erläutert, wie Sie die App auf einen Datenträger für eine *Ad-Hoc*-Verteilung exportieren. Das resultierende APK kann ohne Verwendung eines App Stores auf Android-Geräten quergeladen werden.

## <a name="publishing-an-applicationandroiddeploy-testpublishingindexmd"></a>[Veröffentlichen einer Anwendung](~/android/deploy-test/publishing/index.md)

In dieser Artikelreihe werden die Schritte erläutert, die zur öffentlichen Verteilung einer mit Xamarin.Android erstellten Anwendung nötig sind. Sie können Kanäle wie E-Mails, private Webserver, Google Play oder den Amazon App Store für Android zur Verteilung nutzen.
