---
title: HealthKit in Xamarin.iOS
description: Dieses Dokument beschreibt HealthKit, ein Framework, eingeführt in iOS 8, die einen zentralisierten, koordinierten und sicheren Datenspeicher für integritätsbezogene Informationen bereitstellt. Es wird erläutert, wie Sie eine app HealthKit bereitstellen und wie Sie Code schreiben, die HealthKit-Framework verwendet.
ms.prod: xamarin
ms.assetid: E3927A21-507C-43BA-A2AD-957716BA9B52
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 8dcb478b303c4c73f7e73dc018ad56b1301389c8
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67830047"
---
# <a name="healthkit-in-xamarinios"></a>HealthKit in Xamarin.iOS

Integrität Kit bietet einen sicheren Datenspeicher für integritätsbezogene Informationen des Benutzers. Integrität Kit apps können mit expliziten Berechtigungen des Benutzers, lesen und Schreiben in diese Datenspeicher und Benachrichtigungen erhalten, wenn die relevante Daten, die hinzugefügt werden. Apps können die Darstellung der Daten oder des Benutzers kann von Apple bereitgestellten Integrität app verwenden, um ein Dashboard für alle ihre Daten anzeigen.

Da integritätsbezogene Daten also vertrauliche und wichtige, Integrität Kit ist stark typisiert, mit berechnungseinheiten und eine explizite Zuordnung mit dem Typ des aufgezeichneten Informationen (z. B. "Blood Zucker-Ebene" oder "Herz"). Darüber hinaus Health-Kit-apps müssen explizite Berechtigungen verwenden, muss anfordern des Zugriffs auf die bestimmten Arten von Informationen, und der Benutzer muss explizit der app Zugriff gewähren auf diese Typen von Daten.

Dieser Artikel bietet eine Einführung:

- Integrität Kitss sicherheitsanforderungen, einschließlich Bereitstellung von Anwendungen und Benutzer die Berechtigung zum Zugriff auf die Integrität Kit Datenbank anfordern;
- Integrität Kitss-Typsystem, das minimiert die Wahrscheinlichkeit, dass falsch angewendet oder Formatelementen Daten;
- Schreiben in den freigegebenen, systemweite-Health-Kit-Datenspeicher.

Dieser Artikel behandelt Themen für fortgeschrittenere Benutzer, z. B. eine Datenbankabfrage, Konvertierung von Maßeinheiten und Empfangen von Benachrichtigungen über neue Daten nicht.

In diesem Artikel erstellen wir eine beispielanwendung auf Herz des Benutzers aufgezeichnet werden:

