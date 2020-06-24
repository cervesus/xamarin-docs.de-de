---
title: Zusammenfassung von Kapitel 28. Standort und Karten
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung von Kapitel 28. Standort und Karten'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F6E20077-687C-45C4-A375-31D4F49BBFA4
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 301dc65c7909603e117717a993959e3c73fa2d32
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84133405"
---
# <a name="summary-of-chapter-28-location-and-maps"></a>Zusammenfassung von Kapitel 28. Standort und Karten

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)

> [!NOTE]
> In den Anmerkungen auf dieser Seite wird beschrieben, inwiefern die Angaben innerhalb des Buchs heute nicht mehr für Xamarin.Forms gelten.

Xamarin.Forms unterstützt ein [`Map`](xref:Xamarin.Forms.Maps.Map)-Element, das von `View` abgeleitet wird. Aufgrund der speziellen Plattformanforderungen, die bei der Verwendung von Karten zum Tragen kommen, sind sie in einer gesonderten Assembly ( **Xamarin.Forms.Maps**) und verwenden einen anderen Namespace: `Xamarin.Forms.Maps`.

## <a name="the-geographic-coordinate-system"></a>Das geografische Koordinatensystem

Ein geografisches Koordinatensystem identifiziert Standorte auf einem kugelförmigen (oder fast kugelförmigen) Objekt wie der Erde. Eine Koordinate besteht aus einem *Breitengrad* und einem *Längengrad*, ausgedrückt in Winkeln.

Ein großer Kreis, bezeichnet als `equator`, liegt in der Mitte zwischen den beiden Polen, durch die die Achse der Erde konzeptionell verläuft.

### <a name="parallels-and-latitude"></a>Breitenkreise und Längengrad

Ein nördlich oder südlich des Äquators von der Mitte der Erde aus gemessener Winkel markiert Linien gleicher Breite, auch bekannt als *Breitenkreise* (Parallelen). Diese reichen von 0 Grad am Äquator bis hin zu 90 Grad am Nord- und Südpol. Per Konvention besitzen Breitengrade nördlich des Äquators positive Werte, Breitengrade südlich des Äquators haben negative Werte.

### <a name="longitude-and-meridians"></a>Längengrad und Meridiane

Die Hälften großer Kreise vom Nord- zum Südpol sind Linien gleicher Länge, auch bekannt als *Meridiane*. Diese sind relativ zum Nullmeridian in Greenwich, England. Per Konvention besitzen Längengrade östlich des Nullmeridians positive Werte von 0 Grad bis 180 Grad, und Längengrade westlich des Nullmeridians haben negative Werte von 0 Grad bis &ndash;180 Grad.

### <a name="the-equirectangular-projection"></a>Die Equirektangularprojektion

Jede Plattkarte der Erde führt Verzerrungen ein. Wenn alle Breiten- und Längengradlinien gerade sind, und wenn gleiche Abstände bei Breitengrad- und Längengradwinkeln gleichen Abständen auf der Karte entsprechen, ist das Ergebnis eine *equirektangulare Projektion*. Bei dieser Karte werden Gebiete näher an den Polen verzerrt, weil sie horizontal gestreckt werden.

### <a name="the-mercator-projection"></a>Die Mercator-Projektion

Die beliebte *Mercator-Projektion* versucht, die horizontale Streckung zu kompensieren, indem diese Gebiete ebenfalls vertikal gestreckt werden. Dies führt zu einer Karte, bei der Gebiete in der Nähe der Pole wesentlich größer erscheinen, als sie tatsächlich sind, aber jedes lokale Gebiet entspricht ziemlich genau der tatsächlichen Fläche.

### <a name="map-services-and-tiles"></a>Kartendienste und -kacheln

Kartendienste verwenden eine Variation der Mercator-Projektion namens `Web Mercator`. Die Kartendienste liefern Bitmapkacheln an einen Client, basierend auf Standort und Zoomfaktor.

## <a name="getting-the-users-location"></a>Abrufen des Standorts des Benutzers

Die Xamarin.Forms-Klassen von `Map` umfassen keine Möglichkeit zum Abrufen des geografischen Standorts des Benutzers, was aber häufig wünschenswert ist, wenn mit Karten gearbeitet wird, sodass dies von einem Abhängigkeitsdienst erledigt werden muss.

> [!NOTE]
> Xamarin.Forms-Anwendungen können stattdessen die in Xamarin.Essentials enthaltene [`Geolocation`](~/essentials/geolocation.md)-Klasse verwenden.

