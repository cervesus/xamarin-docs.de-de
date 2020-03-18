---
title: 'Exemplarische Vorgehensweise: Binden einer Android-Kotlin-Bibliothek'
description: Dieser Artikel enthält eine praktische exemplarische Vorgehensweise zum Erstellen einer Xamarin.Android-Bindung für eine vorhandene Kotlin-Bibliothek, BubblePicker. Es werden Themen wie das Kompilieren einer Kotlin-Bibliothek, deren Bindung und die Verwendung der Bindung in einer Xamarin.Android-Anwendung behandelt.
ms.prod: xamarin
ms.assetid: 5E45451F-514F-4F2B-A6E8-599ED2EFDA2C
ms.technology: xamarin-android
author: alexeystrakh
ms.author: alstrakh
ms.date: 02/11/2020
ms.openlocfilehash: cbd7c796cd13aa45dc107bddf06ca44d6adbdf9d
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "77519674"
---
# <a name="walkthrough-bind-an-android-kotlin-library"></a>Exemplarische Vorgehensweise: Binden einer Android-Kotlin-Bibliothek

Xamarin ermöglicht Mobilgeräteentwicklern das Erstellen von plattformübergreifenden nativen mobilen Apps mit Visual Studio und C#. Sie können die standardmäßig im SDK der Android-Plattform enthaltenen Komponenten verwenden. In vielen Fällen kann aber die Verwendung von Drittanbieter-SDKs wünschenswert sein, die für diese Plattform geschrieben wurden. Xamarin macht dies über Bindungen möglich. Wenn Sie das Android-Framework eines Drittanbieters in Ihre Xamarin.Android-Anwendung integrieren möchten, müssen Sie eine Xamarin.Android-Bindung erstellen, damit Sie das Framework in Ihren Anwendungen verwenden können.

Die Android-Plattform wird zusammen mit den nativen Sprachen und Tools ständig weiterentwickelt. Dazu gehört die vor kurzem erfolgte Einführung der Sprache „Kotlin“, die letztendlich Java ersetzen soll. Es gibt eine Reihe von Drittanbieter-SDKs, die bereits von Java nach Kotlin migriert wurden und neue Herausforderungen darstellen. Obwohl der Kotlin-Bindungsprozess dem von Java ähnelt, sind zusätzliche Schritte und Konfigurationseinstellungen erforderlich, damit eine Erstellung und Ausführung als Teil einer Xamarin.Android-Anwendung möglich ist.  

Dieses Dokuments verfolgt das Ziel, eine übergeordnete Vorgehensweise für dieses Szenario anhand einer ausführlichen Schritt-für-Schritt-Anleitung mit einem einfachen Beispiel zu skizzieren.

## <a name="background"></a>Hintergrund

Kotlin wurde im Februar 2016 veröffentlicht und 2017 als Alternative zum normalen Java-Compiler in Android Studio positioniert. 2019 wurde von Google angekündigt, dass die-Programmiersprache Kotlin zur bevorzugten Sprache für Entwickler von Android-Apps wird. Der die allgemeine Vorgehensweise für die Bindung ähnelt [dem Bindungsprozess regulärer Java-Bibliotheken](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/) mit einigen wichtigen Kotlin-spezifischen Schritten.

## <a name="prerequisites"></a>Voraussetzungen

Für die Durchführung dieser exemplarischen Vorgehensweise benötigen Sie Folgendes:

