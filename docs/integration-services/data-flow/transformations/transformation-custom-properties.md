---
title: Propriétés personnalisées des transformations | Microsoft Docs
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
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- Aggregate transformation [Integration Services]
- Slowly Changing Dimension transformation
- Import Column transformation [Integration Services]
- Sort transformation
- Unpivot transformation
- Merge Join transformation
- Data Mining Query transformation
- Fuzzy Grouping transformation
- Data Conversion transformation
- Fuzzy Lookup transformation
- Term Extraction transformation
- Row Count transformation custom properties [Integration Services]
- transformations [Integration Services], properties
- Pivot transformation
- Lookup transformation
- Percentage Sampling transformation
- Export Column transformation [Integration Services]
- Row Sampling transformation
- Conditional Split transformation custom properties [Integration Services]
- custom properties [Integration Services]
- Audit transformation
- Term Lookup transformation
- Script Component transformation custom properties [Integration Services]
- Derived Column transformation
- OLE DB Command transformation
- Copy Column transformation custom properties [Integration Services]
- Character Map transformation custom properties [Integration Services]
ms.assetid: 56f5df6a-56f6-43df-bca9-08476a3bd931
caps.latest.revision: 72
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 64627ae61041de97eba4101cf149e5d073aa0d55
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="transformation-custom-properties"></a>Propriétés personnalisées des transformations
  En plus des propriétés qui sont communes à la plupart des objets de flux de données dans le modèle objet [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] , de nombreux objets de flux de données possèdent des propriétés personnalisées qui sont spécifiques à l'objet. Ces propriétés personnalisées sont uniquement disponibles au moment de l'exécution et ne sont pas documentées dans la documentation de référence de la programmation managée de [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] .  
  
 Cette rubrique présente et décrit les propriétés personnalisées des différentes transformations de flux de données. Pour plus d'informations sur les propriétés communes à la plupart des objets de flux de données, consultez [Common Properties](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796).  
  
 Certaines propriétés de transformations peuvent être définies à l'aide d'expressions de propriété. Pour plus d’informations, consultez [Propriétés du flux de données pouvant être définies à l’aide d’expressions](http://msdn.microsoft.com/library/cd0e171a-08be-45d6-81dc-ed94f37698b8).  
  
## <a name="transformations-with-custom-properties"></a>Transformations avec les propriétés personnalisées  
  
||||  
|-|-|-|  
|[Agrégat](#aggregate)|[Exportation de colonne](#extract)|[Nombre de lignes](#rowcount)|  
|[Auditer](#audit)|[Regroupement probable](#fgroup)|[Échantillonnage de lignes](#rowsamp)|  
|[Transformation du cache](#cachetransform)|[Recherche floue](#flookup)|[Composant Script](#script)|  
|[Table des caractères](#charmap)|[Importation de colonne](#insert)|[Dimension à variation lente](#scd)|  
|[Fractionnement conditionnel](#condsplit)|[Lookup](#lookup)|[Trier](#sort)|  
|[Copie de colonnes](#copymap)|[Merge Join](#mjoin)|[Extraction de terme](#textract)|  
|[Conversion de données](#dataconv)|[Commande OLE DB](#oledbcmd)|[Recherche de terme](#tlookup)|  
|[Requête d'exploration de données](#dmquery)|[Échantillonnage du pourcentage](#percent)|[Supprimer le tableau croisé dynamique](#unpivot)|  
|[Colonne dérivée](#derived)|[Tableau croisé dynamique](#pivot)||  
  
### <a name="transformations-without-custom-properties"></a>Transformations sans propriétés personnalisées  
 Les transformations suivantes ne disposent pas de propriétés personnalisées au niveau du composant, de l'entrée ou de la sortie : [Merge Transformation](../../../integration-services/data-flow/transformations/merge-transformation.md), [Multicast Transformation](../../../integration-services/data-flow/transformations/multicast-transformation.md)et [Union All Transformation](../../../integration-services/data-flow/transformations/union-all-transformation.md). Ils font uniquement appel aux propriétés communes à l'ensemble des composants de flux de données.  
  
##  <a name="aggregate"></a> Transformation d'agrégation, propriétés personnalisées  
 La transformation d'agrégation dispose à la fois de propriétés personnalisées et de propriétés communes à l'ensemble des composants de flux de données.  
  
 Le tableau suivant décrit les propriétés personnalisées de la transformation d'agrégation. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|AutoExtendFactor|Entier|Valeur entre 1 et 100 qui spécifie le pourcentage d'extension possible de la mémoire lors de l'opération d'agrégation. La valeur par défaut de cette propriété est de **25**.|  
|CountDistinctKeys|Entier|Valeur précisant le nombre exact de comptages de valeurs que l'agrégation peut écrire. Si une valeur CountDistinctScale est spécifiée, la valeur dans CountDistinctKeys est prioritaire.|  
|CountDistinctScale|Integer (énumération)|Valeur qui décrit le nombre approximatif de valeurs distinctes que peut totaliser l'agrégation dans une colonne. Cette propriété peut prendre les valeurs suivantes :<br /><br /> **Bas** (1) : indique jusqu'à 500 000 valeurs de clés.<br /><br /> **Moyen** (2) : indique jusqu'à 5 millions de valeurs de clés.<br /><br /> **Haut** (3) : indique plus de 25 millions de valeurs de clés.<br /><br /> **Non spécifié** (0) : indique qu’aucune valeur CountDistinctScale n’est utilisée. L’utilisation de l’option **Non spécifié** (0) peut avoir un impact sur les performances dans les jeux de données volumineux.|  
|Keys|Entier|Valeur qui spécifie le nombre exact de clés GROUP BY écrites par l'agrégation. Si une valeur KeyScalevalue est spécifiée, la valeur dans Keys est prioritaire.|  
|KeyScale|Integer (énumération)|Valeur qui décrit le nombre approximatif de valeurs de clés GROUP BY que l'agrégation peut écrire. Cette propriété peut prendre les valeurs suivantes :<br /><br /> **Bas** (1) : indique jusqu'à 500 000 valeurs de clés.<br /><br /> **Moyen** (2) : indique jusqu'à 5 millions de valeurs de clés.<br /><br /> **Haut** (3) : indique plus de 25 millions de valeurs de clés.<br /><br /> **Non spécifié** (0) : indique qu’aucune valeur KeyScale n’est utilisée.|  
  
 Le tableau suivant décrit les propriétés personnalisées de la sortie de la transformation d'agrégation. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|Keys|Entier|Valeur qui spécifie le nombre exact de clés GROUP BY que l'agrégation peut écrire. Si une valeur KeyScale est spécifiée, la valeur dans Keys est prioritaire.|  
|KeyScale|Integer (énumération)|Valeur qui décrit le nombre approximatif de valeurs de clés GROUP BY que l'agrégation peut écrire. Cette propriété peut prendre les valeurs suivantes :<br /><br /> **Bas** (1) : indique jusqu'à 500 000 valeurs de clés.<br /><br /> **Moyen** (2) : indique jusqu'à 5 millions de valeurs de clés.<br /><br /> **Haut** (3) : indique plus de 25 millions de valeurs de clés.<br /><br /> **Non spécifié** (0) : indique qu’aucune valeur KeyScale n’est utilisée.|  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes de sortie de la transformation d'agrégation. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|AggregationColumnId|Entier|**LineageID** d'une colonne participant à des fonctions GROUP BY ou d'agrégation.|  
|AggregationComparisonFlags|Entier|Valeur qui spécifie la manière dont la transformation d'agrégation compare les données de chaîne dans une colonne. Pour plus d'informations, voir [Comparing String Data](../../../integration-services/data-flow/comparing-string-data.md).|  
|AggregationType|Integer (énumération)|Valeur qui spécifie l'opération d'agrégation à appliquer à la colonne. Cette propriété peut prendre les valeurs suivantes :<br /><br /> **Group by** (0)<br /><br /> **Count** (1)<br /><br /> **Count all** (2)<br /><br /> **Countdistinct** (3)<br /><br /> **Sum** (4)<br /><br /> **Average** (5)<br /><br /> **Maximum** (7)<br /><br /> **Minimum** (6)|  
|CountDistinctKeys|Entier|Valeur spécifiant le nombre exact de clés que l'agrégation peut écrire lorsque le type d'agrégation est **COUNT DISTINCT**. Si une valeur CountDistinctScale est spécifiée, la valeur dans CountDistinctKeys est prioritaire.|  
|CountDistinctScale|Integer (énumération)|Valeur qui décrit le nombre approximatif de valeurs de clés que peut écrire l'agrégation lorsque le type d'agrégation est **COUNT DISTINCT**. Cette propriété peut prendre les valeurs suivantes :<br /><br /> **Bas** (1) : indique jusqu'à 500 000 valeurs de clés.<br /><br /> **Moyen** (2) : indique jusqu'à 5 millions de valeurs de clés.<br /><br /> **Haut** (3) : indique plus de 25 millions de valeurs de clés.<br /><br /> **Non spécifié** (0) : indique qu’aucune valeur CountDistinctScale n’est utilisée.|  
|IsBig|Booléen|Valeur qui indique si la colonne contient une valeur supérieure à 4 milliards ou une valeur avec une meilleure précision qu'une valeur à virgule flottante double précision. La valeur peut être 0 ou 1. 0 indique que IsBig a la valeur **False** et que la colonne ne contient aucune valeur élevée ou exacte. La valeur par défaut de cette propriété est 1.|  
  
 L'entrée et les colonnes d'entrée de la transformation d'agrégation ne sont pas dotées de propriétés personnalisées.  
  
 Pour plus d’informations, voir [Aggregate Transformation](../../../integration-services/data-flow/transformations/aggregate-transformation.md).  
  
##  <a name="audit"></a> Transformation d'audit, propriétés personnalisées  
 La transformation d'audit dispose uniquement des propriétés communes à l'ensemble des composants de flux de données au niveau du composant.  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes de sortie de la transformation d'audit. Toutes les propriétés sont en lecture/écriture.  
  
|Nom de la propriété|Type de données|Description|  
|-------------------|---------------|-----------------|  
|LineageItemSelected|Integer (énumération)|Élément d'audit sélectionné pour la sortie. Cette propriété peut prendre les valeurs suivantes :<br /><br /> **GUID d’instance d’exécution** (0)<br /><br /> **Heure de début de l’exécution** (4)<br /><br /> **Nom de l’ordinateur** (5)<br /><br /> **ID du package** (1)<br /><br /> **Nom du package** (2)<br /><br /> **ID de la tâche** (8)<br /><br /> **Nom de la tâche** (7)<br /><br /> **Nom d’utilisateur** (6)<br /><br /> **ID de version** (3)|  
  
 L'entrée, les colonnes d'entrée et la sortie de la transformation d'audit ne disposent pas de propriétés personnalisées.  
  
 Pour plus d’informations, voir [Audit Transformation](../../../integration-services/data-flow/transformations/audit-transformation.md).  
  
##  <a name="cachetransform"></a> Transformation de transformation du cache, propriétés personnalisées  
 La transformation de transformation du cache dispose à la fois de propriétés personnalisées et de propriétés communes à l'ensemble des composants de flux de données.  
  
 Le tableau qui suit décrit les propriétés de la transformation de transformation du cache. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|Connectionmanager|String|Spécifie le nom du gestionnaire de connexions.|  
|ValidateExternalMetadata|Booléen|Indique si la transformation du cache est validée à l'aide de sources de données externes au moment de la conception. Si la propriété possède la valeur **False**, la validation avec des sources de données externes a lieu au moment de l'exécution.<br /><br /> La valeur par défaut est **True**.|  
|AvailableInputColumns|String|Liste des colonnes d'entrée disponibles.|  
|InputColumns|String|Liste des colonnes d'entrée sélectionnées.|  
|CacheColumnName|String|Spécifie le nom de la colonne mappée à une colonne d'entrée sélectionnée.<br /><br /> Le nom de la colonne dans la propriété CacheColumnName doit correspondre au nom de la colonne correspondante répertoriée dans la page **Colonnes** de **l’Éditeur du gestionnaire de connexions du cache**.<br /><br /> Pour plus d'informations, consultez [Cache Connection Manager Editor](../../../integration-services/data-flow/transformations/cache-connection-manager-editor.md)|  
  
##  <a name="charmap"></a> Transformation de la table des caractères, propriétés personnalisées  
 La transformation de la table des caractères dispose uniquement des propriétés communes à l'ensemble des composants de flux de données au niveau du composant.  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes de sortie de la transformation de la table des caractères. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|InputColumnLineageId|Entier|Valeur spécifiant le **LineageID** de la colonne d'entrée qui correspond à la source de la colonne de sortie.|  
|MapFlags|Integer (énumération)|Valeur précisant les opérations de chaîne que la transformation de la table des caractères applique dans la colonne. Cette propriété peut prendre les valeurs suivantes :<br /><br /> **Inversion d’octet** (2)<br /><br /> **Pleine chasse** (6)<br /><br /> **Demi-chasse** (5)<br /><br /> **Hiragana** (3)<br /><br /> **Katakana** (4)<br /><br /> **Casse linguistique** (7)<br /><br /> **Minuscules** (0)<br /><br /> **Chinois simplifié** (8)<br /><br /> **Chinois traditionnel**(9)<br /><br /> **Majuscules** (1)|  
  
 L'entrée, les colonnes d'entrée et la sortie de la transformation de la table des caractères ne disposent pas de propriétés personnalisées.  
  
 Pour plus d’informations, voir [Character Map Transformation](../../../integration-services/data-flow/transformations/character-map-transformation.md).  
  
##  <a name="condsplit"></a> Transformation de fractionnement conditionnel, propriétés personnalisées  
 La transformation de fractionnement conditionnel dispose uniquement des propriétés communes à l'ensemble des composants de flux de données au niveau du composant.  
  
 Le tableau suivant décrit les propriétés personnalisées de la sortie de la transformation de fractionnement conditionnel. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|EvaluationOrder|Entier|Valeur qui spécifie la position d'une condition, associée à une sortie, dans la liste des conditions évaluées par la transformation de fractionnement conditionnel. Les conditions sont évaluées dans l'ordre, de la valeur la plus basse à la valeur la plus élevée.|  
|Expression|String|Expression qui représente la condition évaluée par la transformation de fractionnement conditionnel. Les colonnes sont représentées par les identificateurs de lignage.|  
|FriendlyExpression|String|Expression qui représente la condition évaluée par la transformation de fractionnement conditionnel. Les colonnes sont représentées par leurs noms.<br /><br /> Il est possible de spécifier la valeur de cette propriété en utilisant l'expression d'une propriété.|  
|IsDefaultOut|Booléen|Valeur indiquant si la sortie est la sortie par défaut.|  
  
 L'entrée, les colonnes d'entrée et les colonnes de sortie de la transformation de fractionnement conditionnel ne disposent pas de propriétés personnalisées.  
  
 Pour plus d’informations, voir [Conditional Split Transformation](../../../integration-services/data-flow/transformations/conditional-split-transformation.md).  
  
##  <a name="copymap"></a> Transformation de copie de colonne, propriétés personnalisées  
 La transformation de copie de colonne dispose uniquement des propriétés communes à l'ensemble des composants de flux de données au niveau du composant.  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes de sortie de la transformation de copie de colonne. Toutes les propriétés sont en lecture/écriture.  
  
|Nom de la propriété|Type de données|Description|  
|-------------------|---------------|-----------------|  
|copyColumnId|Entier|**LineageID** de la colonne d'entrée à partir de laquelle la colonne de sortie est copiée.|  
  
 L'entrée, les colonnes d'entrée et la sortie de la transformation de copie de colonne ne disposent pas de propriétés personnalisées.  
  
 Pour plus d’informations, voir [Copy Column Transformation](../../../integration-services/data-flow/transformations/copy-column-transformation.md).  
  
##  <a name="dataconv"></a> Transformation de conversion de données, propriétés personnalisées  
 La transformation de conversion de données dispose uniquement des propriétés communes à l'ensemble des composants de flux de données au niveau du composant.  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes de sortie de la transformation de conversion de données. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|FastParse|Booléen|Valeur qui indique si la colonne fait appel aux routines d'analyse fournies par [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] (routines plus rapides mais qui ne tiennent pas compte des paramètres régionaux) ou bien aux routines d'analyse standard qui prennent en compte les paramètres régionaux. La valeur par défaut de cette propriété est **False**. Pour plus d'informations, consultez [Fast Parse](http://msdn.microsoft.com/library/6688707d-3c5b-404e-aa2f-e13092ac8d95) et [Standard Parse](http://msdn.microsoft.com/library/dfe835b1-ea52-4e18-a23a-5188c5b6f013). .<br /><br /> Remarque : cette propriété n’est pas disponible dans l’ **Éditeur de transformation de conversion de données**, mais peut être définie à l’aide de l’ **éditeur avancé**.|  
|SourceInputColumnLineageId|Entier|**LineageID** de la colonne d'entrée correspondant à la source de la colonne de sortie.|  
  
 L'entrée, les colonnes d'entrée et la sortie de la transformation de conversion de données ne disposent pas de propriétés personnalisées.  
  
 Pour plus d’informations, voir [Data Conversion Transformation](../../../integration-services/data-flow/transformations/data-conversion-transformation.md).  
  
##  <a name="dmquery"></a> Transformation de requête d'exploration de données ; propriétés personnalisées  
 La transformation de requête d'exploration de données dispose à la fois de propriétés personnalisées et de propriétés communes à l'ensemble des composants de flux de données.  
  
 Le tableau suivant décrit les propriétés personnalisées de la transformation de requête d'exploration de données. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|ASConnectionId|String|Identificateur unique de l'objet de connexion.|  
|ASConnectionString|String|Chaîne de connexion d'un projet [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] ou d'une base de données [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] .|  
|CatalogName|String|Nom d'une base de données [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] .|  
|ModelName|String|Nom du modèle d'exploration de données.|  
|ModelStructureName|String|Nom de la structure d'exploration de données.|  
|ObjectRef|String|Balise XML qui identifie la structure d'exploration de données utilisée par la transformation.|  
|QueryText|String|Instruction de requête de prédiction utilisée par la transformation.|  
  
 L'entrée, les colonnes d'entrée, la sortie et les colonnes de sortie de la transformation de requête d'exploration de données ne disposent pas de propriétés personnalisées.  
  
 Pour plus d’informations, voir [Data Mining Query Transformation](../../../integration-services/data-flow/transformations/data-mining-query-transformation.md).  
  
##  <a name="derived"></a> Transformation de colonne dérivée, propriétés personnalisées  
 La transformation de colonne dérivée dispose uniquement des propriétés communes à l'ensemble des composants de flux de données au niveau du composant.  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes d'entrée et de sortie de la transformation de colonne dérivée. Lorsque vous ajoutez la colonne dérivée en tant que nouvelle colonne, ces propriétés personnalisées s'appliquent à la nouvelle colonne de sortie ; si vous choisissez de remplacer le contenu d'une colonne d'entrée existante par les résultats de la colonne dérivée, ces propriétés personnalisées s'appliquent à la colonne d'entrée existante. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|Expression|String|Expression qui représente la condition évaluée par la transformation de fractionnement conditionnel. Les colonnes sont représentées par la propriété **LineageID** pour la colonne.|  
|FriendlyExpression|String|Expression qui représente la condition évaluée par la transformation de fractionnement conditionnel. Les colonnes sont représentées par leurs noms.<br /><br /> Il est possible de spécifier la valeur de cette propriété en utilisant l'expression d'une propriété.|  
  
 L'entrée et la sortie de la transformation de colonne dérivée ne sont pas dotées de propriétés personnalisées.  
  
 Pour plus d’informations, voir [Derived Column Transformation](../../../integration-services/data-flow/transformations/derived-column-transformation.md).  
  
##  <a name="extract"></a> Transformation d'exportation de colonne, propriétés personnalisées  
 La transformation d'exportation de colonne dispose uniquement des propriétés communes à l'ensemble des composants de flux de données au niveau du composant.  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes d'entrée de la transformation d'exportation de colonne. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|AllowAppend|Booléen|Valeur spécifiant si la transformation ajoute ou non des données à un fichier existant. La valeur par défaut de cette propriété est **False**.|  
|ForceTruncate|Booléen|Valeur qui spécifie si la transformation tronque un fichier existant avant d'écrire des données. La valeur par défaut de cette propriété est **False**.|  
|FileDataColumnID|Entier|Valeur qui identifie la colonne contenant les données que la transformation insère dans un fichier. Dans la colonne d'extraction, cette propriété a une valeur de **0**; dans la colonne du chemin d'accès, elle contient le **LineageID** de la colonne d'extraction.|  
|WriteBOM|Booléen|Valeur spécifiant si une marque d'ordre d'octet est écrite dans le fichier.|  
  
 L'entrée, la sortie et les colonnes de sortie de la transformation d'exportation de colonne ne disposent pas de propriétés personnalisées.  
  
 Pour plus d’informations, voir [Export Column Transformation](../../../integration-services/data-flow/transformations/export-column-transformation.md).  
  
##  <a name="insert"></a> Transformation d'importation de colonne, propriétés personnalisées  
 La transformation d'importation de colonne dispose uniquement des propriétés communes à l'ensemble des composants de flux de données au niveau du composant.  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes d'entrée de la transformation d'importation de colonne. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|ExpectBOM|Booléen|Valeur qui spécifie si la transformation d'importation de colonne attend une marque d'ordre d'octet. Une marque d'ordre d'octet n'est attendue que si les données sont du type de données DT_NTEXT.|  
|FileDataColumnID|Entier|Valeur qui identifie la colonne contenant les données que la transformation insère dans le flux de données. Dans la colonne de données à insérer, cette propriété a une valeur de 0 ; dans la colonne contenant les chemins d'accès aux fichiers sources, elle contient le **LineageID** de la colonne de données à insérer.|  
  
 L'entrée, la sortie et les colonnes de sortie de la transformation d'importation de colonne ne disposent pas de propriétés personnalisées.  
  
 Pour plus d’informations, voir [Import Column Transformation](../../../integration-services/data-flow/transformations/import-column-transformation.md).  
  
##  <a name="fgroup"></a> Transformation de regroupement probable, propriétés personnalisées  
 La transformation de regroupement probable dispose à la fois de propriétés personnalisées et de propriétés communes à l'ensemble des composants de flux de données.  
  
 Le tableau suivant décrit les propriétés personnalisées de la transformation de regroupement probable. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|Séparateurs|String|Séparateurs de jetons utilisés par la transformation. Les séparateurs par défaut incluent les caractères suivants : espace ( ), virgule (,), point (.), point-virgule (;), deux-points (:), trait d’union (-), guillemet dactylographique double ("), guillemet dactylographique simple (’), esperluette (&), barre oblique (/), barre oblique inverse (\\), arobase (@), point d’exclamation (!), point d’interrogation (?), parenthèse ouvrante ((), parenthèse fermante ()), signe inférieur à (\<), signe supérieur à (>), crochet ouvrant ([), crochet fermant (]), accolade ouvrante ({), accolade fermante (}), barre verticale (&#124;), signe dièse (#), astérisque (*), signe insertion (^) et symbole de pourcentage (%).|  
|Exhaustive|Booléen|Valeur spécifiant si chaque enregistrement d'entrée est comparé à tous les autres enregistrements d'entrée. La valeur **True** sert principalement à des fins de débogage. La valeur par défaut de cette propriété est **False**.<br /><br /> Remarque : cette propriété n’est pas disponible dans l’ **Éditeur de transformation de regroupement probable**, mais peut être définie à l’aide de l’ **éditeur avancé**.|  
|MaxMemoryUsage|Entier|Quantité de mémoire maximale que peut utiliser la transformation. La valeur par défaut de cette propriété est **0**, permettant ainsi l'utilisation dynamique de la mémoire.<br /><br /> Il est possible de spécifier la valeur de cette propriété en utilisant l'expression d'une propriété.<br /><br /> Remarque : cette propriété n’est pas disponible dans l’ **Éditeur de transformation de regroupement probable**, mais peut être définie à l’aide de l’ **éditeur avancé**.|  
|MinSimilarity|Double|Seuil de similarité (exprimé par une valeur comprise entre 0 et 1) que la transformation utilise pour identifier des doublons.  La valeur par défaut de cette propriété est de 0,8.|  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes d'entrée de la transformation de regroupement probable. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|ExactFuzzy|Integer (énumération)|Valeur qui spécifie si la transformation réalise une correspondance approximative ou une correspondance exacte. Les valeurs valides sont **Exacte** et **Approximative**. La valeur par défaut de cette propriété est **Approximative**.|  
|FuzzyComparisonFlags|Integer (énumération)|Valeur qui spécifie la manière dont la transformation compare les données de chaîne dans une colonne. Cette propriété peut prendre les valeurs suivantes :<br /><br /> **FullySensitive**<br /><br /> **IgnoreCase**<br /><br /> **IgnoreKanaType**<br /><br /> **IgnoreNonSpace**<br /><br /> **IgnoreSymbols**<br /><br /> **IgnoreWidth**<br /><br /> <br /><br /> Pour plus d'informations, voir [Comparing String Data](../../../integration-services/data-flow/comparing-string-data.md).|  
|LeadingTrailingNumeralsSignificant|Integer (énumération)|Valeur qui précise l'importance des chiffres. Cette propriété peut prendre les valeurs suivantes :<br /><br /> **NumeralsNotSpecial** (0) : à utiliser si les chiffres ne sont pas significatifs.<br /><br /> **LeadingNumeralsSignificant** (1) : à utiliser si les premiers chiffres sont significatifs.<br /><br /> **TrailingNumeralsSignificant** (2) : à utiliser si les derniers chiffres sont significatifs.<br /><br /> **LeadingAndTrailingNumeralsSignificant** (3) : à utiliser si les premiers et les derniers chiffres sont significatifs.|  
|MinSimilarity|Double|Seuil de similarité (exprimé par une valeur comprise entre 0 et 1) utilisé pour la jointure sur la colonne. Seules les lignes supérieures au seuil sont validées comme des correspondances.|  
|ToBeCleaned|Booléen|Valeur qui spécifie si la colonne est utilisée pour identifier des doublons, à savoir s'il s'agit d'une colonne dans laquelle vous procédez à des regroupements. La valeur par défaut de cette propriété est **False**.|  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes de sortie de la transformation de regroupement probable. Toutes les propriétés sont en lecture/écriture.  
  
|Nom de la propriété|Type de données|Description|  
|-------------------|---------------|-----------------|  
|ColumnType|Integer (énumération)|Valeur qui identifie le type de colonne de sortie. Cette propriété peut prendre les valeurs suivantes :<br /><br /> **Undefined** (0)<br /><br /> **KeyIn** (1)<br /><br /> **KeyOut** (2)<br /><br /> **Similarity** (3)<br /><br /> **ColumnSimilarity** (4)<br /><br /> **PassThru** (5)<br /><br /> **Canonica**l (6)|  
|InputID|Entier|**LineageID** de la colonne d'entrée correspondante.|  
  
 L'entrée et la sortie de la transformation de regroupement probable ne sont pas dotées de propriétés personnalisées.  
  
 Pour plus d’informations, voir [Fuzzy Grouping Transformation](../../../integration-services/data-flow/transformations/fuzzy-grouping-transformation.md).  
  
##  <a name="flookup"></a> Transformation de recherche floue, propriétés personnalisées  
 La transformation de recherche floue dispose à la fois de propriétés personnalisées et de propriétés communes à l'ensemble des composants de flux de données.  
  
 Le tableau suivant décrit les propriétés personnalisées de la transformation de recherche floue. Toutes les propriétés à l’exception de **ReferenceMetadataXML** sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|CopyReferenceTable|Booléen|Spécifie si une copie de la table de référence doit être réalisée pour la construction de l'index de recherche floue et les recherches suivantes. La valeur par défaut de cette propriété est **True**.|  
|Séparateurs|String|Séparateurs utilisés par la transformation pour marquer les valeurs de colonne. Les séparateurs par défaut incluent les caractères suivants : espace ( ), virgule (,), point (.), point-virgule (;), deux-points (:), trait d’union (-), guillemet dactylographique double ("), guillemet dactylographique simple (’), esperluette (&), barre oblique (\\), barre oblique inverse (\), arobase (@), point d’exclamation (!), point d’interrogation (?), parenthèse ouvrante ((), parenthèse fermante ()), signe inférieur à (\<), signe supérieur à (>), crochet ouvrant ([), crochet fermant (]), accolade ouvrante ({), accolade fermante (}), barre verticale (&#124;). signe dièse (#), astérisque (*), signe insertion (^) et symbole de pourcentage (%).|  
|DropExistingMatchIndex|Booléen|Valeur spécifiant si l’index de correspondance spécifié dans MatchIndexName est supprimé quand MatchIndexOptions n’est pas défini sur ReuseExistingIndex. La valeur par défaut de cette propriété est **True**.|  
|Exhaustive|Booléen|Valeur spécifiant si chaque enregistrement d'entrée est comparé à tous les autres enregistrements d'entrée. La valeur **True** sert principalement à des fins de débogage. La valeur par défaut de cette propriété est **False**.<br /><br /> Remarque : cette propriété n’est pas disponible dans l’ **Éditeur de transformation de recherche floue**, mais peut être définie à l’aide de l’ **éditeur avancé**.|  
|MatchIndexName|String|Nom de l'index de correspondance. L'index de correspondance désigne la table dans laquelle la transformation crée et enregistre l'index qu'elle utilise. En cas de réutilisation de l’index de correspondance, MatchIndexName spécifie l’index à réutiliser. MatchIndexName doit être un nom d’identificateur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] valide. Par exemple, si le nom contient des espaces, il doit apparaître entre crochets.|  
|MatchIndexOptions|Integer (énumération)|Valeur précisant la manière dont la transformation gère l'index de correspondance. Cette propriété peut prendre les valeurs suivantes :<br /><br /> **ReuseExistingIndex** (0)<br /><br /> **GenerateNewIndex** (1)<br /><br /> **GenerateAndPersistNewIndex** (2)<br /><br /> **GenerateAndMaintainNewIndex** (3)|  
|MaxMemoryUsage|Entier|Taille maximale du cache pour la table de recherche. La valeur par défaut de cette propriété est **0**, ce qui signifie que la taille du cache est illimitée.<br /><br /> Il est possible de spécifier la valeur de cette propriété en utilisant l'expression d'une propriété.<br /><br /> Remarque : cette propriété n’est pas disponible dans l’ **Éditeur de transformation de recherche floue**, mais peut être définie à l’aide de l’ **éditeur avancé**.|  
|MaxOutputMatchesPerInput|Entier|Nombre maximal de correspondances que la transformation peut retourner pour chaque ligne d'entrée. La valeur par défaut de cette propriété est **1**.<br /><br /> Remarque : les valeurs supérieures à 100 sont uniquement définissables à l’aide de l’ **éditeur avancé**.|  
|MinSimilarity|Entier|Seuil de similarité (exprimé par une valeur comprise entre 0 et 1) que la transformation utilise au niveau du composant. Seules les lignes supérieures au seuil sont validées comme des correspondances.|  
|ReferenceMetadataXML|String|[!INCLUDE[ssInternalOnly](../../../includes/ssinternalonly-md.md)]|  
|ReferenceTableName|String|Nom de la table de recherche. Le nom doit être un nom d'identificateur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] valide. Par exemple, si le nom contient des espaces, il doit apparaître entre crochets.|  
|WarmCaches|Booléen|Lorsque la valeur définie est True, la recherche charge partiellement l'index et la table de référence dans la mémoire avant le début de l'exécution, ce qui permet d'améliorer les performances.|  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes d'entrée de la transformation de recherche floue. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|FuzzyComparisonFlags|Entier|Valeur qui spécifie la manière dont la transformation compare les données de chaîne dans une colonne. Pour plus d'informations, voir [Comparing String Data](../../../integration-services/data-flow/comparing-string-data.md).|  
|FuzzyComparisonFlagsEx|Integer (énumération)|Valeur qui spécifie quels indicateurs de comparaison étendus sont utilisés par la transformation. Les valeurs peuvent comprendre **MapExpandLigatures, MapFoldCZone**, **MapFoldDigits**, **MapPrecomposed**et **NoMapping**. **NoMapping** ne peut être utilisé avec d'autres indicateurs.|  
|JoinToReferenceColumn|String|Valeur spécifiant le nom de la colonne dans la table de référence à laquelle la colonne est jointe.|  
|JoinType|Entier|Valeur qui spécifie si la transformation réalise une correspondance approximative ou une correspondance exacte. La valeur par défaut de cette propriété est **Approximative**. La valeur entière pour le type de jointure exact est **1** et la valeur pour le type de jointure flou est **2**.|  
|MinSimilarity|Double|Seuil de similarité (exprimé par une valeur comprise entre 0 et 1) que la transformation utilise au niveau de la colonne. Seules les lignes supérieures au seuil sont validées comme des correspondances.|  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes de sortie de la transformation de recherche floue. Toutes les propriétés sont en lecture/écriture.  
  
> [!NOTE]  
>  Pour les colonnes de sortie qui contiennent des valeurs de relais issues des colonnes d’entrée correspondantes, CopyFromReferenceColumn est vide et SourceInputColumnLineageID contient le **LineageID** de la colonne d’entrée correspondante. Pour les colonnes de sortie qui contiennent des résultats de recherche, CopyFromReferenceColumn contient le nom de la colonne de recherche et SourceInputColumnLineageID est vide.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|ColumnType|Entier (énumération)|Valeur qui identifie le type de colonne de sortie pour les colonnes que la transformation ajoute à la sortie. Cette propriété peut prendre les valeurs suivantes :<br /><br /> **Undefined** (0)<br /><br /> **Similarity** (1)<br /><br /> **Confidence** (2)<br /><br /> **ColumnSimilarity** (3)|  
|CopyFromReferenceColumn|String|Valeur précisant le nom de la colonne dans la table de référence qui fournit la valeur dans une colonne de sortie.|  
|SourceInputColumnLineageId|Entier|Valeur identifiant la colonne d'entrée qui fournit des valeurs à cette colonne de sortie.|  
  
 L'entrée et la sortie de la transformation de recherche floue ne sont pas dotées de propriétés personnalisées.  
  
 Pour plus d’informations, voir [Fuzzy Lookup Transformation](../../../integration-services/data-flow/transformations/fuzzy-lookup-transformation.md).  
  
##  <a name="lookup"></a> Transformation de recherche, propriétés personnalisées  
 La transformation de recherche dispose à la fois de propriétés personnalisées et de propriétés communes à l'ensemble des composants de flux de données.  
  
 Le tableau suivant décrit les propriétés personnalisées de la transformation de recherche. Toutes les propriétés à l’exception de **ReferenceMetadataXML** sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|CacheType|Entier (énumération)|Type de cache de la table de recherche. Les valeurs sont **Full** (0), **Partial** (1) et **None** (2). La valeur par défaut de cette propriété est **Full**.|  
|DefaultCodePage|Entier|Page de codes à utiliser par défaut lorsque les informations de page de codes ne sont pas disponibles à partir de la source de données.|  
|MaxMemoryUsage|Entier|Taille maximale du cache pour la table de recherche. La valeur par défaut de cette propriété est **25**, ce qui signifie que la taille du cache est illimitée.|  
|MaxMemoryUsage64|Entier|Taille maximale du cache pour la table de recherche sur un ordinateur 64 bits.|  
|NoMatchBehavior|Integer (énumération)|Valeur qui spécifie si les lignes qui ne disposent pas d'entrées correspondantes dans le dataset de référence sont traitées comme des erreurs.<br /><br /> Quand la propriété est définie avec la valeur **Traiter comme erreurs les lignes sans entrées correspondantes** (0), les lignes sans entrées correspondantes sont traitées comme des erreurs. Vous pouvez spécifier la démarche à suivre lorsque ce type d'erreur se produit à partir de la page **Sortie d'erreur** de la boîte de dialogue **Éditeur de transformation de recherche** . Pour plus d’informations, consultez [Éditeur de transformation de recherche &#40;page Sortie d’erreur&#41;](../../../integration-services/data-flow/transformations/lookup-transformation-editor-error-output-page.md).<br /><br /> Quand la propriété est définie avec la valeur **Envoyer les lignes sans entrées correspondantes dans la sortie sans correspondance** (1), les lignes ne sont pas traitées comme des erreurs.<br /><br /> La valeur par défaut est **Traiter comme erreurs les lignes sans entrées correspondantes** (0).|  
|ParameterMap|String|Liste délimitée par des points-virgules d’identificateurs de lignage mappés aux paramètres utilisés dans l’instruction **SqlCommand** .|  
|ReferenceMetadataXML|String|Métadonnées des colonnes de la table de recherche que la transformation copie vers sa sortie.|  
|SqlCommand|String|Instruction SELECT qui remplit la table de recherche.|  
|SqlCommandParam|String|Instruction SQL paramétrable qui remplit la table de recherche.|  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes d'entrée de la transformation de recherche. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|CopyFromReferenceColumn|String|Nom de la colonne dans la table de référence depuis laquelle une colonne est copiée.|  
|JoinToReferenceColumns|String|Nom de la colonne dans la table de référence à laquelle une colonne source est jointe.|  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes de sortie de la transformation de recherche. Toutes les propriétés sont en lecture/écriture.  
  
|Nom de la propriété|Type de données|Description|  
|-------------------|---------------|-----------------|  
|CopyFromReferenceColumn|String|Nom de la colonne dans la table de référence depuis laquelle une colonne est copiée.|  
  
 L'entrée et la sortie de la transformation de recherche ne sont pas dotées de propriétés personnalisées.  
  
 Pour plus d’informations, voir [Lookup Transformation](../../../integration-services/data-flow/transformations/lookup-transformation.md).  
  
##  <a name="mjoin"></a> Transformation de jointure de fusion, propriétés personnalisées  
 La transformation de jointure de fusion dispose à la fois de propriétés personnalisées et de propriétés communes à l'ensemble des composants de flux de données.  
  
 Le tableau suivant décrit les propriétés personnalisées de la transformation de jointure de fusion.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|JoinType|Integer (énumération)|Spécifie si la jointure est une jointure interne (2), externe gauche (1) ou complète (0).|  
|MaxBuffersPerInput|Entier|Vous n'avez plus à configurer la valeur de la propriété **MaxBuffersPerInput** car Microsoft a apporté des modifications qui réduisent le risque que la transformation de jointure de fusion consomme de la mémoire en excès. Ce problème s'est quelquefois produit lorsque plusieurs entrées de jointure de fusion produisaient des données à des taux irréguliers.|  
|NumKeyColumns|Entier|Nombre de colonnes utilisées dans la jointure.|  
|TreatNullsAsEqual|Booléen|Valeur qui spécifie si la transformation gère les valeurs NULL comme valeurs égales. La valeur par défaut de cette propriété est **True**. Si la valeur de la propriété est **False**, la transformation gère les valeurs NULL de la même manière que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes de sortie de la transformation de jointure de fusion. Toutes les propriétés sont en lecture/écriture.  
  
|Nom de la propriété|Type de données|Description|  
|-------------------|---------------|-----------------|  
|InputColumnID|Entier|**LineageID** de la colonne d'entrée de laquelle les données sont copiées vers cette colonne de sortie.|  
  
 L'entrée, les colonnes d'entrée et la sortie de la transformation de jointure de fusion ne disposent pas de propriétés personnalisées.  
  
 Pour plus d’informations, voir [Merge Join Transformation](../../../integration-services/data-flow/transformations/merge-join-transformation.md).  
  
##  <a name="oledbcmd"></a> Transformation de commande OLE DB, propriétés personnalisées  
 La transformation de commande OLE DB dispose à la fois de propriétés personnalisées et de propriétés communes à l'ensemble des composants de flux de données.  
  
 Le tableau suivant décrit les propriétés personnalisées de la transformation de commande OLE DB.  
  
|Nom de la propriété|Type de données|Description|  
|-------------------|---------------|-----------------|  
|CommandTimeout|Entier|Nombre maximal de secondes pendant lesquelles la commande SQL peut être exécutée avant d'arriver à expiration. Une valeur égale à **0** indique une durée illimitée. La valeur par défaut de cette propriété est **0**.|  
|DefaultCodePage|Entier|Page de codes à utiliser lorsque les informations de page de codes ne sont pas disponibles depuis la source de données.|  
|SqlCommand|String|Instruction Transact-SQL que la transformation exécute pour chaque ligne dans le flux de données.<br /><br /> Il est possible de spécifier la valeur de cette propriété en utilisant l'expression d'une propriété.|  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes externes de la transformation de commande OLE DB. Toutes les propriétés sont en lecture/écriture.  
  
|Nom de la propriété|Type de données|Description|  
|-------------------|---------------|-----------------|  
|DBParamInfoFlag|Entier (masque de bits)|Ensemble d'indicateurs qui décrivent les caractéristiques des paramètres. Pour plus d'informations, consultez DBPARAMFLAGSENUM dans la documentation OLE DB disponible dans MSDN Library.|  
  
 L'entrée, les colonnes d'entrée, la sortie et les colonnes de sortie de la transformation de commande OLE DB ne disposent pas de propriétés personnalisées.  
  
 Pour plus d’informations, voir [OLE DB Command Transformation](../../../integration-services/data-flow/transformations/ole-db-command-transformation.md).  
  
##  <a name="percent"></a> Transformation d'échantillonnage par pourcentage, propriétés personnalisées  
 La transformation d'échantillonnage par pourcentage dispose à la fois de propriétés personnalisées et de propriétés communes à l'ensemble des composants de flux de données.  
  
 Le tableau suivant décrit les propriétés personnalisées de la transformation d'échantillonnage par pourcentage.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|SamplingSeed|Entier|Valeur de départ employée par le générateur de nombres aléatoires. La valeur par défaut de cette propriété est **0**, ce qui indique que la transformation utilise un nombre de cycles.|  
|SamplingValue|Entier|Taille de l'échantillon sous la forme d'un pourcentage de la source.<br /><br /> Il est possible de spécifier la valeur de cette propriété en utilisant l'expression d'une propriété.|  
  
 Le tableau suivant décrit les propriétés personnalisées des sorties de la transformation d'échantillonnage par pourcentage. Toutes les propriétés sont en lecture/écriture.  
  
|Nom de la propriété|Type de données|Description|  
|-------------------|---------------|-----------------|  
|Selected|Booléen|Désigne la sortie vers laquelle les lignes échantillonnées sont dirigées. Dans la sortie sélectionnée, Selected est défini sur **True**, tandis que dans la sortie non sélectionnée, Selected est défini sur **False**.|  
  
 L'entrée, les colonnes d'entrée et les colonnes de sortie de la transformation d'échantillonnage par pourcentage ne disposent pas de propriétés personnalisées.  
  
 Pour plus d’informations, voir [Percentage Sampling Transformation](../../../integration-services/data-flow/transformations/percentage-sampling-transformation.md).  
  
##  <a name="pivot"></a> Transformation de tableau croisé dynamique, propriétés personnalisées  
 Le tableau suivant décrit les propriétés de composant personnalisées de la transformation de tableau croisé dynamique.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|**PassThroughUnmatchedPivotKeyts**|Booléen|Affectez la valeur **True** pour configurer la transformation de tableau croisé dynamique afin d'ignorer les lignes contenant des valeurs non reconnues dans la colonne Clé de tableau croisé dynamique et de générer toutes les valeurs clés de tableau croisé dynamique dans un message de journal, lorsque le package est exécuté.|  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes d'entrée de la transformation de tableau croisé dynamique. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|PivotUsage|Integer (énumération)|Valeur qui spécifie le rôle d'une colonne lorsque le jeu de données est croisé.<br /><br /> **0**:<br />                      La colonne n'est pas croisée dynamiquement et les valeurs de la colonne sont transmises à la sortie de la transformation.<br /><br /> **1**:<br />                      La colonne fait partie de la clé d'ensemble qui identifie une ou plusieurs lignes comme appartenant à un même ensemble. Toutes les lignes d'entrée portant la même clé d'ensemble sont combinées dans une ligne de sortie.<br /><br /> **2**:<br />                      La colonne est une colonne tableau croisé dynamique. Au moins une colonne est créée à partir de chaque valeur de colonne.<br /><br /> **3**:<br />                      Les valeurs de cette colonne sont placées dans les colonnes créées à la suite du croisement dynamique.|  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes de sortie de la transformation de tableau croisé dynamique. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|PivotKeyValue|String|Une des valeurs possibles de la colonne marquée comme clé de tableau croisé dynamique par la valeur de sa propriété PivotUsage.<br /><br /> Il est possible de spécifier la valeur de cette propriété en utilisant l'expression d'une propriété.|  
|SourceColumn|Entier|**LineageID** d’une colonne d’entrée qui contient une valeur croisée dynamique ou -1. La valeur -1 indique que la colonne ne participe à aucune opération croisée dynamique.|  
  
 Pour plus d’informations, voir [Pivot Transformation](../../../integration-services/data-flow/transformations/pivot-transformation.md).  
  
##  <a name="rowcount"></a> Transformation de calcul du nombre de lignes, propriétés personnalisées  
 La transformation de calcul du nombre de lignes dispose à la fois de propriétés personnalisées et de propriétés communes à l'ensemble des composants de flux de données.  
  
 Le tableau suivant décrit les propriétés personnalisées de la transformation de calcul du nombre de lignes. Toutes les propriétés sont en lecture/écriture.  
  
|Nom de la propriété|Type de données|Description|  
|-------------------|---------------|-----------------|  
|VariableName|String|Nom de la variable contenant le nombre de lignes.|  
  
 L'entrée, les colonnes d'entrée, la sortie et les colonnes de sortie de la transformation de calcul du nombre de lignes ne disposent pas de propriétés personnalisées.  
  
 Pour plus d’informations, voir [Row Count Transformation](../../../integration-services/data-flow/transformations/row-count-transformation.md).  
  
##  <a name="rowsamp"></a> Transformation d'échantillonnage de lignes, propriétés personnalisées  
 La transformation d'échantillonnage de lignes dispose à la fois de propriétés personnalisées et de propriétés communes à l'ensemble des composants de flux de données.  
  
 Le tableau suivant décrit les propriétés personnalisées de la transformation d'échantillonnage de lignes. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|SamplingSeed|Entier|Valeur de départ employée par le générateur de nombres aléatoires. La valeur par défaut de cette propriété est **0**, ce qui indique que la transformation utilise un nombre de cycles.|  
|SamplingValue|Entier|Nombre de lignes dans l'échantillon.<br /><br /> Il est possible de spécifier la valeur de cette propriété en utilisant l'expression d'une propriété.|  
  
 Le tableau suivant décrit les propriétés personnalisées des sorties de la transformation d'échantillonnage de lignes. Toutes les propriétés sont en lecture/écriture.  
  
|Nom de la propriété|Type de données|Description|  
|-------------------|---------------|-----------------|  
|Selected|Booléen|Désigne la sortie vers laquelle les lignes échantillonnées sont dirigées. Dans la sortie sélectionnée, Selected est défini sur **True**, tandis que dans la sortie non sélectionnée, Selected est défini sur **False**.|  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes de sortie de la transformation d'échantillonnage de lignes. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|InputColumnLineageId|Entier|Valeur spécifiant le **LineageID** de la colonne d'entrée qui correspond à la source de la colonne de sortie.|  
  
 L'entrée et les colonnes d'entrée de la transformation d'échantillonnage de lignes ne sont pas dotées de propriétés personnalisées.  
  
 Pour plus d’informations, voir [Row Sampling Transformation](../../../integration-services/data-flow/transformations/row-sampling-transformation.md).  
  
##  <a name="script"></a> Composant Script, propriétés personnalisées  
 Le composant Script dispose à la fois de propriétés personnalisées et de propriétés communes à l'ensemble des composants de flux de données. Les mêmes propriétés personnalisées sont disponibles si le composant Script opère en qualité de source, de transformation ou de destination.  
  
 Le tableau suivant décrit les propriétés personnalisées du composant Script. Toutes les propriétés sont en lecture/écriture.  
  
|Nom de la propriété|Type de données|Description|  
|-------------------|---------------|-----------------|  
|ReadOnlyVariables|String|Liste séparée par des virgules de variables disponibles pour l'accès en lecture seule du composant Script.|  
|ReadWriteVariables|String|Liste séparée par des virgules de variables disponibles pour l'accès en lecture/écriture du composant Script.|  
  
 L'entrée, les colonnes d'entrée, la sortie et les colonnes de sortie du composant Script ne disposent pas de propriétés personnalisées, sauf si le développeur du script crée des propriétés personnalisées pour elles.  
  
 Pour plus d’informations, voir [Script Component](../../../integration-services/data-flow/transformations/script-component.md).  
  
##  <a name="scd"></a> Transformation de dimension à variation lente, propriétés personnalisées  
 La transformation de dimension à variation lente dispose à la fois de propriétés personnalisées et de propriétés communes à l'ensemble des composants de flux de données.  
  
 Le tableau suivant décrit les propriétés personnalisées de la transformation de dimension à variation lente. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|CurrentRowWhere|String|Clause WHERE dans l'instruction SELECT chargée de sélectionner la ligne actuelle parmi des lignes dotées de la même clé d'entreprise.|  
|EnableInferredMember|Booléen|Valeur indiquant si les mises à jour de membre déduit sont détectées. La valeur par défaut de cette propriété est **True**.|  
|FailOnFixedAttributeChange|Booléen|Valeur qui indique si la transformation échoue lorsque les colonnes de ligne comportant des attributs fixes contiennent des modifications ou lorsque la recherche dans la table de dimension se solde par un échec. Si vous vous attendez à ce que les lignes entrantes contiennent de nouveaux enregistrements, définissez cette valeur sur **True** pour poursuivre la transformation après l'échec de la recherche car la transformation se fonde sur cet échec pour identifier de nouveaux enregistrements. La valeur par défaut de cette propriété est **False**.|  
|FailOnLookupFailure|Booléen|Valeur qui indique si la transformation échoue lorsque la recherche d'un enregistrement existant se solde également par un échec. La valeur par défaut de cette propriété est **False**.|  
|IncomingRowChangeType|Entier|Valeur qui spécifie si toutes les lignes entrantes sont des nouvelles lignes ou si la transformation doit détecter le type de modification.|  
|InferredMemberIndicator|String|Nom de la colonne du membre déduit.|  
|SqlCommand|String|Instruction SQL employée pour créer un ensemble de lignes de schéma.|  
|UpdateChangingAttributeHistory|Booléen|Valeur qui indique si les mises à jour d'attribut d'historique sont dirigées sur la sortie de la transformation pour les mises à jour d'attribut variable.|  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes d'entrée de la transformation de dimension à variation lente. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|ColumnType|Integer (énumération)|Type de mise à jour de la colonne. Les valeurs sont les suivantes : **Attribut de variation** (2), **Attribut fixe** (4), **Attribut d’historique** (3), **Clé** (1) et **Autre** (0).|  
  
 L'entrée, les sorties et les colonnes de sortie de la transformation de dimension à variation lente ne disposent pas de propriétés personnalisées.  
  
 Pour plus d’informations, voir [Slowly Changing Dimension Transformation](../../../integration-services/data-flow/transformations/slowly-changing-dimension-transformation.md).  
  
##  <a name="sort"></a> Transformation de tri, propriétés personnalisées  
 La transformation de tri dispose à la fois de propriétés personnalisées et de propriétés communes à l'ensemble des composants de flux de données.  
  
 Le tableau suivant décrit les propriétés personnalisées de la transformation de tri. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|EliminateDuplicates|Booléen|Spécifie si la transformation supprime les lignes dupliquées dans la sortie de la transformation. La valeur par défaut de cette propriété est **False**.|  
|MaximumThreads|Entier|Contient le nombre maximal de threads que peut utiliser la transformation lors du tri. Une valeur égale à **0** indique une quantité infinie de threads. La valeur par défaut de cette propriété est **0**.<br /><br /> Il est possible de spécifier la valeur de cette propriété en utilisant l'expression d'une propriété.|  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes d'entrée de la transformation de tri. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|NewComparisonFlags|Entier (masque de bits)|Valeur qui spécifie la manière dont la transformation compare les données de chaîne dans une colonne. Pour plus d'informations, voir [Comparing String Data](../../../integration-services/data-flow/comparing-string-data.md).|  
|NewSortKeyPosition|Entier|Valeur qui spécifie l'ordre de tri de la colonne. Une valeur égale à 0 indique que les données ne sont pas triées sur cette colonne.|  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes de sortie de la transformation de tri. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|SortColumnID|Entier|**LineageID** d'une colonne de tri.|  
  
 L'entrée et la sortie de la transformation de tri ne sont pas dotées de propriétés personnalisées.  
  
 Pour plus d’informations, voir [Sort Transformation](../../../integration-services/data-flow/transformations/sort-transformation.md).  
  
##  <a name="textract"></a> Transformation d'extraction de terme, propriétés personnalisées  
 La transformation d'extraction de terme dispose à la fois de propriétés personnalisées et de propriétés communes à l'ensemble des composants de flux de données.  
  
 Le tableau suivant décrit les propriétés personnalisées de la transformation d'extraction de terme. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|--------------|-----------------|  
|FrequencyThreshold|Entier|Valeur numérique indiquant le nombre d'occurrences d'un terme avant son extraction. La valeur par défaut de cette propriété est de **2**.|  
|IsCaseSensitive|Booléen|Valeur qui précise si l'extraction de noms et d'expressions nominales doit tenir compte de la casse. La valeur par défaut de cette propriété est **False**.|  
|MaxLengthOfTerm|Entier|Valeur numérique qui indique la longueur maximale d'un terme. Cette propriété concerne uniquement les expressions. La valeur par défaut de cette propriété est de **12**.|  
|NeedRefenceData|Booléen|Valeur qui spécifie si la transformation utilise une liste de termes d'exclusion stockée dans une table de référence. La valeur par défaut de cette propriété est **False**.|  
|OutTermColumn|String|Nom de la colonne qui contient les termes d'exclusion.|  
|OutTermTable|String|Nom de la table contenant la colonne qui comporte des termes d'exclusion.|  
|ScoreType|Entier|Valeur qui précise le type de score à associer au terme. Les valeurs valides sont 0 qui indique la fréquence et 1 qui désigne un score TFIDF. Le score TFIDF est le produit de la fréquence des termes (TF, Term Frequency) et de la fréquence inverse de documents (IDF, Inverse Document Frequency), défini comme suit : TFIDF d’un terme T = (fréquence de T) \* log( (#lignes en entrée) / (#lignes ayant T)). La valeur par défaut de cette propriété est **0**.|  
|WordOrPhrase|Entier|Valeur qui spécifie le type de terme. Les valeurs valides sont 0 qui indique des mots uniquement, 1 qui désigne des expressions nominales seulement et 2 qui indique des mots et des expressions nominales à la fois. La valeur par défaut de cette propriété est **0**.|  
  
 L'entrée, les colonnes d'entrée, la sortie et les colonnes de sortie de la transformation d'extraction de terme ne disposent pas de propriétés personnalisées.  
  
 Pour plus d’informations, voir [Term Extraction Transformation](../../../integration-services/data-flow/transformations/term-extraction-transformation.md).  
  
##  <a name="tlookup"></a> Transformation de recherche de terme, propriétés personnalisées  
 La transformation de recherche de terme dispose à la fois de propriétés personnalisées et de propriétés communes à l'ensemble des composants de flux de données.  
  
 Le tableau suivant décrit les propriétés personnalisées de la transformation de recherche de terme. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|IsCaseSensitive|Booléen|Valeur qui spécifie si une comparaison respectant la casse s'applique à la correspondance du texte de la colonne d'entrée et au terme de recherche. La valeur par défaut de cette propriété est **False**.|  
|RefTermColumn|String|Nom de la colonne qui contient les termes de recherche.|  
|RefTermTable|String|Nom de la table contenant la colonne qui comporte des termes de recherche.|  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes d'entrée de la transformation de recherche de terme. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|InputColumnType|Entier|Valeur qui spécifie l'utilisation de la colonne. Les valeurs valides sont 0 pour une colonne de relais, 1 pour une colonne de recherche et 2 pour une colonne qui est à la fois une colonne de relais et de recherche.|  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes de sortie de la transformation de recherche de terme. Toutes les propriétés sont en lecture/écriture.  
  
|Nom de la propriété|Type de données|Description|  
|-------------------|---------------|-----------------|  
|CustomLineageID|Entier|**LineageID** de la colonne d'entrée correspondante si la valeur du **InputColumnType** de cette colonne est égale à 0 ou 2.|  
  
 L'entrée et la sortie de la transformation de recherche de terme ne sont pas dotées de propriétés personnalisées.  
  
 Pour plus d’informations, voir [Term Lookup Transformation](../../../integration-services/data-flow/transformations/term-lookup-transformation.md).  
  
##  <a name="unpivot"></a> Transformation UnPivot, propriétés personnalisées  
 La transformation UnPivot dispose uniquement des propriétés communes à l'ensemble des composants de flux de données au niveau du composant.  
  
> [!NOTE]  
>  Cette section porte sur le scénario UnPivot présenté dans [Transformation Unpivot](../../../integration-services/data-flow/transformations/unpivot-transformation.md) pour illustrer l’utilisation des options décrites.  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes d'entrée de la transformation UnPivot. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|DestinationColumn|Entier|**LineageID** de la colonne de sortie à laquelle la colonne d'entrée est mappée. Une valeur égale à -1 indique que la colonne d'entrée n'est pas mappée à une colonne de sortie.|  
|PivotKeyValue|String|Valeur copiée vers une colonne de sortie de la transformation.<br /><br /> Il est possible de spécifier la valeur de cette propriété en utilisant l'expression d'une propriété.<br /><br /> Dans le scénario UnPivot décrit dans [Unpivot Transformation](../../../integration-services/data-flow/transformations/unpivot-transformation.md), les valeurs de tableau croisé dynamique sont les valeurs texte Ham, Coke, Milk, Beer et Chips. Ces valeurs apparaîtront sous la forme de valeurs texte dans la nouvelle colonne Product définie au moyen de l'option **Nom de colonne de la valeur de clé de tableau croisé dynamique** .|  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes de sortie de la transformation UnPivot. Toutes les propriétés sont en lecture/écriture.  
  
|Nom de la propriété|Type de données|Description|  
|-------------------|---------------|-----------------|  
|PivotKey|Booléen|Indique si les valeurs dans la propriété **PivotKeyValue** des colonnes d'entrée sont écrites dans cette colonne de sortie.<br /><br /> Dans le scénario UnPivot décrit dans [Unpivot Transformation](../../../integration-services/data-flow/transformations/unpivot-transformation.md), le nom de la colonne de la valeur de tableau croisé dynamique est **Product** et définit la nouvelle colonne **Product** dans laquelle les colonnes Ham, Coke; Milk, Beer et Chips ne sont pas croisées dynamiquement.|  
  
 L'entrée et la sortie de la transformation UnPivot ne sont pas dotées de propriétés personnalisées.  
  
 Pour plus d’informations, voir [Unpivot Transformation](../../../integration-services/data-flow/transformations/unpivot-transformation.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Transformations Integration Services](../../../integration-services/data-flow/transformations/integration-services-transformations.md)   
 [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)   
 [Propriétés du chemin](http://msdn.microsoft.com/library/89b1e347-9579-4f6b-af74-c6519ea08eea)   
 [Propriétés du flux de données pouvant être définies à l’aide d’expressions](http://msdn.microsoft.com/library/cd0e171a-08be-45d6-81dc-ed94f37698b8)  
  
  
