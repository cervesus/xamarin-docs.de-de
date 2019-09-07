---
title: Beheben von Bibliotheksinstallationsfehlern
description: In einigen Fällen erhalten Sie möglicherweise Fehler bei der Installation von Android-Unterstützungs Bibliotheken. Dieses Handbuch enthält Problem Umgehungen für einige häufige Fehler.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2AE68ACE-8496-445D-BF17-5E4097D4AE35
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/14/2018
ms.openlocfilehash: 8107a26e090aa920d71146d5f2af8b8365697d6b
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757208"
---
# <a name="resolving-library-installation-errors"></a>Beheben von Bibliotheksinstallationsfehlern

_In einigen Fällen erhalten Sie möglicherweise Fehler bei der Installation von Android-Unterstützungs Bibliotheken. Dieses Handbuch enthält Problem Umgehungen für einige häufige Fehler._

## <a name="overview"></a>Übersicht

Beim Erstellen eines xamarin. Android-App-Projekts erhalten Sie möglicherweise Buildfehler, wenn Visual Studio oder Visual Studio für Mac versuchen, Abhängigkeits Bibliotheken herunterzuladen und zu installieren. Viele dieser Fehler werden durch Netzwerkkonnektivitätsprobleme, Datei Beschädigungen oder Versions Probleme verursacht. In dieser Anleitung werden die am häufigsten auftretenden Installationsfehler für die Unterstützungs Bibliothek beschrieben, und es werden die Schritte zum Umgehen dieser Probleme und zum erneuten aufbauen des App-Projekts beschrieben. 

## <a name="errors-while-downloading-m2repository"></a>Fehler beim Herunterladen von m2Repository

Möglicherweise werden **m2repository** -Fehler angezeigt, wenn Sie auf ein nuget-Paket der Android-Unterstützungs Bibliotheken oder Google Play Dienste verweisen. Die Fehlermeldung sieht etwa wie die folgende aus:

```shell
Download failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\22.2.1\content directory.
```

Dieses Beispiel gilt für **Android\_m2repository\_R16**, aber möglicherweise wird die gleiche Fehlermeldung für eine andere Version angezeigt, z. b. **\_Android m2repository\_R18** oder **Android\_ . m2repository\_R25**. 

### <a name="automatic-recovery-from-m2repository-errors"></a>Automatische Wiederherstellung nach m2repository-Fehlern 

Häufig kann dieses Problem behoben werden, indem die problematische Bibliothek und die Neuerstellung gemäß den folgenden Schritten gelöscht werden: 

1. Navigieren Sie zum Verzeichnis der Unterstützungs Bibliothek auf Ihrem Computer:

    - Unter Windows befinden sich Support Bibliotheken unter **C\\:\\Benutzer\\_Benutzername_\\APPDATA\\local xamarin**. 

    - Auf Mac OS X befinden sich Support Bibliotheken unter **/Users/_username_/.local/share/Xamarin**. 

