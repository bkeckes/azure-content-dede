<properties
   pageTitle="Anwendungs- oder benutzerspezifischer Marathon-Dienst | Microsoft Azure"
   description="Anwendungs- oder benutzerspezifischen Marathon-Dienst erstellen"
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Container, Marathon, Microservices, DC/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/12/2016"
   ms.author="rogardle"/>

# Erstellen eines anwendungs- oder benutzerspezifischen Marathon-Diensts

Für Azure Container Service wird eine Gruppe von Masterservern bereitgestellt, auf denen wir Apache Mesos und Marathon vorkonfigurieren. Diese Komponenten können zum Orchestrieren Ihrer Anwendungen im Cluster verwendet werden, aber es ist ratsam, die Master nicht für diese Zwecke zu nutzen. Zum Optimieren der Konfiguration von Marathon ist beispielsweise das Anmelden an den Mastern selbst und das Vornehmen von Änderungen erforderlich. Dies führt zu speziellen Masterservern, die sich vom Standard leicht unterscheiden und unabhängig gepflegt und verwaltet werden müssen. Außerdem ist die Konfiguration, die für ein Team erforderlich ist, unter Umständen nicht die optimale Konfiguration für ein anderes Team. In diesem Artikel wird beschrieben, wie Sie einen benutzer- oder anwendungsspezifischen Marathon-Dienst hinzufügen.

Da sich dieser Dienst im Besitz eines einzelnen Benutzers oder Teams befindet, kann er wie gewünscht konfiguriert werden. Außerdem wird mit Azure Container Service sichergestellt, dass der Dienst weiter ausgeführt wird. Wenn der Dienst ausfällt, wird er von Azure Container Service für Sie neu gestartet. Meistens merken Sie gar nicht, dass es zu einem Ausfall gekommen ist.

## Voraussetzungen

[Stellen Sie eine Instanz von Azure Container Service mit dem Orchestratortyp DCOS bereit](container-service-deployment.md), [stellen Sie sicher, dass der Client eine Verbindung mit Ihrem Cluster herstellen kann](container-service-connect.md), und[AZURE.INCLUDE [installieren Sie die DC/OS-CLI](../../includes/container-service-install-dcos-cli-include.md)].

## Erstellen Sie einen anwendungs- oder benutzerspezifischen Marathon-Dienst.

Beginnen Sie, indem Sie eine JSON-Konfigurationsdatei erstellen, mit der der Name des zu erstellenden Anwendungsdiensts definiert wird. Hier verwenden wir `marathon-alice` als Name für das Framework. Speichern Sie die Datei unter einem Namen wie `marathon-alice.json`:

```json
{"marathon": {"framework-name": "marathon-alice" }}
```

Verwenden Sie als Nächstes die DC/OS-CLI, um die Marathon-Instanz mit den Optionen zu installieren, die in Ihrer Konfigurationsdatei festgelegt sind:

```bash
dcos package install --options=marathon-alice.json marathon
```

Sie sollten jetzt auf der Registerkarte „Dienste“ der DC/OS-Benutzeroberfläche sehen, dass Ihr `marathon-alice`-Dienst ausgeführt wird. Auf die Benutzeroberfläche können Sie unter `http://<hostname>/service/marathon-alice/` direkt zugreifen.

## Einrichten der DC/OS-CLI für den Zugriff auf den Dienst

Sie können Ihre DC/OS-CLI optional konfigurieren, um auf diesen neuen Dienst zuzugreifen, indem Sie die `marathon.url`-Eigenschaft wie folgt festlegen, damit sie auf die `marathon-alice`-Instanz zeigt:

```bash
dcos config set marathon.url http://<hostname>/service/marathon-alice/
```

Sie können mit dem Befehl `dcos config show` überprüfen, für welche Instanz von Marathon die CLI verwendet wird. Sie können den Befehl `dcos config unset marathon.url` nutzen, um wieder zur Masterinstanz des Marathon-Diensts zurückzuwechseln.

<!---HONumber=AcomDC_0525_2016-->