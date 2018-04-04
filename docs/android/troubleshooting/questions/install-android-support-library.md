---
title: Wie kann ich die Bibliotheken, Android, unterstützen von den Paketen Xamarin.Android.Support manuell installieren?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A9CB8CA8-8A6D-405E-B84C-A16CE452C0F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: e760a87cbd1e0220ed5cf3a350d3539ffe29650e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packages"></a>Wie kann ich die Bibliotheken, Android, unterstützen von den Paketen Xamarin.Android.Support manuell installieren?

## <a name="example-steps-for-xamarinandroidsupportv4"></a>Beispielschritte für Xamarin.Android.Support.v4 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Laden Sie das gewünschte Xamarin.Android.Support NuGet-Paket (z. B. indem Sie sie mit dem NuGet-Paket-Manager installieren).

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

Herunterladen **android\_m2repository.zip** von Google mit der URL Merry **"Ildasm"**. Alternativ können Sie überprüfen, welche Version von den _Android Unterstützung Repository_ im Android SDK Manager haben derzeit installiert:

!["Android SDK-Manager mit der Android-Unterstützung Repository Version 32 installiert"](install-android-support-library-images/sdk-extras.png)

Wenn die Version der Telefonnummer, die Sie für das NuGet-Paket benötigen entspricht, müssen Sie keine neuen herunterladen. Sie können stattdessen erneut zippen vorhandenen **m2repository** Verzeichnis unter **Extras\\android** in der _-SDK-Pfad_ (wie am Anfang der Android dargestellt SDK Fenster "Manager").

