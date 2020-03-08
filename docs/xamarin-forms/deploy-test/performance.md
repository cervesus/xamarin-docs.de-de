---
title: Verbessern der Leistung von Xamarin.Forms-Apps
description: Es gibt viele Techniken zum Verbessern der Leistung von Xamarin.Forms-Anwendungen. Wenn Sie diese Kniffe kombinieren, können Sie die CPU-Auslastung und die Speichermenge, die von einer Anwendung verwendet wird, erheblich reduzieren.
ms.prod: xamarin
ms.assetid: 0be84c56-6698-448d-be5a-b4205f1caa9f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/27/2019
ms.openlocfilehash: 4427d347723284a2f8897612f10857270c9631bf
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78913414"
---
# <a name="improve-xamarinforms-app-performance"></a>Verbessern der Leistung von Xamarin.Forms-Apps

> [!VIDEO https://youtube.com/embed/RZvdql3Ev0E]

**Weiterentwicklung 2016: Optimieren der App-Leistung mit Xamarin.Forms**

Eine schlechte Anwendungsleistung kann sich auf unterschiedliche Weise bemerkbar machen. Die Anwendung reagiert scheinbar nicht mehr, der Bildlauf ist möglicherweise verlangsamt, und auch die Akkulaufzeit des Geräts kann abnehmen. Leistungsoptimierung umfasst jedoch mehr als das bloße Implementieren eines effizienten Codes. Es muss ebenfalls berücksichtigt werden, wie der Benutzer die Leistung der Anwendung wahrnimmt. Wenn beispielsweise Vorgänge ausgeführt werden können, ohne dass der Benutzer daran gehindert wird, gleichzeitig andere Aktivitäten auszuführen, kann dies dazu beitragen die Benutzerfreundlichkeit zu verbessern.

Es gibt zahlreiche Techniken zum Verbessern der Leistung und der wahrnehmbaren Leistung von Xamarin.Forms-Anwendungen. Wenn Sie diese Kniffe kombinieren, können Sie die CPU-Auslastung und die Speichermenge, die von einer Anwendung verwendet wird, erheblich reduzieren.

> [!NOTE]
> Bevor Sie diesen Artikel lesen, sollten Sie zuerst den Artikel [Cross-Platform Performance (Plattformübergreifende Leistung)](~/cross-platform/deploy-test/memory-perf-best-practices.md) lesen, der nicht-plattformspezifische Methoden zur Verbesserung der Arbeitsspeicherauslastung und Leistung von Anwendungen beschreibt, die mit der Xamarin-Plattform erstellt wurden.

## <a name="enable-the-xaml-compiler"></a>Aktivieren des XAML-Compilers

XAML kann optional auch direkt mit dem XAML-Compiler (XAMLC) in der Zwischensprache (Intermediate Language, IL) kompiliert werden. XAMLC bietet eine Reihe von Vorteilen:

- Er führt eine XAML-Überprüfung zur Kompilierzeit durch und benachrichtigt den Benutzer über eventuelle Fehler.
- Er entfernt etwas Lade- und Instanziierungszeit für XAML-Elemente.
- Er hilft bei der Verringerung der Dateigröße der finalen Assembly, indem XAML-Dateien nicht mehr eingeschlossen werden.

XAMLC ist in neuen Xamarin.Forms-Lösungen standardmäßig aktiviert. Allerdings muss es möglicherweise in älteren Lösungen aktiviert werden. Weitere Informationen finden Sie unter [Compiling XAML](~/xamarin-forms/xaml/xamlc.md) (Kompilieren von XAML).

## <a name="use-compiled-bindings"></a>Verwenden kompilierter Bindungen

Kompilierte Bindungen verbessern die Datenbindungsleistung in Xamarin.Forms-Anwendungen, indem die Bindungsausdrücke zur Kompilierzeit anstatt zur Laufzeit mit Reflektion aufgelöst werden. Das Kompilieren eines Bindungsausdrucks generiert kompilierten Code, der in der Regel eine Bindung 8 bis 20 Mal schneller auflöst als bei Verwendung einer klassischen Bindung. Weitere Informationen finden Sie unter [Kompilierte Bindungen](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md).

## <a name="reduce-unnecessary-bindings"></a>Reduzieren unnötiger Bindungen

Verwenden Sie keine Bindungen für Inhalte, die problemlos statisch festgelegt werden können. Das Binden von Daten, die nicht gebunden werden müssen, bringt keine Vorteile mit sich, da Bindungen nicht kostengünstig sind. Das Festlegen von `Button.Text = "Accept"` ist beispielsweise mit weniger Mehraufwand verbunden als das Binden von [`Button.Text`](xref:Xamarin.Forms.Button.Text) an die Eigenschaft `string` mit dem Wert „Akzeptieren“.

## <a name="use-fast-renderers"></a>Verwenden von schnellen Renderern

Schnelle Renderer reduzieren die Inflations- und Renderingkosten von Xamarin.Forms-Steuerelementen in Android, indem sie die resultierende native Steuerelementhierarchie vereinfachen. Durch die Senkung der Anzahl der erstellten Objekte wird die Leistung weiter verbessert, was wiederum die Komplexität der visuellen Struktur und die Speicherbelegung verringert.

Ab Xamarin.Forms 4.0 verwenden alle Anwendungen mit dem Ziel `FormsAppCompatActivity` standardmäßig schnelle Renderer. Weitere Informationen finden Sie unter [Schnelle Renderer](~/xamarin-forms/internals/fast-renderers.md).

## <a name="enable-startup-tracing-on-android"></a>Aktivieren der Startablaufverfolgung unter Android

Die AOT-Kompilierung (Ahead-Of-Time) unter Android minimiert den JIT-Anwendungsstartaufwand (Just-In-Time) und die Arbeitsspeicherauslastung, wobei ein viel größeres APK erstellt wird. Eine Alternative ist die Verwendung von Startablaufverfolgung, die im Vergleich zur konventionellen AOT-Kompilierung einen Kompromiss zwischen der Android-APK-Größe und der Startzeit bietet.

Anstatt so viel von der Anwendung wie möglich mit nicht verwaltetem Code zu kompilieren, kompiliert die Startablaufverfolgung nur den Satz verwalteter Methoden, die die teuersten Teile des Anwendungsstarts in einer leeren Xamarin.Forms-Anwendung darstellen. Diese Vorgehensweise führt im Vergleich zur konventionellen AOT-Kompilierung zu einer verringerten APK-Größe, während gleichzeitig ähnliche Startverbesserungen bereitgestellt werden.

## <a name="enable-layout-compression"></a>Aktivieren der Layoutkomprimierung

Durch die Layoutkomprimierung werden bestimmte Layouts aus der visuellen Struktur entfernt, um die Leistung des Seitenrenderings zu verbessern. Der daraus resultierende Leistungsvorteil variiert je nach Komplexität einer Seite, der Version des verwendeten Betriebssystems und des Geräts, auf dem die Anwendung ausgeführt wird. Die größten Leistungssteigerungen werden jedoch bei älteren Geräten zu verzeichnen sein. Weitere Informationen finden Sie unter [Layoutkomprimierung](~/xamarin-forms/user-interface/layouts/layout-compression.md).

## <a name="choose-the-correct-layout"></a>Auswählen des richtigen Layouts

Ein Layout, in dem mehrere untergeordnete Elemente angezeigt werden können, das jedoch nur über ein untergeordnetes Element verfügt, ist Vergeudung. Im folgenden Codebeispiel wird ein [`StackLayout`](xref:Xamarin.Forms.StackLayout) mit einem untergeordneten Element dargestellt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DisplayImage.HomePage">
    <StackLayout>
        <Image Source="waterfront.jpg" />
    </StackLayout>
</ContentPage>
```

Dies ist Vergeudung, und das [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Element sollte wie im folgenden Codebeispiel gezeigt entfernt werden:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DisplayImage.HomePage">
    <Image Source="waterfront.jpg" />
</ContentPage>
```

Darüber hinaus sollten Sie nicht versuchen, die Darstellung eines bestimmten Layouts durch Kombinieren anderer Layouts zu reproduzieren. Dies führt dazu, dass unnötige Layoutberechnungen durchgeführt werden. Versuchen Sie beispielsweise nicht, ein [`Grid`](xref:Xamarin.Forms.Grid)-Layout durch eine Kombination aus [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Instanzen zu reproduzieren. Das folgende Codebeispiel veranschaulicht dieses Fehlverhalten beispielhaft:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
    <StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Name:" />
            <Entry Placeholder="Enter your name" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Age:" />
            <Entry Placeholder="Enter your age" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Occupation:" />
            <Entry Placeholder="Enter your occupation" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Address:" />
            <Entry Placeholder="Enter your address" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

Dies ist Vergeudung, da unnötige Layoutberechnungen durchgeführt werden. Stattdessen kann das gewünschte Layout besser wie im folgenden Codebeispiel gezeigt über ein [`Grid`](xref:Xamarin.Forms.Grid) erreicht werden:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="100" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="30" />
            <RowDefinition Height="30" />
            <RowDefinition Height="30" />
            <RowDefinition Height="30" />
        </Grid.RowDefinitions>
        <Label Text="Name:" />
        <Entry Grid.Column="1" Placeholder="Enter your name" />
        <Label Grid.Row="1" Text="Age:" />
        <Entry Grid.Row="1" Grid.Column="1" Placeholder="Enter your age" />
        <Label Grid.Row="2" Text="Occupation:" />
        <Entry Grid.Row="2" Grid.Column="1" Placeholder="Enter your occupation" />
        <Label Grid.Row="3" Text="Address:" />
        <Entry Grid.Row="3" Grid.Column="1" Placeholder="Enter your address" />
    </Grid>
</ContentPage>
```

## <a name="optimize-layout-performance"></a>Optimieren der Layoutleistung

Befolgen Sie die nachstehenden Richtlinien, um die bestmögliche Layoutleistung zu erreichen:

- Verringern Sie die Tiefe der Layouthierarchien durch die Angabe von [`Margin`](xref:Xamarin.Forms.View.Margin)-Eigenschaftswerten. Diese ermöglichen die Erstellung von Layouts mit weniger Umbrüchen in Ansichten. Weitere Informationen finden Sie unter [Seitenränder und Textabstand](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).
- Versuchen Sie bei der Verwendung eines [`Grid`](xref:Xamarin.Forms.Grid), sicherzustellen, dass so wenige Zeilen und Spalten wie möglich auf die Größe [`Auto`](xref:Xamarin.Forms.GridLength.Auto) festgelegt werden. Durch jede Zeile oder Spalte, deren Größe automatisch angepasst wird, wird verursacht, dass die Layout-Engine zusätzliche Layoutberechnungen durchführt. Verwenden Sie stattdessen wenn möglich Zeilen und Spalten mit festen Größen. Alternativ können Sie mit dem [`GridUnitType.Star`](xref:Xamarin.Forms.GridUnitType.Star)-Enumerationswert Zeilen und Spalten festlegen, die einen proportionalen Speicherplatz belegen sollen. Voraussetzung hierfür ist, dass die übergeordnete Struktur diese Layoutrichtlinien einhält.
- Legen Sie die Eigenschaften [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) und [`HorizontalOptions`](xref:Xamarin.Forms.View.VerticalOptions) eines Layouts nur dann fest, wenn dies erforderlich ist. Die Standardwerte von [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) und [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) ermöglichen die beste Layoutoptimierung. Das Ändern dieser Eigenschaften ist mit Kosten und Speicherplatzbelegung verbunden, selbst dann, wenn sie auf die Standardwerte festgelegt werden.
- Vermeiden Sie möglichst die Verwendung eines [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout). Dies führt dazu, dass die CPU erheblich mehr Arbeit übernehmen muss.
- Vermeiden Sie bei Verwendung eines [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) möglichst die Verwendung der Eigenschaft [`AbsoluteLayout.AutoSize`](xref:Xamarin.Forms.AbsoluteLayout.AutoSize).
- Stellen Sie bei Verwendung eines [`StackLayout`](xref:Xamarin.Forms.StackLayout) sicher, dass für [`LayoutOptions.Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) nur ein untergeordnetes Element festgelegt ist. Mit dieser Eigenschaft wird sichergestellt, dass das angegebene untergeordnete Element den größten Bereich belegt, der im `StackLayout` verfügbar ist. Zudem ist es Vergeudung, diese Berechnungen mehrmals durchzuführen.
- Vermeiden Sie den Aufruf der [`Layout`](xref:Xamarin.Forms.Layout)-Klasse, da dies zur Durchführung teurer Layoutberechnungen führt. Die Wahrscheinlichkeit ist groß, dass das gewünschte Layoutverhalten stattdessen durch Festlegen der Eigenschaften [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) und [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) abgerufen werden kann. Alternativ können Sie Unterklassen für die [`Layout<View>`](xref:Xamarin.Forms.Layout`1)-Klasse erstellen, um das gewünschte Layoutverhalten zu erzielen.
- Aktualisieren Sie [`Label`](xref:Xamarin.Forms.Label)-Instanzen nicht häufiger als erforderlich. Die Änderung der Bezeichnungsgröße kann dazu führen, dass das gesamte Bildschirmlayout neu berechnet wird.
- Legen Sie die Eigenschaft [`Label.VerticalTextAlignment`](xref:Xamarin.Forms.Label.VerticalTextAlignment) nur dann fest, wenn dies erforderlich ist.
- Legen Sie den [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) aller [`Label`](xref:Xamarin.Forms.Label)-Instanzen möglichst auf [`NoWrap`](xref:Xamarin.Forms.LineBreakMode.NoWrap) fest.

## <a name="use-asynchronous-programming"></a>Verwenden von asynchroner Programmierung

Die allgemeine Reaktionsfähigkeit Ihrer Anwendung kann verbessert und Leistungsengpässe können oft vermieden werden, indem asynchrone Programmierung verwendet wird. Das [aufgabenbasierte asynchrone Muster](/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap) ist in .NET das empfohlene Entwurfsmuster für asynchrone Vorgänge. Die falsche Verwendung von aufgabenbasierten asynchronen Mustern kann jedoch zu leistungsschwachen Anwendungen führen. Daher sollten die folgenden Richtlinien bei Verwendung von aufgabenbasierten asynchronen Mustern beachtet werden.

### <a name="fundamentals"></a>Grundlagen

- Machen Sie sich mit dem Aufgabenlebenszyklus vertraut, der von der `TaskStatus`-Enumeration dargestellt wird. Weitere Informationen finden Sie unter [The meaning of TaskStatus (Die Bedeutung von TaskStatus)](https://devblogs.microsoft.com/pfxteam/the-meaning-of-taskstatus/) und [Aufgabenstatus](/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap#task-status).
- Verwenden Sie die `Task.WhenAll`-Methode, um asynchron auf den Abschluss mehrerer asynchroner Vorgänge zu warten, anstatt für mehrere asynchrone Vorgänge `await` einzeln zu verwenden. Weitere Informationen finden Sie unter [Task.WhenAll](/dotnet/standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern#taskwhenall).
- Verwenden Sie die `Task.WhenAny`-Methode, um asynchron auf den Abschluss mehrerer asynchroner Vorgänge zu warten. Weitere Informationen finden Sie unter [Task.WhenAny](/dotnet/standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern#taskwhenall).
- Verwenden Sie die `Task.Delay`-Methode, um ein `Task`-Objekt zu erstellen, das nach dem angegebenen Zeitpunkt abgeschlossen wird. Dies ist beispielsweise beim Abrufen von Daten und Verzögern von Benutzereingaben für einen vordefinierten Zeitraum nützlich. Weitere Informationen finden Sie unter [Task.Delay](/dotnet/standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern#taskdelay).
- Führen Sie rechenintensive, synchrone CPU-Vorgänge auf dem Threadpool mit der `Task.Run`-Methode aus. Diese Methode ist eine Verknüpfung für die `TaskFactory.StartNew`-Methode mit den optimalen Argumenten. Weitere Informationen finden Sie unter [Task.Run](/dotnet/standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern#taskrun).
- Vermeiden Sie es, asynchrone Konstruktoren zu erstellen. Verwenden Sie stattdessen Lebenszyklusereignisse oder separate Initialisierungslogik, um `await` ordnungsgemäß für beliebige Initialisierungen auszuführen. Weitere Informationen finden Sie unter [Asynchrone Konstruktoren](https://blog.stephencleary.com/2013/01/async-oop-2-constructors.html) auf blog.stephencleary.com.
- Verwenden Sie das verzögerte Aufgabenmuster, um das Warten auf asynchrone Vorgänge beim Anwendungsstart zu vermeiden. Weitere Informationen finden Sie unter [AsyncLazy](https://devblogs.microsoft.com/pfxteam/asynclazyt/).
- Erstellen Sie einen Aufgabenwrapper für vorhandene asynchrone Vorgänge, die keine aufgabenbasierten asynchronen Muster verwenden, indem Sie `TaskCompletionSource<T>`-Objekte erstellen. Diese Objekte erhalten die Vorteile der `Task`-Programmierbarkeit und ermöglichen es Ihnen, die Lebensdauer und den Abschluss der zugeordneten `Task`-Elemente zu steuern. Weitere Informationen finden Sie unter [Das Wesen von TaskCompletionSource](https://devblogs.microsoft.com/pfxteam/the-nature-of-taskcompletionsourcetresult/).
 
- Geben Sie ein `Task`-Objekt zurück, anstatt ein erwartetes `Task`-Objekt zurückzugeben, wenn das Ergebnis eines asynchronen Vorgangs nicht verarbeitet werden muss. Aufgrund weniger Kontextwechsel ist dies leistungsfähiger.
- Verwenden Sie die TPL-Datenflussbibliothek (Task Parallel Library) in Szenarios wie der Verarbeitung von Daten, sobald sie verfügbar sind, oder wenn Sie über mehrere Vorgänge verfügen, die asynchron miteinander kommunizieren müssen. Weitere Informationen finden Sie unter [Datenfluss (Task Parallel Library)](/dotnet/standard/parallel-programming/dataflow-task-parallel-library).

### <a name="ui"></a>UI

- Rufen Sie eine asynchrone Version einer API auf, wenn diese verfügbar ist. So wird der UI-Thread nicht blockiert und die Benutzerfreundlichkeit der Anwendung optimiert.
- Aktualisieren Sie Benutzeroberflächenelemente mit Daten aus asynchronen Vorgängen auf dem Benutzeroberflächenthread, um die Auslösung von Ausnahmen zu vermeiden. Updates der `ListView.ItemsSource`-Eigenschaft werden jedoch automatisch an den Benutzeroberflächenthread gemarshallt. Informationen dazu, wie bestimmt wird, ob Code auf dem Benutzeroberflächenthread ausgeführt wird, finden Sie unter [Xamarin.Essentials: MainThread](~/essentials/main-thread.md?content=xamarin/xamarin-forms).

    > [!IMPORTANT]
    > Alle Steuerelementeigenschaften, die per Datenbindung aktualisiert werden, werden automatisch an den Benutzeroberflächenthread gemarshallt.

### <a name="error-handling"></a>Fehlerbehandlung

- Erfahren Sie mehr über die asynchrone Ausnahmebehandlung. Nicht behandelte Ausnahmen, die von asynchron ausgeführtem Code ausgelöst werden, werden an den aufrufenden Thread zurückgegeben. Bestimmte Szenarios sind hiervon ausgenommen. Weitere Informationen finden Sie unter [Ausnahmebehandlung (Task Parallel Library)](/dotnet/standard/parallel-programming/exception-handling-task-parallel-library).
- Vermeiden Sie es, `async void`-Methoden zu erstellen. Erstellen Sie stattdessen `async Task`-Methoden. Diese Methoden erleichtern die Fehlerbehandlung, Erstellbarkeit und Testbarkeit. Eine Ausnahme von dieser Richtlinie bilden asynchrone Ereignishandler, die `void` zurückgeben müssen. Weitere Informationen finden Sie unter [Vermeiden von asynchronen Voids](/archive/msdn-magazine/2013/march/async-await-best-practices-in-asynchronous-programming#avoid-async-void).
- Kombinieren Sie blockierenden und asynchronen Code nicht, indem Sie die Methoden `Task.Wait`, `Task.Result` oder `GetAwaiter().GetResult` aufrufen, da dies zu Deadlocks führen kann. Wenn Sie jedoch gegen diese Führungslinie verstoßen müssen, besteht der empfohlene Ansatz darin, die `GetAwaiter().GetResult`-Methode aufzurufen, da Aufgabenausnahmen dabei beibehalten werden. Weitere Informationen finden Sie unter [Durchgehend asynchron](/archive/msdn-magazine/2013/march/async-await-best-practices-in-asynchronous-programming#async-all-the-way) und [Behandlung von Aufgabenausnahmen in .NET 4.5](https://devblogs.microsoft.com/pfxteam/task-exception-handling-in-net-4-5/).
- Verwenden Sie nach Möglichkeit die `ConfigureAwait`-Methode, um kontextfreien Code zu erstellen. Kontextfreier Code führt zu einer besseren Leistung bei mobilen Anwendungen und ist eine hilfreiche Methode zum Vermeiden von Deadlocks beim Arbeiten mit einer teilweise asynchronen Codebasis. Weitere Informationen finden Sie unter [Konfigurieren des Kontexts](/archive/msdn-magazine/2013/march/async-await-best-practices-in-asynchronous-programming#configure-context).
- Verwenden Sie *Fortsetzungsaufgaben* für Funktionalitäten wie das Verarbeiten von Ausnahmen, die vom vorherigen asynchronen Vorgang ausgelöst wurden, und brechen Sie eine Fortsetzung ab, bevor sie beginnt oder während sie ausgeführt wird. Weitere Informationen finden Sie unter [Verketten von Aufgaben mithilfe von kontinuierlichen Aufgaben](/dotnet/standard/parallel-programming/chaining-tasks-by-using-continuation-tasks).
- Verwenden Sie eine asynchrone `ICommand`-Implementierung, wenn asynchrone Vorgänge über `ICommand` aufgerufen werden. Dadurch wird sichergestellt, dass alle Ausnahmen in der asynchronen Befehlslogik behandelt werden können. Weitere Informationen finden Sie unter [Asynchrone Programmierung: Muster für asynchrone MVVM-Anwendungen: Befehle](/archive/msdn-magazine/2014/april/async-programming-patterns-for-asynchronous-mvvm-applications-commands).

## <a name="choose-a-dependency-injection-container-carefully"></a>Sorgfältiges Auswählen des Containers für die Abhängigkeitsinjektion

Abhängigkeitsinjektionscontainer stellen zusätzliche Leistungseinschränkungen in mobilen Anwendungen dar. Das Registrieren und Auflösen von Typen mit einem Container wirkt sich negativ auf die Leistung aus, da der Container Reflektion zum Erstellen der einzelnen Typen verwendet, insbesondere dann, wenn Abhängigkeiten für jede Seitennavigation in der App rekonstruiert werden müssen. Wenn zahlreiche oder tiefe Abhängigkeiten vorhanden sind, können die Kosten für die Erstellung erheblich zunehmen. Außerdem kann die Typregistrierung, die normalerweise während des Anwendungsstarts auftritt, abhängig vom verwendeten Container eine spürbare Auswirkung auf die Startzeit haben.

Als Alternative kann die Abhängigkeitsinjektion durch manuelle Implementierung mithilfe von Factorys leistungsstärker werden.

## <a name="create-shell-applications"></a>Erstellen von Shellanwendungen

Xamarin.Forms-Shellanwendungen bieten eine Benutzeroberfläche mit zahlreichen Optionen für die Navigation, basierend auf Flyouts und Registerkarten. Wenn Ihre Anwendungsbenutzeroberfläche mit der Shell implementiert werden kann, ist es vorteilhaft, so vorzugehen. Shellanwendungen helfen, ein schlechtes Starterlebnis zu vermeiden, da Seiten bei Bedarf als Reaktion auf die Navigation erstellt werden und nicht beim Start der Anwendung, wie bei Anwendungen, die eine [„TabbedPage“](xref:Xamarin.Forms.TabbedPage) verwenden. Weitere Informationen finden Sie unter [Xamarin.Forms-Shell](~/xamarin-forms/app-fundamentals/shell/index.md).

## <a name="use-collectionview-instead-of-listview"></a>Verwenden von CollectionView anstelle von ListView

[`CollectionView`](xref:Xamarin.Forms.CollectionView) ist eine Ansicht für die Darstellung von Listen mit Daten mit unterschiedlichen Layoutspezifikationen. Sie bietet eine flexiblere und leistungsfähigere Alternative zu [`ListView`](xref:Xamarin.Forms.ListView). Weitere Informationen finden Sie unter [Xamarin.Forms: CollectionView](~/xamarin-forms/user-interface/collectionview/index.md).

## <a name="optimize-listview-performance"></a>Optimieren der ListView-Leistung

Bei Verwendung einer [`ListView`](xref:Xamarin.Forms.ListView) müssen einige Benutzeroberflächen optimiert werden:

- **Initialisierung**: Das Zeitintervall beginnt mit der Erstellung des Steuerelements und endet, wenn die Elemente auf dem Bildschirm angezeigt werden.
- **Bildlauf**: Die Möglichkeit, einen Bildlauf durch die Liste durchzuführen, und die Sicherstellung, dass die Benutzeroberfläche nicht hinter Fingerbewegungen zurückbleibt.
- **Interaktion** für das Hinzufügen, Löschen und Auswählen von Elementen.

Für das [`ListView`](xref:Xamarin.Forms.ListView)-Steuerelement ist eine Anwendung erforderlich, mit der Daten- und Zellenvorlagen bereitgestellt werden können. Wie dies erreicht wird, hat großen Einfluss auf die Leistung des Steuerelements. Weitere Informationen finden Sie unter [ListView Performance](~/xamarin-forms/user-interface/listview/performance.md) (ListView-Leistung).

## <a name="optimize-image-resources"></a>Optimieren von Bildressourcen

Das Anzeigen von Bildressourcen kann den Speicherbedarf der Anwendung erheblich erhöhen. Sie sollten daher nur erstellt werden, wenn dies erforderlich ist. Und sie sollten freigegeben werden, sobald die Anwendung sie nicht mehr benötigt. Wenn beispielsweise eine Anwendung ein Bild anzeigt, indem sie die zugehörigen Daten aus einem Datenstrom liest, müssen Sie sicherstellen, dass dieser Datenstrom nur erstellt wird, wenn dies erforderlich ist, und dass er freigegeben wird, sobald er nicht mehr benötigt wird. Dies kann erreicht werden, indem der Datenstrom bei Erstellung der Seite erstellt wird, oder bei Auslösung des [`Page.Appearing`](xref:Xamarin.Forms.Page.Appearing)-Ereignisses, und der Datenstrom anschließend bei Auslösung des [`Page.Disappearing`](xref:Xamarin.Forms.Page.Disappearing)-Ereignisses verworfen wird.

Wenn Sie ein Bild für die Anzeige mit der [`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri))-Methode herunterladen, müssen Sie das heruntergeladene Bild zwischenspeichern, indem Sie sicherstellen, dass die Eigenschaft [`UriImageSource.CachingEnabled`](xref:Xamarin.Forms.UriImageSource.CachingEnabled) auf `true` festgelegt ist. Weitere Informationen finden Sie unter [Arbeiten mit Bildern](~/xamarin-forms/user-interface/images.md).

