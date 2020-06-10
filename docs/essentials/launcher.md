---
title: "Xamarin.Essentials Launcher" description: "Mit der Launcher-Klasse in Xamarin.Essentials kann eine Anwendung einen URI über das System öffnen."
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33 author: jamesmontemagno ms.custom: video ms.author: jamont ms.date: 08/20/2019 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinessentials-launcher"></a>Xamarin.Essentials: Startprogramm

Mit der **Launcher**-Klasse kann eine Anwendung einen URI durch das System öffnen. Das wird häufig für Deep Linking für benutzerdefinierte URI-Schemas einer andere Anwendung verwendet. Wenn Sie im Browser eine Website öffnen möchten, verweisen Sie auf die **[Browser](open-browser.md)** API.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-launcher"></a>Verwenden des Startprogramms

Fügen Sie Ihrem Projekt einen Xamarin.Essentials-Verweis hinzu:

```csharp
using Xamarin.Essentials;
```

Um die Launcher-Funktionalität zu nutzen, rufen Sie die Methode `OpenAsync` auf, und geben Sie `string` oder `Uri` zum Öffnen an. Optional lässt sich mit der `CanOpenAsync`-Methode überprüfen, ob das URI-Schema von einer Anwendung auf dem Gerät verarbeitet werden kann.

```csharp
public class LauncherTest
{
    public async Task OpenRideShareAsync()
    {
        var supportsUri = await Launcher.CanOpenAsync("lyft://");
        if (supportsUri)
            await Launcher.OpenAsync("lyft://ridetype?id=lyft_line");
    }
}
```

Dies kann mit `TryOpenAsync` zu einem einzelnen Aufruf kombiniert werden, der überprüft, ob der Parameter geöffnet werden kann, und ihn ggf. öffnet.

```csharp
public class LauncherTest
{
    public async Task<bool> OpenRideShareAsync()
    {
        return await Launcher.TryOpenAsync("lyft://ridetype?id=lyft_line");
    }
}
```

### <a name="additional-platform-setup"></a>Einrichtung zusätzlicher Plattformen

# <a name="android"></a>[Android](#tab/android)

Kein zusätzliches Setup.

# <a name="ios"></a>[iOS](#tab/ios)

In iOS 9 und höher erzwingt Apple, welche Schemas eine Anwendung abfragen kann. Um anzugeben, welche Schemas Sie verwenden möchten, müssen Sie `LSApplicationQueriesSchemes` in Ihrer `Info.plist`-Datei angeben.

```
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>lyft</string>  
    <string>fb</string>
</array>
```

# <a name="uwp"></a>[UWP](#tab/uwp)

Kein zusätzliches Setup.

-----

## <a name="files"></a>Dateien

Mithilfe dieser Features kann eine App andere Apps zum Öffnen und Anzeigen einer Datei anfordern. Xamarin.Essentials erkennt automatisch den Dateityp (MIME) und fordert an, dass die Datei geöffnet wird.

Hier sehen Sie ein Beispiel, wie Text auf einen Datenträger geschrieben und angefordert wird, diesen zu öffnen:

```csharp
var fn = "File.txt";
var file = Path.Combine(FileSystem.CacheDirectory, fn);
File.WriteAllText(file, "Hello World");

await Launcher.OpenAsync(new OpenFileRequest
{
    File = new ReadOnlyFile(file)
});
```

## <a name="platform-differences"></a>Plattformunterschiede

# <a name="android"></a>[Android](#tab/android)

Die von `CanOpenAsync` zurückgegebene Aufgabe wird sofort abgeschlossen.

# <a name="ios"></a>[iOS](#tab/ios)

Wenn die Zielanwendung auf diesem Gerät noch nie von `OpenAsync` aus Ihrer Anwendung geöffnet wurde, fordert iOS den Benutzer einmal auf, Ihrer Anwendung das Öffnen zu erlauben.

Die von `CanOpenAsync` zurückgegebene Aufgabe wird sofort abgeschlossen.

Weitere Informationen zur iOS-Implementierung [finden Sie hier](xref:UIKit.UIApplication.CanOpenUrl*).

# <a name="uwp"></a>[UWP](#tab/uwp)

Keine Plattformunterschiede.

-----

## <a name="api"></a>API

- [Launcher-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Launcher)
- [Launcher-API-Dokumentation](xref:Xamarin.Essentials.Launcher)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Launcher-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
