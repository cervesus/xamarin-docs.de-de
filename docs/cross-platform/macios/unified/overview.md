---
title: "Übersicht über Unified-API"
description: "Die neue Formatvorlage API einfacher als je zuvor Mac und iOS sowie für das können Sie zur Unterstützung der 32- und 64-Bit-Anwendungen mit dem binary-Code freigeben."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5F0CEC18-5EF6-4A99-9DCF-1A3B57EA157C
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: a05e30f911e46613fe5c567f148b37833bf88306
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="unified-api-overview"></a>Übersicht über Unified-API

_Die neue Formatvorlage API einfacher als je zuvor Mac und iOS sowie für das können Sie zur Unterstützung der 32- und 64-Bit-Anwendungen mit dem binary-Code freigeben._


Zur Verbesserung der codenutzung Mac und iOS und können Entwickler haben eine Codebasis, die auf 32- und 64-Bit, funktioniert in vorgestellt frühzeitig 2015 eine neue API in Xamarin.Mac und Xamarin.iOS Produkte, die die einheitliche API aufgerufen.

> [!IMPORTANT]
> **Klassische Profil Veraltung:** als neue Plattformen in Xamarin.iOS hinzugefügt werden beginnen wir schrittweise Funktionen aus dem klassischen Profil (monotouch.dll) als veraltet markiert. Beispielsweise wurde die Option nicht NRC (neue Ref-Anzahl) entfernt. Für alle einheitliche Anwendungen immer NRC aktiviert wurde (d. h. nicht NRC wurde nie eine Option) und sind keine Probleme bekannt. Zukünftige Versionen werden die Möglichkeit der Verwendung von Boehm als der Garbage Collector entfernt. Dies war auch eine Option für einheitliche Anwendungen nicht verfügbar. Die vollständige Entfernen des klassischen Unterstützung weiter fortfahren mit der Version von Xamarin.iOS 10.0 ist geplant.




# <a name="ios"></a>iOS

Die `Xamarin.iOS.dll` Assembly, die im Lieferumfang von Xamarin.iOS 8.6 ist unsere **erste stabil und unterstützte Version** der Unified API für iOS.
Vorschau-Vorgängerversionen der einheitliche API sind schließen jedoch nicht vollständig kompatibel.

# <a name="mac"></a>Mac

Die `Xamarin.Mac.dll` Assembly in die stabile Kanal Xamarin.Mac ist unsere **erste stabil und unterstützte Version** der Unified API für Mac.
Vorschau-Vorgängerversionen der einheitliche API sind schließen jedoch nicht vollständig kompatibel.

# <a name="runtime-defaults"></a>Common Language Runtime-Standardwerte

Die einheitliche API verwendet standardmäßig die **SGen** Garbage Collection und [neue Verweiszählung](~/ios/internals/newrefcount.md) System zum Nachverfolgen des Besitzes von Objekten. Dieselbe Funktion wurde auf Xamarin.Mac portiert.

Dies löst eine Reihe von Problemen, die Entwickler mit dem alten System Datenwachstums und auch vereinfachen [Speicherverwaltung](~/cross-platform/deploy-test/memory-perf-best-practices.md).

Beachten Sie, dass es möglich ist, neue Refcount-Wert auch für die klassische API zu aktivieren, aber die Standardeinstellung ist konservativ und erfordert keine Benutzer Änderungen vornehmen. Mit der API Unified wir hat die Gelegenheit, ändern Sie die Standardeinstellung, und bieten Entwicklern die Verbesserungen am gleichzeitig Umgestalten und ihren Code erneut zu testen.

<a name="namespace-changes" />

# <a name="library-split"></a>Bibliothek Teilen

Ab diesem Punkt wird unsere APIs auf zwei Arten angefügt werden sollen:

-  **Klassische API:** beschränkt auf 32 Bits (nur) und verfügbar gemacht, in der `monotouch.dll` und `XamMac.dll` Assemblys.
-  **Einheitliche API:** unterstützen 32- und 64-Bit-Entwicklung mit dem eine einzige API zur Verfügung, in der `Xamarin.iOS.dll` und `Xamarin.Mac.dll` Assemblys.

Dies bedeutet, dass für Enterprise-Entwickler (keine Anwendung im App Store), Sie fortfahren können mithilfe der vorhandenen klassischen-APIs, wie wir ihnen eine unbegrenzte Zeitdauer aufbewahrt werden soll, oder Sie verwalten beibehalten wird an die neuen APIs aktualisieren können.

