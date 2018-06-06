---
title: Benutzeroberflächen-Steuerelemente in Xamarin.iOS
description: Dieses enthält Dokumentenlinks zu Anleitungen, die verschiedene iOS Steuerelemente der Benutzeroberfläche Xamarin.iOS Entwickler zu beschreiben. Verknüpfter Inhalt erläutert Warnungen, Schaltflächen, Auflistungsansichten, Bilder, manuelle Kamera Steuerelemente, Karten, Bezeichnungen, Bildlaufbereich, Datumsauswahl und mehr.
ms.prod: xamarin
ms.assetid: C00EA232-ADCC-42AD-BF86-B526414A21C6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 477cb4e479224b9e795f3460e0f59e0aa2038630
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790041"
---
# <a name="user-interface-controls-in-xamarinios"></a>Benutzeroberflächen-Steuerelemente in Xamarin.iOS

Dieses Dokument führt einige der am häufigsten verwendeten iOS Steuerelemente der Benutzeroberfläche und deren Verwendung.

## <a name="alertsalertsmd"></a>[Benachrichtigungen](alerts.md)

Ab mit iOS 8 hat UIAlertController abgeschlossenen ersetzten UIActionSheet und UIAlertView die nun veraltet sind.

## <a name="buttonsbuttonsmd"></a>[Schaltflächen](buttons.md)

Die UIButton-Klasse wird zum Darstellen von verschiedenen verschiedene Stile Schaltfläche iOS Bildschirme verwendet. In diesem Abschnitt werden die verschiedenen Optionen für die Arbeit mit den Schaltflächen im iOS.

## <a name="collection-viewsuicollectionviewmd"></a>[Sammlungsansichten](uicollectionview.md)

Auflistungsansichten, zur Verfügung, in der `UICollectionView` Klasse, ein neues Konzept in iOS 6, in denen mehrere Elemente darstellen, auf dem Bildschirm mithilfe von Layouts eingeführt werden. Die Muster zum Bereitstellen von Daten für eine `UICollectionView` Elemente erstellen und diese Elemente führen Sie die gleichen Delegierung und die Daten Datenquelle in der iOS-Entwicklung häufig verwendeter Muster interagieren.

## <a name="imagesimagemd"></a>[Bilder](image.md)

Hinzufügen von Bildern auf Ihre app sind zwei Schritte erforderlich: Fügen Sie zunächst die Bilder dem Projekt; Anschließend fügen Sie Steuerelemente und Code, um sie auf einem Bildschirm anzuzeigen. Finden Sie in der [arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md) Artikel ausführlichere Abdeckung des Bilds in Xamarin.iOS behandeln.

## <a name="manual-camera-controlsintro-to-manual-camera-controlsmd"></a>[Steuerelemente der manuellen Kamera](intro-to-manual-camera-controls.md)

Die manuelle Kamera Kontrollmechanismen, die von bereitgestellten der `AVFoundation Framework` in iOS 8, können Sie eine mobile Anwendung mit vollständiger Kontrolle über ein iOS-Gerät Kamera. Derartige Differenzierte Steuerung kann verwendet werden, erstellen professionelle Ebene Kamera Anwendungen und Interpreten Kompositionen bereitstellen, indem Sie die Parameter der Kamera beim Offlineschalten von ein noch Bild oder ein Video optimieren.

## <a name="mapsios-mapsindexmd"></a>[Karten](ios-maps/index.md)

Karten sind ein gängiges Feature in allen modernen mobilen Betriebssystemen. iOS bietet Unterstützung systemintern über das Map-Kit-Framework. Map-Kit können Anwendungen problemlos umfassende, interaktive Karten hinzufügen. Diese Zuordnungen können in einer Vielzahl von Möglichkeiten, wie z. B. Hinzufügen von Anmerkungen zum Kennzeichnen von Standorten auf einer Karte und Überlagern von Grafiken der willkürlichen Formen angepasst werden. Map-Kit hat auch integrierten Unterstützung für die aktuelle Position eines Geräts angezeigt.

## <a name="labelslabelsmd"></a>[Bezeichnungen](labels.md)

Die `UILabel` Steuerelement dient zum Anzeigen von einzelnen und mehreren Zeilen nur-Text zu lesen.

## <a name="pickers-and-date-pickerspickermd"></a>[Farbauswahl und Datumsauswahl](picker.md)

