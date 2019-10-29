---
title: Benutzeroberflächen in iOS
description: Dieses Dokument ist mit Anleitungen verknüpft, die beschreiben, wie Benutzeroberflächen in der xamarin. IOS-App erstellt werden. Die verknüpften Handbücher umfassen die Darstellungs-API, das Erstellen von Benutzeroberflächen Objekten, Layoutoptionen und vieles mehr.
ms.prod: xamarin
ms.assetid: 1BB46561-F503-491E-A27C-7878E7EBE00B
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/14/2017
ms.openlocfilehash: 325898b3c934e25ae1610a3437f787476dcd22cb
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73003342"
---
# <a name="user-interfaces-in-ios"></a>Benutzeroberflächen in iOS

## <a name="appearance-apiintroduction-to-the-appearance-apimd"></a>[Darstellungs-API](introduction-to-the-appearance-api.md)

IOS ermöglicht das Design vieler visueller Attribute der Steuerelemente der Benutzeroberfläche mithilfe der uiappearance-APIs.

## <a name="creating-user-interface-objectsiosuser-interfaceios-uicreating-ui-objectsmd"></a>[Erstellen von Benutzeroberflächenobjekten](~/ios/user-interface/ios-ui/creating-ui-objects.md)

Apple gruppiert verwandte Funktionen in "Frameworks", die xamarin. IOS-Namespaces entsprechen. `UIKit` ist der Namespace, der alle Benutzeroberflächen-Steuerelemente für IOS enthält.

## <a name="layout-optionsiosuser-interfaceios-uilayout-optionsmd"></a>[Layoutoptionen](~/ios/user-interface/ios-ui/layout-options.md)

Es gibt zwei verschiedene Mechanismen, um das Layout zu steuern, wenn die Größe einer Ansicht geändert oder gedreht wird: "autoSizing" und "AutoLayout".

## <a name="providing-haptic-feedbackiosuser-interfaceios-uihaptic-feedbackmd"></a>[Übermitteln von haptischem Feedback](~/ios/user-interface/ios-ui/haptic-feedback.md)

In diesem Artikel werden die neuen Typen von willkürlich verfügbarem Feedback in ios 10 und deren Implementierung in xamarin. IOS behandelt.

## <a name="working-with-the-ui-threadiosuser-interfaceios-uiui-threadmd"></a>[Arbeiten mit dem UI-Thread](~/ios/user-interface/ios-ui/ui-thread.md)

Der Code sollte nur Änderungen an den Steuerelementen der Benutzeroberfläche vom Hauptthread (oder UI) aus vornehmen. Alle Aktualisierungen der Benutzeroberfläche, die in einem anderen Thread (z. b. einem Rückruf oder einem Hintergrund Thread) auftreten, werden möglicherweise nicht auf dem Bildschirm gerendert oder können sogar zu einem Absturz führen.
