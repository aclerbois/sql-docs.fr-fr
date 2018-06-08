---
title: Relations | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: b704b7e2fdc299006d77e08314d2b16ffd750a0a
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="relationships"></a>Relations 
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Dans les modèles tabulaires, une relation est une connexion entre deux tables de données. La relation établit la façon dont les données des deux tables doivent être mises en corrélation. Par exemple, il est possible de créer une relation entre une table Clients et une table Commandes afin d'indiquer le nom du client associé à chaque commande.  
  
 Lors de l'utilisation de l'Assistant Importation de table pour effectuer une importation depuis la même source de données, les relations qui existent déjà dans les tables (à la source de données) que vous choisissez d'importer seront recréées dans le modèle. Vous pouvez afficher les relations détectées et recréées automatiquement à l'aide du générateur de modèles dans la vue de diagramme, ou en utilisant la boîte de dialogue Gérer les relations. Vous pouvez également créer des relations entre les tables manuellement à l'aide du générateur de modèles dans la vue de diagramme, ou en utilisant la boîte de dialogue Créer une relation ou Gérer les relations.  
  
 Après avoir défini les relations entre les tables, automatiquement lors de l'importation ou créées manuellement, vous serez en mesure de filtrer les données à l'aide de colonnes associées et de rechercher des valeurs dans les tables associées.  
  
> [!TIP]  
>  Si votre modèle contient plusieurs relations, la vue de diagramme peut vous aider à mieux visualiser et créer les relations entre les tables.  
  
  
##  <a name="what"></a> Avantages  
 Une relation est une connexion entre deux tables de données, basée sur une ou plusieurs colonnes dans chaque table. Pour comprendre pourquoi les relations sont utiles, imaginez que vous effectuez le suivi des données des commandes client dans votre entreprise. Vous pouvez effectuer le suivi de toutes les données dans une table individuelle possédant une structure similaire à :  
  
|CustomerID|Nom|EMail|DiscountRate|OrderID|OrderDate|Product|Quantité|  
|----------------|----------|-----------|------------------|-------------|---------------|-------------|--------------|  
|1|Ashton|chris.ashton@contoso.com|.05|256|2010-01-07|Compact Digital|11|  
|1|Ashton|chris.ashton@contoso.com|.05|255|2010-01-03|SLR Camera|15|  
|2|Jaworski|michal.jaworski@contoso.com|.10|254|2010-01-03|Budget Movie-Maker|27|  
  
 Cette approche peut fonctionner, mais elle implique le stockage de nombreuses données redondantes, telles que l'adresse de messagerie du client pour chaque commande. Le stockage est bon marché, mais vous devez vous assurer de mettre à jour chaque ligne pour ce client si l'adresse de messagerie change. Une solution à ce problème consiste à fractionner les données en plusieurs tables et à définir des relations entre ces tables. C’est l’approche utilisée dans *bases de données relationnelles* que SQL Server. Par exemple, une base de données que vous importez dans un modèle peut représenter les données des commandes en utilisant trois tables associées :  
  
### <a name="customers"></a>Clients (Customers)  
  
|[CustomerID]|Nom|EMail|  
|--------------------|----------|-----------|  
|1|Ashton|chris.ashton@contoso.com|  
|2|Jaworski|michal.jaworski@contoso.com|  
  
### <a name="customerdiscounts"></a>CustomerDiscounts  
  
|[CustomerID]|DiscountRate|  
|--------------------|------------------|  
|1|.05|  
|2|.10|  
  
### <a name="orders"></a>Orders  
  
