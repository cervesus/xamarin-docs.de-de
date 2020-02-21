---
title: Xamarin.Forms-Features für Dual-Screen-Geräte
description: In diesem Artikel wird erklärt, wie sich für Dual-Screen-Geräte optimierte Apps mit Xamarin.Forms erstellen lassen.
ms.prod: xamarin
ms.assetid: f9906e83-f8ae-48f9-997b-e1540b96ee8e
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
ms.openlocfilehash: 41a7ef4e447bb71582264b4e73629566d3ffd4e7
ms.sourcegitcommit: 524fc148bad17272bda83c50775771daa45bfd7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2020
ms.locfileid: "77480512"
---
# <a name="xamarinforms-dual-screen"></a>Xamarin.Forms-Features für Dual-Screen-Geräte

![](~/media/shared/preview.png "This API is currently pre-release")

Mit Surface Duo (Android) und Surface Neo (Windows 10X) werden neue Muster für Touch-Anwendungen eingeführt. In Xamarin.Forms sind die Klassen `TwoPaneView` und `DualScreenInfo` enthalten, sodass Sie Apps für diese Geräte entwickeln können.

## <a name="dual-screen-patterns"></a>[Muster für zwei Bildschirme](design-patterns.md)

Wenn Sie überlegen, wie Sie die Möglichkeiten von zwei Bildschirmen auf Dual-Screen-Geräten am besten ausnutzen, können Sie den Artikel zu Entwurfsmustern lesen. Dort finden Sie Informationen zur besten Lösung für die Benutzeroberfläche Ihrer Anwendung.

## <a name="twopaneview"></a>[TwoPaneView](twopaneview.md)

Mit der `TwoPaneView`-Klasse von Xamarin.Forms, die vom gleichnamigen UWP-Steuerelement inspiriert ist, wird ein Layout eingeführt, das plattformübergreifend für Dual-Screen-Geräte optimiert ist.

## <a name="dualscreeninfo"></a>[DualScreenInfo](dual-screen-info.md)

Mithilfe der `DualScreenInfo`-Klasse können Sie z. B. bestimmen, in welchem Bereich Ihre Ansicht angezeigt wird, wie groß sie ist, welche Ausrichtung das Gerät hat und welchen Öffnungsgrad das Scharnier aufweist.

## <a name="dualscreenhelper"></a>[DualScreenHelper](dual-screen-helper.md)

Mit der `DualScreenHelper`-Klasse können Sie überprüfen, ob die Plattform das Öffnen eines neuen Fensters als Bild im Bild unterstützt. Auf einem Surface Neo-Gerät im Modus „Verfassen“ bedeutet dies, dass Sie ein neues Fenster mit der „WonderBar“ genannten Eingabezeile öffnen können.
