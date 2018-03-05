---
title: Beheben von Installationsfehlern Bibliothek
description: "In einigen Fällen können Fehler auftreten, während der Installation von Bibliotheken für Android, unterstützen. Dieses Handbuch enthält problemumgehungen für einige häufige Fehler."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2AE68ACE-8496-445D-BF17-5E4097D4AE35
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 2df101615ed512d362fc065a1bb7080f3fd3bb33
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="resolving-library-installation-errors"></a>Beheben von Installationsfehlern Bibliothek

_In einigen Fällen können Fehler auftreten, während der Installation von Bibliotheken für Android, unterstützen. Dieses Handbuch enthält problemumgehungen für einige häufige Fehler._

## <a name="overview"></a>Übersicht

Beim Erstellen eines app-Projekts Xamarin.Android, möglicherweise erhalten Sie Buildfehler beim Visual Studio oder Visual Studio für Mac herunterladen und installieren die Abhängigkeit Bibliotheken. Viele dieser Fehler werden durch Netzwerkverbindungsprobleme, dass die Datei beschädigt oder Versionsprobleme verursacht. Dieses Handbuch beschreibt die am häufigsten verwendeten Support Library-Installationsfehler und enthält die Schritte, um diese Probleme umgehen und Abrufen von app-Projekt erneut erstellen. 

 
<a name="m2repository" />
 
## <a name="errors-while-downloading-m2repository"></a>Fehler beim Herunterladen der m2Repository

Möglicherweise **m2repository** Fehler, wenn Sie ein NuGet-Paket der Android-Unterstützungsbibliotheken oder Google Play-Dienste zu verweisen. Die Fehlermeldung sieht etwa wie die folgende aus:

```shell
Download failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\22.2.1\content directory.
```

Dieses Beispiel gilt für **android\_m2repository\_r16**, aber möglicherweise dieselbe Fehlermeldung für eine andere Version wie z. B. **android\_m2repository\_r18**  oder **android\_m2repository\_r25**. 


<a name="automatic" /> 

### <a name="automatic-recovery-from-m2repository-errors"></a>Automatische Wiederherstellung nach Fehlern m2repository 

Häufig kann dieses Problem behoben werden, durch die problematischen Bibliothek löschen und Neuerstellen von entsprechend der folgenden Schritte aus: 

1. Navigieren Sie zum Verzeichnis Support Library auf Ihrem Computer:

    -   Unter Windows befinden sich Unterstützungsbibliotheken am **"c:"\\Benutzer\\_Benutzername_\\AppData\\lokale\\Xamarin**. 

    -   Unter Mac OS X, Support-Bibliotheken befinden sich am **Users /_Benutzername_/.local/share/Xamarin**. 

2. Suchen der Bibliothek und Version Ordner, der Fehlermeldung entspricht. Befindet sich z. B. Ordner Bibliothek und die Version der oben aufgeführten Fehlermeldung unter **Android.Support. v4\\22.2.1**:

    [![Beispiel der Speicherort des Ordners für 22.2.1-Unterstützungsbibliothek](resolving-library-installation-errors-images/01-example-location.png)](resolving-library-installation-errors-images/01-example-location.png)

3. Löschen Sie den Inhalt der Ordner "Version". Achten Sie darauf, dass Sie zum Entfernen der **ZIP** Datei sowie das **Inhalt** und **eingebettete** Unterverzeichnisse in diesen Ordner. Für die Beispiel-Fehlermeldung oben, Dateien und Unterverzeichnisse in diesem Screenshot dargestellt (**Inhalt**, **eingebettete**, und **android_m2repository_r16.zip**) sind gelöscht werden:

    [![Beispiel-Inhalt des 22.2.1 unterstützen Bibliotheksordner](resolving-library-installation-errors-images/02-example-folder-vs.png)](resolving-library-installation-errors-images/02-example-folder-vs.png)

   Beachten Sie, dass es wichtig ist, löschen Sie die *gesamte* Inhalt dieses Ordners. Obwohl dieser Ordner anfänglich enthalten kann die "fehlt" **android\_m2repository\_r16.zip** Datei, diese Datei wurde möglicherweise teilweise heruntergeladene oder ist beschädigt.

