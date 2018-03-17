---
title: "WatchOS-Benutzeroberfläche"
ms.topic: article
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/19/2016
ms.openlocfilehash: f7a7b565372970cc487b664ed7415cf876e290b6
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
---
# <a name="watchos-user-interface"></a>WatchOS-Benutzeroberfläche

Die [ **WatchKitCatalog** ](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog) Beispiel zeigt verschiedene WatchOS-Steuerelemente. Die app Storyboard wird (klicken Sie zum Verkleinern die Tasten) im folgenden dargestellt:

[![](images/storyboard-sml.png "Beispiel WatchOS layout")](images/storyboard.png#lightbox)

Die programmgesteuerte Namen aller Steuerelemente vorangestellt `WKInterface` (z. b. `WKInterfaceLabel`, `WKInterfaceButton`).

|Steuerelement|Beschreibung|Bildschirmabbildung|
|---|---|---|
|Bezeichnung|Verwendung `SetText` und andere Eigenschaften zur Steuerung der Darstellung von Text in ein Label-Steuerelement. `NSAttributedString` wird ebenfalls unterstützt.<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs)|![](Images/label.png)|
|Schaltfläche|Erstellen und Festlegen von Eigenschaften in das Storyboard. STRG + Ziehen, um das Hinzufügen einer `Action` implementiert einen Handler für, wenn darauf geklickt wird.<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs)|![](Images/button.png)|
|Schalter|Verwendung `SetOn` zum Steuern des Switch-Zustands.<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs)|![](Images/switch.png)|
|Slider|Viele verschiedene Formate sind möglich.<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs)|![](Images/slider.png)|
|Bild|Verwenden Sie `myImage.SetImage("MyWatchImage")` Laden von Bildern auf der Apple Watch oder `WKInterfaceDevice.CurrentDevice.AddCachedImage` für die wiederholte Verwendung auf der Apple Watch zwischengespeichert.<br />[Image-Steuerelement-Dokumentation](~/ios/watchos/user-interface/image.md)<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs)|![](Images/image.png)|
|Trennzeichen|Verwenden Sie Trennzeichen, um attraktive Überwachungsfenster Benutzeroberflächen zu erstellen.<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs)|![](Images/separator.png)| 
|Zuordnung|Kartenbilds wird der Apple Watch statisch angezeigt, aber Sie können zahlreiche Aspekte ihrer Darstellung, einschließlich des Hinzufügens von Pins steuern.<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs)|![](Images/map.png)|
|Film & InlineMove|Filme können entweder ihre eigenen öffnen oder Inline<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs)|![](Images/movie.png)|
|Gruppieren|Verwenden von Gruppen, um attraktive Überwachungsfenster Benutzeroberflächen zu erstellen.<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs)|![](Images/group.png)|
|Tabelle|Eine vereinfachte Version der Tabellen auf iOS. Implementieren `DidSelectRow` zum Reagieren auf Benutzerauswahl (oder verwenden eine Segue).<br />[Dokumentation zu Tabelle-Steuerelement](~/ios/watchos/user-interface/table.md)<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/Table%20Detail%20Controller/TableDetailController.cs)|![](Images/table.png)|
|Gerät|`WKInterfaceDevice.CurrentDevice` enthält Eigenschaften, z. B. `ScreenBounds`, `ScreenScale`, und `PreferredContentSizeCategory`.<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs)|![](Images/device.png)|
|[Menü](~/ios/watchos/user-interface/menu.md)|Definieren Sie den Force-drücken Sie im Menü in das Storyboard und implementieren Sie die Aktionen für jede Schaltfläche im Code.<br />[Menü-Steuerelement (Touch-Force)-Dokumentation](~/ios/watchos/user-interface/menu.md)<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs)|![](Images/controller.png)|
|Texteingabe|Verwendung `PresentTextInputController` und `WKTextInputMode` Enumeration.<br />[Text-Eingabe-Dokumentation](~/ios/watchos/user-interface/text-input.md)<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputController.cs)|![](Images/textinput.png)|
|Digitale Crown|Digitale Kronenlänge können verwendet werden, um eine Auswahl Laufwerk oder der Rotation im Code verfolgt werden kann.<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs)|![](Images/digital-crown.png)|
|Gesten|Es gibt vier Arten von Gestenhandler-Erkennung, die eine Szene hinzugefügt werden können: Tippen, Wischen zum Schwenken und LongPress.<br />[Katalog-code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs)|![](Images/gestures.png)|


## <a name="related-links"></a>Verwandte Links

- [WatchKitCatalog (Beispiel)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Überwachen-Kit-API-Referenz](https://developer.xamarin.com/api/namespace/WatchKit/)
