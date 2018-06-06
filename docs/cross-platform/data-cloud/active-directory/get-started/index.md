---
title: Azure Active Directory
description: Dieses Dokument beschreibt die Schritte, die befolgt werden müssen, um eine mobile app für die Authentifizierung bei Azure Active Directory zu ermöglichen.
ms.prod: xamarin
ms.assetid: 70B3C2AB-CB4D-420C-9CFA-20CCFA0E3C78
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: ca33422817f19dbb0a04e8870800d3f5efa8af2a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780064"
---
# <a name="azure-active-directory"></a>Azure Active Directory

_Registrieren einer app zur Verwendung von Azure Active Directory_

Azure Active Directory können Entwickler sichere Ressourcen wie Dateien, Verknüpfungen und Web-APIs, die mit dem gleichen organisationskonto, mit denen Mitarbeiter anmelden, um ihre Systeme oder Überprüfen Sie ihre e-Mail-Nachrichten.

Entwickeln von mobilen Anwendungen, die mit Azure Active Directory authentifiziert werden können, umfasst drei Schritte.
Die ersten beiden Schritte sind in der Regel gleich, unabhängig davon, welche Dienste Sie verwenden möchten. Der dritte Schritt unterscheidet sich für jeden Diensttyp:

  1. [Registrieren bei Azure Active Directory](~/cross-platform/data-cloud/active-directory/get-started/register.md) auf die *windowsazure.com* Portal klicken Sie dann
  2. [Konfigurieren von Diensten](~/cross-platform/data-cloud/active-directory/get-started/configure.md).
  3. Entwickeln von mobilen apps, die unter Verwendung des Diensts.

Beispiele für andere Dienste, die Sie zugreifen können:

- [Graph-API](~/cross-platform/data-cloud/active-directory/graph.md)
- Web-API
- Office 365


## <a name="conclusion"></a>Schlussfolgerung

Mit den Schritten weiter oben aufgeführten kann Ihre mobilen apps für Azure Active Directory authentifizieren. Die Active Directory-Authentifizierungsbibliothek (ADAL) kann viel einfacher mit weniger Codezeilen Weiterleitungstopologie Großteil des Codes identisch sind und daher über Plattformen hinweg freigegeben vornehmen.



## <a name="related-links"></a>Verwandte Links

- [Microsoft NativeClient-Beispiel](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
