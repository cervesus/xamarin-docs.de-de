---
title: Schriftarten
ms.prod: xamarin
ms.assetid: 3F543FC5-FDED-47F8-8D2C-481FCC98BFDA
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 09/09/2018
ms.openlocfilehash: daeebc4d1531e340b305b810096b72094ab9230d
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84566369"
---
# <a name="fonts"></a>Schriftarten

## <a name="overview"></a>Übersicht

Ab API-Ebene 26 ermöglicht der Android SDK Schriftarten, wie z. b. Layouts oder drawables, als Ressourcen behandelt zu werden. Die [Android-Unterstützungs Bibliothek 26 nuget](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/26.1.0.1) führt einen Backport für die neuen Schriftart-APIs auf die Apps aus, die auf API-Ebene 14 oder höher ausgerichtet sind.

Nach dem Ziel der API 26 oder der Installation der Android-Unterstützungs Bibliothek V26 gibt es zwei Möglichkeiten, Schriftarten in einer Android-Anwendung zu verwenden:

1. **Verpacken der Schriftart als Android-Ressource** &ndash; Dadurch wird sichergestellt, dass die Schriftart für die Anwendung immer verfügbar ist, aber die Größe des APK vergrößert.
2. **Schriftarten** &ndash; herunterladen Android unterstützt auch das Herunterladen einer Schriftart von einem _Schriftart Anbieter_. Der Schriftart Anbieter überprüft, ob die Schriftart bereits auf dem Gerät vorhanden ist. Bei Bedarf wird die Schriftart heruntergeladen und auf dem Gerät zwischengespeichert. Diese Schriftart kann von mehreren Anwendungen gemeinsam genutzt werden.

Ähnliche Schriftarten (oder eine Schriftart, die möglicherweise mehrere unterschiedliche Stile hat) können in _Schriftfamilien_gruppiert werden. Dies ermöglicht es Entwicklern, bestimmte Attribute der Schriftart anzugeben, z. b. die Gewichtung, und Android wählt automatisch die entsprechende Schriftart aus der Schriftfamilie aus.

Die Android-Unterstützungs Bibliothek V26 unterstützt die Unterstützung von Schriftarten auf API-Ebene 26. Wenn Sie auf die älteren API-Ebenen abzielen, ist es erforderlich, den `app` XML-Namespace zu deklarieren und die verschiedenen Schriftart Attribute mit dem `android:` -Namespace und dem- `app:` Namespace zu benennen. Wenn nur der `android:` Namespace verwendet wird, werden die Schriftarten nicht als Geräte mit API-Ebene 25 oder weniger angezeigt. Mit diesem XML-Code Ausschnitt wird beispielsweise eine neue [_Schriftfamilien_](#font_families) Ressource deklariert, die auf API-Ebene 14 und höher funktioniert:

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

     <font  android:font="@font/sourcesanspro_regular"
            android:fontStyle="normal"
            android:fontWeight="400"
            app:font="@font/sourcesanspro_regular"
            app:fontStyle="normal"
            app:fontWeight="400" />

</font-family>
```

Solange Schriftarten für eine Android-Anwendung ordnungsgemäß bereitgestellt werden, können Sie durch Festlegen des- [ `fontFamily` Attributs](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily)auf ein UI-Widget angewendet werden. Der folgende Code Ausschnitt veranschaulicht z. b., wie eine Schriftart in einer TextView angezeigt wird:

```xml
<TextView
    android:text="The quick brown fox jumped over the lazy dog."
    android:fontFamily="@font/sourcesanspro_regular"
    app:fontFamily="@font/sourcesanspro_regular"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

In diesem Leitfaden wird zunächst erläutert, wie Schriftarten als Android-Ressource verwendet werden, und anschließend wird erläutert, wie Schriftarten zur Laufzeit heruntergeladen werden.

## <a name="fonts-as-a-resource"></a>Schriftarten als Ressource

Das Verpacken einer Schriftart in ein Android-APK stellt sicher, dass Sie für die Anwendung immer verfügbar ist. Eine Schriftart Datei (eine-Datei). TTF oder a. Die OTF-Datei) wird einer xamarin. Android-Anwendung wie jede andere Ressource hinzugefügt, indem Dateien in ein Unterverzeichnis im Ordner " **Resources** " eines xamarin. Android-Projekts kopiert werden. Schriftarten Ressourcen werden in einem Unterverzeichnis der **Schriftart** des **Ressourcen** Ordners des Projekts gespeichert.

