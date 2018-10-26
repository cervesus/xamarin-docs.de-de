---
title: Steuerelemente der Benutzeroberfläche in Xamarin.iOS
description: Dieses Dokument enthält Links zu Leitfäden, die die verschiedenen iOS Steuerelemente der Benutzeroberfläche für Xamarin.iOS-Entwickler zu beschreiben. Verknüpfter Inhalt wird erläutert, Warnungen, Schaltflächen, Auflistungsansichten, Bilder, Steuerelemente der manuellen Kamera, Zuordnungen, Bezeichnungen, dateiöffnungs-, datumserkennung und mehr.
ms.prod: xamarin
ms.assetid: C00EA232-ADCC-42AD-BF86-B526414A21C6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 25b17dbebebf7bcae92ebdc294c798101d39b987
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121619"
---
# <a name="user-interface-controls-in-xamarinios"></a>Steuerelemente der Benutzeroberfläche in Xamarin.iOS

In diesem Dokument werden einige der am häufigsten verwendeten Steuerelemente der Benutzeroberfläche iOS und deren Verwendung.

## <a name="alertsalertsmd"></a>[Benachrichtigungen](alerts.md)

Ab iOS 8, UIAlertController hat abgeschlossenen ersetzten UIActionSheet aus, und UIAlertView der sind jetzt veraltet.

## <a name="buttonsbuttonsmd"></a>[Schaltflächen](buttons.md)

Die UIButton-Klasse wird zum Darstellen von verschiedenen verschiedene Arten der Schaltfläche in iOS-Bildschirmen verwendet. In diesem Abschnitt werden die verschiedenen Optionen für die Arbeit mit Schaltflächen in iOS eingeführt.

## <a name="collection-viewsuicollectionviewmd"></a>[Sammlungsansichten](uicollectionview.md)

Auflistungsansichten, verfügbar in der `UICollectionView` -Klasse sind ein neues Konzept in iOS 6, die Sie einführen, mehrere Elemente auf dem Bildschirm mithilfe von Layouts darstellen. Die Muster zum Bereitstellen von Daten für eine `UICollectionView` Elemente erstellen und diese Elemente führen Sie die gleichen Delegierung und Datenquellen-Muster, die häufig in iOS-Entwicklung verwendet interagieren.

## <a name="imagesimagemd"></a>[Bilder](image.md)

Hinzufügen von Bildern zu Ihrer app sind zwei Schritte erforderlich: Fügen Sie zunächst die Images auf Ihr Projekt, Anschließend fügen Sie Steuerelemente und Code auf einem Bildschirm anzeigen. Finden Sie in der [arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md) detaillierte Abdeckung der Bildbehandlung in Xamarin.iOS-Artikel.

## <a name="manual-camera-controlsintro-to-manual-camera-controlsmd"></a>[Steuerelemente der manuellen Kamera](intro-to-manual-camera-controls.md)

Den Kamerasteuerelemente manuell, gebotenen die `AVFoundation Framework` in iOS 8 können Sie eine mobile Anwendung vollständiger Kontrolle über ein iOS-Gerät-Kamera. Erstellen professionelle Ebene Kamera-Anwendungen und Bereitstellen von Interpreten Kompositionen Anpassungen von den Parametern der Kamera und gleichzeitig eine noch Bild- oder videobearbeitung, kann dieses detaillierte Maß an Kontrolle verwendet werden.

## <a name="mapsios-mapsindexmd"></a>[Karten](ios-maps/index.md)

Zuordnungen sind ein Feature in allen modernen mobilen Betriebssystemen. iOS bietet Unterstützung systemintern über das Map-Kit-Framework. Map Kit können Anwendungen problemlos interaktive Karten hinzufügen. Diese Zuordnungen können in einer Vielzahl von Möglichkeiten, wie etwa das Hinzufügen von Anmerkungen, um die Speicherorte auf einer Karte zu markieren und Überlagern von Grafiken von willkürlichen Formen angepasst werden. Map Kit verfügt auch über integrierte Unterstützung für die aktuelle Position eines Geräts angezeigt.

## <a name="labelslabelsmd"></a>[Bezeichnungen](labels.md)

Die `UILabel` Steuerelement dient zum Anzeigen von einzelnen und mehrzeilige, nur-Text zu lesen.

## <a name="pickers-and-date-pickerspickermd"></a>[Auswahltools und Datumsauswahl](picker.md)

