---
title: Übersicht über Unified API
description: Xamarin Unified-API können sie zum Freigeben von Code zwischen Mac und iOS und Unterstützung der 32- und 64-Bit-Anwendungen mit der gleichen Binärdatei.
ms.prod: xamarin
ms.assetid: 5F0CEC18-5EF6-4A99-9DCF-1A3B57EA157C
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 1d159d280bd3b8855c32e3e437dfdefcbe0463cb
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61261128"
---
# <a name="unified-api-overview"></a>Übersicht über Unified API

Xamarin Unified-API können sie zum Freigeben von Code zwischen Mac und iOS und Unterstützung der 32- und 64-Bit-Anwendungen mit der gleichen Binärdatei. Die Unified API ist standardmäßig in neuen Xamarin.iOS- und Xamarin.Mac-Projekten verwendet.

> [!IMPORTANT]
> Die Xamarin klassische-API, die der Unified API vorangestellt, wurde als veraltet markiert. 
> - Die letzte Version von Xamarin.iOS Unterstützung der klassischen-API (monotouch.dll) war Xamarin.iOS 9.10.
> - Xamarin.Mac unterstützt weiterhin die klassische-API, aber es ist nicht mehr aktualisiert. Da sie veraltet ist, sollten Entwickler ihre Anwendungen für die Unified API verschieben.

## <a name="updating-classic-api-based-apps"></a>Aktualisieren der klassischen API-basierte Apps

Befolgen Sie die entsprechenden Anweisungen für Ihre Plattform aus:

- [Aktualisieren von vorhandenen Apps](updating-apps.md)
- [Aktualisieren von vorhandenen iOS-Apps](updating-ios-apps.md)
- [Aktualisieren von vorhandenen Mac-Apps](updating-mac-apps.md)
- [Aktualisieren von vorhandenen Xamarin.Forms-Apps](updating-xamarin-forms-apps.md)
- [Migrieren einer Bindung zu Unified API](update-binding.md)

## <a name="tips-for-updating-code-to-the-unified-apiupdating-tipsmd"></a>[Tipps zum Aktualisieren von Code für Unified API](updating-tips.md)

Unabhängig davon, welche Anwendungen Sie migrieren, sehen Sie sich [diese Tipps](updating-tips.md) können Sie mit der Unified API erfolgreich aktualisiert.

## <a name="library-split"></a>Bibliothek Teilen

Ab diesem Zeitpunkt wird unsere APIs auf zwei Arten angezeigt werden:

-  **Klassische-API:** Beschränkt auf 32-Bit (nur) und verfügbar gemacht werden, der `monotouch.dll` und `XamMac.dll` Assemblys.
-  **Einheitliche API:** Unterstützt 32- und 64-Bit-Entwicklung mit einer einzelnen API zur Verfügung, in der `Xamarin.iOS.dll` und `Xamarin.Mac.dll` Assemblys.

Dies bedeutet, dass für Enterprise-Entwickler (nicht abzielt den App Store), können Sie weiterhin verwenden die vorhandenen klassischen APIs, wie wir sie unbegrenzt, oder Sie Verwaltung beibehalten wird auf die neuen APIs aktualisieren können.

<a name="namespace-changes" />

## <a name="namespace-changes"></a>Namespace-Änderungen

Um die Reibung zum Teilen von Code zwischen Produkten Mac- und iOS zu verringern, ändern wir die Namespaces für die APIs in den Produkten.

Wir werden das Präfix "MonoTouch" von unserer iOS-Produkt- und "MonoMac" aus unserer Mac-Produkte für die Datentypen löschen.

Dies erleichtert das Freigeben von Code zwischen den Mac- und iOS-Plattformen ohne zurückzugreifen, für die bedingte Kompilierung und reduziert die Komplexität oben auf der Ihre Quellcodedateien.

-  **Klassische-API:** Verwenden von Namespaces `MonoTouch.` oder `MonoMac.` Präfix.
-  **Einheitliche API:** Kein Namespace-Präfix

## <a name="runtime-defaults"></a>Runtime-Standardeinstellungen

Die Unified API verwendet standardmäßig die **SGen** Garbage Collector und [neue Verweiszählung](~/ios/internals/newrefcount.md) System zum Nachverfolgen des Besitzes von Objekten. Mit diesem Feature wurde zu Xamarin.Mac portiert.

Dies behebt eine Reihe von Problemen, die Entwickler mit dem alten System konfrontiert, und vereinfacht [Speicherverwaltung](~/cross-platform/deploy-test/memory-perf-best-practices.md).