> [!NOTE]
> Die Schriftarten sollten die **Buildaktion** " **androidresource** " aufweisen, oder Sie werden nicht in das endgültige APK gepackt. Die Buildaktion sollte automatisch von der IDE festgelegt werden.

Wenn viele ähnliche Schriftart Dateien vorhanden sind (z. b. die gleiche Schriftart mit unterschiedlichen Gewichtungen oder Stilen), ist es möglich, Sie in eine Schriftfamilie zu gruppieren.

<a name="font_families"></a>

### <a name="font-families"></a>Schriftfamilien

Eine Schriftfamilie ist ein Satz von Schriftarten, die unterschiedliche Gewichtungen und Stile aufweisen. Beispielsweise können separate Schriftart Dateien für Fett formatierte oder kursiv formatierte Schriftarten vorhanden sein. Die Schriftfamilie wird durch `font` Elemente in einer XML-Datei definiert, die im Verzeichnis **Ressourcen/Schriftart** aufbewahrt wird. Jede Schriftfamilie sollte über eine eigene XML-Datei verfügen.

Fügen Sie zum Erstellen einer Schriftfamilie zuerst alle Schriftarten zum Ordner **Ressourcen/Schriftart** hinzu. Erstellen Sie dann eine neue XML-Datei im Ordner Schriftart für die Schriftfamilie. Der Name der XML-Datei hat keine Affinität oder Beziehung zu den Schriftarten, auf die verwiesen wird. die Ressourcen Datei kann ein beliebiger gültiger Android-Ressourcen Dateiname sein. Diese XML-Datei verfügt über ein root- `font-family` Element, das ein oder mehrere- `font` Elemente enthält. Jedes- `font` Element deklariert die Attribute einer Schriftart.

Der folgende XML-Code ist ein Beispiel für eine Schriftfamilie für die " _Sources Sans Pro_ "-Schriftart, die viele verschiedene Schriftart Gewichtungen definiert. Diese Datei wird als Datei im Ordner " **Resources/Font** " mit dem Namen " **sourcesanspro. XML**" gespeichert:

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family xmlns:android="http://schemas.android.com/apk/res/android"
             xmlns:app="http://schemas.android.com/apk/res-auto">
    <font android:font="@font/sourcesanspro_regular"
          android:fontStyle="normal"
          android:fontWeight="400"
          app:font="@font/sourcesanspro_regular"
          app:fontStyle="normal"
          app:fontWeight="400" />
    <font android:font="@font/sourcesanspro_bold"
          android:fontStyle="normal"
          android:fontWeight="800"
          app:font="@font/sourcesanspro_bold"
          app:fontStyle="normal"
          app:fontWeight="800" />
    <font android:font="@font/sourcesanspro_italic"
          android:fontStyle="italic"
          android:fontWeight="400"
          app:font="@font/sourcesanspro_italic"
          app:fontStyle="italic"
          app:fontWeight="400" />
