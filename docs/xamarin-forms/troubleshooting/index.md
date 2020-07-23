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
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5a84b7a5ca336b5823f1e0d2201e17cb4f152c27
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86930844"
---
# <a name="troubleshooting"></a>Problembehandlung

_Häufige Fehlerbedingungen und deren Behebung_

## <a name="error-unable-to-find-a-version-of-xamarinforms-compatible-with"></a>Fehler: "Es wurde keine Version gefunden, die Xamarin.Forms mit kompatibel ist..."

Wenn Sie alle nuget-Pakete in einer Projekt Mappe oder in einem Android-App-Projekt aktualisieren, können die folgenden Fehler im **Paket Konsolen** Fenster angezeigt werden Xamarin.Forms Xamarin.Forms :

```csharp
Attempting to resolve dependency 'Xamarin.Android.Support.v7.AppCompat (= 23.3.0.0)'.
Attempting to resolve dependency 'Xamarin.Android.Support.v4 (= 23.3.0.0)'.
Looking for updates for 'Xamarin.Android.Support.v7.MediaRouter'...
Updating 'Xamarin.Android.Support.v7.MediaRouter' from version '23.3.0.0' to '23.3.1.0' in project 'Todo.Droid'.
Updating 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0' to 'Xamarin.Android.Support.v7.MediaRouter 23.3.1.0' failed.
Unable to find a version of 'Xamarin.Forms' that is compatible with 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0'.
```

### <a name="what-causes-this-error"></a>Was verursacht diesen Fehler?

Visual Studio für Mac (oder Visual Studio) kann darauf hindeuten, dass Updates für den Xamarin.Forms nuget-Paketname *und alle Abhängigkeiten*verfügbar sind. In Xamarin Studio könnte der Knoten **Pakete** der Projekt Mappe wie folgt aussehen (die Versionsnummern können abweichen):

![Ordner für Android-Projektpakete](images/updates-available.png)

Dieser Fehler kann auftreten, wenn Sie versuchen, _alle_ Pakete zu aktualisieren.

Dies liegt daran, dass bei Android-Projekten, die auf eine Ziel-/Kompilierungs Version von Android 6,0 (API 23) oder niedriger festgelegt sind, Xamarin.Forms eine feste Abhängigkeit von *bestimmten* Versionen der Android-Unterstützungspakete besteht. Obwohl aktualisierte Versionen dieser Pakete verfügbar sein können, Xamarin.Forms ist nicht notwendigerweise mit Ihnen kompatibel.

In diesem Fall sollten Sie _nur_ das **Xamarin.Forms** Paket aktualisieren, da dadurch sichergestellt wird, dass die Abhängigkeiten in kompatiblen Versionen verbleiben. Andere Pakete, die Sie Ihrem Projekt hinzugefügt haben, können auch einzeln aktualisiert werden, solange Sie nicht bewirken, dass die Android-Unterstützungspakete aktualisiert werden.

> [!NOTE]
> Wenn Sie Xamarin.Forms 2.3.4 oder höher verwenden **und** die Ziel-/Kompilierungs Version Ihres Android-Projekts auf Android 7,0 (API 24) oder höher festgelegt ist, gelten die oben genannten Hardwareabhängigkeiten nicht mehr, und Sie können die Unterstützungspakete unabhängig vom Xamarin.Forms Paket aktualisieren.

### <a name="fix-remove-all-packages-and-re-add-xamarinforms"></a>Behebung: Entfernen Sie alle Pakete, und fügen Sie erneut hinzu.Xamarin.Forms

Wenn die **xamarin. Android. Support** -Pakete auf inkompatible Versionen aktualisiert wurden, ist die einfachste Lösung:

1. Löschen Sie alle nuget-Pakete manuell im Android-Projekt.
2. Fügen Sie das Paket erneut hinzu **Xamarin.Forms** .

Dadurch werden automatisch die *richtigen* Versionen der anderen Pakete heruntergeladen.

Ein Video zu diesem Prozess finden Sie in diesem [Forumsbeitrag](https://forums.xamarin.com/discussion/comment/170012/#Comment_170012).