### <a name="the-location-tracker-api"></a>Die Standorttracker-API

Die [**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform)-Projektmappe enthält Code für eine Standorttracker-API. Die [`GeographicLocation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/GeographicLocation.cs)-Struktur kapselt einen Breiten- und Längengrad. Die [`ILocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/ILocationTracker.cs)-Schnittstelle definiert zwei Methoden zum Starten und Anhalten des Standorttrackers sowie ein Ereignis, wenn ein neuer Standort verfügbar ist.

#### <a name="the-ios-location-manager"></a>Der Standort-Manager von iOS

Die iOS-Implementierung von `ILocationTracker` ist eine [`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/LocationTracker.cs)-Klasse, die den [`CLLocationManager`](xref:CoreLocation.CLLocationManager) von iOS verwendet.

#### <a name="the-android-location-manager"></a>Der Standort-Manager von Android

Die Android-Implementierung von `ILocationTracker` ist eine [`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/LocationTracker.cs)-Klasse, die den [`LocationManager`](xref:Android.Locations.LocationManager) von Android verwendet.

#### <a name="the-uwp-geo-locator"></a>Der Geo-Locator von UWP

Die universelle Windows-Plattformimplementierung von `ILocationTracker` ist eine [`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/LocationTracker.cs)-Klasse, die den [`Geolocator`](/uwp/api/Windows.Devices.Geolocation.Geolocator) der UWP verwendet.

### <a name="display-the-phones-location"></a>Anzeigen des Standorts eines Telefons

Im [**WhereAmI**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/WhereAmI)-Beispiel wird der Standorttracker verwendet, um den Standort des Telefons anzuzeigen, sowohl als Text als auch auf einer equirektangularen Karte.

### <a name="the-required-overhead"></a>Der erforderliche Mehraufwand

Damit **WhereAmI** den Standorttracker verwenden kann, ist ein gewisser Mehraufwand erforderlich. Zunächst müssen alle Projekte in der **WhereAmI**-Projektmappe Verweise auf die entsprechenden Projekte in **Xamarin.FormsBook.Platform** enthalten, und jedes **WhereAmI**-Projekt muss die `Toolkit.Init`-Methode aufrufen.

Ein gewisser plattformspezifischer Mehraufwand in Form von Standortberechtigungen ist erforderlich.

#### <a name="location-permission-for-ios"></a>Standortberechtigungen für iOS

Bei iOS muss die Datei **info.plist** Elemente einschließen, die den Text einer Frage enthalten, mit der der Benutzer aufgefordert wird, das Abrufen des Benutzerstandorts zuzulassen.

#### <a name="location-permissions-for-android"></a>Standortberechtigungen für Android

Android-Anwendungen, die den Standort des Benutzers abrufen, müssen über eine ACCESS_FILE_LOCATION-Berechtigung in der Datei „AndroidManifest.xml“ verfügen.

#### <a name="location-permissions-for-the-uwp"></a>Standortberechtigungen für UWP

Eine universelle Windows-Plattformanwendung muss über eine `location`-Gerätefunktion verfügen, die in der Datei „Package.appxmanifest“ gekennzeichnet ist.

## <a name="working-with-xamarinformsmaps"></a>Arbeiten mit Xamarin.Forms.Maps

Bei der Verwendung der `Map`-Klasse gibt es mehrere Anforderungen.

### <a name="the-nuget-package"></a>Das NuGet-Paket

Die NuGet-Bibliothek **Xamarin.Forms.Maps** muss der Anwendungsprojektmappe hinzugefügt werden. Die Versionsnummer sollte mit der des derzeit installierten **Xamarin.Forms** -Pakets identisch sein.

### <a name="initializing-the-maps-package"></a>Initialisieren des Maps-Pakets

Die Anwendungsprojekte müssen die `Xamarin.FormsMaps.Init`-Methode ausrufen, nachdem `Xamarin.Forms.Forms.Init` aufgerufen wurde.

### <a name="enabling-map-services"></a>Aktivieren von Kartendiensten

Da die `Map` den Standort des Benutzers ermitteln kann, muss die Anwendung die Berechtigung für den Benutzer in der weiter oben in diesem Kapitel beschriebenen Art abrufen:

#### <a name="enabling-ios-maps"></a>Aktivieren von iOS-Karten

Eine iOS-Anwendung, die `Map` verwendet, benötigt zwei Zeilen in der Datei „info.plist“.

#### <a name="enabling-android-maps"></a>Aktivieren von Android-Karten

Für die Verwendung der Google Maps-Dienste ist ein Autorisierungsschlüssel erforderlich. Dieser Schlüssel wird in die Datei **AndroidManifest.xml** eingefügt. Zusätzlich erfordert die Datei **AndroidManifest.xml** `manifest`-Tags, die am Ermitteln des Standorts des Benutzers beteiligt sind.

#### <a name="enabling-uwp-maps"></a>Aktivieren von UWP-Karten

Eine universelle Windows-Plattformanwendung benötigt für die Verwendung von Bing Maps einen Autorisierungsschlüssel. Dieser Schlüssel wird als Argument an die `Xamarin.FormsMaps.Init`-Methode übergeben. Die Anwendung muss auch für Ortungsdienste aktiviert werden.

### <a name="the-unadorned-map"></a>Die einfache Karte

Das [**MapDemos**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/MapDemos)-Beispiel besteht aus einer Datei [MapsDemoHomePage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml) und einer CodeBehind-Datei [MapsDemoHomePage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml.cs), die das Navigieren zu verschiedenen Demonstrationsprogrammen ermöglicht.

In der Datei [BasicMapPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/BasicMapPage.xaml) wird gezeigt, wie Sie die [`Map`](xref:Xamarin.Forms.Maps.Map)-Ansicht anzeigen. Standardmäßig wird in dieser die Stadt Rom angezeigt, aber die Karte kann vom Benutzer geändert werden.

Um horizontales und vertikales Scrollen zu deaktivieren, legen Sie die [`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled)-Eigenschaft auf `false` fest. Um Zoomen zu deaktivieren, legen Sie [`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled) auf `false` fest. Diese Eigenschaften funktionieren vielleicht nicht auf allen Plattformen.

### <a name="streets-and-terrain"></a>Straßen und Gelände

Sie können verschiedene Typen von Karten anzeigen, indem Sie die `Map`-Eigenschaft [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) vom Typ [`MapType`](xref:Xamarin.Forms.Maps.MapType) festlegen, eine Enumeration mit drei Membern:

- [`Street`](xref:Xamarin.Forms.Maps.MapType.Street), der Standardwert.
- [`Satellite`](xref:Xamarin.Forms.Maps.MapType.Satellite)
- [`Hybrid`](xref:Xamarin.Forms.Maps.MapType.Hybrid)

In der Datei [MapTypesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypesPage.xaml) wird gezeigt, wie Sie ein Optionsfeld verwenden, um den Kartentyp auszuwählen. Dabei wird die [`RadioButtonManager`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioButtonManager.cs)-Klasse aus der [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)-Bibliothek sowie eine Klasse, basierend auf der Datei [MapTypeRadioButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypeRadioButton.xaml), verwendet.

### <a name="map-coordinates"></a>Kartenkoordinaten

Ein Programm kann das aktuelle Gebiet, das von der `Map` angezeigt wird, mithilfe der [`VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion)-Eigenschaft anzeigen. Diese Eigenschaft wird *nicht* von einer bindbaren Eigenschaft unterstützt, und es gibt keinen Benachrichtigungsmechanismus, um anzugeben, wenn sie sich geändert hat, sodass ein Programm, das die Eigenschaft überwachen möchte, wahrscheinlich zu diesem Zweck einen Timer verwenden sollte.

