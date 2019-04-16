---
ms.openlocfilehash: 5bcf548c0ff907df0913e77a1def2fe27f3eecfd
ms.sourcegitcommit: e7f27ba75cae5099ef053b819b84132a77d4f9e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2019
ms.locfileid: "52294991"
---

Beim Erstellen von iOS-Apps in Visual Studio und im Mac Build-Agent wird das APP-Bündel nicht zurück auf den Windows-Computer kopiert. Xamarin Tools für Visual Studio 7.4 führt die neue Eigenschaft `CopyAppBundle` ein, mit der CI-Builds APP-Bündel zurück auf Windows kopieren können.

Um diese Funktion nutzen zu können, fügen Sie die `CopyAppBundle`-Eigenschaft der CSPROJ-Datei unter der Eigenschaftengruppe hinzu, auf die diese Funktion angewendet werden soll. In folgendem Beispiel wird veranschaulicht, wie Sie ein APP-Bündel für einen **Debugbuild**, der **iPhoneSimulator** als Ziel verwendet, zurück auf einen Windows-Computer kopieren:

    <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">
        <CopyAppBundle>true</CopyAppBundle>
    </PropertyGroup>

