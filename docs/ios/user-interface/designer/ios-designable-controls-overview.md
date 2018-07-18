---
title: Benutzerdefinierte Steuerelemente im Xamarin Designer für iOS
description: Der Xamarin-Designer für iOS unterstützt benutzerdefiniertes Rendern von Steuerelementen im Projekt erstellt oder aus externen Quellen wie der Xamarin Component Store verwiesen wird.
ms.prod: xamarin
ms.assetid: D8F07D63-B006-4050-9D1B-AC6FCDA71B99
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 05b190f4bfd4058e9e2f6e465e6026fa76dce6f4
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995696"
---
# <a name="custom-controls-in-the-xamarin-designer-for-ios"></a>Benutzerdefinierte Steuerelemente im Xamarin Designer für iOS

_Der Xamarin-Designer für iOS unterstützt benutzerdefiniertes Rendern von Steuerelementen im Projekt erstellt oder aus externen Quellen wie der Xamarin Component Store verwiesen wird._

Die Xamarin-Designer für iOS ist ein leistungsfähiges Tool für die visuelle Darstellung der Benutzeroberfläche einer Anwendung und bietet WYSIWYG-Bearbeitung Unterstützung für die meisten iOS-Ansichten und ansichtscontroller. Ihre app kann auch benutzerdefinierte Steuerelemente enthalten, die in iOS integriert diejenigen zu erweitern. Wenn diese benutzerdefinierten Steuerelemente mit ein paar Richtlinien wider geschrieben werden, können sie auch vom iOS-Designer bereitstellen noch bessere bearbeitungsmöglichkeiten gerendert werden. Dieses Dokument befasst sich diese Richtlinien.

## <a name="requirements"></a>Anforderungen

Ein Steuerelement, das alle folgenden Anforderungen erfüllt, wird auf der Entwurfsoberfläche gerendert werden:

1.  Es ist eine direkte oder indirekte Unterklasse der [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/) oder [UIViewController](https://developer.xamarin.com/api/type/UIKit.UIView/Controller). Andere [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) Unterklassen werden als Symbol auf der Entwurfsoberfläche angezeigt.
2.  Es wurde eine [RegisterAttribute](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) , die sie für Objective-c verfügbar machen
3.  Sie verfügt über [der erforderliche IntPtr-Konstruktor](~/ios/internals/api-design/index.md).
4.  Entweder implementiert die [IComponent](xref:System.ComponentModel.IComponent) -Schnittstelle oder verfügt über eine [DesignTimeVisibleAttribute](xref:System.ComponentModel.DesignTimeVisibleAttribute) auf "true" festgelegt.

Steuerelemente, die im Code definiert, die die oben genannten Anforderungen erfüllen werden im Designer angezeigt, wenn für den Simulator für das enthaltende Projekt kompiliert wird. Standardmäßig werden alle benutzerdefinierten Steuerelemente angezeigt, der **benutzerdefinierte Komponenten** Teil der **Toolbox**. Jedoch [CategoryAttribute](xref:System.ComponentModel.CategoryAttribute) kann angewendet werden, um das benutzerdefinierte Steuerelement-Klasse, um einen anderen Abschnitt anzugeben.

Der Designer unterstützt das Laden von Objective-C-Bibliotheken von Drittanbietern nicht.

## <a name="custom-properties"></a>Benutzerdefinierte Eigenschaften

Eine Eigenschaft deklariert, indem ein benutzerdefiniertes Steuerelement wird im Eigenschaftenbereich angezeigt, wenn die folgenden Bedingungen erfüllt sind:

1.  Die Eigenschaft weist einen öffentlichen Getter und Setter.
1.  Die Eigenschaft weist einen [ExportAttribute](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) als auch ein [BrowsableAttribute](xref:System.ComponentModel.BrowsableAttribute) auf "true" festgelegt.
1.  Der Eigenschaftentyp ist ein numerischer Typ, den Enumerationstyp, die Zeichenfolge, die "bool", [SizeF](xref:System.Drawing.SizeF), [UIColor](https://developer.xamarin.com/api/type/UIKit.UIColor/), oder [UIImage](https://developer.xamarin.com/api/type/UIKit.UIImage/). Diese Liste der unterstützten Typen kann in Zukunft erweitert werden.


Die Eigenschaft kann auch mit ergänzt werden, eine [DisplayNameAttribute](xref:System.ComponentModel.DisplayNameAttribute) an die Bezeichnung, die Sie im Eigenschaftenbereich angezeigt wird.

## <a name="initialization"></a>Initialisierung

Für `UIViewController` Unterklassen, verwenden Sie die [ViewDidLoad](https://developer.xamarin.com/api/member/UIKit.UIViewController.ViewDidLoad/) -Methode für Code, Sie im Designer erstellten, Ansichten abhängt.

Für `UIView` und andere `NSObject` Unterklassen, die ["AwakeFromNib"](https://developer.xamarin.com/api/member/Foundation.NSObject.AwakeFromNib/) Methode ist der empfohlene Platz zum Ausführen der Initialisierung des benutzerdefinierten Steuerelements nach dem sie aus der Layoutdatei geladen wird. Dies ist, da alle benutzerdefinierten Eigenschaften legen Sie in den Eigenschaftenbereich nicht festgelegt werden, wenn es sich bei Konstruktor des Steuerelements ausgeführt wird, aber sie werden festgelegt werden, bevor Sie `AwakeFromNib` aufgerufen wird:


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

Wenn Sie auch das Steuerelement entworfen wurde, direkt aus dem Code erstellt werden, sollten Sie eine Methode erstellen, die allgemeine Initialisierungscode, wie folgt:

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

## <a name="property-initialization-and-awakefromnib"></a>Initialisierung und "AwakeFromNib"

Vorsichtig sollte vorgenommen werden, wann und wo initialisieren entworfen Eigenschaften in einer benutzerdefinierten Komponente, die Werte, die im iOS-Designer festgelegt wurden, nicht überschrieben. Führen Sie beispielsweise den folgenden Code ein:

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

Die `CustomView` Komponente macht eine `Counter` -Eigenschaft, die vom Entwickler in der iOS-Designer festgelegt werden kann. Jedoch unabhängig davon, welcher Wert, in den Designer, den Wert des festgelegt ist der `Counter` Eigenschaft wird immer 0 (null) sein. Erläuterung:

-  Eine Instanz von der `CustomControl` aus der Storyboard-Datei vergrößert wird.
-  Alle Eigenschaften, die in der iOS-Designer geändert werden festgelegt (wie z. B. das Festlegen des Werts von der `Counter` zu zwei (2), z. B.).
-  Die `AwakeFromNib` Methode ausgeführt wird, und ein Aufruf erfolgt, um der Komponente `Initialize` Methode.
-  In `Initialize` den Wert des der `Counter` Eigenschaft wird auf 0 (null) zurückgesetzt wird.


Entweder um die oben genannten Situation zu beheben, Initialisieren der `Counter` Eigenschaft an anderer Stelle (z. B. in den Konstruktor der Komponente) oder nicht überschreiben die `AwakeFromNib` Methode, und rufen `Initialize` Wenn für die Komponente keine weitere Initialisierung außerhalb von was erforderlich ist wird derzeit von seiner Konstruktoren behandelt wird.

## <a name="design-mode"></a>Entwurfsmodus

Ein benutzerdefiniertes Steuerelement muss auf der Entwurfsoberfläche ein paar Einschränkungen entsprechen:

-  App-Bundle-Ressourcen sind nicht verfügbar ist, im Entwurfsmodus befindet. Images sind verfügbar, wenn über geladen [UIImage Methoden](https://developer.xamarin.com/api/type/UIKit.UIImage/%2fM) .
-  Asynchrone Vorgänge, z. B. webanforderungen, sollte nicht im Entwurfsmodus ausgeführt werden. Die Entwurfsoberfläche unterstützt keine Animationen oder anderen asynchronen Updates für die Benutzeroberfläche des Steuerelements.


Ein benutzerdefiniertes Steuerelement implementieren kann [IComponent](xref:System.ComponentModel.IComponent) und verwenden Sie die [DesignMode](xref:System.ComponentModel.ISite.DesignMode) Eigenschaft zu überprüfen, ob sie auf der Entwurfsoberfläche angezeigt wird. In diesem Beispiel wird die Bezeichnung "Entwurfsmodus" auf der Entwurfsoberfläche und die "Runtime" zur Laufzeit angezeigt:

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
        if (Site != null &amp;&amp; Site.DesignMode)
            Text = "Design Mode";
        else
            Text = "Runtime";
    }
}
```

Sie sollten immer überprüfen die `Site` -Eigenschaft für `null` bevor Sie eines ihrer Elemente zugreifen. Wenn `Site` ist `null`, es ist sicherer, davon aus, das Steuerelement nicht im Designer ausgeführt wird.
Im Entwurfsmodus `Site` wird festgelegt, nach dem Ausführen der Konstruktor des Steuerelements und vor dem `AwakeFromNib` aufgerufen wird.

## <a name="debugging"></a>Debuggen

Ein Steuerelement, das die oben genannten Anforderungen erfüllt werden in der Toolbox angezeigt und auf der Oberfläche gerendert werden.
Wenn ein Steuerelement nicht gerendert wird, überprüfen Sie Fehler in das Steuerelement oder eine ihrer Abhängigkeiten.

Die Entwurfsoberfläche kann häufig von einzelnen Steuerelementen und anderen Steuerelementen Rendern weiterhin ausgelöste Ausnahmen abfangen. Das fehlerhafte Steuerelement wird mit einem roten Platzhalter ersetzt, und sehen Sie die ausnahmeablaufverfolgung durch Klicken auf das Ausrufezeichensymbol:

 ![](ios-designable-controls-overview-images/exception-box.png "Ein fehlerhafter Steuerelement als rote Platzhalter und die Details der Ausnahme")

Wenn Debugsymbole für das Steuerelement verfügbar sind, sind für die Ablaufverfolgung müssen Dateinamen und Zeilennummern. Doppelklicken auf eine Zeile in der stapelüberwachung springt zu dieser Zeile im Quellcode.

Wenn der Designer das fehlerhafte Steuerelement nicht isolieren kann, wird eine Warnmeldung am oberen Rand der Entwurfsoberfläche angezeigt:

 ![](ios-designable-controls-overview-images/info-bar.png "Eine Warnmeldung am oberen Rand der Entwurfsoberfläche")

Vollständige Rendering wird fortgesetzt, wenn das fehlerhafte Steuerelement ist fixiert oder auf der Entwurfsoberfläche entfernt.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die Erstellung und Anwendung benutzerdefinierter Steuerelemente im iOS Designer eingeführt. Zuerst beschrieben sie Anforderungen, die Steuerelemente erfüllen müssen, um auf der Entwurfsoberfläche gerendert werden, und benutzerdefinierte Eigenschaften im Eigenschaftenbereich verfügbar machen. Sie finden ihn dann an der Code-behind - Initialisierung des Steuerelements und die DesignMode-Eigenschaft. Schließlich es wurde beschrieben, was geschieht, wenn Ausnahmen ausgelöst werden und zur Lösung dieses Problems.