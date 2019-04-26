---
title: Azure Active Directory
description: Dieses Dokument beschreibt die Schritte, die beachtet werden müssen, um eine mobile app für die Authentifizierung mit Azure Active Directory zu ermöglichen.
ms.prod: xamarin
ms.assetid: 70B3C2AB-CB4D-420C-9CFA-20CCFA0E3C78
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: ca33422817f19dbb0a04e8870800d3f5efa8af2a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61318297"
---
# <a name="azure-active-directory"></a>Azure Active Directory

_Registrieren einer app zum Azure Active Directory verwenden_

Azure Active Directory können Entwickler sichere Ressourcen wie Dateien, Verknüpfungen und Web-APIs, die mit dem gleichen organisationskonto an, die Mitarbeiter verwenden, melden Sie sich mit ihren Systemen oder Überprüfen Sie ihre e-Mails.

Entwickeln von mobilen Anwendungen, die in Azure Active Directory authentifiziert werden können, umfasst drei Schritte.
Die ersten beiden Schritte sind in der Regel identisch, egal, welche Dienste Sie verwenden möchten. Der dritte Schritt ist für jeden Diensttyp unterschiedlich:

  1. [Registrieren bei Azure Active Directory](~/cross-platform/data-cloud/active-directory/get-started/register.md) auf die *windowsazure.com* Portal, klicken Sie dann
  2. [Konfigurieren von Diensten](~/cross-platform/data-cloud/active-directory/get-started/configure.md).
  3. Entwickeln Sie mobile apps mit den Diensten.

Beispiele für verschiedene Dienste, die Sie zugreifen können, sind:

- [Graph-API](~/cross-platform/data-cloud/active-directory/graph.md)
- Web-API
- Office365


## <a name="conclusion"></a>Schlussbemerkung

Mit den Schritten oben, Sie können Ihre mobilen apps mit Azure Active Directory authentifizieren. Der Active Directory-Authentifizierungsbibliothek (ADAL) wesentlich vereinfacht mit weniger Codezeilen erforderlich, wobei der größte Teil des Codes identisch und daher über Plattformen hinweg gemeinsam nutzbar machen.



## <a name="related-links"></a>Verwandte Links

- [Microsoft NativeClient-Beispiel](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
