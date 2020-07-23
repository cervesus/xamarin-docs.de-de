---
title: Steuerelemente der watchos-Benutzeroberfläche in xamarin
description: In diesem Dokument werden die verschiedenen Steuerelemente beschrieben, die für die Verwendung in watchos-Benutzeroberflächen verfügbar sind. Es enthält eine Beschreibung der Bezeichnungen, Schaltflächen, Schalter, Schieberegler, Bilder, Trennzeichen, Zuordnungen usw.
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/19/2016
ms.openlocfilehash: b8f33be80851815f00b0460a8e31f01733c425e4
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939716"
---
# <a name="watchos-user-interface-controls-in-xamarin"></a>Steuerelemente der watchos-Benutzeroberfläche in xamarin

Das [**watchkitcatalog**](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog) -Beispiel veranschaulicht verschiedene watchos-Steuerelemente. Das Storyboard der APP wird hier angezeigt (zum Vergrößern klicken):

[![Beispiel für watchos-Layout](images/storyboard-sml.png)](images/storyboard.png#lightbox)

Die programmgesteuerten Namen aller Steuerelemente haben das Präfix `WKInterface` (z. b. `WKInterfaceLabel`, `WKInterfaceButton`).

|Control|Beschreibung|Screenshot|
|---|---|---|
|Bezeichnung|Verwenden `SetText` Sie und andere Eigenschaften, um die Darstellung von Text in einem Label-Steuerelement zu steuern. `NSAttributedString`wird ebenfalls unterstützt.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs)|![](Images/label.png)|
|Schaltfläche|Erstellen und Festlegen von Eigenschaften im Storyboard. Fügen Sie zum Hinzufügen eines `Action` zum Implementieren eines Handlers, wenn darauf geklickt wird, STRG + Ziehen hinzu.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs)|![](Images/button.png)|
|Schalter|Verwenden `SetOn` Sie, um den Wechsel Zustand zu steuern.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs)|![](Images/switch.png)|
|Slider|Viele verschiedene Stile sind möglich.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs)|![](Images/slider.png)|
|Abbild|Verwenden `myImage.SetImage("MyWatchImage")` Sie, um Bilder auf der Überwachung zu laden, oder, `WKInterfaceDevice.CurrentDevice.AddCachedImage` um Sie für die wiederholte Verwendung auf der Überwachung zwischenzuspeichern.<br />[Dokumentation zur Image Steuerung](~/ios/watchos/user-interface/image.md)<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs)|![](Images/image.png)|
|Trennzeichen|Verwenden Sie Trennzeichen, um ansprechende Benutzeroberflächen zu erstellen.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs)|![](Images/separator.png)| 
|Zuordnung|Das Kartenbild wird statisch auf der Uhr angezeigt, Sie können jedoch viele Aspekte der Darstellung steuern, einschließlich Hinzufügen von Pins.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs)|![](Images/map.png)|
|Movie & Inline Move|Filme können entweder eigenständig oder Inline geöffnet werden.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs)|![](Images/movie.png)|
|Group|Verwenden Sie Gruppen, um ansprechende Benutzeroberflächen zu erstellen.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs)|![](Images/group.png)|
|Tabelle|Eine vereinfachte Version von Tabellen unter IOS. Implementieren `DidSelectRow` Sie, um auf die Benutzer Auswahl zu reagieren (oder verwenden Sie einen "*").<br />[Dokumentation zur Tabellensteuerung](~/ios/watchos/user-interface/table.md)<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/Table%20Detail%20Controller/TableDetailController.cs)|![](Images/table.png)|
|Sicherungsmedium|`WKInterfaceDevice.CurrentDevice`schließt Eigenschaften wie `ScreenBounds` , `ScreenScale` und ein `PreferredContentSizeCategory` .<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs)|![](Images/device.png)|
|[Menü](~/ios/watchos/user-interface/menu.md)|Definieren Sie das Menü "Force-Press" im Storyboard, und implementieren Sie die Aktionen für jede Schaltfläche im Code.<br />[Menü Steuerungs Dokumentation (Force Touch)](~/ios/watchos/user-interface/menu.md)<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs)|![](Images/controller.png)|
|Texteingabe|Verwenden Sie `PresentTextInputController` und die- `WKTextInputMode` Enumeration.<br />[Text Eingabe Dokumentation](~/ios/watchos/user-interface/text-input.md)<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputController.cs)|![](Images/textinput.png)|
|Digital Crown|Der Digital Crown kann verwendet werden, um eine Auswahl zu steuern, oder die Drehung kann im Code nachverfolgt werden.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs)|![](Images/digital-crown.png)|
|Gesten|Es gibt vier Arten von Gestenerkennung, die einer Szene hinzugefügt werden können: Tap, swipe, Pan und longpress.<br />[Katalog Code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs)|![](Images/gestures.png)|

## <a name="related-links"></a>Ähnliche Themen

- [Watchkitcatalog (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Watch Kit-API-Referenz](xref:WatchKit)
