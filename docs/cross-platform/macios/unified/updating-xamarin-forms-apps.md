---
title: Aktualisieren vorhandener xamarin. Forms-apps
description: In diesem Dokument werden die Schritte beschrieben, die zum Aktualisieren einer xamarin. Forms-APP von der Classic API auf die Unified API ausgeführt werden müssen.
ms.prod: xamarin
ms.assetid: C2F0D1D1-256D-44A4-AAC9-B06A0CB41E70
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: dad1b7173e302931455887fdaa4730347f0e5e55
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73014999"
---
# <a name="updating-existing-xamarinforms-apps"></a>Aktualisieren vorhandener xamarin. Forms-apps

_Führen Sie diese Schritte aus, um eine vorhandene xamarin. Forms-APP so zu aktualisieren, dass Sie die Unified API und Update auf Version 1.3.1 verwendet_

> [!IMPORTANT]
> Da xamarin. Forms 1.3.1 das erste Release ist, das die Unified API unterstützt, sollte die gesamte Projekt Mappe so aktualisiert werden, dass Sie die neueste Version zur gleichen Zeit wie die Migration der IOS-APP zu Unified verwendet. Dies bedeutet, dass Sie zusätzlich zum Aktualisieren des IOS-Projekts für Unified Support auch Code in _allen_ Projekten in der Projekt Mappe bearbeiten müssen.

Das Update wird in zwei Schritten ausgeführt:

1. Migrieren Sie die IOS-App zum Unified API mithilfe des Visual Studio für Mac Build in Migration Tool.

    - Verwenden Sie das Migrationstool, um das Projekt automatisch zu aktualisieren.

    - Aktualisieren Sie Native IOS-APIs, wie in den Anweisungen zum [Aktualisieren von IOS-apps](~/cross-platform/macios/unified/updating-ios-apps.md) (insbesondere in benutzerdefiniertem Renderer oder Abhängigkeits Dienst Code) beschrieben.

2. Aktualisieren Sie die gesamte Projekt Mappe in xamarin. Forms Version 1,3.

    1. Installieren Sie das xamarin. Forms 1.3.1-nuget-Paket.

    2. Aktualisieren Sie die `App`-Klasse im freigegebenen Code.

    3. Aktualisieren Sie die `AppDelegate` im IOS-Projekt.

    4. Aktualisieren Sie die `MainActivity` im Android-Projekt.

    5. Aktualisieren Sie die `MainPage` im Windows Phone Projekt.

## <a name="1-ios-app-unified-migration"></a>1. IOS-app (vereinheitlichte Migration)

Ein Teil der Migration erfordert die Aktualisierung von xamarin. Forms auf Version 1,3, die die Unified API unterstützt. Damit die richtigen Assemblyverweise erstellt werden können, müssen wir zuerst das IOS-Projekt aktualisieren, um die Unified API zu verwenden.

### <a name="migration-tool"></a>Migrations Tool

Klicken Sie auf das IOS-Projekt, damit es ausgewählt ist. Wählen Sie dann **Projekt > zu xamarin. IOS Unified API migrieren** aus, und stimmen Sie der angezeigten Warnmeldung zu.

![](updating-xamarin-forms-apps-images/beta-tool1.png "Choose Project > Migrate to Xamarin.iOS Unified API... and agree to the warning message that appears")

Dies erfolgt automatisch:

- Ändern Sie den Projekttyp zur Unterstützung der Unified 64-Bit-API.
- Ändern Sie den frameworkverweis in **xamarin. IOS** (durch Ersetzen der alten **MonoTouch** -Referenz).
- Ändern Sie die Namespace Verweise im Code, um das `MonoTouch` Präfix zu entfernen.
- Aktualisieren Sie die **csproj** -Datei, um die richtigen Buildziele für den Unified API zu verwenden.

**Bereinigen** und **Erstellen** Sie das Projekt, um sicherzustellen, dass keine weiteren Fehler behoben werden können. Es sollte keine weitere Aktion erforderlich sein. Diese Schritte werden in den [Unified API docs](~/cross-platform/macios/unified/updating-ios-apps.md)ausführlicher erläutert.

