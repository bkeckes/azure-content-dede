<properties 
   pageTitle="Grundlegende Befehle der Azure-Befehlszeilenschnittstelle für Linux und Mac | Microsoft Azure"
   description="Grundlegende Befehle der Azure-Befehlszeilenschnittstelle, um Ihnen beim Verwalten Ihrer virtuellen Computer im Azure Resource Manager-Modus mit Linux und Mac zu helfen"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="RicksterCDN" 
   manager="timlt" 
   editor="tysonn" 
   tags="azure-resource-manager"/>
   
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="04/11/2016"
   ms.author="rclaus" />

# Allgemeine Befehle der Azure-Befehlszeilenschnittstelle für Linux und Mac

Bevor Sie die Azure-Befehlszeilenschnittstelle mit Resource Manager-Befehlen und -Vorlagen zur Bereitstellung von Azure-Ressourcen und -Workloads über Ressourcengruppen verwenden können, benötigen Sie ein Azure-Konto. Wenn Sie noch kein Konto haben, erhalten Sie [hier eine kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

Wenn Sie die Azure-Befehlszeilenschnittstelle noch nicht installiert und mit Ihrem Abonnement verbunden haben, finden Sie unter [Installieren der Azure-Befehlszeilenschnittstelle ](../xplat-cli-install.md) weitere Informationen. Legen Sie den Modus mit `azure config mode arm` auf `arm` fest, und stellen Sie mit dem Befehl `azure login` eine Verbindung mit Azure her.

## Grundlegende Azure Resource Manager-Befehle in der Azure-Befehlszeilenschnittstelle

Dieser Artikel behandelt die grundlegenden Befehle, die Sie mit der Azure-Befehlszeilenschnittstelle verwenden, um Ihre ARM-Ressourcen (hauptsächlich virtuelle Computer) im Azure-Abonnement zu verwalten und mit ihnen zu interagieren. Ausführlichere Hilfe zu bestimmten Befehlszeilenschaltern und -optionen finden Sie in der Onlinehilfe zu Befehlen und Optionen, indem Sie `azure <command> <subcommand> --help` oder `azure help <command> <subcommand>` eingeben.

> [AZURE.NOTE] Diese Beispiele umfassen keine vorlagenbasierten Vorgänge, die im Allgemeinen für VM-Bereitstellungen im Ressourcen-Manager empfohlen werden. Informationen finden Sie unter [Verwenden der Azure-Befehlszeilenschnittstelle mit Azure-Ressourcen-Manager](../xplat-cli-azure-resource-manager.md) und [Bereitstellen und Verwalten von virtuellen Computern mit Azure-Ressourcen-Manager-Vorlagen und der Azure-Befehlszeilenschnittstelle](virtual-machines-linux-cli-deploy-templates.md).

Aufgabe | Ressourcen-Manager
-------------- | ----------- | -------------------------
Erstellen grundlegender virtueller Computer | `azure vm quick-create [options] <resource-group> <name> <location> <os-type> <image-urn> <admin-username> <admin-password>`<br/><br/>(Rufen Sie `image-urn` aus dem Befehl `azure vm image list` ab. Nähere Einzelheiten finden Sie [in diesem Artikel](virtual-machines-linux-cli-ps-findimage.md).)
Erstellen eines virtuellen Linux-Computers | `azure  vm create [options] <resource-group> <name> <location> -y "Linux"`
Erstellen eines virtuellen Windows-Computers | `azure  vm create [options] <resource-group> <name> <location> -y "Windows"`
Auflisten der virtuellen Computer | `azure  vm list [options]`
Abrufen von Informationen zu einem virtuellen Computer | `azure  vm show [options] <resource_group> <name>`
Starten eines virtuellen Computers | `azure vm start [options] <resource_group> <name>`
Anhalten eines virtuellen Computers | `azure vm stop [options] <resource_group> <name>`
Aufheben der Zuordnung eines virtuellen Computers | `azure vm deallocate [options] <resource-group> <name>`
Neustarten eines virtuellen Computers | `azure vm restart [options] <resource_group> <name>`
Löschen eines virtuellen Computers | `azure vm delete [options] <resource_group> <name>`
Erfassen eines virtuellen Computers | `azure vm capture [options] <resource_group> <name>`
Erstellen eines virtuellen Computers aus einem Benutzer-Image | `azure  vm create [options] –q <image-name> <resource-group> <name> <location> <os-type>`
Erstellen eines virtuellen Computers von einem speziellen Datenträger | `azue  vm create [options] –d <os-disk-vhd> <resource-group> <name> <location> <os-type>`
Hinzufügen eines Datenträgers zu einem virtuellen Computer | `azure  vm disk attach-new [options] <resource-group> <vm-name> <size-in-gb> [vhd-name]`
Entfernen eines Datenträgers aus einem virtuellen Computer | `azure  vm disk detach [options] <resource-group> <vm-name> <lun>`
Hinzufügen einer generischen Erweiterung zu einem virtuellen Computer |`azure  vm extension set [options] <resource-group> <vm-name> <name> <publisher-name> <version>`
Hinzufügen einer VMAccess-Erweiterung zu einem virtuellen Computer | `azure vm reset-access [options] <resource-group> <name>`
Hinzufügen einer Docker-Erweiterung zu einem virtuellen Computer | `azure  vm docker create [options] <resource-group> <name> <location> <os-type>`
Entfernen einer VM-Erweiterung | `azure  vm extension set [options] –u <resource-group> <vm-name> <name> <publisher-name> <version>`
Abrufen der Nutzung virtueller Computerressourcen | `azure vm list-usage [options] <location>`
Abrufen Sie aller verfügbaren Größen von virtuellen Computern | `azure vm sizes [options]`


## Nächste Schritte

* Weitere Beispiele für die Befehle der Befehlszeilenschnittstelle, die über die grundlegende Verwaltung virtueller Computer hinausgehen, finden Sie unter [Azure-CLI-Befehle im Azure Resource Manager-Modus (ARM-Modus)](azure-cli-arm-commands.md).

<!---HONumber=AcomDC_0504_2016-->