<properties
	pageTitle="Erstellen eines Labs in DevTest Labs | Microsoft Azure"
	description="Erstellen eines neuen Labs in DevTest Labs für virtuelle Computer"
	services="devtest-lab,virtual-machines"
	documentationCenter="na"
	authors="tomarcher"
	manager="douge"
	editor=""/>

<tags
	ms.service="devtest-lab"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="05/08/2016"
	ms.author="tarcher"/>

# Erstellen eines Labs in Azure DevTest Labs

## Voraussetzungen

Zum Erstellen eines Labs benötigen Sie Folgendes:

- Ein Azure-Abonnement. Weitere Informationen zu den Azure-Kaufoptionen finden Sie unter [Azure erwerben](https://azure.microsoft.com/pricing/purchase-options/) oder [Kostenlose Testversion (1 Monat)](https://azure.microsoft.com/pricing/free-trial/). Zum Erstellen des Labs müssen Sie der Besitzer des Abonnements sein.
- Eine Azure-Ressourcengruppe für das Lab. Weitere Informationen finden Sie unter [Übersicht über den Azure-Ressourcen-Manager](../resource-group-overview.md) und [Rollenbasierte Access Control in Azure](../active-directory/role-based-access-control-configure.md).

## Erstellen eines Labs

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) an.

1. Wählen Sie **Durchsuchen**.

1. Wählen Sie in der Liste **DevTest Labs** aus.

1. Tippen Sie auf dem Blatt **DevTest Labs** auf **Hinzufügen**.

    ![Hinzufügen eines Labs](./media/devtest-lab-create-lab/add-lab-button.png)

1. Führen Sie auf dem Blatt **DevTest Lab erstellen** folgende Schritte aus:

    1. Geben Sie einen **Labnamen** für das neue Lab ein.
    1. Wählen Sie das **Abonnement** aus, das mit dem Lab verknüpft werden soll.
    1. Wählen Sie einen **Speicherort** für das Lab aus.
    1. Tippen Sie auf **Erstellen**.

    ![Erstellen eines Lab-Blatts](./media/devtest-lab-create-lab/create-devtestlab-blade.png)

## Nächste Schritte

Nachdem das Lab erstellt wurde, sollten Sie ggf. die folgenden Schritte ausführen:

- [Sicherer Zugriff auf ein Lab](devtest-lab-add-devtest-user.md)

- [Festlegen von Labrichtlinien](devtest-lab-set-lab-policy.md).

- [Erstellen einer Labvorlage](devtest-lab-create-template.md).

- [Erstellen benutzerdefinierter Artefakte für Ihre VMs](devtest-lab-artifact-author.md).

- [Hinzufügen einer VM mit Artefakten zu einem Lab](devtest-lab-add-vm-with-artifacts.md)

<!---HONumber=AcomDC_0511_2016-->