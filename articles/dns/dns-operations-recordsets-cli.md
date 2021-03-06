<properties 
   pageTitle="Verwalten von DNS-Datensatzgruppen und Einträgen in Azure DNS mithilfe der Azure-Befehlszeilenschnittstelle (CLI) | Microsoft Azure" 
   description="Verwalten von DNS-Datensatzgruppen und Einträgen in Azure DNS, wenn Sie Ihre Domäne in Azure DNS hosten. Alle Befehle der Befehlszeilenschnittstelle (CLI) für Vorgänge für Datensatzgruppen und Einträge." 
   services="dns" 
   documentationCenter="na" 
   authors="cherylmc" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="05/06/2016"
   ms.author="cherylmc"/>

# Verwalten von DNS-Ressourceneinträgen und -DNS-Ressourceneintragssätzen über die Befehlszeilenschnittstelle


> [AZURE.SELECTOR]
- [Azure-Portal](dns-operations-recordsets-portal.md)
- [Azure-Befehlszeilenschnittstelle](dns-operations-recordsets-cli.md)
- [PowerShell](dns-operations-recordsets.md)


In diesem Artikel wird gezeigt, wie Sie Ressourceneintragssätze und Einträge für die DNS-Zone mit der plattformübergreifenden Azure-Befehlszeilenschnittstelle (Command-Line Interface, CLI) verwalten.

Es ist wichtig, den Unterschied zwischen DNS-Ressourceneintragssätzen und einzelnen DNS-Ressourceneinträgen zu verstehen. Ein Ressourceneintragssatz ist die Auflistung von Einträgen in einer Zone, die den gleichen Namen tragen und den gleichen Typ aufweisen. Weitere Informationen finden Sie unter [Grundlegende Informationen zu Ressourceneinträgen und Ressourceneintragssätzen](dns-getstarted-create-recordset-cli.md).


## Azure DNS und plattformübergreifende Azure-CLI

Azure DNS ist ein nur über Azure-Ressourcen-Manager verfügbarer Dienst. Er besitzt keine ASM-API. Sie müssen sicherstellen, dass die Azure-CLI für den Resource Manager-Modus konfiguriert ist. Verwenden Sie dazu den Befehl `azure config mode arm`.<BR>Falls die Meldung: *„Fehler: ‚dns‘ ist kein Azure-Befehl“ angezeigt* wird, verwenden Sie wahrscheinlich die Azure-CLI im ASM-Modus und nicht im Resource Manager-Modus.

## Erstellen eines neuen Ressourceneintragssatzes und eines Eintrags

Weitere Informationen zum Erstellen eines Ressourceneintragssatzes im Azure-Portal finden Sie unter [Erstellen von DNS-Einträgen und -Ressourceneintragssätzen mit dem Azure-Portal](dns-getstarted-create-recordset-cli.md).


## Abrufen eines Ressourceneintragssatzes

Verwenden Sie `azure network dns record-set show` zum Abrufen eines vorhandenen Ressourceneintragssatzes. Geben Sie die Ressourcengruppe, den Zonennamen, den relativen Namen des Ressourceneintragssatzes und den Eintragstyp an. Ersetzen Sie anhand des folgenden Beispiels die Werte durch Ihre eigenen.

	azure network dns record-set show myresourcegroup contoso.com www A


## Auflisten von Ressourceneintragssätzen

Sie können alle Eintragssätze in einer DNS-Zone mit dem Befehl `azure network dns record-set list` auflisten: Sie müssen den Namen der Ressourcengruppe und der Zone angeben.

### So listen Sie alle Ressourceneintragssätze auf

Dieses Beispiel gibt alle Ressourceneintragssätze unabhängig vom Namen oder Eintragstyp zurück:

	azure network dns record-set list myresourcegroup contoso.com

### So listen Sie Ressourceneintragssätze eines bestimmten Typs auf

Dadurch werden alle Ressourceneintragssätze zurückgegeben, die dem angegebenen Eintragstyp (in diesem Fall A-Einträge) entsprechen:

	azure network dns record-set list myresourcegroup contoso.com A 


## Hinzufügen eines Eintrags zu einer Datensatzgruppe

Datensätze werden mithilfe von `azure network dns record-set add-record` zu Datensatzgruppen hinzugefügt. Die Parameter zum Hinzufügen von Einträgen zu einer Datensatzgruppe variieren je nach Typ der Datensatzgruppe. Wenn Sie beispielsweise einen Ressourceneintragssatz des Typs *A* verwenden, können Sie nur Einträge mit dem Parameter `-a <IPv4 address>` angeben.

Verwenden Sie `azure network dns record-set create` zum Erstellen eines Ressourceneintragssatzes. Geben Sie die Ressourcengruppe, den Zonennamen, den relativen Namen des Ressourceneintragssatzes, den Eintragstyp und die Gültigkeitsdauer (TTL) an. Wenn der Parameter „--ttl“ nicht definiert ist, liegt der Standardwert bei 4 (in Sekunden).
	
	azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300


Fügen Sie nach Erstellen des A-Ressourceneintragssatzes die IPv4-Adresse mithilfe von `azure network dns record-set add-record` hinzu:

	azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1 


