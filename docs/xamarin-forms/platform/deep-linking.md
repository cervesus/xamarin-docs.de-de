---
title: Indizieren Sie die Anwendung und Deep Links
description: "Indizieren Sie die Anwendung ermöglicht Anwendungen, die andernfalls vergessen würde, nachdem ein Paar wird verwendet, um die relevanten bleiben, indem Sie in den Suchergebnissen angezeigt wird. Deep Links ermöglicht Anwendungen, die auf ein Suchergebnis reagieren, die Anwendungsdaten, in der Regel durch Navigieren zu einer Seite aus dem deep-Link enthält. In diesem Artikel wird veranschaulicht, wie Anwendung Indizierung verwenden und deep links, um Xamarin.Forms Anwendungsinhalt auf IOS- und Android-Geräte durchsuchbar zu machen."
ms.topic: article
ms.prod: xamarin
ms.assetid: 410C5D19-AA3C-4E0D-B799-E288C5803226
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/11/2016
ms.openlocfilehash: b2decf1331764ed6b1696126d8b23318e329e0c7
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="application-indexing-and-deep-linking"></a>Indizieren Sie die Anwendung und Deep Links

_Indizieren Sie die Anwendung ermöglicht Anwendungen, die andernfalls vergessen würde, nachdem ein Paar wird verwendet, um die relevanten bleiben, indem Sie in den Suchergebnissen angezeigt wird. Deep Links ermöglicht Anwendungen, die auf ein Suchergebnis reagieren, die Anwendungsdaten, in der Regel durch Navigieren zu einer Seite aus dem deep-Link enthält. In diesem Artikel wird veranschaulicht, wie Anwendung Indizierung verwenden und deep links, um Xamarin.Forms Anwendungsinhalt auf IOS- und Android-Geräte durchsuchbar zu machen._

## <a name="overview"></a>Übersicht

Xamarin.Forms Anwendung Indizierung und deep Links bieten eine API für die Veröffentlichung von Metadaten für die Anwendung Indizierung, wie Benutzer über Anwendungen navigieren. Indizierte Inhalt kann dann nach in Spotlight-Suche, in der Google-Suche oder in eine Websuche gesucht. Tippen Sie auf ein Suchergebnis, die einen deep-Link enthält, wird ein Ereignis auszulösen, die von einer Anwendung verarbeitet werden können, und dient normalerweise zum Navigieren auf der Seite auf die verwiesen wird der deep-Link.

Die beispielanwendung veranschaulicht eine Todo List-Anwendung, in dem die Daten in einer lokalen SQLite-Datenbank gespeichert werden, wie in den folgenden Screenshots dargestellt:

![](deep-linking-images/screenshots.png ""Todolist"-Anwendung")

Jede `TodoItem` Instanz, die vom Benutzer erstellten indiziert wird. Clientplattform-spezifische Suche kann dann zum Suchen der indizierter Daten aus der Anwendung verwendet werden. Beim Tippen auf ein Ergebniselement Suche für die Anwendung die Anwendung gestartet wird, die `TodoItemPage` navigiert wird, und die `TodoItem` auf die verwiesen wird der umfassende Link wird angezeigt.

Weitere Informationen zur Verwendung der SQLite-Datenbank finden Sie unter [arbeiten mit einer lokalen Datenbank](~/xamarin-forms/app-fundamentals/databases.md).

> [!NOTE]
> **Hinweis**: Xamarin.Forms-Anwendung, Indizierung und deep verknüpfen Funktionalität ist nur für IOS- und Android-Plattformen verfügbar und erfordert iOS 9 und API 23 bzw.

## <a name="setup"></a>Setup

Die folgenden Abschnitte enthalten zusätzliche Einrichtung Anweisungen für die Verwendung dieser Funktion auf IOS- und Android-Plattformen.

### <a name="ios"></a>iOS

Auf die iOS-Plattform steht es ohne zusätzliche Einrichtung erforderlich, um diese Funktionalität verwenden.

### <a name="android"></a>Android

Es gibt eine Anzahl von erforderlichen Komponenten, die erfüllt werden müssen, um die Anwendung Indizierung und umfassende verknüpfen Funktionen verwenden, auf Android-Plattform:

