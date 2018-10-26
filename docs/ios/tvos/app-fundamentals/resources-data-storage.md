---
title: TvOS-Ressourcen und die Datenspeicherung in Xamarin
description: Dieser Artikel beschreibt das Arbeiten mit Ressourcen und die persistente Speicherung von Daten in einer TvOS-app mit Xamarin erstellt wurde. Es wird erläutert, iCloud, Data, Speicher und on-Demand-Ressourcen.
ms.prod: xamarin
ms.assetid: C56B5046-D2C0-4B63-9CE0-ADAA0EFD368A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 8f8ecc115738fb97f71b4ea6b2cdcc5c2714372d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108183"
---
# <a name="tvos-resources-and-data-storage-in-xamarin"></a>TvOS-Ressourcen und die Datenspeicherung in Xamarin

_Dieser Artikel behandelt die Arbeit mit Ressourcen und die persistente Speicherung von Daten in einer Xamarin.tvOS-app._

<a name="tvOS-Resource-Limitations" />

## <a name="tvos-resource-limitations"></a>TvOS Ressourceneinschränkungen

Im Gegensatz zu iOS-Geräte bietet neue Apple TV sehr begrenzten persistenten und lokalen Speicher für TvOS-apps oder Daten an. Für sehr kleine Elemente (wie benutzereinstellungen), TvOS-app immer noch Zugriff auf `NSUserDefaults` mit einem [maximal 500 KB Daten](https://forums.developer.apple.com/message/50696#50696). Aber wenn Ihrer app Xamarin.tvOS größere Mengen von Informationen beibehalten werden muss, es muss speichern und abrufen, der Daten aus [iCloud](#iCloud-Data-Storage).

TvOS schränkt darüber hinaus die Größe einer Apple TV-App auf 200MB. Wenn Ihre app mit Ressourcen außerhalb dieser Größe erforderlich ist, müssen sie werden verpackt und mit geladen [On-Demand-Ressourcen](#On-Demand-Resources) (bis zu zusätzlich 2 GB). Wenn diese Einschränkungen, ist es wichtig, dass Sie ordnungsgemäß Zeit das Herunterladen von zusätzlichen Ressourcen am besten für die Benutzer Ihrer app bereitstellen. Weitere Informationen finden Sie unter Apple [On-Demand-Ressourcen-Handbuch](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083).

<a name="Non-Persistent-Downloads" />

## <a name="non-persistent-downloads"></a>Nicht persistenter Downloads

Jede TvOS-app wird ein temporäres Cacheverzeichnis bereitgestellt, dem auf die zusätzlichen Ressourcen und Ressourcen heruntergeladen werden. Dieses Verzeichnis werden beibehalten, solange die app immer noch ausgeführt wird. Aber wie Apple TV benötigt, um Platz für anderen apps oder die systemnutzung freizugeben, kann diesen Cache gelöscht werden.

Daher kann nicht Ihre app auf zuvor heruntergeladene Inhalt nicht verfügbar sind, beim nächsten Starten basieren. Ihre Xamarin.tvOS-app sollte immer das Vorhandensein des erforderlichen Ressourcen überprüfen und diese nach Bedarf herunterladen.

> [!IMPORTANT]
> Während Sie die Möglichkeit, andere Objekte und die Ressourcen nach Bedarf heruntergeladen haben, warnt Apple für den gesamten Platz im Cache, der Ihrer app nutzen, da dies zu unvorhersehbaren Ergebnissen führen kann.




<a name="Managing-Resources" />

## <a name="managing-resources"></a>Verwalten von Ressourcen

Wie oben aufgrund des begrenzten, nicht permanente Speicher für TvOS-apps verfügbaren Informationen angegeben, erfordern diese Einschränkungen eine sorgfältige Planung, um ein optimales Benutzererlebnis für Ihre Xamarin.tvOS-app zu erstellen.

<a name="iCloud-Data-Storage" />

### <a name="icloud-data-storage"></a>iCloud-Datenspeicher

Da Speicher auf Apple TV beschränkt ist, ist Sie nicht nur es sehr eingeschränkte persistenten und lokalen Speicher, Ihre app verfügt über keine Garantie dafür, die alle Informationen, die sie zuvor heruntergeladen haben. das nächste Mal zur Verfügung stehen diese ausgeführt wird.

Daher muss Ihre app Xamarin.tvOS Benutzerdaten in einer iCloud Data Store speichern. Apple bietet zwei iCloud-basierte Datenspeicheroptionen für Ihre TvOS-apps:

- **iCloud-Schlüssel-Wert-Speicher (KVS)** : für einzelne Informationen (weniger als 1 MB), dass Ihre app möglicherweise (wie benutzereinstellungen), können Sie iCloud KVS Speicher. iCloud KVS Daten werden in die Cloud und alle Geräte des Benutzers, die die gleiche app ausgeführt wird automatisch synchronisiert. Weitere Informationen finden Sie unter den [Schlüssel-Wert-Speicher](~/ios/data-cloud/introduction-to-icloud.md) Teil unserer [Einführung in iCloud](~/ios/data-cloud/introduction-to-icloud.md) Dokument oder APNs von Apple [Entwerfen für Schlüssel-Wert-Daten in iCloud](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/iCloudDesignGuide/Chapters/DesigningForKey-ValueDataIniCloud.html#//apple_ref/doc/uid/TP40012094-CH7) Dokumentation.
- **CloudKit** : für die Speicherung von größere Teile der Informationen (größer als 1 MB) von Apple-CloudKit-Framework verwenden. Im Gegensatz zu iCloud KVS Speicher können die CloudKit Daten von allen Benutzern Ihrer app (als auch für ein einzelner Benutzer privat wird) freigegeben werden. Weitere Informationen zu bilden, informieren Sie sich unsere [Einführung in CloudKit](~/ios/data-cloud/intro-to-cloudkit.md) Dokumentation oder APNs von Apple [CloudKit Schnellstart](https://developer.apple.com/library/prerelease/tvos/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987).

> [!IMPORTANT]
> Apple [stellt Tools zur Verfügung](https://developer.apple.com/support/allowing-users-to-manage-data/), die Entwickler dabei unterstützen, die Datenschutz-Grundverordnung (DSGVO) der Europäischen Union umzusetzen.

<a name="On-Demand-Resources" />

### <a name="on-demand-resources"></a>On-Demand-Ressourcen

On-Demand-Ressourcen bereitstellen, app-Inhalte und Ressourcen (getrennt von der app-Bündel), die auf den App Store gehostet und als Ihre app erforderliche heruntergeladen werden. Bis zu zusätzlich 2 GB Daten kann bereitgestellt werden mithilfe von On-Demand-Ressourcen. Sie ermöglichen es der ersten app herunterladen, die kleiner sind (TvOS-apps sind beschränkt auf maximal 200MB), während gleichzeitig die umfangreichen Ressourcen nach Bedarf.

Wenn eine TvOS-app On-Demand-Ressourcen anfordert, wird das System automatisch das Herunterladen und Speichern von Inhalt der app-Cacheverzeichnis verwalten. Ihre app muss warten, für diesen Inhalt heruntergeladen und verfügbar gemacht, damit er fortfahren kann.

Diese Ressourcen möglicherweise weiterhin auf Apple TV in mehrere Start Ihrer App, daher beschleunigen starten Zyklus zwischengespeichert werden. Allerdings kann nicht Ihre app basieren, für alle zuvor heruntergeladenen Inhalte nicht verfügbar sind, das nächste Mal starten. Finden Sie unter den [Non-Persistent Downloads](#Non-Persistent-Downloads) Abschnitt Weitere Details.

Verwenden Sie Xcode, um Pakete mit verwandtem Inhalt (z. B. alle Ressourcen für Spiele Stufe 2) eine bieten Ressourcentag zugeordnet zu erstellen. Später wird Ihre app On-Demand-Ressourcen durch Angeben dieser Ressourcentag anfordern. Ihre app sollte eine Benutzeroberfläche darzustellen, für dem Benutzer, die mit dem Hinweis, dass der Inhalt heruntergeladen wird. Weitere Informationen finden Sie unter Apple [On-Demand-Ressourcen-Handbuch](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083).

> [!IMPORTANT]
> Das Abfragemodell das richtige Gleichgewicht zwischen der Anzahl an, wie oft die app verfügt über On-Demand-Ressourcen herunterladen und die Größe der einzelnen Downloads sollte vorsichtig vorgenommen werden. Benutzer sind möglicherweise für Ihre app enttäuscht, wenn Gameplay ständig unterbrochen wird, um neue Inhalte herunterladen oder ein einzigen Download zu lange dauert.




<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die speichereinschränkungen der Größe, Ressourcen und Daten auf einer Xamarin.tvOS-app platziert wird, vom System TvOS behandelt. Es verfügt über Optionen zum Umgehen dieser Einschränkungen und Vorschläge für ein optimales Benutzererlebnis für Ihre app zu erstellen, angezeigt.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [TvOS Human Interface-Handbücher](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos verwendet.](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
