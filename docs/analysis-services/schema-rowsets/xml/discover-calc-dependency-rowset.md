---
title: Ensemble de lignes DISCOVER_CALC_DEPENDENCY | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 3646b6dc83597918d4568003763b2aa26a7fd5a8
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="discovercalcdependency-rowset"></a>Ensemble de lignes DISCOVER_CALC_DEPENDENCY
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Rapports sur les dépendances entre les calculs et sur les objets référencés dans ces calculs. Vous pouvez utiliser ces informations dans une application cliente pour créer un rapport sur les problèmes avec des formules complexes, ou pour avertir lorsque des objets connexes sont supprimés ou modifiés. Vous pouvez également utiliser l'ensemble de lignes pour extraire les expressions DAX utilisées dans des mesures ou colonnes calculées.  
  
 **S'applique à :** modèles tabulaires  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 L'ensemble de lignes **DISCOVER_CALC_DEPENDENCY** contient les colonnes suivantes. La table spécifie également le type de données, indique si la colonne peut être restreinte afin de limiter les lignes qui sont retournées et fournit une description de chaque colonne.  
  
|Nom de colonne|Indicateur de type|Restriction| Description|  
|-----------------|--------------------|-----------------|-----------------|  
|**DATABASE_NAME**|**DBTYPE_WSTR**|Oui|Spécifie le nom de la base de données qui contient l'objet pour lequel l'analyse de dépendance est demandée. Si omis, le nom de la base de données active est utilisé.<br /><br /> L'ensemble de lignes **DISCOVER_DEPENDENCY_CALC** peut être restreint à l'aide de cette colonne.|  
|**OBJECT_TYPE**|**DBTYPE_WSTR**|Oui|Indique le type de l'objet pour lequel l'analyse de dépendance est demandée. Il doit s'agir de l'un des types d'objets suivants :<br /><br /> **ACTIVE_RELATIONSHIP**: relation active<br /><br /> **CALC_COLUMN**: colonne calculée<br /><br /> **HIERARCHY**: hiérarchie<br /><br /> **MEASURE**: mesure<br /><br /> **RELATIONSHIP**: relation<br /><br /> **KPI**: indicateur de performance clé (KPI)<br /><br /> <br /><br /> Notez que la **DISCOVER_DEPENDENCY_CALC** ensemble de lignes peut être restreint à l’aide de cette colonne.|  
|**REQUÊTE**|**DBTYPE_WSTR**|Oui|Pour les modèles tabulaires créés dans [!INCLUDE[ssSQL11SP1](../../../includes/sssql11sp1-md.md)], vous pouvez inclure une expression ou une requête DAX afin de montrer le diagramme de dépendances pour cette expression ou cette requête. La restriction QUERY permet aux applications clientes de déterminer les objets qui sont utilisés par une requête DAX.<br /><br /> La restriction **QUERY** peut être spécifiée dans XMLA ou dans la clause WHERE d'une requête DMV. Pour plus d'informations, consultez la section Exemples.|  
|**TABLE**|**DBTYPE_WSTR**||Nom de la table qui contient l'objet pour lequel les informations de dépendance sont générées.|  
|**OBJECT**|**DBTYPE_WSTR**||Nom de l'objet pour lequel les informations de dépendance sont générées. Si l'objet est une mesure ou une colonne calculée, utilisez le nom de la mesure. Si l'objet est une relation, le nom de la table (ou la dimension du cube) qui contient la colonne qui participe à la relation.|  
|**EXPRESSION**|**DBTYPE_WSTR**||Formule qui contient l'objet pour lequel les dépendances sont recherchées.|  
|**REFERENCED_OBJECT_TYPE**|**DBTYPE_WSTR**||Retourne le type de l'objet qui a une dépendance sur l'objet référencé. Les objets retournés peuvent être les suivants :<br /><br /> **CALC_COLUMN**: colonne calculée<br /><br /> **COLUMN**: colonne de données<br /><br /> **MEASURE**: mesure<br /><br /> **RELATIONSHIP**: relation<br /><br /> **KPI**: indicateur de performance clé (KPI)|  
|**REFERENCED_TABLE**|**DBTYPE_ WSTR**||Nom de la table qui contient l'objet dépendant.|  
|**REFERENCED_OBJECT**|**DBTYPE_ WSTR**||Nom de l'objet qui a une dépendance sur l'objet référencé. Pour les mesures et les colonnes calculées, nom de la mesure ou de la colonne. Pour les relations, nom complet de la table (ou de la dimension du cube) qui contient l'objet dépendant.|  
|**REFERENCED_EXPRESSION**|**DBTYPE_WSTR**||Formule, soit dans une colonne calculée, soit dans une mesure, qui est dépendante sur l'objet référencé.|  
  