Weitere Informationen finden Sie unter [Optimieren von Bildressourcen](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages).

## <a name="reduce-the-visual-tree-size"></a>Verringern der Größe der visuellen Struktur

Durch eine Reduzierung der Anzahl von Elementen auf einer Seite wird der Seitenrenderer schneller. Es gibt zwei Haupttechniken, um dies zu erreichen. Bei der ersten Technik werden Elemente ausgeblendet, die nicht sichtbar sind. Die Eigenschaft [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) der einzelnen Elemente bestimmt, ob die Elemente Teil der visuellen Struktur sein sollten. Daher sollten Sie das Element entfernen oder die zugehörige Eigenschaft `IsVisible` auf `false` festlegen, wenn ein Element nicht sichtbar ist, da es ausgeblendet wird.

Bei der zweiten Technik werden unnötige Elemente entfernt. Im folgenden Codebeispiel wird ein Seitenlayout dargestellt, das mehrere [`Label`](xref:Xamarin.Forms.Label)-Objekte enthält:

```xaml
<StackLayout>
    <StackLayout Padding="20,20,0,0">
        <Label Text="Hello" />
    </StackLayout>
    <StackLayout Padding="20,20,0,0">
        <Label Text="Welcome to the App!" />
    </StackLayout>
    <StackLayout Padding="20,20,0,0">
        <Label Text="Downloading Data..." />
    </StackLayout>
</StackLayout>
```

