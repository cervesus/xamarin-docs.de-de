---
title: Benutzerdefinierte Steuerelemente im Xamarin Designer für IOS
description: Der Xamarin Designer für IOS unterstützt das Rendern von benutzerdefinierten Steuerelementen, die in Ihrem Projekt erstellt wurden oder von externen Quellen wie dem xamarin Component Store referenziert
ms.prod: xamarin
ms.assetid: D8F07D63-B006-4050-9D1B-AC6FCDA71B99
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/22/2017
ms.openlocfilehash: e934059f5428780ea19917068503b58961ac5673
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70284182"
---
# <a name="custom-controls-in-the-xamarin-designer-for-ios"></a>Benutzerdefinierte Steuerelemente im Xamarin Designer für IOS

_Der Xamarin Designer für IOS unterstützt das Rendern von benutzerdefinierten Steuerelementen, die in Ihrem Projekt erstellt wurden oder von externen Quellen wie dem xamarin Component Store referenziert_

Der Xamarin Designer für IOS ist ein leistungsfähiges Tool zum Visualisieren der Benutzeroberfläche einer Anwendung und bietet Unterstützung für die WYSIWYG-Bearbeitung für die meisten IOS-Ansichten und Ansichts Controller. Ihre APP kann auch benutzerdefinierte Steuerelemente enthalten, mit denen die in ios integrierten Erweiterungen erweitert werden. Wenn diese benutzerdefinierten Steuerelemente mit wenigen Richtlinien geschrieben sind, können Sie auch durch den IOS-Designer gerendert werden, was eine noch umfassendere Bearbeitungsfunktion ermöglicht. Dieses Dokument befasst sich mit diesen Richtlinien.

## <a name="requirements"></a>Anforderungen

Ein Steuerelement, das alle folgenden Anforderungen erfüllt, wird auf der Entwurfs Oberfläche gerendert:

1. Es handelt sich um eine direkte oder indirekte Unterklasse von [UIView](xref:UIKit.UIView) oder [UIViewController](xref:UIKit.UIViewController). Andere [NSObject](xref:Foundation.NSObject) -Unterklassen werden als Symbol auf der Entwurfs Oberfläche angezeigt.
2. Es verfügt über ein [RegisterAttribute](xref:Foundation.RegisterAttribute) , um es für Target-C verfügbar zu machen.
3. Er verfügt über [den erforderlichen IntPtr-Konstruktor](~/ios/internals/api-design/index.md).
4. Entweder wird die [IComponent](xref:System.ComponentModel.IComponent) -Schnittstelle implementiert, oder ein [DesignTimeVisibleAttribute-Attribut](xref:System.ComponentModel.DesignTimeVisibleAttribute) ist auf true festgelegt.

Im Code definierte Steuerelemente, die die oben genannten Anforderungen erfüllen, werden im Designer angezeigt, wenn Ihr enthaltende Projekt für den Simulator kompiliert wird. Standardmäßig werden alle benutzerdefinierten Steuerelemente im Abschnitt **benutzerdefinierte Komponenten** der **Toolbox**angezeigt. Allerdings kann das [CategoryAttribute](xref:System.ComponentModel.CategoryAttribute) auf die Klasse des benutzerdefinierten Steuer Elements angewendet werden, um einen anderen Abschnitt anzugeben.

Der Designer unterstützt das Laden von Ziel-C-Bibliotheken von Drittanbietern nicht.

## <a name="custom-properties"></a>Benutzerdefinierte Eigenschaften

Eine Eigenschaft, die von einem benutzerdefinierten Steuerelement deklariert wird, wird im Eigenschaften Panel angezeigt, wenn die folgenden Bedingungen erfüllt sind:

1. Die-Eigenschaft verfügt über einen öffentlichen Getter und Setter.
1. Die-Eigenschaft verfügt über ein [Export Attribute-Attribut](xref:Foundation.ExportAttribute) , und ein [BrowsableAttribute-Attribut](xref:System.ComponentModel.BrowsableAttribute) ist auf true festgelegt.
1. Der Eigenschaftentyp ist ein numerischer Typ, Enumerationstyp, String, bool, [SizeF](xref:System.Drawing.SizeF), [uicolor](xref:UIKit.UIColor)oder [uiimage](xref:UIKit.UIImage). Diese Liste der unterstützten Typen kann in Zukunft erweitert werden.


