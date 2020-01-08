---
title: .Net-Einbettungs Einschränkungen
description: In diesem Dokument werden die Einschränkungen der .net-Einbettung beschrieben, das Tool, mit dem .NET-Code in anderen Programmiersprachen verwendet werden kann.
ms.prod: xamarin
ms.assetid: EBBBB886-1CEF-4DF4-AFDD-CA96049F878E
author: davidortinau
ms.author: daortin
ms.date: 11/14/2017
ms.openlocfilehash: a8b63638861e8d44deb4ea72959d7461190f7713
ms.sourcegitcommit: 6266ef043ae0289f174e901f204f2a280a53c071
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/30/2019
ms.locfileid: "75545805"
---
# <a name="net-embedding-limitations"></a>.Net-Einbettungs Einschränkungen

In diesem Dokument werden die Einschränkungen der .net-Einbettung erläutert und, wann immer möglich, Problem Umgehungen für Sie bereitstellt.

## <a name="general"></a>Allgemein

### <a name="use-more-than-one-embedded-library-in-a-project"></a>Verwenden Sie mehr als eine eingebettete Bibliothek in einem Projekt.

Es ist nicht möglich, zwei Mono-Laufzeiten innerhalb derselben Anwendung gemeinsam zu haben. Dies bedeutet, dass Sie in derselben Anwendung nicht zwei verschiedene .net-Einbettungs generierte Bibliotheken verwenden können.

Problem **Umgehung:** Sie können den Generator verwenden, um eine einzelne Bibliothek zu erstellen, die mehrere Assemblys (aus verschiedenen Projekten) enthält.

### <a name="subclassing"></a>Unterklassen

.Net-Einbettung vereinfacht die Integration der Mono-Laufzeit in Anwendungen, indem eine Reihe von einsatzbereiten APIs für die Zielsprache und-Plattform verfügbar gemacht wird.

Dabei handelt es sich jedoch nicht um eine bidirektionale Integration, z. b. können Sie keinen verwalteten Typ unterteilen und erwarten, dass verwalteter Code einen Rückruf innerhalb des nativen Codes durchgeführt wird, da der verwaltete Code diese Co-Existenz nicht kennt.

Abhängig von Ihren Anforderungen kann es möglich sein, Teile dieser Einschränkung zu umgehen, z. b.

* Ihr verwalteter Code kann in ihrem nativen Code p/aufrufen. Hierfür muss der verwaltete Code angepasst werden, um Anpassungen von nativem Code zuzulassen.

* Verwenden Sie Produkte wie xamarin. IOS, und machen Sie eine verwaltete Bibliothek verfügbar, die Ziel-C (in diesem Fall) die Unterklasse einiger verwalteter NSObject-Unterklassen ermöglicht.

## <a name="objective-c-generated-code"></a>Ziel-C-generierter Code

### <a name="nullability"></a>NULL-Zulässigkeit

Es gibt keine Metadaten in .net, die uns mitteilen, ob ein NULL-Verweis akzeptabel ist oder nicht für eine API. Die meisten APIs lösen `ArgumentNullException` aus, wenn Sie nicht mit einem `null`-Argument zurechtkommen. Dies ist problematisch, da die Ziel-C-Behandlung von Ausnahmen etwas besseres vermieden wird.

Da es nicht möglich ist, genaue NULL-Zulässigkeit-Anmerkungen in den Header Dateien zu generieren und verwaltete Ausnahmen zu minimieren, werden standardmäßig nicht-NULL-Argumente (`NS_ASSUME_NONNULL_BEGIN`) hinzugefügt, und es werden einige spezifische, wenn Genauigkeit möglich, auf NULL feststellbare Anmerkungen eingefügt.

### <a name="bitcode-ios"></a>Bitcode (IOS)

Derzeit unterstützt .net-Einbettungen keinen Bitcode unter IOS, der für einige Xcode-Projektvorlagen aktiviert ist. Dies muss deaktiviert werden, damit generierte Frameworks erfolgreich verknüpft werden können.

* Für IOS ist Bitcode optional, um apps an den AppStore von Apple zu übermitteln. Xamarin. IOS unterstützt dies nicht für IOS, da der generierte Bitcode "Inlineassembly" ist. Dies bietet keinen Vorteil von der IOS-Plattform, da Sie nicht serverseitig optimiert werden kann, sondern die Binärdateien vergrößert und die Zeit für die Erstellung verlängert wird.

* Für tvos und watchos ist Bitcode erforderlich, um apps an den AppStore von Apple zu senden. Xamarin. IOS unterstützt Bitcode in tvos (als "Inlineassembly") und watchos (als "llvm/IR"), um diese Anforderung zu erfüllen.

* Für macOS ist die Bitcode Unterstützung derzeit nicht erforderlich und wird von xamarin. Mac nicht unterstützt.

![Bitcode-Option](images/ios-bitcode-option.png)