</font-family>
```

Das- `fontStyle` Attribut verfügt über zwei mögliche Werte:

- **Normal** &ndash; eine normale Schriftart
- **kursiv** &ndash; eine kursiv Schrift

Das `fontWeight` -Attribut entspricht dem CSS `font-weight` -Attribut und bezieht sich auf die Stärke der Schriftart. Dies ist ein Wert im Bereich von 100 bis 900. In der folgenden Liste werden die allgemeinen Schrift Gewichtungswerte und deren Namen beschrieben:

- **Dünn** &ndash; 100
- **Zusätzliches Licht** &ndash; 200
- **Hell** &ndash; 300
- **Normal** &ndash; 400
- **Mittel** &ndash; 500
- **Halbfett** &ndash; 600
- **Fett** &ndash; formatiert 700
- **Extra Fett** &ndash; 800
- **Schwarz** &ndash; 900

Nachdem eine Schriftfamilie definiert wurde, kann Sie deklarativ verwendet werden, indem die `fontFamily` Attribute, `textStyle` und `fontWeight` in der Layoutdatei festgelegt werden.  Der folgende XML-Code Ausschnitt legt z. b. die Schriftart 600 Weight (normal) und einen kursiv Formatierungs Text fest:

```xml
<TextView
    android:text="Sans Source Pro semi-bold italic, 600 weight, italic"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:fontFamily="@font/sourcesanspro"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:gravity="center_horizontal"
    android:fontWeight="600"
    android:textStyle="italic"
    />
