---
title: Signieren des Android-Anwendungspakets
description: So signieren Sie das Android-Anwendungspaket (APK) für die Veröffentlichung
ms.prod: xamarin
ms.assetid: 8E3EFBB2-F8AD-C126-5F32-7FD140791E53
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/02/2018
ms.openlocfilehash: c6a606bf326d1e59398ab77c51b1de5ed3e497e0
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571531"
---
# <a name="signing-the-android-application-package"></a>Signieren des Android-Anwendungspakets

Unter [Preparing an Application for Release (Vorbereiten einer Anwendung auf die Veröffentlichung)](~/android/deploy-test/release-prep/index.md) wurde der **Archiv-Manager** verwendet, um die App zu erstellen und zum Signieren und Veröffentlichen in ein Archiv abzulegen. In diesem Abschnitt wird erklärt, wie eine Android-Signierungsidentität und ein neues Signaturzertifikat für Android-Anwendungen erstellt werden und die archivierte App *ad hoc* auf dem Datenträger veröffentlicht wird. Das resultierende APK kann ohne Verwendung eines App Stores auf Android-Geräten quergeladen werden.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Unter [Zur Veröffentlichung aktivieren](~/android/deploy-test/release-prep/index.md#archive) stellte das Dialogfeld **Verteilungskanal** zwei Möglichkeiten für die Verteilung dar. Wählen Sie **Ad-Hoc** aus:

[![Dialogfeld „Verteilungskanal“](images/vs/01-distribution-channel-sml.png)](images/vs/01-distribution-channel.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Unter [Zur Veröffentlichung aktivieren](~/android/deploy-test/release-prep/index.md#archive) stellte das Dialogfeld **Signieren und verteilen...** zwei Möglichkeiten für die Verteilung dar. Wählen Sie **Ad-Hoc** aus, und klicken Sie auf **Weiter**:

[![Dialogfeld „Sign and Distribute“ (Signieren und verteilen)](images/xs/01-select-ad-hoc-sml.png)](images/xs/01-select-ad-hoc.png#lightbox)

-----

<a name="newcertvs"></a>
<a name="newcert"></a>
<a name="newcertxs"></a>

## <a name="create-a-new-certificate"></a>Erstellen eines neuen Zertifikats

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Nachdem **Ad-Hoc** ausgewählt wurde, öffnet Visual Studio wie im nächsten Screenshot gezeigt die Seite **Signierungsidentität** des Dialogfelds. Um das .APK zu veröffentlichen, muss es zunächst mithilfe eines Signaturschlüssels (auch als Zertifikat bezeichnet) signiert werden.

Ein vorhandenes Zertifikat kann verwendet werden, indem Sie auf die Schaltfläche **Importieren** klicken und dann mit [APK signieren](#sign-the-apk) fortfahren. Andernfalls klicken Sie auf die Schaltfläche **+** , um ein neues Zertifikat zu erstellen:

[![Ad-Hoc-Signierungsidentität](images/vs/02-ad-hoc-signing-identity-vs-sml.png)](images/vs/02-ad-hoc-signing-identity-vs.png#lightbox)

Das Dialogfeld **Create Android Key Store** (Android-Schlüsselspeicher erstellen) wird angezeigt. Verwenden Sie dieses, um ein neues Signaturzertifikat zu erstellen, das zum Signieren von Android-Anwendungen verwendet werden kann. Geben Sie wie im folgenden Dialogfeld gezeigt die erforderlichen Informationen (rot umrandet) ein:

[![Dialogfeld „Create Android Key Store“ (Android-Schlüsselspeicher erstellen)](images/vs/03-create-android-key-store-vs-sml.png)](images/vs/03-create-android-key-store-vs.png#lightbox)

Das folgende Beispiel veranschaulicht die Art von Informationen, die angegeben werden müssen. Klicken Sie auf **Erstellen**, um das neue Zertifikat zu erstellen:

[![Erstellen eines neuen Zertifikats](images/vs/04-key-store-example-vs-sml.png)](images/vs/04-key-store-example-vs.png#lightbox)

Die resultierende Keystore-Datei befindet sich an folgendem Speicherort:

**C:\\Benutzer\\*BENUTZERNAME*\\AppData\\Local\\Xamarin\\Mono for Android\\Keystore\\*ALIAS*\\*ALIAS*.keystore**

Wenn Sie z.B. **chimp** als Alias verwenden, wird im Zuge der oben genannten Schritte ein neuer Signierungsschlüssel an folgendem Speicherort erstellt:

**C:\\Benutzer\\*BENUTZERNAME*\\AppData\\Local\\Xamarin\\Mono for Android\\Keystore\\chimp\\chimp.keystore**

> [!NOTE]
> Achten Sie darauf, die daraus entstehende Keystore-Datei und das Kennwort an einem sicheren Speicherort zu speichern, da diese nicht in der Projektmappe enthalten sind. Wenn Sie Ihre Keystore-Datei verlieren (z.B. weil Sie einen anderen Computer verwenden oder Windows neu installiert haben), können Sie Ihre App nicht mit demselben Zertifikat wie vorherige Versionen signieren.

Weitere Informationen zur Keystore-Datei finden Sie unter [Finding your Keystore's MD5 or SHA1 Signature (Suchen der MD5- oder SHA1-Signatur Ihrer Keystore-Datei)](~/android/deploy-test/signing/keystore-signature.md).

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Nachdem Sie auf **Ad-hoc** geklickt haben, öffnet Visual Studio für Mac das Dialogfeld **Android-Signierungsidentität**, wie im nachfolgenden Screenshot gezeigt. Um das .APK zu veröffentlichen, muss es zunächst mithilfe eines Signaturschlüssels (auch als Zertifikat bezeichnet) signiert werden. Wenn ein Zertifikat bereits vorhanden ist, klicken Sie auf die Schaltfläche **Vorhandenen Schlüssel importieren**, um es zu importieren. Fahren Sie dann damit fort, [das APK auf andere Weise zu signieren](#sign-the-apk), und klicken Sie auf die Schaltfläche **Neuen Schlüssel erstellen**, um ein neues Zertifikat zu erstellen:

[![Dialogfeld „Android-Signierungsidentität“](images/xs/02-android-signing-identity-sml.png)](images/xs/02-android-signing-identity.png#lightbox)

Das Dialogfeld **Neues Zertifikat erstellen** wird dazu verwendet, ein neues Signaturzertifikat zu erstellen, das zum Signieren von Android-Anwendungen verwendet werden kann. Klicken Sie auf **OK**, nachdem Sie die erforderlichen Informationen eingegeben haben:

[![Dialogfeld „Neues Zertifikat erstellen“](images/xs/03-create-new-certificate-sml.png)](images/xs/03-create-new-certificate.png#lightbox)

Die resultierende Keystore-Datei befindet sich an folgendem Speicherort:

**~/Bibliothek/Developer/Xamarin/Keystore/alias/alias.keystore**

Die oben genannten Schritte können z.B. einen neuen Signaturschlüssel an folgendem Speicherort erstellen:

**~/Bibliothek/Developer/Xamarin/Keystore/chimp/chimp.keystore**

> [!NOTE]
> Achten Sie darauf, die daraus entstehende Keystore-Datei und das Kennwort an einem sicheren Speicherort zu speichern, da diese nicht in der Projektmappe enthalten sind. Wenn Sie Ihre Keystore-Datei verlieren (z.B. weil Sie einen anderen Computer verwenden oder macOS neu installiert haben), können Sie Ihre App nicht mit demselben Zertifikat wie vorherige Versionen signieren.

Weitere Informationen zur Keystore-Datei finden Sie unter [Finding your Keystore's MD5 or SHA1 Signature (Suchen der MD5- oder SHA1-Signatur Ihrer Keystore-Datei)](~/android/deploy-test/signing/keystore-signature.md).

-----

## <a name="sign-the-apk"></a>Signieren des APKs

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Wenn Sie auf **Erstellen** klicken, wird ein neuer Schlüsselspeicher (der ein neues Zertifikat enthält) gespeichert und wie im folgenden Screenshot gezeigt unter **Signierungsidentität** aufgelistet. Um eine App in Google Play zu veröffentlichen, klicken Sie auf **Abbrechen**, und wechseln Sie zu [Publishing to Google Play (Veröffentlichen in Google Play)](~/android/deploy-test/publishing/publishing-to-google-play/index.md).
Um *ad-hoc* zu veröffentlichen, wählen Sie die für die Signierung zu verwendende Signierungsidentität aus, und klicken Sie auf **Speichern unter**, um die App für die unabhängige Verteilung zu veröffentlichen. Beispielsweise wird im folgenden Screenshot die Signierungsidentität **chimp** (zuvor erstellt) ausgewählt:

[![Beispiel für Signierungsidentität](images/vs/05-save-as-vs-sml.png)](images/vs/05-save-as-vs.png#lightbox)

Als Nächstes zeigt der **Archive Manager** den Veröffentlichungsfortschritt an. Wenn der Veröffentlichungsprozess abgeschlossen ist, öffnet sich das Dialogfeld **Speichern unter**, um nach einem Speicherort zum Speichern der erstellten .APK-Datei zu fragen:

[![Dialogfeld „Speichern unter“](images/vs/06-save-as-dialog-vs-sml.png)](images/vs/06-save-as-dialog-vs.png#lightbox)

Navigieren Sie zum gewünschten Speicherort, und klicken Sie auf **Speichern**. Wenn das Kennwort für den Schlüssel unbekannt ist, erscheint das Dialogfeld **Signierungskennwort**, um nach dem Kennwort für das ausgewählte Zertifikat zu fragen:

[![Dialogfeld „Signierungskennwort“](images/vs/07-signing-password-vs-sml.png)](images/vs/07-signing-password-vs.png#lightbox)

Nachdem der Signierungsvorgang abgeschlossen ist, klicken Sie auf **Distribution öffnen**:

[![Schaltfläche „Distribution öffnen“](images/vs/08-open-distribution-sml.png)](images/vs/08-open-distribution.png#lightbox)

Dadurch öffnet Windows Explorer den Ordner, der die erstellte APK-Datei enthält. Jetzt hat Visual Studio die Xamarin.Android-Anwendung in ein APK kompiliert, das zur Verteilung bereit ist.
Der folgende Screenshot zeigt ein Beispiel der für die Veröffentlichung bereiten App **MyApp.MyApp.apk**:

[![APK wird in Windows Explorer angezeigt](images/vs/09-generated-app-vs-sml.png)](images/vs/09-generated-app-vs.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Wie hier gezeigt, wurde ein neues Zertifikat zum Schlüsselspeicher hinzugefügt. Um eine App in Google Play zu veröffentlichen, klicken Sie auf **Abbrechen**, und wechseln Sie zu [Publishing to Google Play (Veröffentlichen in Google Play)](~/android/deploy-test/publishing/publishing-to-google-play/index.md).
Andernfalls klicken Sie auf **Weiter**, um die App wie im folgenden Beispiel gezeigt *ad-hoc* (für eine unabhängige Verteilung) zu veröffentlichen:

[![Dialogfeld „Sign and Distribute“ (Signieren und verteilen)](images/xs/04-select-identity-sml.png)](images/xs/04-select-identity.png#lightbox)

Das Dialogfeld **Als Ad-hoc veröffentlichen** bietet eine Zusammenfassung der signierten App, bevor sie veröffentlicht wird. Wenn diese Informationen korrekt sind, klicken Sie auf **Veröffentlichen**.

[![Als Ad-hoc veröffentlichen](images/xs/05-publish-ad-hoc-sml.png)](images/xs/05-publish-ad-hoc.png#lightbox)

Das Dialogfeld **APK-Ausgabedatei** speichert das APK am angegebenen Pfad. Klicken Sie auf **Speichern**.

![Dialogfeld „APK-Ausgabedatei“](images/xs/06-output-apk-file.png)

Als Nächstes geben Sie das Kennwort für das Zertifikat (das Kennwort, das im Dialogfeld **Neues Zertifikat erstellen** verwendet wurde) ein, und klicken Sie auf **OK**:

![Eingeben des Kennwort des Zertifikats](images/xs/07-signing-certificate.png)

Das APK wird mit dem Zertifikat signiert und am angegebenen Speicherort gespeichert. Klicken Sie auf **Im Finder anzeigen**:

[![Dialogfeld „Publication Succeeded“ (Veröffentlichung erfolgreich)](images/xs/08-app-is-ready-sml.png)](images/xs/08-app-is-ready.png#lightbox)

Hierdurch öffnet sich der Finder für den Speicherort der signierten APK-Datei:

[![APK wird im Finder angezeigt](images/xs/09-show-in-finder-sml.png)](images/xs/09-show-in-finder.png#lightbox)

Das APK kann im Finder kopiert und an das endgültige Ziel gesendet werden. Es wird empfohlen, das APK vor der Verteilung auf einem Android-Gerät zu installieren und auszuprobieren. Weitere Informationen zum Veröffentlichen eines *Ad-Hoc*-APK finden Sie unter [Unabhängiges Veröffentlichen](~/android/deploy-test/publishing/publishing-independently.md).

-----

## <a name="next-steps"></a>Nächste Schritte

Nachdem das Anwendungspaket für die Veröffentlichung signiert wurde, muss es veröffentlicht werden. In den folgenden Abschnitten werden verschiedene Möglichkeiten zum Veröffentlichen einer Anwendung beschrieben.
