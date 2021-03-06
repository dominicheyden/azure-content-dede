<properties
	pageTitle="Azure Insights: Verwenden von automatischen Skalierungsvorgängen zum Senden von E-Mail- und Webhook-Warnbenachrichtigungen | Microsoft Azure"
	description="Erfahren Sie, wie Sie automatischer Skalierungsvorgänge in Azure Insights nutzen können, um Web-URLs aufzurufen oder E-Mail-Benachrichtigungen zu senden."
	authors="kamathashwin"
	manager=""
	editor=""
	services="azure-portal"
	documentationCenter="na"/>

<tags
	ms.service="azure-portal"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="03/30/2016"
	ms.author="ashwink"/>

# Verwenden von automatischen Skalierungsvorgängen zum Senden von E-Mail- und Webhook-Warnbenachrichtigungen in Azure Insights

In diesem Artikel wird beschrieben, wie Sie Trigger einrichten, damit Sie basierend auf Skalierungsvorgängen in Azure bestimmte Web-URLs aufrufen oder E-Mails senden können.

## Webhooks
Mithilfe von Webhooks können Sie die Azure-Warnbenachrichtigungen für die Nachbearbeitung oder benutzerdefinierte Benachrichtigungen an andere Systeme weiterleiten. Beispielsweise können Warnungen an Dienste weitergeleitet werden, die eine eingehende Webanforderung zum Senden einer SMS, zum Protokollieren von Fehlern oder zum Benachrichtigen eines Teams über Chat- oder Messagingdienste usw. verarbeiten können. Der URI des Webhooks muss ein gültiger HTTP- oder HTTPS-Endpunkt sein.

## E-Mail
E-Mails können an eine beliebige gültige E-Mail-Adresse gesendet werden. Administratoren und Co-Administratoren des Abonnements, in der die Regel ausgeführt wird, werden ebenfalls benachrichtigt.


## Clouddienste und Web-Apps
Sie können über das Azure-Portal für Cloud-Dienste und Serverfarmen (Web-Apps) teilnehmen.

- Wählen Sie die Metrik **skalieren nach**.

![skalieren nach](./media/insights-autoscale-to-webhook-email/insights-autoscale-scale-by.png)

## Skalierungsgruppen für virtuelle Computer
Bei neueren ARM-basierten virtuellen Computern (Skalierungsgruppen für virtuelle Computer) können Sie die REST-API, PowerShell und Befehlszeilenschnittstelle für die Konfiguration verwenden. Eine Portalschnittstelle ist noch nicht verfügbar.


## Authentifizierung in Webhooks
Es gibt zwei Arten von Authentifizierungs-URIs:

	1. Token-base authentication, where you save the webhook URI with a token ID as a query parameter. For example, https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue
	2. Basic authentication, where you use a user ID and password. For example, https://userid:password@mysamplealert/webcallback?someparamater=somevalue&parameter=value

## Benachrichtigung über automatische Skalierung mit dem Webhook-Nutzlastschema
Beim Generieren der Benachrichtigung über automatische Skalierung werden die folgenden Metadaten in die Webhook-Nutzlast aufgenommen:

```
{
        "version": "1.0",
        "status": "Activated",
        "operation": "Scale In",
        "context": {
                "timestamp": "2016-03-11T07:31:04.5834118Z",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/myautoscaleSetting",
                "name": "myautoscaleSetting",
                "details": "Autoscale successfully started scale operation for resource 'MyCSRole' from capacity '3' to capacity '2'",
                "subscriptionId": "s1",
                "resourceGroupName": "rg1",
                "resourceName": "MyCSRole",
                "resourceType": "microsoft.classiccompute/domainnames/slots/roles",
                "resourceId": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService/slots/Production/roles/MyCSRole",
                "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService",
                "oldCapacity": "3",
                "newCapacity": "2"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```


|Feld |Erforderlich?|	Beschreibung|
|---|---|---|
|status |Ja |Status, der angibt, dass ein automatischer Skalierungsvorgang generiert wurde|
|Vorgang|	Ja |Der Vorgang zum Erhöhen der Anzahl der Instanzen ist „Horizontal hochskalieren“ und „Horizontal herunterskalieren“ zum Verringern der Anzahl der Instanzen.|
|context|	Ja |Der Kontext des automatischen Skalierungsvorgangs|
|timestamp|	Ja |Zeitstempel der Auslösung des automatischen Skalierungsvorgangs|
|id |Ja|	ARM (Azure Resource Manager)-ID der Einstellung für die automatische Skalierung|
|name |Ja|	Name der Einstellung für die automatische Skalierung|
|Details|	Ja |Erläuterung der Aktion, die der Dienst für die automatische Skalierung ausgeführt hat und der Änderung der Instanzenanzahl|
|subscriptionId|	Ja |Abonnement-ID der Zielressource, die skaliert wird|
|ResourceGroupName|	Ja|	Ressourcengruppenname der Zielressource, die skaliert wird|
|resourceName |Ja|	Name der Zielressource, die skaliert wird|
|resourceType |Ja|	Die drei unterstützten Werte: „microsoft.classiccompute/domainnames/slots/roles“ – Clouddienstrollen, „microsoft.compute/virtualmachinescalesets“ – Skalierungsgruppen für virtuelle Computer und „Microsoft.Web/serverfarms“ – Web-App|
|Ressourcen-ID |Ja|ARM-ID der Zielressource, die skaliert wird|
|portalLink |Ja |Link vom Azure-Portal zur Zusammenfassungsseite der Zielressource|
|oldCapacity|	Ja |Die aktuelle (alte) Anzahl von Instanzen, wenn die automatische Skalierung eine Skalierungsaktion durchgeführt hat|
|newCapacity|	Ja |Die neue Anzahl der Instanzen, auf die die automatische Skalierung die Ressource skaliert hat|
|Eigenschaften|	Nein|	Optional. Gruppe von <Key  Value>-Paaren (z. B. Wörterbuch <String  String>). Das Feld "properties" ist optional. In einer angepassten Benutzeroberfläche oder einem auf Logik-Apps basierenden Workflow können Sie Schlüssel und Werte eingeben, die mithilfe der Nutzlast übergeben werden können. Alternativ können benutzerdefinierte Eigenschaften über den Webhook-URI selbst (als Abfrageparameter) an den ausgehenden Webhook-Aufruf zurückgegeben werden.|

<!---HONumber=AcomDC_0330_2016-->