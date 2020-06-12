---
title: App-Linking in Android
description: In diesem Leitfaden wird erläutert, wie Android 6.0 App-Linking unterstützt. Dies ist eine Technik, mit der mobile Apps auf URLs auf Websites reagieren können. Es wird erläutert, was App-Linking ist, wie App-Linking in einer Android 6.0-Anwendung implementiert wird und wie eine Website so konfiguriert wird, dass der mobilen App Berechtigungen für eine Domäne erteilt werden.
ms.prod: xamarin
ms.assetid: 48174E39-19FD-43BC-B54C-9AF11D4B1F91
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: bdcefd6a1b0192dc337afd5b5a5535a20eeaef9e
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571388"
---
# <a name="app-linking-in-android"></a>App-Linking in Android

_In diesem Leitfaden wird erläutert, wie Android 6.0 App-Linking unterstützt. Dies ist eine Technik, mit der mobile Apps auf URLs auf Websites reagieren können. Es wird erläutert, was App-Linking ist, wie App-Linking in einer Android 6.0-Anwendung implementiert wird und wie eine Website so konfiguriert wird, dass der mobilen App Berechtigungen für eine Domäne erteilt werden._

## <a name="app-linking-overview"></a>Übersicht über App-Linking

Mobile Apps befinden sich nicht mehr in einem Silo &ndash; in vielen Fällen sind sie wichtige Komponenten der jeweiligen Unternehmen, zusammen mit deren Website. Für Unternehmen ist es wünschenswert, ihre Webpräsenz und mobilen Apps nahtlos zu verbinden, wobei Links auf einer Website mobile Apps starten und relevante Inhalte in der jeweiligen mobilen App anzeigen. *App-Linking* (auch als *Deep-Linking* bezeichnet) ist ein Verfahren, das es einem mobilen Gerät ermöglicht, auf einen URI zu reagieren und eine mobile App zu starten, die diesem URI entspricht.

Android verarbeitet App-Linking über das *Intent-System* &ndash; wenn der Benutzer auf einen Link in einem mobilen Browser klickt, sendet der mobile Browser eine Absicht (Intent), die Android an eine registrierte Anwendung delegiert. Wenn Sie z. B. auf einen Link auf einer Kochwebsite klicken, wird eine mobile App geöffnet, die dieser Website zugeordnet ist, und in der App wird ein bestimmtes Rezept angezeigt. Sind mehrere Apps registriert, die diese Absicht (Intent) verarbeiten können, wird von Android ein sogenannter *Vereindeutigungsdialog* ausgelöst, in dem der Benutzer aufgefordert wird, die Anwendung auszuwählen, von der die Absicht verarbeitet werden soll. Beispiel:

![Beispielscreenshot eines Vereindeutigungsdialogs](app-linking-images/01-disambiguation-dialog.png)

Android 6.0 verbessert dies durch Verwenden von automatischer Linkverarbeitung. Android kann eine App automatisch als Standardhandler für einen URI registrieren &ndash; die App wird automatisch gestartet und navigiert direkt zu der relevanten Aktivität. Wie Android 6.0 festlegt, wie ein URI verarbeitet wird, hängt von den folgenden Kriterien ab:

1. **Eine vorhandene APP ist bereits mit dem URI verknüpft.** &ndash; Der Benutzer hat möglicherweise bereits eine vorhandene App mit einem URI verknüpft. In diesem Fall verwendet Android diese App weiterhin.
2. **Mit dem URI ist keine vorhandene App verknüpft, aber es ist eine unterstützende App installiert.** &ndash; In diesem Szenario hat der Benutzer keine vorhandene App angegeben, also verwendet Android die installierte unterstützende App, um die Anforderung zu verarbeiten.
3. **Mit dem URI ist keine vorhandene App verknüpft, aber es sind viele unterstützende Apps installiert.** &ndash; Da mehrere Anwendungen vorhanden sind, die den URI unterstützen, wird der Vereindeutigungsdialog angezeigt, und der Benutzer muss auswählen, welche App den URI verarbeiten soll.

Hat der Benutzer keine Apps installiert sind, die den URI unterstützen, und wird später eine solche installiert, legt Android diese App als Standardhandler für den URI fest, nachdem die Verknüpfung mit der Website überprüft wurde, die mit dem URI verknüpft ist.

In diesem Leitfaden wird erläutert, wie eine Android 6.0-App konfiguriert wird und wie die Digital Asset Links-Datei erstellt und veröffentlicht wird, um App-Linking in Android 6.0 zu unterstützen.

## <a name="requirements"></a>Anforderungen