```

### <a name="programmatically-assigning-fonts"></a>Programm gesteuertes Zuweisen von Schriftarten

Schriftarten können Programm gesteuert mithilfe der- [`Resources.GetFont`](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int)) Methode zum Abrufen eines-Objekts festgelegt werden [`Typeface`](https://developer.android.com/reference/android/graphics/Typeface.html) . Viele Ansichten verfügen über eine `TypeFace` Eigenschaft, die verwendet werden kann, um die Schriftart dem Widget zuzuweisen. In diesem Code Ausschnitt wird gezeigt, wie die Schriftart Programm gesteuert auf eine TextView festgelegt wird:

```csharp
Android.Graphics.Typeface typeface = this.Resources.GetFont(Resource.Font.caveat_regular);
textView1.Typeface = typeface;
textView1.Text = "Changed the font";
```

Die- `GetFont` Methode lädt automatisch die erste Schriftart innerhalb einer Schriftfamilie.  Verwenden Sie die-Methode, um eine Schriftart zu laden, die mit einem bestimmten Stil übereinstimmt `Typeface.Create` . Diese Methode versucht, eine Schriftart zu laden, die mit dem angegebenen Stil übereinstimmt. In diesem Code Ausschnitt wird beispielsweise versucht, ein Fett `Typeface` formatiertes Objekt aus einer Schriftfamilie zu laden, die in **Ressourcen/Schriftarten**definiert ist:

```csharp
var typeface = Typeface.Create("<FONT FAMILY NAME>", Android.Graphics.TypefaceStyle.Bold);
textView1.Typeface = typeface;
```

## <a name="downloading-fonts"></a>Schriftarten herunterladen

Anstatt Schriftarten als Anwendungs Ressource zu verpacken, kann Android Schriftarten von einer Remote Quelle herunterladen. Dies hat den Vorteil, dass die Größe des APK reduziert wird.

Schriftarten werden mithilfe eines _Schriftart Anbieters_heruntergeladen. Dies ist ein spezieller Inhaltsanbieter, der das herunterladen und Zwischenspeichern von Schriftarten für alle Anwendungen auf dem Gerät verwaltet. Android 8,0 enthält einen Schriftart Anbieter zum Herunterladen von Schriftarten aus dem [Google-schriftrepository](https://fonts.google.com). Dieser Standard Schriftart Anbieter wird mit der Android-Unterstützungs Bibliothek V26 auf API-Ebene 14 zurückportiert.

Wenn eine APP eine Schriftart anfordert, prüft der Schriftart Anbieter zunächst, ob die Schriftart bereits auf dem Gerät vorhanden ist. Wenn dies nicht der Fall ist, wird versucht, die Schriftart herunterzuladen. Wenn die Schriftart nicht heruntergeladen werden kann, verwendet Android die Standard Schriftart des Systems. Nachdem die Schriftart heruntergeladen wurde, ist Sie für alle Anwendungen auf dem Gerät verfügbar, nicht nur für die APP, die die ursprüngliche Anforderung durchgeführt hat.

Wenn eine Anforderung zum Herunterladen einer Schriftart erstellt wird, fragt die APP den Schriftart Anbieter nicht direkt ab. Stattdessen verwenden apps eine Instanz der [`FontsContract`](https://developer.android.com/reference/android/provider/FontsContract.html) API (oder, [`FontsContractCompat`](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html) Wenn die Unterstützungs Bibliothek 26 verwendet wird).  

Android 8,0 unterstützt das Herunterladen von Schriftarten auf zwei verschiedene Arten:

1. **Herunterladbare Schriftarten als Ressource** &ndash; deklarieren Eine APP kann herunterladbare Schriftarten für Android über XML-Ressourcen Dateien deklarieren. Diese Dateien enthalten alle Metadaten, die Android zum asynchronen Herunterladen der Schriftarten benötigt, wenn die APP gestartet und auf dem Gerät zwischengespeichert wird.
2. **Programm gesteuert** &ndash; APIs in Android-API-Ebene 26 ermöglichen es einer Anwendung, die Schriftarten Programm gesteuert herunterzuladen, während die Anwendung ausgeführt wird. Apps erstellen ein `FontRequest` -Objekt für eine bestimmte Schriftart und übergeben dieses Objekt an die- `FontsContract` Klasse. Der `FontsContract` übernimmt `FontRequest` und ruft die Schriftart von einem _Schriftart Anbieter_ab. Android lädt die Schriftart synchron herunter. Ein Beispiel für das Erstellen eines s `FontRequest` finden Sie weiter unten in diesem Handbuch.

Unabhängig davon, welcher Ansatz verwendet wird, werden Ressourcen Dateien hinzugefügt, die der xamarin. Android-Anwendung hinzugefügt werden müssen, bevor Schriftarten heruntergeladen werden können. Zuerst müssen die Schriftart (n) in einer XML-Datei im Verzeichnis " **Resources/Font** " als Teil einer Schriftfamilie deklariert werden. Dieser Code Ausschnitt ist ein Beispiel für das Herunterladen von Schriftarten aus der [Google Fonts Open Source-Sammlung](https://fonts.google.com) mithilfe des standardmäßigen Schriftart Anbieters, der mit Android 8,0 (oder der Support Bibliothek V26) bereit steht:

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family xmlns:android="http://schemas.android.com/apk/res/android"
             xmlns:app="http://schemas.android.com/apk/res-auto"
             android:fontProviderAuthority="com.google.android.gms.fonts"
             android:fontProviderPackage="com.google.android.gms"
             android:fontProviderQuery="VT323"
             android:fontProviderCerts="@array/com_google_android_gms_fonts_certs"
             app:fontProviderAuthority="com.google.android.gms.fonts"
             app:fontProviderPackage="com.google.android.gms"
             app:fontProviderQuery="VT323"
             app:fontProviderCerts="@array/com_google_android_gms_fonts_certs"
>
</font-family>
```

Das- `font-family` Element enthält die folgenden Attribute, die die für Android zum Herunterladen der Schriftarten erforderlichen Informationen deklarieren:

1. **fontproviderauthority** &ndash; Die Autorität des Schriftart Anbieters, der für die Anforderung verwendet werden soll.
2. **fontpackage** &ndash; Das Paket für den Schriftart Anbieter, der für die Anforderung verwendet werden soll. Wird verwendet, um die Identität des Anbieters zu überprüfen.
3. **fontquery** &ndash; Dies ist eine Zeichenfolge, die dem Schriftart Anbieter dabei helfen soll, die angeforderte Schriftart zu finden. Details zur Schriftart Abfrage sind für den Schriftart Anbieter spezifisch. Die- [`QueryBuilder`](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs) Klasse in der [herunterladbaren Schriftarten](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/) -Beispiel-App bietet einige Informationen über das Abfrage Format für Schriftarten aus der Open Source-Sammlung Google Fonts.
4. **fontprovidercerts** &ndash;  Ein Ressourcen Array mit der Liste der Sätze von Hashes für die Zertifikate, mit denen der Anbieter signiert werden soll.

