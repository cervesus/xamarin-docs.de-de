---
title: Anwendungsindizierung und Deep Linking
description: In diesem Artikel wird erläutert, wie Sie mit der Anwendungsindizierung und Deep Linking die Suche nach Inhalten von Xamarin.Forms-Anwendungen auf iOS- und Android-Geräten ermöglichen können.
ms.prod: xamarin
ms.assetid: 410C5D19-AA3C-4E0D-B799-E288C5803226
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 11/28/2018
ms.openlocfilehash: c4e634ce51080ad38b093e1355767c73c72e837a
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2019
ms.locfileid: "54208056"
---
# <a name="application-indexing-and-deep-linking"></a>Anwendungsindizierung und Deep Linking

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/deeplinking/)

_Mit der Anwendungsindizierung können Anwendungen, die andernfalls nach einigem Gebrauch vergessen würden, relevant bleiben, indem sie in den Suchergebnissen angezeigt werden. Mit Deep Linking können Anwendungen auf ein Suchergebnis reagieren, das Anwendungsdaten enthält, in der Regel durch die Navigation zu einer Seite, auf die über einen Deep-Link verwiesen wird. In diesem Artikel wird erläutert, wie Sie mit der Anwendungsindizierung und Deep Linking die Suche nach Inhalten von Xamarin.Forms-Anwendungen auf iOS- und Android-Geräten ermöglichen können._

