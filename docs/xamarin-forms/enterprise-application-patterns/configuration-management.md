---
title: Konfigurationsmanagement (Configuration Management)
ms.prod: xamarin
ms.assetid: 50d6e780-e768-47f8-9361-3af11e56b87b
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 9318f86077c9cdb98e073e4816dae4fdabbfe568
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="configuration-management"></a>Konfigurationsmanagement (Configuration Management)

Einstellungen können Sie die Trennung von Daten, die Verhalten einer App aus dem Code konfiguriert, das Verhalten geändert werden, ohne die app neu zu erstellen. Es gibt zwei Arten von Einstellungen: app-Einstellungen und die benutzereinstellungen.

App-Einstellungen sind Daten, die eine app erstellt und verwaltet. Sie können Daten wie z. B. festen Webdienst-Endpunkte, die API-Schlüssel und Laufzeitstatus enthalten. App-Einstellungen sind auf das Vorhandensein der app gebunden und sind nur für diese Anwendung von Bedeutung.

Benutzereinstellungen werden die anpassbare Einstellungen einer App, die beeinflussen das Verhalten der app und häufig erneute Anpassung erfordern. Beispielsweise können eine app anzugeben, wo Sie das Abrufen von Daten aus, und wie auf dem Bildschirm angezeigt.

