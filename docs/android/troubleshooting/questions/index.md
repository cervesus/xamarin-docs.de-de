---
title: Häufig gestellte Fragen zu Xamarin.Android
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 0F0FDD2B-FFB1-476F-B674-81DB3A5E1CF3
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/29/2018
ms.openlocfilehash: 28b13951a0d681ffdb8e643667e496e0b3606628
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "76724002"
---
# <a name="android-frequently-asked-questions"></a>Häufig gestellte Fragen zu Android

## <a name="installation--setup"></a>Installation und Setup

### <a name="which-android-sdk-packages-should-i-install"></a>[Welche Android SDK-Pakete sollte ich installieren?](install-android-sdk-packages.md)

Bei der Installation des Android SDKs werden nicht automatisch alle Pakete installiert, die für die Entwicklung erforderlich sind. Obwohl die Anforderungen der einzelnen Entwickler variieren, werden in dieser Anleitung die Pakete erläutert, die in der Regel für die Entwicklung mit Xamarin.Android benötigt werden.

### <a name="where-can-i-set-my-android-sdk-locations"></a>[Wo kann ich meine Android SDK-Speicherorte festlegen?](android-sdk-location.md)

In dieser Anleitung werden die Standardeinstellungen des Android SDK beschrieben, die für die meisten Setups funktionieren sollten. Darüber hinaus wird beschrieben, wie diese Standardeinstellungen bei Bedarf in Visual Studio für Mac oder Visual Studio geändert werden.

### <a name="how-do-i-update-the-java-development-kit-jdk-version"></a>[Wie aktualisiere ich die Java Development Kit-Version (JDK)?](update-jdk.md)

In diesem Artikel wird erklärt, wie die Java Development Kit-Version (JDK) unter Windows und Mac aktualisiert wird.

### <a name="can-i-use-java-development-kit-jdk-version-9-or-later"></a>[Kann ich die Java Development Kit (JDK)-Version 9 oder höher verwenden?](jdk9-errors.md)

Xamarin.Android erfordert JDK 8 oder das Microsoft Mobile OpenJDK. In diesem Artikel werden einige häufige Fehlermeldungen, die möglicherweise angezeigt werden, wenn JDK 9 oder höher installiert ist, sowie Anweisungen zum Überprüfen der JDK-Version aufgelistet.

### <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packages"></a>[Wie kann ich die Android-Unterstützungsbibliotheken, die für die Xamarin.Android.Support-Pakete erforderlich sind, manuell installieren?](install-android-support-library.md)

Diese Anleitung enthält Beispielschritte für die Installation der `Xamarin.Android.Support.v4`-Unterstützungsbibliothek unter Windows und Mac.

### <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>[Welche USB-Treiber sind zum Debuggen von Android unter Windows erforderlich?](android-drivers-debug-windows.md)

Zum Debuggen auf einem Android-Gerät bei der Entwicklung in Windows müssen Sie einen kompatiblen USB-Treiber installieren. Der Android SDK-Manager enthält standardmäßig den „Google-USB-Treiber“, der Unterstützung für Nexus-Geräte bietet.
Für andere Geräte sind vom jeweiligen Gerätehersteller veröffentlichte USB-Treiber erforderlich. Diese Anleitung enthält Informationen zum Ermitteln dieser Treiber sowie zu alternativen Testmethoden.

### <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vm"></a>[Ist es möglich, über eine Windows-VM mit den Android-Emulatoren, die auf einem Mac ausgeführt werden, eine Verbindung herzustellen?](connect-android-emulator-mac-windows.md)

In dieser Anleitung werden die Methoden bei Verwendung des Android-Emulators behandelt.

## <a name="general-questions"></a>Allgemeine Fragen

### <a name="how-do-i-automate-an-android-nunit-test-project"></a>[Wie automatisiere ich ein Android NUnit-Testprojekt?](automate-android-nunit-test.md)

In dieser Anleitung werden die Schritte zum Einrichten eines Android NUnit-Testprojekts beschrieben, _nicht_ die Schritte für ein Xamarin.UITest-Projekt. Die Xamarin.UITest-Anleitungen finden Sie [hier](/appcenter/test-cloud/preparing-for-upload).

