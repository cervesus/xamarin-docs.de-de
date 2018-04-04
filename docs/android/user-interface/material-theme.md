---
title: Design "Material"
description: Wie Design Ihrer app Xamarin.Android mit Material Design
ms.prod: xamarin
ms.assetid: DC4CDBD0-3DF9-4B7E-B876-29128985E2A7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: a3b5f908330833a38aad9e329835a4a437fc29f0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="material-theme"></a>Design "Material"

*Design "Material"* ist eine Benutzer-schnittstellenstil, die das Aussehen und Verhalten von Ansichten und Aktivitäten, die beginnend mit Android 5.0.x (Lollipop) bestimmt. Design "Material" ist in Android 5.0 integriert, damit sie vom System Benutzeroberfläche sowie von Anwendungen verwendet wird. Design "Material" ist nicht "Design" in dem Sinne, der eine systemweite Darstellung-Option, die ein Benutzer dynamisch aus einem Menü "Einstellungen" auswählen kann. Stattdessen kann Design "Material" als eine Gruppe von verwandten Basis Formatvorlagen betrachtet werden, die Sie verwenden können, um das Aussehen und Verhalten Ihrer App anzupassen.

Android bietet drei Arten von Design "Material":

-  `Theme.Material` &ndash; Dunkle Design "Material" Version; Dies ist der Standard-Typ in Android 5.0.

-  `Theme.Material.Light` &ndash; Vereinfachte Versionen von Design "Material".

-  `Theme.Material.Light.DarkActionBar` &ndash; Light-Version des Design "Material", jedoch mit einem dunklen Aktionsleiste.

Beispiele für diese Design "Material" Varianten werden hier angezeigt:

[![Beispiel-Screenshots der Design "dunkel", Design "hell" und Aktionsleiste dunklen Design](material-theme-images/three-flavors-sml.png)](material-theme-images/three-flavors.png#lightbox)

Sie können eine Ableitung von Design "Material", um ein eigenes Design erstellen einiger oder aller Farbe Attribute überschreiben. Sie können z. B. ein Design, das von abgeleitet ist erstellen `Theme.Material.Light`, aber die Farbe des app-Leiste entsprechend die Farbe des Ihre Marke überschreibt. Sie können auch einzelne Sichten formatieren; Sie können z. B. einen Stil für erstellen [CardView](~/android/user-interface/controls/card-view.md) , verfügt über mehr abgerundete Ecken und verwendet eine dunklere Hintergrundfarbe.

Können Sie eine einzelne Design für eine ganze app verwenden, oder können Sie verschiedene Designs für unterschiedliche Bildschirme (Aktivitäten) in einer app. In den oben genannten Screenshots verwendet z. B. eine einzelne app Design für jede Aktivität integrierten Farbschemata veranschaulicht. Optionsfelder wechseln Sie die app zu anderen Aktivitäten und folglich verschiedene Designs anzuzeigen.

Da Design "Material" nur für Android 5.0 und höher unterstützt wird, können nicht Sie sie (oder ein benutzerdefiniertes Design, das Design "Material" abgeleitet) zum Design Ihrer app verwenden zum Ausführen unter früheren Versionen von Android. Allerdings können Sie konfigurieren, der app um Material Design für Geräte mit Android 5.0 und ordnungsgemäß ein Fallback auf einen früheren Design bei der Ausführung in älteren Versionen von Android (finden Sie unter der [Kompatibilität](#compatibility) Abschnitt dieses Artikels Einzelheiten).


## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um die neuen Android 5.0 Material Design-Funktionen in Xamarin-basierten apps zu verwenden:

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 or later must be installed and configured with either Visual Studio or Visual Studio for Mac. 

-  **Android SDK** &ndash; Android 5.0.x (API 21) oder höher muss installiert sein über den Android SDK Manager.

-  **Java JDK 1.8** &ndash; JDK 1.7 genutzt werden, wenn Sie ausdrücklich Anwendung-API-Ebene 23 und früher sind. JDK 1.8 steht über [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

So konfigurieren Sie ein Android 5.0-app-Projekt finden Sie unter [Einrichten einer Android 5.0-Projekt](~/android/platform/lollipop.md).


## <a name="using-the-built-in-themes"></a>Verwenden von integrierten Designs

Die einfachste Möglichkeit zum Design "Material" verwenden, ist zum Konfigurieren Ihrer Anwendung um ein integriertes Design ohne Anpassung verwenden. Wenn Sie nicht explizit ein Design konfigurieren möchten, Ihre app wird standardmäßig `Theme.Material` (das Design "dunkel"). Wenn Ihre app nur eine Aktivität verfügt, können Sie ein Design auf Anwendungsebene konfigurieren. Verfügt Ihre Anwendung mehrere Aktivitäten, können Sie ein Design auf Anwendungsebene konfigurieren, damit er dasselbe Design bei allen Aktivitäten verwendet oder können Sie verschiedene Aktivitäten unterschiedliche Designs zuweisen. In den folgenden Abschnitten wird erläutert, wie Designs auf app-Ebene und die auf Aktivitätsebene konfigurieren.


### <a name="theming-an-application"></a>Designumgebung einer Anwendung

Zum Konfigurieren einer vollständigen Anwendung mit einem Design "Material" Flavor legen die `android:theme` Attribut des den Anwendungsknoten in **AndroidManifest.xml** auf einen der folgenden:

-  `@android:style/Theme.Material` &ndash; Design "dunkel".

-  `@android:style/Theme.Material.Light` &ndash; Design "hell".

-  `@android:style/Theme.Material.Light.DarkActionBar` &ndash; Design "hell" mit dunkel Aktionsleiste.

Das folgende Beispiel konfiguriert die Anwendung *"MyApp"* das Design "hell" verwenden:

```xml
<application android:label="MyApp" 
             android:theme="@android:style/Theme.Material.Light">
</application>
```

Alternativ können Sie die Anwendung festlegen `Theme` -Attribut im **AssemblyInfo.cs** (oder **Properties.cs**). Zum Beispiel:

```C#
[assembly: Application(Theme="@android:style/Theme.Material.Light")]
```

Wenn das Design "Anwendung" festgelegt ist, um `@android:style/Theme.Material.Light`, jeder Aktivität in *"MyApp"* wird `Theme.Material.Light`.


### <a name="theming-an-activity"></a>Designumgebung einer Aktivität

Design eine Aktivität, fügen Sie eine `Theme` festlegen auf die `[Activity]` über Ihre Aktivität Deklaration-Attributs aus, und weisen Sie `Theme` an das Design "Material" Flavor, die Sie verwenden möchten. Das folgende Beispiel-Designs eine Aktivität mit `Theme.Material.Light`:

```C#
[Activity(Theme = "@android:style/Theme.Material.Light",
          Label = "MyApp", MainLauncher = true, Icon = "@drawable/icon")]  
```

Andere Aktivitäten in dieser app verwendet die standardmäßige `Theme.Material` Dunkles Farbschema (oder, wenn konfiguriert, die anwendungseinstellung Design ").

<a name="customtheme" />

## <a name="using-custom-themes"></a>Verwenden von benutzerdefinierten Designs

Sie können Ihre Marke erhöhen, indem Sie ein benutzerdefiniertes Design, das Ihrer app mit Ihrem Organisations-Stile erstellen&rsquo;s Farben. Um ein benutzerdefiniertes Design zu erstellen, definieren Sie eine neue Formatvorlage, die eine integrierte Design "Material" Flavor abgeleitet überschreiben die Color-Attribute, die Sie ändern möchten. Sie können z. B. ein benutzerdefiniertes Design, das von abgeleitet ist definieren `Theme.Material.Light.DarkActionBar` und ändert die Hintergrundfarbe für den Bildschirm Beige anstelle von Leerzeichen.

Design "Material" macht die folgenden Layoutattribute für die Anpassung an:

-  `colorPrimary` &ndash; Die Farbe des app-Leiste.

-  `colorPrimaryDark` &ndash; Die Farbe der Statusleiste und kontextabhängige app-leisten; Dies ist normalerweise eine dunkle Version `colorPrimary`.

-  `colorAccent` &ndash; Die Farbe des Benutzeroberflächen-Steuerelemente, z. B. Kontrollkästchen, Optionsfelder und Textfelder bearbeiten.

-  `windowBackground` &ndash; Die Farbe des Hintergrunds Bildschirm.

-  `textColorPrimary` &ndash; Die Farbe der Benutzeroberflächentext in der app-Leiste.

-  `statusBarColor` &ndash; Die Farbe der Statusleiste.

-  `navigationBarColor` &ndash; Die Farbe der Navigationsleiste.

In der folgenden Abbildung sind diese Bereiche Bildschirm bezeichnet:

[![Darstellung der Attribute und ihre zugehörigen Bildschirm Bereiche](material-theme-images/screen-attributes-sml.png)](material-theme-images/screen-attributes.png#lightbox)

Standardmäßig `statusBarColor` festgelegt ist, auf den Wert des `colorPrimaryDark`. Sie können festlegen, `statusBarColor` zu einer Volltonfarbe aus, oder Sie legen sie den `@android:color/transparent` auf die Statusleiste transparent zu gestalten. Die Navigationsleiste kann auch erfolgen transparent durch Festlegen von `navigationBarColor` auf `@android:color/transparent`.


### <a name="creating-a-custom-app-theme"></a>Erstellen eine benutzerdefinierte App-Design

Sie können eine benutzerdefinierte app-Design erstellen, indem erstellen und Ändern von Dateien in den **Ressourcen** Ordner des app-Projekts. Um Ihre app mit einem benutzerdefinierten Design zu formatieren, verwenden Sie die folgenden Schritte aus:

-   Erstellen einer **"Colors.xml"** Datei **Ressourcen/Werte** &mdash; Sie diese Datei verwenden, um Ihre benutzerdefinierte Designfarben definieren. Sie können z. B. den folgenden Code in einfügen **"Colors.xml"** , die Ihnen beim Einstieg helfen:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <color name="my_blue">#3498DB</color>
    <color name="my_green">#77D065</color>
    <color name="my_purple">#B455B6</color>
    <color name="my_gray">#738182</color>
</resources>
```

-   Ändern Sie diese Beispieldatei zum definieren, die Namen und die Farbcodes für Farbe-Ressourcen, die Sie in einem benutzerdefinierten Design verwenden.

-   Erstellen einer **Ressourcen/Werte-v21** Ordner. In diesem Ordner erstellen einen **styles.xml** Datei:

    [![Speicherort der styles.xml im Ordner Ressourcen/Werte-21.xml](material-theme-images/values-v21-sml.png)](material-theme-images/values-v21.png#lightbox)

    Beachten Sie, dass **Ressourcen/Werte-v21** gilt nur für Android 5.0 &ndash; ältere Versionen von Android werden nicht gelesen, Dateien in diesem Ordner.

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

-   An diesem Punkt eine app, die verwendet *MyCustomTheme* zeigt die Stock `Theme.Material.Light` Design ohne Anpassungen:

    [![Benutzerdefiniertes Design Darstellung vor Anpassungen](material-theme-images/custom-theme-before-sml.png)](material-theme-images/custom-theme-before.png#lightbox)

-   Hinzufügen von Anpassungen von Farbe in **styles.xml** durch Definieren der Farben der Layoutattribute, die Sie ändern möchten. Z. B. so ändern Sie die Farbe des app-Leiste an, `my_blue` und ändern Sie die Farbe von UI-Steuerelementen zu `my_purple`, fügen Sie die Farbe, überschreibungen, um **styles.xml** mit Verweisen auf Farbressourcen im konfigurierten **"Colors.xml"**:

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

Mit diesen Änderungen vorhanden, eine app, die verwendet *MyCustomTheme* zeigt eine app-Leiste Farbe im `my_blue` und UI-Steuerelemente in `my_purple`, verwenden jedoch den `Theme.Material.Light` Farbschema anderen Orten wegzulassen:

[![Benutzerdefiniertes Design Darstellung nach Anpassungen](material-theme-images/custom-theme-after-sml.png)](material-theme-images/custom-theme-after.png#lightbox)

In diesem Beispiel *MyCustomTheme* Grunde verwendet Farben aus `Theme.Material.Light` für den Hintergrund Farbe, Statusleiste, und Textfarben, aber es ändert sich die Farbe der app-Leiste auf `my_blue` und legt die Farbe des Optionsfelds, `my_purple`.

<a name="customview" />

### <a name="creating-a-custom-view-style"></a>Erstellen einen benutzerdefinierte Ansicht-Stil

Android 5.0.x erleichtert auch möglich, eine einzelne Ansicht zu formatieren. Nach der Erstellung **"Colors.xml"** und **styles.xml** (wie im vorherigen Abschnitt beschrieben), können Sie einen Ansichtsstil hinzufügen **styles.xml**.
Um eine einzelne Ansicht zu formatieren, verwenden Sie die folgenden Schritte aus:

-   Bearbeiten Sie **Resources/values-v21/styles.xml** und Hinzufügen einer `style` Knoten mit dem Namen des Formats benutzerdefinierte Ansicht. Legen Sie die benutzerdefinierte Farbe Attribute für die Ansicht in dieser `style` Knoten. Z. B. erstellen eine benutzerdefinierten [CardView](~/android/user-interface/controls/card-view.md) Format, das mehr Ecken und verwendet abgerundeten `my_blue` als Hintergrundfarbe Karte hinzufügen eine `style` Knoten **styles.xml** (innerhalb der `resources`Knoten) und den Background-Farbe und die Ecke Radius konfigurieren:

```xml
<!-- Theme an individual view: -->
<style name="CardView.MyBlue">

    <!-- Change the background color to Xamarin blue: -->
    <item name="cardBackgroundColor">@color/my_blue</item>

    <!-- Make the corners very round: -->
    <item name="cardCornerRadius">18dp</item>
</style>
```

-   Legen Sie das Layout der `style` Attribut für diese Sicht mit dem benutzerdefinierten Stil-Namen übereinstimmen, den Sie im vorherigen Schritt ausgewählt haben. Zum Beispiel:

```xml
<android.support.v7.widget.CardView
    style="@style/CardView.MyBlue"
    android:layout_width="200dp"
    android:layout_height="100dp"
    android:layout_gravity="center_horizontal">
```

Der folgende Screenshot veranschaulicht, die Standardeinstellung `CardView` (gezeigt auf der linken Seite) heißt wie im Vergleich zu einer `CardView` hat, die mit dem benutzerdefinierten formatiert wurden `CardView.MyBlue` Design (auf der rechten Seite dargestellt):

[![Beispiele für Standard CardView und benutzerdefinierte CardView](material-theme-images/custom-cardview-sml.png)](material-theme-images/custom-cardview.png#lightbox)

In diesem Beispiel wird die benutzerdefinierte `CardView` wird angezeigt, wobei die Farbe des Hintergrunds `my_blue` und einen Eckradius 18dp.


## <a name="compatibility"></a>Kompatibilität

Verwenden Sie zum Formatieren der app, damit er wird auf Material Design für Android 5.0, aber zu einem nach unten weisenden-kompatiblen-Stil auf ältere Android automatisch zurückgesetzt, die folgenden Schritte aus:

-   Definieren Sie ein benutzerdefiniertes Design in **Resources/values-v21/styles.xml** abgeleitet, die einen Material Design-Stil. Zum Beispiel:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

-   Definieren Sie ein benutzerdefiniertes Design in **Resources/values/styles.xml** , leitet sich von einer älteren Design jedoch den gleichen Designnamen wie oben beschrieben verwendet. Zum Beispiel:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Holo.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

-   In **AndroidManifest.xml**, konfigurieren Sie Ihre app mit dem Namen des benutzerdefinierten Designs. 
    Zum Beispiel:

```xml
<application android:label="MyApp" 
             android:theme="@style/MyCustomTheme">
</application>
```

-   Alternativ können Sie eine bestimmte Aktivität, die mit Ihrer benutzerdefinierten Design formatieren:

```C#
[Activity(Label = "MyActivity", Theme = "@style/MyCustomTheme")]
```

Wenn Ihr Design in definierte Farben verwendet eine **"Colors.xml"** file, achten Sie darauf, dass Sie diese Datei in **Ressourcen/Werte** (statt **Ressourcen/Werte-v21**), damit beide Versionen von Ihr benutzerdefinierte Design kann Ihre Farbe Definitionen zugreifen.

Wenn Ihre app auf einem Android 5.0-Gerät ausgeführt wird, verwenden Sie das Designdefinition, die im angegebenen **Resources/values-v21/styles.xml**. Wenn diese app auf älteren Android-Geräten ausgeführt wird, es wird automatisch ein Fallback auf der Designdefinition, die im angegebenen **Resources/values/styles.xml**.

Weitere Informationen zum Design-Kompatibilität mit älteren Android-Versionen finden Sie unter [alternativen Ressourcen](~/android/app-fundamentals/resources-in-android/alternate-resources.md).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel eingeführt, die neue Design "Material" Benutzer Schnittstelle Formatvorlage im Android 5.0.x (Lollipop) enthalten. Es beschrieben der drei integrierten Design "Material"-Typen, die Sie zum Formatieren der app verwenden können, es wird erläutert, wie ein benutzerdefiniertes Design für das branding Ihrer app zu erstellen und sie ein Beispiel für die Design eine einzelne Ansicht bereitgestellt. Schließlich wurde in diesem Artikel erläutert, wie Design "Material" in Ihrer app zu verwenden, und gleichzeitig aus Gründen der Abwärtskompatibilität mit früheren Versionen von Android werden.



## <a name="related-links"></a>Verwandte Links

- [ThemeSwitcher (Beispiel)](https://developer.xamarin.com/samples/monodroid/android5.0/ThemeSwitcher)
- [Einführung in die Lollipop](~/android/platform/lollipop.md)
- [CardView](~/android/user-interface/controls/card-view.md)
- [Alternative Ressourcen](~/android/app-fundamentals/resources-in-android/alternate-resources.md)
- [Entwicklervorschau für Android L](http://developer.android.com/preview/index.html)
- [Material Entwurf](http://developer.android.com/preview/material/index.html)
- [Material Entwurfsprinzipien](http://static.googleusercontent.com/media/www.google.com/en/us/design/material-design.pdf)
- [Kompatibilität](http://developer.android.com/preview/material/compatibility.html)
