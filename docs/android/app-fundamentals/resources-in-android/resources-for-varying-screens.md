---
title: Erstellen von Ressourcen für unterschiedliche Bildschirme
ms.prod: xamarin
ms.assetid: 3D17DE45-115C-7192-5685-44F8EEE07DCC
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/28/2018
ms.openlocfilehash: df8ee3da8a1341cd1dd879e8e70687d9fbd9957b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117433"
---
# <a name="creating-resources-for-varying-screens"></a>Erstellen von Ressourcen für unterschiedliche Bildschirme

Android selbst wird ausgeführt auf vielen verschiedenen Geräten, jeweils eine Vielzahl von Lösungen, Bildschirmgrößen und bildschirmdichten. Android führt, Skalierung und Größenänderung, um eine Anwendung auf diesen Geräten arbeiten, jedoch kann dies zu einer suboptimalen benutzerfreundlichkeit. Z. B. Bilder möglicherweise unscharf angezeigt, oder sie können für eine Sicht erwartungsgemäß positioniert werden.


## <a name="concepts"></a>Konzepte

Einige Begriffe und Konzepte sind wichtig zu verstehen, um mehrere Bildschirme zu unterstützen.

- **Bildschirmgröße** &ndash; die Menge des physischen Speicherplatzes für die Anzeige von Ihrer Anwendung

- **Bildschirm Dichte** &ndash; die Anzahl der Pixel in einem angegebenen Bereich auf dem Bildschirm. Die typische Maßeinheit ist Punkt pro Zoll (dpi).

- **Auflösung** &ndash; die Gesamtanzahl der Pixel auf dem Bildschirm. Beim Entwickeln von Anwendungen, ist die Lösung nicht so wichtig wie Bildschirmgröße und Dichte.

- **Dichte unabhängigen Pixeln (dp)** &ndash; eine virtuelle Maßeinheit um Layouts entworfen werden, unabhängig von der Dichte zu ermöglichen. Diese Formel wird verwendet, um dp in Bildschirmpixel zu konvertieren:

    px &equals; dp &times; dpi &divide; 160

- **Ausrichtung** &ndash; der Ausrichtung des Bildschirms gilt Querformat bei breiter als hoch ist. Im Gegensatz dazu ist Hochformat auf, wenn der Bildschirm höher ist als breit ist. Während der Lebensdauer einer Anwendung kann die Ausrichtung ändern, wie der Benutzer das Gerät dreht.

Beachten Sie, dass die ersten drei dieser Konzepte miteinander verwandt sind &ndash; Erhöhen der Auflösung, ohne die Dichte erhöhen die Größe des Bildschirms erhöhen. Aber wenn sowohl die Auflösung die Dichte erhöht werden, klicken Sie dann die Größe des Bildschirms unverändert bleiben kann. Diese Beziehung zwischen der Größe des Bildschirms, Dichte und Auflösung erschweren bildschirmunterstützung schnell.

Damit mit dieser Komplexität umgehen können, die Android-Framework verwenden lieber *Dichte unabhängigen Pixeln (dp)* für mehrere Layouts für Startbildschirm. Mit dichteunabhängigen Pixeln, erscheint die UI-Elemente für den Benutzer auf die gleiche physische Größe auf Bildschirmen mit verschiedenen dichten zu haben.


## <a name="supporting-various-screen-sizes-and-densities"></a>Unterstützung für verschiedene Bildschirmgrößen und dichten

Android behandelt die meisten Aufgaben der Layouts ordnungsgemäß für jede Bildschirmkonfiguration rendern. Es gibt jedoch einige Aktionen, die ausgeführt werden können, um das System helfen.

Die Verwendung der Dichte unabhängigen Pixeln anstelle der tatsächlichen Pixel Layouts reicht in den meisten Fällen um Unabhängigkeit von der Dichte sicherzustellen.
Android kann die zeichenbarer Ressourcen zur Laufzeit, um die entsprechende Größe skaliert werden.
Allerdings ist es möglich, dass die Skalierung Bitmaps unscharf angezeigt bewirken. Um dieses Problem zu umgehen, geben Sie alternative Ressourcen für die verschiedenen dichten. Beim Entwerfen von Geräten für mehrere Lösungen und bildschirmdichten, dies einfacher belegen wird für den Einstieg die höhere Auflösung oder die Dichte images sowie anschließendes Herunterskalieren.


