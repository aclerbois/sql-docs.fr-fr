---
title: Utiliser des jeux de colonnes | Microsoft Docs
ms.custom: ''
ms.date: 07/30/2015
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- sparse columns, column sets
- column sets
ms.assetid: a4f9de95-dc8f-4ad8-b957-137e32bfa500
caps.latest.revision: 28
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 221ee8a61bb5c433332b1cf293d019d849e20568
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-column-sets"></a>Utiliser des jeux de colonnes
[!INCLUDE[tsql-appliesto-ss2016-all-md](../../includes/tsql-appliesto-ss2016-all-md.md)]

  Les tables qui utilisent des colonnes éparses peuvent désigner un jeu de colonnes pour retourner toutes les colonnes éparses dans la table. Un jeu de colonnes est une représentation XML non typée qui combine toutes les colonnes éparses d'une table dans une sortie structurée. Un jeu de colonnes est semblable à une colonne calculée, dans la mesure où le jeu de colonnes n'est pas stocké physiquement dans la table. Un jeu de colonnes diffère d'une colonne calculée, dans le sens où le jeu de colonnes est peut être mis à jour directement.  
  
 Vous devez envisager d'utiliser des jeux de colonnes lorsque le nombre de colonnes dans une table est élevé et qu'il serait trop long d'opérer individuellement sur chacune d'elles. Les applications peuvent présenter une amélioration des performances lorsqu'elles sélectionnent et insèrent des données à l'aide de jeux de colonnes sur des tables qui contiennent de nombreuses colonnes. Toutefois, les performances des jeux de colonnes peuvent être réduites lorsque de nombreux index sont définis sur les colonnes de la table. Cela est dû au fait que la quantité de mémoire requise pour un plan d'exécution augmente.  
  
 Pour définir un jeu de colonnes, utilisez les mots clés *<nom_jeu_colonnes>* FOR ALL_SPARSE_COLUMNS dans les instructions [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) ou [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
## <a name="guidelines-for-using-column-sets"></a>Indications pour utiliser des jeux de colonnes  
 Lorsque vous utilisez des jeux de colonnes, considérez les indications suivantes :  
  
-   Les colonnes éparses et un jeu de colonnes peuvent être ajoutés dans le cadre de la même instruction.  
  
-   Un jeu de colonnes ne peut pas être ajouté à une table si celle-ci contient déjà des colonnes éparses.  
  
-   Le jeu de colonnes ne peut pas être modifié. Pour modifier un jeu de colonnes, vous devez le supprimer et recréer les colonnes éparses et le jeu de colonnes.  
  
-   Un jeu de colonnes peut être ajouté à une table qui n'inclut pas de colonnes éparses. Si des colonnes éparses sont ajoutées ultérieurement à la table, elles apparaîtront dans le jeu de colonnes.  
  
-   Un seul jeu de colonnes est autorisé par table.  
  
-   Un jeu de colonnes est facultatif et n'est pas nécessaire pour pouvoir utiliser des colonnes éparses.  
  
-   Il est impossible de définir des contraintes ou des valeurs par défaut sur un jeu de colonnes.  
  
-   Les colonnes calculées ne peuvent pas contenir de colonnes de jeu de colonnes.  
  
-   Les requêtes distribuées ne sont pas prises en charge sur les tables qui contiennent des jeux de colonnes.  
  
-   La réplication ne prend pas en charge les jeux de colonnes.  
  
-   La capture de données modifiées ne prend pas en charge les jeux de colonnes.  
  
-   Un jeu de colonnes ne peut faire partie d'aucun type d'index. Cela inclut les index XML, les index de recherche en texte intégral et les vues indexées. Un jeu de colonnes ne peut pas être ajouté en tant que colonne incluse dans un index.  
  
-   Un jeu de colonnes ne peut pas être utilisé dans l'expression de filtre d'un index filtré ou de statistiques filtrées.  
  
-   Lorsqu'une vue inclut un jeu de colonnes, celui-ci apparaît dans la vue comme une colonne XML.  
  
-   Un jeu de colonnes ne peut pas être inclus dans une définition de vue indexée.  
  
-   Les vues partitionnées qui incluent des tables qui contiennent des jeux de colonnes peuvent être mises à jour lorsque la vue partitionnée spécifie les colonnes éparses par nom. Une vue partitionnée ne peut pas être mise à jour lorsqu'elle fait référence au jeu de colonnes.  
  
-   Les notifications de requêtes qui renvoient à des jeux de colonnes ne sont pas autorisées.  
  
-   Les données XML ont une limite de taille de 2 Go. Si les données combinées de toutes les colonnes éparses non nulles dans une ligne dépassent cette limite, la requête ou opération DML génère une erreur.  
  
-   Pour plus d’informations sur les données retournées par la fonction COLUMNS_UPDATED, consultez [Utiliser des colonnes éparses](../../relational-databases/tables/use-sparse-columns.md).  
  
## <a name="guidelines-for-selecting-data-from-a-column-set"></a>Indications pour la sélection de données dans un jeu de colonnes  
 Considérez les indications suivantes lors de la sélection de données dans un jeu de colonnes :  
  
-   D'un point de vue conceptuel, un jeu de colonnes est un type de colonne XML calculé pouvant être mis à jour, qui regroupe un jeu de colonnes relationnelles sous-jacentes dans une représentation XML unique. Le jeu de colonnes prend en charge uniquement la propriété ALL_SPARSE_COLUMNS. Cette propriété est utilisée pour regrouper toutes les valeurs non nulles de toutes les colonnes éparses pour une ligne particulière.  
  
-   Dans l'éditeur de tables [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , les jeux de colonnes sont affichés sous forme de champ XML modifiable. Définissez les jeux de colonnes au format suivant :  
  
    ```  
    <column_name_1>value1</column_name_1><column_name_2>value2</column_name_2>...  
    ```  
  
     Voici quelques exemples de valeurs de jeu de colonnes :  
  
    -   `<sparseProp1>10</sparseProp1><sparseProp3>20</sparseProp3>`  
  
    -   `<DocTitle>Bicycle Parts List</DocTitle><Region>West</Region>`  
  
-   Les colonnes éparses qui contiennent des valeurs NULL sont omises de la représentation XML pour le jeu de colonnes.  
  
> [!WARNING]  
>  L'ajout d'un jeu de colonnes modifie le comportement des requêtes SELECT *. La requête retournera le jeu de colonnes sous forme de colonne XML et ne retournera pas les colonnes éparses individuelles. Les concepteurs de schémas et développeurs de logiciels doivent prendre soin de ne pas arrêter les applications existantes.  
  
## <a name="inserting-or-modifying-data-in-a-column-set"></a>Insertion ou modification de données dans un jeu de colonnes  
 La manipulation des données d'une colonne éparse peut s'effectuer en utilisant le nom de chaque colonne ou en faisant référence au nom du jeu de colonnes et en spécifiant les valeurs du jeu de colonnes à l'aide du format XML du jeu de colonnes. Les colonnes éparses peuvent apparaître dans n'importe quel ordre dans la colonne XML.  
  
 Lorsque des valeurs de colonnes éparses sont insérées ou mises à jour à l'aide du jeu de colonnes XML, les valeurs insérées dans les colonnes éparses sous-jacentes sont converties implicitement à partir du type de données **xml** . Dans le cas des colonnes numériques, une valeur vierge dans le XML pour la colonne numérique est convertie en chaîne vide. Cela provoque l'insertion d'un zéro dans la colonne numérique, comme illustré dans l'exemple suivant.  
  
```  
CREATE TABLE t (i int SPARSE, cs xml column_set FOR ALL_SPARSE_COLUMNS);  
GO  
INSERT t(cs) VALUES ('<i/>');  
GO  
SELECT i FROM t;  
GO  
```  
  
 Dans cet exemple, aucune valeur n'a été spécifiée pour le `i`de colonne, mais la valeur `0` a été insérée.  
  
## <a name="using-the-sqlvariant-data-type"></a>Utilisation du Type de données sql_variant  
 Le type de données **sql_variant** peut stocker plusieurs types de données différents, tels que **int**, **char**et **date**. Les jeux de colonnes retournent les informations de type de données telles que l’échelle, la précision et les informations relatives aux paramètres régionaux associées à une valeur **sql_variant** sous la forme d’attributs dans la colonne XML générée. Si vous essayez de fournir ces attributs dans une instruction XML générée de manière personnalisée en tant qu'entrée pour une opération d'insertion ou de mise à jour sur un jeu de colonnes, certains de ces attributs sont requis et une valeur par défaut est assignée à certains d'entre eux. Le tableau suivant répertorie les types de données et les valeurs par défaut générées par le serveur lorsque la valeur n'est pas fournie.  
  
|Type de données|localeID*|sqlCompareOptions|sqlCollationVersion|SqlSortId|Longueur maximale|Précision|Échelle|  
|---------------|----------------|-----------------------|-------------------------|---------------|--------------------|---------------|-----------|  
|**char**, **varchar**, **binary**|-1|'Par défaut'|0|0|8000|Non applicable**|Non applicable|  
|**nvarchar**|-1|'Par défaut'|0|0|4000|Non applicable|Non applicable|  
|**decimal**, **float**, **real**|Non applicable|Non applicable|Non applicable|Non applicable|Non applicable|18|0|  
|**integer**, **bigint**, **tinyint**, **smallint**|Non applicable|Non applicable|Non applicable|Non applicable|Non applicable|Non applicable|Non applicable|  
|**datetime2**|Non applicable|Non applicable|Non applicable|Non applicable|Non applicable|Non applicable|7|  
|**datetime offset**|Non applicable|Non applicable|Non applicable|Non applicable|Non applicable|Non applicable|7|  
|**datetime**, **date**, **smalldatetime**|Non applicable|Non applicable|Non applicable|Non applicable|Non applicable|Non applicable|Non applicable|  
|**money**, **smallmoney**|Non applicable|Non applicable|Non applicable|Non applicable|Non applicable|Non applicable|Non applicable|  
|**time**|Non applicable|Non applicable|Non applicable|Non applicable|Non applicable|Non applicable|7|  
  
 \*  localeID -1 correspond aux paramètres régionaux par défaut. Les paramètres régionaux pour l'anglais sont 1033.  
  
 ** Non applicable = Aucune valeur n’est sortie pour ces attributs durant une opération de sélection sur le jeu de colonnes. Génère une erreur lorsqu'une valeur est spécifiée pour cet attribut par l'appelant dans la représentation XML fournie pour un jeu de colonnes dans une opération d'insertion ou de mise à jour.  
  
## <a name="security"></a>Sécurité  
 Le modèle de sécurité pour un jeu de colonnes fonctionne de manière semblable au modèle de sécurité qui existe entre la table et les colonnes. Les jeux de colonnes peuvent être visualisés comme une minitable et une opération de sélection est semblable à uneopération SELECT * sur cette minitable. Mais la relation entre le jeu de colonnes et les colonnes éparses est une relation de regroupement plutôt qu'un conteneur strict. Le modèle de sécurité vérifie la sécurité sur la colonne de jeu de colonnes et honore les opérations DENY sur les colonnes éparses sous-jacentes. Les caractéristiques supplémentaires du modèle de sécurité sont les suivantes :  
  
-   Des autorisations de sécurité peuvent être accordées et révoquées pour la colonne de jeu de colonnes, comme pour toute autre colonne dans la table.  
  
-   Une valeur GRANT ou REVOKE d'autorisation SELECT, INSERT, UPDATE, DELETE et REFERENCES sur une colonne de jeu de colonnes n'est pas propagée aux colonnes membres sous-jacentes de ce jeu. Elle s'applique uniquement à l'utilisation de la colonne de jeu de colonnes. L'autorisation DENY sur un jeu de colonnes est propagée aux colonnes éparses sous-jacentes de la table.  
  
-   L'exécution d'instructions SELECT, INSERT, UPDATE et DELETE sur la colonne de jeu de colonnes requiert que l'utilisateur ait des autorisations correspondantes sur la colonne de jeu de colonnes et également l'autorisation correspondante sur toutes les colonnes éparses dans la table. Étant donné que le jeu de colonnes représente toutes les colonnes éparses dans la table, vous devez avoir l'autorisation sur toutes les colonnes éparses, y compris celles que vous ne modifierez peut-être pas.  
  
-   L'exécution d'une instruction REVOKE sur une colonne éparse ou un jeu de colonnes provoque l'application par défaut de la sécurité à leur objet parent.  
  
## <a name="examples"></a>Exemples  
 Dans les exemples suivants, une table de documents contient le jeu de colonnes commun `DocID` et `Title`. Le groupe Production souhaite avoir une colonne `ProductionSpecification` et `ProductionLocation` pour tous les documents de production. Le groupe Marketing souhaite avoir une colonne `MarketingSurveyGroup` pour les documents de marketing.  
  
### <a name="a-creating-a-table-that-has-a-column-set"></a>A. Création d'une table qui a un jeu de colonnes  
 L'exemple suivant crée la table qui utilise des colonnes éparses et inclut le jeu de colonnes `SpecialPurposeColumns`. L'exemple insère deux lignes dans la table, puis sélectionne des données de la table.  
  
> [!NOTE]  
>  Cette table ne possède que cinq colonnes, de manière à simplifier son affichage et sa lecture.  
  
```  
USE AdventureWorks2012;  
GO  
  
CREATE TABLE DocumentStoreWithColumnSet  
    (DocID int PRIMARY KEY,  
     Title varchar(200) NOT NULL,  
     ProductionSpecification varchar(20) SPARSE NULL,  
     ProductionLocation smallint SPARSE NULL,  
     MarketingSurveyGroup varchar(20) SPARSE NULL,  
     MarketingProgramID int SPARSE NULL,  
     SpecialPurposeColumns XML COLUMN_SET FOR ALL_SPARSE_COLUMNS);  
GO  
```  
  
### <a name="b-inserting-data-to-a-table-by-using-the-names-of-the-sparse-columns"></a>B. Insertion de données dans une table en utilisant les noms des colonnes éparses  
 Les exemples suivants insèrent deux lignes dans la table créée dans l'exemple A. Les exemples utilisent les noms des colonnes éparses et ne font pas référence au jeu de colonnes.  
  
```  
INSERT DocumentStoreWithColumnSet (DocID, Title, ProductionSpecification, ProductionLocation)  
VALUES (1, 'Tire Spec 1', 'AXZZ217', 27);  
GO  
  
INSERT DocumentStoreWithColumnSet (DocID, Title, MarketingSurveyGroup)  
VALUES (2, 'Survey 2142', 'Men 25 - 35');  
GO  
```  
  
### <a name="c-inserting-data-to-a-table-by-using-the-name-of-the-column-set"></a>C. Insertion de données dans une table en utilisant le nom du jeu de colonnes  
 L'exemple suivant insère une troisième ligne dans la table créée dans l'exemple A. Cette fois, les noms des colonnes éparses ne sont pas utilisés. Au lieu de cela, le nom du jeu de colonnes est utilisé et l'insertion fournit les valeurs pour deux des quatre colonnes éparses au format XML.  
  
```  
INSERT DocumentStoreWithColumnSet (DocID, Title, SpecialPurposeColumns)  
VALUES (3, 'Tire Spec 2', '<ProductionSpecification>AXW9R411</ProductionSpecification><ProductionLocation>38</ProductionLocation>');  
GO  
```  
  
### <a name="d-observing-the-results-of-a-column-set-when-select--is-used"></a>D. Observation des résultats d'un jeu de colonnes lorsque SELECT * est utilisé  
 L'exemple suivant sélectionne toutes les colonnes de la table qui contient un jeu de colonnes. Il retourne une colonne XML avec les valeurs combinées des colonnes éparses. Il ne retourne pas les colonnes éparses individuellement.  
  
```  
SELECT DocID, Title, SpecialPurposeColumns FROM DocumentStoreWithColumnSet ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `DocID Title        SpecialPurposeColumns`  
  
 `1      Tire Spec 1  <ProductionSpecification>AXZZ217</ProductionSpecification><ProductionLocation>27</ProductionLocation>`  
  
 `2      Survey 2142  <MarketingSurveyGroup>Men 25 - 35</MarketingSurveyGroup>`  
  
 `3      Tire Spec 2  <ProductionSpecification>AXW9R411</ProductionSpecification><ProductionLocation>38</ProductionLocation>`  
  
### <a name="e-observing-the-results-of-selecting-the-column-set-by-name"></a>E. Observation des résultats de la sélection du jeu de colonnes par nom  
 Le département Production ne s'intéressant pas aux données de marketing, cet exemple ajoute une clause `WHERE` afin de restreindre la sortie. L'exemple utilise le nom du jeu de colonnes.  
  
```  
SELECT DocID, Title, SpecialPurposeColumns  
FROM DocumentStoreWithColumnSet  
WHERE ProductionSpecification IS NOT NULL ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `DocID Title        SpecialPurposeColumns`  
  
 `1     Tire Spec 1  <ProductionSpecification>AXZZ217</ProductionSpecification><ProductionLocation>27</ProductionLocation>`  
  
 `3     Tire Spec 2  <ProductionSpecification>AXW9R411</ProductionSpecification><ProductionLocation>38</ProductionLocation>`  
  
### <a name="f-observing-the-results-of-selecting-sparse-columns-by-name"></a>F. Observation des résultats de la sélection des colonnes éparses par nom  
 Lorsqu'une table contient un jeu de colonnes, vous pouvez tout de même interroger la table en utilisant les noms de colonnes individuellement, comme illustré dans l'exemple suivant.  
  
```  
SELECT DocID, Title, ProductionSpecification, ProductionLocation   
FROM DocumentStoreWithColumnSet  
WHERE ProductionSpecification IS NOT NULL ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `DocID Title        ProductionSpecification ProductionLocation`  
  
 `1     Tire Spec 1  AXZZ217                 27`  
  
 `3     Tire Spec 2  AXW9R411                38`  
  
### <a name="g-updating-a-table-by-using-a-column-set"></a>G. Mise à jour d'une table en utilisant un jeu de colonnes  
 L'exemple suivant met à jour le troisième enregistrement avec les nouvelles valeurs pour les deux colonnes éparses utilisées par cette ligne.  
  
```  
UPDATE DocumentStoreWithColumnSet  
SET SpecialPurposeColumns = '<ProductionSpecification>ZZ285W</ProductionSpecification><ProductionLocation>38</ProductionLocation>'  
WHERE DocID = 3 ;  
GO  
```  
  
> [!IMPORTANT]  
>  Une instruction UPDATE qui utilise un jeu de colonnes met à jour toutes les colonnes éparses dans la table. Les colonnes éparses qui ne sont pas référencées sont mises à jour avec la valeur NULL.  
  
 L'exemple suivant met à jour le troisième enregistrement, mais spécifie uniquement la valeur d'une des deux colonnes remplies. La deuxième colonne `ProductionLocation` n'est pas incluse dans l'instruction `UPDATE` et est mise à jour avec la valeur NULL.  
  
```  
UPDATE DocumentStoreWithColumnSet  
SET SpecialPurposeColumns = '<ProductionSpecification>ZZ285W</ProductionSpecification>'  
WHERE DocID = 3 ;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Utiliser des colonnes éparses](../../relational-databases/tables/use-sparse-columns.md)  
  
  