### <a name="update-native-ios-apis-if-required"></a>Aktualisieren von systemeigenen IOS-APIs (falls erforderlich)

Wenn Sie zusätzlichen systemeigenen IOS-Code (z. b. benutzerdefinierte Renderer oder Abhängigkeits Dienste) hinzugefügt haben, müssen Sie möglicherweise zusätzliche manuelle Code Korrekturen durchführen. Kompilieren Sie Ihre APP erneut, und lesen Sie die [Anweisungen zum Aktualisieren vorhandener IOS-apps](~/cross-platform/macios/unified/updating-ios-apps.md) , um weitere Informationen zu ggf. erforderlichen Änderungen zu erhalten. [Diese Tipps](~/cross-platform/macios/unified/updating-tips.md) helfen Ihnen auch, Änderungen zu identifizieren, die erforderlich sind.

## <a name="2-xamarinforms-131-update"></a>2. xamarin. Forms 1.3.1-Update

Nachdem die IOS-App auf die Unified API aktualisiert wurde, muss der Rest der Lösung auf xamarin. Forms Version 1.3.1 aktualisiert werden. Dies umfasst Folgendes:

- Aktualisieren Sie das xamarin. Forms-nuget-Paket in jedem Projekt.
- Ändern des Codes für die Verwendung der neuen xamarin. Forms-`Application`, `FormsApplicationDelegate` (IOS), `FormsApplicationActivity` (Android) und `FormsApplicationPage` (Windows Phone)-Klassen.

Diese Schritte werden im folgenden erläutert:

### <a name="21-update-nuget-in-all-projects"></a>2,1 Aktualisieren von nuget in allen Projekten

Aktualisieren Sie xamarin. Forms auf die Vorabversion 1.3.1 mit dem nuget-Paket-Manager für alle Projekte in der Projekt Mappe: PCL (sofern vorhanden), Ios, Android und Windows phone. Es wird empfohlen, das xamarin. Forms-nuget-Paket zu **Löschen und erneut hinzuzufügen** , um auf Version 1,3 zu aktualisieren.

> [!NOTE]
> Xamarin. Forms Version 1.3.1 befindet sich derzeit in der *Vorabversion*. Dies bedeutet, dass Sie die **vorab** Version in nuget (über ein Tick-Box-Element in Visual Studio für Mac oder eine Dropdown Liste in Visual Studio) auswählen müssen, um die neueste Vorabversion anzuzeigen.

> [!IMPORTANT]
> Wenn Sie Visual Studio verwenden, stellen Sie sicher, dass die neueste Version des nuget-Paket-Managers installiert ist. Ältere Versionen von nuget in Visual Studio installieren die einheitliche Version von xamarin. Forms 1.3.1 nicht ordnungsgemäß. Wechseln Sie zu Extras **> Erweiterungen und Updates...** , und klicken Sie auf die Liste **installiert** , um zu überprüfen, ob der **nuget-Paket-Manager für Visual Studio** mindestens Version 2.8.5 ist. Wenn Sie älter ist, klicken Sie auf die **Update** Liste, um die neueste Version herunterzuladen.

Nachdem Sie das nuget-Paket in xamarin. Forms 1.3.1 aktualisiert haben, nehmen Sie die folgenden Änderungen in den einzelnen Projekten vor, um ein Upgrade auf die neue `Xamarin.Forms.Application`-Klasse auszuführen.

### <a name="22-portable-class-library-or-shared-project"></a>2,2 Portable Klassenbibliothek (oder frei gegebenes Projekt)

Ändern Sie die Datei **app.cs** wie folgt:

- Die `App`-Klasse erbt nun von `Application`.
- Die `MainPage`-Eigenschaft wird auf die erste Inhaltsseite festgelegt, die Sie anzeigen möchten.

```csharp
public class App : Application // superclass new in 1.3
{
    public App ()
    {
        // The root page of your application
        MainPage = new ContentPage {...}; // property new in 1.3
    }
```

Wir haben die `GetMainPage`-Methode vollständig entfernt und stattdessen die `MainPage`- *Eigenschaft* für die `Application`-Unterklasse festgelegt.