Beachten Sie, dass es möglich ist, neuer Refcount auch für die klassische-API zu aktivieren, aber die Standardeinstellung ist konservativ und erfordert keine Benutzer keine Änderungen vornehmen. Mit der Unified API, wir haben die Möglichkeit, ändern Sie die Standardeinstellung, und erhalten Entwickler alle Verbesserungen zur gleichen Zeit, dass sie Umgestalten und ihres Codes testen.

## <a name="api-changes"></a>API-Änderungen

Der Unified API entfernt, als veraltet markierten Methoden und es gibt einige Fälle, bei der es Tippfehler in der API-Namen, wenn sie auf den ursprünglichen MonoTouch und MonoMac-Namespace in den klassischen APIs gebunden wurden. Diese Instanzen in der neuen Unified-APIs korrigiert wurden und in Ihre Komponente, IOS- und Macintosh-Anwendungen aktualisiert werden müssen. Hier ist eine Liste der am häufigsten verwendeten Seiten, die auftreten können:

|Name der klassischen API-Methode|Name der einheitliche API-Methode|
|--- |--- |
|`UINavigationController.PushViewControllerAnimated()`|`UINavigationController.PushViewController()`|
|`UINavigationController.PopViewControllerAnimated()`|`UINavigationController.PopViewController()`|
|`CGContext.SetRGBFillColor()`|`CGContext.SetFillColor()`|
|`NetworkReachability.SetCallback()`|`NetworkReachability.SetNotification()`|
|`CGContext.SetShadowWithColor`|`CGContext.SetShadow`|
|`UIView.StringSize`|`UIKit.UIStringDrawing.StringSize`|

Eine vollständige Liste der Änderungen beim Wechsel vom klassischen Bereitstellungsmodell der Unified API, finden Sie in unserem [klassisch (monotouch.dll) Vs Unified (Xamarin.iOS.dll) API-Unterschiede](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) Dokumentation.

## <a name="updating-to-unified"></a>Aktualisieren auf einheitliche

Einige alte/unterteilt/als veraltet markierte-API in **klassischen** sind nicht verfügbar, in der **Unified** API. Es kann einfacher sein, beheben Sie die `CS0616` Warnungen vor dem Starten Ihrer (manuell oder automatisch) zu aktualisieren, da Sie müssen die `[Obsolete]` Attribut Nachricht (Teil der Warnung), die Sie an die richtige API führen.

Beachten Sie, die wir veröffentlichen eine [ *Diff* ](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) im klassischen VS unified API-Änderungen, die entweder vor oder nach Ihrer projektaktualisierungen verwendet werden können. Immer noch behoben. die oft veraltete Aufrufe im klassischen Bereitstellungsmodell wird werden eine Zeitersparnis (weniger Suchen nach Dokumentation).

Führen Sie diese Anweisungen, um [Aktualisieren von vorhandenen iOS-apps](~/cross-platform/macios/unified/updating-ios-apps.md), oder [Mac-apps](~/cross-platform/macios/unified/updating-mac-apps.md) der Unified API.
Überprüfen Sie die restlichen auf dieser Seite und [diese Tipps](~/cross-platform/macios/unified/updating-tips.md) zusätzliche Informationen zum Migrieren von Code.

### <a name="nuget"></a>NuGet

NuGet-Pakete, die bisher Xamarin.iOS über die klassische-API unterstützt veröffentlicht ihre Assemblys mit der **Monotouch10** Plattform-Moniker.

Die Unified-API führt eine neue Plattform-ID für kompatiblen Paketen - **Xamarin.iOS10**. Vorhandene NuGet-Pakete zum Hinzufügen der Unterstützung für diese Plattform, mit der Erstellung mit der Unified API aktualisiert werden müssen.

> [!IMPORTANT]
> Wenn Sie einen Fehler in der Form haben _"Fehler 3 kann nicht sowohl"monotouch.dll"und"Xamarin.iOS.dll"im gleichen Xamarin.iOS-Projekt enthalten – explizit 'Xamarin.iOS.dll' verwiesen wird, solange"monotouch.dll"verwiesen wird" Xxx "," Version = 0.0.000, Kultur = Neutral, PublicKeyToken = Null'"_ nach der Konvertierung Ihrer Anwendung auf die Unified-APIs, es liegt in der Regel müssen entweder eine Komponente oder ein NuGet-Paket im Projekt, das nicht der Unified API aktualisiert wurde. Sie müssen zum Entfernen des vorhandenen Komponente/NuGet-Pakets, ein update auf eine Version, die Unified-APIs unterstützt, und führen Sie einen bereinigten Build.

### <a name="the-road-to-64-bits"></a>Der Weg zur 64-Bit