Nachdem die Schriftarten definiert wurden, ist es möglicherweise erforderlich, Informationen über die _Schriftart Zertifikate_ bereitzustellen, die an dem Download beteiligt sind.

### <a name="font-certificates"></a>Schriftart Zertifikate

Wenn der Schriftart Anbieter auf dem Gerät nicht vorinstalliert ist, oder wenn die APP die Bibliothek verwendet `Xamarin.Android.Support.Compat` , benötigt Android die Sicherheitszertifikate des Schriftart Anbieters. Diese Zertifikate werden in einer Array Ressourcen Datei aufgelistet, die im Verzeichnis **Resources/Values** aufbewahrt wird.

Der folgende XML-Code heißt beispielsweise **Resources/Values/fonts_cert. XML** und speichert die Zertifikate für den Google-Schriftart Anbieter:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="com_google_android_gms_fonts_certs">
        <item>@array/com_google_android_gms_fonts_certs_dev</item>
        <item>@array/com_google_android_gms_fonts_certs_prod</item>
    </array>
    <string-array name="com_google_android_gms_fonts_certs_dev">
        <item>
            MIIEqDCCA5CgAwIBAgIJANWFuGx90071MA0GCSqGSIb3DQEBBAUAMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbTAeFw0wODA0MTUyMzM2NTZaFw0zNTA5MDEyMzM2NTZaMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbTCCASAwDQYJKoZIhvcNAQEBBQADggENADCCAQgCggEBANbOLggKv+IxTdGNs8/TGFy0PTP6DHThvbbR24kT9ixcOd9W+EaBPWW+wPPKQmsHxajtWjmQwWfna8mZuSeJS48LIgAZlKkpFeVyxW0qMBujb8X8ETrWy550NaFtI6t9+u7hZeTfHwqNvacKhp1RbE6dBRGWynwMVX8XW8N1+UjFaq6GCJukT4qmpN2afb8sCjUigq0GuMwYXrFVee74bQgLHWGJwPmvmLHC69EH6kWr22ijx4OKXlSIx2xT1AsSHee70w5iDBiK4aph27yH3TxkXy9V89TDdexAcKk/cVHYNnDBapcavl7y0RiQ4biu8ymM8Ga/nmzhRKya6G0cGw8CAQOjgfwwgfkwHQYDVR0OBBYEFI0cxb6VTEM8YYY6FbBMvAPyT+CyMIHJBgNVHSMEgcEwgb6AFI0cxb6VTEM8YYY6FbBMvAPyT+CyoYGapIGXMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbYIJANWFuGx90071MAwGA1UdEwQFMAMBAf8wDQYJKoZIhvcNAQEEBQADggEBABnTDPEF+3iSP0wNfdIjIz1AlnrPzgAIHVvXxunW7SBrDhEglQZBbKJEk5kT0mtKoOD1JMrSu1xuTKEBahWRbqHsXclaXjoBADb0kkjVEJu/Lh5hgYZnOjvlba8Ld7HCKePCVePoTJBdI4fvugnL8TsgK05aIskyY0hKI9L8KfqfGTl1lzOv2KoWD0KWwtAWPoGChZxmQ+nBli+gwYMzM1vAkP+aayLe0a1EQimlOalO762r0GXO0ks+UeXde2Z4e+8S/pf7pITEI/tP+MxJTALw9QUWEv9lKTk+jkbqxbsh8nfBUapfKqYn0eidpwq2AzVp3juYl7//fKnaPhJD9gs=
        </item>
    </string-array>
    <string-array name="com_google_android_gms_fonts_certs_prod">
        <item>
            MIIEQzCCAyugAwIBAgIJAMLgh0ZkSjCNMA0GCSqGSIb3DQEBBAUAMHQxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1Nb3VudGFpbiBWaWV3MRQwEgYDVQQKEwtHb29nbGUgSW5jLjEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDAeFw0wODA4MjEyMzEzMzRaFw0zNjAxMDcyMzEzMzRaMHQxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1Nb3VudGFpbiBWaWV3MRQwEgYDVQQKEwtHb29nbGUgSW5jLjEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDCCASAwDQYJKoZIhvcNAQEBBQADggENADCCAQgCggEBAKtWLgDYO6IIrgqWbxJOKdoR8qtW0I9Y4sypEwPpt1TTcvZApxsdyxMJZ2JORland2qSGT2y5b+3JKkedxiLDmpHpDsz2WCbdxgxRczfey5YZnTJ4VZbH0xqWVW/8lGmPav5xVwnIiJS6HXk+BVKZF+JcWjAsb/GEuq/eFdpuzSqeYTcfi6idkyugwfYwXFU1+5fZKUaRKYCwkkFQVfcAs1fXA5V+++FGfvjJ/CxURaSxaBvGdGDhfXE28LWuT9ozCl5xw4Yq5OGazvV24mZVSoOO0yZ31j7kYvtwYK6NeADwbSxDdJEqO4k//0zOHKrUiGYXtqw/A0LFFtqoZKFjnkCAQOjgdkwgdYwHQYDVR0OBBYEFMd9jMIhF1Ylmn/Tgt9r45jk14alMIGmBgNVHSMEgZ4wgZuAFMd9jMIhF1Ylmn/Tgt9r45jk14aloXikdjB0MQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEUMBIGA1UEChMLR29vZ2xlIEluYy4xEDAOBgNVBAsTB0FuZHJvaWQxEDAOBgNVBAMTB0FuZHJvaWSCCQDC4IdGZEowjTAMBgNVHRMEBTADAQH/MA0GCSqGSIb3DQEBBAUAA4IBAQBt0lLO74UwLDYKqs6Tm8/yzKkEu116FmH4rkaymUIE0P9KaMftGlMexFlaYjzmB2OxZyl6euNXEsQH8gjwyxCUKRJNexBiGcCEyj6z+a1fuHHvkiaai+KL8W1EyNmgjmyy8AW7P+LLlkR+ho5zEHatRbM/YAnqGcFh5iZBqpknHf1SKMXFh4dd239FJ1jWYfbMDMy3NS5CTMQ2XFI1MvcyUTdZPErjQfTbQe3aDQsQcafEQPD+nqActifKZ0Np0IS9L9kR/wbNvyz6ENwPiTrjV2KRkEjH78ZMcUQXg0L3BYHJ3lc69Vs5Ddf9uUGGMYldX3WfMBEmh/9iFBDAaTCK
        </item>
    </string-array>
