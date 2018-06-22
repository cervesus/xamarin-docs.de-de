---
title: Häufig gestellte Fragen (FAQs)
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 0F0FDD2B-FFB1-476F-B674-81DB3A5E1CF3
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/19/2018
ms.openlocfilehash: 2deeb4309be44c1c5f8d78980707336ac8301761
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30774355"
---
# <a name="frequently-asked-questions"></a>Häufig gestellte Fragen (FAQs)

## <a name="installation--setup"></a>Installation und Setup

### <a name="which-android-sdk-packages-should-i-installinstall-android-sdk-packagesmd"></a>[Welche Android SDK-Pakete sollte ich installieren?](install-android-sdk-packages.md)

Installation des Android SDK hinzugefügt nicht alle die mindestens erforderlichen Pakete für die Entwicklung von automatisch. Während der einzelnen Entwickler unterschiedlich sein muss, wird dieses Handbuch die Pakete, die in der Regel für die Entwicklung mit Xamarin.Android werden erläutert.

### <a name="where-can-i-set-my-android-sdk-locationsandroid-sdk-locationmd"></a>[Wo kann ich meine Android SDK-Speicherorte festlegen?](android-sdk-location.md)

Dieses Handbuch beschreibt beide die Standardeinstellungen von Android SDK, die für die meisten Installationen geeignet sein sollte; und wie Sie diese Standardeinstellungen in Visual Studio für Mac oder Visual Studio bei Bedarf zu ändern.

### <a name="how-do-i-update-the-java-development-kit-jdk-versionupdate-jdkmd"></a>[Wie aktualisiere ich die Java Development Kit-Version (JDK)?](update-jdk.md)

In diesem Artikel wird veranschaulicht, wie die Java Development Kit (JDK) Version auf Windows- und Mac aktualisieren

### <a name="can-i-use-java-development-kit-jdk-version-9jdk9-errorsmd"></a>[Kann ich die Java Development Kit-Version 9 (JDK) verwenden?](jdk9-errors.md)

Xamarin.Android erfordert JDK 8 anstelle von JDK 9. Dieser Artikel beschreibt einige allgemeine Fehlermeldungen, die möglicherweise angezeigt werden, wenn der JDK 9 zusammen mit Anweisungen zum Überprüfen der JDK-Version installiert ist.


### <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packagesinstall-android-support-librarymd"></a>[Wie kann ich die Android-Unterstützungsbibliotheken, die für die Xamarin.Android.Support-Pakete erforderlich sind, manuell installieren?](install-android-support-library.md)

Dieses Handbuch enthält Beispielschritte für die Installation der `Xamarin.Android.Support.v4` auf Windows- und Mac-Unterstützungsbibliothek

### <a name="how-do-i-install-google-play-services-in-an-emulatorinstall-gpsmd"></a>[Wie installiere ich einen Google Play Services-Emulator?](install-gps.md)

Dieses Handbuch enthält Links zu Informationen zum Installieren von Google Play-Dienste in Visual Studio-Android-Emulator.

### <a name="what-usb-drivers-do-i-need-to-debug-android-on-windowsandroid-drivers-debug-windowsmd"></a>[Welche USB-Treiber sind zum Debuggen von Android unter Windows erforderlich?](android-drivers-debug-windows.md)

So debuggen Sie auf einem Android-Gerät bei der Entwicklung in Windows; Sie müssen einen kompatiblen USB-Treiber installieren. Android SDK Manager umfasst "Google-USB-Treiber" wird standardmäßig die Unterstützung für Nexus-Geräte hinzugefügt.
Bei anderen Geräten müssen USB-Treiber, die vom Gerätehersteller speziell veröffentlicht. Dieses Handbuch enthält Informationen zum Ermitteln dieser Treiber Prüfverfahren, die auch als alternativer.

### <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vmconnect-android-emulator-mac-windowsmd"></a>[Ist es möglich, über eine Windows-VM mit den Android-Emulatoren, die auf einem Mac ausgeführt werden, eine Verbindung herzustellen?](connect-android-emulator-mac-windows.md)

Dieser Leitfaden behandelt Methoden auf, wenn Sie einen Google Android-Emulator verwenden.

## <a name="general-questions"></a>Allgemeine Fragen

### <a name="how-do-i-automate-an-android-nunit-test-projectautomate-android-nunit-testmd"></a>[Wie automatisiere ich ein Android NUnit-Testprojekt?](automate-android-nunit-test.md)

