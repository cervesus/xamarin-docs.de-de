---
title: Anwendungsindizierung und Deep Linking
description: In diesem Artikel wird erläutert, wie mit anwendungsindizierung und deep linking zum Inhalt von Xamarin.Forms-Anwendung unter iOS und Android-Geräte durchsuchbar zu machen.
ms.prod: xamarin
ms.assetid: 410C5D19-AA3C-4E0D-B799-E288C5803226
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 11/28/2018
ms.openlocfilehash: f73760e2dc2310a9c1cd7a63a03ead37283a415f
ms.sourcegitcommit: 215cad17324ba3fbc23487ce66cd4e1cc74eb879
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/30/2018
ms.locfileid: "52710009"
---
# <a name="application-indexing-and-deep-linking"></a>Anwendungsindizierung und Deep Linking

_Anwendungsindizierung ermöglicht Anwendungen, die andernfalls vergessen werden würde, nach ein Paar verwendet, um gefragt zu bleiben, indem Sie in den Suchergebnissen angezeigt wird. Deep Links ermöglicht Anwendungen, die auf ein Suchergebnis reagieren, die Anwendungsdaten, in der Regel enthält, navigieren Sie zu einer Seite, auf die über einen deep-Link verwiesen wird. In diesem Artikel wird erläutert, wie mit anwendungsindizierung und deep linking zum Inhalt von Xamarin.Forms-Anwendung unter iOS und Android-Geräte durchsuchbar zu machen._