## <a name="namespace-changes"></a>Namespace-Änderungen

Um die Unstimmigkeiten zum Freigeben von Code für Mac und iOS Produkte zu reduzieren, werden wir die Namespaces für die APIs der Produkte ändert.

Wir werden das Präfix "MonoTouch" von unserer iOS-Produkt- und "MonoMac" unser Produkt Mac, auf die Datentypen löschen aus.

Dies vereinfacht den Code zwischen den Macintosh und iOS-Plattformen ohne bedingte Kompilierung Abfragebeispiel freigeben möchten, und reduziert die Komplexität am oberen Rand den Quellcodedateien.

-  **Klassische API:** Namespaces verwenden `MonoTouch.` oder `MonoMac.` Präfix.
-  **Einheitliche API:** kein Namespacepräfix

## <a name="api-changes"></a>API-Änderungen

Die einheitliche API entfernt veraltete Methoden und einige Instanzen vorhanden sind, bei der es Tippfehler in den API-Namen, wenn sie auf die ursprünglichen MonoTouch und MonoMac Namespaces in der klassischen-APIs gebunden wurden. Diese Instanzen in der neuen APIs Unified korrigiert wurden und in Ihrer Komponente, IOS- und Mac-Anwendungen aktualisiert werden müssen. Hier ist eine Liste der gängigsten Einträge, die auftreten kann:

<table width="100%" border="1">
<tr>
    <th>Name der klassische API-Methode</th>
    <th>Name der einheitliche API-Methode</th>
</tr>
<tr>
    <td>UINavigationController.PushViewControllerAnimated()</td>
    <td>UINavigationController.PushViewController()</td>
</tr>
<tr>
    <td>UINavigationController.PopViewControllerAnimated()</td>
    <td>UINavigationController.PopViewController()</td>
</tr>
<tr>
    <td>CGContext.SetRGBFillColor()</td>
    <td>CGContext.SetFillColor()</td>
</tr>
<tr>
    <td>NetworkReachability.SetCallback()</td>
    <td>NetworkReachability.SetNotification()</td>
</tr>
<tr>
    <td>CGContext.SetShadowWithColor</td>
    <td>CGContext.SetShadow</td>
</tr>
<tr>
    <td>UIView.StringSize</td>
    <td>UIKit.UIStringDrawing.StringSize</td>
</tr>
</table>

Eine vollständige Liste der Änderungen beim Wechseln in der klassischen auf die einheitliche API, finden Sie in unserem [Classic (monotouch.dll) Vs Unified (Xamarin.iOS.dll) API-Unterschiede](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) Dokumentation.


## <a name="updating-to-unified"></a>Aktualisieren auf einheitliche

Mehrere alten/unterteilt/veraltete API in **klassischen** sind nicht verfügbar in der **Unified** API. Es kann einfacher sein, beheben Sie die `CS0616` Warnungen vor dem Starten der (manuelle oder automatisierte) zu aktualisieren, da die müssen die `[Obsolete]` -Attribut Nachricht (Teil der Warnung) und leitet Sie an die richtige API.

Beachten Sie, die wir veröffentlichen ein [ *Diff* ](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) im klassischen VS unified API-Änderungen, die entweder vor oder nach Ihrem Projekt Updates verwendet werden können. Weiterhin korrigieren und die häufig veraltete Aufrufe im klassischen wird eine Zeitersparnis (weniger Dokumentation Suchvorgänge) werden.

Führen Sie diese Anweisungen, um [Aktualisieren von vorhandenen iOS-apps](~/cross-platform/macios/unified/updating-ios-apps.md), oder [Mac apps](~/cross-platform/macios/unified/updating-mac-apps.md) der Unified-API.
Überprüfen Sie den Rest dieser Seite und [diese Tipps](~/cross-platform/macios/unified/updating-tips.md) zusätzliche Informationen zum Migrieren von Code.



# <a name="components-and-nuget"></a>Komponenten und NuGet

Die meisten vorhandenen Komponenten und NuGet-Pakete müssen aktualisiert werden, um die einheitliche API zu unterstützen.
Komponenten, die für die klassische-API können nicht aus einem Projekt einheitliche API verwiesen werden.

