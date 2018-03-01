---
title: "Verwenden einer mobilen Anwendung für Azure"
description: "Azure Mobile Apps können Sie zum Entwickeln von apps mit skalierbare Back-Ends mit Unterstützung für mobile-Authentifizierung, offline-Synchronisierung und Push-Benachrichtigungen in Azure App Service, gehostet. Dieser Artikel, die nur für Azure Mobile Apps, die eine Node.js-Back-End verwenden gilt, werden die Abfragen, einfügen, aktualisieren und Löschen von Daten in einer Tabelle in einer Instanz von Azure-Mobile-Apps erläutert."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2B3EFD0A-2922-437D-B151-4B4DE46E2095
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 5b087700e3a5276e19454a8dafedb508758b7b71
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="consuming-an-azure-mobile-app"></a>Verwenden einer mobilen Anwendung für Azure

_Azure Mobile Apps können Sie zum Entwickeln von apps mit skalierbare Back-Ends mit Unterstützung für mobile-Authentifizierung, offline-Synchronisierung und Push-Benachrichtigungen in Azure App Service, gehostet. Dieser Artikel, die nur für Azure Mobile Apps, die eine Node.js-Back-End verwenden gilt, werden die Abfragen, einfügen, aktualisieren und Löschen von Daten in einer Tabelle in einer Instanz von Azure-Mobile-Apps erläutert._

Informationen zum Erstellen einer Azure-Mobile-Apps-Instanz, die von Xamarin.Forms genutzt werden können, finden Sie unter [erstellen Sie eine app Xamarin.Forms](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started/). Nach der folgenden Anweisungen, die herunterladbaren Beispiel-Anwendung konfiguriert werden kann durch Festlegen die Instanz von Azure Mobile Apps nutzen die `Constants.ApplicationURL` an die URL der Azure-Mobile Apps-Instanz. Klicken Sie dann beim Ausführen der beispielanwendung werden sie mit der Azure-Mobile Apps-Instanz verbunden, wie im folgenden Screenshot gezeigt:

![](azure-images/portal.png "Beispielanwendung")

Zugriff auf Azure Mobile Apps erfolgt über die [Azure Mobile Client-SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/), und alle Verbindungen aus dem Xamarin.Forms-beispielanwendung in Azure über HTTPS hergestellt werden.

> [!NOTE]
> In iOS 9 und höher erzwingt App-Transport-Sicherheit (ATS) sichere Verbindungen zwischen Internetressourcen (z. B. die app-Back-End-Server) und der app, um das unbeabsichtigten Weitergabe von vertraulichen Informationen zu verhindern. Da ATS in apps für iOS 9 erstellt standardmäßig aktiviert ist, werden alle Verbindungen unterliegen ATS sicherheitsanforderungen. Wenn Verbindungen mit diesen Anforderungen nicht erfüllen, werden sie mit einer Ausnahme fehl.
> ATS können von hinzugewählt werden, ist es nicht möglich ist, verwenden Sie die `HTTPS` -Protokolls und sichere Kommunikation für Ressourcen im Internet. Dies kann erreicht werden, indem Sie die app aktualisiert **"Info.plist"** Datei. Weitere Informationen finden Sie unter [App Transportsicherheit](~/ios/app-fundamentals/ats.md).

## <a name="consuming-an-azure-mobile-app-instance"></a>Nutzen eine Instanz von Azure Mobile-App

Die [Azure Mobile Client-SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) bietet die `MobileServiceClient` Klasse, die von einer Xamarin.Forms-Anwendung verwendet wird Zugriff auf die Azure-Mobile Apps-Instanz, wie im folgenden Codebeispiel gezeigt:

```csharp
IMobileServiceTable<TodoItem> todoTable;
MobileServiceClient client;

public TodoItemManager ()
{
  client = new MobileServiceClient (Constants.ApplicationURL);
  todoTable = client.GetTable<TodoItem> ();
}
```

