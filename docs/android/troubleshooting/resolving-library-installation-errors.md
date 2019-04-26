---
title: Beheben von Bibliotheksinstallationsfehlern
description: In einigen Fällen erhalten Sie möglicherweise Fehler bei der Installation von Android-Unterstützungsbibliotheken. Dieses Handbuch bietet problemumgehungen für einige häufige Fehler.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2AE68ACE-8496-445D-BF17-5E4097D4AE35
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/14/2018
ms.openlocfilehash: 2ef81cfda92a6497e69f27b0584a97996094b1a4
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61093304"
---
# <a name="resolving-library-installation-errors"></a>Beheben von Bibliotheksinstallationsfehlern

_In einigen Fällen erhalten Sie möglicherweise Fehler bei der Installation von Android-Unterstützungsbibliotheken. Dieses Handbuch bietet problemumgehungen für einige häufige Fehler._

## <a name="overview"></a>Übersicht

Beim Erstellen einer Xamarin.Android-app-Projekt, erhalten Sie möglicherweise Buildfehler auftreten, wenn Visual Studio oder Visual Studio für Mac herunterladen und Installieren des Dependency-Bibliotheken versuchen. Viele dieser Fehler werden durch Probleme mit der Netzwerkkonnektivität, dass die Datei beschädigt oder Versionsprobleme verursacht. Dieses Handbuch wird beschrieben, die am häufigsten verwendeten Support Library-Installationsfehler, und beschreibt, wie diese Probleme umgehen, und erhalten Sie Ihr app-Projekt erneut erstellen. 

 
 
## <a name="errors-while-downloading-m2repository"></a>Fehler beim Herunterladen von m2Repository

Sie sehen möglicherweise **m2repository** Fehler, wenn Sie ein NuGet-Paket der Android-Unterstützungsbibliotheken oder Google Play-Dienste verweisen. Die Fehlermeldung sieht etwa wie die folgende aus:

```shell
Download failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\22.2.1\content directory.
```

Dieses Beispiel gilt für **android\_m2repository\_r16**, aber Sie können diese gleichen Fehlermeldung für eine andere Version wie z. B. sehen **android\_m2repository\_r18**  oder **android\_m2repository\_r25**. 



### <a name="automatic-recovery-from-m2repository-errors"></a>Automatische Wiederherstellung bei m2repository-Fehler 

Dieses Problem kann häufig behoben werden, durch die problematischen Bibliothek löschen und Neuerstellen von gemäß der folgenden Schritte aus: 

1. Navigieren Sie zum Verzeichnis Support-Bibliothek auf Ihrem Computer:

    -   Support-Bibliotheken für Windows, befinden sich unter **C:\\Benutzer\\_Benutzername_\\AppData\\lokalen\\Xamarin**. 

    -   Unter Mac OS X-Unterstützungsbibliotheken befinden sich unter **/Users /_Benutzername_/.local/share/Xamarin**. 

