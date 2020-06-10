---
title: Übersicht über Unified API
description: Die Unified API von xamarin ermöglicht die gemeinsame Nutzung von Code zwischen Mac und IOS sowie die Unterstützung von 32-und 64-Bit-Anwendungen mit derselben Binärdatei.
ms.prod: xamarin
ms.assetid: 5F0CEC18-5EF6-4A99-9DCF-1A3B57EA157C
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: e683170b048be5ab5cc39fa8560c82916ead5d50
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84570933"
---
# <a name="unified-api-overview"></a>Übersicht über Unified API

Die Unified API von xamarin ermöglicht die gemeinsame Nutzung von Code zwischen Mac und IOS sowie die Unterstützung von 32-und 64-Bit-Anwendungen mit derselben Binärdatei. Der Unified API wird standardmäßig in neuen xamarin. IOS-und xamarin. Mac-Projekten verwendet.

> [!IMPORTANT]
> Das xamarin-Classic API, das dem Unified API vorangestellt ist, ist veraltet.
>
> - Die letzte Version von xamarin. IOS zur Unterstützung der Classic API ("MonoTouch. dll") war xamarin. IOS 9,10.
> - Xamarin. Mac unterstützt zwar weiterhin die Classic API, wird jedoch nicht mehr aktualisiert. Da sie veraltet ist, sollten Entwickler ihre Anwendungen in die Unified API verschieben.

## <a name="updating-classic-api-based-apps"></a>Aktualisieren von Classic API basierten apps

Befolgen Sie die entsprechenden Anweisungen für Ihre Plattform:

- [Aktualisieren von vorhandenen Apps](updating-apps.md)
- [Aktualisieren von vorhandenen iOS-Apps](updating-ios-apps.md)
- [Aktualisieren von vorhandenen Mac-Apps](updating-mac-apps.md)
- [Aktualisieren von vorhandenen Xamarin.Forms-Apps](updating-xamarin-forms-apps.md)
- [Migrieren einer Bindung zu Unified API](update-binding.md)

## <a name="tips-for-updating-code-to-the-unified-api"></a>[Tipps zum Aktualisieren von Code für Unified API](updating-tips.md)

Unabhängig davon, welche Anwendungen migriert werden, sehen Sie sich [diese Tipps](updating-tips.md) an, um Sie bei der erfolgreichen Aktualisierung des Unified API zu unterstützen.

## <a name="library-split"></a>Bibliotheks Teilung

Ab diesem Zeitpunkt werden unsere APIs auf zwei Arten angezeigt:

- **Classic API:** Beschränkt auf 32 Bits (nur) und in den Assemblys und verfügbar gemacht `monotouch.dll` `XamMac.dll` .
- **Unified API:** Unterstützung der 32-und 64-Bit-Entwicklung mit einer einzelnen API, die in den Assemblys `Xamarin.iOS.dll` und verfügbar `Xamarin.Mac.dll`

Dies bedeutet, dass Sie für Entwickler von Unternehmen (nicht für den App Store) die vorhandenen klassischen APIs weiterhin verwenden können, da wir Sie dauerhaft beibehalten, oder Sie können ein Upgrade auf die neuen APIs durchführen.

<a name="namespace-changes"></a>

## <a name="namespace-changes"></a>Namespace Änderungen

Um die reibungslose Freigabe von Code zwischen den Mac-und IOS-Produkten zu reduzieren, ändern wir die Namespaces für die APIs in den Produkten.

Wir löschen das Präfix "MonoTouch" aus unserem IOS-Produkt und "monomac" aus dem Mac-Produkt für die Datentypen.

Dies vereinfacht das Freigeben von Code zwischen den Mac-und IOS-Plattformen, ohne dass auf die bedingte Kompilierung zurückgegriffen wird. Dadurch wird das Rauschen am Anfang der Quell Code Dateien reduziert.

- **Classic API:** Namespaces verwenden `MonoTouch.` oder `MonoMac.` Präfix.
- **Unified API:** Kein Namespace Präfix

## <a name="runtime-defaults"></a>Laufzeit-Standardwerte

Der Unified API verwendet standardmäßig die **Sgen** -Garbage Collector und das [neue Verweis zählungs](~/ios/internals/newrefcount.md) System zum Nachverfolgen des Objekt Besitzes. Diese Funktion wurde in xamarin. Mac portiert.

