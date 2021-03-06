<properties
	pageTitle="Azure Functions-Trigger mit Timer | Microsoft Azure"
	description="Erfahren Sie, wie Trigger mit Timer in Azure Functions verwendet werden."
	services="functions"
	documentationCenter="na"
	authors="christopheranderson"
	manager="erikre"
	editor=""
	tags=""
	keywords="Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur"/>

<tags
	ms.service="functions"
	ms.devlang="multiple"
	ms.topic="reference"
	ms.tgt_pltfrm="multiple"
	ms.workload="na"
	ms.date="05/16/2016"
	ms.author="chrande"/>

# Azure Functions-Trigger mit Timer

Dieser Artikel erläutert das Konfigurieren von Triggern mit Timer in Azure Functions. Trigger mit Timer rufen Funktionen anhand eines Zeitplans einmalig oder wiederholt auf.

[AZURE.INCLUDE [Einführung](../../includes/functions-bindings-intro.md)]

## „function.json“ für Trigger mit Timer

Die Datei *function.json* enthält einen Zeitplanausdruck und einen Schalter, der angibt, ob die Funktion sofort ausgelöst werden soll.

```json
{
  "bindings": [
    {
      "schedule": "0 * * * * *",
      "runOnStartup": true,
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Der Trigger mit Timer übernimmt automatisch das horizontale Hochskalieren über mehrere Instanzen: Nur eine einzelne Instanz einer bestimmten Timerfunktion wird für alle Instanzen ausgeführt.

## Format des Zeitplanausdrucks

Der Zeitplanausdruck kann ein [CRON-Ausdruck](http://en.wikipedia.org/wiki/Cron#CRON_expression) mit sechs Feldern sein: {Sekunde} {Minute} {Stunde} {Tag} {Monat} {Wochentag}. In vielen Dokumenten mit CRON-Ausdrücken, die online zu finden sind, wird das Feld {Sekunde} ausgelassen. Wenn Sie daher aus einem dieser Dokumente kopieren, müssen Sie das zusätzliche Feld berücksichtigen.

Der Zeitplanausdruck kann auch im Format *hh:mm:ss* vorliegen, um die Verzögerung zwischen den Zeitpunkten der Funktionsauslösung anzugeben.

Hier sind einige Beispiele für Zeitplanausdrücke aufgeführt.

Alle 5 Minuten auslösen:

```json
"schedule": "0 */5 * * * *",
"runOnStartup": false,
```

Sofort und anschließend alle zwei Stunden auslösen:

```json
"schedule": "0 0 */2 * * *",
"runOnStartup": true,
```

Alle 15 Sekunden auslösen:

```json
"schedule": "00:00:15",
"runOnStartup": false,
```

## C#-Codebeispiel für Trigger mit Timer

Dieses C#-Codebeispiel schreibt bei jedem Auslösen der Funktion ein Protokoll.

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");    
}
```

## Nächste Schritte

[AZURE.INCLUDE [Nächste Schritte](../../includes/functions-bindings-next-steps.md)]

<!---HONumber=AcomDC_0525_2016-->