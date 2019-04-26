---
title: Materialdesign
description: Wie Sie das Design Ihrer Xamarin.Android-app mit Material Design
ms.prod: xamarin
ms.assetid: DC4CDBD0-3DF9-4B7E-B876-29128985E2A7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 512775864f5ad55ddfedd53b83dd02d7b0e1d1f8
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61309094"
---
# <a name="material-theme"></a>Materialdesign

*Materialdesign* ist eine Art der Benutzer-Schnittstelle, die das Aussehen und Verhalten von Ansichten und Aktivitäten, die ab Android 5.0 (Lollipop) bestimmt. Material Design ist in Android 5.0 integriert, damit sie vom System Benutzeroberfläche sowie von Anwendungen verwendet wird. Material Design ist nicht "Design" im Sinne einer systemweiten Darstellung-Option, die ein Benutzer dynamisch aus einem Menü "Einstellungen" auswählen können. Stattdessen kann Material Design als eine Gruppe von zugehörigen integrierten Basis Formatvorlagen betrachtet werden, die Sie verwenden können, um das Aussehen und Verhalten Ihrer App anzupassen.

Android bietet drei Arten von Material Design:

-  `Theme.Material` &ndash; Dunkel Version des Material Design; Dies ist die Standard-Konfiguration in Android 5.0.

-  `Theme.Material.Light` &ndash; Light-Version von Material Design.

-  `Theme.Material.Light.DarkActionBar` &ndash; Light-Version des Material Design, jedoch mit einem dunklen Aktionsleiste.

Beispiele für diese Material Design-Varianten werden hier angezeigt:

[![Beispiel-Screenshot, der das Design "dunkel" Design "hell" und Aktionsleiste Dunkles Design](material-theme-images/three-flavors-sml.png)](material-theme-images/three-flavors.png#lightbox)

Sie können eine Ableitung von Material Design zum Erstellen eigener Designs, einige oder alle Farbattributen überschreiben. Sie können z. B. ein Design, das von abgeleitet ist erstellen `Theme.Material.Light`, aber überschreibt die Farbe des app um die Farbe der Ihre Marke zu entsprechen. Sie können auch einzelne Ansichten formatieren; Sie können z. B. erstellen ein Stils für [CardView](~/android/user-interface/controls/card-view.md) , mehr abgerundete Ecken aufweist, und verwendet eine dunklere Farbe des Hintergrunds.

Können Sie ein einzelnes Design für eine gesamte Anwendung oder können Sie verschiedene Designs für unterschiedliche Bildschirme (Aktivitäten) in einer app. In den obigen Screenshots verwendet z. B. eine einzelne app für jede Aktivität ein anderes Design integrierten Farbschemata veranschaulicht. Optionsfelder wechseln Sie die app an verschiedene Aktivitäten, und daher zeigen verschiedene Designs.

Da Material Design nur unter Android 5.0 und höher unterstützt wird, können nicht Sie (oder ein benutzerdefiniertes Design Material Design abgeleitet) auf das Design Ihrer app verwenden für die Ausführung in früheren Versionen von Android. Allerdings können Sie Ihre app, um Material Design auf Android 5.0-Geräte und ordnungsgemäß ein Fallback auf einen früheren Design bei der Ausführung in älteren Versionen von Android konfigurieren (finden Sie unter den [Kompatibilität](#compatibility) Abschnitt dieses Artikels Informationen).


## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um die neue Android 5.0 Material Design-Features in Xamarin-basierte apps verwenden:

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 or later must be installed and configured with either Visual Studio or Visual Studio for Mac. 

-  **Android SDK** &ndash; Android 5.0 (API 21) oder höher muss installiert sein über den Android SDK Manager.

-  **Java JDK 1.8** &ndash; JDK 1.7 kann verwendet werden, wenn Sie speziell für die API-Ebene 23 und früher sind. JDK 1.8 steht [Oracle](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

Vorgehensweise: Konfigurieren Sie ein Android 5.0-app-Projekt finden Sie unter [sich ein Android 5.0-Projekt](~/android/platform/lollipop.md).


## <a name="using-the-built-in-themes"></a>Verwenden von integrierten Designs

Die einfachste Möglichkeit zum Verwenden von Material Design ist so konfigurieren Sie Ihre app, um eine integrierte Design ohne Anpassung verwenden. Wenn Sie nicht explizit ein Design konfigurieren möchten, Ihre app wird standardmäßig `Theme.Material` (das dunkle Design). Wenn Ihre Anwendung nur eine Aktivität verfügt, können Sie ein Design auf der Aktivitätsebene konfigurieren. Wenn Ihre app mehrere Aktivitäten aufweist, können Sie ein Design auf Anwendungsebene konfigurieren, damit dasselbe Design in allen Aktivitäten verwendet, oder Sie verschiedene Designs, um verschiedene Aktivitäten zuweisen können. In den folgenden Abschnitten wird erläutert, wie Designs auf app-Ebene und auf der Aktivitätsebene konfigurieren.


### <a name="theming-an-application"></a>Design einer Anwendung

Um eine gesamte Anwendung in ein Material Design-Typ verwenden konfigurieren zu können, legen die `android:theme` Attribut den Anwendungsknoten in **"androidmanifest.xml"** auf einen der folgenden:

-  `@android:style/Theme.Material` &ndash; Design "dunkel".

-  `@android:style/Theme.Material.Light` &ndash; Design "hell".

-  `@android:style/Theme.Material.Light.DarkActionBar` &ndash; Design "hell" mit der dunklen Aktionsleiste.

Im folgenden Beispiel wird die Anwendung *"MyApp"* das Design "hell" verwenden:

```xml
<application android:label="MyApp" 
             android:theme="@android:style/Theme.Material.Light">
</application>
```

Alternativ können Sie festlegen, die Anwendung `Theme` -Attribut im **"AssemblyInfo.cs"** (oder **Properties.cs**). Zum Beispiel:

```C#
[assembly: Application(Theme="@android:style/Theme.Material.Light")]
```

Wenn das Anwendungsdesign festgelegte `@android:style/Theme.Material.Light`, jede Aktivität in *"MyApp"* erscheint mit `Theme.Material.Light`.


### <a name="theming-an-activity"></a>Designs mit einer Aktivität

Design eine Aktivität, die Sie hinzufügen eine `Theme` zum Festlegen der `[Activity]` Attribut oberhalb der Deklaration der Aktivität, und weisen Sie `Theme` auf das Material Design-Konfiguration, die Sie verwenden möchten. Das folgende Beispiel-Designs, eine Aktivität mit `Theme.Material.Light`:

```C#
[Activity(Theme = "@android:style/Theme.Material.Light",
          Label = "MyApp", MainLauncher = true, Icon = "@drawable/icon")]  
```

Andere Aktivitäten in dieser app verwenden die standardmäßige `Theme.Material` Dunkles Farbschema (oder, falls konfiguriert, die Einstellung für Design).

<a name="customtheme" />

## <a name="using-custom-themes"></a>Verwenden von benutzerdefinierten Designs

Sie können Ihre Marke verbessern, indem das Erstellen eines benutzerdefinierten Designs, die Ihre app an Ihre Marke anpassen formatiert&rsquo;s Farben. Um ein benutzerdefiniertes Design zu erstellen, definieren Sie ein neues Format, das leitet sich von einem integrierten Material Design-Typ, überschreiben die Farbe-Attribute, die Sie ändern möchten. Sie können z. B. definieren ein benutzerdefiniertes Designs, die von abgeleitet `Theme.Material.Light.DarkActionBar` und ändert die Hintergrundfarbe des Beige statt weiß.

Material Design macht die folgenden Layoutattribute für die Anpassung:

-  `colorPrimary` &ndash; Die Farbe von der app-Leiste.

-  `colorPrimaryDark` &ndash; Die Farbe der Statusleiste und kontextbezogene app-leisten; Dies ist normalerweise eine dunkle Version `colorPrimary`.

-  `colorAccent` &ndash; Die Farbe des UI-Steuerelemente wie Kontrollkästchen, Optionsfelder, und Bearbeiten von Textfeldern.

-  `windowBackground` &ndash; Die Farbe des Hintergrunds Bildschirm.

-  `textColorPrimary` &ndash; Die Farbe des Benutzeroberflächen-Text in der app-Leiste.

-  `statusBarColor` &ndash; Die Farbe der Statusleiste.

-  `navigationBarColor` &ndash; Die Farbe der Navigationsleiste.

In der folgenden Abbildung sind die folgenden Bildschirmbereiche bezeichnet:

[![Diagramm der Attribute und ihre zugeordneten Bildschirmbereiche](material-theme-images/screen-attributes-sml.png)](material-theme-images/screen-attributes.png#lightbox)

In der Standardeinstellung `statusBarColor` festgelegt ist, auf den Wert der `colorPrimaryDark`. Sie können festlegen, `statusBarColor` auf eine Volltonfarbe, oder Sie können sie festlegen, um `@android:color/transparent` auf die Statusleiste transparent zu gestalten. Die Navigationsleiste kann auch erfolgen transparent durch Festlegen von `navigationBarColor` zu `@android:color/transparent`.


### <a name="creating-a-custom-app-theme"></a>Erstellen eines benutzerdefinierten App-Designs

Sie können eine benutzerdefinierte app-Design erstellen, indem erstellen und Ändern von Dateien in die **Ressourcen** Ordner des app-Projekts. Um Ihre app mit einem benutzerdefinierten Design zu formatieren, verwenden Sie die folgenden Schritte aus:

-   Erstellen einer **"Colors.xml"** Datei **Ressourcen/Values** &mdash; Sie diese Datei verwenden, um Ihre benutzerdefinierten Designfarben zu definieren. Sie können z. B. den folgenden Code einfügen **"Colors.xml"** , die Ihnen beim Einstieg helfen:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <color name="my_blue">#3498DB</color>
    <color name="my_green">#77D065</color>
    <color name="my_purple">#B455B6</color>
    <color name="my_gray">#738182</color>
</resources>
```

-   Ändern Sie diese Beispieldatei, um die Namen und die Farbcodes für Farbe-Ressourcen, die Sie in einem benutzerdefinierten Design verwenden zu definieren.

-   Erstellen Sie eine **Ressourcen/Werte-v21** Ordner. Erstellen Sie in diesem Ordner eine **styles.xml** Datei:

    [![Speicherort der styles.xml im Ordner "Ressourcen/Werte-21.xml"](material-theme-images/values-v21-sml.png)](material-theme-images/values-v21.png#lightbox)

    Beachten Sie, dass **Ressourcen/Werte-v21** bezieht sich auf Android 5.0 &ndash; Dateien in diesem Ordner werden von ältere Versionen von Android nicht gelesen.

-   Hinzufügen einer `resources` Knoten **styles.xml** und definieren Sie eine `style` Knoten mit dem Namen des benutzerdefinierten Designs. Hier ist z. B. eine **styles.xml** Datei, die definiert *MyCustomTheme* (abgeleitet von der integrierten `Theme.Material.Light` Designstil):

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <!-- Inherit from the light Material Theme -->
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Customizations go here -->
    </style>
</resources>
```

-   An diesem Punkt eine app, *MyCustomTheme* zeigt die Stock `Theme.Material.Light` Design ohne Anpassungen:

    [![Benutzerdefiniertes Design aussehen, bevor Sie Anpassungen](material-theme-images/custom-theme-before-sml.png)](material-theme-images/custom-theme-before.png#lightbox)

-   Hinzufügen von Anpassungen von Farbe in **styles.xml** definieren die Farben der Layoutattribute, die Sie ändern möchten. So ändern Sie die Farbe des app-Leiste an, beispielsweise `my_blue` , und ändern Sie die Farbe des Benutzeroberflächen-Steuerelemente `my_purple`, fügen Sie die Farbe, überschreibungen, um **styles.xml** mit Verweisen auf im konfigurierten Farbressourcen **"Colors.xml"**:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <!-- Inherit from the light Material Theme -->
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Override the app bar color -->
        <item name="android:colorPrimary">@color/my_blue</item>
        <!-- Override the color of UI controls -->
        <item name="android:colorAccent">@color/my_purple</item>
    </style>
</resources>
```

Mit diesen Änderungen vorhanden, eine app, die verwendet *MyCustomTheme* zeigt eine app-Leiste Farbe in `my_blue` und UI-Steuerelemente in `my_purple`, aber die `Theme.Material.Light` Farbschema alle anderen Elemente:

[![Benutzerdefiniertes Design-Darstellung nach Anpassungen](material-theme-images/custom-theme-after-sml.png)](material-theme-images/custom-theme-after.png#lightbox)

In diesem Beispiel *MyCustomTheme* verwendet Farben aus `Theme.Material.Light` für den Hintergrund der Farbe, Statusleiste und Textfarben, sondern ändert sich die Farbe der app-Leiste auf `my_blue` und legt die Farbe des Optionsfelds, `my_purple`.

<a name="customview" />

### <a name="creating-a-custom-view-style"></a>Erstellen einen Stil für die benutzerdefinierte Ansicht

Android 5.0 erleichtert auch möglich, eine einzelne Ansicht zu formatieren. Nach der Erstellung **"Colors.xml"** und **styles.xml** (wie im vorherigen Abschnitt beschrieben), können Sie einen Stil anzeigen, hinzufügen **styles.xml**.
Um eine einzelne Ansicht zu formatieren, verwenden Sie die folgenden Schritte aus:

-   Bearbeiten Sie **Resources/values-v21/styles.xml** und Hinzufügen einer `style` Knoten mit dem Namen Ihres Stils benutzerdefinierte Ansicht. Legen Sie die benutzerdefinierte Farbe Attribute für die Ansicht in dieser `style` Knoten. Beispielsweise erstellen Sie eine benutzerdefinierte [CardView](~/android/user-interface/controls/card-view.md) Stil, der mehr Ecken und verwendet gerundet wurde `my_blue` als der Hintergrundfarbe der Karte, Hinzufügen einer `style` Knoten **styles.xml** (innerhalb der `resources`Knoten) und den Background-Farbe und die Ecke Radius konfigurieren:

```xml
<!-- Theme an individual view: -->
<style name="CardView.MyBlue">

    <!-- Change the background color to Xamarin blue: -->
    <item name="cardBackgroundColor">@color/my_blue</item>

    <!-- Make the corners very round: -->
    <item name="cardCornerRadius">18dp</item>
</style>
```

-   Legen Sie im Layout der `style` Attribut für diese Ansicht mit dem benutzerdefinierten Stil-Namen übereinstimmen, den Sie im vorherigen Schritt ausgewählt haben. Zum Beispiel:

```xml
<android.support.v7.widget.CardView
    style="@style/CardView.MyBlue"
    android:layout_width="200dp"
    android:layout_height="100dp"
    android:layout_gravity="center_horizontal">
```

Der folgende Screenshot enthält ein Beispiel des standardmäßigen `CardView` (dargestellt auf der linken Seite) heißt wie im Vergleich zu einem `CardView` hat, die mit dem benutzerdefinierten formatiert wurde `CardView.MyBlue` Design (auf der rechten Seite dargestellt):

[![Beispiele für standardmäßige CardView und benutzerdefinierte CardView-](material-theme-images/custom-cardview-sml.png)](material-theme-images/custom-cardview.png#lightbox)

In diesem Beispiel wird die benutzerdefinierte `CardView` wird angezeigt, mit der Hintergrundfarbe `my_blue` und einen Eckradius 18dp.


## <a name="compatibility"></a>Kompatibilität

Um Ihre app formatieren, damit es Material Design in Android 5.0 verwendet, jedoch auf ein nach unten-kompatiblen Format in älteren Android-Versionen automatisch zurückgesetzt, verwenden Sie die folgenden Schritte aus:

-   Definieren ein benutzerdefiniertes Designs in **Resources/values-v21/styles.xml** abgeleitet, die einen Material Design-Stil. Zum Beispiel:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

-   Definieren ein benutzerdefiniertes Designs in **Resources/values/styles.xml** , die aus einer älteren Design abgeleitet wird, aber der gleiche Name des Designs wie oben beschrieben verwendet. Zum Beispiel:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Holo.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

-   In **"androidmanifest.xml"**, konfigurieren Sie Ihre app mit dem Namen der benutzerdefinierten Designs. 
    Zum Beispiel:

```xml
<application android:label="MyApp" 
             android:theme="@style/MyCustomTheme">
</application>
```

-   Alternativ können Sie eine bestimmte Aktivität, die mit Ihrer benutzerdefinierten Designs formatieren:

```C#
[Activity(Label = "MyActivity", Theme = "@style/MyCustomTheme")]
```

Wenn Ihr Design in definierte Farben verwendet eine **"Colors.xml"** Datei, achten Sie darauf, dass Sie diese Datei in **Ressourcen/Values** (statt **Ressourcen/Werte-v21**), damit beide Versionen von Ihr benutzerdefinierte Design kann die Color-Definitionen zugreifen.

Wenn Ihre app auf einem Android 5.0-Gerät ausgeführt wird, verwenden Sie die Designdefinition, die im angegebenen **Resources/values-v21/styles.xml**. Wenn diese app auf älteren Android-Geräten ausgeführt wird, es wird automatisch ein Fallback auf die Designdefinition, die im angegebenen **Resources/values/styles.xml**.

Weitere Informationen über die Design-Kompatibilität mit älteren Android-Versionen finden Sie unter [alternative Ressourcen](~/android/app-fundamentals/resources-in-android/alternate-resources.md).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel eingeführt, die neuen Material Design Benutzer schnittstellenstil in Android 5.0 (Lollipop) enthalten. Es wurde beschrieben, die drei integrierten Material Design-Typen, die Sie zum Formatieren der app verwenden können, es wurde erklärt, wie ein benutzerdefiniertes Design für das branding Ihrer app zu erstellen und sie ein Beispiel dafür, wie Design eine einzelne Ansicht bereitgestellt. Schließlich wurde in diesem Artikel erläutert, wie Material Design in Ihrer app zu verwenden, gleichzeitig aus Gründen der Abwärtskompatibilität mit älteren Versionen von Android werden.



## <a name="related-links"></a>Verwandte Links

- [ThemeSwitcher (Beispiel)](https://developer.xamarin.com/samples/monodroid/android5.0/ThemeSwitcher)
- [Einführung in die Lollipop](../platform/lollipop.md)
- [CardView](controls/card-view.md)
- [Alternative Ressourcen](../app-fundamentals/resources-in-android/alternate-resources.md)
- [Android Lollipop](https://developer.android.com/about/versions/lollipop)
- [Kreis mit Android-Entwickler](https://developer.android.com/about/versions/pie/)
- [Materialdesign](https://developer.android.com/guide/topics/ui/look-and-feel/)
- [Material Entwurfsprinzipien](https://material.io/design/)
- [Verwalten der Anwendungskompatibilität](https://developer.android.com/training/backward-compatible-ui/)