Diese neue `Application` Basisklasse unterstützt auch die über schreibungen `OnStart`, `OnSleep`und `OnResume`, um Ihnen die Verwaltung des Lebenszyklus Ihrer Anwendung zu erleichtern.

Die `App`-Klasse wird dann in jedem App-Projekt an eine neue `LoadApplication` Methode weitergegeben, wie unten beschrieben:

### <a name="23-ios-app"></a>2,3 IOS-App

Ändern Sie die Datei **AppDelegate.cs** wie folgt:

- Die Klasse erbt von `FormsApplicationDelegate` (statt zuvor `UIApplicationDelegate`).
- `LoadApplication` wird mit einer neuen Instanz von `App`aufgerufen.

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

### <a name="23-android-app"></a>2,3 Android-App

Ändern Sie die Datei **MainActivity.cs** wie folgt:

- Die Klasse erbt von `FormsApplicationActivity` (statt zuvor `FormsActivity`).
- `LoadApplication` wird mit einer neuen Instanz von aufgerufen `App`

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

### <a name="24-windows-phone-app"></a>2,4 Windows Phone-App

Wir müssen die **MainPage** aktualisieren, sowohl das XAML als auch den Code Behind.

Ändern Sie die Datei " **MainPage. XAML** " wie folgt:

- Das XAML-Stamm Element sollte `winPhone:FormsApplicationPage`werden.
- Das `xmlns:phone`-Attribut sollte in *geändert* werden `xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"`

Unten ist ein aktualisiertes Beispiel dargestellt. Sie sollten diese Schritte nur bearbeiten müssen (die restlichen Attribute sollten unverändert bleiben):

```xml
<winPhone:FormsApplicationPage
   ...
   xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"
    ...>
</winPhone:FormsApplicationPage>
```

Ändern Sie die Datei **MainPage.XAML.cs** wie folgt:

- Die Klasse erbt von `FormsApplicationPage` (statt zuvor `PhoneApplicationPage`).
- `LoadApplication` wird mit einer neuen Instanz der `App` Klasse xamarin. Forms aufgerufen. Möglicherweise müssen Sie diesen Verweis vollständig qualifizieren, da Windows Phone seine eigene `App` Klasse bereits definiert hat.

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

### <a name="troubleshooting"></a>Problembehandlung

Gelegentlich wird ein Fehler angezeigt, der diesem ähnelt, nachdem das xamarin. Forms-nuget-Paket aktualisiert wurde. Dies tritt auf, wenn der nuget-Updater Verweise auf ältere Versionen nicht vollständig aus Ihren **csproj** -Dateien entfernt.

>Ihr\_Project. csproj: Fehler: dieses Projekt verweist auf nuget-Pakete, die auf diesem Computer fehlen. Aktivieren Sie die nuget-Paket Wiederherstellung zum herunterladen.  Weitere Informationen finden Sie unter http://go.microsoft.com/fwlink/?LinkID=322105. Die fehlende Datei ist.. /.. /Packages/Xamarin.Forms.1.2.3.6257/Build/Portable-Win + net45 + WP80 + MonoAndroid10 + MonoTouch10/xamarin. Forms. targets. (Ihr\_Projekt)

Um diese Fehler zu beheben, öffnen Sie die **csproj** -Datei in einem Text-Editor, und suchen Sie nach `<Target` Elementen, die auf ältere Versionen von xamarin. Forms verweisen, z. b. auf das unten gezeigte-Element. Löschen Sie das gesamte Element manuell aus der **csproj** -Datei, und speichern Sie die Änderungen.

```csharp
  <Target Name="EnsureNuGetPackageBuildImports" BeforeTargets="PrepareForBuild">
    <PropertyGroup>
      <ErrorText>This project references NuGet package(s) that are missing on this computer. Enable NuGet Package Restore to download them.  For more information, see http://go.microsoft.com/fwlink/?LinkID=322105. The missing file is {0}.</ErrorText>
    </PropertyGroup>
    <Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets'))" />
  </Target>
```

Das Projekt sollte erfolgreich erstellt werden, sobald diese alten Verweise entfernt wurden.

