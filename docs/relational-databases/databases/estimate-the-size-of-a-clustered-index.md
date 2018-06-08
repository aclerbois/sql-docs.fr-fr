---
title: Estimer la taille d’un index cluster | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: SQL
ms.prod_service: database-engine, sql-database
ms.component: indexes
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- space allocation [SQL Server], index size
- size [SQL Server], tables
- disk space [SQL Server], indexes
- predicting table size [SQL Server]
- table size [SQL Server]
- estimating table size
- space [SQL Server], indexes
- clustered indexes, table size
- nonclustered indexes [SQL Server], table size
- designing databases [SQL Server], estimating size
- calculating table size
ms.assetid: 2b5137f8-98ad-46b5-9aae-4c980259bf8d
caps.latest.revision: 49
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: c10509bbdde9b1fc19184f8429a8d72b06131cf5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="estimate-the-size-of-a-clustered-index"></a>Estimer la taille d’un index cluster

[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  Vous pouvez estimer la quantité d'espace nécessaire au stockage des données d'un index cluster en procédant comme suit :  
  
1.  Calculez l'espace utilisé pour le stockage des données au niveau feuille de l'index cluster.  
  
2.  Calculez l'espace utilisé pour le stockage des informations relatives à l'index cluster.  
  
3.  Faites la somme des valeurs calculées.  
  
## <a name="step-1-calculate-the-space-used-to-store-data-in-the-leaf-level"></a>Étape 1. Calculer l'espace utilisé pour le stockage des données au niveau feuille  
  
1.  Déterminez le nombre de lignes que contiendra la table :  
  
     ***Num_Rows***  = nombre de lignes de la table  
  
2.  Spécifiez le nombre de colonnes de longueur fixe et variable et calculez l'espace nécessaire à leur stockage :  
  
     Calculez l'espace que chacun de ces groupes de colonnes occupe dans la ligne de données. La taille d'une colonne dépend du type des données et de la longueur spécifiée.  
  
     ***Num_Cols***  = nombre total de colonnes (de longueur fixe et variable)  
  
     ***Fixed_Data_Size***  = taille totale en octets de toutes les colonnes de longueur fixe  
  
     ***Num_Variable_Cols***  = nombre de colonnes de longueur variable  
  
     ***Max_Var_Size***  = taille maximale en octets de toutes les colonnes de longueur variable  
  
3.  S'il s'agit d'un index cluster non unique, définissez la colonne *uniqueifier* :  
  
     Cette colonne est une colonne de longueur variable qui accepte les valeurs NULL. Elle présentera une valeur non NULL et une taille de 4 octets dans les lignes qui contiennent des valeurs de clés non uniques. Cette valeur fait partie de la clé d'index et est nécessaire pour garantir que chaque ligne contient une valeur de clé unique.  
  
     ***Num_Cols***  = ***Num_Cols*** + 1  
  
     ***Num_Variable_Cols***  = ***Num_Variable_Cols*** + 1  
  
     ***Max_Var_Size***  = ***Max_Var_Size*** + 4  
  
     Ces modifications supposent que toutes ces valeurs ne seront pas uniques.  
  
4.  Une partie de la ligne, connue sous le nom de bitmap NULL, est réservée pour gérer la possibilité de valeur NULL de la colonne. Calculez sa taille :  
  
     ***Null_Bitmap***  = 2 + ((***Num_Cols*** + 7) / 8)  
  
     Seule la partie entière de l'expression précédente doit être utilisée ; omettez le reste.  
  
5.  Calculez la taille des données de longueur variable :  
  
     En présence de colonnes de longueur variable dans la table, déterminez l'espace utilisé pour stocker les colonnes dans la ligne au moyen de la formule suivante :  
  
     ***Variable_Data_Size***  = 2 + (***Num_Variable_Cols*** x 2) + ***Max_Var_Size***  
  
     Les octets ajoutés à ***Max_Var_Size*** servent à assurer le suivi de chaque colonne variable. Il est supposé, lorsque vous utilisez cette formule, que toutes les colonnes de longueur variable sont entièrement remplies. Si vous pensez qu’un pourcentage inférieur de l’espace de stockage des colonnes de longueur variable sera utilisé, vous pouvez ajuster la valeur de ***Max_Var_Size*** en fonction de ce pourcentage pour obtenir une estimation plus précise de la taille globale de la table.  
  
    > [!NOTE]  
    >  Vous pouvez combiner des colonnes **varchar**, **nvarchar**, **varbinary**ou **sql_variant** qui provoquent le dépassement de la largeur totale de la table définie au-delà de 8 060 octets. La longueur de chacune de ces colonnes doit toujours être inférieure à la limite de 8 000 octets pour une colonne **varchar**, **varbinary**ou **sql_variant** et de 4 000 octets pour les colonnes **nvarchar** . Toutefois, l'association de leurs largeurs peut dépasser la limite de 8 060 octets dans une table.  
  
     En l’absence de toute colonne de longueur variable, attribuez la valeur 0 à ***Variable_Data_Size*** .  
  
6.  Calculez la taille totale de la ligne :  
  
     ***Row_Size***  = ***Fixed_Data_Size*** + ***Variable_Data_Size*** + ***Null_Bitmap*** + 4  
  
     La valeur 4 correspond à l'espace réservé à l'en-tête d'une ligne de données.  
  
7.  Calculez le nombre de lignes par page (8 096 octets disponibles par page) :  
  
     ***Rows_Per_Page***  = 8096 / (***Row_Size*** + 2)  
  
     Comme les lignes ne peuvent pas être fractionnées sur plusieurs pages de données, arrondissez le nombre de lignes par page à la ligne entière inférieure. La valeur 2 dans la formule correspond à l'entrée de la ligne dans le tableau d'emplacements de la page.  
  
8.  Calculez le nombre de lignes libres réservées par page en fonction du taux de remplissage [Fill_Factor](../../relational-databases/indexes/specify-fill-factor-for-an-index.md) spécifié :  
  
     ***Free_Rows_Per_Page***  = 8096 x ((100 - ***Fill_Factor***) / 100) / (***Row_Size*** + 2)  
  
     Le facteur de remplissage utilisé dans le calcul est un nombre et non un pourcentage. Comme les lignes ne peuvent pas être fractionnées sur plusieurs pages de données, arrondissez le nombre de lignes par page à la ligne entière inférieure. Au fur et à mesure que le facteur de remplissage s'accroît, davantage de données seront stockées sur chaque page et il y aura moins de pages. La valeur 2 dans la formule correspond à l'entrée de la ligne dans le tableau d'emplacements de la page.  
  
9. Calculez ensuite le nombre de pages de données requises pour le stockage de toutes les lignes :  
  
     ***Num_Leaf_Pages***  = ***Num_Rows*** / (***Rows_Per_Page*** - ***Free_Rows_Per_Page***)  
  
     Le nombre de pages de données estimé doit être arrondi à la page entière la plus proche.  
  
10. Calculez la quantité d'espace nécessaire pour le stockage des données au niveau feuille (au total, 8 192 octets par page) :  
  
     ***Leaf_space_used***  = 8192 x ***Num_Leaf_Pages***  
  
## <a name="step-2-calculate-the-space-used-to-store-index-information"></a>Étape 2. Calculer l'espace utilisé pour le stockage des informations d'index  
 Vous pouvez estimer la quantité d'espace nécessaire au stockage des niveaux supérieurs de l'index en procédant comme suit :  
  
1.  Spécifiez le nombre de colonnes de longueur fixe et de longueur variable de la clé d'index et calculez l'espace nécessaire à leur stockage :  
  
     Les colonnes clés d'un index peuvent inclure des colonnes de longueur fixe et variable. Pour estimer la taille de la ligne d'index du niveau intérieur, calculez l'espace occupé par chacun de ces groupes de colonnes dans la ligne d'index. La taille d'une colonne dépend du type des données et de la longueur spécifiée.  
  
     ***Num_Key_Cols***  = nombre total de colonnes clés (de longueur fixe et variable)  
  
     ***Fixed_Key_Size***  = taille totale en octets de toutes les colonnes clés de longueur fixe  
  
     ***Num_Variable_Key_Cols***  = nombre de colonnes clés de longueur variable  
  
     ***Max_Var_Key_Size***  = taille maximale en octets de toutes les colonnes clés de longueur variable  
  
2.  Définissez les colonnes uniqueifier nécessaires s'il s'agit d'un index non unique :  
  
     Cette colonne est une colonne de longueur variable qui accepte les valeurs NULL. Elle présentera une valeur non NULL et une taille de 4 octets dans les lignes qui contiennent des valeurs de clés d'index non uniques. Cette valeur fait partie de la clé d'index et est nécessaire pour garantir que chaque ligne contient une valeur de clé unique.  
  
     ***Num_Key_Cols***  = ***Num_Key_Cols*** + 1  
  
     ***Num_Variable_Key_Cols***  = ***Num_Variable_Key_Cols*** + 1  
  
     ***Max_Var_Key_Size***  = ***Max_Var_Key_Size*** + 4  
  
     Ces modifications supposent que toutes ces valeurs ne seront pas uniques.  
  
3.  Calculez la taille de null bitmap :  
  
     En présence de colonnes autorisant des valeurs Null dans la clé d'index, une partie de la ligne d'index est réservée à la bitmap Null. Calculez sa taille :  
  
     ***Index_Null_Bitmap***  = 2 + ((nombre de colonnes dans la ligne d’index + 7) / 8)  
  
     Seule la partie entière de l'expression précédente doit être utilisée. Omettez le reste.  
  
     En l’absence de toute colonne clé autorisant les valeurs Null, attribuez la valeur 0 à ***Index_Null_Bitmap*** .  
  
4.  Calculez la taille des données de longueur variable :  
  
     En présence de colonnes de longueur variable, déterminez l'espace utilisé pour le stockage des colonnes dans la ligne d'index au moyen de la formule suivante :  
  
     ***Variable_Key_Size***  = 2 + (***Num_Variable_Key_Cols*** x 2) + ***Max_Var_Key_Size***  
  
     Les octets ajoutés à ***Max_Var_Key_Size*** servent à assurer le suivi de chaque colonne de longueur variable. Il est supposé, lorsque vous utilisez cette formule, que toutes les colonnes de longueur variable sont entièrement remplies. Si vous pensez qu’un pourcentage inférieur de l’espace de stockage des colonnes de longueur variable sera utilisé, vous pouvez ajuster la valeur de ***Max_Var_Key_Size*** en fonction de ce pourcentage pour obtenir une estimation plus précise de la taille globale de la table.  
  
     En l’absence de toute colonne de longueur variable, attribuez la valeur 0 à ***Variable_Key_Size*** .  
  
5.  Calculez la taille de la ligne d'index :  
  
     ***Index_Row_Size***  = ***Fixed_Key_Size*** + ***Variable_Key_Size*** + ***Index_Null_Bitmap*** + 1 (représentant la surcharge de l’en-tête de la ligne d’index) + 6 (représentant le pointeur de l’ID de la page enfant)  
  
6.  Calculez le nombre de lignes d'index par page (8 096 octets libres par page) :  
  
     ***Index_Rows_Per_Page***  = 8096 / (***Index_Row_Size*** + 2)  
  
     Comme les lignes d'index ne peuvent pas être fractionnées sur plusieurs pages de données, arrondissez le nombre de lignes d'index par page à la ligne entière inférieure. Le chiffre 2 de la formule concerne l'entrée de la ligne du tableau d'emplacement de la page.  
  
7.  Calculez le nombre de niveaux contenus dans l'index :  
  
     ***Non-leaf_Levels***  = 1 + log (Index_Rows_Per_Page) (***Num_Leaf_Pages*** / ***Index_Rows_Per_Page***)  
  
     Arrondissez cette valeur au nombre entier supérieur le plus proche. Cette valeur n'inclut pas le niveau feuille de l'index cluster.  
  
8.  Calculez le nombre de pages non-feuille contenues dans l'index :  
  
     ***Num_Index_Pages =*** ∑Level ***(Num_Leaf_Pages / (Index_Rows_Per_Page***^Level ***))***  
  
     où 1 <= Level <= ***Non-leaf_Levels***  
  
     Arrondissez chaque élément de la somme au nombre entier supérieur le plus proche. À titre d’exemple simple, imaginez un index où ***Num_Leaf_Pages*** = 1000 et ***Index_Rows_Per_Page*** = 25. Le premier niveau d'index au-dessus du niveau feuille stocke 1 000 lignes d'index, ce qui représente une ligne d'index par page feuille et 25 lignes d'index par page. Par conséquent, il faut 40 pages pour stocker ces 1 000 lignes d'index. Le niveau suivant de l'index doit stocker 40 lignes. Cela requiert donc 2 pages. Le niveau final de l'index doit stocker 2 lignes. Cela requiert donc 1 page. Il en résulte 43 pages d'index non-feuille. Lorsque ces nombres sont utilisés dans les formules précédentes, le résultat est le suivant :  
  
     ***Non-leaf_Levels***  = 1 + log(25) (1000 / 25) = 3  
  
     ***Num_Index_Pages*** = 1000/(25^3)+ 1000/(25^2) + 1000/(25^1) = 1 + 2 + 40 = 43, ce qui correspond au nombre de pages décrit dans l’exemple.  
  
9. Calculez la taille de l'index (8 192 octets par page) :  
  
     ***Index_Space_Used***  = 8192 x ***Num_Index_Pages***  
  
## <a name="step-3-total-the-calculated-values"></a>Étape 3. Faire la somme des valeurs calculées  
 Faites la somme des valeurs obtenues à partir des deux étapes précédentes :  
  
 Taille de l’index cluster (octets) = ***Leaf_Space_Used*** + ***Index_Space_used***  
  
 Ce calcul ne tient pas compte des éléments suivants :  
  
-   Partitionnement  
  
     L'espace réservé pour le partitionnement est minime, mais complexe à calculer. Il n'est pas nécessaire de l'inclure.  
  
-   Pages d'allocation  
  
     Au moins une page IAM est utilisée pour assurer le suivi des pages allouées à un segment de mémoire, mais l'espace réservé est minime. De plus, il n'existe aucun algorithme capable de calculer de manière déterministe et exacte le nombre de pages IAM qui seront utilisées.  
  
-   Valeurs LOB  
  
     L’algorithme permettant de déterminer avec exactitude la quantité d’espace qui sera utilisée pour stocker les valeurs des types de données LOB **varchar(max)**, **varbinary(max)**, **nvarchar(max)**, **text**, **ntext**, **xml**et **image** est complexe. Il suffit simplement de faire la somme de la taille moyenne des valeurs LOB attendues, de la multiplier par ***Num_Rows***, puis d’ajouter le résultat à la taille totale de l’index cluster.  
  
-   Compression  
  
     Vous ne pouvez pas précalculer la taille d'un index compressé.  
  
-   Colonnes éparses  
  
     Pour plus d'informations sur l'espace nécessaire pour les colonnes éparses, consultez [Use Sparse Columns](../../relational-databases/tables/use-sparse-columns.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Description des index cluster et non-cluster](../../relational-databases/indexes/clustered-and-nonclustered-indexes-described.md)   
 [Estimer la taille d'une table](../../relational-databases/databases/estimate-the-size-of-a-table.md)   
 [Créer des index cluster](../../relational-databases/indexes/create-clustered-indexes.md)   
 [Créez des index non-cluster](../../relational-databases/indexes/create-nonclustered-indexes.md)   
 [Estimer la taille d'un index non-cluster](../../relational-databases/databases/estimate-the-size-of-a-nonclustered-index.md)   
 [Estimer la taille d'un segment de mémoire](../../relational-databases/databases/estimate-the-size-of-a-heap.md)   
 [Estimer la taille d'une base de données](../../relational-databases/databases/estimate-the-size-of-a-database.md)  
  
  
