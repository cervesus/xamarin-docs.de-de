---
title: Materialdesign
description: 'Gewusst wie: Design Ihrer xamarin. Android-App mit Material Design'
ms.prod: xamarin
ms.assetid: DC4CDBD0-3DF9-4B7E-B876-29128985E2A7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 809f6241b3a17f63fe3077f896095c303e1dfd2e
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78916751"
---
# <a name="material-theme"></a>Materialdesign

Das *Material* Design ist ein Benutzeroberflächen Stil, der das Erscheinungsbild von Ansichten und Aktivitäten bestimmt, beginnend mit Android 5,0 (Lollipop). Das Material Design ist in Android 5,0 integriert, sodass es von der Benutzeroberfläche des Systems sowie von Anwendungen verwendet wird. Das Material Design ist kein "Design" im Sinne einer systemweiten Darstellungs Option, die ein Benutzer dynamisch aus dem Menü "Einstellungen" auswählen kann. Vielmehr kann das Material Design als ein Satz verwandter, integrierter Basis Stile betrachtet werden, die Sie zum Anpassen des Erscheinungsbild Ihrer APP verwenden können.

Android bietet drei Material Design-Varianten:

- `Theme.Material` &ndash; dunkle Version des Material Designs. Dies ist die Standardkonfiguration in Android 5,0.

- `Theme.Material.Light` &ndash; Light-Version des Material Designs.

- `Theme.Material.Light.DarkActionBar` &ndash; helle Version des Material Designs, aber mit einer dunklen Aktionsleiste.

Beispiele für diese Material Design-Varianten werden hier angezeigt:

[![Beispiel-Screenshots des Design "dunkel", "Light Theme" und "Dark Aktionsleiste Theme"](material-theme-images/three-flavors-sml.png)](material-theme-images/three-flavors.png#lightbox)

Sie können aus dem Material Design ableiten, um ein eigenes Design zu erstellen und einige oder alle Farb Attribute außer Kraft zu setzen. Sie können z. b. ein Design erstellen, das von `Theme.Material.Light`abgeleitet ist, aber die Farbe der APP-Leiste entsprechend der Farbe Ihrer Marke überschreibt. Sie können auch einzelne Ansichten formatieren. Beispielsweise können Sie einen Stil für [CardView](~/android/user-interface/controls/card-view.md) erstellen, der mehr abgerundete Ecken aufweist und eine dunklere Hintergrundfarbe verwendet.

Sie können ein einzelnes Design für eine gesamte App verwenden, oder Sie können unterschiedliche Designs für verschiedene Bildschirme (Aktivitäten) in einer App verwenden. In den obigen Screenshots verwendet eine einzelne APP beispielsweise ein anderes Design für jede Aktivität, um die integrierten Farbschemas zu veranschaulichen. Options Felder schalten die APP auf verschiedene Aktivitäten um und zeigen als Ergebnis andere Designs an.

Da das Material Design nur unter Android 5,0 und höher unterstützt wird, können Sie es (oder ein benutzerdefiniertes Design, das aus dem Material Design abgeleitet ist) nicht verwenden, um die APP für die Ausführung unter früheren Versionen von Android zu verwenden. Allerdings können Sie Ihre APP so konfigurieren, dass Sie das Material Design auf Android 5,0-Geräten verwendet, und auf ein früheres Design zurückgreifen, wenn es unter älteren Versionen von Android ausgeführt wird (Weitere Informationen finden Sie im Abschnitt zur [Kompatibilität](#compatibility) in diesem Artikel).

## <a name="requirements"></a>Requirements (Anforderungen)

Folgendes ist erforderlich, um die neuen Android 5,0-Material Design Features in xamarin-basierten apps zu verwenden:

- **Xamarin. Android** &ndash; xamarin. Android 4,20 oder höher muss entweder mit Visual Studio oder mit Visual Studio für Mac installiert und konfiguriert werden. 

- **Android SDK** &ndash; Android 5,0 (API 21) oder höher muss über den Android SDK Manager installiert werden.

- **Java JDK 1,8** &ndash; JDK 1,7 kann verwendet werden, wenn Sie speziell auf API-Ebene 23 und früher abzielen. JDK 1,8 ist in [Oracle](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)verfügbar.

Informationen zum Konfigurieren eines Android 5,0-App-Projekts finden Sie unter [Einrichten eines Android 5,0-Projekts](~/android/platform/lollipop.md).

## <a name="using-the-built-in-themes"></a>Verwenden der integrierten Designs

Die einfachste Möglichkeit, das Material Design zu verwenden, besteht darin, Ihre APP für die Verwendung eines integrierten Designs ohne Anpassung zu konfigurieren. Wenn Sie kein Design explizit konfigurieren möchten, wird Ihre APP standardmäßig `Theme.Material` (das dunkle Design). Wenn Ihre APP nur über eine Aktivität verfügt, können Sie ein Design auf der Aktivitäts Ebene konfigurieren. Wenn Ihre APP mehrere Aktivitäten umfasst, können Sie ein Design auf Anwendungsebene so konfigurieren, dass das gleiche Design für alle Aktivitäten verwendet wird, oder Sie können verschiedene Designs verschiedenen Aktivitäten zuweisen. In den folgenden Abschnitten wird erläutert, wie Designs auf App-Ebene und auf Aktivitäts Ebene konfiguriert werden.

### <a name="theming-an-application"></a>Design einer Anwendung

Um eine gesamte Anwendung für die Verwendung einer Material Design-Konfiguration zu konfigurieren, legen Sie das `android:theme`-Attribut des Anwendungs Knotens in " **androidmanifest. XML** " auf einen der folgenden Einstellungen fest:

- `@android:style/Theme.Material` &ndash; dunkles Design.

- `@android:style/Theme.Material.Light` &ndash; hellen Design.

- `@android:style/Theme.Material.Light.DarkActionBar` &ndash; helles Design mit der dunklen Aktionsleiste.

Im folgenden Beispiel wird die Anwendung *myapp* für die Verwendung des Design Light konfiguriert:

```xml
<application android:label="MyApp" 
             android:theme="@android:style/Theme.Material.Light">
</application>
```

Alternativ können Sie das Application `Theme`-Attribut in **AssemblyInfo.cs** (oder **Properties.cs**) festlegen. Beispiel:

```C#
[assembly: Application(Theme="@android:style/Theme.Material.Light")]
```

Wenn das Anwendungsdesign auf `@android:style/Theme.Material.Light`festgelegt ist, wird jede Aktivität in *myapp* mithilfe `Theme.Material.Light`angezeigt.

### <a name="theming-an-activity"></a>Design einer Aktivität

Wenn Sie ein Design für eine Aktivität verwenden möchten, fügen Sie dem `[Activity]`-Attribut oberhalb der Aktivitäts Deklaration eine `Theme` Einstellung hinzu, und weisen Sie `Theme` der gewünschten Material Design-Konfiguration zu. Im folgenden Beispiel wird eine-Aktivität mit `Theme.Material.Light`dargestellt:

```C#
[Activity(Theme = "@android:style/Theme.Material.Light",
          Label = "MyApp", MainLauncher = true, Icon = "@drawable/icon")]  
```

Andere Aktivitäten in dieser APP verwenden die Standard `Theme.Material` dunkles Farbschema (oder, wenn konfiguriert, die Einstellung des Anwendungs Designs).

<a name="customtheme" />

## <a name="using-custom-themes"></a>Verwenden von benutzerdefinierten Designs

Sie können Ihre Marke verbessern, indem Sie ein benutzerdefiniertes Design erstellen, das Ihre APP mit ihren Marken&rsquo;s-Farben formatiert. Um ein benutzerdefiniertes Design zu erstellen, definieren Sie ein neues Format, das von einer integrierten Material Design-Konfiguration abgeleitet wird und die Farb Attribute überschreibt, die Sie ändern möchten. Sie können z. b. ein benutzerdefiniertes Design definieren, das von `Theme.Material.Light.DarkActionBar` abgeleitet ist, und die Hintergrundfarbe des Bildschirms in beige statt weiß ändern.

Das Material Design macht die folgenden Layoutattribute für die Anpassung verfügbar:

- `colorPrimary` &ndash; die Farbe der APP-Leiste.

- `colorPrimaryDark` &ndash; die Farbe der Statusleiste und der kontextbezogenen App-leisten. Dies ist normalerweise eine dunkle Version von `colorPrimary`.

- `colorAccent` &ndash; die Farbe von UI-Steuerelementen, z. b. Kontrollkästchen, Options Felder und Textfelder bearbeiten.

- `windowBackground` &ndash; die Farbe des Bildschirm Hintergrunds.

- `textColorPrimary` &ndash; die Farbe des UI-Texts in der APP-Leiste.

- `statusBarColor` &ndash; die Farbe der Statusleiste.

- `navigationBarColor` &ndash; die Farbe der Navigationsleiste.

Diese Bildschirmbereiche werden im folgenden Diagramm bezeichnet:

[![Diagramm der Attribute und der zugehörigen Bildschirmbereiche](material-theme-images/screen-attributes-sml.png)](material-theme-images/screen-attributes.png#lightbox)

Standardmäßig wird `statusBarColor` auf den Wert `colorPrimaryDark`festgelegt. Sie können `statusBarColor` auf eine voll Tonfarbe festlegen, oder Sie können Sie auf `@android:color/transparent` festlegen, um die Statusleiste transparent zu gestalten. Die Navigationsleiste kann auch transparent gemacht werden, indem `navigationBarColor` auf `@android:color/transparent`festgelegt wird.

### <a name="creating-a-custom-app-theme"></a>Erstellen eines benutzerdefinierten App-Designs

Sie können ein benutzerdefiniertes App-Design erstellen, indem Sie Dateien im **Ressourcen** Ordner des App-Projekts erstellen und ändern. Um Ihre APP mit einem benutzerdefinierten Design zu formatieren, führen Sie die folgenden Schritte aus:

- Erstellen Sie eine Datei " **Colors. XML** " in **Ressourcen/Werten** &mdash; Sie diese Datei verwenden, um die benutzerdefinierten Design Farben zu definieren. Beispielsweise können Sie den folgenden Code in **Colors. XML** einfügen, um Ihnen den Einstieg zu erleichtern:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <color name="my_blue">#3498DB</color>
    <color name="my_green">#77D065</color>
    <color name="my_purple">#B455B6</color>
    <color name="my_gray">#738182</color>
</resources>
```

- Ändern Sie diese Beispieldatei, um die Namen und Farb Codes für Farb Ressourcen zu definieren, die Sie in Ihrem benutzerdefinierten Design verwenden werden.

- Erstellen Sie einen Ordner " **Resources/Values-V21** ". Erstellen Sie in diesem Ordner die Datei **Styles. XML** :

    [![Speicherort von "Styles. xml" im Ordner "Resources/Values-21. xml"](material-theme-images/values-v21-sml.png)](material-theme-images/values-v21.png#lightbox)

    Beachten Sie, dass die **Ressourcen/Werte V21** für Android 5,0 spezifisch sind &ndash; ältere Versionen von Android keine Dateien in diesem Ordner lesen.

- Fügen Sie der Datei **Styles. XML** einen `resources` Knoten hinzu, und definieren Sie einen `style` Knoten mit dem Namen des benutzerdefinierten Designs. Hier sehen Sie beispielsweise die Datei **Styles. XML** , die *mycustomtheme* definiert (abgeleitet aus dem integrierten `Theme.Material.Light` Design Style):

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <!-- Inherit from the light Material Theme -->
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Customizations go here -->
    </style>
</resources>
```

- An dieser Stelle zeigt eine APP, die *mycustomtheme* verwendet, das `Theme.Material.Light` Design ohne Anpassungen an:

    [![benutzerdefiniertes Design vor Anpassungen](material-theme-images/custom-theme-before-sml.png)](material-theme-images/custom-theme-before.png#lightbox)

- Fügen Sie der Datei " **Styles. XML** " Farbanpassungen hinzu, indem Sie die Farben der Layoutattribute definieren, die Sie ändern möchten. Wenn Sie z. b. die Farbe der APP-Leiste in `my_blue` ändern und die Farbe der UI-Steuerelemente in `my_purple`ändern möchten, fügen Sie " **Styles. XML** " Farb Überschreibungen hinzu, die auf Farb Ressourcen verweisen, die in **Colors. XML**

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

Nachdem diese Änderungen vorgenommen wurden, zeigt eine APP, die *mycustomtheme* verwendet, eine APP-leisten Farbe in `my_blue` und UI-Steuerelementen in `my_purple`an, aber das `Theme.Material.Light` Farbschema überall anders zu verwenden:

[![benutzerdefiniertes Design nach Anpassungen](material-theme-images/custom-theme-after-sml.png)](material-theme-images/custom-theme-after.png#lightbox)

In diesem Beispiel gibt *mycustomtheme* Farben von `Theme.Material.Light` für die Hintergrundfarbe, die Statusleiste und die Textfarben aus, ändert jedoch die Farbe der APP-Leiste in `my_blue` und legt die Farbe des Options Felds auf `my_purple`fest.

<a name="customview" />

### <a name="creating-a-custom-view-style"></a>Erstellen eines benutzerdefinierten Ansichts Stils

Android 5,0 ermöglicht Ihnen auch, eine einzelne Ansicht zu formatieren. Nachdem Sie " **Colors. XML** " und " **Styles. XML** " (wie im vorherigen Abschnitt beschrieben) erstellt haben, können Sie " **Styles. XML**" einen Ansichts Stil hinzufügen.
Um eine einzelne Ansicht zu formatieren, führen Sie die folgenden Schritte aus:

- Bearbeiten Sie **Resources/Values-V21/Styles. XML** , und fügen Sie einen `style` Knoten mit dem Namen Ihres benutzerdefinierten Ansichts Stils hinzu. Legen Sie die benutzerdefinierten Farb Attribute für Ihre Ansicht in diesem `style` Knoten fest. Um z. b. einen benutzerdefinierten [Karten Ansichts](~/android/user-interface/controls/card-view.md) Stil zu erstellen, der mehr abgerundete Ecken aufweist und `my_blue` als Karten Hintergrundfarbe verwendet, fügen Sie der Datei " **Styles. XML** " (innerhalb des Knotens `resources`) einen `style` Knoten hinzu und konfigurieren die Hintergrundfarbe und den Eckradius:

```xml
<!-- Theme an individual view: -->
<style name="CardView.MyBlue">

    <!-- Change the background color to Xamarin blue: -->
    <item name="cardBackgroundColor">@color/my_blue</item>

    <!-- Make the corners very round: -->
    <item name="cardCornerRadius">18dp</item>
</style>
```

- Legen Sie im Layout das `style`-Attribut für diese Ansicht so fest, dass es mit dem im vorherigen Schritt ausgewählten benutzerdefinierten Stilnamen identisch ist. Beispiel:

```xml
<android.support.v7.widget.CardView
    style="@style/CardView.MyBlue"
    android:layout_width="200dp"
    android:layout_height="100dp"
    android:layout_gravity="center_horizontal">
```

Der folgende Screenshot zeigt ein Beispiel für die Standard `CardView` (auf der linken Seite) im Vergleich zu einem `CardView`, das mit dem benutzerdefinierten `CardView.MyBlue` Design formatiert wurde (auf der rechten Seite angezeigt):

[![Beispiele für Standard CardView und Custom CardView](material-theme-images/custom-cardview-sml.png)](material-theme-images/custom-cardview.png#lightbox)

In diesem Beispiel wird die benutzerdefinierte `CardView` mit der Hintergrundfarbe `my_blue` und einem 18dp-Eckradius angezeigt.

## <a name="compatibility"></a>Kompatibilität

Führen Sie die folgenden Schritte aus, um Ihre APP so zu formatieren, dass Sie das Material Design unter Android 5,0 verwendet, aber automatisch auf eine abwärts kompatible Formatvorlage für ältere Android-Versionen zurückgreift:

- Definieren Sie ein benutzerdefiniertes Design in **Resources/Values-V21/Styles. XML** , das von einem Material Design Style abgeleitet ist. Beispiel:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

- Definieren Sie ein benutzerdefiniertes Design in **Resources/Values/Styles. XML** , das von einem älteren Design abgeleitet ist, aber den gleichen Design Namen wie oben verwendet. Beispiel:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Holo.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

- Konfigurieren Sie Ihre APP in " **androidmanifest. XML**" mit dem benutzerdefinierten Design Namen. 
    Beispiel:

```xml
<application android:label="MyApp" 
             android:theme="@style/MyCustomTheme">
</application>
```

- Alternativ können Sie eine bestimmte Aktivität mithilfe des benutzerdefinierten Designs formatieren:

```C#
[Activity(Label = "MyActivity", Theme = "@style/MyCustomTheme")]
```

Wenn in Ihrem Design Farben verwendet werden, die in einer Datei " **Colors. XML** " definiert sind, stellen Sie sicher, dass Sie diese Datei in **Ressourcen/Werte** (anstelle von " **Resources/Values-V21**") platzieren, damit beide Versionen des benutzerdefinierten Designs auf ihre Farb Definitionen zugreifen können.

Wenn Ihre APP auf einem Android 5,0-Gerät ausgeführt wird, wird die in **Resources/Values-V21/Styles. XML**angegebene Design Definition verwendet. Wenn diese APP auf älteren Android-Geräten ausgeführt wird, wird Sie automatisch auf die in **Resources/Values/Styles. XML**angegebene Design Definition zurückgreifen.

Weitere Informationen zur Design Kompatibilität mit älteren Android-Versionen finden Sie unter [Alternative Ressourcen](~/android/app-fundamentals/resources-in-android/alternate-resources.md).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das neue Design Design der Benutzeroberfläche vorgestellt, das in Android 5,0 (Lollipop) enthalten ist. Es wurden die drei integrierten Material Design-Varianten beschrieben, die Sie verwenden können, um Ihre APP zu formatieren. es wurde erläutert, wie Sie ein benutzerdefiniertes Design zum Branding Ihrer APP erstellen, und es wurde ein Beispiel für das Design einer einzelnen Ansicht bereitgestellt. Schließlich wurde in diesem Artikel erläutert, wie Sie das Material Design in Ihrer APP verwenden und gleichzeitig die Kompatibilität mit älteren Versionen von Android aufrechterhalten.

## <a name="related-links"></a>Verwandte Links

- [Themeswitcher (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-themeswitcher)
- [Einführung in Lollipop](../platform/lollipop.md)
- [CardView](controls/card-view.md)
- [Alternative Ressourcen](../app-fundamentals/resources-in-android/alternate-resources.md)
- [Android Lollipop](https://developer.android.com/about/versions/lollipop)
- [Android-Kreis Entwickler](https://developer.android.com/about/versions/pie/)
- [Material Design](https://developer.android.com/guide/topics/ui/look-and-feel/)
- [Grundlagen des Material Entwurfs](https://material.io/design/)
- [Beibehalten der Kompatibilität](https://developer.android.com/training/backward-compatible-ui/)