### <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>[Warum kann mein Android-Releasebuild keine Verbindung mit dem Internet herstellen?](android-internet.md)

Die häufigste Ursache für dieses Problem besteht darin, dass die **INTERNET**-Berechtigung automatisch in einem Debugbuild enthalten ist, für einen Releasebuild aber manuell festgelegt werden muss. In dieser Anleitung wird beschrieben, wie Sie die Berechtigung für Releasebuilds aktivieren.

### <a name="smarter-xamarin-android-support-v4--v13-nuget-packages"></a>[NuGet-Pakete für eine intelligentere Xamarin Android-Unterstützung v4/v13 ](android-support-v4v13-libraries.md)

`Support-v4` und `Support-v13` können nicht zusammen in der gleichen App verwendet werden, sie schließen sich gegenseitig aus. Dies liegt daran, dass `Support-v13` alle Typen und die Implementierung von `Support-v4` enthält. Wenn Sie in einem Projekt auf beide Bibliotheken verweisen, werden Fehler wegen doppelter Typen zurückgegeben.

### <a name="how-do-i-resolve-a-pathtoolongexception-error"></a>[Wie behebe ich einen PathTooLongException-Fehler?](path-too-long-exception.md)

In diesem Artikel wird erläutert, wie ein **PathTooLongException-** Fehler behoben wird, der beim Kompilieren eines Xamarin.Android-Projekts auftreten kann.

## <a name="deprecated"></a>Als veraltet markiert

> [!NOTE]
> Die folgenden Artikel beziehen sich auf Probleme, die in den neuesten Versionen von Xamarin behoben wurden. Melden Sie bitte einen [neuen Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md) mit allen Versionsinformationen und der vollständigen Buildprotokollausgabe, wenn das Problem in der aktuellen Version der Software auftreten sollte.

### <a name="what-version-of-xamarinandroid-added-lollipop-support"></a>[In welcher Version von Xamarin.Android wurde Lollipop-Unterstützung hinzugefügt?](xa-lollipop.md)

Diese Anleitung wurde ursprünglich für die Vorschauversion von Android L geschrieben, die ab Xamarin.Android 4.17 unterstützt wird. Ab Xamarin.Android 4.20 wird Android Lollipop unterstützt.

### <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>[Android.Support.v7.AppCompat: Keine Ressource gefunden, die mit dem angegebenen Namen übereinstimmt: attr 'android:actionModeShareDrawable'](missing-action-mode-share-drawable.md)

Dieser Fehler kann in älteren Versionen von Xamarin auftreten, wenn einige der erforderlichen Android SDK-Pakete fehlen.

### <a name="adjusting-java-memory-parameters-for-the-android-designer"></a>[Anpassen der Java-Speicherparameter für Android Designer](android-designer-java-memory.md)

Die Standardspeicherparameter, die beim Starten des `java`-Prozesses für den Android-Designer verwendet werden, sind möglicherweise mit einigen Systemkonfigurationen nicht kompatibel. Ab Xamarin Studio 5.7.2.7 und Xamarin für Visual Studio 3.9.344 können diese Einstellungen projektbezogen angepasst werden.

### <a name="my-android-resourcedesignercs-file-will-not-update"></a>[Meine Android-Datei „Resource.designer.cs“ wird nicht aktualisiert](resource-designer-wont-update.md)

Ein Fehler in Xamarin.Studio 5.1 hat zuvor zu beschädigten CSPROJ-Dateien geführt, da der XML-Code in der CSPROJ-Datei teilweise oder vollständig gelöscht wurde. Dies führte dazu, dass wichtige Komponenten des Android-Buildsystems (wie die Aktualisierung der Resource.designer.cs-Datei) fehlerhaft waren. Seit Veröffentlichung der stabilen Version 5.1.4 am 15. Juli tritt dieser Fehler nicht mehr auf. Allerdings muss die Projektdatei in vielen Fällen wie in dieser Anleitung beschrieben manuell repariert werden.
