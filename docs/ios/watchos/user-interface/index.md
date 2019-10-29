---
title: Steuerelemente der watchos-Benutzeroberfläche in xamarin
description: In diesem Dokument werden die verschiedenen Steuerelemente beschrieben, die für die Verwendung in watchos-Benutzeroberflächen verfügbar sind. Es enthält eine Beschreibung der Bezeichnungen, Schaltflächen, Schalter, Schieberegler, Bilder, Trennzeichen, Zuordnungen usw.
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/19/2016
ms.openlocfilehash: 8836eafbbce30586116fd7a7b125da55fe6edf8e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032720"
---
# <a name="watchos-user-interface-controls-in-xamarin"></a>Steuerelemente der watchos-Benutzeroberfläche in xamarin

Das [**watchkitcatalog**](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog) -Beispiel veranschaulicht verschiedene watchos-Steuerelemente. Das Storyboard der APP wird hier angezeigt (zum Vergrößern klicken):

[![](images/storyboard-sml.png "Sample watchOS layout")](images/storyboard.png#lightbox)

Die programmgesteuerten Namen aller Steuerelemente haben das Präfix `WKInterface` (z. b. `WKInterfaceLabel`, `WKInterfaceButton`).

|Steuerelement|Beschreibung|Bildschirmabbildung|
|---|---|---|
|Bezeichnung|Verwenden Sie `SetText` und andere Eigenschaften, um die Darstellung von Text in einem Label-Steuerelement zu steuern. `NSAttributedString` wird ebenfalls unterstützt.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs)|![](Images/label.png)|
|Schaltfläche|Erstellen und Festlegen von Eigenschaften im Storyboard. Fügen Sie zum Hinzufügen eines `Action` zum Implementieren eines Handlers zum Implementieren eines Handlers STRG + ziehen ein.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs)|![](Images/button.png)|
|Schalter|Verwenden Sie `SetOn`, um den Wechsel Zustand zu steuern.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs)|![](Images/switch.png)|
|Slider|Viele verschiedene Stile sind möglich.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs)|![](Images/slider.png)|
|Bild|Verwenden Sie `myImage.SetImage("MyWatchImage")`, um Bilder auf der Überwachung zu laden, oder `WKInterfaceDevice.CurrentDevice.AddCachedImage`, um Sie für die wiederholte Verwendung auf der Überwachung zwischenzuspeichern.<br />[Dokumentation zur Image Steuerung](~/ios/watchos/user-interface/image.md)<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs)|![](Images/image.png)|
|Trennzeichen|Verwenden Sie Trennzeichen, um ansprechende Benutzeroberflächen zu erstellen.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs)|![](Images/separator.png)| 
|Zuordnung|Das Kartenbild wird statisch auf der Uhr angezeigt, Sie können jedoch viele Aspekte der Darstellung steuern, einschließlich Hinzufügen von Pins.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs)|![](Images/map.png)|
|Movie & Inline Move|Filme können entweder eigenständig oder Inline geöffnet werden.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs)|![](Images/movie.png)|
|Gruppieren|Verwenden Sie Gruppen, um ansprechende Benutzeroberflächen zu erstellen.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs)|![](Images/group.png)|
|Tabelle|Eine vereinfachte Version von Tabellen unter IOS. Implementieren `DidSelectRow` Sie, um auf die Benutzer Auswahl zu reagieren (oder verwenden Sie einen).<br />[Dokumentation zur Tabellensteuerung](~/ios/watchos/user-interface/table.md)<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/Table%20Detail%20Controller/TableDetailController.cs)|![](Images/table.png)|
|Gerät|`WKInterfaceDevice.CurrentDevice` enthält Eigenschaften wie `ScreenBounds`, `ScreenScale`und `PreferredContentSizeCategory`.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs)|![](Images/device.png)|
|[Menü](~/ios/watchos/user-interface/menu.md)|Definieren Sie das Menü "Force-Press" im Storyboard, und implementieren Sie die Aktionen für jede Schaltfläche im Code.<br />[Menü Steuerungs Dokumentation (Force Touch)](~/ios/watchos/user-interface/menu.md)<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs)|![](Images/controller.png)|
|Texteingabe|Verwenden Sie `PresentTextInputController` und die `WKTextInputMode`-Enumeration.<br />[Text Eingabe Dokumentation](~/ios/watchos/user-interface/text-input.md)<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputController.cs)|![](Images/textinput.png)|
|Digital Crown|Der Digital Crown kann verwendet werden, um eine Auswahl zu steuern, oder die Drehung kann im Code nachverfolgt werden.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs)|![](Images/digital-crown.png)|
|Gesten|Es gibt vier Arten von Gestenerkennung, die einer Szene hinzugefügt werden können: Tippen, Schwenken, Schwenken und longpress.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs)|![](Images/gestures.png)|

## <a name="related-links"></a>Verwandte Links

- [Watchkitcatalog (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Watch Kit-API-Referenz](xref:WatchKit)
