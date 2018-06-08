---
title: Estimer la taille d’un segment de mémoire | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: databases
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- disk space [SQL Server], indexes
- estimating heap size
- size [SQL Server], heap
- space [SQL Server], indexes
- heaps
ms.assetid: 81fd5ec9-ce0f-4c2c-8ba0-6c483cea6c75
caps.latest.revision: 28
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: f07c117f34944526381e7695b59ac4d2ed2d4cf1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="estimate-the-size-of-a-heap"></a>Estimer la taille d’un segment de mémoire
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  La procédure suivante vous permet d'obtenir une estimation de la quantité d'espace nécessaire au stockage de données dans un segment de mémoire.  
  
1.  Déterminez le nombre de lignes que contiendra la table :  
  
     ***Num_Rows***  = nombre de lignes de la table  
  
2.  Spécifiez le nombre de colonnes de longueur fixe et variable et calculez l'espace nécessaire à leur stockage :  
  
     Calculez l'espace que chacun de ces groupes de colonnes occupe dans la ligne de données. La taille d'une colonne dépend du type des données et de la longueur spécifiée.  
  
     ***Num_Cols***  = nombre total de colonnes (de longueur fixe et variable)  
  
     ***Fixed_Data_Size***  = taille totale en octets de toutes les colonnes de longueur fixe  
  
     ***Num_Variable_Cols***  = nombre de colonnes de longueur variable  
  
     ***Max_Var_Size***  = taille maximale en octets de toutes les colonnes de longueur variable  
  
3.  Une partie de la ligne, connue sous le nom de bitmap NULL, est réservée pour gérer la possibilité de valeur NULL de la colonne. Calculez sa taille :  
  
     ***Null_Bitmap***  = 2 + ((***Num_Cols*** + 7) / 8)  
  
     Seule la partie entière de l'expression doit être utilisée. Omettez le reste.  
  
4.  Calculez la taille des données de longueur variable :  
  
     En présence de colonnes de longueur variable dans la table, déterminez l'espace utilisé pour stocker les colonnes dans la ligne au moyen de la formule suivante :  
  
     ***Variable_Data_Size***  = 2 + (***Num_Variable_Cols*** x 2) + ***Max_Var_Size***  
  
     Les octets ajoutés à ***Max_Var_Size*** servent à assurer le suivi de chaque colonne de longueur variable. Il est supposé, lorsque vous utilisez cette formule, que toutes les colonnes de longueur variable sont entièrement remplies. Si vous pensez qu’un pourcentage inférieur de l’espace de stockage des colonnes de longueur variable sera utilisé, vous pouvez ajuster la valeur de ***Max_Var_Size*** en fonction de ce pourcentage pour obtenir une estimation plus précise de la taille globale de la table.  
  
    > [!NOTE]  
    >  Vous pouvez combiner des colonnes **varchar**, **nvarchar**, **varbinary**ou **sql_variant** qui provoquent le dépassement de la largeur totale de la table définie au-delà de 8 060 octets. La longueur de chacune de ces colonnes doit toujours être inférieure à la limite de 8 000 octets pour une colonne **varchar**, **nvarchar,****varbinary**ou **sql_variant** . Toutefois, l'association de leurs largeurs peut dépasser la limite de 8 060 octets dans une table.  
  
     En l’absence de toute colonne de longueur variable, attribuez la valeur 0 à ***Variable_Data_Size*** .  
  
5.  Calculez la taille totale de la ligne :  
  
     ***Row_Size***  = ***Fixed_Data_Size*** + ***Variable_Data_Size*** + ***Null_Bitmap*** + 4  
  
     La valeur 4 dans la formule correspond à l'espace réservé à l'en-tête de la ligne de données.  
  
6.  Calculez le nombre de lignes par page (8 096 octets disponibles par page) :  
  
     ***Rows_Per_Page***  = 8096 / (***Row_Size*** + 2)  
  
     Comme les lignes ne peuvent pas être fractionnées sur plusieurs pages de données, arrondissez le nombre de lignes par page à la ligne entière inférieure. La valeur 2 dans la formule correspond à l'entrée de la ligne dans le tableau d'emplacements de la page.  
  
7.  Calculez ensuite le nombre de pages de données requises pour le stockage de toutes les lignes :  
  
     ***Num_Pages***  = ***Num_Rows*** / ***Rows_Per_Page***  
  
     Le nombre de pages de données estimé doit être arrondi à la page entière la plus proche.  
  
8.  Calculez la quantité d'espace nécessaire pour le stockage des données dans le segment de mémoire (8 192 octets par page) :  
  
     Taille du segment de mémoire (octets) = 8192 x ***Num_Pages***  
  
 Ce calcul ne tient pas compte des éléments suivants :  
  
-   Partitionnement  
  
     L'espace réservé pour le partitionnement est minime, mais complexe à calculer. Il n'est pas nécessaire de l'inclure.  
  
-   Pages d'allocation  
  
     Au moins une page IAM est utilisée pour assurer le suivi des pages allouées à un segment de mémoire, mais l'espace réservé est minime. De plus, il n'existe aucun algorithme capable de calculer de manière déterministe et exacte le nombre de pages IAM qui seront utilisées.  
  
-   Valeurs LOB  
  
     L’algorithme permettant de déterminer avec exactitude la quantité d’espace qui sera utilisée pour stocker les valeurs des types de données LOB **varchar(max)**, **varbinary(max)**, **nvarchar(max)**, **text**, **ntextxml**et **image** est complexe. Vous pouvez simplement ajouter la taille moyenne des valeurs LOB attendues à la taille totale du segment de mémoire.  
  
-   Compression  
  
     Vous ne pouvez pas précalculer la taille d'un segment de mémoire compressé.  
  
-   Colonnes éparses  
  
     Pour plus d'informations sur l'espace nécessaire pour les colonnes éparses, consultez [Use Sparse Columns](../../relational-databases/tables/use-sparse-columns.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Segments &#40;tables sans index cluster&#41;](../../relational-databases/indexes/heaps-tables-without-clustered-indexes.md)   
 [Description des index cluster et non-cluster](../../relational-databases/indexes/clustered-and-nonclustered-indexes-described.md)   
 [Créer des index cluster](../../relational-databases/indexes/create-clustered-indexes.md)   
 [Créez des index non-cluster](../../relational-databases/indexes/create-nonclustered-indexes.md)   
 [Estimer la taille d'une table](../../relational-databases/databases/estimate-the-size-of-a-table.md)   
 [Estimer la taille d'un index cluster](../../relational-databases/databases/estimate-the-size-of-a-clustered-index.md)   
 [Estimer la taille d'un index non-cluster](../../relational-databases/databases/estimate-the-size-of-a-nonclustered-index.md)   
 [Estimer la taille d'une base de données](../../relational-databases/databases/estimate-the-size-of-a-database.md)  
  
  
