---
title: Index XML (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: xml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- removing indexes
- deleting indexes
- secondary indexes [XML in SQL Server]
- xml data type [SQL Server], indexes
- dropping indexes
- PATH index
- DROP_EXISTING clause
- XML [SQL Server], indexes
- primary indexes [XML in SQL Server]
- indexes [SQL Server], XML
- XML indexes [SQL Server], secondary
- BLOBs, XML indexes
- disabling indexes
- XML indexes [SQL Server], modifying
- XML indexes [SQL Server]
- XML indexes [SQL Server], primary
- modifying indexes
- XML indexes [SQL Server], dropping
- VALUE index
- XML indexes [SQL Server], xml data type
- PROPERTY index
- XML indexes [SQL Server], creating
ms.assetid: f5c9209d-b3f3-4543-b30b-01365a5e7333
caps.latest.revision: 59
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 7b5109d315e3c4bafdf7af1d1c8b1bf3734e3e6d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="xml-indexes-sql-server"></a>Index XML (SQL Server)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Des index XML peuvent être créés sur des colonnes de type **xml** . L'indexation porte sur les balises, les valeurs et les chemins d'accès rencontrés dans les instances XML de la colonne et contribue à l'optimisation des performances des requêtes. Votre application peut bénéficier d'un index XML dans les situations suivantes :  
  
-   Les requêtes portant sur des colonnes XML sont fréquentes dans votre charge de travail. Le coût de la maintenance des index XML au cours de la modification des données doit être pris en compte lors de l'évaluation des avantages.  
  
-   Vos valeurs XML sont relativement grandes et les parties récupérées relativement petites. En créant un index, vous n'avez plus à analyser l'ensemble des données lors de l'exécution et pouvez profiter de la recherche d'index pour accélérer le traitement des requêtes.  
  
 Les index XML sont classés en plusieurs catégories :  
  
-   Index XML primaires  
  
-   Index XML secondaires  
  
 Le premier index portant sur la colonne de type **xml** est obligatoirement l'index XML primaire. Par le biais de l'index XML primaire, les trois types d'index secondaires suivants sont pris en charge : PATH, VALUE, and PROPERTY. Selon le type de requêtes, ces index secondaires peuvent contribuer à améliorer les performances liées à l'exécution de requêtes.  
  
> [!NOTE]  
>  Vous ne pouvez pas créer ou modifier d'index XML à moins que les options de base de données ne soient définies correctement pour utiliser le type de données **xml** . Pour plus d’informations, consultez [Utiliser la recherche en texte intégral avec des colonnes XML](../../relational-databases/xml/use-full-text-search-with-xml-columns.md).  
  
 Les instances XML sont stockées dans les colonnes de type **xml** sous forme de BLOB (Binary Large Objects, objets volumineux binaires). Ces instances XML peuvent donc être volumineuses et la représentation binaire stockée d'instances de type **xml** peut atteindre jusqu'à 2 Go. Sans index, ces objets sont fragmentés au moment de l'exécution du programme afin d'évaluer une requête, ce qui peut prendre du temps. Examinons, par exemple, la requête suivante :  
  
```  
WITH XMLNAMESPACES ('http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS "PD")  
  
SELECT CatalogDescription.query('  
  /PD:ProductDescription/PD:Summary  
') as Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist ('/PD:ProductDescription/@ProductModelID[.="19"]') = 1  
```  
  
 Pour pouvoir sélectionner les instances XML satisfaisant la condition stipulée dans la clause `WHERE` , le BLOB XML se trouvant dans chaque ligne de la table `Production.ProductModel` est fragmenté au moment de l'exécution. L'expression `(/PD:ProductDescription/@ProductModelID[.="19"]`) tirée de la méthode `exist()` est ensuite évaluée. Une telle fragmentation à l'exécution peut être coûteuse selon la taille et le nombre d'instances stockées dans la colonne.  
  
 Si l’exécution de requêtes sur des BLOB est courante dans l’environnement de votre application, cette méthodologie permet d’indexer les colonnes de type **xml** . En contrepartie, le coût associé à la gestion de l'index lors de la modification des données est également à prendre en compte.  
  
## <a name="primary-xml-index"></a>Index XML primaires  
 L'index XML primaire indexe toutes les balises, valeurs et chemins d'accès rencontrés dans les instances XML d'une colonne XML. Pour créer un index XML primaire, la table contenant la colonne XML doit posséder un index cluster portant sur la clé primaire de la table. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise cette clé primaire pour corréler les lignes de l'index XML primaire avec des lignes dans la table qui contient la colonne XML.  
  
 L'index XML primaire correspond à une représentation fragmentée et persistante des objets blob XML inclus dans la colonne des données de type **xml** . Pour chacun de ces objets blob XML de la colonne, l'index crée plusieurs lignes de données. Le nombre de lignes dans l'index est presque égal au nombre de nœuds se trouvant dans l'objet blob XML. Lorsqu'une requête extrait l'intégralité de l'instance XML, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit l'instance à partir de la colonne XML. Les requêtes dans des instances XML utilisent l'index XML primaire et peuvent renvoyer des valeurs scalaires ou des sous-arborescences XML en se servant de l'index lui-même.  
  
 Chaque ligne stocke les informations suivantes relatives aux nœuds :  
  