Hintergrundinformationen zur Unterstützung von 32 und 64 Bit-Anwendungen und Informationen zu Frameworks finden Sie unter den [32 und 64-bit-Aspekte zur Geräteplattform](~/cross-platform/macios/32-and-64/index.md).

 <a name="new-data-types" />

#### <a name="new-data-types"></a>Neue Datentypen

Verwenden Sie den Kern der Differenz Mac und iOS-APIs architekturspezifische Datentypen, die immer auf 32-Bit auf 32-Bit-Plattformen und 64-Bit auf 64-Bit-Plattformen sind.

Objective-C-beispielsweise ordnet die `NSInteger` Datentyp, `int32_t` auf 32-Bit-Systemen und zu `int64_t` auf 64-Bit-Systemen.

Ersetzen wir dieses Verhaltens auf unsere Unified API entsprechend der vorherigen Verwendung `int` (die in .NET wird als definiert immer `System.Int32`) in einen neuen Datentyp: `System.nint`.  Sie können das "n" wie "native" vorstellen, damit die systemeigene ganze Zahl eingeben, der Plattform.

Wir führen `nint`, `nuint` und `nfloat` als auch bereitstellen Datentypen baut auf den sie bei Bedarf.

Weitere Informationen zu diesen Änderungen des Datentyps, finden Sie unter den [systemeigene Typen](~/cross-platform/macios/nativetypes.md) Dokument.

### <a name="how-to-detect-the-architecture-of-ios-apps"></a>Gewusst wie: erkennen die Architektur von iOS-apps

Es gibt möglicherweise Situationen, in denen Ihre Anwendung benötigt, zu wissen, ob es auf einem 32-Bit oder eine 64-Bit-iOS-System ausgeführt wird. Der folgende Code dient zum Überprüfen der Architektur:

```csharp
if (IntPtr.Size == 4) {
    Console.WriteLine ("32-bit App");
} else if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit App");
}
```

<a name="deprecated-apis" />

### <a name="arrays-and-systemcollectionsgeneric"></a>Arrays und System.Collections.Generic

Da C# Indexer erwarten, dass ein `int`, müssen Sie explizit umwandeln `nint` Werte `int` Zugriff auf die Elemente in einer Auflistung oder ein Array. Zum Beispiel:

```csharp
public List<string> Names = new List<string>();
...

public string GetName(nint index) {
    return Names[(int)index];
}

```

Dieses Verhalten wird erwartet, da die Umwandlung von `int` zu `nint` ist auf 64-Bit-Systemen verlustbehaftete, eine implizite Konvertierung erfolgt nicht.

### <a name="converting-datetime-to-nsdate"></a>Konvertieren von DateTime in NSDate

Bei Verwendung der Unified-APIs, die implizite Konvertierung von `DateTime` zu `NSDate` Werte nicht mehr ausgeführt. Diese Werte müssen explizit von einem Typ in einen anderen konvertiert werden. Die folgenden Erweiterungsmethoden können verwendet werden, um diesen Prozess zu automatisieren:

```csharp
public static DateTime NSDateToDateTime(this NSDate date)
{
    // NSDate has a wider range than DateTime, so clip
    // the converted date to DateTime.Min|MaxValue.
    double secs = date.SecondsSinceReferenceDate;
    if (secs < -63113904000)
        return DateTime.MinValue;
    if (secs > 252423993599)
        return DateTime.MaxValue;
    return (DateTime) date;
}

public static NSDate DateTimeToNSDate(this DateTime date)
{
    if (date.Kind == DateTimeKind.Unspecified)
        date = DateTime.SpecifyKind (date, /* DateTimeKind.Local or DateTimeKind.Utc, this depends on each app */)
    return (NSDate) date;
}

```

<a name="deprecated-typos" />

### <a name="deprecated-apis-and-typos"></a>Veraltete APIs und Tippfehler

In klassischen Xamarin.iOS-API (monotouch.dll) die `[Obsolete]` -Attribut wurde auf zwei unterschiedliche Arten verwendet:

-  **Veraltete iOS-API:** Dies ist bei der Apple-Hinweise, Sie beenden, mithilfe einer API, da es durch eine neuere Version ersetzt wird. Der klassischen API ist immer noch in Ordnung und oft erforderlich (Wenn Sie die ältere Version von iOS unterstützen).
 Diese API (und die `[Obsolete]` Attribut), die in den neuen Xamarin.iOS-Assemblys enthalten sind.
-  **Falsche API** einige API musste Tippfehler, auf deren Namen.

