---
title: App-Verknüpfung in Android
description: In dieser Anleitung wird erläutert, wie Android 6,0 App-Verknüpfungen unterstützt, eine Technik, mit der Mobile Apps auf URLs auf Websites reagieren können. Es wird erläutert, was app-Linking ist, wie die APP-Verknüpfung in einer Android 6,0-Anwendung implementiert wird und wie eine Website so konfiguriert wird, dass der Mobile App für eine Domäne Berechtigungen erteilt werden.
ms.prod: xamarin
ms.assetid: 48174E39-19FD-43BC-B54C-9AF11D4B1F91
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 9ca14ff360fb3f1d7fdc8df277a93b0d30c4394c
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70119695"
---
# <a name="app-linking-in-android"></a>App-Verknüpfung in Android

_In dieser Anleitung wird erläutert, wie Android 6,0 App-Verknüpfungen unterstützt, eine Technik, mit der Mobile Apps auf URLs auf Websites reagieren können. Es wird erläutert, was app-Linking ist, wie die APP-Verknüpfung in einer Android 6,0-Anwendung implementiert wird und wie eine Website so konfiguriert wird, dass der Mobile App für eine Domäne Berechtigungen erteilt werden._

## <a name="app-linking-overview"></a>Übersicht über APP-Verknüpfung

Mobile Anwendungen sind in vielen Fällen nicht mehr &ndash; in einem Silo Leben, da Sie zusammen mit Ihrer Website wichtige Komponenten ihrer Unternehmen sind. Es ist wünschenswert, dass Unternehmen ihre Webpräsenz und mobilen Anwendungen nahtlos mit Links auf einer Website verbinden, die mobile Anwendungen startet und relevante Inhalte in der Mobile App anzeigt. *App-Verknüpfung* (auch als *Deep-Linking*bezeichnet) ist eine Technik, die es einem mobilen Gerät ermöglicht, auf einen URI zu reagieren und eine mobile Anwendung zu starten, die diesem URI entspricht.

Android verarbeitet App-Verknüpfungen über das *Intent-System* &ndash; , wenn der Benutzer auf einen Link in einem mobilen Browser klickt. der Mobile Browser versendet eine Absicht, die Android an eine registrierte Anwendung delegiert. Wenn Sie z. b. auf einen Link auf einer kochwebsite klicken, wird eine Mobile App geöffnet, die dieser Website zugeordnet ist, und dem Benutzer wird eine bestimmte Rezept Anzeige angezeigt. Wenn mehr als eine Anwendung für die Verarbeitung dieser Absicht registriert ist, gibt Android das als *mehrdeutigkeits Dialogfeld bezeichnete Dialog* Feld an, in dem ein Benutzer gefragt wird, welche Anwendung die Anwendung auswählen soll, die die Absicht erfüllen soll, z. b.:

![Screenshot eines Beispiels für die Mehrdeutigkeit](app-linking-images/01-disambiguation-dialog.png)

Android 6,0 verbessert dies mithilfe der automatischen Verknüpfungs Behandlung. Android kann eine Anwendung automatisch als Standard Handler für einen URI &ndash; registrieren, der von der APP automatisch gestartet wird, und direkt zur relevanten Aktivität navigiert. Wie Android 6,0 die Behandlung eines URI beschließt, hängt von den folgenden Kriterien ab:

1. **Eine vorhandene APP ist bereits dem URI zugeordnet** . &ndash; Der Benutzer hat möglicherweise bereits eine vorhandene App mit einem URI verknüpft. In diesem Fall wird die Anwendung von Android weiterhin verwendet.
2. **Dem URI ist keine vorhandene App zugeordnet, aber es ist eine unterstützende App installiert** . &ndash; In diesem Szenario hat der Benutzer keine vorhandene App angegeben, daher verwendet Android die installierte unterstützende Anwendung, um die Anforderung zu verarbeiten.
3. **Dem URI ist keine vorhandene App zugeordnet, aber es sind viele unterstützende apps installiert** . &ndash; Da es mehrere Anwendungen gibt, die den URI unterstützen, wird das Dialogfeld für die Mehrdeutigkeit angezeigt, und der Benutzer muss auswählen, welche App den URI verarbeiten soll.

