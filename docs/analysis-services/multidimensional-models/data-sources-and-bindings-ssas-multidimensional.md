---
title: Sources de données et liaisons (SSAS multidimensionnel) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 2461bbdc3707ed30e130aaa32e117f24f9fe379d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="data-sources-and-bindings-ssas-multidimensional"></a>Sources de données et liaisons (SSAS Multidimensionnel)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Les cubes, les dimensions et autres objets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] peuvent être liés à une source de données. Une source de données peut être l'un des objets suivants :  
  
-   Source de données relationnelle.  
  
-   Un pipeline [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui produit un ensemble de lignes (ou des ensembles de lignes avec chapitre).  
  
 Le moyen d'exprimer la source de données varie en fonction du type de source de données. Par exemple, une source de données relationnelle se distingue par la chaîne de connexion. Pour plus d'informations sur les sources de données, consultez [Data Sources in Multidimensional Models](../../analysis-services/multidimensional-models/data-sources-in-multidimensional-models.md).  
  
 Indépendamment de la source de données utilisée, la vue de source de données (DSV, Data Source View) contient les métadonnées pour la source de données. Donc, les liaisons pour un cube ou d'autres objets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sont exprimées en tant que liaisons à DSV. Ces liaisons peuvent inclure des liaisons à des objets logiques tels que des vues, des colonnes calculées et des relations qui n'existent pas physiquement dans la source de données. [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ajoute une colonne calculée qui encapsule l'expression dans l'élément DSV, puis lie la mesure OLAP correspondante à cette colonne dans la vue de source de données. Pour plus d'informations sur les vues de source de données (DSV), consultez [Data Source Views in Multidimensional Models](../../analysis-services/multidimensional-models/data-source-views-in-multidimensional-models.md).  
  
 Chaque objet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] se lie à la source de données d'une façon qui lui est propre. De plus, les liaisons de données pour ces objets et la définition de la source de données peuvent être insérées à la définition de l'objet lié aux données (par exemple, la dimension), ou fournies hors ligne, sous la forme d'un jeu séparé de définitions.  
  
## <a name="analysis-services-data-types"></a>Types de données Analysis Services  
 Les types de données utilisés dans les liaisons doivent correspondre aux types de données pris en charge par [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Les types de données suivants sont définis dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]:  
  
|Type de données Analysis Services| Description|  
|---------------------------------|-----------------|  
|BigInt|Entier signé de 64 bits. Ce type de données est mappé au type de données Int64 dans Microsoft [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] et au type de données DBTYPE_I8 dans OLE DB.|  
|Bool|Valeur booléenne. Ce type de données est mappé au type de données Boolean dans le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] et au type de données DBTYPE_BOOL dans OLE DB.|  
|Monétaire (Currency)|Valeur monétaire comprise entre -263 (ou -922,337,203,685,477.5808) et 263-1 (ou +922,337,203,685,477.5807) avec une précision d'un dix-millième d'une unité monétaire. Ce type de données est mappé au type de données Decimal dans le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] et au type de données DBTYPE_CY dans OLE DB.|  
|Date|Données de date stockées sous la forme d'un nombre à virgule flottante double précision. La partie entière correspond au nombre de jours depuis le 30 décembre 1899 tandis que la partie fractionnaire désigne une fraction d'un jour. Ce type de données est mappé sur le type de données DateTime dans le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] et sur le type de données DBTYPE_DATE dans OLE DB.|  
|Double|Nombre à virgule flottante double précision compris entre -1.79E +308 et 1.79E +308. Ce type de données est mappé au type de données Double dans le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] et au type de données DBTYPE_R8 dans OLE DB.|  
|Entier|Entier signé de 32 bits. Ce type de données est mappé sur le type de données Int32 dans le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] et sur le type de données DBTYPE_I4 dans OLE DB.|  
|Unique|Nombre à virgule flottante simple précision compris entre -3.40E +38 et 3.40E +38. Ce type de données est mappé sur le type de données Single dans le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] et sur le type de données DBTYPE_R4 dans OLE DB.|  
|SmallInt|Entier signé 16 bits. Ce type de données est mappé au type de données Int16 dans le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] et au type de données DBTYPE_I2 dans OLE DB.|  
|TinyInt|Entier signé 8 bits. Ce type de données est mappé au type de données SByte dans le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] et au type de données DBTYPE_I1 dans OLE DB.<br /><br /> Remarque : si une source de données contient des champs de type tinyint et que la propriété Auto-incrément est définie sur True, ils seront convertis en entiers dans la vue de source de données.|  
|UnsignedBigInt|Entier non signé 64 bits. Ce type de données est mappé sur le type de données UInt64 dans le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] et sur le type de données DBTYPE_UI8 dans OLE DB.|  
|UnsignedInt|Entier non signé 32 bits. Ce type de données est mappé sur le type de données UInt32 dans le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] et sur le type de données DBTYPE_UI4 dans OLE DB.|  
|UnsignedSmallInt|Entier non signé 16 bits. Ce type de données est mappé au type de données UInt16 dans le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] et au type de données DBTYPE_UI2 dans OLE DB.|  
|WChar|Flux de caractères Unicode terminé par le caractère NULL. Ce type de données est mappé au type de données String dans le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] et au type de données DBTYPE_WSTR dans OLE DB.|  
  
 Toutes les données reçues de la source de données sont converties selon le type [!INCLUDE[ssAS](../../includes/ssas-md.md)] spécifié dans la liaison (habituellement pendant le traitement). Une erreur est déclenchée si la conversion ne peut pas être effectuée (par exemple, de String en Int). [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] définit habituellement le type de données dans la liaison selon celui qui correspond le mieux au type de la source dans la source de données. Par exemple, les types SQL Date, DateTime, SmallDateTime, DateTime2, DateTimeOffset sont mappés sur le type Date [!INCLUDE[ssAS](../../includes/ssas-md.md)] , et le type SQL Time est mappé sur String.  
  
