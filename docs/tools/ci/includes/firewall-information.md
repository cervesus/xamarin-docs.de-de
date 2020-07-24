---
ms.openlocfilehash: d46968f9c53314abe561e7f4871cfbf6e07b7002
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "61047606"
---
## <a name="firewall-configuration"></a>Firewall-Konfiguration

Damit Tests an Xamarin Test Cloud übermittelt werden können, muss der Computer, der die Tests sendet, mit den Test Cloud Servern kommunizieren können. Firewalls müssen konfiguriert werden, um Netzwerk Datenverkehr zu und von den Servern unter **testcloud.xamarin.com** an den Ports 80 und 443 zuzulassen. Dieser Endpunkt wird von DNS verwaltet, und die IP-Adresse kann geändert werden. 

In einigen Situationen muss ein Test (oder ein Gerät, auf dem der Test ausgeführt wird) mit Webservern kommunizieren, die durch eine Firewall geschützt sind. In diesem Szenario muss die Firewall so konfiguriert werden, dass Sie Datenverkehr von den folgenden IP-Adressen zulässt:

* **195.249.159.238**
* **195.249.159.239**
