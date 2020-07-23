---
title: Internationalisierungs Codierungen in xamarin. IOS
description: In diesem Dokument werden die Codierungs Codierungen in xamarin. IOS beschrieben, und es werden die verfügbaren Codierungen erläutert und erläutert, wie Sie einer app hinzugefügt werden
ms.prod: xamarin
ms.assetid: F5117294-28BB-4583-B6A0-A339B050FDE1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/28/2017
ms.openlocfilehash: 88f95f797d1230c818a635376f7b17a2264afadf
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86930337"
---
# <a name="internationalization-encodings-in-xamarinios"></a>Internationalisierungs Codierungen in xamarin. IOS

Nicht alle Codierungen sind standardmäßig in der xamarin. IOS-Klassenbibliothek enthalten.

Um die Größe der Anwendung zu reduzieren, umfasst xamarin. IOS keine bestimmte Codierung, und Sie müssen mfinger anweisen, die Assemblys einzuschließen, die die Unterstützung für die benötigte Codierung enthalten.

Dazu wählen Sie die zusätzlichen Codierungen aus dem Bereich IOS-Build/erweitert in Visual Studio für Mac oder Visual Studio aus:

 [![Auswählen der zusätzlichen Codierungen](encodings-images/00.png)](encodings-images/00.png#lightbox)

 [![Auswählen der zusätzlichen Codierungen](encodings-images/00a.png)](encodings-images/00a.png#lightbox)

Sie können eine der folgenden Aktionen auswählen:

- CJK: für Chineese, Japanisch und Koreanisch
- midäast: Arabisch, Hebräisch, Türkisch und Latin5.
- Sonstiges: Kyrillisch, Baltikum, Vietnamesisch, Ukrainisch und Thailändisch
- selten: EBCDIC-Codierungen und andere seltene Codepages
- West: lateinische Sprachen, Ostern und Westeuropäisch
- alle

 <a name="cjk"></a>

## <a name="cjk"></a>CJK

- CP51932
- CP932
- CP936
- CP949
- CP950
- CP54936

 <a name="mideast"></a>

## <a name="mideast"></a>Naher Osten

- CP1254
- CP1255
- CP1256
- CP28596
- CP28598
- CP28599
- CP38598

 <a name="other"></a>

## <a name="other"></a>Sonstige

- CP1251
- CP1257
- CP1258
- CP20866
- CP21866
- CP28594
- CP28595
- CP57002
- CP874

 <a name="rare"></a>

## <a name="rare"></a>ere

- CP1026
- CP1047
- CP1140
- CP1141
- CP1142
- CP1143
- CP1144
- CP1145
- CP1146
- CP1147
- CP1148
- CP1149
- CP20273
- CP20277
- CP20278
- CP20280
- CP20284
- CP20285
- CP20290
- CP20297
- CP20420
- CP20424
- CP20871
- CP21025
- CP37
- CP500
- CP708
- CP852
- CP855
- CP857
- CP858
- CP862
- CP864
- CP866
- CP869
- CP870
- CP875

 <a name="west"></a>

## <a name="west"></a>Westen

- CP10000
- CP10079
- CP1250
- CP1252
- CP1253
- CP28592
- CP28593
- CP28597
- CP28605
- CP437
- CP850
- CP860
- CP861
- CP863
- CP865
