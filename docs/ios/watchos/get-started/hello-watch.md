---
title: Hallo, WatchOS – Exemplarische Vorgehensweise
description: Dieses Dokument enthält eine exemplarische Vorgehensweise zum Erstellen einer einfachen WatchOS-Anwendung mithilfe von Xamarin. Es wird beschrieben, wie in Visual Studio und Visual Studio für Mac zu arbeiten, arbeiten mit Storyboards und reagieren auf Ereignisse im Code.
ms.prod: xamarin
ms.assetid: AD1DA488-51AB-420A-A0B7-3AE69A964A40
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 12/14/2016
ms.openlocfilehash: 7066acc82603b1c74dea2b7f0050bc63c46316f1
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122230"
---
# <a name="hello-watchos--walkthrough"></a>Hallo, WatchOS – Exemplarische Vorgehensweise

Nach der Erstellung einer Lösung, die die Schritte in [Setup und Installation](~/ios/watchos/get-started/installation.md), Sie haben die 3-Projekte:

- Die iOS übergeordnete-app die für Setup oder andere Verwaltungsaufgaben auf dem Gerät verwendet wird. (Bei anderen Arten von iOS-Erweiterungen werden soll, ist dies häufig an wie die app "Container" bezeichnet.) Mit Watch-Apps, sie werden für Benutzer, zu die Ausführung der Watch-App ohne **jemals** Ausführen der app übergeordneten;
- Der Watch-Erweiterung, die Programmcode für die Watch-app enthält; und
- Die Watch-App, die die Storyboard und Image-Ressourcen enthält, die auf der Apple Watch gerendert werden.

Überprüfen Sie, ob Ihre [Verweise korrekt sind](~/ios/watchos/get-started/project-references.md):, dass die übergeordnete app einen Verweis auf die Erweiterung hat und die Erweiterung einen Verweis auf die Watch-App enthält.

Bestätigen Sie, dass die Bündel-IDs führen Sie die \*.watchkitextension \*.watchkitapp Konvention hat und dass die Erweiterung der "Info.plist"-Datei ist **WKApp-Bundle-ID** Wert festgelegt wird, um die Bündel-ID von die Watch-App.

Sie sollten die Watch-App nun ausgeführt werden, aber da die Storyboard-Datei in der Watch-App leer ist, wäre Sie angeben können, nicht.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![](hello-watch-images/projectstructure.png "Im Projektmappen-Explorer")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](hello-watch-images/vs-projectstructure.png "Im Projektmappen-Explorer")

-----

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)
    
Doppelklicken Sie auf die Interface.storyboard in Ihrer App überwachen, um Xamarin.IOS-Designer zu starten (Wenn Sie auf einem Mac sind Sie können auch mit der rechten Maustaste und **Öffnen mit > Interface Builder von Xcode**)


1.  Stellen Sie sicher die **Toolbox** und **Eigenschaften** Pads sind sichtbar,
1.  Klicken Sie auf dem Controller-Schnittstelle,
1.  Legen Sie die ID und Titel des Controllers Schnittstelle **InterfaceController** und **Hi Watch**,
1.  Überprüfen Sie die **Klasse** nastaven NA hodnotu **InterfaceController**

    ![](hello-watch-images/interfacecontrollerattributes.png "Legen Sie die ID und Titel des Controllers-Schnittstelle auf InterfaceController \"und\" Hi überwachen")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Doppelklicken Sie auf die Interface.storyboard in Ihrer App sehen Sie sich mit dem Xamarin.IOS-Designer in Visual Studio bearbeiten:

1.  Öffnen Sie den Bereich "Eigenschaften";
1.  Ändern Sie die Klasse **InterfaceController**;
1.  Klicken Sie auf die Schnittstellencontrollers. und
1.  Legen Sie die ID und Titel des Controllers Schnittstelle **InterfaceController** und **Hi Watch**.

    ![](hello-watch-images/vs-interfacecontrollerattributes.png "Legen Sie die ID und Titel des Controllers-Schnittstelle auf InterfaceController \"und\" Hi überwachen")

