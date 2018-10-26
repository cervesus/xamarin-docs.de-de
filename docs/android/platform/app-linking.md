---
title: App-Verknüpfung unter Android
description: Dieses Handbuch wird erläutert, wie Android 6.0-app-Verknüpfung, ein Verfahren unterstützt, die mobile apps in Websites auf URLs reagieren können. Es wird erläutert, welche app-Verknüpfung ist, wie app-Verknüpfung in einer Android 6.0-Anwendung implementiert und wie Sie eine Website zum Gewähren von Berechtigungen für die mobile app für eine Domäne zu konfigurieren.
ms.prod: xamarin
ms.assetid: 48174E39-19FD-43BC-B54C-9AF11D4B1F91
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: dd4ba236df8e5993c7f7ed86393eb66ce01db595
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111264"
---
# <a name="app-linking-in-android"></a>App-Verknüpfung unter Android

_Dieses Handbuch wird erläutert, wie Android 6.0-app-Verknüpfung, ein Verfahren unterstützt, die mobile apps in Websites auf URLs reagieren können. Es wird erläutert, welche app-Verknüpfung ist, wie app-Verknüpfung in einer Android 6.0-Anwendung implementiert und wie Sie eine Website zum Gewähren von Berechtigungen für die mobile app für eine Domäne zu konfigurieren._

## <a name="app-linking-overview"></a>Übersicht über die App verknüpfen

Mobile Anwendungen nicht mehr befinden sich in einem Silo &ndash; in vielen Fällen sind sie ein wichtiger Komponenten der ihr Unternehmen, zusammen mit ihrer Website. Es ist wünschenswert, dass Unternehmen ihre Onlinepräsenz und mobile Anwendungen, nahtlos mit Links auf einer Website starten mobile Anwendungen und relevanter Inhalt angezeigt, in der mobilen app zu verbinden. *App-Verknüpfung* (so genannte *Deep-linking*) ist eine Technik, die ermöglicht einem mobilen Gerät zum Reagieren auf einen URI ein, und starten Sie eine mobile Anwendung, die an diesen URI entspricht.

Android behandelt app-Verknüpfung über die *Zielsystem* &ndash; klickt der Benutzer auf einen Link in einem mobilen Browser, wird der mobile Browser ein Intent-Objekt, das auf eine registrierte Anwendung Android delegieren senden. Z. B. würde durch Klicken auf einen Link auf einer Website Kochen öffnen Sie eine mobile app, die die Website zugeordnet ist und eine bestimmte Recipe für den Benutzer angezeigt. Liegt mehr als eine Anwendung, behandeln diese Absicht registriert, und klicken Sie dann so genannten Android löst eine *Dialogfeld zur Klärung* , die fragt eines Benutzers welche Anwendung die Anwendung auswählen, die für die Absicht, behandelt werden sollen Beispiel:

![Beispielscreenshot, der ein Dialogfeld zur Klärung](app-linking-images/01-disambiguation-dialog.png)

Android 6.0 verbessert dies durch das automatische Verknüpfung behandeln. Es ist möglich, für Android zum automatisch registrieren einer Anwendung als Standardhandler für einen URI &ndash; die app automatisch gestartet werden und navigieren Sie direkt zu der entsprechenden Aktivität. Wie Android 6.0 entscheidet, eine URI zu behandeln, hängt von den folgenden Kriterien:

1. **Eine vorhandene app ist bereits mit dem URI zugeordneten** &ndash; der Benutzer kann haben bereits eine vorhandene app einem URI zugeordnet. In diesem Fall weiterhin Android wird die Anwendung zu verwenden.
2. **Es ist keine vorhandene app mit dem URI zugeordneten, aber es ist eine unterstützende app installiert.** &ndash; In diesem Szenario wird der Benutzer wurde nicht angegeben eine vorhandene app damit Android der installierten unterstützende Anwendung verwendet werden, um die Anforderung zu verarbeiten.
3. **Keine vorhandenen app ist mit dem URI, aber viele unterstützende apps installiert sind** &ndash; , da es mehrere Anwendungen, die den URI zu unterstützen gibt, das Dialogfeld zur Klärung wird angezeigt, und der Benutzer muss die app wird auswählen. Behandeln Sie den URI an.

Wenn der Benutzer hat keine apps installiert, die den URI zu unterstützen und anschließend wird eine installiert ist, wird Android festgelegt dieser Anwendung als Standardhandler für den URI nach dem Überprüfen der Zuordnung mit der Website, die mit dem URI zugeordnet ist.

Dieses Handbuch wird erläutert, wie eine Android 6.0-Anwendung zu konfigurieren und das Erstellen und veröffentlichen Sie die Datei Digital Asset Links zur Unterstützung der app-Verknüpfung in Android 6.0.

