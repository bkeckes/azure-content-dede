<properties
   pageTitle="Publish-WebApplicationVM | Microsoft Azure"
   description="Erfahren Sie, wie eine Webanwendung auf einem virtuellen Computer bereitgestellt wird. Dieses Skript erstellt die erforderlichen Ressourcen in Ihrem Azure-Abonnement, sofern noch nicht vorhanden."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="04/18/2016"
   ms.author="tarcher" />

# Publish-WebApplicationWebSite (Windows PowerShell-Skript)

Stellt eine Webanwendung auf einem virtuellen Computer bereit. Das Skript erstellt die erforderlichen Ressourcen in Ihrem Azure-Abonnement, wenn sie noch nicht vorhanden sind.

```
Publish-WebApplicationVM
–Configuration <configuration>
-SubscriptionName <subscriptionName>
-WebDeployPackage <packageName>
-VMPassword @{Name = "name"; Password = "password")
-DatabaseServerPassword @{Name = "name"; Password = "password"}
-SendHostMessagesToOutput
-Verbose
```

### Konfiguration

Der Pfad zur JSON-Konfigurationsdatei, in der die Details der Bereitstellung beschrieben sind.

|Aliase|(Keine)|
|---|---|
|Erforderlich|true|
|Position|benannt|
|Standardwert|(Keine)|
|Pipelineeingabe akzeptieren?|false|
|Platzhalterzeichen akzeptieren?|false|

### SubscriptionName

Der Name des Azure-Abonnements, in dem Sie den virtuellen Computer erstellen möchten.

|Aliase|(Keine)|
|---|---|
|Erforderlich|false|
|Position|benannt|
|Standardwert|Verwendet das erste Abonnement in der Abonnementdatei.|
|Pipelineeingabe akzeptieren?|false|
|Platzhalterzeichen akzeptieren?|false|

### WebDeployPackage

Der Pfad zum Webbereitstellungspaket für die Veröffentlichung auf dem virtuellen Computer. Sie können dieses Paket in Visual Studio mithilfe des Assistenten "Web veröffentlichen" erstellen. Weitere Informationen finden Sie unter [Vorgehensweise: Erstellen eines Webbereitstellungspakets in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).

|Aliase|(Keine)|
|---|---|
|Erforderlich|false|
|Position|benannt|
|Standardwert|(Keine)|
|Pipelineeingabe akzeptieren?|false|
|Platzhalterzeichen akzeptieren?|false|

### AllowUntrusted

Bei „true“ können Zertifikate verwendet werden, die nicht von einer vertrauenswürdigen Zertifizierungsstelle signiert sind.

|Aliase|(Keine)|
|---|---|
|Erforderlich|false|
|Position|benannt|
|Standardwert|false|
|Pipelineeingabe akzeptieren?|false|
|Platzhalterzeichen akzeptieren?|false|

### VMPassword

Die Anmeldeinformationen für das Konto des virtuellen Computers. Beispiel: -VMPassword @{Name = "Admin"; Password = "Kennwort"}

|Aliase|(Keine)|
|---|---|
|Erforderlich|false|
|Position|benannt|
|Standardwert|(Keine)|
|Pipelineeingabe akzeptieren?|false|
|Platzhalterzeichen akzeptieren?|false|

### DatabaseServerPassword

Die Anmeldeinformationen für die SQL-Datenbank in Azure. Beispiel: -DatabaseServerPassword @{Name = "Admin"; Password = "Kennwort"}

|Aliase|(Keine)|
|---|---|
|Erforderlich|false|
|Position|benannt|
|Standardwert|(Keine)|
|Pipelineeingabe akzeptieren?|false|
|Platzhalterzeichen akzeptieren?|false|

### SendHostMessagesToOutput

Falls "true", werden Nachrichten vom Skript in den Ausgabedatenstrom ausgegeben.

|Aliase|(Keine)|
|---|---|
|Erforderlich|false|
|Position|benannt|
|Standardwert|false|
|Pipelineeingabe akzeptieren?|false|
|Platzhalterzeichen akzeptieren?|false|

## Hinweise

Eine ausführliche Erläuterung der Verwendung des Skripts zum Erstellen von Entwicklungs- und Testumgebungen finden Sie unter [Verwenden von Windows PowerShell-Skripts zum Veröffentlichen in Entwicklungs- und Testumgebungen](vs-azure-tools-publishing-using-powershell-scripts.md).

In der JSON-Konfigurationsdatei sind die Details angegeben, was bereitgestellt werden muss. Dazu zählen die Informationen, die Sie beim Erstellen des Projekts angegeben haben, z. B. der Name, die Affinitätsgruppe, das VHD-Image und die Größe des virtuellen Computers. Dazu zählen außerdem die Endpunkte auf dem virtuellen Computer, die Datenbanken, die bereitgestellt werden sollen, und Webbereitstellungsparameter. Der folgende Code zeigt ein Beispiel einer JSON-Konfigurationsdatei:

```
{
    "environmentSettings": {
        "cloudService": {
            "name": "myvmname",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myvmname",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201404.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [
                    {
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [
            {
                "connectionStringName": "",
                "databaseName": "",
                "serverName": "",
                "user": "",
                "password": ""
            }
        ],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

Sie können die JSON-Konfigurationsdatei bearbeiten, um die Elemente zu ändern, die bereitgestellt werden. Ein virtueller Computer und ein Clouddienst sind erforderlich, aber der Datenbankabschnitt ist optional.

<!---HONumber=AcomDC_0420_2016-->