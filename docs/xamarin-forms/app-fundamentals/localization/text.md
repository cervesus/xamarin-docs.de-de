---
title: Zeichenfolgen- und Imagelokalisierung
description: Xamarin.Forms-Apps können mithilfe von .NET-Ressourcendateien lokalisiert werden.
ms.prod: xamarin
ms.assetid: 852B4ED3-2D2D-48A5-A759-A6591F6A1509
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/06/2016
ms.openlocfilehash: 6f12670dd463471ba1e337802453c775adbe16a7
ms.sourcegitcommit: 0044d04990faa0b144b8626a4fceea0fdff95cfe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/22/2019
ms.locfileid: "56666947"
---
# <a name="localization"></a>Lokalisierung

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/UsingResxLocalization/)

_Xamarin.Forms-Apps können mithilfe von .NET-Ressourcendateien lokalisiert werden._

## <a name="overview"></a>Übersicht

Der integrierte Mechanismus zum Lokalisieren von .NET-Anwendungen verwendet [RESX-Dateien](http://msdn.microsoft.com/library/ekyft91f(v=vs.90).aspx) und die Klassen in den Namespaces `System.Resources` und `System.Globalization`. Die RESX-Dateien mit übersetzten Zeichenfolgen werden in die Xamarin.Forms-Assembly gemeinsam mit einer vom Compiler generierten Klasse eingebettet. Diese Klasse ermöglicht stark typisierten Zugriff auf die Übersetzung. Der übersetzte Text kann dann im Code abgerufen werden.

### <a name="sample-code"></a>Beispielcode

Es gibt zwei Beispiele zu dem hier vorliegenden Dokument:

* [UsingResxLocalization](https://github.com/xamarin/xamarin-forms-samples/tree/master/UsingResxLocalization) stellt eine sehr einfache Demonstration der erläuterten Konzepte dar. Die später verwendeten Codeausschnitte stammen alle aus diesem Beispiel.
* Bei [TodoLocalized](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized) handelt es sich um eine einfache funktionierende App, die diese Lokalisierungstechniken verwendet.

#### <a name="shared-projects-are-not-recommended"></a>Freigegebene Projekte werden nicht empfohlen.

Das TodoLocalized-Beispiel umfasst zwar eine [Demo für freigegebene Projekte](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized/SharedProject/), aber da das Buildsystem eingeschränkt ist, können die Ressourcendateien keine **.designer.cs**-Datei erstellen. Dadurch kann nicht mehr auf stark typisierte übersetzte Zeichenfolgen in Code zugegriffen werden.

Der Rest dieses Artikels bezieht sich auf Projekte, in denen die Vorlage für die Xamarin.Forms-.NET Standard-Bibliothek verwendet wird.

## <a name="globalizing-xamarinforms-code"></a>Globalisieren von Xamarin.Forms-Code

Der Prozess, in dem die Anwendung auf die weltweite Verwendung vorbereitet wird, wird als **Globalisierung** bezeichnet. Das bedeutet, dass Code geschrieben wird, der verschiedene Sprachen anzeigen kann.

Ein Hauptbestandteil der Globalisierung einer Anwendung ist das Erstellen einer Benutzeroberfläche ohne *hartcodierten* Text. Stattdessen sollen sämtliche Elemente, die dem Benutzer angezeigt werden, aus verschiedenen Zeichenfolgen abgerufen werden, die in die ausgewählte Sprache übersetzt wurden.

In diesem Dokument wird erläutert, wie RESX-Dateien zum Speichern und Abrufen dieser Zeichenfolgen verwendet werden können, damit sie je nach Einstellung des Benutzers angezeigt werden können.

Die Beispiele sind für die folgenden Sprachen verfügbar: Englisch, Französisch, Deutsch, Chinesisch, Japanisch, Russisch und Portugiesisch (Brasilien). Anwendungen können in so viele Sprachen wie nötig übersetzt werden.

> [!NOTE]
> Auf der Universellen Windows-Plattform sollten anstelle von RESX-Dateien RESW-Dateien zur Lokalisierung von Pushbenachrichtigungen verwendet werden. Weitere Informationen finden Sie unter [UWP Localization (UWP-Lokalisierung)](/windows/uwp/design/globalizing/globalizing-portal/).

### <a name="adding-resources"></a>Hinzufügen von Ressourcen

Der erste Schritt der Globalisierung einer .NET Standard-Bibliotheksanwendung mit Xamarin.Forms ist das Hinzufügen einer RESX-Ressourcendatei, in der der gesamte in der App verwendete Text gespeichert werden soll. Fügen Sie erst eine RESX-Datei, die den Standardtext enthält, und anschließend zusätzliche RESX-Dateien für die einzelnen Sprachen hinzu, die unterstützt werden sollen.

#### <a name="base-language-resource"></a>Basisressourcen für Sprachen

Die Datei für Basisressourcen (RESX) enthält Zeichenfolgen in den Standardsprachen (in den Beispielen wird von Englisch als der Standardsprache ausgegangen). Fügen Sie die Datei zum Xamarin.Forms-Projekt mit allgemeinem Code hinzu, indem Sie erst mit der rechten Maustaste auf das Projekt und anschließend mit der linken auf **Hinzufügen > Neue Datei...** klicken.

Wählen Sie einen aussagekräftigen Namen aus, z. B. **AppResources**, und klicken Sie auf **OK**.

[![Ressourcendatei hinzufügen](text-images/resx-new-file-sml.png "Dialogfeld „Neue Datei“")](text-images/resx-new-file.png#lightbox "Dialogfeld „Neue Datei“")

Dann werden zwei Dateien zum Projekt hinzugefügt:

* die **AppResources.resx**-Datei, in der übersetzbare Zeichenfolgen im XML-Format gespeichert werden
* die **AppResources.designer.cs**, die eine partielle Klasse dahingehend deklariert, dass sie Verweise auf alle in der RESX-XML-Datei erstellten Elemente enthält.

In der Projektmappenstruktur werden die Dateien miteinander verknüpft. Die RESX-Datei *sollte* bearbeitet werden, damit neue übersetzbare Zeichenfolgen hinzugefügt werden können. Die **.designer.cs**-Datei hingegen sollte *nicht* bearbeitet werden.

![](text-images/appresources-tree.png "AppResources.resx-Datei")

##### <a name="string-visibility"></a>Sichtbarkeit der Zeichenfolgen

Wenn stark typisierte Verweise auf Zeichenfolgen erstellt werden, gelten sie standardmäßig für die Assembly als `internal`. Dies liegt daran, dass das Standardbuildtool für RESX-Dateien die **.designer.cs**-Datei zusammen mit `internal`-Eigenschaften erstellt.

Wählen Sie die **AppResources.resx**-Datei aus, und rufen Sie das **Eigenschaftenpad** ab, um zu prüfen, an welcher Stelle das Buildtool konfiguriert ist. Auf dem folgenden Screenshot sehen Sie das **benutzerdefinierte Tool ResXFileCodeGenerator**.


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](text-images/vs-resx-internal-sml.png "Eigenschaftenfenster für AppResources.Resx")](text-images/vs-resx-internal.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![](text-images/xs-resx-internal-sml.png "Eigenschaftenpad für AppResources.Resx")](text-images/xs-resx-internal.png#lightbox)

-----

Wenn die stark typisierten Zeichenfolgeneigenschaften `public` sein sollen, müssen Sie die Konfiguration manuell auf das **benutzerdefinierte Tool PublicResXFileCodeGenerator** festlegen, wie es im folgenden Screenshot gezeigt ist:


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](text-images/vs-resx-public-sml.png "Eigenschaftenfenster für AppResources.Resx")](text-images/vs-resx-public.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![](text-images/xs-resx-internal-sml.png "Eigenschaftenpad für AppResources.Resx")](text-images/xs-resx-internal.png#lightbox)


[![](text-images/xs-resx-public-sml.png "Eigenschaftenpad für AppResources.Resx")](text-images/xs-resx-public.png#lightbox)

-----

Diese Änderung ist optional und nur erforderlich, wenn Sie Verweise auf lokalisierte Zeichenfolgen für verschiedene Assemblys herstellen möchten (z. B., wenn Sie die RESX-Dateien in eine andere Assembly für Ihren Code verschieben). In dem Beispiel zu diesem Artikel bleiben die Zeichenfolgen `internal`, weil sie in der .NET Standard-Bibliotheksassembly für Xamarin.Forms definiert sind, in der sie auch verwendet werden.

Sie müssen nur das benutzerdefinierte Tool für die RESX-Basisdatei festlegen. Es ist nicht notwendig, dass Sie *ein* Buildtool für die sprachspezifischen RESX-Dateien festlegen, die in den folgenden Abschnitten thematisiert werden.

##### <a name="editing-the-resx-file"></a>Bearbeiten der RESX-Datei

Leider gibt es keinen in Visual Studio für Mac integrierten RESX-Editor. Für jede neu hinzugefügte übersetzbare Zeichenfolge muss auch ein neues `data`-Element für die XML hinzugefügt werden. Jedes `data`-Element kann aus den folgenden Bestandteilen bestehen:

* Das `name`-Attribut (erforderlich) ist der Schlüssel für diese übersetzbare Zeichenfolge. Es muss sich dabei um einen gültigen C#-Eigenschaftennamen handeln. Daher sind weder Leerzeichen noch Sonderzeichen zulässig.
* Das `value`-Element (erforderlich) ist die tatsächliche Zeichenfolge, die in der Anwendung angezeigt wird.
* Das `comment`-Element (optional) kann Anweisungen für den Übersetzer enthalten, in denen erläutert wird, wie die Zeichenfolge verwendet wird.
* Das `xml:space`-Attribut (optional) steuert, wie die Abstände in der Zeichenfolge beibehalten werden.

Einige Beispiele für `data`-Elemente finden Sie im folgenden Codebeispiel:

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

Während die Anwendung geschrieben wird, sollte jeglicher Text, der dem Benutzer angezeigt werden soll, den RESX-Basisressourcendateien in einem neuen `data`-Element hinzugefügt werden. Es wird empfohlen, dass Sie so viele `comment`-Elemente wie möglich hinzufügen, damit eine qualitativ hochwertige Übersetzung erstellt werden kann.

> [!NOTE]
> Im Lieferumfang von Visual Studio ist ein einfacher RESX-Editor enthalten. Dies gilt auch für die kostenlose Community Edition. Wenn Sie Zugriff auf einen Windows-Computer haben, können Sie diesen nutzen, um Zeichenfolgen zu RESX-Dateien hinzuzufügen und diese zu bearbeiten.

#### <a name="language-specific-resources"></a>Sprachspezifische Ressourcen

In der Regel werden die Standardtextzeichenfolgen erst übersetzt, wenn bereits ein Großteil der Anwendung geschrieben wurde. In diesem Fall enthält die RESX-Standarddatei mehrere Zeichenfolgen. Trotzdem sollten Sie die sprachspezifischen Ressourcen schon früh im Laufe des Entwicklungszyklus hinzufügen. Optional können Sie diese auch mit maschinell übersetztem Text auffüllen, um den Lokalisierungscode besser testen zu können.

Für jede Sprache, die unterstützt werden soll, wird eine zusätzliche RESX-Datei hinzugefügt.
Für sprachspezifische Ressourcendateien muss eine bestimmte Namenskonvention eingehalten werden. Verwenden Sie denselben Dateinamen wie für die Basisressourcendatei (z. B. **AppResources**), und fügen Sie erst einen Punkt (.) und anschließend den Sprachcode hinzu. Hier einige einfache Beispiele:

* **AppResources.fr.resx:** Übersetzungen ins Französische
* **AppResources.es.resx:** Übersetzungen ins Spanische
* **AppResources.de.resx:** Übersetzungen ins Deutsche
* **AppResources.ja.resx:** Übersetzungen ins Japanische
* **AppResources.zh-Hans.resx:** Übersetzungen ins Chinesische (vereinfacht)
* **AppResources.zh-Hant.resx:** Übersetzungen ins Chinesische (traditionell)
* **AppResources.pt.resx:** Übersetzungen ins Portugiesische
* **AppResources.pt-BR.resx:** Übersetzungen ins Portugiesische (Brasilien)

In der Regel werden die Ländercodes verwendet, die aus zwei Buchstaben bestehen, aber teilweise werden auch andere Formate (wie beim Chinesischen) oder sogar Gebietsschemabezeichner verwendet, die aus vier Zeichen bestehen (wie beim brasilianischen Portugiesisch), um die jeweilige Region anzugeben.

Für diese sprachspezifischen Ressourcendateien ist *keine* partielle **.designer.cs**-Klasse erforderlich. Daher können sie als reguläre XML-Dateien hinzugefügt werden, wenn für den Buildvorgang **EmbeddedResource** festgelegt ist. Auf dem folgenden Screenshot ist eine Projektmappe zu sehen, die sprachspezifische Ressourcendateien enthält:

![](text-images/appresources-langs.png "Sprachspezifische Ressourcendateien")

Wenn Sie eine Anwendung entwickeln und der RESX-Basisdatei Text hinzufügen, sollten Sie sie an entsprechende Übersetzer senden, die dann alle `data`-Elemente übersetzen und eine sprachspezifische Ressourcendatei liefern, die anhand der bereits erläuterten Namenskonvention benannt wird und zu der App hinzugefügt werden kann. Nachfolgend finden Sie einige Beispiele für maschinell erstellte Übersetzungen:

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

Der Übersetzer muss nur das `value`-Element aktualisieren, das `comment`-Element soll nicht übersetzt werden. Denken Sie beim Bearbeiten von XML-Dateien daran, reservierte Zeichen wie `<`, `>`, `&` mit `&lt;`, `&gt;` und `&amp;` mit Escapezeichen zu versehen, wenn Sie im `value` oder im `comment` angezeigt werden.

<a name="incode" />

### <a name="using-resources-in-code"></a>Verwenden von Ressourcen in Code

Sie können Zeichenfolgen aus den RESX-Ressourcendateien mithilfe der `AppResources`-Klasse im Code für Ihre Benutzeroberfläche verwenden. Der `name`, der den einzelnen Zeichenfolgen in der RESX-Datei zugewiesen ist, ist dann eine Eigenschaft für diese Klasse, auf die wie im Folgenden gezeigt in Xamarin.Forms-Code verwiesen werden kann:

```csharp
var myLabel = new Label ();
var myEntry = new Entry ();
var myButton = new Button ();
// populate UI with translated text values from resources
myLabel.Text = AppResources.NotesLabel;
myEntry.Placeholder = AppResources.NotesPlaceholder;
myButton.Text = AppResources.AddButton;
```

Das Rendering der Benutzeroberfläche unter iOS, Android und auf der Universellen Windows-Plattform (UWP) entspricht allen Erwartungen, allerdings kann die App jetzt in mehrere Sprachen übersetzt werden, weil der Text aus einer anderen Ressource geladen und nicht mehr hartcodiert wird. Auf dem folgenden Screenshot wird die UI auf den einzelnen Plattformen vor der Übersetzung gezeigt:

![](text-images/simple-example-english.png "Plattformübergreifende Benutzeroberflächen vor der Übersetzung")

### <a name="troubleshooting"></a>Problembehandlung

#### <a name="testing-a-specific-language"></a>Testen einer bestimmten Sprache

Es kann kompliziert sein, den Simulator oder das Gerät auf verschiedene Sprachen umzustellen. Dies ist vor allem dann der Fall, wenn Sie während der Entwicklung verschiedene Kulturen schnell testen möchten.

Sie können erzwingen, dass eine bestimmte Sprache geladen wird, indem Sie die `Culture` wie im folgenden Codeausschnitt gezeigt festlegen:

```csharp
// force a specific culture, useful for quick testing
AppResources.Culture =  new CultureInfo("fr-FR");
```

Sie können diesen Ansatz (also das Festlegen einer Kultur in der `AppResources`-Klasse direkt) auch für die Implementierung einer Sprachauswahl in Ihrer App verwenden anstatt das Gebietsschema eines Geräts zu verwenden.

#### <a name="loading-embedded-resources"></a>Laden von eingebetteten Ressourcen

Der folgende Codeausschnitt sollte Ihnen beim Beheben von Problemen mit eingebetteten Ressourcen wie RESX-Dateien helfen. Fügen Sie diesen Code in einem frühen Stadium des App-Lebenszyklus zu Ihrer App hinzu. Dann werden alle in die Assembly eingebetteten Ressourcen aufgelistet, indem der vollständige Ressourcenbezeichner angezeigt wird:

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

In der **AppResources.Designer.cs**-Datei (ungefähr in *Zeile 33*) wird der vollständige *Ressourcen-Managername* (z. B. `"UsingResxLocalization.Resx.AppResources"`) ähnlich wie im folgenden Code angegeben:

```csharp
System.Resources.ResourceManager temp =
        new System.Resources.ResourceManager(
                "UsingResxLocalization.Resx.AppResources",
                typeof(AppResources).GetTypeInfo().Assembly);
```

Gleichen Sie die **Anwendungsausgabe** mit den Ergebnissen des oben dargestellten Debugcodes ab, um zu überprüfen, ob die richtigen Ressourcen aufgelistet werden (d. h. `"UsingResxLocalization.Resx.AppResources"`).

Wenn dies nicht der Fall ist, kann die `AppResources`-Klasse ihre Ressourcen nicht laden.
Überprüfen Sie Folgendes, um Probleme zu lösen, bei denen die Ressourcen nicht gefunden werden können:

* Der Standardnamespace für das Projekt sollte mit dem Stammnamespace in der **AppResources.Designer.cs**-Datei übereinstimmen.
* Wenn sich die **AppResources.resx**-Datei in einem Unterverzeichnis befindet, sollte der Name dieses Verzeichnisses Teil des Namespace *und* Teil des Ressourcenbezeichners sein.
* Für die **AppResources.resx**-Datei sollte für den Buildvorgang **EmbeddedResource** festgelegt sein.
* Das Kontrollkästchen für **Projektoptionen > Quellcode > .NET-Benennungsrichtlinien > Use Visual Studio-style resources names (Ressourcennamen im Format von Visual Studio verwenden)** sollte aktiviert sein. Wenn Sie möchten, können Sie diese Option auch deaktivieren, allerdings müssen dann die Namespaces, die verwendet werden, um einen Verweis auf RESX-Ressourcen herzustellen, für die gesamte App aktualisiert werden.

#### <a name="doesnt-work-in-debug-mode-android-only"></a>Funktioniert nicht im DEBUG-Modus (gilt nur für Android)

Wenn die übersetzten Zeichenfolgen zwar in Ihren RELEASE-Builds für Android funktionieren, aber nicht während des Debuggens, klicken Sie mit der rechten Maustaste auf das **Android-Projekt**, navigieren Sie zu **Optionen > Build > Android-Build**, und vergewissern Sie sich, dass das Kontrollkästchen für **Schnelle Assemblybereitstellung** NICHT aktiviert ist. Wenn diese Option aktiviert ist, treten Probleme beim Laden von Ressourcen auf. Sie sollten sie daher nicht verwenden, wenn Sie lokalisierte Apps testen.

### <a name="displaying-the-correct-language"></a>Anzeigen der richtigen Sprache

Bisher wurde in diesem Artikel erläutert, wie Sie Code so schreiben, dass Übersetzungen bereitgestellt werden können. Es wurde allerdings noch nicht erklärt, wie Sie dafür sorgen können, dass die Übersetzungen auch angezeigt werden. Xamarin.Forms-Code kann die Ressourcen von .NET nutzen, um die Übersetzung in der richtigen Sprache anzuzeigen, allerdings ist eine Abfrage des Betriebssystems für die einzelnen Plattformen notwendig, um zu bestimmen, welche Sprache der Benutzer ausgewählt hat.

Da in geringem Maße plattformspezifischer Code erforderlich ist, um die Spracheinstellungen des Benutzers abzufragen, sollten Sie einen [Abhängigkeitsdienst](~/xamarin-forms/app-fundamentals/dependency-service/index.md) verwenden, um diese Informationen der Xamarin.Forms-App zur Verfügung zu stellen und für die einzelnen Plattformen zu implementieren.

Definieren Sie zunächst eine Schnittstelle, um die vom Benutzer bevorzugte Kultur verfügbar zu machen. Nehmen Sie den folgenden Code als Beispiel:

```csharp
public interface ILocalize
{
    CultureInfo GetCurrentCultureInfo ();
    void SetLocale (CultureInfo ci);
}
```

Verwenden Sie dann das [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)-Konzept in der `App`-Klasse für Xamarin.Forms, um die Schnittstelle aufzurufen und die RESX-Ressourcenkultur auf den richtigen Wert festzulegen. Beachten Sie, dass Sie diesen Wert für die UWP nicht manuell festlegen müssen, weil das Ressourcenframework automatisch die ausgewählte Sprache für diese Plattformen erkennt.

```csharp
if (Device.RuntimePlatform == Device.iOS || Device.RuntimePlatform == Device.Android)
{
    var ci = DependencyService.Get<ILocalize>().GetCurrentCultureInfo();
    Resx.AppResources.Culture = ci; // set the RESX for resource localization
    DependencyService.Get<ILocalize>().SetLocale(ci); // set the Thread for locale-aware methods
}
```

Die Ressource `Culture` muss festgelegt werden, wenn die Anwendung zum ersten Mal geladen wird, damit die richtigen Sprachzeichenfolgen verwendet werden. Optional können Sie auch diesen Wert den plattformspezifischen Ereignissen entsprechend aktualisieren, die unter iOS und Android möglicherweise ausgelöst werden, wenn der Benutzer die Spracheinstellungen ändert, während die App gerade ausgeführt wird.

Die Implementierungen für die `ILocalize`-Schnittstelle werden im nächsten Abschnitt ([Plattformspezifischer Code](#Platform-specific_Code)) erläutert. Diese Implementierungen nutzen die folgende `PlatformCulture`-Hilfsprogrammklasse:

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

### <a name="platform-specific-code"></a>Plattformspezifischer Code

Der Code, mit dem ermittelt werden kann, welche Sprache angezeigt wird, muss plattformspezifisch sein, weil diese Informationen für iOS, Android und die UWP nicht auf genau die gleiche Weise verfügbar gemacht werden. Den Code für den Abhängigkeitsdienst `ILocalize` finden Sie in den folgenden Abschnitten zu den einzelnen Plattformen. Außerdem werden plattformspezifische Anforderungen aufgeführt, damit gewährleistet wird, dass lokalisierter Text richtig gerendert wird.

Der plattformspezifische Code muss außerdem Fälle handhaben können, in denen das Betriebssystem es den Benutzern ermöglicht, einen Gebietsschemabezeichner zu konfigurieren, der nicht von der `CultureInfo`-Klasse von .NET unterstützt wird. In diesen Fällen muss benutzerdefinierter Code geschrieben werden, um nicht unterstützte Gebietsschemas zu erkennen und die besten mit .NET kompatiblen Gebietsschemas zu ersetzen.

#### <a name="ios-application-project"></a>Anwendungsprojekt für iOS

iOS-Benutzer wählen ihre bevorzugte Sprache getrennt von der Formatierungskultur für Datum und Uhrzeit aus. Damit die richtigen Ressourcen zum Lokalisieren einer Xamarin.Forms-App geladen werden können, muss nur das `NSLocale.PreferredLanguages`-Array für das erste Element abgefragt werden.

Die folgende Implementierung des `ILocalize`-Abhängigkeitsdienst sollte in dem iOS-Anwendungsprojekt platziert werden. Da unter iOS Unterstriche anstelle von Gedankenstrichen (.NET Standard-Darstellung) verwendet werden, ersetzt der Code den Unterstrich, bevor er die `CultureInfo`-Klasse instanziiert:

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
            // .NET cultures don't support underscores
            string netLanguage = iOSLanguage.Replace("_", "-");

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
> Die `try/catch`-Blöcke in der `GetCurrentCultureInfo`-Methode imitieren das Fallbackverhalten, das in der Regel für Gebietsschemabezeichner verwendet wird. Wenn keine genaue Übereinstimmung gefunden wird, suchen Sie nur anhand der Sprache nach einer starken Übereinstimmung (erster Zeichenblock im Gebietsschema).
>
> Im Fall von Xamarin.Forms sind einige Gebietsschemas zwar unter iOS gültig, stimmen aber nicht mit gültiger `CultureInfo` in .NET überein. Der obenstehende Code soll dieses Szenario verarbeiten.
>
> Beispielsweise können Sie über die Anzeige **Einstellungen > Sprache &amp; Region** unter iOS die **Sprache** für Ihr Smartphone auf **Englisch**, aber die **Region** auf **Spanien** festlegen. Dadurch entsteht die Gebietsschemazeichenfolge `"en-ES"`. Wenn das Erstellen von `CultureInfo` fehlschlägt, verwendet der Code nur die ersten beiden Buchstaben, um die Anzeigesprache auszuwählen.
>
> Entwickler sollten die Methoden `iOSToDotnetLanguage` und `ToDotnetFallbackLanguage` so ändern, dass sie bestimmte Fälle verarbeiten, die für ihre unterstützten Sprachen erforderlich sind.

Einige vom System definierten Benutzeroberflächenelemente wie die Schaltfläche **Fertig** auf dem `Picker`-Steuerelement werden automatisch von iOS übersetzt. Damit iOS gezwungen ist, diese Elemente zu übersetzen, müssen Sie angeben, welche Sprachen in der **Info.plist**-Datei unterstützt werden. Sie können diese Werte wie auf dem folgenden Screenshot gezeigt über **Info.plist > Quelle** hinzufügen:

![Lokalisierungsschlüssel in Info.plist](text-images/info-plist.png "Lokalisierungsschlüssel in Info.plist")

Stattdessen können Sie auch die **Info.plist**-Datei in einem XML-Editor öffnen und die Werte direkt bearbeiten:

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

Sobald Sie den Abhängigkeitsdienst implementiert und die **Info.plist**-Datei aktualisiert haben, kann die iOS-App lokalisierten Text anzeigen.

> [!NOTE]
> Beachten Sie, dass Apple die portugiesische Sprache anders behandelt als Sie vielleicht erwarten würden.
> In der [Apple-Dokumentation](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW2) heißt es wie folgt: _Verwenden Sie „pt“ als Sprach-ID für brasilianisches Portugiesisch und pt-PT als Sprach-ID für das Portugiesisch, das in Portugal gesprochen wird._
> D. h., wenn Portugiesisch als Sprache in einem Gebietsschema verwendet wird, das nicht dem Standard entspricht, ist die Fallbacksprache unter iOS brasilianisches Portugiesisch, wenn kein Code geschrieben wurde, um dieses Verhalten zu vermeiden (s. `ToDotnetFallbackLanguage` weiter oben).

Weitere Informationen zur iOS-Lokalisierung finden Sie unter [iOS Localization (iOS-Lokalisierung)](~/ios/app-fundamentals/localization/index.md).

#### <a name="android-application-project"></a>Anwendungsprojekt für Android

Android macht das zum jeweiligen Zeitpunkt ausgewählte Gebietsschema über `Java.Util.Locale.Default` verfügbar und verwendet ebenfalls einen Unterstrich anstelle eines Gedankenstrichs als Trennzeichen. Dieser wird durch den folgenden Code ersetzt. Fügen Sie die folgende Implementierung des Abhängigkeitsdiensts zum Android-Anwendungsprojekt hinzu:

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
> Die `try/catch`-Blöcke in der `GetCurrentCultureInfo`-Methode imitieren das Fallbackverhalten, das in der Regel für Gebietsschemabezeichner verwendet wird. Wenn keine genaue Übereinstimmung gefunden wird, suchen Sie nur anhand der Sprache nach einer starken Übereinstimmung (erster Zeichenblock im Gebietsschema).
>
> Im Fall von Xamarin.Forms sind einige Gebietsschemas zwar unter Android gültig, stimmen aber nicht mit gültiger `CultureInfo` in .NET überein. Der obenstehende Code soll dieses Szenario verarbeiten.
>
> Entwickler sollten die Methoden `iOSToDotnetLanguage` und `ToDotnetFallbackLanguage` so ändern, dass sie bestimmte Fälle verarbeiten, die für ihre unterstützten Sprachen erforderlich sind.

Sobald dieser Code zum Android-Anwendungsprojekt hinzugefügt wurde, können übersetzte Zeichenfolgen automatisch angezeigt werden.

> [!NOTE]
>️ **WARNUNG:** Wenn die übersetzten Zeichenfolgen zwar in Ihren RELEASE-Builds für Android funktionieren, aber nicht während des Debuggens, klicken Sie mit der rechten Maustaste auf das **Android-Projekt**, navigieren Sie zu **Optionen > Build > Android-Build**, und vergewissern Sie sich, dass das Kontrollkästchen für **Schnelle Assemblybereitstellung** NICHT aktiviert ist. Wenn diese Option aktiviert ist, treten Probleme beim Laden von Ressourcen auf. Sie sollten sie daher nicht verwenden, wenn Sie lokalisierte Apps testen.

Weitere Informationen zur Android-Lokalisierung finden Sie unter [Android Localization (Android-Lokalisierung)](~/android/app-fundamentals/localization.md).

#### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Für UWP-Projekte ist kein Abhängigkeitsdienst erforderlich. Stattdessen legt diese Plattform automatisch die Kultur der Ressource richtig fest.

##### <a name="assemblyinfocs"></a>AssemblyInfo.cs

Erweitern Sie den Eigenschaftenknoten im .NET Standard-Bibliotheksprojekt, und doppelklicken Sie auf die **AssemblyInfo.cs**-Datei. Fügen Sie die folgende Zeile zu der Datei hinzu, um die Assemblysprache für die neutralen Ressourcen auf Englisch festzulegen:

```csharp
[assembly: NeutralResourcesLanguage("en")]
```

Dann wird der Ressourcen-Manager über die Standardkultur der App informiert, wodurch gewährleistet wird, dass die in der sprachneutralen RESX-Datei (**AppResources.resx**) definierten Zeichenfolgen angezeigt werden, wenn die App in einem der englischen Gebietsschemas ausgeführt wird.

### <a name="example"></a>Beispiel

Wenn Sie wie oben dargestellt die plattformspezifischen Projekte aktualisieren und die App mit übersetzten RESX-Dateien erneut kompilieren, sind aktualisierte Übersetzungen in den einzelnen Apps verfügbar. Hier sehen Sie einen Screenshot von der Übersetzung des Beispielcodes ins Chinesische (vereinfacht):

![](text-images/simple-example-hans.png "Übersetzung der plattformübergreifenden Benutzeroberflächen ins Chinesische (vereinfacht)")

Weitere Informationen zur UWP-Lokalisierung finden Sie unter [UWP Localization (UWP-Lokalisierung)](/windows/uwp/design/globalizing/globalizing-portal/).

## <a name="localizing-xaml"></a>Lokalisieren von XAML

Wenn Sie eine Xamarin.Forms-Benutzeroberfläche in XAML erstellen, sollte das Markup in etwa wie folgt aussehen. Zeichenfolgen werden dabei direkt in die XML eingebettet:

```xaml
<Label Text="Notes:" />
<Entry Placeholder="eg. buy milk" />
<Button Text="Add to list" />
```

Im Idealfall können Benutzeroberflächensteuerelemente direkt in die XAML übersetzt werden. Dafür muss nur eine *Markuperweiterung* erstellt werden. Den Code für eine Markuperweiterung, die die RESX-Ressource für XAML verfügbar macht, finden Sie im folgenden Beispiel. Diese Klasse sollte gemeinsam mit den XAML-Seiten zum allgemeinen Xamarin.Forms-Code hinzugefügt werden:

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

Nachfolgend werden die wichtigen Elemente aus dem obenstehenden Code erläutert:

* Die Klasse hat zwar den Namen `TranslateExtension`, aber standardmäßig können Sie mit dem Begriff **Translate** (Übersetzen) in Ihrem Markup auf diese verweisen.
* Die Klasse implementiert die `IMarkupExtension`. Nur wenn dies geschieht, kann Xamarin.Forms funktionieren.
* `"UsingResxLocalization.Resx.AppResources"` ist der Ressourcenbezeichner für die RESX-Ressourcen. Dieser besteht aus unserem Standardnamespace, dem Ordner, in dem sich die Ressourcendateien befinden und dem Namen der RESX-Standarddatei.
* Die `ResourceManager`-Klasse wird mithilfe der `IntrospectionExtensions.GetTypeInfo(typeof(TranslateExtension)).Assembly)` erstellt, um die aktuelle Assembly zu bestimmen, aus der Ressourcen geladen werden sollen, und wird im statischen `ResMgr`-Feld zwischengespeichert. Sie wird als `Lazy`-Typ erstellt, wodurch sie erst erstellt wird, wenn sie zum ersten Mal in der `ProvideValue`-Methode verwendet wird.
* `ci` verwendet den Abhängigkeitsdienst, um die vom Benutzer ausgewählte Sprache aus dem nativen Betriebssystem abzurufen.
* `GetString` ist die Methode, die die tatsächliche übersetzte Zeichenfolge aus den Ressourcendateien abruft. Auf der Universellen Windows-Plattform hat `ci` den Wert NULL, weil die `ILocalize`-Schnittstelle nicht implementiert wird. Dies entspricht dem Vorgang des Aufrufens einer `GetString`-Methode mit nur dem ersten Parameter. Stattdessen erkennt das Ressourcenframework automatisch das Gebietsschema und ruft die übersetzte Zeichenfolge aus der jeweiligen RESX-Datei ab.
* Die Fehlerbehandlung wurde hinzugefügt, um den Debugvorgang von fehlenden Ressourcen durch das Auslösen einer Ausnahme zu unterstützen (gilt nur für den `DEBUG`-Modus).

Anhand des folgenden XAML-Ausschnitts sehen Sie, wie die Markuperweiterung verwendet werden soll. Sie müssen dafür zwei Schritte ausführen:

1. Deklarieren Sie den benutzerdefinierten `xmlns:i18n`-Namespace im Stammknoten. `namespace` und `assembly` müssen genau mit den Projekteinstellungen übereinstimmen. In diesem Beispiel sind sie identisch, sie können sich in Ihrem Projekt aber auch unterscheiden.
2. Verwenden Sie eine `{Binding}`-Syntax für Attribute, die normalerweise Text enthalten, um die Markuperweiterung `Translate` aufzurufen. Der einzige erforderliche Parameter ist der Ressourcenschlüssel.

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

Sie können auch die folgende ausführliche Syntax für die Markuperweiterung verwenden:

```xaml
<Button Text="{i18n:TranslateExtension Text=AddButton}" />
```

## <a name="localizing-platform-specific-elements"></a>Lokalisieren von plattformspezifischen Elementen

Obwohl die Übersetzung der Benutzeroberfläche in Xamarin.Forms-Code abgewickelt werden kann, müssen einige Elemente auf der plattformspezifischen Ebene des Projekts lokalisiert werden. In diesem Abschnitt erfahren Sie, wie Sie die folgenden Elemente lokalisieren:

* Application Name
* Bilder

Das Beispielprojekt umfasst ein lokalisiertes Bild mit dem Namen **flag.png**, auf das in C# wie folgt verwiesen wird:

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

In der XAML wird wie folgt auf das Flagbild verwiesen:

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

Bildverweise wie diese werden von allen Plattformen automatisch in lokalisierte Versionen der Bilder aufgelöst, wenn die nachfolgend erläuterten Strukturen implementiert werden.

### <a name="ios-application-project"></a>Anwendungsprojekt für iOS

iOS verwendet einen Benennungsstandard, der als Lokalisierungsprojekt- oder **.lproj**-Verzeichnisse bezeichnet wird, in denen Bild- und Zeichenfolgenressourcen enthalten sind. Diese Verzeichnisse können lokalisierte Versionen von Bildern, die in der App verwendet werden, und die **InfoPlist.strings**-Datei enthalten, die zum Lokalisieren des App-Namens verwendet werden kann. Weitere Informationen zur iOS-Lokalisierung finden Sie unter [iOS Localization (iOS-Lokalisierung)](~/ios/app-fundamentals/localization/index.md).

#### <a name="images"></a>Bilder

Auf dem folgenden Screenshot sehen Sie die Beispiel-App für iOS mit sprachspezifischen **.lproj**-Verzeichnissen. Das **es.lproj**-Verzeichnis für Spanien enthält lokalisierte Versionen des Standardbilds sowie das **flag.png**-Bild:

![](text-images/ios-resources.png "iOS-Verzeichnisse für Lokalisierungsprojekte")

Jedes Sprachverzeichnis enthält eine entsprechend lokalisierte Kopie des **flag.png**-Bilds. Wenn kein Bild vorhanden ist, wird standardmäßig das Bild verwendet, das im Standardsprachverzeichnis gespeichert ist. Wenn Sie eine vollständige Retina-Unterstützung benötigen, sollten Sie **@2x**- und **@3x**-Kopien jedes Bilds bereitstellen.

#### <a name="app-name"></a>App-Name

**InfoPlist.strings** besteht nur aus einem Schlüsselwert, mit dem der Name der App konfiguriert wird:

```csharp
"CFBundleDisplayName" = "ResxEspañol";
```

Wenn die Anwendung ausgeführt wird, werden sowohl der App-Name als auch das Bild lokalisiert:

![](text-images/ios-imageicon.png "Übersetzung eines Texts und eines Bilds in der iOS-Beispiel-App")

### <a name="android-application-project"></a>Anwendungsprojekt für Android

Unter Android werden lokalisierte Bilder an anderer Stelle gespeichert. Dafür werden **drawable**- und **strings**-Verzeichnisse mit einem Sprachcode als Suffix verwendet. Wenn ein Gebietsschemacode erforderlich ist, der aus vier Buchstaben besteht (z. B. zh-TW oder pt-BR), muss unter Android ein zusätzliches **r** nach dem Gedankenstrich bzw. nach dem Gebietsschemacode eingefügt werden (z. B. zh-rTW oder pt-rBR). Weitere Informationen zur Android-Lokalisierung finden Sie unter [Android Localization (Android-Lokalisierung)](~/android/app-fundamentals/localization.md).

#### <a name="images"></a>Bilder

Auf dem folgenden Screenshot sehen Sie das Android-Beispiel mit einigen lokalisierten „drawable“- und „strings“-Verzeichnissen:

![](text-images/android-resources.png "Lokalisierte „drawable“- und „strings“-Verzeichnisse unter Android")

Beachten Sie, dass unter Android nicht die Codes zh-Hans und zh-Hant für vereinfachtes und traditionelles Chinesisch verwendet werden. Stattdessen werden nur länderspezifische Codes wie zh-CN und zh-TW unterstützt.

Wenn verschiedene Auflösungen von Bildern für HD-Bildschirme unterstützt werden sollen, erstellen Sie zusätzliche Ordner mit `-*dpi`-Suffixen wie **drawables-es-mdpi**, **drawables-es-xdpi** oder **drawables-es-xxdpi**. Weitere Informationen finden Sie unter [Providing Alternative Android Resources (Bereitstellen alternativer Android-Ressourcen)](http://developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources).

#### <a name="app-name"></a>App-Name

**strings.xml** besteht nur aus einem Schlüsselwert, mit dem der Name der App konfiguriert wird:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">ResxEspañol</string>
</resources>
```

Aktualisieren Sie die **MainActivity.cs**-Datei im Android-App-Projekt, damit das `Label`-Objekt einen Verweis auf die XML der Zeichenfolgen herstellt.

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
```

Dann lokalisiert die App den App-Namen und das Bild. Hier sehen Sie einen Screenshot mit dem Ergebnis (für Spanisch):

![](text-images/android-imageicon.png "Übersetzung eines Texts und eines Bilds in der Android-Beispiel-App")

### <a name="universal-windows-platform-application-projects"></a>UWP-Anwendungsprojekte (Universelle Windows-Plattform)

Die Ressourceninfrastruktur der Universellen Windows-Plattform vereinfacht die Lokalisierung von Bildern und des App-Namens. Weitere Informationen zur UWP-Lokalisierung finden Sie unter [UWP Localization (UWP-Lokalisierung)](/windows/uwp/design/globalizing/globalizing-portal/).

#### <a name="images"></a>Bilder

Bilder können lokalisiert werden, indem sie wie auf dem folgenden Screenshot gezeigt in einem ressourcenspezifischen Ordner platziert werden:

![](text-images/uwp-image-folder-structure.png "Ordnerstruktur für die Bildlokalisierung auf der UWP")

Zur Laufzeit wählt die Windows-Ressourceninfrastruktur anhand des Gebietsschemas des Benutzers ein passendes Bild aus.

## <a name="summary"></a>Zusammenfassung

Xamarin.Forms-Anwendungen können mithilfe von RESX-Dateien und .NET-Globalisierungsklassen lokalisiert werden. Ein Großteil der Lokalisierung wird durch den allgemeinen Code gehandhabt. Nur ein geringer Teil des Codes, mit dem die vom Benutzer bevorzugte Sprache ermittelt wird, ist plattformspezifisch.

Bilder werden in der Regel auf plattformspezifischer Ebene gehandhabt. So wird die unter iOS und Android bereitgestellte Unterstützung für mehrere Auflösungen genutzt.

## <a name="related-links"></a>Verwandte Links

- [RESX Localization Sample (RESX-Lokalisierungsbeispiel)](https://developer.xamarin.com/samples/xamarin-forms/UsingResxLocalization/)
- [TodoLocalized Sample App (TodoLocalized-Beispiel-App)](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalized/)
- [Cross-Platform Localization (Plattformübergreifende Lokalisierung)](~/cross-platform/app-fundamentals/localization.md)
- [iOS Localization (iOS-Lokalisierung)](~/ios/app-fundamentals/localization/index.md)
- [Android Localization (Android-Lokalisierung)](~/android/app-fundamentals/localization.md)
- [UWP Localization (UWP-Lokalisierung)](/windows/uwp/design/globalizing/globalizing-portal/)
- [Verwenden der CultureInfo-Klasse (MSDN)](http://msdn.microsoft.com/library/87k6sx8t%28v=vs.90%29.aspx)
- [Suchen und Verwenden von Ressourcen für eine bestimmte Kultur (MSDN)](http://msdn.microsoft.com/library/s9ckwb4b%28v=vs.90%29.aspx)
