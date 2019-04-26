---
ms.openlocfilehash: d46968f9c53314abe561e7f4871cfbf6e07b7002
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61047606"
---
## <a name="firewall-configuration"></a>Konfiguration der Firewall

In der Reihenfolge für Tests an Xamarin Test Cloud übermittelt werden muss der Computer, übermitteln die Tests mit den Test Cloud-Servern kommunizieren. Firewalls müssen konfiguriert werden, zum Zulassen des Netzwerkverkehrs zu und von den Servern am **testcloud.xamarin.com** an Port 80 und 443. Dieser Endpunkt wird von DNS verwaltet, und die IP-Adresse sind vorbehalten. 

In einigen Situationen muss einen Test (oder ein Gerät, das Ausführen des Tests) mit Webservern, die durch eine Firewall geschützt, kommunizieren. In diesem Szenario muss die Firewall zum Zulassen von Datenverkehr von den folgenden IP-Adressen konfiguriert werden:

* **195.249.159.238**
* **195.249.159.239**
