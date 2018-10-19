---
title: Häufig gestellte Fragen (FAQs)
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 0F0FDD2B-FFB1-476F-B674-81DB3A5E1CF3
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: ad3fc32245880f6603c63d33aac49309fd61b753
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2018
ms.locfileid: "36935463"
---
# <a name="frequently-asked-questions"></a>Häufig gestellte Fragen (FAQs)

## <a name="installation--setup"></a>Installation und Setup

### <a name="which-android-sdk-packages-should-i-installinstall-android-sdk-packagesmd"></a>[Welche Android SDK-Pakete sollte ich installieren?](install-android-sdk-packages.md)

Der Android SDK-Installation enthalten nicht automatisch alle mindestens erforderlichen Pakete für die Entwicklung von. Während einzelner Entwickler variieren muss, wird dieses Handbuch erläutert, die Pakete, die in der Regel für die Entwicklung mit Xamarin.Android benötigt werden.

### <a name="where-can-i-set-my-android-sdk-locationsandroid-sdk-locationmd"></a>[Wo kann ich meine Android SDK-Speicherorte festlegen?](android-sdk-location.md)

Dieses Handbuch beschreibt beide die Standardeinstellungen des Android SDK, die für die meisten Setups funktionieren sollte; und wie Sie diese Standardeinstellungen in Visual Studio für Mac oder Visual Studio bei Bedarf zu ändern.

### <a name="how-do-i-update-the-java-development-kit-jdk-versionupdate-jdkmd"></a>[Wie aktualisiere ich die Java Development Kit-Version (JDK)?](update-jdk.md)

In diesem Artikel wird veranschaulicht, wie zum Aktualisieren der Java Development Kit (JDK) Version unter Windows und Mac.

### <a name="can-i-use-java-development-kit-jdk-version-9-or-laterjdk9-errorsmd"></a>[Kann ich die Java Development Kit (JDK) Version 9 oder höher verwenden?](jdk9-errors.md)

Xamarin.Android erfordert JDK 8 "oder" Microsoft Mobile OpenJDK. Dieser Artikel beschreibt einige häufige Fehlermeldungen, angezeigt werden können, wenn JDK 9 oder höher installiert ist, sowie Anweisungen zum Überprüfen der JDK-Version.


### <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packagesinstall-android-support-librarymd"></a>[Wie kann ich die Android-Unterstützungsbibliotheken, die für die Xamarin.Android.Support-Pakete erforderlich sind, manuell installieren?](install-android-support-library.md)

Dieses Handbuch enthält die Beispielschritte für die Installation der `Xamarin.Android.Support.v4` Support-Bibliothek für Windows und Mac.

### <a name="what-usb-drivers-do-i-need-to-debug-android-on-windowsandroid-drivers-debug-windowsmd"></a>[Welche USB-Treiber sind zum Debuggen von Android unter Windows erforderlich?](android-drivers-debug-windows.md)

Um auf Android-Geräten Debuggen, wenn in Windows zu entwickeln; Sie müssen einen kompatiblen USB-Treiber zu installieren. Android SDK-Manager umfasst "Google-USB-Treiber" werden standardmäßig die Unterstützung für Nexus-Geräte hinzugefügt.
Andere Geräte erfordern, USB-Treiber, die vom Gerätehersteller veröffentlicht wird. Dieses Handbuch enthält Informationen zum Ermitteln dieser Treiber als auch andere Testmethoden.

### <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vmconnect-android-emulator-mac-windowsmd"></a>[Ist es möglich, über eine Windows-VM mit den Android-Emulatoren, die auf einem Mac ausgeführt werden, eine Verbindung herzustellen?](connect-android-emulator-mac-windows.md)

Dieses Handbuch enthält Methoden, wenn Sie den Android-Emulator verwenden.

## <a name="general-questions"></a>Allgemeine Fragen

### <a name="how-do-i-automate-an-android-nunit-test-projectautomate-android-nunit-testmd"></a>[Wie automatisiere ich ein Android NUnit-Testprojekt?](automate-android-nunit-test.md)