-   Le nom de la balise tel que le nom d'un élément ou d'un attribut.  
  
-   La valeur du nœud.  
  
-   Le type de nœud, par exemple un nœud élément, un nœud attribut ou un nœud texte.  
  
-   Les informations sur l'ordre du document, représentées par un identifiant de nœud interne.  
  
-   Le chemin d'accès de chacun des nœuds vers la racine de l'arborescence XML. Cette colonne fait l'objet de recherches pour y trouver la présence d'expressions de chemin d'accès mentionnées dans la requête.  
  
-   La clé primaire de la table de base. Cette clé est copiée dans l'index XML primaire afin de pouvoir effectuer une jointure en retour avec la table de base et le nombre maximal de colonnes dans la clé primaire de la table de base est limité à 15.  
  
 Les informations de ce nœud sont utilisées afin d'évaluer et d'élaborer les résultats sous forme de données XML découlant d'une requête donnée. Pour des raisons d'optimisation, les informations relatives au nom de la balise et au type de nœud sont encodées sous forme de valeurs entières et la colonne Path s'appuie sur ce même encodage. En outre, les chemins d'accès sont stockés dans l'ordre inverse afin de pouvoir faire correspondre les chemins d'accès où seul le suffixe est connu. Exemple :  
  
-   `//ContactRecord/PhoneNumber` , où seuls les deux derniers niveaux sont connus ;  
  
 - ou -  
  
