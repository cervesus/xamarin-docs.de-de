---
title: Touch ID
description: Touch ID handelt es sich um Apple Fingerabdruck biometrische authentifizierungstechnologie.
ms.topic: article
ms.prod: xamarin
ms.assetid: 4BC8EFD6-52FC-4793-BA69-D6BFF850FE5F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 3564b4f7d41822fdd9ab167fb3e756f26678a17b
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
---
# <a name="touch-id"></a>Touch ID

Touch ID wurde als Mittel zum Authentifizieren des Benutzers - ähnlich wie eine Kennung in iOS 7 eingeführt. Es wurde jedoch zum Entsperren des Geräts, den App Store, mithilfe der iTunes und Authentifizieren der iCloud-Schlüsselbund beschränkt.

Es sind nun zwei Möglichkeiten zur Verwendung von Touch ID als Authentifizierungsmechanismus in einer iOS 8-Anwendung mithilfe der lokalen Authentifizierung-API. Es ist derzeit nicht möglich, lokale Authentifizierung zu verwenden, um die Remote authentifiziert werden.

Um Touch ID und die folglich vollständig zu verstehen, sollten wir Schlüsselbund Services durchsuchen und die Bedeutung dieser neuen Änderungen für Daten des Benutzers. Schlüsselbundverwaltung wurde auch in iOS 8 durch die Verwendung der neuen Funktion von Zugriffssteuerungslisten (ACLs) auf Erweitert.

## <a name="keychain--secure-enclave"></a>Schlüsselbund & sicheren Enclave

Schlüsselsammlung ist eine große Datenbank bereitstellen sicheren Speicher für Kennwörter, Schlüssel, Zertifikate und Hinweise für eine einzelne Apple-ID. In iOS 8 eine Anwendung immer hat Zugriff auf die eigenen eindeutigen Schlüsselbund-Elemente und sind nicht verfügbar Schlüsselbund Elemente anderer Anwendungen. Dies unterscheidet sich von OS X, in dem die Schlüsselkette mit einem einzigen Kennwort entsperrt wird, durch alle Keychain-Services-fähige Anwendung, die den keychain-Wert zu verwenden. In diesem Artikel konzentrieren wir uns auf die Funktionsweise von Schlüsselbund in iOS 8.

Schlüsselsammlung ist eine spezielle Datenbank, in dem jede Zeile wird als bezeichnet eine _Schlüsselbund_. Jedes Element wird durch Schlüsselbund Attribute beschrieben, und setzt sich aus der verschlüsselten Werte. Um effiziente Verwendung von Schlüsselbund zuzulassen, ist optimiert für kleine Elemente oder _geheime Schlüssel_.
Jedes Element des keychain-Wert wird durch die Benutzer-Kennung und eine eindeutige Geräte-Schlüssel geschützt. Schlüsselbund Elemente sollten geschützt werden, auch wenn kein Benutzer ihre Geräte verwendet. Dies ist in iOS implementiert, indem Sie nur die Elemente zur Verfügung, wenn das Gerät entsperrt wird zulassen – das Gerät gesperrt ist sie sind nicht verfügbar. Sie können auch in einer verschlüsselten Sicherung gespeichert werden. Eine der wichtigsten Funktionen der Schlüsselsammlung ist Zugriffssteuerung zu erzwingen. eine Anwendung hat Zugriff auf seine Teil den keychain-Wert, und alle anderen Anwendungen werden verhindert. Das folgende Diagramm veranschaulicht die Interaktion einer Anwendung mit den keychain-Wert:

[![](touchid-images/image1.png "Dieses Diagramm veranschaulicht die Interaktion einer Anwendung mit den keychain-Wert")](touchid-images/image1.png#lightbox)

### <a name="secure-enclave"></a>Sichere Enclave

Schlüsselbund kann nicht allein das Schlüsselbund-Element entschlüsselt; Stattdessen erfolgt der *Secure Enclave*. Sichere Enclave ist ein CO-Prozessor in den A7-Chip, der zur Bestimmung einer erfolgreichen Übereinstimmung von den Fingerabdruckdaten der Sensor Touch ID für einen registrierten Druck zuständig ist. Anschließend wird er das Schlüsselbund-Element zu entschlüsseln und den entschlüsselten geheimen Schlüssel an den keychain-Wert zurückgeben.

### <a name="working-with-keychain"></a>Arbeiten mit Schlüsselbund

Zuerst sollte Ihre Anwendung Abfragen, in den keychain-Wert zu überprüfen, ob ein Kennwort vorhanden ist. Wenn es nicht vorhanden ist, müssen Sie möglicherweise nach einem Kennwort gefragt werden, damit der Benutzer ständig gebeten wird nicht. Wenn das Kennwort werden aktualisiert muss, den Benutzer ein neues Kennwort auffordern und in den aktualisierten Wert an den keychain-Wert übergeben.

> [!NOTE]
> Nachdem über den keychain-Wert mit einem geheimen Schlüssel abgerufen werden, sollten alle Verweise auf die Daten aus dem Arbeitsspeicher gelöscht werden. Nie weisen sie eine globale Variable.

## <a name="keychain-acl-and-touch-id"></a>Schlüsselbund ACL und Touch ID

Access Control List ist ein neues Schlüsselbund-Element-Attribut in iOS 8, die beschreibt, das Informationen darüber, was geschehen muss, um einen bestimmten Vorgang auftreten zu ermöglichen. Dies ist möglicherweise in Form von eine Warnung anzeigen oder das Anfordern einer Kennung. ACL können Sie den Zugriff und die Authentifizierung für einen Schlüsselbund Element festlegen. Das folgende Diagramm zeigt, wie dieses neue Attribut mit dem Rest des Elements Schlüsselbund verknüpft:

[![](touchid-images/image2.png "Dieses Diagramm zeigt, wie dieses neue Attribut mit dem Rest des Elements Schlüsselbund verknüpft")](touchid-images/image2.png#lightbox)

Seit iOS 8 ist jetzt eine neue Vorhandensein Benutzerrichtlinie `SecAccessControl`, wird die von der sicheren Enclave auf einem iPhone 5s und höher erzwungen. Wir sehen, in der folgenden Tabelle zusammengestellt einfach wie die Gerätekonfiguration richtlinienauswertung beeinflussen kann:

|Gerätekonfiguration|Auswertung von Richtlinien|Sicherungsverfahren|
|--- |--- |--- |
|Gerät ohne Kennung|Kein Zugriff|Keiner|
|Gerät mit der Kennung|Erfordert die Kennung|Keiner|
|Gerät mit Touch ID|Touch ID bevorzugt|Ermöglicht der Kennung|

Alle Vorgänge in der sicheren Enclave können voneinander als vertrauenswürdig eingestuft. Dies bedeutet, dass wir das Authentifizierungsergebnis Touch ID verwenden können, um die Entschlüsselung der Schlüsselsammlung-Element zu autorisieren. Die sichere Enclave bleiben einen Zähler der fehlgeschlagenen Übereinstimmungen von Touch ID, in denen Fall ein Benutzer mit der Kennung zurücksetzen muss.
Ein neues Framework in iOS 8 aufgerufen _lokale Authentifizierung_, dieser Vorgang der Authentifizierung innerhalb des Geräts unterstützt. Wir untersuchen dies im nächsten Abschnitt.

## <a name="local-authentication"></a>Lokale Authentifizierung

Wie wir im vorherigen Abschnitt eingerichtet haben, können Anwendungen für die lokale Authentifizierung zum Authentifizieren des Benutzers in die Einhaltung der Sicherheitsrichtlinie, die auf dem Gerät eingerichtet wurde.

Die API ermöglicht den derzeit nur zwei Funktionen: Erstens, sie die vorhandenen Dienste für die Schlüsselsammlung durch die Verwendung von neuen Schlüsselbund Zugriffssteuerungslisten (ACLs) unterstützt. Schlüsselbund Daten können mit der erfolgreichen Authentifizierung Benutzer Fingerabdruck entsperrt werden.

Zweitens stellt LocalAuthentication zwei Methoden, um die Anwendung lokal zu authentifizieren. Entwickler sollten verwenden `CanEvaluatePolicy` zu bestimmen, ob das Gerät zur Annahme von Touch ID fähig ist und dann `EvaluatePolicy` Starten des Vorgangs für die Authentifizierung.

Während beide Funktionen für die lokalen Authentifizierung bieten, aber es einen Mechanismus für die Anwendung oder der Benutzer zur Authentifizierung mit einem Remoteserver nicht bereitgestellt.
Lokaler Authentifizierung bietet eine neue standard-Benutzeroberfläche für die Authentifizierung an. Im Fall von Touch ID ist dies eine Warnungsansicht mit zwei Schaltflächen, wie unten gezeigt. Eine Schaltfläche "Abbrechen", und Authentifizierung – die Kennung fallback überprüfbarer verwendet. Es gibt auch eine benutzerdefinierte Meldung, die festgelegt werden muss. Es wird empfohlen, dies verwenden, um dem Benutzer erklären, warum Touch ID Authentifizierung erforderlich ist.

[![](touchid-images/image12.png "Touch ID Authentifizierung Warnung")](touchid-images/image12.png#lightbox)

### <a name="with-keychain-services"></a>Mit Schlüsselbund-Diensten

Erläutert, ein wenig zuvor wie eine Schlüsselbund wird entschlüsselt, mithilfe der sicheren Enclave unsere Kennung zu überprüfen. In iOS 8 verwenden wir das lokale Authentifizierung, zum Anfordern von Touch ID Überprüfung in Verbindung mit dem Feature Access Control Lists, die die Implementierung des fallback-Mechanismus oder das Kennwort bereitstellt.
ACL verwenden, sollten wir verwenden, die `SecAccessControl` Richtlinie, und überprüfen den Status des Geräts mit `SecAccessible.WhenPasscodeSetThisDeviceOnly` oder `SecAccessible.WhenUnlocked`.

#### <a name="considerations-with-acl"></a>Überlegungen zur ACL

Es gibt viele Dinge, die wir im Hinterkopf behalten soll, wenn die Schlüsselsammlung ACL mit, und einige davon sind nachfolgend aufgeführt:

-   Verwenden Sie nur mit Anwendung im Vordergrund – Wenn Sie jeden Keychain-Vorgang in einem Hintergrundthread aufrufen, die der Aufruf fehl.
-   Hinzufügen und Aktualisieren der Schlüssel können eine Authentifizierung erforderlich ist.
-   Wenn eine Anforderung mehrere übereinstimmende Elemente in den keychain-Wert zurückgibt, möglicherweise Authentifizierung erforderlich.
-   ACL geschützte Elemente sind nur für Geräte, und daher nicht synchronisiert oder gesichert werden.

### <a name="using-local-authentication-without-keychain-services"></a>Mithilfe der lokalen Authentifizierung ohne Schlüsselbund-Dienste

Lokaler Authentifizierung wurde als eine Möglichkeit, Anmeldeinformationen, z. B. Kennung oder Touch ID zu sammeln und zur Bearbeitung der sicheren Enclave Authentifizieren des Benutzers beendet. Vorstellen des Zertifikats als Brücke zwischen der Anwendung und die Enclave sichern, die nie direkt miteinander kommunizieren können. Sie können auch für die richtlinienauswertung für Ihre Anwendung verwendet werden.

Zu diesem Zweck, dass eine Anwendung ruft die richtlinienauswertung in lokale Authentifizierung, wodurch den Vorgang im Secure Enclave gestartet wird. Sie können diese Option, um die Authentifizierung Ihrer App zu ermöglichen, ohne direkten Abfragen/Zugriff auf die sicheren Enclave nutzen.

[![](touchid-images/image13a.png "Mithilfe der lokalen Authentifizierung ohne Schlüsselbund-Dienste")](touchid-images/image13a.png#lightbox)

Mithilfe der lokalen Authentifizierung in Ihrer Anwendung stellt eine einfache Möglichkeit zum Implementieren der Überprüfung des Benutzers, z. B. ein Feature für die Augen der Besitzer des Geräts, z. B. bankanwendungen auf, oder klicken Sie auf Hilfe Jugendschutz ausschließlich für die einzelnen zu schaffen die Anwendung. Sie können es auch verwenden, als eine Möglichkeit zur Authentifizierung Erweiterung, die bereits vorhanden ist – Benutzer wie seine Informationen sicher, doch sie auch gerne Optionen.

Die Sicherheit der lokalen Authentifizierung unterscheidet sich von dem von den keychain-Wert. Z. B. wenn die Schlüsselsammlung zu verwenden, ist die Vertrauensstellung zwischen dem Betriebssystem und die Enclave Secure. Mit der lokalen Authentifizierung ist es zwischen der Anwendung und das Betriebssystem, das bedeutet, dass Sie nur Zugriff auf die Ergebnisse des Secure Enclave, nicht die sichere Enclave selbst.

Auf das Thema Sicherheit, es ist auch äußerst wichtig zu wissen, dass es **kein Zugriff** registrierten Finger oder Fingerabdruck Bilder. Die sichere Enclave ist der Besitzer diese Informationen und keine andere Systemkomponente besitzt somit den Zugriff darauf.

Um Touch ID ohne Schlüsselbund durch die Nutzung der lokalen Authentifizierung-API zu verwenden, gibt es einige Funktionen, die es verwenden können. Diese werden unten genauer beschrieben:

*   `CanEvaluatePolicy` – Dies wird einfach überprüfen, um festzustellen, ob das Gerät zur Annahme von Touch ID fähig ist
*   `EvaluatePolicy` – Dies startet den Authentifizierungsvorgang und zeigt die Benutzeroberfläche an und gibt eine `true` oder `false` Antwort.
*   `DeviceOwnerAuthenticationWithBiometrics` – Dies ist die Richtlinie, die verwendet werden kann, um den Bildschirm Touch ID anzuzeigen. Es ist zu beachten, dass es keine Kennung Fallbackmechanismus hier, Sie sollten stattdessen diese Vorgehensweise implementieren, in der Anwendung, damit Benutzer die Touch ID Authentifizierung überspringen können.

Es gibt einige Vorsichtsmaßnahmen bei unter Verwendung der lokalen Authentifizierung, die unten aufgeführt sind:

*   Wie bei Schlüsselbund, kann es nur im Vordergrund ausgeführt werden. Aufrufen dieser in einem Hintergrundthread führt dazu, dass es zu Fehlern werden.
*   Bedenken Sie, die die richtlinienauswertung nicht durchgeführt werden kann. Eine Schaltfläche Kennung müssen als eine Webrollen implementiert werden.
*   Geben Sie an einer `localizedReason` zu erklären, warum die Authentifizierung erforderlich ist. Dadurch wird die um Vertrauensstellung mit dem Benutzer zu erstellen.

Als Nächstes wird im Abschnitt unten wir zum Implementieren der API unter Berücksichtigung dieser Einschränkungen betrachten.

## <a name="adding-touch-id-to-your-application"></a>Touch ID der Anwendung hinzufügen

In den vorherigen Abschnitten wurde der Theorie Zugriffs und der Authentifizierung über Schlüsselbund und lokalen Authentifizierung. Sehen wir uns ansehen, wie Sie Touch ID in der Anwendung integrieren können.

### <a name="walkthrough"></a>Exemplarische Vorgehensweise

Daher betrachten wir dazu unsere Anwendung einige Touch ID Authentifizierung hinzugefügt. In dieser exemplarischen Vorgehensweise werden wir verwenden die [Storyboard Tabelle](https://developer.xamarin.com/samples/StoryboardTable/) Beispiel. Wir möchten sicherstellen, dass nur der Besitzer des Geräts etwas in dieser Liste hinzufügen kann, wir nicht unnötig, da jeder Benutzer ein Element hinzuzufügen können möchten!

1.  Laden Sie das Beispiel und führen Sie es in Visual Studio für Mac.
2.  Doppelklick auf `MainStoryboard.Storyboard` um das Beispiel in der iOS-Designer zu öffnen. In diesem Beispiel möchten wir, die Authentifizierung gesteuert wird vorliegenden Anwendung einen neuen Bildschirm hinzufügen. Dies geht vor der aktuellen `MasterViewController`.
3.  Ziehen Sie ein neues **Modellansichtcontroller** aus der **Toolbox** auf die **Entwurfsoberfläche**. Legen Sie diese als die **Root Modellansichtcontroller** von **STRG + Drag &** aus der **Navigation Controller**:

    [![](touchid-images/image4.png "Legen Sie die Stamm-View-Controller")](touchid-images/image4.png#lightbox)
4.  Nennen Sie die neue Ansicht Controller `AuthenticationViewController`.
5.  Ziehen Sie eine Schaltfläche, und platzieren Sie es auf die `AuthenticationViewController`. Rufen Sie diese `AuthenticateButton`, und weisen Sie ihm den Text `Add a Chore`.
6.  Erstellen Sie ein Ereignis auf der `AuthenticateButton` aufgerufen `AuthenticateMe`.
7.  Erstellen Sie eine manuelle aus segue `AuthenticationViewController` durch Klicken auf der schwarzen Leiste am unteren und **STRG + Drag &** in der Menüleiste auf die `MasterViewController` auswählen und **Push** (oder **anzeigen** Größenklassen verwenden):

    [![](touchid-images/image5.png "Ziehen Sie in der Menüleiste auf die MasterViewController und Push auswählen oder anzeigen")](touchid-images/image6.png#lightbox)
8.  Klicken Sie auf den neu erstellten segue und weisen Sie ihm den Bezeichner `AuthenticationSegue`, wie unten gezeigt:

    [![](touchid-images/image7.png "Den Bezeichner für die Segue AuthenticationSegue festgelegt")](touchid-images/image7.png#lightbox)
9.  Fügen Sie den folgenden Code zu `AuthenticationViewController` hinzu:

    ```
    partial void AuthenticateMe (UIButton sender)
        {
            var context = new LAContext();
            NSError AuthError;
            var myReason = new NSString("To add a new chore");


            if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, out AuthError)){
                var replyHandler = new LAContextReplyHandler((success, error) => {

                    this.InvokeOnMainThread(()=>{
                        if(success){
                            Console.WriteLine("You logged in!");
                            PerformSegue("AuthenticationSegue", this);
                        }
                        else{
                            //Show fallback mechanism here
                        }
                    });

                });
                context.EvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, myReason, replyHandler);
            };
        }
    ```

Dies ist der Code, den Sie Touch ID-Authentifizierung mit der lokalen Authentifizierung implementieren müssen. Die hervorgehobenen Zeilen in der folgenden Abbildung veranschaulichen die Verwendung der lokalen Authentifizierung:

[![](touchid-images/image8.png "Die hervorgehobenen Zeilen veranschaulichen die Verwendung der lokalen Authentifizierung")](touchid-images/image8.png#lightbox)

Zunächst muss hergestellt werden, wenn das Gerät die Fähigkeit akzeptieren Touch ID Eingabe mithilfe der `CanEvaluatePolicy` und die Richtlinie übergeben `DeviceOwnerAuthenticationWithBiometrics`. Wenn dies zutrifft, und klicken Sie dann wir können die Touch ID-Benutzeroberfläche anzeigen, indem Sie mit `EvaluatePolicy`. Es gibt drei Arten von Informationen, die wir übergeben haben `EvaluatePolicy` – die Richtlinie selbst, eine Zeichenfolge, die erläutern, warum die Authentifizierung erforderlich ist und eine Antwort-Handler. Die Antwort-Handler weist die Anwendung bei einer Authentifizierung erfolgreich oder nicht erfolgreich ist, was geschehen soll. Sehen wir uns genauer an den Handler für die Antwort:

[![](touchid-images/image9.png "Die Antwort-handler")](touchid-images/image9.png#lightbox)

Die Antwort-Handler angegeben ist, vom Typ `LAContextReplyHandler`, der den Erfolg Parameter – akzeptiert eine `bool` Wert, und ein `NSError` aufgerufen `error`. Wenn er erfolgreich ist, ist dies, wo es tatsächlich ausgeführt werden nach Belieben es ist, möchten wir authentifizieren – in diesem Fall Anzeige den Bildschirm, der wir eine neue Aufgabe hinzugefügt wird. Denken Sie daran zu den Einschränkungen der lokalen Authentifizierung ist, im Vordergrund ausführen, achten Sie unbedingt Transportservers `InvokeOnMainThread`:

[![](touchid-images/image10.png "Verwenden Sie für die lokale Authentifizierung InvokeOnMainThread")](touchid-images/image10.png#lightbox)

Zum Schluss die Authentifizierung erfolgreich verlaufen ist, möchten wir für den Übergang in den `MasterViewController`. Die `PerformSegue` Methode kann dazu verwendet werden:

[![](touchid-images/image11.png "Rufen Sie PerformSegue-Methode für den Übergang in den MasterViewController")](touchid-images/image11.png#lightbox)

## <a name="summary"></a>Zusammenfassung
In diesem Handbuch erläutert wird, Schlüsselbund und Funktionsweise in iOS. Wir auch Schlüsselbund ACL, untersucht und Änderung dieser in iOS. Als Nächstes haben wir einen Blick auf die lokale Authentifizierung Framework ist neu in iOS 8, und klicken Sie dann implementieren Touch ID Authentifizierung in der vorliegenden Anwendung betrachtet.

## <a name="related-links"></a>Verwandte Links

- [Touch ID-Beispiel](https://developer.xamarin.com/samples/StoryboardTable_LocalAuthentication)
- [Schlüsselbund WWDC-Beispiel](https://developer.xamarin.com/samples/KeychainTouchID/)
- [Schlüsselbund (Beispiel)](https://developer.xamarin.com/samples/Keychain/)
