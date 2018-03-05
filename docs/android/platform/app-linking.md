---
title: "In der Android-App verknüpfen"
description: "Diese Anleitung wird erläutert, wie Android 6.0 app-Verknüpfung, eine Technik unterstützt, die mobile-apps So reagieren Sie auf die URLs auf Websites ermöglicht. Es wird erläutert, welche app-Verknüpfung ist, wie app-Verknüpfung in einer Anwendung für Android 6.0 implementiert und eine Website zum Erteilen von Berechtigungen an die mobile Anwendung für eine Domäne zu konfigurieren."
ms.topic: article
ms.prod: xamarin
ms.assetid: DDE54082-6E2B-9ED9-05FB-D9C1D1B1258E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 726890e48407dd26f52c5aeaecf4eab51dcc5182
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="app-linking-in-android"></a>In der Android-App verknüpfen

_Diese Anleitung wird erläutert, wie Android 6.0 app-Verknüpfung, eine Technik unterstützt, die mobile-apps So reagieren Sie auf die URLs auf Websites ermöglicht. Es wird erläutert, welche app-Verknüpfung ist, wie app-Verknüpfung in einer Anwendung für Android 6.0 implementiert und eine Website zum Erteilen von Berechtigungen an die mobile Anwendung für eine Domäne zu konfigurieren._

## <a name="app-linking-overview"></a>Übersicht über App-Verknüpfung

Mobile Anwendungen nicht mehr befinden sich in einem Silo &ndash; in vielen Fällen sind eine wichtige Komponenten von Unternehmen, zusammen mit ihrer Website. Es ist für Unternehmen, ihre Web-Vorhandensein und mobilen Anwendungen nahtlos mit Links auf einer Website Starten von mobilen Anwendungen und relevanter Inhalt angezeigt, in der mobilen app verbinden wünschenswert. *App-Verknüpfung* (auch bezeichnet als *Deep verknüpfen*) ist eine Technik, die ermöglicht einem mobilen Gerät auf einen URI zu reagieren, und starten Sie eine mobile Anwendung, die diesen URI entspricht.

Android behandelt app-Verknüpfung über die *beabsichtigte System* &ndash; klickt der Benutzer auf einen Link in einem mobilen Browser, wird die mobile Browser Priorität, die auf einer registrierten Anwendung Android delegieren, verteilen. Z. B. würde durch Klicken auf einen Link auf einer Website Kochrezepte öffnen Sie eine mobile app, die für die jeweilige Website zugeordnet ist und ein bestimmtes Rezept für den Benutzer anzeigen. Ist es mehr als einer Anwendung registriert, behandeln diese Absicht, Sie so genannten Android löst eine *Dialogfeld zur Klärung* , die fragt eines Benutzers welche Anwendung für die Anwendung auswählen, die für die Absicht behandeln soll Beispiel:

![Bildschirmabbildung von Beispiel eines Dialogfelds zur Klärung](app-linking-images/01-disambiguation-dialog.png)

Android 6.0 verbessert dies durch das automatische Verknüpfung behandeln. Es ist möglich, dass Android automatisch registrieren einer Anwendung als Standardprogramm für einen URI &ndash; die app automatisch gestartet werden und direkt an die entsprechende Aktivität zu navigieren. Wie Android 6.0 entscheidet, einen URI zu behandeln, hängt von den folgenden Kriterien:

1. **Eine vorhandene app ist bereits mit dem URI zugeordneten** &ndash; der Benutzer möglicherweise bereits verknüpft haben eine vorhandene app mit einem URI. In diesem Fall weiterhin Android wird diese Anwendung verwenden.
2. **Keine vorhandenen app mit dem URI zugeordnet ist, aber eine unterstützende app installiert ist** &ndash; In diesem Szenario wird der Benutzer wurde nicht angegeben eine vorhandene app, damit Android der installierten unterstützende Anwendung verwendet werden, um die Anforderung zu verarbeiten.
3. **Keine vorhandenen app mit dem URI zugeordnet ist, aber viele unterstützende apps installiert sind** &ndash; kommen mehrere Anwendungen, die den URI zu unterstützen, wird das Dialogfeld "Mehrdeutigkeit" wird angezeigt, und der Benutzer muss auswählen, welche app wird Behandeln Sie den URI an.

Wenn der Benutzer hat keine apps installiert, die den URI zu unterstützen und eine anschließend installiert ist, werden diese Anwendung der Standardhandler klicken Sie dann Android als festgelegt für den URI nach der Überprüfung der Zuordnung mit der Website, die den URI zugeordnet ist.

Dieses Handbuch wird erläutert, so konfigurieren Sie eine Anwendung für Android 6.0 und zum Erstellen und veröffentlichen Sie die Datei digitale Asset Links zur Unterstützung der app im Android 6.0 verknüpfen.

## <a name="requirements"></a>Anforderungen