Xamarin.Forms umfasst einen persistenten Wörterbuch, das zum Speichern von Einstellungsdaten verwendet werden kann. Dieses Wörterbuch kann zugegriffen werden, mithilfe der [ `Application.Current.Properties` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Properties/) -Eigenschaft, und alle Daten, die in der er eingefügt werden wird gespeichert, wenn die app wechselt in den Ruhezustand und wird wiederhergestellt, wenn die app fortgesetzt wird bzw. erneut wird gestartet. Darüber hinaus die [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) -Klasse verfügt auch über eine [ `SavePropertiesAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.SavePropertiesAsync()/) -Methode, können eine app, die Einstellungen, die bei der ersten Verwendung gespeichert haben. Weitere Informationen zu diesem Wörterbuch, finden Sie unter [Eigenschaftenwörterbuch](~/xamarin-forms/app-fundamentals/application-class.md#Properties_Dictionary).

Speichern von Daten mit Xamarin.Forms persistenten Wörterbuch jedoch ein Nachteil ist, dass es nicht leicht an Daten gebunden ist. Aus diesem Grund verwendet die mobilen Anwendung für eShopOnContainers die Xam.Plugins.Settings-Bibliothek von [NuGet](https://www.nuget.org/packages/Xam.Plugins.Settings/). Diese Bibliothek bietet einen konsistenten, typsicher und plattformübergreifende Ansatz zum beibehalten und Abrufen von App- und Benutzerdaten-Einstellungen bei der Verwendung der systemeigenen einstellungsverwaltung von jeder Plattform bereitgestellt werden. Darüber hinaus ist es einfach, mit der Datenbindung Datenzugriff Einstellungen von der Bibliothek verfügbar gemacht werden.

> [!NOTE]
> Obwohl die Bibliothek Xam.Plugin.Settings App- und Benutzerdaten Einstellungen gespeichert werden kann, wird keine Unterscheidung zwischen den beiden.

## <a name="creating-a-settings-class"></a>Erstellen eine Settings-Klasse

Wenn Sie die Xam.Plugins.Settings-Bibliothek verwenden, eine einzelne statische Klasse sollten erstellt werden, enthält die App- und Benutzerdaten-Einstellungen, die von der app erforderlich sind. Das folgende Codebeispiel zeigt Settings-Klasse in der mobilen eShopOnContainers-app:

```csharp
public static class Settings  
{  
    private static ISettings AppSettings  
    {  
        get  
        {  
            return CrossSettings.Current;  
        }  
    }  
    ...  
}
```

Einstellungen können gelesen und geschrieben werden, über die `ISettings` -API, die von der Xam.Plugins.Settings-Bibliothek bereitgestellt wird. Diese Bibliothek enthält, die verwendet werden kann, auf die API Singleton `CrossSettings.Current`, und eine app Settings-Klasse sollten Verfügbarmachen dieses Singleton über eine `ISettings` Eigenschaft.

> [!NOTE]
> Using-Direktiven für die Namespaces Plugin.Settings und Plugin.Settings.Abstractions sollten auf eine Klasse hinzugefügt werden, die Zugriff auf die Xam.Plugins.Settings Bibliothekstypen erforderlich sind.

## <a name="adding-a-setting"></a>Hinzufügen einer Einstellung

Jede Einstellung besteht aus einem Schlüssel, einen Standardwert und eine Eigenschaft. Das folgende Codebeispiel zeigt alle drei Elemente für eine benutzereinstellung, die die base-URL für die online Services, die darstellt mit die mobilen Anwendung für eShopOnContainers verbunden:

```csharp
public static class Settings  
{  
    ...  
    private const string IdUrlBase = "url_base";  
    private static readonly string UrlBaseDefault = GlobalSetting.Instance.BaseEndpoint;  
    ...  

    public static string UrlBase  
    {  
        get  
        {  
            return AppSettings.GetValueOrDefault<string>(IdUrlBase, UrlBaseDefault);  
        }  
        set  
        {  
            AppSettings.AddOrUpdateValue<string>(IdUrlBase, value);  
        }  
    }  
}
```

Der Schlüssel ist immer eine Konstante Zeichenfolge, die definiert, den Namen des Schlüssels, mit dem Standardwert für die Einstellung wird ein statischer Readonly-Wert des erforderlichen Typs. Einen Standardwert wird sichergestellt, dass ein gültiger Wert ist verfügbar, wenn eine Einstellung Festlegung abgerufen wird.

Die `UrlBase` statische Eigenschaft mithilfe von zwei Methoden der `ISettings` API zum Lesen oder schreiben den Einstellungswert. Die `ISettings.GetValueOrDefault` Methode wird verwendet, um eines Einstellungswerts aus plattformspezifischen-Speicher abzurufen. Wenn kein Wert für die Einstellung definiert ist, wird stattdessen seinen Standardwert abgerufen. Auf ähnliche Weise die `ISettings.AddOrUpdateValue` Methode wird verwendet, um einen Einstellungswert plattformspezifischen Speicher beizubehalten.

Sondern definieren, die einen Standardwert innerhalb der `Settings` -Klasse, die `UrlBaseDefault` Zeichenfolge erhält seinen Wert aus der `GlobalSetting` Klasse. Das folgende Codebeispiel zeigt die `BaseEndpoint` Eigenschaft und `UpdateEndpoint` Methode in dieser Klasse:

```csharp
public class GlobalSetting  
{  
    ...  
    public string BaseEndpoint  
    {  
        get { return _baseEndpoint; }  
        set  
        {  
            _baseEndpoint = value;  
            UpdateEndpoint(_baseEndpoint);  
        }  
    }  
    ...  

    private void UpdateEndpoint(string baseEndpoint)  
    {  
        RegisterWebsite = string.Format("{0}:5105/Account/Register", baseEndpoint);  
        CatalogEndpoint = string.Format("{0}:5101", baseEndpoint);  
        OrdersEndpoint = string.Format("{0}:5102", baseEndpoint);  
        BasketEndpoint = string.Format("{0}:5103", baseEndpoint);  
        IdentityEndpoint = string.Format("{0}:5105/connect/authorize", baseEndpoint);  
        UserInfoEndpoint = string.Format("{0}:5105/connect/userinfo", baseEndpoint);  
        TokenEndpoint = string.Format("{0}:5105/connect/token", baseEndpoint);  
        LogoutEndpoint = string.Format("{0}:5105/connect/endsession", baseEndpoint);  
        IdentityCallback = string.Format("{0}:5105/xamarincallback", baseEndpoint);  
        LogoutCallback = string.Format("{0}:5105/Account/Redirecting", baseEndpoint);  
    }  
}
```

Jedes Mal die `BaseEndpoint` Eigenschaft festgelegt ist, die `UpdateEndpoint` -Methode aufgerufen wird. Diese Methode wird aktualisiert, eine Reihe von Eigenschaften auf der Grundlage der `UrlBase` benutzereinstellung, die von bereitgestellten der `Settings` Klasse, die verschiedene Endpunkte darstellen, die mit die mobilen Anwendung für eShopOnContainers verbindet.

## <a name="data-binding-to-user-settings"></a>Binden von Daten an Benutzereinstellungen

In der mobilen app eShopOnContainers die `SettingsView` macht zwei benutzereinstellungen. Diese Einstellungen ermöglichen die Konfiguration der gibt an, ob die app Daten von Microservices abgerufen werden sollen, die als Docker-Container bereitgestellt werden, oder gibt an, ob die app Daten von simulierten Diensten abgerufen werden sollen, die eine Internetverbindung erfordern. Wenn Sie zum Abrufen von Daten von Datenvolumes Microservices auswählen, muss eine Basis-Endpunkt-URL für die Microservices angegeben werden. Abbildung 7 – 1 zeigt die `SettingsView` , wenn der Benutzer hat ausgewählt zum Abrufen von Daten von Datenvolumes Microservices.

![](configuration-management-images/settings-endpoint.png "Benutzereinstellungen, die von der mobilen Anwendung für eShopOnContainers verfügbar gemacht werden")

**Abbildung 7 – 1**: benutzereinstellungen, die von der mobilen Anwendung für eShopOnContainers verfügbar gemacht werden

Binden von Daten dienen zum Abrufen und Festlegen von Einstellungen, die verfügbar gemacht werden, indem die `Settings` Klasse. Dies erfolgt durch Steuerelemente für die Ansicht-Bindung auf Ansicht Modelleigenschaften, die wiederum Eigenschaften im Zugriff auf die `Settings` Klasse und durch das Auslösen einer Eigenschaft geändert Benachrichtigung auf, wenn der Einstellungswert geändert hat. Informationen wie die mobilen Anwendung für eShopOnContainers Ansicht erstellt modelliert und ordnet sie Sichten, finden Sie unter [erstellen automatisch mit einer Ansicht Modell Locator einem Ansichtsmodell](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

Das folgende Codebeispiel zeigt die [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) -Steuerelement aus der `SettingsView` , das den Benutzer zur Eingabe einer Basis-Endpunkt-URL für die Datenvolumes Microservices ermöglicht:

```xaml
<Entry Text="{Binding Endpoint, Mode=TwoWay}" />
```

Dies [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Steuerelement bindet an die `Endpoint` Eigenschaft von der `SettingsViewModel` Klasse, die über eine bidirektionale Bindung. Das folgende Codebeispiel zeigt die Endpunkt-Eigenschaft:

```csharp
public string Endpoint  
{  
    get { return _endpoint; }  
    set  
    {  
        _endpoint = value;  

        if(!string.IsNullOrEmpty(_endpoint))  
        {  
            UpdateEndpoint(_endpoint);  
        }  

        RaisePropertyChanged(() => Endpoint);  
    }  
}
```

Wenn die `Endpoint` festgelegt wird die `UpdateEndpoint` -Methode aufgerufen wird, vorausgesetzt, dass der angegebene Wert gültig ist, und die Eigenschaft geändert wird die Benachrichtigung ausgelöst. Das folgende Codebeispiel zeigt die `UpdateEndpoint` Methode:

```csharp
private void UpdateEndpoint(string endpoint)  
{  
    Settings.UrlBase = endpoint;  
}
```

Diese Methode aktualisiert die `UrlBase` Eigenschaft in der `Settings` Klasse mit dem Basis-Endpunkt-URL-Wert vom Benutzer eingegeben werden, wodurch plattformspezifischen Speicher beibehalten werden.

Wenn die `SettingsView` navigiert wird, die `InitializeAsync` Methode in der `SettingsViewModel` Klasse ausgeführt wird. Im folgenden Codebeispiel wird diese Methode veranschaulicht:

```csharp
public override Task InitializeAsync(object navigationData)  
{  
    ...  
    Endpoint = Settings.UrlBase;  
    ...  
}
```

Die-Methode legt die `Endpoint` auf den Wert der Eigenschaft der `UrlBase` Eigenschaft in der `Settings` Klasse. Zugreifen auf die `UrlBase` -Eigenschaft bewirkt, dass die Bibliothek Xam.Plugins.Settings der Einstellungswert aus der plattformspezifischen Speicher abrufen. Informationen darüber, wie die `InitializeAsync` Methode aufgerufen wird, finden Sie unter [übergeben von Parametern während der Navigation](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation).

Dieser Mechanismus wird sichergestellt, dass wenn ein Benutzer auf die SettingsView navigiert, benutzereinstellungen aus plattformspezifischen Speicher abgerufen und durch die Datenbindung angezeigt. Klicken Sie dann, wenn der Benutzer die Einstellungswerte ändert, wird sichergestellt, dass Datenbindung, dass sie sofort an plattformspezifischen-Speicher gespeichert werden.

## <a name="summary"></a>Zusammenfassung

Einstellungen können Sie die Trennung von Daten, die Verhalten einer App aus dem Code konfiguriert, das Verhalten geändert werden, ohne die app neu zu erstellen. App-Einstellungen sind Daten, die eine app erstellt und verwaltet und benutzereinstellungen werden die anpassbare Einstellungen einer App, die beeinflussen das Verhalten der app und häufig erneute Anpassung erfordern.

Die Xam.Plugins.Settings-Bibliothek stellt eine konsistente, typsichere, plattformübergreifende Ansatz zum beibehalten und zum Abrufen der app und benutzereinstellungen und Datenbindung kann Einstellungen, die mit der Bibliothek zugreifen verwendet werden.


## <a name="related-links"></a>Verwandte Links

- [Download-e-Book (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
