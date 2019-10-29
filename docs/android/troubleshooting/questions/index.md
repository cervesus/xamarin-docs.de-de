---
title: Häufig gestellte Fragen zu xamarin. Android
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 0F0FDD2B-FFB1-476F-B674-81DB3A5E1CF3
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/29/2018
ms.openlocfilehash: 35df724850e1fc945c096aebc91b7aa84936bdc1
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73026978"
---
# <a name="android-frequently-asked-questions"></a>Häufig gestellte Fragen zu Android

## <a name="installation--setup"></a>Installation & Setup

### <a name="which-android-sdk-packages-should-i-installinstall-android-sdk-packagesmd"></a>[Welche Android SDK-Pakete sollte ich installieren?](install-android-sdk-packages.md)

Die Installation des Android SDK umfasst nicht automatisch alle Pakete, die für die Entwicklung erforderlich sind. Während sich die einzelnen Entwickler benötigen, werden in diesem Handbuch die Pakete erläutert, die in der Regel für die Entwicklung mit xamarin. Android erforderlich sind.

### <a name="where-can-i-set-my-android-sdk-locationsandroid-sdk-locationmd"></a>[Wo kann ich meine Android SDK-Speicherorte festlegen?](android-sdk-location.md)

In diesem Handbuch werden die Standardeinstellungen des Android SDK beschrieben, die für die meisten Setups funktionieren sollten. und wie Sie diese Standardeinstellungen in Visual Studio für Mac oder Visual Studio ändern, falls erforderlich.

### <a name="how-do-i-update-the-java-development-kit-jdk-versionupdate-jdkmd"></a>[Wie aktualisiere ich die Java Development Kit-Version (JDK)?](update-jdk.md)

In diesem Artikel wird veranschaulicht, wie die Java Development Kit (JDK)-Version unter Windows und Mac aktualisiert wird.

### <a name="can-i-use-java-development-kit-jdk-version-9-or-laterjdk9-errorsmd"></a>[Kann ich Java Development Kit (JDK), Version 9 oder höher, verwenden?](jdk9-errors.md)

Xamarin. Android erfordert JDK 8 oder das Microsoft Mobile openjdk. In diesem Artikel werden einige häufige Fehlermeldungen aufgeführt, die möglicherweise angezeigt werden, wenn JDK 9 oder höher installiert ist, sowie Anweisungen zum Überprüfen der JDK-Version.

### <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packagesinstall-android-support-librarymd"></a>[Wie kann ich die Android-Unterstützungsbibliotheken, die für die Xamarin.Android.Support-Pakete erforderlich sind, manuell installieren?](install-android-support-library.md)

Dieses Handbuch enthält Beispiel Schritte für die Installation der `Xamarin.Android.Support.v4`-Unterstützungs Bibliothek unter Windows & Mac.

### <a name="what-usb-drivers-do-i-need-to-debug-android-on-windowsandroid-drivers-debug-windowsmd"></a>[Welche USB-Treiber sind zum Debuggen von Android unter Windows erforderlich?](android-drivers-debug-windows.md)

So debuggen Sie auf einem Android-Gerät bei der Entwicklung in Windows Sie müssen einen kompatiblen USB-Treiber installieren. Der Android SDK-Manager enthält standardmäßig den "Google-USB-Treiber", der Unterstützung für Nexus-Geräte bietet.
Für andere Geräte sind vom Gerätehersteller veröffentlichte USB-Treiber erforderlich. Dieses Handbuch enthält Informationen zum Ermitteln dieser Treiber und alternativer Testmethoden.

### <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vmconnect-android-emulator-mac-windowsmd"></a>[Ist es möglich, über eine Windows-VM mit den Android-Emulatoren, die auf einem Mac ausgeführt werden, eine Verbindung herzustellen?](connect-android-emulator-mac-windows.md)

Dieser Leitfaden behandelt die Methoden bei der Verwendung des Android-Emulators.

## <a name="general-questions"></a>Allgemeine Fragen

### <a name="how-do-i-automate-an-android-nunit-test-projectautomate-android-nunit-testmd"></a>[Wie automatisiere ich ein Android NUnit-Testprojekt?](automate-android-nunit-test.md)

