---
title: Info.plist-Referenz für Xamarin.iOS
description: In diesem Dokument werden verschiedene Schlüssel/Wert-Paare beschrieben, die in der Info.plist-Datei einer Xamarin.iOS-App festgelegt werden können. Diese Schlüssel sind erforderlich, wenn ihre App bestimmte Tasks durchführt, z.B. das Zugreifen auf den Standort, Fotos, das Mikrofon oder die Kamera.
ms.prod: xamarin
ms.assetid: 944DFDB5-ADBA-4D6E-984C-5AEC19A1CC57
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 01/18/2017
ms.openlocfilehash: d94a647583539ac6af603a9074e1966c1c41d587
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73028466"
---
# <a name="infoplist-reference-for-xamarinios"></a>Info.plist-Referenz für Xamarin.iOS

Weitere Informationen zur Verwendung von Info.plist-Schlüsseln finden Sie im Artikel [Working with Security and Privacy (Arbeiten mit Sicherheit und Datenschutz)](~/ios/app-fundamentals/security-privacy.md). 

## <a name="location"></a>Speicherort 

Das Zugreifen auf den Standort des Benutzers erfordert Änderungen an „Info.plist“. Die folgenden Schlüssel, die mit den Standortdaten zusammenhängen, sollten festgelegt werden: 

- **NSLocationWhenInUseUsageDescription**: Für den Zugriff auf den Standort des Benutzers während dieser mit Ihrer App interagiert. 
- **NSLocationAlwaysUsageDescription**: Für den Zugriff Ihrer App auf den Standort des Benutzers aus dem Hintergrund.

## <a name="photos"></a>Fotos 

NSPhotoLibraryUsageDescription  

## <a name="contacts"></a>Kontaktpersonen 

NSContactsUsageDescription 

## <a name="calendar-data"></a>Kalenderdaten 
    
NSCalendarsUsageDescription 

## <a name="reminder-data"></a>Erinnerungsdaten 
    
NSRemindersUsageDescription 

## <a name="bluetooth-peripherals"></a>Bluetooth-Peripheriegeräte 
    
NSBluetoothPeripheralUsageDescription 

## <a name="microphone"></a>Mikrofon 

NSMicrophoneUsageDescription 

## <a name="camera"></a>Camera 
    
NSCameraUsageDescription 

## <a name="reading-healthkit"></a>Lesen von HealthKit  

NSHealthShareUsageDescription 

NSHealthUpdateUsageDescription 

## <a name="background-modes"></a>Hintergrundmodi 
    
UIBackgroundModes 

## <a name="homekit"></a>HomeKit 

NSHomeKitUsageDescription 

## <a name="related-links"></a>Verwandte Links

- [Apple-Referenzleitfaden](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW10)