1. Eine Version der Anwendung muss auf Google Play live sein.
1. Eine Begleit-Website muss für die Anwendung in Googles-Entwicklerkonsole registriert werden. Sobald die Anwendung eine Website zugeordnet ist, werden URLs können indiziert diese Aufgabe für die Website und die Anwendung, die dann in den Suchergebnissen angezeigter bedient werden kann. Weitere Informationen finden Sie unter [App auf Google-Suche indizieren](https://support.google.com/googleplay/android-developer/answer/6041489) Google-Website.
1. Ihre Anwendung muss HTTP-URL Intents unterstützen, auf die `MainActivity` -Klasse, die Anwendung, welche Arten von Schemata für URL-Daten indizieren Teilen Sie die Anwendung auf reagieren kann. Weitere Informationen finden Sie unter [Konfigurieren des Filters Absicht](~/android/platform/app-linking.md#configure-intent-filter).

Nachdem diese Voraussetzungen erfüllt sind, ist die folgende zusätzliche Konfigurationsschritte erforderlich, um Xamarin.Forms Anwendung Indizierung und deep links auf der Android-Plattform zu verwenden:

1. Installieren der [Xamarin.Forms.AppLinks](https://www.nuget.org/packages/Xamarin.Forms.AppLinks/) NuGet-Paket in der Android-Anwendungsprojekt.
1. In der `MainActivity.cs` Datei verwenden möchten, importieren die `Xamarin.Forms.Platform.Android.AppLinks` Namespace.
1. In der `MainActivity.OnCreate` außer Kraft setzen, fügen Sie die folgende Codezeile unterhalb `Forms.Init(this, bundle)`:

```csharp
AndroidAppLinks.Init (this);
```

Weitere Informationen finden Sie unter [Tiefe Link Content mit Xamarin.Forms URL Navigation](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) im Xamarin-Blog.

## <a name="indexing-a-page"></a>Indizieren einer Seite

Der Prozess für die Indizierung von einer Seite und Offenlegung für Google und Spotlight-Suche lautet wie folgt:

1. Erstellen einer [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) , enthält die Metadaten erforderlich, um die Indexseite zusammen mit einem deep-Link zur Seite zurückzukehren, wenn der Benutzer, den indizierten Inhalt in den Suchergebnissen angezeigter auswählt.
1. Registrieren der [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) Instanz für die Suche indizieren.

Im folgenden Codebeispiel wird veranschaulicht, wie zum Erstellen einer [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) Instanz:

```csharp
AppLinkEntry GetAppLink (TodoItem item)
{
  var pageType = GetType ().ToString ();
  var pageLink = new AppLinkEntry {
    Title = item.Name,
    Description = item.Notes,
    AppLinkUri = new Uri (string.Format ("http://{0}/{1}?id={2}",
      App.AppName, pageType, WebUtility.UrlEncode (item.ID)), UriKind.RelativeOrAbsolute),
    IsLinkActive = true,
    Thumbnail = ImageSource.FromFile ("monkey.png")
  };

  return pageLink;
}
```

Die [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) Instanz enthält eine Reihe von Eigenschaften, deren Werte erforderlich sind, um die Indexseite und erstellen einen deep-Link. Die [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.Title/), [ `Description` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.Description/), und [ `Thumbnail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.Thumbnail/) Eigenschaften werden verwendet, um den indizierten Inhalt zu identifizieren, wenn es in den Suchergebnissen angezeigt wird. Die [ `IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) -Eigenschaftensatz auf `true` um anzugeben, dass der indizierte Inhalt derzeit angezeigt wird. Die [ `AppLinkUri` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.AppLinkUri/) Eigenschaft ist ein `Uri` , enthält die erforderlichen Informationen an der aktuellen Seite zurückzugeben und Anzeigen der aktuellen `TodoItem`. Das folgende Beispiel zeigt ein Beispiel für `Uri` für die beispielanwendung:

```csharp
http://deeplinking/DeepLinking.TodoItemPage?id=ec38ebd1-811e-4809-8a55-0d028fce7819
```

Dies `Uri` enthält alle Informationen, die zum Starten erforderlich die `deeplinking` app, navigieren Sie zu der `DeepLinking.TodoItemPage`, und zeigen die `TodoItem` mit dem ein `ID` von `ec38ebd1-811e-4809-8a55-0d028fce7819`.

## <a name="registering-content-for-indexing"></a>Beim Registrieren für die Indizierung

Sobald ein [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) Instanz erstellt wurde, er muss registriert werden, für die Indizierung in Suchergebnisse angezeigt werden sollen. Dies wird erreicht, mit der [ `RegisterLink` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IAppLinks.RegisterLink/p/Xamarin.Forms.IAppLinkEntry/) Methode, wie im folgenden Codebeispiel gezeigt:

```csharp
Application.Current.AppLinks.RegisterLink (appLink);
```

Dadurch wird die [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) Instanz für der Anwendungsverzeichnis [ `AppLinks` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.AppLinks/) Auflistung.

> [!NOTE]
> **Hinweis**: die `RegisterLink` Methode kann auch verwendet werden, um den Inhalt zu aktualisieren, die für eine Seite indiziert wurde, ist.

Sobald ein [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) Instanz registriert wurde für die Indizierung, können sie in den Suchergebnissen angezeigt. Der folgende Screenshot zeigt indizierte Inhalt, die angezeigt wird, in den Suchergebnissen auf das iOS-Plattform:

![](deep-linking-images/ios-search.png "Indizierte Inhalt in den Suchergebnissen auf iOS")

## <a name="de-registering-indexed-content"></a>Registrierung aufheben indiziert Inhalt

Die [ `DeregisterLink` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IAppLinks.DeregisterLink/p/Xamarin.Forms.IAppLinkEntry/) Methode wird verwendet, um indizierte Inhalt in Suchergebnissen entfernen wie im folgenden Codebeispiel gezeigt:

```csharp
Application.Current.AppLinks.DeregisterLink (appLink);
```

Dies entfernt das [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) Instanz aus der Anwendungsverzeichnis [ `AppLinks` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.AppLinks/) Auflistung.

> [!NOTE]
> **Hinweis**: auf Android es ist nicht möglich, die indizierte Inhalt aus Suchergebnisse zu löschen.

<a name="responding" />

## <a name="responding-to-a-deep-link"></a>Reagieren auf eine Deep-Link

Wenn indizierte Inhalt in den Suchergebnissen angezeigt wird und von einem Benutzer ausgewählt ist die `App` -Klasse für die Anwendung eine Anforderung zum Behandeln erhält die `Uri` in der indizierte Inhalt enthalten. Diese Anforderung verarbeitet werden kann, der [ `OnAppLinkRequestReceived` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnAppLinkRequestReceived/p/System.Uri/) zu überschreiben, wie im folgenden Codebeispiel gezeigt:

```csharp
public class App : Application
{
  ...

  protected override async void OnAppLinkRequestReceived (Uri uri)
  {
    string appDomain = "http://" + App.AppName.ToLowerInvariant () + "/";
    if (!uri.ToString ().ToLowerInvariant ().StartsWith (appDomain)) {
      return;
    }

    string pageUrl = uri.ToString ().Replace (appDomain, string.Empty).Trim ();
    var parts = pageUrl.Split ('?');
    string page = parts [0];
    string pageParameter = parts [1].Replace ("id=", string.Empty);

    var formsPage = Activator.CreateInstance (Type.GetType (page));
    var todoItemPage = formsPage as TodoItemPage;
    if (todoItemPage != null) {
      var todoItem = App.Database.Find (pageParameter);
      todoItemPage.BindingContext = todoItem;
      await MainPage.Navigation.PushAsync (formsPage as Page);
    }

    base.OnAppLinkRequestReceived (uri);
  }
}
```

Die [ `OnAppLinkRequestReceived` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnAppLinkRequestReceived/p/System.Uri/) Methode überprüft, ob die empfangenen Daten `Uri` richtet sich die Anwendung, ehe die Analyse der `Uri` in die Seite, zu der navigiert werden und die Parameter, der auf der Seite übergeben werden. Eine Instanz der Seite für die leichte Navigation wird erstellt, und die `TodoItem` dargestellt von der Seite Parameter abgerufen wird. Die [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) der Seite, um zu der navigiert wird festgelegt die `TodoItem`. Dadurch wird sichergestellt, dass bei der `TodoItemPage` wird angezeigt, die [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushAsync/p/Xamarin.Forms.Page/) -Methode, es wird angezeigt werden die `TodoItem` , deren `ID` ist Bestandteil der deep-Link.

## <a name="making-content-available-for-search-indexing"></a>Verfügbarmachen von Inhalt für die Indizierung

Jedes Mal die Seite, die durch eine deep-Link dargestellt angezeigt wird, die [ `AppLinkEntry.IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) Eigenschaft kann festgelegt werden, um `true`. Auf IOS- und Android dadurch die [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) Instanz verfügbar, für die Indizierung, und klicken Sie auf nur iOS, stellt er auch die `AppLinkEntry` Instanz für die Übergabe verfügbar. Weitere Informationen zur Übergabe finden Sie unter [Einführung in die Übergabe](~/ios/platform/handoff.md).

Das folgende Codebeispiel veranschaulicht das Festlegen der [ `AppLinkEntry.IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) Eigenschaft `true` in der [ `Page.OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) außer Kraft setzen:

```csharp
protected override void OnAppearing ()
{
  appLink = GetAppLink (BindingContext as TodoItem);
  if (appLink != null) {
    appLink.IsLinkActive = true;
  }
}
```

Auf ähnliche Weise, wenn die Seite, die durch eine deep-Link dargestellt wird Weg von navigiert wird die [ `AppLinkEntry.IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) Eigenschaft kann festgelegt werden, um `false`. Auf IOS- und Android, verhindert die [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) angekündigt werden, für die Indizierung, und klicken Sie auf iOS nur It auch Instanz beendet Werbung die `AppLinkEntry` Instanz für die Übergabe. Dies geschieht der [ `Page.OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing()/) zu überschreiben, wie im folgenden Codebeispiel gezeigt:

```csharp
protected override void OnDisappearing ()
{
  if (appLink != null) {
    appLink.IsLinkActive = false;
  }
}
```

## <a name="providing-data-to-handoff"></a>Bereitstellen von Daten für die Übergabe

Bei iOS kann können anwendungsspezifische Daten gespeichert werden beim Indizieren der Seite. Dies erfolgt durch Hinzufügen von Daten zu den [ `KeyValues` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.KeyValues/) Auflistung, die eine `Dictionary<string, string>` für das Speichern in Übergabe von Schlüssel-Wert-Paaren, die verwendet werden. Übergabe ist eine Möglichkeit für den Benutzer zum Starten einer Aktivität auf einem ihrer Geräte und dieser Aktivität auf einem anderen Gerät (, das durch den Benutzer iCloud-Konto). Der folgende Code zeigt ein Beispiel für anwendungsspezifische Schlüssel-Wert-Paare speichern:

```csharp
var pageLink = new AppLinkEntry {
  ...  
};
pageLink.KeyValues.Add("appName", App.AppName);
pageLink.KeyValues.Add("companyName", "Xamarin");
```

In gespeicherten Werte der [ `KeyValues` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.KeyValues/) Auflistung wird in den Metadaten für die indizierte Seite gespeichert werden, und wird wiederhergestellt werden, sobald der Benutzer auf ein Suchergebnis tippt deep-Link enthält (oder wenn Übergabe verwendet wird, um den Inhalt auf einem anderen anzuzeigen angemeldeten Geräte).

Darüber hinaus können Werte für die folgenden Schlüssel angegeben werden:

- `contentType` – ein `string` zur Angabe des Bezeichners alle denselben Typ, der indiziert Inhalt. Die Aufrufkonvention an, für diesen Wert verwenden, die empfohlene ist der Typname der Seite, den indizierten Inhalt enthält.
- `associatedWebPage` – ein `string` , das die Webseite besuchen. wenn der indizierte Inhalt auch im Web angezeigt werden kann oder die Anwendung des Safari deep-Links unterstützt darstellt.
- `shouldAddToPublicIndex` – ein `string` entweder `true` oder `false` , die steuert, ob, klicken Sie dann den Benutzern angezeigt werden kann, wer die Anwendung auf seinem iOS-Gerät installiert haben Apple öffentlichen Cloud Index, den indizierten Inhalt hinzuzufügen. Allerdings nur weil der Inhalt für die öffentliche Indizierung festgelegt wurde, bedeutet es nicht, dass es automatisch von Apple öffentlichen Cloud Index hinzugefügt werden. Weitere Informationen finden Sie unter [öffentliche Indizierung](~/ios/platform/search/nsuseractivity.md). Beachten Sie, die diesen Schlüssel soll, um festgelegt werden `false` beim Hinzufügen von persönlichen Daten, die [ `KeyValues` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.KeyValues/) Auflistung.

> [!NOTE]
> **Hinweis**: die `KeyValues` Auflistung wird nicht auf die Android-Plattform verwendet.

Weitere Informationen zur Übergabe finden Sie unter [Einführung in die Übergabe](~/ios/platform/handoff.md).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird veranschaulicht, wie Anwendung Indizierung und deep links, um Xamarin.Forms Anwendungsinhalt auf IOS- und Android-Geräte durchsuchbar zu machen. Indizieren Sie die Anwendung ermöglicht Anwendungen, bleiben relevanten angezeigt, die in den Suchergebnissen, die über andernfalls vergessen werden würde, nachdem ein Paar verwendet werden.


## <a name="related-links"></a>Verwandte Links

- [Umfassende verknüpfen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/deeplinking/)
- [iOS-Such-APIs](~/ios/platform/search/index.md)
- [App-Verknüpfung in Android 6.0](~/android/platform/app-linking.md)
- [AppLinkEntry](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/)
- [IAppLinkEntry](https://developer.xamarin.com/api/type/Xamarin.Forms.IAppLinkEntry/)
- [IAppLinks](https://developer.xamarin.com/api/type/Xamarin.Forms.IAppLinks/)
