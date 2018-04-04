---
title: Ressourcen und die Speicherung von Daten
description: In diesem Artikel wird das Arbeiten mit Ressourcen und die persistente Speicherung von Daten in einer app Xamarin.tvOS behandelt.
ms.prod: xamarin
ms.assetid: C56B5046-D2C0-4B63-9CE0-ADAA0EFD368A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: b4d96ef50498b454da583a955169b9d51c29dd01
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="resources-and-data-storage"></a>Ressourcen und die Speicherung von Daten

_In diesem Artikel wird das Arbeiten mit Ressourcen und die persistente Speicherung von Daten in einer app Xamarin.tvOS behandelt._

<a name="tvOS-Resource-Limitations" />

## <a name="tvos-resource-limitations"></a>tvos. außerdem wurden Ressourceneinschränkungen

Im Gegensatz zu iOS-Geräten bietet der neue Apple TV sehr begrenzten persistenten und lokalen Speicher für tvos. außerdem wurden apps oder Daten an. Für sehr kleine Elemente (z. B. benutzereinstellungen) Ihrer app tvos. außerdem wurden hat weiterhin Zugriff auf `NSUserDefaults` mit einem [maximal 500 KB Daten](https://forums.developer.apple.com/message/50696#50696). Jedoch wenn Ihre app Xamarin.tvOS größere Mengen an Informationen beibehalten muss, es muss speichern und abrufen, dass Daten von [iCloud](#iCloud-Data-Storage).

Darüber hinaus schränkt tvos. außerdem wurden die Größe einer Apple TV-App auf 200MB. Wenn Ihre app auf Ressourcen außerhalb dieser Größe benötigt, müssen sie als verpackt und mit geladen [On-Demand-Ressourcen](#On-Demand-Resources) (bis zu einer zusätzlichen 2 GB). Wenn diese Einschränkungen, ist es wichtig, dass Sie ordnungsgemäß viel Zeit das Herunterladen von zusätzlichen Ressourcen am besten für Benutzer der app angeben. Weitere Informationen finden Sie in der Apple- [On-Demand-Handbuch](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083).

<a name="Non-Persistent-Downloads" />

## <a name="non-persistent-downloads"></a>Nicht persistenter Downloads

Jede app tvos. außerdem wurden, wird ein temporärer Cacheverzeichnis bereitgestellt, die auf zusätzliche Ressourcen und Ressourcen heruntergeladen werden. Dieses Verzeichnis wird beibehalten werden, solange die Anwendung weiterhin ausgeführt wird. Allerdings ab, die Apple TV benötigt, um Platz für andere apps oder systemnutzung freizugeben, kann diesen Cache gelöscht werden.

Daher kann nicht Ihre app auf zuvor heruntergeladene Inhalt nicht verfügbar sind, das er gestartet, wird das nächste Mal basieren. Ihre app Xamarin.tvOS sollte immer das Vorhandensein der erforderlichen Ressourcen überprüfen und diese nach Bedarf heruntergeladen.

> [!IMPORTANT]
> Während Sie die Möglichkeit, andere Ressourcen und Ressourcen nach Bedarf heruntergeladen haben, warnt Apple für den gesamten Platz im Cache der Ihrer app verwenden, da es zu unvorhersehbaren Ergebnissen führen kann.




<a name="Managing-Resources" />

## <a name="managing-resources"></a>Verwalten von Ressourcen

Wie oben aufgrund des begrenzten nicht persistente Speicherung von Informationen für tvos. außerdem wurden-apps verfügbar angegeben, erfordern diese Einschränkungen sorgfältiger Planung eine hervorragende Benutzeroberfläche für Ihre app Xamarin.tvOS bereitgestellt.

<a name="iCloud-Data-Storage" />

### <a name="icloud-data-storage"></a>iCloud Datenspeicher

Da Speicher auf den Apple TV beschränkt ist, nicht nur ist es nur sehr eingeschränkt persistent, lokalen Speicher Ihrer app hat keine Garantie, die alle Informationen, die sie zuvor heruntergeladen haben das nächste Mal zur Verfügung stehen diese ausgeführt wird.

Daher muss Ihre app Xamarin.tvOS Benutzerdaten in eine iCloud Datenspeicher speichern. Apple bietet zwei iCloud kennwortbasierten Datenschutzdienst Speicheroptionen für Ihre apps tvos. außerdem wurden:

- **iCloud Schlüssel-Wert-Speicher (KVS)** – Geben Sie für kleine Mengen an Informationen (weniger als 1 MB), dass Ihre app möglicherweise können Sie iCloud KVS Speicher (z. B. benutzereinstellungen) erforderlich. iCloud KVS Daten werden automatisch mit der Cloud und aller Geräte des Benutzers, die die gleiche app ausgeführt synchronisiert werden. Weitere Informationen finden Sie unter der [Schlüssel-Wert-Speicher](~/ios/data-cloud/introduction-to-icloud.md) Teil unserer [Einführung in die iCloud](~/ios/data-cloud/introduction-to-icloud.md) Dokument oder APNs von Apple [Entwerfen für Schlüssel-Wert-Daten in iCloud](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/iCloudDesignGuide/Chapters/DesigningForKey-ValueDataIniCloud.html#//apple_ref/doc/uid/TP40012094-CH7) (Dokumentation).
- **CloudKit** – für die Speicherung von größeren Angaben (größer als 1 MB), Apple CloudKit-Framework verwenden. Im Gegensatz zu iCloud KVS Speicher können CloudKit Daten für alle Benutzer Ihrer app (ebenso wie für einen einzelnen Benutzer privat wird) freigegeben werden. Weitere Informationen zu bilden, finden Sie unter unsere [Einführung in CloudKit](~/ios/data-cloud/intro-to-cloudkit.md) -Dokumentation oder APNs von Apple [CloudKit Schnellstart](https://developer.apple.com/library/prerelease/tvos/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987).

<a name="On-Demand-Resources" />

### <a name="on-demand-resources"></a>On-Demand-Ressourcen

On-Demand-Ressourcen bereitzustellen, app-Inhalte und Ressourcen (getrennt von der app-Bündel), die im App Store gehostet und ggf. von Ihrer app heruntergeladen. Bis zu 2 GB zusätzlichem Daten können bedient werden mithilfe von On-Demand-Ressourcen. Sie ermöglichen es der ersten app herunterladen, die kleiner sind (tvos. außerdem wurden apps sind beschränkt auf maximal 200MB), dabei aber nicht auf umfangreiche Ressourcen nach Bedarf.

Wenn eine app tvos. außerdem wurden die On-Demand-Ressourcen anfordert, wird das System automatisch das Herunterladen und die Speicher dieses Inhalts auf die app Cacheverzeichnis verwalten. Ihre app muss warten, für diesen Inhalt heruntergeladen und verfügbar gemacht, damit sie fortgesetzt werden kann.

Diese Ressourcen werden möglicherweise weiterhin auf dem Apple TV während des gesamten mehrere startet der app, daher beschleunigen starten Zyklus zwischengespeichert werden soll. Allerdings kann nicht Ihre app auf zuvor heruntergeladene Inhalt nicht verfügbar sind, das er gestartet, wird das nächste Mal basieren. Finden Sie unter der [nicht persistenter Downloads](#Non-Persistent-Downloads) im Abschnitt oben für weitere Details.

Verwenden Sie Xcode, um Pakete mit verwandtem Inhalt (z. B. alle Ressourcen für Spiel Stufe 2) einen angegebenen Ressourcentag zugeordnet zu erstellen. Später wird Ihre app On-Demand-Ressource durch Angeben dieses Ressourcentag anfordern. Ihre app sollte eine Benutzeroberfläche vorweisen, die dem Benutzer angezeigt, die besagt, dass der Inhalt heruntergeladen wird. Weitere Informationen finden Sie in der Apple- [On-Demand-Handbuch](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083).

> [!IMPORTANT]
> Vorsichtig sollte vorgenommen werden, das richtige Gleichgewicht zwischen oft die app erhält Ressourcen bei Bedarf heruntergeladen und die Größe der einzelnen Downloads zu erreichen. Benutzer kann mit Ihrer app frustriert werden, wenn Spielverlauf ständig unterbrochen wird, um neue Inhalte herunterladen oder ein einzigen Download zu lange dauert.




<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat die Größe, Ressourcen- und Daten Speicher Einschränkungen auf eine app Xamarin.tvOS platziert werden, vom System tvos. außerdem wurden behandelt. Sie verfügt über Optionen zum Umgehen dieser Einschränkungen und Vorschläge enthalten, die eine hervorragende Benutzeroberfläche für Ihre app bereitgestellt angezeigt.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos. außerdem wurden Handbücher für interaktive Workflowdienste-Schnittstelle](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos. außerdem wurden](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
