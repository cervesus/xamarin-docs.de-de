---
title: 'title: "Zeichenfolgen- und Imagelokalisierung in Xamarin.Forms" description: "Xamarin.Forms-Apps können mithilfe von .NET-Ressourcendateien lokalisiert werden."'
description: 'zone_pivot_groups: "platform" ms.prod: xamarin ms.assetid: 852B4ED3-2D2D-48A5-A759-A6591F6A1509 ms.technology: xamarin-forms author: profexorgeek ms.author: jusjohns ms.date: 11/01/2019 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 852B4ED3-2D2D-48A5-A759-A6591F6A1509
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 11/01/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: af15dc5a23404a11be6207bef7b4fc3e4bf9fad7
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2020
ms.locfileid: "84137601"
---
# <a name="xamarinforms-string-and-image-localization"></a>Zeichenfolgen- und Bildlokalisierung für Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/usingresxlocalization)

Lokalisierung ist der Prozess, bei dem eine Anwendung an die jeweilige Sprache oder die kulturellen Anforderungen eines Zielmarkts angepasst wird. Damit die Lokalisierung erfolgreich ist, müssen der Text und die Bilder in einer Anwendung möglicherweise in mehrere Sprachen übersetzt werden. Eine lokalisierte Anwendung zeigt den übersetzten Text automatisch basierend auf den Kultureinstellungen des mobilen Geräts an:

![Screenshots der Lokalisierungsanwendung unter iOS und Android](text-images/localizationdemo-screenshots.png)