2. Suchen Sie die Bibliothek und den Versions Ordner, der der Fehlermeldung entspricht. Beispielsweise befindet sich der Ordner Bibliothek und Version für die oben aufgeführte Fehlermeldung unter **Android. Support.\\V4 22.2.1**:

    [![Beispiel für einen Ordner Speicherort für die 22.2.1-Unterstützungs Bibliothek](resolving-library-installation-errors-images/01-example-location.png)](resolving-library-installation-errors-images/01-example-location.png#lightbox)

3. Löschen Sie den Inhalt des Versions Ordners. Stellen Sie sicher, dass Sie die **ZIP** -Datei sowie die **Inhalte** und **eingebetteten** Unterverzeichnisse in diesem Ordner entfernen. In der obigen Beispiel Fehlermeldung werden die Dateien und Unterverzeichnisse, die in diesem Screenshot angezeigt werden ("**Content**", " **Embedded**" und " **android_m2repository_r16. zip**") gelöscht:

    [![Beispiel Inhalt des Ordners "22.2.1 Support Library"](resolving-library-installation-errors-images/02-example-folder-vs.png)](resolving-library-installation-errors-images/02-example-folder-vs.png#lightbox)

   Beachten Sie, dass es wichtig ist, den *gesamten* Inhalt dieses Ordners zu löschen. Obwohl dieser Ordner anfänglich die "fehlende" **Android\_m2repository\_R16. zip** -Datei enthalten kann, wurde diese Datei möglicherweise teilweise heruntergeladen oder beschädigt.

4. Wenn Sie das &ndash; Projekt neu erstellen, bewirkt dies, dass der Buildprozess die fehlende Bibliothek erneut herunterlädt.

In den meisten Fällen wird der Buildfehler durch diese Schritte gelöst, und Sie können den Vorgang fortsetzen. Wenn durch das Löschen dieser Bibliothek der Buildfehler nicht behoben wird, müssen Sie die Datei **Android\_m2repository\_r_nn_. zip** manuell herunterladen und installieren, wie im nächsten Abschnitt beschrieben. 

### <a name="manually-downloading-m2repository"></a>Manuelles Herunterladen m2repository

Wenn Sie die oben beschriebenen automatischen Wiederherstellungsschritte verwendet haben und weiterhin Buildfehler vorliegen, können Sie die **Datei\_Android\_m2repository r_nn_. zip** (über einen Webbrowser) manuell herunterladen und gemäß den folgenden Schritten installieren. . Diese Vorgehensweise ist auch dann nützlich, wenn Sie nicht über Internet Zugriff auf dem Entwicklungs Computer verfügen, aber Sie können das Archiv mithilfe eines anderen Computers herunterladen. 

1. Laden Sie **die\_Android\_m2repository r_nn_. zip** -Datei herunter, die den &ndash; Links für die Fehlermeldung entspricht (zusammen mit dem entsprechenden MD5-Hash der einzelnen Link-URLs):

    - [android\_m2repository\_r33.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r33.zip) &ndash; 5FB756A25962361D17BBE99C3B3FCC44

    - [Android\_m2repository\_R32. zip](https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip) &ndash; F16A3455987DBAE5783F058F19F7FCDF

    - [Android\_m2repository\_R31. zip](https://dl-ssl.google.com/android/repository/android_m2repository_r31.zip) &ndash; 99a8907ce2324316e754a95e4c2d786e

    - [android\_m2repository\_r30.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r30.zip) &ndash; 05AD180B8BDC7C21D6BCB94DDE7F2C8F

    - [Android\_m2repository\_R29. zip](https://dl-ssl.google.com/android/repository/android_m2repository_r29.zip) &ndash; 2a3a8a6d6826ef6cc653030e7d695c41

    - [android\_m2repository\_r28.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r28.zip) &ndash; 17BE247580748F1EDB72E9F374AA0223

    - [Android\_m2repository\_R27. zip](https://dl-ssl.google.com/android/repository/android_m2repository_r27.zip) &ndash; C9FD4FCD69D7D12B1D9DF076B7BE4E1C

    - [Android\_m2repository\_R26. zip](https://dl-ssl.google.com/android/repository/android_m2repository_r26.zip) &ndash; 8157fc1c311bb36420c1d8992af54a4d

    - [android\_m2repository\_r25.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip) &ndash; 0B3F1796C97C707339FB13AE8507AF50

    - [android\_m2repository\_r24.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r24.zip) &ndash; 8E3C9EC713781EDFE1EFBC5974136BEA

    - [Android\_m2repository\_R23. zip](https://dl-ssl.google.com/android/repository/android_m2repository_r23.zip) &ndash; D5BB66B3640FD9B9C6362C9DB5AB0FE7

    - [android\_m2repository\_r22.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r22.zip) &ndash; 96659D653BDE0FAEDB818170891F2BB0

    - [Android\_m2repository\_R21. zip](https://dl-ssl.google.com/android/repository/android_m2repository_r21.zip) &ndash; CD3223F2EFE068A26682B9E9C4B6FBB5

    - [Android\_m2repository\_R20. zip](https://dl-ssl.google.com/android/repository/android_m2repository_r20.zip) &ndash; 650e58df02db1a832386fa4a2ent46b1a

    - [android\_m2repository\_r19.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r19.zip) &ndash; 263B062D6EFAA8AEE39E9460B8A5851A

    - [android\_m2repository\_r18.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r18.zip) &ndash; 25947AD38DCB4865ABEB61522FAFDA0E

    - [android\_m2repository\_r17.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r17.zip) &ndash; 49054774F44AE5F35A6BA9D3C117EFD8

    - [android\_m2repository\_r16.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip) &ndash; 0595E577D19D31708195A83087881EE6

    Wenn das **m2repository** Archive in dieser Tabelle nicht angezeigt wird, können Sie die Download-URL erstellen, indem **https://dl-ssl.google.com/android/repository/** Sie dem Namen des herunter zuladenden **m2repository** voranstellen. Verwenden **https://dl-ssl.google.com/android/repository/android\_m2repository\_r10.zip** Sie beispielsweise, um **Android\_ m2repository\_ R10. zip**herunterzuladen.

2. Benennen Sie die Datei in den entsprechenden MD5-Hash der Download-URL um, wie in der obigen Tabelle gezeigt. Wenn Sie z **. b. Android\_m2repository\_R25. zip**heruntergeladen haben, benennen Sie es in **0b3f1796c97c707339fb13ae8507af50. zip**um. Wenn der MD5-Hash für die Download-URL der heruntergeladenen Datei nicht in der Tabelle angezeigt wird, können Sie einen [Online-MD5-Generator](http://www.webconfs.com/online-md5-generator.php) verwenden, um die URL in eine MD5-Hash Zeichenfolge zu konvertieren. 

3. Kopieren Sie die Datei in den Ordner xamarin **Zips** : 

    - Unter Windows befindet sich dieser Ordner im Verzeichnis **C:\\Benutzer\\***Benutzername***\\APPDATA\\\\local xamarin\\Zips**. 

    - Auf Mac OS X befindet sich dieser Ordner unter **/Users/***username***/.local/share/Xamarin/Zips**. 

    Der folgende Screenshot veranschaulicht z. b. das Ergebnis **,\_Wenn\_Android m2repository R16. zip** heruntergeladen und in den MD5-Hash seiner Download-URL unter Windows umbenannt wird:

    [![Beispiel für das "R16. zip"-Repository, das in 0595e577d19d31708195a83087881ee6. ZIP umbenannt wird](resolving-library-installation-errors-images/03-md5-rename-vs.png)](resolving-library-installation-errors-images/03-md5-rename-vs.png#lightbox)

Wenn der Buildfehler durch diesen Vorgang nicht behoben wird, müssen Sie die **Datei\_Android\_m2repository r_nn_. zip** manuell herunterladen, die ZIP-Datei entpacken und ihren Inhalt installieren, wie im nächsten Abschnitt beschrieben. 

### <a name="manually-downloading-and-installing-m2repository-files"></a>Manuelles Herunterladen und Installieren von m2repository-Dateien

Das vollständig manuelle Verfahren für die Wiederherstellung nach **m2repository** -Fehlern umfasst das Herunterladen der **Android\_m2repository\_r_nn_. zip** -Datei (über einen Webbrowser), das Entpacken und Kopieren des Inhalts in die Unterstützung Bibliotheksverzeichnis auf Ihrem Computer. Im folgenden Beispiel wird die folgende Fehlermeldung wieder hergestellt: 

```shell
Unzipping failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\23.1.1\content directory.
```

Führen Sie die folgenden Schritte aus, um **m2repository** herunterzuladen und den Inhalt zu installieren:

1. Löschen Sie den Inhalt des Bibliotheks Ordners, der der Fehlermeldung entspricht. In der obigen Fehlermeldung würden Sie z. b. den Inhalt von **C:\\Benutzer\\***Benutzername***\\APPDATA\\\\local xamarin\\Android. Support. v4löschen.23.1.1.0\\** . 
    Wie bereits beschrieben, müssen Sie den gesamten Inhalt dieses Verzeichnisses löschen:

    [![Löschen von Content-, Embedded-und android_m2repository-Ordnern aus dem Ordner 23.1.1.0](resolving-library-installation-errors-images/04-delete-contents-vs.png)](resolving-library-installation-errors-images/04-delete-contents-vs.png#lightbox)

2. Laden Sie **die\_Android\_m2repository r_nn_. zip** -Datei von Google herunter, die der Fehlermeldung entspricht (Weitere Links finden Sie in der Tabelle im vorherigen Abschnitt).

3. Extrahieren Sie dieses **ZIP** -Archiv an einen beliebigen Speicherort (z. b. den Desktop). Dadurch sollte ein Verzeichnis erstellt werden, das dem Namen des **ZIP** -Archivs entspricht. In diesem Verzeichnis sollten Sie ein Unterverzeichnis mit dem Namen **m2repository**finden: 

    [![Ordner "m2repository" im extrahierten ZIP-Archiv gefunden.](resolving-library-installation-errors-images/05-m2repository-vs.png)](resolving-library-installation-errors-images/05-m2repository-vs.png#lightbox)

4. Erstellen Sie in dem Verzeichnis mit Versions Angabe, das Sie in Schritt 1 gelöscht haben, den **Inhalt** und die **eingebetteten** Unterverzeichnisse neu. Der folgende Screenshot veranschaulicht beispielsweise, wie **Inhalt** und **eingebettete** Unterverzeichnisse im Ordner **23.1.1.0** für **Android\_m2repository\_R25. zip**erstellt werden: 

    [![Erstellen von Inhalt und eingebetteten Ordnern im Ordner "23.1.1.0"](resolving-library-installation-errors-images/06-recreate-folders-vs.png)](resolving-library-installation-errors-images/06-recreate-folders-vs.png#lightbox)

5. Kopieren Sie **m2repository** aus der extrahierten **ZIP** -Ausgabe in das **Inhalts** Verzeichnis, das Sie im vorherigen Schritt erstellt haben: 

    [![Screenshot von m2repository, der in den Ordner 23.1.1.0/Content kopiert wird](resolving-library-installation-errors-images/07-copied-m2repository-vs.png)](resolving-library-installation-errors-images/07-copied-m2repository-vs.png#lightbox)

6. Navigieren Sie im extrahierten **ZIP** -Verzeichnis zu **m2repository\\com\\Android\\Support\\Support-V4** , und öffnen Sie den Ordner, der der oben erstellten Versionsnummer entspricht (in diesem Beispiel: **23.1.1**):

    [![Beispiel für eine Auflistung von Dateien, die im Ordner "Support-V4/23.1.1" enthalten sind](resolving-library-installation-errors-images/08-zip-contents-vs.png)](resolving-library-installation-errors-images/08-zip-contents-vs.png#lightbox)

7. Kopieren Sie alle Dateien in diesem Ordner in das **eingebettete** Verzeichnis, das Sie in Schritt 4 erstellt haben:

    [![Beispiel für Dateien, die in den Ordner "23.1.1.0/Embedded" kopiert werden](resolving-library-installation-errors-images/09-copied-vs.png)](resolving-library-installation-errors-images/09-copied-vs.png#lightbox)

8. Überprüfen Sie, ob alle Dateien kopiert wurden. Das **eingebettete** Verzeichnis sollte nun Dateien wie " **. jar**", " **. Aar**" und " **. POM**" enthalten.

9. Entzippen Sie den Inhalt aller extrahierten **. Aar** -Dateien in das **eingebettete** Verzeichnis. Fügen Sie unter Windows eine **ZIP** -Erweiterung an die **. Aar** -Datei an, öffnen Sie Sie, und kopieren Sie den Inhalt in das **eingebettete** Verzeichnis.
    Entzippen Sie unter macOS die **. Aar** -Datei mithilfe des **Entzippen Sie** -Befehls im Terminal (z. b. **Entzippen Sie File. Aar**).

An diesem Punkt haben Sie die fehlenden Komponenten manuell installiert, und das Projekt sollte ohne Fehler erstellt werden. Wenn dies nicht der Wert ist, vergewissern Sie sich, dass Sie die **m2repository** **. zip** -Archiv Version heruntergeladen haben, die exakt der Version in der Fehlermeldung entspricht, und überprüfen Sie, ob Sie Ihren Inhalt an den richtigen Speicherorten installiert haben, wie in den obigen Schritten beschrieben 

## <a name="summary"></a>Zusammenfassung 

In diesem Artikel wurde erläutert, wie Sie nach häufigen Fehlern wiederherstellen, die während des automatischen Downloads und der Installation von Abhängigkeits Bibliotheken stattfinden können. Es wurde beschrieben, wie die problematische Bibliothek gelöscht und das Projekt neu erstellt werden kann, um die Bibliothek erneut herunterzuladen und erneut zu installieren. Es wurde beschrieben, wie Sie die Bibliothek herunterladen und im Ordner **Zips** installieren. Außerdem wurde ein ausführlicheres Verfahren zum manuellen herunterladen und Installieren der erforderlichen Dateien als Möglichkeit zum Umgehen von Problemen beschrieben, die nicht über automatische Methoden aufgelöst werden können. 
