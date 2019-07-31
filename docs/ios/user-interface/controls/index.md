---
title: Steuerelemente der Benutzeroberfläche in xamarin. IOS
description: Dieses Dokument ist mit Anleitungen verknüpft, die die verschiedenen IOS-Benutzeroberflächen-Steuerelemente beschreiben, die für xamarin. IOS-Entwickler verfügbar sind Mit verknüpften Inhalten werden Warnungen, Schaltflächen, Auflistungs Ansichten, Bilder, manuelle Kamera Steuerelemente, Karten, Bezeichnungen, Picker, Datums-Picker und mehr erläutert.
ms.prod: xamarin
ms.assetid: C00EA232-ADCC-42AD-BF86-B526414A21C6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 28dc915e373ed16336551fa28fde8cbae8ee815e
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68657486"
---
# <a name="user-interface-controls-in-xamarinios"></a>Steuerelemente der Benutzeroberfläche in xamarin. IOS

In diesem Dokument werden einige der gängigsten IOS-Benutzeroberflächen-Steuerelemente und deren Verwendung vorgestellt.

## <a name="alertsalertsmd"></a>[Benachrichtigungen](alerts.md)

Ab IOS 8 hat uialertcontroller den ersetzten uiaktionsheet und UIAlertView abgeschlossen, die beide nun als veraltet markiert wurden.

## <a name="buttonsbuttonsmd"></a>[Schaltflächen](buttons.md)

Die UIButton-Klasse wird verwendet, um verschiedene Arten von Schaltflächen in ios-Bildschirmen darzustellen. In diesem Abschnitt werden die verschiedenen Optionen zum Arbeiten mit Schaltflächen in ios vorgestellt.

## <a name="collection-viewsuicollectionviewmd"></a>[Sammlungsansichten](uicollectionview.md)

Sammlungs Sichten, die in der `UICollectionView` -Klasse verfügbar sind, sind ein neues Konzept in ios 6, das die Darstellung mehrerer Elemente auf dem Bildschirm mithilfe von Layouts einführt. Die Muster für die Bereitstellung von `UICollectionView` Daten für einen, um Elemente zu erstellen und mit diesen Elementen zu interagieren, folgen derselben Delegierung und denselben Datenquellen Mustern, die häufig bei der IOS-Entwicklung

## <a name="imagesimagemd"></a>[Bilder](image.md)

Zum Hinzufügen von Bildern zu Ihrer APP sind zwei Schritte erforderlich: zuerst fügen Sie die Bilder dem Projekt hinzu. Fügen Sie anschließend Steuerelemente und Code hinzu, um Sie auf einem Bildschirm anzuzeigen. Ausführliche Informationen zur Bildverarbeitung in xamarin. IOS finden Sie im Artikel [Arbeiten mit Images](~/ios/app-fundamentals/images-icons/index.md) .

## <a name="manual-camera-controlsintro-to-manual-camera-controlsmd"></a>[Steuerelemente der manuellen Kamera](intro-to-manual-camera-controls.md)

Die manuellen Kamera Steuerelemente, die von `AVFoundation Framework` der in ios 8 bereitgestellt werden, ermöglichen einer mobilen Anwendung die vollständige Kontrolle über die Kamera eines IOS-Geräts. Diese differenzierte Steuerungsebene kann verwendet werden, um professionelle Kamera Anwendungen zu erstellen und Künstler Kompositionen bereitzustellen, indem Sie die Parameter der Kamera optimieren, während gleichzeitig ein Bild oder Video erstellt wird.

## <a name="mapsios-mapsindexmd"></a>[Karten](ios-maps/index.md)

Maps sind ein gängiges Feature in allen modernen mobilen Betriebssystemen. IOS bietet eine systemeigene Zuordnungs Unterstützung durch das Map Kit-Framework. Mit dem Map-Kit können Anwendungen problemlos umfangreiche, interaktive Karten hinzufügen. Diese Zuordnungen können auf verschiedene Arten angepasst werden, z. b. das Hinzufügen von Anmerkungen zum Markieren von Positionen auf einer Karte und das Überlagern von Grafiken beliebiger Formen. Map Kit verfügt auch über integrierte Unterstützung für die Anzeige des aktuellen Speicher Orts eines Geräts.

## <a name="labelslabelsmd"></a>[Bezeichnungen](labels.md)

Das `UILabel` -Steuerelement wird zum Anzeigen von einzeiligen und mehrzeiligen schreibgeschützten Text verwendet.

## <a name="pickers-and-date-pickerspickermd"></a>[Picker und Datums-Picker](picker.md)

