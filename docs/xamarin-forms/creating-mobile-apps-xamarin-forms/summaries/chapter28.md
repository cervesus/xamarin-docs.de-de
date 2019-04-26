---
title: Zusammenfassung der Kapitel 28. Position und Karten
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 28. Position und Karten'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F6E20077-687C-45C4-A375-31D4F49BBFA4
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 0abd7e6cb5b8b9650a3dc324338587ff59a80a19
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61331457"
---
# <a name="summary-of-chapter-28-location-and-maps"></a>Zusammenfassung der Kapitel 28. Position und Karten

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)

> [!NOTE]
> Anmerkungen zu dieser Version auf dieser Seite Geben Sie Bereiche, in denen Xamarin.Forms aus den Informationen im Buch abweichend hat, an.

Xamarin.Forms unterstützt eine [ `Map` ](xref:Xamarin.Forms.Maps.Map) -Element, das von abgeleitet ist `View`. Aufgrund der speziellen Anforderungen an die Plattform beteiligt, die anhand von Karten, werden sie in einer separaten Assembly, implementiert **Xamarin.Forms.Maps**, und einen anderen Namespace umfassen: `Xamarin.Forms.Maps`.

## <a name="the-geographic-coordinate-system"></a>Das geografische Koordinatensystem

Ein geografisches Koordinatensystem identifiziert Positionen auf ein kugelförmiges (oder fast sphärischen)-Objekt, wie der Erde. Eine Koordinate beider besteht eine *Latitude* und *Längengrad* Winkel ausgedrückt.

Wird aufgerufen, ein Kreis hervorragend die `equator` genau zwischen den zwei Pole zurück, die über den der Erdachse konzeptionell erweitert wird.

### <a name="parallels-and-latitude"></a>Parallels und Breitengrad

Ein Winkel nördlich bzw. südlich des Äquators aus der Mitte der Erde Markierungen gemessen Codezeilen gleich Latitude genannt *Parallels*. Diese reichen von 0 Grad am Äquator und 90 Grad Norden und Süden Polen erstrecken. Gemäß der Konvention Breitengraden nördlich vom Äquator sind positive Werte und die südlich des Äquators werden negative Werte.

### <a name="longitude-and-meridians"></a>Längen- und meridiane

Hälften des großen Kreise vom Nord-zum Südpol werden Zeilen gleich Längengrad, auch bekannt als *meridiane*. Dies sind relativ zu dem Nullmeridian in Greenwich in England. Gemäß der Konvention Längengrade östlich vom Nullmeridian sind positive Werte von 0 Grad um 180 Grad und Längengraden westlicher Richtung gemessen wird dem Nullmeridian negative Werte von 0 Grad um &ndash;um 180 Grad.

### <a name="the-equirectangular-projection"></a>Die equirektangularprojektion

Jede flache Zuordnung der Erde dar, führt Anzeigegeräts. Wenn alle Zeilen aus Breiten- und Längengrad gerade sind, und gleich Unterschiede in der Breiten- und Längengrad Winkel in gleichmäßigen Abständen auf der Karte entsprechen, das Ergebnis ist einer *equirektangularprojektion*. Diese Zuordnung verzerrt Bereiche näher zu den Polen erstrecken, da sie horizontal gestreckt werden.

### <a name="the-mercator-projection"></a>Die Mercator-Projektion

Das beliebte *Mercator-Projektion* versucht, die für die horizontale größenanpassung durch stretching auch diese Bereiche vertikal zu kompensieren. Dies führt zu einer Zuordnung, an der Bereiche in der Nähe der Polen viel größer, als sie eigentlich sind, jedoch LAN sehr eng mit den tatsächlichen Bereich entspricht angezeigt.

### <a name="map-services-and-tiles"></a>Map-Dienste und Kacheln

Map-Dienste verwenden eine Variation des die Mercator-Projektion wird aufgerufen, `Web Mercator`. Die Map-Dienste bieten Bitmap-Kacheln auf einem Client basierend auf den Speicherort und der Zoomstufe.

## <a name="getting-the-users-location"></a>Die Position des Benutzers abrufen

Die Xamarin.Forms `Map` Klassen umfassen eine Funktion zum Abrufen der geografische Standort des Benutzers nicht, aber dies ist häufig wünschenswert, bei der Arbeit mit Zuordnungen, also eine Abhängigkeitsdienst behandeln muss.

> [!NOTE]
> Xamarin.Forms-Anwendungen können stattdessen die [ `Geolocation` ](~/essentials/geolocation.md) Klasse, die in Xamarin.Essentials enthalten.