Dadurch wird eine Reihe von Problemen gelöst, die Entwickler mit dem alten System konfrontiert haben, und auch die [Speicherverwaltung](~/cross-platform/deploy-test/memory-perf-best-practices.md)vereinfachen.

Beachten Sie, dass es möglich ist, den neuen refcount auch für den Classic API zu aktivieren. der Standardwert ist jedoch konservativ und erfordert nicht, dass Benutzer Änderungen vornehmen. Mit dem Unified API haben wir die Möglichkeit, die Standardeinstellung zu ändern und Entwicklern alle Verbesserungen zur Verfügung zu stellen, in denen Sie Ihren Code umgestalten und erneut testen.

## <a name="api-changes"></a>API-Änderungen

Der Unified API entfernt veraltete Methoden, und es gibt einige Instanzen, bei denen Tippfehler in den API-Namen vorhanden waren, als Sie an die ursprünglichen MonoTouch-und monomac-Namespaces in den klassischen APIs gebunden wurden. Diese Instanzen wurden in den neuen vereinheitlichten APIs korrigiert und müssen in ihren Komponenten, IOS-und Mac-Anwendungen aktualisiert werden. Im folgenden finden Sie eine Liste der gängigsten, die Sie möglicherweise ausführen:

|Name der Classic API Methode|Name der Unified API Methode|
|--- |--- |
|`UINavigationController.PushViewControllerAnimated()`|`UINavigationController.PushViewController()`|
|`UINavigationController.PopViewControllerAnimated()`|`UINavigationController.PopViewController()`|
|`CGContext.SetRGBFillColor()`|`CGContext.SetFillColor()`|
|`NetworkReachability.SetCallback()`|`NetworkReachability.SetNotification()`|
|`CGContext.SetShadowWithColor`|`CGContext.SetShadow`|
|`UIView.StringSize`|`UIKit.UIStringDrawing.StringSize`|

Eine vollständige Liste der Änderungen beim Wechsel von der klassischen zum Unified API finden Sie in unserer [klassischen Dokumentation (monoouch. dll) vs Unified (xamarin. IOS. dll) API-Unterschiede](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md) .

## <a name="updating-to-unified"></a>Aktualisieren auf Unified

Einige alte/unterbrochene/veraltete **APIs sind in** der **vereinheitlichten** API nicht verfügbar. Es kann einfacher sein, die Warnungen zu beheben, `CS0616` bevor Sie mit der (manuellen oder automatisierten) Aktualisierung beginnen, da Sie über die `[Obsolete]` Attribut Meldung (Teil der Warnung) zur richtigen API gelangen.

Beachten Sie, dass wir einen [*diff*](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md) der klassischen vs Unified API-Änderungen veröffentlichen, die entweder vor oder nach dem Projekt aktualisiert werden können. Immer noch eine Korrektur der veralteten Aufrufe in klassischem, ist oft ein Zeitersparnis (weniger Dokumentations Suchvorgänge).

Befolgen Sie diese Anweisungen, um [vorhandene IOS-apps](~/cross-platform/macios/unified/updating-ios-apps.md)oder [Mac-apps](~/cross-platform/macios/unified/updating-mac-apps.md) auf die Unified API zu aktualisieren.
Lesen Sie den Rest dieser Seite und [diese Tipps](~/cross-platform/macios/unified/updating-tips.md) , um weitere Informationen zum Migrieren von Code zu erhalten.

### <a name="nuget"></a>NuGet

Nuget-Pakete, die xamarin. IOS zuvor über das Classic API unterstützte, veröffentlichten Assemblys mithilfe des **Monotouch10** -Platt Form Monikers.

Der Unified API führt einen neuen Platt Form Bezeichner für kompatible Pakete ein ( **xamarin. iOS10**). Vorhandene nuget-Pakete müssen aktualisiert werden, um Unterstützung für diese Plattform hinzuzufügen, indem Sie für die Unified API erstellt werden.

> [!IMPORTANT]
> Wenn Sie einen Fehler im _Format "Fehler 3 können nicht sowohl ' MonoTouch. dll ' als auch ' xamarin. IOS. dll ' in dasselbe xamarin. IOS-Projekt einschließen (xamarin). auf" IOS. dll "wird explizit verwiesen, während" MonoTouch. dll "von" xxx, Version = 0.0.000, Culture = neutral, PublicKeyToken = null ""_ nach der Umstellung der Anwendung in die vereinheitlichten APIs referenziert wird. Dies liegt in der Regel daran, dass eine Komponente oder ein nuget-Paket im Projekt vorhanden ist, das nicht auf den Unified API aktualisiert wurde Sie müssen die vorhandene Komponente bzw. nuget entfernen, ein Update auf eine Version durchführen, die die vereinheitlichten APIs unterstützt, und einen sauberen Build durchführen.