|[CustomerID]|OrderID|OrderDate|Product|Quantité|  
|--------------------|-------------|---------------|-------------|--------------|  
|1|256|2010-01-07|Compact Digital|11|  
|1|255|2010-01-03|SLR Camera|15|  
|2|254|2010-01-03|Budget Movie-Maker|27|  
  
 Si vous importez ces tables à partir de la même base de données, l’Assistant Importation de table peut détecter les relations entre les tables en fonction des colonnes qui sont entre [crochets] et reproduire ces relations dans le générateur de modèles. Pour plus d'informations, consultez [« Détection automatique et inférence des relations »](#detection) dans cette rubrique. Si vous importez des tables provenant de plusieurs sources, vous pouvez créer manuellement des relations, comme décrit dans [créer une relation entre deux Tables](../../analysis-services/tabular-models/create-a-relationship-between-two-tables-ssas-tabular.md).  
  
### <a name="columns-and-keys"></a>Colonnes et clés  
 Les relations sont basées sur les colonnes des tables qui contiennent les mêmes données. Par exemple, les tables Customers et Orders peuvent être associées l'une à l'autre car elles contiennent toutes les deux une colonne qui stocke un ID de client. Dans cet exemple, les noms des colonnes sont les mêmes, mais ce n'est pas nécessaire. L'un deux peut être CustomerID et un autre CustomerNumber, tant que toutes les lignes de la table Orders contiennent un ID qui est également stocké dans la table Customers.  
  
 Dans une base de données relationnelle, il existe plusieurs types de *clés*, qui sont en général simplement des colonnes avec des propriétés spéciales. Ces quatre types de clés peuvent être utilisés dans des bases de données relationnelles :  
  
-   *Clé primaire*: identifie de façon unique une ligne dans une table, telle que CustomerID dans la table Customers.  
  
-   *Clé secondaire* (ou *clé candidate*) : une colonne autre que la clé primaire qui est unique. Par exemple, une table Employees peut stocker un ID d'employé et un numéro de sécurité sociale, qui sont tous les deux uniques.  
  
-   *Clé étrangère*: une colonne qui fait référence à une colonne unique dans une autre table, telle que CustomerID dans la table Orders, laquelle fait référence à CustomerID dans la table Customers.  
  
-   *Clé composite*: une clé composée de plusieurs colonnes. Les clés composites ne sont pas prises en charge dans les modèles tabulaires. Pour plus d'informations, consultez « Clés composites et colonnes de recherche » dans cette rubrique.  
  
 Dans les modèles tabulaires, la clé primaire ou la clé secondaire est dite *colonne de recherche associée*ou simplement *colonne de recherche*. Si une table possède à la fois une clé primaire et une clé secondaire, vous pouvez utiliser l'une ou l'autre comme colonne de recherche. La clé étrangère est connue sous le nom de *colonne source* ou plus simplement *colonne*. Dans notre exemple, une relation serait définie entre CustomerID dans la table Orders (la colonne) et CustomerID (la colonne de recherche) dans la table Customers. Si vous importez des données à partir d'une base de données relationnelle, le générateur de modèles choisit par défaut la clé étrangère dans une table et la clé primaire correspondante dans l'autre table. Toutefois, vous pouvez utiliser toute colonne possédant des valeurs uniques comme colonne de recherche.  
  
### <a name="types-of-relationships"></a>Types de relations  
 La relation entre les clients et les commandes est une *relation un-à-plusieurs*. Chaque client peut avoir plusieurs commandes, mais une commande ne peut pas avoir plusieurs clients. Les autres types de relations sont *un-à-un* et *plusieurs à plusieurs*. La table CustomerDiscounts, qui définit un taux d'escompte unique pour chaque client, a une relation un-à-un avec la table Customers. Un exemple de relation plusieurs à plusieurs est une relation directe entre Products et Customers, dans laquelle un client peut acheter plusieurs produits et un même produit peut être acheté par plusieurs clients. Le Générateur de modèles ne prend pas en charge les relations plusieurs-à-plusieurs dans l'interface utilisateur. Pour plus d'informations, consultez «[Relations plusieurs-à-plusieurs](#bkmk_many_to_many)» dans cette rubrique.  
  
 Le tableau ci-dessous indique les relations entre les trois tables :  
  
