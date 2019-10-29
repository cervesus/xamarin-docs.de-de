---
title: Tipps zum Aktualisieren von Code für Unified API
description: In diesem Dokument werden häufige Fehler und verschiedene Tipps erläutert, die beim Aktualisieren einer Anwendung für die Verwendung von xamarin-Unified API nützlich sind.
ms.prod: xamarin
ms.assetid: 8DD34D21-342C-48E9-97AA-1B649DD8B61F
ms.date: 03/29/2017
author: davidortinau
ms.author: daortin
ms.openlocfilehash: ee207ffc83f887e9c86c650b6f93fc2ff9f5b69c
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73014997"
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

Und fügen das `DependsOnTargets="_CollectBundleResources"`-Attribut hinzu. Und zwar so:

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

- Das Vergleichen von `enum`s erfordert möglicherweise eine `(int)` Umwandlung.

- `NSDictionary.IntValue` jetzt ein `nint`zurückgibt, gibt es eine `Int32Value`, die stattdessen verwendet werden kann.

- `nfloat`-und `nint` Typen können nicht als `const`gekennzeichnet werden. `static readonly nint` ist eine sinnvolle Alternative.

- Dinge, die direkt im `MonoTouch.`-Namespace verwendet werden, befinden sich jetzt in der Regel im `ObjCRuntime.`-Namespace (z. b. `MonoTouch.Constants.Version` ist jetzt `ObjCRuntime.Constants.Version`).

- Code, der Objekte serialisiert, kann bei dem Versuch, `nint` und `nfloat` Typen zu serialisieren, unterbrechen. Vergewissern Sie sich, dass der Serialisierungscode nach der Migration erwartungsgemäß funktioniert.

- Manchmal missgibt das automatisierte Tool Code in `#if #else` bedingten Compilerdirektiven. In diesem Fall müssen Sie die Korrekturen manuell vornehmen (siehe die folgenden allgemeinen Fehler).

- Manuell exportierte Methoden, die `[Export]` verwenden, werden möglicherweise nicht automatisch vom Migrationstool korrigiert, z. b. in diesem Code-Snippert müssen Sie den Rückgabetyp manuell auf `nfloat`aktualisieren:

  ```csharp
  [Export("tableView:heightForRowAtIndexPath:")]
  public nfloat HeightForRow(UITableView tableView, NSIndexPath indexPath)
  ```

- Der Unified API bietet keine implizite Konvertierung zwischen nsdate und .NET DateTime, da es sich nicht um eine Verlust lose Konvertierung handelt. Um Fehler im Zusammenhang mit `DateTimeKind.Unspecified` zu vermeiden, konvertieren Sie die .net `DateTime` vor der Umwandlung in `NSDate`in lokale oder UTC-.

- Ziel-C-kategoriemethoden werden nun als Erweiterungs Methoden im Unified API generiert. Beispielsweise würde Code, der zuvor `UIView.DrawString` verwendet hat, jetzt im Unified API auf `NSString.DrawString` verweisen.

- Code, der AVFoundation-Klassen mit `VideoSettings` verwendet, sollte so geändert werden, dass die Eigenschaft `WeakVideoSettings` verwendet wird Hierfür ist ein `Dictionary`erforderlich, das als Eigenschaft für die Einstellungs Klassen verfügbar ist, z. b.:

  ```csharp
  vidrec.WeakVideoSettings = new AVVideoSettings() { ... }.Dictionary;
  ```

