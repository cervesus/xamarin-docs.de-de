---
title: WatchOS-Steuerelemente der Benutzeroberfläche in Xamarin
description: Dieses Dokument beschreibt die verschiedenen Steuerelemente, die für die Verwendung in WatchOS-Benutzeroberflächen verfügbar sind. Eine Beschreibung der Bezeichnungen, Schaltflächen, Switches, Schieberegler, Bilder, Trennzeichen, Zuordnungen und mehr darüber.
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/19/2016
ms.openlocfilehash: a7be193cee60b40f70b3dd4a840e0a26ccb8c3b2
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109002"
---
# <a name="watchos-user-interface-controls-in-xamarin"></a>WatchOS-Steuerelemente der Benutzeroberfläche in Xamarin

Die [ **WatchKitCatalog** ](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog) veranschaulicht verschiedene WatchOS-Steuerelemente. Die app Storyboard lautet (zum Vergrößern klicken):

[![](images/storyboard-sml.png "Beispiel-WatchOS-layout")](images/storyboard.png#lightbox)

Die programmgesteuerte Namen aller Steuerelemente enthält das Präfix `WKInterface` (z. b. `WKInterfaceLabel`, `WKInterfaceButton`).

|Steuerelement|Beschreibung|Bildschirmabbildung|
|---|---|---|
|Bezeichnung|Verwendung `SetText` und andere Eigenschaften zur Steuerung der Darstellung von Text in ein Label-Steuerelement. `NSAttributedString` wird ebenfalls unterstützt.<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs)|![](Images/label.png)|
|Schaltfläche|Erstellen Sie ein, und legen Sie Eigenschaften im Storyboard. STRG + Ziehen zum Hinzufügen einer `Action` zum Implementieren eines Handlers für, wenn darauf geklickt wird.<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs)|![](Images/button.png)|
|Schalter|Verwendung `SetOn` zur Steuerung des Status wechseln.<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs)|![](Images/switch.png)|
|Slider|Viele verschiedene Formate sind möglich.<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs)|![](Images/slider.png)|
|Bild|Verwenden Sie `myImage.SetImage("MyWatchImage")` zum Laden von Bildern auf der Apple Watch oder `WKInterfaceDevice.CurrentDevice.AddCachedImage` um diese für die wiederholte Verwendung auf der Apple Watch zwischenzuspeichern.<br />[Image-Steuerelement-Dokumentation](~/ios/watchos/user-interface/image.md)<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs)|![](Images/image.png)|
|Trennzeichen|Verwenden Sie Trennzeichen, um attraktive Watch Benutzeroberflächen zu erstellen.<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs)|![](Images/separator.png)| 
|Zuordnung|Das Map-Bild wird auf der Apple Watch statisch angezeigt, jedoch können Sie zahlreiche Aspekte der Darstellung, einschließlich des Hinzufügens von Pins steuern.<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs)|![](Images/map.png)|
|Film & InlineMove|Filme können entweder Open selbstständig oder Inline<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs)|![](Images/movie.png)|
|Gruppieren|Verwenden von Gruppen, um attraktive Watch Benutzeroberflächen zu erstellen.<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs)|![](Images/group.png)|
|Tabelle|Eine vereinfachte Version der Tabellen unter iOS. Implementieren `DidSelectRow` reagieren auf Benutzerauswahl (oder eines segues).<br />[Tabelle-Dokumentation](~/ios/watchos/user-interface/table.md)<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/Table%20Detail%20Controller/TableDetailController.cs)|![](Images/table.png)|
|Gerät|`WKInterfaceDevice.CurrentDevice` enthält Eigenschaften, wie z. B. `ScreenBounds`, `ScreenScale`, und `PreferredContentSizeCategory`.<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs)|![](Images/device.png)|
|[Menü](~/ios/watchos/user-interface/menu.md)|Definieren Sie das Menü "drücken Sie-Force" im Storyboard und implementieren Sie die Aktionen für jede Schaltfläche im Code.<br />[Menü (Force Touch) Dokumentation](~/ios/watchos/user-interface/menu.md)<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs)|![](Images/controller.png)|
|Texteingabe|Verwendung `PresentTextInputController` und `WKTextInputMode` Enumeration.<br />[Text Input-Dokumentation](~/ios/watchos/user-interface/text-input.md)<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputController.cs)|![](Images/textinput.png)|
|Digitale Krone|Die digitale Crown kann verwendet werden, um eine Auswahl zu steuern, oder es die Drehung im Code verfolgt werden kann.<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs)|![](Images/digital-crown.png)|
|Gesten|Es gibt vier Arten von gestenerkennung, die in einer Szene hinzugefügt werden können: Tippen Sie auf, Wischen, schwenken und LongPress.<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs)|![](Images/gestures.png)|


## <a name="related-links"></a>Verwandte Links

- [WatchKitCatalog (Beispiel)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Sehen Sie sich Kit-API-Referenz](https://developer.xamarin.com/api/namespace/WatchKit/)
