<properties
pageTitle="Hinzufügen der Salesforce-API zu PowerApps Enterprise- oder Logik-Apps | Microsoft Azure"
description="Übersicht über die Salesforce-API und REST-API-Parameter"
services=""	
documentationCenter="" 	
authors="msftman"	
manager="erikre"	
editor=""
tags="connectors"/>

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="na"
ms.date="03/16/2016"
ms.author="deonhe"/>

# Erste Schritte mit der Salesforce-API
Verbinden Sie sich mit Salesforce, um z. B. Objekte zu erstellen und abzurufen. Die Salesforce-API kann in Folgendem verwendet werden:

- Logik-Apps 
- PowerApps

> [AZURE.SELECTOR]
- [Logik-Apps](../articles/connectors/connectors-create-api-salesforce.md)
- [PowerApps Enterprise](../articles/power-apps/powerapps-create-api-salesforce.md)

&nbsp;

>[AZURE.NOTE] Diese Version des Artikels gilt für die Schemaversion 2015-08-01-preview für Logik-Apps. Um die Schemaversion 2014-12-01-preview aufzurufen, klicken Sie auf [Salesforce-Connector](../app-service-logic/app-service-logic-connector-salesforce.md).

Salesforce ermöglicht Folgendes:

- Erstellen eines Geschäftsworkflows basierend auf den Daten, die aus Salesforce abgerufen werden. 
- Verwenden von Triggern, wenn ein Objekt erstellt oder aktualisiert wird.
- Verwenden von Aktionen, um z. B. ein Objekt erstellen oder zu löschen. Diese Aktionen erhalten eine Antwort und stellen anschließend die Ausgabe anderen Aktionen zur Verfügung. Wenn z. B. ein neues Objekt in Salesforce erstellt wird, können Sie eine E-Mail über Office 365 senden.
- Hinzufügen der Salesforce-API zu PowerApps Enterprise. Die Benutzer können diese API anschließend in ihren Apps verwenden. 

Informationen zum Hinzufügen einer API in PowerApps Enterprise finden Sie unter [Registrieren einer API in PowerApps](../power-apps/powerapps-register-from-available-apis.md).

Informationen zum Hinzufügen eines Vorgangs in Logik-Apps finden Sie unter [Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md).

## Trigger und Aktionen
Die Salesforce-API weist die folgenden Trigger und Aktionen auf.

| Trigger | Aktionen|
| --- | --- |
|<ul><li>Wenn ein Objekt erstellt wird</li><li>Wenn ein Objekt geändert wird</li></ul> | <ul><li>Objekt erstellen</li><li>Objekte abrufen</li><li>Wenn ein Objekt erstellt wird</li><li>Wenn ein Objekt geändert wird</li><li>Objekt löschen</li><li>Objekt abrufen</li><li>Objekttypen (SObjects) abrufen</li><li>Objekt aktualisieren</li></ul>

Alle APIs unterstützen Daten im JSON- und XML-Format.

## Herstellen einer Verbindung mit Salesforce 

Wenn Sie diese API Ihren Logik-Apps hinzufügen, müssen Sie ihnen das Herstellen einer Verbindung mit Salesforce erlauben.

1. Melden Sie sich bei Ihrem Salesforce-Konto an.
2. Erlauben Sie Ihren Logik-Apps, sich mit Ihrem Salesforce-Konto zu verbinden und es zu nutzen. 

Nachdem Sie die Verbindung hergestellt haben, geben Sie die Salesforce-Eigenschaften ein, z. B. den Tabellennamen. In der **REST-API-Referenz** in diesem Thema werden diese Eigenschaften beschrieben.

>[AZURE.TIP] Sie können dieselbe Verbindung in anderen Logik-Apps verwenden.

## Swagger-REST-API – Referenz
Gilt für Version: 1.0.


### Objekt erstellen
Erstellt ein Salesforce-Objekt. ```POST: /datasets/default/tables/{table}/items```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|string|Ja|path|(Keine)|Salesforce SObject-Typ (Beispiel: Lead)|
|item| |Ja|body|(Keine)|Zu erstellendes Salesforce-Objekt|

### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|die Standardeinstellung|Fehler beim Vorgang.|



