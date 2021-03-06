<properties
	pageTitle="Ressourceneinschränkungen für Azure SQL-Datenbanken"
	description="Diese Seite beschreibt einige allgemeine Ressourceneinschränkungen für Azure SQL-Datenbanken."
	services="sql-database"
	documentationCenter="na"
	authors="carlrabeler"
	manager="jhubbard"
	editor="monicar" />


<tags
	ms.service="sql-database"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="data-management"
	ms.date="05/02/2016"
	ms.author="carlrab" />


# Ressourceneinschränkungen für Azure SQL-Datenbank

## Übersicht

Azure SQL-Datenbank verwaltet die für eine Datenbank verfügbaren Ressourcen mithilfe zweier verschiedener Mechanismen: **Ressourcenkontrolle** und **Durchsetzung von Grenzen**. Dieser Artikel beschreibt diese zwei Hauptbereiche der Ressourcenverwaltung.

## Ressourcenkontrolle
Eines der Entwurfsziele der Tarife Basic, Standard und Premium ist es, dass sich die Azure SQL-Datenbank so verhält, als ob sie auf einem eigenen Computer vollständig isoliert von anderen Datenbanken ausgeführt wird. Die Ressourcenkontrolle emuliert dieses Verhalten. Wenn die aggregierte Ressourcenverwendung die maximal verfügbaren Ressourcen an CPU, Arbeitsspeicher, Protokoll-E/A und Daten-E/A erreicht, die der Datenbank zugeordnet sind, reiht die Ressourcenkontrolle die Ausführung von Abfragen in eine Warteschlange ein und ordnet den Abfragen in der Warteschlange Ressourcen zu, während sie nach und nach freigegeben werden.

Wie bei einem dedizierten Computer führt das Nutzen aller verfügbarer Ressourcen zu einer längeren Ausführung der derzeit ausgeführten Abfragen, was Befehlstimeouts auf dem Client zur Folge haben kann. Anwendungen mit aggressiver Wiederholungslogik und solche, die Abfragen für die Datenbank mit hoher Frequenz ausführen, können bei dem Versuch auf Fehlermeldungen stoßen, neue Abfragen auszuführen, wenn das Limit gleichzeitiger Anforderungen bereits erreicht wurde.

### Empfehlungen:
Überwachen Sie die Ressourcenverwendung sowie die durchschnittlichen Antwortzeiten für Abfragen, wenn die maximale Auslastung einer Datenbank fast erreicht wurde. Falls längere Abfragewartezeiten auftreten, haben Sie in der Regel drei Optionen:

1.	Reduzieren Sie die Menge der eingehenden Anfragen in der Datenbank, um einen Timeout und das Anhäufen von Anfragen zu verhindern.

2.	Weisen Sie der Datenbank eine höhere Leistungsstufe zu.

3.	Optimieren Sie Abfragen, um die Ressourcenverwendung für jede Abfrage zu reduzieren. Weitere Informationen finden Sie im Abschnitt "Abfrageoptimierung/Abfragehinweise" im Artikel "Leitfaden zur Azure SQL-Datenbankleistung".

## Durchsetzung von Grenzwerten
Durch das Verweigern neuer Anforderungen bei Erreichen der Limits werden andere Ressourcen als CPU, Arbeitsspeicher, Protokoll-E/A und Daten-E/A durchgesetzt. Clients erhalten abhängig von der erreichten Grenze eine [Fehlermeldung](sql-database-develop-error-messages.md).

Z. B. wird die Anzahl der Verbindungen mit einer SQL-Datenbank sowie die Anzahl der gleichzeitigen Anforderungen, die verarbeitet werden können, beschränkt. Mit einer SQL-Datenbank kann die Anzahl der Verbindungen mit der Datenbank größer als die Anzahl der gleichzeitigen Anforderungen sein, um Verbindungspooling zu unterstützen. Während die Anzahl der verfügbaren Verbindungen einfach von der Anwendung gesteuert werden kann, ist die Anzahl paralleler Anforderungen oft schwieriger zu schätzen und zu steuern. Insbesondere bei Spitzenbelastungen können Fehler auftreten, wenn die Anwendung entweder zu viele Anforderungen sendet oder die Datenbank seine Ressourcengrenzen erreicht und anfängt, Workerthreads aufgrund einer längeren Ausführungszeit von Abfragen anzuhäufen.

## Tarife und Leistungsebenen

Für eine einzelne Datenbank sind deren Einschränkungen durch die Dienstebene und Leistungsstufe der Datenbank definiert. In der folgenden Tabelle sind die Merkmale von Basic-, Standard- und Premium-Datenbanken für unterschiedliche Leistungsstufen beschrieben.

[AZURE.INCLUDE [Tarife für SQL-Datenbank](../../includes/sql-database-service-tiers-table.md)]

[Elastische Datenbankpools](sql-database-elastic-pool.md) nutzen Ressourcen gemeinsam für die Datenbanken im Pool. In der folgenden Tabelle sind die Merkmale von elastischen Basic-, Standard- und Premium-Datenbankpools beschrieben.

[AZURE.INCLUDE [Tabelle der SQL-Datenbank-Dienstebenen für elastische Datenbanken](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Eine erweiterte Definition der einzelnen Ressourcen, die in den vorangehenden Tabellen aufgeführt werden, können Sie der Beschreibungen in [Funktionen und Grenzen der Serviceebenen](sql-database-performance-guidance.md#service-tier-capabilities-and-limits) entnehmen. Eine Übersicht über die Diensttarife finden Sie unter [Dienstebenen und Leistungsstufen für Azure SQL-Datenbank](sql-database-service-tiers.md).

## Weitere Einschränkungen für SQL-Datenbanken

| Bereich | Begrenzung | Beschreibung |
|---|---|---|
| Datenbanken mit automatisiertem Export pro Abonnement | 10 | Automatisierter Export ermöglicht es Ihnen, einen benutzerdefinierten Zeitplan für die Sicherung Ihrer SQL-Datenbanken zu erstellen. Weitere Informationen finden Sie unter [SQL Databases: Support for Automated SQL Database Exports](http://weblogs.asp.net/scottgu/windows-azure-july-updates-sql-database-traffic-manager-autoscale-virtual-machines).|
| Datenbanken pro Server | Bis zu 5.000 | Bis zu 5.000 Datenbanken pro Server sind auf V12-Servern zulässig. In der Praxis gelten möglicherweise niedrigere Grenzwerte, abhängig von der Anmeldeaktivität in allen Datenbanken auf dem Server und der Abfragenutzung von Systemsichten in der Masterdatenbank. Die Kunden sollten die Datenbankverbindungen auf Probleme überwachen, wenn die Anzahl der Datenbanken auf einem Server deutlich steigt. |  
| DTUs pro Server. | 45000 | 45\.000 DTUs pro Server sind auf V12-Servern für die Bereitstellung von Datenbanken, elastischen Pools und Data Warehouses verfügbar. |



## Ressourcen

[Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md)

[Dienst- und Leistungsebenen für Azure SQL-Datenbanken](sql-database-service-tiers.md)

[Fehlermeldungen für Clientprogramme der SQL-Datenbank](sql-database-develop-error-messages.md)

<!---HONumber=AcomDC_0504_2016-->