Dasselbe Seitenlayout kann wie im folgenden Codebeispiel gezeigt mit einer reduzierten Elementanzahl verwaltet werden:

```xaml
<StackLayout Padding="20,35,20,20" Spacing="25">
  <Label Text="Hello" />
  <Label Text="Welcome to the App!" />
  <Label Text="Downloading Data..." />
</StackLayout>
```

## <a name="reduce-the-application-resource-dictionary-size"></a>Verringern der Größe des Ressourcenverzeichnisses der Anwendung

Alle Ressourcen, die in der gesamten Anwendung verwendet werden, sollten im Ressourcenverzeichnis der Anwendung gespeichert werden, damit eine Duplizierung verhindert wird. Dadurch kann der Umfang des XAML-Codes reduziert werden, der in der gesamten Anwendung analysiert werden muss. Das folgende Codebeispiel zeigt die `HeadingLabelStyle`-Ressource, die in der gesamten Anwendung verwendet wird und daher im Ressourcenverzeichnis der Anwendung definiert ist:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Resources.App">
     <Application.Resources>
         <ResourceDictionary>
            <Style x:Key="HeadingLabelStyle" TargetType="Label">
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="TextColor" Value="Red" />
            </Style>
         </ResourceDictionary>
     </Application.Resources>
</Application>
```

XAML-Code, der für eine Seite spezifisch ist, sollte jedoch nicht im Ressourcenverzeichnis der Anwendung enthalten sein, da die Ressourcen dann beim Starten der Anwendung analysiert werden und nicht, wenn dies auf einer Seite erforderlich ist. Wenn eine Ressource von einer anderen Seite als der Startseite verwendet wird, sollte sie in das Ressourcenverzeichnis dieser Seite aufgenommen werden und folglich dabei helfen, den XAML-Code zu reduzieren, der beim Starten der Anwendung analysiert wird. Das folgende Codebeispiel zeigt die `HeadingLabelStyle`-Ressource, die sich nur auf einer einzelnen Seite befindet und daher im Ressourcenverzeichnis der Seite definiert ist:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Test.HomePage"
             Padding="0,20,0,0">
    <ContentPage.Resources>
        <ResourceDictionary>
          <Style x:Key="HeadingLabelStyle" TargetType="Label">
              <Setter Property="HorizontalOptions" Value="Center" />
              <Setter Property="FontSize" Value="Large" />
              <Setter Property="TextColor" Value="Red" />
          </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Weitere Informationen zu Anwendungsressourcen finden Sie unter [XAML-Stile](~/xamarin-forms/user-interface/styles/xaml/index.md).

## <a name="use-the-custom-renderer-pattern"></a>Verwenden des benutzerdefinierten Renderermusters

Die meisten Xamarin.Forms-Rendererklassen stellen die `OnElementChanged`-Methode bereit, die bei der Erstellung eines benutzerdefinierten Xamarin.Forms-Steuerelements aufgerufen wird, um das entsprechende native Steuerelement zu rendern. Passen Sie Rendererklassen in jedem Plattformprojekt an, und setzen Sie diese Methode anschließend außer Kraft, um das native Steuerelement zu instanziieren und anzupassen. Mit der `SetNativeControl`-Methode wird das native Steuerelement instanziiert und die Steuerelementreferenz der Eigenschaft `Control` zugewiesen.

In einigen Fällen kann die `OnElementChanged`-Methode mehrmals aufgerufen werden. Daher müssen Sie bei der Instanziierung eines nativen Steuerelements sorgfältig vorgehen, um Arbeitsspeicherverluste zu vermeiden, die Auswirkungen auf die Leistung haben können. Im folgenden Codebeispiel ist der Ansatz zu sehen, der beim Instanziieren eines neuen nativen Steuerelements in einem benutzerdefinierten Renderer verwendet werden soll:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null)
  {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null)
  {
    if (Control == null)
    {
      // Instantiate the native control with the SetNativeControl method
    }
    // Configure the control and subscribe to event handlers
  }
}
```

