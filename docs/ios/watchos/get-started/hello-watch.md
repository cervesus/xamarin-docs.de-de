---
title: Hello, watchos – Exemplarische Vorgehensweise
description: Dieses Dokument enthält eine exemplarische Vorgehensweise zum Aufbauen einer einfachen watchos-Anwendung mithilfe von xamarin. Es wird beschrieben, wie Sie sowohl in Visual Studio als auch in Visual Studio für Mac arbeiten, mit Storyboards arbeiten und auf Ereignisse im Code reagieren.
ms.prod: xamarin
ms.assetid: AD1DA488-51AB-420A-A0B7-3AE69A964A40
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 12/14/2016
ms.openlocfilehash: 3f69f10274c413a107a40b2f404b3227cfee67cf
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936739"
---
# <a name="hello-watchos--walkthrough"></a>Hello, watchos – Exemplarische Vorgehensweise

Nachdem Sie eine Lösung erstellt haben, die die Schritte unter [Setup und Installation](~/ios/watchos/get-started/installation.md)durchführt, verfügen Sie über drei Projekte:

- Die übergeordnete IOS-APP, die für Setup oder andere Verwaltungsaufgaben auf dem Gerät verwendet wird. (Bei anderen Typen von IOS-Erweiterungen wird dies häufig als "Container"-App bezeichnet.) Mit Watch-Apps ist es möglich, dass Benutzer mit der Ausführung der Watch-App beginnen, ohne die übergeordnete APP **jemals** ausführen zu müssen.
- Die Watch-Erweiterung, die den Programmcode für die Watch-app enthält. immer
- Die Watch-APP, die die Storyboard-und Bild Ressourcen enthält, die auf der Überwachung gerendert werden.

Überprüfen Sie, ob Ihre [Verweise korrekt sind](~/ios/watchos/get-started/project-references.md): die übergeordnete app enthält einen Verweis auf die Erweiterung, und die Erweiterung enthält einen Verweis auf die Watch-app.

Vergewissern Sie sich, dass die Bündel \* Bezeichner der. watchkitextension \* . watchkitapp-Konvention folgen und dass die Datei "Info. plist" der Erweiterung den **wkapp-Bundle-ID** -Wert für die Paket-ID Ihrer Watch-App festgelegt hat.

Sie sollten ihre Watch-APP jetzt ausführen können, aber da die storyboarddatei in der Watch-App leer ist, können Sie nicht sagen.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

![Der Projektmappen-Explorer](hello-watch-images/projectstructure.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Der Projektmappen-Explorer](hello-watch-images/vs-projectstructure.png)

-----

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Doppelklicken Sie in ihrer Watch-App auf das Interface. Storyboard, um den xamarin IOS-Designer zu starten (wenn Sie sich auf einem Mac befinden, können Sie auch mit der rechten Maustaste klicken und **mit > Xcode-Interface Builder öffnen**).

