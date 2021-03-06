<properties 
	pageTitle="Application Insights in Microsoft Azure" 
	description="Erkennen, begrenzen und diagnostizieren Sie Probleme in Ihrer Live-Web- oder Geräteanwendung. Überwachen und verbessern Sie fortlaufend die Beliebtheit bei den Benutzern." 
	services="application-insights" 
    documentationCenter=""
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="11/25/2015" 
	ms.author="awills"/>
 
# Visual Studio Application Insights

Application Insights ist ein erweiterbarer Analysedienst, der Live-Anwendungen überwacht. Sie können hiermit Leistungsprobleme erkennen und diagnostizieren und nachvollziehen, wie die Benutzer Ihre App nutzen. Der Dienst ist für Entwickler konzipiert und dient dazu, die Leistung und Benutzerfreundlichkeit Ihrer App kontinuierlich zu verbessern.

![Erstellen Sie ein Diagramm mit der Statistik zur Benutzeraktivität, oder führen Sie eine Detailsuche bei bestimmten Ereignissen aus.](./media/app-service-app-insights-get-started/00-sample.png)

Er funktioniert mit Web-Apps und eigenständigen Apps auf einer Vielzahl von Plattformen: .NET oder J2EE, lokal gehostet oder in der Cloud.

Application Insights ist für das Entwicklungsteam konzipiert. Damit können Sie folgende Aktionen ausführen:

* [Analysieren von Verwendungsmustern][knowUsers], um Ihre Benutzer besser zu verstehen und Ihre App kontinuierlich zu verbessern. 
 * Zähler für Seitenansichten, neue und wiederkehrende Benutzer, Geolocation, Plattformen und andere zentrale Nutzungsanalysen.
 * Ablaufverfolgung für Nutzungspfade, um den Erfolg der einzelnen Features zu bewerten.
* [Erkennen, Selektieren und Diagnostizieren][detect] Sie Leistungsprobleme, und korrigieren Sie sie, bevor die meisten Benutzer sie bemerken.
 *  Warnungen bei Leistungsänderungen oder Abstürzen.
 *  Metriken zur Diagnose von Leistungsproblemen, z. B. Reaktionszeiten, CPU-Auslastung, Nachverfolgung von Abhängigkeiten.
 *  Verfügbarkeitstests für Web-Apps.
 *  Berichte und Warnungen zu Abstürzen und Ausnahmen.
 *  Leistungsstarke Suche für Diagnoseprotokolle (einschließlich Protokollablaufverfolgungen aus Ihren bevorzugten Protokollierungsframeworks).

Das SDK für jede Plattform umfasst eine Reihe von Modulen, die die Anwendung ohne Konfigurationsaufwand überwachen. Darüber hinaus können Sie Ihre eigene Telemetrie für detailliertere und besser abgestimmte Analysen codieren.

Von der Anwendung gesammelte Telemetriedaten werden im Azure-Portal gespeichert und analysiert, wo intuitive Ansichten und leistungsstarke Tools für eine schnelle Diagnose und Analyse zur Verfügung stehen.



Benötigen Sie noch tiefer gehende Analysen? [Exportieren Sie](app-insights-export-telemetry.md) Ihre Daten [nach SQL](app-insights-code-sample-export-telemetry-sql-database.md), [Power BI](app-insights-export-power-bi.md) oder eigenen Tools.

![Anzeigen von Daten in Power BI](./media/app-service-app-insights-get-started/210.png)

## Plattformen und Sprachen

Es gibt SDKs für eine wachsende Palette an Plattformen. Derzeit enthält die Liste:

 * [ASP.NET-Server][greenbrown] auf Azure oder dem IIS-Server
 * [Azure Cloud Services](app-insights-cloudservices.md)
 * [J2EE-Server][java]
 * [Webseiten][client]\: HTML und JavaScript
 * [Windows-Dienste, Workerrollen und Desktop-Apps][desktop]
 * [Weitere Plattformen][platforms] – Node.js, PHP, Python, Ruby, Joomla, SharePoint, WordPress

Application Insights kann Telemetriedaten auch aus vorhandenen ASP.NET-Web-Apps in IIS abrufen, ohne diese neu zu erstellen.

Wenn Ihre App Clients, Server und andere Komponenten aufweist, können Sie sie alle instrumentieren. Die Daten werden in das Application Insights-Portal integriert, sodass Sie beispielsweise Ereignisse auf dem Client mit Ereignissen auf dem Server vergleichen können.