4. Das Projekt neu &ndash; zu den Buildprozess die fehlenden Bibliothek erneut herunterladen.

In den meisten Fällen werden diese Schritte beheben der Fehler beim Erstellen und ermöglichen es Ihnen, den Vorgang fortzusetzen. Wenn den Buildfehler durch das Löschen dieser Bibliothek nicht behoben wird, müssen Sie manuell herunterladen und Installieren der **android\_m2repository\_r_nn_.zip** Datei wie im nächsten Abschnitt beschrieben. 


<a name="download" /> 

### <a name="manually-downloading-m2repository"></a>M2repository manuell herunterladen

Wenn Sie versucht haben, verwenden die oben beschriebenen Schritte für die automatische Wiederherstellung und Buildfehler weiterhin bestehen, können Sie manuell herunterladen der **android\_m2repository\_r_nn_.zip** Datei (mit einem Webbrowser), und installieren Sie es entsprechend um die folgenden Schritte aus. Diese Vorgehensweise ist auch hilfreich, wenn Sie keinen Zugriff auf das Internet auf dem Entwicklungscomputer, aber Sie das Archiv, das mit einem anderen Computer herunterladen können. 

1.  Herunterladen der **android\_m2repository\_r_nn_.zip** Datei, die mit der Fehlermeldung entspricht &ndash; werden Links bereitgestellt, in der folgenden Liste (zusammen mit den entsprechenden MD5-Hash für jede Verknüpfung URL):

    -   [android\_m2repository\_r33.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r33.zip) &ndash; 5FB756A25962361D17BBE99C3B3FCC44

    -   [Android\_m2repository\_r32.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip) &ndash; F16A3455987DBAE5783F058F19F7FCDF

    -   [Android\_m2repository\_r31.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r31.zip) &ndash; 99A8907CE2324316E754A95E4C2D786E

    -   [Android\_m2repository\_r30.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r30.zip) &ndash; 05AD180B8BDC7C21D6BCB94DDE7F2C8F

    -   [android\_m2repository\_r29.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r29.zip) &ndash; 2A3A8A6D6826EF6CC653030E7D695C41

    -   [android\_m2repository\_r28.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r28.zip) &ndash; 17BE247580748F1EDB72E9F374AA0223

    -   [Android\_m2repository\_r27.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r27.zip) &ndash; C9FD4FCD69D7D12B1D9DF076B7BE4E1C

    -   [android\_m2repository\_r26.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r26.zip) &ndash; 8157FC1C311BB36420C1D8992AF54A4D

    -   [android\_m2repository\_r25.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip) &ndash; 0B3F1796C97C707339FB13AE8507AF50

    -   [android\_m2repository\_r24.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r24.zip) &ndash; 8E3C9EC713781EDFE1EFBC5974136BEA

    -   [Android\_m2repository\_r23.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r23.zip) &ndash; D5BB66B3640FD9B9C6362C9DB5AB0FE7

    -   [android\_m2repository\_r22.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r22.zip) &ndash; 96659D653BDE0FAEDB818170891F2BB0

    -   [Android\_m2repository\_r21.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r21.zip) &ndash; CD3223F2EFE068A26682B9E9C4B6FBB5

    -   [Android\_m2repository\_r20.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r20.zip) &ndash; 650E58DF02DB1A832386FA4A2DE46B1A

    -   [android\_m2repository\_r19.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r19.zip) &ndash; 263B062D6EFAA8AEE39E9460B8A5851A

    -   [android\_m2repository\_r18.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r18.zip) &ndash; 25947AD38DCB4865ABEB61522FAFDA0E

    -   [android\_m2repository\_r17.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r17.zip) &ndash; 49054774F44AE5F35A6BA9D3C117EFD8

    -   [android\_m2repository\_r16.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip) &ndash; 0595E577D19D31708195A83087881EE6

    Wenn die **m2repository** Archiv wird nicht angezeigt in dieser Tabelle können Sie die Download-URL erstellen, vorangestellt **https://dl-ssl.google.com/android/repository/** auf den Namen des der **m2repository**  herunterladen. Verwenden Sie z. B. **https://dl-ssl.google.com/android/repository/android\_m2repository\_r10.zip** herunterladen **android\_m2repository\_r10.zip** .

