---
title: Xamarin.Forms-Effekte
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a6206d2c561df74a01b7d7408e8d542f1e2189d3
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139333"
---
# <a name="xamarinforms-effects"></a>Xamarin.Forms-Effekte

_Xamarin.Forms-Benutzeroberflächen werden mit den nativen Steuerelementen der Zielplattform gerendert. Dadurch können Xamarin.Forms-Anwendungen ihr Erscheinungsbild auf den einzelnen Plattformen beibehalten. Durch Effekte können native Steuerelemente auf jeder Plattform angepasst werden, ohne dass ein benutzerdefinierter Renderer implementiert werden muss._

## <a name="introduction-to-effects"></a>[Einführung in Effekte](introduction.md)

Durch Effekte können native Steuerelemente auf jeder Plattform angepasst werden. Sie werden normalerweise für kleine Formatierungsänderungen verwendet. In diesem Artikel werden Effekte vorgestellt und die Unterschiede zwischen Effekten und benutzerdefinierten Renderern verdeutlicht. Darüber hinaus wird die `PlatformEffect`-Klasse beschrieben.

## <a name="creating-an-effect"></a>[Erstellen eines Effekts](creating.md)

Durch Effekte können Steuerelemente mühelos angepasst werden. In diesem Artikel wird veranschaulicht, wie Sie einen Effekt erstellen können, der die Hintergrundfarbe des [`Entry`](xref:Xamarin.Forms.Entry)-Steuerelements verändert, wenn das Steuerelement ausgewählt wird.

## <a name="passing-parameters-to-an-effect"></a>[Übergeben von Parametern an Effekte](passing-parameters/index.md)

Wenn Sie einen Effekt erstellen, der durch Parameter konfiguriert wird, können Sie den Effekt wiederverwenden. In diesen Artikeln wird veranschaulicht, wie Sie Eigenschaften verwenden können, um Parameter an einen Effekt zu übergeben, und wie Sie einen Parameter zur Laufzeit anpassen können.

## <a name="invoking-events-from-an-effect"></a>[Aufrufen von Ereignissen über einen Effekt](touch-tracking.md)

Effekte können Ereignisse aufrufen. In diesem Artikel erfahren Sie, wie Sie ein Ereignis erstellen können, das umfassende Fingerverfolgung implementiert und eine Anwendung über Drücken, Bewegen und Loslassen informiert.

## <a name="reusable-roundeffect"></a>[Wiederverwendbarer RoundEffect](reusable-roundeffect.md)

RoundEffect ist ein wiederverwendbarer Effekt, der auf jedes aus VisualElement stammende Steuerelement angewendet werden kann, um das Steuerelement als Kreis zu rendern. Dieser Effekt kann verwendet werden, um kreisförmige Bilder, kreisförmige Schaltflächen oder andere kreisförmige Steuerelemente zu erstellen.
