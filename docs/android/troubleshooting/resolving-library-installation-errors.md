---
title: Beheben von Bibliotheksinstallationsfehlern
description: In einigen Fällen erhalten Sie möglicherweise Fehlermeldungen bei der Installation von Android-Unterstützungsbibliotheken. In diesem Handbuch werden Problemumgehungen zu einigen häufigen Fehlern beschrieben.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2AE68ACE-8496-445D-BF17-5E4097D4AE35
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/14/2018
ms.openlocfilehash: e99e12aa73374cc8145fad0eb5aad7f3259e0ca2
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "76724299"
---
# <a name="resolving-library-installation-errors"></a>Beheben von Bibliotheksinstallationsfehlern

_In einigen Fällen erhalten Sie möglicherweise Fehlermeldungen bei der Installation von Android-Unterstützungsbibliotheken. In diesem Handbuch werden Problemumgehungen zu einigen häufigen Fehlern beschrieben._

## <a name="overview"></a>Übersicht

Beim Erstellen eines Xamarin.Android-App-Projekts erhalten Sie möglicherweise Buildfehlermeldungen, wenn Visual Studio oder Visual Studio für Mac versuchen, Abhängigkeitsbibliotheken herunterzuladen und zu installieren. Viele dieser Fehler werden durch Netzwerkkonnektivitäts-Probleme, Dateibeschädigungen oder Versionsprobleme verursacht. In diesem Handbuch werden die am häufigsten auftretenden Fehler bei der Installation von Unterstützungsbibliotheken sowie die Schritte zum Umgehen dieser Probleme und zum erneuten Erstellen des App-Projekts beschrieben.

## <a name="errors-while-downloading-m2repository"></a>Fehler beim Herunterladen von m2Repository

Wenn Sie auf ein NuGet-Paket der Android-Unterstützungsbibliotheken oder Google Play-Dienste verweisen, werden möglicherweise **m2repository**-Fehler angezeigt. Die Fehlermeldung sieht etwa wie die folgende aus:

```shell
Download failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\22.2.1\content directory.
```

Dieses Beispiel gilt für **android\_m2repository\_r16**, dieselbe Fehlermeldung kann jedoch für eine andere Version angezeigt werden, z. B. **android\_m2repository\_r18** oder **android\_m2repository\_r25**.

### <a name="automatic-recovery-from-m2repository-errors"></a>Automatische Wiederherstellung bei m2repository-Fehlern

Häufig kann dieses Problem behoben werden, indem die problematische Bibliothek gelöscht und in den folgenden Schritten neu erstellt wird:

1. Navigieren Sie zum Verzeichnis der Unterstützungsbibliothek auf Ihrem Computer:

    - Unter Windows befinden sich Unterstützungsbibliotheken unter **C:\\Benutzer\\_Benutzername_\\AppData\\Local\\Xamarin**.

    - Unter Mac OS X befinden sich Unterstützungsbibliotheken unter **/Users/_Benutzername_/.local/share/Xamarin**.

