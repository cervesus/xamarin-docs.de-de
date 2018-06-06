---
title: Benutzeroberflächen in iOS
description: Dieses enthält Dokumentenlinks zu Anleitungen, die zum Erstellen von Benutzeroberflächen in Xamarin.iOS app beschreiben. Der verknüpfte Handbücher Behandeln der Darstellung-API erstellen Benutzeroberflächenobjekte, Layoutoptionen und vieles mehr.
ms.prod: xamarin
ms.assetid: 1BB46561-F503-491E-A27C-7878E7EBE00B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: a51d3f57106a282ed72b45dedf356739244e247f
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790366"
---
# <a name="user-interfaces-in-ios"></a>Benutzeroberflächen in iOS

## <a name="appearance-apiintroduction-to-the-appearance-apimd"></a>[Darstellungs-API](introduction-to-the-appearance-api.md)

iOS kann viele visuellen Attribute eines Designs mithilfe der UIAppearance-APIs werden die Steuerelemente der Benutzeroberfläche.

## <a name="creating-user-interface-objectsiosuser-interfaceios-uicreating-ui-objectsmd"></a>[Erstellen von Benutzeroberflächenobjekten](~/ios/user-interface/ios-ui/creating-ui-objects.md)

Apple-Gruppen im Zusammenhang mit Funktionen in "Frameworks" die Xamarin.iOS Namespaces entsprechen. `UIKit` ist der Namespace, der alle Steuerelemente der Benutzeroberfläche für iOS enthält.

## <a name="layout-optionsiosuser-interfaceios-uilayout-optionsmd"></a>[Layoutoptionen](~/ios/user-interface/ios-ui/layout-options.md)

Es gibt zwei unterschiedliche Mechanismen für das Layout steuern, wenn eine Sicht geändert oder gedreht wird: Automatisches Anpassen der Größe und Autolayout.

## <a name="providing-haptic-feedbackiosuser-interfaceios-uihaptic-feedbackmd"></a>[Übermitteln von haptischem Feedback](~/ios/user-interface/ios-ui/haptic-feedback.md)

Dieser Artikel behandelt die neuen Typen für haptic Feedback in iOS 10 und deren Implementierung in Xamarin.iOS verfügbar.

## <a name="working-with-the-ui-threadiosuser-interfaceios-uiui-threadmd"></a>[Arbeiten mit dem UI-Thread](~/ios/user-interface/ios-ui/ui-thread.md)

Den Code sollte nur Änderungen an den Benutzer Benutzeroberflächen-Steuerelemente aus dem Hauptknoten (oder UI)-Thread zu gestalten. Alle UI-Updates, die auf einem anderen Thread (z. B. einen Rückruf oder Hintergrund-Thread) auftreten möglicherweise nicht auf dem Bildschirm gerendert abrufen, oder Sie können auch verursacht einen Absturz.