Die folgenden Beispiele zeigen, wie Sie eine Datensatzgruppe jedes Eintragstyps mit einem einzelnen Eintrag erstellen.

[AZURE.INCLUDE [dns-add-record-cli-include](../../includes/dns-add-record-cli-include.md)]


## Aktualisieren eines Eintrags in einem vorhandenen Ressourceneintragssatz

### So fügen Sie eine weitere IP-Adresse (1.2.3.4) einem vorhandenen A-Ressourceneintragssatz (www) hinzu: 

	azure network dns record-set add-record  myresourcegroup contoso.com  A
	-a 1.2.3.4
	info:    Executing command network dns record-set add-record
	Record set name: www
	+ Looking up the dns zone "contoso.com"
	+ Looking up the DNS record set "www"
	+ Updating DNS record set "www"
	data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/a/www
	data:    Name                            : www	
	data:    Type                            : Microsoft.Network/dnszones/a
	data:    Location                        : global
	data:    TTL                             : 4
	data:    A records:
	data:        IPv4 address                : 192.168.1.1
	data:        IPv4 address                : 1.2.3.4
	data:
	info:    network dns record-set add-record command OK

### Verwenden Sie `azure network dns record-set delete-record`, um einen vorhandenen Wert aus einem Ressourceneintragssatz zu entfernen.
 
	azure network dns record-set delete-record myresourcegroup contoso.com www A -a 1.2.3.4
	info:    Executing command network dns record-set delete-record
	+ Looking up the DNS record set "www"
	Delete DNS record? [y/n] y
	+ Updating DNS record set "www"
	data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/A/www
	data:    Name                            : www
	data:    Type                            : Microsoft.Network/dnszones/A
	data:    Location                        : global
	data:    TTL                             : 4
	data:    A records:
	data:    IPv4 address                    : 192.168.1.1
	data:
	info:    network dns record-set delete-record command OK



## Entfernen eines Eintrags aus einem Ressourceneintragssatz

Einträge können mithilfe von `azure network dns record-set delete-record` aus einem Ressourceneintragssatz entfernt werden. Der entfernte Eintrag muss bei allen Parametern exakt mit einem vorhandenen Eintrag übereinstimmen.

Durch Entfernen des letzten Eintrags aus einem Ressourceneintragssatz wird der Ressourceneintragssatz nicht gelöscht. Im Abschnitt [Löschen eines Ressourceneintragssatzes](#delete) in diesem Artikel finden Sie weitere Informationen.

	azure network dns record-set delete-record myresourcegroup contoso.com www A -a 192.168.1.1

	azure network dns record-set delete myresourcegroup contoso.com www A

### Entfernen eines AAAA-Eintrags aus einem Ressourceneintragssatz

	azure network dns record-set delete-record myresourcegroup contoso.com test-aaaa  AAAA -b "2607:f8b0:4009:1803::1005"

### Entfernen eines CNAME-Eintrags aus einem Ressourceneintragssatz

	azure network dns record-set delete-record myresourcegroup contoso.com test-cname CNAME -c www.contoso.com
	

### Entfernen eines MX-Eintrags aus einem Ressourceneintragssatz

	azure network dns record-set delete-record myresourcegroup contoso.com "@" MX -e "mail.contoso.com" -f 5

### Entfernen eines NS-Eintrags aus einem Ressourceneintragssatz
	
	azure network dns record-set delete-record myresourcegroup contoso.com  "test-ns" NS -d "ns1.contoso.com"

### Entfernen eines SRV-Eintrags aus einem Ressourceneintragssatz

	azure network dns record-set delete-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 -w 5 -o 8080 -u "sip.contoso.com" 

### Entfernen eines TXT-Eintrags aus einem Ressourceneintragssatz

	azure network dns record-set delete-record myresourcegroup contoso.com  "test-TXT" TXT -x "this is a TXT record"

## <a name="delete"></a>Löschen eines Ressourceneintragssatzes

Ressourceneintragssätze können mit dem Cmdlet `Remove-AzureRmDnsRecordSet` gelöscht werden. Sie können nicht die SOA- und NS-Ressourceneintragssätze an der Zonenspitze (Name = „@“) löschen, die beim Erstellen der Zone automatisch erstellt werden. Sie werden automatisch gelöscht, wenn die Zone gelöscht wird.

Im folgenden Beispiel wird die A-Datensatzgruppe mit Namen "Test-a" aus der DNS-Zone von "contoso.com" entfernt:

	azure network dns record-set delete myresourcegroup contoso.com  "test-a" A 

Mit dem optionalen Switch *-q* kann diese Bestätigungsaufforderung unterdrückt werden.


## Nächste Schritte

Weitere Informationen zu Azure DNS finden Sie unter [Azure DNS – Übersicht](dns-overview.md). Weitere Informationen zum Automatisieren von DNS finden Sie unter [Erstellen von DNS-Zonen und -Ressourceneintragssätzen mithilfe des .NET SDK](dns-sdk.md).

Wenn Sie mit Reverse-DNS-Einträgen arbeiten möchten, siehe [Verwalten von Reverse-DNS-Einträgen](dns-reverse-dns-record-operations-cli.md).




 

<!---HONumber=AcomDC_0518_2016-->