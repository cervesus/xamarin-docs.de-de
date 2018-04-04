---
title: Hello, überwachen
description: Erste Schritte mit Xamarin und watchOS
ms.prod: xamarin
ms.assetid: AD1DA488-51AB-420A-A0B7-3AE69A964A40
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/14/2016
ms.openlocfilehash: 2281fa801d32e8d8934767ae090503ca523d7eff
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="hello-watch"></a>Hello, überwachen

Nach dem Erstellen einer Lösung, die die Schritte in [Einrichtung und Installation](~/ios/watchos/get-started/installation.md), müssen Sie 3-Projekte:

- Die iOS übergeordnete app die für Setup oder andere Verwaltungsaufgaben auf dem Gerät verwendet wird. (Mit anderen Typen von iOS-Erweiterungen, wird dies häufig an wie die app "Container" bezeichnet.) Mit Watch-Apps ist es möglich für Benutzer mit der die Watch-App ohne Ausführung beginnen **jemals** Ausführen der app übergeordneten;
- Der Watch-Erweiterung die Programm-Codes für die Watch-app enthält; und
- Die Watch-App, die die Storyboard und Image-Ressourcen enthält, die auf der Apple Watch gerendert werden.

Überprüfen Sie, ob Ihre [Verweise korrekt sind](~/ios/watchos/get-started/project-references.md):, dass die übergeordnete app einen Verweis auf die Erweiterung enthält und die Erweiterung einen Verweis auf die Watch-App enthält.

Vergewissern Sie sich, dass Ihr Paket entsprechen den \*.watchkitextension \*.watchkitapp Konvention und hat die Datei "Info.plist" der Erweiterungsdatei wurde **WKApp-Paket-ID** Wert festgelegt wird, um die Paket-ID des Ihre Watch-App.

Sie können jetzt Ihre Watch-App ausführen muss wäre nicht, da jedoch Storyboarddatei innerhalb des Watch-App leer ist, Sie mitteilen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![](hello-watch-images/projectstructure.png "Im Projektmappen-Explorer")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-watch-images/vs-projectstructure.png "Im Projektmappen-Explorer")

-----

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)
    
Doppelklicken Sie auf die Interface.storyboard in Ihrer App überwachen, um den Xamarin iOS-Designer zu starten (Wenn Sie auf einem Mac sind Sie können auch mit der rechten Maustaste und **Öffnen mit > Xcode Schnittstelle-Generator**)


1.  Sicherstellen der **Toolbox** und **Eigenschaften** Pads sind sichtbar,
1.  Klicken Sie auf diese Option, um die Schnittstelle Controller auswählen,
1.  Legen Sie die ID und Titel des Controllers-Schnittstelle, **InterfaceController** und **Hi Überwachungsfenster**,
1.  Überprüfen Sie die **Klasse** festgelegt ist, um **InterfaceController**

    ![](hello-watch-images/interfacecontrollerattributes.png "Legen Sie die ID und Titel des Controllers-Schnittstelle auf InterfaceController "und" Hi überwachen")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Doppelklicken Sie auf die Interface.storyboard in Ihre Watch-App mit dem Xamarin iOS-Designer in Visual Studio bearbeiten:

1.  Öffnen Sie im Bereich "Eigenschaften".
1.  Ändern Sie den Klassennamen in **InterfaceController**;
1.  Klicken Sie auf der Schnittstelle Controller; und
1.  Legen Sie die ID und Titel des Controllers-Schnittstelle, **InterfaceController** und **Hi Überwachungsfenster**.

    ![](hello-watch-images/vs-interfacecontrollerattributes.png "Legen Sie die ID und Titel des Controllers-Schnittstelle auf InterfaceController "und" Hi überwachen")

-----


Die Benutzeroberfläche zu erstellen:

1. Aus der **Toolbox** Pad
1. Drag & drop eine **Schaltfläche** und ein **Bezeichnung** auf der Szene und
1. Legen Sie den Text und die Attribute der Steuerelemente wie gezeigt:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![](hello-watch-images/draganddrop.png "Der Text und die Attribute der Steuerelemente wie dargestellt festgelegt")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-watch-images/vs-draganddrop.png "Der Text und die Attribute der Steuerelemente wie dargestellt festgelegt")

-----

