---
title: Aktualisieren von vorhandenen Xamarin.Forms-Apps
description: Dieses Dokument beschreibt die Schritte, die beachtet werden müssen, um eine Xamarin.Forms-app von der klassischen API, die Unified API zu aktualisieren.
ms.prod: xamarin
ms.assetid: C2F0D1D1-256D-44A4-AAC9-B06A0CB41E70
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: d5c16b034b07d3e9875412f041c16b293557438a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61211855"
---
# <a name="updating-existing-xamarinforms-apps"></a>Aktualisieren von vorhandenen Xamarin.Forms-Apps

_Führen Sie diese Schritte zum Aktualisieren einer vorhandenen Xamarin.Forms-app verwenden Sie die Unified API und ein update auf Version 1.3.1_

> [!IMPORTANT]
> Da Xamarin.Forms 1.3.1 die erste Version, die die Unified API unterstützt ist, sollte die gesamte Lösung aktualisiert werden, um die neueste Version zur gleichen Zeit wie die Migration von der iOS-app zu Unified verwenden. Dies bedeutet, dass zusätzlich zum Aktualisieren des iOS-Projekts für Unified-Support, außerdem Sie Code in bearbeiten müssen _alle_ die Projekte in der Projektmappe.

Die Aktualisierung erfolgt in zwei Schritten:

1. Migrieren Sie die iOS-app an die Unified API mit Visual Studio für Mac Build-Migrationstools kennen.

    - Verwenden Sie das Migrationstool, um das Projekt automatisch aktualisiert.

    - Update iOS Native APIs wie in den Anweisungen, um [Aktualisieren von iOS-apps](~/cross-platform/macios/unified/updating-ios-apps.md) (insbesondere in benutzerdefinierten Renderer oder eine Abhängigkeit Dienstcode).

2. Aktualisieren Sie die gesamte Lösung in Xamarin.Forms-Version 1.3.

    1. Installieren Sie das Xamarin.Forms 1.3.1-NuGet-Paket.

    2. Update der `App` Klasse in den freigegebenen Code.

    3. Update der `AppDelegate` im iOS-Projekt.

    4. Update der `MainActivity` im Android-Projekt.

    5. Update der `MainPage` im Windows Phone-Projekt.

## <a name="1-ios-app-unified-migration"></a>1. iOS-App (Unified-Migration)

Bei der Migration erfordert das Aktualisieren von Xamarin.Forms auf Version 1.3, die die Unified API unterstützt. In der Reihenfolge für die richtige Assembly-Verweise erstellt werden soll müssen wir zuerst das iOS-Projekt zum Verwenden der Unified API aktualisieren.

### <a name="migration-tool"></a>Migrationstool

Klicken Sie auf das iOS-Projekt, damit es aktiviert ist, wählen Sie dann **Projekt > migrieren zur vereinheitlichten Xamarin.iOS-API...**  und akzeptieren Sie die Warnmeldung, die angezeigt wird.

![](updating-xamarin-forms-apps-images/beta-tool1.png "Projekt auswählen > zu vereinheitlichten Xamarin.iOS-API migrieren... und akzeptieren Sie die Warnmeldung, die angezeigt wird")

Damit wird automatisch:

 - Ändern Sie den Projekttyp aus, um die einheitliche 64-Bit-API zu unterstützen.
 - Ändern Sie den frameworkverweis um **Xamarin.iOS** (die alte **Monotouch** Verweis).
 - Ändern Sie die Namespaceverweise in den Code zum Entfernen der `MonoTouch` Präfix.
 - Update der **Csproj** Datei, die die richtigen Build-Ziele für die Unified API verwendet.

**Bereinigen** und **erstellen** das Projekt, um sicherzustellen, dass keine weiteren Fehler zu beheben. Keine weiteren Aktionen sollten erforderlich sein. Diese Schritte werden ausführlicher in den [Unified API-Dokumentation](~/cross-platform/macios/unified/updating-ios-apps.md).