- Der NSObject-`.ctor(IntPtr)` Konstruktor wurde von "Public" in "Protected" geändert ([um eine unsachgemäße Verwendung zu verhindern](~/cross-platform/macios/unified/overview.md#NSObject_ctor)).

- `NSAction` wurde durch den standardmäßigen .net-`Action`[ersetzt](~/cross-platform/macios/unified/overview.md#NSAction) . Einige einfache Delegaten (einzelner Parameter) wurden ebenfalls durch `Action<T>`ersetzt.

Lesen Sie abschließend die [Unterschiede zu den klassischen v-Unified API](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md) , um Änderungen an APIs in Ihrem Code zu suchen. Durch [das Durchsuchen dieser Seite](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md) können Sie klassische APIs und deren Aktualisierung finden.

> [!NOTE]
> Der `MonoTouch.Dialog`-Namespace bleibt nach der Migration unverändert. Wenn Ihr Code **MonoTouch. Dialog** verwendet, sollten Sie diesen Namespace weiterhin verwenden. ändern Sie `MonoTouch.Dialog` *nicht* in `Dialog`!

## <a name="common-compiler-errors"></a>Häufige Compilerfehler

Weitere Beispiele für häufige Fehler sind im folgenden zusammen mit der Lösung aufgeführt:

**Fehler CS0012: der Typ "MonoTouch. UIKit. UIView" ist in einer Assembly definiert, auf die nicht verwiesen wird.**

Behebung: Dies bedeutet in der Regel, dass das Projekt auf eine Komponente oder ein nuget-Paket verweist, das nicht mit dem Unified API erstellt wurde. Löschen Sie alle Komponenten und nuget-Pakete, und fügen Sie Sie erneut hinzu. Wenn der Fehler hierdurch nicht behoben wird, unterstützt die externe Bibliothek das Unified API möglicherweise noch nicht.

**Fehler MT0034: sowohl ' MonoTouch. dll ' als auch ' xamarin. IOS. dll ' können nicht in dasselbe xamarin. IOS-Projekt eingeschlossen werden. auf ' xamarin. IOS. dll ' wird explizit verwiesen, während ' MonoTouch. dll ' von ' xamarin. Mobile, Version = 0.6.3.0, Culture = neutral ' referenziert wird. PublicKeyToken = null '.**

Behebung: Löschen Sie die Komponente, die diesen Fehler verursacht, und fügen Sie Sie dem Projekt erneut hinzu.

**Fehler CS0234: der Typ-oder Namespace Name ' Foundation ' ist im Namespace ' MonoTouch ' nicht vorhanden. Fehlt ein Assemblyverweis?**

Behebung: das automatisierte Migrationstool in Visual Studio für Mac *sollte* alle `MonoTouch.Foundation` Verweise auf `Foundation`aktualisieren, aber in einigen Fällen müssen diese manuell aktualisiert werden. Ähnliche Fehler werden möglicherweise für die anderen Namespaces angezeigt, die zuvor in `MonoTouch`enthalten waren, z. b. `UIKit`.

**Fehler CS0266: der Typ "Double" kann nicht implizit in "System. float" konvertiert werden.**

Korrektur: Ändern des Typs und umwandeln in `nfloat`. Dieser Fehler kann auch für die anderen Typen mit 64-Bit-äquivalenten auftreten (z. b. `nint`,).

```csharp
nfloat scale = (nfloat)Math.Min(rect.Width, rect.Height);
```

**Fehler CS0266: der Typ "CoreGraphics. CGRect" kann nicht implizit in "System. Drawing. rechglef" konvertiert werden. Eine explizite Konvertierung ist vorhanden (fehlt eine Umwandlung?)**

Behebung: Ändern Sie die Instanzen in `CGRect``RectangleF`, `SizeF` `CGSize`, und `PointF` `CGPoint`. Der Namespace `using System.Drawing;` sollte durch `using CoreGraphics;` ersetzt werden (wenn er nicht bereits vorhanden ist).

**Fehler CS1502: die beste überladene Methoden Übereinstimmung für "CoreGraphics. cgcontext. setlinedash (System. nfloat, System. nfloat [])" weist einige ungültige Argumente auf.**

Korrektur: Ändern Sie den Arraytyp in `nfloat[]`, und wandeln Sie `Math.PI`explizit um.

```csharp
grphc.SetLineDash (0, new nfloat[] { 0, 3 * (nfloat)Math.PI });
```

**Fehler CS0115: ' wordstablesource. rowsinsection (UIKit. uitableview, int) ' ist als außer Kraft Setzung gekennzeichnet, aber es wurde keine passende Methode zum Überschreiben gefunden.**

Behebung: Ändern Sie den Rückgabewert und die Parametertypen in `nint`. Dies tritt häufig bei Methoden Überschreibungen auf, z. b. bei `UITableViewSource`, einschließlich `RowsInSection`, `NumberOfSections`, `GetHeightForRow`, `TitleForHeader`, `GetViewForHeader`usw.

```csharp
public override nint RowsInSection (UITableView tableview, nint section) {
```

**Fehler CS0508: `WordsTableSource.NumberOfSections(UIKit.UITableView)`: der Rückgabetyp muss "System. NINT" sein, um mit dem überschriebenen Member zu vergleichen `UIKit.UITableViewSource.NumberOfSections(UIKit.UITableView)`**

Behebung: Wenn der Rückgabetyp in "`nint`" geändert wird, wandeln Sie den Rückgabewert in `nint`um.

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    return (nint)navItems.Count;
}
```

**Fehler CS1061: der Typ "CoreGraphics. cgpath" enthält keine Definition für "addelipseinrect".**

Behebung: korrigieren Sie die Schreibweise in `AddEllipseInRect`. Weitere Namensänderungen sind:

- Ändern Sie "Color. Black" in "`NSColor.Black`".
- Ändern Sie MapKit "AddAnnotation" in `AddAnnotations`.
- Ändern Sie AVFoundation ' datausingencoding ' in `Encode`.
- Ändern Sie AVFoundation ' AVMetadataObject. typeqrcode ' in `AVMetadataObjectType.QRCode`.
- Ändern Sie AVFoundation "videosettings" in "`WeakVideoSettings`".
- Ändern Sie "popviewcontrolleranimierte" in "`PopViewController`".
- Ändern Sie CoreGraphics "cgbitmapcontext. abtrgbfillcolor" in `SetFillColor`.

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
