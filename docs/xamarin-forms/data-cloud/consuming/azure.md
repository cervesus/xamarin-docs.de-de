---
title: Verwenden einer Azure-Mobile-App
description: In diesem Artikel, der nur für Azure Mobile Apps, die eine Node.js-Back-End verwenden, wird erläutert, wie zum Abfragen, einfügen, aktualisieren und Löschen von Daten in einer Tabelle in einer Azure Mobile Apps-Instanz gespeichert werden.
ms.prod: xamarin
ms.assetid: 2B3EFD0A-2922-437D-B151-4B4DE46E2095
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 8bd77ec4975fae3cc7245c5adc2b5ef18568b9e1
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57667581"
---
# <a name="consuming-an-azure-mobile-app"></a>Verwenden einer Azure-Mobile-App

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzure/)

_Azure Mobile Apps können Sie zum Entwickeln von apps mit skalierbaren Back-Ends in Azure App Service gehostet wird, mit Unterstützung für mobile Authentifizierung, offlinesynchronisierung und Pushbenachrichtigungen. In diesem Artikel, der nur für Azure Mobile Apps, die eine Node.js-Back-End verwenden, wird erläutert, wie zum Abfragen, einfügen, aktualisieren und Löschen von Daten in einer Tabelle in einer Azure Mobile Apps-Instanz gespeichert werden._