Ein neues natives Steuerelement sollte nur einmal instanziiert werden, wenn der Wert der Eigenschaft `Control``null` lautet. Ein Steuerelement sollte außerdem nur dann erstellt, konfiguriert und vom Ereignishandler abonniert werden, wenn der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt wird. Gleichermaßen sollte das Abonnement für Ereignishandler nur dann gekündigt werden, wenn sich das Element ändert, an das der Renderer angefügt wurde. Mit diesem Ansatz kann ein gut funktionierender benutzerdefinierter Renderer erstellt werden, der nicht durch Speicherverluste beeinträchtigt wird.

> [!IMPORTANT]
> Die `SetNativeControl`-Methode sollte nur aufgerufen werden, wenn die `e.NewElement`-Eigenschaft nicht `null` ist und die `Control`-Eigenschaft `null` ist.

Weitere Informationen zu benutzerdefinierten Renderern finden Sie unter [Customizing Controls on Each Platform (Anpassen von Steuerelementen auf den einzelnen Plattformen)](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

## <a name="related-links"></a>Verwandte Links

- [Plattformübergreifende Leistung](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Kompilieren von XAML](~/xamarin-forms/xaml/xamlc.md)
- [Kompilierte Bindungen](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)
- [Schnelle Renderer](~/xamarin-forms/internals/fast-renderers.md)
- [Layoutkomprimierung](~/xamarin-forms/user-interface/layouts/layout-compression.md)
- [Xamarin.Forms-Shell](~/xamarin-forms/app-fundamentals/shell/index.md)
- [Xamarin.Forms: CollectionView](~/xamarin-forms/user-interface/collectionview/index.md)
- [ListView-Leistung](~/xamarin-forms/user-interface/listview/performance.md)
- [Optimieren von Bildressourcen](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages)
- [XAML-Stile](~/xamarin-forms/user-interface/styles/xaml/index.md)
- [Anpassen von Steuerelementen auf jeder Plattform](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
