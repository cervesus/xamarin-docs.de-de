---
title: 'Xamarin.Essentials: Problembehandlung'
description: In diesem Artikel wird beschrieben, wie Sie auftretende Fehler beim Entwickeln mit der Bibliothek Xamarin.Essentials beheben.
ms.assetid: 2E474FAF-F841-4E3C-B815-F7ABD8EE3361
author: jamesmontemagno
ms.author: jamont
ms.date: 01/06/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 67a060667cef5f96fa864aa2db3356ac5b9b2dfb
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137123"
---
# <a name="xamarinessentials-troubleshooting"></a>Xamarin.Essentials: Problembehandlung

## <a name="error-version-conflict-detected-for-xamarinandroidsupportcompat"></a>Fehler: Bei Xamarin.Android.Support.Compat wurde ein Versionskonflikt festgestellt.

Der folgende Fehler kann auftreten, wenn Sie mit einem Xamarin.Forms-Projekt,
für das Xamarin.Essentials verwendet wird, NuGet-Pakete aktualisieren (oder ein neues Paket hinzufügen):

```error
NU1107: Version conflict detected for Xamarin.Android.Support.Compat. Reference the package directly from the project to resolve this issue.
 MyApp -> Xamarin.Essentials 1.3.1 -> Xamarin.Android.Support.CustomTabs 28.0.0.3 -> Xamarin.Android.Support.Compat (= 28.0.0.3)
 MyApp -> Xamarin.Forms 3.1.0.583944 -> Xamarin.Android.Support.v4 25.4.0.2 -> Xamarin.Android.Support.Compat (= 25.4.0.2).
```

Das Problem liegt bei den nicht übereinstimmenden Abhängigkeiten der zwei NuGet-Pakete. Durch das Hinzufügen einer bestimmten Version der Abhängigkeit (in diesem Fall **Xamarin.Android.Support.Compat**), die beides unterstützt, kann das Problem behoben werden.

Fügen Sie dazu das NuGet-Paket, welches die Quelle des Konflikts darstellt, manuell hinzu, und verwenden Sie die **Versionsliste**, um eine bestimmte Version auszuwählen. Aktuell behebt die Version 28.0.0.3 der NuGet-Pakete Xamarin.Android.Support.Compat und Xamarin.Android.Support.Core.Util diesen Fehler.

Weitere Informationen finden Sie [in diesem Blogbeitrag](https://redth.codes/how-to-fix-the-dreaded-version-conflict-nuget-error-in-your-xamarin-android-projects/) und in einem Video zum Lösen des Problems.

Sollten bei Ihnen Probleme oder Fehler auftreten, würden wir uns freuen, wenn Sie dies im [GitHub-Repository für Xamarin.Essentials](https://github.com/xamarin/Essentials) melden.
