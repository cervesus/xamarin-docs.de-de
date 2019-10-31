---
title: Manuelles Hochladen des APKs
ms.prod: xamarin
ms.assetid: 1309C251-ABF0-4412-B1F5-200DC8321A9D
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
ms.openlocfilehash: b5b7a416cf67c217862987e7fa29bfb6a9692642
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021251"
---
# <a name="manually-uploading-the-apk"></a>Manuelles Hochladen des APKs

Beim erstmaligen Einreichen eines APKs bei Google Play (oder bei Verwendung einer frühen Xamarin.Android-Version) muss das APK manuell mithilfe der [Google Play Developer Console](https://play.google.com/apps/publish) hochgeladen werden. In dieser Anleitung werden die hierfür notwendigen Schritte beschrieben. 

## <a name="google-play-developer-console"></a>Google Play Developer Console

Sobald das APK kompiliert ist und die Werbeelemente vorbereitet sind, müssen Sie die Anwendung auf Google Play hochladen. Melden Sie sich hierzu bei der unten dargestellten [Google Play Developer Console](https://play.google.com/apps/publish) an. Klicken Sie auf die Schaltfläche **Android-App auf Google Play veröffentlichen**, um den Verteilungsprozess für die Anwendung zu initialisieren.

[![Google Play Developer Console](manually-uploading-the-apk-images/00-google-play-developer-console-sml.png)](manually-uploading-the-apk-images/00-google-play-developer-console.png#lightbox)

Wenn Sie bereits eine vorhandene App bei Google Play registriert haben, klicken Sie auf die Schaltfläche **Add new application** (Neue Anwendung hinzufügen):

[![Schaltfläche „Neue Anwendung hinzufügen“](manually-uploading-the-apk-images/01-existing-app-sml.png)](manually-uploading-the-apk-images/01-existing-app.png#lightbox)

Wenn das Dialogfeld **ADD NEW APPLICATION** (Neue Anwendung hinzufügen) angezeigt wird, geben Sie den Namen der App ein, und klicken Sie auf **APK hochladen**:

[![Schaltfläche „APK hochladen“](manually-uploading-the-apk-images/02-add-new-application-sml.png)](manually-uploading-the-apk-images/02-add-new-application.png#lightbox)

Auf der nächsten Seite kann die App für Alpha- oder Betatests oder aber für den Produktionseinsatz veröffentlicht werden. Im folgenden Beispiel wird die Registerkarte **Alpha-Test** ausgewählt. Da **MyApp** keine Lizenzierungsdienste verwendet, müssen Sie in diesem Beispiel nicht auf die Schaltfläche **Get license key** (Lizenzierungsschlüssel anfordern) klicken. Klicken Sie auf die Schaltfläche **Erste APK-Datei in Alphaphase hochladen**, um die Datei in den Alphakanal hochzuladen:

[![Schaltfläche „Erste APK-Datei in Alphaphase hochladen“](manually-uploading-the-apk-images/03-upload-to-alpha-sml.png)](manually-uploading-the-apk-images/03-upload-to-alpha.png#lightbox)

Das Dialogfeld **Erste APK-Datei in Alphaphase hochladen** wird angezeigt. Sie können das APK entweder durch Klicken auf die Schaltfläche **Hinzufügen** oder per Drag & Drop hochladen: 

[![Dialogfeld „Erste APK-Datei in Alphaphase hochladen“](manually-uploading-the-apk-images/04-upload-dialog-sml.png)](manually-uploading-the-apk-images/04-upload-dialog.png#lightbox)

Achten Sie darauf, das releasefähige APK hochzuladen, das verteilt werden soll.
Im nächsten Dialogfeld wird der Uploadstatus des APKs angezeigt:

[![Fortschrittsanzeige für Upload](manually-uploading-the-apk-images/05-upload-progress-sml.png)](manually-uploading-the-apk-images/05-upload-progress.png#lightbox)

Nach dem Hochladen des APKs können Sie eine Testmethode auswählen:

[![Dialogfeld „Testmethode auswählen“](manually-uploading-the-apk-images/06-select-testing-method-sml.png)](manually-uploading-the-apk-images/06-select-testing-method.png#lightbox)

Weitere Informationen zum Testen von Apps finden Sie in der Google-Anleitung [Alpha-/Betatests einrichten](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en).

Nachdem das APK hochgeladen wurde, wird es als Entwurf gespeichert. Es kann erst veröffentlicht werden, wenn weitere Angaben auf Google Play bereitgestellt werden. Diese werden im Folgenden beschrieben.

## <a name="store-listing"></a>Store-Eintrag

Klicken Sie in der **Google Play Developer Console** auf **Store-Eintrag**. Hierdurch können Sie Informationen eingeben, die auf Google Play potenziellen Benutzern der Anwendung angezeigt werden: 

[![Dialogfeld „Store-Eintrag“](manually-uploading-the-apk-images/07-store-listing-sml.png)](manually-uploading-the-apk-images/07-store-listing.png#lightbox)

### <a name="graphics-assets"></a>Grafische Elemente

Scrollen Sie auf der Seite **Store-Eintrag** nach unten zum Abschnitt **Grafische Elemente**:

[![Abschnitt „Grafische Elemente“](manually-uploading-the-apk-images/08-graphic-assets-sml.png)](manually-uploading-the-apk-images/08-graphic-assets.png#lightbox)

In diesem Abschnitt können Sie alle vorbereiteten Werbeelemente hochladen. Während dieses Vorgangs erhalten Sie Hinweise zu erforderlichen Werbeelementen und dem benötigten Format dieser Elemente.

### <a name="categorization"></a>Kategorisierung

Nach dem Abschnitt **Grafische Elemente** wird der Abschnitt **Kategorisierung** angezeigt. Wählen Sie hier den Anwendungstyp und die Kategorie aus:

[![Abschnitt „Kategorisierung“](manually-uploading-the-apk-images/09-categorization-sml.png)](manually-uploading-the-apk-images/09-categorization.png#lightbox)

Die Einstufung des Inhalts wird im übernächsten Abschnitt beschrieben.

### <a name="contact-details"></a>Kontaktdaten

Im letzten Abschnitt der Seite geht es um **Kontaktdaten**. Hier werden die Kontaktdaten des Anwendungsentwicklers erfasst:

[![Abschnitt „Kontaktdaten“](manually-uploading-the-apk-images/10-contact-details-sml.png)](manually-uploading-the-apk-images/10-contact-details.png#lightbox)

Sie können wie im Screenshot oben gezeigt im Abschnitt **Datenschutzerklärung** eine URL angeben, die auf die Datenschutzrichtlinie der App verweist.

## <a name="content-rating"></a>Einstufung des Inhalts

Klicken Sie in der **Google Play Developer Console** auf **Einstufung des Inhalts**. Auf dieser Seite nehmen Sie für Ihre App die Einstufung des Inhalts vor. Eine solche Einstufung ist für alle Anwendungen erforderlich, die auf Google Play hochgeladen werden. Klicken Sie auf die Schaltfläche **Fortfahren**, um die Bearbeitung des Fragebogens zur Einstufung des Inhalts abzuschließen:

[![Abschnitt „Einstufung des Inhalts“](manually-uploading-the-apk-images/11-content-rating-sml.png)](manually-uploading-the-apk-images/11-content-rating.png#lightbox)

Für sämtliche Google Play-Anwendungen ist eine Einstufung auf der Grundlage des Einstufungssystems von Google Play erforderlich. Zusätzliche müssen alle Anwendungen in Einklang mit den [Richtlinien für Entwicklerinhalte](https://www.android.com/us/developer-content-policy.html) stehen.

Im Folgenden werden die vier Stufen des Einstufungssystems von Google Play aufgeführt. Außerdem werden Hinweise zu Funktionen und Inhalten gegeben, die eine bestimmte Einstufung erforderlich machen oder erzwingen: 

- **Alle**: Das Zugreifen auf, Veröffentlichen von oder Freigeben von Standortdaten ist nicht möglich. Vom Benutzer generierte Inhalte können nicht gehostet werden. Die Kommunikation zwischen Benutzern ist nicht möglich. 

- **Stufe 3 - Niedrig**: Anwendungen, die auf Standortdaten zugreifen, diese aber nicht freigeben. Schließt Darstellung von leichter Gewalt oder Gewalt in Cartoons ein. 

- **Stufe 2 - Mittel**: Anspielungen auf Drogen, Alkohol oder Tabak. Des Weiteren werden folgende Themen eingeschlossen: Glücksspiel oder simuliertes Glücksspiel; hetzerische Inhalte; obszöne Äußerungen und anstößiger Humor; anzügliche Bemerkungen oder sexuelle Anspielungen; 
    intensive Gewalt in Verbindung mit dem Fantasy-Genre; realistische Darstellung von Gewalt; Ortung von Benutzern durch andere Benutzer; Kommunikation zwischen Benutzern; 
    Freigeben von Standortdaten eines Benutzers. 

- **Stufe 1 - Hoch**: Schwerpunktmäßige Darstellung des Konsums oder Verkaufs von Alkohol, Tabak oder Drogen. Des Weiteren werden folgende Themen eingeschlossen: große Anzahl von anzüglichen Bemerkungen oder sexuellen Anspielungen; explizite Darstellung intensiver Gewalt. 

Die Elemente in der Liste „Stufe 2 - Mittel“ sind subjektiv. Es ist daher möglich, dass eine Richtlinie, die auf den ersten Blick eine Einstufung von Inhalten in Stufe 2 verlangt, letztlich doch „Stufe 1 - Hoch“ als Einstufung fordert. 

## <a name="pricing-amp-distribution"></a>Preisgestaltung &amp; Vertrieb

Klicken Sie in der **Google Play Developer Console** auf **Preisgestaltung und Vertrieb**. Legen Sie auf dieser Seite einen Preis fest, wenn die App kostenpflichtig ist.
Alternativ kann die Anwendung kostenlos an alle Benutzer verteilt werden. Eine einmal als kostenlos deklarierte Anwendung muss auch zukünftig kostenlos sein.
Es ist auf Google Play nicht möglich, eine kostenlose Anwendung in eine kostenpflichtige umzuwandeln. In einer kostenlosen App können jedoch Inhalte über die In-App-Abrechnung verkauft werden. Eine Umwandlung einer kostenpflichtigen App in eine kostenlose ist hingegen bei Google Play jederzeit möglich.

Vor der Veröffentlichung einer kostenpflichtigen App müssen Sie ein Händlerkonto erstellen. Klicken Sie hierzu auf **Händlerkonto einrichten**, und folgen Sie den dort aufgeführten Schritten.

[![ Dialogfeld „Preisgestaltung und Vertrieb“](manually-uploading-the-apk-images/12-pricing-sml.png)](manually-uploading-the-apk-images/12-pricing.png#lightbox)

### <a name="manage-countries"></a>Länder verwalten

Im nächsten Abschnitt, **Länder verwalten**, können Sie einstellen, in welchen Ländern eine App verteilt werden kann:

[![Dialogfeld „Länder verwalten“](manually-uploading-the-apk-images/13-manage-countries-sml.png)](manually-uploading-the-apk-images/13-manage-countries.png#lightbox)

### <a name="other-information"></a>Sonstige Informationen

Scrollen Sie weiter nach unten, um anzugeben, ob die App Werbung enthält. Im Abschnitt **Gerätekategorien** können Sie Optionen auswählen, mit denen die App optional für Android Wear, Android TV oder Android Auto verteilt wird:

[![Enthält Abschnitt „Ads“ (Werbung)](manually-uploading-the-apk-images/14-contains-ads-sml.png)](manually-uploading-the-apk-images/14-contains-ads.png#lightbox)

Nach diesem Abschnitt werden Ihnen zusätzliche Optionen angezeigt. Hierbei können Sie z.B. die Option **Designed for Families** auswählen oder die App über Google Play for Education verteilen.

### <a name="consent"></a>Einverständniserklärung

Im unteren Bereich der Seite **Preisgestaltung &amp; Verteilung** befindet sich der Abschnitt **Einverständniserklärung**.
Die Angaben in diesem Abschnitt sind obligatorisch. Mit diesen bestätigen Sie, dass die App in Einklang mit den [Richtlinien für Android-Content](https://www.android.com/market/terms/developer-content-policy.html#hl=us) steht. Des Weiteren erklären Sie, dass die Anwendung US-amerikanischen Exportgesetzen unterliegt:

[![Abschnitt „Einverständniserklärung“](manually-uploading-the-apk-images/15-consent-sml.png)](manually-uploading-the-apk-images/15-consent.png#lightbox)

Bei der Veröffentlichung einer Xamarin.Android-App gibt es noch viele weitere Aspekte, die in dieser Anleitung nicht behandelt werden können.
Weitere Informationen zum Veröffentlichen einer App auf Google Play finden Sie unter [Willkommen in der Google Play Console-Hilfe](https://support.google.com/googleplay/android-developer#topic=3450769).

## <a name="google-play-filters"></a>Google Play-Filter

Wenn Benutzer die Google Play-Website nach Anwendungen durchsuchen, können sie nach allen veröffentlichten Anwendungen suchen. Verwenden Benutzer für die Suche allerdings Google Play auf einem Android-Gerät, variieren die Ergebnisse geringfügig. Dies liegt daran, dass die Ergebnisse nach Gerätekompatibilität gefiltert werden. Wenn eine Anwendung beispielsweise SMS senden muss, wird Benutzern von Google Play, die nicht über ein Gerät mit der entsprechenden Funktion verfügen, diese Anwendung nicht angezeigt. Die auf die Suchergebnisse angewendeten Filter werden auf der Grundlage folgender Aspekte erstellt:

1. die Hardwarekonfiguration des Geräts
2. Deklarationen in der Manifestdatei der Anwendung
3. der Netzbetreiber (sofern vorhanden)
4. der Ort des Geräts

Sie können Elemente zum Anwendungsmanifest hinzufügen, um einzustellen, nach welchen Kriterien eine App im Google Play Store gefiltert wird. Die folgenden Elemente und Attribute können zum Filtern von Anwendungen verwendet werden:

- [supports-screen](https://developer.android.com/guide/topics/manifest/supports-screens-element.html): Die angegebenen Attribute werden von Google Play verwendet, um zu bestimmen, ob eine Anwendung auf einem Gerät mit einer bestimmten Bildschirmgröße bereitgestellt werden kann. 
    Hierbei geht Google Play davon aus, dass Android zwar ein Layout an einen größeren Bildschirm anpassen kann, der umgekehrte Vorgang jedoch nicht möglich ist. Eine Anwendung, die normale Bildschirmgrößen unterstützt, wird daher bei Suchanfragen auf Geräten mit großen Bildschirmen, nicht aber auf Geräten mit kleinen Bildschirmen angezeigt. Wenn eine Xamarin.Android-Anwendung kein `<supports-screen>`-Element in der Manifestdatei bereitstellt, wird von Google Play angenommen, dass alle Attribute auf den Wert „true“ festgelegt sind und die Anwendung alle Bildschirmgrößen unterstützt. Dieses Element muss manuell zur Datei **AndroidManifest.xml** hinzugefügt werden. 

- [uses-configuration](https://developer.android.com/guide/topics/manifest/uses-configuration-element.html) &ndash; Dieses Manifestelement wird verwendet, um bestimmte Hardwareeigenschaften wie die verwendete Tastatur, Navigationsgeräte oder den Touchscreen anzufordern. Dieses Element muss manuell zur Datei **AndroidManifest.xml** hinzugefügt werden. 

- [uses-feature](https://developer.android.com/guide/topics/manifest/uses-feature-element.html): In diesem Manifestelement werden Hardwareeigenschaften oder Softwarefunktionen deklariert, über die ein Gerät zur ordnungsgemäßen Ausführung der Anwendung verfügen muss. Dieses Attribut dient nur zu Informationszwecken. Google Play zeigt zwar die Anwendung nicht auf Geräten an, die diesem Filterkriterium nicht entsprechen. Die Anwendung kann aber dennoch auf andere Weise installiert werden (z.B. manuell oder durch Herunterladen). Dieses Element muss manuell zur Datei **AndroidManifest.xml** hinzugefügt werden. 

- [uses-library](https://developer.android.com/guide/topics/manifest/uses-library-element.html): In diesem Element werden bestimmte freigegebene Bibliotheken (beispielsweise die von Google Maps) angegeben, die auf einem Gerät vorhanden sein müssen. Dieses Element kann auch mit `Android.App.UsesLibraryAttribute` angegeben werden. Beispiel: 

    ```csharp
    [assembly: UsesLibrary("com.google.android.maps", true)]
    ```

- [uses-permission](https://developer.android.com/guide/topics/manifest/uses-permission-element.html): Mit diesem Element lassen sich bestimmte Hardwareeigenschaften ermitteln, zur Ausführung der Anwendung erforderlich sind, jedoch nicht korrekt im `<uses-feature>`-Element deklariert wurden. Wenn z.B. eine Anwendung die Berechtigung zur Verwendung der Kamera anfordert, wird von Google Play angenommen, dass das Gerät über eine Kamera verfügt. Dies ist auch dann der Fall, wenn kein `<uses-feature>`-Element vorhanden ist, in dem die Kamera deklariert wird. Dieses Element kann mit `Android.App.UsesPermissionsAttribute` festgelegt werden. Beispiel: 

    ```csharp
    [assembly: UsesPermission(Manifest.Permission.Camera)]
    ```

- [uses-sdk](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html)&ndash; Mit diesem Element wird die Mindestversion der Android-API-Ebene deklariert, die für die Anwendung erforderlich ist. Das Element kann in den Xamarin.Android-Optionen eines Xamarin.Android-Projekts festgelegt werden. 

- [compatible-screens](https://developer.android.com/guide/topics/manifest/compatible-screens-element.html): Dieses Element wird zum Filtern von Anwendungen verwendet, die nicht der Bildschirmgröße und -dichte entsprechen, die im Element angegeben wurden. Für die meisten Anwendungen sollte auf die Nutzung dieses Filters verzichtet werden, da dieser für Spiele mit hohen Leistungsanforderungen oder aber Anwendungen vorgesehen ist, bei denen strenge Verteilungskontrollen erforderlich sind. Stattdessen sollte das oben erwähnte `<support-screen>`-Attribut verwendet werden. 

- [supports-gl-texture](https://developer.android.com/guide/topics/manifest/supports-gl-texture-element.html): In diesem Element werden GL-Texturkomprimierungsformate deklariert, die für die Anwendung erforderlich sind. Für die meisten Anwendungen sollte auf die Nutzung dieses Filters verzichtet werden, da dieser für Spiele mit hohen Leistungsanforderungen oder aber Anwendungen vorgesehen ist, bei denen strenge Verteilungskontrollen erforderlich sind. 

Weitere Informationen zum Konfigurieren des App-Manifests finden Sie im Android-Thema [App Manifest (App-Manifest)](https://developer.android.com/guide/topics/manifest/manifest-intro.html).
