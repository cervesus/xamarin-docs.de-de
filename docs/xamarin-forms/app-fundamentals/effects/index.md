---
title: Xamarin.Forms-Auswirkungen
description: Mit Effekten können native Steuerelemente auf den einzelnen Plattformen angepasst werden, ohne auf eine Implementierung des benutzerdefinierten Renderer zurückgreifen.
ms.prod: xamarin
ms.assetid: 8AF168A7-4CD9-4603-B961-15B8B1543784
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: 9d859377c40c6fca07e140c50da46d8f30aaae04
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994457"
---
# <a name="xamarinforms-effects"></a>Xamarin.Forms-Auswirkungen

_Xamarin.Forms-Benutzeroberflächen werden gerendert mit den nativen Steuerelementen der Zielplattform, sodass Xamarin.Forms-Anwendungen das entsprechende Aussehen und Verhalten für die einzelnen Plattformen beibehalten werden sollen. Mit Effekten können native Steuerelemente auf den einzelnen Plattformen angepasst werden, ohne auf eine Implementierung des benutzerdefinierten Renderer zurückgreifen._

## <a name="introduction-to-effectsintroductionmd"></a>[Einführung in die Effekte](introduction.md)

Können durch Effekte native Steuerelemente auf den einzelnen Plattformen angepasst werden, und für kleine Formatierungsänderungen in der Regel verwendet werden. Dieser Artikel bietet eine Einführung in die Effekte, wird beschrieben, die Grenze zwischen Effekte und benutzerdefinierte Renderer und beschreibt die `PlatformEffect` Klasse.

## <a name="creating-an-effectcreatingmd"></a>[Erstellen einen Effekt](creating.md)

Auswirkungen die Anpassung eines Steuerelements zu vereinfachen. In diesem Artikel wird veranschaulicht, wie Sie einen Effekt zu erstellen, die die Farbe des Hintergrunds Ändern der [ `Entry` ](xref:Xamarin.Forms.Entry) steuern, wenn das Steuerelement den Fokus erhält.

## <a name="passing-parameters-to-an-effectpassing-parametersindexmd"></a>[Übergeben von Parametern an einen Effekt](passing-parameters/index.md)

Erstellen einen Effekt, der mithilfe von Parametern konfiguriert ist, ermöglicht den Effekt, der wiederverwendet werden. Dieser Artikel veranschaulicht die Verwendung von Eigenschaften zum Übergeben von Parametern an eine Wirkung und einen Parameter zur Laufzeit ändern.

## <a name="invoking-events-from-an-effecttouch-trackingmd"></a>[Aufrufen von Ereignissen aus eines Effekts](touch-tracking.md)

Effekte können Ereignisse aufrufen. In diesem Artikel zeigt, wie ein Ereignis zu erstellen, die Low-Level-Multitouch-Finger-nachverfolgung implementiert, und eine Anwendung für die Fingereingabe klickt, wechselt und Versionen signalisiert, wird.
