---
ms.openlocfilehash: 3b1603b6af5ebb5558c3cd764f41fdbe24351b9b
ms.sourcegitcommit: 3f0e4f10e5def19122588bb05f26ab2baa9df6eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2020
ms.locfileid: "68669659"
---
REST-Anforderungen erfolgen über HTTP mit den gleichen HTTP-Verben, mit denen Webbrowser Seiten abrufen und Daten an Server senden. In dieser Übung erstellen Sie eine Klasse, die mit dem GET-Verb Daten aus der Web-API [OpenWeatherMap](https://openweathermap.org/) abruft. Diese Web-API kann zum Abrufen von Wettervorhersagedaten für einen bestimmten Standort verwendet werden. Diese Web-API erfordert, dass Sie sich für einen API-Schlüssel registrieren.

> [!div class="nextstepaction"]
> [Für einen API-Schlüssel registrieren](https://home.openweathermap.org/users/sign_up)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Fügen Sie im **Projektmappen-Explorer** dem Projekt **WebServiceTutorial** eine neue Klasse mit dem Namen `Constants` hinzu. Entfernen Sie dann in **Constants.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```csharp
    namespace WebServiceTutorial
    {
        public static class Constants
        {
            public const string OpenWeatherMapEndpoint = "https://api.openweathermap.org/data/2.5/weather";
            public const string OpenWeatherMapAPIKey = "INSERT_API_KEY_HERE";
        }
    }
    ```

    Dieser Code definiert zwei Konstanten. Die Konstante `OpenWeatherMapEndpoint` definiert den Endpunkt, für den Webanforderungen ausgeführt werden, und die Konstante `OpenWeatherMapAPIKey` definiert Ihren persönlichen API-Schlüssel für den Dienst [OpenWeatherMap](https://openweathermap.org/).

    > [!IMPORTANT]
    > Sie müssen Ihren persönlichen API-Schlüssel für OpenWeatherMap als Wert der Konstante `OpenWeatherMapAPIKey` festlegen.

1. Fügen Sie im **Projektmappen-Explorer** dem Projekt **WebServicesTutorial** eine neue Klasse mit dem Namen `WeatherData` hinzu. Entfernen Sie dann in **WeatherData.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```csharp
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class WeatherData
        {
            [JsonProperty("name")]
            public string Title { get; set; }

            [JsonProperty("weather")]
            public Weather[] Weather { get; set; }

            [JsonProperty("main")]
            public Main Main { get; set; }

            [JsonProperty("visibility")]
            public long Visibility { get; set; }

            [JsonProperty("wind")]
            public Wind Wind { get; set; }
        }

        public class Main
        {
            [JsonProperty("temp")]
            public double Temperature { get; set; }

            [JsonProperty("humidity")]
            public long Humidity { get; set; }
        }

        public class Weather
        {
            [JsonProperty("main")]
            public string Visibility { get; set; }
        }

        public class Wind
        {
            [JsonProperty("speed")]
            public double Speed { get; set; }
        }
    }
    ```

    Dieser Code definiert vier Klassen, die verwendet werden, um die vom Webdienst abgerufenen JSON-Daten zu modellieren. Jede Eigenschaft verfügt über ein `JsonProperty`-Attribut, das einen JSON-Feldnamen enthält. Beim Deserialisieren der JSON-Daten in Modellobjekte verwendet „Newtonsoft.Json“ diese Zuordnung von JSON-Feldnamen und CLR-Eigenschaften.

    > [!NOTE]
    > Die obigen Klassendefinitionen wurden vereinfacht und modellieren die vom Webdienst abgerufenen JSON-Daten nicht vollständig. Ein Beispiel für ein vollständiges Datenmodell finden Sie unter [Weather App](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/weather/).

1. Fügen Sie im **Projektmappen-Explorer** dem Projekt **WebServiceTutorial** eine neue Klasse mit dem Namen `RestService` hinzu. Entfernen Sie dann in **RestService.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```csharp
    using System;
    using System.Diagnostics;
    using System.Net.Http;
    using System.Threading.Tasks;
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class RestService
        {
            HttpClient _client;

            public RestService()
            {
                _client = new HttpClient();
            }

            public async Task<WeatherData> GetWeatherDataAsync(string uri)
            {
                WeatherData weatherData = null;
                try
                {
                    HttpResponseMessage response = await _client.GetAsync(uri);
                    if (response.IsSuccessStatusCode)
                    {
                        string content = await response.Content.ReadAsStringAsync();
                        weatherData = JsonConvert.DeserializeObject<WeatherData>(content);
                    }
                }
                catch (Exception ex)
                {
                    Debug.WriteLine("\tERROR {0}", ex.Message);
                }

                return weatherData;
            }
        }
    }
    ```

    Dieser Code definiert als einzige Methode `GetWeatherDataAsync`, die Wetterdaten für einen bestimmten Standort aus der Web-API von [OpenWeatherMap](https://openweathermap.org/) abruft. Diese Methode verwendet die Methode `HttpClient.GetAsync`, um eine GET-Anforderung an die Web-API zu senden, die vom Argument `uri` angegeben wird. Die Web-API sendet eine Antwort, die in einem `HttpResponseMessage`-Objekt gespeichert wird. Die Antwort enthält einen HTTP-Statuscode, der angibt, ob die HTTP-Anforderung erfolgreich war oder fehlgeschlagen ist. Wenn die Anforderung erfolgreich war, antwortet die Web-API mit dem HTTP-Statuscode 200 (OK) und einer JSON-Antwort, die in der Eigenschaft `HttpResponseMessage.Content` enthalten ist. Diese JSON-Daten werden mithilfe der Methode `HttpContent.ReadAsStringAsync` in ein `string`-Element gelesen und anschließend mithilfe der Methode `JsonConvert.DeserializeObject` in ein `WeatherData`-Objekt deserialisiert. Diese Methode verwendet für die Deserialisierung die Zuordnungen zwischen JSON-Feldnamen und CLR-Eigenschaften, die in der `WeatherData`-Klasse definiert sind.

1. Erstellen Sie die Projektmappe, um sich zu vergewissern, dass keine Fehler vorliegen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Fügen Sie im **Lösungspad** dem Projekt **WebServiceTutorial** eine neue Klasse mit dem Namen `Constants` hinzu. Entfernen Sie dann in **Constants.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```csharp
    namespace WebServiceTutorial
    {
        public static class Constants
        {
            public static string OpenWeatherMapEndpoint = "https://api.openweathermap.org/data/2.5/weather";
            public static string OpenWeatherMapAPIKey = "INSERT_API_KEY_HERE";
        }
    }
    ```

    Dieser Code definiert zwei Konstanten. Die Konstante `OpenWeatherMapEndpoint` definiert den Endpunkt, für den Webanforderungen ausgeführt werden, und die Konstante `OpenWeatherMapAPIKey` definiert Ihren persönlichen API-Schlüssel für den Dienst [OpenWeatherMap](https://openweathermap.org/).

    > [!IMPORTANT]
    > Sie müssen Ihren persönlichen API-Schlüssel für OpenWeatherMap als Wert der Konstante `OpenWeatherMapAPIKey` festlegen.

1. Fügen Sie im **Lösungspad** dem Projekt **WebServicesTutorial** eine neue Klasse mit dem Namen `WeatherData` hinzu. Entfernen Sie dann in **WeatherData.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```csharp
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class WeatherData
        {
            [JsonProperty("name")]
            public string Title { get; set; }

            [JsonProperty("weather")]
            public Weather[] Weather { get; set; }

            [JsonProperty("main")]
            public Main Main { get; set; }

            [JsonProperty("visibility")]
            public long Visibility { get; set; }

            [JsonProperty("wind")]
            public Wind Wind { get; set; }
        }

        public class Main
        {
            [JsonProperty("temp")]
            public double Temperature { get; set; }

            [JsonProperty("humidity")]
            public long Humidity { get; set; }
        }

        public class Weather
        {
            [JsonProperty("main")]
            public string Visibility { get; set; }
        }

        public class Wind
        {
            [JsonProperty("speed")]
            public double Speed { get; set; }
        }
    }
    ```

    Dieser Code definiert vier Klassen, die verwendet werden, um die vom Webdienst abgerufenen JSON-Daten zu modellieren. Jede Eigenschaft verfügt über ein `JsonProperty`-Attribut, das einen JSON-Feldnamen enthält. Beim Deserialisieren der JSON-Daten in Modellobjekte verwendet „Newtonsoft.Json“ diese Zuordnung von JSON-Feldnamen und CLR-Eigenschaften.

    > [!NOTE]
    > Die obigen Klassendefinitionen wurden vereinfacht und modellieren die vom Webdienst abgerufenen JSON-Daten nicht vollständig. Ein Beispiel für ein vollständiges Datenmodell finden Sie unter [Weather App](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/weather/).

1. Fügen Sie im **Lösungspad** dem Projekt **WebServiceTutorial** eine neue Klasse mit dem Namen `RestService` hinzu. Entfernen Sie dann in **RestService.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```csharp
    using System;
    using System.Diagnostics;
    using System.Net.Http;
    using System.Threading.Tasks;
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class RestService
        {
            HttpClient _client;

            public RestService()
            {
                _client = new HttpClient();
            }

            public async Task<WeatherData> GetWeatherDataAsync(string uri)
            {
                WeatherData weatherData = null;
                try
                {
                    HttpResponseMessage response = await _client.GetAsync(uri);
                    if (response.IsSuccessStatusCode)
                    {
                        string content = await response.Content.ReadAsStringAsync();
                        weatherData = JsonConvert.DeserializeObject<WeatherData>(content);
                    }
                }
                catch (Exception ex)
                {
                    Debug.WriteLine("\tERROR {0}", ex.Message);
                }

                return weatherData;
            }
        }
    }
    ```

    Dieser Code definiert als einzige Methode `GetWeatherDataAsync`, die Wetterdaten für einen bestimmten Standort aus der Web-API von [OpenWeatherMap](https://openweathermap.org/) abruft. Diese Methode verwendet die Methode `HttpClient.GetAsync`, um eine GET-Anforderung an die Web-API zu senden, die vom Argument `uri` angegeben wird. Die Web-API sendet eine Antwort, die in einem `HttpResponseMessage`-Objekt gespeichert wird. Die Antwort enthält einen HTTP-Statuscode, der angibt, ob die HTTP-Anforderung erfolgreich war oder fehlgeschlagen ist. Wenn die Anforderung erfolgreich war, antwortet die Web-API mit dem HTTP-Statuscode 200 (OK) und einer JSON-Antwort, die in der Eigenschaft `HttpResponseMessage.Content` enthalten ist. Diese JSON-Daten werden mithilfe der Methode `HttpContent.ReadAsStringAsync` in ein `string`-Element gelesen und anschließend mithilfe der Methode `JsonConvert.DeserializeObject` in ein `WeatherData`-Objekt deserialisiert. Diese Methode verwendet für die Deserialisierung die Zuordnungen zwischen JSON-Feldnamen und CLR-Eigenschaften, die in der `WeatherData`-Klasse definiert sind.

1. Erstellen Sie die Projektmappe, um sich zu vergewissern, dass keine Fehler vorliegen.
