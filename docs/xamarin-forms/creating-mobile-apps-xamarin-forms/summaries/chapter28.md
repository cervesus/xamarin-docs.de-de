---
title: Zusammenfassung zu Kapitel 28. Speicherort und Zuordnungen
description: 'Erstellen von Mobile Apps mit xamarin. Forms: Zusammenfassung von Kapitel 28. Speicherort und Zuordnungen'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F6E20077-687C-45C4-A375-31D4F49BBFA4
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 5dcd84536cc6d80deb753fc6fe57f9090f6b2dad
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697078"
---
# <a name="summary-of-chapter-28-location-and-maps"></a>Zusammenfassung zu Kapitel 28. Speicherort und Zuordnungen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)

> [!NOTE]
> Hinweise auf dieser Seite geben Bereiche an, in denen xamarin. Forms von dem im Buch dargestellten Material abweickte.

Xamarin. Forms unterstützt ein [`Map`](xref:Xamarin.Forms.Maps.Map) Element, das von `View` abgeleitet ist. Aufgrund der speziellen Platt Form Anforderungen in Bezug auf die Verwendung von Maps werden Sie in einer separaten Assembly ( **xamarin. Forms. Maps**) implementiert und umfassen einen anderen Namespace: `Xamarin.Forms.Maps`.

## <a name="the-geographic-coordinate-system"></a>Das geografische Koordinatensystem

Ein geografisches Koordinatensystem identifiziert Positionen auf einem kugelförmigen (oder fast kugelförmigen) Objekt wie der Erde. Eine Koordinate besteht aus einem *breiten* -und *Längen* Grad, der in Winkeln ausgedrückt wird.

Ein großartiger Kreis, der als `equator` bezeichnet wird, liegt zwischen den beiden Polen, über die die Achse der Erde konzeptionell erweitert wird.

### <a name="parallels-and-latitude"></a>Parallels und Breitengrad

Ein Winkel, der Nord oder Süd des Äquator von der Mitte der Erde gemessen wird, kennzeichnet Linien mit gleichem Breitengrad, die als *Parallels*bezeichnet werden. Diese reichen von 0 Grad am Äquator bis 90 Grad am Nord-und Südpol. Gemäß der Konvention sind die Breitengrade Nord des Äquator positive Werte, und die südlichen des Äquator sind negative Werte.

### <a name="longitude-and-meridians"></a>Längengrad und Meridiane

Die Hälfte der großen Kreise vom Nordpol zum südlichen Pol sind Linien mit gleichem Längengrad, auch als *Meridiane*bezeichnet. Diese sind relativ zum Prim Meridian in Greenwich, England. Gemäß der Konvention sind die Längengrad Ost des Prim Meridians positive Werte zwischen 0 und 180 Grad, und die Längenwerte im Westen des Prim Meridians sind negative Werte von 0 Grad bis &ndash;180 Grad.

### <a name="the-equirectangular-projection"></a>Die equirecht eckige Projektion

Jede flache Karte der Erde führt zu einer Verzerrung. Wenn alle breiten-und Längengrade gleich sind, und wenn gleichmäßige Unterschiede in den Bereichen breiten-und Längengrad auf der Karte gleich sind, ist das Ergebnis eine *equirecht eckige Projektion*. Diese Zuordnung verzerrt Bereiche, die sich näher an den Polen befinden, da Sie horizontal gestreckt werden.

### <a name="the-mercator-projection"></a>Die Mercator-Projektion

Die beliebte *Mercator-Projektion* versucht, die horizontale Streckung zu kompensieren, indem diese Bereiche ebenfalls vertikal gestreckt werden. Dies führt zu einer Karte, bei der Bereiche in der Nähe der Polen deutlich größer sind als Sie tatsächlich sind

### <a name="map-services-and-tiles"></a>Zuordnen von Diensten und Kacheln

Kartendienste verwenden eine Variation der Mercator-Projektion namens `Web Mercator`. Die Map-Dienste stellen Bitmap-Kacheln auf der Grundlage von Standort und Zoomstufe an einen Client bereit.

## <a name="getting-the-users-location"></a>Der Speicherort des Benutzers wird erhalten.

Die xamarin. Forms-`Map` Klassen enthalten keine Möglichkeit, den geografischen Standort des Benutzers zu erhalten. Dies ist jedoch beim Arbeiten mit Maps häufig wünschenswert, sodass ein Abhängigkeits Dienst ihn verarbeiten muss.

> [!NOTE]
> Xamarin. Forms-Anwendungen können stattdessen die [`Geolocation`](~/essentials/geolocation.md) -Klasse verwenden, die in xamarin. Essentials enthalten ist.