1. Legen Sie die **Namen** in der **Eigenschaften** Pad für jedes Steuerelement. In diesem Beispiel haben wir verwendet `myButton` und `myLabel`.

1. Wählen Sie die Schaltfläche "Storyboard", und wechseln Sie zu der **Eigenschaften** Pads **Ereignisse** aufzulisten, klicken Sie dann

1. Erstellen Sie ein neues **Aktion** dazu `OnButtonPress` und **EINGABETASTE**.
  Die Aktion in der Liste angezeigt wird, und eine partielle Methode wird automatisch in c# erstellt werden.

![](hello-watch-images/buttonaction.png "Die Aktion OnButtonPress hinzugefügt, mit einer Schaltfläche")

Nachdem Sie das Storyboard speichern die **InterfaceController.designer.cs** mit dem Namen von Steuerelementen und Aktionen aktualisiert wird... Wenn Sie diese Datei öffnen, nachdem es aktualisiert hat, können Sie sehen wie die `RegisterAttribute` entspricht dem Controller und den UI-Steuerelementen wie c# Nachrichteninstanzvariablen mit markierten entsprechen der `OutletAttribute` und Zuordnung von Aktionen zu partielle Methoden mit markiert die `ActionAttribute`:

```csharp
// WARNING
//
// This file has been generated automatically by Visual Studio for Mac from the outlets and
// actions declared in your storyboard file.
// Manual changes to this file will not be maintained.
//
[Register ("InterfaceController")]
partial class InterfaceController
{
    [Outlet]
    [GeneratedCode ("iOS Designer", "1.0")]
    WatchKit.WKInterfaceButton myButton { get; set; }

    [Outlet]
    [GeneratedCode ("iOS Designer", "1.0")]
    WatchKit.WKInterfaceLabel myLabel { get; set; }

    [Action ("OnButtonPress:")]
    [GeneratedCode ("iOS Designer", "1.0")]
    partial void OnButtonPress (WatchKit.WKInterfaceButton sender);

    void ReleaseDesignerOutlets ()
    {
        if (myButton != null) {
            myButton.Dispose ();
            myButton = null;
        }
        if (myLabel != null) {
            myLabel.Dispose ();
            myLabel = null;
        }
    }
}
```

Öffnen Sie jetzt **InterfaceController.cs** (*nicht* InterfaceController.designer.cs) und fügen Sie den folgenden Code hinzu:

```csharp
int clickCount = 0;

partial void OnButtonPress (WatchKit.WKInterfaceButton sender)
{
  var msg = String.Format("Clicked {0} times", ++clickCount);
  myLabel.SetText(msg);
}

```

Dieser Code sollte relativ transparent sein: die Instanzvariable `clickCount` wird jedes Mal inkrementiert, die Funktion `OnButtonPress` aufgerufen wird. Der Text der `myLabel` diese Anzahl; entsprechend geändert wird `myLabel`, natürlich ist der Name eines die Steckdosen, die Sie in XCode erstellt haben. Die `partial` Funktion ist die Implementierung der Funktion, die verknüpft sind, mit dem Namen der Aktion, die Sie angegeben haben.

Wenn es nicht bereits das Startup-Projekt ist,

1. Mit der rechten Maustaste auf das Projekt Watch-Erweiterung, und wählen Sie **als Startprojekt festlegen**,

1. Legen Sie das Bereitstellungsziel entweder einem Überwachungsfenster Kit-kompatiblen Simulator Image (z. B. iPhone 6 iOS 8.2),

1. Drücken Sie die **Debuggen** Schaltfläche, um einen Build und Simulator starten auslösen.

    [![](hello-watch-images/readytodebug-sml.png "Die Elemente der Visual Studio-Benutzeroberfläche")](hello-watch-images/readytodebug.png#lightbox)

Wenn der Simulator gestartet wird, drücken Sie die Schaltfläche, um die Bezeichnung zu erhöhen.
Herzlichen Glückwunsch, Sie haben sich selbst eine Watch-App!

![](hello-watch-images/running.png "Die app im Simulator ausführen")


## <a name="related-links"></a>Verwandte Links

- [Erste Schritte (Beispiel)](https://developer.xamarin.com/samples/monotouch/WatchKit/GettingStarted/)
- [Setup und Installation](~/ios/watchos/get-started/installation.md)
- [Erste Watch-App-video](http://blog.xamarin.com/your-first-watch-kit-app/)