-   `/Book/*/Title` où le caractère générique (`*`) est mentionné au milieu de l’expression.  
  
 Le processeur de requêtes utilise l'index XML primaire dans le cas de requêtes mettant en œuvre des [xml Data Type Methods](../../t-sql/xml/xml-data-type-methods.md) et renvoie les valeurs scalaires ou les sous-arborescences XML tirées de l'index primaire lui-même (cet index stocke toutes les informations nécessaires afin de reconstruire l'instance XML).  
  
 Par exemple, la requête suivante renvoie les informations sommaires stockées dans la colonne `CatalogDescription` de type **xml** et provenant de la table `ProductModel`. Elle ne renvoie les informations dans la balise <`Summary`> que pour les modèles de produits dont la description de catalogue stocke également la description située dans la balise <`Features`>.  
  
```  
WITH XMLNAMESPACES ('http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS "PD")SELECT CatalogDescription.query('  /PD:ProductDescription/PD:Summary') as ResultFROM Production.ProductModelWHERE CatalogDescription.exist ('/PD:ProductDescription/PD:Features') = 1  
```  
  
 Concernant l'index XML primaire, au lieu de fragmenter chaque instance d'objet blob XML se trouvant dans la table de base, les lignes de l'index correspondant à chaque objet blob XML sont soumises à des recherches séquentielles pour retrouver l'expression indiquée dans la méthode `exist()`. Si le chemin d'accès est retrouvé dans la colonne Path de l'index, l'élément <`Summary`> ainsi que ses sous-arborescences sont extraits de l'index XML primaire, puis convertis en objet blob XML suite à l'exécution de la méthode `query()`.  
  
 Notez que l'index XML primaire n'est pas sollicité lors de la récupération d'une instance XML complète. Par exemple, la requête suivante extrait de la table l'instance XML tout entière décrivant les instructions de fabrication d'un modèle de produit donné.  
  
```  
USE AdventureWorks2012;SELECT InstructionsFROM Production.ProductModel WHERE ProductModelID=7;  
```  
  
## <a name="secondary-xml-indexes"></a>Index XML secondaires  
 Afin d'améliorer les performances lors des recherches, vous pouvez créer des index XML secondaires. Un index XML primaire doit être défini au préalable avant de pouvoir créer des index secondaires. Il en existe trois types différents :  
  
-   les index XML secondaires de type PATH (s'appuyant sur le chemin d'accès) ;  
  
-   les index XML secondaires de type VALUE (utilisant la valeur comme critère de recherche) ;  
  
-   les index XML secondaires de type PROPERTY (pour rechercher des données d'après leurs propriétés).  
  
 Voici quelques consignes pour vous aider à créer un ou plusieurs index secondaires :  
  
-   Si votre charge de travail utilise souvent des expressions de chemin sur les colonnes XML, l'index secondaire PATH a toutes les chances de l'accélérer. C’est ce que vous pouvez constater le plus souvent en cas d’utilisation de la méthode **exist()** sur des colonnes XML dans la clause WHERE de Transact-SQL.  
  
-   Si votre charge de travail récupère plusieurs valeurs d'instances XML individuelles en utilisant des expressions de chemin, le clustering des chemins pour chaque instance XML dans l'index PROPERTY peut s'avérer fort utile. Ce scénario se produit généralement avec un sac de propriétés où les propriétés d'un objet sont récupérées et la valeur de sa clé primaire est connue.  
  
-   Si votre charge de travail implique le lancement de requêtes sur des valeurs d'instances XML pour lesquelles vous ne connaissez pas les noms d'élément ou d'attribut, vous aurez peut-être intérêt à créer l'index VALUE. C’est généralement ce qui se produit en cas de recherches d’axes descendants telles que //author[last-name="Howard"], où les éléments \<author> peuvent se trouver à tout niveau de la hiérarchie. Cela se rencontre aussi dans les requêtes basées sur des caractères génériques telles que /book [@* = "novel"], où la requête recherche les éléments \<book> ayant des attributs de valeur « novel ».  
  
### <a name="path-secondary-xml-index"></a>les index XML secondaires de type PATH (s'appuyant sur le chemin d'accès) ;  
 Si vos requêtes précisent habituellement les expressions de chemin d'accès sur les colonnes de type **xml** , un index secondaire de type PATH peut s'avérer à même d'accélérer la recherche. Comme nous l’avons vu précédemment, l’index primaire est des plus utiles pour les requêtes indiquant la méthode **exist()** dans leur clause WHERE. Si vous ajoutez à présent un index secondaire de type PATH, il se peut que vous amélioriez encore la rapidité de la recherche lancée par de telles requêtes.  
  
 Bien qu'un index XML primaire évite de fragmenter les objets blob XML à l'exécution, il peut ne pas offrir les meilleurs temps de réponse pour les requêtes s'appuyant sur des expressions de chemin d'accès. Une recherche, lancée sur toutes les lignes constituant l'index XML primaire correspondant à un objet blob XML et s'opérant de façon séquentielle pour des instances XML volumineuses, peut s'avérer des plus lentes. Dans ce cas, un index secondaire construit sur les valeurs de chemin d'accès et sur celles des nœuds de l'index primaire peut accélérer de façon significative la recherche sur l'index. Dans le cas d'un index XML secondaire de type PATH, les valeurs de chemin d'accès et des nœuds correspondent à des colonnes clés permettant donc des recherches plus efficaces si ces dernières portent sur le chemin d'accès. Il se peut que l'optimiseur de requête utilise l'index de type PATH dans des expressions telles que celles mentionnées ci-dessous :  
  
-   `/root/Location` , n'indiquant que son chemin d'accès ;  
  
 - ou -  
  
-   `/root/Location/@LocationID[.="10"]` , où le chemin et la valeur du nœud sont précisés.  
  
 La requête suivante illustre un cas de figure où l'index de type PATH s'avère particulièrement utile :  
  
```  
WITH XMLNAMESPACES ('http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS "PD")  
  
SELECT CatalogDescription.query('  
  /PD:ProductDescription/PD:Summary  
') AS Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist ('/PD:ProductDescription/@ProductModelID[.="19"]') = 1  
```  
  
 Dans la requête, l'expression de chemin d'accès `/PD:ProductDescription/@ProductModelID` et sa valeur `"19"` se trouvant dans la méthode `exist()` correspondent aux champs de clé définis pour l'index de type PATH. Des recherches directes portant sur l'index PATH peuvent s'effectuer et offrir ainsi de meilleures performances que dans le cas d'une recherche séquentielle portant sur les valeurs de chemin d'accès de l'index primaire.  
  
### <a name="value-secondary-xml-index"></a>les index XML secondaires de type VALUE (utilisant la valeur comme critère de recherche) ;  
 Si des requêtes s'appuient sur les valeurs pour ses résultats, par exemple `/Root/ProductDescription/@*[. = "Mountain Bike"]` ou `//ProductDescription[@Name = "Mountain Bike"]`, et que le chemin n'est pas entièrement précisé ou qu'il contient un caractère générique, vous pourriez obtenir des résultats plus rapidement en construisant un index XML secondaire sur les valeurs des nœuds de l'index XML primaire.  
  
 Les colonnes clés (correspondant aux valeurs de nœud et aux chemins d'accès) de l'index de type VALUE sont tirées de l'index XML primaire. Si vous devez lancer des requêtes sur des valeurs provenant d'instances XML sans connaître le nom des éléments ou des attributs contenant les valeurs, il se peut que l'index de type VALUE vous soit particulièrement utile. Par exemple, l'expression suivante tire parti de l'utilisation d'un index de type VALUE :  
  
-   `//author[LastName="someName"]`, où vous connaissez la valeur de l'élément <`LastName`>, mais où le parent <`author`> peut se placer n'importe où.  
  
-   `/book[@* = "someValue"]`, où la requête recherche l'élément <`book`> dont un attribut possède la valeur `"someValue"`.  
  
 La requête suivante renvoie `ContactID` à partir de la table `Contact` . La clause `WHERE` indique un filtre permettant de rechercher des valeurs dans la colonne `AdditionalContactInfo` de type **xml**. L'ID des contacts n'est renvoyé que si l'objet blob XML relatif aux informations complémentaires sur les contacts correspondants contient un numéro de téléphone spécifique. Puisque l'élément <`telephoneNumber`> peut apparaître n'importe où dans le document XML, l'expression du chemin d'accès indique l'axe descendant-or-self.  
  
```  
WITH XMLNAMESPACES (  
  'http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo' AS CI,  
  'http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes' AS ACT)  
  
SELECT ContactID   
FROM   Person.Contact  
WHERE  AdditionalContactInfo.exist('//ACT:telephoneNumber/ACT:number[.="111-111-1111"]') = 1  
```  
  
 Dans ce cas, nous connaissons donc la valeur de recherche correspondant à <`number`> mais elle peut se trouver n'importe où dans l'instance XML en tant qu'enfant de l'élément <`telephoneNumber`>. Ce type de requête pourrait être plus efficace grâce à la recherche d'une valeur précise dans les index.  
  
### <a name="property-secondary-index"></a>Index secondaire de type PROPERTY  
 Les requêtes chargées d'extraire plusieurs valeurs d'instances XML distinctes peuvent bénéficier de l'utilisation d'un index de type PROPERTY. Un scénario type de cette utilisation s’illustre quand vous récupérez les propriétés d’objets par le biais de la méthode **value()** de type **xml** et que la valeur de la clé primaire de l’objet en question est connue.  
  
 L'index utilisant le paramètre PROPERTY se construit d'après des colonnes (PK pour la clé primaire, Path pour le chemin d'accès, ainsi que la valeur du nœud) issues de l'index XML primaire, où PK correspond à la clé primaire de la table de base.  
  
 Par exemple, concernant le modèle de produit `19`, la requête suivante extrait les valeurs des attributs `ProductModelID` et `ProductModelName` grâce à la méthode `value()` . Contrairement aux requêtes utilisant les index XML primaires ou tout autre index XML secondaire, l'index PROPERTY peut permettre une exécution plus rapide.  
  
```  
WITH XMLNAMESPACES ('http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS "PD")  
  
SELECT CatalogDescription.value('(/PD:ProductDescription/@ProductModelID)[1]', 'int') as ModelID,  
       CatalogDescription.value('(/PD:ProductDescription/@ProductModelName)[1]', 'varchar(30)') as ModelName          
FROM Production.ProductModel     
WHERE ProductModelID = 19  
```  
  
 À part les différences décrites plus loin dans cette rubrique, la création d’un index XML portant sur une colonne de type**xml** est semblable à celle d’un index portant sur une colonne de type non**xml** . Les instructions DDL [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes permettent de créer et de gérer les index XML :  
  
-   [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)  
  
-   [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)  
  
-   [DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)  
  
## <a name="getting-information-about-xml-indexes"></a>Obtention d'informations sur les index XML  
 Les entrées d'index XML apparaissent dans l'affichage catalogue, sys.indexes, avec l'index de « type » 3. La colonne name contient le nom de l'index XML.  
  
 Les index XML sont également enregistrés dans l'affichage catalogue, sys.xml_indexes, qui contient toutes les colonnes de sys.indexes, ainsi que d'autres plus spécifiques et utiles aux index XML. La valeur NULL de la colonne, secondary_type, indique qu'il s'agit d'un index XML primaire ; les valeurs « P », « R » et « V » représentent respectivement les index XML secondaires PATH, PROPERTY et VALUE.  
  
 L’espace utilisé par les index XML figure dans la fonction table [sys.dm_db_index_physical_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md), qui fournit des informations telles que le nombre de pages disque occupées, la taille moyenne des lignes en octets et le nombre d'enregistrements, pour tous les types d'index. Vous y trouvez également les index XML. Ces informations sont disponibles pour chaque partition de la base de données. Les index XML utilisent le même schéma et la même fonction de partitionnement que la table de base.  
  
## <a name="see-also"></a> Voir aussi  
 [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)   
 [Données XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md)  
  
  
