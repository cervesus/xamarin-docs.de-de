---
title: tvos-Ressourcen und-Datenspeicherung in xamarin
description: In diesem Artikel wird beschrieben, wie Sie Ressourcen und persistente Datenspeicherung in einer mit xamarin erstellten tvos-App bearbeiten. Es werden icloud-Datenspeicherung und Bedarfs gesteuerte Ressourcen erläutert.
ms.prod: xamarin
ms.assetid: C56B5046-D2C0-4B63-9CE0-ADAA0EFD368A
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 8619fa73a4dbaabe1e161c634b6a794b701d5135
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84570857"
---
# <a name="tvos-resources-and-data-storage-in-xamarin"></a>tvos-Ressourcen und-Datenspeicherung in xamarin

_In diesem Artikel wird das Arbeiten mit Ressourcen und der persistenten Datenspeicherung in einer xamarin. tvos-App behandelt._

<a name="tvOS-Resource-Limitations"></a>

## <a name="tvos-resource-limitations"></a>Einschränkungen der tvos-Ressourcen

Anders als bei IOS-Geräten bietet das neue Apple TV einen extrem eingeschränkten permanenten, lokalen Speicher für tvos-Apps oder-Daten. Für sehr kleine Elemente (z. b. Benutzereinstellungen) hat ihre tvos-App weiterhin Zugriff auf `NSUserDefaults` mit einem [Grenzwert von Daten von 500 KB](https://forums.developer.apple.com/message/50696#50696). Wenn Ihre xamarin. tvos-App jedoch größere Mengen von Informationen beibehalten muss, muss Sie diese Daten aus [icloud](#iCloud-Data-Storage)speichern und abrufen.

Außerdem schränkt tvos die Größe einer Apple TV-App auf 200 MB ein. Wenn Ihre APP Ressourcen erfordert, die diese Größe überschreiten, müssen Sie mit [bedarfsgesteuerten Ressourcen](#On-Demand-Resources) (bis zu zusätzlichen 2 GB) verpackt und geladen werden. Angesichts dieser Einschränkungen ist es wichtig, dass Sie das Herunterladen zusätzlicher Assets ordnungsgemäß durcharbeiten, um den Benutzern Ihrer APP die beste Leistung zu bieten. Weitere Informationen finden Sie [im Handbuch zu on-Demand-Ressourcen von](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083)Apple.

<a name="Non-Persistent-Downloads"></a>

## <a name="non-persistent-downloads"></a>Nicht persistente Downloads

Jeder tvos-APP wird ein temporäres Cache Verzeichnis bereitgestellt, in das die zusätzlichen Ressourcen und Assets heruntergeladen werden. Dieses Verzeichnis wird beibehalten, solange die APP weiterhin ausgeführt wird. Da Apple TV jedoch Platz für andere apps oder die Systemnutzung freigeben muss, kann dieser Cache gelöscht werden.

Folglich kann Ihre APP nicht darauf zurückgreifen, dass zuvor heruntergeladene Inhalte verfügbar sind, wenn Sie das nächste Mal gestartet werden. Ihre xamarin. tvos-app sollte immer überprüfen, ob die erforderlichen Ressourcen vorhanden sind, und Sie nach Bedarf herunterladen.

> [!IMPORTANT]
> Obwohl Sie die Möglichkeit haben, andere Ressourcen und Ressourcen nach Bedarf herunterzuladen, warnt Apple davor, den gesamten Speicherplatz im Cache Ihrer APP zu nutzen, da dies zu unvorhersehbaren Ergebnissen führen kann.

<a name="Managing-Resources"></a>

## <a name="managing-resources"></a>Verwalten von Ressourcen

Wie bereits erwähnt, ist aufgrund der begrenzten, nicht permanenten Speicherung von Informationen, die für tvos-Apps zur Verfügung stehen, eine sorgfältige Planung erforderlich, um eine gute Benutzer Darstellung für Ihre xamarin. tvos-APP zu erstellen.

<a name="iCloud-Data-Storage"></a>

### <a name="icloud-data-storage"></a>icloud-Datenspeicher

Da der Speicherplatz auf dem Apple TV eingeschränkt ist, nicht nur sehr begrenzter, lokaler Speicher vorhanden ist, hat Ihre APP keine Garantie, dass alle zuvor heruntergeladenen Informationen verfügbar sind, wenn Sie das nächste Mal ausgeführt werden.

Folglich muss Ihre xamarin. tvos-App alle Benutzerdaten in einem icloud-Datenspeicher speichern. Apple bietet zwei auf icloud basierende Daten Speicherungs Optionen für Ihre tvos-apps:

- **icloud-Schlüssel-Wert-Speicher (KVS)** : bei kleinen Informationen (weniger als 1 MB), die Ihre APP möglicherweise benötigt (wie z. b. Benutzereinstellungen), können Sie die icloud-KVS-Speicherung verwenden. icloud-KVS-Daten werden automatisch mit der Cloud und allen Geräten des Benutzers synchronisiert, auf denen dieselbe app ausgeführt wird. Weitere Informationen finden Sie im Abschnitt " [Schlüssel-Wert-Speicherung](~/ios/data-cloud/introduction-to-icloud.md) " unserer [Einführung in das icloud](~/ios/data-cloud/introduction-to-icloud.md) -Dokument oder in der Dokumentation zu Apple- [Design für Schlüssel-Wert-Daten in](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/iCloudDesignGuide/Chapters/DesigningForKey-ValueDataIniCloud.html#//apple_ref/doc/uid/TP40012094-CH7) der Dokumentation zu icloud.
- **Cloudkit** : um größere Informationen (mehr als 1 MB) zu speichern, verwenden Sie das cloudkit-Framework von Apple. Im Gegensatz zum icloud-KVS-Speicher können cloudkit-Daten für alle Benutzer der APP freigegeben werden (und als privat für einen einzelnen Benutzer). Weitere Informationen finden Sie [in der Einführung in die cloudkit](~/ios/data-cloud/intro-to-cloudkit.md) -Dokumentation oder in der [cloudkit-Schnellstart](https://developer.apple.com/library/prerelease/tvos/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987)von Apple.

> [!IMPORTANT]
> Apple [stellt Tools zur Verfügung](https://developer.apple.com/support/allowing-users-to-manage-data/), die Entwickler dabei unterstützen, die Datenschutz-Grundverordnung (DSGVO) der Europäischen Union umzusetzen.

<a name="On-Demand-Resources"></a>

### <a name="on-demand-resources"></a>Bedarfs gesteuerte Ressourcen

On-Demand-Ressourcen bieten App-Inhalte und-Assets (getrennt von der APP Bundle), die im App Store gehostet und nach Bedarf von ihrer App heruntergeladen werden. Bis zu zusätzlichen 2 GB an Daten können mithilfe von bedarfsgesteuerten Ressourcen bereitgestellt werden. Sie ermöglichen es, dass der anfängliche App-Download kleiner ist (tvos-apps sind auf maximal 200 MB beschränkt), während gleichzeitig umfassende Ressourcen bereitgestellt werden.

Wenn eine tvos-App Bedarfs gesteuerte Ressourcen anfordert, verwaltet das System automatisch das herunterladen und Speichern dieses Inhalts im Cache Verzeichnis der app. Ihre APP muss warten, bis der Inhalt heruntergeladen und verfügbar gemacht wird, bevor Sie fortgesetzt werden kann.

Diese Ressourcen können weiterhin auf dem Apple TV während mehrerer Starts Ihrer APP zwischengespeichert werden, wodurch der Startvorgang beschleunigt wird. Ihre APP kann sich jedoch nicht darauf verlassen, dass zuvor heruntergeladene Inhalte verfügbar sind, wenn Sie das nächste Mal gestartet werden. Weitere Informationen finden Sie weiter oben im Abschnitt [nicht persistente Downloads](#Non-Persistent-Downloads) .

Sie verwenden Xcode, um Bündel verwandter Inhalte (z. b. alle Assets für Game Level 2) zu erstellen, die mit einem Give-ressourcentag verknüpft sind. Später fordert Ihre APP die Bedarfs gesteuerte Ressource an, indem Sie dieses ressourcentag angibt. Ihre APP sollte dem Benutzer eine Benutzeroberfläche präsentieren, die besagt, dass Inhalt heruntergeladen wird. Weitere Informationen finden Sie [im Handbuch zu on-Demand-Ressourcen von](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083)Apple.

> [!IMPORTANT]
> Achten Sie darauf, das richtige Gleichgewicht zwischen der Häufigkeit, mit der die APP Bedarfs gesteuerte Ressourcen herunterladen muss, und der Größe der einzelnen Downloads zu treffen. Der Benutzer kann mit Ihrer APP frustriert werden, wenn das Spiel ständig unterbrochen wird, um neue Inhalte herunterzuladen, oder wenn ein einzelner Download zu viel Zeit in Anspruch nimmt.

<a name="Summary"></a>

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Größen-, Ressourcen-und Datenspeicher Einschränkungen beschrieben, die für eine xamarin. tvos-app durch das tvos-System festgelegt wurden. Es stehen Optionen zur Verfügung, um diese Einschränkungen zu umgehen und Vorschläge für eine gute Benutzerumgebung für Ihre APP zu erstellen.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Leitfaden zur APP-Programmierung für tvos](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