### <a name="the-location-tracker-api"></a>Die Location Tracker-API

Die [**xamarin. formsbook. Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) -Lösung enthält Code für eine Location Tracker-API. Die [`GeographicLocation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/GeographicLocation.cs) -Struktur kapselt einen breiten-und Längengrad. Die [`ILocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/ILocationTracker.cs) -Schnittstelle definiert zwei Methoden, um den Speicherort Tracker zu starten und anzuhalten, sowie ein Ereignis, wenn ein neuer Speicherort verfügbar ist.

#### <a name="the-ios-location-manager"></a>Der IOS Location Manager

Die IOS-Implementierung von `ILocationTracker` ist eine [`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/LocationTracker.cs) Klasse, die das IOS- [`CLLocationManager`](xref:CoreLocation.CLLocationManager)nutzt.

#### <a name="the-android-location-manager"></a>Der Android Location Manager

Die Android-Implementierung von `ILocationTracker` ist eine [`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/LocationTracker.cs) Klasse, die die Android- [`LocationManager`](xref:Android.Locations.LocationManager) -Klasse verwendet.

#### <a name="the-uwp-geo-locator"></a>Der UWP-erweiterungslocator

Die universelle Windows-Plattform Implementierung von `ILocationTracker` ist eine [`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/LocationTracker.cs) Klasse, die die UWP- [`Geolocator`](/uwp/api/Windows.Devices.Geolocation.Geolocator)verwendet.

### <a name="display-the-phones-location"></a>Speicherort des Telefons anzeigen

Das Beispiel " [**whereami**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/WhereAmI) " verwendet die Location Tracker, um den Speicherort des Telefons sowohl im Text als auch auf einer equirecht eckigen Karte anzuzeigen.

### <a name="the-required-overhead"></a>Der erforderliche Verwaltungsaufwand

Es ist ein zusätzlicher Aufwand erforderlich, damit der **eami** die Location Tracker verwendet. Zuerst müssen alle Projekte in der Projekt Mappe " **whereami** " Verweise auf die entsprechenden Projekte in **xamarin. formsbook. Platform**aufweisen, und jedes **whieami** -Projekt muss die `Toolkit.Init`-Methode aufrufen.

Ein zusätzlicher plattformspezifischer Aufwand in Form von Speicherort Berechtigungen ist erforderlich.

#### <a name="location-permission-for-ios"></a>Location-Berechtigung für IOS

Für IOS muss die **Info. plist** -Dateielemente enthalten, die den Text einer Frage enthalten, die den Benutzer auffordert, den Speicherort dieses Benutzers zuzulassen.

#### <a name="location-permissions-for-android"></a>Standort Berechtigungen für Android

Android-Anwendungen, die den Speicherort des Benutzers abrufen, müssen in der Datei "androidmanifest. xml" über eine ACCESS_FILE_LOCATION-Berechtigung verfügen.

#### <a name="location-permissions-for-the-uwp"></a>Speicherort Berechtigungen für die UWP

Eine universelle Windows-Plattform Anwendung muss über eine `location` Geräte Funktion verfügen, die in der Datei "Package. appxmanifest" gekennzeichnet ist.

## <a name="working-with-xamarinformsmaps"></a>Arbeiten mit xamarin. Forms. Maps

Bei der Verwendung der `Map`-Klasse sind mehrere Anforderungen beteiligt.

### <a name="the-nuget-package"></a>Das nuget-Paket

Die **xamarin. Forms. Maps** -nuget-Bibliothek muss der Anwendungsprojekt Mappe hinzugefügt werden. Die Versionsnummer muss mit dem derzeit installierten **xamarin. Forms** -Paket identisch sein.

### <a name="initializing-the-maps-package"></a>Initialisieren des Maps-Pakets

Die Anwendungsprojekte müssen die `Xamarin.FormsMaps.Init`-Methode abrufen, nachdem `Xamarin.Forms.Forms.Init` aufgerufen wurde.

### <a name="enabling-map-services"></a>Aktivieren von Map-Diensten

Da der `Map` den Speicherort des Benutzers abrufen kann, muss die Anwendung die Berechtigung für den Benutzer wie zuvor in diesem Kapitel beschrieben erhalten:

#### <a name="enabling-ios-maps"></a>Aktivieren von IOS-Karten

Eine IOS-Anwendung, die `Map` verwendet, benötigt zwei Zeilen in der Datei "Info. plist".

#### <a name="enabling-android-maps"></a>Aktivieren von Android-Karten

Ein Autorisierungs Schlüssel ist für die Verwendung von Google Map Services erforderlich. Dieser Schlüssel wird in die Datei " **androidmanifest. XML** " eingefügt. Außerdem erfordert die Datei " **androidmanifest. XML** " `manifest`-Tags, die an der Beschaffung des Benutzer Standorts beteiligt sind.

#### <a name="enabling-uwp-maps"></a>Aktivieren von UWP-Zuordnungen

Eine universelle Windows-Plattform Anwendung benötigt einen Autorisierungs Schlüssel für die Verwendung von "mit der Verwendung von" Dieser Schlüssel wird als Argument an die `Xamarin.FormsMaps.Init`-Methode übermittelt. Die Anwendung muss auch für Location-Dienste aktiviert werden.

### <a name="the-unadorned-map"></a>Die nicht geschmückte Karte

Das [**mapdemos**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/MapDemos) -Beispiel besteht aus einer [mapsdemohomepage. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml) -Datei und einer [MapsDemoHomePage.XAML.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml.cs) -Code Behind-Datei, die die Navigation zu verschiedenen Demonstrations Programmen ermöglicht.

Die Datei " [basicmappage. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/BasicMapPage.xaml) " zeigt, wie die [`Map`](xref:Xamarin.Forms.Maps.Map) Ansicht angezeigt wird. Standardmäßig wird die Stadt Rom angezeigt. die Zuordnung kann jedoch vom Benutzer bearbeitet werden.

Legen Sie zum Deaktivieren des horizontalen und vertikalen Bildlaufs die [`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled) -Eigenschaft auf `false` fest. Legen Sie zum Deaktivieren des Zoom [`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled) auf `false` fest. Diese Eigenschaften funktionieren möglicherweise nicht auf allen Plattformen.

### <a name="streets-and-terrain"></a>Straßen und Gelände

Sie können verschiedene Typen von Zuordnungen anzeigen, indem Sie die `Map`-Eigenschaft [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) vom Typ [`MapType`](xref:Xamarin.Forms.Maps.MapType), eine Enumeration mit drei Membern festlegen:

- [`Street`](xref:Xamarin.Forms.Maps.MapType.Street), der Standardwert
- [`Satellite`](xref:Xamarin.Forms.Maps.MapType.Satellite)
- [`Hybrid`](xref:Xamarin.Forms.Maps.MapType.Hybrid)

Die [maptypespage. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypesPage.xaml) -Datei zeigt, wie ein Optionsfeld verwendet wird, um den Kartentyp auszuwählen. Sie verwendet die [`RadioButtonManager`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioButtonManager.cs) -Klasse in der [**xamarin. formsbook. Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) -Bibliothek und eine-Klasse, die auf der [maptyperadiobutton. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypeRadioButton.xaml) -Datei basiert.

### <a name="map-coordinates"></a>Kartenkoordinaten

Ein Programm kann den aktuellen Bereich abrufen, der vom `Map` über die [`VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion) -Eigenschaft angezeigt wird. Diese Eigenschaft wird *nicht* durch eine bindbare Eigenschaft unterstützt, und es gibt keinen Benachrichtigungs Mechanismus, der anzeigt, wann Sie geändert wurde. ein Programm, das die Eigenschaft überwachen möchte, sollte daher wahrscheinlich einen Zeitgeber für diesen Zweck verwenden.

