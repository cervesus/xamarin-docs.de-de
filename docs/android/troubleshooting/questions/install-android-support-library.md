---
title: Wie kann ich die Android-Unterstützungsbibliotheken, die für die Xamarin.Android.Support-Pakete erforderlich sind, manuell installieren?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A9CB8CA8-8A6D-405E-B84C-A16CE452C0F7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 107fcd39a64ef1d7758d211ab47a07e1ece52f4e
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757236"
---
# <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packages"></a>Wie kann ich die Android-Unterstützungsbibliotheken, die für die Xamarin.Android.Support-Pakete erforderlich sind, manuell installieren?

## <a name="example-steps-for-xamarinandroidsupportv4"></a>Beispiel Schritte für xamarin. Android. Support. v4 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Laden Sie das gewünschte xamarin. Android. Support-nuget-Paket herunter (z. b. durch die Installation mit dem nuget-Paket-Manager).

Verwenden `ildasm` Sie, um zu überprüfen, welche Version von **android_m2repository. zip** das nuget-Paket benötigt:

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr SourceUrl
```

Beispielausgabe:

```cmd
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
```

Laden **Sie\_Android m2repository. zip** von Google mithilfe der von **Ildasm**zurückgegebenen URL herunter. Alternativ können Sie überprüfen, welche Version des _Android-Unterstützungs Repository_ Sie derzeit im Android SDK-Manager installiert haben:

!["Android SDK-Manager mit installiertem Android-Unterstützungs Repository Version 32 installiert"](install-android-support-library-images/sdk-extras.png)

Wenn die Version mit der Version übereinstimmt, die Sie für das nuget-Paket benötigen, müssen Sie nichts neues herunterladen. Sie können stattdessen das vorhandene **m2repository** -Verzeichnis, das sich unter **Extras\\Android** befindet, im _SDK-Pfad_ neu komprimieren (wie oben im Fenster "Android SDK-Manager" gezeigt).

Berechnen Sie den MD5-Hash der URL, die von **Ildasm**zurückgegeben wurde. Formatieren Sie die resultierende Zeichenfolge so, dass Sie alle Großbuchstaben und keine Leerzeichen verwendet. Passen Sie z. b `$url` . die Variable nach Bedarf an, und führen Sie dann die folgenden zwei Zeilen (basierend auf [ C# dem ursprünglichen Code von xamarin. Android](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208)) in PowerShell aus:

```powershell
$url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"
(([System.Security.Cryptography.MD5]::Create()).ComputeHash([System.Text.Encoding]::UTF8.GetBytes($url)) | %{ $_.ToString("X02") }) -join ""
```

Beispielausgabe:

```powershell
F16A3455987DBAE5783F058F19F7FCDF
```

Kopieren **Sie\_Android m2repository. zip** in den Ordner **% LocalAppData%\\xamarin\\ \\Zips** . Benennen Sie die Datei um, um den MD5-Hash aus dem vorherigen MD5-Hash Berechnungsschritt zu verwenden. Beispiel:

**%LOCALAPPDATA%\\Xamarin\\zips\\F16A3455987DBAE5783F058F19F7FCDF.zip**

Optionale Entzippen Sie die Datei in **%\\LocalAppData% xamarin\\\\xamarin. Android. Support.\\V4\\ 23.4.0.0 Content** (Erstellen eines **Inhalts\\m2repository** Unterverzeichnis). Wenn Sie diesen Schritt überspringen, wird der erste Build, der die Bibliothek verwendet, etwas länger dauern, da dieser Schritt ausgeführt werden muss.
Die Versionsnummer für das Unterverzeichnis (in diesem Beispiel**23.4.0.0** ) stimmt nicht mit der Version des nuget-Pakets überein. Sie können verwenden `ildasm` , um die richtige Versionsnummer zu ermitteln:

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr /C:"string 'Version'"
```

Beispielausgabe:

```cmd
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
```

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Laden Sie das gewünschte xamarin. Android. Support-nuget-Paket herunter (z. b. durch die Installation mit dem nuget-Paket-Manager).

Doppelklicken Sie auf die _xamarin. Android. Support. v4_ -Assembly im Abschnitt _Verweise_ des Android-Projekts in Visual Studio für Mac, um die Assembly im assemblybrowser zu öffnen. Stellen Sie sicher , dass die Dropdown Liste Sprache _C#_ auf festgelegt ist, und wählen Sie die _xamarin. Android. Support. v4_ -Assembly der obersten Ebene aus der Navigationsstruktur des assemblybrowsers aus. Suchen Sie `SourceUrl` die-Eigenschaft unter einem `IncludeAndroidResourcesFrom` der `JavaLibraryReference` Attribute oder:

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

