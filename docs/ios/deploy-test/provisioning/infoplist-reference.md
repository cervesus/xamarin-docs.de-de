---
title: Info.plist-Referenz für Xamarin.iOS
description: In diesem Dokument werden verschiedene Schlüssel/Wert-Paare beschrieben, die in der Info.plist-Datei einer Xamarin.iOS-App festgelegt werden können. Diese Schlüssel sind erforderlich, wenn ihre App bestimmte Tasks durchführt, z.B. das Zugreifen auf den Standort, Fotos, das Mikrofon oder die Kamera.
ms.prod: xamarin
ms.assetid: 944DFDB5-ADBA-4D6E-984C-5AEC19A1CC57
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 01/18/2017
ms.openlocfilehash: 654eca1098f9486e0c41fd296b3f8d381ac7ea34
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105375"
---
# <a name="infoplist-reference-for-xamarinios"></a>Info.plist-Referenz für Xamarin.iOS

Weitere Informationen zur Verwendung von Info.plist-Schlüsseln finden Sie im Artikel [Working with Security and Privacy (Arbeiten mit Sicherheit und Datenschutz)](~/ios/app-fundamentals/security-privacy.md). 

## <a name="location"></a>Speicherort 

Das Zugreifen auf den Standort des Benutzers erfordert Änderungen an „Info.plist“. Die folgenden Schlüssel, die mit den Standortdaten zusammenhängen, sollten festgelegt werden: 

* **NSLocationWhenInUseUsageDescription**: Für den Zugriff auf den Standort des Benutzers während dieser mit Ihrer App interagiert. 
* **NSLocationAlwaysUsageDescription**: Für den Zugriff Ihrer App auf den Standort des Benutzers aus dem Hintergrund.

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

## <a name="camera"></a>Kamera 
    
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