`VisibleRegion` ist vom Typ [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan), eine Klasse mit vier schreibgeschützten Eigenschaften:

- [`Center`](xref:Xamarin.Forms.Maps.MapSpan.Center) vom Typ [`Position`](xref:Xamarin.Forms.Maps.Position)
- [`LatitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LatitudeDegrees) vom Typ `double`, der die Höhe des angezeigten Flächen Bereichs angibt.
- [`LongitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LongitudeDegrees) vom Typ `double`, der die Breite des angezeigten Flächen Bereichs angibt.
- [`Radius`](xref:Xamarin.Forms.Maps.MapSpan.Radius) vom Typ [`Distance`](xref:Xamarin.Forms.Maps.Distance), der die Größe des größten Kreis Bereichs angibt, der auf der Karte sichtbar ist.

`Position` und `Distance` sind beide Strukturen. `Position` definiert zwei schreibgeschützte Eigenschaften, die über den [`Position`-Konstruktor](xref:Xamarin.Forms.Maps.Position.%23ctor(System.Double,System.Double))festgelegt werden:

- [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude)
- [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude)

`Distance` soll einen Einheiten unabhängigen Abstand durch die Umstellung zwischen Metrik-und englischen Einheiten bereitstellen. Ein `Distance` Wert kann auf verschiedene Arten erstellt werden:

- [`Distance`-Konstruktor](xref:Xamarin.Forms.Maps.Distance.%23ctor(System.Double)) mit einem Abstand in Metern
- [`Distance.FromMeters`](xref:Xamarin.Forms.Maps.Distance.FromMeters(System.Double)) statische Methode
- [`Distance.FromKilometers`](xref:Xamarin.Forms.Maps.Distance.FromKilometers(System.Double)) statische Methode
- [`Distance.FromMiles`](xref:Xamarin.Forms.Maps.Distance.FromMiles(System.Double)) statische Methode

Der Wert ist in drei Eigenschaften verfügbar:

- [`Meters`](xref:Xamarin.Forms.Maps.Distance.Meters) vom Typ `double`
- [`Kilometers`](xref:Xamarin.Forms.Maps.Distance.Kilometers) vom Typ `double`
- [`Miles`](xref:Xamarin.Forms.Maps.Distance.Miles) vom Typ `double`

Die [mapcoordinatespage. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml) -Datei enthält mehrere `Label` Elemente zum Anzeigen der `MapSpan` Informationen. Die [MapCoordinatesPage.XAML.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml.cs) -Code Behind-Datei verwendet einen Timer, um die Informationen zu erhalten, wenn der Benutzer die Zuordnung bearbeitet.

### <a name="position-extensions"></a>Positions Erweiterungen

Eine neue Bibliothek für dieses Buch mit dem Namen [**xamarin. formsbook. Toolkit. Maps**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit.Maps) enthält Karten spezifische, aber plattformunabhängige Typen. Die [`PositionExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) -Klasse verfügt über eine `ToString`-Methode für `Position` und eine-Methode, um den Abstand zwischen zwei `Position` Werten zu berechnen.

### <a name="setting-an-initial-location"></a>Festlegen eines Ausgangs Speicher Orts

Sie können die [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)) -Methode von `Map` aufgerufen, um einen Speicherort und eine Zoomstufe auf der Karte Programm gesteuert festzulegen. Das-Argument ist vom Typ `MapSpan`. Sie können ein `MapSpan`-Objekt mit einer der folgenden Optionen erstellen:

- [`MapSpan` Konstruktor](xref:Xamarin.Forms.Maps.MapSpan.%23ctor(Xamarin.Forms.Maps.Position,System.Double,System.Double)) mit einem `Position` und breiten-und Längengrad
- [`MapSpan.FromCenterAndRadius`](xref:Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius(Xamarin.Forms.Maps.Position,Xamarin.Forms.Maps.Distance)) mit einer `Position` und einem RADIUS

Es ist auch möglich, mithilfe der Methoden [`ClampLatitude`](xref:Xamarin.Forms.Maps.MapSpan.ClampLatitude(System.Double,System.Double)) oder [`WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom(System.Double))eine neue `MapSpan` aus einer vorhandenen zu erstellen.

Die Datei " [wyomingpage. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml) " und die [WyomingPage.XAML.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml.cs) -Code Behind-Datei veranschaulichen, wie die `MoveToRegion`-Methode verwendet wird, um den Status von Wyoming anzuzeigen.

Alternativ können Sie den [`Map`-Konstruktor](xref:Xamarin.Forms.Maps.Map.%23ctor(Xamarin.Forms.Maps.MapSpan)) mit einem `MapSpan`-Objekt verwenden, um den Speicherort der Zuordnung zu initialisieren. Die [xamarinhqpage. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/XamarinHQPage.xaml) -Datei zeigt, wie Sie dies vollständig in XAML durchführen können, um die Hauptniederlassung von xamarin in San Francisco anzuzeigen.

### <a name="dynamic-zooming"></a>Dynamisches Zoomen

Mit einem `Slider` können Sie eine Karte dynamisch vergrößern. Die Datei " [radiuszoompage. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml) " und die [RadiusZoomPage.XAML.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml.cs) -Code Behind-Datei zeigen, wie der Radius einer Karte basierend auf dem `Slider` Wert geändert wird.

Die Datei " [. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml) " und die Code Behind-Datei " [LongitudeZoomPage.XAML.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml.cs) " zeigen einen alternativen Ansatz, der besser für Android funktioniert, aber keines dieser Ansätze funktioniert auf den Windows-Plattformen.

### <a name="the-phones-location"></a>Der Speicherort des Telefons