Dieses Handbuch erfordert Xamarin.Android 6.1 und eine Anwendung für Android 6.0 (API-Ebene 23) oder höher.

App-Verknüpfung kann in früheren Versionen von Android mithilfe der [Rivets NuGet-Paket](https://www.nuget.org/packages/Rivets/) aus dem Store Xamarin-Komponente. Das Paket Nieten ist nicht kompatibel mit Android 6.0; app-Verknüpfung Es unterstützt Android 6.0 app verknüpfen nicht.

## <a name="configuring-app-linking-in-android-60"></a>Konfigurieren von App-Verknüpfung in Android 6.0

Einrichten von app-Links in Android 6.0 umfasst zwei Hauptschritte:

1. **Hinzufügen von einem oder mehreren Absicht-Filtern für der Website URI** &ndash; die beabsichtigte Filter Richtlinienplan Android wie eine URL in einem mobilen Browser zu behandeln.
2. **Veröffentlichen einer *digitale Asset Links JSON* Datei auf der Website** &ndash; Dies ist eine Datei, die in einer Website hochgeladen und von Android verwendet, um die Beziehung zwischen der mobilen Anwendung und der Domäne der Website zu überprüfen. Ohne diesen Schritt kann nicht Android die app als Standardhandle für URIS zu installieren; der Benutzer muss dies manuell tun.

<a name="configure-intent-filter" />

### <a name="configuring-the-intent-filter"></a>Konfigurieren des Intent-Filters

Es ist notwendig, einen beabsichtigten Filter konfigurieren, der einen URI (oder möglich, einen Satz von URIs) von einer Website einer Aktivität an eine Android-Anwendung zugeordnet ist. In Xamarin.Android, wird diese Beziehung durch Erweitern einer Aktivität mit der [IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/). Der beabsichtigte Filter muss die folgende Informationen deklarieren:

* **`Intent.ActionView`** &ndash; Dadurch wird der beabsichtigten Filter, um die Antwort auf Anforderungen zum Anzeigen von Informationen registriert.
* **`Categories`** &ndash;  Der beabsichtigte Filter sollten beide registrieren  **[Intent.CategoryBrowsable](https://developer.xamarin.com/api/field/Android.Content.Intent.CategoryBrowsable/)**  und  **[Intent.CategoryDefault](https://developer.xamarin.com/api/field/Android.Content.Intent.CategoryDefault/)**  ordnungsgemäß werden sollen die Web-URI zu behandeln.
* **`DataScheme`** &ndash; Der beabsichtigte Filter muss deklarieren `http` und/oder `https`. Hierbei handelt es sich um gültige nur zwei Schemas.
* **`DataHost`** &ndash; Dies ist die Domäne, die die URIs aus hergestellt werden.
* **`DataPathPrefix`** &ndash; Dies ist ein optionaler Pfad auf Ressourcen auf der Website.
* **`AutoVerify`** &ndash; Die `autoVerify` -Attribut weist Android, um zu überprüfen, ob die Beziehung zwischen der Anwendung und der Website. Dadurch wird mehr unten besprochen.

Das folgende Beispiel zeigt, wie Sie die [IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) , behandeln Links aus `https://www.recipe-app.com/recipes` und `http://www.recipe-app.com/recipes`:

```csharp
[IntentFilter(new [] { Intent.ActionView },
              Categories = new[] { Intent.CategoryBrowsable, Intent.CategoryDefault },
              DataScheme = "http",
              DataHost = "recipe-app.com",
              DataPathPrefix = "/recipe"),
              AutoVerify=true]
public class RecipeActivity : Activity
{
    // Code for the activity omitted
}
```

Android überprüft jeden Host, der durch die beabsichtigte Filter für die digitale Ressourcen-Datei auf der Website vor der Registrierung der Anwendung als Standardprogramm für einen URI identifiziert wird. Damit Android die app als Standardhandler herstellen kann, müssen die beabsichtigte Filter Überprüfung bestehen.

### <a name="creating-the-digital-assets-link-file"></a>Erstellen von Digital Assets-Linkdatei

Android-app-Verknüpfung 6.0 erfordert, dass Android überprüfen Sie, ob die Zuordnung zwischen der Anwendung und die Website vor dem Festlegen der Anwendung als Standardprogramm für den URI. Diese Überprüfung erfolgt, wenn zunächst die Anwendung installiert wird. Die *digitale Ressourcen Links* Datei ist eine JSON-Datei, die von der relevanten Webdomain(s) gehostet wird.

> [!NOTE]
> **Hinweis:** der `android:autoVerify` Attribut muss festgelegt werden, durch den beabsichtigten Filter &ndash; andernfalls Android wird die Überprüfung nicht ausführen.

Die Datei wird durch den Webmaster der Domäne an der Position eingefügt **https://domain/.well-known/assetlinks.json**.

Die digitale Asset-Datei enthält die erforderlichen Metadaten für Android aus, um die Zuordnung zu überprüfen. Ein **assetlinks.json** Datei hat die folgenden Schlüssel-Wert-Paare:

* `namespace` &ndash; der Namespace des Android-Anwendung.
* `package_name` &ndash; Der Paketname der Android-Anwendung (im Anwendungsmanifest deklariert).
* `sha256_cert_fingerprints` &ndash; der signierte Anwendung SHA256-Fingerabdrücke. Sie finden Sie im Handbuch [suchen Sie nach Ihrem Schlüsselspeicher MD5 oder SHA1-Signatur](~/android/deploy-test/signing/keystore-signature.md) für Weitere Informationen zum Abrufen der SHA1-Fingerabdruck eines einer Anwendung.

Der folgende Codeausschnitt zeigt ein Beispiel für **assetlinks.json** mit einer einzigen Anwendung aufgeführt:

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

Es ist möglich, um verschiedene Versionen unterstützen mehrere SHA256-Fingerabdruck zu registrieren oder der Anwendung erstellt. Dies als Nächstes **assetlinks.json** Datei ist ein Beispiel für mehrere Anwendungen registrieren:

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

Die [Google digitale Asset Links Website](https://developers.google.com/digital-asset-links/tools/generator) verfügt über ein online-Tool, das Ihnen dabei helfen kann, zu erstellen und testen die Datei Digital Assets.

### <a name="testing-app-links"></a>Testen von App-Links

Nach der Implementierung von app-Verknüpfungen, sollten die verschiedenen Codeteile behandelt getestet werden, um sicherzustellen, dass sie erwartungsgemäß funktionieren.

Es ist möglich, vergewissern Sie sich, dass die Datei Digital Assets ordnungsgemäß formatiert und mithilfe Googles digitale Asset Links API gehostet wird, wie im folgenden Beispiel gezeigt:

```html
https://digitalassetlinks.googleapis.com/v1/statements:list?source.web.site=
  https://<WEB SITE ADDRESS>:&relation=delegate_permission/common.handle_all_urls
```

Es gibt zwei Tests, die ausgeführt werden können, um sicherzustellen, dass die beabsichtigte Filter ordnungsgemäß konfiguriert wurden und die app als der Standardhandler für einen URI festgelegt wird:

1.  Die digitale Medienobjektdatei wird ordnungsgemäß gehostet werden, wie oben beschrieben. Der erste Test wird Priorität verteilen, die für die mobile Anwendung Android umleiten soll. Die Android-Anwendung sollte starten, und zeigen die Aktivität, die für die URL registriert. An einer Eingabeaufforderung Folgendes ein:

    ```shell
    $ adb shell am start -a android.intent.action.VIEW \
        -c android.intent.category.BROWSABLE \
        -d "http://<domain1>/recipe/scalloped-potato"
    ```

2.  Zeigen Sie die vorhandene Verknüpfung, die Behandlung von Richtlinien für die Anwendungen, die auf einem bestimmten Gerät installiert. Der folgende Befehl wird eine Liste von Link-Richtlinien für jeden Benutzer auf dem Gerät mit den folgenden Informationen ausgeben. Geben Sie an der Eingabeaufforderung folgenden Befehl ein:

    ```shell
    $ adb shell dumpsys package domain-preferred-apps
    ```

    * **`Package`** &ndash; Der Paketname der Anwendung.
    * **`Domain`** &ndash; Die Domänen (durch Leerzeichen getrennt), deren Weblinks von der Anwendung behandelt werden
    * **`Status`** &ndash; Dies ist der aktuelle Status der Link-Behandlung für die app. Der Wert **immer** bedeutet, die die Anwendung muss `android:autoVerify=true` deklariert und die systemüberprüfung durchgeführt wurde übergeben. Es folgt eine hexadezimale Zahl, die das Android-System-Datensatz für die Einstellung darstellt.

    Zum Beispiel:

    ```shell
    $ adb shell dumpsys package domain-preferred-apps

    App linkages for user 0:
    Package: com.android.vending
    Domains: play.google.com market.android.com
    Status: always : 200000002
    ```

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch erläutert, wie app-Verknüpfung funktioniert in Android 6.0. Es behandelt dann konfigurieren Sie eine Android 6.0-Anwendung zu unterstützen, und reagieren auf app-Links. Es wurde auch So testen Sie die app-Verknüpfung in einer Android-Anwendung beschrieben.


## <a name="related-links"></a>Verwandte Links

- [Finden der MD5- oder SHA1-Signatur Ihres Keystores](~/android/deploy-test/signing/keystore-signature.md)
- [Aktivitäten und Prioritäten](https://university.xamarin.com/classes#4)
- [AppLinks](http://applinks.org/)
- [Google Digital Assets Links](https://developers.google.com/digital-asset-links/)
- [Anweisung List Generator "und" Tester](https://developers.google.com/digital-asset-links/tools/generator)
