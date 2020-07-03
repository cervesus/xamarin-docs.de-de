---
title: Binden von IOS SWIFT-Bibliotheken
description: In diesem Dokument wird beschrieben, wie Sie c#-Bindungen für SWIFT-Code erstellen, sodass Sie Native Bibliotheken und cocoapods in einer xamarin. IOS-Anwendung nutzen können.
ms.prod: xamarin
ms.assetid: 890EFCCA-A2A2-4561-88EA-30DE3041F61D
ms.technology: xamarin-ios
author: alexeystrakh
ms.author: alstrakh
ms.date: 02/11/2020
ms.openlocfilehash: 72ab1d9f10ee308313569528d152d5930a258207
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85852964"
---
# <a name="bind-ios-swift-libraries"></a>Binden von IOS SWIFT-Bibliotheken

> [!IMPORTANT]
> Wir untersuchen derzeit die benutzerdefinierte Bindungs Verwendung auf der xamarin-Plattform. Nehmen Sie an [**dieser Umfrage**](https://www.surveymonkey.com/r/KKBHNLT) Teil, um zukünftige Entwicklungsbemühungen zu informieren.

Die IOS-Plattform wird zusammen mit den nativen Sprachen und Tools ständig weiterentwickelt, und es gibt zahlreiche Bibliotheken von Drittanbietern, die mit den neuesten angeboten entwickelt wurden. Das maximieren von Code und der Wiederverwendung von Komponenten ist eines der wichtigsten Ziele der plattformübergreifenden Entwicklung. Die Möglichkeit zur Wiederverwendung von mit SWIFT erstellten Komponenten ist für xamarin-Entwickler immer wichtiger geworden, da ihre Beliebtheit bei Entwicklern weiterhin zunimmt. Möglicherweise sind Sie bereits mit dem Binden regulärer [Ziel-C-](https://docs.microsoft.com/xamarin/ios/platform/binding-objective-c/walkthrough) Bibliotheken vertraut. Es ist jetzt eine zusätzliche Dokumentation verfügbar, die den Prozess der [Bindung eines SWIFT-Frameworks](walkthrough.md)beschreibt, sodass Sie von einer xamarin-Anwendung auf die gleiche Weise genutzt werden können. Dieses Dokument soll einen allgemeinen Ansatz zum Erstellen einer SWIFT-Bindung für xamarin beschreiben.

## <a name="high-level-approach"></a>Allgemeiner Ansatz

Mit xamarin können Sie eine native Bibliothek von Drittanbietern binden, damit Sie von einer xamarin-Anwendung genutzt werden kann. SWIFT ist die neue Sprache und das Erstellen einer Bindung für Bibliotheken, die mit dieser Sprache erstellt wurden, erfordert einige zusätzliche Schritte und Tools. Diese Vorgehensweise umfasst die folgenden vier Schritte:

1. Aufbauen der nativen Bibliothek
1. Vorbereiten der xamarin-Metadaten, die xamarin-Tools das Generieren von c#-Klassen ermöglichen
1. Aufbauen einer xamarin-Bindungs Bibliothek mithilfe der nativen Bibliothek und der Metadaten
1. Verwenden der xamarin-Bindungs Bibliothek in einer xamarin-Anwendung

In den folgenden Abschnitten werden diese Schritte mit zusätzlichen Details erläutert.

### <a name="build-the-native-library"></a>Erstellen der nativen Bibliothek

Der erste Schritt besteht darin, ein System eigenes SWIFT-Framework bereit zu lassen, das den Ziel-C-Header erstellt. Diese Datei ist ein automatisch generierter Header, der gewünschte SWIFT-Klassen,-Methoden und-Felder verfügbar macht, sodass diese sowohl für das Ziel-C als auch für c# über eine xamarin-Bindungs Bibliothek verfügbar sind. Diese Datei befindet sich innerhalb des Frameworks unter folgendem Pfad: ** \<FrameworkName> . Framework/Headers/ \<FrameworkName> -Swift. h**. Wenn die verfügbar gemachte Schnittstelle über alle erforderlichen Mitglieder verfügt, können Sie mit dem nächsten Schritt fortfahren. Andernfalls sind weitere Schritte erforderlich, um diese Mitglieder verfügbar zu machen. Die Vorgehensweise hängt davon ab, ob Sie auf den SWIFT Framework-Quellcode zugreifen können:

- Wenn Sie Zugriff auf den Code haben, können Sie die erforderlichen SWIFT-Member mit dem `@objc` -Attribut ergänzen und einige zusätzliche Regeln anwenden, um den Xcode-Buildtools mitzuteilen, dass diese Member für die Ziel-C-Welt und den-Header verfügbar gemacht werden sollen.
- Wenn Sie keinen Zugriff auf den Quell Code haben, müssen Sie ein Proxy-SWIFT-Framework erstellen, das das ursprüngliche SWIFT-Framework umschließt und die öffentliche Schnittstelle definiert, die Ihre Anwendung mit dem- `@objc` Attribut benötigt.

### <a name="prepare-the-xamarin-metadata"></a>Vorbereiten der xamarin-Metadaten

Der zweite Schritt besteht im Vorbereiten von API-Definitions Schnittstellen, die von einem Bindungs Projekt verwendet werden, um c#-Klassen zu generieren. Diese Definitionen können manuell oder automatisch durch das Ziel- [Sharpie](https://docs.microsoft.com/xamarin/cross-platform/macios/binding/objective-sharpie/) -Tool und die oben genannte automatisch generierten ** \<FrameworkName> -Swift. h-** Header Datei erstellt werden. Nachdem die Metadaten generiert wurden, sollten Sie überprüft und manuell überprüft werden.

### <a name="build-the-xamarinios-binding-library"></a>Erstellen der xamarin. IOS-Bindungs Bibliothek

Der dritte Schritt besteht darin, eine spezielle Projekt-xamarin. IOS-Bindungs Bibliothek zu erstellen. Er verweist auf die Frameworks und die Metadaten, die im vorherigen Schritt zusammen mit allen zusätzlichen Abhängigkeiten vorbereitet werden, von denen das jeweilige Framework abhängig ist. Außerdem wird das Verknüpfen der nativen Frameworks, auf die verwiesen wird, mit der verwendeten xamarin. IOS-Anwendung behandelt.

### <a name="consume-the-xamarin-binding-library"></a>Verwenden der xamarin-Bindungs Bibliothek

Der vierte und letzte Schritt besteht darin, in einer xamarin. IOS-Anwendung auf die Bindungs Bibliothek zu verweisen. Es ist ausreichend, die Verwendung der nativen Bibliothek innerhalb von xamarin. IOS-Anwendungen zu aktivieren, die auf IOS 12,2 und höher abzielen. Für Anwendungen, die eine niedrigere Version als Ziel haben, sind einige zusätzliche Schritte erforderlich:

- Fügen Sie SWIFT dylib-Abhängigkeiten für die Laufzeitunterstützung hinzu. Ab IOS 12,2 und SWIFT 5,1 wurde die Sprache ABI (Anwendungs Binärschnittstelle) stabil und kompatibel. Aus diesem Grund muss jede Anwendung, die auf eine niedrigere IOS-Version abzielt, SWIFT-dylisb-Abhängigkeiten enthalten, die vom Framework verwendet werden. Verwenden Sie das [nuget-Paket swiftruntimesupport](https://www.nuget.org/packages/Xamarin.iOS.SwiftRuntimeSupport/) , um erforderliche dylib-Abhängigkeiten automatisch in das resultierende Anwendungspaket einzuschließen.
- Fügen Sie den Ordner **swiftsupport** mit signierten dylisb hinzu, der vom AppStore während des Uploadvorgangs überprüft wird. Das Paket muss signiert und mithilfe von Xcode-Tools an die App Store-Verbindung verteilt werden. andernfalls wird es automatisch abgelehnt.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Der obige Ansatz beschreibt die allgemeine Schritte, die erforderlich sind, um eine SWIFT-Bindung für xamarin zu erstellen. Es gibt zahlreiche Schritte auf niedrigerer Ebene und weitere Details, die beim Vorbereiten dieser Bindungen in der Praxis berücksichtigt werden müssen, einschließlich der Anpassung an Änderungen in den systemeigenen Tools und Sprachen. Ziel ist es, Sie dabei zu unterstützen, ein besseres Verständnis dieses Konzepts und der grundlegenden Schritte zu erhalten, die an diesem Vorgang beteiligt sind. Eine ausführliche Schritt-für-Schritt-Anleitung finden Sie in der exemplarischen Vorgehensweise zur [xamarin SWIFT-Bindung](walkthrough.md) .

## <a name="related-links"></a>Verwandte Links

- [Xcode](https://apps.apple.com/us/app/xcode/id497799835)
- [Visual Studio für Mac](https://visualstudio.microsoft.com/downloads)
- [Objektive Sharpie](https://docs.microsoft.com/xamarin/cross-platform/macios/binding/objective-sharpie/)
- [Sharpie-Metadatenüberprüfung](https://docs.microsoft.com/xamarin/cross-platform/macios/binding/objective-sharpie/platform/verify)
- [Bindungs Ziel-C-Framework](https://docs.microsoft.com/xamarin/ios/platform/binding-objective-c/walkthrough)
- [Gigya IOS SDK (SWIFT-Framework)](https://developers.gigya.com/display/GD/Swift+SDK)
- [SWIFT 5,1 ABI-Stabilität](https://swift.org/blog/swift-5-1-released/)
- [SWiF truntimesupport-nuget](https://www.nuget.org/packages/Xamarin.iOS.SwiftRuntimeSupport/)
- [Xamarin UITest-Automatisierung](https://docs.microsoft.com/appcenter/test-cloud/uitest/)
- [Xamarin. IOS UITest-Konfiguration](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest)
- [AppCenter-Test Cloud](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest)
- [Beispielprojektrepository](https://github.com/xamcat/xamarin-binding-swift-framework)