Diese Anleitung enthält die Schritte zum Einrichten einer Testprojekt Android NUnit _nicht_ ein Xamarin.UITest-Projekt. Xamarin.UITest Handbücher verwendbaren [hier](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

### <a name="how-do-i-enable-intellisense-in-android-axml-filesenable-axml-intellisensemd"></a>[Wie aktiviere ich Intellisense in AXML-Dateien von Android?](enable-axml-intellisense.md)

Dieses Handbuch beschreibt, wie Sie Visual Studio Intellisense für Android .axml Dateien zu aktivieren.

### <a name="why-cant-my-android-release-build-connect-to-the-internetandroid-internetmd"></a>[Warum kann mein Android-Releasebuild keine Verbindung mit dem Internet herstellen?](android-internet.md)

Die häufigste Ursache für dieses Problem ist, die die **INTERNET** Berechtigung ist in einem Debugbuild automatisch enthalten, sondern muss manuell für einen Releasebuild festgelegt werden. Dieses Handbuch beschreibt, wie die Berechtigung für Releasebuilds zu aktivieren.

### <a name="smarter-xamarin-android-support-v4--v13-nuget-packagesandroid-support-v4v13-librariesmd"></a>[NuGet-Pakete für eine intelligentere Xamarin Android-Unterstützung v4/v13 ](android-support-v4v13-libraries.md)

`Support-v4` und `Support-v13` kann nicht verwendet werden zusammen in derselben app, d. h. sie sind sich gegenseitig ausschließende. Grund hierfür ist, `Support-v13` tatsächlich enthält alle Typen und Implementierung von `Support-v4`. Wenn Sie versuchen, und verweisen beide im selben Projekt treten doppelte Typfehler.

### <a name="how-do-i-resolve-a-pathtoolongexception-errorpath-too-long-exceptionmd"></a>[Wie löse ich eine PathTooLongException-Fehler?](path-too-long-exception.md)

In diesem Artikel wird erläutert, wie zum Auflösen einer **PathTooLongException** Fehler, die beim Erstellen eines Projekts Xamarin.Android auftreten können.



## <a name="deprecated"></a>Als veraltet markiert

> [!NOTE]
> Die folgenden Artikel gelten für Probleme, die in den neuesten Versionen von Xamarin aufgelöst wurden. Allerdings tritt das Problem auf die neueste Version der Software, bitte der Datei eine [neuen Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md) mit Ihrer vollständigen versionsverwaltung-Informationen "und" vollständig "Ausgabeprotokoll erstellen.

### <a name="what-version-of-xamarinandroid-added-lollipop-supportxa-lollipopmd"></a>[In welcher Version von Xamarin.Android wurde Lollipop-Unterstützung hinzugefügt?](xa-lollipop.md)

Dieses Handbuch wurde ursprünglich für die Vorschau Android L geschrieben. Xamarin.Android 4.17 hinzugefügt, dass Android Preview-Unterstützung für L & Xamarin.Android 4.20 Android Lollipop-Unterstützung hinzugefügt.

### <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawablemissing-action-mode-share-drawablemd"></a>[Android.Support.v7.AppCompat: Keine Ressource gefunden, die mit dem angegebenen Namen übereinstimmt: attr 'android:actionModeShareDrawable'](missing-action-mode-share-drawable.md)

Dieser Fehler kann in früheren Versionen von Xamarin auftreten, wenn einige der erforderlichen Android-SDK-Pacakages fehlen.

### <a name="adjusting-java-memory-parameters-for-the-android-designerandroid-designer-java-memorymd"></a>[Anpassen der Java-Speicherparameter für Android Designer](android-designer-java-memory.md)

Die Standardparameter für den Arbeitsspeicher, die verwendet werden, beim Starten der `java` für der Android-Designer möglicherweise nicht kompatibel mit einigen Systemkonfigurationen verarbeiten. Beginnend mit Xamarin Studio 5.7.2.7 und Xamarin für Visual Studio 3.9.344 diese Einstellungen kann auf der Basis eines pro Projekt angepasst werden.

### <a name="my-android-resourcedesignercs-file-will-not-updateresource-designer-wont-updatemd"></a>[Meine Android-Datei „Resource.designer.cs“ wird nicht aktualisiert](resource-designer-wont-update.md)

Ein Fehler im Xamarin.Studio 5.1 beschädigt zuvor CSPROJ-Dateien durch teilweise oder vollständig löschen den XML-Code in der CSPROJ-Datei. Dies würde dazu führen, dass wichtige Bestandteile der Android-Buildsystem (z. B. Aktualisieren der Android Resource.designer.cs) fehlschlägt. Zum Zeitpunkt der 5.1.4 stabile Version am 15. Juli, dieser Fehler behoben wurde. aber in vielen Fällen die Projektdatei manuell, wie in diesem Handbuch beschrieben behoben werden muss.



