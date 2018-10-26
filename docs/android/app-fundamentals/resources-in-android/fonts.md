---
title: Schriftarten
ms.prod: xamarin
ms.assetid: 3F543FC5-FDED-47F8-8D2C-481FCC98BFDA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/0/2018
ms.openlocfilehash: fb0e1c42efc6bb51dcd899c7e6017007a82d5c7f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105934"
---
# <a name="fonts"></a>Schriftarten

## <a name="overview"></a>Übersicht

API-Ebene 26 ab, das Android SDK können Schriftarten als Ressourcen, wie eine Layouts behandelt werden sollen oder zeichenbarer Ressourcen. Die [Android Support Library 26 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/26.1.0.1) wird Backport die neue Schriftart-APIs, die apps, API-Ebene 14 oder höher ausgerichtet, sind.

Nachdem abzielt API 26 oder installieren die Android Support-Bibliothek v26 gibt es zwei Möglichkeiten zur Verwendung von Schriftarten in einer Android-Anwendung:

1. **Packen Sie die Schriftart als ein Android-Ressourcen** &ndash; Dadurch wird sichergestellt, dass die Schriftart immer der Anwendung zur Verfügung ist, aber die Größe des APK erhöhen. 
2. **Laden Sie die Schriftarten** &ndash; Android unterstützt auch eine Schriftart aus Herunterladen einer _Schriftart Anbieter_. Der Anbieter für die Schriftart wird überprüft, ob die Schriftart bereits auf dem Gerät vorhanden ist. Bei Bedarf wird die Schriftart heruntergeladen und auf dem Gerät zwischengespeichert werden. Diese Schriftart kann zwischen mehreren Anwendungen freigegeben werden.

Ähnlich wie Schriftarten (oder eine Schriftart, die möglicherweise mehrere verschiedene Formate haben) in gruppiert werden _Schriftfamilien_. Dadurch können Entwickler bestimmte Attribute der Schriftart, z. B. dessen Gewichtung angeben und Android wählt automatisch die passende Schriftart aus der Schriftfamilie.

