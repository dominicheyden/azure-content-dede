<properties
	pageTitle="Erste Schritte mit Azure IoT Hub für Java | Microsoft Azure"
	description="Tutorial für den Einstieg in Azure IoT Hub mit Java. Verwenden Sie Azure IoT Hub und Java mit den Microsoft Azure IoT SDKs, um eine IoT-Lösung zu implementieren."
	services="iot-hub"
	documentationCenter="java"
	authors="dominicbetts"
	manager="timlt"
	editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="03/22/2016"
     ms.author="dobett"/>

# Erste Schritte mit Azure IoT Hub für Java

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

## Einführung

Azure IoT Hub ist ein vollständig verwalteter Dienst, der eine zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von IoT-Geräten und einem Lösungs-Back-End ermöglicht. Eines der größten Probleme im Zusammenhang mit IoT-Projekten ist die sichere und zuverlässige Verbindung von Geräten mit dem Lösungs-Back-End. Um diese Herausforderungen zu meistern, bietet IoT Hub:

- Ein zuverlässiges, hyperskalierbares Messaging zwischen Geräten und Cloud (Device-to-Cloud, D2C) sowie zwischen Cloud und Geräten (Cloud-to-Device, C2D)
- Eine sichere Kommunikation unter Verwendung von Zugriffssteuerung und Sicherheitsanmeldeinformationen auf Gerätebasis
- Gerätebibliotheken für die gängigsten Sprachen und Plattformen

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

- Erstellen eines IoT Hubs mit dem Azure-Portal
- Erstellen einer Geräteidentität im IoT Hub
- Erstellen Sie ein simuliertes Gerät, das Telemetriedaten an Ihr Cloud-Back-End sendet.

Am Ende dieses Tutorials verfügen Sie über drei Java-Konsolenanwendungen:

* **create-device-identity**. Hiermit werden eine Geräteidentität und ein zugehöriger Sicherheitsschlüssel zum Verbinden mit Ihrem simulierten Gerät erstellt.
* **read-d2c-messages**. Hiermit wird die Telemetrie angezeigt, die Ihr simuliertes Gerät sendet.
* **simulated-device**. Hiermit wird mithilfe der zuvor erstellten Geräteidentität eine Verbindung mit Ihrem IoT Hub hergestellt und jede Sekunde eine Telemetrienachricht gesendet.

> [AZURE.NOTE] Im Artikel [IoT Hub-SDKs][lnk-hub-sdks] finden Sie Informationen über die verschiedenen SDKs, mit denen Sie sowohl Anwendungen für Geräte als auch das zugehörige Lösungs-Back-End erstellen können.

Für dieses Tutorial benötigen Sie Folgendes:

+ Java SE 8. <br/> Unter [Vorbereiten Ihrer Entwicklungsumgebung][lnk-dev-setup] wird beschrieben, wie Sie für dieses Tutorial Java unter Windows oder Linux installieren.

+ Maven 3. <br/> Unter [Vorbereiten Ihrer Entwicklungsumgebung][lnk-dev-setup] wird beschrieben, wie Sie für dieses Tutorial Maven unter Windows oder Linux installieren.

+ Ein aktives Azure-Konto. <br/>Wenn Sie noch kein Konto haben, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Einzelheiten finden Sie unter [Kostenlose Azure-Testversion][lnk-free-trial].

## Erstellen eines IoT Hubs

Sie müssen einen IoT Hub erstellen, mit dem Ihr simuliertes Gerät verbunden werden kann. Die folgenden Schritte veranschaulichen, wie Sie diese Aufgabe mit dem Azure-Portal ausführen.

1. Melden Sie sich beim [Azure-Portal][lnk-portal] an.

2. Klicken Sie in der Navigationsleiste auf **Neu**, klicken Sie auf **Internet der Dinge** und dann auf **Azure IoT Hub**.

    ![][1]

3. Wählen Sie auf dem Blatt **IoT Hub** die gewünschte Konfiguration für Ihren IoT Hub.

    ![][2]

    * Geben Sie im Feld **Name** einen Namen für Ihren IoT Hub ein. Wenn der **Name** gültig und verfügbar ist, wird im Feld **Name** ein grünes Häkchen angezeigt.
    * Wählen Sie eine **Preis- und Skalierungsstufe** aus. Für dieses Tutorial ist keine bestimmte Stufe erforderlich.
    * Erstellen Sie in **Ressourcengruppe** eine neue Ressourcengruppe, oder wählen Sie eine vorhandene aus. Weitere Informationen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen][lnk-resource-groups].
    * Wählen Sie in **Standort** den Standort aus, an dem Ihr IoT Hub gehostet werden soll.  