### <a name="the-road-to-64-bits"></a>Der Weg zu 64 Bits

Hintergrundinformationen zur Unterstützung von 32-und 64-Bit-Anwendungen sowie Informationen zu Frameworks finden Sie unter den [Überlegungen zu 32 und 64 Bit](~/cross-platform/macios/32-and-64/index.md)

 <a name="new-data-types"></a>

#### <a name="new-data-types"></a>Neue Datentypen

Im Kern des Unterschieds verwenden sowohl Mac-als auch IOS-APIs eine architekturspezifische Datentypen, die auf 32-Bit-Plattformen immer 32 Bit und 64 Bit auf 64-Bit-Plattformen sind.

Beispielsweise ordnet Ziel-C den `NSInteger` Datentyp `int32_t` auf 32-Bit-Systemen und auf `int64_t` 64-Bit-Systemen zu.

Um dieses Verhalten zu erfüllen, ersetzen wir in unserem Unified API die vorherigen Verwendungen von `int` (die in .net als immer wird definiert definiert `System.Int32` ) auf einen neuen Datentyp: `System.nint` .  Sie können sich "n" als "Native" vorstellen, also den nativen ganzzahligen Typ der Plattform.

Wir führen `nint` eine Einführung `nuint` `nfloat` durch und stellen auch ggf. auf Ihnen basierende Datentypen bereit.

Weitere Informationen zu diesen Datentyp Änderungen finden Sie im Dokument [native Typen](~/cross-platform/macios/nativetypes.md) .

### <a name="how-to-detect-the-architecture-of-ios-apps"></a>Erkennen der Architektur von IOS-apps

Es kann Situationen geben, in denen Ihre Anwendung wissen muss, ob Sie auf einem 32-Bit-oder einem 64-Bit-IOS-System ausgeführt wird. Der folgende Code kann verwendet werden, um die Architektur zu überprüfen:

```csharp
if (IntPtr.Size == 4) {
    Console.WriteLine ("32-bit App");
} else if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit App");
}
```

<a name="deprecated-apis"></a>

### <a name="arrays-and-systemcollectionsgeneric"></a>Arrays und System. Collections. Generic

Da c#-Indexer einen Typ von erwarten `int` , müssen Sie Werte explizit in umwandeln, `nint` `int` um auf die Elemente in einer Auflistung oder einem Array zuzugreifen. Beispiel:

```csharp
public List<string> Names = new List<string>();
...

public string GetName(nint index) {
    return Names[(int)index];
}

```

Dies ist das erwartete Verhalten, weil die Umwandlung von in den 64-Bit-Wert `int` `nint` Verlust hat, wird keine implizite Konvertierung durchgeführt.

### <a name="converting-datetime-to-nsdate"></a>DateTime wird in nsdate umgerechnet

Bei Verwendung der vereinheitlichten APIs wird die implizite Konvertierung von `DateTime` in- `NSDate` Werte nicht mehr durchgeführt. Diese Werte müssen explizit von einem Typ in einen anderen konvertiert werden. Die folgenden Erweiterungs Methoden können verwendet werden, um diesen Prozess zu automatisieren:

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

<a name="deprecated-typos"></a>

### <a name="deprecated-apis-and-typos"></a>Veraltete APIs und Typos

In der klassischen xamarin. IOS-API (MonoTouch. dll) wurde das- `[Obsolete]` Attribut auf zwei verschiedene Arten verwendet:

- Als **veraltet markierte IOS-API:** Dies ist der Fall, wenn Apple Ihnen Hinweise zum Beenden der Verwendung einer API gibt, da Sie durch eine neuere ersetzt wird. Der Classic API ist weiterhin in Ordnung und oft erforderlich (wenn Sie die ältere Version von IOS unterstützen).
 Eine derartige API (und das- `[Obsolete]` Attribut) ist in den neuen xamarin. IOS-Assemblys enthalten.
- **Falsche API** Für einige APIs gab es Tippfehler in ihren Namen.

