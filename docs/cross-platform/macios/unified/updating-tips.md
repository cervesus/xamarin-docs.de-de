---
title: Tipps zum Aktualisieren von Code für Unified API
description: In diesem Dokument werden häufige Fehler und verschiedene Tipps erläutert, die beim Aktualisieren einer Anwendung für die Verwendung von xamarin-Unified API nützlich sind.
ms.prod: xamarin
ms.assetid: 8DD34D21-342C-48E9-97AA-1B649DD8B61F
ms.date: 03/29/2017
author: asb3993
ms.author: amburns
ms.openlocfilehash: 844730d2ace717b951df2d80b2add6d1094fe997
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "70226101"
---
# <a name="tips-for-updating-code-to-the-unified-api"></a>Tipps zum Aktualisieren von Code für Unified API

Beim Aktualisieren älterer xamarin-Projektmappen auf die Unified API können die folgenden Fehler auftreten.

## <a name="nsinvalidargumentexception-could-not-find-storyboard-error"></a>Nsinvalidargumentexception konnte keinen storyboardfehler finden.

Es gibt einen [Fehler](https://bugzilla.xamarin.com/show_bug.cgi?id=25569) in der aktuellen Version von Visual Studio für Mac, die nach der Verwendung des automatisierten Migrationstools auftreten können, um Ihr Projekt in die vereinheitlichten APIs zu konvertieren. Wenn Sie nach dem Update eine Fehlermeldung im Format erhalten, erhalten Sie Folgendes:

```console
Objective-C exception thrown. Name: NSInvalidArgumentException Reason: Could not find a storyboard named 'xxx' in bundle NSBundle...
```

Sie können Folgendes tun, um dieses Problem zu beheben. Suchen Sie nach der folgenden buildzieldatei:

```console
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/2.1/Xamarin.iOS.Common.targets
```

In dieser Datei müssen Sie die folgende Ziel Deklaration finden:

```xml
<Target Name="_CopyContentToBundle"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

Und fügen Sie `DependsOnTargets="_CollectBundleResources"` das-Attribut hinzu. Und zwar so:

```xml
<Target Name="_CopyContentToBundle"
        DependsOnTargets="_CollectBundleResources"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

Speichern Sie die Datei, starten Sie Visual Studio für Mac neu, und führen Sie eine saubere & Erstellung Ihres Projekts aus. Eine Korrektur für dieses Problem sollte in Kürze von xamarin veröffentlicht werden.

## <a name="useful-tips"></a>Hilfreiche Tipps

Nachdem Sie das Migrationstool verwendet haben, erhalten Sie möglicherweise trotzdem einige Compilerfehler, die einen manuellen Eingriff erfordern.
Einige Dinge, die möglicherweise manuell korrigiert werden müssen, sind:

- Das `enum`Vergleichen von s erfordert `(int)` möglicherweise eine Umwandlung.

- `NSDictionary.IntValue`gibt jetzt ein `nint`zurück, `Int32Value` das stattdessen verwendet werden kann.

- `nfloat`die `nint` Typen und können nicht `const`markiert werden. `static readonly nint` ist eine sinnvolle Alternative.

- Dinge, die direkt `MonoTouch.` im-Namespace verwendet werden, befinden sich jetzt in der `ObjCRuntime.` Regel im- `MonoTouch.Constants.Version` Namespace (z `ObjCRuntime.Constants.Version`. b. ist jetzt).

- Code, der-Objekte serialisiert, kann beim Serialisieren `nint` von `nfloat` -und-Typen unterbrechen. Vergewissern Sie sich, dass der Serialisierungscode nach der Migration erwartungsgemäß funktioniert.

- Manchmal verpasst das automatisierte Tool Code in `#if #else` bedingten Compilerdirektiven. In diesem Fall müssen Sie die Korrekturen manuell vornehmen (siehe die folgenden allgemeinen Fehler).

- Manuell exportierte Methoden `[Export]` , die verwenden, werden möglicherweise nicht automatisch vom Migrationstool korrigiert, z. b. in diesem Code-Snippert müssen `nfloat`Sie den Rückgabetyp manuell auf Aktualisieren:

  ```csharp
  [Export("tableView:heightForRowAtIndexPath:")]
  public nfloat HeightForRow(UITableView tableView, NSIndexPath indexPath)
  ```

- Der Unified API bietet keine implizite Konvertierung zwischen nsdate und .NET DateTime, da es sich nicht um eine Verlust lose Konvertierung handelt. Um Fehler zu verhindern, `DateTimeKind.Unspecified` die sich auf `DateTime` das Konvertieren von .net in local oder `NSDate`UTC vor dem umwandeln in beziehen.

- Ziel-C-kategoriemethoden werden nun als Erweiterungs Methoden im Unified API generiert. Beispielsweise würde Code, der zuvor `UIView.DrawString` verwendet wurde, `NSString.DrawString` jetzt in der Unified API auf verweisen.

- Code, der AVFoundation- `VideoSettings` Klassen mit verwendet, sollte `WeakVideoSettings` sich ändern, sodass die-Eigenschaft verwendet Hierfür ist ein `Dictionary`erforderlich, das als Eigenschaft für die Einstellungs Klassen verfügbar ist, z. b.:

  ```csharp
  vidrec.WeakVideoSettings = new AVVideoSettings() { ... }.Dictionary;
  ```

- Der NSObject `.ctor(IntPtr)` -Konstruktor wurde von "Public" in "Protected" geändert (um eine nicht ordnungsgemäße[Verwendung zu verhindern](~/cross-platform/macios/unified/overview.md#NSObject_ctor)).

- `NSAction` wurde durch das standardmäßige `Action`-Element von .NET [ersetzt](~/cross-platform/macios/unified/overview.md#NSAction). Einige einfache Delegaten (einzelner Parameter) wurden auch durch ersetzt `Action<T>`.

Lesen Sie abschließend die [Unterschiede zu den klassischen v-Unified API](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md) , um Änderungen an APIs in Ihrem Code zu suchen. Durch [das Durchsuchen dieser Seite](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md) können Sie klassische APIs und deren Aktualisierung finden.

> [!NOTE]
> Der `MonoTouch.Dialog` Namespace bleibt nach der Migration unverändert. Wenn Ihr Code **MonoTouch. Dialog** verwendet, sollten Sie diesen Namespace weiterhin verwenden. ändern `MonoTouch.Dialog` Sie nicht in `Dialog`!

## <a name="common-compiler-errors"></a>Häufige Compilerfehler

Weitere Beispiele für häufige Fehler sind im folgenden zusammen mit der Lösung aufgeführt:

**Fehler CS0012: Der Typ "MonoTouch. UIKit. UIView" ist in einer Assembly definiert, auf die nicht verwiesen wird.**

Zusetzen Dies bedeutet in der Regel, dass das Projekt auf eine Komponente oder ein nuget-Paket verweist, das nicht mit dem Unified API erstellt wurde. Löschen Sie alle Komponenten und nuget-Pakete, und fügen Sie Sie erneut hinzu. Wenn der Fehler hierdurch nicht behoben wird, unterstützt die externe Bibliothek das Unified API möglicherweise noch nicht.

**Fehler MT0034: "MonoTouch. dll" und "xamarin. IOS. dll" können nicht im gleichen xamarin. IOS-Projekt enthalten sein. auf "xamarin. IOS. dll" wird explizit verwiesen, während "MonoTouch. dll" von "xamarin. Mobile, Version = 0.6.3.0, Culture = neutral, PublicKeyToken = null" referenziert wird**

Zusetzen Löschen Sie die Komponente, von der dieser Fehler verursacht wird, und fügen Sie Sie dem Projekt erneut hinzu.

**Fehler CS0234: Der Typ-oder Namespace Name ' Foundation ' ist im Namespace ' MonoTouch ' nicht vorhanden. Fehlt ein Assemblyverweis?**

Zusetzen Das automatisierte Migrationstool in Visual Studio für Mac *sollte* alle `MonoTouch.Foundation` Verweise auf `Foundation`aktualisieren, aber in einigen Fällen müssen diese manuell aktualisiert werden. Ähnliche Fehler werden möglicherweise für die anderen Namespaces angezeigt, `MonoTouch`die zuvor in `UIKit`enthalten waren, z. b.

**Fehler CS0266: Der Typ "Double" kann nicht implizit in "System. float" konvertiert werden.**

Behebung: Ändern Sie den Typ, `nfloat`und wandeln Sie in um Dieser Fehler kann auch für die anderen Typen mit 64-Bit-äquivalenten auftreten `nint`(z. b.).

```csharp
nfloat scale = (nfloat)Math.Min(rect.Width, rect.Height);
```

**Fehler CS0266: Der Typ "CoreGraphics. CGRect" kann nicht implizit in "System. Drawing. rechglef" konvertiert werden. Eine explizite Konvertierung ist vorhanden (fehlt eine Umwandlung?)**

Zusetzen Ändern Sie Instanzen `RectangleF` in `CGRect`in `SizeF` ,`CGSize`inund `PointF` in .`CGPoint` Der Namespace `using System.Drawing;` sollte `using CoreGraphics;` durch ersetzt werden (wenn er nicht bereits vorhanden ist).

**Fehler CS1502: Die beste überladene Methoden Übereinstimmung für "CoreGraphics. cgcontext. setlinedash (System. nfloat, System. nfloat [])" weist einige ungültige Argumente auf.**

Zusetzen Ändern Sie den Arraytyp `nfloat[]` in `Math.PI`und explizit.

```csharp
grphc.SetLineDash (0, new nfloat[] { 0, 3 * (nfloat)Math.PI });
```

**Fehler CS0115: ' wordstablesource. rowsinsection (UIKit. uitableview, int) ' ist als außer Kraft Setzung gekennzeichnet, aber es wurde keine passende Methode zum Überschreiben gefunden.**

Zusetzen Ändern Sie den Rückgabewert und die Parameter `nint`Typen in. Dies tritt häufig `UITableViewSource`in Methoden Überschreibungen auf, wie z. b. denen, einschließlich `GetViewForHeader` `RowsInSection`, `NumberOfSections`, `GetHeightForRow`, `TitleForHeader`, usw.

```csharp
public override nint RowsInSection (UITableView tableview, nint section) {
```

**Fehler CS0508: `WordsTableSource.NumberOfSections(UIKit.UITableView)`: der Rückgabetyp muss "System. NINT" sein, um mit dem überschriebenen Member zu vergleichen`UIKit.UITableViewSource.NumberOfSections(UIKit.UITableView)`**

Zusetzen Wenn der Rückgabetyp in `nint`geändert wird, wandeln Sie den `nint`Rückgabewert in um.

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    return (nint)navItems.Count;
}
```

**Fehler CS1061: Der Typ "CoreGraphics. cgpath" enthält keine Definition für "addelipseinrect".**

Zusetzen Korrekte Schreibweise `AddEllipseInRect`in. Weitere Namensänderungen sind:

- Ändern Sie "Color. Black" `NSColor.Black`in.
- Ändern Sie MapKit "AddAnnotation" `AddAnnotations`in.
- Ändern Sie AVFoundation ' datausingencoding ' `Encode`in.
- Ändern Sie AVFoundation ' AVMetadataObject. typeqrcode ' `AVMetadataObjectType.QRCode`in.
- Ändern Sie AVFoundation ' videosettings ' `WeakVideoSettings`in.
- Ändern Sie popviewcontrolleranimierte `PopViewController`in.
- Ändern Sie CoreGraphics "cgbitmapcontext. abtrgbfillcolor" `SetFillColor`in.

**Fehler CS0546: Überschreiben nicht möglich, da ' MapKit. mkannotation. Koordinate ' nicht über einen über schreibbaren Set-Accessor (CS0546) verfügt.**

Beim Erstellen einer benutzerdefinierten Anmerkung durch Unterklassen-mkannotation hat das Koordinaten Feld keinen Setter, sondern nur einen Getter.

[Behebung](https://forums.xamarin.com/discussion/comment/109505/#Comment_109505):

- Fügen Sie ein Feld hinzu, um die Koordinate nachzuverfolgen.
- Dieses Feld im Getter der Koordinaten Eigenschaft zurückgeben
- Überschreiben der setkoordinatenmethode und Festlegen des Felds
- Aufrufen von setkoordinate in Ihrem ctor mit dem übergebenen Koordinaten Parameter

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

- [Aktualisieren von apps](~/cross-platform/macios/unified/updating-apps.md)
- [Aktualisieren von IOS-apps](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Aktualisieren von Mac-apps](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Aktualisieren von xamarin. Forms-apps](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Aktualisieren von Bindungen](~/cross-platform/macios/unified/update-binding.md)
- [Arbeiten mit nativen Typen in plattformübergreifenden Apps](~/cross-platform/macios/native-types-cross-platform.md)
- [Unterschiede bei klassischem vs Unified API](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
