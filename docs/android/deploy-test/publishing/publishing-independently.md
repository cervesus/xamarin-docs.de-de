---
title: "Unabhängiges Veröffentlichen"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6FB4DEF2-01AD-C5FE-0950-CE1BF088A9C6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/21/2017
ms.openlocfilehash: 5e09bb1150c3cc53104b41b75a2c3d4d2db4e5ff
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="publishing-independently"></a>Unabhängiges Veröffentlichen

Sie können eine Anwendung veröffentlichen, ohne einen vorhandenen Android Marketplace zu verwenden. In diesem Abschnitt werden solche alternative Veröffentlichungsmethoden und Lizenzierungsebenen von Xamarin.Android erläutert.


## <a name="xamarin-licensing"></a>Xamarin-Lizenzierung

Vier Lizenzen stehen für die Entwicklung, Bereitstellung und Verteilung von Xamarin.Android-Apps zur Verfügung:

-   **Visual Studio Community**: für Studenten, kleine Teams und OSS-Entwickler, die Windows verwenden.

-   **Visual Studio Professional**: für einzelne Entwickler oder kleine Teams (nur Windows). Mit dieser Lizenz haben Sie Zugriff auf ein Standard- oder Cloud-Abonnement sowie auf zusätzliche Inhalte von Xamarin University. Des Weiteren bestehen keine Nutzungseinschränkungen.

-   **Visual Studio Enterprise**: für Teams beliebiger Größe (nur Windows). Diese Lizenz umfasst Unternehmensfunktionen, ein Standard- oder Cloud-Abonnement und einen Rabatt von 25 % für Xamarin Test Cloud.

Besuchen Sie [visualstudio.com](https://www.visualstudio.com/xamarin/), um sich die Community-Edition herunterzuladen oder mehr über den Erwerb der Professional- und Enterprise-Editionen zu erfahren.


## <a name="allow-installation-from-unknown-sources"></a>Installieren von Anwendungen aus unbekannten Quellen zulassen

Standardmäßig können Android-Benutzer nur Anwendungen von Google Play herunterladen und installieren. Damit ein Benutzer Anwendungen auch aus anderen Quellen als dem Marketplace herunterladen kann, muss dieser die Einstellung *Unbekannte Quellen* auf einem Gerät aktivieren, bevor eine Anwendung installiert wird. Diese Einstellung befindet sich wie im folgenden Diagramm dargestellt unter **Einstellungen > Sicherheit**:

[![Bildschirm „Einstellungen > Sicherheit“](publishing-independently-images/settings.png)](publishing-independently-images/settings.png#lightbox)


> [!IMPORTANT]
> Einige Netzwerkanbieter verhindern unabhängig von der Einstellung die Installation von Anwendungen aus unbekannten Quellen.



## <a name="publishing-by-e-mail"></a>Veröffentlichen per E-Mail

Durch das Anfügen des Release-APKs an eine E-Mail können Sie eine Anwendung schnell und komfortabel an Benutzer verteilen. Wenn der Benutzer die E-Mail auf einem Android-Gerät öffnet, erkennt Android die APK-Anlage und zeigt wie im folgenden Screenshot dargestellt die Schaltfläche **Installieren** an:

[![Schaltfläche „Installieren“ bei Öffnen einer Anlage](publishing-independently-images/publishing-via-email.png)](publishing-independently-images/publishing-via-email.png#lightbox)

Obwohl die Verteilung per E-Mail einfach ist, stehen wenige Schutzmaßnahmen gegen Softwarepiraterie oder die nicht gestattete Verteilung bereit. Diese Art der Verteilung eignet sich daher am besten für Situationen, in denen nur wenige Personen die Anwendung per E-Mail erhalten. Die Personen müssen als vertrauenswürdig gelten und dürfen die Anwendung nicht verteilen.


## <a name="publishing-by-web"></a>Veröffentlichen per Web

Sie können eine Anwendung auch über einen Webserver verteilen. Hierzu müssen Sie die Anwendung auf den Webserver hochladen und den Benutzern anschließend einen Downloadlink zur Verfügung stellen. Wenn ein Android-Gerät einen Link abruft und die Anwendung herunterlädt, wird diese automatisch installiert, sobald der Download abgeschlossen ist.


## <a name="manually-installing-an-apk"></a>Manuelles Installieren eines APKs

Die manuelle Installation ist die dritte Option zur Installation von Anwendungen. Gehen Sie wie folgt vor, um eine manuelle Installation durchzuführen:

1.   **Verteilen Sie eine Kopie des APKs an Benutzer**: Diese Kopie kann beispielsweise über eine CD oder einen USB-Speicherstick verteilt werden.
1.   **Der Benutzer installiert die Anwendung auf einem Android-Gerät**: Verwenden Sie das Befehlszeilentool *Android Debug Bridge* (**adb**). **adb** ist ein vielseitiges Befehlszeilentool, das die Kommunikation mit einer Emulatorinstanz oder einem Android-Gerät ermöglicht. Das Android SDK beinhaltet das Tool **adb**. Es befindet sich im Verzeichnis **<sdk>/platform-tools/**.

Das Android-Gerät muss mit einem USB-Kabel mit dem Computer verbunden sein.
Für Windows-Computer sind möglicherweise zusätzliche USB-Treiber vom Smartphonehersteller erforderlich, damit das Gerät von **adb** erkannt wird. Auf Installationsanweisungen für diese zusätzlichen USB-Treiber kann in diesem nicht eingegangen werden.

Vor dem Ausführen eines **adb**-Befehls sollte bekannt sein, ob und wenn ja welche Emulatorinstanzen oder Geräte verbunden sind. Eine Liste der verbundenen Geräte können Sie sich mit dem Befehl `devices` anzeigen lassen, wie im folgenden Codeausschnitt demonstriert wird:

```shell
$ adb devices
List of devices attached
        0149B2EC03012005device
```

Nachdem die verbundenen Geräte bestätigt wurden, kann die Anwendung durch Ausführen des Befehls `install` mit **adb** installiert werden:

```shell
$ adb install <path-to-apk>
```

Der folgende Codeausschnitt zeigt ein Beispiel für die Installation einer Anwendung auf einem verbundenen Gerät:

```shell
$ adb install helloworld.apk
3772 KB/s (3013594 bytes in 0.780s)
        pkg: /data/local/tmp/helloworld.apk
Success
```

Wenn die Anwendung bereits installiert ist, kann `adb install` das APK nicht installieren und gibt eine Fehlermeldung aus, wie im folgenden Beispiel gezeigt wird:

```shell
$ adb install helloworld.apk
4037 KB/s (3013594 bytes in 0.728s)
        pkg: /data/local/tmp/helloworld.apk
Failure [INSTALL_FAILED_ALREADY_EXISTS]
```

In diesem Fall muss die Anwendung auf dem Gerät deinstalliert werden. Führen Sie zunächst den Befehl `adb uninstall` aus:

```shell
adb uninstall <package_name>
```

Der folgende Codeausschnitt zeigt ein Beispiel für die Deinstallation einer Anwendung:

```shell
$ adb uninstall mono.samples.helloworld
Success
```