Die Bibliothek für Android unterstützt v26 wird Backport-Unterstützung für Schriftarten, API-Ebene 26. Wenn Ihr Ziel ist die ältere API-Ebenen, ist es erforderlich, deklarieren die `app` XML-Namespace und nennen Sie die verschiedenen Schriftartattribute mithilfe der `android:` Namespace und die `app:` Namespace. Wenn nur die `android:` Namespace wird verwendet, dann ist die Schriftarten nicht angezeigten-Geräte-API-Ebene 25 oder weniger. Diese XML-Ausschnitt deklariert beispielsweise eine neue [ _Schriftfamilie_ ](#font_families) Ressource, die in API-Ebene 14 und höher funktioniert:

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

Solange Schriftarten einer Android-Anwendung ordnungsgemäß bereitgestellt werden, sie können angewendet werden auf ein UI-Widgets durch Festlegen der [ `fontFamily` Attribut](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily). Der folgende Codeausschnitt zeigt beispielsweise, wie Sie eine Schriftart in eine TextView anzeigen:

```xml
<TextView
    android:text="The quick brown fox jumped over the lazy dog."
    android:fontFamily="@font/sourcesanspro_regular"
    app:fontFamily="@font/sourcesanspro_regular"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

Dieses Handbuch wird zunächst erläutert, wie Schriftarten als ein Android-Ressourcen verwenden, und fahren Sie mit, wie Schriftarten zur Laufzeit geladen werden.

## <a name="fonts-as-a-resource"></a>Schriftarten als Ressource

Verpacken eine Schriftart in ein Android APK wird sichergestellt, dass es immer für die Anwendung verfügbar ist. Eine Schriftartdatei (entweder ein. TTF oder ein. OTF-Datei) wird an eine Xamarin.Android-Anwendung wie jede andere Ressource hinzugefügt, durch Kopieren von Dateien in ein Unterverzeichnis im Verzeichnis der **Ressourcen** Ordner eines Xamarin.Android-Projekts. Schriftarten Ressourcen befinden sich einem **Schriftart** Unterverzeichnis von der **Ressourcen** -Ordner des Projekts. 

> [!NOTE]
> Die Schriftarten müssen eine **Buildvorgang** von **AndroidResource** oder wird nicht in der finalen Android-Anwendungspakets verpackt werden. Der Buildvorgang sollte automatisch von der IDE festgelegt werden.

Wenn es viele ähnliche Schriftartdateien (z. B. mit dem gleichen Schriftgrad mit verschiedenen Gewichtungen oder Stile sind) ist es möglich, diese in eine Schriftfamilie gruppieren.

<a name="font_families" />

### <a name="font-families"></a>Schriftartfamilien

Eine Schriftfamilie ist eine Reihe von Schriftarten, die unterschiedliche Gewichtungen und Stile. Möglicherweise gibt es z. B. separate Schriftartdateien für fett oder kursiv Schriftarten. Die Schriftfamilie wird definiert, indem `font` Elemente in einer XML-Datei, die gehalten wird, in der **Ressourcen/Schriftart** Verzeichnis. Jede Schriftfamilie müssen eigene XML-Datei.

Um eine Schriftfamilie zu erstellen, fügen Sie zuerst alle Schriftarten auf die **Ressourcen/Schriftart** Ordner. Erstellen Sie eine XML-Datei im Ordner "Schriftart" für die Schriftfamilie. Der Namen des XML-Datei enthält keine Affinität oder die Beziehung, um die Schriftarten, die auf die verwiesen wird. die Ressourcendatei kann einen beliebigen Dateinamen zulässige Android-Ressourcen sein. Diese XML-Datei müssen einen Stamm `font-family` Element, das eine oder mehrere enthält `font` Elemente. Jede `font` Element deklariert die Attribute einer Schriftart.

Das folgende XML ist ein Beispiel für eine Schriftfamilie für den _Quellen Sans Pro_ Schriftart, die viele verschiedene Schriftbreiten definiert. Dies wird als Datei gespeichert der **Ressourcen/Schriftart** Ordner mit dem Namen **sourcesanspro.xml**:

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

Die `fontStyle` Attribut besitzt zwei mögliche Werte:

* **normale** &ndash; ein normaler Schriftart
* **Kursiv** &ndash; einen kursiven Schriftschnitt

Die `fontWeight` Attribut entspricht, zu dem CSS `font-weight` Attribut, und bezieht sich auf die Stärke der Schriftart. Dies ist ein Wert im Bereich von 100 bis 900. Die folgende Liste beschreibt allgemeine Gewicht Schriftartwerte und deren Namen:

* **Schlanke** &ndash; 100
* **Sehr hell** &ndash; 200
* **Light** &ndash; 300
* **Normal** &ndash; 400
* **Mittel** &ndash; 500
* **Semi-Bold** &ndash; 600
* **Fett** &ndash; 700
* **Sehr fett** &ndash; 800
* **Schwarz** &ndash; 900

Nachdem Sie eine Schriftfamilie definiert wurde, kann verwendet werden deklarativ durch Festlegen der `fontFamily`, `textStyle`, und `fontWeight` Attribute in der Layoutdatei.  Der folgende XML-Codeausschnitt legt z. eine Gewichtung von 600-Schriftart (normalen) und ein kursiver Text-Format:

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

### <a name="programmatically-assigning-fonts"></a>Programmgesteuert Zuweisen von Schriftarten

Schriftarten können programmgesteuert festgelegt werden, mithilfe der [ `Resources.GetFont` ](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int)) Methode zum Abrufen einer [ `Typeface` ](https://developer.android.com/reference/android/graphics/Typeface.html) Objekt. Viele Ansichten verfügen über eine `TypeFace` -Eigenschaft, die verwendet werden kann, das Widget die Schriftart zuweisen. Dieser Codeausschnitt zeigt, wie die Schriftart eine TextView programmgesteuert festgelegt wird:

```csharp
Android.Graphics.Typeface typeface = this.Resources.GetFont(Resource.Font.caveat_regular);
textView1.Typeface = typeface;
textView1.Text = "Changed the font";
```

Die `GetFont` Methode lädt automatisch die erste Schriftart in eine Schriftfamilie.  Verwenden Sie zum Laden einer Schriftart, die ein bestimmtes Format entspricht dem `Typeface.Create` Methode. Diese Methode versucht, eine Schriftart zu laden, die im angegebene Format entspricht. Als Beispiel dieser Ausschnitt versucht, eine fett laden `Typeface` Objekt aus einer Schriftfamilie, die in definierten **Ressourcen/Schriftarten**:

```csharp
var typeface = Typeface.Create("<FONT FAMILY NAME>", Android.Graphics.TypefaceStyle.Bold);
textView1.Typeface = typeface;
```

## <a name="downloading-fonts"></a>Herunterladen von Schriftarten

Verpacken von Schriftarten als Anwendungsressource anstelle von kann Android Schriftarten aus einer Remotequelle herunterladen. Dies ist die wünschenswerte Auswirkungen Verringern der Größe des APK haben. 

Schriftarten werden heruntergeladen, mit der Hilfe von einem _Schriftart Anbieter_. Dies ist eine spezielle Inhaltsanbieter, der das Herunterladen und Zwischenspeichern von Schriftarten für alle Anwendungen auf dem Gerät verwaltet. Android 8.0 enthält einen Schriftart-Anbieter zum Herunterladen von Schriftarten aus dem [Google Schriftart Repository](http://fonts.google.com). Diese Standardanbieter für die Schriftart wird bereitgestellt, um API-Ebene 14 mit der v26 Android Support-Bibliothek.

Wenn eine app für eine Schriftart anfordert, prüft der Schriftart-Anbieter zunächst festzustellen, ob die Schriftart bereits auf dem Gerät vorhanden ist. Wenn dies nicht der Fall ist, es wird versucht, die Schriftart zu laden. Wenn die Schriftart nicht heruntergeladen, klicken Sie dann für Android werden verwenden die Standardschriftart des Systems. Wenn die Schriftart heruntergeladen wurde, ist es für alle Anwendungen auf dem Gerät, nicht nur die app, die die ursprüngliche Anforderung verfügbar.

Wenn eine Anforderung zum Herunterladen einer Schriftart erfolgt, wird die app nicht direkt den Schriftart-Anbieter abgefragt werden. Stattdessen eine Instanz von apps verwenden die [ `FontsContract` ](https://developer.android.com/reference/android/provider/FontsContract.html) API (oder die [ `FontsContractCompat` ](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html) Wenn 26-Support-Bibliothek verwendet wird).  

Android 8.0 unterstützt das Herunterladen von Schriftarten auf zwei Arten:

1. **Herunterladbare Schriftarten als Ressource deklarieren** &ndash; eine app kann zum Herunterladen von Schriftarten für Android über XML-Ressourcendateien deklarieren. Diese Dateien enthält alle Metadaten, die Android muss die Schriftarten asynchron zu laden, wenn die Anwendung gestartet und auf dem Gerät Zwischenspeichern.
2. **Programmgesteuertes** &ndash; -APIs in Android-API-Ebene 26 ermöglichen einer Anwendung, die die Schriftarten programmgesteuert zu laden, während die Anwendung ausgeführt wird. Apps erstellt eine `FontRequest` Objekts, die für eine angegebene Schriftart, und übergeben Sie dieses Objekt die `FontsContract` Klasse. Die `FontsContract` nimmt die `FontRequest` und ruft die Schriftart aus einer _Schriftart Anbieter_. Android wird synchron auf die Schriftart heruntergeladen. Ein Beispiel zum Erstellen einer `FontRequest` wird weiter unten in diesem Handbuch angezeigt werden.

Unabhängig davon, welcher Ansatz verwendet wird können die Ressourcendateien, die die Xamarin.Android-Anwendung, bevor Sie Schriftarten hinzugefügt werden müssen, heruntergeladen werden. Zunächst muss der Schriftart(en) deklariert werden, in eine XML-Datei in die **Ressourcen/Schriftart** Verzeichnis als Teil einer Schriftartfamilie. Dieser Codeausschnitt ist ein Beispiel zum Herunterladen von Schriftarten aus dem [Google Schriftarten Open-Source-Sammlung](https://fonts.google.com) mithilfe des Standardanbieters für die Schriftart, die in Android 8.0 (oder -Unterstützungsbibliothek v26) enthalten ist:

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

Die `font-family` Element enthält die folgenden Attribute, deklarieren die Informationen, die für Android müssen, um die Schriftarten herunterzuladen:

1. **FontProviderAuthority** &ndash; die Autorität des Schriftart-Anbieters für die Anforderung verwendet werden.
2. **FontPackage** &ndash; das Paket für den Schriftart-Anbieter für die Anforderung verwendet werden. Dies wird verwendet, um zu überprüfen, ob die Identität des Anbieters.
3. **FontQuery** &ndash; Dies ist eine Zeichenfolge, die den Schriftart-Anbieter, suchen Sie die gewünschte Schriftart helfen. Details für die Schriftart-Abfrage sind spezifisch für den Anbieter der Schriftart. Die [ `QueryBuilder` ](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs) -Klasse in der [herunterladbare Schriftarten](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/) Beispiel-app enthält einige Informationen auf das Format der Abfrage für Schriftarten aus dem Google Schriftarten Open-Source-Sammlung.
4. **FontProviderCerts** &ndash; ressourcenarrays mit der Liste der Gruppen von Hashes für die Zertifikate, die der Anbieter mit signiert werden sollen.

Nachdem die Schriftarten definiert sind, muss möglicherweise von Informationen über die _Schriftart Zertifikate_ beteiligt, die mit dem Download.

### <a name="font-certificates"></a>Schriftart-Zertifikate

Wenn der Anbieter für die Schriftart nicht auf dem Gerät vorinstalliert ist oder die app verwendet die `Xamarin.Android.Support.Compat` Android-Bibliothek ist der Sicherheitszertifikate des Schriftart-Anbieters erforderlich. Diese Zertifikate in einer Array-Ressourcendatei, die jeweils im aufgeführt **Ressourcen/Values** Verzeichnis. 

Z. B. das folgende XML heißt **Resources/values/fonts_cert.xml** und speichert die Zertifikate für den Anbieter der Google-Schriftart: 

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

Mit diesen Ressourcendateien vorhanden kann die app die Schriftarten herunterladen.

### <a name="declaring-downloadable-fonts-as-resources"></a>Deklarieren Sie herunterladbare Schriftarten als Ressourcen

Durch Auflisten der herunterladbaren Schriftarten in die **"androidmanifest.xml"**, Android werden die Schriftarten asynchron herunterladen, beim ersten der app Start. Die Schriftart des selbst finden Sie in einer Array-Ressourcendatei, etwa so aus: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="downloadable_fonts" translatable="false">
        <item>@font/vt323</item>
    </array>
</resources>
```

Um diese Schriftarten herunterzuladen, müssen Sie in deklariert werden **"androidmanifest.xml"** durch Hinzufügen von `meta-data` als untergeordnetes Element der `application` Element. Wenn die herunterladbaren Schriftarten in einer Ressourcendatei an deklariert werden z. B. **Resources/values/downloadable_fonts.xml**, und klicken Sie dann diesen Codeausschnitt, der dem Manifest hinzugefügt werden müssten: 

```xml
<meta-data android:name="downloadable_fonts" android:resource="@array/downloadable_fonts" />
```

### <a name="downloading-a-font-with-the-font-apis"></a>Herunterladen einer Schriftart die Schriftart-APIs

Es ist möglich, programmgesteuert Herunterladen einer Schriftart durch Instanziieren einer [ `FontRequest` ](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html) -Objekt und übergibt dieses an die `FontContractCompat.RequestFont` Methode. Die `FontContractCompat.RequestFont` Methode zuerst überprüfen, ob die Schriftart auf dem Gerät vorhanden ist, und klicken Sie dann bei Bedarf wird asynchron Abfrage den Schriftart-Anbieter und versuchen Sie es aus, um die Schriftart für die app herunterzuladen. Wenn `FontRequest` ist, kann nicht heruntergeladen werden die Schriftart Android die Standardschriftart des Systems verwendet wird. 

Ein `FontRequest` Objekt enthält Informationen, die vom Anbieter Schriftart zum finden und Herunterladen einer Schriftart verwendet werden. Ein `FontRequest` erfordert vier Arten von Informationen:

1. **Schriftart-Anbieter Autorität** &ndash; die Autorität des Schriftart-Anbieters für die Anforderung verwendet werden.
2. **Schriftartenpaket** &ndash; das Paket für den Schriftart-Anbieter für die Anforderung verwendet werden. Dies wird verwendet, um zu überprüfen, ob die Identität des Anbieters.
3. **Schriftart-Abfrage** &ndash; Dies ist eine Zeichenfolge, die den Schriftart-Anbieter, suchen Sie die gewünschte Schriftart helfen. Details für die Schriftart-Abfrage sind spezifisch für den Anbieter der Schriftart. Die Details der Zeichenfolge beziehen sich auf den Anbieter der Schriftart. Die [ `QueryBuilder` ](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs) -Klasse in der [herunterladbare Schriftarten](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/) Beispiel-app enthält einige Informationen auf das Format der Abfrage für Schriftarten aus dem Google Schriftarten Open-Source-Sammlung.
4. **Zertifikate von Schriftart** &ndash; ressourcenarrays mit der Liste der Sätze von Hashes für die Zertifikate, die der Anbieter mit signiert werden sollte. 

Dieser Codeausschnitt ist ein Beispiel für die Instanziierung eines neuen `FontRequest` Objekt:

```csharp
FontRequest request = new FontRequest("com.google.android.gms.fonts", "com.google.android.gms", <FontToDownload>, Resource.Array.com_google_android_gms_fonts_certs);
```

Im vorherigen Codeausschnitt `FontToDownload` ist eine Abfrage, die die Schriftart aus der Google-Schriftarten Open-Source-Sammlung unterstützen. 

Vor der Übergabe der `FontRequest` auf die `FontContractCompat.RequestFont` -Methode, es gibt zwei Objekte, die erstellt werden müssen:

* **`FontsContractCompat.FontRequestCallback`** &ndash; Dies ist eine abstrakte Klasse, die erweitert werden muss. Es ist ein Rückruf an, die aufgerufen wird, wenn `RequestFont` abgeschlossen ist. Eine Xamarin.Android-app muss als Unterklasse `FontsContractCompat.FontRequestCallback` und überschreiben die `OnTypefaceRequestFailed` und `OnTypefaceRetrieved`, die Aktionen, die ausgeführt wird, wenn der Download fehlschlägt oder erfolgreich ausgeführt bzw. wird bereitstellen.
* **`Handler`** &ndash; Dies ist eine `Handler` werden die vom verwendet `RequestFont` die Schriftart für einen Thread bei Bedarf heruntergeladen. Schriftarten sollten **nicht** im UI-Thread heruntergeladen werden.

Dieser Codeausschnitt ist ein Beispiel für eine C# -Klasse, die asynchron eine Schriftart aus Google Schriftarten Open-Source-Sammlung heruntergeladen wird. Implementiert die `FontRequestCallback` -Schnittstelle, und löst eine C# Ereignis beim `FontRequest` wurde beendet. 

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

Verwenden Sie dieses Hilfsprogramm, ein neues `FontDownloadHelper` erstellt wird, und ein Ereignishandler zugewiesen wird:  

```csharp
var fontHelper = new FontDownloadHelper();

fontHelper.FontDownloaded += (object sender, FontDownloadEventArg e) => 
{
    //React to the request
};
fontHelper.DownloadFonts(this); // this is an Android Context instance.
```

## <a name="summary"></a>Zusammenfassung

Dieser Leitfaden erläutert die neuen APIs in Android 8.0 herunterladbare Schriftarten und Schriftarten als Ressourcen unterstützen. Er erläutert, wie vorhandene Schriftarten in ein APK eingebettet und in einem Layout zu verwenden. Es wird erläutert, wie Android 8.0 herunterladen Schriftarten vom Anbieter einer Schriftart, entweder programmgesteuert oder durch deklarieren die Schriftart Metadaten in Ressourcendateien unterstützt.

## <a name="related-links"></a>Verwandte Links

- [fontFamily](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily)
- [FontConfig](https://developer.android.com/reference/android/text/FontConfig.html)
- [FontRequest](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html)
- [FontsContractCompat](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html)
- [Resources.GetFont](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int))
- [Schriftart](https://developer.android.com/reference/android/graphics/Typeface.html)
- [Android Support-Bibliothek 26 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/)
- [Verwenden von Schriftarten in Android](https://www.youtube.com/watch?v=TfB-TsLFJdM)
- [CSS-Font-Weight-Spezifikation](https://www.w3.org/TR/css-fonts-3/#font-weight-numeric-values)
- [Google-Schriftarten Open-Source-Sammlung](https://fonts.google.com/)
- [Quelle Sans Pro](https://fonts.google.com/specimen/Source+Sans+Pro)
