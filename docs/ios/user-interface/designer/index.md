---
title: Aufbauen von Benutzeroberflächen mit dem IOS-Designer
description: In diesem Dokument wird beschrieben, wie Sie die-Xamarin Designer für IOS verwenden, um die Benutzeroberfläche einer APP mit Storyboards und XIb-Dateien zu erstellen. Es ist mit Dokumenten verknüpft, die die Verfügbarkeit des Tools, seine grundlegende Funktionalität, die Entwurfs fähigen Steuerelemente und Exemplarische Vorgehensweisen zur Verwendung erörtern.
ms.prod: xamarin
ms.assetid: E35EFB69-EBBA-40E3-ADBE-CB8016F17127
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 05/31/2018
ms.openlocfilehash: 577c5602c1cbc331564c3034b3f0c11a4b97bc0c
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70279840"
---
# <a name="building-user-interfaces-with-the-ios-designer"></a>Aufbauen von Benutzeroberflächen mit dem IOS-Designer

_Der Xamarin Designer für IOS ist ein visueller Designer für das IOS-Storyboard und Interface Builder Formate, die vollständig in Visual Studio für Mac und Visual Studio integriert sind. Der IOS-Designer behält vollständige Kompatibilität mit den Storyboard-und XIb-Formaten bei, sodass Dateien sowohl in Visual Studio für Mac als auch in Visual Studio, zusätzlich zum Interface Builder von Xcode bearbeitet werden können. Darüber hinaus unterstützt das Xamarin Designer für IOS erweiterte Funktionen, wie z. b. benutzerdefinierte Steuerelemente, die zur Entwurfszeit im Editor gerbt werden._

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![IOS-Designer in Visual Studio für Mac](images/designer-vsmac-sml.png "Der IOS-Designer")](images/designer-vsmac.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![IOS-Designer in Visual Studio](images/designer-vs.png "Der IOS-Designer")](images/designer-vs.png#lightbox)

-----

## <a name="availability"></a>Verfügbarkeit

Der Xamarin Designer für IOS ist in Visual Studio für Mac und in Visual Studio 2017 unter Windows verfügbar.

In diesen Handbüchern wird davon ausgegangen, dass Sie mit den Inhalten vertraut sind, die in den ersten Schritten mit [xamarin. IOS](~/ios/get-started/index.md)behandelt

## <a name="ios-designer-basicsintroductionmd"></a>[iOS-Designer-Grundlagen](introduction.md)

In diesem Handbuch werden die Funktionen des xamarin IOS-Designers behandelt. Es behandelt die Grundlagen des Designers und zeigt, wie der Designer zum visuellen Anordnen von Steuerelementen und zum Bearbeiten von Eigenschaften verwendet wird.

## <a name="designable-controls-overviewios-designable-controls-overviewmd"></a>[Übersicht über Entwurfs fähige Steuerelemente](ios-designable-controls-overview.md)

Dieses Handbuch befasst sich ausführlich mit benutzerdefinierten Steuerelementen, der Art und Weise, wie diese erstellt werden und welche Anforderungen erfüllt werden müssen, damit Sie auf der Entwurfs Oberfläche gerendert Außerdem wird gezeigt, wie häufige Probleme, die bei der Verwendung von Entwurfs fähigen Steuerelementen auftreten können, behoben werden.

## <a name="walkthrough---using-custom-controls-with-ios-designerios-designable-controls-walkthroughmd"></a>[Exemplarische Vorgehensweise: Verwenden benutzerdefinierter Steuerelemente mit IOS-Designer](ios-designable-controls-walkthrough.md)

Dieser Artikel enthält eine schrittweise exemplarische Vorgehensweise, die zeigt, wie Sie ein benutzerdefiniertes Steuerelement erstellen und im IOS-Designer verwenden. Es zeigt, wie Sie ein Steuerelement in der Toolbox des Designers verfügbar machen, damit es in eine Ansicht gezogen bzw. dort abgelegt werden kann. Außerdem wird gezeigt, wie ein Steuerelement implementiert wird, sodass es zur Entwurfszeit und zur Laufzeit ordnungsgemäß gerendert wird. Außerdem wird gezeigt, wie Eigenschaften erstellt werden, die zur Entwurfszeit festgelegt werden können.

## <a name="auto-layout-with-the-xamarin-ios-designerdesigner-auto-layoutmd"></a>[Automatisches Layout mit dem xamarin IOS-Designer](designer-auto-layout.md)

Dieses Handbuch enthält eine Einführung in das automatische IOS-Layout und den neuen Einschränkungs Workflow im IOS-Designer.
