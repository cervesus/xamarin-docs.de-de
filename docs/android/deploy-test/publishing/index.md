---
title: Veröffentlichen einer Anwendung
ms.prod: xamarin
ms.assetid: 51E19000-040A-2B74-C462-EC57C617085C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 3062afda82be2b45146687b9d77210a42f8f55fa
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "75487957"
---
# <a name="publishing-an-application"></a>Veröffentlichen einer Anwendung

Wenn Sie eine praktische App erstellt haben, will diese auch benutzt werden. In diesem Abschnitt werden die Schritte erläutert, die mit der öffentlichen Verteilung einer App über verschiedene Kanäle einhergehen, die mit Xamarin.Android erstellt wurde. Zu diesen Kanälen zählen z.B. E-Mails, ein privater Webserver, Google Play oder der Amazon Appstore.

## <a name="overview"></a>Übersicht

Der letzte Schritt der Entwicklung einer Xamarin.Android-Anwendung ist die Veröffentlichung dieser. Die Veröffentlichung ist der Vorgang der Kompilierung einer Xamarin.Android-Anwendung, sodass Sie gebrauchsfertig ist und von Benutzern auf deren Geräten installiert werden kann. Sie besteht aus zwei wesentlichen Aufgaben:

- **Vorbereitung für die Veröffentlichung**: Es wird an einer Releaseversion der Anwendung gearbeitet, die für Android-Geräte bereitgestellt werden soll. Weitere Informationen zur Vorbereitung des Releases finden Sie unter [Vorbereiten einer Anwendung für die Veröffentlichung](~/android/deploy-test/release-prep/index.md).

- **Verteilung**: Die Releaseversion einer Anwendung wird über mindestens einen Verteilungskanal verfügbar gemacht.

In folgendem Diagramm werden die Schritte zur Veröffentlichung einer Xamarin.Android-Anwendung veranschaulicht:

[![Erstellen und Bereitstellen (Flowchart)](images/build-and-deploy-steps.png)](images/build-and-deploy-steps.png#lightbox)

Wie Sie im oben stehenden Diagramm erkennen können, unterscheidet sich die Vorbereitung bei unterschiedlichen Verteilungsmethoden nicht. Es gibt mehrere Möglichkeiten, wie Sie eine Android-Anwendung für Benutzer bereitstellen können:

- **Über eine Website**: Eine Xamarin.Android-Anwendung kann auf einer Website zum Download zur Verfügung gestellt werden. Dort können Benutzer die Anwendung über einen Link installieren.
- **Über eine E-Mail**: Benutzer können eine Xamarin.Android-Anwendung über Ihre E-Mail installieren. Die Anwendung wird installiert, wenn der Anhang mit einem Android-Gerät geöffnet wird.
- **Über einen Marketplace**: Es gibt mehrere Marketplaces für Anwendungen, die der Verteilung dienen, wie z.B. [Google Play](https://play.google.com/) oder der [Amazon App Store für Android](https://www.amazon.com/mobile-apps/b?ie=UTF8&node=2350149011).

Ein etablierten Marketplace ist die am häufigsten gewählte Methode zur Verteilung einer Anwendung, da so die weiteste Reichweite und die höchstmögliche Kontrolle über die Verteilung erreicht wird. Die Veröffentlichung einer Anwendung auf einem Marketplace geht jedoch mit einem Mehraufwand einher.

Eine Xamarin.Android-Anwendung kann gleichzeitig über mehrere Kanäle verteilt werden. Eine Anwendung kann beispielsweise in Google Play und dem Amazon Appstore für Android veröffentlicht sowie über einen Webserver heruntergeladen werden.

Die beiden anderen Verteilungsmethoden (Download und E-Mail) sind besonders bei einer eingegrenzten Teilmenge an Benutzern nützlich, wie etwa in einer Unternehmensumgebung oder bei einer Anwendung, die nur für eine kleine oder ausgewählte Gruppe von Benutzern gedacht ist.
Die Verteilung über einen Server oder via E-Mail sind zudem einfachere Veröffentlichungsmodelle. Der Vorbereitungsaufwand für die Veröffentlichung ist geringer.

Mit dem Verteilungsprogramm für mobile Apps von Amazon können Entwickler mobiler Apps ihre Anwendungen auf Amazon verteilen und verkaufen. Benutzer können mit dem Amazon Appstore-Anwendung auf ihren Android-Geräten Apps entdecken und erwerben. Unten stehend finden Sie einen Screenshot des Amazon Appstores, der auf einem Android-Gerät ausgeführt wird:

Google Play ist einer der umfassendsten und beliebtesten Marketplaces für Android-Anwendungen. Auf Google Play können Benutzer Anwendungen entdecken, herunterladen, bewerten und kaufen – und das mit einem Klick auf ein einziges Symbol auf ihrem Gerät oder ihrem Computer. Google Play bietet zudem Tools zur Analyse von Verkaufs- und Markttrends. Darüber hinaus bietet es auch Tools, mit denen Sie eingrenzen können, welche Geräte und Benutzer eine Anwendung herunterladen dürfen. Unten stehend finden Sie einen Screenshot von Google Play auf einem Android-Gerät:

[![Screenshot von Google Play](images/google-play-app.png)](images/google-play-app.png#lightbox)

In diesem Abschnitt wurde gezeigt, wie die Anwendung in einem Store wie Google Play zusammen mit passenden Werbematerialien hochgeladen werden kann. Es wurden allgemeine Informationen zu APK-Erweiterungsdateien und eine Übersicht über deren Funktionsweise zur Verfügung gestellt. Außerdem wurden die Google-Lizenzierungsdienste beschrieben. Zum Schluss wurden alternative Verteilungsmethoden wie u.a. die Verwendung eines HTTP-Webservers, die einfache Verteilung per E-Mail und der Amazon App Store für Android dargestellt.

## <a name="related-links"></a>Verwandte Links

- [HelloWorldPublishing (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/helloworldpublishing)
- [Buildprozess](~/android/deploy-test/building-apps/build-process.md)
- [Verknüpfen](~/android/deploy-test/linker.md)
- [Abrufen eines API-Schlüssels für Google Maps](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [Bereitstellen über Visual Studio App Center](https://docs.microsoft.com/appcenter/distribution/stores/googleplay)
- [Anwendungssignierung](https://source.android.com/security/apksigning/)
- [Veröffentlichung in Google Play](https://developer.android.com/distribute/googleplay/publish/index.html)
- [Google-Anwendungslizenzierung](https://developer.android.com/guide/google/play/licensing/index.html)
- [Android.Play.ExpansionLibrary](https://github.com/mattleibow/Android.Play.ExpansionLibrary)
- [Verteilungsportal für mobile Apps](https://developer.amazon.com/welcome.html)
- [Häufig gestellte Fragen zur Verteilung mobiler Apps von Amazon](https://developer.amazon.com/help/faq.html)
