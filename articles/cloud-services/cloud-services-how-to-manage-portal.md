<properties 
	pageTitle="Allgemeine Verwaltungsaufgaben für Clouddienste | Microsoft Azure" 
	description="Hier erfahren Sie mehr über die Verwaltung von Clouddiensten im Azure-Portal. In diesen Beispielen wird das Azure-Portal verwendet." 
	services="cloud-services" 
	documentationCenter="" 
	authors="Thraka" 
	manager="timlt" 
	editor=""/>

<tags 
	ms.service="cloud-services" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="04/26/2016"
	ms.author="adegeo"/>


# Verwalten von Clouddiensten

> [AZURE.SELECTOR]
- [Azure-Portal](cloud-services-how-to-manage-portal.md)
- [Klassisches Azure-Portal](cloud-services-how-to-manage.md)


Im Bereich **Cloud Services** des Azure-Portals können Sie eine Dienstrolle oder eine Bereitstellung aktualisieren, eine Bereitstellung zur Produktion heraufstufen, Ressourcen mit Ihrem Clouddienst verknüpfen, sodass Sie die Ressourcenabhängigkeiten sehen und die Ressourcen zusammen skalieren können, und einen Clouddienst oder eine Bereitstellung löschen.


## Aktualisieren einer Clouddienstrolle oder -bereitstellung

Wenn Sie den Anwendungscode für den Clouddienst aktualisieren müssen, verwenden Sie die Option **Aktualisieren** auf dem Blatt für Clouddienste. Sie können eine oder alle Rollen aktualisieren. Sie müssen ein neues Dienstpaket und eine neue Dienstkonfigurationsdatei hochladen.

1. Wählen Sie im [Azure-Portal][] den Clouddienst aus, den Sie aktualisieren möchten. Dadurch wird das Blatt für die Clouddienstinstanz geöffnet.

2. Klicken Sie auf dem Blatt auf die Schaltfläche **Aktualisieren**.

    ![Schaltfläche „Aktualisieren“](./media/cloud-services-how-to-manage-portal/update-button.png)

