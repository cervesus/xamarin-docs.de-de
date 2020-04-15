---
title: Wie kann ich die Android-Unterstützungsbibliotheken, die für die Xamarin.Android.Support-Pakete erforderlich sind, manuell installieren?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A9CB8CA8-8A6D-405E-B84C-A16CE452C0F7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 99571e0b62592597bb1fffdc8d3ed8336fe050b2
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73026926"
---
# <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packages"></a>Wie kann ich die Android-Unterstützungsbibliotheken, die für die Xamarin.Android.Support-Pakete erforderlich sind, manuell installieren?

## <a name="example-steps-for-xamarinandroidsupportv4"></a>Beispielschritte für Xamarin.Android.Support.v4 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Laden Sie das gewünschte Xamarin.Android.Support-NuGet-Paket herunter (z. B. durch eine Installation über den NuGet-Paket-Manager).

Überprüfen Sie mit `ildasm`, welche Version von **android_m2repository.zip** das NuGet-Paket erfordert:

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr SourceUrl
```

Beispielausgabe:

```cmd
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
```

Laden Sie **android\_m2repository.zip** von Google herunter, indem Sie die von **ildasm** zurückgegebene URL verwenden. Alternativ können Sie überprüfen, welche Version des _Android Support Repository_ Sie zurzeit im Android-SDK-Manager installiert haben:

![Android-SDK-Manager mit Android Support Repository, Version 32 installiert](install-android-support-library-images/sdk-extras.png)

Wenn die Version mit der übereinstimmt, die Sie für das NuGet-Paket benötigen, müssen Sie keine neue Software herunterladen. Sie können stattdessen das vorhandene Verzeichnis **m2repository**, das sich _SDK-Pfad_ unter **extras\\android** befindet, noch einmal zippen. Der Pfad wird oben im Fenster „Android-SDK-Manager“ angezeigt.

Berechnen Sie den MD5-Hash der URL, die von **ildasm** zurückgegeben wurde. Formatieren Sie die resultierende Zeichenfolge so, dass nur Großbuchstaben und keine Leerzeichen verwendet werden. Passen Sie z. B. die Variable `$url` nach Bedarf an, und führen Sie dann die folgenden zwei Zeilen (die auf [dem ursprünglichen C#-Code aus Xamarin.Android](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208) basieren) in PowerShell aus:

```powershell
$url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"
(([System.Security.Cryptography.MD5]::Create()).ComputeHash([System.Text.Encoding]::UTF8.GetBytes($url)) | %{ $_.ToString("X02") }) -join ""
```

Beispielausgabe:

```powershell
F16A3455987DBAE5783F058F19F7FCDF
```

Kopieren Sie **android\_m2repository.zip** in den Ordner **%LOCALAPPDATA%\\Xamarin\\zips\\** . Benennen Sie die Datei um, um den MD5-Hash aus dem vorherigen Hashberechnungsschritt zu verwenden. Zum Beispiel:

**%LOCALAPPDATA%\\Xamarin\\zips\\F16A3455987DBAE5783F058F19F7FCDF.zip**

(Optional) Entzippen Sie die Datei in **%LOCALAPPDATA%\\Xamarin\\Xamarin.Android.Support.v4\\23.4.0.0\\content\\** (so erstellen Sie ein Unterverzeichnis des Typs **content\\m2repository**). Wenn Sie diesen Schritt überspringen möchten, nimmt der erste Build, der diese Bibliothek verwendet, etwas mehr Zeit in Anspruch, da er diesen Schritt durchführen muss.
Die Versionsnummer dieses Unterverzeichnisses (**23.4.0.0** in diesem Beispiel) ist nicht ganz dieselbe wie die Versionsnummer des NuGet-Pakets. Die richtige Versionsnummer können Sie mithilfe von `ildasm` ermitteln:

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr /C:"string 'Version'"
```

Beispielausgabe:

```cmd
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
```

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Laden Sie das gewünschte Xamarin.Android.Support-NuGet-Paket herunter (z. B. durch eine Installation über den NuGet-Paket-Manager).

Doppelklicken Sie auf die Assembly _Xamarin.Android.Support.v4_ im Abschnitt _Verweise_ des Android-Projekts in Visual Studio für Mac, um die Assembly im Assembly-Browser zu öffnen. Stellen Sie sicher, dass die Dropdownliste _Sprache_ auf _C#_ festgelegt ist, und wählen Sie die oberste _Xamarin.Android.Support.v4_-Assembly aus der Navigationsstruktur des Assembly-Browsers aus. Suchen Sie die Eigenschaft `SourceUrl` unter dem Attribut `IncludeAndroidResourcesFrom` oder `JavaLibraryReference`:

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