|Relation|Type|colonne de recherche|colonne|  
|------------------|----------|-------------------|------------|  
|Customers-CustomerDiscounts|un-à-un|Customers.CustomerID|CustomerDiscounts.CustomerID|  
|Customers-Orders|un-à-plusieurs|Customers.CustomerID|Orders.CustomerID|  
  
### <a name="relationships-and-performance"></a>Relations et performances  
 Après la création de toute relation, le générateur de modèles doit normalement recalculer toutes les formules qui utilisent des colonnes des tables de la relation nouvellement créée. Le traitement peut prendre du temps, selon la quantité de données et la complexité des relations.  
  
##  <a name="requirements"></a> Requirements for relationships  
 Le générateur de modèles présente plusieurs conditions qui doivent être remplies lors de la création de relations :  
  
### <a name="single-active-relationship-between-tables"></a>Relation active unique entre des tables  
 Plusieurs relations pourraient générer des dépendances ambiguës entre des tables. Pour créer des calculs exacts, vous avez besoin d'un chemin d'accès unique entre une table et la table suivante. Par conséquent, il ne peut y avoir qu'une seule relation active entre chaque paire de tables. Par exemple, dans AdventureWorks DW 2012, la table DimDate contient une colonne DateKey qui est associée à trois colonnes différentes dans la table FactInternetSales : OrderDate, DueDate et ShipDate. Si vous tentez d'importer ces tables, la première relation est créée avec succès, mais vous recevrez l'erreur suivante sur les relations consécutives qui impliquent la même colonne :  
  
 \* Relation : table [colonne 1] -> table [colonne 2] - état : erreur - raison : Impossible de créer une relation entre tables \<table 1 > et \<tableau 2 >. Une seule relation directe ou indirecte peut exister entre deux tables.  
  
 Si vous avez deux tables et plusieurs relations entre elles, vous devez importer plusieurs copies de la table qui contient la colonne de recherche et créer une relation entre chaque paire de tables.  
  
 Il peut y avoir plusieurs relations inactives entre les tables. Le chemin d'accès à utiliser entre les tables est spécifié par le client de création de rapports au moment de la requête.  
  
### <a name="one-relationship-for-each-source-column"></a>Une relation pour chaque colonne source  
 Une colonne source ne peut pas participer à plusieurs relations. Si vous avez déjà utilisé une colonne comme colonne source dans une relation, mais souhaitez utiliser cette colonne pour la connexion à une autre colonne de recherche connexe dans une table différente, vous pouvez créer une copie de la colonne et utiliser cette colonne pour la nouvelle relation.  
  
 Il est facile de créer une copie d'une colonne qui a exactement les mêmes valeurs, en utilisant une formule DAX dans une colonne calculée. Pour plus d’informations, consultez [créer une colonne calculée](../../analysis-services/tabular-models/ssas-calculated-columns-create-a-calculated-column.md).  
  
### <a name="unique-identifier-for-each-table"></a>Identificateur unique pour chaque table  
 Chaque table doit avoir une colonne unique qui identifie de façon unique chaque ligne dans cette table. Cette colonne est communément appelée clé primaire.  
  
### <a name="unique-lookup-columns"></a>Colonnes de recherche uniques  
 Les valeurs des données dans la colonne de recherche doivent être uniques. En d'autres termes, la colonne ne doit pas contenir de doublons. Dans les modèles tabulaires, les valeurs Null et les chaînes vides sont équivalentes à un espace, qui est une valeur de donnée distincte. Cela signifie que vous ne pouvez pas avoir plusieurs valeurs Null dans la colonne de recherche.  
  
### <a name="compatible-data-types"></a>Types de données compatibles  
 Les types de données dans la colonne source et la colonne de recherche doivent être compatibles. Pour plus d’informations sur les types de données, consultez [prise en charge des Types de données](../../analysis-services/tabular-models/data-types-supported-ssas-tabular.md).  
  
