---
title: Xamarin.Forms-Features für Dual-Screen-Geräte
description: In diesem Artikel wird erklärt, wie sich für Dual-Screen-Geräte optimierte Apps mit Xamarin.Forms erstellen lassen.
ms.prod: xamarin
ms.assetid: f9906e83-f8ae-48f9-997b-e1540b96ee8e
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
ms.openlocfilehash: 344b6293090ffa4281ea6351f7f176a5be37e5bd
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "78165552"
---
# <a name="xamarinforms-dual-screen"></a>Xamarin.Forms-Features für Dual-Screen-Geräte

![](~/media/shared/preview.png "This API is currently pre-release")

Mit Surface Duo (Android) und Surface Neo (Windows 10X) werden neue Muster für Touch-Anwendungen eingeführt. In Xamarin.Forms sind die Klassen `TwoPaneView` und `DualScreenInfo` enthalten, sodass Sie Apps für diese Geräte entwickeln können.

## <a name="dual-screen-design-patterns"></a>[Dual-Screen-Entwurfsmuster](design-patterns.md)

Wenn Sie überlegen, wie Sie die Möglichkeiten von zwei Bildschirmen auf Dual-Screen-Geräten am besten ausnutzen, können Sie den Artikel zu Entwurfsmustern lesen. Dort finden Sie Informationen zur besten Lösung für die Benutzeroberfläche Ihrer Anwendung.

## <a name="dual-screen-layout"></a>[Layout für zwei Bildschirme](twopaneview.md)

Mit der `TwoPaneView`-Klasse von Xamarin.Forms, die vom gleichnamigen UWP-Steuerelement inspiriert ist, wird ein Layout eingeführt, das plattformübergreifend für Dual-Screen-Geräte optimiert ist.

## <a name="dual-screen-device-capabilities"></a>[Funktionen für Dual-Screen-Geräte](dual-screen-info.md)

Mithilfe der `DualScreenInfo`-Klasse können Sie z. B. bestimmen, in welchem Bereich Ihre Ansicht angezeigt wird, wie groß sie ist, welche Ausrichtung das Gerät hat und welchen Öffnungsgrad das Scharnier aufweist.

## <a name="dual-screen-platform-helpers"></a>[Plattformhilfsprogramme für zwei Bildschirme](dual-screen-helper.md)

Mit der `DualScreenHelper`-Klasse können Sie überprüfen, ob die Plattform das Öffnen eines neuen Fensters als Bild im Bild unterstützt. Auf einem Surface Neo-Gerät im Modus „Verfassen“ bedeutet dies, dass Sie ein neues Fenster mit der „WonderBar“ genannten Eingabezeile öffnen können.

## <a name="dual-screen-triggers"></a>[Trigger für zwei Bildschirme](triggers.md)

Der [`Xamarin.Forms.DualScreen`](xref:Xamarin.Forms.DualScreen)-Namespace enthält zwei Zustandstrigger, die eine [`VisualState`](xref:Xamarin.Forms.VisualState)-Änderung auslösen, wenn sich der Ansichtsmodus des angefügten Layouts oder Fensters ändert.