Die [`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser) -Eigenschaft von `Map` funktioniert auf jeder Plattform etwas anders, wie die Datei " [showlocationpage. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ShowLocationPage.xaml) " veranschaulicht:

- Unter IOS gibt ein blauer Punkt den Speicherort des Telefons an, aber Sie müssen manuell dorthin navigieren.
- Unter Android wird ein Symbol angezeigt, das beim Überdrücken von per pushübertragung die Karte an den Speicherort des Telefons verschiebt.
- Die UWP ähnelt IOS, aber manchmal wird automatisch zum Speicherort navigiert.

Das Projekt **mapdemos** versucht, den Android-Ansatz nachzuahmen, indem zuerst eine Symbol basierte Schaltfläche auf der Grundlage der Datei [mylocationbutton. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml) und der [MyLocationButton.XAML.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml.cs) -Code Behind-Datei definiert wird.

Die Datei " [gotolocationpage. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml) " und die Code Behind-Datei " [GoToLocationPage.XAML.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml.cs) " verwenden diese Schaltfläche, um zum Speicherort des Telefons zu navigieren.

### <a name="pins-and-science-museums"></a>Pins und Wissenschaftsmuseen

Schließlich definiert die `Map` Klasse eine [`Pins`](xref:Xamarin.Forms.Maps.Map.Pins) Eigenschaft vom Typ `IList<Pin>`. Die [`Pin`](xref:Xamarin.Forms.Maps.Pin) -Klasse definiert vier Eigenschaften:

- [`Label`](xref:Xamarin.Forms.Maps.Pin.Label) vom Typ `string`, eine erforderliche Eigenschaft
- [`Address`](xref:Xamarin.Forms.Maps.Pin.Address) vom Typ `string`, eine optionale, lesbare Adresse
- [`Position`](xref:Xamarin.Forms.Maps.Pin.Position) vom Typ `Position`, der angibt, wo die PIN auf der Karte angezeigt wird.
- [`Type`](xref:Xamarin.Forms.Maps.Pin.Type) vom Typ [`PinType`](xref:Xamarin.Forms.Maps.PinType), eine Enumeration, die nicht verwendet wird.

Das **mapdemos** -Projekt enthält die Datei " [sciencemuseums. XML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Data/ScienceMuseums.xml)", die Science-Museen im USA und [`Locations`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Locations.cs) und [`Site`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Site.cs) Klassen zum Deserialisieren dieser Daten auflistet.

Die Datei " [sciencemuseumspage. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml) " und die Datei " [ScienceMuseumsPage.XAML.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml.cs) Code-Behind" zeigen Pins für diese Wissenschaftsmuseen in der Karte an. Wenn der Benutzer auf eine PIN tippt, werden die Adresse und eine Website für das Portal angezeigt.

### <a name="the-distance-between-two-points"></a>Der Abstand zwischen zwei Punkten.

Die [`PositionExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) -Klasse enthält eine [`DistanceTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs#L88) Methode mit einer vereinfachten Berechnung der Entfernung zwischen zwei geografischen Standorten.

Diese wird in der Datei " [localmuseumspage. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml) " und in der [LocalMuseumsPage.XAML.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml.cs) -Code Behind-Datei verwendet, um auch die Entfernung zum Museum aus der Position des Benutzers anzuzeigen:

[![Dreifacher Screenshot der Seite "lokale Museen"](images/ch28fg28-small.png "Entfernung zu einem Speicherort")](images/ch28fg28-large.png#lightbox "Entfernung zu einem Speicherort")

Das Programm veranschaulicht außerdem, wie die Anzahl der Pins basierend auf dem Speicherort der Karte dynamisch eingeschränkt wird.

## <a name="geocoding-and-back-again"></a>Geocodierung und wieder zurück

Die [**xamarin. Forms. Maps**](xref:Xamarin.Forms.Maps) -Assembly enthält auch eine [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) -Klasse mit einer [`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync(System.String)) -Methode, die eine Text Adresse in NULL oder mehr mögliche geografische Positionen konvertiert, und eine andere Methode [`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync(Xamarin.Forms.Maps.Position)) , die in die andere konvertiert wird. Orientierung.

Diese Funktion wird in der Datei " [geocoderroundtrip. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml) " und der [GeocoderRoundTrip.XAML.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml.cs) -Code Behind-Datei veranschaulicht.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 28 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch28-Aug2016.pdf)
- [Kapitel 28 Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)
- [Xamarin. Forms-Karte](~/xamarin-forms/user-interface/map/index.md)
