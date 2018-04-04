---
title: Zusammenfassung der Kapitel 28. Speicherort und Karten
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F6E20077-687C-45C4-A375-31D4F49BBFA4
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 40d2b67f1a1655ec1d731493446f320b8aef17ca
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-28-location-and-maps"></a>Zusammenfassung der Kapitel 28. Speicherort und Karten

Xamarin.Forms unterstützt eine [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) Element, das von abgeleitet ist `View`. Aufgrund von besonderen Anforderungen an die Plattform beteiligten anhand von Karten, werden sie in einer separaten Assembly implementiert **Xamarin.Forms.Maps**, und einen anderen Namespace umfassen: `Xamarin.Forms.Maps`.

## <a name="the-geographic-coordinate-system"></a>Das geografische Koordinatensystem

Ein geografisches Koordinatensystem identifiziert Positionen für ein Objekt sphärischen (oder nahezu sphärischen) wie der Erde dar. Eine Koordinate beider besteht aus einem *Breitengrad* und *Längengrad* Winkel ausgedrückt.

Wird aufgerufen, ein Kreis hervorragend die `equator` ist in der Mitte zwischen den zwei Pole zurück, die über die Achse der Erde konzeptionell erweitert.

### <a name="parallels-and-latitude"></a>Parallels und Breitengrad

Ein Winkel nördlich bzw. südlich des Äquators aus der Mitte der Erde Marken gemessen Zeilen gleich Breitengrad genannt *Parallels*. Diese reichen von 0 Grad am Äquator 90 Grad an die North "und" Süd Polen reichen. Gemäß der Konvention Breitengraden nördlich vom Äquator werden positive Werte zulässig sind, und die südlich des Äquators negative Werte sind.

### <a name="longitude-and-meridians"></a>Längen- und meridiane

Bildhälfte gute Kreise vom Nord-zum Südpol sind Zeilen gleich Länge, auch bekannt als *meridiane*. Dies sind relativ zu dem Nullmeridian in Greenwich in England. Gemäß der Konvention Längengrade einen vom Nullmeridian sind positive Werte von 0 Grad um 180 Grad und Längengrade westlich vom Nullmeridian negative Werte von 0 Grad ein, um &ndash;um 180 Grad.

### <a name="the-equirectangular-projection"></a>Die equirektangularprojektion

Jede flache Zuordnung der Erde dar, führt Verzerrung. Wenn alle Zeilen der Breiten- und Längengrad gerade sind, und auf der Karte in gleichmäßigen Abständen gleich Unterschiede in den Breiten- und Längengrad Winkel entsprechen, das Ergebnis ist einer *equirektangularprojektion*. Diese Zuordnung verzerrt Bereiche näher zu den Polen erstrecken, da sie horizontal gestreckt werden.

### <a name="the-mercator-projection"></a>Die Mercator-Projektion

Der beliebten *Mercator-Projektion* versucht, für die horizontale Strecken durch Strecken auch diese Bereiche vertikal zu kompensieren. Dies führt zu einer Zuordnung, die Bereiche in der Nähe der Polen viel größer Anzeigeposition, als dies tatsächlich der Fall, aber LAN sehr eng mit dem tatsächlichen Bereich entspricht.

### <a name="map-services-and-tiles"></a>Map-Dienste und Kacheln

Map-Dienste verwenden eine Variation des die Mercator-Projektion bezeichnet `Web Mercator`. Die Zuordnung Dienste übermitteln Bitmap-Kacheln an einen Client basierend auf den Speicherort und der Zoomstufe an.

## <a name="getting-the-users-location"></a>Vom Standort des Benutzers abrufen

Der Xamarin.Forms `Map` Klassen nehmen Sie eine Funktion zum Abrufen der geografische Standort des Benutzers nicht, aber dies ist häufig wünschenswert, beim Arbeiten mit Zuordnungen, also eine Abhängigkeitsdienst behandeln muss.

### <a name="the-location-tracker-api"></a>Der Quellspeicherort-Nachfolgegerät API

