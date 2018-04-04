---
title: 'Warum schlägt mein iOS-Build fehl mit: kein Signaturschlüssel gültig iPhone-Code in einem Schlüssel gefunden?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9DF24C46-D521-4112-9B21-52EA4E8D90D0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 5334e3009906896644caa47c715f912fa379c627
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychain"></a>Warum schlägt mein iOS-Build fehl mit: kein Signaturschlüssel gültig iPhone-Code in einem Schlüssel gefunden?

## <a name="cause-of-the-error"></a>Ursache des Fehlers
Diese Fehlermeldung tritt auf, wenn das betreffende Projekt gültigen Anmeldeinformationen im Code-signing sucht jedoch nicht an diese zu finden. Signieren von Code ist für das Testen und Bereitstellungen auf physischen iOS-Geräten erforderlich. als auch Ad-hoc- & Anwendung Builds zu speichern. 


### <a name="provisioning-devices"></a>Bereitstellung von Geräten
Vor einem iOS-Gerät noch nicht bereitgestellt wird, die folgenden Schritte gelangen Sie durch den Prozess vollständiges schrittweises: [Handbuch für Geräte bereitstellen](~/ios/get-started/installation/device-provisioning/index.md)


## <a name="bug-when-using-ios-simulator"></a>Fehler bei Verwendung von iOS-Simulator

> [!NOTE]
> Dieses Problem wurde in den neuesten Versionen von Xamarin für Visual Studio behoben. Allerdings tritt das Problem auf die neueste Version der Software, bitte der Datei eine [neuen Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md) mit Ihrer vollständigen versionsverwaltung-Informationen "und" vollständig "Ausgabeprotokoll erstellen.


Xamarin.Visual Studio 3.11 Dies verursachte den iOS-Projekt in einer Xamarin.Forms-Vorlage hinzufügen, erstellt der Codesign Entitlements.plist für den Simulator ist ein Fehler aufgetreten; Testen des Simulators effektiv, blockieren.

### <a name="how-to-fix"></a>Gewusst wie: beheben
Sie können die Umgehung des Problems durch das Entfernen der `<CodesignEntitlements>` Flag aus der Debug-builds in der CSPROJ-Datei. Sie können dies wie folgt tun:

*Warnung: Fehler in der CSPROJ-Dateien können Ihres Projekts, unterbrechen, daher ist es ratsam, Ihre Dateien zu sichern, bevor Sie es erneut versuchen.*

1. Klicken Sie mit der mit der rechten Maustaste auf das iOS-Projekt im Projektmappen-Bereich, und wählen Sie **Projekt entladen**
2. Mit der rechten Maustaste auf das Projekt erneut, und wählen Sie **[Projektname] .csproj bearbeiten**
3. Suchen Sie das Debuggen PropertyGroups, sie sollten zunächst Flags an, die wie folgt aussehen:
   - Debuggen: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">`
   - Version: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhoneSimulator' ">`
4. Löschen Sie in jedem der Builds, die der Simulator verwendet wird, oder kommentieren Sie die folgende Eigenschaft: `<CodesignEntitlements>Entitlements.plist</CodesignEntitlements>`
5. Das Projekt erneut laden, und Sie sollten für den Simulator bereitstellen werden.

### <a name="next-steps"></a>Nächste Schritte
Für weitere Unterstützung zu erhalten, wenden Sie sich an uns, oder bleibt dieses Problem auch nach der Nutzung der oben angegebenen Informationen finden Sie unter [welche Supportoptionen für Xamarin verfügbar sind?](~/cross-platform/troubleshooting/support-options.md) Informationen zu Kontaktoptionen Vorschläge, sowie zur die Datei eines neuen Fehlers bei Bedarf. 
