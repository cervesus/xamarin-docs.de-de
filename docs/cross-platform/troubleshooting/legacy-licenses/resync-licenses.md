---
title: Wie synchronisieren ich Xamarin-Lizenzen manuell?
ms.topic: article
ms.prod: xamarin
ms.assetid: D0BD93E9-3A1F-4E5B-8EE8-36ADC33DCFE4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 2413b6b7563a6ed1e17a8db61d2d61ddc85e71ae
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="how-do-i-manually-resynchronize-xamarin-licenses"></a>Wie synchronisieren ich Xamarin-Lizenzen manuell?

> [!IMPORTANT]
> Dieses Handbuch gilt nicht für die meisten Benutzer von MSDN, da sie nicht erforderlich sind, besitzen oder Xamarin-Konten anmelden, es sei denn, die [Xamarin-Komponenten speichern](https://components.xamarin.com/) oder [Visual Studio für Mac (Mac)](~/cross-platform/get-started/requirements.md).




## <a name="overview"></a>Übersicht

In der Regel werden die Lizenzinformationen mit dem Xamarin-Lizenz-Server automatisch neu synchronisiert werden beim Starten der IDE. Beispielsweise erscheint Lizenz-Upgrades, Downgrades und-Erneuerung normalerweise automatisch nach dem Neustart der IDE. Die Lizenzinformationen in der IDE dargestellten übereinstimmen, die Informationen auf Ihrem [Kontoseite](https://store.xamarin.com/account/my/subscription/computers). Wenn die Informationen noch nicht übereinstimmende befinden, können Sie zunächst überprüfen, dass Ihre Kontoinformationen Store korrekt aussieht, und klicken Sie dann manuell neu zu synchronisieren die Lizenzen von Abmelden, löschen die Lizenzdateien auf Datenträger und dann wieder anmelden.

### <a name="note-for-ios-developers-on-windows"></a>Beachten Sie für iOS-Entwickler unter Windows

iOS-Entwickler in Windows müssen möglicherweise auch Schritte für Visual Studio für Mac ausgeführt; auch wenn nicht direkt auf Ihrer paarweise zugeordneten Mac-buildhost entwickeln.

## <a name="quick-manual-refresh-steps"></a>Schnelle manuelle Aktualisierung Schritte

Diese schnelle Vorgang überspringt den Schritt des Löschens der Lizenzdateien auf Datenträger ab, die möglicherweise in einigen Fällen ausreichend. 

1.  Melden Sie sich über die IDE Ihrer Xamarin-Konto:
    -   Visual Studio für Mac
        -   Oben rechts im Willkommensbildschirm
        -   **Visual Studio für Mac > Konto** (Mac)
        -   **Extras > Konto** (Windows)
    -   Visual Studio
        -   **Extras > Xamarin-Konto**
2.  Melden Sie sich wieder in die Xamarin-Konto in der IDE.

## <a name="extended-manual-license-refresh-steps"></a>Erweiterte Lizenz manuelle Aktualisierung Schritte

1.  Melden Sie sich über die IDE Ihrer Xamarin-Konto. 
2.  Beenden der IDE an.
3.  Überprüfen Sie, dass die Kontoinformationen Store richtig aussieht. Insbesondere bei doppelten Computernamen Überprüfen der [Seite "Computer"](https://store.xamarin.com/account/my/subscription/computers).

4.  Verwenden, wenn Sie ein Paar von doppelten Computernamen angezeigt wird, die **deaktivieren** Dropdown-Menü zu entfernenden Elements _beide_ Member des Paars:
    
    ![Lizenz -> deaktivieren auf https://store.xamarin.com/account/my/subscription/computers](resync-licenses-images/deactivate.png "Deactivate-Dropdown-Menü-Element verwenden, um beide Mitglieder des Paars zu entfernen")

5.  Löschen Sie alle verbleibenden Kopien der Lizenzdateien, die auf dem Datenträger noch vorhanden.
    -   Windows

        Löschen Sie alle der folgenden Ordner:
        -   `%PROGRAMDATA%\Mono for Android`
        -   `%PROGRAMDATA%\MonoTouch`
        -   `%LOCALAPPDATA%\VirtualStore\ProgramData\Mono for Android`
        -   `%LOCALAPPDATA%\VirtualStore\ProgramData\MonoTouch`

        In Standardinstallationen von Windows:
        -   `%PROGRAMDATA%` wird erweitert, um `C:\ProgramData`
        -   `%LOCALAPPDATA%` wird erweitert, um `C:\Users\%USERNAME%\AppData\Local`

        Die `VirtualStore` Kopien die Lizenzen werden von Windows in bestimmten Szenarien automatisch erstellt. Wenn die Kopien VirtualStore vorhanden sind, diese werden gelesen _stattdessen_ Lizenzen in `%PROGRAMDATA%`.

    -   Mac

        Löschen Sie alle der folgenden Ordner:

        -   `~/Library/MonoAndroid`
        -   `~/Library/MonoTouch`
        -   `~/Library/Xamarin.Mac`

        Sie können diese Pfade im zugreifen **Finder** durch Auswahl der **wechseln > wechseln Sie zum Ordner** Menü- und in das Dialogfenster eingefügt. Finder automatisch ersetzt die ~ Tildezeichen mit Ihrem Ordner "Benutzer".

6.  Geöffneten Visual Studio für Mac oder Visual Studio und Protokolldateien zurück in die Xamarin-Konto.

## <a name="supplementary-information-individual-license-file-locations"></a>Zusätzliche Informationen: einzelner Lizenz Dateispeicherorte

### <a name="windows"></a>Windows

Unter Windows werden die einzelnen Lizenzdateien in den folgenden Speicherorten gespeichert:

-   Xamarin.Android:  
     `%PROGRAMDATA%\Mono for Android\License\monoandroid.licx`
-   Xamarin.iOS:  
     `%PROGRAMDATA%\MonoTouch\License\monotouch.licx`

Lizenzen für *Testversion* Abonnements unterschiedlich benannt werden:

-   Xamarin.Android:  
     `%PROGRAMDATA%\Mono for Android\License\monoandroid.trial.licx`
-   Xamarin.iOS:  
     `%PROGRAMDATA%\MonoTouch\License\monotouch.trial.licx`

### <a name="mac"></a>Mac

Die einzelne Lizenzdateien werden auf einem Mac in den folgenden Speicherorten gespeichert:

-   Xamarin.Android:  
     `~/Library/MonoAndroid/License`
-   Xamarin.iOS:  
     `~/Library/MonoTouch/License.v2`
-   Xamarin.Mac:  
     `~/Library/Xamarin.Mac/License`

Lizenzen für *Testversion* Abonnements unterschiedlich benannt werden:

-   Xamarin.Android:  
     `~/Library/MonoAndroid/License.trial`
-   Xamarin.iOS:  
     `~/Library/MonoTouch/License.trial`
-   Xamarin.Mac:  
     `~/Library/Xamarin.Mac/License.trial`

## <a name="additional-troubleshooting-steps"></a>Weitere Schritte zur Problembehandlung

-   Überprüfen Sie, ob Sie alle nicht-ASCII-Zeichen in Ihren Benutzernamen ein, in den Namen Ihres Computers oder in vollständig erweiterter Pfade, der die Lizenzdateien haben.

-   [Wenden Sie sich an Entwickler von Xamarin-Unterstützung](http://xamarin.com/support)
