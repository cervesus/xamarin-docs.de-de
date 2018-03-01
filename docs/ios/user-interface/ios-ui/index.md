---
title: "IOS-Benutzeroberfläche"
description: "Erläutert das Arbeiten mit iOS-Benutzeroberfläche in einem Xamarin.iOS-app."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2B3E45FA-C30F-D708-0E8F-3EE02BD1A867
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 831ddfff7e05c391472b280095564f90528369ff
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="user-interface-in-ios"></a>IOS-Benutzeroberfläche

## <a name="appearance-apiintroduction-to-the-appearance-apimd"></a>[API-Darstellung](introduction-to-the-appearance-api.md)

iOS kann viele visuellen Attribute eines Designs mithilfe der UIAppearance-APIs werden die Steuerelemente der Benutzeroberfläche.

## <a name="creating-user-interface-objectsiosuser-interfaceios-uicreating-ui-objectsmd"></a>[Erstellen von Benutzeroberflächenobjekten](~/ios/user-interface/ios-ui/creating-ui-objects.md)

Apple-Gruppen im Zusammenhang mit Funktionen in "Frameworks" die Xamarin.iOS Namespaces entsprechen. `UIKit` ist der Namespace, der alle Steuerelemente der Benutzeroberfläche für iOS enthält.

## <a name="layout-optionsiosuser-interfaceios-uilayout-optionsmd"></a>[Layoutoptionen](~/ios/user-interface/ios-ui/layout-options.md)

Es gibt zwei unterschiedliche Mechanismen für das Layout steuern, wenn eine Sicht geändert oder gedreht wird: Automatisches Anpassen der Größe und Autolayout.

## <a name="providing-haptic-feedbackiosuser-interfaceios-uihaptic-feedbackmd"></a>[Haptic Übermitteln von Feedback](~/ios/user-interface/ios-ui/haptic-feedback.md)

Dieser Artikel behandelt die neuen Typen für haptic Feedback in iOS 10 und deren Implementierung in Xamarin.iOS verfügbar.

## <a name="working-with-the-ui-threadiosuser-interfaceios-uiui-threadmd"></a>[Arbeiten mit der UI-Thread](~/ios/user-interface/ios-ui/ui-thread.md)

Den Code sollte nur Änderungen an den Benutzer Benutzeroberflächen-Steuerelemente aus dem Hauptknoten (oder UI)-Thread zu gestalten. Alle UI-Updates, die auf einem anderen Thread (z. B. einen Rückruf oder Hintergrund-Thread) auftreten möglicherweise nicht auf dem Bildschirm gerendert abrufen, oder Sie können auch verursacht einen Absturz.




