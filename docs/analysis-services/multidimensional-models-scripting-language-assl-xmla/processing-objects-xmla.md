---
title: Le traitement des objets (XMLA) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 5318e41b9f5aed9d1cc4b3855bb649aa6f3e63f6
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="processing-objects-xmla"></a>Traitement d'objets (XMLA)
  Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], le traitement est l’étape ou la série d’étapes qui transforme les données en informations pour l’analyse de l’entreprise. Si le traitement varie selon le type d'objet, le traitement consiste toujours à transformer des données en informations.  
  
 Processus un [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] de l’objet, vous pouvez utiliser la [processus](../../analysis-services/xmla/xml-elements-commands/process-element-xmla.md) commande. Le **processus** commande peut traiter les objets suivants dans un [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] instance :  
  
-   Cubes  
  
-   Bases de données  
  
-   Dimensions  
  
-   les groupes de mesures ;  
  
-   Modèles d'exploration de données  
  
-   Structures d'exploration de données  
  
-   Partitions  
  
 Pour contrôler le traitement d’objets, le **processus** commande possède plusieurs propriétés qui peuvent être définies. Le **processus** commande possède des propriétés qui contrôlent : la quantité de traitement sera effectuée, les objets seront traités, s’il faut utiliser des liaisons hors ligne, comment gérer les erreurs et la gestion des tables d’écriture différée.  
  
## <a name="specifying-processing-options"></a>Spécification d'options de traitement  
 Le [Type](../../analysis-services/xmla/xml-elements-properties/type-element-xmla.md) propriété de la **processus** commande spécifie l’option de traitement à utiliser lors du traitement de l’objet. Pour plus d’informations sur les options de traitement, consultez [Options et paramètres de traitement &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/processing-options-and-settings-analysis-services.md).  
  
 Le tableau suivant répertorie les constantes pour les **Type** propriété et les divers objets qui peuvent être traitées à l’aide de chaque constante.  
  
