---
title: Modifier les propriétés dans une vue de Source de données (Analysis Services) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 7ec8840a9d4f66247c41466a6d32c7dd6eee7de6
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="change-properties-in-a-data-source-view-analysis-services"></a>Modifier les propriétés d'une vue de source de données (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Après avoir défini une vue de source de données à l'aide de l'Assistant Vue de source de données, puis ajouté des tables, des vues, des calculs nommés et des requêtes nommées à la vue de source de données, vous pouvez modifier les propriétés en rapport avec :  
  
-   les critères de correspondance de vues de source de données ;  
  
-   les options avancées de vue de source de données ;  
  
-   les noms d'objets ;  
  
-   les métadonnées d'objets.  
  
 Vous pouvez également consulter les métadonnées d'objets récupérées dans la source de données et qui ne peuvent pas être modifiées.  
  
## <a name="viewing-or-changing-data-source-view-properties"></a>Consultation ou modification des propriétés d'une vue de source de données  
 Les propriétés d'une vue de source de données, autres que la description de la vue de source de données, sont définies par l'Assistant Vue de source de données lorsque vous définissez initialement la vue de source de données. Le tableau suivant répertorie et décrit les propriétés d'une vue de source de données.  
  
> [!NOTE]  
>  Le volet des propriétés affiche les propriétés du fichier .dsv, ainsi que de l'objet DSV. Pour afficher les propriétés de l'objet, double-cliquez dessus dans l'Explorateur de solutions. Les propriétés seront mises à jour afin de refléter les propriétés que vous voyez dans le tableau suivant.  
  
|Propriété|Description|  
|--------------|-----------------|  
|Source de données|Indique la source de données dans la vue de source de données dont vous consultez les propriétés|  
|Description|Fournit la description de la vue de source de données.|  
|Nom|Indique le nom de la vue de source de données qui apparaît dans l'Explorateur de solutions ou la base de données Analysis Services. Vous pouvez modifier le nom de la vue de source de données ici ou dans l'Explorateur de solutions.|  
|NameMatchingCriteria|Critères de correspondance de noms pour la source de données. La valeur par défaut est (none) si des relations clé primaire/clé étrangère ont été détectées par l'Assistant Vue de source de données. Que cette propriété ait été définie ou non par l'Assistant Vue de source de données, vous pouvez spécifier une valeur ici. S'il existe des relations de base de données et que vous spécifiez des critères de correspondance de noms, les deux seront utilisés pour déterminer les relations entre les tables existantes et les nouvelles tables ajoutées.|  
|RetrieveRelationships|Indique si les relations sont récupérées dans la base de données. La valeur par défaut est True.|  
|SchemaRestriction|Indique les restrictions, le cas échéant, sur les schémas récupérés dans une source de données. Par défaut, aucune restriction de schéma n'est définie.|  
  
## <a name="viewing-or-changing-datatable-properties"></a>Consultation ou modification des propriétés DataTable  
 Les propriétés**DataTable** sont les propriétés des tables, des vues et des requêtes nommées dans une vue de source de données. Ces propriétés sont définies lorsque n'importe lequel des objets est ajouté à la vue de source de données. Le tableau suivant répertorie et décrit les propriétés des objets **DataTable** d’une vue de source de données.  
  
