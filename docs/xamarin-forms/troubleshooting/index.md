---
title: Problembehandlung
description: Häufige Fehlerbedingungen und deren Behebung
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63291951-7375-4CBF-BCC3-2E4AD157A2C8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 93cab36b21e2fe73a0e6890140b5ebaeb32f7951
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760037"
---
# <a name="troubleshooting"></a>Problembehandlung

_Häufige Fehlerbedingungen und deren Behebung_

## <a name="error-unable-to-find-a-version-of-xamarinforms-compatible-with"></a>Fehler: "Es kann keine Version von xamarin. Forms gefunden werden, die mit"... "kompatibel ist.

Wenn Sie alle nuget-Pakete in einer xamarin. Forms-Projekt Mappe oder in einem xamarin. Forms Android-App-Projekt aktualisieren, können die folgenden Fehler im **Paket Konsolen** Fenster angezeigt werden:

```csharp
Attempting to resolve dependency 'Xamarin.Android.Support.v7.AppCompat (= 23.3.0.0)'.
Attempting to resolve dependency 'Xamarin.Android.Support.v4 (= 23.3.0.0)'.
Looking for updates for 'Xamarin.Android.Support.v7.MediaRouter'...
Updating 'Xamarin.Android.Support.v7.MediaRouter' from version '23.3.0.0' to '23.3.1.0' in project 'Todo.Droid'.
Updating 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0' to 'Xamarin.Android.Support.v7.MediaRouter 23.3.1.0' failed.
Unable to find a version of 'Xamarin.Forms' that is compatible with 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0'.
```

### <a name="what-causes-this-error"></a>Was verursacht diesen Fehler?

Visual Studio für Mac (oder Visual Studio) gibt möglicherweise an, dass Updates für xamarin. Forms-nuget-Paketname *und alle Abhängigkeiten*verfügbar sind. In Xamarin Studio könnte der Knoten **Pakete** der Projekt Mappe wie folgt aussehen (die Versionsnummern können abweichen):

![](images/updates-available.png "Ordner für Android-Projektpakete")

Dieser Fehler kann auftreten, wenn Sie versuchen, _alle_ Pakete zu aktualisieren.

Dies liegt daran, dass xamarin. Forms bei Android-Projekten, die auf eine Ziel-/Kompilierungs Version von Android 6,0 (API 23) oder niedriger festgelegt sind, eine feste Abhängigkeit von *bestimmten* Versionen der Android-Unterstützungspakete aufweist. Obwohl aktualisierte Versionen dieser Pakete verfügbar sein können, ist xamarin. Forms nicht notwendigerweise kompatibel mit Ihnen.

In diesem Fall sollten Sie _nur_ das **xamarin. Forms** -Paket aktualisieren, da dadurch sichergestellt wird, dass die Abhängigkeiten in kompatiblen Versionen verbleiben. Andere Pakete, die Sie Ihrem Projekt hinzugefügt haben, können auch einzeln aktualisiert werden, solange Sie nicht bewirken, dass die Android-Unterstützungspakete aktualisiert werden.

> [!NOTE]
> Wenn Sie xamarin. Forms 2.3.4 oder höher verwenden **und** die Ziel-/Kompilierungs Version Ihres Android-Projekts auf Android 7,0 (API 24) oder höher festgelegt ist, gelten die oben genannten Hardwareabhängigkeiten nicht mehr, und Sie können die Unterstützungspakete unabhängig von aktualisieren. Das xamarin. Forms-Paket.

### <a name="fix-remove-all-packages-and-re-add-xamarinforms"></a>Zusetzen Entfernen Sie alle Pakete, und fügen Sie xamarin. Forms erneut hinzu.

Wenn die **xamarin. Android. Support** -Pakete auf inkompatible Versionen aktualisiert wurden, ist die einfachste Lösung:

1. Löschen Sie alle nuget-Pakete manuell im Android-Projekt.
2. Fügen Sie das **xamarin. Forms** -Paket erneut hinzu.

Dadurch werden automatisch die *richtigen* Versionen der anderen Pakete heruntergeladen.

Ein Video zu diesem Prozess finden Sie in diesem [Forumsbeitrag](https://forums.xamarin.com/discussion/comment/170012/#Comment_170012).