Dieser Leitfaden behandelt die Schritte zum Einrichten von ein Android NUnit-Testprojekt _nicht_ eines Xamarin.UITest-Projekts. Xamarin.UITest-Anleitungen finden Sie [hier](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

### <a name="how-do-i-enable-intellisense-in-android-axml-filesenable-axml-intellisensemd"></a>[Wie aktiviere ich Intellisense in AXML-Dateien von Android?](enable-axml-intellisense.md)

Dieses Handbuch beschreibt, wie Sie Visual Studio Intellisense für Android axml-Dateien zu aktivieren.

### <a name="why-cant-my-android-release-build-connect-to-the-internetandroid-internetmd"></a>[Warum kann mein Android-Releasebuild keine Verbindung mit dem Internet herstellen?](android-internet.md)

Die häufigste Ursache für dieses Problem ist, die die **INTERNET** Berechtigung ist in einem Debugbuild automatisch enthalten, sondern muss manuell für einen Releasebuild festgelegt werden. Dieses Handbuch beschreibt, wie Sie die Berechtigung für Releasebuilds zu aktivieren.

### <a name="smarter-xamarin-android-support-v4--v13-nuget-packagesandroid-support-v4v13-librariesmd"></a>[NuGet-Pakete für eine intelligentere Xamarin Android-Unterstützung v4/v13 ](android-support-v4v13-libraries.md)

`Support-v4` und `Support-v13` kann nicht verwendet werden zusammen in derselben app, also stellen sich gegenseitig ausschließende. Grund hierfür ist, `Support-v13` enthält alle Typen und Implementierung von `Support-v4`. Wenn Sie versuchen, und beide im selben Projekt verweisen, werden doppelte Typnamen Fehler auftreten.

### <a name="how-do-i-resolve-a-pathtoolongexception-errorpath-too-long-exceptionmd"></a>[Wie löse ich eine PathTooLongException-Fehler?](path-too-long-exception.md)

In diesem Artikel wird erläutert, wie zum Auflösen einer **PathTooLongException** Fehler, die beim Erstellen eines Xamarin.Android-Projekts auftreten können.



## <a name="deprecated"></a>Als veraltet markiert

> [!NOTE]
> Die folgenden Artikel gelten für Probleme, die in früheren Versionen von Xamarin aufgelöst wurden. Jedoch, wenn das Problem auf die neueste Version der Software auftritt, melden Sie bitte eine [neuer Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md) erstellen Sie mit der Ihre vollständige versionsverwaltung Informationen "und" vollständig "Protokollausgabe.

### <a name="what-version-of-xamarinandroid-added-lollipop-supportxa-lollipopmd"></a>[In welcher Version von Xamarin.Android wurde Lollipop-Unterstützung hinzugefügt?](xa-lollipop.md)

Dieses Handbuch wurde ursprünglich für die Vorschau für Android L geschrieben. Xamarin.Android 4.17 hinzugefügt, dass Android Vorschauunterstützung für L & Xamarin.Android 4.20 Android Lollipop-Unterstützung hinzugefügt.

### <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawablemissing-action-mode-share-drawablemd"></a>[Android.Support.v7.AppCompat: Keine Ressource gefunden, die mit dem angegebenen Namen übereinstimmt: attr 'android:actionModeShareDrawable'](missing-action-mode-share-drawable.md)

Dieser Fehler kann in früheren Versionen von Xamarin auftreten, wenn einige der erforderlichen Android-SDK-Pakete fehlen.

### <a name="adjusting-java-memory-parameters-for-the-android-designerandroid-designer-java-memorymd"></a>[Anpassen der Java-Speicherparameter für Android Designer](android-designer-java-memory.md)

Die Standardparameter für den Arbeitsspeicher, die verwendet werden, beim Starten der `java` für Android Designer möglicherweise nicht kompatibel mit einigen Systemkonfigurationen zu verarbeiten. Beginnend mit Xamarin Studio 5.7.2.7 und Xamarin für Visual Studio 3.9.344 diese Einstellungen kann individuell pro Projekt angepasst werden.

### <a name="my-android-resourcedesignercs-file-will-not-updateresource-designer-wont-updatemd"></a>[Meine Android-Datei „Resource.designer.cs“ wird nicht aktualisiert](resource-designer-wont-update.md)

Ein Fehler in xamarin.Studio die Datei 5.1 beschädigt zuvor CSPROJ-Dateien teilweise oder vollständig löschen Sie den XML-Code in die CSPROJ-Datei. Dies würde dazu führen, dass wichtige Teile des Android-Buildsystem (z. B. Aktualisieren der Android Resource.designer.cs) fehlschlägt. Zum Zeitpunkt der 5.1.4 stabile Version am 15. Juli, dieses Problem wurde behoben. aber in vielen Fällen die Projektdatei manuell, wie in diesem Handbuch beschrieben behoben werden muss.