Dieses Handbuch enthält die Schritte zum Einrichten eines Android nunit-Testprojekts, _nicht_ als xamarin. UITest-Projekt. Xamarin. UITest-Anleitungen finden Sie [hier](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

### <a name="why-cant-my-android-release-build-connect-to-the-internetandroid-internetmd"></a>[Warum kann mein Android-Releasebuild keine Verbindung mit dem Internet herstellen?](android-internet.md)

Die häufigste Ursache für dieses Problem ist, dass die **Internet** Berechtigung automatisch in einem Debugbuild enthalten ist, aber für einen Releasebuild manuell festgelegt werden muss. In diesem Leitfaden wird beschrieben, wie Sie die-Berechtigung für Releasebuilds aktivieren.

### <a name="smarter-xamarin-android-support-v4--v13-nuget-packagesandroid-support-v4v13-librariesmd"></a>[NuGet-Pakete für eine intelligentere Xamarin Android-Unterstützung v4/v13 ](android-support-v4v13-libraries.md)

`Support-v4` und `Support-v13` können nicht gemeinsam in derselben App verwendet werden, d. h., Sie schließen sich gegenseitig aus. Der Grund hierfür ist, dass `Support-v13` tatsächlich alle Typen und die Implementierung von `Support-v4`enthält. Wenn Sie versuchen, auf beide im gleichen Projekt zu verweisen, werden doppelte Typfehler auftreten.

### <a name="how-do-i-resolve-a-pathtoolongexception-errorpath-too-long-exceptionmd"></a>[Gewusst wie Auflösen eines PathTooLongException-Fehlers?](path-too-long-exception.md)

In diesem Artikel wird erläutert, wie Sie einen **PathTooLongException** -Fehler beheben, der beim Aufbau eines xamarin. Android-Projekts auftreten kann.

## <a name="deprecated"></a>Veraltet

> [!NOTE]
> Die folgenden Artikel gelten für Probleme, die in neueren Versionen von xamarin behoben wurden. Wenn das Problem jedoch in der aktuellen Version der Software auftritt, melden Sie einen [neuen Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md) mit den vollständigen Versionsinformationen und der vollständigen buildprotokolleausgabe.

### <a name="what-version-of-xamarinandroid-added-lollipop-supportxa-lollipopmd"></a>[In welcher Version von Xamarin.Android wurde Lollipop-Unterstützung hinzugefügt?](xa-lollipop.md)

Dieses Handbuch wurde ursprünglich für die Vorschauversion von Android L geschrieben. Xamarin. Android 4,17 hat die Unterstützung für Android L Preview & xamarin. Android 4,20 hinzugefügt, die Unterstützung für Android Lollipop hat.

### <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawablemissing-action-mode-share-drawablemd"></a>[Android.Support.v7.AppCompat: Keine Ressource gefunden, die mit dem angegebenen Namen übereinstimmt: attr 'android:actionModeShareDrawable'](missing-action-mode-share-drawable.md)

Dieser Fehler kann in älteren Versionen von xamarin auftreten, wenn einige der erforderlichen Android SDK Pakete fehlen.

### <a name="adjusting-java-memory-parameters-for-the-android-designerandroid-designer-java-memorymd"></a>[Anpassen der Java-Speicherparameter für Android Designer](android-designer-java-memory.md)

Die standardmäßigen Arbeitsspeicher Parameter, die beim Starten des `java` Prozesses für den Android-Designer verwendet werden, sind möglicherweise mit einigen Systemkonfigurationen nicht kompatibel. Ab Xamarin Studio 5.7.2.7 und xamarin für Visual Studio 3.9.344 können diese Einstellungen auf Projektbasis angepasst werden.

### <a name="my-android-resourcedesignercs-file-will-not-updateresource-designer-wont-updatemd"></a>[Meine Android-Datei „Resource.designer.cs“ wird nicht aktualisiert](resource-designer-wont-update.md)

Ein Fehler in xamarin. Studio 5,1 zuvor beschädigte csproj-Dateien, indem der XML-Code in der CSPROJ-Datei teilweise oder vollständig gelöscht wurde. Dies würde dazu führen, dass wichtige Teile des Android-Buildsystems (z. b. das Aktualisieren des Android-Resource.Designer.cs) fehlschlagen. Ab dem 15. Juli wurde dieser Fehler korrigiert. in vielen Fällen muss die Projektdatei jedoch manuell repariert werden, wie in diesem Handbuch beschrieben.
