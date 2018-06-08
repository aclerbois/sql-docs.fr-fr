---
title: Estimer la taille d’un index non-cluster | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
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
- predicting table size [SQL Server]
- table size [SQL Server]
- estimating table size
- clustered indexes, table size
- designing databases [SQL Server], estimating size
- calculating table size
ms.assetid: c183b0e4-ef4c-4bfc-8575-5ac219c25b0a
caps.latest.revision: 40
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: a73e9c741ef92adc7f43a1edbc1be52d54be450f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="estimate-the-size-of-a-nonclustered-index"></a>Estimer la taille d'un index non-cluster

[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  Utilisez la procédure suivante pour estimer la quantité d'espace nécessaire au stockage d'un index non cluster :  
  
1.  Calculez les variables à utiliser dans les étapes 2 et 3.  
  
2.  Calculez l'espace utilisé pour stocker les informations d'index au niveau feuille de l'index non cluster.  
  
3.  Calculez l'espace utilisé pour stocker les informations d'index aux niveaux non-feuille de l'index non cluster.  
  
4.  Faites la somme des valeurs calculées.  
  
## <a name="step-1-calculate-variables-for-use-in-steps-2-and-3"></a>Étape 1. Calculer les variables à utiliser dans les étapes 2 et 3  
 Vous pouvez utiliser la procédure suivante afin de calculer les variables utilisées pour estimer la quantité d'espace nécessaire au stockage des niveaux supérieurs de l'index.  
  
1.  Déterminez le nombre de lignes que contiendra la table :  
  
     ***Num_Rows***  = nombre de lignes dans la table  
  
2.  Spécifiez le nombre de colonnes de longueur fixe et de longueur variable de la clé d'index et calculez l'espace nécessaire à leur stockage :  
  
     Les colonnes clés d'un index peuvent inclure des colonnes de longueur fixe et variable. Pour estimer la taille de la ligne d'index du niveau intérieur, calculez l'espace occupé par chacun de ces groupes de colonnes dans la ligne d'index. La taille d'une colonne dépend du type des données et de la longueur spécifiée.  
  
     ***Num_Key_Cols***  = nombre total de colonnes clés (de longueur fixe et variable)  
  
     ***Fixed_Key_Size***  = taille totale en octets de toutes les colonnes clés de longueur fixe  
  
     ***Num_Variable_Key_Cols***  = nombre de colonnes clés de longueur variable  
  
     ***Max_Var_Key_Size***  = taille maximale en octets des colonnes clés de longueur variable  
  
3.  Tenez compte du localisateur de ligne de données nécessaire si l'index n'est pas unique :  
  
     Si l'index non-cluster n'est pas unique, le localisateur de ligne de données est associé à la clé d'index non-cluster pour produire une valeur de clé unique pour chaque ligne.  
  
     Si l'index non-cluster est au-dessus d'un segment, le localisateur de ligne de données est le RID de ce segment. Sa taille est de 8 octets.  
  
     ***Num_Key_Cols***  = ***Num_Key_Cols*** + 1  
  
     ***Num_Variable_Key_Cols***  = ***Num_Variable_Key_Cols*** + 1  
  
     ***Max_Var_Key_Size***  = ***Max_Var_Key_Size*** + 8  
  
     Si l'index non cluster est au-dessus d'un index cluster, le localisateur de lignes de données est la clé de clustering. Les colonnes qui doivent être associées à la clé d'index non cluster sont les colonnes de la clé de clustering qui ne sont pas déjà présentes dans l'ensemble des colonnes clés de l'index non cluster.  
  
     ***Num_Key_Cols***  = ***Num_Key_Cols*** + nombre de colonnes clés de clustering non présentes dans l’ensemble de colonnes clés de l’index non cluster (+ 1 si l’index cluster n’est pas unique)  
  
     ***Fixed_Key_Size***  = ***Fixed_Key_Size*** + taille totale en octets des colonnes clés de clustering de longueur fixe non présentes dans l’ensemble de colonnes clés de l’index non cluster  
  
     ***Num_Variable_Key_Cols***  = ***Num_Variable_Key_Cols*** + nombre de colonnes clés de clustering de longueur variable non présentes dans l’ensemble de colonnes clés de l’index non cluster (+ 1 si l’index cluster n’est pas unique)  
  
     ***Max_Var_Key_Size***  = ***Max_Var_Key_Size*** + taille maximale en octets des colonnes clés de clustering de longueur variable non présentes dans l’ensemble de colonnes clés de l’index non cluster (+ 4 si l’index cluster n’est pas unique)  
  
4.  Une partie de la ligne, nommée null bitmap, peut être réservée à la gestion de l'acceptation de valeurs NULL dans les colonnes. Calculez sa taille :  
  
     En présence de colonnes autorisant des valeurs Null dans la clé d'index, notamment toutes les colonnes clés de clustering nécessaires comme décrit dans l'étape 1.3, une partie de la ligne d'index est réservée à la bitmap Null.  
  
     ***Index_Null_Bitmap***  = 2 + ((nombre de colonnes dans la ligne d’index + 7) / 8)  
  
     Seule la partie entière de l'expression précédente doit être utilisée. Omettez le reste.  
  
     En l’absence de colonnes clés pouvant être NULL, attribuez à ***Index_Null_Bitmap*** la valeur 0.  
  
5.  Calculez la taille des données de longueur variable :  
  
     En présence de colonnes de longueur variable dans la clé d'index, notamment les colonnes clés d'index cluster nécessaires, déterminez l'espace utilisé pour le stockage des colonnes dans la ligne d'index :  
  
     ***Variable_Key_Size***  = 2 + (***Num_Variable_Key_Cols*** x 2) + ***Max_Var_Key_Size***  
  
     Les octets ajoutés à ***Max_Var_Key_Size*** servent à assurer le suivi de chaque colonne variable. Cette formule suppose que toutes les colonnes de longueur variable sont entièrement remplies. Si vous pensez qu’un pourcentage inférieur de l’espace de stockage des colonnes de longueur variable sera utilisé, vous pouvez ajuster la valeur de ***Max_Var_Key_Size*** en fonction de ce pourcentage pour obtenir une estimation plus précise de la taille globale de la table.  
  
     En l’absence de toute colonne de longueur variable, attribuez la valeur 0 à ***Variable_Key_Size*** .  
  
6.  Calculez la taille de la ligne d'index :  
  
     ***Index_Row_Size***  = ***Fixed_Key_Size*** + ***Variable_Key_Size*** + ***Index_Null_Bitmap*** + 1 (représentant la surcharge de l’en-tête de la ligne d’index) + 6 (représentant le pointeur de l’ID de la page enfant)  
  
7.  Calculez le nombre de lignes d'index par page (8 096 octets libres par page) :  
  
     ***Index_Rows_Per_Page***  = 8096 / (***Index_Row_Size*** + 2)  
  
     Comme les lignes d'index ne peuvent pas être fractionnées sur plusieurs pages de données, arrondissez le nombre de lignes d'index par page à la ligne entière inférieure. Le chiffre 2 de la formule concerne l'entrée de la ligne du tableau d'emplacement de la page.  
  
## <a name="step-2-calculate-the-space-used-to-store-index-information-in-the-leaf-level"></a>Étape 2. Calculer l'espace utilisé pour le stockage des informations d'index au niveau feuille  
 Vous pouvez utiliser la procédure suivante pour estimer la quantité d'espace nécessaire au stockage du niveau feuille de l'index. Vous aurez besoin des valeurs conservées dans l'étape 1 pour effectuer cette étape-ci.  
  
1.  Spécifiez le nombre de colonnes de longueur fixe et de longueur variable au niveau feuille et calculez l'espace nécessaire à leur stockage :  
  
    > [!NOTE]  
    >  Vous pouvez étendre un index non cluster en incluant les colonnes sans clé en plus des colonnes de clés d'index. Ces colonnes supplémentaires sont stockées uniquement au niveau feuille de l'index non-cluster. Pour plus d’informations, consultez [Créer des index avec colonnes incluses](../../relational-databases/indexes/create-indexes-with-included-columns.md).  
  
    > [!NOTE]  
    >  Vous pouvez combiner des colonnes **varchar**, **nvarchar**, **varbinary**ou **sql_variant** qui provoquent le dépassement de la largeur totale de la table définie au-delà de 8 060 octets. La longueur de chacune de ces colonnes doit toujours être inférieure à la limite de 8 000 octets pour une colonne **varchar**, **varbinary**ou **sql_variant** et de 4 000 octets pour les colonnes **nvarchar** . Toutefois, l'association de leurs largeurs peut dépasser la limite de 8 060 octets dans une table. Ceci s'applique également aux lignes de feuille d'index non-cluster possédant des colonnes incluses.  
  
     Si l'index non-cluster ne possède aucune colonne incluse, utilisez les valeurs de l'étape 1, y compris les éventuelles modifications déterminées dans l'étape 1.3 :  
  
     ***Num_Leaf_Cols***  = ***Num_Key_Cols***  
  
     ***Fixed_Leaf_Size***  = ***Fixed_Key_Size***  
  
     ***Num_Variable_Leaf_Cols***  = ***Num_Variable_Key_Cols***  
  
     ***Max_Var_Leaf_Size***  = ***Max_Var_Key_Size***  
  
     Si l'index non-cluster ne possède aucune colonne incluse, ajoutez les valeurs appropriées aux valeurs de l'étape 1, y compris les éventuelles modifications de l'étape 1.3. La taille d'une colonne dépend du type des données et de la longueur spécifiée. Pour plus d’informations, consultez [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).  
  
     ***Num_Leaf_Cols***  = ***Num_Key_Cols*** + nombre de colonnes incluses  
  
     ***Fixed_Leaf_Size***  = ***Fixed_Key_Size*** + taille totale en octets des colonnes incluses de longueur fixe  
  
     ***Num_Variable_Leaf_Cols***  = ***Num_Variable_Key_Cols*** + nombre de colonnes incluses de longueur variable  
  
     ***Max_Var_Leaf_Size***  = ***Max_Var_Key_Size*** + taille maximale en octets des colonnes incluses de longueur variable  
  
2.  Tenez compte du localisateur de ligne de données :  
  
     Si l'index non-cluster n'est pas unique, la surcharge du localisateur de ligne de données a déjà été prise en compte dans l'étape 1.3 et aucune modification supplémentaire n'est nécessaire. Passez à l'étape suivante.  
  
     Si l'index non-cluster est unique, le localisateur de ligne des données doit être pris en compte dans toutes les lignes au niveau feuille.  
  
     Si l'index non-cluster est au-dessus d'un segment, le localisateur de ligne de données est le RID du segment (taille 8 octets).  
  
     ***Num_Leaf_Cols***  = ***Num_Leaf_Cols*** + 1  
  
     ***Num_Variable_Leaf_Cols***  = ***Num_Variable_Leaf_Cols*** + 1  
  
     ***Max_Var_Leaf_Size***  = ***Max_Var_Leaf_Size*** + 8  
  
     Si l'index non cluster est au-dessus d'un index cluster, le localisateur de lignes de données est la clé de clustering. Les colonnes qui doivent être associées à la clé d'index non cluster sont les colonnes de la clé de clustering qui ne sont pas déjà présentes dans l'ensemble des colonnes clés de l'index non cluster.  
  
     ***Num_Leaf_Cols***  = ***Num_Leaf_Cols*** + nombre de colonnes clés de clustering non présentes dans l’ensemble de colonnes clés de l’index non cluster (+ 1 si l’index cluster n’est pas unique)  
  
     ***Fixed_Leaf_Size***  = ***Fixed_Leaf_Size*** + nombre de colonnes clés de clustering de longueur fixe non présentes dans l’ensemble de colonnes clés de l’index non cluster  
  
     ***Num_Variable_Leaf_Cols***  = ***Num_Variable_Leaf_Cols*** + nombre de colonnes clés de clustering de longueur variable non présentes dans l’ensemble de colonnes clés de l’index non cluster (+ 1 si l’index cluster n’est pas unique)  
  
     ***Max_Var_Leaf_Size***  = ***Max_Var_Leaf_Size*** + taille en octets des colonnes clés de clustering de longueur variable non présentes dans l’ensemble de colonnes clés de l’index non cluster (+ 4 si l’index cluster n’est pas unique)  
  
3.  Calculez la taille de null bitmap :  
  
     ***Leaf_Null_Bitmap***  = 2 + ((***Num_Leaf_Cols*** + 7) / 8)  
  
     Seule la partie entière de l'expression précédente doit être utilisée. Omettez le reste.  
  
4.  Calculez la taille des données de longueur variable :  
  
     En présence de colonnes de longueur variable dans la clé d'index, notamment les colonnes clés de cluster nécessaires comme décrit précédemment dans l'étape 2.2, déterminez l'espace utilisé pour le stockage des colonnes dans la ligne d'index :  
  
     ***Variable_Leaf_Size***  = 2 + (***Num_Variable_Leaf_Cols*** x 2) + ***Max_Var_Leaf_Size***  
  
     Les octets ajoutés à ***Max_Var_Key_Size*** servent à assurer le suivi de chaque colonne variable. Cette formule suppose que toutes les colonnes de longueur variable sont entièrement remplies. Si vous estimez qu’un pourcentage inférieur de l’espace de stockage des colonnes de longueur variable sera utilisé, vous pouvez multiplier la valeur ***Max_Var_Leaf_Size*** par ce pourcentage pour obtenir une estimation plus précise de la taille globale de la table.  
  
     En l’absence de colonnes de longueur variable, attribuez à ***Variable_Leaf_Size*** la valeur 0.  
  
5.  Calculez la taille de la ligne d'index :  
  
     ***Leaf_Row_Size***  = ***Fixed_Leaf_Size*** + ***Variable_Leaf_Size*** + ***Leaf_Null_Bitmap*** + 1 (représentant la surcharge de l’en-tête de la ligne d’index)  
  
6.  Calculez le nombre de lignes d'index par page (8 096 octets libres par page) :  
  
     ***Leaf_Rows_Per_Page***  = 8096 / (***Leaf_Row_Size*** + 2)  
  
     Comme les lignes d'index ne peuvent pas être fractionnées sur plusieurs pages de données, arrondissez le nombre de lignes d'index par page à la ligne entière inférieure. Le chiffre 2 de la formule concerne l'entrée de la ligne du tableau d'emplacement de la page.  
  
7.  Calculez le nombre de lignes libres réservées par page en fonction du taux de remplissage [Fill_Factor](../../relational-databases/indexes/specify-fill-factor-for-an-index.md) spécifié :  
  
     ***Free_Rows_Per_Page***  = 8096 x ((100 - ***Fill_Factor***) / 100) / (***Leaf_Row_Size*** + 2)  
  
     Le facteur de remplissage utilisé dans le calcul est un nombre et non un pourcentage. Comme les lignes ne peuvent pas être fractionnées sur plusieurs pages de données, arrondissez le nombre de lignes par page à la ligne entière inférieure. Au fur et à mesure que le facteur de remplissage s'accroît, davantage de données seront stockées sur chaque page et il y aura moins de pages. Le chiffre 2 de la formule concerne l'entrée de la ligne du tableau d'emplacement de la page.  
  
8.  Calculez ensuite le nombre de pages de données requises pour le stockage de toutes les lignes :  
  
     ***Num_Leaf_Pages***  = ***Num_Rows*** / (***Leaf_Rows_Per_Page*** - ***Free_Rows_Per_Page***)  
  
     Le nombre de pages de données estimé doit être arrondi à la page entière la plus proche.  
  
9. Calculez la taille de l'index (8 192 octets par page) :  
  
     ***Leaf_Space_Used***  = 8192 x ***Num_Leaf_Pages***  
  
## <a name="step-3-calculate-the-space-used-to-store-index-information-in-the-non-leaf-levels"></a>Étape 3. Calculer l'espace utilisé pour le stockage des informations d'index aux niveaux non-feuille  
 Utilisez la procédure suivante pour estimer la quantité d'espace nécessaire au stockage des niveaux intermédiaires et racine de l'index. Vous aurez besoin des valeurs conservées aux étapes 2 et 3 pour effectuer cette étape-ci.  
  
1.  Calculez le nombre de niveaux non-feuille contenus dans l'index :  
  
     ***Non-leaf Levels***  = 1 + log( Index_Rows_Per_Page) (***Num_Leaf_Pages*** / ***Index_Rows_Per_Page***)  
  
     Arrondissez cette valeur au nombre entier supérieur le plus proche. Cette valeur n'inclut pas le niveau feuille de l'index non cluster.  
  
2.  Calculez le nombre de pages non-feuille contenues dans l'index :  
  
     ***Num_Index_Pages***  = ∑Level (***Num_Leaf_Pages/Index_Rows_Per_Page***^Level)where 1 <= Level <= ***Levels***  
  
     Arrondissez chaque élément de la somme au nombre entier supérieur le plus proche. À titre d’exemple simple, imaginez un index où ***Num_Leaf_Pages*** = 1000 et ***Index_Rows_Per_Page*** = 25. Le premier niveau d'index au-dessus du niveau feuille stocke 1 000 lignes d'index, ce qui représente une ligne d'index par page feuille et 25 lignes d'index par page. Par conséquent, il faut 40 pages pour stocker ces 1 000 lignes d'index. Le niveau suivant de l'index doit stocker 40 lignes. Cela requiert donc 2 pages. Le niveau final de l'index doit stocker 2 lignes. Cela requiert donc 1 page. Il en résulte 43 pages d'index non-feuille. Lorsque ces nombres sont utilisés dans les formules précédentes, le résultat est le suivant :  
  
     ***Non-leaf_Levels***  = 1 + log(25) (1000 / 25) = 3  
  
     ***Num_Index_Pages*** = 1000/(25^3)+ 1000/(25^2) + 1000/(25^1) = 1 + 2 + 40 = 43, ce qui correspond au nombre de pages décrit dans l’exemple.  
  
3.  Calculez la taille de l'index (8 192 octets par page) :  
  
     ***Index_Space_Used***  = 8192 x ***Num_Index_Pages***  
  
## <a name="step-4-total-the-calculated-values"></a>Étape 4. Faire la somme des valeurs calculées  
 Faites la somme des valeurs obtenues à partir des deux étapes précédentes :  
  
 Taille de l’index non cluster (octets) = ***Leaf_Space_Used*** + ***Index_Space_used***  
  
 Ce calcul ne tient pas compte des éléments suivants :  
  
-   Partitionnement  
  
     L'espace réservé pour le partitionnement est minime, mais complexe à calculer. Il n'est pas nécessaire de l'inclure.  
  
-   Pages d'allocation  
  
     Au moins une page IAM est utilisée pour assurer le suivi des pages allouées à un segment de mémoire, mais l'espace réservé est minime. De plus, il n'existe aucun algorithme capable de calculer de manière déterministe et exacte le nombre de pages IAM qui seront utilisées.  
  
-   Valeurs LOB  
  
     L’algorithme permettant de déterminer avec exactitude la quantité d’espace qui sera utilisée pour stocker les valeurs des types de données LOB **varchar(max)**, **varbinary(max)**, **nvarchar(max)**, **text**, **ntext**, **xml**et **image** est complexe. Il suffit d’ajouter simplement la taille moyenne des valeurs LOB attendues, de la multiplier par ***Num_Rows***et d’ajouter ce produit à la taille totale de l’index non cluster.  
  
-   Compression  
  
     Vous ne pouvez pas précalculer la taille d'un index compressé.  
  
-   Colonnes éparses  
  
     Pour plus d'informations sur l'espace nécessaire pour les colonnes éparses, consultez [Use Sparse Columns](../../relational-databases/tables/use-sparse-columns.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Description des index cluster et non-cluster](../../relational-databases/indexes/clustered-and-nonclustered-indexes-described.md)   
 [Créez des index non-cluster](../../relational-databases/indexes/create-nonclustered-indexes.md)   
 [Créer des index cluster](../../relational-databases/indexes/create-clustered-indexes.md)   
 [Estimer la taille d'une table](../../relational-databases/databases/estimate-the-size-of-a-table.md)   
 [Estimer la taille d'un index cluster](../../relational-databases/databases/estimate-the-size-of-a-clustered-index.md)   
 [Estimer la taille d'un segment de mémoire](../../relational-databases/databases/estimate-the-size-of-a-heap.md)   
 [Estimer la taille d'une base de données](../../relational-databases/databases/estimate-the-size-of-a-database.md)  
  
  
