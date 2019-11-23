---
title: Erstellen von Ressourcen für unterschiedliche Bildschirme
ms.prod: xamarin
ms.assetid: 3D17DE45-115C-7192-5685-44F8EEE07DCC
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/28/2018
ms.openlocfilehash: cbd392dcae173eb3baf0fb8f0c3c4ec7c0da23a1
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73025117"
---
# <a name="creating-resources-for-varying-screens"></a>Erstellen von Ressourcen für unterschiedliche Bildschirme

Android selbst kann auf vielen verschiedenen Geräten ausgeführt werden, die jeweils über eine Vielzahl von Auflösungen, Bildschirmgrößen und Bildschirm dichten verfügen. Android führt die Skalierung und die Größe der Größe aus, damit Ihre Anwendung auf diesen Geräten funktioniert. Dies kann jedoch zu einer nicht optimalen Benutzer Leistung führen. Beispielsweise können Bilder unscharf angezeigt werden, oder Sie können in einer Ansicht erwartungsgemäß positioniert werden.

## <a name="concepts"></a>Konzepte

Einige Begriffe und Konzepte sind wichtig zu verstehen, um mehrere Bildschirme zu unterstützen.

- **Bildschirmgröße** &ndash; die Menge an physischem Speicherplatz für die Anzeige der Anwendung

- **Bildschirm Dichte** &ndash; die Anzahl der Pixel in einem beliebigen Bereich auf dem Bildschirm. Die typische Maßeinheit ist dpi (dots per inch).

- **Auflösung** &ndash; die Gesamtzahl der Pixel auf dem Bildschirm. Beim Entwickeln von Anwendungen ist die Auflösung nicht so wichtig wie Bildschirmgröße und Dichte.

- Das **Dichte unabhängige Pixel (DP)** &ndash; eine virtuelle Maßeinheit, damit Layouts unabhängig von der Dichte entworfen werden können. Diese Formel wird verwendet, um DP in Bildschirm Pixel zu konvertieren:

    PX &equals; DP &times; dpi &divide; 160

- Die **Ausrichtung** &ndash; die Ausrichtung des Bildschirms wird als Querformat betrachtet, wenn es breiter ist als das Hochformat. Im Gegensatz dazu ist die Hochformat Ausrichtung, wenn der Bildschirm größer als breit ist. Die Ausrichtung kann sich während der Lebensdauer einer Anwendung ändern, wenn der Benutzer das Gerät dreht.

Beachten Sie, dass die ersten drei dieser Konzepte miteinander verknüpft sind, &ndash; durch Erhöhen der Auflösung ohne Erhöhung der Dichte die Bildschirmgröße erhöht werden. Wenn jedoch die Dichte und die Auflösung angehoben werden, kann die Bildschirmgröße unverändert bleiben. Diese Beziehung zwischen Bildschirmgröße, Dichte und Auflösung erschwert die Bildschirm Unterstützung.

Zur Unterstützung dieser Komplexität bevorzugt das Android-Framework die Verwendung von *Dichte unabhängigen Pixeln (DP)* für Bildschirmlayouts. Durch die Verwendung von Dichte unabhängigen Pixeln werden dem Benutzer Benutzeroberflächen Elemente angezeigt, die dieselbe physische Größe auf Bildschirmen mit unterschiedlichen dichten aufweisen.

## <a name="supporting-various-screen-sizes-and-densities"></a>Unterstützung verschiedener Bildschirmgrößen und dichten

Android erledigt den größten Teil der Arbeit, um die Layouts für jede Bildschirm Konfiguration ordnungsgemäß zu erzeugen. Es gibt jedoch einige Aktionen, die durchgeführt werden können, um das System zu unterstützen.