> [!VIDEO https://youtube.com/embed/UJv4jUs7cJw]

**Deep Linking mit Xamarin.Forms und Azure, [Video der Xamarin University](https://university.xamarin.com/)** (in englischer Sprache)


Die Funktion „Anwendungsindizierung und Deep Linking“ von Xamarin.Forms stellen eine API für die Veröffentlichung von Metadaten für die Anwendungsindizierung bereit, wenn Benutzer durch Anwendungen navigieren. In der Spotlight-Suche, der Google-Suche oder in einer Websuche kann dann nach indizierten Inhalten gesucht werden. Durch das Tippen auf ein Suchergebnis, das einen Deep-Link enthält, wird ein Ereignis ausgelöst, das von einer Anwendung verarbeitet werden kann und in der Regel zum Navigieren zu der Seite verwendet wird, auf welche der Deep-Link verweist.

In dieser Beispielanwendung wird eine Anwendung mit Aufgabenlisten (ToDoList) veranschaulicht, bei der die Daten in einer lokalen SQLite-Datenbank gespeichert sind, wie aus den folgenden Screenshots hervorgeht:

![](deep-linking-images/screenshots.png "Aufgabenlisten-Anwendung")

Jede vom Benutzer erstellte `TodoItem`-Instanz wird indiziert. Anschließend kann die plattformspezifische Suche zum Auffinden der indizierten Daten aus der Anwendung verwendet werden. Wenn der Benutzer in den Suchergebnissen auf ein Element für die Anwendung tippt, wird die Anwendung gestartet, die Seite `TodoItemPage` wird aufgerufen, und die `TodoItem`-Instanz, auf die der Deep-Link verweist, wird angezeigt.

Weitere Informationen zur Verwendung einer SQLite-Datenbank finden Sie unter [Lokale Datenbanken von Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md).

> [!NOTE]
> Die Funktion „Anwendungsindizierung und Deep Linking“ von Xamarin.Forms ist nur auf iOS- und Android-Plattformen verfügbar und setzt mindestens iOS 9 bzw. API 23 voraus.

## <a name="setup"></a>Setup

Die folgenden Abschnitte enthalten alle weiteren Installationsanweisungen für die Verwendung dieses Features auf iOS- und Android-Plattformen.

### <a name="ios"></a>iOS

Stellen Sie auf der iOS-Plattform sicher, dass in Ihrem iOS-Projekt die Datei **Entitlements.plist** als benutzerdefinierte Berechtigungsdatei für die Signatur des Pakets festgelegt ist.

So verwenden Sie universelle iOS-Links

1. Fügen Sie Ihrer App die Berechtigung „Zugehörige Bereiche“ mit dem Schlüssel `applinks` und allen Domänen, die Ihre App unterstützen soll, hinzu.
1. Fügen Sie Ihrer Website eine AASA-Datei (Apple App Site Association) hinzu.
1. Fügen Sie der AASA-Datei den Schlüssel `applinks` hinzu.

Weitere Informationen finden Sie unter [Allowing Apps and Websites to Link to Your Content](https://developer.apple.com/documentation/uikit/core_app/allowing_apps_and_websites_to_link_to_your_content) (Apps und Websites die Verknüfung mit Ihren Inhalten erlauben) auf developer.apple.com.

### <a name="android"></a>Android

Auf der Android-Plattform sind eine Reihe von Voraussetzungen zu erfüllen, bevor Sie die Funktion „Anwendungsindizierung und Deep Linking“ verwenden können:

1. Eine Version der Anwendung muss in Google Play aktiv sein.
1. Eine begleitende Website muss für die Anwendung in der Google Developers Console registriert sein. Sobald die Anwendung einer Website zugeordnet ist, können URLs indiziert werden, die sowohl für die Website als auch die Anwendung gelten und dann in den Suchergebnissen bereitgestellt werden können. Weitere Informationen finden Sie auf der Google-Website [App-Indexierung für die Google-Suche](https://support.google.com/googleplay/android-developer/answer/6041489).
1. Ihre Anwendung muss HTTP-URL-Intents für die `MainActivity`-Klasse unterstützen, die der Anwendungsindizierung mitteilen, auf welche Art von URL-Datenschemas die Anwendung reagieren kann. Weitere Informationen finden Sie unter [Konfigurieren des Intent-Filters](~/android/platform/app-linking.md#configure-intent-filter).

Wenn diese Voraussetzungen erfüllt sind, ist die folgende zusätzliche Konfiguration erforderlich, um Anwendungsindizierung und Deep Linking von Xamarin.Forms auf der Android-Plattform zu verwenden:

1. Installieren Sie das NuGet-Paket [Xamarin.Forms.AppLinks](https://www.nuget.org/packages/Xamarin.Forms.AppLinks/) im Android-Anwendungsprojekt.
1. Fügen Sie in der Datei **MainActivity.cs** eine Deklaration zur Verwendung des Namespace `Xamarin.Forms.Platform.Android.AppLinks` hinzu.
1. Fügen Sie in der Datei **MainActivity.cs** eine Deklaration zur Verwendung des Namespace `Firebase` hinzu.
1. Erstellen Sie in einem Webbrowser über die [Firebase-Konsole](https://console.firebase.google.com/) ein neues Projekt.
1. Fügen Sie in der Firebase-Konsole Firebase zu Ihrer Android-App hinzu, und geben Sie die erforderlichen Daten ein.
1. Laden Sie die resultierende Datei **google-services.json** herunter.
1. Fügen Sie die Datei **google-services.json** im Stammverzeichnis des Android-Projekts hinzu, und legen Sie den **Buildvorgang** auf **GoogleServicesJson** fest.
1. Fügen Sie in der `MainActivity.OnCreate`-Methodenüberschreibung unter `Forms.Init(this, bundle)` die folgende Codezeile hinzu:

```csharp
FirebaseApp.InitializeApp(this);
AndroidAppLinks.Init(this);
```

Wenn die Datei **google-services.json** dem Projekt hinzugefügt wird (und der Buildvorgang auf *GoogleServicesJson** festgelegt ist), extrahiert der Buildvorgang die Client-ID und den API-Schlüssel und fügt dann diese Anmeldeinformationen der generierten Manifestdatei hinzu.

Weitere Informationen finden Sie unter [Deep Link Content with Xamarin.Forms URL Navigation](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) (Deep Linking von Inhalten mit der URL-Navigation von Xamarin.Forms) im Xamarin-Blog.

## <a name="indexing-a-page"></a>Indizierung einer Seite

Folgender Vorgang wird zum Indizieren einer Seite und zum Offenlegen für die Google- und Spotlight-Suche verwendet:

1. Erstellen Sie eine [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry)-Klasse, die die zum Indizieren der Seite erforderlichen Metadaten und einen Deep-Link enthält, der auf die Seite zurückverweist, wenn der Benutzer den indizierten Inhalt in den Suchergebnissen auswählt.
1. Registrieren Sie die [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry)-Instanz, um sie für die Suche zu indizieren.

Im folgenden Codebeispiel wird veranschaulicht, wie Sie eine [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry)-Instanz erstellen:

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

Die [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry)-Instanz enthält eine Reihe von Eigenschaften, deren Werte erforderlich sind, um die Seite zu indizieren und einen Deep-Link zu erstellen. Die Eigenschaften [`Title`](xref:Xamarin.Forms.IAppLinkEntry.Title), [`Description`](xref:Xamarin.Forms.IAppLinkEntry.Description) und [`Thumbnail`](xref:Xamarin.Forms.IAppLinkEntry.Thumbnail) werden zum Identifizieren des indizierten Inhalts verwendet, wenn er in den Suchergebnissen angezeigt wird. Die [`IsLinkActive`](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive)-Eigenschaft wird auf `true` festgelegt, um anzugeben, dass der indizierte Inhalt derzeit angezeigt wird. Die [`AppLinkUri`](xref:Xamarin.Forms.IAppLinkEntry.AppLinkUri)-Eigenschaft ist ein `Uri` und enthält die Informationen, die für die Rückkehr zur aktuellen Seite und zum Anzeigen der aktuellen `TodoItem`-Instanz erforderlich sind. Das folgende Beispiel zeigt einen Beispiel-`Uri` für die Beispielanwendung:

```csharp
http://deeplinking/DeepLinking.TodoItemPage?id=2
```

Dieser `Uri` enthält alle Informationen, die zum Starten der `deeplinking`-App, zum Navigieren zur Seite `DeepLinking.TodoItemPage` und zum Anzeigen der `TodoItem`-Instanz mit der `ID` = 2 erforderlich sind.

## <a name="registering-content-for-indexing"></a>Registrieren von Inhalt für die Indizierung

Wenn eine [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry)-Instanz erstellt wurde, muss sie für die Indizierung registriert werden, damit sie in den Suchergebnissen angezeigt wird. Dies erfolgt mit der [`RegisterLink`](xref:Xamarin.Forms.IAppLinks.RegisterLink(Xamarin.Forms.IAppLinkEntry))-Methode, wie aus dem folgenden Codebeispiel hervorgeht:

```csharp
Application.Current.AppLinks.RegisterLink (appLink);
```

Dadurch wird die [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry)-Instanz der [`AppLinks`](xref:Xamarin.Forms.Application.AppLinks)-Sammlung der Anwendung hinzugefügt.

> [!NOTE]
> Mit der `RegisterLink`-Methode können Sie auch den für eine Seite indizierten Inhalt aktualisieren.

Wenn Sie die [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry)-Instanz für die Indizierung registriert haben, kann sie in den Suchergebnissen angezeigt werden. Der folgende Screenshot zeigt den indizierten Inhalt, wie er auf der iOS-Plattform in den Suchergebnissen angezeigt wird:

![](deep-linking-images/ios-search.png "Indizierter Inhalt in den Suchergebnissen unter iOS")

## <a name="de-registering-indexed-content"></a>Aufheben der Registrierung von indizierten Inhalten

Mit der [`DeregisterLink`](xref:Xamarin.Forms.IAppLinks.DeregisterLink(Xamarin.Forms.IAppLinkEntry))-Methode können Sie indizierte Inhalte aus den Suchergebnissen entfernen, wie aus dem folgenden Codebeispiel hervorgeht:

```csharp
Application.Current.AppLinks.DeregisterLink (appLink);
```

Dadurch wird die [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry)-Instanz aus der [`AppLinks`](xref:Xamarin.Forms.Application.AppLinks)-Sammlung der Anwendung entfernt.

> [!NOTE]
> Unter Android ist es nicht möglich, indizierten Inhalt aus den Suchergebnissen zu entfernen.

<a name="responding" />

## <a name="responding-to-a-deep-link"></a>Reagieren auf einen Deep-Link

Wenn indizierter Inhalt in den Suchergebnissen angezeigt und von einem Benutzer ausgewählt wird, erhält die `App`-Klasse für die Anwendung eine Anforderung zum Verarbeiten des `Uri`, der in dem indizierten Inhalt enthalten ist. Diese Anforderung kann in der [`OnAppLinkRequestReceived`](xref:Xamarin.Forms.Application.OnAppLinkRequestReceived(System.Uri))-Methodenüberschreibung verarbeitet werden, wie im folgenden Codebeispiel gezeigt:

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

Die [`OnAppLinkRequestReceived`](xref:Xamarin.Forms.Application.OnAppLinkRequestReceived(System.Uri))-Methode überprüft, ob der empfangene `Uri` für die Anwendung bestimmt ist. Erst dann analysiert sie die Seite, zu der mit dem `Uri` navigiert werden soll, und den Parameter, welcher der Seite übergeben werden soll. Es wird eine Instanz der Seite erstellt, zu der navigiert werden soll, und die vom Seitenparameter dargestellte `TodoItem`-Instanz wird abgerufen. [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) wird auf der Seite, zu der navigiert werden soll, auf `TodoItem` gesetzt. So wird sichergestellt, dass bei der Anzeige von `TodoItemPage` mit der [`PushAsync`](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page))-Methode die `TodoItem`-Instanz angezeigt wird, deren `ID` sich im Deep-Link befindet.

## <a name="making-content-available-for-search-indexing"></a>Bereitstellen von Inhalt für die Suchindizierung

Jedes Mal, wenn die durch einen Deep-Link dargestellte Seite angezeigt wird, kann die [`AppLinkEntry.IsLinkActive`](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive)-Eigenschaft auf `true` festgelegt werden. Unter iOS und Android wird dadurch die [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry)-Instanz für die Suchindizierung bereitgestellt. Nur unter iOS wird zusätzlich auch die `AppLinkEntry`-Instanz für das Handoff verfügbar gemacht. Weitere Informationen zum Handoff finden Sie unter [Handoff in Xamarin.iOS](~/ios/platform/handoff.md).

Das folgende Codebeispiel zeigt, wie die [`AppLinkEntry.IsLinkActive`](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive)-Eigenschaft in der [`Page.OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing)-Methodenüberschreibung auf `true` festgelegt wird:

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

In ähnlicher Weise kann die [`AppLinkEntry.IsLinkActive`](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive)-Eigenschaft auf `false` festgelegt werden, wenn bei der Navigation die von einem Deep-Link dargestellte Seite verlassen wird. Unter iOS und Android wird dadurch die Ankündigung der [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry)-Instanz für die Suchindizierung beendet. Nur unter iOS wird zusätzlich auch die Ankündigung der `AppLinkEntry`-Instanz für das Handoff beendet. Dies kann in der [`Page.OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing)-Methodenüberschreibung verarbeitet werden, wie im folgenden Codebeispiel gezeigt:

```csharp
protected override void OnDisappearing()
{
    if (appLink != null)
    {
        appLink.IsLinkActive = false;
    }
}
```

## <a name="providing-data-to-handoff"></a>Bereitstellen von Daten für das Handoff

Unter iOS können anwendungsspezifische Daten beim Indizieren der Seite gespeichert werden. Dies erfolgt durch Hinzufügen von Daten zur [`KeyValues`](xref:Xamarin.Forms.IAppLinkEntry.KeyValues)-Sammlung, ein `Dictionary<string, string>` zum Speichern von Schlüssel-Wert-Paaren, die beim Handoff verwendet werden. „Handoff“ ist eine Funktion, mit welcher der Benutzer eine Aktivität auf einem seiner Geräte beginnen und auf einem anderen Gerät (nach Identifizierung durch das iCloud-Benutzerkonto) fortsetzen kann. Der folgende Code zeigt ein Beispiel für das Speichern von anwendungsspezifischen Schlüssel-Wert-Paaren:

```csharp
var pageLink = new AppLinkEntry
{
    ...
};
pageLink.KeyValues.Add("appName", App.AppName);
pageLink.KeyValues.Add("companyName", "Xamarin");
```

In der [`KeyValues`](xref:Xamarin.Forms.IAppLinkEntry.KeyValues)-Sammlung gespeicherte Werte werden in den Metadaten für die indizierte Seite gespeichert und wiederhergestellt, wenn der Benutzer auf ein Suchergebnis tippt, das einen Deep-Link enthält (oder wenn er zum Anzeigen des Inhalts auf einem anderen angemeldeten Gerät die Handoff-Funktion verwendet).

Darüber hinaus können Werte für die folgenden Schlüssel angegeben werden:

- `contentType`: Ein `string` zur Angabe des einheitlichen Typbezeichners des indizierten Inhalts. Die empfohlene Konvention für die Verwendung dieses Werts ist der Typname der Seite, die den indizierten Inhalt enthält.
- `associatedWebPage`: Ein `string`, der die zu besuchende Webseite darstellt, wenn der indizierte Inhalt auch im Web angezeigt werden kann, oder wenn die Anwendung Deep-Links von Safari unterstützt.
- `shouldAddToPublicIndex`: Ein `string` mit dem Wert `true` oder `false`, der steuert, ob der indizierte Inhalt dem Index der öffentlichen iCloud von Apple hinzugefügt werden soll, der dann auch Benutzern angezeigt wird, welche die Anwendung nicht auf ihrem iOS-Gerät installiert haben. Auch wenn Sie Inhalte für die öffentliche Indizierung festgelegt haben, bedeutet das nicht, dass sie automatisch dem Index der öffentlichen iCloud von Apple hinzugefügt werden. Weitere Informationen finden Sie unter [Indizieren für die öffentliche Suche](~/ios/platform/search/nsuseractivity.md). Beachten Sie, dass dieser Schlüssel auf `false` festgelegt werden sollte, wenn Sie der [`KeyValues`](xref:Xamarin.Forms.IAppLinkEntry.KeyValues)-Sammlung persönliche Daten hinzufügen.

> [!NOTE]
> Die `KeyValues`-Sammlung wird auf der Android-Plattform nicht verwendet.

Weitere Informationen zum Handoff finden Sie unter [Handoff in Xamarin.iOS](~/ios/platform/handoff.md).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird erläutert, wie Sie mit der Anwendungsindizierung und Deep Linking die Suche nach Inhalten von Xamarin.Forms-Anwendungen auf iOS- und Android-Geräten ermöglichen können. Mit der Anwendungsindizierung können Anwendungen, die andernfalls nach einigem Gebrauch vergessen würden, relevant bleiben, indem sie in den Suchergebnissen angezeigt werden.

## <a name="related-links"></a>Verwandte Links

- [Deep Linking (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/deeplinking/)
- [Such-APIs in Xamarin.iOS](~/ios/platform/search/index.md)
- [App-Verknüpfung in Android 6.0](~/android/platform/app-linking.md)
- [AppLinkEntry](xref:Xamarin.Forms.AppLinkEntry)
- [IAppLinkEntry](xref:Xamarin.Forms.IAppLinkEntry)
- [IAppLinks](xref:Xamarin.Forms.IAppLinks)