Wenn die `MobileServiceClient` Instanz erstellt wird, eine Anwendung-URL muss angegeben werden, zur Identifikation der Instanz von Azure Mobile Apps. Dieser Wert kann abgerufen werden, über das Dashboard für die mobile app in der [Microsoft Azure-Verwaltungsportal](https://portal.azure.com/).

Ein Verweis auf die `TodoItem` Tabelle gespeichert ist, in der Azure-Mobile Apps-Instanz abgerufen werden muss, bevor Sie Vorgänge für diese Tabelle ausgeführt werden können. Dies erfolgt durch Aufrufen der `GetTable` Methode für die `MobileServiceClient` Instanz fest, welche gibt einen `IMobileServiceTable<TodoItem>` Verweis.

### <a name="querying-data"></a>Abfrage von Daten

Die Inhalte einer Tabelle abgerufen werden können, durch Aufrufen der `IMobileServiceTable.ToEnumerableAsync` -Methode, die asynchron wertet die Abfrage und die Ergebnisse zurückgibt. Daten können auch sein gefiltert Serverseite dazu eine `Where` -Klausel der Abfrage. Die `Where` Klausel wendet ein zeilenfilterungsprädikat auf die Abfrage für die Tabelle aus, wie im folgenden Codebeispiel gezeigt:

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

Diese Abfrage gibt alle Elemente aus der `TodoItem` Tabelle, deren `Done` -Eigenschaft gleich `false`. Die Abfrageergebnisse werden anschließend platziert eine `ObservableCollection` für die Anzeige.

### <a name="inserting-data"></a>Einfügen von Daten

Beim Einfügen von Daten in der Instanz von Azure Mobile Apps, neue Spalten automatisch generiert werden in der Tabelle nach Bedarf bereitgestellt, dynamische Schema in der Azure-Mobile Apps-Instanz aktiviert ist. Die `IMobileServiceTable.InsertAsync` Methode wird verwendet, um eine neue Zeile mit Daten in der angegebenen Tabelle einfügen wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task SaveTaskAsync (TodoItem item)
{
  ...
  await todoTable.InsertAsync (item);
  ...
}
```

Wenn Sie eine einfügeanforderung ausführen zu können, muss eine ID nicht in den Daten übergeben werden, mit der Azure-Mobile Apps-Instanz angegeben werden. Wenn die Insert-Anforderung eine ID enthält einen `MobileServiceInvalidOperationException` ausgelöst.

Nach der `InsertAsync` -Methode abgeschlossen wurde, die die Daten in der Azure-Mobile Apps-Instanz-ID wird aufgefüllt, der `TodoItem` Instanz in der Anwendung Xamarin.Forms.

### <a name="updating-data"></a>Aktualisieren von Daten

Beim Aktualisieren von Daten in der Azure-Mobile Apps-Instanz neue Spalten automatisch generiert werden in der Tabelle nach Bedarf bereitgestellt, dynamische Schema in der Azure-Mobile Apps-Instanz aktiviert ist. Die `IMobileServiceTable.UpdateAsync` Methode wird zum Aktualisieren von vorhandener Daten mit neuen Informationen verwendet, wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task SaveTaskAsync (TodoItem item)
{
  ...
  await todoTable.UpdateAsync (item);
  ...
}
```

Wenn eine updateanforderung ausführen, wird eine ID muss angegeben werden, damit die Azure-Mobile Apps-Instanz die zu aktualisierenden Daten identifizieren kann. Diese ID-Wert befindet sich in der `TodoItem.ID` Eigenschaft. Wenn die updateanforderung eine ID enthält, besteht keine Möglichkeit für die Azure Mobile Apps-Instanz, um zu bestimmen, die Daten aktualisiert werden, und somit eine `MobileServiceInvalidOperationException` ausgelöst.

### <a name="deleting-data"></a>Löschen von Daten

Die `IMobileServiceTable.DeleteAsync` Methode dient zum Löschen von Daten aus einer Tabelle mit Azure Mobile Apps, wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task DeleteTaskAsync (TodoItem item)
{
  ...
  await todoTable.DeleteAsync(item);
  ...
}
```

Wenn eine Delete-Anforderung ausführen, wird eine ID muss angegeben werden, damit die Azure-Mobile-App-Sinstance der zu löschenden Daten identifizieren kann. Diese ID-Wert befindet sich in der `TodoItem.ID` Eigenschaft. Wenn die Delete-Anforderung eine ID enthält, besteht keine Möglichkeit für die Azure Mobile Apps-Instanz, um die Daten zu bestimmen, die gelöscht, und daher ein `MobileServiceInvalidOperationException` ausgelöst.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird erläutert, wie mithilfe der [Azure Mobile Client-SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) auf Abfragen, einfügen, aktualisieren und Löschen von Daten in einer Tabelle in einer Azure-Mobile apps-Instanz gespeichert. Das SDK bietet die `MobileServiceClient` -Klasse, die von einer Xamarin.Forms-Anwendung verwendet wird, Zugriff auf die Azure-Mobile Apps-Instanz.


## <a name="related-links"></a>Verwandte Links

- [TodoAzure (sample)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzure/)
- [Erstellen Sie eine Xamarin.Forms-app](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started/)
- [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
