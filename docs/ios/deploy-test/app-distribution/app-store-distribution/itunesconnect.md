---
title: Konfigurieren einer App in iTunes Connect
description: Dieser Artikel beschreibt die Einrichtung und Verwaltung einer Xamarin.iOS-Anwendung in iTunes Connect für die Veröffentlichung im App Store.
ms.prod: xamarin
ms.assetid: 74587317-4b15-4904-9582-dcd914827cbc
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 97b3e5323329d2df024e05f1829b12b239b37299
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103048"
---
# <a name="configuring-an-app-in-itunes-connect"></a>Konfigurieren einer App in iTunes Connect

> [!IMPORTANT]
> Apple [hat mitgeteilt](https://developer.apple.com/news/?id=05072018a), dass ab Juli 2018 alle Apps und Updates, die an den App Store gesendet werden, mit dem iOS 11 SDK erstellt worden sein und das [iPhone X-Display unterstützen](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md) müssen.

iTunes Connect ist eine Suite von webbasierten Tools, mit denen Sie u.a. Ihre iOS-Anwendungen im App Store verwalten können. Eine Xamarin.iOS-Anwendung muss ordnungsgemäß in iTunes Connect eingerichtet und konfiguriert werden, bevor sie zur Überprüfung an Apple gesendet und schließlich für den Verkauf oder als kostenlose App im App Store veröffentlicht werden kann.

iTunes Connect kann für Folgendes verwendet werden:

- Festlegen des Anwendungsnamens (wie im App Store angezeigt)
- Bereitstellen von Screenshots oder Videos aus der laufenden Anwendung von iOS-Geräten, die sie unterstützt
- Liefern Sie eine kurze und klare Beschreibung der Anwendung, einschließlich aller Funktionen und Vorteile für den Benutzer.
- Stellen Sie Kategorien, Unterkategorien und Schlüsselwörter bereit, damit die Benutzer Ihre App im App Store gut finden können.
- Geben Sie Schlüsselwörter an, die die Suche nach der App erleichtern.
- Geben Sie Kontakt- und Support-URLs zu Ihrer Website an (Anforderung von Apple).
- Legen Sie die Altersfreigabe Ihrer Anwendung für die Jugendschutzeinstellungen im App Store fest.
- Wählen Sie den Verkaufspreis aus, oder geben Sie an, dass die Anwendung kostenlos ist.
- Konfigurieren Sie optionale App Store-Technologien, wie z.B. Game Center und In-App-Käufe.

Darüber hinaus sollten Sie für die App auch ansprechendes, hochauflösendes Bildmaterial bereithalten, falls Apple die App in den App Store aufnehmen möchte. Weitere Informationen finden Sie im [iTunes Connect-Entwicklerhandbuch](https://developer.apple.com/support/itunes-connect/) von Apple.

## <a name="managing-agreements-tax-and-banking"></a>Verwalten des Bereichs „Agreements, Tax and Banking“ (Vereinbarungen, Steuern und Bankverbindungen)

Im Bereich **Agreements, Tax, and Banking** (Vereinbarungen, Steuern und Bankverbindungen) in iTunes Connect werden die erforderlichen Finanzinformationen zu iTunes-Entwicklerzahlungen und Quellensteuern sowie der Status Ihrer Verträge mit Apple bereitgestellt. Bevor Sie eine iOS-Anwendung im App Store veröffentlichen können (kostenlos oder für den Verkauf), müssen Sie die entsprechenden Verträge eingegangen sein und jeglichen Änderungen an bestehenden Verträgen zugestimmt haben.

[![](itunesconnect-images/agreement01.png "Verwalten von Vereinbarungen, Steuern und Bankverbindungen")](itunesconnect-images/agreement01.png#lightbox)

Anschließend können Sie Folgendes tun:

- Einen **Team Agent** (Team-Agent) stellen und andere Benutzerrollen für Ihr iTunes-Konto wie **Admin** (Administrator) oder **Finance** (Finanzen) definieren
- Sich für **Verträge** registrieren, mit denen eine Organisation Anwendungen über den App Store verteilen kann, und diese verwalten
- Den Namen der **Legal Entity**  angeben („Juristische Person“, d.h. der Name des Verkäufers im App Store), auf den die Bankverbindungen laufen und auf den die Verträge und Steuerinformationen Ihrer Organisation ausgestellt werden
- **Bankverbindungen** und **Steuerinformationen** angeben, wenn Sie Anwendungen über den App Store verkaufen möchten

Diese Informationen _müssen unbedingt_ korrekt und ordnungsgemäß eingegeben werden, bevor eine iOS-Anwendung zur Überprüfung und Veröffentlichung an iTunes Connect übermittelt werden kann. Weitere Informationen finden Sie in der Apple-Dokumentation unter [Vereinbarungen, Steuern und Bankverbindungen verwalten](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Chapters/ManagingContractsandBanking.html#//apple_ref/doc/uid/TP40011225-CH21-SW1).

<a name="creating" />

## <a name="creating-an-itunes-connect-record"></a>Erstellen eines iTunes Connect-Datensatzes

Bevor Sie eine Xamarin.iOS-Anwendung in iTunes Connect für die Verteilung über den App Store hochladen können, müssen Sie für die Anwendung einen Datensatz in iTunes Connect erstellen. Dieser Datensatz enthält alle Informationen zur Anwendung, so wie sie im App Store angezeigt werden wird (in so vielen Sprachen wie nötig), und auch alle Informationen, die während der Verteilung der App für ihre Verwaltung erforderlich sind. Darüber hinaus werden damit App Store-Technologien wie das iAd App Network oder das Game Center konfiguriert.

Um eine iOS-Anwendung zu iTunes Connect hinzuzufügen, müssen Sie **Team Agent** oder ein Benutzer mit den Rollen **Admin** (Administrator) oder **Technical** (Techniker) sein.

Führen Sie folgende Schritte in [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) aus:

1. Klicken Sie auf **My Apps** (Meine Apps):

    [![](itunesconnect-images/add01.png "Klicken Sie auf Meine Apps")](itunesconnect-images/add01.png#lightbox)
2. Klicken Sie auf das **+** oben links, und wählen Sie **New iOS App** (Neue iOS-App) aus:

    [![](itunesconnect-images/add02.png "Hinzufügen einer neuen iOS-App")](itunesconnect-images/add02.png#lightbox)
3. In iTunes Connect wird das Dialogfeld **New iOS-App** (Neue iOS-App) angezeigt:

    [![](itunesconnect-images/add03.png "Dialogfeld „Neue iOS-App“")](itunesconnect-images/add03.png#lightbox)
4. Geben Sie in das Feld **Name** den Namen und unter **Version** die Versionsnummer der Anwendung so ein, wie sie im App Store angezeigt werden sollen.
5. Wählen Sie unter **Primary Language** die Hauptsprache aus.
6. Geben Sie die **SKU**-Nummer ein. Dabei handelt es sich um einen eindeutigen, unveränderlichen Bezeichner, mit dem die Anwendung gefunden werden kann. Sie wird dem Benutzer nicht angezeigt und kann nach der Erstellung der App _nicht_ mehr geändert werden.
7. Wählen Sie die **Bundle ID** (Bundle-ID) für die Anwendung aus, die Sie im Developer Center bei der Bereitstellung der Anwendung erstellt haben. Genau diese Bundle-ID muss bei der Signierung des Verteilungsbündels in Visual Studio für Mac oder Visual Studio verwendet werden. Weitere Informationen finden Sie in der Dokumentation unter [Erstellen eines Verteilungsprofils](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile) und [Auswählen eines Verteilungsprofils in einem Xamarin.iOS-Projekt](~/ios/get-started/installation/device-provisioning/index.md).
8. Klicken Sie auf **Create** (Erstellen), um den neuen iTunes Connect-Datensatz für die Anwendung zu erstellen.

Die neue Anwendung wird in iTunes Connect erstellt. Anschließend können Sie die erforderlichen Informationen wie die Beschreibung, die Preise, die Kategorien und die Altersfreigabe bereitstellen:

[![](itunesconnect-images/add04.png "Die neue Anwendung wird in iTunes Connect erstellt")](itunesconnect-images/add04.png#lightbox)

<a name="managing" />

## <a name="managing-app-videos-and-screenshots"></a>Verwalten von App-Videos und -Screenshots

Eines der wichtigsten Elemente für die erfolgreiche Vermarktung Ihrer iOS-Anwendung im App Store sind ansprechende Screenshots und optional Vorschauvideos. Verwenden Sie dazu Liveausschnitte aus Ihrer Anwendung, die die Interaktion des Benutzers hervorheben und die individuellen Funktionen der App vorstellen. Mit Vorschauvideos können sich Benutzer eine Vorstellung von der Benutzererfahrung machen.

Apple hat folgende Empfehlungen für Screenshots:

- Achten Sie darauf, dass Ihr Screenshot die bestmögliche Qualität der Darstellung auf den iOS-Geräten vermittelt, die von der Anwendung unterstützt werden, und dass der Inhalt lesbar ist.
- Auf dem Screenshot darf kein iOS-Gerät als Rahmen zu sehen sein.
- Zeigen Sie die Anwendung aus der Sicht des Benutzers, im Vollbildmodus und ohne Grafiken oder Ränder um den Screenshot.
- Entfernen Sie immer die Statusleiste aus Screenshots. In iTunes Connect dürfen Screenshots diesen Bereich nicht enthalten.
- Falls möglich, nehmen Sie Screenshots auf hochauflösender Retina iOS-Hardware auf (nicht im iOS-Simulator).
- Auf dem iPhone und dem iPad wird der erste Screenshot in den App Store-Suchergebnissen angezeigt, falls kein Video der App verfügbar ist. Stellen Sie den besten Screenshot daher an erste Stelle.
- Erzählen Sie mit den fünf Screenshots die Geschichte der Anwendung, und unterstreichen Sie dabei spannende Momente.

Apple verlangt, dass Screenshots und Videos in jeder Bildschirmgröße und -auflösung bereitgestellt werden, die Ihre Anwendung unterstützt. Darüber hinaus sollten je nach den unterstützten Ausrichtungen Versionen im Hoch- und Querformat bereitgestellt werden.

Die folgenden Bildschirmgrößen und -auflösungen sind derzeit erforderlich:

[!include[](~/ios/includes/table-itunesconnect.md)]

### <a name="editing-screenshots-in-itunes-connect"></a>Bearbeiten von Screenshots in iTunes Connect

Führen Sie folgende Schritte in [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) aus:

1. Klicken Sie auf **My Apps** (Meine Apps).
2. Klicken Sie auf das **Symbol** Ihrer Anwendung.
3. Wählen Sie die Registerkarte **Versions** (Versionen) aus.
4. Scrollen Sie zum Abschnitt **Screenshots**.
5. Wählen Sie die **Bildgröße** aus, und ziehen Sie die erforderlichen Bilder auf den Bildschirm (bis zu fünf Stück pro Bildschirmgröße):

    [![](itunesconnect-images/screenshot01.png "Wählen Sie die Bildgröße aus, und ziehen Sie die erforderlichen Bilder auf den Bildschirm")](itunesconnect-images/screenshot01.png#lightbox)
6. Wiederholen Sie diese Schritte für alle erforderlichen Bildschirmgrößen.
7. Klicken Sie am oberen Bildschirmrand auf **Save** (Speichern), um die Änderungen zu speichern.

> [!NOTE]
> Hinweis: Apple lehnt Ihre Anwendung ab, wenn die Screenshots oder das Vorschauvideo für die App nicht mit den aktuellen Funktionen Ihrer Anwendung übereinstimmen.

<a name="metadata" />

## <a name="managing-name-description-whats-new-keywords-and-urls"></a>Verwalten des Namens, der Felder „Description“ (Beschreibung) und „What's New“ (Neues in dieser Version) sowie der Schlüsselwörter und URLs

Dieser Abschnitt des iTunes Connect-Anwendungsdatensatzes stellt lokalisierte Informationen zur Anwendung, ihrem Zweck, den Änderungen an neuen Versionen, Schlüsselwörter für die Suche und die iAd-Unterstützung sowie alle unterstützenden URLs bereit.

### <a name="app-name"></a>App-Name

Wählen Sie einen beschreibenden Namen aus, der den Inhalt und Zweck der Anwendung widerspiegelt. Versuchen Sie, ihn so kurz und präzise wie möglich zu halten. Der Name der Anwendung ist maßgeblich, damit Benutzer die Anwendung finden können. Er sollte daher einfach und leicht zu merken sein. Achten Sie vor allem darauf, wie der Name auf iOS-Geräten (iPad, iPhone und iPod Touch) angezeigt wird.

Apple empfiehlt Folgendes bei der Auswahl eines App-Namens:

- Er sollte möglichst kurz, einfach und leicht zu merken sein.
- Stellen Sie sicher, dass kein Copyright oder Trademark eines Drittanbieters verletzt wird.
- Er soll der Funktionalität der Anwendung entsprechen.
- Geben Sie für ausländische Märkte gegebenenfalls einen lokalisierten Namen an.

### <a name="description"></a>Beschreibung 

Schreiben Sie eine klare, präzise und informative Beschreibung zur Anwendung und ihren Funktionen. Die ersten Zeilen sind die wichtigsten und bieten Ihnen die Möglichkeit, einen guten ersten Eindruck zu hinterlassen und das Interesse des Benutzers zu wecken. Beschrieben Sie also das Besondere an Ihrer Anwendung gegenüber ähnlichen Apps.

Apple gibt folgende Tipps für Beschreibungen:

- Es sollten ein oder zwei kurze, einführende Absätze oder eine kurze Aufzählung der wichtigsten Funktionen enthalten sein.
- Bieten Sie für ausländische Märkte gegebenenfalls lokalisierte Beschreibungen an.
- Nehmen Sie Bewertungen, Lob oder Erfahrungsberichte von Benutzern, wenn überhaupt, nur am Ende auf.
- Verwenden Sie Zeilenumbrüche und Aufzählungszeichen, um die Lesbarkeit zu verbessern.
- Beachten Sie, dass die App-Beschreibung im App Store auf verschiedenen Gerätetypen unterschiedlich dargestellt wird. Daher sollten Sie sicherstellen, dass die wichtigsten Sätze in der Beschreibung leicht sichtbar sind.

### <a name="whats-new"></a>Neues

Beim Hochladen einer neuen Version Ihrer Anwendung sollten Sie das Feld **What‘s New** (Neues in dieser Version) sorgfältig und wohlüberlegt ausfüllen.

Apple empfiehlt dafür Folgendes:

- Fügen Sie Messaging-Funktionen hinzu, um Benutzer zu einem Update zu ermutigen.
- Listen Sie Elemente nach ihrer Wichtigkeit auf, und heben Sie Änderungen und Fehlerkorrekturen hervor.
- Stellen Sie die Änderungen in einfacher, natürlicher Sprache statt in technischem Jargon vor.

### <a name="keywords"></a>Stichwörter

Durchdachte und strategische Schlüsselwörter, die sich auf die Funktionalität der Anwendung beziehen, helfen Benutzern dabei, die Anwendung leicht im App Store zu finden. Falls Ihre Anwendung darüber hinaus iAd-Ads enthält, verwendet das iAd App Network die Schlüsselwörter bei der Auswahl der in Ihrer App eingeblendeten Werbeanzeigen.

Apple hat folgende Empfehlungen für Schlüsselwörter:

- Verwenden Sie keine konkurrierenden Namen von Apps, Unternehmen, Produkten oder Marken.
- Verwenden Sie keine generischen Wörter oder Begriffe.
- Verwenden Sie keine ungeeigneten oder anstößigen Begriffe oder irrelevante Wörter wie Namen von Prominenten.
- Lokalisieren Sie Schlüsselwörter ggf. für ausländische Märkte.

### <a name="urls"></a>URLs

Apple verlangt, dass Entwickler einen Link zu ihrer Website bereitstellen und Support für alle Probleme oder Fragen anbieten, die Benutzer mit bzw. zu der Anwendung haben könnten. Außerdem muss ein Link zur Datenschutzrichtlinie der Anwendung bereitgestellt werden (die auf Ihrer Website gehostet werden muss).

Stellen Sie optional einen Link zu den auf Ihrer Website gehosteten Marketinginformationen bereit, denn dort können Sie mehr Informationen zur Anwendung als im App Store anbieten.

### <a name="editing-name-description-whats-new-keywords-and-urls-in-itunes-connect"></a>Bearbeiten des Namens, der Beschreibung, der Neuigkeiten, der Schlüsselwörter oder der URLs in iTunes Connect

Führen Sie folgende Schritte in [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) aus:

1. Klicken Sie auf **My Apps** (Meine Apps).
2. Klicken Sie auf das **Symbol** Ihrer Anwendung.
3. Wählen Sie die Registerkarte **Versions** (Versionen) aus.
4. Scrollen Sie zum Abschnitt **Name**.
5. Tragen Sie alle erforderlichen Informationen ein:

    [![](itunesconnect-images/name01.png "Bearbeiten des Namens, der Beschreibung, der Neuigkeiten, der Schlüsselwörter oder der URLs in iTunes Connect")](itunesconnect-images/name01.png#lightbox)
6. Klicken Sie am oberen Bildschirmrand auf **Save** (Speichern), um die Änderungen zu speichern.

> [!IMPORTANT]
> Hinweis: Apple lehnt Ihre Anwendung ab, wenn der Name, die Beschreibung, die Neuigkeiten, die Schlüsselwörter oder die URLs nicht mit den aktuellen Funktionen Ihrer App übereinstimmen.

<a name="general" />

## <a name="maintaining-general-app-information"></a>Verwalten des Bereichs „General App Information“ (Allgemeine Informationen zur App)

In diesem Abschnitt des iTunes Connect-Anwendungsdatensatzes wird die eindeutige ID der Anwendung (wie von Apple zugewiesen) bereitgestellt sowie ihre Kategorien, die Altersfreigabe, das Copyright und die Informationen zum herausgebenden Unternehmen.

### <a name="app-icon"></a>App-Symbol

> [!IMPORTANT]
>  App-Symbole werden nicht mehr über iTunes Connect übermittelt. Sie müssen durch ein **AppIcon**-Bild übermittelt werden, das in der Datei **Assets.xcassets** des Projekts festgelegt ist. Weitere Informationen finden Sie im Handbuch zu [App Store-Symbolen](~/ios/app-fundamentals/images-icons/app-store-icon.md).

Das App-Symbol ist das Gesicht Ihrer Anwendung den Benutzern gegenüber. Es muss daher einprägsam und auch bei kleiner Größe gut erkennbar sein. Einprägsame Symbole sind übersichtlich, einfach und sofort wiedererkennbar.

Apple empfiehlt Folgendes für Anwendungssymbole:

- Entwerfen Sie ein für Ihre Anwendung geeignetes Symbol.
- Erstellen Sie ein einfaches Symbol, das dem Design der Anwendung entspricht.
- Verwenden Sie keine Wörter.
- Denken Sie global: Ein einzelnes App-Symbol wird im App Store für alle Regionen verwendet.

Sie benötigen dafür ein Bild mit 1024 x 1024 Pixel.

Weitere Informationen finden Sie bei Apple unter [iOS Human Interface Guidelines (Richtlinien für menschliche iOS-Schnittstellen)](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/index.html#//apple_ref/doc/uid/TP40006556) und im Abschnitt zum großen App-Symbol in der Dokumentation mit den [allgemeinen Informationen zur App](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Appendices/Properties.html#//apple_ref/doc/uid/TP40011225-CH26-SW7).

### <a name="app-id"></a>App-ID

Dies ist eine eindeutige Identifikationsnummer, die der Anwendung bei der Erstellung des iTunes Connect-Datensatzes von Apple zugewiesen wird. Sie können diese Nummer verwenden, wenn Sie verschiedene webbasierte Schnittstellen aufrufen, die Apple bereitstellt, einschließlich App Store-Informationen auf Ihrer Website.

### <a name="version-number"></a>Versionsnummer

Dies ist die aktuelle, aktive Version der Anwendung, so wie Sie Benutzern im App Store angezeigt wird.

### <a name="category-and-sub-category"></a>Kategorie und Unterkategorie

Ein wichtiger Aspekt für die Auffindbarkeit Ihrer Anwendung ist die Kategorie, in der sie im App Store angezeigt wird. Benutzer können eine Sammlung von Apps anhand von Kategorien durchsuchen und so die für sie interessanten Anwendungen finden. In iTunes Connect können Sie Anwendungen beim Definieren bis zu zwei verschiedenen Kategorien zuweisen. Wählen Sie dabei unbedingt die Kategorien aus, die die Hauptfunktion der Anwendung am besten beschreiben.

### <a name="ratings"></a>Altersfreigabe

Alle Anwendungen im App Store müssen eine Altersfreigabe haben. Diese dient dem Jugendschutz und schränkt den Zugriff für Kinder je nach Typ und den Inhalt der Anwendung ein. iTunes Connect stellt für die Definition Ihrer Anwendung eine Liste von Inhaltsbeschreibungen bereit. Darin geben Sie an, wie häufig bestimmte Inhalte in Ihrer Anwendung auftauchen. Ihre Auswahl wird in eine Altersfreigabe umgewandelt, die im App Store angezeigt wird.

Für Anwendungen, die sich an Kinder richten, gibt es im App Store eine eigene Kategorie für Kinder bis 11 Jahre. Selbst wenn sich Ihre Anwendung nicht ausdrücklich an Kinder wendet, helfen Sie Benutzern, indem Sie angemessene Altersfreigaben für Inhalte angeben.

> [!IMPORTANT]
> Hinweis: Apple lehnt alle Anwendungen mit obszönen, pornografischen, anstößigen und verleumderischen Inhalten ab.

### <a name="copyright-and-company-information"></a>Copyright und Informationen über das Unternehmen

Die Angabe von Copyright-Informationen ist freiwillig, während Informationen zum herausgebenden Unternehmen wie Adress- und Kontaktdaten (für im koreanischen App Store veröffentlichte Anwendungen) bereitgestellt werden müssen. Diese Informationen werden wie erforderlich im App Store angezeigt.

### <a name="editing-general-app-information-in-itunes-connect"></a>Bearbeiten des Bereichs „General App Information“ (Allgemeine Informationen zur App) in iTunes Connect

Führen Sie folgende Schritte in [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) aus:

1. Klicken Sie auf **My Apps** (Meine Apps).
2. Klicken Sie auf das **Symbol** Ihrer Anwendung.
3. Wählen Sie die Registerkarte **Versions** (Versionen) aus.
4. Scrollen Sie zum Abschnitt **General App Information** (Allgemeine Informationen zur App).
5. Tragen Sie alle erforderlichen Informationen ein:

    [![](itunesconnect-images/general01.png "Bearbeiten des Bereichs „Allgemeine Informationen zur App“ in iTunes Connect")](itunesconnect-images/general01.png#lightbox)
6. Klicken Sie neben **Rating** (Altersfreigabe) auf **Edit** (Bearbeiten), um die entsprechenden Informationen einzugeben:

    [![](itunesconnect-images/general02.png "Bearbeiten der Altersfreigabe")](itunesconnect-images/general02.png#lightbox)
6. Klicken Sie am oberen Bildschirmrand auf **Save** (Speichern), um die Änderungen zu speichern.

> [!NOTE]
> Hinweis: Apple lehnt Ihre Übermittlung ab, wenn die Kategorien oder die Altersfreigabe nicht mit den aktuellen Funktionen Ihrer Anwendung übereinstimmen.

<a name="game-center" />

## <a name="maintaining-game-center-information"></a>Verwalten von Informationen im Game Center

Für iOS-Spieleanwendungen, die das Apple Game Center unterstützen, können Sie Informationen wie für den Benutzer verfügbare Ranglisten und Zwischenerfolge im Spiel bereitstellen und angeben, ob die Anwendung über eine Netzwerkverbindung Multiplayer-kompatibel ist.

### <a name="editing-game-center-information-in-itunes-connect"></a>Bearbeiten von Game Center-Informationen in iTunes Connect

Führen Sie folgende Schritte in [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) aus:

1. Klicken Sie auf **My Apps** (Meine Apps).
2. Klicken Sie auf das **Symbol** Ihrer Anwendung.
3. Wählen Sie die Registerkarte **Versions** (Versionen) aus.
4. Scrollen Sie zum Abschnitt **Game Center**.
5. Stellen Sie den Schalter im Abschnitt **Game Center** auf **On** (An).
5. Tragen Sie alle erforderlichen Informationen ein:

    [![](itunesconnect-images/gamecenter01.png "Bearbeiten von Game Center-Informationen in iTunes Connect")](itunesconnect-images/gamecenter01.png#lightbox)
6. Klicken Sie am oberen Bildschirmrand auf **Save** (Speichern), um die Änderungen zu speichern.

Aktivieren Sie das **Game Center** auf der entsprechenden Registerkarte, und aktualisieren Sie alle für diese Anwendung verfügbaren **Leaderboards** (Ranglisten) oder **Achievements** (Erfolge):

[![](itunesconnect-images/gamecenter02.png "Aktivieren von Game Center")](itunesconnect-images/gamecenter02.png#lightbox)

[![](itunesconnect-images/gamecenter03.png "Aktualisieren Sie alle für diese Anwendung verfügbaren Bestenlisten oder Erfolge")](itunesconnect-images/gamecenter03.png#lightbox)

## <a name="maintaining-app-review-information"></a>Verwalten von Informationen zur App-Bewertung

In diesem Abschnitt können Sie die erforderlichen Informationen für das Apple-Personal bereitstellen, das Ihre Anwendung prüft, z.B. Kontaktdaten (falls es Fragen gibt), alle ggf. erforderlichen Demokonten und Hinweise, die dem Tester dabei helfen, Ihre App erfolgreich zu prüfen.

### <a name="editing-app-review-information-in-itunes-connect"></a>Bearbeiten des Bereichs „App Review Information“ (App-Bewertungen) in iTunes Connect

Führen Sie folgende Schritte in [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) aus:

1. Klicken Sie auf **My Apps** (Meine Apps).
2. Klicken Sie auf das **Symbol** Ihrer Anwendung.
3. Wählen Sie die Registerkarte **Versions** (Versionen) aus.
4. Scrollen Sie zum Abschnitt **App Review Information** (App-Bewertungen).
5. Tragen Sie alle erforderlichen Informationen ein:

    [![](itunesconnect-images/review01.png "Bearbeiten des Bereichs „App-Bewertungen“ in iTunes Connect")](itunesconnect-images/review01.png#lightbox)
6. Wählen Sie aus, wie die Anwendung nach erfolgreicher Überprüfung im App Store veröffentlicht werden soll:

    [![](itunesconnect-images/review02.png "Bearbeiten von Release-Information in iTunes Connect")](itunesconnect-images/review02.png#lightbox)
6. Klicken Sie am oberen Bildschirmrand auf **Save** (Speichern), um die Änderungen zu speichern.


## <a name="maintaining-pricing-information"></a>Verwalten der Preisinformationen

Wenn Sie Ihre Anwendung für den Verkauf veröffentlichen möchten, müssen Sie eine der verfügbaren Preisstufen und das Datum auswählen, an dem der angegebene Preise in Kraft tritt. Bei Redaktionsschluss war dies z.B. **Preisstufe 1**:

[![](itunesconnect-images/price01.png "Verwalten der Preisinformationen")](itunesconnect-images/price01.png#lightbox)

### <a name="educational-discount"></a>Rabatt für Bildungseinrichtungen

Aktivieren Sie dieses Kontrollkästchen, wenn Ihre Anwendung Bildungseinrichtungen zu einem reduzierten Preis angeboten werden soll, sollten diese mehrere Exemplare gleichzeitig kaufen. Die Details zum Rabatt finden sich im neuesten **Paid Application Agreement** (Vertrag zu kostenpflichtigen Anwendungen), den Sie unterschreiben müssen, bevor diese Anwendung Bildungseinrichtungen zur Verfügung gestellt wird.

### <a name="custom-business-to-business-application"></a>Anpassbare B2B-Anwendungen

Eine Anwendung, die als **anpassbare B2B-Anwendung** eingerichtet wurde, steht nur den Kunden des **Apple Volume Purchase Program** zur Verfügung, die Sie in iTunes Connect angeben. Außerdem ist die App dann nur in den entsprechenden Regionen verfügbar (Volume Purchase Program-Kunden aus den USA müssen z.B. das US-amerikanische App Store-Volume Purchase Program for Business verwenden).

Anpassbare B2B-Anwendungen stehen nicht für Bildungseinrichtungen oder allgemeine App Store-Kunden zur Verfügung. Weitere Informationen zum *App Store-Volume Purchase Program for Business* finden Sie auf der Seite [Frequently Asked Questions](http://vpp.itunes.apple.com/faq) bei Apple. Weitere Informationen dazu, wie sich Ihre Kunden für das **Volume Purchase Program** registrieren können, finden Sie auf der Seite [Bereitstellungsprogramme](http://enroll.vpp.itunes.apple.com) bei Apple.

### <a name="editing-pricing-information-in-itunes-connect"></a>Bearbeiten der Preisinformationen in iTunes Connect

Führen Sie folgende Schritte in [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) aus:

1. Klicken Sie auf **My Apps** (Meine Apps).
2. Klicken Sie auf das **Symbol** Ihrer Anwendung.
3. Klicken Sie auf die Registerkarte **Pricing** (Preise):

    [![](itunesconnect-images/price02.png "Bearbeiten der Preisinformationen in iTunes Connect")](itunesconnect-images/price02.png#lightbox)
4. Wählen Sie unter **Availability Date** das Datum aus, ab dem die App verfügbar ist.
5. Wählen Sie den gewünschten Preis aus der Dropdownliste **Price Tier** (Preisstufe) aus.
5. Aktivieren Sie optional **Educational Discounts** (Rabatte für Bildungseinrichtungen).
6. Definieren Sie die Anwendung optional als eine **Custom B2B Application** (Anpassbare B2B-Anwendung).
6. Klicken Sie auf **Save** (Speichern), um die Änderungen zu speichern.

<a name="iap" />

## <a name="maintaining-in-app-purchase-information"></a>Verwalten des Bereichs „In-App Purchases“ (In-App-Käufe)

Wenn Sie virtuelle, App-interne Produkte verkaufen möchten (z.B. neue Level in Spielen oder Anwendungsfunktionen), können Sie diese Elemente in diesem Abschnitt erstellen und verwalten.

[![](itunesconnect-images/inapp01.png "Verwalten des Bereichs „In-App-Käufe“")](itunesconnect-images/inapp01.png#lightbox)

Weitere Informationen zu In-App-Käufen in einer Xamarin.iOS-Anwendung finden Sie in unserer Dokumentation zu [In-App-Käufen](~/ios/platform/in-app-purchasing/index.md).

## <a name="viewing-application-reviews"></a>Anzeigen von Benutzerbewertungen

Sobald die Anwendung im App Store veröffentlicht wurde, können Benutzer, die die Anwendung kaufen oder kostenlos herunterladen, die App schriftlich bewerten und Sterne dafür vergeben. In diesem Abschnitt können Sie diese Bewertungen anzeigen lassen. Zum Beispiel:

[![](itunesconnect-images/reviews01.png "Anzeigen von Benutzerbewertungen")](itunesconnect-images/reviews01.png#lightbox)

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde beschrieben, wie Sie eine Xamarin.iOS-Anwendung mit iTunes Connect für die Veröffentlichung im App Store vorbereiten und wie Sie alle angezeigten Informationen zu Ihrer Anwendung im Store verwalten.

## <a name="related-links"></a>Verwandte Links

- [Working with Images (Arbeiten mit Bildern)](~/ios/app-fundamentals/images-icons/index.md)
- [Anleitung für den iOS-App-Entwicklungsworkflow: Verteilen von Anwendungen](http://developer.apple.com/library/ios/#documentation/Xcode/Conceptual/ios_development_workflow/35-Distributing_Applications/distributing_applications.html)
- [Tipps für die Übermittlung an den App Store](https://developer.apple.com/appstore/resources/submission/tips.html)
- [Richtlinien für die Überprüfung im App Store](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [iTunes Connect-Entwicklerleitfaden](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Chapters/About.html#//apple_ref/doc/uid/TP40011225-CH1-SW1)
