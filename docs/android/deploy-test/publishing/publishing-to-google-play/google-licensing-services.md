---
title: Google-Lizenzierungsdienste
ms.prod: xamarin
ms.assetid: E96BDCC3-454A-A797-5819-905E2BB1AC41
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 12/20/2017
ms.openlocfilehash: eedfcfe2ed274ddf541addec67e66250deab7899
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114625"
---
# <a name="google-licensing-services"></a>Google-Lizenzierungsdienste

Vor der Einführung von Google Play waren Android-Anwendungen von älteren Kopierschutzmöglichkeiten, die von Google Market angeboten wurden, abhängig, um sicherstellen zu können, dass nur autorisiere Benutzer die Anwendungen auf ihren Geräten ausführen konnten. Der Kopierschutzmechanismus war aufgrund seiner Einschränkungen keine ideale Anwendungsschutzlösung.

Die Google-Lizenzierungdienste dienen als Ersatz für diesen älteren Kopierschutzmechanismus.
Die Google-Lizenzierungsdienste sind flexibel, sicher und netzwerkbasiert, und können von den Android-Anwendungen abgefragt werden, um zu bestimmen, ob eine Anwendung zur Ausführung auf einem bestimmten Gerät lizenziert ist.

Die Google-Lizenzierungsdienste sind dahingehend flexibel, dass Android-Anwendungen die volle Kontrolle darüber haben, wann und wie oft die Lizenz überprüft, und wie die Antwort des Lizenzierungsservers bearbeitet werden soll.

Die Google-Lizenzierungsdienste sind dahingehend sicher, dass jede Antwort mit einem RSA-Schlüsselpaar signiert wird, das ausschließlich zwischen dem Google Play-Server und der Anwendung freigegeben wird. Google Play stellt einen öffentlichen Schlüssel für Entwickler zur Verfügung, der in der Android-Anwendung eingebettet ist und zur Authentifizierung der Antworten verwendet wird. Der Google Play-Server behält den privaten Schlüssel intern.

Eine Anwendung, die die Google-Lizenzierung implementiert hat, sendet eine Anforderung an einen Dienst, der von der Google Play-Anwendung auf dem Gerät gehostet wird. Google Play sendet diese Anforderung weiter an den Google-Lizenzierungsserver, der daraufhin mit dem Lizenzstatus antwortet: 

[![Lizenzierungsserver-Workflowdiagramm](google-licensing-services-images/gp-licensing-service-overview.png)](google-licensing-services-images/gp-licensing-service-overview.png#lightbox)

In dem obenstehenden Diagramm wird der folgende Workflow dargestellt: 

-   Die Anwendung stellt einen Paketnamen, eine *Nonce* (ein kryptografischer Authentifikator), die zur Überprüfung der Serverantwort verwendet wird, und einen Rückruf, der die Antwort asynchron bearbeiten kann, bereit. 

-   Google Play stellt u.a. Informationen über das Google-Konto, das Gerät an sich und die IMSI-Nummer zur Verfügung. 

Die Google-Lizenzierungsdienste sind ebenfalls Kernkomponenten von APK-Erweiterungsdateien (auf die an späterer Stelle in diesem Dokument eingegangen wird). APK-Erweiterungsdateien verwenden Google-Lizenzierungsdienste, um die URL der Dateien, die heruntergeladen werden sollen, abzurufen.


## <a name="requirements"></a>Anforderungen

Anwendungen, die nicht über Google Play erworben werden, profitieren nicht von den Google-Lizenzierungsdiensten. Wenn Google Play nicht auf einem Gerät installiert ist, werden die Anwendungen, die Lizenzierungsdienste verwenden, weiterhin wie gewohnt auf dem Gerät ausgeführt.

Google Play muss auf das Internet zugreifen können, um funktionieren zu können. Eine Anwendung kann die Lizenz zwischenspeichern, damit Szenarios berücksichtigt werden, in denen das Gerät keinen Zugriff auf die Google Play-Lizenzierungsserver hat.

Kostenlose Anwendungen erfordern nur eine Google-Lizenzierung, wenn die Anwendung APK-Erweiterungsdateien verwendet.
