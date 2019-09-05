---
title: Wie führe ich eine gründliche Deinstallation von Xamarin für Visual Studio aus?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: c1742239-05ea-449d-9c99-611e5e5a90e4
author: conceptdev
ms.author: crdun
ms.date: 12/02/2016
ms.openlocfilehash: 4d8396574f02cde4c75fd96c98f79d5ed4080293
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291073"
---
# <a name="how-do-i-perform-a-thorough-uninstall-for-xamarin-for-visual-studio"></a>Wie führe ich eine gründliche Deinstallation von Xamarin für Visual Studio aus?


1. Deinstallieren Sie in der Windows-Systemsteuerung eine der folgenden Elemente:

    - Xamarin
    - Xamarin für Windows
    - Xamarin.Android
    - Xamarin.iOS
    - Xamarin für Visual Studio

2. Löschen Sie in Explorer alle verbleibenden Dateien aus den xamarin Visual Studio-Erweiterungs Ordnern (alle Versionen, einschließlich der _Programmdateien_ und der _Programmdateien (x86)_ ):

    _C:\\Programmdateien\*MicrosoftVisualStudio1\*0Common7\\IDE\\-Erweiterungen\\xamarin\\\\_

3. Löschen Sie auch das MEF-Komponenten Cache Verzeichnis von Visual Studio:

    _%LOCALAPPDATA%\\Microsoft\\VisualStudio\\1\*.0\\ComponentModelCache_

    Tatsächlich ist dieser Schritt selbst oft ausreichend, um Fehler wie die folgenden zu beheben:

    - "Das" xamarinshellpackage "-Paket wurde nicht ordnungsgemäß geladen.

    - "Die Projektdatei... kann nicht geöffnet werden. Ein Projekt Untertyp fehlt.

    - "Der Objekt Verweis ist nicht auf eine Instanz eines Objekts festgelegt.  at Xamarin.VisualStudio.IOS.XamarinIOSPackage.Initialize()"

    - "SetSite failed for Package" (in der Datei " _activityLog. XML_" von Visual Studio)

    - "Legacysitepackage failed for Package" (in der Datei " _activityLog. XML_" von Visual Studio)

    (Weitere Informationen finden Sie auch in der Visual Studio-Erweiterung für den [MEF-Komponenten Cache](https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd)  Weitere Informationen zu dem upstreamproblem in Visual Studio, das diese Fehler verursachen kann, finden Sie in [Bug 40781, Kommentar 19](https://bugzilla.xamarin.com/show_bug.cgi?id=40781#c19) .)

4. Überprüfen Sie auch das _virtualstore_ -Verzeichnis, um festzustellen, ob Windows ggf. über Lagerungs Dateien für die _\\xamarin_ -oder _componentmodelcache_ -Dateien der Erweiterungen gespeichert hat:

    _%LOCALAPPDATA%\\VirtualStore_

5. Öffnen Sie den Registrierungs-`regedit`Editor ().

6. Suchen Sie nach folgendem Schlüssel:

    _HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Wow6432Node\\Microsoft\\Windows\\CurrentVersion\\SharedDlls_

7. Suchen und löschen Sie alle Einträge, die diesem Muster entsprechen:

    _C:\\Programmdateien\*MicrosoftVisualStudio1\*0Common7\\IDE\\-Erweiterungen\\xamarin\\\\_

8. Suchen Sie nach diesem Schlüssel:

    _HKEY\_Current\_User\\SoftwareMicrosoft\\VisualStudio\\10\\ExtensionManager-\\datdingdeletions\\\*_

9. Löschen Sie alle Einträge, die zu Xamarin gehören könnten.  Hier ist beispielsweise eine solche, die verwendet wird, um in älteren Versionen von xamarin Probleme zu verursachen:

    _Mono.VisualStudio.Shell,1.0_

10. Öffnen Sie eine `cmd.exe` Administrator Eingabeaufforderung, und führen Sie `devenv /setup` dann `devenv /updateconfiguration` die Befehle und für jede installierte Version von Visual Studio aus.  Beispiel für Visual Studio 2015:

    ```
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /setup
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /updateconfiguration
    ```

11. Starten Sie den Computer neu.

12. Installieren Sie die aktuelle stabile Version von xamarin mithilfe von von [VisualStudio.com](https://visualstudio.com/xamarin/)neu.

## <a name="additional-troubleshooting-steps-for-package-did-not-load-correctly"></a>Zusätzliche Schritte zur Problembehandlung für "das Paket wurde nicht ordnungsgemäß geladen"

In Fällen, in denen der Fehler "das Paket wurde nicht ordnungsgemäß geladen" aufgelöst wird, finden Sie hier einige weitere Schritte.

1. Erstellen Sie ein neues Windows-Benutzerkonto.

2. Überprüfen Sie, ob die xamarin Visual Studio-Erweiterungen für den neuen Benutzer ohne Fehler geladen werden.

3. Wenn die Erweiterungen ordnungsgemäß geladen werden, wird das Problem wahrscheinlich durch einige der gespeicherten Einstellungen für den ursprünglichen Benutzer verursacht:

    - **Im Explorer** – _% LocalAppData%\\Microsoft\\VisualStudio\\1\*. 0_
    - **In regedit** – _HKEY\_Current\_User\\Software\\Microsoft\\VisualStudio\\1\*. 0_
    - **In regedit** – _HKEY\_Current\_User\\Software\\Microsoft\\VisualStudio\\1\*0\_config_

4. Wenn diese gespeicherten Einstellungen anscheinend das Problem darstellen, können Sie versuchen, Sie zu sichern und anschließend zu löschen.