Vorhandene Komponenten, die keine Verweise auf `monotouch.dll` (oder `XamMac.dll`) sollte keine Updates erforderlich.

## <a name="components"></a>Komponenten

iOS-Komponenten in der [Xamarin Komponentenspeicher](https://components.xamarin.com/) zum Arbeiten mit Projekten einheitliche API aktualisiert werden müssen. Xamarin ist funktionsfähig, um die Komponenten zu aktualisieren, veröffentlichen, und anderen Autoren in die Kunst durchzusetzen.

## <a name="nuget"></a>NuGet

NuGet-Pakete, die zuvor über das klassische API Xamarin.iOS unterstützt veröffentlicht ihre Assemblys mit der **Monotouch10** Plattform Moniker.

Die einheitliche API führt eine neue Plattform-ID für kompatible Pakete - **Xamarin.iOS10**. Vorhandene NuGet-Pakete zum Hinzufügen der Unterstützung für diese Plattform durch Erstellen von für die einheitliche API aktualisiert werden müssen.

> [!IMPORTANT]
> **Hinweis:** haben einen Fehler in der Form _"Fehler 3 kann nicht im selben Projekt Xamarin.iOS"monotouch.dll"und"Xamarin.iOS.dll"enthalten – explizit"Xamarin.iOS.dll"verwiesen wird, während"monotouch.dll"verwiesen wird" Xxx Version = 0.0.000, Culture = Neutral, PublicKeyToken = Null'"_ nach dem Konvertieren der anwendungskennworts an die Unified-APIs, es liegt in der Regel müssen eine Komponente oder die NuGet-Paket in das Projekt, das nicht auf die einheitliche API aktualisiert wurde. Sie müssen die vorhandene Komponente/NuGet entfernen, update auf eine Version, die Unified-APIs unterstützt, und führen Sie einen bereinigten Build.






<a name="deprecated-apis" />

#<a name="arrays-and-systemcollectionsgeneric"></a>Arrays and System.Collections.Generic

Da C#-Indexer erwarten, dass ein `int`, stehen Ihnen explizit umgewandelt `nint` Werte `int` Zugriff auf die Elemente in einer Auflistung oder ein Array. Zum Beispiel:

```csharp
public List<string> Names = new List<string>();
...

public string GetName(nint index) {
    return Names[(int)index];
}

```

Dieses Verhalten wird erwartet, da die Umwandlung aus `int` auf `nint` ist auf 64-Bit-Systemen lossy, eine implizite Konvertierung erfolgt nicht.

# <a name="converting-datetime-to-nsdate"></a>Konvertieren von "DateTime" in NSDate

Bei Verwendung der Unified APIs, die implizite Konvertierung von `DateTime` zu `NSDate` Werte nicht mehr ausgeführt. Diese Werte müssen explizit von einem Typ in einen anderen konvertiert werden. Die folgenden Erweiterungsmethoden können verwendet werden, um diesen Prozess zu automatisieren:

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

# <a name="deprecated-apis-and-typos"></a>Nicht mehr unterstützte APIs und Tippfehler

Inside Xamarin.iOS klassische API (monotouch.dll) die `[Obsolete]` Attribut wurde auf zwei unterschiedliche Arten verwendet:

-  **IOS-API veraltet:** Dies ist bei der Apple-Hinweise zur unter Verwendung einer API, da er durch eine neuere Version ersetzt wird, wird beendet. Die klassische API ist noch gut und oft erforderlich (wenn die ältere Version von iOS unterstützt werden).
 Diese API (und die `[Obsolete]` Attribut), die in den neuen Xamarin.iOS Assemblys enthalten sind.
-  **Falscher API** einige API Tippfehler hatte, auf deren Namen.


Für die ursprüngliche Assemblys (monotouch.dll und XamMac.dll) wir den alten Code verfügbar aus Kompatibilitätsgründen beibehalten, aber sie wurden aus den einheitliche API-Assemblys (Xamarin.iOS.dll und Xamarin.Mac) entfernt

<a name="NSObject_ctor" />

# <a name="nsobject-subclasses-ctorintptr"></a>NSObject Unterklassen .ctor(IntPtr)

Jede `NSObject` Teilklasse verfügt über einen Konstruktor, der akzeptiert ein `IntPtr`. Dies ist wie eine neue verwaltete Instanz aus einem systemeigenen ObjC Handle instanziiert werden können.

Im klassischen Dies war eine `public` Konstruktor. Jedoch leicht zu dieser Funktion in Benutzercode Missbrauch war, z. B. das Erstellen mehrerer Instanzen für eine einzelne ObjC verwaltet *oder* eine verwaltete Instanz zu erstellen, würde den erwarteten verwalteten Zustand (für Unterklassen) fehlt.

Um diese Art von Problemen zu vermeiden der `IntPtr` Konstruktoren sind jetzt `protected` in **einheitliche** -API, die nur für Klassifizierung verwendet werden soll. Dadurch wird sichergestellt, dass die korrigieren/Safe API verwendet wird, um die verwaltete Instanz von Handles, d. h. erstellen

    var label = Runtime.GetNSObject<UILabel> (handle);

Diese API wird eine vorhandene verwaltete Instanz zurückgeben, (falls sie bereits vorhanden ist) oder ein neues Konto (wenn erforderlich) erstellt wird. Es ist bereits in der klassischen und einheitliche API verfügbar.

Beachten Sie, dass die `.ctor(NSObjectFlag)` ist jetzt auch `protected` , aber dieses Objekt wurde außerhalb von Unterklassen selten verwendet.

<a name="NSAction" />

# <a name="nsaction-replaced-with-action"></a>Mit der Aktion ersetzt NSAction

Mit den APIs Unified `NSAction` wurde zugunsten des standardmäßigen .NET entfernt `Action`. Dies ist eine große Verbesserung, da `Action` ist ein allgemeine .NET Typ während `NSAction` wurde speziell für Xamarin.iOS. Beide müssen exakt die gleiche, aber unterschiedliche und inkompatible Typen wurden und führte zu weiteren Code geschrieben werden, um das gleiche Ergebnis erzielen müssen.

Angenommen, Ihre vorhandene Xamarin-Anwendung den folgenden Code enthalten:

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (new NSAction (delegate() {
    ShowDropDownAnimated (tblDataView);
}));
```
Sie können jetzt durch einen einfachen Lambda-Ausdruck ersetzt werden:

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (() => ShowDropDownAnimated(tblDataView));
```

Zuvor wäre, die einen Compilerfehler, da ein `Action` kann nicht zugewiesen werden `NSAction`, da `UITapGestureRecognizer` jetzt ein `Action` anstelle von einer `NSAction` gilt in den Unified-APIs.

# <a name="custom-delegates-replaced-with-actiont"></a>Benutzerdefinierte Delegaten mit der Aktion ersetzt<T>

In **einheitliche** einige einfache (z. B. einen Parameter) .net Delegaten mit ersetzt wurden `Action<T>`. Z. B.

    public delegate void NSNotificationHandler (NSNotification notification);

kann nun verwendet werden, als ein `Action<NSNotification>`. Dieser Code heraufstufen wiederverwenden und Codeduplikaten innerhalb von Xamarin.iOS und Ihren eigenen Anwendungen reduzieren.


# <a name="taskbool-replaced-with-taskbooleannserror"></a>Aufgabe<bool> Aufgabe < boolescher Wert, NSError >> ersetzt

In **klassischen** gab es einige asynchrone APIs zurückgeben `Task<bool>`. Jedoch einige von ihnen, in dem werden soll, wenn ein `NSError` Teil der Signatur, d. h. war der `bool` wurde bereits `true` und mussten Sie Abfangen einer Ausnahme zum Abrufen der `NSError`.

Da einige Fehler häufig werden und der Rückgabewert nicht nützlich waren wurde dieses Muster **einheitliche** zurückzugebenden eine `Task<Tuple<Boolean,NSError>>`. Dadurch können Sie überprüfen den Erfolg und aller Fehler, die während der asynchrone Aufruf aufgetreten sein könnte.

# <a name="nsstring-vs-string"></a>NSString Vs-Zeichenfolge

In einigen Fällen einige Konstanten aus geändert werden mussten `string` zu `NSString`, z. B. `UITableViewCell`

**Classic**

    public virtual string ReuseIdentifier { get; }

**Unified**

    public virtual NSString ReuseIdentifier { get; }

Im Allgemeinen verwenden wir die .NET `System.String` Typ. Jedoch trotz der Richtlinien von Apple, einige systemeigene API Konstanter Zeiger (nicht die Zeichenfolge selbst) vergleichen und diese kann nur verwendet, wenn wir die Konstanten als verfügbar machen `NSString`.


 <a name="protocols" />

# <a name="objective-c-protocols"></a>Objective-C-Protokolle

Die ursprüngliche MonoTouch keine vollständige Unterstützung für ObjC Protokolle und einige – nicht optimal,-API wurden hinzugefügt das häufigste Szenario zu unterstützen. Diese Einschränkung nicht mehr vorhanden, aber für die Abwärtskompatibilität, mehrere APIs beibehalten werden, um in `monotouch.dll` und `XamMac.dll`.

Diese Einschränkungen wurden entfernt und zu den APIs Unified bereinigt. Die meisten Änderungen werden wie folgt aussehen:

**Classic**

    public virtual AVAssetResourceLoaderDelegate Delegate { get; }

**Unified**

    public virtual IAVAssetResourceLoaderDelegate Delegate { get; }

Die `I` Präfix bedeutet, dass **einheitliche** eine Schnittstelle, anstelle eines bestimmten Typs, für das Protokoll ObjC verfügbar machen. Dies wird Fälle vereinfachen, in denen Sie nicht Unterklasse den spezifischen Typ sollen, den Xamarin.iOS bereitgestellt.

Es einige-API, um genauere und einfach zu verwenden, z. B. werden ebenfalls zulässig:

**Classic**

    public virtual void SelectionDidChange (NSObject uiTextInput);

**Unified**

    public virtual void SelectionDidChange (IUITextInput uiTextInput);

Solche API sind jetzt einfacher zu uns, ohne Refering in der Dokumentation, und die IDE-codevervollständigung Geben Sie mit nützlicher Vorschläge basierend auf den Protokollschnittstelle.

## <a name="nscoding-protocol"></a>NSCoding-Protokoll

Unserer ursprüngliche Bindung ein .ctor(NSCoder) für jeden Typ - enthalten, selbst wenn es nicht unterstützt die `NSCoding` Protokoll.  Ein einzelnes `Encode(NSCoder)` Methode wurde im die `NSObject` um das Objekt zu codieren.
Aber diese Methode funktioniert nur, wenn die Instanz NSCoding Protokoll angeglichene.

Klicken Sie auf die einheitliche API haben wir dieses Problem behoben.  Die neuen Assemblys haben nur die `.ctor(NSCoder)` , wenn der Typ entspricht `NSCoding`. Auch solche Typen verfügen jetzt über eine `Encode(NSCoder)` Methode entspricht der `INSCoding` Schnittstelle.

Nur geringe Auswirkungen: In den meisten Fällen wird nicht durch diese Änderung Anwendungen Einfluss auf die Konstruktoren alten, entfernten, konnte nicht verwendet werden.


## <a name="further-tips"></a>Weitere Tipps

Zusätzliche Änderungen zu berücksichtigen sind aufgeführt, der [Tipps zum Aktualisieren von apps, die einheitliche API](~/cross-platform/macios/unified/updating-tips.md).

# <a name="sample-code"></a>Beispielcode

Ab dem 31. Juli haben wir Ports der iOS-Beispiele auf diese neue API veröffentlicht, auf die `magic-types` zur Verzweigung [Monotouch-Samples](https://github.com/xamarin/monotouch-samples/commits/magic-types).

Wir überprüfen für Mac, in beiden Beispielen die [Mac-Samples](https://github.com/xamarin/mac-samples) -Repository (anzeigt, die neuen APIs in Mavericks/Yosemite) als auch 32/64-Bit-Beispiele, in der Verzweigung Magic-Typen [Mac-Samples](https://github.com/xamarin/monotouch-samples/commits/magic-types).



## <a name="related-links"></a>Verwandte Links

- [Aktualisieren von iOS-Apps](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Aktualisieren von Apps für Mac](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Aktualisieren von Apps mit Xamarin.Forms](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Aktualisieren von Bindungen](~/cross-platform/macios/unified/update-binding.md)
- [Arbeiten mit nativen Typen in plattformübergreifenden Apps](~/cross-platform/macios/native-types-cross-platform.md)
- [Aktualisieren von Tipps](~/cross-platform/macios/unified/updating-tips.md)
- [Klassische Vs einheitliche API-Unterschiede](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
