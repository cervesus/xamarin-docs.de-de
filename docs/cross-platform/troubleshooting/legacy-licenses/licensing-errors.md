---
title: Einige bestimmte Lizenzierungsfehlern
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: D26BDF2D-923B-4BC1-841A-74583155DB71
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 8592fe5381c974e999477d0ef6ca6ebdd8b38cc4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="some-specific-licensing-errors"></a>Einige bestimmte Lizenzierungsfehlern

> [!IMPORTANT]
> Die unten aufgeführten Informationen gilt nicht für MSDN-Benutzer, da sie Probleme, die für ältere Xamarin-Lizenzen spezifisch sind. Wenn Sie ein MSDN-Benutzer sind, werden Fehlermeldungen, ähnlich wie unten angezeigt Bitte versuchen Sie es [aktualisieren auf die neueste Version von Xamarin](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/) & finden in diesem [-Lizenzierungsoptionen geführt](~/cross-platform/get-started/requirements.md).



## <a name="invalid-license"></a>"Ungültige Lizenz"

> Ungültige Lizenz. Reaktivieren Sie Xamarin.Android (XA9999) kann nicht Lizenz-Version zu bestimmen. (XA9010)

Diese Meldungen können in bestimmten Situationen auftreten.

-   Die aktuelle Lizenz auf dem Datenträger ist möglicherweise nicht-mehr-synchronisiert mit dem aktuellen Benutzerinformationen auf dem Computer. [Aktualisieren die Lizenzdateien](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md) dieses Problem in vielen Fällen aufgelöst wird.

-   Der Computer möglicherweise 2 in Konflikt stehende doppelte Aktivierungen. Diese Art von Lizenz wird nicht mehr von Xamarin. Wenn Probleme auftreten, die angezeigt werden, um Sie zu diesem Fehler verknüpft werden [e-Mail-Support](https://www.xamarin.com/support).

-   Die Xamarin-Konto kann eine Xamarin-Lizenz nicht noch eingeladen werden. Siehe auch: [Team Lizenzverwaltung](~/cross-platform/troubleshooting/legacy-licenses/team-management.md).

## <a name="failed-to-load-android-entitlements"></a>"Fehler beim Laden von Android-Berechtigungen"

> Mono.VisualStudio.Shell.ShellPackage-Fehler: 0: Fehler beim Laden der Android-Berechtigungen: Ungültige Lizenz. Reaktivieren Sie Xamarin.Android (XA9999) Mono.VisualStudio.Shell.ShellPackage Fehler: 0: Fehler bei der Aktualisierung der Lizenz: Ungültige Lizenz. Reaktivieren Sie Xamarin.Android (XA9999)

Dies ist eine ältere Version des Problems "Ungültige Lizenz" von oben. Die gleichen Schritte zur Problembehandlung anwenden zu können.

## <a name="this-version-was-released-after-your-subscription-expired"></a>"Diese Version wurde freigegeben, nachdem Ihr Abonnement abgelaufen"

> Fehler XA9000: Diese Version wurde freigegeben, nachdem Ihr Abonnement abgelaufen ist (11/11/2014 5:11:41 Uhr). (XA9000) (Mobile.Android.Utilities) Fehler XA9010: nicht Lizenz-Version zu ermitteln. (XA9010) (Mobile.Android.Utilities)

Diese Nachrichten werden häufig in zwei Situationen auf:

-   Die Lizenzen auf dem Datenträger sind veraltet. [Aktualisieren die Lizenzdateien](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md) dieses Problem in vielen Fällen aufgelöst wird.

-   Die Xamarin-Abonnement ist nicht mehr aktiv, und die aktuelle installierte Version von Xamarin ist aktueller als das Ablaufdatum des Abonnements. In diesem Fall die richtige Lösung ist [downgrade](http://kb.xamarin.com/customer/portal/articles/1699777) auf eine Version von Xamarin, die freigegeben wurde, bevor das Abonnement als abgelaufen gekennzeichnet. Wahlweise können Sie eine e-Mail senden [ hello@xamarin.com ](mailto:hello@xamarin.com) die letzte gültige Version für Ihr Abonnement anfordern. Wenn den Fehler nach dem Ausführen eines Downgrades weiterhin angezeigt wird, achten Sie darauf, dass Sie versuchen, die Lizenzdateien zu aktualisieren.

## <a name="additional-references"></a>Zusätzliche Referenzen

-   [Aktualisieren von Lizenzen](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md)
-   [Herunterstufen für Xamarin](http://kb.xamarin.com/customer/portal/articles/1699777-downgrading)
-   [Liste der Fehlercodes Xamarin.Android](~/android/troubleshooting/errors.md)
-   [Liste der Fehlercodes MonoTouch (Xamarin.iOS)](~/ios/troubleshooting/mtouch-errors.md)

### <a name="next-steps"></a>Nächste Schritte
Für weitere Unterstützung zu erhalten, wenden Sie sich an uns, oder bleibt dieses Problem auch nach der Nutzung der oben angegebenen Informationen finden Sie unter [welche Supportoptionen für Xamarin verfügbar sind?](~/cross-platform/troubleshooting/support-options.md) Informationen zu Kontaktoptionen Vorschläge, sowie zur die Datei eines neuen Fehlers bei Bedarf.
