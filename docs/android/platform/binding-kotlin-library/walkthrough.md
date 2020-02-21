---
title: 'Exemplarische Vorgehensweise: Binden einer Android-Kotlin-Bibliothek'
description: Dieser Artikel enthält eine praktische Exemplarische Vorgehensweise zum Erstellen einer xamarin. Android-Bindung für eine vorhandene artelin-Bibliothek, bubblepicker. Es behandelt Themen wie das Kompilieren einer Kotlin-Bibliothek, deren Bindung und die Verwendung der Bindung in einer xamarin. Android-Anwendung.
ms.prod: xamarin
ms.assetid: 5E45451F-514F-4F2B-A6E8-599ED2EFDA2C
ms.technology: xamarin-android
author: alexeystrakh
ms.author: alstrakh
ms.date: 02/11/2020
ms.openlocfilehash: cbd7c796cd13aa45dc107bddf06ca44d6adbdf9d
ms.sourcegitcommit: fec87846fcb262fc8b79774a395908c8c8fc8f5b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2020
ms.locfileid: "77519674"
---
# <a name="walkthrough-bind-an-android-kotlin-library"></a>Exemplarische Vorgehensweise: Binden einer Android-Kotlin-Bibliothek

Xamarin ermöglicht mobilen Entwicklern das Erstellen von plattformübergreifenden nativen mobilen Apps mithilfe von C#Visual Studio und. Sie können die Android Platform SDK-komponentenstandard mäßig verwenden, aber in vielen Fällen möchten Sie auch Drittanbieter-SDKs verwenden, die für diese Plattform geschrieben wurden, und xamarin ermöglicht es Ihnen, dies über Bindungen durchzuführen. Wenn Sie ein Drittanbieter-Android-Framework in Ihre xamarin. Android-Anwendung integrieren möchten, müssen Sie eine xamarin. Android-Bindung für die Anwendung erstellen, bevor Sie Sie in Ihren Anwendungen verwenden können.

Die Android-Plattform wird zusammen mit den nativen Sprachen und Tools ständig weiterentwickelt, einschließlich der aktuellen Einführung der Sprache "Kotlin", die schließlich zum Ersetzen von Java festgelegt wird. Es gibt eine Reihe von 3D--Anbietern von Drittanbietern, die bereits von Java zu Kotlin migriert wurden und neue Herausforderungen darstellen. Obwohl der Kotlin-Bindungsprozess Java ähnelt, sind zusätzliche Schritte und Konfigurationseinstellungen erforderlich, um erfolgreich als Teil einer xamarin. Android-Anwendung erstellt und ausgeführt zu werden.  

Das Ziel dieses Dokuments besteht darin, ein allgemeines Verfahren für die Lösung dieses Szenarios aufzuzeigen und eine ausführliche Schritt-für-Schritt-Anleitung mit einem einfachen Beispiel bereitzustellen.

## <a name="background"></a>Hintergrund

"Kotlin" wurde im Februar 2016 veröffentlicht und als Alternative zum Standard-Java-Compiler positioniert, um 2017 zu Android Studio. Später in 2019 hat Google angekündigt, dass die-Programmiersprache "Kotlin" zu Ihrer bevorzugten Sprache für Android-App-Entwickler werden würde. Der Ansatz für die allgemeine Bindung ähnelt [dem Bindungsprozess regulärer Java-Bibliotheken](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/) mit einigen wichtigen, mit einem Paar wichtigen.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie diese exemplarische Vorgehensweise abschließen möchten, müssen folgende Voraussetzungen erfüllt sein:

- [Android Studio](https://developer.android.com/studio)
- [Visual Studio für Mac](https://visualstudio.microsoft.com/downloads)
- [Java-compilercompiler](http://java-decompiler.github.io/)

## <a name="build-a-native-library"></a>Erstellen einer nativen Bibliothek

Der erste Schritt besteht in der Erstellung einer nativen-Datei mit dem Android Studio. Die Bibliothek wird normalerweise von einem Entwickler von Drittanbietern bereitgestellt oder ist im [Maven-Repository von Google und anderen remoterepositorys](https://maven.google.com/web/index.html) verfügbar. In diesem Tutorial wird z. b. eine Bindung für die-Blasen Auswahl-artelin-Bibliothek erstellt:

![GitHub bubblepicker-Demo](walkthrough-images/github-bubblepicker-demo.png)

1. Laden Sie [den Quellcode](https://github.com/igalata/Bubble-Picker/archive/develop.zip) von GitHub für die Bibliothek herunter, und entpacken Sie ihn in eine **Blasen**Auswahl für lokale Ordner.

1. Starten Sie die Android Studio, und wählen Sie die Menüoption **vorhandenes Android Studio Projekt öffnen** aus, indem Sie den lokalen Ordner Blasen Auswahl auswählen:

    ![Android Studio Projekt öffnen](walkthrough-images/android-studio-open-project.png)

1. Vergewissern Sie sich, dass die Android Studio auf dem neuesten Stand ist, einschließlich gradle. Der Quellcode kann erfolgreich auf Android Studio v 3.5.3, gradle v 5.4.1, erstellt werden. Anweisungen zum Aktualisieren von gradle auf die neueste gradle-Version [finden Sie hier](https://gradle.org/install/).

1. Vergewissern Sie sich, dass die erforderliche Android SDK installiert ist. Der Quellcode erfordert Android SDK V25. Öffnen Sie Extras **> SDK-Manager** -Menüoption, um SDK-Komponenten zu installieren.

1. Aktualisieren und synchronisieren Sie die Hauptkonfigurationsdatei " **Build. gradle** ", die sich im Stammverzeichnis des Projekt Ordners befindet:

    - Festlegen der Version von "Kotlin" auf 1.3.10

        ```gradle
        buildscript {
            ext.kotlin_version = '1.3.10'
        }
        ```

    - Registrieren Sie das Standard-Maven-Repository von Google, damit die Abhängigkeit der Unterstützungs Bibliothek aufgelöst werden kann:

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

    - Nachdem die Konfigurationsdatei aktualisiert wurde, ist Sie nicht mehr synchronisiert, und gradle zeigt die Schaltfläche **Jetzt synchronisieren** an, drücken Sie, und warten Sie, bis der Synchronisierungs Vorgang abgeschlossen ist:

        ![Jetzt Android Studio gradle Sync](walkthrough-images/android-studio-gradle-syncnow.png)

        > [!TIP]
        > Der Abhängigkeits Cache von gradle ist möglicherweise beschädigt, Dies tritt manchmal nach einem Timeout der Netzwerkverbindung auf. Laden Sie Abhängigkeiten erneut herunter, und synchronisieren Sie das Projekt (erfordert Netzwerk).

        > [!TIP]
        > Der Status eines gradle-Buildprozesses (Daemon) ist möglicherweise beschädigt. Wenn Sie alle gradle-Daemons beenden, kann dieses Problem gelöst werden. Beenden der gradle-Buildprozesse (Neustart erforderlich). Wenn gradle-Prozesse beschädigt sind, können Sie auch versuchen, die IDE zu schließen und anschließend alle Java-Prozesse zu beenden.

        > [!TIP]
        > Ihr Projekt verwendet möglicherweise ein Drittanbieter-Plug-in, das nicht mit den anderen Plug-ins im Projekt oder mit der vom Projekt angeforderten Version des gradle kompatibel ist.

1. Öffnen **Sie das** gradle-Menü auf der rechten Seite, navigieren Sie zum Menü **bubblepicker > Aufgaben** , führen Sie die Buildaufgabe aus, indem Sie darauf Doppel tippen, und warten Sie, bis der Buildprozess beendet wurde:

    ![Android Studio gradle-Execute-Aufgabe](walkthrough-images/android-studio-gradle-execute-task.png)

1. Öffnen Sie den Ordner Dateien der Stamm Ordner, und navigieren Sie zum Buildordner: **Blase-Picker-> bubblepicker-> Build->-Ausgaben-> Aar**, speichern Sie die Datei **bubblepicker-Release. Aar** als **bubblepicker-v 1.0. Aar.** diese Datei wird später im Bindungsprozess verwendet:

    ![Android Studio Aar-Ausgabe](walkthrough-images/android-studio-aar-output.png)

Bei der Aar-Datei handelt es sich um ein Android-Archiv, das den kompilierten Kotlin-Quellcode und die Ressourcen enthält, die von Android zum Ausführen einer Anwendung mit diesem SDK benötigt werden.  

## <a name="prepare-metadata"></a>Vorbereiten von Metadaten

Der zweite Schritt besteht darin, die metadatentransformations-Datei vorzubereiten, die von xamarin. Android verwendet wird C# , um die jeweiligen Klassen zu generieren. Ein xamarin. Android-Bindungs Projekt ermittelt alle systemeigenen Klassen und Member aus einem bestimmten Android-Archiv und erzeugt anschließend eine XML-Datei mit den entsprechenden Metadaten. Die manuell erstellte metadatentransformations-Datei wird dann auf die zuvor generierte Baseline angewendet, um die endgültige XML-Definitionsdatei zu C# erstellen, die zum Generieren des Codes verwendet wird.

Die Metadaten verwenden die [XPath](https://www.w3.org/TR/xpath/) - Syntax und werden vom Bindungs Generator verwendet, um die Erstellung der bindungsassembly zu beeinflussen. Der Artikel [Java Binding Metadata](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata) enthält weitere Informationen zu Transformationen, die angewendet werden können:

1. Erstellen Sie eine leere Datei " **Metadata. XML** ":

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <metadata>
    </metadata>
    ```

1. Definieren von XML-Transformationen:

- Die native-Umwandlungen von "Kotlin" hat zwei Abhängigkeiten, die Sie C# nicht für die Welt verfügbar machen möchten, definieren Sie zwei Transformationen, um Sie vollständig zu ignorieren Wichtig: die systemeigenen Member werden nicht aus der resultierenden Binärdatei entfernt, sondern C# es werden nur Klassen generiert. Der [Java-Compiler](http://java-decompiler.github.io/) kann verwendet werden, um die Abhängigkeiten zu identifizieren. Führen Sie das Tool aus, und öffnen Sie die zuvor erstellte Aar-Datei. als Ergebnis wird die Struktur des Android-Archivs angezeigt, in der alle Abhängigkeiten, Werte, Ressourcen, Manifests und Klassen reflektiert werden:  

    ![Java-compilerabhängigkeiten](walkthrough-images/java-decompiler-dependencies.png)

    Die Transformationen zum Überspringen der Verarbeitung dieser Pakete werden mithilfe von XPath-Anweisungen definiert:

    ```xml
    <remove-node path="/api/package[starts-with(@name,'org.jbox2d')]" />
    <remove-node path="/api/package[starts-with(@name,'org.slf4j')]" />
    ```

- Die native `BubblePicker`-Klasse verfügt über zwei Methoden `getBackgroundColor` und `setBackgroundColor`, und die folgende Transformation ändert Sie C# in eine `BackgroundColor`-Eigenschaft:

    ```xml
    <attr path="/api/package[@name='com.igalata.bubblepicker.rendering']/class[@name='BubblePicker']/method[@name='getBackground' and count(parameter)=0]" name="propertyName">BackgroundColor</attr>
    <attr path="/api/package[@name='com.igalata.bubblepicker.rendering']/class[@name='BubblePicker']/method[@name='setBackground' and count(parameter)=1 and parameter[1][@type='int']]" name="propertyName">BackgroundColor</attr>
    ```

- Nicht signierte Typen `UInt, UShort, ULong, UByte` die eine besondere Behandlung erfordern. Für diese Typen ändert sich der Name und die Parametertypen von "ventlin" automatisch, was sich im generierten Code widerspiegelt:

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

    Darüber hinaus werden auch verwandte Typen wie `UIntArray, UShortArray, ULongArray, UByteArray` von "Kotlin" beeinflusst. Der Methodenname wird so geändert, dass ein zusätzliches Suffix enthalten ist, und die Parameter werden in ein Array von Elementen mit signierten Versionen der gleichen Typen geändert. Im folgenden Beispiel wird ein Parameter vom Typ `UIntArray` automatisch in `int[]` konvertiert, und der Methodenname wird von `fooUIntArrayMethod` in `fooUIntArrayMethod--ajY-9A`geändert. Letzteres wird von xamarin. Android-Tools erkannt und als gültiger Methodenname generiert:

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

    Um ihm einen aussagekräftigen Namen zu geben, können die folgenden Metadaten der Datei " **Metadata. XML**" hinzugefügt werden, die den Namen wieder auf den ursprünglich im Kotlin-Code definierten Namen aktualisieren:

    ```xml
    <attr path="/api/package[@name='com.microsoft.simplekotlinlib']/class[@name='FooClass']/method[@name='fooUIntArrayMethod--ajY-9A']" name="managedName">fooUIntArrayMethod</attr>
    ```

    Im Beispiel bubblepicker gibt es keine Member, die nicht signierte Typen verwenden, sodass keine weiteren Änderungen erforderlich sind.

- Bei der Umwandlung von memlin-Elementen mit generischen Parametern werden standardmäßig die Parameter von Java`Lang.Object` Sorte. Beispielsweise hat eine Kotlin-Methode einen generischen Parameter \<t >:

    ```Kotlin
    public open fun <T>fooGenericMethod(value: T) : String {
    return "fooGenericMethod${value}"
    }
    ```

    Nachdem eine xamarin. Android-Bindung generiert wurde, wird die-Methode C# wie folgt für verfügbar gemacht:

    ```csharp
    [Register ("fooGenericMethod", "(Ljava/lang/Object;)Ljava/lang/String;", "GetFooGenericMethod_Ljava_lang_Object_Handler")]
    [JavaTypeParameters (new string[] {
        "T"
    })]

    public virtual string FooGenericMethod (Java.Lang.Object value);
    ```

    Java-und Kotlin-Generika werden von xamarin. Android-Bindungen nicht unterstützt, C# daher wird eine generalisierte Methode für den Zugriff auf die generische API erstellt. Als Problem Umgehung können Sie eine Wrapper-Kotlin-Bibliothek erstellen und erforderliche APIs in einer stark typisierten Weise ohne Generika verfügbar machen. Alternativ können Sie Hilfsprogramme auf C# der Seite erstellen, um das Problem auf die gleiche Weise wie über stark typisierte APIs zu beheben.

    > [!TIP]
    > Durch Transformieren der Metadaten können alle Änderungen auf die generierte Bindung angewendet werden. Im Artikel [Binden der Java-Bibliothek](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/) wird ausführlich erläutert, wie die Metadaten generiert und verarbeitet werden.

## <a name="build-a-binding-library"></a>Erstellen einer Bindungs Bibliothek

Der nächste Schritt besteht darin, ein xamarin. Android-Bindungs Projekt mithilfe der Visual Studio-Bindungs Vorlage zu erstellen, erforderliche Metadaten hinzuzufügen, Native Verweise zu erstellen und dann das Projekt zu erstellen, um eine verwendbare Bibliothek zu erstellen:

1. Öffnen Sie Visual Studio für Mac, und erstellen Sie ein neues xamarin. Android-Bindungs Bibliotheksprojekt, benennen Sie es mit einem Namen, in diesem Fall **testbubblepicker. Binding** , und schließen Sie den Assistenten ab. Die xamarin. Android-Bindungs Vorlage befindet sich unter folgendem Pfad: **Android-> Bibliothek > Bindungs Bibliothek**:

    ![Erstellen einer Bindung in Visual Studio](walkthrough-images/visual-studio-create-binding.png)

    Im Ordner Transformationen sind drei Haupt Transformations Dateien vorhanden:

    - **Metadata. XML** – ermöglicht Änderungen an der endgültigen API, z. b. das Ändern des Namespace der generierten Bindung.
    - **EnumFields. XML** – enthält die Zuordnung zwischen Java int-Konstanten C# und enumerationsenumeraten.
    - **EnumMethods. XML** – ermöglicht das Ändern von Methoden Parametern und Rückgabe Typen aus Java- C# int-Konstanten in Enumerationstypen.

    Behalten Sie die Dateien " **EnumFields. XML** " und " **EnumMethods. XML** " bei, und aktualisieren Sie die Datei " **Metadata. XML** ", um Ihre Transformationen

1. Ersetzen Sie die vorhandene Datei " **Transformations/Metadata. XML** " durch die Datei " **Metadata. XML** ", die im vorherigen Schritt erstellt wurde. Vergewissern Sie sich im Fenster Eigenschaften, dass die **Aktion Dateibuild** auf **transformationfile**festgelegt ist:

    ![Visual Studio-Metadaten](walkthrough-images/visual-studio-metadata.png)

1. Fügen Sie die Datei **bubblepicker-v 1.0. Aar** , die Sie in Schritt 1 erstellt haben, als Native Referenz zum Bindungs Projekt hinzu. Öffnen Sie Finder, und navigieren Sie zum Ordner mit dem Android-Archiv, um native Bibliotheks Verweise hinzuzufügen. Ziehen Sie das Archiv per Drag & amp; Drop in den Ordner Jars in Projektmappen-Explorer. Alternativ können Sie die Option Kontextmenü **Hinzufügen** im Ordner jar verwenden und **vorhandene Dateien**auswählen.... Wählen Sie diese exemplarische Vorgehensweise aus, um die Datei in das Verzeichnis zu kopieren. Stellen Sie sicher, dass die **Buildaktion** auf **libraryprojectzip**festgelegt ist:

    ![Native Referenz zu Visual Studio](walkthrough-images/visual-studio-native-reference.png)

1. Fügen Sie einen Verweis auf das [nuget-Paket xamarin. Kotlin. stdlib](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/) hinzu. Dieses Paket ist eine Bindung für die Kotlin-Standard Bibliothek. Ohne dieses Paket funktioniert die Bindung nur dann, wenn die "Kotlin"-Bibliothek keine für C# "Kotlin" spezifischen Typen verwendet; andernfalls werden alle diese Member nicht verfügbar gemacht, und jede APP, die versucht, die Bindung zu verwenden, stürzt zur Laufzeit ab.

    > [!TIP]
    > Aufgrund einer Einschränkung von xamarin. Android können Bindungs Tools nur ein einzelnes Android-Archiv (AAR) pro Bindungs Projekt hinzugefügt werden. Wenn mehrere Aar-Dateien eingeschlossen werden müssen, sind mehrere xamarin. Android-Projekte erforderlich, eine pro Aar. Wenn dies in dieser exemplarischen Vorgehensweise der Fall wäre, müssten die vorherigen vier Aktionen dieses Schritts für jedes Archiv wiederholt werden. Als alternative Möglichkeit können mehrere Android-Archive manuell als einzelnes Archiv zusammengeführt werden. Daher können Sie ein einzelnes xamarin. Android-Bindungs Projekt verwenden.

1. Die letzte Aktion besteht darin, die Bibliothek zu erstellen und keine Kompilierungsfehler zu erstellen. Bei Kompilierungs Fehlern können Sie mithilfe der Datei "Metadata. xml" adressiert und behandelt werden, die Sie zuvor erstellt haben, indem Sie XML-Transformations Metadaten hinzugefügt haben, die Bibliothekselemente hinzufügen, entfernen oder umbenennen.

## <a name="consume-the-binding-library"></a>Verwenden der Bindungs Bibliothek

Der letzte Schritt besteht darin, die xamarin. Android-Bindungs Bibliothek in einer xamarin. Android-Anwendung zu verwenden. Erstellen Sie ein neues xamarin. Android-Projekt, und fügen Sie einen Verweis auf die Benutzeroberfläche der Bindungs Bibliothek und die gerengerauswahl

1. Erstellen Sie ein xamarin. Android-Projekt. Verwenden Sie die **Android-> app > Android-App** als Ausgangspunkt, und wählen Sie die Option **neuester und größte** als Zielplattformen aus, um Kompatibilitätsprobleme zu vermeiden. Die folgenden Schritte sind für dieses Projekt als Ziel:

    ![Visual Studio-app erstellen](walkthrough-images/visual-studio-create-app.png)

1. Fügen Sie dem Bindungs Projekt einen Projekt Verweis hinzu, oder fügen Sie einen Verweis auf die zuvor erstellte DLL hinzu:

    ![Visual Studio-Bindungs Verweis hinzufügen. png](walkthrough-images/visual-studio-add-binding-reference.png)

1. Fügen Sie einen Verweis auf das [nuget-Paket xamarin. Kotlin. stdlib](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/) hinzu, das Sie zuvor dem xamarin. Android-Bindungs Projekt hinzugefügt haben. Es fügt Unterstützung für alle für die Laufzeit erforderlichen Typen von Kotlin hinzu.  Ohne dieses Paket kann die APP kompiliert werden, stürzt aber zur Laufzeit ab:

    ![Visual Studio hinzufügen von "stdlib" nuget](walkthrough-images/visual-studio-add-stdlib-nuget.png)

1. Fügen Sie dem Android-Layout für `MainActivity`das `BubblePicker`-Steuerelement hinzu. Öffnen Sie die Datei " **testbubblepicker/Resources/Layout/content_main. XML** ", und fügen Sie den bubblepicker-Steuerelement Knoten als letztes Element des root relativelayout-Steuer Elements an:

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

1. Aktualisieren Sie den Quellcode der APP, und fügen Sie die Initialisierungs Logik zum `MainActivity`hinzu, wodurch das Blasen Auswahl-SDK aktiviert wird:

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

    `BubblePickerAdapter` und `BubblePickerListener` sind zwei Klassen, die von Grund auf neu erstellt werden müssen, die die Blasen Daten und die Interaktion mit Daten steuern:

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

1. Führen Sie die APP aus, um die Benutzeroberfläche der Blasen Auswahl zu rendernden:

    ![Bubblepicker-Demo](walkthrough-images/bubble-picker-demo.png)

    Das Beispiel erfordert zusätzlichen Code zum Rendering von Elementen und zum Verarbeiten von Interaktionen, das `BubblePicker` Steuerelement wurde jedoch erfolgreich erstellt und aktiviert.

Glückwunsch! Sie haben erfolgreich eine xamarin. Android-App und eine Bindungs Bibliothek erstellt, die eine Kotlin-Bibliothek verwendet.  

Sie sollten jetzt über eine grundlegende xamarin. Android-Anwendung verfügen, die eine systemeigene-Datei mit einer Datei mit einer xamarin. Android-Bindungs Bibliothek verwendet. In dieser exemplarischen Vorgehensweise wird ein einfaches Beispiel verwendet, um die Schlüsselkonzepte, die eingeführt werden, besser hervorzuheben. In realen Szenarien müssen Sie wahrscheinlich eine größere Anzahl von APIs verfügbar machen und metadatentransformationen darauf anwenden.

## <a name="related-links"></a>Verwandte Links

- [Android Studio](https://developer.android.com/studio)
- [Gradle-Installation](https://gradle.org/install/)
- [Visual Studio für Mac](https://visualstudio.microsoft.com/downloads)
- [Java-compilercompiler](http://java-decompiler.github.io/)
- [Bubblepicker-Kotlin-Bibliothek](https://github.com/igalata/Bubble-Picker)
- [Binden der Java-Bibliothek](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/)
- [XPath](https://www.w3.org/TR/xpath/)
- [Metadaten der Java-Bindung](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata)
- [Xamarin. Kotlin. stdlib-nuget](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/)
- [Beispielprojektrepository](https://github.com/alexeystrakh/xamarin-binding-kotlin-framework)
