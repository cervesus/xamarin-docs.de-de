---
title: Benutzerdefinierte Steuerelemente in die Xamarin-Designer für iOS
description: Die Xamarin-Designer für iOS unterstützt benutzerdefinierte Rendern von Steuerelementen in Ihrem Projekt erstellt oder aus externen Quellen, wie der Xamarin-Komponentenspeicher verwiesen wird.
ms.prod: xamarin
ms.assetid: D8F07D63-B006-4050-9D1B-AC6FCDA71B99
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 113fab2fd0d1a055d566606885cefbafe3185529
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="custom-controls-in-the-xamarin-designer-for-ios"></a>Benutzerdefinierte Steuerelemente in die Xamarin-Designer für iOS

_Die Xamarin-Designer für iOS unterstützt benutzerdefinierte Rendern von Steuerelementen in Ihrem Projekt erstellt oder aus externen Quellen, wie der Xamarin-Komponentenspeicher verwiesen wird._

Der Xamarin-Designer für iOS ist ein leistungsstarkes Tool für die visuelle Darstellung der Benutzeroberfläche einer Anwendung und bietet WYSIWYG-Unterstützung für die meisten iOS Ansichten und Controllern Ansicht bearbeiten. Ihre app kann auch benutzerdefinierte Steuerelemente enthalten, die diejenigen iOS integriert erweitern. Wenn einige Richtlinien beachten Sie diese benutzerdefinierten Steuerelemente geschrieben werden, können sie auch von der iOS-Designer bietet eine noch umfassendere Bearbeitungsoberfläche gerendert werden. Dieses Dokument untersucht, mit diesen Richtlinien.

## <a name="requirements"></a>Anforderungen

Ein Steuerelement, das die folgenden Anforderungen erfüllt, wird auf der Entwurfsoberfläche gerendert werden:

1.  Es ist eine direkte oder indirekte Unterklasse von [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/) oder [UIViewController](https://developer.xamarin.com/api/type/UIKit.UIView/Controller). Andere [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) Unterklassen werden als Symbol auf der Entwurfsoberfläche angezeigt.
2.  Es wurde eine [RegisterAttribute](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) für die bereitzustellenden zu Objective-c aus.
3.  Es wurde [erforderliche IntPtr-Konstruktor](~/ios/internals/api-design/index.md).
4.  Entweder implementiert die [IComponent](https://developer.xamarin.com/api/type/System.ComponentModel.IComponent/) -Schnittstelle oder hat ein [DesignTimeVisibleAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.DesignTimeVisibleAttribute/) auf "true" festgelegt.

Steuerelemente, die im Code definiert, die die oben genannten Anforderungen erfüllt werden im Designer angezeigt, wenn für den Simulator seines enthaltenden Projekts kompiliert wird. Standardmäßig werden alle benutzerdefinierten Steuerelemente angezeigt, der **benutzerdefinierte Komponenten** Teil der **Toolbox**. Jedoch [CategoryAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.CategoryAttribute/) kann angewendet werden, um das benutzerdefinierte Steuerelement-Klasse, um einen anderen Abschnitt anzugeben.

Der Designer unterstützt das Laden anderer Bibliotheken von Drittanbietern Objective-C nicht.

## <a name="custom-properties"></a>Benutzerdefinierte Eigenschaften

Eine Eigenschaft deklariert, indem ein benutzerdefiniertes Steuerelement wird im Eigenschaftenbereich angezeigt, wenn die folgenden Bedingungen erfüllt sind:

1.  Die Eigenschaft weist einen öffentlichen Getter und Setter-Methode.
1.  Die Eigenschaft weist ein [ExportAttribute](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) sowie ein [BrowsableAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.BrowsableAttribute/) auf "true" festgelegt.
1.  Der Eigenschaftstyp ist einer einen numerischen Typ, Enumerationstyp, String, Bool, [SizeF](https://developer.xamarin.com/api/type/System.Drawing.SizeF/), [UIColor](https://developer.xamarin.com/api/type/UIKit.UIColor/), oder [UIImage](https://developer.xamarin.com/api/type/UIKit.UIImage/). Diese Liste der unterstützten Typen kann in der Zukunft erweitert werden.


Die Eigenschaft kann auch mit ergänzt werden eine [DisplayNameAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.DisplayNameAttribute/) an die Bezeichnung, die für sie im Eigenschaftenbereich angezeigt wird.

## <a name="initialization"></a>Initialisierung

Für `UIViewController` Unterklassen, verwenden Sie die [ViewDidLoad](https://developer.xamarin.com/api/member/UIKit.UIViewController.ViewDidLoad/) Methode für Code, Sie im Designer erstellte, Sichten abhängt.

Für `UIView` und andere `NSObject` Unterklassen, die [AwakeFromNib](https://developer.xamarin.com/api/member/Foundation.NSObject.AwakeFromNib/) Methode ist die empfohlene Stelle aus, um die Initialisierung eines benutzerdefinierten Steuerelements ausführen, nachdem es die Layoutdatei geladen wird. Dies ist, da die benutzerdefinierten Eigenschaften im Eigenschaftenbereich festlegen nicht festgelegt werden, wenn der Konstruktor des Steuerelements wird ausgeführt, aber sie werden vor dem festgelegt `AwakeFromNib` aufgerufen wird:


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

Wen das Steuerelement auch direkt aus Code erstellt werden, sollten Sie eine Methode erstellen, die allgemeine Initialisierungscode wie folgt:

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

## <a name="property-initialization-and-awakefromnib"></a>Eigenschaftsinitialisierung und AwakeFromNib

Für wann und wo initialisieren entworfen Eigenschaften in einer benutzerdefinierten Komponente zum Überschreiben nicht die Werte, die in der iOS-Designer festgelegt wurden, sollte vorsichtig vorgenommen werden. Nehmen Sie beispielsweise den folgenden Code ein:

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

Die `CustomView` Komponente verfügbar macht eine `Counter` -Eigenschaft, die vom Entwickler in der iOS-Designer festgelegt werden kann. Jedoch unabhängig davon, welcher Wert, in den Designer, den Wert des festgelegt ist der `Counter` -Eigenschaft wird immer 0 (null) sein. Erläuterung:

-  Eine Instanz von der `CustomControl` aus der Storyboard-Datei vergrößert wird.
-  Alle Eigenschaften in der iOS-Designer geändert werden festgelegt (wie z. B. das Festlegen des Werts der `Counter` zu zwei (2), z. B.).
-  Die `AwakeFromNib` -Methode wird ausgeführt und erfolgt ein Aufruf an der Komponente `Initialize` Methode.
-  In `Initialize` den Wert von der `Counter` Eigenschaft auf 0 (null) zurückgesetzt wird.


Um die oben genannten Situation zu beheben, initialisieren Sie entweder die `Counter` Eigenschaft an anderer Stelle (z. B. wie in der Komponente Konstruktor) oder nicht außer Kraft setzen die `AwakeFromNib` Methode, und rufen `Initialize` Wenn die Komponente keine weitere Initialisierung außerhalb der was erforderlich ist wird derzeit von seiner Konstruktoren behandelt wird.

## <a name="design-mode"></a>Entwurfsmodus

Auf der Entwurfsoberfläche angezeigt wird muss ein benutzerdefiniertes Steuerelement einige Einschränkungen entsprechen:

-  App-Bündel-Ressourcen stehen nicht im Entwurfsmodus. Images sind verfügbar, wenn durch geladen [UIImage Methoden](https://developer.xamarin.com/api/type/UIKit.UIImage/%2fM) .
-  Asynchrone Vorgänge, z. B. webanforderungen sollten nicht im Entwurfsmodus ausgeführt werden. Die Entwurfsoberfläche unterstützt keine Animationen oder andere asynchroner Updates an das Steuerelement-Benutzeroberfläche.


Ein benutzerdefiniertes Steuerelement implementieren kann [IComponent](https://developer.xamarin.com/api/type/System.ComponentModel.IComponent/) und Verwenden der [DesignMode](https://developer.xamarin.com/api/property/System.ComponentModel.ISite.DesignMode/) Eigenschaft zu überprüfen, ob sie auf der Entwurfsoberfläche angezeigt wird. In diesem Beispiel wird die Bezeichnung "Entwurfsmodus" auf der Entwurfsoberfläche und die "Laufzeit" zur Laufzeit angezeigt werden:

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

Überprüfen Sie immer die `Site` -Eigenschaft für `null` bevor Sie versuchen, eines seiner Member zugreifen. Wenn `Site` ist `null`, wird davon ausgegangen, das Steuerelement im Designer nicht ausgeführt wird.
Im Entwurfsmodus `Site` wird festgelegt, nachdem das Steuerelement Konstruktor ausgeführt wurde und bevor `AwakeFromNib` aufgerufen wird.

## <a name="debugging"></a>Debuggen

Ein Steuerelement, das die oben genannten Anforderungen erfüllt werden in der Toolbox angezeigt und auf der Oberfläche gerendert werden.
Wenn ein Steuerelement nicht gerendert wird, überprüfen Sie für Fehler in das Steuerelement oder eine ihrer Abhängigkeiten.

Die Entwurfsoberfläche ziehen kann häufig Abfangen von Ausnahmen, die von einzelnen Steuerelementen ausgelöst werden, während Sie den Vorgang fortsetzen zu anderen Steuerelementen so gerendert werden. Das fehlerhafte Steuerelement wird durch einen roten Platzhalter ersetzt, und Sie können die ausnahmeablaufverfolgung anzeigen, indem Sie durch Klicken auf das Symbol "Ausrufezeichen":

 ![](ios-designable-controls-overview-images/exception-box.png "Ein fehlerhaftes Steuerelement als rote Platzhalter und die Details der Ausnahme")

Wenn Sie Debugsymbole für das Steuerelement verfügbar sind, müssen die Ablaufverfolgung Dateinamen und Zeilennummern. Doppelklick auf eine Zeile in die stapelüberwachung springt in die entsprechende Zeile in den Quellcode einfügen.

Wenn der Designer das fehlerhafte Steuerelement nicht isolieren kann, wird am oberen Rand der Entwurfsoberfläche eine Warnmeldung angezeigt:

 ![](ios-designable-controls-overview-images/info-bar.png "Eine Warnmeldung am oberen Rand der Entwurfsoberfläche")

Vollständige Rendering wird fortgesetzt, wenn das fehlerhafte Steuerelement korrigiert oder aus der Entwurfsoberfläche entfernt wird.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel eingeführt, die Erstellung und Anwendung von benutzerdefinierten Steuerelementen in der iOS-Designer. Zuerst beschrieben es an, denen Steuerelemente erfüllen müssen, um auf der Entwurfsoberfläche gerendert werden, und benutzerdefinierte Eigenschaften im Eigenschaftenbereich verfügbar machen. Er erläutert, klicken Sie dann die CodeBehind - Initialisierung des Steuerelements und die DesignMode-Eigenschaft. Schließlich es beschrieben, was geschieht, wenn Ausnahmen ausgelöst werden und zum Beheben dieses Problems.