Die Datumsauswahl-Steuerelement zeigt 'Wheel-Like'-Steuerelement, das eine bildlauffähige Liste von Werten mit den ausgewählten Wert wird hervorgehoben enthält. Benutzer drehen Sie das Mausrad, um die gewünschte Option.

Ein bestimmter Benutzer die Schreibweise für die Datumsauswahl das Datum und/oder die Uhrzeit fest. Diese Apple bereit hat eine benutzerdefinierte Unterklasse der UIPickerView Klasse mit dem Namen UIDatePicker erstellt.

## <a name="progress-and-activity-indicatorsprogress-activity-indicatormd"></a>[Status- und Aktivitätsindikatoren](progress-activity-indicator.md)

iOS bietet die wesentlichen zwei Möglichkeiten zum Anzeigen des Status in Ihrer app: Aktivitätsindikatoren (einschließlich einen bestimmten _Netzwerk_ Aktivitätsanzeige) und Statusanzeigen.

## <a name="search-barssearchbarmd"></a>[Suchleisten](searchbar.md)

Die UISearchBar wird verwendet, um über eine Liste von Werten zu suchen. 

## <a name="sliders-steppers-and-segmented-controlsslider-switch-segmented-controlsmd"></a>[Schieberegler Stepper und segmentierte Steuerelemente](slider-switch-segmented-controls.md)

Das Schieberegler-Steuerelement ermöglicht einfache Auswahl eines numerischen Werts innerhalb eines Bereichs. iOS verwendet die `UISwitch` wie einen booleschen Wert eingeben, die durch ein Optionsfeld auf anderen Plattformen dargestellt werden kann. Ein segmentiertes Steuerelement ist ein organisiert, dass Benutzer mit einer kleinen Anzahl von Optionen interagieren können.

## <a name="stack-viewuistackviewmd"></a>[Stapelansicht](uistackview.md)

Die Stack-Steuerelement (`UIStackView`) nutzt die Leistungsfähigkeit von Automatisches Layout und Größenklassen einen Stapel von Unteransichten entweder horizontal oder vertikal zu verwalten, die dynamisch auf die Ausrichtung und der Bildschirmgröße die Größe des iOS-Geräts reagiert.

## <a name="tables-and-cellstablesindexmd"></a>[Tabellen und Zellen](tables/index.md)

seine Abschnitt führt die Klassen, die zum Erstellen und zeigen Sie Tabellen und enthält Beispiele für deren Verwendung in Xamarin.iOS. Es wird behandelt, verwenden die standarddarstellung für Tabellen, Anpassen des Layouts, das Implementieren von bearbeiten und die Verwendung der Xamarin.IOS-Designer, um eine Tabelle visuell zu entwerfen. Manchmal ist die Anzeige offensichtlich eine Liste von Zeilen (z. B. die Musik-app) und in anderen Situationen ist es schwer zu erkennen, das Table-Steuerelement (z. B. die Bearbeitung in die Kontakte-app oder einer Konversation in der Nachrichten-app).

## <a name="text-inputtext-inputmd"></a>[Texteingabe](text-input.md)

Akzeptieren die Texteingabe des Benutzers erfolgt mithilfe der `UITextField` für einzeilige Eingaben und UITextView für mehrzeiligen bearbeitbaren Text. Können Sie diese beiden Steuerelemente auf einem Bildschirm ziehen und doppelklicken Sie auf, um den Begrüßungstext festzulegen.

## <a name="tab-bars-and-tab-bar-controllerscreating-tabbed-applicationsmd"></a>[Registerkartenleisten und Registerkartenleisten-Controller](creating-tabbed-applications.md)

iOS-Anwendungen, die über eine Benutzeroberfläche für die Registerkarte der Navigationsleiste werden mithilfe der UITabBarController-Klasse erstellt. In diesem Artikel behandeln wir durch die Schritte zum Einrichten einer Anwendung mit Registerkarten, die mehrere Controller und Ansichten enthält. Klicken Sie dann untersucht, wie eine UITabBarController geladen, wenn es nicht der Root-Controller, z. B. nach einer Anmeldeseite.

## <a name="web-viewsuiwebviewmd"></a>[Webansichten](uiwebview.md)

In diesem Artikel wird untersucht, jede der drei Webansicht, die von Apple bereitgestellten: `UIWebView`, `WKWebview`, und `SFSafariViewController`, die ähnlichkeiten und Unterschiede und wie sie verwendet werden können.

## <a name="related-links"></a>Verwandte Links

- [Steuerelemente (Beispiel)](https://developer.xamarin.com/samples/Controls/)
