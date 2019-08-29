---
title: Berührungs-ID in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Sie die Berührungs-ID, die biometrische Fingerabdruck-Authentifizierungstechnologie von Apple, in xamarin. IOS-Apps verwenden.
ms.prod: xamarin
ms.assetid: 4BC8EFD6-52FC-4793-BA69-D6BFF850FE5F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: 0f92dca71f74266e1408cd65c842f729a9a648ce
ms.sourcegitcommit: 3d21bb1a6d9b78b65aa49917b545c39d44aa3e3c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70065668"
---
# <a name="touch-id-in-xamarinios"></a>Berührungs-ID in xamarin. IOS

Die Berührungs-ID wurde in ios 7 als Mittel zum Authentifizieren des Benutzers eingeführt, ähnlich wie bei einer Kennung. Dies war jedoch auf das Entsperren des Geräts, das Verwenden des App Store, die Verwendung von iTunes und das Authentifizieren der icloud-Keychain beschränkt.

Es gibt zwei Möglichkeiten, die Berührungs-ID als Authentifizierungsmechanismus in einer IOS 8-Anwendung mithilfe der lokalen Authentifizierungs-API zu verwenden. Es ist derzeit nicht möglich, die lokale Authentifizierung für die Remote Authentifizierung zu verwenden.

Um die Berührungs-ID und ihren Wert vollständig zu verstehen, sollten Sie Keychain-Dienste kennenlernen und die neuen Änderungen für die Daten des Benutzers durchführen. Der Keychain-Zugriff wurde auch in ios 8 durch die Verwendung des neuen Features für die Access Control Listen (ACLs) erweitert.

## <a name="keychain--secure-enclave"></a>Keychain & Secure Enclave

Keychain ist eine große Datenbank, die sicheren Speicher für Kenn Wörter, Schlüssel, Zertifikate und Notizen für eine einzelne Apple-ID bereitstellt. In ios 8 hat eine Anwendung immer Zugriff auf eigene eindeutige Keychain-Elemente und kann nicht auf Keychain-Elemente anderer Anwendungen zugreifen. Dies unterscheidet sich von OS X, bei dem die Keychain mit einem einzelnen Kennwort entsperrt wird, sodass jede Keychain-Dienste-fähige Anwendung die Keychain verwendet. In diesem Artikel konzentrieren wir uns auf die Funktionsweise der keychain in ios 8.

Keychain ist eine spezialisierte Datenbank, in der jede Zeile als _Keychain-Element_bezeichnet wird. Jedes Element wird von Keychain-Attributen beschrieben und besteht aus verschlüsselten Werten. Um die effiziente Verwendung von Keychain zu ermöglichen, ist es für kleine Elemente oder _Geheimnisse_optimiert.
Jedes Keychain-Element wird durch die Benutzerkennung und einen eindeutigen geheimen Geräte Schlüssel geschützt. Keychain-Elemente sollten auch dann geschützt werden, wenn Benutzer ihre Geräte nicht verwenden. Dies wird in ios implementiert, da die Elemente nur dann verfügbar werden können, wenn das Gerät entsperrt ist – wenn das Gerät gesperrt ist, sind Sie nicht mehr verfügbar. Sie können auch in einer verschlüsselten Sicherung gespeichert werden. Eines der wichtigsten Features von Keychain besteht darin, die Zugriffs Steuerung zu erzwingen. eine Anwendung kann auf ihren Teil der Keychain zugreifen, und alle anderen Anwendungen werden verhindert. Im folgenden Diagramm wird veranschaulicht, wie eine Anwendung mit der Keychain interagiert:

[![](touchid-images/image1.png "Dieses Diagramm veranschaulicht, wie eine Anwendung mit der Keychain interagiert.")](touchid-images/image1.png#lightbox)

### <a name="secure-enclave"></a>Sichere Enclave

Der Keychain kann das Keychain-Element nicht selbst entschlüsseln. Stattdessen erfolgt die Ausführung in der *sicheren Enclave*. Bei der sicheren Enclave handelt es sich um einen Co-Prozessor im A7-Chip, der dafür verantwortlich ist, eine erfolgreiche Überprüfung von Fingerabdruckdaten vom touchid-Sensor mit einem registrierten Druck zu ermitteln. Anschließend wird das Keychain-Element entschlüsselt und der entschlüsselte geheime Schlüssel an die Keychain zurückgegeben.

### <a name="working-with-keychain"></a>Arbeiten mit Keychain

Zuerst sollte Ihre Anwendung die Keychain Abfragen, um zu ermitteln, ob ein Kennwort vorhanden ist. Wenn Sie nicht vorhanden ist, müssen Sie möglicherweise zur Eingabe eines Kennworts aufgefordert werden, damit der Benutzer nicht ständig gefragt wird. Wenn das Kennwort aktualisiert werden muss, fordern Sie den Benutzer zur Eingabe eines neuen Kennworts auf, und übergeben Sie den aktualisierten Wert an die Keychain.

> [!NOTE]
> Nachdem Sie einen geheimen Schlüssel verwendet haben, der aus der Keychain abgerufen wurde, sollten alle Verweise auf die Daten aus dem Arbeitsspeicher gelöscht werden. Weisen Sie es niemals einer globalen Variablen zu.

## <a name="keychain-acl-and-touch-id"></a>Keychain-ACL und Berührungs-ID

Access Control Liste ist ein neues Keychain-Element Attribut in ios 8, das die Informationen darüber beschreibt, was geschehen muss, um einen bestimmten Vorgang zuzulassen. Dies kann in der Form angezeigt werden, wenn ein Warnungs Dialogfeld angezeigt oder eine Kennung angefordert werden muss. Mithilfe der ACL können Sie Barrierefreiheit und Authentifizierung für ein Keychain-Element festlegen. Das folgende Diagramm zeigt, wie dieses neue Attribut mit dem Rest des Keychain-Elements verknüpft wird:

[![](touchid-images/image2.png "Dieses Diagramm zeigt, wie dieses neue Attribut mit dem Rest des Keychain-Elements verknüpft wird.")](touchid-images/image2.png#lightbox)

Ab IOS 8 gibt es jetzt eine neue Benutzer Anwesenheits Richtlinie, `SecAccessControl`die von der sicheren Enclave auf einem iPhone 5S und höher erzwungen wird. In der folgenden Tabelle sehen Sie, wie die Gerätekonfiguration die Richtlinien Auswertung beeinflussen kann:

|Gerätekonfiguration|Richtlinien Auswertung|Sicherungsmechanismus|
|--- |--- |--- |
|Gerät ohne Kennung|Kein Zugriff|None|
|Gerät mit Kennung|Erfordert Kennung|None|
|Gerät mit Berührungs-ID|Bevorzugt die Berührungs-ID|Ermöglicht Passcode|

Alle Vorgänge innerhalb der sicheren Enclave können einander vertrauen. Dies bedeutet, dass wir das Ergebnis der Berührungs-ID-Authentifizierung verwenden können, um die Schlüsselbund Element-Entschlüsselung zu autorisieren. Die sichere Enclave hält auch einen gegen Übereinstimmung mit Fehlern bei Berührungs-IDs. in diesem Fall muss der Benutzer die Kennung wiederherstellen.
Ein neues Framework in ios 8, das als _lokale Authentifizierung_bezeichnet wird, unterstützt diesen Authentifizierungsprozess innerhalb des Geräts. Dies wird im nächsten Abschnitt erläutert.

## <a name="local-authentication"></a>Lokale Authentifizierung

Wie bereits im vorherigen Abschnitt beschrieben, können Anwendungen die lokale Authentifizierung zum Authentifizieren des Benutzers in Bezug auf die Sicherheitsrichtlinie verwenden, die auf dem Gerät eingerichtet wurde.

Derzeit bietet die API nur zwei Funktionen: Erstens werden die vorhandenen Keychain-Dienste durch die Verwendung neuer Keychain-Access Control Listen (ACLs) unterstützt. Keychain-Daten können mit der erfolgreichen Authentifizierung eines Benutzer Fingerabdrucks entsperrt werden.

Zweitens bietet localauthentication zwei Methoden, um Ihre Anwendung lokal zu authentifizieren. Entwickler sollten verwenden `CanEvaluatePolicy` , um zu bestimmen, ob das Gerät die Berührungs-ID akzeptieren kann `EvaluatePolicy` , und dann den Authentifizierungs Vorgang zu starten.

Obwohl beide Funktionen eine lokale Authentifizierung bieten, bieten Sie keinen Mechanismus für die Anwendung oder den Benutzer, um sich bei einem Remote Server zu authentifizieren.
Die lokale Authentifizierung stellt eine neue Standardbenutzer Oberfläche für die Authentifizierung bereit. Im Fall von "Fingereingabe-ID" handelt es sich hierbei um eine Warnungs Ansicht mit zwei Schaltflächen, wie unten gezeigt. Eine Schaltfläche, die abgebrochen werden soll, und eine Schaltfläche, um die Ausweich Methode für die Authentifizierung zu verwenden – die Kennung. Es gibt auch eine benutzerdefinierte Meldung, die festgelegt werden muss. Es wird empfohlen, diesen Wert zu verwenden, um dem Benutzer zu erklären, warum die Berührungs-ID-Authentifizierung erforderlich ist.

[![](touchid-images/image12.png "Die Berührungs-ID-Authentifizierungs Warnung")](touchid-images/image12.png#lightbox)

### <a name="with-keychain-services"></a>Mit Keychain-Diensten

Wir haben uns kurz mit der Entschlüsselung eines Keychain-Elements beschäftigt, indem wir mit dem sicheren Enclave unsere Kennung verifizieren. In ios 8 können wir die lokale Authentifizierung verwenden, um die Überprüfung von Finger Eingaben in Verbindung mit der Funktion "Access Control Listen" anzufordern, die die Implementierung des Fall Back Mechanismus oder das Kennwort bereitstellt.
Um ACL verwenden zu können, sollten Sie `SecAccessControl` die Richtlinie verwenden und dann den Status des Geräts mithilfe `SecAccessible.WhenPasscodeSetThisDeviceOnly` von `SecAccessible.WhenUnlocked`oder überprüfen.

#### <a name="considerations-with-acl"></a>Überlegungen zu ACL

Bei der Verwendung von ACL mit der Keychain sollten Sie viele Punkte beachten, und einige davon sind unten aufgeführt:

- Nur mit Vordergrund Anwendung verwenden – Wenn Sie einen Keychain-Vorgang für einen Hintergrund Thread aufzurufen, tritt beim-Vorgang ein Fehler auf.
- Das Hinzufügen und Aktualisieren von Keychain-Elementen kann eine Authentifizierung erfordern.
- Wenn eine Anforderung mehrere übereinstimmende Elemente in der Keychain zurückgibt, ist möglicherweise eine Authentifizierung erforderlich.
- Geschützte ACL-Elemente sind Geräte basiert und werden daher nicht synchronisiert oder gesichert.

### <a name="using-local-authentication-without-keychain-services"></a>Verwenden der lokalen Authentifizierung ohne Keychain-Dienste

Die lokale Authentifizierung wurde als Möglichkeit zum Erfassen von Anmelde Informationen (z. b. Kennung oder Fingereingabe-ID) und zum Arbeiten mit der sicheren Enclave erstellt, um die Authentifizierung des Benutzers abzuschließen. Stellen Sie sich dies als Brücke zwischen Ihrer Anwendung und der sicheren Enclave vor, die niemals direkt miteinander kommunizieren kann. Sie kann auch für die Richtlinien Auswertung für Ihre Anwendung verwendet werden.

Zu diesem Zweck wird von einer Anwendung die Richtlinien Auswertung innerhalb der lokalen Authentifizierung aufgerufen, die den Vorgang in der sicheren Enclave startet. Sie können dies nutzen, um die Authentifizierung für Ihre APP bereitzustellen, ohne direkt auf die sichere Enclave abzufragen oder darauf zuzugreifen.

[![](touchid-images/image13a.png "Verwenden der lokalen Authentifizierung ohne Keychain-Dienste")](touchid-images/image13a.png#lightbox)

Die Verwendung der lokalen Authentifizierung in Ihrer Anwendung bietet eine einfache Möglichkeit, die Benutzer Überprüfung zu implementieren, z. b. um eine Funktion nur für die Augen des Geräte Besitzers (z. b. Bankinganwendungen) zu entsperren oder um Jugendschutz Personen zu unterliegen. Asyl. Sie können diese Methode auch verwenden, um die Authentifizierung zu erweitern, die bereits vorhanden ist – Benutzer, deren Informationen sicher sind, aber auch über Optionen verfügen.

Die Sicherheit der lokalen Authentifizierung unterscheidet sich von der der Keychain. Wenn Sie z. b. die Keychain verwenden, liegt die Vertrauensstellung zwischen dem Betriebssystem und der sicheren Enclave. Bei der lokalen Authentifizierung liegt der Dienst zwischen der Anwendung und dem Betriebssystem. Dies bedeutet, dass Sie nur auf die Ergebnisse der sicheren Enclave, nicht auf die sichere Enclave selbst zugreifen können.

Bei der Sicherheit ist es auch äußerst wichtig zu wissen, dass es **keinen Zugriff** auf registrierte Finger oder Fingerabdruckbilder gibt. Die sichere Enclave ist der Besitzer dieser Informationen, sodass keine andere Systemkomponente darauf zugreifen kann.

Um die Berührungs-ID ohne Keychain zu verwenden, indem Sie die lokale Authentifizierungs-API nutzen, gibt es einige Funktionen, die wir verwenden können. Diese werden im folgenden beschrieben:

* `CanEvaluatePolicy`– Hiermit wird lediglich überprüft, ob das Gerät die Berührungs-ID annehmen kann.
* `EvaluatePolicy`– Hiermit wird der Authentifizierungs Vorgang gestartet, und die Benutzeroberfläche wird angezeigt `true` , `false` und es wird eine Antwort oder zurückgegeben.
* `DeviceOwnerAuthenticationWithBiometrics`– Dies ist die Richtlinie, die verwendet werden kann, um den Touchscreen der touchkennung anzuzeigen. Beachten Sie, dass hier kein Kennungs Fall Back Mechanismus vorhanden ist. stattdessen sollten Sie diesen Fall Back in Ihre Anwendung implementieren, damit Benutzer die Berührungs-ID-Authentifizierung überspringen können.

Es gibt einige Einschränkungen bei der Verwendung der lokalen Authentifizierung, die unten aufgeführt sind:

* Wie bei Keychain kann Sie nur im Vordergrund ausgeführt werden. Wenn Sie ihn in einem Hintergrund Thread aufrufen, tritt ein Fehler auf.
* Beachten Sie, dass die Richtlinien Auswertung möglicherweise fehlschlägt. Eine Kennung-Schaltfläche muss als Fallback implementiert werden.
* Sie müssen einen `localizedReason` angeben, um zu erläutern, warum die Authentifizierung erforderlich ist. Dies trägt dazu bei, eine Vertrauensstellung mit dem Benutzer zu erstellen.

Im folgenden Abschnitt erfahren Sie, wie Sie die API implementieren, um diese Einschränkungen zu berücksichtigen.

## <a name="adding-touch-id-to-your-application"></a>Hinzufügen der Fingereingabe-ID zu Ihrer Anwendung

In den vorherigen Abschnitten haben wir uns die Theorie hinter dem Zugriff und der Authentifizierung mithilfe von Keychain und lokaler Authentifizierung angesehen. Wir sehen uns nun an, wie Sie die Berührungs-ID in Ihre Anwendung integrieren können.

### <a name="walkthrough"></a>Exemplarische Vorgehensweise

Sehen wir uns nun an, wie wir der Anwendung eine Fingereingabe-ID-Authentifizierung hinzufügen. In dieser exemplarischen Vorgehensweise aktualisieren wir das [Storyboard-Tabellen](https://docs.microsoft.com/samples/xamarin/ios-samples/data/storyboardtable/) Beispiel, indem wir eine lokale Authentifizierung hinzufügen, sodass es wie das Beispiel [Storyboard Table – local Authentication](https://docs.microsoft.com/samples/xamarin/ios-samples/storyboardtable-localauthentication) funktioniert, das nur authentifizierten Benutzern das Hinzufügen von Aufgaben zur Liste gestattet.

1. Laden Sie das Beispiel herunter, und führen Sie es in Visual Studio für Mac aus.
2. Doppelklicken Sie `MainStoryboard.Storyboard` auf, um das Beispiel im IOS-Designer zu öffnen. In diesem Beispiel möchten wir der Anwendung einen neuen Bildschirm hinzufügen, mit dem die Authentifizierung gesteuert wird. Dies erfolgt vor dem aktuellen `MasterViewController`.
3. Ziehen Sie einen neuen **Ansichts Controller** aus der **Toolbox** auf den **Designoberfläche**. Legen Sie diese als Stamm **Ansichts Controller** durch Drücken von **Strg + Drag** vom **Navigations Controller**fest:

    [![](touchid-images/image4.png "Festlegen des root View Controller")](touchid-images/image4.png#lightbox)
4. Benennen Sie den neuen Ansichts Controller `AuthenticationViewController`.
5. Ziehen Sie als nächstes eine Schaltfläche, und platzieren `AuthenticationViewController`Sie Sie auf dem. Nennen Sie `AuthenticateButton`diese, und versehen Sie Sie `Add a Chore`mit dem Text.
6. Erstellen Sie ein Ereignis für `AuthenticateButton` den `AuthenticateMe`aufgerufenen.
7. Erstellen Sie ein manuelles Bild `AuthenticationViewController` aus, indem Sie auf die schwarze Leiste am unteren Rand klicken, **STRG + Ziehen** von `MasterViewController` der Leiste auf die und dann auf **Push** (oder bei Verwendung von Größenklassen **anzeigen** ) klicken:

    [![](touchid-images/image5.png "Ziehen Sie von der Leiste auf den masterviewcontroller, und wählen Sie Pushvorgang oder anzeigen aus.")](touchid-images/image6.png#lightbox)
8. Klicken Sie auf den neu erstellten Abschnitt, und versehen Sie ihn `AuthenticationSegue`wie unten dargestellt mit dem Bezeichner:

    [![](touchid-images/image7.png "Legen Sie den segue-Bezeichner auf authenticationsegue fest.")](touchid-images/image7.png#lightbox)
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

Dies ist der gesamte Code, den Sie benötigen, um die Berührungs-ID-Authentifizierung mit lokaler Authentifizierung zu implementieren. Die markierten Zeilen in der Abbildung unten zeigen die Verwendung der lokalen Authentifizierung:

[![](touchid-images/image8.png "Die markierten Zeilen zeigen die Verwendung der lokalen Authentifizierung an.")](touchid-images/image8.png#lightbox)

Zuerst müssen wir festlegen, ob das Gerät Fingereingabe `CanEvaluatePolicy` -IDs akzeptieren kann, indem es verwendet und die Richtlinie `DeviceOwnerAuthenticationWithBiometrics`übergibt. Wenn dies zutrifft, können wir die Benutzeroberfläche der touchid mithilfe `EvaluatePolicy`von anzeigen. Es gibt drei Informationen, die wir an `EvaluatePolicy` – übergeben müssen, die Richtlinie selbst, eine Zeichenfolge, die die Gründe für die Authentifizierung erläutert, und einen Antwort Handler. Der Antwort Handler teilt der Anwendung mit, was Sie im Falle einer erfolgreichen oder fehlgeschlagenen Authentifizierung tun soll. Sehen wir uns den Antwort Handler genauer an:

[![](touchid-images/image9.png "Der Antwort Handler.")](touchid-images/image9.png#lightbox)

Der Antwort Handler `LAContextReplyHandler`wird vom Typ angegeben, der die Parameter Success – a `bool` Value und eine `NSError` aufgerufene `error`annimmt. Wenn der Vorgang erfolgreich ist, können wir tatsächlich den gewünschten Vorgang ausführen – in diesem Fall wird der Bildschirm angezeigt, auf dem wir ein neues Chore hinzufügen können. Beachten Sie, dass eine der Einschränkungen der lokalen Authentifizierung darin besteht, dass Sie im Vordergrund ausgeführt werden muss. Stellen Sie daher `InvokeOnMainThread`sicher, dass Sie Folgendes verwenden:

[![](touchid-images/image10.png "Verwenden von invokeonmainthread für die lokale Authentifizierung")](touchid-images/image10.png#lightbox)

Wenn die Authentifizierung erfolgreich war, möchten wir zum Schluss zu `MasterViewController`wechseln. Hierfür `PerformSegue` kann die-Methode verwendet werden:

[![](touchid-images/image11.png "\"Performangue\"-Methode für den Übergang zum masterviewcontroller aufzurufen")](touchid-images/image11.png#lightbox)

## <a name="summary"></a>Zusammenfassung

In dieser Anleitung haben wir uns mit Keychain und deren Funktionsweise in ios beschäftigt. Wir haben auch die Keychain-ACL und Änderungen an dieser in ios untersucht. Als nächstes haben wir uns das lokale Authentifizierungs Framework angeschaut, das neu in ios 8 ist, und dann die Implementierung der Touchscreen-Authentifizierung in unserer Anwendung erläutert.

## <a name="related-links"></a>Verwandte Links

- [Storyboardtabelle – lokale Authentifizierung](https://docs.microsoft.com/samples/xamarin/ios-samples/storyboardtable-localauthentication)
- [Keychain-WWDC-Beispiel](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-keychaintouchid/)
- [Keychain (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/keychain/)