Bei den ursprünglichen Assemblys ("MonoTouch. dll" und "xammac. dll") wurde der alte Code aus Kompatibilitätsgründen verfügbar gehalten, aber aus den Unified API Assemblys (xamarin. IOS. dll und xamarin. Mac) entfernt.

<a name="NSObject_ctor"></a>

### <a name="nsobject-subclasses-ctorintptr"></a>NSObject Unterklassen. ctor (IntPtr)

Jede `NSObject` Unterklasse verfügt über einen Konstruktor, der ein akzeptiert `IntPtr` . Auf diese Weise können wir eine neue verwaltete Instanz aus einem systemeigenen ObjC-handle instanziieren.

In Classic war dies ein `public` Konstruktor. Es war jedoch einfach, dieses Feature in Benutzercode zu missbrauchen, z. b. das Erstellen mehrerer verwalteter Instanzen für eine einzelne ObjC-Instanz *oder* das Erstellen einer verwalteten Instanz, die den erwarteten verwalteten Zustand (für Unterklassen) nicht aufzeigte.

Um diese Art von Problemen zu vermeiden `IntPtr` , befinden sich die Konstruktoren jetzt `protected` in der **vereinheitlichten** API, sodass Sie nur für die Unterklassen verwendet werden können. Dadurch wird sichergestellt, dass die richtige/sichere API verwendet wird, um verwaltete Instanzen aus Handles zu erstellen, d. h.

```csharp
var label = Runtime.GetNSObject<UILabel> (handle);
```

Diese API gibt eine vorhandene verwaltete Instanz zurück (falls Sie bereits vorhanden ist) oder erstellt eine neue Instanz (falls erforderlich). Es ist bereits in klassischer und einheitlicher API verfügbar.

Beachten Sie, dass die `.ctor(NSObjectFlag)` jetzt auch ist, `protected` aber diese wurde nur selten außerhalb der Unterklassen verwendet.

<a name="NSAction"></a>

### <a name="nsaction-replaced-with-action"></a>Nsaction ersetzt durch Aktion

Mit den Unified APIs wurde aus `NSAction` dem standardmäßigen .net entfernt `Action` . Dies ist eine große Verbesserung `Action` , da ein gängiger .NET-Typ ist, der jedoch `NSAction` für xamarin. IOS spezifisch ist. Beide Aufgaben tun genau dasselbe, aber Sie waren unterschiedliche und inkompatible Typen, und es musste mehr Code geschrieben werden, um das gleiche Ergebnis zu erzielen.

Wenn Ihre vorhandene xamarin-Anwendung z. b. den folgenden Code enthält:

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (new NSAction (delegate() {
    ShowDropDownAnimated (tblDataView);
}));
```

Sie kann jetzt durch einen einfachen Lambda-Ausdruck ersetzt werden:

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (() => ShowDropDownAnimated(tblDataView));
```

Früher war dies ein Compilerfehler, da kein `Action` zugewiesen werden kann `NSAction` `UITapGestureRecognizer` . da nun jedoch einen anstelle von erfordert, `Action` `NSAction` ist er in den vereinheitlichten APIs gültig.

### <a name="custom-delegates-replaced-with-actiont"></a>Durch Aktion ersetzte benutzerdefinierte Delegaten\<T>

In **vereinheitlichten** (z. b. einem Parameter) wurden .net-Delegaten durch ersetzt `Action<T>` . Beispiel:

```csharp
public delegate void NSNotificationHandler (NSNotification notification);
```

kann nun als verwendet werden `Action<NSNotification>` . Dies fördert die Wiederverwendung von Code und verringert die Code Duplizierung in xamarin. IOS und ihren eigenen Anwendungen.

### <a name="taskbool-replaced-with-taskbooleannserror"></a>Der Task \<bool> wurde durch den Task<Boolean, nserror>>

Im **klassischen** Modell gab es einige Async-APIs, die zurückkehrten `Task<bool>` . Einige von Ihnen sind jedoch zu verwenden, wenn ein `NSError` Teil der Signatur war, d. h., der `bool` war bereits vorhanden, `true` und Sie mussten eine Ausnahme abfangen, um das zu erhalten `NSError` .

Da einige Fehler sehr häufig auftreten und der Rückgabewert nicht nützlich war, wurde dieses Muster in **Unified** geändert, um einen zurückzugeben `Task<Tuple<Boolean,NSError>>` . Dies ermöglicht es Ihnen, sowohl den Erfolg als auch alle Fehler zu überprüfen, die während des Async-Aufrufes aufgetreten sind.