|Propriété|Description|  
|--------------|-----------------|  
|AllowChangesDuringGeneration|Indique si l'Assistant Génération de schéma dispose d'autorisations pour remplacer une table de vue de source de données au cours de la régénération. Cette propriété existe seulement sur les tables générées initialement par l'Assistant Génération de schéma. Pour plus d’informations, consultez [Présentation de la génération incrémentielle](../../analysis-services/multidimensional-models/understanding-incremental-generation.md).|  
|DataSource|Définit la source de données de l'objet. Vous ne pouvez pas modifier cette propriété.|  
|Description|Fournit la description de la table, la vue ou la requête nommée. Si la vue ou la table de la base de données sous-jacente dispose d'une description stockée comme propriété étendue, cette valeur s'affiche. Vous pouvez modifier cette propriété.|  
|FriendlyName|Indique un nom de table ou de vue plus facile à comprendre ou plus approprié pour la zone de sujet. Par défaut, la valeur de la propriété **FriendlyName** d’une table ou d’une vue est identique à celle de la propriété **Name** correspondante. La propriété **FriendlyName** est utilisée par des objets OLAP et d’exploration de données lors de la définition de noms d’objets basés sur des tables ou des vues. Vous pouvez modifier cette propriété.|  
|Nom|Indique le nom de la vue ou de la table sous-jacente, ou le nom de la requête nommée. La propriété **Name** est utilisée par des objets OLAP et d’exploration de données lors de la définition de noms d’objets basés sur des requêtes nommées. Vous pouvez modifier cette propriété uniquement pour les requêtes nommées.|  
|QueryDefinition|Définit une requête nommée. Cette propriété ne s'applique qu'aux requêtes nommées et vous ne pouvez pas la modifier directement. Pour modifier cette propriété, vous devez modifier la requête nommée.|  
|Schéma|Indique le schéma de base de données applicable à la table, la vue ou la requête nommée. Cette propriété n’est pas modifiable.|  
|TableType|Indique le type de table de la table, de la vue ou de la requête nommée. Cette propriété n’est pas modifiable.|  
  
## <a name="viewing-or-changing-datacolumn-properties"></a>Consultation ou modification des propriétés DataColumn  
 Les propriétés**DataColumn** sont les propriétés des colonnes de tables, de vues et de requêtes nommées dans une vue de source de données. Ces propriétés sont définies lorsque n'importe lequel de ces objets est ajouté à la vue de source de données, soit à partir de la vue ou de la table sous-jacente, soit à partir d'une requête nommée, soit tel que défini par un calcul nommé. Le tableau suivant répertorie et décrit les propriétés des objets **DataColumn** d’une vue de source de données.  
  
|Propriété|Description|  
|--------------|-----------------|  
|AllowNull|Spécifie la propriété NULL de la colonne en fonction de la colonne dans la table sous-jacente, d'une valeur ou d'une requête nommée. Cette propriété n’est pas modifiable.|  
|DataType|Spécifie le type de données de la colonne en fonction de la colonne dans la table sous-jacente, d'une valeur ou d'une requête nommée. Vous ne pouvez pas modifier directement cette propriété. Cependant, si nécessaire, pour remplacer le type de données d'une colonne dans une table ou une vue, remplacez la table par une requête nommée qui convertit la colonne en type de données souhaité.|  
|DateTimeMode|Indique le format de sérialisation de la date pour les colonnes **DateTime** . La valeur par défaut est **UnspecifiedLocal**. Cette propriété est modifiable.|  
|Description|Fournit la description de la colonne. Si la colonne de base de données sous-jacente dispose d'une description stockée comme propriété étendue, cette valeur s'affiche. Vous pouvez modifier cette propriété.|  
|FriendlyName|Indique un nom de colonne dans une table ou une vue plus facile à comprendre ou plus approprié pour la zone de sujet. Par défaut, la valeur de la propriété **FriendlyName** d’une colonne dans une table ou une vue est identique à celle de la propriété **Name** correspondante. La propriété **FriendlyName** est utilisée par des objets OLAP et d’exploration de données lors de la définition d’attributs basés sur des colonnes de tables ou de vues. Vous pouvez modifier cette propriété.|  
|Longueur|Indique la longueur maximale de la colonne, en fonction des données de la colonne dans la vue ou la table sous-jacente.|  
|Nom|Indique le nom de la colonne sous-jacente, ou le nom du calcul nommé. La propriété **Name** est utilisée par des objets OLAP et d’exploration de données lors de la définition d’attributs basés sur des calculs nommés. Vous pouvez modifier cette propriété uniquement pour les calculs nommés.|  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de sources de données dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/data-source-views-in-multidimensional-models.md)   
 [Utiliser des diagrammes dans le Concepteur de vue de Source de données & #40 ; Analysis Services & #41 ;](../../analysis-services/multidimensional-models/work-with-diagrams-in-data-source-view-designer-analysis-services.md)  
  
  