`VisibleRegion` ist vom Typ [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan), einer Klasse mit vier schreibgeschützten Eigenschaften:

- [`Center`](xref:Xamarin.Forms.Maps.MapSpan.Center) vom Typ [`Position`](xref:Xamarin.Forms.Maps.Position)
- [`LatitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LatitudeDegrees) vom Typ `double`, was die Höhe des auf der Karte angezeigten Gebiets angibt.
- [`LongitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LongitudeDegrees) vom Typ `double`, was die Breite des auf der Karte angezeigten Gebiets angibt.
- [`Radius`](xref:Xamarin.Forms.Maps.MapSpan.Radius) vom Typ [`Distance`](xref:Xamarin.Forms.Maps.Distance), was die Größe des größten kreisförmigen Gebiets angibt, das auf der Karte sichtbar ist.

`Position` und `Distance` sind beides Strukturen. `Position` definiert über den [`Position`-Konstruktor](xref:Xamarin.Forms.Maps.Position.%23ctor(System.Double,System.Double)) zwei schreibgeschützte Eigenschaften:

- [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude)
- [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude)

`Distance` soll eine einheitenunabhängige Entfernung bereitstellen, indem zwischen metrischen und englischen Einheiten konvertiert wird. Ein `Distance`-Wert kann auf verschiedene Weise erstellt werden:

- [`Distance`-Konstruktor](xref:Xamarin.Forms.Maps.Distance.%23ctor(System.Double)) mit einer Entfernung in Metern.
- Statische Methode [`Distance.FromMeters`](xref:Xamarin.Forms.Maps.Distance.FromMeters(System.Double)).
- Statische Methode [`Distance.FromKilometers`](xref:Xamarin.Forms.Maps.Distance.FromKilometers(System.Double)).
- Statische Methode [`Distance.FromMiles`](xref:Xamarin.Forms.Maps.Distance.FromMiles(System.Double)).

Der Wert steht über drei Eigenschaften zur Verfügung:

- [`Meters`](xref:Xamarin.Forms.Maps.Distance.Meters) vom Typ `double`
- [`Kilometers`](xref:Xamarin.Forms.Maps.Distance.Kilometers) vom Typ `double`
- [`Miles`](xref:Xamarin.Forms.Maps.Distance.Miles) vom Typ `double`

Die Datei [MapCoordinatesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml) enthält mehrere `Label`-Elemente zum Anzeigen der `MapSpan`-Informationen. Die CodeBehind-Datei [MapCoordinatesPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml.cs) verwendet einen Timer, um die Informationen aktuell zu halten, wenn der Benutzer die Karte ändert.

### <a name="position-extensions"></a>Positionserweiterungen

Eine neue Bibliothek für dieses Buch namens [**Xamarin.FormsBook.Toolkit.Maps**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit.Maps) enthält kartenspezifische, aber plattformunabhängige Typen. Die [`PositionExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs)-Klasse besitzt eine `ToString`-Methode für `Position` sowie eine Methode zum Berechnen der Entfernung zwischen zwei `Position`-Werten.

### <a name="setting-an-initial-location"></a>Festlegen eines Ausgangsorts

Sie können die [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan))-Methode von `Map` aufrufen, um einen Standort und Zoomfaktor programmgesteuert auf der Karte festzulegen. Das Argument ist vom Typ `MapSpan`. Sie können auf eine der folgenden Weisen ein `MapSpan`-Objekt erstellen:

- [`MapSpan`-Konstruktor](xref:Xamarin.Forms.Maps.MapSpan.%23ctor(Xamarin.Forms.Maps.Position,System.Double,System.Double)) mit einem `Position`-Element sowie einem Breiten- und Längengradbereich
- [`MapSpan.FromCenterAndRadius`](xref:Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius(Xamarin.Forms.Maps.Position,Xamarin.Forms.Maps.Distance)) mit einem `Position`-Element sowie einem Radius

Es ist auch möglich, eine neue `MapSpan` aus einer vorhandenen zu erstellen, indem Sie die Methoden [`ClampLatitude`](xref:Xamarin.Forms.Maps.MapSpan.ClampLatitude(System.Double,System.Double)) oder [`WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom(System.Double)) verwenden.

Die Datei [WyomingPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml) und die CodeBehind-Datei [WyomingPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml.cs) veranschaulichen, wie Sie die `MoveToRegion`-Methode verwenden, um den Bundesstaat Wyoming anzuzeigen.

Alternativ können Sie den [`Map`-Konstruktor](xref:Xamarin.Forms.Maps.Map.%23ctor(Xamarin.Forms.Maps.MapSpan)) mit einem `MapSpan`-Objekt verwenden, um den Standort der Karte zu initialisieren. Die Datei [XamarinHQPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/XamarinHQPage.xaml) zeigt, wie Sie dies vollständig in XAML machen, um die Hauptniederlassung von Xamarin in San Francisco anzuzeigen.

### <a name="dynamic-zooming"></a>Dynamisches Zoomen

Sie können einen `Slider` verwenden, um eine Karte dynamisch zu zoomen. Due Datei [RadiusZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml) und die CodeBehind-Datei [RadiusZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml.cs) zeigen, wie Sie den Radius einer Karte ändern, basierend auf dem `Slider`-Wert.

Die Datei [LongitudeZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml) und die CodeBehind-Datei [LongitudeZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml.cs) zeigen einen alternativen Ansatz, der unter Android besser funktioniert, aber keiner der Ansätze funktioniert gut auf der Windows-Plattform.

### <a name="the-phones-location"></a>Der Standort eines Telefons

