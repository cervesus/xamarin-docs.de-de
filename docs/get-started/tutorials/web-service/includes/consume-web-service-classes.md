---
ms.openlocfilehash: 90f3f9ff5ed29a1ae2c93e355fc15bc6550d78dd
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "77135155"
---
In dieser Übung erstellen Sie eine Benutzeroberfläche, um die `RestService`-Klasse zu verwenden, die wiederum Daten aus der Web-API [OpenWeatherMap](https://openweathermap.org/) abruft.

# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. Doppelklicken Sie im **Projektmappen-Explorer** im Projekt **WebServiceTutorial** auf die Datei **MainPage.xaml**, um sie zu öffnen. Entfernen Sie dann in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="WebServiceTutorial.MainPage">
        <Grid Margin="20,35,20,20">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="0.4*" />
                <ColumnDefinition Width="0.6*" />
            </Grid.ColumnDefinitions>
            <Grid.RowDefinitions>
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
            </Grid.RowDefinitions>
            <Entry x:Name="cityEntry"
                   Grid.ColumnSpan="2"
                   Text="Seattle" />
            <Button Grid.ColumnSpan="2"
                    Grid.Row="1"
                    Text="Get Weather"
                    Clicked="OnButtonClicked" />                
            <Label Grid.Row="2"
                   Text="Location:" />
            <Label Grid.Row="2"
                   Grid.Column="1"
                   Text="{Binding Title}" />            
            <Label Grid.Row="3"
                   Text="Temperature:" />
            <Label Grid.Row="3"
                   Grid.Column="1"
                   Text="{Binding Main.Temperature}" />            
            <Label Grid.Row="4"
                   Text="Wind Speed:" />
            <Label Grid.Row="4"
                   Grid.Column="1"
                   Text="{Binding Wind.Speed}" />            
            <Label Grid.Row="5"
                   Text="Humidity:" />
            <Label Grid.Row="5"
                   Grid.Column="1"
                   Text="{Binding Main.Humidity}" />            
            <Label Grid.Row="6"
                   Text="Visibility:" />
            <Label Grid.Row="6"
                   Grid.Column="1"
                   Text="{Binding Weather[0].Visibility}" />
        </Grid>
    </ContentPage>
    ```

    Dieser Code definiert deklarativ die Benutzeroberfläche für die Seite, die aus einer [`Entry`](xref:Xamarin.Forms.Entry)-Klasse, einem [`Button`](xref:Xamarin.Forms.Button)-Objekt und einer Reihe von [`Label`](xref:Xamarin.Forms.Label)-Instanzen in einer [`Grid`](xref:Xamarin.Forms.Grid)-Klasse besteht. Die `Entry`-Klasse ist bereits mit „Seattle“ gefüllt, indem dessen [`Text`](xref:Xamarin.Forms.InputView.Text)-Eigenschaft festgelegt wird. Das `Button`-Objekt legt sein [`Clicked`](xref:Xamarin.Forms.Button.Clicked)-Ereignis auf einen Ereignishandler namens `OnButtonClicked` fest, der im nächsten Schritt erstellt wird. Die Hälfte der `Label`-Instanzen zeigen statischen Text an. Die andere Hälfte der Instanzen stellt Datenbindung mit den `WeatherData`-Eigenschaften her. Während der Laufzeit suchen die `Label`-Instanzen, die Datenbindung verwenden, in ihren entsprechenden [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Eigenschaften nach dem `WeatherData`-Objekt, das in ihren Bindungsausdrücken verwendet werden soll. Weitere Informationen zur Datenbindung finden Sie unter [Xamarin.Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md).

    Außerdem hat die [`Entry`](xref:Xamarin.Forms.Entry)-Klasse einen Namen, der mit dem `x:Name`-Attribut festgelegt wird. Dadurch kann die CodeBehind-Datei mithilfe des zugewiesenen Namens auf das Objekt zugreifen.

1. Erweitern Sie **MainPage.xaml** im **Projektmappen-Explorer** im Projekt **WebServiceTutorial**, und doppelklicken Sie dann auf die Datei **MainPage.xaml.cs**, um sie zu öffnen. Entfernen Sie dann in **MainPage.xaml.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace WebServiceTutorial
    {
        public partial class MainPage : ContentPage
        {
            RestService _restService;

            public MainPage()
            {
                InitializeComponent();
                _restService = new RestService();
            }

            async void OnButtonClicked(object sender, EventArgs e)
            {
                if (!string.IsNullOrWhiteSpace(cityEntry.Text))
                {
                    WeatherData weatherData = await _restService.GetWeatherDataAsync(GenerateRequestUri(Constants.OpenWeatherMapEndpoint));
                    BindingContext = weatherData;
                }
            }

            string GenerateRequestUri(string endpoint)
            {
                string requestUri = endpoint;
                requestUri += $"?q={cityEntry.Text}";
                requestUri += "&units=imperial"; // or units=metric
                requestUri += $"&APPID={Constants.OpenWeatherMapAPIKey}";
                return requestUri;
            }
        }
    }
    ```

    Die `OnButtonClicked`-Methode, die ausgeführt wird, wenn auf das [`Button`](xref:Xamarin.Forms.Button)-Objekt getippt wird, ruft die `RestService.GetWeatherDataAsync`-Methode auf, damit diese Wetterdaten für die Stadt abruft, deren Name in der [`Entry`](xref:Xamarin.Forms.Entry)-Klasse eingegeben wurde. Die `GetWeatherDataAsync`-Methode erfordert ein `string`-Argument, das den URI der Web-API darstellt, die aufgerufen wird. Dies wird von der `GenerateRequestUri`-Methode erstellt. Diese Methode nimmt die Endpunktadresse, die in der `OpenWeatherMapEndpoint`-Konstante gespeichert ist, und fügt die Abfrageparameter hinzu, die Folgendes angeben:

    - Die Stadt, für die Wetterdaten abgerufen werden soll.
    - Die Einheiten, in denen die Wetterdaten zurückgegeben werden sollen.
    - Ihr persönlicher API-Schlüssel.

    Nachdem die angeforderten Wetterdaten abgerufen wurden, wird die [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Eigenschaft der Seite auf das `WeatherData`-Objekt festgelegt. Weitere Informationen zur `BindingContext`-Eigenschaft finden Sie unter [Xamarin.Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md) im Abschnitt [Bindungen mit Bindungskontext](~/xamarin-forms/app-fundamentals/data-binding/basic-bindings.md#bindings-with-a-binding-context).

    > [!IMPORTANT]
    > Die Eigenschaft [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) wird über die visuelle Struktur vererbt. Da sie auf das [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt festgelegt wurde, erben deshalb untergeordnete Objekte von der `ContentPage`-Klasse ihren Wert, darunter auch die [`Label`](xref:Xamarin.Forms.Label)-Instanzen.

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten: Tippen Sie auf das [`Button`](xref:Xamarin.Forms.Button)-Objekt, um aktuelle Wetterdaten für Seattle abzurufen:

    [![Screenshot der Wetterdaten für Seattle unter iOS und Android](../images/consume-web-service.png "Wetterdaten für Seattle")](../images/consume-web-service-large.png#lightbox "Wetterdaten für Seattle")

    > [!IMPORTANT]
    > Ihr persönlicher API-Schlüssel für OpenWeatherMap muss als Wert der Konstante `OpenWeatherMapAPIKey` in der Klasse `Constants` festgelegt werden.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Doppelklicken Sie im **Lösungspad** im Projekt **WebServiceTutorial** auf die Datei **MainPage.xaml**, um sie zu öffnen. Entfernen Sie dann in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="WebServiceTutorial.MainPage">
        <Grid Margin="20,35,20,20">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="0.4*" />
                <ColumnDefinition Width="0.6*" />
            </Grid.ColumnDefinitions>
            <Grid.RowDefinitions>
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
            </Grid.RowDefinitions>
            <Entry x:Name="cityEntry"
                   Grid.ColumnSpan="2"
                   Text="Seattle" />
            <Button Grid.ColumnSpan="2"
                    Grid.Row="1"
                    Text="Get Weather"
                    Clicked="OnButtonClicked" />                
            <Label Grid.Row="2"
                   Text="Location:" />
            <Label Grid.Row="2"
                   Grid.Column="1"
                   Text="{Binding Title}" />            
            <Label Grid.Row="3"
                   Text="Temperature:" />
            <Label Grid.Row="3"
                   Grid.Column="1"
                   Text="{Binding Main.Temperature}" />            
            <Label Grid.Row="4"
                   Text="Wind Speed:" />
            <Label Grid.Row="4"
                   Grid.Column="1"
                   Text="{Binding Wind.Speed}" />            
            <Label Grid.Row="5"
                   Text="Humidity:" />
            <Label Grid.Row="5"
                   Grid.Column="1"
                   Text="{Binding Main.Humidity}" />            
            <Label Grid.Row="6"
                   Text="Visibility:" />
            <Label Grid.Row="6"
                   Grid.Column="1"
                   Text="{Binding Weather[0].Visibility}" />
        </Grid>
    </ContentPage>
    ```

    Dieser Code definiert deklarativ die Benutzeroberfläche für die Seite, die aus einer [`Entry`](xref:Xamarin.Forms.Entry)-Klasse, einem [`Button`](xref:Xamarin.Forms.Button)-Objekt und einer Reihe von [`Label`](xref:Xamarin.Forms.Label)-Instanzen in einer [`Grid`](xref:Xamarin.Forms.Grid)-Klasse besteht. Die `Entry`-Klasse ist bereits mit „Seattle“ gefüllt, indem dessen [`Text`](xref:Xamarin.Forms.InputView.Text)-Eigenschaft festgelegt wird. Das `Button`-Objekt legt sein [`Clicked`](xref:Xamarin.Forms.Button.Clicked)-Ereignis auf einen Ereignishandler namens `OnButtonClicked` fest, der im nächsten Schritt erstellt wird. Die Hälfte der `Label`-Instanzen zeigen statischen Text an. Die andere Hälfte der Instanzen stellt Datenbindung mit den `WeatherData`-Eigenschaften her. Während der Laufzeit suchen die `Label`-Instanzen, die Datenbindung verwenden, in ihren entsprechenden [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Eigenschaften nach dem `WeatherData`-Objekt, das in ihren Bindungsausdrücken verwendet werden soll. Weitere Informationen zur Datenbindung finden Sie unter [Xamarin.Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md).

    Außerdem hat die [`Entry`](xref:Xamarin.Forms.Entry)-Klasse einen Namen, der mit dem `x:Name`-Attribut festgelegt wird. Dadurch kann die CodeBehind-Datei mithilfe des zugewiesenen Namens auf das Objekt zugreifen.

    Weitere Informationen zur Verwendung von REST-basierten Webdiensten in Xamarin.Forms finden Sie in [Consume a RESTful Web Service (Verwenden eines RESTful-Webdienstes)](~/xamarin-forms/data-cloud/web-services/rest.md).

1. Erweitern Sie **MainPage.xaml** im **Lösungspad** im Projekt **WebServiceTutorial**, und doppelklicken Sie dann auf die Datei **MainPage.xaml.cs**, um sie zu öffnen. Entfernen Sie dann in **MainPage.xaml.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace WebServiceTutorial
    {
        public partial class MainPage : ContentPage
        {
            RestService _restService;

            public MainPage()
            {
                InitializeComponent();
                _restService = new RestService();
            }

            async void OnButtonClicked(object sender, EventArgs e)
            {
                if (!string.IsNullOrWhiteSpace(cityEntry.Text))
                {
                    WeatherData weatherData = await _restService.GetWeatherDataAsync(GenerateRequestUri(Constants.OpenWeatherMapEndpoint));
                    BindingContext = weatherData;
                }
            }

            string GenerateRequestUri(string endpoint)
            {
                string requestUri = endpoint;
                requestUri += $"?q={cityEntry.Text}";
                requestUri += "&units=imperial"; // or units=metric
                requestUri += $"&APPID={Constants.OpenWeatherMapAPIKey}";
                return requestUri;
            }
        }
    }
    ```

    Die `OnButtonClicked`-Methode, die ausgeführt wird, wenn auf das [`Button`](xref:Xamarin.Forms.Button)-Objekt getippt wird, ruft die `RestService.GetWeatherDataAsync`-Methode auf, damit diese Wetterdaten für die Stadt abruft, deren Name in der [`Entry`](xref:Xamarin.Forms.Entry)-Klasse eingegeben wurde. Die `GetWeatherDataAsync`-Methode erfordert ein `string`-Argument, das den URI der Web-API darstellt, die aufgerufen wird. Dies wird von der `GenerateRequestUri`-Methode erstellt. Diese Methode nimmt die Endpunktadresse, die in der `OpenWeatherMapEndpoint`-Konstante gespeichert ist, und fügt die Abfrageparameter hinzu, die Folgendes angeben:

    - Die Stadt, für die Wetterdaten abgerufen werden soll.
    - Die Einheiten, in denen die Wetterdaten zurückgegeben werden sollen.
    - Ihr persönlicher API-Schlüssel.

    Nachdem die angeforderten Wetterdaten abgerufen wurden, wird die [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Eigenschaft der Seite auf das `WeatherData`-Objekt festgelegt. Weitere Informationen zur `BindingContext`-Eigenschaft finden Sie unter [Xamarin.Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md) im Abschnitt [Bindungen mit Bindungskontext](~/xamarin-forms/app-fundamentals/data-binding/basic-bindings.md#bindings-with-a-binding-context).

    > [!IMPORTANT]
    > Die Eigenschaft [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) wird über die visuelle Struktur vererbt. Da sie auf das [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt festgelegt wurde, erben deshalb untergeordnete Objekte von der `ContentPage`-Klasse ihren Wert, darunter auch die [`Label`](xref:Xamarin.Forms.Label)-Instanzen.

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten: Tippen Sie auf das [`Button`](xref:Xamarin.Forms.Button)-Objekt, um aktuelle Wetterdaten für Seattle abzurufen:

    [![Screenshot der Wetterdaten für Seattle unter iOS und Android](../images/consume-web-service.png "Wetterdaten für Seattle")](../images/consume-web-service-large.png#lightbox "Wetterdaten für Seattle")

    > [!IMPORTANT]
    > Ihr persönlicher API-Schlüssel für OpenWeatherMap muss als Wert der Konstante `OpenWeatherMapAPIKey` in der Klasse `Constants` festgelegt werden.

    Weitere Informationen zur Verwendung von REST-basierten Webdiensten in Xamarin.Forms finden Sie in [Consume a RESTful Web Service (Verwenden eines RESTful-Webdienstes)](~/xamarin-forms/data-cloud/web-services/rest.md).