Die-Eigenschaft kann auch mit einem [DisplayNameAttribute](xref:System.ComponentModel.DisplayNameAttribute) versehen werden, um die Bezeichnung anzugeben, die im Eigenschaften Panel für die Eigenschaft angezeigt wird.

## <a name="initialization"></a>Initialisierung

Für `UIViewController` Unterklassen sollten Sie die [viewDidLoad](xref:UIKit.UIViewController.ViewDidLoad) -Methode für Code verwenden, der von Sichten abhängt, die Sie im Designer erstellt haben.

Für `UIView` und andere `NSObject` Unterklassen ist die [awakeFromNib](xref:Foundation.NSObject.AwakeFromNib) -Methode der empfohlene Ort, um die Initialisierung des benutzerdefinierten Steuer Elements auszuführen, nachdem es aus der Layoutdatei geladen wurde. Dies liegt daran, dass benutzerdefinierte Eigenschaften, die im Eigenschaften Panel festgelegt sind, nicht festgelegt werden, wenn der Konstruktor des Steuer Elements ausgeführt `AwakeFromNib` wird, aber vor dem Aufrufen von festgelegt wird:


```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {

    public CustomView (IntPtr handle) : base (handle) { }

    public override void AwakeFromNib ()
    {
        // Initialize the view here.
    }
}
```

Wenn das Steuerelement auch direkt aus dem Code erstellt werden soll, empfiehlt es sich, eine Methode zu erstellen, die einen allgemeinen Initialisierungs Code aufweist, wie folgt:

```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {

    public CustomView (IntPtr handle) : base (handle) { }

    public CustomView ()
    {
        // Called when created from code.
        Initialize ();
    }

    public override void AwakeFromNib ()
    {
        // Called when loaded from xib or storyboard.
        Initialize ();
    }

    void Initialize ()
    {
        // Common initialization code here.
    }
}
```

## <a name="property-initialization-and-awakefromnib"></a>Eigenschafts Initialisierung und awakeFromNib

Es muss darauf geachtet werden, wann und wo die Entwurfs fähigen Eigenschaften in einer benutzerdefinierten Komponente initialisiert werden sollen, um Werte, die innerhalb des IOS-Designers festgelegt wurden, nicht zu überschreiben. Betrachten Sie beispielsweise den folgenden Code:

```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {

    [Export ("Counter"), Browsable (true)]
    public int Counter {get; set;}

    public CustomView (IntPtr handle) : base (handle) { }

    public CustomView ()
    {
        // Called when created from code.
        Initialize ();
    }

    public override void AwakeFromNib ()
    {
        // Called when loaded from xib or storyboard.
        Initialize ();
    }

    void Initialize ()
    {
        // Common initialization code here.
        Counter = 0;
    }
}
```

Die `CustomView` Komponente macht eine `Counter` Eigenschaft verfügbar, die vom Entwickler innerhalb des IOS-Designers festgelegt werden kann. Unabhängig davon, welcher Wert innerhalb des Designers festgelegt wird, ist der Wert `Counter` der-Eigenschaft immer 0 (null). Erläuterung:

- Eine Instanz von `CustomControl` wird aus der storyboarddatei aufgeblasen.
- Alle Eigenschaften, die im IOS-Designer geändert wurden, werden festgelegt (z. `Counter` b. das Festlegen des Werts von auf zwei (2)).
- Die `AwakeFromNib` -Methode wird ausgeführt, und es wird ein-Rückruf an `Initialize` die-Methode der Komponente vorgenommen.
- Innerhalb `Initialize` des Werts `Counter` der-Eigenschaft wird auf 0 (null) zurückgesetzt.


Um die obige Situation zu beheben, initialisieren Sie `Counter` entweder die-Eigenschaft an anderer Stelle (z. b. im Konstruktor der `AwakeFromNib` Komponente), `Initialize` oder überschreiben Sie die-Methode nicht, und wenden Sie an, wenn die Komponente keine weitere Initialisierung außerhalb der wird derzeit von seinen Konstruktoren verarbeitet.

## <a name="design-mode"></a>Entwurfs Modus

Auf der Entwurfs Oberfläche muss ein benutzerdefiniertes Steuerelement einige Einschränkungen einhalten:

- App-Bündel Ressourcen sind im Entwurfs Modus nicht verfügbar. Bilder sind verfügbar, wenn Sie über [uiimage-Methoden](xref:UIKit.UIImage) geladen werden.
- Asynchrone Vorgänge, z. b. Webanforderungen, sollten nicht im Entwurfs Modus ausgeführt werden. Die Entwurfs Oberfläche unterstützt keine Animation oder andere asynchrone Aktualisierungen der Benutzeroberfläche des Steuer Elements.


Ein benutzerdefiniertes Steuerelement kann [IComponent](xref:System.ComponentModel.IComponent) implementieren und die [designMode](xref:System.ComponentModel.ISite.DesignMode) -Eigenschaft verwenden, um zu überprüfen, ob es sich auf der Entwurfs Oberfläche befindet. In diesem Beispiel wird die Bezeichnung "Entwurfs Modus" auf der Entwurfs Oberfläche und "Runtime" zur Laufzeit angezeigt:

```csharp
[Register ("DesignerAwareLabel")]
public class DesignerAwareLabel : UILabel, IComponent {

    #region IComponent implementation

    public ISite Site { get; set; }
    public event EventHandler Disposed;

    #endregion

    public DesignerAwareLabel (IntPtr handle) : base (handle) { }

    public override void AwakeFromNib ()
    {
        if (Site != null && Site.DesignMode)
            Text = "Design Mode";
        else
            Text = "Runtime";
    }
}
```

Überprüfen Sie immer die `Site` -Eigenschaft `null` für, bevor Sie versuchen, auf eines seiner Member zuzugreifen. Wenn `Site`den Wert hat,kanndavonausgegangenwerden,dassdasSteuerelementnichtimDesignerausgeführtwird.`null`
Im Entwurfs Modus wird `Site` festgelegt, nachdem der Konstruktor des Steuer Elements ausgeführt wurde und `AwakeFromNib` bevor aufgerufen wird.

## <a name="debugging"></a>Debuggen

Ein Steuerelement, das die oben genannten Anforderungen erfüllt, wird in der Toolbox angezeigt und auf der-Oberfläche gerendert.
Wenn ein Steuerelement nicht gerendert wird, überprüfen Sie das Steuerelement auf Fehler oder eine seiner Abhängigkeiten.

Die Entwurfs Oberfläche kann häufig Ausnahmen abfangen, die von einzelnen Steuerelementen ausgelöst werden, während andere Steuerelemente weitergegeben werden. Das fehlerhafte Steuerelement wird durch einen roten Platzhalter ersetzt, und Sie können die Ausnahme Ablauf Verfolgung anzeigen, indem Sie auf das Ausrufezeichen Symbol klicken:

 ![](ios-designable-controls-overview-images/exception-box.png "Ein fehlerhaftes Steuerelement als roter Platzhalter und Ausnahme Details")

Wenn für das Steuerelement Debugsymbole verfügbar sind, enthält die Ablauf Verfolgung Dateinamen und Zeilennummern.
Wenn Sie in der Stapel Überwachung auf eine Zeile doppelklicken, wird diese Zeile im Quellcode angezeigt.

Wenn der Designer das fehlerhafte Steuerelement nicht isolieren kann, wird oben in der Entwurfs Oberfläche eine Warnmeldung angezeigt:

 ![](ios-designable-controls-overview-images/info-bar.png "Eine Warnmeldung am oberen Rand der Entwurfs Oberfläche")

Das vollständige Rendering wird fortgesetzt, wenn das fehlerhafte Steuerelement korrigiert oder von der Entwurfs Oberfläche entfernt wurde.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die Erstellung und Anwendung von benutzerdefinierten Steuerelementen im IOS-Designer erläutert. Zuerst wurden Anforderungen beschrieben, die Steuerelemente erfüllen müssen, damit Sie auf der Entwurfs Oberfläche gerendert werden, und benutzerdefinierte Eigenschaften im Eigenschaften Panel verfügbar machen. Anschließend wurde die Code-Behind-Initialisierung des Steuer Elements und der DesignMode-Eigenschaft betrachtet. Schließlich wurde beschrieben, was geschieht, wenn Ausnahmen ausgelöst werden und wie dieses Problem behoben wird.
