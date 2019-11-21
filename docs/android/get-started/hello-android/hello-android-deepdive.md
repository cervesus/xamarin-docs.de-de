---
title: 'Hello, Android: Ausführliche Erläuterungen'
description: In diesem zweiteiligen Leitfaden erstellen Sie Ihre erste Xamarin.Android-Anwendung. Außerdem entwickeln Sie ein Verständnis für die grundlegenden Aspekte der Entwicklung von Android-Anwendungen mit Xamarin. Währenddessen werden Ihnen die Tools, Konzepte und Schritte vorgestellt, die zum Erstellen und Bereitstellen einer Xamarin.Android-Anwendung erforderlich sind.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: EF0E110B-20EA-43F6-9476-1A0F41AFD298
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 10/05/2018
ms.openlocfilehash: ee72c51611503f92e7ede3a01a7918780652935c
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028008"
---
# <a name="hello-android-deep-dive"></a>Hello, Android: Ausführliche Erläuterungen

_In diesem zweiteiligen Leitfaden erstellen Sie Ihre erste Xamarin.Android-Anwendung. Außerdem entwickeln Sie ein Verständnis für die grundlegenden Aspekte der Entwicklung von Android-Anwendungen mit Xamarin. Währenddessen werden Ihnen die Tools, Konzepte und Schritte vorgestellt, die zum Erstellen und Bereitstellen einer Xamarin.Android-Anwendung erforderlich sind._

Im Artikel [Hello, Android Quickstart (Hallo, Android: Schnellstart)](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-quickstart.md) haben Sie Ihre erste Xamarin.Android-Anwendung erstellt und ausgeführt. Nun ist es an der Zeit, ein tieferes Verständnis für die Funktionsweise von Android-Anwendungen zu entwickeln, sodass Sie komplexere Programme erstellen können. In diesem Leitfaden werden die Schritte überprüft, die Sie in der exemplarischen Vorgehensweise zu „Hallo, Android“ vorgenommen haben, sodass Sie ein Verständnis für die bisher ausgeführten Schritte und grundlegende Kenntnisse über die Entwicklung von Android-Anwendungen entwickeln können.

Dieser Leitfaden befasst sich mit folgenden Themen:

::: zone pivot="windows"

- **Einführung in Visual Studio**: Einführung in Visual Studio und das Erstellen einer neuen Xamarin.Android-Anwendung

- **Aufbau einer Xamarin.Android-Anwendung**: Überblick über die wesentlichen Bestandteile einer Xamarin.Android-Anwendung

- **Grundlagen zu Apps und Architektur**: Einführung in Aktivitäten, das Android-Manifest und die Grundlagen der Android-Entwicklung

- **Benutzeroberfläche (User Interface, UI)** : Erstellen von Benutzeroberflächen mit dem Android Designer

- **Aktivitäten und Aktivitätslebenszyklus**: Eine Einführung in den Aktivitätslebenszyklus und das Schreiben der Benutzeroberfläche in Code

- **Tests, Bereitstellung und Vollendung**: Fertigstellen der Anwendung mit Ratschlägen zu Tests, Bereitstellung, Erstellen von Grafiken usw.

::: zone-end
::: zone pivot="macos"

- **Einführung in Visual Studio für Mac**: Einführung in Visual Studio für Mac und das Erstellen einer neuen Xamarin.Android-Anwendung

- **Aufbau einer Xamarin.Android-Anwendung**: Überblick über die wesentlichen Bestandteile einer Xamarin.Android-Anwendung

- **Grundlagen zu Apps und Architektur**: Einführung in Aktivitäten, das Android-Manifest und die Grundlagen der Android-Entwicklung

- **Benutzeroberfläche (User Interface, UI)** : Erstellen von Benutzeroberflächen mit dem Android Designer

- **Aktivitäten und Aktivitätslebenszyklus**: Eine Einführung in den Aktivitätslebenszyklus und das Schreiben der Benutzeroberfläche in Code

- **Tests, Bereitstellung und Vollendung**: Fertigstellen der Anwendung mit Ratschlägen zu Tests, Bereitstellung, Erstellen von Grafiken usw.

::: zone-end

Dieser Leitfaden unterstützt Sie dabei, die Fertigkeiten und Kenntnisse zu entwickeln, die zum Erstellen einer Android-Anwendung für einen Bildschirm erforderlich sind. Nachdem Sie diesen durchgearbeitet haben, sollten Sie die verschiedenen Bestandteile einer Xamarin.Android-Anwendung und deren Zusammenwirken verstehen können.

::: zone pivot="windows"

## <a name="introduction-to-visual-studio"></a>Einführung in Visual Studio

Visual Studio ist eine leistungsstarke IDE von Microsoft. Sie enthält z.B. einen vollständig integrierten visuellen Designer, einen Text-Editor mit Refactoringtools, einen Assembly-Browser und Quellcodeintegration. In diesem Leitfaden erfahren Sie mehr über die Verwendung einiger grundlegender Funktionen von Visual Studio mit dem Xamarin-Plug-In.