4. Wenn Sie die Konfigurationsoptionen für Ihren IoT Hub ausgewählt haben, klicken Sie auf **Erstellen**. Die Erstellung des IoT Hubs kann einige Minuten dauern. Im Startmenü oder im Benachrichtigungsbereich können Sie den Fortschritt überwachen und den Status überprüfen.

    ![][3]

5. Nachdem der IoT Hub erfolgreich erstellt wurde, öffnen Sie das Blatt für den neuen IoT Hub, notieren Sie den **Hostnamen**, und klicken Sie dann auf das **Schlüsselsymbol**.

    ![][4]

6. Klicken Sie auf die Richtlinie **iothubowner**, und kopieren Sie oder notieren Sie die Verbindungszeichenfolge im Blatt **iothubowner**.

    ![][5]

7. Klicken Sie auf dem IoT Hub-Blatt auf **Einstellungen** und auf dem Blatt **Einstellungen** auf **Messaging**. Notieren Sie sich auf dem Blatt **Messaging** die Angaben für **Event Hub-kompatibler Name** und **Event Hub-kompatibler Endpunkt**. Sie benötigen diese Werte beim Erstellen der Anwendung **read-d2c-messages**.

    ![][6]

Sie haben nun Ihren IoT Hub erstellt und verfügen über den IoT Hub-Hostnamen, die IoT Hub-Verbindungszeichenfolge, den Event Hub-kompatiblen Namen und den Event Hub-kompatiblen Endpunkt. Diese Angaben benötigen Sie, um den Rest dieses Tutorials abschließen zu können.

[AZURE.INCLUDE [iot-hub-get-started-cloud-java](../../includes/iot-hub-get-started-cloud-java.md)]


[AZURE.INCLUDE [iot-hub-get-started-device-java](../../includes/iot-hub-get-started-device-java.md)]

## Ausführen der Anwendungen

Sie können nun die Anwendungen ausführen.

1. Führen Sie an einer Befehlszeile im Ordner „read-d2c“ den folgenden Befehl aus, um mit der Überwachung Ihres IoT Hubs zu beginnen:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![][7]

2. Führen Sie an einer Befehlszeile im Ordner „simulated-device“ den folgenden Befehl aus, um mit dem Senden von Telemetriedaten an Ihren IoT Hub zu beginnen:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![][8]

## Nächste Schritte

In diesem Tutorial haben Sie im Portal einen neuen IoT Hub konfiguriert und anschließend in der Identitätsregistrierung des Hubs eine Geräteidentität erstellt. Sie haben diese Geräteidentität in einem simulierten Gerät verwendet, das D2C-Nachrichten (Device-to-Cloud, Gerät-an-Cloud) an den Hub sendet, und Sie haben eine weitere App erstellt, die die vom Hub empfangenen Nachrichten anzeigt. In den folgenden Tutorials werden weitere Features und Szenarien für IoT Hubs vorgestellt:

- Unter [Senden von C2D-Nachrichten mit IoT Hub][lnk-c2d-tutorial] wird erläutert, wie Sie Nachrichten an Geräte senden und das von IoT Hub generierte Feedback zur Übermittlung verarbeiten.
- In [Verarbeiten von D2C-Nachrichten][lnk-process-d2c-tutorial] wird erläutert, wie Sie zuverlässig Telemetriedaten und interaktive Nachrichten von Geräten verarbeiten können.
- In [Hochladen von Dateien von Geräten][lnk-upload-tutorial] wird beschrieben, wie mithilfe von C2D-Nachrichten Dateiuploads von Geräten erleichtert werden.

<!-- Images. -->
[1]: ./media/iot-hub-java-java-getstarted/create-iot-hub1.png
[2]: ./media/iot-hub-java-java-getstarted/create-iot-hub2.png
[3]: ./media/iot-hub-java-java-getstarted/create-iot-hub3.png
[4]: ./media/iot-hub-java-java-getstarted/create-iot-hub4.png
[5]: ./media/iot-hub-java-java-getstarted/create-iot-hub5.png
[6]: ./media/iot-hub-java-java-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-java-java-getstarted/runapp1.png
[8]: ./media/iot-hub-java-java-getstarted/runapp2.png

<!-- Links -->
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/java/device/doc/devbox_setup.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-upload-tutorial]: iot-hub-csharp-csharp-file-upload.md

[lnk-hub-sdks]: iot-hub-sdks-summary.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-resource-groups]: resource-group-portal.md
[lnk-portal]: https://portal.azure.com/

<!---HONumber=AcomDC_0406_2016-->