Die [ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) Lösung enthält Code für ein Quellspeicherort-Nachfolgegerät API. Die [ `GeographicLocation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/GeographicLocation.cs) Struktur kapselt, eine Breiten- und Längengrad. Die [ `ILocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/ILocationTracker.cs) Schnittstelle definiert zwei Methoden zum Starten und Anhalten der Quellspeicherort-Nachfolgegerät und ein Ereignis, wenn Sie ein neuer Speicherort verfügbar ist.

#### <a name="the-ios-location-manager"></a>Der Quellspeicherort-Manager für iOS

Die iOS-Implementierung von `ILocationTracker` ist ein [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/LocationTracker.cs) , mit der, Klasse verwenden, die e/as [ `CLLocationManager` ](https://developer.xamarin.com/api/type/CoreLocation.CLLocationManager/).

#### <a name="the-android-location-manager"></a>Android-Standort-manager

Die Android-Implementierung von `ILocationTracker` ist ein [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/LocationTracker.cs) -Klasse, die die Android Nutzung von [ `LocationManager` ](https://developer.xamarin.com/api/type/Android.Locations.LocationManager/) Klasse.

#### <a name="the-windows-runtime-geo-locator"></a>Der Windows-Runtime-Geo-locator

Die Windows-Runtime-Implementierung von `ILocationTracker` ist ein [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/LocationTracker.cs) -Klasse, die die universelle Windows-Plattform Nutzung von [ `Geolocator` ](https://msdn.microsoft.com/library/windows/apps/br225534).

### <a name="display-the-phones-location"></a>Anzeigen des Telefons-Standorts

Die [ **WhereAmI** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/WhereAmI) -Beispiel verwendet die Quellspeicherort-Nachfolgegerät zum Speicherort des Telefons, sowohl Text als auch auf einer Karte Equirectangular anzeigen.

### <a name="the-required-overhead"></a>Der erforderliche Aufwand

Ein gewisser Aufwand ist erforderlich, damit **WhereAmI** Quellspeicherort-Nachfolgegerät verwenden. Erstens aller Projekte in der **WhereAmI** Lösung benötigen Verweise auf den entsprechenden Projekten in **Xamarin.FormsBook.Platform**, und jede **WhereAmI** Projekt Rufen Sie müssen die `Toolkit.Init` Methode.

Einige plattformspezifischen Mehraufwand in Form von Berechtigungen, ist erforderlich.

#### <a name="location-permission-for-ios"></a>Speicherort-Berechtigung für iOS

Für iOS die **"Info.plist"** Includedatei müssen Elemente mit dem Text der Frage des Benutzers zum Abrufen des Benutzers-Speicherorts zulassen.

#### <a name="location-permissions-for-android"></a>Berechtigungen für Android

Android-Anwendungen, die vom Standort des Benutzers zu erhalten, müssen eine ACCESS_FILE_LOCATION-Berechtigung in der Datei "AndroidManifest.xml" sein.

#### <a name="location-permissions-for-the-windows-runtime"></a>Berechtigungen für die Windows-Runtime

Eine Windows- oder Windows Phone-Anwendung benötigen eine `location` Gerätefunktion in der Datei "Package.appxmanifest" gekennzeichnet.

## <a name="working-with-xamarinformsmaps"></a>Arbeiten mit Xamarin.Forms.Maps

Mehrere Anforderungen bei der Verwendung beteiligt sind die `Map` Klasse.

### <a name="the-nuget-package"></a>Das NuGet-Paket

Die **Xamarin.Forms.Maps** NuGet-Bibliothek zur der webanwendungslösung hinzugefügt werden. Die Versionsnummer muss identisch mit der **Xamarin.Forms** Paket zurzeit installiert.

### <a name="initializing-the-maps-package"></a>Initialisieren der Maps-Paket

Die Anwendungsprojekte aufrufen, müssen die `Xamarin.FormsMaps.Init` -Methode nach einem Aufruf an `Xamarin.Forms.Forms.Init`.

### <a name="enabling-map-services"></a>Map-Dienste aktivieren

Da die `Map` können vom Standort des Benutzers abrufen, die Anwendung muss über die Berechtigung für den Benutzer auf die weiter oben in diesem Kapitel beschriebenen Weise abrufen:

#### <a name="enabling-ios-maps"></a>Aktivieren der iOS zugeordnet

Eine iOS-Anwendung mit `Map` benötigt zwei Zeilen in der Datei "Info.plist".

#### <a name="enabling-android-maps"></a>Ordnet Android aktivieren

Ein Schlüssel für die Autorisierung ist für die Verwendung von Google-Karte Services erforderlich. Dieser Schlüssel wird eingefügt, der **AndroidManifest.xml** Datei. Darüber hinaus die **AndroidManifest.xml** Datei erfordert `manifest` Tags beteiligt, die vom Standort des Benutzers abrufen.

#### <a name="enabling-windows-runtime-maps"></a>Aktivieren der Windows-Runtime ordnet

Eine Windows-Runtime-Anwendung erfordert einen Schlüssel für die Autorisierung für die Verwendung von Bing Maps. Dieser Schlüssel wird übergeben, als Argument an die `Xamarin.FormsMaps.Init` Methode. Die Anwendung muss auch für ortsbezogene Dienste aktiviert werden.

### <a name="the-unadorned-map"></a>Die Zuordnung nicht erweiterten

Die [ **MapDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/MapDemos) Beispiel besteht aus einem [MapsDemoHomePage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml) Datei und [MapsDemoHomePage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml.cs) Code-Behind-Datei, die mit ermöglicht das Navigieren zu verschiedenen Demoprogramme.

Die [BasicMapPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/BasicMapPage.xaml) Datei veranschaulicht, wie die [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) anzeigen. Standardmäßig zeigt es die Stadt des ROM, aber die Zuordnung kann vom Benutzer bearbeitet werden.

Legen Sie zum Deaktivieren der horizontalen und vertikalen Bildlauf der [ `HasScrollEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.HasScrollEnabled/) Eigenschaft `false`. Legen Sie zum Deaktivieren des Zoomens [ `HasZoomEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.HasZoomEnabled/) auf `false`. Diese Eigenschaften funktionieren möglicherweise nicht auf allen Plattformen.

### <a name="streets-and-terrain"></a>Straßen und Terrain

Sie können verschiedene Typen von Karten anzeigen, indem Sie die Einstellung der `Map` Eigenschaft [ `MapType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.MapType/) des Typs [ `MapType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapType/), eine Enumeration mit drei Membern:

- [`Street`](https://developer.xamarin.com/api/field/Xamarin.Forms.Maps.MapType.Street/), der Standardwert
- [`Satellite`](https://developer.xamarin.com/api/field/Xamarin.Forms.Maps.MapType.Satellite/)
- [`Hybrid`](https://developer.xamarin.com/api/field/Xamarin.Forms.Maps.MapType.Hybrid/)

Die [MapTypesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypesPage.xaml) Datei zeigt, wie ein Optionsfeld zu verwenden, um die Map-Typ auszuwählen. Er nutzt die [ `RadioButtonManager` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioButtonManager.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) -Bibliothek und einer Klasse auf der Grundlage der [MapTypeRadioButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypeRadioButton.xaml) Datei.

### <a name="map-coordinates"></a>Kartenkoordinaten

Ein Programm kann im aktuellen Bereich, das Abrufen der `Map` angezeigt, über die [ `VisibleRegion` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.VisibleRegion/) Eigenschaft. Diese Eigenschaft ist *nicht* durch eine bindbare Eigenschaft gesichert ist und es gibt keinen Benachrichtigungsmechanismus an, wenn es geändert wurde, damit ein Programm, das die Eigenschaft überwachen möchte einen Zeitgeber wahrscheinlich zu diesem Zweck verwenden soll.

`VisibleRegion` ist vom Typ [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/), eine Klasse mit vier schreibgeschützte Eigenschaften:

- [`Center`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.Center/) vom Typ [`Position`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/)
- [`LatitudeDegrees`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.LatitudeDegrees/) Der Typ `double`, der die Höhe von den angezeigten Bereich der Karte angibt
- [`LongitudeDegrees`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.LongitudeDegrees/) Der Typ `double`, der angibt, der Breite des angezeigten Bereich der Karte
- [`Radius`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.Radius/) Der Typ [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/), der angibt, der Größe des größten kreisbereiche auf der Karte angezeigt.

`Position` und `Distance` sind beide Strukturen. `Position` definiert zwei schreibgeschützte Eigenschaften festlegen, über die [ `Position` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Position.Position/p/System.Double/System.Double/):

- [`Latitude`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Position.Latitude/)
- [`Longitude`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Position.Longitude/)

`Distance` soll einen Einheit typunabhängig Abstand bieten durch Konvertieren zwischen Metrik und englische Einheiten. Ein `Distance` Wert kann auf verschiedene Arten erstellt werden:

- [`Distance` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Distance.Distance/p/System.Double/) mit einem Abstand in Metern
- [`Distance.FromMeters`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromMeters/p/System.Double/) statische Methode
- [`Distance.FromKilometers`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromKilometers/p/System.Double/) statische Methode
- [`Distance.FromMiles`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromMiles/p/System.Double/) statische Methode

Der Wert ist in drei Eigenschaften verfügbar:

- [`Meters`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Meters/) vom Typ `double`
- [`Kilometers`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Kilometers/) vom Typ `double`
- [`Miles`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Miles/) vom Typ `double`

Die [MapCoordinatesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml) -Datei enthält mehrere `Label` Elemente zum Anzeigen der `MapSpan` Informationen. Die [MapCoordinatesPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml.cs) Code-Behind-Datei verwendet einen Zeitgeber, behalten Sie die Informationen aktualisiert, wenn der Benutzer die Karte bearbeitet.

### <a name="position-extensions"></a>Position-Erweiterungen

Eine neue Bibliothek für diese mit dem Namen Book [ **Xamarin.FormsBook.Toolkit.Maps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit.Maps) Map-spezifische jedoch plattformunabhängige Typen enthält. Die [ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) -Klasse verfügt über eine `ToString` Methode zum `Position`, und eine Methode zum Berechnen des Abstands zwischen zwei `Position` Werte.

### <a name="setting-an-initial-location"></a>Festlegen einer anfänglichen Speicherorts

Sie erreichen die [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion/p/Xamarin.Forms.Maps.MapSpan/) Methode `Map` programmgesteuert eine Speicherort und Zoom-Ebene in der Zuordnung festgelegt. Das Argument ist vom Typ `MapSpan`. Sie erstellen eine `MapSpan` -Objekt mithilfe eines der folgenden:

- [`MapSpan` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.MapSpan.MapSpan/p/Xamarin.Forms.Maps.Position/System.Double/System.Double/) mit einem `Position`, und die Breiten- und Längengrad Spanne
- [`MapSpan.FromCenterAndRadius`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius/p/Xamarin.Forms.Maps.Position/Xamarin.Forms.Maps.Distance/) mit einem `Position` und Radius

Es ist auch möglich, zum Erstellen eines neuen `MapSpan` aus einer vorhandenen mithilfe der Methoden [ `ClampLatitude` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.ClampLatitude/p/System.Double/System.Double/) oder [ `WithZoom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.WithZoom/p/System.Double/).

Die [WyomingPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml) Datei und [WyomingPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml.cs) Code-Behind-Datei veranschaulicht, wie die `MoveToRegion` Methode, um den Status des Wyoming anzuzeigen.

Alternativ können Sie die [ `Map` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Map.Map/p/Xamarin.Forms.Maps.MapSpan/) mit einem `MapSpan` Objekt, um den Speicherort der Zuordnung zu initialisieren. Die [XamarinHQPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/XamarinHQPage.xaml) Datei zeigt, wie dazu vollständig in XAML die Xamarin Hauptsitz in San Francisco angezeigt.

### <a name="dynamic-zooming"></a>Dynamische vergrößern/verkleinern

Sie können eine `Slider` dynamisch eine Zuordnung zu vergrößern. Die [RadiusZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml) Datei und [RadiusZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml.cs) Code-Behind-Datei wird gezeigt, wie so ändern Sie den Radius einer Zuordnung auf der Grundlage der `Slider` Wert.

Die [LongitudeZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml) Datei und [LongitudeZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml.cs) Code-Behind-Datei anzeigen, eine alternative Methode, die eine bessere Leistung für Android verwendet werden kann, aber keiner dieser Ansätze funktioniert gut auf der Windows Plattformen.

### <a name="the-phones-location"></a>Speicherort des Telefons

Die [ `IsShowingUser` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.IsShowingUser/) Eigenschaft `Map` funktioniert etwas unterschiedlich, auf die drei Plattformen wie die [ShowLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ShowLocationPage.xaml) Datei veranschaulicht:

- Bei iOS kann der blauer Punkt des Telefons Speicherort angibt, jedoch müssen Sie manuell navigieren vorhanden
- Auf Android-Geräten ein Symbol angezeigt wird, die bei abgelegt, wechselt der Karte, um den Speicherort des Telefons
- Die universelle Windows-Plattform iOS ähnelt, aber in einigen Fällen automatisch navigiert zu der Position

Die **MapDemos** Projekt versucht, Android Ansatzes zu imitieren, indem Sie zunächst definieren ein Symbol basierend auf der Grundlage der [MyLocationButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml) Datei und [MyLocationButton.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml.cs) Code-Behind-Datei.

Die [GoToLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml) Datei und [GoToLocationPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml.cs) Code-Behind-Datei mit dieser Schaltfläche können Sie um zum Speicherort des Telefons zu navigieren.

### <a name="pins-and-science-museums"></a>Pins und Wissenschaft Museen

Schließlich die `Map` Klasse definiert ein [ `Pins` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.Pins/) Eigenschaft vom Typ `IList<Pin>`. Die [ `Pin` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Pin/) Klasse definiert vier Eigenschaften:

- [`Label`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Label/) Der Typ `string`, eine erforderliche Eigenschaft
- [`Address`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Address/) Der Typ `string`, eine optionale lesbare Adresse
- [`Position`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Position/) Der Typ `Position`, der angibt, denen die Pin auf der Karte angezeigt
- [`Type`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Type/) Der Typ [ `PinType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.PinType/), eine Enumeration, die nicht verwendet wird

Die **MapDemos** Projekt enthält die Datei [ScienceMuseums.xml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Data/ScienceMuseums.xml), womit Science Museen in den USA und [ `Locations` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Locations.cs) und [ `Site` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Site.cs) Klassen für diese Daten zu deserialisieren.

Die [ScienceMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml) Datei und [ScienceMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml.cs) Code-Behind-Datei Anzeige Pins für diese Science Museen in der Zuordnung. Wenn der Benutzer eine Pin tippt, zeigt die Adresse und eine Website für das Museum.

### <a name="the-distance-between-two-points"></a>Der Abstand zwischen zwei Punkten

Die [ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) Klasse enthält eine [ `DistanceTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs#L88) Methode mit einer vereinfachten Berechnung der Entfernung zwischen zwei geografisch getrennten Standorten.

Hiermit wird die [LocalMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml) Datei und [LocalMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml.cs) Code-Behind-Datei auch die Entfernung dem Museum aus den Standort des Benutzers angezeigt werden sollen:

[![Dreifacher Screenshot der Seite mit lokalen Museen](images/ch28fg28-small.png "Abstand an einem Speicherort")](images/ch28fg28-large.png#lightbox "Abstand an einem Speicherort")

Das Programm wird gezeigt, wie die Anzahl von Pins basierend auf den Speicherort der Zuordnung dynamisch zu beschränken.

## <a name="geocoding-and-back-again"></a>Codieren und wieder

Die [ **Xamarin.Forms.Maps** ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) Assembly enthält auch eine [ `Geocoder` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Geocoder/) -Klasse mit einem [ `GetPositionsForAddressAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync/p/System.String/) Methode, die konvertiert eine Textadresse in 0 (null) oder mehr mögliche geografischen Positionen und eine andere Methode [ `GetAddressesForPositionAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync/p/Xamarin.Forms.Maps.Position/) , der in die andere Richtung konvertiert.

Die [GeocoderRoundTrip.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml) Datei und [GeocoderRoundTrip.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml.cs) Code-Behind-Datei veranschaulicht diese Funktion.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 28 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch28-Aug2016.pdf)
- [Kapitel 28-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)
- [Kartensteuerelement](~/xamarin-forms/user-interface/map.md)