2. Suchen Sie den-Bibliothek und die Version-Ordner, der Fehlermeldung entspricht. Befindet sich z. B. im Ordner "Library und die Version" für die obenstehende Fehlermeldung unter **Android.Support. v4\\22.2.1**:

    [![Beispiel der Speicherort des Ordners für 22.2.1 Unterstützungsbibliothek](resolving-library-installation-errors-images/01-example-location.png)](resolving-library-installation-errors-images/01-example-location.png#lightbox)

3. Löschen Sie den Inhalt der Ordner "Version". Entfernen Sie die **ZIP** Datei als auch die **Inhalt** und **eingebettete** Unterverzeichnisse innerhalb dieses Ordners. Für die Beispiel-Fehlermeldung oben, Dateien und Unterverzeichnisse, die in diesem Screenshot gezeigt (**Inhalt**, **eingebettete**, und **android_m2repository_r16.zip**) sind gelöscht werden:

    [![Beispielinhalt der 22.2.1 unterstützen Bibliotheksordner](resolving-library-installation-errors-images/02-example-folder-vs.png)](resolving-library-installation-errors-images/02-example-folder-vs.png#lightbox)

   Beachten Sie, dass es wichtig ist, löschen Sie die *gesamte* Inhalt dieses Ordners. Obwohl dieser Ordner ursprünglich enthalten möglicherweise die "fehlt" **android\_m2repository\_r16.zip** Datei, diese Datei wurde möglicherweise teilweise heruntergeladene oder ist beschädigt.

4. Das Projekt neu &ndash; zu während des Buildprozesses die fehlende Bibliothek erneut herunterzuladen.

In den meisten Fällen werden diese Schritte den Buildfehler zu beheben und ermöglichen es Ihnen, um den Vorgang fortzusetzen. Wenn der Fehler beim Erstellen dieser Bibliothek löschen nicht behoben wird, müssen Sie manuell herunterladen und installieren Sie die **android\_m2repository\_r_nn_.zip** Datei wie im nächsten Abschnitt beschrieben. 



### <a name="manually-downloading-m2repository"></a>Manuell herunterladen m2repository

Wenn Sie versucht haben, verwenden die obigen Schritte für die automatische Wiederherstellung und weiterhin Buildfehler auftreten haben, können Sie manuell herunterladen der **android\_m2repository\_r_nn_.zip** Datei (mit einem Webbrowser), und installieren Sie es entsprechend um die folgenden Schritte aus. Dieses Verfahren ist auch nützlich, wenn Sie keinen Zugriff auf das Internet auf dem Entwicklungscomputer gespeichert, jedoch Sie das Archiv, einem anderen Computer herunterladen können. 

1.  Herunterladen der **android\_m2repository\_r_nn_.zip** Datei, die die Fehlermeldung entspricht &ndash; Links werden bereitgestellt, in der folgenden Liste (zusammen mit den entsprechenden MD5-Hash des Links -URL):

    -   [android\_m2repository\_r33.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r33.zip) &ndash; 5FB756A25962361D17BBE99C3B3FCC44

    -   [android\_m2repository\_r32.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip) &ndash; F16A3455987DBAE5783F058F19F7FCDF

    -   [android\_m2repository\_r31.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r31.zip) &ndash; 99A8907CE2324316E754A95E4C2D786E

    -   [android\_m2repository\_r30.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r30.zip) &ndash; 05AD180B8BDC7C21D6BCB94DDE7F2C8F

    -   [android\_m2repository\_r29.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r29.zip) &ndash; 2A3A8A6D6826EF6CC653030E7D695C41

    -   [android\_m2repository\_r28.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r28.zip) &ndash; 17BE247580748F1EDB72E9F374AA0223

    -   [android\_m2repository\_r27.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r27.zip) &ndash; C9FD4FCD69D7D12B1D9DF076B7BE4E1C

    -   [android\_m2repository\_r26.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r26.zip) &ndash; 8157FC1C311BB36420C1D8992AF54A4D

    -   [android\_m2repository\_r25.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip) &ndash; 0B3F1796C97C707339FB13AE8507AF50

    -   [android\_m2repository\_r24.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r24.zip) &ndash; 8E3C9EC713781EDFE1EFBC5974136BEA

    -   [android\_m2repository\_r23.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r23.zip) &ndash; D5BB66B3640FD9B9C6362C9DB5AB0FE7

    -   [android\_m2repository\_r22.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r22.zip) &ndash; 96659D653BDE0FAEDB818170891F2BB0

    -   [android\_m2repository\_r21.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r21.zip) &ndash; CD3223F2EFE068A26682B9E9C4B6FBB5

    -   [android\_m2repository\_r20.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r20.zip) &ndash; 650E58DF02DB1A832386FA4A2DE46B1A

    -   [android\_m2repository\_r19.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r19.zip) &ndash; 263B062D6EFAA8AEE39E9460B8A5851A

    -   [android\_m2repository\_r18.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r18.zip) &ndash; 25947AD38DCB4865ABEB61522FAFDA0E

    -   [android\_m2repository\_r17.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r17.zip) &ndash; 49054774F44AE5F35A6BA9D3C117EFD8

    -   [android\_m2repository\_r16.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip) &ndash; 0595E577D19D31708195A83087881EE6

    Wenn der **m2repository** Archiv wird nicht angezeigt in dieser Tabelle können Sie die Download-URL erstellen, durch voranstellen **https://dl-ssl.google.com/android/repository/** auf den Namen der **m2repository** herunterladen. Verwenden Sie z. B. **https://dl-ssl.google.com/android/repository/android\_m2repository\_r10.zip** herunterladen **android\_m2repository\_r10.zip**.

2.  Benennen Sie die Datei, in der entsprechenden MD5-Hash der Download-URL wie in der obigen Tabelle gezeigt. Angenommen, Sie heruntergeladen haben **android\_m2repository\_r25.zip**, benennen Sie sie in **0B3F1796C97C707339FB13AE8507AF50.zip**. Wenn der MD5-Hash für die Download-URL der heruntergeladenen Datei in der Tabelle nicht angezeigt wird, können Sie mithilfe einer [online MD5-Generator](http://www.webconfs.com/online-md5-generator.php) , die URL in eine MD5-Hash-Zeichenfolge zu konvertieren. 

3.  Kopieren Sie die Datei mit dem Xamarin **zips** Ordner: 

    -   Unter Windows, befindet sich dieser Ordner unter **C:\\Benutzer\\***Benutzername***\\AppData\\lokalen\\Xamarin\\zips**. 

    -   Unter Mac OS X, befindet sich dieser Ordner unter **/Users /***Benutzername***/.local/share/Xamarin/zips**. 

    Der folgende Screenshot veranschaulicht z. B. das Ergebnis beim **android\_m2repository\_r16.zip** heruntergeladen und in den MD5-Hash, der die Download-URL auf Windows umbenannt:

    [![Beispiel für das umbenannt wird, um 0595E577D19D31708195A83087881EE6.zip r16.zip-repository](resolving-library-installation-errors-images/03-md5-rename-vs.png)](resolving-library-installation-errors-images/03-md5-rename-vs.png#lightbox)


Wenn dieses Verfahren nicht den Buildfehler behoben wird, müssen Sie manuell herunterladen der **android\_m2repository\_r_nn_.zip** Datei, entpacken Sie es und seinen Inhalt zu installieren, wie im nächsten Abschnitt beschrieben. 


### <a name="manually-downloading-and-installing-m2repository-files"></a>Manuell herunterladen und Installieren von m2repository-Dateien

Der vollständig manueller Prozess zum Wiederherstellen von **m2repository** Fehler bringt Herunterladen der **android\_m2repository\_r_nn_.zip** Datei (mit einem Webbrowser), entzippt er und sein Inhalt in das Verzeichnis des Support-Bibliothek auf Ihrem Computer kopieren. Im folgenden Beispiel werden wir diese Fehlermeldung Wiederherstellung: 

```shell
Unzipping failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\23.1.1\content directory.
```

Verwenden Sie die folgenden Schritte aus, um das Herunterladen **m2repository** und seinen Inhalt zu installieren:

1.  Löschen Sie den Inhalt der Ordner "Library", der Fehlermeldung entspricht. Klicken Sie beispielsweise in der oben angezeigten Fehlermeldung löschen den Inhalt der **C:\\Benutzer\\***Benutzername***\\AppData\\lokalen\\Xamarin\\ Android.Support. v4\\23.1.1.0**. 
    Wie zuvor beschrieben, müssen Sie den gesamten Inhalt dieses Verzeichnisses löschen:

    [![Löschen von Inhalten, eingebettet, und die 23.1.1.0 android_m2repository Ordner Ordner](resolving-library-installation-errors-images/04-delete-contents-vs.png)](resolving-library-installation-errors-images/04-delete-contents-vs.png#lightbox)

2.  Herunterladen der **android\_m2repository\_r_nn_.zip** Datei von Google, die dem Fehler entspricht Meldung (siehe die Tabelle im Abschnitt oben links).

3.  Diese extrahieren **ZIP** -Archiv in einen beliebigen Speicherort (z. B. den Desktop). Dies sollte erstellen Sie ein Verzeichnis, das den Namen des entspricht, der **ZIP** Archiv. In diesem Verzeichnis finden Sie ein Unterverzeichnis namens **m2repository**: 

    [![m2repository Ordner finden Sie im extrahierten Zip-Archiv](resolving-library-installation-errors-images/05-m2repository-vs.png)](resolving-library-installation-errors-images/05-m2repository-vs.png#lightbox)

4.  Im Bibliotheksverzeichnis mit Versionsangabe, die Sie in Schritt 1 gelöscht, neu erstellen die **Inhalt** und **eingebettete** Unterverzeichnisse. Beispielsweise der folgende Screenshot veranschaulicht **Inhalt** und **eingebettete** Unterverzeichnisse erstellt wird, der **23.1.1.0** Ordner für **android \_m2repository\_r25.zip**: 

    [![Erstellen von Inhalt und embedded-Ordner in der 23.1.1.0 Ordner](resolving-library-installation-errors-images/06-recreate-folders-vs.png)](resolving-library-installation-errors-images/06-recreate-folders-vs.png#lightbox)

5.  Kopie **m2repository** aus dem extrahierten **ZIP** in die **Inhalt** Verzeichnis, das Sie im vorherigen Schritt erstellt haben: 

    [![Screenshot der m2repository in 23.1.1.0/content-Ordner kopiert](resolving-library-installation-errors-images/07-copied-m2repository-vs.png)](resolving-library-installation-errors-images/07-copied-m2repository-vs.png#lightbox)

6.  In den extrahierten **ZIP** Verzeichnis, navigieren Sie zu **m2repository\\com\\android\\unterstützen\\Support-v4-** , und öffnen Sie die entsprechenden Ordner die Versionsnummer, die zuvor erstellte (in diesem Beispiel **23.1.1**):

    [![Beispiel für Liste der Dateien, die im Ordner "support-v4/23.1.1" enthalten](resolving-library-installation-errors-images/08-zip-contents-vs.png)](resolving-library-installation-errors-images/08-zip-contents-vs.png#lightbox)

7.  Kopieren Sie alle Dateien in diesen Ordner mit den **eingebettete** in Schritt 4 erstellten Verzeichnis:

    [![Beispiel für die Dateien, die in den Ordner 23.1.1.0/embedded kopiert](resolving-library-installation-errors-images/09-copied-vs.png)](resolving-library-installation-errors-images/09-copied-vs.png#lightbox)

8.  Stellen Sie sicher, dass alle Dateien kopiert wurden. Die **eingebettete** Verzeichnis sollte jetzt enthalten Dateien wie z. B. **JAR**, **aar**, und **.pom**.

9.  Entzippen Sie den Inhalt der extrahierten **aar** von Dateien in die **eingebettete** Verzeichnis. Auf Windows, fügen Sie eine **ZIP** Erweiterung der **aar-Datei** Datei, öffnen Sie es und kopieren Sie den Inhalt in die **eingebettete** Verzeichnis.
    Unter MacOS, entpacken Sie die **aar** -Datei mit der **Entzippen** Befehl im Terminal (z. B. **Entzippen file.aar**).

An diesem Punkt haben Sie die fehlenden Komponenten manuell installiert, und das Projekt sollte ohne Fehler erstellt. Wenn nicht der Fall, stellen Sie sicher, dass Sie heruntergeladen haben die **m2repository** **ZIP** archivieren Sie die Version, die genau die Version in der Fehlermeldung entspricht, und stellen Sie sicher, dass Sie dessen Inhalt im installiert haben die Korrigieren Sie Speicherorte an, wie in den vorherigen Schritten beschrieben. 



## <a name="summary"></a>Zusammenfassung 

In diesem Artikel wurde erläutert, wie häufige Fehler zu beheben, die während der automatischen Download und Installation der Abhängigkeit Bibliotheken ausgeführt werden können. Es wurde beschrieben, wie problematische Bibliothek löschen und das Projekt neu erstellen, als eine Möglichkeit, erneut herunterladen und installieren Sie die Bibliothek erneut. Es wurde beschrieben, wie Sie die Bibliothek herunterladen und installieren Sie es in der **zips** Ordner. Es wurde außerdem beschrieben, eine weitere Verfahren für das manuelle herunterladen und installieren die erforderlichen Dateien als eine Möglichkeit zum Umgehen der Probleme, die über automatische bedeutet, dass nicht aufgelöst werden kann. 
