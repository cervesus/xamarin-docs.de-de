---
title: Erstellen von Xamarin.iOS-Anwendungsprofilen mit Instruments
description: In diesem Dokumentation wird beschrieben, wie Sie die Instruments-App von Apple für die Profilerstellung für eine Xamarin.iOS-App verwenden können, die auf einem Gerät oder auf einem Emulator installiert ist.
ms.prod: xamarin
ms.assetid: 70A8CAC8-20C2-655B-37C3-ACF9EA7874D8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 25129d532af0b146afedf28865649ffc9e38ee17
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785611"
---
# <a name="profiling-xamarinios-applications-with-instruments"></a>Erstellen von Xamarin.iOS-Anwendungsprofilen mit Instruments

Xcode **Instruments** ist ein Tool, das zur Erstellung von Xamarin.iOS-App-Profilen auf einem Gerät oder im Simulator verwendet werden kann. Mono verwendet das Just-in-Time-Modell zum Kompilieren von Code und analysiert die anfallenden Daten nicht, was auch für Instruments gilt. Es kann daher schwierig sein, die Ergebnisse von auf Simulatoren basierenden Anwendungen, die Instruments nutzen, zu verwerten.
Aufgrund dieses Problems wird in dieser Anleitung darauf eingegangen, wie die Entwickler-App zum Analysieren von Instruments-Ausgaben verwendet werden kann, die in diesem Dokument dargestellt werden.

## <a name="requirements"></a>Anforderungen

Xcode Instruments kann nur auf einem Mac ausgeführt werden.

## <a name="opening-the-instruments-app"></a>Öffnen der Instruments-App

Wählen Sie das Gerät aus, und führen Sie die Instruments-App aus:

1.  Öffnen Sie das Xamarin.iOS-Projekt in Visual Studio für Mac.
2.  Wählen Sie die Konfiguration **Debug|iPhone** (Debuggen|iPhone) aus.
3.  Verbinden Sie das iOS-Gerät mit dem Computer.
4.  Klicken Sie im Menü **Ausführen** auf **Auf Gerät hochladen**. Die Anwendung wird nun erstellt und auf das Gerät hochgeladen.
5.  Klicken Sie im Menü **Tools** auf **Instrumente starten**.


Die Instrumente werden nun geöffnet, und das folgende Dialogfeld wird angezeigt:

 [![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments1.png "Eine Profilerstellungsvorlage auswählen")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments1.png#lightbox)

Klicken Sie auf die Profilvorlage **Allocations** (Speicherbelegungen). Sie können zwar auch eine der anderen Vorlagen auswählen, doch in diesem Artikel wird nur auf die Profilvorlage **Allocations** (Speicherbelegungen) eingegangen.

Wählen Sie anschließend das Gerät und die Anwendung mit dem Menü im oberen Bereich des Fensters aus:

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments2.png "Gerät und Anwendung auswählen")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments2.png#lightbox)

Das iOS-Gerät sollte im Menü im oberen Bereich des Fensters ausgewählt sein. Daneben sollte außerdem die Anwendung, für die das Profil erstellt wird (**MemoryDemo** im Screenshot oben), ausgewählt sein.

Wenn das Gerät im Menü nicht aufgeführt wird, suchen Sie in der **Konsole** in Visual Studio für Mac nach Fehlermeldungen, die möglicherweise bei der Bereitstellung der App auf einem Gerät angezeigt werden. Stellen Sie außerdem sicher, dass das Gerät für die Entwicklung mit Xcode Organizer bereitgestellt wurde.

Klicken Sie auf die Schaltfläche **Auswählen**, um sich den nächsten Bildschirm anzeigen zu lassen:

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments3.png "Die Profilerstellungsschnittstelle")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments3.png#lightbox)

Klicken Sie auf die Schaltfläche zum Aufzeichnen (roter Kreis oben links ), um die Profilerstellung zu starten.

Im folgenden Screenshot wird die Profilerstellung mit **Instruments** beispielhaft dargestellt:

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments4.png "Beispiel für die Profilerstellung mit Instruments")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments4.png#lightbox)

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurde gezeigt, wie Sie Xcode Instruments starten, um eine iOS-App in Visual Studio für Mac zu überwachen. Im nächsten Artikel ([Instruments Walkthrough (Erste Schritte mit Instruments)](~/ios/deploy-test/walkthrough-apples-instrument.md)) finden Sie ein Beispiel zum Diagnostizieren eines Speicherproblems mithilfe von Instruments.

## <a name="related-links"></a>Verwandte Links

- [Exemplarische Vorgehensweise: Instruments](~/ios/deploy-test/walkthrough-apples-instrument.md)
- [Xamarin.iOS-Garbage Collection](https://krumelur.me/2015/04/27/xamarin-ios-the-garbage-collector-and-me/)
