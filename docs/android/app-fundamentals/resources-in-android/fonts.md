---
title: Schriftarten
ms.prod: xamarin
ms.assetid: 3F543FC5-FDED-47F8-8D2C-481FCC98BFDA
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 04/13/2018
ms.openlocfilehash: 086576ea7d806bb0768fbe4563df7fca99244ccb
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
ms.locfileid: "31044820"
---
# <a name="fonts"></a>Schriftarten

## <a name="overview"></a>Übersicht

API-Ebene 26 ab, mit dem Android SDK können Schriftarten als Ressourcen, wie eine Layouts behandelt werden soll oder Drawables. Die [Android Support Library 26 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/26.1.0.1) wird Backport die neue Schriftart-APIs für diese apps, die API-Ebene 14 oder höher abzielen.

Nach der Anwendung 26-API oder der Android-Unterstützungsbibliothek v26 aus installieren gibt es zwei Möglichkeiten zur Verwendung von Schriftarten in einer Android-Anwendung:

1. **Packen Sie die Schriftart als Android Ressource** &ndash; Dadurch wird sichergestellt, dass die Schriftart immer für die Anwendung verfügbar ist, aber die Größe des der APK erhöhen. 
2. **Laden Sie die Schriftarten** &ndash; Android unterstützt auch eine Schriftart aus Herunterladen einer _Schriftart Anbieter_. Der Anbieter Schriftart wird überprüft, ob die Schriftart bereits auf dem Gerät vorhanden ist. Falls erforderlich, wird die Schriftart heruntergeladen und auf dem Gerät zwischengespeichert. Diese Schriftart kann zwischen mehreren Anwendungen gemeinsam genutzt werden.

Ähnlich wie Schriftarten (oder eine Schriftart, die mehrere verschiedene Formate haben kann) in gruppiert werden _Schriftartfamilien_. Dadurch können Entwickler bestimmte Attribute der Schriftart, z. B. der Gewichtung angeben und Android wählt automatisch die passende Schriftart aus der Schriftfamilie.

