---
title: Tipps zum Aktualisieren von Code für Unified API
description: Dieses Dokument behandelt häufige Fehler und verschiedene Tipps, die hilfreich bei der Aktualisierung einer Anwendung zur Verwendung Xamarins Unified API.
ms.prod: xamarin
ms.assetid: 8DD34D21-342C-48E9-97AA-1B649DD8B61F
ms.date: 03/29/2017
author: asb3993
ms.author: amburns
ms.openlocfilehash: a5083e1d31377caece1b8fb4faf33b6e3ff88202
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61211819"
---
# <a name="tips-for-updating-code-to-the-unified-api"></a>Tipps zum Aktualisieren von Code für Unified API

Beim Aktualisieren älterer Xamarin-Lösungen auf der Unified API können die folgenden Fehler auftreten.

## <a name="nsinvalidargumentexception-could-not-find-storyboard-error"></a>NSInvalidArgumentException wurde nicht gefunden. Storyboard-Fehler

Es gibt eine [Fehler](https://bugzilla.xamarin.com/show_bug.cgi?id=25569) in der aktuellen Version von Visual Studio für Mac, die auftreten können, nach der Verwendung der automatisierten Migrationstool, um Ihr Projekt in das Unified-APIs zu konvertieren. Nach dem Update, wenn Sie eine Fehlermeldung erhalten, die sich in der Form:

```console
Objective-C exception thrown. Name: NSInvalidArgumentException Reason: Could not find a storyboard named 'xxx' in bundle NSBundle...
```

Sie können Folgendes auf dieses Problem beheben, suchen Sie die folgenden Build-Ziel-Datei:

```console
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/2.1/Xamarin.iOS.Common.targets
```

In dieser Datei müssen Sie die folgende Zieldeklaration zu ermitteln:

```xml
<Target Name="_CopyContentToBundle"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

Und fügen die `DependsOnTargets="_CollectBundleResources"` -Attribut an. Und zwar so:

```xml
<Target Name="_CopyContentToBundle"
        DependsOnTargets="_CollectBundleResources"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

Speichern Sie die Datei, Neustart von Visual Studio für Mac, und führen Sie einen Bereinigungs- und Ihres Projekts neu erstellen. Eine Behebung dieses Problems sollten von Xamarin in Kürze veröffentlicht.

## <a name="useful-tips"></a>Nützliche Tipps

Das Migrationstool aus verwenden, erhalten Sie möglicherweise immer noch einige Compiler-Fehler manuell eingreifen.
Einige Dinge, die möglicherweise manuell behoben werden müssen, gehören:

* Vergleichen von `enum`erfordern möglicherweise eine `(int)` Umwandlung.

* `NSDictionary.IntValue` Gibt jetzt eine `nint`, gibt es eine `Int32Value` stattdessen verwendet werden kann.

* `nfloat` und `nint` Typen können nicht markiert werden `const`;   `static readonly nint` ist eine sinnvolle Alternative.

* Dinge, die verwendet, um direkt im werden die `MonoTouch.` Namespace stehen jetzt in der Regel in der `ObjCRuntime.` Namespace (z. B.: `MonoTouch.Constants.Version` ist jetzt `ObjCRuntime.Constants.Version`).

* Code, der die Objekte serialisiert möglicherweise unterbrochen, beim Serialisieren `nint` und `nfloat` Typen. Achten Sie darauf, um zu überprüfen, dass Ihre Serialisierungscode funktioniert wie erwartet nach der Migration.

* Manchmal als automatisierte Tool-Code in Fehler `#if #else` bedingte Compiler-Direktiven. In diesem Fall müssen Sie die Updates manuell vornehmen (siehe Abschnitt zu häufigen Fehlern unten).

* Manuell die exportierten Methoden mit `[Export]` möglicherweise nicht automatisch behoben werden vom Migrationstool, z. B. in diesem Code Snippert müssen Sie manuell aktualisieren, den Rückgabetyp zu `nfloat`:

    ```csharp
    [Export("tableView:heightForRowAtIndexPath:")]
    public nfloat HeightForRow(UITableView tableView, NSIndexPath indexPath)
    ```

 * Die Unified API bietet keine implizite Konvertierung zwischen NSDate und .NET DateTime-Wert, da es sich nicht um eine Verlustlose Konvertierung ist. Um zu verhindern, dass Fehler im Zusammenhang mit `DateTimeKind.Unspecified` Konvertieren von .NET `DateTime` an lokale oder UTC vor der Konvertierung zu `NSDate`.

 * Objective-C-Kategorie-Methoden sind jetzt als Erweiterungsmethoden in der Unified API generiert. Beispielsweise code, der zuvor verwendet `UIView.DrawString` jetzt verweisen würde `NSString.DrawString` in der Unified API.

 * Code mit AVFoundation Klassen mit `VideoSettings` sollten ändern, um verwenden die `WeakVideoSettings` Eigenschaft. Dies erfordert eine `Dictionary`, steht als Eigenschaft für die für Einstellungenklassen, z.B.:

    ```csharp
    vidrec.WeakVideoSettings = new AVVideoSettings() { ... }.Dictionary;
    ```

 * Die NSObject `.ctor(IntPtr)` Konstruktor wurde geändert von öffentlichen, geschützten ([auf die falsche Verwendung zu verhindern, dass](~/cross-platform/macios/unified/overview.md#NSObject_ctor)).

 * `NSAction` wurde [ersetzt](~/cross-platform/macios/unified/overview.md#NSAction) mit der Starndard .NET `Action`. Wurden einige einfache (einzelne Parameter) Delegaten auch durch ersetzt `Action<T>`.

Schließlich finden Sie unter den [Klassisch V Unified API-Unterschiede](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) , um Änderungen an APIs in Ihrem Code zu suchen. Suche [auf dieser Seite](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) hilft finden Sie klassischen APIs und was sie auf aktualisiert haben.

**Hinweis:** der `MonoTouch.Dialog` Namespace bleibt nach der Migration. Wenn Ihr Code verwendet **MonoTouch.Dialog** sollten Sie weiterhin nach diesen Namespace – übernehmen *nicht* ändern `MonoTouch.Dialog` zu `Dialog`!

## <a name="common-compiler-errors"></a>Häufige Compilerfehler

Weitere Beispiele für häufige Fehler sind nachfolgend, zusammen mit der Lösung aufgeführt:

**Fehler CS0012-Fehler: Der Typ 'MonoTouch.UIKit.UIView' ist in einer Assembly definiert, die nicht verwiesen wird.**

Fix: Dies bedeutet normalerweise, dass das Projekt verweist auf eine Komponente oder ein NuGet-Paket, das nicht mit der Unified API erstellt wurde. Sollten Sie löschen und erneut hinzufügen, alle Komponenten und NuGet Pakete. Wenn dies nicht den Fehler behoben wird, kann die externe Bibliothek noch nicht der Unified API unterstützt.

**Fehler-MT0034: Kann nicht sowohl "monotouch.dll" und "Xamarin.iOS.dll" in der gleichen Xamarin.iOS-Projekt - sind explizit 'Xamarin.iOS.dll' verwiesen wird, solange "monotouch.dll" verwiesen wird "Xamarin.Mobile, Version 0.6.3.0, Culture = Neutral, PublicKeyToken = = Null".**

Fix: Löschen Sie die Komponente, die die Ursache dieses Fehlers ist ein, und wieder zum Projekt hinzu.

**Fehler-CS0234: Der Typ oder Namespace-Name "Foundation" im Namespace 'MonoTouch' ist nicht vorhanden. Fehlt einen Assemblyverweis?**

Fix: Die automatisierte Migrationstool in Visual Studio für Mac *sollten* aktualisieren Sie alle `MonoTouch.Foundation` Verweise auf `Foundation`, aber in einigen Fällen müssen diese manuell aktualisiert werden. Ähnliche Fehler können angezeigt werden, für die anderen Namespaces, die zuvor in enthaltenen `MonoTouch`, z. B. `UIKit`.

**Fehler-CS0266: Typ "double", 'System.float' kann nicht implizit konvertiert werden.**

Fix: ändern und die Umwandlung in `nfloat`. Dieser Fehler kann auch auftreten, für andere Typen mit 64-Bit-Entsprechungen (z. B. `nint`,)

```csharp
nfloat scale = (nfloat)Math.Min(rect.Width, rect.Height);
```

**Fehler-CS0266: Typ "CoreGraphics.CGRect',"System.Drawing.RectangleF"nicht implizit konvertiert werden. Eine explizite Konvertierung vorhanden (fehlt eine Umwandlung?)**

Fix: Ändern von Instanzen, um `RectangleF` zu `CGRect`, `SizeF` zu `CGSize`, und `PointF` zu `CGPoint`. Der Namespace `using System.Drawing;` sollte ersetzt werden durch `using CoreGraphics;` (sofern er nicht bereits vorhanden ist).

**Fehler CS1502: The best overloaded method match for 'CoreGraphics.CGContext.SetLineDash(System.nfloat, System.nfloat[])' has some invalid arguments**

Fix: Ändern der Arraytyp `nfloat[]` und explizit umwandeln `Math.PI`.

```csharp
grphc.SetLineDash (0, new nfloat[] { 0, 3 * (nfloat)Math.PI });
```

**Fehler CS0115: 'WordsTableSource.RowsInSection (UIKit.UITableView, Int)' ist als eine Außerkraftsetzung, aber keine passende Methode gefunden, die außer Kraft setzen gekennzeichnet.**

Fix: Ändern Sie die Rückgabetypen für Wert und Parameter zum `nint`. Dieser häufig Fehler tritt in methodenüberschreibungen z. B. auf `UITableViewSource`, einschließlich `RowsInSection`, `NumberOfSections`, `GetHeightForRow`, `TitleForHeader`, `GetViewForHeader`usw.

```csharp
public override nint RowsInSection (UITableView tableview, nint section) {
```

**Fehler-CS0508: `WordsTableSource.NumberOfSections(UIKit.UITableView)': return type must be 'System.nint' to match overridden member `UIKit.UITableViewSource.NumberOfSections(UIKit.UITableView)'**

Fix: Wenn der Rückgabetyp geändert wird, um `nint`, wandeln Sie den Rückgabewert in `nint`.

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    return (nint)navItems.Count;
}
```

**Error CS1061: Typ "CoreGraphics.CGPath" enthält keine Definition für "AddElipseInRect"**

Fix: Korrigieren Sie die Schreibweise in `AddEllipseInRect`. Andere Namensänderungen umfassen Folgendes:

* Ändern Sie in "Color.Black" `NSColor.Black`.
* Ändern Sie in MapKit "AddAnnotation" `AddAnnotations`.
* Ändern Sie AVFoundation "DataUsingEncoding" in `Encode`.
* Ändern Sie AVFoundation "AVMetadataObject.TypeQRCode" in `AVMetadataObjectType.QRCode`.
* Ändern Sie AVFoundation "VideoSettings" in `WeakVideoSettings`.
* Ändern PopViewControllerAnimated zu `PopViewController`.
* Ändern Sie CoreGraphics "CGBitmapContext.SetRGBFillColor" in `SetFillColor`.

**Fehler CS0546: Überschreiben nicht möglich; "MapKit.MKAnnotation.Coordinate" keinen überschreibbaren Set-Accessor (CS0546)**

Wenn Sie eine benutzerdefinierte Anmerkung zu erstellen, durch Ableitung von Unterklassen MKAnnotation hat das Feld "Koordinate" keine Set-Methode, nur ein Getter.

[Beheben Sie](https://forums.xamarin.com/discussion/comment/109505/#Comment_109505):

* Hinzufügen eines Felds zum Nachverfolgen der Koordinate.
* Dieses Feld in den Getter der Eigenschaft-Koordinate zurückgeben
* Überschreiben Sie die SetCoordinate-Methode, und legen Sie das Feld
* Rufen Sie in Ihrer "ctor" mit der übergebenen Koordinatenparameter SetCoordinate

Der Inhalt sollte Folgendem ähnlich sehen:

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
- [Aktualisieren von Mac-Apps](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Aktualisieren von Xamarin.Forms-Apps](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Aktualisieren von Bindungen](~/cross-platform/macios/unified/update-binding.md)
- [Arbeiten mit nativen Typen in plattformübergreifenden Apps](~/cross-platform/macios/native-types-cross-platform.md)
- [Klassischen Vs Unified API-Unterschiede](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