2.  Benennen Sie die Datei mit den entsprechenden MD5-Hash der Download-URL wie in der obigen Tabelle gezeigt. Angenommen, Sie heruntergeladen haben **android\_m2repository\_r25.zip**, benennen Sie sie um **0B3F1796C97C707339FB13AE8507AF50.zip**. Wenn der MD5-Hash für die Download-URL der heruntergeladenen Datei nicht in der Tabelle angezeigt wird, können Sie eine [online MD5-Generator](http://www.webconfs.com/online-md5-generator.php) , die URL in eine MD5-Hash-Zeichenfolge zu konvertieren. 

3.  Kopieren Sie die Datei in die Xamarin **komprimiert** Ordner: 

    -   Unter Windows befindet sich dieser Ordner unter **"c:"\\Benutzer\\***Benutzername***\\AppData\\lokale\\Xamarin\\komprimiert**. 

    -   Unter Mac OS X, befindet sich dieser Ordner unter **Users /***Benutzername***/.local/share/Xamarin/zips**. 

    Das folgende Bildschirmfoto zeigt z. B. das Ergebnis beim **android\_m2repository\_r16.zip** wird heruntergeladen und umbenannt werden, um die MD5-Hash der Download-URL, unter Windows:

    [![Beispiel für das r16.zip-Repository wird in 0595E577D19D31708195A83087881EE6.zip umbenannt](resolving-library-installation-errors-images/03-md5-rename-vs.png)](resolving-library-installation-errors-images/03-md5-rename-vs.png)


Wenn diese Prozedur nicht die Buildfehler behoben wird, müssen Sie manuell herunterladen der **android\_m2repository\_r_nn_.zip** Datei, entpacken Sie es, und installieren Sie den Inhalt aus, wie im nächsten Abschnitt beschrieben. 


<a name="install" /> 

### <a name="manually-downloading-and-installing-m2repository-files"></a>Manuell herunterladen und Installieren von m2repository-Dateien

Der vollständig manuellen Prozess für die Wiederherstellung **m2repository** Fehler bringt Herunterladen der **android\_m2repository\_r_nn_.zip** Datei (mit einem Webbrowser), entzippt es, und kopieren seinen Inhalt in das Bibliotheksverzeichnis Unterstützung auf Ihrem Computer. Im folgenden Beispiel werden wir diese Fehlermeldung Wiederherstellung: 

```shell
Unzipping failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\23.1.1\content directory.
```

Verwenden Sie die folgenden Schritte aus, um herunterladen **m2repository** und seinen Inhalt zu installieren:

1.  Löschen Sie den Inhalt des Ordners "Bibliothek", der Fehlermeldung entspricht. Klicken Sie beispielsweise in der oben aufgeführten Fehlermeldung löschen den Inhalt der **"c:"\\Benutzer\\***Benutzername***\\AppData\\lokale\\Xamarin\\ Android.Support. v4\\23.1.1.0**. 
    Wie zuvor beschrieben, müssen Sie den gesamten Inhalt dieses Verzeichnisses löschen:

    [![Löschen von Inhalten, eingebettet, und android_m2repository Ordnern über den 23.1.1.0 Ordner](resolving-library-installation-errors-images/04-delete-contents-vs.png)](resolving-library-installation-errors-images/04-delete-contents-vs.png)

2.  Herunterladen der **android\_m2repository\_r_nn_.zip** Datei von Google, die dem Fehler entspricht (siehe die Tabelle im vorherigen Abschnitt, um Links).

3.  Diese extrahieren **ZIP** Archive an einem beliebigen Speicherort (z. B. den Desktop). Dies sollte erstellen Sie ein Verzeichnis, das den Namen des entspricht, der **ZIP** Archiv. In diesem Verzeichnis finden Sie ein Unterverzeichnis mit dem Namen **m2repository**: 

    [![m2repository Ordner im extrahierten Zip-Archiv gefunden](resolving-library-installation-errors-images/05-m2repository-vs.png)](resolving-library-installation-errors-images/05-m2repository-vs.png)

4.  Im Bibliotheksverzeichnis mit Versionsangabe, die Sie in Schritt 1 gelöscht, Neuerstellen der **Inhalt** und **eingebettete** Unterverzeichnisse. Der folgende Screenshot veranschaulicht beispielsweise **Inhalt** und **eingebettete** Unterverzeichnisse erstellt wird, der **23.1.1.0** Ordner für **android \_m2repository\_r25.zip**: 

    [![Erstellen von Inhalt und eingebettete Ordnern in der 23.1.1.0 Ordner](resolving-library-installation-errors-images/06-recreate-folders-vs.png)](resolving-library-installation-errors-images/06-recreate-folders-vs.png)

5.  Kopie **m2repository** aus den extrahierten **ZIP** in der **Inhalt** von Ihnen im vorherigen Schritt erstellte Verzeichnis: 

    [![Screenshot des m2repository 23.1.1.0/content Ordner kopiert](resolving-library-installation-errors-images/07-copied-m2repository-vs.png)](resolving-library-installation-errors-images/07-copied-m2repository-vs.png)

6.  In den extrahierten **ZIP** Verzeichnis durchsuchen, um **m2repository\\com\\android\\unterstützen\\Unterstützung v4** , und öffnen Sie die entsprechenden Ordner die Versionsnummer, die oben erstellte (in diesem Beispiel **23.1.1**):

    [![Beispiel-Liste der Dateien, die im Ordner "support-v4/23.1.1" enthalten](resolving-library-installation-errors-images/08-zip-contents-vs.png)](resolving-library-installation-errors-images/08-zip-contents-vs.png)

7.  Kopieren Sie alle Dateien in diesem Ordner auf den **eingebettete** in Schritt 4 erstellte Verzeichnis:

    [![Beispiel für die Dateien in den Ordner 23.1.1.0/embedded kopiert](resolving-library-installation-errors-images/09-copied-vs.png)](resolving-library-installation-errors-images/09-copied-vs.png)

8.  Stellen Sie sicher, dass alle Dateien kopiert werden. Die **eingebettete** Verzeichnis sollte jetzt enthalten Dateien wie z. B. **JAR**, **aar**, und **.pom**.

An diesem Punkt haben Sie die fehlenden Komponenten manuell installiert, und das Projekt sollte ohne Fehler erstellt. Wenn dies nicht der Fall, stellen Sie sicher, dass Sie heruntergeladen haben die **m2repository** **ZIP** archivieren Version, die genau auf die Version in der Fehlermeldung entspricht und stellen Sie sicher, dass die Installation von Inhalt in der Korrigieren Sie die Speicherorte, wie in den obigen Schritten beschrieben. 


<a name="summary" /> 

## <a name="summary"></a>Zusammenfassung 

In diesem Artikel wird erläutert, wie die Wiederherstellung von häufigen Fehlern, die während der automatischen Download und Installation der Abhängigkeit Bibliotheken stattfinden kann. Er beschreibt, wie Sie problematische Bibliothek löschen und das Projekt als eine Möglichkeit, erneut herunterladen und installieren die Bibliothek erneut neu. Er beschreibt, wie Sie die Bibliothek herunterladen und installieren ihn in die **komprimiert** Ordner. Außerdem wurde beschrieben, ein komplizierter Verfahren zum manuell herunterladen und installieren die erforderlichen Dateien als eine Möglichkeit zum Umgehen von Problemen, die über automatische bedeutet, dass nicht aufgelöst werden kann. 