- [Android Studio](https://developer.android.com/studio)
- [Visual Studio für Mac](https://visualstudio.microsoft.com/downloads)
- [Java Decompiler](http://java-decompiler.github.io/)

## <a name="build-a-native-library"></a>Erstellen einer nativen Bibliothek

Der erste Schritt besteht darin, mit Android Studio eine native Kotlin-Bibliothek zu erstellen. Die Bibliothek wird normalerweise von einem Drittanbieter bereitgestellt oder ist im [Maven-Repository von Google](https://maven.google.com/web/index.html) und anderen Remoterepositorys verfügbar. In diesem Tutorial wird als Beispiel eine Bindung für die Kotlin-Bibliothek „Bubble Picker“ erstellt:

![GitHub BubblePicker-Demo](walkthrough-images/github-bubblepicker-demo.png)

1. Laden Sie [den Quellcode](https://github.com/igalata/Bubble-Picker/archive/develop.zip) für die Bibliothek von GitHub herunter, und entpacken Sie ihn in einen lokalen Ordner namens **Bubble-Picker**.

1. Starten Sie die Android Studio, und wählen Sie die Menüoption **Vorhandenes Android Studio-Projekt öffnen** und dann den lokalen Ordner „Bubble-Picker“ aus:

    ![Android Studio – Projekt öffnen](walkthrough-images/android-studio-open-project.png)

1. Vergewissern Sie sich, dass die Android Studio, einschließlich Gradle, auf dem neuesten Stand ist. Der Quellcode kann erfolgreich mit Android Studio v3.5.3 und Gradle v5.4.1 kompiliert werden. Anweisungen zum Aktualisieren von Gradle auf die aktuelle Gradle-Version [finden Sie hier](https://gradle.org/install/).

1. Vergewissern Sie sich, dass das erforderliche Android SDK installiert ist. Der Quellcode erfordert Android SDK v25. Öffnen Sie die Menüoption **Tools > SDK-Manager**, um SDK-Komponenten zu installieren.

1. Aktualisieren und synchronisieren Sie die Hauptkonfigurationsdatei **build.gradle**, die sich im Stammverzeichnis des Projektordners befindet:

    - Legen Sie die Kotlin-Version auf 1.3.10 fest:

        ```gradle
        buildscript {
            ext.kotlin_version = '1.3.10'
        }
        ```

    - Registrieren Sie das Maven-Standardrepository von Google, damit die Abhängigkeit zur Unterstützungsbibliothek aufgelöst werden kann:

        ```gradle
        allprojects {
            repositories {
                jcenter()
                maven {
                    url "https://maven.google.com"
                }
            }
        }
        ```

    - Nachdem die Konfigurationsdatei aktualisiert wurde, ist Sie nicht synchronisiert, sodass Gradle die Schaltfläche **Jetzt synchronisieren** anzeigt. Drücken Sie die Schaltfläche, und warten Sie, bis der Synchronisierungsvorgang abgeschlossen ist:

        ![Android Studio – Gradle – Jetzt synchronisieren](walkthrough-images/android-studio-gradle-syncnow.png)

        > [!TIP]
        > Der Abhängigkeitscache von Gradle ist möglicherweise beschädigt. Dies tritt manchmal nach einem Timeout der Netzwerkverbindung auf. Laden Sie Abhängigkeiten erneut herunter, und synchronisieren Sie das Projekt (Netzwerk erforderlich).

        > [!TIP]
        > Der Zustand eines Gradle-Buildprozesses (Daemon) ist möglicherweise beschädigt. Wenn Sie alle Gradle-Daemons beenden, kann dieses Problem u. U. gelöst werden. Beenden Sie Gradle-Buildprozesse (Neustart erforderlich). Wenn Gradle-Prozesse beschädigt sind, können Sie auch versuchen, die IDE zu schließen und anschließend alle Java-Prozesse zu beenden.

        > [!TIP]
        > Ihr Projekt verwendet möglicherweise ein Drittanbieter-Plug-In, das nicht mit den anderen Plug-Ins im Projekt oder mit der für das Projekt erforderlichen Gradle-Version kompatibel ist.

1. Öffnen Sie das Gradle-Menü auf der rechten Seite, navigieren Sie zum Menü **bubblepicker > Aufgaben** Menü, führen Sie die Aufgabe **build** aus, indem Sie darauf doppeltippen, und warten Sie, bis der Buildprozess abgeschlossen ist:

    ![Android Studio – Gradle – Aufgabe ausführen](walkthrough-images/android-studio-gradle-execute-task.png)

1. Öffnen Sie den Stammordner im Dateibrowser, und navigieren Sie zum Buildordner: **Bubble-Picker -> bubblepicker ->  build -> outputs -> aar**. Speichern Sie die Datei **bubblepicker-release.aar** als **bubblepicker-v1.0.aar**. Diese Datei wird später im Bindungsprozess verwendet:

    ![Android Studio – AAR-Ausgabe](walkthrough-images/android-studio-aar-output.png)

Bei der AAR-Datei handelt es sich um ein Android-Archiv, das den kompilierten Kotlin-Quellcode und die Objekte enthält, die von Android zum Ausführen einer Anwendung mit diesem SDK benötigt werden.  

## <a name="prepare-metadata"></a>Vorbereiten der Metadaten

Der zweite Schritt besteht darin, die Metadatentransformationsdatei vorzubereiten, die von Xamarin.Android verwendet wird, um die jeweiligen C#-Klassen zu generieren. Ein Xamarin.Android-Bindungsprojekt ermittelt alle nativen Klassen und Member in einem bestimmten Android-Archiv und erzeugt anschließend eine XML-Datei mit den entsprechenden Metadaten. Die manuell erstellte Metadatentransformationsdatei wird dann auf die zuvor generierte Baseline angewendet, um die endgültige XML-Definitionsdatei zu erstellen, die zum Generieren des C#-Codes verwendet wird.

Die Metadaten verwenden  [XPath](https://www.w3.org/TR/xpath/) -Syntax und werden vom Bindungsgenerator verwendet, um die Erstellung der Bindungsassembly zu beeinflussen. Im Artikel [Metadaten für Java-Bindungen](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata) finden Sie weitere Informationen zu Transformationen, die angewendet werden können:

1. Erstellen Sie eine leere Datei namens **Metadata.xml**:

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <metadata>
    </metadata>
    ```

1. Definieren Sie die XML-Transformationen:

- Die native Kotlin-Bibliothek weist zwei Abhängigkeiten auf, die nicht für die C#-Welt verfügbar gemacht werden sollen. Definieren Sie daher zwei Transformationen, damit sie vollständig ignoriert werden. Wichtig: Die nativen Member werden nicht aus der resultierenden Binärdatei entfernt, sondern es werden nur C#-Klassen generiert. Die Abhängigkeiten können mit [Java Decompiler](http://java-decompiler.github.io/) ermittelt werden. Führen Sie das Tool aus, und öffnen Sie die zuvor erstellte AAR-Datei. Daraufhin wird die Struktur des Android-Archivs mit allen Abhängigkeiten, Werten, Ressourcen, Manifesten und Klassen angezeigt:  

    ![Java Decompiler – Abhängigkeiten](walkthrough-images/java-decompiler-dependencies.png)

    Die Transformationen zum Überspringen der Verarbeitung dieser Pakete werden mit XPath-Anweisungen definiert:

    ```xml
    <remove-node path="/api/package[starts-with(@name,'org.jbox2d')]" />
    <remove-node path="/api/package[starts-with(@name,'org.slf4j')]" />
    ```

- Die native `BubblePicker`-Klasse verfügt über zwei Methoden, `getBackgroundColor` und `setBackgroundColor`. Die folgende Transformation ändert sie in die C#-Eigenschaft `BackgroundColor`:

    ```xml
    <attr path="/api/package[@name='com.igalata.bubblepicker.rendering']/class[@name='BubblePicker']/method[@name='getBackground' and count(parameter)=0]" name="propertyName">BackgroundColor</attr>
    <attr path="/api/package[@name='com.igalata.bubblepicker.rendering']/class[@name='BubblePicker']/method[@name='setBackground' and count(parameter)=1 and parameter[1][@type='int']]" name="propertyName">BackgroundColor</attr>
    ```

- Für vorzeichenlose Typen (`UInt, UShort, ULong, UByte`) ist eine besondere Behandlung erforderlich. Für diese Typen ändert Kotlin die Methodennamen und die Parametertypen automatisch, was sich generierten Code ersichtlich ist:

    ```Kotlin
    public open fun fooUIntMethod(value: UInt) : String {
        return "fooUIntMethod${value}"
    }
    ```

    Dieser Code wird in den folgenden Java-Bytecode kompiliert:

    ```Java byte code
    @NotNull
    public String fooUIntMethod-WZ4Q5Ns(int value) {
    return "fooUIntMethod" + UInt.toString-impl(value);
    }
    ```

    Darüber hinaus werden auch verwandte Typen wie `UIntArray, UShortArray, ULongArray, UByteArray` von Kotlin beeinflusst. Der Methodenname wird so geändert, dass ein zusätzliches Suffix enthalten ist, und die Parameter werden in ein Array von Elementen mit vorzeichenbehafteten Versionen der gleichen Typen geändert. Im folgenden Beispiel wird ein Parameter vom Typ `UIntArray` automatisch in `int[]` konvertiert, und der Methodenname wird von `fooUIntArrayMethod` in `fooUIntArrayMethod--ajY-9A` geändert. Der Name wird von Xamarin.Android-Tools erkannt und als gültiger Methodenname generiert:

    ```Kotlin
    public open fun fooUIntArrayMethod(value: UIntArray) : String {
        return "fooUIntArrayMethod${value.size}"
    }
    ```

    Dieser Code wird in den folgenden Java-Bytecode kompiliert:

    ```Java byte code
    @NotNull
    public String fooUIntArrayMethod--ajY-9A(@NotNull int[] value) {
        Intrinsics.checkParameterIsNotNull(value, "value");
        return "fooUIntArrayMethod" + UIntArray.getSize-impl(value);
    }
    ```

    Um einen aussagekräftigen Namen zuzuweisen, können die folgenden Metadaten zu **metadata.xml** hinzugefügt werden, die den Namen wieder in den ursprünglich im Kotlin-Code definierten Namen aktualisieren:

    ```xml
    <attr path="/api/package[@name='com.microsoft.simplekotlinlib']/class[@name='FooClass']/method[@name='fooUIntArrayMethod--ajY-9A']" name="managedName">fooUIntArrayMethod</attr>
    ```

    Im BubblePicker-Beispiel gibt es keine Member, die vorzeichenlose Typen verwenden, sodass keine weiteren Änderungen erforderlich sind.

- Kotlin-Member mit generischen Parametern werden standardmäßig in Parameter des Typs „Java.`Lang.Object` “ umgewandelt. Eine Kotlin-Methode weist beispielsweise einen generischen Parameter \<T> auf:

    ```Kotlin
    public open fun <T>fooGenericMethod(value: T) : String {
    return "fooGenericMethod${value}"
    }
    ```

    Sobald eine Xamarin.Android-Bindung generiert wurde, wird die-Methode wie folgt für C# verfügbar gemacht:

    ```csharp
    [Register ("fooGenericMethod", "(Ljava/lang/Object;)Ljava/lang/String;", "GetFooGenericMethod_Ljava_lang_Object_Handler")]
    [JavaTypeParameters (new string[] {
        "T"
    })]

    public virtual string FooGenericMethod (Java.Lang.Object value);
    ```

    Java- und Kotlin-Generics werden von Xamarin.Android-Bindungen nicht unterstützt. Daher wird eine generalisierte C#-Methode für den Zugriff auf die generische API erstellt. Als Problemumgehung können Sie eine Kotlin-Wrapperbibliothek erstellen und erforderliche APIs stark typisiert ohne Generics verfügbar machen. Alternativ können Sie Hilfsprogramme auf der C#-Seite erstellen, um das Problem ebenfalls über stark typisierte APIs zu beheben.

    > [!TIP]
    > Durch Transformation der Metadaten können alle Änderungen auf die generierte Bindung angewendet werden. Im Artikel [Binden einer Java-Bibliothek](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/) wird ausführlich erläutert, wie die Metadaten generiert und verarbeitet werden.

## <a name="build-a-binding-library"></a>Erstellen einer Bindungsbibliothek

Der nächste Schritt besteht darin, mit der Visual Studio-Bindungsvorlage ein Xamarin.Android-Bindungsprojekt zu erstellen, erforderliche Metadaten sowie native Verweise hinzuzufügen und dann das Projekt zu kompilieren, um eine verwendbare Bibliothek zu erstellen:

1. Öffnen Sie Visual Studio für Mac, und erstellen Sie ein neues Xamarin.Android-Bindungsbibliotheksprojekt, weisen Sie einen Namen zu (**testBubblePicker.Binding**), und schließen Sie den Assistenten ab. Die Xamarin.Android-Bindungsvorlage befindet sich im folgenden Pfad: **Android > Bibliothek > Bindungsbibliothek**:

    ![Visual Studio – Bindung erstellen](walkthrough-images/visual-studio-create-binding.png)

    Im Ordner „Transformations“ sind drei Haupttransformationsdateien vorhanden:

    - **Metadata.xml**: Ermöglicht die Durchführung von Änderungen an der endgültigen API, z. B. Ändern des Namespaces der generierten Bindung.
    - **EnumFields.xml**: Enthält die Zuordnung zwischen ganzzahligen Java-Konstanten und C#-Enumerationen.
    - **EnumMethods.xml**: Ermöglicht die Änderung von Methodenparametern und Rückgabetypen von ganzzahligen Java-Konstanten in C#-Enumerationen.

    Lassen Sie die Dateien **EnumFields.xml** und **EnumMethods.xml** leer und aktualisieren Sie **Metadata.xml**, um Ihre Transformationen zu definieren.

1. Ersetzen Sie die vorhandene **Transformations/Metadata.xml**-Datei durch die Datei **Metadata.xml**, die im vorherigen Schritt erstellt wurde. Vergewissern Sie sich im Eigenschaftenfenster, dass der **Buildvorgang** für die Datei auf **TransformationFile** festgelegt ist:

    ![Visual Studio – Metadaten](walkthrough-images/visual-studio-metadata.png)

1. Fügen Sie die Datei **bubblepicker-v1.0.aar** Datei, die Sie in Schritt 1 erstellt haben, als nativen Verweis zum Bindungsprojekt hinzu. Öffnen Sie Finder, und navigieren Sie zum Ordner mit dem Android-Archiv, um native Bibliotheksverweise hinzuzufügen. Ziehen Sie das Archiv im Projektmappen-Explorer in den Ordner „Jars“. Alternativ können Sie die Kontextmenüoption **Hinzufügen** im Ordner „Jars“ verwenden und **Vorhandene Dateien...** auswählen. Kopieren Sie die Datei für diese exemplarische Vorgehensweise in das Verzeichnis. Achten Sie darauf, dass der **Buildvorgang** auf **LibraryProjectZip** festgelegt ist:

    ![Visual Studio – Nativer Verweis](walkthrough-images/visual-studio-native-reference.png)

1. Fügen Sie einen Verweis zum Paket [Xamarin.Kotlin.StdLib NuGet](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/) hinzu. Dieses Paket ist eine Bindung für die Kotlin-Standardbibliothek. Ohne dieses Paket funktioniert die Bindung nur dann, wenn die Kotlin-Bibliothek keine Kotlin-spezifischen Typen verwendet. Andernfalls werden alle diese Member nicht für C# verfügbar gemacht, sodass jede App, die versucht, die Bindung zu verwenden, zur Laufzeit abstürzt.

    > [!TIP]
    > Aufgrund einer Einschränkung von Xamarin.Android können Bindungstools nur ein einzelnes Android-Archiv (AAR) pro Bindungsprojekt hinzufügen. Wenn mehrere AAR-Dateien eingeschlossen werden müssen, sind mehrere Xamarin.Android-Projekte erforderlich, jeweils eins pro AAR. Wenn dies in dieser exemplarischen Vorgehensweise der Fall wäre, müssten die oben aufgeführten vier Aktionen dieses Schritts für jedes Archiv wiederholt werden. Als alternative Möglichkeit können mehrere Android-Archive manuell als einzelnes Archiv zusammengeführt werden, sodass Sie ein einzelnes Xamarin.Android-Bindungsprojekt verwenden können.

1. Die letzte Aktion besteht darin, die Bibliothek ohne Kompilierungsfehler zu erstellen. Wenn Kompilierungsfehler auftreten, können diese anhand der Metadata.xml-Datei, die Sie zuvor durch Hinzufügen von XML-Transformationsmetadaten, die Bibliotheksmember hinzufügen, entfernen oder umbenennen, erstellt haben, ermittelt und behandelt werden.

## <a name="consume-the-binding-library"></a>Verwenden der Bindungsbibliothek

Der letzte Schritt besteht darin, die Xamarin.Android-Bindungsbibliothek in einer Xamarin.Android-Anwendung zu verwenden. Erstellen Sie ein neues Xamarin.Android-Projekt, fügen Sie einen Verweis zur Bindungsbibliothek hinzu und rendern Sie die Bubble Picker-Benutzeroberfläche:

1. Erstellen Sie das Xamarin.Android-Projekt. Verwenden Sie die **Android-> App > Android App** als Ausgangspunkt, und wählen Sie **Aktuellste und größte** als Zielplattformoption aus, um Kompatibilitätsprobleme zu vermeiden. Alle folgenden Schritte gelten für dieses Projekt:

    ![Visual Studio – App erstellen](walkthrough-images/visual-studio-create-app.png)

1. Fügen Sie einen Projektverweis zum Bindungsprojekt oder einen Verweis auf die zuvor erstellte DLL hinzu:

    ![Visual Studio Add Binding Reference.png](walkthrough-images/visual-studio-add-binding-reference.png)

1. Fügen Sie einen Verweis zum [Xamarin.Kotlin.StdLib NuGet](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/)-Paket hinzu, das Sie zuvor dem Xamarin.Android-Bindungsprojekt hinzugefügt haben, damit alle Kotlin-spezifischen Typen unterstützt werden, die zur Laufzeit verarbeitet werden müssen.  Ohne dieses Paket kann die App zwar kompiliert werden, die App stürzt aber zur Laufzeit ab:

    ![Visual Studio – StdLib NuGet hinzufügen](walkthrough-images/visual-studio-add-stdlib-nuget.png)

1. Fügen Sie das `BubblePicker`-Steuerelement zum Android-Layout für `MainActivity` hinzu. Öffnen Sie die Datei **testBubblePicker/Resources/layout/content_main.xml**, und fügen Sie den BubblePicker-Steuerelementknoten als letztes Element des RelativeLayout-Stammsteuerelements hinzu:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <RelativeLayout …>
        …
        <com.igalata.bubblepicker.rendering.BubblePicker
            android:id="@+id/picker"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:backgroundColor="@android:color/white" />
    </RelativeLayout>
    ```

1. Aktualisieren Sie den Quellcode der App, und fügen Sie die Initialisierungslogik zu `MainActivity` hinzu, damit das Bubble Picker-SDK aktiviert wird:

    ```csharp
    protected override void OnCreate(Bundle savedInstanceState)
    {
        ...
        var picker = FindViewById<BubblePicker>(Resource.Id.picker);
        picker.BubbleSize = 20;
        picker.Adapter = new BubblePickerAdapter();
        picker.Listener = new BubblePickerListener(picker);
        ...
    }
    ```

    `BubblePickerAdapter` und `BubblePickerListener` sind zwei Klassen, die neu erstellt werden müssen, um die bubbles-Daten zu verarbeiten und die Interaktion zu steuern:

    ```csharp
    public class BubblePickerAdapter : Java.Lang.Object, IBubblePickerAdapter
    {
        private List<string> _bubbles = new List<string>();
        public int TotalCount => _bubbles.Count;
        public BubblePickerAdapter()
        {
            for (int i = 0; i < 10; i++)
            {
                _bubbles.Add($"Item {i}");
            }
        }

        public PickerItem GetItem(int itemIndex)
        {
            if (itemIndex < 0 || itemIndex >= _bubbles.Count)
                return null;

            var result = _bubbles[itemIndex];
            var item = new PickerItem(result);
            return item;
        }
    }

    public class BubblePickerListener : Java.Lang.Object, IBubblePickerListener
    {
        public View Picker { get; }
        public BubblePickerListener(View picker)
        {
            Picker = picker;
        }

        public void OnBubbleDeselected(PickerItem item)
        {
            Snackbar.Make(Picker, $"Deselected: {item.Title}", Snackbar.LengthLong)
                .SetAction("Action", (Android.Views.View.IOnClickListener)null)
                .Show();
        }

        public void OnBubbleSelected(PickerItem item)
        {
            Snackbar.Make(Picker, $"Selected: {item.Title}", Snackbar.LengthLong)
            .SetAction("Action", (Android.Views.View.IOnClickListener)null)
            .Show();
        }
    }
    ```

1. Führen Sie die App aus, um die Bubble Picker-Benutzeroberfläche zu rendern:

    ![BubblePicker-Demo](walkthrough-images/bubble-picker-demo.png)

    Das Beispiel benötigt weiteren Code zum Rendern von Elementen und zum Verarbeiten von Interaktionen. Das `BubblePicker`-Steuerelement wurde jedoch erfolgreich erstellt und aktiviert.

Herzlichen Glückwunsch! Sie haben erfolgreich eine Xamarin.Android-App und eine Bindungsbibliothek erstellt, die eine Kotlin-Bibliothek verwendet.  

Sie sollten jetzt über eine einfache Xamarin.Android-Anwendung verfügen, die über eine Xamarin.Android-Bindungsbibliothek eine native Kotlin-Bibliothek verwendet. In dieser exemplarischen Vorgehensweise wird ein einfaches Beispiel verwendet, um die neuen Schlüsselkonzepte besser herauszustellen. In realen Szenarien müssen Sie wahrscheinlich eine größere Anzahl von APIs verfügbar machen und Metadatentransformationen darauf anwenden.

## <a name="related-links"></a>Verwandte Links

- [Android Studio](https://developer.android.com/studio)
- [Gradle-Installation](https://gradle.org/install/)
- [Visual Studio für Mac](https://visualstudio.microsoft.com/downloads)
- [Java Decompiler](http://java-decompiler.github.io/)
- [Kotlin-Bibliothek „BubblePicker“](https://github.com/igalata/Bubble-Picker)
- [Binden einer Java-Bibliothek](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/)
- [XPath](https://www.w3.org/TR/xpath/)
- [Metadaten für Java-Bindungen](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata)
- [Xamarin.Kotlin.StdLib NuGet](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/)
- [Beispielprojektrepository](https://github.com/alexeystrakh/xamarin-binding-kotlin-framework)
