---
title: Benutzeroberflächen in iOS
description: Dieses Dokument enthält Links zu Leitfäden, die zum Erstellen von Benutzeroberflächen in xamarin IOS-app beschrieben. Die verknüpften Handbüchern behandelt die Darstellung-API erstellen, Benutzeroberflächenobjekte, Layoutoptionen und mehr.
ms.prod: xamarin
ms.assetid: 1BB46561-F503-491E-A27C-7878E7EBE00B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/14/2017
ms.openlocfilehash: efb88ada8a4b4c36dd49de137eb64acd63552968
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61382335"
---
# <a name="user-interfaces-in-ios"></a>Benutzeroberflächen in iOS

## <a name="appearance-apiintroduction-to-the-appearance-apimd"></a>[Darstellungs-API](introduction-to-the-appearance-api.md)

iOS kann viele visuellen Attribute die Steuerelemente der Benutzeroberfläche zum Design mithilfe der UIAppearance-APIs.

## <a name="creating-user-interface-objectsiosuser-interfaceios-uicreating-ui-objectsmd"></a>[Erstellen von Benutzeroberflächenobjekten](~/ios/user-interface/ios-ui/creating-ui-objects.md)

Apple-Gruppen im Zusammenhang mit Funktionen in "Frameworks" die Xamarin.iOS-Namespaces entsprechen. `UIKit` ist der Namespace, der alle Steuerelemente der Benutzeroberfläche für iOS enthält.

## <a name="layout-optionsiosuser-interfaceios-uilayout-optionsmd"></a>[Layoutoptionen](~/ios/user-interface/ios-ui/layout-options.md)

Es gibt zwei unterschiedliche Mechanismen für die Steuerung des Layouts, wenn eine Sicht geändert oder gedreht wird: Automatisches Anpassen der Größe und automatisches Layout.

## <a name="providing-haptic-feedbackiosuser-interfaceios-uihaptic-feedbackmd"></a>[Übermitteln von haptischem Feedback](~/ios/user-interface/ios-ui/haptic-feedback.md)

Dieser Artikel behandelt die neuen Typen für das Übermitteln von haptischem Feedback in iOS 10 und implementieren Sie diese in Xamarin.iOS zur Verfügung.

## <a name="working-with-the-ui-threadiosuser-interfaceios-uiui-threadmd"></a>[Arbeiten mit dem UI-Thread](~/ios/user-interface/ios-ui/ui-thread.md)

Ihr Code sollte nur Änderungen an den Benutzer Steuerelemente der Benutzeroberfläche aus der primären (oder UI) Thread vornehmen. Alle Aktualisierungen der Benutzeroberfläche, die auf einem anderen Thread (z. B. einen Rückruf oder Hintergrund-Thread) auftreten können nicht auf dem Bildschirm gerendert zu erhalten, oder es können auch einen Absturz verursachen.




