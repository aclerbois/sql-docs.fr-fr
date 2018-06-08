---
title: Définir les propriétés d’un composant de flux de données | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: data-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- components [Integration Services], properties
ms.assetid: 73000ef6-52a2-4dec-8320-0e79acf0c2c5
caps.latest.revision: 50
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 2a159f57e4315cba3f9eb3ee2d27cfffb94fb6f5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-the-properties-of-a-data-flow-component"></a>Définir les propriétés d’un composant de flux de données
  Pour définir les propriétés de composants de flux de données, qui incluent des sources, des destinations et des transformations, utilisez l'une des fonctionnalités suivantes :  
  
-   Les éditeurs de composants fournis par [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Ces éditeurs ne peuvent être utilisés que pour définir les propriétés personnalisées de chaque composant de flux de données.  
  
-   La fenêtre **Propriétés** énumère les propriétés personnalisées définies au niveau du composant pour chaque élément, ainsi que les propriétés communes à tous les éléments du flux de données.  
  
-   La boîte de dialogue **Éditeur avancé** permet d’accéder aux propriétés personnalisées pour chaque composant. La boîte de dialogue **Éditeur avancé** permet aussi d’accéder aux propriétés communes à l’ensemble des composants de flux de données, à savoir les propriétés des entrées, des sorties, des sorties d’erreurs, des colonnes et des colonnes externes.  
  
## <a name="set-the-properties-of-a-data-flow-component-with-a-component-editor"></a>Définir les propriétés d’un composant de flux de données à l’aide d’un éditeur de composant  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Cliquez sur l’onglet **Flux de contrôle** , puis double-cliquez sur la tâche de flux de données qui contient le flux de données et le composant dont vous voulez afficher ou modifier les propriétés.  
  
4.  Double-cliquez sur le composant du flux de données.  
  
5.  Dans l'éditeur de composant, affichez ou modifiez les valeurs des propriétés, puis fermez l'éditeur.  
  
6.  Pour enregistrer le package mis à jour, dans le menu **Fichier** , cliquez sur **Enregistrer les éléments sélectionnés**.  
  
## <a name="set-the-properties-of-a-data-flow-component-in-the-properties-window"></a>Définir les propriétés d’un composant de flux de données dans la fenêtre Propriétés  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Cliquez sur l’onglet **Flux de contrôle** , puis double-cliquez sur la tâche de flux de données qui contient le composant dont vous voulez afficher ou modifier les propriétés.  
  
4.  Cliquez avec le bouton droit sur le composant du flux de données, puis cliquez sur **Propriétés**.  
  
5.  Affichez ou modifiez les valeurs des propriétés, puis fermez la fenêtre **Propriétés** .  
  
    > [!NOTE]  
    >  De nombreuses propriétés sont en lecture seule et ne peuvent donc pas être modifiées.  
  
6.  Pour enregistrer le package mis à jour, dans le menu **Fichier** , cliquez sur **Enregistrer les éléments sélectionnés**.  
  
## <a name="set-the-properties-of-a-data-flow-component-with-the-advanced-editor"></a>Définir les propriétés d’un composant de flux de données à l’aide de l’Éditeur avancé  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Cliquez sur l’onglet **Flux de contrôle** , puis double-cliquez sur la tâche de flux de données qui contient le composant à afficher ou à modifier.  
  
4.  Dans le concepteur de flux de données, cliquez avec le bouton droit sur le composant du flux de données, puis cliquez sur **Afficher l’éditeur avancé**.  
  
    > [!NOTE]  
    >  Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les composants de flux de données qui prennent en charge plusieurs entrées ne peuvent pas utiliser **l’éditeur avancé**.  
  
5.  Dans la boîte de dialogue **Éditeur avancé** , procédez de l’une des façons suivantes :  
  
    -   Pour afficher et spécifier la connexion utilisée par le composant, cliquez sur l’onglet **Gestionnaires de connexions** .  
  
        > [!NOTE]  
        >  L’onglet **Gestionnaire de connexions** est accessible uniquement aux composants de flux de données qui utilisent des gestionnaires de connexions pour se connecter à des sources de données telles que des fichiers et des bases de données.  
  
    -   Pour afficher et modifier les propriétés au niveau du composant, cliquez sur l’onglet **Propriétés du composant** .  
  
    -   Pour afficher et modifier les mappages entre des colonnes externes et la sortie disponible, cliquez sur l’onglet **Mappage de colonnes** .  
  
        > [!NOTE]  
        >  L’onglet **Mappage de colonnes** est disponible uniquement pendant l’affichage ou la modification des sources et des destinations.  
  
    -   Pour afficher la liste des colonnes d’entrée disponibles et mettre à jour le nom des colonnes de sortie, cliquez sur l’onglet **Colonnes d’entrée** .  
  
        > [!NOTE]  
        >  L'onglet Colonnes d'entrée est disponible uniquement en cas d'utilisation de transformations ou de destinations. Pour plus d’informations, consultez [Transformations Integration Services](../../integration-services/data-flow/transformations/integration-services-transformations.md).  
  
    -   Pour afficher et modifier les propriétés des entrées, des sorties, des sorties d’erreur et les propriétés des colonnes qu’elles contiennent, cliquez sur l’onglet **Propriétés d’entrée et de sortie** .  
  
        > [!NOTE]  
        >  Les sources ne comportent pas d'entrées. Les destinations ne comportent pas de sorties, sauf pour une sortie d'erreur facultative.  
  
6.  Affichez ou modifiez les valeurs des propriétés.  
  
7.  Cliquez sur **OK**.  
  
8.  Pour enregistrer le package mis à jour, dans le menu **Fichier** , cliquez sur **Enregistrer les éléments sélectionnés**.  

## <a name="common-properties-of-data-flow-components"></a>Propriétés communes des composants de flux de données
Les objets de flux de données dans le modèle objet [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] présentent des propriétés communes et personnalisées au niveau des composants, des entrées et sorties, et des colonnes d'entrée et de sortie. De nombreuses propriétés ont des valeurs en lecture seule qui sont assignées au moment de l'exécution par le moteur de flux de données.  
  
 Cette rubrique répertorie et décrit les propriétés communes des objets de flux de données.  
  
-   [Components](#components)  
  
-   [Entrées](#inputs)  
  
-   [Input columns](#inputcolumns)  
  
-   [Sorties](#outputs)  
  
-   [Colonnes de sortie](#outputcolumns)  
  
 
###  <a name="components"></a> Component properties  
 Dans le modèle objet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], un composant dans le flux de données implémente l'interface <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100>.  
  
 Le tableau suivant décrit les propriétés des composants dans un flux de données. Certaines propriétés ont des valeurs en lecture seule qui sont assignées au moment de l'exécution par le moteur de flux de données.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|ComponentClassID|String|CLSID du composant.|  
|ContactInfo|String|Informations de contact pour le développeur d'un composant.|  
|Description|String|Description du composant de flux de données. La valeur par défaut de cette propriété est le nom du composant de flux de données.|  
|ID|Entier|Valeur qui identifie de manière unique cette instance du composant.|  
|IdentificationString|String|Identifie le composant.|  
|IsDefaultLocale|Booléen|Indique si le composant utilise les paramètres régionaux de la tâche de flux de données à laquelle il appartient.|  
|LocaleID|Entier|Paramètres régionaux utilisés par le composant de flux de données lors de l'exécution du package. Tous les paramètres régionaux Windows sont disponibles dans les composants de flux de données.|  
|Nom   |String|Nom du composant de flux de données.|  
|PipelineVersion|Entier|Version de la tâche de flux de données dans laquelle un composant est destiné à être exécuté.|  
|UsesDispositions|Booléen|Indique si un composant a une sortie d'erreur.|  
|ValidateExternalMetadata|Booléen|Indique si les métadonnées des colonnes externes sont validées. La valeur par défaut de cette propriété est **True**.|  
|Options de version|Entier|Version d'un composant.|  
  
###  <a name="inputs"></a> Propriétés des entrées  
 Dans le modèle objet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , les transformations et destinations ont des sorties. Une entrée d'un composant dans le flux de données implémente l'interface <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInput100>.  
  
 Le tableau suivant décrit les propriétés des entrées de composants dans un flux de données. Certaines propriétés ont des valeurs en lecture seule qui sont assignées au moment de l'exécution par le moteur de flux de données.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|Description|String|Description de l'entrée.|  
|ErrorOrTruncationOperation|String|Chaîne facultative qui spécifie les types d'erreurs ou troncations qui peuvent se produire lors du traitement d'une ligne.|  
|ErrorRowDisposition|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSRowDisposition>|Valeur qui spécifie la gestion des erreurs. Ces valeurs sont **Fail component**, **Ignore failure**et **Redirect row**.|  
|HasSideEffects|Booléen|Indique si un composant peut être supprimé du plan d'exécution du flux de données lorsqu'il n'est pas attaché à un composant en aval et lorsque la propriété **RunInOptimizedMode** a la valeur **true**.|  
|ID|Entier|Valeur qui identifie l'entrée de façon unique.|  
|IdentificationString|String|Chaîne qui identifie l'entrée.|  
|IsSorted|Booléen|Indique si les données dans l'entrée sont triées.|  
|Nom   |String|Nom de l'entrée.|  
|SourceLocale|Entier|ID de paramètres régionaux (LCID) des données d'entrée.|  
|TruncationRowDisposition|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSRowDisposition>|Valeur qui détermine la façon dont le composant gère les troncations qui se produisent lors du traitement des lignes. . Ces valeurs sont **Fail component**, **Ignore failure**et **Redirect row**.|  
  
 Les destinations et certaines transformations ne prennent pas en charge les sorties d’erreur, et les propriétés ErrorRowDisposition et TruncationRowDisposition de ces composants sont en lecture seule.  
  
###  <a name="inputcolumns"></a> Propriétés des colonnes d’entrée  
 Dans le modèle objet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , une entrée contient une collection de colonnes d'entrée. Une colonne d'entrée d'un composant dans le flux de données implémente l'interface <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInputColumn100>.  
  
 Le tableau suivant décrit les propriétés des colonnes d'entrée de composants dans un flux de données. Certaines propriétés ont des valeurs en lecture seule qui sont assignées au moment de l'exécution par le moteur de flux de données.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|ComparisonFlags|Entier|Ensemble d'indicateurs qui spécifient la comparaison des colonnes ayant un type de données character. Pour plus d’informations, consultez [Comparing String Data](../../integration-services/data-flow/comparing-string-data.md).|  
|Description|String|Décrit la colonne d'entrée.|  
|ErrorOrTruncationOperation|String|Chaîne facultative qui spécifie les types d'erreurs ou troncations qui peuvent se produire lors du traitement d'une ligne.|  
|ErrorRowDisposition|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSRowDisposition>|Valeur qui spécifie la gestion des erreurs. Ces valeurs sont **Fail component**, **Ignore failure**et **Redirect row**.|  
|ExternalMetadataColumnID|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSExternalMetadataColumn100>|ID de la colonne de métadonnées externe assigné à une colonne d'entrée.|  
|ID|Entier|Valeur qui identifie la colonne d'entrée de façon unique.|  
|IdentificationString|String|Chaîne qui identifie la colonne d'entrée.|  
|LineageID|Entier|ID de la colonne en amont.|  
|LineageIdentificationString|String|Chaîne d’identification qui inclut le nom de la colonne en amont.|  
|Nom   |String|Nom de la colonne d'entrée.|  
|SortKeyPosition|Entier|Valeur qui indique si une colonne est triée, son ordre de tri et l'ordre dans lequel plusieurs colonnes sont triées. La valeur **0** indique que la colonne n'est pas triée.  Pour plus d’informations, consultez [Trier des données pour les transformations de fusion et de jointure de fusion](../../integration-services/data-flow/transformations/sort-data-for-the-merge-and-merge-join-transformations.md).|  
|TruncationRowDisposition|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSRowDisposition>|Valeur qui détermine la façon dont le composant gère les troncations qui se produisent lors du traitement des lignes. Ces valeurs sont **Fail component**, **Ignore failure**et **Redirect row**.|  
|UpstreamComponentName|String|Nom du composant en amont.|  
|UsageType|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSUsageType>|Valeur qui détermine la façon dont une colonne d'entrée est utilisée par le composant.|  
  
 Les propriétés de type de données des colonnes d'entrée sont également décrites sous « Propriétés du type de données ».  
  
###  <a name="outputs"></a> Propriétés des sorties  
 Dans le modèle objet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , les sources et les transformations ont des sorties. Une sortie d'un composant dans le flux de données implémente l'interface <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100>.  
  
 Le tableau suivant décrit les propriétés des sorties de composants dans un flux de données. Certaines propriétés ont des valeurs en lecture seule qui sont assignées au moment de l'exécution par le moteur de flux de données.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|DeleteOutputOnPathDetached|Booléen|Valeur qui détermine si le moteur de flux de données supprime la sortie lorsqu'elle est détachée d'un chemin d'accès.|  
|Description|String|Décrit la sortie.|  
|ErrorOrTruncationOperation|String|Chaîne facultative qui spécifie les types d'erreurs ou troncations qui peuvent se produire lors du traitement d'une ligne.|  
|ErrorRowDisposition|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSRowDisposition>|Valeur qui spécifie la gestion des erreurs. Ces valeurs sont **Fail component**, **Ignore failure**et **Redirect row**.|  
|ExclusionGroup|Entier|Valeur qui identifie un groupe de sorties s'excluant mutuellement.|  
|HasSideEffects|Booléen|Valeur qui indique si un composant peut être supprimé du plan d'exécution du flux de données lorsqu'il n'est pas attaché à un composant en amont et lorsque la propriété **RunInOptimizedMode** a la valeur **true**.|  
|ID|Entier|Valeur qui identifie la sortie de façon unique.|  
|IdentificationString|String|Chaîne qui identifie la sortie.|  
|IsErrorOut|Booléen|Indique si la sortie est une sortie d'erreur.|  
|IsSorted|Booléen|Indique si la sortie est triée. La valeur par défaut est **False**.<br /><br /> **\*\* Important \*\*** L’affectation de la valeur **True** à la propriété **IsSorted** ne permet pas de trier les données. Cette propriété indique uniquement aux composants en aval que les données ont été précédemment triées. Pour plus d’informations, consultez [Trier des données pour les transformations de fusion et de jointure de fusion](../../integration-services/data-flow/transformations/sort-data-for-the-merge-and-merge-join-transformations.md).|  
|Nom   |String|Nom de la sortie.|  
|SynchronousInputID|Entier|ID d'une entrée synchrone avec la sortie.|  
|TruncationRowDisposition|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSRowDisposition>|Valeur qui détermine la façon dont le composant gère les troncations qui se produisent lors du traitement des lignes. Ces valeurs sont **Fail component**, **Ignore failure**et **Redirect row**.|  
  
###  <a name="outputcolumns"></a> Propriétés des colonnes de sortie  
 Dans le modèle objet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , une sortie contient une collection de colonnes de sortie. Une colonne de sortie d'un composant dans le flux de données implémente l'interface <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100>.  
  
 Le tableau suivant décrit les propriétés des colonnes de sortie de composants dans un flux de données. Certaines propriétés ont des valeurs en lecture seule qui sont assignées au moment de l'exécution par le moteur de flux de données.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|ComparisonFlags|Entier|Ensemble d'indicateurs qui spécifient la comparaison des colonnes ayant un type de données character. Pour plus d’informations, consultez [Comparing String Data](../../integration-services/data-flow/comparing-string-data.md).|  
|Description|String|Décrit la colonne de sortie.|  
|ErrorOrTruncationOperation|String|Chaîne facultative qui spécifie les types d'erreurs ou troncations qui peuvent se produire lors du traitement d'une ligne.|  
|ErrorRowDisposition|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSRowDisposition>|Valeur qui spécifie la gestion des erreurs. Ces valeurs sont **Fail component**, **Ignore failure**et **Redirect row**. La valeur par défaut est **Composant défaillant**.|  
|ExternalMetadataColumnID|Entier|ID de la colonne de métadonnées externe assigné à une colonne d'entrée.|  
|ID|Entier|Valeur qui identifie la colonne de sortie de façon unique.|  
|IdentificationString|String|Chaîne qui identifie la colonne de sortie.|  
|LineageID|Entier|ID de la colonne de sortie. Les composants en aval font référence à la colonne à l'aide de cette valeur.|  
|LineageIdentificationString|String|Chaîne d’identification qui inclut le nom de la colonne.|  
|Nom   |String|Nom de la colonne de sortie.|  
|SortKeyPosition|Entier|Valeur qui indique si une colonne est triée, son ordre de tri et l'ordre dans lequel plusieurs colonnes sont triées. La valeur **0** indique que la colonne n'est pas triée. Pour plus d’informations, consultez [Trier des données pour les transformations de fusion et de jointure de fusion](../../integration-services/data-flow/transformations/sort-data-for-the-merge-and-merge-join-transformations.md).|  
|SpecialFlags|Entier|Valeur qui contient les indicateurs spéciaux de la colonne de sortie.|  
|TruncationRowDisposition|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSRowDisposition>|Valeur qui détermine la façon dont le composant gère les troncations qui se produisent lors du traitement des lignes. Ces valeurs sont **Fail component**, **Ignore failure**et **Redirect row**. La valeur par défaut est **Composant défaillant**.|  
  
 Les colonnes de sortie incluent également un jeu de propriétés de type de données.  
  
### <a name="external-metadata-column-properties"></a>Propriétés des colonnes de métadonnées externes  
 Dans le modèle objet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , les entrées et sorties peuvent contenir une collection de colonnes de métadonnées externes. Une colonne de métadonnées externe d'un composant dans le flux de données implémente l'interface <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSExternalMetadataColumn100>.  
  
 Le tableau suivant décrit les propriétés des colonnes de métadonnées externes de composants dans un flux de données. Certaines propriétés ont des valeurs en lecture seule qui sont assignées au moment de l'exécution par le moteur de flux de données.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|Description|String|Décrit la colonne externe.|  
|ID|Entier|Valeur qui identifie la colonne de façon unique.|  
|IdentificationString|String|Chaîne qui identifie la colonne.|  
|Nom   |String|Nom de la colonne externe.|  
  
 Les colonnes de métadonnées externes incluent également un jeu de propriétés de type de données.  
  
### <a name="data-type-properties"></a>Propriétés de type de données  
 Les colonnes de sortie et les colonnes de métadonnées externes incluent un jeu de propriétés de type de données. Selon le type de données de la colonne, les propriétés peuvent être en lecture/écriture ou en lecture seule.  
  
 Le tableau suivant décrit les propriétés de type de données des colonnes de sortie et des colonnes de métadonnées externes.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|CodePage|Entier|Spécifie la page de codes pour les données de chaîne qui ne sont pas Unicode.|  
|DataType|Integer (énumération)|Type de données [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] de la colonne. Pour plus d'informations, consultez [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).|  
|Longueur|Entier|Longueur d'une colonne en caractères.|  
|Précision|Entier|Précision d'une colonne numérique.|  
|Échelle|Entier|Échelle d'une colonne numérique.|  

## <a name="custom-properties-of-data-flow-components"></a>Propriétés personnalisées des composants de flux de données
Pour plus d’informations sur les propriétés personnalisées, consultez les rubriques suivantes :  
  
-   [Propriétés personnalisées ADO NET](../../integration-services/data-flow/ado-net-custom-properties.md)  
  
-   [Propriétés personnalisées de la tâche de contrôle de capture de données modifiées](../../integration-services/control-flow/cdc-control-task-custom-properties.md)  
  
-   [Propriétés personnalisées des sources CDC](../../integration-services/data-flow/cdc-source-custom-properties.md)  
  
-   [Propriétés personnalisées de la destination d’apprentissage du modèle d’exploration de données](../../integration-services/data-flow/data-mining-model-training-destination-custom-properties.md)  
  
-   [Propriétés personnalisées de la destination DataReader](../../integration-services/data-flow/datareader-destination-custom-properties.md)  
  
-   [Propriétés personnalisées de la destination de traitement de dimension](../../integration-services/data-flow/dimension-processing-destination-custom-properies.md)  
  
-   [Propriétés personnalisées d’Excel](../../integration-services/data-flow/excel-custom-properties.md)  
  
-   [Propriétés personnalisées des fichiers plats](../../integration-services/data-flow/flat-file-custom-properties.md)  
  
-   [Propriétés personnalisées des destinations ODBC](../../integration-services/data-flow/odbc-destination-custom-properties.md)  
  
-   [Propriétés personnalisées des sources ODBC](../../integration-services/data-flow/odbc-source-custom-properties.md)  
  
-   [OLE DB Custom Properties](../../integration-services/data-flow/ole-db-custom-properties.md)Propriétés personnalisées OLE DB  
  
-   [Propriétés personnalisées de la destination de traitement de partition](../../integration-services/data-flow/partition-processing-destination-custom-properties.md)  
  
-   [Propriétés personnalisées des fichiers bruts](../../integration-services/data-flow/raw-file-custom-properties.md)  
  
-   [Propriétés personnalisées de la destination du jeu d’enregistrements](../../integration-services/data-flow/recordset-destination-custom-properties.md)  
  
-   [Propriétés personnalisées de la destination SQL Server Compact Edition](../../integration-services/data-flow/sql-server-compact-edition-destination-custom-properties.md)  
  
-   [Propriétés personnalisées de la destination SQL Server](../../integration-services/data-flow/sql-server-destination-custom-properties.md)  
  
-   [Propriétés personnalisées des transformations](../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
-   [Propriétés personnalisées des sources XML](../../integration-services/data-flow/xml-source-custom-properties.md)  
  
## <a name="use-an-expression-in-a-data-flow-component"></a>Utiliser une expression dans un composant de flux de données
Cette procédure permet d'ajouter une expression à la transformation de fractionnement conditionnel ou à la transformation de colonne dérivée. La transformation de fractionnement conditionnel utilise des expressions pour définir les conditions qui dirigent les lignes de données vers une sortie de transformation et la transformation de colonne dérivée utilise des expressions pour définir les valeurs affectées aux colonnes.  
  
 Pour implémenter une expression dans une transformation, le package doit déjà inclure au moins une tâche de flux de données et une source. 
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur l’onglet **Flux de contrôle** , puis cliquez sur la tâche de flux de données contenant le flux de données dans lequel vous voulez implémenter une expression.  
  
4.  Cliquez sur l’onglet **Flux de données** , et faites glisser une transformation de fractionnement conditionnel ou de colonne dérivée de la **Boîte à outils** jusqu’à l’aire de conception.  
  
5.  Faites glisser le connecteur vert de la source ou d'une transformation vers la transformation de fractionnement conditionnel ou de colonne dérivée.  
  
6.  Double-cliquez sur la transformation pour ouvrir sa boîte de dialogue.  
  
7.  Dans le volet gauche, développez **Variables** pour afficher les variables système et définies par l’utilisateur, et développez **Colonnes** pour afficher les colonnes d’entrée de transformation.  
  
8.  Dans le volet droit, développez **Fonctions mathématiques**, **Fonctions de chaîne**, **Fonctions de date et d’heure**, **Fonctions NULL**, **Casts de type**et **Opérateurs** pour accéder aux fonctions, conversions et opérateurs fournis par la grammaire d’expression.  
  
9. Selon la transformation, effectuez l'une des opérations suivantes pour générer une expression :  
  
    -   Dans la boîte de dialogue **Éditeur de transformation de fractionnement conditionnel** , faites glisser les variables, colonnes, fonctions, conversions et opérateurs vers la colonne **Condition** . Vous pouvez également aussi taper une expression directement dans la colonne **Condition** .  
  
    -   Dans la boîte de dialogue **Éditeur de transformation de colonne dérivée** , faites glisser les variables, colonnes, fonctions, conversions et opérateurs vers la colonne **Expression** . Vous pouvez également taper une expression directement dans la colonne **Expression** .  
  
        > [!NOTE]  
        >  Quand la colonne **Condition** ou **Expression** n’est plus active, le texte de l’expression peut être mis en surbrillance, ce qui indique que la syntaxe de l’expression est incorrecte.  
  
10. Cliquez sur **OK** pour quitter la boîte de dialogue.  
  
    > [!NOTE]  
    >  Si l'expression n'est pas valide, une alerte apparaît et décrit les erreurs de syntaxe de l'expression.  

## <a name="data-flow-properties-that-you-can-set-with-an-expression"></a>Propriétés de flux de données que vous pouvez définir avec une expression
Les valeurs de certaines propriétés d'objets de flux de données peuvent être spécifiées à l'aide d'expressions de propriété disponibles sur le conteneur de tâche de flux de données.  
  
 Pour plus d’informations sur l’utilisation d’expressions de propriété, consultez [Expressions de propriété dans des packages](../../integration-services/expressions/use-property-expressions-in-packages.md).  
  
 Vous pouvez utiliser des expressions de propriété pour personnaliser les configurations de chaque instance déployée d'un package. Vous pouvez également utiliser des expressions de propriété pour spécifier des contraintes d’exécution pour un package à l’aide de l’option **/set** avec l’utilitaire d’invite de commandes **dtexec** . Par exemple, vous pouvez limiter le nombre maximal de threads ( **MaximumThreads** ) utilisés par la transformation de tri ou l’utilisation maximale de la mémoire ( **MaxMemoryUsage** des transformations de regroupement probable et de recherche floue. Si elles sont libres, ces transformations peuvent mettre en cache de grandes quantités de données en mémoire.  
  
 Pour spécifier une expression de propriété pour une des propriétés d’objets de flux de données répertoriées dans cette rubrique, affichez la fenêtre **Propriétés** pour la tâche de flux de données en la sélectionnant sur l’aire **Flux de contrôle** du concepteur ou en sélectionnant l’onglet **Flux de données** du concepteur sans sélectionner de composant ou de chemin individuel. Sélectionnez la propriété **Expressions** , puis cliquez sur les points de suspension (...) pour afficher la boîte de dialogue de **l’Éditeur d’expressions de la propriété** . Déroulez la liste **Propriété** pour sélectionner une propriété, puis entrez une expression dans la zone de texte **Expression** ou cliquez sur les points de suspension (...) pour afficher la boîte de dialogue **Générateur d’expressions** .  
  
 La liste **Propriété** affiche les propriétés disponibles uniquement pour les objets de flux de données que vous avez déjà placés sur l’aire **Flux de données** du concepteur. Par conséquent, vous ne pouvez pas utiliser la liste **Propriété** pour consulter toutes les propriétés possibles des objets de flux de données qui prennent en charge les expressions de propriété. Par exemple, si vous avez placé une source ADO.NET sur l’aire du concepteur, la liste **Propriété** contient une entrée pour la propriété **[ADO NET Source].[SqlCommand]** . La liste affiche également de nombreuses propriétés de la tâche de flux de données elle-même.  
 
 Les valeurs des propriétés de la liste suivante peuvent être spécifiées à l'aide d'expressions de propriété.  
  
### <a name="data-flow-sources"></a>Sources de flux de données  
  
|Objet de flux de données|Propriété|  
|----------------------|--------------|  
|Source ADO NET|Propriété TableOrViewName<br /><br /> Propriété SQLCommand|  
|Source XML|Propriété XMLData<br /><br /> Propriété XMLSchemaDefinition|  
  
### <a name="data-flow-transformations"></a>Transformations du flux de données  
 Pour plus d’informations sur ces propriétés personnalisées, consultez [Propriétés personnalisées des transformations](../../integration-services/data-flow/transformations/transformation-custom-properties.md).  
  
|Objet de flux de données|Propriété|  
|----------------------|--------------|  
|transformation de fractionnement conditionnel|Propriété FriendlyExpression|  
|Transformation de colonnes dérivées|Propriété FriendlyExpression|  
|Transformation de regroupement approximatif|Propriété MaxMemoryUsage|  
|transformation de recherche floue|Propriété MaxMemoryUsage|  
|Transformation de recherche|Propriété SQLCommand<br /><br /> Propriété SqlCommandParam|  
|transformation de commande OLE DB|Propriété SQLCommand|  
|transformation de l'échantillonnage du pourcentage|Propriété SamplingValue|  
|transformation de tableau croisé dynamique|Propriété PivotKeyValue|  
|transformation d'échantillonnage de lignes|Propriété SamplingValue|  
|transformation de tri|Propriété MaximumThreads|  
|Transformation Unpivot|Propriété PivotKeyValue|  
  
### <a name="data-flow-destinations"></a>Destinations du flux de données  
  
|Objet de flux de données|Propriété|  
|----------------------|--------------|  
|Destination ADO NET|Propriété TableOrViewName<br /><br /> Propriété BatchSize<br /><br /> Propriété CommandTimeout|  
|Destination de fichier plat|Propriété Header|  
|Destination [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact|Propriété TableName|  
|Destination [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Propriété BulkInsertTableName<br /><br /> Propriété BulkInsertFirstRow<br /><br /> Propriété BulkInsertLastRow<br /><br /> Propriété BulkInsertOrder<br /><br /> Propriété Timeout|  