## So funktioniert's

Sie installieren ein kleines SDK in Ihrer Anwendung und richten ein Konto im Application Insights-Portal ein. Das SDK überwacht Ihre App und sendet Telemetriedaten an das Portal. Das Portal zeigt Ihnen statistische Diagramme und bietet leistungsfähige Suchtools, mit denen Sie Probleme diagnostizieren können.

![Das Application Insights-SDK in Ihrer App sendet Telemetriedaten an Ihre Application Insights-Ressource im Azure-Portal.](./media/app-service-app-insights-get-started/01-scheme.png)

Das SDK umfasst mehrere Module, die Telemetriedaten sammeln, um z. B. Benutzer und Sitzungen zu zählen und Leistungsdaten zu erfassen. Sie können auch Ihren eigenen benutzerdefinierten Code zum Senden von Telemetriedaten an das Portal schreiben. Benutzerdefinierte Telemetrie ist besonders nützlich für die Ablaufverfolgung von User Storys: Sie können Ereignisse zählen wie z. B. Schaltflächenklicks, das Erreichen bestimmter Ziele oder Benutzerfehler.

Für ASP.NET-Server und Azure Web-Apps können Sie auch [Statusmonitor][redfield] installieren, der zwei Verwendungszwecke erfüllt. Sie können damit folgende Aktionen durchführen:

* Überwachen einer Web-App, ohne sie neu zu erstellen oder neu zu installieren.
* Verfolgen von Aufrufen abhängiger Module.



### Wie sieht der Aufwand aus?

Die Auswirkungen auf die Leistung sind sehr gering. Aufrufe werden nicht blockierend nachverfolgt, zusammengefasst und in einem separaten Thread gesendet.



## Erste Schritte

1. Sie benötigen ein [Microsoft Azure](http://azure.com)-Abonnement. Die Registrierung ist kostenlos, und Sie können den kostenlosen [Tarif](https://azure.microsoft.com/pricing/details/application-insights/)von Application Insights auswählen.

2. Bei Verwendung von Visual Studio:

 * Neues Projekt: Aktivieren Sie das Kontrollkästchen **Application Insights hinzufügen**.
 * Bestehendes Projekt: Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **Application Insights hinzufügen** aus.

Wenn Sie Visual Studio nicht verwenden oder die Optionen für das Projekt nicht verfügbar sind, [suchen Sie Ihre Plattform hier](app-insights-platforms.md).

3. Führen Sie die App aus (im Entwicklungsmodus, oder veröffentlichen Sie sie), und sehen Sie im [Azure-Portal](https://portal.azure.com) zu, wie Daten gesammelt werden.

## Code


[Beispiele und exemplarische Vorgehensweisen](app-insights-code-samples.md)

[SDK Labs](https://www.myget.org/gallery/applicationinsights-sdk-labs) – NuGet-Pakete, die Sie als Ergänzungen des Application Insights SDK installieren und deinstallieren können. Probieren Sie sie aus, und geben Sie uns Feedback!


## Support und Feedback

* Fragen und Probleme:
 * [Problembehandlung][qna]
 * [MSDN-Forum](https://social.msdn.microsoft.com/Forums/vstudio/de-DE/home?forum=ApplicationInsights)
 * [StackOverflow](http://stackoverflow.com/questions/tagged/ms-application-insights)
* Fehler:
 * [Kontakt](https://connect.microsoft.com/VisualStudio/Feedback/LoadSubmitFeedbackForm?FormID=6076)
* Vorschläge:
 * [Aussagen von Benutzern](http://visualstudio.uservoice.com/forums/121579-visual-studio/category/77108-application-insights)


## Videos


> [AZURE.VIDEO 218]

> [AZURE.VIDEO usage-monitoring-application-insights]

> [AZURE.VIDEO performance-monitoring-application-insights]


<!--Link references-->

[android]: https://github.com/Microsoft/ApplicationInsights-Android
[azure]: ../insights-perf-analytics.md
[client]: app-insights-javascript.md
[desktop]: app-insights-windows-desktop.md
[detect]: app-insights-detect-triage-diagnose.md
[greenbrown]: app-insights-asp-net.md
[ios]: https://github.com/Microsoft/ApplicationInsights-iOS
[java]: app-insights-java-get-started.md
[knowUsers]: app-insights-overview-usage.md
[platforms]: app-insights-platforms.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[windows]: app-insights-windows-get-started.md

 

<!---HONumber=AcomDC_1203_2015-->