---
title: Einrichten der Windows-Projekte
description: "Hinzufügen von neuen Windows-Projekten zu einer vorhandenen Xamarin.Forms-Projektmappe"
ms.topic: article
ms.prod: xamarin
ms.assetid: A0774D2E-6994-4D91-84E8-DAB66FC92320
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: 2acabc68c595bfb3bbd5c3516f68cd777ba10327
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="setup-windows-projects"></a>Einrichten der Windows-Projekte

_Hinzufügen von neuen Windows-Projekten zu einer vorhandenen Xamarin.Forms-Projektmappe_

Ältere Xamarin.Forms-Projekte (oder solche, die auf Mac OS erstellt&nbsp;X) hat keine Einrichten dieser Windows-Projekte.

Dies bedeutet, dass Sie diese Projekttypen zum Erstellen von Anwendungen für Windows 8.1, Windows Phone 8.1 und Windows 10 (UWP) manuell hinzufügen müssen.

## <a name="add-a-windows-81-app"></a>Hinzufügen einer Windows 8.1-app

* Wenn Sie die Vorlage PCL verwendet [das Profil aktualisieren](#pcl), klicken Sie dann
* [Hinzufügen einer Windows 8.1-app](~/xamarin-forms/platform/windows/installation/tablet.md) für Tablet/Desktop Formfaktoren.

## <a name="add-a-windows-phone-81-app"></a>Hinzufügen einer Windows Phone 8.1-app

* Wenn Sie die Vorlage PCL verwendet [das Profil aktualisieren](#pcl), klicken Sie dann
* [Hinzufügen einer Windows Phone 8.1-app](~/xamarin-forms/platform/windows/installation/phone.md)

## <a name="add-a-universal-windows-platform-uwp-app"></a>Fügen Sie eine universelle Windows app-Plattform (UWP)

* Erstellen von [UWP](https://msdn.microsoft.com/library/windows/apps/dn894631.aspx) apps erfordert Visual Studio 2015 auf Windows 10 ausgeführt wird.
* Wenn Sie die Vorlage PCL verwendet [das Profil aktualisieren](#pcl), klicken Sie dann
* [Fügen Sie eine universelle Windows Plattform-app](~/xamarin-forms/platform/windows/installation/universal.md)

<a name="pcl" />

### <a name="update-your-pcl-profile"></a>Aktualisieren Sie das PCL-Profil

Wenn Ihre vorhandene Xamarin.Forms-app die Portable Klassenbibliothek (PCL)-Vorlage verwendet, müssen Sie ein Profil aktualisieren.

1. **mit der rechten Maustaste > Eigenschaften** (bestehenden Einstellungen können abweichen)

  ![](images/targets.png "PCL-Ziele")

2. Klicken Sie auf die **ändern...**  Schaltfläche

3. Stellen Sie sicher der **Windows 8** und **Windows Phone 8.1** Optionen aktiviert sind (und **Windows Phone Silveright** ist *deaktiviert*):

  ![](images/pcl.png "PCL Zieloptionen")

4. Drücken Sie **OK** und die Änderungen zu speichern.

Dies ist identisch mit dem **Profil 111** , wenn Sie Ihre PCL in Visual Studio für Mac mithilfe der Dropdown-Liste konfigurieren.

  ![](images/pcl-xs.png "PCL Profil 111")

**Hinweis:** Wenn Ihre Projektmappe noch kein Windows Phone 8 Silverlight-Projekt enthält, sollte die PCL auf 259 Profil festgelegt werden. Silverlight für Windows Phone 8-Unterstützung ist als veraltet eingestuft, daher wird empfohlen, dass Sie es durch die Projekttypen, die auf dieser Seite angezeigten ersetzen.
