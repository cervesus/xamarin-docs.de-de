---
title: Touch-ID in Xamarin.iOS
description: Dieses Dokument beschreibt, wie Sie Touch ID authentifizierungstechnologie für Apple Biometrisches Fingerabdruck, in Xamarin.iOS-apps verwenden.
ms.prod: xamarin
ms.assetid: 4BC8EFD6-52FC-4793-BA69-D6BFF850FE5F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: 2d67bc71361e335515cfba8b5a20e157ed6b6b05
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114339"
---
# <a name="touch-id-in-xamarinios"></a>Touch-ID in Xamarin.iOS

Touch ID wurde in iOS 7 als Mittel zum Authentifizieren des Benutzers, um eine Kennung ähnlichen eingeführt. Es war jedoch auf die entsperrung des Geräts, verwenden die App-Store, mithilfe von iTunes und iCloud-Schlüsselbund nur Authentifizierung beschränkt.

Es gibt jetzt zwei Möglichkeiten, Touch ID als Authentifizierungsmechanismus in einer iOS 8-Anwendung, die mit der lokalen Authentifizierungs-API verwenden. Es ist derzeit nicht möglich, die lokale Authentifizierung verwenden, um die Remote authentifiziert.

Um Touch ID und lohnt vollständig verstehen zu können, sollten wir Keychain-Services erkunden und ihre Bedeutung dieser neuen Änderungen für die Daten des Benutzers. Keychain-Zugriff wurde auch auf IOS 8 durch die Verwendung der neuen Funktion von Zugriffssteuerungslisten (ACLs) erweitert.

## <a name="keychain--secure-enclave"></a>Keychain & sichere Enclave

Keychain ist eine große Datenbank bereitstellen sicheren Speicher für Kennwörter, Schlüssel, Zertifikate und Anmerkungen zu dieser Version für eine einzelne Apple-ID IOS 8 eine Anwendung immer hat Zugriff auf die eigene eindeutige Schlüssel zu, und kann nicht auf alle Keychain-Elemente von anderen Anwendungen zugreifen. Dies unterscheidet sich von OS X, in dem die Keychain mit einem einzigen Kennwort, entsperrt ist, eine Keychain Services unterstützende Anwendung, die den Schlüsselbund verwenden lassen. In diesem Artikel konzentrieren wir uns auf die Funktionsweise der Schlüsselbund in iOS 8.

Keychain ist eine spezielle Datenbank, in dem jede Zeile wird als bezeichnet ein _Schlüsselbund_. Jedes Element wird durch den Keychain-Attribute beschrieben und besteht aus der verschlüsselten Werte. Um für die effiziente Nutzung der Keychain zu ermöglichen, ist optimiert für kleine Elemente oder _Geheimnisse_.
Jedes Element des Keychain wird durch die Kennung von Benutzern und eindeutigen geheimen Geräteschlüssel geschützt. Keychain-Elemente sollten geschützt werden, selbst wenn Benutzer ihre Geräte nicht verwenden. Dies wird in iOS implementiert, indem nur die Elemente, die verfügbar, wenn das Gerät entsperrt wird zugelassen, wenn das Gerät gesperrt ist sind sie nicht mehr verfügbar. Sie können auch in einer verschlüsselten Sicherung gespeichert werden. Eines der wichtigsten Features der Keychain ist die Zugriffssteuerung zu erzwingen. eine Anwendung hat Zugriff auf den jeweiligen Teil der Keychain aus, und alle anderen Anwendungen werden verhindert. Das folgende Diagramm veranschaulicht, wie eine Anwendung mit der Keychain interagiert:

[![](touchid-images/image1.png "Dieses Diagramm veranschaulicht die Interaktion einer Anwendung mit der keychain")](touchid-images/image1.png#lightbox)

### <a name="secure-enclave"></a>Sichere Enclave

Die Keychain kann nicht den Keychain-Element selbst entschlüsselt werden; Stattdessen erfolgt es in der *sichere Enclave*. Die sichere Enclave wird ein CO-Prozessor in den A7-Chip, der zum Bestimmen einer Übereinstimmung von Fingerabdruckdaten vom Sensor Touch ID für einen registrierten Druck zuständig ist. Anschließend wird er der Keychain-Element zu entschlüsseln und den entschlüsselten geheimen Schlüssel im Schlüsselbund zurück.

### <a name="working-with-keychain"></a>Arbeiten mit Keychain

Zunächst sollte die Anwendung Abfragen, in die Keychain aus, um festzustellen, ob ein Kennwort vorhanden ist. Wenn es nicht vorhanden ist, müssen Sie möglicherweise zur Kennworteingabe aufgefordert werden, damit der Benutzer immer wieder gestellt wird. Wenn das Kennwort werden aktualisiert muss, vom Benutzer ein neues Kennwort ein, und übergeben Sie den aktualisierten Wert im Schlüsselbund.

> [!NOTE]
> Nachdem mithilfe eines geheimen Schlüssels aus dem Schlüsselbund abgerufen werden, sollten alle Verweise auf die Daten aus dem Speicher geleert werden. Weisen Sie es nie zu einer globalen Variablen.

## <a name="keychain-acl-and-touch-id"></a>Keychain-ACL und Touch ID

Zugriffssteuerungsliste ist ein neues Keychain-Element-Attribut in iOS 8, die beschreibt, die Informationen darüber, was geschehen muss, um einen bestimmten Vorgang auftreten zu ermöglichen. Dies ist möglicherweise im Formular anzeigen eines Warndialogfelds oder Anfordern einer Kennung. ACL können Sie den Zugriff auf und Authentifizierung für einen Schlüsselbund Element festgelegt. Das folgende Diagramm zeigt, wie dieses neue Attribut mit dem Rest des Keychain-Elements gebunden:

[![](touchid-images/image2.png "Dieses Diagramm zeigt, wie dieses neue Attribut mit den restlichen das Schlüssel-Objekt verknüpft")](touchid-images/image2.png#lightbox)

Ab iOS 8 ist jetzt eine neue Richtlinie für Benutzer vorhanden, `SecAccessControl`, die durch die sichere Enclave auf ein iPhone 5 s und höher erzwungen wird. Wir können sehen, in der Tabelle unten einfach wie die Gerätekonfiguration richtlinienauswertung beeinflussen kann:

|Gerätekonfiguration|Richtlinienauswertung|Mechanismus für Sicherungen|
|--- |--- |--- |
|Gerät ohne Kennung|Kein Zugriff|Keine|
|Gerät mit der Kennung|Erfordert die Kennung|Keine|
|Gerät mit Touch ID|Touch ID bevorzugt|Ermöglicht der Kennung|

Alle Vorgänge in der sicheren Enclave können voneinander als vertrauenswürdig eingestuft. Dies bedeutet, dass wir das Authentifizierungsergebnis Touch ID verwenden können, um die Entschlüsselung der Keychain-Element zu autorisieren. Die sichere Enclave speichert auch einen Zähler des fehlgeschlagenen Übereinstimmungen von Touch ID, in denen Fall einen Benutzer wieder mit der Kennung aufweist.
Ein neues Framework in iOS 8 namens _lokale Authentifizierung_, unterstützt diese Art der Authentifizierung innerhalb des Geräts. Wir untersuchen dies im nächsten Abschnitt.

## <a name="local-authentication"></a>Lokale Authentifizierung

Wie wir im vorherigen Abschnitt eingerichtet haben, können Anwendungen für die lokale Authentifizierung zum Authentifizieren des Benutzers in die Einhaltung der Sicherheitsrichtlinie, die auf dem Gerät eingerichtet wurde.

Die API bietet derzeit nur zwei Funktionen: Erstens unterstützt Sie die vorhandenen Keychain-Dienste durch die Verwendung von neuen Keychain Zugriffssteuerungslisten (ACLs). Keychain-Daten können mit der erfolgreichen Authentifizierung eines Fingerabdrucks Benutzer entsperrt werden.

Zweitens LocalAuthentication zwei stellt Methoden bereit, um die Anwendung lokal zu authentifizieren. Entwickler sollten verwenden `CanEvaluatePolicy` zu bestimmen, ob das Gerät Touch ID akzeptieren kann, und klicken Sie dann `EvaluatePolicy` , starten Sie den Authentifizierungsvorgang.

Beide Funktionen für die lokalen Authentifizierung sind, doch bieten sie nicht über einen Mechanismus für die Anwendung oder den Benutzer zur Authentifizierung mit einem Remoteserver.
Lokaler Authentifizierung bietet eine neue standard-Benutzeroberfläche für die Authentifizierung. Im Fall von Touch ID verwenden ist dies eine Warnungsansicht mit zwei Schaltflächen, wie unten dargestellt. Eine Schaltfläche "Abbrechen", und ein bis der fallback bedeutet, dass der Authentifizierung – die Kennung verwenden. Es gibt auch eine benutzerdefinierte Meldung, die festgelegt werden muss. Es wird empfohlen, diese verwenden, um dem Benutzer erklären, warum die Authentifizierung mit Touch ID erforderlich ist.

[![](touchid-images/image12.png "Die Touch ID-Authentifizierung-Warnung")](touchid-images/image12.png#lightbox)

### <a name="with-keychain-services"></a>Mit Keychain-Diensten

Wir haben uns angeschaut vorhin wie ein Keychain-Element wird entschlüsselt, mithilfe der sicheren Enclave unsere Kennung zu überprüfen. In iOS 8 können wir die lokale Authentifizierung verwenden, zum Anfordern von Touch ID-Überprüfung in Verbindung mit dem Feature Access Control Lists, die die Implementierung des Fallbackmechanismus oder des Kennworts bereitstellt.
ACL verwenden, sollten wir verwenden, die `SecAccessControl` Richtlinie, und aktivieren Sie dann den Status des Geräts mit `SecAccessible.WhenPasscodeSetThisDeviceOnly` oder `SecAccessible.WhenUnlocked`.

#### <a name="considerations-with-acl"></a>Überlegungen zu mit der ACL

Es gibt viele Dinge, die wir bedenken sollten, wenn die Keychain ACL mit ein, und einige davon sind unten aufgeführt:

-   Verwenden Sie nur mit Vordergrundanwendung – Wenn Sie jeden Keychain-Vorgang in einem Hintergrundthread aufrufen, die der Aufruf fehl.
-   Hinzufügen und Aktualisieren von Keychain-Elementen können eine Authentifizierung erforderlich ist.
-   Wenn eine Anforderung mehrere übereinstimmende Elemente in der Keychain zurückgibt, kann die Authentifizierung erforderlich sein.
-   ACL geschützte Elemente sind nur für Geräte, und daher nicht synchronisiert oder gesichert werden.

### <a name="using-local-authentication-without-keychain-services"></a>Mithilfe der lokalen Authentifizierung ohne Keychain-Dienste

Lokaler Authentifizierung wurde als eine Möglichkeit zum Sammeln von Anmeldeinformationen, z. B. Kennung oder Touch ID und arbeiten Sie mit der sicheren Enclave Abschließen der Authentifizierung des Benutzers erstellt. Betrachten sie als Brücke zwischen der Anwendung und die sichere Enclave, die nie direkt miteinander kommunizieren können. Sie können auch für die richtlinienauswertung für Ihre Anwendung verwendet werden.

Ruft dazu eine Anwendung die richtlinienauswertung in lokalen-Authentifizierung, die Beginn des Vorgangs in sichere Enclave. Sie können diese Option, um die Authentifizierung zu Ihrer app zu ermöglichen, ohne direkt Abfragen/den Zugriff auf die sichere Enclave nutzen.

[![](touchid-images/image13a.png "Mithilfe der lokalen Authentifizierung ohne Keychain-Dienste")](touchid-images/image13a.png#lightbox)

Mithilfe der lokalen Authentifizierung in Ihrer Anwendung bietet eine einfache Möglichkeit der Implementierung der Überprüfung des Benutzers, z. B. um ein Feature nur für die Augen der Eigentümer des Geräts, z. B. bankanwendungen, oder klicken Sie auf den Hilfe-Jugendschutz für die einzelnen zu entsperren. die Anwendung. Sie können es auch verwenden, als eine Möglichkeit, Authentifizierung, die bereits vorhanden ist, erweitern – Benutzer wie ihre Informationen sicher, aber sie möchten auch Optionen zur Verfügung.

Die Sicherheit der lokalen Authentifizierung unterscheidet sich von der Schlüsselbund. Beispielsweise ist bei Verwendung die Keychain die Vertrauensstellung zwischen dem Betriebssystem und die sichere Enclave. Mit der lokalen Authentifizierung ist es zwischen der Anwendung und das Betriebssystem, was bedeutet, dass Sie nur Zugriff auf die Ergebnisse der sicheren Enclave, nicht die sichere Enclave selbst haben.

Zu diesem Thema Sicherheit, es ist auch äußerst wichtig zu wissen, dass es **kein Zugriff** registrierten Finger oder Fingerabdruck-Images. Die sichere Enclave ist der Besitzer dieser Informationen ein, und daher keine anderen Systemkomponente kann darauf zugreifen.

Um Touch ID ohne Keychain durch die Nutzung der lokalen Authentifizierungs-API zu verwenden, gibt es einige Funktionen, die wir verwenden können. Diese werden nachfolgend ausführlich erläutert:

*   `CanEvaluatePolicy` – Dies wird einfach überprüfen, um festzustellen, ob das Gerät Touch ID. akzeptieren kann
*   `EvaluatePolicy` – Dies startet den Authentifizierungsvorgang wird die Benutzeroberfläche angezeigt und gibt eine `true` oder `false` Antwort.
*   `DeviceOwnerAuthenticationWithBiometrics` – Dies ist die Richtlinie, die verwendet werden kann, um die Touch ID-Bildschirm anzuzeigen. Es ist erwähnenswert, dass kein Kennung-fallback-Mechanismus hier vorhanden ist, sollten Sie stattdessen diese Vorgehensweise implementieren, in der Anwendung für Benutzer die Touch ID Authentifizierung überspringen können.

Es gibt einige Einschränkungen bei der Verwendung der lokalen Authentifizierung, die unten aufgeführt sind:

*   Wie bei Keychain, kann es nur im Vordergrund ausgeführt werden. Aufruf in einem Hintergrundthread wird Fehler verursachen.
*   Bedenken Sie, die die richtlinienauswertung fehlschlagen kann. Eine Schaltfläche "Kennung" wird als ein Fallback implementiert werden müssen.
*   Geben Sie an einer `localizedReason` zu erklären, warum die Authentifizierung erforderlich ist. Dadurch wird die um Vertrauensstellung mit dem Benutzer zu erstellen.

Als Nächstes wird im folgenden Abschnitt, wir Gewusst wie: Implementieren der API und berücksichtigen Sie diese Einschränkungen betrachten.

## <a name="adding-touch-id-to-your-application"></a>Touch ID der Anwendung hinzufügen

In den vorherigen Abschnitten haben Sie die Theorie hinter den Zugriff und die Authentifizierung mit Keychain und lokale Authentifizierung. Es dauert nun einen Blick, wie Sie Touch ID in Ihrer Anwendung integrieren können.

### <a name="walkthrough"></a>Exemplarische Vorgehensweise

Daher sehen wir uns unsere Anwendung einige Touch ID-Authentifizierung hinzugefügt. In dieser exemplarischen Vorgehensweise werden wir aktualisieren den [Storyboard Tabelle](https://developer.xamarin.com/samples/StoryboardTable/) Beispiel wird die lokalen Authentifizierung hinzufügen, damit an, dass es wie funktioniert die [Storyboard-Tabelle – lokale Authentifizierung](https://developer.xamarin.com/samples/monotouch/StoryboardTable_LocalAuthentication/) Beispiel nur lässt Authentifizierte Benutzer auf die Aufgaben zur Liste hinzuzufügen.

1. Herunterzuladen Sie das Beispiel, und führen Sie es in Visual Studio für Mac.
2. Einen Doppelklick auf `MainStoryboard.Storyboard` , im Beispiel in der iOS-Designer zu öffnen. In diesem Beispiel möchten wir unsere Anwendung einen neuen Bildschirm hinzufügen, die die Authentifizierung gesteuert wird. Dies geht vor der aktuellen `MasterViewController`.
3. Ziehen Sie ein neues **Ansichtscontroller** aus der **Toolbox** auf die **Entwurfsoberfläche**. Legen Sie diese als die **Root View Controller** von **STRG + Ziehen** aus der **Navigationscontroller**:

    [![](touchid-images/image4.png "Den Root View Controller festlegen")](touchid-images/image4.png#lightbox)
4.  Benennen Sie die neue View Controller `AuthenticationViewController`.
5. Als Nächstes ziehen Sie eine Schaltfläche, und platzieren Sie es auf die `AuthenticationViewController`. Rufen Sie diese `AuthenticateButton`, und geben sie den Text `Add a Chore`.
6. Erstellen Sie ein Ereignis für die `AuthenticateButton` namens `AuthenticateMe`.
7. Erstellen Sie eine manuelle aus segue `AuthenticationViewController` durch Klicken auf die schwarze Leiste am unteren Rand und **STRG + Ziehen** aus den Balken, um die `MasterViewController` auswählen und **Push** (oder **anzeigen** Wenn Größenklassen verwenden):

    [![](touchid-images/image5.png "Ziehen Sie in der Leiste auf den MasterViewController und Auswählen von Push oder anzeigen")](touchid-images/image6.png#lightbox)
8. Klicken Sie auf das neu erstellte segue und weisen Sie ihm den Bezeichner `AuthenticationSegue`, wie unten gezeigt:

    [![](touchid-images/image7.png "Legen Sie den Segue-Bezeichner auf AuthenticationSegue")](touchid-images/image7.png#lightbox)
9. Fügen Sie den folgenden Code zu `AuthenticationViewController` hinzu:

    ```csharp
    partial void AuthenticateMe (UIButton sender)
    {
        var context = new LAContext();
        NSError AuthError;
        var myReason = new NSString("To add a new chore");

        if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, out AuthError)){
            var replyHandler = new LAContextReplyHandler((success, error) => {
                this.InvokeOnMainThread(()=> {
                    if(success)
                    {
                        Console.WriteLine("You logged in!");
                        PerformSegue("AuthenticationSegue", this);
                    }
                    else
                    {
                        // Show fallback mechanism here
                    }
                });
            });
            context.EvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, myReason, replyHandler);
        };
    }
    ```

Dies ist der Code, die für die Touch ID-Authentifizierung, die mit der lokalen Authentifizierung implementiert werden müssen. Die hervorgehobenen Zeilen in der folgenden Abbildung zeigen die Verwendung der lokalen Authentifizierung:

[![](touchid-images/image8.png "Die hervorgehobenen Zeilen zeigen die Verwendung der lokalen Authentifizierung")](touchid-images/image8.png#lightbox)

Zunächst müssen wir ermitteln, ob das Gerät kann akzeptieren Touch ID, die Eingabe, mit der `CanEvaluatePolicy` und übergeben Sie in der Richtlinie `DeviceOwnerAuthenticationWithBiometrics`. Wenn dies gilt, wir können die Touch ID-Benutzeroberfläche anzeigen, indem Sie mithilfe von `EvaluatePolicy`. Es gibt drei Arten von Informationen, die wir übergeben müssen `EvaluatePolicy` – die Richtlinie selbst, eine Zeichenfolge, die erläutern, warum die Authentifizierung erforderlich ist und eine Antwort-Handler. Der Handler für die Antwort weist die Anwendung im Falle einer Authentifizierung erfolgreich "oder" nicht erfolgreich ist, was geschehen soll. Sehen wir uns näher an den Handler für die Antwort:

[![](touchid-images/image9.png "Der Handler für die Antwort")](touchid-images/image9.png#lightbox)

Der Handler für die Antwort vom Typ angegeben ist `LAContextReplyHandler`, der Erfolg – Parameter akzeptiert eine `bool` Wert und ein `NSError` namens `error`. Wenn sie erfolgreich ist, ist dies, wo wir tatsächlich ausgeführt werden, wie wir authentifizieren – möchten in diesem Fall Anzeige den Bildschirm, der wir eine neue Aufgabe hinzugefügt wird. Denken Sie daran, den Vorbehalt enthält, für die lokale Authentifizierung ist, dass er sein muss, führen Sie auf den Vordergrund, stellen Sie daher unbedingt `InvokeOnMainThread`:

[![](touchid-images/image10.png "Verwenden Sie für die lokale Authentifizierung InvokeOnMainThread")](touchid-images/image10.png#lightbox)

Wenn die Authentifizierung erfolgreich ist, wir möchten Übergang in die `MasterViewController`. Die `PerformSegue` Methode kann dazu verwendet werden:

[![](touchid-images/image11.png "Rufen Sie PerformSegue-Methode für den Übergang an den MasterViewController")](touchid-images/image11.png#lightbox)

## <a name="summary"></a>Zusammenfassung
In diesem Handbuch erläutert Schlüsselbund und wie dies unter iOS funktioniert. Behandelt auch die Keychain-ACL, und Änderungen an dieser unter iOS. Als Nächstes haben wir einen Blick auf die lokale Authentifizierung-Framework, das ist neu in iOS 8 und blickte dann auf die Implementierung von Touch ID-Authentifizierung in unserer Anwendung.

## <a name="related-links"></a>Verwandte Links

- [Storyboard-Tabelle – lokale Authentifizierung](https://developer.xamarin.com/samples/monotouch/StoryboardTable_LocalAuthentication/) 
- [Keychain-WWDC-Beispiel](https://developer.xamarin.com/samples/KeychainTouchID/)
- [Keychain-(Beispiel)](https://developer.xamarin.com/samples/Keychain/)
