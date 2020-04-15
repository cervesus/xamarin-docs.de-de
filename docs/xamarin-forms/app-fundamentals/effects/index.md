---
title: Xamarin.Forms-Effekte
description: Durch Effekte können native Steuerelemente auf jeder Plattform angepasst werden, ohne dass ein benutzerdefinierter Renderer implementiert werden muss.
ms.prod: xamarin
ms.assetid: 8AF168A7-4CD9-4603-B961-15B8B1543784
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: 2de1d1dd065a01bb457ebf03acdc0c01529abf7b
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73083835"
---
# <a name="xamarinforms-effects"></a>Xamarin.Forms-Effekte

_Xamarin.Forms-Benutzeroberflächen werden mit den nativen Steuerelementen der Zielplattform gerendert. Dadurch können Anwendungen von Xamarin.Forms das Erscheinungsbild der einzelnen Plattformen beibehalten. Durch Effekte können native Steuerelemente auf jeder Plattform angepasst werden, ohne dass ein benutzerdefinierter Renderer implementiert werden muss._

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