### <a name="nsstring-vs-string"></a>NSString im Vergleich zu String

In einigen Fällen mussten einige Konstanten von in geändert werden `string` `NSString` , z. b.`UITableViewCell`

**Klassisch**

```csharp
public virtual string ReuseIdentifier { get; }
```

**Einheitlich**

```csharp
public virtual NSString ReuseIdentifier { get; }
```

Im allgemeinen bevorzugen wir den .net- `System.String` Typ. Trotz der Apple-Richtlinien hingegen vergleichen einige Native API Konstante Zeiger (nicht die Zeichenfolge selbst), und dies kann nur funktionieren, wenn wir die Konstanten als verfügbar machen `NSString` .

 <a name="protocols"></a>

### <a name="objective-c-protocols"></a>Ziel-C-Protokolle

Der ursprüngliche MonoTouch hat keine vollständige Unterstützung für ObjC-Protokolle und einige, nicht optimale API hinzugefügt, um das gängigste Szenario zu unterstützen. Diese Einschränkung ist nicht mehr vorhanden, aber aus Gründen der Abwärtskompatibilität werden mehrere APIs innerhalb von `monotouch.dll` und beibehalten `XamMac.dll` .

Diese Einschränkungen wurden in den vereinheitlichten APIs entfernt und bereinigt. Die meisten Änderungen sehen wie folgt aus:

**Klassisch**

```csharp
public virtual AVAssetResourceLoaderDelegate Delegate { get; }
```

**Einheitlich**

```csharp
public virtual IAVAssetResourceLoaderDelegate Delegate { get; }
```

Das `I` Präfix bedeutet, dass **Unified** eine Schnittstelle anstelle eines bestimmten Typs für das ObjC-Protokoll verfügbar macht. Dies erleichtert Fälle, in denen der von xamarin. IOS bereitgestellte spezifische Typ nicht unterteilt werden soll.

Außerdem war es möglich, eine API präziser und benutzerfreundlicher zu verwenden, z. b.:

**Klassisch**

```csharp
public virtual void SelectionDidChange (NSObject uiTextInput);
```

**Einheitlich**

```csharp
public virtual void SelectionDidChange (IUITextInput uiTextInput);
```

Diese API ist jetzt einfacher zu nutzen, ohne auf die Dokumentation zu verweisen, und ihre IDE-Codevervollständigung bietet Ihnen auf der Grundlage des Protokolls/der Schnittstelle ausführlichere Vorschläge.

#### <a name="nscoding-protocol"></a>Nscoding-Protokoll

Unsere ursprüngliche Bindung enthielt eine. ctor (nscoder) für jeden Typ, auch wenn das Protokoll nicht unterstützt wurde `NSCoding` .  Eine einzelne `Encode(NSCoder)` Methode war im vorhanden `NSObject` , um das Objekt zu codieren.
Diese Methode funktioniert jedoch nur, wenn die Instanz mit dem nscoding-Protokoll konform ist.

Auf der Unified API wir dies korrigiert haben.  Die neuen Assemblys verfügen nur über den `.ctor(NSCoder)` , wenn der Typ entspricht `NSCoding` . Außerdem verfügen solche Typen nun über eine- `Encode(NSCoder)` Methode, die der- `INSCoding` Schnittstelle entspricht.

Geringe Auswirkung: in den meisten Fällen wirkt sich diese Änderung nicht auf Anwendungen aus, da die alten, entfernten Konstruktoren nicht verwendet werden konnten.

## <a name="further-tips"></a>Weitere Tipps

Weitere Änderungen, die Sie beachten sollten, sind in den [Tipps zum Aktualisieren von apps auf die Unified API](~/cross-platform/macios/unified/updating-tips.md)aufgeführt.


## <a name="related-links"></a>Verwandte Links

- [Aktualisieren von IOS-apps](updating-ios-apps.md)
- [Aktualisieren von Mac-apps](updating-mac-apps.md)
- [Aktualisieren von xamarin. Forms-apps](updating-xamarin-forms-apps.md)
- [Aktualisieren von Bindungen](update-binding.md)
- [Tipps werden aktualisiert](updating-tips.md)
- [Unterschiede bei klassischem vs Unified API](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
- [Arbeiten mit nativen Typen in plattformübergreifenden Apps](~/cross-platform/macios/native-types-cross-platform.md)
