---
title: "Erstellen von Ressourcen für unterschiedliche Bildschirme"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3D17DE45-115C-7192-5685-44F8EEE07DCC
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: fcd77d97d492baee441cfd428e58ea83525f927e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="creating-resources-for-varying-screens"></a>Erstellen von Ressourcen für unterschiedliche Bildschirme

Android selbst wird ausgeführt auf vielen verschiedenen Geräten mit einer Vielzahl von Lösungen, Bildschirmgrößen und Bildschirm dichten. Android wird durchführen, Skalierung und Ändern der Größe, damit die Anwendung, die auf diesen Geräten arbeiten, aber kann dies ein nicht optimaler Benutzererlebnis. Z. B. Bilder möglicherweise unscharf angezeigt wird, Bilder, wodurch die Position von Elementen der Benutzeroberfläche in das Layout wird überlappen oder entweder zu weit auseinander, zu viele (oder nicht genügend) ein Bildschirm Platz belegen können.


## <a name="concepts"></a>Konzepte

Einige Begriffe und Konzepte sind wichtig zu verstehen, um mehrere Bildschirme zu unterstützen.

- **Displaygröße** &ndash; die Menge des physischen Speicherplatzes für die Anzeige von Ihrer Anwendung

- **Bildschirm Dichte** &ndash; die Anzahl der Pixel in einem angegebenen Bereich auf dem Bildschirm. Die typische Maßeinheit ist Punkte pro Zoll (dpi).

- **Auflösung** &ndash; die Gesamtanzahl der Pixel auf dem Bildschirm. Wenn Sie Anwendungen entwickeln, beträgt die Auflösung nicht so wichtig wie die Bildschirmgröße und Dichte.

- **Dichte Pixels (dp)** &ndash; Dies ist eine virtuelle Maßeinheit um Layouts entworfen werden unabhängig von der Dichte zu ermöglichen. Dp in Pixel konvertiert wird die folgende Formel verwendet:

    px &equals; dp &times; dpi &divide; 160

- **Ausrichtung** &ndash; der Ausrichtung des Bildschirms gilt Landscape sein, wenn es breiter als breit ist. Im Gegensatz dazu ist Hochformat auf, wenn der Bildschirm höher ist als breit ist. Die Ausrichtung kann während der Lebensdauer einer Anwendung ändern, wie der Benutzer das Gerät dreht.

Beachten Sie, dass die ersten drei dieser Konzepte Kommunikation beziehen &ndash; die Auflösung zu erhöhen, ohne die Dichte erhöhen die Größe des Bildschirms erhöhen. Jedoch kann erweitert werden, die Dichte und die Auflösung der Bildschirmgröße unverändert bleiben. Diese Beziehung zwischen der Bildschirmgröße und Dichte Auflösung erschweren bildschirmunterstützung sehr schnell.

Zur Unterstützung der Umgang mit dieser Komplexität Android Framework bevorzugt *Dichte typunabhängig Pixel (dp)* für Bildschirmlayouts. Mithilfe der Dichte geräteunabhängige Pixel werden Elemente der Benutzeroberfläche für den Benutzer haben die gleiche physikalische Größe auf Bildschirmen mit unterschiedliche Dichten angezeigt.


## <a name="supporting-various-screen-sizes-and-densities"></a>Unterstützung für verschiedene Bildschirmgrößen und dichten

Android verarbeitet die meiste Arbeit Layouts ordnungsgemäß für jede Bildschirmkonfiguration auf gerendert. Es gibt jedoch einige Aktionen, die ausgeführt werden können, um das System helfen.

Die Verwendung der Dichte typunabhängig Pixel anstelle der tatsächlichen Pixel Layouts reicht in den meisten Fällen um Dichte Unabhängigkeit sicherzustellen.
Android kann die Drawables zur Laufzeit, um die geeignete Größe skaliert werden.
Es ist jedoch möglich, dass diese Skalierung Bitmaps unscharf erscheinen verursachen. Um dies zu vermeiden, kann es notwendig, das Bereitstellen alternativer Ressourcen für die unterschiedliche Dichten sein. Beim Entwerfen von Geräten für mehrere Lösungen und Bildschirm dichten einfacher nachweisen wird zunächst die höhere Auflösung oder die Dichte Bilder und skalieren Sie anschließend nach unten. Dadurch wird verhindert, alle Weichzeichnen oder Verzerrung, der von der Größenänderung führen kann.