## <a name="example"></a>Exemple  
 **Syntaxe de base**  
  
 La requête suivante illustre une requête DMV simple qui retourne des valeurs pour toutes les colonnes de cet ensemble de lignes, à l'aide de la base de données par défaut sur la connexion actuelle. Vous pouvez exécuter cette requête dans une fenêtre de requête MDX et afficher ses résultats dans [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]. Vous pouvez également suivre les techniques décrites dans [interrogation Power Pivot DMV à partir d’Excel](http://go.microsoft.com/fwlink/?LinkID=235146) pour afficher les résultats de la requête DMV dans Excel.  
  
```  
SELECT * FROM $System.DISCOVER_CALC_DEPENDENCY  
```  
  
## <a name="example"></a>Exemple  
 **Trier les résultats**  
  
 Ajoutez une clause ORDER BY pour trier l'ensemble de lignes par table ou une autre colonne.  
  
```  
SELECT * FROM $System.DISCOVER_CALC_DEPENDENCY ORDER BY [TABLE] ASC  
```  
  
## <a name="example"></a>Exemple  
 **Filtrer à l’aide d’une clause WHERE**  
  
 La requête suivante montre comment ajouter une restriction à l'aide de la clause WHERE. Il est possible d'utiliser les colonnes suivantes en tant que filtres de requête dans une clause WHERE : **Database_Name**, **Object_Type**et **Query**.  
  
```  
SELECT * From $SYSTEM.DISCOVER_CALC_DEPENDENCY WHERE OBJECT_TYPE = 'RELATIONSHIP' OR OBJECT_TYPE = 'ACTIVE_RELATIONSHIP'  
```  
  
## <a name="example"></a>Exemple  
 **Filtrer des mesures et des colonnes calculées afin d’afficher les expressions DAX sous-jacentes**  
  
 Dans cette requête, vous pouvez sélectionner uniquement la mesure ou la colonne calculée, puis afficher l'expression DAX utilisée dans le calcul. La colonne EXPRESSION contient les expressions DAX. Si vous utilisez DISCOVER_CALC_DEPENDENCY pour extraire l'expression DAX utilisée dans le modèle, cette requête est suffisante à cette fin. Elle retourne toutes les expressions utilisées dans le modèle, par ordre croissant.  
  
```  
SELECT * From $SYSTEM.DISCOVER_CALC_DEPENDENCY WHERE OBJECT_TYPE = 'MEASURE' OR OBJECT_TYPE = 'CALC_COLUMN' ORDER BY [EXPRESSION] ASC  
```  
  
