---
title: Healthkit in xamarin. IOS
description: In diesem Dokument wird healthkit beschrieben. dabei handelt es sich um ein in ios 8 eingeführte Framework, das einen zentralisierten, koordinierten und sicheren Datenspeicher für Integritäts bezogene Informationen bereitstellt. Es wird erläutert, wie eine healthkit-App bereitgestellt wird und wie Code geschrieben wird, der das healthkit-Framework verwendet.
ms.prod: xamarin
ms.assetid: E3927A21-507C-43BA-A2AD-957716BA9B52
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 0c733789883c9752d63824d0bca7356a88d05659
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86929661"
---
# <a name="healthkit-in-xamarinios"></a>Healthkit in xamarin. IOS

Health Kit bietet einen sicheren Datenspeicher für die Integritäts bezogenen Informationen des Benutzers. Health Kit-Apps können mit der expliziten Berechtigung des Benutzers Lese-und Schreibvorgänge für diesen Datenspeicher durch nehmen und Benachrichtigungen erhalten, wenn relevante Daten hinzugefügt werden. Apps können die Daten darstellen, oder Benutzer können die von Apple bereitgestellte Integritäts-App verwenden, um ein Dashboard aller Ihrer Daten anzuzeigen.

Da Integritäts bezogene Daten so sensibel und wichtig sind, ist das Integritäts-Kit stark typisiert, mit Maßeinheiten und einer expliziten Zuordnung zum Typ der Daten, die aufgezeichnet werden (z.: blutzuckergrad oder herzpreis). Außerdem müssen die Integritäts-Kit-apps explizite Berechtigungen verwenden, den Zugriff auf bestimmte Informationstypen anfordern, und der Benutzer muss der APP explizit Zugriff auf diese Datentypen erteilen.

In diesem Artikel wird Folgendes vorgestellt:

- Die Sicherheitsanforderungen des Health Kits, einschließlich der Anwendungs Bereitstellung und der Anforderung der Benutzer Berechtigung für den Zugriff auf die Health Kit-Datenbank;
- Der Typ System von Health Kit, wodurch die Wahrscheinlichkeit minimiert wird, dass Daten falsch angewendet oder fehlinterpretiert werden können.
- Schreiben in den freigegebenen, systemweiten Health Kit-Datenspeicher.

In diesem Artikel werden keine erweiterten Themen behandelt, wie z. b. das Abfragen der Datenbank, das Wechseln zwischen Maßeinheiten oder das Empfangen von Benachrichtigungen zu neuen Daten.

In diesem Artikel erstellen wir eine Beispielanwendung zum Aufzeichnen der Herzblut-Rate des Benutzers:

[![Eine Beispielanwendung zum Aufzeichnen der Taktfrequenz des Benutzers](healthkit-images/image01.png)](healthkit-images/image01.png#lightbox)

## <a name="requirements"></a>Anforderungen

Folgende Schritte sind erforderlich, um die in diesem Artikel beschriebenen Schritte auszuführen:

- **Xcode 7 und IOS 8 (oder höher)** – die neuesten Xcode-und IOS-APIs von Apple müssen auf dem Computer des Entwicklers installiert und konfiguriert werden.
- **Visual Studio für Mac oder Visual Studio** – die neueste Version von Visual Studio für Mac muss auf dem Computer des Entwicklers installiert und konfiguriert werden.
- **IOS 8-Gerät (oder höher)** – ein IOS-Gerät, auf dem die neueste Version von IOS 8 oder höher zum Testen ausgeführt wird.

> [!IMPORTANT]
> Health Kit wurde in ios 8 eingeführt. Derzeit ist das Health Kit im IOS-Simulator nicht verfügbar, und das Debuggen erfordert eine Verbindung mit einem physischen IOS-Gerät.

## <a name="creating-and-provisioning-a-health-kit-app"></a>Erstellen und Bereitstellen einer Health Kit-App
Bevor eine xamarin IOS 8-Anwendung die healthkit-API verwenden kann, muss sie ordnungsgemäß konfiguriert und bereitgestellt werden. In diesem Abschnitt werden die erforderlichen Schritte zum ordnungsgemäßen Einrichten der xamarin-Anwendung behandelt.

Health Kit-apps erfordern Folgendes:

- Eine explizite **App-ID**.
- Ein **Bereitstellungs Profil** , das mit dieser expliziten **App-ID** und den **Health Kit** -Berechtigungen verknüpft ist.
- Ein `Entitlements.plist` mit einer- `com.apple.developer.healthkit` Eigenschaft vom Typ `Boolean` , die auf festgelegt ist `Yes` .
- Ein `Info.plist` - `UIRequiredDeviceCapabilities` Wert, dessen Schlüssel einen Eintrag mit dem `String` Wert enthält `healthkit` .
- Der `Info.plist` muss auch über die entsprechenden Datenschutzbestimmungen verfügen: eine `String` Erläuterung für den Schlüssel `NSHealthUpdateUsageDescription` , wenn die APP Daten schreiben wird, und eine `String` Erläuterung für den Schlüssel `NSHealthShareUsageDescription` , wenn die APP Integritäts-Kit-Daten lesen wird.

Weitere Informationen zur Bereitstellung einer IOS-App finden Sie im Artikel zur [Geräte Bereitstellung](~/ios/get-started/installation/device-provisioning/index.md) in der Reihe " **Getting Started** " von xamarin beschreibt die Beziehung zwischen den Entwickler Zertifikaten, App-IDs, Bereitstellungs Profilen und App-Berechtigungen.

<a name="explicit-appid"></a>

### <a name="explicit-app-id-and-provisioning-profile"></a>Explizite APP-ID und Bereitstellungs Profil

Die Erstellung einer expliziten **App-ID** und eines geeigneten **Bereitstellungs Profils** erfolgt im [IOS dev Center](https://developer.apple.com/devcenter/ios/index.action)von Apple. 

Ihre aktuellen **App-IDs** sind im Abschnitt [Zertifikate, Bezeichner & profile](https://developer.apple.com/account/ios/identifiers/bundle/bundleList.action) des dev Centers aufgeführt. Häufig werden in dieser Liste die **ID** -Werte von angezeigt `*` , die angeben, dass der Name der **App-ID**  -  **Name** mit einer beliebigen Anzahl von Suffixen verwendet werden kann. Solche Platzhalter- *App-IDs* können nicht mit Health Kit verwendet werden.

Um eine explizite **App-ID**zu erstellen, klicken Sie auf die **+** Schaltfläche in der oberen rechten Ecke, um zur Seite **IOS-APP-ID registrieren** zu gelangen:

[![Registrieren einer APP im Apple-Entwickler Portal](healthkit-images/image02.png)](healthkit-images/image02.png#lightbox)

Verwenden Sie nach dem Erstellen einer APP-Beschreibung den Abschnitt **explizite APP-ID** , um eine ID für Ihre Anwendung zu erstellen. Aktivieren Sie im Abschnitt " **App Services** " im Abschnitt **Dienste aktivieren** den **integritätskit** .

Wenn Sie dies abgeschlossen haben, klicken Sie auf die Schaltfläche **weiter** , um die **App-ID** in Ihrem Konto zu registrieren. Sie werden auf die Seite **Zertifikate, Bezeichner und Profile** zurückgeführt. Klicken Sie auf **Bereitstellungs profile** , um zur Liste Ihrer aktuellen Bereitstellungs Profile zu gelangen, und klicken Sie auf die **+** Schaltfläche in der oberen rechten Ecke, um zur Seite **IOS-Bereitstellungs Profil hinzufügen** zu gelangen. Wählen Sie die Option **IOS App Development** aus, und klicken Sie auf **weiter** , um zur Seite **App-ID auswählen** zu gelangen. Wählen Sie hier die explizite **App-ID** aus, die Sie zuvor angegeben haben:

[![Auswählen der expliziten APP-ID](healthkit-images/image03.png)](healthkit-images/image03.png#lightbox)

Klicken Sie auf **weiter** , und arbeiten Sie die verbleibenden Bildschirme durch, wobei Sie die **Entwickler Zertifikate**, **Geräte**und einen **Namen** für dieses **Bereitstellungs Profil**angeben:

[![Das Bereitstellungs Profil wird erstellt.](healthkit-images/image04.png)](healthkit-images/image04.png#lightbox)

Klicken Sie auf **generieren** , und warten Sie auf die Erstellung Ihres Profils. Laden Sie die Datei herunter, und doppelklicken Sie darauf, um Sie in Xcode zu installieren. Sie können überprüfen, ob die Installation unter **Xcode > Einstellungen > Konten > Details anzeigen...** Das soeben installierte Bereitstellungs Profil sollte angezeigt werden, und es sollte das Symbol für das Integritäts-Kit und alle anderen speziellen Dienste in **der Berechtigungs Zeile enthalten** :

[![Anzeigen des Profils in Xcode](healthkit-images/image05.png)](healthkit-images/image05.png#lightbox)

<a name="associating-appid"></a>

### <a name="associating-the-app-id-and-provisioning-profile-with-your-xamarinios-app"></a>Zuordnen der APP-ID und des Bereitstellungs Profils zu ihrer xamarin. IOS-App

Nachdem Sie ein entsprechendes **Bereitstellungs Profil** wie beschrieben erstellt und installiert haben, wäre es in der Regel an der Zeit, eine Projekt Mappe in Visual Studio für Mac oder Visual Studio zu erstellen. Der Integritäts-Kit-Zugriff ist für alle IOS c#-oder F #-Projekte verfügbar.

Anstatt den Prozess der Erstellung eines xamarin IOS 8-Projekts per Hand zu durchlaufen, öffnen Sie die Beispiel-APP, die mit diesem Artikel verknüpft ist (einschließlich eines vorgefertigten Storyboards und Codes). Klicken Sie im **Lösungspad**mit der rechten Maustaste auf das Projekt, und klicken Sie dann auf das Dialogfeld **Optionen** , um die Beispiel-App dem Integritäts-Kit-fähigen **Bereitstellungs Profil**zuzuordnen. Wechseln Sie zum Bereich **IOS-Anwendung** , und geben Sie die explizite APP-ID ein, die Sie zuvor erstellt haben, als **Bündel**- **ID** der APP

[![Explizite APP-ID eingeben](healthkit-images/image06.png)](healthkit-images/image06.png#lightbox)

Wechseln Sie jetzt zum **IOS-Bündel-Signierungs** Panel. Ihr kürzlich installiertes **Bereitstellungs Profil**mit seiner Zuordnung zur expliziten **App-ID**ist nun als **Bereitstellungs Profil**verfügbar:

[![Wählen Sie das Bereitstellungs Profil aus.](healthkit-images/image07.png)](healthkit-images/image07.png#lightbox)

Wenn das **Bereitstellungs Profil** nicht verfügbar ist, überprüfen Sie die **Bündel** -ID im **IOS-Anwendungs** Panel im Vergleich zu den **im IOS dev Center** angegebenen Einstellungen, und das **Bereitstellungs Profil** ist installiert (**Xcode > Einstellungen > Konten > Details anzeigen...**).

Wenn das Integritäts-Kit-aktivierte **Bereitstellungs Profil** ausgewählt ist, klicken Sie auf **OK** , um das Dialogfeld Projektoptionen zu schließen.

### <a name="entitlementsplist-and-infoplist-values"></a>Werte der Berechtigungen. plist und Info. plist

Die Beispiel-app enthält eine `Entitlements.plist` -Datei (die für Integritäts-Kit-aktivierte apps erforderlich ist) und nicht in jeder Projektvorlage enthalten. Wenn das Projekt keine Berechtigungen enthält, klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **Datei > neue Datei... > IOS > Berechtigungen. plist** aus, um eine manuell hinzuzufügen.

Letztendlich muss Ihr `Entitlements.plist` über das folgende Schlüssel-Wert-Paar verfügen:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.developer.HealthKit</key>
    <true/>
</dict>
</plist>

```

Ebenso muss der `Info.plist` für die APP über den Wert verfügen, der `healthkit` dem Schlüssel zugeordnet ist `UIRequiredDeviceCapabilities` :

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
<string>armv7</string>
    <string>healthkit</string>
</array>

```

Die in diesem Artikel bereitgestellte Beispielanwendung enthält eine vorkonfigurierte `Entitlements.plist` , die alle erforderlichen Schlüssel enthält.

<a name="programming"></a>

## <a name="programming-health-kit"></a>Programmieren von Health Kit

Der Integritäts-Kit-Datenspeicher ist ein privater, Benutzer spezifischer Datenspeicher, der von den apps gemeinsam genutzt wird. Da Integritäts Informationen so sensibel sind, muss der Benutzer positive Schritte ausführen, um den Datenzugriff zu ermöglichen. Dieser Zugriff ist möglicherweise partiell (Schreibzugriff, aber nicht gelesen, Zugriff für einige Arten von Daten, jedoch nicht für andere usw.) und kann jederzeit widerrufen werden. Integritäts-Kit-Anwendungen sollten in der Regel geschrieben werden, und es ist zu beachten, dass viele Benutzer daran zögern werden, ihre Integritäts bezogenen Informationen zu speichern.

Health Kit-Daten sind auf angegebene Apple-Typen beschränkt. Diese Typen sind streng definiert: einige, wie z. b. "bluttyp", sind auf die besonderen Werte einer von Apple bereitgestellten Enumeration beschränkt, während andere eine Größe mit einer Maßeinheit (z. b. grams, Kalorien und Litern) kombinieren. Selbst Daten, die eine kompatible Maßeinheit gemeinsam nutzen, werden durch ihre unterschieden `HKObjectType` . beispielsweise fängt das Typsystem einen falschen Versuch, einen `HKQuantityTypeIdentifier.NumberOfTimesFallen` Wert in einem Feld zu speichern, das einen erwartet, `HKQuantityTypeIdentifier.FlightsClimbed` obwohl beide die `HKUnit.Count` Maßeinheit verwenden.

Die Typen, die im Health Kit-Datenspeicher gespeichert werden können, sind alle Unterklassen von `HKObjectType` . `HKCharacteristicType`Objekte speichern das biologische Geschlecht, den bluttyp und das Geburtsdatum. Häufiger sind jedoch- `HKSampleType` Objekte, die Daten darstellen, die zu einem bestimmten Zeitpunkt oder über einen bestimmten Zeitpunkt erfasst werden. 

[![Hksampletype-Objekt Diagramm](healthkit-images/image08.png)](healthkit-images/image08.png#lightbox)

`HKSampleType`ist abstrakt und verfügt über vier konkrete Unterklassen. Zurzeit gibt es nur einen Datentyp, bei dem es sich `HKCategoryType` um eine standbyanalyse handelt. Die große Mehrheit der Daten im Health Kit ist vom Typ `HKQuantityType` und speichert Ihre Daten in `HKQuantitySample` Objekten, die mit dem vertrauten Factoryentwurfsmuster erstellt werden:

[![Die große Mehrheit der Daten im Health Kit ist vom Typ "hkquantitytype" und speichert Ihre Daten in hkquantitysample-Objekten.](healthkit-images/image09.png)](healthkit-images/image09.png#lightbox)

`HKQuantityType`die Typen reichen von `HKQuantityTypeIdentifier.ActiveEnergyBurned` bis `HKQuantityTypeIdentifier.StepCount` . 

<a name="requesting-permission"></a>

### <a name="requesting-permission-from-the-user"></a>Anfordern der Berechtigung für den Benutzer

Endbenutzer müssen positive Schritte durchführen, um einer APP das Lesen oder Schreiben von Integritäts-Kit-Daten zu ermöglichen. Dies erfolgt über die Integritäts-APP, die auf IOS 8-Geräten vorinstalliert ist. Wenn eine Integritäts-Kit-App zum ersten Mal ausgeführt wird, wird dem Benutzer ein vom System GESTEUERTER Integritäts **Zugriffs** Dialogfeld angezeigt:

[![Dem Benutzer wird ein vom System GESTEUERTER Integritäts Zugriffs Dialogfeld angezeigt.](healthkit-images/image10.png)](healthkit-images/image10.png#lightbox)

Der Benutzer kann die Berechtigungen später mithilfe des Dialog Felds " **Quellen** " der Integritäts-App ändern:

[![Der Benutzer kann Berechtigungen mithilfe des Dialog Felds "Integritäts-Apps"](healthkit-images/image11.png)](healthkit-images/image11.png#lightbox)

Da Integritäts Informationen äußerst sensibel sind, sollten App-Entwickler ihre Programme defensiv schreiben, wobei die Annahme besteht, dass die Berechtigungen während der Ausführung der APP verweigert und geändert werden. Das häufigste Verfahren besteht darin, Berechtigungen in der `UIApplicationDelegate.OnActivated` -Methode anzufordern und dann die Benutzeroberfläche nach Bedarf zu ändern.

### <a name="permissions-walkthrough"></a>Durchgängige Berechtigungen

Öffnen Sie die Datei in Ihrem von Health Kit bereitgestellten Projekt `AppDelegate.cs` . Beachten Sie die-Anweisung, die verwendet `HealthKit` ; am Anfang der Datei.

Der folgende Code bezieht sich auf Health Kit-Berechtigungen:

```csharp
private HKHealthStore healthKitStore = new HKHealthStore ();

public override void OnActivated (UIApplication application)
{
        ValidateAuthorization ();
}

private void ValidateAuthorization ()
{
        var heartRateId = HKQuantityTypeIdentifierKey.HeartRate;
        var heartRateType = HKObjectType.GetQuantityType (heartRateId);
        var typesToWrite = new NSSet (new [] { heartRateType });
        var typesToRead = new NSSet ();
        healthKitStore.RequestAuthorizationToShare (
                typesToWrite, 
                typesToRead, 
                ReactToHealthCarePermissions);
}

void ReactToHealthCarePermissions (bool success, NSError error)
{
        var access = healthKitStore.GetAuthorizationStatus (HKObjectType.GetQuantityType (HKQuantityTypeIdentifierKey.HeartRate));
        if (access.HasFlag (HKAuthorizationStatus.SharingAuthorized)) {
                HeartRateModel.Instance.Enabled = true;
        } else {
                HeartRateModel.Instance.Enabled = false;
        }
}

```

Der gesamte Code in diesen Methoden kann Inline in ausgeführt werden `OnActivated` , aber die Beispiel-App verwendet separate Methoden, um ihre Absicht übersichtlicher zu machen: `ValidateAuthorization()` verfügt über die erforderlichen Schritte, um den Zugriff auf die spezifischen Typen anzufordern, die geschrieben werden (und lesen, wenn die APP gewünscht ist) `ReactToHealthCarePermissions()`

Der Auftrag von `ValidateAuthorization()` besteht darin, den Satz von zu erstellen `HKObjectTypes` , den die app schreiben wird, und die Autorisierung zum Aktualisieren dieser Daten anzufordern. In der Beispiel-APP `HKObjectType` ist der für den Schlüssel `KHQuantityTypeIdentifierKey.HeartRate` . Dieser Typ wird dem Satz hinzugefügt `typesToWrite` , während der Satz `typesToRead` leer bleibt. Diese Sätze und ein Verweis auf den `ReactToHealthCarePermissions()` Rückruf werden an den-Wert übermittelt `HKHealthStore.RequestAuthorizationToShare()` .

Der `ReactToHealthCarePermissions()` Rückruf wird aufgerufen, nachdem der Benutzer mit dem Berechtigungs Dialogfeld interagiert hat und zwei Informationen an Sie übermittelt haben: einen `bool` Wert, der ist, `true` Wenn der Benutzer mit dem Berechtigungs Dialogfeld interagiert hat, und ein `NSError` , der bei nicht-NULL einen Fehler angibt, der mit der Darstellung des Berechtigungs Dialogfelds verknüpft ist.

> [!IMPORTANT]
> Um die Argumente für diese Funktion zu verdeutlichen: die _Erfolgs_ -und _Fehler_ Parameter geben nicht an, ob der Benutzer die Berechtigung zum Zugreifen auf Health Kit-Daten erteilt hat. Sie geben lediglich an, dass der Benutzer die Möglichkeit erhält, den Zugriff auf die Daten zuzulassen.

Um zu bestätigen, ob die APP Zugriff auf die Daten hat, `HKHealthStore.GetAuthorizationStatus()` wird verwendet und übergibt `HKQuantityTypeIdentifierKey.HeartRate` . Basierend auf dem zurückgegebenen Status aktiviert oder deaktiviert die APP die Möglichkeit, Daten einzugeben. Es gibt keine Standardbenutzer Funktion für den Umgang mit Zugriffs Verweigerung, und es gibt viele mögliche Optionen. In der Beispiel-APP wird der Status für ein Singleton-Objekt festgelegt, `HeartRateModel` das wiederum relevante Ereignisse auslöst.

## <a name="model-view-and-controller"></a>Modell, Ansicht und Controller

`HeartRateModel`Öffnen Sie die Datei, um das Singleton-Objekt zu überprüfen `HeartRateModel.cs` :

```csharp
using System;
using HealthKit;
using Foundation;

namespace HKWork
{
        public class GenericEventArgs<T> : EventArgs
        {
                public T Value { get; protected set; }
                public DateTime Time { get; protected set; }

                public GenericEventArgs (T value)
                {
                        this.Value = value;
                        Time = DateTime.Now;
                }
        }

        public delegate void GenericEventHandler<T> (object sender,GenericEventArgs<T> args);

        public sealed class HeartRateModel : NSObject
        {
                private static volatile HeartRateModel singleton;
                private static object syncRoot = new Object ();

                private HeartRateModel ()
                {
                }

                public static HeartRateModel Instance {
                        get {
                                //Double-check lazy initialization
                                if (singleton == null) {
                                        lock (syncRoot) {
                                                if (singleton == null) {
                                                        singleton = new HeartRateModel ();
                                                }
                                        }
                                }

                                return singleton;
                        }
                }

                private bool enabled = false;

                public event GenericEventHandler<bool> EnabledChanged;
                public event GenericEventHandler<String> ErrorMessageChanged;
                public event GenericEventHandler<Double> HeartRateStored;

                public bool Enabled { 
                        get { return enabled; }
                        set {
                                if (enabled != value) {
                                        enabled = value;
                                        InvokeOnMainThread(() => EnabledChanged (this, new GenericEventArgs<bool>(value)));
                                }
                        }
                }

                public void PermissionsError(string msg)
                {
                        Enabled = false;
                        InvokeOnMainThread(() => ErrorMessageChanged (this, new GenericEventArgs<string>(msg)));
                }

                //Converts its argument into a strongly-typed quantity representing the value in beats-per-minute
                public HKQuantity HeartRateInBeatsPerMinute(ushort beatsPerMinute)
                {
                        var heartRateUnitType = HKUnit.Count.UnitDividedBy (HKUnit.Minute);
                        var quantity = HKQuantity.FromQuantity (heartRateUnitType, beatsPerMinute);

                        return quantity;
                }
                        
                public void StoreHeartRate(HKQuantity quantity)
                {
                        var bpm = HKUnit.Count.UnitDividedBy (HKUnit.Minute);
                        //Confirm that the value passed in is of a valid type (can be converted to beats-per-minute)
                        if (! quantity.IsCompatible(bpm))
                        {
                                InvokeOnMainThread(() => ErrorMessageChanged(this, new GenericEventArgs<string> ("Units must be compatible with BPM")));
                        }

                        var heartRateId = HKQuantityTypeIdentifierKey.HeartRate;
                        var heartRateQuantityType = HKQuantityType.GetQuantityType (heartRateId);
                        var heartRateSample = HKQuantitySample.FromType (heartRateQuantityType, quantity, new NSDate (), new NSDate (), new HKMetadata());

                        using (var healthKitStore = new HKHealthStore ()) {
                                healthKitStore.SaveObject (heartRateSample, (success, error) => {
                                        InvokeOnMainThread (() => {
                                                if (success) {
                                                        HeartRateStored(this, new GenericEventArgs<Double>(quantity.GetDoubleValue(bpm)));
                                                } else {
                                                        ErrorMessageChanged(this, new GenericEventArgs<string>("Save failed"));
                                                }
                                                if (error != null) {
                                                        //If there's some kind of error, disable 
                                                        Enabled = false;
                                                        ErrorMessageChanged (this, new GenericEventArgs<string>(error.ToString()));
                                                }
                                        });
                                });
                        }
                }
        }
}

```

Der erste Abschnitt ist Code Bausteine zum Erstellen generischer Ereignisse und Handler. Der erste Teil der `HeartRateModel` Klasse ist auch ein Baustein für die Erstellung eines Thread sicheren Singleton-Objekts.

Anschließend werden `HeartRateModel` drei Ereignisse verfügbar gemacht: 

- `EnabledChanged`: Gibt an, dass die Speicher Raten Speicherung aktiviert oder deaktiviert wurde (Beachten Sie, dass der Speicher anfänglich deaktiviert ist). 
- `ErrorMessageChanged`Für diese Beispiel-App gibt es ein sehr einfaches Fehler Behandlungsmodell: eine Zeichenfolge mit dem letzten Fehler. 
- `HeartRateStored`: Wird ausgelöst, wenn eine Herzrate in der Health Kit-Datenbank gespeichert wird.

Beachten Sie, dass jedes Mal, wenn diese Ereignisse ausgelöst werden, über erstellt wird `NSObject.InvokeOnMainThread()` . Dadurch können Abonnenten die Benutzeroberfläche aktualisieren. Alternativ können die Ereignisse so dokumentiert werden, dass Sie für Hintergrundthreads ausgelöst werden, und die Verantwortung für die Sicherstellung, dass die Kompatibilität für ihre Handler verbleibt. Thread Überlegungen sind in Health Kit-Anwendungen wichtig, da viele der Funktionen, z. b. die Berechtigungs Anforderung, asynchron sind und ihre Rückrufe für nicht-Hauptthreads ausführen.

Der für den Integritäts-Kit spezifische Code in `HeartRateModel` ist in den beiden Funktionen `HeartRateInBeatsPerMinute()` und `StoreHeartRate()` . 

`HeartRateInBeatsPerMinute()`Konvertiert das Argument in ein stark typisiertes Integritäts-Kit `HKQuantity` . Der Typ der Menge ist das, das von angegeben wird, `HKQuantityTypeIdentifierKey.HeartRate` und die Einheiten der Menge werden `HKUnit.Count` durch dividiert `HKUnit.Minute` (d. h., die Einheit ist " *Beats" pro Minute*). 

Die- `StoreHeartRate()` Funktion nimmt eine `HKQuantity` (in der Beispiel-APP, die von erstellt wurde `HeartRateInBeatsPerMinute()` ). Um die Daten zu überprüfen, wird die `HKQuantity.IsCompatible()` -Methode verwendet, die zurückgibt, `true` Wenn die Einheiten des Objekts in die Einheiten im-Argument konvertiert werden können. Wenn die Menge mit erstellt wurde `HeartRateInBeatsPerMinute()` , wird offensichtlich zurückgegeben `true` , aber es würde auch zurückgeben, `true` Wenn die Menge erstellt wurde, z.b. *Beats pro Stunde*. Üblicherweise `HKQuantity.IsCompatible()` kann verwendet werden, um Massen-, Entfernungs-und Energiewerte zu überprüfen, die der Benutzer oder ein Gerät in einem Mess Gerät (z. b. als kaiserliche Einheiten) eingeben oder anzeigen kann, das jedoch in einem anderen System (z. b. metrikeinheiten) gespeichert werden kann. 

Nachdem die Kompatibilität der Menge überprüft wurde, `HKQuantitySample.FromType()` wird die Factory-Methode verwendet, um ein stark typisiertes Objekt zu erstellen `heartRateSample` . `HKSample`-Objekte verfügen über ein Start-und Enddatum. bei sofortigen Messungen sollten diese Werte identisch sein, wie Sie im Beispiel dargestellt werden. Im Beispiel werden auch keine Schlüssel-Wert-Daten im-Argument festgelegt `HKMetadata` , aber Sie können Code wie den folgenden Code verwenden, um den Sensor Speicherort anzugeben:

```csharp
var hkm = new HKMetadata();
hkm.HeartRateSensorLocation = HKHeartRateSensorLocation.Chest;

```

Nachdem der `heartRateSample` erstellt wurde, erstellt der Code mit dem using-Block eine neue Verbindung mit der Datenbank. Innerhalb dieses Blocks versucht die- `HKHealthStore.SaveObject()` Methode den asynchronen Schreibzugriff auf die Datenbank. Der resultierende aufrufsausdruck für den Lambda-Ausdruck löst relevante Ereignisse aus, entweder `HeartRateStored` oder `ErrorMessageChanged` .

Nachdem das Modell programmiert wurde, ist es Zeit, zu sehen, wie der Controller den Zustand des Modells widerspiegelt. Öffnen Sie die Datei `HKWorkViewController.cs`. Der Konstruktor verbindet einfach den `HeartRateModel` Singleton mit Ereignis Behandlungsmethoden (Dies kann wiederum Inline mit Lambda-Ausdrücken erfolgen, aber separate Methoden machen die Absicht etwas deutlicher):

```csharp
public HKWorkViewController (IntPtr handle) : base (handle)
{
     HeartRateModel.Instance.EnabledChanged += OnEnabledChanged;
     HeartRateModel.Instance.ErrorMessageChanged += OnErrorMessageChanged;
     HeartRateModel.Instance.HeartRateStored += OnHeartBeatStored;
}

```

Im folgenden sind die relevanten Handler aufgeführt:

```csharp
void OnEnabledChanged (object sender, GenericEventArgs<bool> args)
{
        StoreData.Enabled = args.Value;
        PermissionsLabel.Text = args.Value ? "Ready to record" : "Not authorized to store data.";
        PermissionsLabel.SizeToFit ();
}

void OnErrorMessageChanged (object sender, GenericEventArgs<string> args)
{
        PermissionsLabel.Text = args.Value;
}

void OnHeartBeatStored (object sender, GenericEventArgs<double> args)
{
        PermissionsLabel.Text = String.Format ("Stored {0} BPM", args.Value);
}

```

Natürlich wäre es in einer Anwendung mit einem einzelnen Controller möglich, das Erstellen eines separaten Modell Objekts und die Verwendung von Ereignissen für die Ablauf Steuerung zu vermeiden, aber die Verwendung von Modell Objekten ist für reale apps besser geeignet.

## <a name="running-the-sample-app"></a>Ausführen der Beispiel-App

Der IOS-Simulator bietet keine Unterstützung für das Health Kit. Das Debuggen muss auf einem physischen Gerät erfolgen, auf dem IOS 8 ausgeführt wird.

Fügen Sie ein ordnungsgemäß bereitgestelltes IOS 8-Entwicklungs Gerät an Ihr System an. Wählen Sie ihn in Visual Studio für Mac als Bereitstellungs Ziel aus, und wählen Sie im Menü **> Debuggen ausführen**aus.

> [!IMPORTANT]
> An dieser Stelle werden Fehler im Zusammenhang mit der Bereitstellung auftreten. Weitere Informationen zum Beheben von Fehlern finden Sie im Abschnitt erstellen und Bereitstellen einer Integritäts-Kit-app. Die Komponenten sind: 
>
> - **IOS dev Center** : explizite APP-ID & Integritäts-Kit-fähiges Bereitstellungs Profil. 
> - **Projektoptionen** : Bündel Bezeichner (explizite APP-ID) & Bereitstellungs Profil.
> - **Quellcode** -Berechtigungen. plist & Info. plist

Wenn Sie davon ausgehen, dass die bereit Stellungen ordnungsgemäß eingerichtet wurden, wird die Anwendung gestartet. Wenn die Methode erreicht wird, fordert Sie die Integritäts- `OnActivated` Kit-Autorisierung an. Wenn das Betriebssystem das erste Mal erreicht hat, wird dem Benutzer das folgende Dialogfeld angezeigt:

[![Dem Benutzer wird dieses Dialogfeld angezeigt.](healthkit-images/image12.png)](healthkit-images/image12.png#lightbox)

Aktivieren Sie Ihre APP, um Herzfrequenz Daten zu aktualisieren, und Ihre APP wird erneut angezeigt. Der `ReactToHealthCarePermissions` Rückruf wird asynchron aktiviert. Dies bewirkt, dass die- `HeartRateModel’s` `Enabled` Eigenschaft geändert wird. Dadurch wird das- `EnabledChanged` Ereignis ausgelöst, wodurch der `HKPermissionsViewController.OnEnabledChanged()` Ereignishandler ausgeführt wird, wodurch die `StoreData` Schaltfläche aktiviert wird. Das folgende Diagramm zeigt die Sequenz:

[![Dieses Diagramm zeigt die Abfolge von Ereignissen.](healthkit-images/image13.png)](healthkit-images/image13.png#lightbox)

Klicken Sie auf die Schaltfläche **Datensatz** . Dies bewirkt `StoreData_TouchUpInside()` , dass der Handler ausgeführt wird, der versucht, den Wert des `heartRate` Textfelds zu analysieren, in einen `HKQuantity` über die zuvor erörterte Funktion zu konvertieren `HeartRateModel.HeartRateInBeatsPerMinute()` und diese Menge an zu übergeben `HeartRateModel.StoreHeartRate()` . Wie bereits erläutert, wird versucht, die Daten zu speichern, und es wird entweder ein- `HeartRateStored` Ereignis oder ein- `ErrorMessageChanged` Ereignis erhoben.

Doppelklicken Sie auf Ihrem Gerät auf die Schaltfläche **Start** , und öffnen Sie die Integritäts-App Wenn Sie auf die Registerkarte **Quellen** klicken, wird die Beispiel-App aufgelistet. Wählen Sie diese Option aus, und erteilen Sie die Berechtigung zum Aktualisieren von Kernsatz Daten. Doppelklicken Sie auf die Schaltfläche **Start** , und wechseln Sie zurück zu Ihrer APP. Auch hier wird `ReactToHealthCarePermissions()` aufgerufen, aber dieses Mal wird die Schaltfläche **StoreData** deaktiviert, da der Zugriff verweigert wird (Beachten Sie, dass dies asynchron erfolgt und die Änderung in der Benutzeroberfläche für den Endbenutzer sichtbar sein kann).

## <a name="advanced-topics"></a>Weiterführende Themen

Das Lesen von Daten aus der Health Kit-Datenbank ähnelt dem Schreiben von Daten: eine gibt die Typen der Daten an, auf die zugegriffen werden soll, die Autorisierung anfordern, und wenn diese Autorisierung erteilt wird, sind die Daten verfügbar, wobei die automatische Konvertierung in kompatible Maßeinheiten erfolgt.

Es gibt eine Reihe von anspruchsvolleren Abfragefunktionen, die Prädikat basierte Abfragen und Abfragen ermöglichen, die Aktualisierungen ausführen, wenn relevante Daten aktualisiert werden. 

Entwickler von Health Kit-Anwendungen sollten den Abschnitt Health Kit der APP- [Überprüfungs Richtlinien](https://developer.apple.com/app-store/review/guidelines/#healthkit)von Apple lesen.

Nachdem Sie die Sicherheits-und Typsystem Modelle verstanden haben, ist das Speichern und Lesen von Daten in der Shared Health Kit-Datenbank recht unkompliziert. Viele der Funktionen in Health Kit werden asynchron ausgeführt, und Anwendungsentwickler müssen ihre Programme entsprechend schreiben.

Zum Zeitpunkt der Erstellung dieses Artikels gibt es derzeit keine Entsprechung für das Health Kit in Android oder Windows phone.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie das Integritäts-Kit ermöglicht, dass Anwendungen Integritäts bezogene Informationen speichern, abrufen und freigeben können, während gleichzeitig eine standardmäßige Integritäts-App bereitgestellt wird, die dem Benutzer den Zugriff auf diese Daten ermöglicht. 

Wir haben auch gesehen, wie Datenschutz, Sicherheit und Datenintegrität Probleme für Integritäts bezogene Informationen überschreiben, und apps, die das Health Kit verwenden, müssen die größere Komplexität in Bezug auf die Anwendungs Verwaltung (Bereitstellung), das Codieren (Typsystem von Health Kit) und die Benutzererfahrung (Benutzersteuerung von Berechtigungen über System Dialogfelder und die Integritäts-APP) behandeln. 

Zum Schluss haben wir eine einfache Implementierung des Integritäts Kits mithilfe der enthaltenen Beispiel-App durchsucht, die Takt Daten in den Integritäts-Kit-Speicher schreibt und einen asynchronen Designbereich aufweist.

## <a name="related-links"></a>Verwandte Links

- [Hkwork (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-introtohealthkit)
- [Einführung in iOS 8](~/ios/platform/introduction-to-ios8.md)