</resources>
```

Wenn diese Ressourcen Dateien vorhanden sind, kann die APP die Schriftarten herunterladen.

### <a name="declaring-downloadable-fonts-as-resources"></a>Deklarieren von Schriftarten als Ressourcen

Wenn Sie die herunterladbaren Schriftarten in " **androidmanifest. XML**" auflisten, werden die Schriftarten von Android beim ersten Start der APP asynchron heruntergeladen. Die Schriftart der Schriftart ist in einer Array Ressourcen Datei aufgeführt, die der folgenden ähnelt:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="downloadable_fonts" translatable="false">
        <item>@font/vt323</item>
    </array>
</resources>
```

Um diese Schriftarten herunterzuladen, müssen Sie in " **androidmanifest. XML** " deklariert werden, indem Sie `meta-data` als untergeordnetes Element des-Elements hinzugefügt werden `application` . Wenn z. b. die herunterladbaren Schriftarten in einer Ressourcen Datei unter " **Resources/Values/downloadable_fonts. XML**" deklariert sind, müsste dieser Code Ausschnitt dem Manifest hinzugefügt werden:

```xml
<meta-data android:name="downloadable_fonts" android:resource="@array/downloadable_fonts" />
```

### <a name="downloading-a-font-with-the-font-apis"></a>Herunterladen einer Schriftart mit den Schriftart-APIs

