---
title: "Xamarin.Essentials: App-Design" description: "In diesem Dokument wird die RequestedTheme-API in Xamarin.Essentials beschrieben, die Informationen dazu enthält, welcher Designstil für die ausgeführte App angefordert wird."
ms.assetid: F6F6D496-A8A9-4B9A-AF1A-370D937E5073 author: jamesmontemagno ms.custom: video ms.author: jamont ms.date: 01/06/2020 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinessentials-app-theme"></a>Xamarin.Essentials: App-Design

Die **RequestedTheme**-API ist Teil der [`AppInfo`](app-information.md)-Klasse und liefert Informationen darüber, welches Design vom System für Ihre ausgeführte Anwendung angefordert wird.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-requestedtheme"></a>Verwenden von RequestedTheme

Fügen Sie Ihrem Projekt einen Xamarin.Essentials-Verweis hinzu:

```csharp
using Xamarin.Essentials;
```

## <a name="obtaining-theme-information"></a>Abrufen der Designinformationen

Das angeforderte Anwendungsdesign kann mithilfe der folgenden API erkannt werden:

```csharp
AppTheme appTheme = AppInfo.RequestedTheme;

```

Dadurch wird das aktuell angeforderte Design vom System für Ihre Anwendung bereitgestellt. Der Rückgabewert ist einer der folgenden:

* Nicht angegeben.
* Hell
* Dunkel

„Nicht angegeben“ wird zurückgegeben, wenn das Betriebssystem nicht über einen bestimmten Benutzeroberflächenstil verfügt, der angefordert werden kann. Ein Beispiel hierfür sind Geräte, auf denen Versionen von iOS vor 13.0 ausgeführt werden.


## <a name="platform-implementation-specifics"></a>Besonderheiten bei der plattformspezifischen Implementierung

# <a name="android"></a>[Android](#tab/android)

Android verwendet Konfigurationsmodi, um den Typ des Designs festzulegen, der vom Benutzer angefordert werden soll. Je nach Android-Version kann er vom Benutzer geändert werden oder wird geändert, wenn der Stromsparmodus aktiviert ist.

Weitere Informationen finden Sie in der offiziellen [Android-Dokumentation zum dunklen Design](https://developer.android.com/guide/topics/ui/look-and-feel/darktheme).


# <a name="ios"></a>[iOS](#tab/ios)

Bei iOS-Versionen vor 13.0 wird stets „Nicht angegeben“ zurückgegeben.


# <a name="uwp"></a>[UWP](#tab/uwp)

Das Aufrufen von `RequestedTheme` muss über den Benutzeroberflächenthread erfolgen. Andernfalls wird eine Ausnahme ausgelöst.

UWP-Anwendungen beachten ihre Einstellung in der UWP-Datei „App.xaml“ unter **RequestedTheme**. Wenn die Option auf ein bestimmtes Design festgelegt ist, gibt Xamarin.Essentials immer diese Einstellung zurück. Um das dynamische Design des Betriebssystems zu verwenden, entfernen Sie diesen Knoten aus Ihrer Anwendung, und Ihre App gibt dann, wenn sie ausgeführt wird, das vom Benutzer in den Windows-Einstellungen (**Einstellungen > Personalisierung > Farben > Standard-App-Modus auswählen**) festgelegte Design zurück.

Weitere In finden Sie in der [UWP-Dokumentation zum angeforderten Design](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.requestedtheme).

--------------

## <a name="api"></a>API

- [AppInfo-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [AppInfo-API-Dokumentation](xref:Xamarin.Essentials.AppInfo)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Theme-Detection-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