2. Suchen Sie die Bibliothek und den Versionsordner laut Fehlermeldung. Beispielsweise befindet sich der Bibliotheks- und Versionsordner für die oben aufgeführte Fehlermeldung unter **Android.Support.v4\\22.2.1**:

    [![Beispiel-Ordnerspeicherort für die 22.2.1-Unterstützungsbibliothek](resolving-library-installation-errors-images/01-example-location.png)](resolving-library-installation-errors-images/01-example-location.png#lightbox)

3. Löschen Sie den Inhalt des Versionsordners. Stellen Sie sicher, dass Sie die **ZIP**-Datei sowie die Unterverzeichnisse **content** und **embedded** in diesem Ordner entfernen. Für die oben gezeigte Beispielfehlermeldung werden die Dateien und Unterverzeichnisse, die in diesem Screenshot angezeigt werden (**content**, **embedded** und **android_m2repository_r16. zip**) gelöscht:

    [![Beispielinhalt des 22.2.1-Unterstützungsbibliothekordners](resolving-library-installation-errors-images/02-example-folder-vs.png)](resolving-library-installation-errors-images/02-example-folder-vs.png#lightbox)

   Achten Sie unbedingt darauf, den *gesamten* Inhalt dieses Ordners zu löschen. Dieser Ordner enthält zwar möglicherweise zunächst die „fehlende“ Datei **android\_m2repository\_r16.zip**, jedoch wurde diese Datei möglicherweise teilweise heruntergeladen oder beschädigt.

4. Erstellen Sie das Projekt neu &ndash; dies bewirkt, dass der Buildprozess die fehlende Bibliothek erneut herunterlädt.

In den meisten Fällen wird der Buildfehler durch diese Schritte behoben, und Sie können fortfahren. Wenn der Buildfehler nicht durch das Löschen dieser Bibliothek behoben wird, müssen Sie die **android\_m2repository\_r_nn_.zip**-Datei wie im nächsten Abschnitt beschrieben manuell herunterladen und installieren.

### <a name="manually-downloading-m2repository"></a>Manuelles Herunterladen von m2repository

Wenn Sie die oben beschriebenen automatischen Wiederherstellungsschritte ausgeführt haben und weiterhin Buildfehler vorliegen, können Sie die **android\_m2repository\_r_nn_. zip**-Datei (über einen Webbrowser) manuell herunterladen und gemäß den folgenden Schritten installieren. Diese Vorgehensweise ist auch dann nützlich, wenn Sie auf dem Entwicklungscomputer nicht über Internetzugriff verfügen, aber das Archiv mithilfe eines anderen Computers herunterladen können.

1. Laden Sie die **android\_m2repository\_r_nn_. zip**-Datei herunter, die der Fehlermeldung entspricht &ndash; Links werden in der folgenden Liste (zusammen mit dem entsprechenden MD5-Hash der URLs der einzelnen Links) bereitgestellt:

    - [android\_m2repository\_r33.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r33.zip) &ndash; 5FB756A25962361D17BBE99C3B3FCC44

    - [android\_m2repository\_r32.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip) &ndash; F16A3455987DBAE5783F058F19F7FCDF

    - [android\_m2repository\_r31.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r31.zip) &ndash; 99A8907CE2324316E754A95E4C2D786E

    - [android\_m2repository\_r30.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r30.zip) &ndash; 05AD180B8BDC7C21D6BCB94DDE7F2C8F

    - [android\_m2repository\_r29.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r29.zip) &ndash; 2A3A8A6D6826EF6CC653030E7D695C41

    - [android\_m2repository\_r28.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r28.zip) &ndash; 17BE247580748F1EDB72E9F374AA0223

    - [android\_m2repository\_r27.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r27.zip) &ndash; C9FD4FCD69D7D12B1D9DF076B7BE4E1C

    - [android\_m2repository\_r26.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r26.zip) &ndash; 8157FC1C311BB36420C1D8992AF54A4D

    - [android\_m2repository\_r25.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip) &ndash; 0B3F1796C97C707339FB13AE8507AF50

    - [android\_m2repository\_r24.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r24.zip) &ndash; 8E3C9EC713781EDFE1EFBC5974136BEA

    - [android\_m2repository\_r23.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r23.zip) &ndash; D5BB66B3640FD9B9C6362C9DB5AB0FE7

    - [android\_m2repository\_r22.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r22.zip) &ndash; 96659D653BDE0FAEDB818170891F2BB0

    - [android\_m2repository\_r21.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r21.zip) &ndash; CD3223F2EFE068A26682B9E9C4B6FBB5

    - [android\_m2repository\_r20.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r20.zip) &ndash; 650E58DF02DB1A832386FA4A2DE46B1A

    - [android\_m2repository\_r19.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r19.zip) &ndash; 263B062D6EFAA8AEE39E9460B8A5851A

    - [android\_m2repository\_r18.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r18.zip) &ndash; 25947AD38DCB4865ABEB61522FAFDA0E

    - [android\_m2repository\_r17.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r17.zip) &ndash; 49054774F44AE5F35A6BA9D3C117EFD8

    - [android\_m2repository\_r16.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip) &ndash; 0595E577D19D31708195A83087881EE6

    Wenn das **m2repository**-Archiv in dieser Tabelle nicht angezeigt wird, können Sie die Download-URL erstellen, indem Sie `https://dl-ssl.google.com/android/repository/` dem Namen der herunterzuladenden **m2repository**-Datei voranstellen. Verwenden Sie beispielsweise **https://dl-ssl.google.com/android/repository/android\_m2repository\_r10.zip** , um **android\_ m2repository\_ r10.zip** herunterzuladen.

2. Benennen Sie die Datei in den entsprechenden MD5-Hash der Download-URL um, wie in der obigen Tabelle gezeigt. Wenn Sie z. B. **android\_m2repository\_r25.zip** heruntergeladen haben, benennen Sie sie in **0b3f1796c97c707339fb13ae8507af50.zip** um. Wenn der MD5-Hash für die Download-URL der heruntergeladenen Datei nicht in der Tabelle angezeigt wird, können Sie einen [Online-MD5-Generator](http://www.webconfs.com/online-md5-generator.php) verwenden, um die URL in eine MD5-Hashzeichenfolge zu konvertieren.

3. Kopieren Sie die Datei in den Xamarin-Ordner **zips**:

    - Unter Windows befindet sich dieser Ordner unter **C:\\Benutzer\\***Benutzername***\\AppData\\Local\\Xamarin\\zips**.

    - Unter Mac OS X befindet sich dieser Ordner unter **/Users/***Benutzername***/.local/share/Xamarin/zips**.

    Der folgende Screenshot zeigt das Ergebnis, wenn z. B. **android\_m2repository\_r16.zip** heruntergeladen und in den MD5-Hash seiner Download-URL unter Windows umbenannt wird:

    [![Beispiel für das r16.zip-Repository, das in 0595E577D19D31708195A83087881EE6.zip umbenannt wird](resolving-library-installation-errors-images/03-md5-rename-vs.png)](resolving-library-installation-errors-images/03-md5-rename-vs.png#lightbox)

Wenn der Buildfehler nicht durch dieses Verfahren behoben wird, müssen Sie die **android\_m2repository\_r_nn_.zip**-Datei wie im nächsten Abschnitt beschrieben manuell herunterladen, entzippen und installieren.

### <a name="manually-downloading-and-installing-m2repository-files"></a>Manuelles Herunterladen und Installieren von m2repository-Dateien

Das vollständige manuelle Verfahren für die Wiederherstellung nach **m2repository**-Fehlern umfasst das Herunterladen der **android\_m2repository\_r_nn_. zip**-Datei (über einen Webbrowser), ihr Entzippen und das Kopieren des Inhalts in das Verzeichnis der Unterstützungsbibliothek auf Ihrem Computer. Im folgenden Beispiel wird die Wiederherstellung nach folgender Fehlermeldung durchgeführt:

```shell
Unzipping failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\23.1.1\content directory.
```

Führen Sie die folgenden Schritte aus, um **m2repository** herunterzuladen und den Inhalt zu installieren:

1. Löschen Sie den Inhalt des Bibliotheksordners laut Fehlermeldung. Gemäß der obigen Fehlermeldung würden Sie z. B. den Inhalt von **C:\\Benutzer\\***Benutzername***\\AppData\\\\Local\\XamarinAndroid.Support.v4\\23.1.1.0** löschen.
    Wie bereits beschrieben, müssen Sie den gesamten Inhalt dieses Verzeichnisses löschen:

    [![Löschen der Ordner „content“, „embedded“ und „android_m2repository“ aus dem 23.1.1.0-Ordner](resolving-library-installation-errors-images/04-delete-contents-vs.png)](resolving-library-installation-errors-images/04-delete-contents-vs.png#lightbox)

2. Laden Sie die Datei **android\_m2repository\_r_nn_.zip** von Google herunter, die der Fehlermeldung entspricht (Links finden Sie in der Tabelle im vorherigen Abschnitt).

3. Extrahieren Sie dieses **ZIP**-Archiv an einem beliebigen Speicherort (z. B. auf dem Desktop). Dadurch sollte ein Verzeichnis erstellt werden, das dem Namen des **ZIP**-Archivs entspricht. In diesem Verzeichnis sollten Sie ein Unterverzeichnis mit dem Namen **m2repository** finden:

    [![m2repository-Ordner im extrahierten ZIP-Archiv](resolving-library-installation-errors-images/05-m2repository-vs.png)](resolving-library-installation-errors-images/05-m2repository-vs.png#lightbox)

4. Erstellen Sie im Bibliotheksverzeichnis mit Versionsangabe, das Sie in Schritt 1 gelöscht haben, die Unterverzeichnisse **content** und **embedded** neu. Der folgende Screenshot veranschaulicht z. B. das Erstellen der Unterverzeichnisse **content** und **embedded** im Ordner **23.1.1.0** für **android\_m2repository\_r25.zip**:

    [![Erstellen der Ordner „content“ und „embedded“ im Ordner „23.1.1.0“](resolving-library-installation-errors-images/06-recreate-folders-vs.png)](resolving-library-installation-errors-images/06-recreate-folders-vs.png#lightbox)

5. Kopieren Sie **m2repository** aus der extrahierten **ZIP**-Datei in das Verzeichnis **content**, das Sie im vorherigen Schritt erstellt haben:

    [![Screenshot des in den Ordner „23.1.1.0/content“ kopierten Verzeichnisses „m2repository“](resolving-library-installation-errors-images/07-copied-m2repository-vs.png)](resolving-library-installation-errors-images/07-copied-m2repository-vs.png#lightbox)

6. Navigieren Sie im extrahierten **ZIP**-Verzeichnis zu **m2repository\\com\\android\\support\\support-v4**, und öffnen Sie den Ordner, der der oben erstellten Versionsnummer entspricht (in diesem Beispiel **23.1.1**):

    [![Beispiel einer Auflistung der Dateien, die im Ordner „support-v4/23.1.1“ enthalten sind](resolving-library-installation-errors-images/08-zip-contents-vs.png)](resolving-library-installation-errors-images/08-zip-contents-vs.png#lightbox)

7. Kopieren Sie alle Dateien in diesem Ordner in das Verzeichnis **embedded**, das Sie in Schritt 4 erstellt haben:

    [![Beispiel für Dateien, die in den Ordner „23.1.1.0/embedded“ kopiert werden](resolving-library-installation-errors-images/09-copied-vs.png)](resolving-library-installation-errors-images/09-copied-vs.png#lightbox)

8. Überprüfen Sie, ob alle Dateien kopiert wurden. Das **embedded**-Verzeichnis sollte nun Dateien wie **JAR**, **AAR** und **POM** enthalten.

9. Entzippen Sie den Inhalt aller extrahierten **AAR**-Dateien in das **embedded**-Verzeichnis. Versehen Sie unter Windows die **AAR**-Datei mit einer **ZIP**-Erweiterung, öffnen Sie sie, und kopieren Sie den Inhalt in das **embedded**-Verzeichnis.
    Entzippen Sie die **AAR**-Datei unter macOS im Terminal mit dem Befehl **unzip** (z. B. **unzip file.aar**).

An diesem Punkt haben Sie die fehlenden Komponenten manuell installiert, und das Projekt sollte ohne Fehler erstellt werden. Falls nicht, überprüfen Sie, ob Sie die **ZIP**-Archivversion von **m2repository** heruntergeladen haben, die exakt der in der Fehlermeldung erwähnten Version entspricht, und ob Sie den Inhalt an den richtigen Speicherorten installiert haben, wie in den obigen Schritten beschrieben.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die Wiederherstellung nach häufig während des automatischen Downloads und der Installation von Abhängigkeitsbibliotheken auftretenden Fehlern erläutert. Es wurde beschrieben, wie die problematische Bibliothek gelöscht und das Projekt neu erstellt wird, um die Bibliothek erneut herunterzuladen und zu installieren.
Das Herunterladen der Bibliothek und ihre Installation im Ordner **zips** wurden beschrieben. Außerdem wurde ein ausführlicheres Verfahren zum manuellen Herunterladen und Installieren der erforderlichen Dateien als Möglichkeit zum Umgehen von Problemen, die nicht automatisch gelöst werden können, beschrieben.
