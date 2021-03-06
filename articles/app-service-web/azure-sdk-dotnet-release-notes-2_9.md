<properties 
   pageTitle="Versionshinweise zu Azure SDK für .NET 2.9" 
   description="Versionshinweise zu Azure SDK für .NET 2.9" 
   services="app-service\web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="03/29/2016"
   ms.author="juliako"/>


# Versionshinweise zu Azure SDK für .NET 2.9

##Übersicht

Dieses Dokument enthält die Versionshinweise für das Azure SDK für .NET 2.9.

Ausführliche Informationen zu Updates in dieser Version finden Sie im [Ankündigungsbeitrag zu Azure SDK 2.9](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).

## Azure SDK 2.9 für Visual Studio 2015 Update 2 und Visual Studio „15“-Vorschau
 
Dieses Update umfasst die folgenden Fehlerbehebungen:

- Problem bei der Generierung des REST-API-Clients, bei dem die Zeichenfolge „Unbekannter Typ“ als Name des Ordners zur Codegenerierung und/oder als Name des im generierten Code eingefügten Namespace angezeigt wurde.
- Problem im Zusammenhang mit geplanten WebJobs, bei dem die Authentifizierungsinformationen nicht an den Vorgang zur Bereitstellung von Scheduler übergeben wurden.

Dieses Update umfasst die folgende neue Funktion:

- Unterstützung für sekundäre App Services auf der Registerkarte „Dienste“ des Dialogfelds zur Bereitstellung von App Services. 

##Azure Data Lake Tools für Visual Studio 2015 Update 2
 
Dieses Update umfasst Folgendes:

- **Azure Data Lake Tools** für Visual Studio ist jetzt im Azure SDK für .NET enthalten. Das Tool wird beim Installieren von Azure SDK automatisch installiert. 

	Das Tool wird regelmäßig aktualisiert, rufen Sie die Updates daher [hier](http://aka.ms/datalaketool) ab.

- Mit **Server-Explorer** können Sie nun alle U-SQL-Metadatenentitäten anzeigen und einige U-SQL-Metadatenentitäten erstellen. Weitere Informationen finden Sie in [diesem]() Blog.


##HDInsight-Tools 

**HDInsight Tools** für Visual Studio unterstützt jetzt HDInsight Version 3.3, einschließlich der Anzeige von Tez-Diagrammen und anderen Sprachkorrekturen.


##Azure-Ressourcen-Manager 

Diese Version umfasst nun die [KeyVault](../resource-manager-keyvault-parameter.md)-Unterstützung für ARM-Vorlagen.

##Weitere Informationen

[Ankündigungsbeitrag zu Azure SDK 2.9 (in englischer Sprache)](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/)

<!---HONumber=AcomDC_0330_2016-->