---
title: Plattform- und Featureunterstützung in Xamarin.Essentials
description: Xamarin.Essentials stellt eine einzelne plattformübergreifende API bereit, die mit jeder iOS-, Android- und UWP-Anwendung kompatibel ist, auf die über freigegebenen Code zugegriffen werden kann, unabhängig davon, wie die Benutzeroberfläche erstellt wird.
ms.assetid: 63FA28A5-6F52-4CB7-AF39-8DF7B436B5A4
author: jamesmontemagno
ms.author: jamont
ms.date: 07/10/2019
ms.openlocfilehash: 2dadc9effb2433467609338d4654e784fe8b085e
ms.sourcegitcommit: 3434624a36a369986b6aeed7959dae60f7112a14
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2019
ms.locfileid: "69629588"
---
# <a name="platform-support"></a>Plattformunterstützung

Xamarin.Essentials unterstützt die folgenden Plattformen und Betriebssysteme:

| Plattform | Version |
| --- | --- |
| Android | 4.4 (API 19) oder höher |
| iOS |10.0 oder höher |
| Tizen | 4.0 oder höher |
| tvOS | 10.0 oder höher |
| watchOS | 4.0 oder höher |
| UWP | 10.0.16299.0 oder höher |

> [!NOTE]
> * Tizen wird vom Samsung-Entwicklungsteam offiziell unterstützt.
> * Für tvOS und watchOS stehen nicht alle APIs zur Verfügung. Weitere Informationen finden Sie im Featureleitfaden.
> * Tizen, tvOS, und watchOS befinden sich derzeit in der Vorschauphase und sind in der Xamarin.Essentials 1.3-Vorversion verfügbar.

## <a name="feature-support"></a>Featureunterstützung

In Xamarin.Essentials wird immer versucht, alle Features auf allen Plattformen bereitzustellen. Je nach Gerät kann es jedoch Einschränkungen geben. Im Folgenden finden Sie Informationen dazu, welche Features auf den einzelnen Plattformen unterstützt werden.

Symbole und ihre Bedeutung:

* ✔ – vollständig unterstützt
* ⚠ – eingeschränkt unterstützt
* ❌ – nicht unterstützt

| Feature | Android | iOS | UWP | watchOS | tvOS | Tizen |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| [Beschleunigungsmesser](accelerometer.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ❌ | ✔ |
| [App-Informationen](app-information.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Barometer](barometer.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ❌ | ✔ |
| [Akku](battery.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ⚠ | ⚠ |
| [Zwischenablage](clipboard.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ❌ |
| [Farbkonverter](color-converters.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Kompass](compass.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [Konnektivität](connectivity.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Erkennen einer Schüttelbewegung](detect-shake.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Geräteanzeigeinformationen](device-display.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ❌ |
| [Geräteinformationen](device-information.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [E-Mail](email.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [Dateisystemhilfsprogramme](file-system-helpers.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Taschenlampe](flashlight.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [Geocodierung](geocoding.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Geolocation](geolocation.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [Gyroskop](gyroscope.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ❌ | ✔ |
| [Startprogramm](launcher.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [Magnetometer](magnetometer.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ❌ | ✔ |
| [MainThread](main-thread.md?content=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Karten](maps.md?content=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ❌ | ✔ |
| [Browser öffnen](open-browser.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [Ausrichtungssensor](orientation-sensor.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ❌ | ✔ |
| [Wählhilfe](phone-dialer.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [Plattformerweiterungen](platform-extensions.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Einstellungen](preferences.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Sicherer Speicher](secure-storage.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Freigeben](share.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [SMS](sms.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [Text-zu-Sprache](text-to-speech.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Einheitenkonverter](unit-converters.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Versionsnachverfolgung](version-tracking.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Vibrieren](vibrate.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |

