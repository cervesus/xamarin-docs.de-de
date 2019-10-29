---
title: Informationen zu Android-API-Ebenen
description: Xamarin. Android verfügt über mehrere Einstellungen der Android-API-Ebene, mit denen die Kompatibilität Ihrer APP mit mehreren Versionen von Android bestimmt wird. In diesem Handbuch wird erläutert, was diese Einstellungen bedeuten, wie Sie konfiguriert werden und welche Auswirkungen Sie zur Laufzeit auf Ihre APP haben.
ms.prod: xamarin
ms.assetid: 58CB7B34-3140-4BEB-BE2E-209928C1878C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/21/2018
ms.openlocfilehash: c7bae50b481e9b0473ff97d720f2115e8f7c3d2e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73018180"
---
# <a name="understanding-android-api-levels"></a>Android API-Ebenen

_Xamarin. Android verfügt über mehrere Einstellungen der Android-API-Ebene, mit denen die Kompatibilität Ihrer APP mit mehreren Versionen von Android bestimmt wird. In diesem Handbuch wird erläutert, was diese Einstellungen bedeuten, wie Sie konfiguriert werden und welche Auswirkungen Sie zur Laufzeit auf Ihre APP haben._

## <a name="quick-start"></a>Schnellstart

Xamarin. Android stellt drei Projekteinstellungen auf Android-API-Ebene bereit:

- [Ziel Framework](#framework) &ndash; gibt an, welches Framework beim Entwickeln der Anwendung verwendet werden soll. Diese API-Ebene wird zur *Kompilier* Zeit von xamarin. Android verwendet.

- [Android-Mindestversion](#minimum) &ndash; gibt die älteste Android-Version an, die von ihrer App unterstützt werden soll. Diese API-Ebene wird zur *Laufzeit* von Android verwendet.

- Die [Android-Zielversion](#target) &ndash; die die Version von Android angibt, auf der Ihre APP ausgeführt werden soll. Diese API-Ebene wird zur *Laufzeit* von Android verwendet.

Bevor Sie eine API-Ebene für Ihr Projekt konfigurieren können, müssen Sie die SDK-Platt Form Komponenten für diese API-Ebene installieren. Weitere Informationen zum herunterladen und Installieren von Android SDK-Komponenten finden Sie unter [Android SDK Setup](~/android/get-started/installation/android-sdk.md).

> [!NOTE]
> Ab August 2018 erfordert die Google Play Konsole, dass neue apps auf API Level 26 (Android 8,0) oder höher ausgerichtet sind.
Vorhandene apps müssen ab dem 2018 auf API-Ebene 26 oder höher ausgerichtet sein. Weitere Informationen finden Sie unter [verbessern der App-Sicherheit und-Leistung auf Google Play für Jahre](https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html).

<!-- markdownlint-disable MD001 -->

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Normalerweise werden alle drei xamarin. Android-API-Ebenen auf denselben Wert festgelegt. Legen Sie auf der Seite Anwendung **Kompilierung mit Android-Version (Ziel Framework)** auf die neueste stabile API-Version (oder zumindest auf die Android-Version mit allen benötigten Features) fest.
Im folgenden Screenshot ist das Ziel Framework auf **Android 7,1 (API-Ebene 25-Nougat)** festgelegt:

[Standardmäßig wird![Framework-Version mit der Android-Version kompiliert.](android-api-levels-images/vs-defaults-sml.png)](android-api-levels-images/vs-defaults.png#lightbox)

Legen Sie auf der Seite **Android-Manifest** die Android-Mindestversion für die **Verwendung von Kompilieren mit der SDK-Version** fest, und legen Sie die Android-Zielversion auf denselben Wert fest wie die Zielframeworkversion (im folgenden Screenshot ist das Android-Ziel Framework auf festgelegt. **Android 7,1 (Nougat)** ):

[auf die Ziel Framework-Version festgelegte![-und Android-Ziel Versionen](android-api-levels-images/vs-manifest-defaults-sml.png)](android-api-levels-images/vs-manifest-defaults.png#lightbox)

Wenn Sie die Abwärtskompatibilität mit einer früheren Version von Android gewährleisten möchten, legen Sie die **Android-Mindestversion auf** die älteste Version von Android fest, die von ihrer App unterstützt werden soll. (Beachten Sie, dass API-Ebene 14 die minimale API-Ebene ist, die für [Google Play Dienste und Firebase-Unterstützung](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html)erforderlich ist. Die folgende Beispielkonfiguration unterstützt Android-Versionen von API-Ebene 14 bis API-Ebene 25:

[![Kompilieren mit API-Ebene 25 Nougat, Android-Mindestversion ist auf API-Ebene 14 festgelegt](android-api-levels-images/vs-minimum-sml.png)](android-api-levels-images/vs-minimum.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Normalerweise werden alle drei xamarin. Android-API-Ebenen auf denselben Wert festgelegt. Legen Sie das **Ziel Framework** auf die neueste stabile API-Version (oder zumindest auf die Android-Version fest, die über alle benötigten Features verfügt). Um das **Ziel Framework**festzulegen, navigieren Sie zu **Build > Allgemein** in den **Projektoptionen**. Im folgenden Screenshot ist das Ziel Framework auf **die Verwendung der neuesten installierten Plattform (8,0)** festgelegt:

[![Ziel Framework standardmäßig verwendet, um die neueste installierte Plattform zu verwenden](android-api-levels-images/xs-default-target-sml.png)](android-api-levels-images/xs-default-target.png#lightbox)

Die Einstellungen für die Android-Mindestversion und die Zielversion finden Sie unter **Build > Android-Anwendung** in **Project Options**. Legen Sie die Android-Mindestversion auf die **automatische Verwendung der Ziel Framework-Version** fest, und legen Sie die Android-Zielversion auf denselben Wert wie die Zielframeworkversion fest. Im folgenden Screenshot ist das Android-Ziel Framework auf **Android 8,0 (API-Ebene 26)** festgelegt, um der oben genannten Ziel Framework-Einstellung zu entsprechen:

[![Festlegen der Ziel-und Frameworkebene in den Projektoptionen](android-api-levels-images/xs-default-app-sml.png)](android-api-levels-images/xs-default-app.png#lightbox)

Wenn Sie die Abwärtskompatibilität mit einer früheren Version von Android beibehalten möchten, ändern Sie die **Android-Mindestversion** in die älteste Version von Android, die von ihrer App unterstützt werden soll. Beachten Sie, dass API-Ebene 14 die minimale API-Ebene ist, die für [Google Play Dienste und Firebase-Unterstützung](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html)erforderlich ist
Die folgende Konfiguration unterstützt z. b. Android-Versionen, so früh wie API-Ebene 14:

[![Minimum-und Ziel Versionen sind auf die automatische Verwendung der Ziel Framework-Version festgelegt.](android-api-levels-images/xs-minimum-sml.png)](android-api-levels-images/xs-minimum.png#lightbox)

-----

Wenn Ihre APP mehrere Android-Versionen unterstützt, muss Ihr Code Laufzeitüberprüfungen enthalten, um sicherzustellen, dass Ihre APP mit der Android-Mindestversion funktioniert (Weitere Informationen finden Sie weiter unten unter [Lauf Zeit Prüfungen für Android-Versionen](#runtimechecks) ). Wenn Sie eine Bibliothek nutzen oder erstellen, finden Sie unter [API-Ebenen und-Bibliotheken](#libraries) unten bewährte Methoden für die Konfiguration von API-Ebenen-Einstellungen für Bibliotheken.

## <a name="android-versions-and-api-levels"></a>Android-Versionen und API-Ebenen

Wenn die Android-Plattform weiterentwickelt wird und neue Android-Versionen veröffentlicht werden, wird jeder Android-Version ein eindeutiger ganzzahliger Bezeichner ( *API-Ebene*) zugewiesen. Daher entspricht jede Android-Version einer einzelnen Android-API-Ebene. Da Benutzer apps sowohl auf älteren als auch in den neuesten Versionen von Android installieren, müssen reale Android-Apps für die Verwendung mit mehreren Android-API-Ebenen entworfen werden.

### <a name="android-versions"></a>Android-Versionen

Jede Version von Android wird durch mehrere Namen geleitet:

- Die Android-Version, z. b. **Android 9,0**
- Ein Code (oder ein Nachtisch), z . b. Kreis
- Eine entsprechende API-Ebene, z. b. **API-Ebene 28**

Ein Android-Codename kann mehreren Versionen und API-Ebenen entsprechen (wie in der folgenden Tabelle gezeigt), jede Android-Version entspricht jedoch genau einer API-Ebene.

Außerdem definiert xamarin. Android buildversionscodes, die den derzeit bekannten Android-API-Ebenen entsprechen. In der folgenden Tabelle finden Sie Unterstützung bei der Übersetzung zwischen API-, Android-Version, Codename und xamarin. Android-buildversionscode (buildversionscodes sind im `Android.OS`-Namespace definiert):

[!include[](~/android/includes/api-levels.md)]

Wie diese Tabelle zeigt, werden neue Android-Versionen häufig &ndash; manchmal mehr als eine Version pro Jahr veröffentlicht. Folglich umfasst das Universum von Android-Geräten, die Ihre APP möglicherweise ausführen, eine Vielzahl von älteren und neueren Android-Versionen. Wie können Sie sicherstellen, dass Ihre APP in so vielen verschiedenen Versionen von Android konsistent und zuverlässig ausgeführt wird? Die API-Ebenen von Android können Sie bei der Verwaltung dieses Problems unterstützen.

### <a name="android-api-levels"></a>Android-API-Ebenen

Jedes Android-Gerät wird auf genau *einer* API-Ebene ausgeführt, &ndash; diese API-Ebene für jede Version der Android-Plattform garantiert eindeutig ist. Die API-Ebene identifiziert exakt die Version des API-Satzes, in der Ihre APP aufgerufen werden kann. Es identifiziert die Kombination von manifeselementen, Berechtigungen usw., die Sie als Entwickler codieren. Das Android-System mit API-Ebenen unterstützt Android bei der Ermittlung, ob eine Anwendung mit einem Android-System Abbild kompatibel ist, bevor die Anwendung auf einem Gerät installiert wird.

Wenn eine Anwendung erstellt wird, enthält Sie die folgenden API-Ebeneninformationen:

- Die *Ziel* -API-Ebene von Android, auf der die app ausgeführt werden soll.

- Die *minimale* Android-API-Ebene, die ein Android-Gerät zum Ausführen der APP benötigt. 

Diese Einstellungen werden verwendet, um sicherzustellen, dass die erforderliche Funktionalität zum ordnungsgemäßen Ausführen der APP auf dem Android-Gerät zur Installationszeit verfügbar ist. Andernfalls wird die Ausführung der APP auf diesem Gerät blockiert. Wenn die API-Ebene eines Android-Geräts beispielsweise niedriger ist als die API-Ebene, die Sie für Ihre APP angeben, hindert das Android-Gerät den Benutzer daran, Ihre APP zu installieren.

## <a name="project-api-level-settings"></a>Einstellungen der Projekt-API-Ebene

In den folgenden Abschnitten wird erläutert, wie Sie den SDK-Manager verwenden, um Ihre Entwicklungsumgebung für die API-Ebenen vorzubereiten, auf die Sie abzielen möchten, gefolgt von detaillierten Erläuterungen zum Konfigurieren des *Ziel Frameworks*, der *Android-Mindestversion*und  *Ziel* Einstellungen für Android-Version in xamarin. Android.

### <a name="android-sdk-platforms"></a>Android SDK Plattformen

Bevor Sie ein Ziel oder eine minimale API-Ebene in xamarin. Android auswählen können, müssen Sie die Android SDK Platt Form Version installieren, die dieser API-Ebene entspricht. Der Bereich der verfügbaren Optionen für das Ziel Framework, die Android-Mindestversion und die Android-Zielversion sind auf den Bereich von Android SDK Versionen beschränkt, die Sie installiert haben. Sie können den SDK-Manager verwenden, um zu überprüfen, ob die erforderlichen Android SDK Versionen installiert sind, und Sie können Sie verwenden, um alle neuen API-Ebenen hinzuzufügen, die Sie für Ihre APP benötigen. Wenn Sie mit der Installation von API-Ebenen nicht vertraut sind, finden Sie weitere Informationen unter [Android SDK Setup](~/android/get-started/installation/android-sdk.md).

<a name="framework" />

### <a name="target-framework"></a>Zielframework

Das *Ziel Framework* (auch als `compileSdkVersion`bezeichnet) ist die spezifische Android Framework-Version (API-Ebene), für die Ihre APP zur Buildzeit kompiliert wird. Mit dieser Einstellung wird festgelegt, welche APIs von Ihrer APP bei der Ausführung *erwartet* werden, aber Sie hat keine Auswirkung darauf, welche APIs bei der Installation Ihrer APP tatsächlich zur Verfügung stehen. Folglich ändert sich das Laufzeitverhalten nicht durch Ändern der Ziel Framework-Einstellung.

Das Ziel Framework identifiziert, mit welchen Bibliotheksversionen Ihre Anwendung verknüpft ist &ndash; diese Einstellung bestimmt, welche APIs Sie in Ihrer APP verwenden können. Wenn Sie z. b. die in Android 5,0 Lollipop eingeführte [notificationbuilder. setcategory](xref:Android.App.Notification.Builder.SetCategory*) -Methode verwenden möchten, müssen Sie das Ziel Framework auf API- **Ebene 21 (Lollipop)** oder höher festlegen. Wenn Sie das Ziel Framework des Projekts auf eine API-Ebene wie **API-Ebene 19 (KitKat)** festlegen und versuchen, die `SetCategory`-Methode im Code aufzurufen, erhalten Sie einen Kompilierungsfehler.

Es wird empfohlen, immer mit der *neuesten* verfügbaren Zielframeworkversion zu kompilieren. Auf diese Weise erhalten Sie hilfreiche Warnmeldungen für alle veralteten APIs, die möglicherweise von Ihrem Code aufgerufen werden. Die Verwendung der neuesten Version des Ziel-Frameworks ist besonders wichtig, wenn Sie die neuesten Versionen der Unterstützungs Bibliotheken verwenden &ndash; jede Bibliothek erwartet, dass Ihre APP auf der mindestens vorhandenen API-Ebene der unterstützten Bibliothek kompiliert wird.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Wenn Sie in Visual Studio auf die Einstellung für das Ziel Framework zugreifen möchten, öffnen Sie die Projekteigenschaften in **Projektmappen-Explorer** und wählen Sie die **Anwendungs** Seite aus:

[![Anwendungsseite der Projekteigenschaften](android-api-levels-images/vs-target-framework-sml.png)](android-api-levels-images/vs-target-framework.png#lightbox)

Legen Sie das Ziel Framework fest, indem Sie eine API-Ebene im Dropdown Menü unter **mit Android-Version kompilieren** auswählen, wie oben gezeigt.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Klicken Sie mit der rechten Maustaste auf den Projektnamen, und wählen Sie **Optionen**aus, um auf die Einstellung Ziel Framework in Visual Studio für Mac zuzugreifen. Dadurch wird das Dialogfeld " **Projektoptionen** " geöffnet. Navigieren Sie in diesem Dialogfeld zu **Build > Allgemein** , wie hier gezeigt:

[![Abschnitt "allgemeine Builds" der Seite "Projektoptionen"](android-api-levels-images/xs-target-framework-sml.png)](android-api-levels-images/xs-target-framework.png#lightbox)

Legen Sie das Ziel Framework fest, indem Sie im Dropdown Menü auf der rechten Seite des **Ziel Frameworks** wie oben gezeigt eine API-Ebene auswählen.

-----

<a name="minimum" />

### <a name="minimum-android-version"></a>Android-Mindestversion

Die *Android-Mindestversion* (auch als `minSdkVersion`bezeichnet) ist die älteste Version des Android-Betriebssystems (d. h. die niedrigste API-Ebene), mit der Ihre Anwendung installiert und ausgeführt werden kann. Standardmäßig kann eine app nur auf Geräten installiert werden, die mit der Ziel Framework-Einstellung oder höher übereinstimmen. Wenn die Android-Mindestversion *niedriger* ist als die Einstellung für das Ziel Framework, kann Ihre APP auch unter früheren Versionen von Android ausgeführt werden. Wenn Sie z. b. das Ziel Framework auf **Android 7,1 (Nougat)** festlegen und die Android-Mindestversion auf **Android 4.0.3 (Ice Cream Sandwich)** festlegen, kann Ihre APP auf einer beliebigen Plattform von API-Ebene 15 bis einschließlich API-Ebene 25 installiert werden.

Obwohl Ihre APP auf diesem Platt Formbereich erfolgreich erstellt und installiert werden kann, ist dies nicht gewährleistet, dass Sie auf allen diesen Plattformen erfolgreich *ausgeführt* werden kann. Wenn Ihre APP beispielsweise auf **Android 5,0 (Lollipop)** installiert ist und Ihr Code eine API aufruft, die nur in **Android 7,1 (Nougat)** und neuer verfügbar ist, erhält Ihre APP einen Laufzeitfehler und kann möglicherweise abstürzen. Daher muss der Code &ndash; zur Laufzeit sicherstellen &ndash;, dass nur die APIs aufgerufen werden, die von dem Android-Gerät unterstützt werden, auf dem es ausgeführt wird. Anders ausgedrückt: der Code muss explizite Laufzeitüberprüfungen enthalten, um sicherzustellen, dass Ihre APP neuere APIs nur auf Geräten verwendet, die zur Unterstützung der Anwendungen aktuell genug sind.
[Lauf Zeit Prüfungen für Android-Versionen](#runtimechecks)weiter unten in diesem Handbuch wird erläutert, wie Sie diese Laufzeitüberprüfungen zu Ihrem Code hinzufügen.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Um auf die Android-mindestversionseinstellung in Visual Studio zuzugreifen, öffnen Sie die Projekteigenschaften in **Projektmappen-Explorer** und wählen Sie die Seite **Android-Manifest** aus. Im Dropdown Menü unter Android- **Mindestversion** können Sie die Android-Mindestversion für die Anwendung auswählen:

[![Android to target-mindestoption zum Kompilieren mithilfe der SDK-Version festgelegt](android-api-levels-images/vs-minimum-version-sml.png)](android-api-levels-images/vs-minimum-version.png#lightbox)

Wenn Sie **Kompilierung mit SDK-Version verwenden**auswählen, entspricht die Android-Mindestversion der Ziel Framework-Einstellung.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Um auf die Android-Mindestversion in Visual Studio für Mac zuzugreifen, klicken Sie mit der rechten Maustaste auf den Projektnamen, und wählen Sie **Optionen**. Dadurch wird das Dialogfeld " **Projektoptionen** " geöffnet. Navigieren Sie zu **Build > Android-Anwendung**.
Wenn Sie das Dropdown Menü rechts von mindestens der **Android-Version**verwenden, können Sie die Android-Mindestversion für die Anwendung festlegen:

[![Android-Mindestversion auf automatische Verwendung der Ziel Framework-Version festgelegt.](android-api-levels-images/xs-minimum-version-sml.png)](android-api-levels-images/xs-minimum-version.png#lightbox)

Wenn Sie die Option **automatisch &ndash; die Ziel Framework-Version verwenden**auswählen, entspricht die Android-Mindestversion der Ziel Framework-Einstellung.

-----

<a name="target" />

### <a name="target-android-version"></a>Android-Ziel Version

Die *Android-Ziel Version* (auch als `targetSdkVersion`bezeichnet) ist die API-Ebene des Android-Geräts, auf dem die app ausgeführt werden soll. Android verwendet diese Einstellung, um zu bestimmen, ob Kompatibilitäts Verhaltensweisen aktiviert werden &ndash; dadurch wird sichergestellt, dass Ihre APP weiterhin erwartungsgemäß funktioniert. Android verwendet die Ziel-Android-Versions Einstellung Ihrer APP, um herauszufinden, welche Verhaltensänderungen auf Ihre APP angewendet werden können, ohne Sie zu unterbrechen (auf diese Weise wird die Kompatibilität von Android bereitgestellt).

Das Ziel Framework und die Android-Zielversion, die sehr ähnliche Namen aufweisen, sind nicht identisch. Die Ziel Framework-Einstellung kommuniziert API-Ziel Informationen zur Verwendung zur *Kompilierzeit*an xamarin. Android, während die Android-Zielversion Informationen zur Ziel-API-Ebene an Android zur *Laufzeit* kommuniziert (wenn die APP auf einem Gerät installiert und ausgeführt wird.)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Um auf diese Einstellung in Visual Studio zuzugreifen, öffnen Sie die Projekteigenschaften in **Projektmappen-Explorer** und wählen Sie die Seite **Android-Manifest** aus. Im Dropdown Menü unter Android- **Zielversion** können Sie die Android-Zielversion für die Anwendung auswählen:

[![Android-Zielversion für die Kompilierung mit SDK-Version festgelegt](android-api-levels-images/vs-target-version-sml.png)](android-api-levels-images/vs-target-version.png#lightbox)

Es wird empfohlen, dass Sie die Android-Zielversion explizit auf die neueste Version von Android festlegen, die Sie zum Testen Ihrer APP verwenden. Im Idealfall sollte es auf die neueste Android SDK Version festgelegt werden &ndash; so können Sie vor dem Durcharbeiten der Verhaltensänderungen neue APIs verwenden. Für die meisten Entwickler empfiehlt es sich *nicht* , die Android-Zielversion für die **Verwendung von Compile mithilfe der SDK-Version**festzulegen.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Um auf diese Einstellung in Visual Studio für Mac zuzugreifen, klicken Sie mit der rechten Maustaste auf den Projektnamen, und wählen Sie **Optionen**. Dadurch wird das Dialogfeld " **Projektoptionen** " geöffnet. Navigieren Sie zu **Build > Android-Anwendung**. Mithilfe des Dropdown Menüs rechts neben der Android- **Zielversion**können Sie die Android-Zielversion für die Anwendung festlegen:

[![Android-Zielversion auf automatische Verwendung der Ziel Framework-Version festgelegt](android-api-levels-images/xs-target-version-sml.png)](android-api-levels-images/xs-target-version.png#lightbox)

Es wird empfohlen, dass Sie die Android-Zielversion explizit auf die neueste Version von Android festlegen, die Sie zum Testen Ihrer APP verwenden. Im Idealfall sollte Sie auf die neueste verfügbare Android SDK Version festgelegt werden &ndash; damit Sie neue APIs verwenden können, bevor Sie die Verhaltensänderungen durcharbeiten. Für die meisten Entwickler empfiehlt es sich nicht, die Ziel-Android-Version auf die **automatische Verwendung der Ziel Framework-Version**festzulegen.

-----

Im Allgemeinen sollte die Android-Zielversion durch die Android-Mindestversion und das Ziel Framework begrenzt werden. Dies bedeutet:

**Android-Mindestversion < = Ziel-Android-Version < = Ziel Framework**

Weitere Informationen zu SDK-Ebenen finden Sie in der Dokumentation zum Android Developer [verwendet-SDK](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html) .

<a name="runtimechecks" />

## <a name="runtime-checks-for-android-versions"></a>Lauf Zeit Prüfungen für Android-Versionen

Wenn jede neue Version von Android veröffentlicht wird, wird die Framework-API aktualisiert, um neue oder Ersatz Funktionen bereitzustellen. Mit wenigen Ausnahmen wird die API-Funktionalität früherer Android-Versionen ohne Änderungen in neuere Android-Versionen weitergeführt. Wenn Ihre APP auf einer bestimmten Android-API-Ebene ausgeführt wird, kann Sie daher in der Regel ohne Änderungen auf einer späteren Android-API-Ebene ausgeführt werden. Aber was geschieht, wenn Sie Ihre APP auch in früheren Versionen von Android ausführen möchten?

Wenn Sie eine minimale Android-Version auswählen, die *niedriger* ist als die Einstellung für das Ziel Framework, sind einige APIs möglicherweise zur Laufzeit nicht für Ihre app verfügbar. Ihre APP kann jedoch weiterhin auf einem früheren Gerät ausgeführt werden, aber mit eingeschränkter Funktionalität. Für jede API, die auf Android-Plattformen nicht verfügbar ist, die ihrer Mindesteinstellung für die Android-Version entspricht, muss der Code den Wert der `Android.OS.Build.VERSION.SdkInt`-Eigenschaft explizit überprüfen, um die API-Ebene der Plattform zu ermitteln, auf der die app ausgeführt wird. Wenn die API-Ebene *niedriger* ist als die Android-Mindestversion, die die API unterstützt, die aufgerufen werden soll, muss Ihr Code eine Möglichkeit finden, ordnungsgemäß zu funktionieren, ohne diesen API-Befehl durchführen zu müssen.

Angenommen, Sie möchten die [notificationbuilder. setcategory](xref:Android.App.Notification.Builder.SetCategory*) -Methode verwenden, um eine Benachrichtigung zu kategorisieren, wenn Sie unter **Android 5,0 Lollipop** (und höher) ausgeführt wird, aber wir möchten weiterhin, dass unsere app unter früheren Versionen von Android ausgeführt wird, wie z **. b. Android 4,1 Jelly Bean** (wo `SetCategory` nicht verfügbar ist). In Bezug auf die Android-Versions Tabelle am Anfang dieses Leitfadens sehen Sie, dass der buildversionscode für **Android 5,0 Lollipop** `Android.OS.BuildVersionCodes.Lollipop`ist. Um ältere Versionen von Android zu unterstützen, bei denen `SetCategory` nicht verfügbar ist, kann der Code die API-Ebene zur Laufzeit erkennen und bedingt `SetCategory` nur dann abrufen, wenn die API-Ebene größer oder gleich dem Lollipop Build-Versions Code ist:

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop)
{
    builder.SetCategory(Notification.CategoryEmail);
}
```

In diesem Beispiel ist das Ziel Framework unserer App auf **Android 5,0 (API-Ebene 21)** und die Android-Mindestversion auf **Android 4,1 (API-Ebene 16)** festgelegt. Da `SetCategory` auf API-Ebene `Android.OS.BuildVersionCodes.Lollipop` und höher verfügbar ist, ruft dieser Beispielcode `SetCategory` nur dann auf, wenn er tatsächlich verfügbar ist &ndash; er versucht *nicht* , `SetCategory` aufzurufen, wenn die API-Ebene 16 ist. , 17, 18, 19 oder 20. Die Funktionalität wird in diesen früheren Android-Versionen nur dahingehend reduziert, dass Benachrichtigungen nicht ordnungsgemäß sortiert werden (da Sie nicht nach Typ kategorisiert werden), aber die Benachrichtigungen werden weiterhin veröffentlicht, um den Benutzer zu benachrichtigen. Unsere APP funktioniert weiterhin, die Funktionalität wird jedoch geringfügig beeinträchtigt.

Im Allgemeinen unterstützt die Überprüfung der Buildversion dem Code die Entscheidung zur Laufzeit zwischen der neuen Methode und der alten Methode. Beispiel:

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

Es gibt keine schnelle und einfache Regel, die erläutert, wie Sie die Funktionalität Ihrer APP reduzieren oder ändern können, wenn Sie auf älteren Android-Versionen ausgeführt wird, bei denen mindestens eine API fehlt. In einigen Fällen (z. b. im obigen `SetCategory` Beispiel) genügt es, den API-Befehl auszulassen, wenn er nicht verfügbar ist. In anderen Fällen müssen Sie jedoch möglicherweise Alternative Funktionen implementieren, wenn `Android.OS.Build.VERSION.SdkInt` erkannt wird, dass Sie niedriger ist als die API-Ebene, die Ihre APP benötigt, um die optimale Umgebung zu präsentieren.

<a name="libraries" />

## <a name="api-levels-and-libraries"></a>API-Ebenen und-Bibliotheken

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Wenn Sie ein xamarin. Android-Bibliotheksprojekt erstellen (z. b. eine Klassenbibliothek oder eine Bindungs Bibliothek), können Sie nur die Ziel Framework-Einstellung konfigurieren, &ndash; die Android-Mindestversion und die Einstellungen der Android-Zielversion nicht verfügbar sind. Dies liegt daran, dass keine **Android-Manifest** -Seite vorhanden ist:

[![nur die Option Kompilierung mit Android-Version ist verfügbar.](android-api-levels-images/vs-library-options-sml.png)](android-api-levels-images/vs-library-options.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Wenn Sie ein xamarin. Android-Bibliotheksprojekt erstellen, gibt es keine **Android-Anwendungs** Seite, auf der Sie die Android-Mindestversion und die Android-Zielversion konfigurieren können &ndash; die Android-Mindestversion und die Android-Zielversion-Einstellungen nicht frei.
Dies liegt daran, dass keine **Build > Android-Anwendungs** Seite vorhanden ist:

[![Seite "Build (allgemein)" ohne Minimum-und Ziel Versions Optionen](android-api-levels-images/xs-library-options-sml.png)](android-api-levels-images/xs-library-options.png#lightbox)

-----

Die Android-Mindestversion und die Android-Zielversion sind nicht verfügbar, da es sich bei der resultierenden Bibliothek nicht um eine eigenständige App handelt, &ndash; die Bibliothek abhängig von der APP, mit der Sie verpackt ist, unter jeder beliebigen Android-Version ausgeführt werden kann. Sie können angeben, wie die Bibliothek *kompiliert*werden soll, aber Sie können nicht vorhersagen, auf welcher Plattform-API-Ebene die Bibliothek ausgeführt werden soll. Beachten Sie, dass beim Verarbeiten oder Erstellen von Bibliotheken die folgenden bewährten Methoden beachtet werden sollten:

- Wenn Sie **eine Android-Bibliothek** nutzen &ndash; Wenn Sie in Ihrer Anwendung eine Android-Bibliothek verwenden, stellen Sie sicher, dass Sie die Ziel Framework-Einstellung Ihrer APP auf eine API-Ebene festlegen, die *mindestens so hoch ist wie* die Ziel Framework-Einstellung der Bibliothek.

- Wenn Sie beim Erstellen einer Android- **Bibliothek** &ndash; eine Android-Bibliothek für die Verwendung durch andere Anwendungen erstellen, achten Sie darauf, dass die Ziel Framework-Einstellung auf die minimale API-Ebene festgelegt wird, die für die Kompilierung benötigt wird.

Diese bewährten Methoden werden empfohlen, um die Situation zu vermeiden, in der eine Bibliothek versucht, eine API aufzurufen, die zur Laufzeit nicht verfügbar ist (was dazu führen kann, dass die APP abstürzen kann). Wenn Sie ein Bibliotheks Entwickler sind, sollten Sie die Verwendung von API-aufrufen auf eine kleine und gut festgelegte Teilmenge der gesamten API-Oberfläche einschränken. Auf diese Weise können Sie sicherstellen, dass Ihre Bibliothek sicher in einer breiteren Palette von Android-Versionen verwendet werden kann.

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurde erläutert, wie Android-API-Ebenen verwendet werden, um die APP-Kompatibilität in verschiedenen Versionen von Android zu verwalten Es wurden ausführliche Schritte zum Konfigurieren des xamarin. Android- *Ziel Frameworks*, der *Android-Mindestversion*und der Projekteinstellungen für *Android-Ziel Versionen* bereitgestellt. Es wurden Anweisungen zum Verwenden des Android SDK-Managers zum Installieren von SDK-Paketen bereitgestellt. Beispiele für das Schreiben von Code für die Bearbeitung verschiedener API-Ebenen zur Laufzeit sowie das Verwalten von API-Ebenen beim Erstellen oder Verarbeiten von Android-Bibliotheken. Außerdem wurde eine umfassende Liste bereitgestellt, die API-Ebenen mit Android-Versionsnummern (z. b. Android 4,4), Android-Versionsnamen (z. b. KitKat) und xamarin. Android-buildversionscodes verknüpft.

## <a name="related-links"></a>Verwandte Links

- [Android SDK-Setup](~/android/get-started/installation/android-sdk.md)
- [SDK-CLI-Tool Änderungen](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Auswählen von "compilesdkversion", "minsdkversion" und "Targe.dkversion"](https://medium.com/google-developers/picking-your-compilesdkversion-minsdkversion-targetsdkversion-a098a0341ebd)
- [Was ist API-Ebene?](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)
- [Codenamen, Tags und Buildnummern](https://source.android.com/source/build-numbers)