-----


Die Benutzeroberfläche zu erstellen:

1. Von der **Toolbox** Pad,
1. Drag & drop eine **Schaltfläche** und **Bezeichnung** auf die Szene, und
1. Legen Sie den Text und die Attribute der Steuerelemente wie gezeigt:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![](hello-watch-images/draganddrop.png "Legen Sie den Text und die Attribute der Steuerelemente wie gezeigt")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](hello-watch-images/vs-draganddrop.png "Legen Sie den Text und die Attribute der Steuerelemente wie gezeigt")

-----

1. Legen Sie die **Namen** in die **Eigenschaften** Pad für jedes Steuerelement. In diesem Beispiel haben wir verwendet `myButton` und `myLabel`.

1. Wählen Sie die Schaltfläche für das Storyboard aus, und fahren Sie mit der **Eigenschaften** Pads **Ereignisse** aufzulisten, klicken Sie dann

1. Erstellen Sie ein neues **Aktion** durch Eingabe `OnButtonPress` und **EINGABETASTE**.
  Die Aktion wird in der Liste angezeigt, und eine partielle Methode wird automatisch erstellt, C#.

![](hello-watch-images/buttonaction.png "Die OnButtonPress-Aktion, die auf eine Schaltfläche hinzugefügt")

Nach dem Speichern des Storyboards, mit der **"interfacecontroller.Designer.cs" vor** mit dem Namen von Steuerelementen und Aktionen aktualisiert wird... Wenn Sie diese Datei öffnen, nachdem es aktualisiert hat, sehen Sie wie die `RegisterAttribute` entspricht dem Controller und wie UI-Steuerelemente entsprechen C# Instanzvariablen mit markiert die `OutletAttribute` und Aktionen wie partielle Methoden, die mit der markiertzugeordnet`ActionAttribute`:

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

Öffnen Sie nun **InterfaceController.cs** (*nicht* "interfacecontroller.Designer.cs" vor), und fügen Sie den folgenden Code hinzu:

```csharp
int clickCount = 0;
partial void OnButtonPress (WatchKit.WKInterfaceButton sender)
{
  var msg = String.Format("Clicked {0} times", ++clickCount);
  myLabel.SetText(msg);
}
```

Dieser Code sollte relativ transparent sein: die Instanzvariable `clickCount` wird jedes Mal erhöht die Funktion `OnButtonPress` aufgerufen wird. Der Text der `myLabel` diese Anzahl; entsprechend geändert wird `myLabel`, ist natürlich der Name eines der Absatzmöglichkeiten, die Sie in XCode erstellt haben. Die `partial` Funktion ist die Implementierung der Funktion mit dem Namen der angegebenen Aktion verknüpft ist.

Wenn sie nicht bereits das Startprojekt ist,

1. Mit der rechten Maustaste auf das Projekt Watch-Erweiterung, und wählen Sie **als Startprojekt festlegen**,

1. Legen Sie das Bereitstellungsziel auf eine Watch-Kit-kompatiblen-Simulators (z. B. iPhone 6 iOS 8.2)

1. Drücken Sie die **Debuggen** Schaltfläche zum Auslösen einer Build- und Simulator starten.

    [![](hello-watch-images/readytodebug-sml.png "Die Elemente der Visual Studio-Benutzeroberfläche")](hello-watch-images/readytodebug.png#lightbox)

Wenn der Simulator gestartet wird, drücken Sie die Schaltfläche, um die Bezeichnung zu erhöhen.
Herzlichen Glückwunsch! Sie haben sich selbst eine Watch-App.

![](hello-watch-images/running.png "Die app im Simulator ausgeführt")


## <a name="related-links"></a>Verwandte Links

- [Erste Schritte (Beispiel)](https://developer.xamarin.com/samples/monotouch/WatchKit/GettingStarted/)
- [Setup und Installation](~/ios/watchos/get-started/installation.md)
- [Erste Watch-App-video](http://blog.xamarin.com/your-first-watch-kit-app/)