In Visual Studio wird Code in _Projektmappen_ und _Projekten_ organisiert. Eine Projektmappe ist ein Container, der mindestens ein Projekt enthält. Ein Projekt kann beispielsweise eine Anwendung (z.B. für iOS oder Android), eine unterstützende Bibliothek oder eine Testanwendung sein. In der App **Phoneword** haben Sie mithilfe der Vorlage **Android Application** ein neues Android-Projekt zur **Phoneword**-Projektmappe hinzugefügt, die im Leitfaden [Hello, Android (Hallo, Android)](~/android/get-started/hello-android/hello-android-quickstart.md) erstellt wurde.

::: zone-end
::: zone pivot="macos"

## <a name="introduction-to-visual-studio-for-mac"></a>Einführung in Visual Studio für Mac

Visual Studio für Mac ist eine kostenlose Open-Source-IDE, die Visual Studio ähnelt. Sie enthält z.B. einen vollständig integrierten visuellen Designer, einen Text-Editor mit Refactoringtools, einen Assembly-Browser und Quellcodeintegration. In diesem Handbuch erfahren Sie mehr über die Verwendung einiger grundlegender Funktionen von Visual Studio für Mac. Wenn Sie mit Visual Studio für Mac noch nicht vertraut sind, sollten Sie für weitere Informationen die [Einführung in Visual Studio für Mac](https://docs.microsoft.com/visualstudio/mac/) aufrufen.

Die Codeorganisation in Visual Studio für Mac baut auf Visual Studio auf und gliedert sich in _Projektmappen_ und _Projekte_. Eine Projektmappe ist ein Container, der mindestens ein Projekt enthält. Ein Projekt kann beispielsweise eine Anwendung (z.B. für iOS oder Android), eine unterstützende Bibliothek oder eine Testanwendung sein. In der App **Phoneword** haben Sie mithilfe der Vorlage **Android Application** ein neues Android-Projekt zur **Phoneword**-Projektmappe hinzugefügt, die im Leitfaden [Hello, Android (Hallo, Android)](~/android/get-started/hello-android/hello-android-quickstart.md) erstellt wurde.

::: zone-end

<a name="anatomy" />

## <a name="anatomy-of-a-xamarinandroid-application"></a>Aufbau einer Xamarin.Android-Anwendung

::: zone pivot="windows"

Der folgende Screenshot listet die Inhalte der Projektmappe auf. Dies ist der Projektmappen-Explorer, der die Verzeichnisstruktur und alle Dateien enthält, die der Projektmappe zugeordnet sind:

[![Projektmappen-Explorer](hello-android-deepdive-images/vs/02-solution-structure-sml.png)](hello-android-deepdive-images/vs/02-solution-structure.png#lightbox)

::: zone-end
::: zone pivot="macos"

Der folgende Screenshot listet die Inhalte der Projektmappe auf. Dies ist das Lösungspad, das die Verzeichnisstruktur und alle Dateien enthält, die der Projektmappe zugeordnet sind:

[![Lösungspad](hello-android-deepdive-images/xs/02-solution-structure-sml.png)](hello-android-deepdive-images/xs/02-solution-structure.png#lightbox)

::: zone-end

Eine Projektmappe namens **Phoneword** wurde erstellt. In dieser wurde das Android-Projekt **Phoneword** platziert.

Betrachten Sie die Elemente innerhalb des Projekts, um jeden Ordner und seinen Zweck zu sehen:

- **Eigenschaften**: Enthält die Datei [AndroidManifest.xml](~/android/platform/android-manifest.md), die alle Anforderungen für die Xamarin.Android-Anwendung beschreibt, einschließlich des Namens, der Versionsnummer und der Berechtigungen. Der Ordner **Eigenschaften** enthält ebenfalls [AssemblyInfo.cs](xref:Microsoft.VisualBasic.ApplicationServices.AssemblyInfo), eine Metadatendatei der .NET-Assembly. Es wird empfohlen, diese Datei mit grundlegenden Informationen zu Ihrer Anwendung zu füllen.

- **Verweise**: Enthält die Assemblys, die zum Erstellen und Ausführen der Anwendung erforderlich sind. Wenn Sie das Verzeichnis „Verweise“ erweitern, werden Ihnen die Verweise zu .NET-Assemblys angezeigt, z.B. [System](xref:System), System.Core und [System.xml](xref:System.Xml), sowie ein Verweis zur Mono.Android-Assembly von Xamarin.

- **Objekte**: Enthält die Dateien, die die Anwendung für die Ausführung benötigt, einschließlich Schriftarten, lokalen Datendateien und Textdateien. Auf die hier enthaltenen Dateien kann über die generierte `Assets`-Klasse zugegriffen werden. Weitere Informationen zu Android-Objekten finden Sie im Xamarin-Leitfaden[Using Android Assets (Verwenden von Android-Objekten)](~/android/app-fundamentals/resources-in-android/android-assets.md).

- **Ressourcen**: Enthält Anwendungsressourcen wie Zeichenfolgen, Bilder und Layouts. Sie können auf diese Ressourcen im Code über die generierte `Resource`-Klasse zugreifen. Der Leitfaden [Android Resources (Android-Ressourcen)](~/android/app-fundamentals/resources-in-android/index.md) enthält weitere Informationen über das Verzeichnis **Ressourcen**. Die Anwendungsvorlage enthält ebenfalls einen kurzen Leitfaden zu Ressourcen in der Datei **AboutResources.txt**.

### <a name="resources"></a>Ressourcen

Das Verzeichnis **Ressourcen** enthält drei Ordner namens **drawable**, **layout**, **mipmap** und **values** sowie eine Datei namens **Resource.designer.cs**.

Diese Elemente werden in der folgenden Tabelle zusammengefasst:

- **drawable**: Die Verzeichnisse „drawable“ enthalten [drawable resources (zeichenbare Ressourcen)](https://developer.android.com/guide/topics/resources/drawable-resource.html) wie Bilder und Bitmaps.

- **mipmap** &ndash; Das mipmap-Verzeichnis enthält zeichenbare Dateien für verschiedene Dichten von Startprogrammsymbolen. In der Standardvorlage enthält das Verzeichnis „drawable“ die Anwendungssymboldatei, **Icon.png**.

::: zone pivot="windows"

- **layout**: Das Verzeichnis „layout“ enthält _Android Designer-Dateien_ (AXML), die die Benutzeroberfläche für jeden Bildschirm oder jede Aktivität definieren. Die Vorlage erstellt ein Standardlayout namens **activity_main.axml**.

::: zone-end
::: zone pivot="macos"

- **layout**: Das Verzeichnis „layout“ enthält _Android Designer-Dateien_ (AXML), die die Benutzeroberfläche für jeden Bildschirm oder jede Aktivität definieren. Die Vorlage erstellt ein Standardlayout namens **Main.axml**.

::: zone-end

- **values**: Dieses Verzeichnis enthält XML-Dateien, die einfache Werte wie Zeichenfolgen, Ganzzahlen und Farben speichern. Die Vorlage erstellt eine Datei namens **Strings.xml**, um Zeichenfolgenwerte zu speichern.

- **Resource.designer.cs**: Diese Datei, die auch als `Resource`-Klasse bekannt ist, ist eine partielle Klasse, die die eindeutigen IDs enthält, die jeder Ressource zugewiesen wurden. Diese wird automatisch von den Xamarin.Android-Tools erstellt und kann bei Bedarf erneut generiert werden. Diese Datei sollte nicht manuell bearbeitet werden, da Xamarin.Android alle manuellen Änderungen überschreibt.

## <a name="app-fundamentals-and-architecture-basics"></a>Grundlagen zu Apps und Architektur

Android-Anwendungen verfügen nicht über einen einzigen Einstiegspunkt. Das bedeutet, dass es keine einzelne Codezeile in der Anwendung gibt, die das Betriebssystem aufruft, um die Anwendung zu starten. Stattdessen startet eine Anwendung, wenn Android eine ihrer Klassen instanziiert. Während dieses Zeitraums lädt Android den gesamten Prozess der Anwendung in den Arbeitsspeicher.

Diese einzigartige Funktion von Android kann äußerst hilfreich beim Entwerfen von komplizierten Anwendungen oder beim Interagieren mit dem Android-Betriebssystem sein. Diese Optionen machen Android jedoch im Umgang mit einem grundlegenden Szenario wie der Anwendung **Phoneword** komplex. Aus diesem Grund ist die Erläuterung der Android-Architektur in zwei Teile aufgeteilt. Dieser Leitfaden analysiert eine Anwendung, die den häufigsten Einstiegspunkt für eine Android-App verwendet: den ersten Bildschirm. In [Hello, Android Multiscreen („Hallo, Android“-Multiscreen)](~/android/get-started/hello-android-multiscreen/index.md) wird die gesamte Komplexität der Android-Architektur erläutert, da verschiedene Möglichkeiten behandelt werden, eine Anwendung zu starten.

### <a name="phoneword-scenario---starting-with-an-activity"></a>Phoneword-Szenario: Starten mit einer Aktivität

Wenn Sie die Anwendung **Phoneword** zum ersten Mal in einem Emulator oder auf einem Gerät starten, erstellt das Betriebssystem die erste *Aktivität*. Eine Aktivität ist eine spezielle Android-Klasse, die einem einzelnen Anwendungsbildschirm entspricht und für das Zeichnen und Steuern der Benutzeroberfläche zuständig ist. Wenn Android die erste Aktivität einer Anwendung erstellt, wird die gesamte Anwendung geladen:

[![Laden der Aktivität](hello-android-deepdive-images/01-activity-load-sml.png)](hello-android-deepdive-images/01-activity-load.png#lightbox)

Da es keinen linearen Ablauf in einer Android-Anwendung gibt (Sie können die Anwendung von mehreren Punkten aus starten), bietet Android eine einzigartige Methode, um zu verfolgen, aus welchen Klassen und Dateien sich eine Anwendung zusammensetzt. Im Beispiel **Phoneword** sind alle Teile, die die Anwendung bilden, mit einer speziellen XML-Datei registriert, die als **Android-Manifest** bezeichnet wird. Die Rolle des **Android-Manifests** ist das Verfolgen der Inhalte, Eigenschaften und Berechtigungen einer Anwendung und das Offenlegen derselben für das Android-Betriebssystem. Sie können sich die Anwendung **Phoneword** wie im folgenden Diagramm dargestellt als einzelne Aktivität (Bildschirm) und Sammlung von Ressourcen und Hilfsprogrammdateien vorstellen, die von der Android-Manifestdatei miteinander verknüpft werden:

[![Hilfsprogramme für Ressourcen](hello-android-deepdive-images/02-resources-helpers-sml.png)](hello-android-deepdive-images/02-resources-helpers.png#lightbox)

In den nächsten Abschnitten werden die Beziehungen zwischen den verschiedenen Teilen der Anwendung **Phoneword** erläutert. Dadurch sollten Sie das oben stehende Diagramm besser nachvollziehen können. Zunächst werden die Benutzeroberfläche sowie die Android Designer- und Layoutdateien erläutert.

## <a name="user-interface"></a>Benutzeroberfläche

> [!TIP]
> Neuere Releases von Visual Studio unterstützen das Öffnen von XML-Dateien in Android Designer.
>
> Sowohl AXML- als auch XML-Dateien werden in Android Designer unterstützt.

::: zone pivot="windows"

**activity_main.axml** ist die Layoutdatei der Benutzeroberfläche für den ersten Bildschirm in der Anwendung. Die Endung „.axml“ gibt an, dass es sich dabei um eine Android Designer-Datei handelt (AXML steht für *Android XML*). Der Name *Main* ist aus Sicht von Android willkürlich &ndash; die Layoutdatei kann auch anders benannt werden. Wenn Sie **activity_main.axml** in der IDE öffnen, wird der visuelle Editor (*Android Designer*) für Android-Layoutdateien aufgerufen:

[![Android Designer](hello-android-deepdive-images/vs/03-android-designer-sml.png "Android Designer")](hello-android-deepdive-images/vs/03-android-designer.png#lightbox)

In der App **Phoneword** ist die ID von **TranslateButton** auf `@+id/TranslateButton` festgelegt:

[![Einstellung der ID von TranslateButton](hello-android-deepdive-images/vs/04-translatebutton-sml.png "Einstellung der ID von TranslateButton")](hello-android-deepdive-images/vs/04-translatebutton.png#lightbox)

::: zone-end
::: zone pivot="macos"

**Main.axml** ist die Layoutdatei der Benutzeroberfläche für den ersten Bildschirm in der Anwendung. Die Endung „.axml“ gibt an, dass es sich dabei um eine Android Designer-Datei handelt (AXML steht für *Android XML*). Der Name *Main* ist aus Sicht von Android willkürlich &ndash; die Layoutdatei kann auch anders benannt werden. Wenn Sie **Main.axml** in der IDE öffnen, wird der visuelle Editor (*Android Designer*) für Android-Layoutdateien aufgerufen:

[![Android Designer](hello-android-deepdive-images/xs/03-android-designer-sml.png)](hello-android-deepdive-images/xs/03-android-designer.png#lightbox)

In der App **Phoneword** ist die ID von **TranslateButton** auf `@+id/TranslateButton` festgelegt:

[![Einstellung der ID von TranslateButton](hello-android-deepdive-images/xs/04-translatebutton-sml.png)](hello-android-deepdive-images/xs/04-translatebutton.png#lightbox)

::: zone-end

Wenn Sie die Eigenschaft `id` von **TranslateButton** festlegen, ordnet der Android Designer das Steuerelement **TranslateButton** der `Resource`-Klasse zu und weist diesem eine *Ressourcen-ID* von `TranslateButton` zu. Diese Zuordnung des visuellen Steuerelements zu einer Klasse ermöglicht es, **TranslateButton** und andere Steuerelemente im App-Code zu finden und zu verwenden. Weitere Informationen dazu erhalten Sie, wenn Sie den Code teilen, der die Steuerelemente steuert. Aktuell müssen Sie nur wissen, dass die Codedarstellung eines Steuerelements im Designer über die `id`-Eigenschaft mit der visuellen Darstellung des Steuerelements verknüpft ist.

### <a name="source-view"></a>Quellansicht

Alles, was auf der Entwurfsoberfläche definiert ist, wird für Xamarin.Android in XML übersetzt. Der Android Designer stellt eine Quellansicht bereit, die die XML-Datei enthält, die mit dem virtuellen Designer generiert wurde. Sie können diese XML-Datei anzeigen, indem Sie wie im folgenden Screenshot dargestellt zum Bereich **Quelle** unten links in der Designeransicht wechseln:

::: zone pivot="windows"

[![Designer-Quellansicht](hello-android-deepdive-images/vs/05-source-view-sml.png "Designer-Quellansicht")](hello-android-deepdive-images/vs/05-source-view.png#lightbox)

::: zone-end
::: zone pivot="macos"

[![Quellansicht des Designers](hello-android-deepdive-images/xs/05-source-view-sml.png)](hello-android-deepdive-images/xs/05-source-view.png#lightbox)

::: zone-end

Dieser XML-Quellcode sollte vier Steuerelemente enthalten: zwei **TextView**-Elemente, ein **EditText**-Element und ein **Button**-Element. Weitere Informationen zum Android Designer finden Sie im Xamarin Android-Leitfaden [Designer Overview (Übersicht über den Designer)](~/android/user-interface/android-designer/index.md).

Die Tools und Konzepte hinter dem visuellen Teil der Benutzeroberfläche sind nun abgedeckt. Nun ist es an der Zeit, sich den Code anzusehen, der die Benutzeroberfläche steuert, während die Aktivitäten und der Aktivitätslebenszyklus erläutert werden.

## <a name="activities-and-the-activity-lifecycle"></a>Aktivitäten und Aktivitätslebenszyklus

Die `Activity`-Klasse enthält den Code, der die Benutzeroberfläche steuert.
Die Aktivität ist dafür zuständig, auf Benutzerinteraktionen zu reagieren und eine dynamische Benutzeroberfläche zu erstellen.
In diesem Abschnitt wird die `Activity`-Klasse eingeführt, der Aktivitätslebenszyklus erläutert und der Code analysiert, der die Benutzeroberfläche in der Anwendung **Phoneword** steuert.

### <a name="activity-class"></a>Aktivitätsklasse

Die Anwendung **Phoneword** besitzt nur einen Bildschirm (Aktivität). Die `MainActivity`-Klasse steuert den Bildschirm und befindet sich in der Datei **MainActivity.cs**. Der Name `MainActivity` hat keine besondere Bedeutung in Android. Obwohl die erste Aktivität in einer Anwendung üblicherweise `MainActivity` benannt wird, ist es für Android unerheblich, wenn diese anders benannt wird.

Wenn Sie **MainActivity.cs** öffnen, können Sie sehen, dass die `MainActivity`-Klasse eine *Unterklasse* der `Activity`-Klasse ist und dass die Aktivität mit dem Attribut [Activity](xref:Android.App.ActivityAttribute) versehen ist:

```csharp
[Activity (Label = "Phone Word", MainLauncher = true)]
public class MainActivity : Activity
{
  ...
}
```

Das `Activity`-Attribut registriert die Aktivität mit dem Android-Manifest. Dadurch weiß Android, dass diese Klasse Teil der Anwendung **Phoneword** ist, die von diesem Manifest verwaltet wird. Die `Label`-Eigenschaft legt den Text fest, der am oberen Rand des Bildschirms angezeigt wird.

Die `MainLauncher`-Eigenschaft teilt Android mit, diese Aktivität anzuzeigen, wenn die Anwendung gestartet wird. Diese Eigenschaft ist wichtig, wenn Sie wie im Leitfaden [Hello, Android Multiscreen („Hallo, Android“-Multiscreen)](~/android/get-started/hello-android-multiscreen/index.md) beschrieben weitere Aktivitäten (Bildschirme) zur Anwendung hinzufügen.

Nachdem die Grundlagen von `MainActivity` behandelt wurden, ist es Zeit für tiefere Einblicke in den Aktivitätscode durch die Einführung des _Aktivitätslebenszyklus_.

### <a name="activity-lifecycle"></a>Aktivitätslebenszyklus

In Android durchlaufen Aktivitäten abhängig von ihren Interaktionen mit dem Benutzer verschiedene Phasen eines Lebenszyklus. Aktivitäten können z.B. erstellt, gestartet und angehalten, fortgesetzt und gelöscht werden. Die `Activity`-Klasse enthält Methoden, die das System an bestimmten Punkten im Lebenszyklus des Bildschirms aufruft. Das folgende Diagramm stellt den typischen Lebenszyklus einer Aktivität sowie einige der entsprechenden Lebenszyklusmethoden dar:

[![Aktivitätslebenszyklus](hello-android-deepdive-images/04-lifecycle-sml.png)](hello-android-deepdive-images/04-lifecycle.png#lightbox)

Indem Sie die `Activity`-Lebenszyklusmethoden überschreiben, können Sie steuern, wie die Aktivität lädt, wie sie auf den Benutzer reagiert und sogar was passiert, nachdem diese auf dem Gerätebildschirm geschlossen wird. Sie können beispielsweise die Lebenszyklusmethoden im oben stehenden Diagramm überschreiben, um einige wichtige Aufgaben auszuführen:

- **OnCreate**: Erstellt Ansichten, initialisiert Variablen und führt andere vorbereitende Arbeiten aus, die erledigt werden müssen, bevor die Aktivität dem Benutzer angezeigt wird. Diese Methode wird nur einmal aufgerufen, wenn die Aktivität in den Arbeitsspeicher geladen wird. 

- **OnResume**: Führt alle Aufgaben aus, die immer dann ausgeführt werden müssen, wenn die Aktivität zum Gerätebildschirm zurückkehrt.

- **OnPause**: Führt alle Aufgaben aus, die immer dann ausgeführt werden müssen, wenn die Aktivität den Gerätebildschirm verlässt.

Wenn Sie benutzerdefinierten Code zu einer Lebenszyklusmethode in der `Activity` hinzufügen, *überschreiben* Sie dadurch die *Basisimplementierung* dieser Lebenszyklusmethode. Navigieren Sie zur bestehenden Lebenszyklusmethode (der bereits Code angefügt wurde), und erweitern Sie diese Methode mit Ihrem eigenen Code. Rufen Sie die Basisimplementierung innerhalb Ihrer Methode auf, um sicherzustellen, dass der ursprüngliche Code vor Ihrem neuen Code ausgeführt wird. Ein Beispiel dafür wird im nächsten Abschnitt dargestellt.

Der Aktivitätslebenszyklus ist ein wichtiger und komplexer Teil von Android. Wenn Sie nach dem Abschluss der Reihe _Erste Schritte_ mehr über Aktivitäten erfahren möchten, finden Sie weitere Informationen im Leitfaden [Activity Lifecycle (Aktivitätslebenszyklus)](~/android/app-fundamentals/activity-lifecycle/index.md). In diesem Leitfaden liegt der nächste Schwerpunkt auf der ersten Phase des Aktivitätslebenszyklus, `OnCreate`.

### <a name="oncreate"></a>OnCreate

Android ruft die `OnCreate`-Methode von `Activity` auf, wenn die Aktivität erstellt wird (bevor der Bildschirm dem Benutzer angezeigt wird). Sie können die `OnCreate`-Lebenszyklusmethode überschreiben, um Ansichten zu erstellen und Ihre Aktivität auf den Benutzer vorzubereiten:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);
    // Additional setup code will go here
}
```

::: zone pivot="windows"

In der **Phoneword**-App sollte in `OnCreate` zuerst die Benutzeroberfläche geladen werden, die im Android Designer erstellt wurde. Rufen Sie zum Laden der Benutzeroberfläche `SetContentView` auf, und übergeben Sie den *Ressourcenlayoutnamen* für die Layoutdatei: **activity_main.axml**. Das Layout befindet sich unter `Resource.Layout.activity_main`:

```csharp
SetContentView (Resource.Layout.activity_main);
```

Wenn `MainActivity` gestartet wird, wird eine Ansicht basierend auf den Inhalten der Datei **activity_main.axml** erstellt.

::: zone-end
::: zone pivot="macos"

In der **Phoneword**-App sollte in `OnCreate` zuerst die Benutzeroberfläche geladen werden, die im Android Designer erstellt wurde. Rufen Sie zum Laden der Benutzeroberfläche `SetContentView` auf, und übergeben Sie den *Ressourcenlayoutnamen* für die Layoutdatei: **Main.axml**. Das Layout befindet sich unter `Resource.Layout.Main`:

```csharp
SetContentView (Resource.Layout.Main);
```

Wenn `MainActivity` gestartet wird, wird eine Ansicht basierend auf den Inhalten der Datei **Main.axml** erstellt. Beachten Sie, dass der Name der Layoutdatei mit dem Aktivitätsnamen übereinstimmt &ndash; *Main*.axml ist das Layout für *Main*Activity. Dies ist aus der Sicht von Android nicht erforderlich. Wenn Sie jedoch weitere Bildschirme zur Anwendung hinzufügen, werden Sie feststellen, dass diese Namenskonvention die Zuordnung der Codedatei zur Layoutdatei erleichtert.

::: zone-end

Nachdem die Layoutdatei vorbereitet ist, können Sie die Steuerelemente suchen.
Rufen Sie zum Suchen eines Steuerelements `FindViewById` auf, und übergeben Sie die Ressourcen-ID des Steuerelements:

```csharp
EditText phoneNumberText = FindViewById<EditText>(Resource.Id.PhoneNumberText);
Button translateButton = FindViewById<Button>(Resource.Id.TranslateButton);
TextView translatedPhoneWord = FindViewById<TextView>(Resource.Id.TranslatedPhoneWord);
```

Da nun Verweise zu den Steuerelementen in der Layoutdatei vorhanden sind, können Sie diese für die Reaktion auf Benutzerinteraktionen programmieren.

### <a name="responding-to-user-interaction"></a>Reagieren auf eine Benutzerinteraktion

In Android wartet das `Click`-Ereignis auf die Toucheingabe des Benutzers. In dieser App wird das `Click`-Ereignis mit einem Lambdaausdruck behandelt, stattdessen könnte jedoch auch ein Delegat oder ein benannter Ereignishandler verwendet werden. Der endgültige Code für **TranslateButton** ähnelt Folgendem: 

```csharp
translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    translatedNumber = PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = string.Empty;
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
    }
};
```

## <a name="testing-deployment-and-finishing-touches"></a>Tests, Bereitstellung und Vollendung

Visual Studio für Mac und Visual Studio stellen beide viele Optionen zum Testen und Bereitstellen einer Anwendung bereit. In diesem Abschnitt werden Debugoptionen behandelt, das Testen von Anwendungen auf einem Gerät veranschaulicht und Tools für das Erstellen von benutzerdefinierten App-Symbolen für verschiedene Bildschirmdichten vorgestellt.

### <a name="debugging-tools"></a>Debuggingtools

Probleme im Anwendungscode können schwer zu diagnostizieren sein. Für die Diagnose von komplexen Codeproblemen können Sie [einen Breakpoint festlegen](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint), [Code schrittweise durchlaufen](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code) oder [Informationen an das Protokollfenster ausgeben](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window).

### <a name="deploy-to-a-device"></a>Bereitstellen für ein Gerät

Der Emulator ist für das Bereitstellen und das Testen einer Anwendung gut geeignet, jedoch wird die endgültige App vom Benutzer nicht in einem Emulator verwendet. Es wird empfohlen, Anwendungen frühzeitig und oft auf einem echten Gerät zu testen.

Bevor ein Android-Gerät für das Testen von Anwendungen verwendet werden kann, muss es für die Entwicklung konfiguriert werden. Der Leitfaden [Set Up Device for Development (Einrichten eines Geräts für die Entwicklung)](~/android/get-started/installation/set-up-device-for-development.md) enthält umfassende Anweisungen, um ein Gerät für die Entwicklung vorzubereiten.

::: zone pivot="windows"

Nachdem das Gerät konfiguriert ist, können Sie auf dieses bereitstellen, indem Sie es anschließen, dieses aus dem Dialogfeld **Gerät auswählen** auswählen und die Anwendung starten:

![Auswählen des Geräts für das Debuggen](hello-android-deepdive-images/vs/06-select-device.png "Auswählen des Geräts für das Debuggen")

::: zone-end
::: zone pivot="macos"

Nachdem das Gerät konfiguriert ist, können Sie auf dieses bereitstellen, indem Sie es anschließen, auf **Start (Play)** (Start (Wiedergabe)) drücken, dieses aus dem Dialogfeld **Gerät auswählen** auswählen und auf **OK** drücken:

[![Auswählen des Geräts für das Debuggen](hello-android-deepdive-images/xs/06-select-device-sml.png)](hello-android-deepdive-images/xs/06-select-device.png#lightbox)

::: zone-end

Dadurch wird die Anwendung auf dem Gerät gestartet:

[![Phoneword-Eingabe](hello-android-deepdive-images/05-enter-phoneword-sml.png)](hello-android-deepdive-images/05-enter-phoneword.png#lightbox)

### <a name="set-icons-for-different-screen-densities"></a>Festlegen von Symbolen für verschiedene Bildschirmdichten

Bei Android-Geräten gibt es verschiedene Bildschirmgrößen und -auflösungen, sodass nicht alle Bilder auf allen Bildschirmen gut aussehen. Auf dem Screenshot sehen Sie beispielsweise ein Symbol mit geringer Dichte auf einem Nexus 5 mit hoher Dichte. Beachten Sie, wie unscharf dieses im Vergleich zu den umgebenden Symbolen ist:

[![Unscharfes Symbol](hello-android-deepdive-images/06-blurry-icon-sml.png)](hello-android-deepdive-images/06-blurry-icon.png#lightbox)

Es wird empfohlen, dies zu berücksichtigen, indem Sie Symbole mit verschiedenen Auflösungen im Ordner **Ressourcen** hinzufügen. Android bietet verschiedene Versionen des Ordners **mipmap**, um Startprogrammsymbole mit verschiedenen Dichten zu verarbeiten: *mdpi* für mittlere Dichte, *hdpi* für hohe und *xhdpi*, *xxhdpi* und *xxxhdpi* für Bildschirme mit sehr hoher Dichte. Symbole mit unterschiedlicher Größe werden in den entsprechenden **mipmap**-Ordnern gespeichert:

::: zone pivot="windows"

![mipmap-Ordner](hello-android-deepdive-images/vs/07-mipmap-folders.png "mipmap-Ordner")

::: zone-end
::: zone pivot="windows"

[![mipmap-Ordner](hello-android-deepdive-images/xs/07-mipmap-folders-sml.png)](hello-android-deepdive-images/xs/07-mipmap-folders.png#lightbox)

::: zone-end

Android wählt das Symbol mit der geeigneten Dichte aus:

[![Symbole mit geeigneter Dichte](hello-android-deepdive-images/07-appropriate-density-sml.png)](hello-android-deepdive-images/07-appropriate-density.png#lightbox)

### <a name="generate-custom-icons"></a>Generieren von benutzerdefinierten Symbolen

Nicht jedem steht ein Designer zur Verfügung, um die benutzerdefinierten Symbole und Startbilder zu erstellen, die eine App benötigt, um hervorzustehen. Hier finden Sie einige alternative Ansätze, um benutzerdefinierte App-Grafiken zu generieren:

::: zone pivot="windows"

- [Android Asset Studio](https://romannurik.github.io/AndroidAssetStudio/index.html) &ndash; Ein webbasierter Generator innerhalb des Browsers für alle Arten von Android-Symbolen mit Links zu anderen nützlichen Communitytools. Dieser funktioniert am besten in Google Chrome.

- Visual Studio: Sie können dies verwenden, um einfache Symbole für Ihre App direkt in der IDE zu erstellen.

- [Glyphish](https://www.glyphish.com/): Qualitativ hochwertige, vorgefertigte Symbole, die entweder gratis heruntergeladen oder käuflich erworben werden können.

- [Fiverr](https://www.fiverr.com/) &ndash; Wählen Sie aus einer Vielzahl von Designern aus, die ab 5 Dollar Symbole für Sie erstellen. Obwohl das Ergebnis nicht immer zuverlässig ist, ist dies eine gute Ressource, falls Sie schnell Symbole benötigen.

::: zone-end
::: zone pivot="macos"

- [Android Asset Studio](https://romannurik.github.io/AndroidAssetStudio/index.html) &ndash; Ein webbasierter Generator innerhalb des Browsers für alle Arten von Android-Symbolen mit Links zu anderen nützlichen Communitytools. Dieser funktioniert am besten in Google Chrome.

- [Sketch 3](https://itunes.apple.com/us/app/sketch/id852320343?mt=12) &ndash; Sketch ist eine Mac-App für das Entwerfen von Benutzeroberflächen, Symbolen usw. Diese App wurde verwendet, um die Xamarin App-Symbole und Startbilder zu entwerfen. Sketch 3 ist im App Store verfügbar und kostet ungefähr 80 Dollar. Sie können ebenfalls das kostenlose [Sketch Tool](https://bohemiancoding.com/sketch/tool/) testen.

- [Pixelmator](https://www.pixelmator.com/): Eine vielseitige Mac-App für das Bearbeiten von Bildern, die ungefähr 30 US-Dollar kostet.

- [Glyphish](https://www.glyphish.com/): Qualitativ hochwertige, vorgefertigte Symbole, die entweder gratis heruntergeladen oder käuflich erworben werden können.

- [Fiverr](https://www.fiverr.com/) &ndash; Wählen Sie aus einer Vielzahl von Designern aus, die ab 5 Dollar Symbole für Sie erstellen. Obwohl das Ergebnis nicht immer zuverlässig ist, ist dies eine gute Ressource, falls Sie schnell Symbole benötigen.

::: zone-end

Weitere Informationen zu Symbolgrößen und Anforderungen finden Sie im Leitfaden [Android Resources (Android-Ressourcen)](~/android/app-fundamentals/resources-in-android/index.md).

::: zone pivot="macos"

### <a name="adding-google-play-services-packages"></a>Hinzufügen von Google Play Services-Paketen

_Google Play Services_ ist eine Reihe von Add-On-Bibliotheken, die es Android-Entwicklern ermöglichen, von den neuesten Funktionen von Google zu profitieren, z.B. von Google Maps, Google Cloud Messaging und der In-App-Abrechnung.
Zuvor wurden die Bindungen für alle Google Play Services-Bibliotheken von Xamarin in Form eines einzelnen Pakets bereitgestellt. Ab Visual Studio für Mac ist ein neues Dialogfeld für Projekte verfügbar, um auszuwählen, welche Google Play Services-Pakete in Ihre App eingeschlossen werden sollen.

Klicken Sie mit der rechten Maustaste auf den Knoten **Pakete** in Ihrer Projektstruktur, und klicken Sie auf **Google Play-Dienst hinzufügen...** :

[![Hinzufügen von Google Play-Diensten](hello-android-deepdive-images/xs/08-add-google-play-services-sml.png)](hello-android-deepdive-images/xs/08-add-google-play-services.png#lightbox)

Wenn das Dialogfeld **Google Play-Dienste hinzufügen** angezeigt wird, wählen Sie die Pakete (NuGets) aus, die Sie Ihrem Projekt hinzufügen möchten:

[![Auswählen von Paketen](hello-android-deepdive-images/xs/09-add-dialog-sml.png)](hello-android-deepdive-images/xs/09-add-dialog.png#lightbox)

Wenn Sie einen Dienst auswählen und auf **Paket hinzufügen** klicken, lädt Visual Studio für Mac das ausgewählte Paket sowie alle erforderlichen abhängigen Google Play Services-Pakete herunter und installiert diese. In manchen Fällen wird Ihnen ein Dialogfeld **Zustimmung zur Lizenz** angezeigt, in dem Sie auf **Annehmen** klicken müssen, bevor die Pakete installiert werden:

[![Zustimmung zur Lizenz](hello-android-deepdive-images/xs/10-license-acceptance-sml.png)](hello-android-deepdive-images/xs/10-license-acceptance.png#lightbox)

::: zone-end

## <a name="summary"></a>Zusammenfassung

Herzlichen Glückwunsch! Sie sollten nun mit den Komponenten einer Xamarin.Android-Anwendung und mit den Tools, die für die Erstellung benötigt werden, vertraut sein.

Im nächsten Tutorial der Reihe _Erste Schritte_ erweitern Sie Ihre Anwendung, um mehrere Bildschirme zu verarbeiten, während Sie erweiterte Android-Architekturen und -Konzepte kennenlernen.