3. Aktualisieren Sie die Bereitstellung mit einer neuen Dienstpaketdatei (.cspkg) und Dienstkonfigurationsdatei (.cscfg).

    ![Bereitstellung aktualisieren](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. **Optional** können Sie die Bereitstellungsbezeichnung und das Speicherkonto aktualisieren.

5. Wenn Dienstrollen nur eine Rolleninstanz haben, aktivieren Sie das Kontrollkästchen **Auch dann bereitstellen, wenn für mindestens eine Rolle nur eine Instanz vorhanden ist**, damit das Update fortgesetzt werden kann.

	Azure kann während des Updates eines Clouddiensts nur dann eine Dienstverfügbarkeit von 99,95 Prozent garantieren, wenn jede Rolle mindestens zwei Rolleninstanzen (virtuelle Computer) hat. In diesem Fall kann ein virtueller Computer Clientanforderungen verarbeiten, während der andere aktualisiert wird.

6. Aktivieren Sie **Bereitstellung starten**, wenn Sie festlegen möchten, dass das Update angewendet wird, nachdem der Upload des Pakets beendet wurde.

7. Klicken Sie auf **OK**, um mit der Aktualisierung des Diensts zu beginnen.



## Austauschen von Bereitstellungen zum Heraufstufen einer Bereitstellung zur Produktion

Verwenden Sie **Austauschen**, um eine Stagingbereitstellung eines Cloud-Diensts zur Produktion heraufzustufen. Wenn Sie beschließen, eine neue Version eines Cloud-Diensts bereitzustellen, können Sie die neue Version in der Cloud-Dienst-Stagingumgebung bereitstellen und testen, während Ihre Kunden die aktuelle Version in der Produktion einsetzen. Wenn Sie bereit sind, die neue Version zur Produktion heraufzustufen, können Sie mit **Austauschen** die URLs austauschen, mit denen die beiden Bereitstellungen adressiert werden.

Sie können Bereitstellungen über die Seite **Cloud-Dienste** oder über das Dashboard austauschen.

1. Wählen Sie im [Azure-Portal][] den Clouddienst aus, den Sie aktualisieren möchten. Dadurch wird das Blatt für die Clouddienstinstanz geöffnet.

2. Klicken Sie auf dem Blatt auf die Schaltfläche **Austauschen**.

    ![Austauschen von Cloud-Diensten](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. Die folgende Bestätigungsaufforderung wird geöffnet.

	![Austauschen von Cloud-Diensten](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. Klicken Sie nach der Verifizierung der Bereitstellungsinformationen auf **OK**, um die Bereitstellungen auszutauschen.

	Der Austausch der Bereitstellungen erfolgt schnell, da sich nur die virtuellen IP-Adressen für die Bereitstellungen ändern.

	Um Rechenkosten zu sparen, können Sie die Bereitstellung in der Stagingumgebung löschen, wenn Sie sicher sind, dass die neue Produktionsbereitstellung wie erwartet funktioniert.

## Verknüpfen einer Ressource mit einem Clouddienst

Das Azure-Portal verknüpft Ressourcen nicht miteinander wie das aktuelle klassische Azure-Portal. Stattdessen müssen Sie zusätzliche Ressourcen in der gleichen Ressourcengruppe bereitstellen, die auch vom Clouddienst verwendet wird.

## Löschen von Bereitstellungen und eines Clouddiensts

Bevor Sie einen Cloud-Dienst löschen können, müssen Sie die einzelnen bestehenden Bereitstellungen löschen.

Um Rechenkosten zu sparen, können Sie Ihre Stagingbereitstellung löschen, nachdem Sie verifiziert haben, dass die Produktionsbereitstellung wie erwartet funktioniert. Rechenkosten für Rolleninstanzen fallen auch dann an, wenn ein Cloud-Dienst nicht ausgeführt wird.

Gehen Sie folgendermaßen vor, um eine Bereitstellung oder Ihren Cloud-Dienst zu löschen.

1. Wählen Sie im [Azure-Portal][] den Clouddienst aus, den Sie löschen möchten. Dadurch wird das Blatt für die Clouddienstinstanz geöffnet.

2. Klicken Sie auf dem Blatt auf die Schaltfläche **Löschen**.

    ![Austauschen von Cloud-Diensten](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. Sie können den gesamten Clouddienst löschen, indem Sie **Clouddienst und seine Bereitstellungen** aktivieren, oder wählen Sie entweder **Produktionsbereitstellung** oder **Stagingbereitstellung** aus.

    ![Austauschen von Cloud-Diensten](./media/cloud-services-how-to-manage-portal/delete-blade.png)

4. Klicken Sie unten auf die Schaltfläche **Löschen**.

5. Klicken Sie zum Löschen des Cloud-Diensts auf **Cloud-Dienst löschen**. Klicken Sie dann an der Bestätigungsaufforderung auf **Ja**.

> [AZURE.NOTE]
Wenn für den Cloud-Dienst die ausführliche Überwachung konfiguriert ist, löscht Azure die Überwachungsdaten aus Ihrem Speicherkonto nicht, wenn Sie den Cloud-Dienst löschen. Sie müssen die Daten manuell löschen. Informationen zum Speicherort der Metriktabellen finden Sie in [diesem](cloud-services-how-to-monitor.md) Artikel.

[Azure-Portal]: https://portal.azure.com

## Nächste Schritte

* [Allgemeine Konfiguration Ihres Clouddiensts](cloud-services-how-to-configure-portal.md)
* Weitere Informationen zum [Bereitstellen eines Clouddiensts](cloud-services-how-to-create-deploy-portal.md)
* [Konfigurieren eines benutzerdefinierten Domänennamens](cloud-services-custom-domain-name-portal.md)
* Konfigurieren von [SSL-Zertifikaten](cloud-services-configure-ssl-certificate-portal.md)

<!---HONumber=AcomDC_0511_2016-->