### <a name="composite-keys-and-lookup-columns"></a>Clés composites et colonnes de recherche  
 Vous ne pouvez pas utiliser de clés composites dans un modèle tabulaire ; vous devez toujours avoir une colonne qui identifie de façon unique chaque ligne dans la table. Si vous essayez d'importer des tables qui ont une relation existante basée sur une clé composite, l'Assistant Importation de table ignore cette relation, car elle ne peut pas être créée dans un modèle tabulaire.  
  
 Si vous souhaitez créer une relation entre deux tables dans le générateur de modèles et que plusieurs colonnes définissent les clés étrangère et primaire, vous devez associer les valeurs pour créer une colonne clé unique avant de créer la relation. Vous pouvez le faire avant d'importer les données, ou dans le générateur de modèles en créant une colonne calculée.  
  
###  <a name="bkmk_many_to_many"></a> Many-to-Many relationships  
 Les modèles tabulaires ne prennent pas en charge les relations plusieurs à plusieurs, et vous ne pouvez pas ajouter de *tables de jointure* dans le générateur de modèles. Toutefois, vous pouvez utiliser les fonctions DAX pour modéliser des relations plusieurs à plusieurs.  
  
 Vous pouvez également essayer de configurer un filtre croisé bidirectionnel pour déterminer s’il atteint le même objectif. Parfois, la configuration requise de la relation plusieurs-à-plusieurs peut être satisfaite par le biais des filtres croisés qui persistent un contexte de filtre plusieurs relations entre les tables. Pour plus d’informations, consultez [Filtres croisés bidirectionnels pour modèles tabulaires dans SQL Server 2016 Analysis Services](../../analysis-services/tabular-models/bi-directional-cross-filters-tabular-models-analysis-services.md) .  
  
### <a name="self-joins-and-loops"></a>Jointures réflexives et boucles  
 Les jointures réflexives ne sont pas autorisées dans les tables de modèle tabulaire. Une jointure réflexive est une relation récursive entre une table et elle-même. Les jointures réflexives sont souvent utilisées pour définir des hiérarchies de type parent-enfant. Par exemple, vous pouvez joindre une table Employees à elle-même pour produire une hiérarchie qui indique la chaîne de gestion dans une entreprise.  
  
 Le générateur de modèles n'autorise pas la création de boucles parmi des relations dans un modèle. En d'autres termes, l'ensemble suivant de relations est interdit.  
  
 Table 1, colonne a   à   Table 2, colonne f  
  
 Table 2, colonne f   à   Table 3, colonne n  
  
 Table 3, colonne n   à   Table 1, colonne a  
  
 Si vous essayez de créer une relation qui entraînerait la création d'une boucle, une erreur est générée.  
  
##  <a name="detection"></a> Inference of relationships  
 Dans certains cas, les relations entre les tables sont automatiquement chaînées. Par exemple, si vous créez une relation entre les deux premiers ensembles de tables ci-dessous, il est déduit qu'une relation existe entre les deux autres tables et une relation est établie automatiquement.  
  
 Products et Category -- relation créée manuellement  
  
 Category et SubCategory -- relation créée manuellement  
  
 Products et SubCategory -- relation inférée  
  
 Pour que des relations soient chaînées automatiquement, elles doivent avoir une même direction, comme dans l'exemple ci-dessus. Si les relations initiales sont, par exemple, entre les ventes et les produits et entre les ventes et les clients, aucune relation n'est inférée. En effet, la relation entre les produits et les clients est une relation plusieurs à plusieurs.  
  
##  <a name="bkmk_detection"></a> Detection of relationships when importing data  
 Lorsque vous importez des données depuis une table de source de données relationnelle, l'Assistant Importation de table détecte les relations existantes dans ces tables source en fonction des données de schéma source. Si des tables associées sont importées, ces relations seront dupliquées dans le modèle.  
  
