---
title: Zeichenfolgen- und Imagelokalisierung
description: Xamarin.Forms-apps können mithilfe von .NET Ressourcendateien lokalisiert werden.
ms.prod: xamarin
ms.assetid: 852B4ED3-2D2D-48A5-A759-A6591F6A1509
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/06/2016
ms.openlocfilehash: 09fe3587e4e435383822e50bd12616747b807f82
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108456"
---
# <a name="localization"></a>Lokalisierung

_Xamarin.Forms-apps können mithilfe von .NET Ressourcendateien lokalisiert werden._

## <a name="overview"></a>Übersicht

Der integrierte Mechanismus zum Lokalisieren von .NET Applications verwendet [RESX-Dateien](http://msdn.microsoft.com/library/ekyft91f(v=vs.90).aspx) und die Klassen in der `System.Resources` und `System.Globalization` Namespaces. Die RESX-Dateien, die mit der übersetzten Zeichenfolgen werden in der Xamarin.Forms-Assembly zusammen mit einer vom Compiler generierte Klasse eingebettet, die stark typisierten Zugriff auf die Übersetzungen bereitstellt. Der übersetzte Text können Sie dann im Code abgerufen werden.

### <a name="sample-code"></a>Beispielcode

Es gibt zwei Beispiele, die in diesem Dokument zugeordnet:

* [UsingResxLocalization](https://github.com/xamarin/xamarin-forms-samples/tree/master/UsingResxLocalization) ist ein sehr einfaches Beispiel der Konzepte erläutert. Die unten gezeigten Codeausschnitte stammen alle vom in diesem Beispiel.
* [TodoLocalized](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized) ist eine grundlegende funktionierende app, die diese Techniken für die Lokalisierung verwendet.

#### <a name="shared-projects-are-not-recommended"></a>Freigegebene Projekte werden nicht empfohlen.

Das Beispiel TodoLocalized enthält eine [freigegebenes Projekt Demo](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized/SharedProject/) jedoch aufgrund von Beschränkungen des Buildsystems die Ressourcendateien nicht erhalten eine **. "Designer.cs"** generierte Datei, der den Zugriff auf unterbricht übersetzte Zeichenfolgen im Code stark typisiert.

Im weiteren Verlauf dieses Dokuments bezieht sich auf Projekte mithilfe der Vorlage für Xamarin.Forms .NET Standard-Bibliothek.

## <a name="globalizing-xamarinforms-code"></a>Globalisieren von Xamarin.Forms-Code

**Globalisieren von** eine Anwendung wird somit "World-Ready". Dies bedeutet, dass das Schreiben von Code, der verschiedene Sprachen angezeigt werden.

Einer der wichtigsten Teile des Globalisieren von einer Anwendung ist die Benutzeroberfläche erstellen, sodass es keine *hartcodierte* Text. Stattdessen soll nichts angezeigt wird, für den Benutzer aus einer Reihe von Zeichenfolgen abgerufen werden, die in ihrer gewählten Sprache übersetzt wurden.

In diesem Dokument untersuchen wir so verwenden Sie RESX-Dateien zum Speichern dieser Zeichenfolgen und für die Anzeige je nach Einstellung des Benutzers abrufen.

Die Beispiele als Ziel Sprachen Englisch, Französisch, Spanisch, Deutsch, Chinesisch, Japanisch, Russisch und Portugiesisch (Brasilien). Anwendungen können in möglichst wenige oder beliebig viele Sprachen nach Bedarf übersetzt werden.

> [!NOTE]
> Für die universelle Windows-Plattform sollte RESW--Dateien für die Lokalisierung der Push-Benachrichtigungen, anstatt in RESX-Dateien verwendet werden. Weitere Informationen finden Sie unter [UWP Lokalisierung](/windows/uwp/design/globalizing/globalizing-portal/).

### <a name="adding-resources"></a>Hinzufügen von Ressourcen

Der erste Schritt bei der Globalisierung Xamarin.Forms .NET Standard Library-Anwendung ist die RESX-Ressourcendateien hinzufügen, die zum Speichern von des verwendeten Texts in der app verwendet werden. Wir müssen eine RESX-Datei, die die Standardtext enthält, und klicken Sie dann hinzufügen zusätzliche RESX-Dateien für jede Sprache, die wir unterstützen möchten.

#### <a name="base-language-resource"></a>Ausgangssprache-Ressource

Die Basis-Ressourcendatei (RESX) enthält die Zeichenfolgen der Standardsprache (den Beispielen wird davon ausgegangen, dass Englisch als Standardsprache ist). Das Xamarin.Forms-Projekt der allgemeine Code die Datei hinzugefügt, indem mit der rechten Maustaste auf das Projekt und **hinzufügen > neue Datei...** .

Wählen Sie einen aussagekräftigen Namen wie **AppResources** , und drücken Sie **OK**.

[![Fügen Sie Ressourcendatei](text-images/resx-new-file-sml.png "Dialogfeld \"neue Datei\"")](text-images/resx-new-file.png#lightbox "Dialogfeld \"neue Datei\"")

Zwei Dateien werden dem Projekt hinzugefügt:

* **AppResources.resx** Datei, in der übersetzender Text in einem XML-Format gespeichert werden.
* **AppResources.designer.cs** -Datei, die enthalten Verweise auf alle Elemente in der RESX XML-Datei erstellt eine partielle Klasse deklariert.

Projektmappenbaum verfügbar, werden die Dateien als verwandte angezeigt. Der RESX-Datei *sollten* bearbeitet werden, um das Hinzufügen neuer übersetzbare Zeichenfolgen; die **. "Designer.cs"** Datei sollte *nicht* bearbeitet werden.

![](text-images/appresources-tree.png "AppResources.resx-Datei")

##### <a name="string-visibility"></a>Zeichenfolge-Sichtbarkeit

Standardmäßig Wenn Verweise auf Zeichenfolgen fester typbindung generiert werden, sie werden `internal` auf die Assembly. Dies ist, da das Standard-Build-Tool für RESX-Dateien generiert die **. designer.cs** -Datei mit `internal` Eigenschaften.

Wählen Sie die **AppResources.resx** Datei, und zeigen die **Eigenschaften** Pad klicken, um zu sehen, wobei dieser Build-Tool zu konfigurieren. Der folgende Screenshot zeigt die **benutzerdefiniertes Tool: ResXFileCodeGenerator**.


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](text-images/vs-resx-internal-sml.png "Fenster \"Eigenschaften\" für AppResources.Resx")](text-images/vs-resx-internal.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![](text-images/xs-resx-internal-sml.png "Pad \"Eigenschaften\" für AppResources.Resx")](text-images/xs-resx-internal.png#lightbox)

-----

Die stark typisierte Eigenschaften vornehmen `public`, müssen Sie die Konfiguration manuell ändern **benutzerdefiniertes Tool: PublicResXFileCodeGenerator**, wie im folgenden Screenshot gezeigt:


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](text-images/vs-resx-public-sml.png "Fenster \"Eigenschaften\" für AppResources.Resx")](text-images/vs-resx-public.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![](text-images/xs-resx-internal-sml.png "Pad \"Eigenschaften\" für AppResources.Resx")](text-images/xs-resx-internal.png#lightbox)


[![](text-images/xs-resx-public-sml.png "Pad \"Eigenschaften\" für AppResources.Resx")](text-images/xs-resx-public.png#lightbox)

-----

Diese Änderung ist optional, und ist nur erforderlich, wenn Sie lokalisierte Zeichenfolgen in anderen Assemblys zu verweisen (z. B., wenn Sie die RESX-Dateien in einer anderen Assembly an Ihrem Code platzieren) möchten. Das Beispiel in diesem Thema bewirkt, dass die Zeichenfolgen `internal` , da sie in der gleichen Assembly mit Xamarin.Forms .NET Standard-Bibliothek definiert sind, denen sie verwendet werden.

Sie müssen nur das benutzerdefinierte Tool auf der Basis RESX-Datei festlegen, wie oben gezeigt. Sie müssen nicht festzulegende *alle* Buildereignis-Tools für die sprachspezifische RESX-Dateien, die in den folgenden Abschnitten erläutert.

##### <a name="editing-the-resx-file"></a>Bearbeiten der RESX-Datei

Leider ist gibt es keine integrierte RESX-Editor in Visual Studio für Mac. Hinzufügen von neuen übersetzbare Zeichenfolgen erfordert das Hinzufügen einer neuen XML- `data` -Element für jede Zeichenfolge. Jede `data` Elements kann Folgendes enthalten:

* `name` -Attribut (erforderlich) ist der Schlüssel für diese übersetzbarer Zeichenfolge. Es muss ein gültiger C# Eigenschaftennamen - daher keine Leerzeichen oder Sonderzeichen enthalten dürfen.
* `value` Element (erforderlich), das die tatsächliche Zeichenfolge, die in der Anwendung angezeigt wird.
* `comment` (optional) Element kann es sich um Anweisungen für das Konvertierungsprogramm enthalten, die erklärt, wie diese Zeichenfolge verwendet wird.
* `xml:space` Das Attribut ist (optional) zum Steuern, wie Abstände in der Zeichenfolge beibehalten wird.

Einige Beispiele für `data` Elemente sind hier dargestellt:

```xml
<data name="NotesLabel" xml:space="preserve">
    <value>Notes:</value>
    <comment>label for input field</comment>
</data>
<data name="NotesPlaceholder" xml:space="preserve">
    <value>eg. buy milk</value>
    <comment>example input for notes field</comment>
</data>
<data name="AddButton" xml:space="preserve">
    <value>Add new item</value>
</data>
```

Wenn die Anwendung geschrieben werden, jedes Datenelement an den Benutzer angezeigte Text hinzugefügt werden sollen die Basis RESX-Ressourcendatei in einer neuen `data` Element. Es wird empfohlen, die Sie einschließen `comment`so weit wie möglich ist, um sicherzustellen, dass eine Übersetzung von hoher Qualität.

> [!NOTE]
> Visual Studio (einschließlich der kostenlosen communityedition) enthält einen grundlegende RESX-Editor. Wenn Sie Zugriff auf einen Windows-Computer verfügen, kann dies eine bequeme Möglichkeit zum Hinzufügen und Bearbeiten von Zeichenfolgen in RESX-Dateien sein.

#### <a name="language-specific-resources"></a>Sprachspezifische Ressourcen

In der Regel nicht die eigentliche Übersetzung die standardtextzeichenfolgen ausgeführt, bis große Teile der Anwendung geschrieben wurden (in diesem Fall die standardmäßige RESX-Datei viele Zeichenfolgen enthalten ist). Es ist immer noch eine gute Idee, frühzeitig im Entwicklungszyklus, die sprachspezifische Ressourcen hinzufügen und füllt optional mit maschinell übersetzter Text, um den Lokalisierungscode zu testen.

Für jede Sprache, die wir unterstützen möchten, wird eine zusätzliche RESX-Datei hinzugefügt.
Sprachspezifische Ressourcendateien müssen eine bestimmte Benennungskonvention befolgen: Verwenden Sie den gleichen Dateinamen wie die grundlegenden Ressourcen (z. b. Datei **AppResources**), gefolgt von einem Punkt (.) und der Sprachcode. Einfache Beispiele:

* **AppResources.fr.resx** -sprachübersetzungen Französisch.
* **AppResources.es.resx** -Spanischer Sprache-Übersetzungen.
* **AppResources.de.resx** -deutscher Sprache-Übersetzungen.
* **AppResources.ja.resx** -Japanischer Sprache-Übersetzungen.
* **AppResources.zh-Hans.resx** – Chinesisch (vereinfacht)-Sprache-Übersetzungen.
* **AppResources.zh-Hant.resx** – Chinesisch (traditionell)-Sprache-Übersetzungen.
* **AppResources.pt.resx** -Portugiesisch Sprache-Übersetzungen.
* **AppResources.pt BR.resx** -Übersetzungen Portugiesisch (Brasilien).

Das allgemeine Muster ist die Verwendung von zwei Buchstaben bestehende Sprachcode, aber es gibt einige Beispiele (z. B. Chinesisch), in ein anderes Format verwendet wird, und weitere Beispiele (z. B. Portugiesisch (Brasilien)), wenn ein aus vier Buchstaben bestehende Gebietsschemabezeichner erforderlich ist.

Diese sprachspezifischen Ressourcendateien *nicht* erfordern eine **. "Designer.cs"** partielle Klasse, damit sie als reguläre XML-Dateien mit hinzugefügt werden können die **Buildvorgang: EmbeddedResource**festgelegt. Dieser Screenshot zeigt eine Lösung, die sprachspezifische Ressourcendateien enthält:

![](text-images/appresources-langs.png "Sprachspezifische Ressourcendateien")

Wie eine Anwendung entwickelt wurde, und die Basis RESX-Datei Text hinzugefügt wurde, Sie sollten senden sie Sie an Übersetzer, die jeweils übersetzt `data` Element- und Rückgabe einer sprachspezifischen Ressourcendatei (mit der Benennungskonvention gezeigt), die in der app eingeschlossen werden. Einige Beispiele für die "maschinell übersetzte" werden unten gezeigt:

**AppResources.es.resx (Spanisch)**

```xml
<data name="AddButton" xml:space="preserve">
    <value>Escribir un artículo</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

**AppResources.ja.resx (Japanisch)**

```xml
<data name="AddButton" xml:space="preserve">
    <value>新しい項目を追加</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

**AppResources.pt-BR.resx (Portugiesisch (Brasilien))**

```xml
<data name="AddButton" xml:space="preserve">
    <value>adicionar novo item</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

Nur die `value` Element muss aktualisiert werden, durch die Translator - die `comment` nicht übersetzt werden soll. Denken Sie daran: beim Bearbeiten von XML-Dateien mit Escapezeichen versehen reservierte Zeichen wie `<`, `>`, `&` mit `&lt;`, `&gt;`, und `&amp;` , wenn sie in stehen die `value` oder `comment`.

<a name="incode" />

### <a name="using-resources-in-code"></a>Verwenden von Ressourcen in Code

Zeichenfolgen in die RESX-Ressourcendateien stehen für die Verwendung in Ihrer Benutzer Schnittstelle Code mithilfe der `AppResources` Klasse. Die `name` zugewiesen werden, um jede Zeichenfolge in der RESX-Datei wird eine Eigenschaft für diese Klasse, die in Xamarin.Forms-Code verwiesen werden kann, wie unten dargestellt:

```csharp
var myLabel = new Label ();
var myEntry = new Entry ();
var myButton = new Button ();
// populate UI with translated text values from resources
myLabel.Text = AppResources.NotesLabel;
myEntry.Placeholder = AppResources.NotesPlaceholder;
myButton.Text = AppResources.AddButton;
```

Die Benutzeroberfläche für iOS, Android und die universelle Windows-Plattform (UWP) gerendert, als Sie erwarten, mit Ausnahme von heute ist es möglich, die app in mehrere Sprachen übersetzt werden, weil der Text aus einer Ressource geladen wird, anstatt hartcodierte. So sieht dieser Screenshot zeigt die Benutzeroberfläche auf jeder Plattform vor der Übersetzung aus:

![](text-images/simple-example-english.png "Plattformübergreifende Benutzeroberflächen vor der Übersetzung")

### <a name="troubleshooting"></a>Problembehandlung

#### <a name="testing-a-specific-language"></a>Testen einer bestimmten Sprache

Es kann schwierig sein, wechseln im Simulator oder auf einem Gerät an verschiedene Sprachen, besonders während der Entwicklung, wenn Sie unterschiedlich schnell testen möchten sein.

Sie können erzwingen, dass eine bestimmte Sprache durch Festlegen von geladen werden die `Culture` wie im folgenden Codeausschnitt gezeigt:

```csharp
// force a specific culture, useful for quick testing
AppResources.Culture =  new CultureInfo("fr-FR");
```

Dieser Ansatz – Festlegen der Kultur direkt auf die `AppResources` Klasse – kann auch zum Implementieren einer Sprachauswahl in Ihrer app (und nicht unter Verwendung des Geräts Gebietsschemas) verwendet werden.

#### <a name="loading-embedded-resources"></a>Laden von eingebetteten Ressourcen

Der folgende Codeausschnitt ist nützlich, wenn Sie versuchen, die zum Debuggen von Problemen mit dem eingebettete Ressourcen (z. B. RESX-Dateien). Fügen Sie diesen Code zu Ihrer app (früh im Lebenszyklus app), und es werden alle in der Assembly, die vollständige Ressourcen-ID mit eingebetteten Ressourcen aufgelistet:

```csharp
using System.Reflection;
// ...
// NOTE: use for debugging, not in released app code!
var assembly = typeof(EmbeddedImages).GetTypeInfo().Assembly; // "EmbeddedImages" should be a class in your app
foreach (var res in assembly.GetManifestResourceNames())
{
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

In der **AppResources.Designer.cs** Datei (um *Zeile 33*), die vollständige *-Manager-Ressourcenname* angegeben ist (z. b. `"UsingResxLocalization.Resx.AppResources"`) der folgende Code ähnelt:

```csharp
System.Resources.ResourceManager temp =
        new System.Resources.ResourceManager(
                "UsingResxLocalization.Resx.AppResources",
                typeof(AppResources).GetTypeInfo().Assembly);
```

Überprüfen Sie die **Anwendungsausgabe** für die Ergebnisse der oben gezeigte Debugcode, um zu bestätigen die richtigen Ressourcen sind aufgeführt (d. h. `"UsingResxLocalization.Resx.AppResources"`).

Falls nicht, wird die `AppResources` Klasse werden die Ressourcen konnten nicht geladen werden.
Überprüfen Sie Folgendes ein, um Probleme zu beheben, in dem die Ressourcen können nicht gefunden werden:

* Der Standardnamespace des Projekts entspricht, den Stamm-Namespace in der **AppResources.Designer.cs** Datei.
* Wenn die **AppResources.resx** Datei befindet sich in einem Unterverzeichnis, das der Name des Unterverzeichnisses muss Teil des Namespace *und* Teil des Ressourcenbezeichners.
* Die **AppResources.resx** Datei **Buildvorgang: EmbeddedResource**.
* Die **Projektoptionen > Quellcode > .NET Naming Policies > Verwenden von Visual Studio-Format Ressourcen Namen** ist. Sie können dies untick, falls gewünscht, aktualisiert jedoch die Namespaces, die verwendet werden, wenn Sie Ihre Ressourcen RESX verweisen müssen, in der gesamten app.

#### <a name="doesnt-work-in-debug-mode-android-only"></a>Funktioniert nicht im Debugmodus (nur Android)

Wenn die übersetzten Zeichenfolgen in Ihre Android-Version-Builds, jedoch nicht während des Debuggens verwenden, rechtsklicken Sie auf die **Android-Projekt** , und wählen Sie **Optionen > Erstellen > Android-Build** und sicherstellen, dass die **Schnelle Assemblybereitstellung** nicht ausgewählt ist. Diese Option führt zu Problemen beim Laden von Ressourcen und sollte nicht verwendet werden, wenn Sie lokalisierte apps testen.

### <a name="displaying-the-correct-language"></a>Die richtige Sprache anzeigen

Bisher haben wir untersucht, wie Sie Code schreiben, damit die Übersetzungen bereitgestellt werden können, aber nicht, wie sie angezeigt werden tatsächlich zu machen. Xamarin.Forms-Code kann nutzen. NET Ressourcen, laden Sie die richtige Sprache-Übersetzungen, aber wir müssen zum Abfragen des Betriebssystems auf jeder Plattform aus, um zu bestimmen, welche Sprache der Benutzer ausgewählt hat.

Da einige plattformspezifischen Code zum Abrufen der Spracheinstellung des Benutzers erforderlich ist, verwenden Sie eine [Abhängigkeitsdienst](~/xamarin-forms/app-fundamentals/dependency-service/index.md) verfügbar machen, Informationen in der Xamarin.Forms-app, und es für jede Plattform implementieren.

Zuerst definieren Sie eine Schnittstelle zum Verfügbarmachen von der bevorzugten Kultur des Benutzers, der folgende Code ähnelt:

```csharp
public interface ILocalize
{
    CultureInfo GetCurrentCultureInfo ();
    void SetLocale (CultureInfo ci);
}
```

Verwenden Sie zweitens die [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) in der Xamarin.Forms `App` Klasse rufen die Schnittstelle, und legen unsere Kultur der RESX-Ressourcen, auf den richtigen Wert. Beachten Sie, dass wir nicht zum manuell festlegen dieses Werts für die universelle Windows-Plattform, da das Framework für die Ressourcen automatisch brauchen erkennt die ausgewählte Sprache auf diesen Plattformen.

```csharp
if (Device.RuntimePlatform == Device.iOS || Device.RuntimePlatform == Device.Android)
{
    var ci = DependencyService.Get<ILocalize>().GetCurrentCultureInfo();
    Resx.AppResources.Culture = ci; // set the RESX for resource localization
    DependencyService.Get<ILocalize>().SetLocale(ci); // set the Thread for locale-aware methods
}
```

Die Ressource `Culture` muss festgelegt werden, wenn die Anwendung zuerst geladen wurde, damit die richtige sprachenzeichenfolgen verwendet werden. Sie können optional diesen Wert entsprechend Clientplattform-spezifische Ereignisse aktualisieren, die unter iOS oder Android ausgelöst werden kann, wenn der Benutzer ihre spracheinstellungen aktualisiert, während die app ausgeführt wird.

Die Implementierungen für die `ILocalize` Schnittstelle werden angezeigt, der [plattformspezifischen Code](#Platform-specific_Code) Abschnitt weiter unten. Diese Implementierungen nutzen diesen `PlatformCulture` Helper-Klasse:

```csharp
public class PlatformCulture
{
    public PlatformCulture (string platformCultureString)
    {
        if (String.IsNullOrEmpty(platformCultureString))
        {
            throw new ArgumentException("Expected culture identifier", "platformCultureString"); // in C# 6 use nameof(platformCultureString)
        }
        PlatformString = platformCultureString.Replace("_", "-"); // .NET expects dash, not underscore
        var dashIndex = PlatformString.IndexOf("-", StringComparison.Ordinal);
        if (dashIndex > 0)
        {
            var parts = PlatformString.Split('-');
            LanguageCode = parts[0];
            LocaleCode = parts[1];
        }
        else
        {
            LanguageCode = PlatformString;
            LocaleCode = "";
        }
    }
    public string PlatformString { get; private set; }
    public string LanguageCode { get; private set; }
    public string LocaleCode { get; private set; }
    public override string ToString()
    {
        return PlatformString;
    }
}
```

<a name="Platform-specific_Code" />

### <a name="platform-specific-code"></a>Plattformspezifischen Code

Der Code zum erkennen, die anzuzeigende Sprache plattformspezifische sein muss, da iOS, Android und UWP alle diese Informationen in anderer Hinsicht verfügbar machen. Der Code für die `ILocalize` Abhängigkeitsdienst dient unten für jede Plattform, zusammen mit zusätzlichen Clientplattform-spezifische Anforderungen, um sicherzustellen, dass lokalisierter Text ordnungsgemäß gerendert wird.

Die plattformspezifischen Code muss auch Fälle, in denen das Betriebssystem ermöglicht es den Benutzer, eine Gebietsschema-ID konfigurieren, die von nicht unterstützt wird. NET `CultureInfo` Klasse. In diesen Fällen muss benutzerdefinierter Code geschrieben werden, erkennen nicht unterstützte Gebietsschemas, und Ersetzen Sie die am besten. NET-kompatiblen Gebietsschema.

#### <a name="ios-application-project"></a>Anwendungsprojekt für iOS

iOS-Benutzer wählen Sie ihre bevorzugte Sprache separat von Datums- und Uhrzeitangabe Kultur zu formatieren. Laden Sie die richtigen Ressourcen, um eine Xamarin.Forms-app zu lokalisieren, wir nur zum Abfragen müssen, der `NSLocale.PreferredLanguages` Array für das erste Element.

Die folgende Implementierung des der `ILocalize` Abhängigkeitsdienst im iOS-Anwendungsprojekt platziert werden soll. Da iOS Unterstriche statt Bindestrichen verwendet (die die .NET standard-Darstellung ist), ersetzt der Code den Unterstrich vor dem Instanziieren der `CultureInfo` Klasse:

```csharp
[assembly:Dependency(typeof(UsingResxLocalization.iOS.Localize))]

namespace UsingResxLocalization.iOS
{
public class Localize : UsingResxLocalization.ILocalize
    {
        public void SetLocale (CultureInfo ci)
        {
            Thread.CurrentThread.CurrentCulture = ci;
            Thread.CurrentThread.CurrentUICulture = ci;
        }
        public CultureInfo GetCurrentCultureInfo ()
        {
            var netLanguage = "en";
            if (NSLocale.PreferredLanguages.Length > 0)
            {
                var pref = NSLocale.PreferredLanguages [0];
                netLanguage = iOSToDotnetLanguage(pref);
            }
            // this gets called a lot - try/catch can be expensive so consider caching or something
            System.Globalization.CultureInfo ci = null;
            try
            {
                ci = new System.Globalization.CultureInfo(netLanguage);
            }
            catch (CultureNotFoundException e1)
            {
                // iOS locale not valid .NET culture (eg. "en-ES" : English in Spain)
                // fallback to first characters, in this case "en"
                try
                {
                    var fallback = ToDotnetFallbackLanguage(new PlatformCulture(netLanguage));
                    ci = new System.Globalization.CultureInfo(fallback);
                }
                catch (CultureNotFoundException e2)
                {
                    // iOS language not valid .NET culture, falling back to English
                    ci = new System.Globalization.CultureInfo("en");
                }
            }
            return ci;
        }
        string iOSToDotnetLanguage(string iOSLanguage)
        {
            var netLanguage = iOSLanguage;
            //certain languages need to be converted to CultureInfo equivalent
            switch (iOSLanguage)
            {
                case "ms-MY":   // "Malaysian (Malaysia)" not supported .NET culture
                case "ms-SG":    // "Malaysian (Singapore)" not supported .NET culture
                    netLanguage = "ms"; // closest supported
                    break;
                case "gsw-CH":  // "Schwiizertüütsch (Swiss German)" not supported .NET culture
                    netLanguage = "de-CH"; // closest supported
                    break;
                // add more application-specific cases here (if required)
                // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
        string ToDotnetFallbackLanguage (PlatformCulture platCulture)
        {
            var netLanguage = platCulture.LanguageCode; // use the first part of the identifier (two chars, usually);
            switch (platCulture.LanguageCode)
            {
                case "pt":
                    netLanguage = "pt-PT"; // fallback to Portuguese (Portugal)
                    break;
                case "gsw":
                    netLanguage = "de-CH"; // equivalent to German (Switzerland) for this app
                    break;
                // add more application-specific cases here (if required)
                // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
    }
}
```

> [!NOTE]
> Die `try/catch` , freigegebene Blöcke in der `GetCurrentCultureInfo` Methode in der Regel mit Gebietsschema-Spezifizierer – verwendete werden, wenn die genaue Übereinstimmung nicht nach für eine sehr enge Übereinstimmung nur auf Grundlage der Sprache (erste Block von Zeichen in das Gebietsschema) gefunden wird, dieses Verhalten zu imitieren.
>
> Im Fall von Xamarin.Forms, einigen Gebietsschemas gelten in iOS, jedoch entsprechen nicht auf ein gültiges `CultureInfo` in .NET und der Code oben versucht, dies zu verarbeiten.
>
> Z. B. iOS **Einstellungen > Allgemein Sprache &amp; Region** Bildschirm können Sie Ihr Telefon konfiguriert **Sprache** zu **Englisch** jedoch  **Region** zu **Spanien**, was dazu führt, in einer Gebietsschema `"en-ES"`. Wenn die `CultureInfo` Fehler bei der Erstellung der Code greift auf zurück mit, dass nur die ersten beiden Buchstaben wählen Sie die Anzeigesprache.
>
> Entwickler sollten ändern, die `iOSToDotnetLanguage` und `ToDotnetFallbackLanguage` Methoden zum Verarbeiten der spezifischen Fälle, die für die unterstützten Sprachen erforderlich sind.


Es gibt einige systemdefinierte Benutzeroberflächen-Elemente, die automatisch von iOS übersetzt wird, wie z. B. werden die **Fertig** Schaltfläche der `Picker` Steuerelement. Erzwingen von iOS verwenden, um diese Elemente zu übersetzen, müssen wir die Sprachen angeben, in unterstützt, die **"Info.plist"** Datei. Sie können diese Werte über hinzufügen **"Info.plist" > Quelle** wie hier gezeigt:

![Lokalisierung-Schlüssel in der Datei "Info.plist"](text-images/info-plist.png "Lokalisierung-Schlüssel in der Datei \"Info.plist\"")

Öffnen Sie alternativ die **"Info.plist"** Datei in einem XML-Editor, und bearbeiten Sie die Werte direkt:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>de</string>
    <string>es</string>
    <string>fr</string>
    <string>ja</string>
    <string>pt</string> <!-- Brazil -->
    <string>pt-PT</string> <!-- Portugal -->
    <string>ru</string>
    <string>zh-Hans</string>
    <string>zh-Hant</string>
</array>
<key>CFBundleDevelopmentRegion</key>
<string>en</string>
```

Sobald Sie den Dependency-Dienst implementiert und aktualisiert haben **"Info.plist"**, die iOS-app wird in der Lage, lokalisierten Text anzeigen.

> [!NOTE]
> Beachten Sie, die Apple behandelt Portugiesisch etwas anders als Sie erwarten würden.
> Von [ihre Docs](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW2): _"verwenden (Pacific Time) als die Sprach-ID für Portugiesisch wie es in Brasilien und pt-PT als die Sprach-ID für Portugiesisch verwendet wird, wie sie in Portugal verwendet wird"_.
> Dies bedeutet, wenn in einem nicht standardmäßigen Gebietsschema, Portugiesisch Sprache ausgewählt ist, der Fallbacksprache werden Portugiesisch (Brasilien) unter iOS ab, Code geschrieben wird, um dieses Verhalten zu ändern (z. B. die `ToDotnetFallbackLanguage` oben).

Weitere Informationen zu iOS Lokalisierung, finden Sie unter [iOS Lokalisierung](~/ios/app-fundamentals/localization/index.md).

#### <a name="android-application-project"></a>Anwendungsprojekt für Android

Android verfügbar macht, das aktuell ausgewählte Gebietsschema über `Java.Util.Locale.Default`, und auch ein Unterstrich als Trennzeichen verwendet, statt einen Bindestrich (die mit dem folgenden Code ersetzt wird). Fügen Sie diese Abhängigkeit dienstimplementierung auf das Projekt für Android-Anwendung hinzu:

```csharp
[assembly:Dependency(typeof(UsingResxLocalization.Android.Localize))]

namespace UsingResxLocalization.Android
{
    public class Localize : UsingResxLocalization.ILocalize
    {
        public void SetLocale(CultureInfo ci)
        {
            Thread.CurrentThread.CurrentCulture = ci;
            Thread.CurrentThread.CurrentUICulture = ci;
        }
        public CultureInfo GetCurrentCultureInfo()
        {
            var netLanguage = "en";
            var androidLocale = Java.Util.Locale.Default;
            netLanguage = AndroidToDotnetLanguage(androidLocale.ToString().Replace("_", "-"));
            // this gets called a lot - try/catch can be expensive so consider caching or something
            System.Globalization.CultureInfo ci = null;
            try
            {
                ci = new System.Globalization.CultureInfo(netLanguage);
            }
            catch (CultureNotFoundException e1)
            {
                // iOS locale not valid .NET culture (eg. "en-ES" : English in Spain)
                // fallback to first characters, in this case "en"
                try
                {
                    var fallback = ToDotnetFallbackLanguage(new PlatformCulture(netLanguage));
                    ci = new System.Globalization.CultureInfo(fallback);
                }
                catch (CultureNotFoundException e2)
                {
                    // iOS language not valid .NET culture, falling back to English
                    ci = new System.Globalization.CultureInfo("en");
                }
            }
            return ci;
        }
        string AndroidToDotnetLanguage(string androidLanguage)
        {
            var netLanguage = androidLanguage;
            //certain languages need to be converted to CultureInfo equivalent
            switch (androidLanguage)
            {
                case "ms-BN":   // "Malaysian (Brunei)" not supported .NET culture
                case "ms-MY":   // "Malaysian (Malaysia)" not supported .NET culture
                case "ms-SG":   // "Malaysian (Singapore)" not supported .NET culture
                    netLanguage = "ms"; // closest supported
                    break;
                case "in-ID":  // "Indonesian (Indonesia)" has different code in  .NET
                    netLanguage = "id-ID"; // correct code for .NET
                    break;
                case "gsw-CH":  // "Schwiizertüütsch (Swiss German)" not supported .NET culture
                    netLanguage = "de-CH"; // closest supported
                    break;
                    // add more application-specific cases here (if required)
                    // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
        string ToDotnetFallbackLanguage(PlatformCulture platCulture)
        {
            var netLanguage = platCulture.LanguageCode; // use the first part of the identifier (two chars, usually);
            switch (platCulture.LanguageCode)
            {
                case "gsw":
                    netLanguage = "de-CH"; // equivalent to German (Switzerland) for this app
                    break;
                    // add more application-specific cases here (if required)
                    // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
    }
}
```

> [!NOTE]
> Die `try/catch` , freigegebene Blöcke in der `GetCurrentCultureInfo` Methode in der Regel mit Gebietsschema-Spezifizierer – verwendete werden, wenn die genaue Übereinstimmung nicht nach für eine sehr enge Übereinstimmung nur auf Grundlage der Sprache (erste Block von Zeichen in das Gebietsschema) gefunden wird, dieses Verhalten zu imitieren.
>
> Im Fall von Xamarin.Forms einigen Gebietsschemas gelten in Android aber sich nicht auf ein gültiges `CultureInfo` in .NET und der Code oben versucht, dies zu verarbeiten.
>
> Entwickler sollten ändern, die `iOSToDotnetLanguage` und `ToDotnetFallbackLanguage` Methoden zum Verarbeiten der spezifischen Fälle, die für die unterstützten Sprachen erforderlich sind.

Nachdem dieser Code dem Projekt für Android-Anwendung hinzugefügt wurde, werden sie automatisch die übersetzte Zeichenfolgen anzeigen.

> [!NOTE]
>️ **Warnung:** , wenn die übersetzten Zeichenfolgen in Ihre Android-Version-Builds, jedoch nicht während des Debuggens verwenden, rechtsklicken Sie auf die **Android-Projekt** , und wählen Sie **Optionen > Erstellen > Android Erstellen Sie** und sicherstellen, dass die **schnelle Assemblybereitstellung** nicht ausgewählt ist. Diese Option führt zu Problemen beim Laden von Ressourcen und sollte nicht verwendet werden, wenn Sie lokalisierte apps testen.

Weitere Informationen zu Android Lokalisierung, finden Sie unter [Android Lokalisierung](~/android/app-fundamentals/localization.md).

#### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Universelle Windows-Plattform (UWP) Projekte erfordern keine Abhängigkeitsdienst. Stattdessen legt dieser Plattform automatisch der Ressource Kultur richtig fest.

##### <a name="assemblyinfocs"></a>AssemblyInfo.cs

Erweitern Sie den Knoten "Eigenschaften" im .NET Standard Library-Projekt, und doppelklicken Sie auf die **"AssemblyInfo.cs"** Datei. Fügen Sie die folgende Zeile der Datei, die der neutralen Ressourcensprache für die Assembly auf Englisch festlegen möchten:

```csharp
[assembly: NeutralResourcesLanguage("en")]
```

Dieser informiert den Ressourcen-Manager der Standardkultur der app, daher sicherstellen, dass die Zeichenfolgen, die in der Language-neutral RESX-Datei definierten (**AppResources.resx**) wird angezeigt, wenn die app in einem der englische Gebietsschemas ausgeführt wird.

### <a name="example"></a>Beispiel

Nach dem aktualisieren den plattformspezifischen Projekten wie oben, und das erneute Kompilieren der app mit übersetzten RESX-Dateien, werden aktualisierte Übersetzungen in jeder app zur Verfügung. Hier ist ein Screenshot aus dem Beispielcode in vereinfachtem Chinesisch übersetzt:

![](text-images/simple-example-hans.png "Plattformübergreifende Benutzeroberflächen übersetzt, Chinesisch (vereinfacht)")

Weitere Informationen zu UWP Lokalisierung, finden Sie unter [UWP Lokalisierung](/windows/uwp/design/globalizing/globalizing-portal/).

## <a name="localizing-xaml"></a>Lokalisieren von XAML

Beim Erstellen einer Xamarin.Forms-Benutzeroberfläche in XAML, das des Markups etwa wie folgt, mit Zeichenfolgen aussehen würde eingebettet, direkt in der XML-Code:

```xaml
<Label Text="Notes:" />
<Entry Placeholder="eg. buy milk" />
<Button Text="Add to list" />
```

Im Idealfall konnte es übersetzt werden Steuerelemente der Benutzeroberfläche direkt in das XAML, die wir, indem Sie erstellen tun können eine *Markuperweiterung*. Der Code für eine Markuperweiterung, die die RESX-Ressourcen, um XAML verfügbar macht, ist unten dargestellt. Diese Klasse sollte für den die allgemeinen Xamarin.Forms-Code (zusammen mit den XAML-Seiten) hinzugefügt werden:

```csharp
using System;
using System.Globalization;
using System.Reflection;
using System.Resources;
using Xamarin.Forms;
using Xamarin.Forms.Xaml;

namespace UsingResxLocalization
{
    // You exclude the 'Extension' suffix when using in XAML
    [ContentProperty("Text")]
    public class TranslateExtension : IMarkupExtension
    {
        readonly CultureInfo ci = null;
        const string ResourceId = "UsingResxLocalization.Resx.AppResources";

        static readonly Lazy<ResourceManager> ResMgr = new Lazy<ResourceManager>(
            () => new ResourceManager(ResourceId, IntrospectionExtensions.GetTypeInfo(typeof(TranslateExtension)).Assembly));

        public string Text { get; set; }

        public TranslateExtension()
        {
            if (Device.RuntimePlatform == Device.iOS || Device.RuntimePlatform == Device.Android)
            {
                ci = DependencyService.Get<ILocalize>().GetCurrentCultureInfo();
            }
        }

        public object ProvideValue(IServiceProvider serviceProvider)
        {
            if (Text == null)
                return string.Empty;

            var translation = ResMgr.Value.GetString(Text, ci);
            if (translation == null)
            {
#if DEBUG
                throw new ArgumentException(
                    string.Format("Key '{0}' was not found in resources '{1}' for culture '{2}'.", Text, ResourceId, ci.Name),
                    "Text");
#else
                translation = Text; // HACK: returns the key, which GETS DISPLAYED TO THE USER
#endif
            }
            return translation;
        }
    }
}
```

In der folgenden Aufzählung werden die wichtigen Elemente im obigen Code erläutert:

* Die Klasse `TranslateExtension`, aber gemäß der Konvention, die wir sehen, wird als **übersetzen** in unser Markup.
* Die Klasse implementiert `IMarkupExtension`, die Xamarin.Forms für diese Arbeit erforderlich ist.
* `"UsingResxLocalization.Resx.AppResources"` ist der Ressourcenbezeichner für unsere RESX-Ressourcen. Es besteht unsere Standard-Namespace, den Ordner, in dem die Dateien befinden, und den standardmäßigen RESX-Dateinamen.
* Die `ResourceManager` Klasse wurde mit `IntrospectionExtensions.GetTypeInfo(typeof(TranslateExtension)).Assembly)` um zu bestimmen, die aktuelle Assembly beim Laden von Ressourcen aus, und in der statischen zwischengespeicherten `ResMgr` Feld. Er wird erstellt, als eine `Lazy` geben, damit seine Erstellung verzögert wird, bis zur ersten in Verwendung ist die `ProvideValue` Methode.
* `ci` verwendet den Abhängigkeitsdienst ausgewählte Sprache des Benutzers aus dem systemeigenen Betriebssystem abgerufen.
* `GetString` ist die Methode, die die tatsächlichen übersetzte Zeichenfolge aus den Ressourcendateien abruft. Für die universelle Windows-Plattform `ci` wird null sein, da die `ILocalize` Schnittstelle ist nicht auf diesen Plattformen implementiert. Dies entspricht dem Aufrufen der `GetString` Methode nur mit dem ersten Parameter. Stattdessen wird das Ressourcen-Framework erkennt automatisch das Gebietsschema und ruft die übersetzte Zeichenfolge aus der entsprechenden RESX-Datei.
* Fehlerbehandlung eingefügt, um fehlende Ressourcen zu debuggen, indem eine Ausnahme ausgelöst wurde (in `DEBUG` nur im Modus).

Der folgende XAML-Codeausschnitt zeigt, wie die Markuperweiterung verwendet wird. Es gibt zwei dafür erforderlichen Schritte aus:

1. Deklarieren Sie die benutzerdefinierte `xmlns:i18n` Namespace im Stammknoten. Die `namespace` und `assembly` müssen die projekteinstellungen genau – in diesem Beispiel sind identisch, jedoch möglicherweise andere in Ihrem Projekt übereinstimmen.
2. Verwendung `{Binding}` Syntax für Attribute, die normalerweise Text enthält auf Aufrufen der `Translate` Markuperweiterung. Der Ressourcenschlüssel ist der einzige erforderliche Parameter.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        x:Class="UsingResxLocalization.FirstPageXaml"
        xmlns:i18n="clr-namespace:UsingResxLocalization;assembly=UsingResxLocalization">
    <StackLayout Padding="0, 20, 0, 0">
        <Label Text="{i18n:Translate NotesLabel}" />
        <Entry Placeholder="{i18n:Translate NotesPlaceholder}" />
        <Button Text="{i18n:Translate AddButton}" />
    </StackLayout>
</ContentPage>
```

Die folgende ausführlicher Syntax ist auch für die Markuperweiterung gültig:

```xaml
<Button Text="{i18n:TranslateExtension Text=AddButton}" />
```

## <a name="localizing-platform-specific-elements"></a>Lokalisieren von plattformspezifischen-Elemente

Obwohl wir die Übersetzung der Benutzeroberfläche im Xamarin.Forms-Code verarbeiten kann, sind einige Elemente, die in den einzelnen plattformspezifischen Projekten ausgeführt werden müssen. Dieser Abschnitt erläutert, wie Sie lokalisieren:

* Application Name
* Bilder

Das Beispielprojekt enthält eine lokalisierte Image mit dem Namen **flag.png**, bezieht sich auf C# wie folgt:

```csharp
var flag = new Image();
switch (Device.RuntimePlatform)
{
    case Device.iOS:
    case Device.Android:
        flag.Source = ImageSource.FromFile("flag.png");
        break;
    case Device.UWP:
        flag.Source = ImageSource.FromFile("Assets/Images/flag.png");
        break;
}
```

Das Flag-Image ist auch in der XAML wie folgt verwiesen:

```xaml
<Image>
  <Image.Source>
    <OnPlatform x:TypeArguments="ImageSource">
        <On Platform="iOS, Android" Value="flag.png" />
        <On Platform="UWP" Value="Assets/Images/flag.png" />
    </OnPlatform>
  </Image.Source>
</Image>
```

Alle Plattformen werden bildreferenzen wie diese auf lokalisierte Versionen der Bilder, automatisch aufgelöst, solange die Projektstrukturen, die unten beschrieben implementiert werden.

### <a name="ios-application-project"></a>Anwendungsprojekt für iOS

iOS verwendet einen Benennungsstandard Lokalisierungsprojekte aufgerufen oder **.lproj** Verzeichnisse Image und Zeichenfolgenressourcen enthalten. Diese Verzeichnisse können lokalisierte Versionen von Images, die in der app verwendet enthalten und auch die **InfoPlist.strings** -Datei, die verwendet werden kann, um den Namen der app zu lokalisieren. Weitere Informationen zu iOS Lokalisierung, finden Sie unter [iOS Lokalisierung](~/ios/app-fundamentals/localization/index.md).

#### <a name="images"></a>Bilder

Dieser Screenshot zeigt die iOS-Beispiel-app mit sprachspezifischen **.lproj** Verzeichnisse. Wird aufgerufen, das Verzeichnis Spanisch **es.lproj**, enthält die lokalisierte Versionen des Standardbilds, als auch **flag.png**:

![](text-images/ios-resources.png "iOS Projektverzeichnissen Lokalisierung")

Jede Sprachverzeichnis enthält eine Kopie des **flag.png**, für die entsprechende Sprache lokalisiert. Wenn kein Bild angegeben ist, standardmäßig das Bild in das Standardverzeichnis für die Sprache des Betriebssystems. Geben Sie für die vollständige Retina-Unterstützung **@2x** und **@3x** Kopien der einzelnen Bilder.

#### <a name="app-name"></a>App-Name

Den Inhalt der **InfoPlist.strings** ist nur ein einzelner Schlüssel-Wert, den app-Namen konfigurieren:

```csharp
"CFBundleDisplayName" = "ResxEspañol";
```

Wenn die Anwendung ausgeführt wird, werden der Name der app und das Bild beide lokalisiert:

![](text-images/ios-imageicon.png "iOS-App-Beispieltext und Imagelokalisierung")

### <a name="android-application-project"></a>Anwendungsprojekt für Android

Android folgt ein anderes Schema für die Speicherung von lokalisierter Bilder mit verschiedenen **drawable** und **Zeichenfolgen** Verzeichnisse mit dem Language-Code-Suffix. Wenn ein mit vier Buchstaben bestehenden Gebietsschemacode (z. B. "Zh-TW oder pt-BR) erforderlich ist, beachten Sie, dass Android eine zusätzliche **r** folgenden Dash/vorherigen code des Gebietsschemas (z. b. Zh-rTW oder pt-rBR). Weitere Informationen zu Android Lokalisierung, finden Sie unter [Android Lokalisierung](~/android/app-fundamentals/localization.md).

#### <a name="images"></a>Bilder

Dieser Screenshot zeigt die Android Beispiel mit einer lokalisierten Zeichenfolgen und zeichenbarer Ressourcen:

![](text-images/android-resources.png "Android lokalisierte zeichenbarer Ressourcen und String-Verzeichnisse")

Beachten Sie, dass Android keine Zh-Hans verwendet und Zh-Hant-Codes für vereinfacht und Chinesisch (traditionell); Stattdessen wird nur unterstützt länderspezifische Codes Zh-CN und Zh-TW.

Zur Unterstützung der Bilder mit einer anderen Auflösung für HD-Bildschirme erstellen Sie zusätzliche sprachordnern mit `-*dpi` Suffixen, z. B. **Drawables-es-Mdpi**, **Drawables-es-Xdpi**, **Drawables-es-Xxdpi**usw. Finden Sie unter [Android Alternative-Ressourcen bereitstellen](http://developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources) für Weitere Informationen.

#### <a name="app-name"></a>App-Name

Den Inhalt der **von "Strings.xml"** ist nur ein einzelner Schlüssel-Wert, den app-Namen konfigurieren:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">ResxEspañol</string>
</resources>
```

Update der **"mainactivity.cs"** im Android-app-Projekt, damit die `Label` verweist auf die XML-Zeichenfolgen.

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
```

Die app ordnet nun die app-Namen und das Image. Hier ist ein Screenshot des Ergebnisses (in Spanisch):

![](text-images/android-imageicon.png "Beispiel zu Android-App-Text und Imagelokalisierung")

### <a name="universal-windows-platform-application-projects"></a>Universelle Windows Plattform-Anwendungsprojekten

Die universelle Windows-Plattform verfügt über eine Ressource-Infrastruktur, die die Lokalisierung von Abbildern und den Namen der app vereinfacht. Weitere Informationen zu UWP Lokalisierung, finden Sie unter [UWP Lokalisierung](/windows/uwp/design/globalizing/globalizing-portal/).

#### <a name="images"></a>Bilder

Bilder können in einem Ordner ressourcenspezifischen Ausschnitte lokalisiert werden, wie im folgenden Screenshot gezeigt:

![](text-images/uwp-image-folder-structure.png "Ordnerstruktur für UWP-Image-Lokalisierung")

Zur Laufzeit wird die Windows-Resource-Infrastruktur das entsprechende Image basierend auf dem Gebietsschema des Benutzers ausgewählt.

## <a name="summary"></a>Zusammenfassung

Xamarin.Forms-Anwendungen können RESX-Dateien mit .NET Globalisierungsklassen lokalisiert werden. Abgesehen von der eine kleine Menge von plattformspezifischem Code zum Erkennen der verwendeten Sprache, die der Benutzer bevorzugt, ist in der allgemeine Code den meisten Aufwand Lokalisierung zentralisiert.

Bilder werden in der Regel auf eine plattformspezifische Weise nutzen zur Unterstützung der mit mehreren Auflösungen in iOS und Android behandelt.

## <a name="related-links"></a>Verwandte Links

- [RESX-Lokalisierungsbeispiel](https://developer.xamarin.com/samples/xamarin-forms/UsingResxLocalization/)
- [TodoLocalized-Beispiel-App](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalized/)
- [Cross-Platform-Lokalisierung](~/cross-platform/app-fundamentals/localization.md)
- [iOS-Lokalisierung](~/ios/app-fundamentals/localization/index.md)
- [Android-Lokalisierung](~/android/app-fundamentals/localization.md)
- [UWP-Lokalisierung](/windows/uwp/design/globalizing/globalizing-portal/)
- [Verwenden der CultureInfo-Klasse (MSDN)](http://msdn.microsoft.com/library/87k6sx8t%28v=vs.90%29.aspx)
- [Suchen und Verwenden von Ressourcen für eine bestimmte Kultur (MSDN)](http://msdn.microsoft.com/library/s9ckwb4b%28v=vs.90%29.aspx)