## <a name="bindings-for-dimensions"></a>Liaisons pour les dimensions  
 Chaque attribut d'une dimension est lié à une colonne dans un élément DSV. Tous les attributs d'une dimension doivent venir d'une source de données unique. Toutefois, les attributs peuvent être liés aux colonnes dans des tables différentes. Les relations entre les tables sont définies dans l'élément DSV. Dans le cas où plusieurs jeux de relations existeraient pour la même table, il peut être nécessaire d'introduire une requête nommée dans l'élément DSV pour qu'il agisse comme une table d'« alias ». Les expressions et les filtres sont définis dans l'élément DSV en utilisant des calculs nommés et des requêtes nommées.  
  
## <a name="bindings-for-measuregroups-measures-and-partitions"></a>Liaisons pour MeasureGroups, les mesures et les partitions  
 Chaque groupe de mesures a les liaisons par défaut suivantes :  
  
-   Le groupe de mesures est lié à une table dans un élément DSV (par exemple, **MeasureGroup.Source**).  
  
-   Chaque mesure est liée à une colonne dans la table (par exemple, **Measure.ValueColumn.Source**).  
  
-   Chaque dimension de groupe de mesures a un jeu d' *attributs de granularité* qui définit la granularité du groupe de mesures. Chacun de ces attributs doit être lié à la ou les colonnes dans la table de faits qui contient la clé d'attribut. (Pour plus d'informations sur les attributs de granularité, consultez Attributs de granularité de MeasureGroup plus loin dans cette rubrique.)  
  
 Ces liaisons par défaut peuvent être remplacées sélectivement par partition. Chaque partition peut spécifier une source de données, un nom de table ou de requête ou une expression de filtre différent. La stratégie de partitionnement la plus commune consiste à remplacer la table par partition, en utilisant la même source de données. Les autres options possibles sont l'application d'un filtre différent par partition ou la modification de la source de données.  
  
 La source de données par défaut doit être définie dans l'élément DSV, en fournissant les informations de schéma, y compris les détails des relations. Aucune table supplémentaire ou requête spécifiée au niveau de la partition ne doit être répertoriée dans l'élément DSV, mais elles doivent avoir le même schéma que la table par défaut définie pour le groupe de mesures, ou au moins, elles doivent contenir toutes les colonnes utilisées par les mesures ou les attributs de granularité. Les liaisons détaillées par mesure et attribut de granularité ne peuvent pas être remplacées au niveau de la partition et elles sont supposées se trouver dans les mêmes colonnes définies pour le groupe de mesures. Par conséquent, si la partition utilise une source de données qui a, en fait, un schéma différent, la requête **TableDefinition** définie pour la partition doit aboutir au même schéma que celui utilisé par le groupe de mesures.  
  
### <a name="measuregroup-granularity-attributes"></a>Attributs de granularité de MeasureGroup  
 Lorsque la granularité d'un groupe de mesures correspond à la granularité connue dans la base de données, et qu'il y a une relation directe de la table de faits à la table de dimension, l'attribut de granularité doit simplement être lié à la colonne ou aux colonnes clés étrangères appropriées sur la table de faits. Prenons l'exemple de la table de faits et de la table de dimension suivantes :  
  
 `Sales(RequestedDate, OrderedProductID, ReplacementProductID, Qty)`  
  
 `Product(ProductID, ProductName,Category)`  
  
 ``  
  
 `Relation: Sales.OrderedProductID -> Product.ProductID`  
  
 `Relation: Sales.ReplacementProductID -> Product.ProductID`  
  
 ``  
  
 Si vous procédez à une analyse par produit commandé, pour Ordered Product sur le rôle de dimension Sales, l'attribut de  granularité Product serait lié à Sales.OrderedProductID.  
  
 Toutefois, dans certains cas **GranularityAttributes** peut ne pas exister en tant que colonne dans la table de faits. Par exemple, **GranularityAttributes** peut ne pas exister en tant que colonne dans les cas suivants :  
  
-   La granularité OLAP a une ampleur plus grande que la granularité dans la source.  
  
-   Une table intermédiaire s'interpose entre la table de faits et la table de dimension.  
  
-   La clé de dimension n'est pas la même que la clé primaire dans la table de dimension.  
  
 Dans tous ces cas, l'élément DSV doit être défini afin que les attributs GranularityAttributes existent sur la table de faits. Par exemple, une requête nommée ou une colonne calculée peut être introduite.  
  
 Notamment, dans les mêmes exemples de tables ci-dessus, si la granularité était par Category, une vue Sales pourrait être introduite :  
  
 `SalesWithCategory(RequestedDate, OrderedProductID, ReplacementProductID, Qty, OrderedProductCategory)`  
  
 `SELECT Sales.*, Product.Category AS OrderedProductCategory`  
  
 `FROM Sales INNER JOIN Product`  
  
 `ON Sales.OrderedProductID = Product.ProductID`  
  
 ``  
  
 Dans ce cas, la catégorie GranularityAttribute est liée à SalesWithCategory.OrderedProductCategory.  
  
### <a name="migrating-from-decision-support-objects"></a>Migration d'objets d'aide à la décision (DSO, Decision Support Objects)  
 DSO (Decision Support Objects) 8.0 permet de lier de nouveau des **PartitionMeasures** . Par conséquent, la stratégie de migration dans ces cas est de construire la requête appropriée.  
  
 De la même façon, il n'est pas possible de lier de nouveau les attributs de dimension dans une partition, bien que DSO 8.0 autorise également cette nouvelle liaison. La stratégie de migration consiste alors à définir les requêtes nommées nécessaires dans l'élément DSV afin que les mêmes tables et colonnes que celles utilisées pour la dimension existent dans l'élément DSV de la partition. Ces cas peuvent requérir l'adoption de la migration simple, dans laquelle la clause From/Join/Filter est mappée à une requête nommée unique plutôt qu'à un jeu structuré de tables associées. Puisque DSO 8.0 permet la nouvelle liaison de PartitionDimensions même lorsque la partition utilise la même source de données, la migration peut également requérir plusieurs DSV pour la même source de données.  
  
 Dans DSO 8.0, les liaisons correspondantes peuvent être exprimées de deux façons différentes, en fonction de l'utilisation de schémas optimisés, en créant une liaison soit à la clé primaire sur la table de dimension, soit à la clé étrangère sur la table de faits. Dans ASSL, ces deux formes ne sont pas distinguées.  
  
 La même approche aux liaisons s'applique même à une partition utilisant une source de données qui ne contient pas de tables de dimension, parce que la liaison est faite à la colonne clé étrangère dans la table de faits, pas à la colonne clé primaire dans la table de dimension.  
  
## <a name="bindings-for-mining-models"></a>Liaisons pour modèles d'exploration de données  
 Un modèle d'exploration de données est soit relationnel, soit OLAP. Les liaisons de données pour un modèle d'exploration de données relationnel sont considérablement différentes des liaisons pour un modèle d'exploration de données OLAP.  
  
### <a name="bindings-for-a-relational-mining-model"></a>Liaisons pour un modèle d'exploration de données relationnel  
 Un modèle d'exploration de données relationnel compte sur les relations définies dans l'élément DSV pour résoudre toute ambiguïté relative à la liaison des colonnes aux sources de données. Dans un modèle d'exploration de données relationnel, les liaisons de données suivent les règles suivantes :  
  
-   Chaque colonne de table non imbriquée est liée à une colonne soit de la table de cas, soit d'une table en rapport avec la table de cas (suivant une relation plusieurs-à-un ou un-à-un). L'élément DSV définit les relations entre les tables.  
  
-   Chaque colonne de table imbriquée est liée à une table source. Puis, les colonnes possédées par la colonne de table imbriquée sont liées aux colonnes sur cette table source ou sur une table en rapport avec la table source. (Une fois encore, la liaison suit une relation plusieurs-à-un ou un-à-un.) Les liaisons de modèle d'exploration de données ne fournissent pas le chemin de jointure à la table imbriquée. Ce sont les relations définies dans l'élément DSV qui fournissent cette information.  
  
### <a name="bindings-for-an-olap-mining-model"></a>Liaisons pour un modèle d'exploration de données OLAP  
 Les modèles d'exploration de données OLAP n'ont pas l'équivalent d'un DSV. Par conséquent, les liaisons de données doivent lever l'ambiguïté entre les colonnes et les sources de données. Par exemple, un modèle d'exploration de données peut être basé sur le cube Sales et les colonnes peuvent être basées sur Qty, Amount et Product Name. Alternativement, un modèle d'exploration de données peut être basé sur Product et les colonnes peuvent être basées sur Product Name, Product Color et une table imbriquée avec Sales Qty.  
  
 Dans un modèle d'exploration de données OLAP, les liaisons de données suivent les règles suivantes :  
  
-   Chaque colonne de table non imbriquée est liée à une mesure sur un cube, à un attribut sur une dimension de ce cube (spécifiant le **CubeDimension** pour lever l’ambiguïté dans le cas de rôles de dimension) ou à un attribut sur une dimension.  
  
-   Chaque colonne de la table imbriquée est liée à un **CubeDimension**. Autrement dit, cela définit la façon de naviguer d'une dimension à un cube connexe ou (dans le cas moins commun de tables imbriquées) d'un cube jusqu'à l'une de ses dimensions.  
  
## <a name="out-of-line-bindings"></a>liaisons hors ligne  
 Les liaisons hors ligne vous permettent de modifier temporairement les liaisons de données existantes pour la durée d'une commande. Les liaisons hors lignes font référence aux liaisons qui sont incluses dans une commande et ne sont pas rendues persistantes. Les liaisons hors lignes s'appliquent uniquement pendant que cette commande particulière s'exécute. Par opposition, les liaisons insérées sont contenues dans la définition d'objet ASSL et sont rendues persistantes avec la définition d'objet dans les métadonnées du serveur.  
  
 ASSL autorise la spécification de liaisons hors ligne sur une commande **Process** , si elle ne fait pas partie d’un lot, ou sur une commande **Batch** . Si les liaisons hors ligne sont spécifiées sur la commande **Batch** , toutes les liaisons spécifiées dans la commande **Batch** créent un nouveau contexte de liaison dans lequel toutes les commandes **Process** du lot s’exécutent. Ce nouveau contexte de liaison inclut des objets traités indirectement en raison de la commande **Process** .  
  
 Lorsque les liaisons hors-ligne sont spécifiées sur une commande, elles remplacent les liaisons insérées contenues dans le DDL rendu persistant pour les objets spécifiés. Ces objets traités peuvent inclure l'objet nommé directement dans la commande **Process** , ou ils peuvent inclure d'autres objets dont le traitement est initialisé automatiquement en tant que partie du traitement.  
  
 Les liaisons hors ligne sont spécifiées en incluant l’objet de collection **Bindings** facultatif avec la commande de traitement. La collection **Bindings** facultative contient les éléments suivants.  
  
|Propriété|Cardinalité|Type| Description|  
|--------------|-----------------|----------|-----------------|  
|**Liaison**|0-n|**Binding**|Fournit une collection de nouvelles liaisons.|  
|**DataSource**|0-1|**DataSource**|Remplace l'élément **DataSource** du serveur qui aurait été utilisé.|  
|**DataSourceView**|0-1|**DataSourceView**|Remplace l'élément **DataSourceView** du<br /><br /> serveur qui aurait été utilisé.|  
  
 Tous les éléments qui sont en rapport avec les liaisons hors-ligne sont facultatifs. Pour tous les éléments non spécifiés, ASSL utilise la spécification contenue dans le DDL de l'objet rendu persistant. La spécification de **DataSource** ou de **DataSourceView** dans la commande **Process** est facultative. Si **DataSource** ou **DataSourceView** sont spécifiés, ils ne sont pas instanciés et ne sont pas rendus persistants une fois la commande **Process** terminée.  
  
### <a name="definition-of-the-out-of-line-binding-type"></a>Définition du type de liaison hors ligne  
 À l’intérieur de la collection **Bindings** hors ligne, ASSL prévoit une collection de liaisons pour les objets multiples, autorisant un élément **Binding**pour chacun. Chaque élément **Binding** a une référence d’objet étendue, qui est semblable à la référence d’objet mais qui peut également faire référence à des objets mineurs (par exemple, des attributs de dimension et des attributs de groupe de mesures). Cet objet prend la forme à deux dimensions typique de la **objet** élément **processus** commandes, sauf que la \< *objet*>\<*/objet*> balises ne sont pas présentes.  
  
 Chaque objet pour lequel la liaison est spécifiée est identifié par un élément XML sous la forme \< *objet*> ID (par exemple, **DimensionID**). Après avoir identifié l’objet aussi spécifiquement que possible sous la forme \< *objet*> ID, vous identifiez l’élément pour lequel la liaison est spécifiée, qui est généralement **Source**. Un scénario commun est celui où **Source** est une propriété de **DataItem**, ce qui est le cas pour les liaisons de colonne dans un attribut. Dans ce cas, vous ne spécifiez pas la balise **DataItem** ; au lieu de cela, vous spécifiez simplement la propriété **Source** , comme si elle était directement sur la colonne à lier.  
  
 Les éléments**KeyColumns** sont identifiés par leur classement à l'intérieur de la collection **KeyColumns** . Ici, il n'est pas possible de spécifier, par exemple, uniquement la première et la troisième colonne clé d'un attribut, parce qu'il n'y a aucun moyen d'indiquer que la deuxième colonne clé doit être ignorée. Toutes les colonnes clés doivent être présentes dans la liaison hors-ligne pour un attribut de dimension.  
  
 Les éléments**Translations**, bien qu'ils n'aient aucun ID, sont identifiés sémantiquement par leur langue. Par conséquent, les éléments **Translations** à l'intérieur d'un élément **Binding** doivent inclure leur identificateur de langue.  
  
 Un élément supplémentaire autorisé dans un élément **Binding** qui n'existe pas directement dans le DDL est **ParentColumnID**, utilisé pour les tables imbriquées pour l'exploration de données. Dans ce cas, il est nécessaire d'identifier la colonne parente dans la table imbriquée pour laquelle la liaison est fournie.  
  
  
