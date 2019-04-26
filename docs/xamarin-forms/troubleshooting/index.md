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
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60850438"
---
# <a name="troubleshooting"></a>Problembehandlung

_Häufige fehlerbedingungen und deren Behebung_

## <a name="error-unable-to-find-a-version-of-xamarinforms-compatible-with"></a>Fehler: "Kann nicht auf eine Version von Xamarin.Forms kompatibel mit suchen..."

Die folgenden Fehler können angezeigt werden, der **Paketkonsole** anzeigen, wenn alle Nuget-Pakete in einer Xamarin.Forms-Projektmappe oder in einer Xamarin.Forms-Android-app-Projekt zu aktualisieren:

```csharp
Attempting to resolve dependency 'Xamarin.Android.Support.v7.AppCompat (= 23.3.0.0)'.
Attempting to resolve dependency 'Xamarin.Android.Support.v4 (= 23.3.0.0)'.
Looking for updates for 'Xamarin.Android.Support.v7.MediaRouter'...
Updating 'Xamarin.Android.Support.v7.MediaRouter' from version '23.3.0.0' to '23.3.1.0' in project 'Todo.Droid'.
Updating 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0' to 'Xamarin.Android.Support.v7.MediaRouter 23.3.1.0' failed.
Unable to find a version of 'Xamarin.Forms' that is compatible with 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0'.
```

### <a name="what-causes-this-error"></a>Was verursacht diesen Fehler?

Visual Studio für Mac (oder Visual Studio) hinweisen, dass Updates für die Xamarin.Forms-Nuget-Paket verfügbar sind *und alle seine Abhängigkeiten*. In Xamarin Studio die Projektmappe des **Pakete** Knoten könnte wie folgt aussehen (Versionsnummern können abweichen):

![](images/updates-available.png "Ordner \"Pakete\" Android-Projekt")

Dieser Fehler kann auftreten, wenn Sie versuchen, aktualisieren Sie _alle_ der Pakete.

Dies ist, da mit Android-Projekte festgelegt werden, um eine Ziel-/kompilierversion für Android 6.0 (API 23) aus, oder Xamarin.Forms unten auf eine harte Abhängigkeit hat *bestimmte* Versionen von Android-Supportpakete. Obwohl aktualisierte Versionen dieser Pakete möglicherweise verfügbar ist, ist Xamarin.Forms nicht unbedingt kompatibel mit ihnen.

In diesem Fall sollten Sie aktualisieren _nur_ der **Xamarin.Forms** Verpacken, da so sichergestellt wird, dass die Abhängigkeiten auf kompatiblen Versionen bleiben. Andere Pakete, die Sie Ihrem Projekt hinzugefügt haben möglicherweise auch einzeln aktualisiert werden, solange sie nicht, dass die Android Supportpakete bewirken aktualisieren.


> [!NOTE]
> Bei Verwendung von Xamarin.Forms 2.3.4 oder höher **und** Ziel-/kompilierversion für Ihr Android-Projekt auf Android 7.0 (API 24) oder höher festgelegt ist, kommt die festen Abhängigkeiten, die nicht mehr die oben genannten Anwendung, und Sie können die Unterstützung aktualisieren Pakete, die unabhängig von der Xamarin.Forms-Paket.


### <a name="fix-remove-all-packages-and-re-add-xamarinforms"></a>Fix: Entfernen Sie alle Pakete, und fügen Sie Xamarin.Forms erneut hinzu

Wenn die **Xamarin.Android.Support** Pakete aktualisiert wurden, um inkompatible Versionen, die einfachste Lösung besteht darin:

1. Löschen Sie manuell alle Nuget-Pakete im Android-Projekt, klicken Sie dann
2. Erneutes Hinzufügen der **Xamarin.Forms** Paket.

Dadurch wird automatisch heruntergeladen. die *richtig* Versionen der anderen Pakete.

Erhalten Sie ein Video hierzu finden Sie in diesem [Foren Post](https://forums.xamarin.com/discussion/comment/170012/#Comment_170012).