##  <a name="bkmk_manually_create"></a> Manually create relationships  
 Alors que la plupart des relations entre les tables d'une source de données relationnelle sont détectées automatiquement et créées dans le modèle tabulaire, il existe également de nombreux cas où vous devez créer manuellement les relations entre les tables de modèle.  
  
 Si votre modèle contient des données de plusieurs sources, vous devrez probablement créer les relations manuellement. Par exemple, vous pouvez importer les tables Customers, CustomerDiscounts et Orders d'une source de données relationnelle. Les relations existantes entre ces tables à la source sont automatiquement créées dans le modèle. Vous pouvez ensuite ajouter une autre table d'une autre source, par exemple, vous importez des données de région d'une table Geography dans un classeur Microsoft Excel. Vous pouvez ensuite créer manuellement une relation entre une colonne de la table Customers et une colonne de la table Geography.  
  
 Pour créer manuellement des relations dans un modèle tabulaire, vous pouvez utiliser le générateur de modèles dans la vue de diagramme ou la boîte de dialogue Gérer les relations. La vue de diagramme affiche les tables, avec les relations entre celles-ci, dans un format graphique. Vous pouvez cliquer sur une colonne dans une table et faire glisser le curseur vers une autre table pour créer facilement une relation, dans l'ordre correct, entre les tables. La boîte de dialogue Gérer les relations affiche les relations entre les tables dans un format de table simple. Pour savoir comment créer manuellement des relations, consultez [créer une relation entre deux Tables](../../analysis-services/tabular-models/create-a-relationship-between-two-tables-ssas-tabular.md).  
  
##  <a name="bkmk_dupl_errors"></a> Duplicate values and other errors  
 Si vous choisissez une colonne qui ne peut pas être utilisée dans la relation, un X rouge s'affiche en regard de la colonne. Vous pouvez placer le curseur sur l'icône d'erreur pour afficher un message qui fournit des informations supplémentaires sur le problème. Voici quelques-uns des problèmes qui peuvent rendre impossible la création d'une relation entre les colonnes sélectionnées :  
  
|Problème ou message|Résolution|  
|------------------------|----------------|  
|La relation ne peut pas être créée car les deux colonnes sélectionnées contiennent des valeurs dupliquées.|Au moins, une colonne de la paire que vous sélectionnez doit contenir uniquement des valeurs uniques pour créer une relation valide.<br /><br /> Vous pouvez modifier les colonnes pour supprimer les doublons ou inverser l'ordre des colonnes afin que la colonne qui contient les valeurs uniques soit utilisée comme **colonne de recherche associée**.|  
|La colonne contient une valeur Null ou vide.|Les colonnes de données ne peuvent pas être jointes entre elles sur une valeur Null. Pour chaque ligne, une valeur doit figurer dans chacune des deux colonnes utilisées dans une relation.|  
  
##  <a name="bkmk_related_tasks"></a> Related tasks  
  
|Rubrique| Description|  
|-----------|-----------------|  
|[Créer une relation entre deux tables](../../analysis-services/tabular-models/create-a-relationship-between-two-tables-ssas-tabular.md)|Décrit comment créer manuellement une relation entre deux tables.|  
|[Supprimer des relations](../../analysis-services/tabular-models/delete-relationships-ssas-tabular.md)|Décrit comment supprimer une relation et les conséquences de la suppression des relations.|  
|[Filtres croisés bidirectionnels pour modèles tabulaires dans SQL Server 2016 Analysis Services](../../analysis-services/tabular-models/bi-directional-cross-filters-tabular-models-analysis-services.md)|Décrit le filtrage croisé bidirectionnel pour les tables associées. Un contexte de filtre d’une relation de table peut être utilisé lors de l’interrogation d’une deuxième relation de table si les tables sont associées et si les filtres croisés bidirectionnels sont définis.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables et colonnes](../../analysis-services/tabular-models/tables-and-columns-ssas-tabular.md)   
 [Importer des données](http://msdn.microsoft.com/library/6617b2a2-9f69-433e-89e0-4c5dc92982cf)  
  
  
