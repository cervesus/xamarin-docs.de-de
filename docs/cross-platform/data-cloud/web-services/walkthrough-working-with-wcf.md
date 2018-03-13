---
title: "Exemplarische Vorgehensweise – arbeiten mit WCF"
description: In dieser exemplarischen Vorgehensweise wird behandelt, wie eine mobile Anwendung mit Xamarin erstellten einen WCF-Webdienst mithilfe der Klasse BasicHttpBinding nutzen kann.
ms.topic: article
ms.prod: xamarin
ms.assetid: D0E83342-2E79-4D25-BD57-43718F5628C4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 02/17/2018
ms.openlocfilehash: b076c7a71d81a474ca80ac32771d5512c21c167c
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="walkthrough---working-with-wcf"></a>Exemplarische Vorgehensweise – arbeiten mit WCF

_In dieser exemplarischen Vorgehensweise wird behandelt, wie eine mobile Anwendung mit Xamarin erstellten einen WCF-Webdienst mithilfe der Klasse BasicHttpBinding nutzen kann._


Es ist eine häufige Anforderung für mobile Anwendungen mit Back-End-Systemen kommunizieren können. Es gibt viele Optionen und Optionen für das Back-End-Frameworks, von denen [Windows Communication Foundation](http://msdn.microsoft.com/en-us/library/ms731082.aspx) (WCF). In dieser exemplarischen Vorgehensweise bietet ein Beispiel, wie ein WCF-Diensts mithilfe eine mobile Anwendung von Xamarin kann nutzen die `BasicHttpBinding` Klasse. Die exemplarische Vorgehensweise enthält die folgenden Themen:

1.  **Erstellen Sie einen WCF-Dienst** – In diesem Abschnitt erstellen wir einen sehr einfachen WCF-Dienst mit zwei Methoden. Die erste Methode dauert einen Zeichenfolgenparameter, während die andere Methode ein c#-Objekt dauert. In diesem Abschnitt besprechen auch so konfigurieren Sie ein Entwickler-Arbeitsstation, um Remotezugriff auf den WCF-Dienst zu ermöglichen.
1.  **Erstellen Sie eine Anwendung Xamarin.Android** – nachdem der WCF-Dienst erstellt wurde, erstellen wir eine einfache Xamarin.Android-Anwendung, die den WCF-Dienst nutzt. Dieser Abschnitt befasst sich mit WCF-Dienstklasse Proxy zur Vereinfachung der Kommunikation mit dem WCF-Dienst zu erstellen.
1.  **Erstellen Sie eine Anwendung Xamarin.iOS** – der letzte Teil dieses Lernprogramms umfasst das Erstellen einer einfachen Xamarin.iOS-Anwendung, die den WCF-Dienst nutzt.

<a name="Requirements" />

## <a name="requirements"></a>Anforderungen

In dieser exemplarischen Vorgehensweise wird davon ausgegangen, dass Sie beim Erstellen und Verwenden von WCF-Diensten vertraut sind.

<a name="Creating_a_WCF_Service" />

## <a name="creating-a-wcf-service"></a>Erstellen eines WCF-Diensts

Bevor wir die erste Aufgabe besteht darin einen WCF-Dienst für eine mobile Anwendungen für die Kommunikation mit erstellen.

1. Starten Sie Visual Studio 2017, und erstellen Sie ein neues Projekt.
1. In der **neues Projekt** wählen Sie im Dialogfeld die **WCF > WCF-Dienstbibliothek** Vorlage und den Namen der Projektmappe `HelloWorldService`:

  ![](walkthrough-working-with-wcf-images/new-wcf-service.png "Erstellen einer neuen WCF-Dienstbibliothek")

1. In **Projektmappen-Explorer**, fügen Sie eine neue Klasse mit dem Namen `HelloWorldData` zum Projekt:

        using System.Runtime.Serialization;

        namespace HelloWorldService
        {
            [DataContract]
            public class HelloWorldData
            {
                [DataMember]
                public bool SayHello { get; set; }

                [DataMember]
                public string Name { get; set; }

                public HelloWorldData()
                {
                    Name = "Hello ";
                    SayHello = false;
                }
            }
        }

1. In **Projektmappen-Explorer**, benennen Sie `IService1.cs` auf `IHelloWorldService.cs`, und benennen Sie `Service1.cs` auf `HelloWorldService.cs`.
1. In **Projektmappen-Explorer**öffnen `IHelloWorldService.cs` und Ersetzen Sie den Code durch folgenden Code:

        using System.ServiceModel;

        namespace HelloWorldService
        {
            [ServiceContract]
            public interface IHelloWorldService
            {
                [OperationContract]
                string SayHelloTo(string name);

                [OperationContract]
                HelloWorldData GetHelloData(HelloWorldData helloWorldData);
            }
        }

    Dieser Dienst bietet zwei Methoden: eine, die eine Zeichenfolge für einen Parameter und einen anderen akzeptiert ein akzeptiert.

1. In **Projektmappen-Explorer**öffnen `HelloWorldService.cs` und Ersetzen Sie den Code durch folgenden Code:

        using System;

        namespace HelloWorldService
        {
            public class HelloWorldService : IHelloWorldService
            {
                public HelloWorldData GetHelloData(HelloWorldData helloWorldData)
                {
                    if (helloWorldData == null)
                        throw new ArgumentException("helloWorldData");

                    if (helloWorldData.SayHello)
                        helloWorldData.Name = "Hello World to {helloWorldData.Name}";

                    return helloWorldData;
                }

                public string SayHelloTo(string name)
                {
                    return "Hello World to you, {name}";
                }
            }
        }

1. In **Projektmappen-Explorer**öffnen `App.config`, Aktualisieren der `name` Attribut des der `<service>` Knoten, die `contract` Attribut des der `<endpoint>` Knoten, und die `baseAddress` Attribut von der `<add>` Knoten:

        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
            ...
            <services>
              <service name="HelloWorldService.HelloWorldService">
                <endpoint address="" binding="basicHttpBinding" contract="HelloWorldService.IHelloWorldService">
                  <identity>
                    <dns value="localhost" />
                  </identity>
                </endpoint>
                <endpoint address="mex" binding="mexHttpBinding" contract="IMetadataExchange" />
                <host>
                  <baseAddresses>
                    <add baseAddress="http://localhost:8733/Design_Time_Addresses/HelloWorldService/" />
                  </baseAddresses>
                </host>
              </service>
            </services>
            ...
        </configuration>

1. Erstellen Sie und führen Sie den WCF-Dienst. Der Dienst wird von WCF-Testclient gehostet werden:

  ![](walkthrough-working-with-wcf-images/hosted-wcf-service.png "WCF-Dienst ausgeführt wird, im Testclient")

1. Klicken Sie mit den WCF-Testclient ausgeführt wird starten Sie einen Browser, und navigieren Sie auf den Endpunkt für den WCF-Dienst:

  ![](walkthrough-working-with-wcf-images/wcf-service-browser.png "WCF-Dienst-Browser-Informationsseite")

> [!IMPORTANT]
> **Hinweis:** im folgende Abschnitt ist nur erforderlich, wenn Sie Remoteverbindungen auf einer Arbeitsstation Windows 10 müssen. Der Abschnitt kann ignoriert werden, wenn Sie eine alternative Plattform auf dem den WCF-Dienst bereitgestellt haben.

<a name="Allow_Remote_Access_to_IIS_Express" />

## <a name="configuring-remote-access-to-iis-express"></a>Konfigurieren von Remotezugriff auf IIS Express

Hosten eines WCFS lokal ist ausreichend, wenn Verbindungen nur aus dem lokalen Computer stammen. Remote-Geräten (z. B. einem Android-Gerät oder einem iPhone) müssen jedoch keine Zugriff auf einen lokalen WCF-Dienst. Aus diesem Grund wird erläutert, wie Windows 10 und IIS Express, um Remoteverbindungen zu konfigurieren:

1.  **Konfigurieren von IIS Express beim Annehmen von Remoteverbindungen** – dieser Schritt umfasst, bearbeiten die Config-Datei für IIS Express, um Remoteverbindungen auf einem bestimmten Port zu akzeptieren, und klicken Sie dann das Einrichten einer Regel für IIS Express, um den eingehenden Datenverkehr zu akzeptieren.
1.  **Windows-Firewall eine Ausnahme hinzufügen** -Sie müssen einen Port über die Windows-Firewall Remoteanwendungen verwenden können, um die Kommunikation mit dem WCF-Dienst öffnen.

Sie müssen die IP-Adresse Ihrer Arbeitsstation zu kennen. Im Rahmen dieses Beispiels wird davon ausgegangen, dass unsere Workstation die IP-Adresse 192.168.1.143 hat.

1. Beginnen wir mit IIS Express zum Abhören von externen Anforderungen konfigurieren. Wir hierzu bearbeiten die Konfigurationsdatei für IIS Express unter `[solutiondirectory]\.vs\config\applicationhost.config`, wie im folgenden Screenshot gezeigt:

    [![](walkthrough-working-with-wcf-images/image05.png "Wir dazu durch Bearbeiten der Konfigurationsdatei für IIS Express unter solutiondirectory.vsconfigapplicationhost.config, wie in diesem Screenshot dargestellt.")](walkthrough-working-with-wcf-images/image05.png#lightbox)


    Suchen Sie die `site` Element mit dem Namen `HelloWorldWcfHost`. Es sollte etwa wie im folgenden XML-Codeausschnitt aussehen:

        <site name="HelloWorldWcfHost" id="2">
            <application path="/" applicationPool="Clr4IntegratedAppPool">
                <virtualDirectory path="/" physicalPath="\\vmware-host\Shared Folders\tom\work\xamarin\code\private-samples\webservices\HelloWorld\HelloWorldWcfHost" />
            </application>
            <bindings>
                <binding protocol="http" bindingInformation="*:8733:localhost" />
            </bindings>
        </site>

    Wir müssen einen anderen hinzufügen `binding` Port 8734 für externe Datenverkehr zu öffnen. Die folgenden XML-Code zum Hinzufügen der `bindings` -Element, und Ersetzen Sie die IP-Adresse durch Ihre eigene IP-Adresse:

        <binding protocol="http" bindingInformation="*:8734:192.168.1.143" />

    Dadurch wird IIS Express zum Akzeptieren von HTTP-Datenverkehr von allen remote IP-Adressen an Port 8734 auf die externe IP-Adresse des Computers konfiguriert. Das obige Ausschnitt wird davon ausgegangen, dass die IP-Adresse des Computers mit IIS Express 192.168.1.143 ist. Nachdem die Änderungen der `bindings` -Element sollte wie folgt aussehen:

        <site name="HelloWorldWcfHost" id="2">
            <application path="/" applicationPool="Clr4IntegratedAppPool">
                <virtualDirectory path="/" physicalPath="\\vmware-host\Shared Folders\tom\work\xamarin\code\private-samples\webservices\HelloWorld\HelloWorldWcfHost" />
            </application>
            <bindings>
                <binding protocol="http" bindingInformation="*:8733:localhost" />
                <binding protocol="http" bindingInformation="*:8734:192.168.1.143" />
            </bindings>
        </site>

1. Wir als Nächstes konfigurieren Sie IIS Express eingehende Verbindungen an Port 8734 akzeptieren. Start up eine administratoreingabeaufforderung, und führen Sie diesen Befehl:

    `> netsh http add urlacl url=http://192.168.1.143:9608/ user=everyone`

1. Der letzte Schritt ist zum Konfigurieren der Windows-Firewall, um externe Datenverkehr über Port 8734 zuzulassen. Führen Sie an einer administrativen Eingabeaufforderung den folgenden Befehl ein:

    `> netsh advfirewall firewall add rule name="IISExpressXamarin" dir=in protocol=tcp localport=8734 profile=private remoteip=localsubnet action=allow`

    Dieser Befehl lässt eingehenden Datenverkehr über Port 8734 auf allen Geräten im gleichen Subnetz wie die Windows 10-Arbeitsstation.

Sie haben eine sehr grundlegende in IIS Express, die akzeptiert eingehende Verbindungen von anderen Geräten oder Computern unsere Subnetz gehosteten WCF-Diensts erstellt. Sie können diese Out testen, indem Sie die Anwendung ausführen und besuchen `http://localhost:8733/Design_Time_Addresses/HelloWorldService/` auf der Arbeitsstation und `http://192.168.1.143:8734/Design_Time_Addresses/HelloWorldService/` von einem anderen Computer im Subnetz.

Damit wird IIS Express ausgeführt wird und den Dienst bedient erhalten bleiben sollen, deaktivieren Sie die **bearbeiten und Fortfahren** option *Projekteigenschaften > Web > Debugger*.

## <a name="creating-a-proxy-for-the-web-service"></a>Erstellen einen Proxy für den Webdienst

Für den WCF-Dienst muss ein Webdienstproxy erstellt werden, bevor eine Anwendung den Dienst verwenden kann. Dies kann folgendermaßen erfüllt werden:

1. Hinzufügen einer standardmäßigen .NET Klassenbibliothek, die mit dem Namen `HelloWorldServiceProxy`, und löschen Sie alle Klassen im Projekt.
1. Führen Sie die `HelloWorldService` Projekt.
1. Mit der `HelloWorldService` Projekt ausführen, fügen Sie einen neuen **verbundenen Dienst** für das Projekt, mit der **Microsoft WCF Web Service Verweisdatenanbieter**.
1. In der **Dienstendpunkt** auf der Registerkarte die **WCF-Web-Dienstverweis konfigurieren** Dialogfeld klicken Sie auf die **Discover** Schaltfläche, löschen Sie `mex` vom Ende des erkannten Endpunkt in der **URI** Dropdown-Menü, geben Sie `HelloWorldServiceProxy` als die **Namespace**, und klicken Sie auf die **Weiter** Schaltfläche.
1. In der **Datentypoptionen** auf der Registerkarte die **WCF-Web-Dienstverweis konfigurieren** Dialogfeld, akzeptieren Sie die Standardeinstellungen, indem Sie auf die **Weiter** Schaltfläche.
1. In der **Clientoptionen** auf der Registerkarte die **WCF-Web-Dienstverweis konfigurieren** im Dialogfeld sicher, dass die **öffentlichen** aktiviert ist, und klicken Sie auf die **Fertig stellen**  Schaltfläche.
1. Erstellen der `HelloWorldServiceProxy` Projekt.

> [!NOTE]
> **Hinweis**: eine Alternative zum Erstellen des Proxys, die mit dem Microsoft WCF Web Verweis Dienstanbieter in Visual Studio 2017 wird das ServiceModel Metadata Utility Tool (svcutil.exe) verwenden. Weitere Informationen finden Sie unter [ServiceModel Metadata Utility Tool (Svcutil.exe)](https://docs.microsoft.com/en-us/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe).

<a name="Creating_a_Xamarin_Android_Application" />

## <a name="creating-a-xamarinandroid-application"></a>Erstellen einer Anwendung Xamarin.Android

Der WCF-Dienstproxy kann wie folgt von einer Anwendung Xamarin.Android genutzt werden:

1. Fügen Sie in Visual Studio ein neues, leeres Android-Projekt zur Projektmappe hinzu, und nennen Sie sie `HelloWorld.Android`.
1. In der `HelloWorld.Android` Projekt, fügen einen Verweis auf die `HelloWorldServiceProxy` Projekt und einem Verweis auf die `System.ServiceModel` Namespace.
1. In **Projektmappen-Explorer**öffnen `Resources/layout/main.axml` , und Ersetzen Sie den vorhandenen XML-Code durch folgendes XML:

        <?xml version="1.0" encoding="utf-8"?>
        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
                  android:orientation="vertical"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent">
            <LinearLayout
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="0px"
                    android:layout_weight="1">
                <Button
                        android:id="@+id/sayHelloWorldButton"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:text="@string/say_hello_world" />
                <TextView
                        android:text="Large Text"
                        android:textAppearance="?android:attr/textAppearanceLarge"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:id="@+id/sayHelloWorldTextView" />
            </LinearLayout>
            <LinearLayout
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="0px"
                    android:layout_weight="1">
                <Button
                        android:id="@+id/getHelloWorldDataButton"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:text="@string/get_hello_world_data" />
                <TextView
                        android:text="Large Text"
                        android:textAppearance="?android:attr/textAppearanceLarge"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:id="@+id/getHelloWorldDataTextView" />
            </LinearLayout>
        </LinearLayout>

    Die folgenden Screenshots zeigt die Benutzeroberfläche im Designer:

    [![](walkthrough-working-with-wcf-images/image09.png "Dies ist ein Screenshot, der dieser Benutzeroberfläche im Designer illustriert")](walkthrough-working-with-wcf-images/image09.png#lightbox)

1. In **Projektmappen-Explorer**öffnen `Resources/values/Strings.xml` und fügen Sie das folgende XML hinzu:

        <string name="say_hello_world">Say Hello World</string>
        <string name="get_hello_world_data">Get Hello World data</string>

1. In **Projektmappen-Explorer**öffnen `MainActivity.cs` und Ersetzen Sie vorhandenen Code durch folgenden Code:

        [Activity(Label = "HelloWorld.Android", MainLauncher = true)]
        public class MainActivity : Activity
        {
            static readonly EndpointAddress Endpoint = new EndpointAddress("<insert_WCF_service_endpoint_here>");

            HelloWorldServiceClient _client;
            Button _getHelloWorldDataButton;
            TextView _getHelloWorldDataTextView;
            Button _sayHelloWorldButton;
            TextView _sayHelloWorldTextView;
            ...
        }

    Ersetzen Sie `<insert_WCF_service_endpoint_here>` mit der Adresse des WCF-Endpunkt.

1. In `MainActivity.cs`, ändern Sie die `OnCreate` Methode so, dass diese den folgenden Code enthält:

        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(bundle);

            SetContentView(Resource.Layout.Main);

            InitializeHelloWorldServiceClient();

            // This button will invoke the GetHelloWorldData - the method that takes a C# object as a parameter.
            _getHelloWorldDataButton = FindViewById<Button>(Resource.Id.getHelloWorldDataButton);
            _getHelloWorldDataButton.Click += GetHelloWorldDataButtonOnClick;
            _getHelloWorldDataTextView = FindViewById<TextView>(Resource.Id.getHelloWorldDataTextView);

            // This button will invoke SayHelloWorld - this method takes a simple string as a parameter.
            _sayHelloWorldButton = FindViewById<Button>(Resource.Id.sayHelloWorldButton);
            _sayHelloWorldButton.Click += SayHelloWorldButtonOnClick;
            _sayHelloWorldTextView = FindViewById<TextView>(Resource.Id.sayHelloWorldTextView);
        }

    Der obige Code initialisiert die Instanzvariablen für die Klasse, und einige Ereignishandler verbindet.

1. In `MainActivity.cs`, instanziieren Sie die Client-Proxyklasse durch Hinzufügen der folgenden zwei Methoden:

        void InitializeHelloWorldServiceClient()
        {
            BasicHttpBinding binding = CreateBasicHttpBinding();
            _client = new HelloWorldServiceClient(binding, Endpoint);
        }

        static BasicHttpBinding CreateBasicHttpBinding()
        {
            BasicHttpBinding binding = new BasicHttpBinding
            {
                Name = "basicHttpBinding",
                MaxBufferSize = 2147483647,
                MaxReceivedMessageSize = 2147483647
            };

            TimeSpan timeout = new TimeSpan(0, 0, 30);
            binding.SendTimeout = timeout;
            binding.OpenTimeout = timeout;
            binding.ReceiveTimeout = timeout;
            return binding;
        }

    Der obige Code instanziiert und initialisiert ein `HelloWorldServiceClient` Objekt.

1. In `MainActivity.cs`, fügen Sie auch Handler für die beiden Schaltflächen in der `Activity`:

        async void GetHelloWorldDataButtonOnClick(object sender, EventArgs e)
        {
            var data = new HelloWorldData
            {
                Name = "Mr. Chad",
                SayHello = true
            };

            _getHelloWorldDataTextView.Text = "Waiting for WCF...";
            HelloWorldData result;
            try
            {
                result = await _client.GetHelloDataAsync(data);
                _getHelloWorldDataTextView.Text = result.Name;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }

        async void SayHelloWorldButtonOnClick(object sender, EventArgs e)
        {
            _sayHelloWorldTextView.Text = "Waiting for WCF...";
            try
            {
                var result = await _client.SayHelloToAsync("Kilroy");
                _sayHelloWorldTextView.Text = result;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }

1. Führen Sie die Anwendung, stellen Sie sicher, dass der WCF-Dienst ausgeführt wird, und klicken Sie auf die beiden Schaltflächen. Ruft die Anwendung die WCF asynchron ausgeführt wird, bereitgestellt, die die `Endpoint` Feld ordnungsgemäß festgelegt ist:

  [![](walkthrough-working-with-wcf-images/image08.png "Innerhalb von 30 Sekunden eine Antwort empfangen werden soll, von jeder WCF-Methode und die Anwendung sollte in etwa diesen Screenshot aussehen")](walkthrough-working-with-wcf-images/image08.png#lightbox)

<a name="Creating_a_Xamarin_iOS_Application" />

## <a name="creating-a-xamarinios-application"></a>Erstellen einer Anwendung Xamarin.iOS

Der WCF-Dienstproxy kann wie folgt von einer Anwendung Xamarin.iOS genutzt werden:

1. Fügen Sie in Visual Studio eine neue iPhone **einzelne Ansicht Anwendung** Projekt der Projektmappe, und nennen Sie sie `HelloWorld.iOS`.
1. In der `HelloWorld.iOS` Projekt, fügen einen Verweis auf die `HelloWorldServiceProxy` Projekt und einem Verweis auf die `System.ServiceModel` Namespace.
1. In **Projektmappen-Explorer**, doppelklicken Sie auf `Main.storyboard` zum Öffnen der Datei in der iOS-Designer. Fügen Sie dann die folgenden `UIButton` und `UITextView` Steuerelemente:

    <table>
        <thead>
            <tr>
                <td></td>
                <td>name</td>
                <td>Titel</td>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td><code>UIButton</code></td>
                <td><code>sayHelloWorldButton</code></td>
                <td>Sagen Sie "Hello, World"</td>
            </tr>
            <tr>
                <td><code>UITextView</code></td>
                <td><code>sayHelloWorldText</code></td>
                <td></td>
            </tr>
            <tr>
                <td><code>UIButton</code></td>
                <td><code>getHelloWorldDataButton</code></td>
                <td>Abrufen von "Hello, World" Daten</td>
            </tr>
            <tr>
                <td><code>UITextView</code></td>
                <td><code>getHelloWorldDataText</code></td>
                <td></td>
            </tr>
        </tbody>
    </table>

    Nach dem Hinzufügen der Steuerelemente, sollte die Benutzeroberfläche den folgenden Screenshot ähneln:

    [![](walkthrough-working-with-wcf-images/image12.png "Nach dem Hinzufügen der Steuerelemente, sollte die Benutzeroberfläche diesem Screenshot entsprechen.")](walkthrough-working-with-wcf-images/image12.png#lightbox)

1. In **Projektmappen-Explorer**öffnen `ViewController.cs` und fügen Sie den folgenden Code hinzu:

        public partial class ViewController : UIViewController
        {
            static readonly EndpointAddress Endpoint = new EndpointAddress("<insert_WCF_service_endpoint_here>");
            HelloWorldServiceClient _client;
            ...
        }

    Ersetzen Sie `<insert_WCF_service_endpoint_here>` mit der Adresse des WCF-Endpunkt.

1. In `ViewController.cs`, aktualisieren Sie die `ViewDidLoad` Methode, sodass die It die folgenden ähnelt:

        public override void ViewDidLoad()
        {
            base.ViewDidLoad();
            InitializeHelloWorldServiceClient();

            getHelloWorldDataButton.TouchUpInside += GetHelloWorldDataButton_TouchUpInside;
            sayHelloWorldButton.TouchUpInside += SayHelloWorldButton_TouchUpInside;
        }

1. In `ViewController.cs`, Hinzufügen der `InitializeHelloWorldServiceClient` und `CreateBasicHttpBinding` Methoden:

        void InitializeHelloWorldServiceClient()
        {
            BasicHttpBinding binding = CreateBasicHttpBinding();
            _client = new HelloWorldServiceClient(binding, Endpoint);
        }

        static BasicHttpBinding CreateBasicHttpBinding()
        {
            BasicHttpBinding binding = new BasicHttpBinding
            {
                Name = "basicHttpBinding",
                MaxBufferSize = 2147483647,
                MaxReceivedMessageSize = 2147483647
            };

            TimeSpan timeout = new TimeSpan(0, 0, 30);
            binding.SendTimeout = timeout;
            binding.OpenTimeout = timeout;
            binding.ReceiveTimeout = timeout;
            return binding;
        }

1. In `ViewController.cs`, fügen Sie Ereignishandler für die `TouchUpInside` Ereignisse auf die beiden `UIButton` Instanzen:

        async void GetHelloWorldDataButton_TouchUpInside(object sender, EventArgs e)
        {
            getHelloWorldDataText.Text = "Waiting for WCF...";
            var data = new HelloWorldData
            {
                Name = "Mr. Chad",
                SayHello = true
            };

            HelloWorldData result;
            try
            {
                result = await _client.GetHelloDataAsync(data);
                getHelloWorldDataText.Text = result.Name;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }

        async void SayHelloWorldButton_TouchUpInside(object sender, EventArgs e)
        {
            sayHelloWorldText.Text = "Waiting for WCF...";
            try
            {
                var result = await _client.SayHelloToAsync("Kilroy");
                sayHelloWorldText.Text = result;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }

1. Führen Sie die Anwendung, stellen Sie sicher, dass der WCF-Dienst ausgeführt wird, und klicken Sie auf die beiden Schaltflächen. Ruft die Anwendung die WCF asynchron ausgeführt wird, bereitgestellt, die die `Endpoint` Feld ordnungsgemäß festgelegt ist:

    [![](walkthrough-working-with-wcf-images/image10.png "Innerhalb von 30 Sekunden eine Antwort empfangen werden soll, von jeder WCF-Methode und die Anwendung sollte wie folgt diesem Screenshot aussehen")](walkthrough-working-with-wcf-images/image10.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieses Lernprogramm behandelt zum Arbeiten mit einem WCF-Diensts in einer mobilen Anwendung, die mit dem Xamarin.Android und Xamarin.iOS. Es wurde gezeigt, wie einen WCF-Dienst erstellen und erläutert, wie Windows 10 und IIS Express zum Akzeptieren von Verbindungen auf remote-Geräten zu konfigurieren. Anschließend wird erläutert, wie ein WCF-Proxy-Client generiert und veranschaulicht, wie den Proxy-Client in Xamarin.Android und Xamarin.iOS Anwendungen zu verwenden.


## <a name="related-links"></a>Verwandte Links

- [HelloWorld (Beispiel)](https://developer.xamarin.com/samples/mobile/WCF-Walkthrough/)
- [Entwickeln von dienstorientierten Anwendungen mit WCF](https://docs.microsoft.com/en-us/dotnet/framework/wcf/index)
- [Vorgehensweise: erstellen ein Windows Communication Foundation-Clients](https://docs.microsoft.com/en-us/dotnet/framework/wcf/how-to-create-a-wcf-client)
- [ServiceModel Metadata Utility Tool (svcutil.exe)](https://docs.microsoft.com/en-us/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
