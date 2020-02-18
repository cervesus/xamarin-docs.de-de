---
title: Xamarin.Forms-Features für Dual-Screen-Geräte
description: In diesem Artikel wird erklärt, wie sich Apps mit Xamarin.Forms ganz einfach für Dual-Screen-Geräte optimieren lassen.
ms.prod: xamarin
ms.assetid: f9906e83-f8ae-48f9-997b-e1540b96ee8e
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
ms.openlocfilehash: 1f648297a608c592d90f2c70494ae4496fe73c0f
ms.sourcegitcommit: 07941cf9704ff88cf4087de5ebdea623ff54edb1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2020
ms.locfileid: "77145514"
---
# <a name="xamarinforms-dual-screen"></a>Xamarin.Forms-Features für Dual-Screen-Geräte

![](~/media/shared/preview.png "This API is currently pre-release")

Mit Surface Duo (Android) und Surface Neo (Windows 10X) werden neue Muster für Touch-Anwendungen eingeführt. Damit Sie die neuen Möglichkeiten in vollem Umfang nutzen können, führt Xamarin.Forms `TwoPaneView` und `DualScreenInfo` ein.

## <a name="dual-screen-patternsdesign-patternsmd"></a>[Muster für zwei Bildschirme](design-patterns.md)

Wenn Sie überlegen, wie Sie die Möglichkeiten von zwei Bildschirmen auf Dual-Screen-Geräten am besten ausnutzen, können Sie den Artikel zu Entwurfsmustern lesen. Dort finden Sie Informationen zur besten Lösung für Ihre Anwendungsschnittstelle.

## <a name="twopaneviewtwopaneviewmd"></a>[TwoPaneView](twopaneview.md)

Mit der `TwoPaneView`-Klasse von Xamarin.Forms, die vom gleichnamigen UWP-Steuerelement inspiriert ist, wird ein Layout eingeführt, das plattformübergreifend für Dual-Screen-Geräte optimiert ist.

## <a name="dualscreeninfodual-screen-infomd"></a>[DualScreenInfo](dual-screen-info.md)

Damit Sie die Möglichkeiten, die Dual-Screen-Geräte Ihnen bieten, voll ausnutzen können, sollten Sie wissen, in welchem Bereich Ihre Ansicht angezeigt wird, wie groß sie ist, welche Ausrichtung das Gerät hat und welchen Öffnungsgrad das Scharnier aufweist. `DualScreenInfo` stellt all diese Informationen bereit.

## <a name="dualscreenhelperdual-screen-helpermd"></a>[DualScreenHelper](dual-screen-helper.md)
Mit `DualScreenHelper` können Sie überprüfen, ob die Plattform das Öffnen eines neuen Fensters als Bild im Bild unterstützt. Auf einem Surface Neo-Gerät im Modus „Verfassen“ bedeutet dies, das Sie ein neues Fenster mit der „Wonderbar“ genannten Eingabezeile öffnen können.