[![](healthkit-images/image01.png "Eine beispielanwendung, um das Herz der Benutzer zu erfassen.")](healthkit-images/image01.png#lightbox)

## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um die in diesem Artikel vorgestellten Schritte auszuführen:

- **Xcode 7 und iOS 8 (oder höher)** – die neuesten Xcode und Apple iOS-APIs müssen installiert und konfiguriert werden, auf dem Computer des Entwicklers.
- **Visual Studio für Mac oder Visual Studio** – die neueste Version von Visual Studio für Mac installiert und auf dem Computer des Entwicklers konfiguriert werden soll.
- **iOS 8 (oder höher) Gerät** – ein iOS-Gerät die neueste Version von iOS 8 oder höher, für die Tests ausgeführt.

> [!IMPORTANT]
> Integrität Kit wurde in iOS 8 eingeführt. Derzeit Integrität Kit ist nicht verfügbar, auf dem iOS-Simulator, und Debuggen ist die Verbindung mit einem physischen iOS-Gerät erforderlich.




## <a name="creating-and-provisioning-a-health-kit-app"></a>Erstellen und Bereitstellen einer Health-Kit-App
Bevor eine Xamarin-iOS 8-Anwendung die HealthKit-API verwenden kann, müssen ordnungsgemäß konfiguriert und bereitgestellt werden. In diesem Abschnitt werden die erforderlichen Schritte zum Einrichten von Ihrer Xamarin-Anwendung ordnungsgemäß behandelt.

Integrität Kit apps erfordern:

- Eine explizite **App-ID**.
- Ein **Bereitstellungsprofil** zugeordnet sind, explizite **App-ID** und **Integrität Kit** Berechtigungen.
- Ein `Entitlements.plist` mit einem `com.apple.developer.healthkit` Eigenschaft vom Typ `Boolean` festgelegt `Yes`.
- Ein `Info.plist` , deren `UIRequiredDeviceCapabilities` Schlüssel enthält einen Eintrag mit der `String` Wert `healthkit`.
- Die `Info.plist` auch, müssen entsprechende Datenschutz-Erläuterung: eine `String` Erklärung für den Schlüssel `NSHealthUpdateUsageDescription` Wenn die app wird zum Schreiben von Daten und ein `String` Erklärung für den Schlüssel `NSHealthShareUsageDescription` Wenn die app zum Lesen der Health-Kit ist Daten.

Weitere Informationen zur Bereitstellung einer iOS-app, die [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md) Artikel in der Xamarin **Einstieg** Reihe beschreibt die Beziehung zwischen Entwicklerzertifikate, App-IDs, Bereitstellungsprofile und App-Berechtigungen.

<a name="explicit-appid" />

### <a name="explicit-app-id-and-provisioning-profile"></a>Explizite App-ID und eines Bereitstellungsprofils

Die Erstellung einer expliziten **App-ID** und eine entsprechende **Bereitstellungsprofil** erfolgt im Apple [iOS Developer Center](https://developer.apple.com/devcenter/ios/index.action). 

Ihre aktuellen **App-IDs** finden Sie in der [Zertifikate, Bezeichner & Profile](https://developer.apple.com/account/ios/identifiers/bundle/bundleList.action) Abschnitt im Dev Center. Diese Liste zeigt häufig **ID** Werte `*`, um anzugeben, die die **App-ID** - **Namen** kann mit einer beliebigen Anzahl von Suffixen verwendet werden. Solche *Wildcard App-IDs* kann nicht mit dem Health-Kit verwendet werden.
 
Eine explizite Erstellung **App-ID**, klicken Sie auf die **+** -Schaltfläche in der rechten oberen Ecke, die Sie zum Ausführen der **Registrieren von iOS-App-ID** Seite:


[![](healthkit-images/image02.png "Registrieren einer app auf Apple-Entwicklerportal")](healthkit-images/image02.png#lightbox)

Verwenden Sie wie in der obigen Abbildung wird gezeigt, nach dem Erstellen einer app-Beschreibung, die **Explicit App ID** Abschnitt aus, um eine ID für Ihre Anwendung zu erstellen. In der **Anwendungsdienste** aktivieren Sie im Abschnitt **Integrität Kit** in die **Dienste aktivieren** Abschnitt.

Wenn Sie fertig sind, drücken Sie die **Weiter** Schaltfläche zum Registrieren der **App-ID** in Ihrem Konto. Sie gelangen zurück auf die **Zertifikate, Bezeichner und Profile** Seite. Klicken Sie auf **Bereitstellungsprofile** gelangen Sie zu der Liste der aktuellen bereitstellungsprofilen, und klicken Sie auf die **+** Schaltfläche in der Ecke oben rechts, die Sie zum Ausführen der **iOS hinzufügen Bereitstellungsprofil** Seite. Wählen Sie die **iOS-App-Entwicklung** aus, und klicken Sie auf **Weiter** , um zu erhalten die **Auswählen der App-ID** Seite. Hier wählen Sie den expliziten **App-ID** , die Sie zuvor angegeben haben:


[![](healthkit-images/image03.png "Wählen Sie die explizite App-ID")](healthkit-images/image03.png#lightbox)

Klicken Sie auf **Weiter** und durch die verbleibenden Bildschirme, in dem Sie angeben werden Ihre **Developer Zertifikate**, **Geräte**, und ein **Namen** für diesen **Bereitstellungsprofil**:

[![](healthkit-images/image04.png "Generieren das Bereitstellungsprofil")](healthkit-images/image04.png#lightbox)

Klicken Sie auf **generieren** und "await" die Erstellung Ihres Profils. Herunterzuladen Sie die Datei, und doppelklicken Sie darauf, um in Xcode zu installieren. Sie können überprüfen, ob die Installation unter **Xcode > Voreinstellungen > Konten > Details anzeigen...** Ihr gerade installierten Bereitstellungsprofil sollte angezeigt werden, und es sollte das Symbol für Health-Kit und andere spezielle Dienste in der **Berechtigungen** Zeile:

[![](healthkit-images/image05.png "Profil in Xcode anzeigen")](healthkit-images/image05.png#lightbox)

<a name="associating-appid" />

### <a name="associating-the-app-id-and-provisioning-profile-with-your-xamarinios-app"></a>Zuordnen der App-ID und ein Bereitstellungsprofil mit Ihrer Xamarin.iOS-App

Nachdem Sie erstellt haben, und installiert eine entsprechende **Bereitstellungsprofil** wie beschrieben, normalerweise wäre es Zeit, eine Projektmappe in Visual Studio für Mac oder Visual Studio zu erstellen. Integrität der Kit Zugriff ist für alle iOS- C# oder F# Projekt.

Anstatt den Prozess zum Erstellen einer Xamarin iOS 8-Projekt manuell zu durchlaufen, öffnen Sie die Beispiel-app, die in diesem Artikel (die eine vorgefertigte Storyboard und Code enthält) hinzugefügt. Zuordnen die Beispiel-app mit dem Health-Kit aktiviert **Bereitstellungsprofil**in der **Lösungspad**mit der rechten Maustaste auf das Projekt, und rufen Sie die **Optionen** Dialogfeld. Wechseln Sie zu der **iOS-Anwendung** Bereich, und geben Sie den expliziten **App-ID** Sie zuvor erstellt haben wie der app die **Bündel-ID**:

[![](healthkit-images/image06.png "Geben Sie die explizite App-ID")](healthkit-images/image06.png#lightbox)

Wechseln Sie nun zur der **iOS Bundle-Signierung** Bereich. Ihre zuletzt installiert **Bereitstellungsprofil**, mit der Zuordnung zu den expliziten **App-ID**, zur Verfügung, als die **Bereitstellungsprofil**:

[![](healthkit-images/image07.png "Wählen Sie das Bereitstellungsprofil")](healthkit-images/image07.png#lightbox)

Wenn die **Bereitstellungsprofil** ist nicht verfügbar ist, überprüfen Sie die **Bündel-ID** in die **iOS-Anwendung** Bereich im Vergleich zu, die in der **iOS Dev Center** und dass die **Bereitstellungsprofil** installiert ist (**Xcode > Voreinstellungen > Konten > Details anzeigen...** ).

Wenn der Health-Kit-aktivierte **Bereitstellungsprofil** wird ausgewählt, klicken Sie auf **OK** um das Dialogfeld "Projektoptionen" zu schließen.

### <a name="entitlementsplist-and-infoplist-values"></a>"Entitlements.plist" und "Info.plist"-Werte

Die beispielanwendung enthält ein `Entitlements.plist` -Datei (mit erforderlich ist, für die Integrität Kit apps aktiviert), und nicht in der Vorlage für alle Projekte enthalten. Wenn Ihr Projekt keine Berechtigungen enthält, mit der rechten Maustaste auf das Projekt, wählen Sie **Datei > neue Datei… > iOS > "Entitlements.plist"** einen manuell hinzufügen.

Letztlich Ihre `Entitlements.plist` müssen die folgenden Schlüssel-Wert-Paar:

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

Auf ähnliche Weise die `Info.plist` für die app den Wert muss `healthkit` zugeordneten der `UIRequiredDeviceCapabilities` Schlüssel:

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
<string>armv7</string>
    <string>healthkit</string>
</array>

```

Mit diesem Artikel bereitgestellte beispielanwendung enthält eine vorkonfigurierte `Entitlements.plist` , die alle erforderlichen Schlüssel enthält.

<a name="programming" />

## <a name="programming-health-kit"></a>Programmierung Integrität Kit

Der Health-Kit-Datenspeicher ist ein privates, benutzerspezifische-Datenspeicher, der von den apps gemeinsam verwendet wird. Da Integritätsinformationen so vertraulich ist, muss der Benutzer eine positive Schritte zum Datenzugriff ausführen. Dieser Zugriff ist möglicherweise teilweise (schreiben, aber nicht lesen, die Zugriff für einige Arten von Daten, aber in anderen, usw.) und kann jederzeit widerrufen werden. Integrität Kit Anwendungen sollten defensiv, unter der Voraussetzung geschrieben werden, dass viele Benutzer zögern, zu deren integritätsbezogene Informationen gespeichert werden.

Kit integritätsdaten ist auf Apple angegeben. Diese Typen sind streng definiert: einige, z. B. Typ hat, sind beschränkt auf die bestimmte Werte einer Enumeration ist Apple angegeben, während andere eine Größe mit einer Maßeinheit (z. B. Gramm, Kalorien und Liter) kombinieren. Sogar Daten, die gemeinsam eine kompatible Maßeinheit unterscheiden sich durch ihre `HKObjectType`; z. B. das Typsystem fängt einen falschen Versuch zum Speichern einer `HKQuantityTypeIdentifier.NumberOfTimesFallen` Wert, der ein Feld erwartet eine `HKQuantityTypeIdentifier.FlightsClimbed` , obwohl beide verwenden die `HKUnit.Count` die Maßeinheit an.

Die Typen, die in den Health-Kit-Datenspeicher gespeichert sind, alle Unterklassen von `HKObjectType`. `HKCharacteristicType` Speichern von Objekten biologischen Geschlecht und Blut Typ Geburtsdatum. Gängiger sind jedoch `HKSampleType` -Objekte, die Daten darstellen, die zu einem bestimmten Zeitpunkt oder über einen Zeitraum entnommen werden. 

[![](healthkit-images/image08.png "Diagramm der HKSampleType-Objekte")](healthkit-images/image08.png#lightbox)

`HKSampleType` ist abstrakt und verfügt über vier konkreten Unterklassen. Es gibt derzeit nur eine Art von `HKCategoryType` Daten, die Analyse den Energiesparmodus ist. Die überwiegende Mehrheit von Daten im Health Kit sind vom Typ `HKQuantityType` und speichern Sie ihre Daten in `HKQuantitySample` -Objekte, die mithilfe von vertrauten Factoryentwurfsmusters erstellt werden:

[![](healthkit-images/image09.png "Die überwiegende Mehrheit von Daten im Health Kit sind vom Typ HKQuantityType und speichern ihre Daten in HKQuantitySample-Objekten")](healthkit-images/image09.png#lightbox)

`HKQuantityType` Typen von reichen `HKQuantityTypeIdentifier.ActiveEnergyBurned` zu `HKQuantityTypeIdentifier.StepCount`. 

<a name="requesting-permission" />

### <a name="requesting-permission-from-the-user"></a>Die Berechtigung vom Benutzer anfordert

Endbenutzer müssen positive Schritte, sodass eine Anwendung zum Lesen oder schreiben die Integrität-Kit-Daten ausführen. Dies erfolgt über die Integritätsdienst-app, die auf Geräte mit iOS 8 bereits installiert ist. Beim ersten eine Health-Kit-app ausgeführt wird, erhält der Benutzer mit einem System kontrolliert **Integrität Zugriff** Dialogfeld:

[![](healthkit-images/image10.png "Der Benutzer ist ein System kontrolliert Integrität Access-Dialogfeld angezeigt")](healthkit-images/image10.png#lightbox)

Der Benutzer kann später mit der Integrität der app Berechtigungen ändern **Quellen** Dialogfeld:

[![](healthkit-images/image11.png "Der Benutzer kann die Berechtigungen, die über Integrität-apps-Dialogfeld \"Datenquellen\" ändern.")](healthkit-images/image11.png#lightbox)

Da Integritätsinformationen ausgesprochen sensibel ist, sollten Anwendungsentwickler, die ihre Programme defensiv, unter der Annahme schreiben, dass Berechtigungen verweigert und geändert, während die app ausgeführt wird. Die am häufigsten verwendete Technik ist zum Anfordern von Berechtigungen in der `UIApplicationDelegate.OnActivated` Methode und ändern Sie die Benutzeroberfläche entsprechend.

### <a name="permissions-walkthrough"></a>Exemplarische Vorgehensweise die Berechtigungen

Öffnen Sie in Ihrem Projekt Integrität Kit bereitgestellt, die `AppDelegate.cs` Datei. Beachten Sie, dass die Anweisung mithilfe `HealthKit`; am Anfang der Datei.


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

Der gesamte Code in diesen Methoden können in ausgeführt werden, Inline `OnActivated`, aber die Beispiel-app verwendet separate Methoden, um ihre Absichten klarer zu machen: `ValidateAuthorization()` verfügt über die erforderlichen Schritte zum Anfordern des Zugriffs auf die bestimmte Typen, die geschrieben werden (und lesen, die bei Bedarf von die app) und `ReactToHealthCarePermissions()` ist ein Rückruf an, die aktiviert wird, nachdem der Benutzer das Dialogfeld "Berechtigungen" in der Health.app interagiert hat.

Die Aufgabe des `ValidateAuthorization()` ist die Erstellung den Satz von `HKObjectTypes` , dass die app schreiben und Autorisierung zum Aktualisieren dieser Daten anfordern. In der Beispiel-app die `HKObjectType` ist für den Schlüssel `KHQuantityTypeIdentifierKey.HeartRate`. Dieser Typ wird dem Satz hinzugefügt `typesToWrite`, zwar den Satz `typesToRead` leer gelassen wird. Diese Sätze und einem Verweis auf die `ReactToHealthCarePermissions()` Rückruf übergeben wird, um `HKHealthStore.RequestAuthorizationToShare()`.

Die `ReactToHealthCarePermissions()` Rückruf wird aufgerufen, nachdem der Benutzer, mit dem Dialogfeld "Berechtigungen interagiert hat", und zwei Angaben übergeben: eine `bool` -Wert, der werden `true` , wenn der Benutzer das Dialogfeld "Berechtigungen" und eine interagierthat`NSError`, wenn Null, womit eine Art von Fehler bei der Übergabe des Dialogfeld "Berechtigungen".

> [!IMPORTANT]
> Zu den Argumenten für diese Funktion deaktiviert ist: die _Erfolg_ und _Fehler_ Parameter nicht angegeben, ob der Benutzer die Zugriffsberechtigung für Health-Kit Daten erteilt hat. Sie geben nur an, dass der Benutzer die Möglichkeit, Zugriff auf die Daten erhalten hat.

Um zu bestätigen, ob die app Zugriff auf die Daten, die `HKHealthStore.GetAuthorizationStatus()` verwendet wird, übergibt `HKQuantityTypeIdentifierKey.HeartRate`. Basierend auf den Status zurückgegeben, die app aktiviert oder deaktiviert die Möglichkeit, Daten einzugeben. Es ist keine Standardbenutzer-Benutzeroberfläche für den Umgang mit einer zugriffsverweigerung verursacht, und es gibt viele Möglichkeiten. Der Status wird festgelegt in der Beispiel-app auf einem `HeartRateModel` Singleton-Objekt, das wiederum relevante Ereignisse auslöst.

## <a name="model-view-and-controller"></a>Modell, Ansicht und Controller

Überprüfen der `HeartRateModel` Singleton-Objekt, öffnen die `HeartRateModel.cs` Datei:

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

Im erste Abschnitt wird die Codebausteine zum Erstellen von generische Ereignisse und Handler. Der erste Teil der `HeartRateModel` -Klasse ist auch Codebausteine zum Erstellen eines Thread-sichere Singletonobjekts.

Klicken Sie dann `HeartRateModel` 3 Ereignisse verfügbar macht: 

- `EnabledChanged` : Gibt an, dass das Herz Speicher aktiviert oder deaktiviert wurde (Beachten Sie, dass Speicher ursprünglich deaktiviert ist). 
- `ErrorMessageChanged` – Für diese Beispiel-app haben wir ein sehr einfaches Modell für die Fehlerbehandlung: eine Zeichenfolge mit dem letzten Fehler. 
- `HeartRateStored` : Wird ausgelöst, wenn eine Herzfrequenz in der Health-Kit-Datenbank gespeichert ist.

Beachten Sie, dass die, wenn diese Ereignisse ausgelöst werden, müssen diese erfolgt über `NSObject.InvokeOnMainThread()`, wodurch Abonnenten für die Benutzeroberfläche zu aktualisieren. Alternativ können die Ereignisse dokumentiert werden, als in Hintergrundthreads ausgelöst wird, und die Verantwortung für das Sicherstellen der Kompatibilität kann ihre Handler überlassen werden. Thread-Überlegungen sind in Health Kit Anwendungen wichtig, da viele der Funktionen, z. B. der berechtigungsanforderung asynchron sind, und führen Sie die Rückrufe für nicht-Main-Threads.

Die "heath"-Kit-spezifischen Code in `HeartRateModel` befindet sich in die beiden Funktionen `HeartRateInBeatsPerMinute()` und `StoreHeartRate()`. 

`HeartRateInBeatsPerMinute()` Konvertiert das Argument in eine stark typisierte Integrität Kit `HKQuantity`. Der Typ der Menge ist, die gemäß der `HKQuantityTypeIdentifierKey.HeartRate` und die von der Menge `HKUnit.Count` geteilt durch `HKUnit.Minute` (das heißt, der die Einheit ist *beats pro Minute*). 

Die `StoreHeartRate()` -Funktion nimmt ein `HKQuantity` (in der Beispiel-app einen erstellt, indem `HeartRateInBeatsPerMinute()` ). Um die Daten zu überprüfen, verwendet der `HKQuantity.IsCompatible()` -Methode, die gibt `true` Wenn in der Einheiten im Argument der objektspezifischen Einheiten konvertiert werden können. Wenn die Menge mit erstellt wurde `HeartRateInBeatsPerMinute()` natürlich als Ergebnis erhalten `true`, doch es wird auch zurückgegeben `true` , wenn die Menge, wie z. B. erstellt wurden *überlegen ist pro Stunde*. Häufiger `HKQuantity.IsCompatible()` kann verwendet werden, zum Überprüfen der Masse, Abstand und Energie, die der Benutzer oder ein Gerät die Eingabe oder Anzeige in einem System, der Maßeinheit (z. B. Imperial Einheiten), aber die möglicherweise in einem anderen System (z. B. die metrischen Einheiten) gespeichert werden. 

Sobald die Kompatibilität der Menge überprüft wurde, die `HKQuantitySample.FromType()` Factorymethode wird verwendet, um einen stark typisierten erstellen `heartRateSample` Objekt. `HKSample` -Objekte haben ein Datum Start- und Endzeit. für die sofortige Sensorenmesswerte enthalten sollten diese Werte entsprechen, wie im Beispiel. Das Beispiel auch ist nicht festgelegt alle Schlüssel-Wert-Daten die `HKMetadata` Argument, aber einer kann Code wie den folgenden Code verwenden, um Sensor Speicherort anzugeben:

```csharp
var hkm = new HKMetadata();
hkm.HeartRateSensorLocation = HKHeartRateSensorLocation.Chest;

```

Sobald die `heartRateSample` wurde erstellt, der Code erstellt eine neue Verbindung mit der Datenbank mit der Block. In diesem Block den `HKHealthStore.SaveObject()` Methode versucht, den asynchronen Schreibvorgang in der Datenbank. Der resultierende Aufruf an den Lambda-Ausdruck löst relevante Ereignisse aus, entweder `HeartRateStored` oder `ErrorMessageChanged`.

Nun, da das Modell so programmiert sind, ist es Zeit, wie der Controller für den Zustand des Modells widerspiegelt. Öffnen Sie die `HKWorkViewController.cs` Datei. Der Konstruktor einfach verbindet die `HeartRateModel` Singleton um Ereignisbehandlungsmethoden (in diesem Fall könnte dies Inline mit Lambda-Ausdrücken, aber separate Methoden machen den Zweck, die etwas offensichtlicher):

```csharp
public HKWorkViewController (IntPtr handle) : base (handle)
{
     HeartRateModel.Instance.EnabledChanged += OnEnabledChanged;
     HeartRateModel.Instance.ErrorMessageChanged += OnErrorMessageChanged;
     HeartRateModel.Instance.HeartRateStored += OnHeartBeatStored;
}

```

Hier sind die entsprechenden Handler:

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

Natürlich in einer Anwendung mit einem Controller, es wäre möglich, um die Erstellung eines separaten Model-Objekts und die Verwendung von Ereignissen für die ablaufsteuerung zu vermeiden, aber die Verwendung von Model-Objekte für WPF-Anwendungen besser geeignet ist.

## <a name="running-the-sample-app"></a>Ausführen der Beispielapp

IOS-Simulator unterstützt keine Integrität Kit. Debuggen muss auf einem physischen Gerät unter iOS 8 durchgeführt werden.

Anfügen eines entwicklungsgeräts ordnungsgemäß bereitgestellt iOS 8 auf Ihrem System. Wählen Sie sie als Bereitstellungsziel in Visual Studio für Mac, und wählen Sie im Menü **ausführen > Debuggen**.

> [!IMPORTANT]
> An diesem Punkt werden die Fehler im Zusammenhang mit der Bereitstellung auftreten. Um Fehler zu beheben, überprüfen Sie Sie erstellen und Bereitstellen von einem Integrität Kit App-Abschnitt oben. Die Komponenten sind: 
>
> - **iOS Developer Center** -explizite App-ID und die Integrität Kit aktiviert Bereitstellungsprofil. 
> - **Projektoptionen** -Bündel-ID (explizite App-ID) und Bereitstellungsprofil.
> - **Quellcode:** -"Entitlements.plist" und "Info.plist"

Vorausgesetzt, dass Vorschriften ordnungsgemäß festgelegt wurden, wird Ihre Anwendung zu starten. Bei Erreichen der `OnActivated` -Methode, er Integrität Kit Autorisierung anfordert. Beim ersten dieser vom Betriebssystem, Fehler aufgetreten ist, wird der Benutzer das folgende Dialogfeld angezeigt:


[![](healthkit-images/image12.png "Dieses Dialogfeld wird dem Benutzer angezeigt werden")](healthkit-images/image12.png#lightbox)

Ermöglichen Sie Ihrer app zum Aktualisieren von Daten mit Herz und Ihre app wird erneut angezeigt. Die `ReactToHealthCarePermissions` Rückruf asynchron aktiviert. Dies bewirkt, dass die `HeartRateModel’s` `Enabled` -Eigenschaft zu ändern, die ausgelöst wird die `EnabledChanged` -Ereignis, das bewirkt die `HKPermissionsViewController.OnEnabledChanged()` -Ereignishandler ausgeführt wird, wodurch die `StoreData` Schaltfläche. Das folgende Diagramm zeigt die Sequenz:


[![](healthkit-images/image13.png "Dieses Diagramm zeigt die Abfolge von Ereignissen")](healthkit-images/image13.png#lightbox)

Drücken Sie die **Datensatz** Schaltfläche. Dies bewirkt, dass die `StoreData_TouchUpInside()` Handler ausgeführt wird, die versuchen, den Wert des analysiert die `heartRate` -Text-Feld konvertieren in ein `HKQuantity` über die zuvor beschriebenen `HeartRateModel.HeartRateInBeatsPerMinute()` Funktion, und übergeben Sie diese Menge an `HeartRateModel.StoreHeartRate()`. Wie bereits zuvor erwähnt, dies wird versucht, die Daten zu speichern und löst entweder eine `HeartRateStored` oder `ErrorMessageChanged` Ereignis.

Doppelklicken Sie auf die **Startseite** auf Ihrem Gerät auf die Schaltfläche, und öffnen Sie die Integrität-app. Klicken Sie auf die **Quellen** Registerkarte, und Sie sehen die Beispiel-app aufgeführt. Wählen Sie diese, und nicht die zulassen Sie Berechtigung zum Aktualisieren von Daten mit Herz. Doppelklicken Sie auf die **Startseite** Schaltfläche und kehren zu Ihrer app zurück. Auch hier `ReactToHealthCarePermissions()` aufgerufen, aber dieses Mal verwenden, da der Zugriff verweigert wird, die **StoreData** Schaltfläche, deaktiviert (Beachten Sie, dass in diesem asynchron Fall und die Änderung in der Benutzeroberfläche ist möglicherweise für den Endbenutzer sichtbar).

## <a name="advanced-topics"></a>Weiterführende Themen

Lesen von Daten aus dem Health-Kit Datenbank ähnelt das Schreiben von Daten: eine gibt die Arten von Daten, die eine Autorisierung Anforderungen zugreifen möchten, und diese Autorisierung gewährt, sind die Daten mit automatische Konvertierung zu kompatiblen Einheiten verfügbar Measure.

Es gibt eine Reihe von komplexere Abfragefunktionen die ermöglichen, Prädikat-basierte Abfragen und Abfragen, die Updates ausführen, wenn relevante Daten aktualisiert werden. 

Entwickler von Anwendungen für Health-Kit prüfen den Health-Kit Teil von Apple [App-Richtlinien überprüfen](https://developer.apple.com/app-store/review/guidelines/#healthkit).

Nachdem die Sicherheits- und das Typsystem Modelle zu verstehen sind, ist das Speichern und Lesen von Daten in der freigegebenen Datenbank für den Health-Kit ziemlich einfach. Viele der Funktionen in Health Kit asynchron ausgeführt werden, und Anwendungsentwickler müssen entsprechend ihrer Programme schreiben.

Zum Zeitpunkt der Verfassung dieses Artikels gibt es derzeit keine Entsprechung Integrität Kit, Android oder Windows Phone.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wir gesehen haben, wie der Health-Kit Anwendungen das speichern können Informationen abrufen und die Systemüberwachung der Dateifreigabe, und bietet gleichzeitig eine standard-Integrität-app, die den Benutzerzugriff und die Kontrolle über diese Daten kann. 

Wir haben auch gesehen, wie Datenschutz, Sicherheit und Integrität Sicherheitsrisiken für integritätsbezogene Informationen überschreiben und apps mithilfe des Integritäts-Kit müssen mit dem Anstieg in der Komplexität bei der Anwendung Verwaltungsaspekte (Bereitstellung), (Integrität Kitss Typ Codierung verarbeiten -System), und die benutzererfahrung (Benutzersteuerelement von Berechtigungen über die System-Dialogfelder und Integrität app). 

Schließlich haben wir einen Blick auf eine einfache Implementierung der Health-Kit mit der enthaltenen Beispiel-app, die heartbeatdaten in den Health-Kit-Speicher geschrieben und verfügt über einen asynchronen unterstützenden Entwurf.

## <a name="related-links"></a>Verwandte Links

- [HKWork (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios8/IntroToHealthKit/)
- [Einführung in iOS 8](~/ios/platform/introduction-to-ios8.md)