## <a name="requirements"></a>Anforderungen

Dieses Handbuch erfordert Xamarin.Android 6.1 und eine Anwendung für Android 6.0 (API-Ebene 23) oder höher.

App-Verknüpfung in früheren Versionen von Android möglich ist, die Verwendung der [Rivets NuGet-Paket](https://www.nuget.org/packages/Rivets/) im Xamarin Component Store. Die Nieten-Paket ist nicht kompatibel mit Android 6.0; app-Verknüpfung Es unterstützt Verknüpfen von Android 6.0-app nicht.

## <a name="configuring-app-linking-in-android-60"></a>Konfigurieren von App-Verknüpfung in Android 6.0

Einrichten von app-Links in Android 6.0 umfasst zwei Hauptschritte:

1. **Hinzufügen von einem oder mehreren Intent-Filtern für der Website URI** &ndash; Beabsichtigter Filter Anleitung Android bei Klicken auf eine URL in einem mobilen Browser behandelt.
2. **Veröffentlichen einer *Digital Asset Links JSON* Datei auf der Website** &ndash; -Datei, die auf einer Website hochgeladen und wird von Android verwendet, um zu überprüfen, ob die Beziehung zwischen der mobilen app und der Domäne der Website. Ohne diesen Schritt kann Android die app als Standardhandle für den URI nicht installieren; der Benutzer muss dies manuell tun.

<a name="configure-intent-filter" />

### <a name="configuring-the-intent-filter"></a>Der Zielfilter konfigurieren

Es ist notwendig, einen intent-Filter konfigurieren, der einen URI (oder möglich, einen Satz von URIs) auf einer Website eine Aktivität in einer Android-Anwendung zugeordnet ist. In Xamarin.Android-diese Beziehung wird hergestellt, indem Sie eine Aktivität mit dem Verzieren der [IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/). Der Zielfilter muss die folgende Informationen deklariert werden:

* **`Intent.ActionView`** &ndash; Dadurch wird der Zielfilter zum Reagieren auf Anforderungen zum Anzeigen von Informationen registriert.
* **`Categories`** &ndash;  Der Zielfilter sollten beide registrieren **[Intent.CategoryBrowsable](https://developer.xamarin.com/api/field/Android.Content.Intent.CategoryBrowsable/)** und **[Intent.CategoryDefault](https://developer.xamarin.com/api/field/Android.Content.Intent.CategoryDefault/)** ordnungsgemäß werden sollen Behandeln Sie die Web-URI.
* **`DataScheme`** &ndash; Der Zielfilter muss deklariert werden `http` und/oder `https`. Hierbei handelt es sich um die nur zwei gültige Schemas.
* **`DataHost`** &ndash; Dies ist die Domäne, die die URIs aus hergestellt werden.
* **`DataPathPrefix`** &ndash; Dies ist ein optionaler Pfad zu den Ressourcen auf der Website.
* **`AutoVerify`** &ndash; Die `autoVerify` Attribut teilt Android, um zu überprüfen, ob die Beziehung zwischen der Anwendung und der Website. Dies wird im folgenden näher eingegangen werden.

Das folgende Beispiel zeigt, wie Sie mit der [IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) , behandeln Links aus `https://www.recipe-app.com/recipes` und `http://www.recipe-app.com/recipes`:

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

Android überprüft jeden Host, der durch die Beabsichtigter Filter für die Datei mit der digitalen Ressourcen auf der Website vor der Registrierung der Anwendungs als Standardhandler für einen URI identifiziert wird. Damit Android die app als Standardhandler herstellen kann, müssen alle Beabsichtigter Filter Überprüfung bestehen.

### <a name="creating-the-digital-assets-link-file"></a>Erstellen der digitalen Ressourcen Link-Datei

Android-app-Verknüpfung 6.0 erfordert, dass es sich bei Android überprüfen Sie, ob die Zuordnung zwischen der Anwendung und die Website vor dem Festlegen der Anwendungs als Standardhandler für den URI an. Diese Überprüfung erfolgt, wenn die Anwendung erstmals installiert wird. Die *digitale Objekte-Links* Datei ist eine JSON-Datei, die von den relevanten Webdomain(s) gehostet wird.

> [!NOTE]
> Die `android:autoVerify` Attribut muss festgelegt werden, von dem Zielfilter &ndash; andernfalls Android wird die Überprüfung nicht ausführen.

Die Datei wird durch den Webmaster der Domäne an der Position platziert **https://domain/.well-known/assetlinks.json**.

Die digitale Asset-Datei enthält die erforderlichen Metadaten für Android, um zu überprüfen, ob die Zuordnung. Ein **assetlinks.json** Datei hat die folgenden Schlüssel-Wert-Paare:

* `namespace` &ndash; der Namespace der Android-Anwendung.
* `package_name` &ndash; Der Paketname der Android-Anwendung (im Anwendungsmanifest deklariert).
* `sha256_cert_fingerprints` &ndash; der SHA256-Fingerabdrücke der signierte Anwendung. Sie finden Sie im Handbuch [Suchen von MD5 oder SHA1-Signatur Ihres Keystores des](~/android/deploy-test/signing/keystore-signature.md) für Weitere Informationen zum Erwerb einer Anwendung des SHA1-Fingerabdrucks.

Der folgende Codeausschnitt zeigt ein Beispiel für **assetlinks.json** mit einer einzelnen Anwendung aufgeführt:

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

Es ist möglich, Registrieren von mehr als eine SHA256-Fingerabdruck zur Unterstützung der verschiedenen Versionen oder builds Ihrer Anwendung. Dies als Nächstes **assetlinks.json** Datei ist ein Beispiel für mehrere Anwendungen registrieren:

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

Die [Google Digital Asset Links Website](https://developers.google.com/digital-asset-links/tools/generator) verfügt über ein online-Tool, die helfen können, mit dem Erstellen und testen die digitalen Ressourcen-Datei.

### <a name="testing-app-links"></a>Testen von App-Links

Nach der Implementierung von app-Verknüpfungen, sollten die verschiedenen Komponenten getestet werden, um sicherzustellen, dass sie erwartungsgemäß funktionieren.

Es ist möglich, um sicherzustellen, dass die digitale Ressourcen-Datei ordnungsgemäß formatiert und mithilfe des Digital Asset Links-API von Google, gehostet wird, wie im folgenden Beispiel gezeigt:

```html
https://digitalassetlinks.googleapis.com/v1/statements:list?source.web.site=
  https://<WEB SITE ADDRESS>:&relation=delegate_permission/common.handle_all_urls
```

Es gibt zwei Tests, die ausgeführt werden können, um sicherzustellen, dass die beabsichtigte Filter ordnungsgemäß konfiguriert wurden und die app als Standard-Handler für einen URI festgelegt wird:

1.  Die digitale Medienobjektdatei wird ordnungsgemäß gehostet werden, wie oben beschrieben. Im erste Test entsendet ein Intent-Objekt, das Android, für die mobile Anwendung umleiten soll. Die Android-Anwendung sollte starten, und zeigen die Aktivität, die für die URL registriert. An einer Eingabeaufforderung Folgendes ein:

    ```shell
    $ adb shell am start -a android.intent.action.VIEW \
        -c android.intent.category.BROWSABLE \
        -d "http://<domain1>/recipe/scalloped-potato"
    ```

2.  Zeigen Sie den vorhandenen Link, der Behandlung von Richtlinien für die Anwendungen auf einem Gerät installiert. Der folgende Befehl wird eine Liste der Link-Richtlinien für jeden Benutzer auf dem Gerät mit den folgenden Informationen sichern. Geben Sie an der Eingabeaufforderung folgenden Befehl ein:

    ```shell
    $ adb shell dumpsys package domain-preferred-apps
    ```

    * **`Package`** &ndash; Der Paketname der Anwendung.
    * **`Domain`** &ndash; Die Domänen, für deren Weblinks von der Anwendung verarbeitet wird (durch Leerzeichen getrennt)
    * **`Status`** &ndash; Dies ist der aktuelle Status der Link-Behandlung für die app. Der Wert **immer** bedeutet, die die Anwendung verfügt über `android:autoVerify=true` deklariert und wurde die Überprüfung übergeben. Es folgt eine hexadezimale Zahl, die das Android System-Datensatz, der die Einstellung darstellt.

    Zum Beispiel:

    ```shell
    $ adb shell dumpsys package domain-preferred-apps

    App linkages for user 0:
    Package: com.android.vending
    Domains: play.google.com market.android.com
    Status: always : 200000002
    ```

## <a name="summary"></a>Zusammenfassung

Dieser Leitfaden erläutert, wie app-Verknüpfung funktioniert in Android 6.0. Sie behandelt dann konfigurieren Sie eine Android 6.0-Anwendung zu unterstützen, und reagieren auf app-Links. Es wurde auch zum Testen der app-Verknüpfung in einer Android-Anwendung beschrieben.


## <a name="related-links"></a>Verwandte Links

- [Finden der MD5- oder SHA1-Signatur Ihres Keystores](~/android/deploy-test/signing/keystore-signature.md)
- [Aktivitäten und Intents zusammen](https://university.xamarin.com/classes#4)
- [AppLinks](http://applinks.org/)
- [Google-digitale Objekte-Links](https://developers.google.com/digital-asset-links/)
- [Anweisung Liste-Generator und Tester](https://developers.google.com/digital-asset-links/tools/generator)
