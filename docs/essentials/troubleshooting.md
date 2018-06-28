---
title: 'Xamarin.Essentials: Problembehandlung'
description: Dieses Dokument beschreibt die Behebung von Problemen bei der Entwicklung mit der Xamarin.Essentials-Bibliothek.
ms.assetid: 2E474FAF-F841-4E3C-B815-F7ABD8EE3361
author: jamesmontemagno
ms.author: jamont
ms.date: 06/26/2018
ms.openlocfilehash: 3dba315aec2475cb334110ba7555f773f4165aa1
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066733"
---
# <a name="xamarinessentials-troubleshooting"></a>Xamarin.Essentials: Problembehandlung

![Vorabversion NuGet](~/media/shared/pre-release.png)

## <a name="error-version-conflict-detected-for-xamarinandroidsupportcompat"></a>Fehler: Versionskonflikt für Xamarin.Android.Support.Compat erkannt

Der folgende Fehler kann auftreten, wenn das Aktualisieren von NuGet-Pakete (oder Hinzufügen eines neuen Pakets) mit einer Xamarin.Forms-Projekt, Xamarin.Essentials verwendet:

```
NU1107: Version conflict detected for Xamarin.Android.Support.Compat. Reference the package directly from the project to resolve this issue. 
 MyApp -> Xamarin.Essentials 0.7.0.17-preview -> Xamarin.Android.Support.CustomTabs 27.0.2 -> Xamarin.Android.Support.Compat (= 27.0.2) 
 MyApp -> Xamarin.Forms 3.1.0.583944 -> Xamarin.Android.Support.v4 25.4.0.2 -> Xamarin.Android.Support.Compat (= 25.4.0.2).
```

Das Problem ist nicht übereinstimmende Abhängigkeiten für die zwei NuGets. Dies kann aufgelöst werden, indem Sie manuell eine bestimmte Version der Abhängigkeit hinzufügen (in diesem Fall **Xamarin.Android.Support.Compat**), können beide unterstützen.

Zu diesem Zweck fügen Sie den NuGet, der die Quelle des Konflikts manuell hinzu, und verwenden die **Version** Liste aus, um eine bestimmte Version auszuwählen. Derzeit wird die Xamarin.Android.Support.Compat NuGet-Version 27.0.2 diesen Fehler zu beheben.

Verweisen auf [diesem Blogbeitrag](https://redth.codes/how-to-fix-the-dreaded-version-conflict-nuget-error-in-your-xamarin-android-projects/) für Weitere Informationen und ein Video zum Lösen des Problems.

Wenn treten ggf. Probleme oder suchen, die ein Fehler melden sie bitte auf die [Xamarin.Essentials GitHub-Repository](http://github.com/xamarin/Essentials).
