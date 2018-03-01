---
title: HealthKit
description: "HealthKit ist ein Framework, eingeführt in iOS 8, die einen zentralisierte, koordinierten und sicheren Datenspeicher für Integrität bezogene Informationen bereitstellt. Das Betriebssystem wird sichergestellt, dass der Datenschutz und Sicherheit der Zustandsinformationen und mit der Integrität-app ein Dashboard für den Benutzer. Mit der Berechtigung des Benutzers können Anwendungen lesen und Schreiben von einer Vielzahl von Zustandsinformationen."
ms.topic: article
ms.prod: xamarin
ms.assetid: E3927A21-507C-43BA-A2AD-957716BA9B52
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 50684d82726a398aabe77d09ff62eac40e277f02
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="healthkit"></a>HealthKit

_HealthKit ist ein Framework, eingeführt in iOS 8, die einen zentralisierte, koordinierten und sicheren Datenspeicher für Integrität bezogene Informationen bereitstellt. Das Betriebssystem wird sichergestellt, dass der Datenschutz und Sicherheit der Zustandsinformationen und mit der Integrität-app ein Dashboard für den Benutzer. Mit der Berechtigung des Benutzers können Anwendungen lesen und Schreiben von einer Vielzahl von Zustandsinformationen._

Integrität Kit bietet einen sicheren Datenspeicher für Integritätsdienst-bezogene Informationen des Benutzers. Integrität Kit apps können mit explizite Berechtigung des Benutzers, lesen und Schreiben in diese Datenspeicher und Benachrichtigungen erhalten, wenn die relevante Daten hinzugefügt werden. Apps können die Daten darstellen, oder des Benutzers kann von Apple bereitgestellten Integrität app verwenden, um ein Dashboard alle ihre Daten anzuzeigen.

Da Integrität produktbezogene Daten daher vertrauliche und entscheidend, Integrität Kit stark typisiert ist, mit Einheiten und eine explizite Zuordnung mit dem Typ der aufgezeichneten Informationen (z. B. Blut Glucose Ebene oder Herzfrequenz). Darüber hinaus Health-Kit-apps müssen explizite Berechtigungen verwenden, muss anfordern des Zugriffs auf bestimmten Typen von Informationen, und der Benutzer muss explizit der appzugriff gewähren den Typen von Daten.

In diesem Artikel werden vorgestellt:

- Integritäts-Kit-sicherheitsanforderungen, einschließlich Bereitstellung von Anwendungen und Benutzer die Berechtigung zum Zugriff auf die Datenbank Integrität Kit anfordern;
- Integritäts-Kit-Typsystem, das minimiert die Möglichkeit, falsch anwenden oder Daten Formatelementen;
- Schreiben in den freigegebenen, systemweite Health-Kit-Datenspeicher.

In diesem Artikel wird nicht mehr erweiterte Themen, z. B. Abfragen der Datenbank, Konvertieren zwischen Maßeinheiten oder Empfangen von Benachrichtigungen zu neuen Daten behandelt.

In diesem Artikel erstellen wir eine beispielanwendung, die zum Aufzeichnen des Benutzers Herzfrequenz von werden:

[![](healthkit-images/image01.png "Eine beispielanwendung, die Benutzer Herzfrequenz aufzeichnen")](healthkit-images/image01.png)

## <a name="requirements"></a>Anforderungen

Im folgenden sind erforderlich, die in diesem Artikel vorgestellten Schritte ausführen:

- **Xcode 7 und iOS 8 (oder höher)** – Apple neuesten Xcode und iOS-APIs installiert und auf dem Computer des Entwicklers konfiguriert werden müssen.
- **Visual Studio für Mac oder Visual Studio** – die neueste Version von Visual Studio für Mac installiert und auf dem Computer des Entwicklers konfiguriert werden.
- **iOS 8 (oder höher) Gerät** – ein iOS-Gerät, das die neueste Version von iOS ausgeführt wird, 8 oder höher zum Testen.