Wenn für den Benutzer keine apps installiert sind, die den URI unterstützen, und anschließend eine installiert wird, legt Android diese Anwendung als Standard Handler für den URI fest, nachdem die Zuordnung zu der Website überprüft wurde, die dem URI zugeordnet ist.

In diesem Handbuch wird erläutert, wie eine Android 6,0-Anwendung konfiguriert wird und wie die Digital Asset Links-Datei erstellt und veröffentlicht wird, um App-Verknüpfungen in Android 6,0 zu unterstützen.

## <a name="requirements"></a>Anforderungen

Diese Anleitung erfordert xamarin. Android 6,1 und eine Anwendung, die auf Android 6,0 (API-Ebene 23) oder höher ausgerichtet ist.

Die APP-Verknüpfung ist in früheren Versionen von Android möglich, indem das [rivets-nuget-Paket](https://www.nuget.org/packages/Rivets/) aus dem xamarin-Komponenten Speicher verwendet wird. Das rivets-Paket ist nicht kompatibel mit der APP-Verknüpfung in Android 6,0; das Verknüpfen von Android 6,0-apps wird nicht unterstützt.

## <a name="configuring-app-linking-in-android-60"></a>Konfigurieren von App-Verknüpfungen in Android 6,0

Das Einrichten von App-Links in Android 6,0 umfasst zwei Hauptschritte:

1. **Hinzufügen eines oder mehrerer Intent-Filter für den Website-URI** &ndash; der Leitfaden für beabsichtigte Filter Android unter How to handle a URL Click in a Mobile Browser.
2. **Veröffentlichen eines *Digital Assets verknüpft eine JSON* -Datei auf der Website** &ndash; . Dies ist eine Datei, die auf eine Website hochgeladen wird und von Android verwendet wird, um die Beziehung zwischen dem Mobile App und der Domäne der Website zu überprüfen. Ohne diesen kann Android die APP nicht als Standard Handle für URI installieren. der Benutzer muss dies manuell durchführen.

<a name="configure-intent-filter" />

### <a name="configuring-the-intent-filter"></a>Konfigurieren des Intent-Filters

Es ist erforderlich, einen beabsichtigten Filter zu konfigurieren, der einen URI (oder einen Satz von URIs) von einer Website einer Aktivität in einer Android-Anwendung zuordnet. In xamarin. Android wird diese Beziehung erstellt, indem eine Aktivität mit dem [intentfilterattribute](xref:Android.App.IntentFilterAttribute)versehen wird. Der Intent-Filter muss die folgenden Informationen deklarieren:

- **`Intent.ActionView`** &ndash; Dadurch wird der Intent-Filter registriert, um auf Anforderungen zum Anzeigen von Informationen zu antworten.
- **`Categories`** Der Intent-Filter sollte sowohl **[Intent. CategoryBrowsable](xref:Android.Content.Intent.CategoryBrowsable)** als auch **[Intent. categorydefault](xref:Android.Content.Intent.CategoryDefault)** registrieren, damit der Web-URI ordnungsgemäß verarbeitet werden kann. &ndash;
- **`DataScheme`** Der Intent-Filter muss und/oder `https`deklarieren `http`. &ndash; Dies sind die einzigen beiden gültigen Schemas.
- **`DataHost`** &ndash; Dies ist die Domäne, von der die URIs stammen.
- **`DataPathPrefix`** &ndash; Dies ist ein optionaler Pfad zu Ressourcen auf der Website.
- **`AutoVerify`** &ndash; Das`autoVerify` -Attribut weist Android an, die Beziehung zwischen der Anwendung und der Website zu überprüfen. Dies wird weiter unten erläutert.

Im folgenden Beispiel wird gezeigt, wie das [intentfilterattribute](xref:Android.App.IntentFilterAttribute) verwendet wird, um `https://www.recipe-app.com/recipes` Links von `http://www.recipe-app.com/recipes`und aus zu behandeln:

```csharp
[IntentFilter(new [] { Intent.ActionView },
              Categories = new[] { Intent.CategoryBrowsable, Intent.CategoryDefault },
              DataScheme = "http",
              DataHost = "recipe-app.com",
              DataPathPrefix = "/recipe",
              AutoVerify=true)]
public class RecipeActivity : Activity
{
    // Code for the activity omitted
}
```

Android überprüft alle Hosts, die von den beabsichtigten Filtern für die Digital Assets-Datei auf der Website identifiziert werden, bevor die Anwendung als Standard Handler für einen URI registriert wird. Alle beabsichtigten Filter müssen die Überprüfung bestehen, bevor Android die APP als Standard Handler einrichten kann.

### <a name="creating-the-digital-assets-link-file"></a>Erstellen der Digital Assets-Linkdatei

Android 6,0-App-Verknüpfung erfordert, dass Android die Zuordnung zwischen der Anwendung und der Website überprüft, bevor die Anwendung als Standard Handler für den URI festgelegt wird. Diese Überprüfung erfolgt, wenn die Anwendung erstmalig installiert wird. Die Datei " *Digital Assets Links* " ist eine JSON-Datei, die von den entsprechenden Webdomänen gehostet wird.

> [!NOTE]
> Das `android:autoVerify` Attribut muss vom Intent-Filter &ndash; festgelegt werden, andernfalls wird die Überprüfung von Android nicht durchgeführt.

Die Datei wird vom Webmaster der Domäne am Speicherort **https://domain/.well-known/assetlinks.json** platziert.

Die Digital Asset-Datei enthält die Metadaten, die für Android erforderlich sind, um die Zuordnung zu überprüfen. Die Datei " **assetlinks. JSON** " verfügt über die folgenden Schlüssel-Wert-Paare:

- `namespace`&ndash; der Namespace der Android-Anwendung.
- `package_name`&ndash; der Paketname der Android-Anwendung (im Anwendungs Manifest deklariert).
- `sha256_cert_fingerprints`&ndash; die SHA256-Fingerabdrücke der signierten Anwendung. Weitere Informationen zum Abrufen des SHA1-Fingerabdrucks einer Anwendung finden Sie im Handbuch [Ermitteln der MD5-oder SHA1-Signatur ihres Keystores](~/android/deploy-test/signing/keystore-signature.md) .

Der folgende Code Ausschnitt ist ein Beispiel für die **Datei "assetlinks. JSON** " mit einer einzelnen aufgelisteten Anwendung:

```json
[
   {
      "relation":[
         "delegate_permission/common.handle_all_urls"
      ],
      "target":{
         "namespace":"android_app",
         "package_name":"com.example",
         "sha256_cert_fingerprints":[
            "14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"
         ]
      }
   }
]
```

Es ist möglich, mehr als einen SHA256-Fingerabdruck zu registrieren, um unterschiedliche Versionen oder Builds der Anwendung zu unterstützen. Die nächste Datei " **assetlinks. JSON** " ist ein Beispiel für das Registrieren mehrerer Anwendungen:

```json
[
   {
      "relation":[
         "delegate_permission/common.handle_all_urls"
      ],
      "target":{
         "namespace":"android_app",
         "package_name":"example.com.puppies.app",
         "sha256_cert_fingerprints":[
            "14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"
         ]
      }
   },
   {
      "relation":[
         "delegate_permission/common.handle_all_urls"
      ],
      "target":{
         "namespace":"android_app",
         "package_name":"example.com.monkeys.app",
         "sha256_cert_fingerprints":[
            "14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"
         ]
      }
   }
]
```

Die [Google Digital Asset Links-Website](https://developers.google.com/digital-asset-links/tools/generator) verfügt über ein Online Tool, das beim Erstellen und Testen der Digital Assets-Datei hilfreich sein kann.

### <a name="testing-app-links"></a>Testen von App-Links

Nachdem Sie App-Links implementiert haben, sollten die verschiedenen Komponenten getestet werden, um sicherzustellen, dass Sie erwartungsgemäß funktionieren.

Es ist möglich, sicherzustellen, dass die Digital Assets-Datei ordnungsgemäß formatiert und gehostet wird, indem Sie die Digital Asset Links-API von Google verwenden, wie im folgenden Beispiel gezeigt:

```html
https://digitalassetlinks.googleapis.com/v1/statements:list?source.web.site=
  https://<WEB SITE ADDRESS>:&relation=delegate_permission/common.handle_all_urls
```

Es können zwei Tests durchgeführt werden, um sicherzustellen, dass die beabsichtigten Filter ordnungsgemäß konfiguriert wurden und dass die APP als Standard Handler für einen URI festgelegt ist:

1. Die Digital Asset-Datei wird wie oben beschrieben ordnungsgemäß gehostet. Der erste Test leitet eine Absicht weiter, welche Android an die Mobile Anwendung umgeleitet werden soll. Die Android-Anwendung sollte die Aktivität starten und anzeigen, die für die URL registriert ist. Geben Sie an einer Eingabeaufforderung Folgendes ein:

    ```shell
    $ adb shell am start -a android.intent.action.VIEW \
        -c android.intent.category.BROWSABLE \
        -d "http://<domain1>/recipe/scalloped-potato"
    ```

2. Zeigen Sie die vorhandenen Richtlinien zur Verbindungs Behandlung für die auf einem bestimmten Gerät installierten Anwendungen an. Mit dem folgenden Befehl wird eine Liste der Link Richtlinien für jeden Benutzer auf dem Gerät mit den folgenden Informationen gesichert. Geben Sie an der Eingabeaufforderung folgenden Befehl ein:

    ```shell
    $ adb shell dumpsys package domain-preferred-apps
    ```

    - **`Package`** &ndash; Der Paketname der Anwendung.
    - **`Domain`** &ndash; Die Domänen (durch Leerzeichen getrennt), deren Weblinks von der Anwendung behandelt werden.
    - **`Status`** &ndash; Dies ist der aktuelle Status der Verbindungs Behandlung für die app. Der Wert bedeutet **immer** , dass die Anwendung deklariert `android:autoVerify=true` hat und die Systemüberprüfung durchgeführt hat. Danach folgt eine hexadezimale Zahl, die den Daten Satz der Einstellung des Android-Systems darstellt.

    Beispiel:

    ```shell
    $ adb shell dumpsys package domain-preferred-apps

    App linkages for user 0:
    Package: com.android.vending
    Domains: play.google.com market.android.com
    Status: always : 200000002
    ```

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurde erläutert, wie die APP-Verknüpfung in Android 6,0 funktioniert. Anschließend wird erläutert, wie eine Android 6,0-Anwendung konfiguriert wird, um App-Links zu unterstützen und darauf zu reagieren. Außerdem wurde erläutert, wie App-Verknüpfungen in einer Android-Anwendung getestet werden.


## <a name="related-links"></a>Verwandte Links

- [Finden der MD5- oder SHA1-Signatur Ihres Keystores](~/android/deploy-test/signing/keystore-signature.md)
- [Aktivitäten und Intents](https://university.xamarin.com/classes#4)
- [AppLinks](http://applinks.org/)
- [Links zu Google Digital Assets](https://developers.google.com/digital-asset-links/)
- [Anweisungs Listen Generator und Tester](https://developers.google.com/digital-asset-links/tools/generator)