### <a name="declare-the-screen-size-the-application-supports"></a>Deklarieren Sie die Größe des Bildschirms der Anwendung unterstützt

Deklarieren die Größe des Bildschirms wird sichergestellt, dass die Anwendung nur die unterstützten Geräte heruntergeladen werden können. Dies geschieht durch Festlegen der [unterstützt Bildschirme](http://developer.android.com/guide/topics/manifest/supports-screens-element.html) Element in der **AndroidManifest.xml** Datei. Dieses Element wird verwendet, um anzugeben, welche Bildschirmgrößen von der Anwendung unterstützt werden. Ein bestimmter Bildschirm gilt unterstützt werden, wenn die Anwendung ordnungsgemäß kann es handelt sich um Layouts an die Bildschirmgröße. Mithilfe dieses manifest Element die Anwendung wird nicht im angezeigt [ *Google Play* ](https://play.google.com/) für Geräte, die nicht die Bildschirm-Spezifikationen entsprechen. Die Anwendung auf Geräten mit nicht unterstützten Bildschirme immer noch ausgeführt wird, enthält jedoch Layouts möglicherweise unscharf angezeigt und pixelig.

Zu diesem Zweck in Xamarin.Android es ist erforderlich, fügen zuerst eine **AndroidManifest.xml** Datei dem Projekt, wenn sie nicht bereits vorhanden ist:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Android Manifest](resources-for-varying-screens-images/01-android-manifest-vs-sml.png)](resources-for-varying-screens-images/01-android-manifest-vs.png#lightbox)

**AndroidManifest.xml** wird hinzugefügt, um die **Eigenschaften** Verzeichnis. Die Datei anschließend bearbeitet wird, um enthalten [unterstützt Bildschirme](http://developer.android.com/guide/topics/manifest/supports-screens-element.html):

[![Unterstützt die Bildschirme hinzufügen](resources-for-varying-screens-images/02-adding-supports-screens-vs-sml.png)](resources-for-varying-screens-images/02-adding-supports-screens-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Android Manifest](resources-for-varying-screens-images/01-android-manifest-xs-sml.png)](resources-for-varying-screens-images/01-android-manifest-xs.png#lightbox)

**AndroidManifest.xml** wird hinzugefügt, um die **Eigenschaften** Verzeichnis. Die Datei anschließend bearbeitet wird, um enthalten [unterstützt Bildschirme](http://developer.android.com/guide/topics/manifest/supports-screens-element.html):

[![Unterstützt die Bildschirme hinzufügen](resources-for-varying-screens-images/02-adding-supports-screens-xs-sml.png)](resources-for-varying-screens-images/02-adding-supports-screens-xs.png#lightbox)

-----

### <a name="provide-alternate-layouts-for-different-screen-sizes"></a>Geben Sie alternative Layouts für verschiedene Bildschirmgrößen

Obwohl Android entsprechend der Größe des Bildschirms angepasst wird, dies nicht in einigen Fällen möglicherweise ausreichend. Es sind einige Elemente der Benutzeroberfläche auf einem größeren Bildschirm zu vergrößern oder zu ändern, die Positionierung der Elemente der Benutzeroberfläche für kleinere Bildschirme wünschenswert.

API-Ebene 13 (Android 3.2) ab, der Bildschirmgrößen sind veraltet zugunsten von der sw*N*dp-Qualifizierer. Dieser neue Qualifizierer deklariert, dass die Speichermenge, die einem bestimmten Layout benötigt. Es wird dringend empfohlen, dass Anwendungen, die unter Android 3.2 oder höher ausführen sollen diese neueren Qualifizierer verwendet werden sollte.

Z. B. ggf. ein Layout eine minimale 700dp der Bildschirmbreite des alternative Layouts würde wechseln Sie in einem Ordner **Layout sw700dp**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Ordner 700dp Bildschirmbreite "Layout"](resources-for-varying-screens-images/03-layout-sw700dp-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![Ordner 700dp Bildschirmbreite "Layout"](resources-for-varying-screens-images/03-layout-sw700dp-xs.png)

-----


Hier sind einige Zahlen für verschiedene Geräte, als Richtlinie:

- **Typische Phone** &ndash; 320dp: eine typische Phone

- **Ein Tablet 5"/"Tweener"Gerät** &ndash; 480dp: z. B. die Samsung-Anmerkung

- **Ein Tablet/7 "** &ndash; 600dp: z. B. die Barnes &amp; Noble Nook

- **Ein 10" Tablet** &ndash; 720dp: z. B. die Motorola Xoom

Anwendungen, dass bis zu 12 (Android 3.1), die Ziel-API Zugriffsebenen Layouts sollten finden Sie in den Verzeichnissen, die die Qualifizierer verwenden **kleine**/**normalen**/**große**  / **Xlarge** als verallgemeinerungen, die unterschiedlichen Bildschirmgrößen, die in den meisten Geräten verfügbar sind. In der folgenden Abbildung sind z. B. vier unterschiedlichen Bildschirmgrößen alternative Ressourcen:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Alternative Ressourcen für vier Bildschirmgrößen](resources-for-varying-screens-images/04-layout-large-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![Alternative Ressourcen für vier Bildschirmgrößen](resources-for-varying-screens-images/04-layout-large-xs.png)

-----

Im folgenden finden einen Vergleich wie die älteren Pre-API-Ebene 13 Bildschirm Größe Qualifizierer mit Dichte typunabhängig Pixel verglichen werden soll:

- 426dp x 320dp ist **klein**

- 470dp x 320dp ist **normal**

- 640dp x 480dp ist **Groß**

- 960dp x 720dp ist **Xlarge**

Neuere Bildschirm size-Qualifizierer in API-Ebene 13 und Sie haben eine höhere Priorität als die älteren Bildschirm Qualifizierer von API-Ebenen-12 und eine untere.
Für Anwendungen, die den alten und neuen API-Ebenen umfassen werden zum Erstellen von alternativer Ressourcen, die beide Sätze von Qualifizierer verwenden, wie im folgenden Screenshot gezeigt möglicherweise erforderlich:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Alternative Ressourcen mit beiden Qualifizierer](resources-for-varying-screens-images/05-both-qualifiers-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![Alternative Ressourcen mit beiden Qualifizierer](resources-for-varying-screens-images/05-both-qualifiers-xs.png)

-----



### <a name="provide-different-bitmaps-for-different-screen-densities"></a>Geben Sie die verschiedenen Bitmaps für verschiedene Bildschirm dichten

Obwohl Android Bitmaps nach Bedarf für ein Gerät skaliert werden, die Bitmaps für sich selbst möglicherweise nicht Programmierframework skalieren nach oben oder unten: möglicherweise werden Sie entweder fuzzy oder unscharf angezeigt. Bereitstellen von Bitmaps für den Bildschirm Dichte geeignet, wird dieses Problem lindern.

Beispielsweise die folgenden Abbildung ist ein Beispiel für Layout und die Darstellung möglicherweise auftretenden Probleme Dichte Geben Sie bei der Ressourcen nicht bereitgestellt werden.

![Screenshots ohne Dichte Ressourcen](resources-for-varying-screens-images/06-density-not-provided.png)

Vergleichen Sie dies zu einem Layout, die mit bestimmten Ressourcen Dichte vorgesehen ist:

![Screenshots mit Dichte-spezifischen Ressourcen](resources-for-varying-screens-images/07-density-specific-resources.png)


### <a name="create-varying-density-resources-with-android-asset-studio"></a>Erstellen Sie unterschiedliche Dichte Ressourcen mit Android Asset-Studio

Die Erstellung von diese Bitmaps der verschiedenen dichten kann etwas zeitraubend sein. Google wurde ein online-Dienstprogramm an einige die Beteiligten bei der Erstellung des diese Bitmaps aufgerufen zeitlichen Aufwand zu reduzieren, kann daher erstellt die [ **Android Asset Studio**](https://romannurik.github.io/AndroidAssetStudio/).

[![Android Asset Studio](resources-for-varying-screens-images/08-android-asset-studio-sml.png)](resources-for-varying-screens-images/08-android-asset-studio.png#lightbox)

Diese Website ist hilfreich für die Erstellung von Bitmaps, die auf die vier allgemeine Bildschirm dichten abzielen, durch die Bereitstellung von einem Bild. Android Asset Studio erstellen Sie die Bitmaps Transponder- und anschließend als Zip-Datei heruntergeladen werden können.


## <a name="tips-for-multiple-screens"></a>Tipps für mehrere Bildschirme

Android ausgeführt wird, auf einer verwirrend Anzahl von Geräten, und die Kombination von Bildschirmgrößen und -Bildschirm dichten kann überwältigend scheinen. Die folgenden Tipps helfen, die zur Unterstützung von verschiedenen Geräten erforderliche Aufwand zu minimieren:

- **Nur entwerfen und entwickeln Sie müssen** &ndash; es gibt viele verschiedene Geräte gibt es einige vorhanden jedoch in seltenen Formfaktoren arbeiten, die sehr aufwändig, Entwerfen und Entwickeln für dauern. Die [ **Bildschirmgröße und die Dichteergebnisse** ](http://developer.android.com/resources/dashboard/screens.html) Dashboard ist eine Seite, die von Google, die Daten, auf die Aufschlüsselung der Bildschirm Größe/Bildschirm Dichte Matrix bereitstellt bereitgestellt. Diese Aufteilung bietet einen Einblick zum Entwicklungsaufwand auf die Unterstützung von Bildschirmen an.

- **Bevorzugen Sie DPs gegenüber Pixel** -Pixel problematische Bildschirm Dichte geändert werden. Keine hartkodierung für Pixelwerte. Vermeiden Sie Pixel zugunsten dp (Dichte unabhängig in Pixel).

- **Vermeiden Sie** [AbsoluteLayout](https://developer.xamarin.com/api/type/Android.Widget.AbsoluteLayout/)
  **ablegen, wo mögliche** &ndash; er veraltetes Feature in API-Ebene 3 (Android 1.5) und wird dazu jedoch fehleranfällig Layouts. Es sollte nicht verwendet werden. Stattdessen versuchen, eine flexiblere Layout Widgets verwenden, z. B. [ **LinearLayout**](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/), [ **RelativeLayout**](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/), oder die neuen [ **GridLayout**](https://developer.xamarin.com/api/type/Android.Widget.GridLayout/).

- **Wählen Sie ein Layout Ausrichtung als Ihrem Standardbrowser** &ndash; z. B. statt der alternativen Ressourcen **Layout Land** und **Layout-Port**, platzieren die Ressourcen für im Querformat **Layout**, und die Ressourcen für Hochformat in **Layout-Port**.

- **LayoutParams für Höhe und Breite verwenden** – Wenn Sie Elemente der Benutzeroberfläche in eine XML-Layout-Datei Definieren eines Android-Anwendung mithilfe der **Wrap_content** und **Fill_parent** Werte müssen weitere Erfolg Stellen Sie sicher eine ordnungsgemäße Darstellung auf verschiedenen Geräten als Pixel oder Dichte geräteunabhängigen Einheiten mit ein. Diese Dimensionswerte bewirken, dass Android Bitmapressourcen nach Bedarf skalieren. Aus diesem Grund gleichen Einheiten Dichte typunabhängig sind am besten reserviert für den Fall angeben die Ränder und Abstand von Elementen der Benutzeroberfläche.


## <a name="testing-multiple-screens"></a>Testen mehrere Bildschirme

Eine Android-Anwendung muss für alle Konfigurationen getestet werden soll, die unterstützt werden. Im Idealfall Geräte auf die eigentlichen Geräte selbst getestet werden, aber in vielen Fällen ist dies nicht möglich oder praktikabel.
In diesem Fall wird die Verwendung der Emulator und virtuelles Android-Geräte-Setup für jeden Gerätekonfiguration nützlich sein.

Android SDK bietet einige Emulator Designs können verwendet werden, zum Erstellen des AVD-Größe, Dichte und Auflösung von vielen Geräten repliziert werden sollen.
Viele der Hardwarehersteller bieten ebenso Designs für ihre Geräte.

Eine andere Möglichkeit ist die Verwendung der Dienste eines dritten Dienst testen.
Diese Dienste werden ein APK schalten, führen Sie es auf vielen verschiedenen Geräten und geben Sie dann auf Feedback wie die Anwendung funktioniert.