Die [`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser)-Eigenschaft von `Map` funktioniert auf jeder Plattform etwas anders, wie die Datei [ShowLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ShowLocationPage.xaml) zeigt:

- Unter iOS zeigt ein blauer Punkt den Standort des Telefons an, aber Sie müssen manuell dorthin navigieren.
- Unter Android wird ein Symbol angezeigt, das, wenn Sie darauf drücken, die Karte an den Standort des Telefons verschiebt.
- Die UWP ähnelt iOS, navigiert aber manchmal automatisch zu dem Standort.

Das **MapDemos**-Projekt versucht, den Android-Ansatz nachzuahmen, indem zuerst eine symbolbasierte Schaltfläche auf Grundlage der Datei [MyLocationButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml) und der CodeBehind-Datei [MyLocationButton.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml.cs) definiert wird.

Die Datei [GoToLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml) und die CodeBehind-Datei [GoToLocationPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml.cs) verwenden diese Schaltfläche, um zum Standort des Telefons zu navigieren.

### <a name="pins-and-science-museums"></a>Stecknadeln und Wissenschaftsmuseen

Schließlich definiert die `Map`-Klasse eine [`Pins`](xref:Xamarin.Forms.Maps.Map.Pins)-Eigenschaft vom Typ `IList<Pin>`. Die [`Pin`](xref:Xamarin.Forms.Maps.Pin)-Klasse definiert vier Eigenschaften:

- [`Label`](xref:Xamarin.Forms.Maps.Pin.Label) vom Typ `string`, eine erforderliche Eigenschaft.
- [`Address`](xref:Xamarin.Forms.Maps.Pin.Address) vom Typ `string`, eine optionale, lesbare Adresse.
- [`Position`](xref:Xamarin.Forms.Maps.Pin.Position) vom Typ `Position`, die angibt, wo die Stecknadel auf der Karte angezeigt wird.
- [`Type`](xref:Xamarin.Forms.Maps.Pin.Type) vom Typ [`PinType`](xref:Xamarin.Forms.Maps.PinType), eine Enumeration, die nicht verwendet wird.

Das **MapDemos**-Projekt enthält die Datei [ScienceMuseums.xml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Data/ScienceMuseums.xml), in der Wissenschaftsmuseen der USA aufgelistet werden, sowie [`Locations`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Locations.cs) und [`Site`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Site.cs)-Klassen zum Deserialisieren dieser Daten.

Die Datei [ScienceMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml) und die CodeBehind-Datei [ScienceMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml.cs) zeigen Stecknadeln für diese Wissenschaftsmuseen auf der Karte an. Wenn der Benutzer auf eine Stecknadel tippt, werden die Adresse und eine Website für das Museum angezeigt.

### <a name="the-distance-between-two-points"></a>Die Entfernung zwischen zwei Punkten

Die [`PositionExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs)-Klasse enthält eine [`DistanceTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs#L88)-Methode mit einer vereinfachten Berechnung der Entfernung zwischen zwei geografischen Standorten.

Diese wird in der Datei [LocalMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml) und der CodeBehind-Datei [LocalMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml.cs) verwendet, um außerdem die Entfernung zu dem Museum vom Standort des Benutzers aus anzuzeigen:

[![Dreifacher Screenshot der Seite mit lokalen Museen](images/ch28fg28-small.png "Entfernung zu einem Ort")](images/ch28fg28-large.png#lightbox "Entfernung zu einem Ort")

Das Programm veranschaulicht außerdem, wie die Anzahl der Stecknadeln auf Grundlage des Standorts der Karte dynamisch eingeschränkt werden kann.

## <a name="geocoding-and-back-again"></a>Geocodierung und wieder zurück

Die [ **Xamarin.Forms.Maps**](xref:Xamarin.Forms.Maps)-Assembly enthält außerdem eine [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder)-Klasse mit einer [`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync(System.String))-Methode, die eine Textadresse in null oder mehr mögliche geografische Positionen konvertiert, sowie eine weitere Methode namens [`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync(Xamarin.Forms.Maps.Position)), die in die andere Richtung konvertiert.

Diese Funktion wird in der Datei [GeocoderRoundTrip.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml) und der CodeBehind-Datei [GeocoderRoundTrip.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml.cs) veranschaulicht.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 28 – vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch28-Aug2016.pdf)
- [Kapitel 28 – Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)
- [Xamarin.Forms-Karte](~/xamarin-forms/user-interface/map/index.md)