### <a name="declare-the-supported-screen-size"></a>Deklarieren Sie die unterstützten Bildschirmgröße

Deklarieren die Größe des Bildschirms wird sichergestellt, dass nur unterstützte Geräte die Anwendung herunterladen können. Dies geschieht durch Festlegen der [unterstützt-Bildschirme](http://developer.android.com/guide/topics/manifest/supports-screens-element.html) Element in der **"androidmanifest.xml"** Datei. Dieses Element wird verwendet, um anzugeben, welche Bildschirmgrößen, von der Anwendung unterstützt werden. Ein vorhandener Bildschirm gilt unterstützt werden, wenn die Anwendung ordnungsgemäß die Layouts, um den Bildschirm ausfüllen platziert werden kann. Mit diesem manifestelement können die Anwendung nicht angezeigt [ *Google Play* ](https://play.google.com/) für Geräte, die die Bildschirm-Spezifikationen nicht entsprechen. Die Anwendung auf Geräten mit nicht unterstützten Bildschirmen immer noch ausgeführt wird, jedoch die Layouts möglicherweise unscharf angezeigt und verpixelt dargestellt.

Unterstützte Bildschirm Sixes deklariert werden, der **Properites/AndroidManifest.xml** -Datei der Lösung:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Android Manifest](resources-for-varying-screens-images/01-android-manifest-sml.w1581.png)](resources-for-varying-screens-images/01-android-manifest.w1581.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Android Manifest](resources-for-varying-screens-images/01-android-manifest-sml.m761.png)](resources-for-varying-screens-images/01-android-manifest.m761.png#lightbox)

-----

Bearbeiten Sie **"androidmanifest.xml"** sollen [unterstützt-Bildschirme](http://developer.android.com/guide/topics/manifest/supports-screens-element.html):

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          android:versionCode="1"
          android:versionName="1.0"
          package="HelloWorld.HelloWorld">
      <uses-sdk android:minSdkVersion="21" android:targetSdkVersion="27" />
      <supports-screens android:resizable="true"
                        android:smallScreens="true"
                        android:normalScreens="true"
                        android:largeScreens="true" />
      <application android:allowBackup="true"
                   android:icon="@mipmap/ic_launcher"
                   android:label="@string/app_name"
                   android:roundIcon="@mipmap/ic_launcher_round"
                   android:supportsRtl="true" android:theme="@style/AppTheme">
  </application>
</manifest>
```

### <a name="provide-alternate-layouts-for-different-screen-sizes"></a>Geben Sie alternative Layouts für verschiedene Bildschirmgrößen


Alternative Layouts können Sie eine Ansicht für ein bestimmter Bildschirmgröße, ändern die Positionierung oder die Größe der Komponente von Elementen der Benutzeroberfläche anpassen.

API-Ebene 13 (Android 3.2) ab, die Bildschirmgrößen sind veraltet zugunsten der sw*N*dp-Qualifizierer. Diese neuen Qualifizierer deklariert, dass die Menge des Speicherplatzes eines bestimmten Layouts benötigt. Es wird empfohlen, dass Anwendungen, die auf Android 3.2 oder höher ausgeführt werden sollen diese neuere Qualifizierer verwendet werden sollte.

Angenommen, ein Layout erforderlich, eine minimale 700 dp der Bildschirmbreite, fiel die alternative Layout in einem Ordner **Layout-sw700dp**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Layout-Ordner für die Breite des 700 dp-Bildschirm](resources-for-varying-screens-images/03-layout-sw700dp-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Layout-Ordner für die Breite des 700 dp-Bildschirm](resources-for-varying-screens-images/03-layout-sw700dp-xs.png)

-----


Als Richtwert sind hier einige Zahlen für verschiedene Geräte:

- **Typische Phone** &ndash; 320 dp: ein typischer Telefon

- **Ein Tablet 5"/"Tweener"Gerät** &ndash; 480 dp: z. B. die Samsung-Anmerkung

- **Ein Tablet/7 "** &ndash; 600 dp: z. B. die Barnes &amp; Noble Nook

- **10" Tablet** &ndash; 720 dp: z. B. die Motorola Xoom

Für Anwendungen, dass bis zu 12 (Android 3.1), die Ziel-API Zugriffsebenen Layouts funktionieren in Verzeichnissen, die die Qualifizierer verwenden **kleine**/**normalen**/**große**  / **sehr groß** als verallgemeinerungen, die verschiedene Bildschirmgrößen, die in den meisten Geräten verfügbar sind. In der folgenden Abbildung sind z. B. alternative Ressourcen für die vier verschiedene Bildschirmgrößen:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Alternative Ressourcen für vier Bildschirmgrößen](resources-for-varying-screens-images/04-layout-large-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Alternative Ressourcen für vier Bildschirmgrößen](resources-for-varying-screens-images/04-layout-large-xs.png)

-----

Im folgenden finden einen Vergleich der, wie die älteren Pre-API-Ebene 13 Bildschirm Größe Qualifizierer mit Dichte unabhängigen Pixeln verglichen werden soll:

- 426 dp X 320 dp ist **klein**

- 470 dp X 320 dp ist **normal**

- 640 dp X 480 dp ist **große**

- 960 dp X 720 dp ist **sehr groß**

Der neuere Bildschirm Größe Qualifizierer in API-Ebene 13 und Sie haben eine höhere Priorität als die ältere Bildschirm Qualifizierer des API-Ebenen-12 und niedriger.
Für Anwendungen, die das alte als auch die neuen API-Ebenen umfassen soll, kann es zum Erstellen von anderer Ressourcen, die beide Sätze von Qualifizierer verwenden, wie im folgenden Screenshot gezeigt erforderlich sein:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Alternative Ressourcen mit beiden Qualifizierern](resources-for-varying-screens-images/05-both-qualifiers-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Alternative Ressourcen mit beiden Qualifizierern](resources-for-varying-screens-images/05-both-qualifiers-xs.png)

-----



### <a name="provide-different-bitmaps-for-different-screen-densities"></a>Geben Sie unterschiedliche Bitmaps für verschiedene bildschirmdichten

Obwohl Android Bitmaps je nach Bedarf für ein Gerät skaliert wird, die Bitmaps für sich selbst möglicherweise nicht elegant zentral hoch- oder Herunterskalieren: möglicherweise werden Sie verschwommene oder Fuzzyübereinstimmungen. Bereitstellen von Bitmaps für die Dichte der Bildschirm geeignet, wird dieses Problem lindern.

Beispielsweise die folgenden Abbildung ist ein Beispiel für Layout und die Darstellung möglicherweise auftretende Probleme bei Dichte-Geben Sie Ressourcen nicht bereitgestellt.

![Screenshots ohne Dichte-Ressourcen](resources-for-varying-screens-images/06-density-not-provided.png)

Vergleichen Sie dies zu einem Layout, die mit Dichte-spezifischen Ressourcen vorgesehen ist:

![Screenshots mit Dichte-spezifischen Ressourcen](resources-for-varying-screens-images/07-density-specific-resources.png)


### <a name="create-varying-density-resources-with-android-asset-studio"></a>Erstellen von unterschiedlicher Dichte-Ressourcen mit Android Asset Studio

Die Erstellung von diese Bitmaps mit verschiedenen dichten kann etwas schwierig sein. Daher hat Google ein online-Dienstprogramm, mit denen einige den Aufwand bei der die Erstellung von diese Bitmaps aufgerufen reduziert kann die [ **Android Asset Studio**](https://romannurik.github.io/AndroidAssetStudio/).

[![Android Asset Studio](resources-for-varying-screens-images/08-android-asset-studio-sml.png)](resources-for-varying-screens-images/08-android-asset-studio.png#lightbox)

Diese Website hilft bei der Erstellung von Bitmaps, die die vier allgemeine bildschirmdichten abzielen, indem Sie ein Abbild bereitstellen. Android Asset Studio wird dann die Bitmaps mit einige Anpassungen erstellen, und lassen Sie diese als Zip-Datei heruntergeladen werden.


## <a name="tips-for-multiple-screens"></a>Tipps für mehrere Bildschirme

Android wird auf eine verwirrende Anzahl von Geräten, und die Kombination von Bildschirmgrößen und bildschirmdichten mag überwältigend scheinen. Die folgenden Tipps können den Aufwand erforderlich, um unterschiedliche Geräte unterstützen zu minimieren:

- **Nur entwerfen und entwickeln Sie für die tatsächlich benötigte** &ndash; es gibt es viele verschiedene Geräte vorhanden, jedoch einige in seltenen geräteausführungen, die zu entwerfen und Entwickeln für dauern. Die [ **Bildschirmgröße und Dichte** ](http://developer.android.com/resources/dashboard/screens.html) Dashboard ist eine Seite, die von Google, die Daten, auf die Aufschlüsselung der Bildschirm Bildschirmgrößen/Dichte Matrix bereitstellt bereitgestellt wird. Aufteilung bietet einen Einblick zum Entwicklungsarbeit auf die Unterstützung von Bildschirmen.

- **Verwenden Sie DPs anstelle von Pixel** -Pixel, werden als Bildschirm Dichte wird problematischen. Keine hartkodierung für Pixelwerte. Vermeiden Sie Pixel zugunsten von dp (Dichte unabhängigen Pixeln).

- **Vermeiden Sie** [von "AbsoluteLayout"](https://developer.xamarin.com/api/type/Android.Widget.AbsoluteLayout/)
  **ganz egal, wo möglich** &ndash; er wird in API-Ebene-3 (Android 1.5) als veraltet markiert und führt zu fehleranfällig Layouts. Es sollte nicht verwendet werden. Versuchen Sie stattdessen, verwenden z. B. eine flexiblere layoutwidgets [ **LinearLayout**](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/), [ **RelativeLayout**](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/), oder beim neuen [ **GridLayout**](https://developer.xamarin.com/api/type/Android.Widget.GridLayout/).

- **Wählen Sie eine layoutausrichtung als Standardsuchanbieter** &ndash; z. B. statt der alternativen Ressourcen **Layout zusteuere** und **Layout-Port**, legen Sie die Ressourcen für im Querformat **Layout**, und die Ressourcen für Hochformat in **Layout-Port**.

- **LayoutParams für Höhe und Breite verwenden** : beim Definieren von UI-Elemente in einer XML-Layout-Datei, ein Android-Anwendung mit der **Wrap_content** und **Fill_parent** -Werte müssen mehr Erfolg Stellen Sie richtige finden Sie auf verschiedenen Geräten als die Verwendung von Pixel oder in Einheiten der Dichte unabhängigen sicher. Diese Dimensionswerte bewirken, dass Android Bitmap-Ressourcen nach Bedarf skalieren. Aus diesem Grund gleichen Dichte unabhängigen Einheiten sind am besten reserviert für die die Ränder angeben und Auffüllung von Elementen der Benutzeroberfläche.


## <a name="testing-multiple-screens"></a>Testen von mehreren Bildschirmen

Eine Android-Anwendung muss für alle Konfigurationen getestet werden, die unterstützt werden. Im Idealfall sollten Geräte auf die eigentlichen Geräte selbst getestet werden, aber in vielen Fällen ist dies nicht möglich oder angebracht.
In diesem Fall wird die Verwendung des Emulators und virtuelle Android-Geräte-Setup für jeden Gerätekonfiguration nützlich sein.

Das Android SDK bietet, dass einige Emulator-Skins verwendet werden, um AVDs zu erstellen, die Größe, Dichte und Auflösung von vielen Geräten repliziert werden sollen.
Viele der Hardwarehersteller bieten ebenso Skins für ihre Geräte.

Eine andere Möglichkeit ist die Dienste von einem Drittanbieter testen Diensts verwenden.
Diese Dienste werden ein APK nutzen, führen Sie es auf vielen verschiedenen Geräten und geben Sie dann auf Feedback wie die Anwendung funktioniert.