> [!VIDEO https://youtube.com/embed/UJv4jUs7cJw]

**Tiefe durch Verknüpfen mit Xamarin.Forms und Azure [Xamarin University](https://university.xamarin.com/)**


Xamarin.Forms-anwendungsindizierung und deep linking geben eine API für die Veröffentlichung von Metadaten für die anwendungsindizierung, wie Benutzer durch Anwendungen navigieren. Indizierten Inhalte kann dann in Spotlight-Suche, in der Google-Suche oder in eine Websuche durchsucht werden. Tippen auf ein Suchergebnis, das einen deep-Link enthält, wird ein Ereignis ausgelöst, die von einer Anwendung verarbeitet werden können, und dient normalerweise zum Navigieren zur Seite auf die von der deep-Link verwiesen wird.

Die beispielanwendung zeigt eine Todolist-Anwendung, in denen die Daten in einer lokalen SQLite-Datenbank gespeichert werden, wie in den folgenden Screenshots gezeigt:

![](deep-linking-images/screenshots.png "\"Todolist\"-Anwendung")

Jede `TodoItem` Instanz, die vom Benutzer erstellten wird indiziert. Clientplattform-spezifische Suchfeld kann dann zum Suchen der indizierter Daten aus der Anwendung verwendet werden. Wenn der Benutzer auf ein Ergebniselement suchen, für die Anwendung tippt, wird die Anwendung gestartet, die `TodoItemPage` navigiert wird, und die `TodoItem` verwiesen wird, aus der Link wird angezeigt.

Weitere Informationen zur Verwendung einer SQLite-Datenbank finden Sie unter [Xamarin.Forms lokale Datenbanken](~/xamarin-forms/app-fundamentals/databases.md).

> [!NOTE]
> Xamarin.Forms-anwendungsindizierung und Deep linking-Funktion ist nur auf IOS- und Android-Plattformen verfügbar und erfordert das jeweils mindestens iOS 9 und API-23.

## <a name="setup"></a>Setup

Die folgenden Abschnitte enthalten weitere installationsanweisungen für die Verwendung dieses Features auf IOS- und Android-Plattformen.

### <a name="ios"></a>iOS

Stellen Sie sicher, dass Ihr iOS-Plattform-Projekt festlegt, auf der iOS-Plattform die **"Entitlements.plist"** Datei wie die Datei "benutzerdefinierte Berechtigungen" für das Signieren des Pakets.

IOS universelle Links verwenden zu können:

1. Fügen Sie eine zugehörige Bereiche-Berechtigung für Ihre app, mit der `applinks` Schlüssel, einschließlich aller Domänen an Ihre app unterstützt wird.
1. Fügen Sie einer Sitezuordnung für Apple-App-Datei auf Ihrer Website.
1. Hinzufügen der `applinks` -Schlüssel in der Sitezuordnung für Apple-App-Datei.

Weitere Informationen finden Sie unter [können Apps und Websites, um eine Verknüpfung zu Ihrer Inhalte](https://developer.apple.com/documentation/uikit/core_app/allowing_apps_and_websites_to_link_to_your_content) auf developer.apple.com.

### <a name="android"></a>Android

Es gibt zahlreiche Voraussetzungen erfüllt sein müssen, um anwendungsindizierung und Deep linking Funktionen verwenden, auf der Android-Plattform:

1. Eine Version der Anwendung muss auf Google Play aktiv sein.
1. Eine begleitende Website muss für die Anwendung in Google Entwicklerkonsole registriert werden. URLs dürfen nicht, sobald die Anwendung auf einer Website zugeordnet wird, indiziert die Arbeit für die Website und die Anwendung, die in den Suchergebnissen dann bereitgestellt werden kann. Weitere Informationen finden Sie unter [App Indexing auf der Google-Suche](https://support.google.com/googleplay/android-developer/answer/6041489) Googles-Website.
1. Ihre Anwendung muss HTTP-URL Intents unterstützen, auf die `MainActivity` -Klasse, die Anwendung, die Indizierung für welche Arten von Daten URL-Schemas der Anwendung mitgeteilt auf reagieren kann. Weitere Informationen finden Sie unter [Konfigurieren des Filters Absicht](~/android/platform/app-linking.md#configure-intent-filter).

Wenn diese Voraussetzungen erfüllt sind, ist die folgende zusätzliche Konfiguration erforderlich, um Xamarin.Forms anwendungsindizierung und deep links auf der Android-Plattform zu verwenden:

1. Installieren Sie die [Xamarin.Forms.AppLinks](https://www.nuget.org/packages/Xamarin.Forms.AppLinks/) NuGet-Paket in das Projekt für Android-Anwendung.
1. In der **"mainactivity.cs"** hinzufügen. eine Deklaration mit der `Xamarin.Forms.Platform.Android.AppLinks` Namespace.
1. In der **"mainactivity.cs"** hinzufügen. eine Deklaration mit der `Firebase` Namespace.
1. Erstellen Sie in einem Webbrowser eines neuen Projekts über die [Firebase-Konsole](https://console.firebase.google.com/).
1. Klicken Sie in der Firebase-Konsole hinzufügen von Firebase zu Ihrer Android-app, und geben Sie die erforderlichen Daten.
1. Laden Sie die resultierende **Google-services.json** Datei.
1. Hinzufügen der **Google-services.json** Datei in das Stammverzeichnis des Android-Projekts, und legen dessen **Buildvorgang** zu **GoogleServicesJson**.
1. In der `MainActivity.OnCreate` außer Kraft setzen, fügen Sie folgenden Code unter `Forms.Init(this, bundle)`:

```csharp
FirebaseApp.InitializeApp(this);
AndroidAppLinks.Init(this);
```

Wenn **Google-services.json** wird dem Projekt hinzugefügt (und die *GoogleServicesJson** Buildvorgang festgelegt ist), der Buildprozess extrahiert die Client-ID und API-Schlüssel und fügt dann die Anmeldeinformationen der manifest-Datei generiert.

Weitere Informationen finden Sie unter [Deep Links Inhalt mit Xamarin.Forms für URL-Navigation](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) im Xamarin-Blog.

## <a name="indexing-a-page"></a>Indizierung einer Seite

Der Prozess für die Indizierung von einer Seite, und eine Offenlegung für Google und Spotlight-Suche ist wie folgt:

1. Erstellen Sie eine [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) , enthält die Metadaten, die erforderlich sind, indizieren Sie die Seite und einen deep-Link auf der Seite zurückgegeben werden soll, wenn der Benutzer die indizierte Inhalte in den Suchergebnissen auswählt.
1. Registrieren der [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) Instanz, die sie für die Suche indiziert.

Im folgenden Codebeispiel wird veranschaulicht, wie zum Erstellen einer [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) Instanz:

```csharp
AppLinkEntry GetAppLink(TodoItem item)
{
    var pageType = GetType().ToString();
    var pageLink = new AppLinkEntry
    {
        Title = item.Name,
        Description = item.Notes,
        AppLinkUri = new Uri($"http://{App.AppName}/{pageType}?id={item.ID}", UriKind.RelativeOrAbsolute),
        IsLinkActive = true,
        Thumbnail = ImageSource.FromFile("monkey.png")
    };

    pageLink.KeyValues.Add("contentType", "TodoItemPage");
    pageLink.KeyValues.Add("appName", App.AppName);
    pageLink.KeyValues.Add("companyName", "Xamarin");

    return pageLink;
}
```

Die [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) Instanz enthält eine Reihe von Eigenschaften, deren Werte erforderlich sind, um die Indexseite, und erstellen Sie einen deep-Link. Die [ `Title` ](xref:Xamarin.Forms.IAppLinkEntry.Title), [ `Description` ](xref:Xamarin.Forms.IAppLinkEntry.Description), und [ `Thumbnail` ](xref:Xamarin.Forms.IAppLinkEntry.Thumbnail) Eigenschaften werden verwendet, um die indizierten Inhalte zu identifizieren, wenn sie in den Suchergebnissen angezeigt wird. Die [ `IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) -Eigenschaftensatz auf `true` um anzugeben, dass die indizierte Inhalte derzeit angezeigt wird. Die [ `AppLinkUri` ](xref:Xamarin.Forms.IAppLinkEntry.AppLinkUri) -Eigenschaft ist eine `Uri` , enthält die erforderlichen Informationen zum Zurückgeben der aktuellen Seite und Anzeigen der aktuellen `TodoItem`. Das folgende Beispiel zeigt ein Beispiel für `Uri` für die beispielanwendung:

```csharp
http://deeplinking/DeepLinking.TodoItemPage?id=2
```

Dies `Uri` enthält alle erforderlichen Informationen zum Starten der `deeplinking` -app navigieren Sie zu der `DeepLinking.TodoItemPage`, und zeigen die `TodoItem` , bei dem ein `ID` von 2.

## <a name="registering-content-for-indexing"></a>Registrieren von Inhalt für die Indizierung

Sobald ein [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) Instanz erstellt wurde, er muss registriert werden, für die Indizierung in den Suchergebnissen angezeigt werden. Dies erfolgt mit der [ `RegisterLink` ](xref:Xamarin.Forms.IAppLinks.RegisterLink(Xamarin.Forms.IAppLinkEntry)) -Methode, wie im folgenden Codebeispiel gezeigt:

```csharp
Application.Current.AppLinks.RegisterLink (appLink);
```

Dadurch wird die [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) Instanz mit der Anwendung [ `AppLinks` ](xref:Xamarin.Forms.Application.AppLinks) Auflistung.

> [!NOTE]
> Die `RegisterLink` Methode kann auch verwendet werden, um den Inhalt zu aktualisieren, die für eine Seite indiziert wurden, ist.

Sobald ein [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) Instanz registriert wurde für die Indizierung können sie in den Suchergebnissen angezeigt. Der folgende Screenshot zeigt die indizierten Inhalte angezeigt werden, in den Suchergebnissen auf das iOS-Plattform:

![](deep-linking-images/ios-search.png "Indizierten Inhalte in den Suchergebnissen unter iOS")

## <a name="de-registering-indexed-content"></a>Aufheben der Registrierung Inhalt indiziert

Die [ `DeregisterLink` ](xref:Xamarin.Forms.IAppLinks.DeregisterLink(Xamarin.Forms.IAppLinkEntry)) Methode dient zum indizierten Inhalte aus den Suchergebnissen angezeigt wird, zu entfernen, wie im folgenden Codebeispiel gezeigt:

```csharp
Application.Current.AppLinks.DeregisterLink (appLink);
```

Dies entfernt die [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) -Instanz, aus der Anwendung [ `AppLinks` ](xref:Xamarin.Forms.Application.AppLinks) Auflistung.

> [!NOTE]
> Unter Android ist es nicht möglich, indizierten Inhalte aus den Suchergebnissen zu entfernen.

<a name="responding" />

## <a name="responding-to-a-deep-link"></a>Reagieren auf einen Deep-Link

Wenn indizierten Inhalte in Suchergebnissen angezeigt wird, und vom Benutzer ausgewählt wurde wird, die `App` -Klasse für die Anwendung eine Anforderung zum Verarbeiten erhält die `Uri` in der indiziert Inhalt enthalten. Diese Anforderung verarbeitet werden kann die [ `OnAppLinkRequestReceived` ](xref:Xamarin.Forms.Application.OnAppLinkRequestReceived(System.Uri)) zu überschreiben, wie im folgenden Codebeispiel gezeigt:

```csharp
public class App : Application
{
    ...
    protected override async void OnAppLinkRequestReceived(Uri uri)
    {
        string appDomain = "http://" + App.AppName.ToLowerInvariant() + "/";
        if (!uri.ToString().ToLowerInvariant().StartsWith(appDomain, StringComparison.Ordinal))
            return;

        string pageUrl = uri.ToString().Replace(appDomain, string.Empty).Trim();
        var parts = pageUrl.Split('?');
        string page = parts[0];
        string pageParameter = parts[1].Replace("id=", string.Empty);

        var formsPage = Activator.CreateInstance(Type.GetType(page));
        var todoItemPage = formsPage as TodoItemPage;
        if (todoItemPage != null)
        {
            var todoItem = await App.Database.GetItemAsync(int.Parse(pageParameter));
            todoItemPage.BindingContext = todoItem;
            await MainPage.Navigation.PushAsync(formsPage as Page);
        }
        base.OnAppLinkRequestReceived(uri);
    }
}
```

Die [ `OnAppLinkRequestReceived` ](xref:Xamarin.Forms.Application.OnAppLinkRequestReceived(System.Uri)) Methode überprüft, ob die empfangenen Daten `Uri` richtet sich die Anwendung vor der Analyse der `Uri` in die Seite, zu der navigiert werden und die Parameter, der auf der Seite übergeben werden. Eine Instanz der Seite für die leichte Navigation wird erstellt, und die `TodoItem` dargestellten Parameter wird von der Seite abgerufen. Die [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) der Seite, um Sie zu der navigiert wird festgelegt die `TodoItem`. So wird sichergestellt, dass bei der `TodoItemPage` wird angezeigt, die [ `PushAsync` ](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page)) -Methode, sie werde dir die `TodoItem` , deren `ID` befindet sich in der deep-Link.

## <a name="making-content-available-for-search-indexing"></a>Bereitstellen von Inhalt für die Search-Indizierung

Jedes Mal die Seite, die durch einen deep-Link dargestellt wird, angezeigt wird, die [ `AppLinkEntry.IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) Eigenschaft kann festgelegt werden, um `true`. Unter iOS und Android dadurch die [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) Instanz verfügbar, für die Search-Indizierung und nur unter iOS, sondern Sie macht auch das `AppLinkEntry` Instanz, die für die Übergabe verfügbar. Weitere Informationen zu Übergabeanimationen finden Sie unter [Einführung in die Übergabe](~/ios/platform/handoff.md).

Das folgende Codebeispiel veranschaulicht das Festlegen der [ `AppLinkEntry.IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) Eigenschaft `true` in die [ `Page.OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) außer Kraft setzen:

```csharp
protected override void OnAppearing()
{
    appLink = GetAppLink(BindingContext as TodoItem);
    if (appLink != null)
    {
        appLink.IsLinkActive = true;
    }
}
```

Auf ähnliche Weise, wenn die Seite, die durch einen deep-Link dargestellt wird weg navigiert, die [ `AppLinkEntry.IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) Eigenschaft kann festgelegt werden, um `false`. Unter iOS und Android wird verhindert, dass die [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) beendet die Instanz, die Ankündigung für die Search-Indizierung, und klicken Sie auf iOS nur It auch Werbung die `AppLinkEntry` Instanz übergeben. Dies kann erreicht werden, der [ `Page.OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) zu überschreiben, wie im folgenden Codebeispiel gezeigt:

```csharp
protected override void OnDisappearing()
{
    if (appLink != null)
    {
        appLink.IsLinkActive = false;
    }
}
```

## <a name="providing-data-to-handoff"></a>Bereitstellen von Daten für die Übergabe

Unter iOS können das anwendungsspezifische Daten beim Indizieren von der Seite gespeichert werden. Dies erfolgt durch Hinzufügen von Daten zu den [ `KeyValues` ](xref:Xamarin.Forms.IAppLinkEntry.KeyValues) Auflistung, die eine `Dictionary<string, string>` zum Speichern von Schlüssel-Wert-Paare, mit denen in übergeben. Übergabe kann der Benutzer eine Aktivität auf einem von ihren Geräten starten und weiterhin diese Aktivität auf einem anderen Geräten, (, das durch die iCloud-Konto des Benutzers). Der folgende Code zeigt ein Beispiel für anwendungsspezifische Schlüssel-Wert-Paare speichern:

```csharp
var pageLink = new AppLinkEntry
{
    ...
};
pageLink.KeyValues.Add("appName", App.AppName);
pageLink.KeyValues.Add("companyName", "Xamarin");
```

In gespeicherten Werte der [ `KeyValues` ](xref:Xamarin.Forms.IAppLinkEntry.KeyValues) Sammlung wird in den Metadaten für die indizierte Seite gespeichert werden, und wird wiederhergestellt, wenn der Benutzer tippt auf ein Suchergebnis, das einen deep-Link enthält (oder wenn der Übergabe an den Inhalt auf einem anderen verwendet wird, angemeldeten Geräte).

Darüber hinaus können die Werte für die folgenden Schlüssel angegeben werden:

- `contentType` – ein `string` zur Angabe des Bezeichners denselben Typ, der die indizierten Inhalte. Die empfohlene Konvention für die Verwendung dieses Werts ist der Typname der Seite, die den indizierten Inhalt enthält.
- `associatedWebPage` – ein `string` , das die Webseite besuchen, wenn die indizierte Inhalte auch im Web angezeigt werden können oder wenn die Anwendung deep-Links von Safari unterstützt darstellt.
- `shouldAddToPublicIndex` – ein `string` entweder `true` oder `false` , die steuert, ob den indizierten Inhalt von Apple öffentlichen Cloud-Index, hinzufügen, die dann Benutzern angezeigt werden können, die die Anwendung nicht auf ihrem iOS-Gerät installiert haben. Allerdings nur, weil Sie Inhalte für die Indizierung von öffentlichen festgelegt wurde, bedeutet nicht, dass es automatisch von Apple öffentliche Cloud Index hinzugefügt werden. Weitere Informationen finden Sie unter [öffentliche Suchindizierung](~/ios/platform/search/nsuseractivity.md). Beachten Sie, dass dieser Schlüssel werden sollte, `false` Hinzufügen von persönlichen Daten, die [ `KeyValues` ](xref:Xamarin.Forms.IAppLinkEntry.KeyValues) Auflistung.

> [!NOTE]
> Die `KeyValues` Collection wird nicht auf die Android-Plattform verwendet.

Weitere Informationen zu Übergabeanimationen finden Sie unter [Einführung in die Übergabe](~/ios/platform/handoff.md).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird erläutert, wie mit anwendungsindizierung und deep linking zum Inhalt von Xamarin.Forms-Anwendung unter iOS und Android-Geräte durchsuchbar zu machen. Anwendungsindizierung kann Anwendungen relevant bleiben, indem Sie in den Suchergebnissen aus, die über andernfalls vergessen werden würde, nachdem ein Paar verwendet angezeigt werden.

## <a name="related-links"></a>Verwandte Links

- [Deep-Linking-(Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/deeplinking/)
- [iOS-Suche-APIs](~/ios/platform/search/index.md)
- [App-Verknüpfung in Android 6.0](~/android/platform/app-linking.md)
- [AppLinkEntry](xref:Xamarin.Forms.AppLinkEntry)
- [IAppLinkEntry](xref:Xamarin.Forms.IAppLinkEntry)
- [IAppLinks](xref:Xamarin.Forms.IAppLinks)
