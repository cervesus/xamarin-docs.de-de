---
title: Materialdesign
description: 'Gewusst wie: Design Ihrer xamarin. Android-App mit Material Design'
ms.prod: xamarin
ms.assetid: DC4CDBD0-3DF9-4B7E-B876-29128985E2A7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: fca8ee02fc48979db1d29716374ba300a0e8bbbf
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522367"
---
# <a name="material-theme"></a>Materialdesign

Das *Material* Design ist ein Benutzeroberflächen Stil, der das Erscheinungsbild von Ansichten und Aktivitäten bestimmt, beginnend mit Android 5,0 (Lollipop). Das Material Design ist in Android 5,0 integriert, sodass es von der Benutzeroberfläche des Systems sowie von Anwendungen verwendet wird. Das Material Design ist kein "Design" im Sinne einer systemweiten Darstellungs Option, die ein Benutzer dynamisch aus dem Menü "Einstellungen" auswählen kann. Vielmehr kann das Material Design als ein Satz verwandter, integrierter Basis Stile betrachtet werden, die Sie zum Anpassen des Erscheinungsbild Ihrer APP verwenden können.

Android bietet drei Material Design-Varianten:

- `Theme.Material`&ndash; Dunkle Version von Material Theme; Dies ist die Standardkonfiguration in Android 5,0.

- `Theme.Material.Light`&ndash; Helle Version des Material Designs.

- `Theme.Material.Light.DarkActionBar`&ndash; Helle Version des Material Designs, aber mit einer dunklen Aktionsleiste.

Beispiele für diese Material Design-Varianten werden hier angezeigt:

[![Beispiel-Screenshots des Design "dunkel", "Light Design" und "Dark Aktionsleiste Design"](material-theme-images/three-flavors-sml.png)](material-theme-images/three-flavors.png#lightbox)

Sie können aus dem Material Design ableiten, um ein eigenes Design zu erstellen und einige oder alle Farb Attribute außer Kraft zu setzen. Sie können z. b. ein Design erstellen, das `Theme.Material.Light`von abgeleitet wird, aber die Farbe der APP-Leiste entsprechend der Farbe Ihrer Marke überschreibt. Sie können auch einzelne Ansichten formatieren. Beispielsweise können Sie einen Stil für [CardView](~/android/user-interface/controls/card-view.md) erstellen, der mehr abgerundete Ecken aufweist und eine dunklere Hintergrundfarbe verwendet.

Sie können ein einzelnes Design für eine gesamte App verwenden, oder Sie können unterschiedliche Designs für verschiedene Bildschirme (Aktivitäten) in einer App verwenden. In den obigen Screenshots verwendet eine einzelne APP beispielsweise ein anderes Design für jede Aktivität, um die integrierten Farbschemas zu veranschaulichen. Options Felder schalten die APP auf verschiedene Aktivitäten um und zeigen als Ergebnis andere Designs an.

Da das Material Design nur unter Android 5,0 und höher unterstützt wird, können Sie es (oder ein benutzerdefiniertes Design, das aus dem Material Design abgeleitet ist) nicht verwenden, um die APP für die Ausführung unter früheren Versionen von Android zu verwenden. Allerdings können Sie Ihre APP so konfigurieren, dass Sie das Material Design auf Android 5,0-Geräten verwendet, und auf ein früheres Design zurückgreifen, wenn es unter älteren Versionen von Android ausgeführt wird (Weitere Informationen finden Sie im Abschnitt zur [Kompatibilität](#compatibility) in diesem Artikel).


## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um die neuen Android 5,0-Material Design Features in xamarin-basierten apps zu verwenden:

- **Xamarin. Android** &ndash; xamarin. Android 4,20 oder höher muss entweder mit Visual Studio oder mit Visual Studio für Mac installiert und konfiguriert werden. 

- **Android SDK** &ndash; Android 5,0 (API 21) oder höher muss über den Android SDK-Manager installiert werden.

- **Java JDK 1,8** &ndash; JDK 1,7 kann verwendet werden, wenn Sie genau die API-Ebene 23 und früher verwenden. JDK 1,8 ist in [Oracle](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)verfügbar.

Informationen zum Konfigurieren eines Android 5,0-App-Projekts finden Sie unter [Einrichten eines Android 5,0-Projekts](~/android/platform/lollipop.md).


## <a name="using-the-built-in-themes"></a>Verwenden der integrierten Designs

Die einfachste Möglichkeit, das Material Design zu verwenden, besteht darin, Ihre APP für die Verwendung eines integrierten Designs ohne Anpassung zu konfigurieren. Wenn Sie kein Design explizit konfigurieren möchten, wird die App standardmäßig auf `Theme.Material` (das dunkle Design) fest. Wenn Ihre APP nur über eine Aktivität verfügt, können Sie ein Design auf der Aktivitäts Ebene konfigurieren. Wenn Ihre APP mehrere Aktivitäten umfasst, können Sie ein Design auf Anwendungsebene so konfigurieren, dass das gleiche Design für alle Aktivitäten verwendet wird, oder Sie können verschiedene Designs verschiedenen Aktivitäten zuweisen. In den folgenden Abschnitten wird erläutert, wie Designs auf App-Ebene und auf Aktivitäts Ebene konfiguriert werden.


### <a name="theming-an-application"></a>Design einer Anwendung

Um eine gesamte Anwendung für die Verwendung einer Material Design-Konfiguration zu konfigurieren `android:theme` , legen Sie das-Attribut des Anwendungs Knotens in " **androidmanifest. XML** " auf einen der folgenden Einstellungen fest:

- `@android:style/Theme.Material`&ndash; Dunkles Design.

- `@android:style/Theme.Material.Light`&ndash; Helles Design.

- `@android:style/Theme.Material.Light.DarkActionBar`&ndash; Helles Design mit dunkler Aktionsleiste.

Im folgenden Beispiel wird die Anwendung *myapp* für die Verwendung des Design Light konfiguriert:

```xml
<application android:label="MyApp" 
             android:theme="@android:style/Theme.Material.Light">
</application>
```

Alternativ können Sie das Anwendungs `Theme` Attribut in **AssemblyInfo.cs** (oder **Properties.cs**) festlegen. Beispiel:

```C#
[assembly: Application(Theme="@android:style/Theme.Material.Light")]
```

Wenn das Anwendungsdesign auf festgelegt `@android:style/Theme.Material.Light`ist, wird jede Aktivität in *myapp* mithilfe `Theme.Material.Light`von angezeigt.


### <a name="theming-an-activity"></a>Design einer Aktivität

Um eine Aktivität zu verwenden, fügen Sie `Theme` dem `[Activity]` Attribut oberhalb der Aktivitäts Deklaration eine Einstellung hinzu `Theme` , und weisen Sie die gewünschte Material Design-Konfiguration zu. Im folgenden Beispiel wird eine-Aktivität `Theme.Material.Light`mit dem-

```C#
[Activity(Theme = "@android:style/Theme.Material.Light",
          Label = "MyApp", MainLauncher = true, Icon = "@drawable/icon")]  
```

Für andere Aktivitäten in dieser APP wird das Standard `Theme.Material` Schema für dunkle Farben verwendet (oder, wenn konfiguriert, die Einstellung des Anwendungs Designs).

<a name="customtheme" />

## <a name="using-custom-themes"></a>Verwenden von benutzerdefinierten Designs

Sie können Ihre Marke verbessern, indem Sie ein benutzerdefiniertes Design erstellen, das&rsquo;Ihre APP mit den Farben ihrer Marken formatiert. Um ein benutzerdefiniertes Design zu erstellen, definieren Sie ein neues Format, das von einer integrierten Material Design-Konfiguration abgeleitet wird und die Farb Attribute überschreibt, die Sie ändern möchten. Beispielsweise können Sie ein benutzerdefiniertes Design definieren, das `Theme.Material.Light.DarkActionBar` von abgeleitet wird, und die Hintergrundfarbe des Bildschirms in beige anstatt weiß ändern.

Das Material Design macht die folgenden Layoutattribute für die Anpassung verfügbar:

- `colorPrimary`&ndash; Die Farbe der APP-Leiste.

- `colorPrimaryDark`Die Farbe der Statusleiste und der kontextbezogenen App-leisten. Dies ist normalerweise eine dunkle Version von `colorPrimary`. &ndash;

- `colorAccent`&ndash; Die Farbe von UI-Steuerelementen, z. b. Kontrollkästchen, Options Felder und Textfelder bearbeiten.

- `windowBackground`&ndash; Die Farbe des Bildschirm Hintergrunds.

- `textColorPrimary`&ndash; Die Farbe des UI-Texts in der APP-Leiste.

- `statusBarColor`&ndash; Die Farbe der Statusleiste.

- `navigationBarColor`&ndash; Die Farbe der Navigationsleiste.

Diese Bildschirmbereiche werden im folgenden Diagramm bezeichnet:

[![Diagramm der Attribute und der zugehörigen Bildschirmbereiche](material-theme-images/screen-attributes-sml.png)](material-theme-images/screen-attributes.png#lightbox)

Standardmäßig `statusBarColor` ist auf den `colorPrimaryDark`Wert festgelegt. Sie können auf `statusBarColor` eine voll Tonfarbe festlegen, oder Sie können Sie auf `@android:color/transparent` festlegen, um die Statusleiste transparent zu gestalten. Die Navigationsleiste kann auch durch Festlegen `navigationBarColor` von auf `@android:color/transparent`transparent gemacht werden.


### <a name="creating-a-custom-app-theme"></a>Erstellen eines benutzerdefinierten App-Designs

Sie können ein benutzerdefiniertes App-Design erstellen, indem Sie Dateien im **Ressourcen** Ordner des App-Projekts erstellen und ändern. Um Ihre APP mit einem benutzerdefinierten Design zu formatieren, führen Sie die folgenden Schritte aus:

- Erstellen einer Datei " **Colors. XML** " in **Ressourcen/Werten** &mdash; Sie verwenden diese Datei, um die benutzerdefinierten Design Farben zu definieren. Beispielsweise können Sie den folgenden Code in **Colors. XML** einfügen, um Ihnen den Einstieg zu erleichtern:

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

    Beachten Sie, dass die **Ressourcen/Werte V21** speziell für Android &ndash; 5,0, ältere Versionen von Android, keine Dateien in diesem Ordner lesen.

- Fügen Sie `resources` einen Knoten zu " **Styles. XML** " `style` hinzu, und definieren Sie einen Knoten mit dem Namen des benutzerdefinierten Designs. Hier sehen Sie beispielsweise die Datei **Styles. XML** , die *mycustomtheme* definiert (abgeleitet aus dem `Theme.Material.Light` integrierten Design Stil):

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <!-- Inherit from the light Material Theme -->
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Customizations go here -->
    </style>
</resources>
```

- An dieser Stelle zeigt eine APP, die *mycustomtheme* verwendet, das Aktien `Theme.Material.Light` Design ohne Anpassungen an:

    [![Darstellung des benutzerdefinierten Designs vor Anpassungen](material-theme-images/custom-theme-before-sml.png)](material-theme-images/custom-theme-before.png#lightbox)

- Fügen Sie der Datei " **Styles. XML** " Farbanpassungen hinzu, indem Sie die Farben der Layoutattribute definieren, die Sie ändern möchten. Wenn Sie z. b. die Farbe der APP `my_blue` -Leiste in ändern und die Farbe von `my_purple`UI-Steuerelementen in ändern möchten, fügen Sie " **Styles. XML** " Farb Überschreibungen hinzu, die auf Farb Ressourcen verweisen, die in **Colors. XML**

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

Nachdem diese Änderungen vorgenommen wurden, zeigt eine APP, die *mycustomtheme* verwendet, eine Farbe der APP `my_blue` -Leiste in und `my_purple`UI-Steuerelemente `Theme.Material.Light` in an, verwendet jedoch das Farbschema an allen anderen Stellen:

[![Darstellung des benutzerdefinierten Designs nach Anpassungen](material-theme-images/custom-theme-after-sml.png)](material-theme-images/custom-theme-after.png#lightbox)

In diesem Beispiel werden von *mycustomtheme* Farben aus `Theme.Material.Light` für die Hintergrundfarbe, die Statusleiste und die Textfarben, aber die Farbe der APP-Leiste in `my_blue` geändert und die Farbe des Options Felds auf `my_purple`festgelegt.

<a name="customview" />

### <a name="creating-a-custom-view-style"></a>Erstellen eines benutzerdefinierten Ansichts Stils

Android 5,0 ermöglicht Ihnen auch, eine einzelne Ansicht zu formatieren. Nachdem Sie " **Colors. XML** " und " **Styles. XML** " (wie im vorherigen Abschnitt beschrieben) erstellt haben, können Sie " **Styles. XML**" einen Ansichts Stil hinzufügen.
Um eine einzelne Ansicht zu formatieren, führen Sie die folgenden Schritte aus:

- Bearbeiten Sie **Resources/Values-V21/Styles. XML** , und `style` fügen Sie einen Knoten mit dem Namen Ihres benutzerdefinierten Ansichts Stils hinzu. Legen Sie die benutzerdefinierten Farb Attribute für Ihre Ansicht `style` innerhalb dieses Knotens fest. Um z. b. einen Benutzer [definierten Karten Ansichts](~/android/user-interface/controls/card-view.md) Stil zu erstellen, der mehr `my_blue` abgerundete Ecken aufweist und als Karten Hintergrund `style` Farbe verwendet, fügen Sie " **Styles. XML** " (innerhalb des `resources` Knotens) einen Knoten hinzu, und konfigurieren Sie die Hintergrundfarbe. Eckradius:

```xml
<!-- Theme an individual view: -->
<style name="CardView.MyBlue">

    <!-- Change the background color to Xamarin blue: -->
    <item name="cardBackgroundColor">@color/my_blue</item>

    <!-- Make the corners very round: -->
    <item name="cardCornerRadius">18dp</item>
</style>
```

- Legen Sie im Layout das `style` -Attribut für diese Ansicht so fest, dass es mit dem im vorherigen Schritt ausgewählten benutzerdefinierten Stilnamen identisch ist. Beispiel:

```xml
<android.support.v7.widget.CardView
    style="@style/CardView.MyBlue"
    android:layout_width="200dp"
    android:layout_height="100dp"
    android:layout_gravity="center_horizontal">
```

Der folgende Screenshot zeigt ein Beispiel für den Standard `CardView` Wert (auf der linken Seite) im Vergleich zu `CardView` einem, der mit dem benutzerdefinierten `CardView.MyBlue` Design formatiert wurde (auf der rechten Seite angezeigt):

[![Beispiele für Standard CardView und Custom CardView](material-theme-images/custom-cardview-sml.png)](material-theme-images/custom-cardview.png#lightbox)

In diesem Beispiel wird der Benutzer `CardView` definierte mit der Hintergrundfarbe `my_blue` und einem 18dp-Eckradius angezeigt.


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