In den meisten Fällen ist die Verwendung von Dichte unabhängigen Pixeln anstelle der eigentlichen Pixel in Layouts ausreichend, um die Unabhängigkeit der Dichte sicherzustellen.
Android skaliert die drawables zur Laufzeit in die entsprechende Größe.
Die Skalierung kann jedoch dazu führen, dass Bitmaps unscharf angezeigt werden. Um dieses Problem zu umgehen, stellen Sie alternative Ressourcen für die unterschiedlichen dichten bereit. Beim Entwerfen von Geräten für mehrere Auflösungen und Bildschirm dichten kann es einfacher sein, mit der höheren Auflösung oder Dichte von Bildern zu beginnen und dann nach unten zu skalieren.

### <a name="declare-the-supported-screen-size"></a>Unterstützte Bildschirmgröße deklarieren

Das Deklarieren der Bildschirmgröße stellt sicher, dass nur unterstützte Geräte die Anwendung herunterladen können. Dies wird erreicht, indem das [unterstützte-Screens-](https://developer.android.com/guide/topics/manifest/supports-screens-element.html) Element in der Datei " **androidmanifest. XML** " festgelegt wird. Dieses Element wird verwendet, um anzugeben, welche Bildschirmgrößen von der Anwendung unterstützt werden. Ein angegebener Bildschirm wird als unterstützt betrachtet, wenn die Anwendung seine Layouts ordnungsgemäß auf den Füll Bildschirm platzieren kann. Wenn Sie dieses Manifest-Element verwenden, wird die Anwendung nicht in [*Google Play*](https://play.google.com/) für Geräte angezeigt, die die Bildschirm Spezifikationen nicht erfüllen. Die Anwendung wird jedoch weiterhin auf Geräten mit nicht unterstützten Bildschirmen ausgeführt, aber die Layouts erscheinen möglicherweise unscharf und pixelweise.

Unterstützte Bildschirmnamen werden in der Datei " **properites/androidmanifest. XML** " der Lösung deklariert:

<!-- markdownlint-disable MD001 -->

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Android Manifest](resources-for-varying-screens-images/01-android-manifest-sml.w1581.png)](resources-for-varying-screens-images/01-android-manifest.w1581.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Android Manifest](resources-for-varying-screens-images/01-android-manifest-sml.m761.png)](resources-for-varying-screens-images/01-android-manifest.m761.png#lightbox)

-----

Bearbeiten Sie " **androidmanifest. XML** " so, dass [Unterstützung für Bildschirme](https://developer.android.com/guide/topics/manifest/supports-screens-element.html)enthalten

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

### <a name="provide-alternate-layouts-for-different-screen-sizes"></a>Bereitstellen alternativer Layouts für verschiedene Bildschirmgrößen

Durch alternative Layouts können Sie eine Ansicht für eine spezielle Bildschirmgröße anpassen, um die Positionierung oder Größe der Komponenten der Komponenten Benutzeroberfläche zu ändern.

Beginnend mit API-Ebene 13 (Android 3,2), werden die Bildschirmgrößen zugunsten der Verwendung des SW*N*-DP-Qualifizierers als veraltet markiert. Dieser neue Qualifizierer deklariert den Umfang des Speicherplatzes, den ein bestimmtes Layout benötigt. Es wird empfohlen, dass Anwendungen, die unter Android 3,2 oder höher ausgeführt werden sollen, diese neueren Qualifizierer verwenden sollten.

Wenn ein Layout z. b. eine mindestens 700 DP-Breite des Bildschirms erfordert, würde das alternative Layout in einem Ordner **Layout sw700dp**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Layoutordner für 700 DP-Bildschirmbreite](resources-for-varying-screens-images/03-layout-sw700dp-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Layoutordner für 700 DP-Bildschirmbreite](resources-for-varying-screens-images/03-layout-sw700dp-xs.png)

-----

Im folgenden finden Sie eine Reihe von Zahlen für verschiedene Geräte:

- **Typisches Telefon** &ndash; 320 DP: ein typisches Telefon

- **Ein 5 "Tablet/" Tweener-Gerät** &ndash; 480 DP: z. b. der Samsung-Hinweis

- **A 7 "Tablet** &ndash; 600 DP: z. b. die" Barnes &amp; Noble Nook "

- **A 10 "Tablet** &ndash; 720 DP: z. b. das Motorola Xoom

Für Anwendungen, die auf API-Ebenen bis 12 (Android 3,1) abzielen, sollten die Layouts in Verzeichnissen verwendet werden, in denen die Qualifizierer **Small**/**Normal**/**Large**/**XLarge** als Verallgemeinerung der verschiedenen Bildschirmgrößen verwendet werden, die in den meisten Geräten zur Verfügung stehen. In der folgenden Abbildung sind z. b. alternative Ressourcen für die vier verschiedenen Bildschirmgrößen verfügbar:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Alternative Ressourcen für vier Bildschirmgrößen](resources-for-varying-screens-images/04-layout-large-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Alternative Ressourcen für vier Bildschirmgrößen](resources-for-varying-screens-images/04-layout-large-xs.png)

-----

Im folgenden wird anhand eines Vergleichs verglichen, wie die älteren Qualifizierer der Bildschirmgröße der älteren Pre-API-Ebene 13 mit Dichte unabhängigen Pixeln verglichen werden:

- 426 DP x 320 DP ist **klein**

- 470 DP x 320 DP ist **Normal**

- 640 DP x 480 DP ist **groß**

- 960 DP x 720 DP ist **XLarge**

Die neueren Bildschirmgrößen Qualifizierer auf API-Ebene 13 und höher haben Vorrang vor den älteren Bildschirm Qualifizierern der API-Ebenen 12 und niedriger.
Für Anwendungen, die sich über die alten und die neuen API-Ebenen erstrecken, kann es erforderlich sein, alternative Ressourcen mithilfe beider Sätze von Qualifizierern zu erstellen, wie im folgenden Screenshot zu sehen:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Alternative Ressourcen mit beiden Qualifizierern](resources-for-varying-screens-images/05-both-qualifiers-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Alternative Ressourcen mit beiden Qualifizierern](resources-for-varying-screens-images/05-both-qualifiers-xs.png)

-----

### <a name="provide-different-bitmaps-for-different-screen-densities"></a>Unterschiedliche Bitmaps für verschiedene Bildschirm dichten bereitstellen

Obwohl Android Bitmaps nach Bedarf für ein Gerät skaliert, werden die Bitmaps selbst möglicherweise nicht elegant zentral hoch-oder herunterskaliert: Sie werden möglicherweise unscharf oder verschwommen. Das Bereitstellen von Bitmaps, die für die Bildschirm Dichte geeignet sind, verringert dieses Problem.

Beispielsweise ist die folgende Abbildung ein Beispiel für Layoutprobleme, die auftreten können, wenn keine Ressourcen für die Dichte angegeben werden.

![Screenshots ohne Dichte Ressourcen](resources-for-varying-screens-images/06-density-not-provided.png)

Vergleichen Sie dies mit einem Layout, das mit Dichte spezifischen Ressourcen entworfen wurde:

![Screenshots mit Dichte spezifischen Ressourcen](resources-for-varying-screens-images/07-density-specific-resources.png)

### <a name="create-varying-density-resources-with-android-asset-studio"></a>Erstellen von unterschiedlichen Dichte Ressourcen mit Android Asset Studio

Die Erstellung dieser Bitmaps verschiedener dichten kann etwas mühsam sein. Google hat daher ein Online Dienstprogramm erstellt, das einige der mit der Erstellung dieser Bitmaps verbundenen Elemente reduzieren kann, die als [**Android Asset Studio**](https://romannurik.github.io/AndroidAssetStudio/)bezeichnet werden.

[![Android Asset Studio](resources-for-varying-screens-images/08-android-asset-studio-sml.png)](resources-for-varying-screens-images/08-android-asset-studio.png#lightbox)

Diese Website unterstützt Sie beim Erstellen von Bitmaps, die auf die vier gängigen Bildschirm dichten abzielen, indem ein Bild bereitgestellt wird. In Android Asset Studio werden dann die Bitmaps mit einigen Anpassungen erstellt und dann als ZIP-Datei heruntergeladen.

## <a name="tips-for-multiple-screens"></a>Tipps für mehrere Bildschirme

Android wird auf einer verwirrenden Anzahl von Geräten ausgeführt, und die Kombination aus Bildschirmgrößen und Bildschirm dichten kann überwältigend erscheinen. Mithilfe der folgenden Tipps können Sie den erforderlichen Aufwand für die Unterstützung verschiedener Geräte minimieren:

- **Entwerfen und entwickeln Sie nur für das, was Sie benötigen** &ndash; es gibt viele verschiedene Geräte, aber einige sind in seltenen Formfaktoren vorhanden, die einen beträchtlichen Aufwand für das Entwerfen und entwickeln von in Anspruch nehmen können. Das Dashboard [**Bildschirmgröße und Dichte**](https://developer.android.com/resources/dashboard/screens.html) ist eine von Google bereitgestellte Seite, die Daten zur Aufschlüsselung der Bildschirmgröße/Bildschirm Dichte Matrix bereitstellt. Diese Aufschlüsselung bietet Einblicke in die Entwicklung von Bildschirmen.

- **Verwenden Sie DPS anstelle von Pixel** -Pixel, wenn sich die Bildschirm Dichte ändert. Die Pixelwerte werden nicht hart codiert. Vermeiden Sie Pixel anstelle von DP (Dichte unabhängige Pixel).

- **Vermeiden** Sie, wenn **möglich** , " [AbsoluteLayout](xref:Android.Widget.AbsoluteLayout) "-
  , &ndash; Sie auf API-Ebene 3 (Android 1,5) veraltet ist und zu einer spröden Layouts führt. Er sollte nicht verwendet werden. Versuchen Sie stattdessen, flexiblere layoutwidgets wie [**LinearLayout**](xref:Android.Widget.LinearLayout), [**relativelayout**](xref:Android.Widget.RelativeLayout)oder das neue [**GridLayout**](xref:Android.Widget.GridLayout)zu verwenden.

- **Wählen Sie eine Layoutausrichtung als Standard** &ndash; z. b. anstatt das alternative Ressourcen **Layout-Land** und den **layoutport**bereitzustellen, legen Sie die Ressourcen für das **Layout im Layout**und die Ressourcen für das Hochformat in **layoutport**ab.

- **Verwenden von layoutpara Metern für Höhe und Breite** : beim Definieren von Benutzeroberflächen Elementen in einer XML-Layoutdatei hat eine Android-Anwendung, die die **wrap_content** -und **fill_parent** Werte verwendet, einen größeren Erfolg sicherzustellen, dass Sie über verschiedene Geräte hinweg ordnungsgemäß aussehen als bei der Verwendung von Pixel oder Dichte Diese Dimensions Werte bewirken, dass Android Bitmapressourcen nach Bedarf skaliert. Aus demselben Grund sind Dichte unabhängige Einheiten am besten reserviert, wenn Sie die Ränder und die Auffüll Zeichen von Benutzeroberflächen Elementen angeben.

## <a name="testing-multiple-screens"></a>Testen mehrerer Bildschirme

Eine Android-Anwendung muss für alle Konfigurationen getestet werden, die unterstützt werden. Im Idealfall sollten Geräte auf den eigentlichen Geräten selbst getestet werden, aber in vielen Fällen ist dies nicht möglich oder praktikabel.
In diesem Fall ist die Verwendung des Emulators und der virtuellen Android-Geräte für jede Gerätekonfiguration nützlich.

Mit dem Android SDK werden einige emulatorskins bereitgestellt, die zum Erstellen von AVDS verwendet werden können, um die Größe, Dichte und Auflösung vieler Geräte zu replizieren.
Viele der Hardwarehersteller stellen auch Skins für Ihre Geräte bereit.

Eine andere Möglichkeit ist die Verwendung der Dienste eines Drittanbieter-Test Diensts.
Diese Dienste nehmen ein APK, führen es auf vielen verschiedenen Geräten aus und geben dann Feedback zur Funktionsweise der Anwendung an.
