---
title: Steuerelemente der watchos-Benutzeroberfläche in xamarin
description: In diesem Dokument werden die verschiedenen Steuerelemente beschrieben, die für die Verwendung in watchos-Benutzeroberflächen verfügbar sind. Es enthält eine Beschreibung der Bezeichnungen, Schaltflächen, Schalter, Schieberegler, Bilder, Trennzeichen, Zuordnungen usw.
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/19/2016
ms.openlocfilehash: 7ac96bce706d42d4334004e62762ff21231f0162
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70766867"
---
# <a name="watchos-user-interface-controls-in-xamarin"></a>Steuerelemente der watchos-Benutzeroberfläche in xamarin

Das [**watchkitcatalog**](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog) -Beispiel veranschaulicht verschiedene watchos-Steuerelemente. Das Storyboard der APP wird hier angezeigt (zum Vergrößern klicken):

[![](images/storyboard-sml.png "Beispiel für watchos-Layout")](images/storyboard.png#lightbox)

Die programmgesteuerten Namen aller Steuerelemente haben das Präfix `WKInterface` (z. b. `WKInterfaceLabel`, `WKInterfaceButton`).

|Steuerelement|Beschreibung|Bildschirmabbildung|
|---|---|---|
|Bezeichnung|Verwenden `SetText` Sie und andere Eigenschaften, um die Darstellung von Text in einem Label-Steuerelement zu steuern. `NSAttributedString`wird ebenfalls unterstützt.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs)|![](Images/label.png)|
|Schaltfläche|Erstellen und Festlegen von Eigenschaften im Storyboard. Fügen Sie zum Hinzufügen eines `Action` zum Implementieren eines Handlers, wenn darauf geklickt wird, STRG + Ziehen hinzu.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs)|![](Images/button.png)|
|Schalter|Verwenden `SetOn` Sie, um den Wechsel Zustand zu steuern.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs)|![](Images/switch.png)|
|Slider|Viele verschiedene Stile sind möglich.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs)|![](Images/slider.png)|
|Bild|Verwenden `myImage.SetImage("MyWatchImage")` Sie, um Bilder auf der Überwachung zu `WKInterfaceDevice.CurrentDevice.AddCachedImage` laden, oder, um Sie für die wiederholte Verwendung auf der Überwachung zwischenzuspeichern.<br />[Dokumentation zur Image Steuerung](~/ios/watchos/user-interface/image.md)<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs)|![](Images/image.png)|
|Trennzeichen|Verwenden Sie Trennzeichen, um ansprechende Benutzeroberflächen zu erstellen.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs)|![](Images/separator.png)| 
|Zuordnung|Das Kartenbild wird statisch auf der Uhr angezeigt, Sie können jedoch viele Aspekte der Darstellung steuern, einschließlich Hinzufügen von Pins.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs)|![](Images/map.png)|
|Movie & Inline Move|Filme können entweder eigenständig oder Inline geöffnet werden.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs)|![](Images/movie.png)|
|Gruppieren|Verwenden Sie Gruppen, um ansprechende Benutzeroberflächen zu erstellen.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs)|![](Images/group.png)|
|Tabelle|Eine vereinfachte Version von Tabellen unter IOS. Implementieren `DidSelectRow` Sie, um auf die Benutzer Auswahl zu reagieren (oder verwenden Sie einen).<br />[Dokumentation zur Tabellensteuerung](~/ios/watchos/user-interface/table.md)<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/Table%20Detail%20Controller/TableDetailController.cs)|![](Images/table.png)|
|Gerät|`WKInterfaceDevice.CurrentDevice`schließt Eigenschaften wie `ScreenBounds`, `ScreenScale`und `PreferredContentSizeCategory`ein.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs)|![](Images/device.png)|
|[Menü](~/ios/watchos/user-interface/menu.md)|Definieren Sie das Menü "Force-Press" im Storyboard, und implementieren Sie die Aktionen für jede Schaltfläche im Code.<br />[Menü Steuerungs Dokumentation (Force Touch)](~/ios/watchos/user-interface/menu.md)<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs)|![](Images/controller.png)|
|Texteingabe|Verwenden `PresentTextInputController` Sie und `WKTextInputMode` die-Enumeration.<br />[Text Eingabe Dokumentation](~/ios/watchos/user-interface/text-input.md)<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputController.cs)|![](Images/textinput.png)|
|Digital Crown|Der Digital Crown kann verwendet werden, um eine Auswahl zu steuern, oder die Drehung kann im Code nachverfolgt werden.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs)|![](Images/digital-crown.png)|
|Gesten|Es gibt vier Arten von Gestenerkennung, die einer Szene hinzugefügt werden können: Tippen, Schwenken, Schwenken und longpress.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs)|![](Images/gestures.png)|

## <a name="related-links"></a>Verwandte Links

- [WatchKitCatalog (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Watch Kit-API-Referenz](xref:WatchKit)