Die Bibliothek für Android unterstützt v26 wird Backport-Unterstützung für Schriftarten auf API-Ebene 26. Beim Abzielen auf die älteren API-Ebenen, es ist erforderlich, deklarieren Sie die `app` XML-Namespace und nennen Sie die verschiedenen Schriftartattribute mithilfe der `android:` Namespace und die `app:` Namespace. Wenn nur die `android:` Namespace wird verwendet, und klicken Sie dann die Schriftarten nicht angezeigten Geräte, die API-Ebene ausgeführt, 25 oder darunter. Diese XML-Ausschnitt deklariert z. B. ein neues [ _Schriftfamilie_ ](#font_families) Ressource, die in API-Ebene 14 und höher ausgeführt wird:

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

Solange Schriftarten für eine Android-Anwendung ordnungsgemäß bereitgestellt werden, sie können angewendet werden auf ein UI-Widget durch Festlegen der [ `fontFamily` Attribut](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily). Der folgende Codeausschnitt zeigt beispielsweise, wie eine Schriftart in ein TextView angezeigt:

```xml
<TextView
    android:text="The quick brown fox jumped over the lazy dog."
    android:fontFamily="@font/sourcesanspro_regular"
    app:fontFamily="@font/sourcesanspro_regular"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

Dieses Handbuch wird zunächst erläutert, wie für die Verwendung von Schriftarten als Android-Ressource, und fahren Sie dann zum Herunterladen von Schriftarten zur Laufzeit zu besprechen.

## <a name="fonts-as-a-resource"></a>Schriftarten, die als Ressource

Packen eine Schriftart in ein Android APK wird sichergestellt, dass es immer für die Anwendung verfügbar ist. Eine Schriftartdatei (entweder ein. TTF oder ein. Durch Kopieren von Dateien in ein Unterverzeichnis im Verzeichnis einer Anwendung Xamarin.Android genau wie jede andere Ressource OTF-Datei) hinzugefügt der **Ressourcen** Ordner eines Xamarin.Android-Projekts. Schriftarten Ressourcen bleiben einer **Schriftart** Unterverzeichnis von der **Ressourcen** Ordner des Projekts. 

> [!NOTE]
> Die Schriftarten müssen eine **Buildvorgang** von **AndroidResource** oder wird nicht in der endgültigen APK verpackt werden. Die Buildaktion sollte automatisch von der IDE festgelegt werden.

Wenn es viele ähnliche Schriftartdateien (z. B. Schriftart mit anders gewichtet oder Stile gibt) ist es möglich, die sie in einer Schriftartfamilie zu gruppieren.

<a name="font_families" />

### <a name="font-families"></a>Schriftartfamilien

Eine Schriftfamilie ist ein Satz von Schriftarten, die anders gewichtet und Formate haben. Möglicherweise gibt es z. B. separate Schriftartdateien für fett oder kursiv-Schriftarten. Die Schriftfamilie wird definiert, indem `font` Elemente in einer XML-Datei, die aufbewahrt werden, in der **Ressourcen/Schriftart** Verzeichnis. Jede Schriftfamilie sollte seinem eigenen XML-Datei haben.

Zum Erstellen einer eine Schriftfamilie, zunächst alle Schriftarten zum Hinzufügen der **Ressourcen/Schriftart** Ordner. Dann erstellen Sie eine neue XML-Datei im Ordner "Schriftart" für die Schriftfamilie. Der Name der XML-Datei ist keine Affinität oder Beziehung die Schriftarten, die auf die verwiesen wird. die Ressourcendatei kann einen beliebigen Dateinamen zulässige Android-Ressource. Diese XML-Datei weist einen Stamm `font-family` Element, das eine oder mehrere enthält `font` Elemente. Jede `font` Element deklariert, die Attribute einer Schriftart.

Das folgende XML ist ein Beispiel für eine Schriftfamilie für die _Quellen Sans Pro_ Schriftart, die viele verschiedene Schriftbreiten definiert. Dies wird als Datei im gespeichert die **Ressourcen/Schriftart** Ordner mit dem Namen **sourcesanspro.xml**:

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
* **Kursiv** &ndash; eine kursive Schriftart.

Die `fontWeight` Attribut entspricht dem CSS `font-weight` Attribut und bezieht sich auf die Stärke der Schriftart. Dies ist ein Wert im Bereich von 100 bis 900. Die folgende Liste beschreibt die allgemeinen Schriftart Gewichtungswerte und deren Namen:

* **Schlanke** &ndash; 100
* **Zusätzliche hell** &ndash; 200
* **Light** &ndash; 300
* **Normal** &ndash; 400
* **Mittel** &ndash; 500
* **Semi fett** &ndash; 600
* **Fett formatierte** &ndash; 700
* **Zusätzliche fett** &ndash; 800
* **Schwarz** &ndash; 900

Nachdem Sie eine Schriftfamilie definiert wurde, kann verwendet werden deklarativ durch Festlegen der `fontFamily`, `textStyle`, und `fontWeight` Attribute in der Layoutdatei.  Beispielsweise legt die folgende XML-Ausschnitt eine Gewichtung von 400-Schriftart (normalen) und einen Stil um Text kursiv:

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

### <a name="programmatically-assigning-fonts"></a>Zuweisen von programmgesteuert Schriftarten

Schriftarten können programmgesteuert festgelegt werden, mithilfe der [ `Resources.GetFont` ](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int)) Methode zum Abrufen einer [ `Typeface` ](https://developer.android.com/reference/android/graphics/Typeface.html) Objekt. Viele Ansichten verfügen über eine `TypeFace` -Eigenschaft, die verwendet werden kann, das Widget die Schriftart zuweisen. Dieser Codeausschnitt zeigt, wie Sie programmgesteuert die Schriftart für eine TextView festlegen:

```csharp
Android.Graphics.Typeface typeface = this.Resources.GetFont(Resource.Font.caveat_regular);
textView1.Typeface = typeface;
textView1.Text = "Changed the font";
```

Die `GetFont` Methode lädt automatisch die erste Schriftart in eine Schriftfamilie.  Verwenden Sie zum Laden einer Schriftart, die ein bestimmtes Format entspricht, die `Typeface.Create` Methode. Diese Methode versucht, eine Schriftart zu laden, die das angegebene Format übereinstimmt. Dieser Ausschnitt versucht beispielsweise auf ein fett laden `Typeface` -Sitzungsobjekts eine Schriftfamilie, die in definiert ist **Ressourcen/Schriftarten**:

```csharp
var typeface = Typeface.Create("<FONT FAMILY NAME>", Android.Graphics.TypefaceStyle.Bold);
textView1.Typeface = typeface;
```

## <a name="downloading-fonts"></a>Herunterladen von Schriftarten

Anstatt als eine Anwendungsressource Verpacken von Schriftarten kann Android Schriftarten aus einer Remotequelle herunterladen. Dies hat die wünschenswerte Auswirkungen Verringern der Größe der APK. 

Schriftarten werden heruntergeladen, mit Unterstützung einer _Schriftart Anbieter_. Dies ist eine spezielle Inhaltsanbieter, die das Herunterladen und Zwischenspeichern von Schriftarten für alle Anwendungen auf dem Gerät verwaltet. Android 8.0 umfasst einen Anbieter Schriftart zum Herunterladen von Schriftarten aus dem [Google Schriftart Repository](http://fonts.google.com). Diese Schriftart Standardanbieter ist mehr auf API-Ebene 14 mit der Android-Unterstützungsbibliothek v26.

Wenn eine app für eine Schriftart anfordert, wird der Schriftart-Anbieter zuerst überprüfen, um festzustellen, ob die Schriftart bereits auf dem Gerät vorhanden ist. Wenn dies nicht der Fall ist, versucht er, klicken Sie dann die Schriftart heruntergeladen. Wenn die Schriftart heruntergeladen, klicken Sie dann für Android werden kann, wird die Standardschriftart des Systems verwendet. Sobald die Schriftart heruntergeladen wurde, ist es für alle Anwendungen auf dem Gerät, nicht nur die app, die die ursprüngliche Anforderung verfügbar.

Wenn eine Anforderung zum Herunterladen einer Schriftart erfolgt, wird die app den Schriftart-Anbieter nicht direkt abzufragen. Apps werden verwenden Sie stattdessen eine Instanz von der [ `FontsContract` ](https://developer.android.com/reference/android/provider/FontsContract.html) API (oder die [ `FontsContractCompat` ](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html) Wenn 26 der Support-Bibliothek verwendet wird).  

Android 8.0 unterstützt Schriftarten auf zwei verschiedene Arten:

1. **Deklarieren Sie herunterladbare Schriftarten als Ressource** &ndash; eine app möglicherweise herunterladbare Schriftarten Android über XML-Ressourcendateien deklarieren. Diese Dateien enthält alle Metadaten Android, die die Schriftarten asynchron heruntergeladen werden soll, wenn die Anwendung gestartet und auf dem Gerät Zwischenspeichern muss.
2. **Programmgesteuertes** &ndash; -APIs in Android-API-Ebene 26, damit eine Anwendung die Schriftarten programmgesteuert zu laden, während die Anwendung ausgeführt wird. Apps erstellt eine `FontRequest` Objekts für eine angegebene Schriftart, und übergeben Sie dieses Objekt die `FontsContract` Klasse. Die `FontsContract` nimmt die `FontRequest` und ruft die Schriftart aus einer _Schriftart Anbieter_. Android wird synchron auf die Schriftart heruntergeladen. Ein Beispiel zum Erstellen einer `FontRequest` wird weiter unten in diesem Handbuch angezeigt werden.

Unabhängig davon, welcher Ansatz verwendet wird können die Ressourcendateien, die an die Anwendung Xamarin.Android vor Schriftarten hinzugefügt werden müssen heruntergeladen werden. Zuerst muss die Schriftart(en) deklariert werden, in eine XML-Datei in die **Ressourcen/Schriftart** Verzeichnis als Teil des eine Schriftfamilie. Dieser Codeausschnitt ist ein Beispiel zum Herunterladen von Schriftarten aus dem [Google Schriftarten Open Source-Auflistung](https://fonts.google.com) mit dem Standardanbieter für Schriftart, die mit Android 8.0 (oder Unterstützungsbibliothek v26) stammen:

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

Die `font-family` Element enthält die folgenden Attribute, deklarieren die Information, dass Android erfordert, um die Schriftarten herunterzuladen:

1. **FontProviderAuthority** &ndash; die Autorität des Schriftart-Anbieters für die Anforderung verwendet werden soll.
2. **FontPackage** &ndash; das Paket für den Anbieter der Schriftart für die Anforderung verwendet werden soll. Dient zum Überprüfen der Identität des Anbieters.
3. **FontQuery** &ndash; Dies ist eine Zeichenfolge, die den Schriftart Anbieter suchen Sie die gewünschte Schriftart helfen. Details für die Schriftart-Abfrage sind spezifisch für die Schriftart-Anbieter. Die [ `QueryBuilder` ](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs) -Klasse in der [herunterladbare Schriftarten](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/) Beispiel-app bietet einige Informationen zu den Abfrage-Format für Schriftarten aus der Google Schriftarten Open Source-Auflistung.
4. **FontProviderCerts** &ndash; ein ressourcenarray mit der Liste der Sätze von Hashes für die Zertifikate, die mit der Anbieter signiert werden soll.

Nachdem die Schriftarten definiert werden, es kann erforderlich sein, Informationen zur der _Schriftart Zertifikate_ mit dem Download beteiligt.

### <a name="font-certificates"></a>Schriftart-Zertifikate

Wenn der Schriftart-Anbieter auf dem Gerät nicht vorinstalliert ist oder wenn die app verwendet die `Xamarin.Android.Support.Compat` Android-Bibliothek erfordert die Sicherheitszertifikate des Schriftart-Anbieters. Diese Zertifikate werden in ein Array-Ressourcendatei, die aufbewahrt werden, in aufgeführt werden **Ressourcen/Werte** Verzeichnis. 

Das folgende XML heißt beispielsweise **Resources/values/fonts_cert.xml** und speichert die Zertifikate für den Google-Schriftart-Anbieter: 

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

Durch Auflisten der herunterladbaren Schriftarten in die **AndroidManifest.XML**, Android werden die Schriftarten asynchron herunterladen, wenn die app zum ersten Mal startet. Die Schriftart des selbst sind aufgeführt, in ein Array-Ressourcendatei, etwa so aus: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="downloadable_fonts" translatable="false">
        <item>@font/vt323</item>
    </array>
</resources>
```

Informationen zum Herunterladen dieser Schriftarten denen in deklariert werden **AndroidManifest.XML** durch Hinzufügen von `meta-data` als untergeordnetes Element der `application` Element. Angenommen, die herunterladbaren Schriftarten in einer Ressourcendatei am deklariert werden **Resources/values/downloadable_fonts.xml**, und klicken Sie dann diesen Ausschnitt, der dem Manifest hinzugefügt werden müssten: 

```xml
<meta-data android:name="downloadable_fonts" android:resource="@array/downloadable_fonts" />
```

### <a name="downloading-a-font-with-the-font-apis"></a>Herunterladen einer Schriftart die Schriftart-APIs

Es ist möglich, programmgesteuert Herunterladen einer Schriftart durch Instanziierung einer [ `FontRequest` ](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html) Objekts und übergeben, um die `FontContractCompat.RequestFont` Methode. Die `FontContractCompat.RequestFont` Methode zuerst überprüfen, um festzustellen, ob die Schriftart auf dem Gerät vorhanden ist, und klicken Sie dann bei Bedarf asynchron Abfrage der Schriftart-Anbieter und versuchen Sie es, um die Schriftart für die app herunterzuladen. Wenn `FontRequest` befindet sich nicht die Schriftart heruntergeladen, Android die Standardschriftart des Systems. 

Ein `FontRequest` Objekt enthält Informationen, die vom Anbieter Schriftart zum Suchen und Herunterladen einer Schriftart verwendet werden. Ein `FontRequest` erfordert vier Arten von Informationen:

1. **Autorität für die Schriftart Anbieter** &ndash; die Autorität des Schriftart-Anbieters für die Anforderung verwendet werden soll.
2. **Schriftartenpaket** &ndash; das Paket für den Anbieter der Schriftart für die Anforderung verwendet werden soll. Dient zum Überprüfen der Identität des Anbieters.
3. **Schriftart Abfrage** &ndash; Dies ist eine Zeichenfolge, die den Schriftart Anbieter suchen Sie die gewünschte Schriftart helfen. Details für die Schriftart-Abfrage sind spezifisch für die Schriftart-Anbieter. Die Details der Zeichenfolge sind spezifisch für den Datenanbieter Schriftart. Die [ `QueryBuilder` ](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs) -Klasse in der [herunterladbare Schriftarten](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/) Beispiel-app bietet einige Informationen zu den Abfrage-Format für Schriftarten aus der Google Schriftarten Open Source-Auflistung.
4. **Zertifikate von Schriftart** &ndash; ein ressourcenarray mit der Liste der Sätze von Hashes für die Zertifikate, die der Anbieter muss signiert sein, mit. 

Dieser Codeausschnitt ist ein Beispiel für die Instanziierung eines neuen `FontRequest` Objekt:

```csharp
FontRequest request = new FontRequest("com.google.android.gms.fonts", "com.google.android.gms", <FontToDownload>, Resource.Array.com_google_android_gms_fonts_certs);
```

Im vorherigen Codeausschnitt `FontToDownload` ist eine Abfrage, die die Schriftart aus der Auflistung Google Open Source-Schriftarten helfen. 

Vor der Übergabe der `FontRequest` auf die `FontContractCompat.RequestFont` -Methode, stehen zwei Objekte, die erstellt werden müssen:

* **`FontsContractCompat.FontRequestCallback`** &ndash; Dies ist eine abstrakte Klasse, die erweitert werden muss. Es ist ein Rückruf, die aufgerufen wird, wenn `RequestFont` abgeschlossen ist. Eine app Xamarin.Android muss als Unterklasse `FontsContractCompat.FontRequestCallback` und überschreiben die `OnTypefaceRequestFailed` und `OnTypefaceRetrieved`, die Aktionen, die ausgeführt werden, wenn der Download fehlschlägt oder erfolgreich ausgeführt bzw. wird bereitstellen.
* **`Handler`** &ndash; Dies ist eine `Handler` von verwendet wird `RequestFont` die Schriftart für einen Thread bei Bedarf heruntergeladen. Schriftarten sollten **nicht** im UI-Thread heruntergeladen werden.

Dieser Codeausschnitt ist ein Beispiel einer C#-Klasse, die asynchron eine Schriftart aus Google Schriftarten Open Source-Auflistung heruntergeladen wird. Implementiert die `FontRequestCallback` -Schnittstelle und löst ein Ereignis c# bei `FontRequest` wurde beendet. 

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

Dieses Hilfsprogramm verwendet eine neue `FontDownloadHelper` erstellt wurde, und ein Ereignishandler zugeordnet ist:  

```csharp
var fontHelper = new FontDownloadHelper();

fontHelper.FontDownloaded += (object sender, FontDownloadEventArg e) => 
{
    //React to the request
};
fontHelper.DownloadFonts(this); // this is an Android Context instance.
```

## <a name="summary"></a>Zusammenfassung

Diese Anleitung erläutert die neuen APIs im Android 8.0 herunterladbare Schriftarten und Schriftarten als Ressourcen unterstützen. Es wurde beschrieben, wie vorhandene Schriftarten in einer APK eingebettet werden sollen, und Ihre Verwendung in einem Layout. Es wird erläutert, wie Android 8.0 Schriftarten vom Anbieter einer Schriftart, entweder programmgesteuert oder indem Sie deklarieren die Schriftart Metadaten in Ressourcendateien unterstützt.

## <a name="related-links"></a>Verwandte Links

- [FontFamily](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily)
- [FontConfig](https://developer.android.com/reference/android/text/FontConfig.html)
- [FontRequest](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html)
- [FontsContractCompat](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html)
- [Resources.GetFont](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int))
- [Schriftart](https://developer.android.com/reference/android/graphics/Typeface.html)
- [Die Bibliothek 26 NuGet Android-Unterstützung](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/)
- [Verwenden von Schriftarten in Android](https://www.youtube.com/watch?v=TfB-TsLFJdM)
- [CSS-Schriftart Gewichtung-Spezifikation](https://www.w3.org/TR/css-fonts-3/#font-weight-numeric-values)
- [Google Schriftarten Open Source-Auflistung](https://fonts.google.com/)
- [Sans Pro Quelle](https://fonts.google.com/specimen/Source+Sans+Pro)
