---
title: Wie kann ich die Android-Unterstützungsbibliotheken erforderlich, die für die Xamarin.Android.Support-Pakete manuell installieren?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A9CB8CA8-8A6D-405E-B84C-A16CE452C0F7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 84ee33fe174c01656144e55bc3cbba7c773950fd
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120644"
---
# <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packages"></a>Wie kann ich die Android-Unterstützungsbibliotheken erforderlich, die für die Xamarin.Android.Support-Pakete manuell installieren?

## <a name="example-steps-for-xamarinandroidsupportv4"></a>Beispielschritte für Xamarin.Android.Support.v4 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Laden Sie das gewünschte Xamarin.Android.Support-NuGet-Paket (z. B. durch die Installation mit dem NuGet-Paket-Manager).

Verwendung `ildasm` überprüfen Sie die Version von **android_m2repository.zip** muss das NuGet-Paket:

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr SourceUrl
```
Beispielausgabe:

```cmd
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
```

Herunterladen **android\_m2repository.zip** von Google über die URL Merry **Ildasm**. Alternativ können Sie überprüfen, welche Version der _Android Support Repository_ derzeit installierten im Android SDK-Manager:

!["Android SDK Manager, die mit Android Support Repository, Version 32 installiert"](install-android-support-library-images/sdk-extras.png)

Wenn die Version der übereinstimmt, die Sie für das NuGet-Paket benötigen, müssen Sie alles, was neu zu laden. Sie können stattdessen erneut zippen Sie den vorhandenen **m2repository** Verzeichnis, das sich befindet, **Extras\\android** in die _SDK-Pfad_ (Siehe den oberen Rand der Android SDK-Manager-Fenster).

Berechnen den MD5-Hash, der von zurückgegebene URL **Ildasm**. Formatieren Sie die resultierende Zeichenfolge, um alle Großbuchstaben und keine Leerzeichen verwenden. Anpassen, z. B. die `$url` Variable benötigt, und führen Sie die folgenden 2 Zeilen (basierend auf [der ursprünglichen C# Code von Xamarin.Android](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208)) in PowerShell:

```powershell
$url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"
(([System.Security.Cryptography.MD5]::Create()).ComputeHash([System.Text.Encoding]::UTF8.GetBytes($url)) | %{ $_.ToString("X02") }) -join ""
```
Beispielausgabe:

```powershell
F16A3455987DBAE5783F058F19F7FCDF
```

Kopie **android\_m2repository.zip** in die **%LocalAppData%\\Xamarin\\zips\\**  Ordner. Benennen Sie die Datei, um die MD5-Hash aus dem vorherigen Schritt berechnen MD5-Hash entscheiden. Zum Beispiel:

**%LocalAppData%\\Xamarin\\zips\\F16A3455987DBAE5783F058F19F7FCDF.zip**

(Optional) Entzippen Sie die Datei in **%LocalAppData%\\Xamarin\\Xamarin.Android.Support.v4\\23.4.0.0\\Inhalt\\**  (Erstellen einer **Inhalt\\m2repository** Unterverzeichnis). Wenn Sie diesen Schritt überspringen, klicken Sie dann dauert der erste Build, der die Bibliothek verwendet ein wenig länger, da sie benötigen, um diesen Schritt abzuschließen.
Die Versionsnummer für das Unterverzeichnis (**23.4.0.0** in diesem Beispiel) ist nicht ganz dasselbe wie das NuGet-Paket-Version. Sie können `ildasm` finden Sie die richtige Versionsnummer:

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

Laden Sie das gewünschte Xamarin.Android.Support-NuGet-Paket (z. B. durch die Installation mit dem NuGet-Paket-Manager).

Doppelklicken Sie auf die _Xamarin.Android.Support.v4_ Assembly unter der _Verweise_ Teil des Android-Projekts in Visual Studio für Mac, um die Assembly in der Assembly-Browser zu öffnen. Sicherstellen, dass die _Sprache_ Dropdownliste nastaven NA hodnotu _C#_ , und wählen Sie auf der obersten Ebene _Xamarin.Android.Support.v4_ -Assembly aus der Assembly-Browser-Navigationsstruktur . Suchen Sie die `SourceUrl` Eigenschaft unter einem der `IncludeAndroidResourcesFrom` oder `JavaLibraryReference` Attribute:

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

Herunterladen **android\_m2repository.zip** aus Google mithilfe der `SourceUrl` Merry **Ildasm**. Alternativ können Sie überprüfen, welche Version der _Android Support Repository_ derzeit installierten im Android SDK-Manager:

!["Android SDK Manager, die mit Android Support Repository, Version 32 installiert"](install-android-support-library-images/sdk-extras.png)

Wenn die Version der übereinstimmt, die Sie für das NuGet-Paket benötigen, müssen Sie alles, was neu zu laden. Sie können stattdessen erneut zippen Sie den vorhandenen **m2repository** Verzeichnis, das sich befindet, **Extras/Android** in die _SDK-Pfad_ (Siehe den oberen Rand des Fensters für die Android SDK-Manager) .

Berechnen den MD5-Hash, der von zurückgegebene URL **Ildasm**. Formatieren Sie die resultierende Zeichenfolge, um alle Großbuchstaben und keine Leerzeichen verwenden. Z. B. die URL-Zeichenfolge nach Bedarf anpassen, und führen Sie dann den folgenden Befehl in einem **Terminal.app** Eingabeaufforderung:

```bash
echo -n "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip" | md5 | tr '[:lower:]' '[:upper:]'
```

Eine weitere Möglichkeit ist die Verwendung der `csharp` Interpreter ausgeführt [gleich C# Code, Xamarin.Android selbst verwendet](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208).
Passen Sie zu diesem Zweck die `url` Variable benötigt, und führen Sie dann den folgenden Befehl in einer **Terminal.app** Eingabeaufforderung:

```bash
csharp -e 'var url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"; string.Concat((System.Security.Cryptography.MD5.Create().ComputeHash(System.Text.Encoding.UTF8.GetBytes(url))).Select(b => b.ToString("X02")))'
```
Beispielausgabe:

```bash
F16A3455987DBAE5783F058F19F7FCDF
```

Kopie **android\_m2repository.zip** auf die **$HOME/.local/share/Xamarin/zips/** Ordner. Benennen Sie die Datei, um die MD5-Hash aus dem vorherigen Schritt berechnen MD5-Hash entscheiden. Zum Beispiel:

**$HOME/.local/share/Xamarin/zips/F16A3455987DBAE5783F058F19F7FCDF.zip**

(Optional) Entzippen Sie die Datei in ein: 

**$HOME/.local/share/Xamarin/Xamarin.Android.Support.v4/23.4.0.0/content/**

(Erstellen einer **Inhalt/m2repository** Unterverzeichnis). Wenn Sie diesen Schritt überspringen, klicken Sie dann dauert der erste Build, der die Bibliothek verwendet ein wenig länger, da sie benötigen, um diesen Schritt abzuschließen.

Die Versionsnummer für das Unterverzeichnis (**23.4.0.0** in diesem Beispiel) ist nicht ganz dasselbe wie das NuGet-Paket-Version. Wie in der **Ildasm** Schritt weiter oben, können Sie die Assembly-Browser in Visual Studio für Mac finden Sie die richtige Versionsnummer. Suchen Sie nach der `Version` Eigenschaft unter einem der `IncludeAndroidResourcesFrom` oder `JavaLibraryReference` Attribute:

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

-----


## <a name="additional-references"></a>Zusätzliche Referenzen

- [Fehler 43245](https://bugzilla.xamarin.com/show_bug.cgi?id=43245) – verunglimpfende "Fehler beim Herunterladen. Laden Sie {0} , und fügen Sie es auf die {1} Directory. " und "Installieren Sie das Paket: '{0}" im SDK-Installationsprogramm verfügbar "Fehlermeldungen im Zusammenhang mit der Xamarin.Android.Support-Pakete

### <a name="next-steps"></a>Nächste Schritte

In diesem Dokument wird das aktuelle Verhalten ab August 2016 erläutert. In diesem Dokument beschriebene Technik ist nicht Teil der Tests stabil Suite für Xamarin, damit es in der Zukunft beschädigen könnten.

Weitere Unterstützung benötigen, kontaktieren uns, oder wenn dieses Problem bestehen bleibt, auch nach der Verwendung der oben genannten Informationen finden Sie unter [welche Supportoptionen für Xamarin verfügbar sind?](~/cross-platform/troubleshooting/support-options.md) auf Kontaktoptionen, Vorschläge, Informationen sowie zur einen neuen Bug-Datei bei Bedarf.

