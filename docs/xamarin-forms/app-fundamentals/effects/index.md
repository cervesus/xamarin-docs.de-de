---
title: Xamarin.Forms Effekte
description: Effekte ermöglichen die native Steuerelemente für jede Plattform angepasst werden, ohne dass hierfür ein benutzerdefinierter Renderer-Implementierung.
ms.prod: xamarin
ms.assetid: 8AF168A7-4CD9-4603-B961-15B8B1543784
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: bc1c600aa6d5ede079fe0af83370393d4f0e298f
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35239695"
---
# <a name="xamarinforms-effects"></a>Xamarin.Forms Effekte

_Xamarin.Forms Benutzeroberflächen werden mit den systemeigenen Steuerelementen der Zielplattform, behalten die entsprechenden Aussehen und Verhalten für jede Plattform von Xamarin.Forms Anwendungen gestatten gerendert. Effekte ermöglichen die native Steuerelemente für jede Plattform angepasst werden, ohne dass hierfür ein benutzerdefinierter Renderer-Implementierung._

## <a name="introduction-to-effectsintroductionmd"></a>[Einführung in die Effekte](introduction.md)

Effekte ermöglichen die native Steuerelemente für jede Plattform angepasst werden, und für kleine Styling Änderungen in der Regel verwendet werden. Dieser Artikel bietet eine Einführung in die Auswirkungen, erläutert die Grenze zwischen Auswirkungen und benutzerdefinierten Renderern und beschreibt die `PlatformEffect` Klasse.

## <a name="creating-an-effectcreatingmd"></a>[Erstellen eines Effekts](creating.md)

Effekte vereinfachen die Anpassung eines Steuerelements an. In diesem Artikel veranschaulicht, wie einen Effekt, die Farbe des Hintergrunds ändert die [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) steuern, wenn das Steuerelement den Fokus erhält.

## <a name="passing-parameters-to-an-effectpassing-parametersindexmd"></a>[Übergeben von Parametern an eines Effekts](passing-parameters/index.md)

Erstellen einen Effekt, der über den Parameter konfiguriert ist, können den Effekt, der wiederverwendet werden. Dieser Artikel veranschaulicht die Verwendung von Eigenschaften zum Übergeben von Parametern an eine Auswirkung und einen Parameter zur Laufzeit ändern.

## <a name="invoking-events-from-an-effecttouch-trackingmd"></a>[Aufrufen von Ereignissen aus eines Effekts](touch-tracking.md)

Effekte können Ereignisse aufrufen. In diesem Artikel zeigt, wie ein Ereignis zu erstellen, implementiert eine nachverfolgung auf niedriger Ebene Multitouch Finger und eine Anwendung für die Fingereingabe drücken, wechselt und Versionen signalisiert, wird.