Laden Sie **android\_m2repository.zip** von Google herunter, indem Sie die von **ildasm** zurückgegebene `SourceUrl` verwenden. Alternativ können Sie überprüfen, welche Version des _Android Support Repository_ Sie zurzeit im Android-SDK-Manager installiert haben:

![Android-SDK-Manager mit Android Support Repository, Version 32 installiert](install-android-support-library-images/sdk-extras.png)

Wenn die Version mit der übereinstimmt, die Sie für das NuGet-Paket benötigen, müssen Sie keine neue Software herunterladen. Sie können stattdessen das vorhandene Verzeichnis **m2repository**, das sich _SDK-Pfad_ unter **extras/android** befindet, noch einmal zippen. Der Pfad wird oben im Fenster „Android-SDK-Manager“ angezeigt.

Berechnen Sie den MD5-Hash der URL, die von **ildasm** zurückgegeben wurde. Formatieren Sie die resultierende Zeichenfolge so, dass nur Großbuchstaben und keine Leerzeichen verwendet werden. Passen Sie beispielsweise die URL-Zeichenfolge nach Bedarf an, und führen Sie den folgenden Befehl in eine **Terminal.app**-Eingabeaufforderung aus:

```bash
echo -n "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip" | md5 | tr '[:lower:]' '[:upper:]'
```

Eine weitere Option ist, mit dem Interpreter `csharp` [denselben C#-Code auszuführen, den Xamarin.Android selbst verwendet](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208).
Dazu müssen Sie die Variable `url` nach Bedarf anpassen und den folgenden Befehl in einer **Terminal.app**-Eingabeaufforderung ausführen:

```bash
csharp -e 'var url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"; string.Concat((System.Security.Cryptography.MD5.Create().ComputeHash(System.Text.Encoding.UTF8.GetBytes(url))).Select(b => b.ToString("X02")))'
```

Beispielausgabe:

```bash
F16A3455987DBAE5783F058F19F7FCDF
```

Kopieren Sie **android\_m2repository.zip** in den Ordner **$HOME/.local/share/Xamarin/zips/** . Benennen Sie die Datei um, um den MD5-Hash aus dem vorherigen Hashberechnungsschritt zu verwenden. Zum Beispiel:

**$HOME/.local/share/Xamarin/zips/F16A3455987DBAE5783F058F19F7FCDF.zip**

(Optional) Entzippen Sie die Datei in: 

**$HOME/.local/share/Xamarin/Xamarin.Android.Support.v4/23.4.0.0/content/**

(so erstellen Sie ein Unterverzeichnis des Typs **content/m2repository**). Wenn Sie diesen Schritt überspringen möchten, nimmt der erste Build, der diese Bibliothek verwendet, etwas mehr Zeit in Anspruch, da er diesen Schritt durchführen muss.

Die Versionsnummer dieses Unterverzeichnisses (**23.4.0.0** in diesem Beispiel) ist nicht ganz dieselbe wie die Versionsnummer des NuGet-Pakets. So wie im **ildasm**-Schritt zuvor, können Sie mit dem Assembly-Browser in Visual Studio für Mac nach der richtigen Versionsnummer suchen. Suchen Sie die Eigenschaft `Version` unter dem Attribut `IncludeAndroidResourcesFrom` oder `JavaLibraryReference`:

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

-----

## <a name="additional-references"></a>Zusätzliche Verweise

- [Bug 43245:](https://bugzilla.xamarin.com/show_bug.cgi?id=43245) Ungenaue Fehlermeldungen „Download failed. Please download {0} and put it to the {1} directory.“ (Download fehlgeschlagen. Laden Sie {0} herunter, und legen Sie es im Verzeichnis {0} ab.) und „Installieren Sie das Paket „{0}“, das im SDK-Installationsprogramm verfügbar ist“, die im Zusammenhang mit Xamarin.Android.Support-Paketen stehen

### <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wird das im August 2016 aktuelle Verhalten besprochen. Die in diesem Artikel beschriebene Technik ist kein Teil einer stabilen Testsammlung für Xamarin. Sie könnte daher in Zukunft nicht mehr unterstützt werden.

Wenn Sie weitere Unterstützung benötigen, kontaktieren Sie uns. Wenn das Problem auch nach Zuhilfenahme der obigen Informationen weiter besteht, lesen Sie die Informationen unter [Hilfe zu Visual Studio erhalten](~/cross-platform/troubleshooting/support-options.md). Dort finden Sie alle Kontaktoptionen, Vorschläge und, falls notwendig, Anleitungen zum Erstellen eines neuen Bugs.
