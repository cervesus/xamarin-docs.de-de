---
title: Grundlegendes zu Android-API-Ebenen
description: "Xamarin.Android hat mehrere Servicelevel Android-API-Einstellungen, die Ihre app-Kompatibilität mit mehreren Versionen von Android zu bestimmen. Dieses Handbuch erklärt, was bedeutet, dass diese Einstellungen, deren Konfiguration und welche Auswirkung auf Ihre app zur Laufzeit aufweisen."
ms.topic: article
ms.prod: xamarin
ms.assetid: 58CB7B34-3140-4BEB-BE2E-209928C1878C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 907af0948e9d081f05cc201c49f94629a513c935
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="understanding-android-api-levels"></a>Grundlegendes zu Android-API-Ebenen

_Xamarin.Android hat mehrere Servicelevel Android-API-Einstellungen, die Ihre app-Kompatibilität mit mehreren Versionen von Android zu bestimmen. Dieses Handbuch erklärt, was bedeutet, dass diese Einstellungen, deren Konfiguration und welche Auswirkung auf Ihre app zur Laufzeit aufweisen._


## <a name="quick-start"></a>Schnellstart

Xamarin.Android macht drei projekteinstellungen der Android-API-Ebene:

-   [Zielframework](#framework) &ndash; gibt an, welche Framework zum Erstellen von Ihrer Anwendung. Diese API-Ebene dient zur *Kompilieren* von Xamarin.Android Zeit.

-   [Mindestversion von Android](#minimum) &ndash; gibt die älteste Android-Version, die Ihre app unterstützt werden sollen. Diese API-Ebene dient zur *ausführen* Zeit von Android.

-   [Android-Version als Ziel](#target) &ndash; gibt die Version von Android, die Ihre app ist für die Ausführung auf ausgelegt. Diese API-Ebene dient zur *ausführen* Zeit von Android.

Bevor Sie eine API-Ebene für das Projekt konfigurieren können, müssen Sie die SDK-Plattform-Komponenten für diese API-Ebene installieren. Weitere Informationen zum Herunterladen und Installieren von Android SDK-Komponenten finden Sie unter [Android SDK-Installation](~/android/get-started/installation/android-sdk.md).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Normalerweise werden alle drei Ebenen von Xamarin.Android-API auf den gleichen Wert festgelegt. Auf der **Anwendung** Seite **Compile using Android Version (Zielframework)** auf die neueste stabile API-Version (oder zumindest auf die Android-Version, die alle Funktionen verfügt, Sie müssen).
Im folgenden Screenshot ist das Zielframework auf festgelegt **Android 7.1 (API-Ebene 25 - Nougat)**:

[![Framework-Version standardmäßig Kompilieren mithilfe von Android-Version als Ziel](android-api-levels-images/vs-defaults-sml.png)](android-api-levels-images/vs-defaults.png#lightbox)

Auf der **Android-Manifest** Seite, legen Sie die mindestens Android-Version auf **verwenden Kompilieren mit SDK Version** , und legen Sie die Ziel-Android-Version auf den gleichen Wert wie die Ziel-Framework-Version (in der folgenden Screenshot des Zielframeworks für Android auf festgelegt ist **Android 7.1 (Nougat)**):

[![Legen Sie Mindest- und Ziel-Android-Versionen auf Zielframework-version](android-api-levels-images/vs-manifest-defaults-sml.png)](android-api-levels-images/vs-manifest-defaults.png#lightbox)

Wenn Sie Abwärtskompatibilität mit einer früheren Version von Android beibehalten möchten, legen Sie **mindestens Android-Version als Ziel** die älteste Version von Android, die Ihre app unterstützt werden sollen. (Beachten Sie, dass API-Ebene 14 für erforderliche minimale API-Ebene [Google Play-Dienste und Firebase](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html).) Das folgende Beispielkonfiguration unterstützt Android-Versionen von API-Ebene 14 bis API-Ebene 25:

[![Kompilieren Sie mit der API-Ebene 25 Nougat "," Minimum Android-Version auf API-Ebene 14 festgelegt](android-api-levels-images/vs-minimum-sml.png)](android-api-levels-images/vs-minimum.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Normalerweise werden alle drei Ebenen von Xamarin.Android-API auf den gleichen Wert festgelegt. Legen Sie **Zielframework** auf die neueste stabile API-Version (oder zumindest auf die Android-Version, die alle Funktionen verfügt, Sie müssen). Festlegen der **Zielframework**, navigieren Sie zu **erstellen > Allgemein** in der **Projektoptionen**. Im folgenden Screenshot ist das Zielframework auf festgelegt **verwenden Sie die neueste installierte Plattform (8.0)**:

[![Neueste installierte Plattform direktionales Zielframework](android-api-levels-images/xs-default-target-sml.png)](android-api-levels-images/xs-default-target.png#lightbox)

Die Einstellungen für die minimale und Ziel Android-Version finden Sie unter **erstellen > Android-Anwendung** in **Projektoptionen**. Legen Sie die mindestens Android-Version auf **Automatic - Framework-Zielversion verwenden** , und legen Sie die Ziel-Android-Version auf den gleichen Wert wie die Ziel-Framework-Version. Im folgenden Screenshot ist das Zielframework für Android auf festgelegt ist **Android 8.0 (API-Ebene 26)** entsprechend die oben genannten Zielframework-Einstellung:

[![Die Ziel- und Framework Ebenen festlegen in den Projekteigenschaften](android-api-levels-images/xs-default-app-sml.png)](android-api-levels-images/xs-default-app.png#lightbox)

Wenn Sie Abwärtskompatibilität mit einer früheren Version von Android beibehalten möchten, ändern Sie **mindestens Android-Version** die älteste Version von Android, die Ihre app unterstützt werden sollen. Beachten Sie, dass API-Ebene 14 für erforderliche minimale API-Ebene [Google Play-Dienste und Firebase](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html).
Beispielsweise unterstützt die folgende Konfiguration so früh wie API-Ebene 14 Android-Versionen aus:

[![Minimum und Ziel-Versionen, die automatisch - verwenden die Zielframeworkversion](android-api-levels-images/xs-minimum-sml.png)](android-api-levels-images/xs-minimum.png#lightbox)

-----


Wenn mehrere Android-Version Ihrer app unterstützt werden, muss Ihr Code Überprüfungen zur Laufzeit, um sicherzustellen, dass Ihre app mit der Einstellung für mindestens Android-Version funktioniert enthalten (finden Sie unter [Laufzeitprüfungen für Android-Versionen](#runtimechecks) unten Einzelheiten). Wenn Sie nutzen oder eine Bibliothek erstellen, finden Sie unter [API-Ebenen und Bibliotheken](#libraries) unten für best Practices bei der Konfiguration von API-level-Einstellungen für Bibliotheken.



## <a name="android-versions-and-api-levels"></a>Android-Versionen und API-Ebenen

Die Android-Plattform entwickelt, und neue Android-Versionen freigegeben werden, wird jede Android-Version einen eindeutige ganzzahlige Bezeichner, genannt zugewiesen der *API-Ebene*. Daher entspricht jede Android-Version auf eine einzelne Android-API-Ebene. Da Benutzer apps auf älteren als auch als das neueste Versionen von Android installieren, müssen die realen Android-apps arbeiten mit mehreren Ebenen von Android-API entworfen werden.


### <a name="android-versions"></a>Android-Versionen

Jede Version von Android im Laufe der mehrere Namen:

-   Die Android-Version, z. B. **Android 7.1**
-   Ein Name, wie z. B. code _Nougat_
-   Eine entsprechende API-Ebene, z. B. **API-Ebene 25**

Eine Android Codename, entsprechen möglicherweise mehrere Versionen und API-Ebenen (wie in der Liste unten dargestellt), aber jede Android-Version entspricht genau eine API-Ebene.

Darüber hinaus Xamarin.Android definiert *Versionscodes erstellen* , der den derzeit bekannten Android-API-Ebenen zuordnen. In der folgenden Liste können Sie die API-Ebene, Android-Version Codename und Xamarin.Android Build Code von Version zu konvertieren.

-   **API-27 (Android 8.1)** &ndash; _Oreo_, Dezember 2017 freigegeben. Erstellen von Code von version `Android.OS.BuildVersionCodes.OMr1`

-   **API-26 (Android 8.0)** &ndash; _Oreo_, August 2017 freigegeben. Erstellen von Code von version `Android.OS.BuildVersionCodes.O`

-   **API-25 (Android 7.1)** &ndash; _Nougat_, Dezember 2016 veröffentlicht. Erstellen von Code von version `Android.OS.BuildVersionCodes.NMr1`

-   **API-24 (Android 7.0)** &ndash; _Nougat_, August 2016 veröffentlicht. Erstellen von Code von version `Android.OS.BuildVersionCodes.N`

-   **API-23 (Android 6.0)** &ndash; _Marshmallow_, August 2015 veröffentlicht. Erstellen von Code von version `Android.OS.BuildVersionCodes.M`

-   **API-22 (Android 5.1)** &ndash; _Lollipop_, die vom März 2015 veröffentlicht. Erstellen von Code von version `Android.OS.BuildVersionCodes.LollipopMr1`

-   **API 21 (Android 5.0)** &ndash; _Lollipop_, die vom November 2014 veröffentlicht. Erstellen von Code von version `Android.OS.BuildVersionCodes.Lollipop`

-   **API-20 (Android 4.4W)** &ndash; _Kitkat Überwachen_, Juni 2014 veröffentlicht. Erstellen von Code von version `Android.OS.BuildVersionCodes.KitKatWatch`

-   **API-19 (Android 4.4)** &ndash; _Kitkat_, Oktober 2013 veröffentlicht. Erstellen von Code von version `Android.OS.BuildVersionCodes.KitKat`

-   **API-18 (Android 4.3)** &ndash; _Gelee Bean_, Juli 2013 veröffentlicht. Erstellen von Code von version `Android.OS.BuildVersionCodes.JellyBeanMr2`

-   **API-17 (Android 4.2-4.2.2)** &ndash; _Gelee Bean_, November 2012 veröffentlicht. Erstellen von Code von version `Android.OS.BuildVersionCodes.JellyBeanMr1`

-   **API-16 (Android 4.1-4.1.1)** &ndash; _Gelee Bean_, Juni 2012 veröffentlicht. Erstellen von Code von version `Android.OS.BuildVersionCodes.JellyBean`

-   **API-15 (Android 4.0.3-4.0.4)** &ndash; _Eis Rustikal Sandwich_, Dezember 2011 veröffentlicht. Erstellen von Code von version `Android.OS.BuildVersionCodes.IceCreamSandwichMr1`

-   **API 14 (Android 4.0-4.0.2)** &ndash; _Eis Rustikal Sandwich_, Version vom Oktober 2011 veröffentlicht. Erstellen von Code von version `Android.OS.BuildVersionCodes.IceCreamSandwich`

-   **API-13 (Android 3.2)** &ndash; _Wabe_, Juni 2011 veröffentlicht. Erstellen von Code von version `Android.OS.BuildVersionCodes.HoneyCombMr2`

-   **API-12 (Android 3.1.x)** &ndash; _Wabe_, Mai 2011 veröffentlicht. Erstellen von Code von version `Android.OS.BuildVersionCodes.HoneyCombMr1`

-   **API 11 (Android 3.0.x)** &ndash; _Honeycomb_, released February 2011. Erstellen von Code von version `Android.OS.BuildVersionCodes.HoneyComb`

-   **API-10 (Android 2.3.3-2.3.4)** &ndash; _Gingerbread_, Februar 2011 veröffentlicht. Erstellen von Code von version `Android.OS.BuildVersionCodes.GingerBreadMr1`

-   **API-9 (Android 2.3-2.3.2)** &ndash; _Gingerbread_, November 2010 veröffentlicht. Erstellen von Code von version `Android.OS.BuildVersionCodes.GingerBread`

-   **API-8 (Android 2.2.x)** &ndash; _Froyo_, Juni 2010 veröffentlicht. Erstellen von Code von version `Android.OS.BuildVersionCodes.Froyo`

-   **API-7 (Android 2.1.x)** &ndash; _Eclair_, Januar 2010 veröffentlicht. Erstellen von Code von version `Android.OS.BuildVersionCodes.EclairMr1`

-   **API 6 (Android 2.0.1)** &ndash; _Eclair_, released December 2009. Erstellen von Code von version `Android.OS.BuildVersionCodes.Eclair01`

-   **API-5 (Android 2.0)** &ndash; _Eclair_, November 2009 veröffentlicht. Erstellen von Code von version `Android.OS.BuildVersionCodes.Eclair`

-   **API-4 (Android 1.6)** &ndash; _Ringdiagramm_, September 2009 veröffentlicht. Erstellen von Code von version `Android.OS.BuildVersionCodes.Donut`

-   **API-3 (Android 1.5)** &ndash; _Cupcake_, Mai 2009 veröffentlicht. Erstellen von Code von version `Android.OS.BuildVersionCodes.Cupcake`

-   **API-2 (Android 1.1)** &ndash; _Base_, Februar 2009 veröffentlicht. Erstellen von Code von version `Android.OS.BuildVersionCodes.Base11`

-   **API-1 (Android 1.0)** &ndash; _Base_, Oktober 2008 veröffentlicht. Erstellen von Code von version `Android.OS.BuildVersionCodes.Base`


Wie diese Liste gibt, werden häufig neue Android-Versionen freigegeben &ndash; manchmal mehrere Releases pro Jahr. Daher gehören eine Vielzahl von Android-ältere und neuere Versionen Universum von Android-Geräten, die Ihre app ausgeführt. Wie können Sie sicherstellen, dass Ihre app, zuverlässig und konsistent auf so viele verschiedene Versionen von Android ausgeführt werden? Android API-Ebenen können Sie dieses Problem zu verwalten.


### <a name="android-api-levels"></a>Android-API-Ebenen

Jede Android-Gerät auf ausgeführt wird, genau *eine* API-Ebene &ndash; ist gewährleistet, dass dieser API-Ebene für jede Version von Android-Plattform eindeutig sein. API-Ebene gibt genau die Version der API-Satz, dem in die app aufrufen kann; Er gibt die Kombination der Elemente von Manifesten, Berechtigungen, usw., Sie Code anhand als Entwickler. Android-Betriebssystem der API-Ebenen kann Android zu bestimmen, ob eine Anwendung mit einem Android Systemabbild vor der Installation der Anwendung auf einem Gerät kompatibel ist.

Wenn eine Anwendung erstellt wird, enthält es die folgenden API-Level-Informationen an:

-   Die *Ziel* API-Ebene für Android, die die app erstellt wird, ausgeführt.

-   Die *minimale* Android-API-Ebene, die ein Android-Gerät benötigen, um die Anwendung auszuführen. 

Diese Einstellungen werden verwendet, um sicherzustellen, dass für die ordnungsgemäße Ausführung die Anwendung benötigten Funktionen bei der Installation auf dem Android-Gerät verfügbar ist. Wenn dies nicht der Fall ist, blockiert die app wird auf diesem Gerät ausgeführt. Z. B. wenn API-Ebene eines Android-Geräts kleiner als die minimale API-Ebene, die Sie für Ihre app angeben ist, wird die Android-Gerät den Benutzer verhindern, dass Ihrer app zu installieren.


## <a name="project-api-level-settings"></a>Projekteinstellungen für die API-Ebene

In den folgenden Abschnitten wird erläutert, wie die SDK-Manager verwenden, um die Entwicklungsumgebung Vorbereiten für die API-Ebenen verwendet werden soll, woraufhin wiederum detaillierte erläuterungen zum Konfigurieren von *Zielframework*, *Minimum Android-Version*, und *Ziel Android-Version* Einstellungen in Xamarin.Android.


### <a name="android-sdk-platforms"></a>Android SDK-Plattformen

Bevor Sie eine Ziel oder die Minimum-API-Ebene in Xamarin.Android auswählen können, müssen Sie die Version der Android-SDK-Plattform, die entspricht installieren, zu dieser API-Ebene. Der Bereich der verfügbaren Optionen für das Zielframework mindestens Android-Version und Ziel-Android-Version ist beschränkt auf den Bereich des Android SDK-Versionen, die Sie installiert haben. Der SDK-Manager können sicherstellen, dass die erforderliche Android-SDK-Versionen installiert sind, und können sie eine neue API-Ebenen hinzufügen, die Sie für Ihre app benötigen. Wenn Sie nicht mit API-Ebenen installieren vertraut sind, finden Sie unter [Android SDK-Installation](~/android/get-started/installation/android-sdk.md).

<a name="framework" />

### <a name="target-framework"></a>Zielframework

Die *Zielframework* (auch bekannt als `compileSdkVersion`) ist die bestimmte Android Frameworkversion (API-Ebene), die Ihre app für während des Buildvorgangs kompiliert wird. Diese Einstellung gibt an, welche APIs Ihrer app *erwartet* verwenden, wenn er ausgeführt wird, aber sie keine Auswirkungen auf die APIs hat nach der Installation wird für Ihre app tatsächlich verfügbar sind. Ändern der Einstellung Zielframework ändert sich daher nicht auf Laufzeitverhalten aus.

Das Zielframework identifiziert, welche Versionen der Bibliothek für Ihre Anwendung verknüpft ist &ndash; Dadurch wird bestimmt, welche APIs, die Sie in Ihrer app verwenden können. Angenommen, Sie verwenden möchten die [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/) Methode, die in Android 5.0 Lollipop eingeführt wurde, müssen Sie das Zielframework auf festlegen **API-Ebene 21 (Lollipop)** oder höher. Wenn Sie Zielframework des Projekts an eine API, z. B. Ebene festlegen **API-Ebene 19 (KitKat)** , und wiederholen Sie zum Aufrufen der `SetCategory` -Methode in Ihrem Code erhalten Sie einen Compilerfehler.

Es wird empfohlen, immer beim Kompilieren mit der *neueste* verfügbare Zielframework-Version. Auf diese Weise bietet Ihnen hilfreich Warnmeldungen für alle nicht mehr unterstützte APIs, die von Ihrem Code aufgerufen werden kann. Anhand der neuesten Version des Zielframeworks ist besonders wichtig, wenn Sie die aktuellen Support Library-Versionen verwenden &ndash; jede Bibliothek erwartet, dass Ihre app auf diesem Unterstützungsbibliothek minimale API-Ebene kompiliert oder größer sein. 

> [!NOTE]
> Ab August 2018, die Konsole der Google wiedergeben setzt voraus, dass neue apps als API-Ebene 26 (Android 8.0 Ziel) oder höher.
Vorhandene apps müssen für die API-Ebene 26 oder höhere Versionen ab November 2018 gelten. Weitere Informationen finden Sie unter [Verbesserung der app-Sicherheit und Leistung auf Google Play nach Jahren kommen](https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html).


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Öffnen Sie in Visual Studio das Zielframework-Einstellung für den Zugriff auf die Projekteigenschaften im **Projektmappen-Explorer** , und wählen Sie die **Anwendung** Seite:

[![Seite "Anwendung" von Projekteigenschaften](android-api-levels-images/vs-target-framework-sml.png)](android-api-levels-images/vs-target-framework.png#lightbox)

Legen Sie das Zielframework durch Auswählen von ein API-Ebene in der Dropdown-Menü unter **Compile using Android Version** wie oben gezeigt.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Für den Zugriff auf die Zielframework-Einstellung in Visual Studio für Mac, mit der rechten Maustaste des Projektnamens, und wählen Sie **Optionen**; Dies öffnet die **Projektoptionen** Dialogfeld. Wechseln Sie in diesem Dialogfeld zu **erstellen > Allgemein** wie hier gezeigt:

[![Abschnitt "Allgemein" der Optionsseite Projekt erstellen](android-api-levels-images/xs-target-framework-sml.png)](android-api-levels-images/xs-target-framework.png#lightbox)

Legen Sie das Zielframework durch Auswählen von ein API-Ebene in der Dropdown-Menü auf der rechten Seite des **Zielframework** wie oben gezeigt.

-----


<a name="minimum" />

### <a name="minimum-android-version"></a>Mindestversion von Android

Die *mindestens Android-Version* (auch bekannt als `minSdkVersion`) wird die älteste Version von Android-Betriebssysteme (d. h. die unterste Ebene API), das Installieren und die Anwendung ausführen können. Standardmäßig kann eine app nur auf Geräten, die mit der Einstellung Zielframework übereinstimmt oder höher werden; Wenn die Einstellung für mindestens Android-Version ist *niedrigere* als die Einstellung Zielframework kann Ihre app auch in früheren Versionen von Android ausführen. Z. B., wenn Sie das Zielframework auf **Android 7.1 (Nougat)** , und legen Sie die mindestens Android-Version auf **Android 4.0.3 (Eis Rustikal Sandwich)**, Ihre app auf einer beliebigen Plattform von API-Ebene 15 installiert werden kann. Um API-Ebene, 25, inklusive.

Auch wenn Ihre Anwendung möglicherweise erfolgreich erstellen und installieren Sie auf diese zahlreichen Plattformen, dies ist keine Garantie, dass es erfolgreich wird *ausführen* für alle von diesen Plattformen. Z. B. wenn die app installiert ist, auf **Android 5.0.x (Lollipop)** und Ihren Code aufruft, eine API, die nur in **Android 7.1 (Nougat)** und höher ist Ihre app wird ein Laufzeitfehler auftreten und stürzt möglicherweise ab. Aus diesem Grund muss Codes sicherstellen &ndash; zur Laufzeit &ndash; , dass sie nur diese APIs aufruft, die von der Android-Gerät unterstützt werden, die ausgeführt wird. Code muss also explizite Runtime Überprüfungen aus, um sicherzustellen, dass Ihre app neuere APIs nur auf Geräten, die aktuell genug, um diese zu unterstützen, sind enthalten.
[Laufzeitprüfungen für Android-Versionen](#runtimechecks)weiter unten in diesem Handbuch wird erläutert, wie diese Überprüfungen zur Laufzeit in den Code einfügen.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Öffnen Sie die Einstellung für mindestens Android in Visual Studio für den Zugriff auf die Projekteigenschaften im **Projektmappen-Explorer** , und wählen Sie die **Android-Manifest** Seite. In der Dropdown-Menü unter **mindestens Android-Version** können, wählen Sie die mindestens Android-Version für Ihre Anwendung:

[![Minimale Android Zieloption legen Sie die Kompilierung mithilfe der SDK-version](android-api-levels-images/vs-minimum-version-sml.png)](android-api-levels-images/vs-minimum-version.png#lightbox)

Bei Auswahl des **verwenden Kompilieren mit SDK Version**, die mindestens Android-Version wird die Einstellung Zielframework identisch sein.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Für den Zugriff auf die Zielframework-Einstellung in Visual Studio für Mac, mit der rechten Maustaste des Projektnamens, und wählen Sie **Optionen**; Dies öffnet die **Projektoptionen** Dialogfeld. Navigieren Sie zu **erstellen > Android-Anwendung**.
Mithilfe des Dropdown-Menüs auf der rechten Seite des **mindestens Android-Version**, Sie können das Minimum Android-Version für Ihre Anwendung festlegen:

[![Mindestversion von Android auf Automatic - Framework-Zielversion verwenden festgelegt](android-api-levels-images/xs-minimum-version-sml.png)](android-api-levels-images/xs-minimum-version.png#lightbox)

Bei Auswahl des **automatische &ndash; verwenden Sie die Zielframeworkversion**, die mindestens Android-Version wird die Einstellung Zielframework identisch sein.

-----


<a name="target" />

### <a name="target-android-version"></a>Android-Zielversion

Die *Android Zielversion* (auch bekannt als `targetSdkVersion`) ist die API von Android-Gerät, an dem die app ausführen. Android verwendet diese Einstellung bestimmt, ob alle Kompatibilität Verhalten ermöglichen &ndash; Dadurch wird sichergestellt, dass Ihre app weiterhin funktioniert wie erwartet. Android wird die Einstellung für Android-Ziel Ihrer App verwendet, um herauszufinden, welches verhaltensänderungen können in Ihrer app übernommen werden ohne diese (Dies ist, wie Android Aufwärtskompatibilität bereitstellt) zu verletzen.

Das Zielframework und die Ziel-Android-Version, während Sie eine sehr ähnliche Namen sind nicht dasselbe. Die Einstellung Zielframework kommuniziert-API-Ebene Zielinformation an Xamarin.Android für die Verwendung zur *Kompilierzeit*, während die Ziel-Android-Version-API-Ebene Zielinformation an Android für die Verwendung zur kommuniziert  *zur Laufzeit* (wenn die app installiert ist und ausgeführt auf einem Gerät).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Öffnen Sie diese Einstellung in Visual Studio für den Zugriff auf die Projekteigenschaften im **Projektmappen-Explorer** , und wählen Sie die **Android-Manifest** Seite. In der Dropdown-Menü unter **Ziel Android-Version** können, wählen Sie die Ziel-Android-Version für Ihre Anwendung:

[![Android-Zielversion festlegen, die die Kompilierung mithilfe der SDK-version](android-api-levels-images/vs-target-version-sml.png)](android-api-levels-images/vs-target-version.png#lightbox)

Es wird empfohlen, dass Sie explizit die Ziel-Android-Version auf die neueste Version von Android festlegen, mit denen Sie die app zu testen. Im Idealfall sollte auf die neueste Android SDK-Version festgelegt werden &ndash; Dies ermöglicht Ihnen die Verwendung der neuen APIs vor dem Durcharbeiten der verhaltensänderungen. Für die meisten Entwickler wir *nicht* sollten Sie die Ziel-Android-Version auf festlegen **verwenden Kompilieren mithilfe von SDK-Version**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Für den Zugriff auf die Zielframework-Einstellung in Visual Studio für Mac, mit der rechten Maustaste des Projektnamens, und wählen Sie **Optionen**; Dies öffnet die **Projektoptionen** Dialogfeld. Navigieren Sie zu **erstellen > Android-Anwendung**.
Mithilfe des Dropdown-Menüs auf der rechten Seite des **Ziel Android-Version**, Sie können die Ziel-Android-Version für die Anwendung festlegen:

[![Android-Zielversion festgelegt wird, die automatische - Framework-Zielversion verwenden](android-api-levels-images/xs-target-version-sml.png)](android-api-levels-images/xs-target-version.png#lightbox)

Es wird empfohlen, dass Sie explizit die Ziel-Android-Version auf die neueste Version von Android festlegen, mit denen Sie die app zu testen. Im Idealfall sollten sie auf die neuesten verfügbaren Android SDK-Version festgelegt werden &ndash; Dies ermöglicht Ihnen die Verwendung der neuen APIs vor dem Durcharbeiten der verhaltensänderungen. Für die meisten Entwickler nicht empfehlenswert, Festlegen der Ziel-Android-Version auf **Automatic - Framework-Zielversion verwenden**.

-----

Im Allgemeinen sollte die Android-Version des Zielservers durch die mindestens erforderliche Android-Version und das Zielframework umschlossen werden. Dies bedeutet:

**Mindestversion von Android < Ziel-Android-Version = < = Zielframework**

Weitere Informationen zu Ebenen SDK finden Sie unter der Android-Entwickler [verwendet Sdk](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html) Dokumentation.


<a name="runtimechecks" />

## <a name="runtime-checks-for-android-versions"></a>Überprüfungen zur Laufzeit für Android-Versionen

Wie jede neue Version von Android veröffentlicht wird, wird das Framework-API aktualisiert, um neue bereitzustellen oder Ersatzfunktionalität. Abgesehen von wenigen Ausnahmen ist-API-Funktionen aus früheren Android-Versionen in neueren Android-Version ohne Änderungen übernommen. Daher werden Ihre app auf einer bestimmten Android-API-Ebene ausgeführt wird, in der Regel kann auf einer höheren Android-API-Ebene ohne Änderungen ausgeführt. Aber was geschieht, wenn Sie auch in früheren Versionen von Android Ihrer app ausführen möchten?

Wenn Sie eine Minimum Android-Version auswählen, ist *niedrigere* als die Einstellung Zielframework einige APIs möglicherweise nicht für Ihre app zur Laufzeit verfügbar. Allerdings kann Ihre app auf einem früheren-Gerät, aber mit eingeschränkter Funktionalität weiterhin ausgeführt. Für jede API, die auf Android-Plattformen entsprechend Ihrer Einstellung für mindestens Android-Version nicht verfügbar ist, muss Ihr Code explizit den Wert der Überprüfen der `Android.OS.Build.VERSION.SdkInt` -Eigenschaft können Sie die API-Ebene der Plattform zu bestimmen, auf die app ausgeführt wird. Wenn die API-Ebene ist *niedrigere* als das Minimum Android-Version, die die API unterstützt aufgerufen werden soll, und der Code hat, um die Suche nach einer Möglichkeit ordnungsgemäß funktioniert, ohne dass dieser API-Aufruf.

Beispielsweise angenommen, wir verwenden möchten die [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/) Methode, um eine Benachrichtigung zu kategorisieren, bei der Ausführung unter **Android 5.0 Lollipop** (und höher), wollen wir noch unserer app in früheren Versionen von Android ausführen, z. B. **Android 4.1 Gelee Bean** (wobei `SetCategory` ist nicht verfügbar). Erkennen, dass in Bezug auf die Android-Version-Tabelle am Anfang dieses Handbuchs, den Code von Version Build für **Android 5.0 Lollipop** ist `Android.OS.BuildVersionCodes.Lollipop`. Zur Unterstützung von älterer Versionen von Android Where `SetCategory` ist nicht verfügbar ist, im Code die API-Ebene zur Laufzeit zu erkennen und bedingt aufrufen kann `SetCategory` nur, wenn die API-Ebene größer oder gleich den Lollipop Build Versionscode ist:

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop)
{
    builder.SetCategory(Notification.CategoryEmail);
}
```

In diesem Beispiel unserer app Zielframework festgelegt ist, um **Android 5.0.x (API-Ebene 21)** und seine mindestens Android-Version festgelegt ist **Android 4.1 (API Level 16)**. Da `SetCategory` finden Sie in der API-Ebene `Android.OS.BuildVersionCodes.Lollipop` und später in diesem Beispielcode ruft `SetCategory` nur, wenn es tatsächlich verfügbar ist &ndash; wird *nicht* versuchen, rufen Sie `SetCategory` bei der API Ebene ist 16, 17, 18, 19 oder 20. Nur in dem Maße, Benachrichtigungen nicht sortiert werden, werden ordnungsgemäß (da sie nicht nach Typ kategorisiert werden), aber die Benachrichtigungen immer noch veröffentlicht werden, damit den Benutzer benachrichtigt werden, die Funktionalität auf diese zuvor Android-Versionen reduziert. Unsere app weiterhin funktioniert, aber seine Funktionalität wird etwas verringert.

Im Allgemeinen unterstützt die Überprüfung der Build-Version den Code zur Laufzeit zwischen Aktionen, die die neue Methode im Vergleich zu die bisherige Methode entscheiden. Zum Beispiel:

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop)
{
    // Do things the Lollipop way
}
else
{
    // Do things the pre-Lollipop way
}
```

Es gibt keine schnelle und einfache Regel, die erklärt, wie verringern, oder ändern Ihre app Funktionen bei der Ausführung in älteren Android-Versionen, die eine oder mehrere APIs Transaktion fehlen, sind. In einigen Fällen (z. B. in der `SetCategory` oben angeführten Beispiel), es ist ausreichend, einfach die API-Aufruf weglassen, wenn er nicht verfügbar ist. Allerdings müssen Sie in anderen Fällen alternative Funktionalität zu implementieren, für den Fall `Android.OS.Build.VERSION.SdkInt` wurde festgestellt, dass Sie kleiner als die API Level sein, dass Ihre app die optimalen Erfahrung zu präsentieren muss.

<a name="libraries" />

## <a name="api-levels-and-libraries"></a>API-Ebenen und Bibliotheken

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Wenn Sie ein Klassenbibliotheksprojekt Xamarin.Android (z. B. einer Klassenbibliothek oder eine Bibliothek Bindungen) erstellen, können Sie konfigurieren, dass nur die Zielframework-Einstellung &ndash; mindestens Android-Version und die Ziel-Android-Version-Einstellungen sind nicht verfügbar. Da besteht also keine **Android-Manifest** Seite:

[![Nur die Kompilierung mit Android-Version-Option verfügbar ist.](android-api-levels-images/vs-library-options-sml.png)](android-api-levels-images/vs-library-options.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Wenn Sie eine Xamarin.Android-Steuerelementbibliothek-Projekt erstellen, besteht keine **Android-Anwendung** Seite dort Sie, die mindestens Android-Version und der Ziel-Android-Version konfigurieren können &ndash; dem Minimum Android-Version und dem Ziel Es sind keine Einstellungen für Android-Version verfügbar.
Da besteht also keine **erstellen > Android-Anwendung** Seite):

[![Erstellen Sie die Seite "Allgemein" ohne Optionen für Minimum und Ziel-version](android-api-levels-images/xs-library-options-sml.png)](android-api-levels-images/xs-library-options.png#lightbox)

-----

Das Minimum Android-Version und Ziel Android versionseinstellungen sind nicht verfügbar, da die resultierende Bibliothek nicht um eine eigenständige app ist &ndash; die Bibliothek ausgeführt werden kann, auf einer Android-Version, je nach der app, die sie mit gepackt ist. Sie können angeben, wie die Bibliothek werden als *kompiliert*, aber Sie können nicht vorhersagen, welche Plattform API-Ebene die Bibliothek ausgeführt wird. In diesem Sinn sollten die folgenden bewährten Methoden Beachten Sie beim Verarbeiten oder Erstellen von Bibliotheken:

-   **Wenn Sie eine Bibliothek für Android verwenden** &ndash; Wenn Sie eine Bibliothek für Android in Ihrer Anwendung verwenden, müssen Sie Ihre app Zielframework festlegen an eine API, d. h. festlegen *mindestens so hoch wie* das Ziel Framework-Einstellung der Bibliothek.

-   **Beim Erstellen einer Bibliothek für Android** &ndash; , wenn Sie eine Android-Bibliothek für die Verwendung von anderen Anwendungen erstellen, werden Sie sicher, dass die Zielframework-Einstellung auf die minimale API-Ebene festgelegt, die es benötigt, um zu kompilieren.

Diese bewährten Methoden werden empfohlen, um zu verhindern, die Situation, in dem eine Bibliothek versucht, eine API aufrufen, die nicht zur Laufzeit verfügbar ist (die die app abstürzt führen kann). Wenn Sie eine Bibliothek-Entwickler sind, sollten Sie sich bemühen, um die Verwendung der API-Aufrufe an eine kleine und zuverlässige Teilmenge der gesamte API-Oberfläche zu beschränken. Dadurch stellen Sie sicher, dass die Bibliothek sicher über einen breiteren Bereich von Android-Versionen verwendet werden kann.


## <a name="summary"></a>Zusammenfassung

Dieses Handbuch erläutert, wie Android-API-Ebenen zum Verwalten von app-Kompatibilität in den verschiedenen Versionen von Android verwendet werden. Diese detaillierte Schritte zum Konfigurieren der Xamarin.Android angegeben *Zielframework*, *mindestens Android-Version*, und *Ziel Android-Version* projekteinstellungen. Es angegeben. Anweisungen zur Verwendung von Android SDK Manager SDK-Paketen installiert enthalten Beispiele für das Schreiben von Code für den Umgang mit verschiedenen API-Ebenen zur Laufzeit und erläutert, wie die API-Ebenen beim Erstellen oder nutzen von Android-Bibliotheken verwalten. Außerdem wird eine umfassende Liste, die API-Ebenen in auf Android-Version-Ziffern (z. B. Android 4.4), Namen von Android-Version (z. B. Kitkat) und Xamarin.Android Build Versionscodes Bezug bereitgestellt.


## <a name="related-links"></a>Verwandte Links

- [Android SDK-Setup](~/android/get-started/installation/android-sdk.md)
- [SDK-CLI-Tools geändert wird.](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Auswählen Ihrer CompileSdkVersion MinSdkVersion und targetSdkVersion](https://medium.com/google-developers/picking-your-compilesdkversion-minsdkversion-targetsdkversion-a098a0341ebd)
- [Was ist die API-Ebene?](http://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)
- [Codenames, Tags und Buildnummern](https://source.android.com/source/build-numbers)
