---
title: 'title: "Xamarin.Forms – Dual-Screen" description: "In diesem Leitfaden wird beschrieben, wie Sie Xamarin.Forms-Apps für Dual-Screen-Geräte erstellen."'
description: 'ms.prod: xamarin ms.assetid: f9906e83-f8ae-48f9-997b-e1540b96ee8e ms.technology: xamarin-forms author: davidortinau ms.author: daortin ms.date: 02/08/2020 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.prod: xamarin
ms.assetid: f9906e83-f8ae-48f9-997b-e1540b96ee8e
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: aeaaeb732adaea45446d6baf833027801abf4d2a
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2020
ms.locfileid: "84138904"
---
# <a name="xamarinforms-dual-screen"></a>Xamarin.Forms-Features für Dual-Screen-Geräte

![](~/media/shared/preview.png "This API is currently pre-release")

Mit Surface Duo (Android) und Surface Neo (Windows 10X) werden neue Muster für Touch-Anwendungen eingeführt. In Xamarin.Forms sind die Klassen `TwoPaneView` und `DualScreenInfo` enthalten, sodass Sie Apps für diese Geräte entwickeln können.

## <a name="dual-screen-design-patterns"></a>[Dual-Screen-Entwurfsmuster](design-patterns.md)

Wenn Sie überlegen, wie Sie die Möglichkeiten von zwei Bildschirmen auf Dual-Screen-Geräten am besten ausnutzen, können Sie den Artikel zu Entwurfsmustern lesen. Dort finden Sie Informationen zur besten Lösung für die Benutzeroberfläche Ihrer Anwendung.

## <a name="dual-screen-layout"></a>[Layout für zwei Bildschirme](twopaneview.md)

Mit der `TwoPaneView`-Klasse von Xamarin.Forms, die vom gleichnamigen UWP-Steuerelement inspiriert ist, wird ein plattformübergreifendes Layout eingeführt, das für Dual-Screen-Geräte optimiert ist.

## <a name="dual-screen-device-capabilities"></a>[Funktionen für Dual-Screen-Geräte](dual-screen-info.md)

Mithilfe der `DualScreenInfo`-Klasse können Sie z. B. bestimmen, in welchem Bereich Ihre Ansicht angezeigt wird, wie groß sie ist, welche Ausrichtung das Gerät hat und welchen Öffnungsgrad das Scharnier aufweist.

## <a name="dual-screen-platform-helpers"></a>[Plattformhilfsprogramme für zwei Bildschirme](dual-screen-helper.md)

Mit der `DualScreenHelper`-Klasse können Sie überprüfen, ob die Plattform das Öffnen eines neuen Fensters als Bild im Bild unterstützt. Auf einem Surface Neo-Gerät im Modus „Verfassen“ bedeutet dies, dass Sie ein neues Fenster mit der „WonderBar“ genannten Eingabezeile öffnen können.

## <a name="dual-screen-triggers"></a>[Trigger für zwei Bildschirme](triggers.md)

Der [`Xamarin.Forms.DualScreen`](xref:Xamarin.Forms.DualScreen)-Namespace enthält zwei Zustandstrigger, die eine [`VisualState`](xref:Xamarin.Forms.VisualState)-Änderung auslösen, wenn sich der Ansichtsmodus des angefügten Layouts oder Fensters ändert.