### <a name="update-native-ios-apis-if-required"></a>Aktualisieren Sie native iOS-APIs (falls erforderlich)

Wenn Sie zusätzliche iOS systemeigenem Code (z. B. benutzerdefinierte Renderer "oder" Abhängigkeitsdienste ") hinzugefügt haben, müssen Sie für zusätzliche manuelle Codekorrekturen durchführen. Kompilieren Sie Ihre app erneut, und beziehen sich auf die [Aktualisieren von vorhandenen iOS-Apps-Anweisungen](~/cross-platform/macios/unified/updating-ios-apps.md) Weitere Informationen zu Änderungen, die möglicherweise erforderlich sind. [Diese Tipps](~/cross-platform/macios/unified/updating-tips.md) hilft auch dabei Änderungen zu identifizieren, die erforderlich sind.

## <a name="2-xamarinforms-131-update"></a>2. Xamarin.Forms 1.3.1-Update

Nach die iOS-app der Unified API aktualisiert wurde, muss der Rest der Lösung auf Xamarin.Forms-Version 1.3.1 aktualisiert werden. Dies umfasst Folgendes:

 - Aktualisieren das Xamarin.Forms-NuGet-Paket in jedem Projekt.
 - Ändern den Code, um die neue Xamarin.Forms verwendet `Application`, `FormsApplicationDelegate` (iOS) `FormsApplicationActivity` (Android), und `FormsApplicationPage` (Windows Phone)-Klassen.

Diese Schritte werden im folgenden erläutert:

### <a name="21-update-nuget-in-all-projects"></a>2.1 Aktualisieren von NuGet in allen Projekten

Aktualisieren Sie Xamarin.Forms, um 1.3.1 Vorabversionen mithilfe des NuGet-Paket-Managers für alle Projekte in der Projektmappe: PCL (falls vorhanden), iOS, Android und Windows Phone. Es wird empfohlen, die Sie **löschen und erneut hinzufügen** das Xamarin.Forms-NuGet-Paket zum Aktualisieren auf Version 1.3.

**HINWEIS:** Xamarin.Forms-Version 1.3.1 befindet sich derzeit in *Vorabversion*. Dies bedeutet, Sie müssen auswählen, die **Vorabversion** option NuGet über (ein Tick-Feld in Visual Studio für Mac) oder eine Drop-Down-Liste in Visual Studio auf die neueste Vorabversion finden Sie unter.

> [!IMPORTANT]
> Wenn Sie Visual Studio verwenden, stellen Sie sicher, dass die neueste Version des NuGet Paket-Managers installiert ist. Ältere Versionen von NuGet in Visual Studio werden die einheitliche-Version von Xamarin.Forms 1.3.1 nicht ordnungsgemäß installiert. Wechseln Sie zu **Tools > Erweiterungen und Updates...**  und klicken Sie auf die **installiert** Liste überprüfen, ob die **NuGet-Paket-Manager für Visual Studio** ist mindestens Version 2.8.5. Wenn sie älter ist, klicken Sie auf die **Updates** Liste aus, um die neueste Version herunterladen.

Nachdem Sie das NuGet-Paket in Xamarin.Forms 1.3.1 aktualisiert haben, nehmen Sie die folgenden Änderungen in jedes Projekt, um das upgrade auf die neue `Xamarin.Forms.Application` Klasse.

### <a name="22-portable-class-library-or-shared-project"></a>2.2 portable Klassenbibliothek (oder ein freigegebenes Projekt)

Ändern der **"App.cs"** Datei, damit:

 - Die `App` Klasse erbt nun von `Application`.
 - Die `MainPage` -Eigenschaftensatz auf der ersten Seite Inhalt Sie anzeigen möchten.

```csharp
public class App : Application // superclass new in 1.3
{
    public App ()
    {
        // The root page of your application
        MainPage = new ContentPage {...}; // property new in 1.3
    }
```

Wurde vollständig entfernt die `GetMainPage` -Methode, und legen Sie stattdessen die `MainPage` *Eigenschaft* auf die `Application` Unterklasse.

Diese neue `Application` Basisklasse unterstützt auch die `OnStart`, `OnSleep`, und `OnResume` , überschreibungen, um den Lebenszyklus einer Anwendung verwalten.

Die `App` Klasse wird dann an einen neuen übergeben `LoadApplication` -Methode in jedem app-Projekt, wie im folgenden beschrieben:

### <a name="23-ios-app"></a>2.3-iOS-App

Ändern der **Datei "appdelegate.cs"** Datei, damit:

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

### <a name="23-android-app"></a>2.3 android-App

Ändern der **"mainactivity.cs"** Datei, damit:

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

### <a name="24-windows-phone-app"></a>2.4 Windows Phone-App

Wir müssen Aktualisieren der **"MainPage"** – sowohl die XAML und Codebehind.

Ändern der **"MainPage.xaml"** Datei, damit:

 - Das XAML-Stammelement muss `winPhone:FormsApplicationPage`.
 - Die `xmlns:phone` Attribut sollte sein, *geändert* auf `xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"`

Ein aktualisiertes Beispiel wird unten angezeigt – Sie sollten nur verwenden, um Folgendes zu bearbeiten (die restlichen Attribute sollte identisch sein):

```xml
<winPhone:FormsApplicationPage
   ...
   xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"
    ...>
</winPhone:FormsApplicationPage>
```

Ändern der **"MainPage.Xaml.cs"** Datei, damit:

 - Die Klasse erbt von `FormsApplicationPage` (anstelle von `PhoneApplicationPage` zuvor).
 - `LoadApplication` wird aufgerufen, durch eine neue Instanz von der Xamarin.Forms `App` Klasse. Möglicherweise müssen Sie diesen Verweis vollständig zu qualifizieren, da Windows Phone hat eigene `App` Klasse bereits definiert.

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

Gelegentlich sehen Sie einen Fehler, die etwa wie folgt nach dem Aktualisieren des Xamarin.Forms-NuGet-Pakets. Es tritt auf, wenn der NuGet-Updater Verweise auf ältere Versionen von nicht vollständig entfernt Ihre **Csproj** Dateien.

>YOUR\_PROJECT.csproj: Fehler: Dieses Projekt verweist auf NuGet-Pakete, die auf diesem Computer fehlen. Aktivieren Sie das NuGet-Paketwiederherstellung, sie herunterzuladen.  Weitere Informationen finden Sie unter http://go.microsoft.com/fwlink/?LinkID=322105. Die fehlende Datei ist... /.. /Packages/Xamarin.Forms.1.2.3.6257/Build/Portable-Win+net45+wp80+MonoAndroid10+MonoTouch10/Xamarin.Forms.targets. (IHRE\_PROJEKT)

Um diese Fehler zu beheben, öffnen Sie die **Csproj** -Datei in einem Texteditor, und suchen Sie nach `<Target` Elemente, die auf älteren Versionen von Xamarin.Forms, z. B. die unten gezeigten Element verweisen. Sie sollten diese gesamte Element aus manuell löschen der **Csproj** Datei, und speichern Sie die Änderungen.

```csharp
  <Target Name="EnsureNuGetPackageBuildImports" BeforeTargets="PrepareForBuild">
    <PropertyGroup>
      <ErrorText>This project references NuGet package(s) that are missing on this computer. Enable NuGet Package Restore to download them.  For more information, see http://go.microsoft.com/fwlink/?LinkID=322105. The missing file is {0}.</ErrorText>
    </PropertyGroup>
    <Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets'))" />
  </Target>
```

Das Projekt sollte erfolgreich erstellt, nachdem diese alten Verweise entfernt wurden.

## <a name="considerations"></a>Weitere Überlegungen

Die folgenden Aspekte sollten bei der Konvertierung eines vorhandenen Xamarin.Forms-Projekts von der klassischen API auf die neue einheitliche API, wenn diese app mindestens eine Komponente oder NuGet-Paket benötigt berücksichtigt werden.

### <a name="components"></a>Komponenten

Jede Komponente, die Sie in Ihrer Anwendung aufgenommen haben, müssen auch die Unified API aktualisiert werden, oder Sie erhalten einen Konflikt auf, wenn Sie versuchen, zu kompilieren. Ersetzen Sie die aktuelle Version mit einer neuen Version von der Xamarin Component Store, die die Unified API unterstützt, und führen Sie einen bereinigten Build, für eine beliebige Komponente enthalten. Jede Komponente, die noch nicht vom Autor, konvertiert wurde, wird eine 32-Bit nur im Komponentenspeicher Warnung angezeigt.

### <a name="nuget-support"></a>NuGet-Unterstützung

Während wir Änderungen an NuGet mit der Unified API-Unterstützung funktioniert hat, wurde keine neue Version von NuGet, damit wir zum Abrufen von NuGet, um die neuen APIs erkennt auswerten.

Bis zu diesem Zeitpunkt an, wie die Komponenten müssen Sie zum Wechseln von alle NuGet-Paket, das Sie in Ihrem Projekt auf eine Version aufgenommen haben, die Unified-APIs unterstützt, und führen anschließend einen bereinigten Build.

> [!IMPORTANT]
> Wenn Sie einen Fehler in der Form haben _"Fehler 3 kann nicht sowohl"monotouch.dll"und"Xamarin.iOS.dll"im gleichen Xamarin.iOS-Projekt enthalten – explizit 'Xamarin.iOS.dll' verwiesen wird, solange"monotouch.dll"verwiesen wird" Xxx "," Version = 0.0.000, Kultur = Neutral, PublicKeyToken = Null'"_ nach der Konvertierung Ihrer Anwendung auf die Unified-APIs, es liegt in der Regel müssen entweder eine Komponente oder ein NuGet-Paket im Projekt, das nicht der Unified API aktualisiert wurde. Sie müssen zum Entfernen des vorhandenen Komponente/NuGet-Pakets, ein update auf eine Version, die Unified-APIs unterstützt, und führen Sie einen bereinigten Build.

## <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Aktivieren die 64-Bit-Builds von Xamarin.iOS-Apps

Für eine mobile Xamarin.iOS-Anwendung, die in der Unified API konvertiert wurde, muss der Entwickler weiterhin das Erstellen der Anwendung für 64-Bit-Computer aus der app-Optionen zu aktivieren. Finden Sie unter den **Aktivieren von 64 Bit Builds von Xamarin.iOS-Apps** von der [32/64 Bit Platform Considerations](~/cross-platform/macios/32-and-64/index.md#enable-64) Dokument für detaillierte Anweisungen zum Aktivieren von 64-Bit-builds.

## <a name="summary"></a>Zusammenfassung

Die Xamarin.Forms-Anwendung sollte jetzt auf Version 1.3.1 aktualisiert werden, und die iOS-app migriert wird, der Unified API (die 64-Bit-Architekturen für die iOS-Plattform unterstützt).

Wie zuvor erwähnt werden, wenn Dependency services, und klicken Sie dann diese auch möglicherweise aktualisiert, um die neuen Datentypen verwenden oder Ihre Xamarin.Forms-app systemeigenen Code wie benutzerdefinierten Renderern enthält [eingeführt, die in der Unified API](~/cross-platform/macios/index.md).

## <a name="related-links"></a>Verwandte Links

- [Aktualisieren von iOS-Apps](~/cross-platform/macios/unified/updating-apps.md)
- [Aktualisieren von iOS-Apps](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Arbeiten mit nativen Typen in plattformübergreifenden Apps](~/cross-platform/macios/native-types-cross-platform.md)
- [Aktualisieren von Tipps](~/cross-platform/macios/unified/updating-tips.md)
- [Klassischen Vs Unified API-Unterschiede](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