Es ist möglich, eine Schriftart Programm gesteuert herunterzuladen, indem ein-Objekt instanziiert [`FontRequest`](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html) und an die-Methode übergeben wird `FontContractCompat.RequestFont` . Die `FontContractCompat.RequestFont` Methode prüft zunächst, ob die Schriftart auf dem Gerät vorhanden ist, und dann, falls erforderlich, den Schriftart Anbieter asynchron abfragt und versucht, die Schriftart für die APP herunterzuladen. Wenn `FontRequest` die Schriftart nicht herunterladen kann, verwendet Android die standardmäßige System Schriftart.

Ein- `FontRequest` Objekt enthält Informationen, die vom Schriftart Anbieter verwendet werden, um eine Schriftart zu suchen und herunterzuladen. Ein `FontRequest` benötigt vier Informationen:

1. **Schriftart Anbieter Autorität** &ndash; Die Autorität des Schriftart Anbieters, der für die Anforderung verwendet werden soll.
2. **Schriftart Paket** &ndash; Das Paket für den Schriftart Anbieter, der für die Anforderung verwendet werden soll. Wird verwendet, um die Identität des Anbieters zu überprüfen.
3. **Schriftart Abfrage** &ndash; Dies ist eine Zeichenfolge, die dem Schriftart Anbieter dabei helfen soll, die angeforderte Schriftart zu finden. Details zur Schriftart Abfrage sind für den Schriftart Anbieter spezifisch. Die Details der Zeichenfolge sind spezifisch für den Schriftart Anbieter. Die- [`QueryBuilder`](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs) Klasse in der [herunterladbaren Schriftarten](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/) -Beispiel-App bietet einige Informationen über das Abfrage Format für Schriftarten aus der Open Source-Sammlung Google Fonts.
4. **Schriftart Anbieter Zertifikate** &ndash;  Ein Ressourcen Array mit der Liste der Sätze von Hashes für die Zertifikate, mit denen der Anbieter signiert werden soll.

Dieser Code Ausschnitt ist ein Beispiel für die Instanziierung eines neuen- `FontRequest` Objekts:

```csharp
FontRequest request = new FontRequest("com.google.android.gms.fonts", "com.google.android.gms", <FontToDownload>, Resource.Array.com_google_android_gms_fonts_certs);
```

Im vorherigen Code Ausschnitt `FontToDownload` finden Sie eine Abfrage, mit der die Schriftart aus der Google Fonts Open Source-Auflistung unterstützt wird.

Bevor der `FontRequest` an die- `FontContractCompat.RequestFont` Methode übergeben wird, müssen zwei-Objekte erstellt werden:

- **`FontsContractCompat.FontRequestCallback`**&ndash;Dies ist eine abstrakte Klasse, die erweitert werden muss. Es handelt sich um einen Rückruf, der aufgerufen wird, wenn `RequestFont` abgeschlossen ist. Eine xamarin. Android-App muss eine Unterklasse aufweisen `FontsContractCompat.FontRequestCallback` und die `OnTypefaceRequestFailed` und überschreiben und `OnTypefaceRetrieved` die Aktionen bereitstellen, die durchgeführt werden, wenn der Download fehlschlägt oder erfolgreich ist.
- **`Handler`**&ndash;Dies ist eine, `Handler` die von verwendet wird `RequestFont` , um die Schriftart bei Bedarf in einen Thread herunterzuladen. Schriftarten sollten **nicht** im UI-Thread heruntergeladen werden.

