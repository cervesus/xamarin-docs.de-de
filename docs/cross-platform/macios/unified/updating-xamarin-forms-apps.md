---
title: Aktualisieren von vorhandenen Xamarin.Forms-Apps
description: "Führen Sie diese Schritte zum Aktualisieren einer vorhandenen Xamarin.Forms-app die einheitliche API, und aktualisieren Sie auf Version 1.3.1"
ms.topic: article
ms.prod: xamarin
ms.assetid: C2F0D1D1-256D-44A4-AAC9-B06A0CB41E70
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 52b53618e23a47884bee6cb821d85b15d759968c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="updating-existing-xamarinforms-apps"></a>Aktualisieren von vorhandenen Xamarin.Forms-Apps

_Führen Sie diese Schritte zum Aktualisieren einer vorhandenen Xamarin.Forms-app die einheitliche API, und aktualisieren Sie auf Version 1.3.1_


> [!IMPORTANT]
> Da Xamarin.Forms 1.3.1 das erste Release, das die einheitliche API unterstützt handelt, sollten die gesamte Projektmappe aktualisiert werden, um die neueste Version zur gleichen Zeit wie das Migrieren der iOS-app auf einheitliche verwenden. Dies bedeutet, dass zusätzlich zum Aktualisieren der iOS-Projekt für die Unterstützung für Unified, außerdem Sie Code in bearbeiten müssen _alle_ Projekte in der Projektmappe.



Das Update wird in zwei Schritten ausgeführt:

1. Migrieren Sie die iOS-app auf die einheitliche API, die mit Visual Studio für Mac Build-Migrationstools.

    - Verwenden Sie das Migrationstool, um das Projekt automatisch aktualisiert.

    - Update iOS systemeigene APIs, wie in den Anweisungen, um [Aktualisieren von iOS-apps](~/cross-platform/macios/unified/updating-ios-apps.md) (insbesondere in benutzerdefinierten Renderer oder Abhängigkeit Dienstcode).

2. Aktualisieren Sie die gesamte Projektmappe in Xamarin.Forms Version 1.3.

    1. Installieren Sie das Xamarin.Forms 1.3.1 NuGet-Paket.

    2. Update der `App` Klasse in den gemeinsam verwendeten Code.

    3. Update der `AppDelegate` im iOS-Projekt.

    4. Update der `MainActivity` im Android-Projekt.

    5. Update der `MainPage` im Windows Phone-Projekt.


# <a name="1-ios-app-unified-migration"></a>1. iOS-App (Unified Migration)

Bei der Migration erfordert ein Upgrade Xamarin.Forms auf Version 1.3, die die einheitliche API unterstützt. Damit die richtigen Assemblyverweise erstellt werden kann müssen wir zunächst die iOS-Projekt zum Verwenden der API Unified zu aktualisieren.

## <a name="migration-tool"></a>Migrationstool

Klicken Sie auf das iOS-Projekt so, dass es aktiviert ist, wählen Sie dann **Projekt > Migrate to Xamarin.iOS einheitliche API...**  und akzeptieren Sie die Warnmeldung, die angezeigt wird.

![](updating-xamarin-forms-apps-images/beta-tool1.png "Wählen Sie Projekt > Migrieren nach Xamarin.iOS einheitliche API..., und akzeptieren Sie die Warnmeldung, die angezeigt wird")

Damit wird automatisch:

 - Ändern Sie den Projekttyp, um die einheitliche 64-Bit-API unterstützen.
 - Ändern Sie den frameworkverweis zu **Xamarin.iOS** (ersetzen die alten **Monotouch** Verweis).
 - Ändern Sie die Namespaceverweise in der Code zum Entfernen der `MonoTouch` Präfix.
 - Update der **Csproj** Datendatei an der richtigen Build-Ziele für die einheitliche API verwendet.

**Fehlerfreie** und **erstellen** des Projekts, um sicherzustellen, dass es sind keine weiteren Fehler zu beheben. Sollte ist keine weitere Aktion erforderlich. Diese Schritte werden ausführlicher in den [einheitliche API Docs](~/cross-platform/macios/unified/updating-ios-apps.md).

## <a name="update-native-ios-apis-if-required"></a>Aktualisieren von systemeigenen iOS-APIs (falls erforderlich)

