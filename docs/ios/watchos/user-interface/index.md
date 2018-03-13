---
title: "WatchOS-Benutzeroberfläche"
ms.topic: article
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/19/2016
ms.openlocfilehash: 0717b8484c6094bb1d9589c44df37745d9e21900
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="watchos-user-interface"></a>WatchOS-Benutzeroberfläche

Die [ **WatchKitCatalog** ](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog) Beispiel zeigt verschiedene WatchOS-Steuerelemente. Die app Storyboard wird (klicken Sie zum Verkleinern die Tasten) im folgenden dargestellt:

[![](images/storyboard-sml.png "Beispiel WatchOS layout")](images/storyboard.png#lightbox)

Die programmgesteuerte Namen aller Steuerelemente vorangestellt `WKInterface` (z. b. `WKInterfaceLabel`, `WKInterfaceButton`).


<table align="center" border="1" cellpadding="1" cellspacing="1">
  <thead>
      <th>
        <strong>Steuerelement</strong>
      </th>
      <th>
        <strong>Beschreibung</strong>
      </th>
      <th>
        <strong>bildschirmabbildung von</strong>
      </th>
    </thead>
    <tbody>
    <tr>
      <td valign="top">
Bezeichnung </td>
      <td valign="top">
Verwendung <code>SetText</code> und andere Eigenschaften zur Steuerung der Darstellung von Text in ein Label-Steuerelement. <code>NSAttributedString</code> wird ebenfalls unterstützt.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs">Katalog-code</a>
      </td>
      <td>
        <img src="Images/label.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Schaltfläche </td>
      <td valign="top">
Erstellen und Festlegen von Eigenschaften in das Storyboard. <kbd>STRG + Ziehen</kbd> Hinzufügen einer <code>Action</code> implementiert einen Handler für, wenn darauf geklickt wird.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs">Katalog-code</a>
      </td>
      <td>
        <img src="Images/button.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Schalter </td>
      <td valign="top">
Verwendung <code>SetOn</code> zum Steuern des Switch-Zustands.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs">Katalog-code</a>
      </td>
      <td>
        <img src="Images/switch.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Slider </td>
      <td valign="top">
Viele verschiedene Formate sind möglich.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs">Katalog-code</a>
      </td>
      <td>
        <img src="Images/slider.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Bild </td>
      <td valign="top">
Verwenden Sie <code>myImage.SetImage("MyWatchImage")</code> Laden von Bildern auf der Apple Watch oder <code>WKInterfaceDevice.CurrentDevice.AddCachedImage</code> für die wiederholte Verwendung auf der Apple Watch zwischengespeichert.
        <br />
        <a href="~/ios/watchos/user-interface/image.md">Image-Steuerelement-Dokumentation</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs">Katalog-code</a>
      </td>
      <td>
        <img src="Images/image.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Trennzeichen </td>
      <td valign="top">
Verwenden Sie Trennzeichen, um attraktive Überwachungsfenster Benutzeroberflächen zu erstellen.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs">Katalog-code</a>
      </td>
      <td>
        <img src="Images/separator.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Zuordnung </td>
      <td valign="top">
Kartenbilds wird der Apple Watch statisch angezeigt, aber Sie können zahlreiche Aspekte ihrer Darstellung, einschließlich des Hinzufügens von Pins steuern.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs">Katalog-code</a>
      </td>
      <td>
        <img src="Images/map.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Film & InlineMove </td>
      <td valign="top">
Filme können entweder ihre eigenen öffnen oder Inline <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs">Katalog-code</a>
      </td>
      <td>
        <img src="Images/movie.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Gruppieren </td>
      <td valign="top">
Verwenden von Gruppen, um attraktive Überwachungsfenster Benutzeroberflächen zu erstellen.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs">Katalog-code</a>
      </td>
      <td>
        <img src="Images/group.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Tabelle </td>
      <td valign="top">
Eine vereinfachte Version der Tabellen auf iOS.
Implementieren <code>DidSelectRow</code> zum Reagieren auf Benutzerauswahl (oder verwenden eine Segue).
        <br />
        <a href="~/ios/watchos/user-interface/table.md">Dokumentation zu Tabelle-Steuerelement</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TableDetailController.cs">Katalog-code</a>
      </td>
      <td>
        <img src="Images/table.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Gerät </td>
      <td valign="top">
        <code>WKInterfaceDevice.CurrentDevice</code> enthält Eigenschaften, z. B. <code>ScreenBounds</code>, <code>ScreenScale</code>, und <code>PreferredContentSizeCategory</code>.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs">Katalog-code</a>
      </td>
      <td>
        <img src="Images/device.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="~/ios/watchos/user-interface/menu.md">Menü</a>
      </td>
      <td valign="top">
Definieren Sie den Force-drücken Sie im Menü in das Storyboard und implementieren Sie die Aktionen für jede Schaltfläche im Code.
        <br />
        <a href="~/ios/watchos/user-interface/menu.md">Menü-Steuerelement (Touch-Force)-Dokumentation</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs">Katalog-code</a>
      </td>
      <td>
        <img src="Images/controller.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Texteingabe </td>
      <td valign="top">
Verwendung <code>PresentTextInputController</code> und <code>WKTextInputMode</code> Enumeration.
        <br />
        <a href="~/ios/watchos/user-interface/text-input.md">Text-Eingabe-Dokumentation</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputDetailController.cs">Katalog-code</a>
      </td>
      <td>
        <img src="Images/textinput.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Digitale Crown </td>
      <td valign="top">
Digitale Kronenlänge können verwendet werden, um eine Auswahl Laufwerk oder der Rotation im Code verfolgt werden kann.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs">Katalog-code</a>
      </td>
      <td>
        <img src="Images/digital-crown.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Gesten </td>
      <td valign="top">
Es gibt vier Arten von Gestenhandler-Erkennung, die eine Szene hinzugefügt werden können: Tippen, Wischen zum Schwenken und LongPress.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs">Katalog-code</a>
      </td>
      <td>
        <img src="Images/gestures.png" class="tableimg">
      </td>
    </tr>
    </tbody>
</table>



## <a name="related-links"></a>Verwandte Links

- [WatchKitCatalog (Beispiel)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Überwachen-Kit-API-Referenz](https://developer.xamarin.com/api/namespace/WatchKit/)