### <a name="the-location-tracker-api"></a>Die API-Nachfolgegerät

Die [ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) Lösung enthält Code für eine API-Nachfolgegerät. Die [ `GeographicLocation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/GeographicLocation.cs) Struktur kapselt einen breiten- und Längengrad. Die [ `ILocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/ILocationTracker.cs) Schnittstelle definiert zwei Methoden zum Starten und Anhalten der nachverfolgung von Speicherort und ein Ereignis, wenn Sie ein neuer Speicherort verfügbar ist.

#### <a name="the-ios-location-manager"></a>Der Quellspeicherort-Manager für iOS

Die iOS-Implementierung von `ILocationTracker` ist eine [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/LocationTracker.cs) Klasse, mit der, Verwendung des iOS [ `CLLocationManager` ](xref:CoreLocation.CLLocationManager).

#### <a name="the-android-location-manager"></a>Der Speicherort von Android-manager

Die Android-Implementierung von `ILocationTracker` ist eine [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/LocationTracker.cs) -Klasse, die die Android verwendet [ `LocationManager` ](https://developer.xamarin.com/api/type/Android.Locations.LocationManager/) Klasse.

#### <a name="the-uwp-geo-locator"></a>Die UWP-Geo-locator

Die universelle Windows-Plattform-Implementierung von `ILocationTracker` ist eine [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/LocationTracker.cs) -Klasse, die die UWP verwendet [ `Geolocator` ](/uwp/api/Windows.Devices.Geolocation.Geolocator).

### <a name="display-the-phones-location"></a>Anzeigen des Telefons-Standorts

Die [ **WhereAmI** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/WhereAmI) Beispiel verwendet die Speicherort-nachverfolgung, um die des Standorts des Telefons, sowohl Text als auch auf einer Karte Equirectangular anzuzeigen.

### <a name="the-required-overhead"></a>Der erforderliche Aufwand

Zusätzlichen Aufwand ist erforderlich, damit **WhereAmI** die Speicherort-nachverfolgung verwenden. Zuerst alle Projekte in der **WhereAmI** Lösung müssen Verweise auf die entsprechenden Projekte in **Xamarin.FormsBook.Platform**, und jede **WhereAmI** Projekt Rufen Sie müssen die `Toolkit.Init` Methode.

Zusätzliche plattformspezifische Reserven zu schaffen, in Form von Berechtigungen für Positionsdaten, ist erforderlich.

#### <a name="location-permission-for-ios"></a>Speicherortberechtigung für iOS

Für iOS die **"Info.plist"** Datei muss die Elemente, die mit dem Text einer Frage, die den Benutzer auffordert, können die erste Standort des Benutzers enthalten.

#### <a name="location-permissions-for-android"></a>Berechtigungen für Positionsdaten für Android

Android-Anwendungen, die den Standort des Benutzers zu erhalten, müssen eine ACCESS_FILE_LOCATION-Berechtigung in der Datei "androidmanifest.xml" verfügen.

#### <a name="location-permissions-for-the-uwp"></a>Berechtigungen für Positionsdaten für die UWP

Eine Anwendung für die universelle Windows-Plattform benötigen eine `location` Gerätefunktion, die in der Datei "Package.appxmanifest" markiert.

## <a name="working-with-xamarinformsmaps"></a>Arbeiten mit Xamarin.Forms.Maps

Mehrere Anforderungen erfolgt mithilfe der `Map` Klasse.

### <a name="the-nuget-package"></a>Das NuGet-Paket

Die **Xamarin.Forms.Maps** der webanwendungslösung muss NuGet-Bibliothek hinzugefügt werden. Die Versionsnummer muss identisch mit der **Xamarin.Forms** Paket zurzeit installiert.

### <a name="initializing-the-maps-package"></a>Initialisieren das Maps-Paket

Die Anwendungsprojekte müssen aufrufen, die `Xamarin.FormsMaps.Init` -Methode nach einem Aufruf an `Xamarin.Forms.Forms.Init`.

### <a name="enabling-map-services"></a>Map-Dienste aktivieren

Da die `Map` können den Standort des Benutzers abrufen, die Anwendung muss über die Berechtigung für den Benutzer auf die weiter oben in diesem Kapitel beschriebenen Weise abrufen:

#### <a name="enabling-ios-maps"></a>Aktivieren der iOS zugeordnet

Eine iOS-Anwendung mit `Map` zwei Zeilen in der Datei "Info.plist" benötigt.

#### <a name="enabling-android-maps"></a>Aktivieren der Android zugeordnet

Ein Autorisierungsschlüssel ist erforderlich, für die Verwendung von Google-Map-Dienste. Dieser Schlüssel wird eingefügt, der **"androidmanifest.xml"** Datei. Darüber hinaus die **"androidmanifest.xml"** Konfigurationsincludedatei `manifest` Tags beteiligt, die den Standort des Benutzers abrufen.

#### <a name="enabling-uwp-maps"></a>Aktivieren der UWP zugeordnet

Eine universelle Windows-Plattform-Anwendung erfordert einen Autorisierungsschlüssel, für die Verwendung von Bing Maps. Dieser Schlüssel wird als Argument für das Übergeben der `Xamarin.FormsMaps.Init` Methode. Die Anwendung muss auch für Standortdienste aktiviert werden.

### <a name="the-unadorned-map"></a>Die Zuordnung nicht erweiterten

Die [ **MapDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/MapDemos) Beispiel besteht aus einem [MapsDemoHomePage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml) Datei und [MapsDemoHomePage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml.cs) Code-Behind-Datei, die ermöglicht das Navigieren zu verschiedenen Demoprogramme.

Die [BasicMapPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/BasicMapPage.xaml) zeigt, wie zum Anzeigen der [ `Map` ](xref:Xamarin.Forms.Maps.Map) anzeigen. Standardmäßig zeigt es die Stadt "ROME", aber die Zuordnung kann vom Benutzer bearbeitet werden.

Legen Sie zum Deaktivieren der horizontalen und vertikalen Bildlauf der [ `HasScrollEnabled` ](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled) Eigenschaft `false`. Legen Sie zum Deaktivieren des Zoomens [ `HasZoomEnabled` ](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled) zu `false`. Diese Eigenschaften funktionieren möglicherweise nicht auf allen Plattformen.

### <a name="streets-and-terrain"></a>Straßen und Gelände

Sie können verschiedene Typen von Karten anzeigen, indem Sie die Einstellung der `Map` Eigenschaft [ `MapType` ](xref:Xamarin.Forms.Maps.Map.MapType) des Typs [ `MapType` ](xref:Xamarin.Forms.Maps.MapType), eine Enumeration mit drei Elementen:

- [`Street`](xref:Xamarin.Forms.Maps.MapType.Street), der Standardwert
- [`Satellite`](xref:Xamarin.Forms.Maps.MapType.Satellite)
- [`Hybrid`](xref:Xamarin.Forms.Maps.MapType.Hybrid)

Die [MapTypesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypesPage.xaml) zeigt, wie ein Optionsfeld zu verwenden, um den Map-Typ auszuwählen. Es nutzt die [ `RadioButtonManager` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioButtonManager.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) -Bibliothek und eine Klasse auf der Grundlage der [MapTypeRadioButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypeRadioButton.xaml) Datei.

### <a name="map-coordinates"></a>Kartenkoordinaten

Ein Programm erhalten dem aktuellen Bereich, der die `Map` angezeigt, über die [ `VisibleRegion` ](xref:Xamarin.Forms.Maps.Map.VisibleRegion) Eigenschaft. Diese Eigenschaft ist *nicht* unterstützt durch eine bindbare Eigenschaft, und es gibt keinen Benachrichtigungsmechanismus an, dass wenn es geändert wurde, so dass ein Programm, das die Eigenschaft überwachen möchte wahrscheinlich einen Timer zu diesem Zweck verwenden sollten.

`VisibleRegion` ist vom Typ [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan), eine Klasse mit vier schreibgeschützte Eigenschaften:

- [`Center`](xref:Xamarin.Forms.Maps.MapSpan.Center) Der Typ [`Position`](xref:Xamarin.Forms.Maps.Position)
- [`LatitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LatitudeDegrees) Der Typ `double`, der angibt, der der Höhe des angezeigten Bereich der Karte
- [`LongitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LongitudeDegrees) Der Typ `double`, der angibt, der Breite des angezeigten Bereich der Karte
- [`Radius`](xref:Xamarin.Forms.Maps.MapSpan.Radius) Der Typ [ `Distance` ](xref:Xamarin.Forms.Maps.Distance), der angibt, der Größe des größten runden Fläche auf der Karte angezeigt.

`Position` und `Distance` handelt es sich sowohl Strukturen. `Position` definiert zwei schreibgeschützte Eigenschaften, die festlegen, über die [ `Position` Konstruktor](xref:Xamarin.Forms.Maps.Position.%23ctor(System.Double,System.Double)):

- [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude)
- [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude)

`Distance` Richtet eine unabhängige Einheit Entfernung zu ermöglichen, indem Sie die Konvertierung zwischen Metrik und der englischen Einheit. Ein `Distance` Wert kann auf verschiedene Weise erstellt werden:

- [`Distance` Konstruktor](xref:Xamarin.Forms.Maps.Distance.%23ctor(System.Double)) mit einer Entfernung in Meter
- [`Distance.FromMeters`](xref:Xamarin.Forms.Maps.Distance.FromMeters(System.Double)) statische Methode
- [`Distance.FromKilometers`](xref:Xamarin.Forms.Maps.Distance.FromKilometers(System.Double)) statische Methode
- [`Distance.FromMiles`](xref:Xamarin.Forms.Maps.Distance.FromMiles(System.Double)) statische Methode

Der Wert ist in drei Eigenschaften verfügbar:

- [`Meters`](xref:Xamarin.Forms.Maps.Distance.Meters) Der Typ `double`
- [`Kilometers`](xref:Xamarin.Forms.Maps.Distance.Kilometers) Der Typ `double`
- [`Miles`](xref:Xamarin.Forms.Maps.Distance.Miles) Der Typ `double`

Die [MapCoordinatesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml) -Datei enthält mehrere `Label` Elemente zum Anzeigen der `MapSpan` Informationen. Die [MapCoordinatesPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml.cs) Code-Behind-Datei verwendet einen Timer, um die Informationen aktualisiert, wenn der Benutzer auf die Karte bearbeitet.

### <a name="position-extensions"></a>Position-Erweiterungen

Dieses Buch mit dem Namen einer neuen Bibliothek [ **Xamarin.FormsBook.Toolkit.Maps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit.Maps) kartenspezifische aber plattformunabhängige-Typen enthält. Die [ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) -Klasse verfügt über eine `ToString` -Methode für `Position`, und eine Methode, der Berechnung der Entfernung zwischen zwei `Position` Werte.

### <a name="setting-an-initial-location"></a>Festlegen einer anfänglichen Speicherorts

Rufen Sie die [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)) -Methode der `Map` programmgesteuert eine Speicherort und Zoom-Ebene auf der Karte festgelegt. Das Argument ist vom Typ `MapSpan`. Sie erstellen eine `MapSpan` -Objekt unter Verwendung eines der folgenden:

- [`MapSpan` Konstruktor](xref:Xamarin.Forms.Maps.MapSpan.%23ctor(Xamarin.Forms.Maps.Position,System.Double,System.Double)) mit einem `Position`, und Breiten- und Längengrad Span
- [`MapSpan.FromCenterAndRadius`](xref:Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius(Xamarin.Forms.Maps.Position,Xamarin.Forms.Maps.Distance)) mit einem `Position` und Radius

Es ist auch möglich, zum Erstellen eines neuen `MapSpan` aus einer vorhandenen mithilfe der Methoden [ `ClampLatitude` ](xref:Xamarin.Forms.Maps.MapSpan.ClampLatitude(System.Double,System.Double)) oder [ `WithZoom` ](xref:Xamarin.Forms.Maps.MapSpan.WithZoom(System.Double)).

Die [WyomingPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml) Datei und [WyomingPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml.cs) Code-Behind-Datei veranschaulicht, wie die `MoveToRegion` Methode, um den Status der Wyoming anzuzeigen.

Alternativ können Sie die [ `Map` Konstruktor](xref:Xamarin.Forms.Maps.Map.%23ctor(Xamarin.Forms.Maps.MapSpan)) mit einem `MapSpan` Objekt, das die Position der Zuordnung zu initialisieren. Die [XamarinHQPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/XamarinHQPage.xaml) zeigt, wie dazu vollständig in XAML, um Xamarin Hauptsitz in San Francisco anzuzeigen.

