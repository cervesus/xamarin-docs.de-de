---
title: Wie führe ich durch eine gründliche für Xamarin für Visual Studio deinstallieren?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: c1742239-05ea-449d-9c99-611e5e5a90e4
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: 99fde9330498ee62d3cf6b5910c2cbfae39cfdeb
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
ms.locfileid: "33917667"
---
# <a name="how-do-i-perform-a-thorough-uninstall-for-xamarin-for-visual-studio"></a>Wie führe ich durch eine gründliche für Xamarin für Visual Studio deinstallieren?


1.  Deinstallieren Sie keines der folgenden, die vorhanden sind, in der Windows-Systemsteuerung:

    -   Xamarin
    -   Xamarin für Windows
    -   Xamarin.Android
    -   Xamarin.iOS
    -   Xamarin für Visual Studio

2.  Löschen Sie alle übrigen Dateien im Explorer von Xamarin für Visual Studio-Erweiterungsordner (alle Versionen einschließlich beide _Programmdateien_ und _Programmdateien (x86)_):

    _"C:"\\Programmdateien\*\\Microsoft Visual Studio 1\*.0\\Common7\\IDE\\Erweiterungen\\Xamarin_

3.  Visual Studio MEF-Komponente Cacheverzeichnis sowie zu löschen:

    _%LocalAppData%\\Microsoft\\VisualStudio\\1\*.0\\ComponentModelCache_

    Diesen Schritt selbst ist häufig ausreichend, um Fehler zu beheben, z. B.:

    -   "Das Paket"XamarinShellPackage"wurde nicht ordnungsgemäß geladen"

    -   "... Die Projektdatei kann nicht geöffnet werden. Es besteht ein fehlender Projektuntertyp"

    -   "Objektverweis nicht auf eine Instanz eines Objekts festgelegt.  at Xamarin.VisualStudio.IOS.XamarinIOSPackage.Initialize()"

    -   "SetSite Fehler für Paket" (in Visual Studio _ActivityLog.xml_)

    -   "LegacySitePackage Fehler für Paket" (in Visual Studio _ActivityLog.xml_)

    (Siehe auch die [MEF-Komponente-Cache leeren](https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd) Visual Studio-Erweiterung.  Und finden Sie unter [Fehler 40781 Kommentar 19](https://bugzilla.xamarin.com/show_bug.cgi?id=40781#c19) etwas mehr Kontext zu diesem upstream Problem in Visual Studio, die diese Fehler verursachen können.)

4.  Überprüfen Sie auch der _VirtualStore_ Directory, um festzustellen, ob Windows eine u. u. bei gespeicherten überlagern Sie die Dateien für die _Erweiterungen\\Xamarin_ oder _ComponentModelCache_Verzeichnisse vorhanden:

    _%LocalAppData%\\VirtualStore_

5.  Öffnen Sie den Registrierungs-Editor (`regedit`).

6.  Suchen Sie nach dem folgenden Schlüssel:

    _HKEY\_lokale\_Computer\\SOFTWARE\\Wow6432Node\\Microsoft\\Windows\\CurrentVersion\\SharedDlls_

7.  Suchen und löschen Sie alle Einträge, die diesem Muster entsprechen:

    _"C:"\\Programmdateien\*\\Microsoft Visual Studio 1\*.0\\Common7\\IDE\\Erweiterungen\\Xamarin_

8.  Suchen Sie nach diesem Schlüssel:

    _HKEY\_aktuelle\_Benutzer\\Software\\Microsoft\\VisualStudio\\1\*.0\\ExtensionManager ab\\PendingDeletions_

9.  Löschen Sie alle Einträge, die zu Xamarin gehören könnten.  Beispielsweise verwendet es folgt eine, die in früheren Versionen von Xamarin Probleme verursachen:

    _Mono.VisualStudio.Shell,1.0_

10. Öffnen Sie eine Administrator `cmd.exe` -Eingabeaufforderung, und führen Sie die `devenv /setup` und `devenv /updateconfiguration` -Befehle für jede installierte Version von Visual Studio.  Beispiel für Visual Studio 2015:

    ```
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /setup
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /updateconfiguration
    ```

11. Starten Sie den Computer neu.

12. Installieren Sie die aktuelle stabile Version des Xamarin mithilfe von [visualstudio.com](https://visualstudio.com/xamarin/).

## <a name="additional-troubleshooting-steps-for-package-did-not-load-correctly"></a>Weitere Schritte zur Fehlerbehebung für "Paket nicht ordnungsgemäß geladen wurden"

Hier sind einige zusätzliche Schritte, um zu versuchen, in Fällen, in denen die oben genannten Schritte nicht den Fehler "Paket wurde nicht ordnungsgemäß geladen" beheben.

1.  Erstellen Sie ein neues Windows-Benutzerkonto an.

2.  Überprüfen Sie, ob die Xamarin für Visual Studio-Erweiterungen ohne Fehler für den neuen Benutzer laden.

3.  Wenn die Erweiterungen ordnungsgemäß geladen werden, ist das Problem wahrscheinlich durch einige der gespeicherten Einstellungen für den ursprünglichen Benutzer verursacht:

    -   **Im Explorer** – _%LocalAppData%\\Microsoft\\VisualStudio\\1\*.0_
    -   **Klicken Sie in Regedit** – _HKEY\_aktuelle\_Benutzer\\Software\\Microsoft\\VisualStudio\\1\*.0_
    -   **Klicken Sie in Regedit** – _HKEY\_aktuelle\_Benutzer\\Software\\Microsoft\\VisualStudio\\1\*.0\_Config_

4.  Wenn diese gespeicherte Einstellungen tatsächlich das Problem zu sein scheinen, können Sie versuchen, sie sichern und löschen sie.
