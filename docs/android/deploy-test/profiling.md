---
title: Profilerstellung für Android-Apps
description: In diesem Leitfaden wird erklärt, wie Profilerstellungstools zum Untersuchen der Leistung und Arbeitsspeicherauslastung einer Android-App verwendet werden.
ms.topic: article
ms.prod: xamarin
ms.assetid: 8C823FEE-A6F6-4C31-9EB6-E51407A2FD8E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: 400075a1cbd2303f2ecddb9b1cc9465bbcbde32d
ms.sourcegitcommit: f255aa286bd52e8a80ffa620c2e93c97f069f8ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68680263"
---
# <a name="profiling-android-apps"></a>Profilerstellung für Android-Apps

Bevor Sie Ihre App in einem App Store bereitstellen, ist es wichtig, jegliche Leistungsengpässe, Probleme mit übermäßiger Speicherauslastung oder ineffiziente Verwendung von Netzwerkressourcen zu identifizieren und zu beheben. Zu diesem Zweck stehen zwei Profilerstellungstools zu Verfügung:

-  Xamarin Profiler 
-  Android Profiler in Android Studio

In diesem Leitfaden wird der Xamarin Profiler vorgestellt und ausführliche Informationen zu den ersten Schritten mit dem Android Profiler bereitgestellt.

 
## <a name="xamarin-profiler"></a>Xamarin Profiler

Der Xamarin Profiler ist eine eigenständige Anwendung für die Profilerstellung von Xamarin-Apps innerhalb der IDE, die in Visual Studio und Visual Studio für Mac integriert ist. Weitere Informationen über die Verwendung des Xamarin Profilers finden Sie unter [Xamarin Profiler](~/tools/profiler/index.md).

> [!NOTE]
> Sie benötigen ein Abonnement von [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/compare/), um das Xamarin Profiler-Feature in Visual Studio Enterprise unter Windows oder Visual Studio für Mac freizuschalten.
 
## <a name="android-studio-profiler"></a>Android Studio Profiler

Ab Android Studio 3.0 ist ein Android Profiler-Tool enthalten. Sie können den Android Profiler zum Messen der Leistung einer Xamarin Android-App verwenden, die mit Visual Studio erstellt wurde, ohne dass dazu eine Visual Studio Enterprise-Lizenz erforderlich ist. Allerdings ist der Android Profiler im Gegensatz zum Xamarin Profiler nicht in Visual Studio integriert und kann nur für die Profilerstellung eines APK (Android-Anwendungspaket) verwendet werden, das im voraus erstellt und in den Android Profiler importiert wurde.

### <a name="launching-a-xamarin-android-app-in-android-profiler"></a>Starten einer Xamarin Android-App im Android Profiler