> [!NOTE]
> Ab dem 30. Juni, werden alle neuen Azure Mobile Apps mit TLS 1.2 standardmäßig erstellt werden. Darüber hinaus, es wird außerdem empfohlen, dass für vorhandene Azure Mobile Apps neu konfiguriert werden, um die Verwendung von TLS 1.2. Informationen zum Erzwingen von TLS 1.2 in Azure Mobile Apps finden Sie unter [Erzwingen von TLS-Versionen](/azure/app-service/app-service-web-tutorial-custom-ssl#enforce-tls-versions). Informationen zum Konfigurieren von Xamarin-Projekte zur Verwendung von TLS 1.2 finden Sie unter [Transport Layer Security (TLS) 1.2](~/cross-platform/app-fundamentals/transport-layer-security.md).

Weitere Informationen zum Erstellen einer Azure Mobile Apps-Instanz, die von Xamarin.Forms genutzt werden können, finden Sie unter [erstellen eine Xamarin.Forms-app](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started/). Nach folgenden Anweisungen, herunterladbare Anwendung für die Nutzung der Azure Mobile Apps-Instanz durch Festlegen von konfiguriert werden kann die `Constants.ApplicationURL` an die URL der Azure Mobile Apps-Instanz. Klicken Sie dann beim Ausführen der beispielanwendung wird es auf die Azure Mobile Apps-Instanz, eine Verbindung herstellen wie im folgenden Screenshot gezeigt:

![](azure-images/portal.png "Beispielanwendung")

Der Zugriff mit Azure Mobile Apps erfolgt über die [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/), und alle Verbindungen aus der Xamarin.Forms-beispielanwendung in Azure werden über HTTPS hergestellt.

> [!NOTE]
> In iOS 9 und höher erzwingt (App Transport Security, ATS) sichere Verbindungen zwischen der Internet-Ressourcen (z. B. die app Back-End-Server) und die app, und verhindert versehentliche Offenlegung vertraulicher Informationen. Da ATS in apps für iOS 9, die standardmäßig aktiviert ist, werden alle Verbindungen unterliegen ATS-sicherheitsanforderungen. Wenn die Verbindungen nicht über diese Anforderungen erfüllen, werden sie mit einer Ausnahme fehlschlagen.
> ATS können von entschieden werden, ist dies nicht möglich, verwenden Sie die `HTTPS` -Protokolls und Sichern der Kommunikation für Ressourcen im Internet. Dies kann erreicht werden, durch die Aktualisierung der app **"Info.plist"** Datei. Weitere Informationen finden Sie unter [App Transport Security](~/ios/app-fundamentals/ats.md).

## <a name="consuming-an-azure-mobile-app-instance"></a>Nutzen eine Azure Mobile App-Instanz

Die [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) bietet die `MobileServiceClient` -Klasse, die von einer Xamarin.Forms-Anwendung verwendet wird, Zugriff auf die Azure Mobile Apps-Instanz, wie im folgenden Codebeispiel gezeigt:

```csharp
IMobileServiceTable<TodoItem> todoTable;
MobileServiceClient client;

public TodoItemManager ()
{
  client = new MobileServiceClient (Constants.ApplicationURL);
  todoTable = client.GetTable<TodoItem> ();
}
```

Wenn die `MobileServiceClient` Instanz erstellt wird, wird eine Anwendungs-URL muss angegeben werden, um die Azure Mobile Apps-Instanz zu identifizieren. Dieser Wert kann abgerufen werden, über das Dashboard für die mobile app in der [Microsoft Azure-Portal](https://portal.azure.com/).

Ein Verweis auf die `TodoItem` Tabelle gespeichert, in der Azure Mobile Apps-Instanz abgerufen werden muss, bevor Vorgänge für die Tabelle ausgeführt werden können. Dies erfolgt durch Aufrufen der `GetTable` Methode für die `MobileServiceClient` -Instanz, gibt eine `IMobileServiceTable<TodoItem>` Verweis.

### <a name="querying-data"></a>Abfrage von Daten

Den Inhalt einer Tabelle abgerufen werden können, durch Aufrufen der `IMobileServiceTable.ToEnumerableAsync` Methode, die asynchron wertet die Abfrage und gibt die Ergebnisse zurück. Können auch sein, Daten gefiltert, serverseitige dazu eine `Where` Klausel in der Abfrage. Die `Where` Klausel wendet ein zeilenfilterungsprädikat auf die Abfrage für die Tabelle aus, wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task<ObservableCollection<TodoItem>> GetTodoItemsAsync (bool syncItems = false)
{
  ...
  IEnumerable<TodoItem> items = await todoTable
              .Where (todoItem => !todoItem.Done)
              .ToEnumerableAsync ();

  return new ObservableCollection<TodoItem> (items);
}
```

Diese Abfrage gibt alle Elemente aus der `TodoItem` Tabelle, deren `Done` Eigenschaft `false`. Die Ergebnisse der Abfrage werden dann aber in einer `ObservableCollection` für die Anzeige.

### <a name="inserting-data"></a>Einfügen von Daten

Beim Einfügen von Daten in der Azure Mobile Apps-Instanz, neue Spalten automatisch generiert werden in der Tabelle nach Bedarf zur Verfügung gestellt, dynamische Schema aktiviert ist, in der Azure Mobile Apps-Instanz. Die `IMobileServiceTable.InsertAsync` Methode wird verwendet, um eine neue Zeile mit Daten in der angegebenen Tabelle einfügen wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task SaveTaskAsync (TodoItem item)
{
  ...
  await todoTable.InsertAsync (item);
  ...
}
```

Wenn Sie eine einfügeanforderung ausführen zu können, muss eine ID nicht in den Daten übergeben werden, mit der Azure Mobile Apps-Instanz angegeben werden. Wenn die Insert-Anforderung eine ID enthält einen `MobileServiceInvalidOperationException` ausgelöst.

Nach der `InsertAsync` -Methode abgeschlossen wurde, wird die ID der Daten in der Azure Mobile Apps-Instanz aufgefüllt werden, der `TodoItem` Instanz in der Xamarin.Forms-Anwendung.

### <a name="updating-data"></a>Aktualisieren von Daten

Beim Aktualisieren von Daten in der Azure Mobile Apps-Instanz neue Spalten automatisch generiert werden in der Tabelle nach Bedarf zur Verfügung gestellt, dynamische Schema aktiviert ist, in der Azure Mobile Apps-Instanz. Die `IMobileServiceTable.UpdateAsync` Methode dient zum Aktualisieren von vorhandener Daten mit neuen Informationen, wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task SaveTaskAsync (TodoItem item)
{
  ...
  await todoTable.UpdateAsync (item);
  ...
}
```

Wenn eine updateanforderung ausführen, muss eine ID angegeben werden, damit die Azure Mobile Apps-Instanz, die zu aktualisierenden Daten identifizieren kann. Dieser ID-Wert befindet sich in der `TodoItem.ID` Eigenschaft. Wenn die Anforderung zum Aktualisieren eine ID enthalten nicht, es gibt keine Möglichkeit für die Azure Mobile Apps-Instanz, um zu bestimmen, die Daten aktualisiert werden, und damit eine `MobileServiceInvalidOperationException` ausgelöst.

### <a name="deleting-data"></a>Löschen von Daten

Die `IMobileServiceTable.DeleteAsync` Methode dient zum Löschen von Daten aus einer Azure Mobile Apps-Tabelle, wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task DeleteTaskAsync (TodoItem item)
{
  ...
  await todoTable.DeleteAsync(item);
  ...
}
```

Wenn Sie eine Delete-Anforderung ausführen zu können, muss eine ID angegeben werden, damit der Azure Mobile App Sinstance der zu löschenden Daten identifizieren kann. Dieser ID-Wert befindet sich in der `TodoItem.ID` Eigenschaft. Wenn die Delete-Anforderung keine ID enthält, besteht keine Möglichkeit für die Azure Mobile Apps-Instanz, um zu bestimmen, die Daten gelöscht werden soll, und daher eine `MobileServiceInvalidOperationException` ausgelöst.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel erläutert, wie Sie mit der [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) zum Abfragen, einfügen, aktualisieren und Löschen von Daten in einer Tabelle in einer Azure Mobile apps-Instanz gespeichert. Das SDK stellt die `MobileServiceClient` -Klasse, die von einer Xamarin.Forms-Anwendung verwendet wird, Zugriff auf die Azure Mobile Apps-Instanz.


## <a name="related-links"></a>Verwandte Links

- [TodoAzure (sample)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzure/)
- [Erstellen einer Xamarin.Forms-app](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started/)
- [Azure Mobile Client-SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