## <a name="example"></a>Exemple  
 **Filtrer à l’aide de la requête**  
  
 Grâce à la restriction QUERY, vous pouvez proposer une requête DAX afin d'afficher tous les objets utilisés dans cette requête. Examinons une requête simple, telle que l'évaluation d'un client. Cette requête, telle qu'elle est écrite, retourne des lignes de données client, où la composition des lignes est basée sur les colonnes de la table Customer. Si vous exécutez maintenant DISCOVER_CALC_DEPENDENCY avec une restriction QUERY de la requête d'évaluation d'un client, vous obtenez les colonnes (ou objets) utilisées dans cette requête. Dans ce cas, il s'agit d'une liste des colonnes de la table Customer.  
  
 Le prochain ensemble de requêtes illustre la syntaxe de la restriction QUERY. Vous pouvez exécuter ces requêtes dans le [modèle tabulaire AdventureWorks SQL Server 2012](http://msftdbprodsamples.codeplex.com/releases/view/55330) pour afficher le résultat défini.  
  
 La première requête montre comment spécifier une restriction QUERY pour les noms d'objet qui comportent des espaces. La deuxième requête, tirée de [Execute DAX queries via OLE DB et ADOMD.NET](http://go.microsoft.com/fwlink/?LinkId=254329), est une requête plus complexe qui inclut des objets de plusieurs tables.  
  
> [!NOTE]  
>  Bien que les requêtes semblent utiliser des guillemets doubles, en fait seuls des guillemets simples sont utilisés. Une paire de guillemets simples englobe ' Evaluate \<Tablename >', et les guillemets simples utilisés autour du nom de table doivent être échappés doublés. Les guillemets simples autour du nom d'une table sont uniquement nécessaires pour les noms de table qui comportent un espace.  
  
```  
SELECT * From $SYSTEM.DISCOVER_CALC_DEPENDENCY WHERE QUERY = 'EVALUATE ''Reseller Sales'''  
```  
  
```  
SELECT * from $system.DISCOVER_CALC_DEPENDENCY WHERE QUERY = 'EVALUATE CALCULATETABLE(VALUES(''Product Subcategory''[Product Subcategory Name]), ''Product Category''[Product Category Name] = "Bikes")'  
```  
  
## <a name="example"></a>Exemple  
 **Exemple XMLA de Restriction QUERY**  
  
 Vous pouvez utiliser une commande Discover de XMLA pour retourner les objets de requête dans une table. XMLA retourne les résultats au format XML brut. Vous pouvez utiliser ADOMD.NET pour analyser les résultats dans un format plus lisible.  
  
```  
<Discover xmlns="urn:schemas-microsoft-com:xml-analysis">  
   <RequestType>DISCOVER_CALC_DEPENDENCY</RequestType>  
     <Restrictions>  
        <RestrictionList>  
            <QUERY>Evaluate 'Reseller Sales'</QUERY>  
        </RestrictionList>  
    </Restrictions>  
    <Properties />  
</Discover>  
```  
  
## <a name="using-adomdnet-to-return-the-rowset"></a>Utilisation d'ADOMD.NET pour retourner l'ensemble de lignes  
 Lorsque vous utilisez ADOMD.NET et l'ensemble de lignes de schéma pour récupérer des métadonnées, vous pouvez utiliser un GUID ou une chaîne pour référencer un objet d'ensemble de lignes de schéma dans la méthode GetSchemaDataSet. Pour plus d'informations, consultez [Working with Schema Rowsets in ADOMD.NET](../../../analysis-services/multidimensional-models-adomd-net-client/retrieving-metadata-working-with-schema-rowsets.md).  
  
 Le tableau suivant fournit le GUID et les valeurs de chaîne qui identifient cet ensemble de lignes.  
  
|Argument|Valeur|  
|--------------|-----------|  
|GUID|a07ccd46-8148-11d0-87bb-00c04fc33942|  
|ADOMDNAME|DependencyGraph|  
  
## <a name="see-also"></a>Voir aussi  
 [Ensembles de lignes de schéma de Analysis Services](../../../analysis-services/schema-rowsets/analysis-services-schema-rowsets.md)   
 [Utiliser dynamique vues de gestion & #40 ; vues de gestion dynamique & #41 ; pour surveiller Analysis Services](../../../analysis-services/instances/use-dynamic-management-views-dmvs-to-monitor-analysis-services.md)  
  
  
