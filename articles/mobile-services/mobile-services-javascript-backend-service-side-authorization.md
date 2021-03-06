<properties
	pageTitle="Dienstseitige Autorisierung von Benutzern in einem mobilen JavaScript-Back-End-Dienst | Microsoft Azure"
	description="Erfahren Sie, wie Sie Benutzer im JavaScript-Back-End von Azure Mobile Services autorisieren."
	services="mobile-services"
	documentationCenter=""
	authors="krisragh"
	manager="dwrede"
	editor=""/>

<tags
	ms.service="mobile-services"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-multiple"
	ms.topic="article"
	ms.devlang="javascript"
	ms.date="03/09/2016"
	ms.author="krisragh"/>

# Dienstseitige Autorisierung von Benutzern in Mobile Services
> [AZURE.SELECTOR]
- [.NET-Back-End](mobile-services-dotnet-backend-service-side-authorization.md)
- [JavaScript-Back-End](mobile-services-javascript-backend-service-side-authorization.md)

&nbsp;

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]
> Die entsprechende Mobile Apps-Version dieses Themas finden Sie in [diesem Beispielcode](https://github.com/Azure/azure-mobile-apps-node/blob/master/samples/personal-table/tables/TodoItem.js#L38).

In diesem Thema wird veranschaulicht, wie serverseitige Skripts verwendet werden können, um Benutzer zu autorisieren. In diesem Lernprogramm registrieren Sie Skripts für Azure Mobile Services, filtern Sie Abfragen anhand von Benutzer-IDs, und gewähren Sie Benutzern nur Zugriff auf deren eigene Daten. Das Filtern der Abfrageergebnisse eines Benutzers anhand der Benutzer-ID ist die einfachste Form der Autorisierung. Je nach Szenario möchten Sie vielleicht auch Benutzer oder Rollentabellen erstellen, um ausführlichere Berechtigungsinformationen für Benutzer zu verfolgen, beispielsweise auf welche Endpunkte ein angegebener Benutzer zugreifen darf.

Dieses Lernprogramm basiert auf Mobile Services-Schnellstart und setzt auf dem Lernprogramm [Hinzufügen von Authentifizierung zu einer vorhandenen Mobile Services-App] auf. Bearbeiten Sie zunächst [Hinzufügen von Authentifizierung zu einer vorhandenen Mobile Services-App].

## <a name="register-scripts"></a>Registrieren von Skripts

1. Melden Sie sich beim [klassischen Azure-Portal] an, klicken Sie auf **Mobile Services** und dann auf Ihren mobilen Dienst. Klicken Sie auf die Registerkarte **Data**, dann auf die Tabelle **TodoItem**.

2. Klicken Sie auf **Skript**, wählen Sie den Vorgang **Einfügen**, ersetzen Sie das vorhandene Skript durch die folgende Funktion, und klicken Sie dann auf **Save**.

        function insert(item, user, request) {
          item.userId = user.userId;
          request.execute();
        }

	Dieses Skript fügt dem Element vor dem Einfügen die Benutzer-ID des authentifizierten Benutzers hinzu.

    >[AZURE.NOTE] Stellen Sie sicher, dass das [dynamische Schema](https://msdn.microsoft.com/library/azure/jj193175.aspx) aktiviert ist. Andernfalls wird die Spalte *userID* nicht automatisch hinzugefügt. Diese Einstellung ist für einen neuen mobilen Dienst standardmäßig aktiviert.

3. Ersetzen Sie auf gleiche Weise den vorhandenen **Read**-Vorgang durch die folgende Funktion: Dieses Skript filtert die zurückgegebenen TodoItem-Objekte, sodass ein Benutzer nur die Elemente empfängt, die er selbst eingefügt hat.

        function read(query, user, request) {
           query.where({ userId: user.userId });
           request.execute();
        }

## <a name="test-app"></a>Testen der App

1. Wenn Sie nun Ihre clientseitige App ausführen, werden keine Elemente zurückgegeben, obwohl es aus früheren Lernprogrammen bereits Elemente in der _TodoItem_-Tabelle gibt. Dies liegt daran, dass die vorherigen Elemente ohne die Spalte für die Benutzer-ID eingefügt wurden und jetzt NULL-Werte haben. Vergewissern Sie sich, dass die neu hinzugefügten Elemente einen zugehörigen userId-Wert in der _TodoItem_-Tabelle haben.

2. Wenn Sie weitere Anmeldekonten haben, sollten Sie sicherstellen, dass Benutzer nur ihre eigenen Daten sehen können. Schließen und löschen Sie dazu die App, und führen Sie sie dann erneut aus. Wenn das Anmeldedialogfeld angezeigt wird, geben Sie andere Anmeldedaten ein, und vergewissern Sie sich, dass die unter den vorherigen Anmeldedaten eingegebenen Elemente nicht angezeigt werden.

<!-- Anchors. -->
[Register server scripts]: #register-scripts
[Next Steps]: #next-steps

<!-- Images. -->

<!-- URLs. -->

[Windows Push Notifications & Live Connect]: http://go.microsoft.com/fwlink/p/?LinkID=257677
[Mobile Services server script reference]: http://go.microsoft.com/fwlink/p/?LinkId=262293
[My Apps dashboard]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Hinzufügen von Authentifizierung zu einer vorhandenen Mobile Services-App]: /develop/mobile/tutorials/get-started-with-users-ios

[klassischen Azure-Portal]: https://manage.windowsazure.com/

<!---HONumber=AcomDC_0323_2016-->