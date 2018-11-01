---
title: Internationalisierungscodierungen in Xamarin.iOS
description: Dieses Dokument beschreibt internationalisierungscodierungen in Xamarin.iOS, erläutern die verfügbaren Codierungen und wie sie eine app hinzugefügt.
ms.prod: xamarin
ms.assetid: F5117294-28BB-4583-B6A0-A339B050FDE1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/28/2017
ms.openlocfilehash: db24c8677b0a3099193132575e92bc43a4c31dea
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675561"
---
# <a name="internationalization-encodings-in-xamarinios"></a>Internationalisierungscodierungen in Xamarin.iOS

Nicht alle Codierungen sind standardmäßig in der Xamarin.iOS-Klassenbibliothek enthalten.

Um die Größe der Anwendung zu reduzieren, Xamarin.iOS nicht enthalten speziellen Codierung, und Sie anweisen Mtouch sollen die Assemblys, die die Unterstützung für die Codierung, die Sie benötigen.

Dies erfolgt durch Auswählen von zusätzlichen Codierungen aus der iOS-Build/erweiterte-Bereich in der Visual Studio für Mac oder Visual Studio:

 [![](encodings-images/00.png "Wählen die zusätzlichen Codierungen")](encodings-images/00.png#lightbox)

 [![](encodings-images/00a.png "Wählen die zusätzlichen Codierungen")](encodings-images/00a.png#lightbox)

Sie können einen davon auswählen:

-  CJK: für Chineese, Japanisch und Koreanisch
-  Naher Osten: Arabisch, Hebräisch, Türkisch und Latin5.
-  andere: Kyrillisch, Baltisch, Vietnamesisch, Ukrainisch und Thai
-  selten: EBCDIC-Codierungen und anderen selten Codepages
-  West: lateinischen Sprachen, Easter und Westeuropäisch
-  alle


 <a name="cjk" />


## <a name="cjk"></a>CJK

-  CP51932
-  CP932
-  CP936
-  CP949
-  CP950
-  CP54936


 <a name="mideast" />


## <a name="mideast"></a>Naher Osten

-  CP1254
-  CP1255
-  CP1256
-  CP28596
-  CP28598
-  CP28599
-  CP38598


 <a name="other" />


## <a name="other"></a>andere

-  CP1251
-  CP1257
-  CP1258
-  CP20866
-  CP21866
-  CP28594
-  CP28595
-  CP57002
-  CP874


 <a name="rare" />


## <a name="rare"></a>selten

-  CP1026
-  CP1047
-  CP1140
-  CP1141
-  CP1142
-  CP1143
-  CP1144
-  CP1145
-  CP1146
-  CP1147
-  CP1148
-  CP1149
-  CP20273
-  CP20277
-  CP20278
-  CP20280
-  CP20284
-  CP20285
-  CP20290
-  CP20297
-  CP20420
-  CP20424
-  CP20871
-  CP21025
-  CP37
-  CP500
-  CP708
-  CP852
-  CP855
-  CP857
-  CP858
-  CP862
-  CP864
-  CP866
-  CP869
-  CP870
-  CP875


 <a name="west" />


## <a name="west"></a>West

-  CP10000
-  CP10079
-  CP1250
-  CP1252
-  CP1253
-  CP28592
-  CP28593
-  CP28597
-  CP28605
-  CP437
-  CP850
-  CP860
-  CP861
-  CP863
-  CP865