.NET Framework enthält einen integrierten Mechanismus zum Lokalisieren von Anwendungen mithilfe von [RESX-Ressourcendateien](https://docs.microsoft.com/dotnet/framework/resources/creating-resource-files-for-desktop-apps). Eine Ressourcendatei speichert Text und andere Inhalte als Name/Wert-Paare, mit denen die Anwendung Inhalte für einen bereitgestellten Schlüssel abrufen kann. Mithilfe von Ressourcendateien können lokalisierte Inhalte vom Anwendungscode getrennt werden.

Zum Lokalisieren von Xamarin.Forms-Anwendungen mit Ressourcendateien müssen Sie die folgenden Schritte ausführen:

1. [Erstellen der RESX-Dateien](#create-resx-files) mit übersetzten Text
1. [Angeben der Standardkultur](#specify-the-default-culture) im freigegebenen Projekt
1. [Lokalisieren von Text in Xamarin.Forms](#localize-text-in-xamarinforms).
1. [Lokalisieren von Bildern](#localize-images) basierend auf Kultureinstellungen für jede Plattform
1. [Lokalisieren des Anwendungsnamens](#localize-the-application-name) auf jeder Plattform
1. [Testen der Lokalisierung](#test-localization) auf jeder Plattform

## <a name="create-resx-files"></a>Erstellen von RESX-Dateien

Ressourcendateien sind XML-Dateien mit der Erweiterung **.resx**, die während des Buildprozesses in binäre Ressourcendateien (.resources) kompiliert werden. Visual Studio 2019 generiert eine Klasse, die eine API bereitstellt, die zum Abrufen von Ressourcen verwendet wird. Eine lokalisierte Anwendung enthält in der Regel eine Standardressourcendatei mit allen in der Anwendung verwendeten Zeichenfolgen sowie Ressourcendateien für die einzelnen unterstützten Sprachen. Die Beispielanwendung verfügt über einen **RESX**-Ordner im freigegebenen Projekt, das die Ressourcendateien enthält, und die Standardressourcendatei mit dem Namen **AppResources.resx**.

Ressourcendateien enthalten die folgenden Informationen für jedes Element:

- **Name** gibt den Schlüssel für den Zugriff auf den Text im Code an.
- **Value** (Wert) gibt den übersetzten Text an.
- **Comment** (Kommentar) ist ein optionales Feld, das zusätzliche Informationen enthält.

::: zone pivot="windows"

Eine Ressourcendatei wird mit dem Dialogfeld **Neues Element hinzufügen** in Visual Studio 2019 hinzugefügt:

![Hinzufügen einer neuen Ressource in Visual Studio 2019](text-images/pc-add-resource-file.png)

Nachdem die Datei hinzugefügt wurde, können Zeilen für jede Textressource hinzugefügt werden:

![Angeben einer Standardtextressource in einer RESX-Datei](text-images/pc-default-strings.png)

Die Dropdowneinstellung **Zugriffsmodifizierer** bestimmt, wie Visual Studio die zum Zugriff auf Ressourcen verwendete Klasse generiert. Wenn der Zugriffsmodifizierer auf **öffentlich** oder **intern** festgelegt wird, wird eine generierte Klasse mit angegebener Zugriffsebene erstellt. Wenn der Zugriffsmodifizierer auf **Keine Codegenerierung** festgelegt wird, wird keine Klassendatei erstellt. Die Standardressourcendatei sollte konfiguriert werden, um eine Klassendatei zu generiert. Das führt dazu, dass eine Datei mit der Erweiterung **.designer.cs** zum Projekt hinzugefügt wird.

Nachdem die Standardressourcendatei erstellt wurde, können für jede Kultur, die von der Anwendung unterstützt wird, zusätzliche Dateien erstellt werden. Jede zusätzliche Ressourcendatei sollte die Übersetzungskultur in den Dateinamen einschließen und den **Zugriffsmodifizierer** aufweisen, der auf **Keine Codegenerierung** festgelegt ist. 

Zur Runtime versucht die Anwendung, eine Ressourcenanforderung in der Spezifikationsreihenfolge aufzulösen. Wenn die Gerätekultur z. B. **en-US** ist, sucht die Anwendung nach Ressourcendateien in dieser Reihenfolge:

1. AppResources.en-US.resx
1. AppResources.en.resx
1. AppResources.resx (Standard)

Der folgende Screenshot zeigt eine spanische Übersetzungsdatei mit dem Namen **AppResources.es.cs**:

![Angeben einer Standardtextressource in einer RESX-Datei](text-images/pc-spanish-strings.png)

Die Übersetzungsdatei verwendet dieselben Namenswerte (**Name**), die in der Standarddatei angegeben sind, enthält jedoch in der Spalte **Value** (Wert) Zeichenfolgen in spanischer Sprache. Außerdem wird der **Zugriffsmodifizierer** auf **Keine Codegenerierung** festgelegt.

::: zone-end
::: zone pivot="macos"

Eine Ressourcendatei wird mit dem Dialogfeld **Neue Datei hinzufügen** in Visual Studio 2019 für Mac hinzugefügt:

![Hinzufügen einer neuen Ressource in Visual Studio 2019 für Mac](text-images/mac-add-resource-file.png)

Nachdem eine Standardressourcendatei erstellt wurde, kann Text hinzugefügt werden, indem `data`-Elemente innerhalb des `root`-Elements in der Ressourcendatei erstellt werden:

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
    ...
    <data name="AddButton" xml:space="preserve">
        <value>Add Note</value>
    </data>
    <data name="NotesLabel" xml:space="preserve">
        <value>Notes:</value>
    </data>
    <data name="NotesPlaceholder" xml:space="preserve">
        <value>e.g. Get Milk</value>
    </data>
</root>
```

Eine **.designer.cs**-Klassendatei kann erstellt werden, indem die Eigenschaft **Custom Tool** (benutzerdefiniertes Tool) in den Ressourcendateioptionen festgelegt wird:

![„Custom Tool“ in den Eigenschaften einer Ressourcendatei](text-images/mac-resx-properties.png)

Wenn die Eigenschaft **Custom Tool** (benutzerdefiniertes Tool) auf **PublicResXFileCodeGenerator** festgelegt wird, führt dies zu einer generierten Klasse mit `public`-Zugriff. Wenn die Eigenschaft **Custom Tool** (benutzerdefiniertes Tool) auf **InternalResXFileCodeGenerator** festgelegt wird, führt dies zu einer generierten Klasse mit `internal`-Zugriff. Ein leerer **Custom Tool**-Wert kann keine Klasse generieren. Der Name der generierten Klasse stimmt mit dem Namen der Ressourcendatei überein. Beispielsweise führt die Datei **AppResources.resx** dazu, dass eine `AppResources`-Klasse in einer Datei namens **AppResources.designer.cs** erstellt wird.

Zusätzliche Ressourcendatei können für jede unterstützte Kultur erstellt werden. Jede Sprachdatei sollte die Übersetzungskultur im Dateinamen enthalten. Eine Datei für z. B. **es-MX** sollte also **AppResources.es-MX.resx** genannt werden.

Zur Runtime versucht die Anwendung, eine Ressourcenanforderung in der Spezifikationsreihenfolge aufzulösen. Wenn die Gerätekultur z. B. **en-US** ist, sucht die Anwendung nach Ressourcendateien in dieser Reihenfolge:

1. AppResources.en-US.resx
1. AppResources.en.resx
1. AppResources.resx (Standard)

Sprachübersetzungsdateien sollten die gleichen **Name**-Werte wie die Standarddatei besitzen. Die folgende XML zeigt die spanische Übersetzungsdatei mit dem Namen **AppResources.es.resx**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
    ...
    <data name="NotesLabel" xml:space="preserve">
        <value>Notas:</value>
    </data>
    <data name="NotesPlaceholder" xml:space="preserve">
        <value>por ejemplo . comprar leche</value>
    </data>
    <data name="AddButton" xml:space="preserve">
        <value>Agregar nuevo elemento</value>
    </data>
</root>
```

::: zone-end

## <a name="specify-the-default-culture"></a>Angeben der Standardkultur

Damit Ressourcendateien ordnungsgemäß funktionieren, muss für die Anwendung eine `NeutralResourcesLanguage` angegeben werden. Im freigegebenen Projekt muss die Datei **AssemblyInfo.cs** angepasst werden, um die Standardkultur anzugeben. Der folgende Code zeigt, wie in der **AssemblyInfo.cs**-Datei `NeutralResourcesLanguage` auf **en-US** festgelegt wird.

```csharp
using System.Resources;

[assembly: NeutralResourcesLanguage("en-US")]
```

> [!WARNING]
> Wenn Sie das Attribut `NeutralResourcesLanguage` nicht angeben, gibt die `ResourceManager`-Klasse `null`-Werte für alle Kulturen ohne bestimmte Ressourcendatei zurück. Wenn die Standardkultur angegeben wird, gibt `ResourceManager` Ergebnisse aus der RESX-Standarddatei für nicht unterstützte Kulturen zurück. Deshalb wird empfohlen, immer die `NeutralResourcesLanguage`-Klasse anzugeben, damit der Text für nicht unterstützte Kulturen angezeigt wird.

Wenn eine Standardressourcendatei erstellt und die Standardkultur in der **AssemblyInfo.cs**-Datei angegeben wurde, kann die Anwendung lokalisierte Zeichenfolgen zur Runtime abrufen.

Weitere Informationen zu Ressourcendateien finden Sie unter [Erstellen von Ressourcendateien für .NET-Apps](https://docs.microsoft.com/dotnet/framework/resources/creating-resource-files-for-desktop-apps).

## <a name="localize-text-in-xamarinforms"></a>Lokalisieren von Text in Xamarin.Forms

Text wird in Xamarin.Forms lokalisiert, indem die generierte `AppResources`-Klasse verwendet wird. Diese Klasse wird basierend auf dem Namen der Standardressourcendatei benannt. Da die Ressourcendatei des Beispielprojekts **AppResources.cs** heißt, generiert Visual Studio eine passende Klasse mit dem Namen `AppResources`. Statische Eigenschaften werden in der `AppResources`-Klasse für jede Zeile in der Ressourcendatei generiert. Die folgenden statischen Eigenschaften werden in der `AppResources`-Klasse der Beispielanwendung generiert:

- AddButton
- NotesLabel
- NotesPlaceholder

Durch Zugreifen auf diese Werte als [x:Static](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md#the-xstatic-markup-extension)-Eigenschaften kann lokalisierter Text in XAML angezeigt werden:

```xaml
<ContentPage ...
             xmlns:resources="clr-namespace:LocalizationDemo.Resx">
    <Label Text="{x:Static resources:AppResources.NotesLabel}" />
    <Entry Placeholder="{x:Static resources:AppResources.NotesPlaceholder}" />
    <Button Text="{x:Static resources:AppResources.AddButton}" />
</ContentPage>
```

Lokalisierter Text kann auch im Code abgerufen werden:

```csharp
public LocalizedCodePage()
{
    Label notesLabel = new Label
    {
        Text = AppResources.NotesLabel,
        // ...
    };
    
    Entry notesEntry = new Entry
    {
        Placeholder = AppResources.NotesPlaceholder,
        //...
    };
    
    Button addButton = new Button
    {
        Text = AppResources.AddButton,
        // ...
    };
    
    Content = new StackLayout
    {
        Children = {
            notesLabel,
            notesEntry,
            addButton
        }
    };
}
```

Die Eigenschaften in der `AppResources`-Klasse verwenden den aktuellen Wert der Eigenschaft `System.Globalization.CultureInfo.CurrentUICulture`, um zu bestimmen, aus welcher Kulturressourcendatei Werte abgerufen werden sollen.

## <a name="localize-images"></a>Lokalisieren von Bildern

Zusätzlich zum Speichern von Text sind RESX-Dateien in der Lage, mehr als nur Text zu speichern – sie können ebenfalls Bilder und Binärdaten speichern. Auf mobilen Geräten stehen jedoch mehrere Bildschirmgrößen und Bildschirmdichten zur Verfügung, und jede mobile Plattform besitzt Funktionen zum Anzeigen von dichteabhängigen Bildern. Deshalb sollte die Funktion der Plattform, Bilder zu lokalisieren, verwendet werden, anstatt Bilder in Ressourcendateien zu speichern.

### <a name="localize-images-on-android"></a>Lokalisieren von Bildern unter Android

Unter Android werden lokalisierte zeichenbare Ressourcen (Bilder) mithilfe einer Benennungskonvention für Ordner im **Ressourcenverzeichnis** gespeichert. Ordner erhalten die Bezeichnung **drawable** mit einem Suffix für die Zielsprache. Zum Beispiel wird der Sprachordner für Spanisch **drawable-es** genannt.

Wenn ein Gebietsschema mit vier Buchstaben benötigt wird, erfordert Android ein zusätzliches **r** nach dem Bindestrich. Beispielsweise sollte der Ordner für das Gebietsschema für Mexiko (es-MX) **drawable-es-rMX** genannt werden. Der Bilddateiname muss in jedem Gebietsschemaordner identisch sein.

![Lokalisierte Bilder im Android-Projekt](text-images/pc-android-images.png)

Weitere Informationen finden Sie unter [Android-Lokalisierung](~/android/app-fundamentals/localization.md).

### <a name="localize-images-on-ios"></a>Lokalisieren von Bildern unter iOS

Unter iOS werden lokalisierte Bilder mithilfe einer Benennungskonvention für Ordner im **Ressourcenverzeichnis** gespeichert. Der Standardordner heißt **Base.lproj**. Sprachspezifische Ordner werden mit der Sprache oder dem Gebietsschemanamen, gefolgt von **.Iproj**, benannt. Zum Beispiel wird der Sprachordner für Spanisch **es.lproj** genannt.

Gebietsschemacodes mit vier Buchstaben funktionieren genauso wie Sprachcodes mit zwei Buchstaben. Beispielsweise sollte der Gebietsschemaordner für Mexico (es-MX) **es-MX.lproj** heißen. Der Bilddateiname muss in jedem Gebietsschemaordner identisch sein.

![Lokalisierte Bilder im iOS-Projekt](text-images/pc-ios-images.png)

> [!NOTE]
> iOS unterstützt die Erstellung eines lokalisierten Ressourcenkatalogs anstelle der Verwendung der IPROJ-Ordnerstruktur. Jedoch müssen diese Kataloge in Xcode erstellt und verwaltet werden.

Weitere Informationen finden Sie unter [iOS-Lokalisierung](~/ios/app-fundamentals/localization/index.md).

### <a name="localize-images-on-uwp"></a>Lokalisieren von Bildern auf der UWP

Auf der UWP werden lokalisierte Bilder mithilfe einer Benennungskonvention für Ordner im **Ressourcen- bzw. Bildverzeichnis** gespeichert. Ordner werden mit der Sprache oder dem Gebietsschema benannt. Beispielsweise wird der Ordner für Spanisch **es** genannt, und der Gebietsschemaordner für Mexiko sollte deshalb **es-MX** genannt werden. Der Bilddateiname muss in jedem Gebietsschemaordner identisch sein.

![Lokalisierte Bilder im UWP-Projekt](text-images/pc-uwp-images.png)

Weitere Informationen finden Sie unter [UWP Localization (UWP-Lokalisierung)](/windows/uwp/design/globalizing/globalizing-portal/).

### <a name="consume-localized-images"></a>Nutzen lokalisierter Bilder

Da jede Plattform Bilder mit einer eindeutigen Dateistruktur speichert, verwendet XAML die `OnPlatform`-Klasse, um die `ImageSource`-Eigenschaft basierend auf der aktuellen Plattform festzulegen.

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

> [!NOTE]
> Die Markuperweiterung `OnPlatform` bietet eine präzisere Möglichkeit, plattformspezifische Werte anzugeben. Weitere Informationen finden Sie unter [OnPlatform-Markuperweiterung](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension).

Die Bildquelle kann basierend auf der `Device.RuntimePlatform`-Eigenschaft im Code festgelegt werden:

```csharp
string imgSrc = Device.RuntimePlatform == Device.UWP ? "Assets/Images/flag.png" : "flag.png";
Image flag = new Image
{
    Source = ImageSource.FromFile(imgSrc),
    WidthRequest = 100
};
```

## <a name="localize-the-application-name"></a>Lokalisieren des Anwendungsnames

Der Anwendungsname wird pro Plattform angegeben und verwendet keine RESX-Ressourcendateien. Unter [Lokalisieren des App-Namens unter Android](~/android/app-fundamentals/localization.md#stringsxml-file-format) finden Sie Informationen zum Lokalisieren des Anwendungsnames unter Android. Unter [Lokalisieren des App-Namens unter iOS](~/ios/app-fundamentals/localization/index.md#app-name) finden Sie Informationen zum Lokalisieren des Anwendungsnames unter iOS. Unter [Lokalisieren von Zeichenfolgen im UWP-Paketmanifest](https://docs.microsoft.com/windows/uwp/app-resources/localize-strings-ui-manifest) finden Sie Informationen zum Lokalisieren des Anwendungsnames auf der UWP.

## <a name="test-localization"></a>Lokalisieren von Tests

Das Testen der Lokalisierung erfolgt am besten durch Ändern der Sprache Ihres Geräts. Es ist möglich, den Wert `System.Globalization.CultureInfo.CurrentUICulture` im Code zu ändern, jedoch ist das Verhalten plattformübergreifend inkonsistent, deshalb ist dies für Tests nicht empfehlenswert.

Unter iOS können Sie über die App „Einstellungen“ die Sprache für jede App einstellen, ohne Ihre Gerätesprache ändern zu müssen.

Unter Android werden die Spracheinstellungen erkannt und zwischengespeichert, sobald die Anwendung gestartet wird. Wenn Sie Sprachen ändern, sollten Sie die Anwendung beenden und dann wieder starten, damit die Änderungen übernommen werden.

## <a name="related-links"></a>Verwandte Links

- [Lokalisierungsbeispielprojekt](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/usingresxlocalization)
- [Erstellen von Ressourcendateien für .NET-Apps](https://docs.microsoft.com/dotnet/framework/resources/creating-resource-files-for-desktop-apps)
- [Cross-Platform Localization (Plattformübergreifende Lokalisierung)](~/cross-platform/app-fundamentals/localization.md)
- [Verwenden der CultureInfo-Klasse (MSDN)](https://docs.microsoft.com/dotnet/api/system.globalization.cultureinfo)
- [Android Localization (Android-Lokalisierung)](~/android/app-fundamentals/localization.md)
- [iOS Localization (iOS-Lokalisierung)](~/ios/app-fundamentals/localization/index.md)
- [UWP Localization (UWP-Lokalisierung)](/windows/uwp/design/globalizing/globalizing-portal/)
- [Suchen und Verwenden von Ressourcen für eine bestimmte Kultur (MSDN)](https://msdn.microsoft.com/library/s9ckwb4b%28v=vs.90%29.aspx)