Wenn Sie zusätzliche iOS systemeigenem Code (z. B. benutzerdefinierte Renderer "oder" Abhängigkeitsdienste ") hinzugefügt haben, müssen Sie zusätzliche manuelle Codekorrekturen durchführen. Erneut kompilieren Sie Ihre app und beziehen sich auf die [Aktualisieren von vorhandenen iOS-Apps Anweisungen](~/cross-platform/macios/unified/updating-ios-apps.md) zusätzliche Informationen zu Änderungen, die möglicherweise erforderlich sind. [Diese Tipps](~/cross-platform/macios/unified/updating-tips.md) hilft auch Änderungen zu identifizieren, die erforderlich sind.


# <a name="2-xamarinforms-131-update"></a>2. Xamarin.Forms 1.3.1 Update

Sobald die iOS-app auf die einheitliche API aktualisiert wurde, muss der Rest der Lösung in Xamarin.Forms Version 1.3.1 aktualisiert werden. Dies umfasst Folgendes:

 - Aktualisieren das Xamarin.Forms-NuGet-Paket in jedem Projekt angelegt.
 - Ändern den Code, um die neue Xamarin.Forms verwenden `Application`, `FormsApplicationDelegate` (iOS), `FormsApplicationActivity` (Android) und `FormsApplicationPage` Klassen (Windows Phone).

Diese Schritte werden im folgenden erläutert:


## <a name="21-update-nuget-in-all-projects"></a>2.1-Update von NuGet in allen Projekten

Aktualisieren Sie Xamarin.Forms auf 1.3.1 Vorabversion mithilfe des NuGet-Paket-Managers für alle Projekte in der Projektmappe: PCL (falls vorhanden), iOS, Android und Windows Phone. Es wird empfohlen, die Sie **löschen und erneut hinzufügen** die Xamarin.Forms-NuGet-Paket zum Aktualisieren von Version 1.3.

**Hinweis:** Xamarin.Forms Version 1.3.1 befindet sich derzeit im *Vorabversion*. Dies bedeutet, Sie müssen auswählen, die **Vorabversion** option NuGet über (ein Tick-Feld in Visual Studio für Mac) oder einer Drop-Dropdownliste in Visual Studio auf die neueste Version der Vorabversion finden Sie unter.


> [!IMPORTANT]
> Wenn Sie Visual Studio verwenden, stellen Sie sicher, dass die neueste Version des NuGet-Paket-Manager installiert ist. Ältere Versionen von NuGet in Visual Studio werden die einheitliche Version des Xamarin.Forms 1.3.1 nicht korrekt installieren. Wechseln Sie zu **Tools > Erweiterungen und Updates...**  , und klicken Sie auf die **installiert** Liste überprüfen, ob die **NuGet-Paket-Manager für Visual Studio** ist mindestens Version 2.8.5. Wenn sie älter ist, klicken Sie auf die **Updates** Liste aus, um die neueste Version herunterzuladen.



Nachdem Sie das NuGet-Paket in Xamarin.Forms 1.3.1 aktualisiert haben, nehmen Sie die folgenden Änderungen in jedem Projekt so aktualisieren Sie auf die neue `Xamarin.Forms.Application` Klasse.

## <a name="22-portable-class-library-or-shared-project"></a>2.2 portable Klassenbibliothek (oder ein freigegebenes Projekt)

Ändern der **App.cs** Datei so, dass:

 - Die `App` Klasse erbt jetzt von `Application`.
 - Die `MainPage` Eigenschaftensatz wird auf der ersten Seite Inhalt angezeigt werden sollen.

```csharp
public class App : Application // superclass new in 1.3
{
    public App ()
    {
        // The root page of your application
        MainPage = new ContentPage {...}; // property new in 1.3
    }
```

Es vollständig entfernt haben die `GetMainPage` -Methode, und legen Sie stattdessen die `MainPage` *Eigenschaft* auf die `Application` Unterklasse.

Diese neue `Application` Basisklasse unterstützt auch die `OnStart`, `OnSleep`, und `OnResume` Außerkraftsetzungen zur einfacheren Verwaltung des Lebenszyklus Ihrer Anwendung.

Die `App` Klasse übergeben, ein neues `LoadApplication` Methode in jeder app-Projekt, wie unten beschrieben:


## <a name="23-ios-app"></a>2.3 iOS-App


Ändern der **AppDelegate.cs** Datei so, dass:

 - Die Klasse erbt von `FormsApplicationDelegate` (anstelle von `UIApplicationDelegate` zuvor).
 - `LoadApplication` wird aufgerufen, durch eine neue Instanz der `App`.


```csharp
[Register ("AppDelegate")]
public partial class AppDelegate :
    global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate // superclass new in 1.3
{
    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.Init ();

        LoadApplication (new App ());  // method is new in 1.3

        return base.FinishedLaunching (app, options);
    }
}
```


## <a name="23-android-app"></a>2.3 android-App

Ändern der **MainActivity.cs** Datei so, dass:

 - Die Klasse erbt von `FormsApplicationActivity` (anstelle von `FormsActivity` zuvor).
 - `LoadApplication` wird aufgerufen, wobei eine neue Instanz der `App`

```csharp
[Activity (Label = "YOURAPPNAM", Icon = "@drawable/icon", MainLauncher = true,
ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
public class MainActivity :
    global::Xamarin.Forms.Platform.Android.FormsApplicationActivity // superclass new in 1.3
{
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        global::Xamarin.Forms.Forms.Init (this, bundle);

        LoadApplication (new App ()); // method is new in 1.3
    }
}
```


## <a name="24-windows-phone-app"></a>2.4 Windows Phone App

Wir aktualisieren müssen die **MainPage** -XAML und das Codebehind.

Ändern der **"MainPage.xaml"** Datei so, dass:

 - Die Verwendung von XAML-Stammelement muss `winPhone:FormsApplicationPage`.
 - Die `xmlns:phone` Attribut muss *geändert* an `xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"`

Ein aktualisiertes Beispiel unten – Sie müssen nur in diese Dinge bearbeiten (der Rest der Attribute muss unverändert bleiben):

```xml
<winPhone:FormsApplicationPage
   ...
   xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"
    ...>
</winPhone:FormsApplicationPage>
```


Ändern der **"MainPage.Xaml.cs"** Datei so, dass:

 - Die Klasse erbt von `FormsApplicationPage` (anstelle von `PhoneApplicationPage` zuvor).
 - `LoadApplication` wird aufgerufen, durch eine neue Instanz von der Xamarin.Forms `App` Klasse. Möglicherweise müssen Sie diesen Verweis vollständig zu qualifizieren, da Windows Phone hat seinem eigenen `App` Klasse bereits definiert.

```csharp
public partial class MainPage : global::Xamarin.Forms.Platform.WinPhone.FormsApplicationPage // superclass new in 1.3
{
    public MainPage()
    {
        InitializeComponent();
        SupportedOrientations = SupportedPageOrientation.PortraitOrLandscape;

        global::Xamarin.Forms.Forms.Init();
        LoadApplication(new YOUR_APP_NAMESPACE.App()); // new in 1.3
    }
 }
```

## <a name="troubleshooting"></a>Problembehandlung

Gelegentlich sehen Sie die Fehler, die etwa wie folgt nach dem Aktualisieren des Xamarin.Forms-NuGet-Pakets. Es tritt auf, wenn es sich bei der NuGet-Updater Verweise auf älteren Versionen von nicht vollständig entfernt die **Csproj** Dateien.

>IHRE\_PROJECT.csproj: Fehler: dieses Projekt verweist auf NuGet-Pakete, die auf diesem Computer nicht vorhanden sind. Aktivieren Sie das NuGet-Paketwiederherstellung herunterladen.  Weitere Informationen finden Sie unter "http://go.microsoft.com/fwlink/?LinkID=322105". Die fehlende Datei ist... /.. /Packages/Xamarin.Forms.1.2.3.6257/Build/Portable-Win+net45+wp80+MonoAndroid10+MonoTouch10/Xamarin.Forms.targets. (IHR\_PROJEKT)

Um diese Fehler zu beheben, öffnen Sie die **Csproj** Datei in einem Text-Editor, und suchen Sie nach `<Target` Elemente, die auf älteren Versionen von Xamarin.Forms, z. B. die unten dargestellte-Element verweisen. Löschen Sie manuell dieses gesamte Element aus der **Csproj** von Dateien und speichern Sie die Änderungen.

```csharp
  <Target Name="EnsureNuGetPackageBuildImports" BeforeTargets="PrepareForBuild">
    <PropertyGroup>
      <ErrorText>This project references NuGet package(s) that are missing on this computer. Enable NuGet Package Restore to download them.  For more information, see http://go.microsoft.com/fwlink/?LinkID=322105. The missing file is {0}.</ErrorText>
    </PropertyGroup>
    <Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets'))" />
  </Target>
```

Das Projekt sollte erfolgreich erstellt, sobald diese alte Verweise entfernt werden.

# <a name="considerations"></a>Weitere Überlegungen

Folgendes sollte beim Konvertieren eines vorhandenen Xamarin.Forms-Projekts von der klassische API an die neue einheitliche API, wenn die app-Komponente oder NuGet-Paket verwendet berücksichtigt werden.

## <a name="components"></a>Komponenten

Jede Komponente, die Sie in Ihrer Anwendung aufgenommen haben, müssen auch die einheitliche API aktualisiert werden, oder Sie erhalten einen Konflikt auf, wenn Sie versuchen, zu kompilieren. Für eine beliebige Komponente enthalten eine neue Version aus dem Speicher der Xamarin-Komponente, die die einheitliche API unterstützt die aktuelle Version ersetzt, und führen Sie einen bereinigten Build aus. Jede Komponente, die noch nicht vom Autor konvertiert wurde, wird eine Warnung nur in der Komponentenspeicher 32-Bit angezeigt.


## <a name="nuget-support"></a>NuGet-Unterstützung

Während es Änderungen zu NuGet zur Bearbeitung der einheitliche API-Unterstützung beigetragen hat, hat nicht stattgefunden eine neue Version von NuGet, damit wir das NuGet erkennt die neuen APIs zu evaluieren.

Bis zu diesem Zeitpunkt wird genau wie die Komponenten müssen Sie wechseln alle NuGet-Paket, das Sie in Ihrem Projekt auf eine Version aufgenommen haben, die Unified-APIs unterstützt, und führen Sie anschließend einen bereinigten Build.

> [!IMPORTANT]
> **Hinweis:** haben einen Fehler in der Form _"Fehler 3 kann nicht im selben Projekt Xamarin.iOS"monotouch.dll"und"Xamarin.iOS.dll"enthalten – explizit"Xamarin.iOS.dll"verwiesen wird, während"monotouch.dll"verwiesen wird" Xxx Version = 0.0.000, Culture = Neutral, PublicKeyToken = Null'"_ nach dem Konvertieren der anwendungskennworts an die Unified-APIs, es liegt in der Regel müssen eine Komponente oder die NuGet-Paket in das Projekt, das nicht auf die einheitliche API aktualisiert wurde. Sie müssen die vorhandene Komponente/NuGet entfernen, update auf eine Version, die Unified-APIs unterstützt, und führen Sie einen bereinigten Build.




# <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Aktivieren die 64-Bit-Builds von Xamarin.iOS-Apps

Für eine Xamarin.iOS mobile Anwendung, die in die einheitliche API konvertiert wurde, muss der Entwickler noch die Erstellung der Anwendung für 64-Bit-Computer aus der app-Optionen zu aktivieren. Finden Sie unter der **aktivieren 64 Bit-erstellt von Xamarin.iOS Apps** von der [32/64-Bit-Plattform Überlegungen](~/cross-platform/macios/32-and-64.md#enable-64) Dokument Weitere ausführliche Anweisungen zur Aktivierung von 64-Bit erstellt.

# <a name="summary"></a>Zusammenfassung

Die Xamarin.Forms-Anwendung sollte jetzt aktualisiert werden, auf Version 1.3.1 und iOS-app auf die einheitliche API (der 64-Bit-Architekturen auf iOS-Plattform unterstützt) migriert.

Wie zuvor erwähnt, wenn Ihre app Xamarin.Forms systemeigenen Code, z. B. benutzerdefinierte Renderer enthält oder Abhängigkeit Dienste, und klicken Sie dann diese auch möglicherweise aktualisiert werden, um die neuen Datentypen verwenden, [eingeführt, die in der API Unified](~/cross-platform/macios/index.md).


## <a name="related-links"></a>Verwandte Links

- [Aktualisieren von iOS-Apps](~/cross-platform/macios/unified/updating-apps.md)
- [Aktualisieren von iOS-Apps](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Arbeiten mit nativen Typen in plattformübergreifenden Apps](~/cross-platform/macios/native-types-cross-platform.md)
- [Aktualisieren von Tipps](~/cross-platform/macios/unified/updating-tips.md)
- [Klassische Vs einheitliche API-Unterschiede](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