Dieser Code Ausschnitt ist ein Beispiel für eine c#-Klasse, die eine Schriftart asynchron aus der Google Fonts Open Source-Sammlung herunterlädt. Es implementiert die `FontRequestCallback` -Schnittstelle und löst ein c#-Ereignis aus, wenn `FontRequest` beendet wurde.

```csharp
public class FontDownloadHelper : FontsContractCompat.FontRequestCallback
{
    // A very simple font query; replace as necessary
    public static readonly String FontToDownload = "Courgette";

    Android.OS.Handler Handler = null;

    public event EventHandler<FontDownloadEventArg> FontDownloaded = delegate
    {
        // just an empty delegate to avoid null reference exceptions.  
    };

    public void DownloadFonts(Context context)
    {
        FontRequest request = new FontRequest("com.google.android.gms.fonts", "com.google.android.gms",FontToDownload , Resource.Array.com_google_android_gms_fonts_certs);
        FontsContractCompat.RequestFont(context, request, this, GetHandlerThreadHandler());
    }

    public override void OnTypefaceRequestFailed(int reason)
    {
        base.OnTypefaceRequestFailed(reason);
        FontDownloaded(this, new FontDownloadEventArg(null));
    }

    public override void OnTypefaceRetrieved(Android.Graphics.Typeface typeface)
    {
        base.OnTypefaceRetrieved(typeface);
        FontDownloaded(this, new FontDownloadEventArg(typeface));
    }

    Handler GetHandlerThreadHandler()
    {
        if (Handler == null)
        {
            HandlerThread handlerThread = new HandlerThread("fonts");
            handlerThread.Start();
            Handler = new Handler(handlerThread.Looper);
        }
        return Handler;
    }
}

public class FontDownloadEventArg : EventArgs
{
    public FontDownloadEventArg(Android.Graphics.Typeface typeface)
    {
        Typeface = typeface;
    }
    public Android.Graphics.Typeface Typeface { get; private set; }
    public bool RequestFailed
    {
        get
        {
            return Typeface != null;
        }
    }
}
```

Um dieses Hilfsprogramm zu verwenden, `FontDownloadHelper` wird ein neuer erstellt, und es wird ein Ereignishandler zugewiesen:  

```csharp
var fontHelper = new FontDownloadHelper();

fontHelper.FontDownloaded += (object sender, FontDownloadEventArg e) =>
{
    //React to the request
};
fontHelper.DownloadFonts(this); // this is an Android Context instance.
```

## <a name="summary"></a>Zusammenfassung

In diesem Handbuch wurden die neuen APIs in Android 8,0 erläutert, um das Herunterladen von Schriftarten und Schriftarten als Ressourcen Es wurde erläutert, wie vorhandene Schriftarten in ein APK eingebettet und in einem Layout verwendet werden. Außerdem wird erläutert, wie Android 8,0 das Herunterladen von Schriftarten von einem Schriftart Anbieter unterstützt, entweder Programm gesteuert oder durch Deklarieren der Schriftart Metadaten in Ressourcen Dateien.

## <a name="related-links"></a>Verwandte Links

- [FontFamily](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily)
- [Fontconfig](https://developer.android.com/reference/android/text/FontConfig.html)
- [Fontrequest](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html)
- [Fontkontratcompat](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html)
- [Resources. GetFont](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int))
- [Schriftart](https://developer.android.com/reference/android/graphics/Typeface.html)
- [Android-Unterstützungs Bibliothek 26 nuget](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/)
- [Verwenden von Schriftarten in Android](https://www.youtube.com/watch?v=TfB-TsLFJdM)
- [CSS-Schriftart Gewichtungs Spezifikation](https://www.w3.org/TR/css-fonts-3/#font-weight-numeric-values)
- [Google Fonts Open Source-Sammlung](https://fonts.google.com/)
- [Quelle Sans Pro](https://fonts.google.com/specimen/Source+Sans+Pro)
