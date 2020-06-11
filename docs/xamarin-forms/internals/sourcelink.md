---
Title: "quelllink mit Xamarin.Forms " Beschreibung: "in diesem Artikel wird erläutert, wie der quelllink zum Debuggen in verwendet wird Xamarin.Forms ."
zone_pivot_groups: "Platform" MS. Prod: xamarin ms. assetid: 1e13b389beb ms. Technology: xamarin-Forms Author: profexorgeek ms. Author: jusjohns ms. Date: 09/26/2019 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="source-link-with-xamarinforms"></a>Quelllink mitXamarin.Forms

Xamarin.FormsNuget-Pakete enthalten Zuordnungen von Quell Verknüpfungen. Mit der Quell Verknüpfung werden kompilierte Bibliotheken, die in einem nuget-Paket enthalten sind, einem Quellcoderepository zugeordnet. Visual Studio lädt Quell Code Dateien während des Debuggens herunter und ermöglicht es Entwicklern, Code schrittweise zu durchlaufen und so das Debuggen von Paketen zu ermöglichen, ohne

Weitere Informationen zum Verwenden des Quell Links finden Sie in der [Dokumentation zum quelllink](/dotnet/standard/library-guidance/sourcelink).

::: zone pivot="windows"

> [!WARNING]
> Visual Studio 2019 unterstützt den quelllink für den **.NET-Debugger** , unterstützt derzeit jedoch keinen quelllink für den **Mono-Debugger**. Daher können Sie den quelllink zum Debuggen von UWP-Apps verwenden, jedoch nicht für die Android-oder IOS-app. Wenn Sie UWP-apps Debuggen, müssen Sie sicherstellen, dass die PDB-Dateien für Bibliotheken, die Sie debuggen möchten, in den Ordner " **AppX** " im Verzeichnis " **bin** " kopiert werden, in

## <a name="enable-source-link"></a>Aktivieren von SourceLink

Die Verwendung des Quell Links erfordert das Aktivieren des Debuggens für externen Code; andernfalls führt der Debugger den Code, der nicht in der aktuellen Projekt Mappe enthalten ist, aus. In Visual Studio 2019 finden Sie diese im Menü " **Optionen** " im Abschnitt " **Debuggen** ":

[![Quelllink aktivieren in Visual Studio 2019](sourcelink-images/sourcelink-enable-pc-cropped.png)](sourcelink-images/sourcelink-enable-pc.png#lightbox)

Stellen Sie sicher, dass " **nur eigenen Code aktivieren** " deaktiviert ist und dass **Unterstützung für quelllink aktivieren** aktiviert ist.

::: zone-end
::: zone pivot="macos"

## <a name="enable-source-link"></a>Aktivieren von SourceLink

Die Verwendung des Quell Links erfordert das Aktivieren des Debuggens für externen Code; andernfalls führt der Debugger den Code, der nicht in der aktuellen Projekt Mappe enthalten ist, aus. Diese Option finden Sie im Fenster " **Einstellungen** " im Abschnitt " **Debugger** ":

[![Quelllink in Visual Studio für Mac aktivieren](sourcelink-images/sourcelink-enable-mac-cropped.png)](sourcelink-images/sourcelink-enable-mac.png#lightbox)

Stellen Sie sicher, dass **externer Code in Einzelschritt** aktiviert ist.

::: zone-end

## <a name="debug-xamarinforms-using-source-link"></a>Debuggen Xamarin.Forms mithilfe des Quell Links

Wenn das Debuggen externer Pakete aktiviert ist, verwendet Visual Studio die Quell Link Zuordnungen, die im nuget-Paket enthalten sind, um den externen Quellcode herunterzuladen und schrittweise zu durchlaufen. Dies kann getestet werden, indem beim Abrufen einer Methode, die von bereitgestellt wird, ein Haltepunkt festgelegt wird Xamarin.Forms :

[![Haltepunkt für Methode festgelegt Xamarin.Forms](sourcelink-images/breakpoint-cropped.png)](sourcelink-images/external-code-available.png#lightbox)

Abhängig von den Einstellungen, die Sie in den **Debuggeroptionen** angegeben haben, werden Sie von Visual Studio gewarnt, dass Quelldateien heruntergeladen werden:

[![Visual Studio-Warnung für externen Code](sourcelink-images/external-code-cropped.png)](sourcelink-images/external-code-available.png#lightbox)

Wenn Sie zulassen, dass Visual Studio die Dateien herunterlädt, führt der Debugger einen Einzelschritt in den externen Code aus.

::: zone pivot="windows"

## <a name="source-link-caching"></a>Zwischenspeichern von Quell Links

Quell Link verwendet Caching für die Leistung. Das cachingverzeichnis für den quelllink wird im Menü " **Optionen** " unter " **Debuggen** " im Abschnitt " **Symbole** " definiert:

[![Zwischenspeichern von Quell Links zwischen Visual Studio](sourcelink-images/sourcelink-caching-pc-cropped.png)](sourcelink-images/sourcelink-caching-pc.png#lightbox)

Mit diesem Menü können Sie das cachingverzeichnis für alle Debugsymbole angeben und den Cache löschen, wenn Sie Probleme mit zwischengespeicherten Symbolen haben.

::: zone-end
::: zone pivot="macos"

## <a name="source-link-caching"></a>Zwischenspeichern von Quell Links

Quell Link verwendet Caching für die Leistung. Das cachingverzeichnis für den quelllink unter MacOS lautet `/Users/<username>/Library/Caches/VisualStudio/8.0/Symbols` . Dieser Ordner enthält Unterordner, in denen das Repository zum Herunterladen von Quelldateien gespeichert wird. Wenn sich das Unterstützungs Repository für ein nuget-Paket geändert hat, müssen Sie diese Ordner möglicherweise manuell löschen, um den Cache zu aktualisieren.

::: zone-end

## <a name="related-links"></a>Verwandte Links

- [Dokumentation zum quelllink](/dotnet/standard/library-guidance/sourcelink)
- [Quelllink auf GitHub](https://github.com/dotnet/sourcelink)
