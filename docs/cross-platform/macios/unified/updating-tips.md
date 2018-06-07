---
title: Tipps zum Aktualisieren von Code für die einheitliche-API
description: Dieses Dokument erläutert häufige Fehler und verschiedene nützliche Tipps beim Aktualisieren einer Anwendung die Xamarin Unified-API verwenden.
ms.prod: xamarin
ms.assetid: 8DD34D21-342C-48E9-97AA-1B649DD8B61F
author: asb3993
ms.author: amburns
ms.openlocfilehash: cab27d5dc38eeab65728f242c6f11fd445601a88
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782117"
---
# <a name="tips-for-updating-code-to-the-unified-api"></a>Tipps zum Aktualisieren von Code für die einheitliche-API

Mit der automatisierten Migrationstool in Visual Studio für Mac iOS und Mac-Projekten, zum Verweisen auf die einheitliche API (Xamarin.iOS.dll oder Xamarin.Mac.dll), und stellen für Sie viele der erforderlichen codeänderungen konvertiert wird (finden Sie unter der [Xamarin Studio-Version Hinweise im Abschnitt "Klassisch auf einheitliche API-Code-Migrationstool"](http://developer.xamarin.com/releases/studio/xamarin.studio_5.7/xamarin.studio_5.7/) spezifische Details).

## <a name="nsinvalidargumentexception-could-not-find-storyboard-error"></a>Storyboard NSInvalidArgumentException wurde nicht gefunden. Fehler

Es ist ein [Fehler](https://bugzilla.xamarin.com/show_bug.cgi?id=25569) in der aktuellen Version von Visual Studio für Mac, die auftreten können, nachdem Sie die automatisierte Migrationstool beim Konvertieren Ihres Projekts an die Unified-APIs mit. Nach dem Update, wenn Sie eine Fehlermeldung, in der Form erhalten:

```console
Objective-C exception thrown. Name: NSInvalidArgumentException Reason: Could not find a storyboard named 'xxx' in bundle NSBundle...
```

Sie können Folgendes verwenden, um dieses Problem zu beheben, suchen Sie die folgenden Build Zieldatei Aktionen ausführen:

```console
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/2.1/Xamarin.iOS.Common.targets
```

In dieser Datei müssen Sie die folgende Zieldeklaration einer suchen:

```xml
<Target Name="_CopyContentToBundle"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

Und Hinzufügen der `DependsOnTargets="_CollectBundleResources"` -Attribut darauf. Und zwar so:

```xml
<Target Name="_CopyContentToBundle"
        DependsOnTargets="_CollectBundleResources"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

Speichern Sie die Datei, Neustart von Visual Studio für Mac, und führen Sie eine Neuinstallation & neu erstellen des Projekts. Eine Behebung dieses Problems sollte in Kürze von Xamarin freigegeben werden.

## <a name="useful-tips"></a>Nützliche Tipps

Nach der Verwendung des Migrationsprogramms, erhalten Sie möglicherweise immer noch einige Compilerfehlern Benutzereingriff.
Schließen einige Dinge, die möglicherweise manuell behoben werden müssen:

* Vergleichen von `enum`s möglicherweise ein `(int)` Umwandlung.

* `NSDictionary.IntValue` Gibt jetzt eine `nint`, besteht eine `Int32Value` kann stattdessen verwendet werden.

* `nfloat` und `nint` Typen können nicht gekennzeichnet werden `const`;   `static readonly nint` ist eine sinnvolle Alternative.

* Dinge, die verwendet wird, werden direkt in die `MonoTouch.` Namespace stehen jetzt in der Regel in der `ObjCRuntime.` Namespace (z. B.: `MonoTouch.Constants.Version` ist jetzt `ObjCRuntime.Constants.Version`).

* Code, der die Objekte serialisiert möglicherweise unterbrochen, beim Serialisieren `nint` und `nfloat` Typen. Achten Sie darauf, um zu überprüfen, dass Ihre Serialisierungscode funktioniert wie erwartet nach der Migration.

* In einigen Fällen automatisierte Tool fehlgeschlagenen Zugriffe auf den Code in `#if #else` bedingte Compilerdirektiven. In diesem Fall müssen Sie die Updates manuell vornehmen (siehe die unten aufgeführten allgemeine Fehler).

* Manuell exportierte Methoden mit `[Export]` kann möglicherweise nicht automatisch vom Migrationstool, z. B. in diesem Code Snippert müssen Sie manuell aktualisieren, den Rückgabetyp zu behoben `nfloat`:

    ```csharp
    [Export("tableView:heightForRowAtIndexPath:")]
    public nfloat HeightForRow(UITableView tableView, NSIndexPath indexPath)
    ```

 * Die einheitliche API bietet keine implizite Konvertierung zwischen NSDate und .NET "DateTime", da es sich nicht um eine verlustfreie Konvertierung ist. Um zu verhindern, dass der Fehler im Zusammenhang mit `DateTimeKind.Unspecified` konvertieren .NET `DateTime` auf local oder vor der Konvertierung zu UTC `NSDate`.

 * Objective-C-Kategorie Methoden als Erweiterungsmethoden in den einheitliche API jetzt generiert. Code, der zuvor verwendet beispielsweise `UIView.DrawString` würde verweisen, jetzt `NSString.DrawString` in die einheitliche API.

 * Code mit AVFoundation Klassen mit `VideoSettings` Verwendung ändern sollte die `WeakVideoSettings` Eigenschaft. Dies erfordert eine `Dictionary`, steht als Eigenschaft für die Anwendungseinstellungen-Klassen, z. B.:

    ```csharp
    vidrec.WeakVideoSettings = new AVVideoSettings() { ... }.Dictionary;
    ```

 * Die NSObject `.ctor(IntPtr)` Konstruktor wurde geändert öffentlich sein, um geschützte ([um zu verhindern, dass die falsche Verwendung](~/cross-platform/macios/unified/overview.md#NSObject_ctor)).

 * `NSAction` wurde [ersetzt](~/cross-platform/macios/unified/overview.md#NSAction) mit dem Starndard .NET `Action`. Einige einfache (einzelnen Parameter) Delegaten auch durch ersetzt wurden `Action<T>`.

Schließlich finden Sie in der [klassischen v einheitliche API-Unterschiede](http://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) um Änderungen an APIs im Code zu suchen. Suche [auf dieser Seite](http://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) hilft suchen Classic-APIs und was sie auf aktualisiert haben.

**Hinweis:** der `MonoTouch.Dialog` Namespace bleibt unverändert, nach der Migration. Wenn im Code verwendet **MonoTouch.Dialog** sollten weiterhin diesen Namespace - dies *nicht* ändern `MonoTouch.Dialog` zu `Dialog`!

## <a name="common-compiler-errors"></a>Häufige Compilerfehler

Weitere Beispiele für häufige Fehler sind nachfolgend, zusammen mit der Projektmappe aufgeführt:

**Fehler CS0012: Der Typ "MonoTouch.UIKit.UIView" ist in einer Assembly definiert, die nicht verwiesen wird.**

Fix: Dies bedeutet normalerweise, dass das Projekt verweist auf eine Komponente oder ein NuGet-Paket, das nicht mit der Unified-API erstellt wurde. Sie sollten auch löschen und erneut hinzufügen, alle Komponenten und NuGet-Pakete. Wenn dadurch den Fehler nicht behoben wird, kann die externe Bibliothek noch keine einheitliche-API unterstützt.

**Fehler MT0034: Kann nicht im selben Xamarin.iOS Projekt - "monotouch.dll" und "Xamarin.iOS.dll" enthalten explizit "Xamarin.iOS.dll" verwiesen wird, während "monotouch.dll" verwiesen wird "Xamarin.Mobile, Version = 0.6.3.0, Culture = Neutral, PublicKeyToken = Null ".**

Fix: Löschen Sie die Komponente, die diesen Fehler verursacht und wieder zum Projekt hinzu.

**Fehler CS0234: Der Typ oder Namespacename 'Foundation' im Namespace 'MonoTouch' nicht vorhanden ist. Fehlt einen Assemblyverweis?**

Fix: Die automatisierte Migration-Tool in Visual Studio für Mac *sollten* aktualisiert alle `MonoTouch.Foundation` Verweise auf `Foundation`, jedoch in einigen Fällen müssen diese manuell aktualisiert werden. Ähnliche Fehler können angezeigt werden, für die anderen Namespaces, die zuvor in enthaltenen `MonoTouch`, wie z. B. `UIKit`.

**Fehler CS0266: Typ "double", "System.float" kann nicht implizit konvertiert werden.**

Fix: ändern und umgewandelt `nfloat`. Dieser Fehler kann auch für andere Typen mit 64-Bit-äquivalente auftreten (z. B. `nint`,)

```csharp
nfloat scale = (nfloat)Math.Min(rect.Width, rect.Height);
```

**Fehler CS0266: Typ "CoreGraphics.CGRect", "System.Drawing.RectangleF" kann nicht implizit konvertiert werden. Eine explizite Konvertierung vorhanden (fehlt eine Umwandlung?)**

Fix: Ändern von Instanzen, die `RectangleF` auf `CGRect`, `SizeF` auf `CGSize`, und `PointF` auf `CGPoint`. Der Namespace `using System.Drawing;` ersetzt werden sollte, mit `using CoreGraphics;` (sofern er nicht bereits vorhanden ist).

**Fehler CS1502: die beste überladene Methode Übereinstimmung für "CoreGraphics.CGContext.SetLineDash (System.nfloat, System.nfloat[])" enthält einige ungültige Argumente**

Fix: Ändern der Arraytyp `nfloat[]` und explizit umgewandelt `Math.PI`.

```csharp
grphc.SetLineDash (0, new nfloat[] { 0, 3 * (nfloat)Math.PI });
```

**Fehler CS0115: "WordsTableSource.RowsInSection (UIKit.UITableView, Int)" wird als eine Außerkraftsetzung, aber keine geeignete Methode zum Überschreiben gefunden gekennzeichnet.**

Fix: Ändern Sie die Rückgabetypen für Wert und Parameter zu `nint`. Dieser häufig Fehler tritt in methodenüberschreibungen datenprofilen auf `UITableViewSource`, einschließlich `RowsInSection`, `NumberOfSections`, `GetHeightForRow`, `TitleForHeader`, `GetViewForHeader`usw.

```csharp
public override nint RowsInSection (UITableView tableview, nint section) {
```

**Fehler-CS0508: `WordsTableSource.NumberOfSections(UIKit.UITableView)': return type must be 'System.nint' to match overridden member `UIKit.UITableViewSource.NumberOfSections(UIKit.UITableView) "**

Fix: Wenn der Rückgabetyp wird geändert zu `nint`, wandeln Sie den Rückgabewert in `nint`.

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    return (nint)navItems.Count;
}
```

**Fehler CS1061: Typ "CoreGraphics.CGPath" ist keine Definition für "AddElipseInRect" enthalten**

Fix: Korrigieren, `AddEllipseInRect`. Andere Änderungen des Computernamens enthalten:

* Ändern Sie in "Color.Black" `NSColor.Black`.
* Ändern Sie MapKit "AddAnnotation" in `AddAnnotations`.
* Ändern Sie AVFoundation "DataUsingEncoding" in `Encode`.
* Ändern Sie AVFoundation "AVMetadataObject.TypeQRCode" in `AVMetadataObjectType.QRCode`.
* Ändern Sie AVFoundation "VideoSettings" in `WeakVideoSettings`.
* Ändern von PopViewControllerAnimated auf `PopViewController`.
* Ändern Sie CoreGraphics "CGBitmapContext.SetRGBFillColor" in `SetFillColor`.

**Fehler-CS0546: kann nicht überschrieben werden, weil "MapKit.MKAnnotation.Coordinate" keinen überschreibbaren Set-Accessor (CS0546)**

Wenn eine benutzerdefinierte Anmerkung zu erstellen, indem das Erstellen von Unterklassen für MKAnnotation weist Feld-Koordinate keine Set-Methode, nur einen Getter.

[Beheben Sie](https://forums.xamarin.com/discussion/comment/109505/#Comment_109505):

* Hinzufügen eines Felds zum Nachverfolgen der Koordinate.
* Dieses Feld in der Getter für eine der-Koordinate Eigenschaft zurückgeben
* Überschreiben Sie die SetCoordinate-Methode, und legen Sie das Feld
* Rufen Sie SetCoordinate in Ihre Ctor mit dem übergebenen-Koordinate-parameter

Es sollte etwa wie folgt aussehen:

```csharp
class BasicPinAnnotation : MKAnnotation
{
    private CLLocationCoordinate2D _coordinate;

    public override CLLocationCoordinate2D Coordinate
    {
        get
        {
            return _coordinate;
        }
    }

    public override void SetCoordinate(CLLocationCoordinate2D value)
    {
        _coordinate = value;
    }

    public BasicPinAnnotation (CLLocationCoordinate2D coordinate)
    {
        SetCoordinate(coordinate);
    }
}
```

## <a name="related-links"></a>Verwandte Links

- [Aktualisieren von Apps](~/cross-platform/macios/unified/updating-apps.md)
- [Aktualisieren von iOS-Apps](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Aktualisieren von Apps für Mac](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Aktualisieren von Apps mit Xamarin.Forms](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Aktualisieren von Bindungen](~/cross-platform/macios/unified/update-binding.md)
- [Arbeiten mit nativen Typen in plattformübergreifenden Apps](~/cross-platform/macios/native-types-cross-platform.md)
- [Klassische Vs einheitliche API-Unterschiede](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
