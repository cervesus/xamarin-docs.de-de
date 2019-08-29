---
title: Gerätebereitstellung für Xamarin.iOS
description: In diesem Artikel wird beschrieben, wie Sie ein Gerät bereitstellen, damit es zum Testen einer Anwendung verwendet werden kann. Außerdem wird erläutert, wie Sie eine App konfigurieren, damit sie Funktionen wie Pushbenachrichtigungen verwenden kann.
ms.prod: xamarin
ms.assetid: CACA5236-3C90-F6DF-FD4E-0797B61670CE
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 05/06/2018
ms.openlocfilehash: 477bb7289588d0ab1b1dd29909c7dc36f2b2dfac
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70121427"
---
# <a name="device-provisioning-for-xamarinios"></a>Gerätebereitstellung für Xamarin.iOS

Beim Entwickeln einer Xamarin.iOS-Anwendung ist es wichtig diese durch Bereitstellen der App auf einem physischen Gerät zu testen, zusätzlich zur Ausführung der Anwendung im Simulator. Bei Ausführung auf einem Gerät kann es aufgrund von Hardwarebeschränkungen, z.B. in Bezug auf den Arbeitsspeicher oder die Netzwerkkonnektivität, zu gerätespezifischen Problemen und Leistungseinbußen kommen. Um auf einem physischen Gerät zu testen, muss das Gerät *bereitgestellt* und Apple darüber informiert werden, dass das Gerät für Tests verwendet wird.

Die hervorgehobenen Abschnitte in der folgenden Abbildung zeigen, welche Schritte vor der iOS-Bereitstellung erforderlich sind:

[![](images/provisioningdiagram.png "Die hervorgehobenen Abschnitte in dieser Abbildung zeigen, welche Schritte vor der iOS-Bereitstellung erforderlich sind")](images/provisioningdiagram.png#lightbox)

Der nächste Schritt besteht dann in der Verteilung der Anwendung. Weitere Informationen zur Bereitstellung finden Sie in den Leitfäden zur [App Distribution (App-Verteilung)](~/ios/deploy-test/app-distribution/index.md).

Bevor Sie die Anwendung auf einem Gerät bereitstellen, benötigen Sie ein aktives Abonnement für das Apple Developer Program, *oder* Sie verwenden [Free Provisioning (Kostenlose Bereitstellung)](~/ios/get-started/installation/device-provisioning/free-provisioning.md). Apple bietet zwei Optionen für das Programm:

- **Apple Developer Program**: Unabhängig davon, ob Sie eine Einzelperson oder eine Organisation sind, ermöglicht Ihnen das Apple Developer Program die Entwicklung, das Testen und das Verteilen von Apps.
- **Apple Developer Enterprise Program**: Das Enterprise-Programm eignet sich am besten für Organisationen, die Apps ausschließlich intern entwickeln und verteilen möchten. Mitglieder des Enterprise-Programms haben keinen Zugriff auf iTunes Connect, und erstellte Apps können nicht im App Store veröffentlicht werden.

Um sich für eines dieser Programme zu registrieren, besuchen Sie das [Apple Developer Portal](https://developer.apple.com/programs/enroll/). Beachten Sie, dass Sie eine [Apple-ID](https://appleid.apple.com/) benötigen, um sich als Apple-Entwickler registrieren zu können. In diesem Leitfaden wird vorausgesetzt, dass Sie Mitglied in einem Apple Developer Program **sind**.

Alternativ hat Apple das [Free Provisioning (Kostenlose Bereitstellung)](~/ios/get-started/installation/device-provisioning/free-provisioning.md) in Xcode 7 eingeführt, das es ermöglicht, *ohne* eine Mitgliedschaft im Apple Developer Program eine einzelne Anwendung auf einem einzelnen Gerät auszuführen. Bei dieser Art der Bereitstellung gibt es eine Vielzahl von Einschränkungen, die [hier](~/ios/get-started/installation/device-provisioning/free-provisioning.md#limitations) ausführlich beschrieben sind.

Jede auf einem Gerät ausgeführte Anwendung muss eine Reihe von Metadaten (oder einen *Fingerabdruck*) enthalten, die Informationen über die Anwendung und den Entwickler geben. Apple verwendet diesen Fingerabdruck, um sicherzustellen, dass die Anwendung bei der Bereitstellung oder beim Ausführen auf dem Gerät eines Benutzers nicht manipuliert wird. Dies wird erreicht, indem App-Entwickler verpflichtet werden, ihre Apple-ID als Entwickler zu registrieren, eine App-ID einzurichten, ein „Zertifikat“ anzufordern und das Gerät zu registrieren, auf dem die Anwendung bereitgestellt wird.

Wenn Sie eine Anwendung auf einem Gerät bereitstellen, wird auf dem iOS-Gerät auch ein Bereitstellungsprofil installiert. Das Bereitstellungsprofil dient dazu, die Informationen zu überprüfen, mit denen die App zur Buildzeit signiert wurde, und wird von Apple kryptografisch signiert. Die Prüfungen des Bereitstellungsprofils und des „Fingerabdrucks“ bestimmen, ob eine Anwendung auf einem Gerät bereitgestellt werden darf, indem die folgenden Schritte ausgeführt werden:

- **Wer**? (Zertifikate: Wurde die App mit einem privaten Schlüssel signiert, der einen entsprechenden öffentlichen Schlüssel im Bereitstellungsprofil aufweist? Das Zertifikat ordnet den Entwickler auch einem Entwicklungsteam zu)
- **Was**? (Individuelle App-ID: Stimmt der in „Info.plist“ festgelegte Bundle-Bezeichner mit der App-ID im Bereitstellungsprofil überein?)
- **Wo**? (Gerät: Ist das Gerät im Bereitstellungsprofil enthalten?)

Anhand dieser Schritte stellen Sie sicher, dass alle Komponenten, die während des Entwicklungsprozesses erstellt oder verwendet werden, einschließlich der Anwendungen und Geräte, einem Apple Developer-Konto zugeordnet werden können.

## <a name="provisioning-your-device"></a>Bereitstellen Ihres Geräts

Es gibt zwei Möglichkeiten zum Bereitstellen Ihres iOS-Geräts:

- **Automatisch (Empfohlen)** : Wählen Sie das Schema **Automatische Bereitstellung** in Ihrem Projekt aus, um Visual Studio Ihre Signieridentitäten, App-IDs und Bereitstellungsprofile automatisch erstellen und verwalten zu lassen. Weitere Informationen zur automatischen Verwaltung der Bereitstellung finden Sie im Leitfaden [Automatische Bereitstellung](automatic-provisioning.md). Dies ist die empfohlene Methode der Bereitstellung eines iOS-Geräts.

- **Manuell**: Signieridentitäten, App-IDs und Bereitstellungsprofile können über das Apple Developer Portal erstellt und verwaltet werden, wie im Leitfaden [Manuelle Bereitstellung](manual-provisioning.md) beschrieben. Diese Artefakte können dann wie im Leitfaden [Apple Account Management](~/cross-platform/macios/apple-account-management.md) beschrieben verwaltet werden.

## <a name="provisioning-for-application-services"></a>Bereitstellung für Anwendungsdienste

Apple stellt eine Auswahl an speziellen Anwendungsdiensten, auch Funktionen genannt, bereit, die für eine Xamarin.iOS-Anwendung aktiviert werden können. Diese Anwendungsdienste müssen im iOS-Bereitstellungsportal beim Erstellen der **App-ID** und auch in der **Entitlements.plist**-Datei, die Teil des Xamarin.iOS-Anwendungsprojekts ist, konfiguriert werden. Informationen zum Hinzufügen von Anwendungsdiensten zu Ihrer App finden Sie in den Leitfäden [Introduction to Capabilities (Einführung in Funktionen)](~/ios/deploy-test/provisioning/capabilities/index.md) und [Working with Entitlements (Arbeiten mit Berechtigungen)](~/ios/deploy-test/provisioning/entitlements.md).

- Erstellen Sie eine App-ID mit den erforderlichen App-Diensten.
- Erstellen Sie ein neues [Bereitstellungsprofil](#provisioning-your-device), das diese App-ID enthält.
- Legen Sie Berechtigungen im Xamarin.iOS-Projekt fest

## <a name="related-links"></a>Verwandte Links

- [Free Provisioning (Kostenlose Bereitstellung)](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [App-Verteilung](~/ios/deploy-test/app-distribution/index.md)
- [Problembehandlung](~/ios/deploy-test/troubleshooting.md)
- [Apple: Leitfaden zur App-Verteilung](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
