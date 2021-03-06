<properties
		pageTitle="Hinzufügen eines Benutzers zu einer Linux-VM in Azure | Microsoft Azure"
		description="Es wird beschrieben, wie Sie einen Benutzer in Azure einer Linux-VM hinzufügen."
		services="virtual-machines-linux"
		documentationCenter=""
		authors="vlivech"
		manager="timlt"
		editor=""
		tags="azure-resource-manager"
/>

<tags
		ms.service="virtual-machines-linux"
		ms.workload="infrastructure-services"
		ms.tgt_pltfrm="vm-linux"
		ms.devlang="na"
		ms.topic="article"
		ms.date="03/04/2016"
		ms.author="v-livech"
/>

# Hinzufügen eines Benutzers zu einer Azure-VM

Bei jeder neu gestarteten Linux-VM ist eine der ersten Aufgaben die Erstellung eines neuen Benutzers. In diesem Artikel sind die Schritte für das Erstellen eines sudo-Benutzerkontos, das Festlegen des Kennworts, das Hinzufügen von öffentlichen SSH-Schlüsseln und das Verwenden von `visudo` für die Nutzung von sudo ohne Kennwort ausführlich beschrieben.

Voraussetzungen: [Ein Azure-Konto](https://azure.microsoft.com/pricing/free-trial/), [öffentliche und private SSH-Schlüssel](virtual-machines-linux-mac-create-ssh-keys.md), eine Azure-Ressourcengruppe und die Azure-Befehlszeilenschnittstelle, die installiert und mit `azure config mode arm` in den Azure Resource Manager-Modus geschaltet wurde.

## Schnellbefehle

```bash
# Add a new user on RedHat family distros
bill@slackware$ sudo useradd -G wheel exampleUser

# Add a new user on Debian family distros
bill@slackware$ sudo useradd -G sudo exampleUser

# Set a password
bill@slackware$ sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully

# Copy the SSH Public Key to the new user
bill@slackware$ ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver

# Change sudoers to allow no password
# Execute visudo as root to edit the /etc/sudoers file
bill@slackware$ visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# to this
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# to this
%sudo   ALL=(ALL) NOPASSWD:ALL

# Verify everything
# Verify the SSH keys & User account
bill@slackware$ ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
exampleuser@exampleserver$ sudo top
```

## Ausführliche exemplarische Vorgehensweise

### Einführung

Eine der ersten und häufigsten Aufgaben bei einem Server ist das Hinzufügen eines Benutzerkontos. Stammanmeldungen sollten immer deaktiviert werden, und auch das Stammkonto selbst sollte niemals mit dem Linux-Server verwendet werden, sondern nur sudo. Die richtige Vorgehensweise für die Verwaltung und Nutzung von Linux besteht darin, einen neuen Benutzer zu erstellen und diesem Benutzer mit sudo dann Berechtigungen für die Stammeskalation zu gewähren.

Hierfür verwenden wir den Befehl `useradd`, mit dem `/etc/passwd`, `/etc/shadow`, `/etc/group` und `/etc/gshadow` geändert werden, um den neuen Linux-Benutzer zu erstellen. Wir fügen dem Befehl `useradd` ein Befehlszeilenflag hinzu, um den neuen Benutzer auch der richtigen sudo-Gruppe unter Linux hinzuzufügen. Mit `useradd` wird zwar ein Eintrag unter `/etc/passwd` erstellt, aber für das neue Benutzerkonto wird kein Kennwort vergeben. Wir erstellen ein Anfangskennwort für den neuen Benutzer, indem wir den einfachen Befehl `passwd` verwenden. Im letzten Schritt ändern wir die sudo-Regeln, damit der Benutzer Befehle mit sudo-Berechtigungen ausführen kann, ohne für jeden Befehl ein Kennwort eingeben zu müssen. Da sich der Benutzer mit dem Schlüsselpaar aus öffentlichem und privatem Schlüssel angemeldet hat, gehen wir davon aus, dass das Benutzerkonto vor Angreifern sicher ist, und lassen den sudo-Zugriff ohne Kennwort zu.

### Hinzufügen eines einzelnen sudo-Benutzers zu einer Azure-VM

Melden Sie sich mit SSH-Schlüsseln an der Azure-VM an. Falls Sie den Zugriff mit einem öffentlichen SSH-Schlüssel noch nicht eingerichtet haben, sollten Sie zuerst den Artikel [Using Public Key Authentication with Azure](http://link.to/article) (Verwenden der Authentifizierung mit öffentlichem Schlüssel für Azure) durcharbeiten.

Mit dem Befehl `useradd` werden die folgenden Aufgaben durchgeführt:

- Erstellen eines neuen Benutzerkontos
- Erstellen einer neuen Benutzergruppe mit dem gleichen Namen
- Einfügen eines leeren Eintrags in `/etc/passwd`
- Einfügen eines leeren Eintrags in `/etc/gpasswd`

Mit dem Befehlszeilenflag `-G` wird das neue Benutzerkonto der richtigen Linux-Gruppe hinzugefügt, und das Konto erhält Berechtigungen für die Stammeskalation.

#### Hinzufügen des Benutzers

```bash
# On RedHat family distros
bill@slackware$ sudo useradd -G wheel exampleUser

# On Debian family distros
bill@slackware$ sudo useradd -G sudo exampleUser
```

#### Festlegen eines Kennworts

Mit dem Befehl `useradd` wird der neue Benutzer erstellt und `/etc/passwd` und `/etc/gpasswd` jeweils ein Eintrag hinzugefügt, aber das Kennwort wird nicht festgelegt. Dies führen wir nun mit dem Befehl `passwd` durch.

```bash
bill@slackware$ sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

Wir verfügen jetzt über einen Benutzer mit sudo-Berechtigungen auf dem Server.

### Hinzufügen des öffentlichen SSH-Schlüssels zum neuen Benutzerkonto

Verwenden Sie auf Ihrem Computer den Befehl `ssh-copy-id` mit dem neuen Kennwort, das Sie gerade festgelegt haben.

```bash
bill@slackware$ ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver
```

### Verwenden von visudo, um die sudo-Nutzung ohne Kennwort zu ermöglichen

Beim Verwenden von `visudo` zum Bearbeiten der Datei `/etc/sudoers` werden einige Schutzebenen festgelegt, um zu verhindern, dass diese sehr wichtige Datei geändert wird. Wenn `visudo` ausgeführt wird, wird die Datei `/etc/sudoers` gesperrt. So wird sichergestellt, dass während der aktiven Bearbeitung keine anderen Personen Änderungen vornehmen können. Die Datei `/etc/sudoers` wird von `visudo` auch auf Fehler untersucht, wenn Sie versuchen, sie zu speichern oder zu beenden. Hiermit wird dafür gesorgt, dass Sie keine fehlerhafte sudoers-Datei im System zurücklassen können.

Es befinden sich bereits Benutzer in der richtigen Standardgruppe für den sudo-Zugriff. Jetzt aktivieren wir diese Gruppen, um sudo ohne Kennwort zu verwenden.

```bash
# Execute visudo as root to edit the /etc/sudoers file
bill@slackware$ visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# to this
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# to this
%sudo   ALL=(ALL) NOPASSWD:ALL
```

### Überprüfen von Benutzer, SSH-Schlüsseln und sudo

```bash
# Verify the SSH keys & User account
bill@slackware$ ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
exampleuser@exampleserver$ sudo top
```

<!---HONumber=AcomDC_0330_2016-->