> [!IMPORTANT]
> **Hinweis:** Health Kit iOS 8 eingeführt wurde. Aktuell, Integrität Kit ist nicht verfügbar, auf dem iOS-Simulator, und Debuggen ist die Verbindung mit einem physischen iOS-Gerät erforderlich.




## <a name="creating-and-provisioning-a-health-kit-app"></a>Erstellen und eine Health-Kit-App-Bereitstellung
Bevor eine Xamarin iOS 8-Anwendung die HealthKit-API verwenden kann, müssen ordnungsgemäß konfiguriert und bereitgestellt werden. In diesem Abschnitt werden die erforderlichen Schritte zum setup für die Xamarin-Anwendung ordnungsgemäß behandelt.

Integrität Kit apps erfordern:

- Eine explizite **App-ID**.
- Ein **Bereitstellungsprofil** zugeordnet sind, explizite **App-ID** und mit **Health Kit** Berechtigungen.
- Ein `Entitlements.plist` mit einem `com.apple.developer.healthkit` Eigenschaft vom Typ `Boolean` festgelegt `Yes`.
- Ein `Info.plist` , deren `UIRequiredDeviceCapabilities` Schlüssel enthält einen Eintrag mit dem `String` Wert `healthkit`.
- Die `Info.plist` benötigen auch entsprechende Datenschutz-Erklärung Einträge: einen `String` Erklärung für den Schlüssel `NSHealthUpdateUsageDescription` Wenn die app mit der Daten zu schreiben und eine `String` Erklärung für den Schlüssel `NSHealthShareUsageDescription` Wenn die app mit der Integrität Kit lesen Daten.

So ermitteln Sie weitere Informationen zum Bereitstellen einer iOS-app die [Gerätebereitstellung](~/ios/get-started/installation/device-provisioning/index.md) Artikel in die Xamarin **Einstieg** Reihe beschreibt die Beziehung zwischen Developer Zertifikate, App-IDs Bereitstellung von Profilen und App-Berechtigungen.

<a name="explicit-appid" />

### <a name="explicit-app-id-and-provisioning-profile"></a>Explizite App-ID und ein Bereitstellungsprofil

