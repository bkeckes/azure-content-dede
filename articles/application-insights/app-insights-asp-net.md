<properties 
	pageTitle="Web-App-Analyse für ASP.NET mit Application Insights" 
	description="Leistung, Verfügbarkeit und Nutzungsanalyse für Ihre lokal oder in Azure gehosteten ASP.NET-Website." 
	services="application-insights" 
    documentationCenter=".net"
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="get-started-article" 
	ms.date="05/12/2016" 
	ms.author="awills"/>


# Einrichten von Application Insights für ASP.NET


[AZURE.INCLUDE [app-insights-selector-get-started-dotnet](../../includes/app-insights-selector-get-started-dotnet.md)]

Das Application Insights SDK sendet Analysetelemetriedaten über Ihre Live-Webanwendung an das Azure-Portal, wo Sie sich anmelden und Diagramme zur Leistung und Nutzung Ihrer App anzeigen können.

![Beispiel für Leistungsüberwachungsdiagramm](./media/app-insights-asp-net/10-perf.png)

Ebenso werden Sie bestimmte Anforderungen, Ausnahmen und Protokollereignisse untersuchen und miteinander in Beziehung setzen können. Mit der API lassen sich Telemetriedaten zum detaillierten Überwachen der Leistung und Auslastung hinzufügen.

#### Vorbereitung

Erforderlich:

* Ein Abonnement für [Microsoft Azure](http://azure.com) Wenn Ihr Team oder Ihre Organisation über ein Azure-Abonnement verfügt, kann der Besitzer Sie mit Ihrem [Microsoft-Konto](http://live.com) hinzufügen.
* Visual Studio 2013, Update 3 oder höher.


## <a name="ide"></a> Hinzufügen von Application Insights zu Ihrem Projekt in Visual Studio

#### Falls es sich um ein neues Projekt handelt …

Wenn Sie in Visual Studio ein neues Projekt erstellen, achten Sie darauf, dass Application Insights ausgewählt ist.


![Erstellen eines ASP.NET-Projekts](./media/app-insights-asp-net/appinsights-01-vsnewp1.png)

Wählen Sie ein Konto mit einer Azure-Anmeldung aus. Sie werden möglicherweise aufgefordert, Ihre Anmeldeinformationen erneut einzugeben. (Oder, wenn Sie sich nicht anmelden, wird der Code des SDK hinzugefügt, den Sie später konfigurieren können.)


#### … oder falls es sich um ein vorhandenes Projekt handelt

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und klicken Sie auf **Application Insights hinzufügen** oder **Application Insights konfigurieren**.

![Application Insights hinzufügen](./media/app-insights-asp-net/appinsights-03-addExisting.png)


#### Einrichtungsoptionen

Wenn Sie erstmals ein Projekt erstellen, werden Sie aufgefordert, sich anzumelden oder bei Microsoft Azure zu registrieren.

Wenn diese App Teil einer größeren Anwendung ist, empfiehlt es sich, sie mithilfe von **Einstellungen konfigurieren** in derselben Ressourcengruppe wie die anderen Komponenten abzulegen.


####<a name="land"></a> Welche Funktion hat "Application Insights hinzufügen"?

Mit dem Befehl wurden die folgenden Schritte ausgeführt (die Sie stattdessen auch [manuell ausführen](app-insights-asp-net-manual.md) können):

1. Er fügt das NuGet-Paket mit dem Application Insights-Web-SDK dem Projekt hinzu. Um es in Visual Studio anzuzeigen, klicken Sie mit der rechten Maustaste auf Ihr Projekt, und wählen Sie "NuGet-Pakete verwalten" aus.
2. Er erstellt eine Application Insights-Ressource im [Azure-Portal][portal]. Hier werden Ihre Daten angezeigt. Er ruft den *Instrumentationsschlüssel* ab, der die Ressource identifiziert.
3. Fügt den Instrumentationsschlüssel in `ApplicationInsights.config` ein, damit das SDK Telemetriedaten an das Portal senden kann.

Wenn Sie sich anfänglich nicht bei Azure anmelden, wird das SDK installiert, ohne dass es eine Verbindung mit einer Ressource herstellt. Sie können während des Debuggens die Application Insights-Telemetriedaten im Suchfenster von Visual Studio anzeigen und durchsuchen. Die anderen Schritte können Sie später abschließen.

## <a name="run"></a> Debuggen des Projekts

Starten Sie Ihre Anwendung mit F5, und probieren Sie es aus: Öffnen Sie verschiedene Seiten, um einige Telemetriedaten zu generieren.

In Visual Studio sehen Sie eine Anzahl der protokollierten Ereignisse.

![In Visual Studio wird die Schaltfläche „Application Insights“ während des Debuggens angezeigt.](./media/app-insights-asp-net/appinsights-09eventcount.png)

Klicken Sie auf diese Schaltfläche, um die Diagnosesuche zu öffnen.



## Debuggen von Telemetriedaten

### Diagnosehub

Der Diagnosehub zeigt (ab Visual Studio 2015) die von Application Insights generierten Servertelemetriedaten. Dies funktioniert auch, wenn Sie sich entschieden haben, nur das SDK zu installieren, ohne es mit einer Ressource im Azure-Portal zu verbinden.

![Öffnen Sie das Fenster „Diagnosetools“, und überprüfen Sie die Application Insights-Ereignisse.](./media/app-insights-asp-net/31.png)


### Diagnosesuche

Im Suchfenster werden die protokollierten Ereignisse angezeigt. (Wenn Sie sich bei der Einrichtung von Application Insights bei Azure angemeldet haben, können Sie die gleichen Ereignisse im Portal durchsuchen.)

![Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie „Application Insights > Durchsuchen“ aus.](./media/app-insights-asp-net/34.png)

Die Freitextsuche funktioniert in allen Feldern in den Ereignissen. Suchen Sie z. B. einen Teil der URL einer Seite, den Wert einer Eigenschaft, z. B. Ort des Kunden, oder bestimmte Wörter in einem Ablaufverfolgungsprotokoll.

Sie können zum Diagnostizieren von fehlerhaften Anforderungen oder von Ausnahmen auch die Registerkarte „Verwandte Elemente“ öffnen.


![](./media/app-insights-asp-net/41.png)



### Ausnahmen

Wenn Sie [Ausnahmeüberwachung eingerichtet haben](app-insights-asp-net-exceptions.md), werden im Suchfenster Ausnahmeberichte angezeigt.

Klicken Sie auf eine Ausnahme, um eine Stapelüberwachung zu erhalten. Wenn der Code der App in Visual Studio geöffnet ist, können Sie sich durch die Stapelüberwachung bis zur entsprechenden Zeile im Code klicken.




### Lokale Überwachung



(In Visual Studio 2015 Update 2) Falls Sie das SDK nicht zum Senden von Telemetriedaten an das Application Insights-Portal konfiguriert haben (sodass kein Instrumentationsschlüssel in „ApplicationInsights.config“ enthalten ist), werden im Diagnosefenster Telemetriedaten aus der letzten Debugsitzung angezeigt.

Dies ist wünschenswert, wenn Sie bereits eine frühere Version der Anwendung veröffentlicht haben. Die Telemetriedaten aus den Debugsitzungen und die Telemetriedaten der veröffentlichten App im Application Insights-Portal müssen getrennt behandelt werden.

Dies ist ebenfalls hilfreich, wenn Sie über [benutzerdefinierte Telemetriedaten](app-insights-api-custom-events-metrics.md) verfügen, die Sie vor dem Senden an das Portal debuggen möchten.


* *Zunächst habe ich Application Insights vollständig für das Senden von Telemetriedaten an das Portal konfiguriert. Aber nun möchte ich die Telemetriedaten nur in Visual Studio anzeigen.*

 * In den Einstellungen des Suchfensters steht eine Option zum Durchsuchen der lokalen Diagnosen zur Verfügung, auch wenn Ihre App Telemetriedaten an das Portal sendet.
 * Damit keine Telemetriedaten mehr an das Portal gesendet werden, kommentieren Sie die Zeile `<instrumentationkey>...` in „ApplicationInsights.config“ aus. Wenn Sie bereit sind, Telemetriedaten erneut an das Portal zu senden, heben Sie die Auskommentierung auf.



## <a name="monitor"></a> Anzeigen von Telemetriedaten im Application Insights-Portal

Im Application Insights-Portal werden Telemetriedaten angezeigt, sobald Ihre Anwendung veröffentlicht wurde. Während des Debuggens soll sichergestellt werden, dass Telemetriedaten ordnungsgemäß gesendet werden.

Öffnen Sie die Application Insights-Ressource im [Azure-Portal][portal].

![Klicken Sie mit der rechten Maustaste auf Ihr Projekt, und öffnen Sie das Azure-Portal.](./media/app-insights-asp-net/appinsights-04-openPortal.png)

Wenn Sie sich beim Hinzufügen von Application Insights zu dieser App nicht bei Azure angemeldet haben, tun Sie dies jetzt. Wählen Sie **Application Insights konfigurieren**. Dadurch können Sie nach der Bereitstellung Ihrer Live-App weiterhin Telemetriedaten aus dieser App anzeigen. Die Telemetriedaten werden im Application Insights-Portal angezeigt.

### Livestream

Verwenden Sie einen Livestream, um beim Debuggen oder direkt nach der Bereitstellung eine Schnellansicht Ihrer Telemetriedaten anzuzeigen.

![Klicken Sie auf dem Blatt „Übersicht“ auf „Livestream“.](./media/app-insights-asp-net/45.png)


Der Livestream ist so konzipiert, dass Sie direkt nach der Bereitstellung überprüfen können, ob Ihre App ordnungsgemäß funktioniert.

Im Livestream werden lediglich Daten der letzten paar Minuten angezeigt und keine Daten beibehalten.

Für den Livestream ist mindestens Version 2.1.0-beta1 des SDK erforderlich.



### Suche: Einzelne Ereignisse

Öffnen Sie die Suche, um einzelne Anforderungen und die zugeordneten Ereignisse zu untersuchen.

![Suchen Sie auf dem Blatt „Suchen“ nach Seitennamen oder anderen Eigenschaften.](./media/app-insights-asp-net/21-search.png)

[Weitere Informationen zur Suche](app-insights-diagnostic-search.md)

* *Keine zugeordneten Ereignisse?* Richten Sie [Serverausnahmen](app-insights-asp-net-exceptions.md) und [Abhängigkeiten](app-insights-asp-net-dependencies.md) ein.


### Metriken: Aggregierte Daten

Suchen Sie in den Übersichtsdiagrammen nach aggregierten Daten. Zuerst sehen Sie lediglich einen oder zwei Punkte. Zum Beispiel:

![Klicken Sie, um weitere Daten anzuzeigen.](./media/app-insights-asp-net/12-first-perf.png)

Klicken Sie sich durch ein beliebiges Diagramm, um ausführlichere Metriken anzuzeigen. [Weitere Informationen zu Metriken.][perf]

* *Keine Daten zu Benutzern oder Seiten?* - [Hinzufügen von Daten zu Benutzern und Seiten](app-insights-web-track-usage.md)

### Analytics: Leistungsfähige Abfragesprache

Wenn sich mehr Daten ansammeln, können Sie Abfragen sowohl zum Aggregieren von Daten als auch zum Ermitteln einzelner Instanzen ausführen. [Analytics]() ist ein leistungsfähiges Tool zum Nachvollziehen der Leistung und Nutzung sowie für Diagnosezwecke.

![Analytics-Beispiel](./media/app-insights-asp-net/025.png)


## Sie sehen keine Daten?

* Stellen Sie in Visual Studio sicher, dass Ihre App Telemetriedaten sendet. Im Ausgabefenster und Diagnosehub sollten Ablaufverfolgungen angezeigt werden.
* Stellen Sie sicher, dass Sie das gewünschte Element in Azure untersuchen. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, klicken Sie auf "Durchsuchen" > "Application Insights", und wählen Sie dann die App aus.
* Verwenden Sie die Anwendung, und öffnen Sie verschiedene Seiten, damit einige Telemetriedaten generiert werden.
* Öffnen Sie das Blatt [Suche][diagnostic], um einzelne Ereignisse anzuzeigen. Manchmal dauert es eine Weile, bis Ereignisse über die Metrikpipeline übertragen werden.
* Warten Sie einige Sekunden, und klicken Sie auf "Aktualisieren".
* Informationen hierzu finden Sie unter [Problembehandlung][qna].



## Veröffentlichen der App

Stellen Sie jetzt Ihre Anwendung bereit, und sehen Sie zu, wie Daten gesammelt werden.

Überwachen Sie mit dem [Livestream](#live-stream) die ersten Minuten einer Bereitstellung oder erneuten Bereitstellung, um zu ermitteln, ob die App ordnungsgemäß funktioniert. Vor allem beim Ersetzen einer älteren Version möchten Sie wissen, ob sich die Leistung verbessert hat. Falls ein Problem auftritt, möchten Sie möglicherweise die alte Version wiederherstellen.


#### Probleme auf dem Buildserver?

Weitere Informationen finden Sie in [diesem Artikel zur Problembehandlung](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).

> [AZURE.NOTE] Wenn die Anwendung viele Telemetriedaten generiert (und Sie Version 2.0.0-beta3 oder höher des ASP.NET-SDK verwenden), reduziert das adaptive Stichprobenmodul automatisch die an das Portal gesendete Datenmenge, indem nur ein repräsentativer Bruchteil der Ereignisse gesendet wird. Ereignisse, die mit derselben Anforderung im Zusammenhang stehen, werden als Gruppe aus- oder abgewählt, sodass Sie zwischen verwandten Ereignissen navigieren können. [Erfahren Sie mehr über das Erstellen von Stichproben.](app-insights-sampling.md)



## Nächste Schritte

- [Daten zu Seiten und Benutzern](../article/application-insights/app-insights-javascript.md#selector1)
- [Ausnahmen](../article/application-insights/app-insights-asp-net-exceptions.md#selector1)
- [Abhängigkeiten](../article/application-insights/app-insights-asp-net-dependencies.md#selector1)
- [Verfügbarkeit](../article/application-insights/app-insights-monitor-web-app-availability.md#selector1)





## So upgraden Sie auf zukünftige SDK-Versionen

Für ein Upgrade auf eine [neue Version des SDK](app-insights-release-notes-dotnet.md) öffnen Sie wieder den NuGet-Paket-Manager und filtern die Ansicht nach installierten Paketen. Wählen Sie "Microsoft.ApplicationInsights.Web" und dann "Upgrade" aus.

Wenn Sie Anpassungen an der Datei "ApplicationInsights.config" vorgenommen haben, speichern Sie vor dem Upgrade eine Kopie davon. Sie können anschließend die Änderungen in die neue Version übernehmen.



## <a name="video"></a>Video

> [AZURE.VIDEO getting-started-with-application-insights]


<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apikey]: app-insights-api-custom-events-metrics.md#ikey
[availability]: app-insights-monitor-web-app-availability.md
[azure]: ../insights-perf-analytics.md
[client]: app-insights-javascript.md
[detect]: app-insights-detect-triage-diagnose.md
[diagnostic]: app-insights-diagnostic-search.md
[knowUsers]: app-insights-overview-usage.md
[metrics]: app-insights-metrics-explorer.md
[netlogs]: app-insights-asp-net-trace-logs.md
[perf]: app-insights-web-monitor-performance.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[roles]: app-insights-resources-roles-access-control.md
[start]: app-insights-overview.md

 

<!---HONumber=AcomDC_0525_2016-->