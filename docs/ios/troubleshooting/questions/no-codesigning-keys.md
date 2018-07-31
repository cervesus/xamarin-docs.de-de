---
title: 'Warum schlägt mein iOS-Build fehl mit: keine gültigen iPhone-codesignierungsschlüssel in Keychain gefunden?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9DF24C46-D521-4112-9B21-52EA4E8D90D0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 75f9a78fdc7d15df217378491f016478cde369ff
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351098"
---
# <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychain"></a>Warum schlägt mein iOS-Build fehl mit: keine gültigen iPhone-codesignierungsschlüssel in Keychain gefunden?

## <a name="cause-of-the-error"></a>Ursache des Fehlers
Diese Fehlermeldung tritt auf, wenn das betreffende Projekt gültige Anmeldeinformationen im Code-signing sucht jedoch nicht zu finden. Signieren von Code ist für Tests und Bereitstellungen auf physischen iOS-Geräten erforderlich. als auch Ad-hoc- und App Builds zu speichern. 


### <a name="provisioning-devices"></a>Bereitstellung von Geräten
Wenn Sie ein iOS-Gerät, bevor Sie bereitgestellt haben, der folgende Leitfaden gelangen Sie durch den Prozess der vollständiges schrittweises: [Bereitstellung für Geräte](~/ios/get-started/installation/device-provisioning/index.md)


## <a name="bug-when-using-ios-simulator"></a>Fehler bei Verwendung von iOS-Simulator

> [!NOTE]
> Dieses Problem wurde in früheren Versionen von Xamarin für Visual Studio behoben. Jedoch, wenn das Problem auf die neueste Version der Software auftritt, melden Sie bitte eine [neuer Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md) erstellen Sie mit der Ihre vollständige versionsverwaltung Informationen "und" vollständig "Protokollausgabe.


Gab es ein Fehler in Xamarin.Visual Studio Version 3.11 der verursacht das iOS-Projekt in einer Xamarin.Forms-Vorlage hat hinzufügen, dass das Codesign "Entitlements.plist", um den Simulator erstellen; Testen die Verwendung des Simulators Verkehr wirkungsvoll gesperrt.

### <a name="how-to-fix"></a>Gewusst wie: beheben
Sie können die Umgehung des Problems durch das Entfernen der `<CodesignEntitlements>` Flag aus der Debug-builds in die CSPROJ-Datei. Sie können dies wie folgt tun:

*Warnung: Fehler in der CSPROJ-Dateien können Ihrem Projekt, unterbrechen, daher ist es ratsam, Ihre Dateien zu sichern, bevor Sie versuchen, dies.*

1. Klicken Sie mit der rechten Maustaste auf das iOS-Projekt im projektmappenbereich, und wählen Sie **Projekt entladen**
2. Erneut rechten Maustaste auf das Projekt, und wählen Sie **[Projektname] .csproj bearbeiten**
3. Suchen Sie das Debuggen PropertyGroups, sie sollten zunächst Flags, die wie folgt aussehen:
   - Debuggen: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">`
   - Version: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhoneSimulator' ">`
4. Klicken Sie in den einzelnen Builds, die der Simulator verwendet wird, löschen Sie, oder kommentieren Sie die folgende Eigenschaft: `<CodesignEntitlements>Entitlements.plist</CodesignEntitlements>`
5. Laden Sie das Projekt, und Sie sollten in der Lage, die für den Simulator bereitgestellt.

### <a name="next-steps"></a>Nächste Schritte
Weitere Unterstützung benötigen, kontaktieren uns, oder wenn dieses Problem bestehen bleibt, auch nach der Verwendung der oben genannten Informationen finden Sie unter [welche Supportoptionen für Xamarin verfügbar sind?](~/cross-platform/troubleshooting/support-options.md) auf Kontaktoptionen, Vorschläge, Informationen sowie zur einen neuen Bug-Datei bei Bedarf. 
