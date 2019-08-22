---
title: Anwendungspaketgröße
description: In diesem Artikel werden die Bestandteile eines Xamarin.Android-Anwendungspakets sowie die zugehörigen Strategien untersucht, die für die effiziente Paketbereitstellung während der Debug- und Releasephasen bei der Entwicklung verwendet werden können.
ms.prod: xamarin
ms.assetid: 8D70CDDD-3D3C-9949-8045-AB8F93D18E74
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/05/2018
ms.openlocfilehash: 9bff233b5507e3456ba3620315bd967d0ac7018d
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69525782"
---
# <a name="application-package-size"></a>Anwendungspaketgröße

_In diesem Artikel werden die Bestandteile eines Xamarin.Android-Anwendungspakets sowie die zugehörigen Strategien untersucht, die für die effiziente Paketbereitstellung während der Debug- und Releasephasen bei der Entwicklung verwendet werden können._


## <a name="overview"></a>Übersicht

Xamarin.Android verwendet eine Vielzahl von Mechanismen, um die Paketgröße zu minimieren und gleichzeitig einen effizienten Debug- und Releasebereitstellungsprozess aufrechtzuerhalten. In diesem Artikel befassen wir uns mit dem Workflow zur Release- und Debugbereitstellung von Xamarin.Android und wie die Xamarin.Android-Plattform sicherstellt, dass wir kleine Anwendungspakete erstellen und veröffentlichen.


## <a name="release-packages"></a>Releasepakete

Um eine vollständig in sich geschlossene Anwendung ausliefern zu können, muss das Paket die Anwendung, die zugehörigen Bibliotheken, den Inhalt, die Mono-Laufzeit und die erforderlichen BCL-Assemblys (Base Class Library) enthalten. Wenn wir zum Beispiel die Standardvorlage „Hello World“ verwenden, sieht der Inhalt eines kompletten Paketbuilds so aus:

[![Paketgröße vor Anwenden des Linkers](app-package-size-images/hello-world-package-size-before-linker.png)](app-package-size-images/hello-world-package-size-before-linker.png#lightbox)

15,8 MB ist eine größere Downloadgröße als von uns gewünscht. Das Problem sind die BCL-Bibliotheken, da sie „mscorlib“, „System“ und „Mono.Android“ enthalten, die viele der notwendigen Komponenten für die Ausführung Ihrer Anwendung bereitstellen. Sie bieten jedoch auch Funktionen, die Sie möglicherweise nicht in Ihrer Anwendung verwenden, sodass es daher ggf. sinnvoller ist, diese Komponenten auszuschließen.

Wenn wir eine Anwendung für die Verteilung erstellen, führen wir einen Prozess aus, der als Verknüpfung bezeichnet wird, der die Anwendung untersucht und jeglichen Code entfernt, der nicht direkt verwendet wird. Dieser Prozess ähnelt der Funktionalität, die [Garbage Collection](~/android/internals/garbage-collection.md) für von Heap zugewiesenen Speicher bereitstellt. Aber anstatt auf Objekte angewendet zu werden, wird die Verknüpfung auf Ihren Code angewendet. Zum Beispiel gibt es in „System.dll“ einen ganzen Namespace für das Senden und Empfangen von E-Mails, aber wenn Ihre Anwendung diese Funktionalität nicht nutzt, verschwendet dieser Code nur Platz. Nachdem der Linker auf die Anwendung „Hello World“ angewendet wurde, sieht unser Paket nun so aus:

[![Paketgröße nach Anwenden des Linkers](app-package-size-images/hello-world-package-size-after-linker.png)](app-package-size-images/hello-world-package-size-after-linker.png#lightbox)

Wie wir sehen, wird dadurch ein erheblicher Teil der BCL entfernt, der nicht verwendet wurde. Beachten Sie, dass die endgültige BCL-Größe davon abhängt, was die Anwendung tatsächlich verwendet. Wenn wir zum Beispiel einen Blick auf eine umfangreichere Beispielanwendung namens „ApiDemo“ werfen, sehen wir, dass die BCL-Komponente größer geworden ist, weil „ApiDemo“ mehr von der BCL verwendet als „Hello World“:

![Paketgröße von „ApiDemo“ nach Anwenden des Linkers](app-package-size-images/api-demo-package-size-after-linker.png)

Wie hier gezeigt, wird Ihr Anwendungspaket in der Regel ca. 2,9 MB größer sein als Ihre Anwendung und deren Abhängigkeiten.


## <a name="debug-packages"></a>Debugpakete

Bei Debugbuilds werden die Dinge etwas anders gehandhabt. Bei einer wiederholten erneuten Bereitstellung auf einem Gerät muss eine Anwendung so schnell wie möglich sein. Daher optimieren wir Debugpakete hinsichtlich Geschwindigkeit der Bereitstellung anstatt Größe.

Android ist beim Kopieren und Installieren eines Pakets relativ langsam, weshalb wir die Paketgröße so klein wie möglich halten möchten. Wie oben erörtert, ist der Linker eine Möglichkeit, die Paketgröße zu minimieren. Das Verknüpfen erfolgt jedoch langsam, und wir möchten generell nur die Teile der Anwendung bereitstellen, die sich seit der letzten Bereitstellung geändert haben. Um dies zu erreichen, trennen wir unsere Anwendung von den Xamarin.Android-Kernkomponenten.

Beim ersten Debuggen auf dem Gerät kopieren wir zwei große Pakete namens *Shared Runtime* und *Shared Platform*. „Shared Runtime“ enthält die Mono-Laufzeit und BCL, während „Shared Platform“ für Android-API-Ebenen spezifische Assemblys enthält:

[![Shared Runtime-Paketgröße](app-package-size-images/shared-runtime-package-size.png)](app-package-size-images/shared-runtime-package-size.png#lightbox)

Das Kopieren dieser Kernkomponenten erfolgt nur einmal, da es ziemlich viel Zeit in Anspruch nimmt, erlaubt aber allen nachfolgenden Anwendungen, die im Debugmodus ausgeführt werden, diese zu nutzen. Schließlich kopieren wir die tatsächliche Anwendung, die klein und schnell ist:

![Die tatsächliche Anwendung ist klein](app-package-size-images/hello-world-debug-application-no-link.png)

### <a name="fast-assembly-deployment"></a>Schnelle Assemblybereitstellung

Die Buildoption *Schnelle Assemblybereitstellung* kann verwendet werden, um die Größe des Debuginstallationspakets weiter zu verringern, indem die Assemblys nicht in das Paket der Anwendung einbezogen werden, die Assemblys nur einmal direkt auf dem Gerät installiert werden und nur Dateien kopiert werden, die seit der letzten Bereitstellung geändert wurden.

Gehen Sie zum Aktivieren von *Schnelle Assemblybereitstellung* folgendermaßen vor:

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Android-Projekt, und wählen Sie **Optionen** aus.

2. Wählen Sie im Dialogfeld **Android-Build** aus:  

    ![Projektoptionen für Android-Build](app-package-size-images/fastdev0.png)

3. Aktivieren Sie die Kontrollkästchen **Freigebenene Mono-Laufzeit** und **Schnelle Assemblybereitstellung**:  

    ![Auf der Registerkarte „Packen“ aktivierte Kontrollkästchen](app-package-size-images/fastdev.png)

4. Klicken Sie auf die Schaltfläche **OK**, um die Änderungen zu speichern und das Dialogfeld „Projektoptionen“ zu schließen.


Wenn die Anwendung das nächste Mal zum Debuggen erstellt wird, werden die Assemblys direkt auf dem Gerät installiert (sofern noch nicht vorhanden), und ein kleineres Anwendungspaket (das die Assemblys nicht enthält) wird auf dem Gerät installiert. Dies verkürzt die Zeit, die benötigt wird, um Änderungen an der Anwendung zum Testen in Betrieb zu nehmen.

Nachdem wir die lange erste Bereitstellung von Shared Runtime und Shared Platform hinter uns gebracht haben, können wir jedes Mal, wenn wir eine Änderung an der Anwendung vornehmen, die neue Version schnell und mühelos bereitstellen, sodass wir über einen schnellen Änderungs-/Bereitstellungs-/Ausführungszyklus verfügen.


## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben wir die Facetten des Packens von Release- und Debugprofilen von Xamarin.Android untersucht. Darüber hinaus haben wir uns mit den Strategien beschäftigt, die die Mono für Android-Plattform verwendet, um eine effiziente Paketbereitstellung während der Debug- und Releasephase der Entwicklung zu ermöglichen.