Für die ursprüngliche Assemblys (monotouch.dll und XamMac.dll), die wir beibehalten des alten Codes für die Kompatibilität zur Verfügung, aber sie wurden entfernt von den Unified API-Assemblys (Xamarin.iOS.dll und Xamarin.Mac)

<a name="NSObject_ctor" />

### <a name="nsobject-subclasses-ctorintptr"></a>NSObject Unterklassen .ctor(IntPtr)

Jede `NSObject` Unterklasse verfügt über einen Konstruktor, akzeptiert eine `IntPtr`. Dies ist wie eine neue verwaltete Instanz aus einem systemeigenen ObjC-Handle instanziiert werden können.

Im klassischen Modell war ein `public` Konstruktor. Aber es einfach zu missbrauchen dieses Feature im Benutzercode war, z. B. das Erstellen mehrerer Instanzen für eine einzelne ObjC-Instanz verwaltet *oder* wie eine verwaltete Instanz erstellen, die den erwarteten verwalteten Zustand (für Unterklassen) fehlen würde.

Um diese Art von Problemen zu vermeiden. die `IntPtr` Konstruktoren sind jetzt `protected` in **einheitliche** -API, um nur für Unterklassen verwendet werden. Dadurch wird sichergestellt, dass die korrigieren/sicher API verwendet wird, um die Erstellung der verwalteten Instanz von Handles, d. h.

    var label = Runtime.GetNSObject<UILabel> (handle);

Diese API gibt eine vorhandene verwaltete Instanz (falls sie bereits vorhanden ist) oder erstellt eine neue Transaktion (wenn erforderlich). Es ist bereits in der klassischen und einheitliche-API verfügbar.

Beachten Sie, dass die `.ctor(NSObjectFlag)` ist jetzt auch `protected` , aber diese außerhalb von Unterklassen nur selten verwendet.

<a name="NSAction" />

### <a name="nsaction-replaced-with-action"></a>Mit der Aktion ersetzt NSAction

Mit dem Unified-APIs `NSAction` wurde zugunsten von .NET standard entfernt `Action`. Dies ist ein großer Vorteil, da `Action` ist ein allgemeine .NET-Typ, während `NSAction` wurde speziell für Xamarin.iOS. Beide führen dasselbe aus, sie waren jedoch unterschiedlichen und inkompatiblen Typen und führte dazu, dass weiterer Code geschrieben werden, um das gleiche Ergebnis erzielen müssen.

Angenommen, Ihre vorhandene Xamarin-Anwendung mit den folgenden Code enthalten:

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (new NSAction (delegate() {
    ShowDropDownAnimated (tblDataView);
}));
```
Sie können jetzt mit einem einfachen Lambda-Ausdruck ersetzt werden:

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (() => ShowDropDownAnimated(tblDataView));
```

Zuvor wäre, die einen Compilerfehler, da ein `Action` kann nicht zugewiesen werden `NSAction`, da `UITapGestureRecognizer` akzeptiert jetzt einen `Action` anstelle von einer `NSAction` gilt in den Unified-APIs.

### <a name="custom-delegates-replaced-with-actiont"></a>Benutzerdefinierte Delegaten mit der Aktion ersetzt<T>

In **einheitliche** einige einfache (z. B. ein Parameter) .net Delegaten wurden durch ersetzt `Action<T>`. Beispiel:

    public delegate void NSNotificationHandler (NSNotification notification);

kann nun verwendet werden, als ein `Action<NSNotification>`. Dieser höher stufen-Code wiederverwenden und weniger Code dupliziert in Xamarin.iOS- und Ihren eigenen Anwendungen.

### <a name="taskbool-replaced-with-taskbooleannserror"></a>Aufgabe<bool> durch Task < boolescher Wert, NSError >> ersetzt

In **klassischen** gab es einige asynchrone APIs zurückgeben `Task<bool>`. Jedoch einige von ihnen, in denen werden soll, wenn ein `NSError` Teil der Signatur, d. h. wurde die `bool` wurde bereits `true` und Sie erhalten eine Ausnahme abfangen musste die `NSError`.

Da einige Fehler sehr verbreitet sind, und der Rückgabewert nicht hilfreich war war dieses Muster in geändert **einheitliche** zurückzugebenden eine `Task<Tuple<Boolean,NSError>>`. Dadurch können Sie überprüfen, die Erfolgs- und alle Fehler, die während der asynchrone Aufruf aufgetreten sind, konnte.

### <a name="nsstring-vs-string"></a>NSString Vs-Zeichenfolge

In einigen Fällen einiger Konstanten aus geändert werden mussten, `string` zu `NSString`, z. B. `UITableViewCell`

**Klassisch**

    public virtual string ReuseIdentifier { get; }