### <a name="dynamic-zooming"></a>Dynamische Zoomen

Sie können eine `Slider` dynamisch eine Zuordnung zu vergrößern. Die [RadiusZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml) Datei und [RadiusZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml.cs) Code-Behind-Datei zeigen, wie so ändern Sie den Radius einer Zuordnung basierend auf den `Slider` Wert.

Die [LongitudeZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml) Datei und [LongitudeZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml.cs) Code-Behind-Datei zeigen, eine alternative Methode, die unter Android besser funktioniert, aber keiner dieser Ansätze funktioniert gut auf die Windows Plattformen.

### <a name="the-phones-location"></a>Die des Standorts des Telefons

Die [ `IsShowingUser` ](xref:Xamarin.Forms.Maps.Map.IsShowingUser) Eigenschaft `Map` funktioniert etwas anders als auf jeder Plattform als der [ShowLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ShowLocationPage.xaml) Datei veranschaulicht:

- Unter iOS ein blauer Punkt gibt an, die des Standorts des Telefons, aber Sie müssen manuell es navigieren
- Unter Android, ein Symbol angezeigt wird, die bei mithilfe von Push übertragen wechselt die Karte, um die des Standorts des Telefons
- Die UWP ist ähnlich wie für iOS, aber manchmal automatisch navigiert zu der Position