Berechnen den MD5-Hash der URL Merry **"Ildasm"**. Formatieren Sie die resultierende Zeichenfolge, um alle Großbuchstaben und keine Leerzeichen verwenden. Passen Sie z. B. die `$url` Variable als erforderlich sind, und führen Sie die folgenden 2 Zeilen (basierend auf [der ursprünglichen C#-Code aus Xamarin.Android](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208)) in PowerShell:

```powershell
$url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"
(([System.Security.Cryptography.MD5]::Create()).ComputeHash([System.Text.Encoding]::UTF8.GetBytes($url)) | %{ $_.ToString("X02") }) -join ""
```
Beispielausgabe:

```powershell
F16A3455987DBAE5783F058F19F7FCDF
```

Kopie **android\_m2repository.zip** in der **%LocalAppData%\\Xamarin\\komprimiert\\**  Ordner. Benennen Sie die Datei, um die MD5-Hash aus dem vorherigen MD5-Hash berechnen Schritt zu verwenden. Zum Beispiel:

**%LOCALAPPDATA%\\Xamarin\\zips\\F16A3455987DBAE5783F058F19F7FCDF.zip**

(Optional) Entzippen Sie die Datei in **%LocalAppData%\\Xamarin\\Xamarin.Android.Support.v4\\23.4.0.0\\Inhalt\\**  (Erstellen einer **Inhalt\\m2repository** Unterverzeichnis). Wenn Sie diesen Schritt überspringen, klicken Sie dann dauert der ersten Build, der die Bibliothek verwendet etwas länger da diese benötigen, um diesen Schritt durchführen.
Die Versionsnummer für das Unterverzeichnis (**23.4.0.0** in diesem Beispiel) ist nicht ganz identisch mit der Version des NuGet-Paket. Sie können `ildasm` die richtige Versionsnummer gefunden:

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr /C:"string 'Version'"
```
Beispielausgabe:

```cmd
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
```

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Laden Sie das gewünschte Xamarin.Android.Support NuGet-Paket (z. B. indem Sie sie mit dem NuGet-Paket-Manager installieren).

Doppelklicken Sie auf die _Xamarin.Android.Support.v4_ Assembly unter der _Verweise_ Abschnitt der Android-Projekts in Visual Studio für Mac, in der Assembly in der Assembly-Browser zu öffnen. Sicherstellen, dass die _Sprache_ Dropdown-Menü auf festgelegt ist _c#_ , und wählen Sie auf der obersten Ebene _Xamarin.Android.Support.v4_ Assembly aus der Assembly Browser Navigationsstruktur. Suchen Sie die `SourceUrl` Eigenschaft unter einem der `IncludeAndroidResourcesFrom` oder `JavaLibraryReference` Attribute:

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

Herunterladen **android\_m2repository.zip** aus Google mithilfe der `SourceUrl` Merry **"Ildasm"**. Alternativ können Sie überprüfen, welche Version von den _Android Unterstützung Repository_ im Android SDK Manager haben derzeit installiert:

!["Android SDK-Manager mit der Android-Unterstützung Repository Version 32 installiert"](install-android-support-library-images/sdk-extras.png)

Wenn die Version der Telefonnummer, die Sie für das NuGet-Paket benötigen entspricht, müssen Sie keine neuen herunterladen. Sie können stattdessen erneut zippen vorhandenen **m2repository** Verzeichnis unter **Extras/Android** in der _-SDK-Pfad_ (Siehe den oberen Rand des Fensters Android SDK-Manager) .

Berechnen den MD5-Hash der URL Merry **"Ildasm"**. Formatieren Sie die resultierende Zeichenfolge, um alle Großbuchstaben und keine Leerzeichen verwenden. Z. B. die URL-Zeichenfolge bei Bedarf anpassen, und führen Sie dann den folgenden Befehl in einem **Terminal.app** Eingabeaufforderung:

```bash
echo -n "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip" | md5 | tr '[:lower:]' '[:upper:]'
```

Eine andere Möglichkeit ist die Verwendung der `csharp` Interpreter auszuführende [der gleichen C#-Code Xamarin.Android selbst verwendet](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208).
Passen Sie zu diesem Zweck die `url` Variable als erforderlich sind, und führen Sie dann den folgenden Befehl in einem **Terminal.app** Eingabeaufforderung:

```bash
csharp -e 'var url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"; string.Concat((System.Security.Cryptography.MD5.Create().ComputeHash(System.Text.Encoding.UTF8.GetBytes(url))).Select(b => b.ToString("X02")))'
```
Beispielausgabe:

```bash
F16A3455987DBAE5783F058F19F7FCDF
```

Kopie **android\_m2repository.zip** auf die **$HOME/.local/share/Xamarin/zips/** Ordner. Benennen Sie die Datei, um die MD5-Hash aus dem vorherigen MD5-Hash berechnen Schritt zu verwenden. Zum Beispiel:

**$HOME/.local/share/Xamarin/zips/F16A3455987DBAE5783F058F19F7FCDF.zip**

(Optional) Entzippen Sie die Datei in ein: 

**$HOME/.local/share/Xamarin/Xamarin.Android.Support.v4/23.4.0.0/content/**

(Erstellen einer **Inhalt/m2repository** Unterverzeichnis). Wenn Sie diesen Schritt überspringen, klicken Sie dann dauert der ersten Build, der die Bibliothek verwendet etwas länger da diese benötigen, um diesen Schritt durchführen.

Die Versionsnummer für das Unterverzeichnis (**23.4.0.0** in diesem Beispiel) ist nicht ganz identisch mit der Version des NuGet-Paket. Wie in der **"Ildasm"** Schritt weiter oben, können Sie den Browser für die Assembly in Visual Studio für Mac finden die richtige Versionsnummer. Suchen Sie nach der `Version` Eigenschaft unter einem der `IncludeAndroidResourcesFrom` oder `JavaLibraryReference` Attribute:

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

-----


## <a name="additional-references"></a>Zusätzliche Referenzen

- [Fehler 43245](https://bugzilla.xamarin.com/show_bug.cgi?id=43245) – verunglimpfende "Fehler beim Herunterladen. Herunterladen Sie {0} und veröffentlichen Sie es in das Verzeichnis \ {1\}." und "Paket installieren: '{0}' ist im SDK-Installationsprogramms verfügbar" Fehlermeldungen im Zusammenhang mit Xamarin.Android.Support-Paketen

### <a name="next-steps"></a>Nächste Schritte

Dieses Dokument beschreibt das aktuelle Verhalten ab August 2016. Die in diesem Dokument beschriebene Technik ist nicht Teil der Tests stabil Suite für Xamarin, damit es in Zukunft unmöglich.

Für weitere Unterstützung zu erhalten, wenden Sie sich an uns, oder bleibt dieses Problem auch nach der Nutzung der oben angegebenen Informationen finden Sie unter [welche Supportoptionen für Xamarin verfügbar sind?](~/cross-platform/troubleshooting/support-options.md) Informationen zu Kontaktoptionen Vorschläge, sowie zur die Datei eines neuen Fehlers bei Bedarf.