Laden **Sie\_Android m2repository. zip** von Google mithilfe `SourceUrl` der von **Ildasm**zurückgegebenen herunter. Alternativ können Sie überprüfen, welche Version des _Android-Unterstützungs Repository_ Sie derzeit im Android SDK-Manager installiert haben:

!["Android SDK-Manager mit installiertem Android-Unterstützungs Repository Version 32 installiert"](install-android-support-library-images/sdk-extras.png)

Wenn die Version mit der Version übereinstimmt, die Sie für das nuget-Paket benötigen, müssen Sie nichts neues herunterladen. Sie können stattdessen das vorhandene **m2repository** -Verzeichnis, das sich unter " **Extras/Android** " befindet, im _SDK-Pfad_ neu komprimieren (wie oben im Fenster "Android SDK-Manager" gezeigt).

Berechnen Sie den MD5-Hash der URL, die von **Ildasm**zurückgegeben wurde. Formatieren Sie die resultierende Zeichenfolge so, dass Sie alle Großbuchstaben und keine Leerzeichen verwendet. Passen Sie z. b. die URL-Zeichenfolge nach Bedarf an, und führen Sie dann den folgenden Befehl an einer **Terminal. app** -Eingabeaufforderung aus:

```bash
echo -n "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip" | md5 | tr '[:lower:]' '[:upper:]'
```

Eine andere Möglichkeit besteht darin, `csharp` den Interpreter zum Ausführen [desselben C# Codes zu verwenden, den xamarin. Android selbst verwendet](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208).
Passen Sie dazu die `url` Variable nach Bedarf an, und führen Sie dann den folgenden Befehl an einer **Terminal. app** -Eingabeaufforderung aus:

```bash
csharp -e 'var url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"; string.Concat((System.Security.Cryptography.MD5.Create().ComputeHash(System.Text.Encoding.UTF8.GetBytes(url))).Select(b => b.ToString("X02")))'
```

Beispielausgabe:

```bash
F16A3455987DBAE5783F058F19F7FCDF
```

Kopieren **Sie\_Android m2repository. zip** in den Ordner **$Home/.local/share/xamarin/Zips/** . Benennen Sie die Datei um, um den MD5-Hash aus dem vorherigen MD5-Hash Berechnungsschritt zu verwenden. Beispiel:

**$HOME/.local/share/Xamarin/zips/F16A3455987DBAE5783F058F19F7FCDF.zip**

Optionale Entzippen Sie die Datei in: 

**$HOME/.local/share/Xamarin/Xamarin.Android.Support.v4/23.4.0.0/content/**

(Erstellen eines **Content/m2repository-** Unterverzeichnisses). Wenn Sie diesen Schritt überspringen, wird der erste Build, der die Bibliothek verwendet, etwas länger dauern, da dieser Schritt ausgeführt werden muss.

Die Versionsnummer für das Unterverzeichnis (in diesem Beispiel**23.4.0.0** ) stimmt nicht mit der Version des nuget-Pakets überein. Wie im zuvor erwähnten **Ildasm** -Schritt können Sie den assemblybrowser in Visual Studio für Mac verwenden, um die richtige Versionsnummer zu ermitteln. Suchen Sie unter `Version` einem `IncludeAndroidResourcesFrom` der Attribute oder `JavaLibraryReference` nach der-Eigenschaft:

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

-----

## <a name="additional-references"></a>Weitere Verweise

- [Fehler 43245](https://bugzilla.xamarin.com/show_bug.cgi?id=43245) – ungenau "der Download ist fehlgeschlagen. Laden Sie {0} es herunter, und fügen {1} Sie es in das Verzeichnis ein. " und "Bitte installieren Sie das Paket{0}: '", das im SDK-Installer verfügbar ist "Fehlermeldungen im Zusammenhang mit xamarin. Android. Support-Paketen

### <a name="next-steps"></a>Nächste Schritte

In diesem Dokument wird das aktuelle Verhalten im August 2016 erläutert. Das in diesem Dokument beschriebene Verfahren ist nicht Teil der stabilen Testsuite für xamarin, sodass es in Zukunft unterbrechen kann.

Weitere Unterstützung erhalten Sie bei der Kontaktaufnahme mit uns, oder wenn das Problem weiterhin besteht, auch nachdem Sie die oben genannten Informationen genutzt haben, finden Sie [unter welche Supportoptionen für xamarin verfügbar sind?](~/cross-platform/troubleshooting/support-options.md) Informationen zu Kontaktoptionen, Vorschlägen und wie Sie bei Bedarf einen neuen Fehler melden. .