Die **MapDemos** Projekt versucht, den Android-Ansatz zu imitieren, durch die erste Definition ein Symbol-basierte auf der Grundlage der [MyLocationButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml) Datei und [MyLocationButton.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml.cs) Code-Behind-Datei.

Die [GoToLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml) Datei und [GoToLocationPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml.cs) Code-Behind-Datei verwenden Sie diese Schaltfläche, um zu dem des Standorts des Telefons zu navigieren.

### <a name="pins-and-science-museums"></a>Pins und Wissenschaft Museen

Zum Schluss die `Map` -Klasse definiert eine [ `Pins` ](xref:Xamarin.Forms.Maps.Map.Pins) Eigenschaft vom Typ `IList<Pin>`. Die [ `Pin` ](xref:Xamarin.Forms.Maps.Pin) -Klasse definiert vier Eigenschaften:

- [`Label`](xref:Xamarin.Forms.Maps.Pin.Label) Der Typ `string`, eine erforderliche Eigenschaft
- [`Address`](xref:Xamarin.Forms.Maps.Pin.Address) Der Typ `string`, eine optionale lesbare Adresse
- [`Position`](xref:Xamarin.Forms.Maps.Pin.Position) Der Typ `Position`, der angibt, wo die Pin auf der Karte angezeigt wird
- [`Type`](xref:Xamarin.Forms.Maps.Pin.Type) Der Typ [ `PinType` ](xref:Xamarin.Forms.Maps.PinType), eine Enumeration, die nicht verwendet wird

Die **MapDemos** -Projekt enthält die Datei [ScienceMuseums.xml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Data/ScienceMuseums.xml), womit Science Museen in den Vereinigten Staaten, aufgelistet und [ `Locations` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Locations.cs) und [ `Site` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Site.cs) Klassen für diese Daten zu deserialisieren.

Die [ScienceMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml) Datei und [ScienceMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml.cs) CodeBehind-Datei-Anzeige-Pins für diese Museen Science, in der Zuordnung. Wenn der Benutzer eine Pin tippt, wird die Adresse und eine Website für das Museum angezeigt.

### <a name="the-distance-between-two-points"></a>Die Entfernung zwischen zwei Punkten

Die [ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) -Klasse enthält eine [ `DistanceTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs#L88) Methode mit einer vereinfachten Berechnung der Entfernung zwischen den beiden geografischen Standorten.

Hiermit wird die [LocalMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml) Datei und [LocalMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml.cs) Code-Behind-Datei auch die Entfernung Museum aus den Standort des Benutzers angezeigt werden sollen:

[![Dreifacher Screenshot der Seite für die lokale Museen](images/ch28fg28-small.png "Abstand an einem Speicherort")](images/ch28fg28-large.png#lightbox "Abstand an einem Ort")

Das Programm stellt auch die Anzahl der Pins, die basierend auf den Speicherort der Karte dynamisch zu beschränken.

## <a name="geocoding-and-back-again"></a>Geocodierung und wieder

Die [ **Xamarin.Forms.Maps** ](xref:Xamarin.Forms.Maps) Assembly enthält auch eine [ `Geocoder` ](xref:Xamarin.Forms.Maps.Geocoder) -Klasse mit einem [ `GetPositionsForAddressAsync` ](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync(System.String)) Methode, die konvertiert eine Textadresse in 0 (null) oder mehr mögliche geografischen Positionen und eine andere Methode [ `GetAddressesForPositionAsync` ](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync(Xamarin.Forms.Maps.Position)) , der in die andere Richtung konvertiert.

Die [GeocoderRoundTrip.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml) Datei und [GeocoderRoundTrip.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml.cs) Code-Behind-Datei führen Sie diese Funktion vor.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 28 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch28-Aug2016.pdf)
- [Kapitel 28-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)
- [Xamarin.Forms-Karte](~/xamarin-forms/user-interface/map.md)