### Objekt abrufen
Ruft ein Salesforce-Objekt ab. ```GET: /datasets/default/tables/{table}/items/{id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|string|Ja|path|(Keine)|Salesforce SObject-Typ (Beispiel: Lead)|
|id|string|Ja|path|(Keine)|Eindeutiger Bezeichner des abzurufenden Salesforce-Objekts|

### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|die Standardeinstellung|Fehler beim Vorgang.|



### Objekt löschen
Löscht ein Salesforce-Objekt. ```DELETE: /datasets/default/tables/{table}/items/{id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|string|Ja|path|(Keine)|Salesforce SObject-Typ (Beispiel: Lead)|
|id|string|Ja|path|(Keine)|Eindeutiger Bezeichner des zu löschenden Salesforce-Objekts|

### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|die Standardeinstellung|Fehler beim Vorgang.|



### Objekt aktualisieren
Aktualisiert ein Salesforce-Objekt. ```PATCH: /datasets/default/tables/{table}/items/{id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|string|Ja|path|(Keine)|Salesforce SObject-Typ (Beispiel: Lead)|
|id|string|Ja|path|(Keine)|Eindeutiger Bezeichner des zu aktualisierenden Salesforce-Objekts|
|item| |Ja|body|(Keine)|Salesforce-Objekt mit den geänderten Eigenschaften|

### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|die Standardeinstellung|Fehler beim Vorgang.|



### Wenn ein Objekt erstellt wird
Löst einen Datenfluss aus, wenn ein Objekt in Salesforce erstellt wird. ```GET: /datasets/default/tables/{table}/onnewitems```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|string|Ja|path|(Keine)|Salesforce SObject-Typ (Beispiel: Lead)|
|$skip|integer|no|query|(Keine)|Anzahl der zu überspringenden Einträge (Standardeinstellung = 0)|
|$top|integer|no|query|(Keine)|Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256)|
|$filter|string|no|query|(Keine)|Eine ODATA-Filterabfrage zum Einschränken der Anzahl der Einträge|
|$orderby|string|no|query|(Keine)|Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen|

### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|die Standardeinstellung|Fehler beim Vorgang.|



### Wenn ein Objekt geändert wird 
Löst einen Datenfluss aus, wenn ein Objekt in Salesforce geändert wird. ```GET: /datasets/default/tables/{table}/onupdateditems```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|string|Ja|path|(Keine)|Salesforce SObject-Typ (Beispiel: Lead)|
|$skip|integer|no|query|(Keine)|Anzahl der zu überspringenden Einträge (Standardeinstellung = 0)|
|$top|integer|no|query|(Keine)|Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256)|
|$filter|string|no|query|(Keine)|Eine ODATA-Filterabfrage zum Einschränken der Anzahl der Einträge|
|$orderby|string|no|query|(Keine)|Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen|

### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|die Standardeinstellung|Fehler beim Vorgang.|



## Objektdefinitionen 

#### DataSetsMetadata

| Name | Datentyp | Erforderlich|
|---|---|---|
|tabular|nicht definiert|no|
|Blob|nicht definiert|no|


#### TabularDataSetsMetadata

| Name | Datentyp | Erforderlich|
|---|---|---|
|Quelle|string|no|
|displayName|string|no|
|urlEncoding|string|no|
|tableDisplayName|string|no|
|tablePluralName|string|no|


#### BlobDataSetsMetadata

| Name | Datentyp | Erforderlich|
|---|---|---|
|Quelle|string|no|
|displayName|string|no|
|urlEncoding|string|no|


#### TableMetadata

| Name | Datentyp | Erforderlich|
|---|---|---|
|name|string|no|
|title|string|no|
|x-ms-permission|string|no|
|schema|nicht definiert|no|


#### DataSetsList

| Name | Datentyp | Erforderlich|
|---|---|---|
|value|array|no|


#### DataSet

| Name | Datentyp | Erforderlich|
|---|---|---|
|Name|string|
|DisplayName|string|no|


#### Tabelle

| Name | Datentyp | Erforderlich|
|---|---|---|
|Name|string|no|
|DisplayName|string|no|


#### Item

| Name | Datentyp | Erforderlich|
|---|---|---|
|ItemInternalId|string|no|


#### ItemsList

| Name | Datentyp | Erforderlich|
|---|---|---|
|value|array|no|


#### TablesList

| Name | Datentyp | Erforderlich|
|---|---|---|
|value|array|no|


## Nächste Schritte

[Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md)

Gehen Sie zur [Liste der APIs](apis-list.md) zurück.


[5]: https://developer.salesforce.com
[6]: ./media/connectors-create-api-salesforce/salesforce-developer-homepage.png
[7]: ./media/connectors-create-api-salesforce/salesforce-create-app.png
[8]: ./media/connectors-create-api-salesforce/salesforce-new-app.png

<!---HONumber=AcomDC_0323_2016-->