## <a name="considerations"></a>Weitere Überlegungen

Beachten Sie die folgenden Überlegungen, wenn Sie ein vorhandenes xamarin. Forms-Projekt vom Classic API in den neuen Unified API umstellen, wenn diese APP von mindestens einer Komponente oder einem nuget-Paket abhängig ist.

### <a name="components"></a>Komponenten

Jede Komponente, die in der Anwendung enthalten ist, muss ebenfalls auf den Unified API aktualisiert werden, oder es tritt ein Konflikt auf, wenn Sie versuchen, eine Kompilierung auszuführen. Ersetzen Sie für jede enthaltene Komponente die aktuelle Version durch eine neue Version aus dem xamarin-Komponenten Speicher, der die Unified API unterstützt, und führen Sie einen sauberen Build aus. Jede Komponente, die noch nicht vom Autor konvertiert wurde, zeigt im Komponenten Speicher eine Warnung mit nur 32 Bit an.

### <a name="nuget-support"></a>NuGet-Unterstützung

Obwohl wir Änderungen an nuget beigetragen haben, um mit der Unified API-Unterstützung zu arbeiten, gibt es noch keine neue Version von nuget. daher evaluieren wir, wie Sie nuget zum Erkennen der neuen APIs erhalten.

Bis zu diesem Zeitpunkt müssen Sie, genau wie die Komponenten, ein beliebiges nuget-Paket, das Sie in Ihrem Projekt enthalten haben, in eine Version wechseln, die die vereinheitlichten APIs unterstützt, und anschließend einen sauberen Build ausführen.

> [!IMPORTANT]
> Wenn ein Fehler in der Form _"Fehler 3 kann nicht sowohl ' MonoTouch. dll ' als auch ' xamarin. IOS. dll ' im gleichen xamarin. IOS-Projekt enthalten ist, wird explizit auf '" "" "" "" "" "" ". neutral, PublicKeyToken = null ' "_ nach der Umstellung der Anwendung in die vereinheitlichten APIs liegt dies in der Regel daran, dass eine Komponente oder ein nuget-Paket im Projekt vorhanden ist, das nicht auf den Unified API aktualisiert wurde. Sie müssen die vorhandene Komponente bzw. nuget entfernen, ein Update auf eine Version durchführen, die die vereinheitlichten APIs unterstützt, und einen sauberen Build durchführen.

## <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Aktivieren von 64-Bit-Builds von xamarin. IOS-apps

Für eine Mobile xamarin. IOS-Anwendung, die in die Unified API konvertiert wurde, muss der Entwickler weiterhin das Entwickeln der Anwendung für 64-Bit-Computer aus den Optionen der APP aktivieren. Ausführliche Anweisungen zum Aktivieren von 64-Bit-Builds finden Sie im Dokument " **Aktivieren von 64-Bit-Builds von xamarin. IOS-apps** " des Dokuments mit der [32/64-Bit-Plattform](~/cross-platform/macios/32-and-64/index.md#enable-64)

## <a name="summary"></a>Zusammenfassung

Die xamarin. Forms-Anwendung sollte jetzt auf Version 1.3.1 aktualisiert werden, und die IOS-App wurde zu dem Unified API migriert (der 64-Bit-Architekturen auf der IOS-Plattform unterstützt).

Wenn Ihre xamarin. Forms-App systemeigenen Code wie z. b. benutzerdefinierte Renderer oder Abhängigkeits Dienste enthält, müssen diese möglicherweise auch aktualisiert werden, um die neuen Typen zu verwenden, die [in der Unified API eingeführt wurden](~/cross-platform/macios/index.md).

## <a name="related-links"></a>Verwandte Links

- [Aktualisieren von IOS-apps](~/cross-platform/macios/unified/updating-apps.md)
- [Aktualisieren von IOS-apps](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Arbeiten mit nativen Typen in plattformübergreifenden Apps](~/cross-platform/macios/native-types-cross-platform.md)
- [Tipps werden aktualisiert](~/cross-platform/macios/unified/updating-tips.md)
- [Unterschiede bei klassischem vs Unified API](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