Das Kontoauswahl-Steuerelement zeigt 'Wheel Like'-Steuerelements, das eine bildlauffähige Liste von Werten mit den ausgewählten Wert wird hervorgehoben enthält. Benutzer drehen Sie das Mausrad, um die gewünschte Option.

Ein bestimmter Benutzer Fall werden für die Datumsauswahl das Datum und / oder festlegen. Diese Apple bereit hat eine benutzerdefinierte Unterklasse von der UIPickerView Klasse mit dem Namen UIDatePicker erstellt.

## <a name="progress-and-activity-indicatorsprogress-activity-indicatormd"></a>[Status- und Aktivitätsindikatoren](progress-activity-indicator.md)

iOS bietet zwei Hauptmethoden, um den Fortschritt in Ihrer app: Aktivität Indikatoren (einschließlich einen bestimmten _Netzwerk_ Aktivität Indikator) und Statusanzeigen.

## <a name="search-barssearchbarmd"></a>[Suche Balken](searchbar.md)

Die UISearchBar wird verwendet, um eine Liste mit Werten durchsuchen. 

## <a name="sliders-steppers-and-segmented-controlsslider-switch-segmented-controlsmd"></a>[Schieberegler, Stepper und segmentierte Steuerelemente](slider-switch-segmented-controls.md)

Das Schieberegler-Steuerelement ermöglicht einfache Auswahl eines numerischen Werts innerhalb eines Bereichs. iOS verwendet die `UISwitch` wie Geben Sie einen booleschen Wert ab, die durch ein Optionsfeld auf anderen Plattformen dargestellt werden kann. Ein segmentiertes Steuerelement ist eine organisierte ermöglichen Benutzern die Interaktion mit einer kleinen Anzahl von Optionen.

## <a name="stack-viewuistackviewmd"></a>[Stapelansicht](uistackview.md)

Der Stapel Ansichtssteuerelement (`UIStackView`) nutzt die Funktionen von Automatisches Layout und Klassen Größe einen Stapel mit Unteransichten, horizontal oder vertikal zu verwalten, die dynamisch auf die Ausrichtung und der Bildschirmgröße Größe der iOS-Gerät antwortet.

## <a name="tables-and-cellstablesindexmd"></a>[Tabellen und Zellen](tables/index.md)

seine Abschnitt führt die Klassen, die zum Erstellen und Anzeige von Tabellen und enthält Beispiele zur Verwendung in Xamarin.iOS. Es wird behandelt, verwenden die standarddarstellung für Tabellen, Anpassen des Layouts, implementieren bearbeiten und die Verwendung der Xamarin-iOS-Designer eine Tabelle visuell zu entwerfen. In einigen Fällen ist die Anzeige offensichtlich eine Liste von Zeilen (z. B. die Musik-app) und anderen Fällen ist es schwierig zu erkennen, das Table-Steuerelement (z. B. die Bearbeitung in der Kontakte-app oder einer Konversation in Nachrichten-app).

## <a name="text-inputtext-inputmd"></a>[Texteingabe](text-input.md)

Benutzereingaben Text erfolgt mit der `UITextField` für einzeilige ein- und UITextView für mehrzeilige bearbeitbares Textfeld. Sie können entweder diese Steuerelemente auf einem Bildschirm ziehen und doppelklicken, um den ursprünglichen Text selbst festgelegt.

## <a name="tab-bars-and-tab-bar-controllerscreating-tabbed-applicationsmd"></a>[Registerkartenleisten und Registerkartenleisten-Controller](creating-tabbed-applications.md)

iOS-Anwendungen, die über eine Registerkartennavigation Benutzeroberfläche werden erstellt, mit der UITabBarController-Klasse. In diesem Artikel werden protokollsuchen zum Einrichten einer im Registerkartenformat-Anwendung, die mehrere Controller und Ansichten enthält. Klicken Sie dann untersucht, wie eine UITabBarController geladen, wenn es nicht der Stamm-Controller, z. B. nach dem Anmeldebildschirm.

## <a name="web-viewsuiwebviewmd"></a>[Webansichten](uiwebview.md)

In diesem Artikel wird untersucht jede der drei Webansichten von Apple bereitgestellten: `UIWebView`, `WKWebview`, und `SFSafariViewController`, die ähnlichkeiten und Unterschiede und deren Verwendung.

## <a name="related-links"></a>Verwandte Links

- [Steuerelemente (Beispiel)](https://developer.xamarin.com/samples/Controls/)
