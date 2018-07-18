---
title: Problembehandlung
description: Häufige fehlerbedingungen und deren Behebung
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63291951-7375-4CBF-BCC3-2E4AD157A2C8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: fbe4fb6fce52636b59a9637ee0150c4c19fcc9da
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30788522"
---
# <a name="troubleshooting"></a>Problembehandlung

_Häufige fehlerbedingungen und deren Behebung_

## <a name="error-unable-to-find-a-version-of-xamarinforms-compatible-with"></a>Fehler: "Es konnte eine Version von Xamarin.Forms kompatibel mit gefunden..."

Die folgenden Fehler können angezeigt werden, der **Paket Konsole** Fenster, wenn alle Nuget-Pakete in einer Xamarin.Forms-Projektmappe oder in einer Xamarin.Forms-Android-app-Projekt zu aktualisieren:

```csharp
Attempting to resolve dependency 'Xamarin.Android.Support.v7.AppCompat (= 23.3.0.0)'.
Attempting to resolve dependency 'Xamarin.Android.Support.v4 (= 23.3.0.0)'.
Looking for updates for 'Xamarin.Android.Support.v7.MediaRouter'...
Updating 'Xamarin.Android.Support.v7.MediaRouter' from version '23.3.0.0' to '23.3.1.0' in project 'Todo.Droid'.
Updating 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0' to 'Xamarin.Android.Support.v7.MediaRouter 23.3.1.0' failed.
Unable to find a version of 'Xamarin.Forms' that is compatible with 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0'.
```

### <a name="what-causes-this-error"></a>Was bewirkt, dass dieser Fehler?

Visual Studio für Mac (oder Visual Studio) hinweisen, dass Updates für die Xamarin.Forms-NuGet-Paket verfügbar sind *und alle seine Abhängigkeiten*. In Xamarin Studio die Projektmappe des **Pakete** Knoten könnte wie folgt aussehen (die Versionsnummern können abweichen):

![](images/updates-available.png "Android-Projekt Pakete (Ordner)")

Dieser Fehler kann auftreten, wenn Sie versuchen, aktualisieren Sie _alle_ der Pakete.

Dies liegt daran mit Android Projekten, auf ein Ziel/Compile-Version von Android 6.0 (API 23 festgelegt) oder unten Xamarin.Forms schwer abhängt *bestimmte* Supportpaketen für die Android-Versionen. Obwohl aktualisierte Versionen von diesen Paketen verfügbar sind, ist Xamarin.Forms nicht unbedingt mit diesen kompatibel.

In diesem Fall sollten Sie aktualisieren _nur_ der **Xamarin.Forms** Verpacken, da so sichergestellt wird, dass die Abhängigkeiten auf kompatible Versionen verbleiben. Andere Pakete, die Sie dem Projekt hinzugefügt haben können auch einzeln aktualisiert werden, solange sie nicht, dass die Android-Unterstützung Pakete bewirken zu aktualisieren.


> [!NOTE]
> Bei Verwendung von Xamarin.Forms 2.3.4 oder höher **und** Ihres Android-Projekts Ziel/Kompilierung Version auf Android 7.0 (24-API) oder höher festgelegt ist, gelten die oben genannten harte Abhängigkeiten und Sie möglicherweise die Unterstützung aktualisieren Pakete, die unabhängig von der Xamarin.Forms-Paket.


### <a name="fix-remove-all-packages-and-re-add-xamarinforms"></a>Fix: Entfernen Sie alle Pakete und erneut fügen Sie hinzu, Xamarin.Forms

Wenn die **Xamarin.Android.Support** Pakete aktualisiert wurden, um nicht kompatible Versionen, die einfachste Lösung besteht darin:

1. Löschen Sie manuell die NuGet-Pakete im Android-Projekt, klicken Sie dann
2. Erneutes Hinzufügen der **Xamarin.Forms** Paket.

Dies wird automatisch herunterladen, die *richtig* Versionen der anderen Pakete.

Um ein Video dieses Prozesses angezeigt wird, finden Sie in diesen [Foren Post](https://forums.xamarin.com/discussion/comment/170012/#Comment_170012).
