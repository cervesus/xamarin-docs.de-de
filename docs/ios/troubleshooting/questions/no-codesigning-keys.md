---
title: 'Warum schlägt mein iOS-Build fehl mit: Keine gültigen iPhone-Codesignaturschlüssel in Keychain gefunden?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9DF24C46-D521-4112-9B21-52EA4E8D90D0
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: e10a04627b903c02140a6a2ead5c379c1e8bdcf6
ms.sourcegitcommit: 13e43f510da37ad55f1c2f5de1913fb0aede6362
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/16/2019
ms.locfileid: "71021390"
---
# <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychain"></a>Warum schlägt mein iOS-Build fehl mit: Keine gültigen iPhone-Codesignaturschlüssel in Keychain gefunden?

## <a name="cause-of-the-error"></a>Fehlerursache

Diese Fehlermeldung tritt auf, wenn das betreffende Projekt nach gültigen Code Signatur Anmelde Informationen sucht, diese jedoch nicht finden kann. Code Signierung ist für Tests und bereit Stellungen auf physischen IOS-Geräten erforderlich. ebenso wie Ad-hoc-& App Store-Builds.

### <a name="provisioning-devices"></a>Bereitstellen von Geräten

Wenn Sie noch kein IOS-Gerät bereitgestellt haben, werden Sie im folgenden Leitfaden durch den vollständigen Schritt-für-Schritt-Vorgang geleitet: [Handbuch zur Geräte Bereitstellung](~/ios/get-started/installation/device-provisioning/index.md)

## <a name="bug-when-using-ios-simulator"></a>Fehler bei Verwendung des IOS-Simulators

> [!NOTE]
> Dieses Problem wurde in den neuesten Versionen von xamarin für Visual Studio behoben. Wenn das Problem jedoch in der aktuellen Version der Software auftritt, melden Sie einen [neuen Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md) mit den vollständigen Versionsinformationen und der vollständigen buildprotokolleausgabe.

In xamarin. Visual Studio 3,11 ist ein Fehler aufgetreten, der bewirkt hat, dass das IOS-Projekt in einer xamarin. Forms-Vorlage die Codesign-Berechtigungen. plist zu simulatorbuilds hinzugefügt hat. effektive Blockierung von Tests mithilfe des Simulators.

### <a name="how-to-fix"></a>Vorgehensweise beim Beheben

Sie können das Problem umgehen, indem Sie `<CodesignEntitlements>` das-Flag aus den Debugbuilds in der CSPROJ-Datei entfernen. Sie können dies wie folgt tun:

> [!WARNING]
> Fehler in csproj-Dateien können das Projekt unterbrechen. Daher empfiehlt es sich, Ihre Dateien zu sichern, bevor Sie dies versuchen.

1. Klicken Sie im Lösungs Bereich mit der rechten Maustaste auf das IOS-Projekt, und wählen Sie **Projekt**
2. Klicken Sie mit der rechten Maustaste erneut auf das Projekt, und wählen Sie **[Projektname]. csproj bearbeiten.**
3. Suchen Sie die Debug PropertyGroups, Sie sollten mit Flags beginnen, die wie folgt aussehen:
   - Gen`<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">`
   - Abgabe`<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhoneSimulator' ">`
4. Löschen Sie in jedem der Builds, die den Simulator verwenden, die folgende Eigenschaft, oder kommentieren Sie Sie aus:`<CodesignEntitlements>Entitlements.plist</CodesignEntitlements>`
5. Laden Sie das Projekt neu, und Sie sollten in der Lage sein, Sie im Simulator bereitzustellen.

### <a name="next-steps"></a>Nächste Schritte
Weitere Unterstützung erhalten Sie bei der Kontaktaufnahme mit uns, oder wenn das Problem weiterhin besteht, auch nachdem Sie die oben genannten Informationen genutzt haben, finden Sie [unter welche Supportoptionen für xamarin verfügbar sind?](~/cross-platform/troubleshooting/support-options.md) Informationen zu Kontaktoptionen, Vorschlägen und wie Sie bei Bedarf einen neuen Fehler melden. .
