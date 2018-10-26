---
title: Grundlegendes zu Android-API-Ebenen
description: Xamarin.Android hat verschiedene Android-API-Einstellungen, die bestimmen, die Kompatibilität Ihrer app mit mehreren Versionen von Android. Dieses Handbuch erklärt, was bedeutet, dass diese Einstellungen, wie Sie diese konfigurieren und welche Auswirkungen auf Ihre app zur Laufzeit aufweisen.
ms.prod: xamarin
ms.assetid: 58CB7B34-3140-4BEB-BE2E-209928C1878C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/21/2018
ms.openlocfilehash: aa522e5226d78c1b43bb52b97991b989491d251f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120059"
---
# <a name="understanding-android-api-levels"></a>Android API-Ebenen

_Xamarin.Android hat verschiedene Android-API-Einstellungen, die bestimmen, die Kompatibilität Ihrer app mit mehreren Versionen von Android. Dieses Handbuch erklärt, was bedeutet, dass diese Einstellungen, wie Sie diese konfigurieren und welche Auswirkungen auf Ihre app zur Laufzeit aufweisen._


## <a name="quick-start"></a>Schnellstart

Xamarin.Android macht drei projekteinstellungen der Android-API-Ebene verfügbar:

-   [Zielframework](#framework) &ndash; gibt an, welches Framework beim Erstellen Ihrer Anwendung verwendet. Diese API-Ebene können Sie zur *Kompilieren* Zeit von Xamarin.Android.

-   [Android-Mindestversion](#minimum) &ndash; gibt an, die älteste Android-Version, dass Sie Ihre app unterstützen soll. Diese API-Ebene können Sie zur *ausführen* Zeit von Android.

-   [Android-Zielversion](#target) &ndash; gibt die Version von Android, die Ihre app ist für die Ausführung auf vorgesehen. Diese API-Ebene können Sie zur *ausführen* Zeit von Android.

Bevor Sie API-Ebene für Ihr Projekt konfigurieren können, müssen Sie die SDK-Plattform-Komponenten für diese API-Ebene installieren. Weitere Informationen zum Herunterladen und Installieren von Android SDK-Komponenten finden Sie unter [Android SDK-Setup](~/android/get-started/installation/android-sdk.md).

> [!NOTE]
> Ab August 2018, die Google Play Console setzt voraus, dass neue apps als-API-Ebene 26 (Android 8.0 Ziel) oder höher.
Vorhandene apps müssen für API-Ebene 26 oder höhere Versionen ab November 2018. Weitere Informationen finden Sie unter [Verbessern der app-Sicherheit und Leistung in Google Play jahrelang stammen](https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html).

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Normalerweise werden alle drei Ebenen von Xamarin.Android-API auf den gleichen Wert festgelegt. Auf der **Anwendung** Seite **Kompilieren mit der Android-Version (Zielframework)** auf die neueste stabile API-Version (oder zumindest auf die Android-Version, die über alle Funktionen verfügt, Sie müssen).
Im folgenden Screenshot ist das Zielframework festgelegt ist, um **Android 7.1 (API-Ebene 25 – Nougat)**:

[![Framework-Version standardmäßig Kompilieren mithilfe von Android-Version als Ziel](android-api-levels-images/vs-defaults-sml.png)](android-api-levels-images/vs-defaults.png#lightbox)

Auf der **Android-Manifest** Seite, legen Sie die Version des Android-Mindestversion auf **verwenden Kompilieren mit der SDK-Version** , und legen Sie die Ziel-Android-Version auf den gleichen Wert wie die Zielframeworkversion (in der folgenden Screenshot des Zielframeworks Android nastaven NA hodnotu **Android 7.1 (Nougat)**):

[![Minimum "und" Target Android Version, die auf die Zielframeworkversion festgelegt](android-api-levels-images/vs-manifest-defaults-sml.png)](android-api-levels-images/vs-manifest-defaults.png#lightbox)

Wenn Sie möchten die Abwärtskompatibilität mit einer früheren Version von Android, legen Sie **Minimum Android Version als Ziel** die älteste Version von Android, dass Sie Ihre app unterstützen soll. (Beachten Sie, dass API-Ebene 14 für erforderliche minimale API-Ebene [Google Play-Dienste und Support für Firebase](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html).) Die folgende Beispielkonfiguration unterstützt Android-Versionen von API-Ebene 14 bis API-Ebene 25:

[![Kompilieren mit der API-Ebene 25 Nougat "," Minimum Android Version, die auf API-Ebene 14 festgelegt](android-api-levels-images/vs-minimum-sml.png)](android-api-levels-images/vs-minimum.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Normalerweise werden alle drei Ebenen von Xamarin.Android-API auf den gleichen Wert festgelegt. Legen Sie **Zielframework** auf die neueste stabile API-Version (oder zumindest auf die Android-Version, die über alle Funktionen verfügt, Sie müssen). Festlegen der **Zielframework**, navigieren Sie zu **erstellen > Allgemein** in die **Projektoptionen**. Im folgenden Screenshot ist das Zielframework festgelegt ist, um **verwenden, die zuletzt installierte Plattform (8.0)**:

[![Zuletzt installierte Plattform verwenden als Zielframework](android-api-levels-images/xs-default-target-sml.png)](android-api-levels-images/xs-default-target.png#lightbox)

Die Länge und Target Android Version-Einstellungen finden Sie unter **erstellen > Android-Anwendung** in **Projektoptionen**. Legen Sie die Version des Android-Mindestversion auf **automatisch – Zielframeworkversion verwenden** und die Ziel-Android-Version auf den gleichen Wert wie die Zielframeworkversion festgelegt. Der folgende Screenshot zeigt, das Ziel Android-Framework festgelegt ist, um **Android 8.0 (API-Ebene 26)** entsprechend die obigen Zielframework-Einstellung:

[![Die Ziel- und Framework-Ebenen festlegen in den Projektoptionen](android-api-levels-images/xs-default-app-sml.png)](android-api-levels-images/xs-default-app.png#lightbox)

Wenn Sie Abwärtskompatibilität mit einer früheren Version von Android beibehalten möchten, ändern **Minimum Android Version** die älteste Version von Android, dass Sie Ihre app unterstützen soll. Beachten Sie, dass API-Ebene 14 für erforderliche minimale API-Ebene [Google Play-Dienste und Support für Firebase](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html).
Beispielsweise unterstützt die folgende Konfiguration so früh wie API-Ebene 14 Android-Versionen aus:

[![Minimum und Ziel-Versionen, die automatisch - verwenden die Framework-Zielversion](android-api-levels-images/xs-minimum-sml.png)](android-api-levels-images/xs-minimum.png#lightbox)

-----


Wenn Ihre app mehrere Android-Versionen unterstützt, muss Ihr Code laufzeitüberprüfungen, um sicherzustellen, dass Ihre app, klicken Sie mit der Android-mindesteinstellung funktioniert enthalten (finden Sie unter [Laufzeitprüfungen für Android-Versionen](#runtimechecks) unten Weitere Informationen). Wenn Sie nutzen oder eine Bibliothek erstellen, finden Sie unter [-API-Ebenen und Bibliotheken](#libraries) unten Informationen zu bewährten Vorgehensweisen bei der Konfiguration von API-Ebene-Einstellungen für Bibliotheken.



## <a name="android-versions-and-api-levels"></a>Android-Versionen und API-Ebenen

Die Android-Plattform entwickelt, und neue Android-Versionen veröffentlicht werden, erhält jede Android-Version einen eindeutige ganzzahlige Bezeichner, mit dem Namen der *-API-Ebene*. Daher entspricht jede Android-Version auf eine einzelne Ebene der Android-API ein. Da Benutzer apps auf älteren auch als die neuesten Versionen von Android installieren, müssen realen Android-apps zum Arbeiten mit mehreren Android-API-Ebenen entworfen werden.


### <a name="android-versions"></a>Android-Versionen

Jede Version von Android hat mehrere Namen:

-   Die Android-Version, z. B. **Android 9.0**
-   Ein Code (oder Dessert) umbenennen, z. B. _Kreis_
-   Eine entsprechende API-Ebene, z. B. **-API-Ebene 28**

Ein Android-Code-Name entspricht möglicherweise mehrerer Versionen und API-Ebenen (wie in der folgenden Tabelle dargestellt), aber jede Android-Version entspricht genau eine API-Ebene.

Darüber hinaus Xamarin.Android definiert *Erstellen des Versionscodes* , der an die derzeit gültigen Android-API-Ebenen zuordnen. In der folgende Tabelle können Sie die API-Ebene, Android-Version, Codename und Xamarin.Android-Build-Version-Code zu konvertieren (Build Versionscodes werden definiert, der `Android.OS` Namespace):

[!include[](~/android/includes/api-levels.md)]

Da diese Tabelle gibt an, werden häufig neue Android-Versionen freigegeben &ndash; manchmal mehr als eine Version pro Jahr. Daher gehören das Universum von Android-Geräten, die Ihre app ausführen, kann eine Vielzahl von älteren und neueren Android-Versionen. Wie können Sie sicherstellen, dass Ihre app so viele unterschiedliche Versionen von Android konsistent und zuverlässig ausgeführt werden? Android-API-Ebenen können Sie dieses Problem zu verwalten.


### <a name="android-api-levels"></a>Android-API-Ebenen

Jedes Android-Gerät auf ausgeführt wird, genau *eine* -API-Ebene &ndash; dieser API-Ebene ist garantiert eindeutig pro Version von Android-Plattform. Die API-Ebene identifiziert genau die Version von der API-Satz, dem Ihre app aufrufen kann. die Kombination der Elemente von Manifesten, Berechtigungen usw., Ihr Code anspricht als Entwickler identifiziert. Android-System von API-Ebenen unterstützt Android, die bestimmen, ob eine Anwendung mit einem Android Systemabbild vor der Installation der Anwendungs auf einem Gerät kompatibel ist.

Wenn eine Anwendung erstellt wird, enthält es die folgende API-Level-Informationen an:

-   Die *Ziel* -API-Ebene von Android, die die app erstellt wird, führen Sie auf.

-   Die *minimale* Android-API-Ebene, die zum Ausführen der app ein Android-Gerät benötigen. 

Diese Einstellungen werden verwendet, um sicherzustellen, dass Funktionen, die benötigt werden, damit Sie die app ordnungsgemäß ausgeführt, die bei der Installation auf dem Android-Gerät verfügbar ist. Wenn dies nicht der Fall ist, wird die Anwendung ausgeführt wird, das Gerät gesperrt. Beispielsweise ist die API-Ebene des Android-Geräten niedriger als die API-Mindestebene, die Sie für Ihre app angeben, wird das Android-Gerät des Benutzers verhindert, dass Ihre app zu installieren.


## <a name="project-api-level-settings"></a>Level-API-projekteinstellungen

In den folgenden Abschnitten wird erläutert, wie der SDK-Manager zu verwenden, um die Entwicklungsumgebung Vorbereiten für die API-Ebenen, Sie möchten, gefolgt von ausführliche erläuterungen zum Konfigurieren von *Zielframework*, *Minimum Android-Version*, und *Target Android Version* Einstellungen in Xamarin.Android.


### <a name="android-sdk-platforms"></a>Android SDK-Plattformen

Bevor Sie eine Ziel oder die Minimum-API-Ebene in Xamarin.Android auswählen können, müssen Sie die Version der Android SDK-Plattform, die ab entspricht installieren, auf diese API-Ebene. Bereich der verfügbaren Auswahlmöglichkeiten für das Zielframework, Minimum Android-Version und Target Android Version ist beschränkt auf den Bereich des Android SDK-Versionen, die Sie installiert haben. Sie können den SDK-Manager verwenden, um sicherzustellen, dass die erforderlichen Android-SDK-Versionen installiert sind und können Sie sie alle neuen API-Ebenen hinzufügen, die Sie für Ihre app benötigen. Wenn Sie nicht mit API-Ebenen installieren vertraut sind, finden Sie unter [Android SDK-Setup](~/android/get-started/installation/android-sdk.md).

<a name="framework" />

### <a name="target-framework"></a>Zielframework

Die *Zielframework* (auch bekannt als `compileSdkVersion`) ist die bestimmte Android-Frameworkversion (API-Ebene), die Ihre app für die zum Zeitpunkt der Erstellung kompiliert wird. Diese Einstellung gibt an, welche APIs Ihre app *erwartet* verwenden, wenn er ausgeführt wird, aber sie keine Auswirkungen auf die APIs hat bei der Installation für Ihre app tatsächlich verfügbar sind. Ändern der Zielframework-Einstellung ändert sich daher nicht auf Laufzeitverhalten aus.

Das Zielframework identifiziert, welche Versionen der Bibliothek für Ihre Anwendung verknüpft ist &ndash; diese Einstellung bestimmt, welche APIs, die Sie in Ihrer app verwenden können. Angenommen, Sie verwenden möchten die [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/) Methode, die in Android 5.0 Lollipop eingeführt wurde, müssen Sie das Zielframework auf festlegen **API Level 21 (Lollipop)** oder höher. Wenn Sie Zielframework des Projekts zu einer API wie z. B. festgelegt **API-Ebene 19 (KitKat)** und versuchen Sie es zum Aufrufen der `SetCategory` Methode im Code, erhalten Sie einen Kompilierungsfehler.

Es wird empfohlen, dass Sie immer die Kompilierung mit der *neueste* verfügbare Zielframework-Version. Auf diese Weise bietet Ihnen hilfreiche Fehlermeldungen für alle veralteten APIs, die möglicherweise von Ihrem Code aufgerufen wird. Mit der neuesten Zielframework-Version ist besonders wichtig, wenn Sie die neuesten Versionen der Support-Bibliothek verwenden &ndash; jede Bibliothek erwartet, dass Ihre app an die API-Mindestebene, Unterstützungsbibliothek kompilierten oder höher sein. 


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Öffnen Sie das Zielframework-Einstellung in Visual Studio für den Zugriff auf die Projekteigenschaften in **Projektmappen-Explorer** , und wählen Sie die **Anwendung** Seite:

[![Seite "Anwendung" der Projekteigenschaften](android-api-levels-images/vs-target-framework-sml.png)](android-api-levels-images/vs-target-framework.png#lightbox)

Legen Sie das Zielframework durch Auswahl von API-Ebene im Dropdown-Menü unter **Kompilieren mit der Android-Version** wie oben gezeigt.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Für den Zugriff auf die Zielframework-Einstellung in Visual Studio für Mac, mit der rechten Maustaste in des Namens des Projekts, und wählen Sie **Optionen**; Dies öffnet die **Projektoptionen** Dialogfeld. Wechseln Sie in diesem Dialogfeld zu **erstellen > Allgemein** wie hier gezeigt:

[![Erstellen Sie die Seite "Projektoptionen" Abschnitt "Allgemein"](android-api-levels-images/xs-target-framework-sml.png)](android-api-levels-images/xs-target-framework.png#lightbox)

Legen Sie das Zielframework durch Auswahl von API-Ebene in der Dropdown-Menü auf der rechten Seite des **Zielframework** wie oben gezeigt.

-----


<a name="minimum" />

### <a name="minimum-android-version"></a>Android-Mindestversion

Die *Minimum Android Version* (auch bekannt als `minSdkVersion`) wird die älteste Version des Android-Betriebssystems (d.h. die niedrigste API-Ebene), das Installieren und Ihre Anwendung ausführen können. Standardmäßig kann eine app nur auf Geräten, die mit der Zielframework-Einstellung übereinstimmt installiert oder höher sein. Wenn die Einstellung für Android-Mindestversion ist *niedrigere* als die Zielframework-Einstellung, kann Ihre app auch auf früheren Versionen von Android ausführen. Z. B., wenn Sie das Zielframework auf **Android 7.1 (Nougat)** , und legen Sie die Version des Android-Mindestversion auf **Android 4.0.3 (Ice Cream Sandwich)**, Ihre app auf jeder Plattform aus API-Ebene 15 installiert werden kann. Um API-Ebene 25, inklusive.

Auch wenn Ihre app möglicherweise erfolgreich erstellen und installieren Sie auf diesem Bereich der Plattformen, dies garantiert nicht, dass erfolgreich wird *ausführen* für alle diese Plattformen. Z. B., wenn Ihre app installiert ist, auf **Android 5.0 (Lollipop)** und Ihren Code Ruft eine API, die nur in verfügbar ist **Android 7.1 (Nougat)** und höher ist Ihre app wird ein Laufzeitfehler ausgelöst und stürzt möglicherweise ab. Aus diesem Grund müssen Ihren Code &ndash; zur Laufzeit &ndash; , dass sie die APIs aufruft, die von der Android-Gerät unterstützt werden, die es ausgeführt wird. Das heißt, muss Ihre Code explizite laufzeitüberprüfungen, um sicherzustellen, dass neuere APIs in Ihre app nur auf Geräten verwendet wird, die aktuell genug, um deren Unterstützung sind enthalten.
[Laufzeitprüfungen für Android-Versionen](#runtimechecks)weiter unten in diesem Handbuch wird erläutert, wie diese Überprüfungen zur Laufzeit zu Ihrem Code hinzufügen.


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Öffnen Sie die Einstellung für Android-Mindestversion in Visual Studio für den Zugriff auf die Projekteigenschaften in **Projektmappen-Explorer** , und wählen Sie die **Android-Manifest** Seite. Im Dropdown-Menü unter **Minimum Android Version** können Sie die Minimum Android Version für Ihre Anwendung auswählen:

[![Android-Mindestversion Zieloption legen Sie die Kompilierung mithilfe von SDK-version](android-api-levels-images/vs-minimum-version-sml.png)](android-api-levels-images/vs-minimum-version.png#lightbox)

Bei Auswahl von **verwenden Kompilieren mit der SDK-Version**, die mindestens Android-Version wird die Zielframework-Einstellung identisch sein.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Für den Zugriff auf die Minimum Android Version in Visual Studio für Mac mit der rechten Maustaste in des Namens des Projekts, und wählen Sie **Optionen**; Dies öffnet die **Projektoptionen** Dialogfeld. Navigieren Sie zu **erstellen > Android-Anwendung**.
Mithilfe der Dropdown-Menü auf der rechten Seite des **Minimum Android Version**, Sie können die Minimum Android-Version für Ihre Anwendung festlegen:

[![Android-Mindestversion automatisch – Zielframeworkversion verwenden](android-api-levels-images/xs-minimum-version-sml.png)](android-api-levels-images/xs-minimum-version.png#lightbox)

Bei Auswahl von **automatische &ndash; verwenden Framework-Zielversion**, die mindestens Android-Version wird die Zielframework-Einstellung identisch sein.

-----


<a name="target" />

### <a name="target-android-version"></a>Android-Zielversion

Die *Android-Zielversion* (auch bekannt als `targetSdkVersion`) ist die API Ebene des Android-Geräts, an dem die app ausführen. Android verwendet diese Einstellung, um zu ermitteln, ob kompatibilitätsverhaltensweisen aktiviert &ndash; Dadurch wird sichergestellt, dass Ihre app weiterhin funktioniert wie erwartet. Android verwendet die Einstellung für Android-Ziel Ihrer App, um zu ermitteln, welche verhaltensänderungen können zu Ihrer app angewendet werden ohne diese (Dies ist, wie Android Aufwärtskompatibilität bietet) zu verletzen.

Die MSBuild-Zielframework und die Ziel-Android-Version, während Sie eine sehr ähnliche Namen sind nicht dasselbe. Die Zielframework-Einstellung kommuniziert die Ebene der Informationen der Ziel-API in Xamarin.Android für die Verwendung zur *Kompilierzeit*, während die Ziel-Android-Version auf Informationen zu Android Ziel-API für die Verwendung zur kommuniziert  *zur Laufzeit* (wenn die app installiert ist und ausgeführt auf einem Gerät).

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Öffnen Sie diese Einstellung in Visual Studio für den Zugriff auf die Projekteigenschaften in **Projektmappen-Explorer** , und wählen Sie die **Android-Manifest** Seite. Im Dropdown-Menü unter **Target Android Version** können Sie den Target Android Version für Ihre Anwendung auswählen:

[![Android-Zielversion festlegen, die die Kompilierung mithilfe von SDK-version](android-api-levels-images/vs-target-version-sml.png)](android-api-levels-images/vs-target-version.png#lightbox)

Es wird empfohlen, dass Sie explizit die Ziel-Android-Version auf die neueste Version von Android festlegen, mit denen Sie Ihre app zu testen. Sie sollten im Idealfall festgelegt werden, auf die neueste Version des Android SDK &ndash; dadurch, dass Sie mit der neuen APIs vor dem Durcharbeiten der verhaltensänderungen. Für die meisten Entwickler wir *nicht* wird empfohlen, die Target Android Version festlegen, um **verwenden Kompilieren mit der SDK-Version**.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Um diese Einstellung in Visual Studio für Mac zugreifen zu können, mit der rechten Maustaste in des Namens des Projekts, und wählen Sie **Optionen**; Dies öffnet die **Projektoptionen** Dialogfeld. Navigieren Sie zu **erstellen > Android-Anwendung**. Mithilfe der Dropdown-Menü auf der rechten Seite des **Target Android Version**, Sie können die Ziel-Android-Version für Ihre Anwendung festlegen:

[![Android-Zielversion festlegen, um automatisch – Zielframeworkversion verwenden](android-api-levels-images/xs-target-version-sml.png)](android-api-levels-images/xs-target-version.png#lightbox)

Es wird empfohlen, dass Sie explizit die Ziel-Android-Version auf die neueste Version von Android festlegen, mit denen Sie Ihre app zu testen. Sie sollten im Idealfall festgelegt werden, auf die neueste verfügbare Android SDK-Version &ndash; dadurch, dass Sie mit der neuen APIs vor dem Durcharbeiten der verhaltensänderungen. Für die meisten Entwickler, wir empfehlen nicht die Target Android Version festlegen, um **automatisch – Zielframeworkversion verwenden**.

-----

Im Allgemeinen sollten Android-Zielversion durch die mindestens erforderliche Android-Version und dem Zielframework begrenzt werden. Dies bedeutet:

**Android-Mindestversion < Android-Zielversion = < = Zielframework**

Weitere Informationen zu den SDK-Ebenen, finden Sie in der Android-Entwickler [verwendet-Sdk](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html) Dokumentation.


<a name="runtimechecks" />

## <a name="runtime-checks-for-android-versions"></a>Laufzeitprüfungen für Android-Versionen

Wenn jeder neuen Version von Android veröffentlicht wird, wird das API-Framework aktualisiert, um neue bereitzustellen oder Ersatzfunktionen. Mit wenigen Ausnahmen ist-API-Funktionen aus früheren Android-Versionen in neueren Versionen ohne Änderungen übernommen. Daher, wenn Ihre app auf eine bestimmte Android-API-Ebene ausgeführt wird, wird es in der Regel auf einer höheren Android-API-Ebene ohne Änderungen ausgeführt werden können. Aber was geschieht, wenn Sie auch Ihre app auf früheren Versionen von Android ausführen möchten?

Wenn Sie eine Android-Mindestversion Version auswählen, ist *niedrigere* als die Zielframework-Einstellung, einige APIs möglicherweise nicht für Ihre app zur Laufzeit verfügbar. Allerdings kann Ihre app weiterhin auf einem älteren Gerät, jedoch mit eingeschränkter Funktionalität ausführen. Für jede API, die auf Ihrem Android-mindesteinstellung für Android-Plattformen nicht verfügbar ist, muss Ihr Code den Wert der explizit überprüfen die `Android.OS.Build.VERSION.SdkInt` Eigenschaft, um die API-Ebene der Plattform zu bestimmen, auf die app ausgeführt wird. Wenn die API-Ebene ist *niedrigere* als das Minimum Android Version, die die API unterstützt Sie aufrufen möchten, und klicken Sie dann hat der Code, um eine Möglichkeit, ordnungsgemäß funktioniert, ohne dass dieser API-Aufruf zu finden.

Beispielsweise nehmen wir an, dass verwendet werden soll die [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/) Methode, um eine Benachrichtigung zu kategorisieren, bei der Ausführung unter **Android 5.0 Lollipop** (und höher), aber wir weiterhin für die app Führen Sie wie in früheren Versionen von Android **Android 4.1 Jelly Bean** (wobei `SetCategory` ist nicht verfügbar). In Bezug auf die Android-Version-Tabelle zu Beginn dieses Handbuchs, sehen Sie, dass der Code des Build-Version für **Android 5.0 Lollipop** ist `Android.OS.BuildVersionCodes.Lollipop`. Zur Unterstützung von älterer Versionen von Android Where `SetCategory` ist nicht verfügbar ist, unser Code können Sie erkennen die API-Ebene zur Laufzeit und bedingt Aufrufen `SetCategory` nur, wenn die API-Ebene größer oder gleich den Versionscode der Lollipop-Build ist:

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop)
{
    builder.SetCategory(Notification.CategoryEmail);
}
```

In diesem Beispiel unserer app Zielframework festgelegt ist, um **Android 5.0 (API Level 21)** und die Version des Android-Mindestversion auf **Android 4.1 (API Level 16)**. Da `SetCategory` finden Sie im API-Ebene `Android.OS.BuildVersionCodes.Lollipop` und später in diesem Beispielcode ruft `SetCategory` nur, wenn es verfügbar ist &ndash; wird *nicht* aufrufen, versucht `SetCategory` bei der API Ebene ist 16, 17, 18, 19 oder 20. Die Funktionalität wird in diese zuvor Android-Versionen reduziert, nur insofern Benachrichtigungen nicht sortiert werden, werden ordnungsgemäß (da sie nicht nach Typ kategorisiert sind), aber die Benachrichtigungen immer noch veröffentlicht werden, um den Benutzer darauf aufmerksam. Unsere app weiterhin funktioniert, aber seine Funktionalität wird leicht verringert.

Im Allgemeinen kann die Build-versionsprüfung Code zur Laufzeit zwischen Aktionen ausgeführt werden und die alte Methode die neue Methode zu entscheiden. Zum Beispiel:

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

Es gibt keine schnelle und einfache Regel, die erklärt, wie Sie verringern, oder ändern den Funktionen Ihrer app aus, wenn sie in älteren Android-Versionen ausgeführt wird, die eine oder mehrere APIs fehlen, werden. In einigen Fällen (z. B. in der `SetCategory` Beispiel oben), es ist ausreichend, um den API-Aufruf zu unterdrücken, wenn sie nicht verfügbar ist. Jedoch in anderen Fällen müssen Sie möglicherweise um alternative Funktionalität zu implementieren, für den Fall `Android.OS.Build.VERSION.SdkInt` wird erkannt, dass Sie kleiner als die API-Stufe aufweisen darf, dass Ihre app benötigt, um die optimale Erlebnis zu bieten.

<a name="libraries" />

## <a name="api-levels-and-libraries"></a>API-Ebenen und Bibliotheken

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Wenn Sie ein Xamarin.Android-Steuerelementbibliothek-Projekt (z. B. eine Klassenbibliothek oder eine bindungsbibliothek) erstellen, können Sie konfigurieren, dass nur die Zielframework-Einstellung &ndash; die Minimum Android-Version und die Target Android versionseinstellungen sind nicht verfügbar. Das ist, da gibt es keine **Android-Manifest** Seite:

[![Nur die Kompilierung mit Android-Version-Option verfügbar ist.](android-api-levels-images/vs-library-options-sml.png)](android-api-levels-images/vs-library-options.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Wenn Sie ein Xamarin.Android-Steuerelementbibliothek-Projekt erstellen, es gibt keine **Android-Anwendung** Seite, die in dem Sie, die mindestens Android-Version und die Target Android Version konfigurieren können &ndash; der Minimum Android-Version und des Ziels Es sind keine Einstellungen für Android-Version verfügbar.
Das ist, da gibt es keine **erstellen > Android-Anwendung** Seite:

[![Erstellen Sie Seite "Allgemein" ohne Optionen für Minimum und Ziel version](android-api-levels-images/xs-library-options-sml.png)](android-api-levels-images/xs-library-options.png#lightbox)

-----

Das Minimum Android-Version und Target Android versionseinstellungen sind nicht verfügbar, da die resultierende Bibliothek nicht um eine eigenständige app ist &ndash; die Bibliothek ausgeführt werden kann, auf jedem Android-Version abhängig von der app, die sie mit verpackt wird. Sie können angeben, wie die Bibliothek ist *kompiliert*, aber Sie können nicht vorhersagen, welche Plattform-API-Ebene die Bibliothek ausgeführt wird. In diesem Sinn die folgenden bewährten Methoden Beachten Sie beim verwenden oder Erstellen von Bibliotheken:

-   **Bei der Nutzung von Android-Bibliothek** &ndash; Wenn Sie eine Android-Bibliothek in Ihrer Anwendung verwenden, werden Sie sicher, dass der app-Zielframework festlegen zu einer API, *mindestens so hoch wie* das Ziel Frameworkeinstellung der Bibliothek.

-   **Wenn Sie eine Android-Bibliothek erstellen** &ndash; , wenn Sie eine Android-Bibliothek für die Verwendung von anderen Anwendungen erstellen, werden Sie sicher, dass die Zielframework-Einstellung auf die API-Mindestebene, die zum Kompilieren erforderlich.

Diese empfohlenen Vorgehensweisen werden empfohlen, um zu verhindern, die Situation, in denen eine Bibliothek versucht, eine API aufrufen, die nicht zur Laufzeit verfügbar ist (die die app zu einem Absturz führen kann). Wenn Sie eine Bibliotheksentwickler sind, sollten Sie sich bemühen, Ihre Nutzung der API-Aufrufe auf einen kleinen und bewährten Teil die gesamte API-Oberfläche einschränken. Dadurch wird sichergestellt, dass die Bibliothek sicher kann, können Sie für eine breitere Palette von Android-Versionen verwendet werden.


## <a name="summary"></a>Zusammenfassung

Dieses Handbuch wurde erklärt, wie Android-API-Ebenen zum Verwalten von app-Kompatibilität für verschiedene Versionen von Android verwendet werden. Er ausführliche Schritte zum Konfigurieren der Xamarin.Android bereitgestellt *Zielframework*, *Minimum Android Version*, und *Target Android Version* projekteinstellungen. Es werden Anweisungen für die Verwendung von Android SDK-Manager zum Installieren des SDK-Pakete enthalten Beispiele zum Schreiben von Code für den Umgang mit verschiedenen API-Ebenen zur Laufzeit, und erläutert, wie zum Verwalten von API-Ebenen beim Erstellen oder Verarbeiten von Android-Bibliotheken bereitgestellt. Zudem enthält er eine umfassende Liste, die API-Ebenen mit Android-Version-Ziffern (z. B. Android 4.4), Android-Version-Namen (z. B. Kitkat) und Versionscodes für Xamarin.Android-Build verknüpft.


## <a name="related-links"></a>Verwandte Links

- [Android SDK-Setup](~/android/get-started/installation/android-sdk.md)
- [SDK-CLI-Tools geändert wird.](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Auswählen Ihrer CompileSdkVersion MinSdkVersion und targetSdkVersion](https://medium.com/google-developers/picking-your-compilesdkversion-minsdkversion-targetsdkversion-a098a0341ebd)
- [Was ist API-Ebene?](http://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)
- [Codenames, Tags und Buildnummern](https://source.android.com/source/build-numbers)
