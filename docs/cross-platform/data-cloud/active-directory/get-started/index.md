---
title: Azure Active Directory
description: In diesem Dokument werden die Schritte beschrieben, die befolgt werden müssen, damit sich ein Mobile App mit Azure Active Directory authentifizieren kann.
ms.prod: xamarin
ms.assetid: 70B3C2AB-CB4D-420C-9CFA-20CCFA0E3C78
author: conceptdev
ms.author: crdun
ms.date: 03/23/2017
ms.openlocfilehash: f51c6af9ed38910f44db2c0886c474deafa096d7
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70289992"
---
# <a name="azure-active-directory"></a>Azure Active Directory

_Registrieren einer APP für die Verwendung Azure Active Directory_

Azure Active Directory ermöglicht es Entwicklern, Ressourcen wie Dateien, Links und Web-APIs mit demselben Organisations Konto zu schützen, mit dem sich die Mitarbeiter bei ihren Systemen anmelden oder Ihre e-Mails überprüfen.

Die Entwicklung mobiler Anwendungen, die sich bei Azure Active Directory authentifizieren können, umfasst drei Schritte.
Die ersten beiden Schritte sind im allgemeinen identisch, unabhängig davon, welche Dienste Sie verwenden möchten. Der dritte Schritt ist für jeden Diensttyp unterschiedlich:

  1. [Registrieren bei Azure Active Directory](~/cross-platform/data-cloud/active-directory/get-started/register.md) im *windowsazure.com* -Portal, dann
  2. [Konfigurieren von Diensten](~/cross-platform/data-cloud/active-directory/get-started/configure.md).
  3. Entwickeln Sie Mobile Apps mithilfe der Dienste.

Beispiele für verschiedene Dienste, auf die Sie zugreifen können:

- [Graph-API](~/cross-platform/data-cloud/active-directory/graph.md)
- Web-API
- Office365


## <a name="conclusion"></a>Schlussbemerkung

Mithilfe der obigen Schritte können Sie Ihre mobilen apps gegen Azure Active Directory authentifizieren. Die Active Directory-Authentifizierungsbibliothek (Adal) vereinfacht die Verwendung von weniger Codezeilen erheblich, während der Großteil des Codes gleich bleibt und die plattformübergreifende Bereitstellung ermöglicht wird.



## <a name="related-links"></a>Verwandte Links

- [Microsoft nativeClient-Beispiel](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