Die Erstellung von einer expliziten **App-ID** und eine entsprechende **Bereitstellungsprofil** erfolgt innerhalb von Apple [iOS Dev Center](https://developer.apple.com/devcenter/ios/index.action). 

Ihre aktuellen **App-IDs** aufgelisteten innerhalb der [Zertifikate "," Bezeichner "und" Profile](https://developer.apple.com/account/ios/identifiers/bundle/bundleList.action) Teil der Developer Center. Häufig zeigt diese Liste **ID** Werte der `*`gibt an, die **App-ID*- **Name** kann mit einer beliebigen Anzahl von Suffixen verwendet werden. Solche *Wildcard App IDs* kann nicht mit der Integrität Kit verwendet werden.
 
So erstellen eine explizite **App-ID**, klicken Sie auf die  **+**  Schaltfläche in der oberen rechten Ecke, um führen die **Registrieren von iOS-App-ID** Seite:


[![](healthkit-images/image02.png "Registrieren einer app auf der Apple-Entwicklerportal")](healthkit-images/image02.png)

Wie in der Abbildung oben gezeigt, nach dem Erstellen einer app-Beschreibung, verwenden die **explizite App-ID** Abschnitt aus, um eine ID für Ihre Anwendung zu erstellen. In der **Anwendungsdienste** aktivieren Sie im Abschnitt **Health Kit** in der **Dienste aktivieren** Abschnitt.

Wenn Sie fertig sind, drücken Sie die **Fortfahren** Schaltfläche zum Registrieren der **App-ID** in Ihrem Konto. Sie werden reaktiviert werden wieder die **Zertifikate, Bezeichner und Profile** Seite. Klicken Sie auf **Provisioning Profile** gelangen Sie zu der Liste der aktuellen provisioning Profile, und klicken Sie auf die  **+**  Schaltfläche in der oberen rechten Ecke zum Ausführen der **iOS hinzufügen Bereitstellungsprofil** Seite. Wählen Sie die **iOS-App-Entwicklung** aus, und klicken Sie auf **Fortfahren** zum Abrufen der **wählen Sie App-ID** Seite. Wählen Sie hier den expliziten **App-ID** , die Sie zuvor angegeben haben:


[![](healthkit-images/image03.png "Wählen Sie die explizite App-ID")](healthkit-images/image03.png)

Klicken Sie auf **Fortfahren** und über die verbleibenden Bildschirme, in dem Sie angeben können Ihre **Developer Zertifikate**, **Gerät(e)**, und ein **Namen** dafür **Bereitstellungsprofil**:

[![](healthkit-images/image04.png "Generieren das Bereitstellungsprofil")](healthkit-images/image04.png)

Klicken Sie auf **generieren** und "await" die Erstellung Ihres Profils. Laden Sie die Datei, und doppelklicken Sie darauf, in Xcode zu installieren. Sie können überprüfen, ob die Installation unter **Xcode > Einstellungen > Konten > Details anzeigen...** Daraufhin sollte die gerade installierten provisioning-Profil, und weist sie das Symbol für Integrität Kit und andere spezielle Dienste in ihre **Berechtigungen** Zeile:

[![](healthkit-images/image05.png "Anzeigen des Profils in Xcode")](healthkit-images/image05.png)

<a name="associating-appid" />

### <a name="associating-the-app-id-and-provisioning-profile-with-your-xamarinios-app"></a>Zuordnen der App-ID und ein Bereitstellungsprofil für Ihre App Xamarin.iOS

Nachdem Sie erstellt haben, und installiert eine entsprechende **Bereitstellungsprofil** wie beschrieben, normalerweise wäre es Zeit für eine Projektmappe in Visual Studio für Mac oder Visual Studio zu erstellen. Integrität Kit Zugriff ist auf iOS C#- oder f#-Projekt verfügbar.

Anstatt Schritt für Schritt durch die ein Xamarin iOS 8-Projekt manuell erstellen, öffnen Sie die Beispiel-app, die für diesen Artikel (einschließlich einer vorgefertigten Storyboard und Code) angefügt. Um Ihre Integrität Kit aktiviert die Beispiel-app zuzuordnen **Bereitstellungsprofil**in der **Lösung Pad**mit der rechten Maustaste auf das Projekt, und bringen seine **Optionen** Dialogfeld. Wechseln Sie zu der **iOS-Anwendung** Bereich, und geben Sie den expliziten **App-ID** zuvor wie der app erstellt **Paket-ID**:

[![](healthkit-images/image06.png "Geben Sie die explizite App-ID")](healthkit-images/image06.png)

Wechseln Sie zu der **iOS Bundle Signing** Bereich. Der zuletzt installierten **Bereitstellungsprofil**, mit deren Zuordnung zu den expliziten **App-ID**, zur Verfügung, als die **Bereitstellungsprofil**:

[![](healthkit-images/image07.png "Wählen Sie das Bereitstellungsprofil")](healthkit-images/image07.png)

Wenn die **Bereitstellungsprofil** ist nicht verfügbar ist, doppelklicken Sie auf die **Paket-ID** in der **iOS-Anwendung** im Vergleich zu, die im angegebenen Bereich der **iOS Developer Center** und dass die **Bereitstellungsprofil** installiert ist (**Xcode > Einstellungen > Konten >... Details anzeigen** ).

Wenn die Integrität-Kit-fähigen **Bereitstellungsprofil** ist ausgewählt haben, klicken Sie auf **OK** zum Schließen des Dialogfelds Projektoptionen.

### <a name="entitlementsplist-and-infoplist-values"></a>Entitlements.plist und Werte der Datei "Info.plist"

Die Beispiel-app enthält ein `Entitlements.plist` -Datei (Dies ist erforderlich für die Integrität Kit apps aktiviert) und nicht in der Vorlage für alle Projekte enthalten. Wenn Ihr Projekt keine Ansprüche enthalten, mit der rechten Maustaste auf das Projekt, wählen Sie **Datei > Neues Datei... > iOS > Entitlements.plist** manuell hinzufügen.

Letztlich Ihre `Entitlements.plist` benötigen die folgenden Schlüssel / Wert-Paar:

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

Auf ähnliche Weise die `Info.plist` die app muss ein Wert von über `healthkit` zugeordneten der `UIRequiredDeviceCapabilities` Schlüssel:

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
<string>armv7</string>
    <string>healthkit</string>
</array>

```

Die beispielanwendung, die mit diesem Artikel bereitgestellte enthält eine vorkonfigurierte `Entitlements.plist` , die alle erforderlichen Schlüssel enthält.

<a name="programming" />

## <a name="programming-health-kit"></a>Programmierung Integrität Kit

Die Integrität Kit Datenspeicher ist eine private, benutzerspezifische Datenspeicher, der von den apps gemeinsam verwendet wird. Da Integritätsinformationen so vertraulich ist, muss der Benutzer positive Schritte zum Datenzugriff ausführen. Dieser Zugriff kann teilweise werden (schreiben, aber nicht lesen, die Zugriff für einige Typen von Daten, aber keine anderen usw.) und kann jederzeit widerrufen werden. Integrität Kit Anwendungen sollten defensiv, Voraussetzung geschrieben werden, dass viele Benutzer zögern über ihre Integrität-bezogene Informationen gespeichert werden.

Daten zur Clientintegrität Kit ist auf Apple angegebenen Typen. Diese Typen werden streng definiert: Einige z. B. Blut-Typ sind beschränkt auf die bestimmte Werte einer Enumeration Apple angegeben ist, während andere eine Größe, die mit einer Maßeinheit (z. B. Gramm Kalorien und Liter) kombinieren. Selbst Daten, die gemeinsam eine kompatible Maßeinheit unterscheiden sich durch ihre `HKObjectType`z. B. das Typsystem fängt einen falschen Versuch zum Speichern einer `HKQuantityTypeIdentifier.NumberOfTimesFallen` Wert, der einem Feld erwartet ein `HKQuantityTypeIdentifier.FlightsClimbed` , obwohl beide verwenden die` HKUnit.Count` Maßeinheit.

Die Typen, die in den Datenspeicher Integrität Kit speicherbare sind alle Unterklassen des `HKObjectType`. `HKCharacteristicType` Speichern von Objekten biologischen Geschlecht, Blut Typ und Geburtsdatum. Weitere allgemeine aber sind `HKSampleType` Objekte, die Daten, die als Stichprobe werden darstellen, zu einem bestimmten Zeitpunkt oder über einen Zeitraum genommen. 

[![](healthkit-images/image08.png "Diagramm der HKSampleType-Objekte")](healthkit-images/image08.png)

`HKSampleType` ist abstrakt und verfügt über vier konkreten Teilklassen. Es ist derzeit nur eine Art von `HKCategoryType` Daten, d. h. Analysis Energiesparmodus. Die Mehrzahl der Daten im Health Kit sind vom Typ `HKQuantityType` und speichern Sie ihre Daten in `HKQuantitySample` Objekte, die mit dem vertrauten Factoryentwurfsmuster erstellt werden:

[![](healthkit-images/image09.png "Die Mehrzahl der Daten im Health Kit sind vom Typ HKQuantityType und speichern ihre Daten in Objekten HKQuantitySample")](healthkit-images/image09.png)

`HKQuantityType` Typen von reichen `HKQuantityTypeIdentifier.ActiveEnergyBurned` auf `HKQuantityTypeIdentifier.StepCount`. 

<a name="requesting-permission" />

### <a name="requesting-permission-from-the-user"></a>Zustimmung des Benutzers

Endbenutzer müssen Schritte positive kann eine app Integrität Kit Daten lesen oder schreiben. Dies erfolgt über die Integrität-app, die auf Geräte mit iOS 8 bereits installiert ist. Beim ersten Health-Kit-app ausgeführt wird, erhält der Benutzer mit einem System gesteuert **Health Zugriff** Dialogfeld:

[![](healthkit-images/image10.png "Erhält der Benutzer ein Dialogfeld für die System-gesteuerten Integrität Zugriff")](healthkit-images/image10.png)

Der Benutzer kann später mithilfe des Integritäts-app Berechtigungen ändern **Quellen** Dialogfeld:

[![](healthkit-images/image11.png "Der Benutzer kann die Berechtigungen, die mit Health apps-Dialogfeld "Datenquellen" ändern.")](healthkit-images/image11.png)

Da Integritätsinformationen extrem sensibel ist, sollten app-Entwickler ihre Programme defensiv, unter der Annahme schreiben, dass die Berechtigungen werden abgelehnt und geändert, während die app ausgeführt wird. Die am häufigsten verwendete Technik ist, zum Anfordern von Berechtigungen in der `UIApplicationDelegate.OnActivated` Methode und die Benutzeroberfläche nach Bedarf ändern.

### <a name="permissions-walkthrough"></a>Berechtigungen Exemplarische Vorgehensweise

Öffnen Sie im Projekt Integrität Kit bereitgestellt, die `AppDelegate.cs` Datei. Beachten Sie die Anweisung mithilfe `HealthKit`; am Anfang der Datei.


Der folgende Code bezieht sich auf Integrität Kit Berechtigungen:

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

Der gesamte Code in diesen Methoden konnten erfolgen Inline `OnActivated`, aber die Beispiel-app separate Methoden verwendet, um ihre Absicht deutlicher zu machen: `ValidateAuthorization()` verfügt über die erforderlichen Schritte zum Anfordern des Zugriffs auf bestimmte Typen, die geschrieben werden (und lesen, bei Bedarf von die app) und `ReactToHealthCarePermissions()` ist ein Rückruf, der aktiviert wird, nachdem der Benutzer mit dem Dialogfeld "Berechtigungen" in der Health.app interagiert hat.

Der Auftrag des `ValidateAuthorization()` wird zum Erstellen der Gruppe von `HKObjectTypes` , dass die Anwendung schreibt und Autorisierung zum Aktualisieren dieser Daten anfordern. In der Beispiel-app die `HKObjectType` ist für den Schlüssel `KHQuantityTypeIdentifierKey.HeartRate`. Dieser Typ wird dem Satz hinzugefügt `typesToWrite`, dagegen den Satz `typesToRead` leer gelassen wird. Diese Sätze und einem Verweis auf die `ReactToHealthCarePermissions()` -Rückruf übergeben wird `HKHealthStore.RequestAuthorizationToShare()`.

Die `ReactToHealthCarePermissions()` Rückruf wird aufgerufen, nachdem der Benutzer mit dem Dialogfeld "Berechtigungen" interagiert hat und zwei Angaben übergeben wird: ein `bool` -Wert, der werden `true` , wenn der Benutzer mit dem Dialogfeld "Berechtigungen" und eine interagierthat`NSError`, falls ungleich Null, womit eine Art von Fehler präsentieren das Dialogfeld "Berechtigungen" zugeordnet.

> [!IMPORTANT]
> **Hinweis:** zu den Argumenten dieser Funktion werden: die _Erfolg_ und _Fehler_ Parameter bedeuten nicht, ob der Benutzer die Berechtigung zum Health Kit Datenzugriff gewährt wurde! Sie nur ein Hinweis, dass der Benutzer die Möglichkeit, den Zugriff auf die Daten zu erlauben erteilt wurden.

Um zu bestätigen, ob die app Zugriff auf die Daten der `HKHealthStore.GetAuthorizationStatus()` Einsatz und übergibt `HKQuantityTypeIdentifierKey.HeartRate`. Basierend auf den Status zurückgegeben, die die app aktiviert oder deaktiviert die Möglichkeit, Daten eingeben. Es ist keine Standardbenutzer-Erfahrung für den Umgang mit einer Verweigerung des Zugriffs, und es gibt zahlreiche mögliche Optionen. Der Status wird festgelegt in der Beispiel-app auf einem `HeartRateModel` Singleton-Objekt, das wiederum relevante Ereignisse auslöst.

## <a name="model-view-and-controller"></a>Modell, Ansicht und Controller

So überprüfen die `HeartRateModel` Singleton-Objekt, öffnen die `HeartRateModel.cs` Datei:

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

Der erste Abschnitt wird Code häufig zum Erstellen von generische Ereignisse und Ereignishandler. Der erste Teil der `HeartRateModel` -Klasse ist außerdem Textbaustein zum Erstellen eines Thread-sichere Singletonobjekts.

Klicken Sie dann `HeartRateModel` 3 Ereignisse verfügbar gemacht werden: 

- `EnabledChanged` -Gibt an, dass Herzfrequenz Speicher aktiviert oder deaktiviert wurde (Beachten Sie, dass Speicher anfänglich deaktiviert ist). 
- `ErrorMessageChanged` -Diese Beispiel-app haben wir ein sehr einfaches Modell für die Fehlerbehandlung: eine Zeichenfolge mit dem letzten Fehler. 
- `HeartRateStored` -Wird ausgelöst, wenn eine Herzfrequenz in der Integrität-Kit-Datenbank gespeichert ist.

Beachten Sie, dass die, wenn diese Ereignisse ausgelöst werden, erfolgt über `NSObject.InvokeOnMainThread()`, wodurch Abonnenten für die Benutzeroberfläche zu aktualisieren. Alternativ können Sie die Ereignisse konnte dokumentiert werden, als ausgelösten in Hintergrundthreads aus, und die Zuständigkeit für das Sicherstellen der Kompatibilität konnte ihre Handler überlassen werden. Thread Überlegungen sind in Health Kit Anwendungen wichtig, da viele der Funktionen, z. B. die Erlaubnis asynchron sind, und führen Sie ihre Rückrufe für nicht-Main-Threads.

Der spezifische "heath"-Kit-Code in `HeartRateModel` befindet sich in zwei Funktionen `HeartRateInBeatsPerMinute()` und `StoreHeartRate()`. 

`HeartRateInBeatsPerMinute()` Konvertiert das Argument in eine stark typisierte Integrität Kit `HKQuantity`. Der Typ der Menge ist, die durch die `HKQuantityTypeIdentifierKey.HeartRate` und sind die Einheiten der Menge `HKUnit.Count` geteilt durch `HKUnit.Minute` (also die Einheit ist *pro Minute ist*). 

Die `StoreHeartRate()` -Funktion akzeptiert eine `HKQuantity` (in der Beispiel-app erstellt eine `HeartRateInBeatsPerMinute()` ). Um die Daten zu überprüfen, verwendet er die `HKQuantity.IsCompatible()` Methode, die zurückgibt `true` Wenn Einheiten für das Objekt in der Einheiten im Argument konvertiert werden können. Wenn mit die Menge erstellt wurde `HeartRateInBeatsPerMinute()` wird offensichtlich zurückgegeben `true`, aber es würde ebenfalls zurückgegeben werden `true` , wenn die Menge, wie z. B. erstellt wurden *ist pro Stunde*. In der Regel `HKQuantity.IsCompatible()` können verwendet werden, um die Masse überprüfen Entfernung und Energie, die der Benutzer oder ein Gerät die Eingabe oder Anzeige in einem System Maßeinheit (z. B. englische Einheiten), aber die möglicherweise in einem anderen System (z. B. metrischen) gespeichert werden. 

Sobald die Kompatibilität der Menge überprüft wurde, die `HKQuantitySample.FromType()` Factory-Methode wird verwendet, um einen stark typisierten erstellen `heartRateSample` Objekt. `HKSample` Objekte verfügen über ein Anfangs- und Enddatum Datum; für sofortige Messwerte sollte diese Werte entsprechen, wie im Beispiel werden. Im Beispiel auch nicht setzt alle Schlüssel-Wert-Daten seiner `HKMetadata` Argument, aber einer konnte Code wie den folgenden Code verwenden, um sensorspeicherorts angeben:

```csharp
var hkm = new HKMetadata();
hkm.HeartRateSensorLocation = HKHeartRateSensorLocation.Chest;

```

Sobald die `heartRateSample` wurde erstellt, der Code erstellt eine neue Verbindung mit der Datenbank mit der using Block. In diesem Block die `HKHealthStore.SaveObject()` Methode versucht, den asynchronen Schreibvorgang in der Datenbank. Der resultierende Aufruf an den Lambda-Ausdruck löst relevante Ereignisse entweder `HeartRateStored` oder `ErrorMessageChanged`.

Nachdem das Modell programmiert wurde, ist es um zu sehen, wie der Controller den Zustand des Modells widerspiegelt. Öffnen Sie die `HKWorkViewController.cs` Datei. Der Konstruktor einfach verbindet die `HeartRateModel` Singleton, Ereignisbehandlungsmethoden (erneut, dies kann mit Lambda-Ausdrücken Inline ausgeführt werden, aber separate Methoden stellen die Absicht etwas leichter ersichtlich):

```csharp
public HKWorkViewController (IntPtr handle) : base (handle)
{
     HeartRateModel.Instance.EnabledChanged += OnEnabledChanged;
     HeartRateModel.Instance.ErrorMessageChanged += OnErrorMessageChanged;
     HeartRateModel.Instance.HeartRateStored += OnHeartBeatStored;
}

```

Hier werden die entsprechenden Handler aus:

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

Natürlich in einer Anwendung mit einem Controller, es ist möglich, um die Erstellung eines separaten Model-Objekts und die Verwendung von Ereignissen für die ablaufsteuerung zu vermeiden, aber die Verwendung von Modellobjekte eignet sich besser für realen apps.

## <a name="running-the-sample-app"></a>Ausführen der Beispielapp

IOS-Simulator unterstützt Integrität Kit nicht. Debuggen muss auf ein physisches Gerät mit iOS 8 durchgeführt werden.

Fügen Sie einem ordnungsgemäß bereitgestellt iOS 8-Gerät-Entwicklung mit Ihrem System. Wählen Sie diese als das Bereitstellungsziel in Visual Studio für Mac, und wählen Sie im **ausführen > Debuggen**.

> [!IMPORTANT]
> **Hinweis:** Fehler bezüglich der Bereitstellung werden an diesem Punkt Oberfläche. Um Fehler zu beheben, überprüfen Sie zum Erstellen und die Bereitstellung eines Integrität Kit App Abschnitts oben aus. Die Komponenten sind: 
>
> - **iOS Dev Center** -explizite App-ID und Integrität Kit Provisioning-Profil aktiviert. 
> - **Projekt Optionen** -Paket-ID (explizite App-ID) & Bereitstellungsprofil.
> - **Quellcode** -Entitlements.plist & Datei "Info.plist"

Vorausgesetzt, dass Vorschriften ordnungsgemäß festgelegt wurden, wird die Anwendung gestartet. Bei Erreichen der `OnActivated` -Methode, er Integrität Kit Autorisierung anfordert. Dies durch das Betriebssystem kommt zum ersten Mal wird der Benutzer das folgende Dialogfeld angezeigt:


[![](healthkit-images/image12.png "Der Benutzer wird in diesem Dialogfeld angezeigt werden")](healthkit-images/image12.png)

Konfigurieren Sie die Anwendung zum Aktualisieren von Herzfrequenz Daten und Ihre app wird erneut angezeigt. Die `ReactToHealthCarePermissions` Rückruf wird asynchron aktiviert werden. Dies bewirkt, dass die `HeartRateModel’s` `Enabled` -Eigenschaft zu ändern, die ausgelöst wird die `EnabledChanged` Ereignis, wodurch die `HKPermissionsViewController.OnEnabledChanged()` -Ereignishandler ausgeführt wird, wodurch die `StoreData` Schaltfläche. Das folgende Diagramm zeigt die Sequenz:


[![](healthkit-images/image13.png "Dieses Diagramm veranschaulicht die Abfolge von Ereignissen")](healthkit-images/image13.png)

Drücken Sie die **Datensatz** Schaltfläche. Dies bewirkt, dass die `StoreData_TouchUpInside()` Handler ausgeführt wird, die versuchen, den Wert des analysiert die `heartRate` Textfeld Umwandeln in eine `HKQuantity` über die bereits vorgestellten `HeartRateModel.HeartRateInBeatsPerMinute()` Funktion, und übergeben Sie die Menge an `HeartRateModel.StoreHeartRate()`. Wie bereits gesagt, dies wird versucht, die Daten zu speichern und löst entweder eine `HeartRateStored` oder `ErrorMessageChanged` Ereignis.

Doppelklicken Sie auf die **Home** Schaltfläche auf Ihrem Gerät, und öffnen Sie Health-app. Klicken Sie auf die **Quellen** Registerkarte, und Sie sehen die Beispiel-app aufgeführt. Wählen Sie sie aus, und nicht die zulassen Sie Berechtigung zum Aktualisieren von Daten Herzfrequenz. Doppelklicken Sie auf die **Home** Schaltfläche und wechseln zurück zu Ihrer app. Noch einmal: `ReactToHealthCarePermissions()` wird aufgerufen, aber diesmal, da der Zugriff verweigert wird, die **StoreData** Schaltfläche wird deaktiviert (Beachten Sie, dass in diesem asynchron Fall und die Änderung in der Benutzeroberfläche kann für den Endbenutzer sichtbar sein).

## <a name="advanced-topics"></a>Weiterführende Themen

Lesen von Daten aus dem Health Kit Datenbank ist das Schreiben von Daten sehr ähnlich: eine gibt die Arten von Daten, die eine Autorisierung für Anforderungen, zugreifen möchte, und diese Autorisierung gewährt wird, die Daten mit automatische Konvertierung zu kompatiblen Einheiten verfügbar sind Measure.

Es gibt diverse komplexere Abfragefunktionen, die es ermöglichen, Prädikat-basierte Abfragen und Abfragen, die Updates ausführen, wenn relevante Daten aktualisiert werden. 

Integrität Kit Anwendungsentwickler sollten Abschnitt Integrität Kit Apple [App überprüfen Richtlinien](https://developer.apple.com/app-store/review/guidelines/#healthkit).

Nachdem die Sicherheits- und Typsystem Modelle verstanden werden, ist speichern und Lesen von Daten in der freigegebenen Datenbank für die Integrität Kit ziemlich einfach. Viele der Funktionen in Health Kit asynchron ausgeführt werden, und Anwendungsentwickler müssen entsprechend ihrem Programm schreiben.

Zum Zeitpunkt der Verfassung dieses Artikels gibt es derzeit keine Entsprechung Integrität Kit in Android oder Windows Phone.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wir gesehen haben, wie Integrität Kit Anwendungen speichern können, abgerufen und der Integrität der Freigabe von Informationen zu verknüpften, gleichzeitig wird eine standardmäßige Health-app, die der Benutzerzugriff und die Kontrolle über diese Daten bereitgestellt. 

Außerdem haben wir gesehen, wie Datenschutz, Sicherheit und Datenintegrität Sicherheitsrisiken für Integrität bezogene Informationen überschreiben und apps mithilfe von Health Kit müssen befasst sich mit die Erhöhung der Komplexität in der Anwendung Verwaltungsaspekte (Bereitstellung), Schreiben von Code (Integrität-Kit-Typ System), und Benutzer (Benutzersteuerelement über System Dialogfelder und Health-app Berechtigungen) auftreten. 

Schließlich haben wir eine einfache Implementierung der Integrität Kit mithilfe der enthaltenen Beispiel-app, die Taktdaten in den Integritäts-Kit-Speicher geschrieben und verfügt über einen asynchronen unterstützende Entwurf betrachten.

## <a name="related-links"></a>Verwandte Links

- [HKWork (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios8/IntroToHealthKit/)
- [Einführung in iOS 8](~/ios/platform/introduction-to-ios8.md)
