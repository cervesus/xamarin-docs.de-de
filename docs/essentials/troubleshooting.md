---
title: 'Xamarin.Essentials: Problembehandlung'
description: Dieses Dokument beschreibt So beheben Sie Probleme bei der Entwicklung mit der Xamarin.Essentials-Bibliothek.
ms.assetid: 2E474FAF-F841-4E3C-B815-F7ABD8EE3361
author: jamesmontemagno
ms.author: jamont
ms.date: 06/26/2018
ms.openlocfilehash: 8cb18ab029d2fd161c60fda7e130f319b8f0c3ab
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947373"
---
# <a name="xamarinessentials-troubleshooting"></a>Xamarin.Essentials: Problembehandlung

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

## <a name="error-version-conflict-detected-for-xamarinandroidsupportcompat"></a>Fehler: Versionskonflikt für Xamarin.Android.Support.Compat erkannt

Der folgende Fehler kann auftreten, wenn es sich bei NuGet-Pakete aktualisieren (oder ein neues Paket hinzufügen) mit einer Xamarin.Forms-Projekt, das Xamarin.Essentials verwendet:

```
NU1107: Version conflict detected for Xamarin.Android.Support.Compat. Reference the package directly from the project to resolve this issue. 
 MyApp -> Xamarin.Essentials 0.8.0-preview -> Xamarin.Android.Support.CustomTabs 27.0.2.1 -> Xamarin.Android.Support.Compat (= 27.0.2.1) 
 MyApp -> Xamarin.Forms 3.1.0.583944 -> Xamarin.Android.Support.v4 25.4.0.2 -> Xamarin.Android.Support.Compat (= 25.4.0.2).
```

Das Problem ist nicht übereinstimmende Abhängigkeiten für die zwei NuGet-Pakete. Dies kann aufgelöst werden, indem Sie manuell eine bestimmte Version der Abhängigkeit hinzufügen (in diesem Fall **Xamarin.Android.Support.Compat**), können beide unterstützen.

Zu diesem Zweck des NuGet-Pakets, das die Quelle des Konflikts manuell hinzufügen, und Verwenden der **Version** Liste, um eine bestimmte Version auszuwählen. Derzeit wird Version 27.0.2.1 Xamarin.Android.Support.Compat & Xamarin.Android.Support.Core.Util NuGet diesen Fehler zu beheben.

Finden Sie unter [in diesem Blogbeitrag](https://redth.codes/how-to-fix-the-dreaded-version-conflict-nuget-error-in-your-xamarin-android-projects/) für Weitere Informationen und ein Video zur Verwendung zur Behebung des Problems.

Wenn Sie auf Probleme oder suchen, die ein Fehler melden Sie diesen auf den [Xamarin.Essentials-GitHub-Repository](http://github.com/xamarin/Essentials).