Das Auswahl Steuerelement zeigt das Steuerelement "Rad-like" an, das eine Bild lauffähige Liste von Werten enthält Benutzer drehen das Rad, um die gewünschte Option auszuwählen.

Ein spezieller Benutzer Fall, um das Datum und/oder die Uhrzeit festzulegen. Zum Bereitstellen für dieses Apple hat eine benutzerdefinierte Unterklasse der uipickerview-Klasse mit dem Namen "UIDatePicker" erstellt.

## <a name="progress-and-activity-indicatorsprogress-activity-indicatormd"></a>[Status- und Aktivitätsindikatoren](progress-activity-indicator.md)

IOS bietet zwei Hauptmöglichkeiten, den Fortschritt in der APP anzugeben: Aktivitätsindikatoren (einschließlich eines bestimmten _Netzwerk_ Aktivitäts Indikators) und Status leisten.

## <a name="search-barssearchbarmd"></a>[Suchleisten](searchbar.md)

Uisearchbar wird verwendet, um eine Liste von Werten zu durchsuchen. 

## <a name="sliders-switches-and-segmented-controlsslider-switch-segmented-controlsmd"></a>[Schieberegler, Schalter und segmentierte Steuerelemente](slider-switch-segmented-controls.md)

Das Schieberegler-Steuerelement ermöglicht die einfache Auswahl eines numerischen Werts innerhalb eines Bereichs. IOS verwendet `UISwitch` als boolesche Eingabe, die auf anderen Plattformen durch eine Options Schaltfläche dargestellt werden kann. Ein segmentiertes Steuerelement ist eine organisierte Methode, mit der Benutzer mit einer kleinen Anzahl von Optionen interagieren können.

## <a name="stack-viewuistackviewmd"></a>[Stapelansicht](uistackview.md)

Das Stapel Ansicht-Steuer`UIStackView`Element () nutzt die Leistungsfähigkeit von automatischen layoutklassen und Größenklassen, um einen Stapel von unter Ansichten entweder horizontal oder vertikal zu verwalten, der dynamisch auf die Ausrichtung und Bildschirmgröße des IOS-Geräts antwortet.

## <a name="tables-and-cellstablesindexmd"></a>[Tabellen und Zellen](tables/index.md)

in seinem Abschnitt werden die Klassen vorgestellt, die zum Erstellen und Anzeigen von Tabellen verwendet werden, und es werden Beispiele für die Verwendung in xamarin. IOS bereitgestellt. Dabei wird die Standarddarstellung für Tabellen, das Anpassen des Layouts, das Implementieren der Bearbeitung und die Verwendung des xamarin IOS-Designers zum visuellen Entwerfen einer Tabelle behandelt. Manchmal ist die Anzeige offensichtlich eine Liste von Zeilen (z. b. die Musik-app), und in anderen Fällen ist es schwierig, das Tabellen Steuerelement zu erkennen (z. b. die Bearbeitung in der App "Kontakte" oder eine Konversation in der Nachrichten-

## <a name="text-inputtext-inputmd"></a>[Texteingabe](text-input.md)

Das akzeptieren von Benutzer Texteingaben wird mit `UITextField` dem für einzeilige Eingaben und uitextview für mehrzeiligen bearbeitbaren Text erreicht. Sie können eines dieser Steuerelemente auf einen Bildschirm ziehen und doppelklicken, um den Anfangstext festzulegen.

## <a name="tab-bars-and-tab-bar-controllerscreating-tabbed-applicationsmd"></a>[Registerkartenleisten und Registerkartenleisten-Controller](creating-tabbed-applications.md)

IOS-Anwendungen, die eine Registerkarte mit Registerkarten Navigation verwenden, werden mithilfe der uitabbarcontroller-Klasse erstellt. In diesem Artikel wird beschrieben, wie Sie eine Anwendung im Registerkarten Format einrichten, die mehrere Controller und Ansichten enthält. Wir untersuchen dann, wie ein uitabbarcontroller geladen wird, wenn es sich nicht um den Stamm Controller handelt, z. b. nach einem Anmeldebildschirm.

## <a name="web-viewsuiwebviewmd"></a>[Webansichten](uiwebview.md)

In diesem Artikel untersuchen wir die drei von Apple bereitgestellten Webansichten: `UIWebView`, `WKWebview`und `SFSafariViewController`, ihre Ähnlichkeiten und Unterschiede sowie deren Verwendung.

## <a name="related-links"></a>Verwandte Links

- [Steuerelemente (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