1. Stellen Sie sicher, dass die Bereiche **Toolbox** und **Eigenschaften** sichtbar sind.
1. Klicken Sie, um den Schnittstellen Controller auszuwählen.
1. Legen Sie den Bezeichner und den Titel des Schnittstellen Controllers auf **interfakecontroller** und **Hi Watch**fest.
1. Überprüfen, ob die **Klasse** auf **interfacecontroller** festgelegt ist

    ![Legen Sie den Bezeichner und den Titel des Schnittstellen Controllers auf interfakecontroller und Hi Watch fest.](hello-watch-images/interfacecontrollerattributes.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Doppelklicken Sie in ihrer Watch-App auf das Interface. Storyboard, um es mit dem xamarin IOS-Designer in Visual Studio zu bearbeiten:

1. Öffnen Sie den Eigenschaften Bereich.
1. Ändern Sie die Klasse in **interfacecontroller**;
1. Klicken Sie auf den Schnittstellen Controller. immer
1. Legen Sie den Bezeichner und den Titel des Schnittstellen Controllers auf **interfakecontroller** und **Hi Watch**fest.

    ![Legen Sie den Bezeichner und den Titel des Schnittstellen Controllers auf interfakecontroller und Hi Watch fest.](hello-watch-images/vs-interfacecontrollerattributes.png)

-----

Erstellen Sie die Benutzeroberfläche:

1. Im **Toolbox** -Pad
1. Ziehen Sie eine **Schaltfläche** und eine **Bezeichnung** per Drag & Drop auf die Szene, und
1. Legen Sie den Text und die Attribute der Steuerelemente wie dargestellt fest:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

![Legen Sie den Text und die Attribute der Steuerelemente wie dargestellt fest.](hello-watch-images/draganddrop.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Legen Sie den Text und die Attribute der Steuerelemente wie dargestellt fest.](hello-watch-images/vs-draganddrop.png)

-----

1. Legen Sie den **Namen** für jedes Steuerelement im **eigenschaftenpad** fest. In diesem Beispiel haben wir `myButton` und verwendet `myLabel` .

1. Wählen Sie die Schaltfläche auf dem Storyboard aus, und navigieren Sie zur **Ereignis** Liste des **eigenschaftenpad** .

1. Erstellen Sie eine neue **Aktion** , indem `OnButtonPress` **Sie eingeben**und die EINGABETASTE drücken.
  Die Aktion wird in der Liste angezeigt, und in c# wird automatisch eine partielle Methode erstellt.

![Die onbuttonpress-Aktion, die einer Schaltfläche hinzugefügt wurde](hello-watch-images/buttonaction.png)

Nachdem Sie das Storyboard gespeichert haben, wird die **InterfaceController.Designer.cs** mit den Steuerelement Namen und-Aktionen aktualisiert. Wenn Sie diese Datei nach der Aktualisierung öffnen, können Sie sehen, wie der `RegisterAttribute` dem Controller entspricht und wie UI-Steuerelemente c#-Instanzvariablen entsprechen, die mit gekennzeichnet sind, `OutletAttribute` und wie Aktionen partiellen Methoden zugeordnet werden, die mit markiert sind `ActionAttribute` :

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

Öffnen Sie nun **InterfaceController.cs** (*nicht* InterfaceController.Designer.cs), und fügen Sie den folgenden Code hinzu:

```csharp
int clickCount = 0;
partial void OnButtonPress (WatchKit.WKInterfaceButton sender)
{
  var msg = String.Format("Clicked {0} times", ++clickCount);
  myLabel.SetText(msg);
}
```

Dieser Code sollte Recht transparent sein: die Instanzvariable `clickCount` wird jedes Mal inkrementiert, wenn die-Funktion `OnButtonPress` aufgerufen wird. Der Text von `myLabel` wird so geändert, dass er diese Anzahl widerspiegelt `myLabel` . natürlich ist der Name eines der Outlets, die Sie in Xcode erstellt haben. Die- `partial` Funktion ist die Implementierung der-Funktion, die dem Namen der von Ihnen angegebenen Aktion zugeordnet ist.

Wenn dies nicht bereits das Startprojekt ist,

1. Klicken Sie mit der rechten Maustaste auf das Überwachungs Erweiterungsprojekt, und wählen Sie **als Startprojekt festlegen**aus.

1. Legen Sie das Bereitstellungs Ziel auf ein Watch Kit-kompatibles simulatorbild (z. b. iPhone 6 IOS 8,2) fest.

1. Klicken Sie auf die Schaltfläche **Debuggen** , um einen Build-und simulatorstart

    [![Die Visual Studio-Schnittstellen Elemente](hello-watch-images/readytodebug-sml.png)](hello-watch-images/readytodebug.png#lightbox)

Wenn der Simulator gestartet wird, klicken Sie auf die Schaltfläche, um die Bezeichnung zu erhöhen.
Herzlichen Glückwunsch! Sie haben eine Watch-App!

![Die im Simulator laufende App](hello-watch-images/running.png)

## <a name="related-links"></a>Verwandte Links

- [Einrichtung und Installation](~/ios/watchos/get-started/installation.md)
- [Video zur ersten Watch-App](https://blog.xamarin.com/your-first-watch-kit-app/)