|**Type** valeur|Objets applicables|  
|--------------------|------------------------|  
|*ProcessFull*|Cube, base de données, dimension, groupe de mesures, modèle d'exploration de données, structure d'exploration de données, partition|  
|*ProcessAdd*|Dimension, partition|  
|*ProcessUpdate*|Dimension|  
|*ProcessIndexes*|Dimension, cube, groupe de mesures, partition|  
|*ProcessData*|Dimension, cube, groupe de mesures, partition|  
|*ProcessDefault*|Cube, base de données, dimension, groupe de mesures, modèle d'exploration de données, structure d'exploration de données, partition|  
|*ProcessClear*|Cube, base de données, dimension, groupe de mesures, modèle d'exploration de données, structure d'exploration de données, partition|  
|*ProcessStructure*|Cube, structure d'exploration de données|  
|*ProcessClearStructureOnly*|Structure d'exploration de données|  
|*ProcessScriptCache*|Cube|  
  
 Pour plus d’informations sur le traitement [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , consultez [du traitement d’un modèle multidimensionnel &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/processing-a-multidimensional-model-analysis-services.md).  
  
## <a name="specifying-objects-to-be-processed"></a>Spécification des objets à traiter  
 Le [objet](../../analysis-services/xmla/xml-elements-properties/object-element-xmla.md) propriété de la **processus** commande contient l’identificateur d’objet de l’objet à traiter. Seul un objet peut être spécifié dans un **processus** commande, mais que le traitement d’un objet traite également tous les objets enfants. Par exemple, le traitement d'un groupe de mesures dans un cube englobe toutes les partitions de ce groupe de mesures. De même, le traitement d'une base de données porte sur tous ses objets, notamment les cubes, les dimensions et les structures d'exploration de données contenus dans la base de données.  
  
 Si vous définissez la **ProcessAffectedObjects** attribut de la **processus** commande est true, les associés à l’objet concerné par le traitement de l’objet spécifié est également traité. Par exemple, si une dimension est mise à jour incrémentielle à l’aide de la *ProcessUpdate* traitement option dans le **processus** de commande, n’importe quelle partition dont les agrégations sont invalidées en raison d’être ajoutées ou supprimées sont également traitées par les membres [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] si **ProcessAffectedObjects** est définie sur true. Dans ce cas, un seul **processus** commande peut traiter plusieurs objets dans une [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] instance, mais [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] détermine quels objets en dehors de l’objet unique spécifié dans le **processus** doit également traiter la commande.  
  
 Toutefois, vous pouvez traiter plusieurs objets, tels que les dimensions, en même temps en utilisant plusieurs **processus** des commandes dans un **lot** commande. Opérations par lots fournissent un niveau plus élevé de contrôle pour le traitement de série ou parallèle des objets sur un [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] instance que l’utilisation de la **ProcessAffectedObjects** d’attribut et vous permettent de pour paramétrer votre approche de traitement supérieure [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] bases de données. Pour plus d’informations sur l’exécution d’opérations par lots, consultez [exécution d’opérations de traitement par lots &#40;XMLA&#41;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/performing-batch-operations-xmla.md).  
  
## <a name="specifying-out-of-line-bindings"></a>Spécification de liaisons hors ligne  
 Si le **processus** commande n’est pas contenue dans un **lot** commande, vous pouvez éventuellement spécifier les liaisons hors ligne dans le [liaisons](../../analysis-services/xmla/xml-elements-properties/bindings-element-xmla.md), [DataSource](../../analysis-services/xmla/xml-elements-properties/datasource-element-xmla.md), et [DataSourceView](../../analysis-services/xmla/xml-elements-properties/datasourceview-element-xmla.md) propriétés de la **processus** commande pour les objets à traiter. Les liaisons hors ligne sont des références à des sources de données, vues de sources de données et d’autres objets dans laquelle la liaison existe uniquement pendant l’exécution de la **processus** commande, et elles remplacent les liaisons existantes associées aux objets en cours de traitement. Si aucune liaison hors ligne n'est spécifiée, les liaisons actuellement associées aux objets à traiter sont utilisées.  
  
 Les liaisons hors ligne sont utilisées dans les circonstances suivantes :  
  
-   traitement incrémentiel d'une partition dans laquelle une table de faits alternative ou un filtre appliqué à la table de faits existante doit être spécifié pour éviter que les lignes ne soient comptées deux fois ;  
  
-   À l’aide d’une tâche de flux de données dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour fournir des données lors du traitement d’une dimension, un modèle d’exploration de données ou une partition.  
  
 Les liaisons hors ligne sont décrites dans le cadre d'ASSL (Analysis Services Scripting Language). Pour plus d’informations sur les liaisons hors ligne dans ASSL, consultez [des Sources de données et des liaisons &#40;multidimensionnels SSAS&#41;](../../analysis-services/multidimensional-models/data-sources-and-bindings-ssas-multidimensional.md).  
  
### <a name="incrementally-updating-partitions"></a>Mise à jour incrémentielle des partitions  
 La mise à jour incrémentielle d'une partition déjà traitée fait généralement appel à une liaison hors ligne, car la liaison spécifiée pour la partition fait référence à des données de table de faits déjà agrégées dans la partition. Lors de la mise à jour incrémentielle d’une partition déjà traitée à l’aide de la **processus** commande [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] effectue les actions suivantes :  
  
-   création d'une partition temporaire avec une structure identique à celle de la partition devant faire l'objet d'une mise à jour incrémentielle ;  
  
-   Traite la partition temporaire, à l’aide de la liaison hors ligne spécifiée dans le **processus** commande.  
  
-   fusion de la partition temporaire avec la partition sélectionnée existante.  
  
 Pour plus d’informations sur la fusion de partitions à l’aide de XML for Analysis (XMLA), consultez [la fusion de Partitions &#40;XMLA&#41;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/merging-partitions-xmla.md).  
  
## <a name="handling-processing-errors"></a>Gestion des erreurs de traitement  
 Le [ErrorConfiguration](../../analysis-services/xmla/xml-elements-properties/errorconfiguration-element-xmla.md) propriété de la **processus** commande vous permet de spécifier comment gérer les erreurs rencontrées lors du traitement d’un objet. Par exemple, lors du traitement d'une dimension, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] rencontre une valeur en double dans la colonne clé de l'attribut de clé. Du fait que les clés d'attribut doivent être uniques, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ignore les enregistrements en double. Selon le [KeyDuplicate](../../analysis-services/scripting/properties/keyduplicate-element-assl.md) propriété du **ErrorConfiguration**, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] impossible :  
  
-   ignorer l'erreur et poursuivre le traitement de la dimension ;  
  
-   retourner un message qui indique qu'[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] a rencontré une clé en double et poursuivre le traitement.  
  
 Il existe de nombreuses conditions similaires **ErrorConfiguration** fournit des options pendant un **processus** commande.  
  
## <a name="managing-writeback-tables"></a>Gestion des tables d'écriture différée  
 Si le **processus** commande rencontre une partition activée en écriture, ou un cube ou groupe de mesures pour une partition de ce type, qui n’est pas déjà entièrement traité, la table d’écriture différée ne peut pas existe déjà pour cette partition. Le [WritebackTableCreation](../../analysis-services/xmla/xml-elements-properties/writebacktablecreation-element-xmla.md) propriété de la **processus** commande détermine si [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] doit créer une table d’écriture différée.  
  
## <a name="examples"></a>Exemples  
  
### <a name="description"></a> Description  
 L'exemple suivant traite entièrement la base de données [!INCLUDE[ssAWDWsp](../../includes/ssawdwsp-md.md)] sample [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
### <a name="code"></a>Code  
  
```  
<Process xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">  
  <Object>  
    <DatabaseID>Adventure Works DW Multidimensional 2012</DatabaseID>  
  </Object>  
  <Type>ProcessFull</Type>  
  <WriteBackTableCreation>UseExisting</WriteBackTableCreation>  
</Process>  
```  
  
### <a name="description"></a> Description  
 L’exemple suivant traite de façon incrémentielle le **Internet_Sales_2004** dans la partition la **Internet Sales** groupe de mesures de la **Adventure Works DW** de cube dans le [!INCLUDE[ssAWDWsp](../../includes/ssawdwsp-md.md)] exemple [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] base de données. Le **processus** commande consiste à ajouter des agrégations pour la commande dates postérieures au 31 décembre 2006 à la partition à l’aide d’une liaison de requête de sortie de la ligne dans le **liaisons** propriété de la **processus** commande pour récupérer les lignes de table de faits à partir de laquelle générer des agrégations à ajouter à la partition.  
  
### <a name="code"></a>Code  
  
```  
<Process ProcessAffectedObjects="true" xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">  
  <Object>  
    <DatabaseID>Adventure Works DW Multidimensional 2012</DatabaseID>  
    <CubeID>Adventure Works DW</CubeID>  
    <MeasureGroupID>Fact Internet Sales 1</MeasureGroupID>  
    <PartitionID>Internet_Sales_2006</PartitionID>  
  </Object>  
  <Bindings>  
    <Binding>  
      <DatabaseID>Adventure Works DW Multidimensional 2012</DatabaseID>  
      <CubeID>Adventure Works DW</CubeID>  
      <MeasureGroupID>Fact Internet Sales 1</MeasureGroupID>  
      <PartitionID>Internet_Sales_2006</PartitionID>  
      <Source xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="QueryBinding">  
        <DataSourceID>Adventure Works DW</DataSourceID>  
        <QueryDefinition>  
          SELECT  
            [dbo].[FactInternetSales].[ProductKey],  
            [dbo].[FactInternetSales].[OrderDateKey],  
            [dbo].[FactInternetSales].[DueDateKey],  
            [dbo].[FactInternetSales].[ShipDateKey],   
            [dbo].[FactInternetSales].[CustomerKey],   
            [dbo].[FactInternetSales].[PromotionKey],  
            [dbo].[FactInternetSales].[CurrencyKey],  
            [dbo].[FactInternetSales].[SalesTerritoryKey],  
            [dbo].[FactInternetSales].[SalesOrderNumber],  
            [dbo].[FactInternetSales].[SalesOrderLineNumber],  
            [dbo].[FactInternetSales].[RevisionNumber],  
            [dbo].[FactInternetSales].[OrderQuantity],  
            [dbo].[FactInternetSales].[UnitPrice],  
            [dbo].[FactInternetSales].[ExtendedAmount],  
            [dbo].[FactInternetSales].[UnitPriceDiscountPct],  
            [dbo].[FactInternetSales].[DiscountAmount],  
            [dbo].[FactInternetSales].[ProductStandardCost],  
            [dbo].[FactInternetSales].[TotalProductCost],  
            [dbo].[FactInternetSales].[SalesAmount],  
            [dbo].[FactInternetSales].[TaxAmt],  
            [dbo].[FactInternetSales].[Freight],  
            [dbo].[FactInternetSales].[CarrierTrackingNumber],  
            [dbo].[FactInternetSales].[CustomerPONumber]  
          FROM [dbo].[FactInternetSales]  
          WHERE OrderDateKey > '1280'  
        </QueryDefinition>  
      </Source>  
    </Binding>  
  </Bindings>  
  <Type>ProcessAdd</Type>  
  <WriteBackTableCreation>UseExisting</WriteBackTableCreation>  
</Process>  
```  
  
  