Für diesen Leitfaden sind Xamarin.Android 6.1 und eine App erforderlich, die für Android 6.0 (API-Ebene 23) oder höher ausgelegt ist.

App-Linking ist in früheren Versionen von Android möglich, indem das [Rivets NuGet-Paket](https://www.nuget.org/packages/Rivets/) aus dem Xamarin Component-Store verwendet wird. Das Rivets-Paket ist nicht mit App-Linking in Android 6.0 kompatibel, denn es unterstützt Android 6.0-App-Linking nicht.

## <a name="configuring-app-linking-in-android-60"></a>Konfigurieren von App-Linking in Android 6.0

Das Einrichten von App-Links in Android 6.0 besteht aus zwei Hauptschritten:

1. **Hinzufügen von Intent-Filtern für den URI der Website** &ndash; Über die Intent-Filter wird Android angewiesen, wie ein URL-Klick in einem mobilen Browser verarbeitet werden soll.
2. **Veröffentlichen einer *Digital Asset Links-JSON*-Datei auf der Website** &ndash; Dies ist eine Datei, die auf eine Website hochgeladen und von Android verwendet wird, um die Beziehung zwischen der mobilen App und der Domäne der Website zu überprüfen. Ohne diese Datei kann Android die App nicht als Standardhandler für den URI installieren. Der Benutzer muss dies manuell vornehmen.

<a name="configure-intent-filter"></a>

### <a name="configuring-the-intent-filter"></a>Konfigurieren des Intent-Filters

Es ist erforderlich, einen Intent-Filter zu konfigurieren, der einen URI (oder eventuell einen Satz von URIs) von einer Website zu einer Aktivität in einer Android-App zuordnet. In Xamarin.Android wird diese Beziehung eingerichtet, indem eine Aktivität mit dem [IntentFilterAttribute](xref:Android.App.IntentFilterAttribute) versehen wird. Im Intent-Filter müssen die folgenden Informationen deklariert sein:

- **`Intent.ActionView`** &ndash; Hiermit wird der Intent-Filter registriert, um auf Anforderungen zum Anzeigen von Informationen zu reagieren.
- **`Categories`** &ndash; Der Intent-Filter sollte sowohl **[Intent.CategoryBrowsable](xref:Android.Content.Intent.CategoryBrowsable)** als auch **[Intent.CategoryDefault](xref:Android.Content.Intent.CategoryDefault)** registrieren, damit der Web-URI ordnungsgemäß verarbeitet werden kann.
- **`DataScheme`** &ndash; Der Intent-Filter muss `http` und/oder `https` deklarieren. Diese beiden Schemas sind die einzigen gültigen Schemas.
- **`DataHost`** &ndash; Dies ist die Domäne, aus der die URIs stammen.
- **`DataPathPrefix`** &ndash; Dies ist ein optionaler Pfad zu Ressourcen auf der Website.
- **`AutoVerify`** &ndash; Das `autoVerify`-Attribut weist Android an, die Beziehung zwischen der App und der Website zu überprüfen. Dies ist weiter unten ausführlicher erläutert.

Im folgenden Beispiel ist gezeigt, wie [IntentFilterAttribute](xref:Android.App.IntentFilterAttribute) verwendet wird, um Links von `https://www.recipe-app.com/recipes` und von `http://www.recipe-app.com/recipes` zu verarbeiten:

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

Android überprüft jeden Host, der durch die Intent-Filter erkannt wurde, gegen die Digital Asset Links-Datei auf der Website, bevor die App als Standardhandler für einen URI registriert wird. Alle Intent-Filter müssen die Überprüfung bestehen, bevor Android die App als Standardhandler einrichten kann.

### <a name="creating-the-digital-assets-link-file"></a>Erstellen der Digital Assets Link-Datei

Android 6.0-App-Linking erfordert, dass Android die Zuordnung zwischen der App und der Website überprüft, bevor die App als Standardhandler für den URI festgelegt wird. Diese Überprüfung erfolgt, wenn die App erstmalig installiert wird. Die *Digital Assets Links*-Datei ist eine JSON-Datei, die von den entsprechenden Webdomänen gehostet wird.

> [!NOTE]
> Das `android:autoVerify`-Attribut muss vom Intent-Filter festgelegt werden &ndash; andernfalls führt Android die Überprüfung nicht aus.

Die Datei wird vom Webmaster der Domäne im Speicherort **https://domain/.well-known/assetlinks.json** platziert.

Die Digital Assets Link-Datei enthält die Metadaten, die für Android erforderlich sind, um die Zuordnung zu überprüfen. Eine **assetlinks.json**-Datei enthält die folgenden Schlüssel-Wert-Paare:

- `namespace` &ndash; der Namespace der Android-Anwendung.
- `package_name` &ndash; der Paketname der Android-Anwendung (deklariert im App-Manifest).
- `sha256_cert_fingerprints` &ndash; die SHA256-Fingerabdrücke der signierten App. Weitere Informationen dazu, wie der SHA1-Fingerabdruck einer App beschafft wird, finden Sie im Leitfaden [Ermitteln Ihrer Keystoresignatur](~/android/deploy-test/signing/keystore-signature.md).

Der folgende Codeausschnitt ist ein Beispiel für **assetlinks.json** mit einer einzigen aufgelisteten App:

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

Es ist möglich, mehrere SHA256-Fingerabdrücke zu registrieren, um unterschiedliche Versionen oder Builds Ihrer App zu unterstützen. Die nächste **assetlinks.json**-Datei ist ein Beispiel für die Registrierung mehrerer Apps:

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

Auf der [Google Digital Asset Links-Website](https://developers.google.com/digital-asset-links/tools/generator) wird ein Onlinetool bereitgestellt, das beim Erstellen und Testen der Digital Asset Links-Datei hilfreich sein kann.

### <a name="testing-app-links"></a>Testen von App-Links

Nachdem Sie App-Links implementiert haben, sollten die verschiedenen Bestandteile getestet werden, um sicherzustellen, dass sie erwartungsgemäß funktionieren.

Durch Verwenden von Googles Digital Asset Links-API lässt sich bestätigen, dass die Digital Asset Links-Datei ordnungsgemäß formatiert ist und gehostet wird. Dies ist im folgenden Beispiel gezeigt:

```html
https://digitalassetlinks.googleapis.com/v1/statements:list?source.web.site=
  https://<WEB SITE ADDRESS>:&relation=delegate_permission/common.handle_all_urls
```

Es können zwei Tests vorgenommen werden, um sich zu vergewissern, dass die Intent-Filter ordnungsgemäß konfiguriert sind und dass die App als Standardhandler für einen URI festgelegt ist:

1. Die Digital Asset Links-Datei wird, wie oben beschrieben, ordnungsgemäß gehostet. Im ersten Test wird eine Absicht (Intent) gesendet, die von Android an die mobile App weitergeleitet werden soll. Die Android-App sollte die Aktivität starten und anzeigen, die für die URL registriert ist. Geben Sie an einer Eingabeaufforderung Folgendes ein:

    ```shell
    $ adb shell am start -a android.intent.action.VIEW \
        -c android.intent.category.BROWSABLE \
        -d "http://<domain1>/recipe/scalloped-potato"
    ```

2. Zeigen Sie die vorhandenen Linkverarbeitungsrichtlinien für die Apps an, die auf einem bestimmten Gerät installiert sind. Mit dem folgenden Befehl wird eine Liste der Linkrichtlinien für jeden Benutzer auf dem Gerät mit den folgenden Informationen erstellt. Geben Sie an der Eingabeaufforderung folgenden Befehl ein:

    ```shell
    $ adb shell dumpsys package domain-preferred-apps
    ```

    - **`Package`** &ndash; Der Paketname der App.
    - **`Domain`** &ndash; Die Domänen (durch Leerzeichen getrennt), deren Weblinks von der App verarbeitet werden.
    - **`Status`** &ndash; Dies ist der aktuelle Linkverarbeitungsstatus für die App. Der Wert **always** bedeutet, dass die App mit `android:autoVerify=true` deklariert ist und die Systemüberprüfung bestanden hat. Darauf folgt eine hexadezimale Zahl, die den Eintrag der Voreinstellung des Android-Systems darstellt.

    Zum Beispiel:

    ```shell
    $ adb shell dumpsys package domain-preferred-apps

    App linkages for user 0:
    Package: com.android.vending
    Domains: play.google.com market.android.com
    Status: always : 200000002
    ```

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurde erläutert, wie App-Linking in Android 6.0 funktioniert. Anschließend wurde dargestellt, wie eine Android 6.0-App so konfiguriert wird, dass sie App-Links unterstützt und auf diese reagiert. Schließlich wurde erläutert, wie App-Linking in einer Android-App getestet wird.

## <a name="related-links"></a>Verwandte Links

- [Finden der MD5- oder SHA1-Signatur Ihres Keystores](~/android/deploy-test/signing/keystore-signature.md)
- [AppLinks](https://developers.facebook.com/docs/applinks)
- [Google Digital Assets Links](https://developers.google.com/digital-asset-links/)
- [Statement List Generator and Tester](https://developers.google.com/digital-asset-links/tools/generator)