**Unified**

    public virtual NSString ReuseIdentifier { get; }

Im Allgemeinen wird es bevorzugt .NET `System.String` Typ. Jedoch trotz der Apple-Richtlinien, eine systemeigene API Konstanter Zeiger (nicht die Zeichenfolge selbst) vergleichen, und diese kann nur verwendet, wenn wir die Konstanten als verfügbar machen `NSString`.

 <a name="protocols" />

### <a name="objective-c-protocols"></a>Objective-C-Protokolle

Die ursprüngliche MonoTouch keine vollständige Unterstützung für ObjC-Protokolle und einige oder nicht optimalen haben,-API-wurden hinzugefügt, um das am häufigsten verwendete Szenario zu unterstützen. Diese Einschränkung nicht mehr vorhanden, aber für die Abwärtskompatibilität, mehrere APIs aufbewahrt werden in `monotouch.dll` und `XamMac.dll`.

Diese Einschränkungen wurden entfernt und für die Unified-APIs bereinigt. Die meisten Änderungen werden wie folgt aussehen:

**Klassisch**

    public virtual AVAssetResourceLoaderDelegate Delegate { get; }

**Unified**

    public virtual IAVAssetResourceLoaderDelegate Delegate { get; }

Die `I` bedeutet, dass Präfix **einheitliche** verfügbar zu machen eine-Schnittstelle anstelle eines bestimmten Typs, für das ObjC-Protokoll. Dies vereinfacht Fälle, in denen Sie nicht um eine Unterklasse den spezifischen Typ möchten, den Xamarin.iOS bereitgestellt.

Außerdem konnte eine API, präziser und einfach zu verwenden, z. b.:

**Klassisch**

    public virtual void SelectionDidChange (NSObject uiTextInput);

**Unified**

    public virtual void SelectionDidChange (IUITextInput uiTextInput);

Diese API sind jetzt einfacher für uns, ohne auf die Dokumentation zu verweisen, und die Fertigstellung der IDE-Code erhalten Sie weitere nützliche Empfehlungen basierend auf dem Protokoll bzw.-Schnittstelle.

#### <a name="nscoding-protocol"></a>NSCoding-Protokoll

Die ursprüngliche Bindung enthalten eine .ctor(NSCoder) für jeden Typ - selbst wenn sie nicht unterstützte die `NSCoding` Protokoll.  Ein einzelnes `Encode(NSCoder)` Methode war in der `NSObject` zum Codieren des Objekts.
Aber diese Methode funktioniert nur, wenn die Instanz NSCoding-Protokoll entspricht.

Auf der Unified API haben wir dieses Problem behoben.  Die neuen Assemblys haben nur die `.ctor(NSCoder)` , wenn der Typ entspricht `NSCoding`. Solche Typen auch inzwischen eine `Encode(NSCoder)` -Methode die entspricht der `INSCoding` Schnittstelle.

Nur geringe Auswirkungen: In den meisten Fällen wird nicht diese Änderung sich auf Anwendungen auswirken, wie die alten, entfernten, Konstruktoren nicht verwendet werden konnte.

## <a name="further-tips"></a>Weitere Tipps

Zusätzliche Änderungen berücksichtigen finden Sie in der [Tipps zum Aktualisieren von apps, die Unified API](~/cross-platform/macios/unified/updating-tips.md).

## <a name="sample-code"></a>Beispielcode

Ab dem 31. Juli haben wir Ports des iOS-Beispiele aus, um diese neue API veröffentlicht, auf die `magic-types` am branch [Monotouch-Samples](https://github.com/xamarin/monotouch-samples/commits/magic-types).

Wir überprüfen für Mac und Beispiele sowohl die [Mac-Beispiele](https://github.com/xamarin/mac-samples) Repository (mit neuen APIs in Mavericks/Yosemite) als auch 32/64-Bit-Beispiele in den Magic-Types-Branch [Mac-Beispiele](https://github.com/xamarin/monotouch-samples/commits/magic-types).

## <a name="related-links"></a>Verwandte Links

- [Aktualisieren von iOS-Apps](updating-ios-apps.md)
- [Aktualisieren von Mac-Apps](updating-mac-apps.md)
- [Aktualisieren von Xamarin.Forms-Apps](updating-xamarin-forms-apps.md)
- [Aktualisieren von Bindungen](update-binding.md)
- [Aktualisieren von Tipps](updating-tips.md)
- [Klassischen Vs Unified API-Unterschiede](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
- [Arbeiten mit nativen Typen in plattformübergreifenden Apps](~/cross-platform/macios/native-types-cross-platform.md)