In den folgenden Schritten wird erklärt, wie Sie eine Xamarin Android-Anwendung im Android Profiler-Tool von Android Studio starten. In den folgenden Beispiel-Screenshots wird die Xamarin Forms-App [XamagonXuzzle](https://docs.microsoft.com/samples/xamarin/mobile-samples/liveplayer-xamagonxuzzlelp/) erstellt, und die Profilerstellung wird mit dem Android Profiler durchgeführt:

1.  Deaktivieren Sie die Option **Shared Runtime verwenden** in den Optionen für die Android-Projekterstellung. Damit wird sichergestellt, dass das APK ohne eine Abhängigkeit für die zur Entwicklungszeit freigegebene Mono-Laufzeit erstellt wurde.

    ![Deaktivieren von „Shared Runtime verwenden“](profiling-images/vswin/01-turn-off-shared-runtime.png)

2.  Erstellen Sie die App zum **Debuggen**, und stellen Sie diese für ein physisches Gerät oder einen Emulator bereit. Dadurch wird eine signierte **Debugversion** des Android-Anwendungspakets erstellt.
    Das im **XamagonXuzzle**-Beispiel resultierende APK heißt **com.companyname.XamagonXuzzle-Signed.apk**.

3.  Öffnen Sie den Projektordner, und navigieren Sie zu **bin/Debug**. Suchen Sie in diesem Ordner die **Signed.apk**-Version der App, und kopieren Sie diese in einen einfach zugänglichen Ort (z.B. dem Desktop). Im folgenden Screenshot wird gezeigt, wo sich das APK **com.companyname.XamagonXuzzle-Signed.apk** befindet. Anschließend wird es zum Desktop kopiert:

    [![Speicherort der zum Debuggen signierten APK-Datei](profiling-images/vswin/02-locating-the-debug-apk-sml.png)](profiling-images/vswin/02-locating-the-debug-apk.png#lightbox)

4.  Starten Sie Android Studio, und klicken Sie auf **Profile or debug APK** (APK profilieren oder debuggen):

    ![Starten des Profilers über den Android Studio-Startbildschirm](profiling-images/vswin/03-android-studio.png)

5.  Navigieren Sie im Dialogfeld **Select APK File** (APK-Datei auswählen) zu dem Android-Anwendungspaket, das Sie zuvor erstellt und kopiert haben. Wählen Sie die APK aus, und klicken Sie auf **OK**: 
    
    ![Auswählen des APK im Dialogfeld „APK auswählen“](profiling-images/vswin/04-select-apk-dialog.png)

6.  Android Studio lädt das APK und disassembliert **classes.dex**:

    ![Einrichten des APK](profiling-images/vswin/05-setting-up-the-apk.png)

7.  Nachdem das APK geladen wurde, zeigt Android Studio den folgenden Projektbildschirm für das APK an. Klicken Sie in der linken Strukturansicht mit der rechten Maustaste auf den Namen der App, und wählen Sie **Open Module Settings** (Moduleinstellungen öffnen) aus:

    [![Speicherort des Menüelements „Moduleinstellungen öffnen“](profiling-images/vswin/06-open-module-settings-sml.png)](profiling-images/vswin/06-open-module-settings.png#lightbox)

8.  Navigieren Sie zu **Projekteinstellungen > Module**, wählen Sie den **-Signed**-Knoten der App aus, und klicken Sie dann auf **&lt;No SDK&gt;** (Kein SDK):

    [![Navigieren zur Einstellung für das SDK](profiling-images/vswin/07-project-settings-modules-sml.png)](profiling-images/vswin/07-project-settings-modules.png#lightbox)

9.  Wählen Sie im Pulldownmenü **Module SDK** (Modul SDK) die Ebene des Android SDK aus, das zum Erstellen der App verwendet wurde (in diesem Beispiel wurde die API-Ebene 26 zum Erstellen von **XamagonXuzzle** verwendet):

    [![Festlegen der Ebene des SDK für das Projekt](profiling-images/vswin/08-project-sdk-level-sml.png)](profiling-images/vswin/08-project-sdk-level.png#lightbox)

    Klicken Sie auf **Anwenden** und **OK**, um diese Einstellung zu speichern.

10. Starten Sie den Profiler über das Symbol auf der Symbolleiste:

    [![Position des Profiler-Symbols auf der Symbolleiste](profiling-images/vswin/09-launch-profiler-sml.png)](profiling-images/vswin/09-launch-profiler.png#lightbox)

11. Wählen Sie das Bereitstellungsziel für die Ausführung/Profilerstellung der App aus, und klicken Sie auf **OK**. Das Bereitstellungsziel kann ein physisches Gerät oder ein virtuelles Gerät sein, das auf einem Emulator ausgeführt wird. In diesem Beispiel wird ein Nexus 5X-Gerät verwendet:

    ![Auswählen des Bereitstellungsziels](profiling-images/vswin/10-select-deployment-target.png)

12. Nachdem der Profiler gestartet wurde, dauert es einige Sekunden, bis eine Verbindung zwischen dem Bereitstellungsgerät und dem App-Prozess hergestellt wird. Während der Installation des APK meldet der Android Profiler **No connected devices** (Keine verbundenen Geräte) und **No debuggable processes** (Keine debugfähige Prozesse).

    [![Der Profiler installiert das APK](profiling-images/vswin/11-no-connected-devices-sml.png)](profiling-images/vswin/11-no-connected-devices.png#lightbox)

13. Nach einigen Sekunden schließt der Android Profiler die Installation des APK ab, startet das APK und meldet den Gerätenamen und den Namen des App-Prozesses, der profiliert wird (in diesem Beispiel **LGE Nexus 5X** und **com.companyname.XamagonXuzzle**):

    [![Das Fenster des Profilers nach dem Start](profiling-images/vswin/12-profiler-starts-sml.png)](profiling-images/vswin/12-profiler-starts.png#lightbox)

14. Nachdem das Gerät und der debugfähige Prozess identifiziert wurden, beginnt der Android Profiler mit der Profilerstellung für die App:

    [![Anzeige des Profilers für die ausgeführte App](profiling-images/vswin/13-profiler-running-sml.png)](profiling-images/vswin/13-profiler-running.png#lightbox)

15. Wenn Sie in **XamagonXuzzle** auf die Schaltfläche **RANDOMIZE** (Zufällig) tippen (wodurch Kacheln zufällig neu angeordnet werden), sehen Sie, dass die CPU-Auslastung während dem zufälligen Intervall der App steigt:

    [![CPU-Auslastung, wenn auf die Schaltfläche „RANDOMIZE“ getippt wird](profiling-images/vswin/14-tap-randomize-sml.png)](profiling-images/vswin/14-tap-randomize.png#lightbox)


### <a name="using-the-android-profiler"></a>Verwenden des Android Profilers

Ausführliche Informationen zur Verwendung des Android Profilers finden Sie in der [Android Studio-Dokumentation](https://developer.android.com/studio/profile/android-profiler.html).
Die folgenden Themen sind Interessant für Xamarin Android-Entwickler:

-   [CPU-Profiler:](https://developer.android.com/studio/profile/cpu-profiler.html) Überprüfen der CPU-Auslastung und Threadaktivitäten der App in Echtzeit.

-   [Arbeitsspeicherprofiler:](https://developer.android.com/studio/profile/memory-profiler.html) Zeigt einen Graph der Arbeitsspeicherauslastung der App in Echtzeit an und enthält eine Schaltfläche zum Aufzeichnen der Speicherbelegungen zur Analyse.

-   [Netzwerkprofiler:](https://developer.android.com/studio/profile/network-profiler.html) Zeigt Netzwerkaktivitäten von Daten in Echtzeit an, die durch die App gesendet und empfangen wurden.
