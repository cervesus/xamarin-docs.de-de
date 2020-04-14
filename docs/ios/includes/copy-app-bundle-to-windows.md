---
ms.openlocfilehash: 22a8542c0e829db8b889abc2c7fe5f5ab9d19ed4
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "69527637"
---

Beim Erstellen von iOS-Apps in Visual Studio und im Mac Build-Agent wird das APP-Bündel nicht zurück auf den Windows-Computer kopiert. Xamarin Tools für Visual Studio 7.4 führt die neue Eigenschaft `CopyAppBundle` ein, mit der CI-Builds APP-Bündel zurück auf Windows kopieren können.

Um diese Funktion nutzen zu können, fügen Sie die `CopyAppBundle`-Eigenschaft der CSPROJ-Datei unter der Eigenschaftengruppe hinzu, auf die diese Funktion angewendet werden soll. In folgendem Beispiel wird veranschaulicht, wie Sie ein APP-Bündel für einen **Debugbuild**, der **iPhoneSimulator** als Ziel verwendet, zurück auf einen Windows-Computer kopieren:

```xml
<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">
    <CopyAppBundle>true</CopyAppBundle>
</PropertyGroup>
```
