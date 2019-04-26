---
title: Konfigurationsmanagement (Configuration Management)
description: In diesem Kapitel wird erläutert, wie die mobile app von eShopOnContainers konfigurationsverwaltung zum Bereitstellen von Anwendungseinstellungen und benutzereinstellungen implementiert.
ms.prod: xamarin
ms.assetid: 50d6e780-e768-47f8-9361-3af11e56b87b
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 6f32d8f328232bdfc644da57bdb3201c60010063
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61381899"
---
# <a name="configuration-management"></a>Konfigurationsmanagement (Configuration Management)

Einstellungen können Sie die Trennung von Daten, die das Verhalten einer App aus dem Code, konfiguriert das Verhalten geändert werden, ohne die app neu zu erstellen. Es gibt zwei Arten von Einstellungen: app-Einstellungen und benutzereinstellungen.

App-Einstellungen sind Daten, die eine app erstellt und verwaltet. Sie können Daten wie feste Webdienst-Endpunkte, API-Schlüssel und des Laufzeitstatus, enthalten. App-Einstellungen sind auf das Vorhandensein der app gebunden, und es sind nur sinnvoll, diese app.

Benutzereinstellungen werden die anpassbaren Einstellungen einer App, die Auswirkungen auf das Verhalten der app und häufig erneute Anpassung ist nicht erforderlich. Beispielsweise kann eine app den Benutzer angeben, wo Sie die Daten abgerufen werden sollen, und wie auf dem Bildschirm angezeigt ermöglichen.

