<properties
   pageTitle="Verknüpfte Vorlagen mit Azure Resource Manager | Microsoft Azure"
   description="Beschreibt, wie verknüpfte Vorlagen in einer Azure-Ressourcen-Manager-Vorlage zum Erstellen einer modularen Vorlagenprojektmappe verwendet werden. Zeigt, wie Parameterwerte übergeben, eine Parameterdatei festgelegt und URLs dynamisch erstellt werden."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/04/2016"
   ms.author="tomfitz"/>

# Verwenden von verknüpften Vorlagen mit Azure-Ressourcen-Manager

Aus einer Azure-Ressourcen-Manager-Vorlage heraus können Sie einen Link zu einer anderen Vorlage herstellen, sodass Sie die Bereitstellung in eine Reihe von ausgewählten und zweckgebundenen Vorlagen zerlegen können. Wie das Zerlegen einer Anwendung in eine Reihe von Codeklassen bietet diese Zerlegung Vorteile in Bezug auf Tests, Wiederverwendung und Lesbarkeit.

Sie können Parameter aus einer Hauptvorlage an eine verknüpfte Vorlage übergeben, und diese Parameter können direkt Parametern oder Variablen zugeordnet werden, die von der aufrufenden Vorlage verfügbar gemacht werden. Die verknüpfte Vorlage kann auch eine Ausgabevariable zurück an die Quellvorlage übergeben, wodurch ein bidirektionaler Datenaustausch zwischen Vorlagen ermöglicht wird.

## Verknüpfen mit einer Vorlage

Sie erstellen einen Link zwischen zwei Vorlagen durch Hinzufügen einer Bereitstellungsressource innerhalb der Hauptvorlage, die auf die verknüpfte Vorlage verweist. Sie legen die **templateLink**-Eigenschaft für den URI der verknüpften Vorlage fest. Für die verknüpfte Vorlage können Sie Parameterwerte angeben, indem Sie die Werte entweder direkt in der Vorlage oder per Verknüpfung mit einer Parameterdatei angeben. Im folgenden Beispiel wird die **parameters**-Eigenschaft verwendet, um direkt einen Parameterwert anzugeben.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "nestedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion": "1.0.0.0"
           }, 
           "parameters": { 
              "StorageAccountName":{"value": "[parameters('StorageAccountName')]"} 
           } 
         } 
      } 
    ] 

Der Ressourcen-Manager-Dienst muss in der Lage sein, auf die verknüpften Vorlagen zuzugreifen. Dies bedeutet, dass Sie keine lokale Datei und keine Datei, die nur in Ihrem lokalen Netzwerk verfügbar ist, für die verknüpfte Vorlage angeben können. Sie müssen einen URI-Wert bereitstellen, der entweder **http** oder **https** enthält. Eine Option besteht darin, Ihre verknüpfte Vorlage in einem Speicherkonto zu platzieren und den URI für dieses Element zu verwenden, wie nachfolgend gezeigt.

    "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0",
    }


## Verknüpfen mit einer Parameterdatei

Im nächsten Beispiel wird die **parametersLink**-Eigenschaft genutzt, um eine Verknüpfung mit einer Parameterdatei herzustellen.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "nestedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri":"https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion":"1.0.0.0"
           }, 
           "parametersLink": { 
              "uri":"https://www.contoso.com/AzureTemplates/parameters.json",
              "contentVersion":"1.0.0.0"
           } 
         } 
      } 
    ] 

Der URI-Wert für die verknüpfte Parameterdatei darf keine lokale Datei sein und muss entweder **http** oder **https** enthalten.

## Verwenden von Variablen für das Verknüpfen von Vorlagen

Die vorherigen Beispiele zeigen hartcodierte URL-Werte für die Vorlagenlinks. Dieser Ansatz funktioniert u. U. für eine einfache Vorlage, aber er funktioniert nicht gut bei der Arbeit mit einer großen Anzahl von modularen Vorlagen. Stattdessen können Sie eine statische Variable erstellen, die eine Basis-URL für die Hauptvorlage speichert, und dann aus dieser Basis-URL dynamisch URLs für die verknüpften Vorlagen erstellen. Der Vorteil dieses Ansatzes besteht darin, dass Sie die Vorlage problemlos verschieben oder verzweigen können, da Sie nur die statische Variable in der Hauptvorlage ändern müssen. Die Hauptvorlage übergibt die richtigen URIs an die zerlegten Vorlagen.

Das folgende Beispiel zeigt, wie Sie eine Basis-URL verwenden können, um zwei URLs für verknüpfte Vorlagen (**sharedTemplateUrl** und **vmTemplate**) zu erstellen.

    "variables": {
        "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
        "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
        "tshirtSizeSmall": {
            "vmSize": "Standard_A1",
            "diskSize": 1023,
            "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
            "vmCount": 2,
            "slaveCount": 1,
            "storage": {
                "name": "[parameters('storageAccountNamePrefix')]",
                "count": 1,
                "pool": "db",
                "map": [0,0],
                "jumpbox": 0
            }
        }
    }

Sie können auch [Bereitstellung()](../resource-group-template-functions/#deployment) verwenden, um die Basis-URL für die aktuelle Vorlage zu erhalten. Mit dieser können Sie die URL für die anderen Vorlagen am gleichen Speicherort abrufen. Dies ist hilfreich, wenn sich der Speicherort der Vorlage ändert (möglicherweise aufgrund einer Versionsverwaltung) oder wenn Sie es vermeiden möchten, URLs in der Vorlagendatei fest programmieren zu müssen.

    "variables": {
        "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
    }

## Zurückgeben von Werten aus einer verknüpften Vorlage

Wenn Sie einen Wert aus der verknüpften Vorlage an die Hauptvorlage übergeben müssen, können Sie einen Wert im **outputs**-Abschnitt der verknüpften Vorlage erstellen. Ein Beispiel finden Sie unter [Freigeben des Status in Azure-Ressourcen-Manager-Vorlagen](best-practices-resource-manager-state.md).

## Nächste Schritte
- Informationen zum Definieren der Bereitstellungsreihenfolge Ihrer Ressourcen finden Sie unter [Definieren von Abhängigkeiten in Azure-Ressourcen-Manager-Vorlagen](resource-group-define-dependencies.md).
- Informationen, wie Sie eine Ressource definieren und von dieser viele Instanzen erstellen, finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen im Azure-Ressourcen-Manager](resource-group-create-multiple.md).

<!---HONumber=AcomDC_0406_2016-->