Xamarin.Forms umfasst einen persistenten Wörterbuch, das zum Speichern von für Einstellungsdaten verwendet werden kann. Dieses Wörterbuch kann zugegriffen werden, mithilfe der [ `Application.Current.Properties` ](xref:Xamarin.Forms.Application.Properties) -Eigenschaft, und alle Daten, die in den es versetzt werden wird gespeichert, wenn die app in den Ruhezustand wechselt und wiederhergestellt, sobald die app fortgesetzt wird, oder dem erneuten Beginn. Darüber hinaus die [ `Application` ](xref:Xamarin.Forms.Application) -Klasse verfügt auch über eine [ `SavePropertiesAsync` ](xref:Xamarin.Forms.Application.SavePropertiesAsync) -Methode, können eine app, damit die Einstellungen gespeichert, wenn erforderlich. Weitere Informationen zu diesem Wörterbuch, finden Sie unter [Wörterbuch "Properties"](~/xamarin-forms/app-fundamentals/application-class.md#Properties_Dictionary).

Speichern von Daten, die mit dem permanenten Xamarin.Forms-Wörterbuch ein Nachteil ist, dass es nicht einfach an Daten gebunden ist. Aus diesem Grund verwendet die "eshoponcontainers" mobile app die Xam.Plugins.Settings Bibliothek, [NuGet](https://www.nuget.org/packages/Xam.Plugins.Settings/). Diese Bibliothek bietet einen konsistenten, typsicher und plattformübergreifenden Ansatz für beibehalten und Abrufen von app und benutzereinstellungen, bei der Verwendung der einheitlichen Verwaltung von jeder Plattform bereitgestellt werden. Darüber hinaus ist es einfach, die Datenbindung zu verwenden, den Zugriff auf für Einstellungsdaten, die von der Bibliothek verfügbar gemacht werden.

> [!NOTE]
> Während die Bibliothek Xam.Plugin.Settings sowohl app als auch benutzereinstellungen speichern kann, ist es keinen Unterschied zwischen den beiden.

## <a name="creating-a-settings-class"></a>Erstellen eine Settings-Klasse

Wenn Sie die Xam.Plugins.Settings-Bibliothek verwenden zu können, eine einzelne statische Klasse sollte erstellt werden, enthält die app und benutzereinstellungen, die von der app erforderlich sind. Das folgende Codebeispiel zeigt die Settings-Klasse in der mobilen app für "eshoponcontainers":

```csharp
public static class Settings  
{  
    private static ISettings AppSettings  
    {  
        get  
        {  
            return CrossSettings.Current;  
        }  
    }  
    ...  
}
```

Einstellungen gelesen und geschrieben werden können, die `ISettings` -API, die von der Xam.Plugins.Settings-Bibliothek bereitgestellt wird. Diese Bibliothek enthält, die verwendet werden kann, den Zugriff auf die API Singleton `CrossSettings.Current`, und der app-Settings-Klasse sollte diesem Singleton über verfügbar zu machen eine `ISettings` Eigenschaft.

> [!NOTE]
> Using-Direktiven für die Namespaces Plugin.Settings und Plugin.Settings.Abstractions sollten auf eine Klasse hinzugefügt werden, die Zugriff auf die Bibliothekstypen Xam.Plugins.Settings erforderlich sind.

## <a name="adding-a-setting"></a>Hinzufügen einer Einstellung

Jede Einstellung besteht aus einem Schlüssel, einen Standardwert und eine Eigenschaft. Das folgende Codebeispiel zeigt alle drei Elemente für eine benutzereinstellung, die die base-URL für den online-Dienste, die die "eshoponcontainers" mobile app eine darstellt Verbindung mit herstellt:

```csharp
public static class Settings  
{  
    ...  
    private const string IdUrlBase = "url_base";  
    private static readonly string UrlBaseDefault = GlobalSetting.Instance.BaseEndpoint;  
    ...  

    public static string UrlBase  
    {  
        get  
        {  
            return AppSettings.GetValueOrDefault<string>(IdUrlBase, UrlBaseDefault);  
        }  
        set  
        {  
            AppSettings.AddOrUpdateValue<string>(IdUrlBase, value);  
        }  
    }  
}
```

Der Schlüssel ist immer eine const-Zeichenfolge, die den Schlüssel definiert wird, wird Sie mit dem Standardwert für die Einstellung des erforderlichen Typs statischen schreibgeschützten Wert. Bereitstellung eines Standardwerts wird sichergestellt, dass ein gültiger Wert ist verfügbar, wenn es sich bei eine Festlegung-Einstellung abgerufen wird.

Die `UrlBase` statische Eigenschaft mithilfe von zwei Methoden der `ISettings` API zum Lesen oder schreiben den Wert der Einstellung. Die `ISettings.GetValueOrDefault` Methode dient zum Abrufen der Wert einer Einstellung von Plattform-spezifischen Speicher. Wenn kein Wert für die Einstellung definiert ist, wird stattdessen der Standardwert abgerufen. Auf ähnliche Weise die `ISettings.AddOrUpdateValue` Methode wird verwendet, um der Wert einer Einstellung in den plattformspezifischen Speicher beizubehalten.

Sondern definieren, die einen Standardwert innerhalb der `Settings` -Klasse, die `UrlBaseDefault` Zeichenfolge erhält ihren Wert aus der `GlobalSetting` Klasse. Das folgende Codebeispiel zeigt die `BaseEndpoint` Eigenschaft und `UpdateEndpoint` Methode in dieser Klasse:

```csharp
public class GlobalSetting  
{  
    ...  
    public string BaseEndpoint  
    {  
        get { return _baseEndpoint; }  
        set  
        {  
            _baseEndpoint = value;  
            UpdateEndpoint(_baseEndpoint);  
        }  
    }  
    ...  

    private void UpdateEndpoint(string baseEndpoint)  
    {  
        RegisterWebsite = string.Format("{0}:5105/Account/Register", baseEndpoint);  
        CatalogEndpoint = string.Format("{0}:5101", baseEndpoint);  
        OrdersEndpoint = string.Format("{0}:5102", baseEndpoint);  
        BasketEndpoint = string.Format("{0}:5103", baseEndpoint);  
        IdentityEndpoint = string.Format("{0}:5105/connect/authorize", baseEndpoint);  
        UserInfoEndpoint = string.Format("{0}:5105/connect/userinfo", baseEndpoint);  
        TokenEndpoint = string.Format("{0}:5105/connect/token", baseEndpoint);  
        LogoutEndpoint = string.Format("{0}:5105/connect/endsession", baseEndpoint);  
        IdentityCallback = string.Format("{0}:5105/xamarincallback", baseEndpoint);  
        LogoutCallback = string.Format("{0}:5105/Account/Redirecting", baseEndpoint);  
    }  
}
```

Jedes Mal die `BaseEndpoint` Eigenschaft festgelegt ist, die `UpdateEndpoint` Methode wird aufgerufen. Diese Methode wird aktualisiert, eine Reihe von Eigenschaften, die alle basieren auf der `UrlBase` benutzereinstellung, die von bereitgestellte der `Settings` -Klasse, die verschiedene Endpunkte darstellen, die mit die mobilen app für "eshoponcontainers" verbindet.

## <a name="data-binding-to-user-settings"></a>Datenbindung an den Benutzereinstellungen

In der mobilen Anwendung "eshoponcontainers" die `SettingsView` stellt zwei benutzereinstellungen zur Verfügung. Diese Einstellungen ermöglichen die Konfiguration gibt an, ob die app Daten von Microservices abrufen soll, die als Docker-Container bereitgestellt werden, oder gibt an, ob die app Daten von pseudodienste abrufen soll, die keine Internetverbindung erforderlich ist. Wenn Sie zum Abrufen von Daten aus Microservices in Containern auswählen, muss eine Basis-Endpunkt-URL für die Microservices angegeben werden. Abbildung 7 – 1 zeigt die `SettingsView` Wenn der Benutzer zum Abrufen von Daten aus Microservices in Containern ausgewählt wurde.

![](configuration-management-images/settings-endpoint.png "Benutzereinstellungen, die von der mobilen app von eShopOnContainers verfügbar gemacht werden")

**Abbildung 7-1**: Benutzereinstellungen, die von der mobilen app von eShopOnContainers verfügbar gemacht werden

Die Datenbindung kann verwendet werden, zum Abrufen und Festlegen von Einstellungen, die verfügbar gemacht werden, indem die `Settings` Klasse. Dies erfolgt durch die Steuerelemente auf die Ansicht-Bindung an Eigenschaften, die Eigenschaften im wiederum Zugriff auf die `Settings` -Klasse, und das Auslösen einer Eigenschaft geändert Benachrichtigung auf, wenn der Einstellungswert geändert hat. Informationen wie die "eshoponcontainers" mobile app Ansicht erstellt Modelle und ordnet diese zu Ansichten finden Sie [automatisch ein Ansichtsmodell mit einem View Model-Locator erstellen](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

Das folgende Codebeispiel zeigt die [ `Entry` ](xref:Xamarin.Forms.Entry) -Steuerelement aus der `SettingsView` , in dem der Benutzer zur Eingabe einer grundlegenden Endpunkt-URL für die Microservices in Containern:

```xaml
<Entry Text="{Binding Endpoint, Mode=TwoWay}" />
```

Dies [ `Entry` ](xref:Xamarin.Forms.Entry) -Steuerelement gebunden wird, die `Endpoint` Eigenschaft der `SettingsViewModel` Klasse, wobei eine bidirektionale Bindung. Das folgende Codebeispiel zeigt die Endpunkt-Eigenschaft:

```csharp
public string Endpoint  
{  
    get { return _endpoint; }  
    set  
    {  
        _endpoint = value;  

        if(!string.IsNullOrEmpty(_endpoint))  
        {  
            UpdateEndpoint(_endpoint);  
        }  

        RaisePropertyChanged(() => Endpoint);  
    }  
}
```

Wenn die `Endpoint` -Eigenschaftensatz der `UpdateEndpoint` -Methode aufgerufen wird, vorausgesetzt, dass der angegebene Wert gültig ist, und die Eigenschaft geändert wird die Benachrichtigung ausgelöst. Die `UpdateEndpoint`-Methode wird in folgendem Codebeispiel veranschaulicht:

```csharp
private void UpdateEndpoint(string endpoint)  
{  
    Settings.UrlBase = endpoint;  
}
```

Diese Methode aktualisiert die `UrlBase` -Eigenschaft in der `Settings` -Klasse mit den grundlegenden Endpunkt-URL-Wert vom Benutzer eingegeben werden, wodurch sie in plattformspezifische Speicher beibehalten werden.

Wenn die `SettingsView` navigiert wird, die `InitializeAsync` -Methode in der die `SettingsViewModel` Klasse ausgeführt wird. Im folgenden Codebeispiel wird diese Methode veranschaulicht:

```csharp
public override Task InitializeAsync(object navigationData)  
{  
    ...  
    Endpoint = Settings.UrlBase;  
    ...  
}
```

Der Methode wird die `Endpoint` Eigenschaft auf den Wert der `UrlBase` -Eigenschaft in der `Settings` Klasse. Zugreifen auf die `UrlBase` -Eigenschaft bewirkt, dass die Xam.Plugins.Settings-Bibliothek, um den Einstellungswert aus plattformspezifischen Storage abrufen. Informationen darüber, wie die `InitializeAsync` Methode aufgerufen wird, finden Sie unter [übergeben von Parametern während der Navigation](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation).

Dieser Mechanismus wird sichergestellt, dass wenn ein Benutzer zum Anzeigen navigiert, benutzereinstellungen aus plattformspezifischen Speicher abgerufen und mithilfe von Datenbindung angezeigt. Klicken Sie dann, wenn der Benutzer die Einstellungswerte ändert, wird sichergestellt Datenbindung, dass sie sofort wieder in den plattformspezifischen-Speicher gespeichert werden.

## <a name="summary"></a>Zusammenfassung

Einstellungen können Sie die Trennung von Daten, die das Verhalten einer App aus dem Code, konfiguriert das Verhalten geändert werden, ohne die app neu zu erstellen. App-Einstellungen sind Daten, die eine app erstellt und verwaltet und benutzereinstellungen sind die anpassbare Einstellungen einer App, die Auswirkungen auf das Verhalten der app und häufig erneute Anpassung ist nicht erforderlich.

Die Xam.Plugins.Settings-Bibliothek bietet eine konsistente, typsicher, plattformübergreifenden Ansatz für beibehalten und Abrufen von app und benutzereinstellungen und die Datenbindung kann verwendet werden, um die Einstellungen, die erstellt werden, mit der Bibliothek zugreifen.


## <a name="related-links"></a>Verwandte Links

- [E-Book (2Mb PDF-Datei) herunterladen](https://aka.ms/xamarinpatternsebook)
- ["eshoponcontainers" (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
